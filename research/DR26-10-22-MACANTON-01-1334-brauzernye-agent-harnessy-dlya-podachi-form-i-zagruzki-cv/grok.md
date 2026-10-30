# DR26-10-22-MACANTON-01-1334 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll research the live product docs and benchmarks, then put the full comparison in one report.A. Scope and method. Claims below are from pages opened in this session on 2026-10-22. Where a product page was not opened in full, the cell says so. No file was written.A. Scope

This is a product comparison for one job: a logged-in person in the US fills multi-page application forms (Greenhouse, Ashby, Airtable, Google Forms, fellowship portals), attaches a local CV PDF, uses dropdowns, and clicks Submit after one human approval. Date of the sources below: 2026-10-22, unless the page itself carries an earlier date.

A claim is tied to a page opened in this session. If the page does not say it, the cell says "not on the page opened." No Greenhouse, Ashby, Airtable, or Google Forms CV-upload success rate was on any page opened here. Public boards (WebArena, OSWorld, Online-Mind2Web, BrowseComp) were not re-opened in this session, so no score from those boards is quoted.

B. Comparison

| Product | 1. Whose browser | 2. Local CV from disk | 3. Submit policy (exact name) | 4. CAPTCHA | 5. Measured reliability | 6. Price and US | 7. Failures seen in sources | 8. Driven by another agent |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Claude in Chrome via Claude Code | User's own Chrome, Edge, or other Chromium. Shares that browser's login state. Not WSL. | Yes. Official: "attach files from your machine." Session must be allowed to Read the file. One upload ≤ 10 MB total. Refuses multi-hard-link files and credential-style names (`.env`, `.pem`, `.key`, anything under `.ssh`) from Claude Code v2.1.293. | Two layers. Extension modes: **Manually approve (Manual)**, **Automatically approve (Auto)**, **Skip all approvals (Skip)**. **Always allow actions on this site** still asks before a download, entering potentially sensitive information, and granting authorizations. Regardless of mode, explicit permission is required for inputting potentially sensitive information. Purchases, creating accounts, credit-card or ID data, and permanent deletions are prohibited. Claude Code **auto** mode adds a second classifier. `bypassPermissions` does not turn that classifier off for these browser tools. | Pauses and asks the person to solve it. | No form-portal number on the docs. GitHub reports show the classifier denying `form_input` and file injection even when the user is watching. | Direct Anthropic plan: Pro, Max, Team, or Enterprise, plus `/login`. Not an API key, Bedrock, Vertex, or Foundry. Dollar price not on the page opened. US: yes, Chrome Web Store extension. | Classifier "Blocked by classifier" / "[Auto-Mode Bypass]" with a wrong hint to add a Bash rule. Service worker idle: "Receiving end does not exist" (fix: `/chrome` → Reconnect). JS `alert`/`confirm` blocks CDP. Extension org must match Claude Code org. HIPAA orgs off until an Owner turns it on. `file_upload` "only files the user has shared with this session" (issue, 2026-07-23). CDP `DOM.setFileInputFiles` returns `{"code":-32000,"message":"Not allowed"}` on google.com (issue, 2026-03-08). | Yes. Claude Code CLI (`claude --chrome`), VS Code, MCP tools `mcp__claude-in-chrome__*`. A repo cannot turn Chrome on by itself (`CLAUDE_CODE_ENABLE_CFC` in project settings is ignored). |
| ChatGPT cloud browser (ChatGPT Work) | Separate cloud computer. Own cookies. Does not use the device browser's tabs, history, passwords, cookies, extensions, or existing sign-ins. FAQ: "Will ChatGPT use accounts already signed in on my device? No." | Not described on the page opened. | **Settings > Cloud browser**: **Always ask** (default), **Auto approve**, **Always allow** (not recommended). Per-site allow/block overrides the default. "Website access permission is separate from approval for consequential actions." Booking, payment, and other hard-to-reverse or financial/legal/account actions ask for confirmation in chat before they run. | Not named. Pauses for input, sign-in, or confirmation. Take over via a link if it gets stuck. Some sites block automated agents. | Not on the page. | Paid ChatGPT plans except Free and Go, supported regions, rollout and workspace permissions. Dollar price not on the page. US: in supported regions. | Site blocks agent traffic. Sign-in is a separate secure form (credentials not visible to the model). Session persists until expiry or **Settings > Cloud browser > Browser data > Cookies** is cleared. | The page describes starting a task in ChatGPT Work on web or mobile. A public orchestrator API for this consumer cloud browser was not on the page. |
| ChatGPT Atlas | Retired. Stopped working 2026-08-09. Cookies and sign-ins do not transfer. | Dead product. | Dead product. | n/a | n/a | Was macOS, several ChatGPT plans, launched 2025-10-21. | Gone after 2026-08-09. | No. |
| ChatGPT desktop built-in browser | Inside the ChatGPT desktop app, for the person to inspect and annotate. Distinct from cloud browser. Whether it is the daily Chrome profile: not stated on the cloud-browser FAQ. | Not on the page opened. | Not on the page opened. | Not on the page opened. | Not on the page opened. | Same ChatGPT Work availability as above, where the desktop app exists. | Not on the page opened. | Not on the page opened. |
| ChatGPT Chrome extension / sidebar | Named by OpenAI as the Chrome-side replacement after Atlas. Spec of whose cookies it uses: not opened. | Not on a page opened. | Not on a page opened. | Not on a page opened. | Not on a page opened. | Not on a page opened. | Not on a page opened. | Not on a page opened. |
| Operator (standalone) | Not a current product on the pages opened. Browser work now sits in ChatGPT Work cloud browser, the desktop built-in browser, and the Chrome extension. | n/a | n/a | n/a | n/a | n/a | n/a | n/a |
| Gemini in Chrome auto browse | Local desktop Chrome, including sites you are signed into. If the machine sleeps or the task is on mobile, it can fall back to a remote browser that does not keep going on your local login. | The help page says Gemini may share "name, contact information, files, preferences" with the site. It does not document attaching a local PDF to a third-party `<input type="file">`. | Every web task asks first: review the plan, then **Start Task**. Aims to confirm before sending communications, modifying data, **submitting web forms**, scheduling events, and highly sensitive financial or health sites. **Take over task** for financial transactions, terms of service, and creating an account. Sign-in: **Settings → AI innovations → Gemini in Chrome → Sites Gemini can sign you in to**, and **Let Gemini browse for you**. Password Manager does not share the password with Gemini. | CAPTCHA is not named. **Take over task** / **Resume** / **Give back task** is the handback. | Google lists its own failure classes: wrong click, purchase without permission, wrong cart quantity, false completion, unnecessary takeover. No portal benchmark on the page. | US, 18+, English, personal Google account, desktop Chrome, not Incognito, not a school account unless an admin enables it. **Google AI Pro**: up to 20 multi-step tasks/day. **Google AI Ultra**: up to 200/day. A secondary write-up (2026-05-25) lists Pro at $19.99/mo and Ultra at $199.99/mo in the US. Those dollar figures are not on the help page. | Remote-browser fallback drops the local session. Confirm-every-task is more clicks than "approve once." Gradual rollout. | No consumer auto-browse API or MCP on the help page. Gemini's developer computer-use API is a different product and was not opened here. |
| Project Mariner | Shut down 2026-05-04. Landing page: technology moved to other Google products. The Verge (2026-05-06) points at Gemini Agent and AI Mode. | Dead as a product. | Dead. | n/a | A wiki cites an old WebVoyager 83.5% figure. That wiki was not treated as a current measurement, and the product is off. | Was an Ultra-tier experiment. | Gone. | No. |
| Perplexity Comet | Comet is a Chromium browser. Cookies, signed-in sessions, and autofill stay on the device unless you sync settings. Personal Computer (Mac) hands web tasks to Comet on logged-in sites. | Personal Computer asks for Full Disk Access so it can read folders you connect. Putting a PDF into a site's file input is not described in the help excerpts opened. | Help text: Comet can "click, type, and submit forms" and run "form submissions or checkout flows" **under supervision**. You can intervene. Exact per-site pre-approval setting name: not in the excerpts. Personal Computer: the phone approves sensitive actions. | Not named in the excerpts. | Not in the excerpts. | Comet help pages do not state a Pro/Max price. Personal Computer is a Mac install. US: the help center does not restrict it. Amazon litigation was not re-opened. | Supervision means you watch. Checkout is in scope of what they claim, which is the opposite of a tight submit gate. | Not in the excerpts. MCP/CLI for Comet was not opened. |
| Microsoft Copilot Cowork local browser | Your Edge on the device. Same work/school SSO, cookies, and sessions. Credentials stay on the device. Not the desktop Copilot app. Not InPrivate or guest. Not a personal Google Chrome profile. | Not documented. | First run: **I understand** (stored in Edge). Then "normal per-site and per-action permissions." Actions that submit personal data, or are otherwise sensitive or consequential, show an approval card: **Approve** (once) or **Reject**. Admin switch, off by default: **Allow Cowork to use the Microsoft Edge browser to perform tasks on behalf of users**. Edge policy name: `CopilotCoworkToolActionsEnabled`. | Hands the tab back for CAPTCHA, password, MFA, locked account, password reset, or account-settings changes. | Not on the page. | Microsoft 365 work or school tenant. Edge ≥ 152.0.4191.53. Cowork on the web at `https://copilot.cloud.microsoft.com`, Edge only. Page dated 2026-09-23. Consumer Google-SSO use: no. | Admin gate. Hidden Edge tab. Interrupted tasks resume only after you say continue/resume. DLP can block a step. | The page is a user-facing Cowork chat, not an orchestrator API. |
| Manus | Default work is a cloud VM (credits cover LLM tokens, virtual machines, third-party APIs). Manus Desktop **My Computer** is local folders you add, via the machine's command line. Manus 2.0 Computer Use: "its own visible workspace" using files, browser, and apps you approved. That is not documented as your daily Chrome profile. | Local files only inside folders you add in My Computer. Upload of those files into a logged-in third-party form: not documented. | Not named. You approve folders at setup. | Not on the pages opened. | Not on the pages opened. | Help center, updated 2026-10-22: Free $0, 300 credits/day, monthly cap 1,500 on daily credits, 1 concurrent task. Pro from $20/month, from 4,000 credits/month, about 17% off annual. Team from $40 per seat/month. US: no geo block on the help page. Manus is part of Meta (updates page, 2025-12-29). | Cloud sandbox is the default browser. Credit burn is task-dependent. Hybrid desktop still needs the desktop app running. | Docs describe the product UI and scheduled tasks. A public "hand this task to Manus" API was not on the pages opened. |
| Genspark | Not opened this session. No verified row. | | | | | | | |
| Fellou | Not opened this session. No verified row. | | | | | | | |
| Opera Neon | Neon is its own browser on Windows and macOS, with its own logged-in session. Not your daily Chrome profile unless you imported data (import was not documented on the pages opened). | Not documented. | **Neon Do** plans steps, runs them in the live browser, and you can pause or take over. MCP write tools (clicks, keyboard, **Fill forms**) are **disabled by default**. You enable them. Payments and logins: the FAQ page lists the question; the answer body was not in the extract. | Not named. Pause / take over exists. | Not on the pages. | Free plan: browser + MCP connector + `opera-browser-cli`. Standard plan unlocks Neon's own agents, including Neon Do. Public launch price $19.90/month (Opera newsroom, 2025-12-11, and Neon news, 2025-12-11). The subscription page opened today shows the Free vs Standard capability split and does not repeat the dollar amount. US: Windows and macOS, no geo limit stated. | `opera-browser-cli` sets automation flags that sites can see (can be turned off by env var). Neon Do itself does not use that CDP flag. MCP from a cloud client is not how the CLI works. The CLI is local. | Yes. MCP server (OAuth, sign-in required) and `opera-browser-cli` (no OAuth). Free of charge for your own agent (Opera news, 2026-08-14). `claude mcp` style connection is the documented path. |
| Browser Use, open source | `Browser.from_system_chrome()` reuses a Chrome profile (README on GitHub). Otherwise it launches its own Chromium. | The README documents profile reuse, not the file-input call. A local Playwright-class browser can set a file on disk. The exact method was not on the page opened. | No vendor submit dialog. You write the stop. | No vendor solver in the local library page opened. | Vendor pricing page shows an internal "Quality / 100" table (Claude Opus 5.5 at 59.4). That is their eval, not WebArena or a job form. | Library is open source. You pay your own model key. US: yes. | MCP stdio has a history of "CLI addon is not installed" unless installed as `browser-use[cli]` (issue, 2025-10). Cloud vs local is easy to mix up. | Yes. Local MCP: `uvx --from 'browser-use[cli]' browser-use --mcp`, and `claude mcp add browser-use -- uvx --from 'browser-use[cli]' browser-use --mcp`. |
| Browser Use Cloud | Cloud stealth browser. Profiles store cookies in the cloud. Not your daily Chrome. | V4 workspace upload (files staged in their cloud, then attached) was not re-opened this session. Do not treat it as a local file picker. | No "approve submit" setting on the pricing pages. | Not on the pricing pages. | Same vendor quality table as above. Blog (2026-10-21) says their cloud MCP `run_session` was not in that eval. | Machine-readable pricing, last verified 2026-09-03: browser time **$0.02 per browser-hour**, residential proxy $5/GB, direct $0.20/GB, model tokens at provider rates plus 20%, no subscription, $1 signup credit, concurrency 10 at $0 up to 1,000 at $25,000 paid. A different docs page still lists **$0.06/hour** Pay As You Go and **$0.03/hour** on Business/Scaleup. Both pages were returned in this session. Use the 2026-09-03 page plus the 2026-10-21 blog ($0.02) unless your invoice shows the older rate. US: yes. | Two live price pages disagree. Cloud IP is still blocked by some sites. | Yes. HTTP MCP `https://api.browser-use.com/v3/mcp` with a bearer key (blog, 2026-10-21). Older docs also show `https://api.browser-use.com/mcp`. |
| browser-harness (2026) | Vendor post (2026-10-17): drives your own Chrome. One connection approval on Chrome 144+ via `chrome://inspect`. | Not in the excerpt. | Not in the excerpt. The agent sees the whole logged-in Chrome, not a sandbox. | Not in the excerpt. | Not in the excerpt. | MIT, no API key, `uvx --from 'browser-harness[mcp]' browser-harness-mcp`. | You are pointed at your real accounts. | Yes, MCP, from that post. The install doc itself was not opened. |
| Skyvern | Cloud browser session or saved browser profile (cookies and sign-ins in Skyvern). Not your daily Chrome. Session lifetime 5 minutes to 4 hours, extendable to 6. | `POST https://api.skyvern.com/v1/upload_file` multipart. The file goes to Skyvern storage (`s3_uri`, `presigned_url`, `file_id`). The agent then uses that stored file. Not an OS file dialog on your Mac. | No per-site "pre-approve Submit" name on the pages opened. You can **Take control** of the live stream, then **Stop controlling**. | Plans: Free and Hobby **Basic CAPTCHA solver**, Pro and Enterprise **Advanced**. Extension `captcha-solver` is Cloud only. Not guaranteed by the word "solver" alone. Take control remains the fallback. Docs also say the solver is an extension you turn on. | Pricing page converts credits to "estimated actions": Free ~170 actions per 5,000 credits, Hobby ~1,200/month, Pro ~6,200/month. A credit is one click, type, dropdown, extract, or navigation. A 10-field form plus submit is given as ~11 credits. That is their estimator, not a success rate. | Free $0, 5,000 one-time credits, 1 concurrent. Hobby $29/month, 30,000 credits, stored credentials. Pro $149/month, 150,000 credits, 2FA/TOTP, 1Password, residential proxy. Enterprise custom. Page seen 2026-10-22. A second billing doc still says Free ~200 actions. US: yes. | Datacenter proxy on Free and Hobby. Login is a stored credential or a saved profile, not today's Chrome cookies. Sessions bill while open, including idle. | Yes. REST API and webhooks. |
| Stagehand + Browserbase | Stagehand v4: `localBrowser.launch()` is a local Chromium with their extension loaded. That is not documented as your existing Chrome profile. `browserbase.launch()` is their cloud browser. | Playwright-style control is claimed. A `setInputFiles` page was not opened. Local Chromium can reach a local path. Cloud Browserbase cannot see your disk unless you upload into the session. | No consumer "approve Submit" toggle. v3/v4 have a domain allow/block policy. You code the stop before the submit click. | Browserbase Developer and Startup plan pages list captcha solving. Stagehand local does not. | Not on the pages opened. | Stagehand is MIT and free. You pay the model. Browserbase pricing page: Developer **$20/mo**, 100 browser hours then **$0.12/hour**, 25 concurrent. Startup **$99/mo**, 500 hours then **$0.10/hour**. Free tier exists on that page. A secondary check (2026-10-05) says Free is 1 hour and 3 concurrent. US: yes. | Local browser is a fresh Chromium unless you wire your own profile (not in the snippet). Cloud sessions are isolated. LLM `act()` can drift. Use a selector for the submit click. | Yes. Stagehand is a library (TypeScript, Python, Go). Browserbase has an MCP path. The official MCP reference was not the page opened. Cloudflare Browser Run can host Stagehand. That is another cloud browser, not your profile. |
| Amazon Nova Act SDK | SDK drives a browser you start (Playwright-class). It is not documented on the security page as attaching to your already-open Chrome profile. | **Off by default.** On only if you set `SecurityOptions.allowed_file_upload_paths`. `file://` open is also off until `allowed_file_open_paths`. `[]` disables. `["*"]` allows every path. | No product setting. You supply `state_guardrail`. Return `GuardrailDecision.BLOCK` and `act()` raises `ActStateGuardrailError`. That is the one-approval gate: block any URL that is not the application host, and do not call the submit instruction until you say so. | Not named. Prompt injection from the page is called out as unsolved. | The security page has no benchmark. A ">90%" marketing figure was not on this page and is not used. | AWS SDK. Unit price and region were not on the security page. | Default is no upload. `["*"]` is the foot-gun. The model can still obey instructions it reads on the page. | Yes. Python SDK. An orchestrator calls `nova.act("...")`. |
| OpenHands browsing | Not re-opened this session. No verified row. Prior knowledge that it embeds browser-use inside a sandbox is not used as a fact. | | | | | | | |
| Agent-E | Not opened. Not treated as a maintained October 2026 product. | | | | | | | |

C. Top 3 for "fill and submit from my profile and my CV, I approve once"

Ranked for this job only: real login, local PDF, one approval before Submit, few false refusals.

1. Claude in Chrome, Manual mode, not auto mode

This is the only major vendor whose own docs, opened today, say both "shares your browser's login state" and "attach files from your machine."

Setup:

- Install the extension `fcoeoabgfenejglbffodgkkbkcdhcgfn` (Chrome Web Store, version 1.0.36 or later).
- `claude` then `/login` on a Pro, Max, Team, or Enterprise account. An API key leaves Chrome off.
- `claude --chrome`, or `/chrome` → **Enabled by default**.
- Extension permission mode: **Manually approve**. In the Cowork side panel the default is **Automatically approve**. For a job form, Manual is the mode that matches "I approve the upload and the submit."
- Put the CV somewhere boring (`~/Applications-CV/anton.pdf`). Not under `.ssh`, not named `.env` / `.pem` / `.key`. One hard link (copy it out of any package store). Under 10 MB. The session must be allowed to Read it.
- In «внутренний путь лаборатории» set the session to Manual (`default`), not `auto` and not `bypassPermissions`. Project `.claude/settings.json` cannot grant `autoMode` rules, and `bypassPermissions` still runs the Chrome tools through the classifier.
- Prompt, concrete: open this exact application URL, use the already signed-in account, fill from this profile file, upload this exact PDF path, stop before Submit and show the field list. You click Allow on the upload card and on the submit card.
- If a modal dialog is up, dismiss it yourself. If tools die with "Receiving end does not exist," `/chrome` → Reconnect extension.
- Essay fields: see section E. Do not let the model draft them.

What still prompts after **Always allow actions on this site**: download, sensitive information, authorizations. A CV and an essay can land in "potentially sensitive information." Budget one card for that, not zero.

2. Nova Act SDK, or Browser Use on your real Chrome profile

Use this when Claude's classifier keeps denying the upload. There is no second safety model. You own the stop.

Nova Act, because the allow-list is an official API:

```python
from nova_act import NovaAct, SecurityOptions, GuardrailDecision, GuardrailInputState
from urllib.parse import urlparse

HOSTS = {"jobs.ashbyhq.com", "boards.greenhouse.io"}

def gate(state: GuardrailInputState) -> GuardrailDecision:
    host = urlparse(state.browser_url).hostname or ""
    return GuardrailDecision.PASS if host in HOSTS else GuardrailDecision.BLOCK

with NovaAct(
    starting_page="https://jobs.ashbyhq.com/...",
    SecurityOptions(allowed_file_upload_paths=["/Users/anton/Applications-CV/*"]),
    state_guardrail=gate,
) as nova:
    nova.act("Fill the form from /Users/anton/Applications-CV/profile.txt. Upload /Users/anton/Applications-CV/anton.pdf. Do not click Submit.")
```

You read the page, then run a second `act` that only clicks Submit. `allowed_file_upload_paths` defaults to off. Do not pass `["*"]`.

Browser Use, when the portal login only exists in your daily Chrome:

- `Browser.from_system_chrome()` (GitHub README).
- Orchestrator: `claude mcp add browser-use -- uvx --from 'browser-use[cli]' browser-use --mcp`.
- The task text must say: fill, upload this path, stop before Submit. There is no vendor card. The stop is the prompt plus your script.
- Cloud Browser Use is the wrong variant for Google SSO. It is a different browser with cloud cookies.

3. Gemini in Chrome auto browse, if you already pay for AI Pro or Ultra

Use it for the clicking and the built-in "confirm before submit." Do not count on it to attach the PDF.

Setup from the help page:

- US, 18+, English, personal Google account, current desktop Chrome, Safe Browsing Enhanced or Standard, not Incognito.
- AI Pro (20 tasks/day) or Ultra (200/day).
- **Settings → AI innovations → Gemini in Chrome → Let Gemini browse for you** on.
- Ask for the form fill. Read the plan. Click **Start Task**. That click is mandatory on every web task.
- When it reaches the file field, click **Take over task**, attach the PDF yourself, then **Resume** or **Give back task**.
- It aims to ask again before **Submitting web forms**. That second confirm is the submit gate. It is not a per-site "always allow submit."
- Keep the Mac awake. If it falls over to the remote browser, your Google SSO on that site is gone and you will be asked to sign in again.

Microsoft Cowork is the same shape (real browser, **Approve** card before submitting personal data, CAPTCHA handback) only if a tenant admin has enabled Cowork Browsing and you are in Edge with the work account. It is the wrong tool for a personal Chrome profile, and local PDF upload is not in the doc (2026-09-23).

Do not pick ChatGPT cloud browser for this job. OpenAI's own FAQ says it will not use accounts already signed in on your device, and the help page does not document a local CV upload. **Always allow** still asks before a consequential action.

D. Unblocking Claude Code's auto-mode classifier for file upload

Official pages: `https://code.claude.com/docs/en/chrome`, `https://code.claude.com/docs/en/permission-modes`, `https://code.claude.com/docs/en/auto-mode-config`, `https://support.claude.com/en/articles/12902446-claude-in-chrome-permissions-guide`. All opened 2026-10-22.

What the classifier is. Auto mode sends tool calls through a second model. Deny rules and explicit ask rules run before it. `permissions.deny` cannot be overridden by the classifier or by anything you type. `permissions.ask` always prompts, and the classifier cannot auto-approve a matching action.

Where rules are read. `autoMode` is read from «внутренний путь лаборатории», managed settings, or `--settings` / the Agent SDK. It is not read from `.claude/settings.json` or `.claude/settings.local.json`. A repo cannot grant itself trust. `autoMode.allow`, `soft_deny`, and `hard_deny` are prose, not tool-name globs. Keep `"$defaults"` in those lists or you replace the built-in rules.

What does not unblock Chrome uploads:

- `permissions.allow` entries such as `mcp__claude-in-chrome__*` or `mcp__claude-in-chrome__file_upload`. Issue 95842 (2026-09-21) added those plus an `autoMode.allow` sentence and the next call was still denied, reason `[Auto-Mode Bypass]`, with the hint "add a Bash permission rule." That hint is wrong for MCP tools. The same reporter's file upload to the same app was denied.
- `bypassPermissions` / `--dangerously-skip-permissions`. Boris Cherny, on issue 86451 (2026-08-13): Claude in Chrome tools are deliberately held to the classifier even in `bypassPermissions`, so signed-in browser actions are never fully unreviewed. Issue 99834 (2026-10-06) reproduces that on the desktop app. An ordinary MCP server is not treated this way. Chrome is the exception.
- `autoMode.environment` trusted domains. That field tells the classifier which hosts are inside your boundary so a push or a fetch is not treated as exfiltration. It is the right fix when the denial reason is an unrecognized host. It is not documented as a waiver for "upload this PDF" or "click Submit."
- Issue 86451's suggested workaround (an explicit allow rule is resolved before the classifier) is older than the `[Auto-Mode Bypass]` denials in 95842 and 99834. Do not treat it as working in October 2026.
- **Skip all approvals** in the extension turns off the extension's own check. It does not remove the Claude Code classifier, and the help center says to use it only when you trust every action.

What actually gets a CV onto the form:

1. Run the application session in Manual mode (`default`), not auto. Approve `file_upload` on the card. That card is the one-time approval. The classifier is not in the path the same way.
2. Satisfy the file rules on `https://code.claude.com/docs/en/chrome`: Read allowed, ≤ 10 MB, single hard link, not a credential path.
3. If you insist on staying in auto, add a `permissions.ask` rule for the submit action so the classifier cannot auto-approve it. For the upload, an `autoMode.allow` sentence in «внутренний путь лаборатории» is the documented exception to soft blocks, and a fresh user message that names the exact file and the exact origin is what the docs say can clear a soft deny. Reporters still got `[Auto-Mode Bypass]` after doing that. If the reason string stays `[Auto-Mode Bypass]` or "Blocked by classifier," leave auto mode for that session. There is no supported in-CLI override on the current docs.
4. If the error is `{"code":-32000,"message":"Not allowed"}`, that is Chrome refusing `DOM.setFileInputFiles` (seen on google.com, issue 32179, 2026-03-08), not the classifier. A settings.json allow-list does not fix it. Google's own properties are a known dead end for this tool.
5. If the error is "only files the user has shared with this session" (issue 80632, 2026-07-23), drag-and-drop and `@` mentions did not satisfy `file_upload` in that report. The current chrome doc says a file the session may Read can be uploaded, and gives a path-based example (`logs/session.log`). Prefer a path the Read tool has already opened in that session, outside credential directories.

E. Voice in, verbatim out, and AI-text detectors

No harness opened here has a setting named "dictation" or "paste my words unchanged."

What works:

- You dictate (macOS Dictation, or any recorder) into a text file. The agent is instructed to insert that file's text and not to paraphrase, shorten, or "improve" it.
- The reliable mechanism is a deterministic fill: Nova Act, Browser Use, Stagehand, or Playwright given the exact string. A chat model (Claude, Gemini, ChatGPT, Neon Do, Comet, Manus) will rewrite unless the instruction forbids it, and it still sometimes rewrites.
- Safest detector path: you focus the field and dictate straight into it. The agent does not touch that field.

Pangram, the detector, is real. Pangram's own blog (2026-10-16) says Pangram 4's internal false-positive rate on 2,046,273 human documents was 0.0041%, and its false-negative rate on 519,993 AI documents was 0.3396%. Those are company numbers. Wikipedia (updated 2026-10-19) records the same 0.0041% claim. Substack added Pangram scans in July 2026. CNN (2026-10-21) and The Dartmouth (2026-10-22) both treat it as strong and not enough, by itself, to prove misconduct. The CEO told Big Think (2026-10-21) that the weak band is short text, about 50 to 100 words. AI-polished prose is in scope of "AI-assisted," not only fully generated prose. Wikipedia notes the company reported detecting AI involvement in 98.83% of samples run through 13 commercial "humanizer" tools. Do not run the essay through a humanizer.

The premise that Successif, Generator, and Pivotal forbid AI answers and run Pangram was not checked on their application pages in this session. It is not established here. If a program bans AI-written or AI-polished answers, the verbatim file path above is the one that matches the ban. Letting Claude or Gemini draft the essay does not.

F. Sources opened this session

- https://code.claude.com/docs/en/chrome (docs index date on the search hit 2026-10-20; fetched 2026-10-22)
- https://code.claude.com/docs/en/auto-mode-config (published line 2026-10-22)
- https://code.claude.com/docs/en/permission-modes (published line 2026-10-22; used for the mode table, not a full-page read)
- https://support.claude.com/en/articles/12902446-claude-in-chrome-permissions-guide (fetched 2026-10-22)
- https://github.com/anthropics/claude-code/issues/95842 (2026-09-21)
- https://github.com/anthropics/claude-code/issues/80910 (2026-07-24)
- https://github.com/anthropics/claude-code/issues/99834 (2026-10-06)
- https://github.com/anthropics/claude-code/issues/86451 (2026-08-13)
- https://github.com/anthropics/claude-code/issues/80632 (2026-07-23)
- https://github.com/anthropics/claude-code/issues/32179 (2026-03-08)
- https://help.openai.com/en/articles/20001280-using-cloud-browser-in-chatgpt (page says updated 14 hours before this session)
- https://help.openai.com/en/articles/20001371-chatgpt-atlas-retirement (updated 12 days before this session)
- https://help.openai.com/en/articles/20001371-evolving-atlas-into-chatgpt-for-browser-based-agentic-work (Atlas stops 2026-08-09; announcement window from 2026-07-09 in the localized copy)
- https://openai.com/index/introducing-chatgpt-atlas/ (2025-10-21, page now says Atlas is deprecated)
- https://support.google.com/gemini/answer/16821166?hl=en (fetched 2026-10-22)
- https://www.theverge.com/tech/925559/google-project-mariner-shut-down (2026-05-06)
- https://9to5google.com/2026/05/25/google-ai-plus-pro-ultra-gemini-features/ (2026-05-25, dollar prices, secondary)
- https://www.perplexity.ai/help-center/comet/en/articles/11732243-advice-and-use-cases (modified 2026-09-14)
- https://www.perplexity.ai/help-center/comet/en/articles/11740986-local-data-device-storage (modified 2026-09-14)
- https://www.perplexity.ai/help-center/comet/en/articles/11734688-assistant-panel (modified 2026-09-14)
- https://www.perplexity.ai/personal-computer-setup (fetched via search 2026-10-22, no page date in the extract)
- https://learn.microsoft.com/en-us/microsoft-365/copilot/cowork/cowork-local-browser (ms.date 2026-09-23)
- https://docs.aws.amazon.com/nova-act/latest/userguide/sdk-security.html (fetched 2026-10-22)
- https://manus.im/docs/features/desktop (2026-09-28)
- https://manus.im/docs/introduction/plans (2026-02-05, vague tiers)
- https://help.manus.im/en/articles/11711111-what-is-the-current-membership-pricing-for-manus (help center says updated today, 2026-10-22)
- https://help.manus.im/en/articles/11711097-what-are-the-rules-for-credits-consumption-and-how-can-i-obtain-them (updated this week)
- https://manus.im/blog/introducing-manus-2-0 (Computer Use paragraph)
- https://manus.im/updates (Meta note 2025-12-29; desktop 2026-03-16)
- https://operaneon.com/subscription (fetched 2026-10-22)
- https://blogs.opera.com/news/2026/08/your-ai-agents-can-use-opera-neon-free-of-charge/ (2026-08-14)
- https://blogs.opera.com/news/2026/03/opera-neon-adds-mcp-connector-to-the-browser/ (2026-03-31)
- https://blogs.opera.com/news/2026/05/opera-browser-cli/ (2026-05-12)
- https://help.opera.com/en/neon-ai-faq/ (2025-09-17)
- https://www.operaneon.com/news/opera-neon-becomes-available-in-public-early-access (2025-12-11, $19.90)
- https://press.opera.com/2025/12/11/opera-opens-public-access-to-opera-neon-its-experimental-agentic-ai-browser/ (2025-12-11)
- https://browser-use.com/pricing.md (machine-readable, verified 2026-09-03)
- https://browser-use.com/pricing (live page, model quality table; conflicts with the older hourly rate)
- https://docs.browser-use.com/cloud/pricing ($0.06/hour page, conflict)
- https://docs.browser-use.com/open-source/customize/integrations/mcp-server (2026-10-16)
- https://github.com/browser-use/browser-use/blob/main/README.md (`Browser.from_system_chrome()`)
- https://browser-use.com/posts/best-mcp-servers-for-browser-automation (2026-10-17, browser-harness)
- https://browser-use.com/posts/best-cloud-browser-mcp-servers (2026-10-21)
- https://www.skyvern.com/pricing (seen 2026-10-22)
- https://www.skyvern.com/docs/browsers (2026-10-18)
- https://www.skyvern.com/docs/api-reference/files/upload-a-file (2026-10-08)
- https://www.skyvern.com/docs/cloud/account-settings/billing-usage (2026-06-18, action estimates differ slightly from the pricing page)
- https://browserbase.com/pricing/ (Developer $20, Startup $99)
- https://browserbase.com/stagehand/ (`localBrowser.launch()`, MIT)
- https://browserbase.com/blog/stagehand-v4/ (v4 post; byline dates disagree inside the page, August 10, 2026 vs a 2026-10-14 crawl stamp)
- https://www.pangram.com/blog/is-pangram-accurate (2026-10-16)
- https://en.wikipedia.org/wiki/Pangram_(AI_detector) (updated 2026-10-19)
- https://www.cnn.com/2026/10/21/tech/ai-detectors-pangram-literary-prizes-cec (2026-10-21)
- https://bigthink.com/science-tech/trust-issues/ (2026-10-21)
- https://www.thedartmouth.com/4bad28ba-df7f-4e58-aba8-cfcf61892765 (2026-10-22)

G. Coverage gaps, inside this report on purpose

Not opened, so no facts: Genspark, Fellou, OpenHands browsing, Agent-E, the ChatGPT desktop built-in browser feature page, the ChatGPT Chrome extension Work-mode spec, Nova Act unit price, Claude Pro/Max dollar price, ChatGPT Plus/Pro dollar price, Comet Pro/Max dollar price, Successif, Generator, and Pivotal application rules. Browser Use has two hourly rates on two live docs ($0.02 vs $0.06). Skyvern's own pages disagree on whether 5,000 credits are ~170 or ~200 actions. Stagehand's v4 post carries two dates. None of those conflicts were resolved by a third page.

H. Простыми словами

Твой обычный Клод умеет жить в твоём настоящем Хроме и прикрепить PDF с диска. Он часто сам себе запрещает это, пока включён «автопилот». Выключи автопилот на время заявки и нажми «разрешить» на загрузке файла и на кнопке Submit. Если он всё равно упирается, ту же работу делает маленькая программа на твоём Маке (Nova Act или Browser Use): она кликает в твоём Хроме, файл берёт с диска, а Submit ждёт твоей команды. Чат-боты в облаке сидят в чужом браузере и твои гугловские входы не видят. Текст ответов надиктуй сам в файл и вели вставить его слово в слово. Детектор Pangram ловит текст, который переписал ИИ, и «очеловечиватели» его не спасают.

🏛M · Grok 4.7 · DR26-10-22-MACANTON-01-1334
