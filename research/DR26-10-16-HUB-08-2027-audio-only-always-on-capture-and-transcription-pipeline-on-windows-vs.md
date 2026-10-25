---
dr_id: DR26-10-16-HUB-08-2027
title: "Audio-only always-on capture and transcription pipeline on Windows (vs Screenpipe)"
date: 2026-10-16
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-16-HUB-08-2027): Audio-only always-on capture and transcription pipeline on Windows (vs Screenpipe)

> The report is a research brief, not findings. It asks whether always-on audio-only capture, ASR and diarization on Windows is best done with Screenpipe set to audio-only, a mature standalone open-source product, or a minimal modular recorder with a durable queue.

## Ключевые выводы
- The supplied text is only the research prompt (task brief). It contains no vendor findings, source citations, benchmarks or measurements, so no conclusions about Screenpipe, alternatives, ASR or diarization can be drawn from it.
- Only one of 6 vendor legs (chatgpt, claudeai, gemini, grok, glm, mistral) had a body on disk at reconciliation (2026-10-18). The registry had wrongly said 'running'. The text provided here carries no results from it.
- Scope required by the brief: audio only with no screen recording or OCR, microphone plus system loopback, automatic RU/EN transcription and speaker diarization, on Windows with macOS portability.
- Three options are to be compared: (A) Screenpipe configured truly audio-only, (B) mature standalone open-source audio capture/transcription products, (C) a minimal modular recorder plus durable queue plus ASR plus diarization. The brief prefers reuse and a simple repairable design.
- Local incident notes cited in the brief (not externally verified) are: silent recording stops at disk pressure while generic health stayed green, loopback/device failures, misleading hybrid-Parquet index text, RU/EN hallucinations on silence, and microphone selection not persisting.
- The brief says turning video off must not be assumed to fix audio reliability. Whether a screen-permission denial, OCR failure, disk threshold or shared supervisor can stop audio in Screenpipe is to be checked against pinned upstream source.
- Required pipeline properties: an immutable original chunk, a manifest (UTC time, device, channel, sample rate, hash), atomic writes, a durable queue with checkpoints, idempotent processing, backpressure, reprocessing, model/config provenance and a reversible correction layer.
- Required monitoring: prove fresh nonempty samples per source and successful transcript output, separately from process health. A 24-hour canary is requested with speech markers, silence, mixed RU/EN, overlap, device unplug, restart and simulated disk pressure.
- Diarization is to be treated as anonymous 'who spoke when' (Speaker A/B), not identity. Downstream summaries, commitments and vault ingestion are unconfirmed hypotheses (optional, pending user clarification).

## Рекомендации / решения
- Treat this note as a placeholder and re-run or re-collect the remaining 5 vendor legs before making any decision.
- Do not change recording configuration or roll out fleet-wide while the research is incomplete.
- When results arrive, check Screenpipe's audio-only path and failure coupling against pinned upstream source (repo, release, commit, file:line), not flag names or marketing text.
- Keep heavy audio data off the OS drive, with no auto-deletion as a prerequisite.
- Run the 24-hour canary in an isolated sandbox, and report WER/DER only where reference data exists.

## Сущности
- **Люди:** Anton
- **Компании:** NVIDIA
- **Продукты/инструменты:** Screenpipe, Whisper, faster-whisper, WhisperX, pyannote, NVIDIA NeMo, WASAPI loopback, Codex

## Открытые вопросы
- Does Screenpipe have a real audio-only path, and can vision, OCR, disk-threshold or supervisor failures stop audio?
- Which 3-5 maintained standalone alternatives capture mic and loopback continuously on Windows, and what are their code and model-weight licenses?
- Which ASR (Whisper variants, NeMo, newer models) is best on mixed RU/EN speech, names, numbers, noise and silence, and what are its latency and resource costs?
- How should cross-chunk speaker consistency, overlap and mic/loopback echo duplicates be handled?
- What is the exact CPU/RAM/GPU/disk/latency cost of each option? All are unknown from this text.
- What downstream use does Anton actually want after diarization: summaries, commitments, or vault ingestion?

## Источник
- DR-ID `DR26-10-16-HUB-08-2027` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»

## Связано
- Screenpipe audio-only mode
- WASAPI loopback capture
- speaker diarization
- RU/EN ASR hallucination on silence
- durable queue and idempotent processing
- capture-path liveness monitoring vs process health
- dr_reconcile partial vendor legs
- knowledge-base ingestion


## Raw model reports

1 reports delivered, synthesis used 1 of 6. [DR26-10-16-HUB-08-2027-audio-only-always-on-capture-and-transcription-pipeline-on-windows-vs/](DR26-10-16-HUB-08-2027-audio-only-always-on-capture-and-transcription-pipeline-on-windows-vs/README.md)
