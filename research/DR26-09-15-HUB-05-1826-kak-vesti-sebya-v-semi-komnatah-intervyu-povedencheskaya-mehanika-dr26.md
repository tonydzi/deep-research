---
dr_id: DR26-09-15-HUB-05-1826
title: "Как ВЕСТИ СЕБЯ в семи комнатах интервью: поведенческая механика (DR26-09-15-HUB-05-1826)"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Поведение в семи комнатах: что доказано, что продано, что делать

Дополняет insight-DR-DR26-09-15-HUB-04-1610-interview-loops-ai-labs-faang (структура лупов). Тот отвечал «как устроена комната», этот — «как себя в ней вести».

## 0. Честность корпуса

**Собрано 3 ноги из 6.** Это **пол** ДР (`min_rails=3`, три разные LLM), а НЕ кворум 4/6. Три ноги прошли `dr_leg_gate --strict --prompt`: chatgpt (118 источников), grok (37), claudeai-a2 (30). Отбиты: claudeai-bb (доехал хвост диалога вместо отчёта), gemini (0 байт), glm (не дошла). Недобранное добирает суточный `dr_latecomer_sweep.py` — если приедет нога, меняющая вывод, синтез правится.

Все три ноги независимо предупредили об одном: **ниша залита продавцами подготовки**. claudeai-a2 отдельно поймал прямой конфликт интересов — `interviewcoder.co`, всплывший в выдаче про правила прокторинга, оказался вендором инструмента для ОБХОДА прокторинга. Ниже каждое утверждение несёт тир.

---

## 1. Три мифа, которые корпус убил

### Миф 1. «Решение принимают за первые 4 минуты»

**[established]** Полевое исследование Frieder et al. 2015 (166 интервьюеров, 691 кандидат): решение за 1 минуту принимают **4.9%**; за 5 минут — около 30%; **69.9% решают после 5+ минут**, из них 17.7% после 15-й минуты и 22.5% уже после конца интервью. Фольклор «4 минуты» тянется из диссертации 1954 года.

**Но якорь реален.** Barrick, Swider & Stewart 2010 (JAP): впечатление, сложившееся в rapport-фазе ДО структурированных вопросов, предсказывает итоговый рейтинг **r=.42** и реальный оффер **r=.22**. Swider 2016: эффект сильнее окрашивает РАННИЕ ответы, слабее — поздние.

⚠️ Ноги разошлись в цифрах первого впечатления: chatgpt цитирует мета-анализ 2026 (204 выборки, 145 исследований) с ρ=.48, но это разные рабочие ситуации, не только интервью. claudeai-a2 тянет thin-slicing (8–12-секундный немой клип) как [established] — это лабораторный артефакт, и он же сам приводит контр-данные Frieder. **Рабочий вывод, который держат все три:** первая минута не выносит вердикт, но задаёт рамку, через которую читают всё остальное. Практика одинакова при любой версии.

**[single-source; R]** Опытные и уверенные интервьюеры решают БЫСТРЕЕ — авторы называют это проблемой, не достоинством.

### Миф 2. «Кандидат должен говорить 70% / 80% / 40% времени»

**[established: отрицательный результат]** Валидированного соотношения речи для успешных интервью НЕ СУЩЕСТВУЕТ. Все три ноги пришли к этому независимо.

- Правила 70/30, 80/20, 40/60 — фольклор coaching-блогов без ссылки на данные.
- Вендор Metaview по ~2700 своим транскриптам даёт медиану 53.2% речи кандидата и медианный самый длинный монолог 2.1 минуты — но **не связывает это с оффером**. Класс (V), заинтересованный источник.
- **[single-source; R; 1960]** В 115 реальных армейских интервью время речи кандидата НЕ различало принятых и отклонённых; в успешных больше говорил интервьюер. Старо и доменно-специфично, но прямо опровергает «захвати эфир».
- Gong 43:57 — это ПРОДАЖИ, не найм. Не путать.

**Что вместо цифры.** Стоп-сигнал смысловой, а не секундный: прямой ответ дан, одно доказательство приведено, релевантность понятна → пауза. Практический маркер: **идёшь дольше ~90 секунд без единой реакции интервьюера** (кивок, «угу», встречный вопрос) → остановись и спроси, углубляться ли.

### Миф 3. «Power pose перед звонком»

**[established fail-to-replicate]** Carney/Cuddy/Yap 2010 (гормоны + склонность к риску) не воспроизвелось: Ranehill et al. 2015, N=200 — никакого эффекта на тестостерон, кортизол, риск; только самоотчёт «чувствую себя сильнее». RCT 2023 на структурированном устном экзамене — тоже ноль. **Соавтор Dana Carney публично отказалась от собственных выводов в 2016 году.** Совет «встань в позу супергероя, это снизит кортизол» = цитирование опровергнутой науки, хотя в 2026-м он всё ещё массово встречается.

---

## 2. Контрмиф: язык тела НЕ полностью фольклор

Это шло НАПРОТИВ ожидания заказа, и две независимые ноги принесли одно и то же.

**[established]** Martín-Raugh et al. 2023 (Journal of Organizational Behavior), мета-анализ 63 исследований, N=4868. Три невербальных сигнала реально предсказывают рейтинг интервью:

| сигнал | ρ |
|---|---|
| профессиональный внешний вид | **.62** |
| зрительный контакт | **.45** |
| движения головы / кивки | **.43** |

Критично: **структура интервью, модальность и длительность почти НЕ ослабляют эти связи.** Структурированный формат от невербалики не защищает.

**Что из этого НЕ следует.** Работают именно эти три, а не жесты руками, не «открытая поза», не поза власти. Прямого RCT «руки в кадре против не в кадре» для tech-интервью не нашла ни одна нога. ρ=.62 за appearance включает одежду и общий вид — это не «купи кольцевую лампу».

**[single-source; эксперимент]** Mastrella et al. 2023: актёры читают ОДИНАКОВЫЙ текст. Тревожная невербалика (меньше глаз, самоприкосновения, ёрзание) → рейтинг ~3.5/5 против ~4/5. Значит часть «слабого кандидата при одинаковом содержании» — это тревожный канал тела, а не содержание.

---

## 3. Главная находка про coding: формат сам режет результат

**[established внутри SE-интервью]** Behroozi et al. 2020: сам факт, что за кандидатом наблюдают, режет correctness **более чем вдвое** (медианный score упал больше чем в два раза, проваливается почти вдвое больше людей). Участники: «very nervous, rushed, stressed, monitored, unable to concentrate». Источник нагрузки назван прямо — «думать, говорить и писать код одновременно».

**[established; Distinguished Paper FSE 2022]** Behroozi/Parnin/Brown: supervised think-aloud СНИЖАЕТ информативность речи и поднимает стресс. **86% одноминутных отрезков, размеченных как стрессовые, пришли с whiteboard-формата (с наблюдателем); 0% — со screencast без наблюдателя.** Цитата кандидата из работы: «Thinking out loud ends up with me spending brain cycles on reflecting on how what I say must be registering with the interviewer».

**Вывод, который держат все три ноги:** «сообщать решения» и «озвучивать каждую возникающую мысль» — РАЗНЫЕ вещи, и вторая вредит.

**Рабочая форма think-aloud:** `наблюдение → две альтернативы → выбор и причина → следующий тест`.

> «I see two approaches. The hash-map version is linear but uses O(n) memory; sorting costs O(n log n) but bounds memory differently. Given this constraint, I'll use the map and first test duplicate inputs.»

**Объявленная пауза** — единственное лекарство, которое предлагают сами авторы papers:

> «Give me ten seconds to compare the two failure modes.»

Молчать 5–15 секунд, предупредив, — норма. Молчать 40 секунд, ничего не печатая и не объявив, — губительно: интервьюер физически не может отличить мысль от застревания.

---

## 4. Семь комнат: первые 60 · темп · паузы · давление · уточнения · «не знаю» · последние 60

### Комната 1. Recruiter screen (~30 мин)

**Первые 60.** Слышно? → 12–30 секунд «кто я сейчас» → **стоп, отдать ход**. Meta формально даёт 5 минут на introductions — это СЛОТ, а не мандат на монолог.

**[в; interviewing.io, инженеры Anthropic 2026]** «Technically strong candidates fail here because they haven't done the reading and can't speak to why they want to work at Anthropic **specifically**». Не «почему AI» — почему ЭТА компания.

**Темп.** Факт-вопросы (локация, notice, стек) — 30–90 секунд. «Tell me about yourself» — 90–150. Сигнал «затянул»: рекрутёр пытается вставить следующее поле формы, а ты не останавливаешься.

**Паузы.** Think-aloud тут почти не нужен. 2–4 секунды перед «why this company» читаются как мысль. Поток про архитектуру флота — промах комнаты.

**Давление.** Рекрутёр редко спорит; давление = «какой диапазон», «есть другие офферы», «стартуешь через 2 недели». **Anthropic явно: не называй salary expectations и competing process первым.**

> «I'm glad to discuss compensation when there's mutual fit; right now I want to understand the role.»

**Уточнения.** 2–4 штуки **в конце**, не в начале. Сильные: «как выглядит успех в первые 90 дней», «роль привязана к команде или team matching после». Слабое: 8 вопросов про бенефиты до закрытия fit.

**[b; Amazon Bar Raisers]** «Ask the questions that you actually want to know the answers to»; интервью в лучшем виде — «conversation with a curious friend».

**«Не знаю».** Про оргструктуру: «I haven't seen that team's charter yet — can you describe how this role sits relative to research vs product?»

**Последние 60.** Один конкретный вопрос + подтверждение следующего шага («you'll send the coding link by Thursday?»). **Не питч.**

---

### Комната 2. Project deep dive / HM (45–60 мин)

**Первые 60.** 20 секунд человеческого обмена, потом карта:

> «The problem was X. I personally owned Y. The measured result was Z. The hardest trade-off was A versus B. I can start with architecture, the failure that changed the design, or rollout — where would you like depth?»

**[в]** Про Anthropic HM: «Have a strong project ready to walk through in depth. The HM call varies a lot by role, but this is the one constant». Слабый — четырёхминутный тур по CV.

**Темп.** Блок 45–90 секунд: `решение → почему → отвергнутая альтернатива → результат` → пауза.

**[C] Stripe** повышает оценку за high-level point + конкретное доказательство + личный impact; **снижает** за rambling, общие слова о команде и **игнорирование попытки интервьюера сменить тему**.

**[в; экс-интервьюер Amazon Brian Kane]** «Don't use the words "we" or "the team" outside of the situation (or sometimes results)».

Признак избыточной длины: пошёл второй пример, хотя первый уже доказал тезис.

**Паузы.** 3–5 секунд на «what would you do differently» — полезно. 15 секунд молчания после «walk me through the architecture» без «let me sketch the pieces» — губительно.

> «I want to separate what we measured from what I inferred. Give me a moment.»

**Давление.** HM перебивает, чтобы добыть ownership. **[C/V; Ashby]** остановки и drill-down — ожидаемая часть формата, а не потеря контроля.

- Сильно: оборвать себя на полуслове → «I'll bookmark rollout. On the reliability question, the direct evidence was…»
- Слабо: «Let me finish my story first» / «as I was saying».
- Спорят: «Under assumption X, the decision was rational. In hindsight, incident Y disproved X, and I changed Z.»
- **Молчат после ответа:** не договаривай. 4 секунды тишины, потом: «Do you want more on the failure mode or the metric?»

**Уточнения.** 1–2 в начале, чтобы понять ось («implementation, org, or customer impact?»). Больше трёх до первого содержательного куска = тянет время.

**«Не знаю».** Формула практикующего EM Chris Pine:

> «I don't have that number in my head. What I remember is the order of magnitude, and here's how I'd look it up on the job.»

**[C] Meta behavioral:** «Being transparent in these situations won't be counted against you».

**Последние 60.** Три строки: измеримый результат · самый дорогой trade-off · что сделал бы иначе сегодня. Вопрос: «What is the failure mode in this team that currently consumes the most senior attention?»

---

### Комната 3. Coding (45–90 мин)

**Первые 60.** Не прыгать в код.

**[C] Meta, дословно:** «Don't jump too quickly into brute forcing the first solution… If you can't find a better solution in a reasonable time, start writing a working solution… Some interviews end without any coding because the interviewee couldn't find the ideal solution. **It's better to have non-optimal but working code than just an idea.**»

Последовательность: прочитать (объявив) → пересказать контракт (вход/выход/ошибки) → 3–5 решающих уточнений → назвать brute force и цель → **начать писать до третьей минуты**.

**⛔ [C] Anthropic: в live-интервью AI запрещён — «this is all you».** У OpenAI правила объявляют отдельно; не уверен — спроси рекрутёра ДО звонка.

**Темп.** Слот Meta: 5 intro + 35 coding + 5 вопросов. Реплики во время кода — короткие маркеры каждые 20–40 секунд («checking the empty case», «this is O(n)»), не лекция. Длинный монолог = ты не пишешь код.

**Паузы.** См. §3 — это главная механика комнаты. Объявленная пауза 5–15 секунд норма; необъявленные 40 секунд — нет.

**Давление.** Подсказка — лестница возврата к задаче, не приговор.

- Ошибка: «Yes, that input breaks my invariant. I'll fix the state transition and rerun it.»
- Намёк: «I think the hint points to repeated work. I'll quantify it before changing the algorithm.»
- Застрял: «The blocker is choosing X versus Y. I've ruled out X because Z. Does constraint W apply?»
- Слабо: спорить, что задача плохая; замирать; стирать всё.

**Уточнения.** Пустые/повторные значения · размер входа · память против latency · обработка invalid input. Потолок ~3–6 за первую минуту. Слабый уточняющий трогает синтаксис; сильный меняет структуру данных.

**«Не знаю».** **[в] Pine:** «It's a huge red flag if someone doesn't know the answer but acts like they do».

> «I don't remember the exact signature; I'll write a helper with the semantics I need.»

⚠️ Ноги разошлись про lookup: chatgpt пишет, что Anthropic разрешает поиск в своих технических интервью, но он ест время. Grok приводит прямой запрет AI. Это РАЗНЫЕ вещи (документация ≠ ассистент), но проверять политику надо у рекрутёра, а не по нашему синтезу.

**Последние 60.** Прогнать normal case → edge case → complexity. Не дописал:

> «The baseline works through X. The unresolved case is Y. The next patch is Z, followed by test W. Current complexity is…»

Не извиняться 40 секунд.

---

### Комната 4. System design LLM/agent (~45–60 мин)

**Первые 60.** Требования, не боксы. **[C] Meta: «Start with requirements.»**

Первый пакет: кто пользователь · какой наблюдаемый результат · scale и latency/SLO · safety/failure boundary · три приоритета · план разговора.

> «I'll first pin down the user outcome, scale, and failure boundary; then I'll propose the smallest architecture and pressure-test one success and one failure path.»

Слабый: сразу «we'll use Kafka and a vector DB» / рисует agent swarm.

**[в]** У Anthropic реально спрашивают про serving LLM, батчинг, GPU под переменной нагрузкой, и просят «focus more on interesting architectural considerations», не на памяти.

**Темп.** Реплики 20–40 секунд, потом вопрос интервьюеру. **Монолог 4+ минут = слабо, даже если содержание верное** — интервьюер не успел вставить constraint. Meta прямо: «Make it a conversation.»

**Паузы.** 5 секунд после «here's the request path», чтобы ткнули в слабое место. Губительно — молчать, пока рисуешь 12 сервисов.

Вслух РАЗДЕЛЯТЬ: модельная способность · детерминированный контракт · eval · runtime failure · человеческая/политическая граница.

**Давление.** «Это неправильно» на дизайне почти всегда = «ты не назвал failure mode».

> «You're right, that queue is a single point — here's the degradation.»
> «That invalidates assumption X. Components A and B survive; C does not. I'll replace C and re-check latency, eval coverage, and rollback.»

Слабо: защищать прежнюю схему ИЛИ молча выбросить всё, не показав, что именно изменилось.

**Уточнения.** Первые 3–5 минут — только требования: QPS · SLA · кто клиент · stateful vs stateless tools · human-in-the-loop · цена false positive/negative · что разрешено автоматизировать · что считается безопасным откатом. Граница: шестой вопрос без единого нарисованного компонента.

**«Не знаю».**

> «I don't know the current HBM capacity of that SKU. The design choice doesn't depend on the exact number — it depends on whether we bound batch size. I'd confirm with the infra owner.»

**[C] Meta** прямо пишет: кандидат не обязан быть экспертом во всех областях дизайна, но должен понимать, **когда нужна чужая экспертиза**.

**Последние 60.** Один success path · один failure path · bottleneck · что не успел (eval / cost / abuse) · главный остаточный риск.

Вопрос: «When this design hits production, what actually pages you?»

---

### Комната 5. Decomposition / ambiguous case

**Самая дорогая комната для твоего рефлекса.** Провал №1 назван ДОСЛОВНО одинаково всеми ногами: **прыжок к решению до разложения задачи.**

**Первые 60.** ⛔ НЕ предлагать решение.

> «Let me confirm the decision, success criterion, and two assumptions. Then I'll take twenty seconds to structure the branches.»

Или: «I'll split user / constraint / success metric / risks, then pick one slice.»

Слабый: «obviously we should fine-tune» на двадцатой секунде. Здесь первое впечатление особенно ядовито — прыжок читается как competence theatre, и дальше интервьюер ищет подтверждение.

**Темп.** Короткие ходы: гипотеза 15–30 секунд → вопрос → стоп. **Говоришь больше минуты без нового вопроса — ты уже «решил».**

**Паузы.** Здесь молчание полезнее, чем в coding. 8–12 секунд после «what else do we need to know» — это «думает», не «умер». **[C] BCG** отдельно советует не исчезать в молчании насовсем и показывать ход рассуждения.

Think-aloud = список НЕИЗВЕСТНЫХ, а не поток продуктовых идей:

> «My current hypotheses are A and B. This datum discriminates between them better than the revenue total, so I'll test it first.»

**Давление.** **Интервьюер молчит после твоего первого плана — это часто тест: продолжишь уточнять или начнёшь строить.** Сильный — ещё один вопрос. Слабый — заполняет тишину решением.

Противоречивые условия: «These constraints cannot all hold simultaneously because X. The smallest relaxation is Y; under that assumption the bounded solution is Z.»

**Уточнения.** Это и есть работа комнаты. Потолок не в количестве, а в ТИПЕ. Хорошие: кто пользователь · что считается победой · какие данные есть · какой риск нельзя допустить · за сколько надо. Граница: третий круг вопросов, который не сужает пространство.

**Тест перед вопросом:** «если мне ответят любое из двух возможных значений — изменится ли мой следующий шаг?» Нет → назови допущение и иди.

**«Не знаю» — козырь комнаты.**

> «I don't know if they have labeled traces. Two designs: if yes, eval harness first; if no, shadow log for a week. Which world are we in?»

**Последние 60.** НЕ решение — карта: проблема · 3 неизвестных · 1 следующий эксперимент · 1 kill-criterion.

> «If we had 10 more minutes I'd pressure-test the metric, not the model.»

---

### Комната 6. Customer simulation (FDE / Solutions / SA)

⚠️ **Здесь качество доказательств хуже всего.** Peer-reviewed работ по поведению кандидата в customer role-play не нашла ни одна нога; первоисточников Palantir/OpenAI/Anthropic с поминутным протоколом тоже нет. Почти всё ниже — перенос консультационной механики + (г) продавцы подготовки. Помечено честно.

**Первые 60.** Остаться в роли, начать с клиента, не с продукта.

> «Before I propose anything, what outcome is blocked, who feels it, and which constraint cannot move?»

Слабый: выходит из роли и объясняет интервьюеру свой фреймворк; или несёт клиенту «we have a five-machine fleet and two preprints», когда тот пришёл с ticket-routing.

**Темп.** Клиент говорит больше в первые 10 минут. Твои реплики 20–45 секунд, потом вопрос или playback:

> «What I'm hearing is X, caused by Y, with deadline Z. Is that accurate?»

**Паузы.** Полезна после боли клиента. Губительна, если подбираешь «впечатляющую архитектуру». Думать вслух здесь звучит как discovery: «I'm hearing latency, not accuracy — is that right?»

**Давление.** Клиент-актёр спорит, меняет KPI, говорит «нам не подходит».

> «You're right that the current plan does not protect your latency SLO. We can narrow scope this week or validate full scope before launch. Which outcome matters more?»

Слабо: спорить с переживанием клиента · обещать неподтверждённую способность · **продавать ещё громче** (ровно твой рефлекс) · принимать ложную предпосылку ради согласия.

**Уточнения.** Порядок discovery: нужный outcome → текущий workflow → кто испытывает боль и кто решает → срок → неизменяемое ограничение → данные/доступ/риск → **и только потом технология**. Граница: если через 8 минут клиент не подтвердил проблему, ты интервьюируешь не его.

**«Не знаю».**

> «I don't know your current eval set. I wouldn't recommend a model swap until we measure X. Here's a one-week probe.»

Слабое: выдуманный ROI.

**Последние 60.** Не продажа — **mutual action record**: customer outcome · что решили · кто владелец · срок · открытый риск.

> «To confirm: I own X by Tuesday; you will provide Y; we will not expand beyond Z until the latency test passes. What have I missed?»

---

### Комната 7. Values / culture

**⚠️ Комната ведёт себя ПО-РАЗНОМУ у разных компаний. Это не одна комната с одним рецептом.**

| | Anthropic | Amazon | Meta |
|---|---|---|---|
| STAR | **вредит** — читается как отрепетированный | ожидаем (LP + анекдоты) | разрешён и ожидаем |
| тон | «be a person, not a framework» | ownership + цифра в Result | конкретные анекдоты, не теория |
| спор | **вознаграждается** | «never be too quick to blame others» | обрежет теоретический тангент |

**[в; interviewing.io, инженеры Anthropic, 2026]** — источник класса «практикующие», не peer-reviewed:

- values-раунд — «where most candidates fail», ближе к therapy session, чем к STAR;
- «Don't over-engineer your answers. The instinct to structure everything like an engineer works against you here. **Be a person, not a framework**»;
- «A genuine answer is messier… might start in the wrong place, correct itself, or surface real uncertainty»;
- **«They actively look for skepticism and pushback. If asked for your honest feedback on Anthropic's mission, give one. Thoughtful disagreement lands better than agreement»**;
- follow-up-вопросы «consistently focus on emotions rather than just outcomes»;
- тривиальные примеры не проходят — нужны моменты с этическим весом.

⚠️ **Честная оговорка от chatgpt-ноги:** в актуальном ПЕРВОИСТОЧНИКЕ Anthropic утверждения «STAR вредит» НЕТ. Компания документирует только conversational clarity/judgment. Безопасный синтез: **STAR держать скрытым каркасом, но не произносить как заученный монолог.**

**Первые 60.** Без гимна миссии и без «I'm deeply aligned». Тезис + реальный конфликт ценностей:

> «My default is X, but in this case X conflicted with Y. I chose Z because…, and evidence A would have made me choose differently.»

**Темп.** Первичный ответ 60–120 секунд, потом дать копать. Не закрывать историю моралью, которую нельзя проверить.

**Паузы.** Пауза ради ПРАВДИВОГО примера — часть сигнала:

> «I want a real example rather than a polished principle. Give me a moment.»

**Давление.** Слабые крайности: капитулировать при первом возражении · защищать позицию как идентичность · изображать абсолютную моральную уверенность там, где есть trade-off.

> «I see the competing value. My choice depends on X. If evidence Y changed, I would update to Z.»

**Уточнения.** Мало. Один раз: «Are you asking about the decision itself, how I handled disagreement, or what I learned afterward?» ⛔ Не спрашивать «какую ценность вы хотите, чтобы я продемонстрировал» — у Anthropic хотят, чтобы ты применил ценности к НЕЗНАКОМОЙ ситуации.

**«Не знаю» / «у меня такого не было».**

> «I haven't actually done that — are you looking for a situation where I…? Because I can talk about that.»

Слабое: «I always do the right thing.»

**Последние 60.** Не комплимент. Один честный вопрос про реальное напряжение:

> «Tell me about a recent case where "do the simple thing" lost to a safety requirement. How was the disagreement resolved?»

---

## 5. Видеозвонок: доказанное против продающего

### Взгляд — здесь ноги РАСХОДЯТСЯ, и это надо знать

- **claudeai-a2:** Shinya et al. 2024 (Scientific Reports) → взгляд МИМО камеры значимо снижает оценки → «смотри в камеру, а не на лицо». Подано как [established].
- **grok:** та же работа, но с ограничениями — 12 говорящих, ЗАГОТОВЛЕННАЯ речь, не интерактивное интервью. **Плюс** Basch & Melchers (JBP 2024): манипуляция «simulated eye contact» даёт околонулевой эффект.
- **chatgpt:** Springer 2024 — естественное **вертикальное** отклонение ~15° (лицо собеседника под камерой) НЕ ухудшило оценки, competence или warmth. А вот **горизонтальный** взгляд на боковой экран снизил hirability и social presence.

**Что сходится у всех трёх, несмотря на конфликт:** штрафуется взгляд, который читается как **отведённый** — вниз в заметки/клавиатуру или вбок на второй монитор. Отсюда конфигурация, безопасная при любой версии:

1. камера на уровне глаз;
2. окно с лицом собеседника **прямо под линзой**;
3. взгляд гуляет камера ↔ лицо;
4. короткий взгляд в объектив на приветствии и финальной фразе;
5. ⛔ не вести разговор с лицом на **боковом** мониторе;
6. заметки на бумаге рядом с камерой лучше, чем взгляд вниз во второй экран.

«Смотреть весь час мёртво в линзу» — не доказанный must.

### Лаг и перебивания

**[established для видеодиалога, не для найма]** Переход между репликами: ~297 мс вживую против **976 мс через Zoom**. Задержка от 600 мс увеличивает перебивания и снижает ощущение естественности, **причём люди часто не осознают источник проблемы** и приписывают его собеседнику.

Практика: после вопроса — естественный короткий beat; при столкновении немедленно уступать («Sorry — go ahead»); не читать overlap как агрессию; если связь системно рвёт реплики, **назвать это вслух**, а не геройствовать.

**[b] Amazon** официально: скажи про Wi-Fi/шум вслух. Bar Raiser рассказывает про кандидата, у которого было **землетрясение** посреди интервью: тот сказал «we can keep going», интервьюер отправил его в безопасность — и человека в итоге наняли.

### Свет, фон, руки

**Фольклор:** ring light, «правильная» цветовая температура, идеальный книжный шкаф. Ни одна нога не нашла рецензируемых данных, что нейтральный фон меняет решение после контроля содержания. **[single-source; R]** В медицинском mock обычная домашняя комната не отличалась от conference room.

**Не фольклор:** чистый синхронный звук · стабильная сеть с резервом · лицо не в контровом свете · отсутствие катастрофического хаоса в кадре. Это операционная профилактика, а не прирост балла.

**Руки:** данных за обязательную видимость рук нет. **[emerging]** Широкий кадр показывает больше телесной информации, но прироста hirability против «голова-плечи» не дал. ρ=.43 в мета-анализе — это движения ГОЛОВЫ, не жесты рук.

### Второй монитор

⚠️ Публичного правила «второй монитор разрешён/запрещён» в live-интервью CodeSignal **не нашла ни одна нога**. Известно: в live-интервью интервьюер видит индикатор ухода из IDE; в **proctored** ассессменте могут писаться камера, микрофон и весь экран.

Решение: live — встроенная среда, шарить только нужный артефакт; proctored — один экран по умолчанию и письменное уточнение заранее. Не считать второй монитор автоматически читерством, но и не предполагать разрешение.

---

## 6. Акцент: что доказано и что из этого следует

**[established; два независимых мета-анализа]**

- Maindidze, Randall, Martín-Raugh, Smith 2025 (IJSA): **k=120, N=20 873**, преимущество стандартного акцента **d=0.46**.
- Schulte et al. 2024 (Applied Psychology): competence **δ=−0.70**, hirability **δ=−0.51**, warmth −0.17.
- Spence et al. 2024 (PSPB): d=0.47 по hireability.

**Самое важное в этих данных — МЕХАНИЗМ.** Штраф идёт через **стереотип компетентности**, а НЕ через понятность. Понятность, сила акцента, тип акцента, модальность и речевые требования роли **не объясняют эффект**. То есть «говори понятнее» штраф НЕ снимает.

**Модератор пола, и он в твою пользу.** Женщины с неродным акцентом: **d=0.83**. Мужчины: **d=0.18, и доверительный интервал пересекает ноль.** Для тебя (мужчина, славянский акцент) эффект в нижней части этой выборки. Не ноль, но и не приговор.

**Что реально компенсирует.** Раз штрафуют канал КОМПЕТЕНТНОСТИ — бить надо туда же: структура, цифры, I-ownership. Не темп речи.

- вывод ВПЕРЕДИ доказательства;
- одна мысль на клаузу;
- явное ударение на выводе;
- короткая пауза между пунктами.

Сигнальные фразы:

> «Short answer: yes. There are two reasons.»
> «The trade-off is X versus Y.»
> «What I personally owned was…»
> «The result was X; the remaining failure mode was Y.»
> «I'll answer the architecture first, then the rollout risk.»

⛔ **Не извиняться за акцент** — данных, что это помогает, нет; извинение кормит ровно тот стереотип компетентности, который штрафуют.

**Просить повторить — можно.** **[single-source; R]** В корпусе EAL-интервью кандидаты просили повторить или перефразировать 10 из 81 основного вопроса; негативные комментарии иногда сопутствовали эпизоду, но относились НЕ к самой просьбе; один интервьюер прямо назвал паузу разумной стратегией.

Локализовать потерянное место, а не переспрашивать вслепую:

> «I caught the latency constraint, but missed the last clause. Could you repeat just that part?»
> «I heard fifty or fifteen — could you repeat the number?»

**[b] Meta:** «If you can't hear the interviewer clearly, let them know so they can accommodate.» **[b] Amazon:** «Be communicative about your needs.»

Данных «сколько раз можно переспросить, прежде чем это станет минусом» — **не найдено**.

---

## 7. Founder-регистр: единственная жёсткая цифра корпуса

**[established для стадии ЗАЯВКИ; R]** Полевой эксперимент (Organization Science), **2400 рандомизированных заявок** на SWE-позиции: профили с фаундерским прошлым получили **13.6% callbacks против 24.0%** у сопоставимых не-фаундеров. **Почти вдвое меньше.**

Ограничения названы честно: фиктивные early-career кандидаты, США, и это **стадия заявки, а не senior-интервью**.

**Механизм — из 20 интервью с рекрутёрами в той же работе.** Дисконт объясняли опасениями fit и commitment: будет ли человек принимать руководство · работать с документацией и структурой · делить ownership · не заскучает ли и не уйдёт ли снова строить своё. Одновременно фаундеров воспринимали как creative, driven, adventurous.

> ⚠️ **Связка с нашим стоп-правилом.** У нас уже стоит запрет называть Антона фаундером в найме (never-call-anton-a-founder) — он наёмный COO/CTO и программист. Эти 13.6% против 24.0% — независимое подтверждение правила ЦИФРОЙ, а не вкусом. Подавлять надо не только слово в резюме, но и **регистр в речи**.

**Прямого исследования речевых маркеров фаундера в найме НЕ НАЙДЕНО ни одной ногой.** Публичных scorecard Anthropic/OpenAI/Google, где отдельно фиксируются «visionary register», «we built», превосходные степени — нет. То, что есть, — конвергентные свидетельства нанимающих лидеров, называющих ровно этот кластер:

- CEO Twilio слушает частоту «I» как красный флаг чрезмерного self-focus;
- другой основатель называет «говорил о себе больше, чем обо мне» крупнейшим красным флагом;
- подборка 20 культурных red flags: «минимизирует вклад других».

**[established, и это важно для баланса]** Мета-анализ impression management нашёл ПОЛОЖИТЕЛЬНУЮ связь честного self-promotion с рейтингами. **Честная самопрезентация сама по себе не вредна.** Риск появляется при неподтверждённых превосходных утверждениях и неясном личном вкладе.

---

## 8. Три привычки под сознательное подавление + готовые замены

### Привычка 1. Питч-рефлекс в первую минуту

**Ломает:** recruiter screen · customer sim · values · team matching.
**Запрет:** слова «we built / lab / preprints / fleet» ДО первого вопроса собеседника.

| было | стало |
|---|---|
| «We built the world's first category-defining multi-agent platform.» | «The observed result was X over period N. My decision was Y, and I personally verified Z. Generalization beyond those conditions remains untested.» |
| «This changes how all companies will work.» | «In this environment it changed X. The next test of whether the result transfers is Y.» |

**Телесный триггер:** захотелось сказать «world-class», «first», «revolutionary» → сначала назови **число, период и границу переноса**.

**На «why Anthropic» — не гимн:**

> «I care about the failure mode when agents look stable and aren't. That's why safety-as-organizing-principle is the job, not a constraint. I also disagree with X — happy to unpack.»

### Привычка 2. «Мы» там, где нужно «я» + масштаб вместо роли

**Ломает:** HM deep dive · behavioral · Amazon ownership.
**Правило:** после каждого «we» автоматически добавлять «My personal ownership was…»

| было | стало |
|---|---|
| «We built and deployed the fleet.» | «The team deployed five machines. I personally owned decomposition, interface contracts and acceptance tests; A was implemented by B, and I verified it through C.» |
| «Multi-agent stability» (без знаменателя) | «I ran N machines. The incident I own: the watchdog missed a silent hang for T minutes. What I changed: exit code X, so a yellow self-report couldn't mask a crash. That's the level I work at.» |

### Привычка 3. Закрытие темы декларацией вместо возвращения хода

**Ломает:** system design · decomposition · values.
**Внутреннее правило:** `тезис → одно доказательство → граница → вернуть ход`.

| было | стало |
|---|---|
| «That proves the architecture works.» | «That is the decision, the evidence, and its boundary. Would you like me to unpack the architecture, the failure, or the rollout?» |
| «So that's solved.» | «That's the path I took — the part I'd want to pressure-test with you is the eval.» |

### Привычка 3-бис (нашла только grok, но бьёт прямо в цель)

**Регистр оркестратора вслух в coding/design.** Любые формулировки вида «I'm an orchestrator rather than a coder» / «я ставлю задачу, модель исполняет».

**Почему опасно:** в live-интервью Anthropic AI запрещён; даже если роль FDE, **в третьей комнате печатаешь ты**.

| было | стало |
|---|---|
| «I orchestrate models rather than coding.» | «I own the engineering outcome: decomposition, interfaces, evals, failure handling, review and incident response. Models produced some components under tests; here is one failure I detected and fixed.» |
| — (think-aloud) | «I'll implement the parser myself. The model would be a component behind this interface with timeout T and a golden-set eval — but that's the design round. Here I'm writing the function.» |

---

## 9. Как развернуться посреди проваленного раунда

**[C] OpenAI прямо предупреждает:** интервью могут намеренно выводить за пределы комфорта и оценивают подход, решения и объяснение reasoning, **а не только завершённый ответ. Ощущение трудности само по себе не доказывает провал.**

**Протокол, 6 шагов:**

1. остановить отравленную ветку;
2. назвать конкретную ошибку и контрпример — **одной фразой, без театра**;
3. сохранить корректный инвариант;
4. выбрать минимальную корректную замену;
5. сделать ход в редакторе **в ближайшие 20 секунд**;
6. при дорогой развилке — один directional question.

> «I found a flaw in assumption X: it breaks on Y. I'm stopping this branch. Requirement Z still holds. The smallest correct recovery is A; I'll test it on B. Unless you want the diagnosis, I'll pivot now.»

**Слабый разворот:** 90 секунд самобичевания · «technically both work» при красном тесте · защита sunk cost · скрытное переписывание · блеф.

**Частота.** Один явный разворот за раунд — легитимно и читается как зрелость. **Повторные развороты внутри одного раунда читаются как потеря контроля.** Точной границы в данных нет.

**Времени мало:**

> «The baseline that works is A. The unresolved failure mode is B. My next test would be C.»

**⚠️ Каузальных данных «кандидаты, которые развернулись, получают оффер чаще» НЕ НАЙДЕНО** ни одной ногой. Есть только рубрика: verification и learning-from-failure скорятся.

---

## 10. Нервы: что работает и что опровергнуто

| приём | вердикт |
|---|---|
| **Reappraisal «I am excited»** вслух (Brooks 2014, JEP:General) | **[established механизм]** Работает лучше, чем «успокойся». >90% людей инстинктивно выбирают «calm down» — и это хуже. Пульс не падает; меняется ЯРЛЫК возбуждения. |
| **Acceptance** тревоги (RCT симулированного интервью, N=82) | **[emerging]** Снижает тревогу сильнее контроля; **suppression не помогает вообще**. |
| **Мок-интервью** (экспозиция) | Самая доказательная подготовка из всех. |
| **Сон** | **[established общая наука]** ≤6 часов бьёт по vigilant attention и рабочей памяти. Интервью-специфичного RCT нет. Обычная полная ночь; «пересыпать» смысла нет. |
| **Кофеин** | **[established]** Мета-анализ 31 RCT: небольшой прирост скорости/точности. Другой мета-анализ: рост тревоги, особенно **от 400 мг**. Обычная привычная доза; ⛔ не удваивать, не добавлять энергетик. |
| **Дыхание** | **[mixed]** Крупный placebo-controlled RCT (N=400) не нашёл преимущества медленного дыхания над matched attention control. Как ритуал переключения — можно; как способ поднять балл — нет. |
| **Power pose** | **⛔ ОПРОВЕРГНУТО** (см. §1). |
| **Разминка голоса** | **[not found]** ни за, ни против. |
| **Время суток** | Правила «только вторник в 10:00» нет. Исследование 818 структурированных интервью эффекта не нашло. Единственная слабая улика: у интервьюера к концу дня слотов время решения падает (усталость → gut) → **не бери последний слот длинного дня**. |

**Внутренняя формула перед звонком:**

> «This is activation, not an emergency. It may remain. My next observable action is to state the constraint and test one hypothesis.»

---

## 11. Между раундами, thank-you, team matching

**В паузе 5–10 минут между раундами.** Записать ТРИ факта: где был сбой · что перенести дальше · что забыть. Вода, движение 1–2 минуты, проверить звук/заряд. **Входить в следующую комнату с нуля.**

⛔ Не делать: полный посмертный разбор · писать рекрутёру, что раунд провален · менять всю стратегию из-за одного холодного интервьюера.

**Thank-you письмо.** **Каузального доказательства влияния на оффер в tech НЕ НАЙДЕНО ни одной ногой.** Цифры, гуляющие по интернету (68–80% менеджеров, «каждый пятый списывает без письма»), — опросы карьерной индустрии, часто без открытой методики, пересказываемые продавцами шаблонов. В медицинском матчинге более свежая работа нашла минимальное влияние. Anthropic/OpenAI/Meta в официальных гайдах письма не требуют.

**Вывод:** дешёвый опцион, не стратегия. Короткое письмо <24ч с ОДНОЙ конкретной отсылкой к разговору.

> «Thank you for the discussion. The constraint around X made the role especially concrete for me. I remain strongly interested and would be glad to continue.»

⛔ Не слать: apology essay · незапрошенный redesign · второе решение задачи. Правило «обязательно в 24 часа» поддерживают в основном job boards и коучи.

**Пауза 5–10 дней.** **[b] OpenAI:** отвечают обычно в течение недели после финала. Назвали срок → ждать до него плюс небольшой буфер. Срок прошёл → ОДИН спокойный check-in. Есть конкурирующий оффер → сказать сразу с точной датой. ⛔ Не слать ежедневные «value-add». **Тишину не читать как доказанный отказ.**

**Team matching.** Это уже **не оценка бара, а headcount.** Матч происходит, только если обе стороны хотят друг друга; можно пройти hiring committee и не получить оффер, если нет места.

⛔ Не проходить экзамен заново и не повторять общий питч. Вместо этого: выяснить bottleneck команды → привязать ОДИН конкретный личный опыт → сказать, что проверишь в первые 30–90 дней → снять риск fit/commitment (тот самый, из §7).

> «You said X is limiting the team. In Y, I personally owned Z. In the first month here I would test A before proposing a larger change.»

Вопросы: «What concrete problem is this approved headcount meant to solve?» · «What would make the first 90 days successful?» · «Where does this role have decision rights?» · «What changed recently that created this opening?» · **«Is there any concern about my background that I can address with a concrete example?»**

---

## 12. Что НЕ найдено (границы прибора, не отсутствие в мире)

Перечисляю честно, потому что заказ прямо этого требовал:

1. Измеренное соотношение речи кандидат:интервьюер в УСПЕШНЫХ интервью — нет ни у одной ноги.
2. Поминутные скрипты «первых 60 секунд» от Big Tech — нет. Есть слоты (5/35/5 у Meta) и рубрики.
3. Исследование founder-регистра как предиктора hire/no-hire — нет.
4. Каузальный эффект thank-you на оффер в tech — нет.
5. Политика dual-monitor в live CodeSignal interview — нет.
6. Циркадный эффект слота на решение о найме — нет.
7. Оптимальное число уточняющих вопросов — нет. «Всегда спроси три» = фольклор.
8. Доказательств, что топ-компании регулярно дают ЗАВЕДОМО нерешаемые задачи как стресс-тест, — нет.
9. RCT «краткий accent coaching убирает hiring bias» — нет.
10. Сколько раз можно переспросить, прежде чем это станет минусом, — нет.
11. Доля кандидатов, превративших проваленный раунд в положительный разворотом, — нет.
12. Peer-reviewed работы по поведению в customer role-play — нет.
13. Влияние кандидатских заметок на балл — нет.
14. Фон и свет как предиктор решения после контроля содержания — нет.

**Охват прибора:** 3 ноги, 185 источников суммарно, английский + русский веб, без платных баз (PsycINFO / APA PsycNet), 15.09.2026.

## Связано

- insight-DR-DR26-09-15-HUB-04-1610-interview-loops-ai-labs-faang — структура лупов; этот отчёт её поведенческая половина
- interview-playbook-by-round-type — плейбук, который этот синтез уточняет
- never-call-anton-a-founder — подтверждено цифрой 13.6% против 24.0%
- mission-llm-hire-weekly-plan
