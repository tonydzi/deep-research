---
dr_id: DR26-06-25-HUB-07
title: "Persistent memory architecture for a personal AI agent (digital twin) on top of Obsidian"
date: 2026-06-25
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-25-HUB-07): Persistent memory architecture for a personal AI agent (digital twin) on top of Obsidian

> Evaluates whether to replace Anton's Obsidian+local-embedding stack with a memory platform (Graphiti/Zep, Hindsight, LangMem, MemPalace, GBrain, Claude-Mem, Cognee, Mem0, Letta) or build missing layers on top, and answers: build, adopting only patterns.

## Ключевые выводы
- A June 2026 comparative study of 12 memory systems concludes no single architecture dominates universally — fit depends on matching representation/extraction/retrieval/maintenance to the workload, and localized maintenance beats global reorganization
- Newer benchmarks show retrieval, not generation, is the dominant failure mode: DynamicMem attributes >93% of failures to retrieval; LifeBench's top systems reach only 55.2% accuracy; Momento shows agents often treat stale context as current truth
- Best design is a hybrid of fidelity and distillation: keep an immutable episodic/transcript ledger for exact replay and provenance, but recall a distilled per-turn 'semantic working-state' object (decisions, entities, open threads, constraints, evidence pointers) by default, escalating to full transcript only on demand
- Structured, temporally-aware memory is the 2025-26 trend: Graphiti/Zep (temporal context graph with validity windows), Hindsight (typed facts/experiences/observations/opinions + TEMPR multi-channel retrieval: semantic+BM25+graph+temporal), Temporal Semantic Memory, User-as-Code — all converge on typed, time-aware, source-traceable memory objects
- Naive one-shot summarization is fundamentally brittle (ProMem: can't anticipate future query needs, no correction loop); better is per-turn incremental schema-bound state distillation, treated as its own subsystem rather than an afterthought of logging
- Large context windows are only a tactical fallback for early conversation history (ConvoMem: brute-force replay can beat some RAG for the first tens-to-low-hundreds of conversations), not a full memory strategy as history grows (MemGPT's OS-style paging remains relevant)
- Graph-enhanced retrieval materially beats vector-only search for temporal/entity/contradiction reasoning; recommended pattern is a bounded graph overlay derived from the existing vault (1 hop default, 2 hops only for relational/temporal queries) rather than adopting a dedicated graph database
- Major failure modes identified: memory poisoning (PoisonedRAG, MINJA — feasible even without direct write access to the memory store), semantic drift from repeated lossy summarization, unbounded growth/maintenance cost, and benchmark overfitting (MemPalace's cited 96.6% R@5 was contested in its own GitHub issues as effectively just a ChromaDB retrieval score, not an end-to-end benchmark)
- Full memory-platform products (MemPalace, GBrain, Claude-Mem, Cognee, Graphiti, and largely Letta) each introduce their own storage substrate/hooks/skill system and would create a second canonical memory store alongside the existing vault — good as reference architectures/donor components, not as drop-in replacements for a vault-centric single-user setup

## Рекомендации / решения
- Build on the existing Obsidian+SQLite+local-embeddings stack as the system of record; do not adopt a full external memory platform
- Add a TurnState SQLite table: per-turn/per-session versioned rows with active_goal, decisions, open_threads, entities, constraints, pending_actions, volatile_facts, and evidence pointers into transcript/note spans; store both materialized state and deltas
- Add a 3-mode retrieval planner: startup (identity + top state deltas), hot-path (vector+lexical+bounded graph expansion), deep-recall (self-triggered full transcript lookup when exact commands/rationale/contradictions are needed)
- Build a derived lightweight graph overlay (Note, Entity, Decision, Thread, Session, Fact nodes; typed edges like mentions/decides/supersedes/reopens) from existing Obsidian wikilinks/frontmatter/SQLite — before considering any dedicated graph engine
- Run an offline daily/idle consolidation ('dreaming') job that only clusters/proposes candidate facts, summaries, and procedures into a quarantine namespace — never auto-promotes to canonical memory
- Use a 3-tier promotion policy (Cold candidate → Warm memory → Hot semantic memory) requiring multiple independent evidence sources or repeated observations before promotion, with automatic demotion on new conflicting evidence or expired volatility windows
- Give every memory record explicit temporal validity fields (event_time, observed_time, valid_from, valid_to, status) and allow contradictory historical states to coexist instead of overwriting
- Rollout order: (1) semantic-state distillation first, (2) bounded graph expansion on top of current vector retrieval, (3) offline candidate consolidation, (4) only then evaluate importing external framework code as small utilities
- Treat MemPalace/GBrain/Claude-Mem/Cognee/Graphiti as reference architectures/donor components (borrow wake-up layering, dream-cycle, self-wiring-graph, hook choreography ideas) rather than platforms to adopt wholesale

## Сущности
- **Люди:** —
- **Компании:** Zep
- **Продукты/инструменты:** LongMemEval, LoCoMo, Momento, DynamicMem, LifeBench, RHELM, MemGPT, Letta, Graphiti, Hindsight, Temporal Semantic Memory, ProMem, SimpleMem, User-as-Code, StateAct, ConvoMem, memsearch, MemPalace, GBrain, Claude-Mem, Cognee, Mem0, LangMem, PoisonedRAG, MINJA, OWASP Agent Memory Guard

## Открытые вопросы
- No universally trusted benchmark suite exists yet in this space; several new benchmarks (Momento, DynamicMem, LifeBench, RHELM) are too recent to have broad independent reproduction
- Little high-quality public evidence exists on per-turn semantic-state distillation specifically for coding agents, as opposed to generic assistants/long-horizon dialogue
- Security literature converges on layered defenses against memory poisoning, but effective thresholds and trust policies remain deployment-specific and unresolved
- Mem0's exact current repo licensing details were not verified in the gathered sources
- The specific state schema, promotion thresholds, and graph-expansion budgets proposed are engineering syntheses from the literature, not directly benchmarked recipes — need validation against Anton's own eval set

## Источник
- DR-ID `DR26-06-25-HUB-07` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\2026-06-25-always-on-memory-architecture-DR.md`

## Связано
- [[vault-data-architecture]]
- [[always-on-memory-pilot]]
- [[turnstate-ledger]]
- [[ak47-simplicity]]
- [[second-brain-northstar]]
- [[verify-existing-before-proposing]]
- [[one-system-propagate]]
- [[insight-DR-DR26-06-27-HUB-04-memory-architecture-patterns-for-long-running-ai-a]] — тот же домен персистентной памяти AI-агента поверх Obsidian, прямое пересечение архитектурных паттернов
- [[insight-DR-DR26-07-28-HUB-09-2338-использование-obsidian-как-второй-мозг-архитектура]] — тот же вопрос архитектуры памяти поверх Obsidian для персонального AI-агента
