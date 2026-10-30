# DR26-10-22-MACANTON-01-1334 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Browser-agent harnesses for real form filling and submission in October 2026

(leg: ChatGPT Pro, Deep research (DR App), then "paste verbatim" in the same chat. Chat: https://приватный чат Collected 2026-10-22 via Chrome-MCP. The leg was ~77K chars; this file keeps all conclusions, all matrix facts and the complete URL index, with prose compacted. Numbers in [N] map to the URL index at the end.)

## Bottom line (ChatGPT leg)

For the exact workflow (real authenticated browser, Google SSO/cookies already present, multi-page application forms, dropdowns, CV/PDF upload from disk, one human approval before an irreversible submit) the lowest-babysitting systems are the ones where browser actions are exposed as explicit tools and the approval boundary is under YOUR control rather than a model-side safety classifier.

Ranking as of October 22, 2026:
1. Best overall: Browser Use + Browser Harness against your existing Chrome. Browser Use positions Browser Harness as a way for coding agents (Claude Code, Codex, Cursor) to control a real Chrome session; its form-filling stack supports uploads, logins, MFA, forms. OSS path has no Claude-style semantic classifier between orchestrator and browser action; put one deterministic approval gate right before final submission. Browser Use cloud benchmark page: 72.9 for its own agent on BU Bench v2.1; cloud browser 90.5% access through 100 bot-protected sites vs 3.0% for plain headless Chrome (vendor benchmarks, not independent WebArena). [1][2][3][4]
2. Best if keeping the existing stack: Claude Code + Claude in Chrome, but stop using Auto mode as the approval mechanism. Anthropic documents authenticated browser state, local file uploads, form filling, Chrome/Edge, CAPTCHA/login handoff. Auto mode inserts a background safety classifier; allow rules do not override every protected path; 2026 bug reports show the Auto classifier blocking Chrome tools despite mcp__claude-in-chrome__* being allow-listed. Fix: default permission mode + explicit tool allow rules + Claude in Chrome per-site "Always allow actions on this site". [5][6][7][8]
3. Best no-code/local-browser alternative: Manus Browser Operator (runs in your existing Chrome/Edge session, inherits logins and local IP, one-time authorization per task/session, then autonomous; triggerable via Manus API). Caveat: arbitrary local <input type=file> attachment is not clearly promised; validate on Ashby/Greenhouse first. [9][10][11][12]
4. If a persistent cloud browser is acceptable: Stagehand + Browserbase (document upload, persistent cookies/session, automated CAPTCHA solving on paid plans, Stagehand runs locally or on Browserbase; your code owns the approval boundary). [13][14][15][16]
- Do NOT make ChatGPT Agent, Gemini Auto Browse, Comet or Copilot the primary engine when the requirement is "approve once, then finish": all retain product-level safeguards (Google requires confirmation before web-form submission; OpenAI confirms consequential actions; Microsoft pauses on sensitive actions; Perplexity checks before sensitive actions). [17][18][19][20]
- Remove Atlas (retired Aug 9, 2026) and Project Mariner (shut down May 2026, folded into Gemini/Chrome and computer-use APIs) from the shortlist. [21][22]

## Comparison matrix (compacted; columns: own profile? | local CV upload | submit policy | CAPTCHA | reliability | price | failure modes | drivable)

- OpenAI ChatGPT Agent / browser extension / Work (Operator absorbed; Atlas retired): current ChatGPT browser integration can work with already signed-in sites; Agent also has isolated computer with user handoff for auth [23][24][25] | file interaction supported by browser-control docs but test the exact portal; API computer-use path wrappable with deterministic automation [26][24] | confirmation on consequential actions; no documented per-site "preapprove Submit forever" setting [17][23] | user takeover [23] | CUA preview 2025-03-11: 38.1% OSWorld, 58.1% WebArena, 87% WebVoyager; ChatGPT Agent 2025-07-17: 68.9% BrowseComp (information finding, not forms) [27][23] | paid ChatGPT; no stable per-plan US price quoted [25] | confirmation pauses, prompt-injection safeguards, watch requirements [17][23] | yes via Computer Use API / Agents SDK [26][28]
- OpenAI Atlas: retired Aug 9, 2026 [21][22]
- Anthropic Claude in Chrome + Claude Code --chrome: YES, real Chrome/Edge, shares login state [5][29] | YES documented: Claude Code reads the local file and sends contents to the browser; 10 MB total, file must be readable by the session, hard links rejected, credential-looking paths (.env, .pem, .ssh) protected [5] | Chrome modes exactly "Manually approve (Manual)", "Automatically approve (Auto)", "Skip all approvals (Skip)"; per site "Always allow actions on this site"; Claude Code modes default, acceptEdits, plan, auto, dontAsk, bypassPermissions [6][30] | stops and asks human [5] | Sonnet 4.5 61.4% OSWorld (2025-09-29), model-level, not product-level [31] | Pro, Max, Team, Enterprise; exact dollar amounts not exposed in official docs retrieved [6][29] | Auto classifier false refusals; 2026 file_upload regressions incl. an Ashby job-application repro; service-worker sleep; debugger detach on complex pages; screenshot timeout [32][33][8][34][35] | yes, Claude Code is the orchestrator; Chrome tools exposed as MCP-style tools; Agent SDK surfaces approvals [5][36]
- Google Gemini in Chrome Auto Browse / agent mode: YES, Chrome, signed-in sites [18] | form filling from a PDF source yes; generic local file-chooser upload not documented [37] | user reviews plan, presses "Start Task"; then asks confirmation before submitting web forms/communications/modifying data, so Start Task is NOT a blanket approval [18] | "Take over task" [18] | Computer Use model benchmarks (Online-Mind2Web/WebVoyager/AndroidWorld) 2025-10-07; no product-level Auto Browse score [38] | US: AI Pro $19.99/mo, Ultra $99.99/mo (Oct 2026 reporting) [18][39] | final-action confirmations, rollout limits, attachment under-documented [18] | consumer Auto Browse: no external API; separate Gemini Computer Use API [38]
- Google Project Mariner: shut down May 2026 [no recoverable URL in the original]
- Perplexity Comet: its own Chromium browser; assistant can click/type/submit [40][41] | not documented for arbitrary file chooser | Enterprise controls "Enable Comet to Control the Browser", "Enable 'Always Allow' option", domain permissions No Access / Read Only; still checks before sensitive actions, so "Always Allow" is not a universal submit bypass [42][20] | handoff | no comparable public score | free browser; Pro $20, Max $200 [43][44] | sensitive-action pauses; security scrutiny [20][45] | not Comet itself (Agent API exists but does not drive your Comet)
- Microsoft Browse with Copilot in Edge / Copilot Actions / Tasks: Browse with Copilot works in user's Edge with cookies; Copilot Tasks has a task browser with persisted cookies [19][46] | file upload not clearly documented; assume takeover for attachment fields | pauses for purchases, reservations, sending communication, deletion; Copilot Tasks approvals can be granted at recurring-task setup [19][46] | handback | no public score | Microsoft 365 Premium $19.99/mo US [47] | preview/admin-controlled parts [19][48] | no general browser-control API
- Manus Browser Operator: YES, existing Chrome/Edge session, logins, tabs, local IP [9][49] | Desktop has local-file access via "My Computer"; Browser Operator local file chooser not guaranteed; conditional until portal-tested [12][49] | click "Authorize" once per task/session, then autonomous; pauses at payment etc. [50][10] | local session reduces CAPTCHA; aggressive anti-bot sites may need manual help [49] | no public score | credit-based Free/Pro/Team; quote dashboard [51] | Manus itself warns drag-and-drop and multi-step forms may not work perfectly [50][49] | YES, triggerable via Manus API; API creates tasks, manages files [10][11][52]
- Genspark: business AI Browser = private cloud instance; Chrome extension clicks/fills in current Chrome [53][54] | no authoritative doc for local file chooser | domain allowlists, credential vaulting, action limits; no exact final-submit setting [53][54] | not documented | no benchmark | included in every Genspark plan; price not exposed [53] | split cloud/extension; under-documented | no external API verified
- Fellou: insufficient authoritative documentation [55]; treat as unverified.
- Opera Neon: its own browser; since 2026-03-31 third-party AI agents can operate inside the active Neon session via MCP Connector [56][57] | not documented, test | no named per-site submit-preapproval switch found | not verified | no score | ~$19.90/mo per a 2026-10-14 third-party check [58][56] | permission docs less mature | YES: MCP Connector for third-party agents [57]
- Browser Use OSS + Browser Harness: YES, real Chrome; plugins for Claude Code/Codex/Cursor [1][59] | YES uploads in form-filling product [2] | your policy; add deterministic approval gate | cloud handles CAPTCHA; local Chrome stops [1][2] | BU Bench v1 78% (2026-01-31); v2.1 72.9 (180 vs 200 tasks inconsistency); vendor benchmark [3][4] | OSS MIT; cloud $0.02/browser-hour + proxy; managed agent +20% model cost [1] | wrong element, dynamic widgets, stale DOM, drift; wrong-tab risk on real profile | excellent: Python lib, cloud API, CDP, CLI/plugins [59][1]
- Browser Use Cloud: cloud sandbox with persistent profiles | yes [2] | programmable | yes [1] | 90.5% access across 200 visits to 100 bot-protected sites vs 3.0% headless (infrastructure test, not task success) [1] | $0.02/h + 20% [1] | separate login provisioning; cloud IP can trigger checks | API/CDP/SDK
- Skyvern Cloud/OSS: saved browser profiles (cookies, localStorage, session files), not your live Chrome [60][61] | yes but cloud-oriented: upload_file takes a file URL Skyvern can fetch, so a local CV must be staged first [61] | autonomous/programmatic; add your own approval step | solves CAPTCHA natively; 2FA [62] | no current WebArena/OSWorld number | OSS self-hostable; Cloud price not reliably surfaced | profile expiry, bot checks, file staging inconvenient | yes, strong API/workflow
- Stagehand + Browserbase: local or Browserbase cloud [14][13] | yes, upload documents [13] | fully programmable; implement one human gate and record approval | paid plans include CAPTCHA solving; Identity adds auth/proxy/CAPTCHA [16][63] | no public number | Free; Developer $20/mo (100 browser-hours then $0.12/h); Startup $99/mo (500 h then $0.10/h); Scale custom; CAPTCHA only on paid [16] | requires engineering; cloud profile must be authenticated | excellent
- Agent-E: research/developer agent, automation browser not your everyday Chrome; no polished local-CV surface; no current product-level result
- OpenHands browsing: sandboxed coding-agent environment; historical AXTree/text observations can miss visual cues [64]; evaluation framework spans coding and browsing [65]; wrong abstraction for personal browser delegation
- Amazon Nova Act: developer SDK/service, AWS/AgentCore deployment; can escalate to humans; approval gate is yours [66][67]; REAL Bench dataset states Nova Act was trained on REAL Bench replicas, so not a clean independent metric [68]; Python SDK, AWS integration [66][69]
- Retriever AI (2026 newcomer): form-filling workflow says open the form in your own Chrome account; fills known answers, flags missing details, pauses before submission; attachment into arbitrary upload widgets not established [70]

### What the benchmark column tells you
WebArena, OSWorld, Online-Mind2Web, WebVoyager and BrowseComp test different things; a 68.9% BrowseComp does not imply 68.9% chance of submitting an Ashby application, and OSWorld does not measure file-chooser reliability [71][72]. WebArena human baseline ~78.24%; success is multiplicative over dozens of clicks [73][74]. Weighting for this use case: right authenticated browser state; deterministic local-file handling; false-refusal risk of the safety layer; control of the irreversible gate; recovery after navigation; then generic score.

## Top recommendations and setups

### Browser Use Browser Harness (best control-to-effort)
Dedicated local Chrome profile "Applications" (Google SSO, LinkedIn/Ashby/Greenhouse sessions, no banking tabs, CV at a fixed path); connect Browser Harness; let Claude Code/Codex issue navigation/form actions [1][59]. Keep profile data in a machine-readable file (identity, links, work_authorization, files.resume_pdf, policy flags such as never_invent_unknown_answers, essays_must_come_from_human_source_text, final_submit_requires_approval). The agent may navigate, click, select, fill, upload freely; it stops only at a semantic submit_application gate implemented OUTSIDE the LLM: summarize form for review -> human approval -> perform exact submit. After approval do not route "is it safe to submit?" back through another model. Checkpoint field state per page in JSON so navigation state loss can be re-filled. CAPTCHA: pause and hand to human. Why it beats Claude Auto: no opaque semantic classifier deciding that "upload my CV to Ashby" is unsafe.

### Claude Code + Claude in Chrome (best migration-free)
Use user-level «внутренний путь лаборатории» (not a repo file): Auto-mode configuration is restricted to user/managed settings or --settings to stop repos from escalating privileges [75]. Example:
```json
{"permissions":{"defaultMode":"default","allow":["mcp__claude-in-chrome__file_upload","mcp__claude-in-chrome__form_input","mcp__claude-in-chrome__browser_batch","mcp__claude-in-chrome__computer"]}}
```
Also allow Read on the CV file/dir (the upload first reads the local file, so browser file_upload permission cannot compensate for a denied Read). In Claude in Chrome set "Always allow actions on this site" for jobs.ashbyhq.com, *.greenhouse.io, *.lever.co, airtable.com, docs.google.com, forms.gle [6]. Do NOT try domain-scoped MCP rules such as mcp__claude-in-chrome__file_upload(domain:jobs.ashbyhq.com): current permissions docs do not support MCP tool-parameter matching that way; bare MCP tool authorization and extension site authorization are separate layers [7]. Launch `claude --chrome` or /chrome. Task contract: operate only in the current application tab and same-site child pages; fill from application-profile.json; upload the CV; navigate Next/Continue; never infer unknown factual answers; never write/rewrite essays; free text only from exact human files; stop immediately before final submit and show company/program, role, unanswered fields, exact free-text answers, uploaded filename; after "APPROVE SUBMIT" press the final Submit button without further confirmation unless the browser requires one. Aggressive route: `claude --chrome --permission-mode bypassPermissions` plus Chrome "Skip all approvals (Skip)"; not recommended as default (requiresUserInteraction-marked MCP operations can still force a prompt; unrestricted control of a profile with unrelated high-value sessions is unnecessary) [6][30][36].

### Manus Browser Operator (best low-configuration local)
Connectors -> My Browser, install Browser Operator, start the task, press Authorize once. Prompt: use My Browser; fill from attached profile/CV/human answers; do not invent; do not write essays; stop only on CAPTCHA/MFA, unavailable factual answer, unsafe attachment, payment/legal-consent step; authorized to submit once every required field matches supplied material [9][50][10][11]. Run a five-portal acceptance test first (Ashby, Greenhouse, Google Forms, Airtable, one custom fellowship portal): open -> identity -> dropdown -> upload CV -> next page -> back/forward -> verify state -> stop before submit [12][52][49]. Engineering-led alternative: Stagehand/Browserbase persistent profile [16][13].

## Fixing Claude Code's file_upload false refusal (three failure classes + one bug class)
1. Claude Code permission prompt (easy): permissions.allow/ask/deny; modes default, auto, dontAsk, bypassPermissions [7][30]. Allow the specific Chrome tools (not every tool) in user settings and allow Read of the CV; Anthropic's browser docs say upload requires the session to be allowed to read the file [5].
2. Claude in Chrome site permission: set "Always allow actions on this site". Extension modes: Manually approve (prompt individually) / Automatically approve (background safety classifier) / Skip all approvals (no automatic browser safety review for ordinary actions) [6].
3. Auto-mode safety-classifier denial (the user's failure): an allow rule is not a guaranteed override of the Auto classifier; Anthropic's Auto docs describe a background classifier with hard-deny / soft-deny / allow concepts, and some actions stay interaction-required even when other systems say allow [75][36]. A September 2026 issue reports computer, form_input, browser_batch blocked by Auto despite an allow rule for mcp__claude-in-chrome__* [8]. Workaround: do not run this workflow in Auto; default mode; allow only needed Chrome tools; per-site Always allow. Skip all approvals (Chrome) and bypassPermissions (Code) are separate controls [6][30].
4. Actual file_upload BUG (not a safety decision): a July 2026 issue reports mcp__claude-in-chrome__file_upload rejecting a readable local file during an Ashby job application ("no way to satisfy 'files the user has ...'"), Claude Code versions ~2.1.206-2.1.218; August reports around 2.1.221/2.1.222 describe path-array and "allowed to read" problems [32][33][76]. Diagnose by error string: "blocked/unsafe/requires approval" -> permission/Auto-classifier; "not allowed to read" -> local Read permission OR upload regression; "paths expected array" -> browser-tool/version bug [76]; "Not allowed to load local resource" / CDP "Not allowed" -> Chrome/extension local-file access (March 2026 report: enabling Chrome extension "Allow access to file URLs" fixed it) [77]; hang/disconnect -> reconnect/service-worker/debugger issue [34][35]. Official docs still advertise local file upload as supported, so a reproducible failure on a current build is an implementation regression [5]. Failover chain: Claude Chrome file_upload -> after two identical failures -> Browser Harness / direct Playwright upload -> resume Claude on the same page. Do not let the model retry the same broken upload twenty times.

## Human-authored answers: voice dictation without AI rewriting
- Not verified: that Successif, Generator and Pivotal all use Pangram or share identical AI-writing rules; the retrieved results did not include authoritative policy pages. Pivotal's AI Safety Research Fellowship page is live for the January 18-April 30, 2027 cohort [78]. Where an application says "answers must be your own", treat that as controlling regardless of detector.
- Pangram 4 Technical Report (July 29, 2026): trained detector targeting current frontier models; reported 0.0041% false-positive rate (~1 in 24,000) on its own evaluation; independent commentary notes non-zero FPR means human writing can occasionally be flagged at scale [79][80].
- Compliance objective: human creates every word -> software transcribes mechanically -> harness pastes exactly those words -> no LLM edits them. Pipeline: human speech -> speech-to-text -> raw_transcript.txt -> human corrects transcription errors -> approved_answer.txt -> DETERMINISTIC browser fill() -> read field value back -> exact-string comparison -> submit gate. Never instruct "improve this answer and put it into the form"; instruct "paste approved_answer.txt verbatim; no grammar fixes, shortening, punctuation changes, intro or conclusion". For auditability let ordinary browser code insert the text (e.g. `await field.fill(answer); assert await field.input_value() == answer`). Keep an audit record per answer (question_id, transcript_file, sha256, browser_value_verified_exact, ai_rewrite=false, approved_at); the hash is provenance, not detector evasion.
- Harness suitability for verbatim fill: Browser Use / Browser Harness excellent (literal file -> deterministic fill -> verify DOM value) [1]; Stagehand/Browserbase excellent (keep critical text insertion outside act()) [15]; Claude Code + Chrome good (file as immutable source, form_input; explicitly prohibit rewriting; Claude is transport/controller, not author) [5]; Skyvern good (supply text as structured workflow input) [61]; Nova Act good for developers (literal string in Python) [66]; Manus usable but less auditable; Gemini Auto Browse / Comet / Copilot / Genspark / Neon possible but not recommended for strict authorship compliance.
- Do not run the transcript through "humanizers", paraphrasers or detector-optimization loops.

## Operational recommendation
Normal path: orchestrator (Claude Code/Codex) + structured profile + human-authored answer repository -> Browser Use Browser Harness on a dedicated real Chrome profile -> fill, dropdowns, upload, navigate -> validator -> ONE human approval -> deterministic Submit click -> save confirmation URL/screenshot. Fallback chain: Browser Harness fails on site UI -> Claude in Chrome with explicit tools -> Manus Browser Operator -> Browserbase/Stagehand persistent cloud profile -> human takeover only for CAPTCHA/MFA/site anomaly. Retry budget: normal click/select 2 attempts; local file upload 2; navigation/state loss 1 recovery; CAPTCHA/MFA immediate handoff; unknown factual answer immediate handoff; essay not in human file immediate handoff; final submit exactly one approval gate. Central insight: submission approval should be an application-level capability token (capability submit_application, origin, application_id, expires_at, single_use), not an LLM conversation; once issued, the browser layer clicks the specific final control once and the LLM gets no chance to reinterpret it as "unsafe". Immediate change for the current Claude deployment: Claude Code Auto mode OFF for applications; explicit browser tools ALLOW; per-site Always allow; CV directory Read ALLOW; free-text from human source files only; CAPTCHA/MFA human; final submit one explicit approval. This removes the avoidable classifier from the hot path but will not eliminate genuine Claude-in-Chrome bugs; keep Browser Harness underneath as the deterministic escape hatch [32][8].

## Sources (primary list from the leg)
https://openai.com/index/introducing-chatgpt-agent/
https://platform.openai.com/docs/guides/tools-computer-use
https://openai.com/index/introducing-chatgpt-atlas/
https://openai.com/index/browsecomp/
https://support.google.com/chrome/answer/16821166?hl=en
https://blog.google/products-and-platforms/products/chrome/gemini-3-auto-browse/
https://one.google.com/intl/en/about/google-ai-plans/
https://www.perplexity.ai/comet
https://manus.im/docs/features/browser-operator
https://manus.im/docs/integrations/manus-browser-operator
https://manus.im/blog/manus-browser-operator
https://manus.im/blog/deep-dive-browser-operator-connector
https://manus.im/docs/integrations/manus-api
https://browser-use.com/
https://browser-use.com/form-filling
https://browser-use.com/posts/ai-browser-agent-benchmark
https://github.com/browser-use/benchmark
https://github.com/browser-use/plugins
https://browserbase.com/pricing/
https://browserbase.com/browsers/
https://docs.browserbase.com/use-cases/automating-form-submissions
https://docs.stagehand.dev/v3/configuration/browser
https://docs.browserbase.com/platform/identity/overview
https://www.skyvern.com/docs/developers/optimization/browser-profiles
https://www.skyvern.com/docs/developers/browser-automation
https://www.skyvern.com/
https://aws.amazon.com/nova/act/
https://docs.aws.amazon.com/ai/responsible-ai/nova-act/overview.html
https://huggingface.co/datasets/amazon-agi/Amazon-Nova-Act-v1.0-evals
https://investor.opera.com/news-releases/news-release-details/browse-your-own-ai-inside-opera-neon
https://www.operaneon.com/
https://www.genspark.ai/business/products/browser
https://www.genspark.ai/helpcenter/chrome-extension
https://arxiv.org/html/2607.27183v1
https://www.pivotal-research.org/fellowships/ai-safety-fellowship

## Complete URL index [N] -> URL
[1] https://browser-use.com/
[2] https://browser-use.com/form-filling
[3] https://browser-use.com/posts/ai-browser-agent-benchmark
[4] https://github.com/browser-use/benchmark
[5] https://code.claude.com/docs/en/chrome
[6] https://support.claude.com/en/articles/12902446-claude-in-chrome-permissions-guide
[7] https://docs.anthropic.com/en/docs/claude-code/permissions
[8] https://github.com/anthropics/claude-code/issues/98169
[9] https://manus.im/docs/features/browser-operator
[10] https://manus.im/blog/deep-dive-browser-operator-connector
[11] https://manus.im/docs/integrations/manus-api
[12] https://manus.im/docs/features/desktop
[13] https://browserbase.com/browsers/
[14] https://docs.stagehand.dev/v3/configuration/browser
[15] https://docs.browserbase.com/use-cases/automating-form-submissions
[16] https://browserbase.com/pricing/
[17] https://help.openai.com/en/articles/11752874-chatgpt-agent
[18] https://support.google.com/chrome/answer/16821166?hl=en
[19] https://support.microsoft.com/en-us/microsoft-copilot/browse-with-copilot
[20] https://www.perplexity.ai/help-center/en/articles/12781449-comet-for-enterprise
[21] https://help.openai.com/en/articles/20001371-chatgpt-atlas-retirement
[22] https://help.openai.com/en/articles/10128477-chatgpt-enterprise-and-edu-release-notes
[23] https://openai.com/index/introducing-chatgpt-agent/
[24] https://developers.openai.com/codex/chrome-extension
[25] https://help.openai.com/en/articles/6825453-chatgpt-release-notes
[26] https://developers.openai.com/api/docs/guides/tools-computer-use
[27] https://openai.com/index/new-tools-for-building-agents/
[28] https://developers.openai.com/api/docs/guides/agents
[29] https://support.claude.com/en/articles/12012173-get-started-with-claude-in-chrome
[30] https://docs.anthropic.com/en/docs/claude-code/cli-reference
[31] https://www.anthropic.com/news/claude-sonnet-4-5
[32] https://github.com/anthropics/claude-code/issues/80632
[33] https://github.com/anthropics/claude-code/issues/85140
[34] https://github.com/anthropics/claude-code/issues/25518
[35] https://github.com/anthropics/claude-code/issues/63963
[36] https://docs.anthropic.com/en/docs/claude-code/mcp
[37] https://blog.google/products-and-platforms/products/chrome/gemini-3-auto-browse/
[38] https://blog.google/innovation-and-ai/models-and-research/google-deepmind/gemini-computer-use-model/
[39] https://www.theverge.com/ai-artificial-intelligence/1005451/google-gemini-free-flash-lite-only
[40] https://www.perplexity.ai/help-center/comet/en/articles/11732243-advice-and-use-cases
[41] https://www.perplexity.ai/comet
[42] https://www.perplexity.ai/help-center/en/articles/13531023-managing-comet-assistant-permissions
[43] https://www.perplexity.ai/help-center/en/articles/11187416-which-perplexity-subscription-plan-is-right-for-you
[44] https://geotoolbox.ai/blog/perplexity-comet
[45] https://www.techradar.com/pro/security/perplexity-responds-to-comet-browser-vulnerability-claims-argues-fake-news
[46] https://support.microsoft.com/en-us/microsoft-365-copilot/using-copilot-tasks
[47] https://www.reuters.com/technology/microsoft-launches-ai-powered-365-premium-bundle-1999-per-month-2025-10-01/
[48] https://support.microsoft.com/en-gb/edge/copilot/using-microsoft-copilot-in-edge-at-work
[49] https://manus.im/docs/integrations/manus-browser-operator
[50] https://manus.im/blog/manus-browser-operator
[51] https://manus.im/docs/introduction/plans
[52] https://open.manus.im/docs/v2/file.upload
[53] https://www.genspark.ai/business/products/browser
[54] https://www.genspark.ai/helpcenter/chrome-extension
[55] https://www.bitbrowser.net/blog/best-agent-browsers
[56] https://www.operaneon.com/
[57] https://investor.opera.com/news-releases/news-release-details/browse-your-own-ai-inside-opera-neon
[58] https://hirethebot.ai/agents/opera-neon
[59] https://github.com/browser-use/plugins
[60] https://www.skyvern.com/docs/developers/optimization/browser-profiles
[61] https://www.skyvern.com/docs/developers/browser-automation
[62] https://www.skyvern.com/
[63] https://docs.browserbase.com/platform/identity/overview
[64] https://arxiv.org/html/2506.03011v1
[65] https://openreview.net/forum?id=OJd3ayDDoF
[66] https://aws.amazon.com/nova/act/
[67] https://docs.aws.amazon.com/ai/responsible-ai/nova-act/overview.html
[68] https://huggingface.co/datasets/amazon-agi/Amazon-Nova-Act-v1.0-evals
[69] https://aws.amazon.com/blogs/machine-learning/amazon-nova-act-sdk-preview-path-to-production-for-browser-automation-agents/
[70] https://rtrvr.ai/use-cases/ai-form-filling
[71] https://openai.com/index/browsecomp/
[72] https://www.notte.cc/glossary/ai-browser-agents/what-are-browser-agent-benchmarks
[73] https://resources.rework.com/tools/ai-agents/best-ai-browser-agents-2026
[74] https://www.testmuai.com/blog/state-of-ai-browser-agents-2026/
[75] https://code.claude.com/docs/en/auto-mode-config
[76] https://github.com/anthropics/claude-code/issues/84627
[77] https://github.com/anthropics/claude-code/issues/32561
[78] https://www.pivotal-research.org/fellowships/ai-safety-fellowship
[79] https://arxiv.org/html/2607.27183v1
[80] https://epoch.ai/data-insights/ai-detectors-false-negatives
[81] https://openai.com/index/introducing-chatgpt-atlas/
[82] https://one.google.com/intl/en/about/google-ai-plans/
[83] https://www.perplexity.ai/help-center/comet/en/articles/12658082-control-what-comet-assistant-can-use
