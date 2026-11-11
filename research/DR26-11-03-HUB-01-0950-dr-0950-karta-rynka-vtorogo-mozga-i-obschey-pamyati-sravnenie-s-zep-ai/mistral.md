# DR26-11-03-HUB-01-0950 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.






IMPORTANT: deliver the complete report as Markdown text directly in this chat (no PDF, no files). Cite every source as a full URL with https://, at least 30 web searches.

Show more
10:14am
Ran a web search

Отчёт готов. Ниже — полная карта рынка по результатам живого поиска (ноябрь 2026): ~70 поисков и открытий первоисточников, все ссылки проверены. Устаревшие факты из вашего брифа помечены ⚠️ с поправкой.

DR26-11-03-HUB-01-0950 — Карта рынка «второго мозга» и общей памяти: личный, корпоративный, слияние, память агентов; сравнение с Zep AI

Дата исследования: 3 ноября 2026 · Подготовлен: к собеседованию с CEO Zep AI (04.11.2026) и для позиционирования собственного продукта (Obsidian-волт + графовый RAG на SQLite + общая память 8 машин и агентов Claude/Codex/Grok)

Резюме — 7 главных выводов
Сегмент «память агентов» стал массовым и коммодитизировался: помимо известных вам Mem0, Letta, Cognee, Supermemory, в 2025–2026 появились LangMem, MemOS, Memobase, Hindsight (Vectorize), Caura, MemMachine (MemVerge), TencentDB Agent Memory — плюс встроенная память у OpenAI, Anthropic, Google, Microsoft, AWS. Zep — один из ~15+ игроков, но с уникальной ставкой на битемпоральный граф.
⚠️ Ваши данные частично устарели: Cognee поднял €7.5M seed (февраль 2026), а не $1.5M ( https://www.eu-startups.com/2026/02/german-ai-infrastructure-startup-cognee-lands-e7-5-million-to-scale-enterprise-grade-memory-technology/); Zep отменил self-hosted Community Edition в апреле 2025 — open source остался только движок Graphiti (~31.2k звёзд, сентябрь 2026) ( https://caura.ai/blog/zep-alternatives/); Recall (recall.it) — это $1.5M раунд, а «€38M» из одного источника — путаница с другой компанией Recall.ai (API для встреч, $10M Series A).
Консолидация корпоративного сегмента идёт быстро: Moveworks куплен ServiceNow за $2.85B (закрыт дек. 2025) ( https://techcrunch.com/2025/03/10/servicenow-buys-moveworks-for-2-85b-to-grow-its-ai-portfolio/), Sana — Workday за $1.1B ( https://investor.workday.com/news-and-events/press-releases/news-details/2025/Workday-Signs-Definitive-Agreement-to-Acquire-Sana-09-16-2025/default.aspx), Onyx (ex-Danswer) — Zoom (сент. 2026) ( https://news.zoom.com/zoom-acquires-onyx-adding-open-source-enterprise-knowledge-search-to-zoom-ai-services/), Limitless — Meta (дек. 2025) ( https://mlq.ai/news/meta-acquires-ai-wearables-startup-limitless-ending-sales-of-pendant-device/), Bee — Amazon (июль 2025) ( https://techcrunch.com/2025/07/22/amazon-acquires-bee-the-ai-wearable-that-records-everything-you-say/).
Big tech закрыл базовый сценарий «память» бесплатно: Copilot Memory выходит в GA в ноябре 2026 с персонализацией на рабочих данных и цитированием источников памяти ( https://windowsforum.com/news/copilot-enhanced-memory-brings-work-data-personalization-in-november-2026.429482/), Claude memory — на всех платных планах с окт. 2025 ( https://www.anthropic.com/news/memory), Gemini — Personal Context с авг. 2025 ( https://9to5google.com/2025/08/13/gemini-personal-context/). Это сжимает рынок «простой персонализации» и оставляет Zep нишу глубоких, темпоральных, multi-source знаний.
Ниша «общая память нескольких ИИ-вендоров у одного человека» занята только open source и ранними стартапами (OpenMemory MCP от Mem0, basic-memory, thesecondbrain.dev, MemoryBox от MemVerge) — ни у одного нет ни SaaS-зрелости, ни битемпоральности, ни нормального решения конфликтов записи и provenance.
Продукта «личная память сотрудника + корпоративная память = два слоя одного графа с правами и offboarding-ом» на рынке НЕ СУЩЕСТВУЕТ. Ближе всех Microsoft (Work IQ: personal memory внутри корпоративного семантического индекса, GA июнь 2026) — но там нет личного владения при увольнении ( https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/).
Все бенчмарки — поле войны вендоров: Mem0 заявляет 92.5% LoCoMo / 94.4% LongMemEval ( https://mem0.ai/blog/ai-memory-benchmarks-in-2026), Zep оспаривает и заявляет превосходство на 24% ( https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/), Hindsight (Vectorize) заявляет >90% LongMemEval ( https://www.prnewswire.com/news-releases/vectorize-breaks-90-on-longmemeval-with-open-source-ai-agent-memory-system-302643146.html). Независимой верификации нет; сами LoCoMo/LongMemEval критикуются за методологические дыры ( https://www.reddit.com/r/AIMemory/comments/1s1jlnd/serious_flaws_in_two_popular_ai_memory_benchmarks/).
Методология и ограничения
Поисковые углы: (1) вендоры памяти агентов, (2) MCP-инфраструктура и кросс-вендорная память, (3) личные second-brain приложения и носимые устройства, (4) enterprise knowledge/AI-платформы, (5) поглощения и раунды как маркеры сегментов, (6) бенчмарки и их критика.
Типы источников: официальные страницы продуктов и pricing (Zep, Microsoft, Anthropic, Mem0, Slack, Zoom, Workday, Writer), arXiv-статьи, TechCrunch/Slashdot-класс СМИ, GitHub-репозитории, вендорские блоги (помечены как заинтересованные), независимые сравнения (vectorize.io, caura.ai — оба являются вендорами, раскрыто в самих статьях).
Ограничения: цены Enterprise-планов (Glean, Writer, Hebbia, Zep) почти нигде не публикуются; звёзды GitHub быстро меняются; значительная часть бенчмарков — самозаявленные. Русскоязычный рынок (например, YandexGPT-память) не исследовался — вне скоупа. Часть источников — SEO-каталоги (надёжность 2/5), они использованы только там, где первоисточник недоступен, и это помечено.
1. Память агентов (agent memory layer)
1.1 Специализированные вендоры: SaaS и open source
Игрок
Модель памяти
OSS / лицензия
Деплой
Цена
Деньги и тяга
Zep
Битемпоральный граф фактов (valid_at/invalid_at), гибридный поиск
Graphiti OSS (~31.2k звёзд на 30.09.2026 —  https://caura.ai/blog/zep-alternatives/); ⚠️ Zep CE (self-hosted) deprecated с апреля 2025
Cloud / BYOK / BYOC
Free 10k credits/мес; Flex $125 (50k credits); Flex Plus $375 (200k); Enterprise custom — официальная страница:  https://www.getzep.com/pricing/
Нет данных о публичном раунде (профили:  https://tracxn.com/d/companies/zep/__poSadJnSfLWHjz05Xi3U5KwnpCMWSU3aDrihLX_8FLs)
Mem0
Векторное хранилище + граф (Mem0g, только в Pro) —  https://vectorize.io/articles/mem0-vs-zep
Apache 2.0, ~48k звёзд (окт. 2026)
Cloud / self-host
Free 10k записей; Starter $19; Pro $249 (граф отсюда) —  https://wallabytoken.com/blog/p/ai-memory-pricing-compared
$24M Series A (окт. 2025, Basis Set Ventures, Peak XV, YC), 14M скачиваний, 41k+ звёзд на момент раунда:  https://www.bwdisrupt.com/article/mem0-raises-24-mn-from-basis-set-ventures-peak-xv-partners-others-577381
Letta (ex-MemGPT)
Агент сам редактирует свою память (self-editing memory, sleep-time compute) —  https://yespress.io/letta
Apache 2.0, self-host бесплатно
Cloud / self-host
Free 3 агента BYOK; Pro $20/мес; Teams $20/seat —  https://wallabytoken.com/blog/p/ai-memory-pricing-compared
$10M seed, Felicis (сент. 2024):  https://techcrunch.com/2024/09/23/letta-one-of-uc-berkeleys-most-anticipated-ai-startups-has-just-come-out-of-stealth/
Cognee
«ECL»-пайплайн (Extract-Cognify-Load) → граф + векторы из документов/кода
OSS (Apache 2.0)
Cloud / self-host
нет данных (индивидуальный)
⚠️ €7.5M seed (февр. 2026), Берлин — не $1.5M:  https://www.eu-startups.com/2026/02/german-ai-infrastructure-startup-cognee-lands-e7-5-million-to-scale-enterprise-grade-memory-technology/
Supermemory
Memory API + плагины для кодинг-агентов; биллинг по SM-токенам
Есть self-hosted binary
Cloud / self-host
Free $5 credits; Pro $19; Max $100; Scale $399 —  https://supermemory.ai/docs/overview/billing и  https://wallabytoken.com/blog/p/ai-memory-pricing-compared
⚠️ $3M pre-seed, Susa Ventures (окт. 2025):  https://www.thesaasnews.com/news/supermemory-raises-3m-pre-seed/
LangMem (LangChain)
SDK: semantic/episodic/procedural память для LangGraph-агентов
OSS SDK
Библиотека + LangGraph Store
бесплатно (SDK)
Часть экосистемы LangChain/LangGraph:  https://www.agentpatternscatalog.org/landing/compositions/langmem/
MemOS (MemTensor)
«Memory OS»: MemCube унифицирует plaintext/activation/parametric память; paper arXiv 2507.03724 —  https://arxiv.org/abs/2507.03724
OSS:  https://github.com/MemTensor/MemOS
Self-host / cloud
нет данных
Компания MemTensor (Шанхай, 2024), оценка $50–100M по Dealroom:  https://dealroom.co/companies/memory-tensor-technology/; заявляет 35.24% экономии токенов
Memobase
Профиль пользователя + таймлайн событий, без эмбеддингов
Apache 2.0 —  https://dev.co/ai/rag/memobase
Self-host (Helm)
нет данных
Малоизвестный OSS-проект; звёзды: нет данных
Graphlit
Облачный API: инжест → knowledge graph + RAG
SDK OSS ( https://github.com/graphlit/graphlit), платформа закрытая
Только cloud
Usage-based —  https://agentery.com/agents/graphlit
Нет данных (профиль PitchBook закрыт:  https://pitchbook.com/profiles/company/467636-05)
Hindsight (Vectorize)
«Человеческая» память: факты → наблюдения → mental models, время-aware, без графовой БД; temporal retrieval strategy
MIT —  https://vectorize.io/blog/introducing-hindsight-agent-memory-that-works-like-human-memory
Self-host / Hindsight Cloud
нет данных (Cloud —  https://docs.hindsight.vectorize.io/cloud/whats-new/)
Запуск окт. 2026 с заявкой >90% LongMemEval (вендор):  https://www.prnewswire.com/news-releases/vectorize-breaks-90-on-longmemeval-with-open-source-ai-agent-memory-system-302643146.html; Memlake: 100M воспоминаний, <100 мс p99 —  https://hindsight.vectorize.io/blog/2026/10/13/memlake
Caura
Governed shared memory: event-time validity, As-Of Recall, supersession, на PostgreSQL
Apache 2.0
Self-host / cloud
нет данных
Молодой игрок; сравнение пишет сам вендор (раскрыто):  https://caura.ai/blog/zep-alternatives/
MemMachine / MemoryBox (MemVerge)
«Big Memory» компания: MemMachine — OSS memory layer для LLM; MemoryBox — личная память между AI-приложениями, локально
OSS + enterprise —  https://www.prnewswire.com/news-releases/memverge-launches-memmachine-worlds-most-powerful-ai-memory-layer-302563813.html
Локально / enterprise
нет данных
MemoryBox в бете:  https://app.dealroom.co/news/feed/memorybox-ai-launches-beta-of-private-ai-memory-tool-for-power-users
A-MEM
Исследование: Zettelkasten-агентная память (заметки пишет сам LLM, динамические связи)
Research code, NeurIPS 2025
—
—
Paper:  https://arxiv.org/html/2502.12110v1
TencentDB Agent Memory
4-уровневый локальный пайплайн памяти L0–L3
MIT (май 2026) —  https://www.marktechpost.com/2026/05/23/tencent-open-sources-tencentdb-agent-memory-a-4-tier-local-memory-pipeline-for-ai-agents/
Локально
нет данных
Заявляет −61% токенов:  https://toknow.ai/posts/tencentdb-agent-memory-local-memory-hub-agents/

Длинный хвост (существуют, но без заметных денег/тяги): SuperLocalMemory, Recallium, MemoryLake, Hjarni (markdown-память), Mnemoverse — видны в сравнениях  https://dev.to/varun_pratapbhardwaj_b13/5-ai-agent-memory-systems-compared-mem0-zep-letta-supermemory-superlocalmemory-2026-benchmark-59p3 и  https://hjarni.com/blog/mem0-vs-supermemory-vs-hjarni.

1.2 Встроенная память больших платформ
Вендор
Что появилось
Дата
Источник
OpenAI ChatGPT
Память между чатами + controls; rollout лёгкой памяти на free tier
Запуск теста февр. 2024; расширение июнь 2025
 https://openai.com/index/memory-and-new-controls-for-chatgpt/ ,  https://x.com/OpenAI/status/1929937841905381558
OpenAI «Dreaming»
⚠️ Неофициально: авто-синтез воспоминаний в фоне, якобы замена сохранённых memories (41.5%→82.8% на собственном eval). Только сторонние блоги, низкая надёжность, официальной страницы не найдено
2026
 https://cicero.studio/en/blog/chatgpt-persistent-memory-personalization-ai-visibility-2026/ ,  https://www.digitalapplied.com/blog/chatgpt-memory-dreaming-v3-openai-2026-guide
Anthropic Claude
Память в Claude app: Team/Enterprise, затем Pro/Max (23.10.2025); project-scoped memory, incognito; плюс memory tool для разработчиков и CLAUDE.md/auto-memory в Claude Code
сент.–окт. 2025
 https://www.anthropic.com/news/memory ,  https://www.macrumors.com/2025/10/23/anthropic-automatic-memory-claude/ ,  https://code.claude.com/docs/en/memory
Google Gemini
«Saved Info»/память + Personal Context (учитывает чаты и приложения Google); Temporary Chats
авг. 2025
 https://9to5google.com/2025/08/13/gemini-personal-context/ ,  https://arstechnica.com/ai/2025/08/google-gemini-will-now-learn-from-your-chats-unless-you-tell-it-not-to/
Microsoft M365 Copilot
Copilot Memory: персонализация на истории чатов (preview с нояб. 2025, GA нояб. 2026), память на рабочих данных, панель источников памяти с цитатами
нояб. 2025 → нояб. 2026
 https://msmessagecenter.com/MC1158329 ,  https://mc.merill.net/message/MC1492154 ,  https://windowsforum.com/news/copilot-enhanced-memory-brings-work-data-personalization-in-november-2026.429482/
AWS Bedrock AgentCore Memory
Управляемая память агентов (short/long-term), consumption pricing за GB ingested/retrieved/stored, MCP server
2025–2026
 https://aws-news.com/article/01a1321e-45d4-3328-617d-4d18d2b276ba ,  https://www.developersdigest.tech/agent-memory/aws-agentcore-memory
Google Vertex AI Memory Bank
Управляемая память per-user: Gemini извлекает факты и предпочтения, scoped per user
2025+
 https://dreaming.press/stack/vertex-ai-memory-bank (низкая надёжность, но согласуется с),  https://discuss.google.dev/t/lets-talk-about-vertex-ai-memory-bank-heres-a-quick-faq-on-the-top-5-questions-weve-seen/244685
Cursor
⚠️ Наоборот: удалил Memories в версии 2.1 — чаты начинаются с нуля, память через Rules/AGENTS.md/MCP
2025–2026
 https://blog.memoryplugin.com/cursor-memory/ ,  https://recallium.ai/cursor
1.3 Бенчмарки: LoCoMo, LongMemEval, DMR — и кто их мерил
LoCoMo — длинные многосессионные диалоги. Mem0 заявляет 92.5% (вендор:  https://mem0.ai/blog/ai-memory-benchmarks-in-2026). Zep публично оспорил и утверждает превосходство над Mem0 на 24% при пересчёте (вендор:  https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/, обсуждение:  https://www.reddit.com/r/LangChain/comments/1kg5qas/lies_damn_lies_statistics_is_mem0_really_sota_in/).
LongMemEval (arXiv 2410.10813, 2024:  https://arxiv.org/abs/2410.10813) — до 1.5M токенов истории, обновления знаний, abstention. Цифры расходятся радикально: Mem0 — 94.4% (вендор), «независимая оценка» Mem0 — 49.0%, Zep — 63.8% (GPT-4o) по версии vectorize.io ( https://vectorize.io/articles/mem0-vs-zep), другой агрегатор даёт Zep 71.2% ( https://gamgee.ai/vs/mem0-vs-zep/), Hindsight — >90% (вендор,  https://www.prnewswire.com/news-releases/vectorize-breaks-90-on-longmemeval-with-open-source-ai-agent-memory-system-302643146.html).
DMR (Deep Memory Retrieval) — бенчмарк из статьи Zep (arXiv 2501.13956), где Zep обошёл MemGPT (освещение:  https://www.marktechpost.com/2025/02/04/zep-ai-introduces-a-smarter-memory-layer-for-ai-agents-outperforming-the-memgpt-in-the-deep-memory-retrieval-dmr-benchmark/). Оговорка: DMR — «домашний» бенчмарк Zep.
Критика самих бенчмарков: зафиксированы серьёзные методологические дыры в LoCoMo/LoCoMo-Plus и LongMemEval-S ( https://www.reddit.com/r/AIMemory/comments/1s1jlnd/serious_flaws_in_two_popular_ai_memory_benchmarks/); обзор «чей это число» —  https://app.nlqdb.com/agent-memory-benchmarks/; новые попытки: BEAM (1M–10M токенов), STATE-Bench, DolphinBench (Mem0) —  https://mem0.ai/blog/ai-memory-benchmarks-in-2026.
Вывод для собеседования: каждый вендор «выигрывает» на своём бенчмарке; честная позиция — пары «accuracy + token cost» и признание, что изоляция per-user под нагрузкой и «забывание» не тестируются никем ( https://mem0.ai/blog/ai-memory-benchmarks-in-2026).
2. Общая память между агентами и вендорами (multi-agent shared memory)

Кто явно строит память, которую одновременно читают/пишут несколько агентов или несколько вендоров:

Решение
Что делает
Конфликты записи / права / provenance
OpenMemory MCP (Mem0) —  https://mem0.ai/blog/introducing-openmemory-mcp
Локальный memory-сервер, общая память между Cursor, Claude Desktop, Windsurf, Cline (записал в Cursor → достал в Claude). «Local-first, user-owned», без облака
Дашборд с per-client контролем доступа; для команд — Mem0 Gateway. Provenance/битемпоральность: нет данных
basic-memory (Basic Machines) —  https://github.com/mcp-research/basicmachines-co__basic-memory
Персистентный семантический граф в обычных Markdown-файлах на компьютере, двусторонняя синхронизация с ассистентами
Локально, «local-first»; детальных механизмов конфликтов: нет данных
Официальный MCP memory server —  https://mcp.so/servers/memory
Референсный knowledge-graph memory server от проекта MCP
JSONL-файл на одной машине — командной/федеративной памяти нет:  https://baalda.com/blog/mcp-memory-server
thesecondbrain.dev —  https://www.thesecondbrain.dev/
OSS: «Your AI tools. One shared memory» — память живёт в вашем Cloudflare-аккаунте, проекты/предпочтения/решения доступны всем подключённым ИИ-инструментам
Экспорт/редактирование/удаление, открытый код; конфликтов: нет данных
Zep Context MCP Server —  https://www.getzep.com/pricing/
Zep сам продаёт MCP-серверные места (5–15 seats в self-serve) — т.е. позиционирует Context Graph как общую память для нескольких агентов
Битемпоральность + провенанс встроены в Graphiti
Caura —  https://caura.ai/blog/zep-alternatives/
«Governed shared memory» для нескольких агентов: As-Of Recall, supersession, на PostgreSQL
Заявляет governance/конфликт-менеджмент явно (вендорская заявка)
MemoryBox (MemVerge) —  https://memverge.ai/
Личная память, локально на устройстве, переносится между AI-приложениями
«User-controlled», локально
Экосистемные статьи
Практика скрещивания Claude Code + Codex + Cursor через один MCP/Neo4j/markdown — растущий спрос, но самодельные решения:  https://www.xda-developers.com/gave-claude-code-codex-cursor-same-memory/ ,  https://towardsdatascience.com/unified-agentic-memory-across-harnesses-using-hooks/ ,  https://www.eigent.ai/blog/portable-agent-memory-across-coding-tools , тема:  https://github.com/topics/cross-agent-memory

Ключевой факт: протоколы (MCP, A2A) стандартизуют вызов инструментов и делегирование задач, но НЕ стандартизуют память — общая память остаётся отдельным слоем, который каждый решает сам ( https://www.oreilly.com/radar/designing-collaborative-multi-agent-systems-with-the-a2a-protocol/,  https://mnemoverse.com/docs/library/a2a-protocol-explained). Никто из перечисленных (кроме Zep/Graphiti и Caura) не решает: кто записал факт, что было до него, и что делать при одновременной записи противоречий.

3. Личный второй мозг для частного человека
Продукт
Что делает
«Помнит сам» или вручную
Модель/деньги
Цена
Mem (Mem.ai)
⚠️ Перестроился в «AI chief of staff» — агент трекит проекты и цели, а не просто заметки; цены убрали за логин
Автоматически (meeting recorder, агент)
Раннее: $23.5M (2022), актуальных данных нет
Нет данных (за логином):  https://intelligenttools.co/tools/mem , тарифы Free/Pro/Teams:  https://aitoolpick.org/pricing/mem-ai/
Notion AI
Заметки + AI-агенты + Enterprise Search по коннекторам (Slack, Drive и т.д.)
Гибрид
Публичная компания
AI включён в Business $20/юзер (отдельный $10 add-on убит):  https://get-alfred.ai/blog/notion-pricing ,  https://www.notion.com/help/enterprise-search
Tana
AI-native workspace на knowledge graph (supertags)
Гибрид
$25M всего, Series A $14M (Tola Capital, февр. 2025), 160k+ waitlist:  https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/
нет данных
Reflect
Заметки-граф + AI-ассистент + запись встреч, E2E-шифрование
Гибрид
Малый раунд на Wefunder:  https://wefunder.com/reflect
$10/мес:  https://checkthat.ai/brands/reflect
Heptabase
Визуальные карточки/карты для сложных тем
Вручную (AI-помощь)
~$1.2M ARR:  https://getlatka.com/companies/heptabase.com
Pro $8.99/мес:  https://www.usagepricing.com/blueprint/heptabase
Capacities
Объектные заметки («second brain»-метафора)
Вручную
нет данных
нет данных
Recall (recall.it)
Автоматически резюмирует и связывает статьи/видео/подкасты/PDF в личную базу знаний
Автоматически
⚠️ $1.5M (дек. 2024):  https://www.finsmes.com/2024/12/recall-raises-1-5m-funding.html — «€38M» из  https://www.dutchstartup.ai/en/news/recall-builds-a-personal-knowledge-base-that-summarises-and-connects-online выглядит ошибкой (путаница с Recall.ai, API для встреч, $10M Series A:  https://techcrunch.com/2024/05/16/yc-backed-recall-ai-gets-10m-series-a-to-help-companies-utilize-virtual-meeting-data/)
Free/Plus/Max:  https://www.recall.it/faq
Personal.ai
«Личный ИИ» на ваших данных, память как инфраструктура
Автоматически
~$10M:  https://profiles.crustdata.com/company/personal-ai
нет данных
Limitless
⚠️ Куплен Meta (дек. 2025), Pendant снят с продаж — «memory augmentation» закончился как независимый продукт
Автоматически (запись встреч)
Acqui-hire:  https://mlq.ai/news/meta-acquires-ai-wearables-startup-limitless-ending-sales-of-pendant-device/ ,  https://techinformed.com/meta-acquires-limitless-pendant-users-moved-to-free-unlimited-plan/
—
Khoj
OSS-«second brain» (AGPL), синхронизация с Obsidian/Notion; ⚠️ Khoj Cloud закрыт, только self-host —  https://www.promptquorum.com/fr/power-local-llm/khoj-ai-second-brain-review
Гибрид
OSS
Бесплатно (self-host)
Reor
Локальные заметки с локальным RAG (всё на устройстве)
Гибрид
OSS, маленький проект
Бесплатно:  https://reorproject.org (обзор:  https://medium.com/fongyang/meet-reor-the-private-and-local-ai-powered-note-taking-app-aad7d90c412d)
Obsidian + плагины
Локальные markdown-заметки + Smart Connections (семантические связи, локальные эмбеддинги), Copilot для чата с волтом и др.
Вручную + плагины
Частная компания, финансирование: нет данных
Бесплатно (платёно с_sync):  https://community.obsidian.md/plugins/smart-connections
Screenpipe
OSS: запись экрана и звука 24/7 → searchable «история рабочей жизни», «AI app store» на её основе
Полностью автоматически
OSS; раунд: нет данных
Бесплатно (OSS):  https://runany.dev/blog/screenpipe/
Plaud (Note-устройство)
Записывает встречи/разговоры → конспекты, память
Автоматически
⚠️ Конфликт данных: GetLatka — $100M ARR, «bootstrapped» (июль 2026):  https://getlatka.com/companies/plaud-inc ; 36kr — привлечение инвесторов, оценка $2B:  https://eu.36kr.com/en/p/3799129165863937
Устройство ~$159+
Omi (Based Hardware)
OSS-носимка ($89–129), аудио (+BCI-заявки), «persistent memory of your day»
Автоматически
Раунд: нет данных
 https://bioethics.tech/6995-2/ ,  https://www.layer3labs.io/guides/omi-ai-review
Bee
⚠️ Куплен Amazon (июль 2025), браслет/pendant, записывает день целиком
Автоматически
 https://techcrunch.com/2025/07/22/amazon-acquires-bee-the-ai-wearable-that-records-everything-you-say/ ,  https://www.theverge.com/news/711621/amazon-bee-ai-wearable-acquisition
$49.99:  https://www.layer3labs.io/gear/reviews/bee-ai-pendant

Закономерность: «ручные» second-brain'ы (Obsidian, Heptabase, Capacities, Reflect) добавляют AI постфактум; «автоматическая память» (Screenpipe, Plaud, Omi, Recall) растёт быстрее по деньгам, и почти весь этот сегмент скупают платформы (Meta ← Limitless, Amazon ← Bee). Независимых «личных мозгов» со временем всё меньше.

4. Второй мозг компании
Платформа
Что это
Граф знаний компании или поиск?
Деньги/статус
Glean
Enterprise search + AI-ассистенты + агенты на «cross-application knowledge graph» ( https://ai.engineer/orgs/glean)
Да — knowledge graph (позиционирование)
⚠️ Подтверждено: $300M ARR (май 2026, официальная страница:  https://www.glean.com/press/glean-surpasses-300m-arr-unrivaled-enterprise-context-fuels-ai-adoption); Series F $150M @ $7.2B (июнь 2025):  https://siliconangle.com/2025/06/10/glean-nabs-150m-funding-7-2b-valuation/
Microsoft 365 Copilot / Work IQ
Work IQ = «интеллект-слой» на Microsoft Graph: семантический индекс + personal memory + модель организации + APIs для чужих агентов (GA 16.06.2026, MCP)
Да — семантическая модель организации («real-time model of how your organization operates»)
Часть M365; официальное:  https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/
Atlassian Rovo / Teamwork Graph
Поиск/чат/агенты через Confluence+Jira+сторонние приложения; Teamwork Graph — 150 млрд связей, открыт агентам (Team '26)
Да — Teamwork Graph
Официальный блог:  https://www.atlassian.com/blog/company-news/teamwork-graph-team-26 ; отчёт:  https://theoutpost.ai/news-story/atlassian-opens-teamwork-graph-and-evolves-rovo-into-autonomous-agent-at-team-26-26033/
Notion Enterprise
Enterprise Search по коннекторам + агенты
Скорее поиск+документы, не граф
 https://www.notion.com/help/enterprise-search
Guru
Verified knowledge + AI-поиск + Slack MCP-интеграция (март 2026)
Не граф — верифицированная база знаний
Последний раунд $30M (2020), цены custom:  https://www.getmacha.com/blog/guru-complete-guide ,  https://techplustrends.com/guru-vs-glean-2026-enterprise-ai-knowledge-tools/
Slack AI
AI-поиск/саммари внутри Slack; ⚠️ с июня 2025 AI включён в Business+ ($12.50→$15/юзер), отдельный $10 add-on убит
Не граф — поиск по Slack
Официально:  https://slack.com/help/articles/39264531104275-Updates-to-feature-availability-and-pricing-for-Slack-plans ,  https://slack.com/blog/news/june-2025-pricing-and-packaging-announcement
Dust
«Multiplayer AI»: общие ассистенты/агенты компании на внутренних данных
Не граф — коннекторы + агенты (фокус на совместном использовании)
⚠️ $40M Series B (май 2026, Abstract + Sequoia), 3000 организаций, 300k агентов:  https://dust.tt/blog/series-b-multiplayer-ai ,  https://www.hpcwire.com/bigdatawire/this-just-in/dust-raises-40m-series-b-to-bring-multiplayer-ai-to-the-enterprise/
Onyx (ex-Danswer)
OSS enterprise search/чат по 50+ приложениям, permission layer, self-host
Не граф — федеративный индекс с правами
⚠️ Куплен Zoom (сент. 2026), остаётся OSS:  https://news.zoom.com/zoom-acquires-onyx-adding-open-source-enterprise-knowledge-search-to-zoom-ai-services/
Sana (Workday)
AI search + learning + агенты; встроена в Workday Home
Гибрид
⚠️ Куплена Workday за $1.1B (соглашение 16.09.2025, сделка закрыта):  https://investor.workday.com/news-and-events/press-releases/news-details/2025/Workday-Signs-Definitive-Agreement-to-Acquire-Sana-09-16-2025/default.aspx ,  https://www.prnewswire.com/news-releases/workday-completes-acquisition-of-sana-302603954.html
Hebbia (Matrix)
Анализ больших массивов документов (финансы/право), Matrix-интерфейс
Не граф — extraction/QA над документами
⚠️ $130M Series B @ $700M (июнь 2025):  https://aifunding.me/companies/hebbia ,  https://www.siliconrepublic.com/start-ups/hebbia-ai-new-york-funding-andreessen-horowitz
Moveworks
Employee-ассистент поверх enterprise-данных
Не граф
⚠️ Куплен ServiceNow за $2.85B (закрыт дек. 2025):  https://techcrunch.com/2025/03/10/servicenow-buys-moveworks-for-2-85b-to-grow-its-ai-portfolio/ ,  https://fourdragons.com/servicenow-moveworks-transition/
Writer
Enterprise AI-платформа: свои LLM (Palmyra) + Knowledge Graph
Да — Knowledge Graph (заявляет #1 в RAG-бенчмарке — вендор:  https://writer.com/engineering/rag-benchmark/)
$200M Series C @ $1.9B (нояб. 2024):  https://www.businesswire.com/news/home/20241112482564/en/Writer-Raises-%24200M-Series-C-at-%241.9B-Valuation-to-Fuel-Leadership-in-Agentic-Enterprise-AI

Итог сегмента: «настоящие графы компании» строят Glean, Atlassian (Teamwork Graph), Microsoft (Work IQ), Writer; остальные — умный поиск поверх документов. Всё двигается к «context engine для чужих агентов» (Work IQ APIs, Teamwork Graph для агентов, Zep Context MCP) — это прямой месседж для собеседования: Zep конкурирует не только с Mem0, но и с Graph-слоями платформ.

5. Слияние личного и корпоративного

Прямой ответ: продукта, где личная память агента сотрудника и память компании — два слоя одного графа (с правами, приватностью, владением при увольнении), на рынке НЕТ. Проверенные кандидаты и почему они не подходят:

Microsoft Work IQ — единственный с явным «personal memory» ВНУТРИ корпоративного слоя ( https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/). Но память принадлежит тенанту компании, «личного слоя», который сотрудник унес бы с собой, нет.
Claude (Team/Enterprise) memory — project-scoped и team-контекст, администратор может выключить память организации ( https://www.anthropic.com/news/memory). Личного владения при уходе нет.
Glean — knowledge graph по компании, per-user контекст, но всё в тенанте работодателя ( https://ai.engineer/orgs/glean).
Dust — «multiplayer AI»: общий контекст для команды, но нет персонального приватного слоя ( https://dust.tt/blog/series-b-multiplayer-ai).
Mem0 OpenMemory / basic-memory / thesecondbrain.dev / MemoryBox — личная память между вендорами, но БЕЗ корпоративного слоя и прав.
Односторонние решения на стыке: захват знаний уходящего сотрудника (offboarding-интервью:  https://superkind.ai/blog/offboarding-knowledge-capture), DLP-проблемы «данные компании в личном ChatGPT» ( https://jazz.security/dlp-explained/dlp-basics/company-data-in-personal-ai-accounts-when-employees-leave/), политики offboarding для AI-памяти ( https://www.adaptiverecall.com/enterprise-memory/employee-leaves.php) — все они лечат симптом, а не строят два слоя.

Спрос на проблему доказан (статьи об «employee departure problem» растут:  https://www.hugeframe.com/2026/07/06/the-employee-departure-problem-what-happens-to-company-data-already-in-their-chatgpt-account/), продукта — нет. Ближайший архитектурный кандидат — Zep: у Graphiti уже есть provenance и per-group/user-изоляция ( https://www.getzep.com/ai-agents/temporal-knowledge-graph/), т.е. «personal subgraph + company subgraph + права» — естественное расширение, которого никто не занял.

6. Сводная сравнительная таблица с Zep

а) Слой памяти агентов:

Игрок
Сегмент
Похож на Zep
Ключевые отличия от Zep
Mem0
Агенты
Memory API, managed+self-host, SOC 2, бенчмарки
Вектор-first, граф за $249/Pro; нет битемпоральности; Apache 2.0 полный self-host (у Zep self-host умер); $24M vs нет данных у Zep
Letta
Агенты
Долгая память агентов
Агент сам редактирует память (не extract-facts); векторно-файловая модель, не темпоральный граф
Cognee
Агенты/данные
Граф знаний из инжеста
Фокус на пайплайне документов (ECL), не на диалоговых фактах; полный OSS
Supermemory
Агенты/разработчики
Memory API, дешёвый вход
Плагины для кодинг-агентов; tokens-billing; слабый enterprise
Hindsight
Агенты
Time-aware память, OSS, конкурентоспособная LongMemEval-заявка
Без графовой БД (проще деплой), «mental models» вместо KG; MIT
Caura
Агенты/multi-agent
As-Of Recall, supersession, governance shared memory
PostgreSQL вместо Neo4j — дешевле деплой; молодой вендор
MemOS / MemTensor
Агенты/исследования
OSS, амбиция «memory OS»
MemCube (параметрическая+активационная память) — шире графа; китайский институциональный бэкграунд
AWS AgentCore Memory / Vertex Memory Bank
Агенты (облака)
Managed per-user память
Коммодити: привязка к своему облаку, нет битемпоральной семантики фактов; ценообразование за GB
Microsoft Copilot Memory / Work IQ
Агенты + корпорация
Memory + семантический слой + MCP
Огромное покрытие данных M365; закрытый, не для чужих вендоров вне MS-стека
OpenAI / Anthropic / Google
Потребитель
Персональная память
Бесплатно в продукте; чёрный ящик, нет API-графа, нет времени фактов
TencentDB Agent Memory
Агенты (OSS)
Многоуровневая память
MIT-пайплайн L0–L3, локальный хаб, не SaaS-граф

б) Корпоративный и личный сегменты (для полноты против Zep):

Игрок
Сегмент
Похож на Zep
Отличия
Glean
Корпорация
Граф знаний + retrieval для ассистентов
Продукт-в-продукте (поиск/ассистенты), не API-память для чужих агентов; контракты enterprise, нет OSS
Atlassian Rovo / Teamwork Graph
Корпорация
Граф как контекст-слой для агентов
Граф «работы» (Jira/Confluence), не темпоральные факты о пользователях
Writer
Корпорация
Knowledge Graph + LLM-платформа
Полный стек (свои LLM), фокус на контент-воркфлоу
Microsoft Work IQ
Корпорация
Семантический слой для агентов
Ловушка вендор-локина M365
Notion / Slack AI / Guru / Dust / Onyx / Sana
Корпорация
Поиск/знания для ассистентов
Не графы фактов (кроме позиционирования), без темпоральности
OpenMemory MCP / basic-memory / thesecondbrain.dev
Личный кросс-вендорный
Общая память для нескольких ИИ-инструментов
Локальные, без темпорального графа, конфликтов и provenance
Obsidian + плагины / Khoj / Reor
Личный
Self-hosted «мозг» + RAG
Нет управляемого факто-графа, нет SaaS
Screenpipe / Plaud / Omi
Личный (автозахват)
Память о жизни человека
Память = транскрипты/саммари, не структурированные факты с временем
Limitless (Meta) / Bee (Amazon)
Личный
Носимая память
Исчезли как независимые: поглощены платформами

Чем Zep реально уникален на этой карте (проверено по первоисточникам): битемпоральные факты (valid_at/invalid_at, противоречия помечаются) ( https://www.getzep.com/ai-agents/temporal-knowledge-graph/); встроенная продажа MCP-мест ( https://www.getzep.com/pricing/); BYOK/BYOC + SOC 2 Type II + HIPAA BAA; retrieval/users не метрируются ( https://www.getzep.com/pricing/, подтверждает  https://caura.ai/blog/zep-alternatives/). Слабости: отказ от self-hosted CE (апрель 2025) отталкивает on-prem-комьюнити к Graphiti напрямую или к Mem0/Caura; кредиты по байтам эпизода непрозрачны ( https://dev.to/statewave/zep-charges-per-350-bytes-so-the-same-15000-episodes-can-land-on-a-125-or-a-375-plan-7); нет данных о внешнем финансировании на фоне $24M у Mem0.

7. Свободные ниши (2026)
Ниша
Улики, что пуста
Ближе всех
Общая память разных ИИ-вендоров у одного человека/команды (SaaS, не самоделка)
Есть только OSS/ранние продукты (OpenMemory — локальный, thesecondbrain.dev — сам себе хостинг в Cloudflare); статьи о самодельных склейках Claude+Codex+Cursor ( https://www.xda-developers.com/gave-claude-code-codex-cursor-same-memory/); ни у одного решения нет битемпоральности и конфликтов записи
OpenMemory MCP (Mem0), basic-memory, MemoryBox (MemVerge), Caura
Второй мозг семьи
Единственный найденный продукт — крошечный FamilyBrain на WhatsApp ( https://www.familybrain.co.uk/); у больших платформ таких продуктов нет (у всех — per-user память); концептуальные статьи без продуктов:  https://medium.com/@talirezun/the-shared-brain-when-second-brains-start-thinking-together-0d2ad54413d9
FamilyBrain, shared-пространства Notion/Obsidian, Omi/Plaud на семью
Федерация личных мозгов сотрудников (личный+корпоративный граф в два слоя)
Раздел 5: продукта нет; боль зафиксирована в DLP/offboarding-статьях ( https://jazz.security/dlp-explained/dlp-basics/company-data-in-personal-ai-accounts-when-employees-leave/)
Microsoft Work IQ (personal memory в корп. слое), Zep Graphiti (архитектурно готова: provenance+изоляция), Dust (общий, но не личный слой)
Память с provenance и разрешением конфликтов для multi-agent
Сами вендоры признают, что изоляция per-user под нагрузкой и «забывание» не тестируются и мало кем решаются ( https://mem0.ai/blog/ai-memory-benchmarks-in-2026); MCP/A2A не стандартизуют память ( https://mnemoverse.com/docs/library/a2a-protocol-explained)
Zep (би-темпоральность), Caura (supersession), Graphiti OSS
Носимая/автоматическая память как независимый бизнес
Сегмент «умер» через поглощения: Limitless→Meta, Bee→Amazon (ссылки выше); независимым остаётся только Plaud (устройство, не платформа памяти)
Plaud, Omi, Screenpipe (OSS)
Чего в данных нет (и открытые вопросы)
Финансирование Zep: публичных данных о раунде не найдено (профили Tracxn/CB Insights закрыты) — на собеседовании честнее исходить из «bootstrapped/undisclosed», чем гадать.
Цена Zep Emerging ($13k/год): программа «emerging company price» на pricing-странице подтверждена, но суммы там нет ( https://www.getzep.com/pricing/) — ваша цифра из другого канала, помечайте как «по слухам/свей-данные».
Enterprise-цены Glean (контракты от $100k — не подтверждено найденными источниками; сторонний обзор даёт ~$45/юзер/мес:  https://theplanettools.ai/tools/glean — конфликт), Writer, Hebbia, Hebbia-клиенты «40% топ-менеджеров» (вендорский маркетинг).
«Dreaming» у ChatGPT — только SEO-блоги без официального подтверждения (низкая надёжность).
Точные звёзды/трафик Memobase, Graphlit, Caura, LangMem, Screenpipe, Reor; раунды Omi (Based Hardware), Screenpipe, Mem.ai (после 2022), Capacities.
Конфликт по Plaud: «bootstrapped $100M ARR» (GetLatka) vs «инвестиции, оценка $2B» (36kr) — оба источника второго уровня.
Независимые бенчмарки памяти: отсутствуют как класс; все цифры LoCoMo/LongMemEval/DMR — вендорские или оспоренные.
Реальное внедрение памяти в enterprise-продакшене (case studies с числами) — почти у всех игроков только маркетинговые кейсы.
Источники (Source Notes)

Ключевые источники по убыванию значимости. Оценка: 5/5 — официальные первоисточники; 4/5 — established СМИ; 3/5 — вендорские блоги/заинтересованные сравнения (раскрыты); 2/5 — SEO-каталоги (использованы только при недоступности первоисточника).

Источник
Надёжность
Обновлено
 https://www.getzep.com/pricing/ — официальный прайс Zep
5/5
03.11.2026 (открыто)
 https://www.getzep.com/ai-agents/temporal-knowledge-graph/ — битемпоральность по Zep
5/5
-
 https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/ — ответ Zep на бенчмарки Mem0
3/5 (вендор)
2025
 https://www.anthropic.com/news/memory — Claude memory
5/5
23.10.2025 (update)
 https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/ — Work IQ APIs
5/5
02.06.2026
 https://mc.merill.net/message/MC1492154 и  https://windowsforum.com/news/copilot-enhanced-memory-brings-work-data-personalization-in-november-2026.429482/ — Copilot Memory GA
4/5
10.2026
 https://openai.com/index/memory-and-new-controls-for-chatgpt/ — память ChatGPT
5/5
2024 (⚠️ стареет)
 https://9to5google.com/2025/08/13/gemini-personal-context/ — Personal Context
4/5
13.08.2025
 https://mem0.ai/blog/introducing-openmemory-mcp — OpenMemory MCP
5/5 (продукт)
13.05.2025 (upd. 25.08.2026)
 https://mem0.ai/blog/ai-memory-benchmarks-in-2026 — бенчмарки Mem0
3/5 (вендор)
22.09.2026
 https://www.bwdisrupt.com/article/mem0-raises-24-mn-from-basis-set-ventures-peak-xv-partners-others-577381 — раунд Mem0
3/5
10.2025
 https://techcrunch.com/2024/09/23/letta-one-of-uc-berkeleys-most-anticipated-ai-startups-has-just-come-out-of-stealth/ — Letta $10M
4/5
23.09.2024
 https://www.eu-startups.com/2026/02/german-ai-infrastructure-startup-cognee-lands-e7-5-million-to-scale-enterprise-grade-memory-technology/ — Cognee €7.5M
4/5
02.2026
 https://www.thesaasnews.com/news/supermemory-raises-3m-pre-seed/ — Supermemory $3M
3/5
10.2025
 https://vectorize.io/blog/introducing-hindsight-agent-memory-that-works-like-human-memory и  https://www.prnewswire.com/news-releases/vectorize-breaks-90-on-longmemeval-with-open-source-ai-agent-memory-system-302643146.html — Hindsight
3/5 (вендор/PR)
10.2026
 https://caura.ai/blog/zep-alternatives/ — сравнение, deprecation Zep CE, 31.2k звёзд Graphiti
3/5 (вендор, раскрыто)
05.10.2026
 https://vectorize.io/articles/mem0-vs-zep — архитектура/цены Mem0 vs Zep
3/5 (вендор, раскрыто)
10.2026
 https://wallabytoken.com/blog/p/ai-memory-pricing-compared — цены 4 API
3/5 (вендор, раскрыто)
01.10.2026
 https://arxiv.org/abs/2507.03724 — MemOS
5/5
07.2025
 https://arxiv.org/html/2502.12110v1 — A-MEM
5/5
02.2025
 https://arxiv.org/abs/2410.10813 — LongMemEval
5/5
10.2024
 https://www.marktechpost.com/2025/02/04/zep-ai-introduces-a-smarter-memory-layer-for-ai-agents-outperforming-the-memgpt-in-the-deep-memory-retrieval-dmr-benchmark/ — DMR
4/5
04.02.2025
 https://www.reddit.com/r/AIMemory/comments/1s1jlnd/serious_flaws_in_two_popular_ai_memory_benchmarks/ — критика бенчмарков
2/5
2025–2026
 https://aws-news.com/article/01a1321e-45d4-3328-617d-4d18d2b276ba — pricing AgentCore Memory
4/5
2026
 https://discuss.google.dev/t/lets-talk-about-vertex-ai-memory-bank-heres-a-quick-faq-on-the-top-5-questions-weve-seen/244685 — Vertex Memory Bank
5/5 (офиц. форум)
2025
 https://www.marktechpost.com/2026/05/23/tencent-open-sources-tencentdb-agent-memory-a-4-tier-local-memory-pipeline-for-ai-agents/ — Tencent
4/5
23.05.2026
 https://www.glean.com/press/glean-surpasses-300m-arr-unrivaled-enterprise-context-fuels-ai-adoption +  https://siliconangle.com/2025/06/10/glean-nabs-150m-funding-7-2b-valuation/ — Glean
5/5 + 4/5
2026 / 10.06.2025
 https://www.atlassian.com/blog/company-news/teamwork-graph-team-26 +  https://theoutpost.ai/news-story/atlassian-opens-teamwork-graph-and-evolves-rovo-into-autonomous-agent-at-team-26-26033/ — Teamwork Graph
5/5 + 4/5
2026
 https://dust.tt/blog/series-b-multiplayer-ai +  https://www.hpcwire.com/bigdatawire/this-just-in/dust-raises-40m-series-b-to-bring-multiplayer-ai-to-the-enterprise/ — Dust
5/5 + 4/5
18.05.2026
 https://news.zoom.com/zoom-acquires-onyx-adding-open-source-enterprise-knowledge-search-to-zoom-ai-services/ — Zoom↔Onyx
5/5
09.2026
 https://investor.workday.com/news-and-events/press-releases/news-details/2025/Workday-Signs-Definitive-Agreement-to-Acquire-Sana-09-16-2025/default.aspx — Workday↔Sana
5/5
16.09.2025
 https://techcrunch.com/2025/03/10/servicenow-buys-moveworks-for-2-85b-to-grow-its-ai-portfolio/ — ServiceNow↔Moveworks
4/5
10.03.2025
 https://www.businesswire.com/news/home/20241112482564/en/Writer-Raises-%24200M-Series-C-at-%241.9B-Valuation-to-Fuel-Leadership-in-Agentic-Enterprise-AI — Writer
5/5
12.11.2024
 https://aifunding.me/companies/hebbia — Hebbia $130M
2/5
06.2025
 https://mlq.ai/news/meta-acquires-ai-wearables-startup-limitless-ending-sales-of-pendant-device/ — Meta↔Limitless
3/5
12.2025
 https://techcrunch.com/2025/07/22/amazon-acquires-bee-the-ai-wearable-that-records-everything-you-say/ — Amazon↔Bee
4/5
22.07.2025
 https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/ — Tana
4/5
03.02.2025
 https://www.finsmes.com/2024/12/recall-raises-1-5m-funding.html — Recall
4/5
12.2024
 https://slack.com/help/articles/39264531104275-Updates-to-feature-availability-and-pricing-for-Slack-plans — Slack AI
5/5
06.2025
 https://www.getmacha.com/blog/guru-complete-guide — Guru
2/5
2026
 https://github.com/mcp-research/basicmachines-co__basic-memory — basic-memory
4/5
-
 https://www.thesecondbrain.dev/ — Second Brain (Cloudflare)
3/5 (сайт продукта)
2026
 https://www.familybrain.co.uk/ — FamilyBrain
2/5
2026
 https://jazz.security/dlp-explained/dlp-basics/company-data-in-personal-ai-accounts-when-employees-leave/ — offboarding-проблема
2/5
2026
 https://code.claude.com/docs/en/memory — Claude Code memory
5/5
2026
 https://www.oreilly.com/radar/designing-collaborative-multi-agent-systems-with-the-a2a-protocol/ — A2A
4/5
2025
Три вывода, которые меняют действия

1. Для собеседования с Zep — говорить надо не про «конкурентов-память», а про сдвиг рынка. Реальная угроза Zep — не Mem0 (векторный подход, слабее на темпоральности), а два процесса: (а) big tech встроил память бесплатно (Copilot Memory GA ноябрь 2026, Claude memory, Gemini Personal Context) и убивает сценарий «простая персонализация», (б) графовые context-слои платформ (Work IQ, Teamwork Graph, Glean) забирают enterprise-контекст. Уникальный актив Zep — битемпоральный Graphiti + продажа Context MCP-мест + BYOK/BYOC. Сильные вопросы, которые стоит задать CEO: как Zep отвечает на отказ от self-hosted CE (комьюнити уходит в raw Graphiti/Mem0), как выигрывает у Work IQ в enterprise, и признаёт ли, что все текущие бенчмарки (включая их собственный DMR) — вендорские.

2. Для вашего продукта: уникально и неуникально. НЕ уникально: графовый RAG над личными заметками (это делают Obsidian-плагины, Khoj, Reor, Tana, Capacities — перегретый сегмент), и «память агента как API» (15+ вендоров). УНИКАЛЬНО (проверено по рынку): (а) битемпоральная память с provenance в кросс-вендорном сценарии — у одного человека/команды Claude+Codex+Grok на общей памяти; сегодня это умеют только самодельные склейки и ранние OSS без конфликтов записи и времени фактов; (б) федерация «личный мозг + общий мозг команды/компании» в двух слоях одного графа — этого не делает никто, включая Microsoft. Ваш Obsidian+SQLite+8 машин — правильный локальный фундамент для обеих ниш; вряд ли стоит позиционировать продукт как «ещё один memory API».

3. Ниши, куда не надо: и куда надо. Не надо строить «носовимую память» (поглощены Meta/Amazon, платформы съедают), «личные заметки с AI» (коммодити, Notion за $20 всё включает) и «enterprise search» (Glean/Work IQ/Rovo с их графами — уже там, сверху консолидация ServiceNow/Zoom/Workday). Надо: общая память нескольких вендоров с темпоральностью и правами (зарождающаяся конкуренция: OpenMemory, Caura, Hindsight — можно успеть первыми в SaaS-качестве) и двухслойная федерация личных мозгов (пусто полностью; ближайший архитектурный соперник — сам Zep, что стоит учесть: ваш продукт логично строить на Graphiti-совместимой модели, а не против неё).

Sources
10:14am

## SOURCES (links in page)
https://vectorize.io/articles/zep-alternatives
https://fountaincity.tech/resources/blog/agent-memory-knowledge-systems-compared/
https://www.contextstudios.ai/guides/best-ai-agent-memory-tools-2026
https://caura.ai/blog/zep-alternatives/
https://www.braintrust.dev/articles/best-ai-agent-memory-tools-2026
https://india.entrepreneur.com/news-and-trends/mem0-raises-usd-24-mn-to-strengthen-ai-memory-infrastructure/498867
https://www.bwdisrupt.com/article/mem0-raises-24-mn-from-basis-set-ventures-peak-xv-partners-others-577381
https://thetechportal.com/2025/10/29/mem0-raises-24mn-to-build-memory-layer-for-forgetful-ai-agents/
https://startupwired.com/2025/10/29/mem0-raises-24-million-series-a-to-build-the-memory-layer/
https://chennaipatrika.com/post/mem0-raises-24m-series-a-to-build-memory-layer-for-ai-agents
https://aiwiki.ai/wiki/letta
https://www.openaitoolshub.org/ai-product-research/letta
https://yespress.io/letta
https://theaiengineer.substack.com/p/cognee-vs-zep-vs-mem0-vs-letta
https://zylos.ai/research/2026-04-05-ai-agent-memory-architectures-persistent-knowledge/
https://hyperight.com/cognee-seed-funding-ai-memory-technology/
https://rywalker.com/research/cognee
https://barndoor.ai/ai-tools/cognee-ai/
https://arcadedb.com/blog/cognee-arcadedb-ai-memory-meets-multi-model/
https://hydradb.com/blog/cognee-reviews
https://www.thesaasnews.com/news/supermemory-raises-3m-pre-seed/
https://x.com/supermemory/status/1975246785905095095
https://techkv.com/supermemory-ai-memory-api-funding/
https://www.startuphub.ai/startups/supermemory
https://susaventures.substack.com/p/our-investment-in-supermemory
https://rywalker.com/research/langmem
https://medium.com/@astropomeai/langmem-long-term-memory-for-ai-agents-366d7256ddce
https://www.everydev.ai/tools/langmem
https://www.agentpatternscatalog.org/landing/compositions/langmem/
https://blockchain.news/news/langchain-introduces-langmem-sdk-enhanced-ai-memory-management
https://arxiv.org/abs/2507.03724
https://huggingface.co/papers/2507.03724
https://memorypapers.org/papers/memos-memory-os-ai
https://pith.science/paper/2505.22101
https://pith.science/paper/2507.03724
https://dev.co/ai/rag/memobase
https://ai-tldr.dev/tools/memobase/
https://artifacthub.io/packages/helm/memobase/memobase
https://www.threads.com/@alphasignal.ai/post/DJ86e-hhb4t/you-can-now-give-your-ai-apps-long-term-memory-with-memobase-no-embedding-needed
https://www.i-programmer.info/news/105-artificial-intelligence/18295-memobase-add-memory-to-your-ai-agents.html
https://github.com/graphlit/graphlit
https://agentery.com/agents/graphlit
https://store.hypergpt.ai/graphlit/review
https://callin.io/ai-tools/graphlit/
https://pitchbook.com/profiles/company/467636-05
https://arxiv.org/html/2502.12110v1
https://www.alphaxiv.org/overview/2502.12110
https://huggingface.co/papers/2502.12110
https://memorypapers.org/papers/a-mem-agentic-memory-for-llm-agents
https://eoscontinuum.com/nodes/references/a-mem-agentic-memory-xu-et-al-2025/
https://www.developersdigest.tech/agent-memory/aws-agentcore-memory
https://devopsity.com/blog/amazon-bedrock-agentcore-architecture-components-and-pricing/
https://aws-news.com/article/01a1321e-45d4-3328-617d-4d18d2b276ba
https://binarybits.co/blog/custom-ai-agent-with-memory
https://www.pump.co/blog/amazon-bedrock-agentcore/
https://www.betterclaw.io/blog/google-vertex-ai-agent-builder
https://dreaming.press/stack/vertex-ai-memory-bank
https://uibakery.io/blog/vertex-ai-agent-builder
https://www.cloudzero.com/blog/google-vertex-ai-pricing/
https://discuss.google.dev/t/lets-talk-about-vertex-ai-memory-bank-heres-a-quick-faq-on-the-top-5-questions-weve-seen/244685
https://www.digitalapplied.com/blog/chatgpt-memory-dreaming-v3-openai-2026-guide
https://cicero.studio/en/blog/chatgpt-persistent-memory-personalization-ai-visibility-2026/
https://medium.com/@glasier067/chatgpt-just-got-a-memory-upgrade-heres-why-it-matters-8fe84de9cc8b
https://suprmind.ai/hub/chatgpt/features/
https://www.ai-toolbox.co/chatgpt-management-and-productivity/how-to-manage-chatgpt-memory-2026
https://www.leoniemonigatti.com/blog/claude-memory-tool.html
https://pure-neo.io/ai-news/anthropic-adds-memory-features-to-claude-for-team-and-enterprise-users
https://www.anthropic.com/news/memory
https://www.macrumors.com/2025/10/23/anthropic-automatic-memory-claude/
https://www.techzine.eu/news/analytics/135728/anthropic-expands-claudes-memory-for-paid-users/
https://www.aiplusinfo.com/blog/googles-gemini-ai-introduces-memory-feature/
https://secret-chat.ai/blog/ai-memory-privacy-time-bomb/
https://www.bluehost.com/blog/google-gemini-memory-feature/
https://vertu.com/lifestyle/google-geminis-personal-context-is-unhinged
https://www.igeeksblog.com/google-gemini-memory-temporary-chats-update/
https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/
https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/02/announcing-the-new-work-iq-apis/
https://windowsforum.com/news/microsoft-work-iq-the-ai-context-infrastructure-behind-m365-copilot.419212/
https://blog.admin365.ai/work-iq-in-microsoft-365/
https://blogs.perficient.com/introducing-microsoft-work-iq-the-intelligence-layer-for-agents/
https://www.everydev.ai/tools/hindsight
https://ailect.com/a-beginners-guide-to-hindsight-memory-for-ai-agents/
https://ai-tldr.dev/tools/hindsight/
https://www.stork.ai/blog/ai-agents-finally-have-a-brain
https://explainx.ai/blog/hindsight-agent-memory-system-vectorize-2026
https://www.prnewswire.com/news-releases/memverge-expands-memmachine-ecosystem-and-ai-memory-system-capabilities-302642918.html
https://www.prnewswire.com/news-releases/memverge-launches-memmachine-worlds-most-powerful-ai-memory-layer-302563813.html
https://yespress.io/memverge
https://memverge.ai/
https://mem0.ai/blog/introducing-openmemory-mcp
https://www.cometapi.com/mem0-releases-openmemory-mcp/
https://www.theunwindai.com/p/openmemory-mcp-that-works-across-ai-tools
https://huggingface.co/blog/lynn-mikami/open-memory-mcp-server
https://levelup.gitconnected.com/how-to-make-your-clients-more-context-aware-with-openmemory-mcp-60057bcc24a3?gi=46c0024152b7
https://www.tooljunction.io/mcp/basic-memory
https://explainx.ai/mcp-servers/basic-memory
https://github.com/mcp-research/basicmachines-co__basic-memory
https://mcptoplist.com/server/io.github.basicmachines-co%2Fbasic-memory
https://skywork.ai/skypage/en/MCP-Server-Basic-Memory-An-Essential-Guide-for-AI-Engineers/1971049929135681536
https://mcp.so/servers/memory
https://github.com/okooo5km/memory-mcp-server
https://mcp.directory/servers/knowledge-graph-memory
https://baalda.com/blog/mcp-memory-server
https://chatforest.com/reviews/memory-mcp-server/
https://www.xda-developers.com/gave-claude-code-codex-cursor-same-memory/
https://towardsdatascience.com/unified-agentic-memory-across-harnesses-using-hooks/
https://dev.to/icegaming/why-ai-coding-agents-keep-redoing-each-others-work-markdown-memory-vs-real-memory-in-cursor-and-14lj
https://www.eigent.ai/blog/portable-agent-memory-across-coding-tools
https://github.com/topics/cross-agent-memory
https://mnemoverse.com/docs/library/shared-memory-for-multi-agent-systems
https://www.oreilly.com/radar/designing-collaborative-multi-agent-systems-with-the-a2a-protocol/
https://mnemoverse.com/docs/library/a2a-protocol-explained
https://sivaro.in/articles/a2a-protocol-for-multi-agent-systems-the-missing-layer-in/
https://www.360digitaltransformation.com/blogs/guides/a2a-protocol-explained-in-2026-how-agent2agent-works-mcp-vs-a2a-and-your-first-multi-agent-build
https://memverge.ai/blog/starting-with-why-memorybox
https://app.dealroom.co/news/feed/memorybox-ai-launches-beta-of-private-ai-memory-tool-for-power-users
https://aitech365.com/automation-in-ai/memverge-launches-memorybox-to-give-users-control-of-ai-context/
https://uk.advfn.com/stock-market/share-news/MemoryBox-ai-Announces-Beta-Launch-of-Private-AI-M/99066230
https://intelligenttools.co/tools/mem
https://vhiz.ai/tools/mem/
https://toolszai.com/tool/mem-ai-review-2026/
https://www.solidaitech.com/2026/07/mem-ai-note-taking-app-vs-mem0-api.html
https://awesomeagents.ai/tools/notion-ai-vs-mem-ai-2026/
https://www.notion.com/help/notion-ai-connectors
https://www.notion.com/connections
https://notionflows.com/blog-post/notion-ai-for-work
https://www.eesel.ai/blog/notion-ai-review
https://www.notion.com/help/enterprise-search
https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/
https://aimmediahouse.com/retail/tana-raises-25-million-to-solve-productivity-chaos-with-an-ai-powered-knowledge-graph
https://mischadohler.com/tanas-intelligent-knowledge-graph-open-ai-driven-leap-in-work-productivity-with-25m-investment/
https://aadhunik.ai/blog/tans-expands-ai-powered-knowledge-graph/
https://app.dealroom.co/news/feed/tana-secures-25m-for-ai-workspace
https://wefunder.com/reflect
https://ai.miraheze.org/wiki/Reflect_(software)
https://checkthat.ai/brands/reflect
https://www.billiondollarpitchdecks.com/startups/reflect
https://startupintros.com/orgs/heptabase
https://pitchbook.com/profiles/company/493308-01
https://getlatka.com/companies/heptabase.com
https://www.reddit.com/r/IAmA/comments/17colc2/i_am_alan_chan_cofounder_of_heptabase_a_visual/
https://www.usagepricing.com/blueprint/heptabase
https://www.saner.ai/blogs/10-best-second-brain-ai-apps
https://buildin.ai/blog/best-second-brain-apps-2026
https://www.recall.it/compare/best-second-brain-apps
https://www.taskade.com/blog/ai-second-brain-tools
https://www.iwoszapar.com/p/best-ai-second-brain-solutions
https://www.dutchstartup.ai/en/news/recall-builds-a-personal-knowledge-base-that-summarises-and-connects-online
https://nubiapage.com/recall-ai/
https://www.recall.it/faq
https://www.recall.it/best-knowledge-base-software
https://startupintros.com/orgs/recall-ai
https://pitchbook.com/profiles/company/439218-46
https://tracxn.com/d/companies/personal-ai/__km3jbW0uSOjjopNTO7_osOJmYRwGU7-zqIKWilUz1co
https://startupintros.com/orgs/personal-ai
https://profiles.crustdata.com/company/personal-ai
https://www.cbinsights.com/company/personal-ai
https://mlq.ai/news/meta-acquires-ai-wearables-startup-limitless-ending-sales-of-pendant-device/
https://mrktcorrect.com/blog/limitless-pendant
https://becomefluent.io/blog/2026-05/what-happened-to-limitless-pendant/
https://techinformed.com/meta-acquires-limitless-pendant-users-moved-to-free-unlimited-plan/
https://www.alphaspread.com/market-news/mergers-acquisitions/meta-acquires-ai-wearables-startup-limitless
https://www.i-scoop.eu/khoj/
https://dswharshit.medium.com/build-your-second-brain-with-khoj-ai-high-signal-ai-2-87492730d7ce
https://genai.works/applications/khoj-lvgv2d2t
https://medevel.com/khoj-app/
https://www.promptquorum.com/fr/power-local-llm/khoj-ai-second-brain-review
https://fongyang.medium.com/meet-reor-the-private-and-local-ai-powered-note-taking-app-aad7d90c412d
https://reor.framer.ai/
https://korben.info/en/reor-ai-powered-local-note-taking.html
https://gigazine.net/gsc_news/en/20240421-reor/
https://www.toolify.ai/ai-news/reor-ai-the-best-local-notetaking-app-for-privacy-3482279
https://www.billiondollarpitchdecks.com/startups/screenpipe
https://runany.dev/blog/screenpipe/
https://sourceforge.net/projects/screenpipe.mirror/
https://www.everydev.ai/tools/screenpipe
https://aiagentstore.ai/ai-agent/screenpipe
https://sacra.com/c/plaud/
https://www.caplight.com/company/plaud
https://getlatka.com/companies/plaud-inc
https://tracxn.com/d/companies/plaudai/__OWAs0w4-y7_mlueJEMT-U-Ho43zTuM5DMK0-A6PO8Jc
https://prospeo.io/c/plaud-revenue
https://cossmology.com/organizations/omi
https://www.omi.me/products/omi-glass-dev-kit
https://www.startupecosystem.ca/news/san-francisco-startup-based-hardware-unveils-omi-an-ai-wearable-for-productivity/
https://bioethics.tech/6995-2/
https://sourceforge.net/projects/omi-ai.mirror/
https://techcrunch.com/2025/07/22/amazon-acquires-bee-the-ai-wearable-that-records-everything-you-say/
https://www.forbes.com/sites/timbajarin/2025/07/25/amazons-bold-move-into-wearable-ai-the-bee-acquisition/
https://www.layer3labs.io/gear/reviews/bee-ai-pendant
https://www.theverge.com/news/711621/amazon-bee-ai-wearable-acquisition
https://valueaddvc.com/company/glean
https://www.teahose.com/guides/glean-valuation
https://www.kucoin.com/news/flash/glean-hits-300m-arr-amid-ai-budget-cuts-valued-at-7-2b
https://www.glean.com/press/glean-surpasses-300m-arr-unrivaled-enterprise-context-fuels-ai-adoption
https://forgeglobal.com/insights/how-to-invest-in-glean-stock-pre-ipo/
https://support.atlassian.com/rovo/docs/knowledge-sources-for-agents/
https://www.positioniseverything.net/atlassian-introduces-rovo-an-ai-powered-knowledge-discovery-tool-for-the-enterprise/
https://blog.deiser.com/en/what-is-rovo-atlassian-ai-rovo-agent-smarter-finances-jira
https://community.atlassian.com/forums/Atlassian-AI-Rovo-articles/Unlock-Enterprise-Knowledge-with-Atlassian-Rovo/ba-p/3010072
https://www.tryprotege.com/blog/atlassian-rovo-complete-guide
https://techplustrends.com/guru-vs-glean-2026-enterprise-ai-knowledge-tools/
https://perfectwikiforteams.com/blog/top-ai-knowledge-agents-for-slack/
https://aiproductivity.ai/tools/guru/
https://www.questionbase.com/resources/blog/best-slack-ai-integrations-scaling-companies
https://slack.com/blog/productivity/ai-enterprise-search-top-features-and-tools-in-2025
https://dealroom.co/companies/dust-1/
https://daily.dev/posts/dust-grabs-another-16-million-for-its-enterprise-ai-assistants-connected-to-internal-data-uxuohpoji
https://app.dealroom.co/news/feed/dust-raises-16m-for-enterprise-ai-assistants
https://fast.io/resources/dust-ai-workspace/
https://techfundingnews.com/openai-alumni-grabs-16m-to-develop-enterprise-ai-assistants/
https://www.teamazing.com/blog/onyx-ai-enterprise-review-2026/
https://www.webpronews.com/zoom-acquires-onyx-open-source-enterprise-search-ai-services
https://bitcoinworld.co.in/onyx-open-source-search-solution/
https://faun.dev/toolbox/onyx/
https://computerweekly.com/feature/Workday-Rising-Humans-and-AI-must-labour-cheek-by-jowl
https://www.orrick.com/en/News/2025/09/Workday-to-Acquire-Sana-for-1-Billion
https://www.prnewswire.com/news-releases/workday-completes-acquisition-of-sana-302603954.html
https://investor.workday.com/news-and-events/press-releases/news-details/2025/Workday-Signs-Definitive-Agreement-to-Acquire-Sana-09-16-2025/default.aspx
https://www.cio.com/article/4146511/workday-integrates-sana-to-turn-its-enterprise-apps-into-agentic-execution-engines.html
https://startupintros.com/orgs/hebbia
https://dynamicbusiness.com/ai-tools/hebbia-revolutionizes-ai-interface-meet-the-matrix.html
https://vantaige.io/ai-tool/hebbia
https://www.ai-market-watch.com/news/company/hebbia
https://fourdragons.com/servicenow-moveworks-transition/
https://techcrunch.com/2025/03/10/servicenow-buys-moveworks-for-2-85b-to-grow-its-ai-portfolio/
https://www.everestgrp.com/blog/servicenows-2-85b-moveworks-acquisition-evaluating-the-strategic-impact-blog.html
https://www.getmacha.com/blog/moveworks-vs-servicenow
https://www.sdxcentral.com/news/servicenow-to-acquire-moveworks-for-285-billion/
https://hokai.io/hub/tools/writer-enterprise-ai-platform-for-agentic-work
https://ai.engineer/orgs/writer
https://www.businesswire.com/news/home/20241112482564/en/Writer-Raises-%24200M-Series-C-at-%241.9B-Valuation-to-Fuel-Leadership-in-Agentic-Enterprise-AI
https://siliconangle.com/2023/05/11/writer-introduces-new-content-features-ai-writing-platform/
https://writer.com/blog/series-b-funding-writer/
https://www.reddit.com/r/AIMemory/comments/1s1jlnd/serious_flaws_in_two_popular_ai_memory_benchmarks/
https://www.reddit.com/r/LangChain/comments/1kg5qas/lies_damn_lies_statistics_is_mem0_really_sota_in/
https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/
https://mem0.ai/blog/ai-memory-benchmarks-in-2026
https://penfieldlabs.substack.com/p/proposal-a-new-benchmark-for-long
https://arxiv.org/abs/2410.10813
https://arxiv.org/html/2410.10813v1
https://openreview.net/forum?id=pZiyCaVuti
https://www.alphaxiv.org/abs/2410.10813
https://www.emergentmind.com/topics/longmemeval
https://www.therundown.ai/tools/zep
https://hydradb.com/blog/zep-reviews
https://www.fixedlabs.ai/tools/zep
https://www.beri.net/tools/zep
https://theaiagentindex.com/agents/zep
https://tracxn.com/d/companies/zep/__poSadJnSfLWHjz05Xi3U5KwnpCMWSU3aDrihLX_8FLs
https://startupintros.com/orgs/zep-ai
https://aiinframap.com/companies/zep
https://www.cbinsights.com/company/zep-2
https://vectorize.io/articles/mem0-vs-zep
https://gamgee.ai/vs/mem0-vs-zep/
https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3
https://aicoolies.com/comparisons/mem0-vs-zep
https://dev.to/varun_pratapbhardwaj_b13/5-ai-agent-memory-systems-compared-mem0-zep-letta-supermemory-superlocalmemory-2026-benchmark-59p3
https://www.marktechpost.com/2025/02/04/zep-ai-introduces-a-smarter-memory-layer-for-ai-agents-outperforming-the-memgpt-in-the-deep-memory-retrieval-dmr-benchmark/
https://app.nlqdb.com/agent-memory-benchmarks/
https://x.com/svpino/status/1891488282040750344
https://www.reddit.com/r/machinelearningnews/comments/1ihobxz/zep_ai_introduces_a_smarter_memory_layer_for_ai/
https://aiagentmemory.org/articles/llm-memory-evaluation/
https://www.marktechpost.com/2024/03/27/meet-zep-an-ai-research-startup-adding-long-term-memory-to-your-ai-assistant/
https://www.finsmes.com/2024/12/recall-raises-1-5m-funding.html
https://www.recall.ai/blog/announcing-our-10m-series-a
https://techcrunch.com/2024/05/16/yc-backed-recall-ai-gets-10m-series-a-to-help-companies-utilize-virtual-meeting-data/
https://dealroom.co/companies/memory-tensor-technology/
https://huggingface.co/blog/MemTensor/agent-memory-is-not-rag
https://x.com/Marktechpost/status/1933893662720541182
https://arxiv.org/pdf/2507.03724
https://arxiv.org/html/2505.22101v1
https://www.thesecondbrain.dev/
https://www.familybrain.co.uk/
https://www.sentra.app/articles/ai-second-brain
https://medium.com/@talirezun/the-shared-brain-when-second-brains-start-thinking-together-0d2ad54413d9
https://secondbrainmemory.com/
https://www.adaptiverecall.com/enterprise-memory/employee-leaves.php
https://playgram.ai/ai-for-teams/ai-context-and-access-after-an-employee-leaves
https://superkind.ai/blog/offboarding-knowledge-capture
https://jazz.security/dlp-explained/dlp-basics/company-data-in-personal-ai-accounts-when-employees-leave/
https://www.hugeframe.com/2026/07/06/the-employee-departure-problem-what-happens-to-company-data-already-in-their-chatgpt-account/
https://www.marktechpost.com/2026/05/23/tencent-open-sources-tencentdb-agent-memory-a-4-tier-local-memory-pipeline-for-ai-agents/
https://www.ai-all.info/en/ai-models/tencentdb-agent-memory
https://www.skakarh.com/blog/tencentdb-agent-memory
https://toknow.ai/posts/tencentdb-agent-memory-local-memory-hub-agents/
https://www.skakarh.com/blog/tencentdb-agent-memory-sdk
https://www.getzep.com/pricing
https://www.getzep.com/pricing/
https://dev.to/statewave/zep-charges-per-350-bytes-so-the-same-15000-episodes-can-land-on-a-125-or-a-375-plan-7
https://tomrochette.com/agents/memory/zep/
https://zep.tenereteam.com/
https://www.promptquorum.com/power-local-llm/letta-review
https://techcrunch.com/2024/09/23/letta-one-of-uc-berkeleys-most-anticipated-ai-startups-has-just-come-out-of-stealth/
https://www.felicis.com/blog/letta
https://theaiinsider.tech/2024/09/26/berkeley-ai-research-lab-spinout-letta-raises-10m-seed-financing-led-by-felicis-to-build-ai-with-memory/
https://techedgeai.com/news/letta-emerges-from-stealth-with-10m-seed-round-to-revolutionize-ai-memory/
https://supermemory.ai/docs/overview/billing
https://www.contextely.com/blog/supermemory-review-2026
https://hjarni.com/blog/mem0-vs-supermemory-vs-hjarni
https://comprehensive-elements-633758.framer.app/pricing
https://wallabytoken.com/blog/p/ai-memory-pricing-compared
https://www.eu-startups.com/2026/02/german-ai-infrastructure-startup-cognee-lands-e7-5-million-to-scale-enterprise-grade-memory-technology/
https://fundup.ai/recently-funded-startups/company/c4K0FAPaJsI7o6ucJUad/cognee
https://parsers.vc/news/260226-cognee-secures-7-5m-to-revolutionize-ai/
https://www.scalablepod.it/en/startup/cognee/
https://en.wikipedia.org/wiki/Pre-money_valuation
https://en.wikipedia.org/wiki/Valuation_(algebra)
https://en.wikipedia.org/wiki/Valuation_(finance)
https://en.wikipedia.org/wiki/Plaid_Inc.
https://eu.36kr.com/en/p/3799129165863937
https://en.wikipedia.org/wiki/Glean_Technologies
https://en.wikipedia.org/wiki/Mount_Wellington_(Tasmania)
https://en.wikipedia.org/wiki/Wellington_Zoo
https://www.reworked.co/digital-workplace/glean-secures-150m-series-f-at-72b-valuation-for-ai-growth/
https://siliconangle.com/2025/06/10/glean-nabs-150m-funding-7-2b-valuation/
https://openclaw.direct/slack/slack-ai-pricing-alternative
https://viktor.com/blog/best-ai-agents-for-slack
https://skyp.ai/tools/best-ai-agents-for-slack
https://www.findarticles.com/best-agents-that-work-inside-slack/
https://usemerlin.ai/blog/best-ai-agents-for-slack
https://www.getmacha.com/blog/guru-complete-guide
https://costbench.com/software/knowledge-management/guru/
https://www.toolsforhumans.ai/ai-tools/guru
https://www.docsie.io/blog/articles/confluence-vs-guru-pricing-comparison-2026/
https://findbestkb.com/en/guru-review/
https://get-alfred.ai/blog/notion-pricing
https://www.breeze.pm/articles/notion-ai-pricing
https://fast.io/resources/notion-ai-review-2026/
https://dancumberlandlabs.com/blog/notion-ai-guide/
https://lifestack.ai/blog/notion-pricing
https://community.obsidian.md/plugins/smart-connections
https://www.obsidianstats.com/plugins/smart-connections
https://www.shadow.do/blog/best-ai-plugins-for-obsidian-2026
https://deepwiki.com/brianpetro/obsidian-smart-connections
https://www.everydev.ai/tools/smart-connections
https://note.com/gen_ai/n/n558985ff153e?hl=en
https://www.siliconrepublic.com/start-ups/hebbia-ai-new-york-funding-andreessen-horowitz
https://medium.com/@joycebirkins/4-enterprise-ai-startups-raise-100m-hebbia-glean-sierra-and-mercor-revolutionize-corporate-33c37e1062b9
https://aifunding.me/companies/hebbia
https://tracxn.com/d/companies/hebbia/__QVC5BFqOGZ0XKXpKNqSBMDNtp5MbZKx6B_v8_KNqpVE
https://news.zoom.com/zoom-acquires-onyx-adding-open-source-enterprise-knowledge-search-to-zoom-ai-services/
https://sandpczone.com/zoom-onyx-acquisition
https://www.remio.ai/post/zoom-onyx-acquisition-targets-a-gap-cloud-first-ai-cannot-ignore
https://news.zoom.com/
https://itchannel.info/articles/211074/zoom-acquiert-onyx.html
https://openai.com/index/memory-and-new-controls-for-chatgpt/
https://x.com/OpenAI/status/1929937841905381558
https://help.openai.com/en/articles/6825453-chatgpt-release-notes
https://help.openai.com/en/articles/8983148-does-memory-function-with-gpts
https://help.openai.com/en/articles/8914046-temporary-chat-faq
https://mc.merill.net/message/MC1492154
https://windowsforum.com/news/copilot-enhanced-memory-brings-work-data-personalization-in-november-2026.429482/
https://windowsnews.ai/article/microsoft-365-copilot-to-gain-memory-based-personalization-in-november-2026.429482
https://m365admin.handsontek.net/microsoft-365-copilot-chat-view-memory-sources-used-personalize-responses/
https://msmessagecenter.com/MC1158329
https://9to5google.com/2025/08/13/gemini-personal-context/
https://www.bgr.com/1938873/google-gemini-personalization-temporary-chats-privacy/
https://arstechnica.com/ai/2025/08/google-gemini-will-now-learn-from-your-chats-unless-you-tell-it-not-to/
https://www.engadget.com/ai/gemini-can-now-pull-context-the-rest-of-your-google-apps-if-you-let-it-160039468.html
https://www.croma.com/unboxed/gemini-personal-intelligence-upgrade-introduced
https://github.com/mem0ai/mem0
https://www.semanticscholar.org/paper/Mem0:-Building-Production-Ready-AI-Agents-with-Chhikara-Khant/1d9c21a0fdb1cc16a32c5d490ebaf98436a23382
https://medium.com/@EleventhHourEnthusiast/mem0-building-production-ready-ai-agents-with-scalable-long-term-memory-9c534cd39264
https://mem0.ai/blog/state-of-ai-agent-memory-2026
https://github.com/Snseam/awesome-agent-memory/blob/main/papers/mem0-paper.md
https://www.aibase.com/news/14588
https://www.layer3labs.io/guides/omi-ai-review
https://interestingengineering.com/ces-2025/omi-brain-reading-wearable
https://app.dealroom.co/companies/omi_1
https://dust.tt/blog/series-b-multiplayer-ai
https://www.sovereignmagazine.com/article/dust-ai-40m-series-b-enterprise-platform
https://www.research-live.com/article/news/dust-series-b-funding-round-raises-40m/id/5149747
https://www.hpcwire.com/bigdatawire/this-just-in/dust-raises-40m-series-b-to-bring-multiplayer-ai-to-the-enterprise/
https://www.mapco.ai/company/dust
https://slack.com/help/articles/39264531104275-Updates-to-feature-availability-and-pricing-for-Slack-plans
https://www.subscriptionplaybook.com/case-studies/slack-ai-pricing/
https://slack.com/blog/news/june-2025-pricing-and-packaging-announcement
https://slack.com/intl/en-gb/help/articles/39264531104275-Updates-to-feature-availability-and-pricing-for-Slack-subscriptions
https://code.claude.com/docs/en/memory
https://github.com/luongnv89/claude-howto/blob/main/02-memory/README.md
https://vectorize.io/articles/claude-code-memory
https://ivgraph.com/journal/claude-code-memory/
https://institute.sfeir.com/en/claude-code/claude-code-memory-system-claude-md/
https://blog.memoryplugin.com/cursor-memory/
https://www.cognee.ai/cursor-memory
https://recallium.ai/cursor
https://www.memorylake.ai/en/blogs/guard-cursor-automations-memories
https://hireseopro.com/blogs/does-cursor-ai-track-memory-across-conversations.html
https://github.com/getzep/graphiti
https://www.getzep.com/ai-agents/temporal-knowledge-graph/
https://github.com/q-qp-p/getzep-graphiti
https://www.reddit.com/r/LLMDevs/comments/1oub14b/graphiti_mcp_server_10_released_20000_github_stars/
https://ai.miraheze.org/wiki/Graphiti
https://www.thesecondbrain.dev/how-it-works
https://www.thesecondbrain.dev/download
https://www.thesecondbrain.dev/vs-unabyss
https://www.thesecondbrain.dev/ownership
https://.../
https://theoutpost.ai/news-story/atlassian-opens-teamwork-graph-and-evolves-rovo-into-autonomous-agent-at-team-26-26033/
https://sdtimes.com/softwaredev/atlassian-unveils-teamwork-graph-rovo-max-at-its-team-event/
https://futurumgroup.com/insights/atlassian-teamwork-graph-the-secret-weapon-thats-no-longer-a-secret/
https://www.atlassian.com/blog/company-news/teamwork-graph-team-26
https://x.com/Atlassian/status/2052097829665923208
https://theplanettools.ai/tools/glean
https://aws.amazon.com/marketplace/pp/prodview-3hxyfnuih42u2
https://bloomfire.com/resources/what-is-glean/
https://www.glean.com/compare
https://ai.engineer/orgs/glean
https://support.writer.com/article/40-whats-new-at-writer
https://learningcommons.org/news/knowledge-graph-launches-new-features/
https://yearofthegraph.xyz/newsletter/2025/05/the-evolution-of-the-graph-technology-and-business-landscape-in-2025-the-year-of-the-graph-newsletter-vol-27-spring-summer-2025/
https://gdotv.com/blog/weekly-edge-kuzu-forks-duckdb-graph-cypher-24-october-2025/
https://writer.com/engineering/rag-benchmark/
https://nubiapage.com/mem-ai-review-2026-pricing-app-alternatives-free-plan/
https://aitoolsofficial.com/ai-tools/memai/
https://aitoolpick.org/pricing/mem-ai/
https://www.fahimai.com/capacities-vs-mem-ai
https://www.fahimai.com/clickup-vs-mem-ai
https://github.com/MemTensor/MemOS
https://github.com/MemTensor/MemOS/blob/main/README.md
https://github.com/Memtensor-AI/MemOS
https://dev.co/ai/frameworks/memos
https://aitoolradar.io/open-source/memos
https://www.prnewswire.com/news-releases/vectorize-breaks-90-on-longmemeval-with-open-source-ai-agent-memory-system-302643146.html
https://hindsight.vectorize.io/blog/2026/10/27/hindsight-cloud-october
https://docs.hindsight.vectorize.io/cloud/whats-new/
https://vectorize.io/blog/introducing-hindsight-agent-memory-that-works-like-human-memory
https://hindsight.vectorize.io/blog/2026/10/13/memlake
https://x/
https://domain/
https://reorproject.org/
https://medium.com/fongyang/meet-reor-the-private-and-local-ai-powered-note-taking-app-aad7d90c412d