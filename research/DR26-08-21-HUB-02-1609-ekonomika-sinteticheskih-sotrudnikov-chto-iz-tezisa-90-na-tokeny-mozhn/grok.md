# DR26-08-21-HUB-02-1609 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. EXECUTIVE VERDICT (stress-test of the thesis)

The thesis mixes three different claims and treats them as one. They are not the same, and they do not currently stand or fall together.

Claim 1 — “Valley fully-loaded is ~$10–15k/month.”  
PARTIALLY TRUE for mid-level non-engineering knowledge work in the Bay Area. FALSE for mid-level software engineers and false as a stand-in for “a Silicon Valley employee.” Bay Area SWE fully-loaded employer cost is typically ~$20–35k/month if equity is counted as a real cost, ~$16–25k/month on cash + benefits alone. The $10–15k band is a marketing average that quietly swaps “SWE in SF” for “ops/support/content person in SF.”

Claim 2 — “Offshore is ~$800–1,500/month plus heavy management.”  
DIRECTIONALLY TRUE for wages of mid-skill BPO/knowledge workers in the Philippines and for many India services-firm salaries. INCOMPLETE as a delivered-cost number. Client bill rates, EOR fees, vendor margin, rework, overlapping hours, and a local manager/QA layer usually put delivered cost in the ~$1,500–4,000/month band for usable mid-level output, higher for actual product engineers.

Claim 3 — “Claude/ChatGPT at $200–500/month is cheaper than the cheapest offshore human and replaces up to 90% of routine knowledge-work staff.”  
The sticker price is real. The comparison is not. A subscription is not an FTE. At employee-replacement intensity, token burn, rate limits, human review, rework, orchestration, evals, and liability put a defensible “AI-agent FTE” in a wide band of roughly $2,000–10,000/month depending on function and how much senior human time is still in the loop. That can beat Bay Area labor. It does not reliably beat a well-run Philippines/India BPO seat on cost, and it does not currently match that seat on accountability, long-horizon reliability, or client-facing trust.

Claim 4 — “Winning org design = few high-level orchestrators + fleets of synthetic employees.”  
PLAUSIBLE as a 2026–2028 design pattern for some functions (L1 support, first-draft content, boilerplate code, structured back-office). NOT evidenced as a general replacement for 90% of knowledge staff. The public “one-person unicorn” discourse is mostly AI product companies selling picks and shovels, not traditional firms that fired their workforce and kept revenue.

Bottom line on the 90% figure: it is not a defensible staff-replacement rate for a typical knowledge-work business in 2026. What is defensible is task-level substitution of 30–70% of tickets/drafts/boilerplate in a few narrow functions, plus 10–30% net capacity gain in several others, with a human still owning quality, exceptions, and liability. Using “90% of staff” in marketing is the motte-and-bailey: 90% of some routine tasks in some jobs, sold as 90% of people.

Epistemic label for this whole report: produced under an instruction not to retrieve live sources in this pass. Figures are from established public series and well-known 2023–2026 papers/reporting. Treat every point estimate as “verify against the primary page before publishing.” A confident wrong number is worse than a range.

------------------------------------------------------------------------------
B. PART 1 — THE COST SIDE

B1. Fully-loaded human cost, 2025–2026 band

Method note. “Fully loaded” here means what the employer actually spends to have one productive seat: cash pay + payroll taxes + benefits + typical tools/equipment + a share of management/HR. It does not include office rent unless noted. Equity is called out separately because including or excluding it moves Bay Area tech by a factor of ~1.5–2.

US baseline (MEASURED, high confidence for the ratio, not the Bay premium).  
BLS Employer Costs for Employee Compensation (ECEC) is the cleanest official series. Through 2024–2025 it has consistently shown total private-industry employer cost well above wage: benefits are on the order of ~30% of total compensation, or ~40–45% on top of wages, for civilian workers. For professional and related occupations the wage is higher but the benefits ratio stays in a similar band (payroll taxes, insurance, retirement). Use ~1.30–1.45× base wage as the US benefits+tax load for non-equity roles. Source: BLS ECEC quarterly releases.

Management overhead is not in ECEC. Spans of control in knowledge work are commonly 1:6 to 1:12. Fully allocating a $180k manager across 8 ICs is ~$22k/year (~$1.8k/month) per seat. That is ESTIMATED, medium confidence. Offshore and AI comparisons that ignore this are cheating; comparisons that apply it only to offshore are also cheating.

1) SF Bay Area

Mid-level software engineer (L4 / “mid”).  
MEASURED-ish (crowdsourced, medium-high confidence on total comp, not on accounting treatment): Levels.fyi and Radford/Carta-type surveys through 2024–2025 put Bay Area mid-level SWE total compensation typically ~$230k–$380k (base often $160k–$210k, rest bonus+equity). Glassdoor base-only numbers look lower because they under-count equity.

Employer cost:
- Cash + benefits (1.35× base, plus modest bonus): roughly $18k–$26k/month. ESTIMATED from BLS load × Levels.fyi bases.
- Including economic cost of equity (the grant the company actually dilutes/expenses): roughly $22k–$35k/month. ESTIMATED.
- Thesis $10–15k/month: too low for this population by ~2×.

Mid-level non-SWE knowledge worker (ops, CS lead, bookkeeper/controller-track, content, research associate, recruiter, mid-marketer).  
Bay cash pay commonly ~$90k–$150k. Fully loaded without equity: ~$10k–$18k/month. This is the only Bay population for which the thesis band is honest. Sources: BLS OES/OEWS metro wages for San Francisco-Oakland-Hayward and San Jose-Sunnyvale-Santa Clara; Glassdoor/Payscale as secondary. OEWS is wage, not TC; add the ECEC load.

US remote (same roles, not Bay)

Wage discount vs Bay is typically ~15–35% depending on role and company policy (some firms now pay one national band; many still geo-adjust). Mid knowledge worker fully loaded: ~$7k–$13k/month. Mid SWE: ~$14k–$25k/month cash+benefits, more with equity. Medium confidence.

2) Eastern Europe (Poland, Romania, Czechia; Ukraine is cheaper but war-risk adjusted)

Mid-level SWE, 2024–2025: commonly €40k–€80k gross in Poland/Romania product companies; Ukraine/Belarus contractor rates can be lower. Sources: local labor stats (GUS Poland, INSSE), Levels.fyi sparse EU data, Deel/Remote salary guides, Stack Overflow survey. Medium confidence.

Fully loaded via EOR (Deel/Remote/Papaya): gross + ~20–30% social contributions depending on country + 10–20% EOR fee. Delivered cost for mid SWE: roughly $4.5k–$9k/month. Mid non-SWE knowledge: roughly $2k–$5k/month. Medium confidence.

3) India

Two different markets are constantly conflated.

Services-firm / BPO / captives (TCS, Infosys, Wipro, large captives): mid knowledge/IT salary often ~₹8–20 lakh/year (~$10k–$24k). Philippines-like BPO voice/back-office can be lower. Source: NASSCOM wage commentary, AmbitionBox/Glassdoor India, Indian EPFO/labour commentary. Medium confidence on ranges, not on any one job title.

Product / Global Capability Centre / strong startup: mid SWE often $25k–$50k+ cash in 2024–2025, FAANG-India much higher. Using “India = $800–1,500” for this population is false.

Delivered cost if you buy from a vendor: bill rates for mid IT services are often ~$18–$40/hour (much higher for niche/onshore-hybrid). That is ~$3k–$7k/month at full utilization, before your own PM. Salary-to-bill multiples of 1.5–2.5× are normal. MEASURED in industry, medium confidence; any one vendor’s rate card varies.

Direct hire + EOR: mid knowledge often ~$1.2k–$2.5k/month fully loaded; mid product SWE ~$2.5k–$5k+. Thesis $800–1,500/month matches junior-to-mid services/BPO salaries, not delivered senior output.

4) Philippines

BPO is the relevant market. Government and industry sources (PSA wage stats, IBPAP/contact-center industry commentary, job boards) have long put typical voice/non-voice agent monthly pay in a band that, converted, is often ~$400–$900. Team leads, accountants, L2 support, “knowledge process” roles: often ~$700–$1,500. 2024–2026 inflation and peso moves shift this; treat as a band.

What the Western buyer pays is not that wage. Standard BPO bill rates for competent English non-voice work commonly land around $8–$18/hour depending on complexity, which is ~$1,400–$3,100/month per seat at reasonable utilization, plus your vendor manager. High-end KPO (finance, medical, legal process) is higher.

Thesis $800–1,500: plausible as wage, or as a cheap bill-rate, not as “fully loaded usable mid-level equivalent to a Valley IC.”

5) What “plus heavy management and QC” actually means

Offshoring literature (Lacity, Willcocks, Oshri; a long trail of IS/outsourcing research; practitioner syntheses from Everest Group, HfS, NelsonHall) repeatedly finds that wage arbitrage is not take-home savings. Recurring extra costs: overlapping hours or delayed cycle time, tighter specs, duplicated QA, travel, vendor management, attrition/training (BPO attrition of 20–40%+ annually is a standard industry complaint), rework, and cultural/context loss. Practitioner rules of thumb that extra coordination eats ~15–40% of gross wage savings are common. That is ESTIMATED / industry-consensus, not a single RCT.

A usable comparison is therefore:

- Philippines agent wage: ~$0.5–1.0k
- Philippines delivered seat: ~$1.5–3.5k
- India services delivered mid seat: ~$2–5k
- India product mid SWE: ~$3–6k
- EE mid SWE: ~$5–9k
- US remote mid knowledge: ~$7–13k
- Bay mid knowledge: ~$10–18k
- Bay mid SWE: ~$20–35k

B2. Real cost of AI agents at “employee replacement” intensity

Sticker prices (MEASURED, high confidence as of 2025 public list prices; re-check before publish):

- ChatGPT Plus ~$20/month; ChatGPT Pro ~$200/month (OpenAI pricing page).
- Claude Pro ~$20/month; Claude Max announced in 2025 at $100 (5×) and $200 (20×) (Anthropic newsroom/pricing).
- Google AI Ultra was announced at $249.99/month (Google I/O 2025 cycle). Verify current SKU.
- Cursor Pro/Business: typically tens of dollars per user/month on the list price; usage-limited, with usage add-ons. Check cursor.com/pricing at publish time.
- GitHub Copilot Individual/Business/Enterprise: tens of dollars per user/month.
- API list prices move. A working 2025-style anchor: Claude Sonnet-class ~$3/million input tokens and ~$15/million output tokens; OpenAI and Google have their own matrices, with cheaper mini/flash and more expensive reasoning SKUs. Always cite the live pricing page.

Sticker $200–500/month is therefore real for one human sitting in one IDE/chat with a premium subscription. It is not the cost of an unsupervised employee substitute.

Token burn at replacement intensity (ESTIMATED, medium-low confidence; this is the number the thesis needs and the industry has not standardized).

Agentic coding/research is not a 2,000-token chat. It is loops: read repo or docs → plan → tool calls → errors → retries → tests → re-plan. Public anecdotes from power users of Cursor/Claude Code/Codex in 2025 commonly talk about tens to hundreds of millions of tokens per month per heavy user. That is not a peer-reviewed panel; treat as ESTIMATED.

Fermi check (ESTIMATED):
- Assume 150M input + 50M output tokens/month of Sonnet-class API at $3 / $15 per million.
- API bill ≈ $150×3 + $50×15 = $450 + $750 = $1,200/month.
- Heavier reasoning models and more retries: $2,000–$6,000/month is easy to hit.
- Lighter support-classification or template content: $50–$400/month in API.

Subscriptions hide this until you hit rate limits. Replacement intensity is exactly when you hit them. Then you either pay API, buy more seats, wait, or downgrade quality. “$200/month, unlimited employee” is a marketing confusion between seat price and meter price.

Hidden costs that belong in fully-loaded AI FTE (mostly ESTIMATED; a few measured pieces):

1. Human review and specification time.  
The 2025 METR randomized study on experienced open-source developers (Cursor + model stack, early 2025) found developers took ~19% longer when allowed to use AI, while believing they were faster. That is MEASURED, high importance, and the single best public warning against naive substitution math. Even if your domain is more AI-friendly than mature open-source repos, you must budget review. A conservative ops design is 0.2–0.5 senior hours per agent-hour of output for anything that can ship to a customer or a codebase. At a $100–150/hour fully-loaded US senior rate, 2 hours/day of review is $4k–$6k/month — already more than the subscription.

2. Rework / hallucination / silent failure.  
Air Canada was held responsible for its chatbot’s invented refund policy (Moffatt v. Air Canada, British Columbia Civil Resolution Tribunal, 2024). That is a legal data point on who pays when the agent is wrong. Rework rates are not in a clean public series. Vendor case studies always show falling handle time; they rarely show end-to-end cost including tickets reopened and brand damage.

3. Orchestration, evals, prompt/ops maintenance.  
This is a new job. Someone writes specs, builds eval harnesses, watches drift, rotates models, manages tool permissions, and keeps context/docs fresh. For one founder tinkering, cost is “evenings.” For a fleet meant to replace staff, it is at least a part-time engineer/ops role ($5k–$20k/month fully loaded depending on geography). SPECULATIVE as a ratio; real as a line item.

4. Infrastructure and compliance.  
SSO, logging, DLP, no-training-on-our-data contracts, SOC2 vendor review, secret management, extra seats for staging, code-privacy mode. For a regulated SMB, easily $0.5k–$3k/month plus counsel time. ESTIMATED.

5. Multiple seats, not one.  
“Replace an employee” in practice is often ChatGPT + Claude + Cursor + a browser agent + a meeting-notetaker + Zapier/Make. Stack cost of $300–$800/month before API overage is common. ESTIMATED from public list prices.

A defensible fully-loaded “AI agent FTE” band for 2026:

| Intensity | Monthly cost band | What’s in it | Confidence |
| Naive sticker | $20–$250 | one Pro/Max seat | High |
| Power-user coding/research | $500–$3,000 | Max/Pro + API overage or usage packs | Medium |
| Replacement attempt with US reviewer | $3,000–$10,000 | tools + API + 0.2–0.5 FTE senior review + evals | Medium-low |
| Replacement attempt with offshore reviewer | $1,500–$5,000 | tools + API + PH/IN reviewer + a local owner | Medium-low |

The thesis compares line 1 of this table to line “offshore wage.” Honest comparison is line 3 or 4 to “offshore delivered seat.”

B3. Management-overhead: offshore QC vs AI supervision

What the literature actually says about offshore:

Decades of IS outsourcing research: cost savings exist but are a function of process maturity. If the work is modular, specified, and measurable (L1 support, payroll ops, transcription, QA scripts), offshore works and management overhead is a known, budgetable percentage. If the work is tacit, political, or underspecified, coordination cost explodes and quality falls. This is established, not new.

Typical practitioner ratios (industry, medium confidence): 1 onshore lead per 8–15 offshore agents in mature BPO; tighter (1:5–8) for knowledge-process work. QC is often a separate 10–20% of headcount in the vendor’s own factory, already partly inside the bill rate.

What exists for AI supervision:

There is not yet a BLS-quality series. What exists:

- Brynjolfsson, Li, Raymond (NBER / QJE pipeline; 2023 working paper on a Fortune 500 support center): generative AI assistance raised productivity ~14% on average, ~34% for the least skilled, and reduced tenure gaps. This is augmentation with humans still on the ticket, not a 1:20 robot factory. MEASURED.
- Dell’Acqua et al., HBS/BCG (2023): GPT-4 made consultants faster and better inside the model’s frontier, and worse when they trusted it outside the frontier (“falling asleep at the wheel”). Supervision quality, not headcount, was the binding variable. MEASURED in a lab/field hybrid.
- METR long-task work (2025 papers and updates): success rates collapse as task horizon lengthens; the 50%-completion time horizon has been on the order of tens of minutes to low hours depending on the date/model, growing fast, but not “a workday unsupervised.” MEASURED on their task suite, not on your back office.
- METR developer RCT (2025): AI slowed experienced developers. Supervision/overhead can exceed gross generation gains. MEASURED.
- Vendor claims (Klarna, Intercom Fin, Zendesk, Salesforce Agentforce): often “X% of conversations resolved without a human.” These are not independent. Treat as vendor-measured, high risk of metric gaming (deflection of easy tickets, not true resolution).

Emerging claimed ratios in 2025–2026 practitioner discourse: 1 human orchestrator per 5–20 agents. Almost all SPECULATIVE. Nobody has published a clean panel of “tickets per orchestrator at constant quality over 12 months” independent of vendors.

Comparison that survives scrutiny:

- Offshore: high coordination cost, but the human agent still has persistent memory, shame, employment law, and the ability to own a three-day thread. QC is a solved (if expensive) operational practice.
- AI: low coordination cost per extra copy, near-zero marginal wage, but supervision is about sampling and evals rather than line-management, and the failure mode is confident silent error rather than slow work. QC is not a solved operational practice at employee-grade reliability.

There is no measured result that “AI supervision cost is lower than offshore QC cost at equal output quality” for a full job. Task-level, yes in some support/coding microtasks. Job-level, no reliable data.

------------------------------------------------------------------------------
C. PART 2 — THE CAPABILITY SIDE

C4. Functions with documented replacement or major augmentation

Support / customer service — strongest documented, and the most walked-back.

- Klarna × OpenAI (Feb–Mar 2024 case study): claimed the assistant did the work of 700 FTE, handled ~2/3 of chats, ~2.3 million conversations in a month, resolution time down (Klarna/OpenAI public case). Later 2025 reporting: CEO said they had gone too far, quality/brand suffered, and they would hire more humans into customer service (Bloomberg and subsequent coverage, May 2025). So: documented automation of a large share of chats; not a documented permanent 700-person replacement.
- Brynjolfsson, Li, Raymond: +14% productivity, humans remain. Best academic evidence for augmentation, not replacement.
- Intercom Fin, Zendesk AI, Salesforce Agentforce: vendor-reported autonomous resolution rates often in the 30–65% band on eligible chats. Independent replication is thin. Use as “possible on L1, vendor-measured.”

Software development — major augmentation, weak replacement.

- GitHub Copilot lab study (Peng et al. 2023, ACM): ~55% faster on a narrow HTTP-server task. MEASURED, not a job.
- Google (Sundar Pichai, 2024 earnings cycle): ~25% of new code related to AI. That is generation share, not net productivity or headcount.
- Microsoft / GitHub: large paid Copilot base; firm-level headcount reduction attributable to Copilot is not shown in public filings as a causal fact.
- METR 2025 RCT: experienced OSS developers slower with AI. Directly contradicts “junior-dev FTE replacement is free.”
- SWE-bench Verified and similar: best models by 2025–2026 solve a majority of some isolated GitHub issues in the benchmark, far from “own this service in production for a quarter.”

Content — volume replacement, quality and distribution not solved.

- Lots of SMBs use models for first drafts. Documented publisher failures: CNET AI articles with errors (2023); Sports Illustrated AI-generated staff scandal (2023); Google helpful-content / spam updates punishing scaled AI content. Augmentation of a writer is real; replacement of an editorial function is repeatedly punished by quality and platform risk.

Bookkeeping / back office — partial, legally bounded.

- Tools (Ramp, Pilot, Digits, Puzzle, numerous AP/AR agents) automate coding, extraction, reconciliation assistance. CPA/controller sign-off and exception handling remain. No clean public case of a mid-size firm deleting finance staff 90% and surviving audit. AICPA/state accountancy rules still sit on the human.

Research / analysis — augmentation.

- Strong at synthesis and first pass. Weak at citation fidelity and on data the model cannot see. Anthropic Economic Index and OpenAI usage reports show research/synthesis as a top use, mostly as a copilot.

Ops / RPA+LLM — works when the process was already a process.

- Structured workflows (invoice intake, KYC document pack, status emails) are the real quiet wins. Unstructured cross-tool ops (“be my COO”) remains demo-grade. OSWorld / WebArena-style computer-use benchmarks through 2025 still showed large failure rates on multi-step GUI work (often well below reliable employee grade). Check the latest leaderboard before citing a number; the level is “improving fast, not employee-reliable.”

Hiring-freeze evidence, not replacement evidence.

- IBM (2023, WSJ): CEO Arvind Krishna said AI could replace ~7,800 back-office jobs over time (~30% of non-customer-facing roles). That was a hiring-pause thesis, not a measured 7,800 termations-and-replacements study. IBM continued to hire in other categories.
- Shopify (Tobi Lütke, 2024 memo): required AI-use proof before adding headcount. This is process, not a published FTE-deleted number.
- Duolingo (2024): reported cutting a portion of contractors in an “AI-first” shift. Subsequent public argument over content quality. Scale of replacement is contractor hours, not 90% of staff.

C5. Claims that were walked back, qualified, or did not mean what the headline said

1. Klarna “700 agents.” 2024: headline replacement. 2025: company publicly re-emphasized humans, cited quality, moved to rehire/rebuild CS. Also: much of Klarna’s headcount drop in 2023–2024 was a profitability restructuring, not a 1:1 robot swap. Instructive because the metric (chats handled) was real and the job-replacement interpretation was oversold.

2. IBM 7,800 jobs. Framing slid from “AI will do these jobs” to “we will not backfill some roles” to ongoing mixed hiring. No public after-action that 7,800 roles are now empty and AI-operated at equal quality.

3. “AI laid off our newsroom/content team.” CNET, SI, and various SEO content farms showed the failure mode: you can replace writers with models; you then pay in errors, reputation, and distribution. Several outlets added human review back.

4. Broad 2023–2024 tech layoffs attributed to AI. Meta, Google, Amazon, Stripe, etc. cut staff for rates, overhiring, and efficiency. Attribution to “GPT replaced them” is mostly narrative. Revelio Labs / Challenger Gray-style breakdowns did not show AI as the primary coded reason for the big rounds.

5. Chegg. Often cited as “AI replacement.” Actual mechanism: OpenAI ate Chegg’s customer, not Chegg’s internal headcount via agents. That is demand destruction, the opposite org lesson.

6. DoNotPay / “robot lawyer.” Regulatory and legal collision; the product could not occupy a licensed-employee slot.

7. Autonomous coding agents as employees (Devin-class 2024 demos). Useful tools; public evidence of firms deleting junior benches and keeping velocity/quality is anecdotal and contested. Benchmarks ≠ a year of on-call.

8. Gartner (2024): prediction that a large share of genAI projects would be abandoned after PoC. This is not a single-company walkback; it is the industry-level walkback of “deploy and fire.”

Pattern: chat/ticket deflection is easy to measure and easy to over-claim. Twelve-month quality, employee knowledge loss, and customer willingness to be stuck with a bot are what force the walkback.

C6. What the deep studies actually support

METR, task length (“Measuring AI Ability to Complete Long Tasks,” 2025, and subsequent horizon updates).  
Supports: exponential growth in the length of tasks models can complete at ~50% success, historically on the order of doubling every several months on their suite. As of the 2025 public points, that horizon was roughly hour-scale, not week-scale.  
Does not support: 90% staff replacement, unattended multi-day jobs, or “cheaper than offshore FTE.”  
Label: MEASURED on a constructed task distribution.

METR, experienced developer RCT (2025).  
Supports: naive “AI makes developers much faster” can be false for experts on real repos; perceived speed ≠ actual speed.  
Does not support: “fire juniors, keep Cursor.” If anything, it warns that the review tax is real.

Anthropic Economic Index (2025–).  
Supports: real usage is concentrated in coding and production tasks; a mix of automation and augmentation; geographic and occupational skew. It is an observational usage index, not a causal replacement census.  
Does not support: 90% FTE substitution.

OpenAI / Eloundou, Manning, Mishkin, Rock (2023, Science/working paper).  
Supports: ~80% of US workers have at least 10% of tasks exposed to GPT-class tools; ~19% have ≥50% of tasks exposed. Exposure ≠ automation ≠ displacement.  
Does not support: 90% of staff. Arithmetic: even the high-exposure group still has ~50% of tasks not exposed in that paper’s coding, and exposure is “could help,” not “does the job.”

McKinsey (2023–2025 genAI reports).  
Supports: a large share of employee time is theoretically automatable (they have used figures in the 60–70% of tasks range for some occupations); economic value in the trillions if adopted; most value requires process rewiring.  
Does not support: you can fire 90% next quarter. McKinsey’s own implementation reports emphasize the gap between pilots and P&L.

Goldman Sachs (2023 and follow-ups).  
Supports: large number of jobs “exposed” globally; modest-looking macro productivity increment over a decade in the base case (order-of-magnitude: around a percentage point or so per year in some of the 2023 framing, with wide uncertainty — quote the latest GS note, not memory, if you publish the 1.5% figure).  
Does not support: firm-level 90% replacement as a safe strategy.

Academic labor (Autor; Acemoglu; Brynjolfsson).  
Consensus shape: task-level substitution + complementarity, not occupation extinction in the short run. Acemoglu has been on the skeptical end of near-term macro TFP impact. Autor emphasizes new tasks and bottleneck human skills.  
None of this literature gives a 90% FTE number for 2026.

“AI cheaper than offshore” as a study conclusion: I could not identify a peer-reviewed, apples-to-apples 2025–2026 paper that measures fully-loaded AI-agent output against fully-loaded India/Philippines output at matched quality for a full job. Vendor blogs exist. That gap is itself a finding.

What the studies jointly support: task-level substitution and uneven augmentation.  
What they do not support: replacement of 90% of routine knowledge-work staff at employee-grade reliability.

------------------------------------------------------------------------------
D. PART 3 — ORG DESIGN

D7. Orchestrator + AI fleet; “one-person unicorns”; revenue per employee

Category error first. Midjourney, Cursor, OpenAI, Anthropic, ElevenLabs, Perplexity, etc. have extreme revenue-per-employee because they sell software with near-zero marginal cost, often with a foundation-model or design moat. They are not case studies of a logistics firm, clinic, agency, or SaaS-implementer that replaced humans with Claude Code. Using them as proof of the thesis is like using Aramco to prove one person can run an oil industry.

Documented high revenue-per-employee tech firms long predate agents (WhatsApp at acquisition, Instagram at acquisition, YouTube early, Bloomberg, many product-led SaaS firms). AI may raise the ceiling; it did not invent the pattern.

What is real in 2025–2026:

- Tiny product teams shipping with heavy AI assistance: widely attested, rarely audited.
- “Solo agency” operators claiming $20k–$100k/month with AI production: abundant on X/YouTube, almost no independent verification, extreme survivorship bias. SPECULATIVE as a sector fact.
- Support orgs running a bot in front of a smaller human tier: real (Klarna, many SaaS companies). The human tier did not go to zero.
- Shopify-style “AI is a default coworker, argue for any new seat”: real as management practice.
- Cognition, Sierra, Harvey, Glean, etc.: companies whose product is the agent, sold into firms that still have employees.

Outliers attributable to AI rather than to being a software company: no clean public panel. If you need a publishable sentence, stay qualitative: “AI is raising output per worker in some functions; audited cases of traditional businesses running at software-like revenue per employee solely because of agents are not yet in the public record.”

D8. Skill profile of orchestrators; what happens if you cut first

The human skills that show up across the studies and the walkbacks:

- Spec writing: turning tacit “how we do things” into evals and acceptance tests.
- Taste / QA: the BCG result — knowing when you are outside the model’s frontier.
- Exception handling and customer trust: the Klarna 2025 reversal.
- Systems ownership: permissions, data boundaries, audit logs.
- Domain liability: the Air Canada holding; licensed professions; finance sign-off.
- People who still understand the work well enough to notice silent failure. This is the killer. If you fire the people who could have caught the error, the eval harness is now your only immune system, and most SMBs do not have one.

What happens when staff is cut before orchestration exists (instructive, not a large-N study):

- Quality regressions on the messy 20% of cases that were never in the FAQ.
- Institutional memory loss (the unwritten process).
- Rehire at higher cost under time pressure (Klarna-shaped story).
- Legal ownership of bot speech (Air Canada-shaped story).
- Remaining humans overloaded as “AI janitors,” which is how you get the METR paradox: more generation, less net speed.

The thesis’s org design is the right cartoon of a possible 2027 firm. The 2026 constraint is that orchestrators are scarcer than agents, and you cannot promote a deleted workforce into that role after the fact.

------------------------------------------------------------------------------
E. PART 4 — THE HONEST COUNTER-CASE

E9. Where agents still fail at employee-grade reliability

Long-horizon work. METR’s measured horizon is the cleanest public number: models can do short tasks; reliability falls as the task grows to hours. A full-time employee is mostly multi-hour, multi-day, interrupted, context-switching work with undocumented constraints. There is no public dataset showing 90%+ autonomous completion of real 4–8 hour knowledge-work shifts. Computer-use benchmarks (OSWorld, WebArena and successors) through 2025 remained far from “hire this instead of a VA.” Cite the latest % from the paper you quote; do not invent a 2026 figure here.

Accountability. An employee can be fired, sued, insured, put on a PIP, called as a witness, and hold a license. An agent cannot. Firms still need a named human on regulated output (finance, law, medicine, insurance, credit, HR decisions, advertising claims).

Client trust. Buyers of B2B services often purchase a person. The Klarna reversal is partly a trust/quality story, not a token-price story.

Memory and organizational context. Agents do not stably own a 9-month client relationship, a messy codebase’s tribal knowledge, or a fight with finance about last year’s exception, unless you build retrieval and process around them. That build is the actual project.

Security and IP. Prompt injection, data leakage into consumer plans, agents following malicious instructions in a webpage or email. Employee-grade reliability includes “does not wire money to the attacker in the invoice.”

Multi-hour autonomous task-completion: no public series at employee-grade (≥95% success with acceptable tail risk) for general knowledge work. Benchmarks at 50–80% on short, scored tasks are not that.

E10. Legal / HR / advertising exposure of telling businesses to cut staff for AI

This is not legal advice. It is a map of where the thesis becomes a liability if used as public education or ads.

United States
- FTC Act §5: unfair/deceptive practices. Performance claims (“replace 90% of staff,” “cheaper than any offshore employee”) are the kind of objective claims that need substantiation at the time you make them. Income/savings claims attract extra scrutiny. The FTC has already gone after unsubstantiated AI claims (e.g., 2024 actions around AI-washing and fake AI products).
- Employment: you can generally lay off in the US at-will, subject to contracts, anti-discrimination, and WARN (60-day notice for covered mass layoffs). Advising a mass-layoff-for-AI plan can drag the adviser into the fact pattern if it is a paid engagement; as content, the risk is more FTC/reputation than WARN.
- If the content is an ad: platform policies (Meta, Google) restrict misleading business-opportunity and “get rich / fire your team” style claims. Expect review friction, not a dedicated “AI replacement” statute.
- Licensed verticals: cannot tell a firm to replace lawyers/CPAs/clinicians with a bot as if that satisfied the licensed duty.

European Union
- Collective redundancies: Directive 98/59/EC plus national law — information and consultation before mass cuts. “Fire 90% because Claude” without works-council process is an employment-law problem for the employer, and a due-diligence problem for anyone selling that plan as a service.
- GDPR: automated decision-making (Art. 22) if the agent decides on people (hiring, credit, firing, support that affects legal rights).
- AI Act: transparency for certain systems; high-risk uses (employment, credit, etc.) carry duties. A chatbot replacing staff is not automatically “high-risk,” but HR-decision agents can be.
- Unfair commercial practices: overstated savings/replacement claims in commercial communications are the EU cousin of FTC §5.

UK: similar consultation duties; ASA can treat misleading savings claims as advertising issues.

RU-speaking markets
- Russian advertising law (FZ-38 “On Advertising”) prohibits unreliable advertising; savings and efficacy claims need a factual basis. Labor Code layoff procedures still apply to the employer. There is no well-known special statute that bans “replace staff with AI” as speech, but consumer-protection and advertising unreliability are the hooks. Confirm with local counsel before RU ads; this is not a researched 2026 case-law survey.
- EU-adjacent RU-speaking (Baltics, etc.): EU rules apply.

Organic social vs paid
- Organic educational content still has misrepresentation risk if it is effectively selling a method with false precision (“90%,” “$200 vs $10k”).
- Paid social: higher chance of enforcement by platform policy than by a ministry, in practice.
- “Up to 90%” does not save you if 90% is not a realistic expected outcome. “Up to” is a classic advertising red flag when the typical result is 10–30%.

Safe posture for public education: describe task shares, cite named studies, give ranges, disclose vendor-claim status, do not give a formula that sounds like financial advice to fire people.

------------------------------------------------------------------------------
F. COST-PER-FTE COMPARISON TABLE

All monthly USD, mid-level, 2025–2026 bands. “AI agent” is not legally an FTE.

Profile | Monthly employer/delivered cost | What the number is | vs thesis | Confidence | Primary source class
SF Bay mid SWE | $20k–$35k | cash+benefits+equity economic cost | Thesis 2× too low | Medium | Levels.fyi + BLS ECEC load
SF Bay mid knowledge (non-SWE) | $10k–$18k | cash+benefits | Thesis OK | Medium | BLS OEWS metro + ECEC
US remote mid knowledge | $7k–$13k | cash+benefits | — | Medium | BLS + geo-discount surveys
US remote mid SWE | $14k–$25k cash+ben (more w/ equity) | | — | Medium | Levels.fyi + ECEC
Eastern Europe mid SWE | $4.5k–$9k | gross+social+EOR | — | Medium | National stats + EOR guides
Eastern Europe mid knowledge | $2k–$5k | same | — | Medium | same
India mid product SWE (direct) | $2.5k–$5k+ | salary+statutory+EOR | Thesis too low | Medium | AmbitionBox/NASSCOM/EOR
India mid services/BPO delivered | $2k–$5k | vendor bill, not wage | Thesis mixes wage & delivered | Medium | industry bill-rate commentary
India / PH wage only, junior-mid BPO | $0.5k–$1.5k | paycheck | Thesis cites this | Medium | PSA/job boards/AmbitionBox
Philippines delivered BPO seat | $1.5k–$3.5k | bill rate + your VM share | Honest comparator | Medium | IBPAP/practitioner rates
AI sticker (Pro/Max/Ultra) | $0.2k–$0.5k | seat, rate-limited | Thesis cites this | High | vendor pricing pages
AI power-user + API | $0.5k–$3k | tools + tokens | Missing from thesis | Medium-low | pricing × anecdotal burn
AI “replacement attempt” + US review | $3k–$10k | tools+tokens+senior time+evals | The real number | Medium-low | Fermi from METR/BCG + wages
AI + offshore reviewer + local owner | $1.5k–$5k | the actually-cheap hybrid | Not in thesis | Medium-low | Fermi

Reading the table without lying:
- AI sticker beats everyone. Irrelevant.
- AI+US reviewer beats Bay SWE, competes with Bay non-SWE, may or may not beat US remote, usually loses to a good PH/IN delivered seat on cost alone.
- The cost-winning 2026 design is not “fire 90% for Claude Max.” It is “offshore or junior humans + AI tools + a strong local orchestrator,” or “AI front layer + smaller human back layer” in support.

------------------------------------------------------------------------------
G. VERDICT ON THE 90% CLAIM

The phrase to unpack: “replace up to 90% of routine knowledge-work staff.”

If “routine” is defined as FAQ-grade support chats, boilerplate CRUD, first-draft social posts, invoice field extraction, after a year of process work: a 70–90% task-automation rate on that slice is conceivable and has vendor-measured cousins (Klarna’s two-thirds of chats). That slice is not 90% of staff in a real company.

If the claim means 90% of knowledge-work headcount in a typical SMB/SME: not supported. A defensible, citable 2026 picture:

Function | Defensible 2026 statement | Not defensible | Evidence grade
L1 customer support | 30–70% of eligible tickets deflected or drafted by AI, humans on the rest and on quality | “Delete 90% of CS” as a permanent, quality-neutral move | Medium (Klarna both directions; BLR 14%)
Junior coding / boilerplate | 10–40% faster on some tasks; possible to stop hiring 1 of N juniors in greenfield CRUD | “Replace the junior bench”; expert slowdown possible | Medium (Copilot lab vs METR RCT)
Senior engineering | Copilot/agent as tool | FTE replacement | Medium-high against replacement
Content drafts | High share of first drafts | High share of published output with no editor | Medium (publisher failures)
Bookkeeping | High share of extraction/coding assistance | Replace controller/CPA | High against full replacement
Research / analysis | Faster first pass | Unattended deliverable to a client | Medium
Ops / VA / admin | Strong on structured; weak on messy multi-hour computer use | 90% VA replacement | Low-medium
Org-wide knowledge staff | 10–30% net task-hours in many firms if they actually rewire process; concentrated in 1–2 functions | 90% of staff | Low for the 90%; medium for “material but partial”

A publishable alternative to 90%:  
“In a few narrow, high-volume, well-specified functions — especially L1 support and templated production — AI can now take a majority of the tasks. Across a whole knowledge-work company, evidence supports task substitution and selective hiring freezes, not nine-out-of-ten people gone.”

------------------------------------------------------------------------------
H. FIVE CASES FOR, FIVE FAILURES

Five most citable FOR (use with the qualifiers attached)

1. Brynjolfsson, Li, Raymond — 551-agent support center, ~14% productivity, larger gains for novices. Best academic citation. Qualifier: humans still did the job.
2. Klarna/OpenAI 2024 — 2/3 of chats, 700-FTE-equivalent workload, speed metrics. Best commercial citation. Qualifier: 2025 human-quality reversal.
3. Peng et al. / GitHub Copilot 2023 — ~55% faster on a specific programming task. Qualifier: lab task, not FTE.
4. Dell’Acqua et al. HBS/BCG 2023 — quality and speed up inside the frontier for consultants. Qualifier: quality down outside the frontier.
5. Shopify 2024 AI-headcount memo / similar “prove you needed a human” policies; plus the existence of real bot-deflection products (Intercom Fin, etc.) with named customers. Qualifier: policy and vendor metrics, not a 90% firing study.

(Honorable: OpenAI exposure paper for “how much of a job is even in range”; Anthropic Economic Index for “what people actually use models for.”)

Five most instructive FAILURES / walkbacks

1. Klarna 2025 — the same flagship case, reversed on quality and human contact.
2. METR 2025 developer RCT — AI slowed experts; kills naive unit-economics.
3. Moffatt v. Air Canada 2024 — the company owns the bot’s lie.
4. CNET / Sports Illustrated AI-content scandals — replacing writers is easy; remaining a publisher is not.
5. IBM 7,800 / general “AI layoffs” headlines — hiring-freeze rhetoric sold as completed replacement, never shown at that scale with quality held constant.

(Honorable: DoNotPay vs licensed work; Gartner PoC-abandonment prediction; Chegg as demand-destruction misread as internal replacement.)

------------------------------------------------------------------------------
I. NUMERIC CLAIMS SAFE vs NOT SAFE TO PUBLISH

SAFE if you attach the source and the qualifier

- BLS ECEC: US private employers pay on the order of 30% of total compensation in benefits (quote the latest table, not this sentence’s rounding).
- Bay Area mid SWE TC commonly $230k–$380k on Levels.fyi-class data in the mid-2020s; fully loaded is not $10k–$15k/month.
- ChatGPT Pro is $200/month; Claude Max was launched at $100 and $200; cite the live page.
- Eloundou et al.: 80% of workers have ≥10% of tasks exposed; 19% have ≥50% exposed.
- Brynjolfsson et al.: ~14% support-productivity lift in that deployment.
- Peng et al.: ~55% faster on that Copilot lab task.
- Klarna 2024: ~2/3 of customer chats via the assistant in the case study period; claimed 700-FTE equivalent workload.
- Klarna 2025: company publicly said it needed more humans in customer service.
- METR 2025 RCT: ~19% slowdown for experienced OSS developers with AI.
- Air Canada was liable for chatbot misinformation (2024 tribunal).
- Philippines/India agent wages can be under $1,500/month; delivered BPO seats are typically higher.

NOT SAFE (misleading, over-precise, or false as stated)

- “A Silicon Valley employee costs $10–15k/month” as a universal. Too low for SWE; only OK for some non-SWE roles with a label.
- “Offshore fully loaded is $800–1,500.” That is wage, not delivered cost, and not Indian product engineering.
- “Claude/ChatGPT is $200–500/month, therefore cheaper than any human FTE.” Compares a seat to a salary and ignores review, tokens, and liability.
- “Replace up to 90% of staff with AI agents.” No study supports 90% of staff. “Up to” does not make it OK.
- “AI is cheaper than the cheapest offshore human” as a fully-loaded, equal-quality claim. Not shown.
- “700 employees were replaced at Klarna.” The company described FTE-equivalent work and later re-emphasized humans. Do not say 700 people were deleted and stayed deleted because of the bot.
- “IBM replaced 7,800 people with AI.” Not established.
- “One-person unicorns prove the org model.” Those firms sell software.
- “AI coding agents replace junior developers.” METR + SWE-bench + on-call reality do not allow this as a blanket.
- Any exact token-burn FTE (“an agent costs $347/month”) unless you measured it on your workload.
- Any orchestrator ratio (“1 human per 20 agents”) as if it were a standard.

The exact sentence that is honest enough for education content:

“Premium AI subscriptions cost a few hundred dollars a month, and in some support and drafting tasks they already take a majority of the volume. That is not the same as employing a person. Once you add usage, review, and the messy work, AI is often cheaper than a Bay Area hire and not magically cheaper than a good Philippines or India seat. Firms that reported huge bot deflection have also had to put humans back. Plan for a hybrid: fewer new hires in narrow functions, not a 90% headcount wipeout.”

------------------------------------------------------------------------------
J. COULD NOT VERIFY IN THIS PASS

Live retrieval was forbidden by the output contract, so the following were not re-opened against primary pages dated 2026-08. Do not treat them as audited.

- 2026 BLS ECEC and OEWS exact dollars for SF, San Jose, and US remote.
- 2026 Levels.fyi medians by level and metro.
- Current Anthropic/OpenAI/Google/Cursor/GitHub list prices, rate limits, and overage SKUs (these move).
- Current Sonnet/GPT/Gemini API rates and any 2026 price cuts.
- Token-burn distributions for Claude Code / Codex / Cursor at “full workday replacement” — no official panel found in the prior public record; only anecdotes.
- Klarna’s actual CS headcount 2023 vs 2024 vs 2025, rehires, and NPS/CSAT series (media summaries only).
- IBM realized (not predicted) role deletions attributable to AI.
- Intercom/Zendesk/Salesforce independent audited autonomous-resolution rates.
- Philippines IBPAP and India NASSCOM 2025–2026 wage yearbooks, peso/rupee average FX for a specific month.
- Typical 2026 BPO bill rates from a named rate card.
- EOR fully-loaded calculators for PL/RO/IN/PH on today’s tax tables.
- METR’s latest 2026 time-horizon point (the series moves; quote the paper version).
- OSWorld/WebArena/SWE-bench leaderboard numbers as of 24 Aug 2026.
- A peer-reviewed AI-vs-offshore equal-quality cost study — not verified because it does not appear to exist.
- Revenue-per-employee for Midjourney/Cursor/etc. (private or poorly sourced).
- Solo-agency income claims.
- Whether Google AI Ultra is still $249.99.
- EU/US/RU enforcement actions specifically against “fire your staff, buy our agents” advertising after mid-2025.
- Russian-language FAS/advertising cases on AI-replacement claims.
- Insurance (E&O) premium deltas for AI-operated CS.
- Actual management-span measurements for AI orchestrators in production.
- The fraction of knowledge-work hours that are “routine” in a typical SMB — no standard statistic; the 90% claim smuggles this.

------------------------------------------------------------------------------
K. HOW THIS SHOULD CHANGE THE THESIS (not extra research — the implication)

Keep: AI seats have a ridiculous sticker price versus Bay Area humans; some functions already run a majority of volume through models; org design is moving toward orchestrators plus tools; offshore wage quotes understate delivered cost.

Kill or rewrite: $10–15k as “the Valley employee”; $800–1,500 as “the offshore employee fully loaded”; $200–500 as “the AI employee fully loaded”; “cheaper than the cheapest offshore human”; “replace up to 90% of staff”; “one-person unicorns” as proof for non-software operating companies.

Replace with a true, sharper thesis:  
The scarce resource is not tokens. It is people who can specify work, catch silent failures, and own the customer. AI collapses the cost of generating a first pass. It does not collapse the cost of being right. The winning 2026 design is a small number of expensive orchestrators, optional cheap human reviewers in PH/IN, and agents on a meter — not a Max subscription standing in for a company.
