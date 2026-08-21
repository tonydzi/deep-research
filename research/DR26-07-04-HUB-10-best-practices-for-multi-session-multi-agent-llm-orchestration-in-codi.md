---
dr_id: DR26-07-04-HUB-10
title: "Best practices for multi-session/multi-agent LLM orchestration in coding: subagent vs sess"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-10): Best practices for multi-session/multi-agent LLM orchestration in coding: subagent vs session vs workflow

> Synthesizes Anthropic/Cognition/OpenAI/LangChain/Cursor/Codex/Aider evidence to define when to use an in-session subagent vs. a separate session vs. a scripted workflow for coding work, and how to design seed prompts, coordination, and verification.

## Ключевые выводы
- Low-coordination architecture wins: one lead session keeping the main thread plus a few isolated worker sessions, each with a clear deliverable, bounded file surface, and explicit verification target — not a free-for-all swarm.
- Coding tasks are a worse fit for multi-agent parallelism than breadth-first research: Anthropic's multi-agent research system beat single-model by 90.2% on decomposable search but at ~15x token cost of chat; coding has fewer truly parallelizable subproblems and higher coordination demands.
- Under matched reasoning-token budgets, single agents often match or beat multi-agent systems on multi-hop reasoning — many reported multi-agent gains are just extra compute, not orchestration magic.
- Homogeneous agent teams degrade past 2-3 members (AgentGroupChat-V2: conventional AutoGen/debate setups peak at 2-3 agents then decline); specialized-role teams can improve up to 5 agents.
- Human supervision bandwidth caps around 3-5 concurrent coding sessions before context-switching overwhelms gains (OpenAI Symphony finding).
- Cold-start hand-off failures are dominated by context allocation, not prompt eloquence: PerspectiveGap benchmark shows models leak distractors, drop shared context, confuse artifact ownership, and hide instructions in the wrong place when composing sub-agent prompts.
- SWE-ContextBench: well-summarized correctly-retrieved prior context helps accuracy; unfiltered or wrong context hurts both accuracy and cost — do not dump the whole repo/conversation into a worker.
- Best verification pattern: independent review in a FRESH context plus hard end-state gates; implementer should never self-certify 'done' — Cognition's review loop finds ~2 bugs/PR, majority severe; Claude Code teams can hook-block completion.
- Failure modes ranked: (1) parallel writers making incompatible implicit decisions even across different files, (2) context poisoning at handoff, (3) over-spawning agents (diminishing/negative returns), (4) false completion (status lag, self-declared done), (5) environment mismatch degrading verification quality.
- Market splits into interaction tools for a human supervisor (Claude Code, Cursor, Aider, Codex, Devin) vs. programmable orchestration frameworks (LangGraph, AutoGen, CrewAI, OpenAI Agents SDK) — solo founders should default to the first unless there's a repeatable high-volume process worth codifying as a workflow.

## Рекомендации / решения
- Use an in-session subagent when the task is read-heavy, compressible to a summary, and doesn't need durable ownership (log/doc/code search, test triage, narrow review).
- Use a separate session when the task has its own deliverable, bounded file surface, and can be seeded self-contained with a testable done-state.
- Use a scripted workflow only when work exceeds ~15 independent chunks, needs more than 3 writable followers, or requires repeatable fan-out/retries/cross-checking.
- Cap at 3 active writable follower sessions plus at most 2 read-only/review followers at any time.
- Seed prompts must include: goal, current state, relevant files/docs, constraints and already-made decisions, non-goals, verification commands, deliverable location, done-means, escalation rail — compact, need-only, no full transcript dumps.
- Default to isolation for writers (separate worktrees, never concurrent same-file writes) and visibility/shared artifacts only for reviewers/researchers.
- Treat any out-of-scope discovery mid-flight as a new tracked ticket, not absorbed work in the same session (mirrors OpenAI Symphony's 'file a new issue' pattern) — this is the drift-offload mechanism.
- Gate 'done' behind: tests pass, diff reviewed, requirements matched, no out-of-scope file changes, plus runtime evidence (screenshots/logs) for UI or behaviorally complex changes; implementer may only say 'ready for review'.
- Track every spawned session as a work item with owner, deliverable, file scope, started-at, deadline, blocked-by, next-check-in rather than letting it be an orphaned chat window.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Cognition, OpenAI, LangChain, Cursor, Devin, Windsurf, Aider, CrewAI, AutoGen
- **Продукты/инструменты:** Claude Code, Claude Research, Claude agent teams, Codex, Symphony, LangGraph, OpenAI Agents SDK/Swarm, AgentForge, AgentGroupChat-V2, PerspectiveGap, SWE-ContextBench, SWE-EVO, SWE-Chain, MASEval

## Открытые вопросы
- No universal crossover formula exists for exactly when multi-agent orchestration becomes net-positive for coding — evidence is piecemeal (coupling, homogeneous team degradation past 2-3, human bandwidth of 3-5) rather than a precise constant.
- Limited public research isolates coding-specific cold-start hand-off quality as its own measurable variable.
- Whether special-purpose multi-agent systems are genuinely better or just single agents with more compute in disguise remains contested (Anthropic's research system vs. the matched-token-budget study).
- Debate unresolved between generic orchestration patterns (easier to adopt) vs. domain-specific/vertical architectures (often outperform per LangChain's benchmark) — no settled guidance on which to default to.

## Источник
- DR-ID `DR26-07-04-HUB-10` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\DR26-07-04-HUB-10-multi-session-orchestration.md`

## Связано
- [[multi-agent-offer-reflex]]
- [[decompose-into-parallel-sessions]]
- [[goal-drift-offload-to-seed-sessions]]
- [[multi-agent-role-discipline]]
- [[test-after-build-skill]]
- [[verify-existing-before-proposing]]
- [[machine-governance-leader-follower]]
- [[cowork-vs-cc-division]]
- [[insight-DR-DR26-07-04-HUB-11-multi-session-and-multi-agent-llm-orchestration-fo]] — сиблинг-DR из той же серии по идентичной теме
