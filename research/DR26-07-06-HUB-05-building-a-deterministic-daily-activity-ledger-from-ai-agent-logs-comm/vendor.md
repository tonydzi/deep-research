# DR26-07-06-HUB-05 — raw report from vendor

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Reliable Daily Ledger for AI Development Work

## Bottom-line recommendation

The best architecture is **an append-only normalized evidence store plus a deterministic daily projection**, with the final prose summary generated last from the already-fused evidence. In other words: do **not** make retrospectives, transcripts, or commit messages the source of truth. Treat every raw signal as an evidence event, project those events into a `day-ledger-YYYY-MM-DD.md`, and make the human-readable paragraph summary the only LLM-written layer. This is the same architectural move used in event sourcing and audit logging: preserve immutable events, then derive read models from them later. That pattern exists because it is replayable, debuggable, and resistant to partial-source failure. citeturn1search1turn12search1turn17search4turn4search9

A second principle matters just as much: **daily ledgering is closer to “curated changelog generation” than to “raw log dumping.”** Conventional Commits, semantic-release, Release Please, and GitHub’s generated release notes all rely on structured or semi-structured sources, but even changelog best practice still insists on a curated layer instead of dumping raw commit logs verbatim. Your day ledger should follow the same rule: deterministic evidence first, curated synthesis second. citeturn10search14turn10search0turn10search2turn10search1turn10search3

The strongest transferable lesson from modern engineering-metrics systems is also relevant here: **never infer “what happened” from one metric or one source.** The SPACE framework explicitly argues that developer productivity and activity cannot be captured by a single dimension, and engineering intelligence platforms therefore aggregate multiple systems instead of relying on Git alone. Your setup should do the same, except your sources are transcripts, ledgers, registries, vault files, commits, and posts rather than GitHub plus Jira. citeturn9search0turn9search11turn8search0turn8search4turn8search21

The architecture I recommend is this:

1. **Ingest** each source into a normalized evidence table.
2. **Fingerprint** each evidence item with a stable idempotency key.
3. **Classify** transcript sessions into real work / obvious noise / uncertain.
4. **Cluster** duplicate evidence into canonical work units.
5. **Project** work units into one calendar day in `Europe/Lisbon`.
6. **Score** each day for completeness and confidence.
7. **Generate** a short LLM summary only from the fused work units, never from the raw logs.

The deterministic-vs-LLM split should be hard and explicit:

| Layer | Use deterministic rules | Use LLM |
|---|---:|---:|
| Source discovery and parsing | Yes | No |
| Timezone normalization and day assignment | Yes | No |
| Exact deduplication and idempotency | Yes | No |
| First-pass transcript noise filtering | Yes | No |
| Confidence scoring and gap flags | Yes | No |
| Borderline-session adjudication | Optional fallback only | Yes, but only on uncertain cases |
| Final human-readable ledger summary | No | Yes |

That split is not just about cost. It is about correctness. Recent work on LLM-as-a-judge shows that LLM judges are useful, but their reliability degrades when they are not grounded in correct references or human verification. For this problem, that means an LLM should summarize grounded evidence, not decide the truth of raw logs unless a case has been pre-filtered into a narrow uncertainty bucket. citeturn14view3turn13search1turn13search2turn13search4

## Canonical evidence model and signal fusion

### The canonical object should be an evidence event, not a session

Do not make “session” the atomic unit. A session is too coarse: it can be noisy, span midnight, contain several unrelated tasks, or contain no real work at all. The atomic unit should be an **evidence event** extracted from any source. That is directly aligned with event-sourcing practice, where application state is reconstructed by replaying immutable events and projecting them into read models. Your daily ledger is exactly such a read model. citeturn1search1turn12search1turn12search18

A normalized evidence record should contain at least:

```json
{
  "evidence_id": "sha256(...)",
  "source_type": "git_commit | transcript_turn | blackbox_ledger | registry | retro_note | file_mtime | social_post",
  "source_native_id": "commit_sha | session_id:turn_id | note_path | post_id",
  "event_time_utc": "2026-07-05T23:14:12Z",
  "event_time_local": "2026-07-06T00:14:12+01:00",
  "day_key": "2026-07-06",
  "actor": "human | agent | automation",
  "machine_id": "optional",
  "artifacts": [
    "repo:book/path/chapter-4.md",
    "vault:DR/DR26-07-06-HUB-05.md"
  ],
  "action_type": "create | edit | decide | research | commit | publish | configure",
  "summary_stub": "drafted methodology section for day-ledger pipeline",
  "reliability_tier": "A | B | C",
  "supporting_fields": {
    "git_author_date": "...",
    "git_committer_date": "...",
    "session_id": "...",
    "registry_id": "..."
  }
}
```

That model makes every downstream step simpler: clustering, attribution, confidence scoring, re-runs, and debugging all become inspectable. It also keeps your ledger debuggable in the same way audit systems keep enough metadata to answer “who did what, where, and when.” citeturn17search4turn4search9

### Reliability should be tiered, not additive

The wrong way to fuse signals is “the thing seen in three places gets triple credit.” That silently double-counts the same work. The correct approach is:

- assign each source a **base reliability tier**,
- cluster evidence that appears to describe the same work,
- take the **best single label** from the highest-tier evidence in the cluster,
- add only a **small corroboration bonus** for independent confirmation.

This mirrors record-linkage and entity-resolution thinking: first determine whether records refer to the same thing, then compute the canonical record, rather than counting every record as a separate entity. Record linkage has a deep literature because joining heterogeneous partial records is exactly the hard part. citeturn11search0turn11search1turn11search2

A practical tiering for your setup is:

| Tier | Sources | Ledger role |
|---|---|---|
| **A** | black-box per-turn ledger, dated registries, decision notes, Git commits | primary evidence |
| **B** | curated retrospectives, authored social posts | corroboration and human wording |
| **C** | classified transcript-derived work events, file mtimes | recall/recovery only |

Why this ranking works:

- Structured ledgers and registries are easiest to project deterministically.
- Git commits are immutable and deterministic, but incomplete and sometimes semantically weak.
- Retrospectives are often high-signal but incomplete by your own description.
- Transcripts and mtimes have the highest recall but the weakest direct semantic trust.

That is also analogous to changelog tooling: structured commit conventions and scoped paths are favored because they are machine-parsable, while final release notes remain curated. Release Please in particular can scope changes to repository subpaths, which is a useful precedent for path-based clustering in your vault and multi-repo setup. citeturn10search2turn10search10turn10search14turn10search3

### Deduplication should happen in three passes

**Pass one: exact identity deduplication.**  
If two records share the same immutable native identifier, they are the same event. Example: the same commit SHA seen in two scans, or the same registry item discovered in two places. This is the easy case and should be fully deterministic. Idempotency guidance from AWS and Stripe is directly relevant here: repeated processing of the same operation should return the same result rather than multiply effects. citeturn1search9turn1search0

**Pass two: deterministic work-unit clustering.**  
Create a `work_unit_key` from normalized artifact paths, action type, and a coarse event-time bucket. A good first implementation is:

```text
work_unit_key =
  hash(
    sorted(canonical_artifact_paths) +
    primary_action_type +
    local_day_key +
    nearest_30min_bucket
  )
```

If a commit, ledger event, and registry entry all touch the same artifact set on the same day within a tight time band, they probably belong to one work unit. Keep all provenance, but emit one canonical item in the daily ledger.

**Pass three: fuzzy near-duplicate merge for text-only sources.**  
Only for notes, social posts, and transcript summaries, compute a similarity fingerprint such as SimHash or MinHash over normalized text. Near-duplicate detection at web scale uses these techniques because they are efficient and robust to small wording changes; here the scale is tiny, but the pattern still transfers cleanly. citeturn2search2turn11search3

The fusion rule should be conservative:

- choose the **canonical title** from the highest-tier source in the cluster;
- union the artifacts;
- preserve all provenance in a footnote or hidden metadata block;
- add a “corroborated by X, Y” note instead of duplicating the accomplishment as separate bullets.

A simple scoring rule works well:

```text
cluster_score =
  max(source_weight)
  + 0.10 * distinct_supporting_sources
  + 0.05 * artifact_overlap_count
  - 0.15 * contradiction_penalty
```

The point is not mathematical elegance. The point is that **duplicate sources should increase confidence, not increase activity volume**.

## Noise filtering and session attribution

### Use a cascade, not a single classifier

The most robust approach for transcript noise is a **three-stage cascade**:

1. **obvious-noise deterministic filter**
2. **cheap classifier for borderline sessions**
3. **optional LLM adjudication only for the uncertain residue**

That is the same broad shape found in bot-detection literature: hybrid rule-based plus learned systems outperform pure heuristics when labels are incomplete, and complementary detectors can trade speed against robustness. One recent study on human-versus-bot web activity explicitly used a rule-based system plus a hierarchical classifier; the older USENIX chat-bot work combined entropy-based and machine-learning-based classifiers, noting that the entropy classifier generalized better to unknown bots while the ML classifier was faster on known patterns. citeturn14view0turn14view1turn15view0turn15view1

### What the deterministic filter should do

The deterministic stage should be designed for **high precision on rejection**, not for perfect recall. Auto-drop only sessions that are very likely to be robot or heartbeat noise. Typical reject conditions:

- zero artifact writes and zero registry emissions,
- only housekeeping commands such as status/list/read without any subsequent mutation,
- known automation identities or heartbeat file names,
- transcript duration below a small threshold and extremely low token/message count,
- repeated periodic timing patterns with negligible content variety,
- repeated templated content across many sessions,
- no decisions, no created files, no changed files, no commits, no outbound note or post.

The entropy angle is useful here. Human work sessions usually have higher variability in timing, artifact sequence, and content than periodic automation. That mirrors the bot-detection result that human behavior tends to be more complex than bot behavior. citeturn14view1turn15view0

### The cheap classifier is worth it after you have labels

Once you have a few hundred examples of “real work,” “automation,” and “uncertain,” build a small classifier. Do **not** start with an LLM here. Start with a logistic regression, gradient-boosted tree, or a similarly cheap interpretable model.

Use features such as:

- number of unique files touched,
- number of write-like tool actions,
- number of registry/decision emissions,
- session duration,
- message-count entropy,
- inter-action timing entropy,
- repetition score,
- ratio of read actions to write actions,
- whether a Git commit followed within a short horizon,
- whether a public post or retrospective references the same artifacts later that day.

If labeled data is scarce, bootstrap it with **weak supervision**: write labeling functions for obvious noise and obvious work, then combine them into probabilistic labels and train a downstream model. That is exactly the problem Snorkel was built for. Precision-recall, not raw accuracy, should be the main evaluation metric because your classes will be imbalanced and “real work” is the positive class you do not want to miss. citeturn14view2turn2search4turn2search3turn2search19

My recommendation is:

- optimize for **high recall** on real work,
- allow an **uncertain** bucket,
- only hard-drop items when the reject confidence is very high.

That prevents the worst failure mode for a personal historical ledger: a day that looks empty when it was not.

### Where an LLM judge is worth the money

Use an LLM judge only in two places:

**First, summarization.**  
This is high leverage. Asking an LLM to convert structured, grounded work units into readable prose is usually worth the tokens.

**Second, narrow adjudication of uncertain sessions.**  
Only after deterministic filtering and the cheap classifier. Give the model a small, structured packet: touched artifacts, action counts, timing features, registry links, and a few excerpted transcript spans. Ask a narrow question such as: “Did this session produce a durable artifact, decision, or externally visible output?” That is a bounded classification task with references.

Do **not** use an LLM as the primary truth engine over raw transcripts. Recent work shows LLM judges are much more reliable when grounded by references; without that grounding, correctness-heavy evaluation degrades. citeturn14view3turn13search2turn13search4

### Day attribution should be event-time based, not file-arrival based

For day assignment, use **event time**, not processing time. Stream-processing systems make this distinction very explicitly because arrival time and occurrence time diverge in real systems; that is exactly your situation when machines sync later, vault files appear after the fact, or a session continues across midnight. Watermarks and late-data handling exist for this reason. citeturn0search3turn5search4turn5search9turn5search13

Use these rules:

- canonical timezone: `Europe/Lisbon`;
- store both UTC and local timestamps on every evidence event;
- ledger unit: **work unit**, not whole session;
- assign each work unit to the day of its **actual evidence timestamps**.

For sessions spanning midnight, split by substantive activity. A practical rule is:

- break sessions on inactivity gaps, for example 30 minutes;
- create sub-work-units around write/decision moments;
- assign each sub-unit to the local day containing the majority of its write events;
- if a sub-unit has no writes, anchor it to the nearest durable outcome it caused.

This avoids absurd outcomes like crediting a 23:55–00:40 editing session entirely to the start day or end day.

### Git dates need explicit policy

Git stores both **AuthorDate** and **CommitDate**. Git’s own formatting docs distinguish them clearly, and the fuller pretty format exposes both. citeturn7view0turn6search4

For a daily activity ledger, use this policy:

- **CommitDate** = when the commit entered history on that machine and is usually the better operational timestamp.
- **AuthorDate** = supporting context only.
- If `abs(CommitDate - AuthorDate) > threshold`, flag the commit as temporally ambiguous.

That matters because rebases, cherry-picks, delayed commits, and amended history can otherwise create false certainty.

### Multi-machine syncing and idempotency

Machine-local file mtimes should be treated as **secondary evidence only**. They are useful for recall, but not for semantic truth. The reason is simple: file timestamps reflect file-system modification semantics, not accomplishment semantics, and they can be affected by sync behavior, metadata changes, or directory operations. Use them to detect candidate activity, never as the canonical description of what happened. citeturn16search1turn16search5turn16search9turn16search21

To keep re-runs idempotent:

- assign every evidence record a stable `evidence_id`,
- write outputs by **full regeneration** of the day projection, not append,
- keep a manifest of input file hashes and assembler version,
- reprocess a rolling window of recent days, not just yesterday.

Idempotency best practices from AWS and Stripe transfer directly: repeated ingestion of the same input should have no additional effect beyond the first accepted representation. citeturn1search9turn1search0

For timezone correctness, use IANA zone data through `zoneinfo`, not hand-written UTC offsets, because `Europe/Lisbon` observes DST and the correct offset changes seasonally. citeturn6search2turn6search10

## Completeness and confidence

### A day is complete only when both ingestion and lateness conditions pass

You should never mark a day “complete” merely because you generated a file for it. A day is complete only when:

- every configured source was scanned successfully,
- expected source roots were reachable,
- all parsed evidence was normalized without fatal errors,
- the day is outside your chosen late-arrival window,
- unresolved uncertain sessions are below a threshold.

This is where stream-processing ideas help. Watermarks exist because systems need a principled way to say “we believe all events for this window have probably arrived.” You want the same concept for a daily ledger. citeturn0search3turn5search4turn5search13

My recommendation:

- mark **same-day and previous-day ledgers as provisional**,
- re-run the last **three local days** each night,
- mark a day **stable** only after a **36–48 hour watermark** has passed.

That gives enough room for delayed sync, late commits, or a retrospective written the next morning.

### Confidence should be per day and per work unit

Do not emit one opaque score. Emit both:

- a **day confidence score**, and
- a **work-unit confidence score**.

A good day score has four components:

```text
day_confidence =
  source_coverage
  + evidence_quality
  + cross-source_corroboration
  - ambiguity_penalties
  - lateness_penalty
```

A practical weighting scheme:

- **source coverage**: 35 points  
  Were all configured sources read without error?
- **evidence quality**: 25 points  
  What share of the day is supported by Tier A evidence?
- **corroboration**: 20 points  
  How many canonical work units are independently confirmed?
- **specificity**: 20 points  
  Do the units reference concrete artifacts, decisions, or outputs?
- **penalties**  
  unresolved uncertain sessions, source failures, midnight ambiguity, large Git author/committer skew, only-mtime evidence, still inside watermark window.

Then map the numeric score to statuses:

| Status | Meaning |
|---|---|
| **Stable** | all configured sources scanned, outside watermark, little ambiguity |
| **Provisional** | likely good, but still inside late-arrival window |
| **Incomplete** | one or more sources missing or parse failed |
| **Needs review** | too much ambiguous/noise-derived evidence |

This is very similar to how audit systems and engineering dashboards distinguish raw event presence from trustworthy reporting: they centralize and deduplicate events, then expose search/report surfaces only after collection. citeturn17search3turn17search1turn17search2

### Gap flags should be explicit and machine-readable

Every day ledger should include a `gaps` section with specific machine-readable reasons, for example:

```yaml
confidence: provisional
score: 74
gaps:
  - missing_source: "public_posts_export"
  - parse_error: "session_jsonl malformed in 2 files"
  - uncertain_sessions: 3
  - only_low_trust_evidence: false
  - watermark_open_until: "2026-07-07T12:00:00+01:00"
```

That is much better than silently pretending completeness. Modern audit tooling does the same thing operationally: it makes the collection surface explicit and queryable, rather than hiding ingestion assumptions. citeturn17search4turn17search0

## Prior art and the patterns that transfer

### Changelog automation transfers surprisingly well

Conventional Commits, semantic-release, Release Please, and GitHub’s generated notes all solve a narrower version of your problem: reconstructing meaningful change history from noisy engineering metadata. They transfer four directly useful patterns:

- **structured categories** such as features, fixes, docs, chores,
- **path scoping** so a change is attributed only to the relevant component,
- **noise exclusion** by user/label/category,
- **final curation layer** instead of raw log dumping. citeturn10search14turn10search0turn10search2turn10search1turn10search9turn10search10

For your ledger, that means:

- classify work units into a small ontology,
- scope them by repo/vault area,
- exclude robot identities and generated-file paths,
- write a clean day narrative from fused work units, not from raw transcripts.

### Audit-log aggregation gives you the right mental model

Cloud Audit Logs explicitly position themselves as a way to answer “who did what, where, and when.” Datadog and Microsoft’s audit tooling likewise centralize events, allow deduplicated searches, and produce scheduled reports. That is exactly the right mental model for your ledger: not a diary, not a changelog, but an **activity projection over a normalized event corpus**. citeturn4search9turn17search4turn17search1turn17search3

The important transfer is this: **application logs are not the same thing as audit records**. So transcripts are not the same thing as the daily ledger. The ledger should be derived from transcripts plus stronger structured evidence, not equated with them.

### Engineering-intelligence systems confirm the multi-signal strategy

Tools like LinearB, DX, and GitClear exist because organizations want to reconstruct work from multiple systems rather than from one proxy. Their product logic is “correlate Git with planning and other tool data,” not “count commits.” The SPACE framework backs the same point academically: use multiple dimensions and multiple data sources rather than a single activity metric. citeturn8search0turn8search4turn8search12turn8search2turn9search0turn9search11

The transferable pattern for you is:

- **Git alone is insufficient**,
- **retrospectives alone are insufficient**,
- **transcripts alone are too noisy**,
- therefore the system needs **correlated evidence with source-aware confidence**.

### Async standup tools show what the final output should feel like

Async standup and check-in tools such as Range and StandupBot collect daily updates, integrate work signals, and maintain searchable history. The transferable lesson is not their data model. It is the UX pattern: **one concise daily artifact that humans can actually scan later**. That is what your `day-ledger-YYYY-MM-DD.md` should be. citeturn4search0turn4search1turn4search5

### Event-sourcing projections are the deepest fit

Of all prior art, event sourcing is the closest architectural analogue. Store each normalized evidence event immutably, then derive a **materialized daily view** from those events. That gives you replayability, deterministic rebuilds, and the ability to improve the projection logic later without losing history. citeturn1search1turn12search1turn12search0turn12search18

## Minimal viable nightly pipeline

### The simplest architecture that will survive real life

If the goal is “a non-technical owner must understand and repair it,” keep it boring:

- **filesystem inputs**
- **one config file**
- **one SQLite database**
- **one nightly script**
- **Markdown output**
- **LLM only for final summary**

Do not start with Kafka, a vector database, a workflow engine, or a microservice split. The system is naturally batch-shaped.

The component list can be this small:

| Component | Purpose |
|---|---|
| `config.yaml` | source roots, repo paths, timezone, ignore rules |
| `ingest.py` | scans sources and normalizes evidence |
| `ledger.db` | SQLite store for evidence events, work units, and manifests |
| `project.py` | rebuilds daily projections and scores confidence |
| `summarize.py` | optional LLM summary from grounded work units |
| `out/day-ledger-YYYY-MM-DD.md` | final daily artifact |
| `out/day-ledger-YYYY-MM-DD.json` | machine-readable projection |

### Nightly step order

The concrete step order I recommend is:

1. **Define the rebuild window.**  
   On each nightly run, rebuild local days `D-2`, `D-1`, and `D`, not just yesterday. This absorbs late-arriving evidence using a watermark-like approach. citeturn0search3turn5search4

2. **Read deterministic sources first.**  
   Parse Git commits, black-box ledger files, structured registries, dated decision notes, dated NOTE registry items, and exported social posts. These provide the strongest skeletal facts.

3. **Read transcript metadata and apply obvious-noise rules.**  
   Reject clearly robotic or heartbeat sessions without spending tokens.

4. **Extract transcript work events only from remaining sessions.**  
   You do not need full semantic summarization here. Just emit low-cost structured markers such as touched files, decisions detected, write operations, and durable outputs.

5. **Scan file mtimes last and use them only as recall hints.**  
   If a file changed but no stronger source exists, produce a low-trust evidence record rather than pretending it is a confirmed accomplishment. citeturn16search1turn16search9

6. **Normalize all evidence to canonical local time.**  
   Convert using `Europe/Lisbon` via IANA time-zone data. citeturn6search2turn6search10

7. **Deduplicate into work units.**  
   Exact-id merge first, then deterministic clustering by artifact/time/action, then optional fuzzy merge for text-only duplicates.

8. **Score each work unit and each day.**  
   Compute confidence, completeness, and gaps.

9. **Generate the Markdown ledger body deterministically.**  
   Emit sections such as: accomplishments, decisions, touched artifacts, commits, posts, open gaps, confidence.

10. **Call the LLM once for the final narrative summary.**  
    Give it only the structured work units plus confidence/gap fields. Because the model sees grounded references rather than raw logs, the output is cheaper and less likely to hallucinate. citeturn14view3

11. **Overwrite the day files atomically.**  
    Do not append. Regenerate. That preserves idempotency. citeturn1search9turn1search0

A good output skeleton looks like:

```markdown
# Day Ledger — 2026-07-06

status: provisional
confidence: 81
sources_scanned:
  - git: ok
  - blackbox_ledger: ok
  - transcripts: ok
  - retros: partial
  - registries: ok
  - public_posts: missing
watermark_open_until: 2026-07-07T12:00:00+01:00

## Summary
<LLM-generated paragraph from grounded work units>

## Canonical accomplishments
- ...
- ...

## Decisions
- ...

## Artifacts touched
- repo:path
- vault:path

## Evidence provenance
- work_unit_id -> [commit, registry, session, note]

## Gaps
- ...
```

### Build order

Build it in this order:

**First build:** deterministic skeleton  
Implement source ingest, evidence normalization, exact dedup, timezone conversion, and daily Markdown emission with no transcript intelligence except obvious filters.

**Second build:** transcript recovery  
Add transcript-derived low-trust evidence extraction, artifact mapping, and work-unit clustering.

**Third build:** confidence and gap model  
Add completeness checks, watermark handling, and explicit review flags.

**Fourth build:** classifier  
Only after you have labeled borderline sessions.

**Fifth build:** LLM summary  
Last, because it is presentation, not truth.

That order mirrors successful event-sourced systems and changelog tooling: the event model and projection logic come first, the prose layer later. citeturn12search1turn10search2turn10search0

## Failure modes that matter

### The top risk is silent false emptiness

**Failure mode:** the day looks empty because the real work lived only in transcripts, an uncommitted file, or a late-sync artifact.

**Why it happens:** relying too heavily on commits or retros; late-arriving evidence; overly aggressive transcript filtering.

**Mitigation:**  
Use multi-source fusion, keep transcripts as recall sources, and rebuild a rolling three-day window with a watermark before marking a day stable. Also optimize your session classifier for recall, not precision, on the “real work” class. Precision-recall is the right metric for such imbalanced detection problems. citeturn9search0turn0search3turn5search4turn2search3turn2search19

### The next risk is double-counting the same work

**Failure mode:** one task appears once from the transcript, once from the commit, once from the registry, and once from the retrospective, so the ledger exaggerates output.

**Why it happens:** counting sources rather than linking records to the same underlying work unit.

**Mitigation:**  
Use stable evidence ids, deterministic work-unit keys, and a record-linkage mindset. Duplicate evidence should raise confidence, not volume. Make the rendered ledger emit one canonical accomplishment with provenance, not four parallel bullets. citeturn11search0turn11search1turn11search2turn1search9

### The third risk is robot work being mistaken for human accomplishment

**Failure mode:** heartbeat, maintenance, or autopilot sessions inflate the day ledger.

**Why it happens:** raw transcripts are noisy; many automated behaviors can look active if you only count messages or tool calls.

**Mitigation:**  
Use the hybrid cascade: deterministic denylist for obvious automation, a cheap classifier for borderline sessions, and LLM adjudication only on the uncertain residue. Bot-detection literature repeatedly shows value in combining interpretable rule-based and learned components, especially when labels are incomplete and unknown bot patterns exist. citeturn14view0turn14view1turn15view0turn15view1turn14view2

### The architecture I would actually ship

If I had to choose one production shape for your setup, it would be this:

- **append-only evidence store in SQLite**
- **deterministic daily projection in local time**
- **explicit late-data watermark**
- **tiered source reliability**
- **cluster-based dedup with provenance retention**
- **transcript filtering by rule cascade first**
- **LLM only for grounded summary and rare uncertainty resolution**

That architecture is the most robust because it inherits four mature ideas that already work elsewhere: curated changelogs do not dump raw logs, audit systems centralize “who did what when,” engineering-intelligence systems correlate multiple tools, and event-sourced systems derive stable read models from immutable evidence. Those patterns fit your AI-agent-plus-vault world unusually well. citeturn10search3turn4search9turn17search4turn8search0turn9search11turn12search1