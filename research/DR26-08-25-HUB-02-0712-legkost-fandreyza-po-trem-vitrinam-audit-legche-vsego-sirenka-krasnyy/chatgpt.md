# DR26-08-25-HUB-02-0712 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

срез данных: 25.08.2026. события после этой даты неизвестны; «конец 2026» ниже — прогноз, а не наблюдаемый факт. под «потолком чека» я понимаю общий размер раунда: размеры вкладов отдельных инвесторов почти нигде не раскрыты.

## executive summary — 7 строк

1. легче всего рейзить на витрине 1, аудите и runtime-безопасности агентов: реалистичный потолок сейчас — $3–5 млн; ближайший публичный прецедент — объявленный 27.05.2026 seed codeintegrity на $5 млн под production-controls для агентов ([codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round)).
2. витрина 2 фандабельна только как узкий enterprise wedge, например telegram-native knowledge/ops layer; горизонтальная формула «подключим все источники» уже занята glean, dust, notion, sana/workday и гиперскейлерами.
3. потолок витрины 2 сейчас — $2–3 млн; $3.5–5 млн становится защищаемым после 2–3 именуемых платящих компаний, повторного использования и измеримого roi.
4. витрина 3 фандабельна как production runtime/control plane, но не как рассказ о «консенсусе и self-healing нашего собственного флота»; потолок сейчас — $1.5–2.5 млн.
5. капитал в 2026 сконцентрирован: немногие pre-seed на carta превышают $2.5 млн, хотя ai получил около половины всех pre-seed-долларов ([carta, 13.08.2026](https://carta.com/data/state-of-pre-seed-q2-2026/)).
6. самый короткий путь к институциональному чеку дают 30 cve и внешние oss-взаимодействия: это независимая рыночная валидация; 1.47 млн сообщений и внутренний fleet пока подтверждают engineering, но не спрос.
7. сильнейшая комбо-упаковка — «детерминированный security/runtime control plane для автономных agent fleets»; попытка одновременно продавать аудит, второй мозг и оркестратор будет прочитана как отсутствие одного покупателя и одного рынка.

## витрина 2 — enterprise knowledge layer / second brain

### 1. фандабельность

вердикт: с оговорками.

как горизонтальный «second brain поверх любых источников» — скорее нет для команды без известного имени и внешнего adoption. этот тезис уже реализован компаниями с десятками коннекторов, permission-aware search, knowledge graphs, агентами и крупными enterprise-клиентами.

как узкий wedge — да. наиболее защищаемая формулировка вашего актива: не «новый glean», а knowledge/operations layer для неструктурированных messaging-first организаций, где telegram одновременно является crm, inbox, журналом решений и каналом исполнения.

заявленные 126 975 чатов, 1.47 млн сообщений и обработка около 51 тыс. диалогов за ночь — данные из задания, внешне не подтвержденные. они доказывают способность обрабатывать реальный корпус и контролировать себестоимость, но сами по себе не доказывают наличие второго клиента, retention или готовность платить.

### 2. красный океан

| игрок | актуальный сигнал капитала/оценки | что уже занимает |
|---|---|---|
| glean | $150 млн series f при оценке $7.2 млрд, 10.06.2025; к 28.05.2026 компания сообщила о $300 млн arr ([glean](https://www.glean.com/press/glean-raises-150m-series-f-at-7-2b-valuation-to-accelerate-enterprise-ai-agent-innovation-globally), [techcrunch](https://techcrunch.com/2026/05/28/gleans-top-line-crosses-300m-as-ai-budget-cutting-becomes-its-major-selling-point/)) | enterprise search, context graph, permissions, 100+ коннекторов, агенты |
| sana | $55 млн при оценке $500 млн, 30.10.2024; договор на продажу workday примерно за $1.1 млрд подписан 16.09.2025 ([sana](https://www.newswire.com/news/sana-raises-55m-at-a-500m-valuation-acquires-workflow-automation-22452594), [workday](https://investor.workday.com/news-and-events/press-releases/news-details/2025/Workday-Signs-Definitive-Agreement-to-Acquire-Sana-09-16-2025/default.aspx)) | поиск, обучение, knowledge assistant и агенты внутри крупной enterprise suite |
| dust | $16 млн series a, 27.06.2024, при $1 млн arr; $40 млн series b, 18.05.2026, при заявленных 3 000 организациях, 51 тыс. mau и 300 тыс. развернутых агентов ([techcrunch](https://techcrunch.com/2024/06/27/dust-grabs-another-16-million-for-its-enterprise-ai-assistants-connected-to-internal-data/), [dust](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/dust-raises-40m-to-make-ai-multiplayer-inside-the-enterprise.html)) | shared enterprise assistants, корпоративные данные, командная работа агентов |
| notion | около $11 млрд по вторичному tender на $270 млн, а не по свежему primary-раунду; цифра не подтверждена официальным релизом notion ([sacra](https://sacra.com/c/notion/)) | установленная workspace-аудитория, встроенные ai search, meeting notes и агенты |
| perplexity | по сообщениям, $200 млн при оценке $20 млрд в сентябре 2025; обсуждение оценки $30 млрд на 25.08.2026 — только переговоры, не закрытый раунд ([axios, 11.09.2025](https://www.axios.com/newsletters/axios-pro-rata-f3623b21-0445-4307-bc34-d57c5e54d90b), [axios, 25.08.2026](https://www.axios.com/newsletters/axios-pro-rata-1e428704-fc05-4ffd-88f5-82fb0d643fad)) | web research плюс поиск по загруженным и корпоративным данным |
| sierra | $950 млн при post-money $15.8 млрд, 04.05.2026; это соседняя категория customer-service agents, а не чистый knowledge layer ([axios](https://www.axios.com/pro/enterprise-software-deals/2026/05/04/sierra-ai-valuation-agentic-enterprise)) | агенты, контекст клиента, исполнение enterprise-workflows |
| onyx | $10 млн seed, 12.03.2025, при десятках enterprise-клиентов и развертывании у netflix на 14 тыс. сотрудников ([onyx](https://onyx.app/blog/seed-round)) | open-source enterprise search, self-hosting, 40+ источников, research agents |

плотность: экстремальная. generic ingestion, rag, connectors, enterprise search и knowledge graph больше не являются самостоятельной категорией — это минимальный набор продукта. прямая атака «мы дешевле glean, потому что обрабатываем данные без llm» потребует доказать сопоставимое качество, permissions, freshness и enterprise-security, а не только меньшую себестоимость.

### 3. кейсы 2024–2026

строго найти 6–8 раундов «при слабом adoption» нельзя: большая часть публичных победителей уже имела oss-дистрибуцию, десятки тысяч пользователей, beta-клиентов или контракты. таблица показывает реальные данные на дату раунда и отделяет настоящие слабые сигналы от ретроспективного мифа «подняли на одном pitch deck».

| команда | сумма / стадия / дата | оценка и lead | что было в момент раунда | чем заменили выручку | воспроизводимость без имени |
|---|---|---|---|---|---|
| supermemory | $3 млн pre-seed, 06.10.2025 | оценка не раскрыта; susa ventures | 50 тыс.+ пользователей, миллионы сохраненных объектов, 10 тыс. github stars, ранние enterprise-запросы ([supermemory](https://supermemory.ai/blog/supermemory-raises-3-million-and-building-the-best-memory-engine-for-llms/)) | consumer/oss adoption и техническая инфраструктура памяти | частично: это ближайший нижний comp, но у вас пока нет 50 тыс. пользователей или 10 тыс. stars |
| tana | $14 млн series a, 03.02.2025; headline «$25 млн» включал прежний $11 млн seed | $100 млн post-money; tola capital | 30 тыс. beta-тестеров, 160 тыс. waitlist, 24 тыс. участников slack; два сооснователя из google ([techcrunch](https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/)) | закрытая beta, waitlist, community и founder history | нет для размера $14 млн; частично для продукта |
| onyx | $10 млн seed, 12.03.2025 | не раскрыта; khosla ventures и first round | десятки enterprise-клиентов, netflix 14 тыс. сотрудников, uc san diego 37 тыс. пользователей, сообщество 3 тыс. разработчиков и 125 контрибьюторов ([onyx](https://onyx.app/blog/seed-round)) | именуемые внедрения и oss community | частично; воспроизводимы named deployments и contributors, но это уже не слабый adoption |
| dust | $16 млн series a, 27.06.2024 | не раскрыта; sequoia | $1 млн arr, более 5 тыс. активных подписчиков, интенсивное использование в qonto, alan и pennylane ([techcrunch](https://techcrunch.com/2024/06/27/dust-grabs-another-16-million-for-its-enterprise-ai-assistants-connected-to-internal-data/)) | здесь выручку уже не заменяли — она была | нет как comp для внутреннего-only продукта |
| letta | $10 млн seed, 24.09.2024 | не раскрыта; felicis | memgpt из berkeley, бесплатный developer environment; hosted cloud только входил в beta ([letta](https://www.prnewswire.com/news-releases/berkeley-ai-research-lab-spinout-letta-raises-10m-seed-financing-led-by-felicis-to-build-ai-with-memory-302257004.html)) | академическая новизна, известная лаборатория, oss-проект и advisors | нет для размера; у команды был явный berkeley/ion stoica anchor |
| contextual ai | $80 млн series a, 01.08.2024 | не раскрыта; greycroft | production deployments в fortune 500 и многолетний контракт с qualcomm; сооснователь ранее участвовал в создании rag в meta ([contextual ai](https://contextual.ai/blog/announcing-series-a)) | research moat, founder pedigree, enterprise-контракт | нет |
| mem0 | $20 млн series a, объявлена 28.10.2025; $24 млн в headline — округленная сумма series a и ранее не объявленного $3.9 млн seed | не раскрыта; basis set ventures | 41 тыс. github stars, 13 млн+ package downloads, 80 тыс. cloud-signups и 186 млн api calls за q3 2025 ([techcrunch](https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps/)) | масштаб oss и api usage | частично, но не без comparable developer distribution |

главный вывод из таблицы: единственный почти прямой публичный pre-seed comp около вашего желаемого диапазона — supermemory на $3 млн, однако у него уже были 50 тыс. пользователей и 10 тыс. github stars. внутренний корпус из 1.47 млн сообщений — менее сильный fundraising-суррогат.

### 4. входной билет

что реально принимали вместо выручки:

- named production deployments с числом охваченных сотрудников: onyx сообщил о 14 тыс. сотрудников netflix и 37 тыс. пользователей uc san diego ([onyx](https://onyx.app/blog/seed-round)).
- регулярность использования внутри клиента: dust раскрывал 75% monthly usage у qonto и 80% weekly usage у alan ([techcrunch](https://techcrunch.com/2024/06/27/dust-grabs-another-16-million-for-its-enterprise-ai-assistants-connected-to-internal-data/)).
- oss/developer distribution: mem0 имел 41 тыс. stars и 13 млн downloads ([techcrunch](https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps/)).
- закрытая beta с реальными тестерами, а не только waitlist: tana сообщала о 30 тыс. beta-пользователей ([techcrunch](https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/)).
- контракт, accuracy или workflow-result на именуемом клиенте: contextual ai раскрыл многолетний контракт с qualcomm ([contextual ai](https://contextual.ai/blog/announcing-series-a)).

что уже почти не работает:

- число проиндексированных сообщений без числа независимых организаций и активных пользователей;
- «работает за ноль llm-токенов» без сравнения точности, freshness, total cost и человеческого труда;
- личный digital twin как единственный deployment;
- waitlist без cohort usage;
- «можем подключить любые источники» без permission model и data lineage.

прямой аналог вашему активу есть только на уровне workload proof. у supermemory до раунда были миллионы сохраненных объектов, но также 50 тыс. пользователей; у glean к декабрю 2025 было 27 млрд индексированных документов, 100+ коннекторов и $200 млн заявленного arr ([glean](https://www.glean.com/blog/glean-200m-arr-milestone)). объем данных становится fundraising-суррогатом только вместе с независимыми пользователями и измеримым результатом.

### 5. честный потолок

сейчас:

- общий pre-seed: $2–3 млн;
- выше $3 млн: с текущими данными не защищается;
- размер одного lead-чека: не подтвержден, поскольку allocations публичных раундов не раскрыты.

реалистичные добавки для $3.5–5 млн:

- 2–3 именуемых компании, не принадлежащие оператору;
- платный pilot или контракт, даже небольшой;
- минимум 8–12 недель retention;
- десятки weekly active seats в каждом deployment;
- permission-aware ответы, citations и измеренная retrieval accuracy;
- подтвержденные savings: токены, часы оператора, скорость ответа lead/customer;
- один узкий workflow, где telegram-корпус дает преимущество, а не обещание «всех источников».

$6 млн+ для команды без pedigree потребует уже не «реалистичных добавок», а исключительного distribution-сигнала уровня onyx/mem0 или крупного стратегического инвестора.

## витрина 3 — оркестрация агентного флота

### 1. фандабельность

вердикт: с оговорками.

категория fundable, но ее название изменилось. деньги идут не в абстрактный «multi-agent consensus», а в:

- production runtime;
- observability и evals;
- permissions, budgets и audit trails;
- tool/auth infrastructure;
- deployment, retries, rollback и reliability;
- управляемую автономию в enterprise-среде.

«несколько наших машин и vps сами договорились и восстановились» — инженерный demo. он не отвечает на вопросы: кто buyer, сколько внешних jobs прошло, какова success rate, какой mttr, какой blast radius и сколько стоит failure.

### 2. кто уже занимает слой

- langchain/langgraph/langsmith: $125 млн при оценке $1.25 млрд, 20.10.2025; компания заявила 90 млн ежемесячных downloads и использование сервисов 35% fortune 500 ([langchain](https://www.langchain.com/blog/series-b)).
- crewai: 22.10.2024 объявлены $18 млн совокупного финансирования, включавшего inception и series a; также заявлялись 10 млн agent executions в месяц и 150 beta enterprise-клиентов ([crewai](https://www.globenewswire.com/news-release/2024/10/22/2966872/0/en/CrewAI-Launches-Multi-Agentic-Platform-to-Deliver-on-the-Promise-of-Generative-AI-for-Enterprise.html)).
- mastra: $22 млн series a, 09.04.2026, led by spark, итого $35 млн; платформа охватывает framework, observability, deploy и memory ([mastra](https://mastra.ai/blog/series-a)).
- sema4.ai: $30.5 млн были объявлены вместе с приобретением robocorp 29.01.2024; еще $25 млн extension объявлены 04.06.2025, доведя заявленный series a total до $55.5 млн ([sema4](https://www.prnewswire.com/news-releases/sema4-ai-raises-30-5-million-to-bring-open-source-powered-ai-to-mission-critical-enterprise-work-302047158.html), [sema4 extension](https://sema4.ai/newsroom/enterprise-ai-agent-platform-runs-natively-snowflake/)).
- sapiom: $15.75 млн seed 06.02.2026, затем $35 млн series a 05.08.2026; к series a компания заявляла 270 млн transactions и более 100 тыс. agent runs в день ([sapiom seed](https://www.sapiom.ai/resources/launch/), [sapiom series a](https://www.sapiom.ai/resources/blog/series-a/)).
- sycamore: $65 млн seed, 30.03.2026, led by coatue и lightspeed; продукт еще работал с fortune 500 deployments, но основатель был бывшим cto atlassian ([sycamore](https://sycamorelab.org/press-releases/sycamore-raises-65m-seed/)).
- cognition — не горизонтальный orchestrator, а agent application для software engineering: 27.05.2026 компания объявила более $1 млрд при $26 млрд post-money и заявила $492 млн annualized revenue run-rate ([techcrunch](https://techcrunch.com/2026/05/27/ai-coding-startup-cognition-raises-1b-at-25b-pre-money-valuation/)). это demand signal, но плохой pre-seed comp.
- autogen не превратился в подтвержденный независимый венчурный победитель: microsoft объединила его с semantic kernel в microsoft agent framework в октябре 2025 ([microsoft](https://devblogs.microsoft.com/foundry/introducing-microsoft-agent-framework-the-open-source-engine-for-agentic-ai-apps/)).
- openai, google и microsoft предлагают собственные open-source agent frameworks; openai agents sdk включает orchestration, handoffs, tracing и guardrails, а google adk — multi-agent framework ([openai](https://openai.com/index/new-tools-for-building-agents/), [google](https://developers.googleblog.com/agent-development-kit-easy-to-build-multi-agent-applications/)).
- mcp с 09.12.2025 находится в agentic ai foundation, а 17.08.2026 туда же перемещен google a2a; следовательно, protocol layer становится нейтральной инфраструктурой, а не moat отдельного стартапа ([anthropic](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation), [axios](https://www.axios.com/2026/08/17/a2a-agentic-ai-foundation-open-ai-standards)).

плотность: высокая в frameworks и протоколах; средняя в настоящем multi-machine production control plane. свободное место еще есть в reliability, policy, cost и security, но уже не в «мы умеем запускать несколько агентов».

### 3. кейсы 2024–2026

| команда | сумма / стадия / дата | оценка и lead | что было в момент раунда | чем заменили выручку | воспроизводимость без имени |
|---|---|---|---|---|---|
| langchain | $25 млн series a, объявлена 15.02.2024 | около $200 млн reported, не official; sequoia | 80 тыс. signups langsmith, 5 тыс. monthly active teams, 40 млн traces за январь, named users rakuten, elastic, moody’s ([langchain](https://www.langchain.com/blog/langsmith-ga), [forbes](https://www.forbes.com/sites/alexkonrad/2024/02/15/open-source-ai-startup-langchain-launches-langsmith/)) | огромная oss-дистрибуция и usage telemetry | частично; основатель не был знаменитостью, но продукт уже был стандартом |
| crewai | $18 млн cumulative, объявлено 22.10.2024; размер именно series a не раскрыт | не раскрыта; series a led by insight | 10 млн agent executions в месяц, 150 beta enterprise-клиентов, заявленное использование почти половиной fortune 500 ([crewai](https://www.globenewswire.com/news-release/2024/10/22/2966872/0/en/CrewAI-Launches-Multi-Agentic-Platform-to-Deliver-on-the-Promise-of-Generative-AI-for-Enterprise.html)) | oss execution volume и enterprise beta | частично; но это уже не weak adoption |
| letta | $10 млн seed, 24.09.2024 | не раскрыта; felicis | memgpt, berkeley research и hosted beta без раскрытой выручки ([letta](https://www.prnewswire.com/news-releases/berkeley-ai-research-lab-spinout-letta-raises-10m-seed-financing-led-by-felicis-to-build-ai-with-memory-302257004.html)) | research/ip и лабораторный anchor | нет для размера |
| mastra | $13 млн seed, закрыт в марте и объявлен 08.10.2025 | не раскрыта; единого lead не указано, 100+ инвесторов | около 190 тыс. weekly downloads, deployments у softbank, adobe, paypal, replit и 75 тыс. сотрудников marsh mclennan на agentic search ([mastra](https://mastra.ai/blog/seed-round)) | oss velocity, founders предыдущего framework gatsby, named adoption | частично; founder history и distribution были существенно сильнее |
| composio | $25 млн series a, 22.07.2025 | не раскрыта; lightspeed | 100 тыс.+ разработчиков, более 200 компаний, millions of requests daily и заявленная семизначная выручка ([composio](https://www.prnewswire.com/news-releases/composio-raises-29m-to-solve-ais-learning-problem-building-skills-that-actually-improve-over-time-302510684.html)) | developer distribution и уже существующая выручка | нет как weak-adoption comp |
| sapiom | $15.75 млн seed, 06.02.2026 | не раскрыта; accel | в seed-анонсе adoption и выручка не раскрывались; основатель ранее руководил payments engineering в shopify и имел exit ([sapiom](https://www.sapiom.ai/resources/launch/)) | operator pedigree, thesis, участие anthropic и других стратегов | нет для размера; продуктовый путь воспроизводим |
| trace | $3 млн, 26.02.2026 | оценка и единый lead не раскрыты; yc, xeno, goodwater и другие | двухчленная founding team, 550+ активных workflows, 10–14% customer tasks назначались агентам ([trace](https://www.trace.so/blog/trace-raised-dollar3m-to-build-the-context-layer-for-ai-at-work)) | реальный workflow count, customer roi и yc distribution | да/частично; ближайший нижний comp |
| xpander | $7.5 млн seed, 17.08.2026 | не раскрыта; pico venture partners | general availability в день раунда; заявлены работы с fortune 500, smes и госорганизациями, но customer count, arr и retention не раскрыты ([xpander](https://xpander.ai/blog/xpander-funding-seed)) | product maturity и неименованные deployments | частично; без имен и метрик это слабее публичного headline |

реально пригодный для вашей команды нижний comp — trace: $3 млн при двух основателях, но уже с 550 внешними workflows. xpander показывает возможный $7.5 млн seed, однако у него были внешние enterprise/government engagements. внутренний fleet без внешних jobs слабее обоих.

### 4. входной билет

принимается:

- десятки тысяч или миллионы external agent executions, а не задачи собственной команды;
- 3–5 внешних deployments, желательно именуемых;
- success rate по задачам и доля полностью автономных завершений;
- uptime, retry/rollback rate, mttr и failure containment;
- измеримая экономия: sapiom к series a заявлял 75% снижения inference-cost у одного клиента ([sapiom](https://www.sapiom.ai/resources/blog/series-a/));
- oss downloads плюс conversion в hosted/enterprise;
- audit logs, permissions и security boundaries;
- доказательство model/framework neutrality.

уже не работает:

- красивый multi-agent demo;
- количество собственных машин;
- «работает без human approval» без ограничений полномочий;
- consensus как feature без показанного снижения ошибок;
- self-healing без failure-injection tests;
- общий тезис «агенты скоро будут везде».

ваша шина — прямой аналог принимаемого суррогата только наполовину. она подтверждает, что команда умеет строить distributed runtime. она не подтверждает, что чужой buyer передаст ей credentials, production workloads и право на автономное восстановление.

### 5. честный потолок

сейчас:

- общий pre-seed: $1.5–2.5 млн;
- $3 млн возможны только при хорошей демонстрации и инвесторе, заранее убежденном в agent infrastructure;
- $5 млн+ сейчас не подтверждается;
- размер одного lead-чека: не подтвержден.

для $3–5 млн нужны:

- минимум 3 внешних флота или 10+ команд;
- 10–100 тыс. внешних jobs в месяц;
- 8–12 недель production history;
- success rate, rollback rate и mttr;
- один именуемый кейс автономного восстановления;
- policy-bound autonomy: какие действия система может выполнить без человека и почему;
- failure-injection suite и публичный benchmark;
- желательно oss layer с внешними contributors.

$6–8 млн потребуют category-scale oss distribution, признанного infra/cyber-якоря или нескольких крупных design partners. это не текущий реалистичный сценарий.

## синтез

### 1. ранжирование

| место | витрина | фандабельность | потолок чека/раунда сейчас | плотность конкуренции | входной билет, которого не хватает | срок годности окна |
|---:|---|---|---:|---|---|---|
| 1 | аудит ai-агентов / mcp security | высокая | $3–5 млн; по заданной рамке stretch $6–8 млн | высокая и растущая, но категория еще формируется; codeintegrity объявил $5 млн seed 27.05.2026 ([источник](https://www.codeintegrity.ai/blog/codeintegrity-seed-round)) | 2–3 named design partners, independent reproduction, security anchor, production metric | примерно 9–15 месяцев; аналитическая оценка |
| 2 | enterprise knowledge / second brain | средняя | $2–3 млн | экстремальная: glean уже заявил $300 млн arr, dust — 3 тыс. организаций ([glean](https://techcrunch.com/2026/05/28/gleans-top-line-crosses-300m-as-ai-budget-cutting-becomes-its-major-selling-point/), [dust](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/dust-raises-40m-to-make-ai-multiplayer-inside-the-enterprise.html)) | внешние платящие deployments, active seats, retention, permissions и roi | generic horizontal window уже почти закрыто; vertical telegram wedge — около 12–18 месяцев |
| 3 | fleet orchestration | низкая сейчас, средняя после внешних deployments | $1.5–2.5 млн | высокая в frameworks, средняя в cross-machine production control | external jobs, reliability/rollback metrics, 3–5 deployments, oss distribution | generic framework — 6–9 месяцев; control/security runtime — около 12–18 месяцев |

сроки — аналитический прогноз, не наблюдаемый market fact.

### 2. где легче всего

легче всего — витрина 1. из перечисленных активов только cve и взаимодействия с внешними oss-maintainers уже прошли границу вашей собственной системы: третьи стороны могут проверить проблему и реакцию экосистемы. backend на 1.47 млн сообщений доказывает scale engineering, но не product adoption. работающий fleet доказывает orchestration engineering, но пока является единственным внутренним deployment. поэтому security имеет самый короткий путь к первому институциональному чеку и уже подтвержден соседним $5 млн seed codeintegrity ([codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round)).

### 3. комбо-гипотеза

рабочее комбо:

> deterministic runtime security and resilience layer for autonomous agent fleets across machines, tools and mcp/a2a endpoints.

здесь витрина 1 остается продуктом и budget owner: security/platform team. витрина 3 становится доказательством, испытательным стендом и частью исполнения политики. это согласуется с тем, куда шли деньги у codeintegrity, xpander, sapiom и sycamore: governance, permissions, runtime evidence и controlled autonomy ([codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round), [xpander](https://xpander.ai/blog/xpander-funding-seed), [sapiom](https://www.sapiom.ai/resources/launch/), [sycamore](https://sycamorelab.org/press-releases/sycamore-raises-65m-seed/)).

нерабочее комбо:

> security platform + second brain + multi-machine fleet operating system.

здесь три buyer’а, три бюджета и три набора конкурентов. investor должен поверить одновременно в cyber, knowledge management и developer infrastructure. это распыление.

отдельная проблема — формулировка «без human approvals». enterprise-рынок покупает не отсутствие контроля, а заранее разрешенную автономию с логами, пределами полномочий и rollback. trace прямо строит human approval gates, xpander продает governed/auditable environment, а sycamore — security и human oversight ([trace](https://www.trace.so/blog/trace-raised-dollar3m-to-build-the-context-layer-for-ai-at-work), [xpander](https://xpander.ai/blog/xpander-funding-seed), [sycamore](https://sycamorelab.org/press-releases/sycamore-raises-65m-seed/)). поэтому правильная формула — «без ручного approval внутри заранее заданной policy», а не «полностью без approvals».

### 4. окно

быстрее всего закрывается generic enterprise knowledge. glean уже сообщил о $300 млн arr, dust — о 3 тыс. организаций, а sana продана workday за примерно $1.1 млрд ([glean](https://techcrunch.com/2026/05/28/gleans-top-line-crosses-300m-as-ai-budget-cutting-becomes-its-major-selling-point/), [dust](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/dust-raises-40m-to-make-ai-multiplayer-inside-the-enterprise.html), [workday](https://investor.workday.com/news-and-events/press-releases/news-details/2025/Workday-Signs-Definitive-Agreement-to-Acquire-Sana-09-16-2025/default.aspx)). сосед с публичным arr уничтожает право нового игрока выдавать ingestion volume за traction.

generic agent orchestration также быстро коммодитизируется из-за openai agents sdk, google adk, microsoft agent framework, mcp и a2a ([openai](https://openai.com/index/new-tools-for-building-agents/), [google](https://developers.googleblog.com/agent-development-kit-easy-to-build-multi-agent-applications/), [microsoft](https://devblogs.microsoft.com/foundry/introducing-microsoft-agent-framework-the-open-source-engine-for-agentic-ai-apps/), [axios](https://www.axios.com/2026/08/17/a2a-agentic-ai-foundation-open-ai-standards)). окно еще живо на уровне production cost, security, reliability и governance.

security-аудит пока имеет наиболее живое окно: покупатель понимает downside, а стандартные controls еще не устоялись. окно не пустое, но доказанный vulnerability corpus все еще может создать differentiation.

### 5. красный океан против новизны

обычному enterprise-saas инвестору легче понять knowledge layer, но сложнее принять, почему еще одна команда победит glean, notion, dust и suite-вендоров. infra/cyber-инвестору легче продать production control plane: sapiom, trace, xpander и sycamore уже сделали категорию читаемой, поэтому проблема теперь не в отсутствии соседей, а в отсутствии вашей внешней adoption-метрики.

для вашей команды knowledge пока занимает второе место благодаря реальному backend. fleet может его обогнать после нескольких внешних deployments, потому что контроль и reliability еще не монополизированы. без внешних deployments новизна fleet читается не как moat, а как неизвестный спрос.

## чему нельзя верить

- self-reported arr и usage нельзя считать audited: $300 млн glean, показатели dust, sapiom и cognition опубликованы самими компаниями или переданы журналистам ([glean](https://techcrunch.com/2026/05/28/gleans-top-line-crosses-300m-as-ai-budget-cutting-becomes-its-major-selling-point/), [dust](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/dust-raises-40m-to-make-ai-multiplayer-inside-the-enterprise.html), [sapiom](https://www.sapiom.ai/resources/blog/series-a/), [cognition](https://techcrunch.com/2026/05/27/ai-coding-startup-cognition-raises-1b-at-25b-pre-money-valuation/)).
- cumulative funding — не свежий чек: crewai $18 млн включали inception и series a; точный размер series a не раскрыт ([crewai](https://www.globenewswire.com/news-release/2024/10/22/2966872/0/en/CrewAI-Launches-Multi-Agentic-Platform-to-Deliver-on-the-Promise-of-Generative-AI-for-Enterprise.html)).
- headline tana «$25 млн» означал $14 млн нового series a плюс прежний $11 млн seed ([techcrunch](https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/)).
- headline mem0 «$24 млн» означал $20 млн series a плюс ранее не объявленный $3.9 млн seed ([techcrunch](https://techcrunch.com/2025/10/28/mem0-raises-24m-from-yc-peak-xv-and-basis-set-to-build-the-memory-layer-for-ai-apps/)).
- composio «$29 млн raised» означало $25 млн series a плюс прежнее финансирование ([composio](https://www.prnewswire.com/news-releases/composio-raises-29m-to-solve-ais-learning-problem-building-skills-that-actually-improve-over-time-302510684.html)).
- commitment или talks — не funded: переговоры perplexity с nvidia при возможной оценке $30 млрд на 25.08.2026 не являются закрытым раундом ([axios](https://www.axios.com/newsletters/axios-pro-rata-1e428704-fc05-4ffd-88f5-82fb0d643fad)).
- аудитория клиента — не аудитория продукта: 126 975 telegram-чатов и 1.47 млн сообщений не означают 126 975 пользователей.
- waitlist — не usage: у tana были не только 160 тыс. waitlist, но и 30 тыс. beta-тестеров; сравнивать только headline некорректно ([techcrunch](https://techcrunch.com/2025/02/03/tana-snaps-up-25m-with-its-ai-powered-knowledge-graph-for-work-racking-up-a-160k-waitlist/)).
- secondary tender — не primary funding: notion около $11 млрд основаны на tender, причем официальный первичный релиз не найден ([sacra](https://sacra.com/c/notion/)).
- оценка, названная «источниками», слабее подтвержденной компанией или filing: поэтому все неофициальные valuations выше отдельно помечены.

## непроверенное и оставшееся за вебом

- реальные размеры отдельных lead-чеков во всех приведенных раундах — не раскрыты;
- оценки supermemory, onyx, dust, letta, contextual ai, mem0, crewai, mastra, composio, sapiom, trace и xpander — не раскрыты;
- точное распределение $18 млн crewai между inception и series a — не подтверждено;
- экономическая структура первоначальных $30.5 млн sema4.ai и доля капитала, связанная с приобретением robocorp, — не подтверждена;
- arr, churn и gross margin большинства ранних компаний — не подтверждены audited statements;
- ваши 1.47 млн сообщений, 126 975 чатов, 51 тыс. nightly dialogues, 30 cve и fleet metrics в этом отчете не проверялись;
- права на использование telegram-данных, permission fidelity, retrieval accuracy и число независимых активных пользователей витрины 2 — не подтверждены;
- uptime, autonomous completion rate, rollback rate, mttr и число внешних workloads витрины 3 — не подтверждены;
- фактические сделки между 26.08.2026 и концом 2026 остаются будущими и не могут быть веб-проверены на дату среза.
