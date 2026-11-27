# DR26-11-19-MACSTUDIOM4-05-1621 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (Expert + skill deep-research; Heavy недоступен, paywall)
url: https://приватный чат
dr_id: DR26-11-19-MACSTUDIOM4-05-1621
topic: ElevenLabs catalog, pricing, grants, use cases
date: 2026-11-20

---

A. Executive Summary

ElevenLabs (as of 20 Nov 2026) is a mature, credit-based voice platform with strong TTS (v3/v4, Flash/Turbo ~$0.04–0.08/1k chars), Scribe STT (v3 batch $0.20/hr, Live $0.22/hr; 90–102 languages including Russian), Instant/Professional Voice Cloning, Conversational Agents (~$0.08/min), Dubbing, Music/SFX, Studio, and an official n8n node + hosted MCP. Black Friday (to 1 Dec 2026) cuts Creator/Pro annual up to 55%. Startup Grants offer 33M characters / 12 months (~$5,500+ value) for eligible early-stage products. Russian quality is production-usable (supported natively; independent MOS ~4.0 and competitive WER) but not always best-in-class versus local Whisper large-v3 or specialized engines; diarization and low-latency Live are strengths. For a near-zero-cash Palo Alto lab already dual-running Scribe + Whisper on ~25h/month Russian dictation, the highest-ROI moves are (1) keep dual STT but shift primary to local + Scribe v3 for hard cases, (2) apply for Startup Grant + Content Creator ambassador track today, and (3) ship a small open-source Russian STT/TTS or agent template that can be noticed. Full commercial rights require paid plans; training is on-by-default for self-serve (opt-out available); Zero Retention is Enterprise/API-focused.

B. Product and API catalog table

Product What it does Model(s) Price unit (API / credits) Russian quality Limits / notes Source
Speech-to-Text (Scribe) Batch & realtime transcription, diarization (≤32 speakers), word timestamps, audio events, keyterms, entity detection, mixed-language Scribe v3 (batch), v3 Live, v2/Medical variants $0.20/hr batch, $0.22/hr Live (+keyterm/editing add-ons); credits in UI (~330/min) Excellent claimed (≤5% WER on v2 for RU); v3 27% fewer errors vs v2 on FLEURS (102 langs). Independent: competitive ~6.5% mean / strong on some sets; not always #1 vs Gemini/Whisper 3 GB / 10h files; concurrency by plan; free ~4.5h batch elevenlabs.io/pricing/api, /docs, blog/scribe-v3 (Nov 2026); verli.app, BenchLM
Text-to-Speech High-quality / low-latency speech, multi-speaker dialogue, tags Eleven v4 / v4 Turbo, v3 / Conversational, Multilingual v2, Flash v2.5 $0.04/1k chars (Flash/Turbo/Conversational), $0.08/1k (v3/v4/Multilingual); ~150 chars/min est. Native support; MOS ~4.0 independent (production-ready with native voices / PVC); stress/numbers need care; strong emotion on v3 Char limits 5k–40k by model; commercial on paid plans elevenlabs.io/pricing/api, /docs/overview/models
Instant Voice Cloning (IVC) Fast clone from short sample N/A Included on Starter+ Good likeness; accent issues possible on cross-lingual ~10s–2 min audio; consent required docs, help.elevenlabs.io
Professional Voice Cloning (PVC) Fine-tuned high-fidelity clone Trained on Multilingual/Flash etc. Creator+ (1+ slots) High fidelity; RU supported; best with native RU samples for RU output; cross-lingual carries accent 30–180 min clean audio; verification; 3–6h training docs, help
Voice Design / Changer / Isolator Generate or transform voices; isolate Various Credits (~1000/min changer) Good Plan-dependent pricing pages
Conversational AI / Agents Voice agents, telephony (Twilio/SIP/WhatsApp), tools/MCP, KB, batch outbound, widget Eleven Realtime Mini/Max + Scribe + chosen LLM ~$0.08/min (+burst $0.16; LLM separate); minutes included by plan (Free 15, Starter 75, Creator 275, Pro 1,238+) Multilingual 90+; RU viable Concurrency 4–40 by plan; TCPA docs available pricing/agents, docs
Dubbing / Dubbing Studio Automated + editable localization, voice clone, lip-sync (some) Dubbing v1/v2 ~$0.33–0.50/min (v1), ~$2.20/min (v2); minutes by plan RU↔EN supported; quality variable Video support; Studio editing pricing, dubbing pages
Studio / Projects / GenFM / Audio Native / ElevenReader Long-form, podcasts, audio embeds, reader Various Credits Usable Project limits by plan pricing
Sound Effects / Music (Eleven Music) Generative SFX & music SFX v3, Music v2/v2.5 Credits (~200/SFX gen, 900/min music); commercial on paid Instrumental/vocals; licensing for commercial Commercial rights on Starter+ pricing
Voice Library / Payouts / Iconic voices Shared voices, marketplace N/A Earn via shares RU voices available — site
Forced Alignment, other Alignment, Image/Video (limited), Reception.ai Various Credits / separate — ZRM not on all docs
Integrations Official n8n node (TTS/STT/Agents), hosted MCP (api.elevenlabs.io/v1/mcp), Python/JS/etc SDKs, Zapier/Make, Claude/ChatGPT — — — Local MCP archived Aug 2026 in favor of hosted. n8n official GitHub elevenlabs, blog

C. Pricing table and three cost profiles

Plans (monthly, Nov 2026; Black Friday annual discounts to 1 Dec on Creator/Pro): Free $0 / 10k credits; Starter $6 (first $1) / 30k; Creator $22 (first $11) / 121k; Pro $99 / 600k; Scale $299 / 1.8M; Business $990 / 6M; Enterprise custom. Credits shared; rollover limited. Commercial rights from Starter+. API PAYG available. STT effectively $0.20–0.22/hr outside pure credits. Agents $0.08/min overage.

(i) STT only, 25 h/month Russian: Starter $6/mo (includes ~30 h batch Scribe v3) or pure PAYG ~$5. Free insufficient (~4.5 h). Local Whisper large-v3 remains $0 alternative.
(ii) STT 25 h + 60 min cloned-voice TTS + 60 min dubbing: Creator ~$22 (PVC + higher allowances) or Starter + overage. TTS Flash ~$0.4–1; dubbing v1 watermark ~$20–30 or higher for clean. Total roughly $25–50 depending on exact model/watermark.
(iii) Voice agent 300 call-min/month: Creator $22 (275 included + ~$2 overage) or Pro for headroom. Telephony (Twilio etc.) extra.

D. Grants, programs, hiring and open-source doors

Startup Grants: 33M characters / 12 months (~680 h Agents, >$5,500 value) for startups <25 employees with monetized long-term product (not agencies/SMBs/kids). Apply via elevenlabs.io/startup-grants; rolling; acceptance within ~1 week if selected. Paying self-serve eligible; no resale.
Ambassador: Content Creator (tutorials/demos) and Community Builder (events) tracks live at elevenlabs.io/ambassador; monthly credits, early access, merch, team line. Community Builder previously noted paused in some contexts; Content Creator form active. Affiliate: 22% first 12 months (11% Business) via PartnerStack.
Hiring: Multiple remote engineering (full-stack, AI/ML, security, DevRel/docs, Forward Deployed) open; bar is production voice/agent experience. Partner Program launched Nov 2026 with certifications and GTM.
OSS: Official Python/JS/etc SDKs (MIT), n8n community node (active), examples, Unity. Local MCP archived Aug 2026 in favor of hosted. Meaningful PRs possible on SDKs, n8n node, agent examples, or independent benchmarks.

E. Use case table

Use case Mechanism Products Value in 90 days Monthly cost Needed to start Best alternative Verdict
a. Voice rail (dual STT, live, diarization) Parallel Scribe v3 + local Whisper; Scribe Live for dictation; diarization on calls/Plaud Scribe v3/Live High (accuracy/latency on RU notes) $0–6 Existing n8n + API key Local Whisper large-v3 (free, competitive RU WER); Groq/Deepgram Tie / ElevenLabs slight edge on diarization & Live; keep dual
b. Operator voice clone (RU native + EN accent) IVC for quick; PVC for fidelity; use for EN/RU narration, intros, replies IVC (Starter+), PVC (Creator+) Medium–High (content + applications) $6–22 + char cost Clean 30–180 min samples + verification XTTS/F5-TTS/Kokoro/Chatterbox local; Cartesia/HeyGen ElevenLabs wins on quality/ease; local for zero-cost tests
c. Voice agent (lead qual, self-robocall, receptionist, mock interviews, “call agents”) Agents + telephony/WhatsApp + tools/MCP to fleet Agents + Scribe + TTS Medium (leads/practice) $22–99 for 300 min Twilio/SIP + prompt + KB Retell/Vapi/Bland + OpenAI Realtime/Gemini Live; local Tie; ElevenLabs integrated but latency/concurrency costs; test carefully
d. Dubbing podcasts/YouTube RU↔EN Auto + Studio edit Dubbing v1/v2 Low–Medium (distribution) $20–100+ Source audio; ToS check Rask/HeyGen/Descript; local pipelines Loses on cost vs value for NotebookLM sources (legal/ToS caution)
e. Content factory (shorts, Audio Native, SFX/music, audiobooks) TTS narration + SFX/Music TTS + SFX + Music + Studio Medium (visibility) $6–22 Scripts + plan NotebookLM (already used), local TTS, Descript Tie / ElevenLabs for clone consistency; keep NotebookLM for listening
f. Hiring/interviews (transcribe, mock agents, voice intro) Scribe + Agents + clone Scribe + Agents + PVC High (100 interviews goal) Low–$22 Recordings Whisper + custom prompts; Vapi ElevenLabs wins on integrated mock agent
g. Shipable artifacts (<2 weeks) Russian STT/TTS benchmark repo; n8n agent template; MCP wrapper for coding fleet; independent RU WER leaderboard API + open data High (visibility to EL staff / labs) $0–6 GitHub + existing data Prior art: some RU STT benches exist (e.g. Smolevich); official MCP/n8n exist Prior art partial; independent RU benchmark + fleet MCP still open

F. Contradictions and debates

Flash pricing: Internal note of $0.04/1k vs vendor $0.15–0.30 resolved — official API is now ~$0.04/1k for Flash/Turbo (post May 2026 cuts).
Russian quality: Vendor claims excellent/≤5% WER and 27% improvement; independent tests show solid but not dominant (MOS ~4.0, WER competitive with Gemini/Whisper/Soniox, variable by domain/noise). Local large-v3 often sufficient and free.
Credit vs pure API billing: STT/Agents often priced outside pure credit pool on API; UI uses credits heavily. Confirm on your dashboard.

G. Failure modes and risks

Training on customer audio is default for self-serve (opt-out in account settings; prospective only); Enterprise off-by-default. Zero Retention Mode is Enterprise/API-eligible for TTS/STT/Agents (not UI, not voice clones); data residency EU/India/SG available higher tiers. Voice-clone requires consent/verification; abuse bans possible. EU AI Act Art. 50 synthetic voice disclosure applies; US TCPA treats AI voice as artificial (consent rules for outbound); Portugal/EU recording consent needed. Billing surprises and AI-only support common complaints (Trustpilot ~2.9/5). Price cuts in 2026 but history of changes; lock-in via credits/clones. Reliability incidents exist but not catastrophic in recent coverage. n8n/MCP integrations solid but local MCP archived.

H. Top 3 moves for us

Apply for Startup Grant + Content Creator ambassador today (zero cost): Frame as applied AI lab building agent fleets + evidence gates with voice rail/digital twin. First step: submit form at elevenlabs.io/startup-grants and /ambassador.
Optimize voice rail + ship independent Russian benchmark (zero–$6): Keep dual STT; primary local Whisper large-v3, Scribe v3 for hard/diarized cases. Publish open RU WER/latency bench on real voice notes. First step: clone a public STT-bench repo and add Scribe v3 calls.
Prototype PVC + mock-interview agent (Creator if grant fails): Record clean RU samples for PVC; build simple hiring-manager mock agent. First step: test IVC on free/Starter today with 1–2 min sample.

Single recommendation: Prioritize the grant + ambassador application and a public Russian STT/agent artifact. This maximizes free credits/visibility toward hiring and distribution with minimal cash risk. Confidence: [established] on pricing/catalog; [emerging] on exact Russian WER rankings and grant acceptance rates. Main assumption: lab can articulate a monetizable voice/agent product story for the grant.

I. Sources (all opened or directly extracted)

elevenlabs.io/pricing (and /api, /agents) — Nov 2026
elevenlabs.io/docs/overview/models and speech-to-text, zero-retention, agents privacy/TCPA — Nov 2026
elevenlabs.io/blog/scribe-v3 and pricing updates — Nov 2026
elevenlabs.io/startup-grants and /ambassador — Nov 2026
github.com/orgs/elevenlabs/repositories and specific n8n/MCP — Nov 2026
Independent: BenchLM, Artificial Analysis AA-WER, verli.app, Smolevich/stt-benchmarks, ThePlanetTools, Habr/Russian forums, Trustpilot summaries — 2026
Others: company blogs, help center, PartnerStack summaries.

All claims above are tied to these; gaps (exact current grant acceptance %, live RU WER on your domain) marked as such.

115 sources

## Links from page anchors

(main-scoped query returned empty; document-wide query, excluding grok.com, returned:)

- https://elevenlabs.io/pricing
- https://elevenlabs.io/docs/overview/models
- https://elevenlabs.io/blog/scribe-v3
- https://elevenlabs.io/startup-grants
- https://github.com/orgs/elevenlabs/repositories
- https://x.ai/legal/privacy-policy
- https://www.onetrust.com/solutions/consent-and-preferences/
