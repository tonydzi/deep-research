---
dr_id: DR26-07-17-HUB-03
title: "Reliable headless Firefox/Chromium automation with a persistent logged-in session on manag"
date: 2026-07-17
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-17-HUB-03): Reliable headless Firefox/Chromium automation with a persistent logged-in session on managed Windows 11

> Diagnoses why Playwright's bundled Firefox fails on a managed Windows 11 machine and recommends the lowest-maintenance stack for headless automation that reuses a real logged-in browser session.

## Ключевые выводы
- The Playwright Firefox crash is a Windows side-by-side (SxS) activation error citing a missing private assembly 'mozglue, version адрес узла' — a manifest/assembly-identity failure, not a generic 'Firefox won't start' problem.
- Most likely root cause (emerging, one single-source analogy): AppLocker/WDAC App Control blocking an unsigned executable running from a nonstandard user-space path (%LOCALAPPDATA%\ms-playwright\...), based on a documented Playwright GitHub issue where AppLocker blocked bundled Chromium in the same location — stock Firefox in Program Files still launches fine, supporting this theory.
- Second most likely cause: broken/stripped embedded manifest metadata inside the Playwright Firefox bundle's mozglue.dll or firefox.exe, verifiable by extracting resources with mt.exe.
- Recommended primary stack: the already-installed stock Firefox (C:\Program Files\Mozilla Firefox\firefox.exe) driven via Selenium + geckodriver (Mozilla's supported WebDriver/Marionette path), using firefoxOptions '-profile <path>' plus '-headless' to reuse a persistent logged-in session.
- Playwright Firefox is built on a patched Juggler-based Firefox fork, not a stable public interface; a single-source GitHub issue reports custom Firefox builds broken with Playwright since v1.61.0 — not recommended as the standard path.
- Fallback if a site behaves better in Chromium: Playwright Chromium or branded Chrome/Edge, but only with a separate non-default automation profile/user-data-dir — Playwright docs and Chrome 136+ now explicitly discourage/restrict automating the default profile (different encryption key for non-standard user-data-dir).
- Puppeteer is a viable secondary option (supports Firefox via WebDriver BiDi by default, persistent userDataDir) but adds a Node.js runtime/package surface with no advantage over Selenium for this case.
- Direct Marionette (--marionette) or raw Remote Agent (--remote-debugging-port) connections work but are more fragile and harder to maintain than Selenium+geckodriver.
- navigator.webdriver is exposed as true by both Firefox (when Marionette is enabled) and Chrome automation modes, giving WebDriver-class headless automation a larger anti-bot detection surface than a live browser-extension-driven tab (an inference, not a guaranteed ban outcome).
- Deterministic diagnostic order to confirm the actual cause before committing engineering time: sxstrace (fusion trace) → mt.exe manifest extraction → CodeIntegrity/AppLocker event log review (event 3077 = block, 3089 = signature detail).

## Рекомендации / решения
- Stop relying on Playwright's bundled Firefox on this machine; switch the primary automation path to Selenium + geckodriver driving the installed stock Firefox binary.
- Create one dedicated 'AutoFF' Firefox profile via 'firefox.exe -P', log into required sites manually once (including 2FA), then reuse it headlessly with '-profile <root dir> -headless'; never share it with daily interactive browsing to avoid profile lock contention.
- Before assuming Playwright Firefox is unsalvageable, run sxstrace + mt.exe manifest extraction + CodeIntegrity/AppLocker log checks to determine whether the failure is a policy block or a corrupted bundle manifest.
- If admin rights are available: export effective AppLocker policy (Get-AppLockerPolicy -Effective -Xml), review CodeIntegrity/AppLocker events, and if component-store corruption is implicated run DISM /Online /Cleanup-Image /RestoreHealth then sfc /scannow.
- If a target site works better in Chromium, use Playwright Chromium or branded Chrome/Edge but always with a separate non-default automation profile/user-data-dir, never the owner's real default profile.
- Avoid stealth/anti-detection patching (process injection, signature bypass) on a managed corporate Windows endpoint — it raises EDR/ToS risk faster than it lowers bot-detection risk; keep a conservative posture (real installed browser, real persistent profile, ordinary window size).

## Сущности
- **Люди:** —
- **Компании:** Mozilla, Microsoft, Google
- **Продукты/инструменты:** Playwright, Selenium, geckodriver, Firefox, Marionette, WebDriver BiDi, Chromium, Chrome, Edge, Puppeteer, AppLocker, Windows App Control for Business (WDAC), sxstrace, mt.exe, DISM, SFC, Selenium Manager

## Открытые вопросы
- Whether the actual root cause on this specific machine is AppLocker/WDAC policy blocking vs. a corrupted manifest inside the Playwright Firefox bundle — unconfirmed until sxstrace/mt.exe/event-log diagnostics are actually run.
- Whether custom/patched Firefox builds are broken with Playwright since v1.61.0 is based on a single GitHub issue and not independently verified.
- Whether Playwright Chromium would fail with the same or a different error class on this host is inferred from an unrelated single-source AppLocker field report, not tested directly.
- Site-specific ban/detection outcomes for headless WebDriver automation vs. a live browser-extension-driven tab remain unestablished (engineering judgment / inference only).

## Источник
- DR-ID `DR26-07-17-HUB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- chrome-autonomy-self-drive
- dr-fanout
- credential-store
- playwright-firefox-mozglue-sxs-failure
- selenium-geckodriver-dedicated-profile
- applocker-appcontrol-policy-blocking
- headless-browser-detection-navigator-webdriver
