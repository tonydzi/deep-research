---
dr_id: DR26-08-14-MACANTON-01-2117
title: "браузерная и экранная рельса для Mac16 и флота — грабли агентных браузеров и экранного контроля"
date: 2026-08-14
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

> Синтез веера 4/6 сделан на рельсе **codex** (93 908 токенов на подписке OpenAI, не в контексте Claude) — канон [[dr-synthesis-via-codex-not-my-context]].
> Сырьё плеч: `_machine-bus/_dr/queue/DR26-08-14-MACANTON-01-2117/results/`.
> Потребитель: [[decision-2026-08-14-browser-and-screen-rail-mac16-fleet]].
> ⚠️ НЕ отменяет [[decision-2026-07-16-browser-automation-layer]] (Firefox-first, слой ДОСТАВАНИЯ кук) и ПОДТВЕРЖДАЕТ [[decision-2026-08-01-agent-browser-control-rail-DR26-08-01-MACANTON-12]] (слой УДЕРЖАНИЯ сессии).

# Синтез: браузерная и экранная рельса для Mac16 и флота

## 1. Шапка

- ✅ доказано: кворум собран, 4 из 6 рельс.
- ✅ доказано: вошли `chatgpt`, `claudeai`, `grok`, `glm`.
- ✅ доказано: не собраны `gemini` и `mistral`; по условию исследования это не блокер.
- ✅ доказано: сильные доказательные рельсы: `chatgpt` (61 уникальный URL), `claudeai` (69), `grok` (74 по условию задачи; frontmatter файла показывает 73).
- ✅ доказано: `glm` имеет 1 уникальный URL и proof-of-work не прошёл.
- ✅ доказано: из `glm` ниже не взяты самостоятельные цифры, версии или факты.
- ✅ доказано: `glm` использован только как второе мнение там, где вывод уже подтверждён сильной рельсой.
- ✅ доказано: предмет решения: Mac16, Windows-хаб, Linux-якорь и Windows-узлы коллег.
- ✅ доказано: практический выход: проект правок матрицы `/firefox` и §4.8 `CLAUDE.md`.
- 🤔 гипотеза: точная текущая формулировка строк `/firefox` в отчётах не приведена, поэтому раздел 7 задаёт смысловой diff, а не буквальный patch по строкам файла.

### Короткий вердикт

✅ доказано: правило «Firefox по максимуму» слишком широкое, если понимать его как основной браузер для любой агентной работы.

✅ доказано: исследования сильнее поддерживают такое разделение:

- Firefox AutoFF: дешёвый headless fetch, обычные сайты, изолированный постоянный профиль, задачи без строгой Chromium-зависимости.
- Dedicated headed Chrome/CfT: сложная DOM/CDP-автоматизация, Chromium-only, живой залогиненный профиль и workflows, где нужен Chrome password manager.
- Chrome на Windows-хабе с резидентным IP: бан-чувствительные действия от лица человека.
- Экранная рельса: только как запасной слой, не как ночная основа.
- Linux-якорь: browser/API rail; если нужен GUI, фиксированный X11/Xvfb, не общий Wayland screen control.

✅ доказано: Firefox не получил в трёх сильных отчётах достаточной сравнительной доказательной базы, чтобы объявить его устойчивее Chrome/CDP для сложного browser control.

✅ доказано: Chrome/CDP получил намного больше доказательств и намного больше найденных поломок одновременно.

✅ доказано: это не парадокс. Chrome лучше изучен и имеет нужные primitives, но требует отдельного профиля, одного владельца, сторожа и fail-closed восстановления.

## 2. Консенсус сильных рельс

### 2.1 Browser rail

✅ доказано: Chrome 136+ игнорирует remote-debugging flags на стандартном каталоге профиля. Для `:9222` нужен отдельный `--user-data-dir`; Chrome for Testing сохраняет automation-сценарий. Источники: [Chrome Developers](https://developer.chrome.com/blog/remote-debugging-port), `chatgpt B1`, `claudeai B3`, `grok B1`.

✅ доказано: подключаться CDP к ежедневному Chrome-профилю нельзя считать устойчивой архитектурой. Источники: [Chrome Developers](https://developer.chrome.com/blog/remote-debugging-port), `claudeai B2`, `grok B1`.

✅ доказано: отдельный постоянный профиль стал обязательным элементом, а не косметической изоляцией. Источники: [Chromium profiles](https://www.chromium.org/developers/creating-and-using-profiles/), [Chrome Developers](https://developer.chrome.com/blog/remote-debugging-port), `chatgpt A`, `grok B1`.

✅ доказано: один профиль нельзя одновременно открывать несколькими browser processes. Источники: [Playwright MCP](https://github.com/microsoft/playwright-mcp), [Puppeteer issue 4860](https://github.com/puppeteer/puppeteer/issues/4860), `chatgpt E1`, `claudeai B2`, `grok B4`.

✅ доказано: профиль нельзя файлово синхронизировать между машинами и ОС как живую сессию. Причины: profile locks и OS-bound encryption. Источники: [Google Security Blog](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html), [Chromium Linux password storage](https://chromium.googlesource.com/chromium/src/%2B/fca37d0c4c6d4b9c5b6ce959f469c4f86179c9e3/docs/linux/password_storage.md), `chatgpt B3`, `grok B4/B6`.

✅ доказано: CDP ломается как минимум на трёх разных уровнях: browser/transport, target/page и document/frame/element refs. Источники: [CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/), [browser-use 5048](https://github.com/browser-use/browser-use/issues/5048), `chatgpt B1`, `grok B2/B3`.

✅ доказано: проверка только `127.0.0.1:9222` недостаточна. Живой порт не доказывает живой WebSocket, target и рабочую страницу. Источники: [browser-use 4579](https://github.com/browser-use/browser-use/issues/4579), [chrome-devtools-mcp 1094](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1094), `chatgpt B1`, `grok B2`.

✅ доказано: готового публичного production-proven heartbeat + semantic rebind мёртвого `targetId` не найдено. Источники: [chrome-devtools-mcp 978](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978), [1094](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1094), [1152](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1152), `chatgpt B1`, `claudeai B1`, `grok B2`.

✅ доказано: target ID нельзя считать постоянной личностью вкладки. Источники: [CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/), [chrome-devtools-mcp 2304](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2304), `chatgpt B1`, `grok B2`.

✅ доказано: после navigation, crash или закрытия вкладки надо отбросить старые frame/element refs, получить новый snapshot и только затем продолжать. Источники: [CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/), `chatgpt B1/E`, `grok E30`.

✅ доказано: молчаливое переключение на первую доступную вкладку опаснее явного падения, потому что агент может действовать не там. Источник: [chrome-devtools-mcp 2304](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2304), `grok B2`.

✅ доказано: восстановление должно быть fail-closed по identity вкладки. Источники: `chatgpt B1`, `grok B2`, [chrome-devtools-mcp 2304](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2304).

✅ доказано: transport MCP может умереть независимо от Chrome/CDP. Источники: [Codex 13138](https://github.com/openai/codex/issues/13138), [Codex 11489](https://github.com/openai/codex/issues/11489), `grok B3`, `claudeai B1`.

✅ доказано: расширение в ежедневном Chrome не является устойчивой ночной рельсой. Возможны idle service worker, native-host collision, `EADDRINUSE` и необходимость reconnect/restart. Источники: [Claude Chrome docs](https://code.claude.com/docs/en/chrome), [Claude Code 20887](https://github.com/anthropics/claude-code/issues/20887), `chatgpt B1`, `grok B3`.

✅ доказано: человек и агент в одном browser/profile создают target churn, focus race и profile-lock race. Источники: [PyAutoGUI](https://github.com/asweigart/pyautogui), [chrome-devtools-mcp 2304](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2304), `chatgpt B1`, `grok B4`.

✅ доказано: screen-click и DOM/CDP нельзя одновременно считать владельцами одной вкладки. Источники: [PyAutoGUI](https://github.com/asweigart/pyautogui), `chatgpt D`, `grok B4`.

✅ доказано: большое число вкладок повышает риск загрузки, таймаутов и transport death. Источники: [chrome-devtools-mcp 978](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/978), [1921](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1921), `chatgpt D`, `claudeai B1`.

✅ доказано: Chrome auto-update является внешней причиной ночного отказа; automation browser надо обновлять кольцами и проверять после обновления. Источники: [Chrome for Testing](https://developer.chrome.com/blog/chrome-for-testing), `chatgpt D/F`, `claudeai B3`, `grok E31`.

✅ доказано: строгое сравнение «Playwright всегда стабильнее CDP» отчётами не доказано. Direct Playwright удобнее, когда настоящий постоянный Chrome-профиль не нужен; CDP нужен, когда важна живая browser identity. Источники: [Playwright BrowserType](https://playwright.dev/docs/api/class-browsertype), `chatgpt C`, `claudeai C`.

✅ доказано: Selenium/Playwright/CDP могут детектироваться сайтами; «headed» не делает автоматизацию невидимой. Источники: [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/troubleshooting/testing/), [Unmasking Web Agents](https://arxiv.org/abs/2606.30119), `chatgpt B1`, `grok B7`.

✅ доказано: контейнер улучшает воспроизводимость и изоляцию процесса, но сам по себе не уменьшает бан-риск. Источники: [Detecting Bot Detection](https://arxiv.org/abs/2606.14525), `chatgpt B1`, `grok B7`.

🤔 гипотеза: Firefox/Marionette менее заметен конкретно для наших аккаунтов, чем stock Chrome/CDP. В отчётах есть утверждения и ссылки на practitioner tooling, но нет controlled A/B на наших сайтах.

### 2.2 Screen rail

✅ доказано: экранная автоматизация зависит прежде всего от permission identity и interactive session, а не от качества модели. Источники: [Apple Accessibility](https://support.apple.com/en-gb/guide/mac-help/-mh43185/mac), [Microsoft UI automation](https://learn.microsoft.com/en-nz/windows/apps/dev-tools/winapp-cli/ui-automation), `claudeai B4`, `grok B5`.

✅ доказано: macOS TCC grants привязаны к приложению/helper identity; обновление или смена versioned path может оставить новый процесс без действующего разрешения. Источники: [Claude Code 30608](https://github.com/anthropics/claude-code/issues/30608), [41101](https://github.com/anthropics/claude-code/issues/41101), [50735](https://github.com/anthropics/claude-code/issues/50735), `chatgpt B2`, `grok B5`.

✅ доказано: наличие галочки в System Settings не доказывает, что текущий helper реально может сделать screenshot и click. Источники: [Claude Code 50735](https://github.com/anthropics/claude-code/issues/50735), [Codex 23992](https://github.com/openai/codex/issues/23992), `chatgpt B2`.

✅ доказано: для Mac нужен функциональный preflight: свежий screenshot и реальный Accessibility round-trip после обновления. Источники: `chatgpt B2/D`, `grok B5`.

✅ доказано: sleep, lid close и lock уничтожают практическую работоспособность screen rail. Источники: [Claude computer use docs](https://code.claude.com/docs/en/computer-use), `chatgpt B2`, `grok B5`.

✅ доказано: Retina/HiDPI и multi-monitor могут давать смещение координат и неверный crop. Источники: [Codex 19429](https://github.com/openai/codex/issues/19429), [PyAutoGUI 589](https://github.com/asweigart/pyautogui/issues/589), `chatgpt E`, `grok E21`.

✅ доказано: Windows UAC secure desktop является жёсткой границей для обычного screenshot/input. Источники: [Microsoft UAC](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/settings-and-configuration), `chatgpt B2`, `claudeai B4`, `grok B5`.

✅ доказано: locked Windows workstation и отсутствие interactive desktop блокируют ввод; RDP disconnect может менять или убирать доступный desktop. Источники: [Microsoft UI automation](https://learn.microsoft.com/en-nz/windows/apps/dev-tools/winapp-cli/ui-automation), [Microsoft Q&A](https://learn.microsoft.com/en-us/answers/questions/1605189/keep-gui-activated-after-rdp-disconnected), `chatgpt B2`, `claudeai B4`.

✅ доказано: Windows-хаб годится для unattended browser rail лучше, чем для произвольной screen rail. Источники: `chatgpt F`, `claudeai F`, `grok F2`.

✅ доказано: Linux X11/Xvfb остаётся самым простым техническим вариантом для synthetic input и screenshot в фиксированной GUI-сессии. Источники: [OpenAdapt design](https://github.com/OpenAdaptAI/openadapt-desktop/blob/main/DESIGN.md), [Playwright MCP](https://github.com/microsoft/playwright-mcp), `chatgpt B2`, `claudeai B4`, `grok B5`.

✅ доказано: Wayland protocol stack уже имеет RemoteDesktop/ScreenCast, PipeWire, libei и restore-token primitives. Источники: [XDG RemoteDesktop](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html), [XDG ScreenCast](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.ScreenCast.html), `chatgpt B2`, `claudeai B5`.

✅ доказано: наличие primitives не означает универсальную unattended-рельсу. Compositor/backend может показать consent, отозвать session или не поддержать input path. Источники: [GNOME portal issue 175](https://gitlab.gnome.org/GNOME/xdg-desktop-portal-gnome/-/issues/175), [trycua 1982](https://github.com/trycua/cua/issues/1982), `chatgpt B2`, `claudeai B5`, `grok B5`.

✅ доказано: для Linux VPS screen rail не нужна как базовая способность; browser/API rail надёжнее. Источники: `chatgpt F`, `claudeai F`, `grok F2`.

### 2.3 Логины и сессии

✅ доказано: cookie copying нельзя считать основной архитектурой. Источники: [Google Security Blog](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html), [Chrome Developers](https://developer.chrome.com/blog/remote-debugging-port), `chatgpt B3`, `grok B6`.

✅ доказано: Chrome cookies и credentials связаны с OS/user/app context. Источники: [Google Security Blog](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html), [Chromium Linux password storage](https://chromium.googlesource.com/chromium/src/%2B/fca37d0c4c6d4b9c5b6ce959f469c4f86179c9e3/docs/linux/password_storage.md), `chatgpt B3`, `grok B6`.

✅ доказано: живой локальный профиль устойчивее копии cookies как общий паттерн. Источники: [Playwright auth](https://playwright.dev/docs/auth), [Browserbase contexts](https://docs.browserbase.com/features/contexts), `chatgpt B3`, `grok B6`.

✅ доказано: `storageState` не равно полной и вечной сессии. Оно может не включать нужный sessionStorage, а server-side token может истечь. Источники: [Playwright auth](https://playwright.dev/docs/auth), `chatgpt E29`, `grok E25`.

✅ доказано: публичной сопоставимой таблицы session half-life по Google/X/Facebook/ChatGPT/Gemini/Claude, rail и OS нет. Источники: [Playwright auth](https://playwright.dev/docs/auth), [Browserbase contexts](https://docs.browserbase.com/features/contexts), `chatgpt A/B3`, `claudeai B6`, `grok B6`.

✅ доказано: любые конкретные сроки жизни login без собственного замера нельзя использовать для выбора рельсы. Источники: `chatgpt B3/G`, `grok A/B6`.

✅ доказано: один account/profile должен иметь одного writer одновременно. Источники: [Browserbase authentication](https://docs.browserbase.com/platform/identity/authentication), `chatgpt D`, `grok B4/E28`.

✅ доказано: стабильные IP, region, profile и fingerprint уменьшают класс «новое устройство», но не гарантируют отсутствие challenge. Источники: [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/troubleshooting/testing/), [Browserbase authentication](https://docs.browserbase.com/platform/identity/authentication), `chatgpt D`, `grok B7`.

✅ доказано: CAPTCHA, 2FA, account warning и «browser may not be secure» должны быть hard stop, а не бесконечный retry. Источники: [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/troubleshooting/testing/), [Playwright 3060](https://github.com/microsoft/playwright/issues/3060), `chatgpt D`, `claudeai A`, `grok F`.

✅ доказано: remote-browser profile persistence сохраняет инфраструктурное состояние, но не отменяет site-side expiry и WS death. Источники: [Browserbase contexts](https://docs.browserbase.com/features/contexts), [browser-use 4688](https://github.com/browser-use/browser-use/issues/4688), `chatgpt B3`, `grok E27`.

✅ доказано: agentic browsers не решают fleet durability; они переносят управление внутрь пользовательской сессии и добавляют prompt-injection risk. Источники: [Brave on Comet prompt injection](https://brave.com/blog/comet-prompt-injection/), `chatgpt B3`, `claudeai B6`, `grok B8`.

## 3. Расхождения между рельсами

### 3.1 Wayland: «условно пригоден» против «hard no»

✅ доказано: `chatgpt` считает Wayland технически возможным при рабочем portal/backend, сохранённом grant и libei.

✅ доказано: `claudeai` также признаёт primitives, но рекомендует X11/Xvfb для практической unattended работы.

✅ доказано: `grok` называет GNOME-class unattended Wayland hard no и допускает только узкие compositor-specific исключения.

✅ доказано: наиболее достоверная версия для нашего Linux-якоря: Wayland не является общей флотской рельсой; конкретная заранее протестированная конфигурация может работать.

✅ доказано: причина выбора: официальные portal APIs доказывают возможность механизма, а GNOME/trycua issues доказывают отсутствие универсальной безлюдности. Источники: [XDG RemoteDesktop](https://flatpak.github.io/xdg-desktop-portal/docs/doc-org.freedesktop.portal.RemoteDesktop.html), [GNOME 175](https://gitlab.gnome.org/GNOME/xdg-desktop-portal-gnome/-/issues/175), [trycua 1982](https://github.com/trycua/cua/issues/1982).

### 3.2 Какая ОС лучше для ночной screen rail

✅ доказано: `chatgpt` ставит разблокированную Windows console session выше macOS и Wayland для screen automation.

✅ доказано: `claudeai` считает Windows screen rail самой хрупкой из-за UAC/RDP, а macOS худшей для unattended screen work из-за TCC.

✅ доказано: `grok` не рекомендует unattended screen control ни на Mac laptop, ни на Windows при наличии UAC/lock; для Linux предлагает X11/Xvfb.

✅ доказано: противоречие снимается разделением «обычный unlocked desktop» и «любая GUI-задача».

✅ доказано: Windows-хаб лучше Mac16 как постоянная физическая площадка, если он остаётся разблокированным и workflow не вызывает UAC.

✅ доказано: X11/Xvfb лучше обоих по воспроизводимости виртуального дисплея, но не даёт доверенного человеческого профиля и residential identity сам по себе.

✅ доказано: для нашего флота достовернее запретить screen rail как ночной default на всех ОС, оставив OS-specific исключения после preflight.

### 3.3 Dedicated Chrome против Firefox-first

✅ доказано: `chatgpt` рекомендует dedicated Chrome/CDP основным rail на macOS/Windows и direct Playwright/remote browser на VPS.

✅ доказано: `claudeai` рекомендует Chrome for Testing + CDP для сложного control, Firefox для fetch/cookie-tolerant задач.

✅ доказано: `grok` рекомендует dedicated headed Chrome на Windows-хабе, Firefox AutoFF для headless fetch.

✅ доказано: сильные рельсы согласны не с blanket Chrome-first и не с blanket Firefox-first, а с разделением по классу задачи.

🤔 гипотеза: Firefox Selenium устойчивее Chrome CDP на наших обычных сайтах. Сравнительного fleet benchmark в отчётах нет.

✅ доказано: Chrome лучше обоснован для сложной DOM/CDP-рельсы, потому что для него есть explicit Target lifecycle, browser endpoint и Chrome DevTools MCP. Источники: [CDP Target domain](https://chromedevtools.github.io/devtools-protocol/tot/Target/), [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp).

### 3.4 Direct Playwright против attach-over-CDP

✅ доказано: `chatgpt` отмечает единичный RCA, где direct Playwright launch был стабильнее CDP transport.

✅ доказано: `claudeai` в общем сравнении предпочитает Playwright Selenium/Puppeteer для новых проектов.

✅ доказано: `grok` выбирает CDP attach к живому dedicated profile ради сессии.

✅ доказано: достоверный вывод: direct Playwright лучше для управляемого disposable browser; CDP attach нужен для долгоживущего локального профиля.

🤔 гипотеза: один Playwright MCP и chrome-devtools-mcp могут безопасно делить один CDP endpoint. `grok` предлагает это узко, но общая конфликтность нескольких owners подтверждена; без собственного теста не принимать.

### 3.5 Firefox и бан-риск

✅ доказано: `claudeai` и `grok` склоняются к Firefox AutoFF для fetch и менее чувствительных задач.

🤔 гипотеза: Firefox автоматически безопаснее для X/Facebook/Grok. Controlled evidence в отчётах нет.

✅ доказано: более сильный фактор для бан-чувствительных задач: постоянный профиль, один writer, residential IP, headed human-like environment и отсутствие автоматизированного login flow. Источники: [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/troubleshooting/testing/), `chatgpt B1/B3`, `grok B7`.

✅ доказано: наш текущий Chrome carve-out для Facebook/X/Grok подтверждается лучше, чем перенос этих действий в headless Firefox.

### 3.6 macOS TCC «после обновления» против «ежемесячно»

✅ доказано: все сильные рельсы подтверждают update/helper identity breakage.

🤔 гипотеза: ежемесячный prompt является универсальным неизбежным поведением для текущего Mac16. Это опирается на публикации и practitioner reports, а не на замер Mac16.

✅ доказано: operational rule не должен зависеть от причины. Перед ночным screen run нужен реальный screenshot+click preflight.

### 3.7 GLM

✅ доказано: `glm` повторяет выводы о dedicated profile, CDP disconnects, TCC, UAC, X11 и cookie encryption.

✅ доказано: эти совпадения не повышают статус доказательств, потому что proof-of-work `glm` не пройден.

## 4. Матрица платформ

| ОС / слой | Что ломается | Насколько часто | Что лечит | Применимость к нашим узлам |
|---|---|---|---|---|
| macOS / browser | ✅ Chrome 136 не открывает debug flags на Default profile | ✅ структурно для затронутого режима; частоты запусков в отчётах нет | ✅ dedicated `user-data-dir`, explicit endpoint, Chrome for Testing | ✅ Mac16: не attach к ежедневному Chrome через flags |
| macOS / browser | ✅ MCP/CDP transport half-death | ✅ повторяется в нескольких issue; общей частоты нет | ✅ timeout, supervisor, restart MCP, затем browser при необходимости | ✅ Mac16: ночной watchdog обязателен |
| macOS / browser | ✅ stale/dead target и silent retarget | ✅ повторяется; общей частоты нет | ✅ logical tab identity, fresh target list, fail-closed | ✅ Mac16: нельзя хранить один pageId навсегда |
| macOS / browser | ✅ human/agent focus и tab churn | ✅ каждый раз при конкурентном foreground control риск существует | ✅ отдельный profile/window, lease, DOM rail вместо pixels | ✅ Mac16: AutoFF и automation Chrome не трогать руками во время run |
| macOS / screen | ✅ TCC grant перестаёт работать после helper/path update | ✅ несколько свежих issue; процент неизвестен | ✅ stable bundle, post-update screenshot+click preflight | ✅ Mac16: главный локальный screen-risk |
| macOS / screen | ✅ sleep/lid/lock убивает screen rail | ✅ детерминировано при наступлении условия | ✅ AC, lid open, no sleep, unlocked session | ✅ Mac16: ночной screen rail нельзя считать автономным без этого |
| macOS / screen | ✅ Retina/HiDPI coordinate mismatch | ✅ конкретные issue; частоты нет | ✅ scale calibration, a11y/DOM first | ✅ Mac16: фиксировать topology и scale до run |
| macOS / login | ✅ Keychain-bound state не переносится как обычный профиль | ✅ структурно при переносе/другом context | ✅ local persistent profile; не копировать cookie DB | ✅ Mac16: отдельные локальные профили |
| Windows / browser | ✅ profile lock и second-process conflict | ✅ детерминировано при двух writers | ✅ one profile, one process, one owner | ✅ Windows-хаб и узлы коллег |
| Windows / browser | ✅ enterprise policy может блокировать remote debugging | ✅ только на managed hosts; fleet frequency неизвестна | ✅ preflight policy; unmanaged automation host | ✅ проверить узлы коллег; хаб вероятно вне доменной политики, но это не доказано |
| Windows / browser | ✅ orphan Chrome/MCP processes | ✅ несколько issue; fleet frequency неизвестна | ✅ process group, TTL, orphan reap по конкретному profile path | ✅ хаб: включить в supervisor |
| Windows / screen | ✅ UAC secure desktop даёт black/no input | ✅ детерминировано при UAC prompt | ✅ исключить elevation из workflow; hard stop | ✅ хаб: ночные jobs не должны вызывать UAC |
| Windows / screen | ✅ lock/RDP disconnect ломает pixels/input | ✅ детерминировано для затронутой session topology | ✅ console session, no auto-lock, проверенный `tscon` flow | ✅ хаб: browser DOM можно оставить, pixels только после preflight |
| Windows / screen | ✅ multi-monitor/DPI меняет координаты | ✅ при смене topology риск постоянный; частоты нет | ✅ fixed display, calibration, UIA/DOM first | ✅ хаб с несколькими дисплеями: отдельный fixed automation display |
| Windows / login | ✅ ABE ломает external cookie copying | ✅ структурно для защищённых cookies | ✅ live profile, не извлекать/копировать Cookies DB | ✅ хаб и Windows-узлы |
| Windows / login | ✅ один account в нескольких writers вызывает site-side churn | ✅ подтверждён класс; частоты нет | ✅ один writer на account; human handoff через тот же profile | ✅ social accounts держать на хабе |
| Linux / browser | ✅ headless/container может иметь datacenter/fingerprint risk | ✅ site-dependent; чисел нет | ✅ обычные сайты/API; residential rail для sensitive; hard stop on challenge | ✅ Linux-якорь не использовать для FB/Reddit/Threads posting |
| Linux / browser | ✅ stale SingletonLock после crash | ✅ известный повторяющийся класс; частоты нет | ✅ сначала доказать отсутствие процесса; затем очищать конкретный stale lock | ✅ Linux-якорь с persistent browser |
| Linux / browser | ✅ Docker CDP может быть недоступен с host | ✅ configuration-dependent | ✅ explicit networking/forward; health check снаружи container | ✅ только если якорь контейнеризован; в отчётах наш факт отсутствует |
| Linux / screen X11 | ✅ нет DISPLAY/Xauthority или X server | ✅ детерминировано на headless VPS без GUI | ✅ Xvfb + fixed DISPLAY или отказаться от screen | ✅ якорь: screen нужен только для browser GUI exception |
| Linux / screen Wayland | ✅ portal consent/session/input no-op | ✅ compositor-dependent; общей частоты нет | ✅ tested restore token/libei или X11/Xvfb | ✅ якорь: не выбирать Wayland как default |
| Linux / login | ✅ keyring может быть недоступен headless | ✅ environment-dependent; частоты нет | ✅ persistent keyring или явно принятый secure single-tenant storage design | ✅ якорь: не обещать login durability до собственного теста |
| Все ОС / auth | ✅ CAPTCHA/2FA/account warning | ✅ site/risk-dependent; частоты нет | ✅ hard stop, same profile/IP, headed login, не автоматизировать challenge | ✅ весь флот |
| Все ОС / auth | ✅ `storageState` истекает или неполон | ✅ неизбежно со временем; срока нет | ✅ auth canary, refresh from live profile, собственная telemetry | ✅ весь флот |
| Все ОС / updates | ✅ browser/tool update меняет rail | ✅ событие неизбежно; поломка не каждый раз | ✅ canary update, smoke, затем rollout | ✅ Mac16 как canary, хаб после проверки либо наоборот по риску задачи |
| Все ОС / agentic browser | ✅ prompt injection и отсутствие fleet watchdog | ✅ класс подтверждён; frequency нет | ✅ не использовать как unattended foundation | ✅ Comet/подобные не выбирать основой |

## 5. Ответ «Firefox или Chrome» для наших сценариев

| Наш сценарий | Вердикт исследований | Рельса | Основание |
|---|---|---|---|
| Обычный headless fetch без сложного login | ✅ подтверждают выбор | Firefox AutoFF | ✅ `claudeai F`, `grok F2`; Chrome преимуществ здесь не доказал |
| Обычная залогиненная страница, Selenium уже работает | 🤔 исследования молчат о превосходстве | Сохранить Firefox до собственного A/B | 🤔 нет сравнительной частоты Firefox Selenium против Chrome CDP |
| Сложная DOM/CDP-автоматизация | ✅ опровергают blanket Firefox-first | Dedicated Chrome/CfT | ✅ CDP Target lifecycle и DevTools MCP имеют нужные primitives |
| Реальный постоянный automation profile | ✅ уточняют выбор | Dedicated local profile, чаще Chrome/CfT | ✅ Chrome default profile запрещён; отдельный profile обязателен |
| Ввод пароля | ✅ подтверждают Chrome carve-out | Chrome | ✅ Chrome password manager работает внутри своего app/user context; cookie copying запрещён |
| Строго Chromium-dependent сайт | ✅ подтверждают | Chrome/CfT | ✅ Firefox функционально не подходит по условию |
| Facebook/X/Grok от лица человека | ✅ подтверждают Chrome на хабе | Headed Chrome на Windows-хабе | ✅ residential IP, постоянный профиль, human-like surface важнее Firefox label |
| Reddit/Threads posting/commenting | ✅ подтверждают location rule | Headed browser только на хабе | ✅ IP-sensitive контекст задан; отчёты поддерживают стабильность IP/profile |
| Google login page | ✅ уточняют: не автоматизировать login | Нормальный headed Chrome, ручной/доверенный login один раз | ✅ Google automation block class; затем reuse profile |
| chat.z.ai bridge через page JS к `127.0.0.1` | ✅ подтверждают симптом, механизм не полностью | Не менять Firefox/Chrome вслепую; убрать page-fetch bridge | ✅ PNA/CORS/extension isolation лишь частично подтверждены |
| Gemini CSP режет bridge | ✅ подтверждают класс CSP/local bridge | Native host/CDP/server-side endpoint | ✅ смена Firefox/Chrome сама не лечит CSP |
| Вторая скачка в Chrome пропадает | 🤔 одна наблюдаемая рельса | Fresh tab/context или CDP download API после теста | 🤔 upstream-доказательство слабое; не обобщать на Firefox |
| claude.ai composer в hidden tab | ✅ наш факт, отчёты не доказывают общий класс | POST `/completion` как известная дверь | 🤔 внешняя сравнительная база Firefox/Chrome отсутствует |
| Ночной Mac16 browser run | ✅ частично подтверждают Firefox-first | Firefox для простого; Chrome/CfT для сложного | ✅ screen rail не использовать; оба browser rails требуют watchdog |
| Ночной Windows-хаб social run | ✅ опровергают Firefox-by-default | Dedicated headed Chrome | ✅ профиль+IP+password manager+Chromium compatibility |
| Linux-якорь обычный web | ✅ не подтверждают Firefox-only | Direct Playwright/Chromium или Firefox по task fit | ✅ важнее отсутствие screen rail и auth sensitivity |
| Linux-якорь бан-чувствительный social | ✅ опровергают | Не исполнять там | ✅ datacenter IP/fingerprint residual risk |
| Extension в ежедневном Chrome | ✅ опровергают как default | Только fallback/attended handoff | ✅ service-worker/native-host/owner conflicts |
| Agentic browser | ✅ опровергают как fleet rail | Не выбирать основой | ✅ durability не решена, prompt-injection risk добавлен |
| Screenshot-click в браузере | ✅ опровергают как default | DOM/a11y/CDP first | ✅ TCC/UAC/Wayland/focus/DPI добавляют независимые отказы |

### Где §4.8 расходится с данными

✅ доказано: фраза «Firefox по максимуму» верна как cost/simplicity preference для обычного fetch и уже работающих Firefox workflows.

✅ доказано: она расходится с данными, если заставляет выбирать Firefox для сложной browser automation, где нужны CDP lifecycle, DevTools MCP, Chrome password manager или Chromium-only behavior.

✅ доказано: текущий carve-out «Chrome для бан-чувствительных действий от лица Антона, ввода пароля и strictly Chromium» подтверждён.

✅ доказано: текущему правилу не хватает четвёртого carve-out: «сложная DOM/CDP-рельса с восстановлением target/transport».

✅ доказано: текущему правилу не хватает различия между stable Chrome и Chrome for Testing/dedicated automation profile.

✅ доказано: текущему правилу не хватает запрета attach к daily Default profile через `:9222`.

✅ доказано: текущему правилу не хватает запрета screen rail как ночного default.

🤔 гипотеза: Firefox должен остаться численно главным браузером после изменения. Отчёты не содержат нашего task mix, поэтому это покажет только собственный счётчик.

## 6. Рекомендации по ночной безлюдной рельсе

### P0. До ближайшего ночного запуска

1. ✅ доказано: разделить рельсы по задаче, а не по лозунгу браузера.
2. ✅ доказано: обычный fetch оставить в AutoFF.
3. ✅ доказано: сложный control перевести на dedicated Chrome/CfT с отдельным profile dir.
4. ✅ доказано: daily Chrome Default profile не открывать через remote-debugging flags.
5. ✅ доказано: один profile = один process = один owner = одна машина.
6. ✅ доказано: IP-sensitive posting оставить только на Windows-хабе.
7. ✅ доказано: не копировать cookie DB и живые profile dirs между узлами.
8. ✅ доказано: перед полезным действием выполнить auth canary по конкретному logged-in marker.
9. ✅ доказано: CAPTCHA/2FA/account warning считать hard stop.
10. ✅ доказано: screen rail ночью выключить по умолчанию.

### P0. Watchdog browser rail

1. ✅ доказано: проверять browser process по точному profile path.
2. ✅ доказано: проверять browser endpoint, но не считать его достаточным.
3. ✅ доказано: ставить timeout на CDP/MCP command.
4. ✅ доказано: проверять target list.
5. ✅ доказано: проверять logical identity вкладки: site, account, workflow и marker.
6. ✅ доказано: выполнять дешёвую page-level команду или новый snapshot.
7. ✅ доказано: при stale refs инвалидировать весь старый snapshot.
8. ✅ доказано: разрешать только один safe retry для идемпотентного действия.
9. ✅ доказано: при identity mismatch не выбирать первую вкладку.
10. ✅ доказано: различать `BROWSER_DOWN`, `TRANSPORT_DOWN`, `TARGET_DOWN`, `PAGE_FROZEN`, `AUTH_DOWN`, `CHALLENGE`.
11. ✅ доказано: сначала перезапускать MCP/transport, затем browser только при доказанной смерти browser.
12. ✅ доказано: после browser restart подключаться к тому же local profile, но заново находить target.
13. ✅ доказано: не удалять SingletonLock, пока не доказано отсутствие живого owner process.
14. ✅ доказано: ограничить число вкладок automation profile.
15. ✅ доказано: reaper должен действовать только на процессы с конкретным automation profile path.

### P0. Экран

1. ✅ доказано: Mac16 перед screen job проверяет текущий helper identity.
2. ✅ доказано: Mac16 делает свежий screenshot preflight.
3. ✅ доказано: Mac16 делает harmless Accessibility click/focus round-trip.
4. ✅ доказано: Mac16 проверяет lid open, AC, no sleep, unlocked session.
5. ✅ доказано: Windows-хаб проверяет console session, no lock и отсутствие ожидаемого UAC.
6. ✅ доказано: Windows-хаб фиксирует monitor topology и DPI.
7. ✅ доказано: Linux-якорь не запускает общий Wayland screen control.
8. ✅ доказано: если Linux GUI действительно нужен, использовать fixed X11/Xvfb и отдельный browser profile.
9. ✅ доказано: screen rail берёт exclusive foreground lease.
10. ✅ доказано: browser DOM rail и screen-click rail не работают одновременно на одной вкладке.

### P1. Обновления и восстановление

1. ✅ доказано: automation browser обновлять canary-кольцом.
2. ✅ доказано: после browser update проверять launch, endpoint, target, page command и auth marker.
3. ✅ доказано: после Claude/Codex/helper update на Mac повторять TCC functional preflight.
4. ✅ доказано: auto-update во время ночного окна либо отключить для pinned automation binary, либо принять как контролируемый restart event.
5. ✅ доказано: после reboot запускать browser supervisor раньше LLM job.
6. ✅ доказано: reboot recovery должен доказать свежий auth marker, а не только process alive.
7. ✅ доказано: extension rail держать как attended fallback, не как unattended recovery.

### P1. Сессии и собственные данные

1. ✅ доказано: вести telemetry `{site, node, OS, browser, rail, profile, IP class, last_login, auth_ok, challenge, forced_logout, cause}`.
2. ✅ доказано: измерять фактический session survival на наших аккаунтах.
3. ✅ доказано: не публиковать и не использовать «срок жизни» до накопления наших наблюдений.
4. ✅ доказано: хранить один account writer и явный handoff.
5. ✅ доказано: remote browser рассматривать только как отдельную рельсу, а не backup cookie jar.
6. 🤔 гипотеза: self-hosted remote browser может быть полезен для non-social concurrency; отчёты не доказывают выгоду для нашего масштаба.

### P2. Не строить сейчас

1. ✅ доказано: не строить универсальный semantic rebind до датированных повторов класса; сначала простой restart+rediscovery fail-closed.
2. ✅ доказано: не внедрять stealth browser без A/B на наших сайтах.
3. ✅ доказано: не переносить social accounts на VPS/cloud ради удобства.
4. ✅ доказано: не делать agentic browser фундаментом флота.
5. ✅ доказано: не пытаться обходить UAC/TCC/Wayland security boundaries.

## 7. Что конкретно менять

### 7.1 Матрица `/firefox`

🤔 гипотеза: буквальный текст текущей матрицы `/firefox` в отчётах отсутствует. Ниже дан обязательный смысловой diff для её строк.

| Строка | Как было | Как станет | Почему |
|---|---|---|---|
| Default browser | ✅ Firefox по максимуму | ✅ Firefox default для fetch, обычных сайтов и уже устойчивых AutoFF workflows | ✅ исследования поддерживают Firefox в этой нише, но не универсальное превосходство |
| Complex browser control | 🤔 отдельной строки нет | ✅ Dedicated Chrome/CfT + CDP/MCP для сложного DOM control и target lifecycle | ✅ CDP primitives и issue evidence относятся к этой архитектуре |
| Chrome profile | 🤔 Chrome вынужденно | ✅ отдельный local `user-data-dir`; daily Default через `:9222` запрещён | ✅ Chrome 136 security change |
| Chrome build | 🤔 просто Chrome | ✅ Chrome for Testing или отдельно управляемый automation Chrome; update canary | ✅ Google сохраняет automation behavior в CfT |
| Password entry | ✅ Chrome | ✅ Chrome, пароль подаёт встроенный password manager; cookie copying запрещён | ✅ app/user-bound storage и ABE |
| Chromium-only | ✅ Chrome | ✅ Chrome/CfT | ✅ функциональная зависимость |
| Ban-sensitive human action | ✅ Chrome | ✅ headed Chrome на Windows-хабе с резидентным IP и одним writer | ✅ profile/IP stability важнее абстрактного stealth |
| Facebook/X/Grok | ✅ Chrome | ✅ headed hub Chrome; login flow не автоматизировать; challenge hard stop | ✅ антибот и account-risk evidence |
| Reddit/Threads posting | ✅ хаб по IP-правилу | ✅ браузер только на хабе; Firefox/Chrome выбирает site compatibility, default headed Chrome для доверенного профиля | ✅ исследования не доказывают Firefox advantage, IP rule остаётся |
| Simple authenticated Firefox site | ✅ AutoFF | ✅ оставить AutoFF, пока наш A/B не покажет хуже | 🤔 сравнительных данных нет, ломать рабочее без данных нельзя |
| Headless sensitive account | 🤔 Firefox допустим | ✅ не считать headless Firefox безопасным автоматически | ✅ Firefox fingerprint advantage не доказан |
| Local bridge from page JS | 🤔 браузер выбирается по привычке | ✅ не использовать page `fetch(127.0.0.1)` как основной bridge; native/CDP/server-side door | ✅ CSP/PNA class не лечится простой сменой браузера |
| Downloads | 🤔 Chrome при необходимости | ✅ одна подтверждённая download operation на fresh tab/context; verify file before next step | 🤔 exact second-download mechanism не доказан, но silent failure известен нам |
| Vendor extension | 🤔 Chrome workaround | ✅ attended fallback only | ✅ service-worker/native-host/owner conflicts |
| Screen click | 🤔 fallback | ✅ attended fallback с lease и preflight; ночью запрещён по умолчанию | ✅ TCC/UAC/Wayland/focus/DPI failures |
| Linux anchor | 🤔 Firefox headless | ✅ browser/API by task fit; X11/Xvfb только при доказанной нужде GUI | ✅ общий screen rail на VPS не нужен |
| Profile ownership | 🤔 не зафиксировано | ✅ one profile = one process = one owner = one node | ✅ locks, encryption и site-side churn |
| Session restore | 🤔 cookies/storageState | ✅ live local profile first; storageState только вспомогательный state | ✅ expiry и неполнота storageState |
| Challenge | 🤔 retry/login | ✅ CAPTCHA/2FA/warning = hard stop | ✅ retry повышает риск и не решает boundary |
| Health check | 🤔 browser launched | ✅ process + endpoint + target identity + page command + auth marker | ✅ port alive недостаточен |
| Recovery | 🤔 restart browser | ✅ restart MCP first; browser only if dead; rediscover target; one safe retry | ✅ три независимых класса смерти |
| Updates | 🤔 auto-update переживём | ✅ canary, smoke, fleet rollout; Mac TCC recheck | ✅ browser/helper updates ломают rail |

### 7.2 Предлагаемая формулировка §4.8

#### ДО

> Firefox по максимуму. Chrome вынужденно и с названной причиной: бан-чувствительное от лица Антона, ввод пароля, строго-Chromium.

#### ПОСЛЕ

> **§4.8 Браузерная рельса: Firefox по умолчанию, выбор по классу задачи.** Обычный fetch, headless и уже устойчивые workflows выполняются в выделенном AutoFF. Chrome/CfT обязателен, когда нужен Chrome password manager, строго Chromium-поведение, сложный DOM/CDP control или бан-чувствительное действие от лица Антона. Для CDP используется только отдельный локальный `user-data-dir`; daily Default profile через remote-debugging flags запрещён. Один профиль = один процесс = один владелец = один узел; профиль и cookie DB между машинами не копируются. IP-sensitive действия остаются только на хабе. Vendor extension и screen-click = attended fallback, не ночная основа. Ночью browser rail проходит process + endpoint + target + page + auth preflight; CAPTCHA/2FA/account warning = hard stop. Причина выбора Chrome называется в отчёте постфактум.

### 7.3 Почему эта правка лучше

✅ доказано: сохраняет приказ «Firefox по максимуму» там, где он подтверждён практикой и не конфликтует с данными.

✅ доказано: добавляет missing carve-out для сложного DOM/CDP control.

✅ доказано: закрывает Chrome 136 rake с Default profile.

✅ доказано: закрывает profile-copy и multi-writer rakes.

✅ доказано: отделяет browser rail от screen rail.

✅ доказано: делает ночной запуск fail-closed по auth и tab identity.

✅ доказано: не заявляет недоказанное превосходство Firefox или Chrome во всех задачах.

### 7.4 До, после, что сломается

| Что | До | После | Что может сломаться |
|---|---|---|---|
| Browser choice | ✅ лозунг по бренду | ✅ решение по классу задачи | 🤔 старые вызовы могут не указывать task class |
| Chrome profile | 🤔 любой рабочий Chrome | ✅ только dedicated automation profile для CDP | ✅ потребуется отдельный первичный login |
| Ночной screen | 🤔 возможен как общий fallback | ✅ запрет по умолчанию | ✅ screen-only workflow остановится и потребует перепроектирования |
| Recovery | 🤔 restart без диагноза | ✅ классификация и safe retry | 🤔 текущие скрипты могут не отдавать точные exit reasons |
| Firefox scope | ✅ максимально широко | ✅ fetch и проверенные workflows | 🤔 часть задач уйдёт в Chrome, счётчик Firefox снизится |
| Chrome scope | ✅ три исключения | ✅ четыре исключения плюс dedicated profile contract | ✅ нужен отдельный lifecycle supervisor |

## 8. Открытые вопросы и пробелы отчётов

🤔 гипотеза: какой browser rail реально даёт больше успешных ночей на Mac16. В отчётах нет нашего A/B.

🤔 гипотеза: Firefox Selenium AutoFF переживает auto-update лучше Chrome/CfT на Mac16. Сравнительных данных нет.

🤔 гипотеза: Firefox реже вызывает challenge на Facebook/X/Grok. Controlled data нет.

🤔 гипотеза: Chrome headed на Windows-хабе даёт максимальный session survival для наших аккаунтов. Механизм правдоподобен, измерений нет.

🤔 гипотеза: какой срок жизни сессий Google/X/Facebook/ChatGPT/Gemini/Claude именно у нас. Публичных данных нет.

🤔 гипотеза: частота browser-level, transport-level и target-level deaths в наших ночных runs. Телеметрии в отчётах нет.

🤔 гипотеза: текущий Chrome на хабе managed policy или нет. В отчётах нет.

🤔 гипотеза: текущая topology RDP/console на хабе переживает disconnect. В отчётах нет нашего теста.

🤔 гипотеза: какой именно helper identity сейчас имеет TCC grants на Mac16. В отчётах нет локального preflight.

🤔 гипотеза: macOS ежемесячный re-prompt воспроизводится на текущей версии и текущем helper Mac16. Нет локального замера.

🤔 гипотеза: может ли текущий Linux-якорь безопасно держать persistent keyring. Конфигурация не исследована.

🤔 гипотеза: используется ли на якоре Wayland, X11, Xvfb или только headless browser. В отчётах нет inventory.

🤔 гипотеза: можно ли shared CDP endpoint дать двум MCP-клиентам без гонок в нашей конфигурации. Не проверено.

🤔 гипотеза: точная причина молчаливого `127.0.0.1` bridge failure в chat.z.ai. PNA/CSP правдоподобны, но RCA нет.

🤔 гипотеза: точная причина второй молча потерянной Chrome download в Gemini flow. Upstream evidence недостаточно.

🤔 гипотеза: hidden-tab freeze claude.ai composer является browser lifecycle behavior или app-specific behavior. В отчётах нет RCA.

🤔 гипотеза: POST `/completion` останется стабильной дверью после vendor update. Нужен contract test.

🤔 гипотеза: remote-browser service окупится для non-social concurrency. Отчёты дают возможности, но не наш cost/failure baseline.

🤔 гипотеза: нужен ли semantic rebind вообще после простого restart+rediscovery. Нет трёх наших датированных поломок класса.

🤔 гипотеза: текущая буквальная матрица `/firefox` содержит все перечисленные строки. Файл не был частью входа.

🤔 гипотеза: новое правило сохранит фактическое большинство задач за Firefox. Нужен usage counter по task class.

## Итог решения

✅ доказано: не отменять Firefox-first целиком.

✅ доказано: сузить Firefox-first до обычного fetch, headless и уже доказанно устойчивых workflows.

✅ доказано: добавить dedicated Chrome/CfT как штатную, а не стыдливую исключительную рельсу для сложного DOM/CDP control.

✅ доказано: бан-чувствительные действия сохранить на headed Chrome Windows-хаба с резидентным IP.

✅ доказано: ночной screen control не считать штатной рельсой ни на Mac16, ни на Windows, ни на Linux.

✅ доказано: выбор браузера без supervisor, auth canary и one-writer profile contract не решает главную проблему.

✅ доказано: §4.8 надо менять с «бренд по умолчанию + три исключения» на «класс задачи + четыре исключения + контракт профиля и восстановления».

## 🧒 Простыми словами

Firefox оставляем рабочей лошадкой для простых поездок.

Для сложного управления нужен отдельный Chrome, не личный Chrome Антона.

Один браузерный профиль нельзя делить между роботами и машинами.

Ночью робот сначала проверяет браузер, вкладку и вход в аккаунт.

Если появился пароль, CAPTCHA или 2FA, робот останавливается.

Экранные клики ночью слишком хрупкие, лучше работать через страницу изнутри.

Главная починка: выбирать рельсу по задаче и громко падать, если она умерла.

## Связано
- [[insight-DR-DR26-08-15-HUB-01-browser-rail-cdp-chrome-и-screen-automation-rail-п]] — тот же кластер тем (browser rail CDP/Chrome + screen automation), синтез на день раньше по смежному DR
- [[insight-DR-DR26-08-15-HUB-03-real-world-failure-modes-of-agent-driven-browsers-]] — тот же исходный DR-документ, параллельный синтез другим вендором того же сырья
