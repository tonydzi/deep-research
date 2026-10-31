# DR26-10-22-MACANTON-02-1416 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# sqlite-graph-memory: первые встраивания и путь к 1000★

**Дата исследования:** 22–23 октября 2026. Все числа зафиксированы на 23.10.2026 (GitHub Search API, HN Algolia API, pypistats.org API, npm registry API) или датированы источником. Всё, что не удалось найти, помечено «не нашёл».

**Вопрос.** Как пакету `sqlite-graph-memory` (PyPI 0.3.1, MIT, 12★, ~5–9 загрузок PyPI/день без зеркал, в официальном MCP Registry с 19.10.2026) получить первые строки установки в чужой документации и дойти до 1000 звёзд GitHub. Четыре подвопроса: (а) куда встроиться, (б) как первые встраивания получили малые memory-проекты, (в) что реально даёт Show HN автору без аудитории, (г) опубликованные конверсии «посетитель → звезда».

---

## Резюме (ранжированные выводы)

1. **Единственный механизм, который на наших данных дал малым memory-проектам взрывной рост, — это чужая платформа со встроенной дистрибуцией, а не чужие README-списки.** [claude-mem](https://github.com/thedotmack/claude-mem) вырос до 102 440★ за ~13 месяцев (создан 31.08.2025), потому что оформился как нативный плагин Claude Code (npm, 68 155 загрузок/мес) и жил в r/ClaudeCode; 61 день авг–окт 2026 дал ему +10 831 звезду ([trendshift.io/repositories/15496](https://trendshift.io/repositories/15496), 23.10.2026). Его добавление в [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) 25.10.2025 на npm-загрузки не повлияло (5–74 загрузки/день в соседние дни — см. раздел б).
2. **Show HN — слабый канал, медиана поста 2 очка** (анализ 188 085 постов, [danfking](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/), 23.04.2026). Все три «своих» Show HN у будущих звёздных memory-проектов провалились: basic-memory 4 очка (15.03.2025), cognee 9/6/7 очков (2024–2025), memU 11 очков (08.08.2025) — а проекты всё равно выросли до 4,3k/32,7k/14,6k★ другими каналами. 87% Show HN про агентную память набирают <10 очков (наш подсчёт по HN Algolia, 22.10.2026).
3. **Один HN-очко ≈ 1,4 звезды за 48 часов** (danfking, n=491 репо). Фронт-пейдж (258+ очков) даёт медиану 509 звёзд в первый день, но 92% эффекта заканчивается за 48 часов. Show HN — импульс, а не рост.
4. **Awesome-списки принимают почти всех и дают почти ноль прямых звёзд.** [rag-kb](https://github.com/fish827-08/rag-kb) был принят в punkpeye/awesome-mcp-servers в день создания (27.08.2026, PR [#13117](https://github.com/punkpeye/awesome-mcp-servers/pull/13117)) — и через 2 месяца имеет 4★. Ценность списков — SEO, перманентность и сигнал для других ворот (Cline marketplace открыто оценивает «community adoption»).
5. **Куратор внутри клиента ценнее списка снаружи.** Cline Marketplace — заявки issue-шаблоном, «звёзды не обязательны» (FAQ репозитория), но у них очередь: memory-заявки висят открытыми с февраля 2026. Anthropic держит три официальных маркетплейса плагинов Claude Code, включая community ([доки](https://code.claude.com/docs/en/plugins/anthropic-marketplaces)) — это самый большой неиспользованный канал именно для sgm.
6. **Конверсия посетитель→звезда для dev-tools: ~3–8%.** Reddit-запуски: 5–8% ([AFFiNE playbook](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/), 25.03.2026, n=12); документация basic-memory: 29 400 просмотров доков → 1 004★ ≈ 3,4% ([блог Basic Machines](https://basicmachines.co/blog/100-github-stars/), 30.06.2025). Репозиторий с 0★ конвертит ~5% посетителей, порог «правдоподобия» — 100–500★ (AFFiNE).
7. **Для sgm вывод:** к 1000★ ведёт не один Show HN (даст 50–300), а цепочка «упаковать как плагин/skill Claude Code → официальные маркетплейсы Anthropic + Cline + cursor.directory → списки как SEO-гигиена → Obsidian-Copilot как уникальный нишевый ход» плюс постоянный контент (у всех выросших проектов был недельный релизный ритм).

---

## Методология

- **Источники:** GitHub Search API (звёзды, даты создания, поиск коммитов — даты добавления в списки через коммит-сообщения), HN Algolia API (очки/даты/ID постов, распределение по очкам через `numericFilters`), pypistats.org API (загрузки без зеркал, ежедневные ряды за 180 дней), npm registry API (загрузки по дням), полные тексты ключевых страниц (CONTRIBUTING, маркетплейсы, блоги).
- **Причинность «встраивание → рост»** проверялась тремя способами: дата добавления в список против последующих загрузок (pypistats/npm), даты Show HN против текущих звёзд, коммиты «Add X» против звёзд X на сегодня.
- **Ограничения:** ретроспективный прирост звёзд за 48 часов для отдельных постов восстановить нельзя (GitHub не отдаёт историю звёзд через доступные API) — используем агрегаты danfking и текущие звёзды; это помечено. Wayback-снимки GitHub-страниц не парсятся. Даты некоторых Reddit-постов (claude-mem 45k★) не найдены.

---

## (а) Куда встроиться: кто держит списки memory-бэкендов и как туда попасть

### A.1 MCP-списки и реестры

| Место | URL | Как принимают | Правила / критерии отказа | Возраст, трафик |
|---|---|---|---|---|
| punkpeye/awesome-mcp-servers | [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) | PR в README ([CONTRIBUTING](https://github.com/punkpeye/awesome-mcp-servers/blob/main/CONTRIBUTING.md)); есть секция «Knowledge & Memory»; агентам — fast-track при «🤖🤖🤖» в заголовке PR | Только серверы с публичным GitHub-репо, которые устанавливают и запускают локально (remote-only → их другой список); следовать формату, алфавитный порядок | **96 785★**, создан 30.11.2024, ежедневно мержат |
| Cline MCP Marketplace | [github.com/cline/mcp-marketplace](https://github.com/cline/mcp-marketplace) | [Issue по шаблону](https://github.com/cline/mcp-marketplace/issues/new?template=mcp-server-submission.yml): ссылка на репо, лого 400×400 PNG, причина добавления; подтверждение, что Cline ставит сервер по одному README/llms-install.md | Ревью: community adoption, developer credibility, project maturity, security (README). В FAQ: «можно без звёзд — Absolutely!»; ревью «пару дней» — на практике очередь (см. A.3) | Репо 795★ (создан 15.02.2025); хост-клиент [cline/cline](https://github.com/cline/cline) — **70 770★**; листинг виден в приложении, one-click install |
| MCP Registry (официальный) | [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io) / [github.com/modelcontextprotocol/registry](https://github.com/modelcontextprotocol/registry) | `server.json` + PR/CLI-паблиш ([quickstart](https://modelcontextprotocol.io/registry/quickstart)); запущен в превью 08.09.2025 ([блог](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/)) | Требуется корректный server.json, маппинг mcpName ↔ пакет | 7 398★; **sgm уже там с 19.10.2026** (подтверждено бейджем в README репо) |
| appcypher/awesome-mcp-servers | [github.com/appcypher/awesome-mcp-servers](https://github.com/appcypher/awesome-mcp-servers) | PR | Кураторский отбор | **5 798★**, создан 28.11.2024 |
| wong2/awesome-mcp-servers | [github.com/wong2/awesome-mcp-servers](https://github.com/wong2/awesome-mcp-servers) | **PR не принимают**: «Submit your MCP on the website: [mcpservers.org/submit](https://mcpservers.org/submit)» (README) | — | 4 379★ (указание в README) |
| Zed extensions | [zed.dev/docs/extensions/mcp-extensions](https://zed.dev/docs/extensions/mcp-extensions) | MCP-расширения депрекейтят в пользу официального MCP Registry («To keep your MCP server available in Zed, publish it to the official registry as well»); трекинг — [issue #59353](https://github.com/zed-industries/zed/issues/59353) | — | покрыто для sgm через Registry |
| Официальные reference-серверы MCP | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) | Только официальные серверы (там живёт официальный memory-сервер) | внешние серверы не принимают | 91 512★ — это «потолок», не цель |
| Массовые каталоги: Smithery ([smithery.ai/servers/new](https://smithery.ai/servers/new)), Glama ([glama.ai](https://glama.ai), 108 273 сервера, автоматически вбирает официальный Registry), mcp.so | сабмит формами ([обзор](https://gingerlabs.ai/blog/find-evaluate-mcp-servers-smithery-glama-mcp-so)) | формы | — | **по вашему ТЗ — не повторять** |

### A.2 Claude Code / Cursor / Codex / Gemini CLI

| Место | URL | Как принимают | Правила | Возраст, трафик |
|---|---|---|---|---|
| Маркетплейсы Anthropic (официальный / community / demo) | [code.claude.com/docs/en/plugins/anthropic-marketplaces](https://code.claude.com/docs/en/plugins/anthropic-marketplaces) | Каталог = GitHub-репо с `.claude-plugin/marketplace.json`: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official), [anthropics/claude-plugins-community](https://github.com/anthropics/claude-plugins-community) (имя `claude-community`), demo | Нужно оформить sgm как плагин Claude Code | Точной цифры звёзд маркетплейсов — не нашёл; дистрибуция идёт прямо в Claude Code/claude.ai |
| Директория плагинов Anthropic (claude.ai) | [code.claude.com/docs/en/plugins/publish](https://code.claude.com/docs/en/plugins/publish) | «Submit to Anthropic's directory» из claude.ai (нужен платный план); после публикации плагин синхронится в сессии Claude Code | пререлиз-чеклист, plugin.json, версионирование | авт. обновления после паблиша |
| awesome-claude-code | [github.com/hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | PR «Add resource» (категория/подкатегория); мержат ежедневно | hand-picked, «инструменты, которые не работают, вырезают» | **56 310★**, создан 19.04.2025 |
| awesome-claude-skills | [github.com/travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | PR ([CONTRIBUTING](https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md)) | кураторский | **15 517★**, создан 16.10.2025 — молодой и быстрорастущий |
| awesome-claude-code-toolkit | [github.com/rohitg00/awesome-claude-code-toolkit](https://github.com/rohitg00/awesome-claude-code-toolkit) | PR | «176+ plugins» в списке | 2 727★, создан 04.02.2026 |
| Cursor Directory | [cursor.directory/plugins/new](https://cursor.directory/plugins/new) | форма сабмита («Submit a Plugin … reach 300k+ developers»); официальная витрина — [cursor.directory/plugins](https://cursor.directory/plugins) («Rules, MCP servers, and integrations built by the community»); официальный маркетплейс — cursor.com/marketplace/publish (см. [форум](https://forum.cursor.com/t/how-do-i-upload-my-plugin-after-filling-out-the-form-there-are-no-buttons-am-i-doing-something-wrong/155138)) | плагин должен ставиться и работать | старый [cursor/mcp-servers](https://github.com/cursor/mcp-servers) закрыт: «deprecated. Use cursor.directory instead» |
| Gemini CLI | — | — | — | публичного каталога сторонних memory-бэкендов **не нашёл** |

### A.3 Примеры недавних добавлений чужих малых пакетов (все даты — даты коммитов/PR/issue в целевых репо)

| Имя | URL | Дата | Число | Вывод |
|---|---|---|---|---|
| mcp-memory-service (doobidoo) в punkpeye/awesome-mcp-servers | [коммит от 22.08.2025](https://github.com/punkpeye/awesome-mcp-servers/commit/c9f0914) | 22.08.2025 | сейчас 2 036★; PyPI 8 649/мес | сработало (PR открыл сам автор) |
| basic-memory в punkpeye/awesome-mcp-servers | [PR #7817](https://github.com/punkpeye/awesome-mcp-servers/pull/7817), коммит 10.06.2026 | 10.06.2026 | добавлен уже при ~4k★ | сработало, но ПОСЛЕ роста (список не источник роста) |
| rag-kb (fish827-08) в punkpeye/awesome-mcp-servers | [github.com/fish827-08/rag-kb](https://github.com/fish827-08/rag-kb) + [PR #13117](https://github.com/punkpeye/awesome-mcp-servers/pull/13117) | 27.08.2026 (репо создан в тот же день) | 4★ через 2 месяца | **не сработало** (прямых звёзд список не дал) |
| Myco (Battam1111) в punkpeye/awesome-mcp-servers | [github.com/Battam1111/Myco](https://github.com/Battam1111/Myco) (коммит 22.04.2026) | 22.04.2026 | 65★ через 6 месяцев | не сработало |
| memviz в punkpeye/awesome-mcp-servers | коммит 16.02.2026 | 16.02.2026 | звёзды — не нашёл | не знаю |
| claude-mem в awesome-claude-code | [PR #262](https://github.com/hesreallyhim/awesome-claude-code/pull/262), коммит [b6e44c1](https://github.com/hesreallyhim/awesome-claude-code/commit/b6e44c1) от 25.10.2025 | 25.10.2025 | npm в соседние дни 5–74 загрузок/день | **не сработало** для дистрибуции (рост пришёл в 2026 из Claude Code-среды и r/ClaudeCode) |
| funes / PLUR / faf-cli / claude-context-optimizer в awesome-claude-code | коммиты [#3179](https://github.com/hesreallyhim/awesome-claude-code) 15.10.2026, [#3152](https://github.com/hesreallyhim/awesome-claude-code) 13.10.2026, [#2703](https://github.com/hesreallyhim/awesome-claude-code) 02.09.2026, [#2435](https://github.com/hesreallyhim/awesome-claude-code) 05.08.2026 | 2026 | звёзды funes/PLUR — не нашёл | не знаю |
| Memory-Plus в wong2/awesome-mcp-servers | коммит 23.05.2025 | 23.05.2025 | звёзды — не нашёл | не знаю |
| Jean Memory в wong2/awesome-mcp-servers | коммит 04.06.2025 | 04.06.2025 | звёзды — не нашёл | не знаю |
| Memory-заявки в Cline Marketplace (m3 Memory [#2074](https://github.com/cline/mcp-marketplace/issues/2074), Mnemoverse [#2590](https://github.com/cline/mcp-marketplace/issues/2590), Memory Nexus [#1033](https://github.com/cline/mcp-marketplace/issues/1033) от 24.03.2026, mcp-memory decay [#1050](https://github.com/cline/mcp-marketplace/issues/1050) от 25.03.2026, MCP Memory Bank [#852](https://github.com/cline/mcp-marketplace/issues/852) от 11.03.2026, hypermarrow [#2644](https://github.com/cline/mcp-marketplace/issues/2644) от 26.09.2026) | issue-сабмиты | февраль–октябрь 2026 | заявки **висят открытыми месяцами**; приток ~12 issue/день (окт 2026: #3055–3199 за 6 дней) | приём медленный — не знаю исхода |
| smart-connections-mcp (оседлал Smart Connections) | [github.com/msdanyg/smart-connections-mcp](https://github.com/msdanyg/smart-connections-mcp) | создан 13.10.2025 | 59★ | частично сработало (ниша, но медленно) |

### A.4 Фреймворки агентных приложений

| Фреймворк | URL страницы memory | Как принимают внешние пакеты | Вывод для sgm |
|---|---|---|---|
| LangChain / LangGraph | [docs.langchain.com/oss/python/langgraph/persistence](https://docs.langchain.com/oss/python/langgraph/persistence) | Список чекпойнтеров — только официальные (sqlite/postgres); публичного списка сторонних memory-бэкендов **нет** | списка нет; путь — свой интеграционный гайд + PR-примеры в cookbook |
| LlamaIndex | [developers.llamaindex.ai/python/framework/module_guides/deploying/agents/memory/](https://developers.llamaindex.ai/python/framework/module_guides/deploying/agents/memory/) | [Страница community-интеграций](https://developers.llamaindex.ai/python/framework/community/integrations/) категоризует loaders/tools/LLMs/vector stores — **категории memory-бэкендов нет** | списка нет |
| Agno | [docs.agno.com/memory/working-with-memories/sqlite-memory](https://docs.agno.com/memory/working-with-memories/sqlite-memory), [кастом-провайдеры](https://docs.agno.com/context-providers/custom-providers) | Сторонние бэкенды — через subclass (код), не список | написать свой гайд «sgm как Agno memory» |
| Pydantic AI | [pydantic.dev/docs/ai/harness/memory/](https://pydantic.dev/docs/ai/harness/memory/) + [issue #530](https://github.com/pydantic/pydantic-ai/issues/530) (запрос внешних memory-сторов, там упоминаются mem0/Zep) | Списка нет; сторонние пакеты пишут интеграции у себя (паттерн [hindsight-pydantic-ai](https://hindsight.vectorize.io/sdks/integrations/pydantic-ai)) | повторить паттерн hindsight: свой домен `sgm + pydantic-ai` |
| CrewAI | интеграция mem0 документирована **у mem0**: [docs.mem0.ai/integrations/crewai](https://docs.mem0.ai/integrations/crewai) (и [langchain](https://docs.mem0.ai/integrations/langchain)) | в чужие доки CrewAI сторонние memory **не встраиваются** | писать гайд у себя, просить ссылку в issue |
| Copilot for Obsidian (клиент внутри Obsidian; поддержка MCP-инструментов появилась в октябре 2026 — [коммиты 19.10.2026](https://github.com/logancyang/obsidian-copilot)) | [github.com/logancyang/obsidian-copilot](https://github.com/logancyang/obsidian-copilot), 7 880★ | docs ([obsidiancopilot.com](https://www.obsidiancopilot.com/en)) + issue/Discussion | **лучший нишевый ход для sgm**: уникальный фит (Obsidian-вики-ссылки) |
| Smart Connections / Text Generator | [brianpetro/obsidian-smart-connections](https://github.com/brianpetro/obsidian-smart-connections), [nhaouari/obsidian-textgenerator-plugin](https://github.com/nhaouari/obsidian-textgenerator-plugin) | списков сторонних memory-бэкендов **не нашёл** | не знаю |
| LibreChat (46 187★) | [librechat.ai/docs/mcp_servers](https://www.librechat.ai/docs/mcp_servers) — «MCP Server Guides»: пошаговые гайды по конкретным серверам | PR в доки проекта | сделать гайд «sgm в LibreChat» и предложить PR |
| Open WebUI (156 548★) | [docs.openwebui.com/features/extensibility/community/](https://docs.openwebui.com/features/extensibility/community/) («Listings are community-submitted»), MCP: [docs](https://docs.openwebui.com/features/extensibility/mcp/) (нативно с v0.6.31, прокси [mcpo](https://github.com/open-webui/mcpo)) | публикация Tool/Function на [openwebui.com](https://openwebui.com/o/ModelContextProtocol) | обернуть sgm как Tool |
| Continue | [docs.continue.dev/customize/deep-dives/mcp](https://docs.continue.dev/customize/deep-dives/mcp) | только конфиг пользователя, каталога **нет** | не подходит |
| Goose | [allowlist](https://goose-docs.ai/docs/guides/allowlist/) | нет публичного сабмита сторонних MCP-серверов | не подходит |

### A.5 Списки «agent memory» и PKM+AI

| Список | URL | Возраст/трафик | Примеры недавних добавлений |
|---|---|---|---|
| TeleAI-UAGI/Awesome-Agent-Memory | [github.com/TeleAI-UAGI/Awesome-Agent-Memory](https://github.com/TeleAI-UAGI/Awesome-Agent-Memory) | 693★, создан 30.10.2025; секции «systems, benchmarks, papers» | конкретные даты добавления чужих малых пакетов — не нашёл |
| AgentMemoryWorld/Awesome-Agent-Memory | [github.com/AgentMemoryWorld/Awesome-Agent-Memory](https://github.com/AgentMemoryWorld/Awesome-Agent-Memory) | 245★, создан 13.01.2026, [TMLR 2026] | не нашёл |
| tfatykhov/awesome-agent-memory | [github.com/tfatykhov/awesome-agent-memory](https://github.com/tfatykhov/awesome-agent-memory) | 24★, создан 28.03.2026 | не нашёл |
| mnemoverse/awesome-agent-memory | [github.com/mnemoverse/awesome-agent-memory](https://github.com/mnemoverse/awesome-agent-memory) | 57★, создан 18.07.2026 | не нашёл |
| kmaasrud/awesome-obsidian | [github.com/kmaasrud/awesome-obsidian](https://github.com/kmaasrud/awesome-obsidian) | 9 481★, с 12.05.2020 (плагины/темы/гайды) | sgm — не плагин, попадание под вопросом |
| danielrosehill/Awesome-Obsidian-AI-Tools | [github.com/danielrosehill/Awesome-Obsidian-AI-Tools](https://github.com/danielrosehill/Awesome-Obsidian-AI-Tools) | 276★, создан 30.09.2024, PR | фокус «плагины», но про AI+Obsidian |
| noghartt/awesome-obsidian-plugins | [github.com/noghartt/awesome-obsidian-plugins](https://github.com/noghartt/awesome-obsidian-plugins) | 91★ | — |
| PKM-гайды (вручную обновляемые списки, контакт с автором): [dsebastien.net «Must-Have Obsidian Plugins 2026» (75+)](https://www.dsebastien.net/the-must-have-obsidian-plugins-for-2026/), [shadow.do «Best AI plugins for Obsidian 2026»](https://www.shadow.do/blog/best-ai-plugins-for-obsidian-2026), [localaimaster.com (Ollama+Obsidian)](https://localaimaster.com/blog/local-ai-obsidian-integration) | | | |

---

## (б) Как малые проекты агентной памяти получили первое встраивание

### Б.1 Сводная таблица

| Имя | URL | Дата (событие) | Число | Вывод |
|---|---|---|---|---|
| **MemGPT → letta** | [github.com/letta-ai/letta](https://github.com/letta-ai/letta) | [HN-пост 16.10.2023](https://news.ycombinator.com/item?id <tg-id>) (363 очка) + [статья arXiv 2310.08560, 14.10.2023](https://arxiv.org/abs/2310.08560) (свой HN-пост [225 очков](https://news.ycombinator.com/item?id <tg-id>)) | сейчас 25 237★; PyPI `letta` 15 031/мес, `letta-client` 161 915/мес ([pypistats](https://pypistats.org/packages/letta)) | сработало (научная статья → HN-фронтпейдж); неповторимо без академического бэкграунда |
| **mem0** | [github.com/mem0ai/mem0](https://github.com/mem0ai/mem0) | [Show HN 04.09.2024](https://news.ycombinator.com/item?id <tg-id>) | 201 очко; сейчас 67 692★; PyPI `mem0ai` 2 020 516/мес ([pypistats](https://pypistats.org/packages/mem0ai)) | сработало (Show HN + X + VC-дистрибуция: YC-бэкед, запущен в янв. 2024, $24M — [TechCrunch 28.10.2025](https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps/)) |
| **graphiti (Zep)** | [github.com/getzep/graphiti](https://github.com/getzep/graphiti) | [Show HN 04.09.2024](https://news.ycombinator.com/item?id <tg-id>); PH-запуск 19.02.2025 ([блог Zep](https://blog.getzep.com/blog/tag/graphiti/)); Zep переключила опенсорс-стратегию на graphiti ([блог](https://blog.getzep.com/announcing-a-new-direction-for-zeps-open-source-strategy/)) | 142 очка; сейчас 32 162★; PyPI `graphiti-core` 619 128/мес ([pypistats](https://pypistats.org/packages/graphiti-core)) | сработало (компания + PH + бенчмарк-позиционирование против «vector-only») |
| **cognee** | [github.com/topoteretes/cognee](https://github.com/topoteretes/cognee) | Show HN: [09.06.2025 — 9 очков](https://news.ycombinator.com/item?id <tg-id>), 13.02.2025 — 6, 22.04.2024 — 7 | сейчас 32 694★; PyPI `cognee` 86 048/мес ([pypistats](https://pypistats.org/packages/cognee)) | **Show HN не сработал**; сработали $7.5M seed ([анонс cognee](https://www.cognee.ai/blog/cognee-news/cognee-raises-seven-million-five-hundred-thousand-dollars-seed), [pulse2](https://pulse2.com/cognee-7-5-million-seed-funding-raised-for-building-enterprise-grade-memory-layer-for-ai-agents/); первый раунд 18.11.2024, всего $9.09M — [tracxn](https://tracxn.com/d/companies/cognee/__XkSxgBzH5vOB59VwVuEQNwoDJxGMlvIPIq8cIV5WsIY)) — т.е. платный контент-маркетинг |
| **mcp-memory-service** | [github.com/doobidoo/mcp-memory-service](https://github.com/doobidoo/mcp-memory-service) | self-PR в punkpeye/awesome-mcp-servers — [коммит 22.08.2025](https://github.com/punkpeye/awesome-mcp-servers/commit/c9f0914) | сейчас 2 036★; PyPI 8 649/мес, ~337/день ([pypistats](https://pypistats.org/packages/mcp-memory-service)) | сработало медленно (2 года, соло-мейнтейнер, без VC) — ближайший аналог траектории sgm |
| **basic-memory** | [github.com/basicmachines-co/basic-memory](https://github.com/basicmachines-co/basic-memory) | [Show HN 15.03.2025 — 4 очка](https://news.ycombinator.com/item?id <tg-id>) (провал); 1 004★ к 30.06.2025 ([блог](https://basicmachines.co/blog/100-github-stars/): 6 месяцев, 29 400 просмотров доков, 9 контрибьюторов); в punkpeye — только [10.06.2026](https://github.com/punkpeye/awesome-mcp-servers/pull/7817) | сейчас 4 327★; PyPI ~700–4 600/день, медиана ~1 500 ([pypistats](https://pypistats.org/packages/basic-memory)) | **HN не сработал; списки пришли позже**; сработали собственный контент + [r/ObsidianMD-пост](https://www.reddit.com/r/ObsidianMD/comments/1l9rdnb/basic_memory_obsidian_ai_that_reads_writes_and/) + свой сабреддит [r/basicmemory](https://www.reddit.com/r/basicmemory/) + Discord. Важно: рост загрузок начался 17–27.05.2026 (1 000→4 581/день) **до** добавления в список |
| **memU (NevaMind)** | [github.com/NevaMind-AI/memU](https://github.com/NevaMind-AI/memU) | [Show HN 08.08.2025 — 11 очков](https://news.ycombinator.com/item?id <tg-id>) (провал); [ещё пост 06.01.2026](https://news.ycombinator.com/item?id <tg-id>) | сейчас 14 557★; PyPI-имя пакета — не нашёл; рост затух: **+238★ за 61 день** авг–окт 2026 ([trendshift](https://trendshift.io/repositories/17374)) | **HN не сработал**; выросли вне HN (маркетинг NevaMind; конкретный первый канал — не нашёл) |
| **OpenMemory (mem0)** | [блог-анонс 13.05.2025](https://mem0.ai/blog/introducing-openmemory-mcp) (обновлен 25.08.2026); новый CLI-репо [github.com/mem0ai/openmemory](https://github.com/mem0ai/openmemory) создан 06.07.2026 | 99★ у нового репо | сработало: ехал на дистрибуции mem0 (67 692★) — встраивание в чужие доки было не нужно |
| **claude-mem** | [github.com/thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | создан 31.08.2025; первые npm-загрузки 29.09.2025 ([npm registry](https://api.npmjs.org/downloads/range/2025-08-28:2025-09-30/claude-mem)); [в awesome-claude-code 25.10.2025](https://github.com/hesreallyhim/awesome-claude-code/commit/b6e44c1); момент рынка: запуск Claude Memory у Anthropic [23.10.2025](https://news.ycombinator.com/item?id <tg-id>) (559 очков) | сейчас **102 440★**, 9 500 форков, 124 контрибьютора; npm 68 155/мес ([npm](https://api.npmjs.org/downloads/point/last-month/claude-mem), 19.09–18.10.2026); **+10 831★ за 61 день** авг–окт 2026 ([trendshift](https://trendshift.io/repositories/15496)); в окт–ноябре 2025 npm был 0–307 загрузок/день ([данные](https://api.npmjs.org/downloads/range/2025-10-08:2025-11-12/claude-mem)) | **сработало сильнее всех** — канал: нативная платформа Claude Code (плагин), r/ClaudeCode ([«45,000 stars», 175 голосов](https://www.reddit.com/r/ClaudeCode/comments/1scz5kk/claudemem_hit_45000_stars_on_github_today_and_it/); дата поста — не нашёл; [46.1K](https://www.augmentcode.com/learn/claude-mem-46k-stars-persistent-memory-claude-code)), 25 локализаций README, Vercel OSS/Greptile/SerpApi спонсорство. Никаких списков в основе |
| **Cline Memory Bank (методология)** | [docs.cline.bot/best-practices/memory-bank](https://docs.cline.bot/best-practices/memory-bank) + [блог Cline](https://cline.bot/blog/memory-bank-how-to-make-cline-an-ai-agent-that-never-forgets) | методология (6 markdown-файлов) встроена в **официальные доки Cline** | порт-репозитории малы: [dazeb/cline-mcp-memory-bank](https://github.com/dazeb/cline-mcp-memory-bank) 61★ (создан 13.03.2025), [sharath29/cline-memory-bank](https://github.com/sharath29/cline-memory-bank) 3★ | сработало как термин и стандарт (десятки портов); автор оригинального промпта — не нашёл; встроили сами Cline, потому что методология стала популярна в их сообществе — «звонок» сделал хост |

### Б.2 Сколько звёзд дало каждое встраивание (где измеримо)

- **Список punkpeye/awesome-mcp-servers**: для rag-kb — **≈0–4★ за 2 месяца** (репо [4★](https://github.com/fish827-08/rag-kb) на 23.10.2026 при добавлении 27.08.2026) — **не сработало** как прямой канал. Для basic-memory — не измеримо, т.к. добавили через год после роста (**не сработало** как источник, полезно как SEO).
- **awesome-claude-code для claude-mem**: npm-загрузки в окно 25–31.10.2025 — 74/13/5/11/3/3/1 в день, без всплеска (**не сработало** для дистрибуции; growth пришёл в 2026 из Claude Code-плагин-среды и Reddit).
- **Первое «встраивание» в жизни letta/graphiti/mem0** — это была не чужая дока, а **чужая витрина с трафиком**: HN-фронтпейдж (363/142/201 очков соответственно). Прирост звёзд за 2 недели постфактум не восстановим — **не нашёл**; по коэффициенту danfking (1.4★/очко) mem0-пост давал порядка ~280★, graphiti ~200★ за 48 часов (оценка, не измерение).
- **Паттерн «чужие доки» реален, но выглядит иначе**: mem0/cognee/graphiti пишут интеграционные доки **на своём сайте** под каждый фреймворк ([docs.mem0.ai/integrations/crewai](https://docs.mem0.ai/integrations/crewai), [langchain](https://docs.mem0.ai/integrations/langchain)); в чужие доки их зовут через issue/PR, а не через список. Пример работающего стороннего пакета на этом пути: [hindsight](https://hindsight.vectorize.io/sdks/integrations/pydantic-ai) сделал pydantic-ai-интеграцию и продвигает её своим контентом ([гайд 04.05.2026](https://hindsight.vectorize.io/guides/2026/05/04/guide-pydantic-ai-memory-with-hindsight)).

---

## (в) Show HN для dev-инструмента без аудитории

### В.1 Распределение очков (HN Algolia, запросы 22.10.2026, тег show_hn)

| Запрос | <10 очков | 10–50 | 50–200 | 200+ | Всего |
|---|---|---|---|---|---|
| «memory agent» | 994 (86,6%) | 93 | 48 | 14 | 1 149 |
| «MCP» | 6 743 (84,7%) | 728 | 371 | 118 | 7 960 |
| «sqlite» | 1 111 | 117 | 90 | 50 | 1 368 |
| «obsidian» | 278 | 28 | 15 | 18 | 339 |
| «LLM memory» | 529 | 71 | 34 | 14 | 648 |

Глобально (188 085 Show HN постов 2012–04.2026, [danfking, 23.04.2026](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/)): **медиана 2 очка**, 90-й перцентиль 24, 99-й — 263; 50+ очков = топ-6%; в 2025 — 28 302 поста, конкуренция ~200 постов/день.

### В.2 24 реальных поста (dev-tools/память/MCP), очки + текущие звёзды репо

Прирост звёзд за 48 часов ретроспективно недоступен — **не нашёл**; колонка «звёзды сейчас» — верхняя граница всего накопленного. Коэффициенты перевода очков→звёзд из danfking см. В.3.

| Имя | URL | Дата (поста) | Число (очки; ★ сейчас) | Вывод |
|---|---|---|---|---|
| mem0 | [HN 41447317](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/mem0ai/mem0) | 04.09.2024 | 201; 67 692★ | сработало |
| graphiti | [HN 41445445](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/getzep/graphiti) | 04.09.2024 | 142; 32 162★ | сработало |
| DiffMem (git как память) | [HN 44969622](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/Growth-Kinetics/DiffMem) | 21.08.2025 | 198; 904★ | сработало |
| ensue-skill («Stop Claude Code from forgetting») | [HN 46426624](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/mutable-state-inc/ensue-skill) | 29.12.2025 | 202; 421★ | сработало |
| Recall (raiyanyahya) | [HN 48622590](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/raiyanyahya/recall) | 21.06.2026 | 138; 763★ | сработало |
| Recall (@joseairosa, Redis) | [HN 45516584](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/joseairosa/recall) | 08.10.2025 | 171; 176★ | частично |
| YourMemory (biological decay) | [HN 47914367](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/sachitrafa/YourMemory) | 26.04.2026 | 98; 270★ | сработало |
| Moltis | [HN 46993587](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/moltis-org/moltis) | 12.02.2026 | 131; 2 906★ | сработало |
| Omnara (2-й пост) | [HN 50183892](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/omnara-ai/omnara) | 21.10.2026 | 111; 3 198★ (1-й пост 12.08.2025 — [310 очков](https://news.ycombinator.com/item?id <tg-id>)) | сработало |
| MCP Memory (fellowgeek, SQLite FTS5 — ближайший аналог sgm) | [HN 49286073](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/fellowgeek/mcp-memory) | 13.08.2026 | 70; 221★ (репо создан в день поста) | сработало |
| Mnemo (Rust+SQLite+petgraph) | [HN 48389586](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/zaydmulani09/mnemo) | 03.06.2026 | 60; 237★ | сработало |
| Total Recall (Claude Code) | [HN 46907183](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/davegoldblatt/total-recall) | 05.02.2026 | 67; 202★ | сработало |
| lossless-memory | [HN 49786419](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/aru-labs/lossless-memory) | 21.09.2026 | 67; 146★ | сработало |
| claude-memory (deshraj) | [HN 41458044](https://news.ycombinator.com/item?id <tg-id>) → repo | 05.09.2024 | 76; ★ — не нашёл | не знаю |
| basic-memory | [HN 43374258](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/basicmachines-co/basic-memory) | 15.03.2025 | **4**; 4 327★ | **пост НЕ сработал** |
| cognee | [HN 44169594](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/topoteretes/cognee) | 09.06.2025 (и 13.02.2025, и 22.04.2024) | **9/6/7**; 32 694★ | **посты НЕ сработали** |
| memU | [HN 44838609](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/NevaMind-AI/memU) | 08.08.2025 | **11**; 14 557★ | **пост НЕ сработал** |
| Files.md (альтернатива Obsidian) | [HN 48179677](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/zakirullin/files.md) | 18.05.2026 | 730; 4 164★ | сработало |
| OpenKnowledge | [HN 48675435](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/inkeep/open-knowledge) | 25.06.2026 | 381; 4 671★ | сработало |
| SmoothMQ (SQLite) | [HN 40837610](https://news.ycombinator.com/item?id <tg-id>) → [repo](https://github.com/poundifdef/smoothmq) | 30.06.2024 | 656; 2 289★ | сработало |
| static.wiki (SQLite, вне памяти — калибровка) | [HN](https://hn.algolia.com/api/v1/search?query=static.wiki&tags=show_hn) | 30.07.2021 | 796 | сработало |
| Mastra (не память — калибровка агентного фреймворка) | [repo](https://github.com/mastra-ai/mastra) | 19.02.2025 | 442 очка | сработало |
| Cursor IDE + MCP (индикатор спроса на тему) | [HN 43506068](https://news.ycombinator.com/item?id <tg-id>) | 28.03.2025 | 109 | сработало |

### В.3 Сколько звёзд даёт пост (медианы по очкам)

По [danfking](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/) (n=491 репо из топ-500 по очкам, все ≥258 очков; коэффициент к меньшим постам — экстраполяция, честно помечаю):

| Очки поста | Звёзды за 48 ч | Откуда число |
|---|---|---|
| <10 (медиана всех постов = 2) | ≈0–3★ | медиана 2 очка × 1.4★/очко — экстраполяция danfking; отдельных измерений не нашёл |
| 10–50 | ≈15–70★ | экстраполяция 1.4★/очко (danfking) |
| 50–200 | ≈70–280★ в 48 ч; в нашей выборке 60–140-очковые посты имеют сейчас 146–763★ (накоплено за месяцы, не за 48 ч) | 1.4★/очко (danfking) + таблица В.2 |
| 200+ | медиана успешного поста (258+): **509★ в день 1**, 40★ в день 2, ~9★/день далее; посты 258–350 очков дают 1.77★/очко, 700+ — всего 0.79★/очко (сублинейно) | [danfking, 23.04.2026](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/) |

**Полураспад Show HN — 24 часа; через 48 ч заканчивается 92% эффекта.** Комментарии не предсказывают звёзды (r=0.10); очки объясняют лишь ~8% дисперсии звёзд (r=0.29). AI/ML и CLI-инструменты конвертят немного лучше среднего (danfking).

### В.4 Что объединяет попавшие на первую страницу — и что убивает

**Что объединяет (данные + разборы):**
- **Время.** Лучший слот — понедельник 00:00 UTC (10.8% постов набирают 50+), далее воскресенье 02:00 UTC (9.8%) и суббота 19:00 UTC (9.2%); худший — четверг 06:00 UTC (2.6%) — [danfking](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/). (Против: [AFFiNE playbook](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) советует вт–чт для «глаз»; конфликт помечен.)
- **Заголовок называет конкретный механизм, без маркетинга**: «I replaced vector databases with Git for AI memory (PoC)» (198), «Stop Claude Code from forgetting everything» (202), «Mem0 – open-source Memory Layer» (201), «Graphiti – LLM-Powered Temporal Knowledge Graphs» (142) ([AFFiNE](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/): техничный честный заголовок бьёт «best productivity app» всегда).
- **Автор в комментариях с первой минуты** (AFFiNE: «Be in the comments from the first minute. HN rewards engagement»).
- **README первого экрана как лендинг**: одна фраза-ценность, GIF/скриншот выше фолда, quickstart ≤5 шагов, видимый CTA звезды (AFFiNE: 1 hero-изображение + 5–7 GIF).
- **Демо без регистрации** и рабочая установка в один шаг (все успешные посты из В.2 — install-and-run).
- **Ответы в первые 2 часа**: «пост закапывается за минуты без 1–2 ранних апвоутов» (danfking) — ранняя вовлечённость решает попадание в rising.

**Что убивало посты:**
- Конкуренция: ~200 Show HN в день в 2026 (danfking); закапывание в очереди за минуты.
- Маркетинговый язык и кликбейт (AFFiNE).
- Комментарии ≠ конверсия: шумная ветка не даёт звёзд (r=0.10, danfking).
- Нулевой соц-пруф: «репо с 0 звёзд конвертит ~5% посетителей… получите 100–200 звёзд от знакомых ДО запуска» (AFFiNE).
- Неверное окно: полураспад 24 ч — «Show HN — это импульс, не стратегия роста» (danfking; сам автор анализа получил 1 очко своим постом MCP-рендерера).

---

## (г) Конверсия «посетитель → звезда»: опубликованные числа

| Источник трафика | Число | Откуда | Вывод |
|---|---|---|---|
| HN (очко → звезда) | **~1.4★ за 1 HN-очко** в 48 ч (медиана по выборке); 258–350 очков → 1.77★/очко; 700+ → 0.79★/очко | [danfking, 23.04.2026](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/) (n=491 репо) | работает как формула планирования |
| HN (фронт-пейдж) | медиана **509★ в первые сутки** для постов 258+ очков; разброс фронт-пейджа 500–2000★/24 ч | danfking + [AFFiNE playbook, 25.03.2026](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) | сработало |
| Reddit (r/selfhosted, r/opensource, r/programming) | **5–8% посетитель→звезда**; 50–300★ с поста (n=12 запусков) | [AFFiNE playbook, 25.03.2026](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) (self-reported) | сработало (Reddit дал AFFiNE 2 000+★ в первый месяц) |
| Документация (doc views → звезда) | basic-memory: **29 400 просмотров доков → 1 004★ ≈ 3.4%** за 6 мес | [Basic Machines blog, 30.06.2025](https://basicmachines.co/blog/100-github-stars/) | сработало |
| Репозиторий без звёзд | конверсия **~5%** посетителей; после 100–500★ незнакомцы начинают ставить; после 1000★ конверсия внешних каналов «~удваивается» | [AFFiNE playbook](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) | ориентир |
| Product Hunt | 200–600★ с сильного запуска (n=8); главное — бейдж соц-пруфа | [AFFiNE playbook](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) | по вашему ТЗ PH у вас не сработал — конфликт опыта с кейсом AFFiNE; помечено |
| Awesome-списки | прямой трафик «мал, но перманентен»; измеримых конверсий **не нашёл**; по кейсу rag-kb ≈0–4★ за 2 мес | [AFFiNE playbook](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) + [наш расчёт](https://github.com/fish827-08/rag-kb) | SEO-гигиена, не канал звёзд |
| GitHub-поиск, Twitter/X, YouTube | опубликованных конверсий **не нашёл** | — | не знаю |

**Пороговые величины соц-пруфа (AFFiNE):** 0–100★ — «посетители уходят сразу»; 100–500 — «достаточно, чтобы попробовать»; 1000–5000 — «активное сообщество»; 5000+ — «упоминают без запроса». Для sgm при текущих 34 посетителях/14 дней и конверсии 3–8% органика даёт ~1★/2 недели — **без чужой дистрибуции 1000★ не набрать** (арифметика: нужно ~15–30k целевых визитов README).

---

## Топ-10 мест для встраивания строки установки sgm

Отсортировано по (верояность принять × охват). «Охват» = звёзды/аудитория хост-платформы; «вероятность» — по правилам и прецедентам из раздела (а).

| # | Место | Вероятность принять | Охват | Первый шаг |
|---|---|---|---|---|
| 1 | **Маркетплейсы плагинов Anthropic для Claude Code** ([community: anthropics/claude-plugins-community](https://github.com/anthropics/claude-plugins-community), [доки](https://code.claude.com/docs/en/plugins/anthropic-marketplaces), [директория claude.ai](https://code.claude.com/docs/en/plugins/publish)) | средняя (есть ревью, но community открыт для сторонних) | максимальный: установка из claude.ai/Code синком — кейс claude-mem (102 440★) | упаковать sgm в плагин Claude Code (`.claude-plugin/plugin.json` + `marketplace.json`), протестировать `claude plugin`, затем PR в community-репо и сабмит в директорию |
| 2 | **Cline MCP Marketplace** ([issue-шаблон](https://github.com/cline/mcp-marketplace)) | средняя (звёзды не нужны, но очередь ревью месяцами — заявки memory висят с 02.2026) | высокий: 70 770★ у Cline, one-click install в приложении | открыть issue по шаблону: лого 400×400, `llms-install.md`, проверка «Cline ставит по одному README» |
| 3 | **punkpeye/awesome-mcp-servers, секция Knowledge & Memory** ([CONTRIBUTING](https://github.com/punkpeye/awesome-mcp-servers/blob/main/CONTRIBUTING.md)) | ~100% (принимают даже агентов; rag-kb взят в день создания) | сам список 96 785★, но прямые клики малы — ценность SEO/кредибилити | PR по формату: строка в «Knowledge & Memory» (локально-устанавливаемый сервер — соответствует scope) |
| 4 | **awesome-claude-code** ([hesreallyhim](https://github.com/hesreallyhim/awesome-claude-code), 56 310★) | высокая (мержат ежедневно, PR «Add resource») | высокий (главный читабельный список экосистемы) | сначала пункт 1 (быть плагином/инструментом Claude Code), затем PR в категорию memory/контекст |
| 5 | **Cursor Directory** ([cursor.directory/plugins/new](https://cursor.directory/plugins/new), витрина [plugins](https://cursor.directory/plugins)) | средняя (форма + модерация) | заявленный «300k+ разработчиков» (слова самой площадки) | сабмит формы с README-first скриншотом и одноминутной установкой (uvx/pip) |
| 6 | **awesome-claude-skills** ([travisvn](https://github.com/travisvn/awesome-claude-skills), 15 517★, создан 16.10.2025) | высокая ([CONTRIBUTING](https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md)) | растущий (skills-тренд: однофайловый skill-репо набрал 144k★ — [обзор](https://medium.com/@aitrends24/a-github-repo-with-one-file-just-hit-144-000-stars-its-a-claude-skill-d82f8b6fa789)) | сделать skill-обёртку sgm (SKILL.md + установка MCP) и PR |
| 7 | **Copilot for Obsidian (MCP)** ([logancyang/obsidian-copilot](https://github.com/logancyang/obsidian-copilot), 7 880★; MCP-поддержка — коммиты 19.10.2026) | средняя (решает мейнтейнер; sgm — идеальный фит: wikilinks как граф) | точная аудитория sgm (владельцы Obsidian-хранилищ), 7.9k★ | issue/Discussion с готовым конфигом MCP + короткий гайд «ваше хранилище как память Claude Code»; предложить PR в их доки ([docs.obsidiancopilot.com](https://docs.obsidiancopilot.com/llm-providers/)) |
| 8 | **LibreChat: MCP Server Guides** ([librechat.ai/docs/mcp_servers](https://www.librechat.ai/docs/mcp_servers), проект 46 187★) | средняя (PR в доки) | высокий | написать гайд «sgm в LibreChat (docker compose + mcp_servers)» и открыть PR в доки проекта |
| 9 | **OpenWebUI Community** ([docs](https://docs.openwebui.com/features/extensibility/community/), проект 156 548★) | высокая (публикация без ревью, «unaudited») | средний (аудитория чат-UI, не IDE) | опубликовать Tool/Function-обёртку sgm (или инструкцию через [mcpo](https://github.com/open-webui/mcpo)) на [openwebui.com](https://openwebui.com/o/ModelContextProtocol) |
| 10 | **Awesome-Agent-Memory (TeleAI-UAGI)** ([693★](https://github.com/TeleAI-UAGI/Awesome-Agent-Memory)) + [AgentMemoryWorld (245★, TMLR 2026)](https://github.com/AgentMemoryWorld/Awesome-Agent-Memory) | высокая (PR, новые списки растут) | низкий трафик, но точная исследовательская аудитория | PR в секцию systems/tools обеих списков |

**Уже сделано / автоматически:** официальный MCP Registry (19.10.2026) → Zed (депрекейтит свои MCP-расширения в пользу Registry), Glama (вбирает Registry). **Не повторять (по вашему ТЗ):** массовые каталоги (Smithery/Glama/mcp.so как отдельные сабмиты), Product Hunt, бенчмарк-посты, веерные PR, купленные звёзды. **Не подходят:** Continue (конфиг без каталога), Goose (allowlist), официальные reference-серверы MCP (только Anthropic), LangChain/LlamaIndex/Agno/Pydantic AI (списков сторонних memory нет — вместо этого пишите интеграционные гайды у себя по паттерну [hindsight](https://hindsight.vectorize.io/sdks/integrations/pydantic-ai), затем предлагайте ссылку в issue).

**Арифметика к 1000★:** Show HN (понедельник 00:00 UTC, честный заголовок про «wikilinks как граф памяти») даёт при попадании в фронт ~150–500★; плагин Claude Code + Cline + cursor.directory — это каналы постоянной установки (кейс claude-mem: 68k npm/мес при 102k★); списки дают ~0 прямых, но нужны как соц-пруф для ревью маркетплейсов. По конверсии 3–8% цель 1000★ требует ≈15–30k целевых визитов README — реально только суммой каналов выше.

---

## Открытые вопросы

1. Прирост звёзд за 48 часов для каждого отдельного Show HN-поста из В.2 — **не нашёл** (GitHub не отдаёт историю звёзд через доступные API; агрегаты danfking — единственный источник).
2. Дата Reddit-поста claude-mem «45,000 stars» и точная кривая 2026 года — **не нашёл** (trendshift даёт только 61-дневное окно).
3. Конверсии для GitHub-поиска, Twitter/X, YouTube — **не нашёл**.
4. Исходы (принято/отклонено) memory-заявок в Cline Marketplace (m3, Mnemoverse, Memory Nexus и др.) — issue открыты, решение **не нашёл**.
5. Звёзды репозиториев funes, PLUR, memviz, Memory-Plus, Jean Memory, deshraj/claude-memory — **не нашёл**.
6. PyPI-пакет memU — **не нашёл** (pypi-имена memu-ai/pymemu не существуют).
7. Трафик (просмотры) самих awesome-списков — GitHub не публикует чужие просмотры, **не нашёл**.

## Источники

| Источник | Доверие | Обновлено |
|---|---|---|
| [danfking — Show HN by the Numbers](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/) (188 085 постов, n=491 репо со звёздами) | 4/5 | 23.04.2026 |
| [AFFiNE playbook (gingiris)](https://gingiris.github.io/growth-tools/blog/2026/03/25/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study/) + [dev.to зеркало](https://dev.to/iris1031/how-to-get-more-github-stars-the-definitive-guide-33k-stars-case-study-2kjo) (self-reported кейс) | 3/5 | 25.03.2026 |
| [Basic Machines — 1,000 GitHub Stars](https://basicmachines.co/blog/100-github-stars/) (данные из первых рук) | 4/5 | 30.06.2025 |
| HN Algolia API ([hn.algolia.com/api/v1/search](https://hn.algolia.com/api/v1/search)) — распределения и посты, запросы разделов В/б | 5/5 | 22–23.10.2026 |
| GitHub Search API (звёзды, даты, коммиты «Add …») — punkpeye/awesome-mcp-servers, awesome-claude-code, cline/mcp-marketplace, репо проектов | 5/5 | 23.10.2026 |
| [pypistats.org API](https://pypistats.org/api/packages/basic-memory/overall?mirrors=false) (без зеркал) | 4/5 | 23.10.2026 |
| [npm registry API](https://api.npmjs.org/downloads/point/last-month/claude-mem) | 5/5 | 23.10.2026 |
| [trendshift.io — claude-mem](https://trendshift.io/repositories/15496), [trendshift.io — memU](https://trendshift.io/repositories/17374) | 4/5 | 23.10.2026 |
| [cline/mcp-marketplace README](https://github.com/cline/mcp-marketplace) (правила приёма из первых рук) | 5/5 | 2026 |
| [punkpeye/awesome-mcp-servers CONTRIBUTING](https://github.com/punkpeye/awesome-mcp-servers/blob/main/CONTRIBUTING.md) | 5/5 | 2026 |
| [Claude Code docs — плагины и маркетплейсы](https://code.claude.com/docs/en/plugins/anthropic-marketplaces), [publish](https://code.claude.com/docs/en/plugins/publish) | 5/5 | 2026 |
| [MCP Registry](https://modelcontextprotocol.io/registry/about) + [анонс 08.09.2025](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/) | 5/5 | 2025–2026 |
| [TechCrunch — mem0 $24M](https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps/) | 4/5 | 28.10.2025 |
| [cognee — раунд $7.5M](https://www.cognee.ai/blog/cognee-news/cognee-raises-seven-million-five-hundred-thousand-dollars-seed), [tracxn](https://tracxn.com/d/companies/cognee/__XkSxgBzH5vOB59VwVuEQNwoDJxGMlvIPIq8cIV5WsIY) | 3/5 | 2024–2025 |
| [Zep blog — Graphiti/PH](https://blog.getzep.com/blog/tag/graphiti/), [стратегия опенсорса](https://blog.getzep.com/announcing-a-new-direction-for-zeps-open-source-strategy/) | 4/5 | 2025 |
| [mem0 — OpenMemory MCP](https://mem0.ai/blog/introducing-openmemory-mcp) | 4/5 | 13.05.2025 (обновл. 25.08.2026) |
| [docs.cline.bot — Memory Bank](https://docs.cline.bot/best-practices/memory-bank) | 5/5 | 2026 |
| [star-history — Preevy playbook](https://www.star-history.com/blog/playbook-for-more-github-stars/) | 3/5 | 24.08.2023 |
| [Zed — MCP extensions](https://zed.dev/docs/extensions/mcp-extensions), [Cursor Directory](https://cursor.directory/plugins/new), [OpenWebUI docs](https://docs.openwebui.com/features/extensibility/community/), [LibreChat MCP guides](https://www.librechat.ai/docs/mcp_servers), [Continue docs](https://docs.continue.dev/customize/deep-dives/mcp) | 4/5 | 2026 |
| [obsidian-copilot](https://github.com/logancyang/obsidian-copilot) (коммиты MCP 19.10.2026), [hindsight/pydantic-ai](https://hindsight.vectorize.io/sdks/integrations/pydantic-ai) | 4/5 | 2026 |

*Конфликты данных:* AFFiNE советует вт–чт для HN, danfking даёт лучшие шансы пн 00:00 UTC — вероятно, работают оба при хорошем контенте; AFFiNE хвалит Product Hunt (200–600★), ваш опыт — нет: кейс AFFiNE относится к consumer-ish self-hosted продукту, для узкого dev-инструмента PH обычно слабее.