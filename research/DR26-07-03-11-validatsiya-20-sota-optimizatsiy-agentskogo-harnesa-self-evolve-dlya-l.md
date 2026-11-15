---
dr_id: DR26-07-03-11
title: "Валидация 20 SOTA-оптимизаций агентского харнеса + self-evolve для local-first стека"
date: 2026-07-03
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-03-11): Валидация 20 SOTA-оптимизаций агентского харнеса + self-evolve для local-first стека

> Исследование проверяет 20 техник оптимизации агентского харнеса (routing, кэш, граф-память, deliberation, self-evolve) применительно к Claude Code + SQLite + Markdown стеку.

## Ключевые выводы
- Большинство заявленных выигрышей (−84% стоимость routing, −56% кэш preheating) реалистичны только относительно наивного baseline; стек с уже существующим детерминированным routing и кэш-дисциплиной получает меньший прирост.
- Claude Code subscription управляет prompt caching автоматически (TTL 1 час); активный 'pre-warming' нецелесообразен — реальная оптимизация это cache hygiene: не переключать модель/effort/MCP инструменты в рамках сессии.
- Reversible context compression (Headroom) даёт 60–95% сокращение токенов при сохранении оригинала локально в SQLite; лучший по ROI приём для solo-оператора.
- HippoRAG показывает до 20% прироста на multi-hop QA, но граф-память уступает простому grep/SQL для прямых фактических запросов; правило: retrieval ladder сначала (SQL→grep→vector), граф только как overlay.
- Bi-temporal граф в SQLite (valid_from, valid_to, observed_at) устраняет 'graph rot' — устаревшие факты не удаляются, а закрываются по времени; применим без внешней инфраструктуры.
- Trace-level synthesis стабильно превосходит forced numerical consensus и Borda Count у multi-agent deliberation; принудительный консенсус подавляет ценные minority perspectives и ухудшает результат.
- Self-Harness даёт до 60% прироста производительности, но только при наличии объективных автоматических evaluators; без них система оптимизирует метрику, а не функцию (metric gaming); безопасная схема: propose→sandbox test→A/B eval→human merge.
- ChatGPT насчитал только 19 из заявленных 20 техник; 20-я техника не специфицирована и не может быть валидирована.
- Угрозы supply-chain в OpenClaw/Hermes: уязвимости loopback-аутентификации, poisoned marketplace skills с косвенными prompt injections; A2A/MCP имеют documented риски tool poisoning, confused deputy, token passthrough.
- DAG-pruning CoT сокращает среднюю длину генерации на 42%; adaptive mode switching (suppress thinking) — на 55% для низкосложных задач без потери качества.

## Рекомендации / решения
- Внедрить cache hygiene как приоритет №1: фиксировать модель/effort/MCP конфиг на старте сессии, избегать переключений в середине.
- Обернуть Claude Code в reversible compression layer (Headroom или аналог), оригинальный текст хранить в SQLite.
- Спроектировать SQLite-схему с bi-temporal edge tracking (valid_from, valid_to, observed_at, source_id, confidence) для граф-памяти вместо pure vector store.
- Соблюдать retrieval ladder: SQL facts → exact grep → vector → reranker → temporal graph → LLM synthesis; не допускать граф-экспансию в качестве первичного retrieval.
- Self-correction loop должен опираться только на внешний сигнал (tests, linter, SQL integrity, JSON schema) — не на 'перечитай и проверь себя' без верификатора; max 2 repair attempt + human review для записи в vault/config.
- Self-evolve реализовывать только в supervised режиме: патч предлагается агентом → sandbox тест → оценка по immutable baseline → merge только человеком.
- Ввести zero-trust политику для MCP/A2A: pin tools, sandbox execution, read/write разделение, блокировка token passthrough, семантическая проверка каждого внешнего Markdown-скилла перед выполнением.
- Избегать anti-pattern: pure vector store (без temporal), forced numerical consensus, unconstrained evolutionary scaffolding, unsandboxed Markdown skills от сторонних источников.
- Провести baseline замеры по классам запросов (direct fact, temporal, multi-hop, code patch, JSON state, safety) в SQLite до внедрения оптимизаций.
- Внедрить structured provenance schema (session, source, claim, entity, edge tables) — это делает 100% node-session linkage тривиальным и аудируемым.

## Сущности
- **Люди:** Kirill Simakov
- **Компании:** Anthropic, Google, OpenAI, Microsoft, Palo Alto Networks, Silverfort, Cyera
- **Продукты/инструменты:** Claude Code, HippoRAG, GraphRAG, Zep, Graphiti, Headroom, RouteLLM, FrugalGPT, AlphaEvolve, Self-Harness, APEX, OpenClaw, Hermes Agent, LLMLingua, Self-Refine, Reflexion

## Открытые вопросы
- Что представляет собой неспецифицированная 20-я техника — без этого набор нельзя считать полным.
- Каков реальный инкрементальный выигрыш dynamic routing поверх уже существующего статического 2-tier routing в конкретном стеке Антона?
- Насколько независимы от вендорских бенчмарков заявленные gains Zep/Graphiti на LongMemEval?
- Каковы безопасные границы autonomy для self-evolve loop: сколько циклов, какие evaluators минимально достаточны?
- Достаточна ли семантическая проверка secondary sub-agent для защиты от padded-file payload injections в Markdown skills?
- Применимы ли выводы о кэш-дисциплине к режиму Anthropic API (не subscription) с явным cache_control?

## Источник
- DR-ID `DR26-07-03-11` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- prompt caching
- model routing
- context compression
- temporal knowledge graph
- bi-temporal memory
- multi-agent deliberation
- trace-level synthesis
- self-evolving agents
