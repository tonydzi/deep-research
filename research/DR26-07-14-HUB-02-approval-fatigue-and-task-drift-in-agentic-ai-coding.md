---
dr_id: DR26-07-14-HUB-02
title: "Approval Fatigue and Task Drift in Agentic AI Coding"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-02): Approval Fatigue and Task Drift in Agentic AI Coding

> What causes approval fatigue and session/task drift in agentic coding tools (Claude Code, Cursor, Copilot, Codex, Windsurf, Devin, Aider), and which interventions actually fix them?

## Ключевые выводы
- Anthropic reports Claude Code users approve ~93% of permission prompts, showing supervision has degraded into reflexive clicking rather than meaningful review.
- Approval fatigue is best modeled via alert/warning-fatigue + habituation research from medicine and security, not generic 'decision fatigue' — a 2025 healthcare systematic review found statistically significant support in only 45% of reviewed cases, and a 2025 field study found no evidence of decision fatigue in a real triage setting.
- Habituation to frequent low-value warnings can generalize to critical warnings when both share the same UI/interaction pattern — uniform approval-click UX trains users to ignore risk distinctions between safe edits and boundary-crossing actions.
- Session/task drift is mainly a context-architecture failure: Anthropic states performance degrades as context fills; Cursor states long conversations accumulate noise and cause the agent to drift toward unrelated work.
- Academic work formalizes this as 'goal drift' (2025 AIES paper: agents diverge from initial objectives under competing pressures) and 'inherited goal drift' (2026 paper: even strong models inherit off-target behavior from weaker or already-drifting trajectories).
- The converging fix across vendors is structural, not more confirmations: bounded autonomy (sandboxing + allowlists + classifier/auto-review for routine approvals), plan-first execution with an external Markdown artifact, verifiable success criteria (tests/lints/screenshots), cheap session-map/compaction tools, and worktree/branch isolation for bifurcated work.
- Claude Code, Cursor, Copilot, Codex, Windsurf, and Devin all now converge on the same operating pattern: short versioned persistent context files (CLAUDE.md/AGENTS.md/Rules), explicit plan-first modes, session compaction/chronicle/progress features, and isolated worktrees for parallel work.
- Generic 'Are you sure?' dialogs and repeated consent-style pop-ups reliably fail (become 'wallpaper', push users to default acceptance); undo is a better mechanism than confirm for frequent reversible actions.
- Full bypass / 'dangerous' no-approval modes are explicitly framed by both Anthropic and OpenAI as tools for controlled/disposable environments, not as the default remedy for approval fatigue on a real machine.
- Best recovery from a drifting session is revert → refine the external plan → rerun in a fresh session, rather than repeatedly correcting the same polluted session; both Cursor and Claude's own docs recommend starting fresh after ~2 corrections on the same issue.

## Рекомендации / решения
- Replace click-by-click supervision with bounded autonomy: sandbox + allowlist + boundary-only human approvals, plus auto-review/classifier handling of routine escalations (Claude Code auto mode, Codex sandbox+approval-policy separation).
- Make every nontrivial task spec-first: produce an external Markdown plan (goal/non-goals, files involved, acceptance tests, explicit out-of-scope, rollback point), then implement in a fresh session against that artifact.
- Keep persistent context files (CLAUDE.md / AGENTS.md / Rules) short, versioned, and load-bearing; promote a correction into the file once it has been repeated twice rather than re-explaining it each session.
- Adopt hard reset/split rules: start a new session when the task changes or after repeated confusion; fork or move to a worktree instead of continuing a bifurcated task in one transcript.
- At the end of long runs, force a compact session map (attempted / changed / passed / failed / remaining / plan location) using native tools (Claude /compact, Copilot /chronicle, Codex /compact, Devin Progress tab, Windsurf plan files) instead of rereading full transcripts.
- Use external verification targets (unit tests, linters, builds, screenshots, acceptance criteria) so the agent's own pass/fail loop replaces constant human judgment calls.
- Use a fresh-context reviewer (subagent, second session, or PR review pass) rather than letting the same session that wrote the code judge it.
- Avoid generic confirm-everywhere UX and avoid defaulting to full-bypass/dangerous permission modes on the primary machine; reserve those for disposable/controlled environments.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, Cursor, GitHub, Windsurf, Cognition (Devin), AHRQ
- **Продукты/инструменты:** Claude Code, Codex, GitHub Copilot, Cursor, Windsurf, Devin, Aider, CLAUDE.md, AGENTS.md

## Открытые вопросы
- Whether embedding-based semantic-drift / goal-adherence detectors will move from academic research into real product adoption, versus simpler structural fixes remaining dominant.
- Whether 'decision fatigue' or 'warning/alert fatigue + habituation' is the more accurate underlying mechanism for approval fatigue — general decision-fatigue evidence is mixed (only 45% of reviewed healthcare cases significant; one 2025 field study found none).
- How well the academic 'inherited goal drift' findings (models inheriting drift from weaker/already-drifting trajectories) generalize to everyday production agentic coding sessions.

## Источник
- DR-ID `DR26-07-14-HUB-02` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DL-2026-07-14--Approval-Fatigue-and-Task-Drift-in-Agentic-AI-Codi.md`

## Связано
- [[agent-approval-ux]]
- [[context-engineering]]
- [[plan-first-workflow]]
- [[session-drift]]
- [[alert-fatigue]]
- [[claude-code-best-practices]]
- [[worktrees-isolation]]
- [[autonomous-coding-agents]]
- [[insight-DR-DR26-07-28-HUB-14-2339-approval-fatigue-и-task-drift-в-агентном-ai-кодинг]] — тот же топик DR — approval fatigue и task drift, более ранний отчёт по той же теме
