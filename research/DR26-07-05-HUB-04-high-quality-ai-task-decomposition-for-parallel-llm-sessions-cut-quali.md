---
dr_id: DR26-07-05-HUB-04
title: "High-quality AI task decomposition for parallel LLM sessions: cut quality + part lifecycle"
date: 2026-07-05
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-05-HUB-04): High-quality AI task decomposition for parallel LLM sessions: cut quality + part lifecycle

> How should an AI decompose a complex task into truly independent parallel LLM sessions (dependency-aware cuts, pre-flight collision checks), and how should parts be managed afterward (re-planning, promotion, merging, integration)?

## Ключевые выводы
- A good decomposition is a cut through a dependency graph plus a write-ownership and decision-ownership map, not a list of task titles — cohesion-aware graph partitioning (e.g. Co-Coder's weighted dependency graph + Infomap community detection, Repository Interface Blueprint) beats naive 'backend/frontend/tests' splits, which can even degrade speed/quality under high cross-agent coupling.
- Worktree/branch isolation prevents file clobbering but not semantic conflicts: two agents can edit different files yet make incompatible implicit decisions (naming, API shape, error semantics, fixture assumptions, duplicate abstractions) — Git merges cleanly while the app is logically broken ('semantic clean merge, product-broken result'); STORM tracks read/write dependencies to catch this earlier instead of at merge time.
- Benchmarks show cooperative multi-agent coding is still fragile: CooperBench finds strong solo coding ability doesn't translate into team coordination (duplicate work, broken expectations, unfulfilled commitments, communication overload); ToolMaze shows implicit semantic failures cause the sharpest drops in agent recovery; TraceElephant shows output-only traces are insufficient for failure attribution in multi-agent swarms.
- Contract-first governance is the load-bearing mechanism before parallel writes start: freeze API/schema/naming/error-semantics/UX-states/test-fixtures into a machine-checkable artifact (Software Design Sketches in CodeTeam, Natural-Language Agent Harnesses) so parallel agents build against mocked/frozen boundaries rather than guessing.
- Industry converges on hierarchy over free-for-all parallelism: Supervisor (plans, builds dependency graph/contracts) → Workers (confined to one worktree/subtask) → Reviewer (fresh-context integration agent, ideally cross-model) — 'Agentic MapReduce' (Devin/Cognition) for wide independent shards, but cautious/serialized for anything touching shared architecture; Anthropic's orchestrator-subagent pattern works well for broad independent research, less so for coding with shared state.
- Traditional textual merge is structurally blind to agent intent — proposed fixes include 'Semantic Rebase'/Delta-CAS (reject stale commits, force re-fetch+regenerate), Observable-Read Isolation / optimistic concurrency control via a DeliveryLog (S-Bus) to prevent Structural Race Conditions, and immutable task folders with correction overlays (Glite ARF) instead of rewriting history.
- Re-decomposition should be scoped/local, not global: Task-Decoupled Planning and dual-thread architectures (background planner monitors state, raises an interruption flag, recalculates only the affected DAG branch) contain the blast radius; re-cut the whole plan only when a shared frozen contract itself turns out wrong.
- A cut is rejectable via a scorable checklist (ChatGPT's 'Cut Quality Score', 0–100 across write independence, decision independence, dependency cohesion, contract clarity, verification locality, merge-order clarity, observability, scope containment — reject below 65, spawn only above 80) and a pre-flight pairwise collision table asking 'how could A silently break B even without touching the same file?'
- Human PM techniques (WBS, INVEST, Team Topologies fracture planes, Conway's Law) remain useful but need AI-specific hardening — e.g., INVEST independence needs an added pairwise-collision test, 'definition of done' must require externally verifiable evidence instead of agent self-certification, and Conway's Law suggests deliberately designing the desired agent-communication topology first (Inverse Conway Maneuver) rather than letting a fluid agent swarm emerge.

## Рекомендации / решения
- Before spawning parallel sessions, produce four artifacts: Task Map (dependency graph, chosen cut + rejected alternatives), Contract File (frozen names/schemas/API/UX/terminology), Part Passports (one per session: owned/forbidden files, inputs, outputs, local verification, escalation triggers, done criteria), and an Integration Ledger (merge order, decisions, risks).
- Run a pre-flight independence test before opening sessions: build the task graph, fill a pairwise collision table (shared write files / shared decisions / shared fixtures / hidden coupling risk), ask the adversarial 'how could A silently break B' question for every pair, freeze shared decisions in a short contract doc, and rehearse the exact merge order.
- Prefer dependency-graph-first, interface/contract-first, or data/domain-ownership cuts as defaults; use layer-based cuts only when the API/schema is already frozen (client from fixed OpenAPI, tests after behavior specified) — layer cuts are the riskiest default for feature discovery.
- Structure execution as Supervisor (plans/contracts) → Workers (isolated worktrees, single subtask, forbidden-file list) → Reviewer/Integration Agent (fresh context, ideally a different model, reconciles diffs against the original contract) — implementers must never self-certify 'done'.
- Handle part lifecycle with explicit protocols and triggers: Promotion (part >~2x estimate, needs new API/schema decision, becomes a blocking prerequisite) → promotion packet + new standalone task; Workstream Merge (two parts converge on the same decision/hub file) → both pause, one merge owner decides, no self-resolution by the two agents; Local Replan vs Global Re-cut depending on whether a shared frozen contract itself broke.
- Integrate sequentially, not simultaneously: merge frozen contracts/stubs first → shared infra → lowest-dependency leaf parts → run tests after each merge → docs/synthesis → full verification → fresh-context review — never allow parallel merges to land at once.
- Give the non-technical solo operator a single status dashboard (part / status / risk-color / owned files / contract decisions / last evidence / blocker / next action) and a fixed list of early-warning phrases that should trigger an immediate pause (e.g., 'I need to edit a file outside my owned files', 'I changed the shared config/schema/router', 'Done' with no evidence).
- Roll out tooling incrementally: Week 1 manual templates and discipline only; Weeks 2-4 add lightweight dependency-graph tooling (dependency-cruiser/madge/pydeps, OpenAPI/JSON Schema/protobuf contracts); Month 2 start tracking decomposition-quality metrics (unexpected shared-file touches, mid-flight collisions, replans, review defects); Month 3 (only once the manual protocol works) add semi-scripted orchestration or frameworks like LangGraph/CrewAI/AutoGen — these add state machinery, not decomposition quality itself.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, Cognition (Devin), GitHub, Google, Factory AI, Cursor
- **Продукты/инструменты:** Co-Coder, STORM, CooperBench, SWE-bench, Multi-SWE-bench, Task-Decoupled Planning (TDP), WebDART, Plan-and-Solve, Least-to-Most prompting, Tree of Thoughts, Graph of Thoughts, LLMCompiler, Claude Code, Anthropic multi-agent research system, OpenAI Codex, GitHub Copilot coding agent, Google Jules, Factory Missions, Cursor worktrees/Composer/Agents Window, LangGraph, AutoGen, CrewAI Flows, OpenAI Agents SDK, CodeTeam / Software Design Sketches (SDS), Natural-Language Agent Harnesses (NLAHs), S-Bus, Glite ARF, PIVOT (Plan-Inspect-eVOlve Trajectories), ToolMaze, TraceElephant, Merge Magic, Repository Interface Blueprint (RIB), Infomap

## Открытые вопросы
- How exactly do production systems (Devin, Codex, Copilot, Jules) implement their internal 'cut algorithm' — public materials describe operational ingredients (isolation, upfront plans, review) but not the precise partitioning logic.
- How to reconcile entangled-context vs task-decoupled-planning risk: restricting an agent's context prevents error propagation but risks local optimizations that violate global architectural constraints if interface contracts are imperfectly specified.
- Whether LLM-as-judge / fully autonomous re-planning is safe when the judge and the executing agent share the same failure modes (a compromised or hallucinating agent can compromise its own judge), and how to handle irreducible ambiguity where necessary business/security context simply isn't in the prompt.
- How much of the proposed heavy protocol (Cut Quality Score, part passports, contract files, semantic rebase, ORI) is validated in practice for a non-technical solo operator vs. still primarily an academic/early-industry pattern.
- Grok's vendor section was truncated in the source collection — its specific findings/contradictions/best-practices beyond the shared framing are not fully captured here.

## Источник
- DR-ID `DR26-07-05-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- decompose-into-parallel-sessions
- multi-agent-offer-reflex
- multi-agent-role-discipline
- test-after-build-skill
- goal-drift-offload-to-seed-sessions
- verify-existing-before-proposing
- connect-rule-pipeline-ownership
- ak47-simplicity
