---
dr_id: DR26-08-11-HUB-02-2212
title: "Асимметрия слияния PR: почему mid-size инфра-OSS мержит аутсайдеров, а вендорские cookbook"
date: 2026-08-11
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-11-HUB-02-2212): Асимметрия слияния PR: почему mid-size инфра-OSS мержит аутсайдеров, а вендорские cookbook-репо — нет

> Отчёт проверяет гипотезу лаборатории (68 PR за 4 недели), что вероятность мержа PR неизвестного контрибьютора определяется КЛАССОМ репозитория (продуктовый SDK/библиотека vs. маркетинговый cookbook/curated-list вендора), а не качеством патча.

## Ключевые выводы
- Для проверки гипотезы 'repo class vs contributor track record' отчёт опирается на датасет AIDev (456K+ PR от агентов), выводы MSR 2026 и исследование NPM-экосистемы.
- Экономика вендорских cookbook-репо (OpenAI, Anthropic, Mistral, xAI, Hugging Face) разобрана отдельно: кто владеет репо внутри компании и почему мерж чужого PR несёт редакционные/брендовые издержки без инженерной выгоды для мейнтейнера.
- Проанализированы паттерны мержа по каждому из проблемных репо лаборатории (claude-cookbooks, openai-cookbook, adk-python-community, xai-cookbook, mistral cookbook, huggingface cookbook) — какие внешние PR всё же мержились и что их объединяло.
- Рассмотрен вопрос переноса репутации: конвертируется ли merged-статус в 4 инфра-проектах (go-sdk, fastmcp, logfire, basic-memory) в заметность/доверие у флагманских вендоров.
- Ландшафт политик раскрытия AI-авторства картирован по проектам curl, Zig, Gentoo, QEMU, Linux kernel — часть сообществ имеет явные политики/запреты.
- Разобрана механика самих PR: латентность ответа, force-push vs инкрементальные коммиты как факторы, влияющие на исход.
- Предложен поэтапный план перераспределения усилий: уход от редакционных поверхностей (cookbooks, awesome-lists) в сторону MCP/agent-infra репозиториев, находящихся в одном шаге зависимости от уже 4 выигранных проектов.

## Рекомендации / решения
- Пересобрать приоритеты скилла local-github-growth: выбирать репо по КЛАССУ — продуктовые SDK/фреймворки/библиотеки, где внешний патч чинит то, что мейнтейнер лично поддерживает и поставляет — а не вендорские cookbook/demo/curated-list репозитории.
- Таргетировать MCP/agent-infra репозитории в пределах одного dependency hop от уже смерженных проектов (go-sdk, fastmcp, logfire, basic-memory), используя эффект локальной для экосистемы репутации.
- Учитывать политику раскрытия AI-авторства конкретного репо/сообщества (curl, Zig, Gentoo, QEMU, Linux kernel имеют явные правила) перед выбором формата контрибуции.
- По 17 открытым PR с мячом на нашей стороне — применить выводы отчёта по механике ответа (латентность, force-push vs инкрементальные коммиты) при принятии решения на каждую позицию.

## Сущности
- **Люди:** —
- **Компании:** OpenAI, Anthropic, Mistral AI, xAI, Hugging Face, Microsoft, Google
- **Продукты/инструменты:** MCP (Model Context Protocol), modelcontextprotocol/go-sdk, fastmcp, pydantic/logfire, basic-memory, AIDev dataset, anthropics/claude-cookbooks, openai/openai-cookbook, google/adk-python-community, xai-org/xai-cookbook, mistralai/cookbook, huggingface/cookbook, curl, Zig, Gentoo, QEMU, Linux kernel

## Открытые вопросы
- Итоговый вердикт по гипотезе 'repo class' (подтверждена/опровергнута/уточнена) не приведён в доступном тексте отчёта — только заявлено, что она 'адъюдицируется' против альтернатив.
- Конкретный ответ на вопрос о переносе репутации (конвертируется ли merged-статус в инфра-проектах в заметность у вендоров) и механизм переноса не раскрыты в предоставленном фрагменте.
- Ранжированный список конкретных репозиториев-целей (вопрос 6 брифа) не приведён в доступном тексте — только общее направление 'MCP/agent-infra в одном hop от текущих побед'.
- Не видно, как именно распределены рекомендации по каждой из 17 открытых позиций — упомянут только общий план по механике ответа.

## Источник
- DR-ID `DR26-08-11-HUB-02-2212` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- local-github-growth skill
- open-source contributor reputation
- vendor DevRel incentives
- PR merge rate studies
- AI-disclosure policies in OSS
- MCP ecosystem
- repo classification (product vs marketing surface)
