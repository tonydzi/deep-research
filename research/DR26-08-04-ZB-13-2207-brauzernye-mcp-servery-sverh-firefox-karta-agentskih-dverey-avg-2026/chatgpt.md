# DR26-08-04-ZB-13-2207 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Ниже — карта на **04.08.2026**. Ваши результаты Firefox/DBSC принимаю как локально подтверждённые и не пересматриваю.

Метки:

- **[established]** — официальная документация, репозиторий/registry либо ваш воспроизводимый замер.
- **[emerging]** — официальная новая/экспериментальная функция с небольшим эксплуатационным стажем.
- **[speculative]** — технически вероятно, но нет подтверждённого контракта.
- **[contested]** — источники или практические наблюдения расходятся.

## Короткий вывод

**Одной Firefox-рельсы достаточно как основной боевой рельсы.** Второй постоянно работающий MCP сейчас не нужен.

После окончания замера 18.08 я бы добавил **один резервный Chromium-движок: Edge + `chrome-devtools-mcp`**, но держал его выключенным и поднимал только для:

- Chromium-only несовместимостей;
- сайтов, которые целенаправленно тестируют только Blink;
- диагностики WebView2/Chrome DevTools;
- производительности, network/console/heap/Lighthouse;
- Android Chromium через ADB.

Почему Edge, а не Chrome: на вашем Windows-хабе это уже системный Chromium; Microsoft официально документирует live attach `chrome-devtools-mcp` к Edge, включая существующий User Data. Отдельного Edge MCP нет — используется сервер Google поверх совместимого CDP. [Документация Microsoft](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/devtools-mcp-server) [established].

Playwright MCP полезен как **лабораторная кросс-движковая рельса**, но не как второй хранитель аккаунтных сессий. Opera Neon — интересный официальный live-session MCP, однако для AK-47-контура пока лишний продукт и дополнительная доверенная сторона.

---

## 1. Перепись браузерных MCP

Версии относятся к доступным стабильным пакетам/публикациям, обнаруженным к указанной дате. Там, где собственного пакета нет, версия обозначена «—», а не придумана.

| Браузер / движок | MCP или официальный протокол | Издатель; версия/дата | Live attach | Headless Win / Linux | Профиль на диске | Решение | Уверенность |
|---|---|---|---|---|---|---|---|
| **Firefox Desktop** | `@mozilla/firefox-devtools-mcp`; WebDriver BiDi + Marionette | Mozilla; **0.9.15**, подтверждено вашим замером 04.08.2026; официальный проект подтверждён [Mozilla Source Docs](https://firefox-source-docs.mozilla.org/ai-agent-tools/firefox-devtools-mcp.html) | **Да:** `--connectExisting`, `--marionettePort`; browser пережил disconnect и re-attach — ваш локальный факт | **Да / да**; MCP требует локальную установку Firefox, старый пакет подтверждает архитектуру BiDi/Selenium и локальный запуск: [npm](https://www.npmjs.com/package/firefox-devtools-mcp) | **Да**, отдельный обычный профиль | **Оставить основной** | [established, local] |
| **Firefox Android** | Тот же Mozilla MCP через ADB/BiDi | Mozilla, та же ветка | Да, в поддерживаемой конфигурации; не аналог desktop profile attach | Хост Win/Linux; сам браузер на Android | Профиль устройства | Оставить как специализированный мобильный путь | [established по вашему инвентарю; конфигурацию ADB отдельно не перепроверял] |
| **Chrome / Chrome for Testing** | `chrome-devtools-mcp` поверх CDP/Puppeteer | Google Chrome DevTools; **1.6.0, 14.07.2026** по [официальному changelog](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/CHANGELOG.md) | **Да:** `--browserUrl`, `--wsEndpoint`; Chrome 144+ — `--autoConnect`. [Официальный README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md) | **Да / да**, включая headless | Свой `--user-data-dir`; auto-connect умеет обнаружить живой профиль. Официальная поддержка — только Chrome/CfT, не произвольный Chromium [репозиторий](https://github.com/ChromeDevTools/chrome-devtools-mcp) | **Поставить позже как холодный резерв**, не как аккаунтную основную рельсу | [established] |
| **Edge** | Тот же `chrome-devtools-mcp`; собственного Edge MCP нет | Google server + официальная инструкция Microsoft; сервер 1.6.0 | **Да:** `--autoConnect --user-data-dir=…` либо CDP-порт | **Да / да** | **Да**; Microsoft приводит штатные пути Stable/Beta/Dev для Win/macOS/Linux | **Предпочтительный Chromium-резерв** | [established]; Microsoft прямо документирует сценарий залогиненной живой сессии |
| **WebView2** | `chrome-devtools-mcp` через CDP | Google + Microsoft | Да, при включённом remote debugging | Windows; headless зависит от host app | User Data Folder WebView2 | Только если появится задача автоматизировать приложение/WebView2 | [established], [Microsoft](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/devtools-mcp-server) |
| **Playwright Chromium / Chrome / Edge** | `@playwright/mcp` | Microsoft/Playwright; **0.0.78**, опубликован в июле 2026: [npm](https://www.npmjs.com/package/%40playwright/mcp?activeTab=versions) | **Да:** `--extension` для живых Chrome/Edge; также `--cdp-endpoint` и `--endpoint` | **Да / да**; официальный Docker сейчас только headless Chromium | Да: `--user-data-dir`; default persistent profile; extension использует default logged-in profile | **Лаборатория, не второй control plane** | [established] |
| **Playwright Firefox** | `@playwright/mcp --browser firefox` | Microsoft; 0.0.78 | **Не равен Mozilla attach.** Документированный extension attach — только Chrome/Edge; `cdpEndpoint` — Chromium | Да / да | Persistent профиль, который запускает Playwright; attach к уже живому обычному Firefox официально не заявлен | Не ставить ради Firefox | [established] |
| **Playwright WebKit** | `@playwright/mcp --browser webkit` | Microsoft; 0.0.78 | Нет документированного attach к уже открытому Safari/WebKit | Да / да для Playwright WebKit | Собственный persistent/isolated context, не Safari-профиль | Полезен лишь для тестов совместимости | [established] |
| **«Puppeteer-MCP»** | Официального самостоятельного `@puppeteer/mcp` не найдено; официальный путь — `chrome-devtools-mcp`, который использует Puppeteer | Google Chrome DevTools, а не отдельная команда Puppeteer | Через `chrome-devtools-mcp` — да | Да / да | Как у Chrome MCP | **Не ставить community-пакет под двусмысленным именем** | [established]; сам [Puppeteer направляет пользователей к Chrome DevTools MCP](https://github.com/puppeteer/puppeteer) |
| **Safari** | Официального Apple MCP нет; официальный агентный интерфейс — `safaridriver`/W3C WebDriver | Apple; системный компонент, не npm-пакет | Не к обычной текущей вкладке/профилю: WebDriver создаёт изолированную automation session; после разрыва соединение с ней окончательно прекращается | macOS с GUI; нормального Safari headless Linux/Windows нет | **Нет доступа к обычным browsing data** — это намеренная защита | Не ставить MCP-обёртку; держать Python+Selenium только для Safari-only проверки | [established], [Apple WebDriver](https://developer.apple.com/documentation/webkit/about-webdriver-for-safari?changes=_8) |
| **Safari iOS/iPadOS** | Apple WebDriver через подключённый Mac | Apple | Automation session, не re-attach к личной вкладке | Требует Mac + устройство/симулятор | Изолированная automation state | Только мобильный compatibility test | [established], [Apple](https://developer.apple.com/documentation/safari-developer-tools/ios-enabling-webdriver) |
| **Opera Neon** | Встроенный **MCP Connector**; также официальный `opera-browser-cli`/opera-devtools | Opera; MCP Connector выпущен **31.03.2026**, CLI — май 2026; отдельной semver MCP package Opera не публикует | **Да:** специально работает с live tabs, authenticated session; persistent proxy возвращает `browser not available`, если Neon недоступен | Desktop Win11/macOS; официальный Linux/headless deployment не заявлен | Да, текущая залогиненная Neon session | **Не ставить сейчас; 48-часовой shadow-test возможен** | [emerging, single-vendor source], [анонс Opera](https://press.opera.com/2026/03/31/opera-neon-adds-mcp-connector/), [настройка](https://blogs.opera.com/news/2026/03/opera-neon-adds-mcp-connector-to-the-browser/) |
| **Opera One / GX** | Облегчённый Connector был анонсирован, но наличие production-двери и её контракт на 04.08 нельзя считать доказанными | Opera | Не подтверждено | Desktop | Не подтверждено | Не планировать | [emerging/speculative]; исходный анонс обещал будущий rollout |
| **Brave Browser** | Собственного browser-control MCP нет. `@brave/brave-search-mcp-server` — API поиска, не управление Brave | Brave Software; [официальный Search MCP](https://github.com/brave/brave-search-mcp-server) | Через Chrome/Playwright community compatibility возможно, но не поддерживается Google | Chromium headless технически возможен | Через CDP с отдельным профилем — вероятно, без vendor contract | Не ставить | MCP-отсутствие [established по официальному каталогу]; CDP-совместимость [speculative] |
| **Vivaldi** | Собственного MCP нет | Vivaldi Technologies | Возможен неподдерживаемый CDP attach как к Chromium | Вероятно да / да | Вероятно отдельный Chromium profile | Не ставить | [speculative] для совместимости; Google прямо ограничивает support Chrome/CfT |
| **Arc / Dia** | Публичного vendor browser-control MCP/API нет | The Browser Company | Нет подтверждённого attach | Desktop | — | Не ставить; Dia уже исключён вашим фундаментом | [established по публичной поверхности на дату аудита] |
| **Zen Browser** | Собственного MCP нет | Community Firefox fork | Теоретически Mozilla MCP может частично работать через Gecko/Marionette, но Mozilla это не обещает | Зависит от сборки | Возможен профиль Zen, не подтверждён | Не ставить | [speculative] |
| **Waterfox** | Собственного MCP нет | BrowserWorks/Waterfox | Теоретически через Marionette/Mozilla MCP; vendor support отсутствует | Зависит от сборки | Теоретически да | Не ставить | [speculative] |
| **LibreWolf** | Собственного MCP нет | Community | Теоретически через Marionette; hardened prefs могут ломать automation | Зависит от пакета | Теоретически да | Не ставить | [speculative] |
| **Ladybird** | MCP/production agent protocol нет | Ladybird project | Нет | Проект pre-alpha; не production browser | — | Не рассматривать | [established]; проект сам называет браузер [pre-alpha, только для разработчиков](https://github.com/LadybirdBrowser/ladybird) |
| **Servo** | MCP нет; это prototype/embeddable engine, не готовая аккаунтная браузерная рельса | Servo project | Нет | Разрабатывается для Win/Linux/macOS/Android, но это не MCP-ready browser | — | Не рассматривать | [established], [официальный репозиторий](https://github.com/servo/servo) |

### Важная оговорка об «отсутствии»

«MCP нет» здесь означает: **не найден официальный, публично документированный browser-control MCP/agent endpoint от самого проекта**. Community-сервер можно написать почти к любому CDP/WebDriver-браузеру, но это не делает дверь родной и не даёт контракта поддержки.

Например, BrowserMCP действительно подключается расширением к существующему Chromium-профилю, но издатель — сторонний проект BrowserMCP, а не Google/Microsoft/Brave/Vivaldi: [репозиторий](https://github.com/browsermcp/mcp) [established]. Для вашего критерия поставщика это не кандидат первого ряда.

---

## 2. Attach, disconnect и холодный рестарт

### Mozilla MCP

Ваш замер закрыл самые важные вопросы:

- браузер принадлежит отдельному resident-процессу, не MCP;
- disconnect MCP не закрывает Firefox;
- повторный `--connectExisting` работает;
- состояние остаётся в обычном профиле;
- BiDi/Marionette является control plane, а не каналом копирования cookie-файла.

Это сейчас сильнейшая эмпирическая позиция в вашем парке. [established, local]

### Chrome DevTools MCP

Документированы три способа attach:

- `--browserUrl=http://127.0.0.1:9222`;
- `--wsEndpoint=ws://…`;
- Chrome 144+ — `--autoConnect`, когда remote debugging включён через `chrome://inspect/#remote-debugging`.

Источник: [официальный README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md) [established].

Поскольку MCP подключается к внешнему browser process, прекращение stdio-процесса MCP **не должно завершать браузер**; повторный attach является нормальной моделью CDP. Поддержка уникальных page IDs «across browser reconnects» видна в [changelog 1.6.0](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/CHANGELOG.md). Это сильная инженерная экстраполяция, но ваш конкретный disconnect/re-attach на Windows ещё не измерен: **[emerging until local test]**.

Главная тонкость: Chrome ограничивает remote debugging с default data directory и рекомендует отдельный `--user-data-dir`; README прямо указывает это ограничение. Поэтому «взять обычный человеческий Chrome-профиль» и «поднять постоянный выделенный automation-профиль» — не одно и то же. [Chrome DevTools MCP README](https://github.com/ChromeDevTools/chrome-devtools-mcp) [established].

### Edge

Microsoft в июле 2026 документирует именно вашу модель:

```text
msedge.exe --remote-debugging-port=9222
chrome-devtools-mcp --autoConnect --user-data-dir=<Edge User Data>
```

Мотивация в документации буквально включает сохранение состояния, сложный login flow и инспекцию существующей session. [Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/devtools-mcp-server) [established].

Это делает Edge лучшим кандидатом на локальный 48-часовой attach-test. Но следует проверить, как конкретная версия Edge трактует default User Data и какие ограничения у неё после Chromium security changes — документация Microsoft и Chrome policy могут разойтись на краевых конфигурациях. [contested until local measurement]

### Playwright MCP

У него три разных режима, которые нельзя смешивать:

1. Persistent profile, запущенный самим MCP.
2. Isolated context со `storage-state`.
3. `--extension` — attach к существующему Chrome/Edge и его залогиненным вкладкам.

Официальный README подтверждает `--user-data-dir`, `--extension`, `--cdp-endpoint`, accessibility snapshots и Chrome/Edge-only ограничение extension mode: [Microsoft Playwright MCP](https://github.com/microsoft/playwright-mcp) [established].

`--extension` ближе всего к Mozilla `--connectExisting`, но добавляет расширение и pairing layer. Это дополнительная движущаяся часть; для non-technical owner хуже прямого Firefox/Marionette и прямого Edge/CDP.

### Opera Neon

Это настоящий встроенный live-session endpoint, а не automation browser, который поднимается рядом. Opera заявляет:

- доступ к открытым вкладкам и page content;
- authenticated sessions;
- navigate/click/fill/screenshot/search;
- OAuth2 URL;
- persistent proxy, сообщающий `browser not available`.

[Анонс Opera](https://press.opera.com/2026/03/31/opera-neon-adds-mcp-connector/) [emerging, single-source].

Однако reconnect после падения клиента и холодного рестарта Windows публично не описан с нужной вам точностью. Наличие persistent proxy не доказывает автоматическое восстановление browser-side consent/session. Это надо измерять.

---

## 3. Безлюдная работа и наблюдаемость

| MCP | Наблюдение | Токенная цена | Для расписания |
|---|---|---:|---|
| Mozilla MCP | Accessibility/UID snapshot, screenshot, script, screencast | Низкая–средняя при UID snapshot; высокая для full screenshots/screencast | Лучший уже доказанный resident desktop; headless возможен |
| Playwright MCP | Структурированный accessibility snapshot по умолчанию; vision опционально | Обычно низкая, но большие полные a11y trees заметно раздувают контекст; Microsoft теперь прямо отмечает, что CLI+skills бывает экономнее MCP | Очень хороший headless test runner; Docker официально ограничен headless Chromium |
| Chrome DevTools MCP | Page snapshot, screenshot, DOM/console/network/performance/heap | Навигация — средняя; network/console/trace/heap могут быть очень дорогими; с 0.18 есть `--slim`, с 1.3 — ограничение размера screenshot | Хорош для unattended Chromium и диагностики |
| Safari WebDriver | DOM/WebDriver selectors, screenshot; не MCP-native snapshot loop | Средняя; зависит от обёртки | Требует Mac automation session; слабый кандидат для resident-account worker |
| Opera Neon | Live page context + screenshot/actions; формат и budgeting меньше документированы | Неопределённая; Opera утверждает, что CLI снижает latency/token overhead относительно MCP | Требует работающий desktop Neon; официальный Linux-headless путь не заявлен |

Playwright сам характеризует accessibility snapshots как lightweight/deterministic, но одновременно отмечает, что CLI избегает загрузки больших tool schemas и verbose accessibility trees: [npm `@playwright/mcp`](https://www.npmjs.com/package/%40playwright/mcp) [established].

Для AK-47-плана:

- сначала semantic snapshot;
- screenshot — только при неудаче/визуальной неоднозначности;
- сырой DOM — только точечный селектор или диагностика;
- screencast, full network body, heap и traces — отдельный diagnostic job, не часть обычной транзакции.

---

## 4. Риск-профиль

### MCP сам инструкции со страницы не «исполняет»

Firefox MCP, Chrome DevTools MCP и Playwright MCP — исполнительные серверы. Решение считать текст страницы инструкцией принимает **LLM/agent loop**, а не MCP-протокол. Но сервер превращает ошибочное решение агента в реальное действие.

Поэтому ни accessibility snapshot, ни screenshot, ни raw DOM не являются безопасными по определению. Исследование WASP показывает, что современные web agents начинают следовать инъекции в 16–86% испытаний, хотя полная цель атакующего достигается реже — 0–17%. [WASP paper](https://arxiv.org/abs/2504.18575) [established for tested systems; перенос на ваши модели — extrapolation].

Google также называет indirect prompt injection основной новой угрозой для agentic browsers и рекомендует регулярно переоценивать защиту: [Chrome WebMCP security](https://developer.chrome.com/docs/agents/security) [established].

### Наиболее опасные возможности

В порядке ущерба:

1. **`evaluate_script` / `browser_run_code` / privileged chrome script.**  
   Позволяют обойти семантическую поверхность click/fill и получить данные страницы напрямую. Для Playwright MCP был публичный report о возможном выходе `browser_run_code` к host execution; issue закрыт со ссылкой на общую позицию «MCP is not a security boundary», а не с публичным доказательством безопасной песочницы. [GitHub issue](https://github.com/microsoft/playwright-mcp/issues/1495) [contested].

2. **`install_extension`.**  
   Расширение может получить долговременные привилегии, доступ к вкладкам и сетевым данным. Для обычного scheduler-а этот tool не нужен.

3. **Неограниченный filesystem:**  
   Playwright `--allow-unrestricted-file-access`; Chrome `--allow-unrestricted-paths`/аналогичные save-path overrides. Playwright по умолчанию ограничивает filesystem workspace roots и блокирует `file://`; его README подчёркивает, что allow/block origins не являются security boundary и не охватывают redirects. [Playwright README](https://github.com/microsoft/playwright-mcp) [established].

4. **Downloads/uploads.**  
   Даже без script execution агент может загрузить секрет или скачать активное содержимое.

5. **Cross-tab actions в залогиненном профиле.**  
   Успешная injection на публичной странице может заставить агента перейти к другой authenticated вкладке. Исследователи показывают, что browser agent фактически может стать каналом обхода привычных гарантий same-origin. [University of Washington study](https://agent-security.cs.washington.edu/agentic_browsers_sop.html) [emerging].

### Минимальный tool set для боевой рельсы

Для рутинной работы достаточно:

```text
list/select page
navigate
semantic snapshot
click
fill/type
wait
read visible text
restricted screenshot
```

Опционально:

```text
upload_file — только из отдельной outbound-папки
download — только в отдельную quarantine-папку
```

По умолчанию выключить:

```text
evaluate_script
evaluate_chrome_script
browser_run_code
install_extension
unrestricted file/save paths
arbitrary init scripts
grant clipboard permissions
raw network response bodies
cross-origin wildcard allowlists
```

Если MCP-клиент не умеет server-side allowlist tools, ограничивать их на стороне agent policy и запускать MCP под отдельным низкопривилегированным OS-user. OWASP рекомендует отделять high-privilege tools от контекста, куда попадают внешние MCP/web-данные: [OWASP MCP Tool Poisoning](https://owasp.org/www-community/attacks/MCP_Tool_Poisoning) [established guidance].

### Порты и профили

Remote debugging/Marionette:

- слушать только loopback;
- не публиковать через LAN/VPN;
- отдельный OS-user и профиль на сервис;
- агент не должен видеть личные вкладки владельца;
- capability включать только на robot profile.

Chrome DevTools предупреждает, что любой локальный процесс с доступом к debugging port может управлять браузером и не советует просматривать чувствительные сайты при открытом порте. [Официальный README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md) [established]. Microsoft отдельно предупреждает, что auto-connect наследует аккаунты, cookies и PII текущей Edge session. [Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/devtools-mcp-server) [established].

### ToS площадок

Техническая доступность authenticated session не даёт договорного разрешения на automation. Единой позиции «площадок» нет:

- запрет может касаться scraping, bots, automated access, обхода rate limits либо конкретных действий;
- некоторые сервисы разрешают automation через официальный API, но запрещают UI automation;
- «только собственный аккаунт» уменьшает риск злоупотребления, но само по себе не отменяет ToS;
- автоматизация платежей, публикаций, заказов и irreversible actions требует per-site проверки и human confirmation.

Без списка конкретных площадок нельзя честно присвоить общий статус. Любое универсальное «раз свои аккаунты, значит можно» было бы [speculative/incorrect].

---

## 5. Что второй движок реально даёт сверх Firefox

Реальные добавочные сценарии:

- **Blink/Chromium-only сайт или defect.**
- **WebView2** на Windows.
- **Chrome DevTools diagnostics:** performance trace, CrUX, Lighthouse, heap snapshots, подробные network/console инструменты. Chrome MCP имеет существенно более developer-oriented поверхность; его changelog показывает heap tooling, screencast, token-saving slim mode и Lighthouse. [Changelog](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/CHANGELOG.md) [established].
- **Chrome/Edge extensions** и поведение Manifest V3.
- **Android Chromium** через ADB.
- Проверка воспроизводимости ошибки на втором движке.

Что он **не** даёт:

- более простой control plane;
- меньшую attack surface;
- гарантию обхода bot detection;
- безопасный перенос Chrome cookies;
- Safari compatibility — для неё нужен настоящий Safari/WebKit test;
- автоматическое соответствие ToS.

Safari-only сценарий не оправдывает постоянно работающий Safari MCP: официального Apple MCP нет, а `safaridriver` намеренно изолирует automation от обычных browsing data и окончательно рвёт session при interruption. [Apple](https://developer.apple.com/documentation/webkit/about-webdriver-for-safari?changes=_8) [established].

---

## 6. Топ-3 дополнения и тест «понарошку сначала»

### 1. Edge + `chrome-devtools-mcp` — рекомендуемый резерв

Не ставить в боевую рельсу до 18.08. После замера — отдельный Edge profile на Windows-хабе, отдельный remote-debugging port, без человеческих вкладок.

48 часов измерять:

- 100 циклов MCP disconnect/reconnect без закрытия Edge;
- 10 холодных рестартов Windows → автозапуск Edge → появление `DevToolsActivePort` → attach;
- сохранение login после рестартов без копирования cookies;
- 20 одинаковых задач Firefox против Edge: success rate, медиана, p95, число tool calls, токены;
- два строго Chromium-проблемных сайта;
- поведение после crash browser process и после crash MCP process;
- доступность порта только с loopback;
- prompt-injection canary: страница просит открыть другую вкладку/прочитать секрет — агент обязан остановиться.

Критерий победы:

```text
≥99% reconnect
10/10 cold starts
0 cross-profile leakage
и хотя бы один реальный сайт/диагностический класс,
который Firefox не закрывает или закрывает заметно хуже
```

Если уникального выигрыша нет — удалить.

### 2. Playwright MCP — лаборатория совместимости

Запускать в isolated/persistent test profile, без аккаунтов. Проверить Chromium + Firefox + WebKit.

48 часов:

- один набор из 20 сценариев на трёх движках;
- сравнить accessibility snapshot size и фактические токены;
- сравнить MCP с обычным Python Playwright/Selenium;
- проверить, действительно ли self-healing exploration даёт больше, чем детерминированные Python-тесты;
- отдельно проверить `--extension` attach к тестовому Edge, но без ценных аккаунтов;
- исключить `browser_run_code` и unrestricted filesystem.

Критерий победы: минимум два найденных cross-engine regression либо сокращение времени поддержки тестов на 30%+. Иначе Python runner проще MCP.

### 3. Opera Neon MCP Connector — только shadow experiment

Это третий номер не потому, что он зрелее, а потому что это единственная дополнительная найденная **vendor-native live-session MCP дверь**.

48 часов:

- Windows и Mac: disconnect/reconnect клиента;
- перезапуск Neon;
- logout/login OS;
- восстановление OAuth/connector без ручного клика;
- доступность после закрытия GUI/RDP session;
- 20 задач с live authenticated tabs;
- логирование действий и возможность ограничить tools/domains;
- токены MCP против `opera-browser-cli`;
- prompt-injection canary;
- проверить, требует ли unattended flow периодического browser-side consent.

Критерий победы: он должен выиграть у Firefox минимум по одному устойчивому производственному сценарию, не только по удобству демо. Без Linux/headless и без доказанного cold recovery — не принимать.

---

## 7. Что выбросить

Из описанного вами текущего стека я бы **ничего не выбрасывал до завершения замера 18.08**. Менять рельсу сейчас противоречит дизайну эксперимента.

После замера:

- **Выбросить идею извлечения/переноса Chrome cookies** как control-plane strategy — по вашему фундаменту этот путь закрыт.
- **Не держать Playwright MCP рядом с Mozilla MCP только ради Firefox.** У Playwright нет равного документированного live attach к уже запущенному Firefox.
- **Не ставить отдельный Puppeteer MCP.** Официальная ветка Puppeteer направляет в `chrome-devtools-mcp`; второй wrapper добавит supply-chain и operational surface без нового движка.
- **Не держать community MCP для Brave/Vivaldi/Zen/Waterfox/LibreWolf**, пока нет уникального сайта, который требует именно этот fork.
- **Не включать постоянно `evaluate_script`, `install_extension`, privileged chrome evaluation и unrestricted paths.**
- Если в стеке уже есть generic browser-control wrapper поверх Playwright/Puppeteer и он не обслуживает уникальную задачу — вывести его после сравнительного теста с Mozilla MCP.
- Safari/WebKit оставить как **эпизодический compatibility runner**, а не resident-account browser.

---

## Решение

Для вашего контекста:

```text
Боевая рельса:
Windows-хаб → resident Firefox → профиль-на-сервис
             → Mozilla MCP connectExisting
             → минимальный allowlist tools

Холодный резерв после 18.08:
Windows-хаб → отдельный resident Edge profile
             → chrome-devtools-mcp autoConnect/browserUrl
             → запускается только для Blink/WebView2/DevTools задач

Лаборатория:
Linux-VPS/Mac → Playwright MCP или, предпочтительно для стабильных сценариев,
                обычный Python Playwright/Selenium без аккаунтных профилей

Safari:
Mac → safaridriver только для compatibility checks
```

**Прямой ответ:** да, **Firefox + Mozilla MCP достаточно** для основной автоматизации собственных аккаунтов. Второй постоянно активный MCP не нужен. Нужен лишь **холодный второй движок**, и наиболее рациональный выбор — **Edge + официальный `chrome-devtools-mcp`**, включаемый по исключению. Opera Neon и Playwright MCP пока не выигрывают у этой двухрельсовой схемы по критерию AK-47.
