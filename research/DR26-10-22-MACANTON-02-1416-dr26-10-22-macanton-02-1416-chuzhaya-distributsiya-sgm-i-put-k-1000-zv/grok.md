# DR26-10-22-MACANTON-02-1416 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала сверю скиллы и живые источники по чужой дистрибуции и росту малых OSS-пакетов памяти.Параллельно сниму внутренний след и живые каталоги, репо памяти и Show HN.Дальше открываю CONTRIBUTING, страницы интеграций и Show HN с живыми цифрами.Добираю недостающие живые цифры по каталогам, Show HN и конверсии, затем сразу полный отчёт.DR26-10-22-MACANTON-02-1416 — sqlite-graph-memory: чужая дистрибуция и путь к 1000 звёзд

Дата отчёта: 2026-10-22. Источники открыты в этом прогоне. Числа — снимок дня, не прогноз.

🎙 Майк: путь к 1000★ у sgm не через бенчмарк-посты и не через Show HN как стратегию. Единственная живая дверь — строка установки в чужом каталоге, который уже читают агенты. Официальный MCP Registry уже есть; следующий ход — Knowledge & Memory у punkpeye и Emerging у TeleAI. Show HN для пакета без аудитории почти всегда 1–13 очков.

Пробелы внутри отчёта помечены «не нашёл». Не выдумываю 2-недельные дельты звёзд и PyPI, если star-history/pypistats за конкретное окно не открывал. Конверсия README→звезда по источникам трафика как полная таблица в индустрии не опубликована.

---

A. Куда встроиться

Критерий кандидата: страница держит СПИСОК бэкендов памяти / RAG / MCP-memory и принимает внешний пакет. Не кандидат: страница провайдеров LLM, yaml-конфиг без каталога, закрытый вендорский marketplace без двери.

A1. Каталоги агентной памяти (самые релевантные)

| имя | URL | дата снимка | число | как принимают | правила / отказ | возраст | вывод |
|---|---|---|---|---|---|---|---|
| TeleAI Awesome-Agent-Memory | https://github.com/TeleAI-UAGI/Awesome-Agent-Memory | 2026-10-22 | 691★, 138 forks, 732 commits | PR | <100★ → collapsed Emerging; CI star-order ±10; описание ≤25 слов, без маркетинга; 1 ресурс/PR; RAG/vector DB вне скоупа | жив, коммиты Sep 2026 (Graphmem #117 2026-09-25) | сработало для малых: Graphmem в Emerging 2026-09-25; Fidelis #113 2026-09-20. sgm (12★) сюда входит |
| CONTRIBUTING TeleAI | https://github.com/TeleAI-UAGI/Awesome-Agent-Memory/blob/main/CONTRIBUTING.md | 2026-10-22 | — | PR | нет PyPI-ссылок; factual; drop-in replacement гнездится под родителем | — | дверь открыта |
| mnemoverse/awesome-agent-memory | https://github.com/mnemoverse/awesome-agent-memory | 2026-10-22 | 35 commits; звёзды в scrape GitHub не отдал | PR, 1 tool, alphabetically | factual, no marketing | MCP-секция уже есть: Basic Memory, Graphmem, Selvedge, Synapse | сработало для малых MCP: Graphmem, Selvedge, Synapse уже в MCP section |
| CONTRIBUTING mnemoverse | https://github.com/mnemoverse/awesome-agent-memory/blob/main/CONTRIBUTING.md | 2026-10-22 | — | PR | one tool per PR | — | дверь открыта |
| punkpeye/awesome-mcp-servers | https://github.com/punkpeye/awesome-mcp-servers | 2026-10-22 | 96.7k★, 18.3k forks, 13 594 commits | PR в README, секция Knowledge & Memory | alphabetically; public GitHub repo; remote-only → другой список; агентам `🤖🤖🤖` в title | LICENSE ~2024-11-30; merge живой (PR #16706 2026-10-18 в прошлом заходе) | сработало: мелкие MCP memory регулярно мержатся (m3-memory #5340 2026-04-24; cortex-plugin, shodh-memory, midas, nexo в README) |
| CONTRIBUTING punkpeye | https://raw.githubusercontent.com/punkpeye/awesome-mcp-servers/main/CONTRIBUTING.md | 2026-10-22 | — | PR | format, alpha, one server/line | — | дверь открыта, охват максимальный |
| Coworker «9 Best MCP Memory Servers» | https://coworker.ai/blog/best-mcp-memory-servers | 2026-10-06, updated 2026-10-15 | editorial, не каталог PR | не принимают пакеты | сравнивает 9 серверов; звёзды на 2026-10-06 | — | не дверь встраивания; полезен как карта конкурентов |

Малые пакеты, недавно попавшие в списки (<500★ на момент добавления, где видно):

| имя | URL | дата | число | вывод |
|---|---|---|---|---|
| Graphmem в TeleAI Emerging | https://github.com/TeleAI-UAGI/Awesome-Agent-Memory/commits/main (#117 2026-09-25) | 2026-09-25 | не нашёл точных ★ на дату PR | сработало (приняли) |
| Fidelis Memory Emerging | тот же commits | 2026-09-20 | не нашёл ★ на дату | сработало (приняли) |
| Graphmem / Selvedge / Synapse в mnemoverse MCP | https://github.com/mnemoverse/awesome-agent-memory | 2026-10-22 | Graphmem/Selvedge/Synapse — малые MIT MCP; точные ★ на дату добавления не нашёл | сработало |
| m3-memory в punkpeye Knowledge & Memory | https://github.com/punkpeye/awesome-mcp-servers/actions/runs/24908088126 | 2026-04-24 | не нашёл ★ на дату | сработало |
| crewai-memory-cosmosdb | https://pypi.org/project/crewai-memory-cosmosdb/ | 2026-09-20 | PyPI 0.1.x, не GitHub-каталог | сработало как адаптер, не как строка в awesome |

A2. MCP-клиенты и реестры

| имя | URL | дата | число | как принимают | отказ | возраст | вывод |
|---|---|---|---|---|---|---|---|
| Official MCP Registry | https://modelcontextprotocol.io/registry/about | 2026-10-22 | preview; экосистема агрегаторов | `mcp-publisher` + server.json; namespace GitHub/DNS/HTTP | private servers нет; spam/takedown по moderation policy | 2025–2026 | sgm уже там с 19.10.2026 (контекст заказа). Это канон, не витрина пользователей |
| PulseMCP | https://www.pulsemcp.com/ | 2026-10-22 | ~21.7k servers | ingest Official Registry daily, process weekly | **новые сабмишены paused** (с Sep 2026, FAUN 2026-10-06) | с Dec 2024 | не слать форму; ждать ingest |
| PulseMCP submit | https://www.pulsemcp.com/submit | 2026-10-22 | — | «publish to Official Registry» | прямая форма закрыта | — | не сработало как дверь сейчас |
| Glama | https://glama.ai/mcp/servers | 2026-10-22 | 108 273 servers | auto-index + claim; Add Server | не нашёл публичных критериев отказа | живой индекс | сработало как витрина; claim после registry |
| MCP Toplist | https://mcptoplist.com/ | 2026-10-22 | 150 963 active; 80% серверов в одном реестре | считает Official+Glama+Smithery+mcp.so+PulseMCP | не принимает руками | Week 43 2026 | не дверь, метрика покрытия |
| Cline MCP Marketplace | https://github.com/cline/mcp-marketplace | 2026-10-22 | 36 commits; ★ в scrape не отдал (ранее ~795) | **issue form**, не PR | community adoption, maturity, security; few stars OK (FAQ) | живой | сработало как one-click Cline; нужен README/`llms-install.md` |
| Zed MCP extensions | https://zed.dev/docs/extensions/mcp-extensions | (прошлый заход) | — | extensions → deprecation toward official registry | не строить отдельный Zed-пакет | — | встраивание = registry, не Zed gallery |
| Goose | https://goose-docs.ai/extensions/ | (прошлый заход) | — | user-add любой MCP; Knowledge Graph Memory как пример | не открытый package catalog | — | пример в доке ценнее, чем PR в каталог; отдельной двери списка не нашёл |
| LibreChat | yaml mcpServers | — | — | конфиг пользователя | нет публичного каталога пакетов | — | не кандидат |
| Open WebUI | community tools + MCP via HTTP/MCPO | — | — | native memory tools свои | не список внешних memory backends | — | не кандидат как список бэкендов |
| Continue | https://docs.continue.dev + hub.continue.dev | 2026-10-22 | — | user yaml / hub | не открытый memory-backend list | — | не кандидат |
| Cursor Marketplace | https://cursor.com/marketplace | 2026-10-22 | vendor plugins | publish + manual review, OSS | не список memory backends | — | слабая дверь; MCP directory «open an issue to request» (прошлый заход) |
| Gemini CLI gallery | topic `gemini-cli-extension`, daily crawl | (прошлый заход) | — | GitHub topic crawl | не ручной PR | — | кандидат, если сделать extension + topic |
| Claude Code official plugins | marketplace Anthropic | 2026-10-21 (прошлый заход) | ~315 official plugins | вендорский | community marketplace отдельно | — | claude-mem туда попал как plugin, не как pip-пакет |
| hesreallyhim/awesome-claude-code | https://github.com/hesreallyhim/awesome-claude-code | 2026-10-22 | 2 212 commits; ★ scrape не отдал (~десятки тысяч в прошлом) | **только issue form**, PR запрещён | ≥14 дней И active commits ИЛИ ≥100★; 1 ресурс; human via web UI, не `gh` | живой | для sgm сейчас отказ: 12★ и нет Claude-Code-native plugin. claude-mem упомянут (badge в README claude-mem) |

A3. Фреймворки агентов (страницы memory)

| имя | URL | дата | как принимают | вывод |
|---|---|---|---|---|
| CrewAI Memory / StorageBackend | https://docs.crewai.com/en/concepts/memory | 2026-06-11+ | код: `Memory(storage=StorageBackend)` | **не список**. Shortcut `memory_config={"provider":"mem0"}` снят в 1.10 (2026-02-27, прошлый заход). Дверь = адаптер + чужой гайд |
| m3-memory CrewAI adapter | https://github.com/skynetcmd/m3-memory/tree/main/m3_memory/integrations/crewai | 2026-10-22 | drop-in StorageBackend, CrewAI ≥1.10 | сработало для малого пакета: строка `pip install "m3-memory[crewai]"` в чужом README |
| crewai-dakera docs PR | https://github.com/crewAIInc/crewAI/issues/6409 | 2026-07-01 | PR в docs CrewAI | попытка встроить docs page; исход мержа в этом прогоне не нашёл |
| Hindsight × CrewAI | https://hindsight.vectorize.io/sdks/integrations/crewai | 2026-10-15 | ExternalMemory storage | чужая дистрибуция работает, когда вендор сам пишет гайд |
| LangChain | LangMem official; community Mem0Memory/ZepMemory | — | partner/community packages | ядра LangChain как списка «любой memory backend» нет. Веер PR в core — известный антипаттерн |
| LlamaIndex | own Memory | — | нет 3rd-party memory-backend list | не кандидат |
| Agno | MemoryManager + db; Hindsight/Mem0Tools | — | 3rd-party tools, не каталог | слабая дверь |
| Pydantic AI Harness Memory | https://pydantic.dev/docs/ai/harness/memory/ | 2026-10-22 | backends `memory`/`file`/`sqlite`/`postgres`; Protocol MemoryStore | **не список чужих пакетов**. SqliteMemoryStore уже встроен. sgm как drop-in сюда не просят; кастомный store — код, не PR в доки |

A4. Obsidian LLM-плагины

| имя | вывод |
|---|---|
| Smart Connections, Copilot for Obsidian, Text Generator | списки **LLM-провайдеров**, не memory backends. Слота «поставь pip-пакет памяти» нет. не кандидат |

A5. Awesome «второй мозг» / PKM+AI

Отдельного живого awesome PKM+AI со слотом «агентная память SQLite» с правилами приёма малых пакетов, кроме TeleAI/mnemoverse, в этом прогоне не нашёл. Punkpeye Knowledge & Memory — ближайший PKM-смежный слот.

---

B. Как маленькие проекты агентной памяти получили первое встраивание

Точные «+N звёзд / +N PyPI за 14 дней после первого чужого упоминания» почти ни у кого не опубликованы. Где нет star-history окна — «не нашёл».

| имя | URL | первое чужое встраивание (что нашёл) | звёзды сейчас | PyPI | 2 недели после встраивания | вывод |
|---|---|---|---|---|---|---|
| basic-memory | https://github.com/basicmachines-co/basic-memory | Coworker #2 local MCP 2026-10-06 (4 106★); mnemoverse MCP section; glama.json + smithery.yaml + server.json + llms-install.md в репо | 4 106★ на 2026-10-06 (Coworker) | https://pypistats.org/packages/basic-memory 2026-10-22: 1 304/день, 7 981/нед, 27 770/мес (включает CI) | не нашёл дельту 14д после первого awesome | сработало долгосрочно: Markdown+wikilinks+Obsidian+MCP без ключа. Рост = продукт + облако $15, не одно встраивание |
| mcp-memory-service (doobidoo) | https://github.com/doobidoo/mcp-memory-service | Coworker #3, 1 987★ на 2026-10-06 | 1 987★ | не открывал pypistats | не нашёл | сработало до ~2k через self-host REST+MCP, не через один список |
| cognee | https://github.com/topoteretes/cognee | punkpeye Knowledge & Memory (исторически); Coworker #7, 31 477★ 2026-10-06 | ~31.5k | прошлый заход pypistats last month 86 048 (2026-10-12) | не нашёл 14д после первого списка | сработало как фреймворк+cloud, не как «строка в awesome» |
| graphiti | https://github.com/getzep/graphiti | Zep paper arXiv:2501.13956; LangGraph гайд help.getzep.com; Coworker #6, 31 488★ 2026-10-06 | ~31.5k | прошлый заход 619 128/мес (2026-10-12) | не нашёл | сработало: компания Zep + paper + SDK. MCP в репо помечен experimental |
| mem0 | https://github.com/mem0ai/mem0 | YC S24; LangGraph/CrewAI integrations docs; Coworker 66 671★ 2026-10-06 | ~67k | pepy в прошлом заходе 45.2M total / 2.35M/30d на 2026-09-25 | не нашёл 14д после первого SDK | сработало как платформа. CrewAI shortcut убран в 1.10 — чужая дистрибуция обратима |
| OpenMemory | Mem0 local MCP | sunset Apr 2026, removed 2026-07-29 https://github.com/mem0ai/mem0/pull/6530 (Coworker) | — | — | — | не сработало: продукт сняли |
| letta (MemGPT) | arXiv:2310.08560 | academic paper → продукт; MIND.ai 23 081★ на 2026-09-07 | ~23k на 2026-09-07 | не открывал | не нашёл | сработало через paper+framework, не через MCP-каталог. Coworker специально не ставит Letta в «memory MCP» |
| memU | https://github.com/NevaMind-AI/memU | mnemoverse OSS frameworks | не нашёл живых ★ в этом прогоне | не открывал | не нашёл | не знаю прирост |
| claude-mem | https://github.com/thedotmack/claude-mem | запуск Aug 2025; Awesome Claude Code (badge в README); Claude Code plugin marketplace | 96 389★ на 2026-10-05 (https://news.one-man-company.com/news/hot-repo-claude-mem-96k-stars); 102 302 на 2026-10-20 (https://www.claudepluginhub.com/marketplaces/thedotmack-thedotmack) | npm, не PyPI | не нашёл 14д после первого plugin listing; 13 мес → ~96k | сработало: native plugin в горячем харнессе, не pip. Не аналог sgm |
| Memory-Bank | паттерн Cursor/Cline | не пакет | — | — | — | не применимо |
| Knowledge Graph Memory (official) | https://github.com/modelcontextprotocol/servers | reference в протоколе | 91 044★ на весь servers repo 2026-10-06 (Coworker) | npm @modelcontextprotocol/server-memory | — | сработало потому что это референс протокола |
| Graphmem / Selvedge / Synapse | mnemoverse MCP | первое встраивание = awesome list | малые | — | не нашёл 14д дельту | приняли; прирост звёзд не нашёл |
| Remembrane | https://news.ycombinator.com/item?id <tg-id> | Show HN 13 pts, SQLite zero-deps + LangChain/CrewAI adapters | не открывал GitHub ★ | — | не нашёл 48h stars | слабый HN, адаптеры заявлены автором |

Кто позвал / как нашли: ни у одного малого пакета не нашёл публичного «нас пригласил мейнтейнер X». Типичный путь: сам PR в awesome / issue в Cline / server.json в Official Registry / адаптер StorageBackend, который вендор или соседний пакет документирует.

Сработало: native plugin в Claude Code (claude-mem); paper+company (graphiti/letta/mem0); local markdown+Obsidian+MCP с тулзингом под каждый клиент (basic-memory). Не сработало как единственный рычаг: Show HN 1–13 pts у SQLite/MCP memory; OpenMemory sunset; CrewAI mem0 shortcut снят.

---

C. Show HN для dev-инструмента без аудитории

Корпус распределения (не «наши 20 постов», а вся площадка):

| имя | URL | дата | число | вывод |
|---|---|---|---|---|
| Daniel King, Show HN by the Numbers | https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/ | 2026-04-23 | 188 085 постов 2012–Apr 2026; median 2 pts; mean 13.5; p90=24; p99=263; 50 pts = top 6%; 28 302 Show HN в 2025; ~200/день в 2026 | норма = 2 очка |
| King, звёзды | тот же | 2026-04-23 | 491 репо с HN score ≥258; 1.4 GitHub stars / HN point за 48h; r=0.29; comments r=0.10; half-life 24h; 92% звёзд к 48h; Day1 509★ vs 0.4 baseline для successful | 1.4 — выборка ХИТОВ (≥258), не медиана всех Show HN |
| King, слот | тот же | 2026-04-23 | Monday 00:00 UTC = 10.8% шанс 50+; Thursday 06:00 UTC = 2.6% | лучший слот: воскресный вечер US |
| King, личный пост Burnish | тот же | 2026-04-23 | 1 point, автор в «29%» | не сработало без аудитории |
| jonno.nz year-in-numbers | https://jonno.nz/posts/2026-07-02-show-hn-year-in-numbers/ | 2026-10-22 | 404 Not Found | не нашёл живую страницу |
| LavX / snubi | https://news.lavx.hu/article/show-hn-posts-are-rising-while-their-scores-are-falling | 2026-01-21 | Dec 2025 avg Show HN 9.04 vs stories 19.53; Show HN >12% всех сторис | шум вырос, скоры упали |
| arXiv 2511.04453 Launch-Day Diffusion | https://arxiv.org/abs/2511.04453 | 2025-11-06 | 138 AI/LLM репо 2024–2025 после HN: 121★/24h, 189★/48h, 289★/week среднее | среднее по HN-exposed AI tools, не медиана Show HN и не «без аудитории» |
| daily.dev Ads paraphrasing King | https://business.daily.dev/resources/hacker-news-marketing-developer-tools-show-hn-launch-day-sustained-coverage/ | 2026-06-03 | 1.4 stars/upvote; 2.3% submissions front page Q1 2026 | вторичный пересказ King |

20+ Show HN про memory / MCP / SQLite-агентов (очки живые; 48h stars — не нашёл, кроме оговорок):

| # | имя | URL | дата (относит. к 2026-10-22) | очки | 48h stars | вывод |
|---|---|---|---|---|---|---|
| 1 | MCP Memory OKF+FTS5 | https://news.ycombinator.com/item?id <tg-id> | ~69 days ago | 71 | не нашёл | сработало до 1-й страницы; тред «это grep по markdown»; автор отвечал в первые часы |
| 2 | Remembrane SQLite zero-deps | https://news.ycombinator.com/item?id <tg-id> | ~56 days | 13 | не нашёл | не 1-я страница |
| 3 | Mindpm SQLite MCP | https://news.ycombinator.com/item?id <tg-id> | ~32 days | 1 | не нашёл | не сработало |
| 4 | Agent Recall SQLite/MCP | https://news.ycombinator.com/item?id <tg-id> | ~4 days | 1 | не нашёл | не сработало; авторский first comment есть |
| 5 | Hmem hierarchical SQLite | https://news.ycombinator.com/item?id <tg-id> | ~7 months | 2 | не нашёл | «My Claude told me to post» — убийца |
| 6 | Engram Bun+SQLite | https://news.ycombinator.com/item?id <tg-id> | ~6 months | 3 | не нашёл | не сработало |
| 7 | sqlite-memory-MCP | https://news.ycombinator.com/item?id <tg-id> | ~4 months | 2 | не нашёл | не Show HN tag в заголовке; 2 pts |
| 8 | Context Harness SQLite | https://news.ycombinator.com/item?id <tg-id> | ~7 months | 2 | не нашёл | не сработало |
| 9 | Local-first LTM MCP/CLI | https://news.ycombinator.com/item?id <tg-id> | ~4 months | 4 | не нашёл | не сработало |
| 10 | Mind-mem 19 MCP tools | https://news.ycombinator.com/item?id <tg-id> | — | очки в scrape не отдал | не нашёл | не знаю итог |
| 11 | Burnish (King) | https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/ | 2026 | 1 | ~0 | не сработало |
| 12–20 | AMP / In Memoria / Agent Recall dup / Cortex / Friday / DecisionNode / ContextVault и др. из прошлого захода | HN items в сводке сессии | 2025–2026 | 5, 2, 1, 3, 9, 23, … | не нашёл | паттерн: 1–13 pts норма; 23 = редкий хвост; 71 = единственный явный front-page в выборке memory |

Медиана звёзд с поста по очкам (индустрия, не наша выборка 20):

| корзина очков | медиана 48h stars | источник | вывод |
|---|---|---|---|
| <10 (сюда падает медиана всех Show HN = 2) | не нашёл опубликованной медианы; King не считал low-score repos | King bias: star API только для score≥258 | не знаю; практически ~0–несколько |
| 10–50 | не нашёл | — | не знаю |
| 50–200 | не нашёл прямой медианы; King 1.4★/pt на хитах | экстраполяция 50×1.4≈70 только если вы в выборке хитов | не знаю для unknown authors |
| 200+ | King: 258–350 → 1.77★/pt; 700+ → 0.79★/pt | https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/ | sublinear |
| dotenvx #1 | 350 pts → 800★ / 48h | https://dotenvx.com/blog/2024/06/30/what-does-number-one-on-hacker-news-get-you.html 2024-06-30 | 13% star rate от GitHub visitors; 17% CTR site→GitHub; 100 visitors/pt на сайт |

Что объединяет попавших на 1-ю страницу (из King + тред 71 pts):

- заголовок конкретный: механизм (OKF, FTS5, SQLite), не «best memory ever»
- слот: Sunday evening / Monday 00:00 UTC
- README готов ДО поста (92% эффекта в 48h, половина в 24h)
- демо без регистрации (GitHub clone / uvx)
- автор отвечает в первые 2 часа (OKF-тред: pcbmaker20 ответил на «why not md»)
- first comment автора: проблема → как работает → лимиты честно (Remembrane, Agent Recall)

Что убивало:

- «My Claude told me to post» (Hmem)
- бенчмарк vs mem0 без воспроизведения (HN скепсис в OKF-треде: «another grep»)
- нет работающего clone за 30 секунд
- регистрация / waitlist
- пустой first comment
- Show HN как единственный GTM при 200 постах/день

Для sgm без аудитории базовый исход Show HN = 1–13 pts, не 1000 звёзд.

---

D. Конверсия посетитель → звезда

Полной опубликованной таблицы «просмотр README → звезда» по источникам (HN / Reddit / чужие доки / GitHub search / awesome / X / YouTube) не нашёл. Ниже фрагменты, которые открыл.

| источник | URL | дата | число | что меряет | вывод |
|---|---|---|---|---|---|
| HN хиты (King) | https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/ | 2026-04-23 | 1.4★ на 1 HN point / 48h; r=0.29 | не view→star; point→star на score≥258 | нельзя переносить на пост с 2 pts |
| HN #1 dotenvx | https://dotenvx.com/blog/2024/06/30/what-does-number-one-on-hacker-news-get-you.html | 2024-06-30 | 35k site visitors / 350 pts = 100 vis/pt; 6k GitHub visitors = 17% CTR; 800★ = 13% от GH visitors | site view → GH view → star | 13% star rate на хите #1, не на README вообще |
| HN AI tools среднее | https://arxiv.org/abs/2511.04453 | 2025-11-06 | 121 / 24h, 189 / 48h, 289 / week | stars after HN exposure, 138 repos | среднее по уже взорвавшимся AI launches |
| GitHub Insights | продукт GitHub: 14-day views/uniques | — | официального % view→star нет | sgm: 34 unique / 14d, 12★ lifetime | оконную конверсию не посчитать |
| Reddit | — | — | не нашёл | — | не нашёл |
| чужие доки | — | — | не нашёл % | механизм есть (install line), цифры конверсии нет | не нашёл |
| поиск GitHub | — | — | не нашёл | — | не нашёл |
| awesome-списки | — | — | не нашёл view→star | punkpeye 96.7k★ списка ≠ звёзды каждого сервера | не нашёл |
| Twitter/X | — | — | не нашёл | — | не нашёл |
| YouTube | — | — | не нашёл | — | не нашёл |

Грубая сборка, не таблица индустрии: на хите HN GitHub-visitor→star ~10–20% (dotenvx 13%). README view→star для холодного OSS без фронта обычно ниже; опубликованного знаменателя нет.

Для sgm сейчас: 34 unique / 14 дней и 12★ за жизнь. Это не конверсия окна, это масштаб спроса.

---

Топ-10 мест для строки установки sgm

Сортировка: вероятность принять × охват. Первый шаг — конкретная дверь.

| # | место | P(принять)×охват | первый шаг |
|---|---|---|---|
| 1 | Official MCP Registry (уже стоит) → Glama claim + Smithery | P высокая (уже published 19.10.2026) × охват агрегаторов (108k Glama, Pulse ingest) | проверить server.json; claim на https://glama.ai/mcp/servers ; `smithery mcp publish` |
| 2 | punkpeye/awesome-mcp-servers · Knowledge & Memory | P средняя-высокая (PR+🤖🤖🤖, alpha, живой merge) × 96.7k★ списка | один PR, одна строка, alphabetical, public repo, не remote-only |
| 3 | TeleAI Awesome-Agent-Memory · Emerging | P высокая при <100★ и factual ≤25 слов × 691★ нишевой аудитории памяти | PR в Emerging; без PyPI-ссылки; без суперлативов |
| 4 | mnemoverse/awesome-agent-memory · MCP memory servers | P высокая (уже держат Basic Memory, Graphmem, Selvedge) × малый список, точная аудитория | один PR, alphabetically, factual; рядом с Basic Memory |
| 5 | Cline mcp-marketplace issue | P средняя (few stars OK, но vetting) × one-click Cline | issue по шаблону + 400×400 PNG + `llms-install.md` как у basic-memory |
| 6 | CrewAI StorageBackend адаптер (не список) | P высокая если код проходит protocol × docs.crewai.com читают билдеры | пакет `sgm[crewai]` по образцу m3-memory; гайд 15 строк; не PR в core CrewAI |
| 7 | Gemini CLI gallery через topic `gemini-cli-extension` | P средняя (crawl, не ручной куратор) × CLI Google | extension + topic; ждать daily crawl |
| 8 | Goose: пример user-add в доке / extensions | P низкая-средняя (нет открытого каталога) × агент-рантайм | issue/PR с готовым snippet, если в доке есть «example memory servers» |
| 9 | Cursor MCP directory issue | P низкая (ручной request) × Cursor users | issue «request listing»; marketplace plugin — отдельная ручная ревью |
| 10 | awesome-claude-code issue form | P низкая сейчас (12★, нет CC-native plugin, порог 14д+activity или 100★) × большой список CC | сначала plugin/skill для Claude Code, потом форма. Не сейчас |

Не делать: Obsidian Copilot/Smart Connections (нет слота); LangChain/LlamaIndex core lists; PulseMCP форма (paused); веер PR; Product Hunt; купленные звёзды; бенчмарк-посты vs mem0/Zep.

Путь к 1000★: чужая дистрибуция копит install, не звёзды. 1000★ у близнецов (basic-memory ~4k, mcp-memory-service ~2k) заняли месяцы продукта + несколько дверей, не один Show HN. claude-mem (~102k) — другой вид: native plugin харнесса.

---

Проверено: каталоги TeleAI, mnemoverse, punkpeye CONTRIBUTING, Cline marketplace, Official Registry about, PulseMCP pause, Glama 108 273, Coworker 9 servers + stars 2026-10-06, pypistats basic-memory, CrewAI StorageBackend, Pydantic AI Memory backends, King 188k Show HN, dotenvx conversion, arXiv HN stars, 10+ живых Show HN memory, claude-mem star snapshots Oct 2026.

Не проверено: 14-дневные дельты star-history/pypistats without-mirrors после конкретных PR; точные ★ punkpeye-серверов на дату merge; view→star Reddit/X/YouTube/awesome; живой jonno.nz (404); PulseMCP weekly visitors для sgm; открытие registry API на предмет записи sgm в этом прогоне (заказ утверждает 19.10.2026).

---

🧒 Простыми словами

Представь полку в чужом магазине, а не свой ларёк у дороги. sgm уже лежит в «официальном складе» MCP. Дальше нужно попасть на две полки, которые люди реально листают: огромный список MCP-серверов и короткий список «память для агентов». Show HN для незнакомца почти всегда проходит мимо кассы. Тысяча звёзд приходит, когда чужие инструкции пишут `pip install sqlite-graph-memory`, а не когда ты сам кричишь в пустоту.

🏛A · Grok 4.6 · 59839
