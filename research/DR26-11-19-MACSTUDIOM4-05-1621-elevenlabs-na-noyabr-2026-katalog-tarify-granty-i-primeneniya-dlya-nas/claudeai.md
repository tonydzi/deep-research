# DR26-11-19-MACSTUDIOM4-05-1621 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: claude.ai (Fable 5.2 Extra, Research)
url: https://приватный чат
dr_id: DR26-11-19-MACSTUDIOM4-05-1621
topic: ElevenLabs catalog, pricing, grants, use cases
date: 2026-11-20

---

ElevenLabs for Palo Alto AI Research Lab: Catalog, Pricing, Free Money and Ranked Use Cases (as of 19 November 2026)

Keep ElevenLabs, but spend almost nothing new on it yet. First, switch the voice rail to Scribe v3, which costs about $5 a month for 25 hours of audio. Second, publish an independent Russian speech-to-text benchmark, because nobody has published one that includes Scribe v3. Third, apply for the free 12-month, 33-million-character Startup Grant before buying any TTS, cloning or agent plan.

TL;DR
Speech-to-text: Scribe v3 launched on 10 Nov 2026 and costs $0.20 per hour through the API. That is about $5.00 a month for your 25 hours of Russian audio, against $9 for the Whisper API (whisper-1, $0.006/min, which Dasha’s pricing guide says is scheduled to shut down on 26 Feb 2027). It is also the best-scoring commercial engine on independent aggregate leaderboards. Russian-specific evidence is thin, but it points the same way. Replace the Whisper API leg, and keep local large-v3 as the free second engine.
Pricing contradiction resolved: Flash/Turbo and v4 Turbo cost $0.04 per 1,000 characters on the official API page, the same on every plan. The “$0.15–0.30” figure is the price of credits on Creative (app) plans for 1-credit-per-character models, not the API Flash price. Profile costs: (i) about $5/month; (ii) about $40/month with Dubbing v1, or about $142 with Dubbing v2; (iii) $24/month for 300 agent minutes, plus the language model (LLM) at $0.70–8 and the carrier billed separately.
Best non-cash wins: the Startup Grant (12 months and 33M characters for companies under 25 employees); an independent Russian benchmark that ElevenLabs staff will notice; and an open-source mock-interview / “call my agents” template on ElevenAgents. Together they make a portfolio for ElevenLabs’ remote Forward Deployed Engineer and DevRel openings. Do not pay to dub NotebookLM audio with ElevenLabs Dubbing. A home-built Scribe → LLM → TTS chain costs about a tenth as much.
A. Executive Summary
ElevenLabs’ product line splits into three subscription families: ElevenCreative (the app, billed in credits), ElevenAgents (billed per call minute) and ElevenAPI (billed in dollars). Most published pricing confusion comes from mixing up credits and API dollars. [established]
The latest flagship releases are Eleven v4 / v4 Turbo text-to-speech (28 Sep 2026) and Scribe v3 / v3 Live speech-to-text (10 Nov 2026).
medium

cellcog

Both lead independent arenas or leaderboards in English and in aggregate.
mixpeek

artificialanalysis

[established]
For Russian there is no independent FLEURS-ru or Common Voice-ru word error rate (WER) for Scribe. Two small community benchmarks (Habr, Feb 2026; one GitHub repo, May 2026) put Scribe first on Russian-heavy audio.
habr

github

[emerging]
API rates read on 19 Nov 2026: TTS $0.04–0.08 per 1,000 characters; Scribe v3 $0.20/hour and v3 Live $0.22/hour; Agents $0.08/min plus LLM and telephony; Dubbing v1 $0.33–0.50/min; Dubbing v2 $2.20/min; Music $0.15/min; sound effects, Voice Isolator and Voice Changer $0.12/min.
elevenlabs

elevenlabs

[established]
Prices have fallen hard over 12 months. Scribe went from $0.40/hour at launch to $0.20; Agents from $0.10/min to $0.08 on Starter; API TTS was cut 20% when v4 launched.
usagepricing
+ 2

Lock-in risk is about features and voices, not price. [established]
Zero Retention Mode is Enterprise-only and covers the API only, not voice clones.
elevenlabs

licenseware

On self-serve plans your audio can be used for model training unless you opt out.
jazz

[emerging]
The Startup Grant (33M credits for 12 months) is worth $1,300–2,600 if spent on API TTS. ElevenLabs’ grants blog says it equals over 680 hours of Conversational AI, and third-party perk pages quote a value of more than $5,500. That is the biggest zero-cash lever available to you. [established]
Your highest-ROI new use is an operator-facing mock-interview agent built as an open-source template. A public phone receptionist is a low-value project for you. [speculative]
Legal: in the US, an AI voice counts as an “artificial voice” under the TCPA, so calls need prior express consent.
econsent

shipkevich

Calling yourself is fine. Since 2 Aug 2026, EU AI Act Article 50 requires that cloned-voice deepfakes and AI interactions be disclosed.
warrant

regulation-ai

[established]
Single recommendation: switch to Scribe v3, benchmark it publicly, and apply for the grant this week. [emerging]
B. Product and API Catalog

Official pages read on 19 November 2026: the ElevenLabs API pricing page, the main pricing page (which carried a “Black Friday, up to 55% off, ends December 1” banner and a stale “first month $1, ends September 30” banner),
elevenlabs

the Scribe v3 launch blog (published 10 Nov 2026, updated 14 Nov 2026), and the ElevenAgents pricing page (read through its indexed snippet, not a full render).

Product What it does Model(s) API price unit (19 Nov 2026) Russian quality evidence Key limits
Scribe (batch STT) Transcription with speaker diarization (up to 32 speakers), word timestamps, audio-event tags, keyterm prompting (up to 1,000 terms), plain-English formatting rules scribe_v3 (new), scribe_v2, Scribe v2 Medical; v1 deprecated $0.20/hour; keyterms +$0.05/h; transcript editing +$0.07/h Vendor: v3 has 27% lower WER than v2 averaged over 102 FLEURS languages and “improves in every language”; no Russian figure published. Independent: see section F 90+ languages (product page says 99); 3 GB per file; US region by default
Scribe Live (realtime STT) Streaming STT with turn prediction, realtime diarization (new in v3), multilingual code-switch mode, keyterm library scribe_v3_live, scribe_v2_realtime (~150 ms) $0.22/hour; keyterms +$0.08/h; keyterm library +$0.08/h; editing +$0.12/h Vendor: 43% lower semantic WER than v2 Realtime across 102 languages Also used inside ElevenAgents
Text-to-speech Speech synthesis with audio tags, multi-speaker dialogue Eleven v4, v4 Turbo (~100 ms), v3, v3 Conversational (~280 ms), Multilingual v2, Flash v2.5 (~75 ms) v4/v3/Multilingual v2: $0.08 per 1k chars. v4 Turbo / v3 Conversational / Flash: $0.04 per 1k chars No independent Russian MOS found. Anecdotal reports of stress (ударение) errors on rare words, surnames and English tech terms [low-authority] v3: 5,000 chars per request; Multilingual v2: 10,000; Flash: 40,000. Flash covers 32 languages; v4 90+
Instant Voice Cloning (IVC) Clone from about 1–2 minutes of audio Works on v4 Starter plan and above Cross-lingual accent “leak” reported anecdotally [low-authority] Consent checkbox only
Professional Voice Cloning (PVC) Fine-tuned clone of your own voice Fine-tunable on v4 (was not supported on v3) Creator plan and above (1 PVC slot); Scale 3; Business 10 No independent RU or RU→EN test found 30 minutes minimum, 1–3 hours recommended; own voice only; spoken verification; training takes hours
Voice Design Create a synthetic voice from a text prompt — Credits / API n/a Free tier includes it
Voice Changer (speech-to-speech) Re-voice a recording — $0.12/min — 70+ languages
Eleven Realtime API Native speech-to-speech conversational models, compatible with OpenAI Realtime v1 Mini (620 ms to first audio), v1 Max (570 ms, tool calling) $0.08/min connected audio plus tokens (Mini $0.20/$6 per 1M input/output; Max $1.80/$9) — Concurrency 4–40 by plan; burst at $0.16/min
Speech Engine Adds ElevenLabs voice to your own chatbot — $0.08/min — Same concurrency table
ElevenAgents Voice/chat/email agents: Twilio, SIP trunk, Exotel, WhatsApp inbound and outbound calls, batch calling, tools/MCP, knowledge base, choice of LLM Uses Scribe v3 Live and v4 Turbo voices; many LLMs $0.08 per call minute on every plan; burst $0.16; $0.003 per text message; LLM passed through; telephony billed by your carrier Same as STT/TTS Included minutes / concurrency: Free 15/4, Starter 75/6, Creator 275/10, Pro 1,238/20, Scale 3,738/30, Business 12,375/40
Reception by ElevenAgents Packaged AI receptionist with a phone number — Basic $29/month: 75 minutes, $0.45/min overage, 1 number, 1 call at a time — Calendar and Zapier integrations
Dubbing v1 Automatic dubbing with speaker detection — $0.33/min with watermark, $0.50/min without No independent RU↔EN test found 29 languages; MP3/MP4/WAV/MOV
Dubbing v2 End-to-end dubbing that keeps voice and emotion — $2.20/min — 92 languages
Dubbing Studio / Productions Editor; fully managed human-in-the-loop dubbing — Starter and above (Studio); Productions on Enterprise — —
Studio (Projects), GenFM, ElevenReader, Audio Native Long-form editor, AI podcast generation, reader app, embeddable article audio player — Credits — Studio projects: Free 3, Starter 20
Sound Effects Text-to-SFX (SFX v3 rolled out 2 Nov 2026), royalty-free SFX v3 $0.12/min n/a 44.1 kHz MP3 / 48 kHz WAV
Eleven Music Text-to-music Music v1/v2 (v2.5 cited by third parties) $0.15/min; $1.50 per fine-tune n/a 5-minute maximum; commercial use on Starter and above
Voice Isolator Removes noise and reverb — $0.12/min Works for any language 500 MB / 1 hour per file
Voice Library and payouts Share a PVC and earn money when paid users generate with it — Fixed fee per 1k characters (rate not published); Stripe Connect; roughly weekly payouts — PVC only; manual review; only paid-user usage counts
Iconic Marketplace Licensed celebrity and estate voices — Negotiated — —
Image and Video, Ads Engine Generative image/video and ad creation inside Creative Third-party models (not verified) Credits; Starter and above — [no source found for model names]
Hosted MCP server Exposes ElevenLabs to Claude, Cursor and other MCP clients — Pay for the underlying usage — Hosted at api.elevenlabs.io/v1/mcp with OAuth. The local elevenlabs-mcp repo was archived on 20 Aug 2026
github


SDKs, CLI, integrations Python and JS SDKs (MIT), n8n node (which you already use), Zapier — — — —

Release timeline, Nov 2025 → Nov 2026.

4 Feb 2026: a $500M Series D led by Sequoia at an $11B valuation, bringing total funding to $781M across five rounds; ElevenLabs says it closed 2025 with over $330M in ARR (ElevenLabs blog).
Spring 2026: API/Agents price cut and pay-as-you-go introduced.
elevenlabs

28 Sep 2026: Eleven v4 / v4 Turbo launched, with a 20% API TTS cut. Dubbing v2, v3 Conversational and Scribe v2 Medical were added to the price list.
usagepricing

releasebot

30 Sep 2026: staff tender at a $22B valuation; Reuters reported the valuation “doubles to $22 billion on surging AI voice-agent demand” (30 Sep 2026).
26 Oct 2026: adjustable clone strength for v4 voices.
releasebot

2 Nov 2026: SFX v3, plus v4 voice-clone fine-tuning expanded.
releasebot

10 Nov 2026: Scribe v3 / v3 Live.
cellcog

[established for releases; funding numbers rely on press as summarised on Wikipedia]

Independent quality benchmarks.

STT: On the Artificial Analysis non-streaming leaderboard (Nov 2026), Scribe v3 scores 1.9% AA-WER. Whisper large-v3 scores 4.07–4.1% on its best host and 10.1% on Replicate, which shows that hosting and decoding choices matter.
artificialanalysis

cekura

Secondary summaries of the same board give AssemblyAI Universal-3.5 Pro 2.99% and OpenAI gpt-transcribe 3.31%.
mixpeek

All of these are dominated by English. [established, but not Russian]
TTS: On the Artificial Analysis Provider Voice arena, Eleven v4 Turbo and v4 are tied at Elo 1340, ahead of Cartesia Sonic 3.6 (1294) and Inworld Realtime TTS-2 (1269).
artificialanalysis

On the Controlled Voice (cloning) arena, v4 Turbo is only 6th at 1156.
artificialanalysis

Alibaba’s Qwen-Audio leads there, according to Miraflow’s reading of the board.
miraflow

[established]
C. Pricing and Limits, with the Three Cost Profiles
C1. Plan table (official, read 19 Nov 2026)
Plan Price/month Creative credits API dollar pool (≈ plan price) Example API inclusions Agents minutes / concurrency Cloning Commercial use
Free $0 10k ≈$0.90 10k v4 chars; 4 h 30 min Scribe 15 / 4 none No
Starter $6 (first month $1 in some promos) 30k $6 75k v4 chars; 30 h Scribe; 18 min Dubbing v1 75 / 6 IVC Yes
Creator $22 (first month $11) 121k $22 275k v4 chars; 110 h Scribe; 44 min Dubbing v1 without watermark; 10 min Dubbing v2 275 / 10 1 PVC Yes
Pro $99 600k $99 1.238M v4 chars; 495 h Scribe 1,238 / 20 1 PVC; 44.1 kHz PCM via API Yes
Scale $299 (3 seats) 1.8M $299 3.738M v4 chars; 1,495 h Scribe 3,738 / 30 3 PVC Yes
Business $990 (10 seats) 6M $990 12.375M v4 chars; 4,950 h Scribe 12,375 / 40 10 PVC Yes
Enterprise Custom Custom Custom Zero Retention Mode, data residency, BAA, SSO Custom More Yes
elevenlabs


Notes on the plan table:

Annual billing costs 10 months’ price for 12 months.
elevenlabs

Creative-app concurrency by plan is 2 / 3 / 5 / 10 / 15 / 25 concurrent requests.
elevenlabs

In the Creative app, unused credits roll over for up to two months.
elevenlabs

On the API, usage above the plan’s included amount is billed at the same per-unit rate through pay-as-you-go.
elevenlabs

The “dollar pool” reading is my inference. Each plan’s included API amounts, multiplied by the unit price, add up to the plan price (for example, Creator’s 275k v4 characters × $0.08 = $22, and its 110 hours × $0.20 = $22). That suggests the inclusions are one shared allowance. [emerging, my inference]
Watch-out: Scribe through Creative credits costs about 330 credits per minute.
mrktcorrect

That makes app transcription about 20 times dearer than the API: roughly $3.95/hour on Starter, against $0.20. Check which subscription family your n8n API key bills against. [emerging]
C2. Resolving the Flash price contradiction ($0.04 vs “$0.15–0.30” per 1k characters)
Channel Model Credits per char Effective $ per 1k chars
API (dollars), every plan and pay-as-you-go Flash v2.5 / Turbo, v4 Turbo, v3 Conversational n/a $0.04
API v4, v3, Multilingual v2 n/a $0.08
Creative credits, Starter 1 credit/char (Multilingual/v3) 1 $6 / 30k = $0.200
Creative, Creator 1 1 $22 / 121k = $0.182
Creative, Pro / Scale / Business 1 1 $99/600k = $0.165; $299/1.8M = $0.166; $990/6M = $0.165
Creative, Flash at 0.5 credit/char 0.5 0.5 Starter $0.100; Creator $0.091; Pro and above ≈$0.083
Creative “extra minute” (≈1k chars) — — Free ~$0.36; Starter ~$0.20; Creator ~$0.18; Pro and above ~$0.17
Pre-cut API (Creator, before the spring-2026 reduction) Flash — $0.11, per ElevenLabs’ own price-cut blog

Verdict: your note of about $0.04 is correct for the API.
saascrmreview

elevenlabs

The vendor’s “$0.15–0.30” matches Creative-plan credit pricing for 1-credit-per-character models, or the Free-plan extra-minute figure, not API Flash.
cloudzero

elevenlabs

[established]

Robocall implication: if the call runs on ElevenAgents, TTS is bundled into the $0.08 per minute, so the per-character price doesn’t matter.
reachkit

A one-way pre-rendered 60-second brief of about 900 characters costs ≈$0.036 on Flash or v4 Turbo.
C3. Three cost profiles (API rates, 19 Nov 2026)

(i) STT only, 25 hours of Russian audio a month. This is exactly your current rail: 50 min/day × 30 days = 25 hours.

Pay-as-you-go: 25 h × $0.20 = $5.00. With Free-plan inclusions first: 4.5 h free + 20.5 h × $0.20 = $4.10. Adding keyterm prompting costs +25 × $0.05 = $1.25.
Starter API plan: $6 with 30 hours included. Pick it only if you also need the commercial licence or IVC.
Cheapest: pay-as-you-go at ≈$4–5/month. Pay-as-you-go top-ups start at $5 and stay valid for 12 months, according to one third-party guide
rakazo

[single-source].
For comparison, the Whisper API (whisper-1) at $0.006/min = $0.36/hour → $9.00, which matches your October estimate. Dasha’s pricing guide reports whisper-1 is deprecated and scheduled to shut down on 26 Feb 2027 [single-source], so that leg has to be replaced anyway.

(ii) STT 25 h + 60 min of cloned-voice TTS + 60 min of dubbing.

STT $5.00.
TTS: 60 min ≈ 60k chars. On v4 that is $4.80; on v4 Turbo $2.40.
Dubbing v1 without watermark: 60 × $0.50 = $30.00.
Usage total: $39.80.
With IVC: Starter $6 plus $33.80 of pay-as-you-go = $39.80.
With PVC: Creator $22 (one slot) plus $17.80 = $39.80. Once usage passes $22, the PVC is effectively free.
With Dubbing v2 instead: 60 × $2.20 = $132 → $141.80.
Home-built dub (Scribe $0.20 + LLM translation of about $0.10 + v4 TTS of the translation at $4.80): about $5 for those 60 minutes, so profile (ii) total ≈ $27, held up by Creator’s $22 PVC floor. [established arithmetic; dollar-pool assumption emerging]

(iii) A voice agent with 300 call-minutes a month.

Platform: Creator $22 (275 minutes) + 25 × $0.08 = $24.00. Pay-as-you-go: 300 × $0.08 = $24.00. Starter: $6 + 225 × $0.08 = $24.00. Every route costs the same, so choose the plan by concurrency and features.
a8gent

LLM, passed through: GPT-5 Mini ≈$0.0024/min → $0.72; Claude Sonnet 4.6 ≈$0.026/min → $7.80. These are a third party’s reading of ElevenLabs’ calculator [single-source]. ElevenLabs adds a 10% uplift on some Gemini and Claude models [single-source].
reachkit

Telephony: not charged by ElevenLabs. Twilio or your SIP provider bills you directly
elevenlabs

[no source found for the current Twilio per-minute rate; verify on Twilio].
Silences longer than 10 seconds get a 95% discount. Burst calls above your concurrency limit cost $0.16/min.
elevenlabs

elevenlabs

Total ≈ $25–32 plus carrier.
Comparison points:
Deepgram Voice Agent: $0.075/min with STT and a managed LLM included, according to a DEV Community comparison
dev

[low-authority].
Retell: about $0.078/min with GPT-5 mini before telephony, according to Macha’s comparison
getmacha

[single-source].
Reception by ElevenAgents: $29 for 75 minutes and $0.45/min after that, so it is the wrong product at 300 minutes.
elevenlabs

D. Free Money, Programs, Hiring and Open-Source Doors

Startup Grants [established]

What you get: 12 months free, 33,000,000 characters, higher concurrency and better support, as shown on the pricing pages read 19 Nov 2026.
elevenlabs

elevenlabs

Eligibility: startups or companies with fewer than 25 employees, anywhere in the world. You need a monetized product use case that uses AI voice. Reselling characters ends the grant.
elevenlabs

elevenlabs

Track record: ElevenLabs’ grants blog said in August 2025 that it had awarded over 4,000 Startup grants, unlocking more than 2.2M hours of free AI voice generation (no newer figure found). Grantees can pitch at Demo Day; the 2nd annual virtual Demo Day on 21 Oct 2026 featured 11 grantees.
Acceptance reality: no acceptance rate is published [no source found]. The Demo Day page calls the program “selective”.
elevenlabs

Logo requirement: the original 2023 version required an “ElevenLabs Grants” logo on the website.
elevenlabs

Whether that still applies is not confirmed.
What it is worth to you: about $1,320 at Flash/v4 Turbo rates or $2,640 at v4 rates if spent purely on TTS. ElevenLabs’ grants blog equates it to over 680 hours of Conversational AI, and third-party perk pages (Superlaunch, InnMind) quote ElevenLabs valuing it at more than $5,500. It is clearly worth the 30 minutes an application takes.
How to apply: fill in the grants application form. Your account is upgraded immediately if accepted.
elevenlabs


Other programs

Students: 3 months of Creator free.
elevenlabs

Impact Program: listed under Resources for nonprofit and accessibility work. You probably don’t qualify.
Affiliate program: PartnerStack, automatic approval. It pays 22% recurring for 12 months on Starter, Creator, Pro and Scale, 11% on Business, and nothing on Enterprise. Google Ads affiliate links are banned. Payouts come at the end of the third month after the commission is earned. Terms were last updated 10 June 2026.
saasaffiliate
+ 2

Cookie length conflicts between sources (30 vs 90 days).
adskull

[emerging; multiple aggregator sources, official terms not opened]
Voice Library payouts: creators had earned over $22M in total across 10,400+ creators and 32 languages as of a May 2026 company post (reported second-hand by Voicfy). Only PVCs qualify, payouts go through Stripe Connect, and only paid users’ generations count.
voicfy

One secondary source estimates about $0.03 per 1,000 characters and says most voices earn under $100 a month
money-forge

[low-authority].
Ambassadors: you already track the Content Creator form (live) and Community Builder (paused). I found nothing newer.

Hiring

ElevenLabs’ Ashby board lists 128 open positions. One aggregator counts 55 and says most are remote-friendly.
ashbyhq

newjob

Forward Deployed Engineer – Software Engineer roles are remote, listed for Germany, London, New York, Poland and San Francisco, plus region-specific roles for Argentina, Mexico, the UK and the USA.
elevenlabs
+ 2

There is also an FDE “Recent Graduate” role and Developer Relations / Developer Experience Engineer listings.
elevenlabs

productjobsanywhere

Portugal: a Full-Stack Engineer posting lists Portugal as a location, which fits your Lisbon time.
ashbyhq

Hiring bar, from the postings: a strong technical background for designing customer integrations; making “compelling technical demonstrations”; spotting patterns that can be turned into products; working directly with customers.
simplify

Pay: one site estimates FDE compensation at $160–240k [low-authority].
fdepulse

Implication: a public, working ElevenAgents demo with real latency and cost numbers is close to a work sample for the job. [emerging]

Open-source doors [emerging]

The official local MCP server (elevenlabs/elevenlabs-mcp) was archived on 20 Aug 2026 in favour of the hosted MCP, so outside PRs there are closed.
github

Community gaps already being filled: Walma-Labs built a podcast-production MCP server, citing the hosted MCP’s narrow focus. neomorrison built a 28-tool MCP server.
github

github

The Python and JS SDKs are MIT-licensed. I could not verify whether they are code-generated or how they take contributions [no source found]. Expect small docs and example PRs to merge more easily than core changes. [speculative]
The best PR target I found is outside ElevenLabs. vakovalskii/stt-ru-benchmark (neuraldeep.ru) benchmarks Whisper, GigaAM v3, Deepgram and Parakeet on FLEURS-ru, RuLibriSpeech and spontaneous podcast speech, but includes no ElevenLabs.
github

A PR that adds Scribe v3 and gpt-4o-transcribe fills a real gap.
E. Use Cases, Ranked
# Use case Mechanism / products 90-day value Monthly cost at your scale To start Best alternative Verdict
1 Voice rail: switch to Scribe v3, drop the Whisper API leg Change the n8n node or HTTP model_id to scribe_v3; add the keyterm list (agent names, repos, tools); keep local large-v3 on the RTX 5060 Ti as the second engine High: saves about $9/month and probably raises Russian accuracy $4–5 Change one parameter today Local Whisper large-v3 ($0); gpt-4o-transcribe; GigaAM v3 (local, Russian-specialised) ElevenLabs wins on paid-API price and aggregate accuracy. Russian gain is plausible but unproven for your voice [emerging]
2 Independent Russian STT benchmark (operator voice notes + FLEURS-ru) Scribe v3, v2, v3 Live vs local large-v3, gpt-4o-transcribe, GigaAM v3, Deepgram Nova-3; publish WER/CER with confidence intervals; PR to stt-ru-benchmark; Habr + X write-up tagging ElevenLabs High: visibility in both RU and EN ecosystems, a hiring artifact, and it answers your own engine question <$5 (FLEURS-ru is about 2–3 h of audio) Hand-correct 100 voice notes as references n/a Prior art: partial. Habr Feb 2026 (Scribe v2, mixed-language YouTube set); Smolevich May 2026 (Scribe v1, 20 memos); vakovalskii (no ElevenLabs). No Scribe v3 Russian benchmark found. Searched GitHub and Habr via web search; npm, PyPI, HF Spaces and the n8n registry were not searched
3 Mock-interview agent (hiring-manager persona) ElevenAgents with the job description and your CV in the knowledge base, Scribe v3 Live, v4 Turbo voice, Claude/GPT as LLM, post-call transcript to a rubric via webhook; publish as an open-source template High: serves the 100-interview goal directly and doubles as an FDE/DevRel work sample Free 15 min, then $0.08/min plus LLM (≈$10–25 at 120–300 min) Free account; write the persona prompt OpenAI Realtime / GPT-Live-1 (reported $0.05/min [single-source]); Gemini Live; Hume; Vapi/Retell Tie on cost, ElevenLabs wins on voice quality and visibility to the target employer
4 Meeting robocall to yourself (60 and 5 minutes before) Option A: ElevenAgents outbound call over Twilio or WhatsApp. Option B: render the brief with v4 Turbo and send it as a Telegram voice note Medium A: ~3 meetings/day × 2 calls × 1 min ≈ 180 min → ≈$15 plus LLM plus Twilio. B: ≈$0.04 per brief → about $7/month B: an n8n branch today. A: a Twilio number Telegram voice note (B); Twilio <Play> with pre-rendered audio ElevenLabs TTS wins; the full Agents call loses to B unless you need two-way Q&A on the call. TCPA consent is not an issue when you call yourself
5 “Call my agents” voice interface to the coding fleet ElevenAgents with server tools / MCP / custom LLM endpoint that queues tasks into your agent orchestrator; Scribe v3 Live multilingual mode for RU/EN code-switching Medium-high (demo value) $5–15 Expose one HTTPS tool endpoint OpenAI Realtime with tools; Pipecat or LiveKit with local Whisper Tie. Prior art not checked in registries [no source found]
6 PVC of the operator (RU native, accented EN) Creator plan; 1–3 hours of clean audio. Your Telegram voice notes are Opus-compressed and noisy, so record 60+ min clean in each language; fine-tune on v4; use for narration and short voice replies Medium $22 (≈$11 first month or less during the Black Friday sale ending 1 Dec) Record 1 hour per language at 44.1 kHz IVC (Starter, $6); open-source F5-TTS / XTTS / Chatterbox (Russian quality unverified) ElevenLabs wins on quality and convenience [emerging]. For job-application intros, a real recording beats a clone on trust
7 Dubbing NotebookLM podcasts RU↔EN Home-built: Scribe v3 (diarized) → LLM translation → v4 TTS with stock voices per speaker → merge Medium ≈$5 per audio hour home-built vs $30 (v1) or $132 (v2) n8n workflow Rask, HeyGen for video lip-sync (prices not verified) Built-in ElevenLabs Dubbing loses on price for audio-only; ElevenLabs components win
8 Content factory (shorts narration, Audio Native, SFX, music, audiobook) v4 Turbo narration $0.04/min; SFX $0.12/min; Music $0.15/min (commercial on Starter+); Audio Native embed Medium $6–22 Starter for the commercial licence Kokoro/Piper locally (Russian coverage unverified); Cartesia; Inworld TTS-2 ($20.83/1M chars) Tie: ElevenLabs best on quality, Inworld cheaper
9 Interview recording transcription and analysis; Plaud/Omi diarization Scribe v3 batch with diarization (17% fewer errors on 3+ speakers, vendor figure) → LLM rubric Medium <$1 Same API key AssemblyAI; WhisperX locally ElevenLabs wins (diarization) [emerging]
10 AI receptionist on a public number/WhatsApp; inbound lead qualification ElevenAgents + WhatsApp/Twilio, or Reception at $29 Low right now (little inbound volume) $6–29 — Retell, Vapi, Bland Defer
11 Affiliate links in your content PartnerStack 22% recurring Low-medium $0 Sign up — Free option; disclose
12 Voice Library payout from the operator PVC Share the PVC Low (most voices earn <$100/month [low-authority]) $0 extra After #6 — Optional; reputational risk of your voice saying anything

Gaps in the original list.

Eleven Realtime is compatible with OpenAI Realtime. That lets you A/B ElevenLabs against OpenAI in one codebase, which makes a strong “independent voice-agent latency benchmark” artifact. Prior art not found; only vendor latency figures were seen.
Scribe v3 Live adds diarization to live calls, which is useful for live meeting notes.
elevenlabs

F. Contradictions and Debates
API price levels in third-party guides. Guides from June to September 2026 quote $0.10 (Multilingual) / $0.05 (Flash) per 1k characters and $0.22 / $0.39 per hour for Scribe.
puter

goodvibecode

The official page on 19 Nov 2026 shows $0.08 / $0.04 and $0.20 / $0.22.
elevenlabs

Both were true at their dates. Cuts came at the spring-2026 pay-as-you-go launch, the 28 Sep v4 launch and around Scribe v3. Two secondary sources describe the Scribe v3 prices as “unchanged versus the previous generation”, which conflicts with the earlier $0.22 / $0.39.
dev

cellcog

Treat any guide older than 28 Sep 2026 as stale. [established]
Is Scribe best for Russian?
For: Habr (Feb 2026): Scribe v2 had the lowest WER, 0.388, against AssemblyAI 0.416, Deepgram Nova-3 0.449, Whisper large-v3 (Fireworks) 0.457 and Yandex SpeechKit 0.657. But the set mixes languages and the references include sound descriptions, so the absolute numbers are inflated.
habr

Smolevich (May 2026): Scribe v1-experimental 11.5% Russian WER, against GigaAM 18.2% and Deepgram Nova-3 21.5%, on 20 memos
github

[single-source, small sample].
Against (steelman): Russian-specialised models win on some domains. In vakovalskii’s benchmark, rankings flip between domains, and the clean-read-speech winner comes second-to-last on audiobooks and spontaneous speech.
github

On People’s Speech English conversational audio, Scribe v2 (13.6%) did not beat self-hosted Whisper large-v3 (11.9%)
wisprs

[single-source]. Vendor FLEURS numbers are internal and averaged per language.
elevenlabs

What would settle it: Scribe v3 vs large-v3 vs GigaAM v3 vs gpt-4o-transcribe on FLEURS-ru, Golos and 100 of your own hand-corrected notes, with confidence intervals. That is your use case #2.
Is Eleven v4 “#1”? It is first on the Provider Voice arena, but 6th on the Controlled Voice (cloning) arena, behind Qwen-Audio among others.
miraflow

ElevenLabs’ “75% preferred” is a vendor-run blind test.
unite

[established]
Value of open-source TTS. Resemble (Chatterbox) and Mistral (Voxtral) publish blind tests where their models beat ElevenLabs Flash v2.5 (63.75% and 68.4% preference).
diffnotes

These are vendor tests, in English, against an older model [low-authority]. For Russian I found no independent MOS for any engine; a Russian TTS MOS bench is open territory.
Telephony “included”? Some guides say ElevenLabs “does not charge extra for telephony”. That is true, but your Twilio or SIP carrier still bills you.
getmacha

elevenlabs

[established]
Affiliate cookie length: 30 vs 90 days across aggregators.
adskull

aiaffiliateprograms

Unresolved. Check the PartnerStack terms.
G. Failure Modes and Risks
Data and training: on self-serve plans, uploads may train models by default; you can opt out in settings, but the opt-out only covers new data. Data is kept up to 30 days in backups. Zero Retention Mode is Enterprise-only and API-only,
licenseware

github

and voice clones can never be zero-retention.
jazz

Action: switch off “Improve the models” today. Don’t send client-confidential calls through self-serve plans. [emerging; secondary compliance sites citing ElevenLabs docs plus the June 2026 changelog]
Clone abuse rules: PVC is limited to your own voice and needs spoken verification. Celebrity and other high-risk voices are blocked. All output can be traced to the account that generated it.
nhimg

elevenlabs

Bypassing verification or impersonating someone risks an account ban.
margabagus

[established]
EU AI Act Art. 50 (applies from 2 Aug 2026):
People must be told when they are talking to an AI system. This applies to your agent, receptionist and mock interviewer.
Deployers must disclose deepfake audio or video, with an audible label for audio.
orrick

regulation-ai

A cloned “you” narrating content plausibly qualifies.
The European Commission published the final, voluntary Code of Practice on marking and labelling AI-generated content on 10 June 2026 (press release IP/26/1328).
Action: add a spoken “AI-generated voice” line to cloned narration and a disclosure at the start of agent calls. [established]
US TCPA: under FCC Declaratory Ruling 24-17 (CG Docket 23-362, adopted 2 Feb and released 8 Feb 2024), calls made with AI-generated voices are “artificial” under the TCPA. Calls to consumers need prior express consent, and telemarketing needs written consent; violations cost $500–1,500 per call. Never point outbound batch calls at cold lead lists. Self-reminder calls are fine. [established]
Portugal/EU call recording: recording needs a GDPR lawful basis and notice to participants [no source found for Portugal-specific rules; verify before recording leads].
NotebookLM output dubbing / ToS: [no source found]. NotebookLM’s hosts are synthetic voices, not people, so dubbing them is low deepfake risk. Check Google’s generative-AI terms on output ownership, and label AI audio under Art. 50 either way. [speculative]
Russian failure modes: stress errors on rare words and surnames; English tech terms (“useEffect”) read with Russian phonetics; a Slavic accent leaking into cross-lingual output.
nikitaz
+ 2

All reported anecdotally [low-authority]. Mitigate with pronunciation dictionaries and the v4 audio tags.
Billing surprises:
Transcribing through Creative credits instead of the API costs about 20 times more.
Burst calls are billed at double rate.
elevenlabs

Dubbing v2 is 4–7 times the price of v1.
Promotional first-month prices revert to full price.
Vendor and roadmap churn: the local MCP repo was archived; v1 Scribe is deprecated;
wisprs

the default TTS model ID is eleven_v3 unless you set v4.
medium

Pin model IDs in n8n.
Reliability incidents: I did not research outage history [no source found]. Keep local large-v3 as a hot fallback.
H. Top 3 Moves, Options and Recommendation

Move 1: Scribe v3 switch plus a public Russian STT benchmark.

First step today, zero cost: set model_id=scribe_v3 in the voice-rail node, pin it, and turn off model-training data use. Start saving pairs of raw voice notes and corrected text.
Trade-off: you lose the Whisper API leg’s diversity, but local large-v3 keeps two engines running.
Risk: v3 may not beat GigaAM on your speech. That is still publishable.

Move 2: apply for the Startup Grant with a concrete voice product.

First step today: submit the grants application, describing the “call my agents” / mock-interview voice agent as a product with a paid tier.
Trade-off: you may have to show ElevenLabs branding and a product URL.
Risk: rejection, at zero cost.

Move 3: open-source an ElevenAgents mock-interview template and use it.

First step today: on the Free plan (15 min), create an agent with a hiring-manager persona from one real FDE or DevRel job description. Record one session and post the repo and clip.
Trade-off: about 3–5 days of build time.
Risk: EU disclosure duties if the public uses it. Add a disclosure line.

Options considered and set aside for now: PVC purchase; built-in ElevenLabs Dubbing; Reception receptionist; Voice Library payouts. Each pays back too slowly within 90 days.

Single recommendation: do Move 1 this week, publish the benchmark within 14 days, and submit Move 2 the same day you start. Confidence: [emerging]. The plan rests on one main assumption: that Scribe v3 will be at least as accurate as large-v3 on your own Russian dictation, so dropping the Whisper API costs no quality. The benchmark itself tests that assumption.

I. Official Pages Read and Dates

ElevenLabs API pricing page: read 19 Nov 2026. Shows v4, v4 Turbo, Scribe v3 and Dubbing v2 prices; carried a stale “ends September 30” promo banner.
ElevenLabs Creative pricing page: read 19 Nov 2026. Black Friday banner “up to 55% off, ends December 1”. 
elevenlabs

ElevenAgents pricing page: indexed snippet read 19 Nov 2026.
“Introducing Scribe v3 and v3 Live” blog: published 10 Nov 2026, updated 14 Nov 2026; read 19 Nov 2026.
ElevenLabs Startup Grants page and grants blog posts, voice-cloning docs, payouts docs, burst-pricing docs, Zero Retention changelog (8 Jun 2026): read through indexed content on 19 Nov 2026.
Habr STT review (10 Feb 2026), the vakovalskii Russian STT benchmark repo, and the archived elevenlabs-mcp repo (archived 20 Aug 2026): 
github

 opened 19 Nov 2026.

## Links from page anchors

https://support.claude.com/articles/8525154-claude-is-providing-incorrect-or-misleading-responses-what-s-going-on
https://medium.com/@gsaidheeraj/eleven-v4-and-v4-turbo-a-voice-that-can-act-and-one-that-can-answer-in-real-time-1473b9fcce1f
https://cellcog.ai/blog/elevenlabs-scribe-v3/
https://mixpeek.com/curated-lists/best-speech-to-text-apis
https://artificialanalysis.ai/speech-to-text/non-streaming
https://habr.com/ru/articles/993786/
https://github.com/Smolevich/stt-benchmarks
https://elevenlabs.io/pricing/api
https://elevenlabs.io/pricing/agents
https://www.usagepricing.com/blueprint/elevenlabs
https://elevenlabs.io/docs/changelog/2026/6/8
https://wiki.licenseware.io/wiki/elevenlabs-licensing/
https://jazz.security/dlp-explained/dlp-basics/is-elevenlabs-safe-for-company-data/
https://econsent.org/posts/ai-voice-agents-tcpa-rules-october-2026/
https://www.shipkevich.com/ai-voice-agents-outbound-calls-tcpa/
https://www.warrant.build/blog/eu-ai-act-article-50-transparency-code-of-practice/
https://www.regulation-ai.eu/en/transparency-obligations/
https://elevenlabs.io/pricing
https://github.com/elevenlabs/elevenlabs-mcp
https://elevenlabs.io/blog/weve-lowered-api-agents-pricing-and-introduced-pay-as-you-go
https://releasebot.io/updates/eleven-labs
https://www.cekura.ai/blogs/word-error-rate
https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice
https://artificialanalysis.ai/text-to-speech/leaderboard/controlled-voice
https://miraflow.ai/blog/elevenlabs-eleven-v4-architecture-benchmarks-explained-2026
https://elevenlabs.io/docs/overview/intro
https://join.elevenlabs.io/developer-api
https://mrktcorrect.com/blog/elevenlabs-pricing
https://saascrmreview.com/elevenlabs-pricing/
https://www.cloudzero.com/blog/elevenlabs-pricing/
https://reachkit.ai/directory/tools/elevenlabs-agents
https://rakazo.com/voice-providers/elevenlabs
https://a8gent.com/platforms/elevenlabs-agents
https://elevenlabs.io/docs/eleven-agents/guides/burst-pricing
https://help.elevenlabs.io/hc/en-us/articles/29298065878929-How-much-does-ElevenAgents-cost
https://dev.to/agent002_a9n9/elevenlabs-cost-per-minute-0072-to-speak-0080-as-an-agent-and-0075-at-deepgram-2k84
https://www.getmacha.com/blog/elevenlabs-agents-pricing-explained
https://elevenlabs.io/reception
https://elevenlabs.io/pricing/api?price.platform=api
https://elevenlabs.io/startup-grants
https://elevenlabs.io/blog/elevenlabs-startup-grants-just-got-bigger-now-12-months-and-over-680-hours-of-conversational-ai-audio
https://demoday.elevenlabs.io/
https://elevenlabs.io/blog/elevenlabs-grants
https://elevenlabs.io/students
https://saasaffiliate.net/programs/elevenlabs/
https://adskull.io/en/affiliate-programs/elevenlabs
https://voicfy.com/blog/ai-voice-licensing-rates-what-platforms-pay
https://www.money-forge.org/blog/voice-acting-income-guide-2026
https://jobs.ashbyhq.com/elevenlabs/
https://newjob.tech/company/elevenlabs
https://elevenlabs.io/careers/6c4c57c1-ec72-42ba-af3a-eb7aebbde2e6/forward-deployed-engineer-software-engineer
https://elevenlabs.io/careers/dd26398c-54d8-4200-a181-65e6203ec9a6/forward-deployed-engineer-recent-graduate
https://productjobsanywhere.com/companies/elevenlabs/
https://simplify.jobs/p/5222d9ab-7246-4455-bd30-6d5d729cc8d4/Forward-Deployed-Engineer--Recent-Graduate
https://fdepulse.com/companies/elevenlabs/
https://github.com/neomorrison/elevenlabs-mcp
https://github.com/Walma-Labs/elevenlabs-mcp
https://github.com/vakovalskii/stt-ru-benchmark
https://elevenlabs.io/blog/scribe-v3
https://developer.puter.com/tutorials/elevenlabs-api-pricing/
https://www.goodvibecode.com/text-to-speech/elevenlabs-api-pricing-explained
https://dev.to/delimitter/elevenlabs-launches-scribe-v3-and-scribe-v3-live-its-most-accurate-speech-to-text-models-yet-5e64
https://wisprs.co/voice/elevenlabs/scribe-v1
https://www.unite.ai/elevenlabs-launches-eleven-v4-with-low-latency-turbo-variant/
https://diffnotes.tech/posts/free-tts-models-replace-elevenlabs
https://www.getmacha.com/blog/elevenlabs-agents-complete-guide
https://aiaffiliateprograms.ai/program/elevenlabs
https://github.com/xolace-official/xolace_app/issues/587
https://nhimg.org/elevenlabs-explained
https://elevenlabs.io/docs/eleven-creative/voices/voice-cloning
https://margabagus.com/elevenlabs-voice-consent-policy/
https://www.orrick.com/en/Insights/2026/08/EU-AI-Act-Transparency-Obligations-for-AI-Generated-Content-Article-50
https://nikitaz.ru/neyroset-elevenlabs-na-russkom/
