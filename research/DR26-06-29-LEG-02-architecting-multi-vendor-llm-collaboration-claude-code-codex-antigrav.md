---
dr_id: DR26-06-29-LEG-02
title: "Architecting multi-vendor LLM collaboration (Claude Code, Codex, Antigravity, Grok) over a"
date: 2026-06-29
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-29-LEG-02): Architecting multi-vendor LLM collaboration (Claude Code, Codex, Antigravity, Grok) over a shared Obsidian vault + Git repo

> How can Claude Code, OpenAI Codex, Google Antigravity, and xAI Grok collaborate reliably on the same local Git/Obsidian vault, each running in its own native harness/subscription, without API abstraction layers or shared runtimes?

## Ключевые выводы
- Heterogeneous review pairings (e.g. Claude reviewing Codex output) caught defects/requested changes in 69.8% of cases vs only 53.1% for homogeneous pairings (same-model reviewing itself), based on a 27-day/37-generation self-applied study of 375 review artifacts (the 'tap' operational study).
- Homogeneous Multi-Agent Debate (MAD) is theoretically a martingale over agents' beliefs — correlated error distributions and shared latent representations mean debate preserves rather than improves correctness, causing groupthink collapse into an initial (often wrong) majority.
- Generator-Validator (advocate/skeptic) topology with a different vendor as critic yields up to 13% accuracy gains on reasoning benchmarks by preventing error cascades; Adaptive Heterogeneous MAD (A-HMAD) reaches 96.5% on GSM8K by treating debate as conditional computation.
- Each vendor has its own rule-loading convention (Claude Code: CLAUDE.md + .claude/rules/, @import up to 5 levels; Codex: AGENTS.md, 32 KiB cap; Antigravity: .agents/agents.md + skills, deprecated GEMINI.md; Grok Build: config.toml, natively reads CLAUDE.md/AGENTS.md) — without a compiled single source of truth, rule drift across harnesses is inevitable.
- Models silently ignore rules past ~200 lines ('lost in the middle') and arbitrate contradictory layered rules randomly, producing non-deterministic failures — root files must stay small with domain rules pushed to path-scoped files loaded only on matching paths/skills.
- Git worktrees isolate filesystem-level edits (near-zero overhead, natively supported via Claude Code's --worktree flag) but do NOT solve git index.lock contention (a crashed process holding the lock freezes all agents) nor semantic/logic conflicts across worktrees that merge cleanly but break the build.
- Advisory Concurrency (e.g. MTPO in CoAgent) is proposed as superior to strict 2PL locking or OCC for long-running agent tasks: agents run optimistically, get a one-way notification on conflicting writes, and the LLM semantically repairs only affected work rather than blocking or discarding everything.
- Among exchange protocols, MCP (Anthropic) is tool/context-oriented not agent-to-agent; A2A (Google/Linux Foundation) is HTTP/SSE-based and brittle for offline local Windows setups where CLIs hang; the file-first 'tap' protocol (YAML-fronted markdown in an inbox/ folder, dual-tier file+notification) is most resilient for this environment.
- GitOfThoughts research found that injecting one agent's historical reasoning into another's context does not improve accuracy on novel problems unless retrieved task similarity exceeds cosine 0.8 — so cross-vendor exchange should carry finalized artifacts/patches/critiques, not full chains of thought.
- Proposed governance model is a 3-tier Coordinator (decomposes, writes spec, never codes) / Specialist (restricted to its worktree, barred from editing root config) / Verifier (must be a different model than the Specialist to preserve the heterogeneous advantage) hierarchy, with critical files made read-only or protected by pre-commit hooks.

## Рекомендации / решения
- Define a master rule manifest (e.g. agent.yaml/RULES.md) and use a pre-commit hook/build script to compile it into each vendor's native format (CLAUDE.md, AGENTS.md, .agents/agents.md), stripping duplicate/conflicting directives before generation.
- Keep root rule files under ~200 lines; push domain/subsystem-specific rules into path-scoped files (.claude/rules/, .agents/skills/) loaded only when relevant paths/skills are touched.
- Hardcode heterogeneous review: never let a model review its own vendor's output for final verification — always route Codex output to Claude/Grok review and vice versa.
- Adopt the tap Tier-1 file-based protocol (inbox/ with YAML frontmatter: task_originator/target_agent/status/payload/provenance_chain/action_required) instead of synchronous A2A/HTTP callbacks, to survive harness hangs on Windows.
- Isolate every discrete task in its own Git worktree, and layer in an advisory-concurrency script (CoAgent/MTPO-inspired) that drops a WARNING.md into an active worktree when a shared dependency changes, letting the agent self-repair instead of hard-aborting.
- Enforce human-in-the-loop merge gates: agents push to a review/ state; a Tier-3 Verifier (always a different vendor) or a human approves before merge into main; protect constitution/RULES.md files as read-only or via pre-commit rejection of automated edits to policy paths.
- Route tasks by vendor specialization (Grok: bulk migration/real-time data/large context; Codex: Python; Antigravity: GCP infra) rather than assigning redundant generative work to every model, since a 3rd/4th model gives diminishing returns without specialization.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, Google, xAI, Linux Foundation
- **Продукты/инструменты:** Claude Code, OpenAI Codex, Google Antigravity (formerly Gemini CLI), xAI Grok Build, Obsidian, Git, Git worktrees, CLAUDE.md, AGENTS.md, GEMINI.md, config.toml, Model Context Protocol (MCP), Agent2Agent Protocol (A2A), tap protocol, CoAgent, Monotonic Trajectory Pre-Order (MTPO), GitOfThoughts, Vibe Kanban, Nimbalyst, DynaDebate, Adaptive Heterogeneous MAD (A-HMAD), GSM8K, gitagent-protocol

## Открытые вопросы
- How exactly to implement the advisory-concurrency 'runtime informs, agent repairs' loop concretely on a local Windows filesystem (no reference implementation given, only conceptual framing via CoAgent/MTPO).
- No concrete tooling specified for detecting/resolving semantic (not just file-level) merge conflicts across worktrees.
- Whether/how the 32 KiB Codex instruction cap and Claude's 200-line 'lost in the middle' threshold should be reconciled into one shared root-file size budget across vendors.
- The tap protocol and CoAgent/MTPO are referenced as if from named studies/frameworks but no citations or links are given — provenance and reproducibility of the 69.8%/53.1% and A-HMAD 96.5% figures need verification.
- How to bootstrap the review pipeline when only two vendors are actively available in Anton's setup, versus the four-vendor ideal described.

## Источник
- DR-ID `DR26-06-29-LEG-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- multi-agent-role-discipline
- one-system-propagate
- machine-bus-telegram-rail
- consensus-active-session-marking
- verify-existing-before-proposing
- git-worktree-isolation
- heterogeneous-review-pairing
- insight-DR-DR26-07-01-ZB-01-sota-architecture-for-a-unified-local-first-second — почти дублирующий DR по той же теме мульти-вендорной коллаборации поверх волта
- insight-DR-DR26-07-28-HUB-10-2339-grok-coding-grok-4-5-grok-build-от-spacexai-возмож — обе разбирают Grok как вендора коллаборации/кодинг-агента
