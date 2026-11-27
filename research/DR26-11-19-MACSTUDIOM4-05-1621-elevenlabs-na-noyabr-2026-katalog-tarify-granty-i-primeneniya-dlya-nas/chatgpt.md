# DR26-11-19-MACSTUDIOM4-05-1621 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt.com (Pro, Deep research, Extra High; 108 citations, 193 searches по счётчику виджета)
url: https://приватный чат
dr_id: DR26-11-19-MACSTUDIOM4-05-1621
topic: ElevenLabs catalog, pricing, grants, use cases
date: 2026-11-20

extraction: backend widget_state.report_message, вынос через get_page_text двумя кусками, склейка из транскрипта сессии
---

# ElevenLabs as of November 2026: catalog, pricing, grants, risks and use cases for Palo Alto AI Research Lab

Research date: **20 November 2026, Europe/Lisbon**. Scope follows the supplied research brief. fileciteturn0file0 Pricing and product status below are point-in-time findings; ElevenLabs was running Black Friday promotions when the pricing pages were read, so I distinguish recurring list prices from temporary first-month/annual discounts. citeturn603799view0turn603799view1

## A. Executive Summary

**[established]** Keep ElevenLabs Scribe in the existing dual-ASR rail: at the current API price of **$0.20/hour**, 25 hours/month costs about **$5**, and Scribe v3 is currently near the top of independent general-purpose STT benchmarks; replacing free local Whisper with Scribe is not justified, but keeping Scribe as an independent cloud “judge” is. (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/pricing/api; Artificial Analysis, accessed 2026-11-20, https://artificialanalysis.ai/speech-to-text/non-streaming) citeturn603799view2turn237891search22

**[established]** The October pricing contradiction is resolved: current official API pricing is **$0.04/1,000 characters for Flash/Turbo** and **$0.08/1,000 for Multilingual/v3-class TTS**; the previously noted **$0.15–0.30/1k** figure is not current ElevenLabs first-party pricing and was most likely a reseller/application markup or stale quote. [no source found for the old vendor leg] (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/pricing/api) citeturn603799view3

**[established]** Eleven v4/v4 Turbo have moved ElevenLabs back to the top tier of hosted TTS: Artificial Analysis currently ranks v4 and v4 Turbo at/near the top of its provider-voice preference leaderboard, while ElevenLabs reports v4 Turbo at roughly **100 ms median model latency**. Russian is supported, but a rigorous independent **Russian-only v4/PVC benchmark remains missing**. (ElevenLabs, 2026-09-28, https://elevenlabs.io/blog/eleven-v4; Artificial Analysis, accessed 2026-11-20, https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice) citeturn528061search0turn859798search0

**[established]** Do **not** make Dubbing v2 a default content-production leg at your present economics: **$2.20/min** makes 60 minutes/month cost **$132 before TTS/STT**, while your RU↔EN use is not backed by an independent language-pair benchmark strong enough to justify that spend. Use it only on distribution assets already proving engagement. (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/pricing/api; ElevenLabs, 2026-05-28, https://elevenlabs.io/blog/introducing-dubbing-v2) citeturn603799view4turn528061search2

**[established]** The highest-ROI new ElevenLabs capability for this lab is not narration; it is **Agents + telephony + MCP/n8n** for inbound lead qualification, self-reminder calls and a “call my coding agents” interface, because the platform already supports Twilio/SIP, WhatsApp Business calling, tools, knowledge bases, batch campaigns and external assistants. (ElevenLabs docs, accessed 2026-11-20, https://elevenlabs.io/docs/eleven-agents/overview; https://elevenlabs.io/docs/eleven-agents/whatsapp; https://elevenlabs.io/mcp) citeturn919106search8turn944199search0turn267184search9

**[established]** Apply to the **Startup Grants program before buying higher plans**: qualifying startups under 25 employees can receive **33M characters / >680 agent-hours / 12 months / >$5,500 stated value**, with rolling applications and a stated one-week decision target. Your lab fits several eligibility dimensions, but the “monetized product/business model” requirement means the application should be tied to a specific product, not internal research or consulting. [single-source] (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/startup-grants) citeturn698578view2

**[established]** The Ambassador door has changed since your prior tracking: **both Community Builder and Content Creator are currently offered**, and the November 2026 terms additionally describe creator rewards/bounties; the affiliate program pays **22%** on Starter–Scale and **11%** on Business purchases for the referred subscriber's first 12 months. [single-source] (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/ambassador; affiliate terms updated 2026-06-10, https://elevenlabs.io/affiliates-terms) citeturn812944view4turn698578view3

**[established]** For hiring visibility, the strongest artifact is an **open Russian speech benchmark + ElevenLabs/n8n/MCP contribution**, not another generic voice demo: ElevenLabs is actively hiring a Developer Experience Engineer whose job explicitly includes SDKs, MCP, API tooling, technical content and “customer zero” work. (ElevenLabs careers, accessed 2026-11-20, https://elevenlabs.io/careers/d16a7ad6-ffbd-4655-8716-4872263f99ce/developer-experience-engineer) citeturn844860search7

**[established]** For public/revenue calls, compliance is not optional: AI voice calls fall under the US TCPA consent framework; EU AI Act Article 50 transparency duties have applied since **2 August 2026**; Portuguese law imposes consent/information requirements around recording and automated marketing. (European Commission, 2026-07-31 and 2026 guidance, https://digital-strategy.ec.europa.eu/en/news/commission-starts-enforcing-ai-act-rules-and-new-transparency-requirements-2-august; Portuguese Diário da República, consolidated Law 41/2004, https://diariodarepublica.pt/dr/legislacao-consolidada/lei/2004-106523049) citeturn951565search2turn919943search5

**Single recommendation — [established]:** keep Scribe as a cheap second ASR, build one self-use ElevenAgent that controls the coding fleet, and turn the implementation into a **public RU/EN Voice-Agent Benchmark + open-source n8n/MCP artifact**; apply for the grant before spending on PVC or Dubbing v2. The main assumption is that visibility to frontier-lab/ElevenLabs engineers is worth more in the next 90 days than polished synthetic-content volume.

## B. Product and API catalog table

For official feature rows below, `[single-source]` means the product capability or limit is documented by ElevenLabs but lacks independent corroboration. Where a benchmark is cited separately, the quality conclusion is not single-source.

| Product / API | What it does | Current model(s) / implementation | Price unit | Russian quality / comparative evidence | Limits / latency | Source URL |
|---|---|---|---|---|---|---|
| **Speech-to-Text — batch** | File transcription, speaker diarization, word timestamps, language detection, audio-event tagging, keyterms, formatting/edit instructions | **Scribe v3**; older Scribe v2; **Scribe v2 Medical** for clinical speech | **$0.20/h**; keyterm +$0.05/h; transcript editing +$0.07/h | **[established]** Scribe v3 is 1.9% on Artificial Analysis' current aggregate AA-WER benchmark, near the frontier, but that is **not a Russian benchmark**. ElevenLabs' older Russian marketing page reported Scribe v1 FLEURS WER 3.1%, Whisper large-v3 5.7%, Gemini Flash 2 3.9%, Deepgram Nova 2 4.8%; treat those RU figures as `[single-source]` vendor results and obsolete for v3. Independent Russian Whisper FLEURS testing puts large-v3 around 5.4–5.6% WER. | 90+ languages; v3 up to 32 diarized speakers; up to 1,000 keyterms; ElevenLabs reports 27% fewer WER than v2 across 102 FLEURS languages | https://elevenlabs.io/docs/overview/capabilities/speech-to-text ; https://elevenlabs.io/docs/overview/models ; https://elevenlabs.io/pricing/api ; https://artificialanalysis.ai/speech-to-text/non-streaming citeturn919106search5turn223023search5turn603799view2turn237891search22 |
| **Scribe realtime** | Streaming transcription for dictation, agents and live calls; diarization, turn prediction, code-switching, keyterm library | **Scribe v3 Live**; older Scribe v2 Realtime | **$0.22/h**; keyterm +$0.08/h; keyterm library +$0.08/h; edit +$0.12/h | **[emerging]** v3 Live is very new (2026-11-10). ElevenLabs reports 43–44% lower error than v2 realtime and 39% lower diarization error than its next-best tested realtime comparator; independent multilingual/Russian-specific reproduction not yet found. `[single-source]` for those claims. | v2 realtime ~150 ms stated; v3 Live designed low latency. Realtime diarization currently v3 Live only. Speaker labels can be revised/merged as context arrives. | https://elevenlabs.io/blog/scribe-v3 ; https://elevenlabs.io/docs/eleven-api/guides/how-to/speech-to-text/realtime/diarization ; https://elevenlabs.io/pricing/api citeturn528061search1turn919106search2turn603799view2 |
| **Text-to-Speech — v4** | Highest-quality expressive synthesis; natural-language acting directions, audio tags, multi-speaker dialogue | **Eleven v4** | **$0.08/1k chars** API category; Creative credits depend on model | **[established]** Artificial Analysis' current provider-voice leaderboard places v4 at/near #1; **no rigorous RU-only v4 benchmark found**. Russian falls within 90+ language support. | 90+ languages; ~10k-char generation class; optimized for quality rather than minimal latency | https://elevenlabs.io/blog/eleven-v4 ; https://elevenlabs.io/docs/overview/capabilities/text-to-speech ; https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice citeturn528061search0turn919106search6turn859798search0 |
| **Text-to-Speech — v4 Turbo** | Interactive/agent TTS from the v4 family | **Eleven v4 Turbo** | Current low-latency API class **$0.04/1k chars** where billed as Flash/Turbo | **[established]** v4 Turbo is also at the top tier of current independent listener-preference ranking; no Russian-only score published. | ElevenLabs states ~**100 ms median inference latency**; WebSocket/agents use | https://elevenlabs.io/docs/changelog/2026/9/28 ; https://elevenlabs.io/pricing/api ; https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice citeturn528061search7turn603799view3turn859798search0 |
| **Text-to-Speech — v3 / v3 Conversational** | Expressive TTS with audio tags; conversational variant optimized for interaction | **Eleven v3**, **Eleven v3 Conversational** | **$0.08/1k chars** class for quality models | Russian supported among 70+ languages. Current independent leaderboard places old v3 below v4 and several newer competitors; therefore use v4 unless compatibility/cost dictates otherwise. | v3 ~5k text limit; Conversational model ~165 ms stated | https://elevenlabs.io/docs/overview/capabilities/text-to-speech ; https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice citeturn919106search6turn859798search0 |
| **Multilingual v2** | Stable long-form narration | **eleven_multilingual_v2** | **$0.08/1k chars** | **[established]** Explicitly supports Russian; useful when long-form stability matters more than v4 expressiveness. No strong current independent Russian comparison found. | 29 languages; 10k chars/request; marketed as stable for long-form | https://elevenlabs.io/pricing/api ; https://elevenlabs.io/docs/overview/capabilities/text-to-speech citeturn603799view3turn919106search6 |
| **Flash / legacy Turbo** | Cheapest low-latency TTS | **Flash v2.5**, legacy Turbo family | **$0.04/1k chars** | Russian explicitly supported by multilingual Flash. **[single-source]** No current RU MOS/head-to-head found. | Pricing page states ~75 ms; 32 languages; 40k chars | https://elevenlabs.io/pricing/api citeturn603799view3 |
| **Instant Voice Cloning (IVC)** | Fast clone from a short sample | IVC + chosen downstream TTS model | Generation priced as TTS; feature starts **Starter $6** | **[emerging]** Works with supported languages including Russian, but docs warn uncommon accents can be less reliable. For RU-native → accented English, expect the source accent to persist rather than magically become native EN. `[single-source]` | ~1–2 minutes clean audio recommended; permission required; IVC cannot be published to public Voice Library | https://elevenlabs.io/docs/eleven-creative/voices/voice-cloning/instant-voice-cloning ; https://elevenlabs.io/docs/help-center/product/voices/my-voices/what-is-my-voices citeturn638505search1turn638505search2 |
| **Professional Voice Cloning (PVC)** | Higher-fidelity personalized voice model | PVC + v4/v3/Multilingual | TTS generation price + **Creator $22 minimum plan** | **[emerging]** Best ElevenLabs path for a stable bilingual digital twin; no independent RU→EN cross-lingual speaker-similarity benchmark found. Training with clean samples representing the intended accent/style is important. | Roughly **30–180 min** clean speech recommended; only your own voice can be PVC-cloned; verification/Voice CAPTCHA; typical training several hours | https://elevenlabs.io/docs/eleven-creative/voices/voice-cloning/professional-voice-cloning ; https://elevenlabs.io/docs/eleven-api/concepts/voice-cloning citeturn638505search0turn638505search3 |
| **Voice Design** | Generates a new synthetic voice from text description | Voice Design + v4/v3 | Preview text consumes ordinary TTS credits; three candidate voices per generation | Russian can be requested through accent/language prompting, but **no RU quality benchmark found**. ElevenLabs itself says PVC is more consistent. `[single-source]` | Experimental; prompts can specify age, accent, pacing, style, quality | https://elevenlabs.io/docs/eleven-creative/voices/voice-design/ citeturn528061search5 |
| **Voice Changer / speech-to-speech** | Converts recorded speech into another voice while retaining timing, accent/emotion, laughs, whispers etc. | `eleven_multilingual_sts_v2`; `eleven_english_sts_v2` | **$0.12/min API**; ~1,000 Creative credits/min | Russian explicitly supported by multilingual model. Useful if preserving the operator's real prosody is more important than pure text-driven narration. `[single-source]` | max segment ~5 min; background-noise removal option | https://elevenlabs.io/docs/overview/capabilities/voice-changer ; https://elevenlabs.io/pricing/api citeturn528061search4turn603799view3 |
| **Realtime Speech-to-Speech API** | End-to-end realtime audio interaction without classic STT→LLM→TTS wiring | **Eleven Realtime v1 Mini**, **v1 Max** | Connected audio **$0.08/min** + token charges; burst $0.16/min | Multilingual agent use supported; no independent Russian-specific benchmark found yet. `[single-source]` | Mini: $0.20/M input + $6/M output tokens; Max: $1.80/M input + $9/M output; concurrency 4→40 by plan | https://elevenlabs.io/pricing/api citeturn603799view2 |
| **ElevenAgents / Conversational AI** | Managed agents: voice/chat, RAG/KB, tools, workflows, testing, analytics, telephony, channels | Agents orchestration + Scribe/TTS/Realtime and selectable LLMs/custom LLM | Included minutes by plan; **$0.08/additional call-minute**, LLM extra; external carrier at cost | **[emerging]** RU can be served by multilingual STT/TTS, but end-to-end RU telephone latency/quality needs your own benchmark. | 15/75/275/1238/3738/12375 included min across Free→Business; concurrency 4/6/10/20/30/40; no agent-count limit | https://elevenlabs.io/docs/eleven-agents/overview ; https://elevenlabs.io/pricing/agents citeturn919106search8turn698578view0 |
| **Twilio / SIP telephony** | Bring phone numbers/carrier, inbound/outbound calls and SMS/MMS integration | Twilio integration, SIP trunks | ElevenLabs adds no carrier markup; normal Agent minute + provider fee | Language quality follows selected agent pipeline | External provider cost separate; SIP/Twilio configuration required | https://elevenlabs.io/docs/eleven-agents/customization/integrations/twilio ; https://elevenlabs.io/pricing/agents citeturn944199search4turn698578view1 |
| **WhatsApp Business messaging/calling** | Native WhatsApp Business voice calls, messages, voice notes/media | Meta WhatsApp Business + ElevenAgents | Agent minute/audio processing + Meta charges | RU supported through underlying models. `[single-source]` | Outbound calls require user permission via approved Meta flow/template; ZRM has limitations; Meta billed separately | https://elevenlabs.io/docs/eleven-agents/whatsapp ; https://elevenlabs.io/docs/eleven-agents/whatsapp/troubleshooting citeturn944199search0turn944199search1 |
| **Batch outbound campaigns** | Bulk outbound phone/WhatsApp campaigns, retries, callbacks and stop-calling handling | ElevenAgents Campaign Manager | normal call-minute + carrier/Meta | Language follows agent | Regulated; caller responsible for jurisdictional consent; WhatsApp permission workflow supported | https://elevenlabs.io/docs/eleven-agents/phone-numbers/campaigns citeturn944199search17 |
| **Automatic Dubbing v2** | Translates audio/video while preserving voice, tone, pacing and background audio | **Dubbing v2 Alpha** | **$2.20/min API** | Russian among 90+/92 supported languages, but **no credible independent RU↔EN score found**. `[single-source]` quality claims. | app up to 1 GB/180 min; API up to 3 GB; 32 speakers; no realtime; paid v2 has no watermark; API transcript editing/regeneration Enterprise-only | https://elevenlabs.io/docs/overview/capabilities/dubbing ; https://elevenlabs.io/pricing/api citeturn528061search8turn603799view4 |
| **Dubbing v1 / Dubbing Studio** | Legacy editable dubbing workflow with per-clip transcript/speaker control | Dubbing v1 | **$0.33/min watermarked**, **$0.50/min unwatermarked / Studio** | RU supported; v1 is cheaper but older and less performance-preserving than v2 according to ElevenLabs. `[single-source]` | Dubbing Studio up to 1 GB/45 min; **maintenance mode** | https://elevenlabs.io/docs/overview/capabilities/dubbing ; https://elevenlabs.io/pricing/api citeturn528061search8turn603799view4 |
| **Lip-sync** | Syncs mouth movement to generated speech in image/video workflows | Third-party image/video models inside ElevenCreative; **not part of Dubbing** | Model-dependent Image & Video pricing | Language mainly depends on source speech/TTS; no RU benchmark found | ElevenLabs explicitly says Dubbing itself does not offer lip-sync | https://elevenlabs.io/docs/help-center/product/dubbing/do-you-offer-lip-sync-in-dubbing ; https://elevenlabs.io/docs/overview/capabilities/image-video citeturn638505search7turn637214search0 |
| **Studio / Projects** | Long-form audio/video authoring: audiobooks, podcasts, voiceovers, timeline editing, captions, Music/SFX/isolator | Studio 4.0 using underlying ElevenLabs models | consumes product credits | Russian supported where underlying TTS/STT supports it | Free has 3 projects; Starter 20; advanced limits plan-dependent | https://elevenlabs.io/studio ; https://elevenlabs.io/docs/eleven-creative/audio-tools/voiceover-studio citeturn638505search9turn638505search11 |
| **GenFM** | Turns documents/URLs/projects into synthetic host/guest or bulletin podcasts | GenFM in Studio | paid-plan credits | Language availability tied to supported TTS; no reason to replace your NotebookLM pipeline | Paid plan required | https://elevenlabs.io/docs/help-center/product/distribution-publishing/gen-fm/how-do-i-use-gen-fm citeturn638505search10 |
| **Audio Native** | Embeddable “listen to article” player for websites | Studio/Audio Native | TTS/plan usage | Russian follows chosen voice/model | Web publishing workflow | https://elevenlabs.io/docs/eleven-creative/audio-tools/audio-native citeturn638505search12 |
| **ElevenReader** | Consumer reading/listening app for documents/articles | Eleven v4-based reader | Reader consumer plan/app economics, distinct from API | 90+ languages, therefore Russian supported; no quality benchmark specific to Reader | iOS/Android/web; iOS requirements apply | https://elevenlabs.io/docs/help-center/product/mobile-apps/elevenreader/what-is-elevenreader ; https://elevenlabs.io/docs/help-center/product/mobile-apps/elevenreader/where-is-elevenreader-available citeturn637214search9turn637214search10 |
| **Sound Effects** | Text-to-audio effects, ambiences and designed sounds | **eleven_text_to_sound_v3**; v2 deprecated for 2027-03-01 | API page **$0.12** unit; Creative ~200 credits/generation | Language-independent prompts; Russian prompt quality not benchmarked | controllable duration; v3 recommended | https://elevenlabs.io/docs/overview/capabilities/sound-effects ; https://elevenlabs.io/pricing/api citeturn528061search6turn603799view3 |
| **Eleven Music** | Prompted music generation and editing | **Music v2.5** current | **$0.15/min API**; ~900 Creative credits/min | Not a speech-language quality issue | commercial use governed by Music Terms/plan; restrictions vary by distribution/use category | https://elevenlabs.io/blog/music-v2-5-model ; https://elevenlabs.io/docs/overview/capabilities/music ; https://elevenlabs.io/music-terms citeturn637214search13turn637214search23turn986727view7 |
| **Music Marketplace** | Licensable AI-assisted tracks from creators | Marketplace licensing | license tier rather than pure API consumption | N/A | social/paid/offline tiers; larger TV/cinema/streaming uses may require enterprise arrangements | https://elevenlabs.io/blog/introducing-the-music-marketplace-in-elevencreative ; https://elevenlabs.io/docs/overview/capabilities/music/marketplace citeturn637214search25turn637214search38 |
| **Voice Isolator** | Removes background noise and isolates speech | Voice Isolator | **$0.12/min API**; ~1,000 credits/min | Useful before Russian STT or PVC capture; language-independent | max ~500 MB / 1h in Creative workflow | https://elevenlabs.io/docs/eleven-creative/audio-tools/voice-isolator ; https://elevenlabs.io/pricing/api citeturn638505search8turn603799view3 |
| **Forced Alignment** | Aligns known text to an audio recording, returning timing alignment | Forced Alignment endpoint | **[no source found]** for a separate explicit price line on current pricing page | Potentially useful for RU subtitle alignment; no RU benchmark found | Requires text + audio; not an ASR substitute | https://elevenlabs.io/docs/eleven-api/guides/cookbooks/forced-alignment citeturn638505search13 |
| **Voice Library / Voice Marketplace** | Public catalog of human-verified PVC voices; creators earn when paid users generate with them | PVC only | Customer pays normal generation price/multiplier; creator reward depends usage/terms | Russian voices can be listed; market depth not independently benchmarked | IVC/Voice Design cannot be published; Voice CAPTCHA, policy review, payout via Stripe Connect | https://elevenlabs.io/docs/eleven-creative/voices/voice-library/ ; https://elevenlabs.io/docs/eleven-creative/voices/payouts citeturn844860search2turn844860search0 |
| **Iconic Marketplace** | Permissioned/licensed well-known voice/persona/IP collaborations | “Iconic” licensed voices/content | deal/use dependent | Depends on individual asset | Curated rights, not an unrestricted public-figure cloning system | https://elevenlabs.io/blog/introducing-the-eleven-album citeturn637214search17 |
| **Image & Video** | Text/reference image generation, video generation, upscaling, lip-sync; ElevenLabs acts as creative orchestration surface over multiple models | Third-party/current image/video models exposed in ElevenCreative/API | per model/generation; video paid; API Pro+ | N/A for Russian except text prompt/model behavior | free image quota reported as 3 requests/day; video paid; some models geographically restricted | https://elevenlabs.io/docs/overview/capabilities/image-video ; https://elevenlabs.io/blog/introducing-the-image-video-and-templates-apis citeturn637214search0turn637214search42 |
| **Flows / Templates** | Visual node graph for chaining speech, Music, SFX, Voice Changer, Isolator, image/video | ElevenCreative Flows | underlying node billing | Depends on components | Suitable for repeatable content pipelines; up to 1M grant credits advertised for Image/Video/Flows grants | https://elevenlabs.io/docs/eleven-creative/products/flows ; https://elevenlabs.io/docs/eleven-creative/playground/image-video citeturn638505search4turn637214search2 |
| **Productions** | Managed human-in-the-loop dubbing/localization/audio production | Human translators/casting/mix + ElevenLabs models | custom/enterprise | Potentially highest-quality localization but economically irrelevant at your current scale | contact sales / managed | https://elevenlabs.io/docs/overview/capabilities/dubbing citeturn528061search8 |
| **Hosted MCP server** | Lets Claude, Claude Code, ChatGPT, Cursor and other MCP clients manage agents and create media | `https://api.elevenlabs.io/v1/mcp`, OAuth | ordinary downstream product charges | N/A | Workspace-scoped OAuth; agent-management and creative tools exposed | https://elevenlabs.io/mcp ; https://elevenlabs.io/docs/eleven-agents/operate/architect/external-assistants citeturn267184search9turn267184search4 |
| **Open-source MCP server** | Local/API-key implementation suitable for extension and PRs | `elevenlabs/elevenlabs-mcp` | ordinary API charges | N/A | public GitHub repo | https://github.com/elevenlabs/elevenlabs-mcp citeturn935555search6 |
| **Official n8n node** | Native workflow nodes for TTS/STT/Agents | `elevenlabs/elevenlabs-n8n` | API usage | Directly relevant to current voice rail | Open source; n8n lists integration as ElevenLabs-created/verified | https://github.com/elevenlabs/elevenlabs-n8n/ ; https://n8n.io/integrations/elevenlabs/ citeturn935555search2turn267184search8 |
| **Zapier / Make / n8n agent integration** | Voice agents invoke automation workflows/tools | MCP/webhooks/API | Agent + external platform charges | N/A | Zapier advertises 7,000+ apps; Make/n8n use webhook/API patterns | https://elevenlabs.io/agents/integrations/zapier ; https://elevenlabs.io/agents/integrations/make ; https://elevenlabs.io/agents/integrations/n8n citeturn267184search5turn267184search6turn267184search7 |
| **SDKs / Agents packages / coding-agent plugin** | Client SDKs, React/mobile agent SDKs and coding-assistant skills | GitHub packages; ElevenLabs plugin | no separate software fee | N/A | Open source; useful contribution surface | https://github.com/elevenlabs/packages ; https://github.com/elevenlabs/plugin citeturn935555search25turn935555search24 |

### Russian STT evidence: what can and cannot be claimed

**[established]** There is **no current, independent, apples-to-apples Russian benchmark I found that simultaneously tests Scribe v3, Whisper large-v3, gpt-4o-transcribe, Deepgram Nova-3 and AssemblyAI's newest multilingual model on the same RU corpus**. Therefore a clean table such as “Scribe v3 = X%, OpenAI = Y%, AssemblyAI = Z% in Russian” would be invented precision.

The strongest current evidence splits into three incompatible benchmark families. Artificial Analysis tests many production STT APIs identically and has Scribe v3 around **1.9% aggregate AA-WER**, but its score should not be read as Russian WER. citeturn237891search22 A small independent Russian technical-speech study from August 2026 found **OpenAI GPT Transcribe substantially stronger than Scribe v2 and Deepgram Nova-3 on English/IT terminology embedded in Russian**, but it predates Scribe v3 and uses a specialized scoring approach rather than conventional broad-corpus WER. `[low-authority]` (2026-08, https://gist.github.com/dannote/1c4e14b88af811db0639b68acdc7769d) citeturn237891search9 A separate hands-on 20-voice-memo benchmark found Deepgram Nova-3 worse than Nova-2 in its Russian subset, again small and predating Scribe v3. `[low-authority]` (accessed 2026-11-20, https://github.com/Smolevich/stt-benchmarks) citeturn237891search10

For your actual distribution—Russian dictation with English technical nouns—the correct answer is therefore empirical: preserve 200–500 real Telegram utterances with manually corrected references and measure normalized WER, technical-entity error rate, number/code accuracy, punctuation utility and downstream coding-agent correction rate. That dataset would itself be a publishable artifact.

## C. Pricing table and the three cost profiles computed

### Current ElevenCreative subscription table

Official pricing was read **20 November 2026**. The page was simultaneously advertising temporary first-month/annual Black Friday discounts; the table below uses the recurring monthly price. [single-source] (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/pricing) citeturn603799view0turn603799view1

| Plan | Recurring monthly price | Monthly credits | Commercial rights / cloning | TTS UI approx minutes | TTS extra-minute indication | Concurrent TTS requests | Notable limits |
|---|---:|---:|---|---:|---:|---:|---|
| Free | $0 | 10,000 | limited commercial use; no IVC/PVC | ~10 | ~$0.36 equivalent | 2 / limited | 3 voice slots; 3 Studio projects |
| Starter | $6 | 30,000 | commercial; **IVC** | ~30 | ~$0.20 | 3 | 10 custom voice slots |
| Creator | $22 | 121,000 | commercial; IVC + **1 PVC** | ~121 | ~$0.18 | 5 | 30 slots |
| Pro | $99 | 600,000 | commercial; 1 PVC | ~600 | ~$0.17 | 10 | 44.1-kHz PCM API, 192-kbps audio |
| Scale | $299 | 1.8M | commercial; 3 PVC | ~1,800 | ~$0.17 | 15 | 3 seats, collaboration |
| Business | $990 | 6M | commercial; 10 PVC | ~6,000 | ~$0.17 | 25 | 10 seats |
| Enterprise | custom | custom | custom | custom | negotiated | elevated | DPA/SLA, SSO, HIPAA BAA, discounts, priority support |

The Creative pool currently approximates **TTS 1 credit/character; STT 330 credits/min; Music 900/min; SFX 200/generation; Voice Changer and Isolator 1,000/min; legacy Dubbing 2,000/min watermarked, 3,000/min unwatermarked, Dubbing Studio 5,000/10,000 per minute depending watermark**. These credit figures coexist with the API's dollar-denominated metering; API usage is explicitly described as billed in USD rather than credits. [single-source] (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/pricing; https://elevenlabs.io/pricing/api) citeturn459285view0turn603799view3

### API list rates that matter to the lab

| API | Current list rate | Important extras |
|---|---:|---|
| TTS Multilingual / quality-class | **$0.08 / 1k chars** | pricing page maps roughly 1,000 chars ≈ 1 minute in its examples |
| TTS Flash/Turbo | **$0.04 / 1k chars** | ~75 ms stated on pricing page |
| Scribe v3 batch | **$0.20 / h** | keyterms +$0.05/h; transcript edit +$0.07/h |
| Scribe v3 Live | **$0.22 / h** | keyterms +$0.08/h; keyterm library +$0.08/h; edit +$0.12/h |
| Realtime v1 Mini/Max | **$0.08 connected audio/min** | separate input/output-token charges |
| Voice Changer | **$0.12/min** | — |
| Voice Isolator | **$0.12/min** | — |
| Music | **$0.15/min** | fine-tuning listed separately at $1.50 unit on API page |
| Dubbing v1 | **$0.33/min watermarked; $0.50/min unwatermarked** | legacy |
| Dubbing v2 | **$2.20/min** | 92 languages |
| ElevenAgents additional call time | **$0.08/min** | LLM billed separately; telephony carrier at cost |
| ElevenAgents burst | **$0.16/min** | only when exceeding subscription concurrency |
| Agent SMS | **$0.003/message** | carrier/channel costs may still apply |

Source: [single-source] ElevenLabs API and Agents pricing, accessed 2026-11-20: https://elevenlabs.io/pricing/api and https://elevenlabs.io/pricing/agents. citeturn603799view2turn603799view3turn603799view4turn698578view1

### Cost profile: STT only, 25 hours/month of Russian audio

**API metering: 25 h × $0.20/h = $5.00/month.** The recurring Starter plan is $6 and its published STT-equivalent allocation is 30 hours, so the lowest *subscription plan* that comfortably covers 25 hours is **Starter $6/month**, while pure API pay-as-you-go arithmetic is **$5**. [established] (ElevenLabs API Pricing, accessed 2026-11-20, https://elevenlabs.io/pricing/api) citeturn603799view2

For your current rail, this makes Scribe almost a rounding error. Removing it to save roughly $5/month gives up model diversity against both OpenAI and local Whisper; that is poor engineering economics unless the dual-transcript disagreement rate is shown to be near zero.

### Cost profile: 25 h STT + 60 min cloned TTS + 60 min dubbing

Using current list rates and the pricing page's approximate mapping of **1k chars ≈ 1 minute** for Flash/Turbo:

25 h Scribe v3 = **$5.00**.
60 min Flash/Turbo TTS ≈ 60k chars × $0.04/1k = **$2.40**.
60 min Dubbing v2 = 60 × $2.20 = **$132.00**.
**Metered total ≈ $139.40/month**, before taxes. citeturn603799view2turn603799view3turn603799view4

If “cloned voice” means **IVC**, Starter is the minimum feature tier; if it means **PVC**, Creator is the minimum. The current pages do not explain the mixed subscription/API invoice mechanics clearly enough for me to assert that you simply add $6 or $22 to the above in every billing configuration, so the safe budgeting statement is: **$139.40 of list-rate metered workload plus the minimum cloning entitlement you require**. `[no source found]` for a universal cross-product invoice formula.

The dominant cost is Dubbing v2: **94.7% of the $139.40 workload**. Therefore optimizing TTS by fractions of a cent is irrelevant while dubbing 60 min/month.

### Cost profile: voice agent with 300 call-minutes/month

The Agents page gives Free 15 min, Starter 75, Creator 275, then **$0.08 per additional minute** on each. LLM and external carrier are extra. citeturn698578view0turn698578view1

On raw cash cost:

- Free/PAYG: 15 included + 285 × $0.08 = **$22.80**.
- Starter: $6 + 225 × $0.08 = **$24.00**.
- Creator: $22 + 25 × $0.08 = **$24.00**.
- Pro: $99, with 1,238 minutes included, so uneconomic at 300 minutes.

For a **commercial public agent**, Starter is the sensible minimum because it explicitly includes a commercial license; for an internal self-reminder/coding interface, Free/PAYG is the cheapest arithmetic. [established] (ElevenLabs Agents Pricing, accessed 2026-11-20, https://elevenlabs.io/pricing/agents) citeturn698578view0

## D. Grants, programs, hiring and open-source doors

### Startup Grants

**[established] [single-source]** The Startup Grants program currently advertises **33,000,000 characters**, **more than 680 hours of Agents**, **more than $5,500 stated value**, and **12 months** of access plus founder/community events. Applications are rolling and the page says applicants are told whether they are accepted “within one week.” (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/startup-grants) citeturn603799view7turn698578view2

Eligibility is unusually specific: there must be a **monetized product use case/business model**, a valid business email, a durable product rather than a one-off campaign, fewer than **25 employees**, and the applicant cannot be an agency, consulting firm or ordinary SMB. Existing paying self-serve customers may apply; enterprise customers may not. One application per company. There is **no published acceptance rate**. [no source found] for “acceptance reality” beyond the program's own page. citeturn698578view2

For Palo Alto AI Research Lab the application should therefore not say “we want free credits to experiment.” The strongest truthful framing is a product such as **VoiceRail / AgentBridge: an RU/EN voice interface for autonomous coding agents**, with an external-user path, open-source client, and paid hosted version. That directly satisfies productization intent better than “research lab” or “consulting.”

### Impact

**[established] [single-source]** The Impact Program gives free access to registered nonprofits, people with accessibility needs, professors and students; nonprofit examples receive extended free access, and the page specifically describes education/research-style uses. It is only relevant if the lab has a qualifying academic/nonprofit relationship—do not distort corporate status to fit it. (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/impact) citeturn812944view6

### Affiliate and ambassador

**[established]** Affiliate terms updated **10 June 2026** pay **22%** of pre-tax purchases for Starter, Creator, Pro and Scale and **11%** for Business, for the referred paying subscriber's first **12 months**; Enterprise is excluded and payments lag by roughly three months. Automation/self-referral abuse is prohibited. [single-source] (ElevenLabs, 2026-06-10, https://elevenlabs.io/affiliates-terms) citeturn698578view3

**[established]** Your older internal note that Community Builder was paused is now stale. The live Ambassador page exposes both **Community Builder** and **Content Creator** tracks; applicants must be 18+, have an active ElevenLabs account, a genuine audience/community, a real plan, and show something they already made with ElevenLabs. Perks include early access, product support, credits/event support and other opportunities; current terms additionally define creator-partner reward mechanisms. [single-source] (ElevenLabs, accessed 2026-11-20, https://elevenlabs.io/ambassador; Ambassador Terms updated 2026-11-17, https://elevenlabs.io/ambassador-terms) citeturn812944view4turn444058search1

The implication is actionable: **do not apply before publishing one technical artifact**. A Russian STT benchmark plus n8n workflow is materially stronger evidence than another promotional tutorial.

### Hackathons and developer community

ElevenLabs has run a worldwide hackathon and a season of recurring developer hackathons, and its current site exposes events, Discord, GitHub and developer social channels. These are useful distribution surfaces, but past hackathon prize pools should not be treated as currently available cash unless a live event explicitly says so. `[single-source]` (https://hackathon.elevenlabs.io/; https://hacks.elevenlabs.io/; accessed 2026-11-20) citeturn935555search3turn935555search17

### Hiring doors

**[established]** Current careers listings include **Forward Deployed Engineer — Software Engineer — USA**, **Enterprise Solutions Engineer — USA**, and **Developer Experience Engineer**, among many other roles. (ElevenLabs Careers, accessed 2026-11-20, https://elevenlabs.io/careers/positions) citeturn844860search8

The **Developer Experience Engineer** role is unusually aligned with your stated goal: it explicitly calls for building/maintaining open-source **SDKs and the MCP server**, improving APIs and developer tooling, acting as “customer zero,” writing technical content, triaging developer issues and moving across multiple languages/stacks. The role is remote across Europe/US and names London, New York, San Francisco and Warsaw as offices. [single-source] (https://elevenlabs.io/careers/d16a7ad6-ffbd-4655-8716-4872263f99ce/developer-experience-engineer, accessed 2026-11-20) citeturn844860search7

The **FDE** role emphasizes embedding with strategic customers, designing bespoke integrations and owning difficult projects end-to-end rather than merely demoing APIs. [single-source] (https://elevenlabs.io/careers/6c4c57c1-ec72-42ba-af3a-eb7aebbde2e6/forward-deployed-engineer-software-engineer-usa, accessed 2026-11-20) citeturn986727view0 The **Enterprise Solutions Engineer** role sits closer to customer technical architecture and the sales cycle. [single-source] (https://elevenlabs.io/careers/275f43d0-b62d-401d-830c-7c1ac0e688aa/enterprise-solutions-engineer-usa, accessed 2026-11-20) citeturn986727view1

The hiring bar inferred from those job descriptions is: **ship across the stack, debug customer problems, communicate technically, contribute to open source, and demonstrate high agency**. That is an inference from the job specs, not a published interview rubric.

### Best open-source PR targets

| Surface | Why it is useful | Concrete PR that is plausibly meaningful | Prior art |
|---|---|---|---|
| `elevenlabs/elevenlabs-n8n` | Directly overlaps your production rail | Add robust **Scribe v3 Live streaming**, diarization/event outputs, transcript-edit/keyterm-library options, retry/back-pressure behavior and RU/EN workflow example | Official repo exists; third-party `n8n-nodes-elevenlabs` also exists. https://github.com/elevenlabs/elevenlabs-n8n/ ; https://github.com/n8n-ninja/n8n-nodes-elevenlabs citeturn935555search2turn935555search36 |
| `elevenlabs/elevenlabs-mcp` | Direct match to DevEx role and coding-agent fleet | Add a **voice-to-coding-session toolchain example**, observability/cost estimator, or agent-eval commands | Official MCP exists; no exact “RU voice rail → multi-agent coding session” official example found. https://github.com/elevenlabs/elevenlabs-mcp citeturn935555search6 |
| `elevenlabs/packages` | React/mobile SDKs and Agents infrastructure | Repro/fix realtime reconnect, tool-callback or multilingual telemetry issues with tests | Official packages repo. https://github.com/elevenlabs/packages citeturn935555search25 |
| `elevenlabs/plugin` | Coding-agent distribution path | Add a benchmark/debugging skill that diagnoses agent latency, call cost and STT disagreements | Official plugin already bundles ElevenLabs agent skills. https://github.com/elevenlabs/plugin citeturn935555search24 |
| Independent benchmark repo | Not constrained by vendor roadmap | `ru-voicebench`: real RU technical dictation + English terms + numbers + diarization + TTS speaker-sim | Small RU benchmarks exist, but **prior art not found** for a current Scribe-v3/OpenAI/AssemblyAI/Deepgram/local unified benchmark. Searched GitHub, general web and benchmark sites; closest were `Smolevich/stt-benchmarks` and dannote's Russian technical STT gist. citeturn237891search9turn237891search10 |

## E. Use case table

Monthly costs are order-of-magnitude estimates at the scale in the brief, excluding taxes and third-party carrier/LLM charges unless stated. “Wins” means it is the best choice for **this lab**, not universally.

| Use case | Mechanism / exact products | Value within 90 days | Monthly cost at your scale | What isneeded to start | Best non-ElevenLabs alternative | Verdict |
|---|---|---|---:|---|---|---|
| **Existing voice rail: second ASR** | Keep Scribe v3 batch beside OpenAI/local transcript; compare and normalize | **High** — protects coding commands from transcription failures for ~$5 | **~$5** for 25 h | Change model to `scribe_v3`; log disagreement and final correction | local Whisper large-v3 on RTX 5060 Ti; OpenAI GPT Transcribe | **ElevenLabs wins as the second engine, loses as sole engine.** Scribe v3 is cheap and independently strong overall; local Whisper is still marginal-cost-free. citeturn603799view2turn237891search22 |
| **Live dictation** | Scribe v3 Live WebSocket → partial/final transcript → agent router | **Medium-high** — less friction than Telegram-upload round trip | **~$5.50** for 25 h realtime | small desktop/mobile capture client; endpointing; interruption handling | Deepgram Nova/Flux, AssemblyAI streaming, OpenAI Realtime, local faster-whisper | **Tie until benchmarked in your RU technical domain.** v3 Live is only ten days old and strong vendor-reported, but evidence is still early. citeturn528061search1turn603799view2 |
| **Multi-speaker calls / Plaud / Omi** | Scribe v3 diarization + word timestamps + speaker labels | **High** if interviews/meetings are frequent | $0.20/h | upload audio, map speakers, summarize downstream | AssemblyAI, Deepgram, WhisperX/Pyannote local | **ElevenLabs wins on integration simplicity; benchmark diarization before replacing specialized pipelines.** v3 supports up to 32 speakers and ElevenLabs reports 17% fewer diarization errors vs v2 in 3+ speaker recordings. `[single-source]` citeturn528061search1 |
| **Operator IVC for quick experiments** | 1–2 min clean RU/EN sample → IVC → v4/Flash | **Medium** | Starter $6 + generation; 60 min Flash ≈ $2.40 | quiet sample, consent/self-ownership, A/B against real voice | Chatterbox Multilingual, XTTS-v2, F5-TTS | **Tie.** ElevenLabs is vastly easier/hosted; local alternatives can be zero marginal cost and private. |
| **Operator PVC / long-term digital twin** | 30–180 min curated recordings → verified PVC → v4 | **High** if used publicly in RU+EN | Creator $22 + usage | record clean corpus containing RU and accented EN; Voice CAPTCHA | Chatterbox, F5-TTS, XTTS, proprietary MiniMax/Cartesia voices | **ElevenLabs wins for hosted fidelity/operational simplicity; loses for privacy/lock-in.** Current TTS quality is top-tier, but RU cross-lingual identity needs your own ABX test. citeturn638505search0turn859798search0 |
| **Daily RU/EN narration / Shorts** | PVC + Flash/v4 depending importance | **High** if it increases distribution cadence | ~cents/day to a few dollars/month for short clips | narration template + subtitle/render pipeline | Kokoro/Piper for generic voices; F5/Chatterbox for clone | **ElevenLabs wins for final public assets; local wins for drafts.** |
| **Job-application voice/video intro** | PVC TTS or Voice Changer + HeyGen/Higgsfield/video layer | **High but low volume** — differentiation if tasteful | <$1 in ElevenLabs speech for dozens of short intros; video vendor dominates | 20–45 sec script per role; explicit “AI-assisted voice” if potentially misleading | HeyGen native voice, real recording | **Tie.** A real personalized recording may signal authenticity better; clone helps scale personalization. |
| **Voice replies to leads** | PVC TTS or speech-to-speech from operator rough recording | **Medium-high** | ~$0.04–0.12/min speech | Telegram/CRM bot, queue for operator approval | real voice note; Cartesia/OpenAI TTS | **ElevenLabs wins when prosody matters; real voice still wins for high-value leads.** |
| **Inbound lead qualification** | ElevenAgent + KB + CRM/n8n tool + Twilio/SIP/WhatsApp | **High** — directly tied to revenue | ~300 min workload ≈ $23–24 ElevenLabs + LLM/carrier | number, disclosure, lead schema, n8n webhook, safe handoff | Vapi/Retell/Bland + OpenAI/Cartesia/Deepgram | **Tie to ElevenLabs win.** ElevenLabs now has the integrated stack; Vapi/Retell may be more flexible as orchestration layers. Must benchmark latency and call completion. citeturn698578view1turn944199search0 |
| **Self reminder robocall 60/5 min before meetings** | Calendar → n8n → outbound Agent/Twilio call to your own number → spoken lead brief | **High** — tiny cost, immediate execution benefit | likely **<$1–2/month** at low minutes + carrier | your own consent; calendar webhook; Twilio/SIP | Twilio + OpenAI TTS; local generated WAV | **ElevenLabs wins operationally.** Legal risk is low when you are calling yourself, but carrier anti-spam behavior still matters. |
| **Public AI receptionist** | ElevenAgent, public number/WhatsApp, KB/RAG, scheduling/CRM tools | **High** if leads exist | $0.08/min above allowance + LLM/carrier | disclosures, opt-out/handoff, logging policy, business-hours rules | Retell, Vapi, Bland, OpenAI Realtime | **Tie.** ElevenLabs has vertically integrated voice quality; Retell/Vapi can make provider swapping easier. |
| **Mock interview hiring-manager** | Agent persona + role/JD KB + transcript scoring + custom tools | **High** for 100-interview target | essentially Free/Starter for tens of sessions; internal browser calls avoid carrier | job descriptions, score rubric, post-call critique | Gemini Live, OpenAI Realtime, Hume | **ElevenLabs wins if you want persistent clone/persona + telemetry; OpenAI/Gemini may win if reasoning quality dominates speech.** |
| **“Call my coding agents”** | Phone/web Agent → authenticated MCP/n8n tools → create/status/cancel Claude Code/Codex/Cursor sessions → spoken summary | **Very high** — unique demo + personal leverage + hiring artifact | likely **<$10–25** at experimental usage | secure tool gateway, allowlist, confirmation for destructive actions, MCP | OpenAI Realtime + custom tool server; Gemini Live | **ElevenLabs wins as demo/integration layer.** Hosted MCP, n8n and telephony make this unusually fast to ship. citeturn267184search4turn267184search7 |
| **Dubbing NotebookLM RU→EN / EN→RU** | Download Notebook audio → Dubbing v2 → target language track | **Low-medium** until source content has traction | **$2.20/min**; 60 min = $132 | source rights; test 5–10 min first | Rask, HeyGen, Descript; DIY translate+TTS | **ElevenLabs loses on current ROI.** v2 is expensive and RU pair lacks independent validation. Use only on proven episodes. citeturn603799view4 |
| **Audio versions of long reads** | Audio Native + PVC/v4 or Multilingual v2 | **Medium** | roughly $0.04–0.08 per ~1k chars API-equivalent | article CMS hook | browser TTS, Piper, Kokoro | **ElevenLabs loses for low-value bulk; wins for flagship articles.** |
| **NotebookLM replacement for listening** | GenFM | **Low / intentionally excluded** | paid credits | none | **NotebookLM** | **ElevenLabs loses.** Existing requirement says NotebookLM stays; no reason to duplicate. |
| **Sound design for shorts/explainers** | SFX v3 in Studio/Flows | **Medium** | ~$0.12 API generation class / credit usage | prompts + renderer | Stable Audio/open libraries | **Tie.** Convenient if already in ElevenCreative; unnecessary vendor dependency otherwise. citeturn528061search6 |
| **Music for videos** | Music v2.5 or Marketplace track | **Medium** if content volume rises | $0.15/min generated plus licensing constraints by use | maintain license record per asset | Suno/Udio/licensed stock/local generative | **Tie.** Rights handling is more explicit than many generative alternatives, but plan/use restrictions must be checked per release. citeturn637214search13turn986727view7 |
| **Audiobook from book project** | Studio + PVC/v4/Multilingual v2 | **Medium** after manuscript and distribution exist | depends on word count; typically tens of dollars, not hundreds, at API TTS rates | chapter preprocessing, pronunciation dictionary, QC | real narrator; local F5/XTTS; Descript | **ElevenLabs wins for prototype, tie for final commercial book** because human narration remains a premium authenticity option. |
| **Interview transcription + analysis** | Scribe v3 batch → diarization → LLM rubric | **Very high** for hiring objective | pennies per interview hour; 100×1 h ≈ **$20 STT** | consent, recorder, automatic analysis template | local WhisperX/Pyannote; OpenAI | **ElevenLabs wins as a cloud second opinion; local should remain primary/parallel for confidentiality.** |
| **Voice CV page** | embedded short PVC clip or Audio Native | **Medium-high** | negligible | 30-sec script, disclosure, CTA | real audio recording | **Tie.** A real clip is more authentic; clone demonstrates product ability. |
| **Independent Russian STT benchmark** | test Scribe v3/v3 Live vs GPT transcribe, Whisper, Deepgram, AssemblyAI, local models on your own real utterances | **Very high** — rare knowledge + public visibility + direct hiring signal | **<$10–30** for many evaluation hours | 500–2,000 labeled utterances, reproducible harness | none; this *is* comparative infrastructure | **ElevenLabs wins as one important benchmark target, not as exclusive stack.** Prior art exists only partially. citeturn237891search9turn237891search10 |
| **Russian TTS/voice-clone benchmark** | ABX/MOS + speaker similarity + pronunciation errors on v4/PVC vs F5/Chatterbox/XTTS/Kokoro | **Very high** | Creator $22 + small inference spend; local inference free | 10–20 speakers or operator-only N=1 first; native raters | local models themselves | **Best research artifact; no vendor verdict until measured.** |
| **n8n Scribe v3 Live node/example** | upstream PR + sample realtime dictation workflow | **Very high** for ElevenLabs-specific visibility | $0 except test calls | fork, tests, issue/PR | custom HTTP/WebSocket node | **ElevenLabs ecosystem wins** because upstream contribution is the goal. |
| **Voice-Agent latency observatory** | instrument STT first-token, end-of-turn, LLM first-token, TTS first-byte, interruption recovery, call RTT | **Very high** for FDE/DevEx credibility | <$25/month test traffic | trace IDs + synthetic call runner | Vapi/Retell/OpenAI/Gemini as competitors | **Tie by design:** publish provider-neutral measurements. |

### NotebookLM dubbing rights

**[established]** Google's current Gemini Notebook help says Audio Overviews can be downloaded and shared, while Google's terms state that Google does **not claim ownership** over original generated content. However, users remain responsible for having rights to source material and respecting copyright. Therefore dubbing your own NotebookLM output is not inherently barred by the product terms I found; the legal constraint is the rights in the **source material and resulting use**, not merely the fact that NotebookLM generated the audio. (Google Gemini Notebook Help, accessed 2026-11-20, https://support.google.com/gemininotebook/answer/16212820; Google Terms effective 2026-07-30, https://policies.google.com/terms?hl=en-US) citeturn473057search0turn473057search2

There is one important qualification: Google expressly prohibits misleading others about AI provenance and says generated AI content cannot be used to violate IP/privacy rights; Notebook help repeats that users should not share copyrighted material without necessary rights. citeturn473057search2turn473057search4 So for a public dubbed NotebookLM podcast, preserve a simple disclosure such as “AI-generated/AI-dubbed audio based on our own source material.” This is a compliance recommendation, not a claim that Google mandates exactly that sentence.

## F. Contradictions and debates

### Flash pricing: $0.04/1k vs $0.15–0.30/1k

**Resolution: [established].** The official API page read on 2026-11-20 says **Flash/Turbo = $0.04 per 1,000 characters** and Multilingual = $0.08/1k. The same page's calculator equates $0.04/1k to roughly $0.04/min under its text-length approximation. citeturn603799view3 The older $0.15–0.30 figure does not match current first-party ElevenLabs list pricing; I found no first-party evidence supporting it. **[no source found]** for the previously recorded “vendor leg,” so it should be treated as reseller markup, bundled application pricing or stale data until the original vendor is identified.

### Creative credits versus API dollars

ElevenLabs simultaneously publishes a **shared Creative credit economy** and **USD-denominated API unit pricing**. Creative says STT ≈330 credits/min and TTS ≈1 credit/character; API says Scribe $0.20/h and Flash $0.04/1k chars. citeturn459285view0turn603799view2 This is not a numerical contradiction; they are two billing surfaces. It does, however, make mixed-product cost forecasting less intuitive. For engineering estimates, use the API price page; for UI/Studio workflows, use credits.

### Scribe “best” versus Russian technical dictation

**Pro-Scribe steelman:** current independent aggregate benchmarking places Scribe v3 at about **1.9% AA-WER**, and ElevenLabs says v3 reduces errors by 27% against v2 across 102 FLEURS languages. citeturn237891search22turn528061search1

**Counter-case:** the best independent Russian evidence I found either predates v3 or uses narrow technical datasets. One August 2026 study found OpenAI GPT Transcribe substantially better than Scribe v2 for embedded IT terminology, while a small RU/EN memo set found poor Russian results for Deepgram Nova-3. `[low-authority]` citeturn237891search9turn237891search10 Therefore “Scribe v3 is the best Russian ASR” is **not established**. The experiment that settles it for you is cheap: benchmark v3 on your own saved Telegram corpus.

### Eleven v4 “#1” versus Russian voice cloning

**[established]** v4 has independent evidence for broad listener preference, and ElevenLabs' v4 launch says roughly three-quarters of its blind head-to-head listeners preferred v4 over competitors. The latter is vendor data; Artificial Analysis provides independent support for top-tier general quality. citeturn528061search0turn859798search0

**[emerging]** That does **not** prove PVC is #1 for a Russian native speaking accented English. Speaker similarity, accent retention, code-switching and Russian stress are different failure modes. No current neutral RU PVC benchmark was found. This gap is strategically useful: measuring it would produce rarer public knowledge than another subjective demo.

### Dubbing v2 quality versus price

ElevenLabs claims Dubbing v2 preserves source performance and emotion across 90+ languages by conditioning on the original performance rather than transcript alone. [single-source] citeturn528061search2 The steelman is that $2.20/min can be cheap versus professional human dubbing. The counterargument for **your** lab is opportunity cost: a 60-minute episode costs $132 before distribution has proved that an English or Russian dub will generate interviews, leads or revenue. No independent RU↔EN quality benchmark strong enough to resolve the tradeoff was found.

### Agents as integrated platform versus modular Vapi/Retell stack

The ElevenLabs case is integration: STT, TTS, orchestration, KB/RAG, Twilio/SIP, WhatsApp, campaigns, testing and MCP all sit in one platform, and external carrier charges are passed through rather than marked up. citeturn919106search8turn698578view1 The modular-stack case is anti-lock-in: Vapi/Retell/custom Realtime systems can swap STT/TTS/LLM providers individually and may expose more granular latency controls. Which wins depends on whether your bottleneck is **ship speed** or **vendor optionality**. For a two-person lab over 90 days, ship speed currently dominates.

## G. Failure modes and risks

### Data retention, training and zero-retention

**[established]** ElevenLabs states that it retains data by default. **Zero Retention Mode is an Enterprise feature available only to select customers and eligible API endpoints**, and web-UI traffic is not covered. Voice-clone creation and Dubbing require retained data and are specifically shown as not ZRM-compatible. (ElevenLabs docs, accessed 2026-11-20, https://elevenlabs.io/docs/eleven-api/resources/zero-retention-mode) citeturn812944view7

For non-Enterprise accounts, the privacy policy permits users to **opt out of model-training use** through Data Use settings, but the opt-out is prospective; past use is not retroactively undone. Enterprise customer data is handled under separate business arrangements. (Privacy Policy updated 2026-10-09, https://elevenlabs.io/privacy-policy; EU Terms, updated 2026-10-09, https://elevenlabs.io/terms-of-use-eu) citeturn812944view8turn812944view9

**Recommendation:** keep source-code secrets, credentials, unreleased customer material and sensitive interview recordings out of web UI; use local ASR as the primary path for sensitive material. Do not treat a consumer Creator account as “zero retention.”

### Voice-clone consent and account-ban risk

ElevenLabs' current policy prohibits cloning or synthesizing a living person's voice without **explicit informed consent or other valid authorization**, prohibits unlawful automated calling, deception and impersonation, and prohibits circumventing voice-verification/Voice CAPTCHA systems. It explicitly reserves restrictions around higher-risk regulated uses. (Prohibited Use Policy, accessed 2026-11-20, https://elevenlabs.io/use-policy) citeturn812944view10

PVC is stricter than IVC in practice: official PVC docs state you can make a professional clone of **your own voice**, with verification; public Voice Library sharing is limited to qualifying PVCs. citeturn638505search0turn844860search2 Uploading synthetic-of-synthetic or deceptive voices can lead to removal or suspension; the Voice Library docs explicitly warn of permanent bans for guardrail evasion. citeturn844860search2

### EU AI Act Article 50

**[established]** Article 50 transparency rules have applied since **2 August 2026**. The European Commission's current guidance says systems designed for direct two-way interaction with natural persons must inform them that they are interacting with AI unless that is obvious; providers of systems generating synthetic audio/image/video/text must add machine-readable marking, and deployers must clearly disclose qualifying deepfakes at first exposure. (European Commission Q&A, 2026, https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act; press release 2026-07-31, https://digital-strategy.ec.europa.eu/en/news/commission-starts-enforcing-ai-act-rules-and-new-transparency-requirements-2-august) citeturn951565search0turn951565search2

For your public EU receptionist or cloned-voice persona, the low-cost implementation is explicit: **“You are speaking with an AI assistant for Palo Alto AI Research Lab; this call may be recorded…”** before substantive interaction. This is more conservative than trying to argue that the synthetic nature is “obvious.”

### United States TCPA / FCC

**[established; older source]** The FCC's February 2024 declaratory ruling held that AI-generated human voices fall within the TCPA category of **artificial or prerecorded voice**, so covered calls require prior express consent absent an exemption; telemarketing generally has stricter written-consent requirements, and prerecorded/artificial-voice messages have identification/opt-out obligations. The rule remains load-bearing even though the foundational ruling predates this report's 12–18 month target. (FCC, released 2024-02-08, https://docs.fcc.gov/public/attachments/FCC-24-17A1.pdf) citeturn951565search5

Do not infer that “ElevenLabs provides outbound campaigns” means cold AI voice prospecting is legally safe. The Campaign Manager docs themselves put regulatory compliance on the caller. citeturn944199search17 For **calls to yourself**, this risk is functionally minimal; for lead outreach, use affirmative opt-in or have counsel review the campaign.

### Portugal / EU call recording and marketing

**[established]** Portugal's consolidated electronic-communications/privacy law restricts listening/storage/interception of communications without required consent and imposes requirements for recording commercial communications; it also regulates unsolicited automated marketing communications and prior consent. (Diário da República, consolidated Law 41/2004, accessed 2026-11-20, https://diariodarepublica.pt/dr/legislacao-consolidada/lei/2004-106523049) citeturn919943search5

Operationally, separate three cases: **your own calls**; inbound calls where you disclose AI/recording at the start; and outbound marketing, which is the highest-risk class. Keep consent timestamp, number, disclosure version and opt-out state in your CRM.

### Reliability and latency

**[established]** Recent official status history shows this is not a theoretical risk. On **10 November 2026** there was a Scribe v3 Live outage; on **3–4 November** some EU ElevenAgent conversations using v4 Turbo had choppy/missing speech because generation fell below realtime under load; October also had endpoint timeouts, TTS latency and IVC creation failures. (ElevenLabs Status, accessed 2026-11-20, https://status.elevenlabs.io/history) citeturn812944view11

This specifically argues against replacing your local ASR with a single cloud path. Keep the existing local failover and add provider timeout/circuit-breaker logic to voice agents.

### Dubbing failure modes

Dubbing v2 is still described as **Alpha**. Current docs warn that output video is usually a preview, most commonly 1080p, does not necessarily preserve source resolution, and is at most stereo rather than 5.1; realtime dubbing is unavailable. citeturn528061search8 Higher cloning strength can retain more original accent but reduce target-language naturalness. That is exactly the tradeoff likely to matter for RU→EN operator content.

### Voice cloning in Russian and cross-lingual English

**[emerging]** IVC can underperform on unusual accents; PVC is the platform's higher-fidelity route. But current evidence is inadequate to promise that a Russian-native PVC speaking English will preserve “identity + desired accent + English pronunciation” simultaneously. The likely failure modes are stress errors in Russian, over-retention of Russian accent in English, phoneme drift on names/code terms, and speaker-similarity loss when pronunciation is forced toward a native English target. The deciding experiment is a bilingual 100-sentence ABX/MOS set, not marketing copy.

Among local competitors, a small open Russian benchmark reports materially mixed results: Chatterbox Multilingual can preserve identity but has digit/pronunciation issues; XTTS-v2 and F5-based Russian models vary strongly by checkpoint/license. `[low-authority]` (https://github.com/meshoklv/ru-speech-bench, accessed 2026-11-20) citeturn859798search3 Treat those as leads for your own testing, not definitive rankings.

### Vendor lock-in and rapid product/pricing change

**[established]** The platform changed materially within the last few months alone: Eleven v4/v4 Turbo launched **28 September 2026**; Eleven Realtime arrived in October; Scribe v3/v3 Live launched **10 November**; SFX v2 is already scheduled for deprecation in March 2027; Dubbing v2 launched in May and remains Alpha. citeturn528061search0turn528061search1turn528061search6

The pricing pages also now expose separate Creative, API and Agents billing models. This creates technical and economic lock-in if your code assumes model IDs, credit conversion or platform orchestration. Use an internal interface such as `ASRProvider`, `TTSProvider`, `VoiceAgentProvider` and keep raw WAV/text at boundaries so a vendor change remains a configuration task.

### Company trajectory and acquisition risk/opportunity

ElevenLabs is not behaving like a narrow TTS vendor. It reported an **$11B Series D valuation in February 2026**, later a **$300M employee tender at a $22B valuation** in September, and said Agents were handling more than 15M conversations/week by that point. These are company-reported metrics and therefore `[single-source]`. (ElevenLabs, 2026-09-30, https://elevenlabs.io/blog/tender-22bn) citeturn267184search0

On **16 November 2026**, ElevenLabs announced that it is acquiring Japanese voice-AI lab **Kotoba**, subject to closing conditions later in Q4. [single-source] (https://elevenlabs.io/blog/kotoba) citeturn267184search1 This reinforces the strategic direction toward full multilingual interaction infrastructure rather than “TTS API only.”

## H. Top 3 moves for us, each with the first step doable today at zero cost, and the single recommendation

### Move one — publish `ru-voicebench`: a real Russian technical-speech benchmark

**Priority: #1. Value: very high. Cost before publication: effectively $0–10. Confidence: [established].**

Build a reproducible corpus from your existing voice-rail history: 500–1,000 Russian Telegram utterances stratified by duration, technical English insertions, numbers, filenames, URLs, commands, background noise and self-corrections. Manually establish a small gold set, then run local Whisper large-v3, your existing OpenAI transcription path, Scribe v3, and—using trial/free allocations where possible—Deepgram and AssemblyAI. Publish ordinary WER **plus** technical-entity error rate, numeral/code accuracy, latency, $/hour and “downstream command correctness.”

The first zero-cost step today is to write a script that exports **100 existing audio/transcript pairs**, hashes/redacts them and creates `manifest.jsonl` with `audio_path`, `language`, `duration`, `reference`, `technical_terms`, `numbers`, `noise`, `speaker_count`. No API spending is needed for that step.

Prior art: `Smolevich/stt-benchmarks` covers 20 RU+EN voice memos and several providers; dannote's `russian-technical-stt-v1` examines Russian technical terminology. Neither currently covers Scribe v3 plus the full modern provider set with your “voice-to-coding-agent” error metric. `[established]` (https://github.com/Smolevich/stt-benchmarks; https://gist.github.com/dannote/1c4e14b88af811db0639b68acdc7769d) citeturn237891search10turn237891search9

This artifact simultaneously serves **rare knowledge, public visibility, ElevenLabs staff attention, frontier-lab hiring signal and an internal production decision**.

### Move two — ship `CallMyAgents`: voice control plane for your coding fleet

**Priority: #2. Value: very high. First-month likely spend: <$25. Confidence: [emerging].**

Architecture: ElevenAgent phone/web interface → authenticated tool gateway → n8n/MCP → `start_agent(machine, repo, task)`, `status(session)`, `summarize(session)`, `cancel(session)` → spoken result. The key engineering feature is not “voice assistant”; it is **capability-safe execution**: allowlisted commands, explicit confirmation for destructive actions, audit logs, idempotency tokens and machine/session routing.

ElevenLabs already exposes hosted MCP to Claude/Claude Code/ChatGPT/Cursor and has an official n8n integration; this minimizes scaffolding. citeturn267184search4turn267184search7

The zero-cost step today is to create a local tool schema and mock server with only three read-only functions: `list_agents()`, `get_agent_status(id)`, `read_last_output(id)`. Connect it first to a Free ElevenAgent/browser widget; do not enable task execution until auth and confirmation semantics are correct.

Prior art: ElevenLabs has generic MCP/agent-management tooling and many workflow examples, but I found **no prior art for this exact cross-machine “phone my Claude Code/Codex/Cursor fleet” control plane** in the official repos searched. **Prior art not found** after checking the hosted MCP docs, open-source MCP repo, plugin, packages and n8n repo.

### Move three — make the operator's bilingual PVC, but only after grant application

**Priority: #3. Value: high if paired with content/hiring, low if treated as vanity. Confidence: [emerging].**

The purpose should be a **digital-twin interface and public technical content**, not passive audio consumption. Record 45–60 minutes of studio-clean material deliberately covering native Russian, accented English, code identifiers, company names, mathematical notation and emotional/register variation. Create a PVC only when Creator credits are justified or the grant is approved, then run a bilingual evaluation against IVC, real voice, Chatterbox/F5/XTTS and generic v4 voices.

The zero-cost step today is to prepare and record the evaluation corpus locally. Do not pay Creator merely to discover that your microphone/sample design is bad.

PVC is preferable to IVC for a long-lived public digital twin because ElevenLabs itself positions PVC as its highest-fidelity, production-oriented clone and it is the only clone type eligible for public Voice Library monetization. [single-source] citeturn528061search5turn844860search1 If a Russian/accent niche is underrepresented, the Voice Library can also become a small distribution/revenue experiment; creators had collectively earned **$22M** by May 2026 according to ElevenLabs, though that aggregate says nothing about your expected individual earnings. [single-source] (ElevenLabs, 2026-05-22, https://elevenlabs.io/blog/22-million-earned-by-voice-creators-on-elevenlabs) citeturn844860search5

**Single recommendation: build `ru-voicebench` first, with `CallMyAgents` as the demo application, and keep ElevenLabs Scribe in production while you measure it. [established]** Do **not** spend recurring money on Dubbing v2 or a polished PVC content factory until one of two conditions is met: the Startup Grant is awarded, or the benchmark/content starts producing interviews/leads. The central assumption is that over the next 90 days **technical artifacts that expose rare multilingual voice-engineering knowledge will create more career and distribution value than additional synthetic media volume**.

## I. Sources

The following are the primary URLs actually used in the investigation. Pages without an explicit publication/update date are marked **accessed 2026-11-20**.

1. ElevenLabs Creative pricing — accessed 2026-11-20: https://elevenlabs.io/pricing citeturn812944view0
2. ElevenLabs API pricing — accessed 2026-11-20: https://elevenlabs.io/pricing/api citeturn812944view1
3. ElevenAgents pricing — accessed 2026-11-20: https://elevenlabs.io/pricing/agents citeturn812944view2
4. ElevenLabs Startup Grants — accessed 2026-11-20: https://elevenlabs.io/startup-grants citeturn812944view3
5. Speech-to-Text capabilities — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/speech-to-text citeturn919106search5
6. ElevenLabs models overview — accessed 2026-11-20: https://elevenlabs.io/docs/overview/models citeturn223023search5
7. Scribe v3 launch — published 2026-11-10, updated 2026-11-14: https://elevenlabs.io/blog/scribe-v3 citeturn528061search1
8. Realtime diarization docs — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-api/guides/how-to/speech-to-text/realtime/diarization citeturn919106search2
9. Realtime STT API reference — accessed 2026-11-20: https://elevenlabs.io/docs/api-reference/speech-to-text/v-1-speech-to-text-realtime citeturn919106search4
10. Russian STT page — accessed 2026-11-20: https://elevenlabs.io/speech-to-text/russian citeturn223023search4
11. Eleven v4 launch — published 2026-09-28, updated 2026-10-19: https://elevenlabs.io/blog/eleven-v4 citeturn528061search0
12. v4/v4 Turbo changelog — 2026-09-28: https://elevenlabs.io/docs/changelog/2026/9/28 citeturn528061search7
13. TTS capabilities — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/text-to-speech citeturn919106search6
14. Instant Voice Cloning — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/voices/voice-cloning/instant-voice-cloning citeturn638505search1
15. Professional Voice Cloning — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/voices/voice-cloning/professional-voice-cloning citeturn638505search0
16. Voice cloning concepts — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-api/concepts/voice-cloning citeturn638505search3
17. My Voices plan access — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/voices/my-voices/what-is-my-voices citeturn638505search2
18. Voice Design — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/voices/voice-design/ citeturn528061search5
19. Voice Changer — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/voice-changer citeturn528061search4
20. ElevenAgents overview — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/overview citeturn919106search8
21. ElevenAgents integration overview — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/integrate/overview citeturn919106search7
22. WhatsApp integration — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/whatsapp citeturn944199search0
23. WhatsApp troubleshooting — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/whatsapp/troubleshooting citeturn944199search1
24. Twilio integration — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/customization/integrations/twilio citeturn944199search4
25. Campaign Manager — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/phone-numbers/campaigns citeturn944199search17
26. ElevenLabs Agents product page — accessed 2026-11-20: https://elevenlabs.io/agents citeturn944199search16
27. Dubbing capabilities — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/dubbing citeturn528061search8
28. Dubbing v2 launch — published 2026-05-28, updated 2026-10-05: https://elevenlabs.io/blog/introducing-dubbing-v2 citeturn528061search2
29. Dubbing Studio docs — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/products/dubbing/dubbing-studio citeturn638505search5
30. Dubbing lip-sync FAQ — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/dubbing/do-you-offer-lip-sync-in-dubbing citeturn638505search7
31. Studio — accessed 2026-11-20: https://elevenlabs.io/studio citeturn638505search9
32. Voiceover Studio docs — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/audio-tools/voiceover-studio citeturn638505search11
33. GenFM — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/distribution-publishing/gen-fm/how-do-i-use-gen-fm citeturn638505search10
34. Audio Native — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/audio-tools/audio-native citeturn638505search12
35. Voice Isolator — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/audio-tools/voice-isolator citeturn638505search8
36. Forced Alignment cookbook — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-api/guides/cookbooks/forced-alignment citeturn638505search13
37. Sound Effects — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/sound-effects citeturn528061search6
38. Music capabilities — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/music citeturn637214search23
39. Music v2.5 — published 2026-09-11: https://elevenlabs.io/blog/music-v2-5-model citeturn637214search13
40. Eleven Music terms — accessed 2026-11-20: https://elevenlabs.io/music-terms citeturn986727view7
41. Music Marketplace announcement — published 2026-03-19: https://elevenlabs.io/blog/introducing-the-music-marketplace-in-elevencreative citeturn637214search25
42. Music Marketplace docs — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/music/marketplace citeturn637214search38
43. Eleven Album / Iconic marketplace context — published 2026-01-21: https://elevenlabs.io/blog/introducing-the-eleven-album citeturn637214search17
44. Voice Library — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/voices/voice-library/ citeturn844860search2
45. Voice payouts — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/voices/payouts citeturn844860search0
46. Voice-sharing docs — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/voices/my-voices/how-do-i-share-a-voice citeturn844860search1
47. Voice Library Rewards Program — accessed 2026-11-20: https://elevenlabs.io/voice-library-rewards-program citeturn844860search4
48. Voice creator payouts article — published 2026-05-22, updated 2026-11-03: https://elevenlabs.io/blog/22-million-earned-by-voice-creators-on-elevenlabs citeturn844860search5
49. EU Voice Library Addendum — updated 2026-03-06: https://elevenlabs.io/pt/vlaeu citeturn844860search6
50. Image & Video capabilities — accessed 2026-11-20: https://elevenlabs.io/docs/overview/capabilities/image-video citeturn637214search0
51. Image/Video/Templates APIs announcement — 2026-10: https://elevenlabs.io/blog/introducing-the-image-video-and-templates-apis citeturn637214search42
52. Flows — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/products/flows citeturn638505search4
53. Image/Video grant-credit docs — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-creative/playground/image-video citeturn637214search2
54. ElevenReader — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/mobile-apps/elevenreader/what-is-elevenreader citeturn637214search9
55. ElevenReader availability — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/mobile-apps/elevenreader/where-is-elevenreader-available citeturn637214search10
56. Mobile app feature page — accessed 2026-11-20: https://elevenlabs.io/docs/help-center/product/mobile-apps/elevenlabs-for-ios-and-android/what-features-are-available-in-the-elevenlabs-mobile-app citeturn637214search12
57. ElevenLabs MCP — accessed 2026-11-20: https://elevenlabs.io/mcp citeturn267184search9
58. External assistants / Claude / ChatGPT / Cursor — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/operate/architect/external-assistants citeturn267184search4
59. Open-source MCP server — accessed 2026-11-20: https://github.com/elevenlabs/elevenlabs-mcp citeturn935555search6
60. Official n8n repo — accessed 2026-11-20: https://github.com/elevenlabs/elevenlabs-n8n/ citeturn935555search2
61. n8n integration directory — accessed 2026-11-20: https://n8n.io/integrations/elevenlabs/ citeturn267184search8
62. ElevenLabs + Zapier — accessed 2026-11-20: https://elevenlabs.io/agents/integrations/zapier citeturn267184search5
63. ElevenLabs + Make — accessed 2026-11-20: https://elevenlabs.io/agents/integrations/make citeturn267184search6
64. ElevenLabs + n8n — accessed 2026-11-20: https://elevenlabs.io/agents/integrations/n8n citeturn267184search7
65. ElevenLabs packages repo — accessed 2026-11-20: https://github.com/elevenlabs/packages citeturn935555search25
66. ElevenLabs coding-agent plugin — accessed 2026-11-20: https://github.com/elevenlabs/plugin citeturn935555search24
67. Third-party n8n prior art — accessed 2026-11-20: https://github.com/n8n-ninja/n8n-nodes-elevenlabs citeturn935555search36
68. Ambassador Program — accessed 2026-11-20: https://elevenlabs.io/ambassador citeturn812944view4
69. Ambassador terms — updated 2026-11-17: https://elevenlabs.io/ambassador-terms citeturn444058search1
70. Affiliate terms — updated 2026-06-10: https://elevenlabs.io/affiliates-terms citeturn698578view3
71. Impact Program — accessed 2026-11-20: https://elevenlabs.io/impact citeturn812944view6
72. Hackathon portal — accessed 2026-11-20: https://hackathon.elevenlabs.io/ citeturn935555search3
73. Hacks / recurring hackathons — accessed 2026-11-20: https://hacks.elevenlabs.io/ citeturn935555search17
74. Careers / open positions — accessed 2026-11-20: https://elevenlabs.io/careers/positions citeturn844860search8
75. Developer Experience Engineer — accessed 2026-11-20: https://elevenlabs.io/careers/d16a7ad6-ffbd-4655-8716-4872263f99ce/developer-experience-engineer citeturn844860search7
76. Forward Deployed Engineer — accessed 2026-11-20: https://elevenlabs.io/careers/6c4c57c1-ec72-42ba-af3a-eb7aebbde2e6/forward-deployed-engineer-software-engineer-usa citeturn986727view0
77. Enterprise Solutions Engineer — accessed 2026-11-20: https://elevenlabs.io/careers/275f43d0-b62d-401d-830c-7c1ac0e688aa/enterprise-solutions-engineer-usa citeturn986727view1
78. Artificial Analysis STT leaderboard — accessed 2026-11-20: https://artificialanalysis.ai/speech-to-text/non-streaming citeturn237891search22
79. Artificial Analysis TTS provider-voice leaderboard — accessed 2026-11-20: https://artificialanalysis.ai/text-to-speech/leaderboard/provider-voice citeturn859798search0
80. Artificial Analysis TTS model comparison — accessed 2026-11-20: https://artificialanalysis.ai/text-to-speech/models?quality=quality-vs-speed citeturn859798search7
81. Independent Whisper-on-FLEURS benchmark — accessed 2026-11-20: https://istitov.github.io/stuff/benchmarks/whisper-on-fleurs/ citeturn223023search8
82. Russian technical STT benchmark — 2026-08: https://gist.github.com/dannote/1c4e14b88af811db0639b68acdc7769d citeturn237891search9
83. RU+EN STT voice-memo benchmark — accessed 2026-11-20: https://github.com/Smolevich/stt-benchmarks citeturn237891search10
84. Russian speech model benchmark repository — accessed 2026-11-20: https://github.com/meshoklv/ru-speech-bench citeturn859798search3
85. AssemblyAI benchmark page — accessed 2026-11-20: https://www.assemblyai.com/benchmarks citeturn237891search0
86. Deepgram benchmarks — accessed 2026-11-20: https://deepgram.com/benchmarks citeturn237891search2
87. Perit STT leaderboard — accessed 2026-11-20: https://perit.ai/research/benchmark/speech-to-text/leaderboard citeturn237891search1
88. Zero Retention Mode — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-api/resources/zero-retention-mode citeturn812944view7
89. Agents retention docs — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/customization/privacy/retention citeturn116918search2
90. Agents ZRM docs — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/customization/privacy/zrm citeturn116918search1
91. ElevenLabs Privacy Policy — updated 2026-10-09: https://elevenlabs.io/privacy-policy citeturn812944view8
92. ElevenLabs EU Terms — updated 2026-10-09: https://elevenlabs.io/terms-of-use-eu citeturn812944view9
93. ElevenLabs Prohibited Use Policy — accessed 2026-11-20: https://elevenlabs.io/use-policy citeturn812944view10
94. ElevenLabs agent disclosure guidance — accessed 2026-11-20: https://elevenlabs.io/docs/eleven-agents/legal/disclosure-requirement citeturn716045search23
95. ElevenLabs service status/history — accessed 2026-11-20: https://status.elevenlabs.io/history citeturn812944view11
96. EU Commission Article 50 Q&A — published 2026: https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act citeturn951565search0
97. EU Commission Article 50 guidance — 2026: https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations citeturn951565search1
98. EU Commission enforcement announcement — published 2026-07-31: https://digital-strategy.ec.europa.eu/en/news/commission-starts-enforcing-ai-act-rules-and-new-transparency-requirements-2-august citeturn951565search2
99. Portuguese Law 41/2004 consolidated text — accessed 2026-11-20: https://diariodarepublica.pt/dr/legislacao-consolidada/lei/2004-106523049 citeturn919943search5
100. FCC Declaratory Ruling FCC 24-17 — released 2024-02-08: https://docs.fcc.gov/public/attachments/FCC-24-17A1.pdf citeturn951565search5
101. Google Gemini Notebook Audio Overview help — accessed 2026-11-20: https://support.google.com/gemininotebook/answer/16212820 citeturn473057search0
102. Gemini Notebook privacy/terms/copyright — accessed 2026-11-20: https://support.google.com/gemininotebook/answer/17004255 citeturn473057search4
103. Google Terms of Service — effective 2026-07-30: https://policies.google.com/terms?hl=en-US citeturn473057search2
104. Google Generative AI Prohibited Use Policy — modified 2024-12-17: https://policies.google.com/terms/generative-ai/use-policy citeturn473057search6
105. ElevenLabs $22B valuation / tender announcement — published 2026-09-30, updated 2026-11-05: https://elevenlabs.io/blog/tender-22bn citeturn267184search0
106. Kotoba acquisition announcement — published 2026-11-16, updated 2026-11-18: https://elevenlabs.io/blog/kotoba citeturn267184search1
107. ElevenLabs $500M ARR / Series D update — published 2026-05-05, updated 2026-11-03: https://elevenlabs.io/blog/500m-arr-and-new-investors citeturn267184search3