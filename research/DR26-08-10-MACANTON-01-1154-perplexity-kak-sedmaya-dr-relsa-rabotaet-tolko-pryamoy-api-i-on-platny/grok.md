# DR26-08-10-MACANTON-01-1154 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-10-MACANTON-01-1154
vendor: xAI Grok
door: cli-headless
rail: grok
cmd: «внутренний путь лаборатории»

collected: 2026-08-10T19:13:36Z
took_sec: 214
exit: 0
sources_unique_urls: 50
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

I'll research Perplexity's four integration surfaces, pricing, quality, MCP, browser automation risks, and failure modes from the live web now.# Perplexity as a Seventh Deep-Research Rail — Deep Investigation Report  
**Research date observed:** 2026-08-10 · **Scope:** unattended multi-machine fleet (cron/launchd), four integration surfaces, cost vs subscription, quality vs existing six rails.

---

## A. Executive Summary

**Recommendation: build an API-only rail using `sonar-deep-research` over the official Sonar HTTP API (preferably async `/v1/async/sonar`), and do not wire browser automation, Comet hacks, or subscription-session scrapers.** MCP is fine for interactive Claude Code/Desktop sessions but is the wrong abstraction for a cron deep-research fan-out. The official `pplx` CLI is **not** a deep-research client — it is Search-API-only and cannot replace `codex`/`grok -p` as a gold-standard rail.  

The hard constraint for this fleet is architectural: **Pro/Max subscription quota does not meaningfully fund programmatic deep research.** Official billing docs state API credits are a separate pay-as-you-go meter; consumer “unlimited” applies to the chat UI only. Expect roughly **$0.40–$0.90+ per deep-research report** (official sample ≈ **$0.82**), or about **$60–$140/month** at 5 reports/day — new money, not burned already-paid bucket. Quality-wise Perplexity’s edge is **speed + dense citations + live web grounding**, not hard multi-hop reasoning leadership; with ChatGPT DR + Gemini DR + Grok Heavy + Claude Research already in the quorum, the seventh rail is **optional diversification**, not a must-have. Confidence on the surface choice: **[established]** for “API yes / browser no”; confidence on “worth the cost given six rails”: **[emerging]** — depends whether citation-breadth diversity measurably moves your synthesis.

---

## B. Key Findings

| # | Finding | Confidence |
|---|---------|------------|
| 1 | Official Sonar HTTP API is production-ready for unattended runs: API-key auth, documented models, async endpoint, rate tiers, structured `usage`/`citations` in responses. | [established] |
| 2 | Consumer Pro/Max subscription is **not** a usable programmatic budget. Help Center: subscription not required for API; API is separate prepaid credits. Secondary sources: Pro once included ~$5/mo API credits; several 2026 writeups say that grant was discontinued or is trivial. | [established] for separation; [emerging]/[single-source] on exact $5 status |
| 3 | Official MCP server exists (`perplexityai/modelcontextprotocol` + remote `https://api.perplexity.ai/mcp`). Tools bill the **API meter**. | [established] |
| 4 | Official CLI `pplx` (Jul 2026) is Search + content snippets only — **not** `sonar-deep-research` chat. | [established] |
| 5 | ToS explicitly bans bots/scrapers/automation against the Services. Browser automation of perplexity.ai is a ban-risk path, not a rail. | [established] |
| 6 | Comet’s agent layer is Chromium + private extensions; no supported public automation API for fleet scripts. Reverse-engineering exists; not supported. | [emerging] |
| 7 | Model renames/deprecations are frequent (llama-3.1-sonar-* → sonar/sonar-pro; sonar-reasoning removed Dec 2025; Sonar Chat Completions → Agent API 2026). Pin model IDs carefully. | [established] |
| 8 | On HLE-class hard reasoning, Sonar Deep Research (~21.1% early 2025) trailed OpenAI Deep Research (~26.6%); Gemini later claimed much higher scores (2025–2026 third-party tables). On SimpleQA/factual speed, Perplexity often looks strong. | [established] early HLE numbers; [emerging] later Gemini claims |
| 9 | Citation quality: Tow Center/CJR (Mar 2025) found Perplexity **best-of-bad** among AI search engines still wrong on **37%** of news-citation probes; paid tiers sometimes more confidently wrong. | [established] |
| 10 | Async deep-research jobs have reported production pain (stuck `IN_PROGRESS`, mid-run stops) in Perplexity community forums 2026. | [emerging] [low-authority] community reports |
| 11 | For a fleet that already has 6 DR rails, Perplexity’s unique residual value is mainly **orthogonal search index + citation density + latency**, not “another reasoning stack.” If subscription-first is non-negotiable, **don’t integrate**. | [speculative] on your synthesis ROI |

---

## C. The Four Surfaces

### C1. (a) Sonar HTTP API — **production-viable for unattended**

**Does it exist officially?** Yes. Perplexity API Platform / Sonar family, including `sonar-deep-research`. Docs: https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research · Platform: https://www.perplexity.ai/api-platform · Pricing: https://docs.perplexity.ai/docs/getting-started/pricing (observed 2026-08-10).

**Auth:** API key from https://console.perplexity.ai/group/keys · `Authorization: Bearer <key>`.

**What it can / cannot do:**
- **Can:** multi-step exhaustive research, hundreds of sources claimed, structured citations + `search_results`, metered `usage` with reasoning/citation/search-query breakdown, long reports (sample ~11k completion tokens + ~194k reasoning tokens).
- **Can:** async submission for long jobs: `POST /v1/async/sonar` (https://docs.perplexity.ai/api-reference/async-sonar-post).
- **Cannot:** burn Pro/Max “Deep Research /day” UI quota. UI deep research and API deep research are different meters.
- **Caveat:** docs show deprecation notice that Sonar Chat Completions is migrating toward **Agent API** (presets: fast/low/medium/high/xhigh). `sonar-deep-research` still documented as a Sonar model with its own 5-meter billing as of observation date. Pin and re-check changelog: https://docs.perplexity.ai/docs/resources/changelog.

**Unattended survival (weeks):** Yes **if** key is in env/secret store, balance auto-top-up enabled, retries on 429, and async poll with hard timeout + failure alarm. Community reports of async status quirks mean you need machine-checkable completion signals (below §F).

**Minimal working example (sync):**

```bash
export PERPLEXITY_API_KEY="pplx-..."

curl --request POST \
  --url https://api.perplexity.ai/v1/sonar \
  --header "Authorization: Bearer $PERPLEXITY_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "sonar-deep-research",
    "messages": [
      {
        "role": "user",
        "content": "Your deep-research prompt ID and full brief here."
      }
    ]
  }' | tee report.json | jq '{model, citations: (.citations|length), usage}'
```

**Async pattern (preferred for cron / multi-minute jobs):**

```bash
# 1) submit
REQ=$(curl -s -X POST https://api.perplexity.ai/v1/async/sonar \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"request":{"model":"sonar-deep-research","messages":[{"role":"user","content":"..."}]}}')
# 2) poll GET /v1/async/sonar/{request_id} until completed (with max wait)
```

Sources: official model page + async API reference cited above.

**Rate limits (Sonar, tier by lifetime credits purchased)** — https://docs.perplexity.ai/docs/admin/rate-limits-usage-tiers (observed 2026-08-10):

| Tier | Lifetime $ purchased | `sonar-deep-research` RPM |
|------|----------------------|---------------------------|
| 0 | $0 | 5 |
| 1 | $50+ | 10 |
| 2 | $250+ | 20 |
| 3 | $500+ | 40 |
| 4 | $1,000+ | 60 |
| 5 | $5,000+ | 100 |

5 reports/day is trivial even at Tier 0; concurrency spikes are the risk, not daily volume.

---

### C2. (b) MCP server — **interactive yes; scheduled rail no**

**Official?** Yes.

| Item | Detail |
|------|--------|
| Repo | https://github.com/perplexityai/modelcontextprotocol |
| npm | `@perplexity-ai/mcp-server` |
| Docs | https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server |
| Remote | `https://api.perplexity.ai/mcp` (Streamable HTTP) |
| Local | stdio via `npx -y @perplexity-ai/mcp-server` |
| Maintainer | Perplexity official org `perplexityai` |
| Adoption signal | Official docs + npm package; GitHub history shows active commits (repo page: 164 commits as of fetch) |

**Tools exposed (official docs/README):**
1. `perplexity_search` — Search API, ranked hits  
2. `perplexity_ask` — Agent API `fast` preset  
3. `perplexity_research` — Agent API `high` preset (deep; can take minutes; streams progress)  
4. `perplexity_reason` — Agent API `medium` preset  

**Important behavioral shift:** older server versions called `sonar-pro` / `sonar-reasoning-pro` / `sonar-deep-research` directly; current tools map to **Agent API presets**, and `strip_thinking` / `reasoning_effort` are removed. That means MCP “research” ≠ necessarily identical engine to UI “Deep Research” or raw `sonar-deep-research`. [established] from official README.

**Billing:** “Tool calls are billed to the API key you connect with at standard API pricing.” Subscription is not the meter.

**Claude Code setup (copy-paste):**

```bash
# Remote (recommended)
claude mcp add --transport http perplexity \
  https://api.perplexity.ai/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"

# Local stdio
claude mcp add perplexity \
  --env PERPLEXITY_API_KEY="your_key_here" \
  -- npx -y @perplexity-ai/mcp-server
```

Optional: `PERPLEXITY_TIMEOUT_MS=600000` for long research (default 5 min per README).

**Third-party MCP servers (differ):**
| Package | Notes |
|---------|--------|
| https://github.com/daniel-lxs/mcp-perplexity | Community; stateful chat history focus; not official [low-authority] |
| https://github.com/jacob-bd/perplexity-web-mcp | Web UI / session based — **no API key**, uses logged-in web → ToS risk; Reddit/self-promo [low-authority] |
| https://github.com/cyanheads/perplexity-mcp-server | API wrapper, validation focus |
| https://github.com/jsonallen/perplexity-mcp | Mentioned in 2026 MCP roundups as search-oriented |
| Proxima multi-AI (Zen4-bit) | Multi-vendor login scrapers — not for unattended production |

**Unattended scheduled agent?** MCP is a **long-lived tool bus for an interactive host**. Failure modes under cron:
- Host process must stay up for multi-minute `perplexity_research`
- Idle timeouts / zombie `npx` children
- Token exhaustion mid-tool without a clean job ledger
- Wrong abstraction: you already want “prompt in → report file out → exit 0”, not “agent may call tools”

**Verdict:** use MCP only as an optional interactive accelerator inside Claude Code; **do not make MCP the seventh rail’s execution path.**

---

### C3. (c) CLI / terminal — **official CLI exists, wrong product for DR**

**Official CLI:** `pplx`  
- Docs: https://docs.perplexity.ai/docs/cli/overview  
- Installer: `curl -fsSL https://github.com/perplexityai/perplexity-cli/releases/latest/download/install.sh \| sh`  
- Repo: https://github.com/perplexityai/perplexity-cli  
- Announcement coverage (Jul 27, 2026): https://www.marktechpost.com/2026/07/27/perplexity-releases-pplx/

**What it does:**
- `pplx search web "..."` → Search API JSON  
- `pplx content snippets "query" <url>` → query-relevant page passages  
- Auth: `PERPLEXITY_API_KEY` or `pplx auth login`  
- Exit contract: success = exit 0 + JSON on stdout; errors JSON on stderr  

**What it does *not* do:**  
No conversational mode, no model selection, no synthesized multi-page deep-research report. MarkTechPost: “It is not a chat client.” So it is **not** analogous to `codex` or `grok -p` for your gold standard.

**Minimal official example:**

```bash
export PERPLEXITY_API_KEY="pplx-..."
pplx search web "kubernetes pod OOMKilled causes" -n 10
```

**Third-party chat CLIs (API wrappers):**
- https://github.com/dawid-szewc/perplexity-cli — simple API client (`perplexity "question"`); stars/adoption modest; third-party [low-authority]  
- Community “Premium Node.js Suite” etc. — not production fleet material  

**Fleet pattern if you need CLI-shaped DR:** thin shell wrapper around Sonar HTTP (your own `pplx-dr`), not the official `pplx` binary:

```bash
#!/usr/bin/env bash
set -euo pipefail
# write report to $OUT; exit non-zero if citations < N or usage.reasoning_tokens < M
curl -sS .../v1/sonar ... | tee "$OUT"
python3 assert_deep.py "$OUT"
```

That wrapper **is** unattended-viable for weeks; official `pplx` alone is not your seventh DR rail.

---

### C4. (d) Headless browser / Comet session — **not production-viable; ToS-hostile**

**ToS position (consumer Services):**  
https://www.perplexity.ai/hub/legal/terms-of-service (page date signals ~2026) prohibits, among other things:
- automation software (bots), hacks, mods designed to modify the Services  
- any robot, spider, crawler, scraper, or other automatic device/process that intercepts, mines, scrapes, extracts, or accesses the Services to monitor/extract/copy/collect information  

(Community restatements track the same bans: https://www.reddit.com/r/perplexity_ai/comments/1qnyrme/heres_what_i_found_out_on_pro/ — [low-authority].)

**API Terms** exist separately for legitimate programmatic use: https://www.perplexity.ai/hub/legal/perplexity-api-terms-of-service — this is the supported path.

**Comet:**
- Marketing: https://www.perplexity.ai/comet — AI browser / personal assistant  
- Reverse engineering (Zenity Labs, Feb 2026): https://labs.zenity.io/post/perplexity-comet-a-reversing-story — documents private extensions (`comet-agent`, etc.) implementing RPC-driven browser automation **from Perplexity’s backend**, not a public scriptable API for customers  
- Third-party Chrome store schedulers like “AutoAgent for Comet” exist [low-authority] — still human-session automation, not a fleet rail  

**Practice reports:** Specific public post-mortems of Playwright vs perplexity.ai 2025–2026 are sparse in high-authority sources; general AI-browser automation literature (2026) flags **account ban risk** when driving vendor UIs without explicit ToS permission. Cloudflare has also published on Perplexity’s own stealth crawling behavior (Aug 2025): https://blog.cloudflare.com/perplexity-is-using-stealth-undeclared-crawlers-to-evade-website-no-crawl-directives/ — relevant as industry context that bot detection is aggressive on both sides; not a license to automate Perplexity’s UI.

**Legal/account-risk framing for a paid account:**
- Realistic downside: account suspension, loss of Pro/Max, loss of history/Spaces, no recourse (“you violated automation/scraper clauses”)  
- For a multi-machine fleet, one shared cookie jar amplifies blast radius  
- **This fails the reliability rule:** captcha, session expiry, anti-bot, and UI churn = silent multi-day outages — exactly the class of failure you already hate  

**Verdict:** **Do not build surface (d).** Zero production-viable path for no-human-in-the-loop.

---

## D. Cost Model

### D1. Consumer / seat plans (secondary synthesis of 2026 guides; verify on-site)

| Plan | Typical 2026 price | Deep Research (UI) | API credits | Notes |
|------|--------------------|--------------------|-------------|-------|
| Free | $0 | Limited | None meaningful | |
| Pro | ~$20/mo or ~$200/yr | Often cited **20/day** | Historically **~$5/mo** claimed by some blogs; official Help: API separate; Reddit: $5 grant discontinued Feb 2026 [single-source]/[low-authority] | “Unlimited” = UI |
| Max | ~$200/mo or ~$2,000/yr | Higher / “unlimited” UI claims | Computer credits (e.g. 10k/mo) ≠ Sonar API | Agentic Computer product, not API DR |
| Education Pro | ~$10/mo | Similar to Pro caps | — | |
| Enterprise Pro | ~$40/seat | Higher seat caps (e.g. 500 DR/day claimed in secondary) | Enterprise procurement | |

Primary UI plan page: https://www.perplexity.ai/pro  
Help “which plan”: https://www.perplexity.ai/help-center/en/articles/11187416-which-perplexity-subscription-plan-is-right-for-you.html  
Secondary pricing roundups: https://felloai.com/perplexity-pricing/ · https://www.finout.io/blog/perplexity-pricing-in-2026 · https://suprmind.ai/hub/perplexity/pricing/ · https://www.eesel.ai/blog/perplexity-pricing  

**Official Help Center billing (critical):**  
https://www.perplexity.ai/help-center/en/articles/10354847-api-payment-and-billing.html  
- API credits: pay-as-you-go; buy in console; auto top-up option  
- **“Is Perplexity Pro required for API usage? No.”**  
- Used credits non-refundable  

**Subscription vs API verdict (decision-critical):**  
**API is a separate meter.** Pro/Max do **not** turn into a meaningful unattended deep-research budget. Even if a $5 Pro credit still exists in some accounts, one official sample deep-research call costs **~$0.82**, so $5 ≈ **6 reports**, not a month of 5/day. [established] on separation; [emerging] on residual free-credit amount.

### D2. Sonar family pricing (official docs calculator data, observed 2026-08-10)

From https://docs.perplexity.ai/docs/getting-started/pricing and model page:

| Model | Input $/1M | Output $/1M | Extra meters |
|-------|------------|-------------|--------------|
| `sonar` | $1 | $1 | Request fee by context: low $5 / med $8 / high $12 per 1K req |
| `sonar-pro` | $3 | $15 | Request: $6 / $10 / $14 per 1K |
| `sonar-reasoning-pro` | $2 | $8 | Request: $6 / $10 / $14 per 1K |
| **`sonar-deep-research`** | **$2** | **$8** | **Citation $2/1M · Reasoning $3/1M · Search queries $5/1K** |

**Official worked sample** (quantum industry report on model page):

| Component | Qty (approx) | Cost |
|-----------|--------------|------|
| Input | ~33 tokens | ~$0 |
| Output | 11,395 | ~$0.091 |
| Citation tokens | 19,028 | ~$0.038 |
| Reasoning tokens | 193,947 | ~$0.582 |
| Search queries | 21 | ~$0.105 |
| **Total** | | **~$0.816** |

### D3. Worked fleet estimate: 5 deep-research reports/day

Assume each report is multi-page with 30–100 sources (your brief). Official sample had ~21 search queries and ~$0.82; heavier briefs can run higher.

| Scenario | $/report | /day | /month (30d) |
|----------|----------|------|--------------|
| Lean (closer to sample, ~20 queries) | $0.50 | $2.50 | **$75** |
| Official sample-like | $0.82 | $4.10 | **$123** |
| Heavy (2× reasoning + 50 queries) | ~$1.50–2.00 | $7.50–10 | **$225–300** |

**Compare to vendors you already pay (order-of-magnitude, not a full audit):**
- ChatGPT Plus/Pro, Gemini Advanced, Grok, Claude Max/Pro, etc. already include **UI or CLI deep research inside subscription** for your existing six rails — marginal cost of another UI DR is often $0 if under quota.  
- Perplexity API DR is **new cash** every call.  
- Relative to OpenAI deep-research API token rates quoted in third-party 2026 comparisons (often much higher input/output $/1M for o-series research models), Perplexity can look **cheaper per timed run** — but that is irrelevant if your alternative is “already-paid subscription rail #3,” not “buy OpenAI research API.”

### D4. Rate limits recap
- Deep research RPM: 5 → 100 by tier (see §C1)  
- Search API: 50 query units/sec (independent of tier)  
- Agent API: up to 2000 RPM at high tiers  
- 429 not billed  

---

## E. Quality Comparison vs ChatGPT DR / Gemini DR / Grok Heavy / Claude Research

### E1. What Perplexity actually differentiates

| Dimension | Perplexity strength | Still unique if you already have 6 rails? |
|-----------|---------------------|-------------------------------------------|
| Citation density / inline sources | Strong product identity; API returns `citations[]` + `search_results[]` | **Partially** — structural citations are easy to machine-parse |
| Freshness / live web index | Core business | **Yes somewhat** — different crawler/index than Google/OpenAI/xAI |
| Latency for “deep” runs | Often **under ~3 minutes** claimed in comparative writeups vs longer OpenAI runs | **Yes** for quorum wall-clock |
| Hard expert reasoning (HLE-class) | Trailed OpenAI at launch era | **No** — not the leader |
| Cost (API) | Competitive vs other *paid* research APIs | **No** vs free-at-margin subscription rails |
| Eloquence / long prose | Good reports | **No** — Claude/ChatGPT already cover |

### E2. Benchmark numbers (cite carefully; eras differ)

| Benchmark | Perplexity | Comparator | Source / date |
|-----------|------------|------------|---------------|
| Humanity’s Last Exam (early deep-research category) | **21.1%** Sonar/Perplexity Deep Research | OpenAI Deep Research **26.6%** | Widely repeated from Feb 2025 OpenAI launch era; e.g. https://www.punku.ai/blog/comprehensive-analysis-deep-research-implementations (Nov 2025) · https://glasp.co/articles/deep-research-tools-compared (updated 2026) |
| SimpleQA (factual short-form) | **93.9%** Sonar (reported) | Strong vs many models | Same comparative articles; [single-source] chains back to vendor/eval packages |
| Later Gemini Deep Research HLE claims | — | Gemini DR **46.4%** / Max **54.6%** in third-party tables | Glasp 2026 summary [emerging] — verify against Google primary before budgeting on it |
| AIMultiple deep-research tooling table (2026) | Sonar ~**87.9%** accuracy / ~2.3 min in one internal suite; Sonar Deep Research **34%** in another plot | Claude Code / Parallel Ultra higher accuracy in same suite | https://aimultiple.com/ai-deep-research [emerging] methodology-dependent |
| News citation probe (not full DR) | Perplexity **37% incorrect** (best of 8 tools) | ChatGPT Search 67% wrong; Grok-3 94% wrong; overall >60% wrong | Tow Center / CJR, Mar 6 2025: https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php · Nieman Lab: https://www.niemanlab.org/2025/03/ai-search-engines-fail-to-produce-accurate-citations-in-over-60-of-tests-according-to-new-tow-center-study/ |

**Preserve disagreement:**  
- Vendor marketing and speed-focused blogs steelman Perplexity as “fastest useful research with densest sources.”  
- Hard-reasoning benchmarks and later Gemini claims steelman “Perplexity is not the accuracy ceiling.”  
- CJR shows even the best AI search citation tool is **wrong more than one-third of the time** on a news-attribution task — so **do not treat citations as ground truth** without a verifier.

### E3. Explicit: what does Perplexity add that you don’t already have?

1. **A different web retrieval stack** (orthogonal failure modes vs Google-backed Gemini and OpenAI browsing).  
2. **First-class machine-readable citations** on the API response (easier assertion than scraping a chat UI).  
3. **Lower wall-clock** for many deep jobs (helps quorum finish).  
4. **Not** a free seventh seat on a paid consumer plan.  
5. **Not** clearly superior synthesis vs ChatGPT DR / Claude Research on hard multi-hop questions.

**Honest negative option:** If the goal is “burn already-paid quota,” Perplexity **adds little for the cost**. If the goal is “maximize cross-vendor disagreement surface and citation coverage,” it can be a thin, cheap-ish seventh vote via API. [speculative] on ROI for *your* synthesis quality.

---

## F. Failure Modes & Silent-Degradation Detection

### F1. Documented / reported failure classes

| Failure | Evidence | Severity for fleet |
|---------|----------|--------------------|
| Model renames / deprecations | Changelog: `llama-3.1-sonar-*-online` removed Feb 22 2025 → use `sonar`/`sonar-pro`; `sonar-reasoning` removed Dec 15 2025; Sonar Chat Completions → Agent API (2026) | https://docs.perplexity.ai/docs/resources/changelog | High if hard-coded old IDs |
| Async job stuck `IN_PROGRESS` / delayed completion | Community bug reports Jan–Apr 2026 | https://community.perplexity.ai/t/sonar-deep-research-taking-longer-than-expected/3599 · https://community.perplexity.ai/t/unable-to-retrieve-the-result-of-an-async-deep-research/4530 | High for cron SLAs |
| Mid-run stops | Community Mar 2026 | https://community.perplexity.ai/t/sonar-deep-research-requests-randomly-stopping-mid-run/4266 | High |
| Agent API timeouts while legacy paths healthy | Community Jun 2026 | https://community.perplexity.ai/t/agent-api-v1-responses-consistently-times-out-with-zero-bytes-while-v1-models-and-chat-completions-are-healthy/5394 | Medium |
| Teams abandon web-scrape MCP | Implicit in ToS + ban risk; web-MCP projects exist precisely because people want subscription not API | GitHub jacob-bd/perplexity-web-mcp etc. | Architectural debt |
| Shallow answer that *looks* deep | Possible if wrong model (`sonar` vs `sonar-deep-research`), truncated finish, or Agent preset `fast` via MCP | Official sample shows deep runs have huge reasoning_token counts | Silent quality fail |

### F2. Machine-checkable acceptance gates for a cron job

Assert **all** of the following before marking the rail ✅:

```text
1. HTTP 200 (or async status == COMPLETED)
2. model == "sonar-deep-research"  (exact string match)
3. finish_reason == "stop" (if present)
4. len(citations) >= MIN_SOURCES          # e.g. 30 for your multi-page bar
5. usage.num_search_queries >= MIN_Q      # e.g. 15
6. usage.reasoning_tokens >= MIN_REASON   # e.g. 50_000  (sample had ~194k)
7. usage.completion_tokens >= MIN_OUT     # e.g. 3_000
8. usage.cost.total_cost within [MIN, MAX] budget envelope
9. Optional: HEAD/GET sample of N citation URLs — reject if >X% hard-404
10. Wall time < TIMEOUT (e.g. 45 min) else FAIL (don’t hang the night batch)
```

**Why this works:** a shallow `sonar`/`sonar-pro` or truncated response will typically fail **reasoning_tokens**, **num_search_queries**, and **citations** together. Official sample response fields (`citation_tokens`, `num_search_queries`, `reasoning_tokens`, `cost`) are exactly the meters you need. Source: model page sample JSON.

### F3. Silent degradation modes to watch
- MCP `perplexity_research` using Agent `high` preset may **not** equal `sonar-deep-research` quality/cost profile after server migration.  
- Returning beautiful prose with few real URLs.  
- Billing continues while quality drops after an unannounced model swap — detect via metrics above, not vibes.

---

## G. Strategic Recommendation

### Named options

| Option | What you build | Pros | Cons / risks |
|--------|----------------|------|--------------|
| **A. API-only rail (recommended)** | Thin client: env key → async `sonar-deep-research` → write JSON+MD → assert gates → exit code | Unattended; official; same shape as gold-standard CLI rails; structured citations | New $60–150+/mo; separate from Pro; async flakiness |
| **B. MCP interactive only** | Official remote MCP in Claude Code | Zero new orchestration for humans | Not a cron rail; bills API; preset ≠ DR model certainty |
| **C. Official `pplx` as search pre-rail** | Use Search API CLI to gather sources, then another model synthesizes | Cheap search units; clean JSON | Not deep research; two-stage complexity |
| **D. Browser / Comet / web-MCP** | Playwright on logged-in Pro | Burns subscription UI quota in theory | **ToS ban**, captcha, silent failure — violates your reliability rule |
| **E. Don’t integrate** | Keep 6-rail quorum | Zero cost/ops; no new vendor churn | Miss orthogonal index/citation diversity |

### Single recommendation
**Option A (API-only seventh rail), optionally plus B for interactive use; never D.**  
If the standing money rule is “only burn already-paid subscription buckets,” choose **Option E** instead — there is no honest Pro/Max path that funds unattended deep research at 5/day.

**Confidence:** **[established]** on technical surface ranking; **[emerging]** on business value given existing six rails.  

**Main assumption:** you will pay Sonar API credits monthly and accept that Pro/Max are unrelated UI products; and that your synthesis benefits from a seventh retrieval stack enough to justify ~$100/mo and ~1 day of engineering.

---

## H. Action Plan (one sitting for a competent engineer)

1. **Decision gate (5 min):** If “must use Pro quota only” → **STOP, Option E**. If “OK to pay API” → continue.  
2. Create API key at https://console.perplexity.ai/group/keys ; buy credits; enable auto top-up.  
3. Run the **minimal sync curl** in §C1 with a 1-paragraph test prompt; save raw JSON.  
4. Run one **full multi-page brief** on `sonar-deep-research`; record `usage.*` and `$ total_cost`.  
5. Implement **async submit + poll** with 45-min hard timeout and exponential backoff on 429.  
6. Codify **assert gates** from §F2 (`assert_deep.py`); fail closed.  
7. Wire into fan-out as rail #7 with same prompt ID; write report path; exit 0 only on gate pass.  
8. **Abandon if:** median cost > $2/report for your briefs **and** synthesis shows no unique claims vs existing rails after a 2-week A/B of “with/without rail 7”; or async failure rate > ~10% after retries.  
9. **Do not:** log into perplexity.ai with Playwright; do not deploy jacob-bd web-MCP for production; do not depend on official `pplx` for DR.  
10. Optional: install official MCP for interactive Claude Code only (`claude mcp add ...`).  
11. Pin model string `sonar-deep-research`; subscribe to changelog https://docs.perplexity.ai/docs/resources/changelog ; re-verify quarterly.  
12. Document in rail passport: **`рельса: Perplexity Sonar API pay-as-you-go`** (not Pro subscription).

---

## I. Sources

### Official Perplexity
1. Sonar Deep Research model + sample cost — https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research (crawled 2026-08-10)  
2. Pricing calculator / Sonar meters — https://docs.perplexity.ai/docs/getting-started/pricing  
3. Rate limits & tiers — https://docs.perplexity.ai/docs/admin/rate-limits-usage-tiers  
4. MCP server docs — https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server  
5. Official MCP GitHub — https://github.com/perplexityai/modelcontextprotocol  
6. Official CLI docs — https://docs.perplexity.ai/docs/cli/overview  
7. Official CLI repo — https://github.com/perplexityai/perplexity-cli  
8. Async Sonar API — https://docs.perplexity.ai/api-reference/async-sonar-post  
9. Changelog / deprecations — https://docs.perplexity.ai/docs/resources/changelog  
10. API payment & billing Help — https://www.perplexity.ai/help-center/en/articles/10354847-api-payment-and-billing.html  
11. Plan comparison Help — https://www.perplexity.ai/help-center/en/articles/11187416-which-perplexity-subscription-plan-is-right-for-you.html  
12. Pro/Max marketing — https://www.perplexity.ai/pro  
13. API platform — https://www.perplexity.ai/api-platform  
14. Terms of Service — https://www.perplexity.ai/hub/legal/terms-of-service  
15. API Terms — https://www.perplexity.ai/hub/legal/perplexity-api-terms-of-service  
16. Comet product — https://www.perplexity.ai/comet  
17. Sonar Pro API intro (historical) — https://www.perplexity.ai/hub/blog/introducing-the-sonar-pro-api (Jan 21, 2025)

### Pricing / secondary
18. Finout Perplexity pricing 2026 — https://www.finout.io/blog/perplexity-pricing-in-2026  
19. FelloAI pricing — https://felloai.com/perplexity-pricing/  
20. Suprmind pricing — https://suprmind.ai/hub/perplexity/pricing/  
21. eesel.ai pricing — https://www.eesel.ai/blog/perplexity-pricing  
22. Puter tutorial API pricing (Jun 2026) — https://developer.puter.com/tutorials/perplexity-api-pricing/  
23. Reddit: $5 Pro API credits discontinued — https://www.reddit.com/r/perplexity_ai/comments/1r4cztm/confirmed_5_monthly_api_credits_for_pro_users/ [low-authority]

### Benchmarks / quality
24. CJR Tow Center citation study (Mar 6, 2025) — https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php  
25. Nieman Lab on CJR study — https://www.niemanlab.org/2025/03/ai-search-engines-fail-to-produce-accurate-citations-in-over-60-of-tests-according-to-new-tow-center-study/  
26. Ars Technica summary — https://arstechnica.com/ai/2025/03/ai-search-engines-give-incorrect-answers-at-an-alarming-60-rate-study-says/  
27. Glasp deep research tools compared — https://glasp.co/articles/deep-research-tools-compared  
28. Punku analysis — https://www.punku.ai/blog/comprehensive-analysis-deep-research-implementations  
29. AIMultiple deep research — https://aimultiple.com/ai-deep-research  
30. arXiv citation URL validity study (2026) — https://arxiv.org/html/2604.03173v1  

### MCP / CLI third parties & coverage
31. MarkTechPost on `pplx` (Jul 27, 2026) — https://www.marktechpost.com/2026/07/27/perplexity-releases-pplx/  
32. dawid-szewc perplexity-cli — https://github.com/dawid-szewc/perplexity-cli  
33. jacob-bd perplexity-web-mcp — https://github.com/jacob-bd/perplexity-web-mcp  
34. Builder.io best MCP servers 2026 — https://www.builder.io/blog/best-mcp-servers-2026  

### Browser / automation / industry
35. Zenity Comet reversing (Feb 11, 2026) — https://labs.zenity.io/post/perplexity-comet-a-reversing-story  
36. Cloudflare on Perplexity crawlers (Aug 4, 2025) — https://blog.cloudflare.com/perplexity-is-using-stealth-undeclared-crawlers-to-evade-website-no-crawl-directives/  
37. Community async issues — links in §F1  

### Community reliability notes
38. Mid-run / async deep research threads — https://community.perplexity.ai/t/sonar-deep-research-requests-randomly-stopping-mid-run/4266 · https://community.perplexity.ai/t/sonar-deep-research-taking-longer-than-expected/3599  

---

### Bottom line (one line)
**Wire Perplexity only as a paid Sonar HTTP `sonar-deep-research` async rail with hard quality gates; treat MCP as interactive sugar; ignore the official search CLI for DR; never automate the website — and if you refuse a separate API bill, skip the seventh rail entirely.**
