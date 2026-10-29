---
dr_id: DR26-07-07-MACB-04
title: "ADOPT framework — доказательное тестирование и внедрение внешней альфы (idea→test→trial→im"
date: 2026-07-07
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-MACB-04): ADOPT framework — доказательное тестирование и внедрение внешней альфы (idea→test→trial→implement→verify)

> Отчёт валидирует и усиливает черновой 6-стадийный ADOPT, картируя его на академические и индустриальные модели внедрения (PDSA, Proctor, COM-B, N-of-1, LLM-evals) и превращая его в шлюзовую систему состояний с обязательными gate-проверками, а не линейный ритуал заметок.

## Ключевые выводы
- ADOPT должен быть gated state machine: INTAKE → MAP → TEST → TRIAL → PROMOTE → VERIFY, с терминальными состояниями DROP / CONFIRMED_EXISTING / SUPERSEDED / RETIRED; DROP возможен после любой стадии (MAP/TEST/TRIAL/VERIFY), VERIFY — явная отдельная стадия, а не часть PROMOTE (иначе получается 'implemented' = тихий мёртвый код)
- Пять инвариантов из проверенных фреймворков: (1) малые локальные тесты до внедрения (PDSA/IHI); (2) успех внедрения ≠ успех результата (Proctor: acceptability, adoption, appropriateness, feasibility, fidelity, cost, penetration, sustainability); (3) механика изменения поведения должна быть явно специфицирована, не подразумеваться (COM-B + BCT taxonomy); (4) соло-тестирование = N-of-1/SCED логика (baseline, intervention, реверсия, угрозы: maturation, confounding, reactivity); (5) для LLM/RAG/агентов promotion требует офлайн-eval датасетов, регрессионных тестов, онлайн-мониторинга и trace-инспекции, а не 'на глаз'
- Единица усыновления — не 'идея', а изменённый механизм: Rule/Skill/Hook/Field/Rubric/Eval case/Dashboard query/Checklist step с явным consumer'ом; если альфу нельзя выразить как изменённый механизм, она остаётся знанием, а не процессом
- Прилипчивость (stickiness) измерима в 3 слоя: adherence = actual_uses/trigger_opportunities (ловит фейковое внедрение — правило есть, но не срабатывает); fidelity = completed_core_steps/required_core_steps; maintenance/automaticity через D+7/D+14/D+30 retention (миф про '21 день' опровергнут — Lally et al.)
- 'Уже внедрено' должно быть первоклассным исходом CONFIRMED_EXISTING (механизм+триггер+consumer+доказательство теста все присутствуют); решающая таблица разводит PARTIAL / DELTA_ADOPT / BLACK_HOLE (consumer есть, но не используется) / NEEDS_VERIFICATION (consumer есть, но нет eval)
- Пороги для соло-оператора должны быть практическими, не псевдо-статистическими: pre-register minimum valuable effect, guardrail limits, минимум opportunities, adherence threshold, drop rule; окна пробации по частоте изменения (daily 7-14д/≥10 срабатываний; weekly 4-6нед/≥6; monthly 2-3мес/≥3; agent/RAG = офлайн-тест + 1-4нед мониторинг; high-risk = ABAB ≥5 наблюдений/фаза)
- Прикладной слой 'идея→тест→пробация→внедрение→verified consumer' для связки человек+LLM+PKM/RAG фрагментирован и незрел: PKM-инструменты (Obsidian Copilot, Readwise, Tana, NotebookLM) только захватывают и извлекают; агентные платформы (Claude Agent Skills) исполняют; eval-инструменты (LangSmith, Ragas, Promptfoo) верифицируют — но никто не оркестрирует весь ADOPT-конвейер
- Иерархия проверки: static checks → unit tests → regression evals → metric checks → LLM-as-judge (только там, где семантику нельзя проверить детерминированно) → human spot check; для соло вопрос не p-value, а 'заплачу ли я сложностью за то, чтобы это оставить?'

## Рекомендации / решения
- Принять ADOPT v1 state model: 0.INTAKE (из HARVEST) → 1.MAP (механизм/поведение/consumer/триггер/outcome + no-op check) → 2.TEST (historical replay/live slice; LLM/RAG=офлайн eval+регрессия; human=baseline/PDSA) → 3.TRIAL (time-boxed, трекать adherence/fidelity/friction/outcome/guardrails) → 4.PROMOTE (реализовать как rule/skill/hook/field/template/eval/checklist + regression test + rollback + именованный consumer) → 5.VERIFY (consumer реально использует; проверки D+7/30/90)
- Разделить CAPTURE (это HARVEST) и ADOPT — ADOPT начинается только когда идея захвачена+оценена+назначена на adoption review
- Сделать no-op check обязательным перед тестированием: same_mechanism_exists? trigger? consumer? evidence? → CONFIRMED_EXISTING / PARTIAL / NEW_ADOPTION
- Три режима оценки: Replay test (агент/промпт/RAG/текст на замороженных исторических задачах), Shadow test (параллельно, но не полагаясь), Live PDSA/N-of-1 (человеческий workflow); для RAG добавить метрики context_precision, context_recall, answer_correctness, citation_support, unsupported_claim_count, latency, token_cost
- Ввести Declined registry как анти-реинфекционную систему — DROP тоже актив обучения (reason, evidence, reconsider_after, ключевые слова для дедупликации)
- Квартальная прополка: показывать promoted-элементы без использования consumer'ом за 30д / без верификации за 90д / с падающей регрессией / поля без запросов / skills без успешных trace / правила, которые никогда не срабатывали → решение KEEP/SIMPLIFY/MERGE/SUPERSEDE/RETIRE
- Enforce 'consumer-required': promotion падает автоматически, если у механизма нет именованного consumer (code gate can_promote())
- Лимитировать WIP: максимум 1-3 одновременных активных trial, чтобы избежать 'слишком много изменений сразу'
- Практический план: Phase1 — построить registry-папки /ADOPT/{00-inbox..99-retired} + adopt-card шаблон + Dataview dashboard; Phase2 — детерминированные promotion-линты; Phase3 — первый eval-датасет из 10 реальных исторических кейсов; Phase4 — прогнать 3 альфа-кандидата (human-workflow, LLM-agent-skill, RAG/note-schema); Phase5 — телеметрия прилипчивости; Phase6 — продвигать агентные изменения как skills с обязательными examples/failure-modes/tests.yaml/named consumer

## Сущности
- **Люди:** Lally
- **Компании:** Spotify, Anthropic, OpenAI, IHI, Toyota
- **Продукты/инструменты:** PDSA / Model for Improvement, Proctor implementation outcomes taxonomy, RE-AIM, Normalization Process Theory, COM-B / Behavior Change Wheel, BCT Taxonomy v1, N-of-1 / SCED, Lean Build-Measure-Learn, Toyota Kata/Kaizen, LangSmith, Ragas, Promptfoo, Anthropic Agent Skills, Anthropic agent evaluation, OpenAI agent evaluation, Obsidian Copilot, RAG Chat plugin, Khoj, AnythingLLM, Readwise, Tana supertags, NotebookLM Deep Research, Architecture Decision Records

## Открытые вопросы
- Не найдено зрелой готовой системы, реализующей полный конвейер 'идея→тест→пробация→внедрение→verified consumer' для связки человек+LLM+PKM/RAG — придётся собирать вручную из фрагментов
- Как именно балансировать rigor vs speed на практике для конкретных типов альфы (низкий/средний/высокий риск) — даны общие принципы, но не готовые пороги под конкретный контекст Антона
- Не проработан вопрос, как LLM-as-judge deployment избежать judge drift на практике сверх общего совета 'заморозить eval set + human spot checks'
- Не указано, как именно ADOPT-registry технически интегрируется с существующим HARVEST-движком Антона (стыковка форматов, где заканчивается HARVEST и начинается ADOPT в реальном pipeline)

## Источник
- DR-ID `DR26-07-07-MACB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- alpha-extraction-engine
- HARVEST
- second-brain-northstar
- vault-data-architecture
- capture-rules-into-bible
- evaluate-recurring-into-routine
- ak47-simplicity
