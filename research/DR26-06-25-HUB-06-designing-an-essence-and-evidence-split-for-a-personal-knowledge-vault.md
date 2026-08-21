---
dr_id: DR26-06-25-HUB-06
title: "Designing an Essence and Evidence Split for a Personal Knowledge Vault"
date: 2026-06-25
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-25-HUB-06): Designing an Essence and Evidence Split for a Personal Knowledge Vault

> How should a large heterogeneous personal vault be architected so a semantic 'digital twin' index stays clean while raw source material remains retrievable and provable?

## Ключевые выводы
- The right architecture is a two-plane system: ESSENCE (durable, self-contained, reusable semantic notes — embedded/searched by meaning) vs EVIDENCE (source-of-record archive kept retrievable but not embedded), linked by explicit provenance.
- This mirrors both knowledge-management theory (Ahrens fleeting/permanent notes, Matuschak atomic evergreen notes, Forte's Progressive Summarization — raw capture must be transformed into durable artifacts, not kept as-is) and modern AI memory systems (LangChain/LangMem semantic vs episodic vs procedural memory, Mem0 promoting durable facts out of episodic summaries, Zep's temporal graph separating raw episodes from entities, Letta's in-context vs archival memory, LlamaIndex's distinct memory blocks, Claude Code's own selective auto-memory).
- Retrieval research supports narrow, atomic indexing: Dense X Retrieval shows proposition-level units beat passage-level for dense retrieval/QA; RAPTOR shows hierarchical summaries help multi-step reasoning; chunking strategy materially affects quality; 'Lost in the Middle' shows models degrade when relevant info is buried in long context.
- Heterogeneous corpora cause 'retrieval dilution' — as corpora mix categories (concepts, transcripts, ledgers, logs), dense retrieval loses discriminative power even with correctly functioning ANN search; naive flat RAG needs domain scoping/routing to avoid this.
- Counter-finding / key caveat: a 2026 ablation on long-conversation memory found verbatim chunks outperform extracted artifacts on two conversational-retrieval benchmarks — distillation is lossy, so raw evidence must never be discarded, only excluded from the primary semantic index.
- Provenance is critical: evidence-tracing and traceable-text research shows essence notes need span-level (not just document-level) links back to source; missing provenance should mean quarantine, not promotion to essence.
- Essence should be treated as revisable semantic state (status: active/superseded, confidence, valid_from/valid_to) while evidence stays immutable — lacking temporal/versioning metadata leads to 'maintenance drift' where the twin speaks with stale certainty.
- Classification should be deterministic-first, probabilistic-second, LLM-third: explicit frontmatter markers > importer/folder priors > structural/lexical heuristics (speaker prefixes, email headers, timestamp density, claim-like titles, link density) > LLM judge only for the ambiguous middle band, calibrated against human-labeled samples (weak supervision/Snorkel-style).

## Рекомендации / решения
- Make `layer: essence|evidence` a mandatory frontmatter field and the source of truth; only `layer: essence` notes enter the primary embedded/semantic index.
- Keep evidence (transcripts, email dumps, chat imports, ledgers, tool logs) on disk, unembedded by default, searchable via lexical methods (BM25/grep) first; add a separate episodic vector namespace only if semantic recall over raw discussions is later needed.
- Require every essence note to carry `evidence_refs` (file/note ID + line or span anchors, kind of source) pointing back to its origin; treat missing provenance as a quarantine signal, not an essence note.
- Adopt a full frontmatter schema: id, layer, memory_type (semantic/episodic/procedural/source_record), source_type, status, confidence, valid_from/valid_to, supersedes/superseded_by, topics, person_scope, evidence_refs, classifier metadata.
- Run backfill classification in 3 passes: deterministic auto-label the obvious tails → human review + calibrate on a stratified ambiguous sample → re-run and produce a diff report (counts moved to essence/evidence/still uncertain/missing provenance).
- Split retrieval into two services: a 'mind service' (dense retrieval + rerank over essence only) and a 'proof service' (BM25/grep + exact span lookup over evidence), and route queries by type (belief/principle questions → essence; 'what exactly was said'/'show source' questions → evidence).
- Use signal-density metrics (lexical density, entropy, link density, assertion density) only to prioritize review/ranking, never as the sole decision rule for essence/evidence classification.
- Before/after the split, run an A/B evaluation with a gold query set split into essence queries, evidence queries, and mixed queries, measuring label precision/recall, essence-index leakage, P@10/NDCG@10, evidence-hit rate, faithfulness, latency and storage growth; calibrate any LLM judges against human labels.

## Сущности
- **Люди:** Andy Matuschak, Tiago Forte, Sönke Ahrens
- **Компании:** LangChain, Mem0, Zep, Letta, LlamaIndex, Cloudflare, Anthropic (Claude Code), OpenAI
- **Продукты/инструменты:** LangMem, Graphiti, Snorkel, RAPTOR, Dense X Retrieval, RAGAS, BM25

## Открытые вопросы
- Where exactly is the threshold between a note that is 'still mostly evidence with a takeaways section' vs one that qualifies as essence — boundary cases need explicit per-note judgment, not a fixed rule.
- How much raw-chunk retrieval capability (per the verbatim-beats-extracted-artifacts ablation) needs to be preserved in the episodic/evidence layer before fidelity loss becomes a real problem for the twin.
- How reliable LLM judges are for scaling classification of the ambiguous middle band — reliability/consistency/bias are called out as open problems requiring rubric design and calibration, not yet solved.
- No single information-density metric is sufficient on its own for essence/evidence classification — exact feature combination and thresholds are unresolved.

## Источник
- DR-ID `DR26-06-25-HUB-06` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- always-on-memory-pilot
- memory-index-hygiene
- second-brain-northstar
- capture-rules-into-bible
- relink-mechanism
- provenance-attribute-real-author
