# DR26-11-03-HUB-01-0950 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Thought Process

Принято. Это исследовательская задача с контрактом: живой веб-поиск, ссылки на первоисточники, русский язык, таблицы, «Чего в данных нет», три вывода, отчёт прямо в чат. Начинаю с плана и первых серий поисков (~55+ запросов планируется).

1 searches

Первые результаты уже дают важные поправки (Zep теперь называет себя «Context Lake», Mem0 — 62,6k звёзд, Supermemory закрыл корпоративное приложение в сент. 2026, DMR = Deep Memory Retrieval). Продолжаю: Letta, Supermemory, MemOS, Memobase, MemMachine.

1 searches

Letta Cloud: Free/$19/$249/Enterprise (по стороннему источнику). Добираю Supermemory, MemOS, Memobase, MemMachine и новых игроков.

1 searches

Много новых фактов (Supermemory на самом деле поднял $26M Seed в окт. 2025 — поправка к вводным; Mem0 уже 68k звёзд; MemOS 11.8k звёзд, плагины для OpenClaw/Hermes). Продолжаю: Memobase, MemMachine, Graphlit, MIRIX, A-MEM.

1 searches

MemMachine (MemVerge, arXiv 2604.04853) и MIRIX (3.5k звёзд) найдены. Ещё: Memobase, Graphlit, LangMem, память Anthropic и leaderboard бенчмарков.

1 searches

Memobase найден (профиль+таймлайн, 85% LoCoMo temporal по их замерам). Также заметил: Couchbase запустила managed Agent Memory + managed MCP (окт. 2026). Добираю Graphlit, LangMem, Anthropic Memory Tool, leaderboard.

1 searches

Graphlit и LangMem закрыты. Теперь облачные гиганты: Anthropic Memory Tool, AWS AgentCore Memory, Vertex AI Memory Bank, Zep (страница цен).

1 searches

Отлично: AgentCore Memory цены найдены (и обновление Oct 2026), Vertex Memory Bank теперь в «Gemini Enterprise Agent Platform» (апр. 2026), Work IQ = Data+Memory+Inference. Теперь проверяю Zep напрямую: цены, Graphiti, конкуренты из их же документации.

Explored 1 files，1 searches

Zep: Graphiti 32.4k звёзд, MCP Server 1.0, «Context Lake» + Konig, заявленные LoCoMo 94.7% и LongMemEval 90.2%; цены с публичной страницы убраны (важная поправка). Теперь MCP-память и споры о бенчмарках.

1 searches

MCP-слой закрыт (OpenMemory, Basic Memory $15/mo + Teams, RecallWorks, Mnemoverse). Спор LoCoMo зафиксирован с обеих сторон (zep-papers issue #5). Перехожу к личному сегменту.

1 searches

Mem.ai (агент «chief of staff», $9–$99) и Notion AI (кредитная модель с 2 нояб. 2026) закрыты. Добираю остальные личные инструменты.

1 searches

Отражаю важные сдвиги: Reflect перешёл в open source с локальными Markdown-файлами; Tana запустила отдельный продуктовый «botless meeting agent». Продолжаю — Heptabase, Recall, Personal.ai, Limitless, OSS-личные.

1 searches

Пусто — переформулирую запросы.

1 searches

Khoj — 37.9k звёзд, Screenpipe переехал в свою org и стал YC S26, Plaud — $100M ARR и IPO-планы. Добираю носимые и локальные OSS.

1 searches

Bee не умер — Amazon выпустил Bee Frontier/Eclipse 15 окт. 2026; Reor фактически заброшен (домен перехвачен); Obsidian Copilot — 3.1M загрузок с агентами Claude Code/Codex внутри волта. Перехожу к корпоративному сегменту.

1 searches

Квота поисковых вызовов на этот запуск исчерпана (61 запрос по 13 батчам выполнено; «Web tool limit reached» пришёл на корпоративном блоке). Данных достаточно, чтобы собрать полный отчёт; непроверенное честно помечено. Ниже — итоговый отчёт.

Карта рынка «второго мозга» и общей памяти vs Zep AI

Дата исследования: 3 ноября 2026. Метод: живой веб-поиск (61 запрос) + чтение первоисточников (сайты продуктов, GitHub, docs, arXiv). Названия продуктов и компаний — в English. Все ссылки — полные URL. Там, где факт не подтверждён, стоит «нет данных». Вводные, которые вы уже знали, не пересказываются — только поправки (раздел 0).

0. Поправки к вашим вводным (устарело / неточно)
ВАШЕ УТВЕРЖДЕНИЕ
	
ЧТО ПОКАЗАЛ ПОИСК
	
ИСТОЧНИК

Zep: цены Flex $125, Flex Plus $375, Emerging $13k/год	На текущей https://www.zep.ai/pricing публичных цен больше нет — только «Request a Demo» / «See enterprise plans». Тиры, судя по всему, переведены за пейволл. Уточняйте напрямую у Zep (завтра на интервью это уместный вопрос)	https://www.zep.ai/pricing
Zep называет конкурентами Mem0, Letta, Cognee, Supermemory, AWS AgentCore Memory, Vertex AI Memory Bank	Подтверждено и расширилось: у Zep появились SEO-страницы «Alternative» по каждому из них, плюс Zep теперь называет себя Context Lake (а не только Context Graph), у них собственный проприетарный графовый сервис Konig поверх OSS-движка Graphiti	https://www.getzep.com/aws-agentcore-memory-alternative , https://www.getzep.com/platform/graphiti
Supermemory: $3M pre-seed	Устарело: всего поднято $29M — $3M ранний раунд + $26M Seed (6 окт. 2025) от Browder Capital, Susa Ventures, Scientifica VC; ангелы — Jeff Dean, Logan Kilpatrick, David Cramer, Dane Knecht. В сент. 2026 Supermemory закрыл «company brain»-приложение и Nova, сфокусировался на memory engine	https://startupintros.com/orgs/supermemory , https://supermemory.ai/blog/author/dhravya
Mem0: 41k+ звёзд	Устарело: 68k звёзд (окт. 2026), Apache-2.0	https://github.com/mem0ai/mem0 (via org-страница)
Glean: $300M ARR	В этом прогоне не перепроверено (лимит поисков). Цифру оставляю как вашу вводную, публичного подтверждения на 2026 не нашёл	нет данных
—	Новое: у Zep CEO и сооснователь — Preston Rasmussen (сооснователи: Pavlo Paliychuk; YC W24, ~10 человек, seed ~$1.8M от Root Ventures / Engineering Capital) — полезно для завтрашнего интервью	https://yespress.io/zep-ai
1. Память агентов (agent memory layer): SaaS и OSS
1.1 Независимые игроки (сверх вашего списка)
ПРОДУКТ
	
МОДЕЛЬ ПАМЯТИ
	
OSS / ЛИЦЕНЗИЯ
	
ДЕПЛОЙ
	
ЦЕНА
	
ДЕНЬГИ И ТЯГА
	
ИСТОЧНИК

Mem0	Вектор + LLM-консилиация фактов (ADD/UPDATE/DELETE/NOOP), опционально граф на Neo4j; скоупы user/agent/app/run	OSS Apache-2.0 + платформа	Облако, self-host	Публичные тарифы не зафиксированы в этом прогоне (страница https://mem0.ai/pricing)	$24M (Seed — Kindred Ventures, Series A — Basis Set; Peak XV, GitHub Fund, YC), 68k звёзд, 35M→186M API-вызовов (Q1→Q3 2025); AWS выбрал Mem0 «эксклюзивным memory-провайдером» для своего Agent SDK	https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps , https://mem0.ai/series-a
Letta (ex-MemGPT)	Агент сам управляет памятью: контекст-блоки, MemFS/файлы, self-editing; агент = персистентный серверный объект	Apache-2.0 + облако	Облако, self-host	Letta Cloud: Free / $19 / $249 / Enterprise (по стороннему обзору)	Репо активно (v1.7.0, окт. 2026); есть Letta Code — «memory-first coding agent»	https://gamgee.ai/vs/mem0-vs-letta , https://agenticskills.io/tools/letta , https://github.com/letta-ai/letta-code-testing
Cognee	Граф знаний из документов/кода/чатов; ECL-пайплайн; remember/recall/forget; онтологии	Apache-2.0	Локально бесплатно, Cognee Cloud, свой облако	нет данных (funding $1.5M — ваша вводная, не перепроверено)	Позиционируется ближе всего к «памяти компании»: коннекторы Slack/GitHub/Linear, «одна общая память для многих агентов»	https://github.com/topoteretes/cognee , https://coworker.ai/blog/mem0-vs-zep-vs-letta
Supermemory	Memory API: факты из диалогов + профили пользователя + retrieval по документам; «Context Cloud», SMFS (файловая система для агентов), Dynamic Dreaming	Engine MIT, single binary self-host	Облако / self-host	нет данных (usage-based)	$29M суммарно; LongMemEval-заявки: 95% (GPT-4o), 85.2% (Gemini 3 Pro) — их собственные замеры	https://startupintros.com/orgs/supermemory , https://supermemory.ai/blog/author/dhravya
LangMem (LangChain)	Семантические памяти: hot-path инструменты + background-менеджер консолидации; хранение в LangGraph Store	MIT	Библиотека (любое хранилище)	бесплатно (библиотека)	Репо langchain-ai/langmem; запущен в фев. 2025, статус beta, активность 2025 г.	https://github.com/langchain-ai/langmem , https://blog.langchain.dev/langmem-sdk-launch/
MemOS (MemTensor)	«Memory OS»: граф + вектор + BM25 + fulltext (4-канальный retrieval), MemCube-изоляция, WorkingMemory/LongTerm/User/Skill и др.; NL-фидбек на память	Apache-2.0	Локально (SQLite, 100% on-device плагины), docker compose (Neo4j+Qdrant), облако	нет данных	11.8k звёзд (окт. 2026); плагины для OpenClaw, Hermes, DeepSeek Harness; заявки LoCoMo 88.83 / LongMemEval 89.20 через собственный OmniMemEval	https://tomrochette.com/agents/memory/memos , https://gist.github.com/Disdjj/eaea6581afb2c83e1d121e630dacde16
Memobase (Memodb)	Профиль пользователя + таймлайн событий (не RAG, не граф); <80ms retrieval	OSS (GitHub)	Self-host docker (FastAPI+Postgres+Redis) + облако с дашбордом	нет данных	Заявка: 85% на LoCoMo temporal — выше Mem0/Zep (их замер)	https://github.com/memodb-io/memobase , https://www.i-programmer.info/news/105-artificial-intelligence/18295-memobase-add-memory-to-your-ai-agents.html
MemMachine (MemVerge)	STM + LTM + профиль; «LLM-sparing»: LLM только для суммаризации/профиля, не на каждый факт; agent-mode retrieval	OSS (GitHub) + коммерческая поддержка	Self-host, облако	нет данных	Backing от MemVerge (дек. 2025); arXiv 2604.04853 (апр. 2026); заявка LoCoMo 91.7% (gpt-4.1-mini, agent mode)	https://github.com/MemMachine/MemMachine , https://www.prnewswire.com/news-releases/memverge-expands-memmachine-ecosystem-and-ai-memory-system-capabilities-302642918.html , https://arxiv.org/html/2604.04853v1
MIRIX	Multi-agent память: Core/Episodic/Semantic/Procedural/Resource/Knowledge Vault; менеджеры-агенты пишут, ассистент читает	OSS	Desktop-приложение + self-host	нет данных	3.5k звёзд; arXiv 2507.07957; поддержка носимых (скриншоты/wearables); активен окт. 2026	https://github.com/Mirix-AI/MIRIX , https://arxiv.org/html/2507.07957v1
Graphlit (Unstruk Data)	«Context layer»: организационная память — сущности, связи, события, темпоральное состояние, полный provenance; 30+ коннекторов; MCP-native	Проприетарный SaaS	Облако	Free / Hobby $49 / Starter $199 + usage-кредиты	Крупных раундов не найдено в этом прогоне: нет данных	https://www.graphlit.com , https://www.graphlit.com/vs/cognee
Hindsight (Vectorize)	Memory banks; retain / recall / reflect; консолидация в «observations» с сохранением evidence; MIT; Postgres	MIT + hosted	Self-host на Postgres / облако	нет данных	Заявка README: LongMemEval-результаты независимо воспроизведены Virginia Tech и The Washington Post	https://coworker.ai/blog/mem0-vs-zep-vs-letta
A-MEM	Агентическая память по мотивам Zettelkasten: динамическое создание связей/заметок (бумага 2025)	OSS (2025, бумага)	Библиотека	—	Академический проект, коммерческой тяги не зафиксировано	https://arxiv.org/abs/2502.12110
1.2 Встроенная память больших вендоров
ВЕНДОР
	
ЧТО ИМЕННО
	
МОДЕЛЬ
	
ЦЕНА/ДОСТУП
	
ИСТОЧНИК

OpenAI	ChatGPT Memory: «Saved memories» + «Reference chat history» (+ в 2026 «memory summary»); выключаемо, удаление из систем за 30 дней	Скрытые суммаризованные представления, не граф, не открыто API	Включено в ChatGPT (в т.ч. Business)	https://help.openai.com/en/articles/8590148-memory-in-chatgpt , https://embracethered.com/blog/posts/2025/chatgpt-how-does-chat-history-memory-preferences-work
Anthropic	(1) Memory tool + context editing в API (2025); (2) в марте 2026 — нативная персистентная память для всех пользователей Claude, включая free, + импорт памяти из других тулов; Opus 4.6 (фев. 2026) — agent teams	Файлово-инструментальная в API; нативная — закрытая	Включено в подписки/API	https://dev.to/mike_w_06c113a8d0bb14c793/anthropic-just-shipped-native-memory-here-is-what-it-cannot-do-l7c , https://www.datastudios.org/post/anthropic-launches-claude-opus-4-6-with-ultra-long-context-agent-teams-and-expanded-output-limits , https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3 (критика: «Memory API без ranked retrieval»)
Google	Vertex AI Memory Bank теперь внутри Gemini Enterprise Agent Platform (запуск 22 апр. 2026): memories.generate, Memory Profiles (структурированные профили, LLM-апдейт); Gemini Enterprise (окт. 2025, ex-Agentspace) — enterprise search + агенты	Профили + факты, управляемый сервис	Agent Platform — usage-based; Gemini Enterprise ~$21 / $30 / $50–60 за юзера/мес (третьи лица)	https://docs.cloud.google.com/gemini-enterprise-agent-platform/reference/rest/v1beta1/projects.locations.reasoningEngines.memories/generate , https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/profiles , https://www.gosearch.ai/faqs/gemini-enterprise-pricing , https://ustechautomations.com/resources/blog/gemini-enterprise-agent-platform-explained-what-it-changes
AWS	Bedrock AgentCore Memory: short-term (события) + long-term (записи); с 6 окт. 2026 STM переведён на consumption-тарификацию	События → извлечённые записи; namespace-изоляция	STM: было $0.25/1k событий; стало $1.00/GB ingest + $0.20/GB retrieval + $0.10/GB-мес; LTM: $0.75/1k записей/мес, retrieval $0.50/1k	https://aws.amazon.com/bedrock/agentcore/pricing , https://aws.amazon.com/about-aws/whats-new/2026/10/agentcore-memory-consumption-based-pricing , https://cloudburn.io/blog/amazon-bedrock-agentcore-pricing
Microsoft	M365 Copilot Work IQ (Ignite, ноя. 2025): «intelligence layer» из трёх компонентов — Data, Memory, Inference; Agent 365 — реестр/управление агентами	Личная рабочая память внутри экосистемы M365	В составе M365 Copilot	https://blog.devops.dev/microsofts-agentic-enterprise-stack-explained-layer-by-layer-74bf523a4954 , https://www.reworked.co/digital-workplace/ignite-2025-shows-microsofts-full-stack-ai-ambition , https://cloudwars.com/ai/ai-agent-copilot-podcast-volt-technologies-mason-whitaker-on-microsoft-agent-365-ignite-highlights
xAI	Grok: «Memory across chats» с апр. 2025 (за пределами EU/UK сначала; к июлю 2026 добралось частично до EU/UK, бета)	Закрытая	Включено в Grok	https://plurality.network/blogs/does-grok-ai-have-persistent-memory , https://www.ai-toolbox.co/grok-management-and-productivity/does-grok-remember-past-conversations-2026
Couchbase	Managed Agent Memory + managed MCP-сервер в AI Data Plane (20 окт. 2026)	БД-вендор добавляет memory как managed-фичу	В составе Couchbase Capella	https://www.i-programmer.info/news/105-artificial-intelligence/18295-memobase-add-memory-to-your-ai-agents.html (новостная заметка-сайдбар)
1.3 Бенчмарки: кто и как мерил (важно для интервью)
LoCoMo (бумага 2024: https://arxiv.org/abs/2402.17753) — самый цитируемый, но самый спорный. Хроника конфликта: Zep заявлял 84% в своей бумаге (https://arxiv.org/abs/2501.13956) → Mem0 опубликовали ре-ран с «исправленной методологией» и получили 58.44% (https://github.com/getzep/zep-papers/issues/5) → Zep ответил постом «Why LoCoMo is a flawed evaluation» (https://www.getzep.com/blog/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory) → независимый разбор фиксирует: Zep пересмотрел до 75.14%, Mem0 получил 58.44%, спор открыт (https://theaiengineer.substack.com/p/cognee-vs-zep-vs-mem0-vs-letta). Сейчас Zep показывает на своём сайте LoCoMo 94.7%, LongMemEval 90.2%, 155–162 мс (https://www.getzep.com/platform/graphiti); Mem0 — LongMemEval 94.4 на своей странице сравнения (цит. по https://coworker.ai/blog/mem0-vs-zep-vs-letta).
LongMemEval (https://arxiv.org/abs/2410.10813) — по нему «каждый вендор показывает себя первым»: Mem0 94.4, Supermemory 95% (GPT-4o) / 85.2% (Gemini 3 Pro) — конфигурации разные, страницы сравнения взаимно противоречивы (https://coworker.ai/blog/mem0-vs-zep-vs-letta , https://supermemory.ai/blog/author/dhravya).
DMR (Deep Memory Retrieval) — бенчмарк из статьи MemGPT (2023); Zep заявлял 94.8% против MemGPT 93.4% (https://theaiengineer.substack.com/p/cognee-vs-zep-vs-mem0-vs-letta). Независимых воспроизведений не нашёл.
OmniMemEval — собственный фреймворк MemTensor на 14 коммерческих продуктов (LoCoMo 88.83 / LongMemEval 89.20 — их заявки) (https://tomrochette.com/agents/memory/memos).
Agent Memory Leaderboard — первый академический общий цикл (август 2026): MemOS — 2-е место среди коммерческих text-memory систем с 45.89 (шкала фреймворка). Это единственная найденная мной надвендорская оценка (цит. по https://tomrochette.com/agents/memory/memos).
Вывод по бенчмаркам: все топовые цифры — вендорские; единственные заявленно-независимые: Hindsight (Virginia Tech + Washington Post воспроизвели LongMemEval) и Agent Memory Leaderboard. На интервью это самый сильный блок вопросов к CEO Zep: почему 94.7 на LoCoMo после публичного пересмотра 84→75.14.
2. Общая память между агентами (multi-agent shared memory)

Кто явно продаёт/строит общую память:

РЕШЕНИЕ
	
КАК ДЕЛИТСЯ ПАМЯТЬ
	
КОНФЛИКТЫ ЗАПИСИ / ПРАВА / PROVENANCE
	
ИСТОЧНИК

Zep / Graphiti MCP Server 1.0	Один граф читается и пишется из Claude Desktop, Cursor, ChatGPT и любых MCP-клиентов; «one user memory across all clients»; группы (group_id) для командных графов	Битемпоральные факты: противоречие не стирает, а закрывает valid_at/invalid_at; у каждого факта — источник-эпизод (provenance); MCP-сервер — reference-only, без auth (сами предупреждают не выставлять наружу)	https://www.getzep.com/blog/graphiti-hits-20k-stars-mcp-server-1-0 , https://help.getzep.com/graphiti/getting-started/mcp-server , https://help.getzep.com/use-zep-in-claude-chatgpt , https://www.getzep.com/blog/tag/context
OpenMemory (Mem0)	Локальный MCP-сервер: общая память для Claude Code, Cursor, Codex, Kimi Code, OpenCode, OpenClaw, Pi Agent	Скоупы user/agent/app/run; реестр конфликтов нет — консилиация ADD/UPDATE/DELETE силами LLM; известны проблемы совместимости с клиентами (issue #2712)	https://docs.mem0.ai/introduction , https://github.com/mem0ai/mem0/issues/2712
Basic Memory (Basic Machines)	Markdown-файлы + MCP: человек и ИИ пишут в одни файлы; клиент-агностик (Claude, Codex, Cursor, ChatGPT, Gemini, Obsidian, OpenClaw, Hermes); Cloud $15/мес (локально бесплатно, AGPL-3.0); есть Teams (роли, team projects, SSO, аудит)	Конфликты решаются git/rclone-синком «with conflict resolution»; права — в облачном Teams-тире; локально — нет ACL	https://github.com/basicmachines-co/basic-memory , https://docs.basicmemory.com/welcome
Mnemoverse	Managed memory over MCP: «одна память в Cursor, Claude Code, VS Code, ChatGPT»; multi-tenant; Rooms (бета)	Отдельные статьи в их библиотеке прямо про нерешённые проблемы: «Shared Memory Poisoning: One Bad Write, Many Agents», «Nowhere to Put the Disagreement», «The Missing Layer: Agent Memory Interop» — сам факт этих текстов = рынок признаёт, что конфликтов записи и происхождения факта нет standards-решения	https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3
Recall (RecallWorks)	OSS MCP-сервер: per-agent namespaces, tiered memory (session/durable/reference/anti-pattern), MIT, один docker	Namespaces против «перекрёстного заражения» агентов; вето/забывание инструментом forget; ACL — нет данных	https://forum.cursor.com/t/recall-open-source-mcp-memory-server-for-cursor-and-any-mcp-client/160078
mex	«Team memory for engineers and their AI agents. Lives in your repo. Shared through Git» — память команды как часть git-репо, MCP + Claude Code/Cursor	Конфликты — через git; права — через права репо	https://github.com/topics/project-memory
musecl-memory	MEMORY.md в git + ~200 строк bash для синка с детекцией конфликтов	Конфликты — детекция в скрипте; контрибуторы в обсуждении Mem0 признают, что для семантического поиска это не замена	https://github.com/mem0ai/mem0/discussions/4051 , https://github.com/musecl/musecl-memory
Letta	Состояние агента — персистентные блоки; multi-agent-паттерны поверх блоков; Letta Code — memory-first harness	Модель «каждый агент управляет своей памятью»; общий граф/ACL — нет данных	https://www.letta.com/blog/agent-memory
MemOS	Плагины OpenClaw/Hermes/DeepSeek Harness на одинMemOS-кор; MemCube = изоляция с «controlled sharing»	MemCube прямо позиционируется как изоляция мульти-юзер/мульти-проект с контролируемым шарингом	https://tomrochette.com/agents/memory/memos
Coworker	«Organizational memory» из 50+ инструментов (Slack, Salesforce, Jira, Drive), общая для людей и агентов через MCP-клиенты (Claude, ChatGPT, Cursor)	Права наследуются от исходных систем («permission-aware by inheritance») — доступ к факту = доступ к источнику; это прямое решение «memory layer как обход ACL»	https://coworker.ai/blog/mem0-vs-zep-vs-letta
Graphlit	MCP-нативная организационная память с 30+ коннекторами	«Full provenance — every answer traceable to source»	https://www.graphlit.com

Ключевой вывод по пункту 2: рынок делится на (а) локальные «файловые» общие памяти (Basic Memory, mex, musecl, AGENTS.md/CLAUDE.md-конвенции — кодифицировано в посте Supermemory про OpenCode: https://supermemory.ai/blog/author/dhravya), (б) MCP-серверы с namespaces (OpenMemory, Recall, Graphiti, Graphiti MCP), (в) managed-организационные (Mnemoverse, Coworker, Couchbase MCP). Стандарта на разрешение конфликтов записи и provenance между разными вендорами не существует — ни один найденный продукт не даёт права доступа на уровне факта между независимыми ИИ-вендорами; ближе всех — Coworker (наследование прав) и Zep (п provenance-эпизоды + битемпоральность).

3. Личный второй мозг (частное лицо)
ПРОДУКТ
	
ПОМНИТ «САМ» ИЛИ ТРЕБУЕТ РУЧНОГО ВЕДЕНИЯ
	
ЦЕНА (2026)
	
СТАТУС/ОСОБЕННОСТИ
	
ИСТОЧНИК

Mem.ai	Сам: «Mem Agent — AI chief of staff», Push-to-Remember, Voice Mode, авто-детект задач из заметок, коннекторы Gmail/Slack/Todoist	Free / Plus $9 / Pro $29 / Pro 2× $49 / Pro 5× $99 (Mem 2.0, с окт. 2025)	Активен; free-план жёстко квотован (25 заметок/25 сообщений)	https://get.mem.ai/pricing , https://www.saner.ai/blogs/mem-ai-reviews
Notion AI	Полу-само: Enterprise Search + Notion Agent + AI Meeting Notes; с 2 нояб. 2026 кредитная модель (1 500 Standard + 500 Select кредитов/юзер/мес; $10/1k кредитов; $0.03–0.30 за запуск Custom Agent)	Business $20/юзер/мес (годовой), AI включён; Enterprise — по запросу	Личная память как таковая отсутствует — это поиск+агенты по воркспейсу	https://gildhq.com/research/notion-ai-cost-model-2026
Reflect	Ручное ведение + опциональный AI BYO-ключ (GPT-4/Whisper), E2E-шифрование	Mac — бесплатно, iOS — подписка (~$10/мес по старому Classic)	Разворот 2026: стал open-source приложением с локальными Markdown-файлами и синком iCloud/Git — классическая SaaS-модель закрыли	https://reflect.app , https://toolguide.io/en/tool/reflect-notes
Tana	Раздвоение: (1) outliner-воркспейс (supertags, AI-кредиты, Plus от $8/мес); (2) новый продукт tana.inc — botless AI meeting-агент (транскрипт звонков → задачи в Linear/Jira/CRM), Pro $20–30, Max $80–120, MCP с OAuth	Free / Pro / Max / Business	—	https://www.saasradar.fr/en/tana , https://scored.tools/tools/tana
Heptabase	Ручное ведение (визуальные вайтборды) + AI-исследование с цитатами	Подписка (см. https://www.heptabase.com/pricing) — точную цену в этом прогоне не зафиксировал	YC-бэкед, активен, 1–10 сотрудников	https://startupintros.com/orgs/heptabase , https://apps.apple.com/us/app/heptabase/id6445801508
Capacities	Ручное (объекты + бидирект-линки) + AI Assistant в Pro	Free core; Pro ~$10–12/мес	Объектная PKM	https://www.aicentralresources.com/compare/mem-vs-capacities
Recall (getrecall.ai)	нет данных — в этом прогоне не проверено	нет данных	нет данных	нет данных
Personal.ai	нет данных — в этом прогоне не проверено	нет данных	нет данных	нет данных
Khoj	Сам: OSS self-host «AI second brain», индексация волта/PDF, агенты, локальные LLM	Бесплатно self-host; облако с free tier	37.9k звёзд; компания расширилась: Pipali (desktop AI co-worker), Open Paper	https://github.com/khoj-ai/khoj , https://khoj.dev
Reor	Локальный AI-notes (RAG + Ollama)	—	Фактически заброшен: reorproject.org перехвачен, issue о хайджеке открыт фев. 2026	https://github.com/reorproject/reor/issues/533
Obsidian + Copilot	Ручной волт + агент-плагин: Copilot V4 запускает Claude Code, Codex, OpenCode прямо в волте; локальный индексатор Miyo; скиллы общие между агентами	Плагин free (BYOK), лицензия опциональна	3.1M загрузок; Smart Connections — семантическая память волта; Shadow — авто-конспект встреч в волт	https://community.obsidian.md/plugins/copilot , https://www.obsidiancopilot.com/en , https://www.shadow.do/blog/best-ai-plugins-for-obsidian-2026
Screenpipe	Максимально «сам»: 24/7 запись экрана+микрофона локально, MCP-сервер → «что я делал во вторник»; с мая 2026 вынесен из mediar-ai, YC S26, 18.5k звёзд; разворот к записи корпоративных рабочих машин и поиску автоматизаций	нет данных (source-available)	—	https://github.com/screenpipe/screenpipe , https://screenpipe.com , https://www.mediar.ai/t/mediar-ai-screenpipe
Plaud (Note/NotePin)	Сам: носимая запись → транскрипт → суммаризация; AI Pro $239.99/год	Устройства от $159 + подписка	Прибыльна, ~$100M recurring, 2.5M пользователей, готовится к IPO	https://www.plaud.ai/products/plaud-note-plaud-ai-pro-plan , https://getlatka.com/companies/plaud-inc , https://www.facebook.com/quartznews/posts/icymi-the-ai-note-taker-startup-betting-it-can-hit-1b-before-going-public-plaud-/1442198984442582
Bee (Amazon)	Сам: ambient-носимый, слушает всё, транскрибирует	Устройства от $75 (Bee Frontier, Bee Eclipse — запуск 15 окт. 2026)	Куплена Amazon в июле 2025; линейка расширена, вокруг — приватностный скандал «always listening»	https://www.aboutamazon.com/news/devices/amazon-personal-ai-bee-frontier-bee-eclipse , https://techcrunch.com/2026/10/15/amazons-bee-launches-two-new-personal-ai-devices-starting-at-75 , https://www.cnbc.com/2026/10/15/amazon-bee-frontier-eclipse-ai-devices.html
Limitless	Сам: пояс-записыватель + app/web, «personalized AI powered by what you've seen, said, and heard»	нет данных в этом прогоне	Активен (апдейты батареи pendant); юридические ограничения записи	https://www.limitless.ai/vs/bee , https://www.afr.com/technology/the-sky-oh-and-the-law-is-the-limit-with-this-ai-wearable-20250704-p5mci7
Omi (Based Hardware)	Сам: OSS-носимый + суммаризация встреч/задач	Dev Kit 2 (цена не зафиксирована)	Активен 2026	https://www.omi.me/blogs/ai-enhanced-therapy/firmware-guides

Автоматическое vs ручное: «помнит само» — носимые (Plaud, Bee, Limitless, Omi), Screenpipe, Mem.ai (после захвата), Khoj (после индексации), ChatGPT/Claude/Grok/Gemini built-in memory. «Ручное» — Obsidian, Heptabase, Capacities, Reflect (кроме голосовых заметок), Tana-outliner. Промежуточный класс «полу-автоматический захват» — Shadow (встречи → Obsidian), Reflect Open, Supermemory-плагины для OpenCode.

4. Второй мозг компании
ПРОДУКТ
	
ЧТО ЭТО: ГРАФ ЗНАНИЙ ИЛИ ПОИСК ПО ДОКУМЕНТАМ
	
ПРОВЕРЕНО?
	
ИСТОЧНИК

Glean	Work AI платформа: enterprise search + People Graph/Work Graph + агенты. Именно «граф компании», а не только поиск — но в этом прогоне не перепроверено (ARR $300M — ваша вводная, не подтверждена)	Частично: свежих данных нет	нет данных (запас: https://www.glean.com)
Microsoft 365 Copilot / Work IQ / Graph	Work IQ = Data + Memory + Inference поверх Microsoft Graph; Agent 365 для управления агентами. Да, это граф компании + личная рабочая память	Да	https://blog.devops.dev/microsofts-agentic-enterprise-stack-explained-layer-by-layer-74bf523a4954 , https://www.reworked.co/digital-workplace/ignite-2025-shows-microsofts-full-stack-ai-ambition
Google Gemini Enterprise (ex-Agentspace)	Enterprise search + агенты + коннекторы (Workspace, M365, Salesforce, ServiceNow, Jira); Memory Bank в Agent Platform. Граф компании — косвенно (через поиск и коннекторы)	Да	https://www.gosearch.ai/faqs/gemini-enterprise-pricing , https://ustechautomations.com/resources/blog/gemini-enterprise-agent-platform-explained-what-it-changes
Notion Enterprise	Enterprise Search по Notion+Slack+Drive и Custom Agents; памяти/графа компании нет	Да (кредитная модель)	https://gildhq.com/research/notion-ai-cost-model-2026
Coworker	Явно «organizational memory»: непрерывный граф из 50+ систем, права наследуются от источников, общая для людей и агентов	Да (вендорский источник)	https://coworker.ai/blog/mem0-vs-zep-vs-letta
Harness Knowledge Graph	Собственный внутренний «Knowledge Graph компании» + DSL (HQL) для агентов: 15–25x экономия токенов против raw-API; 4-уровневая модель владения данными	Да (вендорский кейс)	https://www.harness.io/blog/why-harness-ai-uses-knowledge-graph
Couchbase AI Data Plane	Managed Agent Memory + managed MCP-сервер: БД-вендор превращает память агентов в инфраструктуру	Да (новость 20 окт. 2026)	https://www.i-programmer.info/news/105-artificial-intelligence/18295-memobase-add-memory-to-your-ai-agents.html
Atlassian Rovo / Teamwork Graph	В вашей выборке; в этом прогоне не перепроверено	нет данных	нет данных (запас: https://www.atlassian.com/software/rovo)
Guru, Slack AI, Dust, Onyx (ex-Danswer), Sana (Workday), Hebbia, Moveworks (ServiceNow), Writer Knowledge Graph	Все из вашей выборки; в этом прогоне квоты на проверку не хватило — позиции 2025 года не подтверждаю и не опровергаю	нет данных	нет данных

Кто строит именно «граф знаний компании», а не поиск: по подтверждённым данным этого прогона — Microsoft (Graph + Work IQ), Coworker (явно заявляет организационную память с наследованием прав), Harness (свой KG + HQL), Zep («Account context» — граф на аккаунт-клиента, https://www.getzep.com/platform/graphiti), Cognee (если компания сама подключает Slack/GitHub/Linear, https://coworker.ai/blog/mem0-vs-zep-vs-letta), Graphlit (сущности/связи/provenance, https://www.graphlit.com). Glean и Writer — по вашей вводной и их историческому позиционированию, но в 2026 не перепроверено.

5. Слияние личного и корпоративного

Прямой ответ: продукта, где личная память агента сотрудника и память компании — два слоя одного графа (с правами, приватностью и владением данными при увольнении), не существует среди найденных. Ни один из ~40 проверенных продуктов не заявляет двухслойную модель «личный мозг + мозг компании в одном графе».

Ближайшие приближения (каждое закрывает часть стека):

ПРИБЛИЖЕНИЕ
	
ЧТО ЕСТЬ
	
ЧЕГО НЕТ

Microsoft Work IQ	Личная рабочая память (Memory) внутри M365 + организационный Graph — архитектурно два слоя, но оба принадлежат Microsoft 365,(employee не владеет ничем)	Экспорта личного слоя за пределы M365; переносимости при увольнении
Coworker	Организационная память с наследованием прав от 50+ систем, общая для людей и агентов	Личный «второй мозг» сотрудника как первый слой; личная память живёт в ChatGPT/Claude, а не в Coworker
Notion	Персональные кредиты и персональные агенты + общий воркспейс	Кредиты — биллинг, не память; личный слой отсутствует
Basic Memory Teams	Локальные личные Markdown-мозги + командные проекты с ролями/SSO/аудитом в облаке	Нет авто-извлечения фактов, нет корпоративного графа, нет коннекторов к системам компании
Gemini Enterprise	Личный контекст в Gemini + организационный поиск	Двухслойный граф не заявлен
Screenpipe (корп. разворот)	Локальная запись рабочего дня сотрудника → организационная карта workflow	Это журнал активности, не семантическая память; права/владение не решены

Ниша «федерация личных мозгов сотрудников в мозг компании» пуста: единственные тексты, которые прямо называют эту проблему, — библиография Mnemoverse («Agent Memory Interop», «Shared Memory for Multi-Agent Systems»: https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3) и маркетинг Coworker («per-agent memory does not compose»: https://coworker.ai/blog/mem0-vs-zep-vs-letta).

6. Сравнительная таблица с Zep
ИГРОК
	
СЕГМЕНТ
	
ПОХОЖ НА ZEP
	
ОТЛИЧАЕТСЯ ОТ ZEP
	
ЦЕНА (ПРОВЕРЕНО)

Zep	Агенты + enterprise	—	—	Публичных цен нет (пейволл); BYOK/BYOC, SOC 2 Type II, HIPAA BAA
Mem0	Агенты	SaaS+OSS, developer-first, экстракция фактов, скоупы	Плоский вектор (+опц. граф Neo4j), нет битемпоральности и graph-обхода; 68k звёзд против 32.4k у Graphiti; статус «эксклюзив memory у AWS Agent SDK»	Тарифы не зафиксированы; OSS Apache-2.0
Letta	Агенты	OSS-ядро + облако, stateful-агенты	Агент сам управляет памятью (блоки, файлы) vs граф, извлекаемый Zep; нет темпорального графа	Cloud $19/$249 (3rd-party)
Cognee	Агенты + «company memory»	Граф знаний, OSS Apache-2.0, self-host	Онтологии/ECL-пайплайн, асинхронный «improve/forget»; нет битемпоральных фактов и managed-экстракции <200мс	Self-host бесплатно; облако — нет данных
Supermemory	Агенты	Memory API + профили + retrieval; MIT engine + self-host binary	Вектор/профили, не граф; SMFS-файлы; «Context Cloud» дешевле-проще; closed company-brain app	нет данных (usage)
LangMem	Агенты (LangGraph)	OSS, экстракция семантических памяти	Библиотека без managed-сервиса, без графа, без SLA	Бесплатно
MemOS	Агенты	Граф+гибридный поиск, OSS, локальные плагины	MemCube-изоляция, 4-канальный retrieval; битемпоральности нет (NL-фидбек вместо); свои бенчмарки OmniMemEval	Self-host бесплатно; облако — нет данных
Memobase	Агенты (B2C-приложения)	Профили + события, time-aware	Профильно-таймлайновая модель, не граф; <80ms	нет данных
MemMachine	Агенты	STM+LTM, OSS + backing, arXiv-бумага	«LLM-sparing» (дешёвый write-path против дорогого у Zep); LoCoMo 91.7% заявлен	нет данных
MIRIX	Агенты, персональные ассистенты	Шесть типов памяти, мульти-агентная запись	Desktop-first, wearable-ориентация; нет enterprise-деплоя	Бесплатно OSS
Graphlit	Агенты + корп. контекст	Граф, provenance, темпоральное состояние, коннекторы	Fully managed SaaS (нет OSS-движка); 30+ интеграций из коробки	$49/$199 + usage
Hindsight	Агенты	OSS, память с evidence	retain/recall/reflect-примитивы на Postgres; заявленно-независимая валидация LongMemEval	нет данных
OpenAI Memory	Личный (consumer/Business)	Персональный контекст через чаты	Закрытая модель, без API, без экспорта, не граф, lock-in как фича	Включено
Anthropic Memory	Личный + API (tool)	—	Memory tool в API = файловая память на клиенте ( ranked retrieval критикуют), нативная память — закрытая	Включено
Google Memory Bank / Gemini Ent.	Агенты + корп.	Managed facts/profiles для агентов, Vertex/GEM-интеграция	Профили+факты, не битемпоральный граф; GCP-lock-in	Usage-based; сиды ~$21–60/юзер
AWS AgentCore Memory	Агенты (Bedrock)	Managed short+long-term, namespaces	События→записи, не граф; AWS-lock-in; партнёрство с Mem0	$0.25/1k events; с окт. 2026 consumption; LTM $0.75/1k/мес
Microsoft Work IQ	Корп.	Граф компании + личная память	Только внутри M365; не API для сторонних агентов	В составе M365 Copilot
Coworker	Корп. + мульти-агент	Организационный граф, общая для людей и агентов, MCP	Наследование прав от источников (у Zep — ACL на уровне платформы); строится из систем, а не из чатов	нет данных
Basic Memory	Личный + команды (MCP)	Markdown+граф связей, кросс-вендорный MCP	Локальные файлы, человеко-читаемость; нет автоматической экстракции; AGPL	Локально free; Cloud $15/мес
Glean	Корп.	Граф людей/знаний компании	Search-first, не memory-layer для агентов; не OSS	Не проверено ($300M ARR — ваша вводная)
7. Свободные ниши (с уликами)
Общая память нескольких ИИ-вендоров у одного человека/команды — ниша наполовину занята, стандарт отсутствует. Улика: Basic Memory, OpenMemory, Mnemoverse, Graphiti MCP уже «шарят» память между Claude/Cursor/Codex/ChatGPT, но конфликт записи решён только git-механикой (https://github.com/mem0ai/mem0/discussions/4051) или namespaces (https://forum.cursor.com/t/recall-open-source-mcp-memory-server-for-cursor-and-any-mcp-client/160078); выделенной литературы о «memory poisoning» и «где жить разногласию» (https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3) нет продуктового решения. Ближе всех: Mnemoverse (managed, мульти-клиент) и Basic Memory (локальный).
Второй мозг семьи — пусто. Улика: ни один найденный личный продукт (Mem.ai, Notion, Reflect, Limitless, Bee) не заявляет семейную память с ролями/наследованием; ближайшие — Notion-воркспейсы и Bee/Limitless (персональные). Продукта нет.
Федерация личных мозгов сотрудников в мозг компании с владением при увольнении — пусто. Улика: Work IQ и Coworker оба держат личный слой внутри платформы работодателя; ни у одного нет «вынести личный граф с собой». Ближе всех: Basic Memory Teams (личные файлы + командный слой) и Coworker (права от источников).
Провенанс и права доступа на уровне факта между вендорами — пусто. Улика: Graphlit и Zep дают provenance внутри своей системы; Coworker наследует права от систем-источников; но cross-vendor ACL на факт никто не продаёт.
Независимая валидация памяти-бенчмарков — только начинает появляться. Улика: Agent Memory Leaderboard (август 2026, первый цикл) и воспроизведение LongMemEval для Hindsight (Virginia Tech/WaPo) — исключения на поле вендорских цифр.
Чего в данных нет
Текущие публичные цены Zep (страница за пейволлом) и цены Mem0, Letta (только 3rd-party), Cognee Cloud, Supermemory, MemOS Cloud, Memobase Cloud, MemMachine, Hindsight, Coworker, Mnemoverse, Limitless, Omi, Screenpipe.
Glean 2026 (ARR/valuation), Atlassian Rovo/Teamwork Graph 2026, Writer Knowledge Graph 2026, Hebbia, Moveworks/ServiceNow (закрытие сделки), Dust, Onyx, Sana/Workday (закрытие), Guru, Slack AI 2026 — квота поисков закончилась на этом блоке; позиции 2025 года не подтверждаю и не опровергаю.
Recall (getrecall.ai) и Personal.ai — статус 2026 не проверен.
Независимые воспроизведения LoCoMo/DMR для Zep и Mem0 — отсутствуют; DMR-числа Zep никем не перепроверены.
Точные лицензии Memobase и доли рынка/выручки всех memory-стартапов (кроме Mem0 API-метрик и Plaud $100M).
Google Gemini consumer-память (Personal Context) в 2026 — отдельно не проверена (проверен только enterprise/Agent Platform).
Три вывода, которые меняют действия

1. Для интервью (завтра, 04.11.2026): главный риск Zep — не Mem0, а бенчмарковая репутация + вендор-лок впораhs. История LoCoMo (84% → 75.14% → Mem0-ре-ран 58.44% → сейчас 94.7% на сайте) — самое уязвимое место; вопросы Preston Rasmussen стоит строить вокруг методологии 94.7% (кто мерил, какие конфиги, почему числа выросли после пересмотра) и вокруг входа AWS/Google в memory-слой (AgentCore съедает низ рынка, Mem0 — его эксклюзив у AWS Agent SDK; Vertex Memory Bank переехал в Gemini Enterprise Agent Platform). Сильная сторона Zep, на которой держать удар: битемпоральность + provenance-эпизоды + «Context Lake» с BYOC/SOC2/HIPAA — этого нет ни у Mem0, ни у вендорских memory, а corporate-запрос на права и историю фактов только обостряется (кейс Coworker/Harness это подтверждает). Уточните и новый прайс (публичных цен больше нет) — это признак движения вверх-маркет.

2. Для вашего продукта: вы находитесь в единственной по-настоящему пустой нише — «федерация личного и корпоративного с владением сотрудника». Ваш стек (Obsidian-волт + графовый RAG на SQLite + общая память 8 машин и Claude/Codex/Grok) уже совпадает с направлением рынка: локальные Markdown/файловые мозги + MCP (Basic Memory $15/мес Cloud и Teams — прямой ориентир прайсинга; Reor умер, Reflect ушёл в локальные файлы — спрос на «локальный второй мозг» подтверждён и деньгами, и трупами). Уникальность вашего подхода — не волт и не RAG (это коммодитизировано), а связка «личный граф ↔ общий граф 8 машин и нескольких вендоров» с контролем записи. Чтобы не повторять чужое: не стройте векторную экстракцию фактов (Mem0), не стройте managed-облако без OSS (Supermemory, Graphlit), не стройте только-личное (все consumer-приложения) — стройте правила конфликта записи и provenance между независимыми агентами: это то, что Mnemoverse описывает текстами, но никто не продаёт кодом.

3. Общий вывод: «память» уже не продукт, а слой инфраструктуры — и идёт вертикализация по деплою. Сверху — hyperscalers с usage-тарифами (AgentCore с 6 окт. 2026 перевёл STM на GB-тарификацию; Couchbase добавила managed Agent Memory и MCP прямо в БД), посередине — memory-API стартапы с OSS-движками (Mem0 $24M, Supermemory $29M, Zep), снизу — локальные MCP-файл-мозги за $0–15/мес. Через 12–18 месяцев чистый «memory SaaS» для агентов сожмётся до 2–3 игроков; выживают те, у кого есть (а) OSS-движок с распределением, (б) enterprise-деплой (BYOC/VPC) и (в) уникальная модель данных. У Zep — (а)+(б)+битемпоральность; у вашего решения — шанс на уникальную модель «двухслойного федеративного графа», которую, по данным этого исследования, никто ещё не вывел на рынок.

## SOURCES (anchors)
https://www.zep.ai/pricing
https://www.getzep.com/aws-agentcore-memory-alternative
https://www.getzep.com/platform/graphiti
https://startupintros.com/orgs/supermemory
https://supermemory.ai/blog/author/dhravya
https://github.com/mem0ai/mem0
https://yespress.io/zep-ai
https://mem0.ai/pricing
https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps
https://mem0.ai/series-a
https://gamgee.ai/vs/mem0-vs-letta
https://agenticskills.io/tools/letta
https://github.com/letta-ai/letta-code-testing
https://github.com/topoteretes/cognee
https://coworker.ai/blog/mem0-vs-zep-vs-letta
https://github.com/langchain-ai/langmem
https://blog.langchain.dev/langmem-sdk-launch/
https://tomrochette.com/agents/memory/memos
https://gist.github.com/Disdjj/eaea6581afb2c83e1d121e630dacde16
https://github.com/memodb-io/memobase
https://www.i-programmer.info/news/105-artificial-intelligence/18295-memobase-add-memory-to-your-ai-agents.html
https://github.com/MemMachine/MemMachine
https://www.prnewswire.com/news-releases/memverge-expands-memmachine-ecosystem-and-ai-memory-system-capabilities-302642918.html
https://arxiv.org/html/2604.04853v1
https://github.com/Mirix-AI/MIRIX
https://arxiv.org/html/2507.07957v1
https://www.graphlit.com/
https://www.graphlit.com/vs/cognee
https://arxiv.org/abs/2502.12110
https://help.openai.com/en/articles/8590148-memory-in-chatgpt
https://embracethered.com/blog/posts/2025/chatgpt-how-does-chat-history-memory-preferences-work
https://dev.to/mike_w_06c113a8d0bb14c793/anthropic-just-shipped-native-memory-here-is-what-it-cannot-do-l7c
https://www.datastudios.org/post/anthropic-launches-claude-opus-4-6-with-ultra-long-context-agent-teams-and-expanded-output-limits
https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3
https://docs.cloud.google.com/gemini-enterprise-agent-platform/reference/rest/v1beta1/projects.locations.reasoningEngines.memories/generate
https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/profiles
https://www.gosearch.ai/faqs/gemini-enterprise-pricing
https://ustechautomations.com/resources/blog/gemini-enterprise-agent-platform-explained-what-it-changes
https://aws.amazon.com/bedrock/agentcore/pricing
https://aws.amazon.com/about-aws/whats-new/2026/10/agentcore-memory-consumption-based-pricing
https://cloudburn.io/blog/amazon-bedrock-agentcore-pricing
https://blog.devops.dev/microsofts-agentic-enterprise-stack-explained-layer-by-layer-74bf523a4954
https://www.reworked.co/digital-workplace/ignite-2025-shows-microsofts-full-stack-ai-ambition
https://cloudwars.com/ai/ai-agent-copilot-podcast-volt-technologies-mason-whitaker-on-microsoft-agent-365-ignite-highlights
https://plurality.network/blogs/does-grok-ai-have-persistent-memory
https://www.ai-toolbox.co/grok-management-and-productivity/does-grok-remember-past-conversations-2026
https://arxiv.org/abs/2402.17753
https://arxiv.org/abs/2501.13956
https://github.com/getzep/zep-papers/issues/5
https://www.getzep.com/blog/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory
https://theaiengineer.substack.com/p/cognee-vs-zep-vs-mem0-vs-letta
https://arxiv.org/abs/2410.10813
https://www.getzep.com/blog/graphiti-hits-20k-stars-mcp-server-1-0
https://help.getzep.com/graphiti/getting-started/mcp-server
https://help.getzep.com/use-zep-in-claude-chatgpt
https://www.getzep.com/blog/tag/context
https://docs.mem0.ai/introduction
https://github.com/mem0ai/mem0/issues/2712
https://github.com/basicmachines-co/basic-memory
https://docs.basicmemory.com/welcome
https://forum.cursor.com/t/recall-open-source-mcp-memory-server-for-cursor-and-any-mcp-client/160078
https://github.com/topics/project-memory
https://github.com/mem0ai/mem0/discussions/4051
https://github.com/musecl/musecl-memory
https://www.letta.com/blog/agent-memory
https://get.mem.ai/pricing
https://www.saner.ai/blogs/mem-ai-reviews
https://gildhq.com/research/notion-ai-cost-model-2026
https://reflect.app/
https://toolguide.io/en/tool/reflect-notes
https://www.saasradar.fr/en/tana
https://scored.tools/tools/tana
https://www.heptabase.com/pricing
https://startupintros.com/orgs/heptabase
https://apps.apple.com/us/app/heptabase/id6445801508
https://www.aicentralresources.com/compare/mem-vs-capacities
https://github.com/khoj-ai/khoj
https://khoj.dev/
https://github.com/reorproject/reor/issues/533
https://community.obsidian.md/plugins/copilot
https://www.obsidiancopilot.com/en
https://www.shadow.do/blog/best-ai-plugins-for-obsidian-2026
https://github.com/screenpipe/screenpipe
https://screenpipe.com/
https://www.mediar.ai/t/mediar-ai-screenpipe
https://www.plaud.ai/products/plaud-note-plaud-ai-pro-plan
https://getlatka.com/companies/plaud-inc
https://www.facebook.com/quartznews/posts/icymi-the-ai-note-taker-startup-betting-it-can-hit-1b-before-going-public-plaud-/1442198984442582
https://www.aboutamazon.com/news/devices/amazon-personal-ai-bee-frontier-bee-eclipse
https://techcrunch.com/2026/10/15/amazons-bee-launches-two-new-personal-ai-devices-starting-at-75
https://www.cnbc.com/2026/10/15/amazon-bee-frontier-eclipse-ai-devices.html
https://www.limitless.ai/vs/bee
https://www.afr.com/technology/the-sky-oh-and-the-law-is-the-limit-with-this-ai-wearable-20250704-p5mci7
https://www.omi.me/blogs/ai-enhanced-therapy/firmware-guides
https://www.glean.com/
https://www.harness.io/blog/why-harness-ai-uses-knowledge-graph
https://www.atlassian.com/software/rovo
https://www.graphlit.com
https://reflect.app
https://khoj.dev
https://screenpipe.com
https://www.glean.com