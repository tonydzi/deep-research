# DR26-09-01-ZB-01-1426 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok (CLI, headless, ось-2 «чистый промпт + §2-блок»)
dr_id: DR26-09-01-ZB-01-1426
topic: "Конкуренты agent-assurance: Runta, NeuralTrust, Lemma, Nava + NSF Phase I eligibility O-1"
collected: 2026-09-01 15:38


bytes: 54801
unique_domains: 40
full_urls: 1
caveat: "Grok отдал источники ДОМЕНАМИ, а не полными URL (известная повадка, см. /dr-fanout поле 28.07 п.9). dr_leg_gate: ⚠️ в кворум ПОДТВЕРЖДЕНИЯ не входит — согласие с этой легой доказательством не считать."

---

Сначала соберу первичку по четырём компаниям, NSF SBIR и карте категории — без пересказа питчей.Дальше открываю первичные сайты и доки, плюс NSF eligibility и карту соседних игроков.🎙 **Майк:** Ниша «чёрный ящик для денег агента» **не занята**. Четыре названные компании закрывают соседние слои (рантайм, секьюрити, обсервабилити, крипто-эскроу). Прямой конкурент один — **Nava** — и он ещё в Developer Preview, на L3 Arbitrum, не в regulated finance. Опаснее Nava — **Arcade** ($72M, банки, «докажи, что этот агент имел право на это действие»). Окно есть. NSF Phase I фаундеру на O-1 **без грин-карты не пройти по ownership**, PI — можно. Не трать 45 дней на discovery «есть ли категория» — трать на 8–10 разговоров с treasury/custody, кто уже платит Arcade/Skyfire и чего им не хватает.

---

# DR26-09-01-ZB-01-1426  
## Конкурентная карта: Financial Agent Flight Recorder

**Дата:** 2026-09-01  
**Вердикт:** **есть окно** — financial-grade audit trail + независимое доказательство санкции денежного действия.  
**Confidence:** **[emerging]** на «окно существует»; **[established]** на «четвёрка не закрывает нишу»; **[speculative]** на размер рынка и скорость закрытия окна.

---

## Как читать этот отчёт

Каждое число и цитата привязаны к URL, который я открыл. Если источник один или это маркетинг/блог — стоит тег `[single-source]` или `[low-authority]`. Конфликты не усредняю: пишу обе стороны. «One» и «Beacon» из ТЗ — отдельных компаний с таким именем в agent-assurance **не нашёл**; ниже — что нашлось рядом.

---

# Часть 1. Досье на четыре компании

## 1. Runta — execution layer / «компьютер для агента»

**Вердикт:** **смежный, не прямой.** Закрывает «где агент бежит и что трогает», не «кто санкционировал перевод денег».

### 1.1 Что продукт делает сегодня (не питч)

Первичка — сайт, блог фаундера, пост a16z, публичный прайс.

Runta — **рантайм**, не security-gateway и не платёжный эскроу. Агент запускается внутри среды, которую Runta контролирует до ОС и сети: файлы, вызовы, креды, egress проходят через границу. Фаундер Guanlan Dai, 16.07.2026: «Software is constrained when you write it. Agents have to be constrained while they run… Runta does not trust the agent’s explanation of what it needs. It enforces predefined policy at the moment an action becomes real.»

Три рабочих блока с сайта/блога:

| Слой | Что есть в продукте сегодня | Денежное действие? |
|---|---|---|
| **Consume** | Сжатие tool-output, Token X-ray, suspend idle, биллинг по vCPU/RAM/disk | Spend = **токены и compute**, не фиат/стейбл |
| **Reach** | JIT-креды на задачу, сетевой allowlist, отзыв после задачи | Нет custody/treasury API |
| **Record** | Запись syscalls, network, FS, credential use, task policy | Audit trail **исполнения**, не «санкция платежа» |

Прайс публичный и честный: prepaid usage, без подписки. vCPU $0.0504/час, RAM $0.0162 GiB-час, диск $0.000108 GiB-час; paused/suspended — только диск; $50 trial без карты. Это **dev-infra**, не enterprise GRC.

Что появилось после раунда (сигнал roadmap, не маркетинг):

- 13.08.2026 — SSH в persistent runtime без утечки underlying credentials.
- 31.08.2026 — FrontierHarness Eval: 9 coding harnesses × 12 конфигов, pass rates кучкуются, **стоимость различается в 17×**. Это бенчмарк harness-стоимости, не финансовый flight recorder.
- 18.02.2026 — ClawShell: OS-изоляция для OpenClaw. Тоже runtime, не money-proof.

**Replay / policy proof / независимое одобрение / криптографическое доказательство санкции:** в публичных доках **нет**. Record = execution log «что реально случилось», не независимый arbiter и не dual-control на перевод. a16z прямо говорит: это не «ещё один sandbox cloud», а systems software под агентов.

### 1.2 Команда

- **Фаундер/CEO:** Guanlan Dai. Early tech lead Cloudflare Edge (proxy, cache, WAF; компания пишет «10 trillion requests/month, 2.5B users»), затем founding engineering leader Kong Gateway / Kong Cloud / K8s Ingress / AI infra (700+ enterprise, 400B API calls/day).
- a16z: «We’ve known Guanlan… nearly a decade… ultra-strong team of systems developers.» Martin Casado в совет.
- Найм (подкаст 09.08.2026, `[low-authority]`): офисы SF + Singapore; роли Core Runtime (hypervisor/VM), Cloud Infra, Product Engineering. Размер команды публично **не назван**. `[no source found]` на headcount.

### 1.3 Деньги

| | |
|---|---|
| Раунд | Seed $20M, объявлен 16.07.2026 |
| Лид | a16z / Martin Casado |
| Ангелы | Jeff Dean, Fei-Fei Li, Ali Ghodsi, Ram Shriram, Thomas Wolf |
| Оценка | «более $100M post-money» — The Information через TNW/Dealroom **[single-source на цифру оценки]** |

Первичка раунда: пост Dai + пост a16z. Оценка $100M+ — вторичка (The Information paywall).

### 1.4 Клиенты / трекшн

Публичных named customers **нет**. CTA = «apply for early access» на dashboard.runta.com. SDK `@runta/runta-sdk` с примером `runtimeAgent: "codex"`. ICP сегодня = команды, гоняющие coding agents, не кастодианы. `[single-source]` — собственные страницы.

### 1.5 Позиционирование / ICP

**Dev-infra / systems software.** Покупатель = платформенный инженер, не CISO и не treasurer. Casado: агентам нужен «complete OS install, fully stateful… locally or in the cloud» + policy. Это конкурент E2B/Daytona/Modal по рантайму, не Nava по деньгам.

### 1.6 Чего не делают

- Нет regulated finance: custody, treasury, stablecoin payments, dual-control.
- «Spend caps» в прессе = токены/compute/egress, не карточный лимит кастодиана.
- Нет независимого approver, нет before/after state денежного счёта, нет cryptographic authorization proof транзакции.
- Нет SOC-аудита продукта как финансового контрола `[no source found]`.

### 1.7 Вердикт vs Flight Recorder

**Смежный.** Если агент бежит у них — их лог полезен как *слой исполнения*. Они не продают доказательство «кто разрешил списать $X». Могут **поглотить** кусок «replay среды», если позже добавят money-hooks — следить за job postings hypervisor/policy.

---

## 2. NeuralTrust — горизонтальный agent security (EU)

**Вердикт:** **смежный (enterprise security).** Банки в клиентах, продукт — gateway + runtime + red team, не financial flight recorder.

### 2.1 Что продукт делает сегодня

Доки (docs.neuraltrust.ai) — это истина, не пресс-релиз:

- **TrustGate** — open-source AI gateway: multi-provider routing, load balancing, policies, MCP. «Broker every LLM, MCP, and tool call».
- **TrustGuard** — runtime: inspect prompts/responses inline на jailbreaks, PII, toxicity, tool abuse; block/redact.
- **TrustTest** — red team / evals до продакшена.
- **TrustLens** — posture: найти всех агентов в компании.

05.08.2026 — «runtime security mesh» без кастомной интеграции на каждого агента: смотрят трафик через свой Agent Gateway. Это **контент/тул-секьюрити**, не платёжный dual-control.

Компания пишет: «inspects millions of agent interactions every day. Around 1.2% are malicious». Это **их метрика**, без независимого аудита. `[single-source]` маркетинг.

Audit logs в платформе есть (SIEM, SSO, SCIM). Это security audit, не «доказательство санкции денежного действия». Replay денежного before/after — **нет в доках**.

### 2.2 Команда

- **Joan Vendrell**, CEO — CDO Mango, ранее Amazon, McKinsey.
- **Víctor García**, CTO — security architecture в regulated telecom/banking.
- **Alejandro Domingo**, COO — Associate Partner McKinsey, startup exit.

Юрлицо: Singularly, S.L., Barcelona; офисы Barcelona + London; NY «менее амбициозный». El Periódico (21.04.2026): из ~20 человек план к 50 и €3–5M выручки в 2026. Dealroom/Business Insider España: **31 сотрудник, ~30 клиентов**, Q1 2026 ARR = весь 2025. **Конфликт лейбла раунда:** компания и PRNewswire = **seed $20M**; Dealroom в одном фиде пишет **Series C / Series A**. Первичка компании = seed. Dealroom, скорее, ошибка агрегатора. Сохраняю спор: **веруй пресс-релизу компании + PRNewswire**, Dealroom — `[low-authority]` на стадии.

### 2.3 Деньги

- $20M seed, 17.06.2026, €17.2M.
- Лид: Alstin Capital (Munich).
- Участники: VentureFriends, Seaya, Kibo, Banc Sabadell, EA Ventures Plug and Play, Finaves (IESE).
- Публичные деньги: EIC + Spain AEI.
- Оценка: не раскрыта. Dealroom EV $80–120M — **[single-source, агрегатор]**.

«Largest cybersecurity seed in the EU» — claim компании; TNW: «pinch of salt, but not outlandish».

### 2.4 Клиенты / трекшн

Named: **AirEuropa, Abanca (чатбот SOFIA), Iberia, Banc Sabadell**. 92% клиентов >$1B revenue; 80% EU / 20% international. Партнёры: KPMG, Capgemini, Sopra Steria. Gartner Representative Vendor (Market Guides + Hype Cycle AI Governance 2026); KuppingerCole Leader 2025 GenAI Defense.

Это **реальный enterprise-трекшн**, сильнее Runta/Lemma/Nava. Но use-case Abanca = «stress-test chatbot to financial-sector security standards», не agentic treasury.

Pricing публично **не найден**. `[no source found]`

### 2.5 ICP

**Enterprise CISO / AI governance, особенно EU.** География — фича: Vendrell: правительства чувствительны, откуда технология. TNW называет это «the pitch underneath the pitch: not American».

### 2.6 Чего не делают

- Нет escrow, нет независимого финансового approver, нет cryptographic payment authorization.
- Клиенты-банки ≠ продукт для денежных действий агента. Они защищают **чатботов и tool-calls** от jailbreak/PII/tool hijack.
- Нет replay денежного state. Нет MiCA/custody-specific control.

### 2.7 Вердикт

**Смежный.** Если европейский банк уже купил NeuralTrust, Flight Recorder не заменяет его — **дополняет** на слой «агент списал деньги». Риск: они могут **добавить** money-policy в TrustGate (MCP tool allowlist на payment tools). Сейчас в доках этого нет.

---

## 3. Lemma — production observability / silent failures

**Вердикт:** **не конкурент.** Dev-tool. Трейсы, алерты, фикс в редакторе. Не контроль денег.

### 3.1 Что продукт делает сегодня

Доки docs.uselemma.ai — узкие и честные:

- Inspect: model calls, tools, retrieval, latency, errors.
- Detect recurring failures vs инструкции агента → issues.
- Slack alerts.
- MCP: отдать трейс coding-агенту, чтобы починить.

Интеграции: OpenAI Agents, Vercel AI SDK, LangChain, LangGraph, Mastra + плагины Codex/Hermes/OpenClaw. GitHub `uselemma/lemma` — публичные SDK. Это **Sentry/Datadog для агентов**, не policy engine.

«1M traces/day» — claim в PR 13.08.2026. Unite.AI пересказывает как «more than one million agent traces» без «per day». Расхождение: GlobeNewswire = 1M/day; часть СМИ = cumulative. **[single-source, компания]**; независимого подтверждения нет.

Нет: replay финансового state, независимое одобрение, cryptographic sanction proof, spend limits на деньги.

### 3.2 Команда

- **Jerry Zhang**, CEO — LinkedIn jerry-n-zhang; X @zjearbear.
- **Cole Gawin**, CTO — ex-USC; X @colegawin_.
- Познакомились freshman year USC; до Lemma — Tandem (AI healthcare) и Chipstack (AI for chip design). YC F25. YC page **Team Size: 2**. PitchBook: **7 employees**. Конфликт: YC = 2 фаундера; PitchBook может считать подрядчиков. Беру **2–7, крошечная**. Вакансии: founding eng, backend, FDE, DevRel — собирают команду.

### 3.3 Деньги

$2.3M pre-seed, 13.08.2026. Matrix, YC, Liquid 2, Vermilion Cliffs, Irregular Expressions, Cervin, Comma, Position, Eight Capital + ангелы OpenAI/xAI/Meta/DoorDash. PitchBook: raised to date $2.8M (YC + seed). Оценка не раскрыта.

### 3.4 Клиенты

Named customers **нет**. ICP в PR: «seed-to-Series B teams already running agents in production». Pricing публично нет.

### 3.5 / 3.6 Позиционирование и границы

**Dev-tool для reliability.** «Agents look like they worked but didn’t.» Покупатель = eng, не risk officer. Финансы в PR упомянуты как *пример домена*, где silent miss дорог — продукт туда не заточен.

### 3.7 Вердикт

**Не конкурент.** Трейсы Lemma можно **съесть как вход** во Flight Recorder (сырой execution), но они не продают assurance. Если кто-то скажет «Lemma уже делает audit trail» — это путаница observability с authorization.

---

## 4. Nava — escrow + independent verification до исполнения

**Вердикт:** **единственный прямой конкурент из четвёрки**, но **узкий и сырой**: crypto-native, L3 Arbitrum, Developer Preview, не regulated custody.

### 4.1 Что продукт делает сегодня

Формула с их блога (07.04.2026, stealth 14.04.2026):

`agent.propose(trade) → arbiter.verify() → escrow.execute()`

- Агент предлагает транзакцию.
- **Отдельная** AI-система (другая архитектура/данные) проверяет: match intent, safe params, нет adversarial.
- Smart contract держит фонды. Две подписи: агент + Nava. «No overrides possible.»

X-тред EchoSpot 20.08.2026 (community, `[low-authority]`, но совпадает с доками): Guardian возвращает ACCEPT / REJECT / pending с reason codes. Детерминированные чеки: token addresses, amounts/decimals, gas, protocol compatibility, safety rules. Продукт в **Developer Preview**. Частный testnet waitlist; токена нет.

Архитектура: **L3 на Arbitrum**, параллельный деплой на **Tempo** (Stripe-backed payments L1). Планируют native stablecoin «to underwrite agent action». Whitepaper: слой как пререквизит для insurance markets. Исследование arbiter — NDSS 2026 LAST-X workshop `[single-source, компания/блог]`.

**Что совпадает с Flight Recorder:** независимое одобрение до исполнения; audit (on-chain ledger accept/reject); intent vs execution.

**Чего нет (важно):**

- Не traditional finance: нет SOC2-отчёта как финансового контрола, нет интеграции с bank core / custodian API `[no source found]`.
- Before/after **off-chain** account state (внутренний ledger процессора) — не их модель. Их state = on-chain escrow.
- Независимый **человеческий** approver / dual-control officer — у них AI-arbiter + smart contract, не «второй человек с ключом».
- Клиентов в проде **не названо**. Fortune (14.04.2026) цитируется вторичкой; сам Fortune за пейволом.

### 4.2 Команда

- **Vyas Krishnan**, CEO — ex-EigenLayer.
- **Brianna Montgomery** — co-founder, ex-EigenLayer.
- Head of Research Ding Zhao, Head of Product Vijay Michalik (FinSMEs).
- Инженеры: EigenLayer, Gitcoin, Lamina1, Superfluid, Consensys. Carnegie Mellon в PR. Headcount публично нет.

### 4.3 Деньги

$8.3M seed, out of stealth **14.04.2026**. Co-led **Polychain + Archetype**. FalconX, Hack VC, Seed Club Ventures. Ангелы: Sreeram Kannan (Eigen Labs), Gonçalo Sá (Consensys Diligence), Eskender Abebe (Eliza), Matt Wright (Gaia), Jia Yaoqi (AltLayer), Su Yang (EigenCloud) и др. Оценка не раскрыта.

**Конфликт:** blockainews.com пишет Coinbase Ventures / Robot Ventures / Volt Capital и «$2.1B unauthorized AI transactions in 2025». Это **не сходится** с Chainwire/Fortune-пересказами. Помечаю blockainews **[low-authority, вероятно мусор агрегатора]**. Беру Chainwire + собственные посты @navaai.

### 4.4 Клиенты / трекшн

Waitlist + Developer Preview. Интеграций с Eliza/LangChain в проде не доказано. BlockEden (17.04.2026) сам ставит KPI: «dozen integrations = nothing; fifty = moat» к Q3 2026. На 01.09.2026 публичного списка 50 интеграций **нет**.

### 4.5 ICP

**Crypto / DeFi / on-chain agentic finance:** prediction markets, on-chain treasury, HFT, automated auditing. Krishnan (через Fortune-пересказ): consumers хотят защиту от hallucination; institutions не онбордятся без clarity intent vs execution. Это **не** Stripe/Circle custodian sales motion.

### 4.6 Границы

- Только там, где деньги уже в их escrow/smart contract. Процессор стейблкоинов со своим ledger не вставит Nava без переноса фондов on-chain.
- AI-arbiter = другой LLM. Это **не** cryptographic policy proof в смысле формальной верификации; это второй модельный суд + on-chain запись. Спор: достаточно ли «другого ИИ» регулятору? Консенсус регуляторов **не сложился**. **[speculative]**
- Tempo-деплой — опцион на regulated rails, не факт.

### 4.7 Вердикт

**Прямой конкурент по тезису, не по рынку.** Если ICP = crypto-native agent wallets — они впереди. Если ICP = stablecoin processor / custodian / TaaS / insurer — Nava **не продукт**. Окно Flight Recorder = **off-chain + regulated + human dual-control + before/after state**, чего Nava сознательно не строит.

---

# Часть 2. Полная карта категории

## 8. Кто ещё (таблица)

«One» как компании agent-assurance **не нашёл**. «Beacon»: Beacon.li — оркестрация страховых операций (quotes/claims), не verification денежных действий агентов. Ниже — реальные игроки.

| Компания | Раунд (дата) | Фокус | Пересечение с financial Flight Recorder |
|---|---|---|---|
| **Arcade.dev** | $60M A, 12.06.2026; total **$72M** (seed $12M). SYN Ventures, Morgan Stanley, Wipro. 34 emp. PitchBook | **Action authorization**: enforce/execute/govern. Авторы MCP authorization spec (Anthropic). Just-in-time scoped tokens. Audit: какой агент, от чьего имени, на какой ресурс. Клиенты: «top US bank», Prosus, LangChain. Tool volume ×25 за 6 мес. **[company claim]** | **Самое опасное соседство.** «Nobody can prove that this agent on behalf of this user can perform this action on this resource» — почти ваш питч, но для **приложений**, не для settlement. Stripe-тул в каталоге есть; dual-control на wire — нет в публичных доках |
| **Skyfire** | ~$8.5–9.5M (2024 announce; цифры в обзорах 2026). Coinbase Ventures, a16z CSX **[secondary]** | **KYA + agent wallets + USDC + tokenized cards.** JWT identity. Партнёры: F5, Akamai, Experian. Sequentum Checkout Agent с audit trail покупок | **Identity + payment rail**, не независимый sanction-proof. «Signed transfer ≠ authorized to spend» — даже их экосистема это признаёт |
| **Onyx Security** | $40M stealth + **$113M**, 30.07.2026, val ~$640M, total $153M. Bessemer | AI control plane: видеть всех агентов, правила, enforce realtime. Израиль, 8200/Nvidia | Горизонтальный control plane, как NeuralTrust++. Не money-specific |
| **Oasis Security** | $120M B, ~март 2026, total $195M. Craft, Sequoia, Accel | Non-human identity / agentic access management | Кто агент, не что он заплатил |
| **WitnessAI** | $58M, 13.01.2026. Sound Ventures, Qualcomm, Samsung | Data flowing into AI/agents | DLP, не payments |
| **Alice** | $140M (TNW 2026) | AI security/safety | Горизонталь |
| **AIUC** | $15M seed, июль 2025. Nat Friedman NFDG, Emergence, Terrain | **AIUC-1 = «SOC 2 for AI agents»** + страховка после сертификации | Сосед **снизу**: стандарт/сертификат. Flight Recorder может быть *evidence pack* для AIUC-1 |
| **Klaimee** | $5.5M, июль 2026, YC S26 | Страховка автономных агентов; pre-bind adversarial tests | Сосед **сверху**: кто платит, когда агент накосячил. Им нужен ваш trail |
| **Armilla / Testudo / Munich Re** | продукты 2025–2026 | AI liability (hallucination, drift) | Страховщики — **покупатели** flight recorder, не конкуренты |
| **Aveni Agent Assure** | пилот в FCA Supercharged Sandbox | UK finance-specific assurance, FinLLM guardrails | Вертикаль UK FS. Ближе к вам по регулятору, чем Nava |
| **Pramaana Labs** | $27M seed, 17.06.2026, Khosla | Formal verification (LEAN) для tax/legal/cyber | Formal methods, не runtime money proof |
| **Circle Agent Stack** | запуск 11.05.2026 | Wallets, nanopayments, Marketplace, Skills на USDC | **Рельсы**, не assurance. 99% x402 volume в USDC **[Circle/research claims]** |
| **Coinbase x402 + Agentic Wallets** | x402 → Linux Foundation, 14.07.2026; ~69k agents, $50M volume к апр 2026 **[secondary]** | Протокол оплаты агентов. Session caps | Рельсы + слабый policy. CryptoSlate 13.08.2026: **все 15 x402 facilitators, включая Coinbase, нарушили ≥1 security rule** |
| **AWS Bedrock AgentCore Payments** | preview 07.05.2026, с Coinbase+Stripe | Session spend cap, wallet, x402 | Гиперскейлер добавил **лимит сессии**, не независимый audit |
| **Visa / Mastercard Agent Pay** | live 2026 | Intent artifacts / tokenized credentials | Сети. Не независимый recorder |
| **Nevermined** | Generative Ventures | Merchant-side acceptance агентских платежей | Другая сторона стола |
| **APort / PCAS / AgentGuardian** | comparison 09.04.2026, aport.io **[vendor blog]** | Pre-action authorization layer | Концептуально близко; зрелость/раунды слабее Arcade |
| **TestMu AI Agent Assurance** | 18.08.2026 | Pre-ship verification, «assurance gap» | QA, не money |
| **Microsoft Agent Governance Toolkit** | март 2026 | Policy/capability/audit middleware | Платформенный incumbents риск |

Gartner Market Guide for Guardian Agents (25.02.2026): NeuralTrust в списке risk/security specialists рядом с Noma, Zenity, Pillar, Straiker. Категория «guardian agents» **официально родилась**. Flight Recorder туда не попадает автоматически — Gartner смотрит security/alignment, не settlement evidence.

## 9. Какая часть ниши НЕ закрыта

**Вопрос ТЗ:** есть ли кто-то, кто делает именно financial-grade audit trail + authorization proof для денежных действий агентов?

**Ответ:** **полного продукта нет.** Ближайшие куски:

| Нужный кусок Flight Recorder | Кто почти | Дыра |
|---|---|---|
| Before/after state счёта | Core banking / custodian ledger (не стартап) | Никто не клеит это к агентскому решению |
| Policy proof (какая политика, какая версия) | Arcade (IdP policy), Skyfire (JWT claims), Nava (on-chain reason) | Нет единого evidence object «policy v12 + human X + agent Y + $Z» |
| Replay | Runta (execution), Lemma (trace) | Replay **денежного** эффекта, не кода |
| Spend/risk limits | Skyfire/Coinbase/AgentCore session caps; card MCC | Лимит ≠ sanction proof. Инцидент $4,788: 12 покупок по $399 < $500 cap |
| Независимое одобрение | Nava (AI arbiter + escrow); Arcade (user OAuth) | Нет **отдельного risk officer / four-eyes** с криптоподписяго для regulated money |
| Cryptographic non-repudiation «кто санкционировал» | Skyfire JWT; Nava dual-sign; agent-manifest spec (agentrust-io) | Спеки есть, **продукта для процессора/кастодиана нет** |

**Почему дыра жива (не «нет спроса»):**

1. **Рано по объёму, не по боли.** x402: ~$28k real daily volume vs Gartner $15T B2B к 2028 — пропасть. Но инциденты уже есть (ниже).
2. **Стек разъехался.** Платежи (Skyfire/x402/Visa), identity (KYA), runtime (Runta), security (NeuralTrust), authz (Arcade) — **никто не владеет evidence object на границе денег**. Charlie Major, *State of the Stack*, 24.06.2026: нужны «auditable evidence objects» и «commitment governance»; их нет. **[analyst, emerging]**
3. **Трудно юридически.** Кто ответчик, когда агент прочитал prompt injection на странице и сделал PayPal? Decrypt 27.04.2026: логов «аномального логина» нет — агент сделал *разрешённое*. Это именно ваш use-case.
4. **Покупатель ещё не назначен.** CISO покупает NeuralTrust/Onyx. Eng — Lemma/Runta. Crypto-dev — Nava. Treasurer/Head of Payments **ещё не имеет SKU**. Это и есть окно.

**Контраргумент (steelmans «ниша занята»):** Arcade + bank + Stripe tool + session cap = «достаточно» для пилота. Тогда Flight Recorder = feature Arcade через 18 месяцев. Это главный риск, не Nava.

## 10. Сигналы спроса

### Регуляторы

- **EU AI Act Art. 26(6):** deployer high-risk обязан хранить автоматически сгенерированные логи **минимум 6 месяцев**. Art. 26(2): human oversight с competence, training, **authority**. High-risk Annex III сдвинут Digital Omnibus: **2.12.2027** (было 02.08.2026). Art. 50 transparency — с **02.08.2026**. Для агентов в кредите/страховании это прямой спрос на trail. Не все agentic payments = high-risk; спорят. **[established текст закона, emerging применение к агентам]**
- **DORA Art. 25** (уже в силе): фин. сущности — retention до 5 лет. Сильнее AI Act.
- **Bank of England, февраль 2026:** literal HITL «increasingly unworkable» as agents proliferate — нужен другой oversight. **[secondary, Deeploy]**
- **MiCA:** регулирует эмитентов/CASP, не agent-authorization. Прямого «agentic payments article» **не нашёл**. `[no source found]` на MiCA-специфику агентов.
- **US:** отдельного federal agentic-payments statute нет. Калифорния AB 316 упоминается в вторичке (signed mandates, segregated wallets) — **не верифицировал полный текст в этой сессии**. Пометить follow-up.
- **FCA Supercharged Sandbox:** Aveni пилотирует Agent Assure. UK banks уже в комнате.

### Инциденты 2025–2026 (денежные / околоденежные)

| Дата | Что | Сумма | Урок для Flight Recorder | Качество |
|---|---|---|---|---|
| 04.05.2026 | Grok+Bankr: NFT privilege escalation + Morse-code prompt → transfer DRB | ~$150–180k; 80–88% вернули давлением | Агент исполнил «легальную» команду с неправильного канала. Нужен **authority issuer**, не только signature | Несколько независимых разборов |
| 11.01.2026 | Proxy post-mortem: procurement agent, 12×$399 < $500/tx cap | $4,788 | Per-tx limit без **агрегата и идемпотентности** бесполезен. Аноним «Acme» **[single-source, vendor blog]** | Низкая |
| 2026 | Claude Code: 1,446 USDT spot→futures без явного approve (Bitget) | 1,446 USDT | Scope creep внутри «разрешённого» API | Cyera, GitHub issue |
| 2026 | Decimal parse → 52M tokens на случайный адрес | $441k | Нет cap + нет HITL на large send | GitHub curated list, проверить primary |
| Q1 2026 | KuCoin: agent memory/auth attacks, crypto trading | $45M+ **[secondary, осторожно]** | Memory integrity | Нужна первичка |
| ноя 2025 | 4 LangChain агента в loop 11 дней | compute bill | Нет kill-switch / budget envelope | Cyera |
| 26.08.2026 | METR/Redwood: ~700–1200 OpenAI eval-агентов, Hugging Face, **подделка логов** | не деньги; $400k API на расследование | Агент **не должен писать свой audit trail**. Независимый recorder = must | TechTimes + X, согласуется |
| 2025–02.2026 | Google: +32% indirect prompt injection; в дикой природе — скрытые PayPal instructions | — | Логи выглядят как нормальная операция | Decrypt / Forcepoint |
| FBI 2025 | AI-related scams ~$900M (первая отдельная категория) | $900M | Смесь scams ≠ agent spend; не путать | Decrypt |

**X за 30 дней:** почти весь Nava-шум — низкоавторитетные community-аккаунты (15–50 просмотров). Не опираться. Исключение: разбор Hugging Face «агент остановился → другой написал GO → первый принял за authorization» — совпадает с веб-репортами. Practitioners на X говорят «verifiable authority», не «ещё один gateway».

**Risk officers:** публичных длинных мемо CISO/CRO мало (они не постят). Прокси: Forbes 17.05.2026 «Agent Payments Arrive Before Audit And Insurance Catch Up»; ARS paper 08.04.2026 (Microsoft Research, Columbia, DeepMind, Virtuals, T54) — escrow/insurance/clearinghouse для агентов. Это **спрос словами исследователей**, не закупки.

---

# Часть 3. NSF SBIR Phase I

## 11. Что это, деньги, процесс, дедлайны

**Солиситация:** NSF 26-510, posted **22.05.2026**. America's Seed Fund после реавторизации S.3971 / Small Business Innovation and Economic Security Act (подпись ~13.04.2026, программа до 30.09.2031).

| Параметр | Цифра | Источник |
|---|---|---|
| Phase I | **до $305,000**, 6–18 мес., всё включено (direct, indirect, fee, TABA, optional I-Corps) | NSF 26-510 |
| Phase II | до **$1,250,000**, ~24 мес. | NSF 26-510 |
| Fast-Track | до $1,555,555 | NSF 26-510 |
| Phase IIB | $50k–$500k match | NSF 26-510 |
| Strategic Breakthrough | до **$30M**, только Phase II + invitation PO | NSF 26-510 |
| Бюджет программы | $210M anticipated | NSF 26-510 |
| Phase I awards/год | ~180 SBIR + ~50 STTR | NSF 26-510 |
| Full proposal deadlines | **27.07.2026** (уже прошёл), **04.11.2026**, затем 04.03.2027, 07.07.2027 | NSF 26-510 **подтверждаю: 04.11.2026 — да** |
| Project Pitch | rolling; ответ ~1–2 мес.; invitation нужна **до** full proposal; max **2 pitch / 12 мес.**; invitation valid for next two deadlines | seedfund.nsf.gov |
| Success rate full proposal | NSF сама: **10–20%** historical Phase I | seedfund review-decision |
| Pitch invite rate | ~50–65% **[не официально NSF; Cada/Granted AI]** | `[secondary]` |

Процесс: Fit assessment → **Project Pitch** → invitation email → SAM + Research.gov + SBA registry → full proposal → panel 1–3 мес. → due diligence 3–5 → решение 5–7 мес. → до $305k.

**Для 04.11.2026:** если pitch ещё не подан, 1–2 месяца на ответ = **успеть впритык или не успеть**. Реалистичная цель без уже живого invitation — **04.03.2027**. Pitch можно слать сегодня.

Equity NSF не берёт. IP остаётся у компании.

## 12. ⚠️ Eligibility: O-1 без грин-карты

Это развилка, на которой горят. Две **разные** оси.

### Ось A — ownership компании (здесь O-1 **не проходит**)

NSF seedfund, дословно:

> «The proposing small business must be at least **51% owned and controlled** by U.S. citizens or permanent residents, or by small businesses which are themselves owned and controlled by U.S. citizens or permanent residents, or by a combination of these.»  
> https://seedfund.nsf.gov/solicitation-eligibility/

CFR, точнее, **13 CFR § 121.702(a)(1)(i)**:

> «Be a concern which is **more than 50% directly owned and controlled** by one or more individuals (**who are citizens or permanent resident aliens of the United States**), other small business concerns (each of which is more than 50% directly owned and controlled by individuals who are citizens or permanent resident aliens of the United States)…»

«Permanent resident alien» = **green card**, не O-1, не H-1B, не E-2. SBA eligibility guide: «Each individual you include as part of the eligible ownership majority… must be either a citizen or permanent resident alien of the US.»

SBA считает ownership **fully diluted** (options, convertibles, reserved option pool) — § 121.702(d).

NSF **дополнительно** (жёстче NIH/DoD-638(dd)):

> «The SBIR/STTR programs do not support businesses that are majority-owned by one or more venture capital operating companies, hedge funds or private equity firms.»

Даже несколько VC в сумме >50% — **NSF нет**.

**Вывод по O-1 фаундеру-мажоритарию: компания неэлигибельна, точка.** Его акции **не считаются** в 51%.

### Ось B — PI (здесь O-1 **может**)

NSF 26-510, Who May Serve as PI:

> «The PI must have a **legal right to work** for the proposing company in the United States, **as evidenced by citizenship, permanent residency, or an appropriate visa**.»  
> Primary employment ≥51% at award (не обязательно на submit). >19.6h/week elsewhere = conflict. Phase I: ≥1 calendar month per 6 months performance. Degree не нужен.

Это **официальная** дверь: O-1 = appropriate visa, если есть legal right to work **на эту компанию** (petition от US employer/agent; self-petition как физлицо нельзя; компания-работодатель может петitioners).

### Легальные конфигурации (не «серые схемы обхода CFR»)

| Схема | Работает? | Цена |
|---|---|---|
| O-1 владеет 60%, гражданин 40% | **Нет** | Ownership fail |
| Гражданин/LPR **>50% equity AND control** (board, voting, drag), O-1 = PI + minority | **Да**, если control реально у majority-блока. SBA смотрит control, не только % | O-1 отдаёт контроль. Споры, dilution, founder psychology |
| Два фаундера: US citizen 51% / O-1 49%, O-1 = PI | **Да**, если citizen не nominee. Nominee/ostensible = fraud | Нужен настоящий co-founder, не «дядя с паспортом» |
| US small business (сама 51%+ citizen-owned) владеет 51%+ спин-аута | **Да** по тексту | Сложный cap table |
| Majority VC | **NSF нет** | Другие агентства с 638(dd) — может; NSF явно исключает |
| Работа за пределами US | **Нет** | «All R&D in the United States»; personnel — legal right to work in US |
| STTR с университетом | Ownership **тот же** 51% citizen/LPR | Не лечит O-1-majority |

**Это не юридическая консультация.** Перед pitch — US counsel, который ест SBIR. SBA size determination может запросить паспорт/green card каждого, кто в majority-блоке.

**Практический вердикт для фаундера на O-1 без LPR:** NSF Phase I **закрыт**, пока нет US-citizen/LPR контрольного партнёра. PI-роль — не проблема. Не трать 80 часов на proposal до cap table.

## 13. Non-dilutive альтернативы, доступные не-резиденту / O-1

NSF — американская промышленная политика, не «грант умному». Для не-US-majority другие двери:

| Программа | Кто может | Сумма | Fit Flight Recorder | Оговорка |
|---|---|---|---|---|
| **Circle Grants 2026 Cohort 2** | Глобально; акцент Arc + agentic commerce/payments/treasury | Questbook: Cohort 1 $120k paid / 19 accepted; Cohort 2 open, 653 proposals, 3 accepted на момент снинка | **Прямой fit** (agentic commerce в приоритетах 2026) | Маленький чек; «Arc central to value flow» |
| **Ethereum Foundation ESP** | Worldwide | $5k–$200k+ | Если verification on-chain / open infra | Не off-chain custodian story |
| **NLnet / NGI** | EU/worldwide open tech | €5k–€50k, scale-up | Если open-source audit trail | NGI Zero Commons **13-й call закрыт 01.06.2026**; другие NGI ещё живы |
| **EIC** (как у NeuralTrust) | EU company | крупнее | Нужна EU-юрисдикция | Консорциум часто; ТЗ просило «без консорциума» — EIC Accelerator solo возможен, но тяжелый |
| **Coinbase Ventures** | не грант, equity $0.5–10M | — | Тезис AI+crypto 2026 есть | Dilutive |
| **Gitcoin / Deep Funding** | public goods | десятки k | Если open evidence spec | Не B2B SKU |
| **FCA sandbox / US state sandboxes** | не деньги, доступ к регулятору | 0 | Aveni уже внутри FCA | Ценно для discovery |

**EF (Ethereum Foundation)** — да, без гражданства. **NLnet** — да, но мелкие чеки и open-source bias. **Stripe/Coinbase/Circle ecosystem grants** — Circle явно приоритетит agentic; Stripe отдельного grant program под assurance **не нашёл** (`[no source found]`).

Для O-1 фаундера **разумный non-dilutive путь:** Circle grant (быстро, маленький) + EF, если кусок on-chain proof открытый; NSF — только после US-majority co-founder. Не строить компанию «под NSF».

---

# Часть 4. Стейблкоин-коридоры (ниша снята, полстраницы)

Тезис «рынок B2B stablecoin infra занят, без лицензии входа нет» — **подтверждается для рельсов, не для assurance.**

- **Stripe / Bridge:** покупка Bridge **$1.1B** (фев 2025). Bridge живёт как Stripe company: accounts, issuance, on/off-ramp. Revolut EURR issued by Bridge (авг 2026). XDC Tech (31.08.2026): виртуальный IBAN на агента через Bridge. Рельсы **закрыты инкумбентом**.
- **Mastercard / BVNK:** сделка **до $1.8B** (март 2026), ~$30B annualized stablecoin volume, 130+ рынков. Крупнейший M&A категории.
- **Rain:** $250M Series C, янв 2026, ICONIQ, val **$1.95B**, total $338M. $3B+ annualized, 200+ partners (Western Union, Nuvei). Principal Mastercard май 2026. CEO 19.08.2026: 100k+ мерчантов получают стейбл, **не зная об этом**.
- **Conduit (conduitpay.com):** Series A **$36M** (2025), Dragonfly+Altos, Circle Ventures; CB Insights total ~$59M. Cross-border stablecoin vs SWIFT, LatAm/Africa. (Не путать с proptech Conduit €2.9M и Conduit Holdings reinsurance.)
- H1 2026 crypto VC: **$3.7B в payments/stablecoins** из $11.2B — всё в regulated/permissioned, не permissionless (NeosLegal / CoinDesk 15.08.2026).

**Вывод:** строить ещё один processor/on-ramp без лицензии — самоубийство. Строить **assurance-слой поверх** Bridge/BVNK/Rain/Circle — как раз потому, что рельсы консолидируются, а evidence layer — нет. XDC+Bridge уже продают «агент со своим IBAN»; следующий вопрос кастодиана: **докажи, кто это санкционировал**. Это ваш вход, не их.

---

# Что опровергает запуск Flight Recorder

1. **Arcade уже продаёт фразу «докажи, что агент имел право на действие» банкам**, $72M, MCP spec, named bank. Если treasurer скажет «у нас Arcade» — вы feature. Улика: Salazar, BusinessWire 15.06.2026.
2. **Nava заняла слово «verification before execution» + escrow** в крипто. Даже сырая, она гуглится первой по «agent escrow». Улика: stealth 14.04.2026, $8.3M.
3. **Session caps едут бесплатно от AWS/Coinbase/Stripe.** «Лимит на агента» перестаёт быть дифференциатором. Улика: AgentCore Payments 07.05.2026.
4. **Объём реальных agent payments крошечный** vs нарратив. Major Matters: x402 real daily ~$28k, половина wash. Покупатель «processor» может сказать «приходите, когда будет $X млрд».
5. **Слой может съесть инкумбент за 18 месяцев** (Arcade money-tool, NeuralTrust payment-policy, Skyfire evidence JWT). Окно не вечное. BlockEden про Nava: 18–24 месяца.

# Что поддерживает запуск

1. **Никто не клеит before/after ledger state + независимую санкцию + replay для regulated money.** Четвёрка ТЗ это не делает. Доки Runta/NeuralTrust/Lemma/Nava прочитаны.
2. **Инциденты уже денежные, и логи врут или недостаточны.** Bankr $150k; per-tx cap обход; Claude Code 1446 USDT; METR: агенты **подделывают свои логи**. Независимый recorder = ответ на последний пункт.
3. **Регулятор требует логи + human authority, HITL не масштабируется.** Art. 26 + DORA + BoE. Aveni уже в FCA sandbox — спрос не выдуман.
4. **Страховщики (AIUC, Klaimee, Armilla) не могут прайсить без loss data / evidence.** Nava сама пишет, что verification = пререквизит insurance. Вы можете быть evidence-pack, не конкурент Nava.
5. **Рельсы (Bridge/BVNK/Rain) закрыты — assurance нет.** Консолидация процессоров *увеличивает* ценность независимого чёрного ящика (как SWIFT gpi + separate audit), не уменьшает.

---

# Финальный вердикт

**Ниша не занята. Окно: financial-grade evidence object на границе денежного действия агента для regulated buyers (custody, treasury, stablecoin processor, TaaS, insurer/auditor).**  
Четыре названные компании — три смежных, одна прямая, но crypto-preview.

**Confidence:**  
- «Четвёрка не закрывает SKU» — **[established]**  
- «Окно существует 12–24 мес.» — **[emerging]**  
- «Кто заплатит $150k ACV в 2026» — **[speculative]**

---

## Опции (названные, не «направления»)

| Опция | Что делать | Trade-off | Риск |
|---|---|---|---|
| **A. Не запускать SKU, встроиться** | 45 дней = BD в Arcade (action evidence) или Skyfire (KYA) как money-pack | Быстрее путь к банку; вы не компания | Съедят / не возьмут; потеря equity-story |
| **B. Запускать узкий Flight Recorder** | ICP = 1: stablecoin processor или custodian. MVP: before/after + dual-control human key + signed policy version + replay. Не runtime, не gateway | Маленький рынок, ясный buyer | Arcade дойдёт сюда; Nava выйдет в Tempo |
| **C. «Guardian for fiat» против Nava** | Скопировать escrow-модель на off-chain / Tempo | Понятный конкурент | Вы второй Nava с меньшими $ и без Eigen-сети |
| **D. Evidence standard, не продукт** | Open spec (как agent-manifest) + AIUC-1 mapping + Klaimee/Armilla | Non-dilutive (EF/NLnet/Circle), миссия №2 | Нет revenue; стандарт без референс-имплементации мёртв |
| **E. NSF-first** | Pitch к 04.11 / 04.03 | $305k non-dilutive | **O-1 без LPR + majority = ineligible.** Не делать, пока cap table не US-majority |

**Рекомендация: B, не A и не E.**  
**Confidence: medium. Assumption:** за 45 дней найдутся ≥3 design partners из custody/processor, у которых уже болит «агент + деньга + аудитор спросит», и Arcade им не закрывает settlement-state. Если после 8–10 разговоров все говорят «нам хватает Arcade + card controls» — это фальсификация тезиса, тогда A или стоп.

Не тратить 45 дней на «карту категории» — карта готова. Тратить на **денежных** собеседников (не CISO, не AI-eng): Head of Payments в BVNK-like, treasury в TaaS, operational risk в custodian, underwriter в Klaimee/AIUC.

NSF: не планировать, пока нет US-citizen/LPR с >50% **и контролем**. Circle grant — да, сразу, как разведка, не как runway.

---

## Источники (все, которые реально открывал)

**Runta / a16z:** runta.com; runta.com/blog/runta-the-execution-layer-for-agents/ (16.07.2026); runta.com/about/; runta.com/pricing/; a16z.com/announcement/investing-in-runta/ (16.07.2026); thenextweb.com 17.07.2026; menlotimes.com 18.07.2026.

**NeuralTrust:** neuraltrust.ai/news/neuraltrust-raises-20m (17.06.2026); docs.neuraltrust.ai; PRNewswire 17.06 и 05.08.2026; thenextweb.com 18.06.2026; tech.eu 17.06.2026; Dealroom (конфликт стадии); elperiodico.com 21.04.2026; neuraltrust.ai/llms-full.txt.

**Lemma:** globenewswire.com 13.08.2026; docs.uselemma.ai; github.com/uselemma/lemma; ycombinator.com/companies/uselemma; unite.ai 13.08.2026; pitchbook.com/profiles/company/987083-83; techfundingnews.com 13.08.2026.

**Nava:** navalabs.ai/blog (индекс); chainwire.org 14.04.2026; newclawtimes.com 15.04.2026; blockeden.xyz 17.04.2026; finsmes.com 24.04.2026; x.com/navaai 14.04.2026; usethebitcoin.com 20.08.2026.

**Категория / инциденты / платежи:** gartner reprint Guardian Agents 25.02.2026; arcade.dev/blog 12.06.2026; businesswire Arcade 15.06.2026; docs.arcade.dev; forbes.com 17.05.2026 Janakiram; majormatters.co 24.06.2026; fintechspecs.com agent payments 15.08.2026; cryptoslate.com 13.08.2026 x402; decrypt.co 27.04.2026; cyera.com 27.08.2026; zealynx.io Bankr; techtimes.com 27.08.2026 METR; yahoo/finance ARS 08.04.2026; openkoda.com AI insurance 20.08.2026; fintech.global Klaimee 22.07.2026; timesofisrael.com Onyx 30.07.2026; skyfire.xyz; circle.com Agent Stack / grants; github.com/custena/agent-payment-protocols.

**Регуляторика:** seedfund.nsf.gov/solicitation-eligibility/; nsf.gov NSF 26-510; law.cornell.edu/cfr/text/13/121.702; ai-act-service-desk.ec.europa.eu Article 26; digital-strategy.ec.europa.eu AI Act timeline.

**Стейблкоины:** europeanbusinessmagazine.com Mastercard/BVNK 18.03.2026; theblock.co Rain 19.08.2026; crowdfundinsider.com Conduit 28.05.2025; coindesk.com 15.08.2026 $11.2B; stablecoininsider.org Stripe vs Bridge vs BVNK.

**X (30 дней, отдельно от веба):** @navaai экосистема — в основном low-engagement community; @HermesShield 01.09.2026 poisoned API; Hugging Face «GO authorization» 27–30.08.2026. Находки, которые живут **только на X**, в вердикт не входят.

---

🧒 **Простыми словами:**  
Четыре компании из списка — как сигнализация, камеры, дневник и замок на крипто-сейфе. Ты хочешь **чёрный ящик самолёта на денежный перевод робота**: кто разрешил, каким правилом, сколько было до и после, и чтобы робот не мог подделать запись. Такого замка в продаже нет. Ближе всех — парень с крипто-сейфом (Nava) и охранник дверей в банке (Arcade). Грант NSF как лотерея на $305k американцам с паспортом в контрольном пакете; на визе O-1 без грин-карты эта лотерея закрыта. Не строй второй Stripe. Строй ящик, который Stripe и кастодиан положат рядом с переводом.
