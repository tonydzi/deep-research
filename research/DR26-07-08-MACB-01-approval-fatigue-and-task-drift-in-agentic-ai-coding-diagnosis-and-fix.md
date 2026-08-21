---
dr_id: DR26-07-08-MACB-01
title: "Approval fatigue and task drift in agentic AI coding — diagnosis and fixes"
date: 2026-07-08
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-08-MACB-01): Approval fatigue and task drift in agentic AI coding — diagnosis and fixes

> The report investigates why AI-coding agents cause approval fatigue and goal/session drift, and surveys what evidence-backed fixes Anthropic, OpenAI, Cursor, GitHub Copilot, Windsurf, Devin and Aider actually use in 2025–2026.

## Ключевые выводы
- Anthropic reports Claude Code users approve ~93% of permission prompts — a rate high enough that human review has stopped being meaningful supervision (rubber-stamping, not review).
- Approval fatigue is best explained by alert/warning-fatigue and habituation research (medicine, security, privacy-consent), not by generic 'decision fatigue': a 2025 healthcare systematic review found significant support in only 45% of reviewed cases, and a 2025 field study found no evidence of decision fatigue in one real triage setting.
- Habituation from frequent low-value prompts can generalize to critical warnings when both share the same UI gesture — meaning if trivial file edits and dangerous shell commands trigger identical approval clicks, users learn one reflex for both.
- Session/task drift is fundamentally a context-architecture problem: the agent's effective goal becomes the sum of the original prompt plus tool outputs, corrections, and stale intermediate state, not just the initial instruction.
- Academic work formalizes this as 'goal drift' (2025 AIES paper: agents gradually diverge from objectives under competing pressures) and 'Inherited Goal Drift' (2026 paper: even strong models inherit off-target behavior from weaker/already-drifting trajectories/session history).
- The dominant, convergent product fix is NOT more confirmations but bounded autonomy: sandboxing + allowlists + classifier/auto-review handling routine approvals (Claude Code auto mode, OpenAI Codex sandbox/approval-policy separation with Auto-review).
- Plan-first execution with an external Markdown artifact, followed by a fresh implementation session, is now a standard pattern across Claude Code, Cursor (Plan Mode), GitHub Copilot (Plan mode), and Windsurf (~/.windsurf/plans) — it measurably reduces both drift and rework.
- External verifiable success criteria (tests, lints, builds, screenshots) are needed to prevent 'looks done' false confidence; without them the human becomes the only pass/fail loop.
- Cheap 'session maps' (Claude /compact, Copilot /chronicle standup, Codex /compact + /fork + /side, Devin's Progress tab, Windsurf stored plan files) are the correct recovery mechanism for long sessions — not rereading full transcripts.
- Explicit hard-reset thresholds are recommended by multiple vendors: start a fresh session/thread when the task changes, the agent gets confused, or it has been corrected twice on the same issue; fork to a worktree for bifurcated/competing work instead of continuing in one thread.

## Рекомендации / решения
- Replace click-by-click supervision with bounded autonomy: sandbox + allowlist + auto-review/classifier handling routine approvals, reserving human attention for true boundary-crossing actions (confidence: high).
- Force a spec/plan-first step (external Markdown artifact: goals/non-goals, files touched, acceptance tests, rollback point) for any multi-file or judgment-heavy task, then implement in a fresh session against that artifact (confidence: high).
- Keep one short, versioned persistent-context file (CLAUDE.md / AGENTS.md / rules) — promote a correction into it only after it's been repeated twice; keep long/conditional guidance out of always-loaded context.
- Adopt a personal reset/split rule set: same feature → same thread; new feature → new thread; side investigation → side thread/worktree; repeated confusion → rewind or restart; competing approaches → parallel worktrees + diff comparison.
- At the end of every long run, demand a compact session map (state summary: attempted/changed/passed/failed/remaining/plan location) rather than relying on rereading the transcript.
- Avoid generic 'are you sure?' confirmations for frequent reversible actions (use undo instead), avoid full bypass/dangerous-permission modes as an everyday default, and avoid letting the same agent/context review its own work — use a fresh-context reviewer or subagent instead.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, GitHub, Cursor, Windsurf, Cognition (Devin), Aider, AHRQ, Nielsen Norman Group
- **Продукты/инструменты:** Claude Code, Codex, GitHub Copilot, Cursor Plan Mode, Windsurf Plan Mode, Devin Desktop, Aider, CLAUDE.md, AGENTS.md, /compact, /chronicle, /fork, /side, /rewind, git worktrees, sandbox/approval-policy modes

## Открытые вопросы
- Semantic drift / goal-adherence detection via embeddings and drift evals is academically promising but has limited real-world product adoption vs. simpler structural fixes (confidence rated only medium).
- How far habituation findings from security-warning and clinical-alert literature actually transfer to AI-coding approval UIs specifically remains an extrapolation, not agent-specific measurement.
- The general 'decision fatigue' theory remains unresolved/mixed in its own literature — unclear how much of it (if any) should be folded back into the approval-fatigue model.

## Источник
- DR-ID `DR26-07-08-MACB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- session-anchor-and-drift-check
- approval-fatigue
- goal-drift
- plan-first-workflow
- bounded-autonomy
- worktree-isolation
- context-compaction
- alert-fatigue
- insight-DR-DR26-07-14-HUB-02-approval-fatigue-and-task-drift-in-agentic-ai-codi — почти дословный дубль-тема (тот же DR-вопрос approval fatigue/task drift), другая дата синтеза
- insight-DR-DR26-07-28-HUB-14-2339-approval-fatigue-и-task-drift-в-агентном-ai-кодинг — тот же топик DR — approval fatigue и task drift, самый ранний отчёт серии
