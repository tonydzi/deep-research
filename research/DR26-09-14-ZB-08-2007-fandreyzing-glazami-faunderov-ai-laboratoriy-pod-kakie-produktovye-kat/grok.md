# DR26-09-14-ZB-08-2007 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала подниму первоисточники по фаундерам, фондам и раундам 2025–2026, затем сверю цифры на живых страницах.Дальше открываю первоисточники по фондам, YC и категориям, которые заказчику реально доступны.Открываю первоисточники с цитатами и добиваю пробелы по агентной инфре, знаниям, роботам и советам мелким командам.A. Executive summary

1. Деньги 2025–2026 концентрируются в двух полюсах: (а) гигантский capex frontier-лабораторий и физической инфры, (б) application-компании с доказанным «продаём работу», а не чат. Медиана рынка при этом гораздо скромнее мега-раундов. PitchBook/Carta/CB Insights рисуют один и тот же разрыв: AI забирает 48–80% венчурных долларов, но львиная доля — мегараунды $100M+. Уверенность: высокая по направлению, средняя по точным процентам из-за разных методик издателей.

2. Sam Altman 6 ноября 2025 публично зафиксировал экономику OpenAI: «We expect to end this year above $20 billion in annualized revenue run rate… We are looking at commitments of about $1.4 trillion over the next 8 years». Это не «маленькая лаборатория поднимает seed». Это государство-в-компании, где чек нужен, чтобы купить гигаватты. Уверенность: высокая (первоисточник — пост Altman, пересказан TechCrunch).

3. Dario Amodei говорит обратное Altman по P&L, но то же по капиталу: scaling требует денег, однако каждая модель как отдельный бизнес «на самом деле прибыльна», а капекс следующей маскирует это. К сентябрю 2026 FT (через вторичные пересказы) пишет, что Anthropic сообщает инвесторам о втором подряд квартале положительного adjusted operating income; Q2 revenue $11.5B, ARR к концу июля ~$65B. Caveat огромный: маржа >80% до revenue-share с Amazon и до стоимости обучения. Уверенность: средняя (нет проспекта IPO, только «people familiar»).

4. Arthur Mensch 8 сентября 2026 закрыл €3B Series D при >€21B — крупнейший европейский tech equity round. Тезис не «ещё одна frontier-лаба», а контроль стека: данные, модель, compute, аудит. Цитата: «Organisations want to industrialise AI inside systems that are already complex: their own data, their own tools, their own compliance constraints. That’s the real problem.» Для европейского фаундера это важнее американского «just scale». Уверенность: высокая.

5. Чистый research-тезис без продукта поднимается только с именем уровня Ilya/Mira. SSI: $1B seed (сен 2024, $5B), ~$2B (апр 2025, $32B), затем 27 июля 2026 Nvidia «substantial investment» + Vera Rubin, FT/Bloomberg называют ~$5B. Цитата Ilya: «We have research that is worthy of scaling up». Thinking Machines: $2B seed при $12B (июль 2025); раунд на $40B+ в сентябре 2026 ещё «in talks». Для команды без pedigree OpenAI/DeepMind это закрытая дверь. Уверенность: высокая по SSI/TML seed, средняя по $5B Nvidia.

6. Application-слой, который реально поднял крупно в 2025–2026, продаёт outcome, не «платформу агентов». Cursor: $2.3B Series D 13 ноября 2025 при $29.3B, >$1B ARR к тому моменту. Sierra: $350M при $10B (сен 2025), $100M ARR за <2 года; позже вторичные источники пишут $950M при $15.8B (май 2026). Harvey: $550M при ~$15.5–15.6B (9 сен 2026), ARR >$400M. Общее: workflow ownership + скорость выручки. Уверенность: высокая по Cursor/Sierra-2025, средняя по Sierra-2026/Harvey-сент (свежие пересказы).

7. Sequoia 14 января 2026 в эссе «2026: This is AGI» дала инвестиционный канон года: «Long-horizon agents are functionally AGI, and 2026 will be their year». Совет фаундерам: перестать продавать «talkers», продавать «doers»; harness, feedback loop, outcome pricing. a16z 3 сентября 2026 (Seema Amble) предупреждает: инкамбенты + Claude уже едят retrieval/RAG; вертикаль выигрывает только если делает job лучше, чем SoR+общий агент. Уверенность: высокая (открытые эссе).

8. YC Fall 2026 RFS больше не просит «ещё один GPT-wrapper». Просит: multiplayer AI, company-scale small software, compliance infra, proving you’re human, physical-world OS, compute at sea, AI-native services. Generic RAG / «второй мозг как продукт» в списке нет. Уверенность: высокая (официальная страница YC).

9. Медианы (не мега-имена). PitchBook-NVCA H1 2026: seed ~$3.0M при ~$18M pre; Series A ~$19.4M при ~$64M pre (~$83M post). AI premium на Series A ~+89% vs non-AI ($83M vs $44M pre). Carta Q1 2026: AI foundational-model Series A median $300M vs $55M non-AI. Вывод: «AI» без трекшена не даёт $300M; даёт его только foundation-model когорта. Уверенность: высокая по направлению, средняя по точным медианам (разные базы Carta vs PitchBook).

10. Категории, которые инвесторы прямо называют перегретыми (PitchBook AI Outlook, дек 2025): second-tier medical scribes, часть precision medicine, aerial defense drones, AI gaming content, части codegen, AI marketing wrappers, second-wave AI cyber, search-as-a-service, AI & data science platforms «не OpenAI/Anthropic». TechCrunch-опрос VC (март 2026, вторичный Superframeworks): thin wrappers, generic productivity, basic CRM, integration-only MCP-мосты. Уверенность: средняя (аналитика, не deal-by-deal).

11. Агентная оркестрация как «ещё один LangChain» — трудно. LangChain сам $125M при $1.25B (окт 2025) уже занял open-source harness. Легко — если оркестрация спрятана внутри продаваемого job (Sierra, Harvey, Cursor). Аудит/evals — средне-легко при логотипах: Braintrust $80M Series B при $800M (17 фев 2026, ICONIQ), клиенты Notion/Replit/Cloudflare/Ramp/Dropbox. Уверенность: высокая.

12. Enterprise knowledge / RAG / «второй мозг»: окно generic-поиска закрывается. Glean $150M Series F при $7.2B (июнь 2025); к июню 2026 раунда нет, ARR якобы ~$300M (~24x) — «самый трезвый мультипль» когорты, но и сигнал, что категория перестала раздуваться. a16z прямо: retrieval assistant — зона инкамбента. Уверенность: средняя по ARR Glean (не company filing).

13. Китайская модель без венчура жива как исключение, не как шаблон. Liang Wenfeng годами говорил, что VC хотят быстрый exit и коммерцию; DeepSeek кормился с quant-фонда High-Flyer. Июнь 2026: первая внешняя серия ~50B RMB / ~$7.4B при ~$52–59B, сам Liang — крупнейший чек (~20B RMB), контроль ~84%. Повторить в Европе без своего хедж-фонда нельзя. Уверенность: средняя (SCMP/Reuters, не filing).

14. Для профиля заказчика (оператор, крипто-фаундер, PhD не-ML, Европа, флот агентов + аудит + знания) структурно закрыты: новая frontier-лаба, чипы/энергия, SSI-style research без продукта. Реалистичны: (1) evals/audit как production infra, (2) вертикальный outcome-агент, (3) AI-native service «продаём работу», (4) multiplayer/company-brain под один named job. Минимальный сигнал «поговорим»: 2–3 платящих design partner + метрика outcome (не демо) + публичный артефакт (бенч/open eval). Уверенность: высокая как синтез, низкая как предсказание конкретного чека.

15. Коррекция уже видна не как «AI умер», а как развилка качества. Public AI multiples сжались в Q1 2026 (PitchBook: vertical conglomerates ~4.5x EV/rev). Altman в сентябре 2026 ругает «unsustainable silliness» у neocloud без контрактного спроса. Amodei зовёт «pace the frontier». Для мелкой команды это значит: трекшен и unit economics важнее нарратива «агенты». Уверенность: средняя (смесь CEO-комментариев и аналитики).

---

B. Карта категорий

Формат: категория | легко/трудно | почему | чек и стадия | что доказать | прецеденты | окно

1. Foundation models / новая frontier-лаборатория | Почти невозможно (без OpenAI/DeepMind/Meta pedigree) / Легко только для 5–8 имён | Capex теперь триллионного порядка; инвесторы финансируют либо incumbents, либо «неолибы» с лицом. Amodei: 3–6 terminal frontier players. Radical Ventures: NeoLabs подняли за год ~7x того, что OpenAI+Anthropic собрали за 7 лет до ChatGPT — но это выходцы из лаб. | Seed уже $1–2B; valuation $5–12B на нуле продукта (SSI, TML). Дальше — стратеги Nvidia/Google, не классический Series A. | Имя + команда из frontier-лабы + research claim, который «worthy of scaling». Продукт не требуется. | SSI $1B@$5B (сен 2024), $2B@$32B (апр 2025), Nvidia substantial / ~$5B (27 июл 2026). Thinking Machines $2B@$12B (15 июл 2025). Mistral €3B@>€21B (8 сен 2026) — уже с ARR-треком, не «чистый research». | Окно для новых имён закрыто. Для выходцев из лаб ещё открыто, но сужается к стратегическому compute, не к VC-seed.

2. Inference-инфра, чипы, энергия, дата-центры | Легко для тех, у кого контрактный backlog и megawatts; трудно для software-only | a16z Machine Age Fund $1.1B (8–9 сен 2026) — отдельный бакет на железо. Altman: compute — bottleneck; одновременно он же называет часть neocloud-экспансии «unsustainable silliness». Связка: деньги есть, если есть offtake. | Growth: сотни миллионов — миллиарды; часто долг + equity + hyperscaler. Seed «ещё один GPU cloud» — уже кладбище. | Контракты (backlog), площадка/энергия, supply Nvidia/TSMC. | CoreWeave backlog $104B (цитируется в репортажах про Altman, сен 2026). Mistral $830M debt на DC (март 2026, вторично). Figure+Nscale до $3.5B compute (3 сен 2026). | Окно открыто на physical layer (power, cooling, networking, memory). Закрывается на undifferentiated GPU-resale.

3. Агентная инфраструктура и оркестрация | Трудно как горизонтальный «агентный Airflow»; средне, если это harness внутри продукта | Sequoia: harness — зона application layer (Manus, Claude Code, Factory). LangChain уже unicorn ($1.25B, окт 2025) на open-source агентном стеке. YC Spring/Summer 2026 забиты «слоями» (auth, memory, sandbox, insurance for agents) — перегрев plumbing. | Seed $3–15M; Series A $15–40M только при usage/revenue. Не $100M без логотипов. | Production usage (не GitHub stars), switching cost, почему не LangGraph/Claude Code. | LangChain $125M@$1.25B (21 окт 2025). Горизонтальные orchestration-стартапы без выручки — не нашёл сопоставимых мега-раундов 2025–26. | Окно горизонтали закрывается. Окно «harness под job» открыто.

4. Оценка, тестирование, наблюдаемость, АУДИТ агентов/моделей | Средне-легко | Единственная picks-and-shovels категория, где покупатель — тот же, кто уже жжёт токены. Braintrust прямо: traces стали «hundreds of megabytes per interaction». Конкуренция плотная (LangSmith, Phoenix, Galileo, Langfuse/ClickHouse), но чеки идут. | Seed $5–15M; Series A ~$25–40M; Series B $80M@$800M (Braintrust, фев 2026). | 5–10 design partners с production traces; evals в CI; логотипы AI-native компаний. Не «наш фреймворк на arXiv». | Braintrust $36M Series A (окт 2024, a16z) → $80M B @$800M (17 фев 2026, ICONIQ). LangSmith внутри LangChain unicorn. Galileo ~$45M cumulative (вторично, май 2026). | Окно открыто 12–24 месяца, пока агенты идут в prod. Дальше — консолидация (Langfuse уже у ClickHouse, янв 2026, вторично).

5. AI security и guardrails | Средне | Покупают, когда агент трогает деньги/PII. Harvey 9 сен 2026 купил Guardrails AI вместе с раундом — сигнал, что security вшивают в вертикаль, а не оставляют отдельным вендором. PitchBook: «second-wave AI cyber» перегрет. | Seed/A: $10–40M. Mega — редко. | Red-team на чужих агентах, regulated design partner, не «wrapper вокруг Llama Guard». | Harvey + Guardrails AI (сен 2026). Отдельный крупный 2026-раунд чистого agent-security unicorn — не нашёл в этом прогоне с первичной цитатой. | Окно узкое: либо встраиваешься в Harvey/Sierra/Cursor, либо берёшь compliance (YC RFS: AI-native compliance). Горизонтальный «AI firewall» — кладбище второй волны.

6. Вертикальные AI-приложения (юр, мед, финансы, промышленность) | Легко в legal/support/coding-adjacent; средне в health; трудно во втором эшелоне scribes | a16z: вертикаль побеждает focus + learning loop + ответственность за результат. Sequoia приводит Harvey как «associate». PitchBook: second-tier medical scribes — overheated. | Seed $5–20M при 1–2 logo; A $20–80M при $1–5M ARR; рост — сотни миллионов при $100M+ ARR. Мультипликаторы 30–100x ARR на гиперросте, сжимаются. | Design partners из AmLaw/Fortune, retention, outcome pricing, данные/evals профессии (Harvey Tenet: 1,750 legal-task environments). | Harvey $160M@$8B (дек 2025, NYT) → $200M@$11B (мар 2026, вторично) → $550M@$15.5–15.6B (9 сен 2026), ARR >$400M. Sierra $350M@$10B (4 сен 2025), $100M ARR (21 ноя 2025). | Окно открыто там, где job повторяется, эксперт может оценить качество, инкамбент не владеет всем job. Закрыто для «ещё один copilot к Salesforce».

7. Coding agents и dev-tools | Легко для лидера с выручкой; почти невозможно для nth Copilot | Самый горячий app-слой 2025. К концу 2025 Cursor уже >$1B ARR. Labs (OpenAI/Anthropic) сами лезут в IDE. Truell: их продукты — «concept car», Cursor — production car. PitchBook: «parts of code gen» overheated. | После product-market fit — миллиарды. До PMF seed обычный, но Series A без usage не будет «AI premium». | WAU разработчиков, enterprise seats, % кода, написанного агентом, gross margin после inference. | Cursor/Anysphere: $900M@$9.9B (5 июн 2025, Thrive), $2.3B@$29.3B (13 ноя 2025, Accel+Coatue; Nvidia+Google). Cognition/Devin — a16z публично хвастает (сен 2026). | Окно №1 закрыто (editor). Окно №2 (агент-SRE, миграции, security patching — Sequoia: Traversal, XBOW) ещё открыто.

8. Enterprise knowledge, RAG, «второй мозг», память | Трудно / почти невозможно как generic product | a16z 3 сен 2026: retrieval assistants — зона SoR + Claude. YC RFS Fall 2026 просит не «company brain как поиск», а multiplayer agents и small software. Glean после Series F не переоценился год. | Late-stage ещё пишет чеки победителю (Glean $150M@$7.2B, июн 2025). Seed generic RAG — pass. | Permissions, connectors, measurable time-to-answer vs Copilot/Glean/Claude, не «наш векторный индекс». | Glean $150M Series F @$7.2B (июнь 2025). Новых 2026-мега раундов «ещё один Glean» не нашёл. | Окно generic RAG закрыто. Окно «память/контекст как часть job-агента» открыто внутри вертикали.

9. Data layer: разметка, синтетика, лицензирование | Средне, с развилкой | Meta купила 49% Scale за $14.3B при implied ~$29B (13 июн 2025, Reuters). Затем Google/OpenAI/Microsoft резали закупки из-за нейтральности. Синтетика и expert data (Harvey Tenet, Mercor — вторично) важнее commodity labeling. | Late: миллиарды (Scale). Seed expert-data: $10–50M. | Лаборатория как клиент ИЛИ expert network с качеством RLHF; после Scale-Meta — доказательство, что ты не «ещё один Outlier». | Scale Meta $14.3B / ~$29B (июн 2025). | Commodity labeling окно закрывается. Expert/domain data и eval data — открыто.

10. Consumer AI | Трудно, но YC снова зовёт | YC Fall 2026: «CONSUMER is going to be so back» — токены дешевеют 10x/год. Факт рынка: «единственная новая иконка — ChatGPT». Perplexity: ~$200M@$20B (сен 2025), ARR-оценки $500–750M на 2026 (Sacra, не filing). | Seed consumer AI — обычный YC-чек. Рост без 10M+ MAU почти не поднимает A. | Retention, habit, не CAC-сжигание; путь к подписке. | Perplexity $500M@$14B (май/июн 2025), $200M@$20B (сен 2025). | Окно «ещё один чат» закрыто. Окно узкого consumer (aging, tutor/Primer, identity) YC считает открытым — это ставка, не доказанный рынок.

11. AI-native services / «сервис, продаваемый как софт» | Легко-средне на seed, если есть outcome | Sequoia: «Remember all that talk of selling work? Now it’s possible.» YC RFS (Summer/Spring 2026) прямо просит AI-native service companies. Sierra — эталон: outcomes-based pricing, $100M ARR за 21 месяц. | Seed $2–8M; A при $1–3M ARR. Не $10B без Bret Taylor. | Paid jobs completed, gross margin после людей+токенов, не headcount-агентство. | Sierra (выше). YC W26: 56 компаний (28%) как AI-native service (вторичный разбор Extruct). | Окно открыто. Цена ошибки — стать BPO с GPU-счётом.

12. Опенсорсные модели и инфраструктура вокруг них | Средне в Европе/суверенитете; трудно в США как «ещё один Llama» | Mistral строит тезис на open-weight + private compute. Sonya Huang (Sequoia, авг 2026): портфельным компаниям — владеть весами в кусках стека; «This is new for 2026». Hugging Face: слухи о сделке Nvidia (сен 2026, вторично, не подтверждаю как факт). | Seed infra вокруг OSS: $10–40M. Сами веса без distribution — не VC-история. | Downloads + enterprise support + sovereign customers. | Mistral Series C €1.7B @ €11.7B (сен 2025, ASML); Series D €3B @ >€21B (8 сен 2026, Samsung + Scaleup Europe Fund). | Окно «европейский суверенный стек» открыто (политика + Scaleup Europe). Окно «форк Llama с сайтом» закрыто.

13. Робототехника и world models | Легко для humanoid/VLA с капиталом и данными; почти невозможно как garage robotics | YC: AI moving into physical world. Figure: >$1B Series C @ $39B post (16 сен 2025); 3 сен 2026 — до $3.5B compute с Nscale. Physical Intelligence ~$400M @ $2.4B (кон. 2024, для тренда). Это не seed-история португальской софт-лабы. | Series B/C: сотни миллионов — миллиард. Seed world-model lab — только с DeepMind/PI pedigree. | Реальные часы робота, завод, данные. | Figure Series C (сен 2025). Robocurve $10M seed на independent physical eval (14 сен 2026) — нишевый аудит, не humanoid. | Окно капитала открыто. Для софт-фаундера без железа — закрыто, кроме eval/data/OS for physical work (YC RFS).

Кладбище / перегрев (отдельным блоком)

- Thin GPT/Claude wrappers и generic productivity — VC pass 2026 (Superframeworks/TechCrunch survey, март 2026).
- Generic RAG / «ChatGPT по своим PDF» — убито Copilot + Claude + Glean.
- Second-tier medical scribes — PitchBook overheated.
- N-th coding copilot без собственной среды и выручки.
- Horizontal «agent orchestration platform» без стандарта.
- Search-as-a-service (PitchBook).
- Second-wave AI cyber / «ещё один guardrail».
- AI marketing content tools.
- Commodity data labeling после Scale-Meta.
- Foundation-model pretender без compute и без имени.

---

C. Голоса фаундеров

Sam Altman (OpenAI)

Прямая цитата, 6 ноября 2025, X, через TechCrunch: «We expect to end this year above $20 billion in annualized revenue run rate and grow to hundreds of billion by 2030. We are looking at commitments of about $1.4 trillion over the next 8 years.» Также: хочет продавать «AI cloud». Интерпретация: фандрейзинг лаборатории = финансирование энергосистемы, не seed. Отдельно Bg2 Pod / Yahoo (ноя 2025): на вопрос «как компания с $13B revenue делает $1.4T commitments» — «If you want to sell your shares, I’ll find you a buyer. Enough.» (цитата по Yahoo, подкаст не расшифровывал сам). Сентябрь 2026 (вторично Superpower Daily): «unsustainable silliness» про neocloud без выручки. Совет мелким (старый канон YC + 2026 Axios): «revenge of the idea guys» — идея+пользователь важнее кода, потому что модели пишут код; одновременно «next generation of models are going to be sobering». Напряжение: он зовёт строить маленькое и одновременно жжёт триллион. Уверенность по $20B/$1.4T: высокая. По «idea guys»: средняя (вторичный пересказ Axios, 8 сен 2026).

Dario Amodei (Anthropic)

Прямых длинных цитат с открытой страницы Anthropic в этом прогоне мало; опора — Dealroom-конспект Cheeky Pint (май 2026, John Collison): «If every model was a company, the model is actually profitable»; coding — самый быстрый use case; API «не commoditized»; «We might be relatively close to the final set of players». Эссе «We Must Pace the Frontier» (суббота, ~12 сен 2026) — призыв замедлить capability. FT 13–14 сен 2026: компания говорит акционерам о втором квартале adjusted operating profit; Q2 rev $11.5B; ARR $65B end-July vs $9B end-2025; GM >80% до Amazon share и training. Интерпретация: инвестору в 2026 уже мало «сжигаем во имя scaling»; нужна история прибыльности, хотя бы adjusted. Уверенность: средняя.

Arthur Mensch (Mistral)

Прямая цитата, 8 сен 2026, TNW: «Organisations want to industrialise AI inside systems that are already complex: their own data, their own tools, their own compliance constraints. That’s the real problem.» Французский парламент, 12 мая 2026 (стенограмма Assemblée): компания ~1000 человек, valuation 12 млрд евро на тот момент, цель 1 млрд выручки к концу года; 30% Франции, 75% Европы; без extra-EU поставщиков «очевидно нет». Февраль 2025, TNW: «Europe needs to invest in owning and operating the infrastructure so that the money that is being made will not just go back to the hyperscalers in the US.» Интерпретация для мелких европейцев: Mensch не говорит «переезжайте в SF любой ценой»; говорит «владейте инфрой и compliance». При этом сам поднимает Samsung/ASML/a16z. Уверенность: высокая.

Demis Hassabis (DeepMind)

Первичный голос 2026 — через биографию Mallaby (март 2026, Axios): продажа Google в 2014 была ответом на то, что VC не тянут «shitload of computers». Попытка спина-аута с Reid Hoffman ($1B pledge) не прошла. 2026: DeepMind — единственная крупная лаба без собственного IPO-трека, потому что Alphabet печатает кэш (capex guide $175–185B на 2026, Axios/247). Isomorphic Labs: $2.1B Series B (май 2026, Thrive) — корпоративный research, который всё же берёт внешние деньги под drug discovery. Интерпретация: независимая лаба проигрывает корпоративному cash-flow, если нет ChatGPT-like revenue. Уверенность: средняя (книга + пресса, не интервью 2026 слово-в-слово про фандрейзинг).

Liang Wenfeng (DeepSeek)

Forbes 30 янв 2025 цитирует 36Kr 2023: VC хотят «exit and hope to commercialize products as soon as possible» — не стыкуется с research. Self-fund через High-Flyer. SCMP 18 июн 2026: ~50B RMB raise, post ~400B RMB / ~$59.2B; Liang personally ~20B RMB; на встрече с инвесторами: DeepSeek хочет «anything relevant to enhance the intelligence» и не интересуется остальным. Вторичный «118 answers» (июль 2026, непроверенный транскрипт): «the government will not give us a penny» — не использую как твёрдый факт. Интерпретация: анти-VC модель работает, только если есть свой денежный станок. Уверенность по раунду: средняя; по мотивам 2023: средняя.

Aravind Srinivas (Perplexity)

Первичных длинных цитат «почему нам дали деньги» в этом прогоне не открыл. Факты раундов: $500M @ $14B (июнь 2025), $200M @ $20B (сен 2025, The Information/TechCrunch-контур), Series E-6 ~$21–23B early 2026 (трекеры). Sacra: ARR ~$750M к авг 2026 — оценка, не disclosure. Интерпретация рынка: consumer+answer engine поднимает, пока есть рост пользователей и Nvidia/Bezos на капе. Почему дали: distribution (поиск как привычка) + цитирование источников как UX-моат, не research pedigree уровня Ilya. Уверенность по цифрам раундов: средняя-высокая; по ARR: низкая-средняя.

Michael Truell (Cursor / Anysphere)

Прямая цитата, BusinessWire 13 ноя 2025: «We believe that coding will be the single biggest driver of global productivity over the next decade… This funding allows us to dramatically increase our investment in research and product.» TechCrunch 9 дек 2025, Fortune AI Brainstorm: lab coding products — «concept car»; Cursor берёт best intelligence рынка + свои модели + «the best tool and end UX». Thrive (Miles Grimshaw, подкаст 2026): инвестировали в человека за год до Series A, без spreadsheet, через recruiting. Интерпретация: чек дали за pull пользователей и скорость ARR ($1M→$100M→$1B), не за PhD. Команда — MIT undergrads, YC S22. Уверенность: высокая.

Bret Taylor (Sierra)

TechCrunch 4 сен 2025: $350M @ $10B, Greenoaks; founded early 2024 с Clay Bavor; hundreds of customers (SoFi, Ramp, Brex). TechCrunch 21 ноя 2025: $100M ARR за 21 месяц; основатели: «That’s a heck of a lot quicker than we expected.» Вторично CNBC май 2026: «A watchful, waiting approach in AI is a path to extinction»; 40%+ Fortune 50. Не ML-исследователь: Salesforce co-CEO, Facebook CTO, председатель совета OpenAI. Интерпретация: это прецедент №1 «оператор/продукт поднимает агентную компанию на миллиарды». Уверенность по 2025: высокая; по $950M/2026: средняя (CNBC/Sacra, company post не открывал).

Ilya Sutskever (SSI)

NVIDIA PR 27 июл 2026, прямая цитата: «We have research that is worthy of scaling up, and having access to a big NVIDIA computer will let us do so.» Dwarkesh, ноя 2025 (OfficeChai): «SSI has raised $3 billion… a lot of their compute goes for inference… If you want to have a product… you need a big staff of engineers, salespeople… what’s actually left for research, the difference becomes a lot smaller.» Про монетизацию: «Right now, we just focus on the research, and then the answer to that question will reveal itself.» Канон компании: first product = safe superintelligence, nothing else until then. Интерпретация: это не шаблон для маленькой лабы; это ставка на человека, который сделал AlexNet/GPT/o1. Уверенность: высокая.

Mira Murati (Thinking Machines)

WIRED/TechCrunch 15 июл 2025: $2B seed @ $12B, a16z lead; «largest seed in history»; продукта нет. X Murati (цитирует WIRED): multimodal, «through conversation, through sight, through the messy way we collaborate»; open-source component. Сен 2026: Accel in talks $1B @ ≥$40B (The Information/TechCrunch) — не closed. Turnover кофаундеров обратно в OpenAI. Интерпретация: seed дали за OpenAI-CTO brand; следующий раунд уже требуют какую-то выручку (источники: >$100M run-rate — расходятся). Уверенность по seed: высокая; по 2026 round: низкая (in talks).

Что фаундеры говорят мелким командам

- Altman/YC-канон: make something people want; не поднимать «потому что AI». 2026-добавка: идея снова в цене, потому что код дешевеет — но это совет строить продукт, не лабу.
- Mensch: не уезжайте из Европы обязательно; берите суверенитет и industrial customers. Talent в Европе есть, структура (капитал, compute) — нет.
- Truell косвенно: UX+harness вокруг чужих моделей — легитимный бизнес, если users платят.
- Taylor косвенно: продавайте completed work, не SDK.
- Ilya: если вы не он — не копируйте «no product, $32B».
- Sequoia фаундерам (это уже инвестор, но адресовано билдерам): obsess over harness, sell work, price to outcomes.

Не нашёл в этом прогоне отдельного «письма Altman/Dario мелким AI-лабораториям Европы 2026» с практическим фандрейз-чеклистом.

---

D. Голоса инвесторов

Sequoia (Pat Grady, Sonya Huang) — «2026: This is AGI», 14 янв 2026

Прямо: «AGI is here, now.» «Long-horizon agents are functionally AGI, and 2026 will be their year.» «AGI is the ability to figure things out.» Приложение 2023–24 = talkers; 2026–27 = doers, коллеги, all-day usage, «selling work». Экспонента METR: горизонт задач агентов удваивается ~каждые 7 месяцев. Инвестируемо: harness, feedback loop, outcome packaging. Примеры: Harvey = associate, XBOW = pentester, Traversal = SRE. Sonya Huang авг 2026: куски стека надо «own down to the weights» — evals → post-training; не обязательно слезать с Opus/GPT. Уверенность: высокая.

a16z

Seema Amble, 3 сен 2026, «The Incumbents Are Coming»: SoR + Claude может съесть AI-native app. Стартап выигрывает только focus на job, learning loop, ответственность за результат. Иерархия агентов: retrieval → process → policy → principal. Retrieval — уже не стартап-территория. Harvey Tenet — как «смастерили curriculum» без лет клиентских данных. Marc Andreessen, 9 сен 2026, «Investing in Cognition»: агенты пишут 90%+ кода Cognition; 10x engineer → 100x. Machine Age Fund $1.1B (8–9 сен 2026) — железо, DC, роботы, consumer devices. Интерпретация: a16z одновременно качает apps, infra и atoms; не качает wrappers.

YC

Fall 2026 RFS (открытая страница): Primer/education, American defense, cloud for small software, multiplayer AI, compute at sea, consumer for 1B, aging, OS for physical world, crypto rails, real-world data, proving you’re human, AI-native compliance, self-maintaining APIs. Это карта «что возьмут». Чего нет: generic RAG, generic agent framework, nth chatbot.

Greenoaks / Thrive / Accel / Coatue / ICONIQ / Benchmark / Lightspeed

По сделкам, не по эссе: Greenoaks — Sierra и SSI (рост без классического SaaS-метрического стыда). Thrive — Cursor (два раунда подряд). Accel+Coatue — Cursor D. ICONIQ — Braintrust B. Lightspeed — Harvey сен 2026. Benchmark — Sierra с ранней стадии. Паттерн: либо founder brand (Taylor, Ilya, Murati), либо сумасшедший pull (Cursor), либо workflow lock-in (Harvey).

Khosla / Founders Fund / Conviction / Amplify / Basis Set / GC

Публичных эссе 2025–26, которые я открыл целиком, нет. GC — в Mistral. Conviction — Harvey. Не выдумываю их «тезисы».

Carta / PitchBook как «голос рынка»

AI = 60%+ долларов Carta Q1 2026; 94% AI-funding в сделках ≥$100M (CB Insights Q1 2026, вторично). Медиана и среднее разъехались: средний Series A ~$43M vs медиана ~$20M — хвост мега-сделок. Это и есть «пузырь для 20 компаний, засуха для остальных».

Признаки коррекции

- Public AI multiples сжатие Q1 2026 (PitchBook Public Comp Sheet, 22 апр 2026).
- Altman vs neocloud overbuild (сен 2026).
- Amodei: pace the frontier + adjusted profit story перед IPO.
- Thinking Machines не взяла $50B+ в конце 2025 (Bloomberg via вторичных) — потолок даже на brand.
- Seed медиана не растёт ($3.5M 2025 → $3.0M H1 2026, PitchBook), тогда как A/B растут. Ранним без трекшена не легче.

Что это меняет в требованиях: для всех, кто не Ilya, чек seed/A требует доказательства job + retention + хотя бы маленькой выручки. Нарратив «агенты изменят мир» больше не уникален.

---

E. Где фаундеры и инвесторы противоречат друг другу

1. Нужен ли продукт. Ilya/Mira: нет, research is the product. Sequoia/YC/a16z для всех остальных: продавайте работу. Рынок разрешает исключение только 2–3 людям в десятилетие.

2. Прибыльность vs scale. Amodei демонстрирует adjusted profit и «каждая модель прибыльна». Altman защищает убыток как capex и огрызается на вопрос про $1.4T. Инвесторы late-stage в 2026 начали хотеть историю Amodei, не историю Altman — отсюда Anthropic IPO-преп и «ill-advised» IPO OpenAI в 2026.

3. Wrappers. Фаундеры application-слоя (Truell) говорят: UX+harness вокруг чужих моделей — это и есть компания. Инвесторы говорят «no thin wrappers». Разница: Cursor не thin, потому что daily habit + $1B ARR. Без этого «harness» = wrapper.

4. Европа. Mensch: оставайтесь, стройте суверенитет. Американский капитал (и YC: 71% San Francisco в одном разборе 2026) голосует ногами за SF. Mistral сам берёт Samsung + a16z + европейский госфонд одновременно. Европейскому фаундеру без Airbus/HSBC в пайплайне совет Mensch не конвертируется в чек Sequoia.

5. AGI-таймлайн. Sequoia: AGI уже здесь (functional). Amodei: надо замедлить frontier. Altman: модели будут «sobering». Инвесторский AGI = «агент работает 30–480 минут». Исследовательский AGI ≠ это. Путаница специально полезна фондам (можно держать valuation) и вредна мелким (думают, что надо строить SSI).

6. Infra overbuild. Фонды поднимают dedicated hardware funds. Altman и Amodei предупреждают, что capex может не найти выручку. Founder infра-стартапа в 2026 слышит оба сигнала в одной неделе.

7. «Сначала свои модели». Huang/Sequoia 2026: own the weights. Truell 2025: берём best models рынка. Практика победителей: чужие модели + узкий свой кусок (Composer, Tenet), не с нуля GPT.

---

F. Применение к профилю заказчика

Структурно закрыто

- Новая frontier-лаборатория. Нет compute на гигаватты, нет имени Ilya/Murati/Amodei, нет выхода из OpenAI. SSI/TML — не аналог.
- Чипы, энергия, DC. Нет площадки и offtake.
- Humanoid/world models как компания. Нет железа и данных.
- Generic RAG / «второй мозг для enterprise» против Glean/Copilot/Claude.
- Горизонтальная оркестрация против LangGraph + Claude Code + внутренних платформ.

Реалистично (в порядке достижимости чека)

Вариант 1 — Production audit / evals / reliability для агентных флотов.
Почему: актив уже есть; категория пишет чеки (Braintrust $80M@$800M); покупатель — engineering/AI team, не «innovation theater». Ankur Goyal — не frontier-researcher, а человек, который дважды страдал от внутренних evals (Impira, Figma).
Что доказать: 3 design partners, traces в prod, evals в CI, хотя бы $20–50k ACV или usage-based, публичный agent-audit бенч на чужих агентах.
Типичный чек: seed $4–12M (EU+US angels/a16z infra-adjacent); A $20–40M при $1M+ ARR.
Цена ошибки: LangSmith/Braintrust/Phoenix съедят горизонталь; останетесь «ещё один dashboard». Срок годности окна: 12–24 мес.
Уверенность: высокая как путь, средняя как размер чека.

Вариант 2 — Вертикальный outcome-агент, где оркестрация спрятана.
Не «платформа агентов». Один job: compliance ops, audit trail для regulated agent, customer-ops в одной индустрии, fleet-ops. Sierra — доказательство, что оператор без ML-PhD поднимает $10B, но Taylor — OpenAI chair и Salesforce co-CEO. Реалистичный компас — Harvey-ранний: domain + design partners, не бренд Taylor.
Что доказать: 2 логотипа, которые платят за completed work; time-to-resolution; готовность взять ответственность (insurance/SLA).
Чек: seed $3–8M; A после $1M ARR.
Цена ошибки: Claude+Salesforce сделают 80% job бесплатно (тезис a16z). Нужен кусок, где judgment + institution learning.
Уверенность: высокая по логике, низкая без выбранной вертикали.

Вариант 3 — AI-native service («продаём работу») на базе флота.
YC и Sequoia это прямо просят. Крипто-операционный опыт помогает: 24/7 ops, инциденты, round-the-clock agents. Не продавать софт португальским SME за €99, а продавать outcome US/UK mid-market.
Что доказать: positive gross margin после людей+токенов; 10→50 клиентов без линейного headcount.
Чек: YC/$2–5M seed; европейские фонды на сервисе платят хуже, чем US на software — релокация в US почти обязательна для A.
Цена ошибки: кассовый разрыв как у агентства; «не SaaS» дисконт у классических фондов.
Уверенность: средняя.

Вариант 4 — Multiplayer company-brain под named workflow (не поиск).
YC Fall 2026 Multiplayer AI + «small software» — ближайший RFS к «второму мозгу». Продукт: общая живая сессия агентов на один процесс (аудит, incident, due diligence), с памятью института.
Что доказать: команда из 5–20 человек реально работает внутри одной agent session каждый день; retention.
Цена ошибки: выглядеть как Notion AI / Glean. Нужно multiplayer+audit, не retrieval.
Уверенность: средняя.

Вариант 5 — Trust/identity/payments для агентов (крипто-рельсы).
YC Fall 2026: proving you’re human + «agents are going to use crypto networks as financial rails». Здесь крипто-бэкграунд — плюс, не минус. Но это уже другая компания, не «лаборатория знаний».
Цена ошибки: крипто-стигма у non-crypto GP; compliance ад.
Уверенность: низкая-средняя.

Что придётся доказать, чтобы чек стал возможен

1. Не лаборатория, а компания с покупателем. Хотя бы 2 платящих.
2. Job, не слой. Слова «оркестрация/RAG/память» в первом слайде = pass. Слова «мы закрываем X часов аудита агентного поведения для Y» = разговор.
3. Релокация или US co-founder. Не идеология, а капе. YC-батчи ~70% SF. Mensch-путь работает, если клиент — Airbus, не если клиент — «европейские SME».
4. Публичный артефакт: open eval suite / leaderboard агентного аудита, который цитируют инженеры. Это дешёвый substitute research-brand.
5. Unit economics токенов. В 2026 инвестор умеет спросить COGS.

Минимальный сигнал «нет» → «поговорим»

Не «пилоты». А: (a) 2 design partners с логотипом, (b) $15–30k/мес run-rate или signed ACV, (c) одна метрика outcome (precision аудита / incidents caught / hours saved), (d) GitHub/blog с методом, который можно проверить. Для US seed этого достаточно, чтобы партнёр ответил на мейл. Для Sequoia/a16z A — нет, нужен порядок $1M ARR и рост.

Прецеденты рейза без исследовательского бэкграунда

- Bret Taylor / Clay Bavor, Sierra — продукт/операторы, $350M@$10B (сен 2025), $100M ARR (ноя 2025). Не ML-lab. Самый сильный прецедент, но невоспроизводимый бренд.
- Winston Weinberg, Harvey — бывший junior litigator, не researcher (со-founder Gabe Pereyra — DeepMind/Meta, то есть research всё же на капе). $550M@~$15.5B (сен 2026). Урок: доменный фаундер + один technical co-founder из лабы.
- Michael Truell et al., Cursor — MIT CS undergrads, YC, не PhD-исследователи. Чек дали за usage.
- Ankur Goyal, Braintrust — founder-operator (Impira→Figma), не Anthropic research. $80M B.
- Arthur Mensch — наоборот, researcher (DeepMind). Не ваш прецедент.
Не нашёл чистого прецедента «европейский крипто-фаундер с PhD не-ML поднял US Series A на agent audit в 2025–26». Ближе всего Braintrust по категории и Sierra/Harvey по «оператор продаёт работу».

Где крипто помогает и где вредит

Помогает: агентные платежи, identity, round-the-clock ops, толерантность к нестандартным структурам капитала, аудитория a16z crypto / Haun, которые в 2026 сами питчат «agentic economy». YC прямо пишет, что агенты будут на crypto rails.
Вредит: многие US app/SaaS GP до сих пор фильтруют «crypto founder» как token-exit risk и retail-hype. Scale/Harvey/Sierra-капы — не крипто-капы. На питче слово «token» в первом абзаце убивает разговор с Benchmark/Sequoia app. Крипто надо спрятать в competence («я строил системы, которые не имеют права упасть»), не в identity.
Прецедент вреда: прямого «отказали, потому что крипто» в открытых письмах 2026 не нашёл. Прецедент помощи: сам Liang — quant/trading cash machine, не VC; DeepSeek. Это ближе к «свой P&L», чем к токену. Если крипто-опыт = был свой денежный поток на R&D — это DeepSeek-like плюс. Если крипто-опыт = ICO/NFT — это минус. Различайте.

Релокация

Для вариантов 1–2: US (SF или NY) на время A почти обязателен. Для варианта Mensch (суверенный audit для EU gov/banks) можно остаться в PT/EU и брать Bpifrance/Index/EQT Scaleup-adjacent — чеки меньше, но реальнее без H1B-театра. Гибрид: Delaware + US sales hire, фаундер летает. Не спрашивать инвестора «какой офис»; иметь US entity и один SF-месяц на цикл встреч.

---

G. Список источников (URL и даты)

Открыты целиком в этом прогоне:
- https://techcrunch.com/2025/11/06/sam-altman-says-openai-has-20b-arr-and-about-1-4-trillion-in-data-center-commitments/ — 6 ноя 2025
- https://www.ycombinator.com/rfs/ — Fall 2026 RFS, прочитано 2026-09-14
- https://www.a16z.news/p/the-incumbents-are-coming — Seema Amble, 3 сен 2026
- https://sequoiacap.com/article/2026-this-is-agi/ — Grady/Huang, 14 янв 2026
- https://techcrunch.com/2025/11/13/coding-assistant-cursor-raises-2-3b-5-months-after-its-previous-round/ — 13 ноя 2025
- https://techcrunch.com/2025/09/04/bret-taylors-sierra-raises-350m-at-a-10b-valuation/ — 4 сен 2025
- https://thenextweb.com/news/mistral-3bn-series-d-samsung-21bn-valuation — 8 сен 2026
- https://nvidianews.nvidia.com/news/ilya-sutskevers-safe-superintelligence-inc-and-nvidia-announce-long-term-strategic-partnership — 27 июл 2026
- https://www.braintrust.dev/blog/announcing-series-b — Ankur Goyal, 17 фев 2026
- https://officechai.com/ai/ilya-sutskever-on-what-makes-ssi-different-from-other-labs-and-how-it-plans-to-make-money/ — 26 ноя 2025 (конспект Dwarkesh)

Ключевые источники по поиску/excerpt (не все открыты целиком; цифры помечены уверенностью выше):
- Axios 28 окт 2025 — Altman $1.4T / 30 GW
- Yahoo Finance ~3 ноя 2025 — Altman «Enough» на Bg2 Pod
- FT 13–14 сен 2026 (пересказы Dealroom, Morningstar, Livemint) — Anthropic adjusted profit, $11.5B Q2, $65B ARR
- Dealroom / Cheeky Pint май 2026 — Amodei «model is actually profitable»
- WSJ 15 ноя 2025 (archive.ph) — Anthropic vs OpenAI burn
- Assemblée nationale стенограмма 12 мая 2026 — Mensch
- SCMP 18 июн 2026 — DeepSeek ~50B RMB / ~$59.2B
- Forbes 30 янв 2025 — Liang, High-Flyer, цитата 36Kr 2023
- Reuters 13 июн 2025 — Meta $14.3B / Scale ~$29B
- TechCrunch 15 июл 2025 и 3 сен 2026 — Thinking Machines $2B@$12B; Accel talks $40B
- TechCrunch 21 окт 2025 — LangChain $125M@$1.25B
- TechCrunch 21 ноя 2025 — Sierra $100M ARR
- NYT 4 дек 2025 — Harvey $160M@$8B
- TNW / Silicon Republic 9 сен 2026 — Harvey $550M@~$15.5–15.6B
- Figure.ai 16 сен 2025 и 3 сен 2026 — Series C $39B; Nscale $3.5B compute
- PitchBook AI Outlook PDF дек 2025 — overheated subsectors
- PitchBook Q1 2026 AI Public Comp Sheet — 22 апр 2026
- PitchBook-NVCA / Carta Q1–H1 2026 — медианы seed/A (через Causo Hub, BVJ, Peony)
- CB Insights / VC Cafe — AI share of VC 2025–26
- Radical Ventures «Rise of NeoLabs» 2 сен 2026
- a16z.news «Investing in Cognition» Marc Andreessen 9 сен 2026
- BusinessWire 13 ноя 2025 — Cursor Series D, цитата Truell
- Axios 17 фев 2026 — Braintrust $80M@$800M

---

H. Что не нашёл и где данные слабые

- Не открыл первоисточник FT по прибыли Anthropic — только пересказы 14.09.2026. Adjusted operating income без SBC и без training cost — это не GAAP profit. Нельзя говорить «Anthropic прибылен» без проспекта.
- Не нашёл первичного company announcement на $950M Sierra @ $15.8B (май 2026) — есть CNBC/Sacra/Quartz.
- Не нашёл первичного confirmation $5B Nvidia→SSI; NVIDIA PR говорит «substantial», сумму дают FT/Bloomberg «people briefed».
- Thinking Machines round @ $40B — in talks, не closed.
- Cursor/SpaceX или xAI $60B: в источниках конфликт (Wikipedia — xAI option; Seedtable/BI — SpaceX). Не использую как твёрдый факт.
- Perplexity и Glean ARR 2026 — оценки Sacra/Teahose, не audited.
- Не нашёл полного эссе партнёров Benchmark, Founders Fund, Khosla, Conviction, Amplify, Basis Set за 2025–26, которое можно цитировать дословно. Их позиция выведена из сделок, это слабее, чем Sequoia/a16z/YC.
- Не нашёл медианы «agent infra seed» отдельно от общего AI seed. Категориальные чеки в таблице — синтез прецедентов, не PitchBook cut.
- Не нашёл прямого публичного совета Dario/Altman «маленькой европейской агентной лаборатории, как поднимать seed в 2026».
- Не нашёл прецедента «крипто-фаундер из Португалии → US Series A на agent audit». Есть соседние (Braintrust, Sierra, Harvey), не клон.
- Китайские цифры DeepSeek гуляют: 50B vs 51B RMB, $52–59B. Беру как диапазон.
- «80% VC в AI в Q1 2026» vs «48% за 2025» vs «60% Carta» — разные базы. Не складывать.
- Не читал полный PitchBook Consumer AI PDF и полный AI Outlook — overheated list из excerpt.
- Dwarkesh×Ilya — цитаты через OfficeChai, не через официальный транскрипт dwarkesh.com.
- Не верифицировал Robocurve $10M дальше собственного блога (14 сен 2026).

Метод: live web search + полная вычитка 10 первоисточников. Где стоит «не нашёл» — это не «не существует», это «в этом прогоне первички нет». Цифры мега-раундов 2026 движутся еженедельно; всё, что сентябрь 2026, перепроверять перед питчем.
