---
dr_id: DR26-07-04-HUB-05
title: "Журнал задач done+undone для Claude Code: markdown-в-Obsidian vs Beads/Backlog.md/Task Mas"
date: 2026-07-04
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-05): Журнал задач done+undone для Claude Code: markdown-в-Obsidian vs Beads/Backlog.md/Task Master/GitHub Issues

> Какая архитектура журнала задач (сделано+не сделано, с перелинковкой родословной/блокировок) лучше всего подходит под стек Антона (локально, Obsidian, SQLite, Syncthing, 4+ машины, не-технарь), и что для этого реально используют в комьюнити Claude Code.

## Ключевые выводы
- Встроенные tasks Claude Code (TaskCreate/TaskUpdate/TaskList, hooks TaskCreated/TaskCompleted) — операционный внутрисессионный слой; транскрипты и auto memory по умолчанию machine-local, общий межмашинный backlog нужен отдельно (SDK SessionStore) — то есть нативные tasks не годятся как главный ledger между машинами.
- PLAN.md/spec-файлы — массовая практика (plan-before-edit, fresh session per phase), но один файл, который одновременно спека+backlog+журнал+решения, быстро превращается в противоречивую свалку — план должен оставаться companion-артефактом, не заменять журнал состояний.
- Beads (Dolt-powered AI-native issue tracker, 25k+ stars на GitHub) даёт dependency-aware execution и hash-based IDs для параллельных агентов, но CLI-first, требует Dolt (embedded single-writer или отдельный sql-server), синк через `bd dolt push/pull` — тяжело в обслуживании под AK-47 и не-технаря; часть комьюнити уже считает его 'раздутым'.
- Backlog.md (markdown-per-task, offline, browser UI/kanban) — хороший референс-дизайн 'проще чем Beads', но центрирован на отдельной папке-бэклоге внутри проекта, а не на уже существующем Obsidian-волте с wikilink-графом.
- Task Master (PRD→tasks, 27k+ stars) — мощный, но тяжёлый: отдельная `.taskmaster`-экосистема, MCP-сервер грузит по умолчанию 36 tools (~21k токенов) — оверкилл для solo-оператора поверх уже существующего второго мозга.
- GitHub Issues/gh CLI и Linear/Jira MCP технически зрелые (sub-issues, dependencies GA, `@claude` GitHub Action), но требуют внешний SaaS/cloud-hosted MCP как source of truth — прямо конфликтует с требованием 'локально, без SaaS'.
- SQLite как главный синкаемый через Syncthing артефакт — плохая идея: официальные доки/FAQ SQLite предупреждают о ненадёжности блокировок на сетевых/шаренных файлах (особенно Windows) и риске corruption при конкурентной записи с разных машин; Syncthing на конфликте создаёт `.sync-conflict-*` копии, что нормально для текста, но плохо для живой БД.
- Рекомендуемая архитектура: одна задача = одна markdown-заметка в Obsidian-волте (source of truth) с полями id/title/state/type/priority/created_at/touched_at/born_from/blocks/blocked_by/supersedes/evidence/outcome/review_after; Claude Code hooks (UserPromptSubmit, TaskCreated, TaskCompleted) мгновенно зеркалят события в эти заметки; SQLite и HTML становятся derived read model (перестраиваемый индекс/дашборд на хабе), не первичным хранилищем.
- Статусы: inbox/open/wip/blocked/done/dropped/someday/archived — критично не путать `someday` (GTD Someday/Maybe, регулярный пересмотр) и `dropped` (осознанный отказ); закрытие `done` без `evidence` считается неполным (evidence-gated close); `stale`-флаг по порогам приоритета (P0=1д, P1=7д, P2=21д, P3=45д, someday=90д) вместо авто-эскалации приоритета; weekly review обязателен (по GTD Дэвида Аллена, иначе журналу перестают доверять).
- UI-разделение: Obsidian (Bases/Tasks/Dataview, масштабируется до сотен тысяч заметок) — живая рабочая панель для редактирования (3 страницы: Несделанные/Журнал сделанных/Когда-нибудь); статический HTML остаётся витриной-снапшотом для ночного отчёта, не редактором.

## Рекомендации / решения
- Перевести главный реестр задач из SQLite в markdown-заметки Obsidian (одна задача = один файл), SQLite оставить только как перестраиваемый индекс/кэш на всегда-включённом хабе.
- Поднять Claude Code hooks (UserPromptSubmit, TaskCreated, TaskCompleted) как auto-capture bus, зеркалирующий встроенные tasks во внешний markdown-ledger — сессионные tasks остаются лишь фронтендом исполнения.
- Сохранить два UI-слоя раздельно: Obsidian — для живого редактирования и связей, HTML — только как read-only снапшот/дашборд.
- Взять минимальный набор из 14 полей и 4 типов связей (born_from, blocks/blocked_by, supersedes, related); не заводить корпоративные поля (epic/sprint/story points/assignee) при одном операторе.
- Внедрить evidence-gated close (нет 'done' без evidence), лимит WIP ≤3 активных задач, обязательный weekly review, отдельный ревью-цикл для `someday` (не смешивать с `open`/`dropped`).
- Миграция в 6 шагов: (1) экспортировать текущий SQLite в markdown-заметки с сохранением старого task_id; (2) добавить новые поля только для новых записей, не ретрокодировать историю; (3) развернуть направление синка на Markdown→SQLite→HTML; (4) раскатать hooks через `.claude/settings.json` на всех машинах (учитывая грабли exec-form на Windows); (5) завести 3 страницы в Obsidian и одно простое правило дисциплины; (6) через 2-4 недели официально понизить SQLite до кэша, старый write-path — только как аварийный fallback.
- Не брать Beads/Task Master/GitHub Issues/Linear-Jira MCP как основной слой — рассматривать их только как источник заимствованных идей дизайна (особенно Backlog.md).

## Сущности
- **Люди:** David Allen (GTD)
- **Компании:** Anthropic, GitHub, Atlassian, Linear
- **Продукты/инструменты:** Claude Code, Agent SDK, Beads, Dolt, Backlog.md, Task Master, GitHub Issues, gh CLI, Linear MCP, Jira/Rovo MCP, Obsidian, Bases, Dataview, Tasks plugin, SQLite, Syncthing

## Открытые вопросы
- Нет прямой официальной формулировки Anthropic, что встроенные tasks НЕ предназначены для межсессионного общего backlog — вывод сделан из совокупности документации (session/auto memory machine-local, SessionStore отдельно) и практики комьюнити, не из буквальной цитаты.
- Точный UX триггера hooks для capture (все prompts vs явные намерения vs отдельная skill-команда) не определён — нужно калибровать по реальным разговорам Антона с Claude Code.

## Источник
- DR-ID `DR26-07-04-HUB-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- ak47-simplicity
- one-system-propagate
- machine-bus-telegram-rail
- hanging-tasks-dashboard
- prefer-visual-dashboards
- second-brain-northstar
- task-assignment-by-machine
