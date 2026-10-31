# DR26-09-16-MACANTON-02-0453 — the exact prompt sent to every model

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
