# DR26-08-14-MACANTON-01-2117 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-14-MACANTON-01-2117
vendor: OpenAI (Codex CLI)
door: cli-headless
rail: codex
cmd: /Users/anton/.npm-global/bin/codex

collected: 2026-08-15T04:26:23Z
took_sec: 356
exit: 0
sources_unique_urls: 61
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

# A. Executive Summary

Исследование актуально на 14 августа 2026 года.

1. **[established] Лучший общий browser rail для локальных macOS/Windows-машин сейчас: отдельный постоянный Chrome-профиль + Chrome DevTools MCP поверх CDP, но с собственным watchdog и восстановлением.** Chrome 136 запретил remote-debugging для стандартного каталога профиля, поэтому отдельный `--user-data-dir` уже не просто удобство, а обязательное условие для порта `9222`. ([Chrome, 17 марта 2025](https://developer.chrome.com/blog/remote-debugging-port), [Chromium profiles](https://www.chromium.org/developers/creating-and-using-profiles/))

2. **[established] Решение “один постоянный targetId” ошибочно.** CDP официально предоставляет события `targetCreated`, `targetDestroyed`, `targetInfoChanged` и `targetCrashed`; target ID надо считать краткоживущим дескриптором и повторно находить вкладку по устойчивой логической идентичности: URL/origin, opener, маркеру в заголовке или собственному nonce в DOM/storage. ([CDP Target domain, текущая документация](https://chromedevtools.github.io/devtools-protocol/tot/Target/))

3. **[emerging] Документированные строительные блоки для автоматического rebind существуют, но production-proven стандартного rebind-loop именно в chrome-devtools-mcp публично не найдено.** Puppeteer документирует событие `disconnected`, повторный `connect()` и target lifecycle; CDP документирует discovery events. Однако официальная документация chrome-devtools-mcp описывает диагностику и ручные обходы, а не полностью автоматическое восстановление вкладки после browser-level disconnect. ([Puppeteer events](https://pptr.dev/api/puppeteer.browserevent), [Puppeteer endpoint](https://pptr.dev/api/puppeteer.browser.wsendpoint), [Chrome DevTools MCP troubleshooting](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md))

4. **[established] Для unattended screen automation Windows с постоянно разблокированной локальной консольной сессией практичнее macOS и Wayland.** Но UAC secure desktop, блокировка станции и смена интерактивной сессии остаются жёсткими границами: обычный ввод туда не проходит. Microsoft прямо указывает, что на locked workstation/secure desktop input injection невозможен; `Ctrl+Alt+Del` нельзя синтезировать. ([Microsoft WinApp UI automation, июль 2026](https://learn.microsoft.com/en-nz/windows/apps/dev-tools/winapp-cli/ui-automation), [Microsoft UAC settings, май 2026](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/settings-and-configuration))

5. **[established] macOS пригодна после стабильного TCC-grant, но обновления и смена helper bundle могут ломать разрешения или внутреннюю проверку.** Есть свежий случай Claude Code, где Accessibility и Screen Recording были выданы, но новый per-version helper не был установлен, и инструмент продолжал отвечать `permission(s) not yet granted`. ([Claude Code #50735, 19 апреля 2026](https://github.com/anthropics/claude-code/issues/50735), [Apple Accessibility](https://support.apple.com/en-gb/guide/mac-help/-mh43185/mac), [Apple Screen Recording](https://support.apple.com/en-euro/guide/mac-help/mchld6aa7d23/mac))

6. **[established] Wayland уже технически умеет screen+input через XDG RemoteDesktop/ScreenCast, PipeWire и libei, включая persistent restore tokens. Но unattended viability зависит от compositor/portal backend и заранее сохранённого разрешения.** Это не универсальная замена X11: compositor вправе закрыть сессию, отфильтровать ввод или снова показать диалог. ([XDG RemoteDesktop](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html), [ScreenCast](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.ScreenCast.html), [InputCapture](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.InputCapture.html))

7. **[established] Cookie-copying следует убрать из основной архитектуры.** Windows Chrome 127 ввёл App-Bound Encryption; Chrome 136 затем закрыл remote-debugging стандартного профиля именно после роста краж cookies через CDP. На macOS ключ связан с Keychain, на Windows с user/app context, на Linux с GNOME Keyring/KWallet либо небезопасным `basic` fallback. ([Google Security Blog, 30 июля 2024](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html), [Chrome remote-debugging change, 17 марта 2025](https://developer.chrome.com/blog/remote-debugging-port), [Chromium Linux password storage](https://chromium.googlesource.com/chromium/src/%2B/fca37d0c4c6d4b9c5b6ce959f469c4f86179c9e3/docs/linux/password_storage.md))

8. **[established] Нет достоверной публичной “периодической таблицы полураспада” логинов Google/X/Facebook/ChatGPT/Gemini/Claude по rail и OS.** В документации Playwright прямо сказано лишь, что сохранённое состояние со временем истекает и его надо обновлять; remote-browser vendors обещают сохранение профиля, но не публикуют независимые cross-site lifetime distributions. ([Playwright authentication](https://playwright.dev/docs/auth), [Browserbase contexts](https://www.browserbase.com/templates/context), [Hyperbrowser profiles](https://www.hyperbrowser.ai/docs/sessions/profiles), [Steel profiles, 13 октября 2025](https://steel.dev/blog/profiles))

**Итоговая рекомендация: [established] оставить выбранную архитектуру dedicated Chrome + `chrome-devtools-mcp --browser-url`, но добавить внешний supervisor, который различает browser death, transport death и target death. Для экрана: Windows UI Automation + screenshot как основной rail, macOS Accessibility/ScreenCaptureKit как резерв для приложений вне браузера, Linux VPS без GUI screen rail — browser/API only. Главная предпосылка: машины можно держать в стабильной, разблокированной пользовательской сессии.**

---

# B. Key Findings

## B1. Browser rail

### CDP нестабилен не одним, а тремя разными способами

**[established] Browser-level death:** Chrome завершился, упал или WebSocket оборвался. Puppeteer выдаёт `disconnected`; на Windows browser-use зафиксировал `httpx.ReadError`, `Page crashed` и silent WebSocket disconnect, причём автор отчёта локализовал нестабильность в CDP transport, поскольку прямой Playwright `launch()` проходил те же действия стабильно. Это один подробный RCA, поэтому вывод о сравнительной стабильности помечается **[single-source]**. ([browser-use #5048, июнь 2026](https://github.com/browser-use/browser-use/issues/5048))

**[established] Target-level death:** браузер жив, но вкладка закрылась, была заменена navigation/process swap или renderer crashed. CDP имеет отдельные события `targetDestroyed` и `targetCrashed`; сохранённый target ID после этого использовать нельзя. ([CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/))

**[emerging] Target-level suspension:** соединение и target существуют, но background tab discarded/frozen и не отвечает на `Network.enable`. В issue chrome-devtools-mcp browser-level команды оставались рабочими, а `Target.activateTarget` немедленно оживлял вкладку. ([chrome-devtools-mcp #1230, 24 марта 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1230))

Отсюда watchdog должен проверять не просто “порт 9222 отвечает”, а три уровня:

- `GET /json/version` или `Browser.getVersion`;
- `Target.getTargets` и наличие логической рабочей вкладки;
- дешёвую page-level команду, например `Runtime.evaluate("1")` или свежий snapshot.

### Подтверждение/опровержение гипотезы о rebind

**Вердикт: частично подтверждено.**

- **[established] Механизм обнаружения мёртвых и новых targets документирован:** `setDiscoverTargets`, `targetCreated`, `targetDestroyed`, `targetCrashed`. ([CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/))
- **[established] Переподключение к живому browser endpoint документировано Puppeteer.** ([Puppeteer wsEndpoint](https://pptr.dev/api/puppeteer.browser.wsendpoint))
- **[speculative] Готовый production-proven heartbeat+automatic semantic rebind в официальном chrome-devtools-mcp не найден.** Официальный troubleshooting предлагает устранение конфликтов, повторный запуск и `--browser-url`, но не обещает прозрачное восстановление потерянного pageId/targetId. ([Chrome DevTools MCP troubleshooting](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md))

Следовательно, правильный паттерн надо реализовать снаружи MCP:

1. слушать disconnect/target lifecycle;
2. инвалидировать все element/frame refs;
3. переполучать `/json/version`, если умер transport;
4. выполнять `Target.getTargets`;
5. выбирать вкладку по устойчивому ключу;
6. активировать target;
7. делать новый snapshot;
8. только затем повторять идемпотентное действие.

### “Только один debugger” — слишком грубая формулировка

**[established] CDP допускает несколько sessions/attachments, но конкретные MCP/Chrome attach paths конфликтуют.** Документация chrome-devtools-mcp при `--autoConnect` прямо советует убедиться, что к debugging port не подключается другой MCP/tool. Это не универсальное ограничение протокола “один debugger”, а ограничение/гонка конкретной интеграции и consent flow. ([Chrome DevTools MCP troubleshooting](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md))

У extension rail конфликт ещё выше уровнем: Claude Desktop и Claude Code регистрировали два native hosts для одного extension origin, после чего расширение выбирало Desktop host и CLI получал `Browser extension is not connected`. ([Claude Code #20887, 26 января 2026](https://github.com/anthropics/claude-code/issues/20887), [Claude Code #46766, 11 апреля 2026](https://github.com/anthropics/claude-code/issues/46766))

### Headed против headless

**[established] Headless удобнее для изоляции и серверов, но хуже для бан-чувствительных логинов.** Cloudflare официально предупреждает, что Selenium, Cypress и Playwright определяются Turnstile как automation, вызывая блокировки и непредсказуемые результаты. ([Cloudflare, 5 мая 2026](https://developers.cloudflare.com/turnstile/troubleshooting/testing/))

**[emerging] Контейнер не является сам по себе ни “stealth”, ни “anti-stealth”.** Исследование 2026 года сообщает, что 75% Chromium-headless-only блокировок в их эксперименте объяснялись HTTP/header signals, но JavaScript environment probing тоже широко распространён. Другая работа обнаружила, что все исследованные web agents отличимы от людей по совокупности network, HTTP и browser fingerprints, причём некоторые stealth-модификации повышали различимость. ([Detecting Bot Detection, июнь 2026](https://arxiv.org/abs/2606.14525), [Unmasking Web Agents, июнь 2026](https://arxiv.org/abs/2606.30119))

Практический вывод: контейнер улучшает lifecycle isolation и воспроизводимость, но datacenter IP, SwiftShader/GPU, fonts, locale, viewport и inconsistent fingerprint могут ухудшить trust. Каузальный cross-site benchmark container-vs-bare-metal для одинакового аккаунта **[no source found]**.

### Человек и агент в одной вкладке

**[established] Одновременное касание одной headed-сессии создаёт гонки focus, navigation и DOM state.** PyAutoGUI отправляет клавиши в окно, которое сейчас имеет focus; CDP сообщает изменения и уничтожение targets асинхронно. Поэтому человеческое переключение вкладки безопасно только если agent actions адресуются pageId/targetId, а screen-click rail на время человеческого управления блокируется. ([PyAutoGUI README](https://github.com/asweigart/pyautogui), [CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/))

## B2. Screen rail по ОС

### Windows

**Что лучше:** UI Automation может обращаться к семантическим элементам и часть read/inspect операций выполнять даже при locked session. **Что хуже:** mouse/keyboard injection требует interactive desktop; secure desktop и SAS не преодолеть обычным агентом. ([Microsoft WinApp UI automation, июль 2026](https://learn.microsoft.com/en-nz/windows/apps/dev-tools/winapp-cli/ui-automation))

Corporate Win11 добавляет policies, Defender/AppLocker и ограничения UIAccess. Разрешение UIAccess воздействовать на elevation prompts регулируется отдельной UAC policy; по умолчанию нельзя предполагать, что агент имеет такие права. ([Microsoft UAC settings, май 2026](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/settings-and-configuration))

**Вывод:** Windows — лучший экранный хаб, только если:

- локальная пользовательская сессия не блокируется;
- отключён sleep дисплея/системы;
- overnight workflow не требует UAC;
- automation запускается в той же interactive session, не как Session 0 service;
- RDP disconnect не уничтожает или не меняет display topology.

### macOS

Accessibility и Screen Recording выдаются конкретным исполняемым приложениям/bundles. Обновление, новый helper path или новая подпись могут привести к тому, что UI показывает разрешение старому principal, а новый процесс его не получает. Свежий Claude Code issue показывает именно per-version helper mismatch; issue Codex показывает crash сразу после grant, предположительно в propagation window, но root cause там предложен участником, а не подтверждён maintainer, поэтому **[single-source]**. ([Claude Code #50735, 19 апреля 2026](https://github.com/anthropics/claude-code/issues/50735), [Codex #23992, 22 мая 2026](https://github.com/openai/codex/issues/23992))

**Вывод:** macOS работает для unattended только после preflight после каждого обновления приложения:

- screenshot не чёрный;
- Accessibility click реально двигает target;
- helper bundle path соответствует текущей версии;
- экран не заблокирован;
- lid/sleep исключены.

### Linux X11

**[established, источник старше 12 месяцев]** X11 остаётся самым прямым Linux rail: screenshot и synthetic input доступны процессу с корректным `DISPLAY`/Xauthority. Цена — слабая изоляция между GUI-клиентами. OpenAdapt рекомендует X11 для 24/7 capture именно потому, что Wayland требует portal integration. ([OpenAdapt design, март 2026](https://github.com/OpenAdaptAI/openadapt-desktop/blob/main/DESIGN.md))

### Linux Wayland

Wayland больше не “абсолютно невозможно”: RemoteDesktop portal передаёт screenshot через PipeWire и поддерживает pointer/keyboard; `persist_mode=2` и restore tokens могут сохранять разрешение. Но портал “typically” показывает диалог, restore token одноразовый и обновляется при восстановлении, а compositor сохраняет контроль над сессией. ([XDG RemoteDesktop](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html), [XDG ScreenCast](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.ScreenCast.html))

**Вердикт по открытому вопросу:** **[emerging] viable для контролируемого GNOME/KDE deployment с проверенным portal backend; всё ещё плохой универсальный выбор для безлюдного агента.** `ydotool`/доступ к `/dev/uinput` обходят compositor на более низком уровне, но требуют привилегий и не решают screenshot/portal/session lifecycle целиком **[no source found для production fleet comparison]**.

## B3. Login/session durability

### Почему profile beats cookie copying

Полный профиль содержит не только cookies, но localStorage, IndexedDB, service-worker state, cache, extensions, passkeys и иногда sessionStorage. Playwright `storageState` покрывает cookies, localStorage и IndexedDB/passkey state, но sessionStorage требует отдельного кода. ([Playwright auth](https://playwright.dev/docs/auth))

Cookie database физически копировать ненадёжно:

- Chrome profile имеет singleton/process lock. Chromium показывает `DisplayProfileInUseError`; параллельный запуск одного каталога небезопасен. ([Chromium process singleton source](https://chromium.googlesource.com/chromium/chromium/%2B/refs/heads/main/chrome/browser/process_singleton_linux.cc))
- Windows App-Bound Encryption привязывает данные к приложению, а старый DPAPI — к пользовательскому контексту. ([Google Security Blog, 30 июля 2024](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html))
- macOS использует Keychain-backed key, Linux — GNOME Keyring/KWallet либо plaintext `basic` fallback. ([Chromium security FAQ](https://chromium.googlesource.com/experimental/chromium/src/%2Bshow/64.0.3282.20/docs/security/faq.md), [Linux password storage](https://chromium.googlesource.com/chromium/src/%2B/fca37d0c4c6d4b9c5b6ce959f469c4f86179c9e3/docs/linux/password_storage.md))
- Device Bound Session Credentials проектируются именно для того, чтобы stolen cookies без device-bound key перестали быть достаточными. ([Google DBSC, 2 апреля 2024](https://blog.google/chromium/fighting-cookie-theft-using-device/))

### Session half-life: честный ответ

Публичных достоверных значений вроде “Claude extension: медиана 23 дня; CDP: 11 дней” не найдено. Ни Google, ни X, Meta, OpenAI, Anthropic, Browserbase, Steel или Hyperbrowser не публикуют сопоставимый survival benchmark по одинаковым аккаунтам, IP и OS.

Доступны лишь механизмы:

- Playwright: saved auth надо периодически пересоздавать после expiry. ([Playwright auth](https://playwright.dev/docs/auth))
- Browserbase Contexts: сохраняют cookies/storage/tokens между browser sessions; live session имеет timeout до 21 600 секунд, а `keepAlive` сохраняет его после disconnect. ([Browserbase create session](https://docs.browserbase.com/reference/api/create-a-session), [Browserbase contexts](https://www.browserbase.com/templates/context))
- Hyperbrowser: сохраняет полный user-data-dir; изменения сохраняются только при `persistChanges:true` и корректном закрытии. ([Hyperbrowser profiles](https://www.hyperbrowser.ai/docs/sessions/profiles))
- Steel: profiles появились в октябре 2025; vendor заявляет cookies, storage, extensions, credentials и fingerprint persistence. Это vendor claim, поэтому **[single-source]**. ([Steel profiles, 13 октября 2025](https://steel.dev/blog/profiles))
- Anchor: default idle timeout 5 минут и max duration 180 минут; это runtime lifetime, не lifetime логина. ([Anchor session timeout](https://anchor-b3ec2715.mintlify.app/advanced/session-timeout))

**[established] Remote browser service сохраняет browser state, но не отменяет server-side expiry, token revocation, risk scoring, device binding или CAPTCHA.**

### Какой OS даёт меньше forced re-logins

**[speculative] Вероятнее всего, определяющими являются стабильность fingerprint/IP/device identity и отсутствие копирования state, а не “сила шифрования cookie store”.** Шифрование влияет на переносимость/извлечение cookies, но сайт видит сетевую и браузерную идентичность, а не то, Keychain или DPAPI расшифровал cookie локально. Cloudflare прямо использует browser/client signals; современные исследования выделяют network, HTTP и browser layers. ([Cloudflare Turnstile, май 2026](https://developers.cloudflare.com/turnstile/), [Unmasking Web Agents, июнь 2026](https://arxiv.org/abs/2606.30119))

Для одинаковых сайтов ожидаемый порядок:

1. постоянный headed Chrome на обычном residential/workstation IP;
2. постоянный профиль remote browser с закреплённым region/proxy/fingerprint;
3. свежий headed container;
4. свежий headless container/datacenter IP.

Но независимого controlled experiment, подтверждающего этот порядок для Google/X/Facebook/ChatGPT/Gemini/Claude, **[no source found]**.

### Agentic browsers

Comet/Atlas/Dia/Neon могут перенести агент внутрь настоящего пользовательского браузера и тем самым сохранить его native profile/session. Но они не могут отменить server-side expiry, account risk scoring, device binding или lock-screen/OS permissions. Публичных независимых overnight session-survival сравнений этих продуктов **[no source found]**.

Следовательно, они скорее **перемещают проблему** из “как подключиться к Chrome” в “насколько стабилен proprietary agent bridge и обновления браузера”.

---

# C. Contradictions & Debates

### “CDP хуже Playwright” против “CDP нужен для реального профиля”

Playwright официально предупреждает, что `connectOverCDP` имеет существенно меньшую fidelity, чем его собственный protocol, и что Playwright-функции могут ломаться, если Chrome запущен с другим набором аргументов. ([Playwright BrowserType](https://playwright.dev/docs/api/class-browsertype))

Сильнейший аргумент в пользу direct Playwright launch: он контролирует browser lifecycle и версии, использует проверенные launch args и меньше зависит от внешнего WebSocket. Это подтверждает Windows RCA browser-use, хотя это один репорт. ([browser-use #5048, июнь 2026](https://github.com/browser-use/browser-use/issues/5048))

Сильнейший аргумент за CDP attach: только он удобно подключает агента к долгоживущему headed browser profile с настоящими cookies, extensions и ручным fallback. Chrome DevTools MCP прямо поддерживает `--browser-url`, `--ws-endpoint` и Chrome 144+ `--autoConnect`. ([Chrome DevTools MCP README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md))

**Решение:** CDP для auth-sensitive реального профиля; direct Playwright protocol для изолированных тестов и сайтов без ценного login state.

### “Wayland уже готов” против “Wayland hard no”

За: официальные portals теперь поддерживают screenshot, pointer, keyboard, clipboard и persistent permission mode. ([XDG RemoteDesktop](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html))

Против: compositor сохраняет право закрывать/ограничивать session; restore token зависит от backend и может не восстановиться при изменении monitor/window topology. OpenAdapt продолжает считать это проблемой для 24/7 recording. ([XDG ScreenCast](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.ScreenCast.html), [OpenAdapt design](https://github.com/OpenAdaptAI/openadapt-desktop/blob/main/DESIGN.md))

**Решение:** Wayland “готов как контролируемая интеграция”, но не “готов как универсальный unattended desktop substrate”.

### “Stealth browser решает detection” против “stealth сам становится fingerprint”

Cloudflare официально видит обычные automation suites. ([Cloudflare testing](https://developers.cloudflare.com/turnstile/troubleshooting/testing/))

Некоторые новые patched browsers заявляют прохождение detection tests, но это self-reported vendor/open-source evidence. Например CloakBrowser заявляет 30/30 тестов; независимой cross-site валидации в источнике нет, поэтому **[low-authority] [single-source]**. ([CloakBrowser, июнь 2026](https://github.com/CloakHQ/cloakbrowser))

Исследование 2026 года, напротив, обнаруживает, что stealth может усиливать отличимость из-за несогласованных слоёв fingerprint. ([Unmasking Web Agents, июнь 2026](https://arxiv.org/abs/2606.30119))

**Решение:** использовать обычный стабильный browser identity, а не наращивать случайные stealth patches без собственного A/B измерения.

---

# D. Best Practices

1. **Один writer на профиль.** Никогда не запускайте два Chrome процесса на одном `user-data-dir`; второй агент подключается к уже работающему browser endpoint.

2. **Разделяйте три идентичности:** browser process, browser context/profile, page target. Не храните targetId между независимыми задачами.

3. **Heartbeat проверяет три слоя:** endpoint, target list, page-level response.

4. **Rebind по semantic key:** ожидаемый origin + нормализованный URL + opener/маркер, а не по порядковому номеру вкладки.

5. **После любого navigation, human takeover, popup или recovery — новый snapshot.** Старые DOM/frame/element refs недействительны.

6. **Один stale-ref retry только для действия, доказанно безопасного при повторе.** Submit/payment/post/delete повторно не нажимать без чтения итогового состояния.

7. **Профиль хранить целиком; cookies отдельно не переносить.** Для parallel read-only jobs клонировать profile snapshot только при закрытом Chrome и не сливать изменения назад побайтно.

8. **Chrome и MCP обновлять кольцами:** canary machine → overnight smoke → остальные. CDP tip-of-tree не гарантирует backwards compatibility. ([CDP overview](https://chromedevtools.github.io/devtools-protocol/index.html))

9. **Ограничить число вкладок.** chrome-devtools-mcp может будить/attach ко всем страницам; репорт с 35 вкладками показал 30–35% CPU у MCP, а 122 вкладки — более 700% total CPU. Это один подробный репорт, поэтому **[single-source]**. ([chrome-devtools-mcp #1921, май 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1921))

10. **Не держать browser rail и screen rail одновременно активными на одной вкладке.** Screen rail должен брать lease на foreground/display.

11. **Screen preflight перед ночью:** правильный monitor topology, DPI, focus, unlocked session, screenshot freshness, click round-trip.

12. **Auth canary до полезной работы:** открыть лёгкий authenticated endpoint и проверить конкретный logged-in marker. Empty page нельзя интерпретировать как logout без screenshot/network evidence.

13. **CAPTCHA/2FA/account warning — hard stop.** Cloudflare прямо указывает, что автоматизация может вызвать unpredictable challenges. ([Cloudflare testing](https://developers.cloudflare.com/turnstile/troubleshooting/testing/))

14. **Для remote services закрепить region/proxy/profile.** Нельзя менять географию и device fingerprint между последовательными запусками одного аккаунта без необходимости.

15. **Собирать собственную survival telemetry:** `site`, `OS`, `rail`, `profile_age`, `last_human_login`, `IP/region`, `auth_ok`, `challenge`, `forced_logout`. Это единственный способ ответить на вопрос о half-life для ваших аккаунтов.

---

# E. Failure Modes

## RAKE TABLE

| # | Rake | Trigger | OS | Симптом / точный текст | Root cause | Надёжная мера | Source + date |
|---:|---|---|---|---|---|---|---|
| 1 | Profile already in use | Второй browser на том же profile | All | `Browser is already in use... use --isolated` | Singleton/profile lock | Один Chrome process; остальные attach | [Codex #16085, 28 Mar 2026](https://github.com/openai/codex/issues/16085) |
| 2 | Default profile больше не debug-able через flags | Chrome 136+ | All Chrome | `ECONNREFUSED ...:9222` | Security hardening | Отдельный `--user-data-dir` | [Chrome, 17 Mar 2025](https://developer.chrome.com/blog/remote-debugging-port) |
| 3 | Target closed | Browser launch/crash/closed target | All | `Protocol error ... Target closed` | Process/target terminated | Перезапуск/reconnect, затем rediscovery | [MCP troubleshooting](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md) |
| 4 | Stale element/frame ref | Navigation, renderer/frame replacement | All | stale/missing node/frame | Snapshot привязан к старому document | Новый snapshot; максимум один safe retry | [CDP Target lifecycle](https://chromedevtools.github.io/devtools-protocol/tot/Target/) |
| 5 | Silent WebSocket disconnect | Idle/network/Windows IOCP | Windows чаще в репорте | hang, `httpx.ReadError`, no immediate disconnect | CDP TCP transport не сигнализирует вовремя | Ping timeout, kill/reconnect transport | [browser-use #5048, Jun 2026](https://github.com/browser-use/browser-use/issues/5048) |
| 6 | Frozen tab poisons enumeration | Chrome Memory Saver/discard | All | `ProtocolError: Network.enable timed out` | Suspended target не отвечает page-session init | `Target.activateTarget`; skip/recreate bad tab | [MCP #1230, 24 Mar 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1230) |
| 7 | Too many tabs wakes browser | MCP attach/list pages | All; measured macOS | Browser freezes/crashes, high CPU | MCP eagerly attaches/collects all pages | Dedicated profile, few tabs, idle disconnect | [MCP #1921, 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1921) |
| 8 | Extension native-host collision | Claude Desktop + Code | macOS reported | `Browser extension is not connected` | Extension connects to wrong native host | Один integration owner; disable competing host | [Claude #20887, 26 Jan 2026](https://github.com/anthropics/claude-code/issues/20887) |
| 9 | Stale auth metadata masquerades as bridge failure | Claude auth state stale | macOS reported | `Browser extension is not connected` | `subscriptionType:null` | Explicit auth refresh and diagnostics | [Claude #46766, 11 Apr 2026](https://github.com/anthropics/claude-code/issues/46766) |
| 10 | Desktop bridge registers too few tools | App update/OAuth regression | Windows reported | Only 3 browser tools available | computer-use MCP not spawned | Version pin/rollback; CDP fallback | [Claude #38783, 27 Mar 2026](https://github.com/anthropics/claude-code/issues/38783) |
| 11 | WSL launches Windows Chrome incorrectly | Auto executable detection | WSL2 | `read ECONNRESET`; `Target.setDiscoverTargets: Target closed` | Cross-OS pipe/process mismatch | Linux Chrome in WSL или Windows MCP + mirrored networking | [MCP #690, 17 Dec 2025](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/690) |
| 12 | WSL cannot reach host CDP | NAT/localhost split | WSL2 | connection refused/reset | `127.0.0.1` names different network namespace | Mirrored networking/explicit host endpoint | [MCP WSL guide](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md) |
| 13 | `autoConnect` default-profile ambiguity | Multiple profiles/Chrome 144+ consent | All | timeout / wrong profile | Default profile selection + permission flow | Explicit dedicated `--browser-url` | [MCP README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md) |
| 14 | Missing DevToolsActivePort | Default profile attach | macOS reported | `Could not find DevToolsActivePort...` | Old discovery path vs Chrome hardening | Dedicated profile + explicit port | [MCP #1830, 8 Apr 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1830) |
| 15 | Playwright-over-CDP feature divergence | External Chrome launch args differ | All | Advanced functions fail inconsistently | CDP lower fidelity than Playwright protocol | Direct Playwright launch where persistent real profile не нужен | [Playwright BrowserType](https://playwright.dev/docs/api/class-browsertype) |
| 16 | Remote session disconnect during connect | Lambda/cloud websocket | Linux cloud | `Target page...closed`; `Connection closed while reading`; timeout | Environment/provider transport interaction | Retry session creation; checkpoint state | [Playwright #36743, 22 Jul 2025](https://github.com/microsoft/playwright/issues/36743) |
| 17 | macOS TCC grant lost/misdirected | App/helper update | macOS | `permission(s) not yet granted` | New helper bundle/version not covered | Post-update permission preflight; stable signed helper | [Claude #50735, 19 Apr 2026](https://github.com/anthropics/claude-code/issues/50735) |
| 18 | Screen capture crashes after grant | TCC propagation/helper bug | macOS | `EXC_BAD_ACCESS` | Null/race after ScreenCapture permission [single-source] | Restart helper; verify screenshot before run | [Codex #23992, 22 May 2026](https://github.com/openai/codex/issues/23992) |
| 19 | Wrong screenshot clip | Zoom/Retina/DIP mismatch | macOS reported | Wrong region/size | CSS pixels passed where CDP expects DIP | Read device scale/visual viewport; normalize coordinates | [Codex #19429, 24 Apr 2026](https://github.com/openai/codex/issues/19429) |
| 20 | Retina/Windows scale click offset | HiDPI scaling | macOS, Windows | Coordinates off ×2 or ×1.25 | Screenshot pixels ≠ logical coordinates | Calibrate screenshot/logical pixel ratio | [PyAutoGUI #589, 2021, older source](https://github.com/asweigart/pyautogui/issues/589) |
| 21 | Second monitor invisible/wrong offsets | Multi-monitor topology | Windows/All | image not found; negative-coordinate errors | Primary-screen-only capture assumptions | Single fixed monitor или topology-aware capture | [PyAutoGUI #321, 2018, older source](https://github.com/asweigart/pyautogui/issues/321) |
| 22 | UAC/lock blocks input | Elevation or locked station | Windows | `no_interactive_desktop` | Secure desktop/SAS security boundary | Pre-elevate before run; never require UAC overnight | [Microsoft, Jul 2026](https://learn.microsoft.com/en-nz/windows/apps/dev-tools/winapp-cli/ui-automation) |
| 23 | Wayland screenshot/input denied | Portal/session unavailable | Linux Wayland | No capture/no synthetic input/consent dialog | Compositor isolation | XDG portals + restore token; otherwise dedicated X11 session | [XDG RemoteDesktop](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html) |
| 24 | Headless server has no display | SSH/container/VPS | Linux | `$DISPLAY` missing, screenshot empty | Нет compositor/X server | Browser DOM/CDP only; Xvfb only when screen semantics needed | [OpenAdapt design, 2026](https://github.com/OpenAdaptAI/openadapt-desktop/blob/main/DESIGN.md) |
| 25 | Automation challenge/forced login | Playwright/Selenium fingerprints | All | Turnstile challenge/block | Browser, client and network signals | Persistent headed identity; hard stop on challenge | [Cloudflare, 5 May 2026](https://developers.cloudflare.com/turnstile/troubleshooting/testing/) |
| 26 | Google rejects embedded/automated login | Automated or embedded browser | All | `This browser or app may not be secure` | Google risk/unsupported browser determination | Login once in normal supported Chrome; reuse profile | [Google community, Jan 2020, older low-authority reports](https://support.google.com/accounts/thread/22873505?hl=en&msgid=24479243) |
| 27 | App-Bound cookies cannot be copied | Copy/extract Windows Chrome state | Windows | Cookie decrypt/extraction fails | Key bound to Chrome app context | Persist original profile; do not copy cookie DB | [Google, 30 Jul 2024](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html) |
| 28 | Linux keyring unavailable | Headless/container startup | Linux | Saved credentials unavailable или `basic` fallback | No unlocked GNOME/KWallet daemon | Persistent keyring session or explicit risk-accepted basic store | [Chromium Linux storage](https://chromium.googlesource.com/chromium/src/%2B/fca37d0c4c6d4b9c5b6ce959f469c4f86179c9e3/docs/linux/password_storage.md) |
| 29 | `storageState` incomplete/expired | SessionStorage auth or server expiry | All | Browser loads but user logged out | SessionStorage omitted/token expired | Persist full profile; authenticated preflight; refresh workflow | [Playwright auth](https://playwright.dev/docs/auth) |
| 30 | Cloud profile changes not saved | Abrupt stop/flag omitted | Cloud | Next session logged out | `persistChanges` false or unsafe close | Explicit persist + graceful release + verification | [Hyperbrowser profiles](https://www.hyperbrowser.ai/docs/sessions/profiles) |
| 31 | Human steals focus | Concurrent manual use | All screen rails | Typing/click in wrong window | OS input follows foreground focus | Exclusive screen lease; CDP for background tabs | [PyAutoGUI README](https://github.com/asweigart/pyautogui) |
| 32 | Agent leaves zombie windows/processes | Cancellation/crash | All | Growing Chrome processes/RAM | Cleanup not executed | OS job/process group + TTL + orphan reap | [Gemini CLI #23025, 18 Mar 2026](https://github.com/google-gemini/gemini-cli/issues/23025) |

---

# F. Strategic Recommendations

## Per-OS decision matrix

| OS | Recommended browser rail | Screen rail | Session persistence | Watchdog | Biggest residual risk |
|---|---|---|---|---|---|
| Windows 11 hub | Dedicated headed Chrome profile + explicit `9222` + chrome-devtools-mcp `--browser-url`; launch Chrome outside WSL | Microsoft UI Automation first, screenshot+coordinate click only for inaccessible surfaces | Полный dedicated `user-data-dir`; один writer; no cookie copying | Process alive → `/json/version` → `Target.getTargets` → page marker → screenshot freshness; restart MCP before Chrome | UAC/lock/RDP session transition makes input impossible |
| Windows laptops | Same rail, but only while AC-powered and interactive session stays unlocked | UIA + calibrated single-monitor screenshot | Отдельный local profile per machine; не синхронизировать profile directory между машинами | Power/display/session-state + CDP three-level check | Sleep, docking/DPI/topology change |
| macOS laptop | Dedicated headed Chrome profile + explicit port; vendor extension only fallback | Accessibility + ScreenCaptureKit/CG capture after TCC preflight | Полный local profile tied to same user/Keychain | Проверка текущего helper bundle, TCC, fresh screenshot, CDP target | App auto-update/helper re-sign invalidates effective permissions |
| Linux VPS | Direct Playwright-managed Chromium for ordinary web work; remote browser service for auth-sensitive sites; CDP dedicated profile only if persistent GUI genuinely нужен | **No general screen rail**; DOM/CDP/API. If unavoidable, fixed X11/Xvfb session | Playwright state for low-risk sessions; full remote provider profile for durable auth | Browser protocol health + auth marker + resource/zombie limits | Datacenter/container fingerprint and CAPTCHA |
| Linux desktop X11 | Dedicated Chrome + CDP | X11 screenshot/input in fixed single-display session | Full local profile + unlocked keyring | `DISPLAY`, Xauthority, screenshot hash and CDP health | X session restart/logout |
| Linux desktop Wayland | Dedicated Chrome + CDP; avoid whole-desktop automation where possible | XDG RemoteDesktop+ScreenCast with tested persistent restore token | Full local profile + keyring | Portal session alive + PipeWire frames + libei input acknowledgement | Portal/compositor revokes session or prompts after restart |

### Remote-browser shortlist

**Browserbase:** наиболее зрелый выбор, если нужны contexts, live view, recordings и до 6 часов browser runtime. `keepAlive` отделяет disconnect клиента от жизни browser session. Риск: vendor cost, cloud IP/fingerprint и server-side auth expiry. ([Browserbase API](https://docs.browserbase.com/reference/api/create-a-session))

**Hyperbrowser:** хороший профильный primitive — полный saved user-data-dir, read-only reuse и явное `persistChanges`. Риск: сохранение гарантировано только при безопасном завершении; public independent lifetime data отсутствуют. ([Hyperbrowser profiles](https://www.hyperbrowser.ai/docs/sessions/profiles))

**Steel:** интересен self-hosted/cloud гибридом и persistent profiles; feature относительно новый, а claims о fingerprint/credentials исходят от vendor. ([Steel profiles, 13 октября 2025](https://steel.dev/blog/profiles), [Steel GitHub](https://github.com/steel-dev/steel-browser))

**Anchor:** headful browser positioning полезно для auth-sensitive flows, но default max session 3 часа недостаточен для цельного overnight run без настройки; state persistence и session runtime надо оценивать раздельно. ([Anchor timeout](https://anchor-b3ec2715.mintlify.app/advanced/session-timeout), [Anchor security](https://docs.anchorbrowser.io/security))

### Одна рекомендация

**[established] Выбрать локальный dedicated Chrome/CDP как основной browser rail на Windows и macOS; direct Playwright или Browserbase Context на Linux VPS; screen automation ограничить Windows UIA и macOS Accessibility, а на VPS не использовать.**

Главный trade-off: вы принимаете необходимость собственного supervisor/rebind layer в обмен на наиболее долговечную настоящую login identity.

Главный риск: не CDP сам по себе, а смешение одного профиля, вкладок и foreground screen между несколькими агентами и человеком.

---

# G. Action Plan

## Неделя 1: измерение, не миграция

1. Создать по одному dedicated profile на каждой машине; один profile directory никогда не синхронизировать файлово между ОС.
2. Ввести logical tab key: `site + account + workflow + nonce`.
3. Собрать browser watchdog с тремя состояниями:

   - `BROWSER_DOWN`;
   - `TRANSPORT_DOWN`;
   - `TARGET_DOWN_OR_FROZEN`.

4. Реализовать rebind-loop на CDP lifecycle events.
5. Записывать login telemetry по каждому сайту, не извлекая cookies.
6. Провести 20 overnight runs на каждом OS/rail.

## Неделя 2: fault injection

Проверить отдельно:

- закрытие рабочей вкладки;
- Chrome crash;
- MCP process kill;
- network loss на 60 секунд;
- Chrome auto-update/restart;
- discarded background tab;
- human tab switch;
- display sleep;
- Windows lock/UAC;
- macOS helper update/TCC;
- Wayland portal restart;
- expired session и CAPTCHA.

Success criterion: supervisor либо восстанавливает browser+target без повторного side effect, либо останавливается с точной категорией причины.

## Неделя 3–4: собственный session-survival benchmark

Для Google, X, Facebook, ChatGPT, Gemini и Claude.ai измерять ежедневно:

- auth marker;
- challenge;
- forced logout;
- refresh без ручного логина;
- rail/OS/IP/profile age;
- последний browser update;
- последний human intervention.

Публиковать Kaplan–Meier survival curves по site×rail только после достаточного числа запусков. Именно этот эксперимент ответит на вопросы о session half-life и лучшей ОС; веб-источники такого ответа сейчас не дают.

## Stop/go критерии

- **Оставить local CDP**, если ≥99% browser/target failures корректно классифицируются и ≥95% безопасно восстанавливаются до полезного действия.
- **Перевести конкретный сайт в remote browser**, если local machine lifecycle вызывает больше re-login, чем закреплённый cloud profile.
- **Запретить screen rail для конкретного workflow**, если ему нужен UAC, lock-screen recovery, Wayland consent или динамический multi-monitor.
- **Не внедрять stealth browser**, пока controlled A/B на ваших сайтах не покажет меньше challenges без роста account warnings.

---

# H. Sources

## Основные и официальные

- [Chrome: Changes to remote debugging switches, 17 марта 2025](https://developer.chrome.com/blog/remote-debugging-port)
- [Chrome DevTools Protocol: Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/)
- [Chrome DevTools Protocol overview](https://chromedevtools.github.io/devtools-protocol/index.html)
- [Chrome DevTools MCP README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md)
- [Chrome DevTools MCP troubleshooting](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md)
- [Puppeteer Browser events](https://pptr.dev/api/puppeteer.browserevent)
- [Puppeteer wsEndpoint](https://pptr.dev/api/puppeteer.browser.wsendpoint)
- [Playwright BrowserType/connectOverCDP](https://playwright.dev/docs/api/class-browsertype)
- [Playwright authentication](https://playwright.dev/docs/auth)
- [Chromium profiles](https://www.chromium.org/developers/creating-and-using-profiles/)
- [Chromium process singleton source](https://chromium.googlesource.com/chromium/chromium/%2B/refs/heads/main/chrome/browser/process_singleton_linux.cc)
- [Chromium Linux password storage](https://chromium.googlesource.com/chromium/src/%2B/fca37d0c4c6d4b9c5b6ce959f469c4f86179c9e3/docs/linux/password_storage.md)
- [Chromium security FAQ](https://chromium.googlesource.com/experimental/chromium/src/%2Bshow/64.0.3282.20/docs/security/faq.md)
- [Google: App-Bound Encryption, 30 июля 2024](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html)
- [Google: Device Bound Session Credentials, 2 апреля 2024](https://blog.google/chromium/fighting-cookie-theft-using-device/)
- [Microsoft UAC settings, май 2026](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/settings-and-configuration)
- [Microsoft WinApp UI automation, июль 2026](https://learn.microsoft.com/en-nz/windows/apps/dev-tools/winapp-cli/ui-automation)
- [Apple Accessibility permissions](https://support.apple.com/en-gb/guide/mac-help/-mh43185/mac)
- [Apple Screen Recording permissions](https://support.apple.com/en-euro/guide/mac-help/mchld6aa7d23/mac)
- [XDG RemoteDesktop portal](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html)
- [XDG ScreenCast portal](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.ScreenCast.html)
- [XDG InputCapture portal](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.InputCapture.html)
- [Cloudflare Turnstile testing, 5 мая 2026](https://developers.cloudflare.com/turnstile/troubleshooting/testing/)
- [Cloudflare Turnstile overview, май 2026](https://developers.cloudflare.com/turnstile/)

## Bug trackers и practitioner evidence

- [chrome-devtools-mcp #690, WSL2 ECONNRESET, 17 декабря 2025](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/690)
- [chrome-devtools-mcp #1230, frozen tabs, 24 марта 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1230)
- [chrome-devtools-mcp #1830, DevToolsActivePort, 8 апреля 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1830)
- [chrome-devtools-mcp #1921, many-tab CPU/crash, 2026](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1921)
- [browser-use #5048, Windows CDP instability, июнь 2026](https://github.com/browser-use/browser-use/issues/5048)
- [Playwright #36743, remote CDP/Lambda disconnects, 22 июля 2025](https://github.com/microsoft/playwright/issues/36743)
- [OpenAI Codex #16085, profile already in use, 28 марта 2026](https://github.com/openai/codex/issues/16085)
- [OpenAI Codex #19276, persistent login gap, 24 апреля 2026](https://github.com/openai/codex/issues/19276)
- [OpenAI Codex #19429, screenshot DIP bug, 24 апреля 2026](https://github.com/openai/codex/issues/19429)
- [OpenAI Codex #20556, macOS capture freeze, 1 мая 2026](https://github.com/openai/codex/issues/20556)
- [OpenAI Codex #23992, ScreenCapture helper crash, 22 мая 2026](https://github.com/openai/codex/issues/23992)
- [Claude Code #20887, native-host collision, 26 января 2026](https://github.com/anthropics/claude-code/issues/20887)
- [Claude Code #31971, extension not connected, 8 марта 2026](https://github.com/anthropics/claude-code/issues/31971)
- [Claude Code #38783, browser tools missing, 27 марта 2026](https://github.com/anthropics/claude-code/issues/38783)
- [Claude Code #46766, stale auth/native-host collision, 11 апреля 2026](https://github.com/anthropics/claude-code/issues/46766)
- [Claude Code #50735, TCC/helper mismatch, 19 апреля 2026](https://github.com/anthropics/claude-code/issues/50735)
- [Gemini CLI #23025, zombie headed windows, 18 марта 2026](https://github.com/google-gemini/gemini-cli/issues/23025)
- [PyAutoGUI README](https://github.com/asweigart/pyautogui)
- [PyAutoGUI #321, multi-monitor, 2018](https://github.com/asweigart/pyautogui/issues/321)
- [PyAutoGUI #589, HiDPI/Retina, 2021](https://github.com/asweigart/pyautogui/issues/589)
- [OpenAdapt Wayland/X11 design, 2026](https://github.com/OpenAdaptAI/openadapt-desktop/blob/main/DESIGN.md)

## Remote-browser services

- [Browserbase create-session API](https://docs.browserbase.com/reference/api/create-a-session)
- [Browserbase persistent contexts](https://www.browserbase.com/templates/context)
- [Hyperbrowser profiles](https://www.hyperbrowser.ai/docs/sessions/profiles)
- [Steel profiles, 13 октября 2025](https://steel.dev/blog/profiles)
- [Steel open-source browser](https://github.com/steel-dev/steel-browser)
- [Anchor session timeouts](https://anchor-b3ec2715.mintlify.app/advanced/session-timeout)
- [Anchor security/headful environments](https://docs.anchorbrowser.io/security)

## Academic research

- [BrowserGym Ecosystem, 6 декабря 2024](https://arxiv.org/abs/2412.05467)
- [OSWorld, 2024](https://arxiv.org/pdf/2404.07972)
- [WAREX, 28 сентября 2025](https://arxiv.org/abs/2510.03285)
- [BrowserArena, 2 октября 2025](https://arxiv.org/abs/2510.02418)
- [Detecting Bot Detection, июнь 2026](https://arxiv.org/abs/2606.14525)
- [Unmasking Web Agents with Multi-Layer Fingerprinting, июнь 2026](https://arxiv.org/abs/2606.30119)
- [FP-Inconsistent, 2024](https://arxiv.org/abs/2406.07647)

**Заключение:** [established] сегодня нет rail, который одновременно гарантирует бессрочный login, нулевой bot detection, автоматический recovery и полный unattended screen access на всех ОС. Наиболее сильная практическая комбинация — настоящий постоянный browser identity, CDP с внешним semantic rebind supervisor, семантический OS accessibility rail вместо координатных кликов и site-specific измерение survival.
