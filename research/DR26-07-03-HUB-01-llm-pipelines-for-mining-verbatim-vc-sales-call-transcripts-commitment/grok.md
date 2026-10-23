# DR26-07-03-HUB-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: Grok (grok.com, Expert mode) | topic: DR26-07-03-HUB-01 call-transcript-mining | date: 2026-07-04 | origin: grok | 81 sources -->

# DR26-07-03-HUB-01 — Grok (Expert) report — LLM Pipelines for VC Call Transcripts

## A. Executive Summary

Mid-2026 best practices for LLM pipelines mining large verbatim sales/VC call transcript corpora center on grounded, structured per-call extraction (explicit commitments/decisions/facts with owners, due dates, and source quotes), layered with embedding-driven cross-call pattern mining (talk ratios, objection taxonomies, pitch patterns, promise completion), and strict hallucination controls via evidence grounding + LLM-as-judge verification. Classic benchmarks (AMI, ICSI, QMSum) and recent LLM work emphasize hierarchical/map-reduce processing for long diarized transcripts, topic-aware chunking, and action-item-focused extraction over generic summarization.

Industry leaders like Gong demonstrate high-value cross-call analytics (talk ratios ~40-45% for top performers, objection/buying signal detection, deal-risk patterns, rep coaching) worth cloning selectively. Fathom and Fireflies excel at reliable per-call action items + CRM sync but lag on deep multi-call intelligence. Open-source efforts (e.g., Meetily) highlight viable local/privacy-focused paths using Whisper-class STT + Ollama/strong open models.

For this setup (~253 calls / 1.2M words, Granola mic-vs-speaker diarization, RU/EN mixed VC deal-flow, nightly batch, cheap-detector→LLM-judge ladder, Obsidian+SQLite CRM, local GPU):
- Core win: Enforce explicit-only extraction with verbatim evidence_quote + timestamp/segment ID in every structured record. Directly addresses distrust of AI summaries and kills hallucinated attributions.
- Speaker mapping from calendar metadata + transcript context (self-intros, roles) is reliable with a dedicated LLM step.
- RU/EN code-switching handled well by modern STT (async preferred) and frontier LLMs with targeted prompts; no full translation needed.
- Nightly batch + model ladder near-optimal; add vector embeddings + lightweight clustering (BERTopic-style) + LLM map-reduce for patterns/alpha.
- Commitment tracking lives in SQLite (linked to calls/leads) with status updates from follow-up transcripts or CRM tasks.

Trade-offs favor precision over recall on commitments (explicit language only), hierarchical processing over pure long-context for cost/control, and grounded structured output (Pydantic/Instructor-style) over free-form summaries.

## B. Key Findings

### 1. Per-call extraction schemas & prompts
Effective schemas are strict JSON:
```json
{
"participants_mapped": [{"label": "Speaker A / Them", "name": "Alex CEO", "role": "Founder", "confidence": 0.85}],
"action_items": [{"owner": "Alex", "task": "Send updated deck by Friday", "due_date": "2026-07-10", "evidence_quote": "Alex: 'I'll send the updated deck by Friday.'", "evidence_segment_id": "turn_47", "confidence": 0.9, "status": "open"}],
"decisions": [...],
"key_facts": [...],
"open_questions": [...],
"talk_ratio": {"them_pct": 62, "me_pct": 38}
}
```
Prompt patterns: detailed system prompt defining "explicit commitment only (clear 'I/we will X by Y')", few-shot examples (incl. RU/EN mixed and negative/implicit cases), chain-of-thought or structured output enforcement, mandatory evidence_quote field.

Long transcripts: topic-aware chunking (embedding cosine similarity on speaker turns outperforms linear/time splits) + map-reduce (per-chunk extraction → dedup/aggregate final pass) or recursive summarization with action-item enrichment. Full long-context (Claude 200k+) works but chunking + reduce remains more controllable and cheaper for high volume.

Speaker mapping (mic-vs-speaker → names): Feed calendar participant list + roles/emails + transcript context (self-intros, "as the investor…") into a dedicated LLM step.

RU/EN mixed: Modern async STT (AssemblyAI Universal, Gladia) handles code-switching in one pass. LLMs process mixed input natively; prompt to "preserve original phrasing and intent, translate only for clarity in output if needed."

### 2. Cross-call analytics
Classic NLP: Embeddings (local or Voyage) of full transcripts or key segments → vector DB → HDBSCAN/UMAP clustering (BERTopic-style) for topics, objections, pitch variants. Easy aggregates: talk ratio (from diarization timestamps), sentiment trends, entity frequency.

LLM map-reduce: Map each call to structured "objections raised + handling + outcome", "winning pitch elements", "commitment status". Reduce across corpus/segments (by stage, rep, outcome) for taxonomies, frequencies, patterns.

Clone from Gong: Talk ratio (high coaching signal), objection/competitor/buying-signal trackers, deal-progression/risk signals, rep performance. Promise-completion rates and cross-call alpha (language predictive of follow-through/close) high-value and feasible in-house. Fathom lighter (per-call + CRM push); Gong leads on multi-call intelligence.

### 3. Commitment/promise tracking
Detect explicit commitments only (high precision). SQLite: commitments table with call_id, lead_id, owner, task, due_date, evidence_quote, status (open/completed/verified), last_checked. Nightly LLM scan of new transcripts for mentions of prior open items ("as discussed, we sent…"), or CRM task updates/webhooks. Grounding with quote + segment ID = single most effective anti-hallucination control. Multi-stage (cheap detector → strong judge) further reduces false positives.

### 4. Failure modes
- ASR errors compounding → high-quality STT + LLM post-correction on critical fields + confidence flags + human audit on high-stakes.
- Speaker misattribution → calendar metadata + dedicated mapping LLM step + confidence scores + review queue.
- Hallucinated attributions → mandatory evidence_quote + LLM-as-judge verification against source snippet + explicit-only rules + few-shot negatives.
- Summary drift / over-extraction → strict schema, dedup across chunks, relevance filter to deal stage, confidence threshold.
- Context loss in chunking / multilingual confusion → overlapping chunks or topic segmentation, async STT, targeted prompts.
- Cost/quality drift from big-model over-reliance → model ladder + caching + periodic eval.

### 5. Cost/architecture
Nightly batch ideal for this volume. Model ladder (rules/cheap model or embeddings for candidates → small/fast LLM for bulk → Sonnet/Opus for judgment, cross-call synthesis, alpha). Cache embeddings/summaries/vector indices. Fine-tune/distill small local models for repetitive high-volume tasks (objection classification, commitment detection) — often beats pure API on cost/speed/privacy once tuned. Local GPU shines for embeddings + Whisper fallback. Streaming unnecessary unless real-time coaching added.

## C. Contradictions & Debates
- Chunking vs full long-context: long-context reduces fragmentation but increases cost/latency/hallucination; hierarchical/topic-segmented map-reduce often wins on coherence/cost/control.
- Explicit-only vs implicit commitments: strict explicit = high precision + trust (our priority) but misses nuance; broader = more recall + noise. Most production biases explicit + grounding.
- Per-call depth vs cross-call breadth: start high-quality grounded per-call (feeds CRM immediately); layer analytics. Over-investing cross-call before per-call is reliable wastes effort.
- API vs local/fine-tuned: frontier APIs win complex reasoning/structured output; local/small win cost/privacy/latency/volume once domain-tuned. Hybrid ladder = pragmatic 2026 consensus.
- Generic summarization vs action-item/decision-focused: action-item pipelines produce more useful CRM/follow-up outputs even if ROUGE sometimes lower.

## D. Best Practices
Schemas: explicit JSON with mandatory evidence grounding every field; version schemas + prompts. Processing: topic-aware embedding chunking + parallel per-chunk extraction + final aggregation/dedup; overlap chunks for cross-turn commitments. Speaker mapping: dedicated LLM step + calendar participants + transcript cues; log confidence + fallback labels. Multilingual: async STT handling code-switching; native-handling prompts. Cross-call: vector DB + BERTopic clustering + periodic LLM map-reduce; track talk ratio / objection freq / promise completion % by stage/rep/outcome. Commitment tracking: SQLite + evidence quotes + status workflow; nightly/trigger verification. QC: multi-stage (detector → extractor → judge with source text); LLM-as-judge for faithfulness; periodic human sampling; "only extract if directly stated + quote it." Evaluation: beyond ROUGE/BERTScore — grounded factuality (MESA-style / custom judge on hallucination/omission/attribution error); track promise completion rate as business KPI. Architecture: nightly batch; model ladder + caching; vector store (local or sqlite-vec/PGVector); tight integration to Obsidian + SQLite CRM (commitments table linked to promise/follow-up cards).

## F. Strategic Recommendations
1. Prioritize grounded per-call extraction as foundation — solves "distrust summaries", feeds everything.
2. Clone selectively from Gong: talk ratio, objection taxonomy, promise tracking/completion, basic deal-progression signals. Skip heavy forecasting until outcome labels exist.
3. Lightweight cross-call layer: embeddings + clustering for patterns; LLM map-reduce for synthesis.
4. Commitment system in SQLite: link leads/calls, require evidence quotes, status updates from follow-ups.
5. Hybrid model strategy: keep/expand cheap-detector → LLM-judge ladder; add local/open models for bulk/embedding + eventual fine-tune.
6. Privacy: local vector store + optional local inference for sensitive VC calls (Meetily-style).
7. Evaluation & iteration: instrument promise completion rate, extraction precision (sampling/judge), pattern usefulness. Prompts/schemas = versioned code.

## G. Action Plan
- Phase 1 (1-2 wk): v1 schema (action_items + decisions + facts + participants_mapped, all with evidence_quote); speaker-mapping LLM step from calendar; nightly extraction job (map-reduce/hierarchical) → JSON + Obsidian + SQLite; basic talk-ratio; test 20-30 recent RU/EN calls.
- Phase 2 (3-6 wk): vector embeddings (call + key segments) + clustering (BERTopic/HDBSCAN); cross-call aggregates + first LLM map-reduce for objection/pitch patterns; commitments table + status workflow; cheap-detector for candidate commitments + strong-judge verification.
- Phase 3 (2-3 mo): full commitment tracking (nightly scan for updates; CRM/promise-card integration); deeper pattern mining + alpha experiments (language features vs follow-through/outcome); eval harness (LLM judge + sampling) + dashboard; optional fine-tune/distill small model.
- Phase 4 (ongoing): more alpha signals, automated follow-up drafting, real-time elements; continuous prompt/schema versioning + model ladder tuning.

## H. Sources
81 sources. Academic: QMSum (Zhong et al., NAACL 2021), AMI/ICSI, Action-Item-Driven Summarization (arXiv 2312.17581). LLM pipelines/prompts: AssemblyAI, Brass Transcripts, AWS Nova, Instructor cookbook. Industry: Gong, Fathom, Chorus, Fireflies (official + 2025-2026 comparisons). Diarization/mapping: Recall.ai, Gladia, Granola docs. Open-source/local: Meetily (GitHub), LangChain/LlamaIndex meeting examples. Hallucination/grounding/eval: MESA, FRAME/SCOPE, LLM-as-judge. Cross-call/embeddings: BERTopic docs; long-doc map-reduce patterns.
