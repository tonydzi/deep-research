---
dr_id: DR26-06-22-HUB-01
title: "Designing an AI Creator Watcher for practical knowledge capture (AI/coding/PKM creators)"
date: 2026-06-22
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-22-HUB-01): Designing an AI Creator Watcher for practical knowledge capture (AI/coding/PKM creators)

> How to design a system that monitors AI/coding/PKM creators across YouTube, Telegram, RSS and podcasts, and extracts only durable, practical knowledge (not hype) into Obsidian + SQLite with explicit freshness/decay tracking.

## Ключевые выводы
- Two-pass pipeline is the core architecture: pass one is a cheap, deterministic filter (exact/semantic dedup, ad/sponsor stripping, topic gating, actionability pre-score) run before any LLM call; pass two uses Sonnet-class extraction only on survivors, and Opus-class synthesis only for corroborated clusters or weekly digests — this controls cost and prevents expensive models from inventing 'insight' from weak material.
- Store 'knowledge units' (atomic practical claims with source spans, valid_as_of, volatility, half_life_days, applicability_score, alpha_score) as the primary object, not full transcripts or generic notes — notes are rendered from these units.
- Freshness modeled as exponential decay: freshness = exp(-ln2 * age_days / half_life_days); effective_utility = alpha_score * applicability_score * freshness * confidence_factor. Half-lives range from ~30 days (model UI/pricing specifics) to 365-730 days (PKM principles, evergreen methods).
- Alpha score formula routes items automatically: alpha_score = 0.30*actionability + 0.20*novelty + 0.15*specificity + 0.15*evidence + 0.10*relevance_to_you + 0.10*durability − 0.15*promo_penalty − 0.10*newsiness; <0.40 discard/telemetry, 0.40-0.60 human skim, >0.60 auto-draft, >0.75 with ≥2 corroborating sources → evergreen candidate.
- Discovery should use official APIs, not scraping: YouTube Data API via channel→uploads-playlist (cheaper than repeated search.list calls); RSS/Atom via Miniflux or FreshRSS for blogs; podcast RSS + Podcast Index for episodes; Telethon/MTProto for Telegram (more capable than Bot API for history access but carries higher policy/session-security risk).
- Pre-LLM ad/noise cleaning is critical: use SponsorBlock data as a prior for sponsored/self-promo segments, regex heuristics for CTAs/affiliate lines, structural cues (no examples/code/commands = drop), and a 'newsiness_score' gate to separate news-recap filler from transferable practical content.
- Dedup requires three layers: exact (hash/ID/normalized URL), near-duplicate (MinHash/SimHash on cleaned text), and semantic cross-platform (E5 embeddings, creator- and date-aware cosine thresholds ~0.89-0.93) — otherwise the same idea reposted across YouTube/Telegram/blog becomes 3-5 separate notes.
- Curated (not ranked) creator watchlist tiered A/B/C: Tier A core = Simon Willison, Ethan Mollick, Hamel Husain, Nicole van der Hoeven, Nick Milo, kepano, Jeremy Howard, Greg Kamradt, Latent Space, Andrej Karpathy, ArjanCodes, ITBeard, Миша Ларченко; Tier B opportunistic needs stronger filtering; Tier C is discovery-only, not a note source.
- Legal/policy boundary: YouTube ToS bans automated scraping/bulk downloading outside the official API; Telegram's API Terms explicitly forbid using data obtained via the API to train or fine-tune AI/ML systems — public-channel-only ingestion with minimal raw retention is the safe posture.
- Recommended minimal-code stack to avoid framework bloat: Miniflux/FreshRSS + YouTube Data API + yt-dlp only on-demand + Telethon for select channels + faster-whisper as default ASR (WhisperX+pyannote only when source-span/diarization quality matters) + sqlite-vec on existing E5 stack + n8n for scheduling/routing only + Obsidian Git export.

## Рекомендации / решения
- MVP: 12 EN + 5 RU core creators (or ~25-40 sources total), YouTube Data API + RSS + podcast RSS only, no Telegram yet, no Opus synthesis, simple human review queue, SQLite-first with Obsidian markdown export; acceptance criteria include dedup catching obvious reposts and <20% non-actionable drafted notes.
- V1: add Telethon ingestion for a narrow set of Telegram channels, semantic dedup via e5/sqlite-vec, SponsorBlock-assisted cleaning, Sonnet-class extraction, faster-whisper/WhisperX ASR fallback, and a freshness engine.
- V2: cross-source synthesis and contradiction detection, automated refresh of stale notes, creator scoring/dashboards, and weekly 'alpha memo' generation.
- Make staleness a first-class, mandatory field on every extracted unit (valid_as_of, volatility, refresh_after) rather than storing notes as if permanently valid.
- Prioritize the watchlist by tier (A/B/C) instead of monitoring all creators equally; use Tier C only for source discovery, never for direct note creation.
- Guard against the main antipatterns: LLM-first without deterministic filtering, storing full transcripts as notes, skipping cross-platform semantic dedup, overweighting news-heavy channels, and never recalibrating the alpha-score thresholds against real accept/reject history.

## Сущности
- **Люди:** Simon Willison, Ethan Mollick, Hamel Husain, Nicole van der Hoeven, Nick Milo, kepano, Jeremy Howard, Greg Kamradt, Matt Wolfe, Andrej Karpathy, Mckay Wrigley, Sam Witteveen, Cole Medin, Theo Browne, David Ondrej, ThePrimeagen, Jeff Delaney, Миша Ларченко, Таня Румянцева, Александр Горный, Вастрик, Сергей Жданов, Stepan Gershuni, Арсений Кравченко
- **Компании:** ArjanCodes, Latent Space, fast.ai, ITBeard, addmeto/Бобук, Hi, AI!, CyberTopus, Fireship
- **Продукты/инструменты:** YouTube Data API, Telethon, yt-dlp, faster-whisper, WhisperX, pyannote.audio, SponsorBlock, Sentence Transformers, E5 embeddings, sqlite-vec, Miniflux, FreshRSS, n8n, Podcast Index, Podgrab, Haystack, LlamaIndex, Obsidian Git plugin, Obsidian Local REST API / MCP plugin

## Открытые вопросы
- Cadence/activity data for several newer vibe-coding YouTubers and RU-native Telegram sources is weak (marked emerging/speculative) because official surfaces didn't expose clean recent publishing data.
- The alpha-score formula is an operational heuristic, not validated ground truth — needs calibration against Anton's own accept/reject history and the vault's existing redundancy profile.
- Whether an automatic 'practical value' classifier can fully replace manual editorial judgment remains speculative, not established.
- The line between personal note-taking and Telegram's ban on using scraped API data to train/fine-tune AI models is not fully resolved legally — treated as a caution zone, not formal legal advice.

## Источник
- DR-ID `DR26-06-22-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-22-HUB-01-designing-an-ai-creator-watcher-for-practical.md`

## Связано
- [[second-brain-northstar]]
- [[vault-data-architecture]]
- [[model-routing-sonnet-grunt]]
- [[ai-creator-watcher]]
- [[knowledge-decay-freshness]]
- [[alpha-score-formula]]
