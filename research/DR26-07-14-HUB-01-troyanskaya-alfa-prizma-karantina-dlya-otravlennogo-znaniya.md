---
dr_id: DR26-07-14-HUB-01
title: "Троянская альфа — призма карантина для отравленного знания"
date: 2026-07-14
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Decision Memo (DR26-07-14-HUB-01): Троянская альфа

> Вопрос: как автономный агент вычищает намайненные идеи из непроверенных источников, прежде чем их внедрить, чтобы «хорошая идея» не оказалась самонаведённым бэкдором.

## Проблема
Наш альфа-майнер тянет идеи из внешнего (доки, чужие каналы, веб-скрейп) в бэклог, который мы потом внедряем. Инъекция на уровне ЗАДАЧ/сообщений у нас закрыта (DR26-06-25-HUB-01). Но на уровне ЗНАНИЙ — нет: отравленная «оптимизация» может сама быть атакой.

## Что DR подтвердил (моя гипотеза оказалась верной по направлению)
- **Две оси — провенанс × blast-radius** = верно. DR формализует как «effect-surface classification» + «immutable provenance capture» (2 из 7 стадий призмы).
- **Матрица маршрутизации** (auto-clear только read-only+обратимое+низкий blast+подтверждённое) = верно, дословно «effect-based review policy».
- ⭐ **Hard-hold на идею, ослабляющую контроль безопасности** = подтверждён и обобщён: алертить на «recommendation that weakens logging/auth/sandboxing»; правильный вопрос не «хотел ли источник навредить», а «меняет ли идея control-relevant поведение / ранг авторитета / будущую память».
- **Tamper-evidence** (связать идею с провенансом неизменяемо) = подтверждён и усилен → **Idea Attestation**.

## Что DR добавил сверх моей гипотезы (важное)
1. **Провенанс ≠ одобрение.** «Attested» никогда не равно «approved». Даже доверенный официальный канал шлёт яд (Amazon Q VS Code extension, PyPI Ultralytics). → провенанс СНИЖАЕТ стоимость ревью, а не заменяет его. Моя матрица слишком легко авто-клирила доверенный источник — поправка.
2. **Одного отравленного документа достаточно** (PoisonedRAG: 5 текстов на млн; «single-doc» атаки). → нужна ось **корроборации**: для high-risk идей ≥2 независимых источника (или 1 первичный + 1 вторичный), которой у меня не было.
3. **Carrier sanitation ДО извлечения** + хранить И видимый текст, И сырой machine-visible (ловить hidden-text/Unicode/URL-параметры-стиринг). Слой ингеста, который я не прописал.
4. **Общая память/знание — read-only по умолчанию.** Писать в общие политики/«одобренные best practices» может только карантин-сервис или подписанный human-review тул, НЕ обычные агенты. ⚠️ Для нас острое: волт синкается на весь флот и у нас культура any-llm-vault-actor «любой LLM пишет в волт» — сюда нужен барьер на shared policy/knowledge-файлы.
5. **Idea Attestation = та же крипта, что конверты задач сессии A** — in-toto/SLSA-форма, DSSE, **Ed25519**, append-only лог (Git/SQLite + Syncthing). ⭐ Красивое схождение: ОДИН крипто-фундамент (Ed25519 подписанные конверты + append-only), два применения — авторизация задач (сессия A) и провенанс идей (сессия B).

## Варианты (из DR)
- **Prism Lite** — санитайз ингеста + видимый/скрытый текст + хеш идеи + захват источника + простой риск-лейбл + человек на всё, что трогает код/установки/секреты/сеть/память/safety. Быстро, one-person-maintainable. Минус: слабый дедуп/трассировка.
- **Prism Standard** (рекомендация DR) — + подписанный Idea Attestation, append-only хранилище, пороги корроборации, авто-политика по effect-surface, read-only shared memory, карантин-реестр посылок. Лучше для форензики/отката. Минус: формализация метаданных = оверхед.
- **Prism Maximal** — + transparency-log, selective-disclosure, adversarial-eval, hybrid-retrieval. Frontier, не для one-person.

## Рекомендация (моя, с поправкой на AK-47)
**Staged: строим Prism Lite СЕЙЧАС, растём в Standard по мере роста реального implement-rate альфы.** DR по умолчанию советует Standard, НО его же условие — «Standard оправдан, когда throughput бэклога высок настолько, что провенанс-долг копится быстрее, чем человек аудитит». У нас альфа-бэклог большой, но реальный implement-rate низкий (идеи лежат) → сразу Standard = ⚠️ УСЛОЖНЕНИЕ ради объёма, которого пока нет. Ставим границу и hard-эвристику сейчас (Lite), Idea Attestation (Standard) — когда поток внедрений вырастет. Уверенность: [established] на контролях, [emerging] на точных порогах. Главное допущение: implement-rate альфы пока низкий (проверяемо по бэклогу).

## Action Plan (что строит сессия B)
1. ⭐ **Граница: намайненная идея → карантин `raw`, НИКОГДА не `ready` напрямую.** Самый ценный шаг — создаёт границу, которую задач-слой уже предполагает. С днём 1 крепить URL+время+хеш+версия-экстрактора+точные evidence-спаны.
2. **Carrier sanitation до извлечения** — хранить видимый + сырой текст; флажить hidden-text, подозрительные URL-параметры, non-printing Unicode, encoded-блобы.
3. **Effect-based auto-clear** — авто-клир ТОЛЬКО если: read-only + обратимо + не меняет исполнение/память + не добавляет зависимостей + не трогает auth/сеть/удаление/логи/safety + подтверждено ≥2 независимыми (или 1 первичный+1 вторичный). Иначе → человек.
4. ⭐ **Hard-hold без DR:** любая идея, предлагающая ослабить контроль безопасности → стоп+алерт независимо от источника (сигнатура троянской альфы).
5. **Read-only shared knowledge** — в общие policy/approved-файлы пишет только карантин-сервис/подписанный тул, не обычные агенты.
6. **Idea Attestation schema** (Standard, отложенный) — signed JSON (idea_id, canonical_claim, supporting_spans, source_records, retrieval_context, normalization_steps, risk_surfaces, corroboration, review_decision, downstream_links), Ed25519, append-only. Мапится на in-toto/SLSA.
7. **Алерт на переходы, не на предложения** (анти-fatigue): low-provenance на high-impact; hidden-text/URL-стиринг под будущую память; ослабление safety; конфликт с политикой; невозможность корроборации; хеш провенанса разошёлся с источником.

## Открытые вопросы
- Точные пороги корроборации/риска — поля ещё не устоялись (нет бенчмарка «safe idea adoption» для автономных флотов).
- C2PA-провенанс сам по себе слаб (revocation/validator-расхождения) — не полагаться на «attested = truth».
- Наш реальный implement-rate альфы — измерить, чтобы решить Lite→Standard триггер.

## Источник
- DR-ID `DR26-07-14-HUB-01` · реестр _DR-Registry · оригинал «внутренний путь лаборатории»
- Ключевые: Anthropic prompt-injection-defenses (2025-11-24), NCSC «prompt injection is not SQL injection» (2025-12-08), PoisonedRAG (USENIX 2025), Microsoft AI Recommendation Poisoning (2026-02-10), SLSA/in-toto/Sigstore/PyPI attestations, NIST IR 8596.

## Связано
- machine-bus-telegram-rail
- apply-deliverables-immediately
- remote-approval-qqq
- alert-ownership-routing
- any-llm-vault-actor
- credential-store
- verify-existing-before-proposing
