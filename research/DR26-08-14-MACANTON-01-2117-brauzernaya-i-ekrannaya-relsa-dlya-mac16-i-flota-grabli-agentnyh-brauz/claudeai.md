# DR26-08-14-MACANTON-01-2117 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-14-MACANTON-01-2117
vendor: Anthropic claude.ai (web, Fable 5)
door: browser-chrome-mcp
rail: claudeai
mode: completion API + web_search_v0 (9 поисков) — класс «умная модель + веб-поиск», НЕ вендорский Deep Research

chat_url: https://приватный чат

collected: 2026-08-15T04:36:01Z
chars: 51945
stop_reason: end_turn
---

# Предварительные результаты: что удалось подтвердить сразу

Ниже — прямые ответы на ваши шесть открытых вопросов по свежим первоисточникам (issue-трекеры, вендорская документация), плюс сжатая таблица граблей и матрица по ОС. Это не полное 25-строчное исследование — это верифицированное ядро.

## Ответы на открытые вопросы

**1. CDP heartbeat + автоматический re-bind targetId — подтверждено: паттерна нет, и это признано в трекере.** [established] Ваш «UNVERIFIED»-флаг теперь верифицирован в вашу пользу. В chrome-devtools-mcp issue #978 (фев 2026) прямым текстом: «The WebSocket transport has no heartbeat/keepalive mechanism. When Chrome becomes unresponsive, the connection silently dies. In browser.js, the code only checks connected state but provides no reconnection logic» — при этом 6+ вкладок роняют соединение с ошибкой MCP error -32000: Connection closed, поскольку каждая страница создаёт отдельную CDP-сессию через Target.attachToTarget и поток событий перегружает единственный WebSocket-канал (https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978). Issue #1094 (март 2026, macOS, `--autoConnect`) фиксирует деградацию долгих сессий и предлагает именно то, что вы ищете: персистить один browser-websocket на весь lifecycle сервера вместо частых reconnect/open-close, добавить явную reconnect state machine с backoff и health-чеками, и предпочитать endpoint из файла DevToolsActivePort статичному browserUrl (https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1094). Issue #1152 (март 2026) показывает, что даже фиксированный `--wsEndpoint` из Codex не спасает: tool-вызовы падают с Transport closed / timed out awaiting tools/call, хотя целевой DevTools-websocket доступен и list_pages резолвится в логе сервера (https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1152). **Практический вывод:** re-bind надо строить самим — сторожевой процесс, который читает `~/.../Chrome/DevToolsActivePort` (порт + GUID browser-endpoint меняются при каждом рестарте Chrome), сверяет с текущим ws, и при расхождении убивает/перезапускает MCP-сервер. Штатного решения нет ни у Google, ни у Anthropic/OpenAI на текущий момент.

**2. Полураспад залогиненной сессии по рельсам.** [emerging] Строгой эмпирики никто не публикует; лучшие доступные числа — от remote-browser-сервисов: Steel документирует, что профили сохраняют аутентификацию до 30 дней (https://llms.steel.dev/articles/browser-infrastructure-for-ai-agents-compared/), и рекомендует связку один профиль + один выделенный IP на аккаунт, чтобы сайт видел те же cookies, storage и знакомый IP, а не свежий браузер из новой сети при каждом запуске (https://docs.steel.dev/overview/profiles-api/overview). Browserbase решает то же через Contexts: логин через Session Live View вручную, затем context ID персистит auth-состояние в последующих сессиях, снижая проблемы истечения сессий (https://docs.browserbase.com/platform/identity/authentication). Полураспад для локальных рельс (extension vs CDP vs profile) по площадкам Google/X/FB — данных уровня «измерено» нет, только анекдоты; это главный кандидат на глубокий проход по трекерам.

**3. Wayland для unattended — «условное да», но только на wlroots-композиторах.** [established] Ввод: ydotool работает через uinput-фреймворк ядра, эмулируя устройство ввода, а не X-события — поэтому работает на X11, Wayland и даже консолиThis is a large research task. Let me continue gathering primary evidence across the key axes.Excellent primary evidence. Let me continue on Codex issues, session durability half-life, WebArena/OSWorld benchmarks, and Wayland portal progress.Strong Codex evidence. Now let me get benchmark infrastructure flakiness (WebArena/OSWorld/WebVoyager) and session half-life data.Good academic grounding. Now let me research session half-life on specific sites, navigator.webdriver / bot detection, and Wayland portal / RemoteDesktop viability.Let me get harder data on Wayland RemoteDesktop portal viability for unattended agents and the "second programmatic download" Chrome behavior, plus SingletonLock profile lock.I have strong coverage. Let me get a few more critical pieces: Wayland RemoteDesktop portal for unattended/headless, agentic browsers (Comet/Atlas) session durability, and the "second download" Chrome behavior + Windows secure desktop black screenshot.I have enough to cover Windows secure-desktop/RDP. Now let me research agentic browsers (Comet/Atlas) session durability and the Wayland RemoteDesktop portal + a couple more targeted searches for session half-life and the "second download" behavior.Note: Atlas retired July 9 2026 (stops working Aug 9 2026) — critical for the report. Let me do a few final targeted searches: Wayland RemoteDesktop portal headless viability, Chrome second-download killing, and Xvfb/container detection.I now have comprehensive coverage. Let me do two final targeted searches to nail down the "CDP heartbeat/re-bind" open question and Chrome's stance on CDP restrictions, then write the report.I now have the critical Chrome 136 + Chrome 144 policy findings. Let me do one final search on session half-life measurements and the "second download" Chrome behavior, then write.I have enough evidence to write a comprehensive report. I have strong primary-source coverage across all three axes and every OS. Let me compose the final report.

---

# Rakes When Coding Agents Drive an External Browser or the Screen: A Cross-OS Field Guide (2025–2026)

## A. Executive Summary

**Bottom line up front:** There is no rail that survives unattended overnight runs on ban-sensitive consumer sites (Google/X/Facebook) without occasional human re-auth on *any* OS. What you *can* build is a rail that survives most nights and fails **loudly and recoverably**. The winning shape, confirmed by the bug trackers, is: a **persistent dedicated Chrome-for-Testing profile driven over CDP** for automation-tolerant sites, a **Firefox persistent profile** for headless cookie/fetch work, and a **watchdog that treats any 2FA/CAPTCHA/`webdriver`-flag event as a hard stop, not a retry**. Your Aug-2026 decision is directionally correct and matches what practitioners converged on — with three important corrections below.

**The five things that most change your plan:**

1. **Chrome 136 (Mar 2025) killed `--remote-debugging-port` on the *default* user-data-dir.** Flag-launching CDP against your real signed-in Default profile is now inert; only a **dedicated `--user-data-dir`** or the `chrome://inspect` toggle works. This is the single most important mechanical fact for your fleet. [established] ([developer.chrome.com, 2025-03-17](https://developer.chrome.com/blog/remote-debugging-port))

2. **Chrome 144+ shows a per-connection permission dialog for CDP/`--autoConnect`**, with no persist option as of early 2026 — directly breaking unattended re-binding. [established] ([chrome-devtools-mcp #825, 2026-01-24](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/825))

3. **Your UNVERIFIED flag is CONFIRMED: nobody ships a production CDP heartbeat with automatic targetId re-binding.** The chrome-devtools-mcp maintainers' own users are *requesting* it as an unbuilt feature (issues #978, #1094, #1152). You will have to build the watchdog yourself. [established]

4. **App-Bound Encryption (Chrome 127, Jul 2024) permanently broke external cookie copying on Windows/macOS** — your Firefox-first choice for cookie extraction is the correct and essentially only durable answer. [established] ([yt-dlp.net](https://yt-dlp.net/errors/could-not-copy-chrome-cookie-database), [dev.to, 2026-03-02](https://dev.to/osovsky/6-ways-to-get-youtube-cookies-for-yt-dlp-in-2026-only-1-works-2cnb))

5. **Wayland unattended screen+input is now *technically* viable via libei + RemoteDesktop portal with session persistence (mid-2023+), but is still a minefield in shipped binaries** (feature-gated builds, silent input no-ops). For unattended agents, **X11-under-Xvfb or a nested compositor remains the pragmatic choice.** [emerging]

**Per-OS one-liner:** For unattended overnight work, **Linux headless (Xvfb/container) is the most controllable**, **Windows is the most fragile for screen work (secure desktop, RDP black screen)** but fine for background CDP, and **macOS is the worst for unattended screen work** because of monthly TCC re-prompts and the signed-out-headless trap you already hit.

---

## B. Key Findings

### B1. The browser rail breaks in a small number of highly reproducible ways

The chrome-devtools-mcp tracker is the richest primary source, and it tells a consistent story: **the transport has no heartbeat, no reconnect state machine, and churns targets.**

- Opening 6+ tabs causes the WebSocket to drop with `MCP error -32000: Connection closed`; each page creates a separate CDP session via `Target.attachToTarget`, the event volume overwhelms the single WebSocket, there is no heartbeat/keepalive, and `browser.js` only checks connected state with no reconnection logic. [established] ([chrome-devtools-mcp #978, 2026-02-17](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978))
- In long-running sessions with `--autoConnect`, tool calls fail with connection/transport errors even while Chrome remote debugging is active; the fix requested is to "persist/reuse one browser websocket," "add an explicit reconnect state machine with backoff and health checks," and "prefer DevToolsActivePort endpoint over static browserURL" — i.e., the reconnect logic **does not exist yet.** [established] ([#1094, 2026-03-04](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1094))
- From Codex specifically: with a fixed `--wsEndpoint`, tool calls intermittently fail with `Transport closed` / `timed out awaiting tools/call after 120s` even though the target WebSocket is reachable — a reconnection/session-lifecycle issue under long-lived usage. [established] ([#1152, 2026-03-09](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1152))

**This directly confirms your UNVERIFIED item.** The CDP `Target.detached` / `Inspector.detached` events exist ([chromedevtools.github.io](https://chromedevtools.github.io/devtools-protocol/)), and `404: No such target id` is the documented stale-ref error, but **the auto-rebind loop on top of them is left to the integrator.** Your snapshot→act→re-snapshot→one-retry→hard-stop loop is the correct hand-built substitute.

### B2. The "two Chromes / cannot attach" class is now structural, not incidental

Your measured pain (in-app browser pane bypassing a "two Chromes" conflict) is the general case:

- Playwright MCP and Claude-in-Chrome are "mutually exclusive on Windows" because Claude-in-Chrome requires Chrome to be running while Playwright MCP requires Chrome to not be running. [established] ([claude-code #24144, 2026-02-08](https://github.com/anthropics/claude-code/issues/24144))
- The Claude-in-Chrome extension path and the CDP path **cannot be satisfied simultaneously**: when using the original Chrome profile, port 9222 cannot be opened reliably; when using a fresh profile where 9222 opens, the browser-side extension is absent — these two requirements cannot currently be satisfied simultaneously through user-facing configuration. [established] ([claude-code #48806, 2026-04-15](https://github.com/anthropics/claude-code/issues/48806))
- Users explicitly note the CDP workaround defeats the purpose: "chrome-devtools MCP launches an isolated, unauthenticated Chrome profile, so they can't reach sites that require my existing logged-in session — which is the whole point". [established] ([claude-code #79445, ~2026-07](https://github.com/anthropics/claude-code/issues/79445))

**Root cause:** Chrome's Singleton model. Chrome enforces a Singleton model per user-data-dir; only one process can use a directory at a time, and with `--headless=new` you cannot reuse the same `--user-data-dir` across concurrent instances — parallel browsers must use separate directories. [established] This is *also* the mechanism behind your "three simultaneous Chromes lost the Grok session" pain: concurrent access to one profile dir corrupts/races the cookie and session state.

### B3. Chrome has actively tightened CDP twice in 18 months — plan for more

- **Chrome 136:** "Starting with Chrome 136, the `--remote-debugging-port` and `--remote-debugging-pipe` flags are no longer supported when used with the default Chrome data directory"; the sanctioned route on a real profile is now the `chrome://inspect` toggle. [established] ([developer.chrome.com, 2025-03-17](https://developer.chrome.com/blog/remote-debugging-port); [dev.to, 2026-03-19](https://dev.to/timtech4u/your-browser-has-a-remote-control-and-nobody-told-you-5e97))
- Google's official guidance: "For browser automation scenarios, we recommend using Chrome for Testing which will continue to respect the existing behavior." [established] ([Chrome for Developers blog, 2025-03-17](https://developer.chrome.com/blog/remote-debugging-port))
- **Chrome 144:** per-connection CDP approval dialog with no persist option (B-above). [established]
- A subtlety your watchdog must handle: a **toggle-enabled** browser publishes a WebSocket endpoint but **404s `/json/version`** — the JSON discovery API you'd normally probe for liveness. On Chrome ≥136 the port flag is inert on the default profile dir; a toggle-enabled browser 404s `/json/version` and you must prove liveness via the `DevToolsActivePort` file instead. [emerging, single-source but technically detailed] ([trycua/cua #2916, ~2026-08](https://github.com/trycua/cua/issues/2916))
- **Enterprise kill switch:** `RemoteDebuggingAllowed=Disabled` fully blocks CDP; on managed Win11 this means Playwright/Codex/Claude CDP simply won't start. A corporate security policy blocking all devtools remote debugging (including `--remote-debugging-pipe`) makes Playwright unable to launch Chrome for a 10,000+ user org. [established] ([playwright #39637, 2026-03-12](https://github.com/microsoft/playwright/issues/39637); [Chrome Enterprise policy](https://chromeenterprise.google/policies/remote-debugging-allowed/))

**Implication:** Your Chrome rail should be **Chrome for Testing**, not stable Chrome, wherever possible — it is the only build Google commits to keeping CDP-open, and it sidesteps the auto-update-breaks-the-driver problem.

### B4. Screen control is an OS-permissions story, not an agent story

- **macOS:** Screen Recording is gated by TCC and, since Sequoia 15, **re-prompts monthly**. Starting with Sequoia, macOS prompts to re-confirm capture permission for each app every 30 days; if the app's code signature changes between launches (common with auto-updating apps) the permission breaks; TCC databases are per-user. [established] ([screenify.studio, 2026-04-23](https://www.screenify.studio/blog/2026-04-23-macos-screen-recording-permissions); [9to5mac, 2024-08-14](https://9to5mac.com/2024/08/14/macos-sequoia-screen-recording-prompt-monthly/)) The **code-signature-change → permission reset** point is exactly why an auto-updating Claude Code / Codex desktop app loses screen access after an update. There is an undocumented plist workaround (setting a far-future date in `ScreenCaptureApprovals.plist`) but it requires Full Disk Access and is fragile. [low-authority] ([Jamf community, 2024](https://community.jamf.com/t5/jamf-pro/monthly-tccc-prompts-macos-v15/m-p/339150))
- **Windows:** UAC's **secure desktop is a separate desktop that screenshots capture as black**, and **RDP disconnect/lock produces black screenshots** because the session has no console. The secure desktop displays the UAC dialog only on the local desktop; a remote user receives a black screen ([learn.microsoft.com](https://learn.microsoft.com/fr-fr/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee844127(v=ws.10))). Blank screenshots on RDP indicate a locked session; once you disconnect RDP the session locks and screenshots go blank ([Ranorex forum](https://www.ranorex.info/problems-with-remote-desktop-t13094.html)). The known fix is `tscon` to redirect the session to console: running `for /f "skip=1 tokens=3" %%s in ('query user %USERNAME%') do (tscon.exe %%s /dest:console)` converts a terminal session into a console session that keeps full graphical context after RDP disconnect. [established] ([learn.microsoft.com Q&A, 2024-03-01](https://learn.microsoft.com/en-us/answers/questions/1605189/keep-gui-activated-after-rdp-disconnected))
- **Linux X11:** works — `xdotool` (XTEST) for input, `scrot`/`grim` for capture; the "just works" baseline for unattended agents (via Xvfb on a headless box).
- **Linux Wayland:** blocked by design; requires portals. See B5.

### B5. Wayland unattended input is *finally* possible but not yet reliable in shipped tooling

The mechanism exists: libei is part of the XDG RemoteDesktop portal (since v1.17, mid-2023) as the transport for emulated input; more recently session persistence support was added so you don't have to allow access on every connection. [established] ([who-t blog, 2026-07](http://who-t.blogspot.com/2026/07/libei-integrations-in-xdg-remotedesktop.html)) And GNOME 47+ supports genuinely unattended headless access via GDM remote login. GNOME supports remote login via RDP where the user authenticates at the greeter then starts a Wayland session — useful for lab machines and servers without GPUs. [emerging] ([Stackademic, 2025-11-18](https://stackademic.com/blog/remote-desktop-on-wayland-in-2025-what-changed-for-linux-support-engineers))

But the *shipped agent tooling* is where it falls apart: On KDE Plasma 6.7 Wayland, the trycua cua-driver renders the agent cursor but input silently no-ops because `wlr_virtual_pointer` isn't advertised and the `portal-libei` modules are feature-gated out of the published binary — `doctor` reports all-green even though input is dead. [emerging, single-source but detailed root cause] ([trycua/cua #1982, 2026-06-23](https://github.com/trycua/cua/issues/1982)) The tooling that *does* work (kwin-mcp, wdotool) is compositor-specific and new. kwin-mcp injects input via KWin's private `org.kde.KWin.EIS.RemoteDesktop` D-Bus interface, which returns a libei fd for low-level emulation without the XDG portal authorization dialog — but that is KDE-only. [emerging] ([isac322/kwin-mcp, 2026-05-30](https://github.com/isac322/kwin-mcp)) And Wayland's security model has **no target-a-window API for input**: wdotool injects into whichever window has focus; Wayland's security model has no "target a specific window" API for input — focus the target first, then run. [emerging] ([cushycush/wdotool](https://github.com/cushycush/wdotool))

**Verdict on your Open Question 3:** Wayland is a "soft yes, hard operations." For an unattended fleet, prefer **X11-under-Xvfb** or a **nested compositor** over betting on portal-libei in shipped agent drivers today.

### B6. Session durability: no rail gives you weeks on ban-sensitive sites; the encrypted-store question has a definite answer

There is **no published rigorous half-life table** for Google/X/Facebook under automation (your Open Question 2 has no clean empirical source — this is a genuine gap). What the sources *do* establish:

- Remote-browser services claim **profiles persist auth "up to 30 days"** (Steel) — but that is a *storage* lifetime, not a *site-accepted* lifetime; sites can invalidate sooner via device-binding. Steel Profiles persist auth up to 30 days. [low-authority/marketing] ([Steel, 2026-04-01](https://llms.steel.dev/articles/browser-infrastructure-for-ai-agents-compared/))
- `storageState` JSON rots because **session cookies and short-lived tokens expire**: session data in storageState becomes stale or expired, especially with session cookies or tokens that have limited lifetimes; the solution is to regularly regenerate the state file. [established] ([BrowserStack, 2025-12-23](https://www.browserstack.com/guide/playwright-storage-state))
- **On the OS-encryption-vs-fingerprint question (your Open Question 5):** the fewer-re-logins effect is **primarily a fingerprint/persistence effect, not a cookie-encryption effect.** Cookie encryption (Keychain/DPAPI/gnome-keyring) governs whether *other processes* can read the store; it does not change how long the *site* honors the session. What sites key on is `navigator.webdriver`, CDP fingerprints, and device binding. So "which OS gives fewest forced re-logins for the same sites" is answered by *which OS lets you present the most consistent, least-automated-looking profile+IP*, not by which keystore you use. [emerging — reasoned from mechanism; no head-to-head OS study found]

### B7. Agentic browsers (Comet/Atlas/Dia/Neon) move the problem, don't solve it

- **Atlas is being retired.** OpenAI announced on July 9, 2026 it is retiring the standalone ChatGPT Atlas browser nine months after its October 2025 launch; Atlas stops working on August 9, 2026, with features moving into the ChatGPT desktop app and a Chrome extension. [established] ([nohacks.co, ~2026-08](https://nohacks.co/blog/agentic-browser-landscape-2026)) Betting your rail on Atlas is now a dead end.
- Their durability "solution" is **holding your live credentials on every site**, which is precisely the attack surface security researchers say is unfixable: the same property that makes agentic browsers useful — access to your email, banking sessions, and login credentials on every site by design — is why no agentic browser can be fully secured; OpenAI wrote in Dec 2025 that prompt injection is "unlikely to ever be fully solved". [established] ([techtimes.com, 2026-06-17](https://www.techtimes.com/articles/318528/20260616/ai-browser-comparison-2026-atlas-vs-comet-vs-dia-ranked-security-use-case.htm)) One report flags Atlas as vulnerable to **session persistence even after logout** — a durability "feature" that is actually a security bug. [low-authority] ([sophiesbureau.com, 2025-11-03](https://sophiesbureau.com/digital-ops/ai-prompt-injection-browser-security))

**Verdict on Open Question 6:** They do not solve durability; they relocate it into a product you don't control and can't watchdog, and they raise your detection/injection risk.

---

## C. Contradictions & Debates

1. **"Use the real signed-in profile" vs. "use a dedicated/throwaway profile."** The durability crowd wants the real profile (keeps you logged in); the stability crowd (and Chrome 136) forces a dedicated `--user-data-dir`. **Resolution:** a *dedicated but persistent, pre-logged-in* profile is the synthesis — not throwaway, not your daily-driver Default. Both camps are right about their half; the trap is using the Default dir.

2. **Extension rail vs. CDP rail.** Anthropic/OpenAI ship extension rails (Claude-in-Chrome, Codex Chrome plugin) that keep your session but are flaky and conflict with CDP (§B2). The CDP rail is stable-ish but unauthenticated by default. The trackers show the extension rail failing constantly ("Browser extension is not connected," dozens of issues). **Your demotion of the vendor extension to fallback is well-supported by the evidence.** The debate is unresolved by the vendors themselves.

3. **Is Wayland ready?** GNOME/KDE upstream say yes (portal-libei, persistence, GDM headless login). Shipped agent drivers say no (feature-gated, silent no-ops). Both are honest; they're describing different layers. Steelman for "ready": on a *correctly built* stack (GNOME 47 + grdctl + libei-enabled driver) unattended works today. Steelman for "not ready": you cannot currently `pip install` an agent driver and trust it on arbitrary Wayland without verifying the libei path is compiled in and advertised.

4. **Container/VM: better or worse for detection (your Open Question 4)?** No clean study exists. Mechanistically: a container **does not change the fingerprint sites see** (same Chrome build, same `navigator.webdriver`, same TLS) unless you also change IP and fingerprint — so containerization is **detection-neutral by itself**. It *does* help stability (clean profile dir, no human touching the same browser, kill/respawn). The remote-browser services' pitch (Steel/Browserbase) is that they add **fingerprint + dedicated IP** on top of the container, which is the part that actually moves detection. [emerging — reasoned; the one concrete container gotcha found: since Chromium M113, `--remote-debugging-address=0.0.0.0` is forcibly remapped to 127.0.0.1, blocking external CDP from a Docker container; workaround is `socat` forwarding 9222→127.0.0.1:9223 ([ytyng.com, 2026-04-20](https://www.ytyng.com/en/blog/docker-chromium-cdp-port))]

---

## D. Best Practices

1. **Use Chrome for Testing, not stable Chrome, for the CDP rail.** It's the only build Google guarantees keeps CDP open, and it decouples you from auto-update breaking the driver. ([developer.chrome.com](https://developer.chrome.com/blog/remote-debugging-port))
2. **Dedicated persistent `--user-data-dir`, pre-logged-in, never your Default profile.** Satisfies Chrome 136 and avoids Singleton races.
3. **One profile ↔ one process ↔ one IP.** Never point three Chromes at one profile dir (your Grok loss). For parallelism, one dedicated dir per worker. ([HuggingFace/PinchTab docs](https://huggingface.co/spaces/AUXteam/WitNote/blob/main/docs/implementations/chrome-files.md))
4. **Prefer `--remote-debugging-pipe` over `:9222` where the client supports it** — it survives sandboxes and avoids the exposed-port and M113 bind-remap problems. Pipes work in sandboxed environments (gVisor, Firecracker) that block runtime TCP bind, because they're inherited file descriptors; no exposed port even on localhost. [established] ([chromedp #1607, 2025-12-23](https://github.com/chromedp/chromedp/issues/1607))
5. **Firefox persistent profile for headless fetch/cookie work.** Firefox stores cookies in a SQLite file other processes may read and does not tie encryption to an OS keystore the way Chrome does — the ABE problem simply doesn't apply. [established] ([yt-dlp.net](https://yt-dlp.net/errors/could-not-copy-chrome-cookie-database)) Your AutoFF decision is correct.
6. **Build the watchdog you assumed nobody built** (they didn't): probe liveness via `DevToolsActivePort` file (not just `/json/version`, which 404s on toggle-enabled browsers), detect `Target.detached`/`404 No such target`, do exactly one re-snapshot+rebind, then hard-stop on 2FA/CAPTCHA/`webdriver`-flag. This is the reconnect state machine the mcp maintainers' users keep asking for.
7. **Treat any auth-challenge as a stop condition, never a retry.** Retrying into Google's "this browser or app may not be secure" or a Turnstile challenge is how you get device-binding 2FA storms and lockouts.
8. **Regenerate `storageState`/profile on a schedule** (e.g., nightly refresh from a known-good login), because session cookies rot. ([BrowserStack](https://www.browserstack.com/guide/playwright-storage-state))
9. **macOS: pin the app path, grant Screen Recording once, and expect a monthly re-grant.** If the desktop app auto-updates its signature, budget for re-granting. Do not rely on headless CLI staying logged in on Mac (your empty-run trap). ([screenify.studio](https://www.screenify.studio/blog/2026-04-23-macos-screen-recording-permissions))
10. **Windows unattended: use `tscon` to keep a console session alive after RDP disconnect**, or run browser work headless/CDP-only (no screen rail). ([learn.microsoft.com](https://learn.microsoft.com/en-us/answers/questions/1605189/keep-gui-activated-after-rdp-disconnected))
11. **Second-download gotcha:** Chrome throttles/blocks a rapid second programmatic download in the same tab (your measured pain — [no clean upstream source found]; corroborated only by your own observation, mark **[unverified]**). Mitigation: one download per fresh tab/context, or use `Browser.setDownloadBehavior` per-context and await completion before the next.

---

## E. Failure Modes — The Rake Table

Ordered roughly by how often practitioners report them. Error text is quoted verbatim where the source gives it.

| # | Rake | Trigger | OS(es) | Symptom (exact text if known) | Root cause | Reliable fix / mitigation | Source + date |
|---|------|---------|--------|-------------------------------|------------|---------------------------|---------------|
| 1 | Vendor browser extension "not connected" | Claude-in-Chrome / Codex plugin from CLI | mac, Win | `Browser extension is not connected. Please ensure the Claude browser extension is installed and running` | Native-messaging bridge / profile / account-mismatch race; extension only lives in one profile | Restart Chrome+CLI; align accounts; fall back to CDP rail | [claude-code #24593 2026-02-10](https://github.com/anthropics/claude-code/issues/24593), [#40758 2026-03-30](https://github.com/anthropics/claude-code/issues/40758), [#79445 ~2026-07](https://github.com/anthropics/claude-code/issues/79445) |
| 2 | CDP transport dies in long sessions | Hours-long agent run, `--autoConnect`/`--wsEndpoint` | mac, Win, Linux | `MCP error -32000: Connection closed`; `Transport closed`; `timed out awaiting tools/call after 120s` | No heartbeat/keepalive; no reconnect state machine | Self-built watchdog: health-check + backoff reconnect; prefer `DevToolsActivePort` | [cdp-mcp #978 2026-02-17](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978), [#1094 2026-03-04](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1094), [#1152 2026-03-09](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1152) |
| 3 | Port 9222 won't bind on real profile | `--remote-debugging-port` on Default user-data-dir | all (Chrome ≥136) | `localhost:9222 → ERR_CONNECTION_REFUSED`; `DevTools remote debugging requires a non-default data directory` | Chrome 136 disabled the flag on default dir | Dedicated `--user-data-dir` or `chrome://inspect` toggle; use Chrome for Testing | [developer.chrome.com 2025-03-17](https://developer.chrome.com/blog/remote-debugging-port), [selenium #16274](https://github.com/SeleniumHQ/selenium/issues/16274), [claude-code #48806 2026-04-15](https://github.com/anthropics/claude-code/issues/48806) |
| 4 | Two Chromes mutually exclusive | Playwright/CDP + vendor extension both want Chrome | Win (worst), all | Playwright fails to launch when Chrome already running | Chrome Singleton per user-data-dir | Separate user-data-dirs; Chromium/CfT alongside; CDP-attach instead of launch | [claude-code #24144 2026-02-08](https://github.com/anthropics/claude-code/issues/24144), [PinchTab docs](https://huggingface.co/spaces/AUXteam/WitNote/blob/main/docs/implementations/chrome-files.md) |
| 5 | Concurrent profile use corrupts session | 3 Chromes → same profile dir | all | Silent logout / session loss (your Grok case) | Singleton race on cookie/session files | One process per profile dir; dedicated dirs per worker | [kb.wisc.edu 2025](https://kb.wisc.edu/cae/148461), [PinchTab docs] |
| 6 | Cookie extraction fails (ABE) | Copy Chrome cookies externally, Chrome ≥127 | Win, mac | `Could not copy Chrome cookie database`; `decrypt_encrypted_value failed`; `failed to decrypt with DPAPI` | App-Bound Encryption ties key to Chrome binary | Use Firefox for extraction; or drive live tab, never copy | [yt-dlp.net](https://yt-dlp.net/errors/could-not-copy-chrome-cookie-database), [yt-dlp #10927 2024-09-05](https://github.com/yt-dlp/yt-dlp/issues/10927), [dev.to 2026-03-02](https://dev.to/osovsky/6-ways-to-get-youtube-cookies-for-yt-dlp-in-2026-only-1-works-2cnb) |
| 7 | macOS screen access silently absent | First screen action w/o TCC grant | mac | Screenshots black/empty; background runs return empty | TCC Screen Recording not granted | Explicit grant; re-grant after every app signature change | your obs; [screenify.studio 2026-04-23](https://www.screenify.studio/blog/2026-04-23-macos-screen-recording-permissions) |
| 8 | macOS monthly TCC re-prompt | 30 days elapse; app auto-updates | mac (Sequoia+) | `[App] is requesting to bypass the system private window picker…` | Sequoia re-auth policy; sig change resets | Plist far-future date (fragile); pin app; human re-grant | [9to5mac 2024-08-14](https://9to5mac.com/2024/08/14/macos-sequoia-screen-recording-prompt-monthly/), [Jamf 2024](https://community.jamf.com/t5/jamf-pro/monthly-tccc-prompts-macos-v15/m-p/339150) |
| 9 | RDP-disconnect black screenshots | Close/lock RDP to Win box | Win | Blank/black screenshots; screen rail returns nothing | Session locks; no console on disconnect | `tscon … /dest:console`; disable auto-lock; console-session VM | [Ranorex](https://www.ranorex.info/problems-with-remote-desktop-t13094.html), [MS Q&A 2024-03-01](https://learn.microsoft.com/en-us/answers/questions/1605189/keep-gui-activated-after-rdp-disconnected) |
| 10 | UAC secure-desktop black frame | Any elevation prompt during capture | Win | Black screen at UAC prompt | Secure desktop is a separate desktop | Avoid elevation in agent flows; policy-disable secure desktop (weakens security) | [MS docs](https://learn.microsoft.com/fr-fr/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee844127(v=ws.10)), [MS Q&A 2025-01](https://learn.microsoft.com/en-us/answers/questions/2151968/quick-assist-admin-prompt-doesnt-appear-from-assis) |
| 11 | Wayland input silently no-ops | Agent driver on Wayland, libei not compiled | Linux (Wayland) | Cursor moves, click never lands; `doctor` all-green | `portal-libei` feature-gated out; `wlr_virtual_pointer` not advertised | Verify libei path; use X11/Xvfb or KDE `KWin.EIS`/nested compositor | [trycua/cua #1982 2026-06-23](https://github.com/trycua/cua/issues/1982) |
| 12 | 6+ tabs drop WebSocket | Multi-page agent run | all | `MCP error -32000: Connection closed` | Event volume on single WS, no backpressure | Cap concurrent tabs; one context per task | [cdp-mcp #978 2026-02-17](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978) |
| 13 | Chrome 144 per-connection CDP prompt | Reconnect under `--autoConnect`, Chrome 144+ | all | Permission dialog every connection | New Chrome security gate, no persist | Chrome for Testing; keep one long-lived WS (don't churn) | [cdp-mcp #825 2026-01-24](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/825) |
| 14 | Google "browser may not be secure" | Automated Google login | all | `This browser or app may not be secure` | `navigator.webdriver`/automation fingerprint | Never automate Google login; pre-auth manually; hard-stop | [Google support thread](https://support.google.com/chrome/thread/224353947), [W3C webdriver minutes](https://www.w3.org/2021/07/14-webdriver-minutes.html) |
| 15 | Enterprise policy blocks CDP | Managed Win11 with RemoteDebuggingAllowed=Disabled | Win (managed) | Chrome won't launch under Playwright | Group policy blocks all remote debugging incl. pipe | Use unmanaged host/VM; or Edge until policy extends | [playwright #39637 2026-03-12](https://github.com/microsoft/playwright/issues/39637), [Chrome policy](https://chromeenterprise.google/policies/remote-debugging-allowed/) |
| 16 | Docker CDP unreachable from host | `--remote-debugging-address=0.0.0.0` in container | Linux (Docker) | External CDP refused | M113+ remaps 0.0.0.0→127.0.0.1 | `socat` forward 9222→127.0.0.1:9223 | [ytyng.com 2026-04-20](https://www.ytyng.com/en/blog/docker-chromium-cdp-port) |
| 17 | Linux SingletonLock stale | Chrome crashed, lock left behind | Linux | Chrome won't reopen profile | Stale `SingletonLock`/`SingletonSocket`/`SingletonCookie` | `rm -rf ~/.config/google-chrome/Singleton*` on startup | [kb.wisc.edu 2025](https://kb.wisc.edu/cae/148461) |
| 18 | Codex Chrome connector fragile (Win) | Attach to signed-in Chrome, Codex Win | Win | Tab attach requires workaround scripts; interferes w/ visible mouse | No stable background-browser mode | Await first-class background mode; use dedicated CfT | [codex #25209 2026-05-30](https://github.com/openai/codex/issues/25209), [#22849 2026-05-15](https://github.com/openai/codex/issues/22849) |
| 19 | Codex desktop IPC EPIPE crash | Browser sidebar window closes mid-run | Win | `IpcRouter Socket error errorCode=EPIPE`; `write EOF`; session→disconnected | Unhandled client-disconnect race in Electron main | Disable browser plugins as containment; await fix | [codex #35985 ~2026-08](https://github.com/openai/codex/issues/35985) |
| 20 | MetaMask (any extension) hangs MCP | Extension present in driven Chrome | all | All tools time out `MCP error -32001`; silent after `Detaching from all non-page targets` | cdp-mcp mishandles non-page target types | Remove/disable extensions in the automation profile | [cdp-mcp #510 2025-11-02](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/510) |
| 21 | Toggle-CDP 404s discovery API | `chrome://inspect` route on real profile | all (≥136) | `/json/version → 404` despite live WS | JSON discovery unavailable on toggle route | Prove liveness via `DevToolsActivePort` file + socket inode | [trycua/cua #2916 ~2026-08](https://github.com/trycua/cua/issues/2916) |
| 22 | storageState rots overnight | Reuse saved auth JSON next day | all | Logged-out mid-run | Session cookies/short tokens expired | Nightly regenerate; prefer refresh-token-backed logins | [BrowserStack 2025-12-23](https://www.browserstack.com/guide/playwright-storage-state) |
| 23 | 127.0.0.1 bridge unreachable from page | Chrome-hosted page fetch to local bridge | all | fetch hangs silently (your case) | Mixed-content/PNA (Private Network Access) or egress proxy | Use extension/CDP channel not page-fetch; or serve bridge same-origin; [partial source] | [codex #23014 2026-05-16](https://github.com/openai/codex/issues/23014) (localhost-URL rejection); your obs **[unverified mechanism]** |
| 24 | WebFetch egress-blocked | Cowork/sandbox network policy | all (sandbox) | `EGRESS_BLOCKED` for common domains | Egress proxy allowlist | Route through allowed domain; adjust network settings | [claude-code #48806 2026-04-15](https://github.com/anthropics/claude-code/issues/48806) |
| 25 | Second programmatic download blocked | Two rapid downloads, same tab | all | 2nd download silently dropped (your case) | Chrome per-tab download throttle | One download per fresh context; `setDownloadBehavior` + await | your obs **[unverified — no clean upstream source found]** |
| 26 | Codex mobile→desktop tool loss | Thread created on mobile, continued desktop | mac | Browser tool absent from tool list | Tool-injection state not synced across surfaces | Recreate thread on desktop | [codex #22852 2026-05-15](https://github.com/openai/codex/issues/22852) |
| 27 | Codex cache-rebuild disconnect | Update Codex desktop, Win/Edge | Win | Browser plugin still "Not connected" post-rebuild | Plugin registration/injection state desync | Manual reinstall; reset flow (itself buggy) | [codex #24720 2026-05-27](https://github.com/openai/codex/issues/24720) |

---

## F. Strategic Recommendations — Per-OS Decision Matrix

| | **macOS laptop** | **Windows 11 hub / laptops** | **Linux VPS** |
|---|---|---|---|
| **Browser rail** | Chrome for Testing, dedicated persistent `--user-data-dir`, CDP over **pipe** (fallback `:9222`); vendor extension = fallback only | Chrome for Testing, dedicated persistent dir, CDP; **avoid managed host** (policy may block CDP). Firefox/AutoFF for fetch | Chrome for Testing headless-new, dedicated dir, CDP over pipe, in container w/ `socat` if needed; Firefox/AutoFF for cookie/fetch |
| **Screen rail** | ScreenCaptureKit/screencapture **only if unavoidable**; expect monthly re-grant; **prefer DOM/CDP over pixels** | **CDP/DOM only for unattended**; if screen needed, `tscon`-console session, no RDP-disconnect, disable auto-lock | **Xvfb + X11** (`xdotool`+`grim`/`scrot`); Wayland only if libei verified (KDE `KWin.EIS` or nested compositor) |
| **Session persistence** | Pre-logged-in dedicated profile; nightly `storageState` refresh; Keychain "Chrome Safe Storage" handles at-rest | Same; DPAPI/ABE means **don't copy cookies** — drive live profile | Same; gnome-keyring/kwallet **or accept plaintext store** on headless (no keyring) — fine since box is single-tenant |
| **Watchdog** | liveness = `DevToolsActivePort` + WS ping; on `Target.detached`/`404` → 1 rebind → hard-stop on 2FA/CAPTCHA; TCC-grant checker | Same + process-zombie reaper (kill orphan `chrome.exe`/`extension-host.exe`); EPIPE-tolerant supervisor | Same + startup `rm Singleton*`; memory/zombie reaper for long runs |
| **Biggest residual risk** | **Monthly TCC re-prompt + signature-change reset** silently kills screen work; headless-logged-out trap | **Managed-policy CDP block** and **RDP/UAC black-screen** for any screen work; Codex/Electron IPC crashes | **Wayland input no-op** if you stray off X11; forced re-login on ban-sensitive sites still possible |

**Named rail options with trade-offs:**

- **Option A — Self-hosted CfT + CDP + Firefox-fetch + hand-built watchdog (your current plan).** Trade-off: max control, zero per-session cost, but you *own* the reconnect/rebind engineering nobody upstream provides. Risk: Chrome tightens CDP again. **Confidence this survives most overnight runs: [emerging→established].**
- **Option B — Remote-browser service (Steel or Browserbase) for the ban-sensitive tier only.** Steel Profiles/Browserbase Contexts persist auth, add fingerprint+dedicated-IP (the part that actually reduces re-login), and remove Singleton/zombie/permission problems entirely. Trade-off: per-session cost, data leaves your machines, and it's beta-ish (Steel is in public beta; GitHub issues reveal occasional breaking changes between minor versions — pin dependencies aggressively [low-authority] ([starlog.is 2026-05-08](https://starlog.is/articles/automation/steel-dev-steel-browser)])). Steel is open-source/self-hostable if data residency matters. **Confidence for durability on social platforms: [emerging].**
- **Option C — Agentic browser (Comet).** Rejected: Atlas is dead (Aug 9 2026), and the durability is inseparable from an unfixable prompt-injection surface you can't watchdog. **[established] not to build your rail on this.**

**Single recommendation:** **Option A as the default rail on all three OSes, with Option B (Steel, self-hosted if data residency matters) as the dedicated tier for Google/X/Facebook only.** Confidence **[emerging→established]**. Main assumption it rests on: *Chrome for Testing continues to honor CDP and Google does not extend the 144-style per-connection prompt to CfT* — the one external dependency that could force you to Option B wholesale.

---

## G. Action Plan

1. **This week — freeze the mechanical foundation.** Switch every machine's automation Chrome to **Chrome for Testing** with a **dedicated persistent `--user-data-dir`** (never Default). Confirm CDP binds via pipe; add `socat` on the Linux container. Pre-log-in each profile by hand once.
2. **Build the watchdog you assumed didn't exist (it doesn't).** Liveness via `DevToolsActivePort` file + WS ping; on `Target.detached`/`404 No such target` do exactly one re-snapshot+rebind; **hard-stop and page a human on any 2FA/CAPTCHA/"browser may not be secure"/`navigator.webdriver`-triggered challenge.** Add a zombie-`chrome`/`extension-host` reaper and (Linux) startup `rm Singleton*`.
3. **Screen rail per OS.** Linux: stand up **Xvfb + X11** and only touch Wayland after verifying the driver's libei path is compiled and advertised. Windows: **CDP/DOM-only unattended**; if pixels are unavoidable, `tscon`-console session with auto-lock disabled and no RDP disconnect. macOS: **avoid unattended screen work**; if required, grant Screen Recording and add a TCC-grant health check that alerts on the monthly reset and after every app update.
4. **Session hygiene.** Nightly `storageState`/profile refresh from a known-good login; one profile↔one process↔one IP; Firefox/AutoFF for all cookie extraction and headless fetch.
5. **Stand up the Option-B fallback for the social tier.** Spin up self-hosted Steel (or Browserbase) with one profile + one dedicated IP per account for Google/X/Facebook; measure *your own* observed session half-life (the number nobody has published) and let that data decide how much of Option A you keep for those sites.
6. **Watch two upstream signals monthly:** Chrome release notes for further CDP/extension restrictions, and the chrome-devtools-mcp tracker for a real reconnect state machine landing (issues #978/#1094/#1152) — the day that ships, you can retire part of your hand-built watchdog.

---

## H. Sources

**Chrome / CDP mechanism & policy**
- Chrome for Developers, "Changes to remote debugging switches" 2025-03-17 — https://developer.chrome.com/blog/remote-debugging-port
- Chrome Enterprise, RemoteDebuggingAllowed policy — https://chromeenterprise.google/policies/remote-debugging-allowed/
- Chrome DevTools Protocol docs (detach events, target 404) — https://chromedevtools.github.io/devtools-protocol/
- dev.to/timtech4u, Chrome 136 flag change 2026-03-19 — https://dev.to/timtech4u/your-browser-has-a-remote-control-and-nobody-told-you-5e97
- ytyng.com, Docker CDP 0.0.0.0 remap 2026-04-20 — https://www.ytyng.com/en/blog/docker-chromium-cdp-port
- chromedp #1607, remote-debugging-pipe 2025-12-23 — https://github.com/chromedp/chromedp/issues/1607
- trycua/cua #2916, toggle-CDP 404 route ~2026-08 — https://github.com/trycua/cua/issues/2916
- OSV CVE-2026-3941 (DevTools policy bypass) — https://api.osv.dev/v1/vulns/CVE-2026-3941

**chrome-devtools-mcp tracker (transport/reconnect)**
- #510 MetaMask timeout 2025-11-02 — https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/510
- #825 persist CDP approval 2026-01-24 — https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/825
- #978 WS drop on 6+ tabs 2026-02-17 — https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978
- #1094 long-session disconnect 2026-03-04 — https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1094
- #1152 Codex wsEndpoint transport 2026-03-09 — https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1152

**Claude Code tracker**
- #20298 2026-01-23, #21371 2026-01-28, #24144 2026-02-08, #24593 2026-02-10, #31971 2026-03-08, #37541 2026-03-22, #40758 2026-03-30, #48806 2026-04-15, #79445 ~2026-07 — https://github.com/anthropics/claude-code/issues/{20298,21371,24144,24593,31971,37541,40758,48806,79445}

**Codex tracker**
- #20642 2026-05-01, #22849 2026-05-15, #22852 2026-05-15, #23014 2026-05-16, #24720 2026-05-27, #25209 2026-05-30, #35985 ~2026-08, #37539 ~2026-08 — https://github.com/openai/codex/issues/{20642,22849,22852,23014,24720,25209,35985,37539}

**Playwright / Selenium / profile lock**
- playwright #39637 corporate CDP block 2026-03-12 — https://github.com/microsoft/playwright/issues/39637
- selenium #16274 non-default dir 2025; #9723 profile dir — https://github.com/SeleniumHQ/selenium/issues/{16274,9723}
- PinchTab Chrome profiles/Singleton doc — https://huggingface.co/spaces/AUXteam/WitNote/blob/main/docs/implementations/chrome-files.md
- kb.wisc.edu SingletonLock 2025 — https://kb.wisc.edu/cae/148461
- BrowserStack storageState 2025-12-23 — https://www.browserstack.com/guide/playwright-storage-state

**Cookie encryption / ABE**
- yt-dlp.net "could not copy cookie DB" — https://yt-dlp.net/errors/could-not-copy-chrome-cookie-database
- dev.to/osovsky, 6 ways yt-dlp cookies 2026-03-02 — https://dev.to/osovsky/6-ways-to-get-youtube-cookies-for-yt-dlp-in-2026-only-1-works-2cnb
- yt-dlp #10927 DPAPI 2024-09-05 — https://github.com/yt-dlp/yt-dlp/issues/10927
- CyberArk C4 Bomb (ABE) 2025-09-04 — https://www.cyberark.com/resources/threat-research-blog/c4-bomb-blowing-up-chromes-appbound-cookie-encryption
- alternativeto.net, Google ABE announcement 2024-08 — https://alternativeto.net/news/2024/8/google-enhances-chrome-security-on-windows-with-app-bound-encryption-to-fight-cookie-theft

**Screen control / OS permissions**
- 9to5mac Sequoia monthly prompt 2024-08-14 — https://9to5mac.com/2024/08/14/macos-sequoia-screen-recording-prompt-monthly/
- screenify.studio TCC guide 2026-04-23 — https://www.screenify.studio/blog/2026-04-23-macos-screen-recording-permissions
- Jamf community TCCC plist workaround 2024 — https://community.jamf.com/t5/jamf-pro/monthly-tccc-prompts-macos-v15/m-p/339150
- MS Q&A keep GUI after RDP (`tscon`) 2024-03-01 — https://learn.microsoft.com/en-us/answers/questions/1605189/keep-gui-activated-after-rdp-disconnected
- MS docs secure-desktop black screen — https://learn.microsoft.com/fr-fr/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee844127(v=ws.10)
- Ranorex RDP blank screenshots — https://www.ranorex.info/problems-with-remote-desktop-t13094.html

**Wayland**
- who-t, libei in XDG portals (persistence) 2026-07 — http://who-t.blogspot.com/2026/07/libei-integrations-in-xdg-remotedesktop.html
- trycua/cua #1982 KDE input no-op 2026-06-23 — https://github.com/trycua/cua/issues/1982
- isac322/kwin-mcp 2026-05-30 — https://github.com/isac322/kwin-mcp
- cushycush/wdotool — https://github.com/cushycush/wdotool
- Stackademic Wayland remote desktop 2025 2025-11-18 — https://stackademic.com/blog/remote-desktop-on-wayland-in-2025-what-changed-for-linux-support-engineers
- rustdesk libei/RemoteDesktop discussion #4515, #10016 — https://github.com/rustdesk/rustdesk/discussions/{4515,10016}
- ydotool — https://github.com/ReimuNotMoe/ydotool

**Remote-browser services**
- Steel Profiles 2025-10-13 — https://steel.dev/blog/profiles ; docs — https://docs.steel.dev/overview/profiles-api/overview ; comparison 2026-04-01 — https://llms.steel.dev/articles/browser-infrastructure-for-ai-agents-compared/
- Browserbase auth docs 2026-04-07 — https://docs.browserbase.com/platform/identity/authentication
- starlog.is Steel review 2026-05-08 — https://starlog.is/articles/automation/steel-dev-steel-browser

**Agentic browsers**
- techtimes AI browser comparison 2026-06-17 — https://www.techtimes.com/articles/318528/20260616/ai-browser-comparison-2026-atlas-vs-comet-vs-dia-ranked-security-use-case.htm
- nohacks.co agentic landscape 2026 (Atlas retirement) — https://nohacks.co/blog/agentic-browser-landscape-2026
- HumanSecurity Atlas vs Comet 2026-01-12 — https://www.humansecurity.com/learn/blog/chatgpt-atlas-vs-perplexity-comet-agentic-browsers/
- sophiesbureau.com prompt injection 2025-11-03 — https://sophiesbureau.com/digital-ops/ai-prompt-injection-browser-security

**Benchmarks / academic**
- benchmarkingagents.com (flakiness, infra) 2026-05-11 — https://benchmarkingagents.com/agent-benchmarks/
- OSWorld GitHub + Verified 2025-07-28 — https://github.com/xlang-ai/osworld
- WebArena Verified (OpenReview) 2025-09 — https://openreview.net/forum?id=CSIo4D7xBG
- WAREX (infra failure assumptions) arXiv 2510.03285 — https://arxiv.org/pdf/2510.03285
- Online-Mind2Web / Signal-Driven Observation arXiv 2606.06708 — https://arxiv.org/pdf/2606.06708

**Bot detection**
- undetected-geckodriver (navigator.webdriver) — https://pypi.org/project/undetected-geckodriver/1.0.5
- W3C WebDriver minutes (webdriver flag history) — https://www.w3.org/2021/07/14-webdriver-minutes.html
- Google support "browser may not be secure" thread — https://support.google.com/chrome/thread/224353947

**Explicitly unverified in this report:** the exact per-site session half-life numbers (no rigorous public source — measure your own); the Chrome "second programmatic download killed" mechanism (your observation only); the 127.0.0.1-bridge-hang mechanism (partial corroboration via codex #23014 localhost-rejection, but not the same failure); a production CDP heartbeat with auto-rebind (**confirmed does not exist** — the strongest finding against your UNVERIFIED flag).

A caveat on the two "up to 30 days" durability numbers: those are vendor storage-lifetime claims, not independently measured site-accepted lifetimes, and are tagged [low-authority] above.