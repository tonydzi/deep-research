---
dr_id: DR26-07-04-ZB-02
title: "Graph RAG on SQLite for agents — validating the niche and article angles"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-ZB-02): Graph RAG on SQLite for agents — validating the niche and article angles

> Deep research validating whether 'SQLite-native Graph RAG using hand-curated Obsidian wikilinks instead of LLM entity extraction' is an empty/underexplored niche worth claiming in a technical article.

## Ключевые выводы
- The niche is NOT empty when framed broadly ('graph rag over markdown/wikilink notes'): direct competitors exist — Vasallo94/ObsidianRAG (local plugin, hybrid vector+BM25, cross-encoder rerank, wikilink expansion), fsck knowledge-graph-tools (vault→SQLite+sqlite-vec+FTS5, no LLM in the tool layer), obsidian-notes-rag, obsidian-graph-mcp, Mnemo, MotherDuck DuckDB-based Obsidian RAG.
- The niche IS still under-claimed narrowly: no found project combines a 100k-note vault + hand-curated (not LLM-extracted) wikilinks + SQLite-only persistence + dense-top-60→bounded-1-hop-expansion→cross-encoder-rerank + query-type gating + published A/B telemetry showing conditional wins/losses.
- Mainstream GraphRAG (Microsoft GraphRAG, LightRAG, nano-graphrag) is architecturally different: all rely on LLM entity/relation extraction and graph/community construction, not pre-existing human-curated edges.
- External academic evidence corroborates the team's own telemetry: a manuscript-collection GraphRAG benchmark found graph expansion improves catalog/complex/thematic queries but hurts entity/semantic queries by diluting the top-ranked list — same shape as the pilot's entity-query regression.
- GraphRAG survey literature (document/citation/hyperlink graphs, graph traversal) supports structure-first retrieval in principle but explicitly warns that k-hop neighbourhood expansion causes information overload/noise if unbounded.
- Evaluation is the field's missing layer — GraphRAG-Bench and RAGBench exist specifically because standard QA benchmarks can't test graph-based retrieval gains; no competitor publishes a query-stratified benchmark with an explicit routing rule.
- Agent-memory products (mem0, Zep/Graphiti, Letta/MemGPT, LangMem) are substitutes, not direct competitors — they build memory from conversations/entity extraction, not from a pre-existing hand-curated personal knowledge graph.
- Embedded graph-DB-on-SQLite alternatives exist (GraphQLite, a 2026 sqlite-graphrag Rust crate) but add cypher-like/graph-native abstractions vs. the pilot's simpler edge-table + retrieval-time expansion approach.
- Consistent failure modes across the field: entity-query dilution, hub-note overreach (generic MOC/tag/daily notes explain nothing), stale wikilinks resurrecting outdated context, messy YAML/frontmatter in real vaults, and graph algorithms (PageRank/centrality) promoting popularity over relevance.

## Рекомендации / решения
- Freeze the article's claim to the narrow, defensible version: SQLite-native, wikilink-based graph RAG for single-user agents, no LLM entity extraction, graph expansion helps thematic/multi-hop queries but hurts entity/name lookups unless routed.
- Build a 150–300 query benchmark stratified by type (entity/name, tool/project, theme/synthesis, relationship/multi-hop, temporal, exact recall, ambiguous) with human-labeled top-12 relevance; use LLM judges only as a secondary check.
- Run retrieval ablations: dense-only, BM25-only, BM25+dense RRF, dense→graph→rerank, graph-only (as cautionary baseline), and the entity-gated router; vary seed counts (5/15/30), neighbour caps (10/20/40/80), and 1-hop vs 2-hop.
- Publish the SQLite telemetry schema (queries, retrieval_runs, candidates, candidate_sources, rerank_scores, answers, feedback, ab_test_assignments) with the SQL queries used to produce article charts.
- Publish a 'failure gallery' — name query buried by neighbours, hub-note explosion, daily-note noise, alias collision, stale link, 2-hop disaster — framed as the article's actual moat (negative results, not another win-only writeup).
- Implement/document a 4-class query router: entity/name off by default, theme/synthesis on, relationship/multi-hop on with path provenance, temporal queries need recency filtering rather than graph expansion.
- Degree-normalize or downweight hub notes (MOCs, daily notes, tags, templates) and store provenance per candidate (source + seed note + edge path) for debuggability.
- Release a public synthetic mini-vault (200–500 notes) plus the benchmark harness so results are reproducible, even though the real 100k-note vault stays private.
- Distribution plan: GitHub repo + technical blog first, then Hacker News/lobste.rs/r/localllama/r/obsidianmd/r/rag, optional short arXiv technical report if benchmarks are solid; lead with 'measured wikilink GraphRAG on SQLite,' not an 'AI agent memory protocol' pitch.

## Сущности
- **Люди:** —
- **Компании:** Microsoft, MotherDuck
- **Продукты/инструменты:** Microsoft GraphRAG, LightRAG, nano-graphrag, txtai, Khoj, Rememberizer, mem0, Zep, Graphiti, Letta, MemGPT, LangMem, LangGraph, RAGAS, Arize Phoenix, LangSmith, TruLens, GraphRAG-Bench, RAGBench, HippoRAG, CG-RAG, G-RAG, Vasallo94/ObsidianRAG, fsck knowledge graph tools, obsidian-notes-rag, obsidian-graph-mcp, Mnemo, GraphQLite, sqlite-graphrag, Kuzu, sqlite-vec, sqlite-graph-memory

## Открытые вопросы
- Whether the narrow niche claim survives public scrutiny once published (someone could surface a closer unpublished competitor).
- Whether hand-curated wikilinks are genuinely higher-quality retrieval priors than LLM-extracted edges, or just differently biased (hub-heavy, stale, inconsistent) — not empirically resolved, only asserted as plausible.
- Whether 2-hop expansion or graph algorithms (PageRank, centrality) could help specific query types — flagged as risky/untested rather than validated.
- Best publication venue and arXiv acceptance likelihood — only suggested, not confirmed.
- Maturity/activity level of GraphQLite and the sqlite-graphrag crate as real competitive alternatives is unverified.

## Источник
- DR-ID `DR26-07-04-ZB-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- Graph RAG
- wikilink graph
- Obsidian vault
- agent memory
- retrieval-augmented generation
- SQLite
- RAG evaluation benchmarks
- query routing
- insight-DR-DR26-07-26-ZB-05-obsidian-alternatives-i-mnogomernaya-perelinkovka — продолжение (2026-07-26): два внешних вендора независимо подтвердили замеренный здесь эффект «граф вредит entity-запросам» (имя механизма — семантическое разбавление) и предложили лечение помимо роутера: entity-enriched эмбеддинги и битемпоральные рёбра; плюс потолок масштаба GUI-приложений на 226k заметок
- insight-DR-DR26-08-04-ZB-14-2214-graph-expansion-fan-penalty — прямая предыстория gate/fan-penalty решения в ZB-14 (entity-query regression, hub-note overreach)
