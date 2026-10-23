# DR26-07-01-ZB-05 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "LLM Fallback Architecture Research"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4adc4d-c5b8-83eb-9456-6642929906b9
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-05
date_added: 2026-07-07
valid_as_of: 2026-07-05
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 3
related_concepts: []
revisit_if: ""
---

# LLM Fallback Architecture Research

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

DR26-07-01-ZB-05

Conduct a deep research investigation on the following topic.



TOPIC:

Best architecture for MODEL-FALLBACK and cost-resilience for an always-on personal agent setup that runs on SUBSCRIPTION buckets (Claude Max/Weekly) and must degrade gracefully when the primary model hits a limit — WITHOUT accidentally spending on paid API. Compare local models (Ollama/Qwen), cheap hosted fallbacks (OpenRouter Minimax, Kimi), and subscription-only ladders; and solve the fact that `claude -p` / Claude Code cannot self-switch model or provider on a limit.



CONTEXT (what exists / what was found):

- Transport/pings are already 0-LLM (OS scheduler + dumb Python detectors) and survive any cloud outage. The failure point is the THINKING step: an inbox robot already on --model sonnet hit "weekly limit" and silently failed to a log, so a peer saw "hub offline".

- Root constraint: `claude -p`/Claude Code does not itself fall back to another model/provider on a limit → an external wrapper-ladder is needed.

- An open internal DR already exists (local Ollama+Qwen, deferred pending a 16GB GPU). New options surfaced from community: a cheap second LLM (Minimax ~$10/mo, Kimi 2.6 on OpenRouter) held as fallback so one agent self-heals when the primary dies; practitioners BAN raw API keys to avoid surprise paid spend and keep only a coding-plan subscription + a cheap fallback; a solo user manually spends ≤~30% of a Claude Max 20x weekly limit/day → agent swarms are needed to actually utilize the subscription.

- Anton's rule: exhaust subscription limits before ANY paid API; paid only by explicit agreement; hub has 2 GPUs (can host local models), laptop is thin.



OBJECTIVE:

Design the fallback LADDER (primary subscription model → cheap/local fallback → when, if ever, paid API), the external wrapper that detects a limit/failure and switches, how to prevent accidental paid-API spend, and where local models fit given a 2-GPU hub.



RESEARCH REQUIREMENTS:

1. Academic — model routing/cascades, cost-aware inference, reliability/failover for LLM systems.

2. Industry practice — OpenRouter routing/fallbacks, LiteLLM proxy, LangChain fallbacks, Ollama/vLLM local serving, Minimax/Kimi/DeepSeek/Qwen cost-quality; how people wrap `claude -p` for failover.

3. Competitive landscape — local vs cheap-hosted vs multi-subscription strategies.

4. Failure analysis — silent-failure-to-log incidents, accidental paid-API bill blowups, quality cliffs when falling back to weak models, local-model reliability.

5. Emerging trends — last 12 months in cheap/local model quality + agent failover tooling.

6. Implementation recommendations — a concrete ladder + the wrapper design (detect limit/error → switch provider → fail LOUD not to a log) + hard guards against paid-API spend + which local model on the 2-GPU hub + monthly cost envelope.



OUTPUT FORMAT:

- Executive summary.

- Fallback-ladder diagram/table with cost + quality + trigger per rung.

- Wrapper design (detection + switch + fail-loud + spend-guard).

- Explicit recommendation: is the ~$10/mo OpenRouter fallback worth it vs local-only vs subscription-only? Under what condition.

- Sources.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"topic":"Best architecture for MODEL-FALLBACK and cost-resilience for an always-on personal agent setup that runs on subscription buckets (Claude Max/Weekly) and degrades gracefully without accidentally spending on paid API. Compare local Ollama/Qwen, cheap hosted fallbacks (OpenRouter Minimax, Kimi), subscription-only ladders, external wrapper around claude -p/Claude Code limitations, spend guards, wrapper design, fail-loud behavior, local models on 2-GPU hub, implementation recommendations and cost envelope.","output_format":"report"}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-01-ZB-05 Conduct a deep research investigation on the following topic: Best architecture for MODEL-FALLBACK and cost-resilience for an always-on personal agent setup that runs on subscription buckets (Claude Max/Weekly) and must degrade gracefully when the primary model hits a limit \u2014 WITHOUT accidentally spending on paid API. Compare local models (Ollama/Qwen), cheap hosted fallbacks (OpenRouter Minimax, Kimi), subscription-only ladders; design external wrapper because claude -p cannot self-switch; include academic research, industry practice, competitive landscape, failure analysis, emerging trends, implementation recommendations; output executive summary, ladder, wrapper, recommendation, sources."}}
```
