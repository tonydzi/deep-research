---
dr_id: DR26-10-28-HUB-05
title: "SeeRay11/OpenFlow и визуальные конструкторы агентных workflow для AI-лаборатории"
date: 2026-10-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-28-HUB-05): SeeRay11/OpenFlow и визуальные конструкторы агентных workflow для AI-лаборатории

> Стоит ли небольшой AI-agent лаборатории брать SeeRay11/OpenFlow (визуальный multi-agent builder поверх OpenCode) и как он соотносится с Flowise, Dify, Langflow, Sim, n8n, LangGraph, CrewAI, AutoGen и Vibe Kanban.

## Ключевые выводы
- OpenFlow v1.3.0 (18.10.2026, MIT, 167 stars/15 forks) — независимый fork OpenCode, весь OpenFlow-код в packages/flow. Режимы: pipeline DAG, swarm (до 6 раундов), tree orchestration, gauntlet. Это не сетевой протокол OpenFlow и не Snowflake Openflow.
- Главное отличие — gauntlet: reviewer получает свежую сессию на каждый verdict, orchestrator не может объявить работу законченной без critic по текущей версии, у critic нет edit-инструментов, при падении всех critics run завершается ошибкой, а не ложным pass. Post-run coach детерминированный, не LLM. Независимость по контексту, но не по среде: critic читает тот же workspace.
- Исполнением владеет страница браузера: закрытие или crash прекращает orchestration, дальше ручной Resume по чекпоинтам run-лога (.openflow/runs). Завершённые узлы не перезапускаются, in-flight возвращаются к существующим сессиям. Это recovery, а не durable background worker, и нет exactly-once для внешних side effects.
- Документированных worktree или file-lease нет: все узлы работают в одном OPENFLOW_PROJECT, так что политика one-writer-per-file обязана обеспечиваться внешним lease-адаптером. Вывод помечен speculative: source-wide проверки и динамического race-теста не было.
- Безопасность: /flow/api не аутентифицирует вызывающего, по умолчанию только loopback, FLOW_ALLOW_REMOTE=1 открывает дыру на запись файлов. Режим permission по умолчанию auto выдаёт одноразовое allow на ask-действия, поэтому границей не является; нужны явные deny и «ask me». Запрет webfetch не запрещает curl через bash, настоящий egress-deny должен быть на уровне ОС или контейнера. Секреты MCP лежат в opencode.json открытым текстом, run-логи содержат prompts и outputs.
- Деньги: spend cap ($5 по умолчанию, $500 максимум) есть только у gauntlet и считается на клиенте, превышение при параллельных вызовах неизвестно. PR #24 (локальные Ollama/LM Studio, универсальный cap, оценка стоимости) на 27.10 остаётся OPEN и в v1.3.0 не входит. Бесплатный путь — shared Zen-модели с суффиксом -free (возможны 429/400).
- Подписки Claude/ChatGPT нельзя использовать скрытым backend: Anthropic запрещает сторонним продуктам OAuth Free/Pro/Max, OpenAI разрешает только поддерживаемый Sign in with ChatGPT. OpenFlow работает через API-ключи или Zen free. С 19.10.2026 часть планов Claude Max/Team включает отдельные API-кредиты, это API-биллинг, а не allowance Claude Code.
- Конкуренты оркестрируют модели и агентов, а не готовые авторизованные harness Claude Code или Codex: Flowise Agentflow V2, Dify, Langflow (v1.13 от 22.10.2026), Sim (самохост от 2 CPU/12 GB RAM), CrewAI Studio (запуск 28.07.2026), AutoGen Studio, Rivet, LangGraph/LangSmith. Ближе всех к harness-слою был Vibe Kanban, но он объявил sunsetting, новую зависимость строить на нём не стоит. LangGraph — самая сильная durability (persistence, resume, HITL), но это платформа, а не лёгкий UI.
- n8n в стеке лаборатории остаётся лучшим control plane для нe-кодовых сценариев (встреча→CRM, контент с approval): сохранённые executions, retry, human approval, с v2.45 idempotency keys, но только для /api/v1 POST и с кэшем успешного ответа 12 ч, то есть не универсальный exactly-once.
- Независимых данных о принятии OpenFlow почти нет: два HN-поста без обсуждений (баллы нестабильны), автор в Reddit обещал улучшить provenance изменений файлов по агентам. Число коммитов унаследовано от OpenCode и размер команды не показывает.

## Рекомендации / решения
- Провести один локальный одноразовый shadow-pilot OpenFlow v1.3.0 (зафиксированная версия, не dev и не PR #24) только для сценария code planner → builder → fresh-context reviewer и проверки видимости и recovery; уверенность в рекомендации около 0,82.
- Не использовать OpenFlow как замену SQLite lease/task runtime и n8n и не переносить в него source-of-truth leases, scheduler и task IDs; схема: общий источник истины → тонкий адаптер → OpenFlow pipeline.
- Бюджет пилота $0: только Zen -free, без платных API-ключей и подписочных токенов; при нехватке бесплатной квоты фиксировать INCONCLUSIVE_PROVIDER_CAPACITY, а не переходить на платный backend.
- Изоляция пилота: отдельный git-проект и профиль без реальных креденшелов и приватных данных, только loopback, без MCP в первом проходе, parallelism 1, egress-deny на уровне ОС, явный deny плюс «ask me» вместо default auto.
- Сравнивать с baseline (текущий harness, роли writer/reviewer, task/lease ledger) по метрикам: корректность на одном наборе тестов, независимость reviewer, нарушения владения файлами, повторные side effects, полнота recovery, время ремонта оператором, видимость, полнота аудита, RSS и CPU. Порог улучшения (например 25–30%) зафиксировать до теста.
- Набор отказов для пилота: закрытие браузера и crash процесса, 429 (только через тестовый double), два builder на один файл (негативный контроль), недоступный reviewer, запрещённый outbound, запись вне проекта, прерванный псевдо-side-effect на локальном mock-ledger.
- Новый общий визуальный builder брать только по необходимости: Flowise Agentflow V2 как лучший прямой конкурент, Dify для бизнес-процессов с триггерами и Human Input; LangGraph, Temporal или Restate рассматривать, лишь если узким местом станет собственная state machine с crash/retry/idempotency; Vibe Kanban и Sim не брать.
- Переход к интеграции OpenFlow возможен, если пилот даст выигрыш, появится внешний lease-мост, universal cap (PR #24) будет смержен и проверен, а исполнение станет фоновым или UI ляжет поверх вашего scheduler; вариант «оставить как есть», если трассировки n8n и harness уже позволяют так же быстро понять статус и точку восстановления.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, OpenCode, Microsoft, CrewAI, LangChain (LangSmith), Temporal, Restate, BloopAI
- **Продукты/инструменты:** —

## Открытые вопросы
- Максимальное превышение spend cap при параллельных запросах в gauntlet неизвестно.
- Есть ли во внутренних проверках OpenFlow защита от двух писателей в один файл: source-wide grep и динамический race-тест не выполнялись.
- Реальное потребление RAM и CPU у OpenFlow, OpenCode и Bun, а также у Flowise, Dify, Rivet и AutoGen Studio не опубликовано и не измерено.
- Переносимость экспортированных графов OpenFlow между инструментами неизвестна.
- Независимого бенчмарка качества OpenFlow нет, runtime-поведение никто не воспроизводил.
- Устойчивость к 429 вне gauntlet и прочих режимов не проверена, для проверки нужен одобренный provider double.
- Точность метрик принятия по HN: баллы в рендере расходились с исходными данными.
- Актуальные цены и точная семантика crash/idempotency у Langflow, Flowise, Dify и Sim не подтверждены первичными источниками.
- Появится ли официальный harness-адаптер Claude Code или Codex на основе допустимой аутентификации.

## Источник
- DR-ID `DR26-10-28-HUB-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- Независимый reviewer со свежим контекстом
- Durable execution и exactly-once side effects
- Worktree и file lease, один писатель на файл
- Оркестрация моделей против оркестрации harness
- Использование подписок и политика провайдеров
- Shadow-first пилот
- Границы разрешений агента и песочница
- n8n как control plane

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-10-29 через gh: 1 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 1 · 🤷 0.
