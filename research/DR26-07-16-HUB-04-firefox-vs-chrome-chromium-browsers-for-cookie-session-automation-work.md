---
dr_id: DR26-07-16-HUB-04
title: "Firefox vs Chrome/Chromium browsers for cookie/session automation workarounds"
date: 2026-07-16
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-16-HUB-04): Firefox vs Chrome/Chromium browsers for cookie/session automation workarounds

> Which desktop browser should the fleet standardize on for external cookie/session extraction and automation now that Chrome App-Bound Encryption blocks the old approach.

## Ключевые выводы
- Chrome 127+ on Windows introduced App-Bound Encryption (ABE) for cookies, tying decryption to app identity so external processes can no longer trivially decrypt them; Edge (ApplicationBoundEncryptionEnabled, Windows ≥127, not supported on macOS) followed the same policy — this is the root cause of rookiepy/browser_cookie3 'could not decrypt' failures, not a bug.
- Firefox remains structurally favorable: cookies.sqlite is a plain file Mozilla itself points users to inspect with SQLite tools, geckodriver officially supports custom prepared profiles, and there is no Windows ABE-equivalent for Firefox.
- Chrome 136 also stopped honoring --remote-debugging-port/--pipe for the default profile — CDP/remote-debug extraction now requires a separate non-default user-data-dir.
- Public ABE bypass techniques exist (COM/elevation-service abuse, process hollowing, e.g. xaitax's Chrome-App-Bound-Encryption-Decryption, ZeroCrumb, claims up to Chrome 144/IElevator2) but are brittle, single-source-documented, resemble malware tooling, and risk tripping EDR/AV — not recommended as standing architecture.
- Other Chromium derivatives (Brave, Opera, Vivaldi, Arc) are not fleet-safe defaults: Brave documents OSCrypt clearly but Windows ABE-era extraction is unverified/mixed; Opera/Vivaldi/Arc lack current vendor documentation on cookie storage internals entirely.
- The supported non-extraction pattern for Chromium-only sites is a dedicated automation profile (separate user-data-dir), one-time manual login, then Playwright/Selenium persistent context or CDP attach — never the owner's default profile.
- Anti-bot detection is stack-based, not single-signal: navigator.webdriver (still real in Chrome --enable-automation/headless and Firefox Marionette), JA4-class TLS/network fingerprints (Cloudflare), and JS/canvas/WebGL/CDP artifact consistency all matter; residential IP + matched timezone/locale + headful real browser + warm long-lived profile + moderate cadence outweighs stealth-patch tooling.
- Stealth tooling ecosystem is churning, not stable: undetected-chromedriver's own community points to nodriver as successor; playwright-stealth's PyPI page self-describes as proof-of-concept only; patchright/camoufox are Chromium-only/Firefox-based anti-detect forks positioned as niche exceptions, not standards.
- Sessions still expire regardless of browser: Firefox cookies.sqlite stores only persistent cookies (session cookie completeness depends on flush timing / WAL-SHM journal files being copied alongside the DB); Google __Secure-1PSIDTS churn is a recurring community-reported pain point — periodic relogin/keepalive should be treated as normal maintenance, not failure.
- Extracted cookies are credentials (X's help center explicitly groups cookies/tokens/keys as compromise-sensitive) and must be stored like passwords, isolated from the owner's personal browsing profile; ToS boundaries (Google, X, Meta) tolerate personal-account automation but forbid impersonation, bypassing protective measures, and mass/inauthentic behavior.

## Рекомендации / решения
- Standardize on Firefox ESR as the default automation/cookie-export browser fleet-wide on Windows desktops/laptops and Macs, with one dedicated profile per major identity/service cluster (google-main, meta-main, x-main, telegram-main) rather than one mega-profile; use containers only for secondary multi-account splits within a service.
- Use Playwright Firefox persistent context or geckodriver with prepared profiles for driving; for a clean cookie export, close Firefox in an orderly way and read a copied cookies.sqlite (plus -wal/-shm if present) rather than reading a hot live DB.
- Keep dedicated Chrome/Edge automation profiles as fallback only for Chromium-only compatibility — separate non-default user-data-dir, connect via persistent context or CDP attach (Storage.getCookies, not deprecated Network.getAllCookies) — never disk-decrypt and never touch the owner's default profile.
- Do not standardize on Brave, Arc, Opera, Vivaldi, or ABE-bypass/stealth forks; treat them as exception-only tools for a specifically blocked site after proving business need, verified on a sacrificial machine first.
- Invest in 'looking like a normal user' (residential IP, matched locale/timezone, headful real browser channel, warm profile, moderate cadence) rather than chasing stealth-patch tooling, which is churning and unreliable.
- Build a keepalive routine that performs ordinary human-legible actions per service on a schedule and expect periodic manual relogin as normal maintenance, not an exception.
- Store extracted cookies as credentials in a protected/encrypted secret store, isolated from the owner's personal browsing identity; keep automation scoped to the account owner's own accounts to stay within Google/X/Meta ToS boundaries.

## Сущности
- **Люди:** —
- **Компании:** Google, Mozilla, Microsoft, Brave, Cloudflare, Meta, X (Twitter), Opera, Vivaldi, Arc (The Browser Company)
- **Продукты/инструменты:** Firefox ESR, geckodriver, Marionette, Playwright, Selenium, Chrome, Edge, Chrome App-Bound Encryption (ABE), ApplicationBoundEncryptionEnabled policy, Chrome DevTools Protocol (CDP), rookiepy, browser_cookie3, undetected-chromedriver, nodriver, playwright-stealth, patchright, camoufox, OSCrypt, Chrome-App-Bound-Encryption-Decryption (xaitax), ZeroCrumb, ABE-Decryption (Majanito), NotebookLM, Multi-Account Containers

## Открытые вопросы
- Whether Brave's current Windows builds are actually ABE-hardened or still using the older DPAPI model — vendor docs and bypass-tool claims conflict (single-source/emerging only).
- No current vendor documentation found for Opera, Vivaldi, or Arc on cookie-storage/encryption internals on Windows — practical extractability and automation suitability unverified.
- No official supported --disable-features= flag found for turning off Chrome ABE; only the managed enterprise policy escape hatch is documented.
- Exact signal weighting anti-bot systems (Google/Meta/X) place on navigator.webdriver vs JA4/TLS fingerprint vs behavioral signals is not publicly specified — inferred from vendor blog posts, not confirmed.
- Google's specific 'essential cookies' contract for session validity (e.g. __Secure-1PSIDTS) is not officially published — evidence for churn is single-source community anecdote (GitHub issue thread).
- Long-term sustainability of patchright/camoufox/nodriver as maintained tools is speculative given the rapid churn already seen in this space (undetected-chromedriver → nodriver).

## Источник
- DR-ID `DR26-07-16-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- browser-automation-fleet
- chrome-cookie-extraction
- app-bound-encryption
- anti-bot-detection
- session-cookie-management
- firefox-profiles-containers
- credential-store
- chrome-autonomy-self-drive
- insight-DR-DR26-07-17-HUB-03-reliable-headless-firefox-chromium-automation-with — directly related prior DR comparing Firefox vs Chrome for the same automation problem
