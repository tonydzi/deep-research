# DR26-07-08-HUB-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Single Canonical Story State for Book and Live Feed

## Executive Summary

The strongest architecture for your case is **not** a single giant “season bible” note. It is a **single canonical content model**—kept in one Obsidian vault namespace, under one editorial owner—from which the book and the live feed are **projected** as separate views. That matches how TV rooms and franchise teams actually keep continuity: the showrunner remains the final story authority, script coordinators and editors guard continuity, and multi-platform franchises use a canon-governance layer so each medium can contribute natively without rewriting the underlying story. In structured-content terms, the winning move is to model the **resource** rather than the **page** or channel artifact. In transmedia terms, the goal is a unified canon with medium-specific expression, not duplicated spines. citeturn24view9turn7search0turn30view10turn30view9turn30view11turn33view0turn33view10turn33view11

For a serialized nonfiction “reality show” about a founder building with an AI co-founder, the canonical model should be built from **typed markdown notes with YAML properties** for: the season, cast, arcs, beats, open loops, rules, motifs, and scoreboard/state. The critical design move is to store in one place both **what is locked fact** and **what is still open, planned, inferred, or forecast**—but to label those states explicitly. The book and the live feed then stop being two authoritative sources and become **read-only projections** with different reveal logic and voice. Obsidian already supports human-readable YAML properties; Bases gives you native database-like views over those notes; Dataview can query the same metadata; Astro, Hugo, Jekyll, and Eleventy all support front matter as data if you want a later build step. Git plus pre-commit hooks plus YAML linting gives you a repairable “AK-47” workflow without introducing a heavy CMS or database. citeturn24view5turn33view3turn24view6turn33view7turn30view0turn30view3turn30view1turn19search0turn33view5turn33view6turn30view4

The practical recommendation is therefore: keep one canonical folder such as `canon/`, with one schema and one editor-of-record; treat every new development as a canon update first; derive two projection views from the same notes; and never hand-edit the projection outputs. If it is not in canon, it did not happen. If it is only planned, it must not masquerade as fact. If it is fact but not yet public in the live vehicle, that is a **reveal-rule problem**, not a second-spine problem. This is the lightest structure that solves the real issue: **story-state drift**, not text-reuse drift. citeturn24view4turn30view12turn29search1turn30view14turn24view3

## Key Findings

### Serialized narrative teams centralize story authority and continuity

Professional TV writing is collaborative, but it is not anarchic. The showrunner is the final creative authority; every beat, arc, and draft passes through that role for final approval. Around that authority, writer-room support roles maintain continuity and prevent contradictions from compounding across episodes and seasons. Arc Studio’s own pitch for season bibles is revealing: changes to the season outline and character backstory are meant to “trickle down” into later notes and scripts so the system stays connected and consistent. That is the exact pattern your current two-spine setup is missing. citeturn24view9turn7search6turn30view14

Large franchises do the same thing at bigger scale. Lucasfilm explicitly formed the Story Group so future Star Wars storytelling would be “connected” and coordinated across creative development, and Marvel editors describe themselves as protectors of larger continuity who plan, coordinate, and move stories across the universe year-round. The principle is stable across scales: **one canon-governance layer, many outputs**. citeturn30view9turn30view10

### Transmedia best practice is one canon with native contributions from each medium

Henry Jenkins’ baseline definition still matters: transmedia works when integral story elements are distributed across channels to create a coordinated experience, and each medium makes its own unique contribution. That matters because it kills a common false choice. You do **not** need identical outputs to have one canon; you need one underlying truth-state plus channel-native storytelling. The book can do hindsight and payoff. The live feed can do immediacy and anticipation. They remain consistent if both are reading from the same canon and differ only in reveal window, angle, and rhetorical form. citeturn30view11turn36view0

This is also where a lot of COPE implementations go wrong. Structured-content practitioners explicitly distinguish **core content** from **channel content**. The core is reusable; the channel layer includes contextual material a specific experience needs. Kontent.ai says channel content supplements the core and that each channel will require some material unique to it. In other words: your story-state should be canonical, but your social-hook language and chapter rhetoric should not be jammed into the same field set. citeturn24view4turn11search8

### Structured content practice says model the thing, not the page

Modern content-modeling guidance is blunt on this point. Content models define the structure and organization of content; content types are the building blocks; and a content model is the total system of linked types, not a single page-shaped artifact. Sanity makes the same point from the other side: content models are about what resources exist, not what a website page looks like. That maps directly onto your issue. A “book spine” and a “live show-state” are **page-shaped outputs**. Your canon should instead model the actual narrative resources: season, cast member, arc, beat, loop, rule, motif, scoreboard item. citeturn33view0turn33view1turn33view10turn33view11

This is why a single monolithic story-state note is the wrong optimization. “One source of truth” does not mean “one file.” In content-model terms, it means **one model, one namespace, one schema, one authority**. The source can be a small set of linked note types and still be a single canonical system. citeturn33view0turn33view1turn33view10

### Markdown front matter is enough to make the canon both writable and queryable

Obsidian properties are stored as YAML at the top of the note, which is specifically designed to be easy for humans and machines to read. Bases, now a core Obsidian capability, gives database-like views over notes and their properties while keeping all data in local markdown files and `.base` YAML view files. Dataview provides the same general pattern from the plugin side: pull metadata from front matter or inline fields and query it. This means you do **not** need a separate database to get queryable story-state. citeturn24view5turn33view3turn33view4turn24view6turn33view7

If you later want stricter machine validation, static generators already know how to treat markdown as data. Jekyll, Hugo, and Eleventy all process front matter as metadata; Astro goes farther and lets you define a collection schema with validation so malformed entries fail fast. That makes Astro especially attractive if you ever want a build step that refuses to project invalid canon. citeturn30view1turn30view2turn30view3turn19search0turn30view0

### Nonfiction requires explicit fact-state, not just plot-state

Because your project is serialized nonfiction, **truth status has to be part of the model**. The Society of Professional Journalists’ code says journalists should take responsibility for accuracy, remember that neither speed nor format excuses inaccuracy, and continue to update and correct information through the life of a story. GIJN’s reporting on platform-native investigations says adapting an investigation for different platforms is real editorial work, and the open question is how much survives translation. The implication is simple: your canon cannot only track “what is narratively juicy”; it must track **what is confirmed, inferred, speculative, planned, corrected, or superseded**. citeturn29search1turn30view12

That is the key to resolving the “retrospective book vs live feed” tension in one model. The book and live outputs should disagree only on **timing, emphasis, and hindsight framing**—not on what was true when. citeturn29search1turn30view12

### The last year’s AI trend is toward checked-in project context and explicit memory files

Over the last twelve months, the most relevant AI trend has not been “bigger context windows solve everything.” It has been movement toward **checked-in, project-local context files** and deliberate memory layers. Anthropic’s Claude Code loads `CLAUDE.md` plus auto memory at session start; OpenAI Codex reads `AGENTS.md` before work; and OpenAI explicitly says required guidance should live in checked-in docs or `AGENTS.md`, with memory treated only as a helpful recall layer rather than the sole source of rules. LangChain’s long-term memory stores JSON documents by namespace and key, and a recent 2026 paper, *Infini Memory*, argues for maintainable topic documents rather than making vector or graph databases mandatory. citeturn24view7turn24view8turn30view8turn30view7turn30view6

Narrative tooling has started converging on the same pattern. Sudowrite’s Story Bible is explicitly described as a source of truth for both the writer and the AI, with connected fields such as synopsis, characters, worldbuilding, outline, scenes, and draft. Novelcrafter’s Codex pushes harder on continuity, automatic mentions, timeline tracking, and progressions so details update across a long project. An experimental open effort, the Narrative Context Protocol, is trying to make narrative context transportable across multi-agent systems as a structured schema. None of these is your final architecture, but they are good signal: the field is moving toward **agent-readable canon files**, not toward magically coherent raw chat history. citeturn32view2turn32view1turn34view0turn34view1

## Contradictions and Debates

### One canon does not mean one expression

The main debate in single-source publishing is always the same: does reuse flatten the medium? The answer is yes, if you model too much at the core. Jenkins’ transmedia rule is that each medium should make its own unique contribution. Kontent.ai makes the same distinction in content operations: core content is reusable, but channels still need supplemental channel content. GIJN’s analysis of platform-native journalism reaches the same conclusion from a nonfiction angle: translation across entry points is editorial judgment, not formatting. citeturn30view11turn24view4turn30view12

So the correct architecture is **one canonical story-state, separate rendition layers**. Do not store teaser copy, CTA logic, chapter rhetoric, or hook styles as canon. Store those in channel templates, style guides, and projection rules. The canon should answer “what is true, open, changed, and at stake?” The rendition layer should answer “how do we say that here?” citeturn24view4turn30view12turn33view11

### When two sources really are correct

There are cases where two sources are justified. First, **core canon and channel-specific presentation** are different kinds of source and should stay separate. Second, **sandbox environments** are legitimate: Contentful environments exist precisely so teams can make changes in isolation without changing the master version. Third, Jenkins’ “continuity vs multiplicity” distinction matters: if you intentionally want alternate versions, fan variants, hypothetical scenarios, or counterfactual experiments, those can exist as parallel non-canon or alt-canon branches. citeturn33view8turn36view0

But none of that argues for two authoritative spines for the same season. Contentful’s general guidance is to keep related content in one space unless there is a real reason to split. For your specific use case—one season, one cast, one season-question, one owner—the right split is **master canon vs projections**, not **book canon vs live canon**. citeturn33view2turn33view8

### The real tension is locked fact versus open possibility

Your current split looks like “past tense vs present/future,” but the deeper modeling tension is **locked fact versus unresolved possibility**. Sudowrite’s connected Story Bible fields implicitly separate ideation, synopsis, outline, scenes, and draft; Arc Studio keeps bibles and outlines connected so changes cascade rather than fork; SPJ insists that stories can and should be corrected as the facts develop. The lesson is that one model can contain both what has happened and what may happen—**if state labels are rigorous**. citeturn32view2turn30view14turn29search1

In practice, that means every canonical item needs a status such as `planned`, `in_progress`, `happened`, `canceled`, `corrected`, or `superseded`, and a truth label such as `observed`, `confirmed`, `inferred`, or `speculative`. Once you do that, “live” and “book” stop needing separate story-state files. citeturn29search1turn32view2turn24view3

## Best Practices

The pattern that best fits your constraints is a **light event-led canon with derived state views**. Think of it as “event-sourced enough to be reliable, but simple enough for one non-technical owner.” The canonical layer should be small, typed, and explicit. Every significant development gets recorded once as a beat or decision. Arcs, loops, and the current season state are summaries that point back to those beats. Book and live views are projections over the same records. This recommendation is an inference, but it is strongly supported by three adjacent bodies of practice: forward-only content migrations, connected story-bible workflows, and document-based AI memory systems. citeturn24view3turn30view5turn30view14turn30view6

A good minimum canonical model for your use case needs these entities:

```text
Season
  season_question
  premise
  tone / truth standard
  current_checkpoint
  active_arcs
  open_loops
  cast_main
  motifs
  scoreboard
  current_story_day
  live_as_of

Cast
  role_in_story
  desire
  obstacle
  relationship_map
  status
  first_appearance
  last_change

Arc
  stake
  start_state
  desired_end_state
  current_state
  beats
  status
  payoff_condition

Beat
  occurred_on
  story_day
  status
  truth_mode
  summary
  consequences
  related_cast
  related_arcs
  opens_loops
  closes_loops
  evidence_refs
  public_live_after
  book_chapter_hint

Open Loop
  question
  opened_by
  current_best_answer
  close_condition
  status
  owner

Rule / Canon Fact
  invariant
  scope
  source
  last_verified

Scoreboard
  metric name
  value
  as_of
  source_beat
```

That field set is not arbitrary. TV bibles and story-bible tools consistently revolve around premise, theme, characters, arcs, outline/beat structure, and continuity; transmedia practice adds chronology and world consistency; structured content practice pushes you toward linked, reusable types rather than one prose blob. For nonfiction, the additional mandatory fields are **truth status**, **evidence**, and **correction history**. citeturn30view14turn32view2turn32view1turn30view11turn30view15turn29search1

The most important schema rule is this: **facts, interpretations, and plans must never share the same unlabeled field**. If a beat note says “the founder nearly quit,” the canon must tell you whether that is a directly observed event, a later interpretation, a planned hook, or an emotional inference. This is what prevents live-content hype from quietly hardening into false book canon later. citeturn29search1turn30view12

The second key rule is **append first, summarize second**. New developments should land as beat notes first. Then the season note, arc notes, loop notes, and scoreboard get updated to reflect what changed. This is the same logic behind docs-as-code and CMS-as-code: keep changes versioned, explicit, and close to the source, and avoid hand-editing the downstream artifacts as if they were the source. citeturn30view4turn24view3turn30view5

The third rule is **one owner, many readers**. Showrunners, script coordinators, and continuity editors exist because consensus editing is a continuity hazard. For your vault, that means one canonical editor updates story-state, while other contributors can propose notes, draft renditions, or mark contradictions, but do not directly alter canon rules casually. In structured content systems, the same governance logic appears as keeping one core space and controlling how changes reach production. citeturn24view9turn7search0turn30view10turn33view2

The fourth rule is **forward-only migration discipline**. Contentful’s CMS-as-code guidance is explicit that down-migrations are often unrealistic and that forward-only plans with backups are safer. That is exactly how you should treat your migration off the two drifting spine files. Do not try to preserve the old pattern with clever cross-links. Move forward into canon, keep backups, and let projections replace the old spines. citeturn24view3turn30view5

## Failure Modes

The most obvious failure mode is the one you already have: **two authoritative narrative spines drifting over time**. It is the same class of failure that led Lucasfilm to formalize a Story Group for connected canon and that keeps Marvel editors working as continuity protectors across titles. Duplicated authority creates canon drift by design. citeturn30view9turn30view10

A subtler failure mode is **over-unification**. Teams often interpret “single source” as “one giant system for everything,” and then everybody steps on everybody else’s toes. Contentful’s retrospective on digital teams describes precisely that kind of monolithic setup: shared code base, shared content model, shared release cycle, and coordination pain at scale. The lesson for your vault is not to create a second canon, but also not to force book styling, live styling, workflow notes, and canon state into the same editable object. Keep one canon; isolate projections and experiments. citeturn37view0turn33view8

Another failure mode is **page-shaped modeling**. If you model “book spine” and “live state” as the main entities, the structure bakes channel assumptions into the source. Sanity explicitly warns against designing the model around pages or interfaces rather than the underlying content resources. In your case, that page-thinking is the disease. citeturn33view11turn33view10

There is also **automation entanglement**. A recent Obsidian publishing write-up is a useful cautionary tale: when automation and manual work committed into the same repository flow, the author became “almost a full-time conflict manager,” then discovered security risks when trying to make a public build pull from a private vault. That is directly relevant here. If you add a build step, keep the private canon vault and the public output pipeline separated enough that automation never becomes a second editing authority. citeturn24view0

A newer failure mode is **treating model memory as canon**. Anthropic and OpenAI are both explicit that project memories and auto-memory mechanisms are context, not enforced source-of-truth rules. OpenAI even says required team guidance should live in `AGENTS.md` or checked-in docs, not in memory alone. So if an AI assistant is part of your workflow, its memory can help retrieval, but the markdown canon remains primary. citeturn24view7turn30view8

Finally, nonfiction systems fail when they lack a **correction trail**. SPJ’s ethics guidance requires that stories be updated and corrected across their lifecycle. If your live feed makes an interpretation that later proves wrong, the system should not silently rewrite the past. It should record a correction and mark what was superseded. Without that, the book will later inherit cleaned-up false certainty. citeturn29search1

## Strategic Recommendations

The strategic recommendation is to define **one canonical story-state system** in the vault and make the book and the live feed mere consumers of it. In practice, that means a dedicated `canon/` folder, one schema version, one owner, and two projection layers: `projections/book/` and `projections/live/`. The projections may be regenerated, embedded as Base views, or updated manually from the same records, but they are never hand-maintained as independent truth stores. citeturn33view3turn33view4turn30view4turn24view3

The conceptual division should be:

```text
canon/
  season.md
  cast/
  arcs/
  beats/
  loops/
  rules/
  motifs/
  scoreboard/

projections/
  live/
  book/

styles/
  live-style.md
  book-style.md

views/
  live.base
  book.base
  open-loops.base
  scoreboard.base
```

`canon/` is truth. `styles/` is channel behavior. `projections/` is disposable output. `views/` is query surface. That division preserves one story while preventing one voice. citeturn24view4turn30view11turn33view3

For the forward-versus-backward tension, use **three axes**, all inside one record:

```text
world status      planned | in_progress | happened | canceled | corrected
truth mode        observed | confirmed | inferred | speculative
reveal policy     live_after | live_hold | book_chapter_hint | spoiler_until
```

That is enough. You do not need a dedicated “past tense canon” and “future tense canon.” A live teaser can legitimately read from a `planned + speculative` beat. A retrospective book chapter can read from the same beat after it becomes `happened + confirmed`, and can also incorporate later consequences from related beats. Same canon, different projection window. This is the cleanest way to make the two vehicles temporally different without making them ontologically different. citeturn29search1turn32view2turn30view14

I recommend **native Obsidian views first, scripts second**. Obsidian Bases already gives you table/list/card views over local markdown properties, and `.base` files are YAML. That is enough to build a “book-ready beats” table, a “live-eligible beats” table, an “open loops” board, and a “scoreboard” view that a non-technical owner can operate inside the vault. Only if you later want automatic export should you add a tiny script or Astro collection validation. citeturn33view3turn33view4turn30view0

If you do add automation, choose the least magical option. A 100–200 line script that checks required fields, generates sorted markdown projections, and fails on malformed YAML is more “AK-47” than a complex CMS-like publishing chain. Use `pre-commit` plus `yamllint`, optionally `markdownlint`, so broken canon cannot be committed casually. citeturn33view5turn33view6turn13search1

The one governance rule that should be non-negotiable is this: **canon updates precede content production**. No live post, longread, or book chapter gets drafted from memory, chat, or intuition alone if the relevant beat or state change is not yet recorded in canon. This is the exact analogue of “documentation must ship with the feature” from docs-as-code. citeturn30view4

## Action Plan

### Canonical vault schema

Start with five note templates and one dashboard note. That is enough to replace both drifting spines.

```yaml
# canon/season.md
---
type: season
season_id: "DR26-07-08-HUB-01"
title: "Founder + AI Co-Founder Season One"
season_question: "Can a non-technical founder build honestly in public without splitting into fake competence, content theater, and real execution?"
status: active
current_story_day: 41
live_as_of: 2026-07-08
current_checkpoint: "beat-2026-07-08-pricing-test-launched"
cast_main:
  - "cast-founder"
  - "cast-ai-cofounder"
active_arcs:
  - "arc-credibility"
  - "arc-first-customer"
open_loops:
  - "loop-pricing"
  - "loop-landing-page"
motifs:
  - honesty
  - public learning
  - embarrassment as proof of reality
scoreboard:
  runway_days: 74
  mrr_usd: 0
  demos_booked: 3
schema_version: 1
---
## Premise
## What is currently true
## What is open
## What changed this week
## Red lines and canon rules
```

```yaml
# canon/beats/beat-2026-07-08-pricing-test-launched.md
---
type: beat
beat_id: "beat-2026-07-08-pricing-test-launched"
story_day: 41
occurred_on: 2026-07-08
recorded_on: 2026-07-08T18:20:00+01:00
status: happened
truth_mode: observed
summary: "Pricing test launched with three offer variants on the landing page."
related_cast:
  - "cast-founder"
related_arcs:
  - "arc-first-customer"
opens_loops:
  - "loop-pricing"
closes_loops: []
consequences:
  - "Reframed tomorrow's live post from promise to experiment."
evidence_refs:
  - "notes/2026-07-08-landing-page-edit"
  - "artifacts/figma-pricing-v3"
public_live_after: 2026-07-09
book_chapter_hint: "ch-041"
spoiler_until: 2026-07-09
schema_version: 1
---
## Observed facts
## Why it matters now
## Later significance
## Related contradictions or corrections
```

```yaml
# canon/loops/loop-pricing.md
---
type: loop
loop_id: "loop-pricing"
question: "Which pricing frame creates the first credible buyer signal?"
opened_by: "beat-2026-07-08-pricing-test-launched"
status: open
current_best_answer: "Unknown"
close_condition: "One frame produces a materially stronger conversion signal or is explicitly abandoned."
owner: "cast-founder"
related_arcs:
  - "arc-first-customer"
schema_version: 1
---
## Stakes
## Current evidence
## Candidate resolutions
```

```yaml
# canon/arcs/arc-first-customer.md
---
type: arc
arc_id: "arc-first-customer"
stake: "Without a first credible customer signal, the whole season-question tilts toward theater."
start_state: "No demand proof"
desired_end_state: "First credible buyer or hard disproof"
current_state: "Experimenting with positioning and pricing"
status: active
beats:
  - "beat-2026-07-08-pricing-test-launched"
schema_version: 1
---
## Arc summary
## Current pressure
## Expected payoff
## What would count as failure
```

```yaml
# canon/rules/rule-honesty.md
---
type: rule
rule_id: "rule-honesty"
scope: season
invariant: "Never narrate skill, traction, or certainty above what evidence supports."
source: "Project editorial doctrine"
last_verified: 2026-07-08
schema_version: 1
---
## Why this rule exists
## Violations to watch for
```

These templates directly encode the tensions the research says matter: authority, continuity, chronology, arcs, open loops, and fact discipline. They also align with Obsidian’s YAML properties model and Bases’ ability to query notes as rows. citeturn24view5turn33view3turn33view4turn30view14turn32view2

### Projection logic for book and live

The **book projection** should read only beats whose `status` is `happened` or `corrected`, sorted by `story_day` and then `occurred_on`, plus the arcs and loops they affect. It may also read the `Later significance` section inside beat notes, because retrospective narrative is allowed to use hindsight—as long as the underlying facts remain the same canon. citeturn30view14turn30view11

The **live projection** should read three buckets: the most recent happened beats that have passed `public_live_after`; open loops with stakes; and planned/in-progress items that are clearly labeled as open or experimental rather than settled truth. That follows journalism’s “speed does not excuse inaccuracy” rule and GIJN’s point that platform-native adaptation is a substantive editorial task. citeturn29search1turn30view12

A simple operator rule makes this practical:

```text
Book asks:
  What actually happened?
  What changed?
  What paid off?
  What did this mean in hindsight?

Live asks:
  What just changed?
  What remains open?
  What is the next honest move?
  What can be revealed now without faking certainty?
```

Those are different questions over the same records. citeturn24view4turn30view12turn30view11

### Native Obsidian views

Before writing any script, create four Base views:

```yaml
# views/open-loops.base
filters:
  and:
    - 'type == "loop"'
    - 'status == "open"'
views:
  - type: table
    name: "Open Loops"
    order:
      - file.name
      - question
      - owner
```

```yaml
# views/live.base
filters:
  or:
    - and:
        - 'type == "beat"'
        - 'status == "happened"'
    - and:
        - 'type == "loop"'
        - 'status == "open"'
    - and:
        - 'type == "beat"'
        - 'status == "planned"'
views:
  - type: table
    name: "Live Projection"
```

```yaml
# views/book.base
filters:
  and:
    - 'type == "beat"'
    - or:
        - 'status == "happened"'
        - 'status == "corrected"'
views:
  - type: table
    name: "Book Projection"
    order:
      - story_day
      - occurred_on
```

You do not need perfect Base syntax on day one; the more important point is that Bases stores view definitions as YAML and lets a non-technical operator sort and filter native markdown notes into human-facing editorial dashboards. citeturn33view3turn33view4

### Migration from two drifting spines to one canon plus projections

Freeze both old spine files immediately. After that, use a forward-only migration:

**Phase one** is schema setup. Create `canon/`, templates, and the minimum properties above. Add `schema_version`, `status`, and `truth_mode` to every template. Add `pre-commit` and `yamllint` so malformed metadata never lands silently. citeturn24view3turn33view5turn33view6

**Phase two** is intake. Take every line or section from the old book spine and live show-state and classify it into one of six buckets: `season fact`, `cast fact`, `arc state`, `beat`, `open loop`, or `channel copy only`. Anything that is actually rhetoric, teaser language, or chapter phrasing does **not** enter canon. Anything uncertain gets a `truth_mode` weaker than `confirmed`. Anything contradicted gets flagged, not normalized. citeturn33view11turn29search1

**Phase three** is beat reconstruction. Rebuild the season chronologically as beats. This is the keystone migration move. Once beats exist, arcs, loops, season state, and scoreboards can be summarized from them. If something cannot be attached to a beat or rule, it is probably not story-state—it is commentary. citeturn30view14turn24view3

**Phase four** is projection replacement. Create `views/book.base` and `views/live.base`, plus two markdown dashboard notes that embed them. Then convert the old spine notes into read-only wrappers that simply point editors to the new projections. This is important psychologically: people stop editing the old source because it is visibly no longer the source. citeturn33view3turn31search7

**Phase five** is operating discipline. From then on, the workflow is: event happens → canon beat update → arc/loop/season refresh → draft book/live content. Never reverse that order. citeturn30view4turn24view9

### Lightweight automation if you want one

If you later want a minimal build loop, keep it tiny:

```text
scripts/
  validate_canon.py
  build_live_projection.py
  build_book_projection.py
.pre-commit-config.yaml
```

`validate_canon.py` checks required properties and valid status values. `build_live_projection.py` emits a sorted markdown brief for today’s live content. `build_book_projection.py` emits a chronological chapter spine with beat summaries and unresolved/closed loop references. If you eventually outgrow that, Astro content collections are the best next step because they enforce schemas and fail invalid entries early. citeturn33view5turn33view6turn30view0

## Sources at Deep Research

This report leaned most heavily on four source clusters.

The **narrative-governance** cluster showed how continuity is actually controlled in real serialized systems: Final Draft on showrunner authority, Scriptation and related room-role sources on continuity labor, Arc Studio on keeping bibles/outlines/scripts connected, StarWars.com on the Story Group, Marvel on editors as continuity protectors, and Henry Jenkins on transmedia as coordinated storytelling in which each medium contributes something native. citeturn24view9turn7search0turn30view14turn30view9turn30view10turn30view11turn36view0

The **structured-content and docs-as-data** cluster established the modeling side: Contentful on content models and content types, Sanity on modeling resources rather than interfaces, Kontent.ai on core content versus channel content, Write the Docs on docs-as-code, and the official docs for Jekyll, Hugo, Eleventy, and Astro on front matter and schema-based collections. Obsidian’s own docs on Properties and Bases, plus Dataview’s metadata behavior, confirmed that a markdown vault can act as a lightweight, local database without becoming a CMS. citeturn33view0turn33view1turn33view10turn33view11turn24view4turn30view4turn30view1turn30view3turn19search0turn30view0turn24view5turn33view3turn33view4turn24view6turn33view7

The **nonfiction and platform-native** cluster contributed the truth-discipline requirement: SPJ’s ethics code on accuracy, correction, and the fact that speed does not excuse inaccuracy, plus GIJN’s reporting that translating longform investigations into platform-native forms is substantive editorial work, not trivial reformatting. citeturn29search1turn30view12

The **recent AI-memory** cluster covered the last 12 months: Anthropic’s `CLAUDE.md` and auto-memory model, OpenAI’s `AGENTS.md` and memories guidance, LangChain’s long-term memory as namespaced JSON documents, the 2026 *Infini Memory* paper on maintainable topic documents, and current writing-tool patterns from Sudowrite, Novelcrafter, and the experimental Narrative Context Protocol. The highest-confidence takeaway from that cluster is not that these specific tools are standards, but that explicit, checked-in context files are quickly becoming the norm for human-plus-AI workflows. citeturn24view7turn24view8turn30view8turn30view7turn30view6turn32view2turn32view1turn34view0turn34view1

The weakest part of the public evidence base was the **build-in-public creator workflow** literature. There are concrete creator examples of Obsidian-to-Astro and custom CI/CD pipelines, including standardized front matter, symlinked content folders, git automation, and real reports of automation conflict/security pitfalls, but there is far less rigorous public documentation of founders running a fully unified “book plus live feed” canon workflow than there is for TV rooms, headless content, or AI memory systems. I have therefore treated creator-tooling evidence as supportive rather than foundational. citeturn25view3turn24view1turn24view0