# DR26-08-01-MACANTON-02-658 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-658
rail: chatgpt (codex exec (gpt-5.6-sol, подписка ChatGPT a2))
session_uuid: 019fbdac-958c-7d42-aebc-0f9f882d0492
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:20:26Z
proof_of_work: 47 уник. URL, 26 веб-поисков — гейт пройден
note: чистый прогон, tool-mix = только web search
---

## 1. Экономический вердикт

Да, американская компания технически может нанять человека, остающегося в Лиссабоне: через собственное португальское юрлицо, Employer of Record — Deel/Remote/G‑P/Velocity Global — либо как португальского независимого подрядчика. Но исследование не подтверждает исходную посылку, что frontier labs «обычно» позволяют любому сотруднику работать из любой страны.

На 1 августа 2026 года Anthropic в основном требует регулярного присутствия в офисе; её remote-friendly вакансии ограничены указанными странами, обычно США/Канада/Великобритания. OpenAI, xAI, Google DeepMind и Meta нанимают по конкретным офисам или заранее одобренным странам. Perplexity прямо пишет: remote означает преимущественно Remote-US, а международные зарплаты устанавливаются по локальному рынку. Реально location-independent выглядит Hugging Face, но конкретная вакансия должна допускать Португалию.

Следовательно, наиболее вероятная сделка из Лиссабона — не «американский payroll дистанционно», а:

1. португальский employee через EOR/местное юрлицо с локализованной зарплатой; или  
2. B2B-контракт через *trabalhador independente* либо Lda., обычно без employee equity и benefits.

При обычном португальском налогообложении $200k base ≈ €175.6k gross и даёт ориентировочно €82–83k employee-net либо €96–97k freelancer-net в год. При $250k: приблизительно €99k и €117k соответственно. Это расчёты, не офферные данные.

Для четырёх детей англоязычная международная школа может стоить €45–85k в год; вместе с жильём, жизнью и спортом семейный порог составляет примерно €100–145k net. Значит, $200k base обычно недостаточен; $250k пограничен; $300k+ cash либо €200k+ при подтверждённом IFICI — уже реалистично закрывает бюджет. Неликвидное equity нельзя считать школьными деньгами.

---

## 2. Компании: международный remote и компенсация

Состояние вакансий проверено 1 августа 2026 года. Диапазоны ниже — опубликованный **base**, если источник прямо не говорит TC.

| Компания | Remote-international из Португалии? | Вероятный инструмент | Реалистичные семейства ролей | Опубликованная компенсация, USD gross | Доказательство и дата |
|---|---|---|---|---:|---|
| Anthropic | **Не по умолчанию.** Отдельные remote-friendly роли существуют, но привязаны к разрешённым локациям и поездкам | Собственные UK/EU entities там, где указано; для PT EOR/contractor публично не подтверждён | Product/Applied Engineer, Research Engineer с сильным OSS, Safeguards; Fellows | Типичный подтверждённый ориентир для Applied AI Architect в Ирландии — €190–215k base; Fellows $3,850/week | Текущая политика требует регулярных приездов; fully remote описан как переходный период при relocation: [Anthropic Careers, проверено июль 2026](https://www.anthropic.com/careers). Перечень текущих remote-friendly travel-required ролей: [Jobs, июль 2026](https://www.anthropic.com/careers/jobs). Fellows: [вакансия, июль 2026](https://job-boards.greenhouse.io/anthropic/jobs/5023394008) |
| OpenAI | **Нет универсального worldwide-remote.** Португалия не фигурирует как разрешённая engineering location; есть Dublin, London, Madrid, Munich и Remote-US | Собственная EU payroll infrastructure в конкретных странах; PT EOR не подтверждён | AI Deployment/FDE, Solutions Architect, Developer Experience, Technical Success | Solutions/Deployment: Levels median TC $418k = $218k base + $200k equity; это US self-report, не PT band | Текущие FDE локации: [OpenAI Careers, июль 2026](https://openai.com/careers/search/?c=923a141e-ff07-478e-9039-1d4fdbd65588). Remote-US явно маркируется отдельно: [Careers, июль 2026](https://openai.com/careers/search/?l=e8062547-b090-4206-8f1e-7329e0014e98). Self-reported TC: [Levels.fyi, обновлено 18 июля 2026](https://www.levels.fyi/companies/openai/salaries/solution-architect) |
| xAI | Core engineering преимущественно office-first; worldwide remote в основном AI Tutor/Human Data и отдельные contracts | Contractor для tutor/data; entity для офисных ролей; PT EOR не подтверждён | AI Tutor — Physics/Math/Software; core MTS значительно менее реалистичен remote | Для PT core-role надёжного диапазона нет; поэтому не оцениваю | xAI говорит, что приоритет — in-person; «exceptional candidates» могут подать откуда угодно: [Careers, июнь 2026](https://x.ai/careers). Remote tutor roster: [Open roles, июль 2026](https://x.ai/careers/open-roles?office=4023161007) |
| Google DeepMind US | **Нет** для произвольной работы из PT. Remote eligibility относится к перечисленной стране | Google entity/payroll только в разрешённой location | Developer Experience, Applied AI/FDE, Cloud AI Solutions — реалистичнее pure research | Надёжного PT/remote band для этих вакансий не опубликовано | Текущий Staff Developer Experience находится в Bengaluru; Applied/FDE — перечисленные офисы: [Google Careers, июль 2026](https://www.google.com/about/careers/applications/jobs/results?hl=en_US). DeepMind research также location-specific: [поиск вакансий, июль 2026](https://www.google.com/about/careers/applications/jobs/results/124776587752022726-research-scientist-ml-efficiency-google-research?q=%22Research+Scientist%22) |
| Meta AI | Remote означает одобренную employment-country/state, не worldwide | Meta entity в поддерживаемых странах; EOR для PT публично не подтверждён | Developer Advocate, Partner/Solutions Engineering, production engineering | Meta Developer Advocate E5: около $380–470k TC; $214–220k base, $134–250k stock, $0–32k bonus — self-report | [InterviewPal, обновлено 6 апреля 2026](https://www.interviewpal.com/companies/meta/salaries/developer-advocate). Это US data, не гарантия международного band |
| Perplexity | **Иногда international, но только перечисленные страны.** Большинство remote — Remote-US; Portugal не обнаружена | International entity/EOR возможен, но поставщик не раскрыт | DevRel/API Platform, MTS product/growth/security, Customer Success Engineer | DevRel US band на текущей вакансии не раскрыт; MTS security $220–405k base; inference platform $250–485k base | Компания прямо говорит, что USD ranges применимы только к US, international salary определяется local market: [Security MTS, июль 2026](https://jobs.ashbyhq.com/perplexity/6c9b3c71-85ba-47db-bce5-44fd9fa95d03). [Inference Platform, июль 2026](https://jobs.ashbyhq.com/perplexity/4b8347e9-615e-4176-bfb8-3be9025f9f27) |
| Hugging Face US | **Да, наиболее remote-native из списка**, но eligibility конкретной вакансии необходимо подтвердить | Собственные entities, EOR либо contractor — вакансия должна это определить | Developer Experience, Inference, OSS/community, Solutions | Текущего надёжного 2026 band для нужной роли не найдено — не подставляю оценку | Полностью remote Developer Experience был документирован в мае 2024, но это **возможно устарело**: [HF post, 23 мая 2024](https://huggingface.co/posts/MoritzLaurer/421330965674799) |
| Serious AI-adjacent US startup | Часто да, но только если вакансия говорит worldwide/EMEA, а не Remote-US | Чаще contractor либо EOR; реже собственная EU entity | DevRel, Solutions, AI Engineer, Technical Content, SDK/product engineering | Конкретный band зависит от работодателя; blanket estimate не даю | Инструмент EOR действительно позволяет нанять без собственного PT entity: [Deel, июль 2026](https://www.deel.com/solutions/payroll/eor/). Это доказательство механизма, не того, что конкретная lab согласится им воспользоваться |

### Что реально является дверью

Это вывод, а не опубликованная hiring policy:

1. **Applied/Product/AI Deployment/Solutions/FDE** — лучший fit для бывшего founder, умеющего самостоятельно доводить системы до production. OpenAI прямо называет former founder плюсом в FDE: [Munich posting, июль 2026](https://openai.com/careers/forward-deployed-software-engineer-munich-munich-germany/).
2. **Developer Experience/DevRel, совмещённый с engineering**, а не чистый evangelism.
3. **Evaluation, safeguards, red-teaming, domain expert** — особенно при physics/crypto/security expertise.
4. **Pure Research Scientist** — худшая вероятность без свежего frontier-ML publication record.
5. **Community/content-only** — доступнее, но обычно слабее по компенсации и equity.

---

## 3. Инструмент и сумма, остающаяся в Португалии

### Метод расчёта

- EUR/USD: €1 = $1.1392, ECB reference 23 июля 2026: [ECB](https://data.ecb.europa.eu/currency-converter).
- Обычный IRS 2026: 12.5–48%, верхняя ставка после €86,634; использованы официальные bracket/deduction values: [PwC 2026 Budget Law](https://www.pwc.com/pt/en/pwcinforfisco/state-budget/2026/pwc-state-budget-law-2026.pdf).
- Employee Social Security: 11% работник + 23.75% работодатель: [PwC 2026 Social Security](https://www.pwc.pt/en/pwcinforfisco/tax-guide/2026/social-security.html).
- Freelancer: IRS обычно на 75% professional-services revenue; необходимо подтвердить расходы, соответствующие 15% gross. [Autoridade Tributária, 2026](https://info.portaldasfinancas.gov.pt/pt/apoio_ao_contribuinte/Cidadaos/Rendimentos/Trabalho_por_conta_propria/Paginas/default.aspx).
- Freelancer SS: 21.4% на 70% service revenue, то есть приблизительно 14.98% gross: [Segurança Social guide, актуализирован к 2025; возможно слегка устарел](https://en.seg-social.pt/documents/18702991/18704017/1009%20-%20New%20scheme%20for%20self-employed%20persons%20-%20Novo%20Regime.pdf).
- В расчёте нет spouse deductions, dependent credits, deductible school/health costs, VAT, accountant, insurance, FX fees или special first-year relief. Округление — до ближайшей €1k.

| Инструмент | $150k gross / €131.7k | $200k / €175.6k | $250k / €219.5k | Equity | Что теряется |
|---|---:|---:|---:|---|---|
| Employee через PT entity/EOR, обычный IRS | ≈€66k net | ≈€82–83k | ≈€99k | Может быть, если plan разрешает EOR/international participants | US health/401(k) заменяются PT benefits; возможна локализация base; employer дополнительно платит 23.75% SS |
| Employee при подтверждённом IFICI | ≈€91k | ≈€121k | ≈€151k | Обычно возможно | Eligibility очень узкая; EOR может разрушить квалифицирующую связь |
| *Trabalhador independente*, simplified regime | ≈€76k | ≈€96–97k | ≈€117k | Обычно нет; только если отдельный plan/grant разрешает contractors | PTO, severance, unemployment protection, employer SS, health cover, bonus и стандартный equity plan |
| Contractor при IFICI | ≈€92k | ≈€123k | ≈€154k | Не гарантируется | Те же employee protections; требуется подтвердить, что именно деятельность и организация квалифицируются |
| Собственная PT Lda. | Нельзя честно вывести одной ставкой | — | — | Может заключить отдельный option/phantom agreement | Бухгалтерия, payroll директора, corporate formalities; retained profit не равен деньгам семьи |

### Почему Lda. не означает «19% и всё»

В 2026 mainland IRC — 19%; для qualifying SME первые €50k taxable profit облагаются по 15%: [правительство Португалии, Budget 2026](https://portugal.gov.pt/en/gc25/communication/news/draft-budget-cuts-taxes-on-companies-and-workers), [актуальный government guide, 29 апреля 2026](https://www.gov.pt/guias/imposto-sobre-o-rendimento-das-pessoas-coletivas-irc-em-portugal). Но после IRC деньги всё ещё нужно вывести зарплатой/management remuneration или дивидендами; возникают личный IRS/28% dividend tax и иногда SS. Lda. выгоднее главным образом для реинвестируемой прибыли и реальных расходов, а не автоматически для полного ежегодного вывода средств на школу.

### IFICI / «NHR 2.0»

Старый NHR закрыт для новых entrants, кроме закончивших переходное оформление. IFICI действует до десяти лет и даёт 20% на qualifying Category A/B income. Официальные условия и ставка: [Autoridade Tributária, проверено июль 2026](https://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/questoes_frequentes/pages/faqs-01018.aspx).

Но это не общая льгота «для любого программиста». Нужны одновременно:

- отсутствие PT tax residency в предыдущие пять лет;
- новая tax residency;
- qualifying scientific/innovation activity;
- работа у квалифицирующей организации либо другая предусмотренная законом связь;
- своевременная регистрация через компетентный орган.

Для уже давно живущего в Лиссабоне человека окно, вероятнее всего, закрыто. Даже если пятилетний тест проходит, обычный EOR может не быть qualifying R&D/employer entity. В июне 2026 опубликован именно такой first-person случай отказа при US employer через Rippling EOR; это анекдот, не административный precedent: [PortugalExpats, 26 июня 2026](https://www.reddit.com/r/PortugalExpats/comments/1ug2a84/ificinhr_20_cae_code_technicality_looking_for/).

**Практический вывод:** бюджетировать нужно по обычному IRS; IFICI считать upside только после письменного заключения португальского налогового специалиста и подтверждения IAPMEI/ANI/другого компетентного органа.

### US withholding и treaty

Работа, физически выполненная в Португалии, обычно является foreign-source services income и не должна облагаться US federal withholding. Американский payer обычно запрашивает:

- W‑8BEN — физлицо;
- W‑8BEN‑E — PT company;
- Form 8233 — если personal services выполняются физически в США и заявляется treaty exemption.

Актуальная IRS инструкция: [Claiming treaty benefits, апрель 2026](https://www.irs.gov/individuals/international-taxpayers/claiming-tax-treaty-benefits); [Form 8233 instructions, декабрь 2025](https://www.irs.gov/instructions/i8233); [US–Portugal treaty, IRS copy, 2026](https://www.irs.gov/pub/irs-trty/portugal.pdf).

Ловушки:

- W‑8BEN не заменяет регистрацию деятельности, Portuguese invoices/recibos verdes, Modelo 3 и SS declarations.
- Дни, когда услуги фактически выполняются в США, требуют отдельного source/treaty анализа.
- Один почти эксклюзивный клиент, контроль времени и обязанностей создают Portuguese false-self-employment/misclassification risk.
- Для B2B-услуг US client VAT обычно не начисляется по place-of-supply rules, но invoice и periodic reporting должен проверить contabilista.

### Equity, PPUs и tender liquidity

Важная поправка: формулировка brief про PPUs уже частично устарела. После реструктуризации 28 октября 2025 года сотрудники и инвесторы владеют equity OpenAI Group; сама OpenAI сообщает, что им принадлежит около 47%: [OpenAI structure, 28 октября 2025](https://openai.com/our-structure/). Публичные employee accounts утверждают, что новые офферы 2026 переходят с PPUs на RSUs; это **не подтверждено официальным plan document**: [Levels.fyi thread, январь 2026](https://www.reddit.com/r/levels_fyi/comments/1qeqst9/openai_shifts_from_ppus_to_rsu_for_all_new_offer/).

- PPU — договорное право участвовать в ограниченном пуле будущей прибыли; это не свободно продаваемая акция.
- RSU — обещание выдать акции при vesting при выполнении plan conditions.
- Tender offer — редкое окно, когда компания/approved investors позволяют eligible holders продать ограниченное количество vested shares.
- Tender не гарантирован, может иметь service-tenure и residency/securities-law ограничения.
- У contractor нет автоматического права на equity: он может получить его только если plan и договор прямо включают non-employees и его юрисдикцию.

Anthropic в феврале 2026 готовила employee tender при оценке около $350bn: [Bloomberg, 4 февраля 2026](https://www.bloomberg.com/news/articles/2026-02-04/anthropic-plans-employee-tender-offer-at-350-billion-valuation).

В Португалии экономическая выгода от foreign RSU обычно является Category A employment income при vesting; последующий рост после vesting — capital gain при продаже. [Doutor Finanças, 28 апреля 2026](https://www.doutorfinancas.pt/literacia-financeira/stock-options-rsu-e-espp-como-declarar-no-irs-acoes-atribuidas-pela-empresa/). Для PPU/phantom cash settlement классификация зависит от документов; нельзя заранее предполагать 28% capital-gains rate. Возможен неприятный сценарий: IRS возникает при vesting, а продать частные акции для оплаты налога нельзя.

---

## 4. Семейный benchmark

Официальные fee schedules показывают:

- CAISL 2026/27: например, grades 3–5 — €15,632 tuition в год до дополнительных сборов: [CAISL, июль 2026](https://www.caislisbon.org/join-us/fee-schedule).
- International Sharing School grade 5 — €18,510: [2026/27 schedule](https://www.sharingschoollisbon.pt/admissions/tuition-fees).
- Более дешёвый Lycée Français: примерно €5,112–6,214 на ребёнка в 2025/26, но это французская система, а не сопоставимая англоязычная international-school product: [LFIP schedule; документ опубликован до середины 2025 — возможно устарел](https://www.lfip.pt/sites/default/files/2025-2026%20TARIF%C3%81RIO%20FRPTEN.pdf).

Июльская оценка обычных расходов семьи из четырёх человек — €4,293/month; жильё 85 м² в дорогой зоне — €2,214/month: [Expatistan, июль 2026](https://www.expatistan.com/cost-of-living/lisbon). Для шести человек и подходящего T4 разумно не масштабировать это линейно, а заложить €55–75k обычных расходов до школы.

Отсюда:

| Сценарий школы | Школа ×4 | Жизнь + жильё + спорт | Требуемый семейный net |
|---|---:|---:|---:|
| Lycée/другая субсидируемая система | €22–30k | €55–70k | ≈€80–105k |
| Средняя англоязычная international | €50–65k | €60–75k | ≈€115–140k |
| CAISL/дорогая школа, старшие классы и fees | €65–90k | €60–75k | ≈€130–165k |

Следовательно:

- $150k gross: не закрывает четыре англоязычные international-school places.
- $200k gross: закрывает обычную жизнь плюс недорогую школьную систему; дорогую international — нет.
- $250k contractor: погранично закрывает средний вариант; employee без IFICI — обычно нет.
- $300k cash contractor либо $300–350k employee cash: наиболее устойчивый ordinary-tax сценарий.
- Большой «TC», где $150–250k приходится на private equity, не равен cash-flow для tuition.

---

## 5. Поездки на on-sites

| Паспорт | Статус | Что допустимо |
|---|---|---|
| Португальский или другой VWP-eligible паспорт | ESTA/VWP, до 90 дней | Meetings, consultations, contract negotiations, конференции, семинары — те же substantive ограничения, что B‑1 |
| Только PT residence permit, паспорт не-VWP страны | Обычно B‑1 visa | Та же ограниченная деловая деятельность; residence permit сам по себе не создаёт ESTA eligibility |
| Третья страна с VWP-паспортом | ESTA, если нет индивидуальных disqualifiers | Решает гражданство/паспорт, не место проживания |
| Канадский паспорт | Обычно отдельное безвизовое B‑1 admission правило | Всё равно нельзя выполнять local productive employment |
| US citizenship/green card | Не ESTA/B‑1 | Иные правила; US citizens также имеют worldwide US filing obligations |

Официально разрешены meetings, консультации, переговоры, conferences и independent research, но не gainful employment в США: [State Department B‑1 fact sheet, июль 2026](https://travel.state.gov/content/travel/en/us-visas/business/b-1-fact-sheet.html). VWP допускает те же business purposes: [State Department VWP](https://travel.state.gov/content/travel/en/us-visas/tourism-visit/visa-waiver-program.html). ESTA — только разрешение приехать к границе, не гарантия admission: [CBP](https://www.cbp.gov/travel/international-visitors/esta?language=pt).

Пограничная линия:

- допустимо: offsite, planning, onboarding meetings, training, conference, консультации;
- рискованно: неделями сидеть за обычным production backlog как часть US team, писать/деплоить код, обслуживать US клиентов на месте;
- особенно плохо: говорить, что едете «работать в офис», не имея work-authorized status;
- US salary source сам по себе не единственный тест, но продуктивная работа на территории США — главная проблема.

Следует иметь письмо с PT employment/contractor status, датами, agenda, возвратным билетом и формулировкой, что поездка не включает US local employment. Публичных доказательств, что перечисленные labs систематически выдают B‑1 letters remote-PT staff, не найдено.

Если позднее нужен переезд: O‑1 реалистичнее H‑1B для человека с существенными публикациями, critical OSS, press, judging и доказуемым original contribution; H‑1B обычно зависит от cap lottery; L‑1 требует не менее года работы за рубежом в qualifying related entity и поэтому невозможен из чистого contractor arrangement.

---

## 6. Промежуточная лестница дохода

Ранжирование учитывает деньги, доступность из Лиссабона и вероятность приблизить lab offer.

| Ранг | Инструмент | Сумма и статус на 1 августа 2026 | Как входить | Конверсия в lab offer |
|---:|---|---|---|---|
| 1 | Anthropic Fellows | $3,850/week, около $65k за четыре месяца; compute около $15k/month | Текущая заявка для cohorts с июля 2026 и далее: [Anthropic posting](https://job-boards.greenhouse.io/anthropic/jobs/5023394008) | **Высокая**, потому что работа и mentorship непосредственно внутри lab; оффер не обещан |
| 2 | Senior expert/evals через Mercor | Advertised average $81/h; senior domain experts до $200/h; текущий chemistry-PhD пример $65–70/h | Профиль и matching: [Mercor Experts, июль 2026](https://www.mercor.space/experts/); внешний разбор rates: [Time, 1 октября 2025](https://time.com/7322386/ai-mercor-professional-tasks-data-annotation/) | **Средняя-низкая**: реальный frontier-lab work, но lab часто не раскрывается и conversion path отсутствует |
| 3 | Handshake AI Fellowship/evals | $30–125/h, 5–20 h/week, projects 2 недели–3+ месяцев, contractor | Подаваться на конкретные opportunities; [официальный FAQ, 16 января 2026](https://support.joinhandshake.com/hc/en-us/articles/32264709473303-Handshake-AI-Fellowship-Overview-FAQs-for-EDU-Partners) | **Низкая-средняя**; полезен конкретный eval artifact, но Handshake, а не lab, является contractor |
| 4 | Прямой DevRel/AI-engineering contract у AI-adjacent startup | Только по опубликованному rate/band конкретной вакансии; общего достоверного 2026 числа нет | LangChain, W&B, Modal, Replicate, Val Town, Continue: искать worldwide/EMEA и предлагать scoped SDK/eval/integration deliverable | **Средняя**, если результат становится production/Open Source reference |
| 5 | Sovereign Tech Fellowship | Freelance 3–12 месяцев, 6–32 h/week, negotiated market rate; employee €64–82k FTE | Раунд закрыт 6 апреля 2026; ожидаются последующие recruitments: [официальная страница, обновлено 17 апреля 2026](https://www.sovereign.tech/programs/fellowship) | **Низкая-средняя** для lab, высокая для укрепления maintainer credentials |
| 6 | NLnet | Размер устанавливается proposal/MoU; без подтверждённого award нельзя считать доходом | Deadline 1 августа 2026, 12:00 CEST касался только GNU Taler/Fediversity; regular call обещан после лета: [NLnet, проверено 1 августа 2026](https://nlnet.nl/propose/) | **Низкая**, если проект не связан с AI infrastructure/security |
| 7 | Sovereign Tech Fund project grant | Project-based funding; универсальной суммы текущий call не обещает | Подать подробный critical-infrastructure proposal через [Sovereign Tech programs, июль 2026](https://www.sovereign.tech/programs) | **Низкая-средняя** при model-serving/security/evals infrastructure |
| 8 | GitHub Sponsors / Open Collective | Recurring donations; надёжного ожидаемого диапазона нет | Включить fiscal-host/sponsor tiers и привязать к maintainer roadmap | **Низкая**; это revenue signal, но не hiring funnel |
| 9 | GitHub Accelerator | В 2026 публичная страница всё ещё в основном описывает cohort 2024; текущая гарантированная сумма/deadline не найдены | Следить за [официальной application page](https://accelerator.github.com/application) | **Низкая-средняя**, если программа вновь открывается |
| — | Claude Corps | $85k/year, deadline был 17 июля 2026 | Не подходит: требуется менее двух лет full-time experience и US-oriented cohort: [официальный FAQ, июль 2026](https://www.anthropic.com/claude-corps/fellow) | Для этого профиля **нулевая** |
| — | Surge/Scale | Публично верифицируемого 2026 contributor rate для данного профиля не найдено | Только конкретная письменная offer rate и оплачиваемый assessment | Не оценивать как стабильный доход |
| — | OpenAI/xAI/Mistral grants | Подходящей открытой individual-builder программы с подтверждённой суммой и 2026 deadline не найдено | Не включать в cash-flow forecast до появления официального call | Неизвестна |

---

## 7. Три предположения brief, оказавшиеся неверными

1. **«Эти компании routinely employ people who are not in the office» не означает “можно жить в любой стране”.**  
   Вакансии систематически различают Remote-US, country-specific remote и перечисленные international offices. Anthropic прямо требует регулярных приездов; OpenAI FDE использует конкретные EU hubs; Perplexity публикует international-local-market policy.

2. **US compensation band не переносится автоматически в Лиссабон.**  
   Perplexity прямо заявляет, что USD range только для US-based positions, а international pay устанавливается по local market. Для остальных labs доказательств обещания US band сотруднику в Португалии не найдено. Поэтому US TC нельзя использовать как Lisbon offer forecast.

3. **EOR не является нейтральной административной оболочкой.**  
   Он превращает US offer в португальскую занятость: местные payroll taxes, benefits и трудовое право; 23.75% employer SS увеличивает employer cost. Он также может помешать IFICI, если формальным работодателем оказывается обычная EOR-компания, а не qualifying R&D/innovation entity. Кроме того, EOR не гарантирует участие в американском equity plan.

Главный переговорный показатель здесь — не headline TC, а отдельно зафиксированные **cash base, employment country, gross-vs-employer-cost definition, equity eligibility, tender eligibility и travel expectations**. Без этих шести пунктов «$300k offer» экономически не определён.
