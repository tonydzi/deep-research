# DR26-07-05-HUB-04 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!--
source: ChatGPT (chatgpt.com), Pro / Deep Research, thought 12m 9s
topic: DR26-07-05-HUB-04 — AI task decomposition for parallel LLM sessions: cut quality + part lifecycle
date_collected: 2026-07-05

chat_url: https://приватный чат
verbatim: yes (DOM extract of full report)
note: this is the FULL-BRIEF run (the accidental title-only run is chat 6a4a935b, collected separately as -chatgpt-bonus)
-->

# DR26-07-05-HUB-04 — AI task decomposition for parallel LLM sessions (ChatGPT Deep Research)

I'll focus on the unresolved layer: high-quality task cuts and part lifecycle, grounding the synthesis in agent-planning research, benchmarks, industry/open-source practice, and operator-ready protocols.

---

A. Executive Summary

The core conclusion: a high-quality AI decomposition is not a list of subtasks. It is a cut through a dependency graph, a write-ownership map, and a decision-ownership map. For parallel LLM coding or knowledge-work sessions, the real enemy is not only Git merge conflict. It is hidden coupling through implicit decisions: API shape, naming, edge-case semantics, data ownership, copy style, test fixtures, migration order, and "obvious" assumptions each agent silently makes.

The strongest current pattern is:

Graph first → contract freeze → part passports → pre-flight collision test → parallel execution → lifecycle checkpointing → fresh-context review → one integration owner.

Recent academic work is converging on this. Co-Coder frames multi-agent coding as graph partitioning, using static dependency graphs and cohesion-aware scheduling because naïve parallelism can become slower or worse when agents split interdependent files. STORM argues that isolated worktrees avoid clobbering but push semantic conflict discovery to merge time; it instead tracks read/write dependencies and rejects unsafe writes earlier. CooperBench shows that even frontier coding agents can perform worse in cooperative settings than solo, with failures driven by broken expectations, poor commitments, and communication overload.

Industry practice points the same way. Anthropic's multi-agent research system works well for wide, independent research because an orchestrator delegates bounded subproblems to subagents and synthesizes results, but their own postmortems emphasize clear boundaries, effort budgets, tool guidance, and observability. Cognition's public writing is more conservative for coding: parallel write agents tend to fragment implicit decisions, so their near-term successful pattern is often read-only subagents, clean-context review, and manager-controlled map-reduce, not free-for-all parallel implementation.

For your operator model — non-technical solo founder running 3–5 parallel Claude Code sessions — the practical answer is not "spawn more agents." It is to force the lead AI to produce:

a dependency map;
a collision table;
a frozen contract file;
one "part passport" per session;
explicit promote / merge / re-cut rules;
a final integration plan owned by one integration session, not by the implementers.

A decomposition should be rejected before spawning sessions if any two parts share write files, share unresolved interface decisions, require the same naming/UX/domain semantics, or cannot be locally verified without another part's output.

B. Key Findings

1. The best decomposition cut is dependency-aware, not task-title-aware

The state-of-the-art direction is cohesion-aware partitioning: build a graph of files, modules, APIs, data objects, or claims; identify hubs; then assign highly connected work to the same agent. Co-Coder formalizes this as a graph-partitioning problem and reports that naïve file-based or "more agents" approaches can degrade speed and quality when cross-agent dependencies are high.

For a solo operator, this means the lead session must not simply say:
"Agent 1: backend, Agent 2: frontend, Agent 3: tests."
It should instead say:
"These files / concepts / APIs form one cohesion cluster. These other files form another. This shared hub must have exactly one owner or a frozen contract before parallel work starts."

The useful unit is a cohesion cluster, not an apparent subtask.

2. Worktree isolation is necessary but insufficient

Worktrees prevent agents from overwriting each other's files. They do not prevent two agents from making incompatible semantic choices. STORM's framing is useful: isolated environments push conflict detection to merge time, while many real conflicts are semantic, not textual. STORM instead tracks whether an agent's write is still safe given the read dependencies it used.

Conflict type / Worktrees catch? / Real risk:
- Same file edited differently — Usually yes — Low-to-medium
- API signature drift — Sometimes — High
- Different naming conventions — No — Medium
- Different error semantics — No — High
- Different mock/test fixture assumptions — Sometimes — High
- Two agents create duplicate abstractions — No — High
- Two agents make conflicting product decisions — No — Very high

So the pre-flight check should focus less on "will Git merge?" and more on "will these agents make the same decision independently?"

3. Current benchmarks show that multi-agent coding coordination is still fragile

SWE-bench and Multi-SWE-bench measure real issue-resolution ability across repositories and languages, but they are primarily single-agent or method-comparison benchmarks, not clean tests of parallel-worker interference. CooperBench is more directly relevant because it evaluates cooperative coding where two agents must implement related features in the same repository. It finds that strong individual coding ability does not reliably translate into team coordination; common failures include duplicate work, broken expectations, unfulfilled commitments, and unproductive communication.

Implication: do not assume that frontier agents know how to cooperate just because they can code. Cooperation must be designed into the task structure.

4. LLM planning helps, but unstructured LLM planning is not enough

Plan-and-Solve, Least-to-Most prompting, Tree of Thoughts, Graph of Thoughts, and related methods all support the general idea that explicit decomposition improves reasoning. But most of those methods were designed for sequential reasoning or search, not for safe parallel write execution.

Recent planning surveys are blunt: LLMs are useful planners, but standalone LLM planning remains brittle on long-horizon tasks, tool constraints, state tracking, and refinement. Current best practice is to combine LLM planning with external structure: graphs, schemas, verifiers, state machines, benchmarks, and explicit constraints.

So the planner prompt must force the model to expose:
candidate cuts; dependency assumptions; pairwise collision risks; unresolved contracts; verification locality; merge order.
A planner that produces only "three tasks" has not done the job.

5. Dynamic re-decomposition should be local, not chaotic

The emerging pattern in agent planning is scoped replanning. Task-Decoupled Planning decomposes long-horizon work into a DAG of subgoals with scoped contexts, then replans locally when feedback invalidates part of the plan. WebDART similarly focuses on dynamic decomposition and replanning when new information makes the initial plan suboptimal.

For parallel LLM sessions, the rule should be:
Re-cut only the affected part unless a shared contract is invalidated.
A bad lifecycle protocol causes re-decomposition thrash: every new discovery reshuffles all sessions. A good protocol contains the blast radius.

6. Product practice is converging on "plan first, isolate, verify, review"

Public product surfaces from Codex, GitHub Copilot coding agent, Google Jules, Factory Missions, Devin, Cursor-style worktrees, and Anthropic's Claude Code patterns all point toward the same operational ingredients: isolated environments, upfront plans, branch/worktree separation, repo instructions, progress visibility, test evidence, and review before merge.

But public materials rarely expose the exact internal "cut algorithm." The visible best practice is therefore less "copy Devin's planner" and more:
force an explicit plan before code; require source/code citations for the plan; make the agent state what it will not touch; use isolated branches/worktrees; require test evidence; review with fresh context; keep final integration under one owner.

7. Human PM techniques remain useful, but need AI-specific hardening

Classic WBS decomposition, INVEST user-story criteria, Team Topologies fracture planes, and Conway's-law-aware splitting are still useful. WBS gives hierarchy; INVEST emphasizes independent/testable slices; Team Topologies emphasizes natural fracture planes and cognitive load; inverse Conway thinking warns that communication structure shapes architecture.

But AI sessions are not normal human teams. They have weaker long-term memory, weaker implicit social repair, and a tendency to invent local conventions. So every human PM idea needs an AI-specific add-on:
- WBS → Add write ownership and dependency graph
- INVEST independence → Add pairwise collision test
- Team Topologies fracture planes → Add explicit file/API/data ownership
- Conway's Law → Make agent communication topology match desired architecture
- Definition of done → Require external verification logs, not self-certification
- Backlog item → Convert to a "part passport" with forbidden files and escalation triggers

C. Contradictions & Debates

1. "Multi-agent systems work" vs "don't build multi-agents"
Both are true in different regimes. Anthropic reports strong results for multi-agent research when the task is broad, search-heavy, and decomposable into independent information-gathering threads (orchestrator-worker architecture, clear subagent boundaries, synthesis by lead). Cognition is more skeptical about parallel write agents in coding because multiple implementers make incompatible implicit choices; their successful pattern is closer to manager + read-only helpers + clean-context reviewer + controlled map-reduce.
Resolution:
- Wide research → Parallelize aggressively
- Independent code leaves → Parallelize cautiously
- Shared architecture/API → Do not parallelize until contract is frozen
- Refactor touching hub files → Usually serialize
- Final integration → Single owner
- Review → Parallel review useful if criteria are explicit

2. "Let agents communicate" vs "communication does not solve coordination"
CooperBench suggests communication alone does not reliably fix coordination. Structure it:
Bad: "Coordinate with the other sessions."
Good: "You may only ask for: API signature, naming decision, test fixture ownership, merge blocker. Every message must produce one of: decision, blocker, or contract update."
The useful artifact is not chat. It is a decision ledger.

3. Worktrees vs shared-state systems
Worktrees are operationally simple but defer semantic conflicts until merge (STORM). Practical synthesis: use worktrees for isolation + add a manual/scripted state-dependency ledger (files read; files written; contracts assumed; decisions made; tests run). If a session writes based on stale assumptions, pause and replan before merge.

4. Layer-based cuts are intuitive but often bad
LLMs love layer cuts (Agent1 backend / Agent2 frontend / Agent3 tests). This often fails because one feature's semantics span all layers. The merge is textually clean but semantically incoherent. Layer cuts are safe only when API/schema is frozen, UI behavior is specified, error semantics are specified, test fixtures are owned, one layer is truly independent. For most product work, vertical domain/data cuts are safer.

5. Upfront planning vs dynamic replanning
Current research favors hybrids: upfront DAG/contract/decomposition, then local replanning when feedback invalidates a node. Operational rule: Replan locally unless a frozen shared contract is wrong. If the shared contract is wrong, pause all affected sessions and run a contract-resolution micro-task.

D. Best Practices

D1. Preferred decomposition strategies, ranked
1. Dependency-graph-first cut — best for codebases, docs, migrations. Lead agent constructs file/module dependency graph, API call graph, shared config/migration/fixture map, hub-files list, likely write set per part; cut so cross-part edges are minimal. Useful unit = cohesion cluster.
2. Interface / contract-first cut — best when parallel parts must interact but boundary can be frozen. Create a contract artifact (public API/schema/interface: function names, types, routes, events, error names, state machine, UX states, test fixtures; decisions frozen: naming, edge cases, backward compat, migration order, logging, security; who owns changes).
3. Data-ownership / domain-ownership cut — best DEFAULT for product code. Each part owns one domain object/workflow/table/user journey end-to-end. (e.g. "User invitation flow" / "Billing export flow" / "Admin audit flow" instead of Backend/Frontend/Tests.)
4. Research facet / source-ownership cut — best for knowledge work. Hidden coupling = claim overlap, source overlap, inconsistent interpretation. Use source/facet ownership (Academic evidence / Industry practice / Failure analysis / Competitive landscape / Implementation framework). Each part returns claims, sources, uncertainty, contradictions, implications, "do not use this source for X" warnings. Integration session must synthesize, not concatenate.
5. Layer-based cut — use ONLY when contracts already frozen (client from fixed OpenAPI, docs after code stable, tests after behavior specified, UI skinning). Risky for feature discovery.

D2. Cut Quality Score (reject or re-cut below 80)
- Write independence (25): no shared write files except explicitly owned contract files
- Decision independence (20): no unresolved naming/API/product/edge-case decision owned by two parts
- Dependency cohesion (15): high internal cohesion; low cross-part dependency
- Contract clarity (15): interfaces/schemas/errors/UX states/fixtures frozen where shared
- Verification locality (10): each part can prove progress without sibling output
- Merge order clarity (5): integration order obvious and reversible
- Operator observability (5): non-technical operator can see status/risk/blockers
- Scope containment (5): each part has clear non-goals and forbidden files
Interpretation: 90–100 spawn; 80–89 spawn but monitor closely; 65–79 re-cut or freeze more contracts; <65 do not parallelize.

D3. Pre-flight Independence Test (run BEFORE opening parallel sessions)
Step 1 — Produce the task graph (code: files/modules, shared configs/migrations/schemas/generated/fixtures, public interfaces, tests likely to fail, hub files; knowledge work: claims, source classes, concepts needing consistent definitions, output sections, likely contradictions).
Step 2 — Create the collision table (per pair: shared write files | shared decisions | shared test/source fixtures | hidden coupling risk | mitigation | pass?). Fail the cut if any pair shares a write file, API/schema decision, migration/config/fixture, UX state, terminology decision, unclear merge order, or verification that depends on sibling completion.
Step 3 — Ask the adversarial question for every pair: "How could A silently break B even if they never edit the same file?" (A changes exported type semantics; both create different names for same concept; both add competing helpers; both alter shared fixture expectations; one changes docs terminology; one changes default behavior without updating tests; one adds migration assumptions the other violates.) If the lead AI cannot answer, it has not understood the cut.
Step 4 — Freeze shared decisions (API/type/route/error names, DB fields, migration order, UX states, logging names, fixture ownership, source/citation ownership, style/terminology). Short Markdown file; prevents three agents making the same choice differently.
Step 5 — Rehearse the merge: "If all parts succeed, what exact order are they integrated in, and what tests run after each merge?" Good order: contract/schema/stub → shared infrastructure → leaf implementations → tests → docs/synthesis → final review.

D4. Part Passport (each parallel session gets one)
Fields: Mission (one-sentence outcome) · Owned artifacts · Read-only context · Forbidden artifacts · Inputs (contracts/assumptions/sources/accepted decisions) · Outputs (diff/report section/tests/decision note/integration packet) · Frozen contracts (API names, schemas, UX states, terminology, edge-case behavior) · Local verification (commands/checks/evidence) · Non-goals · Escalation triggers (pause & report if: owned files insufficient; forbidden files need edits; contract appears wrong; sibling output required; scope expands materially; tests fail for reasons outside this part) · Done criteria (evidence-based, externally reviewable, no self-certification).

E. Failure Modes
- Fake independence (agents edit different files but behavior conflicts) → decision ledger + pairwise collision test
- Hub-file collision (everyone needs same config/schema/router/index) → one owner or serial contract task
- Layer-split incoherence → prefer vertical/domain cuts
- Duplicate abstractions → define abstraction owner
- Implicit API drift (tests fail after clean merge) → contract-first artifact
- Re-decomposition thrash → local replan only unless contract breaks
- Communication theater (long updates, no decisions) → messages must update blocker/decision/contract
- Premature promotion (tiny subtask becomes standalone bureaucracy) → promote only on defined triggers
- Late promotion (one part balloons, blocks integration) → checkpoint scope ratio
- Merge-time archaeology (integrator cannot tell why choices were made) → require integration packet
- Review rubber-stamp (implementer says "done" and gets merged) → fresh-context review gate
- Knowledge-work contradiction (inconsistent definitions in final report) → claim/source ownership matrix
- Tool over-automation (orchestration hides bad cut) → decomposition quality gate before tooling
The most expensive failure is semantic clean merge, product-broken result. Git says yes; reality says no.

F. Strategic Recommendations

F1. Use a four-artifact operating system (create BEFORE spawning sessions)
1. Task Map (goal, non-goals, candidate cuts, chosen cut + why, why rejected cuts rejected, dependency graph, shared contracts, integration order).
2. Contract File (contracts/<task-id>.md — canonical source for names/schemas/API shape/UX states/edge cases/terminology).
3. Part Passports (parts/<task-id>/<part-id>.md — one per session, what each Claude Code session receives).
4. Integration Ledger (integration/<task-id>.md — branch/worktree name, part status, files changed, tests run, decisions made, merge order, unresolved risks, final synthesis notes).

F2. Use lifecycle states, not ad hoc judgment
Candidate → Contracted → Spawned → In-flight → Checkpointed → Ready-for-review → Fresh-context-reviewed → Integrated → Archived.
Exception states: Needs-recut, Promoted-to-standalone, Merged-with-part-X, Serialized, Abandoned.

F3. Promotion protocol — promote a part into standalone task when too large/decision-heavy to remain a leaf.
Triggers: part scope >~2× original estimate; needs new API/schema/domain decision; requires modifying forbidden files; becomes prerequisite for multiple other parts; cannot be verified locally; agent discovers product/architecture ambiguity; siblings waiting on its decision.
Promotion Packet: original part · why promotion needed · work completed · decisions already made · decisions not yet made · downstream consumers · proposed new task · parent task impact. After: freeze parent part; create new contract/architecture task; update dependent passports; resume only unaffected parts.

F4. Workstream merge protocol — merge two converging streams when solving same decision / touching same cohesion cluster.
Triggers: both need same API/schema decision; overlapping abstractions; same hub file; same terminology differently; same test fixtures; ambiguous integration order.
Merge Packet: streams being merged · collision discovered · stop condition (both stop writes) · decision comparison · canonical decision · surviving branch · transplanted work · review required · tests/checks.
Critical rule: Do not let both sessions "resolve it themselves." Appoint one merge owner.

F5. Integration protocol — owned by a dedicated lead/integration session. Implementers submit integration packets, not final truth (summary · files changed · contracts used · decisions made · tests run · known risks · suggested merge order · rollback notes). Integration order: merge frozen contracts/stubs → shared infra → lowest-dependency leaf parts → tests after each → docs/synthesis → full verification → fresh-context review → final operator summary. Integration session actively looks for duplicate helpers, inconsistent names, mismatched edge cases, stale assumptions, tests that pass only in isolation, docs contradicting behavior.

F6. Supervision ergonomics for non-technical solo operator
Single dashboard: | Part | Status | Risk | Owned files/sections | Contract decisions | Last evidence | Blocker | Next action |
Risk colors: Green (within passport, local verification possible) / Yellow (scope or contract ambiguity, contained) / Red (forbidden files, shared decision, or blocked integration).
Event-based check-ins: after repo/task recon; before first write; when touching any contract/shared file; when local verification first passes/fails; before declaring ready for review; when escalation trigger fires.
Check-in format: Status (G/Y/R) · What changed (one paragraph) · Evidence (commands/files/sources/diff) · Decisions made · Blockers · Contract changes requested (Y/N + exact change) · Continue/Pause recommendation.
Early-warning signals of a bad cut — pause immediately if a session says: "I need to edit a file outside my owned files." / "I assumed the API would look like…" / "I created a helper similar to…" / "This depends on what another session decides." / "I changed the shared config/schema/router." / "I updated tests globally." / "I refactored while I was here." / "I could not run verification, but it should work." / "I renamed the concept for clarity." / "I need the other branch to pass tests." / "There are many small conflicts, but they seem easy." / "Done" without command output, source list, or diff summary.
Most important rule for a non-technical operator: If two sessions are about to make the same decision, pause both and create a contract-resolution micro-task.

F7. Competitive landscape
Direct tools: Claude Code / Anthropic multi-agent patterns (orchestrator-subagent, generator-verifier, agent teams, shared state — patterns not a full decomposition-quality metric); OpenAI Codex cloud/desktop (isolated agents, branch/worktree parallelism, logs/test evidence); Cursor agents/worktrees (isolation doesn't solve semantic coupling); Devin/Cognition (planning before coding, review, cautious about parallel writers); Factory Missions (structured missions, plan, milestones, dashboard, needs strong automated verification); GitHub Copilot coding agent (branch-based, logs, PR); Google Jules (plan-approval in isolated VM, task-level).
Indirect frameworks: LangGraph (graph/state machine, conditional edges, parallel tool execution); AutoGen (group-chat, hierarchical coordination); CrewAI Flows (event-driven workflows, state, branching, visualization); OpenAI Agents SDK (manager vs handoff; avoid splitting unless instructions/tools/policy differ). Useful once decomposition protocol is stable; do not magically produce good cuts.
Substitutes: One strong agent sequential (high coupling, unclear architecture); one writer + read-only reviewers (shared codebase, risky feature); scripted workflow (>~15 independent chunks or repetitive ops); human PM/architect first (unclear product semantics/architecture); contract-first design task (multiple future sessions depend on same API/schema).

G. Action Plan

G1. Pasteable decomposition prompt (use with lead AI BEFORE spawning sessions)
"""
You are the decomposition lead for a complex coding/knowledge-work task.
Your job is NOT to make a generic task list. Your job is to decide whether this task can be safely split into independent parallel LLM sessions, and if yes, produce collision-resistant part packets.
Context: Operator will run 3–5 parallel LLM sessions. Implementers must not self-certify done. Fresh-context review will happen later. Prefer fewer, truly independent parts over many coupled parts. Say DO NOT PARALLELIZE if the task is too coupled.
Task: <PASTE TASK HERE>
Known constraints: <PASTE EXISTING CONTEXT / NON-GOALS / REPO DETAILS HERE>
Produce:
1. Task understanding (goal, non-goals, unknowns, assumptions)
2. Dependency/coupling map (code: files/modules, shared configs/schemas/migrations/fixtures, public interfaces, hub files, tests likely affected; knowledge work: major claims, source classes, concepts needing consistent definitions, output sections, likely contradictions)
3. Candidate decomposition cuts (>=3: dependency-graph-first; interface/contract-first; domain/data-ownership or source/facet-ownership; layer-based only if safe). For each: parts, why it might work, hidden coupling risks, merge risks, verification strategy, Cut Quality Score.
4. Selected cut (or DO NOT PARALLELIZE) + why rejected cuts rejected.
5. Pre-flight collision table for every pair.
6. Frozen contracts required before spawning (or create serial contract-resolution task).
7. Part passports (mission, owned files, read-only context, forbidden files, inputs, outputs, frozen contracts, local verification, non-goals, escalation triggers, done criteria, exact seed prompt for the worker session).
8. Integration plan (merge/synthesis owner, merge order, checks after each merge, final verification, rollback risks).
9. Lifecycle rules (triggers for local replan, global re-cut, promotion, workstream merge, serialization, abandonment).
10. Operator dashboard (minimal status table).
Self-critique BEFORE finalizing: Where is hidden coupling likely? Which pair is most dangerous? Which implicit decisions could diverge? Which part is most likely to grow? What would make this cut fail at merge time? Revise the plan once after the self-critique.
"""

G2. Pre-flight checklist (all "yes" before starting)
Graph/ownership: each part exclusive owned files; shared write files have exactly one owner; hub files owned by one part or serial; cross-part dependencies listed.
Contracts: shared API/schema/type names frozen; error semantics frozen; UX states/copy terminology frozen; test fixture ownership defined; knowledge-work definitions & source ownership defined.
Verification: each part runs local checks without sibling completion; explicit evidence requirements; fresh-context reviewer can verify from packet.
Lifecycle: promotion/merge/replan triggers defined; integration order defined.
Operator: dashboard one row per part; standardized check-in format; early-warning signals listed.
Decision: Spawn / Re-cut / Serialize / Create contract-resolution task first.

G3. Lifecycle protocol
Local replan (new info affects only itself): part pauses → reports discovery → updates passport → lead approves → continues.
Global re-cut (decomposition boundary invalid): affected parts pause → lead updates dependency map → new collision table → new passports → resume after contract update. Triggers: shared contract wrong; two parts need same file/decision; merge order no longer clear; one part becomes prerequisite for others.
Promotion (one part becomes standalone task): part pauses → promotion packet → lead freezes parent assumptions → new standalone task → dependent parts updated.
Workstream merge (two parts converge): both pause → compare decision ledgers → choose canonical approach → one merge owner → transplant useful work → fresh review.
Integration (parts ready): integration owner receives packets → merges one at a time → checks after each → resolves semantic conflicts → fresh-context review → final synthesis.

G4. Minimal tooling roadmap
Week 1 — Manual but disciplined: templates TASK_MAP.md, CONTRACT.md, PART_PASSPORT.md, INTEGRATION_LEDGER.md, STATUS_DASHBOARD.md. Rule: no parallel session starts without a part passport and collision table.
Weeks 2–4 — Lightweight dependency inspection: TS/JS (dependency-cruiser, madge, ts-prune, eslint boundaries); Python (pydeps, import-linter, modulegraph); general (ripgrep, git grep, tree, cloc); ADR Markdown; API contracts (OpenAPI, JSON Schema, protobuf, TS types); one test command per part + full integration command. Knowledge work: SOURCE_MATRIX.md, CLAIM_LEDGER.md, CONTRADICTIONS.md, TERMINOLOGY.md.
Month 2 — Add evaluation: track historical decomposition quality (predicted risky pairs, actual conflicts, hidden decisions found, recut needed?, merge pain 1–5). Metrics: shared files touched unexpectedly; contract changes after spawn; pairwise collisions discovered mid-flight; local replans; global re-cuts; review defects per part; integration time vs implementation time.
Month 3 — Semi-scripted orchestration (only after manual protocol works): generate folders for passports; create branches/worktrees; maintain status dashboard; collect integration packets; run verification commands; diff forbidden-file violations; flag shared touched files. LangGraph/CrewAI Flows/AutoGen/OpenAI Agents SDK only if process already clear — they add state/orchestration, not the decomposition quality gate.

G5. Practical decision rules
Parallelize when: parts have separate owned files; contracts frozen; each part verifies locally; merge order obvious; hidden decision overlap low; one part's failure doesn't block all others.
Do not parallelize when: task is mostly architecture discovery; same hub files central; API/schema unsettled; product semantics unclear; tests can't run locally; every part needs same context; lead AI cannot explain how parts could collide.
Use one writer + helpers when: broad codebase search needed; many design alternatives; implementation touches shared architecture; quality > speed.
Use parallel research sessions when: sources separable; claims independently verifiable; final synthesis has one owner; terminology frozen.

H. Sources
Academic/benchmark: Co-Coder / cohesion-aware task partitioning (arXiv); STORM / shared workspace state management (arXiv); Task-Decoupled Planning (arXiv); Agent Planning Benchmark (arXiv); LLM workflow dependency trust paper (NeurIPS); CooperBench (arXiv, cooperbench.com); SWE-bench & Multi-SWE-bench (swebench.com); Plan-and-Solve, Least-to-Most, Tree of Thoughts, Graph of Thoughts, LLMCompiler, WebDART (arXiv); LLM planning surveys (aclanthology, arXiv).
Industry/product: Anthropic multi-agent research system (anthropic.com); Anthropic multi-agent coordination patterns (Claude docs); OpenAI Codex cloud/desktop; GitHub Copilot coding agent; Google Jules; Devin / Cognition (cognition.com, Devin Docs); Factory Missions (Factory Documentation).
Orchestration & PM substitutes: LangGraph, AutoGen, CrewAI Flows, OpenAI Agents SDK; WBS, INVEST, Team Topologies, Conway's Law / inverse Conway maneuver (martinfowler.com, pmi.org, agilealliance.org, teamtopologies.com).
