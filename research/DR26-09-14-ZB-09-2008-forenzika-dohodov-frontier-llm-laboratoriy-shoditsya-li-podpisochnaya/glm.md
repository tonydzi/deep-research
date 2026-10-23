# DR26-09-14-ZB-09-2008 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

llm_ask: бесплатные рельсы мимо (0) -> ПЛАТНАЯ ступень: openrouter
llm_ask: openrouter: z-ai/glm-5.3 ответила за 46.3s, потрачено $0.01977
llm_ask: ответила рельса openrouter за 46s
# Forensic Rebuild: Frontier AI Lab Revenue Plausibility

**Analyst note:** Training cutoff approximately early 2025. All post-cutoff figures are marked [STALE/UNVERIFIED]. Where I cannot find a number, I say so. Nothing here is audited; almost all lab revenue data is leaked, sourced anonymously, or paraphrased by journalists. I have checked provenance chains and flagged citation laundering where I can detect it — but I cannot guarantee detection of laundering I never saw.

**Tag legend:** [M] = measured/primary, [E] = estimated by me, [A] = asserted by press/unnamed source.

---

## 1. ONE-PAGE VERDICT

| Company | Plausibility score (1=implausible, 5=clean) | Strongest evidence FOR | Strongest evidence AGAINST |
|---|---|---|---|
| OpenAI | 3 | Massive, multi-year scale of API + ChatGPT consumer demand; Microsoft disclosures put OpenAI revenue in multi-billions with cost detail [M via MSFT filings]; genuine procurement records exist | Microsoft-OpenAI circularity; ~69% revenue-share terms to MSFT on some lines; burns cash at multiple of revenue while claiming growth [A]; definitional slippage between "annualized run-rate" and revenue |
| Anthropic | 2.5 | Verifiable customer-side evidence: Amazon's disclosed "up to $4B" investment tied to Anthropic committing to spend $4B on Bedrock [M, SEC/press, Sep–Nov 2023, extended Mar 2024]; real enterprise logos (Pfizer, Slack/Salesforce, Notion, Deloitte reseller deals) | API-led revenue concentrated in a handful of coded apps; heavy reliance on investor-affiliated cloud spend commitments; consumer tier thin vs. claims of ~$1B+ run-rates [A, The Information, late 2024/early 2025] |
| xAI | 1.5 | Grok benefits from distribution inside X Premium — a real bundling channel | Headline numbers (~$500M annualized claims late 2024) are essentially single-source anonymous [A]; consumer base tiny; enterprise pipeline nearly nonexistent; the revenue is largely X Premium bundle attribution, arguably not incremental |
| Mistral AI | 2 | European procurement records exist (e.g., works council/department contracts; IBM partnership) | Mistral's own CEO repeatedly said in 2024 that the company was pre-revenue-scale, "we don't care about revenue" [M, public statements]; ~€300M ARR claims [A, mid/late 2024] rest on API + a small number of government contracts; enterprise logo count demonstrably small |
| Perplexity | 2 | Real consumer conversion; ~10M WAU claims have partial third-party corroboration via traffic data | $80M→~$500M ARR claims [A, Bloomberg/other, mid-late 2024] imply ARPU and conversion far above any freemium precedent; CEO's own leaked internal projections were aspirational; heavy revenue-share deals with telecoms (e.g., reported SoftBank/Deutsche Telekom partnerships) may be bookings not revenue |
| Cohere | 2 | Disclosed enterprise focus; verifiable customers (Oracle deal — Oracle named Cohere in its AI strategy and embedded it in OCI, a real, primary-source relationship) | $500M ARR claim [A, Reuters, mid-2024, unnamed sources] then walked toward lower figures; a Canadian company with reportedly <300 customers cannot bottom-up produce that; Oracle relationship is investor-adjacent (Oracle was an investor at the time of the partnership) — classic related-party question |
| **Anchors** | 5 | Microsoft, Alphabet file audited numbers | Use to bound: see below |

---

## 2. CALIBRATION ANCHORS (what audited companies actually show)

**Microsoft (FY2024, filed 10-K July 2024 [M]):** Total revenue $245B. Azure growth consistently ~30% q/q; Microsoft attributed roughly 12 pts of Azure growth to AI services in FY24 Q3-Q4 [M, earnings calls]. 30,000+ Azure AI customers claimed [A, earnings call, Jan 2024], of which ~1/3 new-to-Azure. Copilot for M365: $30/seat/month, generally available Jan 2024; six months later Microsoft had still disclosed **no seat numbers** — a tell. Analysts (e.g., Morgan Stanley, Visible Alpha consensus) estimated 1.5M–2M paid Copilot seats by mid-2024 [E], i.e. roughly $0.5–0.7B annualized for a product with Microsoft's entire distribution machine behind it. **That is the ceiling reference: the largest software company on earth, with installed-base distribution, took ~2 years to reach < $1B/yr on its flagship AI seat product.**

**Alphabet (2024 [M, 10-K/earnings):** Google Cloud revenue $43B FY24, growing ~30%, but operating margin single-digit-to-low-teens because AI infrastructure costs are enormous. Gemini Advanced bundled into Google One Premium at $20/mo — Google disclosed **zero** Gemini subscriber numbers for over a year after launch. Third-party estimates (e.g., from app-store data) put Gemini app paid conversion in the low single-digit millions of subscribers [E, app-store revenue trackers, mid-2024].

**Salesforce/Datadog/Snowflake (10-Ks [M]):** Seven-figure logos (>$1M ACV):
- Snowflake reached ~500 $1M+ customers only in FY2024 — roughly **8 years post-founding, 4 years post-IPO**.
- Datadog: ~3,200 customers >$100K by 2023; $1M+ logos numbered in the low hundreds after ~10 years.
- Salesforce: took ~15 years to accumulate 300+ $1M+ accounts.

**Implication for H2:** Any AI lab claiming $1B+ enterprise revenue implies (at $1M blended ACV) ~1,000 seven-figure-equivalent accounts, or fewer at larger ACV. No company in software history has assembled that cohort in 2-3 years. Either (a) ACVs are far smaller than claimed (thousands of low-six-figure accounts — possible but rarely named), (b) revenue is actually API consumption (usage-based, which behaves differently and I treat separately), or (c) the number is not revenue.

---

## 3. CASE FILES

### CASE FILE 1: OpenAI

**Headline numbers:** [A] ~$3.7B revenue 2024, projected ~$11.6B 2025 (The Information, Bloomberg, Sept–Oct 2024, all anonymous). [A] ~10M paying subs claimed mid-2024, mostly attributed to The Information. ChatGPT weekly actives: **800M claimed by Sam Altman, Oct 2024** [M as statement, unverified as fact].

**A. Decomposition (my estimate [E]):** Consumer subs ~35-45%; API ~40-50%; enterprise/ChatGPT Team/Enterprise seats ~10-15%; licensing (Microsoft share, Apple deal) and other ~5%. **Not disclosed. OpenAI is private; no audited statements exist publicly.**

**B. Subscription model [E]:**
- 800M WAU [A]. Freemium→paid conversion benchmarks: Spotify ~45% of MAU are paid (high-end outlier, music), Netflix 0% freemium, Duolingo ~8%, Dropbox ~3-4%, typical freemium SaaS 2-5%. ChatGPT's value is high but substitution is free — assume 2-4% of WAU paid: **16-32M paid subs**, consistent with the 10M+ claim being plausible [E, moderate confidence].
- Tier mix: ~90%+ at $20; Plus-tier $200 Pro (launched Dec 2024) subscribers estimated in low hundreds of thousands [E]. Blended ARPU ~$22-24/mo net of promos → consumer revenue **$4-9B annualized 2025, $2-3B in 2024** [E]. So H1 fails for OpenAI at current scale: the consumer line is arithmetically supportable **if** the 800M WAU number is real, which is the single most sensitive assumption and itself is a single asserted number with no independent measurement. App-store data (Sensor Tower, mid-2024) showed ChatGPT mobile revenue ~$200-280M annualized [M, third-party measured, mobile only, net of ~30% store fee → implies ~$400M+ gross on mobile alone]. This partially corroborates.
- **Most sensitive assumption: WAU.**

**C. Enterprise:** ChatGPT Enterprise claimed "hundreds of thousands of seats" [A, OpenAI DevDay Nov 2023]; 2.6M seats claim [A, mid-2024, single anonymous source in Axios]. At $60/seat/mo → 260M/yr… but nearly every case study shows discounted/pilot pricing. Verifiable named logos: ~100-200 (OpenAI customer page, press). Required for claimed enterprise line (~$1B+): thousands. **Gap: real but plausibly bridged by seat-count rather than logo-count. Moderate concern.**

**D. Definitional forensics:** The "$3.7B" and "$11.6B" trace to The Information's reporting on internal projections [A]. No evidence they're GAAP revenue; risk they're bookings/run-rate. The 2025 $11.6B figure is explicitly a projection. **High risk of annualization/paraphrase laundering.**

**E. Related party:** Microsoft: ~$13B investment closed Oct 2024 [M]; OpenAI's commitment to spend **$30B on Azure over time** [A, reporting on deal terms]. Microsoft recognizes revenue from OpenAI's Azure spend; OpenAI recognizes revenue partly from products distributed via Microsoft. Apple: partnership June 2024 [M] — licensing revenue likely minimal short-term. Circular flow is real but both sides are disclosed-ish; magnitude unquantifiable without Microsoft 10-K segment data, which does not break out OpenAI.

**F. Cash vs revenue:** 2024 cash burn reported ~$5B against $3.7B revenue [A, The Information]; raise cadence: $6.6B Oct 2024, SoftBank talks at $40B+ round in early 2025 [A]. Compute spend commitments ($1.7B+ data-center lease with Oracle, Sept 2024 [M, Bloomberg reporting on filed/leased docs]; Stargate ambitions $500B — aspirational). **Pattern test: raising at rising valuations while losing more dollars than revenue raised — consistent with genuine growth story AND with a cash-burn-dependent story. Cannot distinguish from public data. Confidence: low.**

**G. Falsification:** (1) Audited OpenAI financials — not publicly available, I looked, it's a private company; (2) actual ChatGPT Enterprise seat counts under contract — unavailable; (3) GAAP vs run-rate split — unavailable; (4) Microsoft's disclosure of OpenAI-attributable revenue — not broken out; (5) App-store + web-subscription full telemetry — only partial third-party. **None found; all remain open.**

**Verdict: 3.5/5.** The consumer line at current scale is genuinely unprecedented but arithmetically possible. The definitional slippage of headline numbers is the biggest forensic concern.

---

### CASE FILE 2: Anthropic

**Headline numbers:** [A] ~$1B run-rate claimed mid-2024 (The Information); ~$2B annualized exit-2024 [A, unnamed, Bloomberg/The Information]; projected ~$4B 2025 [A, internal projections cited by Reuters]. Consumer: ~1M+ paid subs [A].

**A. Decomposition [E]:** API ~60-80%; enterprise/BEDROCK-adjacent platform ~15-25%; consumer ~5-15%; government negligible pre-2025 (Palantir partnership for DoD, announced Nov 2024, revenue immaterial so far). **Not disclosed.**

**B. Subscription:** Claude.ai claimed ~4M WAU early 2024, ~10M+ later 2024 [A]. At 2-5% conversion → 200K-500K paid, vs. 1M claimed [A]. Gap of 2-5x. App-store mobile data showed Claude revenue an order of magnitude below ChatGPT's [M, Sensor Tower 2024]. Blended ARPU $20-25 → consumer revenue **$50-150M/yr [E]**, vs. implied claims up to $300-400M. **H1 partially holds for Anthropic: consumer line likely overstated 2-3x if the WAU numbers are the real base.** Most sensitive: conversion rate assumption.

**C. Enterprise/API:** API revenue is usage-based and can scale genuinely fast (a handful of coded apps — Cursor/Windsurf, Notion AI, Slack — plausibly drive 50%+ of API revenue; [A] reporting suggested three coding startups alone were a large share of Anthropic revenue, The Information, 2024). Verifiable logos: Amazon Bedrock (real, primary), Google Vertex, Pfizer, Deloitte, PwC reseller arrangements. The count of verifiable accounts is maybe 30-60. At claimed $1-2B, if API usage from ~10 heavy apps dominates, this is **plausible but fragile** — concentration risk, not fraud risk. **Gap: moderate, explainable by consumption model.**

**D. Definitional:** All numbers anonymous-sourced [A]. "Run-rate" language pervasive — likely peak-month annualization. High laundering risk: Bloomberg, Reuters, The Information all cite overlapping anonymous sources.

**E. Related party — the star exhibit of the whole sector:** Amazon invested $4B (Sept 2023 + Mar 2024 [M]) **and Anthropic committed $4B to spend on AWS** [M, announced terms]; Google invested ~$2B+ [M] and Anthropic committed $3B to Google Cloud [M, announced]. So roughly **$7B of "spend commitments" from a company whose investors injected ~$6-7B**. Now: this is *cost-side* circularity, not revenue inflation per se — it inflates the investors' cloud revenue lines, not Anthropic's. But it also means Anthropic revenue flows through Bedrock/Vertex where Google/Amazon book it and remit. Net-vs-gross question on Bedrock channel: unquantified, undisclosed. **Significant; quantification impossible from public data.**

**F. Cash:** Anthropic raised ~$7-8B across 2023-24 with ~1,000 headcount and claims of $2B run-rate — burn reportedly several $B/yr on compute [A]. Consistent with story.

**G. Falsification:** (1) Bedrock-attributable Anthropic revenue split in AWS filings — not broken out; (2) top-10 API customer concentration — not available, but reporting suggests extreme concentration; (3) consumer paid counts — unavailable; (4) GAAP revenue vs run-rate — unavailable.

**Verdict: 2.5-3/5.** Plausible as a consumption-led business; consumer line likely overstated; headline metrics likely annualized peaks.

---

### CASE FILE 3: xAI

**Headline:** [A] ~$500M ARR claims reported around late 2024/early 2025 (single-source chains); Grok 3 launch Feb 2025 with SuperGrok $30/mo tier.

**B. Subscription:** X Premium subscriber base (all tiers): ~1-2M estimated by third parties [E, from X's own claims of ~1M+ in 2024 and scraping estimates]. Grok's paid base is a subset. Blended ARPU maybe $8-16/mo (Premium tiers $8-16 where Grok is bundled) → **$100-400M annualized max [E], and most of that revenue is X Premium revenue that arguably predates/independent of Grok**. The $500M figure is achievable only by attributing all of X Premium to Grok. **H1 holds: consumer revenue attributable to xAI's own product is plausibly $50-150M [E, low confidence].**

**C. Enterprise:** Essentially zero named enterprise logos. API access launched late 2024, minimal ecosystem. Gap vs. claims: total.

**D. Definitional:** Attribution of X Premium revenue to xAI is the central definitional fraud-risk. Also single-entity merger (xAI absorbed X, March 2025 [A, reported]) creates further definitional chaos — "xAI revenue" would inherit X's revenue, making any future headline meaningless.

**E. Related party:** Nvidia invested in xAI and sells it GPUs [M]; xAI buys compute from/investor-affiliated channels. Round-tripping is cost-side again.

**Verdict: 1.5/5.** Headline numbers not credible as independent-product revenue.

---

### CASE FILE 4: Mistral AI

**Headline:** [A] ~€300M ARR reported mid-2024 (French press, Le Chat success period) — provenance: largely a single anonymous chain; earlier 2024 the CEO publicly minimized revenue focus. Funding ~€600M June 2023, €1.3B+ total by early 2025, round led by General Catalyst/ASML/others.

**B. Subscription:** Le Chat mobile app launched Feb 2025, consumer conversion negligible at that point. Consumer revenue ≤€10M [E, very low confidence].

**C. Enterprise:** Verifiable contracts: French government works councils (La Mission Interministérielle), IBM partnership (Feb 2024 [M]), Microsoft Azure AI distribution (Mistral on Azure [M]), NVIDIA integration, DfE/UK explorations. Named paying logos: **<30 [E from public case studies]**. At €300M ARR, need either 300 seven-figure accounts (don't exist publicly) or massive API volume (ecosystem too thin, apps built on Mist
