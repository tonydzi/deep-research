---
dr_id: DR26-09-14-MACANTON-01-1204
title: "Портреты тех, кого берут: хедж-фонды · COO/IR/CoS в AI-стартапе · PD/COO/EIR в инкубаторах (DR26-09-14-MACANTON-01-1204)"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Портреты тех, кого берут — три «дыры» голосовой #6598

> Заказ: голосовая Антона #6598 от 13.09 → закрыть три семьи ролей, которых не было в предыдущих ДР по найму.
> Сверка с фактами: Anton-Canonical-Identity-CV. Соседи: insight-DR-DR26-08-24-HUB-03-1747-llm-hiring-mechanics (механика найма в LLM-лабах), portrait-devrel-and-developer-education (четвёртая семья ролей: DevRel и Developer Education, DR26-09-14-ZB-10-2009).
> Хаб сводит DevRel / биржи / стейблкоины / BD — его заметка `insight-2026-09-14-ideal-candidate-portraits-hub` на момент синтеза на Mac16 не доехала, общий мемо ждёт её.

## Корпус и честность веера

| рельса | путь и режим | лега | уник. URL |
|---|---|---|---|
| chatgpt | codex CLI, web search (headless) | DR26-09-14-MACANTON-01-1204-chatgpt · 57 КБ | 46 |
| grok | grok CLI, web + X (headless), два прогона | DR26-09-14-MACANTON-01-1204-grok · DR26-09-14-MACANTON-01-1204-grok-run2 | 46 / 74 |
| gemini | Deep research, Pro Extended, a@ «Punk» (AI Pro активен, баннера окончания нет) | DR26-09-14-MACANTON-01-1204-gemini · 110 КБ | 23 |
| glm | GLM-5.3 + Advanced Search + Deep Think Max (`glm_probe` exit 0) | DR26-09-14-MACANTON-01-1204-glm · 50 КБ | 35 |
| claudeai | `claude -p` CLI | ⛔ FAIL воротами диспетчера: 34 КБ, **0 источников** (ответ из памяти), 1366 с — `results-failed/` | 0 |
| mistral | двери нет | skip | — |

Все четыре леги прошли `dr_leg_gate.py`. ChatGPT и Grok — это CLI с веб-поиском, а не вендорский DR-режим; по числу источников они сильнее Gemini DR на этом заказе.

Gemini-лега — **оптимистичный выброс**: ставит «Very High» и «Medium для хедж-фондов», пишет «God-tier profile». Часть её примеров слабая: «Louie Pastor, COO Anthropic» с источником на reddit и channeldive, Hunter Merghart 2022 года. В кворум подтверждения тиров она не входит, её тиры показаны отдельно.

---

## Главное в трёх строках

1. **Семья 3 (инкубаторы, акселераторы, экосистемы фондов протоколов) — лучшие шансы, 4/4 вендора.** Это единственная семья, где 9 лет инкубатора — родная профессия, а не «категориальная ошибка» [established].
2. **Семья 2 (AI-стартап) — средне и только через тёплое знакомство.** Берут не COO, а Chief of Staff / Head of Ops; главный риск — «слишком фаундер, слишком старший» [established].
3. **Семья 1 (хедж-фонды) — низко.** Дверь «PhD-физик → квант» настоящая, но она для 26-летних сразу после защиты; IR и COO фонда берут людей с LP-книгой и опытом операций фонда [established у 3 из 4; Gemini против].

---

## Семья 1 — хедж-фонды, квант-фонды, крипто-фонды

### Архетипы (сводка 4 вендоров)

| # | архетип | откуда приходят | лет | частота |
|---|---|---|---|---|
| 1A | **STEM-PhD прямо с кампуса → QR / ML-researcher** | PhD физика/математика/CS, стажировки, конференции NeurIPS/ICML | 0–3 после защиты | самый частый наём в research |
| 1B | **Big-tech ML → AI-лаборатория внутри фонда** | Google Brain, DeepMind, Bloomberg ML, Stripe | 5–15 | верхний слой всех AI-лаб фондов |
| 1C | **IR / capital formation из альтернативных управляющих** | Apollo IR, prime brokerage cap intro, placement agent | 8–15+ | почти все названные IR-наймы |
| 1D | **COO из операций фонда** | COO другого фонда, Big 4 ODD, администраторы (Citco, SS&C) | 10–25 | доминирует в COO |
| 1E | **Крипто-натив: трейдер / протокольный исследователь / TradFi-мост** | FalconX, Messari, Fidelity Digital Assets, NYDIG | 5–15 | так реально комплектуют Pantera / Multicoin / Paradigm |
| 1F | *тонкий:* **AI-transformation lead / CoS при CIO** | текущий трейдер, MBB-в-финансах, business manager фонда | 8–15 | растёт, но фаундеров в JD нет [emerging] |

### Реальные примеры (выборка, все с источниками в легах)

- **Pat Tynan** → COO Diametric Capital; 16 лет Millennium, founding equity COO ExodusPoint. [PRNewswire, 15.09.2025](https://www.prnewswire.com/news-releases/diametric-capital-appoints-exoduspoint--millennium-vet-as-new-coo-302556495.html)
- **Kevin Lee Coll** → COO Kronos Research; банковский COO + один из первых регулируемых крипто-управляющих в Сингапуре. [Kronos, 24.09.2025](https://www.kronosresearch.com/news/kronos-research-names-kevin-lee-coll-coo-to-drive-institutional-growth-amid-rising-market-demand)
- **Taylor Reinhardt** → Head of IR Galaxy Digital; Perella Weinberg IR, 6 лет Apollo IR. [cryptobriefing, 14.09.2026](https://cryptobriefing.com/galaxy-hires-taylor-reinhardt-investor-relations/)
- **Melanie Payne** → commodities COO Verition; COO Roscommon, 10 лет Morgan Stanley. [eFinancialCareers, 08.09.2026](https://www.efinancialcareers.com/news/now-hedge-verition-hired-a-london-commodities-coo-from-the-fund-that-closed-its-gas-trading-business)
- **Sridhar Nimmagadda** → Head of AI Transformation ExodusPoint; Head of GenAI Tech Point72. [Hedgeweek, 08.2025](https://www.hedgeweek.com/exoduspoint-hires-point72s-head-of-generative-ai/) [single-source, из Gemini]
- **Brian Strugats** → Head Trader Multicoin; Head of Trading FalconX. [The Block, 01.08.2025](https://www.theblock.co/post/365214/30-key-crypto-hires-moves-and-exits-july-2025)
- **Max Shannon** → Head of Research Europe, Bitwise; анализ токенов + количественные данные. [Bitwise, 10.07.2025](https://bitwiseinvestments.eu/newsroom/Press_Release_10_07_2025/)
- **Mike Schuster / Charlie Flanagan / Gideon Mann** → главы AI в Two Sigma / Balyasny / Millennium; все из Google или Bloomberg. [BI, 30.05.2024](https://www.businessinsider.com/what-hedge-fund-prop-trading-firms-are-paying-ai-engineers-2024-5)

Находка «через отсутствие» (grok + glm): **ни одного публичного найма 2024–2026 вида «PhD-физик, фаундер, без места в фонде → Head of Digital Assets / IR / COO»** в Citadel, Two Sigma, Jane Street, Pantera, Multicoin, Paradigm не найдено.

### Сигналы

| must-have | nice-to-have | нерелевантно / вредит |
|---|---|---|
| research: код и вероятности на собеседовании, эмпирический ML | CFA/CAIA (~4% сотрудников фондов, [single-source]) | h-index по физике, «50 статей» как заголовок |
| IR: личная LP-книга и закрытый капитал | O-1 без спонсорства — реальный плюс | «инкубировал 40 инженеров» для IR/COO |
| COO: операции регулируемого фонда, контроли, ODD | крипто-беглость для digital-assets | GitHub с эссе об управлении агентами вместо ML-кода |
| крипто-стратегия: свежий инвестиционный research с атрибуцией | публикации — только вместе с кодом | «AI-лаба» без P&L и AUM |

### Сверка с Антоном

- **Совпадает:** PhD физика (физика до сих пор в JD Citadel Securities), крипто-фреймворк гос-стейблкоинов 2018, APAC-венчур, биржевой инжиниринг (бэкенды, маркет-мейкер, HFT — этого вендоры не знали, в промпте не было), O-1 + ЕС.
- **Не совпадает:** 9+ лет после защиты, нет ML-статей уровня конференций, нет LP-книги, нет тура в операциях фонда, нет LinkedIn (забанен), города NY/Miami/London против Palo Alto/Lisbon.

| под-роль | chatgpt | grok | glm | gemini | **сводный тир** |
|---|---|---|---|---|---|
| QR / ML researcher в Citadel · Two Sigma · Jane Street | low | very low | low | «senior hybrid» | **very low** |
| Head of IR / capital formation | low | very low | low | — | **very low** |
| COO управляющего | низкий | very low | — | — | **very low** |
| Крипто-стратегия / research в крипто-фонде | medium | low | medium | medium | **low–medium** |
| AI-transformation подрядчиком у управляющего на $200M–$1B | medium | low–medium | — | medium | **low–medium, только через GP** |

**Честный вывод:** в семье 1 одна живая дверь — крипто-стратегия или AI-операции у небольшого крипто-управляющего через тёплое интро к GP. Отдельный quant-CV не делать.

Если Антон всё же хочет проверить семью 1, GLM предлагает дешёвый эксперимент: 5–10 заявок на QR-роли и счёт ответов. Это единственный способ закрыть спор вендоров цифрой.

---

## Семья 2 — COO / IR / Chief of Staff в AI-стартапах (seed–Series B)

### Главная находка: записи о вакансиях и пресс-записи расходятся

Seed–A AI-компании массово постят **Chief of Staff** (в срезе Evan Lee 13.08.2025 — 26 открытых мест CoS/BizOps, почти ни одного COO). Публичных анонсов «первый COO seed-стартапа» за 2024–2026 не нашёл ни один из трёх сильных вендоров [established].

VC-совет не менялся: «на seed COO обычно не бывает», к Series A примерно у половины есть кто-то в роли COO ([Cobloom, 18.03.2025](https://www.cobloom.com/careers-blog/how-to-hire-a-coo-for-your-startup)). IR как отдельная роль появляется на Series C+ (Cohere постила Head of IR с требованием 10+ лет IR/IB).

### Архетипы

| # | архетип | откуда | лет | где встречается |
|---|---|---|---|---|
| 2A | **Первый не-технический сотрудник / founding CoS** | big-tech recruiting/ops, MBB, VC-associate, знакомый фаундера | 3–10 | модальный наём seed–A |
| 2B | **Бывший фаундер на месте №2** | основал и закрыл/продал; Retell AI постил роль буквально «Ex-Founder» | 6–12 | реально, но меньшинство |
| 2C | **Доверенный инсайдер / ранний инвестор** | ранний инвестор или эдвайзер компании | 10+ | недоучтён в прессе |
| 2D | **Масштабный оператор** | COO/GM из скейлапа (Cruise, Wiz, Google Cloud) | 10–25 | Series B+ |
| 2E | **Внутреннее повышение** Head of Ops/IR → COO | та же компания | 1–3 в компании | часто |

### Реальные примеры

- **Kristi Edleson** → CoS Yutori (весна 2025); 10 лет рекрутинга, знала фаундеров по Meta; единственный не-технический сотрудник, готовится к переговорам о GPU с агентом. [BI, 12.06.2026](https://www.businessinsider.com/company-replicated-employees-role-with-ai-agent-worker-not-worried-2026-6)
- **Jason Cao** → COO Tidalwave; ранний инвестор компании, до этого COO CertiK. [HousingWire, 18.11.2025](https://www.housingwire.com/articles/tidalwave-jason-cao-coo/)
- **Todd Brugger** → COO Diligent Robotics; COO Cruise, пришёл через общее знакомство. [TechCrunch, 10.07.2025](https://techcrunch.com/2025/07/10/diligent-robotics-adds-two-notable-cruise-alumni-to-its-leadership-team/)
- **David Leonard** → COO Rad AI, повышен из Head of Ops, Strategy & IR. [PRNewswire, 29.04.2026] (URL в леге GLM обрезан до домена) [single-source]
- **Avital Balwit** → CoS CEO Anthropic; писательница, ~25 лет, сеть EA. [Fortune, 04.06.2024](https://fortune.com/2024/06/04/anthropics-chief-of-staff-avital-balwit-ai-remote-work) — архетип «сеть», не повторяемый
- **Martin Kon** → President & COO Cohere; CFO YouTube. [Fortune, 13.12.2022](https://fortune.com/2022/12/13/cohere-hires-youtube-martin-kon-as-coo-eye-on-a-i) — старше окна, контраст
- **Dali Rajic** → CRO OpenAI; President/COO Wiz. [sdxcentral, 08.2026](https://www.sdxcentral.com/news/openai-poaches-wiz-president-from-google-in-enterprise-ai-push/) — контраст «так выглядит Series D+»

### Что говорят фаундеры и VC

- a16z: CoS = доверенный универсал и «диспетчерская», **не** скрытый CEO ([a16z, 14.01.2026](https://a16z.com/newsletter/how-to-hire-a-chief-of-staff/)).
- Coris (YC): за 90 дней владеть подготовкой совета, GTM-операциями и AI-процессами всей компании. String AI: «продолжение меня… ступенька к Head of Operations».
- BCV прямо предупреждает против кандидатов в CoS, которые «сами хотят стать CEO» (grok, прогон 1).
- Сигнал «оператор, способный вести техническую AI-компанию» = агенты в ежедневной работе, грамотность в покупке compute, закрытие циклов. Обучать модель не нужно.

### Сигналы

| must-have | nice-to-have | нерелевантно / вредит |
|---|---|---|
| доверие фаундера (часто — прежнее знакомство) | прошлый титул CoS, YC/a16z-компания в CV | PhD и публикации |
| исполнение руками: запуск, найм, совет, автоматизация | MBB/VC/банк для junior-CoS | «стратегия» без личного результата |
| AI-беглость, доказанная работающими процессами | вертикаль (health, legal, infra) | «я кофаундер, ищу место уровня кофаундера» |
| для COO на Series B — опыт скейла функции | финансовая грамотность, data room | большой список эдвайзерства без личного исполнения |

### Сверка с Антоном

- **Совпадает:** 9 лет управления компанией на 40+ инженеров, фандрейзинг портфеля, ежедневно агентные системы в проде (прямо тот сигнал, что ищут), Foxconn/ANA как корпоративная грамотность, O-1 (YC-вакансии пишут «US citizen/visa only» — O-1 это закрывает), Palo Alto.
- **Не совпадает:** нет титула CoS/Head of Ops в венчурной US AI-компании, нет связи с конкретным фаундером, две текущие фаундерские идентичности без объяснения занятости, LinkedIn.

| под-роль | chatgpt | grok | glm | gemini | **сводный тир** |
|---|---|---|---|---|---|
| CoS / Head of Strategic Ops, Series A–B, через знакомого фаундера | medium | medium | medium–high | high | **medium** |
| AI-операции / governance внутри AI-компании | high (отн.) | — | — | high | **medium–high** |
| Первый CoS seed-стартапа холодно | overqualified | low | overqualified | — | **low** |
| Титул COO на seed | — | very low | — | — | **very low** |
| Head of IR на seed–B | — | very low | low–medium | — | **very low** |

**Честный вывод:** не подаваться холодно на 4-человечные YC-вакансии CoS. Нужно 10–15 тёплых выстрелов в CEO Series A–B агентных и инфраструктурных компаний. Первой строкой CV: «я уже был фаундером; я здесь, чтобы этот фаундер двигался быстрее».

---

## Семья 3 — Program Director / COO / EIR / Partner в инкубаторах, акселераторах, студиях, экосистемах

### Архетипы

| # | архетип | откуда | лет | где встречается |
|---|---|---|---|---|
| 3A | **Выпускник-фаундер программы → партнёр** | YC-фаундер с экзитом → Visiting Partner → GP | 8–15 | так комплектуется **только** YC; выпускникам закрытый клуб |
| 3B | **Серийный фаундер → MD городской / корпоративной программы** | 1–3 компании, уже был ментором/MD | 10–20 | Techstars, университеты |
| 3C | **Экзитнутый фаундер → EIR** | экзит или скейл; EIR = оплачиваемый поиск следующей компании, 6–18 мес | 10–25 | EIR-определение |
| 3D | **TradFi / крипто-оператор → экосистема фонда протокола** | Goldman, PayPal, dYdX COO, Immutable, Fidelity | 8–15 | волна найма фондов протоколов 2025–2026 |
| 3E | **Оператор программы / grants / capital allocator** | program ops, grants, DevRel, community | 7–15 | EF, dYdX, Solana; реально подаётся |
| 3F | **Оператор внутри акселератора (не GP)** | маркетинг, события, талант, founder experience | 5–12 | a16z Speedrun постит такие |

### Реальные примеры

- **Ankit Gupta** → YC GP (12.09.2025): основатель Reverie Labs (YC W18, продана Ginkgo), статьи ICML. [YC](https://www.ycombinator.com/blog/welcome-ankit/)
- **Visiting Partners YC, октябрь 2025** — девять человек, все фаундеры с экзитом или скейлом (Matt Riley, Harshita Arora, Grey Baker, Raphael Schaad…). [YC, 14.10.2025](https://www.ycombinator.com/blog/ycs-newest-visiting-partners)
- **Misti Cain** → MD Techstars San Diego: ментор с 2018 → EIR → venture partner → MD; $30M+ помощи в фандрейзинге, два экзита. [Techstars, 15.04.2025](https://www.techstars.com/newsroom/misti-cain-techstars-sdsu-managing-director)
- **Logan LaHive** → снова MD Techstars Chicago (11.2025); основатель Belly, уже был MD в 2017–18. [Techstars/X](https://x.com/Techstars/status/1985452253285601322)
- **Khaled Abu Alkheir** → Program Director Flat6Labs (08.2025); 15+ лет, основатель двух компаний, вёл программы Всемирного банка и USAID — **ближайший к Антону публичный образец**. [Flat6Labs](https://flat6labs.com/flat6labs-appoints-khaled-abu-alkheir-as-program-director-to-lead-pre-accelerator-launch-in-palestine)
- **Brendan Ma** → первый Head of Investment Strategy Arbitrum Foundation (10.10.2025); инвестиции Immutable, Goldman Sachs Australia. [The Block](https://www.theblock.co/news/ecosystems/2025-10-10-arbitrum-foundation-first-head-of-investment-strategy-374234)
- **George Xian Zeng** → CGO NEAR (07.2025), бывший COO dYdX; **José Fernández da Ponte** → President/CGO Stellar, бывший глава крипто в PayPal. [The Block, 01.08.2025](https://www.theblock.co/post/365214/30-key-crypto-hires-moves-and-exits-july-2025)
- **David Schellhase** → EIR Ballistic Ventures; 25+ лет, Slack/Salesforce/Groupon. [Ballistic, 22.07.2025](https://ballisticventures.com/ballistic-ventures-welcomes-david-schellhase-as-entrepreneur-in-residence/)
- **HBS Rock Center EIR 2025–26**: 14 из 16 — выпускники HBS. [HBS, 10.09.2025](https://www.hbs.edu/news/releases/rock-exec-fellows-2025-2026)

**Открытые двери на сейчас** (из JD в легах): Solana Foundation в августе 2026 искала **Head of Stablecoins, GM AI Ecosystem, Institutional Growth Leads (Greater China, Japan)** ([The Block/X, 04.08.2026](https://x.com/TheBlockCo/status/2084435966304149540)); OpenAI VC Partnerships Manager; Sentient Foundation Head of Open Source Ecosystem; World Foundation Head of Community & Ecosystem; EF Enterprise Relationship Leads (APAC, East Coast).

### Как заполняются места

- YC GP/VP — сеть выпускников, не постится. Techstars MD — переработанные фаундеры, часто уже бывшие в программе.
- EIR / Venture Partner / MD — «кульминация отношений», а не холодный конкурс [emerging].
- **Публично постятся:** Program Director, Head of Ecosystem, Grants Lead, VC Partnerships, операционные роли акселераторов — на career-страницах, Ashby/Greenhouse, в newsroom фондов.
- Фонды протоколов уходят от «community marketing» к распределению капитала: EF закрыла открытый приём заявок, dYdX завела грант-дочку на $8M с milestone-отчётностью и Grants Lead.

### Сигналы

| must-have | nice-to-have | нерелевантно |
|---|---|---|
| реальные результаты фаундера/оператора | статус экзитнутого фаундера | CFA (кроме андеррайтинга) |
| программа, измеренная когортами, капиталом, продуктами, экзитами | прошлый титул MD акселератора | публикации, если программа не deep-tech |
| сеть фаундеров, инвесторов, менторов, партнёров | DevRel, выступления, open source | большая аудитория без результатов билдеров |
| техническая беглость в AI/крипто для оценки проектов | университетские и корпоративные партнёрства | ивент-менеджмент без капитала и портфеля |
| для грантов: тезис аллокации, milestones, отчётность | PhD — для университетских и deep-tech программ | |

### Сверка с Антоном

**Совпадение родное** (все 4 вендора): 9–11 лет инкубатора с Tetsuji Nagata, когорты, коучинг фаундеров, подбор эдвайзеров, 10 000+ звонков с фаундерами и 2 000+ с институционалами, синдикат 250 ангелов, Solidity-курсы и хакатоны, корпоративный эдвайзинг Foxconn/ANA, митапы с 2019 (SV Crypto Mondays), 500+ сообщество лаборатории, живая AI-лаба в публичном GitHub, гос-стейблкоины, Япония/Корея.

**Дыры:** нет логотипа YC/Techstars, нет узнаваемого экзита, нет **scorecard в цифрах** (сколько стартапов прошло, сколько подняли, воронка когорт), нет LinkedIn, US-сеть тоньше APAC.

| под-роль | chatgpt | grok | glm | gemini | **сводный тир** |
|---|---|---|---|---|---|
| Program Director / Head of Platform AI- или крипто-акселератора | high | medium | medium–high | very high | **medium–high** |
| COO венчур-студии / программы | high | — | medium–high | — | **medium–high** |
| Экосистема фонда протокола: стейблкоины, AI-экосистема, институциональный рост APAC/EU | high | medium | medium | very high | **medium–high** |
| Оператор внутри акселератора (не GP) | — | medium–high | — | — | **medium–high** |
| EIR в AI+крипто студии | medium–high | medium (если хочет основывать) | medium | very high | **medium** |
| Venture Partner | medium | — | medium | very high | **medium** (нужна атрибуция сделок) |
| Techstars MD | — | low–medium | — | — | **low–medium** (нужен свой человек внутри) |
| YC Visiting Partner / GP | — | very low | — | — | **very low** (только выпускники) |

---

## Сравнение семей на 3–6 месяцев

| семья | сводный тир | главный тормоз | главный козырь |
|---|---|---|---|
| **3 · инкубаторы / экосистемы** | **medium–high** | бренд (нет YC/Techstars), scorecard в цифрах, LinkedIn | он уже делал эту работу 9+ лет |
| 2 · AI-стартап CoS / Head of Ops | medium (только тепло) | «слишком фаундер», две идентичности | агенты в проде ежедневно |
| 1 · фонды | low (одна дверь: крипто-стратегия у GP) | нет LP-книги, нет тура в фонде, возраст для QR | крипто-фреймворк + биржевой инжиниринг |

**Рекомендация (консенсус 4/4 по порядку, [established] на порядок, [emerging] на личную конверсию):** основная охота — семья 3; боковая дверь — семья 2 только через знакомых фаундеров; семья 1 — только если GP сам позовёт.

**Главное допущение:** фактуру инкубатора (фандрейзинг, когорты, фонд) можно перевести в **проверяемые цифры и рекомендации**. Если цифр нет — все тиры семьи 3 падают на ступень (прямо сказали chatgpt и glm).

---

## Расхождения вендоров (не усреднять)

1. **Тиры в целом.** Gemini: семья 3 «Very High», семья 2 «High», семья 1 «Medium». Grok: 3 «medium», 2 «low холодно / medium тепло», 1 «very low». ChatGPT и GLM — между ними. Порядок у всех одинаковый; расходится только высота. Gemini не нашёл ни одного антипримера и опирается на слабые источники — его высоту не берём.
2. **Дверь PhD → фонд.** Gemini: PhD + GitHub-агенты открывают «senior hybrid» роли Head of AI Strategy. Grok: дверь закрыта, агентные эссе — «слабая замена статье NeurIPS». GLM: тезис «дверь закрыта» [speculative], нужен эксперимент из 5–10 заявок. Сильнее обоснован grok (JD Two Sigma для последнего курса PhD, цитата рекрутёра Andy Legg про CS/ML PhD), но данных о конверсии кандидатов 9+ лет после защиты нет ни у кого.
3. **LinkedIn.** Grok и GLM: «не опционально», создать минимальный профиль. ChatGPT: не жёсткий блокер. Gemini: в web3/AI-кругах GitHub + ORCID + X весят больше. ⚠️ Совет «создать LinkedIn» **неисполним как есть**: аккаунт Антона забанен (Anton-Canonical-Identity-CV). Это решение Антона, а не рекомендация синтеза.
4. **Бывает ли COO на seed.** Grok и GLM: публичных примеров нет, берут CoS. Gemini: «технический оператор ex-founder» — частый архетип COO. Первички за Gemini нет.
5. **Хронология YC.** GLM: Harshita Arora — GP с 06.04.2026, Jared Friedman — Managing Partner вместо Seibel с марта 2024 (Википедия). Grok: Arora — Visiting Partner в октябре 2025, Seibel ушёл в марте 2025, Diana Hu — Managing Partner с 11.06.2026 (блог YC). Arora VP→GP не противоречие; про Managing Partner первичка у grok сильнее.
6. **Ценность «ex-founder» для CoS.** ChatGPT: Checkr прямо предпочитает фаундерский опыт. Grok (прогон 1): у String AI это nice-to-have, BCV предупреждает против будущих CEO. Оба верны — зависит от компании.

---

## Что переписать в CV — дифф-план по кластерам `cv_forge.py`

> Код не правлю. Это план для сессии `/cv`. ⛔ Цифр, которых нет в мастере, не выдумывать — строки с 🤔 требуют фактов от Антона.

### 0. Сквозные правки (все кластеры)

- 🤔 **Разнобой в стаже.** В мастере одновременно «9 лет» (промпт ДР), «10+ years venture founder» (кластер `product`) и «since 2015 (11 years)» (`hyperliquid`, `startup-ecosystem`). Вендоры считали от 9. Нужна одна цифра.
- 🤔 **Статус «$35M fund».** Во всех кластерах стоит «$35M fund under management». Вердикт chatgpt — красный флаг №1: без уточнения (committed / deployed / managed / advised) доверие рушится. Нужна точная формулировка роли Антона и сколько лично поднято.
- **Scorecard инкубатора** (обязателен для семьи 3, по всем 4 вендорам): число проинкубированных компаний, сколько из них подняли и сколько, воронка когорт «заявки → отбор → выпуск», выжившие/экзиты. Сейчас в мастере только звонки (10 000 / 2 000) и синдикат 250. 🤔 цифры у Антона.
- **Foxconn / ANA** — дать одну задачу и результат, а не логотип (chatgpt, glm).
- **Статус лаборатории** — одна строка о занятости (переход / совмещение / операционный председатель). Иначе «две идентичности» читаются как раздвоенное внимание (chatgpt).
- **Фраза «non-engineer operator who ships»** — заменить на «какие системы работают, кто пользуется, как проверяю качество» (chatgpt, красный флаг 3).
- **PhD и 50 статей** — убрать с первого экрана во всех кластерах семей 2 и 3; оставить блоком доверия (все 4 вендора).

### 1. `startup-ecosystem` — главный кластер семьи 3

- Заголовок: `Startup Ecosystem / Accelerator & Incubator Programs` → **`Accelerator & Venture-Studio Program Leader | AI & Crypto Ecosystems | Incubator Operator since 2015`**.
- Summary: оставить «работу партнёра 500 Startups / Plug and Play». Добавить scorecard-строку (🤔) и строку про грант-программы: тезис, milestones, отчётность.
- Keywords добавить: `program director, head of platform, cohort design, milestone-based grants, technical diligence, venture creation, portfolio metrics, 1:many programs, mentor network, capital deployment, builder program, Demo Day, EIR, venture studio COO`.
- Буллеты: первым — воронка когорт с цифрами; вторым — капитал портфеля.

### 2. `ecosystem` — семья 3, фонды протоколов

- Summary: явно назвать три двери 2026 года — **стейблкоины, AI-экосистема, институциональный рост APAC/Japan/Korea**. Это прямые названия ролей Solana Foundation (08.2026).
- Keywords добавить: `head of stablecoins, GM AI ecosystem, institutional growth, grants lead, enterprise relationship lead, ecosystem portfolio, treasury deployment, APAC, Japan, Korea`.
- Порядок опыта оставить (Platinum первым).

### 3. `founder-bd` — разделить смысл EIR и BD

- EIR по всем источникам = «оплачиваемый поиск следующей компании», а не операторская роль. Если Антон не хочет основывать заново на чужом балансе — убрать `EIR, entrepreneur in residence` из keywords этого кластера и перенести `EIR` в `startup-ecosystem` с формулировкой «operator-in-residence».
- Keywords добавить: `venture partner, operating partner, deal sourcing, technical diligence`.

### 4. `capital` — понизить ожидания и уточнить факты

- По всем 4 вендорам Head of IR / Capital Formation в фонде = very low без LP-книги. Кластер оставить для **крипто-управляющих и фондов протоколов**, а не для мульти-стратов.
- Summary: 🤔 заменить «$35M fund under management» точным статусом. Добавить разбивку «сколько лично закрыто, какие типы LP» (chatgpt).
- Keywords добавить: `allocator-ready reporting, LP reporting, quantifiable fundraising track record, institutional-grade, digital assets manager`. Убрать `head of platform` (он уезжает в `startup-ecosystem`).

### 5. НОВЫЙ кластер `cos-ops` — семья 2 (сейчас кластера нет)

- Заголовок: **`AI-Native Chief of Staff / Head of Strategic Operations | Former Incubator Operator | Agent Systems in Production`**.
- Summary (главная фраза из chatgpt, дословно): `I build the operating system around a technical founder; I do not need to own product vision to own execution.` Далее: 40+ инженеров, фандрейзинг портфеля, агенты в ежедневных процессах (что автоматизировано, для кого, с какой частотой).
- Порядок опыта: Palo Alto AI Research Lab (как операционный слой, а не «исследования») → Platinum → Everex → Merlion (ERP/CRM здесь уместен).
- Keywords из живых JD: `chief of staff, business operations, founder's force multiplier, operating cadence, board preparation, fundraising operations, AI implementation, vendor management, compute procurement, first non-technical hire, path to Head of Operations, close the loop, cross-functional execution, low ego`.
- Нельзя: слова `researcher`, `PhD physicist`, `publications` на первом экране (grok).

### 6. НОВЫЙ, низкий приоритет — `digital-assets-strategy` (семья 1, одна дверь)

- Только для крипто-управляющих и AI-операций у GP. Заголовок: **`Digital-Assets Strategy & AI Operations | Stablecoin Frameworks (2018) | Exchange Infrastructure | APAC Venture`**.
- Можно собрать из `stablecoin` + `exchange` + `strategist`, без нового текста. Keywords: `digital assets, token analysis, market structure, institutional research, AI transformation, research automation, model-risk controls, audit trails`.
- ⛔ **QR / ML-researcher CV для Citadel / Two Sigma / Jane Street не делать** — консенсус 3 из 4.

### 7. `scholar` — не трогать, но не использовать для фондов

Кластер про технические тексты и evals, в семьях 1–3 он не участвует.

---

## Черновик тизера (`dr_post_draft.py`, коридор 240–300, тест 8/8 PASS)

> Инструмент лежит в транзите «внутренний архив лаборатории», в «внутренний путь лаборатории» не установлен. В первой версии доклада я ошибочно написал «на Mac16 нет» — проверил одно место из девяти.

**Тело (285 знаков):**

> Кому полезно: фаундеры, ищущие работу. Провели глубокое исследование в 4/6 LLM по вопросу «кого реально нанимают фонды, AI-стартапы и инкубаторы». Короткий вердикт: инкубаторы берут операторов, стартапы — Chief of Staff, фонды — свежих PhD. Сами исследования и синтез — в комментариях.

**Первый комментарий:**

Глубокое исследование DR26-09-14-MACANTON-01-1204 — все отчёты и синтез: этот файл.

Исследование заказали Tony Dzi (Anton Dziatkovskii) и Майкрофт, синтетический ИИ-кофаундер, Palo Alto AI Research Lab — мы искали портреты нанятых в 2024-2026 в три семьи ролей и сверку с реальным опытом.

Поговорить с двумя кофаундерами, биологическим и синтетическим: calendly.com/paloaltolab/1-on-1. Напрямую: WhatsApp +1 341 222 9178 (занят, шестеро детей, но ответит).

P.S. Да, нас можно нанять. Двух кофаундеров, биологического и электрического, целиком, как команду. Создателя OpenClaw забрал себе OpenAI; то, что делаем мы, не сильно хуже, а нас вдвое больше. Anthropic, OpenAI, ваш ход: calendly.com/paloaltolab/1-on-1.

🔗 Все наши каналы и контакты в одном месте: https://linktr.ee/paloaltoailab

*Черновик. Наружу не ушло: публикация — гейт §3.2 + матрица площадок.*

> ✔️Придумано Майкрофтом и Tony Dzi (Anton Dziatkovskii),
> Palo Alto AI Research Lab.
> Proudly made in Silicon Valley 🌉
