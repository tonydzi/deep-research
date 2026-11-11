# DR26-11-03-HUB-01-0950 — the exact prompt sent to every model

# DR26-11-03-HUB-01-0950 — Карта рынка второго мозга и общей памяти: личный, корпоративный, слияние сотрудник+компания, память агентов; сравнение с Zep AI

OUTPUT CONTRACT (read first): this is a RESEARCH task. Use live web search and read primary sources (product sites, docs, pricing pages, GitHub repos, funding announcements, papers). Every non-trivial claim carries a link. If a fact is not found, write «нет данных», do not guess. Answer in Russian; product and company names stay in English. Date of research: November 2026; prefer 2025-2026 sources and mark anything older.

# Тема
Карта рынка «второго мозга» и общей памяти: для частного человека, для сотрудника, для всей компании, и для ИИ-агентов. Кто это строит сейчас, SaaS и open source, и как эти продукты соотносятся с Zep AI.

# Зачем (потребитель отчёта)
1. Антон завтра (04.11.2026) проходит собеседование с CEO Zep AI на роль Head of Forward Deployed Engineering. Нужна точная картина конкурентов Zep и того, чем Zep отличается.
2. Мы сами строим «второй мозг» (Obsidian-волт + графовый RAG на SQLite + общая память 8 машин и нескольких ИИ-агентов: Claude, Codex, Grok). Решение, которое кормит отчёт: в каком сегменте наш подход уникален, а где мы повторяем чужое.

# Что мы уже знаем (НЕ пересказывай, только поправь, если устарело)
- Zep: Context Graph на пользователя/аккаунт, битемпоральные факты (valid_at / invalid_at, противоречия помечаются, а не стираются), гибридный поиск (эмбеддинги + BM25 + обход графа + reranker), <200 мс, Managed / BYOK / BYOC, SOC 2 Type II, HIPAA BAA; open-source движок Graphiti; статья arXiv 2501.13956; цены Flex $125 и Flex Plus $375 в месяц, Emerging $13k/год; Zep сам называет конкурентами Mem0, Letta, Cognee, Supermemory, AWS AgentCore Memory, Vertex AI Memory Bank.
- Mem0: раунд $24M (окт. 2025), 41k+ звёзд. Letta: агент сам управляет памятью (MemFS). Cognee: OSS граф+память, $1.5M. Supermemory: $3M pre-seed. Glean: enterprise search и knowledge graph, $300M ARR, контракты от $100k/год.

# Вопросы
1. **Память агентов (agent memory layer)**, SaaS и OSS: полный список игроков 2025-2026 сверх перечисленных (например LangMem, MemOS, Memobase, MemMachine, Graphlit, Hindsight, A-MEM, OpenAI/Anthropic/Google встроенная память, AWS AgentCore Memory, Vertex Memory Bank, Microsoft). Для каждого: модель памяти (вектор / граф / темпоральный граф / файлы / суммаризация), open source или нет, лицензия, деплой (облако / self-host / VPC), цена, деньги и тяга (звёзды, клиенты), бенчмарки (LoCoMo, LongMemEval, DMR) с оговоркой, кто их мерил.
2. **Общая память между агентами (multi-agent shared memory)**: кто явно продаёт или строит память, которую одновременно читают и пишут несколько агентов или несколько разных вендоров (Claude, Codex, Cursor и т.д.); MCP-серверы памяти (например OpenMemory, basic-memory и подобные); как решают конфликты записи, права доступа и происхождение факта (provenance).
3. **Личный второй мозг для частного человека**: SaaS и OSS (например Mem.ai, Notion AI, Reflect, Tana, Heptabase, Capacities, Recall, Personal.ai, Limitless/Rewind и что с ними после покупки, Khoj, Reor, Obsidian + плагины ИИ, Screenpipe, носимые записывающие устройства вроде Plaud, Omi, Bee). Что из этого «помнит за человека» автоматически, а что требует ручного ведения.
4. **Второй мозг компании (знания всех сотрудников в одном месте)**: Glean, Microsoft 365 Copilot / Work IQ / Graph, Atlassian Rovo, Notion Enterprise, Guru, Slack AI, Dust, Onyx (бывш. Danswer), Sana (Workday), Hebbia, Moveworks, Writer Knowledge Graph и другие. Кто строит именно «граф знаний компании», а не поиск по документам.
5. **Слияние личного и корпоративного**: кто реализует модель «у каждого сотрудника свой второй мозг/память, и они объединяются в общий мозг компании» с правами, приватностью и владением данными при увольнении. Есть ли продукты, где личная память агента сотрудника и память компании — два слоя одного графа. Назови прямо, если такого продукта нет.
6. **Сравнительная таблица с Zep**: по каждому крупному игроку из пунктов 1-5 — сегмент (личный / корпоративный / агенты / слияние), чем похож на Zep, чем отличается (модель памяти, время фактов, enterprise-развёртывание, open source, цена).
7. **Свободные ниши**: где в 2026 нет сильного игрока (например, общая память нескольких разных ИИ-вендоров у одного человека или команды; второй мозг семьи; федерация личных мозгов сотрудников). Для каждой ниши — улика, что она пуста, и кто ближе всех.

# Формат
- Таблицы по каждому сегменту, затем общая сравнительная таблица с Zep.
- Раздел «Чего в данных нет».
- В конце: три вывода, которые меняют действия (для собеседования в Zep и для нашего продукта).
