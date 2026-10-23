# DR26-08-01-MACANTON-02-658 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-658
rail: grok (grok -p (Grok Build CLI, подписка Heavy))
session_uuid: 019fbdac-be48-71d1-89a3-7ddd767003f6
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:20:27Z
proof_of_work: 19 уник. URL, 12 web_fetch веб-поисков — гейт пройден
note: чистый прогон: tool-mix = web_fetch x12 + todo_write, в workspace не лазил
---

Начинаю Deep Research по DR26-08-01-MACANTON-02-658: механика найма из Лиссабона, деньги net, налоги PT, travel и промежуточные ступени. Сначала подниму источники по компаниям, EOR/contractor, geo-pay и португальскому налоговому режиму.# DR26-08-01-MACANTON-02-658  
## What a US frontier-AI-lab offer actually pays a builder in Lisbon

**Research date:** 2026-08-01 · **Scope:** instruments, money, tax, travel — not “how to get noticed”.  
**Evidence rule:** figures without a dated source are omitted. Pre–mid-2025 material is flagged as possibly stale.

---

## 1. Экономический вердикт (≤300 слов)

Американская компания **может** платить человеку в Лиссабоне — но не «как в SF на payroll» по умолчанию.

**Инструменты (по убыванию простоты для US-стороны):**  
1) **contractor** (W-8BEN, invoice от *trabalhador independente* / PT LDA);  
2) **EOR** (Deel / Remote / Velocity Global ≈ $599/мес fee + локальные соцвзносы);  
3) **своя EU/PT entity** (редко ради одного человека);  
4) **«только approved countries / relocate»** — у OpenAI, Anthropic, xAI, DeepMind, Meta это **норма**, не исключение.

**Кто реально берёт remote-international из EU:** Hugging Face (remote-first, EMEA), AI-adjacent (LangChain, W&B, Modal…), иногда Perplexity (офисы London/Berlin/Belgrade, remote чаще US). **Anthropic / OpenAI / xAI** — hybrid/onsite, найм там, где есть юр.присутствие; **Fellows Anthropic** — US/UK/Canada, не PT.

**Деньги:** US-band frontier eng (OpenAI median SWE TC ~$850–860k; Anthropic senior ~$590k) — **не доказан** для Lisbon-remote. DevRel/advocate US band ниже (~$165–413k Meta/Google). EOR/contractor в EU почти всегда **geo-cut или cash-only без equity**.

**Net в Лиссабоне:** старый NHR для новых закрыт. **IFICI 20%** — только при certified activity + certified PT entity; remote на US employer **обычно не квалифицирует**. Стандарт: IRS до ~48% + SS (employee ~11% / employer ~23.75%; самозанятый ~29.6% базы).

**Семья 6 + 4 intl school:** школы ≈ €60–100k/год + жильё/жизнь/спорт → **~€130–200k+ net** минимум «комфорт». Это закрывают: US-band EOR base ≥~$250–350k gross **или** contractor cash ≥~$200–280k **без** geo-haircut; **не** закрывают: grants alone, part-time RLHF, «mid EU DevRel» без equity.

**Вывод:** механика найма есть; **US total-comp + equity из Lisbon — редкость, не план**. Реалистичный путь к школе+спорту: contractor/EOR cash у HF / AI-adjacent / vendor (Mercor/Surge) **или** EU entity lab → потом upgrade. Permanent US onsite — fallback (O-1), не цель.

---

## 2. Table: company × remote-intl × instrument × roles × comp band × source

| Company | Hires remote-intl from PT? | Typical instrument | Role families more open | Comp band (USD gross TC, **US-reported**) | Source + date |
|--------|---------------------------|--------------------|-------------------------|-------------------------------------------|---------------|
| **OpenAI** | **No as default.** Hires where legal presence / office; hybrid ~3 days office; EU jobs e.g. Dublin sales, Remote-Brussels policy — not “Lisbon WFA”. | Employee on local entity; contractor rare for core tech | Applied/eng/research **if** already in approved geo; Global Affairs remote EU | SWE US: ~$253k (L2) – $1.27M (L6); **median ~$850–860k** | Levels.fyi OpenAI SWE, updated ~2026-07-31; careers/WFA notes 2025; Blind in-office 2025-09 |
| **Anthropic** | **No as default.** SF / NYC / London hybrid; Fellows **US/UK/Canada only** | Employee; fellowship stipend (not full equity seat) | Research/eng if in office cities; Fellows AI safety | SWE US: ~$367k – $1.25M; senior median **~$590k** (base ~$328k + stock ~$262k/yr) | Levels.fyi Anthropic, ~2026-07-30; Greenhouse Fellows 2026 ($3,850/wk) |
| **xAI** | **Unlikely.** Bay Area / onsite culture; postings not “anywhere” | Employee US | Eng/research if relocate or US base | **No reliable Lisbon/EOR series** — omit | Careers pattern (public) — inference: onsite-first |
| **Google DeepMind** | **No remote-from-PT.** “Intentional location strategy”: London, Bay Area, Paris, Zurich, NYC, etc. | Google local entity employee | Research/eng **only** in listed sites | Google Dev Advocate US: **$192k–$376k** L3–L6, median ~$225k (proxy for DevRel door) | deepmind.google/careers locations; Levels.fyi Google Dev Advocate ~2026-07-31 |
| **Meta AI** | **Hard geo policy.** Remote possible only in **supported locations**; **location-based pay** | Local employee; EOR only if Meta supports country | Applied/eng/DevRel if geo approved | Dev Advocate US: **$165k–$413k** E3–E5, median ~$195k | Levels.fyi Meta Dev Advocate ~2026-07-31; Meta remote location policies (ongoing) |
| **Perplexity** | **Partial.** Offices SF/NYC/Austin/DC/London/Berlin/Belgrade; some **Remote (US)** roles; “apply to timezone” | Local employee; some remote US; EU offices | Eng, product, GTM | Postings show e.g. **$175–300k** ranges on some Ashby roles (role-specific) | perplexity.ai/hub/careers (2026); jobs.ashbyhq.com/Perplexity |
| **Hugging Face** | **Yes — best documented fit.** Remote-first; **EMEA Remote** postings (ML eng, advocate, etc.) | Employee via EU setup / local contract; contractor possible early | Open-source ML eng, DevRel/advocate, tools — **realistic door for public OSS** | **No solid Levels.fyi frontier-band series** — market EU tech / HF-adjacent; do not invent | apply.workable.com/huggingface; 4dayweek / Built In “EMEA Remote” 2025–26 |
| **Mistral / EU labs** | **Yes within EU** (entity) | French/EU employee | Eng, research, DevX | EU bands (not SF); check live postings | Careers (live) |
| **AI-adjacent US** (LangChain, W&B, Modal, Replicate, Continue…) | **Often yes** (remote-friendly startups) | Contractor or EOR | DevRel, solutions, eng | Startup cash **~$120–250k** range common in market commentary; **verify per offer** — sparse public series | Industry hiring pattern 2025–26 (weak primary data) |

**Hypothesis (labeled):** Frontier labs do not publish “Portugal remote = SF TC”. Absence of Lisbon postings + hybrid policy = **effective refusal of US-band remote-PT seats**, not geo-adjust after hire.

---

## 3. Table: instrument × PT net × equity × what you give up

Assumptions for **illustrative net** (not tax advice): PT tax resident, **no IFICI**, single earner, roughly mid-progressive IRS + SS. Exact net needs contador; rates change yearly.

| Instrument | How it works | Approx. PT take-home on **$200k cash** / **$350k cash** (order-of-magnitude) | Equity access | What you give up |
|------------|--------------|-----------------------------------------------------------------------------|---------------|------------------|
| **US employee via EOR (Deel/Remote/…)** | US co pays EOR; you are **employee of EOR’s PT entity**; EOR fee ~**$599/employee/mo** (2026 public) + employer SS ~**23.75%** of salary (company cost, not your net) | Cash is Categoria A. Employee SS ~**11%**. IRS progressive to **~48%** + solidarity. **Rough net:** ~**55–65%** of gross cash at high incomes → on $200k ≈ **$110–130k** net; on $350k ≈ **$180–220k** net *(hypothesis range; run simulator)* | Possible if US co grants to EOR employees (policy-dependent; securities law for non-US persons) | Lower cash than SF band if geo-priced; benefits = local PT package not US; EOR = middleman; **IFICI usually fails** (foreign US co ≠ certified PT innovation entity) |
| **Independent contractor (*trabalhador independente*)** | Invoice US client; **W-8BEN**; services **performed in PT** → generally **not US-source** (no 30% WH if no US performance) | Categoria B + **SS ~29.6%** of contribution base (simplified regimes may differ). Effective all-in often **~40–55%** of gross depending on expenses/regime. **Rough net:** $200k → **~$90–120k**; $350k → **~$160–210k** *(hypothesis)* | **Usually none** (cash contract) | No employer equity, no US health/401k, no severance culture, misclassification risk for US co if you look like employee |
| **Own PT company (LDA) invoices US** | Corporate tax + salary/dividends planning | Can be more efficient at high cash **if** structured; still SS on salary; dividends 28% (standard) unless treaty/structure | Same as contractor re: client equity (rare) | Admin cost, accountant, PE risk if poorly run; **not automatic IFICI** |
| **US employee on US payroll while living PT** | Almost never compliant long-term (tax, labor, permanent establishment) | Double tax / illegal employment risk | Full US equity **if** granted | **High compliance risk** — not a recommended instrument |
| **Relocate / EU office seat** | Real employee of OpenAI Ireland / Google Ireland / Anthropic UK etc. | Local country tax (IE/UK/FR) — not this brief’s Lisbon net | Full employee equity/PPUs/RSUs per plan | Family move or split-family; not “stay Lisbon” |

### Equity that isn’t stock (OpenAI / Anthropic) for a non-US person

| Item | Evidence | Non-US / contractor implications |
|------|----------|----------------------------------|
| **OpenAI PPUs → later share conversion; tenders** | Levels.fyi OpenAI equity explainer (updated note: **as of Jan 2026** new offers shift toward **RSUs**, cliff removed); tenders e.g. SoftBank secondary **2024-11**, large tender **~$6.6B Oct 2025** @ ~$400–500B valuation; sellers = current/former **service providers** | PPUs/RSUs historically for **employees/service providers**. **Hypothesis:** pure 1099-style foreign contractor **often excluded** from employee equity pools unless grant agreement says otherwise. Liquidity = **tender windows**, not public market. PT tax on tender proceeds = capital gains / employment income classification → **need PT tax counsel** (not automatic “foreign exempt”). |
| **Anthropic** | Tender **Apr 2026** reported ~$350B; employees often **held** rather than sold | Same pattern: employee equity + occasional secondary. Foreign contractor access **undocumented** → treat as **no equity** unless offer letter grants it. |

**US–PT treaty (practical):**  
- Treaty in force (IRS pub/trty/portugal).  
- **Independent personal services (Art. 15):** generally taxed only in residence state unless fixed base in other state or **≥183 days** presence.  
- **Services performed outside the US** by nonresident → typically **foreign-source**, **no US withholding** with valid **W-8BEN**.  
- Work **physically in the US** (on-site weeks) can create **US-source** income for those days → withholding / Form 1042-S issues; long stays risk **183-day** threshold.  
- You file **Portuguese IRS** on worldwide income as resident; credit US tax if any via treaty.

---

## 4. Travel status for on-sites (not immigration)

### Permitted activities (high level)

| Status | Who | Typical use | Allowed (business) | Border traps |
|--------|-----|-------------|--------------------|--------------|
| **ESTA / VWP** | Citizens of VWP countries (most EU, not all) | ≤90 days, tourism + **limited business** | Meetings, conferences, training, negotiating contracts | **Performing productive work for US pay while on US soil** as if local labor; “I’m here for 3 months coding at HQ”; multiple long trips that look like de facto residence |
| **B-1** | Non-VWP or when ESTA inappropriate | Similar business visitor | Same class of activities; letter from company helps | Same: hands-on work product delivered as US employment without work visa |
| **No status / wrong stamp** | — | — | — | Refusal / removal; future VWP bar |

**Critical line (practice, not slogan):** Being **paid by a US company while physically working in the US** is the risk zone. Short onsite for **meetings / offsites / conferences** is the intended use of B-1/ESTA; **doing your regular remote job from the SF office for weeks** is what gets people into trouble. Company **invitation letter** (purpose, dates, who pays travel, no local salary) is common best practice; **documented sponsorship of “business visa letters” by named AI labs** is **not systematically published** — treat as **ad hoc HR support**, not a product.

### Passport scenarios

| Scenario | Entry tool | Notes |
|----------|------------|-------|
| **EU passport** (e.g. PT/other VWP) | ESTA for short business | Confirm VWP list still includes citizenship; Portugal is VWP. |
| **PT residence only, third-country passport on VWP list** | ESTA if nationality is VWP | Residence permit ≠ US entry right. |
| **Third-country passport not on VWP** | **B-1/B-2** (or other) at consulate | Longer lead time; company letter + ties to PT matter. |
| **Any + long cumulative US presence** | Risk of tax/immigration “you live here” | Track days; **183-day** treaty/tax exposure. |

### Fallback permanent onsite (one short section)

| Route | Realism for this profile |
|-------|---------------------------|
| **O-1A** (extraordinary ability) | Publications, OSS impact, press, original contributions, critical role — **plausible** for a public builder; employer petition; no lottery. Evidence bar is high and **documentation-heavy**. |
| **H-1B** | Lottery; employer-driven; odds not a plan. |
| **L-1** | Needs **1 year** employment at foreign affiliate first — only if company has entity structure. |

---

## 5. Ranked intermediate rungs (pay now, keep door open) — 2026

| Rank | Rung | 2026 money (sourced) | How to enter | Converts to full lab offer? |
|------|------|----------------------|--------------|------------------------------|
| **1** | **Expert contractor to labs via Mercor / Surge / Scale-Outlier / Handshake** | Mercor: expert **$75–$200+/hr** (national avg AI trainer ~$31/hr floor); listings **$50–$140/hr** model trainer; specialists to **$250/hr**. Surge advertises **$250k+/yr** agentic coding benchmarks. Work remote. | Apply + assessments on platform; domain expertise (eng, physics, evals) | **Medium:** proximity to lab data pipelines; not guaranteed full-time. Cash is real now. |
| **2** | **Full-time / near-FT remote at Hugging Face or AI-adjacent (LangChain, W&B, Modal…)** | HF: EU remote roles live; cash typically **below** OpenAI/Anthropic US TC (no solid public median — verify offer). Startup DevRel/eng often **~$120–250k** cash in market (weak public series) | Careers pages; OSS track record is the product | **High** for HF↔lab network; **medium** for adjacent→frontier |
| **3** | **Anthropic Fellows (AI safety)** | **$3,850/week** (~$15.4k/mo) + **~$15k/mo compute**, ~**4 months**, extendable | Greenhouse + Constellation form; cohorts 2026 (e.g. apply deadlines Apr/Jul 2026 depending on cohort) | **Medium-high inside Anthropic funnel** — but **US/UK/Canada only**, not Lisbon-resident path unless relocate temporarily |
| **4** | **GitHub Accelerator / Secure OSS Fund** | Accelerator: **$40,000/project** non-dilutive + mentorship. Secure Open Source Fund: **$10,000/project** | github.com/open-source/accelerator; invitation/rolling for security fund | **Low-medium** direct hire; **high** for visibility/signal |
| **5** | **Sovereign Tech Fund (DE)** | Min **€50k**, up to **~€1M** historically; **€23M+** invested 2022–24 across 60+ projects | sovereign.tech applications; project must fit digital infrastructure | **Low** to US lab job; **high** cash for maintainer runway |
| **6** | **NLnet / NGI** | Project grants (historically **€5k–€50k+** per call; check open call) | nlnet.nl open calls | Low hire conversion; runway |
| **7** | **Linux Foundation / Alpha-Omega** | Security funding for critical OSS (project-dependent; not fixed salary) | alpha-omega.dev / LF programs | Low hire conversion |
| **8** | **GitHub Sponsors / Open Collective** | Variable; not a family-of-six floor | Sponsors on repos | Low |
| **9** | **OpenAI / xAI “programs” equivalent to Anthropic Fellows** | **No comparable public remote stipend at Anthropic scale found for PT-resident** in this pass | Watch careers | — |

**Family-budget stress test (sourced components):**

| Cost block | Sourced range | Annual (4 kids) |
|------------|---------------|-----------------|
| Lisbon international school tuition 2025/26 | ~**€9k–€28k/child** by grade/school (CAISL ~€18–28k; St Julian’s ~€16–26k; mid ~€13–22k) | **€60k–€100k+** tuition alone |
| Family living ex-school (Lisbon family-of-4 proxy) | Numbeo family-of-4 **ex-rent ~€2,760/mo**; family-of-4 all-in often cited **~€4.5–6k/mo** before premium schools | **€50k–€80k+** living for larger household (scale up for 6) |
| **Working target net** | Schools + living + sports + buffer | **≈ €130,000 – €200,000+ net / year** |

**What clears that net (order-of-magnitude, post-tax hypothesis):**

| Scenario | Clears school+sport floor? |
|----------|----------------------------|
| Mercor/Surge expert **$150/hr × 30 hr/wk × 48 wk ≈ $216k** gross contractor | **Borderline / yes** if high utilization and SS optimized |
| EOR salary **$250–350k** cash, no IFICI | **Yes** on upper end |
| US-band OpenAI/Anthropic **employee TC $500k+** while living Lisbon | **Yes if cash+liquid equity real** — but **availability of that seat from Lisbon is the bottleneck** |
| Grants only ($40k + €50k) | **No** |
| Google/Meta DevRel US median **~$195–225k** if geo-cut 20–40% | **Maybe / no** after tax + 4 schools |

---

## 6. Three assumptions in this brief that research shows are **WRONG**

### Wrong #1  
**“A US frontier lab will hire me remote in Lisbon on the same total-comp instrument as a Bay Area employee.”**  
**Evidence:** OpenAI/Anthropic hybrid + hire-where-we-have-presence; Anthropic Fellows limited to **US/UK/Canada**; DeepMind “intentional locations”; Meta location-based pay; OpenAI WFA analyses say **no international WFA free-for-all** (2025). Hugging Face / adjacent ≠ OpenAI TC.

### Wrong #2  
**“NHR / IFICI (‘NHR 2.0’) will tax my US remote salary at 20%.”**  
**Evidence:** Classic NHR closed to new entrants; IFICI requires **certified** high-value activity **and** typically link to **certified PT innovation/export/R&D entity**. Remote developer on **foreign** US payroll/invoice is the textbook **exclusion** (Worktugal, AIX, Pérez-Llorca 2025–26). Without that certification → **standard progressive IRS + SS**.

### Wrong #3  
**“Contractor vs employee is a paper difference; equity and net land the same.”**  
**Evidence:** OpenAI/Anthropic equity (PPUs/RSUs/tenders) is built for **employees / service providers on equity plans**; tenders liquidate **vested employee-side** units. Contractors typically get **cash only**, bear **~29.6% SS**, lose benefits, and face **misclassification** limits that push serious roles to **EOR** (Deel **$599/mo**) with **local** employment economics — not SF total comp.

---

## Appendix A — Mechanics map (instruments)

```
US company wants Lisbon person
        │
        ├─► Contractor + W-8BEN + PT invoice
        │     • Fast, cash, often no equity
        │     • Work in PT = no US WH (treaty/source)
        │
        ├─► EOR (Deel / Remote / G-P / Velocity)
        │     • Legal employee in PT; fee ~$599/mo + employer SS
        │     • Possible equity if co policy allows
        │
        ├─► Company EU entity (IE/UK/FR/DE/PT)
        │     • Real payroll; rare for 1 hire unless scale
        │
        └─► "We don't hire there — relocate or no"
              • Default for many frontier core roles 2025–26
```

## Appendix B — Key primary links (dated)

| Topic | Link | Date signal |
|-------|------|-------------|
| OpenAI careers / locations | https://openai.com/careers/ | Live 2026-08 |
| OpenAI WFA summary | https://wfa.team/blog/openai-work-from-anywhere-policy-2025/ | 2025-09 |
| Anthropic Fellows | https://alignment.anthropic.com/2025/anthropic-fellows-program-2026/ | 2026 cohorts |
| Levels Anthropic SWE | https://www.levels.fyi/companies/anthropic/salaries/software-engineer | ~2026-07-30 |
| Levels OpenAI SWE | https://www.levels.fyi/companies/openai/salaries/software-engineer | ~2026-07-31 |
| IFICI vs NHR | https://blog.worktugal.com/ifici-vs-nhr-portugal-tax-guide/ | 2026-01 |
| IFICI eligibility 2026 | https://www.aixinvestment.com/nhr-tax-in-portugal-eligibility-updates/ | 2026 |
| PT tax guide PDF | https://www.perezllorca.com/wp-content/uploads/2025/08/Portuguese-Tax-Guide-for-Individuals.pdf | 2025-08 |
| US–PT treaty | https://www.irs.gov/pub/irs-trty/portugal.pdf | IRS |
| Deel EOR pricing | https://www.deel.com/pricing/ | ~$599 EOR 2026 |
| Lisbon school fees | https://www.tutopiya.com/blog/international-school-fees/most-expensive-international-school-lisbon/ | 2025/26 |
| Numbeo Lisbon COL | https://www.numbeo.com/cost-of-living/in/Lisbon | Jul 2026 |
| Mercor rates | https://www.mercor.com/resources/experts/ai-trainer-salary-hourly-rates/ | 2026-06 |
| Surge $250k+ coding | https://surgehq.ai/ | Live 2026 |
| GitHub Accelerator $40k | https://github.com/open-source/accelerator | Live |
| Sovereign Tech Fund | https://www.sovereign.tech/ | €50k+ min (case study 2025) |
| Perplexity locations | https://www.perplexity.ai/hub/careers | Live |
| DeepMind locations | https://deepmind.google/careers/ | Live |
| OpenAI PPU/tender mechanics | https://www.levels.fyi/blog/openai-compensation.html | Updated through 2026 notes |

---

## Appendix C — Confidence & gaps

| Claim class | Confidence |
|-------------|------------|
| Frontier labs are **not** Lisbon-remote-first | **High** (careers + WFA + Fellows geo) |
| EOR/contractor instruments exist and are used industry-wide | **High** |
| Exact **geo-haircut %** Anthropic/OpenAI would apply **if** they hired Lisbon | **Low** — almost no first-person 2025–26 Lisbon offers published |
| Precise PT net on $X | **Medium** — needs personal tax simulation |
| Foreign contractor holding PPUs | **Low-medium** — plan documents private; default assumption = no |

**Not tax, immigration, or employment advice.** Before any real offer: PT *contador* + US employment counsel + immigration counsel for travel pattern.

---

🎙 **Майк:** Коротко: «нанять из Лиссабона» = **да как contractor/EOR/HF**, «получить OpenAI-median $850k TC сидя дома» = **нет как дефолт**. Семью из шести с четырьмя intl-школами кормит **крупный cash** (vendor expert или EOR ≥~$250–350k) или **реальный equity-seat**, а не гранты и не «надежда на NHR 2.0».

🧒 **Простыми словами:**  
Американская фирма **может** платить тебе в Лиссабоне — как фрилансеру или через «аренду работодателя» (EOR).  
Но большие AI-лаборатории почти всегда хотят, чтобы ты был **в их стране/офисе**, и **не** платят «как в Калифорнии», если ты в Португалии.  
Старая «низкая налог-скидка» для всех новичков **закончилась**; 20% теперь только для особых «инновационных» кейсов.  
На школу четверых детей нужно **много чистыми деньгами** — это тянут крупные контракты/зарплата, а не мелкие гранты.
