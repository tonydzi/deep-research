---
dr_id: DR26-09-29-HUB-01-0839
title: "bb (get-bb) как оркестратор мульти-агентного флота: паттерны использования и интерфейсы"
date: 2026-09-29
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-09-29-HUB-01-0839): bb (get-bb) как оркестратор мульти-агентного флота: паттерны использования и интерфейсы

> Постановочный запрос deep-research (без вошедших в присланный текст ответов вендоров) о том, кто и как использует bb в проде как оркестратора агентов, как подключают несколько LLM-агентов и какие интерфейсы/альтернативы подходят для VPS-флота Антона.

## Ключевые выводы
- Присланный текст содержит только запрос/промпт DR26-09-29-HUB-01-0839 (контекст + 6 вопросов), фактические ответы вендоров (chatgpt/gemini/grok/glm/mistral) в этом фрагменте отсутствуют — тело результатов лежит отдельно в «внутренний архив лаборатории»
- Текущее состояние флота-заказчика на момент запроса: bb-app 0.41.0 работает как systemd-сервис на Linux-VPS (Hetzner, tailnet), web UI на :8443, latest версия bb на тот момент — 0.44.0
- Флот состоит из Windows-хаба с 2 GPU, Windows/Mac ноутбуков и Linux-VPS-анкера; агенты работают на подписках: Claude Code (основной), Codex, Cursor, Grok CLI, Gemini CLI, GLM, Mistral
- На узлах уже стоит 100+ рутин в Windows Task Scheduler, межмашинный синк идёт через Syncthing, координация — через Telegram-шину
- Ключевые открытые вопросы касаются архитектурного выбора: bb как главный оркестратор vs LLM-агент поверх bb (manager thread/child threads/workflow plugin), маршрутизация задач между 4-6 агентами, tailnet-only vs bb connect relay (риск неаутентифицированного API), конфликт bb Automations vs внешний планировщик (systemd timers/Task Scheduler)
- Отдельно поставлен вопрос про грабли версий 0.42–0.44 (workflow plugin, dispatch queue, concurrency limit, split views, plugin safe mode) — что зрелое, а что experimental_
- Запрошено сравнение с альтернативами: OpenClaw, Conductor, Vibe Kanban, omp, claude-squad — по критерию web-UI-поверх-CLI-агентов для сценария VPS+флот+свои подписки

## Рекомендации / решения
- Прочитать фактическое тело результатов ДР по пути «внутренний архив лаборатории» — в присланном фрагменте ответов нет, дистилляция на их основе невозможна без повторного запроса с полным телом
- После получения реальных ответов — свести в целевую схему для флота (что оркестратор, где UI, как подключить 4-6 агентов) и явно оценить риск bb connect relay (API без аутентификации) против tailnet-only варианта

## Сущности
- **Люди:** —
- **Компании:** Hetzner
- **Продукты/инструменты:** bb (get-bb, getbb.app), Claude Code, Codex, Cursor, Grok CLI, Gemini CLI, GLM, Mistral, Syncthing, Telegram, Windows Task Scheduler, tailnet, OpenClaw, Conductor, Vibe Kanban, omp, claude-squad, ACP

## Открытые вопросы
- Кто и как реально использует bb в проде/ежедневно (живые кейсы из блогов, X/Twitter, HN, Reddit, YouTube, GitHub) — bb как главный оркестратор или LLM-агент поверх bb?
- Как люди подключают к bb несколько разных агентов одновременно и как делят между ними работу (планирование/исполнение/ревью)?
- Какие интерфейсы bb реально используются для headless-сервера на VPS (web app, desktop AppImage, CLI, HTTP API, iOS/PWA, bb connect relay) и что безопаснее — tailnet-only или relay?
- Как решается конфликт «два планировщика» (bb Automations vs systemd timers/Task Scheduler), есть ли кейсы миграции существующих cron-рутин внутрь bb?
- Что из нового в bb 0.42–0.44 (workflow plugin, dispatch queue, concurrency limit, split views, plugin safe mode) реально работает для мульти-агентной оркестрации, а что experimental_?
- Есть ли среди альтернатив (OpenClaw, Conductor, Vibe Kanban, omp, claude-squad) решение с лучшим web-UI-поверх-CLI-агентов для сценария VPS+флот+свои подписки?

## Источник
- DR-ID `DR26-09-29-HUB-01-0839` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»

## Связано
- bb-orchestrator
- multi-agent-fleet-routing
- tailnet-vs-relay-security
- automation-scheduler-conflict
- cli-agent-web-ui-alternatives
