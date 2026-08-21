---
dr_id: DR26-06-29-LEG-01
title: "Cross-vendor LLM agent collaboration on a shared markdown vault (rules, consensus, concurr"
date: 2026-06-29
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-29-LEG-01): Cross-vendor LLM agent collaboration on a shared markdown vault (rules, consensus, concurrency, protocols, governance)

> How can multiple AI coding agents (Claude Code, OpenAI Agents/Codex, Gemini CLI, Grok) safely collaborate on the same markdown vault/repo, and does multi-vendor review actually improve output quality?

## Ключевые выводы
- Industry is converging on a single master rules file: AGENTS.md is the emerging open standard (OpenAI, Google, GitHub Copilot), while Claude Code still looks for CLAUDE.md and Gemini CLI for GEMINI.md — recommended fix is `@AGENTS.md` as the sole line in each vendor file (or symlink) for DRY single-source-of-truth.
- Claude Code currently ignores AGENTS.md unless explicitly pointed at it, and context compaction can cause it to 'forget' earlier instructions in long sessions — instructions can drift if not actively kept in view.
- Heterogeneous model pairs catch more defects than same-model pairs: in a 27-day Claude+Codex project, cross-review found issues 69.8% of the time vs 53.1% when a model reviewed its own output.
- Structured multi-agent debate/ensemble improves accuracy modestly on hard tasks (~1-4% over majority voting, e.g. 78%→82% in one benchmark), with diminishing returns beyond ~7 agents; on easy tasks simple majority voting is nearly as good as debate.
- Multi-agent systems consume 10-100x more tokens/API calls than single-agent, and risk sycophancy/'disagreement collapse' where agents converge on confident-but-wrong consensus (one study found 18-47% error rates under agreement-based stopping).
- Best-practice concurrency control is git-based isolation, not real-time sync: each agent works in its own git worktree/branch/clone, changes are merged via standard git merge/rebase, and conflicts are resolved by humans or a dedicated 'resolve conflicts' step (e.g. the 'Tap' framework runs Claude and Codex in separate worktrees exchanging via PRs).
- Cross-vendor messaging lacks a mature unified standard; current approaches include file-based markdown+YAML 'inbox' messaging (Tap), Google's Agent2Agent (A2A, JSON-RPC over HTTP/SSE), and Zed's Agent Client Protocol (ACP, JSON-RPC over stdin/stdout) — all should tag messages with agent identity/task ID to prevent feedback loops.
- Governance requires a human-locked 'constitution' file that no agent may edit (branch protection/hooks), full git-commit audit trails tagged by agent identity, and human-in-the-loop gating for high-impact or rule-related merges.
- Cost/quality routing recommendation: use the cheapest competent model first, escalate to stronger models only for hard tasks, and add a new vendor/model only if it demonstrably fills a gap (e.g. >5-10% improvement) since returns diminish fast past 3-7 agents.

## Рекомендации / решения
- Consolidate all agent instructions into one master AGENTS.md and make CLAUDE.md/GEMINI.md/etc. single-line includes (`@AGENTS.md`) or symlinks; verify each agent actually loads it with a dummy test rule.
- Isolate every agent's work in its own git worktree/branch/clone — never let two agents write to the same working directory simultaneously; merge via standard git workflow with human or agent-assisted conflict resolution.
- Pilot a 2-agent loop first (e.g. Claude + Codex: one generates, the other critiques/revises) and measure bug-detection gains vs single-agent baseline before adding more vendors.
- Lock the canonical rules/constitution file via branch protection so no agent can edit it; require human sign-off on any rule changes and tag all agent commits with agent identity for audit trails.
- Adopt a file-based cross-vendor protocol (markdown + YAML metadata in a shared inbox folder, à la Tap) for portability across harnesses without needing network APIs; include task/round IDs to prevent infinite review loops.
- Cap ensemble size at ~2-5 complementary models; add a third/fourth model only if it improves defect detection meaningfully, and use anti-sycophancy prompting (explicit devil's-advocate instructions) or a judge/verifier role to curb false consensus.
- Implement fail-safes: max review-round limits (e.g. 3), loop/cycle detection, and escalation to a human reviewer when confidence stays low or consensus isn't reached.
- Track token usage, defects found per round, and time per agent to know when to stop adding models (plateauing marginal benefit).

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, Google, xAI, GitHub, Zed, Microsoft, Meta, LangChain
- **Продукты/инструменты:** Claude Code, CLAUDE.md, AGENTS.md, Gemini CLI, GEMINI.md, Grok Build, GitHub Copilot, Agent2Agent (A2A), Agent Client Protocol (ACP), Tap framework, git worktree, AutoGen, LangGraph, Google ADK, Yjs (CRDT), Redis streams, .mcp.json, Codex config.toml

## Открытые вопросы
- No unified cross-vendor communication standard exists yet — A2A and ACP are early steps but not fully mature or widely adopted.
- Unclear which orchestration framework (LangChain/LangGraph, AutoGen, Microsoft Agent Framework, Google ADK) is the right long-term choice.
- Exact threshold where debate/ensemble gains plateau (3-7 agents) is task-dependent and needs empirical measurement on Anton's own tasks rather than assumed from benchmarks.
- How to reliably prevent Claude Code from ignoring AGENTS.md / losing instructions after context compaction remains unresolved beyond 'keep instructions in view'.
- Real-time CRDT-based collaboration (vs git-based file/branch workflow) for code vaults is technically possible but unproven/underused in this context.

## Источник
- DR-ID `DR26-06-29-LEG-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- multi-agent-offer-reflex
- multi-agent-role-discipline
- one-system-propagate
- verify-existing-before-proposing
- test-after-build-skill
- ak47-simplicity
- insight-DR-DR26-06-28-HUB-01-architecture-for-multi-llm-agents-claude-codex-gem — соседний DR (через день) на ту же тему мульти-агентной коллаборации над общим vault
- insight-2026-08-16-codex-claude-one-vault-delta — сирота дословно цитирует статистику этого DR (69.8% vs 53.1% дефектов)
