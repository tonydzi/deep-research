# DR26-09-14-ZB-09-2008 — raw report from claudecli

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Note before I start: my model training cutoff is January 2026. Everything dated after that — i.e. most of the specific 2026 figures below — comes from live web search run just now, not from memory. That matters forensically: the private-company numbers are almost all **un-audited secondary reporting**, and I will mark them as such. The only genuinely audited numbers in this whole exercise are the calibration anchors (Microsoft, Alphabet) and one litigation-adjacent filing (SpaceX's S-1, which consolidates xAI). Treat the private-lab precision as illusory.

---

# FORENSIC INVESTIGATION: ARE FRONTIER-LAB REVENUES ARITHMETICALLY POSSIBLE?

## PREAMBLE — the meta-finding that governs every case file

Before any single company: the **provenance structure** of this entire dataset is the story. I traced the sourcing chain on every headline number I could, and the pattern is consistent and damning for anyone who wants precision:

- **~4 primary origins carry the entire narrative.** (1) *The Information* (subscriber-shared documents, run-rate leaks, burn figures); (2) *Bloomberg* (circular-deal mapping, the $40B OpenAI run-rate); (3) *Ramp* corporate-card spend data (customer concentration); (4) *Menlo Ventures* enterprise-adoption survey (market share). Almost every other number is downstream of these.
- **A dense layer of citation-laundering sits on top.** Domains like getpanto.ai, valueaddvc.com, aibusinessweekly.net, axis-intelligence.com, useluminix.com, sacra.com, etc. re-publish the same 4 origins with added false precision and no new evidence. Two of them "agreeing" confirms nothing — I checked, and they share provenance. **I discount them to zero as independent corroboration.** [MEASURED: I opened the search result sets; the same figures recur verbatim across domains.]
- **The labs are private.** None of OpenAI, Anthropic, xAI, Mistral, Perplexity or Cohere files audited financials. Every "$X billion ARR" is either (a) self-reported by the company to press/investors, or (b) leaked from an investor deck. **None is GAAP revenue verified by an auditor.** The word "revenue" in these headlines is doing work it has not earned.

That is the null result you should hold onto: **for the six labs, the top-down numbers cannot be independently confirmed at all**, and the bottom-up reconstructions below are the *only* discipline available. Where they reconcile, I say so. Where they don't, I say that louder.

---

## CASE FILE 1 — OpenAI

### A. Revenue decomposition
Headline: **run-rate "$40B+" as of Aug 2026** [ASSERTED → Bloomberg, 2026-08-13, "OpenAI's Revenue Run Rate Tops $40 Billion Ahead of IPO"], up from ~$25B ARR (Feb–Mar 2026) and ~$10B (Jun 2025). A leaked August breakdown circulated as: ~$22B ChatGPT subscriptions, ~$12B API, ~$6B Sora/ads/licensing [ASSERTED, single leaked breakdown, un-named deck]. The company also claims **enterprise now >40% of revenue, crossing consumer parity** [ASSERTED, company statement].

Critical definitional flag: **"$40B" is an annualized run-rate, not a fiscal-year revenue.** The audited-adjacent number that exists is *actual* H1 2025 revenue ≈ **$4.3B** [ASSERTED → Reuters relaying The Information] and Q1 2026 revenue **$5.7B with $3.7B cash burn** [ASSERTED → The Information, "OpenAI Burned $3.7 Billion in First Three Months of 2026," shared shareholder documents]. Q1 2026 × 4 ≈ $23B — i.e. the run-rate more than doubling to $40B in five months requires accepting a July "+20% MoM" spike [ASSERTED → Bloomberg] annualized off a single strong month. **That is textbook peak-month annualization.**

### B. Bottom-up subscription model
Inputs: WAU **900M** (Mar 2026) [ASSERTED]; paying consumer subs **"50M+"** (OpenAI COO) [ASSERTED, no date-stamped audit]; tier split — the widely-repeated "5.8% Pro" figure is **5.8% of consumer *revenue*, Jan 2025**, not 5.8% of users [MEASURED: this is the actual claim; the blogs mis-derived "2.9M Pro users"].

Correcting that error is the whole ballgame. If Pro ($200) is 5.8% of revenue and Plus is $20:
- Solve for Pro user-share x: 200x / (200x + 20(1−x)) = 0.058 → **x ≈ 0.6% of paying users.** Pro is a *sliver*, exactly as H1 predicts.
- Blended monthly ARPU ≈ 0.994×$20 + 0.006×$200 = **~$21/mo**.

| Scenario | Paying subs | Blended ARPU/mo | Implied annual consumer sub revenue |
|---|---|---|---|
| Bear | 40M | $19 | **$9.1B** |
| Base | 50M | $21 | **$12.6B** |
| Bull | 55M | $24 (richer Team/Business mix) | **$15.8B** |

**Reconciliation:** pure Plus/Pro consumer reconstructs to **~$12–13B**, *not* the "~$22B ChatGPT subscriptions" leak. The $22B is only reachable if it bundles ChatGPT Team/Business/Enterprise *seats* into "subscriptions." So the leak isn't necessarily false — but it is **mislabeled**: it is not consumer freemium conversion, it is seat-based B2B wearing a consumer label. **H1 is directionally vindicated on the consumer line.**
Most sensitive assumption: **the paid-subscriber count (50M).** Everything keys off it, and it is a single un-audited executive assertion.

### C. Bottom-up enterprise model
Claimed enterprise ≈ >40% of $40B ≈ **$16B+** [ASSERTED]. Also "9M+ paying business users" [ASSERTED]. At ChatGPT Enterprise ~$60/seat/mo blended, 9M seats × $60 × 12 = **$6.5B** — so seat revenue alone doesn't reach $16B; the balance must be **API consumption** (developers, not seats). Cross-check: Ramp/Menlo data shows OpenAI + Anthropic together, and OpenAI's API is real and large. Observable named enterprise logos exist in abundance (this is OpenAI's genuine strength). **The gap here is smaller than for peers** — but note the concentration finding below.

### D. Definitional forensics
- "$40B" = run-rate off a peak month. **Flag.**
- "$22B subscriptions" = almost certainly bundles B2B seats. **Mislabel flag.**
- "$100M ads ARR in under six weeks" [ASSERTED → repeated] = six-week annualization, the most aggressive possible. **Flag.**

### E. Related-party / circular-flow
This is OpenAI's defining forensic risk. Mapped circular deals [ASSERTED → Bloomberg, "AI Circular Deals," 2026]: **Nvidia up to $100B into OpenAI ↔ OpenAI buys Nvidia chips; OpenAI–Oracle ~$300B; OpenAI–AMD (chips for up to 10% AMD stake, ~$300B purchase commitment).** Over **$800B** of intertwined vendor-financing across the ecosystem. None of this is *revenue* for OpenAI directly, but it means OpenAI's demand is partly **funded by its own suppliers**, and Microsoft is simultaneously OpenAI's largest investor and, via GitHub, a large *counterparty in Anthropic's* revenue. The revenue line is real API usage; the *sustainability* of the counterparties' spend is the circular question.

### F. Cash vs revenue
Q1 2026: **$5.7B revenue, $3.7B burn (65% of revenue)** [The Information]. Cash $73B end-Q1 (from $40B) — i.e. the burn is covered by *financing, not operations*. OpenAI's own forecast: **~$14B loss in 2026** [ASSERTED → Yahoo/report]. Gross margin after inference: undisclosed; the fact that they raise every few months at rising valuations while claiming hypergrowth is **exactly the testable pattern in the brief — and it tests positive.** Revenue growth is real; it is also *necessary* to service the compute commitments, which is a different thing from *profitable*.

### G. Falsification list
1. *Audited GAAP revenue showing $40B is real, not run-rate* — **looked; does not exist** (private). 
2. *Consumer subs materially above 55M at >$25 ARPU* — **looked; only 50M asserted.** Would raise my subscription plausibility. 
3. *Pro tier being a large user share* — **looked; it's ~0.6%.** Confirms H1. 
4. *Enterprise revenue net of Microsoft/related-party* — **not disclosed.** 
5. *A single audited quarter contradicting the $3.7B burn* — **none found; the burn number is the most credible figure OpenAI has, because it's the one that hurts.**

**Verdict OpenAI: revenue is largely REAL but the labels are inflated (run-rate-as-revenue, seats-as-subscriptions, peak-month annualization). Plausibility 4/5.**

---

## CASE FILE 2 — Anthropic

### A. Revenue decomposition
Headline run-rate escalated absurdly fast in the reporting: ~$5B (Aug 2025) → "$14B" (Feb 2026) → "$30B" (Apr) → "$47B" (May) → **"$65B" (Jul 2026)** [all ASSERTED, secondary; the $65B from valueaddvc-tier sources — **low confidence, likely laundered**]. The most *load-bearing, best-sourced* figure is **$5B run-rate with ~80% from API/enterprise** [The Information; Menlo Ventures]. I trust the $5B-era decomposition far more than the $65B headline.

### B. Bottom-up subscription model
Anthropic is **~80% API/enterprise, ~20% consumer** [ASSERTED → repeated, Menlo]. Consumer (Claude.ai Pro/Max at $20/$100–200) is therefore *not* the story and cannot carry the number — which is *consistent* with H1, just via a different route: Anthropic doesn't pretend consumer carries it. Consumer ≈ 20% × $5B ≈ $1B, plausible at a few million Pro subs. **No inflation needed here; the model is honest about being enterprise-led.**

### C. Bottom-up enterprise model — THE KEY FINDING
[MEASURED-adjacent, best evidence in the entire report]: **Cursor + GitHub Copilot alone ≈ $1.2B of the ~$4–5B total — "nearly a quarter" from two customers** [The Information, relayed VentureBeat, 2026]. And **top 1% of customers = ~80% of enterprise revenue** [Ramp research]. Claude Code itself ≈ $400M annualized [The Information].

This is **H2 confirmed in its strongest form.** The enterprise line is real revenue but **radically concentrated**: you do *not* need "hundreds of seven-figure logos" because a handful of AI-coding customers (themselves VC-funded, themselves burning cash, one of them — GitHub — owned by Microsoft, which funds OpenAI) do the work. The forensic risk isn't fabrication; it's **fragility**: if Cursor's economics break or Copilot re-routes to an in-house/OpenAI model, ~25% of revenue moves. Ex-those-two, the rest grew "11-fold YoY" [The Information] — genuinely fast, but off a small base.

### D. Definitional forensics
The $5B→$65B ramp in ~11 months is the reddest flag in the sector. $5B and the concentration data are well-sourced. **$30B/$47B/$65B are single-report or laundered and I mark them LOW confidence.** The pattern "run-rate re-quoted upward monthly" is precisely the peak-annualization risk.

### E. Related-party / circular-flow
Less capex-circular than OpenAI, but note: **a quarter of revenue flows through two apps, one (GitHub) owned by Microsoft** [theclarity.us, 2026-09-06, relaying the same data]. Amazon and Google are both large *investors* and *cloud providers* — compute-for-equity dynamics apply (Anthropic's costs and one funding source are the same entity).

### F. Cash vs revenue
Anthropic claims approaching profitability / "first profit" in some 2026 reports [ASSERTED, LOW confidence]. Given 7× Claude-Opus token pricing vs GPT-5 [VentureBeat] and enterprise mix, gross margins *could* be better than OpenAI's — but a **pricing war explicitly threatens margins** [VentureBeat headline]. Undisclosed and unverifiable.

### G. Falsification list
1. *Concentration is NOT real (broad customer base)* — **looked; Ramp + The Information both say 80%/top-1%. Concentration confirmed.** 
2. *$65B run-rate has a primary source* — **looked; could not find one that isn't laundered.** 
3. *Consumer is secretly large* — **no; ~20%.** 
4. *Cursor/Copilot $1.2B is double-counted* — **checked; reported as direct API spend, not double-counted.** 
5. *Margins are positive after inference* — **not disclosed.**

**Verdict Anthropic: the $5B-era number is REAL but H2-concentrated to an extreme; the $30–65B headlines are UNVERIFIED and likely run-rate inflation. Plausibility of the *headline* 2/5; plausibility of *"real enterprise revenue exists, just concentrated"* 4/5.**

---

## CASE FILE 3 — xAI

### A/D. Revenue decomposition & definitional
Grok/xAI AI revenue ≈ **$500M annualized (mid-2026)**, up from ~$350M (2025) [ASSERTED → Sacra, secondary]. Consolidated with X's ad/subscription revenue, "$3.3B+ annualized by year-end" [ASSERTED] — but that **bundles X (Twitter) advertising into "AI revenue,"** which is a category error. The AI-attributable line is the ~$500M.

### B. Subscription model
SuperGrok ~$30/mo, SuperGrok Heavy ~$300/mo [ASSERTED]. Even generously: to reach $500M *from subscriptions* you'd need ~1.4M SuperGrok-equivalent subs. Plausible only if distribution via X inflates conversion. The high tier ($300) is again a sliver. **H1 applies.**

### C/E/F. The forensic core — this one has an audited-adjacent number
**xAI burned $6.4B in a year; ~$1B/month burn against ~$500M ARR** [MEASURED-adjacent → SpaceX IPO/S-1 filing surfaced via Yahoo Finance/Bloomberg, 2026 — *this is the closest thing to an audited disclosure in the whole sector because it rides in SpaceX's filing*]. Net loss $1.46B in the Sept-2025 quarter alone. **On Feb 2, 2026, xAI merged into SpaceX (all-stock, ~$250B valuation)** [ASSERTED → repeated] — which is *itself* a forensic event: an unprofitable AI lab absorbed into a related private entity controlled by the same person, moving its financials off any standalone scrutiny. Prior: $20B Series E (Jan 2026, $230B valuation); ~$42B raised cumulatively.

Burn ÷ revenue ≈ **24×.** This is the worst revenue-quality profile of the six: the smallest verified revenue, the largest verified burn, and a related-party merger that removes standalone disclosure.

### G. Falsification list
1. *AI revenue is >$1B standalone* — **no evidence; ~$500M.** 
2. *Burn is overstated* — **the S-1 says $6.4B; it's the credible number.** 
3. *X ad revenue is legitimately AI* — **no; it's legacy Twitter.**

**Verdict xAI: AI revenue REAL but tiny (~$500M) and dwarfed by burn; "$3.3B" conflates X ads. Plausibility of the *AI* number 3/5; plausibility of the *conflated* number 1/5. H1 + cash-burn story strongly supported.**

---

## CASE FILE 4 — Mistral AI

### A/B/C/D
ARR trajectory: **€300M (Sep 2025) → ~$400M (Jan 2026)**, targeting **>€1B revenue by end-2026** [ASSERTED → Maddyness / Davos CEO statement; Sacra]. Revenue = enterprise licensing + Le Chat Enterprise + consumer Chat; **~60% from Europe** [ASSERTED]. Funding: €1.7B Series C (Sep 2025, ASML-led, €11.7B post) → **$3.5B at $20B (Jun 2026)** [ASSERTED].

Forensic read: the €300M→€1B *target* is a **projection, not a run-rate** — correctly labeled as a target by the CEO, which is more honest than peers. The €300M ARR is small enough to be plausibly enterprise-license-driven without needing a fantastical consumer curve. Biggest unverifiable: the **split between binding enterprise contracts and pipeline** — European licensing deals (sovereign-AI, ASML as strategic investor-customer) raise the **investor-customer** flag (ASML is both lead investor and a potential customer).

### E/F/G
Related-party: ASML investor-customer overlap [flag, unquantified]. Cash: undisclosed; €1.7B + $3.5B raised against ~$400M ARR implies heavy burn but far less extreme than xAI. Falsification: I looked for audited French filings (Mistral SAS accounts may eventually appear in French registres) — **not found in this run**; that is where the truth physically lives.

**Verdict Mistral: small, plausibly-real enterprise ARR, honestly labeled as a target. Plausibility 4/5. Least inflated of the group, mostly because the numbers are small and the CEO says "target."**

---

## CASE FILE 5 — Perplexity

### A/B/C/D
ARR **~$100M (early 2025) → >$450M (Mar 2026)**, targeting $656M by year-end [ASSERTED → repeated, FT relayed for the 100M MAU figure]. **100M+ MAU** (FT, Apr 2026, *with agents included* — a definitional softener) [ASSERTED]. Paid users: **not disclosed; ~1M estimated** [ESTIMATED → Resourcera calculation, weak].

The forensic tell: **"$450M ARR — a 50% jump in a single month" driven by a Feb-25 product launch** [ASSERTED]. That is peak-month annualization stated openly. At ~1M paid users × ~$20/mo × 12 = **$240M** — so the reconstructed subscription line is **roughly half** the claimed $450M ARR, meaning the balance is **usage-based/agent/API** revenue annualized off a spike. **H1 applies to the subscription half; the other half is annualized new-product revenue that has not proven durable.**

### E/F/G
Perplexity is a **reseller of others' models** — its COGS is API payments to OpenAI/Anthropic/Google, so gross margin is structurally thin and its "revenue" partly round-trips to competitors. Raised repeatedly at soaring valuations ($23B/$20B range, 2026) against <$500M ARR — the raise-often-while-claiming-growth pattern **tests positive**. Falsification: *a disclosed paid-subscriber count* would settle it — **does not exist.**

**Verdict Perplexity: real but small (~$240M reconstructable, $450M claimed via annualized spike); thin-margin reseller. Plausibility 3/5. H1 supported.**

---

## CASE FILE 6 — Cohere

### A/B/C/D
**$240M ARR (2025), +287% YoY from $62M (end-2024)**, beating a $200M internal target [ASSERTED → CNBC "Cohere tops revenue target," 2026-02-13, investor memo — *relatively well-sourced*]. **~85% from private deployments**; named customers Oracle, Fujitsu, RBC, LG, Notion on multi-year contracts [ASSERTED]. But note the tension: **SeekingAlpha (2025) reported annualized revenue at just ~$70M** at one point [ASSERTED → SeekingAlpha], vs the $240M year-end. That's a wide intra-year spread — consistent with a small number of large contracts landing lumpily (classic enterprise), *and* consistent with run-rate volatility.

### C — enterprise reconstruction
$240M with ~85% private-deployment across named logos like Oracle/RBC/Fujitsu implies **a handful of seven-/eight-figure deals**, not hundreds. Verifiable named customers *exist and are nameable* — better than most peers — but the count is **small**, so concentration risk is high (H2-style, but disclosed).

### E/F/G
**Nvidia-backed** [ASSERTED] — investor-supplier overlap. Valuation $7B (Sep 2025) against $240M ARR = ~29× ARR. Falsification: *contract-level ACV disclosure* would settle concentration — not public. The $70M vs $240M discrepancy is the thing I'd most want reconciled.

**Verdict Cohere: smallest and arguably cleanest — nameable enterprise logos, a CNBC-sourced beat, honest "private deployment" framing. Plausibility 3.5/5. H2-concentration present but disclosed, not hidden.**

---

## CALIBRATION ANCHORS (audited — these bound what's plausible)

**Microsoft** [MEASURED → SEC filings / IR]: AI run-rate **$13B (disclosed FY25 Q2, Jan 2025), then the metric was withdrawn from disclosure** — itself a tell; reportedly ~$37B run-rate by FY26 Q3 [ASSERTED, no longer formally disclosed]. **M365 Copilot >30M paid seats (FY26 Q4)** [MEASURED-adjacent, IR]. Capex guided to **$190B+** — the demand that funds the labs' revenue is the same hyperscaler capex cycle.

**Alphabet** [MEASURED → SEC 8-K, Q2 2026, googexhibit991q22026.htm]: total revenue **$119.8B**, **Google Cloud $24.8B (+82%)**, **cloud backlog $514B (RPO), ~50% to convert within 24 months**, cloud operating margin 35.6%. **Gemini app 950M active users.** This is what audited AI-adjacent scale looks like: RPO is disclosed *as backlog, not as revenue* — exactly the discipline the private labs lack. Google booking $514B of backlog and still only recognizing ~half within 2 years shows how *slowly* even real enterprise commitments convert to revenue — a direct rebuke to private "run-rate = revenue" framing.

**SaaS comparable (Salesforce)** [MEASURED → SEC filings]: took **~10 years to reach $1B** revenue; "fastest ever to $5B" was still ~15 years in. The historical enterprise-software cohort of seven-figure logos accumulates over **years**, not months. When a private lab claims to add tens of $B of "enterprise ARR" in under a year, it is either (a) a genuinely unprecedented curve, or (b) not measuring GAAP enterprise revenue. The anchor says: **be very suspicious of (a).**

---

## DELIVERABLE 1 — ONE-PAGE VERDICT (plausibility 1–5, bottom-up)

| Company | Score | Strongest FOR | Strongest AGAINST |
|---|---|---|---|
| **OpenAI** | **4/5** | Q1'26 $5.7B rev / $3.7B burn shared to shareholders (the painful number is credible); real API + 50M subs | "$40B" is peak-month run-rate; "$22B subscriptions" bundles B2B seats |
| **Anthropic** | **2/5 headline, 4/5 core** | Cursor+Copilot $1.2B and 80%/top-1% concentration are Ramp+TI-sourced (real revenue) | $30B→$65B ramp has no primary source; extreme 2-customer fragility |
| **xAI** | **3/5 AI-line, 1/5 headline** | $6.4B burn rides SpaceX's S-1 (near-audited) | ~$500M AI ARR vs ~$1B/mo burn; "$3.3B" conflates X ads; merged out of view |
| **Mistral** | **4/5** | CEO labels €1B as a *target*, not run-rate; €300M ARR small & enterprise-plausible | ASML investor-customer overlap; no audited French accounts found |
| **Perplexity** | **3/5** | $100M→$450M growth is real directionally | $450M = 50%-in-one-month annualized spike; thin-margin reseller; ~$240M reconstructable |
| **Cohere** | **3.5/5** | CNBC investor-memo $240M beat; nameable logos (Oracle/RBC/Fujitsu) | $70M vs $240M intra-year spread; tiny logo count = concentration |

---

## DELIVERABLE 2 — COMPARISON TABLE

| Company | Claimed (date, type) | Bottom-up reconstructed range | Gap | Dominant real source | Biggest unverifiable component |
|---|---|---|---|---|---|
| OpenAI | $40B run-rate (Aug'26) | Consumer $9–16B + API/ent. $16–24B ≈ **$25–40B** | Consumer line ~$9B *below* the "$22B subs" label | ChatGPT subs + API | Enterprise net of Microsoft/related-party; peak-month portion |
| Anthropic | $5B (well-sourced) → $65B (laundered) | **$4–6B** defensible | $30–65B **unverified** | Enterprise API (coding) | Any run-rate above ~$6B; margin after inference |
| xAI | $500M AI / $3.3B consolidated | **~$500M** AI | $2.8B = X ads, not AI | Grok subs + X (legacy) | AI-vs-legacy split post-SpaceX merger |
| Mistral | €1B *target* / €300–400M ARR | **~$400M** | Target ≠ current (honest) | Enterprise licensing (EU) | Binding contracts vs pipeline; ASML overlap |
| Perplexity | $450M ARR (Mar'26) | **~$240M** subs + agent spike | ~$210M is annualized new-product | Consumer subs | Paid-sub count (undisclosed); reseller margin |
| Cohere | $240M ARR (2025) | **$70–240M** (lumpy) | Intra-year $170M swing | Private enterprise deployments | ACV per contract; logo count |
| *MSFT (anchor)* | *$13B→$37B (audited then withdrawn)* | *n/a — audited* | *—* | *Copilot seats + Azure AI* | *Metric withdrawal itself* |
| *Alphabet (anchor)* | *Cloud $24.8B/qtr, $514B RPO (audited)* | *n/a — audited* | *—* | *Cloud + Gemini* | *— (properly disclosed)* |

---

## DELIVERABLE 3 — THE THREE NUMBERS THAT WOULD SETTLE IT, AND WHERE THEY PHYSICALLY LIVE

1. **OpenAI's audited GAAP full-year revenue and its consumer-vs-enterprise-vs-API split, net of Microsoft-related-party transactions.** Lives in: the **S-1 registration statement** OpenAI must file with the **SEC** ahead of the widely-reported IPO. This single document collapses the run-rate/seats/subscriptions ambiguity instantly. Until it's filed, everything is a leak.
2. **Anthropic's revenue net of its top ~10 customers (especially Cursor and GitHub/Microsoft), with contract terms.** Lives in: **Anthropic's investor data room / Ramp's underlying anonymized card-spend panel**, and would surface in any **Anthropic S-1** or in **Microsoft's / a customer's own audited segment disclosure**. The concentration is the whole risk; the de-concentrated number is the truth.
3. **xAI's standalone AI-only revenue and burn, separated from X advertising, post-SpaceX merger.** Lives in: the **SpaceX S-1 / audited consolidated financials** (SpaceX is heading toward disclosure; xAI now consolidates into it). This is the *only* one of the three that partially exists already — the $6.4B burn came from exactly this filing — which is why xAI's cash story is the best-evidenced and worst-looking.

Secondary settling numbers, by physical location: **French registre du commerce** (Mistral SAS statutory accounts); **USASpending.gov / SAM.gov / FPDS** and **UK Contracts Finder / TED** (government seats for OpenAI/Anthropic — the audited *customer* side); **Microsoft & Alphabet 10-Ks** (their disclosed AI run-rates bound the whole market from above).

---

## DELIVERABLE 4 — WHICH OF H1 / H2 / H0 THE EVIDENCE SUPPORTS

**Per company:**
- **OpenAI** → **H0-leaning** (revenue broadly real) **but with H1 label-inflation**: subscriptions *are* a big real line, yet "$22B subscriptions" smuggles B2B seats and "$40B" is run-rate. The high tier (Pro) is genuinely a ~0.6% sliver — H1's specific claim is *correct*. Confidence: **Medium-High.**
- **Anthropic** → **H2 confirmed, strong form**: real enterprise revenue, but ~25% from two customers and 80% from the top 1%. Not fabricated, but exactly the "small number of very large contracts doing the work" thesis. The $30–65B headlines are **unverified**. Confidence: **High** on concentration, **High** that the top-end headline is inflated.
- **xAI** → **H1 + cash-burn thesis**: tiny AI subscription revenue, gigantic burn, and "$3.3B" is achieved only by conflating legacy X advertising. Confidence: **High.**
- **Mistral** → **H0** (the cleanest): small, plausibly-real, honestly labeled as a *target*. No inflation detected. Confidence: **Medium** (small numbers, thin disclosure).
- **Perplexity** → **H1** (subscription half real, other half annualized spike) **+ thin-margin reseller** risk. Confidence: **Medium-High.**
- **Cohere** → **H2-mild but disclosed**: real, nameable enterprise deals, concentrated in few logos, honestly framed. Confidence: **Medium.**

**Overall verdict (blunt):**
The evidence supports **a blend of H0 and H2, with H1 vindicated on the specific consumer-arithmetic claims.** Namely:
- **H1 (subscriptions can't carry it) — SUPPORTED as stated.** The $100–250 tiers really are sub-1% slivers (OpenAI Pro ~0.6%; SuperGrok Heavy, Claude Max, Perplexity Max all tiny). Blended consumer ARPU sits near **$20–21**, and pure consumer freemium reconstructs to a fraction of headline "subscription" lines. Where labs *label* seat-based B2B as "subscriptions," they mask this. **Confidence: High.**
- **H2 (enterprise inflated by concentration & definition) — SUPPORTED, strongest at Anthropic.** The enterprise revenue is *real* but produced by **a handful of very large, often investor-/supplier-adjacent accounts**, not the "hundreds of seven-figure logos" a mature-SaaS lens expects — and the Salesforce/Alphabet anchors show that cohort takes *years*, not months, to build. Plus pervasive **run-rate-as-revenue** and **peak-month/peak-week annualization**. **Confidence: High.**
- **H0 (numbers broadly real) — PARTIALLY SUPPORTED, and this is the honest steelman:** the *underlying usage* is genuinely unprecedented. Alphabet's audited **$514B cloud RPO** and **950M Gemini users**, Microsoft's **30M+ Copilot seats**, OpenAI's **credible $5.7B/quarter with $3.7B burn** — these are real, audited-or-painful numbers showing demand that *no prior software cycle matched*. The labs are not Enron; they are not booking phantom revenue. **What they are doing is presenting run-rates, bookings-flavored ARR, and seat-bundles in the most flattering framing available for a private company that never has to reconcile to an auditor.** **Confidence: Medium-High.**

**The one-sentence forensic bottom line:** The *cash going in* is audited and enormous; the *revenue coming out* is real but **systematically over-labeled** — run-rate dressed as revenue, seats dressed as subscriptions, two customers dressed as a market, and peak months dressed as years — and because all six labs are private, **the single document that would end every argument (an SEC S-1) does not yet exist for any of them.** Until OpenAI's or Anthropic's S-1 lands, the correct analyst posture is: *believe the burn, discount the run-rate, and never call two blogs a corroboration.*

**Staleness caveat:** My training ends Jan 2026; all post-Jan-2026 figures above are from live search of an ecosystem dominated by ~4 primary sources and heavy citation-laundering. I have flagged every ASSERTED/ESTIMATED/MEASURED accordingly. The audited anchors (Alphabet 8-K, SpaceX S-1-derived burn, Microsoft IR) are the parts you can most trust; the private-lab run-rates are the parts you should trust least.

**Sources (opened this run):**
- [Bloomberg — OpenAI run-rate tops $40B](https://www.bloomberg.com/news/articles/2026-08-13/openai-s-revenue-run-rate-tops-40-billion-ahead-of-ipo)
- [The Information — OpenAI burned $3.7B in Q1 2026](https://www.theinformation.com/articles/openai-burned-3-7-billion-first-three-months-2026)
- [Reuters via TradingView — OpenAI H1 2025 revenue ~$4.3B](https://my.tradingview.com/news/reuters.com,2025:newsml_L3N3VH0A8:0-openai-s-first-half-revenue-rises-16-to-about-4-3-billion-the-information-reports)
- [VentureBeat — Anthropic revenue tied to two customers](https://venturebeat.com/ai/anthropic-revenue-tied-to-two-customers-as-ai-pricing-war-threatens-margins)
- [PYMNTS — OpenAI & Anthropic 80% revenue from 1% of customers](https://www.pymnts.com/news/artificial-intelligence/2026/openai-and-anthropic-get-80percent-of-revenue-from-1percent-of-customers/)
- [Bloomberg — AI circular deals graphic](https://www.bloomberg.com/graphics/2026-ai-circular-deals/)
- [Yahoo Finance — xAI burned $6.4B (SpaceX IPO filing)](https://finance.yahoo.com/sectors/technology/articles/xai-burned-6-4b-last-222608682.html)
- [StartupHub — xAI $500M ARR vs $1B monthly burn](https://www.startuphub.ai/ai-news/ai-figures/2026/figure-elon-musk-spacexai-financial-breakdown-2026-07-09)
- [Maddyness — Mistral on track for €1B by 2026](https://www.maddyness.com/uk/2026/01/23/mistral-ai-on-track-to-reach-one-billion-euros-in-revenue-by-2026/)
- [Sacra — Mistral revenue/valuation](https://sacra.com/c/mistral/)
- [FT-relayed / getpanto — Perplexity 100M MAU, $450M ARR](https://www.getpanto.ai/blog/perplexity-ai-statistics)
- [CNBC — Cohere tops revenue target ($240M), IPO momentum](https://www.cnbc.com/2026/02/13/ai-startup-cohere-revenue-ipo.html)
- [SeekingAlpha — Cohere annualized revenue ~$70M (earlier)](https://seekingalpha.com/news/4412490-cohere-annualized-revenue-hits-70m-as-it-focuses-on-enterprise-ai-report)
- [Alphabet Q2 2026 8-K (SEC)](https://www.sec.gov/Archives/edgar/data/0001652044/000165204426000066/googexhibit991q22026.htm)
- [Microsoft FY26 Q3 earnings (IR)](https://www.microsoft.com/en-us/investor/earnings/fy-2026-q3/productivity-and-business-processes-performance)
- [IntuitionLabs — ChatGPT Free/Plus $20/Pro $200 tiers](https://intuitionlabs.ai/articles/chatgpt-plans-comparison)
- [Salesforce SEC 8-K filings (revenue milestones)](https://www.sec.gov/Archives/edgar/data/0001108524/000110852415000011/crm-q1exhibit991.htm)
