# DR26-09-14-ZB-20-2206 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала подниму живые вакансии, интервью-процессы и конкретные наймы FDE за 2025–2026, плюс то, что уже лежит в волте по этой теме.Дальше открою первоисточники по интервью, вакансиям и конкретным людям из команд OpenAI/Sierra/Harvey.Ещё пара первоисточников по вакансиям Anthropic/OpenAI и по тому, кого реально зовут founding FDE.A. ОТВЕТЫ НА ОСОБЫЕ ВОПРОСЫ

Главный вопрос: какой реальный уровень кодинга требуется

Короткий вердикт. Человек, который строит рабочие системы через AI-агентов, но не пишет код сам «с листа», на Palantir почти наверняка валится на первом техническом экране. На OpenAI / Anthropic / Cursor / Ramp он может пройти дальше, но только если умеет защитить каждую строку, починить сломанный прод и написать рабочий кусок без модели, когда модель врёт. «Я оркестрирую агентов» без умения открыть пустой файл и за 45–60 минут сдать работающий парсер / клиент API / пайплайн — это не FDE. Это соседняя роль: solutions engineer, deployment strategist, legal engineer, enablement. Источники ниже.

Palantir (эталон, самая жёсткая «с листа» планка). Рекрутер → HackerRank или live CodePair ~90–120 минут, язык свободный, часто Python. Задачи: графы (adjacency list, topological sort, multi-source BFS), applied rollups по маленькому датасету, иногда SQL join+aggregate и маленький REST-клиент. На онсайте пул раундов: Decomposition, Learning, Coding, Re-engineering, иногда system design. Re-engineering: дают 250–500 строк чужого, иногда на языке, который кандидат не знает, просят найти баги и починить. AI на скрине Palantir строго запрещён. Это не «LeetCode hard как Google», но это LeetCode-medium, одетый в пользовательский сценарий, плюс умение читать чужой код без подсказки модели. Молчание до конца — красный флаг: коммуникация оценивается внутри того же часа.

OpenAI. Колин Джарвис (Head of FDE) в открытой части интервью: нужны strong software engineers + коммуникаторы; ценность «не в написании кода», а в исходе, но инженерами они остаются. Независимые гайды по лупам 2026: substantial take-home (~5 часов) на RAG / retries / eval design, потом walkthrough, потом onsite 3–4 часа (HM, technical, design/case). Вес: примерно половина — case, customer empathy, business judgment; техническая половина — Python, LLM-adjacent coding, «how do you know the AI system actually works». Скорость лупа ~3 недели. Live coding здесь ближе к «собери маленький прод-кусок», чем к графам Palantir, но пустой файл и работающий код всё равно нужны.

Anthropic. JD прямо: «Strong programming skills with proficiency in Python (and ideally TypeScript/Java) and experience shipping production applications» + production LLM (prompting, agents, evals, scale). 4+ года в technical customer-facing роли или SWE с консалтингом; «Former technical founders are also encouraged to apply». Гайды описывают practical non-LeetCode coding и отдельный customer-simulation, где режут ~60% тех, кто прошёл код. То есть код — гейт, но не финальный фильтр.

Cursor. FDE идёт почти тем же лупом, что product SWE: recruiter/manager → два 60-мин технических телефона → платный онсайт-проект 8–9 часов на живом куске кодовой базы Cursor. AI-инструменты (Cursor, ChatGPT, web search) разрешены и ожидаются. Вставка сырого вывода модели без суждения — быстрый отказ. Pauline Brunet (ведёт FDE Cursor глобально): founding FDE — 5+ лет SWE, new grads и early-career сейчас не берут; команда с Spotify, Rippling, Palantir. JD: Python + TypeScript, production AI-native workflows, не прототипы; «this is not a demo role».

Ramp. Leo Mehr (Director of Eng, вырос FDE с 2 человек в 2023 до ~16 в 2025 и ~30 в 2026): на интервью теперь хотят, чтобы кандидат пользовался AI. Принцип №1 функции — always be scoping, не «да» на каждый SAP-интеграционный запрос в пятницу вечером. Агенты уже срезают ~20% времени на скоуп, но не заменяют умение сказать «нет». Кодинг всё ещё инженерный: FDE сидит в engineering org и пишет в core product.

Общий пол 2026 по независимым гайдам лупов. Coding — production-quality, не олимпиада: JSONL → сессии, paginated API + retries, messy CSV → схема. System design — не Instagram, а VPC/HIPAA/SSO, rollback, evals, cost vs latency. Signature-раунд — ambiguous decomposition (самый высокий вес, ~30%, pass rate ~40%); прыжок к решению до скоупа — самый цитируемый instant reject. Self-assessment The Forward Deployed: если умеешь только «читая страницу» — не умеешь; coding floor — из пустого файла собрать маленькую программу по user-сценарию и один раз её расширить, когда требования меняются, рассказывая вслух. AI-ассистент на learning/debug раундах у части компаний уже норма, но кандидат обязан остаться тем, кто защищает каждую строку.

Проходит ли «агентный строитель, который сам с листа не пишет». Нет на Palantir. На Cursor/Ramp — только если в 8-часовом проекте или live-сессии он ведёт агента как старший инженер (план → гипотеза → тест → защита), а не как оператор чата. На OpenAI/Anthropic — take-home может спасти сильного системщика, но walkthrough его съест, если он не может объяснить, почему retriever, а не агент, и что ломается при пустом timestamp. Это не гипотеза: Palantir-история Vinoo Ganesh про Phoenix ровно об этом — дырка в timestamp ушла в epoch, Cassandra попросила 14 ТБ RAM, система умерла. Человек, который не читает данные и не пишет защитный код, такое не ловит, агент тоже.

Palantir-школа vs AI-стартапы: где планка ниже и почему

Ниже не «легче попасть», а «другой фильтр».

Palantir ниже по: обязательный прод-опыт с LLM/агентами (платформа своя: Foundry/AIP/Ontology); обязательный бренд FAANG; возраст/сеньорность на входе (есть настоящий new-grad / intern pipeline, редко у кого ещё). Palantir выше по: алгоритмический экран без AI, decomposition без спеки, willingness to deploy (в т.ч. government, clearance, 25%+ travel), «why Palantir» с конкретной миссией, умение за час выучить незнакомый API. До ~2016 у них было больше FDE (Deltas), чем обычных SWE; роль буквально «startup CTO в миниатюре». Shyam Sankar на earnings: «Remember, only Palantir has FDEs. Everyone else has sparkling sales engineers». Это маркетинг, но культурный сигнал: они считают копии половинчатыми.

AI-лаборатории (OpenAI, Anthropic) ниже по сырым алгоритмам, выше по: evals, RAG/agents в кастомном VPC, business judgment, 25–50% travel, домен (life sciences / FS отдельные реквизиции). OpenAI FDE org: 2 человека в январе 2025 → ~40 к декабрю 2025 → ~140 к августу 2026 плюс отдельный Deployment Company (~150 с Tomoro + рост). Comp: Palantir FDSE median ~$208–215k TC; mid-senior FDE у лабораторий публично $350–550k, staff выше. Планка «просто попасть IC» у лабораторий выше по бренду и конкуренции, ниже по LeetCode.

Вертикальные AI-стартапы (Harvey, Sierra, Glean, Decagon, Cursor, Modal) ниже по формальному алгоритмическому ритуалу, выше по скорости 0→1, домену и «ты будешь founding FDE, пиши устав функции сам». Harvey founding FDE: 2+ years production software, $165–200k + equity, NYC in-person. Glean founding FDE: $160–270k, founder-level product creation с FDPM. Cursor: 5+ years, не new grad. Modal FDE-Systems: 3+ years SWE + cloud/K8s/IaC, $180–240k, in-person NYC/SF/Stockholm — это ближе к solutions/infra engineer, чем к Palantir Delta.

Почему так. Palantir 20 лет отбирает людей, которые выживут в чужом он-преме без платформенной зрелости. Лаборатории отбирают людей, которые заставят frontier-модель работать на грязных данных клиента и вернут сигнал в research. Стартапы отбирают людей, которые за 6 недель закроют last mile и превратят его в фичу. Vinoo Ganesh: большинство сегодняшних «FDE» — sales engineer на втором звонке или консультант с ноутбуком; настоящий FDE — extension of product, иначе это consulting с лучшим названием.

Сколько в FDE от инженера и сколько от консультанта-переговорщика, и как это сдвинулось с агентами

Palantir-канон: пара Delta (инженер) + Echo (deployment strategist). Delta пишет код и кастомизирует платформу; Echo снимает операционную потребность. День Delta (2019, Palantir blog): часть недели на площадке клиента (митинги + debug/deploy/config), часть — мелкие код-изменения, PR, план; часть — внутренняя поддержка. Соотношение в работе ~60/40 engineer/diplomat на Delta, ближе к 20/80 на Echo. Palantir JD до сих пор: «responsibilities look similar to those of a startup CTO».

2025–2026, агенты. Natalie Meurer (Sierra, 5 лет Palantir 2016–2021, теперь Head of Agent Engineering): dirty secret в том, что слово FDE растянулось на DevOps, data integration, ontology, solutions, enablement. Код стал дешёвым; единственный хедж — accountability за исход клиента. «Forward deployed engineering is dead. Long live forward deployed engineering». На AI Engineer World’s Fair, июль 2026. Это не «больше не надо кодить». Это: кодить теперь могут агенты, отвечать за то, что агент сделал в проде клиента — нет.

Leo Mehr / Ramp: агент в Slack срезает 20% скоупа; опасность — «token maxing slop cannon», который штампует плохие фичи быстро. Оставшаяся работа FDE — scoping и отказ. Соотношение сдвигается от 70/30 «пиши кастом» к 50/50 или 40/60 «реши, что строить / обобщить / отказать», но отчётность остаётся в engineering, не в sales.

OpenAI (Jarvis): ~50% времени on-site (UAE/Japan до 80%). День: разговор с payments-командой про миграцию легаси → на следующий день тесты → на третий код миграции или UI. Ценность не в LOC, а в том, чтобы сделать себя ненужным. Если тебя зовут снова на ту же проблему — ты её не решил. Feedback в research формулируется как «модель ошибается на COBOL→Java вот такого класса», не «клиент недоволен».

Итог по соотношению. Инженерская половина не исчезла, она сместилась: меньше зелёного поля с нуля, больше evals, connectors, guardrails, «починить то, что агент сломал», обобщить в платформу. Переговорная половина выросла: scoping, отказ, доверие (Morgan Stanley: техника 6 недель, доверие 4 месяца, adoption 98% advisors). Кто умеет только говорить — SE. Кто умеет только кодить в вакууме — SWE. FDE 2026 = инженер, которого можно посадить в комнату с нетехническим VP и который вечером сам катит в прод.

Как читается опыт «CTO своей компании» на FDE-найме

Важная развилка, которую путают кандидаты.

1. Основатель-CTO, который сам писал и продавал, и компания умерла или он вышел — это плюс, почти канон. Anthropic JD буквально зовёт former technical founders. Leo Mehr: не career FDE, а ex-founder Lumos (a16z), потом Ramp FDE. Het Trivedi: провалившийся стартап → блог про деплой LLM на Truss → DM кофаундера Baseten → FDE. Sunny Rekhi (Decagon, CTO of FDE): «after years as a founder». Founding FDE в Glean/Harvey описан как «autonomy and accountability of a founder».

2. Наёмный COO/CTO с длинным стажем, опционом, без основания компании — это другой сигнал. Palantir сравнивает работу FDE с работой startup CTO, но нанимает на IC. Читается так: «умеешь ли ты всё ещё personally ship в чужом репо за неделю, или ты уже человек, который ставит задачи другим». Если 11 лет ты руководил и не можешь показать личный код последних 24 месяцев — тебя положат в Head of FDE / Technical Deployment Lead, не в IC, и то только если есть proof, что ты сам в поле. Если proof нет — «overqualified / won’t get dirty / won’t travel 50%».

3. Перевод, который работает: не «CTO of X», а «I was the person in the room who owned the outcome when the platform met messy production data, I wrote the first version, I sat with the operator, I brought the pattern home». Vinoo: FDE, который осчастливил один аккаунт и ничего не вернул в продукт, провалил единственную цель роли. Наёмный CTO без этой петли выглядит как дорогой консультант.

4. Возраст и уровень. Palantir FDSE historically 0–7 years. Cursor founding FDE — 5+ years SWE, не школа. Head of FDE / Director — это уже Leo Mehr / Jarvis / Brunet / Meurer. Человек с 20 годами карьеры на IC FDE в Palantir new-grad треке будет странным; на founding FDE / staff FDE / Head of FDE в лаборатории или стартапе — нормальным, если кодинг живой.

Не нашёл публичного примера, где наёмный не-фаундер CTO крипто-компании без FAANG/Palantir-бренда за последние 24 месяца был нанят именно IC FDE в OpenAI/Anthropic/Palantir. Это не значит «невозможно». Это значит: в открытом следе таких людей нет, и бремя доказательства на кандидате.

---

B. РЫНОК 2025–2026, КОГО КОПИРУЮТ

Роль придумал Palantir в начале 2010-х (внутренне Deltas). a16z назвал её hottest job in startups (2025). Indeed: FDE-like postings +800% Jan–Sep 2025, +729% к апрелю 2026 vs год назад, average posted base ~$172k. Plank census 12.09.2026: 1333 живых FDE-постингов в 565 компаниях; median posted base midpoint $193k по 388 постингам с зарплатой; только 9% вакансий сидят в frontier labs + Palantir, длинный хвост — growth-stage AI, которые нанимают первых FDE. OpenAI DeployCo $4B+ (май 2026, покупка Tomoro ~150 FDE), Anthropic Ode JV ~$1.5B, Microsoft Frontier $2.5B, AWS $1B FDE org (июнь 2026), Google Cloud массовый набор, Salesforce заявил 1000 FDE. Пентагон сам начал нанимать FDE под именем War Force (июнь 2026). Это уже не ниша Palantir, это индустриальный рефлекс last-mile.

---

C. КОМПАНИИ СКОУПА

Ниже — одна рамка на компанию. По именам: публичный след IC-наймов тонкий. FDE не делают пресс-релизов «я джун». Ниже только люди, которых я реально нашёл в открытых анонсах, био конференций, LinkedIn-постах, подкастах, корпоративных блогах. Где 12–20 живых IC за 24 месяца в одной фирме из открытых источников не собирается — так и написано.

---

C1. PALANTIR (эталон FDSE)

1. Кого берут. Palantir почти не публикует имен новых FDSE. Видимый слой — alumni, которые УХОДЯТ в FDE-функции других компаний, и лидеры, которые эту школу строили.

Проверяемые люди (не все «наняты за 24 месяца», часть — канон школы, без которого копии непонятны):
- Vinoo Ganesh — Palantir PD (Phoenix storage), затем FDE commercial/DoD/NatSec/healthcare/O&G, вёл Project Frontline (~250 человек прошли ротацию SWE→FDE; выпускники теперь ведут FDE в OpenAI, Anthropic, xAI, Anduril). Сейчас CEO Kepler. Latent Space, 12.09.2026.
- Natalie Meurer — Palantir 2016–2021: выучила код с policy-бэкграунда, privacy + FDE на law enforcement/defense, патент audit-log. Georgetown STIA, не CS. Затем Stanford GSB MBA 2023, Sierra. AI Engineer talk 28.07.2026.
- Anjor Kanekar — 7 лет FDE Palantir, в т.ч. финальная сборка Airbus; источник Pragmatic Engineer (12.08.2025). Сейчас advisor по сборке FDE-функций.
- Shilpa Balaji — бывший Palantir FDE и hiring lead, теперь talent/ops в Promise; панель First Round Review про найм FDE (фев 2026).
- Nic Prettejohn — Head of AI UK Palantir; FT 02.11.2025: FDE = «product discovery from the inside».
- Trae Stephens, Matt Grimm — Palantir FDSE → co-founders Anduril (старше 24 месяцев, но Business Insider 04.06.2025 использует их как доказательство, что роль штампует фаундеров).
- Greg Barbaccia — Federal CIO / acting TTS director, уходит в Palantir сентябрь 2026 (FedScoop via Channel Dive). Это не FDE IC, это reverse-deploy: правительство → Palantir.

Не нашёл: публичный список 12–20 конкретных людей, нанятых Palantir на FDSE в 2024–2026. Компания не анонсирует IC. Живые вакансии есть пачками (Plank: 75 open FDE-family на профиле Palantir; Aced: 76 на 02.09.2026, включая new-grad).

2. Предыдущий опыт. Канон Palantir: CS/math/physics/EE или equivalent; 0–7 лет; generalist programmer (Python, Java, TypeScript); клиентский темперамент; готовность ехать на завод / базу / банк. Meurer — не CS, policy nerd + self-taught code. Ganesh — PD engineer, который стал FDE после того, как его система умерла на реальных данных банка. Не-канон (не ML-PhD, не FAANG) у видимых людей — норма, не исключение: Palantir сам и есть канон.

3. Паттерны. Сигнал: decomposition вслух, learning speed на чужом коде, owned outcome в грязной среде. Дверь: campus/new-grad, реферал, intern→full-time; холодная заявка возможна из-за объёма найма. Тайминг: AIP bootcamps 2023–24 конвертировали в семизначные контракты → найм FDE не циклический, а структурный.

4. Идеальный кандидат. One-liner: «инженер, которого можно высадить в непонятную операцию в понедельник и у которого к пятнице что-то работает у реального пользователя». Обязательно: пишет код без AI на экране; структурирует туман; учит незнакомый стек за час; говорит с нетехнарём; ездит. Желательно: домен (defense, manufacturing, healthcare, finance); clearance. Пороги: new-grad OK; senior — 8+ лет Foundry уже на стороне клиента (см. Trinity Industries JD: 8+ data eng, 4+ Foundry). Первая строка резюме: не «passionate about data», а «shipped X in customer environment against Y constraint, measured by Z».

5. Анти-паттерны. Чистый LeetCode-герой без декомпозиции. Консультант, который не коммитит. «Почему Palantir» без конкретной миссии. Отказ от government work / travel. Скрытие за процессом («нужен SOW»). Выглядит как плюс, читается как минус: слишком senior IC, который хочет управлять людьми, а не сидеть в workshop; «я уже CTO».

6. Как оценивают. 4–6 недель. Recruiter (why Palantir) → OA/live coding → onsite 3 раунда из пула Decomposition/Learning/Coding/Re-engineering → HM. Валятся на decomposition и на «why Palantir». AI на скрине нельзя.

7. Конвертация нестандартного. Meurer: tech policy → self-taught code → Palantir privacy/FDE. Формула: «I learned to code in order to put safeguards on systems that touch civil liberties, then sat with the operators». Physics/math PhD без ML — классический Palantir-вход, если человек кодит.

8. Источники. Palantir careers / Lever FDSE; blog palantir «Dev versus Delta»; The Forward Deployed Palantir guide (reviewed 13.07.2026); Pragmatic Engineer 12.08.2025; Latent Space Ganesh 12.09.2026; Meurer talk.

---

C2. OPENAI (FDE + Deployment Company)

1. Кого берут.

- Colin Jarvis — основал FDE-функцию январь 2025 (X, 15.01.2025). В OpenAI с ноября 2022, месяц ChatGPT, один из 2–3 человек в Европе, сначала Solutions Architect / Head of SA. До: Accenture AI, Mudano, EY, everyLIFE. Образование: Celtic Studies + Strategy and International Security — не CS. Glasgow. К авг 2026 — Global Head, org ~140. Altimeter podcast 26.11.2025; Eng Leadership 15.08.2026.
- Arnaud Fournier — FDE EMEA, Париж как крупнейший FDE-офис OpenAI (~10 человек на окт 2025). FT 02.11.2025: план ~50 FDE в 2025. Le Monde 13.05.2026: станет CTO OpenAI Deployment Company. Journal du Net 17.10.2025.
- Manan Mehta — OpenAI FDE SF, LinkedIn 13.10.2025. До: Senior Data Scientist C3 AI (2024–2025); PhD Engineering + Statistics minor, UIUC 2020–2024; BITS Pilani BE. Не FAANG-SWE. RocketReach/LinkedIn.
- Eoin Power-Moran — OpenAI FDE, Jarvis публично назвал его автором демо end-to-end AI system (LinkedIn 26.11.2025).
- Команда, которую Jarvis перечислил на offsite Napa, дек 2025 (публичный LinkedIn-пост, не полный CV каждого): Robel Yemiru, Damon Doucet, Vishnu Chopra, Sharan T., Corvin Deboeser, Sam Passaglia, Gustav von Zitzewitz, Frederic Winter, Jordan Seow, Hélène Pélissier, Omer Khan, Pearson Henri, Randy Zhao, Sean Saito, Qiwei Li, Madeline Oltmann, Jay Xu, Samuel Kramer, Samuel Pickover, Sam Wong, Thomas Labadie, Lionel Cheng, Ilyass El Mansouri, Darrell Adjei, Ayman Farhat, Lukas Frei, Jennifer Lau, Katrina Mansoni, Viktoriia Oliinyk, Eoin Boylan, Hannah Koch, Joshua Geron, Louis Delano, Stephen Petersilge, James Dyett, Nathan Harbacek, Brandon Baker, Roy Ziv, Ryan Cain. Это ~40 имён living FDE org конца 2025. Полные CV на каждого в открытом вебе я не открывал поштучно — не выдумываю бэкграунд.
- Tomoro → OpenAI Deployment Company, 11.05.2026, ~150 FDE/Deployment Specialists. Кофаундеры: Rishabh Sagar (16 лет high-volume cloud data / productionizing ML, FS), Chris Spencer (Accenture AI lead, Head of Data Solutions Mudano 8+ yrs, EY, RBS — тот же колодец, что Jarvis), Ed Broussard, Chloe Kelleher, Ash Garner, Sandi Chanda, Albert Phelps. Клиенты Tomoro: Tesco, Virgin Atlantic, Supercell, Fidelity, Red Bull, Mattel, NBA. OpenAI official post. Не нашёл, кто из 150 именно «FDE» vs «deployment specialist».

2. Предыдущий опыт. Два канала: (а) внутренний апгрейд SA→FDE (Jarvis); (б) enterprise applied-AI / data science / consulting-engineer (C3, Accenture, Mudano, EY); (в) покупка целой FDE-фирмы. PhD-не-ML (Mehta, engineering) проходит. Не-CS (Jarvis, Celtic Studies) проходит на head-уровне после доказательства delivery. FAANG не обязателен.

3. Паттерны. Сигнал: довели клиента до production (не PoC), умеют evals, возвращают конкретный сигнал в research. Дверь: внутренний перевод; реферал; acquihire. Тайминг: 2025 — сбор функции с нуля; май 2026 — отдельная компания, потому что repeatable problems вылезли из custom work.

4. Портрет. One-liner Jarvis: FDE focused on outcomes; make yourself obsolete. Обязательно: strong SWE + communicator + готовность 50% on-site. Желательно: домен (отдельные роли Life Sciences, FS). Пороги JD Life Sciences (дек 2025): base $180–280k, SF hybrid 3 days, travel up to 50%. Первая строка: «I took an ambiguous production problem in [domain], shipped a system operators actually used, measured it, and fed the failure mode back into the model/product».

5. Анти-паттерны. Solution architect, который не пишет на инфре клиента (это соседняя роль SA, она у OpenAI осталась отдельно). Generalizing too early. Неумение отличить «клиент описал» от «так выглядят данные». Отказ ездить.

6. Оценка. ~3 недели. Recruiter, take-home, walkthrough, onsite. Валятся на eval-вопросе и на customer-pitch.

7. Конвертация. Jarvis: консалтинг/SA + Celtic Studies → Head of FDE. Перевод: не «я консультант», а «I was already the person getting customers to production; we needed a function that writes on their infra and talks to research». Mehta: PhD + C3 applied DS → FDE за ~год после защиты.

8. Источники. openai.com/index/openai-launches-the-deployment-company/; Jarvis X 15.01.2025 и 27.11.2025; FT 02.11.2025; Pragmatic Engineer 12.08.2025; Eng Leadership 15.08.2026; LinkedIn Jarvis 02.12.2025; LinkedIn Mehta 13.10.2025.

---

C3. ANTHROPIC (Applied AI / founding FDE)

1. Кого берут. Публичных IC-анонсов мало.
- Cat de Jong — Head of Applied AI (FDE живут здесь). До: Snowflake, data side. FT 02.11.2025: Applied AI (FDE + product engineers) вырастет 5× в 2025. Snowflake Summit 2026: команда помогает клиентам строить первых агентов — данные, evals, custom integrations.
- Alexander Bricken — Applied AI, LinkedIn 03.12.2025: «we’ve just spun up a new segment… hiring Global Head of FDE and FDE ICs US + London».
Не нашёл имён founding FDE IC, которых Anthropic публично представил в 2025–2026. JD крутится с лета 2025 (Boston/NY/SF/Seattle/London), base $200–300k.

2. Опыт. JD: 4+ years technical customer-facing (FDE) или SWE + consulting; former technical founders welcome; production LLMs (prompting, agents, evals, scale); Python + ещё один язык; travel 25–50%. Для Парижа — native French. Не требуют ML-PhD.

3. Паттерны. Сигнал: Claude в проде у regulated клиента + eval harness. Дверь: founding FDE волна дек 2025 — окно «напиши устав». Тайминг: параллельно Ode JV с Blackstone/H&F/GS (~100 инженеров, CEO Chris Taylor — имя JV, не проверял CV в этом прогоне).

4. Портрет. One-liner JD: embed with most strategic customers, ship production Claude apps, codify repeatable patterns. Обязательно: Python prod + LLM prod + customer-facing. Желательно: founder, safety fluency, regulated domain.

5. Анти-паттерны. Игнор safety/constitutional AI на интервью. Чистый researcher. Чистый consultant без кода. Выглядит как плюс, минус: «я делал PoC на Claude API» без evals и без VPC.

6. Оценка. ~21–28 дней по вторичным гайдам. Coding practical + customer simulation (основной нож). Mission alignment.

7. Конвертация. Founder явно приглашён. Не-фаундер CTO — не запрещён, но JD формулирует «founders», не «executives».

8. Источники. Anthropic Greenhouse/LinkedIn JD; FT 02.11.2025; Bricken LinkedIn 03.12.2025; startup.jobs Anthropic FDE.

---

C4. SIERRA (агенты клиентского сервиса; Palantir-кровь)

1. Кого берут.
- Natalie Meurer — Head of Agent Engineering, >120 инженеров к 2026. Palantir 2016–2021, GSB 2023, Sierra с ~2023–24, в 2024 определила agent engineering как customer-accountable applied AI. Не CS. Публичный доклад «Dirty Secret of FDE», AI Engineer, 28.07.2026. Sierra hiring across FDE roles в конце того же доклада.
Не нашёл 12 именных IC FDE Sierra за 24 месяца в открытом вебе.

2. Опыт, который сработал у видимого лидера: policy + self-taught code + 5 лет Palantir на government + MBA + умение учить клиентов (Sierra University). Странный актив: «learned to code to do privacy engineering».

3. Паттерны. Sierra сознательно назвала роль agent engineer, не FDE — чтобы не размыть. Сигнал: агент, который живёт в политике компании, интегрирован в ops-системы, работает вне демо. Дверь: Palantir alumni — главная труба в эту фирму.

4. Портрет. Palantir vintage FDE, который умеет учить клиента строить агентов, а не вечно сидеть в аккаунте.

5. Анти-паттерны. Хотеть title FDE без понимания vintage. Демо-агент без handoff rules и без прод-надёжности.

6. Оценка. Не нашёл полного публичного лупа Sierra FDE. Meurer hire-side: ищут generalists, прошедших несколько vintage.

7. Конвертация. Meurer = учебник неканона.

8. Источники. YouTube Byv311hdoHE; latent.space 01.07.2026; sierra.ai/author/natalie-meurer; Stanford GSB profile.

---

C5. HARVEY (legal AI)

1. Кого берут. Winston Weinberg (CEO) 02.12.2025: запускаем Founding Team of Forward-Deployed Engineers в NYC. Это дата рождения функции, не список нанятых. Не нашёл публичных имён founding FDE Harvey, которые вышли в прессу с «I joined as FDE». Параллельный трек, который Harvey нанимает пачками — Legal Engineers (бывшие юристы, не FDE). a16z Jobs 14.09.2026: 70% команды пришло за этот год; Legal Engineer in-house до $320k.

2. JD founding FDE (Ashby, NYC, $165–200k + equity + bonus, in-person): 2+ years building and operating production software, 0→1 ownership, ambiguity; embed в AmLaw / in-house; customize retrieval, agents, prompts, guardrails; integrate DMS/KM/SSO (iManage, NetDocuments). Perspective AI (осторожно: это вендор customer-research, цифры headcount 40–80 — оценка, не disclosure Harvey).

3. Паттерны. Дверь: founding window дек 2025. Сигнал: прод-софт + готовность сидеть в BigLaw 6–9 месяцев. Юридический диплом не обязателен для FDE (для Legal Engineer — да).

4. Портрет. Инженер, который не боится партнёра Magic Circle и iManage.

5. Анти-паттерны. «Я сделал RAG на HuggingFace». Нет интеграций с DMS. Желание remote при in-person NYC.

6. Оценка. Не нашёл детального публичного лупа именно Harvey FDE. Компания быстро растёт, Talent VP Maggie Landers наняла 1000+ за 14 месяцев — процесс, вероятно, стартап-сжатый.

7. Конвертация. Юрист → Legal Engineer (другая дверь). Инженер без права → FDE, если умеет DMS/evals.

8. Источники. Weinberg LinkedIn 02.12.2025; jobs.ashbyhq.com/harvey/24f97125-c406-4ae3-9f94-07dd1310e511; harvey.ai/blog/landing-a-job-at-harvey.

---

C6. DECAGON (customer-service agents) — важная развилка

Публичный конфликт сигналов. CEO Jesse Zhang (Newcomer / PYMNTS авг 2026): Decagon бьёт $100M ARR и «избегает» FDE-модели Sierra — продукт должен настраиваться клиентом без вечно встроенного инженера. Одновременно: вакансия Forward Deployed Engineer, Agent Builder (The Homebase, 07.02.2026, $175–230k, senior 5+); Sunny Rekhi — «CTO of Forward Deployed Engineering», доклад AI Engineer 28.07.2026: FDE в Decagon = product engineering, тот же бар, тот же reporting; два вида FDE — (1) конфигурирует мозг агента, (2) возвращает повторяющиеся запросы в продукт. То есть FDE у них есть, но идеология — не жить в аккаунте вечно, а обобщать за 6 недель.

1. Кого берут. Sunny Rekhi — бывший фаундер, создал функцию, пишет блог «why I created the role». New-grad cohort фев 2026 (Kelly Lu MIT/YC founder, intern Ramp+Discord; Sajan Shah CMU intern Glean; Max Weng UCSD intern Stripe) — это general engineering, не доказано что FDE.

2–4. Бар = product SWE + enterprise agent builds (AOPs, handoff rules). Паттерн-сигнал: «enterprise A попросил X, я знаю что B попросит через 2 недели — строю сразу для всех».

5. Анти-паттерн: хотеть вечный embed. Здесь это минус.

6. Оценка. Не нашёл полного лупа. Доклад Rekhi — лучший первоисточник про культуру.

7. Конвертация. Founder → Head of FDE.

8. Источники. YouTube 7wu2hsRfvV0; PYMNTS 14.08.2026; thehomebase.ai Decagon FDE posting.

---

C7. GLEAN

1. Кого берут. Не нашёл именных founding FDE. Вакансия Founding Forward Deployed Engineer $160–270k, US remote (позже снята): под с FDPM, 0→1 product surfaces с C-suite, то что построил — шипится всем клиентам Glean.

2. JD about you: think like a founder, as comfortable with CEO as in a codebase, shipped products real people use.

3. Паттерн: product-led FDE, не services-led. HeroHunt 2026 recruiting guide прямо делит: Glean/Sierra/Decagon/Ramp = product-led (спрашивают «это фича?»), OpenAI/Anthropic/Palantir ближе к services-led embed.

4. Портрет. Основатель внутри чужой платформы.

5. Анти: «настрою существующий поиск». Роль — invent the next surface.

6. Не нашёл лупа.

7. Founder-language в JD — прямой перевод нестандартного опыта.

8. welcometothejungle / startup.jobs Glean founding FDE.

---

C8. SCALE AI

1. Кого берут. Не нашёл именных FDE IC 2024–2026. Вакансии живые: FDE GenAI $156–195k; Senior Forward Deployed AI Engineer Enterprise $252–315k, 5+ years SWE, LLMs/RAG/agents in production, connectors в cloud/DWH.

2. Функция исторически — RLHF/data pipelines внутри frontier labs и government. Perspective AI описывает Scale как мост data engine ↔ enterprise; цифры headcount у этого источника не независимы.

3. Сигнал: messy data unification + security-adjacent. Дверь: defense/gov.

4. Портрет. Data/ML engineer, которого можно посадить в lab или агентство.

5. Анти: нет production agents/RAG. Нет готовности к government.

6. Не нашёл детального лупа Scale FDE отдельно от общего FDE-шаблона.

7. Не нашёл публичной конвертации неканона.

8. welcometothejungle Scale AI postings.

---

C9. CURSOR (Anysphere)

1. Кого берут.
- Pauline Brunet — leads FDE globally. 10 лет AI deployments в консалтинге, третья tech-компания. Нанимает «unicorns»: 5+ years SWE, customer-facing; команда со Spotify, Rippling, Palantir. Не берёт new grads на founding set. Workman Labs transcript её доклада (проверено 12.09.2026).
Имён IC «я присоединился FDE Cursor» в открытом поиске не нашёл.

2. JD (Australia/Japan/Singapore и US-эквивалент): embed с customer eng teams; Cursor-powered workflows (refactors, migrations, PR review loops, incident→fix, spec→implementation); tracing, evals, latency/cost; Python + TS; production reliability.

3. Сигнал: убедил Staff+/VP Eng принять новый способ работать в их репо. Дверь: 5+ years + customer aptitude; реферал Cursor/Graphite в форме заявки. Тайминг: enterprise rollouts Cursor 2025–26.

4. Портрет. Сильный SWE, который умеет продать изменение workflow инженерам, не закупщикам.

5. Анти: demo-only. New grad. Не умеет TS. Не может объяснить, почему агент не должен трогать этот репо.

6. Оценка. Тот же луп, что product: 2 технических телефона + 8–9 часов paid project на реальном коде Cursor, AI разрешён, сырой paste — отказ. Это самый честный ответ на вопрос «проходит ли агентный строитель»: да, если в этом 8-часовом дне он выглядит как старший инженер, ведущий инструменты, а не как человек, который не может без них.

7. Консалтинг 10 лет (Brunet) → Head of FDE. Перевод: «I have been doing AI deployments to enterprises for 10 years» + третья компания, не «я консультант McKinsey».

8. cursor.com/careers/forward-deployed-engineer-australia-japan-singapore; fdeinterviews.com/company/cursor; Workman Labs Brunet.

---

C10. MODAL

1. Кого берут. Не нашёл именных FDE. Роли: FDE-Systems $180–240k (3+ years SWE, AWS/GCP/Azure, Docker/K8s, Terraform/Pulumi, in-person NYC/SF/Stockholm); FDE-ML $180–250k; отдельно был FDE-ML AI Coding Agents (Lovable, Suno, Mistral, sandbox product). Клиенты в JD: Suno, Lovable, Cognition, Meta.

2. Это ближе к solutions/infra engineer + лёгкий pre-sales, чем к Palantir Delta. «Partner with sales», demos, PoCs, migration paths.

3. Сигнал: сам поднимал прод-инфру GPU/serverless, умеет говорить с CTO/ML lead. OSS/side project — бонус.

4. Портрет. Cloud/systems engineer, которому нравится чужая боль.

5. Анти: нет облака. Нет желания in-person. Чистый LLM-промптер без K8s.

6. Не нашёл лупа. Ожидай systems design + hands-on infra.

7. Solutions engineering прямо указан как acceptable background.

8. builtinsf.com Modal FDE-Systems; jobs.ashbyhq.com/modal; Redpoint job board Oct 2025.

---

C11. СОСЕДИ, КОТОРЫЕ КОПИРУЮТ МОДЕЛЬ (чтобы не делать вид, что скоуп = весь рынок)

Ramp — Leo Mehr: Cornell engineering physics, Oxford CS/philosophy, HRT algorithm engineer, Stanford MS CS, co-founder Lumos (a16z), ~вторая FDE-наёмка Ramp late 2023, теперь Director, org ~30. Не Palantir. Доказывает: founder + markets + CS бьёт Palantir-родословную в fintech FDE. Engineering.ramp.com / X thread 05.08.2025.

Baseten — Het Trivedi, FDE с января 2024. Провалившийся стартап, блог про деплой OSS LLM через Truss, кофаундер сам написал в DM. Medium 02.06.2024. Дверь = контент. Не канон, не FAANG.

Retell — Tyler D’Silva, founding FDE. Waterloo, 6 co-ops, «Cali or bust», 2-day work trial. YouTube «Meet FDE» 18.07.2026. Дверь = paid work trial, не LeetCode.

Databricks, Stripe, Salesforce, Google Cloud, AWS, Cohere, Mistral, Rippling, Notion, Retool — массово открывают FDE-family роли в 2026 (Plank/GSDC/JBC индексы). Поименно за 24 месяца в этом прогоне не собирал.

a16z FDE Fellowship 2026 — Vinoo: за ужином FDE из Snowflake, Anthropic и стартапов описывали три разные работы одним словом. Это главный мета-вывод рынка: title перестал быть спецификацией. Смотри reporting line (product vs sales) и incentive (платформа vs часы).

---

D. ПОВТОРЯЮЩИЕСЯ ПАТТЕРНЫ ПО ВСЕМУ СКОУПУ

(а) Сигнал (>50% видимых нанятых / JD):
- Личный прод-код, не слайды.
- Конец-в-конец ownership в условиях ambiguity.
- Умение скоупить и отказать.
- Коммуникация с нетехнарём без потери технической линии.
- Travel 25–50% как default, не exception.
- Python как лингва франка; второй язык (TS/Java) часто нужен.
- Для AI-FDE 2026: evals, RAG/agents, «how would I know it’s working».

Чего почти нет у отвергнутых (по гайдам лупов, не по моей выборке reject’ов — reject’ы компании не публикуют, это ограничение):
- Прыжок к решению до вопросов.
- Молчаливый код.
- PoC без метрики.
- «Мы» вместо «я» в behavioural.
- Сырой AI-вывод без суда.

(б) Дверь:
- Palantir alumni — главная труба в Sierra/Anduril/xAI/OpenAI FDE leadership (Ganesh: Frontline grads).
- Consulting-engineer колодец Accenture/Mudano/EY → OpenAI (Jarvis, Spencer).
- Founder (успешный или нет) → founding FDE / Head.
- Acquihire целой FDE-фирмы (Tomoro, Northslope — Palantir alumni firm, OpenAI DeployCo июль 2026).
- Контент, который нашёл кофаундер (Trivedi).
- Work trial (D’Silva).
- Реферал. Холодная заявка работает там, где объём вакансий огромный (Palantir, Google, Salesforce), не в founding FDE из 3 человек.

Не нашёл доказательств, что OSS-звёзды сами по себе открывают FDE-дверь чаще реферала. Modal упоминает OSS как бонус, не как вход.

(в) Тайминг:
- 2025 H1: лаборатории ставят founding FDE.
- 2025 H2: 800%+ рост постингов, a16z «hottest job».
- 2026 H1: $9B+ в deployment ventures, покупка готовых FDE-команд, потому что найм по одному слишком медленный (median days-to-fill у RFS: 71).
- Окно founding FDE в конкретной фирме живёт месяцы, не годы: Harvey «founding» янв→«director» к весне 2026 (AdamGTM). Кто приходит во вторую волну — уже не пишет устав.

Неканонический путь. Из проверяемых людей выше у значительной доли нет ML-PhD и нет FAANG-SWE: Jarvis (Celtic Studies, consulting), Meurer (STIA, self-taught), Mehta (non-CS-brand PhD engineering, C3), Trivedi (failed startup + blog), Brunet (consulting 10 yrs), Spencer (Accenture/Mudano/EY/RBS). Канон «Stanford CS → Palantir → OpenAI FDE» существует, но он не единственный, и на head-уровне даже не доминирует в открытом следе.

---

E. СОБИРАТЕЛЬНЫЙ ПОРТРЕТ ИДЕАЛЬНОГО КАНДИДАТА (IC FDE в AI-компании 2026)

One-liner. Инженер, которого можно встроить в чужую операцию на неделю и у которого к концу недели у реального пользователя что-то работает, с метрикой, и кусок этого работы можно унести в продукт.

Обязательные 7:
1. Сам пишет и дебажит прод-код (Python ± TS/Java). На Palantir — без AI, с листа. На Cursor/Ramp — с AI, но с защитой каждой строки.
2. Конец-в-конец ownership: discovery → scope → build → eval → rollout → pager.
3. Decomposition: 2 минуты не предлагает продукт.
4. Клиентская комната: отказ с because, перевод в деньги/риск.
5. Production AI 2026: evals, failure modes, VPC/IAM, не «я звал API».
6. Готов ездить 25–50% и работать в чужом Slack/VPC.
7. Low-ego high-execution: Jarvis прямо это написал в hiring-посте.

Сильно желательные:
- Домен, совпадающий с реквизицией (FS, life sciences, legal, manufacturing, defense).
- Founder или founder-like 0→1.
- Palantir / applied-AI consulting / enterprise implementation.
- Публический артефакт: репо, пост, доклад, патент — не обязательно звёзды.
- Умение вернуть паттерн в платформу (иначе ты консультант).

Измеримые пороги, которые реально стоят в JD, не в мечтах рекрутеров:
- Годы: Palantir new-grad OK; Anthropic 4+; Harvey 2+ prod; Cursor founding 5+; Scale senior 5+; Modal 3+.
- Comp: Palantir TC ~$155–295k (median ~$208–215k); Anthropic base $200–300k; OpenAI FDE base в постингах до ~$345k + equity; Harvey founding $165–200k; Glean $160–270k; Cursor/Modal $180–240k+. Frontier staff — существенно выше, но это вторичные агрегаторы, не offer letter.
- Команда/капитал/звёзды репо: ни один проверенный JD не требует h-index, звёзд GitHub или raised capital. Anthropic зовёт founders, не «raised $Xm».
- Аудитория контента: не порог, но Trivedi доказывает, что один точный пост бьёт холодную заявку.

Первая строка резюме, которая соответствует тому, как они сами описывают работу:
«Embedded with [тип клиента], turned an ambiguous operational problem into a production system operators used daily; measured [метрика]; upstreamed [паттерн] into the product.»
Не: «CTO / thought leader / passionate about AI». Не: «10x engineer». Не: «led a team of N». FDE IC нанимают за личный исход, не за span of control.

---

F. АНТИ-ПАТТЕРНЫ И ДИСКВАЛИФИКАТОРЫ

Режет на входе:
- Не пишешь код (или не можешь это доказать на таймере).
- Не ездишь.
- Нужна виза в контуре, где JD спрашивает «will you require sponsorship» и фирма не спонсирует (Cursor форма прямо спрашивает).
- «Почему FDE, а не SWE» без ответа. Правильный ответ: outcomes / customer / last mile, не «надоело кодить».
- Для Palantir: не можешь защитить работу с government.
- Для Anthropic: игнор safety.
- Для Cursor: не убедил технических лидов, убедил только закупки.

Выглядит как плюс, читается как минус:
- «Я CTO» на IC-роль без свежего личного кода и без готовности быть на площадке 50% — overqualified, won’t get dirty.
- «Я построил 40 агентов» без evals и без пользователя, который от этого зависит — hobby column (The Forward Deployed: это item 5, оффер на нём и ломается).
- MBA без кода после MBA.
- FAANG SWE, который никогда не говорил с клиентом.
- Long tenure в одной фирме без историй «я сел в чужой системе и починил» — лояльность ок, evidence embed нет.
- Крипто-бэкграунд без перевода на regulated enterprise ops (AML/KYC у Palantir — плюс; «мы делали токен» — шум).
- Слишком много «мы». FDE behavioural — I.
- Title-hunting: «хочу FDE потому что hottest job». Recruiter это слышит.

---

G. КАК ОЦЕНИВАЮТ (сводка лупов)

Типичный 2026 FDE loop: 5–8 стадий, 3–6 недель.

1. Recruiter 30 мин: why FDE, travel, comp.
2. HM 45–60: глубина прошлой работы, IC contribution.
3. Coding 45–60 live или take-home: messy data, API, retries. Palantir — графы, без AI. Labs — applied. Cursor/Ramp — AI allowed.
4. System/AI design 60: VPC, evals, rollback, cost.
5. Decomposition 45–60: высший вес, низкий pass.
6. Customer simulation 45: frustrated non-technical person.
7. Behavioral/values.

Кто решает. Обычно hiring manager FDE-функции + bar raiser из engineering. На founding ролях — основатель/VP Eng (Weinberg у Harvey, Jarvis у OpenAI, Brunet у Cursor).

Где валятся чаще всего (сходятся гайды, не внутренние dashboards — внутренних я не видел):
- Decomposition: решение до скоупа.
- Customer round: overpromise или inability to push back.
- Coding: не доводят до working + tests; или (на AI-разрешённых лупах) не могут объяснить, что сделала модель.
- Evidence: лучший проект — чатбот, не система, от которой зависели люди.

Длительность: OpenAI ~3 недели; Palantir 4–6; стартапы сжатее, иногда work trial 2 дня.

---

H. КОНВЕРТАЦИЯ НЕСТАНДАРТНОГО ОПЫТА (проверяемые переводы)

Failed founder → FDE: Het Trivedi. Перевод: не «мой стартап не взлетел», а «я научился деплоить OSS LLM, написал как, кофаундер инфраструктуры это прочитал».
Founder → Head of FDE: Leo Mehr, Sunny Rekhi. Перевод: «я уже жил в ambiguity и ownership, теперь тот же рефлекс на чужих enterprise-аккаунтах».
Non-CS + self-taught → Palantir FDE → Sierra lead: Natalie Meurer. Перевод: «выучила код, чтобы поставить safeguard на систему, которая трогает права людей, потом сидела с операторами».
Consulting/SA, не CS degree → Head of FDE OpenAI: Colin Jarvis. Перевод: «я уже доводил клиентов до production; нам нужна была функция, которая пишет на их инфре и кормит research».
PhD не-ML + applied DS в enterprise AI → OpenAI FDE: Manan Mehta. Перевод: engineering PhD + год в C3 на реальных клиентских данных, не «я исследователь».
10 years consulting AI deployments → Cursor Head of FDE: Pauline Brunet. Перевод: deployments, не decks; третья tech-компания как proof, что ушёл из чистого консалтинга.
Banking analyst → Accenture AI → Mudano → Tomoro co-founder → OpenAI DeployCo: Chris Spencer. Перевод: productionizing data/ML в банках, затем фирма, которая делает только last mile.
Неанглоязычный: Парижский офис OpenAI FDE — крупнейший (Fournier); Anthropic Paris JD требует native French. Акцент не барьер, если язык рабочего контура есть.
Без визы: O-1 / EU passport — прямой плюс, Cursor/большинство стартапов спрашивают sponsorship в форме.
Немолодой: публичный IC-след моложе; head-роли (Jarvis, Brunet, Meurer, Mehr) — mid-career. Не нашёл публичного примера 50-летнего new FDE IC в лаборатории. Не утверждаю, что их нет — не нашёл.
Крипто: Meurer в GSB хотела «нормы в крипте»; как FDE-вход это не сработало, сработал Palantir government + code. Не нашёл FDE-оффера 2024–26, где главный актив — «я из крипты».

---

I. ЧТО ЭТОМУ ЧЕЛОВЕКУ ИСКАТЬ В СВОЁМ ПРОШЛОМ

Сначала жёсткая сверка с портретом, без подгона.

Где профиль дотягивает (это активы, их надо достать из операционки, они не выглядят как FDE в резюме):
- PhD cryptography/systems, не ML — Palantir-совместимо (physics/math/systems классика). Лаборатории не требуют ML-PhD на FDE.
- 11 лет наёмным COO/CTO в одной крипто-компании + 9 лет в дистрибуции электроники — редкий домен: messy supply chain, regulated flows, партнёрства, капитал. Это «nouns and verbs» Palantir/Ganesh, если есть истории с пола склада / казначейства / ончейна, не с слайда борда.
- Python, C++, мульти-агентные системы в проде в паре с агентами — ровно 2026 FDE daily, ЕСЛИ можешь защитить архитектуру и починить без агента.
- Обучение разработчиков и фаундеров + PhD про обучение разработчиков — прямой актив для enablement-vintage (Meurer / Sierra University) и для Cursor (убедить eng org сменить workflow).
- O-1 + польский паспорт, Palo Alto / Sintra, готовность к релокации — снимает sponsorship-дисквалификатор.
- Длинный tenure, не прыгун — для клиента FDE это плюс (доживёшь до конца embed), для стартап-рекрутера — вопрос «а ты быстрый?».

Где профиль объективно не дотягивает, и чем это не закрыть дешёвой формулировкой:
1. IC-код с листа на таймере. 20 лет, из них 11 руководитель. Если live Palantir-экран (графы, без AI) не пишется в боевом темпе — Palantir FDSE закрыт. Замещение: не «я же с агентами». Замещение = 4–6 недель сверки навыка ИЛИ цель = Head/founding FDE/Technical Deployment Lead, где take-home и case весят больше, чем HackerRank. Если и take-home не пишется руками — роли нет.
2. Он не фаундер. Anthropic/Glean/Harvey в тексте зовут founder-like. Наёмный CTO с небольшим опционом ≠ founder. Замещать словом «founder» — ложь, её увидят. Замещение: конкретные 0→1 внутри Platinum (новый продукт, новый рынок, система, без которой операция встала бы), рассказанные как IC-исход, не как «я руководил».
3. Нет бренда Palantir/FAANG/applied-AI lab. Дверь будет холоднее. Замещение: реферал, публический артефакт (система, пост, доклад), доменный match (crypto/FS/supply chain reqs), O-1 как operational convenience.
4. Сеньорность vs IC. FDE IC в лаборатории часто 4–8 лет post-college. 20 лет карьеры на IC без управленческого мандата выглядит странно для обеих сторон: тебе мало денег и мало рычага, им — риск, что ты не будешь коммитить в чужой репо. Целевая полка: Staff FDE / Technical Deployment Lead / Head of FDE / founding FDE в Series B–D, не new-grad Palantir.
5. Крипто как единственный бренд. Без перевода в «regulated operations, custody, AML, partner integrations, capital raise as customer-discovery» это минус в BigLaw/defense и нейтрально в OpenAI FS. Не переводить = шум.
6. «Строю с агентами, сам с листа не пишу». На текущем рынке это дисквалификатор Palantir и высокий риск OpenAI/Anthropic walkthrough. На Cursor/Ramp это проходимо только при доказанном engineering judgment. Замещать нечем, кроме реальной тренировки пустого файла.

Не льстить: этот профиль ближе к founding/staff FDE или Head of deployment в AI-стартапе/лаборатории, чем к классическому Palantir Delta 3 лет из колледжа. Плюс, которого нет у большинства IC, — операционный масштаб, капитал, партнёрства, обучение людей, право работать в US без sponsorship, двуконтинентальность. Минус, которого нет у большинства IC, — давно не быть человеком, который в чужом CodePair пишет граф.

Вопросы-подсказки. Положительный ответ на каждый закрывает элемент портрета, который в резюме обычно не пишут:

1. Вспомни случай, когда ты лично, не через подчинённого, сел в чужую продовую систему (склад, кастоди, биржа, банк-партнёр) и к концу недели у оператора что-то изменилось в работе. Что именно ты написал руками.
2. Вспомни, когда данные клиента оказались дырявыми (пустой timestamp, две правды в двух системах, Excel как data quality), и твоя красивая архитектура умерла. Что ты сделал в ту ночь.
3. Вспомни, когда бизнес попросил «сделай X», а ты сказал нет, потому что X не двигало метрику, и предложил меньший кусок, который можно было проверить за дни. Чем кончилось.
4. Вспомни, когда ты объяснял технический отказ нетехнарскому VP / регулятору / партнёру так, что они кивнули на риск в деньгах, не на архитектуру.
5. Вспомни систему, которую ты сделал для одного клиента/отдела, и которую потом без тебя использовали другие. Что ты обобщил, что выбросил.
6. Вспомни, когда ты учил взрослых инженеров или фаундеров делать работу по-новому, они сопротивлялись, и ты довёл до adoption. Какая была единица сопротивления (как parquet-viewer у Ganesh).
7. Вспомни инцидент, где ты был на pager’е сам, не «команда дежурила». Что чинил, сколько длилось, как предотвращал повтор.
8. Вспомни интеграцию с легаси, у которой не было API, только nightly file / SOAP / человек на почте. Как сделал идемпотентность.
9. Вспомни, когда модель/агент/скрипт врал в проде, и ты построил eval или golden set, а не «подкрутил промпт». Какая была метрика до/после.
10. Вспомни поездку, где ты работал не в офисе, а на площадке (склад Merlion, клиент, биржа, конференция с руками в системе). Без этой поездки решение было бы другим?
11. Вспомни привлечение капитала или партнёрство, где ты был не «присутствовал», а лично закрывал техническую due diligence с той стороны стола. Какие три вопроса они задали, на которые слайд не отвечал.
12. Вспомни момент, когда ты был единственным горлом: если ты не сделаешь, сделка/релиз/лицензия сорвётся. Что сделал своими руками в последние 48 часов.
13. Вспомни, как ты за неделю выучил чужой стек достаточно, чтобы поменять поведение системы, не переписывая её. Что читал, что ломал, что оставил.
14. Вспомни конфликт с сильным инженером или вендором, где ты удержал техническую линию и не потерял отношение. Формулировка отказа.
15. Вспомни артефакт (репо, внутренний инструмент, курс, препринт, патч), который жил после тебя. Кто им пользуется сейчас. Если такого нет — это дыра evidence, её не закроет PhD.

Если на 1, 2, 7, 9, 12 честный ответ «это делали другие, я ставил задачу» — IC FDE в Palantir/OpenAI/Cursor сейчас не твоя дверь. Тогда дверь = Head of FDE / Technical Deployment Lead / founding deployment в стартапе, и в первой строке резюме должно быть «I built and ran the function that got customers to production», не «I am a CTO looking to go back to IC». Если на эти вопросы есть живые «я» — кодируй их как FDE-исходы и целься в founding/staff FDE лабораторий и вертикалей (OpenAI DeployCo, Anthropic Applied AI, Harvey/Glean founding, Cursor FDE, Ramp), Palantir — только после того, как пустой файл снова слушается.

---

J. ИСТОЧНИКИ (открыты в этом прогоне)

Первичные / почти первичные:
- https://openai.com/index/openai-launches-the-deployment-company/
- https://x.com/colintjarvis/status/1879532522956329135 (15.01.2025, основание FDE OpenAI)
- https://www.youtube.com/watch?v=cBD7_R-Cizg (Jarvis, Altimeter, 26.11.2025)
- https://newsletter.pragmaticengineer.com/p/forward-deployed-engineers (Orosz, 12.08.2025; Jarvis, Mehr, Kanekar)
- https://newsletter.eng-leadership.com/p/inside-openais-forward-deployed-engineer (15.08.2026; org ~140)
- https://www.ft.com/content/91002071-7874-4cb7-9245-08ca0571c408 (02.11.2025; Fournier, de Jong, Prettejohn, +800%)
- https://www.journaldunet.com/intelligence-artificielle/1545345-arnaud-fournier/ (17.10.2025)
- https://www.lemonde.fr/economie/article/2026/05/13/ia-les-ingenieurs-missionnes-chez-les-clients-nouveau-front-de-la-bataille-entre-openai-anthropic-ou-mistral_6688673_3234.html
- https://www.latent.space/p/forward-deployed-engineer-best-practices (Ganesh, 12.09.2026)
- https://www.youtube.com/watch?v=Byv311hdoHE (Meurer, 28.07.2026)
- https://www.theforwarddeployed.io/interviews/palantir
- https://www.theforwarddeployed.io/are-you-ready
- https://cursor.com/careers/forward-deployed-engineer-australia-japan-singapore
- https://jobs.ashbyhq.com/harvey/24f97125-c406-4ae3-9f94-07dd1310e511
- https://www.linkedin.com/posts/winston-weinberg_more-than-ever-harvey-customers-and-prospects-activity-7401624951067508737-GY1e
- https://www.linkedin.com/posts/mananm2_im-thrilled-to-share-that-ive-joined-openai-activity-7383642758206603266-k1zY
- https://www.linkedin.com/posts/colin-jarvis-50019658_its-been-a-big-year-for-openai-fde-as-we-activity-7401596375555153920-nkCl
- https://www.linkedin.com/jobs/view/forward-deployed-engineer-at-anthropic-4360027629
- https://raw.githubusercontent.com/aishwaryanr/awesome-generative-ai-guide/main/interview_prep/roles/forward-deployed-engineer/README.md
- https://joinplank.com/fde-job-market (census 12.09.2026)
- https://www.youtube.com/watch?v=7wu2hsRfvV0 (Rekhi, Decagon)
- https://www.youtube.com/watch?v=ITMXwI6QL6A (Mehr, Ramp)
- https://www.fdehub.org/p/explore-then-exploit-a-conversation (Mehr)
- https://medium.com/@het.trivedi05/what-i-learned-as-a-forward-deployed-engineer-working-at-an-ai-startup-6046e0c7e1fe
- Palantir FDSE posting via startup.jobs (роль = startup CTO)
- Modal FDE-Systems via builtinsf.com
- Scale AI / Glean postings via welcometothejungle

Вторичные, использовал осторожно (маркетинг/агрегаторы, не offer letters): Perspective AI «survey of 1500 FDEs», GSDC 16-company PDF, Aced/Exponent, fdenest, fdeinterviews, Recruiting from Scratch. Comp staff-уровня $560k–$785k — из этих агрегаторов, не из primary offer.

Не нашёл (явный список пробелов, не «этого нет»):
- 12–20 именных IC FDE, нанятых Palantir / Anthropic / Harvey / Glean / Scale / Modal / Sierra за 24 месяца, с проверяемыми CV. Открытый слой — лидеры, founding announcements, org-photo OpenAI, acquihire Tomoro.
- Внутренние pass-rate Palantir/OpenAI (цифры ~40% на decomposition — из prep-гайдов, не из компаний).
- Полный публичный interview loop Harvey, Glean, Scale, Sierra, Modal, Decagon.
- Подтверждённый пример наёмного не-фаундера CTO крипто-компании → IC FDE в top-lab за 24 месяца.
- Точный headcount Harvey FDE (оценка 40–80 — не disclosure).
- Живой FT full-text: пейволл отдал 403, цитаты сверлены по Moneycontrol/eWeek пересказам + сниппет поиска.

🎙 Майк: идём верно, если цель — founding/staff FDE или Head of deployment, не «вернусь в Palantir как Delta из колледжа». Неверно, если ставка на title FDE как на hottest job без живого пустого файла. Вопрос, который решает всё: когда ты в последний раз в чужом репозитории, без своей команды, сам довёл систему до состояния, в котором оператор от неё зависит — и можешь это рассказать на 45 минут с деталями, которые выдумавший не выдержит?

🧒 Простыми словами: работу придумали в Palantir: инженера сажают к клиенту, чтобы штуковина заработала, а не чтобы нарисовать слайд. Сейчас так делают все AI-компании, потому что модель умная, а в бухгалтерии клиента всё равно хаос. На собеседовании у старой школы Palantir надо уметь писать код руками, как диктант. У новых — можно звать робота на помощь, но если робот соврал, ты должен это увидеть. Человек, который 11 лет был большим начальником и сам давно не чинил трубы, на эту работу «рядовым сантехником» не пройдёт. Ему надо целиться в старшего, который учит сантехников и сам всё ещё может залезть под раковину.
