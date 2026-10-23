# DR26-07-16-HUB-04 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- PROVENANCE
source: Grok (SuperGrok, Expert, Grok 4.5), account Tony0x.eth, chat https://приватный чат
dr_id: DR26-07-16-HUB-04
topic: headless browser automation on managed Win11 (SxS mozglue / geckodriver stock Firefox)
collected: 2026-07-16  machine: хаб  operator: Anton

-->

# DR26-07-16-HUB-04 — ответ Grok (Expert, 75 sources)

## 1. Root-cause SxS mozglue v1.0.0.0
Точная причина (по Grok) — дефект embedded-манифеста внутри firefox.exe Playwright-сборки (Juggler-патч): манифест объявляет mozglue.dll как SxS-зависимую сборку (`<dependentAssembly><assemblyIdentity name="mozglue" version="адрес узла" type="win32"/>`), а не как приватный файл приложения (`<file name="mozglue.dll"/>`). Windows ищет сборку в WinSxS/зарегистрированных приватных и падает «Activation context generation failed». Совпадает с Event Log. Аналог подтверждён в кастомных билдах Firefox (camoufox) — тот же verbatim. [emerging; прямого Playwright-issue с этим verbatim в 2025-2026 не найдено]
Почему на managed падает, а стоковый v127 работает: стоковый ставится инсталлятором Mozilla (правильный манифест+регистрация); Playwright-бандл — «portable» с embedded-ресурсами, на захардненной (endpoint/возможный WDAC/AppLocker/строгие политики активации) — жёсткий фейл.
Порядок причин: (1) embedded-манифест дефект билда [основная]; (2) endpoint/WDAC/AppLocker ужесточают разрешение application-local SxS; (3) WinSxS-стор/нет fallback для неподписанных; (4) конфликт с агентами [менее вероятно].
Диагностика: `sxstrace trace/parse`, `Get-WinEvent SideBySide`, `Get-AppLockerPolicy -Effective`, `Get-CIPolicy` (WDAC), `Get-MpPreference` (ASR), sigcheck -i на манифест.
Фиксы: без админа почти нет; с админом — бинарный патч манифеста (как camoufox-workaround: заменить dependency-блок пробелами) [single-source, рискованно, ломает подпись] → лучше НЕ использовать Playwright-Firefox-бандл вообще.

## 2. Драйвинг стокового Firefox
Playwright требует свой Juggler-билд; executablePath на стоковый обычно не работает.
- **(а) geckodriver + Selenium (Marionette): полноценно работает, persistent-профиль (-profile), запускается на managed, ремонтопригодность высокая → ЛУЧШИЙ.**
- (б) WebDriver-BiDi: Playwright 1.57+ `channel:'moz-firefox'` запускает стоковый FF через BiDi; persistent частично/экспериментально [emerging].
- (в) -marionette напрямую: низкоуровнево, не практично.
- (г) Playwright executablePath на стоковый: не рекомендуется, часто не работает (нет Juggler).
- (д) Puppeteer: Firefox через BiDi с 2024+ [emerging]; Selenium зрелее для FF.

## 3. Playwright Chromium
Скорее всего запустится (другой бандлинг, нет mozglue-SxS). `launch_persistent_context` → мы сами ведём профиль → ABE/DBSC не мешают (бьют внешнее извлечение, не сессию внутри управляемого браузера). Бан-риск ВЫШЕ, чем у живой вкладки через расширение; headless легче детектится (Cloudflare); на managed возможен блок CDP-политикой.

## 4. Best-practice (AK-47)
**Selenium + geckodriver + стоковый Firefox + dedicated automation-профиль.** Минимум частей (Python+pip+уже установленный Firefox), trusted signed компоненты (меньше ASR/endpoint-блоков чем bundled Playwright), persistent logged-in недели/месяцы, чинится обновлением Selenium/geckodriver, анти-детект без malware. Ранжирование: Selenium FF > Playwright Chromium persistent (если CDP не блокнут) > Playwright BiDi moz-firefox [emerging]. Избегать: Playwright Firefox bundled, portable custom-FF, malware-класс.

## 5. Конкретика
- Один раз вручную: `about:profiles` → Create New Profile `automation` → Launch → залогиниться (2FA) → закрыть → запомнить путь (`...\AppData\Roaming\Mozilla\Firefox\Profiles\xxxx.automation`).
- `pip install selenium webdriver-manager`
- Скрипт: `Options().add_argument("-profile"); add_argument(PROFILE_PATH); add_argument("--headless")`; `webdriver.Firefox(service=Service(GeckoDriverManager().install()), options=...)`.
- Чек-лист: профиль+логин один раз → pip install → подставить PROFILE_PATH → `python auto_firefox.py` (.bat-обёртка) → отладка без --headless → «профиль занят» = закрыть все Firefox → обновление `pip install -U` → бэкап = копия папки профиля.
Fallback: `playwright.chromium.launch_persistent_context(user_data_dir=..., headless=True)`.
