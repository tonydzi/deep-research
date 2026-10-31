# DR26-09-14-ZB-09-2008 — the exact prompt sent to every model

# Forensic investigation: are the headline revenues of frontier LLM labs arithmetically possible?

You are a forensic financial analyst in the tradition of short-seller research desks
(Muddy Waters, Hindenburg, Kynikos) and of accounting sceptics like Francine McKenna
and the FT Alphaville / Bethany McLean school. Your job is NOT to cheer and NOT to
debunk. Your job is to rebuild the revenue of AI labs BOTTOM-UP from unit economics
and check whether the top-down numbers the market repeats can actually exist.

Think as deeply and as long as you can. Turn on the strongest reasoning / deep-research
mode available in this account. I need a maximally thorough, sourced, dated answer with
explicit confidence levels and explicit gaps.

## The claim under test (treat as a hypothesis to falsify, not a conclusion)

H1 (subscriptions): consumer subscriptions cannot carry the reported revenue, because
the paid-conversion rate on freemium AI chat products is low (order of 3-10% of MAU,
possibly lower), the $100-250/month tiers are a very thin sliver (order of tenths of a
percent to low single-digit percent of paying users), and the blended ARPU is therefore
far below what a naive "N million users x $20" arithmetic implies.

H2 (enterprise): "enterprise revenue" is being inflated by (a) counting bookings, ACV,
TCV, RPO or non-binding LOIs as revenue; (b) a small number of very large contracts
(including related-party or investor-affiliated ones) doing most of the work;
(c) credits, discounts and compute-for-equity round-tripping. To produce $1B+ of real
annualised enterprise revenue you need hundreds of seven-figure accounts, and the
enterprise software market historically produces that cohort slowly.

H0 (null / the steelman you MUST also argue): the reported numbers are broadly real.
API + enterprise seats + government contracts + a genuinely unprecedented consumer
conversion curve can produce them. Argue this case as hard as you argue H1/H2, and say
which side the evidence actually favours.

## Companies (cover at least 6; run each as its own case file)

1. OpenAI
2. Anthropic
3. xAI
4. Mistral AI
5. Perplexity
6. Cohere
Plus, as CALIBRATION ANCHORS (they file audited numbers, so they bound what is
plausible): Microsoft's AI/Copilot disclosures, Alphabet/Google Cloud + Gemini,
and at least one mature SaaS comparable (e.g. Salesforce, Datadog, Snowflake) for
how fast a company historically reaches N hundred seven-figure logos.

## What each case file must contain

A. **Revenue decomposition.** Split every reported figure into: consumer subscription /
   prosumer high tier / API usage / enterprise seats & platform / government & defence /
   licensing & other. Where the split is not disclosed, say so and give your estimated
   split with a stated method.

B. **Bottom-up subscription model.** Build the arithmetic explicitly:
   MAU or WAU (source + date) -> paying conversion % -> tier mix ($20 vs $100-250) ->
   churn -> blended ARPU -> implied subscription revenue. Show the range (bear / base /
   bull) and compare it with what the company or its leaks claim. Name the single
   assumption that the answer is most sensitive to.

C. **Bottom-up enterprise model.** How many paying enterprise accounts would be needed
   at what average contract value to produce the claimed enterprise line? Then go find
   how many named, verifiable large customers actually exist (press releases, case
   studies, procurement records, USASpending / TED / GOV.UK contract registers, SEC
   filings of the CUSTOMER side, which are often the only audited evidence). Compare
   the required count with the observable count and quantify the gap.

D. **Definitional forensics.** For every headline number, establish which metric it
   actually is: GAAP revenue, annualised run-rate (ARR) from a single good month or
   week, bookings, RPO, gross vs net of inference cost, gross vs net of revenue share
   with a cloud partner, or a journalist's paraphrase of an unnamed source. Flag
   annualisation of a peak month, and flag any number that traces back to a single
   anonymous press report that everyone then re-cited (citation laundering).

E. **Related-party and circular-flow analysis.** Map compute-for-equity, vendor
   financing, investor-customers, and cloud-credit round-trips. Where an investor is
   also a large customer, quantify how much of the revenue line that relationship could
   explain.

F. **Cash vs revenue.** Revenue claims are cheap; cash burn is not. Estimate gross
   margin after inference cost, and reconcile the revenue story with the size and
   frequency of the funding rounds. A company raising every few months at a rising
   valuation while claiming rapid revenue growth is a specific, testable pattern:
   test it.

G. **Falsification list.** For each company list the 3-5 specific facts that, if found,
   would DISPROVE your conclusion, and say whether you looked for them and what you found.

## Evidence rules (non-negotiable)

- Cite a source and a DATE for every number. Prefer, in order: audited filings of the
  company or of its counterparties; regulatory and procurement registers; court and
  litigation exhibits; primary company disclosures; reputable investigative press with
  named documents; then everything else.
- Distinguish MEASURED from ESTIMATED from ASSERTED. Mark each number.
- Two sources agreeing does NOT confirm a fact if both trace to the same origin.
  Explicitly check for shared provenance before calling anything corroborated.
- Say plainly where the data does not exist. "I could not find it" is a valid and
  valuable answer; do not fill gaps with plausible-sounding numbers.
- Note your training-data cutoff and mark which parts of the answer may be stale.

## Deliverable

1. One-page verdict: for each company, a score on "reported revenue is plausible
   bottom-up" (1-5) with the single strongest piece of evidence for and against.
2. A comparison table across all companies: claimed revenue, bottom-up reconstructed
   range, gap, dominant revenue source, biggest unverifiable component.
3. The three specific numbers that, if any regulator or journalist obtained them,
   would settle the question fastest - and where those numbers physically live.
4. An explicit statement of which of H1 / H2 / H0 the evidence supports, per company
   and overall, with your confidence level.

Internal analysis, not for publication. Be blunt.
