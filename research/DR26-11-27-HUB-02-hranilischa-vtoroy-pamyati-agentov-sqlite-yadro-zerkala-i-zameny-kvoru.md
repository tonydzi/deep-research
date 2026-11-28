---
dr_id: DR26-11-27-HUB-02
title: "Хранилища второй памяти агентов: SQLite-ядро, зеркала и замены (кворум Gemini, ноябрь 2026"
date: 2026-11-27
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-27-HUB-02): Хранилища второй памяти агентов: SQLite-ядро, зеркала и замены (кворум Gemini, ноябрь 2026)

> Какие движки (ClickHouse/chDB, Postgres+pgvector, Qdrant, LanceDB, DuckDB и др.) подходят как векторное зеркало или замена подсистем SQLite-памяти агентов, какие свойства и тесты нужны SQLite-MVP и куда безопасно нести пилоты на GitHub.

## Ключевые выводы
- Отчёт Gemini не имел живого веб-поиска и доступа к чатам оператора: события и релизы после марта 2026 помечены «не подтверждено», 2-3 свежих принятых внешних PR по каждому движку не найдены, фабрикация PR запрещена. Это не полноценный браузерный отчёт для кворума.
- Разделение ролей: векторы (numpy, 105k чанков, ~150–645 МБ) разумно выносить в зеркало (Qdrant, LanceDB, pgvector, sqlite-vec) с chunk_id как внешним ключом. FTS5 на 105k чанков оставить в SQLite: работает за единицы мс без сети. Журнал запросов оставить в SQLite WAL, при необходимости реплицировать в ClickHouse/DuckDB для аналитики.
- Граф (438k сущностей, 8M упоминаний, 5M рёбер) не переносить в чистые векторные БД (Qdrant/Chroma): у них нет эффективных multi-hop джойнов. Кандидаты для выноса: Postgres (рекурсивные CTE), Neo4j, FalkorDB, Kuzu. Postgres+ParadeDB+pgvectorscale назван единственным движком, способным заменить всё сразу (BM25, векторы, граф через SQL).
- Свойства SQLite-MVP: SQLite как единый источник истины, внешний движок хранит только проекцию (chunk_id, vector, filter_metadata, content_hash); транзакционный outbox sync_outbox с идемпотентным применением по content_hash; обязательный предикат tenant_id/agent_id/namespace в каждом запросе (FTS, граф, ANN); WAL + synchronous=NORMAL + busy_timeout=5000 + foreign_keys=ON, один writer и N read-only соединений.
- Пять обязательных negative-тестов: test_crash_between_sqlite_and_mirror (SIGKILL после коммита до отправки в зеркало), test_partial_batch_rollback (ошибка на 25-й из 50 записей), test_cross_agent_isolation_leak (ns_alpha vs ns_beta по семантике, FTS и 2 хопам графа), test_idempotent_replay_no_duplicates (3 повтора outbox-батча), test_concurrent_writer_lock_timeout (10 потоков 5 секунд без database is locked и порчи FTS5).
- Где ломается SQLite: нет встроенного суб-линейного ANN (numpy brute-force ок до ~200–500k векторов, ~15–40 мс; дальше упирается в память и холодный старт); single-writer в WAL при массовом пересчёте графа; деградация 3+ хопов на 5M рёбер при выходе за page cache (индексы ~300–500 МБ); Syncthing не мержит страницы SQLite и -wal/-shm, синхронизировать можно только снапшоты (VACUUM INTO) или Litestream/LiteFS.
- Предложенные точки вклада (все без подтверждённых свежих PR): lancedb/vectordb-recipes, qdrant/examples, paradedb/paradedb или timescale/pgvectorscale (examples/), chdb-io/chdb или ClickHouse/examples + ClickGems (нужна CLA), duckdb/community-extensions. Время мержа и свежие принятые PR не подтверждены.
- Бенчмарки LoCoMo (Snap Research, 2024) и LongMemEval (ICLR 2025) оценивают алгоритм памяти, а не СУБД при фиксированном retrieval; публичных бенчмарков 2026 года, изолирующих именно хранилище, отчёт не нашёл (не подтверждено).
- ColBERT/late interaction раздувает индекс в 10–30 раз (критично для 105k чанков); для локального гибрида без API предложен bge-m3 (dense+sparse+multi-vector) как оптимальный. Остальные вендоры эмбеддингов (Jina, Voyage, Cohere, Nomic, Qwen3, OpenAI, Google, Mixedbread) перечислены без подтверждённых свежих cookbook-примеров по памяти агентов.
- Политика безопасного вклада: только синтетические или открытые данные (никаких дампов сущностей, логов, эмбеддингов лаборатории); ключи только из os.environ или локальные модели; gitleaks/detect-secrets перед коммитом; воспроизводимый бенчмарк с random.seed(42) без невоспроизводимых цифр; проверка лицензии и подписание CLA до открытия PR.

## Рекомендации / решения
- Оставить SQLite ядром и источником истины (FTS5, граф, журнал), а векторы вынести в зеркало (LanceDB или Qdrant) через sync_outbox; полную замену на Postgres+ParadeDB+pgvectorscale рассматривать только при выходе за пределы single-writer и page cache.
- Реализовать в MVP транзакционный outbox, идемпотентный воркер по content_hash и принудительный фильтр tenant_id/agent_id/namespace во всех путях поиска, включая обход графа.
- Внедрить пять fault-injection тестов (crash, partial batch, isolation leak, idempotent replay, concurrent writer) как обязательные, падающие при нарушении персистентности или изоляции.
- Не синхронизировать живой .db через Syncthing; использовать immutable-снапшоты VACUUM INTO либо Litestream/LiteFS.
- Пилоты «SecondBrain on <engine>» нести в LanceDB (vectordb-recipes), Qdrant (examples), Postgres/ParadeDB, ClickHouse/chDB и DuckDB; перед открытием PR проверить принятые PR командой gh pr list --state merged и правила CONTRIBUTING/CLA.
- Не засчитывать этот отчёт как браузерный кворум: оператору нужно прогнать вопросы в режиме с живым веб-поиском, сохранить URL чатов и доказательства режима, а факты, помеченные «не подтверждено», перепроверить по первоисточникам на 2026-11-24.
- При необходимости локального гибридного поиска без API использовать bge-m3 или другие open-weights модели; ColBERT-подход для 105k чанков не брать из-за раздувания индекса.

## Сущности
- **Люди:** —
- **Компании:** Mem0, Zep, Letta, Cognee, Redis, ClickHouse, Plastic Labs (Honcho), Supermemory, Vectorize (Hindsight), Memobase, MemTensor (MemOS), LangChain/LangGraph, Qdrant, LanceDB, DuckDB, Timescale, ParadeDB, Turbopuffer, Milvus, Weaviate, Chroma, Neo4j, FalkorDB, Jina AI, Voyage AI, Cohere, Nomic, BAAI, Qwen, OpenAI, Google, Mixedbread, Snap Research, Cloudflare
- **Продукты/инструменты:** —

## Открытые вопросы
- Какие дефолтные бэкенды и релизы у Mem0, Zep/Graphiti, Letta (letta-code), Cognee, Redis Agent Memory Server, ClickHouse Agent Memory, auxten/clickmem, Honcho, Supermemory, Hindsight, Memobase, MemOS, memU на апрель–ноябрь 2026?
- Есть ли публичные бенчмарки 2026 года, сравнивающие именно хранилища при одинаковом retrieval на LoCoMo, LongMemEval или MemBench?
- Какие 2-3 свежих принятых внешних PR за 2026 год есть в lancedb/vectordb-recipes, qdrant/examples, paradedb/paradedb, chdb-io/chdb, duckdb/community-extensions, и каково типичное время мержа?
- Какой актуальный статус персистентности HNSW в DuckDB vss и судьба Kuzu и форка Ladybug после 2025–2026?
- Существуют ли у Voyage, Jina, Cohere, Nomic, Qwen3, Mixedbread cookbook-репозитории с открытым приёмом PR и примерами про память агентов на ноябрь 2026?
- Существуют ли уже в чатах ChatGPT, Claude.ai, Grok и др. отчёты по DR26-11-05-HUB-03-1726 (поиск на локальной машине оператора)?
- Какие лицензии и точные хранилища у Honcho и Supermemory (AGPL/MIT/Apache уточнить по репозиториям)?

## Источник
- DR-ID `DR26-11-27-HUB-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- agent memory storage
- SQLite FTS5
- transactional outbox
- vector mirror vs replacement
- tenant isolation
- fault-injection tests
- pgvector / pgvectorscale / ParadeDB
- LanceDB

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-28 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
