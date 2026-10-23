# DR26-08-10-MACANTON-01-1154 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-10-MACANTON-01-1154
rail: claudeai
model: Fable 5 (claude.ai)
mode: smart model + web_search_v0 (НЕ вендорский Deep Research)
door: browser-chrome-mcp POST /completion
chat_url: https://приватный чат
collected: 2026-08-11T14:21:16-07:00

chars: 24907
---

# Perplexity as a Seventh Deep-Research Rail: Integration Feasibility Report

*Research date: 11 August 2026. All prices and model names verified against docs.perplexity.ai on this date unless otherwise stated.*

---

## A. Executive Summary

**Build one surface only: the HTTP API, as a fully headless rail — and treat it as a new budget line, because no subscription feeds it.** Perplexity in 2026 is unusually API-friendly for unattended operation: its `advanced-deep-research` preset on the new Agent API runs the same multi-step engine that powers the consumer Deep Research product, there is an async job endpoint, an official MCP server, and (since July 2026) an official CLI — but the CLI only does raw search, the MCP server bills the same API meter and adds nothing for cron use, and headless browser automation of perplexity.ai is contractually prohibited, technically hostile, and sits inside an active CFAA litigation zone. The two caveats that should temper enthusiasm: (1) your "burn already-paid quota" rule fails here — the historical $5/month API credit for Pro subscribers appears to have been quietly discontinued in early 2026, with developers reporting sudden 401 errors, and even if present it covers ~12 deep-research calls; and (2) the only *independent* benchmarks (2025) place Perplexity Deep Research clearly below ChatGPT DR and Gemini DR on depth/coverage, so at ~$60–160/month for your workload it is a freshness-and-citations rail, not a depth rail. A defensible cheaper variant: run it as a 1-report/day tie-breaker rail (~$12–30/month).

## B. Key Findings

1. **[established] The API now reaches the real deep-research engine.** Sonar Chat Completions has been renamed/superseded by the Agent API, and Deep Research 2.0 via the advanced-deep-research preset is explicitly the same engine as the consumer product, performing dozens of searches and reading hundreds of documents per query. The old `sonar-deep-research` model remains available and priced on the legacy Sonar API.

2. **[established] An official CLI exists but is not a research rail.** `pplx`, released ~July 27, 2026, is an official single-binary client for the Search API only — no chat, no model selection, no synthesized answers; it exposes exactly `pplx search web` and `pplx content fetch`, with a strict contract of exit code 0 plus one JSON object on stdout, and errors as exit 1 with a JSON error object on stderr. That contract is exactly your `codex`/`grok -p` gold standard — but for search primitives, not reports.

3. **[emerging → contested] Subscription→API credit is unreliable.** Sources actively disagree in 2026: an August 2026 guide states Pro still includes $5/month of API credits which don't roll over, while a May 2026 FinOps analysis reports the credit was quietly removed in early 2026 without notice, breaking Raycast/terminal workflows with 401s, and a July 2026 review says 2026 reporting conflicts and advises confirming at signup rather than counting on it. Decision-safe conclusion: **budget as if the API is a strictly separate meter.**

4. **[established] Unattended-safe primitives exist**: an async endpoint (`POST /v1/async/sonar`, rate-limited to 5 requests/min at entry tier, with high polling limits on the GET endpoints) removes the need to hold a 5–30-minute HTTP connection open from a cron job.

5. **[established] Independent quality data is mixed and mostly pre-dates Deep Research 2.0.** ResearcherBench (July 2025) scored Perplexity Deep Research 0.48 on rubric coverage vs 0.70 for OpenAI DR and 0.69 for Gemini DR — but 0.85 faithfulness (second-best) and 0.56 groundedness (near-best). Perplexity's own DRACO benchmark (Feb 2026) claims the opposite ordering — treat as vendor-favorable.

6. **[established] Headless browser automation is a dead end.** Perplexity's ToS prohibits using automation software (bots), hacks, or unauthorized third-party tooling against the Services, and a February 2026 terms update explicitly prohibits bots and scrapers and restricts consumer plans to personal, non-commercial use.

7. **[established] Interface churn is Perplexity's defining API risk.** Three breaking generational changes in ~18 months: `llama-3.1-sonar-*` → `sonar` family (early 2025), Sonar Chat Completions → Agent API (2026), and the official MCP server dropping the legacy sonar-pro / sonar-reasoning-pro / sonar-deep-research tools in favor of Agent API presets — with old parameters now silently ignored if sent. Silently-ignored parameters are precisely the kind of degradation your fleet must detect mechanically.

---

## C. The Four Surfaces

### C1. HTTP API — ✅ the production-viable rail

**Exists officially.** Two generations coexist today:

- **Legacy Sonar API** (`https://api.perplexity.ai/chat/completions`): `sonar`, `sonar-pro`, `sonar-reasoning-pro`, `sonar-deep-research`. Carries a deprecation banner ("Sonar Chat Completions is now Agent API," with a published migration guide) but still priced and served as of Aug 11, 2026.
- **Agent API** (`POST /v1/agent`, with `/v1/responses` accepted as an OpenAI-SDK-compatible alias, so you can point the OpenAI SDK at base_url `https://api.perplexity.ai/v1`). Presets include fast-search, pro-search, deep-research, and advanced-deep-research, with multi-provider models (OpenAI, Anthropic, Google, xAI, Sonar) at direct provider pricing. Note: the pricing page's calculator also shows preset IDs `fast/low/medium/high/xhigh` — the preset naming appears to be in flux between docs pages; verify `GET`-able preset list before hardcoding. [single-source discrepancy, both official pages]

**Auth:** API key from the API Console at perplexity.ai (prepaid credits, pay-as-you-go, no subscription required). Export as `PERPLEXITY_API_KEY`.

**Minimal working invocation (Agent API, deep research):**

```bash
export PERPLEXITY_API_KEY="pplx-..."   # from API Console → API Keys

curl https://api.perplexity.ai/v1/agent \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "preset": "deep-research",
    "input": "PROMPT_ID_047: <your standard fan-out prompt>"
  }' > report.json
```

**Cron-grade variant (async, legacy Sonar deep research):**

```bash
# submit
REQ=$(curl -s https://api.perplexity.ai/v1/async/sonar \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"sonar-deep-research",
       "messages":[{"role":"user","content":"..."}]}' | jq -r .id)
# poll (cheap: GET is rate-limited at thousands/min), then write to vault
curl -s "https://api.perplexity.ai/v1/async/sonar/$REQ" \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY"
```

**Capability:** advanced-deep-research is the consumer engine; `sonar-deep-research` is the older (March 2025) research model. Both return usage metadata (search query counts, reasoning/citation tokens; completed Agent API responses report a calculated `usage.cost.total_cost`) — this is your degradation sensor (§F).

**Unattended survivability:** High. Key-based auth, no sessions, no captchas; 429s return Retry-After and are not billed, and usage tiers rise permanently with cumulative spend. Set client timeouts to 300 s+ for sync deep research (practitioner guidance: 180–300 s for sonar-deep-research since runs take minutes), or use async and avoid the problem.

### C2. MCP server — ✅ real, but wrong tool for cron

**Official:** `perplexityai/modelcontextprotocol` on GitHub, npm package `@perplexity-ai/mcp-server`. Current tools wrap the Search API plus Agent API presets — a fast conversational tool and a deep research tool on the high preset; deep runs can take minutes and the server streams progress; it also supports Docker and an HTTP mode on :8080/mcp, and exports a server factory for embedding. Transport: stdio (via `npx`) and streamable HTTP.

**Claude Code setup (exact):**

```bash
claude mcp add perplexity \
  --env PERPLEXITY_API_KEY="pplx-..." \
  -- npx -y @perplexity-ai/mcp-server
# or: /plugin marketplace add perplexityai/modelcontextprotocol → /plugin install perplexity
```

The README's own Claude Desktop config sets PERPLEXITY_TIMEOUT_MS to 600000 — a tacit admission that 10-minute tool calls are normal, which is exactly what times out under default MCP client settings.

**Billing:** the API meter, full stop — it requires `PERPLEXITY_API_KEY`; there is no subscription-backed MCP path.

**Failure modes for scheduled agents:** long-lived streaming tool calls hitting client-side timeouts (hence the 600 s env var); schema churn (strip_thinking / reasoning_effort now ignored silently rather than erroring); npx-spawned stdio children orphaned when the host agent is killed by a watchdog (generic MCP hazard, not Perplexity-specific — [no source found], operational inference); and a reported Agent API bug where Gemini-backed deep-research presets returned parsed system instructions instead of user-targeted output — a live example of silent quality failure. **Verdict: use MCP only in interactive Claude Code sessions; for nightly runs, call the HTTP API directly — same meter, fewer moving parts.**

### C3. CLI — ✅ official, ❌ not a research rail

Official `pplx` (repo: perplexityai/perplexity-cli, released ~July 27, 2026) covers only the Search API: `pplx search web` and `pplx content fetch`, JSON-only output, and — critically for you — `pplx auth login` is TTY-only, so agents and CI must export PERPLEXITY_API_KEY; Search API billing is $5.00 per 1,000 requests at 50 QPS on every tier. It supports domain filters, date filters, multi-phrasing of one query, and an --output-dir mode that writes full results to disk with a saved_to path on stdout. It is a superb *search primitive* for your agents (grounded hits at half a cent each) but produces no report. Third-party CLIs that do hit deep-research models exist (e.g., dawid-szewc/perplexity-cli, nileshr/pplx-cli, noQuli/perplexity-cli — all API-key wrappers), but they're thin wrappers you could replace with 30 lines of Python; adoption signals are modest and maintenance is hobbyist-grade, so if you want a `grok -p`-style binary for reports, **write your own wrapper around the async endpoint** rather than adopting a third-party CLI. [low-authority: individual repos]

### C4. Headless browser / Comet — ❌ don't

- **ToS:** the Terms of Service prohibit automation software (bots), hacks, mods, or other unauthorized third-party tooling in connection with the Services; the February 2026 update explicitly bans bots/scrapers and limits consumer plans — including paid Pro — to personal, non-commercial use. Your fan-out is commercial-adjacent and unambiguously automated.
- **Practice:** perplexity.ai runs aggressive bot walls (even its own pricing page sits behind one, per a July 2026 reviewer). I found no credible 2025–26 practitioner writeups of *sustained* Playwright/Selenium automation against a logged-in perplexity.ai session — the ecosystem discussion is entirely about the generic Cloudflare-fingerprint arms race, which matches your "days of silent failure" experience on other rails. [no source found for perplexity.ai-specific breakage timelines — absence of evidence, flagged]
- **Comet:** free since October 2, 2025, but it exposes no supported automation/extension API for external scripts; its agent is user-driven. And the legal weather around agentic browsing is stormy: Amazon sued Perplexity in Nov 2025, a district judge granted a preliminary injunction on March 10, 2026, finding Comet accessed accounts with the user's permission but without Amazon's authorization; the Ninth Circuit stayed it pending appeal, with oral arguments June 11, 2026, and the ACLU warning that a broad ruling could expose ordinary automation users to CFAA liability. One August 2026 report says the Ninth Circuit has now ruled that the user, not the agent, is the accessing party [single-source, low-authority outlet — verify before relying on it]. Either way: the realistic downside of scripting a paid Perplexity account is account termination without refund plus, in the current climate, non-trivial legal ambiguity. **A rail that can be deleted by a ban-hammer is not a rail.**

---

## D. Cost Model

**Plan tiers (observed Aug 2026):** Free $0; Pro $20/mo ($200/yr ≈ $16.67); Education Pro $10 with verification; Max $200/mo ($2,000/yr, web-only annual); Enterprise Pro $40/seat; Enterprise Max $325/seat; Comet free, Comet Plus $5/mo; Pro includes ~20 Deep Research/day — but note early-2026 user backlash over quietly tightened caps, with pop-ups citing "up to 20 per month" limits contradicting older marketing. Published consumer limits are unstable; do not build capacity plans on them.

**API rate card (docs.perplexity.ai, Aug 11, 2026):**

| Meter | Rate |
|---|---|
| `sonar` | $1/$1 per 1M in/out + $5–12 /1k requests (context low→high) |
| `sonar-pro` | $3/$15 per 1M + $6–14 /1k requests |
| `sonar-reasoning-pro` | $2/$8 per 1M + $6–14 /1k requests |
| **`sonar-deep-research`** | **$2/$8 per 1M + $2/1M citation tokens + $3/1M reasoning tokens + $5/1k search queries** (no flat request fee) |
| Sonar Pro "Pro Search" mode | $3/$15 tokens + $14–22 /1k requests |
| Agent API tools | web_search $0.0025, fetch_url $0.00025, sandbox $0.03/session; third-party models at direct provider rates (e.g., Claude Opus-class $5/$25 per 1M) |
| Search API | $5 /1k requests, no token charges |

**Worked estimate — 5 deep-research reports/day (~152/month):**

Using the representative per-call profile embedded in Perplexity's own pricing calculator (20k citation tokens, 74k reasoning tokens, 18 searches), plus ~1k input / 5k output:

| Component | Per call |
|---|---|
| Input 1k × $2/1M | $0.002 |
| Output 5k × $8/1M | $0.040 |
| Citation 20k × $2/1M | $0.040 |
| Reasoning 74k × $3/1M | $0.222 |
| Searches 18 × $5/1k | $0.090 |
| **Total** | **≈ $0.39** |

Third-party observed ranges agree: $0.30–$1.30+ per query, with reasoning tokens usually the biggest driver; ≈$0.41+ per full deep-research query. Your 30–100-source reports sit at the heavy end, so budget **$0.50–1.50/report → $75–230/month at 5/day**, or **$15–45/month at 1/day**. Running the same workload through the Agent API's `advanced-deep-research` preset on a frontier third-party model would multiply token costs several-fold (Opus-class rates) — plausibly $1.5–5/run, $230–760/month [speculative estimate, no published per-run figures].

**Comparison:** each of your six existing rails delivers deep research inside a flat $20–200/month subscription you already pay. Perplexity would be your only *metered* rail, at roughly 3–10× a Pro subscription's price for the same monthly report count — with no way to buy it flat, because **no subscription tier grants programmatic deep-research access** (the disputed $5 credit ≈ 12 calls at best; credits don't roll over and apply only to API spend).

**Rate limits:** sonar-deep-research: 5 requests/min at entry tier; async submission 5/min, async polling 3,000–6,000/min; Search API a flat 50 QPS on all tiers; tiers rise permanently with cumulative lifetime spend and unlock higher RPM. 5 RPM is irrelevant for 5 reports/night.

**Subscription-vs-API verdict: strictly separate meters.** [established]

---

## E. Quality vs Your Six Rails

| Benchmark (date, independence) | OpenAI DR | Gemini DR | Grok | Perplexity DR |
|---|---|---|---|---|
| ResearcherBench coverage (Jul 2025, **independent**) | **0.703** | 0.693 | 0.441 (DeepSearch) | 0.480 |
| ResearcherBench faithfulness | 0.84 | **0.86** | 0.69 | 0.85 |
| ResearcherBench groundedness | 0.34 | **0.59** | 0.32 | 0.56 |
| Tow Center citation-error rate (Mar 2025, **independent**, article-attribution task) | ~57–73% wrong (ChatGPT Search) | poor | **94% wrong (Grok-3)** | **37% wrong — best of 8** |
| DRACO (Feb 2026, **Perplexity's own**) | lower | lower | n/a | **best in all 10 domains; Law 89.4%, Academic 82.4%; lowest latency 459.6 s vs 592–1,808 s** |

Sources: ResearcherBench's table shows OpenAI and Gemini DR outperforming other systems by 20–30% on rubric coverage, with Perplexity moderate but strong on faithfulness and groundedness; the Tow Center found chatbots collectively wrong on >60% of citation queries, Perplexity best at 37% and Grok 3 worst at 94%; DRACO results show Perplexity leading all ten domains and in factual accuracy, breadth/depth, and citation quality, with the lowest latency — vendor-published, and note DRACO's tasks are sampled from Perplexity's own production Deep Research requests, a structural home-field advantage. Countervailing data point on citations: a 2025 bibliographic-retrieval study found Perplexity among the *most* prone to fabricating academic references (72% fabricated in that task) — citation reliability is strong for *web-page* grounding, weak for *scholarly bibliography* generation. Preserve that split when you decide what job to give this rail.

**What Perplexity adds that you don't already have:** (1) its own crawl/index rather than another wrapper over the same engines — genuine source-pool diversity for your quorum; (2) the best independently measured web-citation discipline of the major vendors (2025 data); (3) speed — sub-8-minute deep runs even by hostile reading of the vendor latency claim; (4) machine-readable per-run cost and search-count telemetry, which none of your browser rails give you. **What it does not add:** depth. On every independent 2025 measurement it trails ChatGPT DR and Gemini DR, which you already run. If Deep Research 2.0 closed that gap, only Perplexity's own benchmark says so — [emerging], unverified.

---

## F. Failure Modes & Silent-Degradation Detection

**Rename/deprecation map:** `llama-3.1-sonar-{small,large,huge}-128k-online` (2024) → deprecated early 2025 → `sonar` / `sonar-pro` / `sonar-reasoning-pro` / `sonar-deep-research` (the `sonar-deep-research` model released March 7, 2025) → 2026: Sonar Chat Completions rebranded as the Agent API with an official migration guide, `/async/chat/completions` → `/v1/async/sonar`, and the MCP server rebuilt around presets. Stale guides still circulating with the 2024 names (I found one dated May 2026) are a live hazard for any LLM-written integration code.

**Documented instabilities:** upstream rate-limiting that fails requests even for users with ample credits and tier headroom, forcing manual queueing (via an aggregator, but the failure was at Perplexity's edge); deep-research presets returning system-instruction text as final output on one model family; the Pro API credit disappearing as surprise 401s. Documented "we ripped Perplexity out of our agent pipeline" post-mortems: **not found** in this pass — the honest reading is that complaints cluster on consumer caps and billing surprises rather than API removals. [no source found — flagged as absence, not evidence of absence]

**Machine-checkable degradation gate (the thing your cron should assert):** deep-research responses carry usage telemetry — reasoning tokens, citation tokens, search-query count, citations, and on the Agent API a computed usage.cost.total_cost on completed responses. A shallow answer masquerading as a report is detectable because it's *cheap*. Suggested assertions per run, calibrated after a 10-run baseline:

```
assert usage.num_search_queries >= 8        # real DR runs ~18+ searches
assert usage.reasoning_tokens  >= 20_000    # representative run: ~74k
assert len(unique_domains(citations)) >= 10
assert output_tokens >= 2_500
assert usage.cost.total_cost >= 0.15        # a $0.02 run is not deep research
```

Any run failing two or more gates → mark rail as degraded, exclude from quorum, alert. This is a materially *better* degradation sensor than anything your browser rails offer.

## G. Strategic Recommendation

**Option 1 — "API-only rail" (recommended).** Wrapper script → `POST /v1/async/sonar` with `sonar-deep-research` (migrate to Agent API `deep-research` preset when you're forced to) → poll → assert gates → write to vault. Trade-offs: only metered rail in the fleet ($75–230/mo at full cadence); interface churn requires an annual migration budget. Risks: pricing/limit changes without notice (demonstrated behavior).

**Option 2 — "Tie-breaker rail."** Same build, fired only when the six-rail quorum splits or on 1 flagship prompt/night: $15–45/mo, most of the epistemic diversity at a fifth of the cost. 

**Option 3 — "Search primitive, not a rail."** Skip rail #7; adopt official `pplx` CLI as a grounded-search tool your *existing* agents call ($0.005/query). Zero new rail complexity; no new report source.

**Option 4 — MCP in interactive sessions only.** Complements 1–3; never for cron.

**Don't build:** headless browser / Comet, under any framing.

**Recommendation: Option 2, upgrading to Option 1 only if a 30-day A/B shows the Perplexity report changes your synthesized consensus on >10% of prompts.** Confidence: **[established]** on the surface choice (API is the only viable unattended surface — every load-bearing fact is multiply corroborated); **[emerging]** on the value-add (independent quality data is pre-2.0 and mixed). **Main assumption:** that a seventh vendor with an independent index adds quorum value even though its measured depth trails your top four — if your synthesis step weights depth over source diversity, Option 3 dominates.

## H. Action Plan (one sitting)

1. Create an API account at the Perplexity API Console; load $10 prepaid credits; export `PERPLEXITY_API_KEY` into the fleet's shared config with a declared budget bucket: **"Perplexity API — metered, NOT subscription"**.
2. If anyone on the team holds Pro: check the console for a monthly $5 credit. Treat presence as a bonus, absence as expected.
3. Fire one sync `sonar-deep-research` call with a real prompt ID; record `usage` fields; confirm citations resolve (spot-check 10 URLs).
4. **Decision point A:** if the run costs >$2 or returns <8 search queries on a genuinely hard prompt — abandon; the rail underperforms its price.
5. Wire the async variant into the wrapper (submit → poll at 60 s → write JSON + rendered MD to vault); add the §F assertion gate; exit 0/1 contract mirroring `pplx`/`grok -p`.
6. Register as rail 7 in the fan-out at tie-breaker cadence; add cost logging per run.
7. After 30 days: compare consensus-shift rate and cost/report vs the other rails. **Decision point B:** promote to full cadence, hold at tie-breaker, or demote to Option 3 (`pplx` search primitive) and delete the rail.
8. Optionally `claude mcp add perplexity` on the interactive machines only, same key.

## I. Sources

Official: docs.perplexity.ai (pricing, rate limits/usage tiers, Agent API quickstart, OpenAI compatibility, CLI overview, SDK — all observed 2026-08-11) · perplexity.ai/hub/blog/agent-api-a-managed-runtime-for-agentic-workflows · perplexity.ai/hub/legal/terms-of-service · github.com/perplexityai/modelcontextprotocol · community.perplexity.ai (Agent API launch thread #4733; Gemini output bug #4374; rate-limit docs bug #5546) · arxiv.org/pdf/2602.11685 (DRACO paper).
Independent benchmarks: arxiv.org/pdf/2507.16280 (ResearcherBench, Jul 2025) · arxiv.org/pdf/2508.04183 (Aug 2025) · cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php (Mar 2025) + niemanlab.org and techspot.com coverage · arxiv.org/pdf/2505.18059 (reference-fabrication study, 2025).
Pricing/plans (third-party, dates as marked): cloudzero.com/blog/perplexity-api-pricing (May 2026) · felloai.com/perplexity-pricing (Aug 2026) · geotoolbox.ai/blog/perplexity-pricing (Jul 2026) · buyersprint.com (Apr 2026) · finout.io (Jun 2026) · suprmind.ai/hub/perplexity/pricing (Aug 2026, [low-authority: competitor]) · developer.puter.com (Jun 2026) · getaiperks.com [low-authority: marketing].
CLI: marktechpost.com/2026/07/27/perplexity-releases-pplx · technosports.co.in (Jul 2026).
Legal/automation: cnbc.com (2026-03-10 injunction) · searchenginejournal.com/amazon-vs-perplexity-the-cfaa-case (Jun 2026) · nohacks.co (Mar 2026) · letsdatascience.com (Jun 2026, ACLU brief) · easternherald.com (2026-08-05, [single-source] appellate outcome) · geeky-gadgets.com/perplexity-bot-scraper-ban (Feb 2026) · techcrunch.com + cyberscoop.com + searchenginejournal.com (Cloudflare stealth-crawling, Aug 2025) · wired/the-decoder/yahoo (AWS probe, Jun 2024).
Instability reports: github.com/pollinations/pollinations/issues/8263 (Feb 2026) · aicerts.ai (usage-cap backlash, Mar 2026, [low-authority]).

---

Two things I could not close in this pass and marked accordingly: verified post-mortems of teams ripping Perplexity out of agent pipelines, and hard adoption metrics (stars, npm downloads, last-commit dates) for the official MCP server and CLI. A deeper background investigation could chase those down across GitHub, forums, and practitioner blogs and pressure-test the cost model against more live workload reports.