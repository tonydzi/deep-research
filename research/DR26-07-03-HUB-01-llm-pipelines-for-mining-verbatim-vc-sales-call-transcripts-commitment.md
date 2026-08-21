---
dr_id: DR26-07-03-HUB-01
title: "LLM pipelines for mining verbatim VC/sales call transcripts: commitment/fact extraction +"
date: 2026-07-03
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-03-HUB-01): LLM pipelines for mining verbatim VC/sales call transcripts: commitment/fact extraction + cross-call patterns

> What are mid-2026 best-practice architectures for turning large corpora of verbatim RU/EN VC call transcripts into grounded per-call extractions (commitments, decisions, facts) plus cross-call analytics (talk ratios, objection patterns, alpha signals)?

## Ключевые выводы
- Distillation, not ASR, is the bottleneck in 2026 — capturing verbatim RU/EN transcripts (e.g. via Granola) is solved; the hard problem is turning sprawling diarized text into structured commitments and CRM data.
- Best-practice per-call extraction schema is strict JSON (participants_mapped, action_items, decisions, key_facts, open_questions, talk_ratio) with a mandatory evidence_quote + segment ID on every field — grounding is the single most effective anti-hallucination control.
- Extract commitments as explicit-only ('I/we will X by Y') rather than implicit/inferred, trading recall for precision and trust; most production systems converge on this bias.
- Long transcripts should use topic-aware embedding-based chunking (cosine similarity on speaker turns beats linear/time splits) plus map-reduce/hierarchical aggregation rather than relying purely on long-context windows — cheaper, more controllable, less hallucination-prone.
- Speaker mapping (mic-vs-speaker → real names) is reliably solved with a dedicated LLM step fed calendar participant lists + roles/emails + transcript self-intro cues, with confidence scores and a review queue for low-confidence cases.
- Modern async STT (AssemblyAI Universal, Gladia) handles RU/EN code-switching in one pass; frontier LLMs process mixed-language input natively, so full translation is unnecessary.
- Cross-call analytics should combine classic NLP (embeddings + HDBSCAN/UMAP or BERTopic-style clustering for objection/pitch/topic taxonomies, talk ratio from diarization) with LLM map-reduce (map each call to structured objections/pitch-elements/commitment-status, then reduce across the corpus).
- Gong is the industry benchmark worth cloning selectively: talk ratio (~40-45% for top performers), objection/buying-signal detection, deal-risk patterns, promise-completion rates as a business KPI — feasible in-house; skip heavy forecasting until outcome labels exist. Fathom/Fireflies are lighter (reliable per-call + CRM sync) but lack deep multi-call intelligence.
- Commitment tracking architecture: SQLite table (call_id, lead_id, owner, task, due_date, evidence_quote, status, last_checked) updated nightly by scanning new transcripts for references to prior open items, or via CRM task webhooks; Gemini additionally recommends separating immutable transcript facts from mutable operational state (status/health) in the schema.
- Recommended cost architecture: nightly batch processing + a model ladder (cheap detector/rules or embeddings for candidates → small/fast LLM for bulk extraction → Sonnet/Opus for judgment, synthesis, and cross-call alpha), with caching of embeddings/summaries and eventual fine-tuning/distillation of small local models for high-volume repetitive tasks (objection classification, commitment detection).
- QC should be multi-stage: cheap detector → extractor → LLM-as-judge verification against source text, plus periodic human sampling and evaluation beyond ROUGE/BERTScore (grounded factuality judging, hallucination/omission/attribution-error tracking, promise-completion-rate KPI).
- Gemini uniquely recommends turning Obsidian into an active semantic substrate via Dataview queries over the JSON extraction output, so the vault dynamically surfaces at-risk commitments and unresolved diligence questions on the daily note, bridging the SQLite database and human intuition.

## Рекомендации / решения
- Phase 1 (1-2 weeks): lock a v1 extraction JSON schema (action_items, decisions, facts, participants_mapped, all with evidence_quote), build a speaker-mapping LLM step from calendar data, run nightly map-reduce extraction into JSON + Obsidian + SQLite, add basic talk-ratio metric, and test on 20-30 recent RU/EN calls.
- Phase 2 (3-6 weeks): add vector embeddings (per call + key segments) and clustering (BERTopic/HDBSCAN) for cross-call aggregates; run first LLM map-reduce pass for objection/pitch patterns; build the commitments table with status workflow; add a cheap-detector→strong-judge verification pipeline for commitment candidates.
- Phase 3 (2-3 months): full commitment tracking with nightly scans for status updates, deeper alpha/pattern mining (language features vs. follow-through/outcome), build an eval harness (LLM judge + human sampling) and dashboard, consider fine-tuning/distilling a small local model.
- Phase 4 (ongoing): expand alpha signal detection, automate follow-up drafting, keep versioning prompts/schemas and tuning the model ladder.
- Clone selectively from Gong (talk ratio, objection taxonomy, promise tracking) rather than building full multi-call forecasting; prioritize grounded per-call extraction first since it feeds everything downstream and solves the 'distrust of AI summaries' problem.
- Use a hybrid model strategy: keep/expand the cheap-detector → LLM-judge ladder already in place, add local/open models for bulk processing and embeddings, and consider local vector store + local inference for privacy-sensitive VC calls (Meetily-style).
- Structure SQLite with immutable extracted facts separate from mutable status/health fields (Gemini's specific recommendation) and layer an Obsidian Dataview dashboard on top as the human-facing view.

## Сущности
- **Люди:** —
- **Компании:** Gong, Fathom, Fireflies, Chorus, AssemblyAI, Gladia, Recall.ai, Granola, Meetily, AWS
- **Продукты/инструменты:** Whisper, Ollama, BERTopic, HDBSCAN, UMAP, sqlite-vec, PGVector, Instructor, LangChain, LlamaIndex, Voyage embeddings, Obsidian Dataview, SQLite, Claude (200k+ context)

## Открытые вопросы
- ChatGPT's Deep Research run stalled/failed to render its report body — its independent perspective was not captured and should be re-fetched if the URL later produces content.
- Whether explicit-only commitment extraction misses too much nuance/recall compared to broader implicit-commitment detection remains an open trade-off debate.
- Whether full long-context processing (e.g., Claude 200k+) could replace chunking+map-reduce as costs/context windows evolve is unresolved — current consensus favors chunking for cost/control but acknowledges the trade-off.
- The exact point at which local/fine-tuned small models outperform frontier APIs on cost/speed/privacy for this specific volume (~253 calls/1.2M words) has not been empirically tested here.
- Gemini's full ~73k-character report could not be captured verbatim (tool truncation) — only executive summary and strategic recommendations were retrieved; full recapture recommended when a non-truncating path is available.

## Источник
- DR-ID `DR26-07-03-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- second-brain-northstar
- alpha-protocol-recall-plus-dr
- obsidian-ingest
- granola-sync
- crm-sync
- model-routing-sonnet-grunt
