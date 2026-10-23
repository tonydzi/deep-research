# DR26-09-16-MACANTON-02-0453 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll gather live sources first: the existing stack, candidate repos, eval benchmarks, multilingual models, and the second-brain vs second-memory literature.Next I’ll open the live GitHub pages and primary papers so stars, licenses, last-commit dates, and benchmark numbers are taken from sources, not memory.I’ll open the remaining repo pages and primary papers so Q1–Q3 numbers come from live GitHub and arXiv, not secondary recaps.A few remaining live pages: repo metadata I still lack, Russian embedding scores, Graphiti’s temporal model, and the terminology sources.I have enough live sources. The report below is the full deliverable.

**A. Executive verdict**

Anton is already on the right architecture. The stack in `sqlite-graph-memory` (dense retrieve → optional 1-hop `wikilinks` → cross-encoder rerank → `--ab` telemetry) is closer to what actually works on a human-edited markdown vault than Mem0, LightRAG, or Microsoft GraphRAG. Those systems win vendor leaderboards by extracting a *new* graph with an LLM. Anton already has a better graph: hand-curated wikilinks. Do not replace it.1(https://github.com/tonydzi/sqlite-graph-memory)

The bottleneck is not “need a graph database.” It is: (1) no gold set, so `--ab` cannot tell him *which query class* the graph helps; (2) e5-base + an old MiniLM reranker leave RU+EN quality on the table; (3) the index is a snapshot (`.npy/.pkl`) with no freshness/dedup/forgetting loop; (4) bitemporal validity is designed (`docs/bitemporal.md`) but not implemented.1(https://github.com/tonydzi/sqlite-graph-memory)

Highest recall-per-hour path for the next 14 days: **private gold set from the vault → score `--ab` with recall@k/nDCG → swap embedder to BGE-M3 or Qwen3-Embedding-0.6B → swap reranker to bge-reranker-v2-m3 or Qwen3-Reranker-0.6B → hybrid BM25+dense → incremental index by mtime → then HippoRAG-style Personalized PageRank on the existing wikilink graph.** Do not adopt Neo4j, do not run Microsoft GraphRAG over 100k notes, do not wait for a vendor LoCoMo number. Those numbers are not comparable across papers.2(https://mnemoverse.com/docs/research/evaluation/ai-memory-benchmarks-field-guide)

Name: keep **second memory** for the storage+associative-recall layer. Keep **second brain** for the agent/skills/decision layer. Research 2025–2026 converges on “agent memory” / “AI memory”; product marketing still says “second brain.” Anton’s split is the more precise one.3(https://vectorize.io/articles/brain-stack-second-company-single-brain)

---

**Q1. GitHub repositories to borrow from (ranked by fit to SQLite + markdown + wikilinks)**

Stars, dates, and licenses below are from pages opened this run. Where the GitHub star widget failed to load, the figure is taken from a second live page and marked. “Hours” = engineering to lift *one module* into the existing three-script pipeline, not a rewrite.

| Rank | Repo | Last commit / activity (this run) | Stars | License | Better than current pipeline at | One concrete borrow | Cost | Verdict |
|---|---|---|---|---|---|---|---|---|
| 1 | HippoRAG 2 (`OSU-NLP-Group/HippoRAG`) https://github.com/OSU-NLP-Group/HippoRAG | Repo live; 162 commits. Exact last-commit date **not found** on loaded page. Paper ICML 2025. | **not found** (page widget failed) | LICENSE present; SPDX **not confirmed** this run | Multi-hop / associative memory without killing factual RAG. PPR over a KG + passage nodes. Reports ~7% lift on associative tasks vs SOTA embeddings. | Personalized PageRank over *existing* `wikilinks` + chunk nodes, not LLM-extracted triples. File-level: `src/hipporag` retrieval/PPR path. Seed = top vector hits (already have). | 8–16h. NetworkX/igraph in-process. No graph DB. Optional LLM OpenIE: **do not take**. | **borrow idea** |
| 2 | Graphiti (`getzep/graphiti`) https://github.com/getzep/graphiti | 2026-09-11 | Vendor: 20,000+ (exact GitHub count **not loaded**) | Apache-2.0 | Bi-temporal edges: `valid_at` / `invalid_at` / `created_at` / `expired_at`. Fact invalidation, not deletion. Hybrid vector+FTS+graph. Claimed LoCoMo 94.7% / LongMemEval 90.2% (vendor). | Edge schema + invalidation policy. Map onto vault frontmatter + `schema.sql`. Class: `EntityEdge` (`valid_at`, `invalid_at`, `created_at`, `expired_at`). | Full adopt: days + Neo4j/FalkorDB/Neptune. Idea-only: 4–8h SQL columns. | **borrow idea** (skip the service) |
| 3 | sqlite-vec (`asg017/sqlite-vec`) https://github.com/asg017/sqlite-vec | Exact last commit **not found**. 464 commits, 8.1k stars loaded. | 8.1k | Apache-2.0 OR MIT | Puts KNN in the same SQLite file as `ab_recall` and `turns`. Incremental DELETE/INSERT by rowid. Metadata columns for mtime/hash. | Replace `.npy/.pkl` with `vec0` virtual table. Keep numpy only as export. | 4–10h. C extension; already runs on Mac/Win. | **adopt** |
| 4 | Mem0 OSS (`mem0ai/mem0`) https://github.com/mem0ai/mem0 | Org push 2026-09-15; release 2026-09-10 | ~65k (org page 65k; ReleaseAlert 65.4k) | Apache-2.0 | Hybrid retrieval: semantic + BM25 + entity match, fused. Open eval harness `mem0ai/memory-benchmarks`. Apr 2026 algo: ADD-only, no overwrite. | Fusion scorer + FTS5 keyword lane beside dense. Entity linking as a *boost*, not a replacement for wikilinks. Eval scripts from `memory-benchmarks`. | Fusion: 6–12h. Full Mem0: service + LLM extract, **do not adopt**. | **borrow idea** |
| 5 | Qwen3-Embedding / Reranker https://github.com/QwenLM/Qwen3-Embedding | Paper 2025-06-05; models Apache-2.0 | n/a (model repo) | Apache-2.0 | Multilingual MTEB: 8B = 70.58 (Jun 2025). 0.6B already 64.33 vs mE5-large-instruct 63.22 and BGE-M3 59.56 on that table. Reranker-0.6B beats bge-reranker-v2-m3 on MTEB-R (65.80 vs 57.03) in Qwen’s run. | Drop-in `E5_MODEL` / `RERANK_MODEL` constants in `brain_ask.py`. Start with 0.6B embed + 0.6B rerank. | 2–6h + reindex. Local. | **adopt** (models, not the training repo) |
| 6 | BGE-M3 + bge-reranker-v2-m3 https://huggingface.co/BAAI/bge-m3 | Paper 2024; still the local hybrid default in 2026 roundups | n/a | MIT | Dense+sparse+ColBERT heads, 100+ languages, 8k ctx. rus-MIRACL nDCG@10 **70.50** dense vs mE5-large **66.99** and mE5-base **61.41**. + rerank → **76.44**. | Use as embedder (and optional sparse/lexical head). Pair with `BAAI/bge-reranker-v2-m3`. | 2–6h + reindex. ~568M, laptop-ok. | **adopt** (alternative to Qwen3-0.6B; A/B them) |
| 7 | basic-memory (`basicmachines-co/basic-memory`) https://github.com/basicmachines-co/basic-memory | Very active (SourcePulse: last commit ~21h as of sync; 4.0k stars loaded) | 4.0k | AGPL-3.0 | Markdown is source of truth; observations/relations in notes; sync; optional cross-encoder rerank. Closest product cousin of the vault. | Observation/relation note format; file-watcher incremental index (`src/basic_memory/sync`, `indexing`). | Idea: 4h. Full product: AGPL copyleft — **do not vendor**. | **borrow idea** |
| 8 | A-MEM (`agiresearch/A-mem`) https://github.com/agiresearch/A-mem | Research repo; MIT | 1.2k | MIT | Zettelkasten-style notes that grow tags/links at write time. Paper arXiv:2502.12110, NeurIPS 2025. | When ingesting a new note, propose 3–5 links/tags from nearest neighbours (human confirms). `memory_layer.py` / note+link update loop. | 8–20h. LLM-on-write cost. | **borrow idea** |
| 9 | txtai (`neuml/txtai`) https://github.com/neuml/txtai | Live; ~12.9–13k stars | ~12.9k | Apache-2.0 (from catalog; confirm LICENSE if vendoring) | Embeddings DB = vectors + graph + SQL in one library. GraphRAG examples without a separate graph product. | Graph overlay API / SQL-over-vectors pattern. | 8–20h if swapping store; 2h if only reading patterns. | **borrow idea** |
| 10 | Letta / MemGPT (`letta-ai/letta`) https://github.com/letta-ai/letta | README commits 2026-09-10; runtime moved to `letta-code` | 24.8k | Check LICENSE before use (**not loaded**) | Core vs archival vs recall memory. Sleep-time / memory-reflection in a git worktree. | Nightly “sleep” job: consolidate duplicates into a summary note, leave originals. Do not take the agent OS. | 6–12h as a cron. Full Letta: wrong shape. | **borrow idea** |
| 11 | Cognee (`topoteretes/cognee`) https://github.com/topoteretes/cognee | Very active (10,270 commits; PyPI 1.5.4 on 2026-09-04) | ~30.7k on one README load; older pages say less — **treat as ~15–30k, verify** | Apache-2.0 (docs/PyPI) | `remember` / `recall` / `forget` / `improve`. Default local stack SQLite+LanceDB. Session vs permanent memory. | API names + `forget()` for superseded notes. Not the ECL extraction pipeline (too LLM-heavy for 100k notes). | Full: days. Idea: 2–4h. | **borrow idea** |
| 12 | LangMem (`langchain-ai/langmem`) https://github.com/langchain-ai/langmem | Last push 2026-09-09 | 1.7k | MIT | Hot-path tools vs background `create_memory_store_manager` consolidation. | Background consolidator as a nightly job, not in the recall path. | 4–8h conceptually. LangGraph store lock-in if adopted whole. | **borrow idea** |
| 13 | LightRAG (`HKUDS/LightRAG`) https://github.com/HKUDS/LightRAG | 2026-09-15 | 39.7k | MIT (agents-oss listing) | Dual-level keywords; local/global/hybrid/mix/naive modes. Cheaper than MS GraphRAG. | Query-mode switch: entity vs theme (you already have `looks_like_entity`). `lightrag/operate.py` + `rerank.py`. | Full: LLM entity extract over 100k notes = expensive and fights hand links. | **skip** full; optional mode names |
| 14 | Khoj (`khoj-ai/khoj`) https://github.com/khoj-ai/khoj | 2026-08-02 | 37.4k | AGPL-3.0 | Mature personal “second brain” app, Obsidian plugin, local models. | Nothing modular enough. Product, not a library. Cloud deprecation banners in 2026 commits. | High. AGPL. | **skip** |
| 15 | PageIndex (`VectifyAI/PageIndex`) https://github.com/VectifyAI/PageIndex | 2026-09-15 | 35.7k | **not confirmed** this run | Vectorless tree index for long PDFs. FinanceBench claim 98.7% (vendor). | Hierarchical TOC navigation for *long single notes* (books, DR dumps). Not for a 100k-note vault. | High LLM-at-query cost. | **skip** (wrong unit of retrieval) |

**Also checked, verdict skip unless noted:**

- **Microsoft GraphRAG** (`microsoft/graphrag`) — last commit 2026-08-24; **maintenance mode**, no new features. Leiden communities + LLM summaries. Wrong problem (corpus-level sensemaking), huge index cost. Skip. https://github.com/microsoft/graphrag
- **nano-graphrag** (`gusye1234/nano-graphrag`) — ~3.7–3.8k, ~1100 LOC clone of MS GraphRAG. Same extraction assumption. Skip.
- **Memori** (`GibsonAI/Memori` / MemoriLabs) — LoCoMo 87% claimed at 721 tokens/query. Conversation/tool-trace memory, not a vault. Skip.
- **memary** (`kingjulio8238/Memary`) — 2.6k, MIT, Neo4j + LlamaIndex, last release v0.1.5 dated 22 Oct (year not on that page). Stale relative to Graphiti. Skip.
- **MemoRAG** (`qhjqhj00/MemoRAG`) — 2.3k; needs a 7B “memory model” for global clues. Wrong hardware budget. Skip.
- **supermemory** — cloud memory API; company-brain product discontinued 2026-09-10. Skip.
- **RAGFlow** — 90.8k, full engine/UI. Skip (too big).
- **R2R** (`SciPhi-AI/R2R`) — ~8k. Production RAG service. Skip.
- **Verba** (Weaviate) — demo app. Skip.
- **Chroma** — 29.3k, Apache-2.0, commit 2026-09-15. Only if sqlite-vec fails at 100k×chunks. Prefer sqlite-vec.
- **LanceDB** — strong embedded store; extra dependency vs SQLite. Skip unless sqlite-vec ANN is too slow.
- **Obsidian Smart Connections** (`brianpetro/obsidian-smart-connections`) — 5.5k, plugin UX. Copilot plugin is AGPL, 1.8M downloads, last update “yesterday” as of 2026-09-09 listing. Skip as a library; Copilot V4 embedding local-search is a UX peer, not a backend.
- **ColBERT / PLAID / jina-colbert-v2** — better matching, 2–3× storage. Revisit only after single-vector + rerank plateaus.
- **LangMem/Mem0 graph modes, Cognee ontologies** — LLM-extracted graphs will *fight* hand wikilinks (duplicate, worse edges). HippoRAG paper is explicit: graph RAG often *hurts* factual tasks unless passages stay first-class. You already learned this with the entity gate.4(https://huggingface.co/papers/2502.14802)

**What the landscape is actually better at than Anton (honest):**

- **Measurement** (Mem0 `memory-benchmarks`, HippoRAG eval splits, LongMemEval axes).
- **Hybrid lexical+dense** (Mem0 Apr 2026, Graphiti, BGE-M3 sparse head).
- **Temporal invalidation** (Graphiti four timestamps).
- **Write-time organization** (A-MEM, basic-memory observations).
- **Index-as-SQL** (sqlite-vec, Cognee default SQLite+LanceDB).

They are **not** better at: owning a human-curated markdown graph, running with zero extra service, or staying editable by hand.

---

**Q2. Tests and benchmarks — and a private gold set at $0 API**

**State of the art (two families).** Strict memory (store now, recall later, multi-session) vs long-context (one huge prompt). Do not mix scores.2(https://mnemoverse.com/docs/research/evaluation/ai-memory-benchmarks-field-guide)

| Benchmark | What it tests | Scale (as reported 2026) | Use for Anton? |
|---|---|---|---|
| **LoCoMo** (Maharana et al., ACL 2024, arXiv:2402.17753) https://github.com/snap-research/locomo | Multi-session conversational QA: single-hop, multi-hop, temporal, open-domain, adversarial | Paper: 50 conv / ~7.5k Q. Vendor harness often 10 conv / 1,986 Q. | **No as headline.** Saturating; two different tests share the name; vendor 90%+ not comparable. Use only to steal *question types*. |
| **LongMemEval** (Wu et al., ICLR 2025, arXiv:2410.10813) https://github.com/xiaowu0162/LongMemEval | 5 abilities: extraction, multi-session, temporal, **knowledge update**, **abstention** | 500 Q; S ~115k tok, M larger | Steal the **update + abstention** axes for vault eval. Do not chase 94% vendor scores. |
| **LongMemEval-V2** (arXiv:2605.12493, May 2026) | Agent-trajectory memory (state, workflows, gotchas) | 451 Q | Later, for skills/agent memory — not the vault retriever. |
| **BEAM** | Contradiction + event order at 128k–10M tok | 2,000 Q | Optional stress test; overkill for v0.2. |
| **PersonaMem / v2** | Dynamic user profile | 5k–6k Q | Close to “who is X / what did I prefer.” Useful template, not the corpus. |
| **MSC** (2021–22) | Multi-session chat | 5k conv | Historical. Skip. |
| **MemBench / MemoryAgentBench / EverMemBench / LifeBench** | 2025–26 agent memory | various | Read taxonomies; do not run until private gold exists. |
| **BEIR / MIRACL / rus-MIRACL** | Retrieval IR metrics | public | Use **metrics** (nDCG@k, MRR, Recall@k), not the English corpora. rus-MIRACL is the right public probe for the embedder. |
| **RAGAS / DeepEval / ARES / TruLens / Giskard / Phoenix** | Faithfulness, answer relevancy, context precision | libraries | Run **locally** with a small judge (Qwen/Llama) after retrieval metrics are green. RAGAS faithfulness is the first generation metric. |
| **TREC iKAT** | Interactive knowledge assistance | TREC | Too dialogue-heavy. Skip. |
| **NIAH / RULER** | Needle-in-haystack | long-context | Tests the LLM window, not the retriever. One diagnostic only. |

**Vendor scores are marketing.** Mem0 reports LoCoMo 92.5 / LongMemEval 94.4 (Apr 2026, platform, not necessarily OSS). Graphiti/Zep report LoCoMo 94.7% @ 155ms and LongMemEval 90.2%. Independent re-runs (Mnemoverse bench-v1, locked 2026-09-02) put stock systems much lower and show naive cosine sometimes beating “memory.” Treat any single % as **untrusted** until the harness, subset, and judge prompt are named.5(https://github.com/mem0ai/mem0)

**Private eval set from the vault (zero API).** This is the only number that should move product decisions. Matches the v0.2 plan already written in the sqlite-graph-memory README.1(https://github.com/tonydzi/sqlite-graph-memory)

Recipe:

1. **Sample notes, not random chunks.** Stratify: people cards, project cards, decisions (`02-Decisions`), concepts, daily notes, RU-only, EN-only, mixed. Target 300–500 notes / 200 queries (README already says 200–500 notes / ~200 queries).
2. **Synthetic questions from the note, locally.** Use a local 7–8B instruct (already on the Windows box). Prompt: “Write 1 factoid, 1 multi-hop, 1 temporal, 1 ‘what superseded X’ question whose answer is *only* in this note. Output JSON {q, answer_span, note_path}.” Human spot-check 20%.
3. **Wikilinks as gold graph edges.** For each query, gold files = (a) source note, (b) `neighbours` if the question is a bridge (“how X relates to Y”). A retrieval hit on (a) = relevant; hit on (b) but not (a) = partial. This is free, already curated, better than LLM-extracted triples.
4. **Temporal questions.** Parse `date_established`, `valid_from`/`valid_to`, decision dates. Templates: “What did I decide about X before DATE?”, “What replaced Y?” Gold = the note whose validity window contains the as-of date.
5. **Hard negatives.** Near-duplicate notes (same title stem, cosine > 0.92, or identical first heading). Gold is the *current* note (`valid_to` empty). Retrieving the stale twin is a labeled failure — this is the contradiction/update axis from LongMemEval.
6. **Query classes (required).** entity / theme / bridge / compare / temporal / navigational — as in the repo roadmap. Report metrics **per class**, or the entity gate will look like a regression.
7. **Scoring `brain_ask.py --ab`.** For each query run Direct (vector) and Associative (vector+graph). Metrics, no LLM required:
   - Recall@k, k ∈ {5,12,60} (12 = current rerank cutoff; 60 = candidate pool).
   - MRR and nDCG@12 on the gold file set (binary relevance is enough).
   - Graph delta: `new_in_top`, `promoted_via_graph` already in `ab_recall`.
   - Stale-hit rate: fraction of top-12 whose `mtime` < note `mtime` or `valid_to` is set.
8. **Generation metrics (second wave).** With retrieved context frozen, local LLM answers. RAGAS faithfulness + answer correctness vs the gold span. If the judge is local, cost is electricity.

**Five tests to run first, and a “good” number:**

| # | Test | Good (proposed; not an industry standard) |
|---|---|---|
| 1 | Recall@12 on **entity** queries, vector-only | ≥ 0.85 (name cards should be easy) |
| 2 | Recall@12 on **bridge/multi-hop**, vector+graph minus vector-only | Graph **+0.10** Recall@12 or it is not earning the hop |
| 3 | nDCG@12 **temporal/update** queries (current fact, not the superseded twin) | ≥ 0.75; stale twin in top-12 ≤ 10% |
| 4 | Index freshness: % chunks whose source mtime > indexed_at | **0** on a nightly run |
| 5 | Faithfulness on 50 held-out answers (local RAGAS) | ≥ 0.80; if recall is high and this is low, the bug is chunking/context, not retrieval |

If (1) fails, fix embeddings/chunking before touching the graph. If (1) is fine and (2) is not, the hop/entity gate/PPR is the lever. If (3) fails, bitemporal validity beats any reranker.

---

**Q3. Cheap local model upgrades (RU+EN)**

Current pipeline (from the live README): embed `intfloat/multilingual-e5-base`; rerank `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1`. Both laptop-sized. That is a 2024 default, not a 2026 one.1(https://github.com/tonydzi/sqlite-graph-memory)

**Embeddings**

| Model | Params / ctx / dim | RU / multilingual numbers (this run) | License | Footprint (approx.) | Rec |
|---|---|---|---|---|---|
| **multilingual-e5-base** (current) | ~278M / 512 / 768 | rus-MIRACL nDCG@10 **61.41** | MIT | ~1 GB | Baseline. Upgrade. |
| multilingual-e5-large | ~560M / 512 / 1024 | rus-MIRACL **66.99**; MMTEB (Qwen table) mE5-large-instruct **63.22** | MIT | ~2 GB | Better RU than base; beaten by BGE-M3 on rus-MIRACL. |
| **BGE-M3** | ~568M / 8k / 1024; dense+sparse+multi-vec | rus-MIRACL **70.50**; MIRACL-all dense ~67.8 / all-heads ~70.0; MMTEB **59.56** | MIT | ~2 GB | **First A/B.** Hybrid heads + MIT + 8k ctx. |
| **Qwen3-Embedding-0.6B** | 0.6B / 32k / 1024 MRL | MMTEB **64.33** (Jun 2025 table) | Apache-2.0 | ~1.5–2 GB | **First A/B** vs M3. Long ctx helps whole-note embed. |
| Qwen3-Embedding-4B / 8B | 4B / 8B / 32k | MMTEB **69.45 / 70.58** | Apache-2.0 | 8B is a GPU job | Desktop-only if 0.6B wins the A/B. |
| jina-embeddings-v3 | 570M / 8k / 1024 + LoRA | MTEB 65.52; MMTEB 64.44; late-chunking native | **CC-BY-NC** weights | ~2 GB | Strong, but NC license. Skip for a public MIT repo unless API. |
| nomic-embed-text v1.5/v2 | — | **RU numbers not found this run** | — | — | Do not pick without a RU probe. |
| snowflake-arctic-embed | — | MIRACL-VISION RU text 0.8242 vs bge-m3 0.8363 (different task) | — | — | Not enough RU evidence. Skip for now. |

**Rerankers (60 candidates — this is the hot path)**

| Model | Size | Numbers (this run) | ~Latency | Rec |
|---|---|---|---|---|
| mmarco MiniLM L12 (current) | ~118M | **no RU/MIRACL number found this run** | Fast on CPU | Replace. |
| **bge-reranker-v2-m3** | 0.6B | MIRACL **69.32** (best multilingual in Jina v3 paper); rus-MIRACL mE5-large+BGE **75.90**, BGE-M3+BGE **76.44** | ~12 ms/pair on L40S → ~0.7s / 60 (GPU; Mac will be slower) | **Default next reranker.** MIT. |
| **Qwen3-Reranker-0.6B** | 0.6B | Qwen table: MTEB-R **65.80** vs BGE-v2-m3 **57.03** (their harness). Jina paper: BEIR 56.28, MIRACL 57.70 — **conflict across harnesses.** | Similar class to BGE 0.6B | A/B against BGE-v2-m3 on the **private** gold set. |
| jina-reranker-v3 | 0.6B listwise | BEIR 61.94; MIRACL 66.50–66.83; **Russian 65.20** | Listwise = 60 docs in one window; memory-heavy | Good if license OK; check NC. |
| Qwen3-Reranker-4B | 4B | Strongest open rerank in Qwen’s table (MTEB-R 69.76) | ~42 ms/pair class | Desktop GPU only. |
| mxbai-rerank-base/large-v2 | 0.5B / 1.5B | BEIR 58.40 / 61.44; MIRACL 55.32 / 57.94 — **weak multilingual** | — | Skip for RU+EN. |
| ColBERT v2 / Jina-ColBERT v2 | ~0.5B multi-vec | Superlinked: ~10% over single-vec; storage 2–3× | Index + score | Defer. |

**Recommendation:** On Mac, A/B **BGE-M3 vs Qwen3-Embedding-0.6B** as retriever, both with **bge-reranker-v2-m3**. Keep e5-base as the control in `--ab` (add a model tag column). Promote Qwen3-4B embed/rerank only if the 0.6B pair wins and the desktop GPU is idle. Do not use jina weights in the public MIT repo without a license read (CC-BY-NC showed up for v3 embeddings).

---

**Q4. Consolidation, forgetting, bitemporal — what fits a hand-edited vault**

**What production systems actually do:**

- **Graphiti / Zep:** bi-temporal edges. Valid time (`valid_at`/`invalid_at`) vs transaction time (`created_at`/`expired_at`). New fact *closes* the old edge; history remains. Provenance to an “episode.” This is the same idea as Anton’s `docs/bitemporal.md` and the memory-record close-by-fields rule. **This is the one to copy.** https://arxiv.org/abs/2501.13956
- **Mem0 (old):** UPDATE/DELETE via LLM memory manager. **Mem0 (Apr 2026):** they *abandoned* overwrite — ADD-only, memories accumulate. That is an admission that LLM update/delete was a source of silent loss. Do not build a robot that rewrites Anton’s notes.
- **Letta:** core (always in context, small, self-edited) vs archival (vector) vs recall (transcript search). Sleep-time reflection writes summaries in a git worktree and merges. Fits as a **nightly job**, not as a replacement for Obsidian.
- **Cognee:** `improve` / `forget`; session cache vs permanent graph.
- **A-MEM:** links and tags evolve when a new note arrives (Zettelkasten).
- **Cognitive science:** Ebbinghaus decay and spaced repetition optimize *human restudy*, not machine indexes. Blind TTL-decay on notes will hide rare but load-bearing decisions (the opposite of what a memory-impaired user needs). Use decay only as a **ranking prior** (prefer recent *and* still-valid), never as deletion.

**What fits a markdown vault humans also edit:**

| Mechanism | Do | Do not |
|---|---|---|
| Consolidation | Nightly: cluster near-dup chunks (cosine + same heading); write a `superseded_by` link; **leave the old note on disk**. Optional LLM merge into a summary note, human-visible. | Auto-delete or auto-rewrite body text. |
| Forgetting | Retrieval filter: `valid_to IS NULL` by default; `as_of` for history. Rank penalty for age *inside* the valid window. | Ebbinghaus-delete. |
| Bitemporal | Frontmatter + SQL: `valid_from`, `valid_to`, `superseded_by`, `observed_at`. Close rows, don’t UPDATE in place. Same fields already required in Anton’s memory canon. | A second graph DB that can diverge from the file. |
| Contradictions | Two valid notes that disagree → surface both with dates (LongMemEval update axis). Prefer the one with later `valid_from` if windows overlap. | LLM “resolve” by picking a side. |
| Provenance | Every chunk row: `path`, `mtime`, `hash`, `indexed_at`. Every answer citation = path + heading. | Embeddings without a file pointer. |

Graphiti’s four timestamps are the industrial version of what the vault already believes. Implement them in `schema.sql` and note YAML. Do not stand up Neo4j to get them.

---

**Q5. Terminology: second brain vs second memory**

**Lineage (primary ideas, not a literature review):**

- **Memex** (Vannevar Bush, *As We May Think*, *The Atlantic*, July 1945): a personal store of books, records, communications, consulted by **associative trails** — “the process of tying two items together is the important thing.” That is recall + association, not reasoning. Bush’s own name is a **memory extender**. https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/ (Atlantic original; also circulated as reprints).
- **MyLifeBits** (Gemmell, Bell et al., 2002): explicit attempt to “fulfill the Memex vision” — capture a life, then retrieve. Lifelogging. Storage-first.
- **Extended mind** (Clark & Chalmers, 1998): Otto’s notebook *is* part of his memory if it is trusted, coupled, and available. A vault that Anton actually queries meets the Otto test; a graveyard of notes does not.
- **Transactive memory** (Wegner): knowledge of *who knows what*. Wikilinks and people cards are a personal transactive index.
- **Zettelkasten:** atomic notes + links. A-MEM cites this for *agent* memory organization.
- **Building a Second Brain / PARA** (Tiago Forte): CODE — Capture, Organize, Distill, **Express**. The product is output (writing, projects), not prosthetic recall. In 2026 Forte is teaching “The AI Second Brain” — connectors + context for an agent that *acts*. That is the thinking layer.
- **Exocortex / memory prosthesis / personal knowledge graph / digital twin:** overlapping slogans. “Memory prosthesis” is the clinical/extended-mind term (DARPA/UCLA hippocampal prosthesis work is literal hardware; do not borrow the medical claim). “Digital twin” implies a model that *simulates* the person; Anton’s vault stores traces, it does not yet decide as he would (that is the clone/goal-1 layer).

**Defensible split:**

- **Second memory** = non-parametric long-term memory: ingest, store with provenance, associate (links), retrieve, invalidate. HippoRAG 2’s own title is *From RAG to Memory*. Graphiti/Zep/Mem0 all say **memory**, not brain.
- **Second brain** = the layer that *uses* memory to think: skills, routines, Alpha Protocol, cofounder voice, decisions. Forte’s BASB. Khoj still markets “Your AI second brain.”

**2025–2026 convergence:** research and infra → **agent memory / AI memory / context graph**. PKM/product → still **second brain**, now meaning “Obsidian + an agent.” Vectorize (2026-06-01) even defines a second brain as “a personal memory system,” which collapses the distinction. Anton should keep the split in public writing: it is the rare precise claim in a slogany market.

**Russian glossary**

- second memory — вторая память (протез воспоминания, не «думалка»)
- second brain — второй мозг (слой решений, скиллы, PARA/CODE)
- Memex — мемекс, «расширитель памяти»
- associative trail — ассоциативная тропа (викиссылка)
- bitemporal — двувременная запись (когда было правдой / когда система узнала)
- memory prosthesis — протез памяти
- extended mind — расширенный разум (блокнот Отто)
- transactive memory — трансactive / распределённая память («кто знает что»)
- exocortex — экзокортекс (маркетинг; не использовать как определение)

---

**Q6. What breaks in personal RAG, and a cheap nightly diagnostic**

| Failure | What you see | Cheap diagnostic (nightly, $0 API) |
|---|---|---|
| **Chunking splits meaning** | Answer has a heading without the rule; or a `link` with no target context | Script: % of chunks that start mid-sentence; % of chunks with a wikilink whose target file is not in the same chunk window. Metric: gold-span coverage = fraction of labeled answer spans wholly inside one retrieved chunk. Target ≥ 0.90. |
| **Stale index after edits** | Confident old policy / old decision | Join notes `mtime`/`hash` to index `indexed_at`/`hash`. Count `stale_chunks`. Fail the night if > 0 for files touched in 24h. |
| **Duplicate / near-dup notes** | Two answers, one superseded | Pairwise cosine on title+first-800 chars for new/changed files; log pairs > 0.92. Also hash-identical bodies. |
| **Link rot** | Graph hop to a missing note | Parse all `wikilinks`, resolve vs filesystem (and aliases). Count dangling. Already in the spirit of `/link-rot`. |
| **Mixed-language queries** | RU query misses EN note (or vice versa) | Hold out 30 RU and 30 EN gold queries; report Recall@12 **split by query lang and by note lang**. If RU→EN is a cliff, the embedder is the bug. |
| **Entity vs topic** | Graph hop explodes a person card | You already have `looks_like_entity` and `--ab`. Nightly: mean `promoted_via_graph` by query class. If entity class nDCG **drops** with graph, gate is wrong (too loose or too tight). |
| **Over-expansion** | Top-12 full of neighbours, gold buried | Cap already 40. Log: gold rank before vs after hop. If median gold rank worsens, cut neighbour cap or require reranker score > τ. |
| **Reranker over-trusts titles** | Title match outranks the paragraph that has the fact | Ablation: rerank with title stripped vs title kept (10% sample). If delta > 0.05 nDCG, down-weight titles or rerank on body-only. |
| **Missing content** | “I don’t have that” on a note that exists | Gold recall@60 (pre-rerank). If @60 is high and @12 is low → reranker. If @60 is low → embedder/chunking/index coverage. Barnett et al. 2024 seven failure points: missing content vs retrieved-but-not-ranked. |
| **Contradiction blend** | Model mixes old and new | Among top-12, count distinct `valid_from` eras for the same entity. If ≥2, the prompt must show dates or the filter must drop closed facts. |

Barnett et al., “Seven Failure Points When Engineering a Retrieval Augmented Generation System” (2024) is the academic checklist: missing content, missed ranking, consolidation drop, incomplete answers, wrong format, incorrect specificity, incomplete. Personal-vault extras are **staleness, dups, link rot, language mix, graph over-expansion** — exactly Anton’s known weak spots.

---

**Do this in the next 2 weeks (max 10 steps, by expected recall gain per hour)**

1. **Build the 200-query private gold set** from real notes (wikilink edges + temporal templates + hard-negative dups). 1–2 days. Unlocks every other measurement. Without this, every model swap is vibes.
2. **Instrument `brain_ask.py --ab` to emit Recall@5/12/60, MRR, nDCG@12 per query class** into `ab_recall`. Half day. Makes the existing telemetry a benchmark.
3. **Reindex with BGE-M3 and with Qwen3-Embedding-0.6B; keep e5-base as control.** Run the gold set. Pick a winner. Half day compute + 2h glue. Largest single-model jump available on this hardware.
4. **Swap reranker to bge-reranker-v2-m3; A/B Qwen3-Reranker-0.6B on the same 60-candidate pools.** Do not change k yet. 2–4h. Second-largest jump (literature: +15–25% answer correctness is the *marketing* range; believe only the gold-set delta).
5. **Add FTS5 (or BGE-M3 sparse) and fuse with dense** the way Mem0 does (semantic + keyword + entity boost). 6–10h. Helps exact names, RU morphology, and quotes.
6. **Freshness: store path, hash, mtime in the index; nightly re-embed only changed files; fail if stale_chunks > 0.** 4–8h. Stops a class of silent wrong answers. sqlite-vec makes this natural — do it in the same pass if possible.
7. **Markdown-aware chunking + late chunking trial** on long notes (embed whole note with 8k ctx, pool to heading windows). 4–8h. Fixes split-meaning. Jina late-chunking paper: nDCG lifts of a few points, larger on long docs.
8. **Near-dup report + `superseded_by` frontmatter** (no deletion). 3–6h. Feeds test 3 (update/contradiction).
9. **HippoRAG-style PPR on the wikilink graph**, seeded by top-15 vector hits, still reranked. Keep the entity gate. 8–16h. This is the associative-memory upgrade that does not require extracting a new KG.
10. **Implement bitemporal columns** (`valid_from`/`valid_to`/`superseded_by`) in SQL + default retrieval filter to open facts; `as_of` flag for “what did I believe then.” 6–10h. Matches Graphiti and the existing design note. Do this before any LLM “forgetting.”

Stop after step 4 if the gold set is not stable. Do not start LightRAG/Graphiti/Neo4j in this window.

---

**Coverage / uncertainty (inside the report, not a follow-up)**

- GitHub star widgets failed on several repo pages this run (Mem0, Graphiti, HippoRAG, Cognee). Stars for those are from org listings, vendor pages, or README headers opened the same day — **verify on github.com before citing in a paper**.
- HippoRAG license SPDX and last-commit date: **not found** on the loaded page.
- LanceDB exact star count: **not found** this run.
- nomic-embed and arctic RU retrieval: **not found** this run.
- Mem0 92.5 / Graphiti 94.7 LoCoMo: **vendor self-report**, different harnesses; independent matrices disagree. Marked as such.
- Latency figures for rerankers are from third-party GPU tables (L40S), not an M-series Mac. Expect 3–10× slower on CPU/MPS; measure on Mac16 before promising UX.
- Jina embedding license (CC-BY-NC) may not match a public MIT repo — confirm before shipping weights.
- I did not re-crawl every candidate’s LICENSE file byte-for-byte; Apache/MIT/AGPL labels above are from the GitHub license badge or LICENSE heading on the opened page.

**Sources (full URLs, opened this run)**

1. https://github.com/tonydzi/sqlite-graph-memory — 2026-09-16 load; architecture, models, v0.2 eval plan
2. https://github.com/mem0ai/mem0 — Apr 2026 algorithm, LoCoMo/LongMemEval table, Apache-2.0
3. https://github.com/orgs/mem0ai/repositories — stars ~65k, updated 2026-09-15
4. https://mem0.ai/blog/state-of-ai-agent-memory-2026 — updated 2026-09-03
5. https://github.com/getzep/graphiti — commit 2026-09-11, Apache-2.0, temporal KG
6. https://arxiv.org/abs/2501.13956 — Zep paper, 2025-01-20
7. https://help.getzep.com/graphiti/getting-started/overview — LoCoMo 94.7% / LongMemEval 90.2% claims
8. https://www.getzep.com/ai-agents/temporal-knowledge-graph/ — four timestamps, 2026-05-31
9. https://github.com/OSU-NLP-Group/HippoRAG — HippoRAG 2
10. https://arxiv.org/abs/2502.14802 — HippoRAG 2 paper (ICML 2025)
11. https://github.com/HKUDS/LightRAG — 39.7k, commit 2026-09-15
12. https://github.com/microsoft/graphrag — maintenance mode, commit 2026-08-24
13. https://github.com/topoteretes/cognee — remember/recall/forget
14. https://github.com/basicmachines-co/basic-memory — 4.0k, AGPL-3.0
15. https://github.com/asg017/sqlite-vec — 8.1k, Apache-2.0/MIT
16. https://github.com/agiresearch/A-mem — 1.2k, MIT, arXiv:2502.12110
17. https://github.com/letta-ai/letta — 24.8k; https://docs.letta.com/guides/agents/architectures/memgpt/
18. https://www.letta.com/blog/context-repositories — sleep-time memory, 2026-02-12
19. https://github.com/langchain-ai/langmem — 1.7k, MIT, push 2026-09-09
20. https://github.com/khoj-ai/khoj — 37.4k, AGPL, commit 2026-08-02
21. https://qwenlm.github.io/blog/qwen3-embedding/ — 2025-06-05, MMTEB 70.58
22. https://github.com/QwenLM/Qwen3-Embedding — tables vs BGE-M3 / e5 / jina
23. https://arxiv.org/pdf/2504.12879 — rus-MIRACL: e5-base 61.41, e5-large 66.99, BGE-M3 70.50, +rerank 76.44
24. https://arxiv.org/html/2509.25085 — jina-reranker-v3; MIRACL 66.5–66.8; RU 65.20; bge-reranker-v2-m3 MIRACL 69.32
25. https://jina.ai/news/late-chunking-in-long-context-embedding-models — 2024-08-22
26. https://mnemoverse.com/docs/research/evaluation/ai-memory-benchmarks-field-guide — 2026-06-23 / updated 2026-09-03
27. https://www.cognee.ai/ai-memory-benchmarks — 2026-09-01, LoCoMo/LongMemEval/BEAM/PersonaMem
28. https://arxiv.org/html/2605.12493v1 — LongMemEval-V2, 2026-05
29. https://github.com/xiaowu0162/LongMemEval
30. https://github.com/snap-research/locomo
31. https://arxiv.org/html/2510.13975v1 — RAG error taxonomy (chunking/retrieval/rerank/generation)
32. https://www.roguedigital.ai/insights/why-first-rag-pipeline-fails — 2026-05-07, eight RAG failure modes
33. https://vectorize.io/articles/brain-stack-second-company-single-brain — 2026-06-01, “second brain” as personal memory
34. https://eric.clst.org/assets/wiki/uploads/EFC/vbush.pdf — Bush, As We May Think, 1945, Memex
35. https://www.researchgate.net/publication/2561462_MyLifeBits_Fulfilling_the_Memex_Vision — Gemmell/Bell 2002
36. https://github.com/chroma-core/chroma — 29.3k, Apache-2.0, 2026-09-15
37. https://github.com/infiniflow/ragflow — 90.8k
38. https://github.com/VectifyAI/PageIndex — 35.7k, 2026-09-15
39. https://github.com/neuml/txtai — ~12.9k
40. https://arxiv.org/html/2402.03216v5 — BGE-M3 MIRACL
41. https://docs.letta.com/concepts/memory-management/ — core / archival / recall

---

**Skill used:** none of the listed slash-skills was a Deep Research runner; this pass used live web search + page opens against the DR26-09-16 brief. Verdict on a new skill: **📝 not now** — the bottleneck is Anton’s gold set and model swap, not another wrapper.

**ELI5:** You already have a box that stores your notes and a string between related notes. Other labs built fancier boxes. Do not buy the fancy box. First write 200 questions you actually care about, then swap the “smell-alike” engine for a newer multilingual one, then mark old facts as ended instead of deleting them. Call the box **вторая память**. The **второй мозг** is the part that decides.
