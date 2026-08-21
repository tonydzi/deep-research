---
dr_id: DR26-06-25-HUB-04
title: "Claude Code Agent Teams for an Individual Power-User"
date: 2026-06-25
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-25-HUB-04): Claude Code Agent Teams for an Individual Power-User

> Investigates whether a solo Windows power-user should use Claude Code Agent Teams (vs plain subagents, deterministic workflows, or an external message bus) for import pipelines, research/review, and multi-machine coordination, and quantifies the cost/risk tradeoffs.

## Ключевые выводы
- Agent Teams' unique value is persistent peer-to-peer coordination among a few live sessions — it wins specifically on breadth-first, read-heavy work (competing-hypothesis research, multi-lens parallel review), not on generic parallelism.
- Official Claude Code docs warn Agent Teams use ~7× more tokens than standard sessions when teammates run in plan mode; costs scale linearly with teammate count, and Anthropic recommends starting with 3-5 teammates, fewer on Windows.
- Independent benchmarking (UpGPT) found Agent Teams 73-124% more expensive than sequential execution at equivalent quality, mainly because each teammate reloads a copy of the same shared codebase context.
- Anthropic's own multi-agent research system (Opus lead + Sonnet subagents) beat single-agent Opus by 90.2% on an internal research eval, but used ~15× the tokens of a chat session — multi-agent gains are real but expensive and task-shape-dependent.
- Scenario modeling shows Teams costs ~1.0-2.1× a single session and ~1.3-2.9× a deterministic orchestrator in moderate cases, with the premium driven mainly by shared-context size, not reasoning depth.
- Windows in-process mode has documented failure modes: no /resume or /rewind for teammates, one team per session, no nested teams, no split-pane support, lead identity corruption after compaction, invisible duplicate teammates (one report: 10-151 duplicates from one spawn), and parent wake-up stalls requiring manual keypresses.
- Idle-notification/duplicate-ack loops wasted 13-22% of team-input tokens and inflated lead turns 3-4× in one real session; a fake task-list prompt-injection vector was also documented (May 2026).
- Model-mixing (Sonnet teammates + Opus lead) is well supported by evidence for high-value research/synthesis, but a widely cited harness README corrected its own earlier claim: Teams only becomes cost-effective past ~30 minutes of equivalent single-session effort.
- For batch/resumable work, frameworks built for durable state (LangGraph, CrewAI Flows, AutoGen/AG2's distributed runtimes, Claude Code's own 'dynamic workflows') all beat Agent Teams on resumability, retries, and reproducibility.
- Academic research shows multi-agent effectiveness is highly task-dependent: one study found relative gains ranging from +80.8% (decomposable financial reasoning) to -70.0% (sequential planning); another found a single agent beats multi-agent systems in ~43.3% of cases; self-organizing teams can underperform their best member by 8-38%.

## Рекомендации / решения
- Keep import pipelines off Agent Teams entirely — use a deterministic orchestration script + SQLite/queue for resumability, retries, and schema validation.
- Keep multi-machine coordination on the existing external message bus/Syncthing, not Agent Teams (which is scoped to one session, no nested/shared teams across sessions).
- Reserve Agent Teams for short, single-sitting, mostly read-only research or review tasks that genuinely need peer-to-peer debate or orthogonal review lenses — otherwise use plain subagents or a workflow.
- On Windows, cap teams at 2-3 teammates (not the general 3-5 default), default to read-only permissions, and avoid long-running or unattended sessions.
- Use Sonnet teammates with an Opus lead only when synthesis/memo quality justifies the premium; otherwise run Sonnet throughout.
- Instrument true cost by aggregating parent + all child-session token JSONLs before making any ROI judgment — parent-only cost views understate real spend.
- Run a small instrumented pilot (2 task classes: a research memo and a multi-lens review) comparing single-session, subagents/workflow, and Agent Teams baselines, with a hard acceptance gate (e.g., ≥30% wall-clock improvement or ≥20% more unique findings, ≤2.0× token multiplier, zero critical failures in 10 runs) before adopting Teams operationally.
- If reproducing task-list prompt-injection artifacts, coordinate via SendMessage/external docs instead of the team task list until fixed.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, UpGPT, Codecentric
- **Продукты/инструменты:** Claude Code, Agent Teams, Claude Code Workflows, Opus 4.6, Sonnet, LangGraph, CrewAI Flows, AutoGen/AG2, SQLite, Syncthing, Redis Streams, tmux, VS Code, Windows Terminal

## Открытые вопросы
- No clean, apples-to-apples public benchmark exists specifically for Claude Code Agent Teams on research/review tasks for solo Windows in-process users.
- GitHub issue stream on Agent Teams bugs is rich but noisy — unclear how many reported failure modes (duplicate teammates, permission deadlocks, task-list injection) are already fixed in the current patch level.
- Whether the June 2026 fixes (teammate effort inheritance, background task handling) meaningfully reduce the documented Windows in-process failure rate remains unverified.
- Exact token profile for the user's own workloads is unknown — scenario cost math in the report is illustrative, not measured.

## Источник
- DR-ID `DR26-06-25-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- multi-agent-offer-reflex
- decompose-into-parallel-sessions
- cowork-vs-cc-division
- ak47-simplicity
- model-routing-sonnet-grunt
- deterministic-script-gotchas
- machine-bus-telegram-rail
- insight-DR-DR26-07-04-HUB-11-multi-session-multi-agent-orchestration-patterns-f — более ранний DR по тому же вопросу, который этот расширяет
