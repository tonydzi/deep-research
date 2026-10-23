# DR26-07-14-FLEE-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok-deep-research (Expert/DeeperSearch)
dr_id: DR26-07-14-FLEE-01
topic: Fully unattended Deep Research fan-out orchestration
collected: 2026-07-16


stats: ~89 sources, Expert mode, thought 1m56s
note: sources rendered in collapsible UI panel; inline citations may be partial
---

# Grok Deep Research — DR26-07-14-FLEE-01

A. Executive Summary

A fully unattended "Deep Research fan-out" — submitting one prompt to the consumer Deep Research modes of ChatGPT, Gemini, and Grok, then reliably collecting the finished reports on a schedule with no live human browser session — is technically feasible today but fragile, high-maintenance, and carries significant policy and reliability risks. The browser automation layer (Firefox + Playwright launch_persistent_context with dedicated per-vendor profiles) is the only viable path for consumer subscriptions, as CLI/API paths do not expose equivalent Deep Research features and xAI explicitly prohibits automated access.

Gemini offers the strongest official path via the Interactions API / Deep Research Agent (background async tasks, collaborative planning, polling). ChatGPT relies on brittle browser UI + partial backend extraction. Grok faces the highest barriers due to explicit AUP prohibitions on "automated or non-human means."

Core challenges include UI drift (selectors break frequently), quota waste on failed/partial runs, long async waits (10–60+ min), extraction robustness (iframes, dynamic widgets, sandboxing), and account risk (real but mitigable with residential static IPs, low cadence, and human-like patterns). Emerging 2025–2026 trends — official agent APIs, Computer Use models (screenshot + action loops), MCP servers, and LLM-driven abstractions like browser-use/Stagehand — are gradually reducing pure-selector fragility but have not yet retired the browser hack for all three vendors' consumer Deep Research features.

Recommended architecture: Job ledger + state machine (probe → submit → monitor → collect) per vendor, using persistent Firefox profiles, multi-layer fallbacks (role/text locators + LLM vision), canary/probe checks before quota spend, and watchdog timeouts. Prioritize official Gemini API; treat Grok automation as high-risk or avoid. Non-technical owners can maintain it with clear docs, logs, and simple profile refresh steps, but expect ongoing repairs. Overall confidence in a production-grade system: emerging (strong patterns exist; full unattended reliability for all three remains non-trivial).

B. Key Findings (with confidence tiers and recency)

Official surfaces are maturing fastest for Gemini — The Gemini Deep Research Agent (via Interactions API, e.g., deep-research-preview-04-2026 or similar) supports background=True for long-running async tasks, collaborative_planning=True/False for two-phase (plan → approve/execute), and polling via interactions.get(). This is production-ready for unattended orchestration and largely retires browser hacks for Gemini. [established, 2026 Google docs/announcements].⁠Aistudio.google +2
xAI/Grok has the strictest prohibition — The June 26, 2026 AUP explicitly bans "Accessing the Services through automated or non-human means, whether through a bot, script, or otherwise," plus scraping and bot access. Violations risk suspension/termination. Consumer DeepSearch/Deep Research appears browser-only on grok.x.ai or x.com; API offers web_search tools but not equivalent consumer Deep Research. [established from primary AUP].⁠X
Playwright persistent contexts are the settled pattern for logged-in sessions — launch_persistent_context(user_data_dir=profile_path) on Firefox reliably inherits cookies, localStorage, and logins across runs without re-auth every time. Dedicated per-vendor profiles (pre-logged once by a human) + residential static IP is the recommended base. Chromium as CDP fallback only. [established, Playwright docs + multiple implementations].⁠GitHub +1
UI automation is highly fragile and drifts fast — Repos automating ChatGPT (e.g., chatgpt-automation-mcp with explicit chatgpt_enable_deep_research()) were archived/out-of-date within months due to UI changes; Deep Research mode specifically broke. Selectors, toggles (Gemini two-phase), and composer behavior change frequently. [emerging pattern from GitHub histories 2025–2026].⁠GitHub
Extraction has vendor-specific stable vs. fragile surfaces:
ChatGPT: DOM scrape post-completion common; backend /backend-api/conversation/<id> + widget_state/report_message for Deep Research reports possible but requires auth token handling (cookies/session or network interception). Iframe sandboxing blocks some synthetic clicks. [single/low-authority sources for production extraction].⁠GitHub +1
Gemini: Official API preferred; browser export/share/PDF or DOM as fallback.
Grok: Primarily DOM or share/export; entangled with x.com login.
Blob downloads and cross-origin issues are known pain points. [emerging consensus from automation repos].
Account/rate-limit risks are real but volume- and pattern-dependent — Reports of ChatGPT Pro bans (even personal/low-volume) tied to automation-like patterns, IP anomalies, or rapid actions. Google has suspended for high-volume OAuth/integrations. Low-cadence, human-like pacing, persistent residential profiles reduce (but do not eliminate) risk. xAI risk is highest due to explicit policy. [mixed evidence; mostly high-volume or suspicious patterns reported].⁠Arte.itlibra +2
Emerging trends (2025–2026) favor abstractions over raw selectors — Computer Use / agent APIs (OpenAI CUA, Gemini, Anthropic), Playwright MCP servers (standardized browser tools for agents), browser-use (Python LLM+Playwright agent lib with high adoption), Stagehand (TS primitives), and tools like browserops (drive real logged-in Chrome from MCP clients) improve resilience via vision/LLM element finding and self-healing. Dedicated agent browsers and scheduled-task features (e.g., Xplorer) are appearing. These coexist with human browsers via profiles/extensions. [emerging, rapid iteration in 2025–2026].⁠Ztabs +2
Orchestration patterns for long-async jobs exist in open source — Job ledgers, state machines, resume-by-ID, watchdog timeouts, and exponential backoff appear in browser-agent and automation repos. Headless scheduled runs via persistent contexts or cloud browsers (Browserbase, etc.) are demonstrated. No single mature "Deep Research fan-out" repo found; most are general chat or agent frameworks. [emerging from scattered GitHub + blogs].

C. Contradictions & Debates

Official API vs. browser hack: Gemini has a clear official win for unattended use. ChatGPT and Grok lag (consumer Deep Research features remain browser-centric or policy-blocked). Some argue browser hacks will persist for feature parity; others say APIs will catch up and retire them. Evidence favors APIs where available; browser remains necessary elsewhere. What would settle it: Full consumer Deep Research parity in official APIs across vendors.
Risk level of own-account automation: Practitioners report successful low-volume personal use with residential IPs and delays; others cite sudden bans/false positives even on Pro tiers. xAI policy is unambiguous prohibition vs. OpenAI/Google's more enforcement-pattern-based approach. Steelman for lower risk: Personal, low-cadence, static residential IP + persistent profile looks "normal." Steelman for higher risk: Any automation deviates from human patterns and can trigger automated systems. Single-source ban reports are common but often lack full context.
Firefox vs. Chromium stealth: Firefox praised for plaintext cookies and less ABE/DBSC hassle (per context). Chromium has more mature stealth patches (Patchright, SeleniumBase UC). Debate ongoing; choice often pragmatic (Firefox per original decision).
LLM-driven vs. selector-based resilience: Pure selectors break fast. LLM vision/description fallbacks (or libs like browser-use) add robustness but increase tokens/cost/latency and introduce new failure modes (vision hallucinations). Hybrid is emerging best practice.

D. Best Practices

Dedicated per-vendor Firefox profiles via launch_persistent_context; human logs in once per profile. Store in version-controlled or backed-up dirs. Use stealth args, human-like delays/mouse simulation where possible.
State machine per task: queued → probe (UI state, composer ready, mode available) → submit (JS insert or careful typing; verify acceptance) → monitor (completion signals or polling, long timeouts + watchdog) → collect (multi-layer) → store/notify. Idempotent via task IDs or recent-dupe checks.
Canary/probe-before-burn: Verify (a) composer holds full prompt, (b) Deep Research mode engaged (toggle/state), (c) run started — before full quota spend. Abort on anomalies. Test small non-submitting interactions.
UI-drift resilience: Primary = role-based, text, data-testid, ARIA locators. Fallbacks = multiple strategies + LLM vision (screenshot + "find the Deep Research toggle"). Self-healing on failure. Avoid brittle CSS/XPath.
Extraction layers (fallback ladder): Official export/share/PDF → backend JSON (ChatGPT conversation API with token) → DOM scrape (post-completion indicators) → escalate. Handle special cases (ChatGPT widget_state/report_message, iframes).
Orchestration: Job ledger (SQLite/Postgres: task_id, prompt, vendor, status, conversation_id, timestamps, retries, logs). Scheduler (cron/systemd/APScheduler/Celery). Workers launch contexts on-demand. Monitoring/alerts (drift detection, failures). Resume on restart via ledger.
Human-like & anti-bot: Residential static IP (settled), low frequency, randomized delays, occasional keepalive visits. Avoid datacenter/VPN churn.
Error handling: Exponential backoff, browser restart, session restore, screenshots on failure. Comprehensive structured logs.
For non-technical owners: Readable logs, one-command profile refresh (open Firefox with profile dir), dashboard for manual retry/escalate, clear runbooks.

E. Failure Modes

UI drift / selector breakage: Most common; causes partial submits (quota burn) or failed collection. Mitigated by probes + fallbacks but requires ongoing maintenance.
Quota/rate-limit waste: Failed probes, partial submits (e.g., Enter key issues), or malformed runs burn limits without results.
Account flags/suspensions: Especially xAI (policy violation). OpenAI/Google more tolerant of low-volume personal but trigger on patterns (speed, volume, IP anomalies). False positives reported.
Session/browser fragility: Expiry, crashes on long runs, resource exhaustion. Persistent profiles help longevity but not infinitely.
Extraction failures: Sandboxed iframes, dynamic content, widget changes, blob issues.
Anti-bot / Cloudflare-like: Less issue on own logged-in profiles but still possible with obvious automation signals.
Abandoned projects: Many GitHub automation repos (including Deep Research-specific) die from UI changes or lack of maintenance.

F. Strategic Recommendations (ranked by evidence strength + repairability for non-technical owner)

Gemini-primary with official API + browser fallback (strongest evidence, lowest fragility for this vendor; easy unattended polling). [established for Gemini API].
Hybrid browser fan-out using modern abstractions (browser-use or Playwright MCP + persistent profiles + state machine + LLM fallbacks) for ChatGPT and fallback Gemini. Higher repairability with good logging/docs. [emerging but practical].
Minimal or avoided for Grok due to explicit AUP ban; monitor for any official agent surface or use API web_search tools if sufficient. High risk of account loss.
Managed/agent-platform route (if available: browserops-style driving real sessions, or future vendor scheduled exports) for lowest maintenance.
Full custom raw Playwright only as last resort — highest maintenance burden.

Primary recommendation: Build the orchestration layer (ledger + state machine + probes + monitoring) around persistent Firefox profiles. Use official Gemini Interactions API as the primary path for that vendor. Implement multi-layer extraction and canary probes for ChatGPT. Treat Grok as optional/high-risk with explicit policy warning and minimal scope. Add drift-detection alarms and easy profile-refresh procedures. This balances capability, resilience, and owner repairability. Main assumption: Low-to-moderate volume personal/research use on a stable residential IP; not high-frequency commercial automation. Confidence: emerging (patterns solid; end-to-end production reliability for all three requires iteration and monitoring).

G. Action Plan (concrete next steps)

Setup (1–2 days): Create dedicated Firefox profiles per vendor (firefox -profile /path/to/profile). Human-login once each. Test Playwright launch_persistent_context navigation and basic interaction. Backup profiles.
Ledger & Orchestrator (3–5 days): Implement simple job store (e.g., SQLite with task records). Python/FastAPI orchestrator or Celery workers. Scheduler trigger (cron or APScheduler) for fan-out on prompt.
Per-vendor executors (core dev, 1–2 weeks):
State machine with probe functions (UI readiness, mode state).
Submit logic (JS insertText or careful typing + verification; handle Gemini plan/approve).
Monitor loop (completion signals, long timeout + watchdog).
Collect with layered extraction.
Resilience layer: Implement locator fallbacks + optional LLM vision helper. Canary checks. Error recovery + alerts (email/Slack/webhook on drift/failure).
Testing & hardening: Canary runs, failure injection (simulated UI change), quota-safe tests. Measure end-to-end latency and success rate.
Ops & docs: Logging/monitoring dashboard. Runbooks: "How to refresh a profile," "How to resume a stuck task," "Escalation ladder." Version profiles and configs.
Monitoring & iteration: Track UI change frequency, success rates, account health. Subscribe to vendor changelogs or use canary probes as early warning. Plan quarterly reviews.
Policy/governance: Document xAI risk explicitly. Start with Gemini + ChatGPT; add Grok only if risk appetite allows and with extra safeguards.

H. Sources (selected; full retrievals via tools; dates as published or effective)

xAI Acceptable Use Policy (effective June 26, 2026): https://x.ai/legal/acceptable-use-policy [primary policy text].
Gemini Deep Research / Interactions API docs and announcements (2026): Google AI for Developers pages, e.g., https://ai.google.dev/gemini-api/docs/deep-research and related (collaborative planning, background tasks).
Playwright launchPersistentContext docs: https://github.com/microsoft/playwright (and API references).
chatgpt-automation-mcp GitHub (archived Apr 27, 2026, notes on Deep Research breakage): https://github.com/cbusillo/chatgpt-automation-mcp.
Various GitHub automation repos and blogs on persistent contexts, extraction, and browser agents (2025–2026).
X posts (practitioner/verified accounts, 2025–2026): e.g., @GoogleAIStudio on Gemini Deep Research API (Apr 2026); @_philschmid on Gemini Computer Use + browser agents; general automation discussions (limited direct "fan-out" hits; more on Gemini agents and general browser automation). Anonymous/low-engagement posts noted as such where relevant; few corroborated independent deep dives on exact multi-vendor unattended Deep Research fan-out.
Browser automation trend posts (e.g., ZTABS May 2026 on AI browser automation landscape; browser-use/Stagehand comparisons 2026).
Account risk reports and policy discussions (various 2025–2026 community and news sources; patterns observed but causation often multi-factor).

All claims tied to retrieved sources; no invented URLs/dates. Single-source or low-authority claims flagged inline where applicable. X insights mostly general agent/browser automation rather than exact topic; corroborated where possible with web sources.