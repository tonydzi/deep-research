# DR26-09-14-ZB-10-2009 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала читаю техзадание целиком и поднимаю контекст по теме, затем иду в живой веб-поиск.Техзадание прочитано. Иду в живой поиск по найму DevRel и Developer Education в лабораториях из скоупа — сначала широкие запросы, потом люди и компании поимённо.Есть первые имена. Теперь копаю по компаниям и конкретным анонсам найма — Anthropic, OpenAI, xAI, Cursor, ElevenLabs и остальные.Есть ядро имён. Добираю OpenAI, Meta, Mistral, LangChain, Together, Modal и процесс найма — плюс неканонические биографии.**Идём верно:** заказчику нужна не вакансия, а кто реально прошёл. За 24 месяца лаборатории берут не «евангелиста с харизмой», а человека с уже существующей публичной поверхностью — YouTube, рассылка, cookbook, комьюнити, OSS — плюс способность писать рабочий код. **DevEd шире DevRel для не-FAANG-инженера.** Вопрос, который держит всё исследование: готов ли кандидат показать артефакт, который лаборатория уже использует, ещё до собеседования?

Ниже — живая разведка на 14 сентября 2026. Каждый человек = проверяемая ссылка. Где имени нет — написано «не нашёл», не «этого нет».

---

# Кого реально берут в DevRel / Developer Education в AI-лабораториях (сент. 2024 — сент. 2026)

## 0. Две двери, не одна роль

Лаборатории уже развели функции в оргструктуре и в зарплатных вилках. Путать их — промах заявки.

| | **DevRel / Developer Advocate / DevRel Engineer** | **Developer Education / Education Engineer / DevEd Lead** |
|---|---|---|
| **Работа** | Лицо платформы, доверие, фидбек в продукт, конференции, X, хакатоны, партнёрства | Учебная система: курсы, cookbooks, YouTube-roadmap, сертификации, Academy |
| **Наниматель** | Head of Platform / Head of DevEx / Head of DevRel | Education org или отдельный DevEd Lead рядом с DevRel |
| **Доказательство** | «Меня знают разработчики» + умею шипить демо | «Я умею учить разработчиков системно» + сам разработчик |
| **Вилка (публичные JD)** | Anthropic Head of DevRel: **$385k–$460k**; GDM Sr DevRel Eng: **$183k–$271k**; LangChain DA LangSmith: **$200k–$240k**; Modal: **$175k–$275k** | Anthropic Developer Education Lead: **$290k–$365k**; LangChain Education Engineer: **$150k–$185k**; Mistral DevEd Lead (London) — вилка не раскрыта |
| **Кто проходит без FAANG-SWE** | Редко: только если артефакт уже рынок | Чаще: педагогика + контент-система + «ты сам разработчик» |

Источники вилок: Anthropic Founding DevRel Lead / Head of DevRel (swyx фиксирует $385–460k и вакансию, которая висела ≥4 месяца); Anthropic Developer Education Lead $290–365k, роль закрыта на момент 14.09.2026; GDM Senior Developer Relations Engineer $183–271k + bonus/equity, дедлайн заявки 23.04.2025; LangChain DA LangSmith $200–240k, в JD прямо просят **product/technical marketing 6–10 лет**, не классический SWE-DevRel; LangChain Education Engineer $150–185k, гибрид «софт + педагогика».

**Какая дверь шире для кандидата без FAANG-инженерного прошлого: Developer Education.**

Почему это claim, а не лозунг:

1. Anthropic явно отделил Education от DevRel: Education Lead «сидит на пересечении DevRel, product и education», скоуп — video, tutorials, API-курсы, cookbooks, YouTube-стратегия, ambassador/certifications. Требование — **5+ лет developer content strategy / DevEd / DevRel / technical content leadership**, не 10 лет руководства экосистемой. Вилка $290–365k, гибрид. JD подчёркивает: «This is not a traditional content role» и «You understand developers as an audience, because you are one» — педагогика + быть разработчиком, не PhD и не FAANG.
2. LangChain Academy нанимает Education Engineer: «5+ years as a software developer **and/or technical educator**». «And/or» — формальная щель для педагога, которой нет в DA LangSmith (там 6–10 лет marketing).
3. Mistral держит **две** вакансии параллельно: Senior/Staff AI Developer Advocate (6+ лет DevRel + enterprise + часто Master's CS) **и** Developer Education Lead в Лондоне (7+ лет technical writing / DevEd, 3+ лет лидерства доков). DevEd-дверь формально не требует enterprise DevRel-стажа.
4. Реальные прохождения без канона почти все несут **образовательный** артефакт: Pauline — Code First Girls + community; Sophia Yang — PhD Educational Psychology; Alejandro AO — YouTube-курсы; Charlie Guo — рассылка-тьюториалы. Чистый «коммьюнити-менеджер без кода» в DevRel лаборатории за 24 месяца **не найден**.

DevRel-дверь у frontier-лабораторий уже: Anthropic Head of DevRel висел ≥4 месяца при $385–460k — это не «никто не хочет», это «почти никто не проходит планку лица платформы».

---

## 1. КОГО РЕАЛЬНО БЕРУТ (живые люди, 24 месяца)

Окно: **14 сентября 2024 — 14 сентября 2026**. Людей чуть старше окна помечаю отдельно: они incumbent, не «наняты в окне».

### Таблица найма (ядро)

| # | Имя | Компания | Титул | Дата | Откуда пришли | Ссылка |
|---|---|---|---|---|---|---|
| 1 | **Lee Robinson** | Cursor / Anysphere | VP Developer Experience / фактически Developer Education | июль 2025 (~80 чел. в компании) | VP DX, Vercel (2020–2025); до того Hy-Vee, Workiva; BS Computer Engineering, Iowa State | The Org; Arena Mag; leerob.com/cursor |
| 2 | **Omar Sanseviero** | Google DeepMind | DevRel Engineering / MTS, Gemini/Gemma/AI Studio | ~8 февраля 2025 | Hugging Face, «Chief Llama Officer»; до того Google SWE; сооснователь **AI Learners** (ML на испанском) | X Omar + Paige; LinkedIn-пост |
| 3 | **Paige Bailey** | Google DeepMind | DevRel Engineering Lead | публично «first DevRel team» 8.02.2025 | GitHub | X @DynamicWebPaige |
| 4 | **Philipp Schmid** | Google DeepMind | Staff Engineer, Developer Experience / DevRel | 6 февраля 2025 | Tech Lead, Hugging Face (cloud/hardware partnerships, Inference Endpoints; рост revenue $0→~$100M/4 года); AWS ML Hero #1 Germany; живёт в Nuremberg | X @_philschmid; philschmid.de |
| 5 | **Patrick Löber** | Google DeepMind | DevRel Engineer (Gemini, Gemma, AI Studio) | 5 февраля 2025 | YouTube-педагог; ранее AssemblyAI | LinkedIn-пост |
| 6 | **Luciano Martins** | Google DeepMind | Developer Advocate, AI & Open Models (Бразилия) | в GDM после фолда Cloud AI-команд; в Google с фев. 2020 | Google Cloud DA; IBM, Oracle, AWS; CS Universidade da Amazônia, магистратура UNICAMP | GitHub lucianommartins; SET Portal |
| 7 | **Logan Kilpatrick** | Google DeepMind (через фолд AI Studio) | Product, Google AI Studio / Gemini API | 9 января 2025 (реорг) | OpenAI, first DevRel | TechCrunch; его X |
| 8 | **Pauline P. Narvas** | OpenAI | Developer Experience (community across Codex, API, ChatGPT) | июнь 2026 | Head of Community Platforms, Vercel (2024–2026); Head of Community, Gitpod/Ona (2021–2024); SRE/platform eng BT и Sky; **Code First Girls** — учила женщин кодить с 2016 | pawlean.com/posts/joining-openai; CV |
| 9 | **Charlie Guo** | OpenAI | Developer Experience (docs, guides, tutorials, examples) | 9 января 2026 | Автор рассылки **Artificial Ignorance** (3 года); AI engineer at Pulley; Stanford CS; основатель YC/500/StartX-стартапов; книга Unscalable | ignorance.ai/p/on-joining-openai; X |
| 10 | **Alejandro AO** | Hugging Face | Developer Advocate | 17 ноября 2025 | AI Education / YouTube @alejandro_ao | X @_alejandroao |
| 11 | **Joffrey Thomas** | Mistral AI | Developer Relations Engineer | 29 апреля 2025 | Hugging Face (enterprise use-cases + **agent-course**) | LinkedIn-пост; команда Sophia Yang |
| 12 | **Andrew Reed** | LangChain | Applied AI (LangGraph/LangSmith, enterprise enablement) | 22 сентября 2025 | Hugging Face, customers/partners, open AI | X @andrewrreed |
| 13 | **Addy Osmani** | Anthropic | Claude Code (делать продукт лучше для разработчиков) | 8 сентября 2026 | Google Chrome, «god of chrome» в реакциях | X @addyosmani |
| 14 | **Kristen Swanson** | Anthropic | MTS → lead **Education Labs** | MTS декабрь 2025; анонс лидерства Education Labs июль 2026 | learning sciences / research / design; нанимает education researcher + design engineer | EdTech Innovation Hub |
| 15 | **Paul Asjes** | ElevenLabs | DX (нанимает DevRel Engineers) | ~начало 2025 (в авг. 2025: «joined six months ago») | Stripe DevRel; WorkOS DX; Superbase ~4 года; живёт в Сингапуре | X @paul_asjes; YouTube-интервью |

### Чуть вне окна 24 месяцев, но incumbent — без них картина лжёт

| Имя | Компания | Дата входа | Почему важен | Ссылка |
|---|---|---|---|---|
| **Alex Albert** | Anthropic, Head of DevRel / later Research PM | 18 марта 2024 | Построил DevRel Anthropic. Публичный артефакт **до** найма: Jailbreak Chat. Не FAANG, не PhD, выпуск CS ~2019–2023 | X @alexalbert__; BI называет его head of developer relations окт. 2025 |
| **Sophia Yang, Ph.D.** | Mistral, Head of Developer Relations | январь 2024 → ушла 17 августа 2026 | **PhD Educational Psychology** (не CS). Senior DS в Anaconda 2018–2024. NumFOCUS board. Собрала DevRel с ~20 человек до 1000+ | The Org; X farewell |
| **Joana Carrasqueira** | Google DeepMind, Head / Senior Manager of Developer Relations | incumbent, путь с TensorFlow DevRel | **Фармацевт → ООН/FIP (47 стран) → MBA IE + Berkeley → Google.** Google сам рекрутил: «You understand how the brain works» | LinkedIn Pulse / Silicon & Spice, май 2025 |

### Соседние наймы, которые путают с DevRel — и не надо

- **swyx (Shawn Wang) → Cognition, сент. 2025** — через сделку Smol AI, не классический DevRel-IC. Сам пишет, что Anthropic **не может закрыть** Head of DevRel.
- **Thariq Shihipar, Anthropic Claude Code MTS** — публичный педагог Claude Code (HTML-as-Markdown, workshops Code w/ Claude), но титул engineering, не DevRel. До Anthropic: сооснователь PubPub, YC-стартап One More Multiverse. Полезен как паттерн «failed/wound-down founder → DX внутри продукта».
- **Hunmin Xavier Choi** — Claude **Community Ambassador**, не FTE (дек. 2025). Ambassadors — отдельная дверь, не оффер.
- **xAI FDE, X Developer Platform** — JD смешивает FDE + DevEx ($180k–$440k), именного DevRel-найма **не нашёл**.

### По компаниям скоупа: что нашёл / не нашёл

**Anthropic.** Incumbent DevRel — Alex Albert (март 2024). Head of DevRel / Founding DevRel Lead висел месяцами при $385–460k; Katelyn Lesse (Head of Platform, ex-Stripe) нанимала DevRel на Claude Developer Platform в ноябре 2025. Developer Education Lead $290–365k — JD снят, **кто именно сел — не нашёл**. Education Labs ведёт Kristen Swanson (не DevRel). Addy Osmani (сент. 2026) — продукт Claude Code, не DevRel-титул. Applied AI Claude Evangelist $240–315k (май 2026) — вакансия, не человек.

**OpenAI.** Head of Developer Experience — **Romain Huet** (ex-Stripe), incumbent. Наняты в окне: Charlie Guo (янв. 2026), Pauline Narvas (июнь 2026). Romain в дек. 2025 писал, что команда растёт: «engineers and storytellers… 4M+ developers». Индия: вакансия Developer Advocate, Developer Experience — Singapore / India-Remote; **кто заполнен — не нашёл**. Codex Ambassadors — 155 человек, это community program, не FTE.

**Google DeepMind.** Самый плотный пакет найма в окне: февраль 2025 — «very first developer relations team». Ядро: Paige Bailey, Omar Sanseviero, Philipp Schmid, Patrick Löber, Luciano Martins; Logan Kilpatrick пришёл реоргом AI Studio (янв. 2025). Joana Carrasqueira — Head. Это **покупка готовых публичных лиц** из HF, GitHub, OpenAI, YouTube, Google Cloud.

**Mistral AI.** Sophia Yang Head of DevRel янв. 2024–авг. 2026 (чуть вне окна входа, внутри окна — строила команду). Наняла Joffrey Thomas апр. 2025. The Org показывает команду: Harizo Rajaona (DevRel Expert), Joffrey Thomas, Clément C. (AI DevRel Engineer). **Биографии Harizo и Clément C. не нашёл в открытом вебе.** Senior/Staff DA (enterprise, 6+ лет) и DevEd Lead London — вакансии 2025–2026, заполнение поимённо не нашёл. Sophia ушла авг. 2026; кто Head сейчас — не нашёл.

**Hugging Face.** Alejandro AO, DA, нояб. 2025 (YouTube-педагог). Dylan Ebert **ушёл** окт. 2025 после 3 лет DA Engineer → Adaptive ML founding DA (PhD Brown, 3D ML YouTube). Omar и Philipp ушли в GDM. HF продолжает нанимать DA Engineer Hub/Enterprise. Паттерн HF: берут людей с **курсами/видео про open models**, не FAANG.

**Cursor / Anysphere.** Lee Robinson, июль 2025. Arena Mag прямо называет его «VP of Developer Education at Cursor». За год: docs, learning courses, выступления про context engineering и Composer; июнь 2026 — внутренний переход в ML (model behavior). Это не «вечный DevRel»: лаборатория/devtools съедает сильного DevRel в продукт/модель.

**LangChain.** Education Engineer / Education Engineer Fullstack — открытые роли (Academy, 1M+ developers). DA LangSmith $200–240k — JD ближе к **technical product marketing**, чем к классическому advocate. Andrew Reed (сент. 2025) — Applied AI, не DevRel-титул. **Именного DA LangSmith, который сел — не нашёл.** Experts Program (нояб. 2025) — volunteer/community, не FTE.

**ElevenLabs.** Paul Asjes (ex-Stripe DevRel) строит DX, нанимает DevRel Engineer нояб. 2025. Вилка в ранних JD $80–150k, позже London £150–250k — разъезд вилок, не выдумка. **Кого именно нанял в команду — не нашёл.** В интервью авг. 2025: не гонять всех через один coding interview, иначе не будет diversity стеков (iOS, Kotlin, Flutter, .NET, Arduino).

**Perplexity.** James Liounis нанимал Developer Relations Manager — Sonar/API Platform с марта 2025; JD жил ещё в мае 2026 ($170–220k, 4+ лет DevRel, «you've shipped code»). **Кто сел — не нашёл.** Параллельно FDE API Platform $205–335k — это не DevRel.

**xAI.** Именного Developer Advocate **не нашёл**. Есть FDE X Developer Platform ($180–440k) с DevEx-обязанностями (docs, sample apps) и Sr Partner Manager Developer Platform. Стратегия xAI — инженеры едут к клиенту, не классический DevRel.

**Together AI.** Именного DA **не нашёл**. CPO Charles Zedlewski (июнь 2025) — Temporal/Cloudera product, не DevRel. GTM/PMM есть; DevRel как функция в открытых следах тонкий.

**Fireworks AI.** JD Technical Developer Advocate NY $160–190k, 3+ лет DevRel. **Кто нанят — не нашёл.** swyx в окт. 2025 прямо предлагает реферить в Fireworks — значит роль была живая.

**Modal.** Нанимает **первых** DevRel Engineers, $175–275k, on-site SF/NY/Stockholm, 3+ лет SWE + 1 год ML. **Именного найма не нашёл** — команда ещё в режиме «first hire».

**Scale AI.** После сделки Meta/Wang (июнь 2025) публичного DevRel-найма **не нашёл**. Нанимают Applied AI / enterprise delivery, не advocacy.

**Meta AI.** Вакансии Developer Advocate, AI Agents (internal, $173–247k, 8+ лет SWE или DevRel) и Wearables DA (AI glasses). **Именного внешнего найма в Llama DevRel за 24 месяца не нашёл.** Meta в этот период покупала research superstar'ов, не DevRel-звёзд.

**Cohere.** Head of DevRel JD циркулирует как «no longer available»; формулировки похожи на founding-роль («first Developer Advocate in North America»). **Кто занимает роль в 2025–2026 — не нашёл.** Крупные наймы Cohere за окно — Joelle Pineau CAIO, не DevRel.

---

## 2. ПРЕДЫДУЩИЙ ОПЫТ — что реально сработало

Разложу по слоям на людях из таблицы, не абстрактно.

### Последняя роль до найма

Повторяется **четыре источника**, не «SWE в FAANG»:

1. **Другая DevRel/DX-машина того же класса.** Lee ← Vercel. Paige ← GitHub. Pauline ← Vercel community. Paul Asjes ← Stripe. Logan ← OpenAI DevRel. Luciano ← Google Cloud DA.
2. **Hugging Face как транзитный аэропорт.** Omar, Philipp, Joffrey, Andrew Reed, Dylan (ушёл). HF — главная кузница AI-DevRel за 24 месяца.
3. **Публичный педагог / автор, которого уже смотрят.** Alejandro YouTube; Charlie рассылка; Patrick YouTube; Philipp блог philschmid.de.
4. **Внутри продукта, который сами учат.** Addy → Claude Code; Thariq уже в Claude Code; Lee через год ушёл из DX в ML Composer.

### Формальное образование — слабый фильтр

| Паттерн | Кто | Комментарий |
|---|---|---|
| CS / Computer Engineering | Lee (Iowa State), Charlie (Stanford), Luciano (Univ. da Amazônia) | Есть, но не отличитель |
| PhD CS | Dylan Ebert (Brown) — **ушёл из HF** | PhD не якорь удержания в DevRel |
| **PhD не-CS** | **Sophia Yang: Educational Psychology** + MS Statistics + MS CS | Это и есть её дверь в Head of DevRel |
| Фарма / public health | **Joana Carrasqueira** | Google рекрутил сам |
| Без PhD, без FAANG-SWE | Alex Albert, Pauline, Alejandro, Joffrey | Проходят через артефакт |

**Канонический путь ML-PhD → FAANG research → DevRel: среди нанятых за 24 месяца почти не встречается.** Dylan — ближайший, и он ушёл из HF в founding DA стартапа.

### Доменный опыт

- **Developer tools / DX-платформы:** Vercel, Stripe, GitHub, Gitpod, Hugging Face Hub, Google Cloud AI.
- **Open-source AI ecosystem:** Transformers, cookbooks, Inference Endpoints, agent courses.
- **Enterprise enablement:** Philipp (cloud partnerships), Andrew Reed (HF customers), Mistral DA JD прямо требует enterprise.
- **Community platforms:** Pauline (построила Vercel Community и Gitpod community from zero).

### Публичный артефакт — это и есть резюме

У > половины названных людей артефакт существовал **до** оффера:

- **Alex Albert** — Jailbreak Chat (сайт, который знала вся prompt-сцена).
- **Charlie Guo** — 3 года Artificial Ignorance, «read, think, tinker, write, repeat»; сам называет путь self-taught AI engineer через build-in-public.
- **Pauline** — 382 поста с 2008, Code First Girls, публичный community-бренд.
- **Philipp Schmid** — philschmid.de, AWS ML Hero, технические гайды по LLM serving.
- **Omar** — AI Learners (испаноязычный ML), Hacker Llama.
- **Alejandro AO** — YouTube-курсы; в анонсе сам просит «course/video ideas».
- **Lee Robinson** — leerob.com, лицо Next.js/Vercel, 270k+ followers.
- **Patrick Löber** — YouTube Python/Docker/ML.
- **Addy Osmani** — годы Chrome DevRel-контента, книги, web.dev.

**Не нашёл** ни одного прохождения в лабораторию скоупа, где человек без публичного следа «просто хорошо прошёл интервью». Может существовать — в открытом вебе не видно.

### «Странный» опыт, который стал активом

| Странность | Кто | Как конвертировали |
|---|---|---|
| Фармацевтика + 47 стран UN | Joana | «ты понимаешь, как работает мозг / как внедрять сложное в институции» |
| PhD Educational Psychology | Sophia Yang | Head of DevRel = education + ecosystem, не research |
| Учила женщин кодить / community from zero | Pauline | OpenAI DevEx community |
| Рассылка вместо job title | Charlie | «я уже делал DevEd OpenAI снаружи» |
| Failed/wound-down founder (PubPub, One More Multiverse) | Thariq | внутри Claude Code как builder+teacher |
| YC-основатель + книга Unscalable | Charlie | «умею объяснять сложное не-инженерам» |
| Не-англоязычный рынок | Omar (ES), Luciano (PT-BR), Sophia (переехала во Францию без французского) | GDM и Mistral берут regional faces |
| Не в SF | Philipp — Nuremberg; Pauline — UK; Luciano — São Paulo; Paul Asjes — Singapore | GDM: «Skillset + mindset > location» |

### Счётчик неканона

Из 15 наймов в окне + 3 incumbent:

- **Не ML-PhD:** 17/18 (исключение Dylan, и он ушёл).
- **Не FAANG-SWE как последняя роль:** ~12/18. FAANG-adjacent: Paige (GitHub), Logan (OpenAI), Luciano (Google Cloud), Addy (Google Chrome), Omar (Google раньше, но последняя роль HF).
- **Не «кодер в проде 10 лет» как главный сигнал:** Pauline (community lead), Sophia (DS + education), Alejandro (educator), Charlie (writer+builder).

Грубо: **больше половины прошедших — не канон.** Но «не канон» ≠ «не технический». Нулевой код в этой выборке не проходит.

---

## 3. ПОВТОРЯЮЩИЕСЯ ПАТТЕРНЫ

### (а) Паттерн-сигнал (что доказывает компетенцию)

Встречается у **>50%** названных:

1. **Уже существующая аудитория разработчиков.** X/YouTube/рассылка/курсы. Lee, Paige, Omar, Philipp, Pauline, Charlie, Alejandro, Patrick, Addy, Alex Albert.
2. **Умение шипить рабочий артефакт, не слайд.** Cookbooks, demos, docs, courses. GDM JD: «produce excellent code samples». ElevenLabs: «come up with a tutorial idea, build a working demo, write the blog post».
3. **Близость к продукту, который учишь.** Не «евангелист всего AI», а Gemini API / Claude Code / Cursor / Mistral models / HF Hub.
4. **Доказанный teaching loop.** Курс, workshop, Academy, YouTube series — не разовый пост.
5. **Фидбек в продукт как часть работы.** Pauline: «best DX work comes from being close to the product». Lee: «pseudo-PM» на запусках Cursor.

Чего почти нет у прошедших (и что вакансии всё ещё пишут):

- ML-PhD как обязаловка.
- «10+ years leading DevRel at scale» для IC-ролей (это для Head, и Head как раз **не закрывается**).
- Чистый marketing without code.
- «Passionate about AI» без URL.

### (б) Паттерн-дверь (как физически вошли)

| Дверь | Примеры | Частота в выборке |
|---|---|---|
| **Покупка известного лица / inbound от репутации** | Lee→Cursor; GDM-пакет (Omar, Paige, Philipp, Patrick); Addy→Anthropic | Самая частая для senior |
| **Реферал / тёплая сеть внутри DX** | Pauline благодарит Romain Huet, Dominik Kundel, Joe; Joffrey благодарит Sophia Yang | Сильная на IC |
| **Публичный артефакт как холодная заявка, которую не надо было холодной делать** | Charlie (рассылка 3 года); Alejandro (YouTube); Alex Albert (Jailbreak Chat, 2024) | Главная дверь неканона |
| **Внутренний фолд / reorg** | Logan + AI Studio → GDM; Luciano Cloud AI → GDM | Google-специфика |
| **Транзит через HF** | Omar, Philipp, Joffrey, Andrew | Систематический pipeline |
| **Холодная заявка без артефакта** | **Не нашёл ни одного подтверждённого прохождения** | — |
| **Покупка команды** | Cognition←Smol AI (swyx) — соседний, не DevRel-IC | Редко |
| **Ambassador → FTE** | Hunmin — ambassador, не FTE. **Конверсии ambassador→оффер лаборатории не нашёл** | Не доказано |

swyx отдельно фиксирует: «you can get a DevRel job just being reliably Good At Twitter» — это дверь для devtools, не обязательно для Anthropic Head. Для Head Anthropic дверь не сработала 4+ месяца.

### (в) Паттерн-тайминга

1. **Волна «DevRel is back» осень 2025.** swyx, 13.10.2025: поиски «developer relations» на максимумах; Cursor←Lee, Factory←Ben Tossell, Cognition←swyx; Anthropic Head всё ещё открыт. Драйвер — bottom-up adoption моделей/агентов, не ZIRP-штаты.
2. **GDM собрал first DevRel team одним пакетом в феврале 2025** — сразу после фолда AI Studio в DeepMind (янв. 2025, «accelerate the research to developer pipeline»). Тайминг: лаборатория поняла, что research не доезжает до разработчика.
3. **Anthropic Education отделяется от DevRel в 2026**, когда Claude Code стал главным developer-продуктом. DevEd Lead $290–365k; Education Labs (Kristen Swanson). Тайминг: когда продукт сложный, одной адвокации мало — нужна учебная система.
4. **OpenAI DevEx добирает storytellers в 2025–2026** (Charlie янв., Pauline июнь), когда поверхность = API + Codex + ChatGPT Apps, «можно работать с OpenAI и ни разу не открыть core API docs» (формулировка Charlie).
5. **Сильный DevRel через 6–12 месяцев уезжает в продукт/модель.** Lee: DX → ML Composer (июнь 2026). Alex Albert: DevRel → research PM. Это не провал роли — это куда лаборатория двигает тех, кто доказал вкус.

---

## 4. СОБИРАТЕЛЬНЫЙ ПОРТРЕТ ИДЕАЛЬНОГО КАНДИДАТА

Два портрета, потому что двери разные. «Берут почти всегда» — профиль, который в этой выборке не отфильтровывается.

### A. DevRel / Developer Advocate в лаборатории

**One-liner:** «Я уже лицо куска экосистемы, в которой живёт ваш продукт, и я умею превратить запуск модели в рабочие демо, доки и фидбек за неделю».

**5–7 обязательных элементов:**

1. Публичный след, который лаборатория может открыть за 10 минут (X / YouTube / блог / курс / GitHub).
2. Руки: Python (часто TS), живое демо, не слайд.
3. Домен: LLM apps / agents / DX tooling, не «общий cloud».
4. Доказанный цикл «запуск → контент в тот же день» (Cat Wu про Anthropic: Tarek и Lydia on DevRel jump in next day).
5. Умение быть голосом разработчика **внутри** (баги доков, DX-трение), не только снаружи.
6. Готовность к IRL (SF/NY/London/Paris) хотя бы частично; GDM — исключение по географии.
7. Для senior/Head: 7–10 лет developer-facing + умение нанять команду (и именно поэтому Head висит).

**3–5 сильно желательных:**

- Уже строил с **этим** API (бонус ElevenLabs, Mistral, Perplexity).
- Enterprise narrative (Mistral Staff DA прямо требует).
- Не-EN рынок (Omar, Luciano).
- Newsletter / YouTube с регулярностью, не всплеском.
- Founder/early operator (Anthropic Claude Evangelist JD: technical founder).

**Измеримые пороги, которые встречаются в JD / на практике:**

- Стаж: IC 3–6 лет DevRel или adjacent; Staff 6+; Head 10+.
- Comp: IC лаборатории часто **$180–270k base**; Head **$385–460k**; это рядом с SWE, не «маркетинг». Pete Hunt (Nov 2025): без равенства банд с SWE хороших DevRel не взять.
- Аудитория: жёстких «N подписчиков» в JD нет. На практике прошедшие — от ~3k (Alejandro) до 270k (Lee). **Порог не число, а «нас уже читают те, кого мы хотим».**
- Звёзды репо / цитаты: не найдены как hard gate. Jailbreak Chat и Next.js — качественные исключения.
- Капитал: не релевантно, кроме founder-трека Evangelist.

**Первая строка резюме, которая рифмуется с наймами:**

> Developer advocate who taught X,000 builders to ship production agents on [Claude/Gemini/Mistral] — courses, cookbooks, and the Discord where the bugs show up first.

Не: «Passionate DevRel professional seeking to leverage synergies in the AI ecosystem.»

### B. Developer Education Lead / Education Engineer

**One-liner:** «Я строю учебную систему, из-за которой разработчик доходит от “слышал про Claude” до “катит в проде”, и я сам этот путь проходил руками».

**Обязательное:**

1. Owned content roadmap end-to-end (Anthropic DevEd Lead).
2. Ты разработчик, не instructional designer со стороны.
3. Форматы: video + written + courses + cookbooks, не один блог.
4. Кросс-функционал: product launches → education ships with the feature.
5. Метрики учебной воронки (completion, time-to-first-API-call). Alex Albert: «Reducing time to first successful API call is the most important DevRel metric».
6. 5+ лет DevEd / technical content leadership **или** 5+ лет SWE + teaching (LangChain «and/or»).

**Сильно желательно:** YouTube-система, сертификация/Academy, работа с подрядчиками (video producers, writers), опыт DeepLearning.AI / HF courses / LangChain Academy.

**Первая строка:**

> Built the curriculum and video system that took developers from first API call to production agents — [N] courses, [YouTube], shipped on every major launch with product.

---

## 5. АНТИ-ПАТТЕРНЫ И ДИСКВАЛИФИКАТОРЫ

Режет на входе, даже при сильном профиле:

1. **Не пишешь код / не можешь задебажить чужой quickstart.** daily.dev Recruiter 2026: «Can we hire a non-engineer as a Developer Advocate? Generally, no.» Dan Goosewin (июль 2025): «If you're not an engineer or don't write code, you're a bad DevRel. Period.» Это консенсус hiring-менеджеров, не блог-мнение.
2. **Маркетолог с титулом DevRel.** Developers «spot non-technical advocates immediately».
3. **Нет URL-портфолио.** Manufact hiring: «share URLs» как шаг 1. Perplexity: «name the campaigns and quote the outcomes».
4. **Холодный веер в пустоту / «я готов евангелизировать что угодно».** Лаборатории берут людей, уже живущих в **их** экосистеме.
5. **Head-of-претензия без команды и без цифр экосистемы.** Anthropic 10+ years leading DevRel — и роль всё равно открыта месяцами: планка реальная.
6. **Невозможность IRL** на ролях, где JD on-site (Modal, LangChain on-site, Anthropic hybrid 25%+, Perplexity SF).
7. **Путаница DevRel и FDE.** FDE = встроить модель у клиента. DevRel = научить рынок. Смешивать в сопроводительном — сигнал, что не понял покупателя.

**Выглядит как плюс, читается как минус:**

| Кажется плюсом | Как читают |
|---|---|
| «10 лет Community Manager, 100k в Discord» | Нет технической глубины |
| «Я инфлюенсер, 200k в TikTok про AI» | Не developer audience |
| PhD в ML без публичного teaching | Уйдёт в research, не будет писать туториалы |
| «Делал DevRel в крипто 2021» | ZIRP-шрам; нужен перевод в «builder education», иначе баннер-риск |
| Слишком широкий «AI thought leader» | Нет продукта, который ты довёл до adoption |
| Победа в хакатонах без shipped content | Разовый спайк, нет аннуитета (Dewan Ahmed: talk = spike, tutorial = annuity) |
| «Готов релоцироваться в SF» без артефакта | SF не заменяет доказательство |
| Ambassador-бейдж без FTE-результата | Лаборатории уже наелись volunteer-программ |

---

## 6. КАК ОЦЕНИВАЮТ

Собрано из JD, публичных процессов и постов нанимающих. Универсального «официального loop лаборатории» в открытом виде мало — ниже то, что проверяемо.

### Типичные этапы (склейка OpenAI/GDM/Mistral/ElevenLabs/Manufact/Perplexity)

1. **Скрининг артефактов, не резюме.** URLs: GitHub, talks, courses, X. Pauline/Charlie вошли с уже известным брендом.
2. **Короткое видео / async intro** (Manufact: 1–3 мин, «nothing polished»).
3. **Intro с hiring manager** (основатель или Head: Sophia, Romain, Paige/Omar, Paul Asjes, Katelyn Lesse).
4. **Technical credibility.** Не LeetCode как у SWE, но: прочитай diff, напиши working example, найди ошибку в доках (Perplexity JD). GDM: MVP на Python/REST/JS.
5. **Take-home: демо + контент, который шиппится вместе.** Manufact прямо: «build a demo or integration and the content piece that ships with it». Это и есть работа.
6. **Panel: product + eng + существующий DevRel.** Кто решает: Head of DevEx / Head of Platform / Head of DevRel; у GDM — Paige/Omar как builders of the team; у OpenAI — Romain Huet.
7. **On-site / hybrid project** у Cursor-класса (Anysphere: 2-day on-site project; AI tools запрещены в первом coding round — это SWE, но сигнал культуры). Для DevRel on-site чаще «talk + working session», не contest.

### Что просят показать

- Time-to-first-success учебный актив.
- Одно видео, где **ты** на экране (LangChain DA: «starring you»).
- Метрики: не vanity. Campaigns with outcomes (Perplexity). HF/GDM — cookbook usage, SDK installs.
- Живой фидбек-петля: баг, который ты донёс до продукта.

### Длительность

- Рынок DevRel 2026: **6–12 недель** (daily.dev Recruiter; 8–12 weeks DA guide).
- Anthropic Head of DevRel: **>4 месяца открыт** при top-of-market pay — supply constraint, не медленный HR.
- GDM пакет февраля 2025 собрался быстро: люди уже были публичными, это inbound/poach, не воронка с нуля.

### На чём валятся

- Не могут live-demo без скрипта.
- Контент «маркетинговый»: эмодзи, нет runnable code.
- Не знают **конкретный** DX продукта (путают Claude API и ChatGPT; Gemini API и Vertex).
- Не умеют сказать «нет» хайпу — лаборатории (особенно Anthropic) режут eвангелизм без responsible-use.
- Comp-ожидание ниже SWE без готовности к «ты всё равно на сцене 30% времени».
- Для Head: нет истории **найма и системы**, только личный бренд.

ElevenLabs (Paul Asjes): если гонять всех через один coding interview — потеряешь iOS/Kotlin/embedded-голоса. Это важный anti-pattern процесса, не кандидата.

---

## 7. КОНВЕРТАЦИЯ НЕСТАНДАРТНОГО ОПЫТА

Рабочие переводы из этой выборки. Формула: **не оправдываться, а переименовать работу в job-to-be-done лаборатории**.

### Основатель провалившегося / свёрнутого стартапа

**Thariq:** PubPub + One More Multiverse (YC) wound down → Claude Code MTS. Перевод: «я строил инструменты, которыми думают люди, и научился выкидывать сложность» (его же тезис: complexity has to earn its keep; system prompt Claude Code резали на 80%).

**Charlie:** несколько стартапов (YC/500/StartX) → рассылка → OpenAI DevEx. Перевод: «я три года учил рынок пользоваться вашим API снаружи, теперь сделаю это изнутри».

Черновик строки: *Founded and wound down [X]; taught [N] builders through [artifact]; now I want the distribution of [Lab] instead of another company.*

### Крипто-бэкграунд

**Не нашёл** подтверждённого найма в скоуп-лабораторию 2024–2026, где главный сигнал — crypto DevRel. Не утверждаю, что дверь закрыта; в открытых анонсах этого нет. Если конвертировать — только через **builder education / infra / open-source**, не через token community.

### Не-кодер

**Полностью не-кодер в DevRel лаборатории — не найден.** Ближайшие:

- Pauline: была platform/SRE, потом community — но OpenAI DevEx всё равно «build with the tools myself».
- Sophia: data scientist, не SWE, но Python/AI ecosystem + PhD education.
- Joana: не кодер в начале; Google взял на community/programs TensorFlow, не на Staff DevRel Engineer.

Дверь не-кодера = **Developer Education / community platforms / Education Labs**, и даже там Anthropic пишет «because you are one» (developer). Kristen Swanson нанимает в Education Labs людей из learning sciences **рядом** с design engineer — это research/education, не классический DA.

### Неанглоязычный

Это **актив**, не дыра, если есть англоязычный слой.

- Omar: AI Learners на испанском → GDM. Перевод: «я уже построил ML-коммьюнити на языке, который лаборатория не покрывает».
- Luciano: PT-BR, Google DevRel Brasil, cookbooks Gemini.
- Sophia: переезд во Францию без французского — сама это ставит как proof of agency.

Перевод: *Built the [language] developer community for [stack]; can represent the lab in [region] without a local vendor.*

GDM публично: hiring anywhere, skillset > location.

### Немолодой

Жёсткого age-сигнала в наймах нет. Paige, Addy, Joana, Romain — senior careers. «Немолодой без публичного следа» в выборке не представлен. Возраст не фильтр; **устаревший стек без LLM-артефакта — фильтр**.

### Без визы

Прямых кейсов «взяли и заспонсили в DevRel лаборатории» в открытом виде мало. Mistral DA JD помечают H1B sponsored на джоб-агрегаторах; GDM remote-friendly globally; OpenAI Singapore/India roles. **Не нашёл** публичного оффера «мы спонсируем junior DevRel в SF». Практический перевод: remote-eligible роль (GDM, HF, ElevenLabs remote-first) или хаб с локальным правом на работу (London Mistral DevEd, Paris, Singapore OpenAI).

### Как формулировать неканон в первой строке

Плохо: «Non-traditional background in pharma/community/content seeking to break into DevRel.»

Хорошо, по реальным переводам:

- Joana-стиль: *Brought new technical practices into 47 countries' institutions; now I do that for developers adopting Gemini.*
- Sophia-стиль: *Educational psychologist who ran data-science advocacy at Anaconda; I build the teaching system around models, not the models.*
- Pauline-стиль: *I help people become builders — coding classrooms, Gitpod community, Vercel community, now OpenAI.*
- Charlie-стиль: *I wrote the unofficial curriculum for building on OpenAI for three years; hiring me is bringing that in-house.*

---

## 8. ИСТОЧНИКИ И ДЫРЫ

### Ключевые первоисточники

- Anthropic Developer Education Lead JD (startup.jobs, вилка $290–365k, роль закрыта): https://startup.jobs/developer-education-lead-anthropic-3-7774080
- Anthropic Founding Developer Relations Lead: https://www.anthropic.com/careers/jobs/4781798008
- Anthropic Claude Evangelist $240–315k: IBTimes / SightsIn Plus, май 2026
- swyx, «DevRel is Unbelievably Back», 13.10.2025: https://dx.tips/devrel-is-back
- GDM first DevRel team, Paige Bailey, 8.02.2025: https://x.com/DynamicWebPaige/status/1888267547923066973
- Philipp Schmid join GDM: https://x.com/_philschmid/status/1887518149295132694 и https://www.philschmid.de/philipp-schmid
- Omar Sanseviero GDM team post: LinkedIn, 8.02.2025
- TechCrunch, AI Studio → DeepMind, 9.01.2025: https://techcrunch.com/2025/01/09/google-folds-more-ai-teams-into-deepmind-to-accelerate-the-research-to-developer-pipeline/
- Pauline → OpenAI: https://www.pawlean.com/posts/joining-openai
- Charlie Guo → OpenAI: https://www.ignorance.ai/p/on-joining-openai
- Lee Robinson Cursor: https://leerob.com/cursor ; Arena Mag «School of Lee»; The Org
- Alejandro AO → HF: https://x.com/_alejandroao/status/1990349384580788632
- Joffrey Thomas → Mistral: LinkedIn, 29.04.2025
- Sophia Yang The Org + farewell X 17.08.2026
- Joana Carrasqueira: https://www.linkedin.com/pulse/from-pharmaceutical-scientist-head-developer-relations-yiyang-hibner-qjj1c
- Andrew Reed → LangChain: https://x.com/andrewrreed/status/1970141539574677884
- Addy Osmani → Anthropic: https://x.com/addyosmani/status/2097210828659564642
- Kristen Swanson Education Labs: EdTech Innovation Hub, 10.07.2026
- LangChain DA LangSmith $200–240k: ZipRecruiter / Jobright
- LangChain Education Engineer: https://jobs.ashbyhq.com/langchain/9037a071-eef2-4083-83f3-d6a5b531aa1d
- Modal DevRel Eng $175–275k: Essence VC / Simplify
- Mistral AI Developer Advocate JD: Remotive / Jobright
- Perplexity DevRel Manager JD: JobsByCulture / Dice
- ElevenLabs Paul Asjes hiring: https://x.com/paul_asjes/status/1993660120648253497
- Katelyn Lesse hiring Anthropic DevRel: https://x.com/katelyn_lesse/status/1986879086506221945
- Cat Wu interview (DevRel names Tarek, Lydia): YouTube «How Anthropic’s product team moves faster»
- Alex Albert DevRel announce: https://x.com/alexalbert__/status/1769773258222813197
- BI, Alex Albert as head of developer relations, окт. 2025
- daily.dev Recruiter, How to Hire DAs 2026: https://recruiter.daily.dev/resources/hire-developer-advocates-devrel-engineers/
- JobsByCulture, DevRel careers 2026: https://jobsbyculture.com/blog/developer-relations-careers-2026
- Angie Jones, DevRel's biggest stage yet (YouTube, 2025): «start doing the job now… put it on my resume»

### Что проверить не удалось («не нашёл», не «этого нет»)

1. Кто занял Anthropic **Developer Education Lead** ($290–365k) и **Head of Developer Relations** ($385–460k) — JD снимали, имён в прессе нет.
2. Кто занял Perplexity **Developer Relations Manager** — вакансия жила ≥14 месяцев.
3. Именные DevRel-наймы **xAI, Together AI, Fireworks, Modal, Scale AI, Cohere, Meta Llama** за 24 месяца в открытых анонсах.
4. Полный состав OpenAI DevEx кроме Romain Huet, Charlie Guo, Pauline Narvas (Romain упоминает Dominik и Joe как hiring circle Pauline — Joe = Joseph McCormick по её посту; Dominik Kundel — известный Twilio/Stripe DevRel, **подтверждения, что Dominik FTE OpenAI, в этом проходе не искал до конца**).
5. Биографии Mistral Harizo Rajaona и Clément C.
6. Конверсия Claude/Codex Ambassador → FTE.
7. Отвергнутые кандидаты (паттерн «чего нет у отвергнутых») — публично почти не существуют; реконструкция только от обратного через JD и прошедших.
8. Внутренние scorecards, кто имеет veto (research vs GTM vs legal).
9. Точный process loop Anthropic DevRel (сколько интервью, есть ли take-home).
10. Виза/sponsorship-статистика по этим ролям.

---

## Вердикт для заявки

**Идти в Developer Education, не в «Head of DevRel». Head — узкая дверь для людей, которые уже построили функцию. DevEd — широкая дверь для человека с публичным учебным артефактом и руками разработчика.**

Что совпадает с прошедшими неканонами: курс/гайд/рассылка/YouTube, которые разработчики уже используют; умение собрать demo в день запуска; формулировка «я учу билдеров шипить на вашей платформе», не «хочу в AI». Что не совпадает: FAANG-SWE, ML-PhD, холодная заявка без URL.

Комп: если цель лаборатория — ориентир IC $180–270k base, lead DevEd Anthropic-класса $290–365k, Head $385–460k. Ниже $150k на Bay Area DevRel лаборатории в этой выборке почти нет (ElevenLabs ранний JD $80–150k — исключение ранней стадии).

---

**Проверено:** публичные анонсы найма, JD с вилками, first-party блоги (Pauline, Charlie, Lee, Philipp), X hiring-менеджеров, TechCrunch по реоргу GDM, swyx по рынку DevRel 2025. **Не проверено:** внутренние офферы и отклонённые воронки (закрыты); заполнение «снятых» JD без имени; LinkedIn-профили за paywall — где не было first-party поста, человека в таблицу не ставил.

---

🧒 **Простыми словами:** лаборатории берут не «человека с правильным дипломом», а человека, которого разработчики уже слушают, и который умеет сам собрать рабочий пример. Учить (курсы, видео, учебник) — дверь шире, чем быть звездой на сцене. Звезда на сцене без учебника висит в вакансии месяцами даже за огромные деньги.
