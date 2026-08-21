---
dr_id: DR26-07-07-HUB-07
title: "Канон-дистрибуция скриптов на гетерогенном флоте (Windows хаб + Win/macOS узлы, Syncthing)"
date: 2026-07-07
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-HUB-07): Канон-дистрибуция скриптов на гетерогенном флоте (Windows хаб + Win/macOS узлы, Syncthing)

> Как надёжно распространять ~30 канон-скриптов/конфигов с always-on хаба на разнородный флот машин (без MDM/CI) с учётом локальных фиксов, отката и верификации.

## Ключевые выводы
- Grok сравнил 4 подхода: (a) манифест-хэши+ночной агент, (b) receive-only Syncthing-шара, (c) Git-pull с хаба, (d) staged/canary rollout — по критериям локальные фиксы, rollback, verify, offline-узлы, кроссплатформенность, АК-47-простота, безопасность, версии/drift, overhead
- Рекомендован гибрид: Git как источник истины на хабе + Syncthing sendonly/receiveonly для P2P-доставки (особенно офлайн-узлам) + тонкий Python-агент на узлах (selective apply/verify/rollback/приоритет оверрайдов) + Git-ветки для canary
- Чистый Syncthing-only проще для старта, но теряет историю/merge; чистый Git мощнее, но хуже для офлайн-узлов (это компенсирует Syncthing); чистый манифест-агент дублирует то, что уже дают Git+Syncthing
- Локальные фиксы узла никогда не клобберятся автоматически: хранятся в отдельной local_overrides/ (приоритет над канон-копией), апстрим идёт через proposals/node-uuid/ или diff в чат → хаб-мейнтейнер вручную вливает в Git
- Безопасность через minisign (Ed25519): хаб подписывает manifest.json, агент на узле верифицирует подпись перед apply и fail-closed при провале верификации (защита от tamper/авто-исполнения скомпрометированного кода)
- Verify-after-apply: агент считает финальный sha256-хэш применённых файлов и репортит в шину сообщений (node, canon_version, git_commit, manifest_hash, timestamp) — единая точка контроля дрейфа по флоту
- Даны конкретный псевдокод Python-агента apply_canon.py (sha256, verify_signature через minisign, backup перед overwrite, platform-aware apply) и 5-шаговая схема «фикс с узла → канон» (override → propose → review → sign → sync → apply+verify)
- Поэтапный план внедрения на живом флоте без остановки: Phase 0 (Git+minisign+canon-dist, 1-2 дня) → Phase 1 pilot на canary-узле (3-7 дней, старый ручной transit ещё жив параллельно) → Phase 2 rollout по группам Windows→macOS (1-2 недели) → Phase 3 staged+hardening (ветка canary, enforce signing) → Phase 4 cleanup (отключить ручной способ)

## Рекомендации / решения
- Внедрить Git-репозиторий на хабе как источник истины для канон-скриптов вместо чисто-файлового Syncthing-подхода
- Добавить minisign (Ed25519) подпись manifest.json на хабе и обязательную верификацию подписи на узлах перед apply (fail-closed)
- Выделить local_overrides/ как защищённую от перезаписи зону на каждом узле с приоритетом над канон-копией
- Реализовать канал upward-фиксов через proposals/node-uuid/ (Syncthing, хаб receive) вместо прямого автослияния изменений с узла
- Внедрять поэтапно (Phase 0-4) на живом флоте: сначала canary-узел, затем Windows-группа, затем macOS, со старым ручным способом как fallback до полного перехода
- Использовать шину сообщений для verify-репортов после apply (canon_version + manifest_hash) для детекта дрейфа по всему флоту

## Сущности
- **Люди:** —
- **Компании:** Grok, SuperGrok
- **Продукты/инструменты:** Syncthing, Git, minisign, Ed25519, Python, Task Scheduler, launchd

## Открытые вопросы
- Как именно генерировать и автоматически обновлять manifest.json на хабе при каждом коммите (пост-коммит хук описан кратко, детали интеграции не раскрыты)
- Не проработан механизм автоматизации proposals (помечен как опциональный/будущий, 'automated proposals (опц.)')
- Нет данных о нагрузке/стоимости обслуживания гибридной схемы в реальной эксплуатации (только оценка 'Setup/обслуживание: Средне-высокое' качественно)
- Не уточнено, как разрешать конфликты, если сразу несколько узлов предлагают разные фиксы одного файла одновременно

## Источник
- DR-ID `DR26-07-07-HUB-07` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- machine-bus-telegram-rail
- one-system-propagate
- config-safety-backup-and-migration-check
- coordinate-sessions-before-sensitive-edit
- claude-skills-git-backup
- onair-board
- decision-2026-07-24-peer-scripts-delivery-channel — тот же вопрос доставки скриптов на гетерогенный флот
