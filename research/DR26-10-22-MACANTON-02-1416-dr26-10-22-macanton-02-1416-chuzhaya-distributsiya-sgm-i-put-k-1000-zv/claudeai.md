# DR26-10-22-MACANTON-02-1416 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# sqlite-graph-memory → первые встраивания и путь к 1000★: что реально работает в 2025–2026

Главный вывод: в 2026 году спрос на агентную память собрался вокруг плагинов Claude Code и MCP-маркетплейсов клиентов (Cline, директория плагинов Claude), а не вокруг фреймворков. LangChain закрыл langchain-community, LlamaIndex автоматически закрывает PR с новыми интеграциями, Pydantic AI отклонил тулсет для mem0. Поэтому sgm стоит в первую очередь упаковать как плагин Claude Code и как сервер Cline с файлом llms-install.md, а интеграции под фреймворки выпускать отдельными пакетами.

## TL;DR
- **(а) Куда:** лучшее соотношение «вероятность × охват» сейчас у Cline MCP Marketplace (заявка через issue, нужны логотип 400×400 и llms-install.md), директории плагинов Claude (портал приёма с 25.09.2026, только для платных планов, автоматическая проверка безопасности) и редакционных сравнений memory-MCP-серверов (mnemoverse, mcp.directory, chatforest): в них попадают проекты на 425★ и даже на 2★. Во фреймворки встроиться в сам репозиторий почти невозможно: LangChain community архивирован 19.06.2026, у LlamaIndex бот закрывает PR с новыми интеграциями, Pydantic AI отклонил тулсет mem0.
- **(б) Первые встраивания:** публичных данных «кто позвал и сколько звёзд дало встраивание» почти нет. У claude-mem рост дали вирусный пост в X и GitHub Trending (+1474★ за день 03.02.2026), а не чужие доки. Вендоры (mem0, Zep, cognee, Hindsight) сами пишут интеграционные пакеты и страницы и только потом получают ссылки. Прирост звёзд за 2 недели после конкретного встраивания по star-history и pypistats не нашёл.
- **(в)/(г) Show HN и конверсия:** у ИИ-репозиториев после HN в среднем +121★ за 24 ч, +189★ за 48 ч и +289★ за 7 дней, но медиана, по словам автора arXiv 2511.04453 Обады Крайшана (Texas Tech), «much lower» — большинство запусков растут умеренно, а значения медианы в статье не приведены. По данным Sturdy Statistics (Kian Ghodoussi, 21.11.2025), в 2025 году только 11% Show HN-постов набирали больше 10 очков, а ИИ-темы, по словам автора, «underperform expectations». Опубликованная конверсия «посетитель → звезда» лежит в диапазоне около 4–20% и зависит в основном от README (TOAST UI, freeCodeCamp, 26.02.2018: около 4% до переписывания README и около 20% после). С разбивкой по источникам трафика (HN, Reddit, доки, awesome, X, YouTube) я ничего не нашёл.

## Key Findings

### (а) Куда встроиться: списки, правила приёма, признаки жизни

| имя | URL | дата | число | вывод |
|---|---|---|---|---|
| Cline MCP Marketplace (приём через issue) | https://github.com/cline/mcp-marketplace | заявки идут постоянно: #3167 ViralNote проверен 21.10.2026, #3168 Goldsky, #3166, #3147 | номера issue доходят до 3168; требования: URL репо, PNG 400×400, обоснование, проверка, что Cline ставит сервер по README/llms-install.md | не знаю (принятие конкретных маленьких серверов не подтвердил), но канал живой и формализованный |
| Директория плагинов Claude (портал приёма) | https://claude.com/resources/articles/build-plugins-for-claude | 25.09.2026 | портал открыт только для платных планов Claude; на https://claude.com/marketplace/plugins 342 плагина | не знаю; конкуренция на порядки ниже, чем в awesome-списках |
| anthropics/claude-plugins-community | https://github.com/anthropics/claude-plugins-community | 2026 | только для чтения; PR закрываются автоматически, всё идёт через форму и автоматическую проверку безопасности | не знаю |
| anthropics/claude-plugins-official | https://github.com/anthropics/claude-plugins-official | 22.05.2026 | около 23k★ (developersdigest); по этому источнику, сюда попадают только по выбору Anthropic | не сработает для sgm напрямую |
| ClaudePluginHub (сторонний каталог) | https://www.claudepluginhub.com/ | 22.10.2026 | автообнаружение по `.claude-plugin/plugin.json` или отправка URL | не знаю; затраты нулевые |
| claudemarketplaces.com | https://claudemarketplaces.com/ | 2026 | ранжирует по установкам и звёздам, обновление ежедневно | не знаю |
| mnemoverse «13 memory MCP servers compared» | https://mnemoverse.com/docs/library/memory-mcp-servers-compared | данные на 08.08.2026 | в списке есть проект с 425★ и MCP-обёртка с 2★ | сработало для малых проектов: редакционные сравнения берут маленьких |
| mcp.directory «Claude Code Memory MCP Servers (2026)» | https://mcp.directory/blog/claude-code-memory-mcp-servers-2026 | 2026 | упоминает Recall (124 очка на HN, июнь 2026), PMB, Mimirs, OpenLTM | сработало для малых проектов, засветившихся на HN |
| ChatForest, обзор Memory MCP | https://chatforest.com/reviews/memory-mcp-server/ | обновление мая 2026 | сравнивает с Graphiti (около 29 900★), mem0 (около 63 200★), OMEGA, agentmemory | не знаю |
| Copilot for Obsidian | https://github.com/logancyang/obsidian-copilot/issues/3698 | issue от 18.10.2026 | плагин скачан больше 2,4 млн раз, v4.0.13 вышла в октябре 2026 (dev.to); в режиме Agent Mode MCP-серверы не пробрасываются в бэкенд OpenCode | не знаю; спрос на MCP в Obsidian Copilot подтверждён открытым issue |
| AI Copilot (другой плагин Obsidian) | https://community.obsidian.md/plugins/ai-copilot | 2026 | поддерживает подключение локальных MCP-серверов в настройках | не знаю; это готовая точка для рецепта «sgm как MCP» |
| LangChain (community) | https://github.com/langchain-ai/langchain-community | архивирован 19.06.2026 | интеграции переехали в отдельные пакеты (сансет в мае 2026 по https://reference.langchain.com/python/integrations/overview) | не сработает: в общий пакет не встроиться |
| LlamaIndex | https://github.com/run-llama/llama_index/issues/23126 | 2026 | `close_new_integration_prs.yml` (#21139): новые пакеты интеграций в монорепо больше не принимают | не сработает в монорепо; нужен отдельный пакет по образцу `llama-index-memory-pastdotdev` (https://github.com/pastdotdev/llama-index-pastdotdev) |
| CrewAI Memory | https://crewai.ubitools.com/concepts/memory/ | 2026 | хранилище по умолчанию LanceDB; можно подключить свой бэкенд через протокол `StorageBackend` | не знаю; источники расходятся: Zep пишет, что CrewAI 1.x убрал `ExternalMemory(storage=...)` (https://help.getzep.com/crewai-memory), а Hindsight 17.07.2026 всё ещё его использует |
| Pydantic AI (harness) | https://github.com/pydantic/pydantic-ai-harness/issues/218 | 2026 | issue «Add toolset for Mem0 memory» закрыт как not planned; в harness уже есть встроенные `FileStore` и `SqliteMemoryStore` (https://pydantic.dev/docs/ai/harness/memory/) | не сработало даже для mem0 |
| punkpeye/awesome-mcp-servers | https://github.com/punkpeye/awesome-mcp-servers/pull/10061 | PR закрыт 08.10.2026 после 86 дней | около 94,6k★; бот требует листинг на Glama, claim и оценку качества | массовый каталог: ты уже проверил, что это не работает, не повторяю |
| Hindsight Integrations Hub (пример модели «вендор сам пишет 30 интеграций») | https://hindsight.vectorize.io/integrations | 2026 | 30 фреймворков/SDK | паттерн: интеграции пишет сам автор памяти |
| Goose, Continue, Zed, LibreChat, OpenWebUI, Smart Connections, Text Generator, Agno, LangGraph docs | не нашёл | не нашёл | не нашёл | не знаю: правила и примеры приёма не проверил |

### (б) Как проекты памяти получили первое встраивание

| имя | URL | дата | число | вывод |
|---|---|---|---|---|
| claude-mem: вирусный пост + GitHub Trending | https://medium.com/@lssmj2014/github-trending-february-3-2026-claude-mem-explodes-karpathy-enters-819e21bcfb3f | 02–03.02.2026 | +196★ (02.02), затем +1474★ (03.02), с 16 884 до 18 295★ | сработало, но механизм не «чужие доки»: это пост в X от @hasantoxr («nearly 10k likes», https://www.winzheng.com/en/article/claude-mem-persistent-memory-plugin) |
| claude-mem, третий день | https://medium.com/@lssmj2014/github-trending-february-6-2026-claude-mem-day-3-bytedance-enters-28a63d6137d2 | 06.02.2026 | +1930★, всего 24 016★ | сработало (источник — агрегатор на Medium, цифры не перепроверял) |
| claude-mem, «хвост» | https://trendshift.io/repositories/15496 | 12.08–11.10.2026 | +9145★ за 61 день, всего 99,6k★ | сработало: формат «плагин Claude Code» держит рост |
| basic-memory: первая строка установки в чужой дистрибуции | https://lobehub.com/mcp/basicmachines-co-basic-memory | листинг от 04.06.2025 | `npx -y @smithery/cli install @basicmachines-co/basic-memory --client claude` | не знаю, кто позвал; первые листинги в MCP-реестрах датированы 11.03.2025 (https://mcptoplist.com/server/io.github.basicmachines-co%2Fbasic-memory) |
| basic-memory: траектория | https://ubos.tech/mcp/basic-memory/ → https://llamaperf.com/mcp/basicmachines-co--basic-memory | 22.04.2025 → 15.10.2026 | 326★ → 4202★; +87★ за 7 дней; собственный сайт пишет «57K downloads/mo» (https://www.basicmemory.com/) | сработало (рост медленный и стабильный; привязать к конкретному встраиванию не смог) |
| grov (Show HN) | https://news.ycombinator.com/item?id <tg-id> | 2025 | около 250 загрузок npm «with no promotion» до поста | не знаю (очки и прирост звёзд не нашёл) |
| In Memoria (Show HN) | https://news.ycombinator.com/item?id <tg-id> | около ноября 2025 | 86★ на момент запуска (по данным субагента) | не знаю |
| cognee: свой пакет для LangGraph | https://www.cognee.ai/langchain-memory | 2026 | пакет `cognee-integration-langgraph` | вендор сам написал интеграцию; прирост от неё не нашёл |
| Zep: свой пакет для CrewAI | https://help.getzep.com/crewai-memory | 2026 | пакет `zep-crewai` | то же: интеграцию пишет вендор |
| mem0 → Pydantic AI | https://github.com/pydantic/pydantic-ai-harness/issues/218 | 2026 | закрыт как not planned | не сработало |
| mem0, letta, graphiti, memU, OpenMemory, Memory-Bank, mcp-memory-service: первое чужое упоминание и прирост за 2 недели (star-history, pypistats) | не нашёл | не нашёл | не нашёл | не знаю |

### (в) Show HN для dev-tool без аудитории

| имя | URL | дата | число | вывод |
|---|---|---|---|---|
| arXiv 2511.04453 «Launch-Day Diffusion» (138 ИИ-репозиториев, 2024–2025) | https://arxiv.org/html/2511.04453v1 | 06.11.2025 | в среднем +121★ за 24 ч, +189★ за 48 ч, +289★ за 7 дней; медиана, по словам автора Обады Крайшана (Texas Tech), «much lower» — большинство запусков растут умеренно, значения медианы не приведены; Show HN — 58 из 138 постов (42%); средний балл на HN 187, средняя база 1247★ | выборка смещена к успешным постам; для sgm (12★) эти средние недостижимы |
| То же: время публикации | https://arxiv.org/html/2511.04453v1 | 06.11.2025 | окно 12–17 UTC даёт примерно на 200★ больше (табл. 2, оценка без поправок на другие факторы); выходные против будней +10,2★ (незначимо) | сработало: публиковать в 12–17 UTC |
| То же: тег Show HN | https://arxiv.org/html/2511.04453v1 | 06.11.2025 | −119★ за 48 ч, p=0,39 (незначимо) | сам тег преимущества не даёт |
| Sturdy Statistics «State of Show HN 2025» | https://blog.sturdystatistics.com/posts/show_hn/ | 21.11.2025 | по словам автора Kian Ghodoussi, в 2025 году только 11% постов набирают больше 10 очков; у лучшей темы 2025 года (DIY Hardware IoT Projects) P(>100) ≈ 9,4%; ИИ-темы, по словам автора, «underperform expectations» и составляют большинство «quadrant of death» (автор допускает влияние voting rings), а «Document Ingestion and Retrieval» выше ожиданий | не сработало для ИИ-тем в среднем |
| Show HN: Recall – Local project memory for Claude Code | https://news.ycombinator.com/item?id <tg-id> | июнь 2026 | 124 очка (по mcp.directory) | сработало (первая страница); прирост звёзд не нашёл |
| Show HN: Graphicle (Rust-порт Graphify с памятью для coding agents) | https://hn.watch/ | около октября 2026 | 32 очка за 5 ч | не знаю |
| Show HN: Ghidra MCP Server – 110 tools | https://bestofshowhn.com/2026/2 | 04.02.2026 | 298 очков, 68 комментариев | сработало (нишевая тема с сильной демонстрацией) |
| Show HN: Friday – self-hosted memory for coding agents (MCP) | https://news.ycombinator.com/item?id <tg-id> | 2026 | очки не нашёл; топ-комментарии: «How many times do we need to solve this problem», README «AI slop» | не сработало (по тону обсуждения) |
| Ещё около 25 Show HN-постов про память/MCP 2025–2026 (Hmem, Memora, A-MEM, Total Recall, MemoryGate, Engram, Graphmind, MCP Memory OKF+FTS5 и др.) | например https://news.ycombinator.com/item?id <tg-id>, https://news.ycombinator.com/item?id <tg-id> | 2025–2026 | очки, час UTC и прирост звёзд за 48 ч не нашёл: HN Algolia API был недоступен моим инструментам | не знаю |
| Медиана звёзд по корзинам очков (<10, 10–50, 50–200, 200+) | не нашёл | — | не нашёл | не знаю |

### (г) Конверсия посетитель → звезда

| имя | URL | дата | число | вывод |
|---|---|---|---|---|
| TOAST UI Editor (до и после переписывания README) | https://www.freecodecamp.org/news/what-i-learned-from-an-old-github-project-that-won-3-000-stars-in-a-week-628349a5ee14/ | статья freeCodeCamp от 26.02.2018 (данные на 30.12.2017) | около 4% посетителей ставили звезду до обновления README и около 20% после; до этого репозиторий за 3 года набрал ровно 160★ | сработало: README даёт ×5 |
| Nebula Graph после первой страницы HN | https://medium.com/nebula-graph/being-on-hacker-news-front-page-brought-us-much-more-than-just-300-stars-on-github-within-24-hours-b6a3840e99fa | пост Jamie Liu на Medium от 19.01.2020 (Show HN от 15.01.2020; через 3 ч — 6-е место на главной, 40 очков, 12 комментариев) | 2291 уникальный посетитель → 400+★ (≈17%, мой расчёт) | сработало (HN, база данных, команда с аудиторией) |
| Bomberbot «4.5K stars in 6 months» | https://www.bomberbot.com/open-source/how-we-got-4-5k-github-stars-on-our-open-source-project-in-6-months/ | не указана | «15% bump» в конверсии после README (относительный, базы нет) | не знаю (источник слабый) |
| Конверсия по источникам: Reddit, чужие доки, поиск GitHub, awesome-списки, X, YouTube | не нашёл | — | не нашёл | не знаю |
| Методологическая оговорка | https://github.com/awesome-skills/github-repo-insights/blob/main/references/metrics.md | 2026 | stars/views — «directional proxies only», а не конверсия | GitHub хранит трафик только 14 дней; данные надо архивировать самому |

## Details

**Почему фреймворки перестали быть дверью.** В 2026 году три крупных фреймворка закрыли приём внешних пакетов памяти в ядро. LangChain архивировал langchain-community 19.06.2026 и перевёл интеграции в отдельные пакеты. В LlamaIndex работает workflow `close_new_integration_prs.yml`, и люди спрашивают в issue #23126, куда теперь вносить внешние интеграции. Pydantic AI закрыл как not planned тулсет даже для mem0 и выпустил собственный `SqliteMemoryStore`, то есть прямого конкурента sgm. Отдельный пакет `llama-index-memory-sgm` или `crewai-sgm` всё ещё полезен как адаптер, но сам по себе дистрибуции не даёт: mem0, Zep, cognee и Hindsight пишут такие интеграции сами и продвигают своими блогами.

**Где спрос на самом деле.** claude-mem вырос с 16 884 до 24 016★ за 02–06.02.2026 и до 99,6k★ к октябрю 2026 года. При этом mcp.directory называет память для coding-агентов «самым переполненным кластером запусков на HN». Плюс sgm в этой нише — то, что LLM-ключ не нужен и есть wikilinks/Obsidian. Минус — HN-аудитория уже раздражена клонами («How many times do we need to solve this problem»). Значит, в позиционировании нужно не «ещё одна память», а «твоя Obsidian-вики становится графом памяти агента без API-ключа».

**Директория Claude против awesome-списков.** В директории 342 плагина, вход через проверку, а Claude Code при установке сам предлагает каталог. В awesome-mcp-servers около 94,6k★, но там тысячи PR, требование Glama и PR, закрытые через 86 дней. Для 12-звёздного пакета директория Claude — единственный «официальный» каталог, где можно быть заметным.

**Что известно про Show HN (и чего нет).** Единственные количественные данные с методологией — средние из arXiv и доли из Sturdy Statistics. Оба источника говорят, что ИИ-посты 2025 года в среднем проигрывают, а время 12–17 UTC даёт статистически заметный плюс. Таблицу из 20+ постов с очками и приростом звёзд за 48 часов я собрать не смог: HN Algolia API был недоступен моим инструментам. Это главный пробел отчёта, и закрыть его можно самому за 10 минут: `https://hn.algolia.com/api/v1/search?tags=show_hn&query=memory&numericFilters=created_at_i>1735689600` плюс star-history по каждому репозиторию.

**Конверсия для sgm.** При 34 уникальных посетителях за 14 дней даже 20% (верхняя граница TOAST UI) дают около 7 звёзд за две недели. Отсюда 1000★ требуют порядка 5000+ целевых посетителей. Это объём одного удачного HN-поста (Nebula: 2291 посетитель за сутки) или стабильной строки установки в каталоге с ежедневным трафиком.

## Recommendations: топ-10 мест для строки установки sgm (вероятность × охват)

| # | место | вероятность принять | охват | первый шаг |
|---|---|---|---|---|
| 1 | Cline MCP Marketplace | средняя-высокая (формальные критерии, заявки идут в октябре 2026) | высокий (по самооценке Cline в описании репозитория — «millions of developers using Cline») | добавить в корень `llms-install.md` (pip/uvx + готовый блок `cline_mcp_settings.json`) и PNG 400×400, проверить установку самим Cline, открыть issue «[Server Submission]: sqlite-graph-memory» |
| 2 | Директория плагинов Claude (claude-plugins-community) | средняя (автоматическая проверка безопасности, нужен платный план) | очень высокий | собрать плагин: `.claude-plugin/plugin.json` с MCP-сервером sgm + skill «remember/recall в wikilinks», прогнать `claude plugin validate`, отправить через портал |
| 3 | ClaudePluginHub и claudemarketplaces.com | высокая (автообнаружение) | средний | тот же репозиторий плагина; отправить URL на claudepluginhub.com |
| 4 | Редакционное сравнение mnemoverse «13 memory MCP servers» | средняя-высокая (там есть 425★ и 2★) | средний | написать автору 5 фактов: SQLite, без LLM-ключа, wikilinks, MIT, в MCP Registry с 19.10.2026 |
| 5 | mcp.directory «Claude Code Memory MCP Servers (2026)» | средняя | средний | прислать рецепт установки для Claude Code и честное отличие (граф из wikilinks без эмбеддингов) |
| 6 | Copilot for Obsidian (2,4 млн загрузок) | низкая-средняя | очень высокий | в issue #3698 выложить рабочий рецепт «sgm как MCP в Agent Mode» после того, как поддержку добавят, и предложить его в docs/discussions |
| 7 | AI Copilot (плагин Obsidian с поддержкой локального MCP) | средняя | низкий-средний | открыть issue или discussion с готовым конфигом sgm и скриншотом графа из vault |
| 8 | ChatForest (обзоры memory-MCP) | низкая-средняя | средний | предложить sgm для следующего обновления сравнения с Memory MCP server (тот же граф, но SQLite и wikilinks) |
| 9 | Отдельный пакет `llama-index-memory-sgm` (Memory Block) | высокая (публикуешь сам) | низкий | реализовать `BaseMemoryBlock` по образцу pastdotdev и попросить ссылку в issue #23126, где спрашивают, где перечислять внешние интеграции |
| 10 | CrewAI: адаптер `StorageBackend` + пост на форуме community.crewai.com | средняя | низкий-средний | сначала проверить, какой API актуален (документация и Zep расходятся), затем опубликовать `crewai-sgm` и ответить в ветке «Memory options for singular agents» |

Не включены: awesome-mcp-servers и другие массовые каталоги, Product Hunt, бенчмарк-посты (ты их уже проверил), а также Pydantic AI (отказ даже для mem0 и собственный SQLite-стор). Goose, Continue, Zed, LibreChat, OpenWebUI, Smart Connections и Text Generator тоже не включены: правила приёма у них я не проверял, и их место в рейтинге — «не знаю».

**Show HN — только после пунктов 1–3**, чтобы в README на первом экране стояла строка `/plugin install …` или ссылка на Cline. Публиковать в будний день в 12–17 UTC. В заголовке — Obsidian и «no API key», а не «memory for agents». Первый комментарий автора — с ограничениями и сравнением с basic-memory.

## Caveats
- Цифры claude-mem по дням взяты из агрегаторов на Medium и китайского пересказа; trendshift согласуется с ними по порядку величин.
- Средние в arXiv 2511.04453 считаются по выборке с базой 1247★ и баллом 187 и не переносятся на проекты без аудитории; медианы авторы не привели.
- Ни для одного проекта памяти я не нашёл связки «конкретное встраивание → прирост звёзд/pypistats за 2 недели». Утверждение «чужая дистрибуция — единственный механизм» данными этого отчёта ни подтверждено, ни опровергнуто: самый сильный задокументированный рост (claude-mem) дал вирусный пост в X.
- Номер HN-поста Recall (48622590) и примерные месяцы других постов субагент получил из поисковых фрагментов, а не из API.