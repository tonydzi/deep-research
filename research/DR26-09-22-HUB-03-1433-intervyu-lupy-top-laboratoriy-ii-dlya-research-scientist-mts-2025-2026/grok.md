# DR26-09-22-HUB-03-1433 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Соберу карту по живым отчётам 2025–2026 и официальным страницам лабораторий. Начинаю с первоисточников и разборов лупов.## A. Таблица лупов по компаниям

Как читать таблицу. «Официально» значит страница компании, открытая в этом прогоне. «Кандидат» значит именной пост или карточка Glassdoor / InterviewQuery / Reddit, текст которой получен в этом прогоне. «Агрегатор» (techinterview.org, datainterview, dataford, jobsbyculture) в таблицу как факт не ставится: эти сайты пересказывают друг друга без именных отчётов. Где источники спорят, стоят оба числа. Где лупа нет, стоит «не нашёл».

| Компания | Роли, как они сами их называют | Последовательность, которую удалось подтвердить | Длительность этапов | Весь луп |
|---|---|---|---|---|
| OpenAI | Research Scientist и Member of Technical Staff у кандидата. Официально гайд общий для ролей, не отдельный для RS | Резюме, вводный звонок (рекрутер или hiring manager), skills assessment (формат команды: pair coding, take-home, technical test, может быть больше одного), финал 4–6 человек за 1–2 дня, решение, иногда референсы | Ревью резюме «обычно неделя». После assessment «услышишь в течение недели». Решение «в течение недели» после финала. Финал сам: 4–6 часов | Сквозных недель официальный гайд не даёт. Личный поиск Alisa Liu (11 компаний, не один луп) занял «несколько месяцев» |
| Anthropic | MTS как плоский титул. Отдельного опубликованного лупа RS не нашёл. Есть отчёты SWE / infra и одного research intern | Официальной лестницы этапов на сайте нет. Кандидат infra SWE, декабрь 2025 / февраль 2026: OA, два кодинга, system design, hiring manager. Research intern, август 2025: автотест, затем звонок с MTS. Перед разговором с сотрудниками вне рекрутинга кандидатов просят подписать NDA (пересказ Bloomberg на Reddit, сам Bloomberg не открывал) | Тот же SWE-отчёт: «five rounds, three weeks». Длительность каждого круга в официальном источнике не опубликована | Для RS/MTS research «не нашёл» сквозной цифры. SWE-самоотчёт: около 3 недель на пять кругов |
| Google DeepMind | Research Scientist, Research Engineer, Software Engineer. Точные шаги «чуть разные для каждой роли» | Официально 4 стадии: (1) рекрутер 30 мин, иногда ещё hiring manager, (2) skills, два или три звонка с возможными коллегами, (3) финал с team lead и руководством, (4) решение и оффер. NDA через Ironclad до содержательных разговоров | Рекрутер: 30 минут. Остальные круги в официальном тексте без минут. Отзыв: «если нет фидбэка дольше недели, это не плохая новость» | Официально 4–10 недель, «может быть дольше из-за доступности интервьюеров». Карточка Glassdoor RS: 10 кругов плюс take-home на 3 часа, около 5 месяцев от заявки до оффера. Другая карточка: два research и два coding. Третья: один coding и один ML по часу. Не усредняю |
| Meta FAIR / Superintelligence Labs | В кандидатских отчётах титул «AI Research Scientist». Prakhar Agarwal называет себя applied researcher в Meta Superintelligence Labs. Отдельного опубликованного лупа «FAIR vs MSL» не нашёл | InterviewQuery, 7 отчётов: кодинг, research deep dive, ML reasoning, behavioral, финал часто удалённо. Один отчёт Q2 2026: презентация своего исследования около 40 минут, сразу вопросы. Один отчёт Q1 2025: «стандартный Meta loop, но с более тяжёлым кодингом» | Минуты кругов в открытом тексте не зафиксированы, кроме ~40 минут презентации | Сквозных недель в открытом тексте InterviewQuery нет. «Не нашёл» официальной цифры |
| xAI | Member of Technical Staff. В открытой вакансии Reasoning (Alignment) отдельного титула RS нет | CV и statement of exceptional work, телефон 15 минут, затем четыре технических: (1) кодинг на языке кандидата, (2–3) две сессии «formulate, design and solve concrete problems in real world with LLM», (4) Meet the Team: прошлое «exceptional work» и vision | Телефон: 15 минут. Минуты четырёх технических в вакансии не указаны | Цель компании: основной процесс (четыре интервью) закончить за одну неделю. Все через Google Meet. Это цель одной вакансии, не замер по всем MTS |
| Mistral | Research Scientist в агрегаторах | Первичный отчёт кандидата целиком не открыл | не нашёл | не нашёл подтверждённой цифры. Агрегаторы одновременно пишут «~15 дней официально», «3–5 недель» для Research Scientist и «7 недель в среднем» ожидания после onsite у Researcher. Это внутреннее противоречие агрегаторов, в таблицу как факт не беру |
| Cohere | Member of Technical Staff. В вакансии Post-Training граница research/engineering стёрта | Карточки Glassdoor, не официальный гайд. Ноябрь 2025, реферал, 4 недели: рекрутер, hiring manager, четыре технических (coding, ML design, leadership, paper presentation). Октябрь 2025: скрин с team lead, затем 4 технических. Сентябрь 2026: hiring manager, затем 3 круга technical skills и research talk. Ещё одна карточка: рекрутер, deep dive проекта с HM, AI system design, debugging, «Coding with AI assistant» | «Процесс занял 4 недели» в карточке ноября 2025. Минуты кругов не указаны | 4 недели в одном отчёте. В других карточках срок не назван, есть жалобы на паузы между HR |
| Scale AI | Для RS отдельного лупа не открыл | не нашёл первичного | не нашёл | не нашёл |
| Perplexity | Research Engineer упоминается в агрегаторе сроков. Открытого самоотчёта RS нет | Reddit-мегатрэд Hack2Hire (текст получен поиском, страница целиком не открылась): телефон/кодинг ~45 мин, Python, многочастный вопрос с отдельными тест-файлами. Onsite 3–4 круга: infra, system design, кодинг, менеджер. Рекрутер иногда задаёт технический вопрос до кодинга | Телефон около 45 минут в этом мегатрэде | Агрегатор leonstaff (страницу целиком не открывал) пишет 11–23 дня в среднем и «Research Engineer около 3 дней». Это не подтверждаю как факт: первоисточник агрегатора не открыт, цифра аномально короткая рядом с мегатрэдом |
| Reflection | MTS, Research Software Engineer, MTS Post-Training | Лупа не опубликован. Открыта вакансия, не интервью | не нашёл | не нашёл |
| Thinking Machines | Вакансии есть (в том числе research recruiting). Лупа нет | не нашёл | не нашёл | не нашёл. Оценка «19 дней» на Extern помечена самим сайтом как оценка, первички под ней не открыл |
| SSI | Публичного описания интервью не нашёл. Доски вакансий в этом прогоне расходятся (0 ролей против чужих инженерных вакансий, похожих на другую компанию) | не нашёл | не нашёл | не нашёл |
| Nvidia | AI Research Scientist в InterviewQuery | Один кандидат Q2 2026: два сильно технических круга, оба про свою прошлую работу, реализацию и код. Презентация около 45 минут внутри сессии около часа | Около часа на сессию с презентацией | Число недель «не нашёл». Позже страница InterviewQuery отдала только тизер за пэйволлом, длинный текст был в выдаче поиска |
| Apple | Machine Learning Researcher / AI Research Scientist | Один отчёт Q1 2026: два скрина (hiring manager и исследователь), затем двухдневный onsite: research talk 45 минут и пять или шесть интервью с исследователями команды | Talk 45 минут. Остальные круги в тексте без минут | InterviewQuery на другом URL пишет «7–8 rounds» и «2–4 weeks» по двум отчётам. Это сводка площадки, не протокол Apple |
| Amazon | Applied Scientist отдельно от Research Scientist и Data Scientist | Официально: заявка, technical phone screen, исход, interview loop, исход в течение 5 рабочих дней, разговор об оффере. Темы лупа названы: coding, knowledge depth and breadth, tech talk, Leadership Principles, Bar Raiser. IGotAnOffer (сводка 200+ Glassdoor, обновлено 4 июня 2026): 1–2 телефона по 60 минут, затем 5–6 кругов по 45–60 минут. Tech talk не у всех команд | Телефон 60 минут. Круг лупа 45–60 минут. Tech talk, если есть, около часа. Исход лупа: обещание Amazon «within 5 business days» | IGotAnOffer пишет «typically two to three weeks». Это сводка, не страница Amazon. Официальная страница минут сквозного лупа не даёт |
| Microsoft | Research Scientist отдельно в официальном «How we hire» не расписан. Общий процесс | Заявка, ревью, иногда короткий скрин, интервью. «Most interviews include 2–4 conversations… each lasting up to an hour». Для части ролей просят писать код или принести work sample. Рекрутер после интервью называет срок следующего шага | До часа на разговор. Число разговоров: чаще 2–4 | Сквозных недель на странице нет. Для RS «не нашёл» отдельной лестницы |

Источники таблицы: https://openai.com/interview-guide/ , https://alisawuffles.github.io/blog/job-search/ , https://deepmind.google/careers/ , https://storage.googleapis.com/deepmind-media/DeepMind.com/Assets/Docs/interviewing-at-google-deepmind.pdf , https://www.anthropic.com/candidate-ai-guidance , https://startup.jobs/member-of-technical-staff-reasoning-alignment-xai-7182794 , https://startup.jobs/member-of-technical-staff-post-training-cohere-2-7167861 , https://amazon.jobs/content/en/how-we-hire/applied-scientist-interview-prep , https://igotanoffer.com/en/advice/amazon-applied-scientist-interview , https://careers.microsoft.com/v2/global/en/hiring-tips , https://www.wired.com/story/meta-ai-job-interview-coding/ , https://www.businessinsider.com/openai-meta-superintelligence-labs-tips-getting-hired-phd-llm-interview-2025-10 , https://www.linkedin.com/jobs/view/member-of-technical-staff-research-software-engineer-at-reflection-ai-4383253252 .

Team match как отдельный этап после оффера официально назван только косвенно. Alisa Liu пишет, что после офферов идут разговоры с будущими тиммейтами и менеджерами, обеды и звонки рекрутеров, и это может тянуться неделями. OpenAI в гайде пишет решение в течение недели и возможные референсы, слово «team match» там нет. DeepMind в четырёх стадиях сразу ведёт к команде роли: финал уже с потенциальным менеджером. Amazon интервьюирует в команду, отдельного team match после комитета в открытых страницах нет.

## B. Форматы этапов, с примерами вопросов

Ниже только то, что сказано живым текстом. Минуты ставлю только там, где они названы.

**ML-кодинг с нуля.** Alisa Liu, июнь 2026, 57 интервью на RS/MTS в 11 компаниях: «This was by far the most common. These questions may ask you to implement a given architecture, a decoding strategy, a traditional ML algorithm, or sometimes way more creative things. Being fluent in PyTorch is a must; in rare cases I was asked to use only numpy, for instance when writing the backwards pass from scratch». Она же: домашка Stanford CS336, реализация и отладка трансформера, «comes up so often in interviews», и тренироваться надо «with AI assistance completely off». Источник: https://alisawuffles.github.io/blog/job-search/

Кандидат на Reddit, февраль 2026, скрин OpenAI (до onsite не дошёл): первый круг был ML-кодинг, forward и backward серии операций в PyTorch, четыре части, успел три. Второй круг: systems coding, concurrency и OS, три части, успел две. Источник, текст получен поиском, полное открытие страницы не удалось: https://www.reddit.com/r/LLM/comments/1r93ip9/my_reflection_on_interviewing_with_anthropic_and

Агрегатор techinterview.org (открыт, без именного кандидата) пишет, что на DeepMind ML-кодинг «typically a custom loss, an attention mechanism, a sampling routine, or a small training loop» и «almost always unaided». Официальный PDF DeepMind эти задачи не перечисляет. Расхождение оставляю как есть: https://www.techinterview.org/post/3233474918/deepmind-interview-process-2026/ и https://storage.googleapis.com/deepmind-media/DeepMind.com/Assets/Docs/interviewing-at-google-deepmind.pdf

**Классический алгоритмический кодинг.** Alisa: «Basically LeetCode, sometimes with some extra flavor. It’s good to build strong foundations here because the concepts often show up in ML coding interviews, too». Тот же URL блога.

InterviewQuery по Meta AI Research Scientist (текст выдачи, повторное открытие страницы не удалось): несколько кандидатов описывают LeetCode-style, на скрине два вопроса easy-to-medium или medium, на onsite medium-to-hard. Один кандидат Q1 2025: луп «felt like a pretty standard Meta loop… with a heavier coding». https://www.interviewquery.com/interview-guides/facebook-research-scientist

Amazon, сводка IGotAnOffer по Glassdoor: на телефоне «one or two LeetCode-style problems, usually at the medium level», «syntactically correct code… without the aid of an IDE». Официальная страница Amazon кодинг называет темой лупа и даёт образец «recursive staircase», текст задачи на странице не раскрыт. https://igotanoffer.com/en/advice/amazon-applied-scientist-interview и https://amazon.jobs/content/en/how-we-hire/applied-scientist-interview-prep

xAI, вакансия: «Coding assessment in a language of your choice». Каких задач, не написано. https://startup.jobs/member-of-technical-staff-reasoning-alignment-xai-7182794

Anthropic, research intern, Glassdoor, 8 августа 2025: «they didn't focus on algorithm leet code style but trying to evaluate general coding capabilities… be fast in coding and refactoring». Вопрос в карточке: «Code a business app in python». https://www.glassdoor.com/Interview/Anthropic-Interview-E8109027-RVW99236753.htm

Anthropic infra SWE, Reddit, текст получен поиском: OA 90 минут, две задачи. Кодинг 1: веб-краулер, BFS, глубина, sitemap, rate limit, dedup, robots.txt, затем сразу просьба сделать concurrent через asyncio и semaphore. Кодинг 2: по периодическим снимкам стека восстановить, когда функция началась и закончилась. https://www.reddit.com/r/InterviewCoderHQ/comments/1rdzgg6/anthropic_swe_interview_loop_full_breakdown_of/

**Technical breadth / rapid-fire.** Alisa, дословно примеры: «What are some different ways of encoding positional information? What is 5D parallelism? What is the difference between PPO and GRPO?» Второй подтип того же формата без кода: спроектировать эксперименты под исследовательский вопрос, защитить выбор, прокомментировать гипотетические результаты, предложить следующий эксперимент. Тот же блог.

Perplexity, мегатрэд: рекрутерский звонок может содержать вопрос про positional encoding трансформера до любого кодинга. https://www.reddit.com/r/Hack2Hire/comments/1us3j19/perplexity_interview_process_experience/

Apple, отчёт Q1 2026 на InterviewQuery: вывести least squares, описать inpainting на обучении и на инференсе, что такое vanishing gradients и какая архитектура с ними боролась, как посчитать Phi без prior. https://www.interviewquery.com/interview-guides/apple-research-scientist

Amazon breadth, по IGotAnOffer: сравнение методов вроде bagging и boosting, темы уровня graduate coursework. Depth: разобрать свой проект до литературы, классических методов и трейдоффов.

**Research discussion и job talk.** Alisa: интервьюер просит рассказать прошлый проект, дальше разговор течёт сам, могут спросить другие статьи с CV. Готовить «why you chose to work on the things you did», инсайты и будущие направления. Job talk короче академического и про одну статью или одно направление. Её собственный talk был целиком про токенайзеры: основная часть про first-author работу, коротко second-author и текущее, потому что они сходились в одну нить.

Nathan Lambert, июль 2022, это старый замер, Alisa ссылается на него как на образец таймлайна, не как на луп 2026: «Spend a lot of time on your job talk. It's not about technical content, but conveying a vision and a story… the more feedback I got, the fewer plots I felt like I should have». Частые вопросы того цикла: «What do you want to work on when you show up?… If you had unlimited compute what would you work on?» https://natolambert.com/writing/ai-phd-job-hunt

Nvidia, один кандидат Q2 2026: «All the questions were heavily focused on my prior work and how I implemented it. It was very very technical and went into the smallest details. At one point, we also discussed the code and implementation». Презентация около 45 минут. https://www.interviewquery.com/interview-guides/nvidia-research-scientist

xAI: «Present your past exceptional work and your vision with xAI to a small audience».

Cohere, Glassdoor, ноябрь 2025: среди четырёх технических есть paper presentation. Вопрос одной карточки: «Design a Machine learning system». https://www.glassdoor.com/Interview/Cohere-Member-Of-Technical-Staff-Interview-Questions-EI_IE6413613.0,6_KO7,32.htm

**Математика и деривации.** Alisa: «Some companies have a math interview, ranging from fun logic puzzles to serious mathematical derivations with pen and paper». Она советует вероятность, линейную алгебру и матанализ. Её math notes были «all for a single fateful interview». Какие именно компании, она не называет.

OpenAI Residency, Джеки Хехир, сессия размещена на форуме 12 марта 2025, программа с 2022: «the interview process will test both the math and the coding, regardless again of what team you're placed on». Нужны linear algebra, statistics, probability. Формального диплома по математике нет, комфорт с материалом есть. https://forum.openai.com/public/videos/openai-residency-program-info-session-2024-08-05

Apple: деривация least squares, см. выше.

**System design для ML-инфраструктуры.** Anthropic SWE, тот же Reddit: «Design an inference API for serving large language models. Variable-length requests, GPU memory management across concurrent requests, request queuing with priority, streaming responses». Автор пишет, что если готовить один круг, то этот.

xAI: две сессии на умение «formulate, design and solve concrete problems in real world with LLM».

Reflection, вакансия MTS Research Software Engineer (это требования к работе, не вопросы интервью): data, tensor, pipeline, expert parallelism, NCCL, FSDP/ZeRO, Ray, Kubernetes, Slurm, PyTorch, JAX, Megatron-style, Triton. https://www.linkedin.com/jobs/view/member-of-technical-staff-research-software-engineer-at-reflection-ai-4383253252

Cohere, вакансия Post-Training: распределённое обучение, Kubernetes, Slurm, Ray, JAX, PyTorch, XLA/MLIR, пост-тренинг SFT и RL. Это снова требования роли, не протокол интервью. https://startup.jobs/member-of-technical-staff-post-training-cohere-2-7167861

**Поведенческое.** Alisa: «totally textbook behavioral interviews, apart from the occasional question about AI safety or societal impacts». Она завалила первый behavioral, потому что пришла без заготовленных историй: в конце интервьюер сказал «You didn't answer the question».

DeepMind, официальный PDF: много интервью competency-based, рекомендуют STAR. «If you don't have the answer, tell us you're not sure». «Include data» про импакт. https://storage.googleapis.com/deepmind-media/DeepMind.com/Assets/Docs/interviewing-at-google-deepmind.pdf

Amazon: Leadership Principles вшиты в технические круги, Bar Raiser назван на официальной странице подготовки.

Anthropic, пересказ Bloomberg на Reddit (май 2026, сам Bloomberg не открывал): культура-интервью идёт быстро, интервьюер «trying to get signal» и обрывает, когда новых данных нет. Хотят скепсиса к компании и к тому, как она идёт к миссии, а не комплиментов. Провал культуры, по пересказу, закрывает процесс. https://www.reddit.com/r/claudexplorers/comments/1tqqlz1/article_on_how_anthropic_approaches_the_hiring и https://the-decoder.com/anthropic-bans-ai-tools-during-job-interviews-to-see-how-candidates-actually-think/

Prakhar Agarwal, applied researcher, Apple, затем OpenAI API, затем Meta Superintelligence Labs, Business Insider, 29 октября 2025: проверяют, понимаешь ли номенклатуру и что такое LLM, и умеешь ли из абстрактной задачи сделать «workable metric-driven solution». Код «much more involved and related to the actual work». https://www.businessinsider.com/openai-meta-superintelligence-labs-tips-getting-hired-phd-llm-interview-2025-10

## C. Чем компании отличаются

**LeetCode.** У Anthropic research intern карточка прямо говорит, что алгоритмический LeetCode не был фокусом, смотрели скорость кода и рефакторинг. У infra SWE Anthropic задачи прикладные (краулер, профайлер, inference API), не «назови паттерн DP». У Meta RS отчёты InterviewQuery наоборот описывают обычный Meta-кодинг поверх research. У Amazon Applied Scientist medium LeetCode на телефоне описан как обычная часть. У OpenAI Alisa говорит, что general coding бывает и что идеи LeetCode всплывают внутри ML-кодинга. У xAI в открытой вакансии кодинг есть, стиль задач не опубликован.

**Job talk.** Есть у Alisa как отдельный тип (компании не поименованы). Есть у Nvidia (защита своей реализации). Есть у Apple (45 минут, затем 5–6 исследователей). Есть как paper presentation в карточке Cohere. Назван опциональным tech talk у Amazon. У xAI это «Meet the Team» с рассказом exceptional work и vision, не академический семинар. У DeepMind официальный сайт job talk не описывает. Агрегатор пишет paper discussion по 60 минут. Это не одно и то же, пока нет именного отчёта.

**Инфраструктура и распараллеливание.** Сильнее всего это видно в требованиях вакансий Reflection и Cohere и в rapid-fire Alisa («What is 5D parallelism?»). Anthropic SWE system design про GPU memory, очередь и стриминг. Для RS DeepMind официальный текст говорит про гипотезы и статьи. Для Research Engineer DeepMind карьеры говорят: «Building robust infrastructure for distributed computing». https://deepmind.google/careers/

**Take-home.** OpenAI официально включает «take-home projects» в возможные форматы skills assessment, рядом с pair coding и technical tests. Не говорит, что он есть у каждой роли. DeepMind официально take-home не описывает. Одна карточка Glassdoor RS описывает «take home with a timer (3 hours)» внутри 10 кругов и 5 месяцев. Anthropic официально регулирует take-home: без Claude, пока прямо не разрешат. Платный 48-часовой work trial OpenAI с суммой около $1000 в этом прогоне встретился только в агрегаторах, страницы которых целиком не открывал (getorava, preper, interviewcoder). В официальном гайде OpenAI такой цифры нет. В таблицу как факт не ставлю.

**Work trial.** Отдельного оплачиваемого пробного проекта на RS/MTS, подтверждённого страницей компании или именным кандидатом в этом прогоне, не нашёл. Ближайшее официальное: take-home у OpenAI и запрет ИИ на take-home у Anthropic.

**Культура как отдельный убийственный круг.** Прямо описана для Anthropic в пересказе Bloomberg. У Amazon это Leadership Principles плюс Bar Raiser на протяжении лупа. У DeepMind это competency и STAR внутри тех же стадий. У xAI культура в четырёх технических кругах отдельной строкой не выделена. Миссия вшита в «vision with xAI».

## D. По каким рубрикам судят и что валит

OpenAI, инженерные интервью, официально: «well-designed solutions to the challenge, high-quality code, optimal performance, and good test coverage», плюс «strong communication and collaboration». Интервью «designed to stretch you beyond your comfort zone». https://openai.com/interview-guide/

Alisa, самый плотный кросс-лабораторный замер 2026: «technical skills and knowledge are evaluated much more than research experience, though the latter probably gets you the interview in the first place». Сильный сигнал на ML-кодинге: трансформер и отладка доведены до автоматизма, PyTorch свободный, backward на numpy не пугает синтаксисом. Слабый сигнал, который она сама словила: behavioral без заранее разложенных историй. Второй её провал подготовки: первое техническое на двух часах сна, десять минут на off-by-one, ночной зубрёж инференса на интервью не пригодился.

DeepMind официально: заранее заданные критерии, STAR, вслух рассуждать, честно сказать «не знаю», приносить цифры импакта. ИИ на живом интервью и на заданиях запрещён, «unless told otherwise». Подготовка с ИИ разрешена.

Anthropic официально, 10 июля 2025: на живом интервью «This is all you–no AI assistance unless we indicate otherwise. We’re curious to see how you think through problems in real time». На take-home то же, пока не написано обратное. Запрещено просить модель выдумать опыт, которого не было. https://www.anthropic.com/candidate-ai-guidance

Prakhar, OpenAI и Meta: сильный найм умеет назвать дыру в текущей модели и превратить её в метрику, а не ждать постановку задачи. «You need to tell them what needs to be done, rather than the other way round».

Amazon: science depth (свой проект до трейдоффов), science breadth (методы и статистика), кодинг без IDE, Leadership Principles, Bar Raiser. IGotAnOffer отдельно пишет, что LP спрашивают даже на техническом телефоне.

xAI по тексту вакансии: сильный софт, готовность смотреть данные руками, умение поставить и решить конкретную задачу с LLM, плюс рассказ о самой сильной прошлой работе.

Cohere по вакансии: «Extremely strong software engineering skills» стоят первыми. Статья на NeurIPS/ICML/ICLR помечена «Bonus». Строка ниже: список «neither an exhaustive nor necessary set of attributes».

Ошибки, которые в открытых текстах названы прямо.

1. Молча сесть писать код. Для OpenAI это вывод агрегаторов, первично не подтверждено. Для DeepMind официально наоборот: думай вслух.
2. Прийти на behavioral «я и так нормальный» без историй. Alisa, первый behavioral.
3. Тренировать ML-кодинг с включённым ИИ и обнаружить это на живом круге. Alisa: «you will underestimate your reliance otherwise».
4. На Anthropic вживую открыть Claude, если рекрутер этого не разрешил.
5. На Apple и Nvidia не уметь защитить собственную реализацию построчно. Оба кандидатских отчёта уходят в код и мелкие решения, а не в обзор статьи.
6. Путать титулы. Nathan Lambert, 2022, всё ещё полезно как карта: paper-writing RS похож на faculty search, product RS смешивает vision и инженерию, ML engineer давит практикой и может дать 3–5 кодингов подряд. Apple у него как раз дала столько кодинга, что он понял, что роль не его.

## E. Что реально открыто без PhD и без статей

OpenAI официально: «We are not credential-driven—rather, we want to understand your unique background and what you can contribute». Рядом: берут и экспертов, и людей с «high potential», то есть тех, кто уже показывал быстрый вход в новую область и результат. Это не обещание, что RS без портфолио пройдёт скрин.

Резидентура OpenAI, слова руководителя программы: формальных требований к образованию и стажу нет. Техническая планка «at parity with a full-time member of staff», только другая. День первый: математика и программирование уже на месте. Смотрит, прототипировал ли человек, был ли OSS, был ли SWE хотя бы летом, работал ли в общем репозитории. Интервью проверяет математику и кодинг на любой команде. Аудитория: соседние STEM-поля, не действующие сотрудники других лабораторий и не те, кто хочет PhD по ML. Срок программы: шесть месяцев, проект боевой, не стажёрский. Это путь внутрь research без ML-PhD. Это не путь «сразу RS с улицы».

Prakhar, про резюме в топовые лаборатории: «Having a Ph.D. helps. It conveys that you're able to work in an abstract domain. But if you can convey that in a different form, be it at a startup or in your role in building an integral piece of software, that is a good enough scenario to get a résumé accepted». Он сам из graduate school UW в Apple, потом его нашли OpenAI и Meta, отдельно он на эти две не подавался. Выборку из одного человека не надо читать как статистику найма.

DeepMind, карьеры, Research Scientist: «They normally hold a PhD… and frequently contribute to foundational research papers». Research Engineer описан как инженер с глубоким ML, мост между теорией и реализацией, часто соавтор статей. Вакансию Research Engineer, Provenance на Greenhouse поиск показал с формулировкой «Ph.D. … or equivalent practical experience». Повторное открытие того же URL вернуло «Page not found», борд неактивен. Формулировку «or equivalent» подтверждаю только как текст выдачи, не как живую вакансию на сегодня. Другая выдача, Research Engineer Human Understanding, требовала PhD и 3+ года, публикации как преимущество. Тоже не переоткрылась как живая страница.

Cohere MTS Post-Training: статья это бонус. Обязательное в тексте: очень сильный SWE, обучение больших моделей, распределённое обучение, пост-тренинг. Прямая строка: если список не про тебя, но ты полезен, всё равно пиши.

Reflection MTS Research Software Engineer: «You may not have a PhD, but you know how to implement a research paper». Глубина нужна хотя бы в одном: distributed training and inference или data infrastructure.

xAI MTS Reasoning: PhD в требованиях открытой вакансии нет. Есть strong software engineering, ручной просмотр данных, statement of exceptional work как фильтр до первого человека.

Amazon Applied Scientist, разбор постингов у IGotAnOffer: чаще PhD, «in some cases, a Master’s degree with 4+ years of relevant industry experience may also be accepted». Research Scientist у Amazon они отделяют как роль про исследования и публикации. Applied Scientist как роль про код плюс исследование. Data Scientist как роль с меньшим кодингом.

Что эти тексты признают как замену диплома: кусок системы в проде, который ты можешь защитить построчно. Реализация чужой статьи до численно стабильного кода. OSS или прототип в общем репозитории. Математика на уровне деривации, не «слушал курс». Опыт распределённого обучения, если цель Cohere, Reflection, RE.

Что эти тексты не признают заменой для титула Research Scientist в DeepMind: «normally hold a PhD» и статьи. Alisa отдельно пишет, что исследовательский опыт, скорее всего, открывает дверь на интервью, а оффер решает техническая беглость. Без двери разговор о беглости не начинается. Для человека без PhD и без статей реалистичные двери в этих источниках: MTS и Research Engineer (OpenAI, Anthropic, xAI, Cohere, Reflection), Applied Scientist (Amazon), резидентура OpenAI, applied researcher по образцу Prakhar. Дверь «DeepMind Research Scientist» в официальном тексте для такого профиля не описана.

## F. Что изменилось к 2025–2026

Политика ИИ разошлась, единого запрета индустрии нет.

Anthropic, страница от 10 июля 2025. На заявке: первый черновик сам, Claude можно для редактуры. На подготовке: Claude поощряется. На take-home и на живом интервью: нельзя, пока прямо не скажут. Цитата живого круга: «This is all you–no AI assistance unless we indicate otherwise».

OpenAI, гайд, открыт 22 сентября 2026, даты редакции на странице нет. «Expectations for AI and other tools vary by interview: some formats intentionally allow them, while others are designed to assess your independent problem-solving without AI tools. We’ll explain what is allowed in your interview preparation materials».

DeepMind, PDF. Готовиться с ИИ можно. «please don't use any AI tools during live interviews, or during interview tasks», пока не скажут иначе.

Microsoft, страница найма, открыта в этом прогоне. Готовиться с ИИ можно, «as long as it reflects their true capabilities». «During assessments and interviews, candidates should demonstrate their own skills without outside assistance unless explicitly permitted». Число кругов на той же странице: чаще 2–4 разговора до часа. Это общий найм, не отдельная политика MSR.

Meta, WIRED, 29 июля 2025, внутренний пост и подтверждение спикера. Компания разрабатывает новый тип кодинг-интервью, где у кандидата есть ИИ-ассистент: «This is more representative of the developer environment that our future employees will work in, and also makes LLM-based cheating less effective». На момент статьи это пилот с mock-кандидатами из сотрудников, не опубликованное правило для Research Scientist. Путать с лупом FAIR нельзя: в тексте речь о coding interviews.

Трекер UnchartedCareer, обновлён 6 августа 2026, открыт. Из релевантных строк, с цитатой на странице трекера: Anthropic бан на живых интервью. Amazon: «please do not use GenAI tools during your interview unless explicitly permitted» (исходную страницу Amazon я не открывал, цитата со страницы трекера). Microsoft: бан на assessment и интервью, подготовка разрешена. Meta: отдельный тип AI-enabled coding. Google: опубликованного бана трекер не нашёл, вместо этого цитата про хотя бы один очный круг, «just to make sure the fundamentals are there». https://unchartedcareer.com/research/ai-interview-policies

Alisa в поиске, который кончился оффером OpenAI в июне 2026, готовилась так, будто на кодинге ИИ выключен. Это её вывод по реальным кругам, и он совпадает с письменными правилами Anthropic и DeepMind на живом круге. Он не отменяет строку OpenAI, что часть форматов ИИ разрешает. Перед каждым кругом OpenAI вопрос рекрутеру обязателен, гайд сам так говорит.

Агентские задачи. Отдельного формата «собери агента за 45 минут» как стандарта лабораторий я не нашёл. Ближайшее первичное: xAI, две сессии про конкретные задачи «in real world with LLM». Cohere, одна карточка Glassdoor с кругом «Coding with AI assistant», без текста задачи. Anthropic SWE: дизайн inference API, это сервис, не агент. Вакансии Cohere и Reflection описывают работу с RL и агентами как работу после найма.

Живые сессии вместо take-home. Оба формата живы. OpenAI держит оба в одном абзаце. Anthropic регулирует оба. DeepMind официально описывает звонки. Одна карточка Glassdoor добавляет туда трёхчасовой take-home. Замены «все ушли в live» в открытых официальных текстах нет.

## G. Что готовить и в каком порядке

Порядок взят из того, что Alisa назвала самым частым по 57 интервью, плюс двери без PhD из раздела E. Компании, где лупа нет (SSI, Thinking Machines, Scale, Mistral как подтверждённый процесс), в очередь подготовки не ставятся: готовить нечего, кроме общего ядра.

1. ML-кодинг с выключенным ИИ, каждый день, пока не перестанет быть страшным. Трансформер целиком, как домашка CS336, на которую указывает Alisa: https://alisawuffles.github.io/blog/job-search/ . Рядом: decoding (жадный, beam, temperature, top-k, top-p), маленький training loop, backward pass на numpy для одной операции. Критерий готовности: чужой баг в своём attention находится без подсказки модели.

2. Обычный кодинг как фундамент, не как отдельная карьера. Alisa: идеи LeetCode вылезают внутри ML-задач. Для Amazon AS и для Meta RS это ещё и отдельный круг medium. Для Anthropic прикладной кодинг важнее паттернов DP: краулер, конкурентность, чтение чужого кода.

3. Rapid-fire на одном листе, вслух, с таймером. Позиционные кодировки, виды параллелизма вплоть до 5D, PPO и GRPO, память на инференсе, что ломается при смене batch, precision и длины контекста. Это список Alisa плюс вакансии Reflection и Cohere. Не учить как викторину: уметь за минуту сказать, что сломается.

4. Один исследовательский или инженерный сюжет на 45 минут. Для профиля без статей сюжетом является система, которую ты построил: зачем, какой был альтернативный дизайн, какие цифры, какой код, что бы ты выкинул. Nvidia и Apple копают до строки реализации. xAI просит exceptional work заранее письмом, это и есть сюжет. Job talk Alisa: одна нить, мало графиков, много «почему я это выбрал».

5. Математика под один жёсткий круг. Вероятность, линейная алгебра, матанализ, деривация на бумаге. Минимум, который назвали и Alisa, и резидентура OpenAI, и отчёт Apple (least squares). Не распыляться на «всю математику ML».

6. Один ML system design. Inference API: переменная длина, память GPU, очередь, стриминг. Это единственный подробно описанный дизайн, который я открыл (Anthropic SWE). Второй контур, если цель RE: как обучать модель, которая не влезает на один ускоритель. Это язык вакансий Reflection и Cohere, не протокол их интервью.

7. Поведенческие истории, разложенные до интервью. Пять коротких случаев с цифрой. Отдельно: безопасность и общественный эффект, их Alisa называет добавкой к обычному behavioral. Для Anthropic отдельная репетиция скепсиса к самой компании, не гимна. Для Amazon те же истории переложить на Leadership Principles. Для OpenAI прочитать Charter и свежий пост команды, в которую целишься: гайд прямо это рекомендует, плюс Deep Learning Book и Spinning Up.

Порядок компаний для человека без PhD и без статей, если цель именно попасть в луп, а не собрать максимальный престиж титула.

Сначала двери, где текст вакансии сам говорит «статья не обязательна» или «PhD может не быть»: Reflection MTS Research Software Engineer, Cohere MTS, xAI MTS, OpenAI по официальной формуле not credential-driven и, отдельно, резидентура, если готов шестимесячный мост. Затем Amazon Applied Scientist, если есть четыре года практики и мастерская степень или эквивалент по их постингам. Затем Nvidia и Apple как круги, где убивает защита своей реализации, а не список публикаций в открытых отчётах. DeepMind Research Engineer только после того, как живая вакансия снова покажет «or equivalent». DeepMind Research Scientist в текущем тексте карьеры рассчитан на PhD. Meta Superintelligence как applied researcher реалистичен по свидетельству Prakhar, если есть кусок продукта и умение назвать дыру метрикой. Лупы SSI, Thinking Machines, Scale и Mistral в подготовку не закладывать, пока не появится первичный отчёт: строить программу на агрегаторе значит учить выдуманный экзамен.

Календарь одного лупа. Официальные «недели» есть только у DeepMind: закладывать 4–10, и карточка в 5 месяцев показывает, что верхняя граница врёт вниз. OpenAI по сумме их же «в течение недели» на ревью, на assessment и на решение плюс 1–2 дня финала умещается примерно в месяц, если календарь не расползётся. xAI пишет цель «основной процесс за неделю» после пятнадцатиминутного телефона. Cohere в одной карточке: 4 недели. Anthropic SWE в одном самоотчёте: 3 недели на пять кругов. Эти цифры нельзя складывать в одну «среднюю лабораторию».

## H. Proof of work

Поисковые запросы этого прогона: «Alisa Liu job search interview blog», «OpenAI research scientist interview process 2025 2026», «Anthropic MTS interview loop 2025», «Google DeepMind research scientist interview process 2025 2026», «Meta FAIR Superintelligence Labs research scientist interview loop», «xAI interview process research engineer MTS», «Nathan Lambert job hunt AI ML RL», «Mistral AI research scientist interview process», «Cohere interview process member of technical staff», «Perplexity AI interview process research engineer», «Anthropic AI assistance coding interview policy», «Scale AI research scientist interview», «Thinking Machines OR Reflection AI OR SSI interview», «NVIDIA research scientist interview process», «Apple machine learning research interview», «OpenAI careers interview guide site:openai.com», «Amazon applied scientist AGI interview loop», «Microsoft research scientist AI interview process», «Meta AI-enabled coding interview», «Reflection AI careers member of technical staff», «work trial Anthropic OR OpenAI OR xAI», «DeepMind Research Engineer PhD equivalent», «Thinking Machines Lab careers interview process», «SSI Safe Superintelligence careers», «Microsoft assessments interviews own skills».

Страницы, открытые целиком инструментом чтения: https://alisawuffles.github.io/blog/job-search/ , https://www.businessinsider.com/openai-ai-researcher-details-job-search-interview-prep-2026-7 , https://www.businessinsider.com/openai-meta-superintelligence-labs-tips-getting-hired-phd-llm-interview-2025-10 , https://natolambert.com/writing/ai-phd-job-hunt , https://storage.googleapis.com/deepmind-media/DeepMind.com/Assets/Docs/interviewing-at-google-deepmind.pdf , https://deepmind.google/careers/ , https://openai.com/interview-guide/ , https://www.anthropic.com/candidate-ai-guidance , https://unchartedcareer.com/research/ai-interview-policies , https://startup.jobs/member-of-technical-staff-reasoning-alignment-xai-7182794 , https://amazon.jobs/content/en/how-we-hire/applied-scientist-interview-prep , https://igotanoffer.com/en/advice/amazon-applied-scientist-interview , https://www.wired.com/story/meta-ai-job-interview-coding/ , https://careers.microsoft.com/v2/global/en/hiring-tips , https://startup.jobs/member-of-technical-staff-post-training-cohere-2-7167861 , https://www.linkedin.com/jobs/view/member-of-technical-staff-research-software-engineer-at-reflection-ai-4383253252 , https://forum.openai.com/public/videos/openai-residency-program-info-session-2024-08-05 , https://www.techinterview.org/post/3233474918/deepmind-interview-process-2026/ , https://www.interviewquery.com/guides/apple-ai-research-scientist , https://www.interviewquery.com/guides/nvidia-ai-research-scientist .

Страницы, с которых взят текст через поиск, а полное открытие не удалось или упёрлось в пэйволл: https://www.reddit.com/r/LLM/comments/1r93ip9/my_reflection_on_interviewing_with_anthropic_and , https://www.reddit.com/r/InterviewCoderHQ/comments/1rdzgg6/anthropic_swe_interview_loop_full_breakdown_of/ , https://www.reddit.com/r/Hack2Hire/comments/1us3j19/perplexity_interview_process_experience/ , https://www.reddit.com/r/claudexplorers/comments/1tqqlz1/article_on_how_anthropic_approaches_the_hiring , https://www.glassdoor.com/Interview/Google-DeepMind-Research-Scientist-Interview-Questions-EI_IE1596815.0,15_KO16,34.htm , https://www.glassdoor.com/Interview/Cohere-Member-Of-Technical-Staff-Interview-Questions-EI_IE6413613.0,6_KO7,32.htm , https://www.glassdoor.com/Interview/Anthropic-Interview-E8109027-RVW99236753.htm , https://www.interviewquery.com/interview-guides/facebook-research-scientist , https://www.interviewquery.com/interview-guides/nvidia-research-scientist , https://www.interviewquery.com/interview-guides/apple-research-scientist , https://the-decoder.com/anthropic-bans-ai-tools-during-job-interviews-to-see-how-candidates-actually-think/ . Greenhouse DeepMind на повторном открытии вернул Page not found: https://job-boards.greenhouse.io/deepmind/jobs/6897219 .

Не подтверждено первичным текстом в этом прогоне, поэтому в выводах не используется как факт: платный work trial OpenAI на $1000, поминутный RS-луп DeepMind из агрегатора, сроки Mistral, лупы Scale, SSI, Thinking Machines, содержание кодинга xAI сверх фразы «coding assessment».

Ограничение прогона. Blind и levels.fyi с разбором именно RS/MTS 2025–2026 целиком не открылись в выдаче как первичные треды. Интервью Mistral, Scale и SSI остаются дырой. Карта годится, чтобы решить порядок подготовки и какие двери открыты без PhD. Она не годится как обещание, что луп конкретной команды в октябре 2026 совпадёт с соседней командой той же компании: OpenAI прямо пишет, что формат assessment зависит от команды.

🧒 Простыми словами. Это карта экзамена, а не рейтинг компаний. Чаще всего просят руками написать кусок модели, без подсказчика. Рассказ про свою работу тоже есть, но он короче, чем кажется со стороны. Без кандидатской диссертации нормальная дверь называется «инженер в исследовательской команде», а не «учёный лаборатории». У двух компаний письменное правило разное: у одной на живом разговоре помощник запрещён, у другой часть заданий помощника разрешает, и это надо спросить до звонка. По нескольким лабораториям публичного рассказа, как у них проходит экзамен, я не нашёл, и пустую клетку не заполнял догадкой.
