# DR26-11-13-MACANTON-01-0708 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll search live sources for the exact embedding-revision log line, Apple's own Photos analysis docs, and any first-person reports before drawing a conclusion.A. Executive Summary

No public exact match and no validated cure were found for this log line:

`Unable to add embedding: provided embedding revision (2) does not match the current embedding revision (3737841669)`

The same is true for the nearby pair `3737841665` versus `3737841669`. A public mapping of those private integers is UNKNOWN. Public Vision feature-print revision 2 must not be treated as the same number, or as proof that the Photos database is corrupt.

What is established is narrower. Apple’s public Vision API refuses to compare image feature prints whose request revisions differ, and an Apple frameworks engineer told developers in June 2023 to reprocess with one revision or pin the old one. Apple’s private MediaAnalysis symbols, indexed from a macOS 26.5-to-26.6 diff, use a different small-integer scheme (`revision 9 (MD7)`, `revision 10 (MD8)`). The ten-digit values in these logs match neither public scheme. The safe reading of the log is a write-time compatibility guard: do not store a vector that was produced under a different embedding revision. That reading is inferred from the wording plus the public Vision analogue. It is not a disassembly of `mediaanalysisd`.

The two Macs in the brief are one household and are not two independent reproductions. The measured rejection bursts last about 0.24 seconds and sit beside successful analysis completions. They do not prove an infinite loop, and they do not prove that the historical 8.99-core load was caused by these rejections.

Recommendation, confidence [emerging]: keep diagnosing with a bounded read-only log experiment, and file one minimal Feedback Assistant report that describes the signature without claiming a proven defect or a cure. Do not Repair Library, delete containers, or disable the daemons on this evidence. The assumption underneath that recommendation is that these short bursts are discards of stale embeddings during an ordinary model migration. The experiment in section G is what would falsify that.

B. Key Findings

1. What “embedding revision” means, and why a mismatch can be rejected safely. [established] for the public Vision API. [speculative] for the private Photos log.

Apple’s Vision documentation says a feature-print revision is the algorithm version used for one request. Each OS release that improves the algorithm increments that revision by 1. By default a request uses the newest revision for the SDK the app was linked against. Indexed body of https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1 (documentation crawl date shown as 2026-11-10; the revision constants themselves date to the Vision feature-print API, with revision 2 associated in public discussion with iOS 17 in 2023). Direct HTML fetch of that page on this run returned only the title shell, so the paragraph above is from the indexed body, not a full rendered page.

If two feature prints come from different revisions, `computeDistance` fails closed:

`Error Domain=com.apple.vis Code=12 "The revision of the observations do not match"`

An Apple frameworks engineer, June 2023, on https://developer.apple.com/forums/thread/732876 (direct fetch was blocked by Apple’s bot wall; this quote is from the indexed thread):

“you are likely attempting to compute the distance of prints from two different revisions. (All `VNObservation`s have a `requestRevision` property which indicates the revision of the request that produced it).”

The engineer’s remedies were: pin `VNGenerateImageFeaturePrintRequestRevision1`, compare only within one revision, or reprocess every stored print with the newer revision. That is a compatibility guard, not a corruption finding. Direct fetch of this forum page failed. [single-source] for the engineer’s wording. The error string itself is repeated in the same indexed thread by the original poster.

An independent developer measurement, September 2023, reported that revision 1 prints were non-normalized 2048-float vectors and revision 2 prints were normalized 768-float vectors, so distances are not on the same scale. https://stackoverflow.com/questions/77185128/is-vnfeatureprintobservation-and-vision-distance-computation-different-for-two (indexed; full page fetch failed). [single-source], and older than 18 months. It shows why mixing revisions is unsafe. It does not identify Photos’ private integer `3737841669`.

2. The private integers are unmapped. [established] that no public map was found. The values themselves are OBSERVED only in the brief.

Searches for `3737841669`, `3737841665`, `provided embedding revision`, `current embedding revision`, and the full sentence `Unable to add embedding` returned no Apple document, release note, developer-forum answer, GitHub issue, or first-person post that contains those tokens. Public mapping: UNKNOWN.

Three numbering schemes are visible, and they do not line up:

- Public Vision feature prints: revision 1 and revision 2, documented as small integers that increment by 1. https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1 and https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision2 (title shells fetched; revision-2 page body not fully rendered).
- Private MediaAnalysis image embeddings, macOS 26.5 versus 26.6 symbol diff: the string `VCPImageEmbeddingRequest only supports revision 9 (MD7) and revision 10 (MD8)`, plus selectors `embeddingVersion` and `MADEmbeddingStore preferredImageEmbeddings:versions:`. Indexed at https://github.com/blacktop/ipsw-diffs/blob/main/26_5_23F77_vs_26_6_23G5028e/DYLIBS/MediaAnalysis.md. A direct raw GET of that path returned 404 on this run, so the file may have moved. [single-source], symbol strings only, not runtime logs.
- The logs in the brief: provided `2`, current `3737841669`, and sometimes current `3737841665`. Arithmetic on the brief: those two current values differ by 4. That is not a published model map.

The logged `2` is therefore not evidence that public Vision revision 2 is the current Photos model, and a mismatch is not evidence that `Photos.sqlite` is corrupt.

3. Exact public signature count is zero independent incidents. [established] for the search coverage below. Not a prevalence rate.

The only exact signature in hand is the pair of family Macs in the brief: MacBookPro16,1 on macOS 26.3.1(a), and Mac16,9 on macOS 27.0.1 build 26A434, both showing provided `2` to current `3737841669`. The brief states they may share migrated Photos or iCloud data, and that sharing was not verified. Count them as one household cluster, not two clean reproductions. No public URL exists for that cluster.

The closest different message is one first-person incident, deduplicated:

- Who: the author of zerosleeps, also the Apple Community poster in thread 256084597. Same person, one incident.
- Date: noticed about a week before 15 June 2025. Hardware: M1 MacBook Pro, plugged in, displays sleep but system does not. OS at onset: macOS Sequoia 15.5 (24F74).
- Symptom: `Embedding version: 0 not supported, skip embedding publishing`. In one roughly three-hour capture, 1,641,196 `mediaanalysisd` log lines, 992,607 errors, of which 918,968 were that sentence. `fs_usage` showed 271,375 opens but only 675 unique files, mostly small images under the home folder, including 1×1 and favicon-sized files. Companion lines included `Image has invalid or too small dimensions (1x1)`, `Failed to decode image`, and `Failed to load Scene Taxonomy for analysis version: 0`.
- Evidence: https://zerosleeps.com/blog/2025/6/15/mediaanalysisd-has-gone-rogue/ (opened, 15 June 2025). [single-source] [low-authority] personal blog, but it is a primary account of that author’s own machine, with counts from `log stream` and `fs_usage`.
- Outcome: Spotlight-excluding the parent directory stopped the thrash and is described by the author as a workaround, not a fix. macOS 15.6, 15.6.1, 15.7, and 15.7.1 did not fix it. https://zerosleeps.com/blog/2025/7/30/does-macos-156-fix-mediaanalysisd/ (opened, 30 July 2025, updates through 31 October 2025). After upgrading to macOS 26 on 16 September 2025 and removing Spotlight exceptions, the author reported the machine quiet, while the unified log still had a couple of thousand copies of the version-0 line. https://zerosleeps.com/blog/2025/9/17/macos-26-fixes-my-mediaanalysisd-problem/ (opened, 17 September 2025). That is a CPU change with the log line still present. It is not this signature, and it is not a Photos-library repair.

Apple Community thread https://discussions.apple.com/thread/256084597 is the same author’s report (indexed excerpts match the blog: 174,522 version-0 lines in 45 minutes, then 4,017,783 errors and over 5 hours of CPU in 9 hours after Safe Mode). Direct fetch was bot-walled. Do not count it as a second incident.

4. Generic high CPU is a different phenomenon. [established] as a long-running class of complaints. It does not establish this case.

Apple’s own man page, mirrored at https://macosbin.com/bin/photoanalysisd (opened; page has no publication date; text is the `man` summary): `photoanalysisd` “handles background processing of photo libraries for Memories, People and scene or object based search.” [low-authority] as a mirror, the wording is Apple’s manual.

Apple’s support article says pet detection on a Mac runs while Photos is not in use, and “could happen quickly or take up to a few days,” depending on background work and library size. https://support.apple.com/en-us/105081 (opened, published 18 March 2025). Photos privacy text says the on-device analysis behind Memories, People & Pets, and Featured Photos includes scene classification, people and pets identification, photo quality, and audio classification, and that Apple does not access the photos. https://www.apple.com/legal/privacy/data/en/photos/ (opened, page date 12 December 2025).

Indexed, not fully opened, and not exact matches: Apple Community threads about `photoanalysisd` at about 99% CPU for 140 hours on Sonoma (https://discussions.apple.com/thread/255737983), days of high CPU after 14.7 on a library of 116,171 photos and 580 videos (https://discussions.apple.com/thread/255781588), and `mediaanalysisd` heat on Tahoe 26 with no log signature (https://discussions.apple.com/thread/256138728). Forum “Me too” counts are not a rate. Separately, an April 2026 developer-forum report of `mediaanalysisd` CPU in the iOS Simulator was traced by a second poster to `MADTextEmbeddingTask` for `com.apple.spotlightknowledged.updater` after `CSSearchableIndex` calls. Skipping those calls in the Simulator left the process idle. https://developer.apple.com/forums/thread/824784 (indexed, April and May 2026). That is a Spotlight text-embedding trigger, not this Photos revision guard.

5. No Apple acknowledgement of the exact signature. [established] for the documents opened.

Opened macOS Tahoe release notes through 26.7.3, published 9 November 2026: https://support.apple.com/en-us/122868. The only Spotlight sentence is that 26.6 “optimizes the Spotlight index in preparation for macOS 27.” No Photos embedding-revision fix, no `mediaanalysisd` fix, and no mention of revision `3737841669`. Developer release-note search hits for Photos in Tahoe 26 were picker crashes and picker option bugs, not this log. https://developer.apple.com/documentation/macos-release-notes/macos-26-release-notes (indexed). No Feedback Assistant status, radar, or “works as designed” answer for this sentence was found.

6. Academic and industry literature does not diagnose this log. [established] as a negative search result.

No paper found names `mediaanalysisd`, `photoanalysisd`, or this sentence. Off-topic hits (image-provenance embeddings, text-embedding inversion) were not used. The relevant industry pattern, from 2025–2026 engineering writeups indexed in this run, is that vectors from two model revisions are different spaces, and a correct system refuses the mix or rebuilds. That pattern explains why a guard is reasonable. It is not evidence that Apple’s daemon is looping.

C. Contradictions and Debates

Side 1, steelmanned. This is an ordinary migration guard. Apple documents that analysis is re-run as libraries and OS versions change, that it can take days, and that it is heavy. The public Vision API already rejects cross-revision feature prints rather than returning a bogus distance. A log that says “unable to add” is a refused insert, which is what a guard should do. The brief’s bursts are 0.24 seconds, and successful completions were seen. A household that migrated libraries onto both macOS 26 and macOS 27 would be expected to share stale derived embeddings. Two machines with one user’s data are one migration, not a population.

Side 2, steelmanned. This is a stuck scheduler. The zerosleeps incident is real, measured, and ugly: the same small files were opened thousands of times an hour, the version-0 line was emitted hundreds of thousands of times, point releases through 15.7.1 did not stop it, and the author still saw the line after macOS 26 even though the CPU quieted. If Photos does the same thing with revision `2` against current `3737841669`, each rejection could be preceded by a full feature computation, and 620 rejections an hour, repeated, would be a defect even if each log burst is short. The 8.99-core historical sample shows the machine can be driven that hard by media analysis. Nothing in the public record proves these particular rejections are cheap.

What the evidence does not decide: whether the rejected embedding is computed expensively and then dropped, or is an old stored vector discarded in microseconds. The brief explicitly says the 0.24-second logging interval does not bound upstream compute time. Both sides survive until asset identity and CPU time are correlated.

D. Best Practices

For the mechanism, the defensible practice is the one Apple already published for third-party apps: never compare or store across revisions. Recompute into the current revision, or keep the old revision pinned and isolated. https://developer.apple.com/forums/thread/732876 (June 2023, indexed). Do not invent a SQL update that rewrites the private revision column. That would be the unsupported database modification the brief correctly avoided.

For an ordinary Mac owner, the official GUI options, and what each does not establish:

1. Leave the Mac awake, on power, with Photos not in front. Prerequisite: none beyond power and idle time. Apple says pet and people indexing on a Mac runs while you are not using Photos and can take up to a few days. https://support.apple.com/en-us/105081 (18 March 2025). Likely extra work: the backlog Apple already queued. iCloud effect: none by itself. Risk: heat and fan noise while it runs. Does not establish: that this log line will stop, that the library is healthy, or that CPU will fall. A few days of idle time is the official window for people-and-pets indexing, not a promised end for every `mediaanalysisd` message.

2. Update macOS. Prerequisite: a normal software-update path, enough disk, and a backup you already trust. Apple’s general Photos advice is to run a current macOS. https://support.apple.com/en-nz/108793 (opened, 25 February 2026). The opened Tahoe notes through 26.7.3 do not claim a fix for this signature. https://support.apple.com/en-us/122868 (9 November 2026). The second Mac is already on macOS 27.0.1 and still showed the signature, so “install the latest OS” is not an established cure for this household. iCloud effect: a new OS can re-queue analysis. Does not establish: a root cause.

3. Quit and reopen Photos. Prerequisite: none. Effect: a temporary pause. Howard Oakley’s 2022 observation, older than 18 months and not re-tested here, was that `photoanalysisd` is suppressed while Photos is open. https://eclecticlight.co/2022/10/07/is-apple-scanning-the-images-on-my-mac/ (indexed, 7 October 2022). Does not establish: progress, corruption, or a cure. Quitting the daemon in Activity Monitor is the same class of pause. Mole, 3 November 2026, says macOS starts it again and it resumes the same queue. https://mole.fit/blog/mediaanalysisd-high-cpu-mac (opened). [low-authority] commercial blog, consistent with the zerosleeps and Apple Community pattern.

4. A separate new library, and optionally another user account. Prerequisite: do not import the large library into it. Hold Option while opening Photos, create a new library, add a few photos. https://support.apple.com/en-nz/108793 (25 February 2026). If the new library stays quiet in a matched idle window and the main library still emits the signature, the variable is the library or its synced state, not the whole Mac. iCloud effect: do not turn on iCloud Photos for that test library unless you intend a second sync. Does not establish: which assets are rejected, or that Repair Library would help. It also does not clear the main library.

5. Repair Library, only after a complete backup, and only if the library itself is the suspect. Prerequisite, in Apple’s words: back up first, by Time Machine, another backup, or a copy of the library. Then quit Photos, hold Command and Option, click Repair. The tool “analyzes the library’s database and repairs any inconsistencies it detects.” If the library uses iCloud Photos, “iCloud checks the library after the repair.” https://support.apple.com/en-us/118221 (opened, 22 May 2024). Likely extra work: a full database pass, then an iCloud consistency check, which can re-queue analysis. Risks: time, heat, and a changed database. Apple does not say it regenerates embeddings or that it is reversible. Does not establish: that a revision mismatch was the bug, that originals were damaged, or that the log will stay gone. A quiet week after repair is still compatible with “repair forced a rebuild that was going to happen anyway.”

6. Apple Support and Feedback Assistant. Prerequisite for a useful report: date, OS build, hardware, the exact sentence, a short log excerpt with asset identifiers hashed or removed, and no image content and no names of people. Feedback Assistant on a Mac is in `/System/Library/CoreServices/Applications` and collects a sysdiagnose when you allow it. https://developer.apple.com/bug-reporting/ (opened, living page, fetched 13 November 2026). One issue per report. Apple may group it with similar reports and may answer “works as designed,” “unable to diagnose,” or “potential fix in a future OS,” and they do not promise a reply. Does not establish: a fix, a prevalence rate, or that Support will see this as corruption.

Reboot is out of scope on the active machine, as requested. It would also be a weak test: the zerosleeps machine was rebooted and the version-0 loop continued.

E. Failure Modes

Claimed internet cures, judged only where a before-and-after exists:

- Force-quit `photoanalysisd` or `mediaanalysisd`. Anecdotal mitigation for minutes. Apple Community posters report the process returns at the same CPU. Indexed thread https://discussions.apple.com/thread/255737983. Not a cure. Relapse is the expected launchd behavior.

- `launchctl disable` of `com.apple.photoanalysisd`, `com.apple.mediaanalysisd`, or `com.apple.photolibraryd`, including recipes that first disable System Integrity Protection. These are circulated on MacRumors, DroidWin, and similar pages (indexed January 2025 and December 2024). Mole’s opened guide says this turns off face, object, and Photos search until reversed, which people forget. https://mole.fit/blog/mediaanalysisd-high-cpu-mac (3 November 2026). Anecdotal mitigation of CPU by deleting the feature. Not endorsed. SIP changes are out of scope and are not a Photos repair.

- Delete `~/Library/Containers/com.apple.mediaanalysisd` or the whole cache. Mole says a container delete throws away finished work and starts analysis again. Cache deletion can reclaim disk after Sequoia cache-growth reports, but regrowth during a real backlog is normal. No before-and-after in the opened sources shows that deleting caches stops a revision-mismatch log. Anecdotal disk mitigation, not a cure for this signature.

- Spotlight-exclude a folder. Worked for the zerosleeps home-folder images as a workaround, with an explicit loss of search. It did not fix the version-0 line’s existence, and those files were not described as a Photos library. Do not exclude the Photos library to “fix” search inside Photos.

- “Reset analysis” one-liners and `launchctl unload` of the system launch agent. No opened report shows a durable before-and-after on the exact signature. Treat them as unverified. Several are aimed at the wrong process (`photoanalysisd` versus `mediaanalysisd`) or at stopping iCloud Photos entirely.

- Repair Library as a first response to this log. Failure mode: a large library is rewritten and then iCloud rechecks it, which can increase analysis, while the log line was only a guard. Apple’s article is for a library that will not open or behaves unexpectedly, not for a background revision message. https://support.apple.com/en-us/118221.

- Calling the 8.99-core sample the proof. Failure mode: other workloads, Intel thermal limits, and a legitimate multi-day index get blamed on a 0.24-second log burst. The brief already separates those.

- Treating the two Macs as a prevalence. They are one user’s possibly shared data. Forum vote counts are not a rate either.

F. Strategic Recommendations

Named options:

Option 1, wait and do nothing else. Trade-off: matches Apple’s “up to a few days” window and avoids every destructive step. Risk: if the same assets are recomputed every hour, waiting does not end it, and the Intel laptop keeps the heat. Does not produce a publishable result.

Option 2, bounded read-only diagnosis plus one Feedback report. Trade-off: a week of comparable logs can separate a cheap discard from an expensive retry without touching the database. Risk: log privacy, and a sysdiagnose contains more than the embedding line, so it stays on the Mac and goes only to Apple if you choose to send it. This is the proportionate step.

Option 3, new library split, still read-only toward the main library. Trade-off: tells you whether a clean library on the same OS is quiet. Risk: easy to contaminate by turning on iCloud Photos. Does not name the assets.

Option 4, Repair Library after a full backup. Trade-off: Apple’s supported database pass if the library is actually inconsistent. Risk: time, iCloud recheck, and a result you cannot interpret unless Option 2 was recorded first. Not justified by the log sentence alone.

Option 5, disable or delete the analysis services. Trade-off: CPU falls. Risk: search, People, and Memories degrade, the work returns when re-enabled, and you learn nothing. Rejected as a cure.

Single recommendation: Option 2, confidence [emerging]. Main assumption: the observed 0.24-second bursts, with successful completions also present, are compatible with a migration guard discarding stale embeddings. That assumption dies if the experiment below shows the same assets being fully recomputed before each rejection across days.

G. Action Plan

Falsifying experiment, read-only, no database writes, no reboot required.

Window: three matched idle periods on AC power, Photos not focused, each at least 60 minutes, on the same Mac, within seven days. Record OS build once.

Predicate, unified log, process `mediaanalysisd` and `photoanalysisd`: the exact sentence, plus nearby lines that already say analysis completed, gallery state saved, task started, or task finished. Do not collect image bytes, filenames that contain people’s names, or face labels. If a log line contains a path or UUID, hash it before it leaves the machine.

What to count, per window:

- Rejection lines, split by the pair of integers (`2` to `3737841669`, `3737841665` to `3737841669`, anything else).
- Unique hashed asset identifiers, not raw identifiers, and how many of those hashes appear in more than one burst.
- Whether a completion or gallery-save line falls inside the same minute.
- CPU time of `mediaanalysisd` and `photoanalysisd` at the start and end of the window, from Activity Monitor’s CPU Time column, so the delta is minutes of CPU rather than a percent spike.

Interpretation, decided in advance:

- Many rejections, almost all unique hashes, CPU-time delta small relative to library size, completions continuing: fits harmless discard of old embeddings during migration. Do not repair. Re-check once after 14 days.
- The same hashes in each hourly burst, with a long task span before each rejection and CPU time climbing by a large fraction of a core-hour per burst: fits repeated expensive recomputation. That is the observation that justifies a bug report as a defect rather than as a question. Still no cure claim.
- Rejections stop while CPU time stays high: the heat is a different workload. Do not attribute it to this sentence.
- A clean new library is silent in a matched window and the main library is not: the library or its synced derived state is implicated. Only then is Repair Library, after a complete backup, a reasonable supported step, and only as a trial with the same counters before and after.

Publication thresholds, each with the prior art it must beat:

- A safe diagnostic guide, widely useful: at least three households, not two machines in one home, with the same sentence, the hashed-asset method above, and a 14-day follow-up that separates “discard once” from “retry forever.” Prior art to cite: Apple’s people-and-pets timing article https://support.apple.com/en-us/105081, the Repair Library limits https://support.apple.com/en-us/118221, and the zerosleeps version-0 incident as a different signature. No prevalence percentage.
- A bug report: one well-logged machine is enough to file, because Apple asks for every issue. https://developer.apple.com/bug-reporting/. It becomes a public claim of a defect only after the repeated-hash, high-CPU-time pattern, on a build that is still current, with no Apple “works as designed” answer. Attach the sysdiagnose from the Mac, not a pasted photo.
- A mitigation: a reversible step with published before-and-after on the same counters, held for 14 days, and a stated relapse. Lowering priority is allowed in the brief as contention relief and must be labeled that way. Disabling daemons does not qualify.
- A permanent cure: the signature stays gone for 30 days, CPU time returns to idle baseline, People and search still function, and an independent second household reproduces the before-and-after. A macOS update that coincides with the end is not a cure unless the notes name it or the second household on the old build still shows the loop. Nothing opened in this run meets that bar.

Prior art for the recommended artifact, a short diagnostic note, not a fix post: Apple’s revision-mismatch error and the June 2023 engineer reply (https://developer.apple.com/forums/thread/732876), Apple’s “few days” analysis window (https://support.apple.com/en-us/105081), and the zerosleeps measurement design of pairing `log stream` with file-activity counts (https://zerosleeps.com/blog/2025/6/15/mediaanalysisd-has-gone-rogue/).

Evidence matrix

| Claim | Status | Exact signature? | Confidence | Source date |
| --- | --- | --- | --- | --- |
| Public Vision refuses cross-revision feature-print distance | VERIFIED via indexed Apple forum and docs | No, related API | [established] | Engineer reply June 2023; docs crawled 2026 |
| Private integers 3737841669 and 3737841665 have a public model map | Not found | Would be exact | UNKNOWN | Searched 13 November 2026 |
| Logged `2` equals Vision revision 2 | INFERRED and rejected | No | [speculative] if asserted | — |
| Household pair shows provided 2 to current 3737841669 | OBSERVED in the brief only | Yes | [single-source] household | 12 November 2026 window in the brief |
| 0.24 s bursts prove an infinite loop or the 8.99-core load | Not shown | No | Rejected | Brief |
| Version-0 line, repeated file opens, Sequoia 15.5, M1 | OBSERVED by that author | No, similar semantics | [single-source] | 15 June 2025, follow-ups to 31 October 2025 |
| macOS 26 quieted that author’s CPU, line remained | OBSERVED by that author | No | [single-source] | 17 September 2025 |
| Tahoe notes through 26.7.3 fix this sentence | Not found | No | [established] negative | Notes published 9 November 2026 |
| Repair Library fixes embedding revisions | Not claimed by Apple | No | Do not infer | Article 22 May 2024 |
| Disable or delete daemons is a cure | Anecdote, feature loss, relapse | No | Rejected as cure | Mole 3 November 2026 |

Search coverage and gaps

Searched and opened or indexed: the full sentence, `provided embedding revision`, `current embedding revision`, both ten-digit integers, `Embedding version: 0 not supported`, Vision revision docs, the June 2023 forum thread, Tahoe consumer notes through 26.7.3, Repair Library, People & Pets timing, Photos privacy, Enhanced Visual Search (https://support.apple.com/en-us/122033, opened, 15 September 2025), Feedback Assistant, `photoanalysisd` man wording, zerosleeps follow-ups, and the MediaAnalysis symbol diff.

Not found: any academic paper, Apple release note, or independent first-person post of the exact sentence. Direct fetches failed for the developer forum, the Apple Community thread, Stack Overflow, and the raw ipsw-diffs file. Those gaps are why the engineer quote, the version-0 Community counts, the 768-versus-2048 measurement, and the `revision 9/10` symbol string are marked indexed or single-source.

No prevalence percentage is possible. The public exact-match count outside this household is zero in this search. That is a coverage result, not a rate of occurrence.

OBSERVED: the brief’s log pairs, burst size and timing, successful completions, 8.99-core historical sample, and the zerosleeps version-0 counts. INFERRED: the log is a write guard analogous to Vision’s distance check. PROPOSED: the seven-day hashed-asset experiment. VERIFIED: Apple’s public revision mismatch behavior, the “few days” analysis window, Repair Library’s actual scope, and the absence of this sentence in the opened Tahoe notes.

No proven cure exists. Do not publish one.

H. Sources

Opened in full in this run:

https://support.apple.com/en-us/118221 (22 May 2024)

https://support.apple.com/en-us/105081 (18 March 2025)

https://www.apple.com/legal/privacy/data/en/photos/ (page date 12 December 2025)

https://support.apple.com/en-us/122033 (15 September 2025)

https://support.apple.com/en-nz/108793 (25 February 2026)

https://support.apple.com/en-us/122868 (9 November 2026)

https://developer.apple.com/bug-reporting/ (living page, fetched 13 November 2026)

https://zerosleeps.com/blog/2025/6/15/mediaanalysisd-has-gone-rogue/ (15 June 2025)

https://zerosleeps.com/blog/2025/7/30/does-macos-156-fix-mediaanalysisd/ (30 July 2025, updates through 31 October 2025)

https://zerosleeps.com/blog/2025/9/17/macos-26-fixes-my-mediaanalysisd-problem/ (17 September 2025)

https://mole.fit/blog/mediaanalysisd-high-cpu-mac (3 November 2026, updated 5 November 2026)

https://macosbin.com/bin/photoanalysisd (undated man-page mirror)

Indexed excerpts used because direct fetch was blocked or returned only a title or 404:

https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1

https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision2

https://developer.apple.com/forums/thread/732876 (June 2023)

https://stackoverflow.com/questions/77185128/is-vnfeatureprintobservation-and-vision-distance-computation-different-for-two (27 September 2023)

https://github.com/blacktop/ipsw-diffs/blob/main/26_5_23F77_vs_26_6_23G5028e/DYLIBS/MediaAnalysis.md (raw GET 404 on 13 November 2026)

https://developer.apple.com/forums/thread/824784 (April 2026, reply May 2026)

https://discussions.apple.com/thread/256084597 (June 2025, same author as zerosleeps)

https://discussions.apple.com/thread/255737983

https://discussions.apple.com/thread/255781588

https://discussions.apple.com/thread/256138728

https://eclecticlight.co/2022/10/07/is-apple-scanning-the-images-on-my-mac/ (7 October 2022)

https://developer.apple.com/documentation/macos-release-notes/macos-26-release-notes
