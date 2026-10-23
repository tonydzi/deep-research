# DR26-07-05-HUB-04 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!--
source: Grok (grok.com), Expert model
topic: DR26-07-05-HUB-04 — AI task decomposition for parallel LLM sessions: cut quality + part lifecycle
date_collected: 2026-07-05

chat_url: https://приватный чат
verbatim: yes (get_page_text)
-->

# DR26-07-05-HUB-04 — AI-driven task decomposition for parallel LLM sessions: cut quality + part lifecycle

Conduct a deep research investigation on the following topic. Search the web extensively and cite sources.

CONTEXT (already known — do NOT re-research this):
We already completed a deep research pass (July 2026) on multi-session LLM orchestration mechanics and adopted its conclusions: lead session + max 3 writable / 2 review parallel sessions; isolation for writers via worktrees; seed prompts must contain goal, context, owned files, non-goals, verification commands, deliverable, done-criteria, escalation; implementers never self-certify "done" (fresh-context review gate); >15 independent chunks -> scripted workflow instead of chat orchestration. Our operator is a non-technical solo founder who asks the AI itself to decompose a complex task, then runs 3-5 parallel Claude Code sessions. Open pain points: (a) the QUALITY of the decomposition cut itself, and (b) what happens to parts AFTER the split - they may grow into standalone tasks or merge with other sessions.

TOPIC:
How to make an AI produce a HIGH-QUALITY decomposition of a complex task into truly independent, non-overlapping parts for parallel LLM coding/knowledge-work sessions - and how to manage the LIFECYCLE of those parts after the split (re-planning mid-flight, promoting a part into a standalone task, merging two converging workstreams, and integrating N parallel results back into one coherent whole).

OBJECTIVE:
Find the best current practices, state-of-the-art approaches, failures, trade-offs, emerging trends, and implementation frameworks - specifically for: (1) decomposition techniques that minimize hidden coupling (dependency-graph-first, interface/contract-first, layer-based, data-ownership-based cuts); (2) pre-flight independence checks - how to VERIFY parts won't collide BEFORE spawning sessions, not at merge time; (3) meta-prompts / planner patterns that make an LLM propose and self-critique its own decomposition; (4) dynamic re-decomposition: when and how to re-cut mid-flight; (5) part-promotion and workstream-merge patterns that preserve implicit decisions (API shape, naming, edge-case handling); (6) final integration/synthesis of N parallel outputs (merge plans, integration sessions, who owns the merge); (7) supervision ergonomics for a NON-TECHNICAL solo operator running 3-5 sessions: minimal dashboards, check-in cadence, early-warning signals of a bad cut.

RESEARCH REQUIREMENTS:

Academic research
papers (task decomposition / planner-executor / HTN planning for LLM agents, plan-and-solve, dynamic re-planning)
meta analyses
benchmarks (decomposition quality, multi-agent coding benchmarks measuring interference between parallel workers)

Industry practice
startups
unicorns
leading companies (Anthropic, OpenAI, Cognition, Cursor, Factory, Devin: how their products CUT work, not just run it)
open source projects (task-graph orchestrators, spec-first / contract-first agent workflows)

Competitive landscape
direct competitors
indirect competitors
substitutes (human PM techniques adapted to AI: WBS, INVEST criteria for user stories, Team Topologies fracture planes, Conway's-law-aware splitting)

Failure analysis
failed attempts
abandoned projects
lessons learned (bad cuts discovered at merge time; hidden coupling via implicit decisions; re-decomposition thrash)

Emerging trends
developments from the last 12 months
frontier approaches (self-critiquing planners, coupling metrics for LLM task graphs, automated merge-conflict-of-decisions detection)

Implementation recommendations
actionable framework: a reusable DECOMPOSITION CHECKLIST/prompt an operator can give the AI ("cut this task so that..."), a pre-flight independence test, and a lifecycle protocol for promote/merge/integrate
roadmap
priorities

OUTPUT FORMAT:

A. Executive Summary
B. Key Findings
C. Contradictions & Debates
D. Best Practices
E. Failure Modes
F. Strategic Recommendations
G. Action Plan
H. Sources

---

Thought for 1m 24s

A. Executive Summary

High-quality AI-driven task decomposition for parallel LLM coding/knowledge-work sessions requires shifting from ad-hoc splits to structured, spec/contract-first, grounded approaches that explicitly minimize hidden coupling. The dominant effective pattern is a planner-executor (or multi-role) architecture augmented by Spec-Driven Development (SDD) workflows such as GitHub Spec Kit and extensions like Spec Kit Agents.⁠arXiv

These externalize artifacts (SPEC.md, PLAN.md, TASKS.md with parallelizable [P] tasks), use pre-phase discovery (read-only repo probes for conventions/APIs/deps) and post-phase validation hooks, and support human gates. This directly addresses the two open pain points: (a) decomposition cut quality (via explicit interfaces, dependency awareness, and self-critique) and (b) post-split part lifecycle (persistent artifacts enable re-planning, promotion, merging via reconciliation, and coherent integration via conformance checks).

Industry leaders diverge: Cognition (Devin) favors single continuous-context agents with strong upfront interactive planning to avoid implicit-decision conflicts, while supporting limited parallel instances consolidated via PRs. Cursor enables parallel agents (up to 8) plus dedicated Plan Mode and background planning. Factory emphasizes agent swarms ("Droids") with orchestration for parallel decomposition, racing approaches, and convergence.⁠Cognition

Empirical evidence from the MAST taxonomy (Cemri et al., 2025) shows multi-agent LLM systems frequently fail due to specification issues, inter-agent misalignment (exactly the hidden coupling via implicit decisions on APIs/naming/edge cases), and weak task verification—validating the context's rule against self-certification and fresh-context review gates.⁠arXiv

Academic foundations include planner-executor frameworks (PEAR benchmark, Plan-and-Solve, ADaPT), HTN (Hierarchical Task Network) hybrids with LLM-generated heuristics or ChatHTN interleaving, and taxonomies of planning that emphasize task decomposition + reflection/refinement.⁠arXiv

Best actionable path for a non-technical solo founder: Adopt or emulate Spec Kit-style SDD as the backbone. Equip the lead planner LLM with a rigorous meta-prompt + decomposition checklist emphasizing contract/interface-first cuts, pre-flight independence verification (dependency graphs + grounding + self-critique), and explicit lifecycle protocols. Use persistent shared artifacts (git + markdown) as the minimal "dashboard." Gate major phases with human review. This delivers higher-quality independent parts, reduces merge-time surprises, and keeps supervision lightweight.

B. Key Findings

Planner-Executor + SDD is the prevailing high-performing pattern. The planner decomposes (often hierarchically or via explicit phases), executors implement with tools. SDD adds mandatory external artifacts and grounding/validation, dramatically reducing context blindness, hallucinations (e.g., phantom APIs), and architectural violations. Spec Kit Agents improved judged quality +0.15 and SWE-bench Lite Pass@1 by +1.7%.⁠arXiv
Contract/Interface-first and grounded decomposition minimizes hidden coupling far better than pure goal-based splits. Techniques that work: dependency-graph-first (analyze planned file touches, API definitions/uses, data flows for overlaps/cycles), interface/contract-first (define APIs, data models, acceptance criteria upfront in SPEC/PLAN so boundaries are explicit and enforceable), layer-based or data-ownership-based cuts (align with natural fracture planes). Pure functional or file-based cuts without these often create implicit shared decisions that surface only at merge.
Pre-flight independence verification is feasible and under-used. Discovery hooks (read-only probes) + validation checks + LLM self-critique against a checklist (INVEST-like criteria adapted: Independent, clear Interfaces, Verifiable done-criteria, minimal cross-session state/ownership overlap, escalation paths) catch collisions before spawning sessions. Dependency extraction from the plan/tasks artifact enables automated or semi-automated checks.
MAST taxonomy provides the clearest empirical map of failures. 14 modes in three categories—System Design/Specification Issues, Inter-Agent Misalignment, Task Verification—derived from 1600+ traces across frameworks. Critical ones include disobeying specs/roles, unaware of termination conditions, information withholding, repeating steps, no/incomplete/incorrect verification, and misalignment on shared context/decisions. Verification failures occur even in "successful" runs; many stem from design rather than model limits.⁠arXiv
Lifecycle management is enabled by persistent explicit artifacts. Re-planning: loop back to Plan/Tasks phase with updated context or trigger on detected mismatch (WebDART-style dynamic decomposition/re-planning, ADaPT as-needed, AdaPlanner feedback-driven). Promotion: extract a task/sub-spec into its own standalone workflow/branch. Merging converging workstreams: race approaches then reconcile via spec/plan diff or judge (preserves implicit decisions by surfacing and adjudicating them explicitly). Integration/synthesis: dedicated phase or lead session validates all outputs against top-level SPEC + runs cross-checks/tests; artifacts make "who owns the merge" clear (often human + synthesis agent or PR process).
Supervision ergonomics for non-technical operators center on artifact-centric minimalism. Shared git repo of SPEC/PLAN/TASKS + status (e.g., markdown tables or simple scripts) serves as living dashboard. Phase gates (Specify → Plan review → Tasks → Implement) provide natural check-in cadence. Early-warning signals: validation hook failures, high cross-task dependency count, plan drift (diff vs prior artifact), test regressions, or LLM-judge flags on session traces for misalignment/termination issues. Avoid heavy custom UIs; leverage existing tools (Cursor/Factory orchestration views, git history).
Human PM techniques translate powerfully. WBS (Work Breakdown Structure) for hierarchical decomposition into verifiable packages. INVEST criteria (especially Independent + Small + Testable) for agent-sized tasks. Team Topologies fracture planes (natural boundaries minimizing coupling—by domain, layer, data ownership, etc.) + Reverse Conway (design "team"/session structure to achieve desired architecture/comms). These inform cut quality and reduce inter-session coordination needs.⁠Mimacom

C. Contradictions & Debates

Multi-agent parallelism vs. single continuous context: Cognition/Devin argues multi-agent is fragile due to context fragmentation and compounding implicit-decision conflicts; prefers unified history + interactive upfront planning. Cursor and Factory actively enable and orchestrate parallel agents/swarms, showing gains when paired with strong planning, specs, and convergence mechanisms. Evidence suggests the difference lies in how parallelism is managed (loose chat orchestration vs. spec/artifact-orchestrated with gates).⁠LinkedIn
Upfront planning depth vs. dynamic/adaptive: Strong evidence for both. Devin/Cursor interactive/background planning + SDD gated phases reduce thrash. Dynamic re-planning (mismatch detection → re-decompose) handles emergence but risks re-decomposition thrash if not gated. Hybrids (static high-level plan + as-needed sub-decomposition + reflection) appear strongest.
Self-verification and "done" signals: MAST shows verification is a top failure category. Context's rule (no self-certify; fresh-context review gate) is strongly supported. Some systems still rely on agent self-assessment, leading to incomplete or incorrect termination.
Spec as overhead vs. enabler: Critics see SDD phases as slowing velocity. Proponents (and data) show net gains via fewer downstream hallucinations, easier debugging/auditing, and reliable parallelism. For complex tasks, the "slow is smooth, smooth is fast" effect dominates.

D. Best Practices

Decomposition techniques prioritizing low coupling: Contract/interface-first (define APIs, contracts, data models, acceptance criteria first). Dependency-graph-first (explicitly map and minimize cross-part file/API/data dependencies). Layer- or ownership-based cuts aligned with fracture planes. Use hierarchical (HTN-inspired) where natural. Always produce explicit TASKS.md with ownership, interfaces, done-criteria, and [P] parallel flags.
Meta-prompts / planner patterns for proposal + self-critique: Lead planner prompt must include: goal, full context, owned files/non-goals, verification commands, deliverable format, done-criteria, escalation. Require structured output (SPEC/PLAN/TASKS). Then self-critique pass: "Review this decomposition against [checklist: independence (no shared mutable state or ambiguous ownership), minimal interfaces, no hidden coupling via naming/edge-cases/conventions, verifiable per-part criteria, alignment with repo reality via grounding]. Propose fixes or confirm clean cut." Iterate critique 1-2x before finalizing.
Pre-flight independence checks: Run discovery/validation hooks (or equivalent repo probes). Extract dependency graph from PLAN/TASKS. LLM critique as above. Optional: simple script checking for overlapping owned files or undefined cross-references. Only spawn sessions after human or automated gate passes.
Dynamic re-decomposition: Trigger on explicit signals (validation failure, new requirement insight, test regression, context bloat). Re-invoke planner with full updated artifacts + delta. Gate via human or judge to avoid thrash. Prefer targeted sub-replanning over full re-cut when possible.
Part-promotion and workstream-merge patterns: Promotion = extract task + its spec slice into standalone Spec Kit workflow or branch; preserve traceability to parent. Merge converging streams: run in parallel (race), then reconcile via explicit diff of plans/specs/decisions or LLM judge + human; update shared artifacts to lock reconciled choices (API shape, naming, edge handling). This preserves implicit decisions by making them explicit and auditable.
Final integration/synthesis: Use a dedicated integration/synthesis session (or lead planner) that consumes all parallel artifacts + top-level SPEC. Validate conformance, run cross-tests, resolve any residual conflicts via the same reconciliation process. Output: merged PR or final deliverable + updated living specs. Ownership: typically human operator for final sign-off, supported by synthesis agent.
Supervision for non-technical solo operator: Artifact repo as single source of truth/dashboard (view SPEC/PLAN/TASKS/status.md in browser or simple viewer). Cadence: async review at phase gates + daily/every-few-hours high-level status skim. Early warnings: automated validation failures, dependency count spikes, drift detection (git diff or LLM compare), termination/unaware flags via trace sampling or judge. Minimal custom tooling; maximize leverage of Cursor/Factory views + git.

E. Failure Modes

Drawn heavily from MAST (empirically grounded) and observed patterns:

Specification/Design Issues: Vague or incomplete specs leading to drift; disobey task/role specs; poor initial decomposition creating unavoidable overlap.
Inter-Agent Misalignment: Information withholding or loss of shared context/decisions; ignoring other agents' outputs; conflicting implicit choices (naming, API shape, error handling, conventions) surfacing only at merge; context fragmentation.
Task Verification Failures: No/incomplete/incorrect verification; self-certification of "done" without fresh review; unaware of termination conditions; repeating completed steps or thrashing.
Lifecycle-specific: Re-decomposition thrash (planner oscillates); bad cuts discovered only at merge/integration; hidden coupling via unstated assumptions; promotion/merge losing traceability or prior decisions; integration producing incoherent whole despite locally correct parts.
Supervision/Operator: Context overload for human; missing early signals; over-reliance on agent self-reporting.

F. Strategic Recommendations

Prioritize cut quality and explicitness over raw parallelism. A clean, contract-first, pre-flight-verified decomposition with 3-5 independent parts outperforms a larger number of loosely coupled ones. Treat SDD artifacts as the coordination infrastructure (specs constrain agents, enable verification, support lifecycle operations, and serve as dashboard).

Invest in grounding + validation hooks (discovery before phases, checks after) as the highest-ROI addition—they front-load error detection and reduce context blindness.

Adopt explicit lifecycle protocols documented in the artifacts themselves; make promotion/merge/integration first-class operations with reconciliation steps that surface and adjudicate implicit decisions.

For the non-technical operator: artifact-centric minimalism + gated phases is the ergonomic sweet spot. Avoid building heavy dashboards; the living specs/plans/tasks + simple status + phase reviews suffice and scale.

Monitor and log against MAST-like categories in your own runs to iteratively improve prompts/workflows (self-improving via failure taxonomy).

G. Action Plan

Immediate (this week):

Adopt or replicate GitHub Spec Kit workflow (or equivalent SDD structure) for your next complex task.
Craft and test a master Decomposition Meta-Prompt + Checklist incorporating the elements in D (goal/context/owned files/non-goals/verification/deliverable/done-criteria/escalation + self-critique pass + INVEST-style independence criteria + contract/dependency awareness).
Run one pilot with pre-flight checks (manual repo probes + critique) and full artifact persistence.

Short-term (2-4 weeks):

Implement lightweight discovery/validation equivalents (scripts or LLM tool calls) if not using full Spec Kit Agents.
Define and document simple lifecycle protocols (promote/merge/integrate steps) in your constitution or README.
Establish minimal supervision rhythm: phase-gate reviews + artifact skim.

Medium-term (1-3 months):

Add automated or semi-automated pre-flight independence verifier (dependency graph extraction + overlap/coupling checks).
Experiment with dynamic triggers for re-planning and HTN-inspired hierarchical cuts where tasks are deeply nested.
Instrument runs for MAST-style failure logging to refine prompts.

Longer-term / Roadmap:

Explore or build coupling metrics for task graphs (shared files, API surface, data flows) to quantify cut quality.
Integrate self-critiquing planner loops and automated merge-conflict-of-decisions detection (diffing specs/decisions).
Evaluate hybrid planner-executor with HTN elements or advanced orchestration (e.g., Factory-style swarm patterns adapted to your stack).
Priorities ranking: (1) Spec/contract-first + grounding/validation (highest impact on cut quality and coupling). (2) Pre-flight checks + explicit self-critique. (3) Artifact-driven lifecycle protocols + human gates. (4) Lightweight supervision signals. (5) Advanced metrics/automation.

Measure success by: reduced merge conflicts/time, higher % of parts completing independently without rework, operator time spent on high-level review vs. firefighting, and task completion quality/consistency.

H. Sources

Cemri et al. (2025). "Why Do Multi-Agent LLM Systems Fail?" arXiv:2503.13657 (MAST taxonomy and MAST-Data).
Spec Kit Agents paper (2026). Context-grounded multi-agent SDD workflows. arXiv:2604.05278v1.
GitHub Spec Kit documentation and toolkit (ongoing 2025-2026). Spec-driven Specify → Plan → Tasks → Implement.
PEAR: Planner-Executor Agent Robustness Benchmark (2025/2026). arXiv:2510.07505.
Cognition Devin blogs and docs (Devin 2.0 Interactive Planning, architecture principles).
Cursor resources on Plan Mode, parallel agents, Composer/Agent workflows (2025-2026).
Factory.ai materials on Droids, multi-agent orchestration, problem decomposition (2025-2026).
Various planner-executor, Plan-and-Solve, ADaPT, HTN+LLM papers (2023-2026, including Huang taxonomy of LLM-agent planning, ChatHTN, L2HP, WebDART/AdaPlanner dynamic re-planning).
Team Topologies (Skelton & Pais) on fracture planes and team interaction modes (analogous application to AI session cuts).
Supporting discussions on SDD (Martin Fowler, Addy Osmani, etc.) and multi-agent challenges.

(93 sources — Grok Expert, thought 1m 24s)
