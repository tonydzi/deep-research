---
dr_id: DR26-07-06-HUB-05
title: "Building a deterministic daily activity ledger from AI-agent logs, commits, and vault arti"
date: 2026-07-06
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-06-HUB-05): Building a deterministic daily activity ledger from AI-agent logs, commits, and vault artifacts

> Deep research on how to reliably reconstruct 'what was accomplished on day X' from heterogeneous, overlapping, partial signals (transcripts, commits, structured ledgers, registries, retros, mtimes, posts) without depending on any single incomplete source like retrospectives.

## Ключевые выводы
- The right architecture is event-sourcing style: treat every raw signal as an immutable normalized 'evidence event', store append-only, and derive a deterministic daily projection (day-ledger-YYYY-MM-DD.md) from it — never make transcripts, commits, or retros themselves the source of truth.
- Sources should be tiered by reliability, not summed: Tier A = black-box per-turn ledger, dated registries, decision notes, git commits (primary evidence); Tier B = curated retros and authored posts (corroboration/wording); Tier C = classified transcript events and file mtimes (recall/recovery only, never proof).
- Deduplication must happen in three passes: (1) exact identity match via stable evidence_id/native id, (2) deterministic work-unit clustering by hash(artifact paths + action type + local day + time bucket), (3) fuzzy near-duplicate merge (SimHash/MinHash) only for free-text sources like notes/posts/transcript summaries — duplicate evidence should raise confidence, never raise counted volume (avoids double-counting).
- Noise filtering should be a 3-stage cascade: deterministic high-precision reject rules first (zero artifacts, only read/status commands, known automation/heartbeat identities, very short duration, low entropy/repetition) → a cheap classifier (logistic regression/GBT, optionally bootstrapped via weak supervision/Snorkel-style labeling functions) for borderline sessions, optimizing for high recall on 'real work' → LLM judge only on the narrow uncertain residue, given a small structured evidence packet (never raw transcript as primary truth), because LLM-as-judge reliability depends on being grounded in references.
- Day attribution must use event time (not file-arrival/processing time), canonical timezone Europe/Lisbon via IANA zoneinfo (DST-aware), storing both UTC and local timestamps; midnight-spanning sessions should be split by inactivity gaps (~30 min) into sub-work-units assigned to the day containing the majority of write events.
- Git commit dates need explicit policy: use CommitDate as primary (entry into history), AuthorDate as supporting context; flag commits where AuthorDate/CommitDate diverge beyond a threshold (rebases/cherry-picks/amends create false certainty).
- Completeness/confidence must be modeled like stream-processing watermarks: rebuild a rolling window (D-2, D-1, D) each night, mark same-day/previous-day as 'provisional', and only mark a day 'stable' after a 36-48h watermark passes, since evidence (sync, late commits, next-morning retros) can arrive late.
- A day confidence score should combine source coverage (~35pts), evidence quality/Tier-A share (~25pts), cross-source corroboration (~20pts), specificity (~20pts), minus penalties (unresolved uncertain sessions, source failures, midnight ambiguity, author/committer date skew, mtime-only evidence, open watermark) — mapped to Stable/Provisional/Incomplete/Needs-review statuses, with an explicit machine-readable 'gaps' list rather than silent incompleteness.
- Prior art transfers cleanly: changelog automation (Conventional Commits, semantic-release, Release Please) shows structured categories + path scoping + noise exclusion + a final curated layer instead of raw log dumps; audit-log aggregation (Cloud Audit Logs, Datadog) models 'who did what, where, when' via centralized deduplicated events; engineering-intelligence platforms (LinearB, DX, GitClear) and the SPACE framework confirm that no single metric/source (esp. not Git alone) suffices; async standup tools (Range, StandupBot) model the target UX of one concise scannable daily artifact.
- The minimal viable pipeline (AK-47-compatible) is a boring batch stack: config.yaml + ingest.py + SQLite ledger.db + project.py + summarize.py + markdown/json output — LLM used ONLY for the final human-readable narrative summary, generated last from already-fused grounded work units, never from raw logs; outputs are fully regenerated (not appended) each run for idempotency.

## Рекомендации / решения
- Implement an append-only SQLite evidence store (one normalized event schema across all source types) with a deterministic daily projection step, and only invoke an LLM at the very end for prose summarization.
- Build in this order: (1) deterministic skeleton (ingest + exact dedup + timezone normalization + markdown emission with only obvious-noise filters), (2) transcript-derived low-trust evidence extraction + work-unit clustering, (3) confidence/gap/watermark model, (4) cheap classifier once labeled borderline sessions exist, (5) LLM summary layer last.
- Assign explicit reliability tiers to sources (A: blackbox ledger/registries/decisions/commits; B: retros/posts; C: transcripts/mtimes) and fuse via clustering + corroboration bonus, not additive counting.
- Use event-time-based day attribution in Europe/Lisbon (IANA zoneinfo), split midnight-spanning sessions by inactivity gaps, and use CommitDate (not AuthorDate) as the primary git timestamp with a divergence flag.
- Rebuild a rolling 3-day window nightly and only mark a day 'stable' after a 36-48h watermark, emitting explicit per-day and per-work-unit confidence scores plus a machine-readable gaps/status block instead of pretending completeness.
- Optimize the noise classifier for high recall on 'real work' (accept an 'uncertain' bucket) rather than high precision, since the worst failure mode for a personal ledger is a day that silently looks empty when work actually happened.

## Сущности
- **Люди:** —
- **Компании:** AWS, Stripe, GitHub, LinearB, DX, GitClear, Datadog, Microsoft, Range, StandupBot
- **Продукты/инструменты:** Conventional Commits, semantic-release, Release Please, Cloud Audit Logs, SPACE framework, Snorkel, SimHash, MinHash, SQLite, IANA zoneinfo/tz database

## Открытые вопросы
- No concrete numeric thresholds are validated empirically (e.g. exact inactivity-gap minutes, watermark hours, confidence-score weights) — these are proposed defaults needing calibration against Anton's actual data.
- The cheap noise classifier requires a labeled dataset of real-work vs automation sessions that doesn't yet exist — bootstrapping via weak supervision (Snorkel-style) is suggested but unimplemented.
- How exactly to bound/cost the LLM adjudication step for 'uncertain' sessions at scale (token budget, packet size) is not fully specified.
- Interaction with existing per-turn 'black box' TurnState ledger and retro pipeline (how much of this design duplicates vs. extends current infra) is not addressed in this report — needs reconciliation with always-on-memory-pilot and turnstate-ledger.

## Источник
- DR-ID `DR26-07-06-HUB-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- day-ledger-methodology
- always-on-memory-pilot
- turnstate-ledger
- vault-data-architecture
- one-system-propagate
- test-after-build-skill
- ak47-simplicity
- second-brain-northstar
