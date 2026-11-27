# DR26-11-19-MACSTUDIOM4-05-1621 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chat.mistral.ai (Vibe Work, Think + skill deep-research)
url: https://chat.mistral.ai/work/242ab839-fbe2-4520-a545-fb323fc29795
dr_id: DR26-11-19-MACSTUDIOM4-05-1621
topic: ElevenLabs catalog, pricing, grants, use cases
date: 2026-11-20

note: канва на момент забора содержала только разделы A и B (каталог), разделы C-I не сняты; таблица каталога передана плоским текстом с разделителем " | "
---

ElevenLabs as of November 2026 — Full Catalog, Pricing, Programs and Use Cases for Palo Alto AI Research Lab

Research question. What does ElevenLabs sell today, what does it cost, whose free money and hiring doors exist, and which concrete uses serve a 2-person AI research lab (operator + AI co-founder + coding-agent fleet, near-zero cash, STT-only user) within 90 days?

Read date. All official pages were opened on 20 November 2026 unless a page carries its own date. Official pricing pages were read during the active Black Friday promo (first month $1, ends Dec 1, 2026).

A. Executive Summary

ElevenLabs is no longer a voice vendor: it is a three-platform company (ElevenAgents, ElevenCreative, ElevenAPI) at ~$22B valuation and $500M+ ARR, with 129 open roles and an FDE hiring motion that matches the operator's profile. It raised a $500M Series D at $11B in Feb 2026 and revalued at $22B in Sep 2026. [established]

The pricing contradiction is resolved from the official page: Flash/Turbo TTS costs $0.04 per 1K characters today; all higher-quality models (v4, v3, Multilingual v2) cost $0.08/1K. The $0.15–0.30/1K figure your vendor leg quoted matches pre-2025 list prices and is obsolete. [established]

Scribe v3/v3 Live (Nov 2026) is the best STT ElevenLabs has shipped (27% fewer WER errors than v2, 2nd place on Artificial Analysis at 1.9% AA-WER) and wins on Russian: Scribe v2 already led independent Russian read-speech (2.86% WER) and spontaneous-speech (5.64%) benchmarks. Your voice rail should flip to Scribe v3 primary + local Whisper secondary, dropping the $9/mo OpenAI leg → ~$5/mo total. [established]

ElevenAgents is now genuinely deployable for a 1-person team: WhatsApp (messages and inbound/outbound calls), Twilio/SIP telephony, batch→Campaign outbound, tool calling, $0.08/min platform fee, no ElevenLabs telephony fee, LLM of your choice (Claude, GPT-5.x, Gemini 3.x). 15 free minutes to prototype. [established]

Biggest free money: the Startup Grant — 33M credits / 12 months (~$5,500+ value), <25 employees, business email, monetization story; rolling review, ~1-week decision; 4,000+ already awarded. Ambassadors: both tracks (Content Creator and Community Builder) are live again — your note that Community Builder was paused is outdated. [established]

Affordable voice products now exist at every price point, but Dubbing v2 is a premium outlier: $2.20/min vs Dubbing v1 $0.33/min (or ~2,000 plan-credits/min with watermark). [established]

Affiliate: up to 22% recurring for 12 months via PartnerStack — the only program that pays cash without a product. [established]

Risks are real but bounded: self-serve data is used for training by default (opt-out, forward-only); Zero Retention is Enterprise-only; a string of Nov 2026 incidents (Scribe v3 Live outage, choppy v4 Turbo EU calls, Voice Library degradation) shows operational growing pains; EU AI Act Art. 50 disclosure duties apply to your calls and cloned voice output since Aug 2, 2026. [established]

The two artifacts most likely to be noticed by ElevenLabs staff or frontier-lab engineers ship in under 2 weeks: an independent Russian/code-switching STT benchmark (prior art exists but no head-to-head with Scribe v3 + local Whisper + Deepgram in the Telegram-dictation domain) and a WhatsApp→coding-agent-fleet voice gateway template. [emerging]

Single recommendation: apply for the Startup Grant today (zero cost, 1-week decision), and run the voice rail + interview-practice agent on the grant. Confidence [established]; main assumption: a lab with a monetizable product idea counts as a "startup" (not an "agency/consulting firm", which are explicitly excluded).

Methodology

Official sources first: pricing pages (3), blog/changelog, docs (models, TCPA, WhatsApp, batch calls, ZRM, voice cloning), grants, affiliates, ambassador, payouts, music terms, ToS, careers, status page, GitHub org. Opened 20 Nov 2026.

Independent cross-checks: NCSpeech Russian/Kazakh ASR benchmark (Oct 2026), Artificial Analysis STT leaderboard, CompanyScope compliance review, third-party cost guides for Deepgram/AssemblyAI/OpenAI rates.

~39 URLs opened and cited; claims seen only in search previews are flagged [single-source] or [low-authority]. Computed cost profiles are labeled "computed" and state their assumptions.

Limitations: no hands-on testing; user-complaint claims are from review aggregators (moderate credibility); ElevenLabs per-position hiring bars inferred from job pages; the Scribe v3 launch post carries no visible date in extraction (published between the Oct 13, 2026 "similar article" and mid-Nov 2026; third-party coverage dates it to early Nov 2026 [single-source]).

Excluded competitors not priced from opened pages (Vapi, Retell, Bland, Hume, Cartesia rates) are named qualitatively only; their per-minute fees are not quoted without a source.

B. Product and API Catalog

Product | What it does | Models / versions | Price unit (official) | Russian quality | Key limits | Source

Speech to Text (Scribe) | Batch + realtime transcription, diarization to 32 speakers, word timestamps, audio-event tagging, 1,000-term keyterm prompting, entity detection (65 types) + redaction, natural-language transcript editing | Scribe v3, Scribe v3 Live (new); Scribe v2, v2 Realtime, v2 Medical, v3 Live Medical | $0.20/hour (batch), $0.22/hour (Live); plan pool: 330 credits/min | Scribe v2: best avg on RU read speech 2.86% WER and spontaneous 5.64% (NCSpeech); FLEURS ru 2.26%; Scribe v1 3.1% vs Whisper large-v3 5.7% (vendor table) | 99 languages (marketing); up to 32 speakers (v3); file ≤ 300MB (web) | API pricing, Scribe v3 blog, Models docs

Text to Speech | Emotive narration, audio tags ([laughs], [said angrily]), multi-speaker lines, IPA phonemes | Eleven v4 / v4 Turbo (flagship, Sep 28, 2026), v3, v3 Conversational, Multilingual v2, Flash v2.5 | v4/v3/Multilingual v2 $0.08 per 1K chars; v4 Turbo/v3 Conversational/Flash/Turbo $0.04 per 1K; plans: 1 credit/char (Flash/Turbo 0.5–1 API) | v4: 90+ languages incl. Russian; #1 Artificial Analysis TTS rank, ~75% blind listener preference (vendor-reported) | v4 char limit 10,000; v3 5,000; Flash 40,000; Free: 10k credits ≈ 10 min, non-commercial | API pricing, v4 blog, Plans

Instant Voice Cloning (IVC) | Clone from ~minutes of audio; consent confirmation required | Voice Lab | Starter plan+ ($6/mo), commercial license | Works across v4 languages | Full commercial rights need Starter+ | Plans

Professional Voice Cloning (PVC) | Fine-tuned clone; own voice only, voice-captcha verification; cannot clone even a consenting third party | v4 clone-strength supported | Creator plan+ ($22/mo), 1+ slot | 30 min audio minimum, 1–3h recommended; spoken voice only (no singing); clone samples in RU + EN advised | Verification retry after 24h | PVC docs, Help consent rule

Voice Design | Prompt → 3 candidate voices per generation | Voice Design v3 | Charged per generation, billed on preview-text characters | RU targetable via prompt | — | Help

Voice Changer (speech-to-speech) | Re-voice a recording | 17,000+ library voices | $0.12/min; 1,000 credits/min in plan pool | 70+ languages | — | API pricing

Voice Isolator | Strip background noise/reverb | — | $0.12/min; 1,000 credits/min | Files ≤500MB / 1h | — | API pricing

Conversational AI (ElevenAgents) | Phone (Twilio/SIP), WhatsApp messages AND calls (in+out), web widget, in-app, CCaaS; tools/MCP; knowledge bases; RAG; workflows/batch outbound; Scribe medical variants | Eleven Realtime v1 Mini/Max (STS, OpenAI-Realtime-compatible WebSocket), Speech Engine pipeline | $0.08/min platform fee + LLM at cost + telephony at cost (no ElevenLabs fee); burst $0.16/min; SMS $0.003 | 90+ languages; v1 Max: 98.8% BigBenchAudio, 570ms time-to-first-audio; Mini: 75.9%, 620ms | Concurrency 4 (Free) → 40 (Business); ZRM not available on batch calls | Agents pricing, Realtime blog, WhatsApp docs, Batch→Campaigns docs

Dubbing | Localize audio/video, speaker detection, sync-timing; v2 preserves emotion of the original performance | Dubbing v2 (May 28, 2026, 92 langs); Dubbing v1 (29 langs); Dubbing Studio; API "coming soon" at v2 launch | v2 $2.20/min; v1 $0.33/min; plan credits: 2,000/min auto (watermark), 3,000 (no watermark), 5,000/10,000 Studio | RU↔EN supported both directions (92 langs) | ≤180 min video (reviewer-reported [single-source]); v2 output watermarked on Free | Dubbing v2 blog, API pricing, Plans

Studio / Projects / Productions | ElevenCreative long-form editor (3 projects Free, 20 Starter); Productions = human-managed dubbing | GenFM podcast presets | via credits | — | — | Plans

GenFM / ElevenReader | Personal AI podcasts from PDFs/links; Reader app (iOS/Android, 32 languages) | — | Free app | 32 narrating languages | GenFM stays where it is for you | GenFM/Reader blog

Audio Native | Embeddable audio player for long reads | — | Not re-verified on Nov 2026 pages — treat as unchanged/legacy [no current official page opened] | — | — | —

Sound Effects | Text→SFX, royalty-free | SFX v3 | Plans: 200 credits/generation; API page lists $0.12/min "Audio Generation" (unit inconsistency, both official) | — | MPS 44.1k/WAV 48k | Plans, API pricing

Eleven Music | Text→full songs ≤5 min; marketplace; UMG partnership (Sep 10, 2026) | Music v2 (May 26, 2026), Music 2.5 endpoints | $0.15/min; 900 credits/min; commercial use Starter+ | Lyrics in major languages; RU prompt support untested here | No artist names/song titles in prompts (terms); self-serve excludes film/TV/Studio-Game commercial use [single-source, terms table not fully captured] | API pricing, Music terms (Oct 9, 2026)

Image & Video | Gen image/video inside ElevenCreative (launched Nov 17, 2025) | — | via credits; Starter tier+ full access | — | — | Blog index, Plans

Voice Library & Payouts | Share PVC; earn per 1K chars used by paid users (default ≈ $0.03/1K chars, weekly payouts via Stripe, $10 minimum); $22M paid to creators by May 22, 2026 | — | revenue-positive program for you | RU voice = niche | only PVC, verified own voice | Payouts, Monetize blog, Blog index

Iconic Marketplace | Licensed celebrity/character voices (Caine, Stan Lee, Hasbro) with commercial rights via request workflow | — | case-by-case license | — | — |

## Links from canvas anchors
https://elevenlabs.io/pricing/api
https://elevenlabs.io/blog/scribe-v3
https://elevenlabs.io/docs/overview/models
https://elevenlabs.io/blog/eleven-v4
https://elevenlabs.io/pricing
https://elevenlabs.io/docs/eleven-creative/voices/voice-cloning/professional-voice-cloning
https://help.elevenlabs.io/hc/en-us/articles/36842751624209-Can-I-create-a-Professional-Voice-Clone-of-someone-else-s-voice
https://help.elevenlabs.io/hc/en-us/articles/29315418701073-How-much-does-Voice-Design-cost
https://elevenlabs.io/pricing/agents
https://elevenlabs.io/blog/realtime
https://elevenlabs.io/docs/eleven-agents/whatsapp
https://elevenlabs.io/docs/eleven-agents/phone-numbers/batch-calls
https://elevenlabs.io/blog/introducing-dubbing-v2
https://elevenlabs.io/blog/genfm-on-elevenreader
https://elevenlabs.io/music-terms
https://elevenlabs.io/blog
https://elevenlabs.io/payouts
https://elevenlabs.io/blog/monetize-your-voice-with-elevenlabs-voice-library-and-create-passive-income