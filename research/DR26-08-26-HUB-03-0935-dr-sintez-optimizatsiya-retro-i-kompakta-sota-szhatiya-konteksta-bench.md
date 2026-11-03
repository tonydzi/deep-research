---
dr_id: DR26-08-26-HUB-03-0935
title: "ДР-синтез: оптимизация ретро и компакта (SOTA сжатия контекста, бенчмарки, апгрейды)"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# ДР-синтез 0935: оптимизация ретро и компакта

Заказ: сессия 65c9f647 26.08 (публикация compact-canon + claw-retro) заказала ДР «что делает мир за пределами GitHub-хоббистов и что нам улучшить». Синтез по частичному сбору 2/6, недобранные рельсы остаются в missing; добор возможен после ребута хаба, но решения ниже он скорее уточнит, чем перевернёт (оба пришедших отчёта сошлись в главном независимо от третьего голоса).

## 1. Карта SOTA (что делает мир)

**Рамка (Grok, established):** три несмешиваемых слоя: L1 = компакция текста промпта (наш /compact и paste-блок), L2 = внешняя память + retrieval (MemGPT/Letta blocks, LangMem, Devin Knowledge, Cursor Memories), L3 = KV-cache компрессия (нам недоступна, нет API). ChatGPT добавляет четвёртый род: ACE-подобная правка переиспользуемых инструкций (наша Библия/CLAUDE.md = ровно это). Мерить их одной линейкой нельзя.

**Продакшн-дефолт везде один (established):** «хвост verbatim + суммаризируй старое» (Aider рекурсивно, LangMem SummarizationNode, Letta, OpenAI opaque compaction item). При этом **полевые замеры «какие поля выжили» не публикует НИКТО** — ни вендоры, ни фреймворки. Наши 354/354 + 7/7 headers + 15/15 фактов + 54358→2522 на момент отчётов уникальны в нише. Это подтверждает козырь №1 из insight-2026-08-26-compact-retro-landscape-github уже не по 16 соседям, а по всему полю.

**Отдельные точки:**
- MemGPT: retrieval по иерархии памяти поднял deep-memory retrieval GPT-4 с 32.1% до 92.5% (established, arXiv:2310.08560) — но это retrieval, не выживание фактов при суммаризации.
- Letta: боевые провалы компакции в issues #3242/#3270/#3279 (summary-of-summary деградация, избыточные триггеры, падение самого суммаризатора). Их memory blocks = именованные verbatim-блоки с жёстким лимитом символов — ближайший родственник нашего paste-блока.
- OpenAI: user-сообщения verbatim, остальное в opaque encrypted item — продуктовое преимущество и evaluation-дыра (проверить выживание нельзя). SentinelOne намерил на их компакции −86% input без падения агрегатного скора (один workload; агрегат прячет потерю редких констрейнтов).
- Factory: главная стратегия = меньше компактить (deferred-context engine: −15.1% input в среднем, −39.4% на p90) — context avoidance, не retention.
- ACE (ICLR): контекст как эволюционирующий playbook с инкрементальными патчами дискретных пунктов; называет риски «brevity bias» и «context collapse». Наш ритуал переизлучения блока каждым /rr — та же семья.
- Свежий препринт (SSRN 6976218): рекурсивная суммаризация удерживает ~41.6% фактов vs 40.6% full-context при −34.8% токенов — аномально низкие абсолюты, требует репликации (emerging).

## 2. Бенчмарки: ниша пуста, и это наш ход (Mission-2)

Оба вендора независимо пришли к одному: **NIAH/RULER/LongBench меряют ДОСТУП к информации, которая ещё в промпте; бенчмарка «сохранил ли кодек компакции состояние сессии» не существует.** («Сошлись» тут не самодоказательство — оба выводили из одной и той же литературы, — но наша собственная утренняя карта 16 соседей дала то же: замеров нет ни у кого.)

Что адоптируемо в наш будущий бенч: типология вопросов LongMemEval (updates, temporal, abstention), атомарные факты FactScore как единица, snapshot/reload-дизайн BFCL V4 Memory, low-lexical-overlap принцип NoLiMa. Что НЕ заявлять: LongMemEval/LoCoMo как «наш бенч» (другая проблема). Grok дал имя: **SCFS — Session Compaction Fact Survival** (фикстуры-транскрипты + манифесты посаженных фактов + кодеки + пробы + кривая поколений). Теоретическое обоснование пробела — rate-distortion analysis arXiv:2607.08032, прямая цитата для мотивации.

→ Карточка: task-2026-08-26-scfs-multi-cycle-benchmark (P1). Она же — тест-дверь для нашей же декларации 26.08 «многоцикловость закрыта архитектурой переизлучения» (alpha-backlog, строка 26.08в): если кривая 1/3/5 циклов падает, декларация опровергнута цифрой и кумулятивность возвращается на стол с evidence.

## 3. Retro/self-improvement петли (Q3)

Промышленная практика тоньше нашей: Anthropic публикует guidance «durable-правила в файлах, транскрипту не доверять», но door-audit и usage-счётчиков НЕ публикует — **claw-retro строже их публичного гайданса** (Grok, established/emerging). Cursor/Windsurf Memories = крошечные списки с UI-удалением (guardrail социальный); Letta = самомодифицирующиеся блоки с потолком символов; Devin Knowledge = wiki с human-in-the-loop и известными жалобами на протухание. Наша связка rule_liveness (5 статусов) + outcome-аудит Step 3🔄 + счётчики §5.8 на этом фоне выглядит передовой — это контент-аргумент, не только утешение.

## 4. Шорт-лист апгрейдов и вердикты скептиков

7 кандидатов после дедупа, **0 adopt-now** (все скептики отработали в пользу «не трогай живой канон без замера»):

| Кандидат | Вердикт | Суть решения |
|---|---|---|
| verbatim-pin-spans (+floors) | 🟡 замер-карточка | наполовину дубль bare-«ДОСЛОВНО» в скелете; слит с паркованными DO-NOT-TOUCH/confidence → A/B-замер |
| post-compact-probe | 🟡 замер-карточка | не дубль, не противоречит #14160; но исполнение директивы «проверь себя после сжатия» не замерено → канарейка |
| multi-cycle-delta-lineage | 🟡 → в бенч | gen:N/delta противоречит декларации «переизлучение закрывает многоцикловость» → сперва кривая SCFS, потом разговор |
| multi-cycle-benchmark (SCFS) | 🟢 карточка P1 | ниша пуста, методика 354/354 уже наша, уникальный датасет = Mission-2 |
| compact-block-validator | ⛔ reject | счётчики из retro_inventory = фальшивая детерминированность (mtime чужих сессий); класс в Breakage-Journal, линт строить после 3-го случая |
| rule-lifecycle-quarantine | ⛔ reject | дубль: rule_liveness уже машина состояний с классовыми часами; авто-expiry, от которого он защищает, у нас не существует |
| promotion-record-metadata | ⛔ reject | на ~70% дубль россыпи (Breakage-Journal, home-matrix, door-audit, liveness); проваливает собственный гейт «у каждого поля живой потребитель» |

Депризовано обоими вендорами (не делать): LLM-as-judge «хорош ли компакт», заголовки сверх 7, graph memory, KV-cache-статьи, авто-expiry правил.

## 5. Раздача: вторая волна площадок (кандидаты, живость проверять перед стуком)

ChatGPT принёс конкретные живые issues (номера НЕ верифицированы инструментом — перед стуком /issue-match): anthropics/claude-code **#23776** (компакция потеряла запрос юзера — ровно наша тема), **#21788** (запрос видимых саммари), **#42817** (контроль авто-компакции, not planned → внешние замеры ценны), **#85075** (протухшая auto-memory — наша provenance-тема), openai/codex **#25900/#21468/#38269** (структурные чекпоинты, видимая компакция, дроп контекста) и letta-ai/letta #3242+#3270+#3279. Grok (без инструментов, номера сознательно не выдумывал): 12-factor-agents (factor «own your context window»), Show HN с 354/354 в первом абзаце, Simon Willison, Latent Space, Letta/LangChain/Aider Discord, Hamel Husain / Eugene Yan для design review SCFS. Публичная позиция одной строкой (Grok): «/compact is a lossy codec; bare ignored instructions 354/354; inline 7-header kept 15/15 at 54,358→2,522; we still do not know gen-5». Внесено в task-2026-08-26-distribute-compact-canon-claw-retro как волна 2.

## 6. Что решено (Decision Memo, confidence)

1. Живой канон /retro и /cc НЕ трогаем — ни один кандидат не пережил скептиков как adopt-now (confidence high, 7/7 вердиктов единогласно по правилам АК-47/shadow-first).
2. Два замера заведены карточками: task-2026-08-26-compact-canon-v11-pin-spans-probe (pin-spans A/B + probe-канарейка) и task-2026-08-26-scfs-multi-cycle-benchmark (кривая 1/3/5 + continuation-score). Урожай обоих кормит волну каталогов 09.09 (v1.1 к подаче).
3. Три отказа записаны выше — не пере-предлагать без нового evidence.
4. Декларация «кумулятивность не нужна» (26.08в) стоит, но получила тест-дверь через SCFS.
5. Missing-рельсы (gemini/glm/claudeai/mistral) добираются после ребута хаба; добор折 в тот же синтез апдейтом, отдельный ДР не заказывать.

## Связки
insight-2026-08-26-compact-retro-landscape-github · task-2026-08-26-distribute-compact-canon-claw-retro · task-2026-08-26-compact-canon-v11-pin-spans-probe · task-2026-08-26-scfs-multi-cycle-benchmark · compact-anton-prefers-zero-touch · compact-format-delivery-gotcha · repos: github.com/tonydzi/compact-canon · github.com/tonydzi/claw-retro
