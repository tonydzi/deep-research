# DR26-08-31-ZB-01-2223 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-31-ZB-01-2223
vendor: grok
rail: grok CLI (подписка xAI), --reasoning-effort high, web-search ON
collected: 2026-08-31

source: external deep research (verbatim)
---
Сначала подниму живые данные: вакансии, seed-раунды, компенсации и то, что рекрутеры/фаундеры пишут прямо сейчас. Потом сверю это с выводами из LobsterDAO.Собрал внешние данные. Короче: LobsterDAO угадал направление (крипта режет людей, AI берёт), но наврал в деталях про деньги. Ниже — цифры, живые вакансии и развилка.**DR26-08-31-ZB-01-2223** · дата среза: 31 августа 2026 · рынок труда и seed в крипте/AI, H2 2026

🎙 **Майк:** идём неверно, если выбирать «наняться в крипто-стартап» или «поднять seed на DeFi-агентов». Верный ход для этого профиля — сесть на **FDE / GTM Engineer / AI Agents Solutions Architect** в Rain / Circle / OpenAI (financial services) / Kraken на 12 месяцев и параллельно копать узкий клин **on-chain verification/escrow**, не generic agent execution. Вопрос, который решает развилку: **есть ли живой оффер на FDE/GTM в 6–8 недель?** Если нет — тогда seed. Если да — сначала зарплата и дистрибуция.

---

# Крипто/AI-рынок H2 2026: где реально нанимают и какую нишу легче поднять

## Короткий вердикт

Две скорости рынка — это не настроение чата, это доски вакансий.

| Ось | Крипта H1–Q2 2026 | AI / AI-native |
|---|---|---|
| Объём найма | Резкое сжатие: январь ~−80% YoY новых постов; H1 — 2 932 активных вакансии [Tiger, вторичные пересказы] | Бум: AI-native индекс 115 348 живых постов на 2 июля; 39 882 из них у 5 849 AI-native компаний |
| Кто режет | Coinbase ~700 (май), Kraken 150, EF 54, BitMEX закрывает торговлю в сентябре; >7 000 увольнений с начала года | Лабы набирают GTM и FDE; AWS объявил $1B на FDE |
| Кто берёт | Engineering 34–46%, compliance ~10%; BD перегрет кандидатами (индекс 2.4×) | FDE уже больше, чем AI/ML engineering (3 077 vs 2 139) |
| Деньги | Медиана web3 seed Q2: **$5.2M**; pre-seed **$2.5M** | AI-agent раунды июля–августа: сотни миллионов, но это **не крипта** |
| Remote | Традиционная крипта ~52% remote; AI-native крипта 24% [CryptoJobsList] | AI-native fully remote 29.7%; лабы (OpenAI 4% remote) — исключение |

**Гипотеза «AI-нарратив + педигри поднимают без продукта»** — частично верна для *AI-fintech* (Natural: $30M Series A в возрасте 193 дней) и *частично ложна* для крипты: Morpho поднял $175M при **$11B+ депозитов**; Variational — $50M при **$200B cumulative volume**, не при нулевом TVL. Neutrl $5M — это **апрель 2025**, и 29 августа 2026 протокол уже стоит на паузе.

**Оценка (моя, не факт):** для профиля «оператор + BD/VC + строит агентов, без ML-research» ожидаемая ценность найма на 12 месяцев **выше**, чем seed «AI-агенты для DeFi-исполнения». Переворот — только если за 8 недель есть term sheet $4–8M от crypto-native лида **и** канал дистрибуции (Circle / Rain / Coinbase). Confidence: **[emerging]**.

---

## A. Рынок труда H2 2026

### A1. Данные, не настроение

**Крипта сжимается, AI раздувается. Это [established] по направлению, [emerging] по точным цифрам, потому что источники считают разные вселенные.**

**Крипта — сжатие.**
Tiger Research (H1 2026, 23 июня): рынок не вернулся к пику 2022; в январе 2026 новые посты на крупных досках упали примерно **на 80% YoY**. В 2025 Coincub насчитал 66 494 новых crypto-вакансии (+47% к предыдущему году), но это всё ещё ниже 2022. На H1 2026 Tiger собрал **2 932 активных** вакансии: engineering 34.1% (999), compliance/legal 10.4% (306). На биржах BD/sales — только 6.7% против 16% compliance. {render_inline_citation}citation_id={15}{/render_inline_citation} {render_inline_citation}citation_id={20}{/render_inline_citation}

TechFlow (14 августа 2026), сводя 7 внешних отчётов + Moledao Q2: с начала 2026 Web3 уволил **>7 000** человек в >40 компаниях. Bitcoin с ~$126 200 (октябрь 2025) до $60–64k к концу июля. Coinbase 5 мая — ~700 человек (14%), Armstrong назвал это «AI-native» трансформацией. Kraken — 150. 23 июня Ethereum Foundation — 54. 23 июля BitMEX объявил закрытие торговли в сентябре. {render_inline_citation}citation_id={4}{/render_inline_citation}

**Противоречие, не сглаживаю.** CryptoJobsList 12 августа пишет, что blockchain hiring **вырос 41.5% в июле 2026** несмотря на увольнения — перераспределение в инфраструктуру, institutional finance и AI, не общий рост отрасли. {render_inline_citation}citation_id={6}{/render_inline_citation} [single-source на +41.5% июля]

**AI внутри крипты — не рост штата, а смена требований.**
Доля JD с AI-скиллами: **23% в начале 2025 → 53.1% в марте 2026**. Это повторяют CryptoJobsList (отчёт 6 мая, >2 000 постов + опрос 800+) и Tiger. Премия mid-level: **$115k AI vs $95k non-AI (+21.1%)**. {render_inline_citation}citation_id={6}{/render_inline_citation} {render_inline_citation}citation_id={4}{/render_inline_citation}

Remote внутри крипты расслоилось: традиционные crypto-роли 52.1% remote, **AI-native crypto-роли только 24% remote**. Дубай в опросе CryptoJobsList назвали идеалом 43.8% респондентов vs 15.5% Silicon Valley. Это опрос, не ATS. [emerging, survey]

Moledao Q2 (TechFlow): engineering = 46% спроса, но только 27.1% заявок (индекс 0.6× = дыра). **BD индекс 2.4×** — кандидатов сильно больше, чем мест. PM 1.4×. Это бьёт профиль «просто BD в крипте»: туда очередь. {render_inline_citation}citation_id={4}{/render_inline_citation}

**AI вне крипты — другой рынок.**
Landed, снимок **2 июля 2026**, 115 348 живых постов, 14 849 компаний, 20 870 с зарплатной вилкой: медиана AI-native **$186k** vs $165k у остальных (+13%). Mid-level премия +21% ($173k vs $143k). FDE медиана $180k. AI/ML engineering $233k. Sales & GTM $183k. Fully remote среди AI-native с указанным режимом: **29.7%** (лабы — исключение: OpenAI 4% fully remote из 721 роли). {render_inline_citation}citation_id={2}{/render_inline_citation}

Indeed Hiring Lab (январь 2026, историческое для H2, но опорное): посты с упоминанием AI на **134% выше** допандемийной базы, общие US-посты только +6%. Forbes / FlexJobs 29 июня: AI-skilled роли +69%, рынок в целом +9%. DataCamp (21 августа): AI/data посты +80% за год, AI engineer +255%. {render_inline_citation}citation_id={1}{/render_inline_citation} {render_inline_citation}citation_id={22}{/render_inline_citation}

PwC 2026 Global AI Jobs Barometer (через Tiger): US-посты с AI-скиллами ≈ **1.12 млн в 2025**, +66% YoY, 2.8% всех постов. {render_inline_citation}citation_id={15}{/render_inline_citation}

**Компенсации — нельзя склеить в одну медиану.**
- AI-native posted median $186k (Landed, июль 2026) — [emerging, ATS]
- Crypto AI mid $115k vs $95k (CryptoJobsList, май) — это **ниже** AI-native рынка; либо выборка беднее, либо «AI в JD» ≠ AI-роль
- Gate: средний рост зарплат crypto-практиков ~18% в 2026; quant до $200k, junior dev ~$77k [single-source]
- Aipplify (апрель, [low-authority]): средняя AI/crypto/Web3 $127.4k, remote 68% — смешивает три рынка, не использую как якорь

**Оценка:** в H2 2026 крипта нанимает **узко** (биржи, платежи, compliance, AI-enablement внутри CEX). AI нанимает **широко** и платит на 1.5–2 ступени выше за тот же «операторский» талант. Remote в крипте ещё жив на CEX (Kraken), в frontier AI — почти нет.

### A2. Живые роли на стыке AI × крипта/финтех

Приоритет — свежие посты августа 2026. Часть Coinbase-листингов на агрегаторах висит 6–12 месяцев — помечены.

| # | Компания | Тайтл | Локация | Вилка | Дата / свежесть | Ссылка |
|---|---|---|---|---|---|---|
| 1 | **Kraken** | AI Agents Solutions Architect – HR | US, remote | $127k–$254k | 20 авг 2026 | [cryptojobslist.com/jobs/ai-agents-solutions-architect-hr-united-states-at-kraken](https://cryptojobslist.com/jobs/ai-agents-solutions-architect-hr-united-states-at-kraken) |
| 2 | Kraken | Senior Software Engineer, AI Enablement – HR | US | $127k–$254k | ~7–11 дней | [web3.career/web3-companies/kraken+ai](https://web3.career/web3-companies/kraken+ai) |
| 3 | Kraken | Senior Software Engineer, AI Enablement – Compliance | US | $127k–$254k | ~7–11 дней | то же |
| 4 | Kraken | Senior Software Engineer, AI Enablement – Finance | London | $130k–$132k | ~9–13 дней | то же |
| 5 | Kraken | Principal Business Operations – AI Automation | London | $91k–$156k | ~4 мес | то же |
| 6 | Kraken | Site Reliability Engineer, AI Agents | US | $96k–$192k | ~3 мес | то же |
| 7 | Kraken | Head of Fraud, Payments & Banking | London | $147k–$210k | ~6 дней | [web3.career/web3-companies/kraken](https://web3.career/web3-companies/kraken) |
| 8 | **OKX** | Principal AI Engineer, AI Agent Development | Remote | $96k–$111k | ~5–6 дней | [web3.career/ai-jobs](https://web3.career/ai-jobs) |
| 9 | OKX | Principal AI Engineer, Chatbot Development | Remote | $129k–$216k | ~5–6 дней | то же |
| 10 | OKX | Principal Engineer, Agent Infrastructure / Memory Architecture | Remote | $148k–$149k | ~5 дней | [web3.career/web3-companies/okx+executive](https://web3.career/web3-companies/okx+executive) |
| 11 | **Taxbit** | Agentic AI Engineer | Remote | $130k–$170k | ~5–6 дней | [web3.career/ai+remote-jobs](https://web3.career/ai+remote-jobs) |
| 12 | **DV Trading** | Senior AI Engineer | Remote | $200k–$300k | ~5–6 дней | то же |
| 13 | **Rain** | Backend Engineer, AI | On-site | $165k–$200k | 28 авг 2026 | [startup.jobs/backend-engineer-ai-rain-xyz-7885996](https://startup.jobs/backend-engineer-ai-rain-xyz-7885996) |
| 14 | **OpenAI** | Forward Deployed Engineer – SF | Hybrid SF, 3 дня офис, travel ≤50% | $162k–$280k base | листинг 28 авг | [startup.jobs/forward-deployed-engineer-fde-sf-openai-6156168](https://startup.jobs/forward-deployed-engineer-fde-sf-openai-6156168) |
| 15 | OpenAI | FDE — Financial Services & Investment | NYC | не раскрыта в сниппете; US FDE у OpenAI historically до ~$345k + equity (BI, нояб 2025, историческое) | набор с конца 2025, живой в 2026 | LinkedIn Colin Jarvis, дек 2025 / май 2026 |
| 16 | **Handshake** | Senior AI Forward Deployed Engineer | SF hybrid 3× | $257k–$300k | 28 авг | [startup.jobs/senior-ai-forward-deployed-engineer-joinhandshake-8684063](https://startup.jobs/senior-ai-forward-deployed-engineer-joinhandshake-8684063) |
| 17 | **Palantir** | Forward Deployed Software Engineer, New Grad – Commercial | Hybrid | $135k–$145k | 27 авг | [startup.jobs/forward-deployed-software-engineer-new-grad-commercial-palantir-8628292](https://startup.jobs/forward-deployed-software-engineer-new-grad-commercial-palantir-8628292) |
| 18 | **White Circle** | Founding GTM Engineer | Hybrid UK | £80k–£110k | 30 авг | [startup.jobs/founding-gtm-engineer-white-circle-8656560](https://startup.jobs/founding-gtm-engineer-white-circle-8656560) |
| 19 | **MoonPay** | Staff Machine Learning Engineer | London | $89k–$152k | ~7 дней | [web3.career/ai-jobs](https://web3.career/ai-jobs) |
| 20 | **Binance** | Core Experience AI Product Manager | Asia / Hong Kong | не раскрыта | ~4–5 дней | то же |
| 21 | **Zscaler** | AI Security Account Executive | Remote | $123k–$150k | ~3–4 дня | [web3.career/ai+remote-jobs](https://web3.career/ai+remote-jobs) |
| 22 | **Brave** | AI Systems Researcher Engineer | Remote | $84k–$90k | ~3–4 дня | то же |
| 23 | **MLabs** (клиент HFT/agents) | Staff Software Engineer – Backend & AI Infra | Remote, US–GMT | $120k–$150k | 28 авг | [startup.jobs/staff-software-engineer-backend-ai-infra-mlabs-7869556](https://startup.jobs/staff-software-engineer-backend-ai-infra-mlabs-7869556) |
| 24 | **Polynom** | Forward Deployed AI Engineer | Paris / Rueil, hybrid | не раскрыта | 31 авг | [welcometothejungle.com/.../forward-deployed-ai-engineer](https://www.welcometothejungle.com/fr/companies/polynom/jobs/forward-deployed-ai-engineer) |
| 25 | **Natural** | hiring across every function (payments for agents) | SF | не раскрыта; 17 человек, $40M+ raised | 20 июл + кредит $100M 19 авг | [natural.com/blog/natural-series-a](https://www.natural.com/blog/natural-series-a) · [email hidden] |
| 26 | **Coinbase** | Principal Machine Learning Engineer | Remote-USA | $306k–$360k | ⚠️ агрегатор показывает 6 мес — **возможно протухло** | [web3.career/web3-companies/coinbase+ai](https://web3.career/web3-companies/coinbase+ai) |
| 27 | Coinbase | Senior Product Manager, AI Productivity | Remote | $170k–$200k | ⚠️ ~1y на агрегаторе | то же |
| 28 | Coinbase | Senior SRE, Core AI Infrastructure | Remote | $186k–$218k | ⚠️ ~1y | то же |

Контекст по объёму досок на конец августа: web3.career показывает **1 348 AI-вакансий в Web3** и **716 remote AI**; CryptoJobs AI — **2 401** открытых crypto/web3 (из них 2 339 remote) — вселенные разные, не складывать. Kraken на агрегаторе — **863** вакансии, OKX — 127 executive-ленты. {render_inline_citation}citation_id={87}{/render_inline_citation} {render_inline_citation}citation_id={85}{/render_inline_citation} {render_inline_citation}citation_id={83}{/render_inline_citation}

Кто **реально закрывает** стык:
- **Биржи, ставшие AI-native internally:** Kraken (агенты в HR/Compliance/Finance, стек n8n + Claude API + MCP + Python — это ровно «оператор агентов», не research). {render_inline_citation}citation_id={5}{/render_inline_citation}
- **Stablecoin payments:** Rain ($250M Series C январь 2026, оценка $1.95B, ICONIQ) нанимает первую AI-команду под agentic payments. {render_inline_citation}citation_id={26}{/render_inline_citation}
- **CEX product AI:** OKX — agent infrastructure + memory + chatbots.
- **Tax/compliance crypto:** Taxbit — Agentic AI Engineer.
- **Prop/HFT:** DV Trading $200–300k; MLabs — agent runtime для HFT.
- **Frontier, не крипта, но деньги:** OpenAI FDE (в т.ч. Financial Services, NYC), Handshake FDE $257–300k.
- **Agent payments, не crypto-native:** Natural (Forerunner $30M, 193 дня от основания). {render_inline_citation}citation_id={15}{/render_inline_citation}

**Оценка:** 15–20 из таблицы — реальный fit. Coinbase-строки без проверки ATS — не считать живыми.

### A3. Роли без формального ML-research

Рынок сам назвал дверь. Landed: AI-native компании постят **больше FDE, чем AI/ML engineering** (3 077 vs 2 139). Sales/GTM 14.5% vs AI/ML 5.4%. «Большинство работ в AI-компаниях — не AI-работы». {render_inline_citation}citation_id={2}{/render_inline_citation}

**Тайтлы, которые берут оператора+BD+агентов:**
1. **Forward Deployed Engineer / Technical Deployment Lead** — Palantir-модель, теперь OpenAI, Anthropic, AWS ($1B, 30 июня), Databricks (281 открытых FDE в индексе Landed).
2. **GTM Engineer / Founding GTM Engineer** — агенты на пайплайн, Clay/n8n/Claude, не research.
3. **AI Agents Solutions Architect** — Kraken: «full-stack ownership», n8n + Claude + MCP, governance, transfer capability бизнес-команде.
4. **Principal Business Operations – AI Automation**
5. **Applied AI / Technical Deployment** в financial services
6. **Partnerships / Strategic Accounts** в stablecoin (Rain Head of Strategic Accounts был на доске; MoonPay)
7. **Product Manager, AI Productivity** (Coinbase — если живой)

Это **не** BD в DeFi-протоколе. Moledao прямо говорит: BD перегрет, generic резюме не видно.

**Люди 2025–2026 (имя → что было до → куда):**

| Кто | Было | Стало | Когда | Источник |
|---|---|---|---|---|
| **Gary Lin** (@garymlin) | Palantir → Explo (куплен Omni) | OpenAI, leadership FDE | 14 мая 2026 | его пост: «reunited with friends from Columbia, Palantir, and Explo» |
| **Colin Jarvis** (@colintjarvis) | OpenAI Europe с ноября 2022, не research lab | Global Head of FDE; команда 2 → ~40 за 2025, найм SF/NYC/Dublin/London/Paris/Munich/Singapore/Tokyo + **FDE Financial Services NYC** | 2025–2026 | LinkedIn дек 2025; Altimeter/BI нояб 2025 |
| **Kahlil Lalji** | Ivella (YC banking, продан Earnin 2023) | CEO Natural, $30M Series A за 193 дня | июл 2026 | TechCrunch 20 июля; natural.com |
| **Reid Cuming** | co-founder Superstate (токенизация) | CEO Ground, $3.6M pre-seed Bain + ParaFi | июн 2026 | The Block 24 июня |
| **Vyas Krishnan, Brianna Montgomery** | EigenLayer | Nava, $8.3M seed (escrow/verification агентов) | апр 2026 | Fortune 14 апреля / FinSMEs |
| **Connor Dempsey** (@Cdempsey44) | crypto operator | stablecoins @ Rain; публично описывает рынок найма | 1 авг 2026 | X, 120k просмотров |

FDE-комп 2026: market-rate медиана ~$180k posted; lab FDE mid total ~$385k, principals >$1.2M (Perspective AI, май 2026, n=1 200 — [emerging]). AWS 30 июня: $1B в FDE. Google Cloud CEO публично набирал; 59 FDE-открытий за раз (июнь, TechTimes). {render_inline_citation}citation_id={62}{/render_inline_citation} {render_inline_citation}citation_id={69}{/render_inline_citation}

**Оценка:** путь «без ML-paper» в 2026 **существует и переплачен**. Это FDE/GTM/Solutions Architect, не «Head of BD» в протоколе. Педигри Palantir/Stripe/crypto-infra важнее, чем arXiv.

### A4. O-1 в 2026

**Факт:** O-1A не имеет кэпа и лотереи, нет prevailing wage. Петицию подаёт US employer, agent, **или US-юрлицо бенефициара** — это USCIS policy update **январь 2025**, не серая схема. Initial 3 года, unlimited extensions. {render_inline_citation}citation_id={32}{/render_inline_citation} [Talvisa, 4 июля 2026]

Индийские O-1: 487 в FY20 → **1 566 в FY24**; в мае 2025 113 заявок vs 91 годом ранее (Boundless, через ET 24 августа 2026). H-1B дороже и лотерейнее — основатели AI идут в O-1. {render_inline_citation}citation_id={26}{/render_inline_citation}

Beyond Border пишет «20 000 approvals in FY 2026» — **[low-authority], не подтверждаю**. OpenSphere (8 июня): кейс O-1A для фаундера AI-agent marketplace, previously «$3B omnichain protocol» — маркетинговый success story, [low-authority].

**Что O-1 меняет для этого профиля (оценка, не закон):**
- Фильтр «нужен H-1B / lottery» **снимается**. Компании вроде Coinbase, Kraken, OpenAI, Rain **могут** принять O-1 holder сменой работодателя (amendment/new I-129). Это дешевле и быстрее лотереи.
- Фильтр «мы не спонсируем визы» у маленьких crypto-стартапов **остаётся**: им всё равно нужен US counsel и petitioner. O-1 не магия.
- Founder-path: своё Delaware LLC + oversight (совет, который может уволить) может само петиционировать. Это и есть окно «запустить своё из США».
- Лабы (OpenAI) часто требуют onsite SF/NYC — виза не блок, **офис** блок.
- **Не нашёл** публичного реестра «компании, которые в 2026 явно пишут O-1 welcome» в JD. Типичный JD молчит про визу. [не подтверждено]

**Оценка [emerging]:** при уже действующем O-1 вы **дороже**, чем кандидат с просьбой о спонсорстве, и **дешевле в friction**, чем H-1B. Это аргумент в сторону найма в US AI/fintech, не в сторону «виза закроет двери».

---

## B. Фандрайз при слабом adoption

### B1. Seed июнь–август 2026 (реестр, не слухи)

**Картина web3 early-stage (Q2 = апр–июн, InnMind / DeFiLlama recompute, снимок 4 июля):**
- Медиана seed **$5.2M** (Q1 было $6.0M, Q4’25 $4.6M)
- Медиана pre-seed **$2.5M** (↑67% QoQ), сделок pre-seed 11 → 17
- Seed сделок 41 → 36
- Бумага: post-money SAFE + token warrant; голых SAFT на pre-seed в их сете **ноль**
- Активных чек-райтеров ~651 в Q2, минимум с 2020
- Вертикали early: DeFi 15 · stablecoins/payments ~10 · crypto×AI agents 8 · prediction 5 · RWA 4 · DePIN 3 · **SocialFi/NFT/metaverse/gaming/restaking ~0** {render_inline_citation}citation_id={40}{/render_inline_citation}

Galaxy «crypto VC −59% QoQ» InnMind режет до ~−29% после вычета одного $2B MGX→Binance. Early-stage count почти плоский: 59 → 52 → 55.

**Именованные раунды (июнь–август + соседние, с URL):**

| Дата | Компания | Сумма | Стадия | Лид | Что вместо traction | Ссылка |
|---|---|---|---|---|---|---|
| 9 июн | **Morpho** | $175M | token purchase, ~$2B val | Paradigm, a16z crypto, Ribbit | **Не seed и не zero-product:** $11B+ deposits, Coinbase/Kraken/Binance/Galaxy/Anchorage | [morpho.org/blog/...](https://morpho.org/blog/morpho-association-raises-175m-to-build-the-open-credit-network-for-the-world/) 9 июня |
| 24 июн | **Cambrian** | $6M (total $11.9M) | Seed, SAFE+warrant | Franklin Templeton, Polychain | API живой: $4.5B lending indexed, 320k DEX pools; a16z CSX pre-seed 2025 | [theblock.co/post/406028](https://www.theblock.co/post/406028/a16z-csx-backed-cambrian-seed-round-blockchain-data-oracle-network) |
| 24 июн | **Ground** | $3.6M | Pre-seed, SAFE+warrant, **без board seats** | Bain Capital Crypto, ParaFi | Superstate co-founder; 3 FTE; API в Aave/Morpho/Maple/Kamino. Раунд закрыт окт 2025, анонс июн 2026 | [theblock.co/.../ground-funding](https://www.theblock.co/news/deals/2026-06-24-superstate-co-founder-ground-funding-crypto-onchain-yield-406040) |
| 20 мая | **Variational** | $50M | Series A | Dragonfly; Bain, Coinbase Ventures, Peak XV | **Не zero TVL:** $200B+ volume, 50k+ accounts, $750M+ OI с беты янв 2025; команда Lucas Schuermann / Edward Yu | CoinDesk 21 мая; FinanceFeeds 20 авг |
| 14 апр (чуть раньше окна, но якорь ниши) | **Nava** | $8.3M | Seed | Polychain, Archetype; Sreeram Kannan angel | Escrow+verification агентов; команда EigenLayer/Consensys; product = testnet/waitlist | Fortune 14 апр; navalabs.ai 7 апр |
| 20 июл | **Natural** | $30M (total >$40M) | Series A @ ~$150M [toflow, single-source на val] | Forerunner (Kirsten Green) | **193 дня**, 17 человек, 6 продуктов GA (wallets/vaults/pay); фаундер — fintech (Ivella), не ML lab | [natural.com/blog/natural-series-a](https://www.natural.com/blog/natural-series-a) |
| 19 авг | Natural | credit facility до $100M | debt | Upper90 | поверх equity | Unite.AI 19 авг |
| 27 июл | **Pilot Protocol** | $4.5M | Seed | Version One; Precursor, Night Capital, Lenny Rachitsky | «250k agents, 2B requests/day» — **self-reported**, не аудировано | citybiz 27 июл |
| ~3–8 июл | **AIsa** | $6.5M cumulative | Seed | Alibaba + Tribe; Draper, Sumitomo, Saison | «150× users Feb–Jun, 200× API» — self-reported | CryptoRank 8 июл |
| 24 июл | **World Foundation** | $52.5M | locked token sale | Pantera, Bain Crypto, Eightco | 39M users, 18M Orb-verified — identity vs deepfakes, не seed-стартап | CoinDesk 24 июл |
| 8 июл | **Paradigm** | $1.2B | AI/robotics **fund** | n/a | сам VC ушёл в AI; уже Zipline, True Anomaly | CoinDesk 8 июл |
| 6 мая | **Corgi** | $160M | Series B @ $1.3B | TCV | **Не крипта.** YC’24, licensed carrier, ~$100M ARR claim, Deel/Artisan. AI-страховка стартапов | TechCrunch 6 мая |
| 19 июн | **Pramaana Labs** | $27M | Seed | Khosla, Accel… | Formal verification LLM для tax/law/finance; Индия | BW Disrupt 19 июн |
| 13 авг | **Mindgard** | $30M | Series A | Album VC | Red-team AI (Cursor 0-day, ChatGPT guardrails) | Tech Funding News |
| 25 июн | **Sail Research** | $80M | Seed+A | Kleiner, Sequoia | AI agent infrastructure — **не crypto** | aifundingtracker |
| 20 июл | **Neo Security** | $100M | Seed+A | a16z, Bessemer | AI appsec | то же |
| 25 июн | **Arca** | $48.5M | Seed+A | GC, Accel | AI-native wealth | то же |
| 20 мая (Lobster якорь) | Variational | см. выше | | | | |
| 18 апр 2025 (**историческое**) | **Neutrl** | $5M | Seed | STIX, Accomplice | **Не H2 2026.** 29 авг 2026: пауза контрактов, $27M liquid vs ~$137M book | Tracxn; CryptoTimes 29 авг |

InnMind ещё верифицирует в Q2: Hypernova $3M (Lemniscap, 250 трейдеров / $30k paid out), XO Market $6M (20VC, Picus, Coinbase Ventures — **prediction markets живут**), Legend $3.5M (Electric), Daya $2.4M (Hivemind — Africa stablecoin), Saturn Credit $2M (Spartan), TVL Capital $5M (Framework).

**Что заменяло traction, когда продукта мало:**
- **Команда/педигри:** Ground (Superstate), Nava (EigenLayer + Sreeram), Natural (Ivella/Earnin + Stripe/Ramp hires)
- **Self-reported usage:** Pilot 250k agents, AIsa 150× — **не проверял independently**
- **Институциональный якорь:** Cambrian = Franklin Templeton на cap table; Morpho = Apollo/VanEck/Circle
- **R&D/formal methods:** Pramaana $27M seed
- **Настоящий объём:** Variational $200B — это не «без продукта»

### B2. Гипотеза «AI-обёртка + педигри = деньги без продукта»

**За (steelman):**
Natural: 193 дня, $30M Series A, фаундер fintech, не AI-lab, «agentic payments aren’t yet a thing» — цитата Kirsten Green. {render_inline_citation}citation_id={15}{/render_inline_citation}
Nava: $8.3M на escrow, waitlist/testnet.
Pilot: $4.5M на «internet for agents», цифры adoption самодекларация.
Sail Research $80M seed+A (Kleiner/Sequoia) — агент-инфра, не crypto.
Pramaana $27M seed без зрелого enterprise footprint в открытых источниках.

Paradigm поднял **$1.2B AI/robotics fund** (8 июля) — крипто-VC голосует ногами за AI. {render_inline_citation}citation_id={39}{/render_inline_citation}

**Против:**
Morpho $175M — это **открытая кредитная сеть с $11B депозитов**, не обёртка. {render_inline_citation}citation_id={0}{/render_inline_citation}
Variational $50M — **$200B volume**. LobsterDAO про «нулевой TVL» здесь **неправ** (или смотрел другую метрику/другой момент).
Corgi $160M — **страховка**, не крипта; плюс claimed ~$100M ARR (Forbes 6 мая). {render_inline_citation}citation_id={46}{/render_inline_citation} {render_inline_citation}citation_id={48}{/render_inline_citation}
Neutrl: seed был, продукт был, **август 2026 = ликвидный стресс** ($27M liquid, early redemption). Это контрпример «поднял = победил».
InnMind: 15% сидов 2022 дошли до токена; поэтому инвесторы требуют SAFE, не SAFT, и «маленький on-chain result» (Hypernova: 250 users, $30k paid) бьёт большой промис.

**Survivorship bias — называю прямо.** Публичные раунды = выжившие. Базы отказов нет: ни Carta, ни PitchBook полный dump seed-rejections за июнь–август 2026 я не открыл. InnMind даёт косвенное: пул чек-райтеров схлопнулся до ~651; cold email ~2%; 68% H1 seed начались с warm intro. Это **не** «AI-обёртка всегда пролетает», это «без intro и без маленького доказательства почти никто не пишет чек».

AI-seed отдельно: InnMind в соседнем материале («7 hidden truths») пишет, что **record AI funding прячет жестокий seed для no-name**. Gravity считает Q3 AI-agent ~$1.32B / 20 раундов к 15 августа — средние чеки $49M→$97M, это **не** ваш чек. {render_inline_citation}citation_id={35}{/render_inline_citation}

**Вердикт по гипотезе: [emerging], с поправкой.** AI-обёртка + педигри работает в **AI-fintech и agent-infra**, если фаундер из Stripe/Palantir/EigenLayer/YC-banking. В **крипто-DeFi без объёма** в H2 2026 чек дают на plumbing (escrow, data, yield API), не на «ещё один агент, который свапает». Corgi в список крипты из LobsterDAO класть нельзя.

### B3. Три ниши — лёгкость seed сейчас

Шкала 1–10 = «насколько реалистично поднять $3–8M seed в ближайшие 90 дней **этому профилю**». Это **оценка**, не рынок.

**(1) AI-агенты для DeFi-исполнения — 3/10, типичный чек $2–6M если вообще, красный океан нарратива.**

Уже заняли / шумят: Nava (verification перед исполнением), Cambrian (data/oracle для агентов), Virtuals (17k агентов / $39.5M cum. revenue — [low-authority, Digital Asset Jobs март]), TermiX $1M YZi Labs (AACP, points farming — X, [low-authority]), Circle Agent Stack (wallets, marketplace, nanopayments USDC, май–авг 2026), x402 Coinbase. Keyrock: agent settlement >$73M / 176M tx май 2025–апр 2026, потом flattening; a16z/Keyrock предупреждают, что часть x402 volume **неорганическая**. {render_inline_citation}citation_id={33}{/render_inline_citation}

Почему тяжело: «агент свапает и фармит» = 2024–25 нарратив, который Lobster сам слышит как hopium. Инвесторы Q2 платят за **plumbing под агентами**, не за ещё одного исполнителя. Без уникального execution venue / RFQ / лицензии это токен-обёртка.

**(2) B2B стейблкоин / cross-border — 5/10 на seed, но 2/10 если без лицензии. Типичный чек $2.4–8M equity (Daya $2.4M Africa; дальше сразу growth как Rain).**

Живой спрос [emerging]: GENIUS Act (июль 2025, историческое) разогрел compliance-найм; Stablecoin Insider (11 июня): stablecoin roles +20–40% к fintech, compliance $150–250k. Rain — $1.95B, Series C $250M. Circle строит Agent Stack. Connor Dempsey (Rain, 1 августа, 120k views): «stablecoin adoption has hit escape velocity». {render_inline_citation}citation_id={8}{/render_inline_citation}

Кто занял: Circle, Coinbase, Rain, MoonPay, Tether-adjacent Africa/MENA (Hivemind, Galaxy). InnMind: «rails are busy; Africa & MENA most legible entry, equity not tokens». Красный океан на US/EU licensed issuance. Белый — **agent-native treasury / B2B RFQ поверх чужих рельс**, если есть BD-клинья в банки. Для вашего профиля это ближе к **найму в Rain/Circle**, чем к своему seed: капитал и лицензии съедят 18 месяцев.

**(3) Верификация и аудит AI-агентов (ваш якорь) — 6.5/10 на узкий on-chain клин, 3/10 на generic enterprise guardrail. Типичный crypto-seed $5–8M (Nava $8.3M); enterprise safety — уже Series A $30–64M.**

Заняли:
- **Crypto-onchain:** Nava $8.3M (Polychain/Archetype) — `agent.propose → arbiter.verify → escrow.execute`. Это прямой конкурент якорю.
- **Enterprise agent security (не крипта):** Straiker $64M Series A (июнь), Mindgard $30M (авг), Runlayer $30M (июнь, Felicis/Khosla), CodeIntegrity $4.8M (май), Certiv $4.2M (март), Capsule $7M, Hush Security $30M (agent identity, июль), Act Security $60M, Zenity $125M Series C (3 авг). New Market Pitch: 27 сделок AI-safety авг 2025–июл 2026, eval + risk = 70% капитала.
- **Formal/verifiable AI:** Pramaana $27M (Khosla), Sentient Arena + $42M open-source AGI program (июнь, Founders Fund/Franklin).

**Оценка:** generic «мы аудируем агентов» — красный океан Silicon Valley. **On-chain intent-escrow + публичный ledger решений + DeFi policy packs** — ещё enterable, но Nava уже сидит на этом стуле с лучшим crypto cap table. Ваш клин должен быть **не «ещё Nava»**, а либо (a) verification **после** исполнения / continuous assurance, либо (b) **агент-агент dispute + insurance hook** (сюда логично Corgi/AI-liability, но это другой рынок), либо (c) **eval harness для DeFi-агентов как dogfood вашего флота** — то, чего нет у enterprise-игроков.

Лёгкость подъёма **этому** профилю: (3) > (2) > (1), если ниша (3) заточена под on-chain и не копирует Nava. Если копирует — (2) через найм, не через seed.

### B4. Недилютивные окна H2 2026

Проверено, с дедлайнами:

| Программа | Что | Дедлайн | Комментарий |
|---|---|---|---|
| **NSF SBIR/STTR** Phase I | до ~$305k | **4 ноября 2026** (следующий слот после 27 июля уже прошёл) | [seedfund.nsf.gov/apply/full-proposal](https://seedfund.nsf.gov/apply/full-proposal) · US small business, SAM+Research.gov. Для verification/formal methods — подходящий класс |
| **EIC Accelerator** | грант < €2.5M + equity €0.5–10M; бюджет 2026 €634M | full proposals **2 сентября и 4 ноября 2026**; short proposals ongoing | [eic.ec.europa.eu/.../eic-2026-work-programme](https://eic.ec.europa.eu/eic-funding-opportunities/eic-2026-work-programme_en) · нужен EU/ассоциированный SME, TRL 6+. Dual-use открыт с 17 июня |
| **EIC Pathfinder Challenges** | до €4M | **28 октября 2026** 17:00 CEST | TRL 1–4, research teams |
| **ERC Advanced Grants** | до €2.5M / 5 лет | **27 августа 2026** — **уже вчера/сегодня**, practically closed | не под оператора |
| **Ethereum Foundation ESP** | $5k–$200k+, no equity | rolling inquiry | [esp.ethereum.foundation](https://esp.ethereum.foundation) · **не коммерческий продукт, не токен**. Formal verification / open tooling — да. «Стартап агентов» — нет |
| **EF $1M Proximity Prize / autoresearch** | кусок приза за Lean-4 proofs | live 20 авг 2026 | [x.com/ethereumfndn/status/2090485714660302877](https://x.com/ethereumfndn/status/2090485714660302877) · только если есть ZK/Lean агенты |
| Alliance / a16z CSX / Colosseum / Base Batches | $10k–$500k SAFE | rolling / batches | InnMind: Alliance $500k @ $5M post; CSX ~7%; Colosseum через Solana hackathon |

**Не нашёл в открытом виде** с рабочей страницей: отдельный US federal «AI agent safety grant» под ваш якорь с дедлайном H2 (Google DeepMind multi-agent safety до $10M, дедлайн 8 августа — **уже прошёл**, GetGrant). Hub71 Abu Dhabi / Singapore EDB — упоминаются агрегаторами, первоисточник раунда не открывал.

**Оценка:** для этого профиля самые реальные nondilutive двери — **EF ESP на open verification tooling** (если готовы отдать IP в public goods) и **NSF SBIR 4 ноября**, если есть US entity. EIC — только если юрлицо в EU и TRL 6.

---

## C. Развилка

### C1. Наняться vs своё — 12 месяцев

Считаю **ожидаемую ценность**, не романтику. Цифры компенсаций — из секций A–B; «ценность сети/опциона» — оценка.

**Путь Hire (12 мес)**  
База: FDE / Solutions Architect / GTM Engineer в Rain, Circle, OpenAI FDE-FS, Kraken agent architect, Natural.  
Cash: $180–280k (US) или £80–110k (UK GTM). Lab FDE total сильно выше, но onsite SF/NYC.  
Плюс: O-1 уже есть → friction низкий; дистрибуция в institutional stablecoin/agent payments; педигри 2027 для своего раунда; не BD-очередь Moledao 2.4×.  
Минус: офис у лаб; Kraken/Coinbase режут людей (вы можете сесть на палубу, которую пилят); equity CEX хуже seed; 12 месяцев «чужого roadmap».

**Путь Found (12 мес)**  
База: seed $3–8M на on-chain agent verification **не-копию Nava**, или yield/stablecoin API как Ground.  
Плюс: асимметрия, O-1 founder-LLC, контент/миссия №2, ваш флот = dogfood.  
Минус: 651 активный крипто-чекрайтер; без warm intro ~2%; Nava уже на стуле verification; DeFi-agent execution = hopium; 12 месяцев = фандрайз + 0–1 дизайн-партнёр, не revenue. Natural-сценарий ($30M за 193 дня) требует **fintech-педигри уровня Ivella + Forerunner**, не «15 лет в крипте».

**EV (моя оценка, не NPV-модель):**  
Hire 12м ≈ $200–400k cash+equity + опцион «второй раунд с логотипом Rain/OpenAI» > Found 12м ≈ 30–40% шанс seed $5M × (ваш кусок после dilution) × высокий шанс остаться pre-revenue.  
Для **личного runway и O-1 utilization** hire выигрывает. Для **миссии лаборатории** found выигрывает только если клин (3) уникален.

**Когда ответ переворачивается:**
1. За 8 недель есть term sheet ≥$5M от Polychain/Archetype/Bain Crypto **и** LOI от Circle/Rain/Coinbase на дизайн-партнёрство.
2. Нет ни одного FDE/GTM интервью за 6 недель (рынок вам сказал «нет»).
3. Цель — Лондон/Париж и EIC, не US cash (тогда EU-юрлицо + Accelerator 4 ноября).
4. Появляется недилютивный EF/NSF кусок ≥$150k, который **сам** делает прототип доказательством — тогда found параллельно, не вместо.
5. (Обратный переворот обратно в hire): если «своё» = ещё один DeFi-агент без escrow/данных/лицензии.

**Рекомендация [emerging]:**  
**8 недель dual-track.** Неделя 1–2: 15 точечных заявок (Kraken Solutions Architect, Rain AI eng/ops, OpenAI FDE-FS NYC, Natural, Circle Agent Stack / DevRel если живой, Taxbit, Handshake FDE). Параллельно 2-страничный клин «on-chain continuous assurance for agent wallets, не Nava-escrow» + 3 intro в Polychain/Archetype/HackVC **только если** есть demo на вашем флоте. На 8-й неделе: оффер → берёте. Term sheet без оффера → found. Ничего → не «ещё питчить DeFi agents», а GTM Engineer / FDE в non-crypto AI (White Circle, Polynom Paris, OpenAI London/Paris) как мост.

Главное допущение: **O-1 + 15 лет крипты/VC + живой флот агентов хватает, чтобы получить 3 интервью FDE/GTM за 8 недель.** Если допущение ложно — found становится default не от силы, а от закрытых дверей.

### C2. Что в выводах LobsterDAO неверно — с уликами

| Утверждение чата | Вердикт | Улика |
|---|---|---|
| AI × crypto — единственный нарратив без спада | **Направление верно, формулировка нет.** AI в JD крипты 23%→53%, но **сам crypto hiring упал**. Нарратив жив как *скилл внутри сжатого рынка*, не как рост отрасли | CryptoJobsList 6 мая; Tiger/TechFlow; Landed |
| «AI companies are hiring right now» | **Верно вне крипты, выборочно внутри** | Landed 115k; Kraken/OKX/Rain; Coinbase режет 700 |
| Стейблкоины в институциональной фазе, спрос на B2B rails | **В основном верно** [emerging] | Rain $1.95B; Circle Agent Stack; GENIUS Act; Dempsey 1 авг; Daya/Ground |
| Крипто-стартапы почти не нанимают, 90% зомби | **Преувеличение.** Протоколы — да, тощие. CEX/payments — сотни ролей (Kraken 863 на агрегаторе). Q2: 55 early раундов | web3.career; InnMind; Moledao |
| Мертво: restaking, DePIN, TON; L2 VC выгорели; мемы негатив | **Restaking/SocialFi/NFT/gaming как seed-тезис — подтверждено (~0 сделок Q2).** DePIN cooling (3 сделки). Prediction **не мертво** (XO Market $6M), но InnMind зовёт trap. L2 picks-and-shovels — настроения чата, прямого «VC запретил L2» нет. TON — [не искал глубоко, не подтверждаю] | InnMind вертикали |
| Деньги шли в Corgi $160M, Morpho $175M, Variational $50M при нулевом TVL, Neutrl $5M | **Суммы верны, интерпретация сломана.** Corgi ≠ crypto. Morpho = $11B deposits. Variational = $200B volume, не zero TVL. Neutrl seed = **апр 2025**, и сейчас протокол в ликвидном стрессе | TechCrunch Corgi; Morpho blog; CoinDesk Variational; CryptoTimes Neutrl 29 авг |
| AI-нарратив + tradfi/bigtech-родословная поднимают даже без продукта | **Частично.** Natural/Nava/Pilot — да, с оговорками. Morpho/Variational — нет, там был продукт. Corgi — продукт+ARR claim. База отказов не видна → survivorship | секция B2 |

**Систематическая ложь линзы LobsterDAO (оценка):** анти-VC европейский DeFi-чат видит *зомби-протоколы* и не видит *CEX/stablecoin/AI-lab payroll*. Отсюда «никто не нанимает» при 863 ролях у Kraken и 3 077 FDE у AI-native. Чат прав, что **DeFi-native headcount умер**; неправ, что **рынок труда на стыке умер**.

---

## X за 30 дней: что говорят практики (отдельно от веба)

Только то, что поднял с 1 августа. Верифицированные/известные vs аноны.

**Известные / high-signal:**
- **Connor Dempsey** (@Cdempsey44, stablecoins @ Rain), 1 августа, 120 514 views: поверхность крипто-компаний сжалась, хорошо платят Coinbase/Circle/Rain; AI — «sexy new thing that crypto was in 2021»; он остаётся в crypto, потому что стейблкоины «hit escape velocity», и «crypto × AI is where I'd take a swing if I were earlier in my career». Это **совпадает** с ATS (Rain нанимает AI backend). {render_inline_citation}citation_id={8}{/render_inline_citation}
- **Ash** (@ahboyash), 19 августа, 76k views: волна тихих увольнений; команда 10 делает работу 30; institutional BD забирают ex-Goldman/ex-Stripe; «most crypto companies are vapourware»; вход через банк/asset manager digital assets desk или early AI. {render_inline_citation}citation_id={10}{/render_inline_citation}
- **CryptoRecruit** (@cryptorecruit, рекрутер), 27 августа: AI infra и defense забирают тех же distributed-systems инженеров, что кормили крипту; пул не вырос, выросло число funded bidder. Совпадает с Landed/FDE.
- **Ben Yorke** (@BenYorke), 25 августа: ultra-lean crypto teams, конкурируют с Anthropic за growth generalists; web3-native маркетинг 2021-го мёртв.

**Аноны / low-authority (не опираюсь):** airdrop-треды TermiX $1M, Americanfort $8M, «physical AI data $12M» — не сверял.

**Что живёт только на X, вебом не покрыто:** скорость «тихих» увольнений августа и тезис Ash про ex-Stripe в institutional BD. Веб это не опровергает и не доказывает количественно.

---

## Развилка — именованные опции

| Опция | Что это | Trade-off | Риск |
|---|---|---|---|
| **A. FDE / Solutions Architect, US** | OpenAI FDE-FS NYC, Rain AI, Kraken Agents Architect, Natural | Макс. cash + бренд; офис/hybrid | Лабы требуют SF/NYC; CEX режут |
| **B. То же, Лондон/Париж** | OpenAI FDE London/Paris, Polynom, White Circle GTM, Kraken Finance AI London | Ближе к релокации EU; ниже вилка | EIC потом проще; US O-1 underused |
| **C. Seed «Nava-like verification»** | $5–8M Polychain-класс | Асимметрия | Nava уже там; без demo не дадут |
| **D. Seed DeFi-execution agents** | «агенты торгуют/свапают» | Быстрый нарратив в чате | Красный океан, hopium, InnMind не фондирует |
| **E. Наняться в Circle/Rain BD-stablecoin** | классический BD | Ваш 15y BD | Moledao 2.4×, ex-Goldman займут |
| **F. Dual-track 8 недель** | A или B + узкий клин C на вашем флоте | Лучший EV | Требует дисциплины не слить оба |

**Одна рекомендация:** **F, с приоритетом A.** Confidence **[emerging]**. Допущение: три интервью FDE/GTM за 8 недель. Если к 26 октября 2026 нет оффера и нет term sheet — пересмотр, не «ещё один питч агентов».

---

## Три вещи, которые я НЕ смог подтвердить

1. **Единую медиану компенсации crypto vs AI за июль–август 2026** в одной методологии. Landed = AI-native ATS; CryptoJobsList = web3 JD + опрос; Gate = white paper. Склеивать $186k и $115k — ложь.
2. **Именованный список работодателей, которые в JD 2026 явно пишут «O-1 accepted».** Политика USCIS ясна; практика конкретных ATS — нет.
3. **Базу отказов seed «AI-обёртка + педигри».** Видны только победители. Нет публичного deny-rate Polychain/a16z за июнь–август.
4. Дополнительно: не пересчитывал ваши 17 865 сообщений LobsterDAO; не открыл живой DefiLlama TVL Variational (объём взял из CoinDesk/FinanceFeeds); не верифицировал self-reported 250k агентов Pilot.

---

## Поиски и открытые страницы

**Запросы (web):** crypto vs AI job market hiring 2026; cryptocurrency jobs hiring freeze 2026 vs AI; AI crypto fintech jobs agent infrastructure stablecoin; crypto seed funding June–August 2026 AI agents; Corgi $160M Morpho $175M Variational $50M Neutrl $5M; Morpho $175M; Variational $50M Series A; Neutrl $5M; O-1 visa sponsorship 2026 crypto AI; AI agent verification audit startup funding 2026; Nava $8.3M; Cambrian $6M; crypto grants 2026 Ethereum Foundation EIC NSF SBIR; forward deployed engineer 2026; Circle jobs Agent Stack; Ground $3.6M; Natural $30M; Colin Jarvis / Gary Lin OpenAI FDE.

**X:** `crypto hiring freeze OR "crypto jobs" AI agents since:2026-08-01`; `("seed round") (crypto OR web3) (AI OR agent) since:2026-07-01`; semantic «crypto job market hiring 2026 vs AI seed funding» с 1 августа.

**Страницы, которые реально открыл (не только сниппет):**  
Landed State of AI-Native Jobs; CryptoJobsList Agent Manager Era; TechFlow Q2 Web3 Recruitment; InnMind Q2 web3 seed report; Kraken AI Agents Solutions Architect JD; Morpho Association $175M blog; TechCrunch Corgi $160M; Natural Series A blog; EIC 2026 work programme; NSF SBIR full proposal deadlines.  
The Block Variational и startup.jobs OpenAI FDE — Cloudflare/challenge, оперся на поисковые сниппеты + вторичные (CoinDesk, FinanceFeeds). Tiger Research H1 — paywall, цифры через TechFlow/Woofun/FNNews. Nava official — Vercel checkpoint.

**Первичные URL для чисел в отчёте (минимум 25):**  
landed.jobs/reports/state-of-ai-native-jobs · cryptojobslist.com/blog/web3-hiring-enters-the-agent-manager-era-ai · techflowpost.com/en-US/article/33283 · blog.innmind.com/web3-startup-funding-in-q2-2026-... · morpho.org/blog/morpho-association-raises-175m-... · techcrunch.com/2026/05/06/insurance-startup-corgi-... · natural.com/blog/natural-series-a · cryptojobslist.com/jobs/ai-agents-solutions-architect-hr-... · web3.career/ai-jobs · web3.career/web3-companies/kraken+ai · startup.jobs/backend-engineer-ai-rain-xyz-7885996 · theblock.co/post/406028 (Cambrian) · theblock.co/news/deals/2026-06-24-...ground... · coindesk.com/.../variational...50-million... · coindesk.com/.../paradigm-launches-usd1-2-billion-ai-fund · coindesk.com/.../world-network-secures-fresh-funding · eic.ec.europa.eu/eic-funding-opportunities/eic-2026-work-programme_en · seedfund.nsf.gov/apply/full-proposal · talvisa.com/blog/o1-visa-sponsor-requirements · forbes.com/sites/bryanrobinson/2026/06/29/ai-hiring-is-booming... · siliconangle.com/.../corgi-raises-160m... · citybiz.co/.../pilot-protocol... · bwdisrupt.com/.../pramaana-labs-raises-27-mn... · techfundingnews.com/mindgard-raises-30m... · x.com/Cdempsey44 (1 авг) · x.com/ahboyash (19 авг) · x.com/ethereumfndn (20 авг, $1M prize)

---

## Confidence map

| Тезис | Тир |
|---|---|
| Крипта режет, AI нанимает | **[established]** |
| 53% crypto JD требуют AI | **[emerging]** (два вендора, одна цифра 53.1%) |
| FDE = главная дверь без ML-research | **[established]** как спрос; **[emerging]** как fit именно вам |
| Медиана web3 seed $5.2M Q2 | **[emerging]** [single-source InnMind/DeFiLlama] |
| Variational ≠ zero product | **[established]** |
| Corgi ≠ crypto | **[established]** |
| Neutrl seed не из H2 2026 + сейчас стресс | **[established]** |
| «AI wrapper поднимает всегда» | **[speculative]** без базы отказов |
| Dual-track hire-first для этого профиля | **[emerging]** (моя оценка) |

---

🧒 **Простыми словами:**  
Представь два магазина. В крипто-магазине почти никого не берут на кассу, но охранников и бухгалтеров ещё ищут. В AI-магазине очередь за людьми, которые умеют запустить робота у клиента в офисе — не придумать робота, а чтобы он работал. Ты как раз такой: умеешь и роботов собирать, и людям продавать. Сначала иди работать туда, где платят (Rain, Circle, OpenAI, Kraken). Свой киоск «робот для DeFi» сейчас как открыть третью шаурму на улице, где уже две. Свой киоск «проверять, не ворует ли чужой робот» — ближе, но место у входа уже занял Nava. Восемь недель попробуй оба: работу и узкую проверку. Кто первый даст бумажку с деньгами — туда.
