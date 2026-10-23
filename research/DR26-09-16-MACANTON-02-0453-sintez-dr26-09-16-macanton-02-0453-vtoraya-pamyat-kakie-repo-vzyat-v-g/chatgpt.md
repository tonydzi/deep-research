# DR26-09-16-MACANTON-02-0453 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Executive conclusion

The fastest route to better recall is not replacing SQLite or materializing a new knowledge graph. It is:

1. Build a frozen, private gold set before changing retrieval.
2. Eliminate stale and duplicate chunks.
3. Add BM25/FTS5 and reciprocal-rank fusion beside dense retrieval.
4. Benchmark stronger multilingual embeddings on Anton’s own RU+EN questions.
5. Add temporal/provenance fields and conservative supersession, while keeping Markdown authoritative.
6. Test graph expansion by query class and depth; retain the existing entity gate unless measured results justify broader expansion.

The strongest ideas to borrow are HippoRAG’s Personalized PageRank retrieval, Graphiti’s temporal/provenance model, Basic Memory’s Markdown/SQLite synchronization, LangMem’s background consolidation pattern, and BGE-M3’s dense+sparse retrieval. Full-framework migrations would add operational complexity without demonstrated benefit.

“Second memory” is a defensible name. A precise label is “personal second-memory system”: a durable, provenance-preserving external memory that captures personal records and retrieves them through semantic, lexical, relational, and temporal associations. It becomes a “second brain” only when it also performs planning, synthesis, judgment, and decision support.

Repository and popularity data below were checked on 2026-09-16. GitHub rounds some displayed star counts; “not found” means the opened source did not expose the value reliably.

B. Q1 — Ranked repositories worth borrowing from

| Rank | Repository | Last activity / stars / license | What improves on sqlite-graph-memory | Concrete item to borrow | Cost and dependencies | Verdict |
|---|---|---|---|---|---|---|
| 1 | HippoRAG 2 | Exact last commit and stars not found; MIT | Multi-hop retrieval using entities, facts, linking and Personalized PageRank; includes retrieval and QA evaluation code | `src/hipporag/evaluation/retrieval_eval.py`, entity linking, and PPR-style score propagation | 24–50 h for a SQLite/networkx experiment; local embedding/LLM possible; no mandatory graph service | Adopt the evaluator; borrow PPR retrieval |
| 2 | Graphiti | 2026-09-11; 30.9k; Apache-2.0 | Explicit episodic provenance, evolving facts, temporal edges and hybrid graph search | Episode→entity/edge extraction and `valid_at`/`invalid_at` semantics | 30–60 h; normally Neo4j, FalkorDB or Neptune plus an extraction LLM | Borrow temporal schema; skip DB migration |
| 3 | Basic Memory | Active in 2026; exact commit date not exposed; stars not found; Apache-2.0 | Closest architectural match: local-first Markdown, SQLite/Postgres, filesystem synchronization, hybrid FTS+vector retrieval | Change synchronization, content identity, project isolation, and hybrid score fusion | 16–32 h; Python, SQLite, FastEmbed; no graph service required | Adopt selected sync/index patterns |
| 4 | BGE-M3 / FlagEmbedding | Exact repo activity/stars not found; MIT | One model produces dense, sparse and multi-vector representations across 100+ languages | Dense+sparse candidate generation and weighted/RRF fusion | 8–20 h plus complete re-embed; about 2.2 GB FP32 or 1.1 GB FP16 weights | Adopt as first challenger |
| 5 | HippoRAG-compatible LightRAG | 2026-09-14; 39.6k; MIT | Dual-level entity/relation and chunk retrieval; incremental updates; local/global query modes | Local/global query classification and dual retrieval, not its full storage layer | 20–50 h; extraction LLM; optional external graph/vector stores | Borrow idea |
| 6 | LangMem | Exact commit date not exposed; 1.7k; MIT | Separates in-conversation memory actions from background extraction and consolidation | Background memory manager’s create/update/delete decision loop | 12–24 h; LLM required for consolidation; LangGraph is optional if ported | Borrow idea |
| 7 | Mem0 | 2026-09-15; 65.4k; Apache-2.0 | Explicit memory CRUD, metadata filters, dedup/update decisions and memory evaluation suite | Add/update/delete decision taxonomy and `memory-benchmarks` harness | 16–32 h; configurable vector DB and LLM; avoid hosted defaults | Borrow idea |
| 8 | Microsoft GraphRAG | Exact current activity/stars not found; MIT | Community detection and hierarchical summaries answer corpus-wide questions that nearest-chunk search misses | Community reports as an offline, separately scored retrieval channel | 40–100 h; expensive graph extraction and summarization | Pilot only for “global” questions |
| 9 | Cognee | Active 2026; exact date/stars not exposed; Apache-2.0 | Typed graph extraction, configurable search modes, feedback and pruning | `DataPoint` schemas and `SearchType`-style explicit retrieval modes | 30–70 h; graph/vector stores and extraction LLM | Borrow typed extraction/eval patterns |
| 10 | txtai | 2026-08-19; 12.9k; Apache-2.0 | Integrates relational SQL, dense/sparse vectors and graphs behind one query layer | SQL-filtered hybrid search and graph-aware embeddings interface | 16–40 h; Python and ANN backend; can remain embedded/local | Borrow idea |
| 11 | nano-graphrag | Last date not found; 3.9k; MIT | Small, understandable GraphRAG implementation; content hashes prevent exact duplicate insertion | Content-hash identity and `only_need_context` retrieval interface | 8–20 h; networkx by default, optional Neo4j; LLM extraction | Borrow code selectively |
| 12 | Letta | Organization active 2026-09-15; core repo stars/date not reliably found; Apache-2.0 | Clear working/core versus archival memory separation and explicit agent-managed memory | Core-memory size budget plus archival-memory search boundary | 12–30 h concept port; full server migration unnecessary | Borrow policy, not platform |
| 13 | sqlite-vec | Current date/stars not found; MIT/Apache-2.0 dual licensing reported by project, but verify before redistribution | Stores vectors inside SQLite, simplifying atomic deletion, metadata joins and index-version checks | Virtual vector table keyed by stable `chunk_id` and content hash | 8–16 h; SQLite extension; benchmark ANN speed on 100k notes | Adopt if Mac/Windows packaging passes |
| 14 | ColBERT / PLAID | Current date/stars not found; MIT | Late interaction preserves token-level matches that single-vector chunk embeddings lose | Add a ColBERT reranking experiment over the top 60, initially offline | 20–50 h; larger index and GPU strongly preferred | Pilot after simpler upgrades |
| 15 | Khoj / Obsidian Smart Connections / Copilot | Exact comparable metadata not found | Good UX and vault ingestion references, but weaker fit for a measured SQLite retrieval core | Inspect filesystem watching, citation UX and incremental reindex behavior | 8–20 h inspection; adopting full apps adds services/UI | Skip as core; borrow UX only |

Primary repository evidence:

- HippoRAG documents its entity/fact extraction, linking, reranking and evaluation modules: https://github.com/OSU-NLP-Group/HippoRAG
- Graphiti was active on 2026-09-11 and lists Apache-2.0, 30,908 stars, and graph backends: https://github.com/getzep and https://github.com/getzep/graphiti/blob/main/examples/quickstart/README.md
- Basic Memory describes its local-first architecture and 2026 hybrid vector/FTS work: https://github.com/basicmachines-co/basic-memory/blob/main/docs/ARCHITECTURE.md and https://github.com/basicmachines-co/basic-memory/blob/main/CHANGELOG.md
- LightRAG’s README describes its dual-layer graph/vector design and incremental architecture: https://github.com/HKUDS/LightRAG
- LangMem exposes hot-path tools and a background memory manager: https://github.com/langchain-ai/langmem
- Mem0’s repository was updated on 2026-09-15 and its open-source engine supports configurable embedders, stores and rerankers: https://github.com/mem0ai and https://github.com/mem0ai/mem0/blob/main/docs/open-source/overview.mdx
- nano-graphrag documents content-hash deduplication, local/global retrieval and its small storage interfaces: https://github.com/gusye1234/nano-graphrag
- txtai combines relational, graph, sparse and dense retrieval and was updated in August 2026: https://github.com/neuml
- Cognee documents add→cognify→search, typed graph construction, feedback and pruning: https://github.com/topoteretes/cognee and https://github.com/topoteretes/cognee/blob/main/cognee/skill.md
- Letta’s maintained memory documentation distinguishes core, archival and conversation memory: https://github.com/letta-ai/skills/blob/main/letta/agent-development/references/memory-architecture.md
- Qwen’s official embedding repository provides a useful modern implementation and comparison table: https://github.com/QwenLM/Qwen3-Embedding/blob/main/README.md

Why several named candidates are not ranked: A-MEM, MemoRAG, memary, Memori, supermemory and mem-agent contain useful research or product ideas, but the opened evidence did not establish a better component-to-effort ratio than the top 15. Memary, for example, has a timestamped “memory stream,” but some advertised rewind functionality remains prospective; its repository showed 2.6k stars and MIT licensing: https://github.com/kingjulio8238/memary. Chroma, LanceDB, RAGFlow, R2R, Verba and PageIndex solve broader storage/application problems; replacing the working SQLite/Markdown core with them would be migration, not a targeted recall improvement.

C. Q2 — Evaluation and a private zero-API-cost benchmark

State of the art

Long-term agent-memory benchmarks increasingly separate extraction, temporal reasoning, updates, multi-session reasoning, abstention and downstream use:

- LoCoMo contains ten very long conversations annotated for QA and event summarization. It is useful for episodic and temporal recall, but it is small and conversational rather than a personal document vault: https://github.com/snap-research/locomo and https://arxiv.org/abs/2402.17753
- LongMemEval has 500 curated questions covering information extraction, multi-session reasoning, temporal reasoning, knowledge updates and abstention. Its paper reports roughly a 30-point degradation for systems operating over long histories: https://arxiv.org/abs/2410.10813 and https://openreview.net/pdf?id=pZiyCaVuti
- LongMemEval-V2, published May 2026, adds 451 manually curated questions about static state, dynamic state, workflows, environment “gotchas” and premise awareness: https://arxiv.org/abs/2605.12493
- LOCOMO-CONV, published September 2026, adds dialog, implicit, counterfactual and composed query styles; it reports that conversational framing reveals retrieval failures hidden by direct QA: https://arxiv.org/abs/2609.03467
- MemoryAgentBench, ICLR 2026, broadens evaluation across agent memory mechanisms rather than only user-history recall: https://openreview.net/pdf?id=DT7JyQC3MR
- PersonaMem, MSC and related persona-dialog datasets are useful for preference consistency, but are weak proxies for 100k-note retrieval because their ground truth is dialogue/persona state rather than document provenance.
- TREC iKAT is especially relevant: it evaluates conversational retrieval conditioned on a Personal Textual Knowledge Base and requires both retrieved passages and PTKB statements. The 2026 track emphasizes robustness across user behaviors: https://www.trecikat.com/guidelines/ and https://www.trecikat.com/
- BEIR-style evaluation remains the correct deterministic base: query, corpus and relevance judgments scored with Recall@k, MRR and nDCG. “Needle” tests are diagnostic stress tests for depth, distractors and exact details, not a sufficient production benchmark.

Framework positioning:

- RAGAS supplies context precision/recall, noise sensitivity, faithfulness and factual-correctness metrics. Many require an LLM judge: https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/ and https://github.com/vibrantlabsai/ragas/blob/main/docs/concepts/metrics/available_metrics/faithfulness.md
- DeepEval offers retrieval metrics and custom G-Eval judges, but its documentation warns that G-Eval is nondeterministic: https://deepeval.com/docs/metrics-llm-evals and https://deepeval.com/tutorials/rag-qa-agent/evaluation
- ARES trains lightweight judges using synthetic data plus a small human-labelled set and evaluates context relevance, faithfulness and answer relevance: https://arxiv.org/abs/2311.09476 and https://github.com/stanford-futuredata/ARES
- TruLens, Giskard and Phoenix/Arize are most valuable for tracing, regression dashboards and production error slices. They do not replace a vault-specific relevance set.

Private eval-set recipe

1. Freeze a vault snapshot and index version. Every result records snapshot hash, model, dimensions, chunker version, graph mode, reranker and random seed.
2. Sample 300 source notes stratified by language, note age, length, folder, link degree and duplicate cluster.
3. Create 600 questions locally:

   - 200 single-note factual questions.
   - 100 RU query→EN evidence and 100 EN query→RU evidence questions.
   - 75 two-hop wikilink questions.
   - 75 temporal/bitemporal questions.
   - 25 superseded-fact questions.
   - 25 unanswerable questions.

4. Generate drafts using a local model, but accept a question only when the answer is an exact span or structured fact recoverable from cited note IDs. Human-review the first 100 and a random 10% thereafter.
5. Ground-truth fields: `query`, `language`, `question_type`, `gold_note_ids`, optional `gold_chunk_spans`, `gold_edges`, `answer`, `valid_time`, `transaction_time`, `must_abstain`.
6. Use wikilinks as weak graph relevance, not unquestioned truth. A linked neighbour is relevant only if it contains evidence needed for the answer.
7. Generate hard negatives from same-title variants, near-duplicate clusters, the same entity at the wrong date, and semantically similar notes without the answer.
8. Run `brain_ask.py --ab` on every question and persist ranked chunk IDs and scores for both vector-only and vector+graph.
9. Score retrieval deterministically:

   - Recall@k = fraction of queries with at least one gold item in top k.
   - MRR@k = mean reciprocal rank of the first gold item.
   - nDCG@k = graded ranking quality when primary evidence, supporting evidence and graph neighbours have different relevance.
   - Graph edge recall = fraction of gold wikilink paths represented in expanded candidates.
   - Duplicate burden = repeated-source or near-duplicate chunks divided by k.

10. Score answers separately. Exact match/F1 or normalized field comparison handles names, dates and lists. A local judge can estimate correctness and faithfulness, but calibrate it against 100 human labels and report judge agreement. RAGAS defines faithfulness as supported answer claims divided by all answer claims: https://github.com/vibrantlabsai/ragas/blob/main/docs/concepts/metrics/available_metrics/faithfulness.md

First five tests and provisional “good” thresholds

These are engineering acceptance targets, not universal scientific standards:

| Test | Initial target |
|---|---|
| Single-note retrieval | Recall@12 ≥ 0.90; MRR@12 ≥ 0.70 |
| Cross-language RU↔EN | Recall@12 ≥ 0.85, with ≤5 percentage-point gap between directions |
| Two-hop wikilink retrieval | Gold-path Recall@40 ≥ 0.75 and nDCG@12 ≥ 0.60 |
| Temporal/superseded facts | ≥0.85 correct current-or-historical answer; stale-fact error ≤0.05 |
| Unanswerable/contradiction | Abstention F1 ≥0.85; answer faithfulness ≥0.95 |

Also require graph mode to improve the appropriate entity/multi-hop slices by at least three absolute nDCG@12 points without reducing the full-set score by more than one point. Otherwise the graph expansion should remain gated.

D. Q3 — Cheap local model upgrades

Published benchmark aggregates are not directly comparable when task sets, model instructions or dates differ. Russian-only retrieval numbers were not consistently exposed by the opened primary model cards, so they are marked “not found”; Anton’s private RU+EN benchmark must decide the winner. MMTEB covers 500+ tasks across 250+ languages, while ruMTEB specifically includes Russian MIRACL, RuBQ and RiaNews retrieval tasks: https://arxiv.org/abs/2502.13595 and https://aclanthology.org/anthology-files/anthology-files/pdf/naacl/2025.naacl-long.12.pdf

Approximate memory means weights only, excluding activations and framework overhead. Latencies are estimates, because no source provided a controlled M-series/consumer-GPU comparison for all models; measure them on Anton’s hardware.

| Model | Params / context | Published evidence | Weight memory | Estimated local speed | Recommendation |
|---|---:|---|---:|---|---|
| `multilingual-e5-base` | 278M / ~512 tokens | Existing baseline; Russian-specific score not found | ~1.1 GB FP32 / 0.55 GB FP16 | Fast; 60 chunks usually sub-second on GPU, low seconds on Mac CPU | Keep as control |
| `multilingual-e5-large` / instruct | 560M / ~512 tokens | MMTEB identified the instruct variant as the best public model in its evaluation; Qwen comparison lists multilingual aggregate 63.22 | ~2.2 / 1.1 GB | Roughly 2× base | Strong low-risk challenger |
| BGE-M3 | 568M / 8,192 | Dense+sparse+multi-vector and strong Russian retrieval in ruMTEB; exact Russian score not found | ~2.3 / 1.15 GB | Similar to E5-large for dense; sparse adds work | Best first experiment |
| jina-embeddings-v3 | 570M / 8,192 | 89 languages; published work reports stronger multilingual aggregate than multilingual-e5-large-instruct | ~2.3 / 1.15 GB | Similar to BGE-M3 | Test, especially for long chunks |
| Qwen3-Embedding-0.6B | 0.6B / 32K | Official table reports 64.34 multilingual aggregate; Russian supported | ~2.4 / 1.2 GB; lower quantized | Similar class, instruction-sensitive | Test after BGE-M3 |
| Qwen3-Embedding-4B | 4B / 32K | Official table reports substantially stronger multilingual results than 0.6B; Russian supported | ~8 GB FP16, ~2.5–4.5 GB quantized | Practical on NVIDIA; slow on Mac CPU | GPU ceiling experiment |
| Qwen3-Embedding-8B | 8B / 32K | Official README reports 70.58 multilingual MTEB as of 2025-06-05 | ~16 GB FP16, ~5–9 GB quantized | Desktop GPU only for routine use | Benchmark ceiling, not default |
| nomic-embed-text v1.5/v2 | Size/version dependent | Primarily English-oriented evidence found; comparable Russian number not found | Not consistently found | Fast for smaller variants | Skip for RU+EN default |
| snowflake-arctic-embed family | 22M–568M, model dependent | Strong English retrieval; comparable Russian result not found | ~0.1–2.3 GB | Small variants very fast | Skip unless latency dominates |
| `bge-reranker-v2-m3` | 568M | Qwen’s comparison table reports multilingual reranking aggregate 57.03; supports multilingual input | ~2.3 / 1.15 GB | Estimated 0.3–1.5 s/60 on consumer GPU; several seconds CPU | Recommended default reranker |
| `jina-reranker-v2` | ~278M, variant dependent | Multilingual and long-context; exact comparable Russian score not found | ~0.6–1.1 GB FP16 | Likely faster than 568M cross-encoders | Benchmark for Mac default |
| `Qwen3-Reranker-0.6B` | 0.6B | Official family supports 100+ languages and strong multilingual reranking | ~1.2 GB FP16 | Similar to BGE reranker | Test against BGE |
| `Qwen3-Reranker-4B/8B` | 4B/8B | Higher published aggregate quality; exact Russian score not found | ~8/16 GB FP16 | Desktop GPU; expensive per 60 | Use only if measured gain justifies |
| ColBERT/PLAID | Encoder dependent | Token-level late interaction improves exact/local matching but greatly enlarges the index | Model plus multi-vector index, often several× dense index | Fast retrieval after expensive indexing; GPU preferred | Later-stage experiment |

Primary model sources:

- Qwen3 official model family, languages and aggregate scores: https://github.com/QwenLM/Qwen3-Embedding/blob/main/README.md
- Qwen3 paper: https://arxiv.org/abs/2506.05176
- Jina v3 model size, languages and context: https://jina.ai/en-US/models/jina-embeddings-v3/ and https://arxiv.org/abs/2409.10173
- Russian-focused ruMTEB results: https://aclanthology.org/anthology-files/anthology-files/pdf/naacl/2025.naacl-long.12.pdf
- BGE’s official MIRACL evaluation instructions: https://bge-model.com/API/evaluation/miracl.html
- Current MTEB model results should be read per task rather than through a single aggregate: https://leaderboard.mteb.org/models/Qwen/Qwen3-Embedding-8B

Recommendation: benchmark BGE-M3, multilingual-e5-large-instruct, jina-v3 and Qwen3-Embedding-0.6B using identical chunks. Test BGE-M3 dense-only and dense+sparse. Pair the top two embedders with `bge-reranker-v2-m3` and Qwen3-Reranker-0.6B. Keep top-60→top-12 initially. Do not select from MTEB alone.

E. Q4 — Consolidation, forgetting and bitemporal memory

What works operationally

1. Immutable source, mutable derived memory. Markdown remains the human-editable source. Chunks, embeddings, relations, summaries and “current fact” views are rebuildable projections.
2. Stable identity. Each indexed unit needs `note_id`, normalized path, heading/block identity, content hash, source modification time, index generation and embedding-model version.
3. Supersession instead of destructive replacement. A new claim closes the valid interval of the old claim or marks it disputed; it does not erase provenance.
4. Decay ranking, not source deletion. Recency may reduce a memory’s retrieval prior, but explicit links, repeated successful retrieval, user pinning and legal/historical importance should resist decay.
5. Consolidation creates a derived summary linked to every source. It should not merge or rewrite human notes automatically.

Graphiti’s strongest fit is the model, not the infrastructure: facts arrive in timestamped episodes, retain provenance and acquire temporal validity. Graphiti’s normal quickstart requires Neo4j, FalkorDB or Neptune, so copying its semantics into SQLite is cheaper than adopting its service stack: https://github.com/getzep/graphiti/blob/main/examples/quickstart/README.md

Mem0 offers explicit add/search/update/delete operations. Its update API overwrites the stored value and adjusts indexes, which is convenient but insufficient for historical personal memory unless Anton separately records the previous version and validity interval: https://docs.mem0.ai/core-concepts/memory-operations/update

Letta’s split is useful for retrieval budgets: core memory is small, always available identity/current-state information; archival memory is large and searched on demand. That maps to “current profile and active commitments” versus the full vault: https://github.com/letta-ai/skills/blob/main/letta/agent-development/references/memory-architecture.md

Recommended SQLite fact schema:

`fact_id, subject, predicate, object, valid_from, valid_to, observed_at, indexed_at, source_note_id, source_span_hash, extraction_method, confidence, status, superseded_by, contradiction_group`

Here, `valid_*` represents when the fact was true in Anton’s world; `observed_at/indexed_at` represents when the system learned it. A contradiction is not resolved merely because one statement is newer: classify it as update, alternative perspective, extraction error or genuine dispute. Current-value queries may prefer open intervals and authoritative sources; historical queries must filter by valid time.

Forgetting should be reversible:

`retrieval_prior = base × recency_decay × access_boost × link_boost × authority × pin`

Apply this prior only after lexical/dense relevance has produced candidates. Never decay the only evidence for a unique autobiographical event. Ebbinghaus-style exponential decay and spaced-repetition boosts are useful scheduling metaphors, but they are not evidence that a fact became false. “Sleep-like consolidation” is best implemented as a nightly dry run that proposes clusters/summaries and reports affected sources; only derived records are created automatically.

F. Q5 — Terminology

Historical line

- Bush’s 1945 Memex was an “enlarged intimate supplement” to personal memory, storing records and supporting associative trails. That is extremely close to Anton’s vault plus wikilinks and associative retrieval: https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/
- MyLifeBits attempted lifetime capture around Gordon Bell and developed software for searching and managing that personal record. It belongs to lifelogging/personal-memory research rather than general knowledge management: https://en.wikipedia.org/wiki/MyLifeBits
- Clark and Chalmers’ “extended mind” is a philosophical claim: reliably coupled external resources may participate in cognition. It is broader than a software architecture.
- Wegner’s transactive memory concerns knowing who or what stores knowledge and how a group retrieves it. A personal agent ecosystem can be a human-machine transactive-memory system.
- Zettelkasten emphasizes atomic notes and authored links for thought development.
- Tiago Forte’s “second brain” and PARA are productivity/organization metaphors spanning capture, organization and creative execution.
- “Personal knowledge graph” accurately describes structured entities and relations but omits raw episodic records, embeddings and generative recall.
- “Exocortex” implies externalized cognitive processing, not merely memory.
- “Memory prosthesis” is clinically suggestive and appropriate when compensating for genuine impairment, but it may imply a regulated medical claim if used as a product promise.
- “Digital twin” is too broad: it suggests a predictive model of the person, not a provenance-preserving memory store.

Defensible definitions

Second memory: “A persistent external system that captures a person’s records and facts, preserves their provenance and temporal validity, and retrieves them through lexical, semantic, relational and temporal associations to support human or agent recall.”

Second brain: “A cognitive support system that uses memory plus active synthesis, planning, simulation, judgment and decision assistance.”

Thus sqlite-graph-memory plus the vault is already a second memory. Claude/Codex operating over it can form part of a second-brain layer, but the stored corpus and retrieval engine should not themselves be called a brain.

In 2025–2026 research, “agent memory,” “long-term memory” and “memory layer” are the dominant technical terms: Mem0 calls itself a memory layer, Cognee an AI memory platform, Letta discusses memory systems, and LongMemEval evaluates long-term memory. “Second brain” remains stronger in consumer productivity; “personal knowledge graph” remains common in structured-data products. “Second memory” is not yet the dominant market label, but it is the more precise product category for Anton’s system. Evidence: https://github.com/mem0ai/mem0, https://github.com/topoteretes/cognee, https://docs.letta.com/, and https://arxiv.org/abs/2410.10813

Short Russian glossary:

- second memory — вторая память
- second brain — второй мозг
- memory prosthesis — протез памяти
- extended mind — расширенный разум
- transactive memory — транзактивная/распределённая память
- personal knowledge graph — персональный граф знаний
- provenance — происхождение и цепочка источников
- bitemporal memory — бивременная память: когда факт был верен и когда система о нём узнала

G. Q6 — Common personal-RAG failures and nightly diagnostics

| Failure | Cheap nightly diagnostic |
|---|---|
| Chunking splits decisions, lists or heading context | Sample boundary windows; count chunks beginning with pronouns/list continuations or ending mid-sentence; run 50 gold queries at two chunk sizes |
| Stale index after edits | Compare note content hash and mtime with indexed hash/generation; fail if any live note lacks a matching current chunk set |
| Deleted notes remain retrievable | Left join indexed `note_id/path` against current filesystem manifest; require zero orphans |
| Exact duplicates | Group normalized-content SHA-256; report duplicate chunks and wasted top-k slots |
| Near duplicates | Local MinHash/SimHash or embedding clustering; track clusters and duplicate burden in retrieved top-12 |
| Broken or ambiguous wikilinks | Parse links and aliases; count unresolved targets, multiply resolved targets and self-links |
| RU/EN asymmetry | Fixed paired bilingual queries; report Recall@12 and MRR separately for RU→RU, EN→EN, RU→EN and EN→RU |
| Entity versus topic query mismatch | Rule-based slice plus gold labels; compare graph delta by class and watch false entity-gate activations |
| Graph over-expansion | Log candidate count by hop, proportion with no gold path, latency and nDCG delta; cap fan-out/hops when noise rises |
| Hub domination | Report retrieval frequency versus graph degree; test whether generic MOC/index notes crowd out substantive evidence |
| Reranker trusts titles | Run paired candidates with misleading/neutralized titles; compare rank flips and body-evidence accuracy |
| Embedding-model/index mismatch | Persist model ID, dimension and normalization in index metadata; refuse queries when runtime configuration differs |
| Temporal confusion | Nightly suite containing “before,” “after,” “at the time” and “current” questions; score stale-fact error |
| Contradictory memories collapse | Query each contradiction group for both current and historical formulations; require citations to every competing source |
| Retrieval looks good but answers hallucinate | Separate retrieval Recall@k from claim-level faithfulness; never let answer quality hide a missed gold source |
| Graph helps averages but hurts ordinary queries | Paired bootstrap of vector-only versus graph runs, overall and by query type; retain the entity gate unless confidence interval is positive |
| High latency or memory regression | Store p50/p95 candidate-generation, graph-expansion and rerank time plus peak RSS for the fixed gold set |
| Quiet ingestion failure | Manifest counts: notes, chunks, unique hashes, embeddings, links and failures; compare with the last successful generation |

H. Two-week plan, ordered by expected recall gain per hour

1. Freeze a 200-question hand-checked seed set: 80 factual, 40 bilingual, 30 temporal, 30 graph/multi-hop and 20 unanswerable.
2. Extend `--ab` logging with query ID, gold note/chunk IDs, ranked candidates, MRR, Recall@12/60 and nDCG@12.
3. Add content hashes and an atomic generation ID; produce stale, missing, orphan and duplicate-chunk counts before every benchmark.
4. Add SQLite FTS5/BM25 candidate retrieval and RRF with dense results; tune only on a development split.
5. Benchmark current e5-base against multilingual-e5-large-instruct, BGE-M3, jina-v3 and Qwen3-Embedding-0.6B on the frozen set.
6. Benchmark BGE and Qwen 0.6B rerankers over the same top-60 candidates; record quality, latency and memory.
7. Add graph diagnostics: expansion size, gold-edge/path recall, hub degree and score delta by entity/topic/temporal query class.
8. Introduce bitemporal/provenance columns for extracted facts and a non-destructive supersession view; do not rewrite Markdown.
9. Create a nightly deterministic regression run with fixed snapshot/model versions and thresholds; LLM judging is a separate optional pass.
10. Expand to 600 questions only after the first measurement identifies the weakest slices; spend annotation effort where uncertainty is highest.

Expected decision after two weeks: select one embedding/reranker pair, one hybrid-fusion configuration, and one graph policy using held-out nDCG/Recall rather than visual judgment. Full Graphiti, LightRAG or GraphRAG adoption should require a statistically credible gain over this strengthened baseline.

I. Limits and uncertainties

- Current stars and latest-commit dates were unavailable for several repositories because their opened GitHub pages did not expose those fields reliably. They are explicitly marked “not found,” not estimated.
- No trustworthy, directly comparable Russian score and 60-candidate latency table was found for every requested model. Published aggregate numbers use different task collections; local latency estimates are planning ranges, not measurements.
- The proposed quality thresholds are acceptance goals for this vault, not claims of universal industry standards.
- Wikipedia was used only for historical orientation where a more accessible primary MyLifeBits page was not available. Architecture and benchmark recommendations rely primarily on repository documentation and papers.
- A medical “memory prosthesis” claim would require clinical validation. “Second memory” avoids that implication while still describing the function accurately.

In plain words: measure before rebuilding. Clean the index, add lexical search, test four multilingual models, and give facts dates plus sources. Keep Markdown as the diary; let SQLite be the fast, rebuildable map.
