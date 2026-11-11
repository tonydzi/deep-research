# DR26-11-03-HUB-01-0950 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Карта рынка «второго мозга» и общей памяти (ноябрь 2026) и место Zep AI

Zep к ноябрю 2026 года больше не просто «слой памяти для агентов». Компания называет себя «Context Lake for AI agents». С августа 2026 года её Memory MCP Server даёт каждому сотруднику одну память на Claude, ChatGPT, Cursor и собственные агенты, а общие графы компании подключаются через политики ABAC. По сути Zep первым из инфраструктурных игроков сделал продукт «личная память сотрудника + память компании в одном хранилище». Не хватает только переносимости памяти при увольнении и федерации между организациями: этого нет ни у кого.

## TL;DR
- **Конкуренты Zep делятся на три группы.** Первая — специализированные стартапы: Mem0 (66,2k звёзд, $24M), Letta, Cognee (seed $7,5M 19.02.2026 по Startup.eu, Tracxn и Dealroom; часть СМИ пишет €7,5M), Supermemory, Hindsight, MemMachine, MemOS. Вторая — гиперскейлеры с управляемой памятью: AWS AgentCore Memory, Vertex AI Memory Bank (GA с декабря 2025), Cloudflare Agent Memory (beta с апреля 2026). Третья — владельцы «графа компании», которые в 2026 году открыли графы агентам через MCP: Glean (Enterprise Graph + Personal Graph; $300M ARR по собственному пресс-релизу Glean от 28.05.2026 — через 15 месяцев после $100M), Microsoft Work IQ, Atlassian Teamwork Graph. У Zep три отличия: битемпоральный граф с provenance, собственная графовая СУБД Konig и развёртывание в VPC/BYOC с ABAC.
- **Ниша «одна память на все ИИ-инструменты одного человека» в 2026 году уже не пуста.** В ней работают Zep Memory MCP, basic-memory, OpenMemory, mcp-memory-service, Supermemory, Hindsight; Codex даже импортирует память из Claude Code. Наш стек (Markdown/Obsidian + SQLite + MCP + несколько агентов) почти дословно повторяет basic-memory и mcp-memory-service. Уникальными у нас могут быть только битемпоральные факты с provenance «какой агент/машина записал» и синхронизация между машинами без облака.
- **Пусты по-прежнему три ниши: переносимость личной памяти сотрудника при увольнении, федерация личных мозгов между организациями и «второй мозг семьи».** Улика — ни один из изученных вендоров (Zep, Glean, Microsoft, Atlassian, Anthropic) не описывает экспорт личного слоя уходящему сотруднику. Они описывают только удаление (RTBF) или хранение по политике организации.

## Поправки к блоку «что мы уже знаем»

| Утверждение | Статус на 03.11.2026 | Что верно сейчас |
|---|---|---|
| Zep: Flex $125 / Flex Plus $375 в месяц | Верно | Бесплатный тариф — 10 000 кредитов в месяц. Flex: 50 000 кредитов, сверх — $25 за 10 000. Flex Plus: 200 000 кредитов, сверх — $75 за 40 000. 1 кредит = эпизод до 350 байт. Поиск, хранение, пользователи и треды не тарифицируются. При годовой оплате — $104 и $312 (скидка 17%) |
| Zep Emerging $13k/год | Не подтверждено | На текущей странице цен Zep такого тарифа нет: только Free / Flex / Flex Plus / Enterprise (custom). Вероятно, устарело |
| Позиционирование Zep | Устарело | Теперь «Context Lake for AI agents» и «unified context layer for enterprise data». Под капотом собственная графовая СУБД Konig: миллионы графов (по одному на пользователя, команду или проект), активные — в памяти, остальные — в объектном хранилище, p95 < 200 мс |
| Новые продукты Zep 2026 | Добавить | Zep v4 «Context API for agents» (14.10.2026): навигация по графу, Ingestion Traces, стабильные коды ошибок. Гиперрёбра — факты о 3+ сущностях (15.10.2026). Memory MCP Server GA (19.08.2026). Плагин для Claude Code, Codex и Cursor (30.07.2026). Observations, Smart Context Assembly, ABAC, provenance, фреймворк оценки и CLI zepctl (март 2026) |
| Open source у Zep | Уточнить | Zep Community Edition больше не поддерживается (с 2025 года). Open source — только Graphiti (Apache-2.0, ~31,3k звёзд на 28.09.2026) |
| Деньги Zep | Уточнить | Публично раскрыто ~$2,3M (YC W24 и др. по CB Insights и PitchBook). Gritt.io отмечает новых инвесторов в январе 2026 года, но сумма и раунд не подтверждены первоисточником |
| Mem0: 41k+ звёзд | Устарело | 66 223 звезды на 28.09.2026, SDK v2.2.1, Apache-2.0. Раунд $24M (Seed + Series A, Basis Set, Peak XV, GitHub Fund, YC) подтверждён |
| Cognee: $1.5M | Устарело | Seed $7,5M 19.02.2026 (лид Pebblebed, далее 42CAP) по Startup.eu, Tracxn и Dealroom; часть СМИ указывает €7,5M — валюту стоит перепроверить; ~31,1k звёзд |
| Supermemory: $3M pre-seed | Верно с уточнением | ~$3M (лид Susa Ventures называет $2,6M), ~31,5k звёзд, ядро под MIT |
| Letta: MemFS | Верно | MemFS — файловая система памяти на git и Markdown. 16.03.2026 Letta объявила «next phase» и отказалась от серверных инструментов памяти. Код переехал в letta-code. Последний известный раунд — $10M seed (сентябрь 2024) |
| Glean: $300M ARR | Верно | По пресс-релизу Glean в Business Wire от 28.05.2026, компания достигла $300M ARR «just 15 months after reaching $100 million ARR»; оценка $7,2B — по Series F на $150M (июнь 2025). Enterprise Graph + Personal Graph |

## Key Findings

1. **Zep сдвинулся из «памяти пользователя чат-бота» в «память сотрудника и компании».** Memory MCP Server даёт каждому человеку один пользовательский граф, общий для Claude, Claude Cowork, ChatGPT, Cursor и агентов на SDK Zep. Каталог graph directory открывает общие standalone-графы организации. Доступ определяется политиками UserGroup ABAC по принципу default-deny. Записи разрешаются отдельно для каждого подключения, аварийный выключатель (kill switch) действует на весь аккаунт. Сервис доступен на managed cloud и BYOC. Так на практике выглядят «два слоя одного графа», о которых спрашивает п.5.
2. **Бенчмарки превратились в маркетинговую войну, и почти все цифры self-reported.** Zep заявляет 94,7% на LoCoMo. Mem0 — 92,5% на LoCoMo и 94,4% на LongMemEval (собственный отчёт 01.04.2026). Hindsight — 94,6% на LongMemEval-S. MemMachine — 91,69% на LoCoMo (arXiv 2604.04853). MemOS — 75,80 на LoCoMo. Ни одна цифра не воспроизведена независимой стороной на одинаковых настройках. Mem0 в своей статье прогнал Zep с неверной конфигурацией. Исправленный результат Zep в блоге «Lies, Damn Lies, & Statistics» (Daniel Chalef, Preston Rasmussen) — «75.14% +/- 0.17, with Zep outperforming Mem0 by 10%» против 68,44% у Mem0 Graph; CTO Mem0 в issue #5 репозитория getzep/zep-papers даёт Zep лишь «58.44 % ± 0.20».
3. **Гиперскейлеры и владельцы рабочих графов сжимают рынок с двух сторон.** AWS AgentCore Memory, Vertex AI Memory Bank и Cloudflare Agent Memory продают память как дешёвую управляемую услугу. Glean, Microsoft Work IQ и Atlassian (150 млрд связей в Teamwork Graph) открыли свои графы внешним агентам через MCP и API.
4. **Сегмент персональных устройств-диктофонов консолидировался вокруг бигтеха.** По Reuters (5 декабря 2025), Meta купила Limitless — производителя кулона, записывающего и транскрибирующего разговоры; условия не раскрыты, до сделки Limitless привлекла более $33M (в том числе от Sam Altman и a16z), команда перешла в wearables-подразделение Reality Labs, а Rewind закрыт. Amazon купил Bee и 15.10.2026 выпустил Bee Frontier ($74,99) и Bee Eclipse ($99,99) с интеграцией Alexa+. Независимым крупным игроком остался Plaud: по Bloomberg (16.06.2026), у него более 2M пользователей (США, Япония и Европа дают около 80% бизнеса) и ожидание $500M продаж в 2026 году; в сентябре 2026 CEO Nathan Xu в WSJ назвал уже 2,5M+ пользователей, $100M ARR и оценку выше $1B.
5. **Ни у кого нет модели «личная память уходит вместе с сотрудником».** Все корпоративные продукты закрепляют личную память за тенантом организации.

## Details

### 1. Память агентов (agent memory layer)

| Игрок | Модель памяти | OSS / лицензия | Деплой | Цена | Деньги и тяга | Бенчмарки (кто мерил) |
|---|---|---|---|---|---|---|
| **Zep** | Битемпоральный граф контекста (Graphiti) поверх Konig; гиперрёбра; provenance | Graphiti — Apache-2.0; сервис закрытый | Cloud, BYOK, BYOC/VPC | Free 10k кредитов; $125 / $375 в месяц; Enterprise — custom | ~$2,3M раскрыто; Graphiti ~31,3k★; заявляет «Fortune 500 deployments» | LoCoMo 94,7%, LongMemEval-S 71,2% (GPT-4o), DMR 94,8% — self-reported |
| **Mem0** | Вектор + граф + KV; извлечённые факты; при конфликте факты самоправятся | Apache-2.0 | Cloud, self-host | $19 Starter, $249 Pro, $999 Scale | $24M; 66,2k★ | LoCoMo 92,5%, LongMemEval 94,4% — self-reported |
| **Letta** | Агент сам управляет памятью; MemFS (git + Markdown) | Apache-2.0 | Cloud, self-host | Free (3 агента); $20/мес | $10M seed (2024, оценка $70M post-money); ~25,3k★ + letta-code ~3,8k★ | LoCoMo 74,0% (по пересказу Mnemoverse; источник — бенчмарк Letta) — self-reported |
| **Cognee** | Граф знаний + векторы, «память как граф» | Apache-2.0 | Self-host, cloud | Freemium, $1 за 1M токенов (по агрегатору) | $7,5M seed (19.02.2026, по Startup.eu, Tracxn и Dealroom; часть СМИ — €7,5M); ~31,1k★ | нет данных |
| **Supermemory** | Управляемый context engine, профили пользователей, явное забывание | Ядро MIT | Cloud + OSS | Free tier | ~$3M; ~31,5k★ | нет независимых данных |
| **Hindsight (Vectorize)** | 4 сети: мировые факты, опыт агента, сводки по сущностям, убеждения/ментальные модели; 4 вида поиска (семантический, ключевой, графовый, темпоральный) | MIT, Postgres | Self-host, Hindsight Cloud | Cloud — оплата по использованию | Раунды нет данных | LongMemEval 91,4% (arXiv 2512.12818, соавторы — Virginia Tech и Washington Post), сейчас заявлено 94,6% LongMemEval-S — self-reported с участием внешних соавторов |
| **MemMachine (MemVerge)** | Эпизодическая память в графе + профиль в SQL + рабочая память; хранит исходные эпизоды | Apache-2.0 | Self-host, AWS Marketplace, cloud | OSS бесплатно; enterprise — нет данных | MemVerge как компания ~$43,5M (вторичный источник) | LoCoMo 84,87% (09.2025) → 91,69% (arXiv, 2026) — self-reported |
| **MemOS (MemTensor)** | «ОС памяти», уровни L1–L3, MemCube, переиспользование навыков | Apache-2.0 | Self-host + cloud API | нет данных | 12,2k★ | LoCoMo 75,80 — self-reported |
| **Memobase** | Профильная память пользователя | OSS (детали — нет данных) | — | нет данных | Внешнего финансирования нет (вторичный источник) | LoCoMo 85% — self-reported, вторичный источник |
| **AWS AgentCore Memory** | Краткосрочная (события) + долгосрочная (стратегии извлечения) | Закрытый | AWS | Кратко: с 06.10.2026 $1/GB ingest, $0,10/GB-мес, $0,20/GB retrieval. Долго: $0,75 за 1000 записей в месяц (встроенные стратегии), $0,50 за 1000 запросов | — | нет данных |
| **Vertex AI Memory Bank** | Извлекаемые «memories» + Sessions | Закрытый | Google Cloud | $0,25 за 1000 memories в месяц, $0,50 за 1000 извлечений (на запуске). Есть сообщения о переходе на GiB/vCPU с 01.09.2026 — не подтверждено | GA 16.12.2025 | нет данных |
| **Cloudflare Agent Memory** | Извлечение структурированных воспоминаний; 5 каналов поиска + RRF; общие профили памяти | Закрытый | Cloudflare edge (Durable Objects) | Не объявлена | Private beta с 17.04.2026 | нет данных |
| **Microsoft Work IQ API** | Семантический индекс + персональная память + навыки в тенанте M365 | Закрытый | Тенант M365 | В составе лицензий Copilot (детали — нет данных) | GA 16.06.2026 | нет данных |
| **OpenAI (ChatGPT / Codex)** | ChatGPT: фоновое построение профиля («Dreaming», с 04.06.2026). Codex: memory v2 в ~/.codex/memories_v2 | ChatGPT закрыт; Codex CLI — Apache-2.0 | Облако / локально | В подписке | — | Своя оценка OpenAI: точность фактов 41,5% → 82,8% |
| **Anthropic (Claude / Claude Code)** | Claude: память аккаунта и проектов. Claude Code: Auto Memory — Markdown-файлы в «внутренний путь лаборатории»&lt;project&gt;/memory/ | Закрытый | Облако / локально | В подписке | — | нет данных |
| LangMem, Graphlit, A-MEM, Redis Agent Memory Server | — | — | — | нет данных (в этом исследовании не проверялись по первоисточникам) | — | — |

**Что это значит.** По модели памяти рынок раскололся на три лагеря. «Факты-экстракты в векторе» — Mem0, Memobase, гиперскейлеры. «Граф знаний» — Zep, Cognee, MemMachine (частично), Hindsight. «Файлы, которыми управляет сам агент» — Letta MemFS, Claude Code Auto Memory, basic-memory. Явную двойную временную ось (когда факт был истинен / когда попал в граф) с нативными point-in-time запросами в хранилище предлагает только Zep. Hindsight заявляет темпоральный поиск, но битемпоральной модели в найденных материалах нет.

### 2. Общая память нескольких агентов и вендоров

| Продукт | Кто читает и пишет | Хранилище | Конфликты записи | Права доступа | Provenance |
|---|---|---|---|---|---|
| **Zep Memory MCP Server** (GA 19.08.2026) | Claude, Claude Cowork, ChatGPT, Cursor, любой MCP-клиент + агенты на SDK — один граф пользователя | Граф Zep (cloud/BYOC) | Битемпоральная инвалидация фактов (старый факт помечается, а не стирается) | SSO (Google Workspace / OIDC); пользователь видит только свой граф; общие графы — через UserGroup ABAC, default-deny; запись разрешается на каждое подключение отдельно; kill switch | Факты связаны с исходными эпизодами; поиск можно фильтровать по источнику |
| **basic-memory** | Claude, ChatGPT, Codex, Cursor, Obsidian, VS Code | Markdown-файлы + индекс в SQLite; поиск полнотекстовый, семантический и по графу | Правки видны в редакторе в реальном времени (детали слияния — нет данных) | Cloud: $19/мес за рабочее пространство до 2 человек | Через историю файлов (детали — нет данных) |
| **OpenMemory MCP (Mem0)** | Claude Desktop, Cursor, Windsurf и др. | Локально: Qdrant + Postgres | Логика Mem0 (самоправка) | Локально, без облака | нет данных |
| **mcp-memory-service** (v11.11.0) | Claude Code, Claude Desktop, Codex CLI, OpenCode, claude.ai (Remote MCP); 13–25+ клиентов | sqlite-vec локально, гибрид с Cloudflare | Автономная консолидация; SSE-события при записи | OAuth; профили не изолированы по проектам | Метаданные источника (по спецификации SHODH) |
| **Cognee MCP** (API mode) | Cursor, Claude Desktop, Cline, Continue и др. — общий граф команды | Граф знаний | нет данных | нет данных | нет данных |
| **Cloudflare Agent Memory** | Несколько агентов и людей через общий профиль памяти | Edge | нет данных | Через профиль | нет данных |
| **Hindsight coding agents** | 20+ агентных оболочек (Claude Code, Codex CLI, Cursor CLI, opencode, Copilot CLI) | Банк памяти на репозиторий | нет данных | Локально | Строится из истории git и прошлых сессий |
| **Встроенная память вендоров** | Claude Code Auto Memory — только Claude Code, только эта машина. Cursor Memories — на человека и проект. Codex memory v2 — разовый импорт из Claude Code | Локальные файлы | Нет | Нет | Нет |
| Anthropic memory MCP reference server; общие блоки Letta (shared blocks) | — | — | нет данных (не проверено в этом исследовании) | — | — |

**Что это значит.** Шумный лозунг 2026 года — «one memory across all your AI tools». Его продают как минимум шесть продуктов, так что ниша занята. Различаются они по трём осям. Первая — разрешение конфликтов: только у Zep есть формальная битемпоральная модель. Вторая — права: корпоративный SSO и ABAC есть только у Zep. Третья — provenance «кто записал»: явной атрибуции записи конкретному агенту или вендору в найденной документации нет ни у кого. Zep отслеживает происхождение до эпизода-источника, а не до агента-писателя.

### 3. Личный второй мозг

| Продукт | Помнит сам или ведётся вручную | OSS / лицензия | Цена | Статус на 11.2026 |
|---|---|---|---|---|
| **Limitless / Rewind** | Автоматически (кулон, запись экрана) | Закрытый | — | Куплен Meta 05.12.2025. Продажи Pendant остановлены. Rewind перестал записывать 19.12.2025. В ЕС и Великобритании сервис отключён. Поддержка кулона ~1 год, то есть фактически до конца 2026 года |
| **Bee (Amazon)** | Автоматически: включаемая запись и сводки разговоров | Закрытый | Frontier $74,99; Eclipse $99,99 | Куплен Amazon. 15.10.2026 вышли новые устройства с Alexa+ и покупками на Amazon.com |
| **Plaud (NotePin)** | Полуавтоматически: запись по кнопке, ИИ-сводки | Закрытый | $159 + Pro $99,99/год | 2M+ пользователей, цель $500M продаж в 2026 году (Bloomberg); IPO в США планируется около 2028 года (WSJ через PYMNTS) |
| **Omi (Based Hardware)** | Автоматически: постоянно слушающий кулон | Open source (железо, прошивка, приложение; лицензия — нет данных) | $179 (акция $129); Unlimited $29,99/мес | Активен; финансирование — нет данных |
| **Screenpipe** | Автоматически: экран и аудио локально | С июня 2026 source-available (Screenpipe Commercial License); старые релизы под MIT | Pro ~$20/мес | В батче YC S26 |
| **Mem.ai** | Вручную + ИИ-организация | Закрытый | нет данных | Mem 2.0 (01.10.2025) |
| **Recall (recall.it)** | Вручную: сохраняешь контент, ИИ сам связывает его в граф | Закрытый | Free; Plus $10; Max $38/мес | Активен; ~$1,5M |
| **Khoj** | Индексирует ваши документы | AGPL-3.0, ~37,8k★ | Self-host бесплатно | Khoj Cloud закрыт 15.04.2026; остался только self-host |
| **Reor** | Вручную + автосвязи | AGPL-3.0 | Бесплатно | Репозиторий заархивирован 07.03.2026 |
| **Tana** | Вручную + ИИ-агент для встреч | Закрытый | нет данных | $25M всего (Series A $14M, 02.2025) |
| **Heptabase** | Вручную (карточки, доски) | Закрытый | Pro $8,99–11,99/мес | Активен |
| **Capacities** | Вручную (типизированные объекты) | Закрытый | Free; ~$10/мес | Активен |
| **Reflect** | Вручную, E2E-шифрование | Закрытый | $10/мес | Активен |
| **ChatGPT / Claude memory** | Автоматически (профиль из диалогов) | Закрытый | В подписке | ChatGPT «Dreaming» (06.2026). Claude: импорт памяти из других ассистентов (экспериментально); 25.08.2026 память чата и Cowork объединена |
| Personal.ai, Notion AI, плагины ИИ для Obsidian | — | — | нет данных | нет данных |

**Что это значит.** «Помнят за человека» автоматически только носимые устройства, запись экрана и встроенная память ChatGPT/Claude. Самые сильные независимые устройства были поглощены бигтехом. Все PKM-приложения (Tana, Heptabase, Capacities, Reflect, Mem) по-прежнему требуют ручного ведения. Открытые локальные инструменты либо умирают (Reor), либо уходят от чистого open source (Screenpipe, Khoj Cloud).

### 4. Второй мозг компании

| Продукт | Граф компании или поиск | Память | Масштаб / деньги | Открытость агентам |
|---|---|---|---|---|
| **Glean** | Граф: Enterprise Graph (люди, команды, клиенты, проекты, процессы) + Personal Graph на каждого сотрудника, учитывает права доступа | Enterprise memory + personal memory; Agentic Engine 2 сохраняет выученное в оба графа | $300M ARR (05.2026), оценка $7,2B | API, агенты |
| **Microsoft 365 Copilot / Work IQ** | Граф: семантический индекс, оргструктура, паттерны совместной работы | Персональная память (явная, выведенная, инструкции); рабочие пространства внутри тенанта | Work IQ API GA 16.06.2026 | API, MCP-серверы со срезами данных M365 |
| **Atlassian Rovo** | Граф: Teamwork Graph, 150 млрд объектов и связей; Code Context | Неявная (из графа) + явная (видимая, редактируемая) | Rovo используют 75% компаний Fortune 500 (заявление Atlassian) | Teamwork Graph CLI + Rovo MCP Server (open beta с 06.05.2026); интеграция с Teams/Copilot |
| **Dust** | Платформа агентов | Agent Memory: персональные графы знаний на пользователя, opt-in (08.2025, вторичный источник) | $21,5M | нет данных |
| **Zep (Context Lake)** | Граф: миллионы графов (пользователь, клиент, команда, тема) | Битемпоральная | См. выше | SDK, MCP |
| Notion Enterprise, Guru, Slack AI, Onyx, Sana (Workday), Hebbia, Moveworks, Writer Knowledge Graph | — | нет данных (в этом исследовании не проверялись) | — | — |

**Что это значит.** Граф компании, а не поиск по документам, строят Glean, Microsoft и Atlassian. С 2026 года все трое сделали ставку на то, чтобы их граф стал контекстом для любых агентов. Майк Кэннон-Брукс (тогда co-CEO Atlassian) в Founders' Keynote на Team '26 в Анахайме сказал прямо: «The real moat is your institutional memory — every plan, document, and decision your teams have ever made», а по данным Atlassian в Teamwork Graph «more than 150 billion connections». Отличие Zep от этой тройки в том, что Zep строит граф из любых данных клиента и не привязан к одному набору SaaS-приложений. Ещё одно отличие: в найденных материалах тройки нет битемпоральной модели фактов.

### 5. Слияние личного и корпоративного

| Продукт | Личный слой | Слой компании | Один граф / два слоя? | Что при увольнении |
|---|---|---|---|---|
| **Zep Memory MCP** | Граф пользователя, привязанный к SSO-личности | Общие standalone-графы через graph directory + ABAC | Да: одно хранилище, два типа графов, единые политики | Удаление пользователя одной командой zepctl (RTBF); экспорт личного слоя сотруднику — нет данных |
| **Glean** | Personal Graph (приватный, на пользователя) | Enterprise Graph | Да: «connects enterprise graph to personal graphs» | нет данных |
| **Microsoft Work IQ** | Персональная память и личный рабочий граф, по умолчанию приватны (по описанию партнёра Microsoft) | Семантический индекс тенанта | Да, внутри тенанта | Данные остаются в тенанте; переноса нет (нет данных о функции) |
| **Atlassian Rovo** | Явная и неявная память пользователя | Teamwork Graph | Да | нет данных |
| **Claude Team/Enterprise** | Память участника | Проекты организации | Частично | Память подчиняется политике хранения организации и попадает в админ-экспорт |

**Ответ прямо:** продукт, где личная память агента сотрудника и память компании — два слоя одного графа, **существует**: Zep (с 19.08.2026), Glean и Microsoft Work IQ. Продукта, где личный слой **принадлежит сотруднику** и уходит с ним при увольнении (или объединяется между организациями), **не найдено**. У всех владелец — тенант работодателя.

### 6. Сравнительная таблица с Zep

| Игрок | Сегмент | Чем похож на Zep | Чем отличается |
|---|---|---|---|
| Mem0 | Агенты | Управляемый слой памяти + OSS + MCP (OpenMemory) | Вектор + граф без битемпоральности; дешевле на входе ($19 против $125); в 10 раз больше денег и звёзд |
| Letta | Агенты | Долгая память агентов | Агент сам управляет памятью (MemFS), а не сервис строит граф; это runtime агентов |
| Cognee | Агенты / компания | Граф знаний, OSS, MCP для команд | Нет битемпоральности (нет данных); open-core из ЕС; нет SOC2/BYOC (нет данных) |
| Supermemory | Агенты / личный | API памяти, MCP, плагины для кодинга | Ориентирован на разговорную память; MIT; нет корпоративного ABAC (нет данных) |
| Hindsight | Агенты | Граф + темпоральный поиск, MCP, облако | MIT и Postgres; «обучение на опыте»; лидирует на LongMemEval по собственным данным |
| MemMachine | Агенты | Граф эпизодов + профиль | Хранит исходные эпизоды без LLM-извлечения; Apache-2.0; продаётся через AWS Marketplace |
| MemOS | Агенты | Многоуровневая память | Китайская экосистема, OpenClaw/Hermes; нет битемпоральности (нет данных) |
| AWS AgentCore / Vertex Memory Bank / Cloudflare | Агенты (гиперскейлеры) | Управляемая память, корпоративный уровень | Извлечённые записи без графа фактов во времени (по найденным описаниям); привязка к облаку; цена за запись или GB |
| Glean | Компания + слияние | Граф компании + личный граф | Готовое приложение для сотрудников; коннекторы к 100+ SaaS; контракты в сотни тысяч долларов; не инфраструктура для чужих агентов |
| Microsoft Work IQ | Компания + слияние | Личная память + оргграф, API для агентов | Только данные M365 и тенант Microsoft |
| Atlassian Rovo / Teamwork Graph | Компания | Граф связей, MCP | Данные экосистемы Atlassian (+GitHub, Figma и др.); нет битемпоральности (нет данных) |
| basic-memory | Личный / мультивендорный | MCP-память для Claude/ChatGPT/Codex/Cursor | Markdown-файлы + SQLite; ориентирован на личную или малую команду; $19/мес |
| mcp-memory-service | Мультиагентный (self-host) | Общая память многих агентов, граф | sqlite-vec, бесплатный self-host, без SSO-уровня enterprise |
| Plaud / Bee / Omi | Личный (захват) | Источник эпизодов | Устройства записи, а не слой памяти для агентов |

### 7. Свободные ниши

| Ниша | Улика, что пуста | Кто ближе всех |
|---|---|---|
| **Переносимость личной памяти сотрудника («memory follows the employee»)** | Zep описывает только удаление (RTBF). У Claude Team память подчиняется политике организации. У Work IQ данные остаются в тенанте. Функций экспорта личного слоя уходящему сотруднику не найдено | Zep (разделение user graph и shared graphs уже есть: остаётся сделать экспорт по политике); Claude (импорт памяти между ассистентами) |
| **Федерация личных мозгов между организациями и людьми** | Каталоги графов есть только внутри одного проекта или тенанта (graph directory Zep, Personal Graph Glean). Межорганизационной федерации не найдено | Zep (graph directory + ABAC); Atlassian (граф открыт через MCP) |
| **Второй мозг семьи** | Ни одного продукта в ходе исследования не встретилось. Специально эта ниша не искалась, поэтому это слабая улика (нет данных) | Basic Memory Cloud (рабочее пространство на 2 человек); Bee/Alexa+ (домашний и внедомашний контекст Amazon) |
| **Provenance на уровне «какой агент или вендор записал факт» + разрешение конфликтов между вендорами** | В документации мультивендорных MCP-памятей атрибуция идёт к эпизоду или источнику, а не к агенту-писателю. Формальное разрешение конфликтов есть только у Zep | Zep (битемпоральность + provenance эпизодов) |
| **Одна память на ИИ-инструменты человека** | **Не пуста**: Zep, basic-memory, OpenMemory, mcp-memory-service, Supermemory, Hindsight, MemoryPlugin, memU | — |

## Recommendations

### Для собеседования в Zep (Head of Forward Deployed Engineering)
- **Начать с Memory MCP Server и graph directory как с главной истории для FDE.** Внедрение — это не «подключить SDK». Это спроектировать онтологию (кастомные типы сущностей и рёбер, иерархии типов в v4), разбить данные на пользовательские и общие графы, написать ABAC-политики для групп и настроить SSO. Сюда же входят фреймворк оценки, сравнение на данных клиента (а не на LoCoMo) и BYOC-развёртывание. Это и есть работа forward-deployed команды.
- **Спросить CEO о трёх рисках.** Первый — деньги: раскрыто ~$2,3M против $24M у Mem0, а публичный анонс раунда 2026 года не найден. Второй — давление гиперскейлеров (AgentCore, Vertex, Cloudflare) и владельцев рабочих графов (Glean, Work IQ, Teamwork Graph через MCP). Третий — продолжение войны бенчмарков: Hindsight и MemMachine заявляют более высокие цифры.
- **Предложить конкретную идею.** Экспорт личного графа сотрудника при увольнении по политике работодателя и federation между проектами — пустая ниша, где у Zep уже есть 80% архитектуры.

### Для нашего продукта
- **Не позиционироваться как «одна память на все ИИ-инструменты».** Это уже делают basic-memory (Markdown + SQLite + MCP — почти наш стек) и mcp-memory-service (sqlite-vec, общий для Claude Code и Codex).
- **Строить уникальность в трёх местах:** (1) битемпоральные факты (valid/invalid + время записи) в локальном SQLite; (2) provenance «какой агент, вендор и машина записали факт» с разрешением конфликтов между Claude, Codex и Grok; (3) синхронизация 8 машин без облачного посредника. В найденных продуктах полного сочетания этих трёх свойств нет.
- **Взять Graphiti как эталон схемы.** Он под Apache-2.0 и поддерживает встроенную БД. Это лучше, чем изобретать свою темпоральную модель, — так же, как Zep отделил Graphiti от Konig.

## Caveats
- Все бенчмарки в отчёте — self-reported или опубликованы с участием вендора (статья Hindsight написана в соавторстве с Virginia Tech и Washington Post). Цифры LoCoMo и LongMemEval получены на разных LLM и настройках и напрямую несравнимы.
- Число звёзд взято в основном из сравнения Mnemoverse на 28.09.2026 и со страниц GitHub. Mnemoverse — сам конкурент, это заинтересованный источник.
- Цены AWS AgentCore, Vertex Memory Bank, а также детали памяти Claude и Microsoft Work IQ частично взяты из вторичных источников, которые ссылаются на официальные страницы. Переход Vertex на тарификацию GiB/vCPU с 01.09.2026 не подтверждён.
- Сведения о новых инвесторах Zep в январе 2026 года (gritt.io) — из агрегатора; суммы нет.
- Подсчёт выручки Zep «в низких-средних шестизначных суммах» (systemaic.com) — домысел аналитика, а не факт; в выводах он не используется.

## Чего в данных нет
- Цены и детали: LangMem, Graphlit, A-MEM, Redis Agent Memory Server, Memobase (лицензия, тяга).
- Текущий статус Anthropic memory MCP reference server и общих блоков Letta (shared blocks) в 2026 году.
- Enterprise-игроки: Notion Enterprise, Guru, Slack AI, Onyx, Sana (Workday), Hebbia, Moveworks, Writer Knowledge Graph — в этом исследовании не проверялись.
- Personal.ai (статус 2025–2026), Notion AI, плагины ИИ для Obsidian; цены Mem и Tana; финансирование Omi, Heptabase, Capacities, Reflect.
- Zep: имена клиентов (есть только «Fortune 500 deployments»), численность команды, раунд 2026 года.
- Любой продукт для «второго мозга семьи» (целенаправленно не искался).

## Три вывода, которые меняют действия
1. **Для собеседования: Zep уже продаёт «память сотрудника + память компании».** Memory MCP Server, graph directory и ABAC с августа 2026 года — это не лабораторная идея. Разговор с CEO стоит строить вокруг того, как FDE превращает это в повторяемые внедрения: онтология, политики, оценка на данных клиента, BYOC. Сравнения «Zep против Mem0 на LoCoMo» стоит избегать.
2. **Для продукта: мы повторяем basic-memory и mcp-memory-service, если не добавим время и происхождение.** Наш стек «Markdown + SQLite + MCP + несколько агентов» уже существует в open source. Ценность появляется только с битемпоральными фактами и атрибуцией записи к агенту или машине с разрешением конфликтов между вендорами. Это первое, что надо доделать.
3. **Общая для обоих ниша — владение личной памятью при смене работодателя и федерация.** Ни Zep, ни Glean, ни Microsoft, ни Atlassian этого не делают. Это и тезис для собеседования (что Zep может построить на своей архитектуре), и возможное направление нашего продукта (личный мозг, который сотрудник приносит в компанию и забирает с собой).

## SOURCES (md_citations)
https://www.getzep.com/ai-agents/
https://blog.getzep.com/
https://blog.getzep.com/agent-memory-mcp/
https://www.getzep.com/pricing/
https://tomrochette.com/agents/memory/zep/
https://renezander.com/guides/zep-vs-graphiti/
https://ai.engineer/orgs/zep
https://www.getzep.com/ai-agents/what-is-a-context-lake/
https://www.getzep.com/blog/author/jack/
https://www.getzep.com/blog/
https://www.getzep.com/blog/evaluation-and-control-evaluation-framework-zepctl-cli-and-dashboard-overhaul/
https://www.getzep.com/blog/zep-api-for-agents/
https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3
https://pitchbook.com/profiles/company/539323-75
https://www.cbinsights.com/company/zep-2
https://www.gritt.io/company/zep-ai/
https://mem0.ai/series-a
https://rywalker.com/research/supermemory
https://susaventures.substack.com/p/our-investment-in-supermemory
https://github.com/supermemoryai/supermemory
https://docs.letta.com/configuration/memory
https://www.prnewswire.com/news-releases/berkeley-ai-research-lab-spinout-letta-raises-10m-seed-financing-led-by-felicis-to-build-ai-with-memory-302257004.html
https://atlan.com/know/best-ai-agent-memory-frameworks-2026/
https://github.com/letta-ai/letta
https://www.getzep.com/ai-agents/how-to-give-ai-agents-long-term-memory/
https://ecorpit.com/ai-agent-memory-mem0-zep-letta-cloudflare-comparison-2026/
https://vectorize.io/hindsight
https://arxiv.org/html/2604.04853v1
https://github.com/Mozy403/MemOS
https://www.microsoft.com/en-us/copilot/blog/2026/06/02/announcing-the-new-work-iq-apis/
https://www.onpointserv.com/post/atlassian-s-team-26-rovo-and-teamwork-graph-transforming-ai-native-enterprise-automation
https://9to5google.com/2026/10/15/amazon-launches-bee-ai-wearables/
https://tech-insider.org/amazon-bee-ai-devices-frontier-eclipse-2026/
https://www.getzep.com/mem0-alternative/
https://www.getzep.com/blog/zep-a-temporal-knowledge-graph-architecture-for-agent-memory/
https://blog.getzep.com/stop-using-rag-for-agent-memory/
https://hydradb.com/blog/zep-reviews
https://www.maximem.ai/zep-alternatives
https://github.com/letta-ai/letta-code
https://theaiagentindex.com/compare/cognee-vs-zep
https://mystartupnews.in/news/19-year-old-indian-founder-dhravya-shah-raises-dollar3-million-for-ai-startup-supermemory
https://venturebeat.com/data/with-91-accuracy-open-source-hindsight-agentic-memory-provides-20-20-vision
https://themenonlab.blog/blog/hindsight-agent-memory-biomimetic-learns
https://vectorize.io/product
https://www.morningstar.com/news/pr-newswire/20251216ph48348/vectorize-breaks-90-on-longmemeval-with-open-source-ai-agent-memory-system
https://aws.amazon.com/marketplace/pp/prodview-fchsnqq73d23a
https://yespress.io/memverge
https://www.prnewswire.com/news-releases/memverge-launches-memmachine-worlds-most-powerful-ai-memory-layer-302563813.html
https://aitoolradar.io/open-source/memos
https://github.com/MemTensor/MemOS
https://mnemoverse.com/docs/research/market-landscape
https://aiagentmemory.org/articles/building-smarter-ai-agents-agentcore-long-term-memory-deep-dive/
https://www.developersdigest.tech/agent-memory/aws-agentcore-memory
https://cloud.google.com/blog/products/ai-machine-learning/new-enhanced-tool-governance-in-vertex-ai-agent-builder
https://www.betterclaw.io/blog/google-vertex-ai-agent-builder
https://docs.cloud.google.com/vertex-ai/docs/release-notes
https://docs.cloud.google.com/vertex-ai/generative-ai/docs/release-notes
https://www.infoq.com/news/2026/04/cloudflare-agent-memory-beta/
https://www.krasa.ai/news/cloudflare-agent-memory-private-beta-persistent-ai
https://www.ai-toolbox.co/chatgpt-management-and-productivity/how-to-manage-chatgpt-memory-2026
https://memu.pro/blog/codex-memory-v2
https://www.thetoolnerd.com/p/your-coding-agent-has-amnesia-the
https://medium.com/@glasier067/chatgpt-just-got-a-memory-upgrade-heres-why-it-matters-8fe84de9cc8b
https://supermemory.ai/blog/claude-code-auto-memory/
https://ivgraph.com/journal/claude-code-memory/
https://blog.getzep.com/why-we-built-a-graph-database-service-for-agent-memory/
https://basicmemory.com/
https://blog.mcpservers.org/posts/mcp-memory-servers
https://mem0.ai/blog/introducing-openmemory-mcp
https://pypi.org/project/mcp-memory-service/
https://pypi.org/project/mcp-memory-service/10.37.0/
https://github.com/doobidoo/mcp-memory-service
https://docs.cognee.ai/cognee-mcp/
https://www.vktr.com/ai-news/cloudflare-agents-week/
https://aq.dev/guides/coding-agent-memory-explained/
https://blog.getzep.com/author/daniel/
https://www.layer3labs.io/guides/best-ai-wearable-pendants-2026
https://www.theodorehq.com/echo/blog/posts/rewind-ai-vs-screenpipe
https://www.hedy.ai/post/meta-acquires-limitless-ai-privacy/
https://luci.memories.ai/blog/rewind-ai-shut-down-on-device-replacement
https://theaiinsider.tech/2026/01/13/amazon-showcases-bee-wearable-as-ai-companion-extending-personal-intelligence-beyond-the-home/
https://techcrunch.com/2026/10/15/amazons-bee-launches-two-new-personal-ai-devices-starting-at-75/
https://sacra.com/c/plaud/
https://www.pymnts.com/news/artificial-intelligence/2026/plaud-plans-to-file-ipo-for-ai-note-taking-startup-in-2028/
https://www.bloomberg.com/news/articles/2026-06-16/plaud-plans-new-wearable-as-ai-note-taking-startup-eyes-500-million-in-sales
https://news.ycombinator.com/item?id <tg-id>
https://www.stork.ai/en/screenpipe
https://get.mem.ai/blog/introducing-mem-2-0
https://www.g2.com/products/recall-recall/pricing
https://www.stork.ai/en/recall-2-0
https://hysenlabs.com/projects/khoj-ai-khoj
https://app.khoj.dev/
https://github.com/reorproject/reor
https://www.prnewswire.com/news-releases/tana-raises-25m-to-unlock-the-power-of-human-ai-collaboration-302365963.html
https://www.usagepricing.com/blueprint/heptabase
https://storyflow.so/blog/best-heptabase-alternatives-2026
https://www.digitalapplied.com/blog/chatgpt-memory-dreaming-v3-openai-2026-guide
https://www.memorylake.ai/en/blogs/claude-memory-chat-and-cowork
https://unabyss.com/blog/claude-memory-feature
https://www.glean.com/blog/enterprise-ai-indexing-context
https://theplanettools.ai/tools/glean
https://www.glean.com/blog/live-fall-25-main
https://chatforest.com/reviews/glean-300m-arr-series-f-7-2b-valuation-enterprise-ai-search-2026/
https://techcommunity.microsoft.com/blog/analyticsonazure/meet-the-iqs-how-microsoft-is-creating-context-aware-ai/4537516
https://windowsforum.com/news/microsoft-work-iq-context-layer-for-copilot-agents-across-m365-and-governance.425518/
https://secure.businesswire.com/news/home/20260506556902/en/Atlassian-Opens-Up-Its-Teamwork-Graph-to-Power-Agentic-Work-Across-the-Enterprise
https://www.longisland-ny.com/2026/05/06/atlassian-unveils-teamwork-graph-rovo-updates-at-its-team-event/
https://www.atlassian.com/blog/company-news/rovo-chat-new-features-september-2026
https://theoutpost.ai/news-story/atlassian-opens-teamwork-graph-and-evolves-rovo-into-autonomous-agent-at-team-26-26033/
https://finance.yahoo.com/sectors/technology/articles/atlassian-opens-teamwork-graph-power-160000211.html
https://theaieconomy.substack.com/p/atlassian-teamwork-graph-open-rovo-ai-agents
https://www.getzep.com/solutions/context-engineering/
https://www.reworked.co/knowledge-findability/glean-thinks-its-ai-understands-your-company-better-than-you-do/
https://www.ingentive.com/work-iq/
https://www.cognee.ai/import-chatgpt-memory-to-claude
https://www.developersdigest.tech/agent-memory/letta
https://theaiengineer.substack.com/p/cognee-vs-zep-vs-mem0-vs-letta
https://vectorize.io/blog/hindsight-building-ai-agents-that-actually-learn
https://github.com/RerankerGuo/MemOS
https://help.getzep.com/graph-overview
https://www.systemaic.com/teardowns/zep-ai