# DR26-07-17-HUB-01 — raw report from publ

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# World-Class Public Roadmaps for Build in Public Projects

## Executive judgment

For a two-person human+AI team shipping in public, the most credible public roadmap is **not** a feature calendar. It is a **hybrid document**: a short north-star section, a **Now / Next / Later** stack with **epic-level items only**, an explicit **Changed our mind** log, and a **dated ship log** where every shipped item links to artifacts such as commits, releases, demos, dev-logs, or book chapters. That design matches how high-trust teams actually communicate when priorities are volatile: direction stays visible, near-term work is concrete, and shipped proof closes the loop. [established] citeturn11view0turn12view0turn16view0turn20view0turn18view1turn37view0turn27view0turn33view1turn34view0

The strongest negative pattern in the sample is also clear: **trust breaks when the roadmap silently stops matching reality**. That happened in several different forms—removed GitHub items that angered followers until GitHub added explanatory comments, Epic’s stale Unreal/Fab board being called “not a roadmap,” GitLab removing direction pages because they were manually maintained, low-traffic, and often outdated, and Canny users complaining that hiding “Complete” made it look as if nothing ever shipped. [established] citeturn36view0turn22view1turn25view0turn29view4

There is **very little direct empirical research** comparing public roadmap formats or measuring customer trust outcomes. The academic literature is mostly on roadmapping under uncertainty, not on public SaaS/OSS roadmaps specifically. The best-supported general conclusion from the research is that roadmaps are primarily **strategic communication tools**, and volatile environments require more explicit maintenance discipline and “a new type of agility,” not more precision theater. [established] citeturn33view1turn33view3

## Case studies

The table below covers **ten named, concrete public roadmap systems or closely related public planning surfaces**. I included a few cautionary near-neighbors because your decision depends as much on **what failed publicly** as on what worked.

| Case | Primary public URL | Structure | Granularity | Update cadence | How DONE is shown | How slips / changes are handled | What followers praise or mock |
|---|---|---|---|---|---|---|---|
| **GitHub Public Roadmap** | `https://github.com/github/roadmap` | Quarter/time-horizon board plus a detailed issue for each item; issues carry release-phase and product labels. | Epic/feature level, not tasks. | GitHub says it will keep the roadmap updated regularly and pair it with quarterly webinars; the 2024 refresh restructured the board around a 12‑month view. | README says shipped items get a `shipped` label and are closed with a comment linking the changelog. | During the Nov. 26, 2024 refresh GitHub explicitly acknowledged removed items, said some may resurface, and added context to removed issues. | Praise: enterprise/product users like the visibility and issue-level detail. Mock/backlash: users reacted sharply when items disappeared or priorities shifted without enough prior context. [single-source for some row details] citeturn11view0turn11view1turn36view0turn36view1turn35search13 |
| **Zed Roadmap** | `https://zed.dev/roadmap` | Hybrid board: “Currently Working On,” “Coming Up Next,” plus triage / planned / in-progress lists sourced from GitHub issues. | High-level features and contributor-worthy initiatives. | [no source found] for a formal cadence. In practice, Zed ships very frequently; its 2024 recap says it shipped 107 stable releases and 191 preview releases in 2024. | The roadmap page itself surfaces some items as “Done,” and releases/yearly recaps provide the shipped proof layer. | No formal slip policy is surfaced on the roadmap page. | Praise: Zed’s transparency and open-source posture get explicit praise on HN. Mock: users have questioned mismatch between roadmap themes and release stream, especially around AI emphasis versus core-editor work. [single-source for cadence] citeturn12view0turn12view1turn12view2turn13search0turn13search11turn13search7 |
| **Tauri 2.0 roadmap** | `https://v2.tauri.app/blog/roadmap-to-tauri-2-0/` | “Painted picture” / milestone roadmap: Beta → RC → Stable, with explanatory milestone gates rather than a large item board. | Major milestones and platform-level capabilities. | Event-driven milestone updates: roadmap post, beta post, RC, stable release. | Beta, RC, and stable release posts act as the completion record. | Tauri explicitly said in Sept. 2023 that it did **not** have hard timelines, only rough targets, because it wanted flexibility for audits and community feedback; by Oct. 2, 2024, stable shipped. Tauri maintainers also said elsewhere they removed target dates from some public roadmap surfaces because keeping them up to date was too much hassle. | Praise: clarity around milestone gates and explicit uncertainty. Critique: rough target of “early 2024” slipped to Oct. 2, 2024, which vindicates the no-hard-dates stance. [single-source for maintainer comment] citeturn16view0turn16view2turn38view3turn38view0turn17search2 |
| **Plausible Analytics roadmap** | `https://feedback.plausible.io/roadmap` | Public feedback portal / roadmap board with “In progress” and a **month-grouped completed archive**. | Feature-request / problem-solution level. | Continuous/as-status-changes [single-source]. | Completed items are listed by month directly on the roadmap page. Plausible blog posts and recaps also tie shipped work back to requested items. | No formal slip policy is visible. | Praise: very legible “done by month” signals; founder explicitly said public roadmap + public forum guide prioritization, with about 80% of prioritization coming straight from users. Risk: voting-led boards can drift toward incrementalism unless curated by vision. [single-source for prioritization percentage] citeturn20view0turn20view1turn20view2turn20view4 |
| **Trigger.dev roadmap** | `https://feedback.trigger.dev/roadmap` and `https://trigger.dev/changelog` | Public feature board plus a rich changelog. | Feature/problem level. | Continuous board; frequent changelog updates. The public board shows planned and in-progress items with user counts and status. | Changelog is detailed and frequent; release notes include contributors and upgrade instructions. | No formal public slip policy found. | Praise: engineers tend to like the tight loop between requests and concrete release notes. Risk: classic public-board hazard—older planned items can linger for a long time unless actively pruned. [single-source for praise inference] citeturn18view0turn18view1turn18view2turn18view3turn18view4 |
| **Canny public roadmap** | `https://feedback.canny.io/` and help docs at `https://help.canny.io/en/articles/3828148-public-roadmap` | Status-column board: Under Review / Planned / In Progress by default once changelog exists; Canny explicitly separates roadmap for future from changelog for past. | Idea / feature-request level. | Triggered by status changes; help docs say public roadmap is sorted by when status last changed. | After a changelog is published, Canny intentionally removes “Complete” from the roadmap and expects completed work to live in changelog. | This is a clean operating rule, but it has confused users, who argued that removing “Complete” made it look as if nothing was ever finished. | Praise: very clear status vocabulary and a complete feedback → roadmap → changelog loop. Mock: some users dislike the deliberate split because “roadmap without complete” feels like endless planning. [single-source for user reaction] citeturn29view0turn29view1turn29view2turn29view4 |
| **Atlassian Cloud Roadmap** | `https://www.atlassian.com/roadmap/cloud` | Large public board with filters for category, status, time, and apps; items carry “Coming soon” or “Released” and quarter labels. | Mid-size feature / initiative cards. | Explicitly last updated May 8, 2026; users can subscribe for **quarterly** email updates highlighting newly released features and updated time frames. | “Released” items remain on the same board with quarter labels. | Atlassian uses a strong non-commitment disclaimer: informational only, not a commitment to deliver, subject to change. | Praise: enterprise buyers like the breadth, filters, and released/coming-soon split. Risk: quarter labeling works at Atlassian’s scale but would create false precision for a tiny volatile team. citeturn37view0turn37view1turn37view2 |
| **Atlassian Data Center Roadmap** | `https://www.atlassian.com/roadmap/data-center` | Same model as Cloud, but for self-managed/Data Center products. | Mid-size feature / initiative cards. | Last updated May 8, 2026; quarterly email subscription for newly released features and updated time frames. | “Released” items stay visible on the board with shipped quarter. | Same informational/not-a-commitment disclaimer model. | Praise: same strengths as Cloud; it is particularly strong for environments where customers genuinely need quarter-level planning. citeturn21view1turn37view0 |
| **GitLab public issue-tracker roadmap model** | Public issues and blog: `https://about.gitlab.com/blog/best-practices-customer-feature-request/`; cautionary removal issue: `https://gitlab.com/gitlab-com/www-gitlab-com/-/work_items/35398` | Not a single pretty board anymore; public feature planning lives through public issues, labels, milestones, and “What’s New / What’s Coming” marketing surfaces. | Issue / feature proposal / milestone level. | Public issues are continuous; GitLab’s 2025 Healthy Backlog Initiative says teams conduct **weekly assessments** of backlog alignment. | Done is shown through monthly releases / “What’s New” surfaces and closed issues. | GitLab is the strongest cautionary example in this sample: it deprecated direction pages because they were inconsistently maintained, low-viewership, confusing, and frequently outdated. | Praise: extreme transparency, direct customer-to-issue loop. Mock/backfire: manual public strategy pages decayed badly enough that GitLab removed them. citeturn25view1turn25view0turn25view2turn24search6 |
| **Supabase changelog-first public shipping model** | Changelog and launch-week surfaces such as `https://supabase.com/changelog/29239-dashboard-weekly-updates-02-09-24-09-09-24` and `https://supabase.com/blog/tags/launch-week` | Not a classic roadmap board. It is a **changelog-first**, heavily narrative shipping surface with weekly dashboard updates and concentrated launch-week releases. | Small feature / release-note level publicly; bigger themes in launch-week posts and retros. | Weekly dashboard updates are explicit; launch weeks provide concentrated periodic narrative bursts. | Exceptionally strong ship proof: weekly updates include concrete changes, and some entries link directly to PRs and product pages. | Because it does not foreground a forward-looking board, it largely avoids public-slip pain. | Praise: outstanding proof-of-shipping and content flywheel. Weakness: by itself, it answers “what shipped” better than “where are we going?” citeturn28search3turn28search9turn28search12turn28search8turn28search16 |

### What the sample says

Across the best-regarded examples, **public items are almost never task-level**. They are features, themes, requests, milestones, or problems-to-solve. That matches the academic literature: software product roadmaps are mainly strategic communication tools, and companies in volatile environments need explicit maintenance mechanisms, not more micro-planning. [established] citeturn11view0turn12view0turn16view0turn20view0turn37view0turn33view1

The second consistent pattern is that **done needs a separate proof surface**. GitHub closes roadmap issues with changelog links; Canny explicitly relocates “Complete” into changelog; Trigger.dev, Supabase, and Linear all use changelog-like systems to prove motion; Plausible keeps completed work grouped by month. When a roadmap has no clearly visible done mechanism, followers start questioning whether anything ever ships. [established] citeturn11view0turn29view0turn29view4turn18view2turn28search3turn20view0turn27view3

## Structure comparison

### What the evidence actually supports

A quarter-based roadmap is **not inherently bad**. Atlassian uses it successfully because its customers really do need quarter-level planning, and Atlassian protects itself with an explicit “not a commitment” disclaimer plus frequent updates and a visible released column. GitHub also uses time buckets because its surface area and customer base justify that amount of forecast structure. [established] citeturn37view0turn37view1turn11view0

For a **small team with volatile priorities**, though, quarter/date-heavy public boards are usually the wrong primary surface. Tauri is the clearest proof point: it publicly avoided hard timelines, slipped from a rough “early 2024” stable target to an Oct. 2, 2024 stable release, and later explicitly said it had removed target dates from some public roadmap surfaces because maintaining them was too much hassle. GitLab’s old direction pages likewise decayed because manual public planning artifacts did not stay current. [established] citeturn16view0turn38view3turn38view0turn17search2turn25view0

The academic literature points the same way, though at a higher level: roadmapping in volatile environments is about **future direction and strategic decision-making**, and organizations often lack explicit maintenance mechanisms. That is exactly why dated, feature-level public plans break so easily—they convert a strategic artifact into an implied delivery contract. [established] citeturn33view1turn33view3

### Head-to-head

| Format | Best use | Trust upside | Trust failure mode | Fit for your case |
|---|---|---|---|---|
| **Now / Next / Later** | Teams with uncertain priorities and short feedback loops. | Signals direction without calendar promises; tolerates reprioritization better than quarters. Teresa Torres’ current synthesis goes further: **Now = solutions**, **Next = opportunities**, **Later = outcomes**, which naturally maps certainty to time. [single-source] citeturn34view0 | If “Now” never ships, the whole board looks fake. If “Later” becomes a dumping ground, it becomes a graveyard. | **Best default.** |
| **Quarter / half / year horizons** | Enterprise planning, regulated work, procurement-heavy buyers, large portfolios. | Useful when customers must plan around upcoming changes. | Creates date pressure and false commitments; followers remember slipped quarters. | **Only as a secondary layer**, not the main public artifact. |
| **Kanban status board** | Feedback portals and products where users want to vote and watch state change. | Easy to understand; makes movement legible. | “In Progress” zombies destroy trust faster than broad horizons do. | Good if actively pruned and paired with a changelog. |
| **Single changelog-first page** | Teams that want to maximize proof-of-shipping with minimal promise exposure. | Best evidence of actual delivery. | Weak on narrative clarity and future direction. | Excellent supporting layer, weak as the only roadmap. |
| **North star + milestones / painted picture** | Major version transitions, platform bets, architecture arcs, books/research programs. | Communicates destination and why it matters; good for community alignment. | Too abstract if not connected to near-term work. | Excellent wrapper around Now / Next / Later. |

This makes the practical answer fairly simple: **the trust-optimal format for your use case is a hybrid**: **North star → Now / Next / Later → Changed our mind → Ship log**. [emerging] It inherits the best properties of Zed, Tauri, Plausible, Trigger.dev, GitHub, and Supabase without importing the big-company liabilities of quarter-based boards. citeturn12view0turn16view0turn20view0turn18view1turn11view0turn28search3turn34view0

### Credible granularity

Public roadmaps should stay at **epic / theme / problem / milestone level**, not task level. GitHub uses issue-level features with labels; Zed highlights platform/features and contributor-worthy initiatives; Atlassian uses feature cards; Tauri used milestone gates; Plausible and Trigger use request/feature level. The literature also treats roadmaps as strategic direction artifacts, not project-task breakdowns. [established] citeturn11view0turn12view0turn16view0turn20view0turn18view1turn37view0turn33view1

For a two-person public build-in-public stack, the public reader should see **10–20 meaningful items total**, not 70 microtasks. Anything more granular than “deliver consensus replay visualizer,” “publish chapter on multi-agent arbitration,” or “add remote-worker orchestration on second machine” will feel like either noise or theater. That last sentence is a synthesis, not a sourced fact. [emerging] citeturn33view1turn34view0

## Trust mechanics

### What reliably builds trust

A **dated ship log with artifact links** is the most consistently visible trust builder in the sample. GitHub closes shipped issues with a changelog link; Supabase runs weekly dashboard updates and launch weeks; Trigger.dev’s release notes are concrete enough to upgrade from; Plausible groups completed items by month; Canny’s whole model assumes a roadmap for future and changelog for past; Linear’s release pipeline changelogs exist specifically to make shipping progress easy to review and share. [established] citeturn11view0turn28search3turn28search9turn18view2turn20view0turn29view0turn27view3

A second strong mechanic is **explanatory change logging when you remove, delay, or reshape an item**. GitHub’s 2024 roadmap refresh only became defensible because GitHub explicitly acknowledged disappointment, added contextual comments to removed issues, and promised continuing updates. GitLab’s removal of direction pages is a more extreme version of the same lesson: if a public planning surface becomes inaccurate, the least-bad option is to explain the failure and consolidate around a more maintainable artifact. [established] citeturn11view1turn36view0turn25view0

A third mechanic is **keeping roadmap items attached to real discussion objects**. GitHub uses individual issues; Zed links roadmap work to GitHub issues and community requests; Plausible and Trigger.dev use publicly votable request objects; GitLab pushes customer requests directly into public issues so the issue acts as a single source of truth. This matters because it makes the roadmap inspectable rather than decorative. [established] citeturn11view0turn12view0turn20view1turn18view1turn25view1

### Things that probably help, but evidence is thinner

**Confidence labels per item**—for example High / Medium / Low confidence—make conceptual sense for a volatile AI/OSS project, but I found very few first-class public-roadmap examples that do this explicitly. Atlassian approximates it through disclaimers and status categories; Tauri approximated it by discussing milestones without hard dates; Teresa Torres’ “Now / Next / Later” framing effectively embeds confidence into structure. Still, explicit confidence fields are mostly a **recommended extension**, not a proven norm in the sample. [speculative] citeturn37view1turn16view0turn34view0

The same is true for an explicit public **anti-roadmap** or **won’t do** section. It is strategically attractive because it prevents speculative wish-casting, but there are few strong official public examples in the 2025–2026 sample. GitLab’s Healthy Backlog Initiative is the closest thing to a public “not aligned / not active” governance mechanism. [speculative][single-source] citeturn25view2

### What breaks trust

**Overpromised dates** are the obvious killer. Atlassian tries to neutralize this by explicitly disclaiming commitment. Tauri removed target dates from one roadmap surface because keeping them up to date was too hard. Teresa Torres’ current practitioner view is blunt: feature-based roadmaps become “contracts,” and when they break, trust erodes. The academic literature on volatile environments points in the same direction, though not specifically for public SaaS boards. [emerging] citeturn37view1turn17search2turn34view0turn33view1

**Silent removals** are worse than visible delays. GitHub’s removal threads show exactly why: users called the removals disrespectful, confusing, and frustrating until the company added context. [established] citeturn36view0

**Zombie “in progress” items** also poison credibility. Canny users complained that removing “Complete” made their roadmap feel like a place where work “fades off into the ether.” That is not exactly the same as a zombie card, but it comes from the same trust hole: followers need to see that work exits the system, not just enters it. [established][single-source] citeturn29view4

**A roadmap that is really a changelog** gets mocked too. Epic’s Unreal community said the public roadmap was not a roadmap because it was published after releases, stale, and not marking progress in real time. That is a useful warning for your case: if your public page mostly updates after the fact, engineers will treat it as marketing, not navigation. [established][single-source] citeturn22view1

## Build in public narrative design

### How the best teams tie roadmap to story

The strongest build-in-public pattern is a **three-layer loop**:

1. **Roadmap layer** says what is being pursued.
2. **Artifact layer** proves actual progress.
3. **Narrative layer** explains why the work matters.

Zed does this unusually well. Its roadmap is public, contribution-oriented, and user-request-linked; the open-source announcement explicitly said the roadmap was based on user feedback and launched “Fireside Hacks,” public live sessions building Zed together with whoever showed up. That is roadmap as community programming, not just prioritization. [established] citeturn12view0turn12view1

Supabase is the clearest **content flywheel** example in the sample. It uses weekly dashboard updates for small, fast shipping proof and “Launch Week” for larger narrative arcs. That means roadmap-adjacent work becomes content, and content becomes the public memory of the roadmap. [established] citeturn28search3turn28search9turn28search12turn28search8

Plausible and Trigger.dev show the same pattern in smaller-company form. Plausible blog posts explicitly say new work came from popular roadmap requests, and monthly recaps narrate what got built. Trigger.dev pairs public feedback items with detailed release notes and engineering posts, so roadmap items do not just change status—they become explainers, migration notes, demos, and docs. [established] citeturn20view2turn20view3turn18view1turn18view2turn18view4

### What cadence works for an engineer audience

For your audience, the best evidence points to a **live board + weekly narrative summary**. Linear explicitly recommends weekly project updates and says it uses them once a week internally, often pushing them to Slack. Supabase’s dashboard weekly updates show that engineers will tolerate and even value a weekly shipping digest if it is concrete. GitHub and Atlassian use slower cadences, but they are much larger organizations with less intimate follower expectations. [emerging] citeturn27view0turn28search3turn37view0turn11view1

So the operational pattern for a build-in-public human+AI duo is likely:

- **Update the public roadmap immediately when state changes.**
- **Post a weekly or biweekly “what moved and why” note.**
- **Make every shipped roadmap item generate at least one public artifact**: a dev-log, release note, code walkthrough, benchmark, or book chapter excerpt.

That last sentence is a recommendation synthesized from the sample, not a directly sourced rule. [emerging] citeturn12view1turn20view2turn18view2turn28search3turn27view0

### For a visible human + AI duo specifically

Your setup gives you an unusual advantage: the roadmap can become the **coordination script between human judgment and AI execution**. That is close in spirit to Linear’s agent-assisted project updates, where the agent drafts status updates by reading recent issue, document, and discussion changes, while the human adds perspective before publishing. [emerging] citeturn27view1

A strong pattern for you is therefore:

- roadmap item → session log
- session log → dev-log / X thread / Telegram post
- dev-log → ship-log linkback
- ship-log → book chapter / architecture note

That is not directly documented by a single source, but it is a logical extension of the patterns visible in Zed, Supabase, Trigger.dev, Plausible, and Linear. [speculative] citeturn12view1turn28search8turn18view2turn20view2turn27view1

## Maintenance discipline

### What the low-overhead playbooks have in common

The high-trust systems in the sample **do not treat roadmap updates as a separate ceremonial job**. They attach them to natural work transitions:

- GitHub: shipped issues get closed with changelog links.  
- Canny: sorting is driven by status changes.  
- Linear: project leads post updates on a schedule, with reminders.  
- GitLab: backlog alignment is reviewed weekly.  
- Atlassian: roadmap has explicit update dates and update-timeframe subscriptions. [established] citeturn11view0turn29view0turn27view0turn25view2turn37view0

That suggests a simple principle for your team: **update the roadmap when work moves, not when you remember to do admin**. [established] citeturn29view0turn27view0turn25view2

### Practical operating rules

A low-overhead solo/small-team playbook that is well aligned with the sample looks like this:

- **One source of truth only.** GitLab’s best-practices article is explicit: the issue or feature-tracking object should be the single source of truth, and distributing the same information elsewhere creates disconnect. [established] citeturn25view1
- **Tie public state changes to internal state changes.** Canny literally sorts public items by last status change, and GitHub uses issue state/labels to drive meaning. [established] citeturn29view0turn11view0
- **Archive DONE aggressively.** Canny added “Archive Roadmaps” because old roadmap surfaces pile up and hide current relevance. [established][single-source] citeturn29view3
- **If you slip, annotate the move once, calmly, with a reason.** GitHub’s removal replies show that explanation beats silence. [established] citeturn36view0
- **Version the roadmap itself lightly.** Atlassian stamps “Last Updated”; GitHub uses issues/discussions as a visible change record. [established] citeturn37view0turn11view1

## Recommended template and ritual

### Recommended operating model

**Recommendation:** adopt a **Hybrid Trust Roadmap**:  
**North Star → How to read this → Now → Next → Later → Changed our mind → Not now → Ship log**. [emerging]

This is the best balance of trust, narrative clarity, and maintenance cost for your case because it preserves future direction without pretending to know exact delivery dates. The main supporting assumption is that your followers are **engineers who prefer evidence and honest uncertainty over polished certainty**. [emerging] citeturn12view0turn16view0turn20view0turn18view1turn27view0turn34view0

### Copy-paste public template

```md
# Project Roadmap

_Last updated: YYYY-MM-DD_

## North Star

What we are trying to make true in the world over the next 6–12 months.
Keep this to 3–6 lines.
Link to manifesto / architecture note / book landing page.

## How to read this

- NOW = work already in motion or queued to ship in ~2 weeks
- NEXT = likely next 2–6 weeks, but still movable
- LATER = important, but not committed
- Confidence = High / Medium / Low
- Every shipped item moves to Ship log with proof links
- If we change our mind, we log it below instead of deleting quietly

## NOW

### Item title
- Status: NOW
- Confidence: High
- Owner: Human / AI / Shared
- Why this matters: 1–2 lines
- Proof target: release / demo / benchmark / chapter / post
- Links: issue | branch/PR | design note | dev-log
- Last moved: YYYY-MM-DD
- Notes: optional

## NEXT

### Item title
- Status: NEXT
- Confidence: Medium
- Owner: Shared
- Why this matters:
- Decision still open:
- Links:
- Last moved:

## LATER

### Item title
- Status: LATER
- Confidence: Low
- Owner: TBD
- Why this matters:
- What would move it forward:
- Links:
- Last moved:

## Changed our mind

### YYYY-MM-DD — Item moved / removed / reframed
- Previous state: NEXT
- New state: LATER
- Why: one blunt sentence
- What we learned:
- Related link:

## Not now

Short list of things people ask for that you are explicitly not prioritizing now.
This prevents wish-casting and repetitive questions.

## Ship log

| Date | Shipped item | What changed | Proof |
|---|---|---|---|
| YYYY-MM-DD | Example item | 1 sentence | release link / PR / demo / post / chapter |
```

### Recommended per-item fields

Publicly visible fields:

- **Title**: phrase it as a problem/result, not an internal task.
- **Status**: Now / Next / Later / Shipped / Changed.
- **Confidence**: High / Medium / Low.
- **Why this matters**: one sentence.
- **Owner**: Human / AI / Shared. This is unusually valuable in your case because the duo itself is part of the story.
- **Proof target**: release, benchmark, demo, chapter, or post.
- **Links**: issue, PR, design note, public dev-log.
- **Last moved**: date.
- **Change note**: only when state changes meaningfully.

Keep internal-only fields off the public page:

- exact dates narrower than broad horizon language
- customer/account names
- speculative experiments below threshold
- dependencies that reveal fragile or embarrassing internals
- tactical task breakdowns
- financial/commercial sensitivities

This division is consistent with how public vs internal roadmaps are described by Productlane and Featurebase, though those are vendor sources and should be treated accordingly. [low-authority][single-source] citeturn31view1turn31view2

### Update ritual for your exact situation

**End-of-session micro-ritual — 3 minutes**
- Did any item change state?
- Did anything ship that needs a ship-log row?
- Did we learn something that invalidates a roadmap item?
- If yes, update immediately.

**Weekly public review — 20 minutes**
- Move at most 0–3 items.
- Publish a short “what moved this week” note.
- Add one sentence for every moved/removed item explaining why.
- Promote one shipped item into a deeper artifact: dev-log, benchmark, or book chapter note.

**Monthly hygiene — 15 minutes**
- Archive older shipped rows into a separate monthly archive page if the log gets long.
- Cull Later into “later but real” vs “not now.”
- Review whether North Star still matches actual shipping.

This cadence is the closest practical blend of Linear’s weekly update discipline, GitLab’s weekly backlog review, and the event-driven status models used by GitHub and Canny. [emerging] citeturn27view0turn25view2turn11view0turn29view0

### Five anti-patterns to never do

1. **Never publish exact dates for anything beyond work already in flight.** Tauri, Atlassian, and the broader practitioner literature all point the same way here. [established] citeturn16view0turn37view1turn34view0
2. **Never silently remove an item.** Move it to “Changed our mind” with one sentence. GitHub’s community threads show why. [established] citeturn36view0
3. **Never let “In Progress” become a museum.** If it has not moved, either explain the stall or demote it. Canny’s user complaints are the public-facing version of this failure. [established][single-source] citeturn29view4
4. **Never expose task-level clutter.** Public readers need strategic motion, not your to-do list. [established] citeturn33view1turn11view0turn12view0
5. **Never separate roadmap from proof.** Every public plan item should eventually link to a release, post, demo, benchmark, or chapter. Otherwise followers must trust words instead of evidence. [established] citeturn11view0turn18view2turn28search3turn20view0

## Decision options

### Option A

**North star + Now / Next / Later + Changed our mind + Ship log**

Trade-offs: best trust/effort ratio; clear narrative; resilient to volatile priorities; requires discipline but little tooling. Risk: if you stop updating the ship log, the whole thing weakens. [emerging] citeturn12view0turn16view0turn20view0turn34view0

### Option B

**Public Kanban / feedback board + separate changelog**

Trade-offs: strongest for interactive community voting and visible status changes; great if you want user demand visible. Risk: higher maintenance, more zombie-item risk, and more surface area for confusion about “complete” versus “announced.” [emerging] citeturn29view0turn29view4turn18view1turn20view0

### Option C

**Changelog-first page + short manifesto**

Trade-offs: lowest promise exposure and best pure shipping proof. Risk: weak answer to “where is this going,” which is one of your core stated goals. [established] citeturn28search3turn18view2turn27view3

### Final recommendation

Use **Option A** as the main public surface, and borrow one element from Option B: **every roadmap item must have a linkable object** (issue, request, design note, or dev-log), plus one element from Option C: a **hard dated ship log**.  
**Confidence:** [established]  
**Main assumption:** your audience rewards **visible evidence, blunt honesty, and short-horizon clarity** more than glossy certainty or quarter-based planning theater. citeturn12view0turn11view0turn20view0turn18view2turn28search3turn34view0

## Source list

- GitHub public roadmap README. Undated current page. URL: `https://github.com/github/roadmap` citeturn11view0
- GitHub discussion, “Re-introducing the GitHub Public Roadmap.” 2024-11-26. URL: `https://github.com/github/roadmap/discussions/1068` citeturn11view1
- GitHub discussion, “Deprecating Outdated Issues on the GitHub Public Roadmap.” 2024-11-26. URL: `https://github.com/github/roadmap/discussions/1014` citeturn36view0
- GitHub blog, “Announcing the GitHub public roadmap.” 2020-07-28. URL: `https://github.blog/news-insights/product-news/announcing-the-github-public-roadmap/` citeturn35search13
- GitHub HN discussion, “GitHub introduces sub-issues, issue types and advanced search.” 2025-01-19 comment surfaced in crawl. URL: `https://news.ycombinator.com/item?id <tg-id>` citeturn36view1
- Zed roadmap. Undated current page. URL: `https://zed.dev/roadmap` citeturn12view0
- Zed blog, “Zed is now open source.” 2024-01-24. URL: `https://zed.dev/blog/zed-is-now-open-source` citeturn12view1
- Zed 2024 recap. Current page summarizing 2024 events. URL: `https://zed.dev/2024` citeturn12view2
- Hacker News discussion referencing Zed transparency. 2024-07-10 thread. URL: `https://news.ycombinator.com/item?id <tg-id>` citeturn13search0
- Reddit, “Confused by the Roadmap versus releases.” 2026-crawled, published ~2026-02. URL: `https://www.reddit.com/r/ZedEditor/comments/1qo31l2/confused_by_the_roadmap_versus_releases/` citeturn13search11
- Tauri blog, “Roadmap to Tauri 2.0.” 2023-09-07, page last updated 2025-01-24. URL: `https://v2.tauri.app/blog/roadmap-to-tauri-2-0/` citeturn16view0
- Tauri blog, “Announcing the Tauri v2 Beta Release.” 2024-02-02. URL: `https://v2.tauri.app/blog/tauri-2-0-0-beta/` citeturn16view2
- Tauri blog, “Tauri 2.0 Stable Release.” 2024-10-02. URL: `https://v2.tauri.app/blog/tauri-20/` citeturn38view3turn38view0
- GitHub discussion answer from Tauri maintainer about removing target dates. 2022-08-25. URL: `https://github.com/orgs/tauri-apps/discussions/5041` citeturn17search2
- Plausible roadmap. Undated current page. URL: `https://feedback.plausible.io/roadmap` citeturn20view0
- Plausible podcast/blog transcript discussing public roadmap priority logic. Undated current page. URL: `https://plausible.io/blog/changelog-podcast` citeturn20view1
- Plausible blog, “How one blog post changed the traction for my startup.” Undated current page. URL: `https://plausible.io/blog/blog-post-changed-my-startup` citeturn20view2
- Plausible blog, “Building Plausible: July 2020 recap.” Undated current page. URL: `https://plausible.io/blog/july-2020-recap` citeturn20view3
- Plausible blog, “Founders: Don’t hide behind a ‘we’.” Undated current page. URL: `https://plausible.io/blog/dont-hide-behind-a-we` citeturn20view4
- Trigger.dev roadmap. Undated current page. URL: `https://feedback.trigger.dev/roadmap` citeturn18view0
- Trigger.dev feedback portal. Undated current page. URL: `https://feedback.trigger.dev/` citeturn18view1
- Trigger.dev changelog. Undated current page. URL: `https://trigger.dev/changelog` citeturn18view2
- Trigger.dev changelog, “Trigger.dev v4 is now GA.” 2025-08-18. URL: `https://trigger.dev/changelog/trigger-v4-ga` citeturn18view3
- Trigger.dev changelog, “Trigger.dev v4.5.0.” 2026-07-02. URL: `https://trigger.dev/changelog/v4-5-0` citeturn18view4
- Canny help, “Public roadmap.” 2025-10-16. URL: `https://help.canny.io/en/articles/3828148-public-roadmap` citeturn29view0
- Canny public portal. Undated current page. URL: `https://feedback.canny.io/` citeturn29view1
- Canny changelog, “Changelog official launch!” 2019-06-17. URL: `https://feedback.canny.io/changelog/changelog-official-launch` citeturn29view2
- Canny changelog, “Archive Roadmaps.” Undated current page. URL: `https://feedback.canny.io/changelog/archive-roadmaps` citeturn29view3
- Canny feature request, “Add the missing columns such as ‘Complete’ to the roadmap view.” Undated current page. URL: `https://feedback.canny.io/feature-requests/p/add-the-missing-columns-such-as-complete-to-the-roadmap-view` citeturn29view4
- Atlassian Cloud Roadmap. Last updated 2026-05-08. URL: `https://www.atlassian.com/roadmap/cloud` citeturn37view0turn37view1
- Atlassian Data Center Roadmap. Last updated 2026-05-08. URL: `https://www.atlassian.com/roadmap/data-center` citeturn21view1
- GitLab blog, “How to incorporate private customer needs into a public product roadmap.” 2021-09-23. URL: `https://about.gitlab.com/blog/best-practices-customer-feature-request/` citeturn25view1
- GitLab work item, “Deprecate direction pages.” 2026 current work item with 2026-02-01 snapshot stats. URL: `https://gitlab.com/gitlab-com/www-gitlab-com/-/work_items/35398` citeturn25view0
- GitLab blog, “Inside GitLab’s Healthy Backlog Initiative.” 2025-07-23. URL: `https://about.gitlab.com/blog/inside-gitlabs-healthy-backlog-initiative/` citeturn25view2
- GitLab “What’s new” page. Undated current page. URL: `https://about.gitlab.com/whats-new/` citeturn24search6
- Supabase changelog, “Dashboard Weekly Updates [02/09/24 - 09/09/24].” 2024-09-12. URL: `https://supabase.com/changelog/29239-dashboard-weekly-updates-02-09-24-09-09-24` citeturn28search3
- Supabase changelog, “Dashboard Weekly Updates [26/08/24 - 30/08/24].” 2024-08-30. URL: `https://supabase.com/changelog/29004-dashboard-weekly-updates-26-08-24-30-08-24` citeturn28search9
- Supabase changelog, “Dashboard Weekly Updates [11th Dec - 18th Dec].” 2023-12-18. URL: `https://supabase.com/changelog/19827-dashboard-weekly-updates-11th-dec-18th-dec` citeturn28search12
- Supabase launch-week tag page. Current page. URL: `https://supabase.com/blog/tags/launch-week` citeturn28search8
- Supabase blog, “Supabase Security Retro: 2025.” 2026-01-07. URL: `https://supabase.com/blog/supabase-security-2025-retro` citeturn28search16
- Linear changelog, “Project Updates.” 2022-08-04. URL: `https://linear.app/changelog/2022-08-04-project-updates` citeturn27view0
- Linear changelog, “Agent assisted project updates.” 2026-06-18. URL: `https://linear.app/changelog/2026-06-18-agent-assisted-project-updates` citeturn27view1
- Linear changelog, “Project update comments.” 2024-02-01. URL: `https://linear.app/changelog/2024-02-01-project-update-comments` citeturn27view2
- Linear changelog landing page with release pipeline changelogs. Current page. URL: `https://linear.app/changelog` citeturn27view3
- Linear blog / now page discussing roadmap and building in public. 2020-12-08 redirect/current page. URL: `https://linear.app/now/linear-raises-usd13m-in-series-a-funding-from-sequoia-capital` citeturn27view4
- VTT / Journal of Systems and Software record, “Software product roadmapping in a volatile business environment.” 2011. DOI on page. URL: `https://cris.vtt.fi/en/publications/software-product-roadmapping-in-a-volatile-business-environment/` citeturn33view1
- ScienceDirect abstract, “An analysis of factors improving technology roadmap credibility.” 2011. URL: `https://www.sciencedirect.com/science/article/abs/pii/S004016251100103X` citeturn33view3
- Product Talk, “Product Roadmaps: How the Best Product Teams Plan for Uncertainty.” 2026-04-08. URL: `https://www.producttalk.org/product-roadmaps/` [single-source] citeturn34view0
- Epic forum thread, “Please Epic, make the ‘roadmap’ a roadmap.” 2025-04-08 and later replies. URL: `https://forums.unrealengine.com/t/please-epic-make-the-roadmap-a-roadmap/2446366` citeturn22view1
- Productlane blog, “How to Build a Public Product Roadmap.” 2026-06-30. URL: `https://productlane.com/blog/public-product-roadmap` [low-authority][single-source] citeturn31view1
- Featurebase blog, “Public Product Roadmap: Benefits, Examples, and How...” 2026-05-14. URL: `https://www.featurebase.app/blog/public-product-roadmap` [low-authority][single-source] citeturn31view2