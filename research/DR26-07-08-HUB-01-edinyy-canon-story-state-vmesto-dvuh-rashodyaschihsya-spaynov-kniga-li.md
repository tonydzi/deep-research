---
dr_id: DR26-07-08-HUB-01
title: "Единый canon story-state вместо двух расходящихся спайнов (книга + live-лента)"
date: 2026-07-08
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-08-HUB-01): Единый canon story-state вместо двух расходящихся спайнов (книга + live-лента)

> Как для сериализованного нон-фикшн проекта (основатель + AI-кофаундер) построить один канонический источник истины, из которого книга и живая лента — это лишь разные проекции, а не два независимых спайна.

## Ключевые выводы
- TV writers rooms и франшизы (Lucasfilm Story Group, Marvel editors) держат один слой управления каноном с единственным финальным творческим авторитетом (шоураннер), вокруг которого continuity-роли предотвращают накопление противоречий между эпизодами/сезонами
- Transmedia-практика (Henry Jenkins) требует одного канона с медиа-специфичным native-вкладом каждого носителя, а не идентичных копий — книга даёт hindsight/payoff, живая лента — immediacy/anticipation, различаясь только timing, emphasis и hindsight-framing, не фактами
- Structured-content практика (Sanity, Contentful, Kontent.ai) учит моделировать ресурс (сущности: сезон, каст, арка, бит, петля, правило), а не 'страницу'/канал — 'книжный спайн' и 'live-state' это page-shaped выходы, а не источник истины
- Obsidian YAML front matter + Bases + Dataview достаточны, чтобы каноничный markdown-vault был queryable как лёгкая база данных без отдельной CMS; Astro/Hugo/Jekyll/Eleventy поддерживают front matter как данные, Astro умеет валидировать схему коллекции
- Для нон-фикшн обязателен явный truth-status (SPJ ethics code: точность и коррекции важнее скорости) — каждый канонический item должен иметь статус мира (planned/in_progress/happened/canceled/corrected) и truth_mode (observed/confirmed/inferred/speculative)
- Тренд последних 12 месяцев в AI-инструментах — checked-in project-context файлы и явные memory-слои (Claude Code CLAUDE.md, OpenAI AGENTS.md, LangChain namespaced memory, Sudowrite Story Bible, Novelcrafter Codex, экспериментальный Narrative Context Protocol) — конвергенция к agent-readable canon files, а не к магической связности из чата
- Главный failure mode — две авторитетные нарративные ветки, расходящиеся во времени (что уже есть у Антона); подводный failure mode — over-unification (один монолит на всё, где все мешают друг другу) и 'page-shaped modeling' (моделирование по каналам вместо сущностей)
- Автоматизация/build-степ может стать второй скрытой властью над каноном (кейс: автор Obsidian→Astro пайплайна стал 'почти full-time conflict manager' и столкнулся с security-рисками при публичной сборке из приватного vault) — приватный canon-vault и публичный output-пайплайн должны быть разделены

## Рекомендации / решения
- Создать единую папку canon/ (season.md, cast/, arcs/, beats/, loops/, rules/, motifs/, scoreboard/) с одной схемой и одним редактором-владельцем; книга и живая лента становятся read-only проекциями projections/book/ и projections/live/, которые никогда не редактируются вручную
- В каждой канонической записи вести три независимые оси: world status (planned|in_progress|happened|canceled|corrected), truth_mode (observed|confirmed|inferred|speculative), reveal policy (live_after|live_hold|book_chapter_hint|spoiler_until) — это снимает необходимость в отдельных 'прошлое-время' и 'будущее-время' канонах
- Правило: новое развитие сюжета сначала фиксируется как beat-заметка (append-first), затем из неё обновляются season/arc/loop/scoreboard заметки (summarize-second) — никогда не редактировать проекции напрямую как источник
- Book-проекция читает только beats со статусом happened/corrected, отсортированные по story_day; live-проекция читает недавние happened-beats после public_live_after + открытые loops + явно помеченные planned/experimental записи
- Строить сначала нативные Obsidian Bases-вьюхи (open-loops.base, live.base, book.base, scoreboard.base) поверх markdown+YAML, и только при реальной потребности добавлять скрипты (validate_canon.py, build_live_projection.py, build_book_projection.py) с pre-commit + yamllint, чтобы битый YAML не коммитился
- Миграция со старых двух спайнов — forward-only: заморозить старые файлы → классифицировать их содержимое по 6 корзинам (season/cast/arc/beat/loop/channel-copy-only) → пересобрать сезон хронологически как beats → заменить старые спайны на read-only обёртки, указывающие на новые проекции
- Держать channel-специфичный язык (тизеры, CTA, риторику глав) вне канона — в styles/live-style.md и styles/book-style.md, чтобы не 'сплющивать' медиа до одного голоса
- AI-память (auto-memory, project memory) использовать только как recall-слой поверх markdown-канона, а не как источник обязательных правил — обязательные правила живут в checked-in canon-файлах

## Сущности
- **Люди:** Henry Jenkins
- **Компании:** Lucasfilm, Marvel, Contentful, Sanity, Kontent.ai, Anthropic, OpenAI, LangChain, Sudowrite, Novelcrafter
- **Продукты/инструменты:** Obsidian Properties, Obsidian Bases, Dataview, Astro, Hugo, Jekyll, Eleventy, Arc Studio, CLAUDE.md, AGENTS.md, Narrative Context Protocol, yamllint, pre-commit, Story Bible (Sudowrite), Codex (Novelcrafter)

## Открытые вопросы
- Как конкретно провести миграцию существующих (уже расходящихся) книжного спайна и live show-state в новую beat-структуру без потери уже накопленного контекста — отчёт даёт общий план (5 фаз), но не проверен на реальных файлах Антона
- Насколько глубоко документирован реальный build-in-public workflow 'книга + live-лента' у других создателей — отчёт прямо отмечает, что эта часть evidence base слабая (в основном отдельные Obsidian→Astro кейсы, не полноценные canon-системы)
- Нужен ли build-степ (Astro validation) вообще, или хватит native Obsidian Bases — решение зависит от того, насколько вырастет объём канона
- Кто выступает единственным редактором канона в реальном workflow Антона (сам Антон или AI-кофаундер) и как это соотносится с multi-agent role discipline

## Источник
- DR-ID `DR26-07-08-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- episode-adapters-content-factory
- reality-show-1to1-contract
- second-brain-northstar
- vault-data-architecture
- cofounder-identity
- everything-becomes-content
- concept-creation-rules
- no-orphan-notes-rule
