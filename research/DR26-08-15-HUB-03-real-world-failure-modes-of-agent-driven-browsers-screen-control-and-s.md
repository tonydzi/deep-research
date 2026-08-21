---
dr_id: DR26-08-15-HUB-03
title: "Real-world failure modes of agent-driven browsers, screen control, and session persistence"
date: 2026-08-15
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-15-HUB-03): Real-world failure modes of agent-driven browsers, screen control, and session persistence (2025-2026)

> How do coding agents (Codex, Claude Code, Playwright MCP, chrome-devtools-mcp, browser-use) actually break when driving an external browser or the OS screen, and what setup per OS survives unattended/overnight operation?

## Ключевые выводы
- Chrome 136+ (announced 17 Mar 2025) silently ignores --remote-debugging-port on the default user-data-dir; CDP only works with a dedicated non-default --user-data-dir, else port 9222 never opens and no error is shown -- the mechanism behind many 'silent logout' reports.
- No vendor documents a production auto-rebind of a dead CDP targetId; the only real-world workaround is a custom stdio proxy that remaps sessions by matching page title/url via GET /json (chrome-devtools-mcp issue #2366, open).
- Cookie-copying as a session strategy is structurally dead on Windows after Chrome 127 App-Bound Encryption (Jul 2024) + Chrome 136's dedicated-profile-uses-different-key change (Mar 2025); the surviving pattern is keeping the live browser process/profile alive, never extracting cookies.
- Screen/computer-use rail only works on an always-on, unlocked, headed desktop: macOS TCC permissions bind to the CLI's versioned binary path and reset on every Claude Code update (issues #30608, #41101); Windows UAC's secure desktop makes screenshots go black by design; Linux Wayland/GNOME portals re-prompt every session, making unattended remote control 'impossible' (GNOME issue #175); Claude's CLI computer-use is macOS-only, interactive-only, unavailable under -p.
- Browserbase-style 'Contexts' persist browser infrastructure indefinitely, but site-level cookies still expire (vendor cites 'e.g. 30 days'); no public, systematic session half-life dataset exists for Google/X/Facebook/ChatGPT/Gemini/Claude.ai under agent rails.
- Agentic consumer browsers (Comet, Atlas, Dia, Neon) inherit the user's logins but do not solve fleet-level durability -- and add prompt-injection risk (Brave demonstrated indirect injection in Comet, 20 Aug 2025; OpenAI's CISO called prompt injection 'unsolved', 28 Oct 2025).
- Datacenter/container IPs fail Cloudflare Turnstile almost regardless of stealth patching; a 2026 benchmark found stock Chrome 148 over plain CDP passed Turnstile while six patched stealth stacks failed the same page (single-site, low-authority result).
- 35 catalogued failure modes (rake table) span profile locking (SingletonLock/'browser already in use'), CDP WebSocket half-open hangs, MCP stdio transport drops requiring app restart, silent page retarget to about:blank/page-0, and orphan headless Chrome processes burning ~400% CPU for days after a task ends.
- The de facto best-practice stack -- dedicated headed Chrome + chrome-devtools-mcp --browserUrl + a custom process-level watchdog (poll /json/version + a cheap CDP call every 15-30s, kill/relaunch on failure, reselect tab by URL not index) -- is what serious operators assemble by hand in 2026; it is not a vendor-supported product.

## Рекомендации / решения
- On the always-on Windows hub, run one dedicated headed Chrome with --remote-debugging-port=9222 --user-data-dir=<agent-profile>, attach via chrome-devtools-mcp --browserUrl; never share that profile across machines, processes, or MCP clients.
- Never copy Chrome/Chromium cookie databases across OS or after Chrome 127+ (ABE) -- keep the live logged-in profile process alive instead of extracting cookies.
- Do not run computer-use/screen automation unattended overnight on any OS; only use it on a headed, unlocked, sleep-disabled desktop (Claude Desktop app, not CLI) with a human able to intervene.
- Build a channel watchdog that probes /json/version plus a cheap CDP call every 15-30s, kills and relaunches the browser+MCP on failure, and reselects the tab by URL allowlist -- do not attempt a targetId-rebind layer, since none exists in production.
- Keep the existing Firefox-for-anonymous-fetch / Chrome-for-ban-sensitive-social split; treat it as a detection-surface choice, not a session-lifetime fix.
- Treat 2FA, CAPTCHA, and Turnstile challenges as hard stops -- never auto-retry, since retries risk account lockout and 'new device' storms.
- Pin the Chrome/Chromium version (e.g. Chrome for Testing) alongside the driver to avoid silent breakage from Chrome auto-updates changing debug-port behavior again.
- Do not treat Comet/Atlas/Dia/Neon-style agentic browsers as the fleet's unattended session rail -- they solve single-human login inheritance, not multi-machine durability, and add prompt-injection exposure.
- Log a simple {site, rail, last_login, last_forced_relogin, reason} table weekly -- it is the only way to build the session half-life dataset that does not currently exist publicly.

## Сущности
- **Люди:** Dane Stuckey, Ian Paterson, Will Harris
- **Компании:** Google/Chrome, Anthropic, OpenAI, Microsoft, Browserbase, Steel, Hyperbrowser, Perplexity, Browser Company, Atlassian, Brave, Cloudflare, DataDome, Akamai, CyberArk, SpecterOps
- **Продукты/инструменты:** Chrome DevTools Protocol (CDP), chrome-devtools-mcp, Playwright MCP, browser-use, Claude Code computer-use, Claude-in-Chrome, Codex, agent-browser, Browserbase Contexts, OSWorld, WebArena, WebVoyager, App-Bound Encryption (ABE), TCC, UAC, xdg-desktop-portal (GNOME/Wayland), ydotool/wlrctl/grim, Comet, Atlas, Dia, Neon

## Открытые вопросы
- No public longitudinal dataset on session half-life per site/rail (Google, X, Facebook, ChatGPT, Gemini, Claude.ai) exists -- all cited numbers are product- or plan-specific, not measured.
- No public study of 'new device'/2FA prompt frequency under agent-driven browsers was found; the mechanism is inferred, not measured.
- Whether stealth-patched browsers or stock Chrome over plain CDP better evade Cloudflare Turnstile is unresolved -- the one benchmark found is single-site, single-day, and Turnstile is itself state-dependent.
- Whether wlroots+ydotool permission grants can be made to persist for truly unattended Linux screen automation is unconfirmed (emerging, not established).
- No vendor has committed to shipping an automatic dead-CDP-targetId rebind; unclear if/when this gap closes.

## Источник
- DR-ID `DR26-08-15-HUB-03` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-08-14-MACANTON-01-2117-browser-screen-rails-grok.md`

## Связано
- [[browser automation]]
- [[chrome-devtools-mcp]]
- [[computer-use]]
- [[session persistence]]
- [[bot detection]]
- [[agent fleet infrastructure]]
- [[chrome security changes]]
- [[screen-access-fleet-wide]]
