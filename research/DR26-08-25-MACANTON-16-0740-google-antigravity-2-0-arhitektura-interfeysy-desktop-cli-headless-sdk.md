---
dr_id: DR26-08-25-MACANTON-16-0740
title: "Google Antigravity 2.0: архитектура, интерфейсы (Desktop/CLI/headless/SDK/Managed API), пр"
date: 2026-08-25
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-25-MACANTON-16-0740): Google Antigravity 2.0: архитектура, интерфейсы (Desktop/CLI/headless/SDK/Managed API), применимость для second brain и AI-личности

> Ресёрч выясняет, что такое Google Antigravity, как он доступен (CLI/headless/browser/SDK), зачем его использовать вместо альтернатив, и насколько он пригоден для построения second brain и искусственной личности.

## Ключевые выводы
- После релиза Antigravity 2.0 (19 мая 2026) продукт стал agent-first платформой: отдельное desktop-приложение (уже не IDE), Antigravity CLI (agy), headless-режим CLI, Python SDK, IDE-расширения и managed Antigravity Agent через Gemini API — все поверхности используют общий agent harness.
- Оценки пригодности по задачам: автономная разработка 9/10, deep research 8.5/10, second brain как orchestration layer 8/10 (но долговременную семантическую память надо строить самому), искусственная личность 8/10 (нужен отдельный memory store), полностью автономный production-agent 5-7/10.
- Ключевая рекомендация для second brain: не заменять Obsidian/vector DB, а использовать Antigravity как агентный слой поверх Markdown-vault (source of truth), с MCP для Google Drive/Gmail/Calendar, отдельным semantic index и custom agent, задающим правила формирования памяти.
- Custom Agents — Markdown-файл с YAML frontmatter (name, tools, mainAgent/subagent, model, commandExecutionPolicy, skills, MCP) — first-class механизм для 'личности', но НЕ даёт автоматической долговременной автобиографической памяти; требуется explicit write/read протокол.
- Headless CLI (agy -p, --output-format text/json/stream-json, --json-schema) — самый удобный API для automation/cron/CI: NDJSON-стрим с tool-events и usage telemetry, поддержка --continue/--conversation для продолжения диалога.
- Consumer-условия Google допускают использование Interactions (данные+метаданные) для улучшения ML-систем Google — для чувствительных данных (медицина, secrets) нужен enterprise-режим или локальная архитектура вместо consumer Antigravity.
- Sandbox CLI (enableTerminalSandbox) по умолчанию ВЫКЛЮЧЕН — это не то же самое, что наличие sandbox; для agent с shell-доступом рекомендуется включать явно. Флаг --dangerously-skip-permissions — анти-паттерн вне disposable VM.
- Enterprise Antigravity НЕ наследует автоматически весь compliance Gemini Enterprise: отсутствуют FedRAMP Moderate/High, IL4/IL5, часть ISO-сертификаций и SOC 1/2/3.
- Managed Agent API (Public Preview) работает в OS-isolated Linux sandbox Google с Python 3.12/Node.js 22, но environment удаляется после ~7 дней неактивности и outbound network по умолчанию unrestricted (нужен allowlist); один interaction может потреблять 100 тыс.-3 млн токенов.
- Долговременный memory API у Antigravity не обнаружен ни в SDK, ни в CLI, ни в managed-agent docs — заявления, что Antigravity сам по себе полноценный second brain, были бы преувеличением; нужен внешний memory layer (pgvector/Qdrant/LlamaIndex).

## Рекомендации / решения
- Строить second brain как: Git Markdown vault → Antigravity CLI/Desktop custom agent → 3 стартовых Skills (ingest, consolidate-memory, critic) → JSON Schema для structured output → Artifact diff → human approval → Git commit; embeddings/vector DB добавлять только когда grep/file search станет бутылочным горлышком.
- Не начинать прототип с managed cloud Antigravity Agent API — локальные Desktop/CLI лучше подходят для PKM (данные на своей машине, контроль filesystem permissions, нет риска удаления environment после неактивности).
- Явно включать enableTerminalSandbox=true и настраивать fine-grained permissions (deny/ask/allow) при работе агента с личными файлами/shell вместо доверия дефолтным настройкам.
- Разделять Personality ≠ Memory ≠ Tools ≠ Goals: identity/values задавать в agent.md, а долговременные факты хранить в отдельном versioned memory store с обязательным confirmation для identity-/health-/finance-/relationship-уровня фактов.
- Для health/sensitive second brain использовать строгий data contract (claim_type: measured/reported/literature/inference, source, confidence, approved_by_user) вместо превращения единичных наблюдений в стабильные факты.
- Использовать headless agy -p с JSON Schema для nightly memory consolidation через cron/systemd, а Remote Control — только после того как локальная permission model проверена и устраивает.

## Сущности
- **Люди:** —
- **Компании:** Google, Alphabet
- **Продукты/инструменты:** Google Antigravity, Antigravity 2.0, Antigravity CLI (agy), Gemini 3, Gemini API, Gemini Enterprise Agent Platform, Python SDK (google-antigravity), MCP (Model Context Protocol), AI Studio Agents Playground, Remote Control, Google Workspace (Gmail, Drive, Docs, Sheets, Slides, Calendar), LlamaIndex, pgvector, Qdrant, Obsidian, Gemini CLI, Google AI Pro/Ultra

## Открытые вопросы
- Нет документированного first-class durable/semantic memory API у Antigravity — требуется ли и как строить его самостоятельно поверх файловой системы или внешней БД.
- Custom Agent schema и lifecycle гарантии неочевидны — продукт быстро меняется, известна проблема зависания при неверном имени tool.
- Remote Control ещё в rollout, доступность не гарантирована всем аккаунтам.
- Реальная стоимость/квоты (pricing) плохо прогнозируемы: baseline считается по объёму агентной работы, а не по числу промптов; в документации встречаются устаревшие/противоречивые цифры (напр. hardcoded Ultra $200/mo).
- Compliance-статус Antigravity нужно перепроверять отдельно от общего Google Cloud perimeter перед regulated deployment.
- Документационные нестыковки (напр. Intel Mac системные требования) требуют проверки в актуальной консоли/аккаунте перед production rollout.

## Источник
- DR-ID `DR26-08-25-MACANTON-16-0740` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- second-brain-northstar
- self-bible-identity-layer
- vault-data-architecture
- model-routing-fable-smart
- MCP integration
- custom-agents-persona
- digital-immortality
- insight-DR-DR26-08-29-MACANTON-17-0743-google-antigravity-2-0-архитектура-интерфейсы-cli- — реестр отмечает DR-0743 как явный дубль DR-0740, та же тема Antigravity 2.0
