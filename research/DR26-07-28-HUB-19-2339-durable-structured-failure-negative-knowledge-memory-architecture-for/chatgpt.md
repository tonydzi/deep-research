# DR26-07-28-HUB-19-2339 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Negative Knowledge Research"
type: deep-research-original
source: chatgpt

dr_id: DR26-07-28-HUB-19-2339
conversation_id: 6a499681-d288-83eb-b763-71c71ddd2184
url: "https://приватный чат"
research_started_at: 2026-07-04T23:27:15.942756Z
research_stopped_at: 2026-07-04T23:33:55.290832Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-07-28T23:39:08
vault_chat_note: "2026-07-05-negative-knowledge-research-6a499681.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
Conduct a deep research investigation on the following topic.



TOPIC: Best cheap architecture for durable "negative knowledge" / FAILURE MEMORY for personal coding agents — record approaches tried and FAILED so agents stop rediscovering dead-ends across sessions, using STRUCTURED (closed-vocabulary) fields, not fuzzy text similarity.



CONTEXT: Failures live nowhere durable today; a rejected-PROPOSAL journal exists but not a tried-and-FAILED technical record; dead-ends rediscovered every session. Reference: paper "Negative Knowledge as Failure-aware Shared Memory for AutoResearch" (arxiv 2606.21024, github.com/hch-wang/Negative_Knowledge) — Depth-1 records (task_id, attempted_route, observation, failure{layer,scope,degree,recommended_action,risk}, rationale, recommended_alternative) with CLOSED vocabularies; Depth-N pattern records; a curator writes, the agent MUST read+adopt/reject before next move ("forced grounding"); WARNING: naive similarity retrieval of a "similar past fix" actively misleads → retrieve by structured tags. Stack already uses SQLite (leads.db, system.db) + markdown cards (concept-*, reglament-*) + "SQL/grep before LLM".



OBJECTIVE: SOTA on failure/anti-pattern memory + the cheapest AK-47 way to add it: schema, when to write, safe retrieval (structured tags), forcing the agent to consult before retry, avoiding landfill.



RESEARCH REQUIREMENTS:

1. Academic — negative knowledge, failure-aware memory, Reflexion/self-refine, case-based reasoning, experience replay for LLM agents.

2. Industry — how AutoGPT-lineage, LangGraph, CrewAI, Reflexion impls store failed attempts; anti-pattern memories.

3. Competitive — structured failure DB vs vector memory vs rules files.

4. Failure analysis — misretrieval, stale "don't do X" that later became valid, over-suppression of exploration.

5. Emerging trends — last 12 months.

6. Implementation — minimal SQLite-table + md-card + grep-before-retry fitting "SQL/grep before LLM"; retrieval strictly by structured domain/layer tags; discipline that makes agents actually write records.



OUTPUT: exec summary; concrete minimal schema (columns + card template) tuned to context; read/write workflow + forced-grounding; anti-patterns (similarity-retrieval trap, stale-negative decay); sources.@Deep research
```

## Отчёт
# Durable Failure Memory for Personal Coding Agents

## Executive summary

The strongest current evidence points to a simple conclusion: if you want personal coding agents to stop rediscovering dead ends across sessions, the cheapest durable architecture is **not** a vector-memory add-on and **not** another free-text journal. It is a **small structured failure database** with **closed-vocabulary tags**, a **curator step** that writes one bounded record per meaningful failed route, and a **forced-grounding gate** that blocks retries until the agent has explicitly consulted relevant failure records and marked each as adopted or rejected. That core pattern is exactly what the recent *Negative Knowledge* paper proposes for AutoResearch, and it is one of the few approaches that makes failures durable, auditable, and operational instead of leaving them as transient logs or fuzzy reflections. In the paper’s benchmark, one structured negative-knowledge record improved retry performance over plain retry while using far fewer tokens than raw self-debug; in the harder setting, a deeper distilled record outperformed raw multi-round self-debug. citeturn10view0turn11view0

This recommendation is also consistent with the failure modes of mainstream agent stacks. Reflexion and Self-Refine improve behavior by looping on free-text feedback, but their memory units are linguistic reflections, not durable typed failure objects. LangGraph and LangChain expose persistent stores, JSON documents, namespaces, filters, semantic search, and AGENTS.md-style memory, but they do **not** ship a first-class failure-memory discipline. CrewAI’s current unified memory explicitly blends semantic similarity, recency, and importance, and auto-extracts facts after tasks; again, that is useful for broad recall, but it is the wrong default for “never retry this route under these conditions.” AutoGPT’s own issue history is blunt: vector memory was “not used effectively,” lacked metadata, and could trap the agent in loops where failed repeated attempts looked relevant and got retried. citeturn18view0turn39view0turn24view0turn24view2turn23view0turn22view0turn4view0turn27view0turn28view0

The practical answer for your stack is therefore an **AK-47 design**: one SQLite table for failure records, one SQLite table for exact-match tags, optional depth-N pattern rollups, and one markdown card per record with YAML front matter mirroring the structured fields. Retrieval should be **SQL/grep first, LLM later**: exact domain/layer/tool/error/route tags narrow the candidate set, then the model reads only the shortlisted cards. Do **not** let the model search the whole failure corpus by semantic similarity first. Recent work on stale-fact memory and memory-system evaluation shows why: similarity retrieval degrades with temporal distance, contradiction is often embedding-similar to the original fact, and systems without explicit maintenance or supersession return stale guidance. citeturn16search4turn16search0turn35view0

## What the research says about failure memory

The academic backdrop is older than LLM agents. Classical case-based reasoning treats problem solving as a cycle of **retrieve, reuse, revise, retain**, and it emphasizes that retrieval should be designed around the purpose of later reuse rather than around generic similarity alone. That matters here because your use case is not “find anything similar”; it is “prevent recurrence of known-bad routes under specific conditions.” The newer 2025 review on case-based reasoning for LLM agents argues that CBR helps when agents need explicit, structured, accountable past experience rather than generic latent recall. citeturn31view1turn31view0

The *Negative Knowledge as Failure-aware Shared Memory for AutoResearch* paper is the closest SOTA match to your problem statement. It defines a bounded negative-knowledge record with base fields `task_id`, `attempted_route`, `observation`, `failure`, `rationale`, and `recommended_alternative`; the nested `failure` object uses closed vocabularies for `layer`, `scope`, `degree`, `recommended_action`, and `risk`. The paper’s key architectural move is not just the schema. It is the workflow: a **separate curator** writes the record from frozen artifacts after an attempt ends, and the next research agent must **explicitly adopt or reject** relevant records before it proposes the next experiment. In the case study, every bank-aware experiment node had to populate `cites_bank`, `rejects_bank`, and `bank_use_rationale`, making the consultation auditable at proposal time. citeturn10view0turn11view0turn12view0turn26view0

Reflexion and Self-Refine are important baselines because they show the limit of in-session self-correction. Reflexion agents maintain free-text reflections in an episodic memory buffer and use those reflections in later trials; Self-Refine iteratively generates feedback and refines its own output. Both help, but neither gives you a durable cross-session technical failure ledger with typed constraints and explicit forced consultation. The *Negative Knowledge* paper says this directly: prior frameworks let failures inform the next in-session attempt, but the failed route itself is usually not preserved as a cross-agent, cross-task object. citeturn18view0turn39view0turn10view0

Recent work over the last year sharpens two additional lessons. First, “experience” memory is moving away from raw storage toward higher-level abstraction: a 2026 survey frames memory evolution as **storage → reflection → experience**, where mature systems increasingly distill trajectories into reusable rules or skills rather than merely storing them. Second, coding- and agent-memory papers are now paying much more attention to **maintenance**, **staleness**, and **retrieval fidelity** instead of only end-task success. The 2026 “agent-native memory” study decomposes memory into representation/storage, extraction, retrieval/routing, and maintenance, and finds that no one architecture wins universally; effectiveness depends on matching the representation and retrieval method to the workload, while similarity-based retrieval degrades with temporal distance and append-only stores tend to return stale facts. citeturn35view1turn35view0

## What frameworks do today and where they fall short

AutoGPT is a useful negative example. In its 2023 design discussions, the project described long-term memory as RAG over vector backends and explicitly stated that vector memory was not being used effectively; the maintainers said they would likely need **structured planning/task management** to create usable memory datapoints. A separate issue showed that the memory upsert path stored only raw text without useful metadata, and the user complaint was exactly your problem in miniature: the agent got stuck in loops because failed repeated attempts looked relevant and were retried. Historical configuration examples from the project exposed memory backends such as local, Pinecone, Redis, Weaviate, and Milvus, but that backend variety did not solve the representation problem. citeturn4view0turn27view0turn28view0

LangGraph and LangChain are more productionable, but what they give you is infrastructure, not policy. Their persistence layer separates **checkpointers** for thread-scoped state from **stores** for cross-thread durable data. Long-term memory is stored as JSON documents under `namespace` and `key`, and the store supports both semantic search and filtering by content. Deep Agents load persistent memory from AGENTS.md files and can also search past conversations through tools. This is powerful, but by default it is still general-purpose memory engineering: preferences, facts, skills, conversation continuity, and searchable episodes. There is no built-in concept of “this route failed under these constraints; force the agent to consult that fact before a retry.” citeturn19view2turn24view0turn24view1turn24view2turn23view0turn23view1

CrewAI is similar. Its current unified memory system uses a single `Memory` class, lets an LLM infer scope/categories/importance when saving, and ranks recalls using a composite of semantic similarity, recency, and importance. The framework also documents knowledge storage in ChromaDB-backed collections and offers reset commands for long-term, short-term, entities, kickoff outputs, and knowledge stores. That is convenient for broad agent memory, but it is almost the opposite of what you want for negative constraints: it optimizes fuzzy useful recall, not exact “do not re-enter this route in this regime unless superseded.” citeturn22view0turn20view0turn21view5

Reflexion- and self-debug-style loops remain valuable inside a single run, but they should be treated as **inputs to curation**, not as the durable store itself. The *Negative Knowledge* results support that. Its depth-1 and depth-N curated records compressed the useful “what failed and what to do instead” signal into a smaller, typed memory unit than raw multi-round self-debug, and the deeper curated record beat raw self-debug on the reported benchmark. In other words: reflection is a good write-time primitive, not a sufficient persistent failure architecture. citeturn10view0turn11view0

## Why vector memory alone is the wrong primitive for failure constraints

The retrieval trap is structural. Similarity search is good at “find semantically related context,” but your target object is a **constraint** on future search, not a general memory snippet. That distinction matters because a “similar past fix” can be exactly the wrong thing when the true lesson is that a tempting route was invalid in a specific regime. The *Negative Knowledge* paper makes this point operationally by insisting on typed failure fields and adopt/reject consultation instead of relying on free-form retrieval. AutoGPT’s issues show the practical version: when memory lacks metadata and is judged only by relevance, agents can loop on failed attempts because those failed attempts still look similar to the current task. citeturn10view0turn11view0turn4view0turn27view0

Recent memory research makes the same point more formally. The 2026 memory-systems study finds that retrieval fidelity degrades as temporal distance grows and identifies explicit query planning plus balanced hybrid search as more effective than naive retrieval; it also warns that append-only memory stores without lifecycle management return stale facts and cause “hallucinations of the past.” The STALE benchmark then shows that even strong models struggle to recognize when earlier memory is no longer valid, especially under implicit conflicts. The MemStrata paper pushes the knife in deeper: contradictions are often almost as embedding-similar to the original fact as plain duplicates, with cosine similarity only near chance for distinguishing contradicted from duplicated facts, which is exactly why deterministic supersession rules beat thresholded similarity when facts evolve. citeturn35view0turn16search0turn16search4

For coding agents, this gets worse because many “negative” lessons are **regime-bound**, not universally true. A route may be wrong only for CPU execution, only for bore-like initial conditions, only for a specific dependency version, only when tests are stale rather than broken, or only when a browser tool is in a certain mode. The *Negative Knowledge* taxonomy’s `scope` field exists precisely to separate `local_failure`, `regime_bound_failure`, and `general_failure`. The TEBench paper on project-level test evolution shows a directly related phenomenon: “Test-Stale” cases are the hardest because systems built around reactive failure signals do not proactively reason about semantically outdated checks. That is the same family of problem as stale negative advice. citeturn11view0turn32view0

The result is straightforward. Use vector or semantic memory for **positive background recall** and maybe for discovering candidates to inspect. Do **not** use it as the authority for negative constraints. The authority should be exact-match structured retrieval over a closed tag vocabulary plus status/supersession logic. That is the cheapest design that directly addresses the failure class your prompt describes. This is an engineering inference, but it is strongly supported by the failure modes and evaluation results above. citeturn11view0turn35view0turn16search4

## Cheapest architecture that fits SQLite plus markdown

The minimal design below is tuned to a stack that already has SQLite, markdown cards, and a “SQL/grep before LLM” habit. It keeps the *Negative Knowledge* depth-1 core, adds only the maintenance fields you need in real coding work, and uses a separate exact-tag table so retrieval is deterministic rather than fuzzy.

### Minimal SQLite schema

```sql
CREATE TABLE failure_records (
    id INTEGER PRIMARY KEY,
    created_at TEXT NOT NULL,
    updated_at TEXT NOT NULL,

    project TEXT NOT NULL,
    repo TEXT NOT NULL,

    task_class TEXT NOT NULL,              -- stable task family, not raw prompt text
    route_signature TEXT NOT NULL,         -- canonical attempted route slug
    attempt_count INTEGER NOT NULL DEFAULT 1,

    layer TEXT NOT NULL CHECK (layer IN (
        'implementation_failure',
        'communication_failure',
        'method_failure',
        'environment_failure',
        'spec_failure'
    )),
    scope TEXT NOT NULL CHECK (scope IN (
        'local_failure',
        'regime_bound_failure',
        'general_failure'
    )),
    degree TEXT NOT NULL CHECK (degree IN (
        'contradicted',
        'partial',
        'inconclusive',
        'unstable',
        'artifact_driven',
        'overclaimed'
    )),
    recommended_action TEXT NOT NULL CHECK (recommended_action IN (
        'retry',
        'change_method',
        'narrow_claim',
        'abandon_route',
        'escalate'
    )),
    risk TEXT NOT NULL CHECK (risk IN (
        'low_risk_omission',
        'medium_risk_drift',
        'high_risk_false_progress'
    )),

    observation TEXT NOT NULL,             -- bounded, 1-3 sentences
    rationale TEXT NOT NULL,               -- bounded, 1-3 sentences
    recommended_alternative TEXT NOT NULL, -- bounded, 1-3 sentences

    evidence_paths TEXT NOT NULL,          -- JSON array of file/log paths
    card_path TEXT NOT NULL,               -- markdown card path

    status TEXT NOT NULL DEFAULT 'active' CHECK (status IN (
        'active',
        'superseded',
        'expired',
        'rejected'
    )),
    superseded_by INTEGER,
    valid_from_commit TEXT,
    valid_until_commit TEXT,
    last_confirmed_at TEXT,
    last_consulted_at TEXT,
    consult_count INTEGER NOT NULL DEFAULT 0,

    UNIQUE(project, repo, task_class, route_signature, layer, scope)
);

CREATE TABLE failure_tags (
    record_id INTEGER NOT NULL,
    tag_key TEXT NOT NULL,                 -- e.g. domain, lang, tool, phase, error_family
    tag_value TEXT NOT NULL,               -- closed vocabulary only
    FOREIGN KEY(record_id) REFERENCES failure_records(id),
    UNIQUE(record_id, tag_key, tag_value)
);

CREATE INDEX idx_failure_active
    ON failure_records(project, repo, status, layer, scope, recommended_action, risk);

CREATE INDEX idx_failure_tags
    ON failure_tags(tag_key, tag_value, record_id);
```

This schema is an opinionated extension of the *Negative Knowledge* base record. The closed-vocabulary failure fields come directly from the paper’s typed design; `status`, `superseded_by`, and validity markers are added because recent memory work shows that stale records and append-only stores become a real failure mode unless memories can be revised, retired, or superseded. The separate tag table is there solely to guarantee exact retrieval in SQL instead of fuzzy similarity. citeturn11view0turn26view0turn16search0turn16search4turn35view0

### Recommended closed tags

Keep the tag vocabulary small and project-local. For a personal coding agent, I would start with these exact tag families:

| tag_key | example values |
|---|---|
| `domain` | `debug`, `refactor`, `tests`, `deps`, `build`, `tooling`, `db`, `frontend`, `backend` |
| `lang` | `python`, `typescript`, `sql`, `bash` |
| `tool` | `pytest`, `sqlite`, `alembic`, `playwright`, `git`, `ripgrep` |
| `phase` | `plan`, `edit`, `run`, `test`, `review`, `deploy` |
| `error_family` | `timeout`, `syntax`, `import`, `api_mismatch`, `schema_mismatch`, `silent_wrongness`, `flaky`, `stale_test` |
| `asset` | `leads.db`, `system.db`, `concept_card`, `reglament_card`, `migration`, `testsuite` |
| `regime` | `local_only`, `cpu_only`, `ci_only`, `dirty_worktree`, `large_input`, `legacy_api` |

The literature does **not** give one universal best vocabulary. What it does support is the principle: use bounded typed fields for the failure class, and use retrieval representations that match the downstream reuse purpose rather than generic semantic likeness. A small, explicit tag set is the cheapest way to operationalize that in your environment. citeturn11view0turn31view1

### Markdown card template

```md
---
id: fail-2026-07-05-0017
project: my-agent-stack
repo: personal-coding-agents
task_class: sqlite-schema-migration
route_signature: regen-migration-from-llm-only
layer: method_failure
scope: regime_bound_failure
degree: unstable
recommended_action: abandon_route
risk: high_risk_false_progress
status: active
tags:
  domain: [db, tooling]
  lang: [sql, python]
  tool: [sqlite, alembic]
  phase: [plan, run]
  error_family: [schema_mismatch, silent_wrongness]
  asset: [leads.db, system.db, migration]
  regime: [dirty_worktree, legacy_api]
evidence_paths:
  - logs/2026-07-05/migrate.log
  - runs/2026-07-05/attempt-3.patch
created_at: 2026-07-05T11:42:00Z
last_confirmed_at: 2026-07-05T11:42:00Z
---

# Failure card

## Observation
Generated migration passed syntax but produced schema drift against live SQLite state.

## Why this failed
LLM-only regeneration inferred target schema from model context instead of reading current SQLite metadata and migration history.

## What to do instead
Read live schema first, diff against migration history, then generate a minimal hand-checked migration stub.

## Boundaries
Applies when repo is dirty or migration history is non-linear. Re-evaluate if schema is freshly snapshotted and migration chain is linear.
```

The YAML front matter mirrors the database so you can `grep` exact tags or inspect cards by hand, while the body stays bounded and readable. That split is consistent with the “bounded and typed” NK design and with current maintainability work arguing that memory needs explicit organization and revision, not infinite append-only prose. citeturn11view0turn16search1

## Read and write workflow with forced grounding

The single most important discipline is that **the executor does not get to decide whether failure memory matters**. The scheduler decides that. The *Negative Knowledge* paper’s strongest idea is not the schema alone; it is the explicit requirement that downstream agents inspect the bank and adopt or reject records before the next move. That forced-grounding step is what turns failure memory from documentation into control flow. citeturn10view0turn12view0

### When to write

Write a failure record only when the attempt consumed real search budget and established a reusable constraint. In practice, that means one of four triggers: the route was abandoned after a nontrivial attempt; the same route family failed twice; the attempt produced **high-risk false progress** such as passing surface checks while drifting from intent; or a human reviewer explicitly rejected the route. Do **not** write trivial syntax slips, typo fixes, or one-off tool hiccups unless they repeat or reveal a broader constraint. This is an inference from the NK paper’s “pay for a failed route only once” principle and from newer maintenance work showing that indiscriminate append-only memory degrades over time. citeturn10view0turn35view0

### Who writes

Use a **curator**, not the failing executor. The NK workflow separates these roles so the curator works from frozen artifacts rather than from the failed agent’s self-justifying narrative. In your stack, the curator can be a cheap deterministic pass plus an LLM only for normalization: collect patch/log/test output, classify the failure with closed-vocabulary fields, write or merge the record, and attach evidence paths. citeturn10view0turn11view0

### How to read safely

Before any retry, the planner derives a deterministic retrieval key from the current intent:

- intended `task_class`
- intended `route_signature`
- planned `domain`, `phase`, `tool`, `lang`
- known `error_family` or boundary regime if present

Then it runs exact SQL and front-matter grep. A practical query looks like this in spirit:

```sql
SELECT fr.*
FROM failure_records fr
JOIN failure_tags d  ON d.record_id = fr.id AND d.tag_key = 'domain' AND d.tag_value = :domain
JOIN failure_tags p  ON p.record_id = fr.id AND p.tag_key = 'phase'  AND p.tag_value = :phase
LEFT JOIN failure_tags t ON t.record_id = fr.id AND t.tag_key = 'tool' AND t.tag_value = :tool
WHERE fr.project = :project
  AND fr.repo = :repo
  AND fr.status = 'active'
  AND fr.route_signature = :route_signature
ORDER BY
  CASE fr.scope
    WHEN 'general_failure' THEN 3
    WHEN 'regime_bound_failure' THEN 2
    ELSE 1
  END DESC,
  CASE fr.risk
    WHEN 'high_risk_false_progress' THEN 3
    WHEN 'medium_risk_drift' THEN 2
    ELSE 1
  END DESC,
  fr.updated_at DESC
LIMIT 10;
```

If exact route matches are empty, back off one level to exact `task_class + domain + phase + tool`, never to unrestricted semantic nearest neighbors. That retrieval order follows directly from the evidence that generic similarity and append-only memory are fragile under temporal change and contradiction. citeturn16search4turn35view0turn27view0

### How to force grounding

Require the planner to emit a machine-validated object before any edit/run/test action:

```json
{
  "consulted_failure_ids": [17, 22],
  "adopted": [
    {"id": 17, "why": "same route_signature and same sqlite+migration regime"}
  ],
  "rejected": [
    {"id": 22, "why": "stale after API upgrade; valid_until_commit exceeded"}
  ],
  "next_route_signature": "read-live-schema-then-stub-migration"
}
```

The scheduler blocks execution if this object is missing, if the planner tries to reuse a route that an adopted record marks `abandon_route`, or if it rejects a high-risk matching record without a bounded justification. This is the local-agent analogue of the NK paper’s `cites_bank`, `rejects_bank`, and `bank_use_rationale` requirement. citeturn12view0

### How to avoid landfill

Use **merge-not-append** as the default. If a new failure lands on the same `(project, repo, task_class, route_signature, layer, scope)` signature, update `attempt_count`, refresh `updated_at`, refresh the card’s evidence block, and only create a new row if the failure boundary materially changed. When the same pattern appears across multiple tasks, synthesize an optional **depth-N pattern card** that records the shared diagnosis and ruled-out routes, mirroring the NK paper’s cross-round synthesis fields. Localized, conservative maintenance is also what the 2026 memory-systems evaluation found to be the safer general default than delayed or global reorganization. citeturn11view0turn35view0

## Anti-patterns and maintenance rules

The first anti-pattern is the obvious one: **semantic-similarity-first retrieval for negative memory**. It feels convenient, but the recent evidence says it is brittle under contradiction, staleness, and near-duplicate wrongness. Use exact tags first, exact route signatures second, and only then optional semantic candidate generation for human review. Never let semantic similarity alone authorize a veto or a retry ban. citeturn16search4turn16search0turn35view0

The second anti-pattern is **append-only failure landfill**. If every red test, typo, and flaky tool response becomes a new durable record, your failure memory stops being a constraint bank and turns into cheap noise with expensive retrieval. The *Negative Knowledge* design is deliberately bounded, typed, and compact for exactly this reason, and recent maintainability work such as Infini Memory and the memory-systems evaluation both emphasize that memory quality depends on maintenance, consolidation, and revision rather than endless accumulation. citeturn10view0turn11view0turn16search1turn35view0

The third anti-pattern is **never decaying negatives**. A “don’t do X” record can go stale when the codebase changes, a dependency upgrades, a tool bug is fixed, a schema migrates, or an earlier limitation becomes irrelevant. STALE shows that current systems are bad at recognizing implicit invalidation, and MemStrata’s main result is that temporal validity needs explicit supersession logic rather than similarity heuristics. For a coding agent, that means every negative record should have at least one of: `valid_from_commit`, `valid_until_commit`, `last_confirmed_at`, or `superseded_by`. If none of those can be maintained, the record should default to lower authority. citeturn16search0turn16search4

The fourth anti-pattern is **letting the executor write its own epitaph**. The NK paper explicitly separates executor and curator to reduce self-assessment bias. In personal-agent practice, the cheapest version of this is a curator prompt or deterministic script that sees the patch, tests, logs, and route slug, but not the executor’s internal self-excuses unless you intentionally store them as evidence. citeturn10view0turn11view0

The fifth anti-pattern is **using always-loaded rules files as the sole negative memory**. AGENTS.md or your `reglament-*` cards are excellent for stable norms, coding conventions, and evergreen process rules; LangGraph Deep Agents explicitly load memory files at startup for that purpose. But rules files are weak for route-specific failure evidence because they bloat over time, blur local versus regime-bound failures, and are hard to supersede surgically. Use rules files for policy, SQLite records for constraints, and markdown cards for inspectability. citeturn23view0turn23view1

## Open questions and limitations

The weakest part of the evidence base is industry standardization. The research side is ahead of the frameworks: there is now credible work on negative knowledge, failure-aware repair memory, staleness benchmarks, maintainable topic documents, and memory maintenance evaluation, but mainstream coding-agent frameworks still expose generic memory primitives rather than a settled “failure memory” standard. That means the architecture above is well-supported in principle, but the exact field vocabulary and retriever policy still need local tuning. citeturn10view0turn13view0turn16search0turn16search1turn35view0

The *Negative Knowledge* paper itself is also early-stage evidence. Its benchmarks and case studies are promising, but they are still limited in scale and domain. I would treat its workflow and schema as the best available template, not as a final universal ontology for coding agents. In particular, for software work you probably need a few extra layers beyond the paper’s base taxonomy, such as `environment_failure` and `spec_failure`, plus commit-scoped validity fields for supersession. That extension is an engineering inference rather than something the paper validates directly. citeturn10view0turn11view0

The final limitation is that any closed vocabulary can calcify. If the tag set is too small, the agent overgeneralizes and suppresses useful exploration. If it is too large, you are back to free-text drift in disguise. The safest compromise is to keep the **failure-class fields globally closed** and the **project tags narrowly closed but reviewable**, then promote repeated patterns into depth-N records only when the same failure signature recurs enough times to justify a stable abstraction. That is the lowest-cost way to balance discipline against over-suppression. citeturn11view0turn31view1turn35view1
