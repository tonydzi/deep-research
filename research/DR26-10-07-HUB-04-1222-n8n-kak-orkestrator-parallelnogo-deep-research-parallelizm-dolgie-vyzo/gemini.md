# DR26-10-07-HUB-04-1222 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Gemini
New chat
Search chats
Images
Videos
Library
Notebooks
New notebook
Recent
КОНТЕНТ ЗАВОД
Стратегия X-контента для инженера
--- id: DR26-10-07-HUB-04-1222 date: 2026-10-07 12:22:26 theme: Параллельные воркфлоу и параллельный Deep Research в n8n: реальн
Запрос порівняння та аналізу даних
--- id: DR26-10-06-HUB-02-1359 date: 2026-10-06 13:59:54 theme: Organic un-fried vegetable-rich brea
AI Startup Fundraising Team Research
Экономика и защита AI-стартапов для SMB в США
Оценка мультиагентных рабочих пространств
LLM Comparison for X Posts
Ошибка отказа языковой модели
Стратегия OSS-контрибьютинга для ИИ-оффера
Стратегия OSS-экспертизы для ИИ-оффера
AI Security Career Path for Senior Engineer
Anthropic Fellows Program Security Stream Analysis
AI Security Career Doors for Anton
Anthropic Fellows Program Application Strategy
Стратегия OSS-контрибьютинга для ИИ-оффера
bb Orchestrator Fleet Setup and Practices
Окестрация агентов с bb
Критика архитектуры второго мозга
Анализ внедрения Jev в агентский флот
Анализ и улучшение CV для Silicon Valley AI Landing Lead
Идеальное CV AI Deployment Strategist 2026
Анализ и оптимизация венчурного CV
Идеальное CV 2026: Стейблкоин Strategy & BD
Activity
CryptoPunk Dz2
Work
•
Expanded
Rio de Mouro, Portugal
From your IP address
Update location
Conversation with Gemini
You said --- id: DR26-10-07-HUB-04-1222 date: 2026-10-07 12:22:26 theme: Параллельные воркфлоу и параллельный…

---

id: DR26-10-07-HUB-04-1222

date: 2026-10-07 12:22:26

theme: Параллельные воркфлоу и параллельный Deep Research в n8n: реальный параллелизм, долгие внешние вызовы, кворум и добор, чужой опыт против Temporal/Prefect/asyncio



targets: chatgpt,gemini,grok,glm,mistral

---




# Deep Research: параллельные воркфлоу и параллельный Deep Research в n8n




Контекст заказчика: self-hosted n8n (наш сайт, ~98 воркфлоу, 68 активных, queue-mode неизвестен). Задача — выяснить, годится ли n8n как ОРКЕСТРАТОР параллельной веерной работы (в частности веера Deep Research по 5-6 внешним LLM-вендорам одновременно), и как это строят те, кто уже это сделал.




Нужны ответы с ссылками на документацию/исходники/посты практиков, с датами, и с явной пометкой где это замер, а где чужое утверждение.




## 1. Параллелизм в n8n: что есть на самом деле

- Как n8n исполняет ветки воркфлоу: действительно параллельно или по очереди (порядок веток, Merge-нода, SplitInBatches)? Что меняет queue mode (Redis + воркеры), `N8N_CONCURRENCY_PRODUCTION_LIMIT`, `EXECUTIONS_MODE=queue`, число воркеров?

- Sub-workflow (Execute Workflow): синхронный/асинхронный вызов, «Execute Once», wait-for-sub-workflow; как сделать fan-out из N параллельных подворкфлоу и собрать результаты (fan-in) без потери ветки.

- Потолки: сколько одновременных HTTP-запросов/исполнений держит один инстанс; где ломается (память, таймауты вебхука, pruning истории исполнений).

- Wait-нода: точность срабатывания, лимиты числа ожидающих исполнений, что происходит при рестарте n8n (переживают ли ожидания), и чем это отличается от Schedule Trigger.




## 2. Долгие и ненадёжные внешние вызовы (ровно наш кейс Deep Research)

- Паттерны для вызова, который длится 5-40 минут (deep research у вендоров): polling, webhook-callback, Wait-until, разбиение на два воркфлоу.

- Идемпотентность и дедупликация повторных запусков; retry с backoff; DLQ (dead-letter) в n8n; как не заказать один и тот же ДР дважды.

- Частичный результат: как собрать кворум «4 из 6 ответили, 2 опоздали» и потом добрать опоздавших, не блокируя синтез.

- Наблюдаемость: единый run-ledger по исполнениям, алерт на упавшую ветку, экран «что сейчас в полёте».




## 3. Кто это уже сделал — чужой опыт, не теория

- Публичные примеры/шаблоны n8n для веера по нескольким LLM-провайдерам одновременно (OpenAI/Anthropic/Gemini/Grok/Mistral/DeepSeek), с кодом или экспортом воркфлоу.

- Сравнение с альтернативами по ЭТОЙ задаче: Temporal, Prefect, Dagster, Windmill, Airflow, Inngest, Trigger.dev, обычный Python asyncio + cron. Где n8n объективно хуже и где лучше.

- Известные грабли практиков: Code-нода (Pyodide, нет pip), размер payload между нодами, бинарные данные, секреты, версионирование воркфлоу в git, миграция.




## 4. Вердикт, который нужен заказчику

- Для веера Deep Research по 6 вендорам раз в сутки: n8n — да или нет, и почему. Если да — минимальная схема нод. Если нет — что вместо.

- Что НЕ переносить в n8n ни при каких условиях (файловая система, git, GUI, локальные процессы Windows/macOS).

- Три самых дешёвых улучшения, которые дают 80% пользы.




Формат ответа: выводы списком, под каждым — источник и дата. Отдельной строкой: чего ты НЕ смог проверить и почему.

Analyzing Parallelism in n8n
Gemini said
Answer now




Pro

Gemini is AI and can make mistakes, including about people. Your privacy and Gemini
Opens in a new window

Analyzing Parallelism in n8n