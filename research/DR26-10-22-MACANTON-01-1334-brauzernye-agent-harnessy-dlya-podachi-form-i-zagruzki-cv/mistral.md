# DR26-10-22-MACANTON-01-1334 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Browser-Agent Harnesses for Autonomous Form Filling (October 2026)

(leg: Mistral Vibe Work, deep-research skill, canvas. Chat: https://chat.mistral.ai/work/8cc9bd8c-cdb8-4368-8dcd-58567785a97c. Collected 2026-10-22 via Chrome-MCP. The canvas was long; table rows are kept faithfully but trimmed of filler. Inline source URLs appear in the "Sources" list at the end, which was harvested from the canvas anchors.)

## Executive summary (from the leg)

- The landscape consolidated in 2026. OpenAI retired the standalone ChatGPT Agent and the Atlas browser; the capability now lives in ChatGPT Work + its cloud browser (https://help.openai.com/en/articles/11752874-chatgpt-agent, https://help.openai.com/en/articles/20001371-evolving-atlas-into-chatgpt-for-browser-based-agentic-work). Google shut down Project Mariner (May 4, 2026) and folded it into Gemini in Chrome "auto-browse" and the Gemini app Agent Mode/Spark (https://www.androidheadlines.com/2026/05/google-shuts-down-project-mariner-ai-agent.html). Anthropic's line is Claude in Chrome (GA Aug 26, 2026, https://support.claude.com/en/articles/12012173-get-started-with-claude-in-chrome) + Claude Cowork + the Claude Code Chrome integration (`claude --chrome`, https://code.claude.com/docs/en/chrome).
- Only three vendors credibly drive your own logged-in Chrome/Edge profile: Anthropic (Claude in Chrome / Claude Code --chrome), Google (Gemini in Chrome auto-browse, Spark desktop Chrome, https://9to5google.com/2026/07/30/gemini-spark-chrome-auto-browse/), Microsoft (Browse with Copilot in Edge). Everything else is a cloud VM browser (ChatGPT Work, Manus, Genspark, Skyvern cloud, Gemini app agent) or a replacement browser (Comet, Fellou, Opera Neon).
- Local file upload works natively in exactly one major-vendor consumer harness: Claude Code + Chrome ("attach files from your machine to upload fields"). ChatGPT Work cannot automate file uploads in its built-in browser; the user must take over (https://learn.chatgpt.com/docs/browser).
- The user's blocker (Claude Code auto-mode safety classifier) is described by this leg as a known filed bug class: GitHub issues #97741 and #98169 (September 2026) document the classifier refusing user-authorized browser actions even after explicit approval and after leaving auto mode, with mcp__claude-in-chrome__* already allowed (https://github.com/anthropics/claude-code/issues/97741, https://github.com/anthropics/claude-code/issues/98169, https://github.com/anthropics/claude-code/issues/91495). [Orchestrator note: the claim of these issue contents is UNVERIFIED by the leg's own chain; checked separately in the synthesis.] Fix: run in Manual mode with pre-approved site permissions + allow rules.
- Reliability: Browser Use reports 97% on Online-Mind2Web (300 live tasks, June 2026, vendor-reported, https://browser-use.com/posts/online-mind2web-benchmark); OSWorld-Verified leaders ~85-90% vs 72.36% human baseline (https://presenc.ai/research/osworld-computer-use-leaderboard-2026, https://leaderboard.steel.dev/leaderboards/osworld/). Big consumer harnesses publish little; real-user reports (Comet: "70% magic, 30% comedy") remain the honest signal.
- Top setup for "profile + CV, human approves once": Claude in Chrome + Claude Code in Manual mode with per-site "Always allow"; Browser Use CLI (local CDP mode) as the orchestrator-friendly alternative; Skyvern as the API-driven batch pipeline (ships a "Job Apps" recipe with upload_file, https://www.skyvern.com/docs/developers/browser-automation).
- AI-text detectors are a real screening layer in hiring: Pangram markets to recruiters, batch-screens resume/cover-letter ZIPs, claims ~1-in-24,000 false positives for Pangram 4 (July 2026) (https://www.pangram.com/use-cases/recruiters, https://en.wikipedia.org/wiki/Pangram_(AI_detector)). No harness has a "voice -> verbatim paste" feature; compose it from OS-level dictation (macOS/Windows built-in, Superwhisper local mode) + agent takeover; avoid AI-cleanup dictation tools (Wispr Flow rewrites by design).

## Comparison table (condensed; US, checked October 2026)

Columns: 1 own logged-in browser? | 2 local file upload | 3 submit policy | 4 CAPTCHA | 5 reliability | 6 price/US | 7 failure modes | 8 drivable from another agent

| Harness | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| Anthropic Claude in Chrome + Cowork | Yes (extension drives your real Chrome; Cowork built-in browser isolated, https://claude.com/blog/cowork-built-in-browser) | Via Claude Code path; Cowork via connected folders | Side panel starts in "Automatically approve"; per-site "Always allow actions on this site"; "Skip all approvals" exists | Pauses, asks you | No public benchmark | Paid plans from $20/mo | Auto-approve flips guardrails off (https://www.eesel.ai/blog/claude-cowork-browser) | Partial: via Claude Code/VS Code; no public REST API for the extension |
| Anthropic Claude Code + Chrome (--chrome) | Yes, visible Chrome, shares login state | Yes, "attach files from your machine to upload fields on web pages" | Prompt -> "allow all actions on that site for the session"; modes Manual / acceptEdits / auto / bypassPermissions | "When Claude encounters a login page or CAPTCHA, it pauses and asks you to handle it manually" | Claude-scaffold systems ~85-90% OSWorld-Verified | Pro/Max $20-$200 | Auto-mode classifier false refusals (#97741, #98169); site permission sometimes ignored (#91495) | Yes: it is the orchestrator CLI; MCP mcp__claude-in-chrome__*; Agent SDK |
| OpenAI ChatGPT Work + cloud browser (Agent/Atlas/Operator retired) | No, remote cloud browser; sign in via takeover | No: "ChatGPT can't automate file uploads in the built-in browser" | Confirms before consequential actions; Settings > Browser > Agent permissions | Stops, "Take over browser" | No form benchmark; mixed hands-on reviews (https://arstechnica.com/features/2025/10/we-let-openais-agent-mode-surf-the-web-for-us-heres-what-happened/) | Paid plans except Free/Go | Site blocklist; takeover/resume sometimes cannot continue the run | No public task API |
| Google Gemini in Chrome auto-browse / Agent Mode / Spark | Yes for auto-browse/Spark desktop Chrome; Gemini-app agent = cloud VM | Spark: local desktop via Mac app + MCP; auto-browse upload not documented | Plan review + approval of payments/sensitive actions | Not documented | None published; experimental | AI Pro $19.99, Ultra $99.99-$199.99 (https://automatereal.com/blog/gemini-agent-mode/) | Experimental; work/school accounts need admin enablement | Partial: Spark MCP integrations; computer-use in Gemini API |
| Microsoft Copilot Actions / Browse with Copilot (Edge) | Yes, current Edge page | Not documented | Sensitive actions require approval (https://siliconangle.com/2025/11/18/copilot-mode-makes-edge-business-enterprise-ready-agentic-browser/) | Not documented | None published | Browse with Copilot needs Microsoft 365 Premium | Narrow action surface | No |
| Perplexity Comet | Yes, it is the browser + cloud agent tasks | Assistant does not touch local files; "Personal Computer on Mac" (Apr 2026) adds local file use | Approval flows; Amazon CFAA preliminary injunction Mar 2026 (stayed on appeal) | Stalls on CAPTCHA (https://www.itechguides.com/perplexity-comet-browser-review-2026-is-it-worth-trying/) | "70% magic, 30% comedy" | Browser free; Browser Agent gated to Pro/Max | Zenity-reported zero-click hijack flaws Mar 2026 (patched) | No |
| Manus (cloud sandbox; Browser Operator extension) | Extension runs in your Chrome/Edge; default cloud VM | Upload to sandbox (<=512 MB); local via My Computer | Autonomous in sandbox; takeover | Not documented | No public benchmark | Free 300 daily credits; $20-$200/mo | Sandbox resets after 7/21 days delete files | Yes, REST API |
| Genspark Super Agent | No, cloud computer | Upload to workspace | Autonomous | Not documented | None | $24.99/mo | Cloud only; credits run out | Partial |
| Fellou | Yes, it is the browser + Computer Use | Yes, recognizes upload buttons; FileAgent | Action plan you approve before run | Not documented | None | Freemium; $19/mo | Young product; job-application story is sponsor content | Partial (Eko 2.0 OSS) |
| Opera Neon | Own agents inside Neon (not your Chrome); MCP connector + Browser CLI let an external agent drive Neon | Not documented in detail | Autonomous; per-site approvals undocumented | Not documented | None | Browser free; agents $19.90/mo | Mixed reactions | Yes: MCP server + Browser CLI |
| Browser Use (OSS + Cloud + CLI) | Yes: CLI local mode attaches to your running Chrome via CDP (tabs, cookies, extensions, logins); Cloud = stealth browsers + cookie sync | Yes in local mode | As coded | Cloud: "can bypass any captcha" (vendor); local: stops | 97% Online-Mind2Web, 300 live tasks (June 2026, vendor-reported) | OSS free; Cloud $0.02/browser-hr, agents model price +20% | Anti-bot walls; vendor-run benchmark | Yes: first-class, installs as a skill into Claude Code/Codex; Python/TS SDK; agent API |
| Skyvern (AGPL OSS + cloud) | Self-hosted: your browser contexts; Cloud: managed + Login blocks | Yes: upload_file; "Job Apps" recipe | Workflow-defined; human-in-the-loop blocks | Basic solver $29; advanced CAPTCHA + 2FA/TOTP + proxies $149 | Self-claimed best on WRITE tasks; no independent benchmark | Free 5,000 credits; Hobby $29; Pro $149 | Credit-based retries burn budget | Yes: REST, MCP, Zapier/Make/n8n |
| Stagehand / Browserbase | Stagehand local drives any Chromium; Browserbase = cloud | Yes, Playwright setInputFiles | Fully code-controlled | Browserbase auto CAPTCHA on Developer plan+ | No benchmark; "up to 80% fewer tokens than Playwright" | Stagehand free; Browserbase Free/$20/$99 | You write code | Yes: SDK for Claude Code/Codex; MCP |
| OpenHands | Runs locally; browser-use BrowserToolSet | Yes through sandbox FS | Code/workflow-defined | Stops | Historic evals only | OSS | DIY setup | Yes: SDK/headless |
| Agent-E (Emergence) | Can use local Chrome profile (BROWSER_STORAGE_DIR) | Yes (Playwright) | Code-defined | Stops | WebVoyager-validated (GPT-4-Turbo era) | OSS; managed API enterprise | Research-grade | Yes |
| Amazon Nova Act | SDK drives local browser (Playwright) or AgentCore cloud browser | Yes via Playwright fallback | Code-controlled; escalates to human supervisor | Not solved; built to stop | ">90% reliability across early enterprise workflows, including complex form handling" (AWS claim 2025) | SDK free with AWS; ~$4.75/agent-hour | Python; OS keyboard quirks | Yes: SDK, MCP, Strands |
| Newcomers 2026 | ChatGPT Work; Cowork built-in browser; Gemini Spark; Browser Use CLI; Vercel agent-browser (https://github.com/vercel-labs/agent-browser) | | | | | | | |

## Benchmark reality check (from the leg)

- OSWorld-Verified (369 tasks): top ~86-90% (Qwen3.8-Max 86.1%, Intelligence-Indeed Agent 90.19%) above 72.36% human baseline; OSWorld 2.0 much harder, best tracked ~78% (https://leaderboard.steel.dev/leaderboards/osworld-2/).
- Online-Mind2Web (300 tasks, 136 live sites): Browser Use Cloud bu-max 97% (https://leaderboard.steel.dev/leaderboards/online-mind2web/), vendor-submitted; the benchmark's own team found frontier agents "scored far below what they reported on older benchmarks" under human evaluation (https://onlinemind2web.com/).
- WebArena best tracked ~74.3% (Oct 11 2026); Gemini 2.5 Pro 54.8% -> 37.8% on WebChoreArena (https://leaderboard.steel.dev/leaderboards/webarena/, https://awesomeagents.ai/leaderboards/web-agent-benchmarks-leaderboard/).
- BrowseComp measures research persistence, not form filling (https://leaderboard.steel.dev/leaderboards/browsecomp/).

## Top-3 (from the leg)

### #1 Claude in Chrome + Claude Code (--chrome), Manual mode, per-site pre-approval
Only major-vendor stack that drives your real logged-in Chrome incl. Google SSO, uploads local files as an official capability, and lets you pre-approve a site once.
Recipe: Claude Pro/Max; install extension (https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn); extension settings -> "Always allow actions on this site" for portals (https://support.claude.com/en/articles/12902446-claude-in-chrome-permissions-guide); launch `claude --chrome` (or /chrome, "Enabled by default"); run in Manual (default) mode, not auto; keep profile.md and CV path in the working dir; prompt: open portal, use profile.md verbatim for free text, upload CV to resume field, fill every page, STOP before final submit and show summary; review, then say "submit it" (the one approval).

### #2 Browser Use CLI, local mode, driven from Claude Code
97% Online-Mind2Web (vendor-reported), local-Chrome attach via CDP with existing cookies/logins, free OSS (https://docs.browser-use.com/open-source/browser-use-cli, https://github.com/browser-use/browser-use/blob/main/AGENTS.md).
Recipe: `uv tool install browser-use` (Python 3.12), `browser-use skill install`; start Chrome with remote debugging or accept the CDP prompt; hand Claude Code the task; have it stop before the final click and print a field-by-field diff. Optional Cloud (use_cloud=True, cloud_profile_id via profile-use cookie sync) for bot-hostile portals ($0.02/browser-hour); CAPTCHA-bypass on employment portals is a ToS gray zone - keep local.

### #3 Skyvern for batch pipeline
Skyvern docs: "upload_file attaches a resume or document. For job applications, the Job Apps recipe in the app already does this end to end" (https://www.skyvern.com/docs/developers/browser-automation). Pricing https://www.skyvern.com/pricing.
Recipe: Cloud account (Free 5,000 credits; Hobby $29; Pro $149 for advanced CAPTCHA + 2FA + proxies); store credentials in the vault; host CV at a fetchable URL; one workflow per portal family: goto -> login -> fill/select_option -> upload_file(CV) -> pause for human review -> submit block; trigger via REST from the orchestrator.
Watch out: cloud browser, not your profile; some SSO portals fight it.

Honorable mentions: Manus Browser Operator; ChatGPT Work (cannot upload your CV without takeover); Comet (plan-gated, March 2026 Zenity findings).

## (b) Unblocking Claude Code auto-mode for file upload / submit (from the leg)

- Classifier: in auto mode a background classifier model grades a written description of each risky action; blocks categories like [Data Exfiltration]; soft blocks are lifted when the user message "directly and specifically describes the exact action Claude is about to take"; boundaries stated in conversation ("don't submit yet") are treated as block signals. After 3 consecutive or 20 total blocks the session falls back to prompting (https://code.claude.com/docs/en/permission-modes, https://www.backslash.security/blog/claude-code-auto-mode-catches-a-lot-but-not-everything, https://claudcod.com/blog/claude-code-auto-mode/).
- Headline claim: there is no documented allow-list that overrides a hard classifier block; allow rules and in-chat approval do not override it. Recipe: route around it.
  1. Do not run the browser task in auto mode (Shift+Tab in CLI). Set `defaultMode: "default"` and allow rules for mcp__claude-in-chrome__* tools in «внутренний путь лаборатории» (https://code.claude.com/docs/en/settings-reference, https://code.claude.com/docs/en/permissions). Rule order deny -> ask -> allow.
  2. Pre-approve sites in the extension ("Always allow actions on this site").
  3. Phrase the task as the exact action.
  4. Do not poison the classifier with boundaries you do not mean ("don't do anything irreversible"); say "stop before the final Submit and show me a summary", then approve.
  5. Keep the CV inside a working directory (additionalDirectories).
  6. Unattended: bypassPermissions (drops all prompts) or a PreToolUse hook auto-approving only mcp__claude-in-chrome__* to allowlisted domains.
  7. If the Desktop app Code tab false-refuses, run the same task from the CLI with --chrome.
- Official docs: https://code.claude.com/docs/en/chrome , https://code.claude.com/docs/en/permissions , https://code.claude.com/docs/en/permission-modes , https://code.claude.com/docs/en/settings-reference , https://support.claude.com/en/articles/12902446-claude-in-chrome-permissions-guide

## (c) Honest limits: AI detectors and voice -> verbatim paste (from the leg)

- Pangram "AI Detector for Recruiters" batch-screens resumes/cover letters/applications; claims 1-in-10,000 FPR; Pangram 4 (July 29, 2026) labels human / AI-assisted / AI-generated, claimed 0.0041% FPR; users 2,700 (June 2025) -> 120,000 (June 2026) (https://www.pangram.com/use-cases/recruiters, https://en.wikipedia.org/wiki/Pangram_(AI_detector)). An "AI-Assisted" label appears when AI edited or polished human-started text, even grammar tools (https://proofreaderpro.ai/blog/pangram-ai-detector-review).
- No harness ships "dictate -> verbatim paste"; compose it. Verbatim dictation engines: macOS/Windows built-in dictation; Superwhisper local Whisper (free plan, no AI processing; https://superwhisper.com/vs/wispr-flow, https://lumevoice.com/blog/superwhisper-vs-wispr-flow-comparison-2026/); AquaVoice (https://ricardogil.com/blog/ai-voice-dictation-2026-wispr-flow-superwhisper-review). Avoid Wispr Flow (cloud, smart editing; https://wisprflow.ai/).
- Handoff pattern: instruct the agent to fill every field except essays, leave essays empty and list prompts; human dictates into each field; approve Submit once. ChatGPT Work: takeover mode. Gemini/Comet/Copilot: take control of the tab. Skyvern/Browser Use/Nova Act: pause/human-in-the-loop before essay fields and Submit; dictated text can live in a local file the agent must paste byte-for-byte (verbatim guarantee = code review).
- Compatible with "no AI-written answers": the human's spoken words transcribed verbatim and placed by an agent acting as a keyboard. Not compatible: agent-drafted text, AI-polished dictation, and possibly human text passed through an AI rewriter.

## Conflicts / open questions (from the leg)

- Benchmarks mostly vendor-submitted; OSWorld 1.0 vs 2.0 not comparable. Pricing drift (Skyvern 5,000 vs ~1,000 free credits). Classifier bugs are single-user reports. Manus ownership history messy. Could not verify AI-policy statements of Successif/Generator/Pivotal.
- Open: Gemini auto-browse file upload? Cowork built-in browser upload? Prevalence of classifier false refusals? Ashby/Greenhouse blocking cloud agents?

## Sources (harvested from the canvas)

https://help.openai.com/en/articles/11752874-chatgpt-agent
https://help.openai.com/en/articles/20001371-evolving-atlas-into-chatgpt-for-browser-based-agentic-work
https://www.androidheadlines.com/2026/05/google-shuts-down-project-mariner-ai-agent.html
https://blog.google/products-and-platforms/products/chrome/gemini-3-auto-browse/
https://www.unite.ai/gemini-in-chrome-opens-to-all-u-s-android-users-as-auto-browse-goes-mobile/
https://9to5google.com/2026/07/30/gemini-spark-chrome-auto-browse/
https://support.claude.com/en/articles/12012173-get-started-with-claude-in-chrome
https://claude.com/blog/cowork-built-in-browser
https://www.clickforest.com/en/blog/claude-cowork-explained
https://creditforstartups.com/pricing/claude-cowork
https://code.claude.com/docs/en/chrome
https://www.eesel.ai/blog/claude-cowork-browser
https://presenc.ai/research/osworld-computer-use-leaderboard-2026
https://github.com/anthropics/claude-code/issues/97741
https://github.com/anthropics/claude-code/issues/98169
https://github.com/anthropics/claude-code/issues/91495
https://learn.chatgpt.com/docs/browser
https://arstechnica.com/features/2025/10/we-let-openais-agent-mode-surf-the-web-for-us-heres-what-happened/
https://automatereal.com/blog/gemini-agent-mode/
https://gemini.google/subscriptions/
https://siliconangle.com/2025/11/18/copilot-mode-makes-edge-business-enterprise-ready-agentic-browser/
https://support.microsoft.com/en-us/topic/copilot-actions-in-edge-5ed5e17e-42df-40a3-984a-20420eba86e2
https://www.switchtools.io/blog/perplexity-comet-ai-agent-browser
https://convly.ai/comet-browser-perplexity-review-2026/
https://www.itechguides.com/perplexity-comet-browser-review-2026-is-it-worth-trying/
https://baeseokjae.github.io/posts/perplexity-comet-browser-review-2026/
https://www.taskade.com/blog/manus-ai-review
https://manus.im/pricing
https://promoclock.co/en/tools/manus/
https://fast.io/resources/manus-ai-limits/
https://www.lindy.ai/blog/genspark-ai-features
https://fellou.ai/
https://siftino.com/tool/fellou/
https://techcrunch.com/sponsor/fellou/the-rise-of-fellou-worlds-first-agentic-ai-browser/
https://toolchase.com/tool/opera-neon/
https://ai-deck.app/blog/opera-neon/
https://docs.browser-use.com/open-source/browser-use-cli
https://github.com/browser-use/browser-use/blob/main/AGENTS.md
https://browser-use.com/posts/online-mind2web-benchmark
https://browser-use.com/pricing
https://www.skyvern.com/docs/developers/browser-automation
https://www.skyvern.com/pricing
https://www.aiagentsquare.com/agents/skyvern
https://github.com/skyvern-ai/skyvern
https://www.browserbase.com/pricing
https://www.stagehand.dev/
https://docs.openhands.dev/sdk/guides/agent-browser-use
https://github.com/EmergenceAI/Agent-E
https://docs.aws.amazon.com/nova-act/latest/userguide/what-is-nova-act.html
https://aws.amazon.com/blogs/machine-learning/amazon-nova-act-sdk-preview-path-to-production-for-browser-automation-agents/
https://www.therundown.ai/tools/amazon-nova
https://github.com/vercel-labs/agent-browser
https://leaderboard.steel.dev/leaderboards/osworld/
https://leaderboard.steel.dev/leaderboards/osworld-2/
https://leaderboard.steel.dev/leaderboards/online-mind2web/
https://onlinemind2web.com/
https://leaderboard.steel.dev/leaderboards/webarena/
https://awesomeagents.ai/leaderboards/web-agent-benchmarks-leaderboard/
https://leaderboard.steel.dev/leaderboards/browsecomp/
https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn
https://support.claude.com/en/articles/12902446-claude-in-chrome-permissions-guide
https://github.com/browser-use/browser-use
https://www.skyvern.com/developers
https://code.claude.com/docs/en/permission-modes
https://www.backslash.security/blog/claude-code-auto-mode-catches-a-lot-but-not-everything
https://claudcod.com/blog/claude-code-auto-mode/
https://code.claude.com/docs/en/settings-reference
https://code.claude.com/docs/en/permissions
https://www.pangram.com/use-cases/recruiters
https://en.wikipedia.org/wiki/Pangram_(AI_detector)
https://proofreaderpro.ai/blog/pangram-ai-detector-review
https://superwhisper.com/vs/wispr-flow
https://lumevoice.com/blog/superwhisper-vs-wispr-flow-comparison-2026/
https://ricardogil.com/blog/ai-voice-dictation-2026-wispr-flow-superwhisper-review
https://wisprflow.ai/
https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork
https://help.openai.com/en/articles/20001280-using-cloud-browser-in-chatgpt
https://blogs.windows.com/msedgedev/2025/10/23/meet-copilot-mode-in-edge-your-ai-browser/
https://investor.opera.com/news-releases/news-release-details/opera-opens-public-access-opera-neon-its-experimental-agentic-ai
