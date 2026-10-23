# DR26-07-22-ZB-01-1948 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (Heavy, multi-agent Deep Research)
dr_id: DR26-07-22-ZB-01-1948
topic: "OpenRouter практический гид июль 2026 — Fusion, free tier, экономика, BYOK, интеграция с Claude-флотом"
vendor: grok

date: 2026-07-22

run_proof: "Thought for 2m 5s · 478 sources"
url: https://приватный чат
---

# Grok Heavy — DR26-07-22-ZB-01-1948 (OpenRouter) — verbatim

_Run proof: "Thought for 2m 5s" + 478 sources. Echo of the prompt trimmed; report body verbatim below._

A. Executive Summary (5-minute answer for our stand)

Top up ≥$10 immediately (unlocks free-model daily limit from 50 → 1,000 requests/day while keeping 20 RPM). Use free models (:free or openrouter/free) for low-stakes grunt work (labeling, dedup, classification, extraction, simple routing). Use Management Keys + Guardrails for per-agent API keys with hard spend caps and ZDR. Point selected Claude Code / Agent SDK instances (or sub-agents) at OpenRouter's Anthropic-compatible endpoint for second-opinion rails alongside existing Codex and Grok. Reserve Fusion (openrouter/fusion) for high-stakes research / critique tasks only — it is a multi-model panel + judge, not a free auto-router, and costs ~4–5× a single completion. BYOK your existing OpenAI key for embeddings (or other high-volume traffic) to keep provider rate limits and pay only a small platform fee after the free BYOK allowance. Enforce account-level privacy (opt-out of training + ZDR) from day one. OpenRouter is currently useful as a cheap/free rail + multi-vendor second-opinion layer; it is not a pure cost reduction vs. direct Anthropic for heavy Claude volume.

B. Key Findings (by the 7 questions)

1. OpenRouter Fusion (summer 2026)
Fusion is a multi-model deliberation system exposed as the model alias openrouter/fusion (or as a tool). A request is answered in parallel by a configurable panel (default Quality: latest Claude Opus + GPT + Gemini Pro, with web search/fetch enabled), a judge model produces structured JSON analysis (consensus, contradictions, coverage gaps, unique insights, blind spots), and the outer model synthesizes the final answer. The outer model decides whether to invoke the panel (force with tool_choice: "required"). Cost is N panel calls + 1 judge + outer ≈ 4–5× a single completion for the default 3-panel; scales linearly. There is a faster openrouter/fusion-flash preset. No free/starter volume for full deliberation is documented; the $0 auto-route to Opus 4.8 you observed is consistent with the outer model answering without invoking the panel, residual free credits, or a temporary test path. Genuinely better for research / "cost of being wrong is high" tasks and second-opinion work; worse (expensive + higher latency) for routine agent turns. Pitfalls: cost surprises if the panel fires, quality variance across panel members, recursion protection prevents nested Fusion, and one practitioner observed the judge defaulting to Opus even on a cheap panel. [established] (official docs + June 2026 announcement blog).

2. Free Tier (July 2026)
Official limits for any model ID ending in :free:
- Lifetime credits purchased < $10 → 20 requests/minute + 50 requests/day.
- ≥ $10 lifetime credits → still 20 RPM but 1,000 requests/day.
Pricing page simplifies the free plan to "50 reqs/day." ~16–25 free models currently listed (NVIDIA Nemotron variants, Poolside Laguna S/XS/M, Google Gemma-4, Cohere North Mini Code, OpenAI gpt-oss-20b, etc.); the list rotates. openrouter/free randomly selects a capable free model. Realistic continuous use on $0: light draft labeling, dedup, simple classification/extraction, low-concurrency agent helpers — not sustained multi-agent fleets. Quality/latency are lower and more variable than paid frontier models; free models can be rate-limited or temporarily unavailable. Catches: several free models (explicitly Poolside Laguna :free) may train on inputs/outputs; free models inherit provider rate limits and can be deprecated. [established] (official rate-limits page + free-models collection + FAQ).

3. Economics
OpenRouter states it passes through underlying provider list prices with no token markup; the platform fee is 5.5% charged when you purchase credits (Pay-as-you-go). Example: Claude Opus 4.8 is listed at $5 / $25 per 1M input/output tokens on OpenRouter (matching current Anthropic list prices across providers). Effective cost for credit users is therefore ~5.5% higher than direct for the same tokens. BYOK and free models change the math. Management Keys allow programmatic creation of ordinary API keys with per-key credit limits, daily/weekly/monthly resets, disable flags, and optional BYOK inclusion in the limit — ideal for per-agent budgets and spend caps. When OpenRouter is cheaper: free models, cheaper secondary providers, multi-vendor convenience, or low-volume second-opinion. Same or more expensive for pure high-volume single-provider Claude traffic (direct usually has higher rate limits and possible volume discounts). [established] for fee structure and pass-through claim; [emerging] for exact vs-volume-discount comparison.

4. BYOK (Bring Your Own Key)
Yes — you can attach existing provider keys (OpenAI, Anthropic, Google, Azure, Bedrock, Vertex, etc.). Keys are encrypted, can be prioritized or fallback, and support "always use for this provider." You keep the provider's rate limits and billing; OpenRouter still routes, applies data policies/guardrails, and provides the unified interface + observability. Fee: 5% of what the same model/provider would cost on OpenRouter credits, deducted from your OpenRouter balance; waived for the first 1,000,000 BYOK requests per month (Oct 2025 announcement still referenced in current docs). Pricing page alternatively states $25,000 of list-price inference / month free of fees then 5% (Pay-as-you-go). Both thresholds appear; for a small fleet the request-based free tier is the relevant one. Catches: data policies (ZDR, training opt-out) still apply and can block a BYOK endpoint; "always use" can surface raw rate-limit errors. [established] for 1M-request free tier and mechanics; [single-source variance] on the $25k figure.

5. Account Features — signal vs. marketing for a small technical Claude fleet
Ranked by practical value for us:
- Management Keys + Guardrails (highest) — programmatic per-agent keys with spend caps/resets + budgets, ZDR enforcement, model/provider allowlists, PII redaction/block, prompt-injection filters. Directly useful.
- Routing / provider object / variants (:nitro, :floor, Auto Router) — concrete control over failover, latency, cost, and privacy.
- Presets + Workspaces — organize system prompts, model defaults, and isolation per project/agent without code changes.
- Observability (Broadcast to OTEL, PostHog, etc.) — useful monitoring with low friction.
- Classifiers (Beta) — async request tagging for analytics; optional and lower priority.
Fusion itself is a specialized high-value tool, not a daily default. Most of these are genuine for a small multi-agent fleet; enterprise marketing is limited to SSO/SAML, contractual SLAs, and higher BYOK free thresholds. [established].

6. Integration with Claude Code / Claude Agent SDK
OpenRouter exposes an Anthropic Messages-compatible endpoint. Official cookbook pattern:
```
export ANTHROPIC_BASE_URL="https://openrouter.ai/api"
export ANTHROPIC_AUTH_TOKEN="<openrouter-key>"
export <key-var>="" # must be empty string
# optional model overrides
export ANTHROPIC_DEFAULT_OPUS_MODEL="~anthropic/claude-opus-latest"
```
Works for Claude Code and can be mirrored for Agent SDK (Anthropic client with base URL override or OpenAI-compatible client pointed at OpenRouter). Direct path is sufficient for many Anthropic-model cases; community proxies (y-router etc.) improve compatibility for betas, context management, or non-Anthropic models. Prioritize Anthropic first-party provider in routing for best fidelity. For pure second-opinion rails, spin separate agent instances or sub-agents that call OpenRouter models (including Fusion) while primary agents stay on direct Claude. [established] (official cookbook).

7. Risks
- Rate limits: Free models hard-capped at 20 RPM + daily bucket; paid traffic inherits provider limits + Cloudflare DDoS. Concurrent agents hit them quickly.
- Data policy: OpenRouter itself does not train or log prompts by default (opt-in for 1% discount or observability). Providers vary ("May train" includes DeepSeek, NVIDIA, and several free models such as Poolside). Force no-training via account privacy settings + per-request provider: { "data_collection": "deny" } or ZDR flags; Guardrails can enforce ZDR. Free models are higher risk.
- Reliability: Status page currently all-operational (90-day high uptime). Documented outages 17 & 19 Feb 2026 (~30–40 min, third-party caching dependency). Earlier gateway incident Aug 2025. No major last-30-day systemic outages reported.
- Last ~6 months: Series B $113 M led by CapitalG (Alphabet) in May 2026 at ~$1.3 B valuation; Fusion launched ~June 2026; free-model roster and Poolside Laguna promotions ongoing; model deprecations continue (common industry pattern). Practitioner X chatter (last 30 days) focuses on free-model rate-limit friction, Fusion quality vs. cost, and free Laguna promotion by CEO @alexatallah / @OpenRouter. [established] for outages, funding, privacy controls; [emerging] for long-term free-model stability.

C. Contradictions & Debates
- BYOK free threshold: Official BYOK docs + Oct 2025 blog = first 1 M requests/month free of the 5% fee. Pricing page = $25 k list-price inference / month free then 5%. Possible dual metric or approximation; small-team reality is the request count.
- "OpenRouter is cheaper" vs. "margin makes it pointless": Token prices are pass-through (multiple independent sources confirm identical list prices for Claude/GPT/Gemini). Effective cost is +5.5% (credit fee) or +5% (BYOK after free tier). Cheaper only when free models or cheaper secondary providers are used, or when unified routing/observability/failover has value. For pure high-volume single Claude traffic, direct is usually better (limits + possible discounts). Practitioners on X and blogs split: convenience and multi-vendor second-opinion worth the fee for many; pure cost optimizers stay direct.
- Fusion $0 observation: Docs make clear full deliberation costs 4–5×; $0 is consistent with outer model not invoking the panel or residual free capacity.
- Free daily limits: Pricing page says 50; detailed limits page says 50 → 1 000 after $10 lifetime credits. Secondary sources occasionally cite older 200 RPD numbers. Official detailed page governs.
- Steel-man the "pointless margin" view: if you only ever call Claude Opus at high volume and already have direct Anthropic keys with good limits, the 5.5% is pure overhead. Counter: free grunt capacity + multi-vendor second-opinion + per-agent budgets + failover is real value for a Claude-first multi-agent fleet.

D. Best Practices (Claude-first multi-agent fleet)
- Top up ≥$10 once; keep a small credit balance.
- Free models only for non-sensitive, low-stakes, rate-limit-tolerant work; always check Activity for which model actually ran.
- Create Management Keys → provision per-agent (or per-machine) inference keys with tight daily/weekly limits + Guardrails (ZDR + model allowlist + budget).
- For second-opinion rails: either (a) separate Claude Code instances pointed at OpenRouter, or (b) Agent SDK sub-agents that call OpenRouter models/Fusion.
- Always set privacy opt-out of training + enforce ZDR / data_collection: "deny" for anything non-public.
- Prefer explicit model IDs over pure auto-routers for production agents; use Fusion only when the cost of error justifies 4–5× spend.
- Monitor Activity dashboard + set spend alerts; use :nitro or provider preferences for latency-critical paths.
- BYOK OpenAI key for embeddings or other high-volume OpenAI traffic while volume stays under the free BYOK allowance.

E. Failure Modes
- Free-model 20 RPM / daily bucket exhausted → 429s that cascade across concurrent agents.
- Fusion panel fires unexpectedly → sudden 4–5× cost spike.
- Free or "May train" providers used without ZDR → prompts retained or trained on.
- Unpinned free models rotate or disappear.
- Direct Anthropic-compatible path + experimental betas → context-management or tool-calling edge cases (mitigate with proxies or disable flags).
- BYOK "always use" + exhausted provider key → hard rate-limit errors with no OpenRouter fallback.
- Gateway-level outage (rare but documented) takes the routing layer down even if providers are up.
- Credit balance goes negative → free models also blocked until topped up.

F. Strategic Recommendations
Option 1 – Minimal / free-first: Top up $10, use free models + Management Keys for grunt work, keep primary agents on direct Claude/Codex/Grok. Trade-off: very low cost, but rate limits and quality variance constrain scale. Risk: free-model deprecation or training. Confidence high for light use.
Option 2 – Hybrid second-opinion (recommended): Free for grunt + selective Fusion / paid OpenRouter models for hard second-opinion tasks + BYOK for embeddings + per-agent keys/Guardrails. Primary stack remains Claude-first. Trade-off: modest platform fee + some operational complexity; gains unified observability, failover, and multi-vendor critique. Risk: cost surprises if Fusion is over-used.
Option 3 – Full migration to OpenRouter as primary rail: Move everything through OpenRouter (including Claude via Anthropic skin). Trade-off: maximum convenience and failover; permanently pay ~5% overhead and accept gateway dependency. Not optimal while direct Claude remains the quality leader and rate-limit king for us.
Recommendation: Option 2 (Hybrid second-opinion). Confidence: [established] on mechanics and free limits; rests on the assumption that free-model capacity + selective Fusion + per-agent spend controls deliver more value than the small platform fee for a multi-vendor, multi-agent fleet that already has strong direct Claude rails.

G. Action Plan (concrete sequence)
1. Top up ≥$10 credits today (unlocks 1 000 free RPD).
2. In account Privacy settings: opt out of training for both paid and free models; enable ZDR preference.
3. Create a Management Key → provision 3–5 ordinary API keys with daily/weekly spend limits and attach Guardrails (budget + ZDR + preferred model allowlist).
4. Add your existing OpenAI key under BYOK (prioritized) for embeddings / secondary traffic.
5. Configure one test Claude Code / Agent SDK instance with the official Anthropic-compatible env vars; verify /status and Activity dashboard.
6. Run a controlled free-model wave (labeling / classification) under the new 1 000 RPD limit; measure quality vs. latency.
7. Test Fusion once on a hard research prompt (force tool if needed) and record actual cost.
8. Wire selected second-opinion sub-agents or parallel rails to OpenRouter models; keep primary agents on direct Claude.
9. Set up basic Observability export if useful; monitor for a week before expanding.
10. Document the exact model allowlist and Guardrail IDs for the rest of the fleet.

H. Sources
Primary / official (retrieved July 2026):
- https://openrouter.ai/pricing
- https://openrouter.ai/docs/api_reference/limits
- https://openrouter.ai/docs/guides/routing/routers/fusion-router
- https://openrouter.ai/blog/announcements/fusion-beats-frontier/
- https://openrouter.ai/docs/guides/overview/auth/byok
- https://openrouter.ai/docs/guides/overview/auth/management-api-keys
- https://openrouter.ai/docs/guides/features/guardrails
- https://openrouter.ai/docs/cookbook/coding-agents/claude-code-integration
- https://openrouter.ai/docs/guides/privacy/provider-logging
- https://openrouter.ai/docs/faq
- https://openrouter.ai/collections/free-models
- https://openrouter.ai/docs/guides/routing/routers/free-router
- https://openrouter.ai/blog/announcements/openrouter-outages-on-february-17-and-19-2026/
- https://status.openrouter.ai/
- https://openrouter.ai/anthropic/claude-opus-4.8
Secondary / corroborating: TechCrunch / The Information Series B coverage (May 2026), practitioner blogs and X posts from verified accounts (@alexatallah, @OpenRouter, technical users June–July 2026).
X examples (last ~30 days): @OpenRouter / @alexatallah promoting free Laguna S 2.1 (21 Jul 2026); practitioner notes on Fusion quality vs. cost and free-tier RPM friction (mid–late July 2026).
