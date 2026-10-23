# DR26-08-04-ZB-13-2207 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сверяю актуальную карту браузерных MCP: издатели, attach к живому браузеру, headless и риски — без пересмотра вашего уже закрытого Firefox-фундамента.Тяну первичные доки: Chrome DevTools MCP, Playwright MCP, Safari MCP и npm-страницы.# Карта браузерных MCP (август 2026) — решение под ваш стек

Фундамент (куки, Firefox+Marionette attach, отклонённые Comet/Atlas/Dia/Leo/облака) **не пересматриваю**. Ниже — только «есть ли ещё родная дверь сверх Лисы» и нужна ли она.

---

## 1) Таблица: браузер → MCP → attach → headless → профиль → ставим?

Уверенность: **E** = established (офиц. док/npm), **Em** = emerging (свежий vendor, мало полевых замеров), **S** = speculative, **C** = contested.

| Браузер / слой | MCP / протокол | Издатель | Версия / свежесть (npm или vendor) | Attach к живому | Headless Win/Linux | Профиль на диске | Ставим? | Почему |
|---|---|---|---|---|---|---|---|---|
| **Firefox** | `@mozilla/firefox-devtools-mcp` | Mozilla (`github.com/mozilla/firefox-devtools-mcp`, npm scope `@mozilla`) | **0.9.15** (npm, ~28.07.2026) | **Да** — `--connect-existing` + `--marionette-port` | **Да** (`--headless`) | **Да** (`--profile-path`) | **ДА (боевая)** | У вас уже ✅ attach + 31 tool + re-attach. Офиц. warning: не оставлять Marionette на обычном сёрфинге. [E] |
| **Chrome** | `chrome-devtools-mcp` | Google Chrome DevTools (`ChromeDevTools/chrome-devtools-mcp`) | **1.6.0** (npm 14.07.2026; репо активно 04.08) | **Да** — `--browserUrl` / `--wsEndpoint` / `--autoConnect` (Chrome ≥144) | **Да** (`--headless`); slim-mode | **Да** (`--userDataDir`); default cache-profile; `--isolated` | **SHADOW only** | Офиц. attach mature. Но ваш cookie-слой: Chrome 127+ ABE + DBSC 146 → **не замена** Firefox для «живой сессии из кук». Официально только Chrome/Chrome for Testing. [E] |
| **Chromium multi** | `@playwright/mcp` | Microsoft Playwright | **0.0.78** (npm **04.08.2026** — живой) | **Частично** — `--cdp-endpoint` (Chromium family); `--extension` (живой Chrome/Edge + extension); **не** Firefox-CDP (CDP снят ~141+) | **Да** + Docker headless chromium | **Да** (`--user-data-dir`); default ms-playwright cache; `--isolated` / `--storage-state` | **SHADOW #2** | Сильнейший a11y-snapshot (дешевле токенов). Multi-engine QA. Не control-plane для ваших логинов. [E] |
| **Safari / WebKit** | `safaridriver --mcp` | Apple (WebKit blog 01.07.2026; STP 247 / Safari 27 beta) | Встроен в STP/Safari 27 beta | **К «Safari window» для debug**, не «ваш ежедневный профиль с паролями» (vendor: нет доступа к AutoFill/personal Safari data; community: isolated WebDriver session) | **Нет** на Win/Linux; только macOS | Не ваш user Safari profile | **Нет (кроме Mac-Safari debug)** | Mac-only. Не для хаба/VPS. [E]/[Em] isolation] |
| **Edge** | Своего MCP **нет** | — | — | Через Playwright `--browser msedge` или community CDP | Как Chromium | Как Chromium | **Нет** | Наследует Chromium-стек, не свою дверь. [E] |
| **Brave** | **Browser MCP нет**; есть `@brave/brave-search-mcp-server` **2.1.0** | Brave (search API) | Search MCP, не browser | N/A | N/A | N/A | **Нет (browser)** | Search ≠ control plane. Leo — UI человека (уже rejected). [E] |
| **Opera Neon** | **Built-in MCP Connector** (не npm-пакет) | Opera (31.03.2026) | Product feature; paid Neon | **Да — live session** (вкладки, логины, click/fill) | **Нет** headless-агент; headful + proxy «browser not available» | Живой UI-профиль Neon | **Нет сейчас** | Attach красивый, но: подписка, второй браузер, write-tools opt-in, не headless Linux, IP-sensitive ≠ ваш resident hub FF. [E] |
| **Puppeteer** | Официального **Puppeteer-MCP нет**; Google указывает на `chrome-devtools-mcp` | Google Puppeteer docs → chrome-devtools-mcp | Community: `puppeteer-mcp-server` **0.7.2** (stale 03.2025) | Community «existing Chrome» — разное качество | Зависит от форка | Зависит | **Нет** | Дубль Chrome DevTools MCP; community stale. [E] |
| **Arc / Dia** | Vendor MCP **нет** | The Browser Company | — | Нет публичного agent-API (Dia уже rejected) | — | — | **Нет** | Maintenance Arc; Dia — skills/UI, не MCP-дверь. [E/Em] |
| **Vivaldi** | Vendor MCP **нет** | — | — | Только community Chromium-CDP | — | — | **Нет** | Нет родной двери. [E] |
| **Zen / Waterfox / LibreWolf** | Vendor MCP **нет** | Firefox-family forks | — | Возможно наследуют Marionette/BiDi (S) | S | S | **Нет** | Нет официального MCP; поддержка Marionette не обещана вендором fork. [S] |
| **Ladybird / Servo** | MCP **нет** | — | pre-alpha engines | Нет | Нет | Нет | **Нет** | Нет agent-door. [E] |
| **Android mobile** | Внутри Firefox MCP: `--android-device` + ADB | Mozilla | same 0.9.15 | Attach к FF Android | N/A (device) | app package | **Уже есть в FF MCP** | Не нужен второй браузерный MCP. [E] |

Источники ключевых строк:  
Chrome DevTools MCP / `--browserUrl` / `--autoConnect` / `--userDataDir` / `--headless` / security disclaimer — [github.com/ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp), [developer.chrome.com/docs/devtools/agents](https://developer.chrome.com/docs/devtools/agents/get-started).  
Firefox MCP / `--connect-existing` / Marionette warning / tool presets / Android — [github.com/mozilla/firefox-devtools-mcp](https://github.com/mozilla/firefox-devtools-mcp).  
Playwright MCP / a11y / CDP / extension / headless Docker — [github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp).  
Safari MCP — [webkit.org/blog/18136](https://webkit.org/blog/18136/introducing-the-safari-mcp-server-for-web-developers/).  
Opera Neon MCP Connector — [blogs.opera.com 31.03.2026](https://blogs.opera.com/news/2026/03/opera-neon-adds-mcp-connector-to-the-browser/), [press.opera.com](https://press.opera.com/2026/03/31/opera-neon-adds-mcp-connector/).  
Brave Search MCP (не browser) — [github.com/brave/brave-search-mcp-server](https://github.com/brave/brave-search-mcp-server).  
npm versions: `[email hidden]`, `@playwright/[email hidden]`, `@mozilla/[email hidden]`, `[email hidden]` (registry 04.08.2026).

---

## 2) Критерий attach (то, что вы доказали на Лисе)

| MCP | Аналог `--connectExisting` | Браузер переживает disconnect MCP? | Re-attach | Залогиненный профиль |
|---|---|---|---|---|
| **Firefox Mozilla** | `--connect-existing` + Marionette+BiDi | **Да** (ваш замер 04.08) | **Да** | `--profile-path` или connect к уже поднятому |
| **Chrome Google** | `--browserUrl=http://127.0.0.1:9222`, `--wsEndpoint`, `--autoConnect` (Chrome 144+) | **Да по дизайну** (browser отдельный от MCP) [E doc; не ваш замер] | **Да** | `--userDataDir` **или** remote-debug с non-default profile (Chrome security: remote-debug не на default profile без оговорок) |
| **Playwright MS** | `--cdp-endpoint` / `--extension` (Chrome/Edge only) | CDP session: browser живёт; MCP exit ≠ kill browser [E/S] | Да при CDP | `--user-data-dir` / `--storage-state` / extension → живые вкладки |
| **Safari Apple** | safaridriver session | WebDriver lifecycle; не «permanent daily Safari» | session-bound | **Не** personal profile [E Apple privacy note] |
| **Opera Neon** | Built-in connector к live Neon | Browser = host; MCP via URL+proxy | Да пока Neon/proxy up | **Да — live logins** (главный маркетинг) [E] |

**Вывод attach:** по «резидентный браузер + re-attach + профиль» зрелые родные двери **только три**: Firefox (доказано у вас), Chrome DevTools (документально зрелый, cookie-слой у вас мёртв для «снаружи»), Opera Neon (live, но продукт-подписка, не headless-флот). Playwright — hybrid (CDP/extension/storage-state), сильнее как harness, слабее как «один forever-профиль владельца».

---

## 3) Безлюдная работа (Windows hub + headless Linux VPS)

| | Night/cron headless Linux (no X) | Headless Win | Cold restart | Observability / токены |
|---|---|---|---|---|
| **Firefox MCP** | Да (`--headless`); Docker в репо | Да | Поднять FF + MCP; attach mode = FF отдельно | `take_snapshot` (a11y-ish UID) + screenshot + script; `saveTo` режет контекст [E] |
| **Chrome MCP** | Да + `--slim --headless` | Да | browserUrl / launch | `take_snapshot` + screenshot + full DevTools suite; usage stats on by default (`--no-usage-statistics`) [E] |
| **Playwright MCP** | **Сильнейший** (Docker `mcr.microsoft.com/playwright/mcp`, pure a11y) | Да | launch / CDP | **Accessibility tree = token-cheap**; vision optional; MS прямо пишет CLI+SKILLS ещё дешевле MCP для coding agents [E] |
| **Safari** | Нет | Нет | — | 16 tools, screenshots + evaluate_js [E] |
| **Opera Neon** | Нет (headful + subscription machine) | Нет unattended | «browser not available» | UI-context; write tools off by default [E] |

Для **Маяка (headless Linux)**: Playwright MCP или Chrome/Firefox headless. Для **IP-sensitive с resident IP хаба**: только ваш resident Firefox (не облако, не Neon на другой машине).

---

## 4) Риск-профиль

### Prompt injection (класс CometJacking)
- Класс атак **доказан** на agentic-browsers (Comet / URL-query / hidden page text) — Brave, LayerX, Trail of Bits.  
- **Любой** browser-MCP, который отдаёт page content + `evaluate_script` + click/fill в **залогиненную** сессию, — тот же класс: page = untrusted, tool-call = trusted. Это не «Comet-only». [E class / Em per-tool]

### Защиты вендоров (что реально есть)
| | Что пишут | Практический минимум |
|---|---|---|
| **Mozilla** | Dedicated profile; осторожность к page content; tool presets; **не оставлять Marionette** (webdriver=true, bot-detection) [E README] | `--tool-preset slim` или `--tools pages snapshot input` **без** `script`/`privileged`/`webextension` для рутин |
| **Google Chrome MCP** | Agent видит всё в session; avoid sensitive; `--blockedUrlPattern` / `--allowedUrlPattern`; slim 3 tools; path restriction (`--allowUnrestrictedPaths` off) [E] | `--slim` или category flags; never daily personal profile |
| **Playwright** | «Not a security boundary»; allow/block origins **не** security boundary; `browser_run_code_unsafe` = RCE-equivalent [E] | Не включать unsafe/code; secrets redaction — convenience only |
| **Safari** | Local only, no Apple exfil of personal Safari data; data goes to **your agent** [E] | Только debug URLs |
| **Opera Neon** | Default **read-only** tools; write opt-in; OAuth MCP URL [E] | Лучший default least-privilege UI, но всё равно page injection |

### Опасные тулы по построению
- `evaluate_script` / `browser_evaluate` / `evaluate_javascript`  
- `install_extension` / extension triggers  
- `allowUnrestrictedPaths` / `--unrestricted-save-paths` / `--allow-unrestricted-file-access`  
- Playwright `browser_run_code_unsafe`  
- Privileged/chrome-context (Mozilla internal)

**Минимальный набор для ваших рутин (свои аккаунты, AK-47):**  
`list_pages` · `navigate` · `take_snapshot` · `click/fill by uid` · `screenshot` (редко) · **без** script/extension/privileged.

### ToS / площадки
- Автоматизация **своих** аккаунтов через WebDriver/`navigator.webdriver` — серая зона: Cloudflare/Akamai детектят Marionette (Mozilla сама пишет). [E vendor warning]  
- Это не «запрет закона» на свои данные, но **бан-риск** на IP-sensitive (FB/Reddit/и т.п.) — у вас уже рельса «только хаб + resident IP». Второй MCP **не снимает** ToS-риск, может усилить fingerprint.

---

## 5) Что даёт сверх Лисы? Честно

| Сценарий | Нужен второй MCP? | Какой |
|---|---|---|
| Повседневные логины / IP-sensitive / resident hub | **Нет** | Firefox |
| Сайт «только Chromium / ломается в Gecko» | **Да, точечно** | Chrome MCP **или** Playwright Chromium, **отдельный** profile, **не** кража Chrome-кук |
| Safari-only баг / iOS WebKit | **Да, Mac-only** | Safari MCP (debug, не ops) |
| Mobile | **Нет** | Firefox MCP `--android-device` |
| WebExtension dev | **Редко** | Firefox (install_extension) или Chrome categoryExtensions |
| Headless Linux QA / a11y-дешёвые токены | **Опционально** | Playwright MCP |
| «Ещё один браузер потому что модно» | **Нет** | — |

**Второй MCP — не обязательная сущность.** Он оправдан только как **fallback-engine** и **token-cheap harness**, не как второй control plane.

---

## DELIVERABLE (2) Топ-3 дополнения + «понарошку 48ч»

### #1 `chrome-devtools-mcp` — Chromium emergency lane  
**Зачем:** строго-Chromium сайты, которые Gecko не тянет; DevTools/performance.  
**Не зачем:** «ещё одна живая сессия с куками» (ABE+DBSC).  

**48ч shadow (критерии ДО старта):**
1. Поднять Chrome **отдельный** `--user-data-dir` + `--remote-debugging-port=9222` (не default daily).  
2. `npx chrome-devtools-mcp@latest --browser-url=http://127.0.0.1:9222 --no-usage-statistics --slim`.  
3. Метрики: attach success ≥3/3; re-attach после kill MCP; login **руками** в этот profile 1 тестовый сайт; 1 сценарий, где FF failed last 30d.  
4. **Win-критерий:** ≥1 реальный «FF не смог, Chrome смог» **или** kill кандидата.  
5. **Не** трогать IP-sensitive production в замер Firefox 04→18.08.

### #2 `@playwright/mcp` — headless multi-engine + дешёвые snapshot  
**Зачем:** Маяк Linux, a11y snapshots, Firefox/**webkit**/chromium **без** reusing production cookies (`--storage-state` export).  

**48ч shadow:**
1. `npx @playwright/mcp@latest --headless --browser chromium` на Linux (или Docker).  
2. Тот же smoke: navigate + snapshot + fill demo form.  
3. Сравнить **токены/ход** snapshot vs Firefox `take_snapshot` на одной странице (один URL, 5 прогонов, median).  
4. Win: ≥30% дешевле токенов **или** webkit-only smoke зелёный; иначе park.

### #3 Safari MCP — **только** MacBook, **только** debug  
**Зачем:** «как выглядит у Safari-юзера».  
**Не:** ops, headless, Windows hub.  

**48ч:** STP/Safari 27 beta + `safaridriver --mcp` + 3 debug-промпта на ваш публичный URL. Win = agent сам нашёл layout bug; иначе не ставить на флот.

**Opera Neon / community Puppeteer / Brave browser MCP / forks — не в топ-3** (подписка / stale / не browser / нет двери).

---

## DELIVERABLE (3) Что выбросить из текущего мышления

| Выбросить / заморозить | Почему |
|---|---|
| Идея «нужен парк 5 браузерных MCP» | AK-47: одна боевая рельса + 0–1 shadow |
| Community Puppeteer-MCP как «официальный» | Google сам → chrome-devtools-mcp; community stale |
| Brave / Arc / Dia / Vivaldi / Zen / Waterfox / LibreWolf / Ladybird / Servo как agent doors | **MCP нет** (или search-only) |
| Opera Neon как «вторая боевая» | Платный UI-browser, не headless-флот, размывает замер Firefox |
| Playwright `connectOverCDP` к Firefox | CDP снят; ваш фундамент |
| Извлечение Chrome-кук «как раньше» | ABE + DBSC — closed |
| Менять боевую рельсу до 18.08 | Явный stop вашего замера |

**Оставить:** Firefox resident + `@mozilla/firefox-devtools-mcp` attach; tool-preset **slim/basic** без script в cron; profile-per-service.

---

## DELIVERABLE (4) Прямой ответ

### **Одной Лисы + Mozilla MCP достаточно для боевого control plane.**

- Attach, re-attach, profile, headless, Android — закрыты.  
- Cookie/session слой совместим с вашей архитектурой.  
- Замер 04→18.08 **не ломать**.

### Второй движок — **не обязателен**. Если вводить — только:

1. **Chrome DevTools MCP** — emergency Chromium (отдельный profile, shadow).  
2. **Playwright MCP** — headless/token harness / multi-engine QA.  
3. Safari — только Mac debug.

**Рекомендация (кофаундер, high confidence на ops, med на «когда понадобится Chromium»):**  
**Сейчас — 1 рельса (Firefox).**  
**После 18.08 — если замер покажет ≥2 реальных Gecko-блокера в месяц → shadow Chrome 48ч по плану #1; иначе не плодить сущности.**

---

## За / против (без приговора)

| За вторую дверь | Против |
|---|---|
| Chromium-only breakage | Cookie control plane уже решён только на FF |
| A11y/token savings (Playwright) | Двойной ops, double fingerprint, double fail modes |
| Safari reality check | Safari ≠ Windows hub / VPS |
| Vendor maturity Chrome/Playwright | Замер Firefox нельзя рвать mid-flight |
| Neon live-attach UX | Paid + not headless + не ваш resident IP stack |

Консенсус рынка «всем нужен Chrome MCP» ≠ истина для **вашего** cookie+IP+AK-47 контекста.

---

🎙 **Майк:** Идём верно: не покупать «зоопарк MCP», купить **опцион** на Chromium и забыть. Единственный риск, который я вижу — вы начнёте «на всякий случай» ставить Playwright+Chrome+Neon параллельно боевой Лисе и сожжёте замер. Вопрос: после 18.08 хотите shadow Chrome на **одном** бан-чувствительном сайте, или сначала закрываем Firefox-метрики в ноль?

---

### 🧒 Простыми словами

У браузеров «пульт для робота» (MCP) есть нормальный только у **Лисы (Mozilla)**, **Хрома (Google)** и (частично) **Playwright (Microsoft)**. Ещё у **Safari на Маке** и **Opera Neon за деньги**. Остальные — почти все **без пульта**.

Вы уже доказали: Лиса умеет «подцепить уже открытый браузер и не убить его». Это главное.  
Второй пульт нужен **только если** какой-то сайт упрямо работает только в Хроме — и то **рядом**, не вместо.  
До 18 августа **ничего не меняем** в боевой рельсе.
