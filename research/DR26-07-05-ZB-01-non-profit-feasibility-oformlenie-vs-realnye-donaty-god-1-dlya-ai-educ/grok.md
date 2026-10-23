# DR26-07-05-ZB-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: Grok (SuperGrok, Expert mode, 121 sources) -->
<!-- topic: DR26-07-05-ZB-01 Non-profit feasibility — оформление vs донаты год-1 (AI-education школа) -->
<!-- date: 2026-07-05 ~03:00, collected by Max (Claude Code, HP17) verbatim -->
<!-- origin: приватный чат -->

Thought for 2m 18s

Non-profit feasibility report: AI-education open-source project ("open second brain" / Claude Code & AI agents tools).
Solo PhD founder (ex-crypto, Lisbon/Portugal tax resident), team of 3, $0 company cash, limited personal reserves. Live build-in-public project with growing audience. Core question: formalize as non-profit for donations/grants or stay informal + platforms?

I researched primary sources (IRS.gov, e-resident.gov.ee, official PT portals, HCB site, ProPublica 990s, foundation/accelerator pages, legal guides, Open Collective docs, GitHub Sponsors policy) as of mid-2026. All numbers are realistic ranges for small/new entities in tech/AI education, not outliers. "Small/new" = early-stage, < $100-200k raised in year 1, limited track record.

## 1. Comparative table of 4 routes

Key assumptions for estimates (sourced below):

"Launch cost" = all-in year-1 setup (government fees + lawyer/accountant/service + basic compliance).
"Annual support" = ongoing reporting/audit/accounting excluding percentage fees on raised funds.
"Founder time" = realistic hours for non-lawyer founder (prep + year-1 admin).
Expected donations year 1 = net to project after platform/host fees where applicable; pessimistic/base/optimistic for this profile (growing but early AI-ed open-source audience, no big existing network). Primary source early: individual recurring small donations from audience. Grants rare in true year 1 without traction or fiscal sponsor.
Hassle 1-5 = composite of bureaucracy, governance, banking/compliance risk for non-resident/remote founder, hidden costs that kill micro-non-profits.

| Route | Time to legally accept donations | Full launch cost (year 1) | Annual support cost (excl. % fees) | Founder time (prep + year-1 admin hrs) | Expected donations year 1 (pess / base / opt) | Hassle 1-5 |
|---|---|---|---|---|---|---|
| (a) US 501(c)(3) standalone (remote/foreign founder) | 6–12+ months (formation + IRS 3–9 mo for Form 1023; foreign ineligible for 1023-EZ) | $3k–10k+ (IRS $600 + state ~$100 + lawyer/translations/compliance $2k–8k+; higher for foreign docs) | $5k–15k+ (Form 990 prep/accountant, state charity registrations, possible audit) | 150–300+ prep + 80–150+ ongoing | Delayed start → low initial; then $10–60k (credibility helps but hard to launch) | 5 (banking for non-res is killer; compliance overload common failure mode) |
| (b) Portuguese associação sem fins lucrativos | 1 day – 4 weeks (Associação na Hora possible) | €800–1.5k (€300 na Hora + €500–1k legal/notary/custom statutes) | €1–3.5k (certified accountant, double-entry SNC-ESNL books, annual filings to IRN) | 20–60 setup + 30–80 ongoing | $15–70k (local patronage tax benefits + EU/PT digital skills grants access) | 2.5–3 (governance + accounting standards moderate; local banking easy) |
| (c) Estonian MTÜ via e-Residency | 1–2 weeks (5 business days registration once e-res ready) | €500–1k (service ~€350 + state/address fees) | €0–1.5k (simple annual report; optional cheap accountant or €29 AI tool) | 10–40 setup + 10–40 ongoing | $15–60k (EU access; low cost helps net) | 1.5 (fully digital, minimal red tape) |
| (d) Fiscal sponsorship (e.g. Hack Club Bank / suitable OC host) — NO own entity | 1–4 weeks (application + approval) | $0 upfront | Included in host fee (typically 5–10%) | 5–20 setup + 10–40 admin/reporting to host | $10–50k base (fastest start; unlocks US tax-deductible donations + some grants) | 1 (lowest; host handles board, 990, banking, compliance) |

Sources & notes for table:

- US 501(c)(3): IRS Form 1023 instructions & processing times; foreign orgs need full 1023 + translations, ineligible for EZ; state fees ~$30–125 typical; lawyer quotes & guides confirm $2k–10k+ realistic for simple cases (higher for foreign). Annual 990 + state solicitation regs common pain point. [IRS]
- PT associação: Official gov.pt «Associação na Hora» €300 (other associations); legal guides confirm €500–1k extra for custom + tax setup. Annual: SNC-ESNL bookkeeping + IRN filings mandatory; public utility status (extra step) unlocks stronger IRC exemptions/VAT benefits if mission-aligned. [www2/gov.pt]
- Estonia MTÜ: e-resident.gov.ee official guide; state fee low, services ~€350–500 incl. address; annual report mandatory but simple/digital (even zero activity); PBO status optional for donor incentives. [e-resident]
- Fiscal (HCB example): Official HCB site — exactly 7% revenue fee, no startup/transaction/card/processing fees; full 501(c)(3) umbrella, bank account, debit cards, automatic taxes/accounting/990, payroll, grant support (tax-deductible). 1,500+ projects including education/coding/hackathons. Apply online; global payouts (Wise supported). OC hosts typically 4–10% host fee + processor. [hackclub]
- Expected donations: Benchmarked against freeCodeCamp (launched informal 2014, formal 2017; community small recurring $5/mo donations drove growth to ~$750k budget by ~2021) and The Odin Project (early commercial tie-in then donations; recent OC cumulative modest tens of k). Small new tech/AI-ed non-profits: audience-driven individual donations dominate year 1; grants (Fast Forward $25k+ accelerator for early AI/tech non-profits, Mozilla, Schmidt Futures, EU/PT digital skills) usually require traction or fiscal sponsor. Corporate sponsorships rare in true year 1 without connections. Realistic median for tiny new: low-to-mid tens of k. [freecodecamp]

## 2. Verdict + threshold + recommended route #1

Formalizing an own legal non-profit in year 1 does NOT pay off for your profile.
Setup + annual compliance costs + founder time opportunity cost (diverted from content/tools/audience growth — your real moat) typically exceed incremental benefits in year 1 for a small new project.

Break-even threshold (rough, conservative): Sustainable net donations/grants of ~$40–60k+ per year (after all fees/compliance) plus either (a) specific larger grant/accelerator that requires own 501(c)(3) or local entity or (b) clear path to EU/PT public-utility scale benefits. Below that, the admin/governance drag and cash burn usually destroy more value than they create. Many micro-non-profits die in years 1–2 exactly from this mismatch.

Route #1 for your exact case (Portugal tax resident, limited reserves, growing open-source AI-ed audience, focus on building not admin):
**Start with fiscal sponsorship (Hack Club Bank or suitable Open Collective host) + informal platforms in parallel. Do NOT form your own entity yet.**

Why this wins:
- Zero upfront cash outlay.
- 1–4 weeks to start accepting tax-deductible (US) donations and position for grants that require 501(c)(3).
- Host handles board/governance/990/banking/compliance — exactly the "hemorroid" factors that kill micros.
- 7% (HCB) or 5–10% host fee is transparent and only paid on what you raise.
- Perfect fit for open-source/AI-ed/coding-education projects (HCB explicitly supports these; many similar grassroots projects thrive).
- Keeps you 100% focused on publishing content/tools and growing audience (the activity that actually drives future donations/grants).
- Builds 12–18 months of real track record + financials → then decide on own PT associação (or Estonia) with data, not hope.

Strong #2: If you want full local control and PT tax-resident simplicity now and can stomach €1–2k setup: form Portuguese associação sem fins lucrativos via Associação na Hora. Fastest/cheapest own-entity route that fits your location and unlocks local/EU grant pathways. Estonia is close second for minimalism.

Avoid standalone remote US 501(c)(3): Disproportionate hassle (especially banking/KYC for non-resident-controlled entity) and delay vs. using fiscal sponsorship for the exact same US tax-deductible + grant benefits.

## 3. If verdict = "do not formalize own entity yet" — cheapest ways to accept donations right now

Use these today (no entity required; works alongside fiscal sponsorship):

- **GitHub Sponsors** (primary for open-source AI tools/content): 0% platform fee on personal accounts (100% minus normal payment processing ~3%; GitHub has covered processing in past promotions). Excellent for OSS projects. Can combine with OC in some setups. [donatr]
- **Ko-fi**: ~5% (Gold tier) + Stripe/PayPal processing (~2.9–3.5%). Very popular with creators; one-time + recurring; low friction.
- **Open Collective** (with or without host): Transparent ledger great for build-in-public. With host: adds 5–10% but unlocks legal/tax wrapper. Without host: lower fees but limited legal protection.

Effective total cost on these platforms: typically 5–10% all-in (far cheaper than full non-profit compliance). Many successful small open-source/ed projects run on exactly this stack for years before (or instead of) formalizing.

Next practical steps (recommended order):
1. Apply to Hack Club Bank (or equivalent OC host) this week — start collecting with 501(c)(3) status.
2. Set up/optimize GitHub Sponsors + Ko-fi buttons on your repos/site.
3. Continue build-in-public aggressively (audience growth = donation growth engine).
4. Track actual monthly recurring + one-time for 3–6 months.
5. Re-evaluate formal entity (PT associação most logical) once you have real numbers or a specific grant target that requires it.
6. For paid consulting (your ex-crypto/PhD expertise): Run via separate personal/LLC with clear separation policies. Common, doable, and low-conflict if finances/time/IP are strictly arm's-length and documented.

Founder compensation ("reasonable compensation"): Allowed in all routes (US 501(c)(3), PT associação, Estonia MTÜ, and most fiscal hosts) for actual documented services. Must be board-approved (or host-approved), comparable to market data for similar roles/size, and not excessive (private inurement risk). Start low/minimal or as contractor while capital is tight; document everything rigorously. PT/Estonia: straightforward local employment/SS rules for tax resident. Fiscal: usually possible via budgeted service line item.

Commerce compatibility (non-profit + paid consulting/LLC): Fully possible and standard. Use non-profit/fiscal project strictly for free open content, donations, grants, and mission work. Run paid AI consulting/advisory via separate personal brand or LLC. Requirements: arm's-length transactions, no cross-subsidization (non-profit cannot fund for-profit activities or vice versa), clear time tracking, conflict-of-interest policy, and disclosure where branding overlaps. Many tech/AI founders operate exactly this hybrid. Risks highest in strict US 501(c)(3) private-benefit doctrine; lower but still real in PT/Estonia (non-distribution constraint). Get basic legal/tax review in Portugal for your specific setup.

Main hidden "hemorroid" costs that kill micro-non-profits in years 1–2 (all routes except pure fiscal/platforms):
- Founder time sink on governance (board recruitment, minutes, decisions) and compliance instead of mission.
- Unexpected accounting/legal bills when rules are more complex than expected.
- Banking/KYC/wire issues for non-residents or cross-border grants.
- Donor reporting/stewardship burden once any grant arrives.
- Scaling compliance (Form 990 complexity grows fast; state registrations multiply).

Fiscal sponsorship removes most of these. Own entity adds them.

**Bottom line:** Your highest-ROI move right now is fiscal sponsorship (HCB or equivalent) + GitHub Sponsors/Ko-fi. Protect your limited reserves and time. Build the audience and proof. Formalize later (PT associação likely) only when the numbers clearly justify it. This is the pragmatic, data-driven path for exactly your constraints and strengths.

(121 sources cited in original chat)
