# DR26-08-01-MACANTON-01-558 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-01-558
rail: grok (Grok Build CLI, подписка Heavy)
public_chat_url: НЕТ (CLI-рельса)
collected_at: 2026-08-01T13:12:11Z
proof_of_work: ⛔ НЕТ — 0 URL, 0 веб-поисков в stderr. Отчёт связный и с именами исследований,
  но проверить, что он читал источники, НЕЛЬЗЯ ⇒ второе мнение, НЕ доказательная база.
---

I'll pull the empirical literature first—newcomer barriers, GFI conversion, cold-start paths, CLA effects, retention, and 2026 AI-contribution norms—then synthesize ranked tactics for your zero-external-contributor case.# What Actually Produces Outside Contributors on a Zero-Traffic Repo

**Scope note.** This is written for a two-person lab with ~4 human-hours/week, ~13 public repos, 0 external contributors, 0 merged upstream PRs, and ~9 visits/week to the flagship. Evidence is weighted toward measured studies; where the literature is thin or sampled only on large projects, that is stated explicitly.

---

## 1. Evidence on the first contribution

### What the barrier literature actually says

**Steinmacher and co-authors** (CSCW 2015 social-barriers study; IST 2015 SLR; ICSE 2016 portal study) remain the backbone. Across literature + interviews + observation they catalogued on the order of **50+ barriers**, commonly grouped as:

| Category | Examples | Why it matters for *tiny* projects |
|---|---|---|
| Finding a way to start | “What should I work on?” | Dominant for large projects; **almost irrelevant if nobody lands on the repo** |
| Social interaction | No reply, rude reply, unclear ownership | **More lethal** for small projects — one human is the entire social layer |
| Technical hurdles | Build, tests, domain knowledge | Still real; agent-era projects can *raise* the floor (multi-machine, SQLite schemas, RAG pipelines) |
| Documentation | Missing CONTRIBUTING, setup fails | Easy win; still not traffic |
| Newcomer’s previous knowledge | “I don’t know this stack” | Pool is narrow for agent-memory tooling |

Key result from the social-barriers work: many dropouts are **not** “couldn’t code the fix” — they are “couldn’t get a usable social signal that the project wanted them.” That is especially true when the maintainer is a single person.

**Hannebauer et al.** treat contribution barriers as the inverse of motivation: barriers are *project properties* that block people who already want to contribute. Their work (OpenSym / thesis line) is consistent with Steinmacher: technical barriers are real, but motivation without a clear entry path still dies.

**Rehman et al.** (cited in Hao et al. 2026): roughly **~30% of people who attempt onboarding actually land a first contribution**; primary blockers are identifying an entry task and acquiring project-specific knowledge. That is an *attempt → first merge* figure among people already trying to join *some* project — not among random GitHub visitors.

**Jensen et al.** (mailing-list “newbies” analysis, widely cited in later GFI mentoring work): a response within **~48 hours** to a newcomer’s first contact is positively associated with continued participation. This is correlational, not a clean RCT, but it is one of the few quantified latency findings the field keeps reusing.

### Large project vs tiny unknown project: the barrier list flips

| Dominant barrier on *large* projects | Dominant barrier on *your* situation |
|---|---|
| Finding a suitable task among thousands of issues | **Anyone ever seeing the repo** |
| Competing with other newcomers for GFIs | Empty inbound funnel |
| Social intimidation of a huge core team | Maintainer may look abandoned (no stars, no external PRs, all traffic self-referred) |
| Complex build / CLA / CI | Domain novelty of multi-agent / RAG tooling |

**Inference (marked):** For a repo with ~9 visits/week and 0 external referrers, optimising GFI labels without a discovery channel is rearranging furniture in an empty store. The lab’s labelled `accepted` queue is necessary but not sufficient; discovery is the binding constraint.

### Where first contributions *come from* (fractions — sparse)

There is **no clean, recent, multi-project census** that says “X% of first PRs come from labelled issues vs docs vs invite vs self-hit bug” for *unknown* repos. What exists is partial:

| Path | What evidence exists | Strength |
|---|---|---|
| **Downstream user hit a bug / needed a feature** (“scratch own itch”) | Classical OSS motivation literature; still the modal story maintainers tell in interviews; survey developers say they often join *because they already found a problem*, not to hunt GFIs (Alderliesten/CHASE survey line) | **Moderate** for *why* people contribute once present; **weak** as a cold-start path for zero-traffic tools |
| **Labelled issues (GFI / help wanted)** | Works mainly on *already-visible* projects. Longitudinal 2021–2025 study of 37 *popular* repos: ~**27% of GFIs get a newcomer PR**; **~73% of GFIs never get one** (Hoshikawa et al. 2026) | **Strong on large/popular**; **not transferable to zero-traffic** |
| **Docs / typos as first PR** | Common advice; Subramanian et al. show first contributions skew to small/easy work including docs — but that is *type of first PR*, not *discovery channel* | **Moderate on type; weak on discovery** |
| **Personal invitation / warm handoff** | Strong in industry practice; almost no controlled measurement of conversion rates for micro-projects | **Anecdotal / moderate** in community health writing |
| **Upstream contributions by the author** | Common founder story (“I fixed X in Y, people found my related tool”); thin quantified literature | **Anecdotal** |

**Honest fraction answer:** For *popular* GFI-using projects, newcomer engagement with GFIs is ~27% of labelled issues, not “most newcomers come from GFI.” For *unknown* projects, the literature does **not** support GFI as a discovery mechanism — only as a conversion surface *after* arrival.

---

## 2. Does the labelled-queue mechanic work?

### What works (when someone already sees the repo)

From Tan et al. 2020 (GFI “first look”), Alderliesten et al. CHASE 2021, Tan et al. 2023 (mentoring on GFIs), and Hoshikawa et al. 2026:

| Finding | Number / result | Source |
|---|---|---|
| GFI adoption among sampled projects | ~43% of sampled repos had GFI-like labels (Alderliesten sample) | CHASE 2021 |
| GFIs as share of all issues | ~1.5% in that sample; ~0.6–0.9% monthly GFI ratio in popular repos 2021–25 | Alderliesten; Hoshikawa |
| Newcomer engagement with GFIs | **~27%** of GFIs get a newcomer PR (stable over 4 years) | Hoshikawa 2026 |
| Merge rate of newcomer GFI PRs | **~62% → ~42%** over 2021–2025 on popular projects | Hoshikawa 2026 |
| Expert/mentor involvement on GFIs | **~70%** of closed GFIs have expert comments; median first expert reply **~8.5 hours** after newcomer comment (half of cases) | Tan et al. 2023 |
| Mentoring vs retention paradox | Expert involvement **positively** correlates with successful *first* contribution; **negatively** with *retention* in their GLM (ability/willingness dominate retention) | Tan et al. 2023 |
| Interaction beats issue text for retention | Hao et al. 2026 (45k first-issues, 777 repos ≥200 stars): interaction features beat intrinsic issue attributes for predicting whether the first-issue solver returns | Hao et al. 2026 |

So: labels help **task selection** on visible projects; **human response + mentoring** drives first merge; **retention** is a different equation (interaction quality, not cheerfulness alone).

### What makes a queue convert vs decorate

| Converts | Decorates / hurts |
|---|---|
| Reproduced defect + expected behaviour + failing test or script | “It would be nice if…” with no repro |
| Scoped to one module; “definition of done” explicit | Touches architecture / multi-agent consensus core without map |
| Claim protocol with expiry (your 7-day claim) | Open forever; abandoned claims block others |
| Maintainer replies in hours–days | Labels only, silence |
| Mentoring comments *during* work (Tan 2023) | “good first issue” on a 400-line design rewrite |
| Bug fixes show higher merge rates historically (~69% bug vs ~53% docs in Hoshikawa task split) | Feature-request GFIs without acceptance criteria |

**Claim-to-PR conversion rate:** There is **no published empirical rate** for a formal “comment ‘claiming this’” protocol at micro-project scale. Closest proxies:

- ~27% of GFIs ever get a newcomer PR (popular projects).
- Of those, merge rates historically ~50–60% band, declining.
- **Inference:** On a zero-traffic repo, claim→PR conversion will be dominated by **claim volume ≈ 0**, not by claim protocol quality. The protocol is still rational insurance *when* volume appears (prevents silent abandon).

Your setup (reproduced defects, claim window, no CLA) is **aligned with best practice** for conversion *conditional on visitors*. It does not create visitors.

### Badly scoped labels hurt

Literature and maintainer practice converge: over-broad “GFI” leads to claim → fail → abandon → maintainer cleanup. Xiao et al. and related recommender work note failed first attempts waste both sides. Hoshikawa notes some popular projects **reduced** GFI labelling post-2024 amid spam / AI-slop pressure (yt-dlp anti-spam anecdote in their discussion — anecdotal cause, measured supply drop).

---

## 3. Cold-start: paths from zero to first outside contributor

Ranked for **unknown micro-projects** (not for Kubernetes-scale). Conversion numbers are order-of-magnitude where measured; otherwise failure frequency is qualitative.

| Rank | Path | Evidence it works for unknowns | Realistic conversion | How it fails |
|---|---|---|---|---|
| **1** | **Direct personal invitation** of people who already felt the pain (past collaborators, people who DMed after a post, workshop attendees) | Strong practice evidence; weak academic RCT | **Inference:** 5–20 targeted asks → 0–2 first PRs if ask is a *specific issue link* + offer of live help; higher if they already use the tool | Vague “contributions welcome”; wrong person; no time-bound ask |
| **2** | **Be a dependency / fix upstream consumers** (publish on PyPI; get imported by *one* other active project) | Classical OSS “itch”; LF guides emphasize users-of-the-software as contributor source | Very long-tailed: most packages get 0 reverse deps; **one** reverse dep with active users can produce 1–3 contributors over months | Tool is a “lab notebook export,” not a dependency |
| **3** | **Author’s upstream reputation** (merged PRs / sustained review in bigger agent/LLM repos) | Anecdotal but repeated founder path; your current 0/37 merges is the opposite signal | Each *merged* PR is a credential; unmerged cold PRs do not convert strangers | Silent queues; PR-as-spam pattern (your 33 open / 0 merged is a measured self-problem) |
| **4** | **Borrowed audience** (guest post, podcast, other people’s newsletters, co-maintained demos, “we used X to build Y”) | Practice; no standard conversion rate for code contrib | Discovery conversion often **stars/clones ≫ PRs**. **Inference:** 1 useful technical write-up on a borrowed channel → tens–hundreds visits if channel is warm; PRs rare without a *task CTA* | Content without a claimable issue; wrong audience (general AI hype ≠ agent-memory engineers) |
| **5** | **HN / Lobsters / Reddit launch** | Heavy selection bias; many fail | Typical small-tool Show HN: spikes of traffic, few sustained PRs; often 0 long-term contributors. Failure is default | “Show HN” without a runnable demo; no issue to land on after the spike |
| **6** | **Awesome-list / goodfirstissue.dev / up-for-grabs inclusion** | Helps *already-labelled* projects that meet list criteria; DeepSource GFI site explicitly targets popular projects | For 9-star repos: often **rejected or ignored**; if listed, traffic is thin and tourist-heavy | Lists require popularity thresholds or human curation; tourist contributors abandon |
| **7** | **Conference / meetup talk** | Works when room is practitioners with the problem | 1 talk → 0–3 serious follow-ups typical for niche topics (**inference**) | Academic-only room; no install path that night |
| **8** | **GFI labels alone** | Strong conversion surface **after** discovery; near-zero as discovery for unknowns | On zero traffic: **≈0** | Decorates empty store |
| **9** | **Bounties at tiny scale** | Often attract mercenary one-shots; poor retention in practice | May buy a first PR; rarely buys second | Wrong incentives; reputation risk |

**Your 37 upstream PRs / 0 merges** is itself evidence of cold-PR failure modes documented in community practice: silent queues, mismatched issues, recipe-spam perception. That path currently **fails for you**, not because upstream is impossible, but because the current pattern is not producing merges (and merge is the credential).

---

## 4. Retention: what brings someone back

Separate levers from first contribution.

### Response latency

| Threshold | Evidence |
|---|---|
| **~48h** first response to newcomer contact | Jensen et al. (classic); still cited as associated with continued participation |
| **~8.5h** median first *expert* comment after newcomer comments on a GFI | Tan et al. 2023 (popular projects with capacity) |
| Week+ silence | Consistently cited as demotivating in barrier studies; CHAOSS treats issue/PR response time as a **health** metric, not a magic number |

Is 48h *meaningfully* better than a week? **Directionally yes** (association studies); **exact dose-response is not precisely known**. For a 4 h/week maintainer, the operable rule is: **do not accept claims you cannot answer within a few days**; batch review is fine if the SLA is published.

### First-PR experience

Hao et al. 2026: **interaction-related** features of the first issue (moderate pre-implementation discussion, project members participating, technically clear rather than purely “positive” cheer) associate more with retention than intrinsic issue text. Causal analysis: moderate discussion intensity helps; **excessive emotional positivity can hurt** (newcomers need technical signal). Reporter expertise is non-linear — issues filed only by ultra-experts can be less accessible.

Zhou/Bao-style “early behaviour predicts LTC” literature: first successful contribution does **not** guarantee retention; many leave after one patch.

### Credit mechanics

| Mechanism | Evidence strength |
|---|---|
| `CONTRIBUTORS.md` / all-contributors bot | **Weak/anecdotal** for retention; good hygiene; no strong RCT that it increases return PRs |
| Release notes co-credit | **Anecdotal**, plausible for identity |
| Path to commit rights / governance | Classical onion model (Nakakoji et al.); **thin measurement** that *advertising* commit-rights path increases retention at micro scale |
| Co-authorship on papers / “research collaborator” framing | Possibly more relevant for an *AI research lab* than for a CLI utility — **inference**, not measured |

### Governance signal

For a 9-star personal account, “you can become a committer after 3 merged PRs” is a **credible future** only if there is a second human or an org account. Offering it early can help the *right* person; it will not create traffic. **Measured effect at your scale: unmeasured.**

---

## 5. No-CLA / “you keep copyright” bet

**Verdict: plausible friction reduction; effect size for micro-projects is largely unmeasured; do not treat it as a growth lever.**

What exists:

- Industry consensus (Chef, GitLab public moves CLA→DCO): CLAs add **process friction**; DCO is lighter; **no CLA** is lightest for pure MIT inbound=outbound.
- Arguments that CLAs depress contributions are **mostly qualitative** (bureaucracy, corporate legal review for employees).
- **No clean natural experiment** with published contribution-rate before/after for small personal MIT repos was found in this pass.
- Employee contributors often cannot sign personal CLAs easily — that friction matters more for *corporate* contributor pools than for hobbyists discovering a 9-star tool.

**For your lab:** no-CLA is **correct default hygiene** under MIT and reduces one barrier. It is **unlikely** to be why a stranger discovers you or makes a first PR. It may matter *later* if a company engineer wants to contribute and their legal team freezes on a custom CLA.

**Do not market no-CLA as the differentiator that will get you to 10 contributors.** Evidence does not support that magnitude.

---

## 6. AI-agent dimension (2026, skeptical)

### (a) How maintainers treat AI-assisted human PRs

Spectrum of real policies (not folk wisdom):

| Stance | Example | Implication |
|---|---|---|
| **Allow listed tools + mandatory disclosure + human understanding** | Open edX AI policy: allow major vendors; require disclosure; contributor must explain code; reviewers may close “AI loop” PRs | Disclosure is **expected hygiene**, not a brand asset |
| **Quality-only, tool-neutral** | ImageMagick discussion stance: care about correctness; humans still review | Disclosure optional noise |
| **De facto hostility to unreviewed AI volume** | Popular projects reducing GFIs / anti-spam after AI-era load (Hoshikawa discussion of yt-dlp/godot patterns) | Undisclosed bulk AI PRs are a **liability** |

**2026 reality:** AI-authored PRs on GitHub are a mass phenomenon (order-of-magnitude “hundreds of thousands to ~1M” scale claims in recent SE work — treat counts as moving). Maintainers care about **review cost**, not philosophy. Disclosure of AI assistance is:

- **Asset** only if paired with demonstrated understanding and small, correct diffs  
- **Liability** if the PR is large, untested, or the author cannot defend it  
- **Noise** when everyone already assumes Copilot/Claude is in the loop  

For *your* project: stating “we build with agents” is fine; **shipping agent-generated unreviewed dumps into your own `accepted` queue** will train the wrong contributor type.

### (b) Contributor pool for agent tooling

**Inference with moderate support:** Likely contributors are:

1. People running multi-agent / Claude Code / local agent stacks who hit memory, citation, or consensus bugs  
2. Researchers who need a citation-gate or integrity benchmark  
3. Not “first-timers hunting GFI t-shirts”

That changes what a good `accepted` issue looks like:

| Traditional GFI | Better for agent-lab tools |
|---|---|
| Typo in README | “Reproduction script fails under SQLite WAL on Mac; expected X” |
| Add badge | “False positive on citation gate for DOI form Y — fixture + failing test included” |
| Easy refactor | “Benchmark case for agent runtime integrity — golden file + score delta” |

Issues should be **agent-solvable but human-verifiable**: deterministic tests, fixtures, explicit acceptance criteria. Agent-using contributors will try to one-shot the PR; your gate should be CI + “explain the fix in 5 lines.”

### (c) Letting AI agents contribute directly

| Approach | Assessment |
|---|---|
| Autonomous agents opening PRs as first-class contributors with no human responsible | **Reputational and quality trap** for a tiny lab right now; peers will treat the project as a spam source |
| Humans using agents, disclosed, with tests | Normal 2026 practice |
| Agent-assisted maintainer triage (you use agents to draft reviews; you ship the review) | Defensible if human owns the merge button |
| “Agents welcome; bot account + signed commits + CI required + human co-author” | Experiment only with strict volume caps; no evidence this grows *recurring human* community |

**Defensible middle:** publish an **AI contribution policy** modelled on Open edX principles (disclosure, understanding, no review-loop abuse) without their CLA baggage. Do **not** advertise “agents may PR freely.”

---

## 7. Anti-patterns with receipts

| Anti-pattern | Receipt |
|---|---|
| **Hacktoberfest-as-growth** | 2020 spam crisis: ~483k event PRs; ~4.8% labelled spam (undercount of low-quality non-spam); maintainers flooded; DO later tightened rules. Quality events still skew tourist. |
| **Over-labelling GFI** | 73% of GFIs never get newcomer PRs even on popular projects; merge rates for newcomer GFI PRs falling (62%→42%); some large projects *reduced* GFI supply post-2024 |
| **“We welcome contributions!” + empty/hard issues** | Barrier literature: false invitation → distrust; Steinmacher social barriers |
| **Begging for stars / vanity contributor counts** | No evidence stars cause contributors at micro scale; your traffic already 100% self-referred — star campaigns won’t fix discovery of *engineers who need the tool* |
| **Cold PR spray to 26 repos** | Your own metric: 37 PRs, 0 merges. Matches “silent queue / recipe spam” failure mode |
| **Bounties at tiny scale** | Anecdotal: one-shot mercenaries, little retention; can attract the wrong skill/motivation mix |
| **Docs-only CONTRIBUTING without runnable path** | Fronchetti et al. (FSE 2023 line): CONTRIBUTING files often fail to convey actual barriers — docs theatre |

---

# Ranked action list (10–15 tactics)

Hours = **human** hours/week to sustain. Agents can amplify writing/tests but not replace social trust.

| # | Tactic | Expected effect | Evidence | Human h/wk | Time-to-signal | Failure mode |
|---|---|---|---|---|---|---|
| 1 | **Stop cold-PR volume; switch to issue-matched, maintainable-size PRs only where a maintainer is alive** | Reputation repair; eventual merge credentials | Your 0/37; issue-match practice | 2–3 | 2–8 weeks per merge | Still silent maintainers; sunk cost |
| 2 | **Personal invites: 5–10 people who already use agents + felt the pain; link one claimable issue each** | Highest odds of first outside PR | Onboarding practice + itch model | 1–2 | 1–4 weeks | Wrong invites; no time-bound ask |
| 3 | **Publish one reverse-dependency path** (PyPI + “install and run in 5 min” + one real consumer repo you control or a partner) | Turns tool into something people *must* touch | Dependency→contributor classic | 2 (burst) | 4–12 weeks | Nobody depends on lab notebooks |
| 4 | **SLA: first human reply <48h on any external issue/PR; publish it** | Retention if anyone arrives | Jensen; Tan 8.5h median on busy projects | 0.5 on-call | Immediate process | SLA broken → trust destroyed |
| 5 | **Keep `accepted` queue but cap at 5–8 tightly scoped, CI-backed issues; retire stale claims at day 7** | Conversion surface when traffic exists | GFI convert literature; claim abandon risk | 0.5 | N/A until traffic | Over-scope issues; queue rot |
| 6 | **Borrowed audience with task CTA** (one technical post/demo on a channel that is *not* your FB wall; end with 1 issue link) | Discovery | Practice; your FB-only referrer problem | 1–2 | 1–3 weeks per hit | Hype post, no task; wrong room |
| 7 | **Ship “fixture pack” issues** (failing test + data + expected output) aimed at agent-using engineers | Matches pool; reduces mentorship cost | Inference from agent-era PR quality problems | 1 | 2–6 weeks | Fixtures too lab-specific |
| 8 | **Merge path for docs/tests from outsiders in <72h with substantive technical review tone** | First-PR experience → retention | Hao interaction > cheer; first-patch literature | 0.5 | When PR exists | Cheerful LGTM without guidance |
| 9 | **AI policy (disclose, understand, human owns merge); apply to yourself publicly** | Sets norms; reduces AI-slop inbound | Open edX-class policies | 0.25 once | — | Policy theatre without CI |
| 10 | **One Lobsters/HN launch only after install path + 3 perfect `accepted` issues + demo GIF** | Spike traffic | Launch culture; high fail rate | 1 prep | Days | Launch with nothing to do |
| 11 | **Awesome-list PRs only where acceptance bar is met; prefer niche agent lists over generic** | Thin discovery | Weak | 0.25 | Weeks–never | Rejected; tourist traffic |
| 12 | **all-contributors + release credit** after first external merge | Retention hygiene | Weak | 0.1 | After first merge | Empty CONTRIB before anyone exists (looks hollow) |
| 13 | **Do not run Hacktoberfest / bounties / star-begging** | Avoids negative selection | HF 2020 data; vanity metrics | 0 | — | FOMO |
| 14 | **Track leading metrics weekly (below); kill tactics with 0 signal by day 45** | Learning system | Product discipline | 0.25 | Continuous | Vanity dashboards |
| 15 | **Optional: invite 1 recurring co-maintainer (human) for review coverage** | Unblocks SLA + governance signal | Onion model; micro-team practice | 1 | 4–12 weeks | Wrong co-maintainer |

---

# 90-day sequence (~4 human-hours/week + agents)

### Weeks 1–2 — Stop bleeding, instrument, pick one flagship

- Freeze spray of low-signal upstream PRs; triage the 33 open ones (close noise, nurture any with maintainer replies).
- Instrument: unique external visitors, referrers, clones, issue views, time-to-first-response, claim events (even if 0).
- Flagship = one repo only (highest “other people would hit this bug” score — likely citation gate or memory layer, not the full second-brain kit).
- Rewrite 4–6 `accepted` issues as **fixture + failing test + claim protocol**; delete decorative labels.
- Draft AI contribution policy (10–15 lines).
- **Stop:** Facebook-only promotion as growth channel; star-asking; adding more GFIs for show.

### Weeks 3–6 — Demand creation, not queue decoration

- 8–12 personal invites (specific people, specific issue, offer 30-min pairing via async comments).
- One borrowed-audience artifact (technical write-up or demo) with CTA to one issue.
- PyPI/install path + “5-minute reproduce” for flagship.
- Maintain &lt;48h reply SLA (agents draft; human sends).
- **Kill signal:** 0 external issue comments and 0 non-self clones by end of week 6 → invitations were wrong or product isn’t legible; rework README demo before more outreach.

### Weeks 7–10 — Convert first strangers; over-invest in first PR

- If any claim/PR: mentor heavily (Tan: mentoring drives first success); merge small wins fast; credit in release notes.
- One Show HN/Lobsters **only if** install path is clean and 3 issues are truly claimable.
- Attempt 1–2 *high-quality* upstream contributions into a repo that is a natural reverse-dep or peer (not 20 repos).
- **Kill signal:** traffic spike with 0 issue engagement → CTA/issues still wrong; do not launch again until fixed.

### Weeks 11–12 — Retention or honest park

- If ≥1 external contributor: second issue ready before they leave; invite them to suggest the next bug; consider all-contributors.
- If 0 external contributors: **park growth theatre**; keep lab tools for internal use; re-enter growth only when either (a) a reverse dependency exists or (b) one real user outside the lab appears organically.
- Write a 1-page postmortem for the vault: what moved leading metrics.

### What to stop if no signal

| If by day… | …and you have | Stop |
|---|---|---|
| 30 | 0 non-self referrers | More self-posts without new channel |
| 45 | 0 external issue interactions | Expanding GFI count; more labels |
| 60 | 0 external PRs despite invites | Generic “OSS community building”; return to product/dependency strategy |
| 90 | 0 recurring contributor | Treat “10–50 contributors” as **not the 90-day goal**; success = 1 external human who returned once |

---

# Metrics (instrument now)

### Leading (weekly)

| Metric | Failure threshold (90 days) |
|---|---|
| Unique visitors from **non-self** referrers | Still ~0 external referrers |
| Clones / unique cloners | Flat at noise |
| Issue page views on `accepted` | ~0 |
| External comments / claims | 0 claims |
| Median time-to-first-response on external threads | &gt;7 days when threads exist |
| Upstream: maintainer replies on *your* PRs | Still 0 productive threads |

### Lagging

| Metric | Failure | Weak success | Strong success (ambitious for 90d) |
|---|---|---|---|
| External contributors with ≥1 merged PR | 0 | 1 | 3+ |
| Contributors with ≥2 PRs (retention) | 0 | 1 | 2+ |
| Merged upstream PRs (credential) | 0 | 1 | 3+ |
| Reverse dependents (other GitHub repos / PyPI downloaders who open issues) | 0 | 1 | small graph |

**Do not use as success:** star count, follower count, “number of labelled issues,” Hacktoberfest PR counts, “we posted N times.”

---

# What the evidence does **not** support

Required section — widely recommended, thin or absent evidence for *your* situation:

1. **“Label good first issues and contributors will come”** — Supported as conversion aid on *visible* projects; **not** as discovery for ~9 visits/week repos.  
2. **“No CLA will attract contributors”** — Unmeasured effect size at micro scale; hygiene ≠ growth.  
3. **“CONTRIBUTING.md + Code of Conduct → community”** — Hygiene; no evidence they create first strangers for unknown tools.  
4. **“all-contributors bot increases retention”** — Plausible recognition theory; **no strong causal evidence**.  
5. **“48h is the magic number vs 72h or 5 days”** — Direction (faster better) is supported; precise threshold is not.  
6. **“Awesome lists are a reliable cold-start”** — Mostly for projects that already look alive.  
7. **“HN launch produces recurring contributors”** — Produces attention spikes; recurring contributors are rare without product pull.  
8. **“Bounties bootstrap community”** at two-person scale — Thin positive evidence; known negative selection.  
9. **“AI disclosure is a unique marketing asset”** — In 2026 it’s table stakes or noise; quality and review cost dominate.  
10. **“Offering commit bits early multiplies retention”** for 0-contributor projects — Onion-model folklore; unmeasured at your scale.  
11. **“37 upstream PRs will eventually convert if you just wait”** — Your data already falsifies passive hope; silent open PRs are not a pipeline.  
12. **“Documentation-only first contributions build a core team”** — They can onboard *someone*; they rarely create maintainers of agent infrastructure.

---

## Bottom line for this lab

**First contribution** to an unknown repo is almost never caused by a beautiful issue tracker. It is caused by **(1) a person who already needed the tool**, **(2) a human who asked them to fix a concrete thing**, or **(3) reputation earned by merged work elsewhere**. Your `accepted` queue + claim protocol + no-CLA are the right *landing pad*; they are not the *airport*.

**Retention** is mostly **fast, technical, non-empty interaction on the first PR** — not badges, not CLA stance, not star counts.

**Highest-ROI 90-day bet under 4 h/week:**  
personal invites + one legible install path + SLA + kill cold-PR spray + instrument discovery.  
**Lowest-ROI:** more labels, more Facebook, more unsolicited upstream volume, Hacktoberfest, CLA marketing.

**Confidence:** moderate on barrier/GFI/retention literature (large-project samples); low-to-moderate on cold-start conversion rates for 9-star agent tools (evidence is thin by nature — survivors write the blog posts).
