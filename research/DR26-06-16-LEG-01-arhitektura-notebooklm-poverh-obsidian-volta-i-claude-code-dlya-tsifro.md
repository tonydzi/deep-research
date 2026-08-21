---
dr_id: DR26-06-16-LEG-01
title: "Архитектура NotebookLM поверх Obsidian-волта и Claude Code для цифрового двойника"
date: 2026-06-16
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-16-LEG-01): Архитектура NotebookLM поверх Obsidian-волта и Claude Code для цифрового двойника

> Отчёт отвечает, какую роль NotebookLM должен играть в стеке персонального цифрового двойника (Obsidian + локальный RAG + Claude Code) и какой путь доступа (UI/Workspace, Enterprise API или неофициальный notebooklm-py) выбрать.

## Ключевые выводы
- NotebookLM лучше использовать как внешний слой 'grounded artifacts' (аудио-обзоры, учебные материалы), а не как ядро памяти/RAG — локальный vault+SQLite+RAG остаётся источником истины.
- У NotebookLM лимиты источников на notebook и изоляция notebooks — прямой импорт всего vault архитектурно плох; правильный паттерн — узкие тематические slices, отобранные Claude Code как curator.
- Три пути доступа: ручной Google Doc/NotebookLM UI (дёшево, низкий риск, мало автоматизации); Enterprise API (официально, но документированы только notebook CRUD/upload источников/audio overview, без документированного chat/query/export); notebooklm-py (мощный, но построен на undocumented API, хрупкий auth/cookies, риск по ToS).
- Enterprise: публичная цена ~$9/лицензия/мес с годовой скидкой, минимум 15 лицензий, есть 14-дневный trial на 5000 лицензий.
- Приватность слоями: personal-аккаунт — human review возможен при отправке feedback; Workspace/Education — явно no training/no human review; Enterprise — данные в GCP-проекте клиента, VPC-SC/CMEK/data residency.
- Исследования long-context RAG и summarization поддерживают curated hierarchical slice вместо full-corpus dump; PKM-исследования по Obsidian показывают, что стратегия извлечения знаний определяет качество second brain; отдельная работа подтверждает NotebookLM как grounded RAG-репетитор для обучения.
- Источники в NotebookLM — статичные копии, автосинка с vault нет (ручной re-sync для Docs/Slides, re-upload для остального) — нужен manifest с хэшами источников для идемпотентности.
- В публичной Enterprise API-документации не нашли endpoint для скачивания сгенерированного аудиофайла — 'полностью безручное' получение mp3 не полностью закрыто официально.
- notebooklm-py: основная инженерная работа проекта — борьба за стабильность auth/cookies, CSRF-refresh, обход anti-automation детекции — сигнал хрупкости для production.
- Продукт NotebookLM (Gemini 3.5, Deep Research, video overviews, Antigravity/secure cloud computer) развивается быстрее, чем документированный API-surface — разрыв между скоростью UI и зрелостью программного доступа.

## Рекомендации / решения
- Строить local-first: Claude Code как curator собирает узкие 'research packets' из vault с manifest (source hashes, privacy_class, selection_reason) до отправки в облако.
- Дефолтный путь — ручной Google Doc/Workspace NotebookLM UI с человеческим approval gate; дёшево, ремонтопригодно, использует уже оплаченную подписку.
- Переходить на NotebookLM Enterprise API только если ручной ingest стал реальным bottleneck и compliance/automation оправдывают минимум 15 лицензий; сначала 14-дневный trial.
- notebooklm-py использовать только как временный мост/прототип в изолированном аккаунте, не как основу для приватного долговременного корпуса цифрового двойника.
- Классифицировать каждый slice по privacy_class (public/internal/sensitive/do_not_upload) до загрузки; чувствительное — только Enterprise или полностью локально, никогда через personal-аккаунт.
- Артефакты NotebookLM (аудио, саммари, деки) хранить в vault как производные с provenance; в SQLite fact layer попадают только вручную подтверждённые выводы.
- Для Enterprise API предпочитать прямую загрузку файла/текста (uploadFile/batchCreate) вместо хэндоффа через Google Doc.

## Сущности
- **Люди:** —
- **Компании:** Google, Google Cloud, Infoxchange, Carbon Group, Rivian, Sonata Design, ElevenLabs, Microsoft
- **Продукты/инструменты:** NotebookLM, NotebookLM Enterprise, Claude Code, Obsidian, notebooklm-py, Gemini, Gemini 3.5, Antigravity, Google Workspace, Google AI Pro/Plus/Ultra, SQLite, Podcastfy, Muyan-TTS, Model Armor, CMEK, VPC-SC, Lore, Illuminate/Learn Your Way

## Открытые вопросы
- Нет публично документированного consumer API для NotebookLM и официального roadmap с датами.
- Нет документированного Enterprise API endpoint для скачивания сгенерированного аудиофайла.
- Нет полной публичной per-call/per-generation тарификации Enterprise API (известна только seat-цена).
- Часть Enterprise API surface помечена Preview/Pre-GA — контракты непредсказуемы.
- Юридический статус unofficial automation (notebooklm-py) относительно ToS не доказан однозначно — риск оценён как ненулевой, но не как подтверждённое нарушение.

## Источник
- DR-ID `DR26-06-16-LEG-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR-notebooklm-claude-code-2026-06-16-RU.md`

## Связано
- [[notebooklm-integration]]
- [[second-brain-northstar]]
- [[vault-data-architecture]]
- [[digital-twin]]
- [[obsidian-second-brain]]
- [[local-first-architecture]]
- [[long-context-rag]]
