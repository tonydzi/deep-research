# DR26-07-28-HUB-20-2339 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Model Fallback Architecture"
type: deep-research-original
source: chatgpt

dr_id: DR26-07-28-HUB-20-2339
conversation_id: 6a49968f-60fc-83eb-8b86-e0e82fd62251
url: "https://приватный чат"
research_started_at: 2026-07-04T23:26:27.388884Z
research_stopped_at: 2026-07-04T23:37:20.880792Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-07-28T23:39:10
vault_chat_note: "2026-07-05-model-fallback-architecture-6a49968f.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
Conduct a deep research investigation on the following topic.



TOPIC: Best architecture for MODEL-FALLBACK and cost-resilience for an always-on personal agent setup running on SUBSCRIPTION buckets (Claude Max/Weekly) that must degrade gracefully when the primary model hits a limit — WITHOUT accidentally spending on paid API. Compare local models (Ollama/Qwen), cheap hosted fallbacks (OpenRouter Minimax, Kimi), and subscription-only ladders; solve that `claude -p` / Claude Code cannot self-switch model/provider on a limit.



CONTEXT: Transport/pings already 0-LLM (OS scheduler + dumb Python detectors), survive any cloud outage. The failure point is the THINKING step: an inbox robot on --model sonnet hit "weekly limit" and silently failed to a log → a peer saw "hub offline". Root: `claude -p`/Claude Code does not self-fallback → external wrapper-ladder needed. An open internal DR exists (local Ollama+Qwen, deferred pending 16GB GPU). Community options: cheap 2nd LLM (Minimax ~$10/mo, Kimi 2.6 on OpenRouter) as fallback so one agent self-heals; practitioners BAN raw API keys to avoid surprise spend, keep coding-plan subscription + cheap fallback; a solo user manually spends ≤~30% of a Claude Max 20x weekly limit/day → swarms needed to utilize it. Rule: exhaust subscription before paid API; paid only by explicit agreement; hub has 2 GPUs, laptop thin.



OBJECTIVE: Design the fallback LADDER (primary subscription → cheap/local fallback → if-ever paid API), the wrapper that detects a limit/failure and switches, guards against accidental paid spend, and where local models fit given a 2-GPU hub.



RESEARCH REQUIREMENTS:

1. Academic — model routing/cascades, cost-aware inference, LLM failover/reliability.

2. Industry — OpenRouter routing/fallbacks, LiteLLM proxy, LangChain fallbacks, Ollama/vLLM local serving, Minimax/Kimi/DeepSeek/Qwen cost-quality; wrapping `claude -p` for failover.

3. Competitive — local vs cheap-hosted vs multi-subscription strategies.

4. Failure analysis — silent-fail-to-log, accidental paid-API blowups, quality cliffs on weak fallback, local-model reliability.

5. Emerging trends — last 12 months (cheap/local model quality + failover tooling).

6. Implementation — concrete ladder + wrapper (detect limit/error → switch → fail LOUD not to a log) + hard spend-guards + which local model on the 2-GPU hub + monthly cost envelope.



OUTPUT: exec summary; fallback-ladder table (cost + quality + trigger per rung); wrapper design (detection + switch + fail-loud + spend-guard); explicit recommendation — is ~$10/mo OpenRouter fallback worth it vs local-only vs subscription-only, and under what condition; sources.@Deep research
```

## Отчёт
# Best Architecture for Model Fallback and Cost Resilience in a Subscription-First Personal Agent

## Executive summary

The core problem is not “how do I add retries,” but “how do I separate billing domains while still recovering automatically.” Anthropic’s own Claude Code gateway documentation makes the key constraint explicit: when a gateway credential is active, the user’s claude.ai subscription is **not** used; traffic is billed per token to the gateway credential owner instead. In other words, one always-on gateway cannot transparently mean “use Claude Max subscription first, then automatically spill into paid API” inside the same authenticated Claude Code session. Claude Code also has a `fallbackModel` setting, but Anthropic documents it as an alternative model used after repeated **availability** failures, not as a cross-provider or cross-credential policy engine. That is why your failure mode has to be solved outside bare `claude -p` / Claude Code, with an external supervisor that chooses among distinct execution profiles. citeturn36view0turn36view1turn1view0

The practical answer for your setup is a **three-domain ladder**: keep the primary rung on native Claude Max / Claude Code using your consumer subscription; keep the second rung on your own hub as a **local** inference service for zero incremental cloud spend; and keep an **explicitly disabled by default** third rung for paid API, enabled only if you deliberately authorize a tiny monthly insurance budget. If you do authorize that budget, a capped cheap-hosted fallback is usually worth it, because OpenRouter already gives you provider failover and opt-in model fallbacks, while current cheap models like DeepSeek V4 Flash, Qwen3 Coder Next, and MiniMax M3 are inexpensive enough that a $10 cap buys a lot of emergency continuity. citeturn6view2turn24view0turn25view0turn6view3

For your specific “always-on inbox robot” case, the best default architecture is therefore:

1. **Primary:** native Claude Max / Claude Code, no API keys present in process env.
2. **Fallback:** local hub runner on Qwen-class open-weight models, served from the 2-GPU hub.
3. **Optional paid insurance:** a tightly capped OpenRouter key on a strict allowlist of cheap models, enabled only by explicit policy.
4. **Final state:** fail loud to humans and downstream systems, never silently to a log. citeturn38view0turn13view3turn13view2turn26view3turn34view0

## What the research and vendor docs imply

The academic literature strongly supports **routing and cascading** as the right mental model. Hybrid LLM showed that routing by predicted query difficulty can cut calls to the larger model by up to 40% without reducing response quality. RouteLLM showed that learned routing can reduce cost by more than 2x in some cases without materially compromising quality, and that routers can transfer across model pairs. More recent work argued that the best results come from combining routing and cascading, with **quality estimation** as the critical ingredient; the 2026 survey frames practical multi-model systems as compositions of pre-routing, post-generation escalation, uncertainty estimation, and deployment constraints rather than one universal algorithm. citeturn19view2turn19view3turn19view0turn19view4turn18view0turn18view3

That matters here because your problem is **not** cost optimization in the abstract. It is operational continuity under a hard subscription quota. In this situation, you need two distinct mechanisms:

- **Failure-driven failover** for rate limits, billing errors, provider outages, and authentication problems.
- **Quality-aware degradation** so the fallback agent changes behavior when it moves to a weaker model, instead of pretending nothing changed. citeturn6view2turn34view0turn18view0

Vendor tooling has also matured noticeably over the last 12 months. Anthropic now documents first-class LLM gateway support for Claude Code; OpenRouter documents separate provider failover and model fallbacks; LiteLLM documents proxy-level fallbacks, provider/model budgets, and even budget-triggered fallback chains; and LangChain now exposes model-fallback middleware and runnable fallbacks. The important operational point is that these tools help **only if the agent loop is wrapped in software you control**. They do not rescue an unwrapped `claude -p` process that reaches a limit and exits while some outer scheduler keeps assuming success. citeturn36view0turn36view1turn6view1turn6view2turn27view0turn27view1turn27view2turn27view3turn11search1turn9search3

The model market has also shifted. Open-weight models have become materially stronger for coding and agentic work. Artificial Analysis placed Kimi K2.6 at 54 on its Intelligence Index and described it as the new leading open-weights model in April 2026. Artificial Analysis scored MiniMax M3 at 55 in June 2026. Qwen3-Coder and Qwen3.6-35B-A3B pushed the “small active parameter, strong agentic coding” trend further, with Qwen explicitly positioning Qwen3-Coder as comparable to Claude Sonnet 4 on open-model agentic coding benchmarks, and Qwen3.6 emphasizing strong coding with only 3B active parameters. That does **not** erase the reliability gap versus frontier Claude on long, messy coding loops, but it makes a local fallback far more viable than it was a year ago. citeturn20search1turn20search3turn13view0turn32search0turn32search1

## Option set and competitive trade-offs

A useful way to think about the architecture is to separate three strategies: **local-only fallback**, **cheap hosted fallback**, and **second-subscription fallback**.

| Strategy | What it solves best | What it breaks first | When it is the right answer |
|---|---|---|---|
| Local-only fallback | Zero new cloud spend, privacy, independence from provider outages | Quality cliffs on harder tasks; GPU/driver/runtime reliability becomes your problem | When “must not spend accidentally” outranks “must stay near-Claude quality” |
| Cheap hosted fallback | Fastest path to self-healing with minimal engineering; strong continuity for low dollars | Still a paid API; needs hard caps and model/provider allowlists | When continuity matters enough to pre-authorize a tiny explicit insurance budget |
| Second subscription | Predictable monthly cost, less “oops” risk than raw PAYG, often integrates into coding tools directly | Another quota system; still requires external switching; idle capacity is wasted | When fallback usage is frequent enough that flat monthly pricing beats small sporadic API usage |

That matrix follows directly from the gateway and pricing mechanics. Anthropic’s docs make clear that gateway credentials replace subscription usage for that session, so a “single magical gateway” does not preserve the consumer-subscription-first rule. OpenRouter adds a reliability layer with automatic provider failover and opt-in model fallbacks, but it is still API billing. MiniMax’s Token Plan is structurally different: it is a subscription product with its own subscription key, separate from pay-as-you-go API keys, and MiniMax documents an Anthropic-compatible endpoint and direct Claude SDK / Claude Code-oriented setup. That makes MiniMax the strongest subscription-only secondary rung if you refuse variable-billed APIs. citeturn36view0turn36view1turn6view2turn22view0turn22view1turn22view2

There is also a crucial compatibility detail. OpenRouter’s Claude Code integration documentation says Claude Code with OpenRouter is only guaranteed with the **Anthropic first-party provider**, and also says Claude Code is optimized for Anthropic models and “may not work correctly with other providers.” So if your fallback model is Qwen, Kimi, DeepSeek, or MiniMax, the robust design is **not** “keep everything inside Claude Code and hope model strings are enough.” The robust design is to use a **runner abstraction**: Claude Code for the subscription rung, a generic local runner for the local rung, and a generic cheap-hosted runner for the paid rung. citeturn28view0

That leads to the following practical judgments.

Local models belong in the architecture, but on the **hub**, not the thin laptop. Ollama is operationally simpler: it exposes a local API at `http://localhost:11434/api` with minimal setup. vLLM is the better hub-tier service because it exposes an OpenAI-compatible API, is built for high-throughput serving, and scales from one GPU to multi-node setups with observability and back-pressure support. For a personal always-on agent, that usually means: Ollama for dev convenience and smoke tests, vLLM for the real hub service. citeturn13view3turn13view2

For actual model choice, the safest local picks are the **Qwen middleweights**, not giant MiniMax open models. Qwen3-Coder-30B-A3B-Instruct is a 31B-parameter BF16 model on Hugging Face, and Qwen3.6-35B-A3B is a 36B open-weight model explicitly documented as vLLM-compatible. By contrast, MiniMax-M2.7’s official model card shows a 229B-parameter model. Even though it is efficient in active parameters, total weight size still dominates local serving practicality. On a 2-GPU hub of unknown VRAM, Qwen 30B/35B-class models are the sane first target; MiniMax M2.7 is a different deployment class unless your hub is unusually fat or you accept aggressive quantization and offload. citeturn30view1turn30view0turn31view2turn30view2

## Failure modes that matter in this design

Your existing incident is the canonical one: the LLM step hit a limit, the process failed, and the failure disappeared into logs while the rest of the system inferred “hub offline.” Anthropic’s SDK and hook docs show exactly where to instrument this. The Agent SDK exposes final result messages with `is_error` on failed conversations and then raises an exception because the CLI exits non-zero. Claude Code hooks also expose a `StopFailure` event with structured error types such as `rate_limit`, `billing_error`, `authentication_failed`, `overloaded`, and `server_error`; Anthropic explicitly says to use this hook for recovery actions and alerts. That means “silent fail to log” is not a platform inevitability; it is an integration bug. citeturn33search1turn33search2turn34view0

The second critical failure mode is **accidental paid spend**. Anthropic’s gateway docs say a gateway credential replaces the subscription for the session, and OpenRouter’s docs show that API keys can have limits and expose remaining credit via `/api/v1/key`. OpenRouter also supports organization-level guardrails and workspace budgets, but those higher-level controls are organization or enterprise features, not the basic solo-user answer. LiteLLM can add another enforcement layer with proxy-wide budgets, provider budgets, and model budgets. The architectural implication is simple: the only safe paid rung is a **dedicated, separately named, separately capped key** with a strict allowlist and no frontier-model access. Do not share your everyday API key with the fallback wrapper. citeturn36view0turn26view3turn26view1turn26view0turn27view1turn27view3turn7search11

The third failure mode is the **quality cliff**. Academic routing work repeatedly shows that cost savings without quality collapse depend on good difficulty or confidence signals. If you drop from Claude Max straight to a weaker local model but keep the same unrestricted prompt and action permissions, you are accepting silent quality degradation exactly where agentic systems are most brittle. The fix is not only “switch model”; it is “switch model **and** switch policy.” The fallback prompt should be narrower, more structured, and more validation-heavy than the primary. citeturn19view2turn19view0turn18view0

The fourth failure mode is **local unreliability**. Local does not mean reliable by default; it means you traded vendor failure domains for GPU memory pressure, driver issues, queue collapse, and your own inference stack. vLLM’s serving docs emphasize observability, autoscaling, and back-pressure in production-grade deployments for exactly this reason. Local is excellent as a cost-resilient rung, but it still needs health checks, queue depth limits, and timeout policies. citeturn13view2

A smaller but important footnote: OpenRouter’s “zero completion insurance” is useful, but it is not enough as your spend safety story. OpenRouter says zero-token error responses are not charged, and its reliability blog says customers generally do not pay for requests that ultimately fail after failover is exhausted. The same blog also warns about edge cases like some 429 paths and partial outputs that may still consume credits. So the correct policy is “use platform protections **plus** hard caps **plus** your own incident logging,” not “trust the router and forget about it.” citeturn26view2turn6view2

## Recommended fallback ladder

The best ladder for your stated governance is below. It is deliberately **not** “subscription, then straight to paid API.” It is “subscription, then local, then optionally paid if you have explicitly turned that rung on.”

| Rung | Implementation | Monthly cloud cost | Expected quality floor | Trigger to enter | Guard condition |
|---|---|---:|---|---|---|
| Primary subscription | Native Claude Max / Claude Code with saved claude.ai login; no gateway credential active | $100 or $200 fixed depending on Max tier. citeturn38view0 | Best overall, especially for messy long-horizon coding and tool use | Normal state | Process env must **not** contain `ANTHROPIC_AUTH_TOKEN`, `<key-var>`, or `ANTHROPIC_BASE_URL` pointing to a paid gateway |
| Local resilience rung | Hub-hosted vLLM service on **Qwen3.6-35B-A3B** as first choice, or **Qwen3-Coder-30B-A3B-Instruct** for lower-memory deployments; Ollama acceptable for simpler operation. citeturn30view0turn31view2turn30view1turn13view3turn13view2 | $0 incremental cloud spend | Good enough for inbox triage, summarization, classification, constrained coding edits; weaker than Claude on ambiguous or long action loops | `rate_limit`, `billing_error`, subscription exhaustion, or primary timeout | Tight prompt, structured JSON output, reduced action scope, validation before commit |
| Paid micro-budget rung | OpenRouter with a dedicated capped key and strict allowlist; first choice **Qwen3 Coder Next** or **DeepSeek V4 Flash**, second choice **MiniMax M3**, optional **Kimi K2.6** only when coding quality matters enough to justify higher cost. citeturn24view0turn25view0turn6view3turn6view4 | Default $0, hard cap e.g. $10 | Better continuity than local-only; DeepSeek is cheapest, Qwen is a better cost-sensitive coding specialist, MiniMax is stronger for long-context agentic work, Kimi is strongest here but much pricier | Local rung unhealthy, local validation fails repeatedly, or task class is marked “high-value/high-complexity” | `ALLOW_PAID_FALLBACK=1`, dedicated key with per-key limit, provider/model allowlist, no BYOK, loud alert on every paid invocation |
| Fail-closed rung | Human page + durable incident + queue deferral | $0 | No automation | All previous rungs unavailable or unsafe | Never silently succeed with no answer |

Two practical notes matter. First, Claude Code’s own `fallbackModel` can still be useful **inside** the primary rung for same-credential Anthropic availability failures, but it does not solve your cross-bucket problem. Second, if you want a **subscription-only** second cloud rung instead of a paid API rung, MiniMax Token Plan is the best fit among the sources reviewed: MiniMax documents fixed monthly subscription tiers, weekly and rolling quota windows, a separate subscription key, and an Anthropic-compatible endpoint intended for coding tools. That is structurally cleaner than raw API credits if you know your cloud fallback will be used frequently enough to justify another flat monthly plan. citeturn1view0turn22view0turn22view1turn22view2

## Wrapper and guardrail design

The wrapper should be a **supervisor state machine**, not a shell alias. The simplest durable design is a small Python daemon or service with three runner profiles: `claude_subscription`, `local_qwen`, and `paid_openrouter`. The supervisor owns task dispatch, process launch, timeout handling, output validation, alerts, and billing-domain policy.

A concrete control flow looks like this:

1. Launch the primary run with the native Claude subscription profile.
2. Treat any of the following as a hard failure: non-zero exit; SDK exception; final SDK result with `is_error`; Claude `StopFailure`; no first token before a short deadline; no completion before the end-to-end SLA; structurally invalid output.
3. Classify the failure. If it is clearly subscription- or provider-related, move to the local rung immediately. If it is a malformed prompt or bad task payload, fail closed instead.
4. On entering any fallback rung, change **both** model and policy:
   - tighter prompt;
   - lower action permissions;
   - required JSON schema;
   - explicit `served_by`, `fallback_level`, and `degraded` flags in metadata;
   - post-generation validation.
5. If the local rung fails or returns invalid output repeatedly, consult paid-fallback policy. If disabled, fail closed. If enabled, launch the cheap-hosted runner with the capped key.
6. Every rung change emits a loud notification to the operator and to downstream status consumers. “Recovered on fallback” is not success-equivalent to “primary healthy.”

Anthropic’s SDK and hook model support this design well. The SDK exposes error-state result messages and exceptions, and hooks expose `StopFailure` with typed error values. Claude Code hooks also support universal JSON output with `continue: false` and a `stopReason`, plus terminal notifications via `terminalSequence`. That gives you the pieces to stop execution decisively and page loudly instead of dribbling errors into transcripts. citeturn33search1turn33search2turn34view0turn34view2turn34view3

A minimal policy file can look like this:

```yaml
profiles:
  claude_subscription:
    runner: claude_code
    env_clear:
      - ANTHROPIC_BASE_URL
      - ANTHROPIC_AUTH_TOKEN
      - <key-var>
      - OPENROUTER_API_KEY
    model: primary_default
    fallback_model: same-account-only

  local_qwen:
    runner: openai_compatible
    base_url: http://hub:8000/v1
    model: Qwen/Qwen3.6-35B-A3B
    timeout_s: 90
    require_json: true
    action_scope: reduced

  paid_openrouter:
    enabled: false
    runner: openai_compatible
    base_url: https://openrouter.ai/api/v1
    api_key_env: OPENROUTER_FALLBACK_KEY
    model_chain:
      - qwen/qwen3-coder-next
      - deepseek/deepseek-v4-flash
      - minimax/minimax-m3
    monthly_cap_usd: 10
    allow_providers:
      - openrouter
    forbid_models:
      - anthropic/*
      - openai/*
      - google/*
```

The spend-guard is the most important part of that design. The reliable pattern is:

- **No paid credentials in the primary process environment.**
- **A separate paid-fallback key** created only for fallback traffic.
- **A hard dollar limit** on that key.
- **An allowlist** that contains only the cheap fallback models you actually accept.
- **No BYOK** and no frontier models on that key.
- **A metric poll** against the key status endpoint before and after every paid call.
- **A mandatory alert** whenever the paid rung is used. citeturn26view3turn26view1turn7search11

LiteLLM is valuable when you want one proxy to own budget logic and provider/model budgets. It can enforce global budgets, per-provider budgets, per-model budgets, and proxy-level fallback routing. But one of its newest features, `budget_fallbacks`, is intentionally **transparent**: it silently reroutes traffic once a key’s model budget is exceeded. That is useful for uptime, but it conflicts with your “fail loud” requirement unless you add explicit notifications and traces around reroutes. For a solo personal agent, that usually means either using LiteLLM as the enforcement layer **plus** your own notification layer, or keeping the ladder logic in your own wrapper where transparency is impossible. citeturn27view0turn27view1turn27view2turn27view3

If you later migrate the “thinking step” away from CLI orchestration and into code, LangChain’s fallback middleware and runnable fallbacks become relevant. Right now, they are not the first fix. Bare `claude -p` failing in production is a supervision problem before it is a chain-composition problem. citeturn11search1turn9search3

## Cost envelope and final recommendation

The fixed subscription baseline is straightforward: Claude Max is $100/month for Max 5x and $200/month for Max 20x, and Anthropic says Max plans still have weekly usage limits, including a global weekly limit and a Sonnet-specific weekly limit. Anthropic also announced in May 2026 that it doubled Claude Code five-hour rate limits for Pro, Max, Team, and seat-based Enterprise plans, which improves headroom but does not remove the need for fallback architecture. citeturn38view0turn38view1

For the optional paid rung, the current market prices are low enough that “fallback insurance” is materially cheaper than most people intuit. On OpenRouter, current posted list prices for the models discussed here are:

- **DeepSeek V4 Flash:** $0.09 / $0.18 per 1M input/output tokens. citeturn25view0
- **Qwen3 Coder Next:** $0.11 / $0.80 per 1M input/output tokens. citeturn24view0
- **Qwen3-Coder-30B-A3B-Instruct:** $0.07 / $0.27 per 1M input/output tokens. citeturn24view1
- **MiniMax M3:** $0.30 / $1.20 per 1M input/output tokens. citeturn6view3
- **Kimi K2.6:** $0.66 / $3.41 per 1M input/output tokens. citeturn6view4

Under a simple 2:1 input:output assumption, a $10 monthly cap buys roughly:

- **~83M total tokens** on DeepSeek V4 Flash.
- **~29M total tokens** on Qwen3 Coder Next.
- **~73M total tokens** on Qwen3-Coder-30B-A3B-Instruct.
- **~17M total tokens** on MiniMax M3.
- **~6M total tokens** on Kimi K2.6.

Those are rough arithmetic estimates from the posted prices above; actual effective cost can be lower when repeated context benefits from prompt caching, and OpenRouter’s model pages say repeated context can reduce effective cost materially, with 60–80% savings shown in their effective-pricing notes. citeturn24view0turn24view1turn25view0turn6view3turn6view4

That creates a clean decision boundary.

**If your rule is literal and absolute — no paid API unless a human has explicitly agreed in advance — then the best architecture is Claude Max primary plus local Qwen fallback, with the paid rung physically disabled by default.** In that mode, your monthly cloud envelope remains just your Claude subscription, and continuity depends on the local hub. This is the most governance-clean design. citeturn38view0turn30view0turn30view1

**If you are willing to pre-authorize a tiny explicit insurance budget, then yes: a ~$10/month OpenRouter fallback is worth it.** It is worth it not because it should replace local, but because it dramatically improves resilience for very small money, especially if you restrict it to ultra-cheap models and use the local hub as the second rung rather than the only rung. In practice, this gives you the best overall stack:

- primary quality from Claude Max,
- zero-cloud-cost resilience from local Qwen,
- and a tiny paid continuity floor for the cases where both the subscription and the local hub disappoint. citeturn6view2turn24view0turn25view0turn13view2turn13view3

**Subscription-only ladders are viable, but I would rank them second.** MiniMax Token Plan is the strongest documented option because it is flat-priced, has a separate subscription key, and exposes an Anthropic-compatible endpoint that can slot into coding-tool workflows. I would choose it over OpenRouter only if one of two conditions is true: either you expect fallback usage to be frequent and sustained enough that a fixed monthly second subscription is operationally simpler than a tiny emergency API budget, or you want another “consumer-like” quota silo and cannot tolerate any variable billing at all. Otherwise, you are paying a fixed second bill to insure a failure mode that may only happen occasionally. citeturn22view0turn22view1turn22view2

The explicit recommendation is therefore:

**Best overall architecture:**  
native Claude Max / Claude Code as primary, a local hub-served Qwen fallback as the default resilience rung, and a separately permissioned OpenRouter micro-budget rung for emergency continuity only.

**Best local model on the 2-GPU hub:**  
start with **Qwen3.6-35B-A3B** if the hub has enough total VRAM to serve it comfortably with quantization and acceptable context; otherwise fall back to **Qwen3-Coder-30B-A3B-Instruct**. Use **vLLM** on the hub and reserve **Ollama** for simpler dev or lower-throughput use. Do **not** make MiniMax-M2.7 your first local target unless your hub is much larger than a typical 2-GPU personal box, because its official weight size puts it in a different operating class. citeturn30view0turn31view2turn30view1turn13view2turn13view3turn30view2

**Best answer to “is ~$10/mo OpenRouter fallback worth it?”**  
Yes, **if and only if** you treat it as a formally approved insurance rung with a dedicated capped key, not as an ad hoc raw API key floating in your shell. If you will not pre-approve that budget, then the right answer is local-first and fail-closed after that. citeturn26view3turn26view1turn7search11turn6view2
