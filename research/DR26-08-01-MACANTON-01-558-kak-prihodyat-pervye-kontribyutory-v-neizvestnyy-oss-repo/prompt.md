# DR26-08-01-MACANTON-01-558 — the exact prompt sent to every model

ROLE: You are a research analyst studying how small, unknown open-source projects acquire their
first 10-50 recurring outside contributors. Your reader runs a two-person AI research lab, not a
company with a DevRel budget. Prefer measured evidence over advice-column consensus; where the
evidence is thin, say so explicitly rather than filling the gap with plausible-sounding practice.

## The situation, honestly stated

A personal GitHub account (~13 public repos, largest 9 stars, 12 followers, ~250 commits over 60
days) publishing tools from running a multi-agent AI lab: agent memory on SQLite, a verbatim
citation gate that catches fabricated RAG citations, cross-machine agent consensus, an agent
runtime integrity benchmark, a Claude Code "second brain" starter kit. Python, MIT licensed.

Current state as of 2026-08-01:
- external contributors: **0**
- 37 upstream PRs sent to 26 other repos: 33 open, 4 closed, **0 merged**
- traffic to the flagship repo: ~9 visits/week, ~100% referred from the author's own Facebook post,
  0 external referrers, 0 forks
- just built: a labelled `accepted` issue queue with a written claim protocol (comment "claiming
  this" → 7 days), 6 genuinely reproduced defects seeded in it, and an explicit no-CLA policy where
  contributors keep their copyright (inbound = outbound under MIT)

Constraint: no budget for paid promotion, no existing audience, and the maintainers are one human
plus AI agents — so any tactic requiring sustained daily human hours will not survive.

## The question

**What actually causes a stranger to make a first contribution to an unknown repository, and what
causes them to come back a second and third time?** Separate the two — first contribution and
retention are different problems with different levers.

## What I need you to cover

1. **Evidence on the first contribution.** What does research on OSS onboarding actually show about
   the barriers (Steinmacher et al. on newcomer barriers, Hannebauer, the "good first issue"
   literature, GitHub/Linux Foundation/OpenSSF contributor surveys, Mozilla and CHAOSS work)? Which
   barriers dominate for a *tiny, unknown* project versus a large one — they are not the same list.
   Quantify where you can: what fraction of first-time contributors come from issues vs docs vs
   personal invitation vs a downstream bug they hit themselves?

2. **Does the labelled-queue mechanic actually work, and under what conditions?** There is published
   analysis of `good first issue` labelling effectiveness — including findings that badly scoped
   labelled issues *hurt* (newcomers claim and abandon). What distinguishes a queue that converts
   from one that decorates? What is the empirical claim-to-PR conversion rate, and what predicts it?

3. **The cold-start problem specifically.** For a repo with near-zero traffic, contributors cannot
   arrive from the queue because nobody sees it. What are the documented paths from zero to first
   contributor? Cover at minimum: being a dependency of something else, being discovered through the
   author's upstream contributions to bigger projects, HN/Reddit/Lobsters launches, awesome-list
   inclusion, conference/meetup talks, "borrowed audience" appearances on other people's platforms,
   and direct personal invitation. Rank by evidence of actually working for unknown projects, and
   give the realistic conversion numbers for each — including how often each fails.

4. **Retention: what makes a contributor come back?** Evidence on maintainer response latency
   (what threshold matters — is 48h meaningfully better than a week?), review tone, first-PR
   experience, credit and recognition mechanics, `CONTRIBUTORS.md` / all-contributors bots,
   co-authorship on releases, governance signals (can a contributor gain commit rights, and does
   offering that path measurably increase retention?).

5. **Does the "you keep your copyright, no CLA" stance actually attract contributors?** This is the
   lab's explicit bet. Is there evidence either way — do CLA/DCO requirements measurably depress
   contribution rates? Compare projects that switched CLA policy if any natural experiments exist.
   Be willing to conclude that the effect is small or unmeasured; do not manufacture support for the
   bet just because the reader made it.

6. **The AI-agent dimension, treated seriously and skeptically.** The lab builds with AI agents and
   says so publicly. (a) How are maintainers in 2026 actually reacting to AI-assisted contributions
   — is disclosure an asset, a liability, or noise? Cite concrete policies from real projects.
   (b) Conversely: for a project *about* AI agents, is the contributor pool different — are the
   likely contributors themselves running agents, and does that change what a good `accepted` issue
   looks like? (c) Is there a defensible way to let AI agents contribute to your project, or is
   that a reputational trap right now?

7. **Anti-patterns with receipts.** What reliably backfires: contributor-count vanity metrics,
   Hacktoberfest-style spam magnets, over-labelling, begging for stars, "we welcome contributions!"
   with an empty queue, bounty programs at small scale. Where possible cite the project and the
   measured outcome, not the folk wisdom.

## Output format

- **A ranked action list** of 10-15 tactics, each with: expected effect, evidence strength
  (strong / moderate / anecdotal / speculative), human-hours per week to sustain, time-to-first-signal,
  and the failure mode that kills it.
- **A 90-day sequence** for this specific situation, assuming ~4 human-hours/week plus agent time.
  Say what to do in week 1 versus week 12, and what to stop doing if it has not produced a signal.
- **A metrics section**: what to instrument now to know in 90 days whether this worked, with
  thresholds that would count as failure. Distinguish leading from lagging indicators.
- **A "what the evidence does not support" section** — tactics that are widely recommended but that
  you could not find real evidence for. This section is required, not optional.
- Cite sources with links. Where a claim is your inference rather than something you read, mark it
  as inference. Where sources disagree, show the disagreement instead of averaging it away.
