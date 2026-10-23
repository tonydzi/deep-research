# DR26-08-24-MACANTON-02-1025 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. RANKED RECOMMENDATION

1st — Ireland (private company limited by shares, “Ltd”).
2nd — Netherlands (BV).
3rd — Lithuania (UAB), with Belgium (BV/SRL) as the reputation-heavier substitute if you would rather pay more for a “boring EU” story than for cheap remote ops.

Do not use Portugal (your constraint, and the right one). Do not use Estonia e-Residency as the EIC vehicle. Do not use Cyprus, Malta, or Luxembourg at this stage.

The job is not “pick the lowest headline tax.” It is simultaneously: (i) be a real SME established in a Horizon-eligible country, (ii) survive EIC / EIC Fund scrutiny as a US-owned company, (iii) not become a Portuguese tax resident or Portuguese PE by accident, (iv) sit cleanly under the existing Delaware C-corp, and (v) look normal to a US acqui-hire lawyer. Ireland is the only jurisdiction that is strong on all five. Netherlands is the close second if you want a higher, safer CFC buffer and Dutch R&D add-ons (WBSO) and can live with civil-law formation and harder banking. Lithuania is the cheap remote-capable EU operating company that is not a letterbox cliché; it loses on US-exit familiarity.

There is no true Delaware-equivalent in the EU. Delaware is “file, forget, common law, US counsel muscle memory, no local director, no POEM fight.” The EU will not sell you that bundle AND EIC eligibility AND protection from a Lisbon-resident founder’s Portuguese tax risk. The honest Delaware-like package in 2026 is: Irish common-law Ltd + Irish-resident non-executive director + registered office/company secretary + EUR account in the company’s name + the EU work actually booked in that company. That is the product you should buy.

Cleanest structure: Delaware C-corp parent (fundraising / US acqui-hire vehicle) → 100% Irish Ltd (EIC applicant / EU operating company). Do not make the Irish company a sister owned by you personally. Do not put a Luxembourg or Dutch HoldCo in the middle at pre-seed. Do not incorporate in Estonia “just for a PIC” with a plan to migrate later.

Flag up front: this is jurisdiction-selection analysis, not an opinion of Portuguese, Irish, Dutch, or US tax counsel. Several gates below are lawyer-only (especially Portuguese POEM/PE/CFC versus Irish 12.5%, and US GILTI on the sub).

────────────────────────────────
B. WHAT “EIC / HORIZON ELIGIBLE” ACTUALLY MEANS

B1. The legal gate (clean for every candidate on your list)

EIC Accelerator sits in Horizon Europe Pillar III. The applicant must be a single SME (or, in some work-programme tracks, a small mid-cap — Accelerator itself remains SME-centric) established in an EU Member State or a Horizon Europe associated country.

“Established” in the Rules for Participation / Financial Regulation sense = a legal person formed under the national law of that country, with legal personality, registered there. Ireland, Netherlands, Estonia, Cyprus, Luxembourg, Lithuania, Malta all satisfy this the day the company number is issued. Associated-country extras that also satisfy it: Norway, Iceland, Israel, UK (associated including EIC since 2024), Switzerland (association restored 2025 — verify the exact entry-into-force against the current work programme), Western Balkans, Ukraine, etc. A Delaware C-corp does not.

You then need a PIC on the EU Funding & Tenders portal, an SME self-declaration under Commission Recommendation 2003/361/EC, and (for Accelerator blended finance) EIC Fund KYC.

B2. The SME trap with a Delaware parent (this is the first real filter)

A 100% US parent is a linked enterprise. Headcount, turnover and balance-sheet of Delaware parent and EU sub are aggregated. At pre-seed with a tiny group, you are still an SME. After a US round that staffs up the parent, you may cease to be an SME and lose Accelerator / SME-only Horizon calls. There is a venture-capital “autonomous SME” carve-out for financial investors; it does not save you from an operating Delaware parent that owns 100% of the EU company.

Implication: apply to EIC before a large US round, or accept that later Horizon SME instruments may close.

B3. The substance / “not a US company in a European hat” gate (this is the one that kills Estonia)

The statute does not say “X points of office / local director.” In practice four filters sit on top of legal establishment:

1. Jury and “European impact.” Accelerator funds European innovation. A pitch that is obviously a Delaware product, Lisbon founder, and a mailbox in Tallinn or Limassol scores badly even when the PIC is valid.
2. Operational capacity in the Grant Agreement. The beneficiary must perform the action. Subcontracting to the US parent is possible but must be justified; you cannot book the grant in Ireland/Estonia and have every engineer and all IP live only in Delaware. Audits look at this.
3. EIC Fund equity diligence (if you take the equity component, typically ~€0.5–15m on top of a grant up to €2.5m). They invest in the applicant, not in the Delaware parent. They will look at UBO chain (you, Portugal; corporate parent, US), where the team sits, where IP sits, and whether the cap table of the sub is a hollow shell.
4. Post-2022 letterbox mood. Cascade-funding scandals and “e-Residency as a grant funnel” have made EISMEA / the Fund less amused by zero-substance Baltics and Mediterranean shells. This is not a published blacklist of Estonia. It is a higher probability of questions, slower KYC, and a worse jury read.

US ownership is not a bar. US-controlled EU SMEs win EIC. Chinese / sanctioned-jurisdiction control is the political problem, not Delaware. Agent-security / dual-use may trigger a security annex and extra questions; that is a content issue, not a country issue.

B4. Who cleanly passes, with nuance

- Ireland, Netherlands, Belgium, Luxembourg, Germany, France: clean legal establishment, no letterbox stigma, high EIC literacy. Pass.
- Lithuania: clean legal establishment, no e-Residency brand, respectable deep-tech/fintech EIC record. Pass, with a mild “why Lithuania?” on the European-impact narrative unless you put a real story on it (EU IBAN via LT-licensed EMI, NATO/security market, etc.).
- Estonia: legal pass, practical yellow. e-Residency OÜ is the poster child of the letterbox concern. Winnable if you add real Estonian (or at least EU) operational capacity; reckless if the OÜ is a virtual office and a card in your wallet.
- Cyprus / Malta: legal pass, practical yellow-to-red on jury + Fund KYC + US DD, not on the statute.
- UK Ltd: legally eligible as associated-country SME. Not an EU entity, so EU-only national top-ups, some cascade programmes, and the “we are an EU company” story are weaker. Association is political. I would not use UK as the EU-funding key unless Ireland/NL are blocked.
- Portugal: legal pass, and the strongest National Contact Point for a Lisbon founder — which is exactly why you are not doing it.

────────────────────────────────
C. THE PORTUGAL-RESIDENT FOUNDER PROBLEM (READ THIS BEFORE THE TABLE)

You live in Lisbon. You are (almost certainly) a Portuguese tax resident on worldwide income. Refusing a Portuguese company does not refuse Portuguese tax. A foreign company that you alone direct from your kitchen is how Autoridade Tributária re-characterises the thing you were trying to avoid.

Three Portuguese attacks, in order of lethality:

1. Place of effective management (POEM). Most EU states tax-resident a company if it is incorporated there or if it is effectively managed there. Dual-resident companies go to the treaty tie-breaker, classically POEM (some post-MLI treaties push this to mutual agreement — a lawyer must read the Ireland–Portugal and Netherlands–Portugal conventions as they stand, not the 2010 OECD model in the abstract). If POEM is Lisbon, the “Irish” company is a Portuguese tax resident that never registered in Portugal. That is worse than having incorporated in Portugal on purpose.

2. Permanent establishment in Portugal. Even if the company stays Irish/Dutch-resident, a PE exists if there is a fixed place of business in Portugal (home-office arguments are live) or a dependent agent who habitually concludes contracts (you, as sole director, signing from Lisbon). PE profits are Portuguese-taxed. The foreign company may also have to register for Portuguese social security as a foreign employer of a Portugal-based director/employee (Regulation 883/2004: you work from PT, you are in the PT social-security system). Payroll registration is a PE magnet.

3. CFC / imputation. Portugal’s ATAD CFC, aimed at controlled low-taxed subsidiaries. Working rule of thumb, to be confirmed by Portuguese counsel: “low tax” ≈ effective tax less than half of Portuguese CIT. Portuguese CIT is ~21% plus possible surcharges, so the danger zone is ETR ≲ 10–11%. Ireland 12.5% on trading income is the tightest of the serious options (probably clears; not a fact until a lawyer says so). Netherlands 19% clears comfortably. Luxembourg ~24–25% clears. Lithuania 16% standard probably clears; Lithuanian preferential small-company rates (historically 0%/5%, reformed upward in 2025 — verify the exact 2026 small-rate) may fail. Estonia 0% on retained profits fails the policy test in spirit and is the easiest CFC memo for AT to write. Malta ~5% effective after the refund fails. Cyprus 12.5% is Ireland-like on headline and fails if you actually live in the 2.5% IP box.

Personal tax, regardless of CFC: salary and director fees for work performed in Portugal are Portuguese-taxed. Dividends to you personally are Portuguese-taxed (treaty WHT reductions at source, then PT IRS with credit). NHR is closed to new applicants (2024). IFICI (“NHR 2.0”) is narrow and probably does not cover a startup founder unless you fit a listed R&D/teaching profile — do not assume it.

Mitigation that actually works (this is why Ireland/NL beat Estonia): at least one resident director in the company’s country with real decision rights; board meetings there (at least some in person); company secretary/registered office; a EUR account in the company’s name; the EIC project costs booked there; you in a documented role that does not make you the sole mind of the company. “Nominee director from a €99 pack” is not mitigation. AT and EIC Fund both know what that looks like.

────────────────────────────────
D. JURISDICTION-BY-JURISDICTION

D1. Ireland — Ltd (RANK 1)

EIC gate: SME incorporated in an EU Member State. Clean. Enterprise Ireland and Irish NCP know Accelerator cold. No letterbox discount. US-owned Irish software Ltds are a normal EIC / Horizon fact pattern.

Tax: 12.5% on trading income, 25% on passive, Knowledge Development Box 10% on qualifying IP with nexus. Large-group Pillar Two 15% is irrelevant at pre-seed. R&D tax credit is real (enhanced in recent budgets — confirm the 2026 rate/credit mechanics with an Irish tax accountant). Horizon grants are generally taxable receipts matched by deductible costs; they are not a tax-free gift.

Effective burden for a pre-seed product company that retains earnings: ~12.5% on Irish trading profit. If you keep residual IP value in Delaware and leave Ireland as a cost-plus / licensed EU operator, Irish profit is a routine margin and the 12.5% bill is small. If EIC foreground IP and EU sales sit in Ireland, 12.5% is the real rate — and that is still the EU’s best serious headline.

Setup / remote: 5–10 working days to incorporate once KYC is done. One director is enough under the Companies Act 2014; a Portuguese resident may legally be that director (EEA). That legal fact is a tax trap — see POEM. Company secretary and Irish registered office are required (service providers are a mature industry). Year-1 all-in if done properly, not via a mill: formation €1.5–3.5k; legal (constitution, share issue to the Delaware parent, intra-group IP licence, board terms) €4–8k; Irish-resident non-exec director €3–8k/year; CS + registered office €0.8–2k/year; accounting €2–3.5k. Realistic year-1 cash: €15–25k. Thereafter €6–12k/year plus the director.

Substance: Irish tax residence is incorporation or central management and control. If you are the only director and you run the company from Lisbon, Ireland will still put it on the register and Portugal can still claim POEM. Buy the Irish-resident director. Hold quarterly boards with Irish minutes. One annual in-person board in Dublin or Cork. Coworking / Enterprise Ireland hub desk is enough physical footprint at this stage; a lease is not required by company law.

Banking: the weak Irish point. AIB/Bank of Ireland have become hostile to non-resident-controlled startups. Practical path: Fire.com (Irish EMI, Iban in the Ltd’s name) and/or Revolut Business, then a traditional bank in parallel. Commission grant payments can go to an EMI IBAN in the beneficiary’s name; confirm with the agency at grant-agreement stage (this has been accepted in Horizon practice, but it is worth a one-line check, not an assumption carved in stone). Stripe, Wise as satellites. Time to a usable EUR account: 2–8 weeks, longest-pole item after the resident director.

Delaware fit: this is the textbook US-EU tech structure. English. Common law. US counsel has seen a thousand Irish Ltds. US–Ireland treaty is excellent. Transfer-pricing story is boring and therefore good.

PT risk: lowest among low-tax EU options if and only if you install Irish management. 12.5% vs Portuguese CFC half-rate is the one number a Lisbon tax lawyer must bless in writing. Social-security registration if you are on Irish payroll while living in PT.

Reputation / exit: best in the EU. Acqui-hire of a Delaware parent with an Irish Ltd underneath is routine. No AML hangover, no “why is this in Cyprus,” no German GmbH employment-law dread, no French works-council dread. EIC grant change-of-control = notify the authority, almost never a US-acquirer blocker. If EIC Fund holds minority shares in the Ltd, that is the only extra closing item (drag / buyout).

D2. Netherlands — BV (RANK 2)

EIC gate: as clean as Ireland. RVO is an excellent NCP. Dutch deep-tech and security companies win EIC regularly. The Hague / Dutch cyber cluster is a credible story for agent-security, better than Ireland’s FDI-tax story if a jury wants “European security industrial base.”

Tax: 19% on the first €200k (confirm 2026 bands), 25.8% above. Innovation box down to 9% on qualifying IP income with nexus. WBSO is a genuine extra non-dilutive R&D wage credit — material for a software product team. Participation exemption for dividends from subs (relevant later, not now).

Effective burden: higher than Ireland on ordinary profits; CFC-safer versus Portugal; WBSO can make the all-in cost of Dutch R&D labour competitive.

Setup / remote: notarial deed (civil law). Video-notary or power of attorney is possible; it is not e-Residency. KvK + UBO. Setup €3–6k legal/notary, plus a Dutch resident director if you want substance/treaty/WHT hygiene. Share capital can be €0.01. Slower and more ceremonial than Ireland.

Substance: tightened since the holding/financing reforms. An operating EIC applicant is not a letterbox holding, so you are not in the worst “substance points for WHT” bucket — but a Dutch resident director plus some Dutch cost base is still the right design. Do not run a BV as a one-man Lisbon shop and expect the treaty network to save you.

Banking: ING/ABN/Rabobank are famously grim for non-resident directors. bunq, Knab, Revolut. A Dutch IBAN is high-status once you have it. Budget time and a local director for the bank interview.

Delaware fit: also textbook (US parent → Dutch BV). Civil law, so US counsel is slightly less on autopilot than Ireland, but still very comfortable. US–NL treaty excellent. TP sophisticated.

PT risk: 19% is a much more comfortable CFC conversation than 12.5%. POEM/PE is the same family of problems as Ireland; same mitigation (NL director, boards in NL).

Reputation / exit: excellent. Mild extra friction versus Ireland: Dutch employment law if you actually hire in NL; works councils at scale (irrelevant at pre-seed). “Why Netherlands?” is a question with many good answers. US acquirers buy Dutch BVs constantly.

When to prefer NL over IE: you want CFC distance from Portugal; you will hire EU engineers and WBSO matters; the security-cluster story is part of the EIC pitch; you accept more formation friction.

D3. Lithuania — UAB (RANK 3)

EIC gate: legal pass. Real economy, not an e-Residency product. Fintech/deep-tech scene is real. Slight narrative discount versus IE/NL, not a statutory problem.

Tax: standard CIT 16% from 2025 (was 15%). Preferential small-company rates exist and were reformed in 2025 (historically 0% then 5%; verify the 2026 0%/6%-class rates before anyone “optimises” into them). For a Portugal-resident controller, take the standard 16% and do not play the small-rate game until a CFC memo says it is safe.

Setup / remote: fast, cheap, close to Estonia in convenience without Estonia’s brand problem. Formation €1–2.5k. Minimum capital historically €1,000 (verify current). English is workable in Vilnius professional services.

Substance: light in company law. POEM is therefore a live Portuguese issue unless you add a Lithuanian (or at least non-Portuguese) director and some local board practice. Easier to look like a mailbox than Ireland/NL, easier to defend than e-Residency Estonia.

Banking: practical advantage. Revolut’s EU banking licence is Lithuanian. Paysera. Traditional banks medium. Getting an EU IBAN in the UAB’s name is one of the easier remote jobs on this list.

Delaware fit: acceptable, not beautiful. US counsel will not hate it; they will ask why. Treaty network thinner than IE/NL. TP still doable (cost-plus EU operator under DE parent).

PT risk: 16% standard should be outside the ATAD “half rate” danger zone; preferential rates may not. POEM still needs a local mind, not just a Vilnius address.

Reputation / exit: neutral. Better than CY/MT/EE-eResidency. Worse than IE/NL/BE. For an acqui-hire, the acquirer is buying Delaware anyway; the UAB is a sub that needs ordinary KYC. Expect a “Baltics / AML” questionnaire, not a deal-breaker.

When to prefer LT: you will not fund Irish/Dutch director-level substance; you need a real EU IBAN fast; you will apply grant-only (no EIC Fund equity, so lighter KYC); you accept a slightly worse jury and DD story.

Belgium as alt-3rd: 25% standard, 20% SME rate on first €100k with a minimum director salary condition; Innovation Income Deduction can crush the rate on IP (and that slice can look CFC-low — be careful); 80% withholding exemption on researcher wages is excellent if you hire in BE; Brussels is next door to the Commission. Formation is notarial and heavier. Reputation for a US buyer is better than Lithuania. Remote-friendliness is worse. Pick BE over LT if the EIC jury / acquirer story matters more than cost.

D4. Estonia — OÜ via e-Residency (DO NOT PICK)

EIC gate: legal yes, practical the weakest of the true EU options. Plenty of OÜs have taken Union money; the mood has shifted against letterboxes. A jury that googles “e-Residency” during lunch is not the jury you want.

Tax: 0% on retained profits; 22% (2025+) when you distribute. Founder-friendly if you never pay dividends. CFC-hostile from Portugal: a 0% ETR on retained earnings is the example ATAD was written for. You do not get to say “but Estonia is EU so it cannot be CFC.” EU membership is not a CFC exemption.

Setup / remote: best in class. e-Residency card, online formation, no local director, hours not weeks, €300–1,500 plus the card. This is why people pick it, and why it is wrong for you.

Substance: legally near-zero. That is the POEM confession. AT’s memo writes itself: Portuguese resident, sole director, decisions in Lisbon, Estonian virtual office.

Banking: worse than the marketing. LHV is possible; traditional banks are not enthusiastic about non-resident e-residents; Wise/Payoneer/LHV mix. Doable, not pleasant.

Delaware fit: mechanically easy, optically a US parent with a Baltic token sub. Transfer pricing is simple because there is nothing there.

Reputation / exit: yellow to red. US DD asks “is this a real company or an e-Residency?” FATF-era Estonian banking stories still live in AML questionnaires. Not fatal. Not Delaware-like.

Use-case where EE would be rational: a bootstrapped digital-nomad services company with no EIC, no US parent, no Portugal CFC problem. That is not you.

D5. Cyprus — Ltd (REJECT)

Legal EIC pass. 12.5% headline, 2.5% IP box with nexus, 0% dividend WHT, notional interest deduction. Remote formation industry is mature. Substance “packages” are for sale and look like what they are.

Kill factors: Portuguese CFC risk if ETR is the IP box; banking still damaged; US acqui-hire counsel will treat Cyprus as offshore; EIC Fund KYC will be slow; “agent-security software in Cyprus” is a sentence you should not put in a jury deck.

D6. Luxembourg — SARL (REJECT AT THIS STAGE)

Legal EIC pass, superb holding-company jurisdiction, ~24–25% combined operating CIT (City rate; verify 2026), expensive notarial setup, €12k SARL capital, real substance expectations, banks that do not want a two-person AI SME. US buyers like Luxembourg on PE funds, not on pre-seed operating companies. “Why Luxembourg?” has no good pre-seed answer except “we over-structured.” Revisit as a HoldCo if a later round demands it. Not the EIC applicant.

D7. Malta — Ltd (REJECT)

Legal EIC pass. 35% with the 6/7 shareholder refund ≈ 5% effective on trading profits — a CFC magnet from Portugal and a political punching bag at OECD/EU level. Banking under permanent AML weather. US DD: iGaming/crypto halo. Not your jurisdiction.

D8. Others worth one paragraph

- UK Ltd: EIC-eligible as associated country, common law, English, US-buyer comfort second only to Ireland inside Europe, easy banking. Not EU. Association-risk. Misses EU-only top-ups. Honourable mention, not top 3.
- Germany GmbH / France SAS: superb EIC ecosystems (EXIST, CIR, Bpifrance), terrible remote vehicles, capital (€25k GmbH) and labour-law exit friction. Skip for a Lisbon-run second entity.
- Nordics: high EIC win rates, high tax, high substance cost. Skip.
- Spain: does not solve your Portuguese AT problem; it rhymes with it.
- Czechia / Poland: cheap EU, weaker EIC brand, Poland’s 9% small-CIT is CFC-suspicious, “why are we here?” at exit.

────────────────────────────────
E. TRADEOFF TABLE (1 = best among realistic options)

                     IE Ltd    NL BV     LT UAB    EE OÜ     CY Ltd    LU SARL   MT Ltd
EIC legal gate         1         1         1         1         1         1         1
EIC substance/jury     1         1         2         4         4         2         4
Headline CIT         12.5%    19/25.8%    16%*      0/22%     12.5%    ~24–25%   35→~5%
PT CFC comfort         2†        1         2*        5         3–5       1         5
Remote formation       2         3         1         1         1         4         2
Remote management      2         3         2         1         2         4         3
Local director needed  yes‡      yes‡     yes‡      no=trap    yes‡      yes       yes
Banking (remote)       3         3         1         3         5         4         5
EU IBAN practical      2         2         1         2         4         3         4
DE parent fit          1         1         2         3         4         2         4
US acqui-hire DD       1         1         3         4         5         2         4
Year-1 real cost      €15–25k  €18–30k   €6–12k    €2–5k    €8–15k    €20–40k   €10–18k
Time to company        1–2w      2–4w      1–2w     days      1–2w      3–6w      2–4w
Time to bank+PIC       4–8w      6–10w     3–6w     3–6w     6–12w     8–16w     8–16w

* Lithuania: standard 16% is the number that belongs in this table. Preferential small rates can ruin the CFC column.
† Ireland 12.5% is probably above Portugal’s ATAD half-rate test. That sentence is exactly what a Portuguese tax lawyer is for.
‡ “Needed” means tax-POEM and EIC/bank reality, not always company law.

────────────────────────────────
F. CLEANEST US-PARENT + EU-SUB STRUCTURE

F1. What to build

Delaware C-corp (existing)
  └── 100% Irish Ltd (new)  ← EIC / Horizon applicant, EU contracting party, EUR account

Shareholder of the Ltd = the Delaware corporation, not you personally. You remain a Delaware stockholder. Sister companies (you own DE and IE in parallel) still aggregate under the SME “linked enterprise” test if you control both, and they make a US exit worse (buyer has to pick up two groups). So sister is strictly worse.

No intermediate HoldCo.

F2. Where IP lives

This is the second landmine after Portuguese POEM.

Horizon default: results (foreground IP) belong to the beneficiary that generates them = the Irish Ltd if it is the applicant.

If all valuable IP stays in Delaware and Ireland is a grant funnel with a licence-in:

- EIC jury / audit: you are using Union money to enrich a US IP company.
- EIC Fund (if they take equity): they own a slice of a hollow Irish vehicle. You have sold the wrong company.
- Transfer pricing: Ireland must earn an arm’s-length return. That is fine technically and bad politically for a grant.

Recommended split:

- Background IP (the pre-existing agent-leash / control-audit core) stays in Delaware.
- Delaware grants the Irish Ltd a written exclusive licence for the EIC project and for EU exploitation, arm’s-length royalty or cost-plus for development services — pick one story and document it. A one-page licence now, a proper TP file when money is non-trivial.
- Foreground IP created under the grant sits in the Irish Ltd (Horizon-native).
- Do not strip foreground IP out of Ireland the week after the last payment. Clawback / relocation risk is real.

F3. Grant-only vs blended (EIC Fund equity)

Accelerator is grant up to €2.5m plus optional equity. Equity is subscribed in the applicant.

- Grant-only (or grant + you decline equity): Delaware keeps 100% of Ireland. Cap table stays clean. This is the default I would aim at for an acqui-hire-oriented pre-seed, unless the cheque size of the Fund changes the company’s life.
- Blended: EIC Fund becomes a minority shareholder of the Irish Ltd beside the Delaware parent. You will need a SHA with drag/tag, information rights, and a change-of-control buyout. A US buyer of Delaware does not automatically own the Fund’s Irish minority. This is solvable — the Fund has done parented structures — and it is extra lawyering at application and at exit.

Decide this before you submit, not after a jury yes.

F4. Who employs the Lisbon founder

Worst design: Irish Ltd, you sole director and only employee, you work exclusively from Lisbon. That is POEM + PE + Portuguese social security, with Irish 12.5% as a decorative detail.

Better designs (lawyer-pick, not founder-pick):

- You remain officer/employee of the Delaware parent. Ireland has an Irish resident director plus (optionally) a part-time Irish operator. Ireland subcontracts a defined R&D slice to Delaware within Horizon subcontracting rules, and keeps enough operational capacity of its own to be a real beneficiary.
- Or: you have dual roles, but Ireland’s board (with the Irish director) is the mind of the Irish company, you have an A1/social-security position that is coherent, and your Portuguese work for Ireland is either PE-accepted and filed (honest PE) or genuinely auxiliary.

“Honest small Portuguese PE of an Irish company” can be cleaner than “we swear there is no PE” if you will in fact run product from Lisbon. That is a Portuguese tax-lawyer call. It is still not a Portuguese subsidiary, which is what you wanted to avoid.

F5. US tax on the same chart

Irish Ltd is a CFC of the Delaware parent. At pre-profit this is paperwork. When profitable, GILTI / Subpart F is the US cost of a 12.5% sub. For an acqui-hire path it may never bite. US tax counsel should still see the structure once; it should not change the Ireland decision at this stage. Netherlands 19% is a slightly nicer FTC match if you ever become a profit machine.

────────────────────────────────
G. FIRST STEPS + COST/TIME FOR THE TOP PICK (IRELAND)

Do these in order. Do not file a CRO application on day one.

Week 0–2 — Portuguese tax memo (mandatory, not optional)
Instruct a Portuguese tax lawyer (not an incorporation agent) on: your personal residence status (ordinary / leftover NHR / IFICI); POEM risk of an Irish Ltd with a Lisbon founder; PE / home-office / dependent-agent; CFC versus 12.5%; social-security if you are a director or employee of a foreign co. Deliverable: a 5–10 page memo that either blesses Ireland with a resident-director design or tells you to go Netherlands at 19%. Cost: ~€2–4k. If this memo says “12.5% is too close,” rank 1 becomes Netherlands. That is the correct reason to switch, not vibe.

Week 1–3 — US and Irish corporate
US startup counsel: confirmation that the Delaware charter/bylaws/any SAFEs allow a 100% foreign sub; a one-page contribution/subscription for the Irish shares; GILTI note. Irish solicitor or a quality formation house (not a €99 website): Ltd limited by shares, subscriber = Delaware parent, ordinary shares, constitution that does not block a later EIC Fund share class if you might take blended finance.

Week 2–4 — People you are buying
Retain: (1) Irish company secretary + registered office; (2) a named Irish-resident non-executive director who will actually attend boards, not a shelf nominee. This person is the POEM insurance and often the bank’s comfort object.

Week 3–5 — Incorporate
CRO, RBO (UBO) filing (Delaware parent + you as natural UBO of the parent), share issue, statutory registers, corporation-tax registration, VAT only if you need it (B2B EU services can often wait; do not register VAT as a reflex).

Week 4–8 — Bank + PIC (longest pole)
Open Fire.com and/or Revolut Business in the Ltd’s exact legal name; start AIB/BOI in parallel. Register the PIC on the Funding & Tenders portal; complete SME self-assessment with the Delaware parent declared as linked; upload CRO extract. PIC validation is days to a few weeks.

Week 5–8 — Intra-group paper
Background IP licence Delaware → Ireland. Services / cost-sharing or cost-plus agreement for any US work that supports the EIC action. Board calendar: quarterly Irish boards, one physical meeting per year. Minute the place of meetings.

Week 6–12 — EIC readiness, not formation
Formation is not the Accelerator application. You still need the short proposal, TRL story, metrics from the paid pilots, freedom-to-operate, and a project whose work package is honestly Irish-beneficiary-shaped. Talk to the Irish NCP / an Enterprise Ireland hub. EI money itself often wants Irish jobs; even if you do not take EI, the coaching is useful. Typical Accelerator clock after the company exists: a cut-off cycle of many months, not weeks.

Money and time, top pick, if done like a company you expect a US buyer to open:

- Clock to a living Irish Ltd: ~2–4 weeks.
- Clock to bank + PIC: ~4–8 weeks.
- Clock to a non-embarrassing EIC applicant: ~2–4 months (substance + paper + proposal).
- Year-1 cash: €15–25k Ireland-side, plus €2–4k Portuguese memo, plus US counsel a few thousand if not already on retainer.
- Recurring: €6–12k compliance + €3–8k Irish director.

That is more than e-Residency by a factor of five to ten. It is cheaper than one Portuguese tax assessment on a mis-homed company, and cheaper than an EIC Fund that walks because the beneficiary is a shell.

────────────────────────────────
H. WHAT IS LAWYER-ONLY (DO NOT TAKE FROM THIS NOTE)

- Portuguese CFC: is Irish 12.5% “less than half” of the Portuguese tax that would have been due, under the actual IRC/ATAD transposition and your facts (2026 rates, surcharges, whether the comparison is CIT-to-CIT or something else).
- Portuguese POEM and PE: Ireland–Portugal treaty after MLI; whether the tie-breaker is still automatic POEM or competent-authority; home-office PE given your Lisbon working pattern.
- Portuguese social security / A1 for a director or employee of an Irish company living in Lisbon; whether that registration is a PE admission.
- Your personal status: ordinary resident vs any remaining NHR vs IFICI eligibility.
- Irish: close-company rules, director-as-employee, R&D credit on software, taxability of the Union grant, VAT on pilots.
- Whether an Irish-resident director who is a service-company professional is enough “central management and control” (usually yes if boards are real; no if they rubber-stamp your Telegram instructions).
- US: CFC/GILTI, transfer-pricing of the licence, 83(b)/QSBS interactions (QSBS is a US-parent story; the sub does not kill it by existing).
- EIC Fund SHA if blended; Horizon subcontracting limits for sending work to Delaware; security annex for agent-security / dual-use.
- Bank KYC: UBO of a US C-corp with a Portugal-resident founder — some EMI onboarding will ask for a Portuguese tax number and source-of-funds. That is compliance, not a reason to use Cyprus.

Anything in this report that looks like a rate, a threshold, or a treaty outcome is a research snapshot, not a filing position.

────────────────────────────────
I. HOW TO CHOOSE IN ONE PARAGRAPH

If the Portuguese memo blesses 12.5% with an Irish resident director, incorporate an Irish Ltd as a 100% Delaware sub, take grant-first EIC, put foreground IP in Ireland, keep background IP in Delaware, and run boards in Ireland. That is the Delaware-equivalent you can actually have: common law, English, US-buyer native, EIC-native, CFC-plausible, remote-manageable with one paid local director.

If the Portuguese memo flinches at 12.5%, same chart with a Dutch BV at 19% and WBSO as the consolation prize.

If you refuse to buy a local director at all, you are not in the EIC-plus-Portugal-resident problem set; you are in the e-Residency problem set, and you should not raise Union money through that vehicle.

Estonia, Cyprus, Malta, Luxembourg, and a Portuguese company are how this goes wrong: the first three on substance/CFC/reputation, Luxembourg on stage-mismatch, Portugal because you already decided AT is not a partner you want on the cap table.
