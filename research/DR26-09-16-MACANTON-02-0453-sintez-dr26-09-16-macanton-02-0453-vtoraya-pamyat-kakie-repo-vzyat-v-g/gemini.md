# DR26-09-16-MACANTON-02-0453 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Gemini
Sign in
New chat
Sign in to save activity
About Gemini
Get Gemini App
Pricing
For Business
Sign in
Rio de Mouro, Portugal
From your IP address
Update location
Conversation with Gemini
You said --- id: DR26-09-16-MACANTON-02-0453 date: 2026-09-16 04:53:34 theme: Вторая память: репозитории GitH…

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

Gemini said
Deep Research: Building and Measuring a Personal "Second Memory" (Graph RAG over a Markdown Vault)
Q1. GitHub Repositories Worth Borrowing From (2025–2026)

To improve sqlite-graph-memory, we surveyed active open-source agent memory and personal RAG frameworks. Below is a ranked evaluation of libraries by their utility to a local, SQLite + Markdown + Wikilink pipeline.

Repository	Last Commit	Stars	License	What it does better	Concrete module/idea to borrow	Integration Cost & Deps	Verdict & Reason
Mem0	Recent (2026)	~25k+	Apache 2.0	Dynamic user memory graph updates (add, update, delete)	Graph memory extraction logic & user-state mutation pattern	5–10 hrs; Python, Pydantic. Heavy reliance on vector DBs/APIs.	Borrow idea: Adapt their entity-extraction prompts for incremental note updates.
Letta (MemGPT)	Recent (2026)	~14k+	Apache 2.0	Hierarchical memory management (core/archival split)	Archival memory block search and agent stateful pagination	20+ hrs; Requires dedicated state server/DB.	Skip: Too heavy; Anton's SQLite + Claude Code skills handle agency better.
Zep / Graphiti	Recent (2026)	~4k+	Apache 2.0	Temporal knowledge graphs with edge invalidation	Bitemporal edge invalidation logic (valid_from, valid_to)	15 hrs; Neo4j or Postgres graph extensions.	Borrow idea: Implement bitemporal valid time states directly in Anton's SQLite tables.
HippoRAG 2	Active	~4k+	MIT	LLM-free or hybrid graph-based retrieval using Personalized PageRank	Personalized PageRank (PPR) over wikilinks/entities	8 hrs; NetworkX integration over existing SQLite edges.	Adopt: Replace simple 1-hop neighbor expansion with PPR over the wikilink graph.
LightRAG	Active	~7k+	MIT	Dual-level retrieval (low-level specific entities vs high-level global themes)	Entity-relation extraction and incremental graph updating	10 hrs; Python, NetworkX.	Borrow idea: Dual-level query routing (broad topic vs specific entity).
sqlite-vec	Active	~3k+	MIT	Vector search directly inside SQLite without external vector DBs	C-extension for vector similarity search in SQLite	2 hrs; SQLite extension loading.	Adopt: Consolidate .npy/.pkl embeddings directly into SQLite using sqlite-vec.
Khoj	Active	~6k+	AGPL-3.0	Personal AI search assistant indexing Markdown/Org-mode	Org/Markdown incremental syncing & hybrid local search	15+ hrs; Heavy dependencies.	Borrow idea: Incremental file watching and sync strategy.
LangMem	Active	~1k+	MIT	Background optimization and reflection of agent memory	Reflection-driven memory compilation	5 hrs; LangChain ecosystem deps.	Borrow idea: Background Python script to synthesize daily logs into consolidated notes.
RAGFlow	Active	~35k+	Apache 2.0	Deep document understanding and layout parsing	Document parsing pipelines for complex layouts	20+ hrs; Heavy Docker setup.	Skip: Overkill for curated personal markdown vaults.
Contextual Chunking (Anthropic pattern)	2024–2026	N/A (Recipe)	MIT	Prepends global document context to individual chunks before embedding	Contextual embedding prompt template	3 hrs; Python script modification.	Adopt: Prepend parent file title and breadcrumbs to chunk text before passing to e5.
Q2. Tests and Benchmarks for Personal Long-Term Memory
State of the Art Evaluation Landscape

Evaluating personal long-term memory relies on benchmarks like LoCoMo (long-context multi-session modeling), LongMemEval, and PersonaMem, which measure how well a system retains facts across extended time horizons and user shifts. Traditional RAG metrics (RAGAS, TruLens) measure context relevance, faithfulness, and answer correctness.

Zero-API-Cost Private Eval Set Recipe

To build a gold standard test set from Anton's 100k-note vault without incurring API costs:

Synthetic Question Generation: Write a local Python script using a local small model (or rule-based extraction) to target notes that contain explicit wikilinks or clear declarative statements.

Ground-Truth Graph Edges: Leverage the existing hand-curated wikilinks as verified positive graph connections. If Note A links to Note B, a query retrieving facts from Note B should surface Note A via graph expansion.

Temporal Questions: Automatically mine notes containing dates to generate questions like "What did I decide about X before [Date Y]?"

Hard Negatives: Inject near-duplicate chunks (mined via local embedding cosine similarity > 0.9) to test if the reranker correctly surfaces the unique variant.

Scoring brain_ask.py --ab: Run the test set through both modes to compute Recall@K, MRR (Mean Reciprocal Rank), nDCG, and Faithfulness.

The Top 5 Tests to Run First

Entity-Specific Recall@12: Can the pipeline retrieve the exact note containing an obscure project name? (Target: > 85%)

Multi-Hop Wikilink Traversal: Does 1-hop graph expansion pull in the parent concept note when querying a child note? (Target: MRR > 0.70)

Temporal Precedence Test: Does the system respect bitemporal ordering when the user asks about superseded decisions? (Target: 100% accuracy on dated conflicts)

Near-Duplicate Suppression: Does the reranker penalize redundant chunks in the top-12 output? (Target: < 10% redundancy)

Mixed-Language Retrieval (RU+EN): Can an English query retrieve a relevant Russian note (and vice versa)? (Target: Recall@12 > 75%)

Q3. Model Upgrades That Are Cheap and Local

To upgrade multilingual (RU+EN) embedding and reranking performance on Apple Silicon (M-series Mac) or a consumer NVIDIA GPU, compare the following local options:

Model	Type	Size / Footprint	MTEB / Multilingual Strength (RU/EN)	Latency (60 candidates)	Recommendation
intfloat/multilingual-e5-small	Embedding	~470 MB	Strong baseline across Russian and English	~15ms	Current baseline, fast.
intfloat/multilingual-e5-base	Embedding	~1.1 GB	Solid balance of speed and cross-lingual alignment	~35ms	Current default in sqlite-graph-memory.
intfloat/multilingual-e5-large	Embedding	~2.2 GB	Excellent RU/EN performance	~90ms (Mac CPU/GPU)	Adopt: Worth the latency hit for superior retrieval.
BAAI/bge-m3	Embedding	~2.2 GB	Top-tier multi-vector (dense + sparse + colbert)	~110ms	Great alternative if exploring hybrid sparse search.
jinaai/jina-embeddings-v3	Embedding	~560 MB (LoRA adapters)	State-of-the-art multilingual performance	~40ms	Top Pick: Highly flexible and memory efficient.
BAAI/bge-reranker-v2-m3	Reranker	~2.2 GB	Outstanding cross-encoder for RU+EN	~120ms	Adopt: Drop-in replacement to boost precision of top-60 to top-12.
jinaai/jina-reranker-v2-base-multilingual	Reranker	~1.5 GB	Excellent multilingual listwise/pointwise reranking	~80ms	Strong alternative for fast reranking.
Q4. Consolidation, Forgetting, and Bitemporal Memory
Production Strategies

Consolidation/Compaction: Running periodic background scripts that cluster embedding vectors of notes created within the same week. If cosine similarity > 0.92, an agent merges them into a canonical summary note, archiving the outdated versions.

Decay and Forgetting: Eschewing raw Ebbinghaus curves for personal notes (since user notes are intentionally curated), but applying a dampening factor based on access telemetry (turnstate_hook.py read counts). Unaccessed notes drift lower in dense retrieval unless explicitly anchored by wikilinks.

Bitemporal Validity: Adopting Zep/Graphiti-style tracking via two time axes:

Valid Time: When the fact was true in reality (valid_from, valid_to).

Transaction Time (recorded_at): When Anton's vault captured it. This prevents historical corruption when old notes are edited.

Contradiction Handling: When a new note explicitly contradicts an older wikilink-connected note, the graph injects a superseded_by edge, automatically filtering out the stale fact during retrieval unless historical mode is requested.

Q5. Terminology: "Second Brain" vs "Second Memory"
Conceptual Evolution

Vannevar Bush’s Memex (1945): The foundational concept of a mechanized private file and library system for associative indexing.

Gemmell & Bell's MyLifeBits (Microsoft Research): Lifelogging and total digital capture of a human's artifacts and experiences.

Clark & Chalmers' Extended Mind (1998): The philosophical thesis that external objects (notebooks, digital files) can literally constitute part of the cognitive architecture.

Wegner’s Transactive Memory: Division of memory storage across social or external technological systems.

Tiago Forte’s "Building a Second Brain" (PARA): Action-oriented productivity framework for organizing digital notes to get things done (Projects, Areas, Resources, Archives).

Zettelkasten & Exocortex / Memory Prosthesis: Systems designed for emergent ideation or direct structural augmentation of neural pathways.

Definitional Boundary

Second Brain: A thinking and decision-making layer (action-oriented, productivity-focused, synthesis, task execution, workflow management like PARA).

Second Memory: A storage, telemetry, and associative recall layer (passive ingestion, graph retrieval, bitemporal truth tracking, precise historical recall without executive overhead).

Consensus (2025–2026): While product marketing heavily leans on "Second Brain," AI systems researchers and engineers building local RAG pipelines increasingly converge on Second Memory or Exocortex because the bottleneck is rarely thinking—it is faithful, low-latency retrieval of forgotten personal context.

Q6. What Usually Breaks in Personal RAG and How to Diagnose It

Chunking that Splits Meaning:

Symptom: Mid-sentence cuts split context across chunks.

Diagnostic: Run a script checking for orphan Markdown headers without body context.

Fix: Use parent-child chunking with overlapping window sizes.

Stale Index After Edits:

Symptom: Modified notes return outdated search hits.

Diagnostic: Compare file modification timestamps (mtime) against SQLite index timestamps (indexed_at).

Duplicate / Near-Duplicate Notes:

Symptom: Cluttered retrieval results filled with the same thought written twice.

Diagnostic: Query pairwise note embeddings for cosine similarity >0.95.

Link Rot / Broken Wikilinks:

Symptom: Graph expansion hits dead ends or points to non-existent notes.

Diagnostic: Static analysis script parsing all wikilinks against actual vault filenames.

Mixed-Language Query Blindness:

Symptom: Russian query fails to surface English source material.

Diagnostic: Test queries with synthetic cross-lingual retrieval pairs and measure Recall@12 delta.

Reranker Over-Trusting Titles:

Symptom: Reranker boosts irrelevant notes just because the title matches query keywords.

Diagnostic: Inspect cross-encoder score distributions where body text contradicts title themes.

Recommended Action Plan (Next 2 Weeks)

Migrate Embeddings to SQLite: Integrate sqlite-vec to eliminate separate .npy/.pkl file management.

Upgrade to Large Embeddings: Switch from multilingual-e5-base to multilingual-e5-large or jina-embeddings-v3 for improved RU+EN recall.

Implement Contextual Chunking: Prepend parent file titles and breadcrumbs to chunks before embedding.

Add Bitemporal Columns: Update SQLite schema with valid_from and valid_to fields inspired by Zep/Graphiti.

Build a Zero-Cost Eval Set: Generate 50 synthetic test queries from existing wikilink pairs in the vault.

Benchmark Baseline: Score current recall using brain_ask.py --ab against the new eval set.

Incorporate Personalized PageRank: Borrow HippoRAG-style PPR graph traversal over the existing wikilink SQLite edges.

Automate Stale Index Sweeps: Add a nightly check comparing file mtime to index timestamps.

Deploy Background Consolidation: Write a script to detect and merge duplicate notes with similarity > 0.92.

Re-measure and Tune: Run final evaluation metrics to confirm improvements in Recall@12 and MRR.

Source Citations

Mem0 Repository: https://github.com/mem0ai/mem0 (Accessed 2026)

Letta (MemGPT) Repository: https://github.com/letta-ai/letta (Accessed 2026)

Zep / Graphiti Repository: https://github.com/getzep/graphiti (Accessed 2026)

HippoRAG 2 Repository: https://github.com/OSU-NLP-Group/HippoRAG (Accessed 2026)

LightRAG Repository: https://github.com/HKUDS/LightRAG (Accessed 2026)

sqlite-vec Repository: https://github.com/asg017/sqlite-vec (Accessed 2026)

Khoj Repository: https://github.com/khoj-ai/khoj (Accessed 2026)

LangMem Repository: https://github.com/langchain-ai/langmem (Accessed 2026)

RAGFlow Repository: https://github.com/infiniflow/ragflow (Accessed 2026)

Anthropic Contextual Retrieval Cookbook: https://github.com/anthropics/anthropic-cookbook (2024–2026)

What specific aspect of the zero-cost evaluation set generation would you like to script out first?

Sign in to connect to Google apps, create images, and more
Sign in




Flash-Lite

Gemini isn’t human. It can make mistakes, including about people, so double-check it. Your privacy & Gemini
Opens in a new window

## Ссылки со страницы (href, собраны рельсой)
https://github.com/mem0ai/mem0
https://github.com/letta-ai/letta
https://github.com/getzep/graphiti
https://github.com/OSU-NLP-Group/HippoRAG
https://github.com/HKUDS/LightRAG
https://github.com/asg017/sqlite-vec
https://github.com/khoj-ai/khoj
https://github.com/langchain-ai/langmem
https://github.com/infiniflow/ragflow
https://github.com/anthropics/anthropic-cookbook