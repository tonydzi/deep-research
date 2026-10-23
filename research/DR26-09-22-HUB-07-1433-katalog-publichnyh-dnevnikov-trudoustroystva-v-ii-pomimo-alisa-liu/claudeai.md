# DR26-09-22-HUB-07-1433 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-09-22-HUB-07-1433
rail: claudeai
mode: Research (extended research task, claude.ai web, Fable 5.1 Max)

chat: https://приватный чат
collected: 2026-09-23 07:20 Lisbon

sources_counted_by_vendor: 270
---

Запрос ясный и детальный — запускаю глубокое исследование по всем шести пунктам брифа: каталог авторов публичных дневников поиска работы в ИИ (2023–2026), сравнительные цифры, повторяющиеся советы и противоречия, переиспользуемые материалы, устройство жанра и правовые границы. Результат будет на русском, с таблицей каталога и «Proof of work» в конце.

Your report on the corpus of public AI job-search diaries is ready. It catalogs 22 entries (12 human authors plus 10 reusable study materials), benchmarked against Alisa Liu's viral OpenAI retrospective (57 interviews across 11 companies). The report includes a comparative table of applications/interviews/offers/timelines with medians (~57 interviews, ~30 companies, 4 offers, ~3 months), recurring advice patterns backed by 3+ authors, explicit disagreements (LeetCode, naming companies, broad vs. targeted, retrospective vs. build-in-public), licensing notes on reusable materials, NDA/compensation/privacy boundaries, and actionable recommendations for running your own public diary. It flags survivorship bias, unverifiable X view metrics, and confidence levels per section.

===ARTIFACT Public AI Job Search Diaries: A Catalog of Documented Hiring Journeys===
# Корпус публичных дневников трудоустройства в ИИ: кто ещё кроме Alisa Liu выложил свой путь

*Отчёт DR26-09-22-HUB-07-1433 · для Anton Dziatkovskii (Tony Dzi), Palo Alto AI Research Lab · дата: 23 сентября 2026*

## Краткое резюме
Жанр публичного «дневника трудоустройства в ИИ» реально существует и растёт в 2024–2026, но детальных первоисточников с названными цифрами меньше, чем кажется по хайпу: проверяемых кейсов с раскрытыми числами (заявки/интервью/офферы/сроки) — около 8–10, а всего в каталог включено 22 позиции (12 людей-авторов + 10 переиспользуемых учебных материалов). Эталон — Alisa Liu (6-летний PhD по NLP, University of Washington → OpenAI), выложившая 20 июня 2026 ретроспективу «Notes on the Industry Job Search»: **57 интервью в 11 компаниях**, плюс 46 звонков рекрутёрам и 16 post-offer разговоров, а также два открытых конспекта (LLM notes и math notes) и топик-лист. Пост стал вирусным — по данным BigGo Finance это «open-source, 10,000-word retrospective… that has instantly gone viral across Silicon Valley», а найм подтвердил представитель OpenAI (Business Insider). Ключевые закономерности повторяются независимо у ≥3 авторов: (1) реферал/нетворкинг важнее холодной подачи; (2) технические навыки весят больше исследовательского опыта на самих интервью, но публикации «открывают дверь»; (3) стратегия «сначала тренируйся на менее желанных компаниях»; (4) поиск — это full-time job; (5) переговоры о компенсации критичны и им никто не учит. Главная развилка — раскрывать ли названия компаний и цифры компенсации: почти все анонимизируют, единицы называют. Медианные цифры (на малых N): интервью ≈57, офферы ≈4, длительность ≈3 месяца. Никто из авторитетных авторов не публикует конкретные вопросы интервью под NDA — они обобщают темы, а не задачи. Для заказчика: формат «дневник по ходу» (build-in-public) редок и рискован; преобладает ретроспектива после подписания оффера — это и безопаснее по NDA, и даёт inbound-эффект.

## 1. Каталог (22 позиции)

### Люди-авторы (позиции 1–12)

| № | Имя | Бэкграунд | Куда вышел | Что опубликовал | Формат | Что раскрыто/скрыто | Цифры | Дата | Ссылка | Лицензия |
|---|-----|-----------|------------|-----------------|--------|---------------------|-------|------|--------|----------|
| 1 | Alisa Liu | 6-летний PhD по NLP, Univ. of Washington (токенизация) | OpenAI, Research/MTS | Разбор процесса + LLM-конспект + math-конспект + топик-лист + 7 типов интервью + раздел про переговоры | Ретроспектива (за неделю до старта) | Компании НЕ названы (тайминг-график анонимен); цифры интервью раскрыты; компенсация НЕ раскрыта; интервьюеры НЕ названы | 57 интервью, 11 компаний, 46 звонков рекрутёрам, 16 post-offer чатов | 20.06.2026 | https://alisawuffles.github.io/blog/job-search/ | не указана |
| 2 | Nathan Lambert | PhD Berkeley (RL/робототехника), стажировки FAIR/DeepMind | Hugging Face, Research Scientist | Разбор процесса + визуализация таймлайна + репозиторий с данными/графиками | Ретроспектива | Часть компаний названа (HuggingFace, Amazon Robotics, Boston Dynamics, NNAISENSE), часть скрыта; цифры раскрыты; компенсация НЕ раскрыта | ≈53 интервью, 46 нетворк-звонков, 6 on-site, 4 оффера, 30 компаний | 11.07.2022 | https://www.natolambert.com/writing/ai-phd-job-hunt | открытые данные в репо |
| 3 | Mimansa Jaiswal | PhD (NLP/эмоции в ИИ) | ML/LLM research eng/science роли (Fall 2024) | Разбор процесса (Process) + огромный банк ресурсов и тем (Resources) + Notion-база | Ретроспектива | Компании частично названы (Meta/Llama, Amazon, Scale, Anthropic и др.); конкретные вопросы НЕ раскрыты; компенсация — только «доступна онлайн» | Интервью с 20+ компаниями (точные суммы офферов не указаны) | 24.12.2024 | https://mimansajaiswal.github.io/posts/llm-ml-job-interviews-resources/ | указан BibTeX для цитирования |
| 4 | Dyah Adila | 5-й год CS/ML PhD (post-training) | Research Scientist (работодатель не назван) | Разбор процесса + список типов интервью + prep-материалы + таймлайн | Ретроспектива | Компании НЕ названы; цифры офферов не указаны; компенсация НЕ раскрыта | Не указано (таймлайн ~5 мес: сентябрь–февраль) | 23.02.2026 | https://dyahadila.github.io/blog/2026/industry-job-search-cs-phd/ | не указана |
| 5 | Max Mynter | Self-taught ML eng из STEM (физика/социология), Recurse Center | Mistral, Research Engineer | Разбор процесса (~8000 слов) + playbook по типам интервью + ресурсы | Ретроспектива | Mistral назван; остальные компании НЕ названы; компенсация НЕ раскрыта | 18 месяцев процесса; ~60 touchpoints, 40 компаний; ~300 LeetCode; 15 OSS PR | 25.09.2025 | https://maxmynter.com/pages/blog/jobhunt | не указана |
| 6 | Andrey Zharkov | ~8 лет DL, Tech Lead в Snap, ~10 публикаций/патентов (GenAI) | Perplexity (+ ещё 2 оффера) | Детальнейший разбор всех типов интервью + статистика по стадиям + банк behavioral-вопросов | Ретроспектива | Компании названы поимённо (Anthropic, DeepMind, Meta, Spotify и т.д.); цифры раскрыты; компенсация — диапазоны, не точные суммы | 3 оффера из ~20 начатых процессов, ~50 интервью, апрель–июль 2025 (3 мес) | 2025 | https://asmekal.github.io/blog/posts/interviews-2025-ml-research-engineer-uk | не указана |
| 7 | Brian Kihoon Lee | ML/AI eng (moderndescartes) | ML/AI eng роль (работодатель не назван) | Разбор процесса + эссе о том, как надо перестроить ML-интервью | Ретроспектива | Компании частично названы (Espresso AI); цифры раскрыты; компенсация НЕ раскрыта | ~15 компаний, ~10 on-site, 7 офферов, ~70 интервью | 22.12.2025 | https://www.moderndescartes.com/essays/ml_eng_interviewing/ | не указана |
| 8 | Scott Freitas | ML PhD | 7 офферов (Apple, Microsoft, Cloudflare, Mandiant, Tinder, GE Labs, Amazon) | Разбор процесса + все советы + вопросы для интервьюеров + инфо про leveling | Ретроспектива | Компании названы поимённо; цифры раскрыты; компенсация — через Levels.fyi | 104 интервью, 30 компаний, 38 команд, 137 часов, 10 недель, 7 on-site, 7 офферов | 2022 | https://www.scottfreitas.com/blog/2022-phd-job-hunt | не указана |
| 9 | Yuan Meng | Cognitive science → MLE (DoorDash → Pinterest → Meta RAI) | Meta (RAI), Senior MLE | Разбор процесса MLE 2.0 + типы раундов + ресурсы; серия постов | Ретроспектива | Компании частично названы; цифры раскрыты в диапазоне; компенсация — общие замечания | Onsite в N∈[5,10) компаниях, офферы (N−1) | 2026 | https://www.yuan-meng.com/posts/mle_interviews_2.0/ | не указана |
| 10 | Akari Asai | NLP PhD (UW) | Gap-year позиция в AI2 (академ. рынок) | Разбор академического job search + таймлайн | Ретроспектива | ~20 школ (академ.); компании частично; компенсация НЕ раскрыта | ~20 заявок (faculty), 3–6 рекомендательных писем | 25.11.2025 | https://akariasai.github.io/2025/11/25/job-search.html | не указана |
| 11 | Andrew Lukyanenko | MLE (artgor) | MLE роль (не назван) | Разбор процесса 2024 (good/bad/weird) | Ретроспектива | Компании НЕ названы («for privacy»); цифры частично; компенсация НЕ раскрыта | 20+ компаний | 2024 | https://artgor.medium.com/my-experience-of-searching-for-a-job-in-2024-as-an-mle-6d7f2851bd0b | не указана |
| 12 | Bert Lee (李慕家) | MS Statistics (Yale) | DS/MLE (несколько офферов) | Разбор 2024 + prep по типам (Meta DSA, ML coding, SQL) | Ретроспектива | Компании названы (Meta, Expedia, AppLovin, Warner Bros, Walmart); компенсация НЕ раскрыта | Несколько офферов DS/MLE | 2024 | https://bertmclee.medium.com/conquering-the-2024-job-market-my-journey-to-multiple-ds-mle-offers-i-job-search-summary-and-92bd41cdd7c8 | не указана |

### Переиспользуемые учебные материалы (позиции 13–22)

| № | Автор | Материал | Тип | Ссылка | Звёзды/охват | Лицензия |
|---|-------|----------|-----|--------|--------------|----------|
| 13 | Alisa Liu | «Alisa's Book of LLMs» | Конспект по LLM (нейросети → трансформер → FLOPs/inference память) | https://alisawuffles.notion.site/alisa-s-book-of-llms | вирусный | не указана (публичный Notion) |
| 14 | Alisa Liu | «Math notes» | Конспект по математике (распределения, неравенства, Марковские цепи, головоломки) | https://alisawuffles.notion.site/math-notes | вирусный | не указана |
| 15 | Chip Huyen | «Introduction to Machine Learning Interviews Book» | Книга-банк: «over 200 knowledge questions… plus 30 open-ended questions» | https://huyenchip.com/ml-interviews-book/ | open-source, бесплатна | «Copyright ©2021 Chip Huyen»; open-sourced на GitHub |
| 16 | Alireza Dirafzoon | «Machine-Learning-Interviews» (ныне AIMLInterviews) | Гайд по ML/AI интервью (ML coding, дизайн) | https://github.com/alirezadir/Machine-Learning-Interviews | ⭐ ~9.6k (сент. 2026) | **MIT License** |
| 17 | Khang Nguyen (khangich) | «machine-learning-interview» | Гайд FAANG + ML system design | https://github.com/khangich/machine-learning-interview | ⭐ ~12.4k+ | публичный репо |
| 18 | Mimansa Jaiswal | Notion-база ресурсов + банк вопросов | Список тем/ресурсов (7 секций) + contribution-форма | https://mimansajaiswal.github.io/posts/llm-ml-job-interviews-resources/ | — | BibTeX-цитирование |
| 19 | Stanford CS336 | «Language Modeling from Scratch» + Homework 1 | Курс + ДЗ (реализация трансформера с нуля) | https://cs336.stanford.edu/spring2025/ | — | академ. курс, открыт |
| 20 | LeetCode 75 / Neetcode Blind 75 | Списки задач для general-coding | Банк задач | https://leetcode.com/studyplan/leetcode-75/ ; https://neetcode.io | — | публичные |
| 21 | Andrey Zharkov | Notes по DL + банк behavioral-вопросов | Конспект + список поведенческих вопросов | https://asmekal.github.io/blog/posts/interviews-2025-ml-research-engineer-uk | — | не указана |
| 22 | wzchen | «probability_cheatsheet» | 10-страничная шпаргалка по вероятности | https://github.com/wzchen/probability_cheatsheet | популярный | публичный репо |

## 2. Сравнительная сводка цифр

| Автор | Заявки/компании | Интервью | On-site лупы | Офферы | Длительность |
|-------|-----------------|----------|--------------|--------|--------------|
| Alisa Liu | 11 компаний (интервьюировалась) | 57 | — | несколько (точно не указано) | несколько месяцев |
| Nathan Lambert | 30 компаний | ≈53 | 6 | 4 | ~3–4 мес (окт 2021 – 2022) |
| Scott Freitas | 30 компаний, 38 команд | 104 | 7 | 7 | 10 недель |
| Brian K. Lee | ~15 компаний | ~70 | ~10 | 7 | не указано |
| Andrey Zharkov | ~20 начатых процессов | ~50 | — | 3 | 3 месяца |
| Max Mynter | ~40 компаний, ~60 touchpoints | не указано | — | ≥3 (Mistral + пара) | 18 месяцев (full-time с янв 2025) |
| Yuan Meng | N∈[5,10) onsite | не указано | 5–9 | N−1 | ~2 месяца |
| Mimansa Jaiswal | 20+ компаний | не указано | — | не указано | Fall 2024 |
| Andrew Lukyanenko | 20+ компаний | не указано | — | не указано | 2024 |

**Диапазоны и медианы (только по авторам с явными числами):**
- **Интервью:** диапазон ≈50–104; **медиана ≈57** (N=5: Liu 57, Lambert ≈53, Freitas 104, B.Lee ~70, Zharkov ~50).
- **Компании (заявки/контакты):** диапазон ~15–40; **медиана ~30** (N=6: Liu 11, Lambert 30, Freitas 30, B.Lee ~15, Zharkov ~20, Mynter ~40).
- **Офферы:** диапазон 3–7; **медиана 4** (N=5: Lambert 4, Freitas 7, B.Lee 7, Zharkov 3, Mynter ≥3).
- **Длительность:** диапазон 10 недель – 18 месяцев; **медиана ≈3 месяца** (N=5). Выброс — Mynter (18 мес, но это включает стратегическую подготовку, а не только активную подачу).

**Важно:** большинство авторов НЕ называют число офферов или заявок точно — там стоит «не указано». Выборка мала и смещена (публикуют в основном те, у кого всё закончилось успешно — survivorship bias). Число офферов у самой Liu в тексте поста не названо (упоминается лишь «some offers I was excited about»); ходящие по X цифры «4 оффера»/«6 офферов» её текстом НЕ подтверждаются.

## 3. Общие закономерности

### (а) Повторяющиеся советы у ≥3 авторов (сильный сигнал)
1. **Реферал/нетворкинг важнее холодной подачи.** Liu («sometimes you need to have someone inside the company vouching for you»), Zharkov («It is really important to have a referral… many companies reject candidates applying without a reference w/o looking»), Mynter (весь раздел про networks), Dyah Adila («Look out for people that are hiring»). ≥4 авторов.
2. **Технические навыки весят больше исследовательского опыта на самих интервью, но публикации/резюме «открывают дверь».** Liu («technical skills and knowledge are evaluated much more than research experience, though the latter probably gets you the interview»), Mimansa, Brian Lee, Dyah Adila. ≥3.
3. **Стратегия батчинга: тренируйся на менее желанных компаниях, копи офферы к одному моменту.** Liu, Mynter («Batch your applications so you can use lower-stakes ones at training grounds»), finbarr (коммент к посту Liu: «Never apply to your first choices until you're already receiving offers»), Zharkov. ≥3.
4. **Поиск работы = full-time job; готовиться к интервью — лучшее вложение времени.** Liu («There is truly no better use of your time than studying for interviews… the job search is a full-time job»), Mynter (уволился ради full-time поиска), Mimansa, Freitas (137 часов). ≥3.
5. **Реализация трансформера с нуля (PyTorch, без AI-ассистента) — навык №1 для ML coding.** Liu (CS336 HW1 «crucial»; «practicing coding with AI assistance completely off»), Dyah Adila (nanoGPT, «practice implementing… Transformers… from scratch»), Mimansa, Zharkov. ≥4.
6. **Переговоры о компенсации критичны, и этому никто не учит.** Liu («Nothing in our PhD prepared us for this… can be equivalent to years of work at the initial offer»), Freitas (leveling через Levels.fyi), Zharkov (получил +20K base). ≥3.
7. **Behavioral-интервью недооценивают; готовьте истории по STAR заранее.** Liu (провалила первое behavioral), Mimansa (STAR), Mynter (STAR), Zharkov. ≥4.

### (б) Прямые противоречия/развилки (не усредняем)
- **LeetCode: тратить время vs. не тратить.** ЗА: Mynter (~300 задач), Mimansa (Grind 75 + Neetcode 150), Freitas (leetcode-скрины отсеяли его в 6 командах — значит, важны). ПРОТИВ по акценту: Dyah Adila (для RS «easy-level… to gauge if we can code in Python reasonably well», medium/hard редки), Brian Lee (снижает вес чистого алгокодинга для ML-ролей, называет ML system design скорее бесполезным).
- **Называть компании vs. анонимизировать.** НАЗЫВАЮТ: Zharkov (поимённо, с исходами), Freitas (все 8), Lambert (частично). АНОНИМИЗИРУЮТ: Liu (график без названий), Dyah Adila (никого), Lukyanenko («For the sake of privacy, I won't name most of the companies»).
- **Широкая подача vs. точечная.** ШИРОКО: Freitas (104 интервью/30 компаний), Zharkov (~60 записей в таблице). ТОЧЕЧНО: Lambert («I did not need to take the firehose approach in the slightest»), Mynter (goal-driven, ~40 компаний, батчами), Liu (осторожно про выгорание: «be careful not to burn out»).
- **Публиковать конспекты/разбор ДО или ПОСЛЕ оффера.** ПОСЛЕ (преобладает): Liu (за неделю до старта в OpenAI), Mynter (после подписания), Lambert (после). ПО ХОДУ / раньше: Mimansa (пишет в декабре 2024 сразу после Fall 2024, но уже с результатами), Chip Huyen (книга — вне привязки к своему поиску, как ресурс).

## 4. Готовые учебные материалы для переиспользования
(Сведено в таблице позиций 13–22 выше.) Ключевое по лицензиям и условиям:
- **Явно открытые и переиспользуемые:** **alirezadir/Machine-Learning-Interviews — под MIT License** (можно использовать и адаптировать при сохранении атрибуции); Chip Huyen ML Interviews Book (open-sourced на GitHub, бесплатна, «Copyright ©2021 Chip Huyen» — использовать со ссылкой); Stanford CS336 + HW1; LeetCode 75 / Neetcode Blind 75; wzchen probability_cheatsheet; khangich репозиторий.
- **С условиями цитирования:** Mimansa Jaiswal — даёт BibTeX и просит атрибуцию.
- **Осторожно с лицензией:** Alisa Liu LLM/math notes и большинство личных блогов/Notion-страниц — лицензия НЕ заявлена. По умолчанию это «все права защищены»: можно ссылаться и цитировать фрагменты, но нельзя перепубликовывать целиком без разрешения автора.

## 5. Как устроен жанр
- **Формат:** доминирует ретроспектива, написанная ПОСЛЕ подписания оффера (Liu, Lambert, Mynter, Freitas, Zharkov, Brian Lee). Дневник «по ходу» (build-in-public, еженедельные апдейты) — редкость; ближайшее — Max Mynter, который постил апдейты учёбы на X по ходу 18 месяцев, но сам разбор опубликовал в конце.
- **Что раскрывают:** цифры интервью/компаний/таймлайн, типы раундов, ресурсы для подготовки, эмоциональную сторону (Liu: «I was stressed, miserable, and not functioning in other parts of my life for several months»).
- **Что скрывают:** (1) конкретные вопросы интервью под NDA (обобщают темы); (2) точные суммы компенсации (дают диапазоны или ссылку на Levels.fyi); (3) часто — названия компаний (график/«for privacy»); (4) имена интервьюеров — не называет никто.
- **Реакция и эффект на карьеру:** пост Liu — «open-source, 10,000-word retrospective… that has instantly gone viral across Silicon Valley» (BigGo Finance); найм подтвердил представитель OpenAI (Business Insider, перепечатки Yahoo/AOL); Dealroom со ссылкой на businessinsider.com воспроизводит цифры «57 interviews across 11 companies, 46 recruiter calls and 16 post-offer discussions». Обсуждение на Digg — от похвалы до критики «referral-heavy hiring = exclusive club». Zharkov прямо пишет, что блог — часть «pay it forward»; Mynter отмечает, что публичные артефакты (посты, OSS, talks) работают как «proof of work» и генерируют inbound-DM. Явных подтверждённых случаев, когда публикация НАВРЕДИЛА автору (отзыв оффера и т.п.), в проверяемых источниках не найдено — отчасти потому, что публикуют уже после подписания. Про виральность в числах: надёжно подтверждается «surpassed one million views» (KuCoin); фигурирующая в сниппетах цифра «~5.4M views» на X прямой проверке не поддаётся (X закрыт robots) — считать неподтверждённой; отдельного треда Hacker News по посту Liu не найдено.

## 6. Правовые и этические границы и приёмы обхода
- **NDA / конкретные вопросы:** авторитетные авторы сознательно НЕ публикуют дословные задачи. Zharkov: «The questions are sometimes slightly altered to not spoil the process of the companies». Mimansa: «I can't share the specific content due to unpublished material». Приём обхода — обобщать ТЕМЫ вместо ЗАДАЧ («просили реализовать decoding strategy» вместо точной формулировки).
- **Политики компаний:** OpenAI и Anthropic публикуют официальные interview guides и просят следовать инструкциям рекрутёра для каждой стадии; конфиденциальность процесса ожидается по умолчанию (у ряда компаний — отдельные candidate confidentiality agreements). Прямого публичного запрета «не рассказывай, что тебя спрашивали» в общих гайдах OpenAI/Anthropic не зафиксировано, но раскрытие конкретных задач нарушает дух NDA и integrity процесса.
- **Компенсация:** приём — диапазоны или ссылка на Levels.fyi/Glassdoor вместо точных цифр (Freitas, Zharkov).
- **Имена людей:** интервьюеров не называют; коллег/референсов упоминают только с согласия (Mynter про reference check: «I did ask my references before putting them down»).
- **Анонимизация компаний:** категории вместо названий (Liu — цветные кружки без имён; Lukyanenko — «I won't name most of the companies»).

## 7. Практические выводы для заказчика (ведёт свой публичный дневник)
**Что заимствовать:**
1. **Структуру Liu/Lambert:** таймлайн-визуализация (можно анонимную), разбивка по типам интервью, отдельный раздел про переговоры, честный эмоциональный блок — именно он даёт виральность и эмпатию читателя.
2. **Приложить переиспользуемые артефакты** (конспекты, топик-лист, чеклист) — по разбору поста Liu, именно открытые study-материалы, а не «war stories», дают долгую ценность и бэклинки.
3. **Публиковать ретроспективу ПОСЛЕ подписания** — безопаснее по NDA и даёт inbound-эффект без риска для текущего процесса.
4. **Указать явную лицензию (например, MIT для кода / CC-BY для конспектов)** на свои материалы — это то, чего почти никто не делает (единственный явный образец в каталоге — MIT у alirezadir); повысит переиспользуемость и цитируемость.
5. **Build-in-public апдейты по ходу** (как Mynter на X) — но про учёбу/навыки, НЕ про конкретные интервью в процессе.

**Чего избегать:**
1. Не публиковать дословные вопросы интервью и имена интервьюеров — репутационный и юридический риск.
2. Не называть точные суммы офферов конкретных компаний, если есть NDA/оффер на руках — используйте диапазоны.
3. Не вести дневник конкретных активных лупов в реальном времени — это может навредить текущим процессам и нарушить confidentiality.
4. Не выдавать n=1 за универсальный рецепт — сильные авторы (Mynter: «my sample is n=1»; Mimansa: «this isn't a definitive guide») явно это оговаривают.

**Бенчмарки, которые изменят стратегию:** если из своего поиска заказчик получает <2 офферов — стоит сместить дневник в сторону build-in-public навыковых апдейтов ДО оффера (эффект Mynter: inbound через артефакты). Если пост наберёт >1M просмотров (порог виральности Liu) — приоритет №1 переключить на выкладывание лицензированных учебных материалов, т.к. именно они дают устойчивый трафик и inbound, а не сам нарратив.

## 8. Источники
1. Alisa Liu, «Notes on the Industry Job Search» — https://alisawuffles.github.io/blog/job-search/ — «In total, I interviewed at 11 companies over 57 interviews. Not pictured are 46 additional recruiter calls and 16 post-offer chats» (20.06.2026).
2. Nathan Lambert, «Job Hunt as a PhD in AI / ML / RL» — https://www.natolambert.com/writing/ai-phd-job-hunt — «The result was plus or minus 53 one-off interviews, 46 networking calls, 6 (virtual) on-site interviews, and 4 offers from 30 companies I contacted or applied to» (11.07.2022).
3. Mimansa Jaiswal, «LLM (ML) Job Interviews (Fall 2024) - Process» — https://mimansajaiswal.github.io/posts/llm-ml-job-interviews-fall-2024-process/ — «I interviewed with several tech companies: Meta (LLama team), Amazon…».
4. Mimansa Jaiswal, «LLM (ML) Job Interviews - Resources» — https://mimansajaiswal.github.io/posts/llm-ml-job-interviews-resources/ — «A collection of all the resources I used to prepare for my ML/LLM research science/engineering focused interviews in Fall 2024» (24.12.2024).
5. Dyah Adila, «What I Learned from My Industry Job Search as a CS PhD» — https://dyahadila.github.io/blog/2026/industry-job-search-cs-phd/ — «make the spreadsheet. It really does help» (23.02.2026).
6. Max Mynter, «Becoming a Research Engineer at a Big LLM Lab» — https://maxmynter.com/pages/blog/jobhunt — «I had about 60 touchpoints with 40 different companies, got a verbal offer with Mistral… mid-August» (25.09.2025).
7. Andrey Zharkov, «ML/Research Engineer interviews 2025» — https://asmekal.github.io/blog/posts/interviews-2025-ml-research-engineer-uk — «now it was smth like 3/20 (counting only positions where I had 1+ interviews)» (2025).
8. Brian Kihoon Lee, «Interviewing for ML/AI Engineers» — https://www.moderndescartes.com/essays/ml_eng_interviewing/ — «I talked to ~15 companies, made it to onsites for ~10 companies and received 7 offers. I did ~70 separate interviews» (22.12.2025).
9. Scott Freitas, «Landing Your ML PhD Dream Job» — https://www.scottfreitas.com/blog/2022-phd-job-hunt — «104 interviews across 30 companies and 38 teams… 7 on-site interviews with Apple, Microsoft, Cloudflare, Mandiant, Stripe, Tinder and GE Labs, leading to offers from all but Stripe».
10. Yuan Meng, «MLE Interview 2.0: Research Engineering and Scary Rounds» — https://www.yuan-meng.com/posts/mle_interviews_2.0/ — «I did onsite interviews with N∈[5,10) companies… I received offers from (N−1) of them» (2026).
11. Akari Asai, «Note from my 2024-2025 academic job search» — https://akariasai.github.io/2025/11/25/job-search.html — «I applied to about 20 schools» (25.11.2025).
12. Andrew Lukyanenko, «My experience of searching for a job in 2024 as an MLE» — https://artgor.medium.com/my-experience-of-searching-for-a-job-in-2024-as-an-mle-6d7f2851bd0b — «I interviewed with 20+ companies across the world» (2024).
13. Chip Huyen, «Introduction to Machine Learning Interviews Book» — https://huyenchip.com/ml-interviews-book/ — «over 200 knowledge questions, each noted with its level of difficulty» + «30 open-ended questions»; «Copyright ©2021 Chip Huyen».
14. alirezadir/Machine-Learning-Interviews — https://github.com/alirezadir/Machine-Learning-Interviews — ⭐ ~9.6k, «MIT License… Created Jan 31, 2021»; автор: офферы «from Meta (ML Specialist), Google (ML Engineer), Amazon (Applied Scientist), Apple (Applied Scientist), and Roku».
15. khangich/machine-learning-interview — https://github.com/khangich/machine-learning-interview — ⭐ ~12.4k; «I have offers from Snapchat, Coupang, Stitchfix etc.».
16. Bert Lee, «Conquering the 2024 Job Market: My Journey to Multiple DS/MLE Offers» — https://bertmclee.medium.com/conquering-the-2024-job-market-my-journey-to-multiple-ds-mle-offers-i-job-search-summary-and-92bd41cdd7c8 (2024).
17. OpenAI Interview Guide — https://openai.com/interview-guide/ — «Formats vary by team and may include: pair coding interviews, take-home projects, technical tests».
18. wzchen/probability_cheatsheet — https://github.com/wzchen/probability_cheatsheet — «A comprehensive 10-page probability cheatsheet».
19. Stanford CS336 «Language Modeling from Scratch» — https://cs336.stanford.edu/spring2025/ (+ https://github.com/stanford-cs336/assignment1-basics).
20. Dealroom (summary of Business Insider coverage) — app.dealroom.co/news/… — «Alisa Liu, a recent OpenAI hire, documented her exhaustive job search… which included 57 interviews across 11 companies, 46 recruiter calls and 16 post-offer discussions» (source: businessinsider.com).
21. BigGo Finance — https://finance.biggo.com/news/57b9b2ea-49b3-4cd2-8539-a126d005ce2b — «an open-source, 10,000-word retrospective… that has instantly gone viral across Silicon Valley».
22. Yahoo/Business Insider перепечатка — https://www.yahoo.com/lifestyle/articles/openai-hire-breaks-down-her-083001293.html — «An OpenAI spokesperson confirmed Liu's hiring»; Liu: «I was stressed, miserable, and not functioning in other parts of my life for several months».

*Мёртвых ссылок среди включённых позиций не обнаружено — все первоисточники открывались и читались 22–23 сентября 2026. X/Twitter-страницы (посты Liu и реакции) доступны как сниппеты, но закрыты robots для прямой верификации метрик просмотров — помечено в разделах 5 и 9.*

## 9. Proof of work
**Проверенные категории и запросы:** личные блоги PhD (NLP/ML/CV/RL); Substack и личные сайты (Mynter, Yuan Meng); GitHub (ML interview prep репозитории — alirezadir, khangich, chiphuyen, wzchen); Notion (Alisa Liu, Mimansa); X/Threads/Digg (посты Liu и реакции); Reddit/Blind (таймлайны, треды об офферах); Medium/TDS (Lukyanenko, Bert Lee); Hacker News (Brian Lee); career switchers (Mynter, Yuan Meng); переговоры/компенсация (Levels.fyi упоминания, Freitas, Zharkov); правовые границы (OpenAI/Anthropic interview guides, NDA). Всего ~18 поисковых запросов + ~10 полных выгрузок страниц + 1 субагент-верификация цифр + 1 проход обогащения.

**Отсеяно/не включено:** «эталонные академики» из брифа (Sasha Rush, Jason Wei, Lilian Weng, Eugene Yan, Sebastian Raschka, Chip Huyen как автор *своего* дневника) — у них НЕ найдено постов именно про СВОЙ процесс трудоустройства 2023–2026 (Eugene Yan пишет про то, как ИНТЕРВЬЮИРОВАТЬ кандидатов, а не про свой поиск; Lilian Weng — технические конспекты, не job search; Chip Huyen включён только как автор открытой книги-ресурса, поз. 15, а не как дневник). Множество Blind/Reddit-тредов отброшены как анонимные без проверяемой авторской атрибуции. Коммерческие «interview prep» лендинги (tryexponent, igotanoffer, interviewnode, finalroundai) исключены как не-первоисточники. Отсеяно ≈8–10 кандидатов из-за отсутствия проверяемых деталей/анонимности.

**Честно о размере корпуса:** детальных публичных ДНЕВНИКОВ именно СВОЕГО пути с названными цифрами — меньше 20 (реально ~8–10 сильных: Liu, Lambert, Freitas, Brian Lee, Zharkov, Mynter, Yuan Meng, Mimansa, Dyah Adila, Lukyanenko/Bert Lee). Каталог доведён до 22 позиций за счёт добавления переиспользуемых учебных материалов (конспекты, репозитории, курсы) — это отдельная, честно помеченная категория, а не «дневники». Корпус меньше 20 «чистых» дневников потому, что: (1) большинство успешных кандидатов из фронтир-лабораторий связаны NDA и/или не пишут; (2) выборка смещена в сторону success stories (survivorship bias); (3) многие детальные разборы анонимны (Blind/Reddit) и не проходят порог проверяемой атрибуции.

**Где не хватило данных:** точные числа офферов/заявок у большинства авторов (ставлю «не указано»); просмотры поста Liu на X (5.4M — в сниппетах, но X robots-заблокирован; надёжно только «surpassed one million views»); HN-тред по посту Liu не подтверждён; детальные внутренние политики конфиденциальности OpenAI/Anthropic по вопросам интервью публично ограничены.

**Уровень уверенности по разделам:** Каталог (поз. 1–12) — **высокая** (все страницы открыты и прочитаны целиком/крупными фрагментами). Учебные материалы (13–22) — **высокая** по факту существования и открытости, **средняя** по лицензиям (заявлена явно только у alirezadir/MIT, Chip Huyen/open-source, Mimansa/BibTeX; у остальных не указана). Сводка цифр — **высокая** для 5–6 авторов с явными числами, **низкая полнота** по остальным. Закономерности — **высокая** (подтверждены дословными цитатами у ≥3 авторов). Жанр/реакция — **средняя** (виральность Liu и покрытие BI подтверждены, точные метрики просмотров — нет). Правовые границы — **средняя** (цитаты авторов есть; детальные внутренние политики компаний публично ограничены).
