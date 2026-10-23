---
dr_id: DR26-07-07-HUB-05
title: "Архитектура fleet-wide памяти для мульти-машинной Claude-системы (per-node namespace vs gi"
date: 2026-07-07
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-HUB-05): Архитектура fleet-wide памяти для мульти-машинной Claude-системы (per-node namespace vs git vs event-ledger vs шина)

> Какая архитектура памяти лучше всего подходит для флота машин с Syncthing-синком, always-on хабом и offline-узлами, не теряя знания и не превышая бюджет always-loaded индекса.

## Ключевые выводы
- Сравнены 4 архитектуры: (a) per-node single-writer namespace с фолдом на хабе, (b) git-репозиторий с merge/PR-ревью, (c) SQLite/append-only event-ledger с детерминированным генератором MEMORY.md, (d) шинный релей через хаб-писатель
- (a) и (d) лидируют по конфликт-безопасности и простоте (АК-47: только папки/файлы + один скрипт-демон на хабе, без новых инструментов на узлах)
- (c) выигрывает по consistency/audit/supersede (полный replay и аудит), но проигрывает по простоте поддержки нетехнарём — требует реализации схемы событий и детерминированной replay-логики
- (b) git даёт отличную историю и supersede через коммиты/теги, но concurrent правки одного файла на двух узлах дают merge-конфликты, требует дисциплины/CI на Windows+macOS
- Рекомендуемая архитектура: (a) per-node single-writer namespace как основа + элементы append-only из (c) (никогда не редактировать существующие файлы, только создавать новые) + авто-мердж .sync-conflict через `git merge-file --union` как страховка
- Ключевая механика Syncthing: canon/ — хаб Send Only / узлы Receive Only; contribute-nodeX/ — узел Send Only / хаб Receive Only на каждый узел отдельно
- Fold-демон на хабе (Python/PowerShell + watchdog/Task Scheduler) сканирует contribute/, дедуплицирует (id/content-hash/семантика), решает фолдить ли (owner-rule почти всегда, local-lesson по AI-review), обновляет canon/ и компактный MEMORY.md (1-2 строки на факт, цель ≤200 строк)
- Supersede реализуется через frontmatter (supersedes: [old-id]) — хаб при фолде помечает старый факт deprecated+superseded_by или переносит в archive/
- Split-brain при offline-узле (неделю не на связи): кандидаты копятся локально, файловых конфликтов почти нет благодаря single-writer+Receive Only; при reconnect хаб пачкой обрабатывает накопленное, дедуп+семантический review+supersede; семантические конфликты уходят в conflict-review файл владельцу
- Миграция с текущей схемы 'хаб-вниз' обратима на каждом шаге: бэкап → добавить frontmatter существующим фактам → создать новые Syncthing-папки (canon/ + contribute-nodeX/ для каждого узла) параллельно старым → тестовый прогон → очистка старой схемы после стабилизации

## Рекомендации / решения
- Внедрить (a) per-node single-writer namespace как основную архитектуру памяти флота
- Обязательный frontmatter-шаблон для новых фактов: id, created, source_node, type, status (candidate|canon), supersedes, priority, tags
- Правило на узлах: писать только новые файлы в свой contribute-nodeX/, никогда не редактировать существующие shared-файлы (single-writer, append-only стиль)
- На хабе — fold-демон, который автоматизирует 90-95% решений о фолде (правила+LLM), критичные случаи — в review.md/лог владельцу
- Добавить периодический авто-мердж .sync-conflict файлов через `git merge-file --union` как дополнительную страховку
- Опционально: держать git только для архива/истории канона (не как основной механизм синка)
- Профилактика split-brain: узлы должны подключаться хотя бы раз в несколько дней; при долгом offline хаб приоритетно обрабатывает contribute/ этого узла

## Сущности
- **Люди:** —
- **Компании:** Grok, SuperGrok
- **Продукты/инструменты:** Syncthing, Obsidian, SQLite, git, MEMORY.md, Task Scheduler, watchdog, CRDT

## Открытые вопросы
- Нужен ли полноценный append-only event-ledger (вариант c) позже для максимального аудита и детерминизма, или (a)+страховки достаточно навсегда
- Как именно AI-review на хабе должен принимать решение fold vs conflict для local-lesson фактов (правила не детализированы полностью)
- Оптимальный интервал/триггер для приоритетной обработки contribute/ узла, долго бывшего offline, не задан численно

## Источник
- DR-ID `DR26-07-07-HUB-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- machine-bus-telegram-rail
- one-system-propagate
- memory-index-hygiene
- always-on-memory-pilot
- machine-governance-leader-follower
- own-fleet-peer-equality
- insight-DR-DR26-06-27-HUB-04-memory-architecture-patterns-for-long-running-ai-a — развитие тех же паттернов working/long-term memory на fleet-wide уровень
