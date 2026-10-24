# DR26-07-05-HUB-04 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "AI Task Decomposition Strategies"
type: ai-conversation
stage: raw
source: chatgpt


authored_by: hybrid
conversation_id: 6a4a9488-b8cc-83eb-8af7-80bbbd292fc4
model: "gpt-5-5-pro"
url: "https://приватный чат"
date_recorded: 2026-07-05
date_added: 2026-07-07
valid_as_of: 2026-07-05
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 22
related_concepts: []
revisit_if: ""
---

# AI Task Decomposition Strategies

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 22 сообщений

## Диалог

**Anton:**

# DR26-07-05-HUB-04 — AI-driven task decomposition for parallel LLM sessions: cut quality + part lifecycle

Conduct a deep research investigation on the following topic.

CONTEXT (already known — do NOT re-research this):
We already completed a deep research pass (July 2026) on multi-session LLM orchestration mechanics and adopted its conclusions: lead session + max 3 writable / 2 review parallel sessions; isolation for writers via worktrees; seed prompts must contain goal, context, owned files, non-goals, verification commands, deliverable, done-criteria, escalation; implementers never self-certify "done" (fresh-context review gate); >15 independent chunks -> scripted workflow instead of chat orchestration. Our operator is a non-technical solo founder who asks the AI itself to decompose a complex task, then runs 3-5 parallel Claude Code sessions. Open pain points: (a) the QUALITY of the decomposition cut itself, and (b) what happens to parts AFTER the split - they may grow into standalone tasks or merge with other sessions.

TOPIC:
How to make an AI produce a HIGH-QUALITY decomposition of a complex task into truly independent, non-overlapping parts for parallel LLM coding/knowledge-work sessions - and how to manage the LIFECYCLE of those parts after the split (re-planning mid-flight, promoting a part into a standalone task, merging two converging workstreams, and integrating N parallel results back into one coherent whole).

OBJECTIVE:
Find the best current practices, state-of-the-art approaches, failures, trade-offs, emerging trends, and implementation frameworks - specifically for: (1) decomposition techniques that minimize hidden coupling (dependency-graph-first, interface/contract-first, layer-based, data-ownership-based cuts); (2) pre-flight independence checks - how to VERIFY parts won't collide BEFORE spawning sessions, not at merge time; (3) meta-prompts / planner patterns that make an LLM propose and self-critique its own decomposition; (4) dynamic re-decomposition: when and how to re-cut mid-flight; (5) part-promotion and workstream-merge patterns that preserve implicit decisions (API shape, naming, edge-case handling); (6) final integration/synthesis of N parallel outputs (merge plans, integration sessions, who owns the merge); (7) supervision ergonomics for a NON-TECHNICAL solo operator running 3-5 sessions: minimal dashboards, check-in cadence, early-warning signals of a bad cut.

RESEARCH REQUIREMENTS:

1. Academic research
- papers (task decomposition / planner-executor / HTN planning for LLM agents, plan-and-solve, dynamic re-planning)
- meta analyses
- benchmarks (decomposition quality, multi-agent coding benchmarks measuring interference between parallel workers)

2. Industry practice
- startups
- unicorns
- leading companies (Anthropic, OpenAI, Cognition, Cursor, Factory, Devin: how their products CUT work, not just run it)
- open source projects (task-graph orchestrators, spec-first / contract-first agent workflows)

3. Competitive landscape
- direct competitors
- indirect competitors
- substitutes (human PM techniques adapted to AI: WBS, INVEST criteria for user stories, Team Topologies fracture planes, Conway's-law-aware splitting)

4. Failure analysis
- failed attempts
- abandoned projects
- lessons learned (bad cuts discovered at merge time; hidden coupling via implicit decisions; re-decomposition thrash)

5. Emerging trends
- developments from the last 12 months
- frontier approaches (self-critiquing planners, coupling metrics for LLM task graphs, automated merge-conflict-of-decisions detection)

6. Implementation recommendations
- actionable framework: a reusable DECOMPOSITION CHECKLIST/prompt an operator can give the AI ("cut this task so that..."), a pre-flight independence test, and a lifecycle protocol for promote/merge/integrate
- roadmap
- priorities

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

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LLM task decomposition planner executor hierarchical task networks dynamic replanning benchmark 2025 paper"},{"q":"LLM agents task decomposition survey planning execution replanning 2025 arXiv"},{"q":"Plan-and-Solve prompting LLM task decomposition paper"},{"q":"decomposition quality benchmark LLM agents task graphs hidden dependencies parallel coding benchmark"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

I’ll focus on the unresolved layer: high-quality task cuts and part lifecycle, grounding the synthesis in agent-planning research, benchmarks, industry/open-source practice, and operator-ready protocols.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Anthropic Claude Code subagents best practices plan tasks official docs"},{"q":"OpenAI Codex cloud coding agent delegate tasks official blog 2025 2026"},{"q":"Cursor background agents parallel coding tasks official docs 2025"},{"q":"Cognition Devin AI engineer task planning official product docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:anthropic.com engineering building effective agents workflows routing parallelization orchestrator evaluator optimizer official"},{"q":"site:docs.anthropic.com claude-code subagents official docs"},{"q":"site:docs.anthropic.com claude-code common workflows plan mode official docs"},{"q":"site:developers.openai.com codex cloud parallel tasks official"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LLM dynamic replanning task decomposition agent benchmark 2026 arxiv AdaPlanBench"},{"q":"LLM planner executor dynamic replanning framework CART traceable zero-shot planning 2025"},{"q":"LLM task decomposition non-redundancy solvability completeness multi-agent planning paper AOP"},{"q":"LLM multi-agent task decomposition allocation reward model solvability completeness non-redundancy"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"parallel coding agents merge conflicts hidden coupling benchmark 2025 2026"},{"q":"multi-agent software engineering benchmark interference parallel agents code repository 2025"},{"q":"AI coding agents benchmark real world repositories long horizon planning merge conflicts 2026"},{"q":"SWE-bench verified agent planning benchmark code agents large scale repository 2025 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Team Topologies fracture planes team boundaries official"},{"q":"INVEST user stories independent negotiable valuable estimable small testable source Bill Wake"},{"q":"work breakdown structure decomposition PMBOK PMI official"},{"q":"Conway's Law modularity team structure software architecture original"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"bounded context domain driven design strategic design software boundaries official"},{"q":"information hiding modularity Parnas 1972 software design criterion modules"},{"q":"coupling cohesion software modularity hidden dependencies data ownership service boundaries"},{"q":"architectural decision records ADR implicit decisions software architecture documentation"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LangGraph multi-agent supervisor handoffs parallel workflow official docs"},{"q":"Microsoft AutoGen multi agent framework planning tasks official docs 2025"},{"q":"CrewAI flows conditional logic parallel task dependencies official docs"},{"q":"LlamaIndex Workflows agent orchestration events steps official docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Factory AI droids software development task planning official"},{"q":"Factory AI platform droids documentation agent planning parallel coding"},{"q":"Factory Code Droid AI software engineer official docs"},{"q":"Factory.ai Droid task decomposition parallel agents"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:docs.cursor.com cloud agents official docs parallel agents worktrees plan mode"},{"q":"site:docs.cursor.com background agents Cursor docs agent branch pull request"},{"q":"Cursor 2.0 agents parallel worktrees official blog"},{"q":"Cursor Agent documentation planning tasks official"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"GitHub Copilot coding agent assign issue pull request planning official docs 2025 2026"},{"q":"GitHub Copilot agent mode plan tasks official docs"},{"q":"GitHub Copilot coding agent creates pull request in background official documentation"},{"q":"GitHub Copilot agent mode multi-agent planning VS Code official docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn737361search3"},{"ref_id":"turn737361search1"},{"ref_id":"turn737361search9"},{"ref_id":"turn737361search17"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn340473search8"},{"ref_id":"turn340473search0"},{"ref_id":"turn340473search1"},{"ref_id":"turn340473search2"},{"ref_id":"turn340473search12"},{"ref_id":"turn340473search18"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn340473search13"},{"ref_id":"turn340473search16"},{"ref_id":"turn340473search21"},{"ref_id":"turn340473search23"},{"ref_id":"turn340473search25"},{"ref_id":"turn340473search10"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"context rot LLM long context performance degradation Chroma 2025"},{"q":"long context language models context rot performance degrade hidden dependencies 2025 research"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LLMCompiler DAG task decomposition parallel function calling arXiv 2023 2024"},{"q":"DAG plan language model agents parallel execution task decomposition 2024 2025"},{"q":"LLM task graph planner dependency graph decomposition independent subtasks parallel agents"},{"q":"Graph of Thoughts LLM problem decomposition graph reasoning paper"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn287493search4"},{"ref_id":"turn287493search3"},{"ref_id":"turn287493search2"},{"ref_id":"turn287493search5"},{"ref_id":"turn287493search25"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"screenshot":[{"ref_id":"turn479629view4","pageno":1}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn382532search18"},{"ref_id":"turn657650search2"},{"ref_id":"turn657650search20"},{"ref_id":"turn657650search0"},{"ref_id":"turn382532search5"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

# A. Executive Summary

The core conclusion: **a high-quality AI decomposition is not a list of subtasks. It is a cut through a dependency graph, a write-ownership map, and a decision-ownership map.** For parallel LLM coding or knowledge-work sessions, the real enemy is not only Git merge conflict. It is **hidden coupling through implicit decisions**: API shape, naming, edge-case semantics, data ownership, copy style, test fixtures, migration order, and “obvious” assumptions each agent silently makes.

The strongest current pattern is:

> **Graph first → contract freeze → part passports → pre-flight collision test → parallel execution → lifecycle checkpointing → fresh-context review → one integration owner.**

Recent academic work is converging on this. Co-Coder frames multi-agent coding as **graph partitioning**, using static dependency graphs and cohesion-aware scheduling because naïve parallelism can become slower or worse when agents split interdependent files. STORM argues that isolated worktrees avoid clobbering but push semantic conflict discovery to merge time; it instead tracks read/write dependencies and rejects unsafe writes earlier. CooperBench shows that even frontier coding agents can perform worse in cooperative settings than solo, with failures driven by broken expectations, poor commitments, and communication overload. citeturn426508view0turn426508view1turn391584view0turn391584view1

Industry practice points the same way. Anthropic’s multi-agent research system works well for wide, independent research because an orchestrator delegates bounded subproblems to subagents and synthesizes results, but their own postmortems emphasize clear boundaries, effort budgets, tool guidance, and observability. Cognition’s public writing is more conservative for coding: parallel write agents tend to fragment implicit decisions, so their near-term successful pattern is often **read-only subagents, clean-context review, and manager-controlled map-reduce**, not free-for-all parallel implementation. citeturn941753view0turn974135view0

For your operator model — non-technical solo founder running 3–5 parallel Claude Code sessions — the practical answer is **not “spawn more agents.”** It is to force the lead AI to produce:

1. a dependency map;
2. a collision table;
3. a frozen contract file;
4. one “part passport” per session;
5. explicit promote / merge / re-cut rules;
6. a final integration plan owned by one integration session, not by the implementers.

A decomposition should be rejected before spawning sessions if any two parts share write files, share unresolved interface decisions, require the same naming/UX/domain semantics, or cannot be locally verified without another part’s output.

---

# B. Key Findings

## 1. The best decomposition cut is dependency-aware, not task-title-aware

The state-of-the-art direction is **cohesion-aware partitioning**: build a graph of files, modules, APIs, data objects, or claims; identify hubs; then assign highly connected work to the same agent. Co-Coder formalizes this as a graph-partitioning problem and reports that naïve file-based or “more agents” approaches can degrade speed and quality when cross-agent dependencies are high. citeturn426508view0

For a solo operator, this means the lead session must not simply say:

> “Agent 1: backend, Agent 2: frontend, Agent 3: tests.”

It should instead say:

> “These files / concepts / APIs form one cohesion cluster. These other files form another. This shared hub must have exactly one owner or a frozen contract before parallel work starts.”

The useful unit is a **cohesion cluster**, not an apparent subtask.

---

## 2. Worktree isolation is necessary but insufficient

Worktrees prevent agents from overwriting each other’s files. They do **not** prevent two agents from making incompatible semantic choices. STORM’s framing is useful: isolated environments push conflict detection to merge time, while many real conflicts are semantic, not textual. STORM instead tracks whether an agent’s write is still safe given the read dependencies it used. citeturn426508view1

This is a critical distinction for Claude Code / Codex / Cursor-style workflows:

| Conflict type | Worktrees catch? | Real risk |
|---|---:|---|
| Same file edited differently | Usually yes | Low-to-medium |
| API signature drift | Sometimes | High |
| Different naming conventions | No | Medium |
| Different error semantics | No | High |
| Different mock/test fixture assumptions | Sometimes | High |
| Two agents create duplicate abstractions | No | High |
| Two agents make conflicting product decisions | No | Very high |

So the pre-flight check should focus less on “will Git merge?” and more on **“will these agents make the same decision independently?”**

---

## 3. Current benchmarks show that multi-agent coding coordination is still fragile

SWE-bench and Multi-SWE-bench measure real issue-resolution ability across repositories and languages, but they are primarily single-agent or method-comparison benchmarks, not clean tests of parallel-worker interference. CooperBench is more directly relevant because it evaluates cooperative coding where two agents must implement related features in the same repository. It finds that strong individual coding ability does not reliably translate into team coordination; common failures include duplicate work, broken expectations, unfulfilled commitments, and unproductive communication. citeturn391584view3turn297367view0turn391584view0turn391584view1

Implication: **do not assume that frontier agents know how to cooperate just because they can code.** Cooperation must be designed into the task structure.

---

## 4. LLM planning helps, but unstructured LLM planning is not enough

Plan-and-Solve, Least-to-Most prompting, Tree of Thoughts, Graph of Thoughts, and related methods all support the general idea that explicit decomposition improves reasoning. But most of those methods were designed for sequential reasoning or search, not for safe parallel write execution. citeturn278809search1turn278809search2turn278809search7turn896936search3

Recent planning surveys are blunt: LLMs are useful planners, but standalone LLM planning remains brittle on long-horizon tasks, tool constraints, state tracking, and refinement. Current best practice is to combine LLM planning with external structure: graphs, schemas, verifiers, state machines, benchmarks, and explicit constraints. citeturn878970search2turn878970search3turn878970search8

So the planner prompt must force the model to expose:

- candidate cuts;
- dependency assumptions;
- pairwise collision risks;
- unresolved contracts;
- verification locality;
- merge order.

A planner that produces only “three tasks” has not done the job.

---

## 5. Dynamic re-decomposition should be local, not chaotic

The emerging pattern in agent planning is **scoped replanning**. Task-Decoupled Planning decomposes long-horizon work into a DAG of subgoals with scoped contexts, then replans locally when feedback invalidates part of the plan. WebDART similarly focuses on dynamic decomposition and replanning when new information makes the initial plan suboptimal. citeturn426508view2turn896936search0

For parallel LLM sessions, the rule should be:

> Re-cut only the affected part unless a shared contract is invalidated.

A bad lifecycle protocol causes **re-decomposition thrash**: every new discovery reshuffles all sessions. A good protocol contains the blast radius.

---

## 6. Product practice is converging on “plan first, isolate, verify, review”

Public product surfaces from Codex, GitHub Copilot coding agent, Google Jules, Factory Missions, Devin, Cursor-style worktrees, and Anthropic’s Claude Code patterns all point toward the same operational ingredients: isolated environments, upfront plans, branch/worktree separation, repo instructions, progress visibility, test evidence, and review before merge. citeturn941753view1turn941753view2turn114685view3turn114685view4turn114685view1turn114685view2turn941753view4

But public materials rarely expose the exact internal “cut algorithm.” The visible best practice is therefore less “copy Devin’s planner” and more:

- force an explicit plan before code;
- require source/code citations for the plan;
- make the agent state what it will not touch;
- use isolated branches/worktrees;
- require test evidence;
- review with fresh context;
- keep final integration under one owner.

---

## 7. Human PM techniques remain useful, but need AI-specific hardening

Classic WBS decomposition, INVEST user-story criteria, Team Topologies fracture planes, and Conway’s-law-aware splitting are still useful. WBS gives hierarchy; INVEST emphasizes independent/testable slices; Team Topologies emphasizes natural fracture planes and cognitive load; inverse Conway thinking warns that communication structure shapes architecture. citeturn523889search2turn523889search1turn132150search0turn132150search1turn132150search9turn523889search11

But AI sessions are not normal human teams. They have weaker long-term memory, weaker implicit social repair, and a tendency to invent local conventions. So every human PM idea needs an AI-specific add-on:

| Human PM concept | AI-specific hardening |
|---|---|
| WBS | Add write ownership and dependency graph |
| INVEST independence | Add pairwise collision test |
| Team Topologies fracture planes | Add explicit file/API/data ownership |
| Conway’s Law | Make agent communication topology match desired architecture |
| Definition of done | Require external verification logs, not self-certification |
| Backlog item | Convert to a “part passport” with forbidden files and escalation triggers |

---

# C. Contradictions & Debates

## 1. “Multi-agent systems work” vs “don’t build multi-agents”

Both are true in different regimes.

Anthropic reports strong results for multi-agent research when the task is broad, search-heavy, and decomposable into independent information-gathering threads. They emphasize orchestrator-worker architecture, clear subagent boundaries, and synthesis by the lead agent. citeturn941753view0

Cognition is more skeptical about parallel write agents in coding because multiple implementers make incompatible implicit choices. Their successful pattern is closer to **manager + read-only helpers + clean-context reviewer + controlled map-reduce**, with parallel writing used cautiously. citeturn974135view0

Resolution:

| Task type | Parallelism recommendation |
|---|---|
| Wide research | Parallelize aggressively |
| Independent code leaves | Parallelize cautiously |
| Shared architecture/API | Do not parallelize until contract is frozen |
| Refactor touching hub files | Usually serialize |
| Final integration | Single owner |
| Review | Parallel review is useful if criteria are explicit |

---

## 2. “Let agents communicate” vs “communication does not solve coordination”

CooperBench suggests that communication alone does not reliably fix coordination. Agents may talk but still duplicate work, make wrong assumptions, or fail to honor commitments. citeturn391584view1turn391584view2

Communication must therefore be **structured**:

Bad:

> “Coordinate with the other sessions.”

Good:

> “You may only ask for: API signature, naming decision, test fixture ownership, merge blocker. Every message must produce one of: decision, blocker, or contract update.”

The useful artifact is not chat. It is a **decision ledger**.

---

## 3. Worktrees vs shared-state systems

Worktrees are operationally simple and fit today’s Claude Code / Cursor / Codex workflows. But STORM highlights their weakness: they defer semantic conflicts until merge. Shared-state systems can detect stale assumptions earlier by tracking read/write dependencies. citeturn426508view1

Practical synthesis:

- Use worktrees for isolation.
- Add a manual or scripted **state-dependency ledger**:
  - files read;
  - files written;
  - contracts assumed;
  - decisions made;
  - tests run.
- If a session writes based on stale assumptions, pause and replan before merge.

---

## 4. Layer-based cuts are intuitive but often bad

LLMs love layer cuts:

- “Agent 1 backend”
- “Agent 2 frontend”
- “Agent 3 tests/docs”

This often fails because one feature’s semantics span all layers. The frontend agent invents UX states; the backend agent invents error names; the test agent invents expected behavior. The merge is textually clean but semantically incoherent.

Layer cuts are safe only when:

- API/schema is frozen;
- UI behavior is specified;
- error semantics are specified;
- test fixtures are owned;
- one layer is truly independent.

For most product work, **vertical domain/data cuts** are safer:

- “Agent 1 owns billing invoice export end-to-end.”
- “Agent 2 owns account deletion end-to-end.”
- “Agent 3 owns audit logging end-to-end.”

---

## 5. Upfront planning vs dynamic replanning

One-shot upfront plans are brittle when the agent discovers hidden constraints. Pure dynamic planning causes thrash. Current research trends favor hybrids: upfront DAG / contract / decomposition, then local replanning when feedback invalidates a node. citeturn426508view2turn896936search0

Operational rule:

> Replan locally unless a frozen shared contract is wrong. If the shared contract is wrong, pause all affected sessions and run a contract-resolution micro-task.

---

# D. Best Practices

## D1. Preferred decomposition strategies, ranked

### 1. Dependency-graph-first cut

Best for codebases, technical docs, migrations, and systems with real structure.

The lead agent should construct:

- file/module dependency graph;
- API call graph where possible;
- shared configuration/migration/test-fixture map;
- “hub files” list;
- likely write set per part.

Then it should cut the graph so that cross-part edges are minimal. This aligns with the Co-Coder direction: parallelism pays when internally cohesive work stays together and cross-agent dependency cost is low. citeturn426508view0

Use when:

- codebase has meaningful modules;
- task spans several files;
- hidden imports or shared fixtures are likely.

Avoid when:

- the repository is tiny;
- the task is mostly product judgment;
- the graph is too noisy to matter.

---

### 2. Interface / contract-first cut

Best when parallel parts must interact but the boundary can be frozen.

Before spawning writers, create a contract artifact:

```md
# Contract: <task-id>

## Public API / schema / interface
- Function names:
- Types:
- Routes:
- Events:
- Error names:
- State machine:
- UX states:
- Test fixtures:

## Decisions frozen
- Naming:
- Edge cases:
- Backward compatibility:
- Migration order:
- Logging/telemetry:
- Security/privacy constraints:

## Who owns changes
- Contract owner:
- Allowed editors:
- Review required for:
```

Then sessions can work behind the contract.

Use when:

- frontend/backend split is unavoidable;
- multiple sessions depend on same schema;
- docs/research sections need consistent terminology.

Reject parallelization if the contract cannot be written clearly.

---

### 3. Data-ownership / domain-ownership cut

Best default for product code.

Each part owns one domain object, workflow, data table, or user journey end-to-end. This reduces semantic leakage because one session owns the relevant decisions.

Examples:

| Bad cut | Better cut |
|---|---|
| Backend / frontend / tests | “User invitation flow” / “Billing export flow” / “Admin audit flow” |
| Models / services / UI | “Subscription cancellation end-to-end” |
| Literature review / recommendations / summary | “Evidence for intervention A” / “Evidence for intervention B” / “Risk & implementation synthesis” |

This maps well to Team Topologies fracture-plane thinking: split along natural seams that reduce coordination load. citeturn132150search0turn132150search1turn132150search9

---

### 4. Research facet / source-ownership cut

Best for knowledge work.

For research, hidden coupling is not file conflicts. It is claim overlap, source overlap, and inconsistent interpretation.

Use source/facet ownership:

| Part | Owns |
|---|---|
| Academic evidence | Papers, surveys, benchmarks |
| Industry practice | Product docs, company posts, case studies |
| Failure analysis | postmortems, abandoned approaches, negative benchmarks |
| Competitive landscape | direct/indirect/substitute mapping |
| Implementation framework | synthesis, checklist, protocol |

Require each part to return:

- claims;
- sources;
- uncertainty;
- contradictions;
- implications;
- “do not use this source for X” warnings.

The integration session must synthesize, not concatenate.

---

### 5. Layer-based cut

Use only when contracts are already frozen.

Layer cuts are acceptable for:

- implementing generated client from fixed OpenAPI schema;
- writing docs after code is stable;
- adding tests after behavior is specified;
- UI skinning where business logic is untouched.

Layer cuts are risky for feature discovery and new product behavior.

---

## D2. Cut Quality Score

Before spawning sessions, score the proposed cut. Reject or re-cut below 80.

| Dimension | Weight | Pass condition |
|---|---:|---|
| Write independence | 25 | No shared write files except explicitly owned contract files |
| Decision independence | 20 | No unresolved naming/API/product/edge-case decision is owned by two parts |
| Dependency cohesion | 15 | High internal cohesion; low cross-part dependency |
| Contract clarity | 15 | Interfaces, schemas, errors, UX states, and test fixtures are frozen where shared |
| Verification locality | 10 | Each part can prove progress without sibling output |
| Merge order clarity | 5 | Integration order is obvious and reversible |
| Operator observability | 5 | Non-technical operator can see status, risk, blockers |
| Scope containment | 5 | Each part has clear non-goals and forbidden files |

Interpretation:

| Score | Action |
|---:|---|
| 90–100 | Spawn sessions |
| 80–89 | Spawn, but monitor closely |
| 65–79 | Re-cut or freeze more contracts |
| <65 | Do not parallelize |

---

## D3. Pre-flight Independence Test

Run this before opening parallel sessions.

### Step 1 — Produce the task graph

For code:

- list likely files/modules touched;
- list shared configs, migrations, schemas, generated files, fixtures;
- list public interfaces;
- list tests likely to fail;
- identify hub files.

For knowledge work:

- list claims;
- list source classes;
- list concepts needing consistent definitions;
- list output sections;
- list likely contradictions.

---

### Step 2 — Create the collision table

```md
| Pair | Shared write files | Shared decisions | Shared test/source fixtures | Hidden coupling risk | Mitigation | Pass? |
|---|---|---|---|---|---|---|
| A × B |  |  |  |  |  | Yes/No |
| A × C |  |  |  |  |  | Yes/No |
| B × C |  |  |  |  |  | Yes/No |
```

Fail the cut if any pair has:

- same write file;
- same API/schema decision;
- same migration/config/test fixture;
- same UX state;
- same terminology decision;
- unclear merge order;
- verification that depends on sibling completion.

---

### Step 3 — Ask the adversarial question

For every pair of parts:

> “How could A silently break B even if they never edit the same file?”

Common answers:

- A changes exported type semantics.
- B assumes old behavior.
- Both create different names for the same concept.
- Both add competing helper functions.
- Both alter shared fixture expectations.
- One changes documentation terminology.
- One changes default behavior without updating tests.
- One adds migration assumptions the other violates.

If the lead AI cannot answer this question, it has not understood the cut.

---

### Step 4 — Freeze shared decisions

Before spawning, freeze:

- API names;
- type names;
- route names;
- error names;
- database fields;
- migration order;
- UX states;
- logging/telemetry names;
- test fixture ownership;
- source/citation ownership for research tasks;
- style/terminology.

This can be a short Markdown file. The point is not bureaucracy. The point is preventing three agents from making the same choice differently.

---

### Step 5 — Rehearse the merge

Ask:

> “If all parts succeed, what exact order are they integrated in, and what tests/checks run after each merge?”

If the answer is vague, the cut is bad.

A good integration order usually looks like:

1. contract/schema/stub;
2. shared infrastructure;
3. leaf implementations;
4. tests;
5. docs/synthesis;
6. final review.

---

## D4. Part Passport

Each parallel session gets a part passport.

```md
# Part Passport: <part-id>

## Mission
One-sentence outcome.

## Owned artifacts
Files, sections, data objects, modules, or source classes this session may modify.

## Read-only context
Files, docs, contracts, or sources this session may inspect but not change.

## Forbidden artifacts
Files, modules, configs, migrations, docs, or decisions this session must not touch.

## Inputs
Contracts, assumptions, source material, existing APIs, accepted decisions.

## Outputs
Expected diff, report section, tests, decision note, or integration packet.

## Frozen contracts
API names, schemas, UX states, terminology, edge-case behavior.

## Local verification
Commands/checks/evidence this session must produce.

## Non-goals
Explicitly out of scope.

## Escalation triggers
Pause and report if:
- owned files are insufficient;
- forbidden files need edits;
- contract appears wrong;
- sibling output is required;
- scope expands materially;
- tests fail for reasons outside this part.

## Done criteria
Evidence-based, externally reviewable. No self-certification.
```

---

# E. Failure Modes

| Failure mode | Symptom | Root cause | Mitigation |
|---|---|---|---|
| **Fake independence** | Agents edit different files but behavior conflicts | Hidden shared decision | Decision ledger + pairwise collision test |
| **Hub-file collision** | Everyone needs same config/schema/router/index | Bad graph cut | One owner or serial contract task |
| **Layer-split incoherence** | Frontend/backend/tests disagree | Semantics split across agents | Prefer vertical/domain cuts |
| **Duplicate abstractions** | Two helpers solve same problem differently | No ownership map | Define abstraction owner |
| **Implicit API drift** | Tests fail after clean merge | Contract not frozen | Contract-first artifact |
| **Re-decomposition thrash** | Plan changes every checkpoint | No replanning threshold | Local replan only unless contract breaks |
| **Communication theater** | Long agent updates, no decisions | Unstructured coordination | Messages must update blocker/decision/contract |
| **Premature promotion** | Tiny subtask becomes standalone bureaucracy | Overreaction to normal complexity | Promote only on defined triggers |
| **Late promotion** | One part balloons and blocks integration | Scope expansion ignored | Checkpoint scope ratio |
| **Merge-time archaeology** | Integrator cannot tell why choices were made | No decision ledger | Require integration packet |
| **Review rubber-stamp** | Implementer says “done” and gets merged | Same context self-certifies | Fresh-context review gate |
| **Knowledge-work contradiction** | Final report has inconsistent definitions | Source/facet overlap unmanaged | Claim/source ownership matrix |
| **Tool over-automation** | Complex orchestration hides bad cut | Workflow engine without contracts | Decomposition quality gate before tooling |

The most expensive failure is **semantic clean merge, product-broken result**. Git says yes; reality says no.

---

# F. Strategic Recommendations

## F1. Use a four-artifact operating system

For every complex task, create four artifacts before spawning sessions.

### 1. Task Map

Shows the whole problem:

```md
- Goal:
- Non-goals:
- Candidate decomposition cuts:
- Chosen cut:
- Why this cut:
- Why rejected cuts were rejected:
- Dependency graph:
- Shared contracts:
- Integration order:
```

### 2. Contract File

Owns shared decisions.

```md
contracts/<task-id>.md
```

This is the canonical source for names, schemas, API shape, UX states, edge cases, and terminology.

### 3. Part Passports

One per session.

```md
parts/<task-id>/<part-id>.md
```

This is what each Claude Code session receives.

### 4. Integration Ledger

Maintained by lead/integration session.

```md
integration/<task-id>.md
```

Tracks:

- branch/worktree name;
- part status;
- files changed;
- tests run;
- decisions made;
- merge order;
- unresolved risks;
- final synthesis notes.

---

## F2. Use lifecycle states, not ad hoc judgment

Each part should move through explicit states.

```text
Candidate
  ↓
Contracted
  ↓
Spawned
  ↓
In-flight
  ↓
Checkpointed
  ↓
Ready-for-review
  ↓
Fresh-context-reviewed
  ↓
Integrated
  ↓
Archived
```

Exception states:

```text
Needs-recut
Promoted-to-standalone
Merged-with-part-X
Serialized
Abandoned
```

This prevents the operator from treating every agent update as a one-off judgment call.

---

## F3. Promotion protocol

Promote a part into a standalone task when it becomes too large or too decision-heavy to remain a leaf.

### Promotion triggers

Promote if any of these occur:

- part scope becomes more than ~2× the original estimate;
- part needs a new API/schema/domain decision;
- part requires modifying forbidden files;
- part becomes a prerequisite for multiple other parts;
- part cannot be verified locally;
- agent discovers a product/architecture ambiguity;
- sibling sessions are waiting on its decision.

### Promotion steps

```md
# Promotion Packet

## Original part
<part-id>

## Why promotion is needed
Concrete trigger.

## Work completed so far
Files, notes, findings, tests.

## Decisions already made
Explicit list.

## Decisions not yet made
Explicit list.

## Downstream consumers
Which parts depend on this.

## Proposed new task
Goal, owned files, non-goals, verification, done criteria.

## Parent task impact
What pauses, what continues, what must be re-cut.
```

After promotion:

- freeze the parent part;
- create a new contract or architecture task;
- update dependent part passports;
- resume only unaffected parts.

---

## F4. Workstream merge protocol

Merge two converging workstreams when they are actually solving the same decision or touching the same cohesion cluster.

### Merge triggers

- both sessions need the same API/schema decision;
- both created overlapping abstractions;
- both edited or need the same hub file;
- both define the same terminology differently;
- both modify the same test fixtures;
- integration order becomes ambiguous.

### Merge steps

```md
# Workstream Merge Packet

## Streams being merged
A, B

## Collision discovered
Specific files/decisions/contracts.

## Stop condition
Both sessions stop writes.

## Decision comparison
A chose:
B chose:

## Canonical decision
Chosen approach and why.

## Surviving branch/worktree
Which branch becomes base.

## Transplanted work
What useful work from other stream is copied over.

## Review required
Fresh-context review focus.

## Tests/checks
Commands and expected evidence.
```

Critical rule:

> Do not let both sessions “resolve it themselves.” Appoint one merge owner.

---

## F5. Integration protocol

Final integration should be owned by a dedicated lead/integration session.

Implementers submit **integration packets**, not final truth.

```md
# Integration Packet: <part-id>

## Summary
What changed.

## Files changed
List.

## Contracts used
List contract versions or decisions.

## Decisions made
List.

## Tests run
Commands + outcomes.

## Known risks
List.

## Suggested merge order
Before/after dependencies.

## Rollback notes
How to revert safely.
```

Integration order:

1. merge frozen contracts/stubs;
2. merge shared infrastructure;
3. merge lowest-dependency leaf parts;
4. run tests after each merge;
5. merge docs/synthesis;
6. run full verification;
7. fresh-context review;
8. final operator summary.

The integration session should actively look for:

- duplicate helpers;
- inconsistent names;
- mismatched edge cases;
- stale assumptions;
- tests that pass only in isolation;
- documentation contradicting behavior.

---

## F6. Supervision ergonomics for a non-technical solo operator

Use a single dashboard. Nothing more complicated than this:

```md
| Part | Status | Risk | Owned files/sections | Contract decisions | Last evidence | Blocker | Next action |
|---|---|---|---|---|---|---|---|
| A | In-flight | Green | ... | ... | tests passed / notes | none | continue |
| B | Checkpointed | Yellow | ... | needs API decision | partial diff | contract unclear | pause + resolve |
| C | Ready review | Green | ... | none | test logs | none | fresh review |
```

Risk colors:

| Risk | Meaning |
|---|---|
| Green | within passport, local verification possible |
| Yellow | scope or contract ambiguity, but contained |
| Red | forbidden files, shared decision, or blocked integration |

### Check-in cadence

Use event-based check-ins rather than constant chatter:

- after initial repo/task reconnaissance;
- before first write;
- when touching any contract/shared file;
- when local verification first passes/fails;
- before declaring ready for review;
- when escalation trigger fires.

For the operator, the only useful check-in format is:

```md
## Status
Green / Yellow / Red

## What changed
One paragraph.

## Evidence
Commands, files, sources, or diff summary.

## Decisions made
Bullets.

## Blockers
Bullets.

## Contract changes requested
Yes/No. If yes, exact change.

## Continue / Pause recommendation
One sentence.
```

### Early-warning signals of a bad cut

Pause immediately if a session says or implies:

- “I need to edit a file outside my owned files.”
- “I assumed the API would look like…”
- “I created a helper similar to…”
- “This depends on what another session decides.”
- “I changed the shared config/schema/router.”
- “I updated tests globally.”
- “I refactored while I was here.”
- “I could not run verification, but it should work.”
- “I renamed the concept for clarity.”
- “I need the other branch to pass tests.”
- “There are many small conflicts, but they seem easy.”
- “Done” without command output, source list, or diff summary.

For a non-technical operator, the most important rule is simple:

> If two sessions are about to make the same decision, pause both and create a contract-resolution micro-task.

---

## F7. Competitive landscape

### Direct competitors / tools

| Tool/product family | Relevant pattern | Limitation for your problem |
|---|---|---|
| Claude Code / Anthropic multi-agent patterns | Orchestrator-subagent, generator-verifier, agent teams, shared state patterns | Public guidance emphasizes patterns, not a full decomposition-quality metric |
| OpenAI Codex cloud/desktop | Multiple isolated coding agents, branch/worktree-style parallelism, logs/test evidence | Parallel task quality still depends on human/AI scoping |
| Cursor agents/worktrees | Convenient parallel branches/worktrees | Isolation does not solve semantic coupling |
| Devin / Cognition | Planning before coding, review, manager/child agent direction | Public writing is cautious about parallel write agents |
| Factory Missions | Structured missions, plan, milestones, dashboard, QA readiness | Works best when repo has strong automated verification |
| GitHub Copilot coding agent | Branch-based autonomous coding with logs and PR workflow | One repo/branch/PR style still requires careful task slicing |
| Google Jules | Plan-approval workflow in isolated VM | Publicly visible model is task-level, not necessarily multi-part cut optimization |

Sources support the general product-surface pattern: isolated environments, upfront plans, repo instructions, branch-based work, logs/test evidence, and review. citeturn941753view1turn941753view2turn114685view3turn114685view4turn114685view1turn114685view2turn974135view0

### Indirect competitors / orchestration frameworks

| Framework | Useful idea |
|---|---|
| LangGraph | Explicit graph/state machine with conditional edges and parallel tool execution |
| AutoGen | Group-chat and hierarchical multi-agent coordination |
| CrewAI Flows | Event-driven workflows, state, branching, visualization |
| OpenAI Agents SDK | Manager vs handoff patterns; avoid splitting unless instructions/tools/policy differ |

These are useful once the decomposition protocol is stable. They do not magically produce good cuts by themselves. citeturn931764view0turn931764view1turn931764view2turn931764view3

### Substitutes

| Substitute | When better |
|---|---|
| One strong agent, sequential execution | High coupling, unclear architecture |
| One writer + multiple read-only reviewers | Shared codebase, risky feature |
| Scripted workflow | More than ~15 independent chunks or repetitive operations |
| Human PM / architect first | Product semantics or architecture are unclear |
| Contract-first design task | Multiple future sessions depend on same API/schema |

---

# G. Action Plan

## G1. Pasteable decomposition prompt

Use this with the lead AI before spawning sessions.

```text
You are the decomposition lead for a complex coding/knowledge-work task.

Your job is NOT to make a generic task list.
Your job is to decide whether this task can be safely split into independent parallel LLM sessions, and if yes, produce collision-resistant part packets.

Context:
- Operator will run 3–5 parallel LLM sessions.
- Implementers must not self-certify done.
- Fresh-context review will happen later.
- Prefer fewer, truly independent parts over many coupled parts.
- Say DO NOT PARALLELIZE if the task is too coupled.

Task:
<PASTE TASK HERE>

Known constraints:
<PASTE EXISTING CONTEXT / NON-GOALS / REPO DETAILS HERE>

Produce the following output.

1. Task understanding
- Goal:
- Non-goals:
- Unknowns:
- Assumptions:

2. Dependency / coupling map
For code:
- likely files/modules touched
- shared configs/schemas/migrations/fixtures
- public interfaces
- hub files
- tests likely affected

For knowledge work:
- major claims
- source classes
- concepts needing consistent definitions
- output sections
- likely contradictions

3. Candidate decomposition cuts
Propose at least 3 candidate cuts:
- dependency-graph-first
- interface/contract-first
- domain/data-ownership or source/facet-ownership
Optionally include layer-based only if safe.

For each candidate:
- parts
- why this cut might work
- hidden coupling risks
- merge risks
- verification strategy
- score using the Cut Quality Score rubric

4. Selected cut
Choose the best cut or say DO NOT PARALLELIZE.
Explain why rejected cuts were rejected.

5. Pre-flight collision table
For every pair of selected parts, fill:
- shared write files/artifacts
- shared decisions
- shared tests/fixtures/sources
- hidden coupling risk
- mitigation
- pass/fail

6. Frozen contracts required before spawning
List every API/schema/name/error/UX/terminology/fixture/edge-case decision that must be fixed first.
If a contract is not yet knowable, create a serial contract-resolution task instead of parallelizing.

7. Part passports
For each part:
- mission
- owned files/artifacts/sections
- read-only context
- forbidden files/artifacts/decisions
- inputs
- outputs
- frozen contracts
- local verification
- non-goals
- escalation triggers
- done criteria
- exact seed prompt for the worker session

8. Integration plan
- merge/synthesis owner
- merge order
- checks after each merge
- final verification
- rollback risks

9. Lifecycle rules
Define specific triggers for:
- local replan
- global re-cut
- part promotion
- workstream merge
- serialization
- abandonment

10. Operator dashboard
Produce a minimal status table the non-technical operator can maintain.

Self-critique:
Before finalizing, attack your own decomposition.
Ask:
- Where is hidden coupling likely?
- Which pair of parts is most dangerous?
- Which implicit decisions could diverge?
- Which part is most likely to grow?
- What would make this cut fail at merge time?

Revise the plan once after the self-critique.
```

---

## G2. Pre-flight checklist

Before starting parallel sessions, require all answers to be “yes.”

```md
# Pre-flight Independence Checklist

## Graph / ownership
- [ ] Each part has exclusive owned files/artifacts/sections.
- [ ] Shared write files have exactly one owner.
- [ ] Hub files are owned by one part or handled serially.
- [ ] Cross-part dependencies are listed.

## Contracts
- [ ] Shared API/schema/type names are frozen.
- [ ] Error semantics are frozen.
- [ ] UX states/copy terminology are frozen where relevant.
- [ ] Test fixture ownership is defined.
- [ ] Knowledge-work definitions and source ownership are defined.

## Verification
- [ ] Each part can run local checks without sibling completion.
- [ ] Each part has explicit evidence requirements.
- [ ] A fresh-context reviewer can verify the part from its packet.

## Lifecycle
- [ ] Promotion triggers are defined.
- [ ] Merge triggers are defined.
- [ ] Replan triggers are defined.
- [ ] Integration order is defined.

## Operator ergonomics
- [ ] Dashboard has one row per part.
- [ ] Check-in format is standardized.
- [ ] Early-warning signals are listed.

Decision:
- [ ] Spawn sessions
- [ ] Re-cut
- [ ] Serialize
- [ ] Create contract-resolution task first
```

---

## G3. Lifecycle protocol

### Local replan

Use when a part discovers new information that affects only itself.

```text
Part pauses → reports discovery → updates passport → lead approves local replan → part continues.
```

Examples:

- file name differs from expected;
- local test path changed;
- source quality weaker than expected;
- implementation approach needs adjustment but contract unchanged.

---

### Global re-cut

Use when the original decomposition boundary is invalid.

```text
Affected parts pause → lead updates dependency map → new collision table → new part passports → resume only after contract update.
```

Triggers:

- shared contract wrong;
- two parts need same file/decision;
- merge order no longer clear;
- one part becomes prerequisite for others.

---

### Promotion

Use when one part becomes a standalone task.

```text
Part pauses → creates promotion packet → lead freezes parent assumptions → new standalone task created → dependent parts updated.
```

---

### Workstream merge

Use when two parts converge.

```text
Both pause → compare decision ledgers → choose canonical approach → one merge owner → transplant useful work → fresh review.
```

---

### Integration

Use when parts are ready.

```text
Integration owner receives packets → merges one at a time → runs checks after each → resolves semantic conflicts → fresh-context review → final synthesis.
```

---

## G4. Minimal tooling roadmap

### Week 1 — Manual but disciplined

Create templates:

```text
TASK_MAP.md
CONTRACT.md
PART_PASSPORT.md
INTEGRATION_LEDGER.md
STATUS_DASHBOARD.md
```

Adopt the rule:

> No parallel session starts without a part passport and collision table.

---

### Weeks 2–4 — Add lightweight dependency inspection

For codebases, add language-specific graph tools where appropriate:

| Stack | Tools to consider |
|---|---|
| TypeScript/JavaScript | dependency-cruiser, madge, ts-prune, eslint boundaries |
| Python | pydeps, import-linter, modulegraph-style import inspection |
| General repo search | ripgrep, git grep, tree, cloc |
| Architecture decisions | ADR Markdown files |
| API contracts | OpenAPI, JSON Schema, protobuf, TypeScript types |
| Tests | one command per part + full integration command |

For knowledge work, add:

```md
SOURCE_MATRIX.md
CLAIM_LEDGER.md
CONTRADICTIONS.md
TERMINOLOGY.md
```

---

### Month 2 — Add evaluation

Track historical decomposition quality.

```md
| Task | Predicted risky pairs | Actual conflicts | Hidden decisions found | Recut needed? | Merge pain 1–5 | Notes |
|---|---|---|---|---|---|---|
```

Useful metrics:

- number of shared files touched unexpectedly;
- number of contract changes after spawn;
- number of pairwise collisions discovered mid-flight;
- number of local replans;
- number of global re-cuts;
- review defects per part;
- integration time vs implementation time.

The goal is to make the lead AI better at predicting coupling.

---

### Month 3 — Semi-scripted orchestration

Only after the manual protocol works, consider wrapping it in a lightweight script or task graph:

- generate folders for part passports;
- create branches/worktrees;
- maintain status dashboard;
- collect integration packets;
- run verification commands;
- diff forbidden-file violations;
- flag shared touched files.

Use LangGraph, CrewAI Flows, AutoGen, or OpenAI Agents SDK only if the process is already clear. Their value is state and orchestration; they do not replace the decomposition quality gate. citeturn931764view0turn931764view1turn931764view2turn931764view3

---

## G5. Practical decision rules

Use these blunt rules.

### Parallelize when:

- parts have separate owned files/artifacts;
- contracts are frozen;
- each part can verify locally;
- merge order is obvious;
- hidden decision overlap is low;
- failure of one part does not block all others.

### Do not parallelize when:

- task is mostly architecture discovery;
- same hub files are central;
- API/schema is unsettled;
- product semantics are unclear;
- tests cannot run locally;
- every part needs the same context;
- the lead AI cannot explain how the parts could collide.

### Use one writer + helpers when:

- broad codebase search is needed;
- many design alternatives exist;
- implementation touches shared architecture;
- quality matters more than speed.

### Use parallel research sessions when:

- sources are separable;
- claims can be independently verified;
- final synthesis has one owner;
- terminology is frozen.

---

# H. Sources

## Academic and benchmark sources

- **Co-Coder / cohesion-aware task partitioning** — supports dependency-graph-first decomposition and the claim that naïve multi-agent coding can degrade when cross-agent dependencies are high. citeturn426508view0  
- **STORM / shared workspace state management** — supports early conflict detection through read/write dependency tracking rather than waiting for worktree merge time. citeturn426508view1  
- **Task-Decoupled Planning** — supports DAG-based decomposition, scoped context, and local replanning. citeturn426508view2  
- **Agent Planning Benchmark** — supports the need to evaluate planning quality directly, including decomposition, constraints, infeasibility, and refinement. citeturn426508view3  
- **LLM workflow dependency trust paper** — supports the warning that apparent parallel subtasks may violate conditional independence through inconsistent symbols, styles, or assumptions. citeturn426508view4  
- **CooperBench** — supports the claim that cooperative coding agents fail through expectation, commitment, and communication failures even when individual ability is strong. citeturn391584view0turn391584view1turn391584view2  
- **SWE-bench and Multi-SWE-bench** — useful baselines for real repository issue resolution, but less direct than CooperBench for parallel interference. citeturn391584view3turn297367view0  
- **Plan-and-Solve, Least-to-Most, Tree of Thoughts, Graph of Thoughts, LLMCompiler, WebDART** — support explicit planning, decomposition, graph reasoning, parallelizable task execution, and dynamic replanning, while not by themselves solving safe parallel write ownership. citeturn278809search1turn278809search2turn278809search7turn896936search3turn896936search10turn896936search0  
- **LLM planning surveys** — support the conclusion that LLM planning is useful but brittle without external structure, verification, and constraints. citeturn878970search2turn878970search3turn878970search8  

## Industry and product-practice sources

- **Anthropic multi-agent research system** — supports orchestrator-worker delegation, subagent boundaries, effort budgets, observability, and synthesis. citeturn941753view0  
- **Anthropic multi-agent coordination patterns** — supports generator-verifier, orchestrator-subagent, agent teams, message bus, and shared-state trade-offs. citeturn941753view4  
- **OpenAI Codex cloud/desktop** — supports isolated task environments, parallel agents, logs, test evidence, and repo instruction files. citeturn941753view1turn941753view2  
- **GitHub Copilot coding agent and Google Jules** — support plan-first, isolated branch/VM, test/log visibility, and repo-instruction patterns. citeturn114685view3turn114685view4  
- **Devin / Cognition** — supports codebase-cited planning, plan approval, clean-context review, and a conservative view of parallel write agents. citeturn114685view2turn974135view0  
- **Factory Missions** — supports structured missions, plan/milestone decomposition, mission-control monitoring, and the importance of automated QA readiness. citeturn114685view1  

## Orchestration frameworks and PM substitutes

- **LangGraph, AutoGen, CrewAI Flows, OpenAI Agents SDK** — useful for explicit state graphs, group coordination, event-driven workflows, and manager/specialist handoff patterns. citeturn931764view0turn931764view1turn931764view2turn931764view3  
- **WBS, INVEST, Team Topologies, Conway’s Law / inverse Conway maneuver** — useful human decomposition analogues, but they need AI-specific additions: explicit artifact ownership, contract freeze, collision checks, and integration ledgers. citeturn523889search2turn523889search1turn132150search0turn132150search1turn132150search9turn523889search11
