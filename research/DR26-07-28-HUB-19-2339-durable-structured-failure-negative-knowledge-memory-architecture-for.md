---
dr_id: DR26-07-28-HUB-19-2339
title: "Durable structured failure/negative-knowledge memory architecture for coding agents"
date: 2026-07-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-19-2339): Durable structured failure/negative-knowledge memory architecture for coding agents

> Deep research on the cheapest durable architecture for 'negative knowledge' / failure memory that stops personal coding agents from rediscovering dead-ends across sessions, using closed-vocabulary structured fields instead of vector similarity.

## Ключевые выводы
- Cheapest durable design = small structured SQLite failure DB with closed-vocabulary tags + a separate curator that writes one bounded record per meaningful failed route + a forced-grounding gate blocking retries until the agent explicitly consults and marks records adopted/rejected — matches the 'Negative Knowledge as Failure-aware Shared Memory for AutoResearch' paper (arxiv 2606.21024).
- Vector/semantic-similarity retrieval is structurally wrong for negative constraints: a 'similar past fix' can be exactly the wrong lesson; MemStrata shows contradictions are nearly as embedding-similar to the original fact as duplicates, and the STALE benchmark shows even strong models struggle to recognize invalidated memory.
- Mainstream agent frameworks lack first-class failure memory: Reflexion/Self-Refine store free-text reflections, not durable typed cross-session objects; LangGraph/LangChain give generic persistent stores (namespace/key JSON, semantic search) but no failure-specific policy; CrewAI's unified memory blends semantic similarity+recency+importance, the wrong default for 'never retry this route'; AutoGPT's own issue history shows vector memory without metadata caused agents to loop on failed attempts that still looked relevant.
- The Negative Knowledge paper's depth-1 schema (task_id, attempted_route, observation, failure{layer,scope,degree,recommended_action,risk}, rationale, recommended_alternative) with closed vocabularies is the closest SOTA match; a curated record beat raw multi-round self-debug in the harder benchmark setting.
- Recommended minimal schema extends the paper with maintenance fields (status, superseded_by, valid_from_commit/valid_until_commit, last_confirmed_at, consult_count) plus a separate failure_tags table for exact-match retrieval — because append-only stores without lifecycle management return stale 'don't do X' facts ('hallucinations of the past').
- Retrieval order must be SQL/grep exact-tag-and-route match first, backing off to task_class+domain+phase+tool, never unrestricted semantic nearest-neighbor for negative memory; semantic search is acceptable only for positive background recall or human-reviewed candidate discovery.
- Write discipline: only write a record when the attempt consumed real search budget and established a reusable constraint (abandoned after nontrivial attempt, same route failed twice, high-risk false progress, or explicit human rejection) — trivial slips are not recorded, to avoid landfill.
- Forced grounding is implemented as a machine-validated JSON object (consulted_failure_ids, adopted[], rejected[], next_route_signature) that the scheduler requires before any edit/run/test action; missing it or reusing an 'abandon_route' record without justification blocks execution — the local-agent analogue of the paper's cites_bank/rejects_bank/bank_use_rationale fields.
- Anti-landfill uses merge-not-append on the (project, repo, task_class, route_signature, layer, scope) signature, and promotes recurring cross-task patterns into optional depth-N pattern cards rather than accumulating duplicate rows.
- Always-loaded rules files (AGENTS.md, reglament-* cards) are good for stable evergreen policy but weak for route-specific failure evidence — they bloat, blur local-vs-regime-bound failures, and are hard to supersede surgically; keep failure evidence in SQLite/markdown cards instead.

## Рекомендации / решения
- Implement two SQLite tables — failure_records (with CHECK-constrained closed-vocabulary columns layer/scope/degree/recommended_action/risk plus status/supersession fields) and failure_tags (exact tag_key/tag_value pairs) — as the core failure store.
- Pair each DB row with a markdown card whose YAML front matter mirrors the structured fields, so records stay grep-able and human-inspectable.
- Route write access through a separate curator step (deterministic pass + light LLM normalization) that works from frozen artifacts (patch/log/test output), never letting the failing executor author its own record.
- Require the planner to emit and pass a validated 'consulted/adopted/rejected' object before any retry; block execution in the scheduler if it's missing or contradicts an active abandon_route record.
- Retrieve strictly via exact SQL/tag match on task_class + route_signature + domain/phase/tool, falling back level by level — never default to semantic similarity for negative constraints.
- Add validity/supersession fields (status, superseded_by, valid_until_commit, last_confirmed_at) to every record so stale 'don't do X' guidance can be retired instead of silently misleading future attempts.
- Merge new failures into existing rows by signature instead of appending, and synthesize depth-N pattern cards only once a failure signature recurs across multiple tasks.

## Сущности
- **Люди:** —
- **Компании:** AutoGPT, LangChain, LangGraph, CrewAI
- **Продукты/инструменты:** Negative Knowledge (arxiv 2606.21024), Reflexion, Self-Refine, AutoGPT, LangGraph, LangChain Deep Agents, AGENTS.md, CrewAI, ChromaDB, Pinecone, Redis, Weaviate, Milvus, STALE benchmark, MemStrata, TEBench, SQLite

## Открытые вопросы
- No industry-standard failure-memory schema exists yet in mainstream frameworks — exact tag vocabulary and retriever policy still need local tuning per project.
- The Negative Knowledge paper itself is early-stage: benchmarks and case studies are limited in scale and domain, so its schema should be treated as a template, not a validated final ontology for coding agents.
- Extra fields proposed for software work (environment_failure, spec_failure layers, commit-scoped validity) are an engineering inference not directly validated by the source paper.
- How to keep the closed vocabulary from calcifying: too small over-generalizes and suppresses useful exploration, too large collapses back into free-text drift — no settled answer given, just a 'globally closed failure-class fields, narrowly closed but reviewable project tags' compromise.

## Источник
- DR-ID `DR26-07-28-HUB-19-2339` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- negative-knowledge-memory
- failure-aware-agent-memory
- case-based-reasoning
- vault-data-architecture
- sql-before-llm-ladder
- ak47-simplicity
- dr-registry
- multi-agent-role-discipline
