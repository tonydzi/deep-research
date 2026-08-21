---
dr_id: DR26-07-04-HUB-08
title: "Журнал задач done+undone для Claude Code: markdown-in-Obsidian vs Beads/Task Master/Backlo"
date: 2026-07-04
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-08): Журнал задач done+undone для Claude Code: markdown-in-Obsidian vs Beads/Task Master/Backlog.md/GitHub Issues/Linear

> Какой инструмент/архитектура лучше всего подходит под требования Антона (локально, без SaaS, 4+ машины, Syncthing, Obsidian, не-технарь) для ведения журнала сделанных и несделанных задач вместе с Claude Code.

## Ключевые выводы
- Лучший вариант — markdown-first журнал в Obsidian-волте (одна задача = одна заметка), а текущий SQLite становится не source of truth, а производным индексом/кэшем для HTML-дашборда и аудита.
- Встроенные tasks Claude Code (TaskCreate/TaskUpdate/TaskGet/TaskList, hooks TaskCreated/TaskCompleted) — это операционный внутрисессионный слой, а не межмашинный backlog: сессии и auto memory по умолчанию machine-local (~/.claude/projects/), общий multi-host resume требует отдельного SDK SessionStore.
- Beads (Dolt-powered AI-native issue tracker, 25k+ stars) силён для dependency-aware мультиагентной работы, но CLI-first, требует embedded Dolt (single-writer lock) или отдельный dolt sql-server, синк через Dolt remotes — не совпадает с принципом 'локально + чинится не-технарём'; часть комьюнити считает его раздутым.
- Backlog.md — markdown-native task manager с zero-config CLI и локальным web UI, ближе всего к нужной модели, но центрирован на отдельной папке бэклога внутри проекта, а не на уже существующем Obsidian-вольте с wikilink-графом.
- Task Master — мощный PRD-to-tasks движок с зависимостями и research mode, но тяжёлый (MCP-сервер по умолчанию грузит 36 tools ~21k токенов), избыточен для соло-оператора.
- GitHub Issues и Linear/Jira MCP отпадают по определению, так как требуют внешнего SaaS/cloud-hosted MCP как source of truth, что запрещено условиями Антона.
- SQLite как главный синкаемый артефакт между 4+ машинами через Syncthing — плохая идея: официальная документация SQLite предупреждает о ненадёжности сетевого/синкаемого locking (особенно на Windows), а Syncthing при конфликте создаёт файлы .sync-conflict, что плохо для live database, но нормально для markdown-текста.
- Obsidian (Bases, Dataview, Tasks plugin, backlinks/wikilinks) масштабируется до сотен тысяч аннотированных заметок и даёт 'бесплатную' перелинковку задач с происхождением (born_from), в отличие от отдельных tracker-инструментов.
- Минимально достаточная схема полей: id/title/state/type/priority/created_at/touched_at/born_from/blocks/blocked_by/supersedes/evidence/outcome/review_after; статусы inbox/open/wip/blocked/done/dropped/someday/archived; поле unblocks не нужно (выводимо из blocked_by).
- Evidence-gated close (нельзя закрыть done без evidence), разделение someday/dropped (GTD Someday/Maybe должен регулярно пересматриваться) и обязательный weekly review — критичные механики против протухания журнала.

## Рекомендации / решения
- Сделать /Tasks/*.md в Obsidian-волте источником истины (одна задача — одна заметка с YAML frontmatter), а tasks.db на always-on хабе — перестраиваемым read-model индексом для HTML-дашборда и ночного аудита.
- Использовать Claude Code hooks (UserPromptSubmit, TaskCreated, TaskCompleted) как auto-capture bus, зеркалирующий события встроенного task-слоя в markdown-ledger, не делая native tasks источником истины.
- Оставить два UI-слоя: Obsidian (Bases cards/table + Dataview + Tasks plugin) как живую рабочую панель для редактирования, статический HTML — только как read-only витрина/snapshot, не редактор.
- Не вводить корпоративные поля (epic/sprint/story points/assignee/team/milestone) при одном операторе — риск протухания журнала.
- Ввести жёсткие правила: evidence обязателен перед done; не более 3 одновременных wip; someday и dropped не смешивать; stale-флаг по порогам приоритета (P0=1д, P1=7д, P2=21д, P3=45д, someday=90д на re-review) вместо автоэскалации приоритета.
- Мигрировать поэтапно: (1) экспорт текущего SQLite в markdown с сохранением id как внешнего ключа; (2) добавить новые поля (born_from, blocks/blocked_by, supersedes, review_after, outcome) только для новых задач; (3) развернуть направление синка на Markdown → SQLite → HTML; (4) поднять hooks на всех машинах через shareable .claude/settings.json; (5) обучить единому набору правил по вводу задач; (6) через 2-4 недели официально понизить tasks.db до кэша и убрать старый write-path.
- Добавить weekly review как отдельный ритуал (по аналогии с GTD Weekly Review) в дополнение к уже существующему ночному аудиту.

## Сущности
- **Люди:** David Allen
- **Компании:** Anthropic, GitHub, Atlassian, Linear
- **Продукты/инструменты:** Claude Code, Claude Agent SDK, TaskCreate/TaskUpdate/TaskGet/TaskList, Beads, Dolt, Backlog.md, Task Master, GitHub Issues, gh CLI, Linear MCP, Jira MCP (Rovo), Obsidian, Obsidian Bases, Dataview, Tasks plugin, SQLite, Syncthing, GTD (Getting Things Done)

## Открытые вопросы
- Нет прямой цитаты в официальной документации Claude Code о том, что встроенные tasks НЕ предназначены для межсессионного общего backlog — вывод сделан как сильная экстраполяция из документации и практики, не буквальная цитата.
- Точный UX паттерн для hooks-capture (все prompts vs специальные префиксы vs отдельная skill-команда vs явное намерение пользователя) не определён — требует выбора по реальной практике использования.
- Не проверено на практике (пилотно), как поведёт себя схема при реальной миграции текущего SQLite-реестра — шаги миграции предложены, но не протестированы.

## Источник
- DR-ID `DR26-07-04-HUB-08` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-04-HUB-08-журнал-задач-done-undone-для-claude-code-под.md`

## Связано
- [[ak47-simplicity]]
- [[vault-data-architecture]]
- [[deterministic-script-gotchas]]
- [[multi-agent-role-discipline]]
- [[second-brain-northstar]]
- [[one-system-propagate]]
- [[hanging-tasks-dashboard]]
- [[prefer-visual-dashboards]]
