---
dr_id: DR26-11-11-ZB-01-1753
title: "DR26-11-11-ZB-01-1753 — какие MCP-коннекторы брать под наши задачи"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Какие MCP-коннекторы брать под наши задачи (DR26-11-11-ZB-01-1753)

Заказ Антона 11.11.2026 17:46 через сессию Z82067. Вход: анализ спроса по 9 125 сессиям 27.05–11.11 (`_scratch/connectors-demand-20261111.txt`). Правило флота §7.13: в постоянном конфиге только http-коннекторы.

**Кто делал.** Оператор веера Codex (приказ 03.11, computer use в Chrome): запустил ChatGPT, claude.ai, GLM, Mistral; Gemini и Grok упали у него. Claude Z40090: Gemini a2@ повторно (та же ошибка), Grok и Gemini заказаны пирам, сбор, гейт, синтез, живая сверка каталога claude.ai (`mcp-registry`) и двух несущих фактов.

**Ноги (dr_leg_gate --strict, все rc=0), кворум 4/6:** ChatGPT DR 347 источников · claude.ai Research 190 · Mistral deep-research 94 · GLM 285 (режим Task UI, усилие не отображается; по наблюдению ретро шёл Flash, а не сильнейший, кворум по усилию завышен). Gemini и Grok не собраны (см. frontmatter), заказаны пирам. Оригиналы: `_originals/deep-research/DR26-11-11-ZB-01-1753-mcp-connectors-*.md`.

## Где сошлись (ChatGPT, claude.ai, Mistral; GLM согласен по пунктам 1 и 5; по п.4 GLM ставит Cloudflare LATER)

1. **Пост в X с любой машины = Typefully MCP** (`mcp.typefully.com/mcp`, OAuth): треды, медиа, расписание; заодно Bluesky, Mastodon, Threads. Buffer (`mcp.buffer.com/mcp`, есть на бесплатном плане) = запасной; держать оба активными для одних и тех же сетей = риск двойных постов.
2. **Официальный X MCP не для записи.** Сверено по docs.x.com: запись только через локальный мост `xurl` (stdio, наш §7.13 запрещает в постоянном конфиге) и свой developer app с оплатой X API ($0.015 за пост, $0.20 со ссылкой). Постинга твитов в списке возможностей нет. Годится ad hoc для чтения.
3. **GitHub: `gh` остаётся главным.** Замеры: Scalekit 4–32× токенов у MCP и 72% надёжности против 100% у CLI; замер OpenRouter: 42× токенов. Remote MCP только read-only toolsets (`.../mcp/x/issues/readonly`) для клиентов без shell.
4. **Брать:** Hugging Face (официальный), Calendly (официальный, бесплатный план), Luma (официальный), Cloudflare (code mode ≈1 000 токенов на схему).
5. **Не брать:** LinkedIn (ToS, аккаунт забанен), Reddit (сверено: новые заявки на API закрыты 31.10.2026, полное отключение в марте 2027), Facebook (у Meta только Ads MCP), ATS-коннекторы Ashby/Greenhouse/Wellfound (сторона работодателя, подать заявку нельзя), Hetzner/Tailscale/Docker MCP (CLI хватает), Brave, Playwright MCP в постоянном конфиге (stdio), Slack/Discord/Signal/Notion (спроса нет), Hootsuite/Publer/Ayrshare (дубль).

## Где разошлись (не усредняем)

- **Веб-поиск:** ChatGPT за Jina (Tavily у него LATER), claude.ai за Exa (+Firecrawl), Mistral за Tavily или Exa, GLM за Firecrawl + Tavily. Три ноги из четырёх говорят: брать ОДИН. Решение: Tavily (remote, в каталоге claude.ai; «keyless» у Tavily = заголовок X-Tavily-Access-Mode, без него OAuth: живая проба 12.11 23:55 дала 401, в приложении «Needs authentication»), Exa запасной.
- **GLM против трёх остальных (критерий GLM: «первичный канал вендора = меньше риска»):** (1) ставит GitHub remote MCP на №1, ссылаясь на поиск инструментов MCP (схема с ~55k до ~4,2k токенов). Три ноги против, у них замеры MCP против CLI. (2) Советует официальный X MCP для постинга. Это опровергается docs.x.com: запись только через stdio-мост `xurl`, постинга твитов в списке возможностей нет. (3) Называет «официальный OpenAlex MCP `mcp.openalex.org`». ⚠️ Поправка 13.11 (ретро Z40090): это ПРАВДА. GLM дал два источника (github.com/ourresearch/openalex-mcp-server, help.openalex.org/access/connector), Mistral назвал тот же репо; 11.11 я проверил одной выдачей WebSearch, не открыв ссылки, и записал «выдумка». Живая проба 13.11: `https://mcp.openalex.org/mcp` отвечает 401 с WWW-Authenticate Bearer (OAuth, scope openalex:query), корень домена отвечает «The MCP endpoint is /mcp». Вердикт: БРАТЬ (OAuth). Урок: вердикт «выдумка» по ноге допустим только после curl каждого URL, который нога привела.
- **Научный кластер (ORCID 58 сессий, Zenodo 21, DOI 22, arXiv 25):** ChatGPT: `scholarly-mcp` + ORCID + Zenodo отдельными HTTP. Mistral: один свой HTTP-демон на VPS (OpenAlex официальный + cyanheads ORCID/Zenodo/DataCite). claude.ai: эти stdio не брать, хостить через Docker MCP Gateway на VPS. Общее: один HTTP-процесс на Маяке, а не stdio. Из каталога claude.ai (моя сверка): **alphaXiv** (`api.alphaxiv.org/mcp/v1`, полный текст arXiv) и **Lacuna Research** (2,3 млн ML-статей + профили авторов) уже remote, ставить ничего не надо.
- **Перевести ДР-веер на OpenRouter API** (claude.ai, место №1): ⛔ противоречит приказу Антона 03.10 «ДР только через браузер» (подписочный Deep Research живёт только в вебе). Не берём; сам OpenRouter MCP для каталога моделей и цен берут ChatGPT и Mistral.
- Одиночные факты, не перепроверенные: Smithery закрыл прокси 31.10.2026 (только ChatGPT); у Brave убран бесплатный тариф (только claude.ai, сам источник оспаривает).

## Таблица решения

| Коннектор | Кластер | Наша частота (сессий) | Вердикт | Почему |
|---|---|---|---|---|
| Typefully `mcp.typefully.com/mcp` | X, Bluesky, Mastodon, Threads | x 45, bsky 5, mastodon 5 | **БРАТЬ (уже в конфиге, нужен OAuth)** | 3/3 ног; треды и медиа; официальная интеграция = низкий риск бана |
| Buffer `mcp.buffer.com/mcp` | соцсети | те же | ЗАПАС | бесплатный план; не держать активным вместе с Typefully на тех же сетях |
| X official `api.x.com/mcp` | X | x 45 | ТОЛЬКО ЧТЕНИЕ, ad hoc | запись через stdio-мост `xurl` + оплата за пост |
| Hugging Face `huggingface.co/mcp` | HF | 18 | **СТОИТ** | 3/3; официальный; 13.11 в приложении Connected, 4 инструмента |
| OpenAlex official `mcp.openalex.org/mcp` (OAuth) | статьи | openalex 6, scholar 58 | **БРАТЬ** | официальный; 2 ноги (GLM, Mistral); подтверждён 13.11 (401 + WWW-Authenticate) |
| Научный HTTP-демон `scholarly_mcp.py` (ORCID + Zenodo + DOI; OpenAlex как бесключевой фолбэк) | статьи | ~150 | **ПОСТАВЛЕН** 13.11 на Маяке (systemd, 127.0.0.1:8790, тест 21/21 на узле) | ORCID/Zenodo/Crossref/DataCite официального MCP не имеют; ⚠️ хост: Маяк без LLM и без пути от узлов с приложением, решение Антона: туннель к Маяку или локальный демон на каждом узле (на ноутбук поднимался за 2 с) |
| alphaXiv (каталог claude.ai) | arXiv | 25 | **БРАТЬ** | уже remote, полный текст; быстрый выигрыш до демона |
| Calendly `mcp.calendly.com` | встречи | 13 | **БРАТЬ** | 3/3; официальный |
| OpenRouter MCP `mcp.openrouter.ai/mcp` | LLM-рельсы | 10 | **БРАТЬ** | 2/3; каталог моделей и цен; ключ живёт 7 дней с потолком $10 |
| Tavily `mcp.tavily.com/mcp` | веб-поиск | jina 5, algolia 6 | **БРАТЬ ОДИН** | 2 ноги TAKE, ChatGPT LATER; OAuth (401 при пробе 23:55, в приложении Needs authentication); Exa запасной |
| Cloudflare code mode `mcp.cloudflare.com/mcp` | инфра | 3 | СТОИТ, кандидат в утиль | 2/4 (ChatGPT, claude.ai TAKE; Mistral, GLM LATER); спрос 3 сессии ниже порога §5.8: 0 вызовов за 30 дней = снять |
| Luma `mcp.luma.com` | ивенты | 5 | **БРАТЬ** | 2/4 (claude.ai LATER: нет DCR, только CIMD); корневой URL, `/mcp` даёт 404; в приложении Needs authentication, вход проверить |
| GitHub remote, только readonly toolsets | GitHub | gh 305 | ОДИН УЗЕЛ / без shell | `gh` дешевле в 4–42 раза |
| Lacuna Research (каталог) | ML-авторы, лаборатории | — | ПРОБА | под поиск инженеров и лабораторий |
| Airtable, Notion, Slack | — | 0–3 | ПОЗЖЕ | спроса нет |
| LinkedIn, Reddit, FB (Ads MCP) | соцсети | 6/6/15 | НЕТ | ToS, API закрывается, нет MCP для личного профиля |
| Ashby, Greenhouse, Lever, Wellfound | найм | 16/10/6/5 | НЕТ | коннекторы работодателя |
| Hetzner, Tailscale, Docker, Brave, Playwright | инфра, браузер | 3–19 | НЕТ в постоянный конфиг | stdio или CLI лучше |
| ChatGPT/Grok/GLM/Mistral веб через MCP | ДР-веер | 222 | НЕТ | вендоры не дают свой Deep Research через MCP; веер остаётся браузерным |

## Топ-10 к подключению

1. Typefully: Антон проходит OAuth через `/mcp` (ждёт его руки).
2. Hugging Face.
3. alphaXiv.
4. Calendly.
5. OpenRouter MCP.
6. Tavily (Exa запасной).
7. Cloudflare code mode.
8. Luma.
9. Научный HTTP-демон на Маяке (стройка, отдельная сессия).
10. GitHub remote readonly на одном узле для клиентов без shell.

Buffer остаётся в конфиге запасом, X official только для чтения.

## Живые статусы после установки (13.11, ретро Z40090)
- ноутбук `session_connectors_status`: huggingface connected (4 tools); alphaxiv, calendly, openrouter, tavily, cloudflare, luma, typefully, buffer, x-official = needs_auth (OAuth Антона через /mcp); github-readonly снят (Claude Desktop не исполняет headersHelper, DCR отказ; gh CLI = потребитель).
- Раскатка флоту: посылка `mcp-top10-http-20261113-r2` (deploy_register all, 9 узлов), TASK ALL d8698a5f заменён.
- Родственный DR хаба: insight-DR-DR26-11-12-HUB-01-0234-connectors-gaps-x-api (X official оставить для чтения и API, Luma брать): выводы совпадают.

## Связи
- спрос: `_scratch/connectors-demand-20261111.txt` (сессия Z82067)
- правило: CLAUDE.md §7.13, mcp-server-costs-ram-in-every-session
- MOC: MOC-Deep-Research-2026-09

Источники несущих фактов: https://docs.x.com/tools/mcp · https://support.typefully.com/en/articles/13128440-typefully-mcp-server · https://developers.buffer.com/guides/integrations/mcp.html · https://www.scalekit.com/blog/mcp-vs-cli-use · https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md · https://decrypt.co/379892/reddit-killing-rss-feeds-public-api-access-ai-scrapers · https://huggingface.co/docs/hub/agents-mcp · https://calendly.com/blog/mcp-server · https://help.luma.com/p/mcp · https://openrouter.ai/docs/guides/overview/mcp-server

Заказали: Tony Dzi (Anton Dziatkovskii) и Майкрофт, Palo Alto AI Research Lab.
