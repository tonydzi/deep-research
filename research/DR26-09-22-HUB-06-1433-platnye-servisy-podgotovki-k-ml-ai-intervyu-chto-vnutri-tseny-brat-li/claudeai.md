# DR26-09-22-HUB-06-1433 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-09-22-HUB-06-1433
rail: claudeai
mode: Research (extended research task, claude.ai web, Fable 5.1 Max)

chat: https://приватный чат
collected: 2026-09-23 07:20 Lisbon

sources_counted_by_vendor: 208
---

Запрос сформулирован полностью и однозначно — запускаю глубокое исследование по всем шести блокам без уточняющих вопросов. Собираю данные по сервисам поимённо (Interview Kickstart, interviewing.io, Exponent, Educative, Hello Interview, IGotAnOffer, Karat, Formation, Pathrise, Rora, Levels.fyi Negotiation и др.), отзывы с Reddit/Blind/Trustpilot, проверяемость их заявленных результатов, покрытие RS/MTS-трека и бесплатные эквиваленты. Отчёт будет на русском, со сравнительной таблицей и «Proof of work» в конце.

Your report on paid ML/AI interview-prep services is ready. It evaluates 13 services (Interview Kickstart, interviewing.io, Prepfully, Exponent, Educative, Hello Interview, IGotAnOffer, Tech Interview Handbook, Karat, Formation, Pathrise, Rora, Levels.fyi) across format, pricing, refund/ISA terms, ML/AI and research-track coverage, and reviews. Key takeaways: mass bootcamps (IK, Pathrise, Formation) are built for FAANG SWE/MLE loops—not lab research loops—and are largely wasted money for a senior/staff RS/MTS/RE profile. The report recommends a targeted approach (Scenario B, ~$1,250–$2,500 plus success-based negotiation): free self-prep as the backbone, 4–6 mock interviews with real lab-level engineers, one research-focused coaching package (Sundeep Teki / Alireza Dirafzoon), and Rora for lab-offer negotiation. It also includes a free-alternatives map (Chip Huyen, Deep-ML, NeetCode ML, Hello Interview), a staged prep plan with thresholds, and clear caveats on hidden pricing (IK, interviewing.io, Rora) and unverified marketing success rates.

===ARTIFACT Paid ML and AI Interview Prep Services: Analysis, Pricing, and Verdict===
# DR26-09-22-HUB-06-1433 — Платные сервисы подготовки к ML/AI-интервью: разбор, цены, вердикт

## TL;DR
- Для профиля senior/staff, целящегося в Research Scientist / MTS / Research Engineer в топовых ИИ-лабораториях, **массовые платные буткемпы (Interview Kickstart, Pathrise, Formation) — деньги на ветер**: они заточены под FAANG SWE/MLE-луп, а не под research-лупы (paper deep-dive, «research taste», ML from scratch). Это подтверждает даже Hello Interview: их ML-гайд дословно «not useful for AI/ML research engineering interviews».
- **Что реально стоит купить точечно**: 4–6 мок-интервью с действующими инженерами (interviewing.io / Prepfully, ~$180–$400 за сессию) незадолго до онсайта + одну нишевую research-коучинг-программу (Sundeep Teki, Alireza Dirafzoon/aimlinterviews.io) + переговоры (Rora на % от прироста или Levels.fyi за фикс). Итого прямых расходов ~$1,250–$2,500 (плюс success-based комиссия за переговоры) против $6,000–$12,000 за «полный» буткемп.
- **Почти всё содержание платных программ воспроизводится бесплатно** (Chip Huyen ML Interviews Book, Deep-ML, NeetCode ML, Hello Interview System Design in a Hurry, публичные гайды OpenAI/Anthropic/DeepMind, бесплатные peer-моки). Платить стоит только за то, что бесплатно не воспроизводится: **живая обратная связь от реального интервьюера уровня лаборатории** и **переговоры на успех с доступом к comp-данным**.

## Ключевые выводы (Key Findings)
1. **Специализированной подготовки именно под research-лупы лабораторий на массовом рынке практически нет.** Она существует только в виде нишевых индивидуальных коучей (Sundeep Teki, Alireza Dirafzoon) и маркетплейсов, где можно вручную выбрать коуча-исследователя (Prepfully, interviewing.io, IGotAnOffer). Крупные буткемпы (IK, Pathrise, Formation) research-трек не покрывают.
2. **Цены крупных буткемпов скрыты за звонком/вебинаром.** IK не публикует цены на сайте — требуется «pre-enrollment webinar»; реальные цифры из отзывов $5,000–$12,000. Pathrise и Formation работают по ISA-подобным моделям (доля от зарплаты/прироста).
3. **Маркетинговые цифры IK и Pathrise не имеют независимого подтверждения** — методология и выборка не раскрыты; у самих сервисов цифры даже противоречат друг другу (см. ниже).
4. **Мок-интервью и переговоры — единственные компоненты с высоким ROI** для senior/staff, потому что именно они не воспроизводятся бесплатно с нужным качеством.

## Главная сравнительная таблица

| Сервис | Формат | Длительность | Кто преподаёт | Цена (подтверждённость) | Возврат / ISA | ML/AI | Research-трек | Отзывы | Вердикт |
|---|---|---|---|---|---|---|---|---|---|
| **Interview Kickstart** | Когорта + 1:1 + моки | 16 нед + 6 мес поддержки | «500+ FAANG» [маркетинг]; качество варьируется [независ.] | **$5,000–$12,000** (цена **скрыта за вебинаром**, цифры из отзывов) | Узкое refund-окно, жалобы CFPB/BBB | Да (ML/GenAI/Agentic) | **Нет** | Смешанные→негативные | ❌ Не брать |
| **interviewing.io** | Анонимные моки + Dedicated Coaching | По сессиям | Действующие Senior/Staff/Principal (FAANG, OpenAI) | $179 старт / $225–$339 company-specific / ~$2,000 за 3 сессии / $4,000 за 10 (**не публикуется**, из обзоров) | Возврат при неудовлетворённости | Да (ML-мок) | Частично (ручной выбор) | Позитив, критика по цене | ✅ Брать точечно |
| **Prepfully** | Маркетплейс коучей | По сессиям | Действующие/бывшие инженеры компаний | ~$89–$419/сессия (**опубликовано пофамильно**) | — | Да | Частично | Позитив | ✅ Брать точечно |
| **Exponent (Aced)** | Подписка + peer-моки + коучинг | Подписка | Курсы + коучи FAANG | $79/мес; ~$144/год; коучинг $199–$249/ч (**опубликовано**) | Возврат 5 дней | Да (ML system design) | Нет | Позитив | ◐ Опц. |
| **Educative** | Текстовые курсы, self-paced | Подписка | Авторы курсов (нет живых людей) | $59/мес; ~$199/год (часто -50–72%) (**опубликовано**) | — | Да (контент) | Нет | Нейтрально | ◐ Опц. |
| **Hello Interview** | Self-serve гайды + AI-tutor | Разовая покупка | Нет живых (коучинг закрыт 31.05.2026) | $47/мес, $79/год, $279 lifetime (**опубликовано**) | — | Да (ML SD) | **Явно нет** | Позитив | ◐ Для SD |
| **IGotAnOffer** | Маркетплейс + гайды | По сессиям | Коучи FAANG | $100–$300+/ч (коучинг) | — | Да | Частично (RS-гайды) | Позитив | ◐ Опц. |
| **Tech Interview Handbook** | Бесплатный ресурс | — | Yangshun Tay (open-source) | **Бесплатно** | — | Нет (SWE) | Нет | Позитив | ✅ Бесплатно |
| **Karat (Brilliant Black Minds)** | B2B-вендор; для кандидатов — бесплатные моки | — | Senior SWE | **Бесплатно** (eligible-кандидаты) | — | Нет (SWE) | Нет | Позитив (NPS +72) | ✅ Если eligible |
| **Formation** | Fellowship, unlimited моки | До оффера | Senior/staff FAANG+ | $2,500/мес ИЛИ ~$5,000 upfront + variable (**опубликовано**) | ISA-подобная (Affirm до 3 лет), $500/нед при выходе | Огранич. (SWE) | Нет | Смешанные | ❌ Не под research |
| **Pathrise** | Fellowship + ISA | 12–24 мес | Менторы «top performers» | **ISA**: 7/11/14% (cap $12/18/24k) [независ.] ИЛИ 9%/6мес/$13k [сайт] | ISA списывается при отсутствии оффера за 12 мес | Не спец. | Нет | Много негатива (Trustpilot) | ❌ Не брать |
| **Rora** | Переговоры + research-коучинг | По кейсу | Переговорщики, спец. по AI-research | **% от прироста** (точный % **не публичен**); «no increase, no fee» | Платишь только при принятии оффера | Да (специализация) | Да (переговоры) | 5★, позитив | ✅ Для оффера лаборатории |
| **Levels.fyi Negotiation** | Переговоры (коучинг) | По кейсу | Бывшие рекрутёры топ-компаний | **Фикс** (истор. $650 mid / $1,800 senior; сейчас выше, с money-back) | Money-back гарантия прироста | Да | Нет | Смешанные | ✅ Для обычного tech-оффера |

*Условные обозначения: ✅ рекомендуется / ◐ опционально / ❌ не рекомендуется для данного профиля.*

## Детали по блокам

### Блок 1. Разбор сервисов поимённо

#### Interview Kickstart (IK) — ML/AI и Applied Agentic AI треки
- **Формат**: когортный, live-классы + записи + 1:1 коучинг + мок-интервью с «Silicon Valley engineers from FAANG». ML Flagship и Applied Agentic AI — отдельные треки.
- **Длительность**: 16 недель live-классов + «6 месяцев поддержки» [маркетинг].
- **Кто преподаёт**: «500+ instructors from FAANG» [маркетинг]. Независимо: качество инструкторов «сильно варьируется между темами» (IGotAnOffer со ссылкой на Reddit). Trustpilot-отзыв: компания «registered in Bangalore, Karnataka, India», выдающая себя за Silicon Valley [независимо].
- **Цена**: **скрыта за вебинаром** — на сайте нет прайса. Реальные цифры из отзывов: **$5,000–$12,000**; ML/DS-треки $9,000–$11,000; пользователь BBB назвал $6,300; на Blind фигурируют $6,000–$10,000 [независимо].
- **Возврат/гарантии**: заявлена «job placement + full refund guarantee», но на практике — узкое «Week 1 refund window», отказы, задержки возврата 5+ недель, жалобы в CFPB и BBB [независимо].
- **Покрытие ML/AI**: да. **Research-трек**: нет.
- **Верификация цифр**: маркетинговая PR-цифра — «average salary hike of 66.5%» и «$300K+ offers», но **официальный собственный блог IK называет другую цифру**: «In 2021, our alums who upleveled received an average pay hike of 53%», а «highest offer received by an Interview Kickstart alum is $1.2 Million» (в других материалах — $1.28M). **[Маркетинг], независимого подтверждения нет; собственные цифры IK противоречивы (53% vs 66.5%), «$1.2M» — это выброс-outlier, а не типичный результат.**

#### interviewing.io
- **Формат**: анонимные (voice + shared editor, без видео) мок-интервью с Senior/Staff/Principal инженерами из FAANG и **OpenAI**; типы: coding, system design, ML, behavioral, EM. Плюс «Dedicated Coaching» пакеты (3/5/10 сессий под конкретную компанию), бесплатный AI-интервьюер и 1 бесплатный peer-мок при регистрации.
- **Кто преподаёт**: действующие инженеры, делающие найм.
- **Цена**: **не публикуется на сайте** («does not publish prices publicly»); сторонние обзоры 2026 стабильно: **$179 старт, $225–$300+ за company-specific, до $339 за FAANG-branded**; пакет из 3 сессий ~$2,000 ($667/сессия); 10 сессий — $4,000 (со скидкой $500 на Blind). Free peer-моки, Pay Later, периодические скидки $200–$700.
- **Возврат**: полный возврат при неудовлетворённости сессией.
- **Покрытие ML/AI**: да, ML-мок как тип. **Research-трек**: только если вручную попадётся интервьюер из лаборатории.

#### Prepfully
- **Формат**: маркетплейс из 1912 коучей, выбор по роли/компании (ML Engineer, Data Scientist, Research); мок-интервью, resume review, AI-моки (beta).
- **Кто преподаёт**: действующие/бывшие инженеры конкретных компаний, профиль виден до брони.
- **Цена**: **опубликована пофамильно**: сессии ~$89–$419; типично $120–$250/час; senior FAANG — верх диапазона.
- **Покрытие ML/AI**: да. **Research-трек**: частично (ручной выбор коуча-исследователя).

#### Exponent (tryexponent, ребрендинг в Aced)
- **Формат**: подписка на курсы + peer-моки + отдельный платный коучинг. Есть **ML System Design** и Generative AI курсы.
- **Цена**: **опубликована**: Monthly $79; Annual ~$144/год (часто со скидкой до ~$12/мес при годовой оплате); коучинг отдельно ~$199–$249/час; 5 бесплатных peer-моков/мес на free-плане. Полный возврат в течение 5 дней.
- **Покрытие ML/AI**: да. **Research-трек**: нет.

#### Educative
- **Формат**: текстовые интерактивные курсы (Grokking the ML Interview, Grokking GenAI System Design), self-paced, без живых людей.
- **Цена**: **опубликована**: Monthly $59; Educative Unlimited ~$199/год (часто -50–72%, ~$80–150); отдельные курсы $79–$99.
- **Покрытие ML/AI**: да (контент). **Research-трек**: нет. Нет обратной связи от человека.

#### Hello Interview
- **Формат**: self-serve письменные гайды + guided practice + AI-tutor. Есть **ML System Design** трек. **Живые мок-интервью и коучинг закрыты 31 мая 2026** — сервис полностью self-serve.
- **Цена**: **опубликована**, разовая (не подписка): $47/мес, $79/год, $279 lifetime; есть free-tier.
- **Покрытие ML/AI**: да (ML system design, applied). **Research-трек**: **явно НЕТ** — их же гайд гласит дословно: «This guide is not useful for AI/ML research engineering interviews».

#### IGotAnOffer
- **Формат**: маркетплейс коучей + гайды. Есть ML mock, Research Scientist гайды (OpenAI/Meta RS).
- **Цена**: коучинг-сессии платно ($100–$300+/час у коуча); гайды по компаниям платно.
- **Покрытие ML/AI**: да. **Research-трек**: частично — есть текстовые RS-гайды и коучи, но не структурированная программа под лупу.

#### Tech Interview Handbook
- **Формат**: **полностью бесплатный** open-source ресурс. Есть раздел про negotiation. **Не** ML/research-специфичен (SWE-фокус).

#### Karat
- **Формат**: B2B-вендор интервью. Для кандидатов — **Brilliant Black Minds** (бесплатные мок-интервью + коучинг для чёрных инженеров, при поддержке Serena Williams). NPS +72; заявлено «3 mock interviews make you 6x more likely to get hired».
- **Цена**: бесплатно (для eligible). **Research-трек**: нет (SWE coding/system design).

#### Formation (formation.dev)
- **Формат**: «Fellowship» — адаптивная программа с unlimited мок-интервью и менторством от senior/staff инженеров FAANG+; заявка + тех-ассессмент на входе.
- **Цена**: **опубликована**: $2,500/мес ИЛИ ~$5,000 upfront + variable (доля от прироста base). Пример с сайта: base $150K → $5,000 upfront; при росте до $175K доплата $7,500. Финансирование через Affirm до 3 лет. При раннем выходе — «$500 per week».
- **Покрытие ML/AI**: ограниченно (в основном SWE). **Research-трек**: нет.

#### Pathrise
- **ISA-модель**: третьи стороны (IGotAnOffer) описывают 3 tier'а — **7% ($12k cap) / 11% ($18k cap) / 14% ($24k cap), проценты PRE-tax**; ранний выход — штраф $250/неделя; при отсутствии оффера за 12 мес ISA списывается. **Собственный сайт Pathrise заявляет иную модель** — «usually 9% paid over 6 months, capped at $13k». Это расхождение — само по себе сигнал непрозрачности.
- **Отзывы/претензии**: 414+ отзывов на Trustpilot, много негатива про ISA как «high-interest loan»; рекомендации подавать жалобы в CFPB (упоминается Edly как покупатель долга).
- **Покрытие ML/AI**: не специализирован. **Research-трек**: нет.
- **Верификация**: «96–98% placement», «средний рост 10–20%» — **[маркетинг], без независимого подтверждения**.

### Блок 2. Отзывы прошедших и верифицируемость цифр
- **IK**: смешанные. Позитив на Trustpilot (много отзывов, но обвинения в накрутке), сильный негатив на BBB/Reddit/Blind: агрессивные продажи, «bait & switch», проблемы с возвратом, жалобы в CFPB. Blind-консенсус: «not worth it», «daylight robbery».
- **Pathrise**: много негатива про ISA.
- **interviewing.io**: преимущественно позитив на Reddit/Blind, критика в основном про цену, не качество.
- **Rora**: 5★ на Trustpilot, позитив. **Levels.fyi negotiation**: смешанные — есть негатив («wasted my money», «they have nothing to lose»).
- **Верификация маркетинговых цифр**: ни IK (53%/66.5%), ни Pathrise (96–98%) не раскрывают методологию/выборку → **считать неподтверждёнными**.

### Блок 3. Покрывают ли research-трек
**Честный ответ: массовый рынок — нет.** Research-луп (research deep-dive/presentation, ML from scratch, ML system design в стиле лаборатории, «research taste», paper discussion) не покрывается буткемпами. Его закрывают:
- **Индивидуальные коучи**: Sundeep Teki (Oxford PhD, ex-Amazon Alexa AI) — гайды по OpenAI/Anthropic/DeepMind ($99 за гайд, $249 за bundle трёх, с зачётом стоимости гайда при апгрейде на коучинг в 30 дней) + 1:1 коучинг; Alireza Dirafzoon (aimlinterviews.io, автор machine-learning-interviews на GitHub, 9.5k★) — коучинг под Applied Scientist/RS/RE/GenAI/Agentic.
- **Маркетплейсы** с ручным выбором research-коуча: Prepfully, interviewing.io, IGotAnOffer.
- **Бесплатные первоисточники**: публичные interview-гайды самих лабораторий и агрегированные разборы (interviewing.io OpenAI/Anthropic guides, igotanoffer RS-гайды, посты кандидатов Yuan Meng и Mimansa Jaiswal).

### Блок 4. Что воспроизводится бесплатно

| Компонент платной программы | Бесплатный эквивалент | Чего не хватает бесплатно |
|---|---|---|
| ML-теория / knowledge вопросы (IK, Educative Grokking) | Chip Huyen «Introduction to ML Interviews» — «over 200 knowledge questions» + 30 open-ended, бесплатно на huyenchip.com/GitHub (автор получила офферы в Google, NVIDIA, Snap, Netflix, Snorkel AI) | Структуры/расписания, подотчётности |
| ML coding from scratch (IK Applied ML) | Deep-ML (бесплатно, softmax/GPT from scratch), NeetCode ML, alirezadir/AIMLInterviews GitHub | Проверки кода реальным интервьюером |
| DSA / LeetCode | NeetCode (Blind 75/150/250), LeetCode | Мок под давлением |
| System design / ML system design (Exponent, Educative) | Hello Interview «System Design in a Hurry» (бесплатно), ByteByteGo-подобные материалы | Свежести, обратной связи |
| Мок-интервью (IK, Formation) | Exponent/Pramp peer-моки (бесплатно), interviewing.io 1 free peer-мок, Karat BBM (для eligible) | **Интервьюер уровня лаборатории и качественный фидбэк** |
| Research/lab-специфика | Публичные гайды interviewing.io (OpenAI/Anthropic), igotanoffer RS-гайды, посты кандидатов | Персональной калибровки под «research taste» |
| Переговоры (Rora, Levels) | Tech Interview Handbook negotiation, Levels.fyi данные о зарплатах | Живого переговорщика на успех |

**Единственное, что бесплатно не воспроизводится**: (1) живая обратная связь от реального интервьюера уровня лаборатории, (2) переговоры на успех с доступом к comp-данным.

### Блок 5. Переговоры о компенсации
- **Rora (teamrora.com)**: модель — **% от прироста оффера**, «no increase, no fee»; **точный процент НЕ публикуется**, раскрывается только на бесплатном созвоне (сторонние оценки 10–30%, но не подтверждено первоисточником — трактовать как непубличное). Заявленные результаты (маркетинг, главная страница): «We've helped more than 1,200 researchers in 8 years» и total negotiated salary с 2017 г. **Явно специализируется на ИИ-лабораториях**: OpenAI, Anthropic, DeepMind, FAIR, Google Brain — есть «Rora 2023 Salary Negotiation Report for AI Researchers» с кейсами (OpenAI $665K→$865K, +30%; Google $216K→$526K total). xAI и термин PPU (profit participation units) на сайте Rora явно не найдены, но специализация на research-офферах с нестандартным equity подтверждена.
- **Levels.fyi Negotiation**: **фиксированный** прайс с money-back гарантией прироста; переговорщики — бывшие рекрутёры топ-компаний; исторически $650 (mid) / $1,800 (senior), сейчас существенно выше. Работает только до принятия оффера.
- **Verdict по переговорам для лаборатории**: для оффера ИИ-лаборатории с нестандартной структурой (PPU/profit units, необычные RSU) — **Rora предпочтительнее**, т.к. специализируется именно на research-офферах и её риск структурно на вашей стороне (платишь только с прироста). Levels.fyi — если хочется предсказуемого фикса и просто «поднять на $10–30K» на обычном tech-оффере.

### Блок 6. Итоговый вердикт в деньгах и часах (профиль: senior/staff → RS/MTS/RE в ИИ-лаборатории)

**Сценарий A — «купить всё» (НЕ рекомендуется)**: IK ML Flagship ($9,000–$11,000) + interviewing.io пакет ($2,000) + Rora (% от прироста, порядка $5,000–$15,000 при оффере $600K+). Итого **$16,000–$28,000+**. Часы: 200–300 ч. **Вывод: переплата, research-трек всё равно не покрыт.**

**Сценарий B — «точечно: моки + research-коуч + переговоры» (РЕКОМЕНДУЕТСЯ)**: research-гайды Sundeep Teki ($249) + 4–6 индивидуальных research/ML-мок-сессий (interviewing.io/Prepfully/aimlinterviews, ~$200–$400 × 5 = $1,000–$2,000) + Rora на % ИЛИ Levels.fyi фикс. Прямые расходы (до переговорной комиссии) **~$1,250–$2,500**; переговоры — success-based. Часы: 120–180 ч (в основном бесплатная самоподготовка + ~6 платных сессий).

**Сценарий C — «всё бесплатно»**: Chip Huyen + Deep-ML + NeetCode ML + Hello Interview + публичные lab-гайды + бесплатные peer-моки + Tech Interview Handbook negotiation. Расходы **$0** (опц. LeetCode Premium ~$159/год). Часы: 150–250 ч (дольше из-за отсутствия структуры и калибровки).

**Рекомендация для Anton Dziatkovskii**: идти по **Сценарию B**. Костяк подготовки — бесплатный (Сценарий C), но купить: (1) 4–6 мок-сессий с людьми из ИИ-индустрии за 2–4 недели до онсайтов; (2) один research-ориентированный коучинг-пакет; (3) переговоры через Rora на офферах лабораторий. Не покупать когортные буткемпы (IK/Formation) и ISA-программы (Pathrise) — они под FAANG SWE, не под research-лупы.

## Рекомендации (staged, с порогами)
1. **Недели 1–6 (бесплатно)**: Chip Huyen book, Deep-ML/NeetCode ML from scratch, Hello Interview ML system design, чтение 5–10 свежих статей целевых лабораторий, публичные lab-гайды. **Порог перехода**: стабильно решаешь ML-coding и можешь час говорить по 3 статьям.
2. **Недели 4–8 (платно, точечно)**: купить research-гайд(ы) (Sundeep Teki $249) и 2–3 мок-сессии для диагностики. **Порог**: фидбэк выявляет конкретные пробелы, а не «всё плохо».
3. **За 2–4 недели до онсайтов (платно)**: 3–4 мок-сессии с интервьюерами из лабораторий (interviewing.io/Prepfully).
4. **На стадии оффера (success-based)**: Rora для лаборатории, Levels.fyi для обычного tech-оффера.

**Что изменит рекомендацию**: если бы появился структурированный когортный research-луп-буткамп с действующими интервьюерами лабораторий и прозрачной ценой — он мог бы заменить шаги 2–3. Пока такого продукта на рынке нет.

## Caveats
- Цены IK и interviewing.io **не публикуются на сайтах** — все цифры из сторонних обзоров/отзывов, помечены как таковые.
- Маркетинговые проценты успеха **не подтверждены независимо**; у IK собственные цифры противоречивы (официальный блог — 53% за 2021 г. против PR-цифры 66.5%), «$1.2M offer» — выброс, не типичный результат. У Pathrise собственная модель ISA (9%/6мес/$13k) не совпадает с описанной третьими сторонами (7/11/14%).
- Точный процент Rora **не публичен**; оценка 10–30% не подтверждена первоисточником.
- Prepfully/interviewing.io имеют ML-мок как тип, но **отдельного research-продукта под лупы лабораторий у массовых сервисов нет** — только ручной выбор коуча.
- Индивидуальные коучи (Teki, Dirafzoon) — их заявленные результаты («100+ placements», «10,000+ candidates») тоже маркетинг без независимого аудита.

## Первичные источники
1. https://www.interviewkickstart.com/machine-learning — «Alumni landing jobs with $300K+ offers, and the highest compensation at a whopping $1.28 Million» [маркетинг]
2. https://www.interviewkickstart.com/reviews — отзывы студентов (Applied Agentic AI, ML Flagship)
3. https://www.trustpilot.com/review/interviewkickstart.com — «pseudo-scam business... registered in Bangalore»; проблемы с возвратом
4. https://www.bbb.org — профиль и жалобы IK: «$6,300... CFPB complaints filed», «Week 1 refund window»
5. https://www.teamblind.com — многочисленные треды «is IK worth $6000/$7k/$10k» — консенсус «not worth it»
6. https://scale.jobs/blog/interview-kickstart-costs-10k-plus-worth-it — «$2,400 to over $12,000... ML/DS range from $9,000 to $11,000»
7. https://prachub.com — «$5,000 to $12,000 range reported repeatedly» (IK)
8. https://interviewkickstart.hashnode.dev — «average pay hike of 53%... sum of all offers $113,890,000» [собственные данные IK]
9. https://interviewing.io/coaching и обзоры (devinterview.ai, spacecomplexity.ai, finalroundai.com, lodely.com) — «$179 to $340 per session... 3-session package around $2,000», «does not publish prices publicly»
10. https://interviewing.io/openai-interview-questions и https://interviewing.io/anthropic-interview-questions — публичные lab-гайды
11. https://prepfully.com/coaches — цены коучей ($89–$419), 1912 коучей
12. https://www.tryexponent.com/coaching и обзоры — $79/мес, ~$144/год, коучинг $199–$249; ML System Design курс
13. https://www.hellointerview.com/pricing — $47/мес, $79/год, $279 lifetime
14. https://www.hellointerview.com/learn/ml-system-design/in-a-hurry — «This guide is not useful for AI/ML research engineering interviews»
15. https://huyenchip.com/ml-interviews-book — бесплатная книга, «over 200 knowledge questions» + 30 open-ended
16. https://github.com/chiphuyen/ml-interviews-book — исходники книги
17. https://www.deep-ml.com — бесплатная ML-coding-платформа (from scratch)
18. https://neetcode.io/practice/machine-learning — бесплатные ML-задачи from scratch
19. https://github.com/alirezadir/AIMLInterviews и https://aimlinterviews.io — гайд + коучинг под RS/RE/Applied Scientist
20. https://sundeepteki.org/company-guides.html — «$99 each or $249 for all three labs» (OpenAI/Anthropic/DeepMind)
21. https://formation.dev/pricing — «$5,000 upfront... additional $7,500 due to increase»; Blind: «2.5k monthly or 5k + RSA»
22. https://igotanoffer.com/en/advice/pathrise-alternatives — «Basic: 7% ($12k cap), Essentials: 11% ($18k cap), Pro: 14% ($24k cap)... PRE-tax»
23. https://www.trustpilot.com/review/pathrise.com — «works like a high-interest loan... File a complaint with the CFPB»
24. https://karat.com/brilliant-black-minds — бесплатные моки, NPS +72, «100% free»
25. https://www.teamrora.com (главная и 2023 AI Researchers Salary Negotiation Report) — «We charge as a percentage of the offer increased... Only pay if you accept»; «1,200 researchers in 8 years»; кейс OpenAI +30%
26. https://www.techinterviewhandbook.org/negotiation — «Levels.fyi charges a flat fee whereas Rora takes a percentage of the negotiated difference»
27. https://www.levels.fyi/services и Blind-треды — фикс-цена ($650 mid / $1,800 senior истор.), money-back гарантия
28. https://igotanoffer.com — Interviewing.io / OpenAI RS / Meta RS / Anthropic гайды и обзоры

## Proof of work
**Выполненные запросы (web_search, 18 шт.):** Interview Kickstart ML price reviews; interviewing.io ML pricing; Pathrise ISA terms; Rora fee reviews; Levels.fyi negotiation cost; Prepfully research scientist mock; Formation.dev ISA reviews; Karat Brilliant Black Minds; Chip Huyen ML book free; Exponent subscription price; Hello Interview pricing ML; IK complaints refund BBB; research scientist interview prep coach; interviewing.io dedicated coaching Anthropic; IK cost $6000 reddit; Educative subscription price; Deep-ML NeetCode free ML; interviewing.io coaching 3 sessions; (Sundeep Teki price и Rora % — прерваны исчерпанием бюджета).

**Открытые/процитированные страницы:** interviewkickstart.com (×3), trustpilot.com (IK, Pathrise, Rora), bbb.org (IK), teamblind.com (×8: IK, Formation, Levels, Exponent), scale.jobs, prachub.com, igotanoffer.com (×6), interviewing.io (×4), prepfully.com (×3), tryexponent.com (×2), hellointerview.com (×3), huyenchip.com, github.com (chiphuyen, alirezadir), deep-ml.com, neetcode.io, formation.dev, mentorcruise.com, thecollegeinvestor.com, careerkarma.com, glassdoor.com, candor.co, teamrora.com (×5), levels.fyi (×5), techinterviewhandbook.org, sundeepteki.org, aimlinterviews.io, karat.com (×4), coursereport.com, finance.yahoo.com.

**Целевой субагент** был направлен на самый тонкий участок — точная модель оплаты Rora, её результаты по офферам ИИ-лабораторий (PPU/нестандартный equity) и точные фикс-цены Levels.fyi. Субагент подтвердил: Rora НЕ публикует конкретный процент (только «% of the negotiated delta», раскрывается на созвоне); нашёл кейсы AI-labs (OpenAI $665K→$865K); текущие фикс-цены Levels.fyi выросли до $1,250/$2,450/$5,000 (истор. $650/$1,800). Затем черновик прошёл enrich_draft, который уточнил дословную формулировку Hello Interview, состав книги Chip Huyen, собственные цифры IK (53% vs 66.5%) и расхождение в ISA Pathrise.

**Что НЕ удалось подтвердить / где цены скрыты:**
- **Точный процент Rora** — не публичен, скрыт за бесплатным созвоном. Оценка 10–30% — из вторичных источников, первоисточником не подтверждена.
- **Цена Interview Kickstart** — скрыта за обязательным вебинаром/звонком; все цифры ($5–12K) — из отзывов, не с официальной страницы.
- **Цена interviewing.io** — не публикуется на сайте; все цифры из сторонних обзоров 2026 г.
- **Точные текущие цены Levels.fyi negotiation** и **точная актуальная ставка Rora** прямо со страниц pricing на 23.09.2026 — не открывались в основном цикле (бюджет исчерпан); субагент дал текущие Levels.fyi ($1,250/$2,450/$5,000) со ссылкой на levels.fyi/services.
- **Маркетинговые проценты успеха IK (53%/66.5%) и Pathrise (96–98%)** — методология/выборка не раскрыты; независимого подтверждения нет.
- **Термин PPU и xAI** на сайте Rora явно не найдены (хотя специализация на AI-lab offers подтверждена).

**Уровень уверенности по блокам:**
- Блок 1 (разбор сервисов): **высокий** для опубликованных цен (Exponent, Educative, Hello Interview, Formation, Prepfully, Levels.fyi); **средний** для скрытых цен (IK, interviewing.io, Rora — диапазоны из отзывов).
- Блок 2 (отзывы/верификация): **высокий** по факту наличия жалоб; **высокий** по выводу о неверифицируемости маркетинговых цифр.
- Блок 3 (research-трек): **высокий** — множественное подтверждение отсутствия массовой research-подготовки.
- Блок 4 (бесплатные эквиваленты): **высокий**.
- Блок 5 (переговоры): **средне-высокий** — модели подтверждены, точный % Rora не публичен.
- Блок 6 (вердикт в деньгах/часах): **средний** — расчёты опираются на диапазоны цен и оценочные часы подготовки.
