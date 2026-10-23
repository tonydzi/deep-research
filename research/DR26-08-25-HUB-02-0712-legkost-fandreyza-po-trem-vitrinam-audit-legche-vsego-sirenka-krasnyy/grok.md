# DR26-08-25-HUB-02-0712 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала сверюсь с живым вебом по раундам 2024–2026 — прошлый прогон без источников врал хором. Потом сразу полный отчёт в этом ответе, без файлов.Крупные игроки закрыты. Дальше — seed/pre-seed с слабым adoption и живые суррогаты traction.A. EXECUTIVE SUMMARY

легче всего рейзить на витрине 1 (аудит ai-агентов / mcp), потолок сейчас $3-5m, потому что в мае 2026 безымянная пятёрка codeintegrity закрыла $4.8m seed на runtime-guardrails после публичного взлома notion, а наш актив (30 cve + 21 000 открытых инстансов + ответы мейнтейнеров) ближе к этому суррогату, чем 1.47m своих сообщений — к glean, и чем «шина на своих машинах» — к langchain.1(https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)

витрина 2 (enterprise knowledge / second brain) фандабельна как категория, но не для команды без pedigree и без чужого adoption: рынок закрыт glean ($7.2b / ~$300m arr к маю 2026), perplexity (закрытая оценка $20b сен 2025, разговор nvidia про $30b+ авг 2026), workday купил sana за $1.1b 16.09.2025. честный потолок нам сейчас $1-2.5m как узкий клин «telegram-native knowledge/crm», не $3-5m «glean для всех источников».2(https://www.reuters.com/technology/ai-company-glean-hits-72-billion-valuation-latest-funding-round-2025-06-10/)

витрина 3 (оркестрация флота) горячая на словах (langchain $125m / $1.25b 20-21.10.2025; cognition $1b+ / $26b 27.05.2026), но все чеки ≥$3m в 2024-26 стояли на oss, который крутят чужие, либо на исследовательском имени, либо на «signed up» fortune-100. внутреннего флота из нескольких машин + vps как суррогата рынок не принимает. потолок нам сейчас $0.5-1.5m без внешнего прогона, $2-3m если шину откроют и её поднимут 2-3 чужие команды.3(https://techcrunch.com/2025/10/21/open-source-agentic-startup-langchain-hits-1-25b-valuation/)

комбо v1+v3 как «control plane под агентов» усиливает витрину 1. тащить v2 в тот же питч — распыление.

окно v2 закрывается быстрее всех (публичный arr соседа убивает dogfood). окно v1 ещё живое на seed, но не на 24 месяца. v3 на уровне «фреймворк агентов» уже занято; узкая щель «мультимашинный консенсус без human-approval» ещё не убита и не доказана.

это не инвестсовет. это замер, на какой витрине институциональный pre-seed чек вероятнее для команды нашего профиля в конце 2026.

---

B. ВИТРИНА 2 — ENTERPRISE KNOWLEDGE / SECOND BRAIN

1. фандабельная категория в конце 2026 для команды без pedigree?

с оговорками, ближе к нет на заявленную ширину («enterprise knowledge layer поверх любых источников»), ближе к да на узкий клин (comms-native knowledge: telegram/whatsapp → crm/поиск/агент).

категория живая и дорогая. деньги в неё льют. но seed/pre-seed 2025-26 в этом поле покупают одно из трёх: (а) oss, который качают чужие разработчики; (б) research/openai/google pedigree; (в) именные enterprise design-partners. «живой backend на 1.47m своих сообщений» в этот список не входит — это dogfood одного тенанта. инвесторы 2026 это читают как «мы пользуемся своим продуктом», не как «рынок».

2. красный океан — кто владеет категорией

glean (palo alto). series f $150m, оценка $7.2b, лид wellington management, анонс 10.06.2025 (reuters + твит ceo arvind jain). к тому моменту компания говорила про >$100m arr; cnbc disruptor 50 (19.05.2026) пишет $100m arr early 2025 → $200m к концу 2025. сторонние оценки arr ~$300m к маю 2026 (valueaddvc / latka) — это не пресс-релиз компании, помечать как вторичка. founders = ex-google search. конкурент microsoft copilot. для нас это потолок категории, не сосед по стадии.2(https://www.reuters.com/technology/ai-company-glean-hits-72-billion-valuation-latest-funding-round-2025-06-10/)

perplexity. закрытый раунд ~$200m @ ~$20b сентябрь 2025 (sacra / valueaddvc). 24.08.2026 reuters: nvidia в разговорах войти в раунд @ >$30b, annualized revenue >$750m (the information → reuters). это не закрытая сделка. enterprise-слой perplexity — один из продуктов, не вся компания; путать consumer search и enterprise knowledge нельзя.4(https://www.reuters.com/technology/nvidia-discusses-perplexity-investment-30-billion-plus-valuation-information-2026-08-24/)

sierra (bret taylor / clay bavor). $350m @ $10b 04.09.2025, лид greenoaks (techcrunch). $950m 04.05.2026, лиды tiger global + gv, оценка «выше $15b» (techcrunch); teahose/sacra ставят $15.8b post — вторичка. self-reported arr: $100m к 21.11.2025, $150m к нач. февраля 2026. продукт — cx-агенты, не knowledge layer, но в питчах 2026 его кладут в ту же полку «enterprise ai for work». founders = ex-salesforce co-ceo + openai board chair. для команды без имени — несопоставимо.5(https://techcrunch.com/2026/05/04/sierra-raises-950m-as-the-race-to-own-enterprise-ai-gets-serious/)

sana labs. workday купил за ~$1.1b, соглашение 16.09.2025, пресс-релиз workday. оценка sana в oct 2024 ~$500m (cbinsights). omsättning 2024 по шведской отчётности ~95 млн sek (efn) — не arr saas в долларах, не путать. категория «ai knowledge + learning» консолидируется в hr/finance-suite, не остаётся независимой.6(https://newsroom.workday.com/2025-09-16-Workday-Signs-Definitive-Agreement-to-Acquire-Sana)

granola. $20m (окт 2024, блог компании), $43m series b 14.05.2025 лид nfdg (nat friedman / daniel gross), $125m series c 25.03.2026 @ $1.5b лид index (danny rimer), kleiner perkins. позиционирование сдвинуто с meeting notes на «second brain for your team / company context». именные клиенты на c: vanta, gusto, asana, cursor, mistral. это не слабый adoption.7(https://techcrunch.com/2026/03/25/granola-raises-125m-hits-1-5b-valuation-as-it-expands-from-meeting-notetaker-to-enterprise-ai-app/)

writer. $200m series c @ $1.9b 12.11.2024, co-leads premji invest / radical / iconiq (techcrunch). total ~$326m. forbes 10.04.2025: pitch deck показывал $9.3m выручки 2023 / прогноз $28m на 2024; компания назвала цифры неточными и говорила про >$50m signed contracts. self-reported, не аудировано. продукт уехал из «writing» в enterprise agents.8(https://techcrunch.com/2024/11/12/generative-ai-startup-writer-raises-200m-at-a-1-9b-valuation/)

hebbia. $130m series b @ ~$700m, анонс 09.07.2024, лид a16z (techcrunch). в момент раунда, по человеку знакомому с сделкой: $13m arr и прибыль. 50x arr. vertical: finance/legal documents, не горизонтальный second brain.9(https://techcrunch.com/2024/07/09/ai-startup-hebbia-rased-130m-at-a-700m-valuation-on-13-million-of-profitable-revenue/)

dust.tt. seed €5m / $5.5m 27.06.2023 sequoia. series a $16m 27.06.2024 sequoia, self-reported $1m arr. series b $40m 18.05.2026, co-leads abstract + sequoia, snowflake и datadog в раунде. self-reported на b: >3000 организаций, 41k mau (апрель 2026), >300k агентов, 70% wau, zero churn 2025. оценка на b не раскрыта. founders: stanislas polu (openai) + gabriel hubert (alan/stripe). это ближайший «ос для команды на внутренних источниках» — и он уже на series b с sequoia в каждом раунде.10(https://thenextweb.com/news/dust-series-b-40-million-multiplayer-ai-enterprise)

ещё incumbents без свежего «ai seed»: notion ai (notion и так многомиллиардная), microsoft copilot / copilot studio, google notebooklm + gemini for workspace, slack ai, salesforce einstein. отдельного раунда notion ai нет — это фича incumbenta.

плотность: высокая. независимый горизонтальный knowledge layer для enterprise в 2026 — это война с glean/microsoft/workday/notion. место на seed есть только в клине (один источник, одна роль, один workflow), не в «поверх любых источников».

3. кейсы команд, поднявших от $3m в категории при слабом adoption, 2024-2026

честный фильтр: «слабый adoption» = нет публичного arr на миллионы и нет пачки fortune-логотипов в пресс-релизе. таких раундов мало; почти все ≥$3m в knowledge/memory стоят на oss-скачиваниях или на имени основателей. ниже — то, что проходит фильтр хоть частично.

| команда | сумма / стадия / дата | оценка | лид | что реально было в момент раунда | чем заменили выручку | воспроизводимо без имени? |
|---|---|---|---|---|---|---|
| dust | $5.5m seed, 27.06.2023 | не раскрыта | sequoia | «thousands of users» на платформе; продукт — оркестрация llm + внутренние источники (gdrive/github/notion/slack); 0 раскрытого arr | usage + founders openai/stripe | частично: usage-суррогат да, pedigree нет |
| llamaindex | $8.5m seed, 06.06.2023; затем $19m series a 04.03.2025 (итого $27.5m) | a не раскрыта | seed: greylock; a: norwest | на a: команда ~20 человек, llama cloud ga, waitlist «>10 000 организаций, включая 90 fortune 500» — waitlist, не клиенты | oss-фреймворк + waitlist enterprise | частично: oss да; waitlist как суррогат да, если он чужой; имя jerry liu в rag-коммьюнити — частично |
| mem0 | $3.9m seed (ранее не анонсирован) + $20m series a, анонс 28.10.2025, итого $24m | не раскрыта | seed: kindred; a: basis set; ycombinator, peak xv, github fund | 41k github stars, 14m python downloads, api 35m (q1) → 186m (q3 2025), 80k cloud signups, слот exclusive memory в aws agent sdk | oss + api-calls + cloud signups | да, если есть сопоставимый чужой usage. наш 1.47m сообщений одного тенанта — нет |
| unstructured | $25m seed+a, 19.07.2023; $40m series b 14.03.2024 @ $230m | $230m на b | a: madrona; b: menlo | на a: 700k downloads, 2400 github repos; на b: self-reported 50k orgs на oss, ~1k платящих, ~1m downloads/мес | oss downloads + early paid | частично: downloads да; 1k paying к b — уже не «слабый» |
| contextual ai | $20m seed 07.06.2023 @ $100m post; $80m series a 01.08.2024, forge ~$566m / pitchbook ~$609m | $100m seed / ~$0.56-0.61b a | seed: bain capital ventures / lightspeed; a: greycroft | founder douwe kiela (meta/huggingface rag). на seed — thesis rag 2.0, не выручка | research pedigree + thesis | нет |
| letta (memgpt) | $10m seed 23.09.2024 @ $70m post | $70m post | felicis; angels jeff dean, clem delangue | 11-13k github stars, paper memgpt, spinout uc berkeley sky lab | paper + oss + lab | нет без lab/paper; oss-часть да |
| granola | $20m, 23.10.2024 (блог) | не раскрыта на этом раунде; позже $1.5b на c | не назван лид на $20m; дальше nfdg, index | рабочий consumer/prosumer notepad с реальной аудиторией встреч. к b/c уже именные логотипы. слабым adoption это было только на самом первом чеке | product-led usage | частично: plg да; последующие $43m/$125m — уже не слабый adoption |

чего в таблице нет и почему: glean/hebbia/writer/sierra — это не слабый adoption. supermemory $2.6m — ниже порога $3m. limitless/rewind: >$33m historically, 05.12.2025 meta купил (techcrunch) — выход, не seed-прецедент для нас. sana — exit в workday, не seed.

вывод по таблице: прецедента «безымянная команда, один внутренний корпус сообщений, горизонтальный enterprise knowledge, чек $3-5m» за 2024-26 я не нашёл. ближайшие аналоги требуют либо openai/berkeley/hf, либо github stars / api calls, которые считают чужие.

4. входной билет — какой суррогат принимают вместо выручки

принимают в 2026:
- oss, который качают и форкают чужие (llamaindex, mem0, unstructured, dust ранний). метрика = stars, downloads, traces, api calls.
- waitlist с именными доменами / fortune-счётчик (llamaindex a: 90 f500 в waitlist). слабее, чем paid, но ещё живёт на a, на pre-seed — только как приложение к oss.
- design-partners с логотипом, даже без arr (dust seed «thousands of users»; granola ранние teams).
- research-paper + lab (letta).
- не принимают: «мы сами на этом живём» без внешнего тенанта. не принимают «n сообщений в нашей базе» как proxy рынка. не принимают telegram-аудиторию основателя как аудиторию продукта.

наш актив 1.47m сообщений / 126 975 чатов / ~51k диалогов/ночь / 0 llm-токенов: прямой аналог принимаемого суррогата — нет. это production-инженерная улика (система не игрушка), но тенант = 1, источник = свой. аналог был бы: вынести пайплайн как продукт, чтобы 10-50 чужих команд прогоняли свои телеграм/почту через него и это было видно (signups, jobs/night, retention). пока этого нет — билет не куплен.

узкий клин, который ещё могут купить: «knowledge/crm слой над грязными мессенджерами (telegram/whatsapp), не над confluence». это не glean. это вертикаль. для неё билет = 3-5 внешних команд, которые реально ведут лидов на этом backend, плюс цифра вроде «x диалогов/ночь на чужих аккаунтах». свои 51k — демо, не рынок.

5. честный потолок чека для нашего профиля

сейчас, как есть (один тенант, нет oss, нет логотипов, нет pedigree): $1.0-2.5m pre-seed / large angel, скорее не институциональный us seed фонд tier-1. медианный seed на carta в q4 2025 = $4.5m при 435 сделках (−23% yoy); годовой счётчик раундов 2025 = 4859, минимум за 6 лет. ai-seed valuation median порядка $19-25m post. это бенчмарк рынка, не наш потолок: без суррогата нас кладут ниже медианы, не выше.11(https://carta.com/data/state-of-private-markets-q4-2025-full-report/)

с реалистичными добавками за 3-6 месяцев (2-3 внешние команды на telegram-backend + публичный oss-кусок ingestion + один именной design-partner): $2.5-4m. stretch $5m только если клин назван узко и есть cyber/enterprise якорь; горизонтальный «second brain / glean-killer» stretch не открывает.

потолок $6-8m по витрине 2 для этой команды в 2026 — не подтверждено ни одним соседом без имени.

---

C. ВИТРИНА 3 — ОРКЕСТРАЦИЯ АГЕНТОВ ФЛОТА

1. фандабельная категория в конце 2026 для команды без pedigree?

с оговорками. «агентная оркестрация» — да, категория живая. «мультимашинная шина + консенсус + self-heal без human approvals, только на своём флоте» — как отдельная категория не доказана. инвесторы покупают (1) фреймворк, который ставят чужие разработчики, (2) sandbox/runtime, который крутят чужие агенты, (3) coding-агента с arr. внутренний control plane на n машинах основателя — это infra-dogfood, не категория.

2. красный океан

langchain. $125m @ $1.25b, анонс 20.10.2025 (блог компании) / 21.10.2025 (techcrunch), лид ivp; sequoia, benchmark, amplify, capitalg, sapphire. self-reported: 90m monthly downloads langchain+langgraph, 35% fortune 500 «use our services», langsmith traces 12x yoy. forbes 09.07.2025 (до закрытия, со ссылкой на источники): ~$16m arr — вторичка, компания тогда не комментировала. seed был $10m от benchmark апрель 2023 на вирусный oss. это стандарт категории: сначала stars/downloads, потом платформа.12(https://www.langchain.com/blog/series-b)

crewai. $18m seed+a 22.10.2024, seed boldstart, a insight partners; angels andrew ng, dharmesh shah (siliconangle). self-reported на раунде: 150+ enterprise в beta, enterprise ga в день анонса. blitzscaling (18.08.2025): 35k+ gh stars, claim «60% fortune 500 using». forge показывает series b-1 $20m 15.04.2026 @ $177.15m — это secondary/filing-данные, не пресс-релиз; помечать как слабо подтверждённое. arr не раскрыт.13(https://siliconangle.com/2024/10/22/agentic-ai-startup-crewai-closes-18m-funding-round/)

cognition (devin). $400m @ $10.2b сен 2025 (techcrunch 08.09.2025). $1b+ @ $26b post 27.05.2026, лиды lux / general catalyst / 8vc (блог cognition + techcrunch). self-reported run-rate $492m на раунде мая 2026; bloomberg 12.08.2026 — разговоры о раунде @ ≥$40b, run-rate «approaching $1b» — не закрыто. это coding-агент с enterprise-логотипами (mercedes, goldman, nasa в их блоге), не fleet-bus. сосед по слову «агенты», не по продукту.14(https://techcrunch.com/2026/05/27/ai-coding-startup-cognition-raises-1b-at-25b-pre-money-valuation/)

sema4.ai (коммерциализация вокруг robocorp / rpa → агенты; не microsoft autogen). $30.5m series a янв 2024 (dealroom: peter fenton / benchmark). +$25m extension 04.06.2025, итого series a $55.5m, snowflake ventures, mayfield, cox; axios. оценка на extension ~$153m (dealroom). продукт: no-code enterprise agents на snowflake. founder rob bearden — cloudera pedigree. не слабый бэкграунд. microsoft autogen / semantic kernel — продукты корпорации, отдельного стартап-раунда нет.

factory.ai. $50m series b @ $300m 25.09.2025 (блог; nea, sequoia, nvidia). $150m series c @ $1.5b 16.04.2026, лид khosla (techcrunch + блог). self-reported: mongodb, ey, nvidia, adobe; «doubled revenue mom 6 months». это sdlc-агенты, не мультимашинный консенсус.

e2b. $21m series a 28.07.2025, лид insight, total $32m (блог e2b). суррогат: «88% fortune 100 signed up», «hundreds of millions of cloud sandboxes». signup ≠ paid. это closest infra-сосед: облачная машина для агента, не шина между своими компами.

letta — см. выше, $10m seed @ $70m, 23.09.2024. stateful agents / memory, не fleet.

relevance ai. $24m series b 06.05.2025, лид bessemer, total $37m (techcrunch). суррогат: 40 000 агентов создано в январе 2025; клиенты qualified, activision, safetyculture. no-code workforce. оценка не раскрыта.

anthropic mcp — протокол, не стартап. экосистема mcp поднимает security-витрину (v1) сильнее, чем orchestration-витрину: инфраструктуру протокола anthropic не отдаст стартапу, а дыры в серверах — отдаст аудиторам.

плотность: на слое «framework / langgraph / crews» — плотно (langchain unicorn, crewai, msft, dozens of wrappers). на слое «sandbox для агента» — e2b и аналоги. на слое «мультимашинный консенсус + self-heal без апрувов человека, как k8s/temporal для агентного флота» — прямых funded-соседей с этой формулировкой я не нашёл. это либо щель, либо отсутствие категории. без чужого adoption рынок решит, что это второе.

3. кейсы ≥$3m при слабом adoption, 2024-2026

| команда | сумма / стадия / дата | оценка | лид | что реально было | чем заменили выручку | воспроизводимо без имени? |
|---|---|---|---|---|---|---|
| langchain | $10m seed, апр 2023; $25m a ~фев 2024 @ ~$200m | seed не раскрыта; a ~$200m | seed benchmark; a sequoia | вирусный oss, недели/месяцы после запуска; выручки почти нет | downloads / github | частично: вирусный oss да; timing 2023 (первая волна llm-infra) сейчас не повторить |
| crewai | $18m seed+a, 22.10.2024 | не раскрыта | boldstart / insight | oss-фреймворк, 150 enterprise beta, ga enterprise в день раунда, arr не назван | oss + enterprise waitlist/beta | частично: oss да; «150 enterprise» без логотипов в пресс-релизе — слабый, но принятый суррогат |
| letta | $10m seed, 23.09.2024 | $70m post | felicis | paper + 11k+ stars + berkeley sky lab + jeff dean | paper/oss/lab | нет |
| e2b | $21m a, 28.07.2025 (total $32m) | не раскрыта | insight partners | hundreds of millions sandboxes; claim 88% f100 signed up | usage + vanity signups | частично: реальный usage да; f100 «signed up» легко надувается, но инвесторы его едят |
| relevance ai | $24m b, 06.05.2025 (total $37m) | не раскрыта | bessemer | 40k агентов создано за январь 2025; несколько именных клиентов | agent-count + 2-3 логотипа | частично: счётчик созданных агентов да, если чужие |
| sema4.ai | $30.5m a, янв 2024; +$25m 04.06.2025 | ~$153m на extension (dealroom) | benchmark / mayfield / snowflake | rebrand robocorp, rpa installed base, snowflake marketplace | incumbent distribution, не «слабый adoption» | нет: это rpa-компания с новым лейблом |
| factory | $50m b 25.09.2025 @ $300m | $300m / позже $1.5b | nea/sequoia → khosla | enterprise logos + terminal-bench #1 | logos + benchmark | нет для нас: другие продукт, другие клиенты, другой масштаб |

прецедента «чек ≥$3m на внутренний флот без oss и без чужих деплоев» нет в найденном. langchain seed — ближайший по стадии и по «слабой выручке», но там были миллионы чужих загрузок, не 5 своих машин.

4. входной билет

принимают:
- monthly downloads / gh stars / traces (langchain).
- число агентов/песочниц, запущенных чужими (e2b, relevance, crewai).
- «x% fortune 500 using» — едят, даже когда это self-reported и часто = «кто-то из инженеров поставил pip install».
- paper + lab.
- не принимают: uptime своего флота, число своих нод, «консенсус без апрувов у нас в проде». это ops, не рынок.

наш актив (шина + консенсус + self-heal на нескольких машинах + якорный vps, внешний adoption = 0): прямого аналога принимаемому суррогату нет. чтобы появился — шина должна ставиться на чужие машины. метрика, которую поймут: n внешних нод, n пиров, n часов без human-touch, 2-3 публичных деплоя не связанных с антоном. до этого билет не куплен.

5. честный потолок чека

как есть (только внутренний прод): $0.5-1.5m angels / pre-seed. институциональный us seed на этой истории в 2026 — не подтверждён аналогом.

если за 3-6 месяцев: публичный oss шины + 2-3 внешних деплоя (хотя бы маленькие команды) + одна цифра вроде «n дней без human approval на чужом флоте»: $2-3.5m.

stretch $4-6m только при пересечении с security (v1): «control plane, который не пускает агента в систему без политики» — тогда это уже не витрина 3, а упаковка витрины 1.

cognition/langchain-потолки ($1b+) сюда не проецировать: это другие продукты и другие команды.

---

D. СИНТЕЗ

1. ранжирование трёх витрин по лёгкости рейза

| витрина | фандабельность | потолок чека сейчас | плотность конкуренции | входной билет, которого нет | срок годности окна |
|---|---|---|---|---|---|
| 1 аудит агентов/mcp (эталон) | высокая для нашего профиля | $3-5m; stretch $6-8m при 4 условиях из прошлого dr | средняя на seed-security, высокая на cyber-ai growth | 2-3 именных design-partner; независимое воспроизведение дыр; cyber/protocol якорь; production-метрика | 12-18 мес. trend micro уже сканит 19k mcp-серверов (28.05.2026); yahoo 24.08.2026 разбирает cvss 10.0 в terraform mcp. incumbents (wiz/crowdstrike/palo) догонят «достаточно хорошо» |
| 2 knowledge / second brain | низкая на горизонталь; низкая-средняя на клин telegram-crm | $1.0-2.5m; stretch $2.5-4m с 2-3 внешними тенантами | очень высокая (glean $7.2b, copilot, workday+sana $1.1b, dust b, granola $1.5b, writer $1.9b, perplexity) | чужой adoption (не свой корпус); oss или логотипы | закрывается быстрее всех: публичный arr соседа убивает dogfood уже сейчас |
| 3 оркестрация флота | низкая без oss; средняя если шину откроют | $0.5-1.5m как есть; $2-3.5m с внешними деплоями | высокая на framework (langchain $1.25b, crewai, msft); низкая на «мультимашинный консенсус» — щель не валидирована | чужие ноды, не свои; downloads/deploys | framework-слой закрыт. щель control-plane ещё открыта, но без соседа-подтверждения окно может не открыться, а закрыться как «нет категории» |

2. где легче всего и почему

на витрине 1. единственная из трёх, где в 2026 уже есть близкий прецедент без superstar-имени: codeintegrity, 5 человек, $4.8m seed, лид syn ventures, 27.05.2026 (geekwire; dealroom). суррогат раунда — публичная атака (notion за <4 часов, economist), не arr. наши 30 cve / 21k инстансов / oss-ответы мейнтейнеров — того же класса улика (внешний, проверяемый вред), что и их взлом. 1.47m своих сообщений и внутренняя шина этого класса улики не дают: их нельзя независимо воспроизвести как угрозу чужому бюджету. security покупается из страха; knowledge — из сравнения с glean; orchestration — из github. страх у нас есть, сравнение с glean — проигрыш, github — пока свой.1(https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)

3. комбо-гипотеза

за: связка «оркестрация как инфраструктура под безопасность агентов» сильнее суммы. codeintegrity уже продаёт runtime-слой между агентом и системой. наш флот — доказательство, что агенты у нас в проде на нескольких машинах, и control plane умеет консенсус без человека. это не вторая витрина, это proof-of-production к первой. инвестор 2026 читает: «аудитор, который сам гоняет агентов, а не только пишет pdf». mcp-экосистема anthropic усиливает именно этот стык.

против: если в питче три равных продукта (мозг / шина / аудит) — это отсутствие фокуса. seed-фонд 2026 режет такие колоды. v2 в комбо вредна: она тянет в сравнение с glean и сжигает время на «а чем вы лучше copilot».

вердикт: упаковать v3 внутрь v1 как infra-доказательство. v2 держать как внутренний инструмент и отдельный клин только если появится внешний telegram-тенант; в seed-питч v1 её не тащить.

4. окно

закрывается быстрее: витрина 2. glean уже ~$200-300m arr, workday купил sana, microsoft сидит в каждом кресле. суррогат «у нас большой корпус» мёртв, когда сосед публикует arr.

ещё живо: витрина 1 на seed. codeintegrity закрылся в мае 2026 — категория не абсорбирована. но сканеры и cve уже mainstream (trend micro, terraform mcp cvss 10.0). горизонт 12-18 месяцев, не 3 года.

витрина 3: окно framework закрыто langchain/crewai. окно «fleet consensus» формально открыто из-за отсутствия соседа — это риск новизны, не подарок.

5. риск красного океана vs риск новизны

что продать легче инвестору 2026: понятную переполненную категорию (v2) или новую без подтверждений (v3)?

понятную. 2026 seed-капитал сконцентрирован (меньше сделок, больше долларов в сделку, ai забирает ~половину pre-seed dollars — carta q2 2026). партнёр, у которого 30 минут, pattern-match'ит «glean/dust для x» или «langchain для y». v2 он понимает и сразу ставит «why you vs glean». v3 он не понимает и ставит «show me three other companies». оба ответа плохие для нас. v1 он понимает как cyber (категория с бюджетом) и не требует arr на seed, если есть дыра. поэтому не «красное vs новое», а «известная закупка (security) vs известная война (knowledge) vs неизвестная категория (fleet)». security выигрывает у обоих.

---

E. ЧЕМУ НЕЛЬЗЯ ВЕРИТЬ

self-reported arr: sierra $100/150/200m, glean $300m (часть оценок — не компания), cognition $492m, writer «>$50m signed contracts», langchain ~$16m (forbes, компания не подтверждала). signed contracts ≠ recognized revenue. annualized consumption ≠ arr.

cumulative funding как свежий чек: writer $326m total, langchain $160m, glean $765m — это история, не размер последнего раунда и не доступный капитал сейчас.

commitment vs funded: perplexity $30b+ и cognition $40b на 12-24.08.2026 — разговоры (the information / bloomberg), не закрытые раунды.

«x% fortune 500 using»: crewai 60%, langchain 35%, e2b 88% f100 signed up. часто = один инженер поставил библиотеку. signup ≠ seat ≠ paid.

аудитория клиента ≠ аудитория продукта: 1.47m сообщений — объём нашего crm, не число покупателей knowledge-layer. то же для «51k диалогов/ночь».

оценка из неназванных источников: techstackipo ставил glean $12b и выдумывал series g — противоречит reuters $7.2b. forge по crewai $177m b — не пресс-релиз. latka/valueaddvc/sacra — модели, не filings.

pedigree-премия: dust/letta/sierra/cognition/glean закрывают чеки, которые безымянной команде не светят. копировать их round size как «категория платит столько» — ошибка прошлого заказа, только в другую сторону.

«медианный us seed 2024 = $2.5m»: устарело как якорь на конец 2026. carta q4 2025: медианный seed-чек $4.5m, но сделок меньше. медиана рынка ≠ наш потолок.

---

F. НЕПРОВЕРЕННОЕ И ЧТО ОСТАЛОСЬ ЗА ВЕБОМ

не подтверждено живым первичным источником в этом прогоне:
- точная оценка dust на series b (18.05.2026) — сумма $40m есть, post-money нет.
- arr crewai; series b $20m @ $177m (forge, 15.04.2026) — нет пресс-релиза.
- glean arr $300m на май 2026 — вторичка, не пресс-релиз (последнее корпоративное: >$100m на момент f, cnbc про $200m на конец 2025).
- perplexity $30b / $750m run-rate — the information через reuters, раунд не закрыт.
- cognition $40b talks — bloomberg, не закрыто.
- codeintegrity: сумма в dealroom $4.8m, в geekwire «$5 million seed» / total $5.25m — мелкий рассинхрон округления; лид syn ventures совпадает.
- sema4 оценка $153m — dealroom, не блог компании.
- langchain $16m arr — forbes unnamed sources, июль 2025.
- доля «60% fortune 500» у crewai — блог инвестора, не независимо.
- наших 30 cve / 21k инстансов этот прогон заново не аудировал (дано рамкой заказа).
- carta «шестилетний минимум числа сделок»: 4859 раундов за 2025, −41% от 2021 — подтверждено. формулировка «2025 число сделок — шестилетний минимум» из прошлого dr на живых данных стоит.

не искал исчерпывающе (осталось за вебом): pitchbook/form d по mem0 и dust b; полный cap table letta; n8n / temporal / inngest / mastra / browserbase как возможные orchestration-соседи; guru / tettra / slite как старый km; personal.ai; hebbia form d; европейские seed в knowledge (кроме dust/sana); китайские/индийские аналоги. если нужен второй проход — бить form d + pitchbook по dust b, mem0, crewai 2026, e2b, и отдельный список mcp-security seed кроме codeintegrity.

---

🎙 майк: рейзить «второй мозг» как продукт — сейчас самый дорогой способ выглядеть глупо: нас сравнят с glean и выкинут. шина без чужих нод — второй. аудит — единственная витрина, где уже заплатили безымянным. вопрос: готовы ли вы на 90 дней выкинуть v2 из питча и открыть кусок шины как proof к v1, или будем продавать три идеи и не закроем ни одну.

🧒 простыми словами: представь три прилавка. на первом продаёшь замок от воров — люди уже платят даже незнакомым, если вор показан вживую. на втором продаёшь «умную библиотеку» — там уже стоят огромные магазины, и «у меня дома много книг» никого не Greет. на третьем продаёшь «дирижёра роботов» — тема модная, но покупают только если роботы крутятся у чужих, не на твоей кухне. стоять надо у замка. библиотеку не тащить в тот же разговор. дирижёра показать только как доказательство, что замок стоит на живых роботах.
