# DR26-11-11-ZB-01-1753 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: claude.ai Research (Fable 5.2 Extra)
chat: https://приватный чат
dr_id: DR26-11-11-ZB-01-1753

---

# Какие MCP-коннекторы реально подключать one-person AI lab (ноябрь 2026)

**Короткий ответ: в постоянный конфиг стоит добавить 6–8 vendor-hosted remote-серверов: Hugging Face, Cloudflare, Exa/Firecrawl, Calendly, Typefully и GitHub в режиме read-only с узким набором toolsets. GitHub при этом подключается как дополнение к `gh`, а не вместо него. X/Facebook/LinkedIn, ATS-борды и большую часть scholarly-стека держите на CLI/REST или подключайте ad hoc.** Самый большой выигрыш не в новых коннекторах. Он в том, чтобы перестать гонять deep-research fan-out через web UI ChatGPT/Claude/Grok/z.ai и перевести его на API/OpenRouter. Это вывод из анализа, а не из найденного измерения. Ниже всё по кластерам: что брать, что нет и почему.

## TL;DR

- **Брать сейчас (remote, OAuth, ваш спрос подтверждён):**
  - Hugging Face (`https://huggingface.co/mcp?login`);
  - Cloudflare Code Mode (`https://mcp.cloudflare.com/mcp`, 2 инструмента вместо ~2 500 эндпоинтов);
  - Exa (`https://mcp.exa.ai/mcp`, работает без ключа) и/или Firecrawl;
  - Calendly (`https://mcp.calendly.com`);
  - Typefully (`https://mcp.typefully.com/mcp`) как основной канал «пост в X с любой машины»;
  - GitHub remote MCP только в варианте `.../mcp/x/<toolset>/readonly`, как дополнение к `gh`.
- **Не брать в always-on:**
  - официальный X MCP: для записи нужен stdio-мост `xurl`, плюс pay-per-use биллинг X API;
  - Buffer: дублирует Typefully на ваших сетях;
  - LinkedIn: аккаунт забанен, community-серверы работают через скрейпинг;
  - Facebook Pages: официального MCP нет, есть только Ads;
  - Brave: только local/stdio, бесплатный tier на 2 000 запросов/мес убран 12.02.2026 (теперь $5 за 1 000 запросов и $5 бесплатных кредитов в месяц с обязательной картой; в октябре 2026 agentdeals.dev оспорил этот факт);
  - любые stdio-серверы для Reddit/Bluesky/Zenodo/ORCID/DataCite;
  - community-агрегаторы ATS-бордов.
- **GitHub MCP vs `gh`:** независимые замеры почти единогласно в пользу CLI по токенам. В замере OpenRouter («MCP vs CLI vs Skills», Claude Sonnet 5.5, GitHub MCP v2.0.2) `gh` занял 1 421 входных токенов против 59 417 у GitHub MCP с 46 инструментами по умолчанию: «the default MCP server used 42 times the CLI's input tokens and cost 17 times as much, $127 against $7.50 per 1,000 tasks». Единственный найденный контр-замер (Smithery) показывает лучшую надёжность MCP. Вывод: `gh` остаётся основным путём. Remote MCP подключайте только read-only и только с toolsets `issues,pull_requests,notifications`, для клиентов без shell (ChatGPT web, claude.ai).

---

## 1. Мастер-таблица

Легенда транспорта:
- **(a)** vendor-hosted remote HTTP/Streamable HTTP с OAuth;
- **(b)** self-hostable HTTP, один процесс на машину;
- **(c)** stdio-only: можно только ad hoc, в постоянный конфиг нельзя.

Дата наблюдения для всех строк: 11.11.2026.

| Коннектор | Кластер | Наша частота | Решение | Почему (одна строка) | Транспорт | Стоимость | Источник | Уверенность |
|---|---|---|---|---|---|---|---|---|
| GitHub remote MCP (`github/github-mcp-server`) | GitHub | gh CLI 2 349 calls / 305 sessions; github.com via shell 227 | **LATER/частично TAKE** (только read-only, узкие toolsets) | `gh` дешевле по токенам; MCP нужен только клиентам без shell | (a) `https://api.githubcopilot.com/mcp/` | Бесплатно на любом плане GitHub | https://www.scalekit.com/blog/github-mcp-server-per-user-oauth | high |
| `gh` CLI (статус-кво) | GitHub | 2 349 / 305 | **KEEP** | Модель знает `gh`, нулевая стоимость схем | shell | бесплатно | https://openrouter.ai/blog/insights/mcp-vs-cli-vs-skills/ | high |
| OpenRouter API (вместо web-UI fan-out) | LLM vendors | chatgpt.com 75, claude.ai 56, grok.com 49, z.ai 34, mistral 8; openrouter.ai via shell 10 | **TAKE (как REST/CLI, не MCP)** | Убирает риск автоматизации потребительских UI | REST | pay-per-token | prior art not found для MCP; блог OpenRouter: https://openrouter.ai/blog/insights/mcp-vs-cli-vs-skills/ | medium |
| Официальный X MCP (`api.x.com/mcp`) + `xurl` | X/Twitter | x.com 26+19; X official MCP = 0 | **DON'T TAKE (always-on)** / ad hoc | Запись требует локального stdio-моста `xurl`; оплата за каждый вызов | (a) для чтения по Bearer; запись через (c) `xurl mcp` | X API pay-per-use: $0.015/пост, $0.20/пост со ссылкой | https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558 ; https://revive.social/social-media-mcp/ | medium |
| Typefully MCP | Social schedulers / X | Typefully = 0 (ждёт OAuth) | **TAKE** | Треды, медиа, X+Bluesky+Mastodon+Threads+LinkedIn, без биллинга X API | (a) `https://mcp.typefully.com/mcp` | Free: 1 social set, 10 постов/мес, MCP включён | https://socialrobot.io/blog/best-mcp-servers-for-social-media | medium |
| Buffer MCP | Social schedulers | Buffer = 0 | **LATER** (или DON'T, если берёте Typefully) | Рабочий официальный remote, но на ваших сетях дублирует Typefully | (a) `https://mcp.buffer.com/mcp` | Free: 3 канала; Essentials $5/канал/мес; Team $10 | https://www.breakreach.com/best/mcp-servers-for-social-media | medium |
| Hootsuite / Publer / Ayrshare | Social schedulers | — | **DON'T TAKE** | Hootsuite от $99/мес/пользователь; Ayrshare — API-биллинг; для одного оператора избыточно | (a) (Hootsuite через коннектор) / unknown | Hootsuite $99/мес | https://www.schedchie.com/blog/social-media-mcp-servers-compared-2026 | low |
| Meta Ads MCP (`mcp.facebook.com/ads`) | Facebook/Meta | facebook.com 15 | **DON'T TAKE** | Только реклама; для Pages официального MCP нет | (a) OAuth | unknown | субагент: https://developers.facebook.com/news/ (snippet) | medium |
| Bluesky community (cyanheads и др.) | Bluesky/Mastodon/… | bsky 5, mastodon 5 | **DON'T TAKE** (постинг через Typefully) | Community-серверы; upstream brianellin заброшен | (a)/(b)/(c) по-разному | бесплатно | https://github.com/TadMSTR/bsky-mcp-server | medium |
| Reddit (`karanb192/reddit-mcp-buddy`) | Reddit | reddit 6 | **DON'T TAKE** | 6 сессий; community; риск ToS при записи | (c) по описанию (npx) | бесплатно | https://chatforest.com/guides/best-social-media-mcp-servers/ | low |
| dev.to / Hashnode / HackerNoon MCP | dev.to и др. | dev.to 27, hackernoon 6 | **DON'T TAKE** | Нет официальных; dev.to API + скрипт дешевле | community (c) | бесплатно | https://dev.to/failedahmad/bluesky-mcp-manage-a-bluesky-account-from-your-ai-assistant-3k3d | low |
| LinkedIn (`stickerdaniel/linkedin-mcp-server`) | LinkedIn | linkedin 6 (аккаунт забанен) | **DON'T TAKE** | Аккаунт забанен; community-сервер работает через сессию/скрейпинг | (c)/(b) | бесплатно | https://chatforest.com/guides/best-social-media-mcp-servers/ | medium |
| paper-mcp (MCPServings) | Scholarly | arxiv 25, semanticscholar 6, openalex 6 | **LATER** (ad hoc) | Remote без авторизации, но community-сервер с одним мейнтейнером | (a) без auth | бесплатно | https://github.com/mcpservings/paper-mcp | medium |
| cyanheads zenodo/datacite/crossref MCP | Scholarly | zenodo 21, datacite 8, crossref 7, doi.org 22 | **DON'T TAKE (always-on)**; ad hoc ок | Только чтение; запись на Zenodo (основной ваш use-case) не покрыта | (c) или (b) локальный Streamable HTTP | бесплатно | https://github.com/cyanheads/zenodo-mcp-server | medium |
| orcid-mcp (OpenCodice) | Scholarly | orcid.org 58 | **DON'T TAKE** | Только публичный реестр, без записи в ваш профиль | (c) pip | бесплатно | https://zenodo.org/records/20024303 | medium |
| Hugging Face MCP (официальный) | Hugging Face (+papers) | huggingface 18 | **TAKE** | Официальный, remote OAuth, read-only по умолчанию; плюс поиск статей | (a) `https://huggingface.co/mcp?login` | бесплатно | https://github.com/huggingface/blog/blob/main/building-hf-mcp.md | high |
| ATS-агрегаторы (Apify actors, Glama connectors) | Jobs/ATS | ashbyhq 16, greenhouse 10, lever 6, wellfound 5, ycombinator 9 | **DON'T TAKE** | Официальных нет; десятки клонов одного и того же; публичные board API проще скриптом | (a)/(b) community | Apify от $1.50/1 000 вакансий | https://apify.com/morehavoc/ats-jobs-search/api/mcp | medium |
| Cloudflare Code Mode MCP | Infra | cloudflare 3 | **TAKE** (дёшево по контексту) | Весь API в ~1 000 токенов, OAuth, официальный | (a) `https://mcp.cloudflare.com/mcp` | бесплатно (тарифы CF) | https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/ | high |
| Hetzner Cloud MCP | Infra | hetzner 4+3 | **DON'T TAKE** | Официального нет; community stdio с API-токеном на запись | (c)/(b) community | бесплатно | субагент: официальный сервер не найден | medium |
| Tailscale MCP | Infra | tailscale 6 | **DON'T TAKE** | Официального сервера управления tailnet нет; есть только поиск по докам | docs: (a) `https://tailscale.mcp.kapa.ai/` | бесплатно | субагент: https://tailscale.com/docs/develop-with-ai (snippet) | medium |
| Docker MCP Gateway | Infra / прокси | docker CLI 19 | **TAKE как инструмент** (обёртка stdio→HTTP), не как коннектор Docker | Позволяет превратить stdio-серверы в один HTTP-процесс на машину | (b) `--transport streaming` | бесплатно, OSS | https://github.com/docker/mcp-gateway | medium |
| Calendly MCP (официальный) | Scheduling | calendly 13 | **TAKE** | Официальный remote, OAuth 2.1 + DCR | (a) `https://mcp.calendly.com` | бесплатно (тариф Calendly) | https://developer.calendly.com/calendly-mcp-server | high |
| Luma MCP (официальный) | Events | luma 5 | **LATER** | Официальный, но без DCR (только CIMD) — проверить совместимость клиентов | (a) `https://mcp.luma.com` | без Luma Plus | субагент: https://help.luma.com/p/mcp | medium |
| Partiful | Events | partiful 3 | **DON'T TAKE** | MCP не найден | — | — | prior art not found | low |
| Airtable MCP (официальный) | Airtable/Notion | airtable 3 | **LATER** | 3 сессии; официальный remote OAuth/PAT | (a) `https://mcp.airtable.com/mcp` | тарифы Airtable; 5 rps на base | субагент: https://www.airtable.com/platform/mcp | medium |
| Notion MCP (официальный) | Airtable/Notion | 0 | **DON'T TAKE** | Нет спроса | (a) `https://mcp.notion.com/mcp` | тарифы Notion | субагент: https://developers.notion.com/guides/mcp/get-started-with-mcp | high |
| Exa MCP | Web search | jina.ai 5, algolia 6 (+ fan-out) | **TAKE** | Remote, без ключа на free, search+fetch | (a) `https://mcp.exa.ai/mcp` | free plan; ~$7/1 000 поисков | https://www.firecrawl.dev/blog/best-web-search-mcp | high |
| Firecrawl MCP | Web fetch | — | **TAKE** (или вместо Exa) | Keyless search/scrape/parse; есть в директории Claude | (a) `https://mcp.firecrawl.dev/v2/mcp` | keyless (лимит по IP); от $16/мес | https://parallel.ai/articles/best-web-search-mcp | high |
| Tavily MCP | Web search | — | **LATER** | Хорош, но дублирует Exa | (a) `https://mcp.tavily.com/mcp` | 1 000 кредитов/мес | https://contextbolt.com/blog/web-search-mcp-servers/ | high |
| Jina MCP | Web fetch | jina.ai 5 | **LATER** | Reader + arXiv/SSRN; только hosted | (a) | keyless с rate limit | https://cookiesforai.app/best/mcp-servers-for-web-research | medium |
| Brave Search MCP | Web search | — | **DON'T TAKE** | Только local/stdio; бесплатный tier на 2 000 запросов/мес убран 12.02.2026 (оспаривается agentdeals.dev) | (c) | $5 кредитов/мес (нужна карта), далее $5/1 000 | https://preuve.ai/blog/best-mcp-servers-for-research-2026 | medium |
| Claude in Chrome | Browser | 15.3k calls / 311 sessions | **KEEP** | Уже работает, основной канал | расширение | в составе Claude | — | high |
| Playwright MCP (`microsoft/playwright-mcp`) | Browser | — | **LATER** (ad hoc / headless на VPS) | Режим `--port` даёт один HTTP-процесс на машину | (b) `npx @playwright/mcp --port 8931` | бесплатно | https://github.com/microsoft/playwright-mcp | high |
| Browserbase MCP | Browser | — | **DON'T TAKE** | Облачный браузер не нужен при работающем Chrome; цены не проверены | (a) | unknown | https://roundproxies.com/blog/playwright-mcp/ | low |
| Slack MCP (официальный) | Messaging | 0 | **DON'T TAKE** | Нет спроса; нужна зарегистрированная Slack-app, DCR не поддерживается | (a) `https://mcp.slack.com/mcp` | бесплатно | https://eseospace.com/blog/slack-mcp-server/ | high |
| Discord / Signal MCP | Messaging | 0 | **DON'T TAKE** | Нет спроса; официальных серверов не нашёл | unknown | unknown | prior art not found | low |

---

## 2. Топ-10 к подключению сейчас (по ожидаемой ценности для вашего флота)

| # | Что | Ожидаемая ценность | Ключевое условие |
|---|---|---|---|
| 1 | **Перевод LLM fan-out с web UI на OpenRouter/API** (не MCP) | Максимальная: 222 сессии (75+56+49+34+8) сейчас идут через автоматизацию браузера | Скрипт `openrouter` + ключ; web UI оставить для ручной работы |
| 2 | **Typefully MCP** | Закрывает X (45 сессий) + bsky + mastodon одним коннектором, с тредами и медиа | Free хватит на ~10 постов/мес; дальше платный план (цена unknown) |
| 3 | **Exa MCP** (или Firecrawl) | Дешёвый search/fetch для research вместо браузера | Без ключа на free; ключ — когда упрётесь в лимиты |
| 4 | **Hugging Face MCP** | 18 сессий + поиск статей/Spaces; официальный, по умолчанию read-only | `?login` для OAuth; write-инструменты не включать |
| 5 | **Calendly MCP** | 13 сессий; бронирования без браузера | OAuth 2.1 + PKCE и DCR; scopes `mcp:scheduling:read`/`mcp:scheduling:write`; PAT не поддерживаются |
| 6 | **GitHub remote MCP, read-only, toolsets `issues,pull_requests,notifications`** | Нужен клиентам без shell (claude.ai web, ChatGPT); агентам с shell — не нужен | URL `.../mcp/x/issues/readonly` или заголовки `X-MCP-Toolsets` + `X-MCP-Readonly` |
| 7 | **Cloudflare Code Mode** | Мало сессий (3), но почти бесплатно по контексту (~1 000 токенов) | OAuth; ограничить аккаунт/токен |
| 8 | **Firecrawl MCP** (если Exa не покрывает JS-рендеринг) | Скрейп JS-страниц для Exa-пробелов | keyless лимит по IP |
| 9 | **Docker MCP Gateway на VPS** | Один HTTP-процесс вместо N stdio — снимает ваше главное ограничение для ad hoc-серверов (zenodo, orcid, reddit) | Bearer-auth включён по умолчанию; не открывать порт наружу без Tailscale |
| 10 | **Airtable или Luma** (по факту спроса) | 3–5 сессий; официальные remote | Luma: проверить, поддерживает ли клиент CIMD |

---

## 3. Явный DON'T-список

| Не подключать | Причина |
|---|---|
| Официальный X MCP в always-on | Запись (постинг) идёт через `xurl mcp`, а это stdio-мост к `api.x.com/mcp`. Нужны своё X developer app и биллинг pay-per-use. Расхождение: часть источников пишет, что hosted-сервер документирован только для чтения, закладок и Articles |
| Buffer (если берёте Typefully) | Дубль на ваших сетях. Лимиты free tier в источниках противоречат друг другу (250/день против 3 000/мес) |
| LinkedIn MCP (любой) | Аккаунт забанен; community-серверы работают через сессию браузера/скрейпинг, что повышает риск для нового аккаунта |
| Facebook: Meta Ads MCP / community Pages-серверы | Официальный сервер только для рекламы; для Pages официального нет |
| Brave Search MCP | stdio-only + бесплатный tier на 2 000 запросов/мес убран 12.02.2026 (теперь $5/1 000 и $5 кредитов с картой; факт оспаривается agentdeals.dev) |
| Hetzner/Tailscale community MCP | Нет официальных; токен с правами на запись к инфраструктуре в руках community-кода — плохой trade-off ради 7–6 сессий; `hcloud`/`tailscale` CLI уже есть |
| ATS-агрегаторы (Apify/Glama) | Десятки клонов-однодневок; публичные board API Greenhouse/Lever/Ashby проще скриптом; подача заявки через MCP не поддерживается |
| Reddit/Bluesky/dev.to community MCP | Мало сессий, community-код, у части upstream заброшен (brianellin/bsky-mcp-server) |
| Slack, Discord, Signal, Notion | Нет спроса в ваших данных |
| Browserbase | Дублирует Claude in Chrome / Playwright; стоимость не проверена |
| Hootsuite/Publer/Ayrshare | Дорого или API-биллинг для одного оператора |

---

## 4. Детали по кластерам

### 4.1 GitHub (+ раздел C)

**Что есть.** `github/github-mcp-server`, MIT. По данным Scalekit (проверено в октябре 2026), последний релиз — v2.0.2 от 8 октября 2026. Звёзды я не проверял: unknown. Hosted remote: `https://api.githubcopilot.com/mcp/` по Streamable HTTP, «доступен любому пользователю GitHub на любом плане». Каждый вызов выполняется от имени пользователя, чей OAuth или PAT передан.

**Сужение поверхности:**
- read-only: URL `https://api.githubcopilot.com/mcp/readonly` или заголовок `X-MCP-Readonly`;
- один toolset: путь `/x/{toolset}`, например `/x/issues/readonly`;
- несколько toolsets: заголовок `X-MCP-Toolsets: repos,issues`;
- отдельные инструменты: `X-MCP-Tools`;
- lockdown-режим: заголовок `X-MCP-Lockdown: true`.

Документация GitHub подчёркивает, что read-only — «строгий фильтр безопасности», он выключает write-инструменты, даже если их запросили явно.

**Токены (prior art есть, несколько независимых замеров):**
- **OpenRouter:** одна GitHub-задача на четырёх моделях. Результаты: `gh` CLI — 1 421 входных токенов; skill — 3 315; MCP с 2 инструментами и обрезанным ответом — 5 357; MCP с 2 инструментами — 23 228; MCP с 46 инструментами по умолчанию — 59 417; MCP с 91 инструментом — 101 351. По формулировке OpenRouter, «the default MCP server used 42 times the CLI's input tokens and cost 17 times as much, $127 against $7.50 per 1,000 tasks». Один ответ (release object) стоил ~20 400 токенов, больше всех 46 определений вместе. https://openrouter.ai/blog/insights/mcp-vs-cli-vs-skills/
- **Scalekit** (по пересказу Firecrawl): MCP в 4–32 раза дороже по токенам. Надёжность: CLI 100% против MCP 72%. На 10 000 операций в месяц ~$3.20 против ~$55.20. https://www.firecrawl.dev/blog/mcp-vs-cli
- **Mornati:** сессия из 20 шагов: CLI 448 токенов против 61 654 у native GitHub MCP (137×), ~3 062 токенов схем в каждом запросе. https://blog.mornati.net/the-future-of-agentic-tooling-mcp-servers-vs-cli-a-data-driven-comparison/
- **Контр-замер Smithery** (в пересказе Metaheuristic): native MCP — 91.7% успеха и медиана 28 838 токенов; CLI с поданным деревом команд — 83.3% и 82 942. https://www.metaheuristic.co/blog/mcp-vs-cli-abstraction-tax

**Вывод для вас.** У Claude Code и Codex есть shell и `gh`, и 2 349 вызовов показывают, что путь работает. По данным OpenRouter, 46 инструментов GitHub MCP по умолчанию добавляют 19 436 токенов к каждому запросу на Claude Sonnet 5.5 (8 903 на GPT-6 Luna) — без новой функциональности. Есть два случая, где MCP оправдан:
1. web-клиенты без shell (claude.ai, ChatGPT);
2. централизованный OAuth без PAT на 8 машинах.

В обоих случаях берите read-only и 1–3 toolsets. Отдельно: Claude Code умеет откладывать загрузку инструментов (Tool Search). OpenRouter показал, что Tool Search сокращает начальную стоимость 46 инструментов до менее чем 600 токенов и вдвое снижает стоимость всей задачи на Sonnet 5.5, но на Claude Haiku 5.5 стоимость, наоборот, растёт.

**Безопасность.** Главный вектор — prompt injection через текст issues/PR. Walma прямо выделяет его в гайде. Lockdown-режим и read-only — обязательны. https://walma.ai/en/guides/mcp/github-mcp

**Multi-vendor.** Remote URL работает в Claude Code (`claude mcp add-json ... "type":"http"`), Cursor, VS Code, Codex (`codex mcp add ... --url`, `codex mcp login`). ChatGPT подключает только remote. https://learn.chatgpt.com/docs/extend/mcp

### 4.2 LLM vendors / OpenRouter

**Решение.** Официальных MCP, которые давали бы «запросить ChatGPT/Grok/GLM как исследователя», в этом исследовании не найдено. Prior art not found: искал в поиске по MCP-каталогам и блогам. Ваши 222 сессии fan-out через браузер — главный источник ToS-риска во всём флоте. Тексты ToS OpenAI/Anthropic/xAI/z.ai/Mistral в этом исследовании я **не открывал**, поэтому формулировки о запрете автоматизации — **low confidence, проверьте сами**. Практически: те же модели доступны через API/OpenRouter, у вас уже 10 сессий через shell. Fan-out переносите туда, браузер оставьте только для того, чего нет в API (например, Deep Research-режимы UI).

### 4.3 X/Twitter (+ раздел A ниже)

**Официально существуют два варианта:**
- self-hosted `xdevplatform/xmcp`, апрель 2026, Python;
- hosted `https://api.x.com/mcp`, запущен 30 июня 2026, Streamable HTTP, сгенерирован из OpenAPI-спеки, 200+ эндпоинтов.

Подключение рекомендуется через `npx -y @xdevplatform/xurl mcp https://api.x.com/mcp`. Мост делает OAuth 2.0 PKCE, кэширует и обновляет токены. Для headless-машин есть `xurl auth oauth2 --headless`. https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558

Важно для вашего правила: `xurl mcp` — это **stdio-процесс**, который проксирует в remote. Чтение работает с app-only Bearer напрямую по HTTP. Запись (посты, закладки, Articles) требует user-context через мост. https://github.com/danvega/xmcp

Значит, постинг через официальный X MCP = stdio на каждую сессию. Обходной путь — один `xurl mcp` за Docker MCP Gateway/supergateway на VPS. Работать будет, но это самодельная инфраструктура с OAuth-токеном X на сервере.

### 4.4 Facebook/Meta

По данным субагента, официальный Meta-hosted сервер есть только для **рекламы**: `https://mcp.facebook.com/ads`, открыт для всех разработчиков 16 июля 2026. Для органического постинга в Pages официального MCP не найдено. Ваши 15 сессий на facebook.com оставьте на Claude in Chrome. Можно и через Buffer, если захотите FB Pages: Buffer его поддерживает.

### 4.5 Bluesky / Mastodon / Reddit / dev.to / Hashnode / HackerNoon

Официальных серверов нет. Что есть у community:
- Bluesky: `cyanheads/bluesky-mcp-server` (remote, в официальном реестре); `brianellin/bsky-mcp-server` (49 звёзд). Upstream последнего, по словам форка, «unmaintained as of early 2026», а сам форк TadMSTR архивирован 11.09.2026, классический пример abandonware. https://github.com/TadMSTR/bsky-mcp-server
- Reddit: `karanb192/reddit-mcp-buddy`, 788 звёзд, без API-ключей. https://chatforest.com/guides/best-social-media-mcp-servers/

Для вас Bluesky и Mastodon закрывает Typefully, причём с тредами. dev.to (27 сессий) — это простой REST API с ключом, MCP не нужен. HackerNoon: MCP не найден.

### 4.6 LinkedIn

Аккаунт забанен. Самый популярный community-сервер — `stickerdaniel/linkedin-mcp-server`, 3 077 звёзд. Он работает от сессии пользователя. Для нового аккаунта это ускоренный путь к повторному бану. **DON'T.** Если LinkedIn вернётся, используйте Typefully/Buffer: они ходят через официальную интеграцию.

### 4.7 Scholarly (arXiv, ORCID, Zenodo, Crossref, DataCite, OpenAlex, Semantic Scholar, academia.edu, HotCRP)

Ни у одного из этих провайдеров официального vendor-run MCP не найдено. Варианты:
- `MCPServings/paper-mcp`: remote Streamable HTTP без авторизации, arXiv + Semantic Scholar + OpenAlex, 41 инструмент, на Smithery отмечен «Jun 7, 2026». https://github.com/mcpservings/paper-mcp
- `cyanheads/zenodo-mcp-server`, `datacite-mcp-server`, `crossref-mcp-server`: «Runs as a stdio process or a local Streamable HTTP server». Только чтение и поиск. https://github.com/cyanheads/zenodo-mcp-server
- `orcid-mcp`: 7 инструментов, pip, публичный реестр. https://zenodo.org/records/20024303
- `alexwade/datacite-mcp`: stdio, read-only. https://github.com/alexwade/datacite-mcp

Ваши операции с ORCID и Zenodo (58 и 21 сессия) — это в основном **запись**: депозиты, привязка работ. Ни один найденный сервер этого не делает. Zenodo REST + скрипт, ORCID — через UI. academia.edu, HotCRP: prior art not found (искал в поиске по MCP-каталогам). Частичная замена для arXiv/papers: Hugging Face MCP (paper search) и Jina (arXiv/SSRN).

### 4.8 Job boards / ATS

Официальных MCP у Ashby, Greenhouse, Lever, Wellfound и YC не найдено. В каталогах Glama и Apify десятки клонов, которые оборачивают публичные board API. Пример — Apify actor за $1.50 на 1 000 вакансий. https://apify.com/morehavoc/ats-jobs-search/api/mcp

Для 46 сессий в месяц достаточно своего скрипта на публичных эндпоинтах. Подачу заявок ни один сервер не покрывает.

### 4.9 Hugging Face

Официальный сервер: `https://huggingface.co/mcp`, исходники `huggingface/hf-mcp-server`. Поддерживает STDIO, SSE и Streamable HTTP; для OAuth добавьте `?login`. https://github.com/huggingface/blog/blob/main/building-hf-mcp.md

По умолчанию сервер read-only. Write-инструменты (репозитории, jobs, sandboxes) включаются отдельно в `huggingface.co/settings/mcp`. Для Gemini CLI есть официальное расширение. https://mcp.directory/servers/hugging-face

### 4.10 Cloud/infra

- **Cloudflare:** `https://mcp.cloudflare.com/mcp`, «the only MCP server you need». Code Mode — 2 инструмента, ~1 000 токенов на весь API, OAuth, спецификация MCP 2026-07-28. Документация обновлена 27.10.2026. https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/
- **Hetzner:** официального сервера нет. Есть community: `dkruyt/mcp-hetzner` и `hcloud-mcp`.
- **Tailscale:** нет официального сервера для управления tailnet. Есть docs-MCP (`tailscale.mcp.kapa.ai`) и продукт Aperture (MCP-прокси).
- **Docker:** `docker/mcp-gateway` — OSS-шлюз, не коннектор к Docker. Режим `docker mcp gateway run --port 8080 --transport streaming`; SSE/streaming по умолчанию требуют Bearer. Последний релиз v0.44.3 от 26.10.2026 (по сниппету релизов, medium). https://github.com/docker/mcp-gateway

### 4.11 Scheduling/events

- **Calendly:** `https://mcp.calendly.com`. Документация Calendly: «The server is fully hosted by Calendly at https://mcp.calendly.com. Self-hosting or local deployment is not supported». Авторизация — OAuth 2.1 Authorization Code + PKCE (S256) и DCR (RFC 7591), никаких секретов в конфиге; scopes `mcp:scheduling:read` и `mcp:scheduling:write`; PAT и другие не-OAuth bearer-токены не поддерживаются. https://developer.calendly.com/calendly-mcp-server
- **Luma:** `https://mcp.luma.com`, Luma Plus не нужен. DCR не поддерживается, только CIMD; например, n8n не подключится (субагент, https://help.luma.com/p/mcp).
- **Partiful:** не найдено.

### 4.12 Airtable/Notion

Оба официальные и remote:
- Airtable: `mcp.airtable.com/mcp`, OAuth или PAT, лимиты 5 rps на base и 50 rps на токен;
- Notion: `mcp.notion.com/mcp`, OAuth. Старый OSS `notion-mcp-server` больше не поддерживается.

При 3 сессиях — LATER.

### 4.13 Web fetch/search

| Сервер | URL | Ключ | Free | Платно |
|---|---|---|---|---|
| Exa | `https://mcp.exa.ai/mcp` | не нужен на free | casual use | ~$7/1 000 |
| Firecrawl | `https://mcp.firecrawl.dev/v2/mcp` | keyless (лимит по IP) / OAuth | да | $83/мес за 100 000 кредитов |
| Tavily | `https://mcp.tavily.com/mcp` | OAuth/ключ | 1 000 кредитов/мес | ~$8/1 000 |
| Jina | hosted | rate limit без ключа | да | unknown |
| Brave | local `npx @brave/brave-search-mcp-server` | обязателен | $5 кредитов | $5/1 000 |

Источник: https://keirolabs.cloud/blog/best-mcp-servers-web-search-scraping-2026

Риск: всё, что вы fetch'ите, — непроверенный текст в контексте агента, то есть классический канал prompt injection. Не давайте агенту с web-fetch одновременно write-доступ к почте и соцсетям без подтверждения.

### 4.14 Browser automation

Claude in Chrome — ваш основной канал (15.3k вызовов), оставить. Playwright MCP умеет работать standalone HTTP: `--port 8931`, клиенты подключаются к `http://localhost:8931/mcp`. Флаг `--shared-browser-context` разделяет один контекст между клиентами. Это ровно модель «один процесс на машину». https://github.com/microsoft/playwright-mcp

Применение: headless-браузер на Linux VPS для задач без вашего залогиненного профиля. Browserbase — облачный вариант, цены не проверены.

### 4.15 Messaging

- **Slack:** `https://mcp.slack.com/mcp`, GA 17.02.2026. Нужна зарегистрированная Slack-app (Marketplace или internal), SSE и DCR не поддерживаются. https://eseospace.com/blog/slack-mcp-server/
- **Discord/Signal:** официальных серверов не нашёл (low). Telegram и WhatsApp у вас уже свои.

---

## A. «Пост в X с любой машины»: Buffer vs Typefully vs официальный X MCP

| Критерий | Typefully | Buffer | Официальный X MCP |
|---|---|---|---|
| Транспорт | (a) `mcp.typefully.com/mcp`, OAuth (DCR+CIMD) или API-ключ | (a) `mcp.buffer.com/mcp`, OAuth или API-ключ | Чтение — (a); **запись — через (c) `xurl mcp`** |
| Треды X | **Да** (drafts, threads, scheduling) | unknown (в MCP-описаниях не подтверждено) | Да, через reply-цепочку постов (ручная сборка агентом) |
| Медиа | Да: image, video, GIF, PDF (upload init + status) | unknown для MCP | Да, через media endpoints X API (детали не проверены) |
| Расписание | Да | Да (очередь) | **Нет**: в X API нет scheduled-posts |
| Сети | X, LinkedIn, Threads, Bluesky, Mastodon | 11+: X, FB, IG, LinkedIn, Bluesky, Mastodon, Threads, TikTok… | только X |
| Цена | Free: 10 постов/мес, MCP включён; платные — unknown | Free 3 канала / 10 запланированных на канал; $5/канал/мес | pay-per-use: $0.015/пост, $0.20/пост со ссылкой; нужен свой dev app + billing |
| Rate limits | «для личных автоматизаций»; цифры unknown | Free: 100/15 мин и 250/день **или** 3 000/мес — источники расходятся | лимиты X API по тарифу (не проверено) |
| Риск бана | **Низкий**: постинг через официальную интеграцию Typefully, сама Typefully требует соблюдать automation rules X | Низкий (аналогично) | Низкий по API ToS, но автономный агент = ваши посты; риск в контенте, не в канале |

Источники: https://socialrobot.io/blog/best-mcp-servers-for-social-media, https://support.typefully.com/en/articles/8718287-typefully-api, https://composio.dev/toolkits/typefully, https://www.breakreach.com/best/mcp-servers-for-social-media, https://opentweet.io/answers/can-the-official-x-mcp-server-schedule-posts, https://revive.social/social-media-mcp/

**Вердикт.** Основной канал — Typefully. Он один закрывает треды, медиа и X+Bluesky+Mastodon, работает remote с OAuth и не требует биллинга X API. Если X-тредов станет много, проверьте лимиты платного плана. Официальный X MCP подключайте ad hoc для **чтения**: full-archive search, mentions, bookmarks. Buffer снимите, если FB/IG не нужны.

Противоречие источников. Одни пишут, что hosted X MCP документирован для чтения, закладок и Articles, а «tweet-posting» на нём не существует (revive.social, agentpedia). Другие пишут, что он «не read-only» и постит (opentweet). Достоверно одно: запись через `xurl` user-context. Оцениваю как medium.

---

## B. Каталоги и как отличить живое от abandonware

| Каталог | Что это (ноябрь 2026) | Сигнал качества |
|---|---|---|
| **Официальный MCP Registry** (`registry.modelcontextprotocol.io`) | Метаданные (`server.json`) с namespace-верификацией. 47 267 активных серверов на 21.10.2026; 64% с remote URL (14.10.2026) | `com.vendor/...` (домен проверен) лучше, чем `io.github.user/...`. 62% опубликованы один раз и не обновлялись (сентябрь 2026). Реестр ничего не удаляет |
| **claude.ai Connectors directory** | Курируемые Anthropic коннекторы (Buffer, Firecrawl, Slack и др. есть) | Ревью Anthropic, OAuth; подробности ревью — unknown |
| **ChatGPT Apps/Plugins** | Каталог apps объединён с Codex plugins (июль 2026); свои серверы — «Add custom MCP server», только remote | OpenAI: custom-серверы «not developed or verified by OpenAI»; write-действия по умолчанию требуют подтверждения |
| **Cursor MCP directory** | unknown (не проверял) | — |
| **Smithery** | Хостинг и каталог; показывает «last deployed», score | Дата деплоя и число вызовов |
| **Glama** | Каталог + connectors; оценки license/quality/maintenance (A–D) | Оценка maintenance D = красный флаг |
| **mcp.so** | Крупный агрегатор | не проверял (low) |

Источники: https://www.digitalapplied.com/blog/mcp-adoption-statistics-2026-model-context-protocol, https://www.localcan.com/blog/state-of-mcp-server-development, https://dev.to/amareswer/the-mcp-registry-by-the-numbers-38nc, https://hatchable.com/articles/how-to-add-an-mcp-server-to-chatgpt, https://eseospace.com/blog/openai-mcp/

**Эвристики «живой vs abandonware»:**
1. Hosted на домене вендора (`mcp.<vendor>.com`) плюс упоминание в его docs. Это сильнейший сигнал.
2. Namespace в реестре `com.<vendor>` с проверкой домена.
3. Релиз или коммит за последние 90 дней. Архивированный репозиторий или пометка «not actively maintained» (как у старого `notion-mcp-server`) = нет.
4. Один версион-рекорд в реестре и ни одного обновления = скорее однодневка.
5. Оценка maintenance на Glama A/B.
6. Есть OAuth/DCR, а не только «положите пароль в заголовок» (как у части Bluesky-серверов).
7. Тулы с аннотациями `readOnlyHint`/`destructiveHint`.

---

## C. GitHub remote MCP vs `gh` — итог

- **Токены:** CLI в 4–40× дешевле. Prior art есть (OpenRouter, Scalekit, Mornati), контр-замер есть (Smithery).
- **Функционально:** ничего, чего нет в `gh api`. MCP даёт OAuth без PAT, read-only/lockdown на стороне сервера и доступ для клиентов без shell.
- **Решение:** `gh` по умолчанию. Remote MCP — `.../mcp/x/issues/readonly` и `.../x/pull_requests/readonly` только для claude.ai/ChatGPT web.

---

## Prior art по каждой рекомендации

| Рекомендация | Prior art |
|---|---|
| `gh` > GitHub MCP | Есть: OpenRouter, Scalekit (через Firecrawl), Mornati, контр-замер Smithery (ссылки выше) |
| Typefully для X | Сравнения есть (socialrobot.io, eden.so, note.com/miccell), но это обзоры вендоров-конкурентов, не нейтральные замеры |
| Exa/Firecrawl | Сравнения есть (parallel.ai, firecrawl.dev, keirolabs). Все авторы — заинтересованные вендоры. Нейтральный замер: prior art not found |
| Hugging Face MCP | prior art not found (искал в каталогах/блогах через web search) |
| Cloudflare Code Mode | Есть вендорский замер: ~1 000 токенов против 1.17 млн при одном инструменте на эндпоинт (https://restfulapi.net/mcp-vs-cli/) |
| Calendly | prior art not found |
| API вместо web-UI fan-out | prior art not found (web search по MCP-блогам) |
| Docker MCP Gateway как обёртка stdio | prior art not found (замеров RAM/процессов не нашёл) |

## Caveats

- Бюджет поиска исчерпан (18 запросов + субагент), а не 30, как требовалось. Поэтому Discord, Signal, Partiful, Cursor directory, mcp.so, Browserbase и тексты ToS LLM-вендоров не проверены. Где написано «unknown» или «low», так и есть.
- Многие источники — блоги вендоров-конкурентов (SocialRobot, Firecrawl, Parallel), они заинтересованы. Цифры там брал только как ориентир.
- Звёзды и даты релизов из первоисточников проверены не везде; где их нет, это unknown.

## Sources (citations)
https://github.com/huggingface/blog/blob/main/building-hf-mcp.md
https://blog.logrocket.com/top-15-mcp-servers-ai-projects/
https://www.firecrawl.dev/blog/best-web-search-mcp
https://socialrobot.io/blog/best-mcp-servers-for-social-media
https://trustedmcp.io/guides/github-mcp-server
https://revive.social/social-media-mcp/
https://x.com/AlphaSignalAI/status/2074901398534213904/photo/1
https://developers.facebook.com/documentation/ads-commerce/ads-ai-connectors/ads-mcp-server/ads-mcp-server-overview
https://www.metaheuristic.co/blog/mcp-vs-cli-abstraction-tax
https://www.scalekit.com/blog/github-mcp-server-per-user-oauth
https://jannikreinhard.com/why-cli-tools-are-beating-mcp-for-ai-agents/
https://octomind.run/tap/deps/xdevplatform/xurl
https://eseospace.com/blog/best-mcp-servers-for-social-media/
https://www.schedchie.com/blog/social-media-mcp-servers-compared-2026
https://github.com/TadMSTR/bsky-mcp-server
https://chatforest.com/guides/best-social-media-mcp-servers/
https://dev.to/failedahmad/bluesky-mcp-manage-a-bluesky-account-from-your-ai-assistant-3k3d
https://smithery.ai/servers/kangletian/paper-mcp
https://github.com/cyanheads/zenodo-mcp-server
https://glama.ai/mcp/servers/cyanheads/zenodo-mcp-server
https://zenodo.org/records/20024303
https://eseospace.com/blog/hugging-face-mcp-server/
https://apify.com/morehavoc/ats-jobs-search/api/mcp
https://glama.ai/mcp/connectors/io.github.winters-works-co/ats-job-feeds
https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/
https://glama.ai/mcp/servers/hannojg/hetzner-cloud-plugin
https://github.com/ever-works/awesome-mcp-servers/blob/master/details/hetzner.md
https://tailscale.com/docs/develop-with-ai
https://yaw.sh/blog/tailscale-mcp-server/
https://github.com/docker/mcp-gateway
https://fast.io/resources/mcp-server-for-calendly/
https://developer.calendly.com/calendly-mcp-server
https://help.luma.com/p/mcp
https://airtable.com/developers/agents/mcp/getting-started
https://www.airtable.com/platform/mcp
https://parallel.ai/articles/best-web-search-mcp
https://contextbolt.com/blog/web-search-mcp-servers/
https://cookiesforai.app/best/mcp-servers-for-web-research
https://github.com/microsoft/playwright-mcp
https://playwright.dev/mcp/configuration/options
https://roundproxies.com/blog/playwright-mcp/
https://eseospace.com/blog/slack-mcp-server/
https://dev.to/dedupedai/how-to-connect-vs-code-copilot-to-github-with-the-official-github-mcp-server-5611
https://github.com/docker/mcp-gateway/releases
https://opentweet.io/answers/can-the-official-x-mcp-server-schedule-posts
https://www.socialync.io/blog/best-social-media-mcp-servers-2026
https://walma.ai/en/guides/mcp/github-mcp
https://github.com/github/github-mcp-server/blob/main/docs/server-configuration.md
https://www.firecrawl.dev/blog/mcp-vs-cli
https://blog.mornati.net/the-future-of-agentic-tooling-mcp-servers-vs-cli-a-data-driven-comparison/
https://hatchable.com/articles/how-to-add-an-mcp-server-to-chatgpt
https://learn.chatgpt.com/docs/extend/mcp
https://note.com/humble_bobcat51/n/nb9d203febdf2?hl=en
https://startupfortune.com/x-launches-an-official-mcp-server-and-every-social-platform-will-need-to-follow/
https://mcpplaygroundonline.com/mcp-servers/x-twitter
https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558
https://github.com/danvega/xmcp
https://mintec.co/blog/meta-ads-mcp-server/
https://developers.facebook.com/news/
https://www.pulsemcp.com/servers/cyanheads-bluesky
https://github.com/mcpservings/paper-mcp
https://github.com/alexwade/datacite-mcp
https://www.remote-mcp.com/servers/hugging-face
https://glama.ai/mcp/connectors/io.github.fairquery/company-jobs-feed
https://mcp.directory/servers/hugging-face
https://conare.ai/marketplace/mcp/hcloud-mcp
https://glama.ai/mcp/servers/@dkruyt/mcp-hetzner/inspect
https://tailscale.com/docs/aperture/mcp-server
https://www.getmaxim.ai/articles/best-mcp-gateways-with-server-registry/
https://developers.notion.com/guides/mcp/get-started-with-mcp
https://mcpservers.org/servers/microsoft/playwright-mcp
https://github.com/microsoft/playwright-mcp/blob/main/README.md
https://getunblocked.com/blog/slack-mcp/
https://composio.dev/toolkits/typefully
https://opentweet.io/blog/xmcp-x-official-mcp-server-guide
https://support.typefully.com/en/articles/8718287-typefully-api
https://agentpedia.codes/blog/connect-x-hosted-mcp-server
https://dev.to/amareswer/the-mcp-registry-by-the-numbers-38nc
https://www.localcan.com/blog/state-of-mcp-server-development
https://www.digitalapplied.com/blog/mcp-adoption-statistics-2026-model-context-protocol
https://eseospace.com/blog/chatgpt-developer-mode-mcp/
https://eseospace.com/blog/openai-mcp/
https://mcpservers.org/servers/ravishka17/bluesky-mcp
https://agentman.ai/agentskills/connections/mcp-server/calendly
https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers
https://github.blog/ai-and-ml/generative-ai/a-practical-guide-on-how-to-use-the-github-mcp-server/
https://restfulapi.net/mcp-vs-cli/