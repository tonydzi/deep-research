---
dr_id: DR26-07-28-HUB-20-2339
title: "Fallback architecture for a subscription-only always-on Claude agent hitting rate limits"
date: 2026-07-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-20-2339): Fallback architecture for a subscription-only always-on Claude agent hitting rate limits

> Deep research on how to design a model-fallback ladder (subscription → local → optional cheap paid API) so an always-on personal agent degrades gracefully when Claude Max hits its weekly limit without ever silently failing or accidentally spending on paid API.

## Ключевые выводы
- Root cause confirmed: `claude -p`/Claude Code has no cross-provider self-fallback — its `fallbackModel` setting only covers same-account availability failures, and activating any gateway credential replaces subscription billing entirely (no 'subscription-first, spill to paid API' inside one authenticated session).
- Academic routing/cascade research (Hybrid LLM, RouteLLM, 2026 survey) shows learned routing/cascading with quality estimation can cut calls to the big model up to 40% or cut cost >2x without quality loss — the right mental model is routing+cascading with an explicit quality-estimation signal, not blind retries.
- Recommended 3-rung ladder: (1) native Claude Max/Claude Code as primary with zero paid credentials in env; (2) local hub-served Qwen model (Qwen3.6-35B-A3B or Qwen3-Coder-30B-A3B-Instruct via vLLM) as zero-cloud-cost resilience rung; (3) optional, disabled-by-default OpenRouter micro-budget rung with a dedicated capped key and strict cheap-model allowlist.
- Open-weight models have closed much of the gap in the last 12 months: Kimi K2.6 (Intelligence Index 54, Apr 2026) and MiniMax M3 (55, Jun 2026) lead open-weights; Qwen3-Coder and Qwen3.6-35B-A3B are positioned as agentic-coding-capable with small active-parameter counts, making local fallback newly viable though still weaker than Claude on long messy coding loops.
- MiniMax-M2.7 (229B params) is the wrong first local target for a 2-GPU hub — total weight size dominates local serving feasibility; Qwen 30B/35B-class models are the sane choice for local (vLLM for the hub service, Ollama only for dev convenience).
- OpenRouter's Claude Code integration is only guaranteed with the Anthropic first-party provider — routing non-Anthropic models (Qwen/Kimi/DeepSeek/MiniMax) through Claude Code itself is unreliable, so the design needs a separate runner abstraction per rung instead of one unified Claude Code process.
- Cost math at $10/month cap (2:1 input:output): ~83M tokens on DeepSeek V4 Flash, ~73M on Qwen3-Coder-30B-A3B-Instruct, ~29M on Qwen3 Coder Next, ~17M on MiniMax M3, ~6M on Kimi K2.6 — a small pre-authorized budget buys substantial emergency continuity.
- The 'silent fail to log' incident is an integration bug, not a platform inevitability: Anthropic's Agent SDK exposes `is_error` results and non-zero exit, and Claude Code hooks expose a typed `StopFailure` event (`rate_limit`, `billing_error`, `authentication_failed`, `overloaded`, `server_error`) meant for recovery/alerting.
- Spend-guard design requires a dedicated, separately-named, separately-capped paid API key with a strict model/provider allowlist (no frontier models, no BYOK) — sharing the everyday API key with the fallback wrapper is the main accidental-spend risk; OpenRouter's 'zero completion insurance' helps but is not sufficient alone.
- Fallback must change policy, not just model: narrower prompt, reduced action scope, required structured JSON output, and validation before commit — switching only the model while keeping full permissions risks a silent quality cliff on agentic tasks.

## Рекомендации / решения
- Adopt the 3-rung ladder: Claude Max/Claude Code primary → local Qwen (vLLM on the 2-GPU hub) as default resilience rung → OpenRouter micro-budget rung (~$10/mo cap) enabled only by explicit policy, never as raw API keys floating in the shell.
- Build a supervisor state machine (not a shell alias) with three runner profiles (claude_subscription / local_qwen / paid_openrouter) that classifies failures via SDK exceptions/`is_error`/`StopFailure`, switches profile AND policy (tighter prompt, reduced action scope, JSON validation), and always fails loud (alert + `degraded`/`fallback_level` metadata) rather than to a log.
- If the paid rung is authorized, restrict it to a dedicated capped OpenRouter key with an allowlist of Qwen3 Coder Next / DeepSeek V4 Flash / MiniMax M3 (Kimi K2.6 only if coding quality justifies its higher cost), forbid Anthropic/OpenAI/Google models on that key, and require a mandatory alert on every paid invocation.
- For the local rung, deploy Qwen3.6-35B-A3B (or Qwen3-Coder-30B-A3B-Instruct if VRAM-constrained) via vLLM on the hub; use Ollama only for lower-throughput dev/testing, not as the production fallback service.
- If literal 'no paid API ever without prior human approval' is the hard rule, skip the paid rung entirely and run Claude Max + local Qwen only — cleanest from a governance standpoint, cloud spend stays fixed at the subscription price.
- Treat LiteLLM's `budget_fallbacks` and similar transparent-reroute features with caution — they conflict with the 'fail loud' requirement unless wrapped with your own explicit notification layer.
- Do not attempt to route non-Anthropic fallback models through Claude Code itself (unsupported/unreliable) — use a generic OpenAI-compatible runner for both the local and paid rungs.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenRouter, MiniMax, DeepSeek, Qwen/Alibaba, Moonshot AI (Kimi), LiteLLM, LangChain, Ollama, vLLM, Artificial Analysis
- **Продукты/инструменты:** Claude Code, Claude Max/Weekly, claude -p, fallbackModel setting, StopFailure hook, Agent SDK, OpenRouter, LiteLLM proxy, LangChain fallback middleware, Ollama, vLLM, Qwen3.6-35B-A3B, Qwen3-Coder-30B-A3B-Instruct, Qwen3 Coder Next, DeepSeek V4 Flash, MiniMax M3, MiniMax M2.7, MiniMax Token Plan, Kimi K2.6, RouteLLM, Hybrid LLM

## Открытые вопросы
- Actual VRAM/GPU specs of the 2-GPU hub were not confirmed — determines whether Qwen3.6-35B-A3B fits comfortably or requires quantization/offload.
- Whether Anton will pre-authorize the ~$10/mo OpenRouter insurance budget at all (report frames this as conditional, not decided).
- No live implementation was verified in this DR — the wrapper/supervisor, spend-guard key, and local vLLM deployment are design recommendations only, not built/tested artifacts.
- Relationship to the already-open internal DR on local Ollama+Qwen (deferred pending 16GB GPU) — whether this report supersedes or merges with that prior thread was not resolved.

## Источник
- DR-ID `DR26-07-28-HUB-20-2339` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-28-HUB-20-2339-model-fallback-architecture-chatgpt.md`
- оригинал: `E:\Obsidian\Anton-Knowledge\01-Conversations\ChatGPT\conversations\2026-07-05-model-fallback-architecture-6a49968f.md`

## Связано
- [[model-routing-fable-smart]]
- [[model-routing-sonnet-grunt]]
- [[prefer-included-limits-before-paid-api]]
- [[node-doctrine-thin-anchor]]
- [[long-task-healthcheck]]
- [[credential-store]]
