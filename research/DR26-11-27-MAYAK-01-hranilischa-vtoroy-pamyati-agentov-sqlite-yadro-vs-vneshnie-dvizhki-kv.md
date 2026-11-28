---
dr_id: DR26-11-27-MAYAK-01
title: "Хранилища второй памяти агентов: SQLite-ядро vs внешние движки (кворум, Gemini)"
date: 2026-11-27
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-27-MAYAK-01): Хранилища второй памяти агентов: SQLite-ядро vs внешние движки (кворум, Gemini)

> Как сочетать SQLite-память лаборатории (FTS5, граф сущностей, numpy-векторы, журнал) с внешними движками как векторным зеркалом либо заменой, какие MVP-свойства и тесты нужны, и куда нести пилоты; без веб-доступа большая часть фактов за 2026 год не подтверждена.

## Ключевые выводы
- Отчёт Gemini честно заявил отсутствие доступа к живому вебу и чатам оператора: факты после марта 2026 по релизам, дефолтам, PR и бенчмаркам помечены «не подтверждено». Это один ответ из четырёх нужных, поэтому кворум не закрыт.
- Для масштаба 105k чанков / 438k сущностей / 8M упоминаний / 5M рёбер вердикт такой: векторы разумно выносить в зеркало (Qdrant, LanceDB, pgvector, sqlite-vec), FTS5 и журнал запросов оставить в SQLite, граф не переносить в чистые векторные БД (Qdrant/Chroma). Для графа подходят SQLite с покрывающими индексами, Postgres с рекурсивными CTE либо Neo4j/FalkorDB/Kuzu.
- Оценка по памяти: 105k векторов размерности 1536 float32 занимают около 645 МБ, brute-force в numpy даёт примерно 15-40 мс на CPU. Это оценка, а не замер, и она упирается в пропускную способность памяти и холодный старт при росте за 200-500k, а тем более за 1M чанков.
- Предложенные MVP-свойства SQLite-ядра: SQLite как единственный источник истины с детерминированным chunk_id, транзакционный outbox sync_outbox с идемпотентным применением по content_hash, обязательный фильтр tenant_id/agent_id/namespace в каждом поиске (FTS, граф, ANN), WAL с synchronous=NORMAL, busy_timeout=5000, foreign_keys=ON, один writer и N read-only соединений.
- Пять обязательных negative-тестов: крах между коммитом в SQLite и отправкой в зеркало, откат частичного батча (потеря записей 25-50), утечка между агентами по FTS/ANN/обходу графа на 2 хопа, повторный прогон батча 3 раза без дублей, 10 параллельных писателей в течение 5 секунд без «database is locked» и порчи FTS5. Тесты описаны как проект, а не как результат прогона.
- Синхронизация SQLite через Syncthing опасна: файлы .db, -wal и -shm не мержатся, при одновременном открытии на двух машинах возникает conflicted copy или порча. Безопасно синхронизировать только неизменяемые снапшоты (VACUUM INTO / sqlite3 .backup) либо использовать репликацию уровня WAL (Litestream, LiteFS).
- Топ-5 для пилотов «SecondBrain on <engine>»: LanceDB (lancedb/vectordb-recipes), Qdrant (qdrant/examples), Postgres+pgvector/ParadeDB, ClickHouse/chDB (+ClickGems), DuckDB (community-extensions). Точные места вклада названы, но время мержа и 2-3 свежих принятых PR за 2026 год по всем пяти не подтверждены, и PR не выдуманы.
- Бенчмарки LoCoMo (Snap Research, 2024) и LongMemEval (ICLR 2025) оцениваются как тесты алгоритма памяти, а не сравнения СУБД. Публичных бенчмарков 2026 года, которые сравнивают хранилища при одинаковом retrieval, в отчёте не подтверждено.
- Таблица «кто на чём» (Mem0, Zep/Graphiti, Letta, Cognee, Redis AMS и др.) собрана из знаний до марта 2026. Для Hindsight и memU данных нет, для остальных систем строки помечены «не подтверждено на 11.2026». Лицензии у Honcho и Supermemory указаны с оговоркой «уточнить».
- Политика безопасного вклада: только синтетические данные или открытые датасеты, секреты через переменные окружения, прогон detect-secrets/gitleaks до коммита, воспроизводимые замеры с фиксированным seed=42, проверка лицензии и подписание CLA вендора (ClickHouse требует CLA) до открытия PR.

## Рекомендации / решения
- Не засчитывать отчёт Gemini как валидный отчёт кворума: у него нет ссылок на источники и нет подтверждённого режима исследования. Нужно получить четыре независимых браузерных отчёта с полными URL, а оператору сохранить URL чатов и данные о режиме.
- Перед запуском новых чатов оператору самому найти существующие чаты вендоров по заголовку DR26-11-05-HUB-03-1726, потому что модель не видит историю браузера.
- Оставить SQLite источником истины. Внешний движок подключать как зеркало: векторы в Qdrant или LanceDB, а аналитику по журналу и 8M упоминаниям в ClickHouse/chDB или DuckDB. Полную замену рассматривать только через Postgres+ParadeDB/pgvectorscale.
- Реализовать sync_outbox и пять negative-тестов до подключения любого внешнего зеркала и проверить, что каждый тест падает на намеренно сломанном коде.
- Запретить синхронизацию живого .db-файла через Syncthing: передавать только снапшоты через VACUUM INTO или перейти на Litestream/LiteFS.
- Перед открытием PR проверить через gh pr list --repo ... --state merged реальные принятые примеры и сроки мержа в lancedb/vectordb-recipes, qdrant/examples, paradedb, chdb-io/chdb и duckdb/community-extensions.
- Для локального гибридного поиска без внешнего API рассмотреть bge-m3 (dense+sparse+multi-vector) в паре с Qdrant. ColBERT и late interaction не брать из-за роста индекса в 10-30 раз.

## Сущности
- **Люди:** —
- **Компании:** Mem0, Zep, Letta, Cognee, Redis, ClickHouse, Plastic Labs (Honcho), Supermemory, Vectorize (Hindsight), Memobase, MemTensor (MemOS), Qdrant, LanceDB, DuckDB, ParadeDB, Timescale, Turbopuffer, Milvus, Weaviate, Chroma, Neo4j, FalkorDB, Jina AI, Voyage AI, Cohere, Nomic, BAAI, Mixedbread, OpenAI, Google, Anthropic, Snap Research, Syncthing
- **Продукты/инструменты:** SQLite FTS5, sqlite-vec, pgvector, pgvectorscale, chDB, auxten/clickmem, Graphiti, letta-code, Redis Agent Memory Server, LangGraph Store, Kuzu/Ladybug, DuckDB VSS, ClickGems, lancedb/vectordb-recipes, qdrant/examples, duckdb/community-extensions, bge-m3, Qwen3-Embedding, jina-embeddings-v3, ColBERT, RAGatouille, Litestream, LiteFS, LoCoMo, LongMemEval, MemBench, detect-secrets, gitleaks, numpy

## Открытые вопросы
- Кворум не закрыт: нет трёх остальных независимых браузерных отчётов с URL чатов и данными о режиме исследования.
- Актуальные дефолтные бэкенды и лицензии систем памяти на 24.11.2026 (Mem0, Zep/Graphiti, Letta, Cognee, Redis AMS, Honcho, Supermemory, Hindsight, Memobase, MemOS, memU).
- Статус clickmem и ClickHouse Agent Memory, а также судьба Kuzu и форка Ladybug после 2025-2026.
- Сохранилось ли у DuckDB VSS ограничение по персистентности HNSW-индекса.
- Существуют ли публичные бенчмарки 2026 года, сравнивающие хранилища (ClickHouse, Postgres, Qdrant, LanceDB, DuckDB) при одинаковом retrieval.
- Номера, даты и ссылки на 2-3 принятых внешних PR за 2026 год и типичное время мержа в репозиториях пяти целевых движков.
- Какие embedding- и retrieval-вендоры реально имеют открытые cookbook-репозитории с примерами про память агентов на 11.2026 (Voyage, Nomic, Mixedbread, Qwen и другие не подтверждены).
- Какие дефолты выбирают OpenAI, Anthropic и Google для нативных функций памяти: вендоры схему хранения не раскрывают.

## Источник
- DR-ID `DR26-11-27-MAYAK-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- векторное зеркало против замены хранилища
- транзакционный outbox и идемпотентная синхронизация
- изоляция арендаторов в памяти агентов
- SQLite WAL и single-writer
- Syncthing и синхронизация баз данных
- LoCoMo и LongMemEval
- гибридный поиск BM25 + ANN
- графовая память агентов

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-28 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
