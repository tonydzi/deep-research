---
dr_id: DR26-07-28-HUB-08-2338
title: "Andrej Karpathy: биография, публичные тезисы про AI/PKM, GitHub-активность и точки входа д"
date: 2026-07-28
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-08-2338): Andrej Karpathy: биография, публичные тезисы про AI/PKM, GitHub-активность и точки входа для open-source коллаборации

> Кто такой Andrej Karpathy (изначальный запрос был про несуществующего 'Андрея Краватова' — переспрос выявил, что имелся в виду Karpathy), что он публично говорит про AI/agents/Obsidian-PKM, и через какие конкретные GitHub issues можно с ним законтачить.

## Ключевые выводы
- Личность уточнена через переспрос: 'Андрей Краватов'/'Andrey Kravatov' в природе не найден — это, вероятно, ослышанное имя Andrej Karpathy (Словакия→Канада; Toronto BSc → UBC MSc → Stanford PhD под Fei-Fei Li, создатель курса CS231n).
- Карьера: founding researcher OpenAI (2015-17) → Director of AI в Tesla Autopilot (2017-22) → снова OpenAI на midtraining/synthetic data (2023-24) → сооснователь Eureka Labs (июль 2024) → 19 мая 2026 перешёл в Anthropic в pretraining-команду (под Nick Joseph), образование планирует вернуть позже.
- Eureka Labs (ai-native школа, LLC зарегистрирована в Delaware 21.06.2024) фактически на паузе: флагманский курс LLM101n не выпущен и репозиторий архивирован, публичной команды/выручки/инвесторов не раскрыто; есть путаница с другой компанией 'Eureka Labs' (Ethereum block building, основатель Nir Magenheim, ~$6.7-7M) — это НЕ тот же проект.
- Ключевые публичные тезисы Karpathy по годам: 2017 'Software 2.0' (поведение задаётся весами, не кодом) → 2023 'the hottest new programming language is English' → 2025 'vibe coding' (быстрые прототипы без чтения кода моделью) и 'Software 3.0' (естественный язык как исполняемый слой) → 2025 'decade of agents, not year of agents' (Dwarkesh podcast) и концепция 'march of nines' (каждый доп. '9' надёжности стоит как весь предыдущий путь) → 2026 'agentic engineering' (агент делегирует, но не освобождает инженера от review).
- Термин 'silent frame', который искал Антон, в проверенном корпусе Karpathy НЕ обнаружен ни в одном источнике — вероятно, ошибка распознавания/расслышки; ближайший релевантный материал про Obsidian — его 'LLM wiki' идея (гист от 4 апр 2026).
- 'LLM wiki' паттерн: вместо RAG (поиск заново при каждом запросе) — LLM инкрементально строит и поддерживает персистентную markdown-wiki (сырые источники неизменны, вторая зона — LLM-owned связанные страницы с provenance); Obsidian = IDE/оболочка ('obsidian is the ide, llm is the programmer, wiki is the codebase'). Слабость: нет строгого claim-level provenance, риск циклической самоподдерживающейся галлюцинации — это открытая ниша для стороннего вклада (verifier-инструмент).
- GitHub: 63 публичных репозитория, ~163k звёзд суммарно. Активные: nanoChat (56k⭐, обучение GPT-2-уровня модели дешевле $100, активно рефакторится, PR-friendly, требует раскрытия использования LLM в PR), llm.c (30k⭐, C/CUDA, помечает issues 'good first issue'), AutoResearch (агент правит train.py, ограниченная автономность, ~55 open issues, maintainer bandwidth ограничен). llm101n архивирован — PR не имеет смысла; LLM-Council — 'vibe-coded toy', автор явно не будет поддерживать.
- Конкретные issue-кандидаты для PR: nanoChat #590 (NaN loss в SFT при пустых micro-batch), #581 (crash chat_cli при истории > seq_len), #550/554 (ValueError в core_eval.py / нет retry на eval bundle), #427 (память/скорость hellaswag eval). AutoResearch #599 (агент может 'зачитерить' метрику val_bpb без реального улучшения — нужен integrity log), #476 (нет структурированного анализа результатов экспериментов), #215 (нет проверки целостности кэша датасета). llm.c #243 (флаг инициализации весов с нуля), #223 (per-parameter tolerance вместо глобального порога в тестах), #246 (добавить WikiText-103 evaluation).
- Публичные контакты: karpathy.ai, x.com/karpathy, github.com/karpathy, gist.github.com/karpathy, youtube.com/@AndrejKarpathy, karpathy.github.io (старый блог), karpathy.medium.com, karpathy.bearblog.dev, Eureka Labs — eurekalabs.ai / github.com/EurekaLabsAI / x.com/EurekaLabsAI. Публичного email/телефона/LinkedIn для холодного контакта не найдено; лучшие каналы по эффективности — PR/issue-комментарий > gist-комментарий > публичный reply в X > generic DM.
- Рекомендованная стратегия коллаборации: сначала мелкий доказанный PR с воспроизведением бага, минимальными изменениями, regression-тестом и раскрытием использования AI-инструментов (без упоминания токенов/fundraising/'synergy' — читается как pitch); затем отдельный исследовательский форк (например verifiable llm-wiki benchmark), и только потом контакт с прямой ссылкой на воспроизводимый результат.

## Рекомендации / решения
- Начать с одного из низкорисковых issues: nanoChat #590 или AutoResearch #599/#476 — доказать компетентность маленьким PR прежде чем выходить на связь.
- Не писать Karpathy generic предложение 'давайте коллаборировать' — вместо этого принести маленький воспроизводимый результат (исправленный баг/улучшенная метрика) и сослаться на конкретный issue/PR.
- Параллельно можно развивать отдельный проект 'llm-wiki-verifier' (immutable source manifest, claim-level provenance, contradiction graph, git-native audit trail) — это ниша, где сам Karpathy явно не закрыл слабость своего 'LLM wiki' паттерна, и потенциально интересна и Антону для его собственной второй-мозг архитектуры.
- Не отправлять PR/контрибуции в LLM101n (архивирован) и LLM-Council (явно объявлен неподдерживаемым toy-проектом).
- Не путать Eureka Labs Karpathy с одноимённым Ethereum-стартапом Nir Magenheim при любом дальнейшем due diligence.
- Учитывать, что после перехода в Anthropic (май 2026) внешняя пропускная способность Karpathy на collaboration, скорее всего, снизилась — рассчитывать на асимметричную, а не партнёрскую динамику.

## Сущности
- **Люди:** Andrej Karpathy, Fei-Fei Li, Nick Joseph, Nir Magenheim
- **Компании:** OpenAI, Tesla, Anthropic, Eureka Labs (AI education, Karpathy), Eureka Labs (Ethereum block building, Magenheim — другая компания), Stanford University, University of Toronto, University of British Columbia
- **Продукты/инструменты:** nanoGPT, nanoChat, llm.c, llama2.c, micrograd, minbpe, AutoResearch, LLM101n, LLM-Council, CS231n, Obsidian, GitHub, LLM wiki (idea/pattern)

## Открытые вопросы
- Действительно ли исходный запрос Антона имел в виду именно Karpathy, или существует реальный 'Андрей Краватов'/другая персона, говорящая про 'silent frame' и Obsidian — термин так и не подтверждён ни в одном источнике.
- Текущий операционный статус Eureka Labs (продолжает ли существовать как компания, вернётся ли Karpathy к образовательной работе) не подтверждён после его перехода в Anthropic.
- Актуальность указанных issue-номеров (#590, #581, #599 и др.) на момент, когда Антон реально будет делать PR — репозитории активно меняются, нужно перепроверить, не взяты ли уже задачи другим контрибьютором.

## Источник
- DR-ID `DR26-07-28-HUB-08-2338` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-28-HUB-08-2338-issledovanie-andreya-kravatova-chatgpt.md`
- оригинал: `E:\Obsidian\Anton-Knowledge\01-Conversations\ChatGPT\conversations\2026-07-22-issledovanie-andreya-kravatova-6a613ceb.md`

## Связано
- [[issue-matching]]
- [[open-source-contribution-strategy]]
- [[second-brain-architecture]]
- [[llm-wiki-pattern]]
- [[vibe-coding]]
- [[software-3.0]]
- [[multi-vendor-cookbook-radar]]
