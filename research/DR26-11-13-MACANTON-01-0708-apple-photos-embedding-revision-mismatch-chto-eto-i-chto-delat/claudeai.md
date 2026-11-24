# DR26-11-13-MACANTON-01-0708 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: https://приватный чат
dr_id: DR26-11-13-MACANTON-01-0708
rail: claudeai
mode: claude.ai Research, Fable 5.2, effort Medium, org 7f07033b; connector Apollo.io was available to the run
topic: Apple Photos embedding revision mismatch
collected: 2026-11-14 06:29 WET

---

# Apple Photos Embedding Revision Rejections: Diagnosis, Evidence and Next Steps

# Apple Photos "embedding revision" rejections: what is known, what is not, and what to do next

Bottom line: no public source matches this exact log signature, and nobody has published a verified cure. The evidence fits a version guard that throws away stale or incompatible embeddings by design. It does not show database corruption. The right call is continued read-only diagnosis plus a minimal Apple Feedback report, not a Photos library repair or any internet "fix".

## TL;DR
- **This exact message has no public match.** Searches for the message, the phrase, and the decimal values 3737841669 and 3737841665 returned nothing. Apple has not acknowledged it, no release note mentions it, and no reproducible upstream issue exists. One thing is verifiable: 3737841664 is hex 0xDECAF000, so the two "current" values are 0xDECAF005 and 0xDECAF001. That looks like a private revision namespace, but what those values mean is UNKNOWN.
- **Apple's public Vision design supports the "harmless guard" reading.** Feature prints from different algorithm revisions cannot be compared, and an Apple frameworks engineer said older observations "must be re-processed with the newer revision". That is evidence for a migration guard, not proof. Whether the rejections in this case follow expensive recomputation is the open question.
- **What to do:** keep the Mac on power and let it idle, update macOS through normal channels, and file one minimal Feedback Assistant report with bounded log excerpts. Treat lower process priority as contention relief only. Do not run Repair Library unless Photos itself shows a user-visible problem, and only after a full library backup. Reject launchctl, SIP, container-deletion and SIGSTOP recipes as cures.

## A. Executive Summary

**Decision.** Classify this as **continued diagnosis plus a minimal bug report** [emerging]. It does not yet justify a repair, and calling it a cure would be unsupported. The recommendation rests on one assumption: the 620-record bursts are cheap discards of stale embeddings, not the tail end of repeated expensive feature computation. The experiment in Section G tests exactly that.

**Why not repair.** Apple Support article 118221 describes Repair Library as a fix for a library that won't open or behaves unexpectedly; the tool "analyses the library's database and repairs any inconsistencies it detects", and iCloud checks the library after the repair completes. The older article HT204967 says "the entire contents of the library re-updates with iCloud" after repair. Nothing observed here is a user-visible Photos failure. A revision mismatch is the normal output of a version check, not a sign of an inconsistent database. A repair would add re-sync and possibly re-analysis load, and it would not test the hypothesis.

**Why not "just wait" alone.** Three bursts of exactly 620 rejections, about an hour apart, could mean a fixed set of stale records being offered and refused on every scheduler pass. If so, waiting will not make them go away. A migration that is making progress should show counts falling over days. The count trend across days is the most decisive cheap observation available.

**Prevalence.** Unknown. The broad symptom of mediaanalysisd using a lot of CPU or storage is widely reported in 2025–2026; OSXDaily (8 January 2025, quoted by Michael Tsai) cited users with mediaanalysisd caches of "15GB+", some "50 GB, 80 GB, even 140GB", and said the trigger "is currently unknown". The exact signature has no public reports. No prevalence rate can be derived from forum activity, and this report does not offer one.

## B. Key Findings

### B1. What revisions mean (primary technical sources)
- **[established] Apple Vision versions its algorithms per request.** VNRequest exposes `revision`, `supportedRevisions`, `defaultRevision` and `currentRevision`. Observations carry the `requestRevision` that produced them. The API was added in iOS 12 / macOS 10.14 per Apple's SDK headers and API diffs. The image feature-print request has public constants Revision1 and Revision2 (Apple developer documentation).
- **[established] Embeddings from different revisions live in different vector spaces and are rejected rather than mixed.** In Apple Developer Forums thread 732876 (June 2023), comparing feature prints across revisions failed with `com.apple.vis Code=12` "The revision of the observations do not match". An Apple "Frameworks Engineer" replied that previously produced observations "must be re-processed with the newer revision". Secondary write-ups describe Revision1 as a 2048-float non-normalized vector and Revision2 as a 768-float normalized vector [single-source, low-authority: MWM on Medium, undated]. Different dimensions and normalization make cross-revision comparison meaningless, so refusing the insert is the safe behavior.
- **[established] Photos uses learned embeddings on-device.** Apple Machine Learning Research describes Photos using a shared image-language backbone trained with a contrastive objective for scene analysis and search, and face/upper-body embeddings clustered on-device for People. Model changes therefore plausibly invalidate stored vectors. This is inference about design, not about this log line.
- **[speculative] The private values.** The arithmetic is fixed: 3737841664 = 0xDECAF000, so 3737841665 = 0xDECAF001 and 3737841669 = 0xDECAF005. The research subagent searched SDK bindings, class-dumps and forums and found no public mapping of the 0xDECAF00x values. Class-dumps do not carry enum values, so this absence proves nothing. **Mapping status: UNKNOWN.** Do not assume that public Vision Revision2 equals the "provided 2" here, or that 0xDECAF005 names a specific Photos model.
- **[emerging] Cross-machine clue.** The MacBook (macOS 26.3.1(a)) and the Mac Studio (macOS 27.0.1, build 26A434) report the *same* current revision, 0xDECAF005. If both were running different model generations, the current values would differ. Identical values point toward stale *provided* data (legacy revision-2 records, or 0xDECAF001 records from an earlier private model) rather than an OS-specific runtime mismatch. Because the two Macs may share migrated or iCloud data, this is not an independent replication.

### B2. Search results, deduplicated by incident
Exact signature: **none found.** That covers the full message, the partial phrase "does not match the current embedding revision", both decimal values and their hex forms, on GitHub, Apple Community, Apple Developer Forums and the general web. The only hits for "embedding revision" were unrelated AI/RAG projects that use the same revision-pinning concept.

Similar semantic mismatch (not this case): Apple Developer Forums 732876, June 2023, iOS 17 beta, third-party app. Vision feature-print revision mismatch, Code=12. The reporter said it persisted even after wiping data. The thread does not record a resolution.

Generic CPU/thermal complaints (not exact-signature; first-person evidence only for the poster's own experience):
1. Apple Community thread 256138728 (macOS 26.0, MacBook Pro 16-inch): mediaanalysisd high CPU and overheating since the macOS 26 beta, persisting into release. Replies offered generic advice; no verified outcome.
2. Apple Community thread 256283166 (macOS Tahoe 26.4.1): Xcode/Simulator triggers mediaanalysisd. A second poster on an Intel 6-core iMac 5K reports more than 600% CPU that stops when Xcode quits. This is a distinct trigger (the Simulator's own photo library), relevant as a confounder.
3. Paul Hudson (twostraws) on X, quoted by Michael Tsai on 7 July 2025: mediaanalysisd "sitting on ~100% CPU for over a week", laptop "hot to the touch", and "Rebooting didn't help." The same post quotes Mastodon user @below (Full Report Below): com.apple.mediaanalysisd "using no less than 143GB. For a 27 GB photo library." Outcome not reported.
4. Jim Brown, comment on Tsai's post, 3 December 2025, Tahoe 26.1: CPU saturation after attaching a NAS drive. His workaround was keeping Photos open in both his and his wife's logins, which he says "prevents mediaanalysisd from running". The commenter's own attribution is speculative.
5. Ben Lovejoy, 9to5Mac, 3 June 2026, macOS 26.5: mediaanalysisd "variously using anything from 50% up to 150%", with kernel_task and corespotlightd. He wrote that "the only solution appears to be" connecting to power overnight and letting the machine get on with it.
6. 9to5Mac follow-up, 20 July 2026: beach balls and trackpad freezes. Michael Simon (Macworld) reported no recurrence for two weeks after deleting ~/Library/Metadata/CoreSpotlight. Lovejoy reported less than one day without recurrence. This is a Spotlight-index mitigation, not a mediaanalysisd or embedding fix [anecdotal].
7. Ask Different answer, June 2026, Tahoe 26.4.1, as relayed by pcglance (October 2026) [low-authority secondary]: about 85% average CPU over two idle hours. Reported calm after excluding a looping folder from Spotlight and restarting.
8. thejazzid/mediaanalysisd-watchdog README (tested on macOS 26.7, M2 Max) [single-source]: claims an analysis task can fail on every asset and retry nightly. It reports that resetting the analysis database "rebuilds, then fails on the same task again", which is a relapse report, and that `launchctl disable` is erased at reboot.

Indexing that eventually completed: older Apple Community threads (Big Sur era) describe People scanning progressing only after days on power. One poster reports success after involving Apple Support engineers. These threads are older than the 12–18 month window and say nothing about embeddings.

### B3. Competing explanations

| Hypothesis | Predicts | Distinguishing observation | Evidence against / status |
|---|---|---|---|
| H1 Normal model migration; stale derived cache discarded | Rejection counts fall across days; analysis progress counters rise; little CPU directly tied to bursts | Daily rejection totals decline; the set of rejected identifiers shrinks | Three equal 620-record bursts in one window fit poorly unless it is batch-capped; plausible, not shown |
| H2 Mixed runtime/model versions | Different "current" values across processes or Macs | Current value varies by process/OS | Both Macs show the same current value (0xDECAF005), which weakens H2 |
| H3 Failed regeneration (old vectors never replaced) | Same records rejected every pass; no matching "re-embedded" success | Identical identifier set per burst; constant count across days | Consistent with equal 620 counts; unverified because identity is unknown |
| H4 Unsupported/malformed media | Rejections cluster on specific assets/types; decode errors nearby | Errors next to specific asset UTIs, legacy media | Revision mismatch is a version check, not a decode failure; weak fit |
| H5 Persistent scheduler/retry defect | Periodic bursts at a fixed cadence with expensive work before each | CPU time rises just before each burst; count constant | Hourly cadence fits periodic scheduling; old AppleInsider plist notes list a 7200 s photos-analysis interval [single-source, Ventura-era]; cost before bursts unmeasured |
| H6 Unrelated CPU/thermal/power load | Heat persists when bursts absent; other processes dominate | CPU time attributed elsewhere; Intel i9 thermals | 8.99-core peak is historical and not linked to bursts; Spotlight/Xcode confounders documented in 2026 reports |

### B4. Interpretation of the 0.24 s bursts
Logging 620 rejections in about 0.24 s is consistent with a bulk insert pass that checks a revision field and refuses mismatches. That step is cheap. The decisive unknown is upstream: whether those 620 vectors were *freshly computed* (expensive, wasted work) or *read from storage* (cheap discard). Two signals separate these. One is CPU time accumulated by mediaanalysisd in the minutes before each burst. The other is whether the "provided" value is a legacy value (2, 0xDECAF001) that a freshly computing current model would never emit. **Inferred:** a fresh computation by the current model should carry the current revision. Provided values of 2 or 0xDECAF001 therefore suggest stored or legacy embeddings being re-offered rather than new computation. This favors H1/H3 over "expensive recompute" [speculative].

## C. Contradictions & Debates

- **"It finishes on its own" vs. "it loops forever."** 9to5Mac (June 2026) and many explainer pages advise leaving the Mac on power overnight. The watchdog README and iThinkDiff (low-authority) assert that some Macs never exit a stuck state. Steelman for "loop": the watchdog author reports a reset followed by the same failure, which is a concrete relapse observation. Steelman for "finishes": Apple's documented design is opportunistic background analysis, and most reports lack long follow-up. Neither side has measured data tied to this signature.
- **Spotlight deletion as a "fix."** Simon reports two weeks with no recurrence. Lovejoy had less than a day of follow-up. It targets CoreSpotlight, not Photos embeddings. Whatever it does for beach balls, it cannot speak to revision rejections.
- **Cache deletion.** Six Colors (June 2025) describes trashing the mediaanalysisd cache to reclaim space. Mole and others warn that deleting the whole container restarts analysis from scratch. Both can be true. Space comes back, the CPU cost recurs, and Apple does not document either outcome.
- **Repair Library.** Community posters sometimes report that repair restored People scanning. Apple frames it narrowly as a fix for database inconsistencies and warns users to back up first. No source ties it to embedding revisions.

## D. Best Practices

For an ordinary Mac owner (plain-language version of question 5):

1. **Let it work (first choice).** Leave the Mac plugged in, awake, with Photos closed, overnight for a few nights. Photos' own People view has long said scanning continues "when you're not using the app and your Mac is connected to power". *Needs:* power, time. *Extra processing:* none beyond what is already queued. *iCloud:* none. *Risk:* none to photos. *Does not show:* that a stuck task will ever finish.
2. **Update macOS** through System Settings > General > Software Update. *Needs:* a current backup. *Extra processing:* likely. New OS versions can bring new models and trigger re-analysis. *iCloud:* none directly. *Risk:* low. *Does not show:* that any update fixed this, unless the rejections stop and stay stopped afterwards.
3. **Quit and reopen Photos.** Harmless, and it only pauses analysis while Photos is open. *Does not show:* anything about the root cause.
4. **Test with a separate clean library.** Hold Option while opening Photos and create a new empty library, or use a separate macOS user account. *Needs:* care to switch back to the original as the System Photo Library. *iCloud:* do not make the test library the System Photo Library on an iCloud-synced account, because that can change what syncs. *Risk:* confusion about which library is active. *Shows:* whether the symptom follows the library (data) or the Mac (software). *Does not show:* which records are stale.
5. **Repair Library (only for visible Photos problems).** Quit Photos, hold Option-Command while opening it, click Repair. *Needs:* a complete, tested backup of the Photos library (Time Machine or a copy to an external drive) and possibly an admin password. *Extra processing:* the repair can take a long time on a large library. With iCloud Photos, the library is re-checked against iCloud afterwards. *Risk:* low per Apple, but it is a database operation on irreplaceable data. *Does not show:* that revision rejections are corruption.
6. **Apple Support / Feedback Assistant.** File one report with a short description, the macOS version, the exact log line, and the time window. Attach a diagnostic only if you are comfortable sharing it, because diagnostics can include personal metadata. *Risk:* privacy exposure from attachments. *Does not show:* a timeline for a fix.

For the technical operator: work read-only. Do not write to private databases. Do not disable SIP, modify launchd, or delete containers. Keep every artifact free of image content, faces, and names.

## E. Failure Modes

- **Destructive "one-line fixes" (reject).** AppleInsider's older guide tells readers to disable SIP and delete or edit the mediaanalysisd LaunchAgent plist. On the sealed system volume of current macOS this is unsupported, it weakens security, and it disables Live Text and search. The huksley gist that moves plists out of /System has the same problem.
- **launchctl disable/kill (reject as a cure).** Per the watchdog README, disable is erased at reboot and kill is undone when launchd relaunches the daemon. At best it masks the symptom.
- **SIGSTOP watchdogs (mitigation only, with costs).** The process gets frozen indefinitely. Photos search, People and Memories stop updating, and memory stays resident. It is reversible, but it means installing software, which is out of scope here, and it shows nothing about the cause.
- **Container/cache deletion.** Triggers full re-analysis (more CPU), and Apple does not document it. One reset-then-relapse report exists.
- **Content-farm cure lists.** pcglance, coreguard, positioniseverything and similar pages blend real reports with generic steps and give no measured outcomes [low-authority].
- **Misattribution.** Spotlight indexing, the Xcode Simulator, NAS volumes and thermal limits on Intel i9 laptops all produce similar heat. Without CPU-time attribution, a "cure" may have fixed something else.
- **Counting errors.** Treating the two family Macs as independent, or treating forum "me too" counts as prevalence.

## F. Strategic Recommendations

**Options**
1. **Wait and observe (passive).** Zero risk. Cannot tell H1 from H3. Could waste weeks if it is a loop.
2. **Read-only diagnosis plus minimal Feedback report (recommended).** Low risk, produces falsifiable data, and gives Apple a precise signature. Costs operator time, and Apple may not respond.
3. **Repair Library after a full backup.** Moderate cost: hours of work plus iCloud re-check. Low data risk with a backup. Does not test the hypothesis, and may reset state and hide evidence.
4. **Clean-library or second-account A/B.** Good at separating data from software. Some risk of a System Photo Library or iCloud mix-up.
5. **Internet daemon/cache hacks.** Reject. They carry security or functional cost and have no validated benefit for this signature.

**Single recommendation:** Option 2, then Option 4 if counts stay constant over 7 days. Confidence: [emerging]. Main assumption: the rejection bursts themselves are cheap, so the urgent question is whether expensive work precedes them.

## G. Action Plan (falsifiable)

**OBSERVED** (from the brief): repeated rejections of provided 2 or 0xDECAF001 against current 0xDECAF005; three 620-record bursts about an hour apart; successful analysis completions alongside; a historical 8.99-core peak.
**INFERRED:** a version-guard discard; the same current revision on both Macs; legacy provided values suggest stored rather than freshly computed vectors.
**PROPOSED:** below.
**VERIFIED:** only the hex arithmetic and Apple's public revision semantics.

**Experiment 1: is this migration or a loop? (read-only, 7 days)**
- Once a day, run the same bounded `log show` window, filtered on the process and the "embedding revision" string. Record: number of bursts, records per burst, distinct provided values, and timestamps. If asset identifiers appear unredacted, store salted hashes only.
- *Migration (H1) is supported if* daily rejection totals fall monotonically, or the hashed identifier set shrinks.
- *Failed regeneration or loop (H3/H5) is supported if* the count stays at about 620 per burst with an identical hash set for 7 or more days.

**Experiment 2: is expensive work happening before each rejection?**
- Sample cumulative CPU time for mediaanalysisd every 60 s with `ps -o time`, and use Activity Monitor's CPU Time column for non-technical owners. Align the samples with burst timestamps.
- *Harmless discard is supported if* the CPU delta in the 10 minutes before a burst is indistinguishable from comparable windows without bursts.
- *Wasteful recompute is supported if* CPU rises consistently before bursts and the wasted share is a measurable fraction of daily mediaanalysisd CPU.
- Also log successful-completion messages and the Photos UI progress indicators, so that "work completed" is measured and not assumed.

**Experiment 3: does the symptom follow the data or the machine?**
- A clean test library on the same Mac. Watch whether rejections appear for it. They should not if stale stored embeddings are the source.

**Publication thresholds (proposed)**
- *Safe diagnostic guide:* Experiments 1–2 reproduced on at least two Macs with **verified independent** accounts and libraries, using GUI-equivalent steps, with no destructive operations.
- *Bug report:* justified **now** with the existing evidence. Update it with the results of Experiments 1–2.
- *Mitigation claim (e.g., lower priority):* an A/B over at least 7 days per arm with comparable usage and power state. Report the reduction in mediaanalysisd CPU time and in heat/fan events, plus the delay to analysis completion.
- *Permanent cure claim:* the rejections and the excess CPU stop and stay stopped for at least 30 days, across at least one macOS update, on at least two independent Macs. A plausible mechanism and a control that did not receive the intervention are also required. Absent these, write "no proven cure".

## Evidence Matrix

| Claim | Exact match? | Confidence | Source (date) |
|---|---|---|---|
| Vision revisions incomparable; must re-process | Semantic only | [established] | Apple Developer Forums 732876, Apple engineer (June 2023) |
| Revision API semantics | No | [established] | Apple Vision docs / SDK headers (iOS 12 / macOS 10.14 era) |
| Photos uses on-device embeddings | No | [established] | Apple ML Research (scene analysis; People recognition) |
| 0xDECAF00x meaning | No | UNKNOWN | No public source found |
| mediaanalysisd high CPU on macOS 26.x | No | [established] as a symptom | 9to5Mac (3 Jun 2026); Apple Community 256138728; Tsai (7 Jul 2025) |
| Reset relapses | No | [single-source] | watchdog README (2026, macOS 26.7) |
| Spotlight deletion stops beach balls | No | [anecdotal] | 9to5Mac (20 Jul 2026) |
| Repair tool scope and iCloud re-check | No | [established] | Apple Support 118221 / HT204967 |
| Exact-signature report or Apple fix | — | None found | — |

## Caveats
- Search coverage: about 18 web searches plus one subagent pass, covering the exact message, the phrase, decimal and hex values, mediaanalysisd/photoanalysisd symptoms, Vision revision docs, Apple support and community pages. Gaps: Reddit and MacRumors were not searched directly; Apple's undocumented internal frameworks were not disassembled; macOS 27 release notes were not reviewed.
- Several explainer sites may be AI-generated or SEO-driven. They were used only to locate first-person reports, never as mechanism evidence.
- Some Apple research and support pages were seen through snippets, so exact publication dates were not always captured.

## H. Sources
Sources are named inline with their dates: Apple Developer Forums 732876; Apple Vision documentation and SDK headers; Apple Machine Learning Research (on-device scene analysis; recognizing people; homomorphic encryption and Enhanced Visual Search); Apple Support 118221 and HT204967 and the Photos User Guide repair page; Apple Community threads 256138728, 256283166, 251551270, 252103789 and 8155926; 9to5Mac (3 June 2026; 20 July 2026); Michael Tsai's blog (7 July 2025, with comments dated 3 December 2025 and 3 June 2026, the latter from Brook McEachern, who reported a workaround after "weeks (months?)" of mediaanalysisd CPU use and linked Ask Different answer apple.stackexchange.com/a/486537); OSXDaily (8 January 2025, as quoted by Tsai); Six Colors (June 2025); AppleInsider's mediaanalysisd guide (Ventura era); the thejazzid/mediaanalysisd-watchdog README; the huksley gist; and the low-authority explainers pcglance, coreguard, Mole and iThinkDiff. No source in this report is invented. Where a claim lacks a source it is marked UNKNOWN or speculative.

## Chat summary (assistant text around the artifact)

Starting the deep research investigation now, working from your brief as written.

I couldn't find a public report of this exact "embedding revision" message, and nobody has published a verified fix. The evidence fits a version check that throws away old embeddings on purpose, not a corrupted database. So I'd recommend read-only diagnosis plus a short Apple Feedback report rather than Repair Library or any of the internet fixes.

- **No match anywhere.** Searches for the full message, the phrase, and both numbers in decimal and hex turned up nothing. Apple hasn't acknowledged it and no release note mentions it. The one thing I could confirm is that the numbers are hex 0xDECAF001 and 0xDECAF005. That looks like a private Apple version scheme, but nothing public says what they mean.
- **Why it looks like a guard, not damage.** Apple's Vision documentation says image fingerprints made by different model versions can't be compared. An Apple engineer said older ones "must be re-processed with the newer revision". Both Macs report the same current version (0xDECAF005), which suggests old stored data is being rejected rather than the two OS versions disagreeing. The Macs may share data, though, so they don't confirm each other independently.
- **The open question.** Three bursts of exactly 620 rejections about an hour apart could be a normal migration. They could also be the same stuck records refused on every pass, which waiting won't fix. The 0.24-second bursts themselves are cheap; what matters is whether the Mac does expensive work right before each one.
- **What to do now:** keep the Mac plugged in and idle, update macOS normally, and file one short Feedback report with limited log excerpts. Only run Repair Library if Photos shows a visible problem, and only after a full backup. Don't use the online "fixes" that disable SIP, use `launchctl`, delete caches or containers, or freeze the process with SIGSTOP. None of them has worked for this, and they cost security, features or a full re-analysis.
- **Tests to run next:**
  1. Count the daily rejections for 7 days. Falling counts mean a migration; a steady ~620 means a loop.
  2. Check mediaanalysisd's CPU time just before each burst.
  3. Try a clean test library to see whether the problem follows the data or the Mac.

The report also includes an evidence table, the competing explanations with what would tell them apart, step-by-step options in plain language for ordinary Mac owners, and the bar for calling something a confirmed cure.

The search had gaps: Reddit, MacRumors and the macOS 27 release notes weren't checked.

## Sources (md_citations of the artifact)
- https://developer.apple.com/forums/thread/732876
- https://github.com/xybp888/iOS-SDKs/blob/master/iPhoneOS13.0.sdk/System/Library/Frameworks/Vision.framework/Headers/VNRequest.h
- http://codeworkshop.net/objc-diff/sdkdiffs/ios/12.0/Vision.html
- https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision2
- https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1?changes=latest_m__1_4__8_5_7_1&language=objc
- https://medium.com/@MWM.io/apples-vision-framework-exploring-advanced-image-similarity-techniques-f7bb7d008763
- https://hackernoon.com/how-i-built-an-on-device-trading-card-scanner-with-apple-vision
- https://machinelearning.apple.com/research/on-device-scene-analysis
- https://machinelearning.apple.com/research/recognizing-people-photos
- https://github.com/MarcelRoozekrans/Rag.NET/issues/607
- https://github.com/xurichard19/strength-and-conditioning-rag/pull/76
- https://discussions.apple.com/thread/256138728
- https://discussions.apple.com/thread/256283166
- https://9to5mac.com/2026/07/20/mac-beach-balls-and-unresponsive-trackpad-this-fix-seems-to-work/
- https://pcglance.com/mediaanalysisd-high-cpu-mac/
- https://github.com/thejazzid/mediaanalysisd-watchdog
- https://discussions.apple.com/thread/251551270
- https://discussions.apple.com/thread/252103789?page=3
- https://appleinsider.com/inside/macos-ventura/tips/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos
- https://macstats.app/blog/what-is-photoanalysisd
- https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/
- https://www.ithinkdiff.com/macos-26-5-beach-ball-fix-mediaanalysisd/
- https://mole.fit/blog/mediaanalysisd-high-cpu-mac
- https://sixcolors.com/post/2025/06/reducing-suboptimal-icloud-photos-storage-sizes/
- https://purplelink.llc/guides/mac-system-data-huge/
- https://discussions.apple.com/thread/254207907
- https://support.apple.com/en-us/118221
- https://www.pressreader.com/australia/apc-australia/20161201/284417033585194
- https://dev.to/gjbianco/controlling-photoanalysisd-3j2a
- https://support.apple.com/guide/photos/repair-your-photo-library-pht6be18f93/mac
- https://gist.github.com/huksley/564be2c903312bcee7dffe415d128f90
- https://coreguard.app/insights/what-is-mediaanalysisd-mac/
- https://www.positioniseverything.net/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos-2/
- https://discussions.apple.com/thread/252835211
- https://mjtsai.com/blog/2025/07/07/fixing-mediaanalysisd-storage-and-cpu-use/
- https://nitin-bodke.medium.com/fixing-high-cpu-usage-and-system-slowdowns-on-macos-by-disabling-spotlight-f96ab10fb4b7
- https://support.apple.com/en-asia/HT204967
