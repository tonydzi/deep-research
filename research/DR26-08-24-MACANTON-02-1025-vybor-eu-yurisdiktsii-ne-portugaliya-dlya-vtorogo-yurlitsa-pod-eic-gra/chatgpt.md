# DR26-08-24-MACANTON-02-1025 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# EU entity for EIC/Horizon funding: jurisdiction recommendation

## Executive conclusion

The best overall choice is:

1. **Ireland — recommended**
2. **Netherlands — safest higher-cost alternative**
3. **Estonia — only with genuine Estonian/EEA operations; not as a Lisbon-managed e-Residency shell**

The clean structure is:

**Existing Delaware C-corp → 100%-owned Irish Ltd → EU applicant, employer, R&D and customer-contracting entity.**

Keep pre-existing background IP in Delaware and license it to the Irish subsidiary on arm’s-length terms; let the Irish beneficiary own grant-funded results it generates, with carefully drafted exploitation licences back to the parent where permitted. Horizon Europe’s default rule is that the beneficiary generating a result owns it, and transfer to a non-associated country such as the US may be subject to grant-agreement conditions. [European Commission IP Helpdesk](https://intellectual-property-helpdesk.ec.europa.eu/regional-helpdesks/european-ip-helpdesk/europe-frequently-asked-questions_en)

For the EIC Accelerator, apply initially for **grant-only**, not blended finance. Grant-only provides up to €2.5 million once per legal entity, covers up to 70% of eligible TRL 6–8 costs, and avoids placing EIC equity into a subsidiary underneath the Delaware cap table. [EIC 2026 Work Programme](https://eic.ec.europa.eu/system/files/2025-11/EIC-Work-Programme-2026.pdf), [EIC applicant guide](https://eic.ec.europa.eu/document/download/9d96fbf3-4d85-4ad0-9483-c77ce348111d_en?filename=EIC+Accelerator+guide+for+applicants_WP26.pdf)

The overriding risk is not the incorporation country. It is **Portugal**. If the founder remains the real decision-maker, develops the product, negotiates contracts and runs the company from Lisbon, Portugal can argue that the foreign company’s effective management or a permanent establishment is in Portugal. Portuguese law treats a company as resident when its head office or effective management is in Portugal. [OECD Portugal residency summary](https://www.oecd.org/tax/automatic-exchange/crs-implementation-and-assistance/tax-residency/Portugal-Tax-Residency.pdf), [PwC Portugal corporate-residence summary](https://taxsummaries.pwc.com/portugal/corporate/corporate-residence)

A foreign registered address, virtual office or nominal director does not cure that problem. The operating and governance facts must genuinely place part of the business in the chosen jurisdiction.

## 1. EIC and Horizon eligibility

All the principal candidates—**Ireland, Netherlands, Estonia, Cyprus, Luxembourg, Lithuania and Malta**—are EU Member States. Finland, included below as an additional strong option, is also an EU Member State. Each therefore passes the same country-level EIC/Horizon gate. The 2026 EIC Work Programme expressly lists all of them among eligible Member States. [EIC 2026 Work Programme](https://eic.ec.europa.eu/system/files/2025-11/EIC-Work-Programme-2026.pdf)

For EIC Accelerator, the operative gate is:

- The applicant must itself be a start-up or SME established in an EU Member State or Horizon Europe Associated Country. [EIC applicant guide](https://eic.ec.europa.eu/document/download/9d96fbf3-4d85-4ad0-9483-c77ce348111d_en?filename=EIC+Accelerator+guide+for+applicants_WP26.pdf)
- A non-associated-country applicant can submit at the earlier stage but must have established or relocated the SME and prove its effective establishment in the EU/Associated Country by the full proposal. [EIC applicant guide](https://eic.ec.europa.eu/document/download/9d96fbf3-4d85-4ad0-9483-c77ce348111d_en?filename=EIC+Accelerator+guide+for+applicants_WP26.pdf)
- An EU parent or sister company may apply if it independently satisfies the eligibility and SME requirements, but the Commission warns against structures designed merely to circumvent eligibility; activities conducted in the non-eligible company are not counted as activities of the EU applicant. [EIC Accelerator FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-accelerator_en)
- The SME assessment aggregates financial and staff data from directly and indirectly linked enterprises. Consequently, the Delaware parent’s headcount and financial data must be disclosed, although a pre-seed Delaware parent is unlikely by itself to push the group outside the SME thresholds. [Funding & Tenders Participant Register](https://ec.europa.eu/info/funding-tenders/opportunities/portal/screen/how-to-participate/participant-register), [Funding & Tenders Online Manual](https://ec.europa.eu/info/funding-tenders/opportunities/docs/2021-2027/common/guidance/om_en.pdf)

There is no published “one local employee” or “six-month substance” safe harbour. Nevertheless, a newly formed subsidiary with no people, costs, bank activity, decision-making or IP rights presents exactly the circumvention/effective-establishment fact pattern that should be avoided.

Other funding nuances:

- Ordinary Horizon Europe calls accept EU legal entities, but many calls require a multinational consortium; incorporation alone does not make a company eligible for every topic. [European Commission Horizon Europe](https://research-and-innovation.ec.europa.eu/funding/funding-opportunities/funding-programmes-and-open-calls/horizon-europe_en)
- EIC Pathfinder Open does not accept a single legal entity; Pathfinder Challenges may accept single applicants. Pathfinder is normally aimed at TRL 1–4 research rather than a product already being piloted. [EIC Pathfinder FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-pathfinder_en)
- EIC Transition can accept a single EU SME, but the proposal must build on an eligible previously funded Horizon/EIC/ERC result at TRL 3–4. A newly incorporated company with no eligible linked project does not obtain Transition eligibility simply from its domicile. [EIC Transition](https://eic.ec.europa.eu/eic-funding-opportunities/eic-transition_en), [EIC Transition FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-transition_en)
- Estonia, Cyprus, Lithuania and Malta also qualify as “widening countries” for the EIC Pre-Accelerator; the next call is planned for 2027, not 2026. Ireland, Netherlands, Luxembourg and Finland do not pass that additional widening-country gate. [EIC Pre-Accelerator FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-pre-accelerator_en)

For this company, EIC Accelerator Open is the most plausible route, but only if the product has completed all aspects of TRL 5 and can credibly start grant-funded work at TRL 6. The EIC also tests whether the innovation is genuinely deep-tech, breakthrough and supported by a defensible IP strategy; “AI software with pilot revenue” does not automatically meet those tests. [EIC applicant guide](https://eic.ec.europa.eu/document/download/9d96fbf3-4d85-4ad0-9483-c77ce348111d_en?filename=EIC+Accelerator+guide+for+applicants_WP26.pdf)

## 2. Ranked trade-off table

Tax figures below are entity-level rates before shareholder taxation, VAT, payroll taxes, withholding taxes and any Portuguese or US top-up.

| Rank | Jurisdiction | Corporate-tax burden | Formation, management and banking | Substance, tax and due-diligence assessment |
|---|---|---|---|---|
| **1** | **Ireland** | 12.5% on trading income; 25% generally applies to non-trading/passive income. [Irish Revenue](https://www.revenue.ie/en/tax-professionals/tdm/income-tax-capital-gains-tax-corporation-tax/part-02/02-02-01.pdf) | Online A1 filing costs €50; the standard target is ten working days and an approved formation-agent scheme targets five working days. [CRO fees](https://cro.ie/Publications/Fees/Company/), [CRO timing](https://cro.ie/registration/company/registration-methods/) A Lisbon-resident founder satisfies the statutory EEA-resident-director condition, avoiding the €25,000 bond; a single-director company needs a separate secretary. [Companies Act s.137](https://www.irishstatutebook.ie/eli/2014/act/38/section/137), [PwC formation guide](https://www.pwc.ie/publications/2024/guide-to-company-formation-ireland.pdf) | Best balance. English-speaking common-law system, familiar to US buyers and counsel. [Irish Department of Enterprise](https://enterprise.gov.ie/en/publications/publication-files/ireland-connected-trading-and-investing-in-a-dynamic-world-summary-report-.pdf) An Irish registered office and Irish activity address are required, and actual Irish decision-making/operations should be added. [PwC formation guide](https://www.pwc.ie/publications/2024/guide-to-company-formation-ireland.pdf) |
| **2** | **Netherlands** | 19% up to €200,000 taxable profit and 25.8% above that in 2026. [KVK](https://www.kvk.nl/en/finance/dutch-tax-rates-in-2026/) | A BV requires a civil-law notary, which can work online; typical notary cost is €500–€1,500, accounting €600–€1,800 annually, and minimum starting capital is €0.01. [Business.gov.nl](https://business.gov.nl/running-your-business/legal-forms-and-governance/private-limited-company-in-the-netherlands/) Conventional bank onboarding for foreign founders may take two to eight weeks, although a SEPA IBAN can be used. [Business.gov.nl banking](https://business.gov.nl/coming-to-the-netherlands/living-in-the-netherlands/opening-a-dutch-business-bank-account/) | Strongest “substance-first” alternative. Registration from abroad requires a Dutch business address and structural physical economic activity, making the company harder to attack as a brass plate but more expensive to run. [Business.gov.nl foreign-founder guide](https://business.gov.nl/starting-your-business/starting-situations/starting-a-business-in-the-netherlands-from-abroad/) Very good US-acquirer diligence profile. |
| **3** | **Estonia** | No regular tax on retained profit; distributions are taxed at company level at 22/78 of the net dividend, equivalent to 22% of pre-distribution profit. [Estonian Tax Board](https://www.emta.ee/en/business-client/taxes-and-payment/income-and-social-taxes/taxation-dividends) | E-Residency costs €150, online OÜ registration €265, a contact person generally €200–€400 annually, and a supported first-year setup is approximately €1,300. [e-Residency cost guide](https://learn.e-resident.gov.ee/hc/en-gb/articles/360000625118-Costs-fees) If the board is outside Estonia, a licensed contact person is required, but that person has no authority to manage the company. [e-Residency contact-person guide](https://learn.e-resident.gov.ee/hc/en-gb/articles/360000624858-Contact-person-legal-address) | Best administration, weakest remote-shell tax answer. Estonia expressly warns that an e-resident company managed abroad will probably have a foreign PE and may owe tax where it is managed. [Estonian Tax Board](https://www.emta.ee/en/business-client/registration-business/non-residents-e-residents/tax-liabilities-companies) Under a Delaware parent, US NCTI also largely destroys the “tax-free retention” advantage. |
| **4** | **Lithuania** | Standard rate 17% in 2026; qualifying small entities with revenue not above €300,000 may obtain 0% for their first two periods and 7% thereafter, subject to statutory exceptions and related-entity rules. [Lithuanian Ministry of Finance](https://finmin.lrv.lt/en/competence-areas/taxation/main-taxes/corporate-income-tax/) | A UAB normally needs €1,000 capital and can be registered in roughly one to two weeks; no residence requirement applies to the general manager or governing bodies. [Invest Lithuania](https://investlithuania.com/wp-content/uploads/Start-your-business-in-Lithuania.pdf) | Good value and eligible for the 2027 widening-country Pre-Accelerator. The 17% standard regime is tax-defensible; the 0%/7% small-company regime creates Portuguese CFC and US NCTI exposure and should not be assumed available to a Delaware-controlled group. |
| **5** | **Cyprus** | Standard corporate tax increased to 15% from 1 January 2026. [Cyprus government](https://www.gov.cy/media/sites/11/2025/05/CY-APR-2026-Final.pdf) | Market offerings show fully remote incorporation in five to ten business days from roughly €950–€2,000, depending on tax registration, secretary, office and banking support. [Zeno Legal](https://www.zenolegal.com/company-registration) Companies must maintain accounts and prepare audited financial statements through an approved auditor. [Business in Cyprus](https://www.businessincyprus.gov.cy/doing-business-in-cyprus/start-your-business/registering-for-income-tax-and-value-added-tax/) | Tax rate is now reasonably clean, and Cyprus has the widening-country advantage. It ranks below Lithuania because real local management is advisable and nominee-director/secretary arrangements generate more beneficial-ownership and substance diligence. |
| **6** | **Finland** | 20% corporate income tax. [Finnish Tax Administration](https://www.vero.fi/en/businesses-and-corporations/taxes-and-charges/limited-companies-and-cooperatives/income-tax/) | The 2026 guided online Oy setup costs €300. [Finnish Patent and Registration Office](https://www.prh.fi/en/companiesandorganisations/yrityksen_perustaminen/osakeyhtio/guided-package-online.html) | Strong legal and buyer reputation, straightforward tax rate and no low-tax optics. It is less attractive here because the rate is higher, the online process is in Finnish/Swedish, and it lacks the widening-country benefit. |
| **7** | **Malta** | 35% headline rate; a 6/7 shareholder refund can produce an effective 5% on qualifying trading income, while an optional final 15% regime is available for a minimum five-year election. [Malta Tax and Customs](https://mtca.gov.mt/business-tax/corporate/corporate_tax), [5% calculation](https://mtca.gov.mt/docs/default-source/documents/guidelines-in-relation-to-the-consolidated-group-%28income-tax%29-rules.pdf), [15% final regime](https://mtca.gov.mt/docs/default-source/documents/legislative-developments/2026/overview-of-recently-published-legal-notices-july-to-december-2025-memo.pdf) | Electronic registry fee begins at €100, but market formation packages are roughly €1,500–€3,500 before continuing accounting/CSP costs. [Malta Business Registry](https://www.mbr.mt/website-pages/fee-structure), [2026 market-cost survey](https://maltacompanycost.com/) Corporate tax returns must be filed by a registered tax practitioner. [MTCA](https://mtca.gov.mt/business-tax/corporate/Income_Tax) | The refund regime creates cash-flow, compliance, PT-CFC and US-NCTI complexity. Electing 15% removes much of the tax advantage while retaining Malta’s administrative complexity. |
| **8** | **Luxembourg** | About 23.87% combined burden for a Luxembourg City company. [Luxembourg 2026 budget plan](https://igf.gouvernement.lu/dam-assets/fr/dossiers/projet-de-plan-budg%C3%A9taire/eng/draft-budg-plan-2026.pdf) | A normal SARL requires a notary and €12,000 fully paid capital. [Guichet.lu](https://guichet.public.lu/en/entreprises/creation-developpement/forme-juridique/societe-capitaux/sarl.html) Market timing is approximately two to six weeks. [Cerno Law](https://www.cerno.lu/blog/creer-sarl-luxembourg-etapes) | Excellent institutional reputation but overbuilt, slow and expensive for a pre-seed operating software company. Its strengths are more useful for investment-holding and fund structures than for this EIC operating subsidiary. |

## 3. Why Ireland wins

Ireland has the best combination of:

- A normal operating-company tax rate that is low but not so low that the company automatically resembles a CFC/tax-deferral vehicle. Ireland’s 12.5% trading rate is above the current Portuguese CFC benchmark of 9.5% in a straightforward like-for-like calculation. The Portuguese test uses actual foreign tax relative to 50% of Portuguese tax, so the final result must be modelled rather than inferred solely from the headline rate. [Chambers Portugal 2026](https://practiceguides.chambers.com/practice-guides/corporate-tax-2026/portugal)
- A legal system and documentary language familiar to Delaware counsel and US acquirers. Ireland is the principal English-speaking common-law EU jurisdiction. [Irish Department of Enterprise](https://enterprise.gov.ie/en/publications/publication-files/ireland-connected-trading-and-investing-in-a-dynamic-world-summary-report-.pdf)
- Fast remote incorporation without a compulsory Irish-resident director. The founder’s Portuguese/EEA residence satisfies the statutory EEA director rule, although it does not create Irish tax substance. [Companies Act s.137](https://www.irishstatutebook.ie/eli/2014/act/38/section/137)
- Straightforward EUR banking. Revolut Business accepts an incorporated active company with an eligible EEA legal address and conducts onboarding online; Wise supports EEA limited companies and provides EUR IBAN details, subject in both cases to KYC and operating-address checks. [Revolut](https://www.revolut.com/en-IE/business/open-startup-bank-account/), [Wise](https://wise.com/help/articles/2977974/can-my-business-use-wise), [Wise account details](https://wise.com/help/articles/2897226/what-is-a-wise-account)
- A clean EIC applicant story if the Irish company actually controls the EU project, project personnel, grant budget and exploitation rights.

Ireland is not a zero-substance solution. Its incorporation materials require an Irish registered office and an Irish address for the proposed activity, even though central administration may be elsewhere. [PwC formation guide](https://www.pwc.ie/publications/2024/guide-to-company-formation-ireland.pdf)

## 4. Recommended ownership and IP structure

### Recommended

**Founder/investors → Delaware C-corp → 100% Irish Ltd**

The Irish subsidiary should:

- Be the EIC/Horizon applicant and grant beneficiary.
- Employ or contract the team performing the EU-funded work.
- Hold the Irish/EU bank account, books, project budget and procurement contracts.
- Invoice EU pilots and customers where commercially appropriate.
- Possess documented rights to use the Delaware background IP.
- Own foreground code, models, security research and other results it generates under the grant, subject to the grant agreement. [European Commission IP Helpdesk](https://intellectual-property-helpdesk.ec.europa.eu/regional-helpdesks/european-ip-helpdesk/europe-frequently-asked-questions_en)

The Delaware parent should:

- Remain the venture-financing and ultimate exit entity.
- Retain background IP created before the Irish operation, unless a tax valuation later shows that a transfer is desirable.
- License that background IP to Ireland under a written agreement.
- Receive only arm’s-length royalties or service charges supported by functions, assets and risks. US §482 permits the IRS to adjust controlled-party prices that differ from results between comparable independent parties. [IRS transfer pricing](https://www.irs.gov/businesses/international-businesses/transfer-pricing)

Do not automatically assign grant-funded foreground IP back to Delaware. Transfers of Horizon results to a US entity may be subject to Commission notice, grant-agreement safeguards or objections, while the beneficiary must retain sufficient rights to exploit the funded innovation. [European Commission IP Helpdesk](https://intellectual-property-helpdesk.ec.europa.eu/regional-helpdesks/european-ip-helpdesk/europe-frequently-asked-questions_en)

### Why not a founder-owned standalone Irish company?

A parallel founder-owned EU company fragments the cap table, raises related-party and IP ownership questions, and forces a later acquirer to buy or merge two separately owned companies. It is appropriate only if the EU business is intentionally being built as an independent spin-out.

### Important EIC equity limitation

For blended-finance applications, the EIC Fund may invest at parent/holding-company level where that is where investors and economic upside reside, but the parent must itself be an SME established in an EU Member State or Associated Country. The Delaware parent does not satisfy that condition. [EIC 2026 Work Programme](https://eic.ec.europa.eu/system/files/2025-11/EIC-Work-Programme-2026.pdf)

The EIC could potentially invest directly in the Irish operating subsidiary, but that would put EIC equity below the existing Delaware cap table and can complicate follow-on financing and exit economics. This is why **grant-only is the cleaner initial route**.

For 2026 Accelerator Open, US parent ownership is not generally an automatic grant-only disqualifier for this AI-security product. The explicit foreign-control restriction in the Open call applies to grant-only projects falling within the critical-raw-materials Challenge, while AI investment support may be subject to economic-security safeguards. [EIC 2026 Work Programme](https://eic.ec.europa.eu/system/files/2025-11/EIC-Work-Programme-2026.pdf)

## 5. Portugal-resident-founder risk

Three separate Portuguese issues must be distinguished.

### A. Portuguese corporate residence

Portugal treats a legal entity as resident if its registered seat or place of effective management is in Portugal. [OECD Portugal residency summary](https://www.oecd.org/tax/automatic-exchange/crs-implementation-and-assistance/tax-residency/Portugal-Tax-Residency.pdf)

The Ireland–Portugal treaty assigns a dual-resident company to the state where its place of effective management is located. [Ireland–Portugal treaty](https://www.revenue.ie/en/tax-professionals/documents/double-taxation-treaties/p/portuguese-republic.pdf)

If the Lisbon founder alone makes strategic decisions, controls the bank, approves major contracts and directs the product, Portugal has a credible effective-management argument even if Irish board minutes are prepared afterward.

### B. Portuguese permanent establishment

A fixed place of business, place of management or office can constitute a Portuguese PE. A person habitually exercising authority to conclude contracts can also create a PE under the Ireland–Portugal treaty. [Ireland–Portugal treaty](https://www.revenue.ie/en/tax-professionals/documents/double-taxation-treaties/p/portuguese-republic.pdf)

The founder’s Lisbon home office, customer negotiations, service delivery and contract authority must therefore be reviewed. Possible outcomes include Portuguese employer/payroll registration, a Portuguese PE of the Irish company, or profit attribution to Portugal.

### C. Portuguese CFC

Portuguese CFC rules can apply to resident individuals or entities holding, directly or indirectly, at least 25% of a foreign entity where the foreign effective tax is below 50% of Portuguese tax and more than 25% of profits comprise specified passive or low-value related-party income. The 2026 benchmark is approximately 9.5%, based on Portugal’s 19% mainland rate. [Chambers Portugal 2026](https://practiceguides.chambers.com/practice-guides/corporate-tax-2026/portugal), [PwC Portugal 2026 rate](https://taxsummaries.pwc.com/portugal/corporate/taxes-on-corporate-income)

An EU/EEA CFC exemption is available where the entity exists for valid economic reasons and conducts genuine business using its own staff, equipment, assets and premises. [Chambers Portugal 2026](https://practiceguides.chambers.com/practice-guides/corporate-tax-2026/portugal)

Consequences for the shortlist:

- Ireland, Netherlands, Cyprus, Finland, Luxembourg and standard-rate Lithuania are normally above the headline 9.5% threshold.
- Estonia may have zero current corporate tax on retained profit.
- Malta’s 5% refund outcome and Lithuania’s 0%/7% small-company regime may fall below the threshold.
- IP royalties and low-value intercompany invoicing are specifically sensitive CFC income categories. [Chambers Portugal 2026](https://practiceguides.chambers.com/practice-guides/corporate-tax-2026/portugal)

Whether indirect ownership through the Delaware parent is attributed to the founder, and how foreign effective tax is calculated after losses, credits or shareholder refunds, requires a written Portuguese opinion.

## 6. US tax effect of a Delaware-owned EU subsidiary

A 100%-owned Irish or other EU corporation will normally be a US controlled foreign corporation. The Delaware parent will have Form 5471 reporting and potentially Subpart F/NCTI inclusions; the IRS defines a CFC generally as a foreign corporation more than 50% owned by qualifying US shareholders. [IRS Form 5471 instructions](https://www.irs.gov/instructions/i5471)

From 2026, the US corporate NCTI rate is 12.6%, with a 90% deemed-paid foreign-tax credit and an approximate 14% foreign-tax crossover rate, subject to FTC baskets, expense allocation and other limitations. [PwC US international-tax analysis](https://www.pwc.com/us/en/tax-services/publications/insights/assets/pwc-international-insights-from-the-one-big-beautiful-bill-act.pdf)

At a simplified headline level:

- Ireland at 12.5% could leave approximately **1.35 percentage points** of residual US NCTI tax: 12.6% − 90% × 12.5%.
- Estonia at 0% on retained profit could face up to the 12.6% current US NCTI rate, eliminating the intended Estonian deferral.
- Lithuania at 7% could leave approximately 6.3 percentage points of residual US tax.
- Malta at an effective 5% could leave approximately 8.1 percentage points.
- Netherlands, Cyprus, Finland, Luxembourg and Lithuania at 17% should generally exceed the approximate 14% crossover rate.

Those calculations are directional, not tax-return calculations; an international US tax adviser must model tested income, losses, credits, §78 gross-up, transfer pricing and the Irish grant’s local tax treatment.

## 7. Concrete implementation plan for Ireland

1. **Obtain a three-country structure memorandum before incorporation.** It should cover Irish residence and trading classification, Portuguese effective management/PE/CFC/payroll, and US CFC/NCTI/Form 5471/§482. This requires Irish, Portuguese and US tax advisers.

2. **Have the Delaware board approve a wholly owned Irish Ltd.** Use a normal private company limited by shares. A one-director LTD is permitted, but it must appoint a separate secretary. [PwC formation guide](https://www.pwc.ie/publications/2024/guide-to-company-formation-ireland.pdf)

3. **Appoint governance that has substance.** The Portugal-resident founder can satisfy the EEA-director rule, but add a genuinely independent Irish-resident director or executive with real authority if economically practical. Hold substantive strategy, financing, IP and major-contract decisions through the Irish board; a nominee who rubber-stamps Lisbon decisions is not sufficient.

4. **Establish an Irish operational footprint.** Obtain the registered office and real activity address, place Irish/EU personnel or contractors under the Irish entity, and give the subsidiary its own project budget, records, bank controls and customer/vendor contracts. There is no published EIC numeric substance threshold, so build evidence around real functions rather than a formal minimum.

5. **Open an EUR account and complete compliance registrations.** Register beneficial ownership, corporation tax, VAT and payroll where applicable. Then register the company in the EU Participant Register, obtain its PIC, complete the SME assessment including the Delaware parent, and appoint the LEAR. A validated bank account requires company bank evidence during grant preparation. [Funding & Tenders Participant Register](https://ec.europa.eu/info/funding-tenders/opportunities/portal/screen/how-to-participate/participant-register), [Funding & Tenders Online Manual](https://ec.europa.eu/info/funding-tenders/opportunities/docs/2021-2027/common/guidance/om_en.pdf)

6. **Execute intercompany agreements before grant work starts.** At minimum: background-IP licence, R&D/services agreement, EU commercial/distribution allocation, data/security responsibilities and transfer-pricing policy. Clearly identify pre-existing Delaware background and Irish grant-funded foreground.

7. **Build the EIC evidence package.** Show that the Irish company controls the project, has completed TRL 5, possesses adequate IP/exploitation rights, employs or contracts the delivery team, pays project costs and has the remaining 30% co-financing plus resources needed for later commercialisation. [EIC applicant guide](https://eic.ec.europa.eu/document/download/9d96fbf3-4d85-4ad0-9483-c77ce348111d_en?filename=EIC+Accelerator+guide+for+applicants_WP26.pdf)

8. **Target a realistic EIC batch.** The remaining published 2026 Accelerator batches are 2 September and 4 November 2026; unless the technical short proposal is already complete, 4 November is the credible target. [EIC funding opportunities](https://eic.ec.europa.eu/funding-opportunities_en)

### Rough Ireland cost and time

- State incorporation fee: **€50**. [CRO](https://cro.ie/Publications/Fees/Company/)
- Incorporation processing: **five working days through the approved fast scheme or up to ten working days through ordinary online A1**, assuming correct documents. [CRO](https://cro.ie/registration/company/registration-methods/)
- A current fully supported remote EEA-founder package including incorporation, registered office, secretary, tax registration and beneficial-ownership filing is advertised at approximately **€2,000 plus VAT**, with three-to-five-day formation after documents are ready. This is a commercial quote, not an official tariff. [Irish licensed formation provider](https://registercompany.ie/product/allinclusive-package/)
- Typical recurring components are roughly **€199–€400 for registered office, €99–€500 for company-secretary service and €1,000–€2,500 for accounting**, before real premises, personnel, tax opinions and transfer-pricing work. These are market estimates rather than regulated prices. [2026 Ireland cost survey](https://corpsec.io/jurisdiction/ireland/guides/cost)
- The company can therefore legally exist in approximately one to two weeks, but becoming an EIC-ready, tax-defensible operating subsidiary takes longer because banking, contracts, governance and genuine operations must follow.

## Final recommendation

Choose **Ireland**, owned 100% by the Delaware C-corp, and use it as a real EU operating/R&D subsidiary rather than a grant wrapper.

Choose the **Netherlands** instead if the company is willing to accept materially higher tax and operating cost in exchange for a stronger built-in physical-substance story.

Choose **Estonia** only if actual Estonian or distributed EEA operations will exist. A Lisbon-managed e-Residency company is administratively elegant but tax-fragile: Estonia itself warns of foreign PE taxation, Portuguese CFC/effective-management risk remains, and US NCTI removes much of the retained-profit benefit. [Estonian Tax Board](https://www.emta.ee/en/business-client/registration-business/non-residents-e-residents/tax-liabilities-companies)

The items requiring formal local advice are: Portuguese corporate residence, PE, CFC and payroll; Irish tax residence, grant taxation, trading-income classification and withholding; US Form 5471/NCTI/FTC and IP-transfer rules; the transfer-pricing method; and EIC-specific IP, foreign-control and change-of-control provisions.
