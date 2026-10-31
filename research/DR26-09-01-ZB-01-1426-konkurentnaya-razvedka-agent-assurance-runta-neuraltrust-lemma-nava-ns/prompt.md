# DR26-09-01-ZB-01-1426 — the exact prompt sent to every model

# Конкурентная разведка: agent-assurance / financial agent verification — Runta, NeuralTrust, Lemma, Nava и вся карта

Контекст. Я рассматриваю запуск продукта «Financial Agent Flight Recorder» — чёрный ящик для денежных действий AI-агентов: before/after state, policy proof, replay, spend/risk limits, независимое одобрение, криптографически проверяемое доказательство «кто санкционировал транзакцию». Первые покупатели: stablecoin processors, trading-as-a-service, treasury teams, custodians, страховщики/аудиторы agentic transactions. Прежде чем вкладывать 45 дней в customer discovery, нужна честная карта конкурентов: не занята ли ниша уже.

Задача — глубокое расследование по каждому пункту, с проверяемыми URL на каждое утверждение. Не пересказывай пресс-релизы — ищи первичку: сайты компаний, доки, GitHub, LinkedIn команды, Crunchbase/PitchBook-данные, посты фаундеров, отзывы клиентов, job postings (они выдают roadmap).

## Часть 1. Досье на четыре компании (по каждой — одинаковая структура)

Компании: **Runta** (seed $20M, a16z, agent execution/security), **NeuralTrust** ($20M, горизонтальный agent security), **Lemma** (pre-seed $2.3M, agent observability, заявляют 1M traces/day), **Nava** (seed $8.3M, escrow + verification до исполнения).

По каждой:
1. Что именно продукт ДЕЛАЕТ сегодня (не из питча, а из доков/демо): какие действия агентов покрывает, есть ли финансовые/денежные действия, есть ли replay, policy proof, независимое одобрение, audit trail.
2. Команда: фаундеры, бэкграунд, инженерное ядро, размер.
3. Деньги: раунды, инвесторы, дата, оценка если известна.
4. Клиенты/трекшн: названные клиенты, design partners, integrations, pricing если публичен.
5. Позиционирование и ICP: кому продают (dev-инструмент? enterprise security? финансы?).
6. ЧЕГО НЕ ДЕЛАЮТ: где границы продукта; покрывают ли regulated finance (custody, treasury, stablecoin payments), есть ли «доказательство санкции денежного действия».
7. Вердикт: прямой конкурент Flight Recorder / смежный / не конкурент — почему.

## Часть 2. Полная карта категории agent-assurance/verification

8. Кто ЕЩЁ в категории (One, Beacon, guardrails/observability игроки, agentic payments security): таблица компания · раунд · фокус · пересечение с финансовым flight recorder.
9. Какая часть ниши НЕ закрыта: есть ли кто-то, кто делает именно financial-grade audit trail + authorization proof для денежных действий агентов? Если да — кто и насколько зрел. Если нет — почему (нет спроса? рано? трудно?).
10. Сигналы спроса: regulatory drivers (EU AI Act, US guidance, MiCA, банковские требования к agentic payments), инциденты с денежными действиями агентов 2025-2026, что пишут risk/compliance-офицеры.

## Часть 3. NSF SBIR Phase I — подробный разбор

11. Что это: суммы (до $305K?), сроки, процесс (Project Pitch → full proposal), success rate, ближайшие дедлайны (называлось 04.11.2026 — проверить).
12. ⚠️ КРИТИЧНО — eligibility: требования к ownership компании (majority US citizens / permanent residents?). Проходит ли фаундер на визе O-1 без грин-карты? Какие обходные конфигурации легальны (co-founder-гражданин с >50%? PI-требования — может ли O-1 быть PI?). Дай точные цитаты правил с nsf.gov/seedfund.
13. Альтернативы non-dilutive для agent-verification R&D, доступные фаундеру-не-резиденту США: EF, NLnet, гранты фондов, ecosystem grants (Coinbase, Circle, Stripe), европейские программы без консорциума.

## Часть 4. Стейблкоин-коридоры (справочно, ниша снята с рассмотрения)

14. Коротко (полстраницы): карта финансирования B2B stablecoin infra за 2026 (Bridge/Stripe, BVNK, Rain, Conduit, региональные) — только чтобы подтвердить/опровергнуть «рынок занят, без лицензии входа нет».

## Формат ответа
- Каждое число и утверждение — с URL источника.
- Отдельный блок «Что опровергает запуск Flight Recorder» и «Что поддерживает запуск» — по 5 пунктов с уликами.
- Финал: вердикт «ниша занята / есть окно, вот оно / данных мало» + confidence.
- Объём: не менее 5000 слов, только по делу.
