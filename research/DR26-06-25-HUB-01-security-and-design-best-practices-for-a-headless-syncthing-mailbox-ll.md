---
dr_id: DR26-06-25-HUB-01
title: "Security and design best practices for a headless Syncthing-mailbox LLM agent"
date: 2026-06-25
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-25-HUB-01): Security and design best practices for a headless Syncthing-mailbox LLM agent

> Whether an LLM GREEN/RED classifier is a safe authorization boundary for a headless agent that reads tasks from a shared Syncthing mailbox and auto-executes them, and what architecture should replace it if not.

## Ключевые выводы
- Prompt injection is a core, architectural risk for any tool-using agent that consumes attacker-influenced text/files/peer messages, not a corner case; OpenAI, Anthropic, OWASP and academic benchmarks converge on layered defense (least privilege, tool-level approvals, deterministic validation, sandboxing, human review), none treating an LLM classifier alone as sufficient.
- Anthropic's own Claude Code auto-mode classifier still had a 17% false-negative rate on real overeager actions in production, and Anthropic itself says manual approval can be safer for high-stakes cases.
- CAPTURE found guardrail models can hit 100% false-negative or 100% false-positive rates in some domains; Prompt Overflow bypassed a detector family >99.5% of the time via fragmented/interleaved placement; a 2026 evaluation of 20,000+ adaptive attacks found every self-protecting model defense eventually failed, with only hardcoded output filtering in separate application code holding up.
- WASP showed realistic prompt injections partially hijacked web agents in up to 86% of cases; full attacker-goal completion was lower only due to current agent incompetence, which the authors warn is a fragile, temporary protection ('security by incompetence').
- Given the user's own dry-run distribution (GREEN=0, RED=113, AUTH=95), the auto-execute carve-out currently provides essentially zero realized operational value while carrying full attack surface.
- Syncthing has nontrivial distributed-systems hazards for a 'folder as queue' design: temp files during incomplete transfers, sync-conflict files propagated as ordinary files, and unpredictable 'who wins' outcomes when not fully in sync — enabling duplicate execution, poison-pill tasks, or ack loops.
- Major agent frameworks (OpenAI Agents SDK/Codex, LangGraph, CrewAI, Anthropic Claude Code) all keep orchestration, approvals, and sandbox policy in application-owned code, not in the model; Codex defaults to network-off and requires approval for destructive/side-effecting calls.
- Even prior to LLM classifiers, AutoGPT's history shows approval surfaces themselves can be attacked (injected console output manipulating the review path) and had sandbox-escape issues — approval alone isn't safe if the approved payload is opaque or decoupled from what actually executes.
- Newer authorization-propagation research argues prompt-injection defense is not the whole problem: multi-agent systems need machine-verifiable scope, time bounds, and one-time-use constraints, since a signed authorization from one interactive session should not become ambient permission for a headless daemon later.

## Рекомендации / решения
- Turn off bus-driven auto-execution now and run the mailbox robot in PLAN-only mode until real GREEN volume justifies the complexity; keep the planner, human ping, audit trail, and AUTH concept for interactive sessions.
- Treat GREEN/RED as advisory only; make the authoritative gate a deterministic rule table (fixed action enum + argument schema validation + path allow-lists), with outbound comms/deletion/config changes/secrets/installs always requiring human approval.
- If automation is reintroduced, replace natural-language mailbox items with immutable, signed JSON task envelopes (task_id, issuer/target machine, timestamps, action_type, args, args_hash, dedupe_key, Ed25519 signature); producers write-once, consumers never edit shared files.
- Split the bus into append-only per-writer namespaces (tasks/<target>/, plans/<writer>/, receipts/<writer>/, authorizations/<writer>/) so consumers only append receipts to their own namespace and never rename/edit shared task files.
- Replace the free-text AUTH block with a cryptographically bound, single-use, short-lived authorization token minted in an interactive human session, bound to exact task_id/action_type/args_hash/target machine, with replay protection.
- Harden the host: dedicated service account, minimal writable filesystem, protected/policy files mounted read-only, no access to general secrets, restricted egress, resource quotas — OS-enforced sandboxing, not just prompt-enforced hooks.
- Define explicit go/no-go criteria before ever re-enabling auto-exec: sustained real GREEN volume, dedicated deterministic wrappers per action, replay-safety by construction, narrow fixed schemas, no general shell/ambient secrets, sandboxed host, and passed red-team testing against injection/replay/duplicate delivery/malformed envelopes.

## Сущности
- **Люди:** —
- **Компании:** OpenAI, Anthropic, OWASP, Microsoft, NIST, Unit 42
- **Продукты/инструменты:** Syncthing, Claude Code, MCP (Model Context Protocol), LangGraph/LangChain, CrewAI, Codex, AutoGPT, AgentDojo, AgentHarm, WASP, Imprompter, AgentXploit, ARGUS/AgentLure, CAPTURE, OS-Harm, Prompt Overflow, SQS, Stripe, NIST SP 800-207

## Открытые вопросы
- No existing study quantifies risk for the exact combination of a Syncthing-synced mailbox plus a scheduled LLM agent (e.g. 'GPT scheduled every 20 minutes') — conclusions are engineering inferences from adjacent agent-security research, not a direct empirical measurement of this setup.
- Whether a narrow whitelist of actions can ever be safely inferred by the LLM itself (vs. purely software-enforced) remains contested; design-pattern literature recommends action-selector patterns but warns fuzzy classifier-based authorization can be gamed by combining benign and malicious intents.
- Unresolved how much real GREEN-classified volume would need to materialize before automation is worth reintroducing, and what red-team test suite would count as 'passed' for the go/no-go criteria.

## Источник
- DR-ID `DR26-06-25-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-25-HUB-01-security-and-design-best-practices-for-a-head.md`

## Связано
- [[machine-bus-telegram-rail]]
- [[consensus-active-session-marking]]
- [[remote-approval-qqq]]
- [[credential-store]]
- [[deterministic-script-gotchas]]
- [[verify-existing-before-proposing]]
- [[multi-agent-role-discipline]]
