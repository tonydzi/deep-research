---
dr_id: DR26-08-29-MACANTON-17-0743
title: "Google Antigravity 2.0: архитектура, интерфейсы (CLI/headless/desktop/managed API) и приго"
date: 2026-08-29
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-29-MACANTON-17-0743): Google Antigravity 2.0: архитектура, интерфейсы (CLI/headless/desktop/managed API) и пригодность для second brain и AI-личности

> DR исследовал, что такое Google Antigravity, как он доступен (CLI/headless/браузер) и какие топ-кейсы для second brain и искусственной личности он даёт.

## Ключевые выводы
- После Antigravity 2.0 (19.05.2026) продукт стал agent-first platform: отдельное desktop-приложение (уже НЕ IDE), Antigravity CLI (agy), headless-режим CLI, Python SDK, IDE-расширения и managed Antigravity Agent через Gemini API — общий agent harness лежит в основе всех поверхностей
- Оценки пригодности: автономная разработка 9/10, Deep Research 8.5/10, second brain/PKM 8/10 (как orchestration layer), искусственная личность 8/10, полностью автономный production-agent 5-7/10
- Для second brain рекомендуемая архитектура: Markdown-vault (Git) как source of truth + custom agent + Skills + MCP (Drive/Gmail/Calendar) + опциональный semantic index (pgvector/Qdrant/LlamaIndex), где Antigravity — активный агентный слой, а не замена Obsidian/vector DB
- Durable/long-term personal memory API у Antigravity НЕ обнаружен ни в SDK, ни в CLI, ни в Managed Agent docs — для персонality continuity нужен отдельный explicit memory store с write/read protocol, полагаться на conversation history нельзя
- Custom Agents — Markdown-файл с YAML frontmatter (name, tools, mainAgent/subagent, model, commandExecutionPolicy, skills, mcpServers) — это готовый механизм задать 'личность', но не память; известный баг: неправильное имя tool может подвесить агента
- Terminal sandbox (enableTerminalSandbox) в CLI по умолчанию ВЫКЛЮЧЕН (false) — для autonomous agent с доступом к shell его нужно включать явно; флаг --dangerously-skip-permissions практически анти-паттерн вне disposable VM
- Consumer/individual Antigravity Terms допускают использование 'Interactions' (данные+метаданные) для улучшения продуктов и ML Google, доступ могут иметь сотрудники/контракторы — для highly sensitive данных (медицина, секреты) нужен opt-out или enterprise/local архитектура
- Antigravity в Gemini Enterprise НЕ наследует автоматически весь compliance родительского Google Cloud: явно отсутствуют FedRAMP Moderate/High, IL4/IL5, ряд ISO-сертификаций и SOC 1/2/3
- Managed Agent работает в изолированном Linux sandbox Google (Public Preview), но outbound network по умолчанию unrestricted (нужен явный allowlist); environment может удаляться после ~7 дней неактивности; один interaction может потреблять ~100 тыс.–3 млн токенов
- Consumer Gemini CLI сворачивается в пользу Antigravity CLI (обслуживание для free/Pro/Ultra прекращено с 18.06.2026); Antigravity 2.0 docs на 24.08.2026: Desktop v2.9.1, CLI v1.1.17, SDK v0.1.13

## Рекомендации / решения
- Не начинать прототип second brain с managed cloud Antigravity Agent API — использовать локальные Desktop/CLI, т.к. данные остаются на машине, проще контролировать filesystem permissions и есть Git-история vault
- Строить second brain по схеме: Git Markdown vault → Antigravity CLI/Desktop → custom agent + Skills → MCP (сначала read-only Drive/Docs, потом Gmail/Calendar) → опционально vector DB только когда grep/file search станет реальным bottleneck
- Использовать Artifacts как audit boundary (ingest → propose memory update → diff → human approval → commit), а не давать агенту напрямую переписывать заметки
- Явно включать enableTerminalSandbox=true и настраивать permissions (deny/ask/allow) вместо --dangerously-skip-permissions при работе с персональными файлами
- Начинать POC с минимум 3 Skills (ingest, consolidate-memory, critic), не с двадцати — progressive disclosure работает лучше при явном description
- Для чувствительных данных (медицина, секреты) не отправлять их в consumer Antigravity без проверки текущего data-collection toggle; рассматривать enterprise/local архитектуру отдельно
- Использовать headless CLI (agy -p, --output-format json/stream-json, --json-schema) как основной automation/CI слой для nightly memory consolidation через cron/scheduled task
- Отделять claim_type (measured/reported/literature/inference) в структурированных заметках памяти, чтобы агент не превращал единичное наблюдение в стабильный факт

## Сущности
- **Люди:** —
- **Компании:** Google, Alphabet
- **Продукты/инструменты:** Google Antigravity, Antigravity 2.0, Antigravity CLI (agy), Gemini 3, Gemini API, Gemini CLI, Python SDK (google-antigravity), MCP (Model Context Protocol), Google Workspace, Google Drive, Gmail, Calendar, AI Studio Agents Playground, Remote Control, Vertex AI, LlamaIndex, pgvector, Qdrant, Obsidian, Google One, Gemini Enterprise Agent Platform

## Открытые вопросы
- Отсутствует документированный first-class semantic/episodic personal memory API у Antigravity — неясно, планирует ли Google его добавить
- Remote Control ещё в rollout, доступность не гарантирована всем аккаунтам
- Точные ценовые квоты Pro/Ultra не публикуются явно (baseline считается по объёму агентной работы, а не по числу промптов) — необходимо измерять на своём workload
- Документационные несостыковки: поддержка Intel macOS (download selector противоречит X86 not supported) и устаревшая цена Ultra $200/mo в доках Subagents
- Compliance-ограничения Antigravity в Gemini Enterprise (отсутствие FedRAMP/IL4-5/SOC) требуют проверки перед regulated deployment
- Lifecycle guarantee схемы Custom Agent неочевиден — продукт быстро меняется, известны баги (зависание при неверном имени tool)

## Источник
- DR-ID `DR26-08-29-MACANTON-17-0743` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- second-brain-northstar
- self-bible-identity-layer
- concept-digital-immortality
- vault-data-architecture
- MCP-integration
- AI-agent-orchestration
- custom-agents-personas
