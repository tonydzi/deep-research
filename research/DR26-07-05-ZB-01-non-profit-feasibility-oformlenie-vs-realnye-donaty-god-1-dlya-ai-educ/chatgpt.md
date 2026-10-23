# DR26-07-05-ZB-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: ChatGPT Pro Deep Research (report re-printed as chat message; original in DR widget) -->
<!-- topic: DR26-07-05-ZB-01 Non-profit feasibility -->
<!-- date: 2026-07-05 ~03:30, collected by Max (Claude Code, HP17) verbatim via blob-download -->
<!-- origin: приватный чат -->

# DR26-07-05-ZB-01 — Non-profit feasibility: AI-education школа

**Тема:** оформление non-profit vs неформальный open-source проект для бесплатного AI-образования.  
**Контекст:** соло-фаундер, PhD, экс-крипто, Лиссабон/Португалия, налоговый резидент Португалии, команда 3 человека, денег на счёте компании $0, личный резерв ограничен, уже есть живой open-source образовательный проект про Claude Code / AI-агентов / “открытый второй мозг”.

Это не юридическая и не налоговая консультация. Это decision memo для выбора маршрута и бюджета. Все “expected donations” ниже — **planning ranges**, не обещание; они зависят от аудитории, воронки, публичного trust, регулярности контента, конкретных LOI/грантов и способности продавать спонсорство.

---

## 1. Короткий вердикт

**Сейчас собственный non-profit открывать не стоит.** При $0 на счёте, ограниченном личном резерве и раннем build-in-public проекте юридическое оформление почти наверняка съест больше времени/денег, чем принесёт в первый год.

**Маршрут №1 для вашего кейса:**  
**сначала принимать деньги максимально дёшево как open-source project** через GitHub Sponsors / Ko-fi / прямые sponsor invoices, **параллельно подать на fiscal sponsorship**, если нужен tax-deductible статус, гранты или фондовые доноры.

Практически:

1. **Сразу:** GitHub Sponsors + Ko-fi + публичная страница бюджета + sponsor deck.
2. **Через 2–4 недели:** подача в fiscal host:
   - **HCB / Hack Club Bank** — если нужен 501(c)(3)-host, foundation grants, payroll, tax-deductible donation workflow.
   - **Open Source Collective** — если проект позиционируется прежде всего как open-source infrastructure/community и вам важны прозрачный ledger, open-source-доноры, GitHub/Open Collective экосистема; но OSC — 501(c)(6), не то же самое, что charity 501(c)(3). OSC прямо пишет, что его структура — 501(c)(6), а не 501(c)(3), и объясняет, что IRS не всегда считает FOSS сам по себе public benefit. citeturn756564view0
3. **Собственный US 501(c)(3):** только после anchor donor / grant LOI / pipeline на **$50k–$100k+ годовых**.
4. **Португальская associação:** только если конкретный EU/Portugal grant, университет, муниципалитет или партнёр требует локальную португальскую legal entity.
5. **Эстонское MTÜ:** для этого кейса почти точно overengineering: не даёт сильного преимущества для US donors, не решает tax-deductibility, добавляет cross-border tax/accounting слой для португальского резидента.

---

## 2. Главная таблица маршрутов

Оценки затрат включают **год-1 cash out**: регистрация, пошлины, базовый юрист/CPA/contabilista/accountant, банковский и отчётный setup. Не включают зарплату фаундера.

**Hassle 1–5:**  
1 = почти no-brainer; 5 = много юр./налог./governance/банковского трения.

| Маршрут | Срок до легального приёма денег | Tax-deductible для US donors? | Cash cost год-1 | Поддержка с года-2 | Founder time | Expected gross donations год-1, pess / base / optimistic | Hassle | Вывод |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| **A. US nonprofit corp + 501(c)(3), удалённо** | **1–3 недели** до entity/payment setup, но **IRS recognition**: планировать **1–3 мес. для 1023-EZ**, **3–9+ мес. для full 1023** | Да, после 501(c)(3) determination; возможны нюансы по retroactivity | **DIY EZ:** ~$500–$1.5k. **Реалистично с юристом/CPA:** ~$3.5k–$10k | ~$300–$1.5k/год при tiny 990-N; ~$1.5k–$5k+ при 990-EZ/CPA/state charity filings | **60–120 ч** | **$1k–$5k / $8k–$25k / $50k–$150k** | **5/5** | Не сейчас. Оправдано после серьёзного US grant/donor pipeline |
| **B. Portugal associação sem fins lucrativos** | **1–3 недели** регистрация, **2–6 недель** с банком/payment/accounting | Нет для US donors. Португальские льготы не автоматические; зависят от статуса/признания | **€600–€2.5k** basic; **€1.5k–€5k** с custom statutes/legal | **€600–€1.8k/год** basic; больше при employees/VAT/grants | **35–80 ч** | **$0.5k–$3k / $4k–$12k / $25k–$75k** | **3–4/5** | Имеет смысл только под Portugal/EU grant или локального институционального партнёра |
| **C. Estonia MTÜ через e-Residency** | e-Residency: **30 дней identity check + 2–5 недель доставка карты**, потом регистрация/банк: дни–недели | Нет для US donors | **€500–€1.5k DIY-ish**, **€1.5k–€3.5k** через provider | **€500–€1.8k/год** | **30–70 ч** | **$0.5k–$3k / $3k–$10k / $15k–$50k** | **3/5** | Не рекомендую: мало donor upside, есть cross-border complexity |
| **D. Fiscal sponsorship без своего юрлица: HCB / OSC / аналоги** | **Несколько дней – несколько недель** после acceptance | **HCB:** да, 501(c)(3)-host. **OSC:** не charity 501(c)(3), а 501(c)(6) host | **$0 fixed**; HCB **7% revenue**; OSC **10% incoming funds + processor fees** | **$0 fixed**, variable fee | **8–25 ч** | **$1k–$5k / $8k–$20k / $35k–$100k** | **2/5** | Лучший legal route, если нужны grants/tax receipts без своего юрлица |
| **E. Не оформляться: GitHub Sponsors / Ko-fi / direct sponsorship** | **1–7 дней** | Нет | **$0 fixed**; platform/payment fees | $0 fixed | **4–10 ч** | **$0.5k–$3k / $3k–$8k / $15k–$30k** | **1/5** | Лучший старт прямо сейчас |

---

## 3. Почему вердикт “пока не оформлять”

### 3.1. Legal entity сама по себе не создаёт donor demand

Для маленького AI/tech education проекта основной bottleneck в год-1 — не отсутствие юрлица, а:

- нет проверенной регулярной donor conversion funnel;
- нет anchor donor / LOI;
- нет доказанных outcomes: completion rate, active learners, stars/forks, повторные пользователи, кейсы трудоустройства/продуктивности;
- нет corporate sponsor package;
- нет grant narrative и reporting capacity.

Юрлицо помогает, когда донор уже готов дать деньги, но ему нужен compliance-wrapper. На старте оно часто становится **административным налогом на фаундера**.

### 3.2. Break-even против fiscal sponsorship

Формула:

```text
break_even_gross_donations =
  (setup_cash + annual_fixed_admin + founder_hours * hourly_value)
  / fiscal_sponsor_fee_rate
```

Плановая оценка для US 501(c)(3):

```text
setup_cash + admin ≈ $6,000
founder time: 80 ч * $50/ч = $4,000
total economic cost ≈ $10,000

vs 10% fiscal host:
break-even ≈ $100,000 gross donations

vs 7% HCB:
break-even ≈ $143,000 gross donations
```

**Cash-only break-even** ниже: при $3.5k–$10k cash setup собственный US 501(c)(3) “окупается” против 10% fiscal fee при **$35k–$100k gross donations**. Но это плохая метрика: founder time и задержка grant/donation execution для соло-фаундера с ограниченным резервом критичнее.

**Реальный порог:** оформлять собственный US 501(c)(3) имеет смысл, когда уже есть **$50k–$100k+ годового pipeline** или подписанный/почти подписанный grant/sponsor LOI, который требует собственного 501(c)(3).

### 3.3. Пороговые условия для оформления

Открывать своё юрлицо стоит, если выполнено хотя бы одно:

| Trigger | Что делать |
|---|---|
| Signed LOI / grant ≥ **$25k–$50k**, где требуется юридическое лицо | Делать fiscal sponsorship или локальную entity под требования гранта |
| Recurring donations ≥ **$3k–$5k/month** минимум 6 месяцев | Рассмотреть собственное юрлицо, если fiscal fee становится дорогим |
| US donor/foundation pipeline ≥ **$100k/year** | US 501(c)(3) начинает иметь смысл |
| EU/Portugal grant требует Portuguese legal person | Portugal associação |
| Нужно легально платить founder salary **>$30k/year** из пожертвований | Fiscal sponsor с payroll или собственный 501(c)(3)/associação с governance |
| Нужны контракты, сотрудники, грантовая отчётность, IP ownership | Собственная entity или fiscal host с contract support |

---

## 4. Маршрут A — US 501(c)(3) удалённо

### 4.1. Что нужно сделать

Типовой путь:

1. Выбрать штат и registered agent.
2. Зарегистрировать nonprofit corporation.
3. Получить EIN.
4. Принять bylaws, conflict-of-interest policy, initial board resolutions, minutes.
5. Открыть банковский счёт / Stripe / PayPal / donation platform.
6. Подать IRS Form 1023-EZ или full Form 1023.
7. Вести ежегодную отчётность: Form 990-N / 990-EZ / 990, state filings, charitable solicitation registrations при необходимости.

IRS требует подавать Form 1023 applications электронно через Pay.gov; для 501(c)(3) используются Form 1023-EZ или Form 1023. citeturn694752view0 EIN можно получить напрямую от IRS бесплатно, но онлайн-инструмент ограничен организациями с principal place of business в США/US territories и responsible party с SSN/ITIN; при foreign principal place нужно использовать другие способы подачи, что добавляет трение. citeturn860043view0

### 4.2. Сроки

Плановые сроки для вашего кейса:

| Этап | Срок |
|---|---:|
| Incorporation + registered agent | 2–10 рабочих дней |
| EIN | minutes online, но для foreign/nonresident setup может быть дольше |
| Bank / Stripe / PayPal nonprofit verification | 1–6 недель |
| 1023-EZ | планировать 1–3 месяца |
| Full 1023 | планировать 3–9+ месяцев |

Юридически принимать money как entity можно до финального IRS determination, но **обещать donors полноценный 501(c)(3) receipt без аккуратного юридического framing нельзя**. Для серьёзных donors это practical blocker.

### 4.3. Стоимость

| Компонент | DIY / low | Реалистично |
|---|---:|---:|
| State nonprofit incorporation | $50–$250 | $50–$250 |
| Registered agent | $50–$200/год | $100–$300/год |
| EIN | $0 | $0 |
| IRS Form 1023-EZ user fee | ~$275 | ~$275 |
| IRS full Form 1023 user fee | ~$600 | ~$600 |
| Юрист: bylaws, board, 1023 review | $0–$1k | $2k–$7.5k |
| CPA/accountant year-1 | $0–$500 | $500–$2k |
| Donation/payment setup | $0–$500 | $0–$1k |
| **Итого год-1** | **$500–$1.5k** | **$3.5k–$10k** |

IRS 990-N доступна большинству small exempt organizations с gross receipts normally $50k or less; для новых организаций IRS считает первый год normally ≤$50k, если получено/pledged ≤$75k. Это снижает отчётную нагрузку на микро-уровне, но не отменяет board/minutes/accounting/governance. citeturn649803view0

### 4.4. Плюсы

- Максимальная понятность для US foundations, CSR donors, major donors.
- Tax-deductible donations для US donors.
- Можно строить долгосрочный brand trust.
- Можно нанимать, платить зарплату, владеть IP, подписывать гранты.

### 4.5. Минусы для вашего кейса

- Медленно и дорого относительно ожидаемых year-1 donations.
- Требуется board с реальной fiduciary role, а не “друзья для галочки”.
- Founder salary и founder-owned consulting рядом с 501(c)(3) требуют conflict governance.
- Банковский KYC для нерезидента/Portugal resident/ex-crypto background может затянуться.
- Public compliance: Form 990, policies, minutes, grant restrictions.
- Фаундер будет тратить attention на governance вместо audience/donor traction.

### 4.6. Когда выбирать US 501(c)(3)

Выбирать только если:

- есть US donor/foundation pipeline **$100k+**;
- грантодатель прямо требует собственный 501(c)(3), а fiscal sponsorship не подходит;
- вы готовы собрать board и жить в режиме nonprofit governance;
- проект — не просто open-source tooling, а явно charitable/educational public benefit с measurable outcomes.

---

## 5. Маршрут B — Portugal associação sem fins lucrativos

### 5.1. Что это даёт

Portugal associação — локальная nonprofit legal entity. Она может быть хорошей для:

- португальских банков и local credibility;
- муниципальных/университетских партнёров;
- EU/Portugal grants;
- локальных программ digital skills / AI literacy;
- найма или contracting в Португалии.

Португальский официальный сервис позволяет создать associação “num só pedido e num só balcão”; при регистрации выдают certificate/statutes, код доступа к electronic corporate card и Social Security number association. citeturn649803view1

### 5.2. Минимальные требования и стоимость

Официально в pedido da constituição должны присутствовать минимум **2 associados**; если выбирается модель statutes с nomeação de órgãos sociais, могут потребоваться минимум **9 associados** — по 3 на каждый орган. RCBE нужно сделать в течение **30 дней** после constituição. Стоимость для “outros tipos de associações” — **€300**. citeturn649803view2

Если у association есть contabilidade organizada, нужен certified accountant; если нет, declaração de início de atividade можно подать сразу или в течение 90 дней. citeturn649803view1

Плановая стоимость:

| Компонент | Оценка |
|---|---:|
| Official registration | €300 |
| Estatutos / legal review | €0–€1,500 |
| Certified accountant setup | €200–€600 |
| Annual accounting/basic filings | €600–€1,800 |
| Bank/payment setup | €0–€500 |
| **Итого год-1** | **€600–€2.5k basic**, **€1.5k–€5k custom/legal-heavy** |

### 5.3. Плюсы

- Локально логично: фаундер — Lisbon/Portugal resident.
- Проще объяснить Portuguese/EU partners.
- Может быть оптимальным, если идти в digital skills grants, local education pilots, universidade/município.
- Не нужно US remote corporate complexity.

### 5.4. Минусы

- Не решает US tax-deductible donations.
- Для международной developer-аудитории donor conversion не лучше, чем GitHub/Ko-fi/Open Collective.
- Local accounting, Portuguese compliance, general assembly, minutes.
- Банки могут задавать вопросы по international donations, crypto background, zero initial funds.
- Для US foundations foreign entity часто хуже, чем US fiscal sponsor.

### 5.5. Когда выбирать Portugal associação

Выбирать, если есть конкретный Portugal/EU opportunity:

- грант/партнёр требует Portuguese legal person;
- университет/муниципалитет готов funding/pilot;
- нужен официальный local education статус;
- вы готовы вести governance на португальском контуре.

Для текущего open-source AI-education проекта без committed local grant — **не первый выбор**.

---

## 6. Маршрут C — Estonia MTÜ через e-Residency

### 6.1. Что это даёт

Estonian MTÜ — nonprofit association. Через e-Residency можно digital-first администрировать entity. Но для вашего кейса это выглядит как “neat legal hack”, а не fundraising advantage.

e-Residency application стоит **€150**, онлайн-заявка занимает около **30 минут**, identity check — **30 дней**, card delivery — **2–5 недель**. citeturn649803view3 У e-Residency company route также есть recurring admin costs: contact person/legal address **€200–€400/year**, bank account setup **€0–€200**, а сама e-Residency не меняет personal tax residency; cross-border tax надо анализировать отдельно. citeturn592071view0

### 6.2. Стоимость

| Компонент | Оценка |
|---|---:|
| e-Residency | €150 |
| MTÜ registration / provider help | €100–€800 |
| Legal address / contact person if needed | €200–€400/year |
| Accounting / annual report | €300–€1,200/year |
| Bank/payment setup | €0–€300 |
| **Итого год-1** | **€500–€1.5k DIY-ish**, **€1.5k–€3.5k provider-heavy** |

### 6.3. Плюсы

- Digital admin.
- EU legal entity.
- Может быть удобно для internet-native governance.

### 6.4. Минусы

- Нет US 501(c)(3) tax-deductible benefit.
- Для Portugal resident founder появляется cross-border management/tax question.
- Не даёт сильного сигнала US foundations.
- Не лучше Portugal association для EU grants.
- Не лучше fiscal sponsorship для open-source donations.

### 6.5. Вывод по Estonia MTÜ

**Не рекомендую.** Вы получите ещё одну foreign entity, но не получите главный fundraising unlock.

---

## 7. Маршрут D — fiscal sponsorship без своего юрлица

Это лучший legal wrapper для “маленький open-source / tech education project, денег мало, нужен trust и ability принимать гранты”.

### 7.1. HCB / Hack Club Bank

HCB позиционирует fiscal sponsorship как 501(c)(3) legal entity, bank account, automatic taxes/accounting and software. Они указывают, что через HCB можно получать foundation grants с tax-deductible 501(c)(3) status, operate globally through a US legal entity, file taxes/Form 990 automatically, pay team members through payroll, accept donations through custom forms. Fee — **7% of revenue**, при этом они заявляют no legal fees, no startup fees, no transaction fees, no card issuing fees, no subscription fees, no check deposit fees, no credit card processing fees. citeturn968850view2

**Плюсы HCB:**

- 501(c)(3)-host без своего юрлица.
- Нет fixed startup cost.
- Payroll встроен.
- Подходит, если нужно принимать foundation grants.
- Быстрее и дешевле, чем свой 501(c)(3).

**Минусы:**

- Acceptance не гарантирован.
- Нужно соответствовать mission/risk policy host.
- Вы не полностью автономны: host контролирует compliance.
- Donor restrictions and payroll rules надо соблюдать.
- Если revenue станет большим, 7% fee станет заметным.

### 7.2. Open Source Collective

OSC — fiscal host для open-source projects. Они прямо пишут, что OSC welcomes open-source projects globally, in any language, если проект clearly connected to open source; software project должен показать legitimacy/activity/usage, maintainer access, open-source license, organizational repository, ideally two or more admins; GitHub projects могут пройти auto/manual review в течение few business days. citeturn313478view0

Fee OSC: **10% host fee on incoming funds**, без setup fees, balance minimums or recurring charges; payment processors such as Stripe/PayPal/Wise берут свои комиссии. citeturn968850view1

Важно: OSC — **501(c)(6)**, а не 501(c)(3). OSC сам объясняет, что это member-based nonprofit for open source ecosystem, и что IRS generally does not consider open source software public benefit solely because it is FOSS. citeturn756564view0

**Плюсы OSC:**

- Отличный fit для open-source content/tools.
- Прозрачный бюджет и ledger.
- Хорошо читается для developer/open-source аудитории.
- Быстрый старт, если repo/license/governance в порядке.
- Можно платить contributors/maintainers; OSC может поддерживать employment/payroll через PEO/EOR, если есть бюджет and compliance setup. citeturn313478view1

**Минусы:**

- Не заменяет 501(c)(3), если донору нужен charitable tax deduction.
- 10% + processor fees.
- Требует open-source fit и governance hygiene.
- Для grants в “charitable education” HCB/501(c)(3) sponsor может быть лучше.

### 7.3. Что случилось с Open Collective Foundation

Старый “Open Collective Foundation” как 501(c)(3)-fiscal sponsor — не тот default route в 2026. Open Collective Foundation announced winding down/dissolution of its fiscal sponsorship by end-2024; Open Collective platform moved into the OFiCo / Open Finance Consortium ecosystem, while fiscal hosts such as OSC remain separate hosts. citeturn735711search0

### 7.4. Вывод по fiscal sponsorship

Для вашего кейса:

- **Если главная задача — open-source credibility и мелкие/корпоративные dev sponsorships:** подавайте в **Open Source Collective**.
- **Если главная задача — grants, charitable receipts, foundation donors, payroll:** подавайте в **HCB** или другой 501(c)(3) fiscal sponsor.
- **Если нужно быстро принимать деньги прямо сейчас:** GitHub Sponsors / Ko-fi, а fiscal sponsorship — параллельно.

---

## 8. Реальные деньги в год-1

### 8.1. Сразу: честная проблема с “медианой”

Публичной чистой базы “median first-year donations for tiny new AI/tech education nonprofits” нет. То, что видно публично, сильно biased:

- успешные проекты публикуют данные чаще;
- провалившиеся микропроекты исчезают;
- гранты lumpier than donations: median часто **$0**, но accepted projects получают $25k–$100k+;
- freeCodeCamp-like examples — survivorship/outlier bias.

Поэтому ниже — **операционные диапазоны**, а не статистическая медиана из единого dataset.

### 8.2. Сводная оценка year-1 fundraising

Для вашего конкретного проекта — AI education + open-source content/tools + build-in-public + growing audience, но без cash reserve и без committed donor pipeline:

| Источник денег | Pessimistic | Base | Optimistic | Комментарий |
|---|---:|---:|---:|---|
| **Индивидуальные донаты аудитории, no tax deduction** | $0.5k–$3k | $3k–$8k | $15k–$30k | GitHub/Ko-fi/Patreon-like. Работает, если есть регулярный контент и clear ask |
| **Индивидуальные донаты через fiscal host** | $1k–$5k | $5k–$15k | $20k–$50k | Trust выше, но 7–10% fee. Tax receipt важен не всем мелким donors |
| **Гранты** | $0 | $0–$10k | $25k–$75k | Median для нового проекта — $0; upside через Fast Forward / AI philanthropy / EU partner |
| **Corporate sponsorships** | $0 | $2.5k–$10k | $15k–$75k | Часто реалистичнее грантов, если есть audience + sponsor deck |
| **Итого без anchor donor** | $1k–$5k | $5k–$15k | $25k–$50k | Это baseline для “маленький живой проект” |
| **Итого с one grant / anchor sponsor** | $5k–$15k | $25k–$75k | $100k–$150k | Только если есть accepted accelerator, LOI, company sponsor |

**Base-case median-like expectation:** **$5k–$15k gross в год-1**.  
**Base-case grants:** **$0**.  
**Главный upside:** не “оформление non-profit”, а **один anchor sponsor / accelerator / grant**.

---

## 9. Источник 1 — individual donations

### 9.1. Что реально ожидать

Для open-source education проекта individual donations почти всегда маленькие и требуют:

- регулярного CTA;
- публичной финансовой цели;
- monthly tiers;
- прозрачного бюджета;
- “why now” campaign;
- social proof.

Типичный early pattern: много $3–$20/month supporters, редкие $100–$500 one-offs.

### 9.2. Benchmark: The Odin Project

The Odin Project — релевантный benchmark: бесплатный open-source coding curriculum, hosted by Open Source Collective. На их Open Collective странице в июле 2026 видно: fiscal host Open Source Collective, total raised **$18,171.23**, today’s balance **$8,555.66**, estimated annual budget **$6,072.15**; текущие incoming transactions включают small recurring contributions типа $1–$5. citeturn809624view0

Это sobering benchmark: даже известный бесплатный coding curriculum не обязательно собирает сотни тысяч от индивидуальных donors через open-source crowd-donation механику.

### 9.3. Benchmark: freeCodeCamp

freeCodeCamp — большой outlier, а не год-1 benchmark. Он started in 2014, является 501(c)(3) public charity, заявляет free education mission and donor-supported status. citeturn809624view1 В публичных источниках видны большие поздние масштабы, например revenue $4.28M in 2022, но я не нашёл надёжной публичной цифры first-year fundraising, поэтому **не использую freeCodeCamp как численный год-1 benchmark**. citeturn982017search0

### 9.4. Вывод

Индивидуальные донаты в год-1 для вашего проекта:

```text
Pessimistic: $0.5k–$3k
Base:        $3k–$8k без fiscal host; $5k–$15k с fiscal host и хорошим CTA
Optimistic:  $15k–$50k при сильной аудитории / viral campaign / one large donor
```

---

## 10. Источник 2 — гранты

### 10.1. Median grant для нового micro-nonprofit

**Медиана: $0.**  
Причина простая: grants are binary/lumpy. Большинство новых проектов ничего не получают в год-1; small accepted subset получает $25k+.

### 10.2. Fast Forward

Fast Forward — один из самых релевантных маршрутов для tech/AI nonprofits. Их accelerator длится 3 месяца и даёт **$25K+ unrestricted funding**, mentorship и fundraising support. Они прямо пишут, что flagship accelerator designed for early-stage tech and AI-powered nonprofits, с hard requirements: nonprofit business model и minimum viable product. citeturn810799view0

**Fit для вас:** хороший, но не “base-case money”. Нужно:

- nonprofit model или fiscal sponsorship;
- MVP уже есть;
- clear social impact, not just “AI productivity content”;
- pitch deck;
- evidence of usage/impact.

**Оценка:**

```text
Вероятность год-1: низкая/средняя, если подать качественно
Размер при успехе: $25k+
Base-case expected value: $0–$5k
```

### 10.3. OpenAI philanthropy

OpenAI launched a **$50M fund** in 2025 for nonprofits/community organizations focused on areas including education, healthcare, economic opportunity and community organizing. citeturn496833news0 В 2026 OpenAI Foundation pledged **$1B in grants over the next year**, focusing on AI benefits, health/life sciences, economic resilience, mental health and societal impacts. citeturn496833news2

**Fit для вас:** возможно, но не считать base-case. Это high-competition strategic philanthropy. Маленький Lisbon-based open-source education project без институционального партнёра и измеримого impact likely low priority.

**Оценка:**

```text
Median: $0
Optimistic: $25k–$100k, если есть clear AI literacy / workforce resilience / public benefit pilot
```

### 10.4. Anthropic philanthropy / Claude Corps

Anthropic announced Claude Corps in June 2026 as a **$150M initiative** placing 1,000 fellows into 400 US nonprofits; reporting also says host organizations receive a $10,000 grant and free Claude credits. citeturn496833news1

**Fit для вас:** слабый как cash grant, потому что программа US-nonprofit/fellowship-oriented. Может быть indirectly relevant если через US fiscal sponsor / US partner.

### 10.5. Schmidt Futures / Schmidt Sciences

Schmidt Sciences / AI2050-type programs are serious AI/science philanthropy, but typically oriented toward research, fellows, science safety, institutional proposals, not small general AI education projects. Public reporting describes Schmidt Sciences as supporting AI2050 and other AI/science programs with large commitments. citeturn496833search3

**Fit для вас:** только если проект переформулирован как research/AI safety/education infrastructure with measurable public-good impact.

### 10.6. Mozilla

Mozilla historically matters in open internet/open-source funding, but current grant availability is episodic and not a reliable base-case. Public reporting in 2024 described Mozilla Foundation layoffs and cuts to advocacy/global programs, so не закладывать Mozilla как year-1 funding expectation. citeturn365618news0

### 10.7. EU / Portugal digital skills

Деньги в EU/Portugal ecosystem есть, но они обычно требуют consortia, local partners, procurement readiness, reporting and matching/admin capacity.

Digital Europe Programme supports AI, cybersecurity, advanced digital skills and broad deployment of digital tech; its overall budget is over **€8.1B** for the 2021–2027 framework. citeturn799987view2 Portugal also just launched Amalia, an open-source AI model funded with **€5.5M EU recovery funds**, with initial applications including public services and education assistants — это сигнал, что Portugal AI public-sector/digital education space is active. citeturn418381news0

**Fit для вас:** лучше, если сделать Portuguese partner route:

- universidade / polytechnic / município;
- “AI literacy for teachers / SMEs / public workers”;
- Portuguese-language curriculum;
- measurable cohorts;
- local associação or partner fiscal host.

**Оценка:**

```text
Median год-1: $0
Base with serious local partner: $5k–$25k
Optimistic: $25k–$75k
```

---

## 11. Источник 3 — corporate sponsorships

Для вашего проекта corporate sponsorships, вероятно, **реалистичнее грантов** в год-1.

Потенциальные спонсоры:

- AI devtools companies;
- coding agent startups;
- cloud/GPU/dev environment providers;
- bootcamps / cohort-based education platforms;
- AI consultancies;
- open-source infra vendors;
- companies hiring AI-native developers.

### 11.1. Почему спонсорства лучше грантов

- Не требуют 501(c)(3) в большинстве случаев.
- Можно продавать через for-profit invoice уже сейчас.
- Быстрее decision cycle.
- Есть понятный value exchange: logo, workshop, newsletter mention, repo/docs sponsorship, community call, benchmark/content series.
- Для AI-tools vendors ваша аудитория может быть commercially valuable.

### 11.2. Но важно не испортить nonprofit purity

Если вы хотите позже 501(c)(3), не превращайте “donation” в покупку рекламы без disclosure. Делайте отдельно:

- **Donation:** no substantial quid pro quo.
- **Sponsorship:** clearly disclosed, limited recognition.
- **Advertising/lead-gen/consulting:** for-profit invoice.

### 11.3. Реалистичные tiers

| Tier | Цена | Что дать |
|---|---:|---|
| Community backer | $500–$1,000 | logo on supporters page, monthly thank-you |
| Open-source sponsor | $2,500–$5,000 | logo, public roadmap mention, one educational post “supported by” |
| Founding sponsor | $10,000–$15,000 | 3–6 month support, workshop/webinar, case study, no editorial control |
| Institutional partner | $25,000+ | cohort / curriculum / grant-like partnership with reporting |

**Год-1 base:** 1–2 спонсора по $2.5k–$5k = **$2.5k–$10k**.  
**Optimistic:** 3–5 sponsors по $5k–$15k = **$15k–$75k**.

---

## 12. Founder salary: можно ли платить себе

Коротко: **да, но не “как владелец”, а как employee/contractor за реальную работу, по reasonable compensation, с governance and conflict controls.** Non-profit не запрещает зарплату. Non-profit запрещает private inurement / excess private benefit.

### 12.1. US 501(c)(3)

Можно платить founder salary, если:

- compensation reasonable относительно рынка;
- founder не сам себе утверждает зарплату;
- независимый board approves;
- есть comparability data;
- есть contemporaneous minutes;
- role, hours, deliverables documented;
- нет private inurement / excess benefit.

IRS 501(c)(3) framework требует charitable/educational purpose и запрещает inurement of earnings to private individuals; это основная legal risk-zone для founder salary. citeturn718426search0

Плановые salary ranges для micro-stage:

| Annual budget | Практически defensible founder comp |
|---:|---:|
| <$25k | $0 или символический stipend $1k–$5k |
| $25k–$50k | $5k–$20k, если founder выполняет core program work |
| $50k–$100k | $20k–$50k |
| $100k–$250k | $40k–$90k, если full-time и board-approved |
| >$250k | market-based salary possible, но нужна сильная governance |

Для Portugal resident founder зарплата/contractor income всё равно будет иметь Portuguese tax/social-security implications.

### 12.2. Portugal associação

Можно платить founder:

- как employee;
- как prestador de serviços / contractor;
- как reimbursed expense;
- но нельзя распределять profits.

Нужно:

- conflict-of-interest approval;
- minutes;
- market-rate justification;
- payroll/social security/tax compliance;
- clear separation от personal consulting.

Практически для год-1 micro-associação: не закладывать зарплату, пока нет recurring funding.

### 12.3. Estonia MTÜ

Можно платить за работу, но:

- не как distribution of profit;
- board/general assembly governance matters;
- если board/founder abroad, contact person/tax residency/management questions matter;
- Portugal tax residency founder всё равно critical.

Для вас это самый неудачный salary route: правовая оболочка digital, но tax reality Portuguese.

### 12.4. Fiscal sponsorship

**HCB:** заявляет built-in payroll for team members. citeturn968850view2  
**OSC:** hosted projects may be able to offer employment/benefits; OSC uses PEO/EOR and employment costs are paid from project budget. citeturn313478view1

Практически: fiscal sponsor — лучший путь для first salary, потому что payroll/compliance уже существует. Но salary должен быть:

- allowed by host;
- within project budget;
- consistent with donor restrictions;
- approved through host process;
- not self-dealing without review.

---

## 13. Совместимость с коммерцией

### 13.1. Можно ли параллельно вести paid AI consulting?

**Да.** Но нельзя смешивать деньги, обещания, IP, donors, clients и brand так, чтобы nonprofit стал lead-gen/субсидией для private business.

Для вашего кейса нормальная конструкция:

```text
Anton / LLC / personal consulting
  - paid AI consulting
  - private clients
  - commercial workshops
  - custom automation
  - invoices, VAT/tax, profit

Open-source education project / fiscal sponsored fund / nonprofit
  - free curriculum
  - open-source tools
  - public docs
  - community calls
  - scholarships/free cohorts
  - donations/grants/sponsorships
```

### 13.2. Допустимые структуры

| Структура | Как работает | Риск |
|---|---|---:|
| **Founder consulting отдельно + nonprofit/fiscal sponsored project** | Самый простой вариант. Разные счета, сайты, accounting, terms | Низкий/средний |
| **Nonprofit + sister LLC founder-owned** | LLC делает paid consulting; nonprofit делает free education | Средний |
| **Nonprofit owns LLC** | Сложнее; LLC может вести earned revenue | Высокий для старта |
| **For-profit donates % revenue to nonprofit** | Нормально, если без misleading claims | Средний |
| **Nonprofit contracts founder/LLC** | Можно, но только independent approval, market rate, recusal, minutes | Высокий |

### 13.3. Что нельзя

Нельзя:

- оплачивать founder consulting из donated funds без independent approval;
- говорить “donation” там, где donor покупает consulting/access/leads;
- отдавать nonprofit audience/IP private LLC бесплатно;
- использовать nonprofit volunteers для paid client delivery;
- обещать donor influence over curriculum/content;
- смешивать GitHub repos, paid proprietary code и nonprofit-funded open-source без license clarity;
- founder approves own salary/contract;
- использовать один бренд так, что donors думают, будто финансируют public education, а фактически субсидируют consulting funnel.

### 13.4. Как сделать безопасно

Минимальный пакет:

1. **Separate names or clear sub-brands.**
   - Example: “Open Second Brain Project” vs “Anton AI Consulting”.
2. **Separate bank/payment accounts.**
3. **Separate landing pages.**
4. **Public conflict statement.**
5. **Written IP policy.**
6. **Contributor license / content license.**
7. **Sponsor policy.**
8. **No private client deliverables funded by donations.**
9. **If nonprofit pays founder/LLC:** independent review + written scope + market quotes + minutes.

---

## 14. Геморрой-фактор: что убивает micro-nonprofits в первые 2 года

### 14.1. Board theater

Фаундер думает: “поставлю друзей в board”.  
Реальность: board должен утверждать salary, conflicts, budgets, major contracts, minutes. Плохой board либо ничего не делает, либо создаёт риск.

**Убийца:** founder wants control + nonprofit requires accountability.

### 14.2. Банковский KYC

Ваш профиль: Portugal resident, ex-crypto, international donations, $0 initial funds, US/foreign structures. Это не запрет, но для bank/compliance это больше вопросов:

- source of funds;
- expected donation volume;
- beneficial owners/controllers;
- sanctioned countries;
- crypto background;
- proof of address/tax residency;
- why US entity if founder in Portugal.

**Убийца:** entity готова, donation page готова, но bank/Stripe/PayPal держит account review.

### 14.3. Grant reporting

Грант — это не free money. Обычно нужны:

- budget categories;
- restricted funds;
- milestones;
- impact metrics;
- receipts;
- quarterly reports;
- final report;
- sometimes audit/financial review.

**Убийца:** фаундер получает $25k grant, но тратит 30–50 часов на reporting и не строит продукт.

### 14.4. Restricted funds

Если donor дал деньги “на teacher AI literacy cohort”, нельзя просто оплатить founder laptop or general consulting costs. Нужна accounting segmentation.

### 14.5. Founder salary optics

Даже reasonable salary может выглядеть плохо, если:

- нет board approval;
- donation page emotional;
- budget маленький;
- founder одновременно продаёт paid consulting;
- нет public budget.

### 14.6. Brand confusion

Один и тот же Telegram/Twitter/GitHub/website для free education и paid consulting может быть нормально, но только при disclosure. Иначе donors чувствуют bait-and-switch.

### 14.7. Compliance drift

Микро-nonprofit часто умирает не от одного штрафа, а от accumulation:

- забыли 990-N;
- забыли RCBE;
- не сделали minutes;
- нет expense policy;
- reimbursements without receipts;
- volunteers leave;
- founder burns out.

### 14.8. Legal setup вместо fundraising

Самый частый провал: 2–3 месяца “строим правильную структуру”, после чего выясняется, что donations всё равно $200/month.

---

## 15. Самый дешёвый способ принимать донаты уже сейчас

### 15.1. GitHub Sponsors

GitHub Sponsors подходит, если проект живёт на GitHub и аудитория developer-heavy.

Fees:

- personal account sponsorships: GitHub charges no fees;
- organization accounts: up to **6%**, split as 3% card processing + 3% GitHub service fee; invoiced billing may avoid card processing part. citeturn467032view4

**Use case:** recurring $5/$10/$25/month supporters, open-source credibility.

**Минус:** не tax-deductible charity donation, если нет fiscal host / nonprofit status.

### 15.2. Ko-fi

Ko-fi подходит для simple support page.

Fees:

- Ko-fi Free: **0% platform fee** on one-time tips/donations;
- **5%** on shop/memberships/monthly/commissions;
- PayPal/Stripe payment processing отдельно, около **3% + $0.30** depending on country/payment method. citeturn467032view5

**Use case:** non-dev audience, one-time support, simple CTA.

**Минус:** слабее для corporate/open-source credibility.

### 15.3. Open Collective / OSC

Если проект проходит open-source eligibility:

- no setup fees / no recurring charges;
- **10% host fee** on incoming funds;
- processor fees separately;
- transparent ledger. citeturn968850view1

**Use case:** open-source transparency, developer community, sponsor logos, public budget.

**Минус:** OSC — 501(c)(6), не 501(c)(3).

### 15.4. HCB

Если accepted:

- **7% revenue fee**;
- no legal/startup/transaction/card/subscription/check/credit card processing fees;
- 501(c)(3) legal entity, grants, payroll, Form 990 handled. citeturn968850view2

**Use case:** foundation grants, tax-deductible donation path, payroll, compliance.

### 15.5. Recommended immediate stack

Для вас:

```text
Now:
  GitHub Sponsors + Ko-fi
  + public budget page
  + sponsor deck
  + clear non-tax-deductible disclaimer

Parallel:
  Apply to HCB if you want 501(c)(3) receipts/grants/payroll
  Apply to OSC if open-source ecosystem fit matters more than 501(c)(3)

Later:
  Own entity only after $50k–$100k/year credible pipeline
```

Текст disclaimer для donation page:

> This project is not yet a registered charity. Contributions support open-source educational content and tools. They are not tax-deductible unless routed through an approved fiscal sponsor. Funds are used for hosting, research, curriculum development, tooling, and contributor support.

---

## 16. Рекомендуемая 90-day execution plan

### Days 0–7: donation-ready без юрлица

1. Создать `/support` page:
   - mission;
   - what exists already;
   - monthly budget;
   - конкретная цель: например `$2,000/month keeps the project alive`;
   - links: GitHub Sponsors, Ko-fi.
2. Добавить `FUNDING.yml` в GitHub.
3. Добавить публичный budget:
   - hosting/tools;
   - content production;
   - contributor bounties;
   - founder stipend only if explicitly stated.
4. Привести repo hygiene:
   - LICENSE: MIT/Apache-2.0 для software;
   - CC BY / CC BY-SA для educational content;
   - CONTRIBUTING.md;
   - CODE_OF_CONDUCT.md;
   - ROADMAP.md;
   - two admins if possible.

### Days 7–30: fiscal sponsor applications + sponsor deck

1. Подать в OSC:
   - organizational repository;
   - open-source license;
   - active commits;
   - public roadmap;
   - evidence of users/stars/downloads/community.
2. Подать в HCB:
   - charitable education framing;
   - AI literacy / open education mission;
   - budget;
   - governance/contact info.
3. Сделать sponsor deck на 6–8 slides:
   - audience;
   - problem;
   - traction;
   - content examples;
   - sponsorship tiers;
   - no editorial control;
   - impact metrics.
4. Outreach to 30 sponsors:
   - AI devtools;
   - agent frameworks;
   - cloud/dev environment vendors;
   - coding education companies;
   - AI consultancies.

### Days 30–60: первая fundraising campaign

Campaign framing:

```text
Goal: $2,000/month
Use: hosting, curriculum, weekly Claude Code / AI-agent tutorials,
open-source templates, community support, contributor bounties.
Milestone: publish 20 high-quality modules and 5 production-grade agent workflows.
```

Metrics to publish monthly:

| Metric | Why it matters |
|---|---|
| GitHub stars/forks | Developer proof |
| Unique learners | Education reach |
| Newsletter/subscribers | Distribution |
| Content completion | Actual learning |
| Issues/PRs/contributors | Community health |
| Sponsor revenue | Sustainability |
| Spend breakdown | Trust |

### Days 60–90: decision gate

| Result after 90 days | Decision |
|---|---|
| <$1k total donations | Stay informal; focus audience/product |
| $1k–$5k total | Stay GitHub/Ko-fi; continue sponsor outreach |
| $5k–$15k and rising | Fiscal sponsor worth it |
| $25k LOI / grant lead | Use HCB/fiscal sponsor or create required local entity |
| $3k–$5k/month recurring | Start own entity planning |
| $50k–$100k credible annual pipeline | US 501(c)(3) or Portugal associação depending donor geography |

---

## 17. Final recommendation

### Decision

**Do not create your own legal non-profit now.**

Your highest ROI path is:

1. **Stay informal/open-source for now.**
2. **Accept money immediately** via GitHub Sponsors + Ko-fi + corporate sponsor invoices.
3. **Apply to fiscal sponsorship**:
   - HCB if you want 501(c)(3)/grants/payroll.
   - OSC if open-source transparency/ecosystem is the main fundraising surface.
4. **Revisit own entity only after traction threshold.**

### Numeric threshold

Own legal non-profit starts making sense when one of these is true:

```text
Recurring donor revenue:
  >= $3k–$5k/month for 6 months

or credible annual donor/grant pipeline:
  >= $50k–$100k/year

or signed grant/sponsor LOI:
  >= $25k–$50k requiring own entity

or specific Portugal/EU grant:
  requires Portuguese associação
```

### Best route ranking for your case

| Rank | Route | Why |
|---:|---|---|
| **1** | **GitHub Sponsors / Ko-fi now + HCB/OSC fiscal sponsorship application** | Fastest, cheapest, preserves focus, enough compliance for early donors |
| **2** | **HCB fiscal sponsorship** | Best if tax-deductible grants/payroll matter |
| **3** | **OSC fiscal hosting** | Best if open-source community/sponsor transparency matters more than 501(c)(3) |
| **4** | **Portugal associação** | Use only for concrete Portugal/EU institutional path |
| **5** | **US 501(c)(3)** | Powerful but too expensive/slow before serious donor pipeline |
| **6** | **Estonia MTÜ** | Not enough benefit for this case |

**Bottom line:** the legal wrapper should follow donor traction, not precede it. For the next 90 days, the correct experiment is not “which jurisdiction?” but **“can this project convert audience + sponsors into $2k–$5k/month?”**