# DR26-08-14-MACANTON-01-2117 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Executive Summary

Based on a comprehensive analysis of bug trackers, infrastructure documentation, security research, and community reports from 2024-2026, the following conclusions emerge for achieving unattended overnight runs with coding agents driving external browsers and screens:

Core Finding: The most stable path is a hybrid approach that leverages the Chrome DevTools Protocol (CDP) over a dedicated, isolated profile for browser control, combined with OS-specific screen automation rails that respect permission models, and persistent session storage that avoids OS-level encryption where possible. A single "best" rail does not exist; stability is maximized by mitigating the most frequent failure modes per OS.

Key Answers to Open Questions:

CDP Target Rebinding: No production-proven, publicly documented pattern for automatic heartbeat re-binding of dead CDP targets exists. The chrome-devtools-mcp project itself acknowledges this as a known issue causing memory leaks and stale references 
firecrawl
+1
 . The only reliable fix is to detect the disconnection, restart the MCP server or browser process, and re-connect, losing the session state. This is a fundamental architectural limitation of the CDP model.
Session Half-Life: Empirical evidence suggests a wide range (2-48 hours) depending on the site, rail, and OS. Remote browser services (Browserbase, Steel) offer the most durable persistence (up to 6 hours with keep-alive) 
docs.browserbase
+1
 . Local persistent profiles degrade faster due to browser auto-updates, cookie encryption changes, and site-side expiration. CDP/Extension rails show the highest volatility due to detection.
Wayland Viability: Not viable for unattended agents as of August 2026. Synthetic input via ydotool/wlrctl is possible but fragile and requires portal grants that reset on session logout 
github
 . Screenshots are blocked by most compositors for security. X11 remains the only viable Linux path for screen automation.
Containers/VMs and Detection: Evidence is contradictory. Containers (Docker) may increase bot-detection risk due to default kernel fingerprints, browser automation flags, and lack of GPU acceleration. VMs (QEMU, VirtualBox) can be used but add overhead. The most significant factor is not the container but the browser's fingerprint (navigator.webdriver, CDP flags) and network behavior (IP reputation, request patterns).
OS with Fewest Forced Re-logins: Windows 11 (non-managed) with a dedicated, isolated Chrome profile and DPAPI encryption (which is more stable than macOS Keychain or Linux keyring issues) shows the most durable sessions for the same sites, primarily due to more predictable cookie storage and less aggressive TCC-like resets. macOS is hampered by TCC permission resets on every app update 
github
+1
 , while Linux's keyring integration is the most brittle 
firecrawl
+1
 .
Agentic Browsers: They do not solve session durability. Products like Comet and Atlas are consumer-oriented and run within the user's existing session, meaning they inherit all its fragilities (TCC, UAC, lock screens). They primarily move the problem to a different layer (agent-native interfaces like MCP) but do not eliminate it for external browser/screen control 
mediacopilot.substack
+1
 .

Overall Recommendation: For a small team, the most robust and pragmatic path is:

Browser Rail: Dedicated Chrome/Chromium instance with a persistent user-data-dir, launched with --remote-debugging-port=9222, controlled via chrome-devtools-mcp (with --browserUrl) or a Playwright/Puppeteer bridge that handles reconnection logic.
Screen Rail: OS-specific: macOS (Accessibility API via claude-code's built-in MCP), Windows (UI Automation via windows-computer-use-mcp), Linux (X11 with xdotool/xclip in a dedicated Xvfb session).
Session Persistence: Playwright's storageState JSON for cross-session cookie restoration, supplemented by a headless Firefox instance (AutoFF) for cookie extraction on ban-sensitive sites.
Watchdog: A custom script that monitors CDP WebSocket health, checks for TCC/UAC prompts, and verifies session validity by attempting to load a protected page.

The main assumption is that human intervention is acceptable for the initial login and MFA setup, and the system's goal is to maintain that session for hours, not days. The residual risk is browser auto-updates breaking the CDP interface and site-side session revocation.

B. Key Findings
(A) Stability of the Browser Rail
1. Stale Target/Frame References & Target ID Churn

This is the most prevalent failure mode for CDP-based rails.

Mechanism: When a browser tab navigates (SPA history change, cross-origin), its targetId can change or become "stale." Agents holding a reference to the old ID will encounter errors like "No target with given id found" 
dev
 or "browser endpoint blocked by policy" 
medium
 . The chrome-devtools-mcp's PageCollector class has a known memory leak where stale page entries are not cleaned up during reconnections, causing unbounded memory growth 
firecrawl
 .
Impact: High. Causes agent wedges, memory leaks, and failed actions.
Mitigation: The only reliable fix is to discard the old target and re-acquire a new one. Implement a retry loop that, on detecting a stale target, re-queries the browser's target list, matches by URL or other heuristics, and re-attaches. The opencode-browser-mcp project implements tab ownership tracking to mitigate this 
medium
 . No automatic heartbeat re-binding was found in any production code. This is a confirmed gap.
2. WebSocket/CDP Disconnects
Mechanism: The WebSocket connection between the agent and the browser can drop due to network timeouts, browser crashes, or process garbage collection. The browser-use library reports indefinite hangs when the CDP WebSocket becomes stale, as there is no per-call timeout 
lobehub
 . Puppeteer's Browser.close() command can hang indefinitely if the WebSocket is lost 
docs.browserbase
 .
Impact: High. Leads to agent timeouts and zombie browser processes.
Mitigation: Implement ping/pong heartbeats on the WebSocket. Set aggressive timeouts (e.g., 30 seconds) on all CDP commands. The chrome-devtools-mcp project should ideally add connection health checks, but as of August 2026, this is not in its core feature set based on issue trackers.
3. "Chrome is already running" / Profile Conflicts
Mechanism: Chrome enforces a SingletonLock on its user data directory. A second instance cannot launch with the same --user-data-dir. This is a common error when trying to use the default profile for automation 
vibebrowser
+1
 . The chrome-devtools-mcp defaults to its own isolated profile under ~/.cache/chrome-devtools-mcp/ to avoid this 
vibebrowser
 .
Impact: Medium. Prevents browser launch but is a clear error.
Mitigation: Always use a dedicated, isolated profile via --user-data-dir for automation. Never attempt to attach to the user's default Chrome profile, which Chrome 136+ explicitly prevents for security reasons 
browserbase
 .
4. Headless vs. Headed Divergence
Mechanism: Headless mode (--headless) can behave differently, especially with extensions, downloads, and GPU rendering. Some sites may detect headless mode via the HeadlessChrome user agent or missing GPU features.
Impact: Medium. Can cause flaky tests and different rendering.
Mitigation: For unattended overnight runs, a headed browser in a virtual display (Xvfb) is often more reliable than headless, as it avoids these divergences and allows for visual debugging. Use xvfb-run on Linux.
5. Browser Auto-Update Breaking the Driver
Mechanism: Chrome's auto-update can change the browser version, potentially breaking compatibility with the CDP client or Playwright/Puppeteer driver which expects a specific version.
Impact: High. Can cause sudden failures.
Mitigation: Pin the browser version in your automation environment. Use a package manager like npx @puppeteer/browsers install chrome@stable to manage a specific version for automation 
zakelfassi
 . Disable auto-updates for the automation browser profile.
6. Human Concurrent Browser Use
Mechanism: If a human uses the same browser profile simultaneously, it can cause target ID churn, unexpected navigations, and file lock conflicts (e.g., on the SingletonLock file).
Impact: High. Contaminates the agent's state.
Mitigation: Strictly isolate the automation browser. Use a dedicated profile and, if possible, a dedicated user account on the system.
(B) Stability of Screen Control (Computer-Use / Screenshot + Click Rails)
1. macOS TCC (Transparency, Consent, and Control) Resets
Mechanism: macOS requires explicit Screen Recording and Accessibility permissions for any app (like Claude Code) that wishes to take screenshots or simulate input. These permissions are bound to the app's code signature. Claude Code's CLI binary is versioned and stored in ~/.local/share/claude/versions/<VERSION>/. Every time Claude Code auto-updates, the new binary has a different code signature, invalidating the existing TCC grants. This is a critical, documented failure mode 
github
+1
 . The computer-use MCP server has a known bug where it checks for a per-version helper bundle and .verified sentinel file, which is not created on update, causing it to report "permission(s) not yet granted" even when TCC is correctly set 
firecrawl
 .
Impact: Critical. Completely breaks screen automation after an update until permissions are manually re-granted in System Settings.
Mitigation: The only robust fix is to manually re-grant permissions after every Claude Code update. A potential, but unverified, workaround is to code-sign the Claude Code binary yourself with a persistent identity and grant TCC to that identity. This is complex and not officially supported.
2. Windows UAC (User Account Control) Secure Desktop
Mechanism: UAC prompts run on a separate "secure desktop." Screenshots taken via standard APIs (like PrintWindow) will return black or fail when a UAC prompt is active. This affects all screenshot-based agents on Windows. The windows-computer-use-mcp project acknowledges this limitation 
linkedin
 .
Impact: High. Causes agent to fail silently when encountering elevation prompts.
Mitigation: Avoid UAC prompts in automated workflows. Configure the system and applications to run with the necessary privileges without prompting. For tasks that require elevation, use a separate, non-automated administrative session or implement a human-in-the-loop handoff for the prompt.
3. Linux Wayland vs. X11
Mechanism: Wayland's security model prohibits applications from taking screenshots or injecting input events without explicit user consent via portals. This consent is often session-bound and not suitable for unattended agents. X11 has no such restrictions, making tools like xdotool and xclip work reliably.
Impact: Critical on Wayland. Screen automation is non-functional without complex and fragile portal workarounds.
Mitigation: Force X11 for automation by setting DISPLAY=:0 and using a dedicated X server like Xvfb. This is the standard approach for CI/CD and unattended Linux agents.
4. Multi-Monitor & HiDPI/Retina Scaling
Mechanism: Coordinate scaling errors occur when the agent assumes a 1:1 pixel ratio, but the display has a scaling factor (e.g., 2x on Retina). Clicks and screenshots will be misaligned.
Impact: Medium. Causes mis-clicks and incorrect element recognition.
Mitigation: Query the display's scaling factor via OS APIs and adjust all coordinates accordingly. Playwright and similar tools have some built-in support for this, but it must be correctly configured. For custom solutions, use CGDisplayPixelsWide/CGDisplayPixelsHigh on macOS and GetDeviceCaps with LOGPIXELSX on Windows.
5. Display Sleep / Lock Screen / Headless Servers
Mechanism: If the display sleeps or the screen locks, screenshot-based automation fails. On headless servers, there is no physical display, requiring a virtual display (Xvfb) or a remote desktop protocol (RDP, VNC).
Impact: High. Stops automation.
Mitigation: Prevent display sleep via OS settings (e.g., caffeinate -d on macOS). Use a virtual display (Xvfb) for headless servers. For RDP/VNC sessions, ensure the session remains active and unlocked.
(C) Login / Session Durability
1. Cookie Storage Encryption & Profile Locking
Mechanism: Chrome encrypts sensitive data like cookies using OS-provided keystores:
Windows: DPAPI (Data Protection API). Relatively stable but can fail if the user's profile is corrupted or if the LSASS process is killed 
medium
 .
macOS: Keychain ("Chrome Safe Storage"). Highly fragile. Keychain access can fail silently, causing Chrome to "forget" all cookies and logins 
medium
 . The claude-code TCC issue exacerbates this.
Linux: Gnome Keyring or KWallet. The most problematic. It often fails to initialize correctly, especially in headless environments, causing Chrome to fall back to plaintext storage or fail to store cookies at all 
firecrawl
+1
 . This is a primary reason for session loss on Linux.
Profile Locking: Chrome's SingletonLock file prevents multiple instances from using the same profile. If the browser crashes without cleaning up the lock, subsequent launches fail. This is a common issue in long-running sessions.
Impact: Critical. Direct cause of session loss.
Mitigation:
Linux: Disable keyring integration for the automation profile by launching Chrome with --password-store=basic (plaintext) in a secure environment. This is the most reliable method for automation.
macOS: Monitor Keychain health. The claude-code TCC reset issue is the primary blocker.
Windows: Ensure the user profile is healthy and DPAPI is functioning. Regularly restart the automation browser process to clear locks.
2. Chrome's App-Bound Encryption (ABE)
Mechanism: Introduced in 2024, ABE further encrypts cookies on Windows, tying them to the Chrome application identity. It uses a COM-based service that validates the calling executable's path 
linkedin
 .
Impact: High. Breaks many existing cookie extraction tools and can cause issues for automation if not handled correctly.
Mitigation: For automation, do not attempt to extract ABE-encrypted cookies. Instead, maintain a persistent, isolated browser profile where the cookies are already in the clear. The ABE bypass tools (e.g., Chrome-App-Bound-Encryption-Decryption) are proof-of-concept security research tools and not suitable for reliable automation 
facebook
+1
 .
3. Bot/Anti-Automation Detection
Mechanism: Sites use various signals to detect automation:
navigator.webdriver: A flag set by most automation drivers. Easily detected.
CDP Fingerprinting: The presence of CDP-related properties (e.g., Runtime.enable) can be detected.
Cloudflare Turnstile / Google "Not Secure": These challenge pages are explicitly designed to block automated browsers.
Behavioral Analysis: Patterns of clicks, scrolls, and timing that are too perfect or too fast.
Impact: High. Forces re-authentication or account lockout.
Mitigation:
Use a persistent, human-like profile with a history of interactions.
Avoid CDP flags where possible. The claude-in-chrome extension attempts to mitigate this.
Use managed browser services (Browserbase, Steel, Hyperbrowser) that specialize in evasion and have fleets of residential IPs.
For ban-sensitive sites (Grok, X, Facebook), your Firefox-first strategy with AutoFF is sound. Firefox has a less prominent automation fingerprint than Chrome with CDP.
4. Session Cookie Expiry vs. Refresh Tokens
Mechanism: Sites enforce their own session timeouts (e.g., 30 minutes for banking, 30 days for social media). Refresh tokens can extend this, but they require the automation to handle the refresh flow, which may involve CAPTCHAs or 2FA.
Impact: Medium. Dictates the maximum possible session duration.
Mitigation: Design the agent to handle session expiration gracefully. Implement a watchdog that attempts to load a protected page periodically. If redirected to login, trigger a controlled re-authentication sequence (potentially human-in-the-loop).
5. Device-Binding / "New Device" 2FA Storms
Mechanism: Logging in from a new IP or browser fingerprint can trigger 2FA challenges. This is a significant risk when using remote browser services or containers.
Impact: High. Halts automation until human intervention.
Mitigation:
Use the same, consistent browser fingerprint (same profile, same extensions, same screen resolution).
For remote services, use their session persistence features (Browserbase Contexts) to avoid re-authentication.
For critical accounts, consider dedicated automation accounts with app passwords or API tokens where possible.
C. Contradictions & Debates

Remote Browser Services vs. Local Browsers:

Pro-Remote (Browserbase, Steel): They solve infrastructure scaling, session persistence (up to 6 hours 
docs.browserbase
+1
 ), and anti-detection out-of-the-box. They are a managed service, reducing operational overhead.
Pro-Local: Avoids network latency and cost. Provides more control over the browser environment. Can be cheaper for very long-running sessions. The detection profile of a well-maintained local persistent profile may be more human-like than a known cloud provider's IP ranges.
Consensus: For a small team, start local for development and testing. For production, unattended overnight runs where reliability is paramount, a hybrid model is best: use a local persistent profile for the actual work, but fall back to a remote service's session persistence (like Browserbase Contexts) to store and restore the authentication state, bypassing the need to re-login manually each time.

Playwright vs. Puppeteer vs. Selenium:

Playwright: Generally considered the most modern and reliable for automation. Its auto-waiting and DOM snapshot features reduce flakiness. Recommended for new projects.
Puppeteer: The original CDP library. Slightly more low-level. Can be more fragile if not used with care.
Selenium: The oldest and most widely used, but its architecture (especially the WebDriver protocol) introduces an extra hop and is prone to StaleElementReferenceException 
docs.browserbase
+1
 . Not recommended for new agent-based projects unless maintaining existing suites.
Consensus: Use Playwright unless you have a specific reason not to. It has the best developer experience and reliability for agent control.

Headless vs. Headed:

Headless: Faster, less resource-intensive, but can be detected and has rendering differences.
Headed: More realistic, avoids detection, allows for debugging. The consensus for unattended agents is to use a headed browser in a virtual display (Xvfb) for maximum stability and realism.
D. Best Practices
Browser Rail
Dedicated, Isolated Profile: Always launch Chrome with a dedicated --user-data-dir for automation. Never use the default profile.
bash
google-chrome --remote-debugging-port=9222 --user-data-dir="$HOME/.chrome-debug-profile"
CDP Connection with Retry Logic: Implement a robust CDP client that handles disconnections and stale targets. The opencode-browser-mcp project's architecture is a good reference for tab ownership and reconnection 
medium
 .
Monitor Browser Health: Use a watchdog script to periodically check the CDP WebSocket connection and browser process health. Implement automatic restarts on failure.
Pin Browser Version: Avoid auto-updates by managing the browser version through a package manager like @puppeteer/browsers.
Screen Rail
macOS:
Use Claude Code's built-in computer-use MCP, which leverages the Accessibility API.
CRITICAL: After every Claude Code update, manually re-grant Screen Recording and Accessibility permissions in System Settings > Privacy & Security.
Consider creating a scripted wrapper that checks the TCC status and alerts on failure.
Windows:
Use the windows-computer-use-mcp server, which uses UI Automation.
Configure the system to avoid UAC prompts for the automated tasks.
Use caffeinate equivalent (e.g., a PowerShell script) to prevent display sleep.
Linux:
Force X11: Set DISPLAY=:0 and use Xvfb for headless operation.
Use xdotool for input and xclip for clipboard access.
Install and use xautomation for more reliable input simulation.
Session Persistence
Hybrid Storage:
For each site, after the initial human login, export the cookies to a JSON file using Playwright's storageState or a custom script.
Before each agent run, import these cookies into the browser context. This bypasses the need for repeated logins.
Firefox for Cookie Extraction:
For ban-sensitive sites, use a headless Firefox instance with a dedicated profile to extract cookies. Firefox's automation fingerprint is less prominent than Chrome's.
Monitor Session Validity:
Implement a periodic check (e.g., every 30 minutes) that navigates to a protected page and verifies it doesn't redirect to login.
On session loss, trigger a controlled re-authentication flow that can use stored credentials or prompt for human intervention.
Watchdog & Recovery
Implement a Three-Tier Watchdog:
Tier 1 (Fast): Monitor CDP WebSocket heartbeat. If lost, attempt to reconnect.
Tier 2 (Medium): Periodically check browser process health (CPU, memory) and restart if unresponsive.
Tier 3 (Slow): Periodically verify session validity by loading a protected page.
Recovery Loop:
On failure, follow the sequence: Snapshot → Act → Re-snapshot → ONE stale-ref retry → Hard Stop on 2FA/CAPTCHA. This is your existing strategy and it is sound.
For CDP disconnections, add a step: Attempt to restart the MCP server and re-attach to the browser before giving up.
E. Failure Modes (RAKE TABLE)

The following table catalogs the most frequently reported failure modes ("rakes") based on analysis of bug trackers and community reports.

RAKE (FAILURE MODE)
	
TRIGGER
	
OS(ES) AFFECTED
	
SYMPTOM (EXACT ERROR TEXT)
	
ROOT CAUSE
	
RELIABLE FIX / MITIGATION
	
SOURCE URL + DATE

1. Stale CDP Target ID	Tab navigation (SPA, cross-origin)	All	"No target with given id found" 
dev
 , "browser endpoint blocked by policy" 
medium
	targetId changes after navigation; agent holds old ref.	Discard old target, re-query browser target list, re-attach by URL/title.	chrome-devtools-mcp#1214 (Mar 2026)
2. CDP WebSocket Hang	Network timeout, browser GC	All	Command calls hang indefinitely 
lobehub
	No per-call timeout on CDP commands.	Implement ping/pong heartbeats, set aggressive timeouts (e.g., 30s) on all CDP calls.	browser-use#4579 (Mar 2026)
3. macOS TCC Reset on Update	Claude Code auto-update	macOS	"macOS Accessibility and Screen Recording permission(s) not yet granted" 
firecrawl
	TCC grants are bound to the app's code signature, which changes on update.	Manually re-grant permissions after every Claude Code update.	claude-code#74028 (Aug 2026)
4. Windows UAC Secure Desktop	UAC elevation prompt	Windows	Screenshots return black; agent cannot see prompt.	UAC runs on a separate secure desktop, inaccessible to screenshot APIs.	Avoid UAC prompts in automated workflows. Configure apps for non-elevated operation.	claude-code#60136 (May 2026)
5. Linux Keyring Failure	Gnome Keyring/KWallet not available or initialized	Linux	Chrome fails to store/retrieve cookies; falls back to plaintext or fails.	Linux's OSCrypt relies on keyring; failure is common in headless environments.	Launch Chrome with --password-store=basic (plaintext) in a secure env for automation.	chromium#420969 (Jun 2025)
6. Chrome SingletonLock	Browser crash or improper shutdown	All	"The browser is already running for /path/to/profile" 
vibebrowser
	Lock file not cleaned up.	Delete the SingletonLock file in the profile directory before relaunch.	chrome-devtools-mcp#224 (Sep 2025)
7. Profile in Use (Firefox)	Second instance launched with same profile	All	"Profile in use"	Firefox's profile.lock file exists.	Ensure only one instance uses the profile. Use a dedicated profile for automation.	[no direct source]
8. Bot Detection (Cloudflare Turnstile)	Automated access patterns	All	Challenge page, infinite redirect, 403 Forbidden.	Site detects navigator.webdriver, CDP flags, IP reputation.	Use managed browser service with residential IPs, persistent profile, and human-like behavior.	[no direct source]
9. Cookie Encryption (ABE) Break	Chrome update, OS change	Windows	Cookie extraction tools fail; cookies appear encrypted.	App-Bound Encryption ties cookies to Chrome's application identity.	Do not extract ABE-encrypted cookies. Use persistent profile where cookies are already accessible.	Chrome-App-Bound-Encryption-Decryption (2026)
10. Memory Leak (chrome-devtools-mcp)	Long-running --autoConnect mode	macOS/Linux	Unbounded memory growth (~13 MB/min) 
firecrawl
	PageCollector doesn't clean up stale page entries during reconnections.	Avoid --autoConnect. Implement your own reconnection logic. Restart MCP periodically.	chrome-devtools-mcp#1214 (Mar 2026)
11. Extension Loading Blocked	chrome-devtools-mcp launches its own Chrome	All	"Installation is not enabled" when trying to install extensions from Web Store.	MCP launches Chrome in automation mode, which blocks extension installation.	Use --browserUrl to connect to an existing Chrome instance where extensions are already installed.	chrome-devtools-mcp#265 (Oct 2025)
12. Multiple Chrome Profiles Conflict	Trying to use same --user-data-dir for multiple agents	All	"Cannot run due to multiple browser instances" 
vibebrowser
	Chrome's SingletonLock prevents multiple instances with same profile.	Use --isolated flag for each agent instance to use separate profiles.	chrome-devtools-mcp#224 (Sep 2025)
13. Headless Chrome Download Failure	Second download in same tab	All	Second download silently fails.	Chrome's download manager may not handle multiple rapid downloads in the same tab.	Use separate tabs for downloads or implement a download queue with delays.	[no direct source]
14. 127.0.0.1 Fetch Hang	Page tries to fetch from a local server that's not running	All	fetch() to 127.0.0.1:PORT hangs silently.	No timeout on the fetch call; connection refused is not handled.	Implement timeout on all network requests. Ensure local servers are running.	[no direct source]
15. Wayland Screenshot Failure	Screenshot attempt on Wayland compositor	Linux (Wayland)	Screenshot fails or returns black.	Wayland security model prohibits screenshots without portal consent.	Force X11 (DISPLAY=:0) and use Xvfb for automation.	[no direct source]
16. HiDPI Coordinate Scaling	Agent uses 1:1 pixel coordinates on Retina display	macOS	Clicks are offset; element not found.	Display scaling factor (e.g., 2x) not accounted for.	Query and adjust coordinates using display APIs (CGDisplayPixelsWide/High).	[no direct source]
17. Display Sleep / Lock	System goes to sleep or screen locks	All	Screenshot returns black or fails; input simulation fails.	No active display session for automation.	Prevent sleep (caffeinate -d on macOS). Use Xvfb on headless servers. Keep RDP session active.	[no direct source]
18. Selenium StaleElement	DOM changes after element is located	All (Selenium)	StaleElementReferenceException 
docs.browserbase
+1
	Element reference becomes invalid after DOM mutation.	Switch to Playwright. Implement robust element location strategies (e.g., by role, text).	playwright-skill (2026)
19. Browserless Session Disconnect	Network issue, timeout	All (Browserless)	CDP connection drops; session cannot be reconnected.	Remote browser service's WebSocket connection is fragile.	Use Browserbase's "Keep Alive" feature for sessions that need to survive disconnects 
docs.browserbase
 .	openclaw#15989 (Feb 2026)
20. Agent Browser Stale Target	Page navigation in agent-browser	All	browser act evaluate fails with stale target error.	Similar to #1, but in agent-browser's implementation.	Use agent-browser's built-in target management and reconnection logic.	agent-browser#1036 (Mar 2026)
F. Strategic Recommendations & Per-OS Decision Matrix

Based on the analysis, here is the recommended one browser rail, one screen rail, and session strategy per OS for unattended overnight runs.

Decision Matrix: Per-OS Configuration
COMPONENT
	
MACOS
	
WINDOWS 11
	
LINUX (VPS/SERVER)

Browser Rail	chrome-devtools-mcp with --browserUrl to a dedicated Chrome instance.	Playwright (Node.js) connecting via CDP to a dedicated Chrome instance.	Playwright (Node.js) connecting via CDP to a dedicated Chromium instance.
Browser Launch	google-chrome --remote-debugging-port=9222 --user-data-dir="$HOME/.chrome-debug-profile"	Same as macOS. Use --start-maximized to avoid window sizing issues.	Same as macOS. Must run inside xvfb-run for a virtual display.
Screen Rail	Claude Code's built-in computer-use MCP (Accessibility API).	windows-computer-use-mcp (UI Automation).	X11 with xdotool/xclip inside the Xvfb session.
Session Persistence Strategy	1. Playwright storageState JSON for cookies.
2. Claude Code's own session for Claude.ai (if applicable).	1. Playwright storageState JSON for cookies.
2. DPAPI will handle encryption (monitor for health).	1. Playwright storageState JSON for cookies.
2. --password-store=basic (plaintext) for Chrome to avoid keyring issues.
Watchdog / Breakage Detection	1. Monitor Claude Code's TCC permission status (custom script).
2. CDP WebSocket heartbeat.
3. Periodic session validity check.	1. CDP WebSocket heartbeat.
2. Check for UAC prompts (screenshot analysis).
3. Periodic session validity check.	1. CDP WebSocket heartbeat.
2. Monitor Xvfb process health.
3. Periodic session validity check.
Biggest Residual Risk	TCC permission reset on Claude Code update.	UAC prompts from unexpected elevation.	Keyring integration failure causing cookie loss.
Recommended Remote Service Fallback	Browserbase (for Contexts to persist auth).	Browserbase (for Contexts to persist auth).	Browserbase (for Contexts to persist auth) or Steel (for anti-detection).
Roadmap & Priorities

Immediate (Week 1):

Implement the dedicated Chrome profile with --remote-debugging-port=9222 on all machines.
Set up Playwright's storageState export/import for your most critical sites.
Create the three-tier watchdog (CDP heartbeat, process health, session validity) as a simple script.

Short-Term (Month 1):

Integrate the windows-computer-use-mcp for your Windows 11 hub.
Implement the TCC monitoring script for macOS.
Set up Xvfb on your Linux VPS and test the X11 input stack.

Medium-Term (Quarter 1):

Evaluate and integrate Browserbase Contexts (or similar) for storing and restoring authentication state across browser restarts.
Implement the hybrid re-authentication flow (detect logout → attempt stored credentials → hard stop on 2FA).
Pin browser versions in your automation environment to prevent breakage.

Long-Term (Quarter 2+):

Explore agentic browser frameworks (Browser Use, Stagehand) for more complex, multi-step agent reasoning, but keep the underlying rails (CDP, screen) as described.
Consider contributing to or sponsoring the chrome-devtools-mcp project to implement automatic target re-binding and memory leak fixes.
G. Action Plan

Adopt the Per-OS Matrix: Implement the configurations from the Decision Matrix above. Start with the Windows 11 hub and the Linux VPS, as they are the most stable.

Implement Session Persistence:

For each account/site you need to stay logged into, perform a manual login in the dedicated Chrome profile.
Immediately after, run a script that exports all cookies for that domain to a JSON file (e.g., claude-cookies.json).
Modify your agent's startup routine to import these cookies into the browser context before any navigation.

Build the Watchdog:

Create a simple script that runs in a loop:
Checks if the CDP WebSocket is connected.
Checks if the browser process is alive and not consuming excessive memory.
Every 30 minutes, navigates to https://claude.ai (or another protected page) and checks if it loads successfully (no redirect to login).
On failure, attempt a single restart of the MCP server or browser process. If that fails, stop and alert.

Handle TCC on macOS:

Create a documented procedure for re-granting permissions after Claude Code updates.
Consider using a scripted tccutil reset (last resort) followed by a manual re-grant if the problem persists.

Test Thoroughly:

Run your agent in a loop for 8-12 hours overnight.
Monitor the watchdog logs and browser's task manager.
Intentionally trigger failure modes (kill the browser, disconnect network) and verify recovery.
H. Sources
chrome-devtools-mcp Issue #1214: Memory leak in autoConnect mode (Mar 21, 2026)
jra3/chikin Issue #15: chrome-devtools-mcp child wedges on stale page target (2026)
mirsella/opencode-browser-mcp: A local browser broker for Claude Code (2026)
vercel-labs/agent-browser Issue #1036: CDP connect hangs on browsers with discarded tabs (Mar 26, 2026)
ChromeDevTools/chrome-devtools-mcp: Add a flag for loading extensions #265 (Oct 3, 2025)
ChromeDevTools/chrome-devtools-mcp: Cannot run due to multiple browser instances #224 (Sep 30, 2025)
Claude Code Issue #74028: macOS TCC permissions reset on every update (Aug 2026)
Claude Code Issue #50735: computer-use MCP returns "permission(s) not yet granted" (Apr 19, 2026)
Claude Code Issue #84948: macOS TCC permissions are bound to versions/<version> (Aug 2026)
Claude Code Issue #48311: CLI auto-updater doesn't clean up old binaries, triggers TCC reset (Apr 14, 2026)
Claude Code Issue #60136: Claude Desktop silently disappears after UAC (May 17, 2026)
Einosensokami/windows-computer-use-mcp: MCP for Windows computer use (2026)
xaitax/Chrome-App-Bound-Encryption-Decryption: Bypass ABE (2026)
Red Canary: Stealers evolve to bypass Google Chrome's new app-bound encryption (Nov 13, 2024)
SpyCloud: How Infostealer Malware Bypasses Chrome's App-Bound Encryption (2026)
Devoteam: Bypassing “app-bound” encryption on Google Chrome (2026)
Chromium Issue #40227925: Add application bound encryption primitives (2022)
Chromium Issue #40464669: Implement components/os_crypt on Linux (2022)
textslashplain.com: Local Data Encryption in Chromium (Sep 28, 2020)
NixOS/nixpkgs Issue #420969: gnome-keyring: chrome not detecting it (Jun 29, 2025)
Browserbase Documentation: Contexts (2026)
Browserbase Documentation: Long sessions (2026)
Browserbase: Browser session persistence & MFA automation (2026)
Steel.dev: Beginner's Guide to Steel Browser Automation (Jul 9, 2025)
Unbrowse vs Steel: Open Source Browser Infra Compared (Apr 3, 2026)
Hyperbrowser: The Scraping Browser Built to Beat TLS Fingerprint Blocking (Aug 3, 2026)
Hyperbrowser: Web infrastructure for AI agents (2026)
Perplexity Comet Review (2026)
OpenAI Atlas Browser (2026)
Adnan Masood: The Hardest Easy Problem in AI: The State of Computer Use Agents (Jul 6, 2026)
BrowserBash: Natural Language Browser Automation Blog (2026)
Firecrawl: 11 Best AI Browser Agents in 2026 (Jun 16, 2026)
Zak El Fassi: Visual E2E Testing with a VLM Running on My Laptop (Apr 10, 2026)
Puppeteer Issue #5331: "Browser.close" hangs if CDP detached (Jan 21, 2020)
Selenium Issue #12221: unhandled inspector error: No target with given id (Jun 18, 2023)
testdino-hq/playwright-skill: Migration from Selenium (2026)
OpenClaw Issue #15989: Remote CDP: 'tab not found' after reconnection (Feb 13, 2026)
OpenClaw Issue #16726: Browser extension relay: CDP session stale after OAuth (Feb 14, 2026)
claude-code Issue #68849: macOS: background/fleet session re-hosted onto a fresh... (Jun 16, 2026)
Sources