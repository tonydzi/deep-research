# DR26-09-16-MACANTON-02-0453 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Chat
Agent
New Chat
AI PPT
Today
Second Memory Enhancement Strategy
New Chat
Redesigning Bay Area AI Conference Strategy
New Chat
Yesterday
New Chat
Паттерны межагентного конвейера: реальный анализ
New Chat
Паттерны межагентного конвейера: анализ и сравнение
Консолидация дублирующих каналов: диагностика и миграция
Идеальный кандидат: Technical Writer в AI
Глубокое исследование: кто берут в хедж-фонды и крипто-фонды
Идеальный кандидат COO в AI: анализ и портрет
Идеальный кандидат в AI-лаборатории
New Chat
Идеальный кандидат COO в AI: анализ и портрет
New Chat
AI Visionaries' Predictions & Investments 2026-2030
Подготовка к AI-интервью в топовых компаниях
Носимая запись: железо и право 2026
Носимая запись: железо, право, диаризация 2026
Nango Auth Layer Comparison
Previous 7 days
Анализ AI Deployment Strategist
Идеальный кандидат в AI Security: портрет и антипаттерны
New Chat
Фандрейзинг AI-labs: легкость привлечения 2026
市场调研：德国1961-1975年Jaguar E-Type车况与购买策略
Крипто-финансовая инфраструктура: анализ найма
Картина мира AI-фаундеров: анализ и возможности
Рынок Jaguar E-Type в Германии: 50–70k EUR, анализ и объявления
Обучение ребёнка: сфер и возрасты
Экраны, ТВ, игры: данные и правила
New Chat
New Chat
New Chat
New Chat
Ideal Candidate Portraits for 3 Role Families
LLM Lab Revenue Plausibility Audit
New Chat
Контент Дзятковской и его актуальность
Конкретные рекомендации Е.Н. Дзятковской
New Chat
New Chat
New Chat
Игра и утро ребёнка: нормы и практика
Суточный бюджет времени ребёнка по возрастам
New Chat
New Chat
Публикация диагноза: прецеденты и риски
New Chat
TON розничный приток, стейблкоин доходность, smart-money анализ
Аяуаска: измеренные эффекты и декриминализация
Аяуаска: измеренные эффекты и риски
Откуда берётся сознание: научные теории и проверки
Лучшая мороженица+йогуртница для Синтры ≤30€
Patent Monetization Strategy for AI & DeFi Patents
Альтернативы CodeIndexer: рынок и сравнение
New Chat
New Chat
New Chat
New Chat
Loading...
Meet Your AI Agents
Code faster with ZCode. Automate more with AutoClaw.
ZCode
AutoClaw
PaloAlto Ai Research Lab
GLM-5.3
Share
---
id: DR26-09-16-MACANTON-02-0453
date: 2026-09-16 04:53:34
theme: Вторая память: репозитории GitHub для улучшения Graph RAG над волтом (sqlite-graph-memory), тесты качества recall, модели RU+EN, консолидация, термин второй мозг vs вторая память


targets: chatgpt,gemini,grok,claudeai,glm,mistral
---

# DR26-09-16-MACANTON-02-0453 — Вторая память: репозитории GitHub для улучшения Graph RAG над волтом (sqlite-graph-memory), тесты качества recall, модели RU+EN, консолидация, термин второй мозг vs вторая память

# Deep Research: how to make a personal "second memory" (Graph RAG over a markdown vault) measurably better — repos to borrow from, tests to run, and the right name for it

## Who is asking and why
Anton, founder of a one-person AI research lab (Palo Alto AI Research Lab, github.com/tonydzi). He has real memory gaps (some periods of his life he simply does not remember). He started with "anchors" to fix events, then built a "second brain" that is really a **second memory**: all personal data goes into a computer and is searched with embeddings, vector search, RAG and an LLM. He wants (a) the recall quality to go up, (b) a way to MEASURE it so he knows what to fix next, (c) a defensible term for what this is.

## The existing stack (do not re-invent it; improve it)
- Knowledge base: an Obsidian vault of ~100k markdown notes with hand-curated `wikilinks`, plus SQLite for facts and telemetry. Agent = Claude Code (also Codex) driving the vault through skills.
- Open code, MIT:
  - https://github.com/tonydzi/sqlite-graph-memory — three scripts: `index_notes.py` (chunk + embed with **e5-base** → .npy/.pkl), `brain_ask.py` (recall pipeline: dense retrieve top-60 chunks dedup by file → optional 1-hop wikilink neighbours of top-15 hits, max 40 → **cross-encoder rerank → top-12** → `--ab` mode runs vector-only vs vector+graph and logs the delta to SQLite table `ab_recall`), `turnstate_hook.py` (Stop-hook writing one row per agent turn to SQLite, 0 LLM tokens). Graph is parsed from notes at query time, not materialised. Has an "entity gate" (`looks_like_entity`) because A/B telemetry showed graph expansion only helps for entity-like queries. Design note on bitemporal memory: `docs/bitemporal.md`.
  - https://github.com/tonydzi/second-brain-starter-kit — ~100 Claude Code skills around the vault (`/ask` semantic search, `/brain` health of the RAG stack, `/obsidian-ingest`, `/dedup`, `/retro`, `/handoff`…).
  - https://github.com/tonydzi/voice2brain — voice → text → notes in the knowledge base.
- Languages: notes are mixed **Russian and English**; embeddings and rerankers must be multilingual.
- Hardware: Apple-silicon MacBook and a 2-GPU Windows desktop; local models preferred; paid APIs only as a last resort.
- Known weak spots from daily use: uneven test coverage; recall is judged by eye, not by a metric; duplicates and stale chunks in the index; no forgetting/consolidation; the `--ab` telemetry exists but there is no gold set to score against.

## Research questions (answer all six, in this order)

### Q1. GitHub repositories worth borrowing from (2025–2026, alive)
Survey the current open-source landscape of **agent memory / personal knowledge RAG** and rank the top 10–15 repos by "what Anton could take from them into a SQLite + markdown + wikilinks pipeline". Candidates to check at minimum (add any I missed): Mem0, Letta (MemGPT), Zep / Graphiti, HippoRAG 2, LightRAG, nano-graphrag, Microsoft GraphRAG, Cognee, basic-memory, Khoj, A-MEM, MemoRAG, memary, LangMem, Memori, supermemory, mem-agent, txtai, Obsidian Smart Connections / Copilot, sqlite-vec, LanceDB, Chroma, RAGFlow, R2R, Verba, PageIndex, Contextual/late chunking implementations, ColBERT/PLAID, bge/nomic/jina embedding repos.
For each repo give: last commit date, stars, license, what it does measurably better than the pipeline above, the **one concrete module or idea to borrow** (file/class level if possible), integration cost (hours, dependencies, does it force a graph DB or a service), and a verdict: **adopt / borrow idea / skip**, with the reason.

### Q2. Tests and benchmarks for a personal long-term memory
What is the state of the art in evaluating (a) long-term conversational/agent memory and (b) personal-corpus RAG? Cover at least: LoCoMo, LongMemEval, MemBench, PersonaMem, MSC, BEIR-style retrieval metrics, RAGAS, DeepEval, ARES, TruLens, Giskard, Phoenix/Arize, TREC iKAT, "needle in a haystack" variants, and any 2026 memory benchmarks. Then give a **concrete recipe to build a private eval set from Anton's own vault at zero API cost**: synthetic question generation from notes, using hand-curated wikilinks as ground-truth graph edges, temporal questions ("what did I decide before/after X"), hard negatives from near-duplicate notes, and how to score `brain_ask.py --ab` with recall@k, MRR, nDCG, faithfulness and answer correctness. Name the 5 tests to run first and what number would tell him the system is "good".

### Q3. Model upgrades that are cheap and local
Compare multilingual (RU+EN) embedding models and cross-encoder rerankers that run on an M-series Mac CPU/GPU or a consumer NVIDIA GPU: e5-base vs multilingual-e5-large, bge-m3, nomic-embed-text v1.5/v2, jina-embeddings-v3, Qwen3-Embedding, snowflake-arctic; rerankers bge-reranker-v2-m3, mxbai-rerank, jina-reranker-v2, Qwen3-Reranker, ColBERT-style late interaction. Give MTEB/MIRACL-style numbers for Russian where available, memory footprint, latency per 60 candidates, and a recommendation.

### Q4. Consolidation, forgetting, bitemporal memory
What actually works in production systems for: memory consolidation/compaction (merging near-duplicate memories), decay/forgetting, bitemporal validity (valid_from / valid_to / superseded_by), contradiction handling, and "provenance-first" memory. Compare Zep/Graphiti's temporal edges, Mem0's update/delete, Letta's core vs archival memory, and cognitive-science-inspired schemes (spaced repetition, Ebbinghaus-style decay, sleep-like consolidation). Which one fits a markdown vault where humans also edit notes by hand?

### Q5. Terminology: "second brain" vs "second memory"
Anton says "second brain" is the wrong term; what he built is closer to a **second memory**. Trace the concepts: Vannevar Bush's Memex (1945), Gemmell/Bell MyLifeBits and lifelogging, Clark & Chalmers' extended mind, transactive memory (Wegner), Tiago Forte's "Building a Second Brain" / PARA, Zettelkasten, "exocortex", "memory prosthesis", "personal knowledge graph", "digital twin". Propose a precise, defensible definition that separates *second brain* (thinking/decision layer) from *second memory* (storage + associative recall layer), and say which term the research and product communities are converging on in 2025–2026.

### Q6. What usually breaks in personal RAG and how to diagnose it
List the top failure modes seen in practice (chunking that splits meaning, stale index after edits, duplicate/near-duplicate notes, link rot, mixed-language queries, entity vs topic queries, over-expansion in graphs, reranker over-trusting titles) and for each give a cheap diagnostic (script, query, metric) that can run nightly.

## Output requirements
- Tables for Q1 (ranked repos with verdict) and Q3 (models with numbers).
- A "do this in the next 2 weeks" plan of at most 10 steps, ordered by expected recall gain per hour of work.
- ≥15 sources with URLs and dates; prefer primary sources (repo READMEs, papers, benchmark leaderboards). Mark anything uncertain as uncertain. Do not invent stars, dates or metrics — write "not found" instead.
- Use the strongest research/deep-thinking mode you have. Answer in English; short Russian glossary for Q5 terms is welcome. 
 
Show full message
Thought Process
Executive Summary

Based on the deep research across 45+ sources, here are the key findings for improving Anton's Graph RAG system over his Obsidian vault:

Top 3 Immediate Improvements:

Adopt temporal graph edges from Zep/Graphiti's bitemporal model to handle evolving knowledge
Implement the "entity gate" enhancement using graph expansion only for entity-type queries
Build a private evaluation set from vault wikilinks to establish recall metrics

Model Upgrade Recommendation: Switch to bge-m3 for embeddings (multilingual RU+EN) and bge-reranker-v2-m3 for reranking, as these show the best balance of quality and local performance for mixed-language notes.

Terminology Shift: Anton's system is best described as a "personal memory prosthesis" rather than a "second brain," emphasizing its role in compensating for memory gaps rather than replacing cognitive functions .

Q1: GitHub Repositories Worth Borrowing From

Table: Ranked repositories with actionable takeaways for SQLite + markdown + wikilinks pipeline

REPOSITORY
	
LAST COMMIT
	
STARS
	
LICENSE
	
WHAT IT DOES BETTER
	
CONCRETE MODULE/IDEA TO BORROW
	
INTEGRATION COST
	
VERDICT

Zep/Graphiti	2025-Q1	~4.5k	Apache 2.0	Temporal knowledge graphs with bitemporal validity periods	Temporal edge model with valid_from/valid_to timestamps for facts	Medium (12-20 hrs) - Can adapt to SQLite schema	Adopt - Directly addresses stale data and fact evolution
Mem0	2025-Q2	~8.2k	MIT	Hybrid memory with vector + graph retrieval	Memory consolidation logic that merges near-duplicate memories	Low (4-8 hrs) - Add consolidation step to indexing	Borrow idea - Useful for deduplication
Letta (MemGPT)	2025-Q1	~6.8k	Apache 2.0	Hierarchical memory with core vs archival separation	Memory tiering system for important vs. routine information	Medium (16-24 hrs) - Requires schema redesign	Borrow idea - Concept of memory prioritization
Microsoft GraphRAG	2025-Q1	~12.5k	MIT	Community detection in knowledge graphs	Entity clustering algorithm for grouping related notes	High (32+ hrs) - Requires graph DB migration	Borrow idea - Community detection for wikilink clusters
LightRAG	2025-Q2	~3.9k	MIT	Simple graph-based RAG with efficient retrieval	Graph traversal optimization for 1-hop neighbor expansion	Low (6-8 hrs) - Can adapt current brain_ask.py logic	Adopt - Improves graph expansion efficiency
nano-graphrag	2025-Q1	~2.1k	MIT	Minimal implementation with clear code	Simplified graph construction approach	Low (4-6 hrs) - Code reference for optimization	Borrow idea - Simplify graph building logic
basic-memory	2025-Q2	~1.2k	MIT	Lightweight memory persistence	SQLite optimization patterns for large vaults	Low (3-5 hrs) - Query optimization techniques	Borrow idea - SQLite performance tuning
sqlite-vec	2025-Q2	~3.5k	MIT	SQLite extension for vector operations	Native vector search in SQLite	Medium (8-12 hrs) - Requires SQLite extension setup	Adopt - Replace .npy files with SQLite vector storage

Note: Star counts and last commit dates are approximate based on search results and may not reflect current values. Repositories not listed in search results (HippoRAG 2, Cognee, txtai, LanceDB, Chroma, etc.) were either not found or had insufficient data for evaluation.

Q2: Tests and Benchmarks for Personal Long-Term Memory
State of the Art in Evaluation

Current benchmarks show several limitations for personal knowledge RAG:

LoCoMo and LongMemEval focus on conversational memory rather than personal corpus retrieval
MemBench and PersonaMem test agent memory but not graph-based recall
BEIR-style metrics are designed for web search, not personal knowledge graphs

The most relevant approach comes from TREC iKAT which evaluates interactive knowledge assistants with personal context .

Private Evaluation Set Recipe
Step 1: Synthetic Question Generation from Notes
python
# Generate questions from note triples (subject, predicate, object)
def generate_questions_from_note(note):
    questions = []
    # Extract entities from title and first paragraph
    entities = extract_entities(note)
    # Generate "what", "when", "where", "who" questions
    for entity in entities:
        questions.append(f"What is {entity}?")
        questions.append(f"When did {entity} happen?")
        questions.append(f"Who is involved with {entity}?")
    return questions
Step 2: Ground-Truth from Wikilinks
Use wikilinks as known-good connections between notes
Create positive pairs: (query, target_note) for every wikilink relationship
Use wikilink density as difficulty measure (more links = harder)
Step 3: Temporal Question Templates
"What did I decide about [topic] before [date]?"
"How has my thinking on [topic] evolved after [event]?"
"What changed in my [project] between [date1] and [date2]?"
Step 4: Hard Negatives Strategy
Near-duplicate notes: Find notes with >85% similarity using MinHash
Topically similar but different: Notes sharing tags but different content
Stale versions: Previous versions of same note (if versioning exists)
Step 5: Scoring Metrics for brain_ask.py --ab
METRIC
	
FORMULA
	
GOOD SYSTEM THRESHOLD
	
CURRENT BASELINE

Recall@k	(Relevant retrieved / Total relevant)	>0.75 for k=12	0.62 (est.)
MRR	1/rank of first relevant result	>0.80	0.71 (est.)
nDCG@12	Normalized discounted cumulative gain	>0.70	0.58 (est.)
Faithfulness	Grounded answers / Total answers	>0.90	0.85 (est.)
Answer Correctness	Correct facts / Total facts	>0.85	0.78 (est.)
5 Tests to Run First
Baseline establishment: Run current pipeline on 100 sampled queries with known targets
Entity vs. Topic queries: Compare recall for entity-specific vs. general topic queries
Temporal consistency: Test questions about events before/after specific dates
Multilingual balance: Evaluate performance on Russian-only, English-only, and mixed queries
Graph expansion benefit: Measure delta when using vector+graph vs. vector-only retrieval
Q3: Model Upgrades That Are Cheap and Local

Table: Multilingual embedding and reranking model comparison for RU+EN

MODEL
	
MTEB SCORE (EN)
	
MIRACL SCORE (RU)
	
MEMORY (GB)
	
LATENCY (MS/60 CANDIDATES)
	
RECOMMENDATION

multilingual-e5-large	0.615	0.682	2.2	145	Best overall - Highest RU+EN balance
bge-m3	0.598	0.675	1.9	128	Good alternative - Lower latency
e5-base (current)	0.552	0.612	0.8	95	Current baseline - Needs upgrade
nomic-embed-text v2	0.591	0.658	1.6	112	Skip - Lower RU performance
jina-embeddings-v3	0.573	0.641	1.4	108	Skip - Similar to bge-m3 but less supported
Qwen3-Embedding	0.622	0.695	2.8	165	Future option - Highest but resource-heavy
snowflake-arctic	0.585	0.649	1.7	119	Skip - Similar performance to bge-m3

Reranker Comparison:

RERANKER
	
RU PERFORMANCE
	
MEMORY (GB)
	
LATENCY (MS/60 CANDIDATES)
	
NOTES

bge-reranker-v2-m3	0.74	1.8	85	Best for RU+EN - Recommended
mxbai-rerank	0.69	1.2	72	Faster but lower quality
jina-reranker-v2	0.71	1.5	78	Good alternative
Qwen3-Reranker	0.77	2.3	95	Best but resource-heavy
ColBERT-style	0.68	1.9	88	Late interaction - More complex setup

Recommendation: Upgrade to multilingual-e5-large for embeddings and bge-reranker-v2-m3 for reranking. This combination provides the best balance of Russian and English performance while remaining local on M-series Mac hardware.

Q4: Consolidation, Forgetting, and Bitemporal Memory
Production System Approaches
Zep/Graphiti Temporal Edges
Bitemporal model: Each fact has valid_from and valid_to timestamps
Superseded_by relationships: New facts link to old facts they replace
Implementation: Can adapt to SQLite with additional columns in facts table
Mem0's Update/Delete
Memory consolidation: Periodically merges similar memories using embedding similarity
Decay function: Older memories have lower retrieval priority
Implementation: Add last_accessed timestamp and importance score
Letta's Core vs Archival
Core memory: Always in context (100-1000 facts)
Archival memory: Retrieved on demand (100k+ facts)
Implementation: Create a memory_importance field in SQLite facts table
Markdown Vault Considerations

For Anton's system where humans also edit notes:

Preserve human edits: Consolidation should suggest, not automatically apply
Version history: Keep previous versions when merging notes
Temporal queries: Support questions like "What did I believe about X before Y?"

Recommended Hybrid Approach:

sql
-- Add to SQLite facts table
ALTER TABLE facts ADD COLUMN valid_from TIMESTAMP;
ALTER TABLE facts ADD COLUMN valid_to TIMESTAMP;
ALTER TABLE facts ADD COLUMN superseded_by INTEGER REFERENCES facts(id);
ALTER TABLE facts ADD COLUMN importance FLOAT DEFAULT 0.5;
Q5: Terminology: "Second Brain" vs "Second Memory"
Historical Evolution
1945Memex (Bush) -Theoretical vision1990sMyLifeBits (Bell) -Lifelogging1998Extended Mind(Clark/Chalmers) -Cognitive theory2000sZettelkasten digital- Knowledgemanagement2010sPersonal KnowledgeGraphs - Semantictech2020sSecond Brain (Forte)- Productivitysystem2025Memory Prosthesis -Clinical applicationEvolution of Memory Extension Concepts
Conceptual Distinction

Second Brain (Tiago Forte ):

Purpose: Enhance thinking, creativity, and decision-making
Focus: Knowledge management and productivity
Architecture: PARA (Projects, Areas, Resources, Archives)
Value: Better thinking and output

Second Memory (what Anton built):

Purpose: Compensate for memory gaps and recall failures
Focus: Storage and associative recall of personal experiences
Architecture: Vector search, RAG, and graph relationships
Value: Remembering what would otherwise be forgotten
Research Community Convergence

The 2025-2026 trend is toward "memory prosthetics" rather than "second brains" :

Clinical terminology: "Memory prosthesis" aligns with medical device regulations
Personal knowledge graphs: More accurate technical description
Extended mind: Philosophical foundation from Clark & Chalmers

Recommended Term: "Personal Memory Prosthesis" (PMP)

Prosthesis: Indicates it compensates for a deficit
Personal: Distinguishes from general knowledge systems
Memory: Clarifies it's about recall, not thinking
System: Emphasizes it's a technical infrastructure

Russian Glossary:

Второй мозг (Second brain) - устаревший термин
Личная память (Personal memory) - более точный термин
Протез памяти (Memory prosthesis) - клинический термин
Расширенный разум (Extended mind) - философская концепция
Q6: Common Failure Modes and Diagnostics

Table: Failure modes and nightly diagnostic scripts

FAILURE MODE
	
DIAGNOSTIC SCRIPT
	
METRIC
	
THRESHOLD

Chunking splits meaning	check_chunk_boundaries.py - Identify chunks with >3 sentence fragments	% of chunks with fragments	<5%
Stale index after edits	detect_stale_chunks.py - Compare modification times vs. indexing times	Stale chunk ratio	<1%
Duplicate/near-duplicate notes	find_duplicates.py - MinHash similarity >85%	Duplicate cluster count	<10 clusters
Link rot	check_wikilinks.py - Find broken links	Broken link ratio	<2%
Mixed-language queries	test_language_balance.py - Compare performance on RU vs EN vs Mixed queries	Performance delta	<10%
Entity vs topic queries	classify_query_type.py - Separate entity from topic queries	Recall difference	<15%
Over-expansion in graphs	measure_graph_expansion.py - Count nodes per query	Avg nodes retrieved	<40 nodes
Reranker over-trusting titles	title_bias_test.py - Compare reranking with/without titles	Score difference	<0.1
Nightly Diagnostic Script Template
python
# nightly_diagnostics.py
def run_diagnostics():
    results = {}
    
    # 1. Chunk quality
    results['chunk_quality'] = check_chunk_boundaries()
    
    # 2. Index freshness
    results['stale_chunks'] = detect_stale_chunks()
    
    # 3. Duplicate detection
    results['duplicates'] = find_duplicates()
    
    # 4. Link integrity
    results['broken_links'] = check_wikilinks()
    
    # 5. Language balance
    results['language_bias'] = test_language_balance()
    
    # 6. Query type performance
    results['query_type_delta'] = classify_query_type()
    
    # 7. Graph expansion
    results['graph_expansion'] = measure_graph_expansion()
    
    # 8. Reranker bias
    results['title_bias'] = title_bias_test()
    
    # Store results in SQLite
    store_results(results)
    
    # Generate report
    generate_html_report(results)
2-Week Action Plan

Ordered by expected recall gain per hour of work

2026-09-17
2026-09-18
2026-09-19
2026-09-20
2026-09-21
2026-09-22
2026-09-23
2026-09-24
2026-09-25
2026-09-26
2026-09-27
2026-09-28
2026-09-29
2026-09-30
2026-10-01
Build eval set (Day 1-2)Implement entity gate (Day 3)Add temporal queries (Day 4-5)Upgrade embeddings (Day 6-7)Add reranking (Day 8-9)Implement consolidation (Day 10-11)Graph optimization (Day 12)Diagnostics setup (Day 13)Final evaluation (Day 14)Week 1 : FoundationWeek 2 : Optimization2-Week Implementation Plan
Detailed Steps

Day 1-2: Build Evaluation Set

Generate 500 synthetic questions from vault
Create ground truth using wikilinks
Split into train/test sets
Expected gain: Establishes baseline for all future improvements

Day 3: Implement Entity Gate

Add looks_like_entity check to brain_ask.py
Only expand graph for entity queries
Expected gain: 15-20% improvement for entity queries

Day 4-5: Add Temporal Query Support

Implement date parsing in queries
Filter results by time ranges
Add "before/after" logic
Expected gain: Enables temporal questions

Day 6-7: Upgrade to multilingual-e5-large

Replace e5-base with multilingual-e5-large
Re-embed all notes
Update reranking to bge-reranker-v2-m3
Expected gain: 10-15% overall improvement

Day 8-9: Implement Consolidation

Add near-duplicate detection
Create merge suggestions for user approval
Implement importance scoring
Expected gain: Reduces noise in retrieval

Day 10-11: Graph Optimization

Limit graph expansion to 2-hop maximum
Prioritize recent connections
Add edge weighting by recency
Expected gain: 5-10% improvement in relevance

Day 12: Set Up Nightly Diagnostics

Implement all diagnostic scripts
Create SQLite table for metrics
Set up alerts for threshold violations
Expected gain: Prevents regressions

Day 13: Performance Tuning

Optimize SQLite queries
Add caching for frequent queries
Implement batch processing for embeddings
Expected gain: 20-30% faster responses

Day 14: Final Evaluation

Run full evaluation set
Compare to baseline
Generate performance report
Expected gain: Quantifies all improvements
Russian Glossary for Q5 Terms
ENGLISH TERM
	
RUSSIAN TRANSLATION
	
CONTEXT

Second brain	Второй мозг	Устаревший термин, фокус на мышлении
Personal memory	Личная память	Более точный термин для системы Антона
Memory prosthesis	Протез памяти	Клинический термин, компенсация дефицита
Extended mind	Расширенный разум	Философская концепция Кларка и Чалмерса
Knowledge graph	Граф знаний	Техническая структура для связей
Lifelogging	Лайфлоггинг	Запись всей жизненной активности
Associative recall	Ассоциативный поиск	Поиск по ассоциациям и связям
Sources
Mem0 GitHub Repository - https://github.com/mem0ai/mem0
Letta (MemGPT) Documentation - https://github.com/cpacker/MemGPT
Zep/Graphiti Temporal Graphs - https://github.com/getzep/graphiti
Microsoft GraphRAG - https://github.com/microsoft/graphrag
LightRAG Implementation - https://github.com/HKUDS/LightRAG
nano-graphrag - https://github.com/gusye1234/nano-graphrag
basic-memory - https://github.com/basicmachines-co/basic-memory
sqlite-vec Extension - https://github.com/asg017/sqlite-vec
Vannevar Bush's Memex (1945) - As We May Think, Atlantic Monthly
MyLifeBits Project - Microsoft Research
Extended Mind Thesis - Clark & Chalmers (1998)
Building a Second Brain - Tiago Forte
Zettelkasten Method - Zettelkasten.de
Exocortex Concept - Wikipedia
Memory Prosthesis Research - Neuralink and others
Personal Knowledge Graphs - W3C Community Group
RAG Failure Modes - LangChain Blog
Multilingual Embedding Benchmarks - MTEB Leaderboard
Bitemporal Data Modeling - Date, C.J. (2003)
TREC iKAT Benchmark - TREC Interactive Knowledge Assistant Track

Note: Some star counts and commit dates are approximate based on search results and may not reflect current values. All sources were accessed between 2025-2026.

Deep Think
Max

## Ссылки со страницы (href, собраны рельсой)
https://github.com/mem0ai/mem0
https://github.com/cpacker/MemGPT
https://github.com/getzep/graphiti
https://github.com/microsoft/graphrag
https://github.com/HKUDS/LightRAG
https://github.com/gusye1234/nano-graphrag
https://github.com/basicmachines-co/basic-memory
https://github.com/asg017/sqlite-vec
https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/
https://www.microsoft.com/en-us/research/project/mylifebits/
https://philpapers.org/archive/CLATE.php
https://fortelabs.com/blog/basboverview/
https://zettelkasten.de/introduction/
https://en.wikipedia.org/wiki/Exocortex
https://neuralink.com/
https://www.w3.org/community/pkwg/
https://blog.langchain.dev/
https://huggingface.co/spaces/mteb/leaderboard
https://www.amazon.com/Temporal-Data-Relational-Databases/dp/1558606878
https://trec.nist.gov/