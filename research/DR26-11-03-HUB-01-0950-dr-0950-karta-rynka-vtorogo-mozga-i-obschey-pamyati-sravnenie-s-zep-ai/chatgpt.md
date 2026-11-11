# DR26-11-03-HUB-01-0950 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-11-03-HUB-01-0950 — Карта рынка «второго мозга» и общей памяти в 2026 году

Дата исследования: **3 ноября 2026 года**. Под «памятью» ниже я различаю четыре архитектурно разные вещи: персонализацию по истории пользователя; долговременную память агента; общий слой знаний/контекста для нескольких агентов; корпоративный knowledge/search layer. Это важно: большинство продуктов маркетингово называют всё это memory, но технологически они решают разные задачи. Источники преимущественно официальные product/docs/GitHub/papers 2025–2026; более старые материалы отмечены, если они существенны.

## Карта рынка и положение Zep

Рынок к концу 2026 года уже не пустой. Более того, **простая “долговременная память для агента” быстро коммодитизируется**: Mem0, Hindsight, Supermemory, Cognee, AWS AgentCore Memory и Google Memory Bank предлагают managed memory; Letta, LangMem, MemOS, MemMachine и A-MEM закрывают различные OSS-подходы; сами OpenAI, Google и Microsoft встраивают память в свои agent stacks. citeturn233617search2turn782055search1turn988966search0turn795437search6turn795437search8

Zep смещается в более узкую и ценную категорию: **temporal context infrastructure**, где память — не просто коллекция chunks или summaries, а изменяющаяся во времени модель сущностей, отношений, утверждений и их источников. В Zep Context Graph факты могут терять актуальность без уничтожения истории; Graphiti хранит temporal metadata и provenance для эпизодов и фактов и сочетает графовый обход, semantic/full-text retrieval и reranking. citeturn983401search1turn983401search3turn301064search9

Это уже не уникально по каждому отдельному признаку. Hindsight имеет temporal/entity graph и разделение фактов и beliefs; Cognee в 2026 добавил durable conflict resolution, bi-temporal memory и provenance; Google Memory Bank хранит revisions; OpenMemory реализует time-valid facts; MemOS строит graph-structured memory. Отличие Zep — **комбинация temporal graph + автоматически извлекаемые факты + provenance + history/invalidation + production retrieval + enterprise deployment** в одном продукте. citeturn800956search3turn583461search8turn988966search0turn795437search8turn795437search1

При этом возникает второй рынок, который стратегически важнее обычной agent memory: **vendor-neutral shared memory**, к которой одновременно подключаются Claude, Codex, Cursor и другие агенты. Здесь уже появились Cognee Cloud, Basic Memory, OpenMemory и Guru/Sana на корпоративной стороне, но рынок заметно менее зрелый. Cognee прямо позиционирует один shared graph для MCP-compatible agents, а Basic Memory — один Markdown/graph brain для Claude, Codex, Cursor, ChatGPT и любого MCP-клиента. citeturn988966search3turn795437search0

### Быстрая сегментация

| Сегмент | Наиболее сильные игроки к 03.11.2026 | Что реально является memory |
|---|---|---|
| Agent memory infrastructure | **Zep, Mem0, Hindsight, Cognee, Supermemory, AWS AgentCore Memory, Google Memory Bank** | автоматически извлекаемые долговременные facts/preferences/events/context |
| Agent-owned state / filesystem memory | **Letta, OpenAI Sandbox Agents, LangMem** | файлы, explicit memories, working state, agent-managed consolidation |
| Shared cross-agent memory | **Cognee, Basic Memory, OpenMemory, MemOS, MemMachine** | общий storage/graph, доступный нескольким агентам через API/MCP |
| Personal second brain | **Mem.ai, Tana, Recall, Capacities, Heptabase, Reflect, Khoj, Obsidian ecosystem** | заметки, связи, saved content, search/RAG; автоматичность сильно различается |
| Ambient personal memory | **Bee, Omi, Plaud, Screenpipe** | непрерывный захват речи/экрана → summaries/search/memory |
| Enterprise brain | **Glean, Microsoft Work IQ, Atlassian Rovo, Sana, Guru, Notion, Onyx, Dust, Writer** | permission-aware search/context поверх корпоративных систем |
| Personal + corporate fusion | **Glean ближе всех; Microsoft/Atlassian/Sana частично** | полноценный employee-owned personal graph + company graph как два слоя одного memory system **не найден** |

## Память ИИ-агентов

### Основные самостоятельные платформы и OSS

| Игрок | Модель памяти | OSS / лицензия | Деплой | Цена на 03.11.2026 | Деньги / тяга | Benchmarks |
|---|---|---|---|---|---|---|
| **Zep / Graphiti** | Temporal knowledge graph; entities, edges, facts, episodes; temporal validity/invalidation; semantic + full-text + graph retrieval; provenance. citeturn983401search1turn983401search3 | Graphiti — Apache-2.0; Zep managed layer proprietary. GitHub Graphiti ~31–32k stars. citeturn983401search4turn983401search5 | Zep Cloud; enterprise/BYOC; Graphiti self-host. Community Edition Zep deprecated. citeturn983401search6turn301064search0 | Официальная текущая pricing page не индексируется поиском; подтверждён free tier. Публичные точные Flex/Flex Plus цены в найденных первичных источниках: **нет данных**. citeturn301064search0 | Graphiti ~31–32k GitHub stars; клиентские числа: **нет данных** в найденной актуальной первичной документации. citeturn983401search4 | Zep-authored paper: DMR 94.8 против MemGPT 93.4; до +18.5 п.п. на LongMemEval и заявленное ~90% снижение latency относительно выбранных baselines. Это **vendor-authored evaluation**, не независимая лига. citeturn983401academia10 |
| **Mem0** | Main memory + semantic/vector search; managed Graph Memory строит entity co-occurrence graph, объединённый с semantic и BM25 ranking. Это не столь типизированный temporal KG, как Zep. citeturn233617search0turn233617search1 | Core Apache-2.0; Graph Memory managed-функция не входит полностью в OSS. ~68k GitHub stars. citeturn233617search4 | Cloud; self-host server; enterprise on-prem. citeturn233617search1turn233617search2 | Free; Starter $19/mo; Pro $249/mo; Enterprise custom. citeturn233617search2 | $24M совокупно Seed + Series A объявлены 28.10.2025; компания заявляла тысячи пользователей/enterprise deployments. citeturn233617search7 | Vendor benchmark Apr-2026: LoCoMo 92.5, LongMemEval 94.4, BEAM 64.1/48.6; p50 retrieval ~0.88–1.09 s в их harness. Managed optimizations отличаются от OSS. citeturn233617search4turn233617search6 |
| **Hindsight / Vectorize** | Structured retain/recall/reflect; vector + keyword + graph traversal + temporal filtering; fact/belief separation, temporal entity graph, confidence/opinions. citeturn800956search3 | MIT; ~58k GitHub stars к концу 2026. citeturn782055search5 | OSS, managed cloud, single-tenant Enterprise; BYOC/on-prem options. citeturn782055search1turn782055search8 | Retain $10/M input tokens; recall $0.75/M output; reflect $0.05/call; storage после grace period $0.25/M tokens/month. citeturn782055search0 | В peer-reviewed ACL 2026 demo paper авторы указывали 13.3k stars и production use в Fortune 500; текущая GitHub traction заметно выше. citeturn800956search3turn782055search5 | ACL paper: 83.6 LongMemEval, 83.2 LoCoMo на 20B open model; 91.4 LongMemEval с Gemini 3 Pro. Поздний vendor benchmark v0.4.19 заявляет LoCoMo 92.0 и LongMemEval 94.6. Разные модели/harness → напрямую с Mem0/Zep сравнивать нельзя. citeturn800956search3turn800956search2 |
| **Cognee** | Knowledge graph + vector/semantic + temporal search; в Enterprise — bi-temporal memory, conflict resolution и provenance. citeturn583461search4turn988966search0 | Apache-2.0; ~33k GitHub stars. citeturn583461search5 | Cloud, local Docker, on-prem/private cloud/BYOC. citeturn583461search4turn583461search7 | Free: 1M tokens / 1 workspace; Standard $1/1M tokens + $5 per additional workspace; Enterprise custom/BYOC. citeturn988966search0 | $7.5M seed announced 19.02.2026 — materially newer than the older $1.5M figure. citeturn988966search4 | Публичный сопоставимый независимый LoCoMo/LongMemEval benchmark: **нет данных** в найденных первичных источниках. |
| **Supermemory** | Per-user memory graph, profiles, fact hierarchies; API/MCP intended to work across models and agent harnesses. citeturn800956search5turn800956search6 | Core repo MIT; ~31.5k stars. citeturn800956search6 | SaaS; Scale/Enterprise support self-host/enterprise deployment. citeturn800956search5 | Free credits; Pro $19/mo; Max $100/mo; Scale $399/mo; Enterprise custom. citeturn800956search5 | Компания в сентябре 2026 прекратила отдельные Nova/company-brain interfaces и сосредоточилась на memory engine/API/MCP; её собственный пост заявляет использование сотнями тысяч компаний, но это маркетинговая self-report metric. citeturn800956search8 | Сопоставимый независимый benchmark: **нет данных**. |
| **Letta** | Agent-managed state; MemFS — git-tracked memory filesystem, который сам агент читает/редактирует; background “dreaming” для consolidation. citeturn583461search1turn583461search2 | Apache-2.0; ~25k GitHub stars. citeturn583461search0 | Hosted или self-hosted. citeturn583461search2 | Текущая публичная цена SaaS в найденных первичных источниках: **нет данных**; docs фиксируют quota MemFS 5 MiB Free / 50 MiB Pro. citeturn583461search1 | ~25k stars; клиентская/финансовая traction в исследованных текущих первичных источниках: **нет данных**. | Прямой актуальный LoCoMo/LongMemEval benchmark: **нет данных**. |
| **LangMem** | Semantic memories + explicit agent-managed hot-path writes + background consolidation; storage abstraction, нативно LangGraph Store/Postgres. citeturn690988search0turn690988search1 | MIT; ~1.7k stars. citeturn690988search2 | Library; local/own infrastructure. | Отдельного SaaS/pricing: **нет данных**. | ~1.7k stars; funding/client numbers: **нет данных**. | **нет данных**. |
| **MemOS / MemTensor** | Memory operating system: graph-structured memories, multimodal memory, “memory cubes”, namespaces/isolation и sharing между users/projects/agents. citeturn690988search4 | Apache-2.0. citeturn690988search4 | OSS/self-host; managed details: **нет данных**. | **нет данных**. | GitHub traction в первичном источнике, доступном поиску: **нет данных**. | Авторы заявляют LoCoMo 88.83 и LongMemEval 89.20; это vendor/project self-evaluation. citeturn690988search4 |
| **MemMachine** | Working memory + episodic graph в Neo4j + profile memory в SQL; model-agnostic; MCP. citeturn690988search9 | Apache-2.0. citeturn690988search9 | Cloud/self-host/Docker. citeturn690988search9 | **нет данных**. | Stars/funding/customers: **нет данных**. | **нет данных**. |
| **A-MEM** | Zettelkasten-like autonomous memory: notes, dynamic indices/tags/links, memory evolution. citeturn800956academia11 | MIT research code; ~1.2k stars. citeturn800956search4turn800956search10 | Research OSS, не production SaaS. | Бесплатный OSS. | NeurIPS 2025 work; commercial traction: **нет данных**. | Academic evaluation есть в paper, но это research prototype, не production service. citeturn800956academia11 |
| **Graphlit** | Graph-oriented multimodal ingestion/RAG/context service; MCP. citeturn800956search0 | Managed platform; OSS status: **нет данных**. | Cloud. | Historical tiers: Hobby $49, Starter $199, Growth $999. Но **новые регистрации закрыты: service winding down**. citeturn800956search0turn800956search9 | В ноябре 2026 не считать активным новым конкурентом. citeturn800956search9 | **нет данных**. |

Самая важная поправка к конкурентной карте Zep: **Hindsight и Cognee в 2026 стали гораздо ближе к Zep, чем следует из карты рынка 2025 года**. Hindsight уже имеет temporal graph и belief/fact semantics; Cognee теперь документирует bi-temporal memory, provenance и durable conflict resolution. То есть “мы единственные с графом/временем” — слабая позиция для интервью. citeturn800956search3turn583461search8turn988966search0

### Hyperscalers и встроенная память моделей

| Игрок | Реализация | Что важно относительно Zep |
|---|---|---|
| **AWS AgentCore Memory** | Short-term raw events плюс автоматически извлекаемые long-term memories: semantic, summarization, preferences, episodic и custom strategies. `actorId` может обозначать user, agent или system. citeturn186549search0turn186549search1 | Это managed infrastructure и сильный distribution threat. Но публичная модель — memory records/strategies, не богатый temporal KG. С 06.10.2026 STM: $1/GB ingest, $0.20/GB retrieval, $0.10/GB-month storage; long-term built-in storage $0.75/1k records/month и retrieval $0.50/1k calls. citeturn795437search5turn795437search6 |
| **Google Memory Bank** | Identity-scoped facts; automatic extraction, consolidation, continuous event ingestion, revisions, TTL, multimodal inputs и IAM-scoped access. citeturn795437search8 | Очень близок к “managed durable user memory”, но не публично описан как relational/temporal entity graph. Revisions дают историю изменения памяти. Storage $0.30/GiB-month; reads — $0.085 за 3M ops equivalent; writes — $0.085 за 1M ops, плюс model/embedding tokens. Billing начался 01.09.2026. citeturn795437search7 |
| **OpenAI ChatGPT Memory** | Saved/user memories + history-derived memory; в 2026 “dreaming” автоматически синтезирует и обновляет memories во времени, включая перевод будущего события в прошедшее после наступления даты. citeturn449271search0turn449271search7 | Это closed, end-user personalization layer, не внешняя neutral memory DB. Нет API, позволяющего использовать ChatGPT personal memory как универсальную память стороннего агента; отдельная цена памяти: **нет данных**. |
| **OpenAI Agents / Sandbox memory** | Agent memory может храниться как files/artifacts; Sandbox docs различают session history и distilled reusable memory; официальный пример показывает отдельные memory layouts для нескольких агентов. citeturn449271search4turn449271search24 | Ближе к Letta/file-based memory, чем к Zep graph. Self-hosted sandbox позволяет приложению управлять compute/files, но orchestration остаётся частью OpenAI Agents stack. citeturn449271search19 |
| **Anthropic Claude** | Claude personalization использует past-chat search, profile preferences, project instructions; Projects имеют отдельную knowledge base/RAG. citeturn449271search1turn449271search20 | Это product-specific personalization/project context. Отдельный vendor-neutral memory service, temporal graph или public memory API для сторонних агентов в изученных актуальных docs: **нет данных**. |
| **Microsoft Copilot Studio memory** | В preview каждый agent хранит отдельную память для конкретного пользователя; memory stored as files, maker не видит user memory; неактивная память удаляется через 28 дней; group/channel memory не поддерживается. citeturn186549search10 | Сильная privacy isolation, но это вертикальная память внутри Microsoft agent environment, а не shared cross-vendor graph. |

По benchmark numbers нельзя строить честный рейтинг “94.8 > 94.6 > 94.4”: Zep, Mem0 и Hindsight используют разные модели, retrieval settings, prompt/harness и иногда разные версии dataset/evaluator. Практически полезный вывод — **все три уже находятся в диапазоне, где architectural properties, latency, governance и write semantics важнее нескольких пунктов vendor benchmark**. Это вывод из различающихся методик самих опубликованных evaluations, а не результат единого независимого head-to-head. citeturn983401academia10turn233617search4turn800956search2turn800956search3

## Общая память нескольких агентов

Здесь рынок существенно менее насыщен. Большинство “agent memory” API предполагает `user_id`/`agent_id`, но это ещё не означает, что Claude, Codex, Cursor и собственный agent могут **одновременно читать и писать в один логически общий brain** с понятными conflict semantics, ACL и provenance.

| Продукт | Реально cross-agent? | Хранилище / интерфейс | Конфликты | Права | Provenance |
|---|---|---|---|---|---|
| **Cognee Cloud** | **Да, явно.** Документация рекламирует “one memory for every MCP-compatible agent/framework” и shared graph, куда sessions из Claude/Cursor и других клиентов пишут обратно. citeturn988966search3 | Knowledge graph + semantic/temporal search; API/MCP. citeturn583461search4turn988966search3 | Durable conflict-resolution API добавлен в 2026; по умолчанию конфликт можно отправлять на review. citeturn583461search8 | Workspace/team-level model; granular full ACL semantics публично описаны не полностью. citeturn988966search0 | Enterprise обещает provenance на every answer. citeturn988966search0 |
| **Basic Memory** | **Да.** Один brain подключается к Claude Desktop/Code, Codex, Cursor, ChatGPT Custom GPT, VS Code и любому MCP client. citeturn795437search0 | Plain Markdown + SQLite/Postgres index + wikilink knowledge graph + semantic/hybrid retrieval; local или Cloud. citeturn795437search0 | Cloud использует rclone-powered bidirectional sync с conflict resolution; snapshots/history. Это прежде всего **file/sync conflict handling**, а не semantic contradiction resolution между facts. citeturn795437search0 | Local filesystem boundaries; cloud auth. Fine-grained fact ACL: **нет данных**. | Markdown/files дают происхождение на уровне notes; formal fact-level provenance: **нет данных**. |
| **OpenMemory / OpenTech-Lab** | **Да.** Watcher ingest'ит session logs агентов, MCP/API дают общий локальный memory layer. citeturn795437search1 | Postgres/OpenSearch + temporal KG: episodes, entities, time-valid facts/entity history; BM25/importance/recency. citeturn795437search1 | Semantic contradiction policy: **нет данных**. | Local infrastructure + encrypted secrets; granular per-agent ACL: **нет данных**. citeturn795437search1 | Episode/entity history есть; explicit source-to-fact provenance contract уровня Zep: **нет данных**. |
| **MemOS** | **Да на уровне архитектуры.** Memory cubes предусматривают isolation/sharing между users/projects/agents. citeturn690988search4 | Graph-structured memory. | **нет данных** о production-grade concurrent conflict semantics. | Namespaces/isolation есть; enterprise ACL details: **нет данных**. | **нет данных**. |
| **MemMachine** | **Да концептуально:** LLM-agnostic + MCP, общий episodic graph/profile stores. citeturn690988search9 | Neo4j + SQL + MCP. | **нет данных**. | **нет данных**. | **нет данных**. |
| **Mem0** | Можно подключать разные модели/agents к одному backend, но найденная документация сильнее описывает memory scoped по entities/users, чем явную “cross-vendor shared brain” модель. citeturn233617search1turn233617search3 | Vector + BM25 + optional managed graph. | Graph обновляется автоматически; публичная explicit contradiction/conflict workflow уровня Cognee/Zep: **нет данных**. | Enterprise/on-prem controls. citeturn233617search2 | Fact-level source lineage: **нет данных** в изученных current docs. |
| **Zep** | Технически **да**: multiple applications/agents могут использовать один user graph или общий `graph_uuid`; Zep не привязан к модели. Но cross-vendor “one personal brain for Claude+Codex+Cursor” не является основным продуктовым UX. citeturn983401search1turn301064search9 | Temporal Context Graph. | Новый противоречащий контекст может invalidate старый fact без удаления исторического состояния. citeturn983401search3 | Governance/API/audit controls; enterprise deployment. citeturn301064search5turn301064search9 | Episodes/sources → facts/edges; source tracing — один из центральных элементов. citeturn983401search3turn301064search9 |

Для пользовательской архитектуры особенно важен **Basic Memory**. Он уже реализует почти буквальную продуктовую формулировку “одна память для Claude, Codex, Cursor, ChatGPT + Obsidian-compatible Markdown + multiple devices”, стоит $15/mo в beta и имеет AGPL-3.0 local version. GitHub MCP registry показывает около 4.4k stars. citeturn795437search0 Это значит, что уникальность “Obsidian + несколько агентов + несколько машин” **как таковая уже исчезла**.

Но Basic Memory не решает ту же задачу, что ваш SQLite graph RAG, если у вас есть структурированные temporal facts, entity resolution и provenance. У Basic Memory graph сформирован observations/wikilinks, а documented conflict resolution относится к bidirectional file sync. Для semantic contradictions — “цена проекта изменилась”, “человек сменил работу”, “агент A и агент B сделали взаимоисключающие выводы” — полноценной documented bitemporal reconciliation модели я не нашёл. citeturn795437search0

Отдельно существует **Mem0 OpenMemory CLI/TUI**, который в 2026 переносит coding sessions между Claude Code, Codex и OpenCode и имеет MIT-лицензию; это уже другая новая реализация, тогда как прежний `mem0ai/openmemory` self-hosted memory app был архивирован. citeturn795437search2turn211448search3 Название OpenMemory, следовательно, неоднозначно: его используют несколько несвязанных проектов.

Самая зрелая из найденных моделей конфликтов сейчас — сочетание идей **Zep + Cognee**: не destructive overwrite, а состояние/история плюс explicit resolution/review. Cognee 1.6.6 в октябре 2026 документировал durable fact conflict-resolution API; Zep сохраняет temporal history/invalidation и source provenance. citeturn583461search8turn983401search3

## Личный второй мозг

Здесь полезно жёстко разделять **manual PKM**, **AI-assisted PKM** и **ambient/autonomous memory**. Большинство приложений, которые называются “second brain”, всё ещё требует от пользователя самому решить, что сохранить.

| Продукт | Автоматичность | Что представляет собой память | Цена / статус | Комментарий |
|---|---|---|---|---|
| **Mem.ai** | Средняя/высокая: notes + agent, voice, автоматическое отслеживание tasks/projects; Pro подключает внешние источники вроде Gmail/Slack/Todoist. citeturn384797search0 | AI-organized personal workspace, не публично документированный temporal KG. | Free; Plus $9/mo; Pro $29/mo и выше. citeturn384797search0 | Один из наиболее “agentic” classic second-brain products. |
| **Notion AI** | Средняя: пользователь создаёт workspace; AI ищет и отвечает по Notion + подключённым sources. citeturn239807search8turn239807search9 | Docs/databases + enterprise search/RAG. | Business/Enterprise; custom agents потребляют Notion credits с мая 2026, $10/1000 shared credits. citeturn239807search10 | Сильнее workspace, чем autonomous memory. |
| **Tana** | Средняя: structured nodes + AI agents + meeting agent автоматически захватывает Zoom/Teams/Meet. citeturn384797search3 | Structured knowledge graph/workspace. | Pro early-bird $20/user/mo; Max $80; Business custom. citeturn384797search3 | Один из наиболее структурированных personal/team knowledge products. |
| **Reflect** | Низкая/средняя. Пользователь ведёт notes; AI помогает работать с ними. | Linked-note PKM. | Новый Reflect Mac — free, AI через BYO key; legacy Classic — $10/mo. citeturn384797search1turn384797search2 | Скорее manual second brain. |
| **Heptabase** | Низкая/средняя. | Visual cards/whiteboards + AI over knowledge base. | Pro $8.99/mo annual; Premium $17.99; Premium+ $53.99. citeturn384797search5 | Manual-first knowledge work. |
| **Capacities** | Низкая/средняя. | Object-based PKM + related-content/search + AI. | Basic free; Pro $9.99/mo annual; AI add-on $8.33/mo annual. citeturn140331search0 | Strong schema/object UX, но не autonomous memory. |
| **Recall** | Средняя: пользователь сохраняет web/content, после чего system автоматически делает tags, links и knowledge graph. citeturn140331search2turn140331search3 | Auto-generated personal knowledge graph по saved content. | Free; Plus $10/mo annual; Max $38/mo; API/MCP доступен. citeturn140331search2 | Ближе к automated knowledge capture, но capture инициирует пользователь. |
| **Personal.ai** | Средняя/высокая: Memory Stacks автоматически накапливают snippets из взаимодействий. citeturn140331search5 | Linked memory nodes / personal model context. | Текущая цена: **нет данных**. | Интересная концепция персонального long-term model context; техническая temporal/provenance модель публично ограничена. |
| **Khoj** | Средняя. Local/self-host personal/team AI; agents, automations; работает с GPT/Claude/Gemini/local models. citeturn537859search4 | Documents/web + personal agent search/RAG. | OSS AGPL-3.0; managed price в исследованных источниках: **нет данных**. | Vendor-neutral и self-hostable. |
| **Reor** | Низкая/средняя. | Local AI notes / semantic retrieval. | AGPL-3.0; repo **архивирован 07.03.2026**; около 8.5k stars на момент архивации. citeturn537859search6turn537859search8 | Не считать живым стратегическим конкурентом. |
| **Obsidian + community AI** | Базовый Obsidian manual-first; плагины добавляют AI/search/automation. В официальном каталоге к ноябрю 2026 ~9,850 plugins, из них ~1,166 категоризированы как AI. citeturn795437search3turn795437search4 | Local Markdown graph/backlinks + произвольные plugins. | Core app pricing здесь не исследовалась; plugin costs варьируются. | Сильнейшая “substrate/platform” модель: данные пользователя остаются plain files, но unified memory semantics приходится строить самому. |
| **Screenpipe** | Высокая: continuous screen/audio capture. | Local-first searchable personal activity history. | Текущая официальная codebase source-available под Screenpipe Commercial License; business sync существует, но shared context между teammates прямо не заявлен как готовая функция. citeturn537859search0turn537859search2 | Ambient memory, а не curated PKM. |
| **Omi** | Высокая: wearable записывает разговоры, далее transcription/apps/memory. citeturn537859search10 | Ambient conversational memory; API/MCP, local options. | Pendant list $179, текущая promo $129; 300 cloud min бесплатно; unlimited annual ~$269. citeturn537859search9 | Open ecosystem; сильный capture layer, не зрелый temporal knowledge graph. |
| **Plaud / NotePin** | Высокая при записи; summaries/transcripts автоматически. | Recorded meetings/conversations → AI notes. | NotePin $159; free 300 min/month, платные tiers ~€9–19/month в EU в зависимости от плана. citeturn862594search0turn862594search1 | “AI recorder” сильнее, чем universal second brain. |
| **Bee / Amazon** | **Очень высокая**: ambient conversations + email/calendar/app integrations; строит summaries/patterns без ручного journaling. citeturn862594search7 | Personal ambient memory. | Bee Frontier $74.99; Eclipse $99.99; Bee Pro $11.99/mo; free tier ограничивает memory horizon. citeturn862594search5turn862594search6 | После присоединения к Amazon в 2025 Bee остаётся активным продуктом и усиливается. citeturn862594search7 |
| **Limitless / Rewind** | Исторически высокая ambient capture. | Audio/screen/life log. | **Независимый продукт фактически заканчивается**: Meta приобрела Limitless; новые Pendant sales прекращены 05.12.2025, Rewind capture отключён 19.12.2025, support existing Pendant обещан до конца 2026. citeturn140331search6 | В 2026 это не самостоятельный долгосрочный конкурент. |

Ключевой разрыв внутри personal market: Bee/Omi/Plaud/Screenpipe хорошо решают **capture**, Obsidian/Tana/Heptabase/Capacities — **human knowledge organization**, Recall — **content ingestion + graph**, но почти никто не объединяет ambient life stream, carefully curated notes, model-independent agent memory, semantic fact conflict resolution и bitemporal provenance в один user-owned substrate. Это синтез перечисленных публичных capabilities, а не заявление конкретного поставщика. citeturn862594search7turn537859search10turn795437search3turn140331search2turn795437search0

## Корпоративный «второй мозг»

Корпоративный рынок намного зрелее personal/shared-agent market, но большая часть лидеров продаёт **permission-aware enterprise search + agent context**, а не memory в смысле Zep.

| Игрок | Что строит | Настоящий knowledge graph? | Агентный слой / права | Цена |
|---|---|---|---|---|
| **Glean** | Enterprise search/assistant поверх корпоративных приложений; Enterprise Graph связывает company knowledge, people/process signals и personal graphs. citeturn239807search0turn239807search1 | **Да, один из самых явных.** Glean прямо называет Enterprise Graph и описывает relationships между content, people и processes. citeturn239807search0 | Permission-aware; individual activity/data signals изолируются и агрегируются для организационных signals. citeturn239807search2 | Публичная официальная list price: **нет данных**. |
| **Microsoft Work IQ** | Semantic workplace intelligence layer поверх email, calendar, meetings, chats, files, people и business systems; GA с 16.06.2026. citeturn239807search4turn239807search5 | **Скорее semantic organizational model**, чем публично описанный property graph. | A2A/MCP/REST, user-scoped permissions, Rego policies, audit, SharePoint Embedded workspaces. citeturn239807search4 | Usage-based; точные единицы в найденном source fragment: **нет данных**. |
| **Atlassian Rovo + Teamwork Graph** | Cross-product search/agents по Jira/Confluence + external apps. | **Да.** Teamwork Graph traverses Atlassian и third-party data и возвращает prejoined structured context. citeturn239807search6 | Atlassian permission model/connectors. citeturn239807search11 | Additional Rovo usage $0.01/credit; enriched Teamwork Graph calls обычно 1–10 credits. citeturn239807search6turn239807search7 |
| **Notion Enterprise Search** | Search + cited answers по Notion, Slack, Drive, Jira и др. citeturn239807search8turn239807search9 | **Нет доказательств общего semantic entity graph** в enterprise-search docs; есть database relations внутри Notion, но search layer описан как retrieval/search. | Permission-aware. citeturn239807search9 | Business/Enterprise; agent credits $10/1000 workspace credits. citeturn239807search10 |
| **Guru** | “AI Knowledge Layer”: governed/verified enterprise knowledge; 100+ integrations и MCP. citeturn279808search0 | Не найдено доказательства property/temporal KG; скорее curated knowledge layer. | Сильная governance, permissions, citations/lineage; ChatGPT/Claude/Copilot/Cursor могут обращаться к одному governed truth через MCP. citeturn279808search0turn279808search1 | Custom. |
| **Sana from Workday** | Enterprise search/answer/action layer; после acquisition Workday завершил сделку 04.11.2025. citeturn224512search0turn224512search1 | Публично документированный general-purpose graph: **нет данных**. | Очень важно: с октября 2026 Sana может отдавать governed company context в Claude, Gemini Enterprise, Microsoft Copilot/Teams и Slack, сохраняя policy/permission/audit control. citeturn224512search2 | Sana for Workday через Flex Credits; standalone current exact price: **нет данных**. |
| **Onyx** | Open-source enterprise search / RAG. | Есть LLM-based knowledge graphs как retrieval technique, но это не обязательно persistent organizational graph уровня Glean. citeturn279808search6 | Permission-aware; self-host/on-prem/region-specific Enterprise. citeturn279808search3turn279808search6 | $20/user/mo annual; Enterprise custom. citeturn279808search3 |
| **Dust** | Team agent platform over company data; custom agents, multi-agent workflows, connectors/MCP, 20+ frontier models. citeturn279808search2 | Persistent enterprise KG: **нет данных**. | Team workspaces; Enterprise single-tenant. | Pro $24/user/mo annual; Max $120; Enterprise custom. citeturn279808search2 |
| **Writer Knowledge Graph** | Writer официально называет Knowledge Graph свою RAG architecture: intent routing, multilevel retrieval, reranking по Confluence/SharePoint/Drive/Notion/files/web. citeturn224512search3 | **Название вводит в заблуждение:** найденное техническое описание больше похоже на advanced RAG, чем на entity/relationship temporal graph. | Enterprise governance. | Enterprise contact sales. citeturn224512search4 |
| **Slack AI / enterprise search** | Search по Slack + connected enterprise sources. citeturn279808search4 | Общий knowledge graph: **нет данных**. | Permission-aware cross-app search. citeturn279808search4 | **нет данных** в исследованных sources. |
| **Moveworks / ServiceNow Otto** | Moveworks в 2026 интегрирован в ServiceNow Otto, объединяющий conversational enterprise agents и ServiceNow workflows. citeturn224512search6turn920586search2 | ServiceNow Knowledge Graph существует и может быть exposed агентам через MCP; это сильный structured-enterprise-data play. citeturn224512search5 | Governed enterprise access/workflows. | **нет данных**. |
| **Hebbia** | Current public primary material, достаточный для точной классификации memory/graph architecture, в исследовании не найден. | **нет данных**. | **нет данных**. | **нет данных**. |

Главные реальные “graph companies” в этой группе — **Glean и Atlassian Teamwork Graph**; ServiceNow также имеет Knowledge Graph как часть своей data/workflow платформы. Writer использует термин Knowledge Graph, но его текущая документация описывает архитектуру RAG/retrieval, поэтому при конкурентном анализе я бы не ставил его в один технический класс с Zep/Glean. citeturn239807search0turn239807search6turn224512search5turn224512search3

Glean и Zep при этом находятся на разных осях. **Glean начинает с корпоративных repositories, identities и permissions и строит retrieval/assistant layer для организации. Zep начинает с agent/user interaction streams и строит evolving temporal context graph.** Их overlap растёт, но primary use case различен. citeturn239807search0turn239807search2turn301064search9

Особенно важен **Sana**: это уже доказательство спроса на “одна корпоративная knowledge/context layer, много AI vendors сверху”. В октябре 2026 Workday прямо объявил, что governed Sana context может использоваться из Claude, Gemini Enterprise, Copilot/Teams и Slack. Это корпоративный аналог вашей идеи shared memory, хотя Sana ориентирован на knowledge/access, а не на write-back personal memory всех этих агентов. citeturn224512search2

## Слияние личного и корпоративного и сравнение с Zep

### Есть ли продукт «мой brain + brain компании»?

**Полного продукта в указанном вами смысле я не нашёл.** Точнее: в публичной документации рассмотренных систем не найдено решения, где одновременно существуют:

личный, долговременный, agent-written memory graph сотрудника; company-owned shared graph; оба — first-class layers одной graph/memory model; per-fact ACL и provenance; policy, определяющая, какие personal facts становятся corporate; и формализованная процедура separation/export/delete при увольнении.

Ближе всего **Glean**, потому что компания прямо описывает Enterprise Graph как объединяющий organization-wide knowledge и **personal graphs**, а privacy docs отдельно говорят об изоляции individual signals. citeturn239807search0turn239807search2 Но доказательства того, что этот “personal graph” является **employee-owned portable second brain**, который человек забирает при увольнении, нет. Правила ownership/export personal graph при termination: **нет данных**.

**Microsoft Work IQ** строит общий semantic workplace layer и выполняет actions с user-scoped permissions, но публичные docs не показывают отдельный переносимый personal memory graph сотрудника как соседний слой company graph. citeturn239807search4

**Atlassian Teamwork Graph** связывает work objects, people и external systems, но также не документирует employee-owned persistent AI memory и post-employment split. citeturn239807search6turn239807search11

**Sana** решает другой кусок проблемы — один governed company context может безопасно обслуживать много AI front ends. Но personal agent memory этих front ends не становится вторым слоем одного graph. citeturn224512search2

**Claude Projects** хорошо демонстрирует privacy split: project/knowledge base могут быть shared внутри Claude for Work, тогда как chats остаются private, пока пользователь их сам не расшарит. Но это sharing model внутри одного vendor, а не unified personal/company memory graph. citeturn449271search34

### Общая сравнительная таблица с Zep

| Игрок | Сегмент | Что похоже на Zep | Главное отличие от Zep | Время / history | Enterprise deployment | OSS | Публичная цена |
|---|---|---|---|---|---|---|---|
| **Zep** | Agents / context infrastructure | — | Temporal Context Graph — базовая модель | **Bitemporal/temporal facts, invalidation + history**. citeturn983401search3 | Managed + enterprise/BYOC; Graphiti self-host. citeturn983401search6 | Graphiti Apache-2.0 | Exact current tiers: **нет данных** в индексируемой official pricing |
| **Hindsight** | Agents | Graph + temporal retrieval + long-term structured memory | Fact/belief/opinion model; retain/recall/reflect abstraction | **Да, temporal entity graph**. citeturn800956search3 | Cloud / single tenant / BYOC/on-prem. citeturn782055search1 | MIT | Usage-based. citeturn782055search0 |
| **Cognee** | Agents / shared-agent | KG, temporal search, provenance, conflict handling | Явно продаёт one shared memory across MCP agents | **Bi-temporal в Enterprise**. citeturn988966search0 | Cloud/local/BYOC | Apache-2.0 | Free; Standard usage-based; Enterprise custom. citeturn988966search0 |
| **Mem0** | Agents | Extracted persistent memory, managed API, graph option | Graph — entity co-occurrence/ranking layer; temporal semantics слабее | Полный bitemporal fact history: **нет данных** | Cloud/on-prem | Apache-2.0 core | $0 / $19 / $249 / custom. citeturn233617search2 |
| **Supermemory** | Agents | User memory graph, model/harness neutral, MCP | Более лёгкая graph/fact hierarchy abstraction; меньше documented temporal semantics | **нет данных** о bitemporal model | SaaS/self-host enterprise | MIT core | $19–399 + custom. citeturn800956search5 |
| **AWS AgentCore Memory** | Agents | Managed automatic long-term extraction | Hyperscaler primitive: records + strategies, не temporal KG | TTL/events/episodic memory; bitemporal facts: **нет данных** | AWS | Нет | Usage-based. citeturn795437search6 |
| **Google Memory Bank** | Agents | Automatic extraction/consolidation + evolving memories | Identity-scoped facts/revisions вместо relation-centric graph | **Revisions есть**, entity temporal graph — нет данных. citeturn795437search8 | Google Cloud regional/multiregional | Нет | $0.30/GiB-month + ops/tokens. citeturn795437search7 |
| **Letta** | Agents | Persistent state beyond context window | Agent сам редактирует filesystem memory; stateful-agent OS, не fact graph | Git/history-oriented; fact validity semantics — нет | Hosted/self-host | Apache-2.0 | **нет данных** |
| **Basic Memory** | Personal / shared-agent | Persistent graph-like context shared by agents | Markdown/wikilink graph; human+AI edit same files; local-first | Snapshots/history, но не bitemporal fact model | Local/cloud | AGPL-3.0 | $15/mo beta cloud. citeturn795437search0 |
| **OpenMemory** | Shared-agent | Temporal KG, entities, episodes, time-valid facts | Local-first developer stack, меньше enterprise governance | **Да, time-valid facts/history**. citeturn795437search1 | Self-host | MIT | Free OSS |
| **MemOS** | Agents / shared-agent | Graph memory + isolation/sharing | Memory-OS abstraction/cubes; earlier maturity | Graph evolution; exact bitemporal semantics — нет данных | OSS | Apache-2.0 | **нет данных** |
| **Glean** | Corporate / partial fusion | Graph context, enterprise retrieval, personalization | Organization-first search/knowledge layer, не conversation-derived agent memory | Temporal fact history: **нет данных** | Enterprise SaaS/single-tenant context | Нет | **нет данных** |
| **Microsoft Work IQ** | Corporate | Persistent contextual layer for agents | Workplace semantics and permissions tied to Microsoft ecosystem | Explicit bitemporal facts: **нет данных** | Microsoft cloud | Нет | Usage-based; exact figure **нет данных** |
| **Atlassian Rovo** | Corporate | Structured graph context + agents | Teamwork/work-object graph, не individual conversational memory | Temporal graph semantics: **нет данных** | Atlassian cloud | Нет | $0.01/extra credit. citeturn239807search7 |
| **Sana / Workday** | Corporate / cross-vendor | One governed context layer for multiple agents | Primarily retrieval/actions over company systems, not evolving personal fact graph | **нет данных** | Enterprise | Нет | **нет данных** |
| **Guru** | Corporate / cross-vendor | Shared governed context for ChatGPT/Claude/Cursor/etc. | Curated verified knowledge, not autonomous temporal memory | **нет данных** | Enterprise | Нет | Custom |
| **Onyx** | Corporate | RAG + optional LLM-generated KG | Enterprise search engine, not memory-of-interactions system | **нет данных** | Self-host/enterprise | Да | $20/user/mo annual. citeturn279808search3 |
| **Tana** | Personal/team | Graph-ish structured knowledge + agents | Human-authored structured workspace | Time validity/contradiction semantics: **нет данных** | SaaS | Нет | $20–80/user/mo + Business. citeturn384797search3 |
| **Recall** | Personal | Automatic KG from ingested material | Content-centric personal graph, not interaction/event memory | Bitemporal facts: **нет данных** | SaaS/API/MCP | Нет данных | $10–38/mo. citeturn140331search2 |
| **Bee** | Personal ambient | Automatic long-term personal context | Capture-first wearable/life context, closed consumer ecosystem | Longitudinal patterns, formal temporal graph — нет данных | Amazon SaaS/device | Нет | $11.99/mo Pro + hardware. citeturn862594search5turn862594search6 |

Для собеседования с Zep наиболее полезное competitive framing не “vector database versus graph database”, а три оси:

**Первая ось — representation.** Mem0/AWS/Google делают memory records/facts; Letta/OpenAI sandbox делают files/state; Zep/Hindsight/Cognee/OpenMemory делают всё более богатые relational/temporal structures. citeturn233617search0turn795437search6turn795437search8turn583461search1turn983401search3turn800956search3

**Вторая ось — scope.** Большинство vendor memory систем — `one application → users`; Basic Memory/Cognee/OpenMemory идут к `many agents → same memory`; Glean/Sana/Guru идут к `many employees/tools → same governed company context`. citeturn795437search0turn988966search3turn224512search2turn279808search1

**Третья ось — governance of truth.** В production недостаточно “найти похожий chunk”: нужны provenance, authority, temporal validity, conflicting writes, ACL и audit. Именно здесь Zep может защищаться от hyperscaler commoditization лучше, чем одним retrieval benchmark. Zep уже документирует source tracing и API/audit logs; Cognee активно движется в ту же сторону. citeturn301064search5turn301064search9turn583461search8

## Свободные ниши, чего в данных нет, и выводы

### Свободные ниши

| Ниша | Улика, что она остаётся открытой | Кто ближе всех |
|---|---|---|
| **Vendor-neutral personal memory для нескольких ИИ одновременно** | OpenAI/Claude/Microsoft memory остаются product-scoped; Basic Memory и Cognee уже дают cross-agent MCP, но никто из найденных игроков не доминирует как универсальный consumer “memory identity layer”. citeturn449271search7turn449271search1turn186549search10turn795437search0turn988966search3 | Basic Memory по UX; Cognee/Zep по structured graph; OpenMemory по local-first temporal model. |
| **Shared cross-agent memory с настоящим concurrent truth management** | MCP решает transport/tool access, но не автоматически provenance, conflicting writes, trust/authority и temporal reconciliation. Basic Memory документирует sync conflicts; Cognee — fact conflict workflow; Zep — temporal invalidation/provenance. Цельной cross-vendor CRDT-like/semantic transaction model не найдено. citeturn795437search0turn583461search8turn983401search3 | Cognee и Zep. |
| **Employee brain + company brain как два слоя одного графа** | Glean говорит о organization-wide + personal graphs, но employee-owned portable memory и separation on termination публично не описаны. Microsoft/Atlassian/Sana также не документируют такой dual-ownership graph. citeturn239807search0turn239807search4turn239807search6turn224512search2 | Glean — архитектурно ближе всех. |
| **“Bring your memory when you change employer”** | Claude позволяет export conversation data, а Basic Memory даёт plain Markdown portability, но корпоративные systems не демонстрируют portable employee memory, которая может безопасно отделить personal knowledge от company IP. citeturn449271search35turn795437search0 | Basic Memory по ownership; Glean по enterprise layer — но две половины пока не соединены. |
| **Семейный second brain** | Рассмотренные personal products в основном user-centric; enterprise products organization-centric. Специализированной сильной platform с family identities, shared/private memories, inheritance/consent и provenance в изученных источниках не найдено. | Basic Memory/Obsidian можно собрать вручную; специализированного лидера — **нет данных**. |
| **Ambient capture → verified temporal graph** | Bee/Omi/Plaud/Screenpipe сильны в capture/transcription; Zep/Hindsight/Cognee — в structured memory. Готового лидирующего продукта, который соединяет непрерывный life stream с provenance-aware temporal facts и human correction loop, не найдено. citeturn862594search7turn537859search10turn862594search1turn800956search3turn988966search0 | Архитектурно: Omi/Screenpipe + Zep/Hindsight/Cognee. |
| **Local-first temporal graph brain на десятках устройств** | Basic Memory решает local Markdown + sync + MCP, OpenMemory — local temporal KG, но enterprise-grade distributed reconciliation/ACL для множества персональных devices/agents публично не показан. citeturn795437search0turn795437search1 | Basic Memory + OpenMemory; ваш стек находится именно в этом промежутке. |

### Чего в данных нет

Несмотря на широкий поиск, несколько важных параметров невозможно честно заполнить из первичных публичных источников на 3 ноября 2026 года. **Точные текущие публичные цены Zep Flex/Flex Plus/Emerging** не удалось подтвердить через индексируемую официальную pricing page: текущая документация подтверждает Free tier и функциональные различия Flex Plus/Enterprise, но не показывает цифры; поэтому ранее известные $125/$375/$13k здесь не выдаются за перепроверенный факт. citeturn301064search0turn301064search2

Нет независимого единого benchmark, где Zep, Mem0, Hindsight, Cognee, Supermemory, AWS и Google протестированы **одновременно**, на одинаковой модели, prompts, tokenizer, retrieval budget, judge и versioned dataset. Поэтому vendor-reported LoCoMo/LongMemEval/DMR нельзя превращать в общий leaderboard. citeturn983401academia10turn233617search4turn800956search2

Для большинства решений нет публичных данных о **write collision rate, lost updates, entity merge precision, temporal contradiction precision, provenance accuracy, deletion propagation, ACL leakage rate и memory poisoning resistance**. Эти метрики для shared memory практически важнее обычного QA score, но vendors пока редко публикуют их.

Не найдено публичной спецификации того, что происходит с “personal graph” сотрудника Glean при увольнении: что удаляется, что сохраняется компании, что экспортируется сотруднику и как разбираются facts смешанного происхождения. То же относится к Microsoft Work IQ, Atlassian и Sana; в публичной документации изученных продуктов такой dual-ownership lifecycle не описан. citeturn239807search0turn239807search4turn239807search6turn224512search2

Для Anthropic не найден отдельный публичный infrastructure product, эквивалентный Zep/Memory Bank/AgentCore Memory: найденные текущие материалы описывают Claude personalization, Projects и past-chat/project retrieval. citeturn449271search1turn449271search20

Для Hebbia по доступным первичным результатам не удалось получить достаточно точную текущую technical documentation, чтобы честно классифицировать его как graph-memory player; поэтому вместо предположений — **нет данных**.

### Три вывода, которые меняют действия

1. **На интервью с CEO Zep не стоит продавать тезис “Zep выигрывает потому, что у него graph + temporal memory”.** К ноябрю 2026 этот moat уже частично скопирован: Hindsight имеет temporal entity graph и fact/belief distinction, Cognee — bi-temporal memory + conflict resolution + provenance, OpenMemory — time-valid facts, Google — revisions. Более сильный тезис: Zep может стать **system of record for agent context**, где moat — combined provenance, temporal truth maintenance, entity resolution, governance, low-latency retrieval и enterprise operability. Вопрос CEO, который демонстрирует понимание рынка: *«Когда AWS/Google commoditize user memory records, Zep хочет выиграть как лучший temporal retrieval engine или как authoritative context/governance plane между data, humans и many agents?»* citeturn800956search3turn988966search0turn795437search1turn795437search8turn301064search9

2. **Для вашего продукта “Obsidian + общий brain для Claude/Codex/Grok на восьми машинах” сам multi-agent/MCP/local-first тезис больше не уникален.** Basic Memory уже почти буквально продаёт Markdown brain + Obsidian-compatible local files + Claude/Codex/Cursor/ChatGPT + cross-device sync; Cognee явно продаёт one shared memory across MCP agents. Поэтому конкурентоспособный слой должен быть выше: **bitemporal facts, semantic merge/conflict resolution, provenance per assertion, trust/authority per agent/source, per-fact privacy scopes и reproducible history**. Именно такой набор всё ещё плохо закрыт в consumer/local-first category. citeturn795437search0turn988966search3turn583461search8

3. **Самая большая открытая ниша — не “ещё один second brain”, а federation of brains.** Пользователь имеет private memory; agent имеет working/episodic memory; команда имеет shared memory; компания — governed knowledge graph. Нужен policy layer, который определяет, что пересекает границы, кто владеет derived fact, как хранится provenance, кто может invalidate его и что происходит при уходе человека из компании. Glean подошёл ближе всех концепцией Enterprise Graph + personal graphs, а Sana показала спрос на один governed context layer для Claude/Gemini/Copilot/Slack, но **полноценного dual-layer employee-owned + company-owned temporal graph в публично изученном рынке нет**. Если ваш стек эволюционировать именно туда, это гораздо более дифференцированная позиция, чем generic personal RAG. citeturn239807search0turn239807search2turn224512search2

## SOURCES (safe_urls)
https://aclanthology.org/2026.acl-demo.27/
https://aclanthology.org/2026.acl-demo.27/?utm_source=chatgpt.com
https://arxiv.org/abs/2501.13956
https://arxiv.org/abs/2501.13956?utm_source=chatgpt.com
https://arxiv.org/abs/2502.12110
https://arxiv.org/abs/2502.12110?utm_source=chatgpt.com
https://aws.amazon.com/about-aws/whats-new/2026/10/agentcore-memory-consumption-based-pricing/
https://aws.amazon.com/about-aws/whats-new/2026/10/agentcore-memory-consumption-based-pricing/?utm_source=chatgpt.com
https://aws.amazon.com/bedrock/agentcore/pricing/?trk=public_post_comment-text
https://aws.amazon.com/bedrock/agentcore/pricing/?trk=public_post_comment-text&utm_source=chatgpt.com
https://bee.computer/blog/building-bee-at-amazon-how-the-wearable-ai-device-is-evolving
https://bee.computer/blog/building-bee-at-amazon-how-the-wearable-ai-device-is-evolving?utm_source=chatgpt.com
https://capacities.io/pricing
https://capacities.io/pricing?utm_source=chatgpt.com
https://cloud.google.com/products/gemini-enterprise-agent-platform/pricing?kgs=e2568ed5114fdaf9
https://cloud.google.com/products/gemini-enterprise-agent-platform/pricing?kgs=e2568ed5114fdaf9&utm_source=chatgpt.com
https://community.obsidian.md/
https://community.obsidian.md/?utm_source=chatgpt.com
https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted
https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted?utm_source=chatgpt.com
https://developers.openai.com/api/docs/guides/agents/sandboxes
https://developers.openai.com/api/docs/guides/agents/sandboxes?utm_source=chatgpt.com
https://developers.openai.com/cookbook/examples/agents_sdk/building_reliable_agents_memory_compaction
https://developers.openai.com/cookbook/examples/agents_sdk/building_reliable_agents_memory_compaction?utm_source=chatgpt.com
https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-memory.html?trk=article-ssr-frontend-pulse_little-text-block
https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-memory.html?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-types.html
https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-types.html?utm_source=chatgpt.com
https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank?hl=en
https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank?hl=en&utm_source=chatgpt.com
https://docs.cognee.ai/api-reference/introduction
https://docs.cognee.ai/api-reference/introduction?utm_source=chatgpt.com
https://docs.dev.hindsight.vectorize.io/enterprise/deployment/overview/
https://docs.dev.hindsight.vectorize.io/enterprise/deployment/overview/?utm_source=chatgpt.com
https://docs.glean.com/security/knowledge-graph
https://docs.glean.com/security/knowledge-graph?utm_source=chatgpt.com
https://docs.hindsight.vectorize.io/cloud/billing/
https://docs.hindsight.vectorize.io/cloud/billing/?utm_source=chatgpt.com
https://docs.letta.com/configuration/memory?trk=article-ssr-frontend-pulse_little-text-block
https://docs.letta.com/configuration/memory?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://docs.letta.com/v1-sdk?trk=article-ssr-frontend-pulse_little-text-block
https://docs.letta.com/v1-sdk?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://docs.moveworks.com/tool-studio/mcp-workspace/connect-a-servicenow-mcp-server
https://docs.moveworks.com/tool-studio/mcp-workspace/connect-a-servicenow-mcp-server?utm_source=chatgpt.com
https://dust.tt/home/pricing?refer=similarlabs_ai-agents_dust
https://dust.tt/home/pricing?refer=similarlabs_ai-agents_dust&utm_source=chatgpt.com
https://eu.plaud.ai/pages/plaud-notepin
https://eu.plaud.ai/pages/plaud-notepin?utm_source=chatgpt.com
https://get.mem.ai/pricing
https://get.mem.ai/pricing?utm_source=chatgpt.com
https://github.com/MemMachine/MemMachine
https://github.com/MemMachine/MemMachine?utm_source=chatgpt.com
https://github.com/MemTensor/MemOS
https://github.com/MemTensor/MemOS?utm_source=chatgpt.com
https://github.com/OpenTech-Lab/openmemory
https://github.com/OpenTech-Lab/openmemory?utm_source=chatgpt.com
https://github.com/WujiangXu/A-mem
https://github.com/WujiangXu/A-mem?utm_source=chatgpt.com
https://github.com/agiresearch/a-mem
https://github.com/agiresearch/a-mem?utm_source=chatgpt.com
https://github.com/ai-khoj/khoj
https://github.com/ai-khoj/khoj?utm_source=chatgpt.com
https://github.com/getzep
https://github.com/getzep?utm_source=chatgpt.com
https://github.com/langchain-ai/langmem/blob/main/README.md
https://github.com/langchain-ai/langmem/blob/main/README.md?utm_source=chatgpt.com
https://github.com/letta-ai/letta
https://github.com/letta-ai/letta?utm_source=chatgpt.com
https://github.com/mcp/basicmachines-co/basic-memory
https://github.com/mcp/basicmachines-co/basic-memory?utm_source=chatgpt.com
https://github.com/mem0ai/mem0
https://github.com/mem0ai/mem0/blob/main/docs/open-source/configuration.mdx
https://github.com/mem0ai/mem0/blob/main/docs/open-source/configuration.mdx?utm_source=chatgpt.com
https://github.com/mem0ai/mem0/blob/main/docs/open-source/overview.mdx
https://github.com/mem0ai/mem0/blob/main/docs/open-source/overview.mdx?utm_source=chatgpt.com
https://github.com/mem0ai/mem0/blob/main/docs/platform/features/graph-memory.mdx
https://github.com/mem0ai/mem0/blob/main/docs/platform/features/graph-memory.mdx?utm_source=chatgpt.com
https://github.com/mem0ai/mem0?utm_source=chatgpt.com
https://github.com/mem0ai/openmemory/blob/main/openmemory-archive/README.md
https://github.com/mem0ai/openmemory/blob/main/openmemory-archive/README.md?utm_source=chatgpt.com
https://github.com/orgs/getzep/repositories
https://github.com/orgs/getzep/repositories?utm_source=chatgpt.com
https://github.com/orgs/mem0ai/repositories
https://github.com/orgs/mem0ai/repositories?utm_source=chatgpt.com
https://github.com/reorproject/reor/blob/main/package.json
https://github.com/reorproject/reor/blob/main/package.json?utm_source=chatgpt.com
https://github.com/reorproject/reor/releases
https://github.com/reorproject/reor/releases?utm_source=chatgpt.com
https://github.com/screenpipe/screenpipe
https://github.com/screenpipe/screenpipe/blob/main/README.md
https://github.com/screenpipe/screenpipe/blob/main/README.md?utm_source=chatgpt.com
https://github.com/screenpipe/screenpipe?utm_source=chatgpt.com
https://github.com/supermemoryai
https://github.com/supermemoryai?utm_source=chatgpt.com
https://github.com/topoteretes/cognee/
https://github.com/topoteretes/cognee/?utm_source=chatgpt.com
https://github.com/topoteretes/cognee/releases
https://github.com/topoteretes/cognee/releases?utm_source=chatgpt.com
https://github.com/vectorize-io/hindsight/blob/main/hindsight-docs/blog/2026-03-23-agent-memory-benchmark.mdx
https://github.com/vectorize-io/hindsight/blob/main/hindsight-docs/blog/2026-03-23-agent-memory-benchmark.mdx?utm_source=chatgpt.com
https://github.com/vectorize-io/hindsight?refer=similarlabs_ai-agents_vectorize-io
https://github.com/vectorize-io/hindsight?refer=similarlabs_ai-agents_vectorize-io&utm_source=chatgpt.com
https://help.getzep.com/api-logging
https://help.getzep.com/api-logging?utm_source=chatgpt.com
https://help.getzep.com/faq
https://help.getzep.com/faq?utm_source=chatgpt.com
https://help.getzep.com/graph-overview
https://help.getzep.com/graph-overview?utm_source=chatgpt.com
https://help.getzep.com/v2/graphiti/getting-started/overview
https://help.getzep.com/v2/graphiti/getting-started/overview?utm_source=chatgpt.com
https://help.getzep.com/v3/changelog/2026/7/9
https://help.getzep.com/v3/changelog/2026/7/9?utm_source=chatgpt.com
https://help.getzep.com/v4/overview
https://help.getzep.com/v4/overview?utm_source=chatgpt.com
https://help.openai.com/en/articles/8590148-memory-in-chatgpt
https://help.openai.com/en/articles/8590148-memory-in-chatgpt?utm_source=chatgpt.com
https://heptabase.com/pricing
https://heptabase.com/pricing?utm_source=chatgpt.com
https://hindsight.vectorize.io/cloud-enterprise/overview
https://hindsight.vectorize.io/cloud-enterprise/overview?utm_source=chatgpt.com
https://investor.workday.com/news-and-events/press-releases/news-details/2026/Introducing-Sana-from-Workday-Superintelligence-for-Work-That-Finds-Answers-Takes-Action-and-Automates-Workflows/
https://investor.workday.com/news-and-events/press-releases/news-details/2026/Introducing-Sana-from-Workday-Superintelligence-for-Work-That-Finds-Answers-Takes-Action-and-Automates-Workflows/?utm_source=chatgpt.com
https://investor.workday.com/news-and-events/press-releases/news-details/2026/Workday-to-Bring-Sana-Everywhere-Employees-Work-Including-Claude-Gemini-Enterprise-Microsoft-Copilot-Slack-and-Teams/default.aspx
https://investor.workday.com/news-and-events/press-releases/news-details/2026/Workday-to-Bring-Sana-Everywhere-Employees-Work-Including-Claude-Gemini-Enterprise-Microsoft-Copilot-Slack-and-Teams/default.aspx?utm_source=chatgpt.com
https://langchain-ai.github.io/langmem/
https://langchain-ai.github.io/langmem/?utm_source=chatgpt.com
https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/work-iq
https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/work-iq?utm_source=chatgpt.com
https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/memory-overview
https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/memory-overview?utm_source=chatgpt.com
https://mem0.ai/pricing
https://mem0.ai/pricing?utm_source=chatgpt.com
https://mem0.ai/research
https://mem0.ai/research?utm_source=chatgpt.com
https://mem0.ai/series-a
https://mem0.ai/series-a?utm_source=chatgpt.com
https://newsroom.workday.com/2025-11-04-Workday-Completes-Acquisition-of-Sana?es_id=945b4f8d79
https://newsroom.workday.com/2025-11-04-Workday-Completes-Acquisition-of-Sana?es_id=945b4f8d79&utm_source=chatgpt.com
https://next.reflect.app/
https://next.reflect.app/?utm_source=chatgpt.com
https://obsidian.md/help/plugins
https://obsidian.md/help/plugins?utm_source=chatgpt.com
https://onyx.app/pricing
https://onyx.app/pricing?utm_source=chatgpt.com
https://onyx.app/search
https://onyx.app/search?utm_source=chatgpt.com
https://openai.com/index/chatgpt-memory-dreaming/
https://openai.com/index/chatgpt-memory-dreaming/?utm_source=chatgpt.com
https://reflect.app/?trk=article-ssr-frontend-pulse_little-text-block
https://reflect.app/?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://slack.com/help/articles/39044407124755-Set-up-and-manage-Slack-enterprise-search
https://slack.com/help/articles/39044407124755-Set-up-and-manage-Slack-enterprise-search?utm_source=chatgpt.com
https://supermemory.ai/blog/an-update-to-supermemory/
https://supermemory.ai/blog/an-update-to-supermemory/?utm_source=chatgpt.com
https://supermemory.ai/pricing/
https://supermemory.ai/pricing/?utm_source=chatgpt.com
https://support.anthropic.com/en/articles/10185728-understanding-claude-s-personalization-features
https://support.anthropic.com/en/articles/10185728-understanding-claude-s-personalization-features?utm_source=chatgpt.com
https://support.anthropic.com/en/articles/11473015-retrieval-augmented-generation-rag-for-projects
https://support.anthropic.com/en/articles/11473015-retrieval-augmented-generation-rag-for-projects?utm_source=chatgpt.com
https://support.anthropic.com/en/articles/9450526-how-can-i-export-my-claude-data
https://support.anthropic.com/en/articles/9450526-how-can-i-export-my-claude-data?utm_source=chatgpt.com
https://support.anthropic.com/en/articles/9519189-project-visibility-and-sharing
https://support.anthropic.com/en/articles/9519189-project-visibility-and-sharing?utm_source=chatgpt.com
https://support.atlassian.com/rovo/docs/rovo-usage-limits/
https://support.atlassian.com/rovo/docs/rovo-usage-limits/?utm_source=chatgpt.com
https://support.writer.com/articles/6965386512-how-to-create-and-manage-a-knowledge-graph
https://support.writer.com/articles/6965386512-how-to-create-and-manage-a-knowledge-graph?utm_source=chatgpt.com
https://tana.inc/pricing
https://tana.inc/pricing?utm_source=chatgpt.com
https://vpn.luca-server.com/px/aHR0cHM6Ly9naXRodWIuY29tL2xhbmdjaGFpbi1haS9sYW5nbWVt
https://vpn.luca-server.com/px/aHR0cHM6Ly9naXRodWIuY29tL2xhbmdjaGFpbi1haS9sYW5nbWVt?utm_source=chatgpt.com
https://wac-cdn-a.atlassian.com/resources/usage
https://wac-cdn-a.atlassian.com/resources/usage?utm_source=chatgpt.com
https://writer.com/plans/
https://writer.com/plans/?utm_source=chatgpt.com
https://www.aboutamazon.com/news/devices/amazon-personal-ai-bee-frontier-bee-eclipse
https://www.aboutamazon.com/news/devices/amazon-personal-ai-bee-frontier-bee-eclipse?trk=article-ssr-frontend-pulse_little-text-block
https://www.aboutamazon.com/news/devices/amazon-personal-ai-bee-frontier-bee-eclipse?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://www.aboutamazon.com/news/devices/amazon-personal-ai-bee-frontier-bee-eclipse?utm_source=chatgpt.com
https://www.atlassian.com/software/rovo/connectors
https://www.atlassian.com/software/rovo/connectors?utm_source=chatgpt.com
https://www.cognee.ai/cognee-raises-seven-million-five-hundred-thousand-dollars-seed
https://www.cognee.ai/cognee-raises-seven-million-five-hundred-thousand-dollars-seed?utm_source=chatgpt.com
https://www.cognee.ai/pricing
https://www.cognee.ai/pricing?utm_source=chatgpt.com
https://www.cognee.ai/product/cloud
https://www.cognee.ai/product/cloud?utm_source=chatgpt.com
https://www.cognee.ai/trust
https://www.cognee.ai/trust?utm_source=chatgpt.com
https://www.getguru.com/pricing
https://www.getguru.com/pricing?utm_source=chatgpt.com
https://www.getguru.com/solutions/ai-enterprise-search
https://www.getguru.com/solutions/ai-enterprise-search?utm_source=chatgpt.com
https://www.glean.com/enterprise-context/enterprise-graph
https://www.glean.com/enterprise-context/enterprise-graph?utm_source=chatgpt.com
https://www.glean.com/enterprise-search
https://www.glean.com/enterprise-search?utm_source=chatgpt.com
https://www.graphlit.com/?trk=article-ssr-frontend-pulse_little-text-block
https://www.graphlit.com/?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://www.graphlit.com/pricing
https://www.graphlit.com/pricing?utm_source=chatgpt.com
https://www.limitless.ai/?trk=organization_guest_main-feed-card-text
https://www.limitless.ai/?trk=organization_guest_main-feed-card-text&utm_source=chatgpt.com
https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/
https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/?utm_source=chatgpt.com
https://www.moveworks.com/us/en/company/news/press-releases/servicenow-otto-creates-the-unified-ai-experience-for-the-enterp
https://www.moveworks.com/us/en/company/news/press-releases/servicenow-otto-creates-the-unified-ai-experience-for-the-enterp?utm_source=chatgpt.com
https://www.moveworks.com/us/en/platform/enterprise-search
https://www.moveworks.com/us/en/platform/enterprise-search?utm_source=chatgpt.com
https://www.notion.com/help/enterprise-search
https://www.notion.com/help/enterprise-search?utm_source=chatgpt.com
https://www.notion.com/product/agents
https://www.notion.com/product/agents?utm_source=chatgpt.com
https://www.notion.com/product/enterprise-search
https://www.notion.com/product/enterprise-search?utm_source=chatgpt.com
https://www.omi.me/pages/pendant
https://www.omi.me/pages/pendant?utm_source=chatgpt.com
https://www.omi.me/pages/pricing
https://www.omi.me/pages/pricing?utm_source=chatgpt.com
https://www.personal.ai/memory
https://www.personal.ai/memory?utm_source=chatgpt.com
https://www.plaud.ai/products/plaud-notepin
https://www.plaud.ai/products/plaud-notepin?utm_source=chatgpt.com
https://www.recall.it/about
https://www.recall.it/about?utm_source=chatgpt.com
https://www.recall.it/pricing?trk=article-ssr-frontend-pulse_little-text-block
https://www.recall.it/pricing?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com