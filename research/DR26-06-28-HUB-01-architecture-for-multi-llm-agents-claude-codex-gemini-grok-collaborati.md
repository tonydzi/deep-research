---
dr_id: DR26-06-28-HUB-01
title: "Architecture for multi-LLM agents (Claude, Codex, Gemini, Grok) collaborating on a shared"
date: 2026-06-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-28-HUB-01): Architecture for multi-LLM agents (Claude, Codex, Gemini, Grok) collaborating on a shared vault via roles and consensus

> How to design a system where multiple LLM agents (Reporter, Analyst, Reviewer, Orchestrator) collaboratively read/write a shared knowledge vault, reach consensus on facts, and stay governed by rules and monitoring.

## Ключевые выводы
- Multi-LLM systems with debate/critique rounds outperform single models on complex tasks by catching each other's hallucinations and errors.
- Three participating agents is cited as a 'sweet spot' balancing diversity of perspective against coordination cost.
- Role-based static pipeline (Reporter → Analyst → Reviewer) is simpler but less flexible than a dynamic orchestrator agent that reads the task and spawns/dispatches sub-agents via a DAG; a hybrid (fixed core roles + orchestrator for routing) is recommended for robustness.
- The Vault should function like a governed knowledge store (HashiCorp Vault-style): version-controlled, access-controlled per agent identity, all changes logged, with an embedding index for semantic retrieval.
- Agents should get ephemeral tokens/minimal credentials from the Vault rather than holding standing API keys, to avoid 'secret sprawl' and enforce least privilege.
- Consensus mechanisms proposed: unanimous approval for critical facts, majority vote otherwise, or a judge agent; unresolved disagreement after N rounds escalates to a human/arbitration agent.
- Runtime guardrails (GuardAgent pattern) should intercept agent actions before commit — checking citation presence, content policy, schema — blocking or requesting self-correction on violation.
- Evaluation should track accuracy/factuality, internal consistency (contradiction linting), citation completeness, and use a smaller/cheaper 'judge' model to score outputs at scale, supplemented by periodic human review.
- Observability should use distributed tracing (OpenTelemetry-style) per agent turn, visualized as a graph, plus a metrics dashboard (e.g. Galileo/Fiddler-style) tracking compliance and consensus rates over time.
- A 10-step implementation plan is given: audit vault/access → integrate LLM APIs → define role prompts → prototype Reporter/Analyst/Reviewer workflow → pick coordination architecture → build consensus protocol (2-3 debate rounds or voting) → add guardrails → set up monitoring/tracing → continuously evaluate and fine-tune on corrected cases → scale to more models (Gemini, Grok).

## Рекомендации / решения
- Start with a static sequential role pipeline (Reporter → Analyst → Reviewer) before building a full dynamic orchestrator, and keep a fallback default chain if the orchestrator path fails.
- Require unanimous agreement for critical/factual claims; escalate to human review only after failing to converge within a fixed number of debate rounds (e.g. 2-3).
- Give each model role-specific prompts matching its strength: code-style/correctness rules for Codex, citation/context-consistency rules for Claude-type text models.
- Implement a GuardAgent-style interception layer on every agent action before it's committed to the Vault (citation check, policy compliance, schema validation).
- Use cheap/small evaluator models to score every output for compliance/quality rather than relying only on expensive human review.
- Log every Vault edit with agent identity and rationale for auditability, and continuously feed human-corrected cases back into fine-tuning data.
- Scale to additional models (Gemini, Grok) only after the core role/consensus/guardrail loop is proven stable with the initial set.

## Сущности
- **Люди:** —
- **Компании:** —
- **Продукты/инструменты:** Claude, Codex, Gemini, Grok, LangChain, AutoGen, HashiCorp Vault, GuardAgent, OpenTelemetry, Galileo, Fiddler, HumanEval

## Открытые вопросы
- No concrete benchmark data given for 'three agents is a sweet spot' or debate-round convergence rates — cited as general empirical claims without specific studies named.
- Exact mechanism for fine-tuning/RLHF on Vault-corrected edits is described only at a high level, not a working pipeline.
- How to weight/combine judge-model scores vs human review in ambiguous or regulated domains is not resolved.
- The report does not specify how conflicts between two agents' simultaneous Vault writes (race conditions) are resolved beyond general 'locked sections' mention.

## Источник
- DR-ID `DR26-06-28-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-28-HUB-01-multi-llm-collaboration-on-a-shared-vault.md`

## Связано
- [[vault-data-architecture]]
- [[machine-governance-leader-follower]]
- [[multi-agent-role-discipline]]
- [[multi-machine-auto-consensus]]
- [[one-system-propagate]]
- [[verify-existing-before-proposing]]
