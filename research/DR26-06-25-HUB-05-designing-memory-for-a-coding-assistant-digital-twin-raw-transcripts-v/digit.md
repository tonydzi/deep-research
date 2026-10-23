# DR26-06-25-HUB-05 — raw report from digit

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Designing Memory for a Coding Assistant Digital Twin

## Executive Summary

Do **not** embed full raw Claude Code transcripts into the **same** semantic RAG index as a curated personal knowledge base. The strongest evidence across retrieval research, long-conversation memory work, and production memory systems points to a layered design instead: keep raw transcripts as an archive and evidence layer, extract structured session memory into a separate episodic/semantic memory layer, and preserve the curated vault as its own high-precision semantic layer. Mixing verbose, low-signal transcript chunks with curated notes is likely to reduce precision, increase distraction, and bloat retrieval, especially when the corpus suddenly shifts from a few thousand high-signal notes to hundreds of thousands of noisy session chunks. Long-context studies show that more context is not equivalent to better use of context, and retrieval studies show that irrelevant material degrades both retrieval and downstream reading. citeturn36view0turn19view0turn14view0turn16view0turn23view1

The best-practice architecture for your setup is a **three-plane system** that maps cleanly onto your existing “SQLite = facts, RAG = meaning, markdown = raw thinking” law. Keep raw session transcripts on disk with full-text / grep / metadata search and transcript-span pointers; write deterministic session facts to SQLite; and embed only **distilled, query-shaped memory objects** into a **separate namespace** from curated notes. Retrieval should be routed by query type: curated knowledge questions hit the curated index first; “what did we decide / why did we choose this / what happened in that session?” queries hit the episodic memory namespace; verbatim or forensic requests fall back to the raw archive. This is the pattern that increasingly appears in current agent-memory systems: memory is written selectively, stored in multiple forms, and retrieved just-in-time rather than by dumping whole histories into the same vector pool. citeturn30view0turn22view2turn22view3turn23view0turn26view3turn39view0

The nuance matters: **do not throw away the raw transcripts**. They are valuable, especially for a “digital twin” that wants to preserve decisions, reasoning traces, exploratory dead ends, and interaction style. But preserving them does **not** require making them equal citizens in the same semantic index as curated notes. The literature increasingly favors either hybrid designs that index summaries-plus-raw separately, or hierarchical designs that retrieve abstractions first and drill down to raw evidence only when needed. That gives you the value of preserved reasoning without paying the precision tax on every retrieval. citeturn13search4turn40view0turn35view0turn32view0

## Key Findings

**Retrieval quality falls when low-density text is mixed indiscriminately with higher-signal text.** Chunking research consistently shows that larger chunks carry more irrelevant information, which can interfere with retrieval and generation, while smaller units can become too fragmented; the sweet spot depends on semantic coherence, not raw file boundaries. In conversational memory specifically, SeCom found that turn-level memory is too fine, session-level memory is too coarse, and summary-only memory loses information; segment-level, topically coherent units performed best. citeturn19view0turn14view0

**Longer input and more retrieved material do not reliably rescue poor indexing decisions.** “Lost in the Middle” showed a robust U-shaped effect: models are better with relevant information at the start or end of context and noticeably worse when it is buried in the middle, even for long-context models. LongMemEval extends that lesson to long-term assistant memory: commercial systems and long-context models show large drops as interaction history scales, and the paper explicitly recommends better indexing, retrieval, and reading strategies rather than relying on raw history or long context alone. citeturn36view0turn16view0turn17view0

**Rerankers help, but they are not a magic antidote to a noisy mixed corpus.** Recent evidence shows that cross-encoder rerankers can improve results at small reranking depths, but performance declines when reranking too many candidates; the authors document “phantom hits,” where rerankers assign high relevance to irrelevant documents. In practice, that means a larger and noisier transcript-heavy candidate pool can make reranking less reliable, not more. citeturn33view0

**Distillation works better than many engineers assume, but summary-only memory is not enough.** A recent structured-distillation study on software-engineering conversations reduced token footprint by 11x while preserving nearly all vector-retrieval quality in the best configuration, and some cross-layer setups slightly outperformed pure verbatim baselines. At the same time, SeCom, RMM, and TSM all show why summary-only designs fail: summaries can lose specific evidence, temporal order, or user-specific nuance, so strong systems pair abstraction with access to raw supporting dialogue. citeturn32view0turn14view0turn13search4turn40view0

**Industry memory systems are converging on typed and scoped memory, not one giant transcript index.** LangChain and LangGraph explicitly separate short-term thread state from long-term memory namespaces and distinguish semantic, episodic, and procedural memory. LlamaIndex separates static memory, fact extraction, and vector memory blocks. Letta distinguishes pinned memory blocks from semantically searchable archival memory. Mem0 splits conversation, session, user, and organizational memory. Anthropic’s Claude Code starts each session with a fresh context and carries forward project knowledge via concise `CLAUDE.md` files and auto memory. Cline preserves project state with a Memory Bank and uses compaction summaries as context fills up. These are all strong signals against “same-index raw-everything.” citeturn30view0turn22view2turn22view3turn22view0turn22view1turn23view0turn26view0turn27view0turn27view1

**Emerging agent-memory research is moving toward episodic-vs-semantic splits, reflection memories, and retrieval routing.** CoALA formalized the distinction between working, episodic, semantic, and procedural memory; later work argues episodic memory is the missing piece for long-term agents. Reflection-based systems like Reflexion, ReAP, and UI-Mem store experiences and failure patterns rather than raw logs, while Cloudflare’s 2026 Agent Memory introduces multi-channel retrieval, typed memories, and raw-message search only as a low-weight safety net. citeturn41view0turn31search1turn38search0turn38search5turn38search2turn39view0

## Contradictions & Debates

There is a real argument **for** preserving raw transcript memory. Full transcripts preserve detail that summaries often erase: precise wording, chronological order, abandoned hypotheses, failure patterns, and “how we got there,” not just the final answer. This matters for a digital twin, and some benchmarks still show a full-context baseline can outperform compressed memory on absolute answer quality in certain settings. In the Mem0 paper, the full-context baseline had the best overall score on LoCoMo, albeit at much higher latency and token cost than structured memory. That is the strongest pro-raw argument: some nuance is genuinely lost when you compress. citeturn23view1turn24view2turn24view3

But the argument **against** mixing raw transcript chunks into the curated semantic layer is stronger for your specific use case. Your curated vault exists to maximize semantic precision over a relatively small, high-signal set of notes. Session transcripts are the opposite: they contain retries, tool chatter, repeated assistant prose, local dead ends, and transient state. SeCom’s framing is directly relevant here: session-level memory is too coarse and drags in irrelevant material; turn-level memory fragments multi-turn meaning; summary-only loses detail. That combination is exactly why “raw transcripts in the same index as curated notes” is the wrong compromise: it inherits the weaknesses of both coarse storage and noisy retrieval. citeturn14view0

The best current systems resolve this by **splitting the problem**. RMM stores each memory entry as a **topic summary plus raw dialogue**, where the summary serves as the search key and the raw dialogue remains available as the payload. TSM similarly retrieves over summaries and raw dialogue together, uses temporal constraints to filter summaries, and then promotes raw turns as evidence. Cloudflare’s production Agent Memory extracts typed memories into storage while also keeping raw-message search as a low-weight fallback for cases where extraction generalized something away. Those systems are not choosing between raw and distilled. They are choosing **distilled-first, raw-available**. citeturn13search4turn40view0turn39view0

There is also a debate about how much distillation is safe. Summary-key retrieval can outperform raw-dialogue retrieval because it denoises conversational text and aligns retrieval with user intent; NAACL 2024 conversation retrieval found that summaries used as retrieval keys and queries outperformed raw dialogues, and then showed that a smaller encoder could be distilled to approximate summary-space retrieval without generating summaries online. But over-distillation can erase exactly the thing you care about: reasoning value. The newest structured-distillation work is encouraging because it preserves retrieval quality in vector search while retaining raw source for drill-down; importantly, the same paper reports that BM25 degraded significantly under distillation, which means the success of distillation depends on the retrieval mechanism you use. citeturn15view0turn32view0

## Best Practices

The clearest best practice is to separate memory by **function**, not just by storage technology. Research and frameworks increasingly converge on at least three categories: **semantic memory** for stable facts and conceptual knowledge, **episodic memory** for what happened in specific sessions or runs, and **procedural memory** for reusable rules, workflows, and instructions. LangChain makes this split explicit; CoALA formalizes it; Mem0, Letta, LlamaIndex, Claude Code, and Cline all operationalize variants of it in product form. citeturn30view0turn41view0turn23view0turn22view0turn22view3turn26view0turn27view0

The second best practice is to keep **raw data and retrieval keys in different shapes**. RMM uses a summary as the retrieval key and raw dialogue as the evidence payload. Conversation retrieval work shows summaries are often better search keys than raw dialogue. TSM shows that compact summaries and persona/topic abstractions improve retrieval, but raw dialogue still matters for evidence and temporal grounding. In other words: use compact, queryable abstractions to win retrieval; use raw spans to preserve fidelity and support drill-down. citeturn13search4turn15view0turn40view0

The third best practice is to use **separate namespaces or filtered stores**, not one blended vector pool. LangGraph’s long-term memory is namespaced JSON; Letta’s archival memory is distinct from pinned memory blocks; Mem0 layers conversation, session, user, and org memory; OpenAI’s project memory and Anthropic’s project knowledge are scoped rather than globally blended; Pinecone’s own metadata-filtering research argues that document type, user context, and recency filtering are essential to quality in RAG systems. This is not just an organizational convenience. It is a relevance-control mechanism. citeturn22view2turn23view0turn25view3turn25view5turn34view0

The fourth best practice is to treat retrieval as **routed and fused**, not monolithic. Cloudflare’s Agent Memory explicitly says no single retrieval method worked best for all queries, so it runs several channels in parallel, fuses them, and keeps raw-message search as a low-weight safety net. That pattern is directly applicable here: curated semantic search should dominate for concept questions; episodic memory should dominate for session-history questions; raw transcript search should act as an evidence fallback for “show me what happened” or “quote the actual discussion” requests. citeturn39view0

The fifth best practice is to preserve **time and supersession**, because memory without update semantics becomes stale noise. Zep stores facts with validity windows and separates facts, summaries, and episodes; Mem0 layers short- and long-term memory by scope; TSM uses temporal validity and “sleep-time” consolidation; LongMemEval explicitly identifies knowledge update and temporal reasoning as core memory abilities. For coding sessions, that means decisions should have `supersedes`, `valid_from`, `valid_to`, and repository / branch / file metadata, so old conclusions stop outranking new ones. citeturn21view0turn21view1turn23view0turn40view0turn16view0

A concrete implementation for your stack can stay simple:

```text
RAW PLANE
- Store full Claude Code transcripts on disk as markdown/jsonl
- Add full-text search + transcript_id + turn/span pointers
- Never use as default semantic retrieval context

FACT PLANE
- SQLite ledger for deterministic extractions:
  decisions, prefs, files touched, tools used, commits, errors, timestamps, pointers

MEANING PLANE
- curated_index        : only curated notes
- episodic_index       : distilled session memory objects
- procedural_index     : reusable rules/workflows/coding preferences

ROUTER
- curated-first for concept/knowledge questions
- episodic-first for "what did we discuss/decide/why" questions
- raw fallback only for verbatim / forensic / evidence requests

READER
- retrieve little
- rerank within or across namespaces
- attach transcript pointers for drill-down
```

That architecture is consistent with the strongest evidence you have and with the machinery you already built. It is also repairable: each layer can be rebuilt independently, audited independently, and rolled back independently. citeturn30view0turn22view2turn22view3turn23view0turn39view0

## Failure Modes

The main failure mode of a single mixed transcript-plus-curated index is **precision dilution**. If your curated layer is only a few thousand notes, adding roughly 180,000 transcript chunks changes the candidate distribution radically. Even if embeddings are decent and reranking is present, more candidate noise means more chances for semantically similar but operationally irrelevant transcript chunks to crowd out higher-value curated notes. Long-context reading then compounds the problem, because once irrelevant context is included, models get distracted and performance drops. citeturn19view0turn33view0turn36view0turn30view0

A second failure mode is **wrong granularity**. Raw turn-level chunks often split a decision across multiple messages; whole-session chunks pull in entire blobs of irrelevant context. SeCom shows both are bad in different ways. For coding assistant data, this is especially acute: one “decision” may span a user request, assistant plan, tool call, error, revised plan, and final resolution. If you index those naively, retrieval either returns incomprehensible fragments or huge noisy bundles. citeturn14view0

A third failure mode is **over-distillation**. If you compress a session down to a single generic summary, you may lose the rationale, rejected alternatives, or temporal order that make the memory useful. TSM’s ablation shows removing summaries hurts some categories, but relying on abstractions alone also misses important evidence; its best behavior comes from combining summaries with raw chunks and temporal facts. RMM similarly keeps a topic-summary search key paired with raw dialogue. citeturn40view0turn13search4

A fourth failure mode is **assuming rerankers will save a bad index design**. “Drowning in Documents” is a warning here: as reranking depth grows, recall can degrade, and rerankers can produce phantom hits. In a mixed corpus where noisy transcript chunks dominate candidate generation, the reranker itself can become another source of drift rather than a reliable repair mechanism. citeturn33view0

A fifth failure mode is **stale or unsuperseded session memory**. Coding workflows evolve quickly. A decision from three sessions ago may be wrong now because the branch changed, a dependency was upgraded, or an experiment failed later. Zep, TSM, and LongMemEval all point toward the same requirement: memory must support updates, invalidation, and time-aware retrieval, not just append-only semantic similarity. citeturn21view0turn21view1turn40view0turn16view0

## Strategic Recommendations

For **this** setup, the highest-confidence recommendation is:

**Keep the raw transcripts, but remove them from the default curated semantic index.** Preserve them as a raw archive plus full-text / metadata searchable evidence layer. Do **not** let them compete directly with curated notes in the same default retrieval pool. This is the cleanest way to preserve digital-twin value without harming curated precision. citeturn14view0turn36view0turn33view0

**Create a separate episodic memory namespace for distilled session objects.** Write session-derived items such as decisions, preferences, tradeoffs, failures, and active work state into a distinct vector namespace and/or SQLite tables. The episodic namespace should be separately retrievable and separately weightable. LongMemEval, Mem0, TSM, LangGraph, and Letta all support the idea that memory works better when storage is scoped and retrieval is selective. citeturn17view0turn23view1turn40view0turn22view2turn22view0

**Use your deterministic ledger as the hot-path write layer and your nightly “dream consolidation” as the promotion layer.** That is not a workaround; it is close to where the field is moving. Cline compacts by summarization when context fills; Cloudflare ingests on compaction and extracts typed memories; TSM uses low-latency online updates plus slower “sleep-time” summary consolidation. Your current pilot is already aligned with this pattern. citeturn27view1turn39view0turn40view0

**Extract from coding sessions only what improves future retrieval.** The useful categories are: final decisions; rationale summaries; rejected alternatives worth remembering; user preferences and standing instructions; files/modules/components touched; important commands/tools collapsed into normalized actions; failure signatures and debugging lessons; unresolved TODOs / next steps; repo/branch/commit/PR references; and transcript-span pointers for later drill-down. Conversational fluff, repetitive assistant recap, bulk stdout/stderr, unchanged diffs, and low-value tool chatter should stay in raw storage only. This recommendation is strongly supported by the typed-memory designs in Zep, Cloudflare Agent Memory, Mem0, and LlamaIndex fact/vector memory blocks. citeturn21view0turn21view1turn39view0turn23view0turn22view3

A practical memory object for the episodic layer could look like this:

```json
{
  "memory_type": "decision",
  "project": "repo-name",
  "session_id": "claude-code-2026-06-24-abc123",
  "title": "Moved auth token refresh into middleware",
  "summary": "We stopped refreshing tokens in route handlers and centralized it in middleware to avoid duplicated retry logic.",
  "rationale": [
    "reduces duplicated refresh code",
    "prevents inconsistent 401 handling",
    "keeps request path simpler"
  ],
  "alternatives_rejected": [
    "per-route refresh wrapper",
    "global axios interceptor in frontend only"
  ],
  "files_touched": ["server/middleware/auth.ts", "server/routes/*.ts"],
  "tools_used": ["grep", "tests", "ripgrep"],
  "status": "current",
  "valid_from": "2026-06-24T18:42:00Z",
  "supersedes": null,
  "transcript_pointer": {
    "file": "sessions/2026-06-24-auth-refactor.md",
    "turn_start": 43,
    "turn_end": 71
  }
}
```

**Default retrieval policy should favor the curated layer.** If the user asks a general conceptual or domain question, the system should query the curated vault first and only pull episodic memory if the query clearly references prior discussions, decisions, or personalized preferences. If the user asks for verbatim reconstruction, chronology, or forensic recall, then the router should explicitly hit the raw archive or mixed episodic+raw path. This is the design pattern behind Anthropic’s project knowledge search, OpenAI’s project memory, Letta archival memory, LangGraph namespaces, and Cloudflare’s multi-channel memory retrieval. citeturn25view3turn25view5turn22view0turn22view2turn39view0

**Do not rely on vector search alone for raw transcripts.** Keep a sparse / full-text path for filenames, error strings, commands, and exact phrases, because exact-match needs survive even in a semantic memory stack. Structured Distillation found vector search preserved well under compression while BM25 degraded; Cloudflare explicitly keeps raw-message full-text search as a safety net. For coding-session archives, that means exact grep and transcript-span access should remain first-class. citeturn32view0turn39view0

The unresolved question is not “raw or distilled?” It is **how much raw access to preserve in the retrieval loop**. The safest answer from the literature is: raw should be **available**, not **default**. The best systems retrieve abstractions first and only dive into raw evidence when the query, confidence, or task requires it. citeturn13search4turn40view0turn39view0

## Action Plan

**Immediate containment**

1. **Quarantine the newly added transcript chunks** by moving them out of the default curated retrieval path. If that is easier operationally, rebuild the curated index from scratch and place transcript-derived data in a separate namespace/store.  
2. **Freeze new raw transcript embedding into the curated namespace** until the evaluation below is complete.  
3. **Keep the raw files on disk** and make sure each session transcript has stable IDs, timestamps, project/repo metadata, and turn-span addressing. citeturn26view0turn27view2turn27view3

**Implement the layered write path**

1. **Hot path, zero-token deterministic extraction** into SQLite: decisions, files, tools, commands, outcomes, errors, branch/commit references, pointer to transcript span.  
2. **Nightly distillation into quarantine**: create candidate memory objects for decisions, preferences, reusable workflows, failure patterns, and unresolved work state.  
3. **Promotion rules**: only promote a candidate to the episodic semantic namespace if it is non-duplicate, scoped, and useful for future retrieval. Supersede older entries instead of appending near-duplicates forever. citeturn39view0turn21view0turn40view0

**Build the router**

Use a simple deterministic classifier first. Route to:
- **Curated only** for conceptual / PKB questions.
- **Episodic first** for “what did we decide / discuss / try / conclude?” questions.
- **Raw archive** for verbatim, chronology, file-path, error-string, or forensic recall.
- **Hybrid fusion** for ambiguous queries, but keep a source prior that favors curated results unless the query explicitly seeks session history. citeturn39view0turn34view0turn22view2

**Run the A/B before committing**

Build a gold set of roughly 100–200 queries split across:
- curated-knowledge questions,
- prior-decision questions,
- preference/instruction questions,
- temporal/session-history questions,
- file/tool/error recall questions,
- verbatim-evidence requests.

Compare at least four configurations:
- curated-only baseline,
- mixed raw+curated same index,
- curated + separate episodic namespace,
- curated + episodic namespace + raw fallback router.

Measure:
- retrieval precision@k / nDCG / MRR,
- answer faithfulness and source correctness,
- contamination rate on curated queries,
- latency,
- index size and daily growth,
- duplicate-hit rate,
- human preference on difficult session-history questions. citeturn16view0turn23view1turn32view0turn39view0

**Adopt explicit go/no-go thresholds**

A sensible engineering rule for this stack would be:
- reject any design that materially lowers curated-query precision,
- reject any design where transcript-derived chunks dominate top results on curated-only questions,
- reject any design that cannot surface a transcript span when answering a session-history question,
- prefer the simplest design that preserves curated precision while materially improving session recall.

Given the evidence already available, the most likely winner is **curated index + separate episodic memory namespace + raw transcript archive with explicit router and drill-down**. citeturn14view0turn33view0turn39view0turn40view0

## Sources@Deep research

**Academic and research papers**

- Liu et al., *Lost in the Middle: How Language Models Use Long Contexts*. citeturn36view0  
- Wu et al., *LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory*. citeturn16view0turn17view0  
- Pan et al., *On Memory Construction and Retrieval for Personalized Conversational Agents*. citeturn14view0  
- Lee et al., *Effective and Efficient Conversation Retrieval for Dialogue State Tracking with Summaries*. citeturn15view0  
- Sarthi et al., *RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval*. citeturn35view0  
- Dense retrieval granularity / proposition indexing results. citeturn18view0  
- Tan et al., *Reflective Memory Management for Long-term Personalized Dialogue Agents*. citeturn13search4  
- Su et al., *Temporal Semantic Memory for Personalized LLM Agents*. citeturn40view0  
- Lewis, *Structured Distillation for Personalized Agent Memory*. citeturn32view0  
- Anonymous / recent IR work, *Drowning in Documents: Consequences of Scaling Reranker Inference*. citeturn33view0  
- Sumers et al., *Cognitive Architectures for Language Agents*. citeturn41view0  
- Pink et al., *Position: Episodic Memory is the Missing Piece for Long-Term LLM Agents*. citeturn31search1  
- Shinn et al., *Reflexion: Language Agents with Verbal Reinforcement Learning*. citeturn38search0  
- Azam et al., *Reflection-Based Memory for Web Navigation Agents*. citeturn38search5  
- Xiao et al., *UI-Mem: Self-Evolving Experience Memory for Online RL GUI Agents*. citeturn38search2  

**Official product and framework documentation**

- LangChain / LangGraph memory overview and long-term memory namespaces. citeturn30view0turn22view2  
- LlamaIndex memory blocks: static, fact extraction, vector memory. citeturn22view3  
- Letta stateful agents and archival memory. citeturn22view0turn22view1  
- Mem0 memory layers and paper. citeturn23view0turn23view1  
- Zep graph, facts/summaries, and temporal knowledge graph. citeturn21view0turn21view1turn21view2  
- Anthropic Claude Code memory and memory tool docs. citeturn26view0turn26view3  
- Anthropic project knowledge and chat search/memory docs. citeturn25view4turn25view5turn26view1turn26view2  
- OpenAI memory and project memory docs. citeturn25view0turn25view1turn25view3  
- Cline Memory Bank, Auto Compact, and task history. citeturn27view0turn27view1turn27view2  
- Aider history files and conventions. citeturn27view3turn27view4  
- Cloudflare Agent Memory architecture and retrieval pipeline. citeturn39view0  
- Pinecone metadata filtering and namespaces as relevance controls. citeturn34view0