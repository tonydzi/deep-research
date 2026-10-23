# DR26-08-01-MACANTON-01-558 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-01-558
rail: chatgpt (codex exec, gpt-5.6-sol, подписка)
session_uuid: 019fbd6d-6d82-7513-b3d7-3a72b2d75b8f
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T13:12:11Z
proof_of_work: 26 уник. URL, 18 веб-поисков в stderr — гейт пройден
---

## Executive conclusion

A stranger’s first contribution is usually caused by three things occurring together:

1. They already have a reason to care—most often because they use the software, need a fix, or were personally invited.
2. They can identify one bounded task whose value and acceptance criteria are obvious.
3. They receive enough human confirmation to believe their effort will not be wasted.

Returning is different. It depends less on more beginner labels and more on whether the first interaction created trust: quick acknowledgment, useful review, a merged and released result, visible credit, and an obvious next task.

For this lab, the `accepted` queue solves part of conversion but none of discovery. At nine visits per week, improving the queue alone is unlikely to produce contributors. The highest-probability strategy is to manufacture a small number of genuine users through integrations, targeted invitations, and upstream relationships—then turn their own encountered problems into contributions.

A crucial evidence limitation: I found no representative study that divides first contributions into “issue discovery vs documentation vs invitation vs downstream bug” percentages, nor reliable contributor-conversion rates for HN, Reddit, awesome lists, or conference talks. Surveys measure motivations, while repository studies observe accepted work; neither establishes the acquisition channel. Any channel ranges below are therefore marked as planning estimates, not published facts.

## What the onboarding evidence says

Steinmacher et al.’s systematic review grouped newcomer barriers into five broad families: finding an appropriate task, technical/setup problems, documentation and project knowledge, social interaction, and understanding the contribution process. Before the first contribution, especially damaging barriers included expectation breakdowns, setup failures, learning curves, and poor reception. The review synthesized evidence from 20 primary studies, but the underlying projects were mostly established communities—not nine-visit/week repositories. [Systematic review](https://www.sciencedirect.com/science/article/pii/S0950584914002390)

Hannebauer and Gruhn’s survey of 117 recently accepted Mozilla and GNOME newcomers reinforces that barriers are contextual: an unexpected obstacle is more discouraging than one the contributor anticipated, and “modifying the software” and “submitting the modification” are separate hurdles. Those projects also had long naturalization histories: earlier evidence cited median times from first contact to accepted patch of roughly 3 months for PostgreSQL, 10 for Apache, and 13 for Python. These figures describe older, large communities and should not be transferred to a small GitHub project. [Hannebauer and Gruhn](https://www.opensym.org/wp-content/uploads/2017/08/a2-hannebauer.pdf)

GitHub’s 2017 survey found:

- 67% considered a license very important when deciding whether to contribute.
- 95% considered responsive maintainers somewhat or very important.
- 18% had personally experienced a negative interaction and 50% had witnessed one; 21% of those affected said it caused them to stop contributing to a project.

The survey also identified incomplete or confusing documentation as the most common user problem. It is large and useful, but self-selected rather than a random sample of all developers. [GitHub Open Source Survey](https://opensourcesurvey.org/2017/)

The Linux Foundation/OpenSSF/Harvard survey of nearly 1,200 contributors found the dominant motivations were adding a needed feature or fix, learning, and enjoyable or creative work. More than half—51.65%—were paid to develop FOSS, so this is not evidence that unknown projects can recruit a large pool of purely discretionary volunteers. It does support “turn users with a need into contributors” more strongly than “advertise volunteer chores.” [2020 FOSS Contributor Survey](https://www.linuxfoundation.org/resources/publications/foss-contributor-2020)

### Tiny project versus large project

For a large project, dominant barriers tend to be codebase complexity, build/setup cost, hidden norms, finding the right subsystem, and waiting for review.

For this project, the order is different:

1. **No exposure and no personal need.** Almost nobody knows the project exists.
2. **Uncertainty that the project is real and durable.** Nine stars, one maintainer, and an AI-heavy workflow increase perceived abandonment and review risk.
3. **Uncertainty about task value.** A newcomer cannot tell whether a seeded defect matters to an actual user.
4. **Setup and architecture comprehension.**
5. **Contribution mechanics.** The new protocol helps here.
6. **Social intimidation.** Likely lower than at Linux/Mozilla, provided the maintainer responds reliably.

That ordering is an inference from the project’s traffic and the literature’s sampling bias; it has not been tested experimentally on tiny repositories.

### What we can and cannot quantify about first-contribution origins

There is no defensible universal breakdown for issue-led, documentation-led, invitation-led, or user-discovered first contributions.

The best adjacent evidence is:

- The LF/OpenSSF survey says needing a feature or fix is the top motivation, but does not identify the first-contribution acquisition channel.
- A study of 171 GitHub newcomers found that “finding a way to start” was the most challenging barrier; 30% could be observed onboarding through GitHub traces, versus 70% in a follow-up survey. It does not provide the requested channel split. [Rehman et al.](https://arxiv.org/abs/2101.08903)
- GitHub includes documentation-like labels in first-issue recommendations, but ranks explicit beginner issues higher. This describes platform discovery logic, not contribution fractions. [GitHub’s good-first-issue system](https://github.blog/open-source/maintainers/how-we-built-good-first-issues/)
- Prior social connections affect project joining, but those studies are observational and mostly concern established teams. [Developer onboarding and prior social links](https://cmustrudel.github.io/papers/fse15onboarding.pdf)

Any article offering clean universal percentages here is almost certainly mixing surveys, GitHub event types, and acquisition channels.

## Does the labelled queue work?

Yes, but only as a conversion surface for people who have already arrived.

An early study of 858 labelled issues found:

- 32.5% were taken by newcomers.
- 39.6% were taken by existing developers.
- 27.8% were deprecated or incomplete.

That 32.5% is not a claim-to-PR rate: it is the share of sampled GFIs taken by newcomers. [Alderliesten and Zaidman](https://azaidman.github.io/publications/alderliestenCHASE2021.pdf)

A study across 11 established projects found average resolution rates of 55.8% for GFIs versus 23.2% for ordinary issues—but project-level GFI resolution ranged from 10.6% to 92.9%. After completing a GFI, the percentage later posting another PR to the same project ranged from 24.9% to 83.9%. Easy completion therefore did not imply retention. Documentation-heavy GFIs appeared weaker for retention than bug-related ones, although that part was exploratory rather than causal. [Tan et al.](https://floss-lab.org/pman/pman3.cgi?DOWNLOAD=199)

Mozilla provides the clearest warning against labels without support:

- First-attempt success: 80.1% for mentored bugs, 85.7% for mentored-plus-GFB, and 67.1% for GFB-only.
- GFB-only first attempts were less likely to succeed than unlabelled attempts.
- Among successful starters, 47.2% of mentored starters and 61.2% of GFB-only starters made at least two contributions.
- Fewer than 15% of onboarding-program participants reached ten contributions, versus more than 20% of nonparticipants.

Mentoring improved first success but did not automatically produce long-term contributors. [Mozilla onboarding study](https://cs.uwaterloo.ca/~rtholmes/papers/msr_2015_labuschagne.pdf)

A 2026 preprint covering 406,826 issues and 1,117 newcomer GFI PRs in 37 popular repositories reports stable newcomer engagement around 27%, while merge rates fell from 61.9% to 42.2% between 2021 and 2025. Because it is a recent preprint and concerns popular repositories, treat it as provisional. [Hoshikawa et al.](https://arxiv.org/abs/2604.27532)

No published study I found reports a general **commented claim → submitted PR** conversion rate. The lab should measure its own.

### A queue that converts

Each `accepted` issue should contain:

- A reproduced failure, preferably with a failing test or one-command reproducer.
- Why the behavior matters to a user or downstream workflow.
- Exact in-scope and out-of-scope boundaries.
- Likely files or subsystem, without writing the solution.
- Acceptance tests and documentation expectations.
- A setup command that has been rerun in a clean environment.
- Estimated effort as a broad class, not false precision.
- Named human reviewer and expected response window.
- A statement that agent use is permitted under the project’s disclosure policy.
- One natural follow-on task if the first patch succeeds.

The seven-day claim expiry is sensible operationally but has no known empirical validation. Do not auto-release a claim silently: ping at days 3 and 6, allow a contributor to extend it, and record why claims end. Otherwise the mechanism can convert normal life interruptions into perceived rejection.

## Cold-start paths, ranked by evidence

The percentages here are either documented platform outcomes or clearly marked planning estimates.

1. **Become useful to downstream users or adjacent projects.** Strongest motivational evidence. A user who hits a real bug has private value from fixing it. There is no general conversion percentage. Planning estimate: fewer than 1% of passive users contribute, but perhaps 5–20% of technically capable users with a blocking, well-scoped bug will engage. Failure is most common because there are no users or the tool is easier to replace than fix.

2. **Personal invitation tied to a person’s demonstrated interest.** Moderate evidence from social-link and socialization research; no clean OSS conversion experiment. Planning estimate: 10–30% reply, 5–15% claim a task, 2–10% submit a PR. Expect 70–90% of invitations not to yield a PR. A generic mass invitation will perform worse and damage reputation.

3. **Useful upstream contributions that create working relationships.** Moderate evidence for prior social ties, but no evidence that PRs operate as advertising. The current result—37 PRs, zero merges—is itself a strong negative signal. Planning estimate after a genuinely merged, substantive PR: 1–5% of people who interact may inspect the author’s work; contributor conversion is usually zero. Stop treating unreviewed PR volume as outreach.

4. **An integration or plugin contributed to a larger ecosystem.** Moderate evidence by mechanism: it places the project where users already solve related problems. No universal conversion rate. Expect most integrations to produce users rather than contributors; one downstream issue or integration PR within 4–8 weeks is a meaningful signal.

5. **Borrowed-audience technical demonstration.** Talks, podcast appearances, guest posts, maintainer newsletters, or another project’s community call. Moderate evidence for visibility, weak evidence for contribution conversion. A relevant niche audience of 100–1,000 may yield 5–50 visits, 0–5 trials, and usually zero PRs; these are planning estimates.

6. **HN, Lobsters, or tightly matched Reddit launches.** Strong evidence that breakout posts can create traffic; weak evidence that traffic produces recurring contributors. One analysis claims 93.2% of roughly four million HN submissions never reached 50 points, although its methodology has been publicly challenged; use it only as a rough failure-rate warning. A 2025 AI-tool study found timing mattered but the `Show HN` tag itself had no advantage after controls. [HN/GitHub diffusion study](https://arxiv.org/abs/2511.04453) Planning estimate: 80–95% of posts produce no durable signal; a modest successful post may yield 50–500 visits, 1–20 trials, 0–2 PRs.

7. **Awesome-list inclusion.** Anecdotally useful for qualified referral traffic, with no convincing causal contributor study. Planning estimate: 0–20 visits/month from a niche list and usually zero PRs. It works only when the tool genuinely belongs and the list itself is maintained.

8. **Meetup or conference talks.** Plausible and potentially high trust, but expensive in human time and poorly studied for unknown-repository conversion. A local talk may produce several conversations and zero PRs. Prefer an existing community’s five-minute demo over organizing an event.

9. **Broad social posting from the author’s own accounts.** Weak in this case: current traffic already shows the audience is not propagating the project. Repeating it without a new result, integration, benchmark, or demonstration is unlikely to change the funnel.

## Retention

The most defensible retention chain is:

```text
First useful patch
  → human acknowledgment
  → actionable review
  → merge
  → release and visible credit
  → personally relevant follow-up
  → growing scope or authority
```

### Response latency

Responsiveness matters greatly: 95% in GitHub’s survey called responsive maintainers important, and studies associate lengthy review with PR abandonment. A 265,325-PR study found complex PRs, novice authors, and lengthy reviews predicted contributor abandonment. [Abandoned PR study](https://doi.org/10.1145/3530785)

However, I found no causal result establishing that 48 hours is a universal cliff or proving it is meaningfully better than seven days. CHAOSS uses “within two business days” as an example benchmark, not an empirically derived threshold. [CHAOSS responsiveness guide](https://www.chaoss.community/practitioner-guide-responsiveness/)

For this project:

- Human acknowledgment within 48 hours is a sensible service target.
- Substantive review within four days is realistic.
- Seven days without any human response is likely damaging.
- A fast bot acknowledgment does not meet the social need; CHAOSS explicitly recommends measuring first human response.

### Review, merge, and tone

Gatekeeper knowledge-sharing and accepting useful newcomer input were positively associated with continued participation in a dataset of 115,948 newcomers to company-owned OSS projects. Rejection and ignored PRs are correlated with fewer future contributions, although selection effects remain. [Gatekeeper study](https://journals.aom.org/doi/10.5465/AMPROC.2023.17622abstract), [PR decisions and future contributions](https://arxiv.org/abs/1812.06269)

Review should therefore:

- Explain the project constraint behind each requested change.
- Distinguish required changes from optional suggestions.
- Avoid rewriting the patch and erasing authorship.
- Close unsuitable PRs promptly with a concrete reason.
- End a merged PR with the next relevant opportunity, not a generic “contribute again.”

### Credit

`CONTRIBUTORS.md` and All Contributors make non-code work visible. Research across thousands of All Contributors projects shows explicit attribution records a broader picture of labor and clarifies what counts as contribution. It does **not** show that installing the bot causally increases retention. [Attribution study](https://research.google/pubs/which-contributions-count-analysis-of-attribution-in-open-source/)

Use credit because it is fair and inexpensive:

- Preserve Git authorship and co-authorship.
- Name contributors in release notes with the effect of their change.
- Credit reproductions, benchmark cases, and design work, not only code.
- Ask permission before using names in promotional posts.

Do not create a giant faces grid solely to inflate the contributor count.

### Governance and commit rights

A visible contributor ladder reduces uncertainty about whether outsiders can ever become trusted maintainers. Evidence supports governance clarity and appropriate distribution of write access as project-health mechanisms, but I found no causal study showing that merely advertising commit rights increases newcomer retention.

Offer a path such as:

- 1 useful contribution: contributor.
- 3 accepted contributions plus constructive review participation: triage permission.
- Sustained ownership of one subsystem: reviewer/CODEOWNER.
- Only after demonstrated judgment: write or release authority.

Do not promise commit rights after a fixed number of PRs; that invites gaming and is unsafe after the XZ social-engineering incident.

A notable case study reports that restructuring data.table governance coincided with a 200% increase in newcomer recruitment, PR resolution falling from over 700 days to under one week, and threefold retention. Because many things changed simultaneously, it is suggestive rather than proof that a contributor ladder caused the result. [data.table case study](https://experts.nau.edu/en/publications/governance-matters-lessons-from-restructuring-the-datatable-oss-p/)

## No CLA and retained copyright

This is a reasonable low-friction policy, but it is not an evidenced acquisition tactic.

GitHub’s survey shows that having a clear license matters—67% rated it very important to contribution—but it did not compare CLA policies. I found:

- No credible natural experiment measuring contribution rates before and after a project removed a CLA.
- No representative estimate of how many prospective contributors abandon because of a CLA or DCO.
- No evidence that an explicit “you retain copyright” statement produces additional contributors.

A CLA can add clicking, legal review, employer approval, and distrust about relicensing. A DCO adds a sign-off requirement but does not normally transfer copyright. Inbound-equals-outbound avoids both. These differences are legally and ideologically meaningful, but their behavioral magnitude is unmeasured.

Keep the statement, but present it as removal of ambiguity:

> Contributions remain yours and are accepted under the repository’s MIT license. No CLA or copyright assignment is required.

Do not market it as a proven contributor magnet.

## AI-assisted contributions in 2026

Disclosure is neither uniformly an asset nor uniformly a liability. The policy landscape is sharply divided:

- NetBSD presumes LLM-generated code is tainted and requires prior written core approval. [NetBSD policy](https://www.netbsd.org/developers/commit-guidelines.html)
- NewPipe asks contributors to avoid work heavily dependent on generated code because it often lacks understanding of project structure. [NewPipe policy](https://github.com/TeamNewPipe/NewPipe/blob/dev/.github/CONTRIBUTING.md)
- Apache Airflow, DataFusion, Arrow, attrs, conda, and others accept AI-assisted contributions with conditions.
- Asahi Linux, QEMU, Clojure, Forgejo, OpenJDK, Servo, Zig, and others reject or severely restrict them. This maintained index links to the individual policies and should be treated as a directory, not research evidence. [OSS AI-policy index](https://github.com/melissawm/open-source-ai-contribution-policies)
- The Linux kernel’s current policy permits tools but requires a human to take full responsibility and uses disclosure/attribution conventions. [Kernel tool-generated-content guidance](https://cdn.kernel.org/doc/html/latest/process/generated-content.html)

The reputational liability is not “AI touched the code.” It is unaccountable volume: unexplained diffs, irreproducible reports, contributors unable to answer review questions, and automated reviewer noise. A 2026 preprint found AI-review-only PRs had a 45.2% merge rate versus 68.37% for human-reviewed PRs, with 60.2% of sampled closed AI-reviewed PRs in the lowest signal-to-noise band. This is recent, observational, and not yet strong enough for universal policy. [AI review-agent study](https://arxiv.org/abs/2604.03196)

For an agent project, the likely contributor pool probably is more AI-enabled than average—but there is no demographic study quantifying this. **Inference:** tasks can assume agent availability while testing distinctly human responsibilities. A good `accepted` issue should require:

- A deterministic reproducer or failing test.
- A concise explanation of why the defect occurs.
- Declared tool/model use when material.
- A list of generated or substantially agent-modified files.
- Evidence that the human ran the tests and understands the diff.
- Small scope and no unrelated “cleanup.”
- The human contributor as the sole accountable interlocutor.

A defensible policy is: **agents may assist; humans submit, explain, test, revise, license, and remain responsible.** Autonomous agent accounts opening unsolicited PRs should not be accepted initially. That boundary is compatible with the project’s subject matter without turning it into an AI-slop destination.

## Ranked action list

Time estimates are ongoing human time; agent execution is excluded.

| Rank | Tactic | Expected effect | Evidence | Human hours/week | First signal | Fatal failure mode |
|---:|---|---|---|---:|---|---|
| 1 | Recruit 3–5 genuine design partners/users in adjacent agent labs | Creates need-driven defects and strongest contributor candidates | Moderate | 1.0 | 1–3 weeks | They never run the tool |
| 2 | Send 2–3 highly specific personal invitations/week | Best controllable path to first human interaction | Moderate | 0.75 | Days–4 weeks | Generic recruiting or mismatched people |
| 3 | Turn one tool into an integration with a used ecosystem | Qualified discovery and downstream need | Moderate | 0.75 | 2–8 weeks | Integration is only promotional |
| 4 | Repair upstream strategy: fewer PRs, prior discussion, one target community | Builds actual social ties and credibility | Moderate | 0.75 | 2–6 weeks | Continuing high-volume unmerged PRs |
| 5 | Rewrite the six accepted issues as executable contracts | Raises arrival-to-PR conversion | Moderate | 0.5 initially, 0.1 upkeep | Next claim | Reproducer/setup does not work |
| 6 | Human acknowledgment ≤48h; substantive review ≤4 days | Reduces abandonment and signals project reality | Moderate–strong | 0.25 at current volume | First issue/PR | AI-only acknowledgment |
| 7 | After every first merge, offer one tailored second task | Directly targets second contribution | Moderate | 0.25 | 1–3 weeks after merge | Generic “more issues welcome” |
| 8 | Ship accepted work quickly and credit it in releases | Closes the motivation loop | Moderate for release; weak for credit causality | 0.25 | Next release | Merged work remains unreleased |
| 9 | Publish one benchmark/demo with a falsifiable result on a borrowed platform | Qualified visibility, not mere awareness | Anecdotal–moderate | 0.5 for 2–3 weeks | Days | Product pitch without reusable evidence |
| 10 | One carefully prepared HN/Lobsters/subreddit launch | Traffic experiment and possible user discovery | Anecdotal | 1–2 once | 24–72h | Measuring stars instead of trials |
| 11 | Submit to 2–3 genuinely relevant awesome lists/directories | Low-cost long-tail referrals | Anecdotal | 0.1 | 2–8 weeks | Irrelevant inclusion or dead list |
| 12 | Add a minimal contributor ladder and AI policy | Reduces governance and accountability uncertainty | Anecdotal | 0.1 | Qualitative | Premature titles or vague AI disclosure |
| 13 | Attribute code and non-code work explicitly | Fairness and social reinforcement | Moderate for visibility; speculative for retention | 0.1 | First contribution | Vanity roster detached from real work |

## A 90-day sequence at four human-hours/week

### Weeks 1–2: make the funnel measurable and credible

Spend the first eight hours on:

- Add a one-question contribution-origin field to the PR template: user bug, accepted issue, personal invitation, upstream interaction, launch/community post, directory/list, or other.
- Add `UTM` parameters to every external post and integration link.
- Rewrite all six issues to include reproducer, clean setup, acceptance tests, scope, expected reviewer SLA, and follow-on task.
- Publish the human-accountability AI policy.
- Add a short contributor ladder.
- Audit the 37 upstream PRs. Continue only those with maintainer engagement or clear upstream value; close none merely for optics, but stop sending more until the approach changes.

Success signal: at least two clean-environment issue reproductions by someone other than the author or the author’s usual agents.

### Weeks 3–4: create actual users

- Identify 10 people working on agent memory, RAG evaluation, citation verification, or agent benchmarks.
- Send five individually researched invitations over two weeks—not “please contribute,” but “you work on X; would you try this 10-minute reproducer against Y and tell me whether the failure criterion matches your workflow?”
- Build one small integration or reproducible comparison with a tool those people already use.
- Make one high-quality upstream contribution only after issue discussion or maintainer confirmation.

Stop condition: if fewer than two of ten invitees respond, revise the project proposition and target list before sending more. Do not scale the same message.

### Weeks 5–6: expose evidence, not the repository

Publish one compact artifact:

- A benchmark result with raw cases.
- A “citation gate caught N fabricated citations across these systems” replication.
- Or an interoperability demo showing two agent runtimes reaching—or failing to reach—consensus.

Place it on one borrowed platform whose audience already discusses that problem. Link to a runnable experiment first and the contribution queue second.

Success signal: 25 qualified repository visitors, five installations/runs, two issue conversations, or one outside reproducer. Stars alone do not qualify.

### Weeks 7–8: one launch experiment

If setup telemetry and external reproduction show the tool works:

- Post once to the best-matched venue: probably Lobsters, HN, or one tightly moderated AI/programming subreddit.
- Explain the observed problem, method, and limitations.
- Include one or two accepted tasks related to the result.
- Be present for the first several hours to answer technical questions.

If the post produces fewer than 25 qualified visits and no external run, do not repeat the same launch elsewhere. Improve the artifact or integration first.

### Weeks 9–10: optimize for the second contribution

For every participant:

- Respond within 48 hours.
- Merge or give decisive feedback within four days where possible.
- Release accepted changes in the next patch release.
- Credit the person and offer one related follow-on task.
- Ask a two-question exit survey if they stop: “What prevented completion?” and “What would make a second contribution worthwhile?”

If there has still been no submitted outside PR, reduce emphasis on beginner-task mechanics. The bottleneck is discovery or user value, not queue quality.

### Weeks 11–12: make the evidence-based decision

Review channel cohorts and stop:

- Channels producing impressions/stars but no installations or conversations.
- Broad social posts without external referrers.
- Upstream PR sending if there is still no substantive review or merge.
- Directory submissions if they send no traffic.
- Any accepted issue claimed twice and abandoned twice; rewrite or remove it.

Continue only sources that produced an external run, reproduced defect, meaningful discussion, PR, or referral relationship.

A realistic 90-day target is **1–3 first contributors and 0–1 repeat contributors**, not 10–50 recurring contributors. Reaching 10 recurring outsiders from the present baseline will probably require a materially larger user base, a dependency relationship, or a sustained partner ecosystem. That forecast is an inference, but it is more consistent with the evidence than promising community growth from six labels.

## Metrics to instrument now

### Leading indicators

Record weekly:

| Metric | Healthy 90-day signal | Failure threshold |
|---|---:|---:|
| Qualified external visitors | ≥150 total | <50 |
| External referrer diversity | ≥4 non-author sources | Still ≥80% author-owned social |
| Clean installations or demo runs by outsiders | ≥10 | <3 |
| External defect reproductions | ≥3 | 0 |
| Meaningful issue conversations | ≥5 people | <2 |
| Targeted invitations sent | 12–20 | Fewer than 8 |
| Invitation reply rate | ≥15% | <10% after 15 invitations |
| Accepted-issue claims | ≥3 | 0 |
| Median first human response | <48h | >72h |
| Median substantive PR review | <4 days | >7 days |

A qualified visitor should reach the setup, example, issue, or technical result—not merely load the repository landing page.

### Funnel metrics

Instrument each person through:

```text
referral → qualified visit → clone/install/run → issue discussion
→ claim → PR → merge → release → second PR → third PR
```

Measure:

- Claim-to-PR conversion.
- PR-to-merge conversion.
- Median claim-to-PR time.
- Claim expiry and stated reason.
- First-to-second contribution within 60 days.
- Second-to-third contribution within another 60 days.
- Human review turns per PR.
- Percentage of PRs released within 14 days.
- Acquisition source per contributor.
- Whether the person was already a user.

Do not combine bots, maintainers, and outside humans in contributor counts.

### Lagging indicators

By day 90:

- First outside PR authors: target 1–3; **failure = 0**.
- First outside merged contributors: target 1–2; **failure = 0**.
- Repeat outside contributors: target 1; absence is disappointing but not conclusive with such a small cohort.
- Third contribution: a stretch result, not a required target.
- Useful external issues/reproductions: target ≥3.
- At least one channel with a demonstrable path to an external run or conversation.

If there are qualified users and issue discussions but no PRs, onboarding is failing. If there are no qualified users, discovery/value is failing. If first PRs merge but nobody returns, the retention loop or continuing personal value is failing.

## Anti-patterns with receipts

- **Contributor-count vanity.** All Contributors research shows that attribution systems reveal previously invisible work; it does not establish that a larger displayed roster means a healthier or more retained community. Count repeat, useful participation instead. [Attribution study](https://research.google/pubs/which-contributions-count-analysis-of-attribution-in-open-source/)

- **Hacktoberfest as acquisition.** In 2020, 621,104 PRs hit participating repositories; only 166,057 were merged, while 34,595 were not accepted and 9,598 were labelled spam or invalid. Another 172,599 went to repositories that had opted out. The event subsequently added opt-in and anti-spam rules. This proves large incentive-driven volume creates substantial triage load; it does not prove every event contributor is low quality. [DigitalOcean 2020 results](https://www.digitalocean.com/blog/hacktoberfest-recap2020)

- **Badly scoped GFI decoration.** In the 858-issue study, 27.8% of sampled GFIs were deprecated or incomplete. Mozilla’s GFB-only first attempts underperformed unlabelled attempts. A label without mentoring, acceptance clarity, or maintenance can actively mislead newcomers.

- **Over-labelling.** The 11-project GFI study found resolution rates from 10.6% to 92.9%. The label is not a portable quality guarantee. More labels can increase stale inventory and claimant competition without increasing useful work.

- **“Contributions welcome” with no work.** The empirical newcomer literature repeatedly identifies finding a suitable starting task as a central barrier. A welcome banner does not resolve task identification.

- **Begging for stars.** Stars measure attention, bookmarking, and social diffusion, not trials or continued labor. Popularity can correlate with survival and contribution while remaining causally ambiguous. For this lab, a star campaign would obscure the actual bottleneck.

- **Small bounty programs.** Gitcoin/Bountysource research finds task description, contributor experience, task type, and bounty amount affect resolution, but does not demonstrate that small bounties build recurring community. Incentives can attract cross-project hunters rather than users. [Gitcoin study](https://www.sciencedirect.com/science/article/pii/S0164121223002303) In 2026 curl ended its security bounty after 20 submissions early in the year reportedly yielded no concrete vulnerability and imposed high triage cost. That is a security-bounty case, not ordinary development, but it demonstrates the AI-era adverse-selection risk. [curl report](https://www.itpro.com/software/open-source/curl-open-source-bug-bounty-program-scrapped)

- **Upstream PR count as marketing.** Thirty-seven sent and zero merged is not borrowed trust; it is an unresolved-work backlog. An upstream relationship begins with accepted value and substantive interaction, not a link in the author profile.

## What the evidence does not support

The literature reviewed does **not** support the following common claims:

- That adding `good first issue` labels by itself increases contributor acquisition.
- That seven days is the optimal claim duration.
- That 48 hours is a proven causal threshold rather than a practical responsiveness target.
- That documentation PRs are universally the best first contribution. They can be accessible, but exploratory GFI evidence suggests they may retain less well than meaningful bug work.
- That `CONTRIBUTORS.md`, release shout-outs, badges, or an All Contributors bot measurably increase return rates.
- That advertising a path to commit rights causes retention, although governance clarity and earned authority are sensible.
- That no-CLA/inbound-equals-outbound measurably increases contributions.
- That HN, Reddit, Lobsters, awesome lists, meetups, or conference talks have stable visit-to-recurring-contributor conversion rates.
- That upstream contributions reliably send contributors back to the contributor’s own repository.
- That a dependency automatically develops a contributor community; many dependencies have large user bases and almost no outside maintenance.
- That stars are a leading indicator of recurring contributors.
- That AI disclosure is generally rewarded or generally punished. Current project policies conflict sharply.
- That contributors to agent projects are predominantly agent users, though it is a reasonable audience hypothesis to test.
- That autonomous agents can replace human review, accountability, or relationship-building without reducing trust or increasing maintainer load.
- That the first 10–50 recurring contributors can be produced predictably on a four-hour weekly budget before the project has genuine outside users.

The lab’s immediate problem is not “how to recruit 50 contributors.” It is to establish one repeatable path from **outside need → trusted interaction → shipped improvement → renewed need**. Until that path works for the first two or three people, broader contributor-growth tactics are mostly premature.
