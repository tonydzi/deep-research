# DR26-08-01-MACANTON-01-0631 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll research OpenClaw and Hermes browser-session architecture end-to-end — official docs, source patterns, and failure modes — then map what transfers cleanly onto a Claude Code MCP stack.Pulling official docs and mechanism-level sources next.# DR26-08-01-MACANTON-01-0631  
## OpenClaw & Hermes Browser-Access Architecture: Session Resilience and Transfer to Claude Code

**Assumptions (stated explicitly, no clarifying questions):**
1. “Vendor live-tab MCP” = Claude Code’s **Claude in Chrome** extension channel (shared login state, visible window, agent pauses on login/CAPTCHA).
2. “Our stack” = Claude Code + optional local MCP servers we can install ourselves.
3. Goal is **session durability** (CDP/WebSocket + login state), not a full rewrite of agent policy.
4. Sources prioritized to official docs (OpenClaw, Hermes, Chrome DevTools, Browser Use) from ~2025-12 through 2026-08; secondary blogs/issues tagged.

---

## 1. Architecture — how the agent reaches the browser

### 1.1 OpenClaw — four attach paths, one control surface

OpenClaw does **not** own a single browser protocol path. The agent talks to a **Gateway-local browser control service** (loopback). That service speaks CDP (and Playwright over CDP for advanced actions) to one of several profiles.

| Profile / mode | Connection topology | Login state source | Human present? |
|---|---|---|---|
| **`openclaw` (managed)** | Gateway → loopback control → launch Chromium with **dedicated user-data-dir** → CDP ports **18800–18899** | Isolated profile on disk | No |
| **`user` (existing-session / Chrome MCP)** | Gateway → **Chrome DevTools MCP `--autoConnect`** → Chrome remote-debugging consent → live tabs | Your real Chrome profile | Yes (Allow dialog) |
| **`chrome` (extension relay)** | Gateway → **loopback WebSocket relay** (presents CDP) ↔ **MV3 extension** (`chrome.debugger`) → only tabs in **OpenClaw tab group** | Your real signed-in Chrome | No (remote phone OK) |
| **Remote CDP** | Gateway → `cdpUrl` (http(s)/ws(s)) → external Chromium (Browserless, Browserbase, Notte, self-host) | Provider / remote profile | Depends |

**Extension relay topology (mechanism-level):**
1. Browser control service (Gateway or node host) starts a **loopback WebSocket relay**.
2. Relay exposes a **CDP-shaped endpoint** to OpenClaw and authenticates both sides with a **host-local token**.
3. Extension attaches to tabs via **`chrome.debugger`**, forwards CDP, manages the **OpenClaw tab group** (drag-in = share, drag-out = revoke).

OpenClaw documents this as the same *shape* as Anthropic’s Claude-in-Chrome and OpenAI’s Codex Chrome extensions.

**Managed / Playwright path:**
- Dedicated agent-only profile; “never touches your personal browser profile.”
- Playwright used for advanced features (AI snapshots, PDFs, downloads metadata); ARIA/role snapshots can work with lighter paths. [single-source: secondary blog; consistent with official “Playwright-backed profiles” wording]

**Remote / attach-only:**
- `browser.profiles.<name>.cdpUrl` attaches; OpenClaw does **not** launch a local browser.
- Loopback external CDP (e.g. Browserless in Docker) needs **`attachOnly: true`**, or OpenClaw treats it as a managed local profile.

```
Agent ──browser tool──► Gateway control service (loopback)
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
   Managed Chromium      Extension Relay      Chrome MCP / Remote CDP
   (user-data-dir)       (WS + chrome.debugger)  (autoConnect / cdpUrl)
   CDP :18800+           CDP-shaped :relay        live session / cloud
```

**Confidence:** [established] for topology from official OpenClaw docs; port numbers and Playwright details [emerging] where only secondary writeups expand beyond the docs.

---

### 1.2 Hermes — multi-provider toolset with local CDP attach

Hermes browser tools (`browser_navigate`, `browser_snapshot`, `browser_click`, …) are **backend-swappable**. Pages are **accessibility trees** with refs (`@e1`, …).

| Backend | Transport | Browser process ownership |
|---|---|---|
| **Browserbase / Browser Use / Firecrawl** | Provider cloud APIs (+ Browserbase keep-alive / stealth) | Provider |
| **Camofox** | HTTP to self-hosted Node server wrapping **Camoufox** (Firefox + C++ fingerprint spoofing) | Local Docker/`make up` |
| **`/browser connect` (local CDP)** | CDP to `http://127.0.0.1:9222` or `ws://…` | Your Chrome/Brave/Edge (or auto-launched) |
| **Local agent-browser** | CLI-driven local Chromium | Hermes/agent-browser |

**`/browser connect` topology:**
- Interactive CLI slash command only (not via Telegram/WebUI).
- Auto-launch Chromium-family with `--remote-debugging-port=9222` if nothing listens.
- Docs strongly recommend a **dedicated `--user-data-dir`** (e.g. `~/.hermes/chrome-debug`) so a second process actually opens the debug port instead of attaching to a normal non-debug Chrome.
- When connected, all high-level browser tools hit **your live instance**.
- Optional raw escape hatch: `browser_cdp` when a CDP endpoint is reachable at session start.

**CDP supervisor (important resilience detail):**  
Hermes runs a **persistent CDP WebSocket per task** that subscribes to Page/Runtime/Target events — used for native dialog detection, frame trees (incl. OOPIF), and faster `browser_console` evaluation when the supervisor is active.

```
Hermes agent tools
        │
   provider router
        │
   ┌────┼────────────┬──────────────┬─────────────┐
   ▼    ▼            ▼              ▼             ▼
Cloud  Camofox    agent-browser   /browser connect
APIs   :9377 REST  local Chromium  CDP :9222 (+ supervisor WS)
```

**Confidence:** [established] from Hermes official browser docs.

---

## 2. Resilience — what actually keeps the session from dropping

This is the core of the brief. **Neither system claims a magic WebSocket that never dies.** They reduce drop impact by (a) **browser process longevity**, (b) **disk profile / cookie persistence**, (c) **agent-level recovery loops**, and (d) **provider keep-alive** for cloud.

### 2.1 OpenClaw

| Failure class | What they do | Source quality |
|---|---|---|
| **CDP/WebSocket connectivity** | Managed browser is a **long-lived process** launched/stopped by control service (`browser start` / `stop`), not a fresh browser per tool call. Attach-only/remote: `stop` closes **control session + Playwright overrides**, not the remote browser. | Official [established] |
| **Heartbeat / auto-reconnect at CDP layer** | **Not documented as a first-class “ping/pong reattach to same targetId” subsystem** in public browser docs. Recovery is operational: `doctor`, restart gateway, re-attach extension, re-`start` managed profile. Secondary blog claims “persistent WebSocket rather than HTTP polling” as CDP basics — true of CDP generally, not a unique OpenClaw reconnect protocol. | Official gaps [emerging]; secondary [low-authority] |
| **Tab/target crash or navigation** | **Agent skill loop**: status/tabs → label tabs → snapshot before act → **resnapshot after UI change** → **recover stale refs once** → treat login/2FA/captcha as **manual**. Refs are page-state scoped (invalidate on navigation). | Official [established] |
| **Navigation detaches extension CDP** | Documented community bug: navigate → `Frame has been detached` / `Target page… closed` → tab lost → 20s control timeouts over remote SSH/Tailscale. Issue closed with fix PR `#30612` (Feb 2026). | GitHub issue [established that it happens]; fix [emerging] |
| **Login survival after reconnect** | **Managed profile user-data-dir on disk** (cookies/storage for *that* profile). macOS: **cookie copy** from system Chrome-family profile into managed profile (**cookies only**; **not** localStorage/IndexedDB). Extension/`user` profiles use **real browser sessions** already on disk. | Official [established] |
| **Long-lived server process** | Yes for managed: Gateway/plugin control service + browser process. Gateway itself is OS-supervised (launchd/systemd); macOS has known **sleep / crash-burst / KeepAlive** issues with separate mitigations. | Official troubleshooting [established] |
| **Tab ownership after Gateway restart** | Durable SQLite ownership for tabs with stable native CDP target + browser identity; Chrome MCP handles are **process-local** and weaker after cold restart. | Official [established] |
| **Extension disconnect fail-closed** | Side-panel copilot: if Gateway drops mid-run, extension keeps custody of run ID; on reconnect **aborts unresolved run** before re-enabling panel (no silent continued actions). | Official [established] |

**What keeps login from becoming “logged out after reattach”:**  
Reattach to the **same Chromium profile directory** (or the same real Chrome). OpenClaw does not primarily dump Playwright `storageState` JSON; persistence is **browser profile on disk** (+ optional cookie import).

**What does *not* appear in docs:**  
A continuous CDP heartbeat that auto-rebinds to the same `targetId` after `Target.detachedFromTarget` without agent/tool retry. If that exists in code, it is **not** clearly documented — mark **[unverified / no source found]** for production planning.

---

### 2.2 Hermes

| Failure class | What they do | Source quality |
|---|---|---|
| **Per-task session isolation** | Cloud: **each task gets its own session**; inactive sessions cleaned (default **~2 min** inactivity; background thread every **30s**); emergency cleanup on process exit. | Official [established] |
| **Cloud reconnect** | Browserbase **`BROWSERBASE_KEEP_ALIVE`** (default on, paid; falls back if plan lacks feature) for “session reconnection after network hiccups.” | Official [established] |
| **Local CDP durability** | Dedicated `user-data-dir` + long-running Chrome on **9222**; tools operate on live instance; optional `browser.cdp_url` for always-on CDP without re-issuing `/browser connect` (feature request marked implemented on main). | Official + issue [established / emerging] |
| **CDP supervisor** | One persistent WS per task for dialogs/frame trees; evaluation can reuse supervisor WS (lower latency). Dialogs: **`must_respond`** with safety auto-dismiss after `dialog_timeout_s` (default **300s**). | Official [established] |
| **Camofox login survival** | `browser.camofox.managed_persistence: true` → deterministic **profile-scoped `userId`**, skip destructive cleanup; **server** must map userId → Firefox profile dir. Wrong YAML path → silent ephemeral identity. State: Hermes `~/.hermes/browser_auth/camofox/` + Camofox server volume. | Official [established] |
| **Headed local mode** | `browser.headed: true` → visible window + **skip per-turn cleanup** so login stays warm across conversation; still reaped after `inactivity_timeout` (default **120s**). | Official [established] |
| **External tab adoption** | `adopt_existing_tab`: GET `/tabs?userId=` once; **if external app closes tab mid-run, next tool call errors** — Hermes does **not** re-poll every call. | Official [established] |
| **Cloud TTL** | Firecrawl `FIRECRAWL_BROWSER_TTL` default **300s** (configurable). Provider plan TTLs apply. | Official [established] |

**Security note (stability-adjacent):** GitHub issue claims `/browser connect` exposes an **unauthenticated local CDP plane** to same-host processes (P2 security). Treat local CDP as **root-equivalent for that browser profile**. [single-source issue] [emerging]

---

### 2.3 Comparative resilience insight (for your failure mode)

Your vendor channel fails as: **MCP session drops**, **classifier blocks JS/select**, **site logout after rate limits**.

| Mechanism | OpenClaw default strength | Hermes default strength | Vendor live-tab MCP |
|---|---|---|---|
| Browser process longevity | Strong (managed) | Medium (task sessions) / Strong (CDP connect + headed) | Weak if MCP/extension session is short-lived |
| Login on disk | Strong (profile dir / real Chrome) | Strong if Camofox persistence or dedicated CDP profile | Strong *while* same browser stays signed in |
| Mid-task tool channel drop | Gateway still has browser; agent re-tools | Supervisor per task; cloud TTL | Channel drop = task stop |
| Safety classifier on tools | Local tools; `evaluateEnabled` toggle | Local/cloud tools; optional `restrict_evaluate` | Vendor classifier can block JS/select |

**Core transferrable idea:** **Decouple “browser lifetime” from “agent/MCP lifetime.”** Keep Chrome (or Camofox) as a supervised long-lived process with a fixed profile; make the agent a **reattachable CDP client**.

**Confidence:** [established] for documented behaviors; [speculative] that this fully fixes Claude Code’s vendor classifier (that is a product policy layer, not a CDP reliability layer).

---

## 3. Self-registration / form-filling / login guardrails

### 3.1 Hermes “Agent Self-Registration” — what it actually is

**Not** “agent invents a Gmail account and fills arbitrary site signups.”

It is **Browser Use Cloud account provisioning** so the agent can obtain a `bu_…` API key without a human opening the dashboard:

1. `POST https://api.browser-use.com/cloud/signup` → `challenge_id` + math `challenge_text`
2. Agent solves math (answer as string with two decimals)
3. `POST …/signup/verify` → `api_key`
4. Optional claim URL for human dashboard later (1 hour)

Then Hermes stores `BROWSER_USE_API_KEY` / `hermes config set …`.

**Confidence:** [established]

### 3.2 Filling forms / site logins — both frameworks

| Concern | OpenClaw | Hermes |
|---|---|---|
| Default behavior | Agent **can** click/type/select; form fill is normal tool use | Same via `browser_type` / click / navigate |
| Explicit refusal model | **Not** a hard “agent may not sign up” gate in browser docs | No global “no signups” gate in browser docs |
| Human-in-the-loop | Skill: **report** login/2FA/captcha/camera as **manual**, don’t guess; extension tab group consent; Chrome MCP Allow dialog; side-panel device approval | Dialog `must_respond` (default); Camofox **VNC** so human solves captcha then agent continues (community practice); headed mode for manual intervention |
| Hard technical guardrails | `evaluateEnabled: false` disables arbitrary JS; **SSRF policy** fail-closed by default; loopback control + gateway auth; extension only sees tab group | Private URL SSRF block / hybrid local sidecar; `browser.restrict_evaluate`; cloud never sees private URLs by default |
| Site ToS / bans | Out of product scope; managed isolation reduces personal-account blast radius | Stealth backends exist; still ToS risk |

Claude Code’s own Chrome integration **pauses and asks the human** on login pages / CAPTCHAs — a product-level HITL, not CDP.

**Confidence:** [established] for documented gates; [emerging] for “agents freely create accounts on third-party sites” — possible technically, not specially productized as safe self-registration.

---

## 4. Anti-detection vs stability trade-off

| Stack | Default | When anti-detect | Stability / ban trade-off |
|---|---|---|---|
| **OpenClaw** | Plain Chromium CDP/Playwright (managed) or real Chrome (extension/MCP) | Remote CDP to Browserbase/Notte/Browserless | Real Chrome = best “not a bot” for logged-in personal workflows, **highest account risk** if agent misbehaves; managed = safer isolation, more bot-like |
| **Hermes** | Local Chromium / CDP / cloud depending on config; **`CAMOFOX_URL` overrides** to Camofox | Camofox (Firefox C++ spoof), Browserbase stealth/proxies/advanced stealth | Community: Camofox wins against cookie walls/bot checks vs plain Chromium; VNC for captchas. Cost: Firefox fingerprint ≠ Chrome; more moving parts (Docker). Reddit users report night-and-day fewer blocks. [low-authority] |
| **Plain CDP attach** | Highest stability for **session identity** (cookies in real profile) | Weak stealth | Sites may still rate-limit/log out *accounts*; automation banner on Chrome remote debug may flag some sites |
| **Managed isolated** | Fresh/isolated profile | Medium | Best for scrapes/tests; worst for “already logged into ChatGPT in my daily browser” unless cookie import / manual login once |

**When attach-to-real vs managed (both ecosystems agree in practice):**
- **Real/attach:** SSO, 2FA already done, “drive my logged-in session.”
- **Managed/isolated:** untrusted automation, parallel jobs, no personal cookie blast radius.
- OpenClaw encodes this as default `openclaw` + prefer `chrome` when away + `user` when at desk.

**Does anti-detect hurt session stability?**  
Documented: Camofox persistence depends on **server honoring userId** — misconfig loses logins every session (stability of *identity*, not WS). Browserbase free tier may force keepAlive off — *cloud* session durability drops. [established]

---

## 5. Failure modes (docs + field) and mitigations

### OpenClaw (documented / frequently reported)

| Symptom | Likely cause | Mitigation |
|---|---|---|
| Control service timeout 20s | Gateway/relay down, proxy env routing CDP through HTTP proxy | `gateway status`, `NO_PROXY=localhost,127.0.0.1`, `doctor` |
| Profile confusion (`chrome` vs `openclaw`) | Extension not attached / wrong defaultProfile | Attach tab group or switch managed default |
| Playwright unavailable | Build/deps | Install Playwright / update gateway; ARIA-only degraded mode |
| CDP websocket not reachable after start | Launch/port conflict | Dedicated ports 18800+, fix executablePath, noSandbox in Docker |
| Extension: tab listed, navigate detaches | Relay navigation bug (remote tunnels) | Update past fix PR; prefer managed for remote gateways |
| `Target.attachToBrowserTarget: Not allowed` | Extension not truly attached / internal Chrome pages | Re-click toolbar, use normal https tabs |
| attachOnly not reachable | External CDP dead | Start Browserless/Chrome with debug port first |
| Stale refs / “can’t see tab” | Navigation without resnapshot; wrong profile | Skill loop: resnapshot; label tabs |
| macOS gateway “dead until touch dashboard” | Sleep / ENETDOWN / launchd respawn gate | pmset, upgrade, external kickstart watchdog |
| ZeroPath Jan 2026 relay loopback bypass research | Misbind / network config | Keep loopback; patch; firewall | [single-source secondary blog] |

### Hermes (documented / field)

| Symptom | Likely cause | Mitigation |
|---|---|---|
| Logins lost every session | `managed_persistence` wrong path or Camofox ephemeral | Nested `browser.camofox.managed_persistence: true` + volume |
| Cloud session dies mid-task | TTL / inactivity 2 min / plan | Raise TTL; keepAlive; use local CDP for long tasks |
| `/browser connect` no port 9222 | Second Chrome window on non-debug process | Dedicated `--user-data-dir` |
| WSL2 + Windows Chrome CDP pain | Endpoint not reachable from WSL | Prefer Windows-side chrome-devtools-mcp |
| Dialogs hang page | No dialog tool / Camofox no CDP dialogs | `browser_dialog`; Camofox lacks pending_dialogs path |
| Popups/cookie walls | Detection / banners | Camofox + VNC human assist (community) |
| Unauthenticated local CDP | By design of remote debugging | Separate profile; firewall; don’t share host with untrusted code |
| Provider registry bugs (Camofox missing) | Setup tooling bugs | Track GitHub issues; set env URL directly |

**Confidence:** [established] for official troubleshooting; [emerging] for community Camofox superiority claims.

---

## 6. Transfer to our stack (Claude Code)

### 6.1 Can we replace vendor live-tab MCP with our own CDP/Playwright MCP?

**Yes, technically and officially supported paths already exist.**

| Path | How it works | Fit for Claude Code |
|---|---|---|
| **Official `chrome-devtools-mcp`** | Google’s MCP; modes: own profile, `--browserUrl=http://127.0.0.1:9222`, or `--autoConnect` (Chrome ≥144, remote-debugging enable + Allow dialog) | First-class; Claude Code install docs exist |
| **Claude in Chrome (vendor extension)** | Extension automation; shared logins; pauses on login/CAPTCHA | Current unreliable channel for multi-minute agent runs |
| **Custom Playwright MCP** | `chromium.launch_persistent_context(user_data_dir)` or `connect_over_cdp` | Full control; we own reconnect/heartbeat |
| **Browserbase/remote CDP MCP** | `wss://…` connect | Stealth + isolation; cost; latency |

Chrome DevTools MCP autoConnect explicitly solves “reuse existing signed-in session / active debugging session” (Dec 2025 Chrome for Developers).

### 6.2 Option comparison (your four options)

| Option | Session durability | Login durability | Classifier / JS block risk | Ban / ToS risk | Ops cost | Best for |
|---|---|---|---|---|---|---|
| **(a) Vendor live-tab MCP** | Weak (MCP/extension drops) | Good while same Chrome stays logged in | **High** (vendor safety on JS/select) | Medium (acts as you) | Low | Short interactive assist |
| **(b) Own CDP-attach to persisted-profile Chrome** | **Strong** if Chrome supervised | **Strong** (profile dir) | **Low** (local MCP) | High if real personal profile; medium if dedicated agent profile | Medium | Authenticated multi-step (ChatGPT, dashboards) |
| **(c) Playwright + storageState** | Strong within process; restart needs reload state | Good if storageState + optional persistent context | Low | Medium (automation fingerprints) | Medium | CI-like scripts, reproducible auth |
| **(d) Browserbase / remote CDP** | Strong within provider TTL/keepAlive | Provider session / optional persistence features | Low | Lower personal-account risk; provider ToS | $$ + network | Parallel scrapes, stealth, headless fleet |

### 6.3 Concrete “what it would look like” for Claude Code (OpenClaw/Hermes-inspired)

**Recommended blueprint (AK-47 version of their architecture):**

1. **Long-lived Chrome** (LaunchAgent/systemd or manual):
   ```bash
   Google Chrome \
     --remote-debugging-port=9222 \
     --user-data-dir="$HOME/.claude-browser/profile" \
     --no-first-run --no-default-browser-check
   ```
2. **MCP** (prefer official over reinventing):
   ```bash
   claude mcp add chrome-devtools --scope user -- \
     npx -y chrome-devtools-mcp@latest \
     --browserUrl=http://127.0.0.1:9222
   ```
   Or `--autoConnect` if you want daily Chrome (with Allow prompt).
3. **Agent skill rules** (copy OpenClaw’s recovery loop): status → snapshot → act → resnapshot after nav → one stale-ref recovery → **stop on 2FA/CAPTCHA** for human.
4. **Optional watchdog**: if `curl -sf http://127.0.0.1:9222/json/version` fails → restart Chrome with same user-data-dir (Hermes/OpenClaw “doctor + start” pattern).
5. **Never** put real daily-driver profile on open CDP without understanding: **any local process can drive that browser**.

**Risks:**
- **Safety classifier:** Local CDP MCP largely **bypasses** vendor browser-tool classifier (good for reliability, **you** own abuse risk).
- **ToS / bans:** Automating ChatGPT/Google/etc. can still lock accounts; dedicated profile contains blast radius.
- **CDP security:** Unauthenticated loopback debugging is powerful; multi-user machines are unsafe.
- **Does not fix site-side logout** from rate limits — only profile persistence + human re-login / cookie hygiene does.

### 6.4 Is it worth it vs keeping vendor channel?

| Keep vendor (a) if… | Build (b)/(c) if… |
|---|---|
| Browser use is rare, short, and HITL is fine | Multi-minute authenticated flows break weekly |
| You want Anthropic’s pause-on-CAPTCHA product safety | Classifier blocks `evaluate`/select mid-task |
| Zero ops appetite | You already run fleet-local Chrome/Firefox skills |

**Worth it?** **Yes for your stated failure mode**, if and only if you use a **dedicated agent profile** (not the primary personal profile) and keep vendor channel as fallback for “quick look.”

---

## Decision-oriented ending

### Named options

1. **Stay on vendor Claude-in-Chrome MCP** — zero build; accept detach/classifier pain.  
2. **Chrome DevTools MCP → dedicated profile on `:9222`** — OpenClaw/Hermes “managed + CDP attach” without rewriting tools.  
3. **Chrome DevTools MCP `--autoConnect` to daily Chrome** — best login reuse; Allow dialog; higher blast radius.  
4. **Custom Playwright MCP + `launch_persistent_context` / storageState** — max control, more code to maintain.  
5. **Browserbase (or Notte/Browserless) remote CDP MCP** — stealth/isolation; not a substitute for local logged-in ChatGPT unless provider supports durable auth.

### ONE recommendation

**Do (2): dedicated-profile Chrome + `chrome-devtools-mcp --browserUrl=http://127.0.0.1:9222`, supervised long-lived process, OpenClaw-style snapshot/stale-ref skill, vendor MCP demoted to secondary.**

- **Confidence:** [emerging] (architecture is [established]; payoff for *your* Claude Code pain is high-confidence by analogy, not A/B measured on your machines).  
- **Main assumption:** Most of your pain is **control-plane / MCP / classifier**, not sites hard-killing cookies every minute; a durable local CDP client will keep the same profile logged in across agent restarts.  
- **What would settle it:** 48h dogfood — 20 multi-step authenticated tasks; count: MCP disconnects, classifier blocks, forced re-logins vs baseline vendor channel.

### FOR / AGAINST map (epistemic neutrality)

| Claim | FOR | AGAINST | Settled by |
|---|---|---|---|
| “Local CDP MCP will stop mid-task detaches” | Browser outlives MCP process; OpenClaw/Hermes design center | MCP process itself can still crash; Chrome may still drop debug targets on nav (OpenClaw bug history) | Instrument disconnect rate |
| “Anti-detect (Camofox) is required” | Community bot-block reports | Your use case is often *already logged-in* real Chrome; stealth can be orthogonal | Failures are bot walls vs session drops |
| “Browserbase is enough alone” | Keep-alive, isolation, captcha tooling | Cost, TTL, private LAN hybrid needed; weak for “my local ChatGPT cookies” | Task mix audit |
| “Agent self-registration solves logins” | Only Browser Use API keys | Not third-party site signup safety | Read Browser Use signup docs (done) |

---

## Sources (all used)

| # | Source | Date / recency |
|---|---|---|
| 1 | [OpenClaw Browser (managed) docs](https://docs.openclaw.ai/tools/browser) | Crawled 2026-08-01 |
| 2 | [OpenClaw Chrome Extension docs](https://docs.openclaw.ai/tools/chrome-extension) | Crawled 2026-07-27 |
| 3 | [OpenClaw Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting) | Crawled 2026-08-01 |
| 4 | [Hermes Browser Automation docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/browser) | Crawled 2026-08-01 |
| 5 | [Browser Use — Hermes integration / Agent Self-Registration](https://docs.browser-use.com/cloud/tutorials/integrations/hermes-agent) | Crawled 2026-07-30 |
| 6 | [Browser Use Agent Sign Up REST flow](https://docs.browser-use.com/cloud/agent-signup) | Current as of fetch 2026-08-01 |
| 7 | [Chrome for Developers — DevTools MCP debug your browser session](https://developer.chrome.com/blog/chrome-devtools-mcp-debug-your-browser-session) | Published 2025-12-11 |
| 8 | [Chrome DevTools MCP GitHub (Claude Code install)](https://github.com/ChromeDevTools/chrome-devtools-mcp) | Crawled 2026-08-01 |
| 9 | [Claude Code Docs — Use Claude Code with Chrome](https://code.claude.com/docs/en/chrome) | Crawled 2026-08-01 |
| 10 | [OpenClaw issue #29431 — Browser Relay loses CDP on navigation](https://github.com/openclaw/openclaw/issues/29431) | Opened 2026-02-28 |
| 11 | [Hermes issue #5507 — `/browser connect` persistent profile / cdp_url](https://github.com/NousResearch/hermes-agent/issues/5507) | 2026-04-06 |
| 12 | Secondary: [laozhang.ai OpenClaw CDP modes guide](https://blog.laozhang.ai/en/posts/openclaw-browser-control) | 2026-03-05 [low-authority] |
| 13 | Secondary: [getopenclaw.ai browser automation troubleshooting](https://www.getopenclaw.ai/help/browser-automation-setup) | Crawled 2026-08-01 [low-authority] |
| 14 | Community: [r/hermesagent Camofox practice](https://www.reddit.com/r/hermesagent/comments/1v0i01i/how_are_you_guys_actually_getting_hermes_to_do/) | Crawled 2026-07-31 [low-authority] |

Claims without a real source above are marked **[no source found]** / **unverified** (notably: OpenClaw internal CDP heartbeat/re-attach state machine details).

---

**Bottom line in one sentence:** OpenClaw and Hermes keep sessions alive by treating the **browser + profile on disk as the durable server** and the agent as a **reattachable CDP client with snapshot/stale-ref recovery**—not by trusting a short-lived vendor “live tab” MCP; for Claude Code, the closest high-leverage port is **supervised Chrome on a dedicated profile + `chrome-devtools-mcp` over CDP**, with the vendor channel as backup.
