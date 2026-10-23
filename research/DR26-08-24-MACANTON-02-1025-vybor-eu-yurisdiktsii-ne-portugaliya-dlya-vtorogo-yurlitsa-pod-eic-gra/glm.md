# DR26-08-24-MACANTON-02-1025 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-24-MACANTON-02-1025
vendor: Z.ai GLM (chat.z.ai)
door: browser-pane-anon
rail: glm

collected: 2026-08-31T22:42:44Z
took_sec: 0
bytes: 19278
note: >-
  Анонимная веб-сессия GLM с веб-поиском и deep-think. Это НЕ вендорский режим Deep Research — уровень «умная модель + поиск».
  Забор через локальный мост dr_glm_relay.py (0 токенов, тело не проходит через контекст).
---

# EU Second-Entity Deep Dive: EIC/Horizon-Eligible Jurisdiction for a Delaware C-Corp (Founder Resident in Lisbon)

**Verdict:** (1) Ireland — Ltd, (2) Estonia — OÜ, (3) Lithuania — UAB.

Ireland wins on the combined weight of EIC credibility, 12.5% trading tax with a 30–35% cash R&D credit, common-law/English-language familiarity for US acquirers, and the fact that a Portuguese-resident founder already satisfies Ireland's EEA-director rule (no bond needed). Estonia wins on speed, cost, and a 0%-on-retained-profits model that is structurally the most resilient against "effective management" residency claims. Lithuania is the low-cost dark horse (16%→17% CIT, fully remote formation, small-company 0%/7% regimes). The critical caveat that applies to all candidates: a Lisbon-resident founder who is sole director and manages day-to-day from Portugal creates effective-management/PE exposure under Portuguese practice regardless of where the company is incorporated — the fix is documented entity-country substance, not jurisdiction shopping.

## 1. The EIC Gate: What "Established in a Member State" Actually Requires

All eight jurisdictions listed are EU Member States, so each cleanly satisfies the formal eligibility test. The EIC Accelerator is open to "a single start-up or SME (including spin-outs) established in an EU Member State or Associated Country," and the 2026 Work Programme frames it as a mono-beneficiary rule for SMEs and small mid-caps up to 499 employees (investment component) [youtube +1].

Nuances that actually decide eligibility:

- **Effective establishment, not mailbox incorporation.** The beneficiary "must prove its effective establishment in a Member State or an Associated Country," and "the Commission may set specific conditions and milestones in the contract to ensure that the interest of the Union is met" [grantthornton]. The EIC network's own guidance states the company "must prove effective establishment" and that "Union interest needs to be met". Proof is tested at full-proposal submission (Step 2), not Step 1 [vitallaw].
- **Third-country relocation route exists — but you don't need it.** A US entity may apply at Step 1 if it intends to "establish or relocate an SME to a MS or AC by the time of submitting the full application" [youtube +1]. Since the EU sub is stood up in advance, the EU sub simply applies as the beneficiary — the cleaner path.
- **SME status with a US parent.** SME status uses Commission Recommendation 2003/361 via a self-assessment in the Funding & Tenders Portal: <250 staff and ≤€50M turnover or ≤€43M balance sheet [kronis]. A 100%-owned Delaware parent makes the EU sub a linked enterprise, so the parent's headcount/turnover aggregate into the test — at pre-seed scale normally still comfortably SME, but the parent relationship must be declared: "the subsidiary has to declare all upstream and downstream relationships… the application is eligible if the SME is deemed independent" [seglerconsulting +1].
- **Funding shape and timing.** Grant component up to €2.5M; EIC Fund equity typically €1–10M (2026 programme and STEP Scale-Up extend beyond that); applicants may choose grant-only, equity-only, or blended — grant-only capped at €2.5M and available once per company under Horizon Europe [euacc +2]. Step 1 is batched on the first Tuesday of every month; 2026 Step 2 cut-offs are Jan 7, Mar 4, May 6, Jul 8, Sep 2, Nov 4 [rasph +2]. The June 2026 amendment opened the Accelerator to dual-use innovation — relevant for agent-security software [youtube].

**Practical implication:** for an e-residency-style company, EIC evaluators will look for real EU activity (local staff/contractor, premises, local bank, board decisions taken in-country). An Irish or Dutch entity faces an easier credibility bar; an Estonian OÜ can clear it but needs a deliberate evidence file.

## 2. Tradeoff Table (all eight candidates)

| Jurisdiction (entity) | EIC fit / "effective establishment" optics | Corporate tax (headline → effective) | Remote setup (time / cost) | Substance & PT-resident-founder risk | Remote banking / IBAN | US-parent & exit friendliness |
|---|---|---|---|---|---|---|
| **Ireland (Ltd)** | Best-in-class; EIC Fund, evaluators, Enterprise Ireland all comfortable | 12.5% trading; R&D credit 30% (35% legislated); KDB 6.25% on qualifying IP [landing.winnovart +2] | 3–10 days; €99–2,000; founder is EEA-resident → no s.137 bond [euacc +2] | No formal substance regime; sole director in Lisbon → management-PE risk (see §5) | AIB/BOI want in-person; Revolut/Fire/Wise fine for day one | #1 for US acquirers: common law, English, dense precedent |
| **Estonia (OÜ)** | Eligible, but e-residency mailbox optics mean you must build an evidence pack | 0% on retained profits; 22/78 on distributions (~28.2% of gross); deemed-distribution traps [grantthornton +1] | 1–3 days online; €265 state fee; contact person/legal address €200–400/yr [erhub +1] | No domestic management-and-control test (residency = incorporation) [learn.e-resident.gov]; but PE/dual-residency risk from Lisbon management [erhub +1] | Estonian banks restrictive for non-residents; Wise/Revolut/Xolo standard [e-resident.gov +1] | Digital share transfers, clean; US counsel slightly less familiar |
| **Netherlands (BV)** | Excellent; strong innovation hub | 19% ≤€200k; 25.8% above; participation exemption ≥5% [bdo +1] | 1–3 weeks; €1,200–3,500; notary via POA, no travel | No substance regime; Lisbon sole-director PE risk same as others | Bunq/Revolut; traditional banks harder without Dutch ties | Very familiar to US acquirers; standard BV precedent |
| **Cyprus (Ltd)** | Eligible; EU MS | 12.5% → 15% from 1 Jan 2026; IP Box (nexus) [euacc +1] | 5–10 days; €1.5–3k; remote via local firm [oxfordglobalservices] | Residency = incorporation or management & control; remotely-run co. needs care [kps.com +1] | Traditional banks push in-person KYC; fintech fallback | Historically structure-heavy → some US DD teams probe more; no US DTT |
| **Luxembourg (S.à r.l.)** | Eligible; overkill for pre-seed | CIT 14%/16% (2025) + solidarity + municipal → ~24–25% aggregate [taxsummaries.pwc +1] | 2–4 weeks; notary; €12,000 min capital [taxfoundation] | Substance fine; costs high | Strong banking | Excellent reputation, but sized for funds/holdings, not 2-person ops |
| **Lithuania (UAB)** | Eligible; credible startup scene (Vinted, Nord) | 16% (2025) → 17% from 2026; small-company 0%/7% regimes from 2026 if conditions met [taxsummaries.pwc +1] | 5–10 business days; ~€1–2k; fully remote [rfflawyers] | Standard PE risk; manageable with substance | Fintech-first; local banks need presence | Fine; less familiar to US counsel than IE/NL |
| **Malta (Ltd)** | Eligible | 35% headline + 6/7 refund → ~5% effective [mtca.gov +1] | 2–4 working days; €1–2k [corporate-focus-ltd] | Refund-mechanism complicates CFC "effective tax" measurement [landing.winnovart]; more PT-scrutiny risk | Fintech-first | US DD teams ask more questions on Malta structures |

*Honorable mention:* Latvia runs an Estonian-style deferred-profit model — worth a look only as a Baltic alternative to Estonia; verify current parameters with local counsel.

## 3. Ranked Recommendation

### #1 — Ireland (private company limited by shares)

**Why it wins.** Ireland is the only English-speaking, common-law EU jurisdiction, which simultaneously lowers friction for (a) EIC evaluation and EIC Fund investment paperwork and (b) a US acqui-hire where the acquirer's counsel will sweep the entity. The 12.5% trading rate carries a 30% R&D credit rising to 35% under Finance Act 2025 (payable in cash even at a loss, first-year payable tranche raised to €87,500) — genuine non-dilutive stacking alongside EIC grants [landing.winnovart +1]. The Knowledge Development Box offers 6.25% on qualifying IP profits for periods commencing before 1 Jan 2027 [revenue].

**Founder-residency mechanics.** Section 137 Companies Act 2014 requires at least one director "ordinarily resident in an EEA state" or a €25,000 bond costing ~€1,500–2,000 per two years [vistra +1]. A Lisbon-resident founder is EEA-resident, so qualifies without a bond or nominee — residency, not passport, is the test [freemanlaw].

**Tradeoffs.** Higher annual compliance than Estonia (accounting + secretarial typically €3–6k/yr); traditional banks want in-person meetings, so fintech IBAN first. Sole-director-from-Lisbon management is the main PE risk (§5).

### #2 — Estonia (OÜ via e-Residency)

**Why #2.** Fastest and cheapest compliant route: €265 state fee, 1–3 days registration, contact person/legal address €200–400/yr [erhub +1]. The 0%-on-retained model is the single best tax regime for a cash-hungry pre-seed — no tax until distribution, with deemed-distribution rules for non-business expenses and TP adjustments to respect. Critically for the Lisbon situation, Estonia determines corporate tax residency by incorporation alone — there is no domestic management-and-control test [learn.e-resident.gov] — so Estonia will never itself claim the OÜ drifted offshore.

**Tradeoffs.** (a) The EIC "effective establishment" bar requires a real evidence file — a Tallinn service provider alone won't convince evaluators; you'd want a local contractor/employee, documented board meetings, and an EU IBAN tied to real operations. (b) Banking: Estonian banks (LHV, SEB, Swedbank) generally require in-person meetings and strong Estonia ties for non-residents — Wise/Revolut/Xolo are the standard workaround, and the e-Residency team confirms an Estonian bank account is "not necessary for most e-residents" [e-resident.gov +1]. (c) Portugal-side PE/effective-management exposure exists as in every jurisdiction (§5) [linkedin +1].

### #3 — Lithuania (UAB)

**Why it makes the podium.** Fully remote formation in 5–10 business days for ~€1–2k including state fees and registered address [rfflawyers]; 16% CIT rising to 17% in 2026, with small-company reduced rates of 0% or 7% from 2026 if headcount/turnover conditions are met — potentially beating everyone on effective rate at this stage [taxsummaries.pwc]. A serious startup ecosystem (Vinted, Nord Security) and EU-normal banking via fintech.

**Tradeoffs.** Less familiar to US acquirers than Ireland/NL; EIC "effective establishment" evidence still needed; the reduced small-company rates carry conditions worth pre-checking with a Lithuanian adviser.

**Netherlands is the strong #4** — excellent reputation and remote notarization via power of attorney (1–3 weeks, €1,200–3,500) — but slower, costlier, and its 25.8% top rate and dividend-WHT friction to the US parent give Ireland the edge. **Cyprus** (15% from 2026, dual incorporation/M&C residency tests, no US treaty) and **Luxembourg** (24–25% aggregate, €12k capital) lose on tax mechanics or scale fit; **Malta's** ~5% effective rate depends on the shareholder-refund mechanism, which muddies both Portugal-CFC analysis and US diligence optics [landing.winnovart].

## 4. Cleanest Delaware-Parent → EU-Subsidiary Structure

```
   Founder(s) + US investors
            │
   Delaware C-Corp (holding, unchanged venture story)
            │ 100%
   Irish Ltd (op-co)  ← IP, contracts, EIC grant & EIC Fund equity
```

- **Delaware parent holds 100% of the Irish Ltd.** The US venture/acqui-hire vehicle stays untouched; an acquirer can buy Delaware parent shares and pick up the Irish sub, or buy the Irish sub directly.
- **IP goes into the EU sub now, while value is minimal.** Founder + Delaware parent execute deeds of assignment for the agent-leash IP to the Irish Ltd with board minutes. This gives a clean nexus story for any future KDB/IP-box claims and avoids EIC evaluators questioning why the funded entity licenses core IP from a US parent.
- **Intercompany simplicity.** One services agreement (Delaware parent ↔ Irish Ltd, cost-plus or arm's-length fee) plus a short transfer-pricing memo. Avoid royalty flows unless real development actually happens in the payer — at pre-seed, services agreements are all you need.
- **EIC mechanics with this structure.** The Irish Ltd applies as sole beneficiary and declares the Delaware parent as a linked enterprise in the SME self-assessment (aggregated headcount/turnover — trivially still SME at pre-seed) [seglerconsulting +1]. If blended finance is won, the EIC Fund invests into the Irish Ltd, diluting Delaware — standard and workable; the "Union interest" milestones typically translate into EU-scaling commitments, not ownership restrictions. Grant-only (≤€2.5M) avoids the equity-layer complexity entirely if preferred [seglerconsulting].
- **Alternative rejected:** founders holding the EU sub directly is cleanest for SME "autonomy" optics but creates two parallel cap tables and two exit transactions — the wrong trade for a future acqui-hire.

## 5. Portugal-Resident Founder: CFC and Permanent-Establishment Exposure

This is the risk that follows the founder, not the jurisdiction — manage it once, correctly, and all eight candidates work.

**Portugal's CFC rule (Art. 76 CIT / 83.º-16 CIRS, ATAD-aligned).** Portugal imputes profits of a non-resident entity to a Portuguese resident holding ≥25% where the entity is in a blacklisted jurisdiction or subject to effective tax below 50% of the Portuguese-equivalent tax [eic.ec.europa +1]. The saving grace: entities located in EU/EEA countries with genuine economic activity (i.e., not artificial arrangements) are exempt [seglerconsulting]. None of the eight candidates is on Portugal's blacklist (Ordinance 150/2004, last amended by 292/2025) [en.wikipedia +1]. **Net effect: substance, not rate, is the CFC protection** — and Malta is the one candidate where the refund-driven ~5% effective rate invites a closer look at whether "effective taxation" and genuine activity line up [landing.winnovart].

**The bigger risk: management PE / dual residency in Portugal.** Portuguese domestic law treats a company as resident where its head office or effective place of management is located, and defines PE to include a "place of management" [globaltaxnews.ey]. Portuguese practitioners report that board members relocating to Portugal who take day-to-day decisions and sign contracts from Portugal "frequently trigger the creation of a PE," with no published profit-allocation guidance from the tax authority [kpmg]. Estonia's own e-Residency guidance confirms e-resident companies can create PEs and tax liabilities abroad [linkedin], and the e-resident ecosystem openly flags that a solo founder-director managing from their home country is the classic fact pattern [erhub +1]. A treaty tie-breaker could then allocate residency to Portugal, collapsing the benefit of any EU structure.

**Mitigations that actually work (all jurisdictions):**
- Documented entity-country decision-making — board meetings held with the registered office as venue (or clearly recorded as taken there), minutes signed, local service provider engaged.
- One local person — a contractor, accountant-facing employee, or (unnecessary in Ireland, helpful in Estonia) a local director.
- Never create a fixed workplace in Lisbon for the EU sub: no employees of the sub working from the founder's home, no contracts habitually concluded from Lisbon (dependent-agent PE), no use of the Lisbon address in marketing or contracts.
- Contracts executed through the entity's infrastructure (its e-signature stack, its registered address, its bank).
- A Portuguese advance ruling (ATAJO / pedido de informação prévia) on the PE and CFC positions if revenue becomes material — the single highest-value piece of professional advice in this report.

## 6. Concrete First Steps + Costs for the Top Pick (Ireland)

| Step | Detail | Time | Cost |
|---|---|---|---|
| 1. Incorporate Ltd via CRO through a formation agent | Name check, Constitution, Form A1 | 3–10 business days | €99–500 agent fee + ~€50 CRO (all-in €1–2k with registered office/secretary bundled) [euacc +1] |
| 2. Registered office + company secretary (mandatory) | Professional address; secretarial service | Day 1 | €300–600/yr each |
| 3. Appoint yourself as director | PT-resident = EEA-resident → no s.137 bond [vistra +1] | Day 1 | €0 |
| 4. Tax registrations (TR2) | Corporation Tax; VAT (B2B EU = reverse charge; OSS if any B2C); employer PAYE if hiring | 2–4 weeks | €0 |
| 5. Banking | Revolut Business / Wise / Fire for day-one EU IBAN; approach AIB/BOI once substance exists | 1–2 weeks | ~€0–30/mo |
| 6. IP assignment | Deeds of assignment founder + Delaware → Irish Ltd; board minutes; simple valuation memo | 1–2 weeks | €1–3k legal |
| 7. Intercompany agreement + TP memo | Delaware ↔ Irish services agreement, cost-plus | 1 week | €1–2k |
| 8. Portugal-side review | PT adviser: CFC/PE analysis; consider ATAJO ruling | 2–6 weeks | €1.5–4k |
| 9. EIC onboarding | Funding & Tenders Portal PIC, SME self-assessment (declare Delaware parent), effective-establishment evidence pack; Step 1 batched first Tuesday monthly; 2026 Step 2: Jan 7 / Mar 4 / May 6 / Jul 8 / Sep 2 / Nov 4 [rasph +1] | Step 1→Step 2 cycle ~4–6 weeks | Optional proposal consultant €5–20k |

**Year-1 budget estimate:** ~€5–8k in entity/compliance costs (excl. optional EIC consulting), vs. ~€1.5–3k for the Estonian route — the delta buys the strongest EIC and exit profile available in the EU.

**If Estonia is chosen instead:** apply for e-Residency (state fee ~€100; card pickup including at Estonian embassy locations, allow 2–4 weeks), register the OÜ online (€265, 1–3 days), engage a Tallinn contact person/legal address (€200–400/yr), open Wise/Revolut IBAN, and — before the EIC full application — build the substance file (local contractor, board minutes taken in Tallinn, co-working address) [erhub +2].

## 7. Jurisdiction-Specific Items Requiring Local Tax Counsel

- **Portugal:** CFC/PE position of the Lisbon-resident founder; feasibility of an ATAJO advance ruling; PIT treatment (28% autonomous rate) on future dividends; interaction with the founder's existing obligations.
- **Ireland:** confirmation that board/management documentation preserves Irish tax residency against a "central management and control" argument; R&D credit and KDB eligibility for agent-security software; IP-transfer valuation.
- **Estonia (if chosen):** management-PE mitigation plan; reduced 14/86 dividend mechanics and any WHT on distributions to a non-resident individual under the Estonia–Portugal treaty.
- **Netherlands (if chosen):** dividend-WHT rate to the US parent under the NL–US treaty and Substance/LOB considerations.
- **Malta (if considered at all):** an effective-tax-rate analysis of the 6/7 refund for Portugal-CFC purposes before committing [landing.winnovart].
- **Any structure:** contemporaneous transfer-pricing documentation for the Delaware–EU services arrangement, and the SME linked-enterprise computation declaring the Delaware parent in the EIC self-assessment [seglerconsulting].
