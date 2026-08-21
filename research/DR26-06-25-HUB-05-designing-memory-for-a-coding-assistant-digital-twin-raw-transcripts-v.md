---
dr_id: DR26-06-25-HUB-05
title: "Designing Memory for a Coding Assistant Digital Twin (raw transcripts vs curated RAG)"
date: 2026-06-25
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-25-HUB-05): Designing Memory for a Coding Assistant Digital Twin (raw transcripts vs curated RAG)

> Should raw Claude Code session transcripts be embedded into the same semantic RAG index as a curated personal knowledge base, and if not, what architecture should replace it?

## Ключевые выводы
- Do NOT mix raw session transcripts into the same semantic index as curated notes — retrieval research shows low-density/noisy text mixed with high-signal text reduces precision and distracts downstream reading (Lost in the Middle, LongMemEval, chunking studies).
- Longer context / more retrieved material does not fix bad indexing: Lost in the Middle shows a U-shaped effect where mid-context info is used worse; LongMemEval shows large performance drops as interaction history scales even for long-context models.
- Rerankers help only at shallow depth — 'Drowning in Documents' shows reranking too many candidates causes 'phantom hits' (high relevance scores on irrelevant docs), so a noisy transcript-heavy pool makes reranking less reliable, not more.
- Structured distillation can cut token footprint ~11x while preserving nearly all vector-retrieval quality, but BM25/sparse retrieval degrades significantly under distillation — so distillation's success depends on retrieval mechanism used.
- Summary-only memory fails: SeCom shows turn-level memory is too fragmented, session-level too coarse, summary-only loses evidence/temporal order/nuance — segment-level topically coherent units perform best; best systems (RMM, TSM) pair a summary as retrieval key with raw dialogue as payload.
- Industry memory systems (LangChain/LangGraph, LlamaIndex, Letta, Mem0, Claude Code's CLAUDE.md+auto memory, Cline's Memory Bank) all converge on typed/scoped memory — semantic vs episodic vs procedural — never one giant raw-transcript vector pool.
- Full-context baseline (Mem0/LoCoMo) can still win on raw answer quality in some settings, at much higher latency/token cost — so raw transcripts retain genuine standalone value, just not as default retrieval context.
- Memory needs update/supersession semantics (Zep's validity windows, TSM's temporal constraints, LongMemEval's temporal reasoning) — coding decisions go stale as branches/dependencies change, so entries need supersedes/valid_from/valid_to fields.

## Рекомендации / решения
- Adopt a three-plane architecture: RAW plane (transcripts on disk, full-text/grep search, transcript-span pointers, never default semantic context) + FACT plane (SQLite ledger of deterministic extractions: decisions, files, tools, commits, errors, timestamps) + MEANING plane (separate curated_index / episodic_index / procedural_index).
- Build a query router: curated-first for concept/knowledge questions, episodic-first for 'what did we decide/discuss/why' questions, raw-archive fallback only for verbatim/forensic/chronology requests, hybrid fusion for ambiguous queries with a prior favoring curated results.
- Extract into episodic memory objects only high-value content: final decisions, rationale, rejected alternatives, user preferences/standing instructions, files/modules touched, normalized tool/commands, failure signatures, unresolved TODOs, repo/commit/PR refs, and a transcript-span pointer for drill-down; leave conversational fluff/stdout/repeated recap in raw storage only.
- Keep a nightly 'dream consolidation' distillation step that promotes candidate memory objects to the episodic namespace only if non-duplicate and useful, superseding older entries rather than appending duplicates forever.
- Never rely on vector search alone for raw transcripts — keep a sparse/full-text/grep path for filenames, error strings, exact commands/phrases as a safety net.
- Before committing to any design, run an A/B with a 100-200 query gold set across curated-only, mixed raw+curated, curated+episodic, and curated+episodic+raw-router configurations; measure precision@k/nDCG/MRR, faithfulness, contamination rate on curated queries, latency, index growth, duplicate-hit rate.
- Adopt go/no-go thresholds: reject any design that lowers curated-query precision, lets transcript chunks dominate curated-only results, or fails to surface a transcript span for session-history questions; the predicted winner is curated index + separate episodic namespace + raw archive with explicit router.

## Сущности
- **Люди:** Liu et al., Wu et al., Pan et al., Lee et al., Sarthi et al., Tan et al., Su et al., Lewis, Sumers et al., Pink et al., Shinn et al., Azam et al., Xiao et al.
- **Компании:** Anthropic, LangChain, LlamaIndex, Letta, Mem0, Zep, Cline, Aider, Cloudflare, Pinecone, OpenAI
- **Продукты/инструменты:** Claude Code, CLAUDE.md, LangGraph, RAPTOR, SeCom, RMM (Reflective Memory Management), TSM (Temporal Semantic Memory), LongMemEval, CoALA, Reflexion, ReAP, UI-Mem, Cloudflare Agent Memory

## Открытые вопросы
- How much raw access should remain in the retrieval loop by default (vs. available-on-demand) — the literature favors 'available, not default' but exact thresholds aren't settled.
- What granularity boundary correctly captures a full coding 'decision' spanning request→plan→tool call→error→revision→resolution without fragmenting or over-bundling.
- Whether BM25/sparse degradation under distillation applies to this specific corpus/retrieval stack, or only to the structured-distillation study's setup.
- Concrete promotion/deduplication rules for episodic memory objects (when exactly to supersede vs. append) are sketched but not fully specified.
- The A/B evaluation (100-200 query gold set across 4 configurations) is proposed but not yet run for this specific setup.

## Источник
- DR-ID `DR26-06-25-HUB-05` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-25-HUB-05-designing-memory-for-a-coding-assistant-digit.md`

## Связано
- [[vault-data-architecture]]
- [[always-on-memory-pilot]]
- [[second-brain-northstar]]
- [[episodic-memory]]
- [[RAG-retrieval-design]]
- [[session-machine-tagging]]
