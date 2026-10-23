# DR26-09-14-ZB-20-2206 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

[⏰ среда, ночь, 00:2X, 16 сентября 2026]

# КОГО РЕАЛЬНО БЕРУТ В FORWARD-DEPLOYED ENGINEER И РОДСТВЕННЫЕ РОЛИ
### Palantir как эталон + его копии в AI (OpenAI, Anthropic, Sierra, Harvey, Decagon, Glean, Scale, Cursor, Modal), 2024–2026

Замечание о методе: ниже — только то, что открыто и проверено по живым источникам. Именных FDE в лабораториях почти никто не анонсирует пресс-релизом, а LinkedIn при машинном фетче отдаёт блок (HTTP 999) — поэтому часть имён помечена «заголовок из выдачи, профиль не открыл». Где не подтвердил — пишу «НЕ НАШЁЛ», а не «этого нет».

---

## 1. КОГО РЕАЛЬНО БЕРУТ — конкретные живые люди

**Ядро жанра — Palantir и его алумни (проверенные FDE):**

1. **Nabeel Qureshi** — FDE в Palantir 2015–2023 (8 лет), работал в т.ч. над проектом Airbus; ушёл в писатели/фаундеры, автор канонического эссе «Reflections on Palantir». https://nabeelqu.substack.com/p/reflections-on-palantir · https://www.lennysnewsletter.com/p/inside-palantir-nabeel-qureshi
2. **Bob McGrew** — ранний инженер PayPal → ~10 лет в Palantir (вёл engineering/product, первые продукты для разведки) → Chief Research Officer OpenAI (ChatGPT, GPT-4, o1). Джо Лонсдейл: «He was in the room when the FDE model was invented». https://blog.joelonsdale.com/p/ep-66-bob-mcgrew-the-superstar-palantir · https://www.ycombinator.com/library/Mt-the-fde-playbook-for-ai-startups-with-bob-mcgrew
3. **Shyam Sankar** — сотрудник Palantir №13, фактически изобрёл роль FDE, ныне CTO. https://www.shyamsankar.com/about
4. **Zach Romanow** (+ **Jesse Rickard, Pete Mills, Samuel Tarng**) — Palantir-алумни, основали **Fourth Age** — компанию, которая сама поставляет forward-deployed инженерию клиентам Palantir. https://finance.yahoo.com/news/9-startups-palantir-alumni-know-110000540.html
5. **Nick Perry, Lekan Wang, Andrew Girvin** — ранние FDE Palantir (healthcare-вертикаль), названы в эссе Куреши. https://nabeelqu.substack.com/p/reflections-on-palantir
6. **Trae Stephens** + **Brian Schimpf** + **Matt Grimm** — ранние Palantir (Стефенс вёл intelligence/defense-рост; Шимпф — engineering director) → соосновали/возглавили **Anduril**. (FDE-титул конкретно у Стефенса НЕ подтверждён — «early employee».) https://foundersfund.com/team/trae-stephens/ · https://en.wikipedia.org/wiki/Anduril_Industries

**AI-лаборатории (проверенные переходы Palantir → лаборатория):**

7. **Kevin Bai** — был первым хайром, масштабировавшим FDE-функцию в Palantir; затем founding FDE в Rippling (вырастил до ~25 чел. за год); сейчас **Applied AI @ Anthropic**. Его определение: «FDE — это customer-facing software engineer… they build on top of a platform, never writing software from scratch». https://finance.biggo.com/podcast/cb47ec147e982d4d
8. **Anish Bikmal** — «Applied AI @ Anthropic | Ex-Palantir» (в Palantir — FDE на Hospitals team). Заголовок из выдачи; профиль не открыл. https://www.linkedin.com/in/anishbikmal/
9. **Tanveer Mittal** — «Applied AI @ Anthropic | Ex-Palantir». Заголовок из выдачи. https://www.linkedin.com/in/tanveermittal/
10. **Colin Jarvis** — Global Head of Forward Deployed Engineering в OpenAI; строит и масштабирует FDE-функцию (по публичным данным команда ~39 инженеров с планом до 52). https://theorg.com/org/openai/org-chart/colin-jarvis · https://newsletter.eng-leadership.com/p/inside-openais-forward-deployed-engineer
11. **Kanav Bhatnagar**, 24 — Senior FDE в **Rippling** (ex-Amazon, ex-Actively AI); герой статьи Business Insider «Мне 24, и у меня самая горячая работа». https://www.aol.com/articles/im-24-old-hottest-job-095901698.html

**AI-стартапы:**

12. **Sunny Rekhi** — **CTO of Forward Deployed Engineering** в Decagon; выступал с докладом «How Forward Deployed Engineering is done at Decagon». https://www.youtube.com/watch?v=7wu2hsRfvV0 · https://www.zenml.io/llmops-database/scaling-forward-deployment-engineering-for-ai-customer-service-agents
13. **Katherine Xiao** — Director of Implementations в Decagon, автор поста о редизайне forward-deployment в «Agent Development». https://decagon.ai/blog/how-decagon-is-redefining-forward-deployment
14. **Jesse Zhang** (CEO) + **Janelle Sallenave** (COO) — Decagon; Zhang публично отвергает «Palantir-style forward-deployed» модель в пользу self-serve. https://decagon.ai/blog/build-the-future-you-want
15. **Anique Drumright** — Chief Product Officer Harvey; публично описала модель «бывший юрист в каждом деплое» (legal engineers, ~180 чел., OTE $220–320k). https://cloud.substack.com/p/harvey-puts-a-former-practicing-lawyer
16. **Clay Bavor** (+ **Bret Taylor**) — соосновали Sierra; Бавор ввёл роль «AI Architect» (доклад «Rise of the AI Architect», AI Engineer World's Fair 2025). https://ai.engineer/speakers/clay-bavor

**Не-канонический вход (важнейшие живые кейсы):**

17. **Het Trivedi** — **провалившийся фаундер → FDE в Baseten**: год делал свой стартап, PMF не нашёл, начал строить side-проекты на GPT/RAG, вёл блог — и его нашёл сооснователь Baseten по этим постам. https://www.baseten.co/blog/what-i-learned-as-a-forward-deployed-engineer-working-at-an-ai-startup/
18. **Andrew Stroup** — forward-deployed из НЕ-техно-истории: в 20 встроен в оборонную fluid-power фирму в Оклахоме, позже — в химбио-оборону Пентагона, затем фаундер Leverage AI. Тезис: «forward-deployed был дефолтом в обороне/тяжпроме/госсекторе десятилетиями до того, как Кремниевая долина дала этому имя». https://stroupaloop.com/blog/2026/05/16/forward-deployed-didnt-start-in-tech/
19. **Blake Hall** (ID.me) — армейский рейнджер, командовал патрулями в Ираке до того, как строить; назван Строупом как forward-deployed архетип. Тот же URL.
20. **Pablo Felgueres** — «Forward Deployed Engineering» в Harvey (заголовок LinkedIn из выдачи; профиль заблокирован, не подтвердил). https://www.linkedin.com/in/pablofelgueres/

Итого: **~20 конкретных людей**, из них 6–8 — с полностью открытыми первоисточниками, остальные — с частично проверяемыми (LinkedIn-заголовок/подкаст-био).

---

## 2. ПРЕДЫДУЩИЙ ОПЫТ — что реально сработало

**Последняя роль до найма.** По разбору 1000 вакансий (bloomberry): в FDE приходят из SWE (45%), Solutions/Sales Engineering (22%), Data Eng/DS (15%), тех-консалтинга (10%), фаундеров/ранних сотрудников (8%). https://bloomberry.com/blog/i-analyzed-1000-forward-deployed-engineer-jobs-what-i-learned/

**Формальное образование.** CS — НЕ обязателен. Вакансия Palantir FDSE (verbatim): «Strong engineering background, preferred in fields such as **Computer Science, Mathematics, Software Engineering, Physics, and Data Science**» + всего «1+ years of post-college work experience». https://jobs.lever.co/palantir/dab396d4-2f14-4796-aac0-0d82883dccf0 · Anthropic FDE: «Bachelor's degree **or an equivalent combination of education, training, and/or experience**». https://job-boards.greenhouse.io/anthropic/jobs/5302966008

**Доменный опыт.** Ценится ВЫШЕ, чем алгоритмы: тяжёлые индустрии (финансы 24%, госсектор/оборона 18%, healthcare 17%, страхование 17%, энергетика 13% из bloomberry). У Куреши коллегами были «бывший SWAT-коп из Огайо», «армейский ветеран», «человек, 15 лет проработавший в разведке». https://nabeelqu.substack.com/p/reflections-on-palantir

**Публичный артефакт.** Часто — сама дверь: Trivedi вошёл через блог; Куреши/Строуп/Бавор известны через эссе и доклады. First Round: сильные FDE — «compulsive builders… whether it's building a tool, releasing some app, or contributing to open-source». https://review.firstround.com/so-you-want-to-hire-a-forward-deployed-engineer/

**«Странный» опыт как актив.** Военные, разведка, тяжпром, нефтедобыча (Thomas Lee Young — вырос у нефтевышек в Тринидаде, работал на линии Jaguar Land Rover). https://stroupaloop.com/blog/2026/05/16/forward-deployed-didnt-start-in-tech/ · Palantir шлёт новичкам **книги по философии** вместе с тех-материалами и смотрит на **спорт/шахматы** как сигнал. https://www.lennysnewsletter.com/p/inside-palantir-nabeel-qureshi

**Сколько без канонического пути:** структурно — большинство. Ни одна из проверенных вакансий (Palantir, OpenAI, Anthropic, Harvey, Glean, Cursor, Scale) не требует ML-PhD; несколько прямо приглашают фаундеров и не-CS. Это, вероятно, наименее credential-gated старшая техническая роль в индустрии.

---

## 3. ПОВТОРЯЮЩИЕСЯ ПАТТЕРНЫ

**(а) Паттерн-СИГНАЛ (что доказывает компетенцию) — есть у >50% взятых:**
- «Owned outcomes under ambiguity» — владение результатом там, где спеки не было. Paraform (OpenAI FDE): ключевой сигнал — «you've owned outcomes in ambiguous environments where the spec didn't exist upfront, beyond feature work inside a structured roadmap». https://www.paraform.com/blog/openai-forward-deployed-engineer
- Production-код, а не прототип (Python в 66% вакансий, TypeScript 35%, AI-агенты 35%, LLM 31% — bloomberry).
- Клиент-фейсинг + перевод для не-технического стейкхолдера. У Sundeep Teki: Customer Obsession (30%) + Communication (25%) = **55% решения**, «far outweighing pure technical depth». https://www.sundeepteki.org/advice/forward-deployed-ai-engineer
- Способность разложить неоднозначную задачу (Palantir Decomposition-раунд).

**(б) Паттерн-ДВЕРЬ (как физически вошли):**
- Реферал / alumni-сеть (Palantir-мафия — самый мощный канал: 111+ компаний, $11.6B поднятого капитала алумни). https://finance.yahoo.com/news/9-startups-palantir-alumni-know-110000540.html
- Публичный контент/OSS как приманка (Trivedi — блог; докладчики на AI Engineer WF).
- Прямой job-post (роль сейчас в открытом найме почти везде).
- Acqui-hire целой команды: OpenAI купила британскую **Tomoro** (~150 FDE) как ядро «OpenAI Deployment Company». https://x.com/OpenAI/status/2053824999736410415 · https://www.cooley.com/news/coverage/2026/2026-05-12-openai-forms-new-joint-venture-openai-deployment-company-and-acquires-tomoro

**(в) Паттерн-ТАЙМИНГА:** взрывной. Рост вакансий +1,165% г/г (bloomberry, окт-2025 — рекорд); отдельно Exponent: с 643 (апр-2025) до 5,330 (апр-2026), +729%. Спрос сильно опережает предложение — «pool is small, geographically concentrated, and not where you'd expect». Вход сейчас = окно.

---

## 4. СОБИРАТЕЛЬНЫЙ ПОРТРЕТ ИДЕАЛЬНОГО КАНДИДАТА

**One-liner:** «Инженер-владелец, который садится внутрь клиента, за дни собирает работающую систему на нашей платформе под неоднозначную реальную задачу, завоёвывает доверие людей на самом верху и переводит их бизнес в софт — и относит выученное обратно в продукт».

**5–7 обязательных элементов бэкграунда:**
1. Умеет писать и **владеть** production-кодом (Python обязателен; +1 язык — TypeScript/Java/C++).
2. Реальный shipped-опыт с LLM/агентами (не демо): retrieval, agents, tool use, evals, MCP-серверы/sub-agents.
3. Клиент-фейсинг послужной список: работал напрямую с заказчиком/стейкхолдером и отвечал за исход.
4. Доказанное «ownership под неоднозначностью» без готового плейбука.
5. Готовность к разъездам (68% ролей требуют travel, часто 25–50%).
6. Быстрое освоение незнакомого домена/инструмента «за ночь».
7. Мотивация к миссии/домену компании (Palantir/Anthropic режут на этом рано).

**3–5 «сильно желательных»:**
- Бывший фаундер ИЛИ первый/ранний инженер, «строивший с нуля» (у OpenAI и Anthropic — прямой «plus»).
- Доменная глубина в тяжёлой индустрии (финансы/оборона/healthcare/крипта).
- Публичный артефакт (репо/блог/доклад).
- Опыт наставничества/обучения (FDE онбордит и «включает» команды клиента).

**Измеримые пороги (из вакансий):** 4–7+ лет опыта (mid-level 60% рынка — 3–5 лет; senior 20% — 6–8 лет); Python в 66%; travel 50–75% в 68% ролей. Комп: median база ~$174k (bloomberry); frontier-лаборатории — TC mid $300–450k, senior $500k+, staff/principal $600k–$1.2M+ (paraform, getperspective). Palantir FDE по Levels.fyi: $135k–$631k, median TC ~$278k. https://www.levels.fyi/companies/palantir/salaries/software-engineer/title/forward-deployed-engineer

**Как формулирует себя в первой строке резюме:** не «Python/C++ engineer», а «Forward-deployed builder: I embed with customers and ship production AI/agent systems that solve their hardest, undefined problems — then productize the win». Ведёт с **владением исходом + отгрузкой агентов**, не с дипломом.

---

## 5. АНТИ-ПАТТЕРНЫ И ДИСКВАЛИФИКАТОРЫ

- **10+ лет в FAANG-комфорте.** First Round (Shilpa Balaji, Palantir): такие — «in the no fly zone», нужен «fresh pair of eyes without established playbooks or dogmatic approaches». Это НЕ про возраст — про закостенелость плейбука. https://review.firstround.com/so-you-want-to-hire-a-forward-deployed-engineer/
- **«Perfect LeetCode record + ноль клиент-фейсинга».** Прямой красный флаг там же.
- **Чистый back-end без коммуникации** (вакансии прямо «screen out backend-only engineers without customer communication skills»).
- **Квота/продажник в маскировке.** Ноль FDE-вакансий несут sales-квоту; если «хочу, чтобы FDE закрыл N сделок» — это не FDE (First Round; bloomberry: 0% quota-carrying).
- **Не пройдёт технический loop.** Tiffany Siu (Palantir): FDE «had to pass the same interview loops as software engineers» — планка кода реальна.

**Выглядит плюсом, читается минусом:**
- **«Я всё делаю через ИИ-агента».** На интервью Palantir ИИ **прямо запрещён**; OpenAI/Anthropic держат живые coding-раунды. Если это твой единственный инструмент — на оценке ты без него.
- **Слишком сеньорный/«переросший».** «Overqualified» пугает не титулом, а сигналом «будет спорить с плейбуком/не поедет в поле».
- **Академизм без отгрузки.** PhD-регалии без production-артефакта читаются как «теоретик».

---

## 6. КАК ОЦЕНИВАЮТ — реальный процесс и где валятся

**Palantir FDSE (эталон; ИИ на интервью ЗАПРЕЩЁН):** recruiter call (мотивация/миссия) → technical screen (live coding в CodePair/Karat ИЛИ HackerRank: задача + SQL + API) → onsite 3×60 мин из пула {**Decomposition**, Learning, Coding, Re-engineering, System design} → hiring manager. Ключевое:
- **Decomposition** («самый важный раунд») — расплывчатая реальная задача (классика с Blind: «8000 записей о лондонских такси — набросай компоненты, API, разверни за неделю»); **кода часто не пишут**, псевдокод. Здесь валятся чаще всего.
- **Re-engineering** — дебаг чужого Python-кода по гипотезам вручную.
- Бар: «LeetCode prep isn't going to be enough here» (interviewing.io), сложность ≈ medium/hard, обёрнутая в сценарий. Отклоняют сильных технически, «когда не могут структурировать неоднозначность». https://www.tryexponent.com/guides/palantir-forward-deployed-engineer-interview · https://interviewing.io/palantir-interview-questions · https://www.teamblind.com/post/palantir-on-site-fdse-interviews-mwvoeyly
- **OpenAI FDE:** ~5-часовой **take-home** «построй реальное на наших API — RAG/агент/eval-harness» + walkthrough, где дизайн evals — различитель; loop 3–5 недель, ~50% веса на кейсы/эмпатию к клиенту/бизнес-суждение. База по зеркалу вакансии «220K–280K» + PPU, до 50% travel. https://builtin.com/job/forward-deployed-engineer-sf/4457354 · https://www.tryexponent.com/guides/openai-forward-deployed-engineer-interview
- **Anthropic Applied AI/FDE:** ~90-мин CodeSignal-стиль (rate limiter, streaming, distributed job queue, LLM system design) + отдельный **customer-discovery** скрин (без редактора: как оценивать покупателя, разбор проваленных ИИ-деплойментов) + make-or-break **values round**. База «$280,000–$320,000», ~25% travel, сдаёт «MCP servers, sub-agents, agent skills». https://job-boards.greenhouse.io/anthropic/jobs/5302966008 · https://getperspective.ai/blog/anthropic-applied-ai-engineer-interview-process-frontier-lab-2026
- **Sierra:** дизайн агентных систем, customer-service evals, разговорная архитектура; Forward Deployed Infrastructure Engineer — планка стажем (4+ лет infra: AWS/Terraform/Go). https://jobs.ashbyhq.com/Sierra/d9c0aa93-e35d-4752-9cef-4c39dcad5365
- **Harvey:** eng-FDE — 2+ года production + 0→1 ownership + LLM-приложения ($165–200k); отдельная дорожка legal engineer — JD + 3+ года практики.
- **Decagon:** бар «равен product-engineering»; дефицитный навык теперь — «**restraint**… treat yourself as an adviser rather than an executor» (т.к. ИИ-кодинг стал так хорош). https://www.zenml.io/llmops-database/scaling-forward-deployment-engineering-for-ai-customer-service-agents
- **Glean/Scale/Cursor/Modal:** первоисточник — вакансии. Glean Founding FDE $160–270k, 25–50% travel, 4+ года client-facing + 0→1 shipped + агенты. https://job-boards.greenhouse.io/gleanwork/jobs/4651991005 · Scale FDE $180–225k, Python + distributed + K8s. https://scale.com/careers/4694861005 · Cursor FDE 5+ лет Python+JS/TS, «built and owned AI-native workflows in production, not just prototypes», «not a demo role». https://cursor.com/careers/forward-deployed-engineer-new-york-remote-san-francisco · Modal — FDE ML/Systems (инфра-уровень SWE). https://www.builtinnyc.com/job/forward-deployed-ml-engineer/4759015

**Кто принимает решение:** hiring manager + петля инженеров; в лабораториях values/mission round имеет право вето. **Где чаще валятся:** decomposition/структурирование неоднозначности; дизайн evals; и — для «строителей через ИИ» — живой coding/re-engineering без ассистента.

**Новый сдвиг 2025–2026 (важно для заказчика):** индустрия начала **разрешать ИИ-инструменты внутри интервью** и оценивать именно «направляешь и **проверяешь** ИИ, а не слепо принимаешь его вывод»; сильнейший позитивный сигнал — «отклонил правдоподобно-неверную подсказку ИИ», провал — «вставил код, который не можешь защитить». Это подтверждено как индустриальный тренд (Meta с конца 2025, Shopify, Rippling, LinkedIn), но НЕ подтверждено конкретно для FDE-петель Palantir/OpenAI/Anthropic. https://pickuma.com/for-dev/ai-coding-tools-in-interviews-2026/

---

## ГЛАВНЫЙ ВОПРОС: пройдёт ли тот, кто строит системы через ИИ-агентов, но не пишет «с листа»?

Честный вердикт по проверенному:
- **Palantir — самый рискованный для такого профиля.** ИИ на интервью запрещён; есть живой coding + re-engineering (ручной дебаг чужого кода). Но **Decomposition — где кода почти нет** — как раз твоя сильная зона. Итог: споткнёшься с высокой вероятностью на ручном coding-раунде, силён в декомпозиции/дизайне. Декомпозиции одной **недостаточно** — ручной coding остаётся жёстким фильтром, и запрет ИИ его специально защищает.
- **AI-native shops (OpenAI/Anthropic/Sierra/Decagon/Cursor/Glean) — заметно дружелюбнее.** Их take-home/оценка буквально = «построй агент/RAG/eval на наших API» — это ровно «строю работающие системы». Decagon прямо говорит, что «restraint» и роль «адвайзера», а не «executor», — теперь дефицитный навык. И общий тренд — учить «направляй+проверяй ИИ» — играет в твою пользу.
- Единственная непокрытая зона везде — **раунд, где надо на глазах защитить код построчно без ассистента**. Прямого источника «ИИ-строитель проходит/не проходит» я **НЕ НАШЁЛ** — это вывод из механики интервью, не цитата.

---

## Сколько в FDE от инженера и сколько от консультанта — и как сдвинули агенты

- Базовый расклад (Gergely Orosz о Google FDE): «~25% coding-related, 50% integration/plumbing, 25% meetings». https://blog.pragmaticengineer.com/the-pulse-forward-deployed-engineering-heats-up-again/
- «Три шляпы»: consultant + PM + software engineer, но **ядро — инженерное и без sales-квоты** (Exponent; bloomberry: 0% quota). Куреши: критичны «unusual sensitivity to social context», «partner at the highest level and gain their trust», «speak the customer's language».
- a16z: Palantir переименовал «solutions/integration engineers» в FDE, подняв статус и отобрав «high-agency, high-EQ» инженеров. https://a16z.com/forward-deployed-job-titles/ · Vinoo Ganesh: если полевой сигнал не возвращается в продукт — «that's consulting… it pays well, it doesn't compound». https://www.latent.space/p/forward-deployed-engineer-best-practices
- **Сдвиг с приходом агентов (2025–2026):** Jake Stauch (First Round): «capabilities are no longer the rate-limiting step… somebody has to steer the product». EPAM строит практику из 10,000 Claude-архитекторов и **250 forward-deployed «Black Belts»**, где «один FDE закрывает работу, требовавшую команды из пяти». https://www.freecodecamp.org/news/agentic-ai-engineering-in-practice-how-to-build-with-claude-code-codex-and-gemini/ · Decagon срезал кастомную инженерию на агента на **80%**. Направление: агенты пишут больше кода → человек-FDE смещается вверх, к переводчику/владельцу продукта/дискавери. Строгой цифры «до/после» ratio я **НЕ НАШЁЛ** — тренд заявлен направленно.

---

## Как читается опыт «CTO собственной компании» на FDE-найме

Асимметрично, и это прямо касается заказчика (важная оговорка ниже):
- **Актив (сильный).** Роль явно фреймится как founder-эквивалент. First Round: FDE «recreate the early days of a startup when it's a couple founders asking customers 'what do you want?'»; вакансии OpenAI/Anthropic прямо зовут бывших фаундеров/ранних инженеров. Владение исходом + стейкхолдер-менеджмент = 55% решения по модели Teki.
- **Пассив (специфический).** Не «фаундер/не-кодер», а **закостенелость и догматизм** + **рустость по ручному коду**. «CTO своей компании» читается минусом ТОЛЬКО если не проходит тех-loop или несёт готовый плейбук.
- ⚠️ **Точность про заказчика:** он **НЕ фаундер** — 11 лет наёмный COO/CTO в крипто-компании (Platinum) с небольшим опционом, до этого 9 лет в дистрибуции (Merlion). Для FDE это читается как **оператор-владелец исхода долгого горизонта**, что желательно, — но подавать себя «фаундером» нельзя (это неправда и легко проверяется). Правильная рамка: «operator/executive who owned P&L, partnerships and delivery under ambiguity for a decade», а не «founder». Long-tenure (11 и 9 лет) — двойной сигнал: против «прыгуна», но следи, чтобы не прочли как «one-playbook comfort» — крыть свежими shipped-агентами.

---

## 7. КОНВЕРТАЦИЯ НЕСТАНДАРТНОГО ОПЫТА — какими переводами

Проверенные «переводы формулировок» (совпадают с реальным языком рекрутеров):
- **Провалившийся фаундер →** «I've owned outcomes in ambiguous environments where the spec didn't exist» (Paraform). Живой кейс: Het Trivedi (провал стартапа → блог → FDE Baseten). https://www.paraform.com/blog/openai-forward-deployed-engineer
- **Консультант / оператор →** «customer engagement + stakeholder management» + «eloquence, clarity, comfort leading a customer meeting» (Teki цитирует критерий Palantir). https://www.sundeepteki.org/advice/forward-deployed-ai-engineer
- **Фаундер/CTO →** «extreme ownership + ship PoCs in days». Не-ML PhD → «research rigor, problem decomposition, rapid cross-domain learning» (context-switching ~25% веса).
- **Крипта →** прямая дверь: **Coinbase** держит FDE в Office of the CTO (страница отдала 403, листинг существует: https://www.coinbase.com/careers/positions/8099099); крипто-фирмы вроде **Axiom** нанимают FDE, а их фаундеры — не-CS матфизики (Yi Sun, Jonathan Wang — math PhD), что доказывает: не-CS-PhD штатно укомплектовывают эти команды (из выдачи, первоисточник IACR не открыл).
- **Не-англоязычный / без визы:** визы спонсируют явно — Anthropic: «We do sponsor visas… every reasonable effort»; вакансия ClarityCare: «H-1B and O-1 sponsorship available», без требования диплома. У заказчика **O-1 + польский (ЕС) паспорт = спонсорство НЕ нужно** — это чистый актив, снимающий целый класс отказов. https://jaabz.com/jobs/209240-forward-deployed-engineer-a-claritycare-ai
- **Немолодой:** документированной возрастной дискриминации в FDE-найме я **НЕ НАШЁЛ** — сигналы «7+ лет / бывший фаундер / 15-летний ветеран агентства» читаются нейтрально-положительно. Это вывод из отсутствия свидетельств, не доказанный факт.
- **Не-каноничный вход как норма:** Palantir — «artist colony, not a factory» (Sankar); Карп: «I like to meet candidates with no data about them: no résumé». Профиль пула у ex-Palantir Jack&Jill: «former founders and CTOs, published academics, operators from hypergrowth companies». https://www.jackandjill.ai/about-us

Чего **НЕ НАШЁЛ**: конкретного именного профиля McKinsey/BCG → Palantir FDE (категория подтверждена, человек не открыт); именного FDE в самом OpenAI с публичным профилем; имени главы Anthropic Applied AI.

---

## 8. ИСТОЧНИКИ (полные URL, открытые в этом прогоне)

Palantir/канон: https://nabeelqu.substack.com/p/reflections-on-palantir · https://www.lennysnewsletter.com/p/inside-palantir-nabeel-qureshi · https://jobs.lever.co/palantir/dab396d4-2f14-4796-aac0-0d82883dccf0 · https://www.tryexponent.com/guides/palantir-forward-deployed-engineer-interview · https://interviewing.io/palantir-interview-questions · https://www.teamblind.com/post/palantir-on-site-fdse-interviews-mwvoeyly · https://vibeengines.com/handbook/the-fde-canon · https://en.wikipedia.org/wiki/Forward_Deployed_Engineer · https://www.shyamsankar.com/about · https://blog.joelonsdale.com/p/ep-66-bob-mcgrew-the-superstar-palantir · https://finance.yahoo.com/news/9-startups-palantir-alumni-know-110000540.html · https://foundersfund.com/team/trae-stephens/ · https://en.wikipedia.org/wiki/Anduril_Industries · https://www.levels.fyi/companies/palantir/salaries/software-engineer/title/forward-deployed-engineer

Рынок/аналитика: https://bloomberry.com/blog/i-analyzed-1000-forward-deployed-engineer-jobs-what-i-learned/ · https://www.paraform.com/insights/forward-deployed-ai-engineer · https://review.firstround.com/so-you-want-to-hire-a-forward-deployed-engineer/ · https://a16z.com/forward-deployed-job-titles/ · https://x.com/a16z/status/1930774473445142934 · https://blog.pragmaticengineer.com/the-pulse-forward-deployed-engineering-heats-up-again/ · https://www.latent.space/p/forward-deployed-engineer-best-practices · https://www.sundeepteki.org/advice/forward-deployed-ai-engineer · https://pickuma.com/for-dev/ai-coding-tools-in-interviews-2026/ · https://www.freecodecamp.org/news/agentic-ai-engineering-in-practice-how-to-build-with-claude-code-codex-and-gemini/ · https://getperspective.ai/blog/2026-fde-hiring-trends-what-1000-job-posts-reveal

OpenAI/Anthropic: https://builtin.com/job/forward-deployed-engineer-sf/4457354 · https://builtin.com/job/forward-deployed-software-engineer/4457361 · https://theorg.com/org/openai/org-chart/colin-jarvis · https://newsletter.eng-leadership.com/p/inside-openais-forward-deployed-engineer · https://www.tryexponent.com/guides/openai-forward-deployed-engineer-interview · https://www.paraform.com/blog/openai-forward-deployed-engineer · https://x.com/OpenAI/status/2053824999736410415 · https://www.cooley.com/news/coverage/2026/2026-05-12-openai-forms-new-joint-venture-openai-deployment-company-and-acquires-tomoro · https://job-boards.greenhouse.io/anthropic/jobs/5302966008 · https://jobs.generalcatalyst.com/companies/anthropic/jobs/62065791-head-of-forward-deployed-engineering · https://getperspective.ai/blog/anthropic-applied-ai-engineer-interview-process-frontier-lab-2026 · https://finance.biggo.com/podcast/cb47ec147e982d4d · https://www.aol.com/articles/im-24-old-hottest-job-095901698.html · https://www.linkedin.com/in/anishbikmal/ · https://www.linkedin.com/in/tanveermittal/

Стартапы: https://jobs.ashbyhq.com/Sierra/d9c0aa93-e35d-4752-9cef-4c39dcad5365 · https://ai.engineer/speakers/clay-bavor · https://techcrunch.com/2026/04/09/sierras-bret-taylor-says-the-era-of-clicking-buttons-is-over/ · https://app.welcometothejungle.com/jobs/2QZ53YSK · https://cloud.substack.com/p/harvey-puts-a-former-practicing-lawyer · https://decagon.ai/blog/build-the-future-you-want · https://decagon.ai/blog/how-decagon-is-redefining-forward-deployment · https://www.youtube.com/watch?v=7wu2hsRfvV0 · https://www.zenml.io/llmops-database/scaling-forward-deployment-engineering-for-ai-customer-service-agents · https://job-boards.greenhouse.io/gleanwork/jobs/4651991005 · https://scale.com/careers/4694861005 · https://cursor.com/careers/forward-deployed-engineer-new-york-remote-san-francisco · https://www.builtinnyc.com/job/forward-deployed-ml-engineer/4759015

Не-канон/визы/крипта: https://www.baseten.co/blog/what-i-learned-as-a-forward-deployed-engineer-working-at-an-ai-startup/ · https://stroupaloop.com/blog/2026/05/16/forward-deployed-didnt-start-in-tech/ · https://jaabz.com/jobs/209240-forward-deployed-engineer-a-claritycare-ai · https://www.jackandjill.ai/about-us · https://www.coinbase.com/careers/positions/8099099 (403, листинг существует)

**Что не удалось проверить (явно):** именной FDE в самом OpenAI с открытым профилем — НЕ НАШЁЛ; имя главы Anthropic Applied AI — НЕ НАШЁЛ; verbatim-цитата Ted Mabrey о роли — НЕ НАШЁЛ; именной McKinsey/BCG → Palantir FDE — НЕ НАШЁЛ; строгая цифра сдвига ratio инженер/консультант до/после агентов — НЕ НАШЁЛ; возрастная дискриминация в FDE — НЕ НАШЁЛ свидетельств (вывод из их отсутствия). LinkedIn-профили (Bikmal, Mittal, Felgueres) — только заголовок из выдачи, тело заблокировано (HTTP 999). Nabeel-эссе на nabeelqu.co отдавало 429; факты взяты с зеркала на Substack.

---

## ЧТО ЭТОМУ ЧЕЛОВЕКУ ИСКАТЬ В СВОЁМ ПРОШЛОМ

15 вопросов-подсказок. Положительный ответ на каждый закрывает элемент портрета. Бьют в обычный операционный опыт, который люди не пишут в резюме. Где профиль объективно не дотягивает — сказано прямо.

1. **Встраивание в клиента.** Вспомни: сидел ли ты неделями внутри чужой организации (партнёра, контрагента, регулятора, биржи-клиента) и разбирался в их процессах изнутри, а не по документам? → закрывает «embed on-site», ядро FDE.
2. **Перевод «верх ↔ инженеры».** Был ли случай, когда ты в одной комнате объяснял не-технaрю (CEO/инвестору/регулятору) сложную систему, а через час ставил инженерам ТЗ по той же вещи? → закрывает 25% «communication» веса.
3. **Интеграция в помойку.** Приходилось ли подключаться к чужой недокументированной/легаси-системе (банковский шлюз, биржевой API, ликвидность-провайдер) и заставлять это работать в проде? → «integration/plumbing» = 50% работы.
4. **Отгрузка за дни, потом закалка.** Был ли раз, когда ты за пару дней собрал грубо работающую штуку под горящую задачу, а потом неделями доводил её до продакшена? → «ship PoC in days».
5. **Спека, которой не было.** Вспомни задачу, где никто не знал, что именно строить, и ты сам определил, что «готово», и отвечал за исход. → «owned outcomes under ambiguity» — сигнал №1.
6. **Чтение чужого кода руками.** Разбирал ли ты недавно чужой код/систему БЕЗ ИИ — реверсил, чинил по гипотезам? → прямой ответ на риск Palantir re-engineering/coding-раунда. Если честно «давно и без ИИ не садился» — это твоя реальная дыра, крой тренировкой live-coding, а не рамкой.
7. **Освоил за ночь.** Был ли инструмент/язык/протокол, который ты выучил за вечер и применил назавтра в деле? → «learning round», rapid domain mastery.
8. **Обучал разработчиков/фаундеров.** Ты учил инженеров и основателей, и у тебя PhD по теме обучения девелоперов — вспомни конкретные случаи, где твоё вмешательство «включило» чужую команду. → это редкий актив прямо под FDE-онбординг клиента; НЕ занижай его.
9. **Доверие скептика на самом верху.** Был ли контрагент/партнёр/чиновник, который сначала тебе не верил, а ты за месяцы завоевал доверие и провёл сделку/интеграцию? → «partner at the highest level, gain trust».
10. **Отклонил правдоподобно-неверное.** Вспомни, когда ты завернул убедительный, но неправильный ответ — от подчинённого, вендора или ИИ-агента. → это буквально топ-сигнал новых ИИ-интервью.
11. **Высокая ставка под туманом.** Принимал ли ты решение с деньгами/риском на кону при неполных данных и отвечал за последствия? (11 лет COO/CTO крипты — тут материал плотный.) → «high-stakes decisioning».
12. **Четыре имени одной сущности.** Ловил ли ты, что в одной компании одну и ту же вещь называют по-разному в разных отделах, и сводил это в единую модель? → «nouns and verbs mapping», сердце discovery.
13. **Агент в проде у пользователя.** Где твои мульти-агентные системы реально работают на живом пользователе, а не в демо? Собери метрики, инциденты, откаты. → «shipped agents, not prototypes» — твой самый горячий сигнал 2026.
14. **Публичный артефакт.** Есть ли у тебя пост/репо/доклад/препринт, по которому тебя можно «поймать» как строителя (как Trivedi поймали по блогу)? Если нет — это дверь, которую надо построить в ближайшие недели.
15. **Наведи мост крипта→enterprise.** Вспомни, где твой крипто/системный/криптографический опыт решал задачу, понятную обычному enterprise (риск, комплаенс, надёжность, безопасность данных). → переводит «нишевую крипту» в универсальный доменный актив.

**Где профиль объективно НЕ дотягивает — без лести:**
- **Ручной live-coding без ИИ.** Твоя сила — оркестрация агентов и декомпозиция; твоя слабость на их шкале — «защити код построчно на глазах без ассистента». На Palantir это фильтр №1, обойти рамкой нельзя — только тренировкой (CodePair-сессии, medium/hard в сценарии). Замещать нечем: это ровно то, что они защищают запретом ИИ.
- **Ты не фаундер.** Замещается почти полностью: FDE зовёт «фаундер ИЛИ ранний инженер/оператор, владевший исходом» — твои 11 лет COO/CTO закрывают «owned outcomes», но слово «founder» о себе не используй (неправда, проверяемо). Рамка — «operator who owned delivery & partnerships under ambiguity».
- **Enterprise-клиент-фейсинг в западном софтверном смысле.** Если ты вёл партнёров/регуляторов, но не «встраивался инженером внутрь клиента ради отгрузки софта» — это разрыв; частично замещается COO-стейкхолдер-опытом, но в AI-native shops его надо пересобрать в терминах «deploy into customer prod».
- **Свежесть западной enterprise-сети/референсов.** Крипта-домен + база Пало-Альто/Синтра — плюс, но реферал в Palantir-мафию/лаборатории у тебя пока не назван; дверь №14 (публичный артефакт) и alumni-нетворк надо строить целенаправленно.

🧒 Простыми словами: FDE — это инженер, которого сажают внутрь клиента чинить его самую запутанную задачу своими руками и параллельно уговаривать больших начальников. Берут туда НЕ по диплому — в их же объявлениях написано «бывший фаундер — плюс», «визу спонсируем», «информатика — лишь одно из желательных направлений». Твой профиль почти везде попадает в «желательное»: PhD, код, живые агенты в проде, O-1 и паспорт ЕС (виза не нужна — большой плюс). Одна настоящая слабина — на интервью просят писать код руками без ИИ, а это не твоя сильная сторона; у Palantir это главный барьер, у AI-стартапов — мягче. Подавай себя как «оператора, который тащил результат в тумане и уже собрал работающих агентов», а не как «фаундера» (ты им не был) и не как «у меня нет ML-диплома» (он и не нужен).
