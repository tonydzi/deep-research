# DR26-11-13-MACANTON-01-0708 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: https://приватный чат
dr_id: DR26-11-13-MACANTON-01-0708
rail: chatgpt
mode: ChatGPT Pro, Deep research chip, Thinking effort Extra High; report from widget_state
topic: Apple Photos embedding revision mismatch
collected: 2026-11-14 06:29 WET

---

# DR26-11-13-MACANTON-01-0708 — Apple Photos embedding revisions, CPU load, and safe remediation

## A. Executive Summary

**Bottom line: [emerging] the exact log signature is strong evidence of a version-compatibility guard rejecting a representation that Photos considers stale or otherwise incompatible, but it is not, by itself, evidence of Photos-library corruption, an infinite retry loop, or the cause of sustained high CPU.** Apple publicly documents the same architectural concept in Vision: requests and observations carry algorithm revisions, newer OS/framework releases can change those revisions, and different revisions represent different implementations of an algorithm. Apple also describes Photos as computing large numbers of local feature-vector “embeddings” and periodically rebuilding or clustering them. ([Apple, “VNGenerateImageFeaturePrintRequestRevision1,” undated](https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1); [Apple ML Research, “Recognizing People in Photos Through Private On-Device Machine Learning,” 2021](https://machinelearning.apple.com/research/recognizing-people-photos)) citeturn617076search4turn864552search10

**The exact private values `2`, `3737841665`, and `3737841669` cannot currently be mapped to public Vision request revisions or to named Photos models. [established] Public mapping: UNKNOWN.** Searches of Apple documentation, Apple Developer Forums, Apple Support Communities, indexed source-code repositories, general web search, Reddit, MacRumors, and exact-number searches found no authoritative mapping for those decimal identifiers and no public Apple document tying them to `VNGenerateImageFeaturePrintRequestRevision2` or any other public Vision revision. Apple’s public feature-print API, in fact, exposes small request-revision integers such as Revision 1, which is a separate namespace unless Apple documents otherwise. ([Apple, Vision request revision documentation, undated](https://developer.apple.com/documentation/vision/vnrequest); [Apple, feature-print revision documentation, undated](https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1)) citeturn617076search8turn617076search4

**No public exact-signature incident or validated cure was found in the searched material as of 14 November 2026. [established]** Searches for the full phrase `"Unable to add embedding: provided embedding revision"`, its shorter semantic forms, and each decimal revision did not return a relevant Apple/Photos incident. The `3737841665` search produced unrelated telephone-number matches, illustrating why raw decimal searching is noisy rather than evidence of prevalence. This absence is a search result, not proof that nobody else has encountered the condition. [single-source insofar as it reflects this research run; no external prevalence source exists]

**The two family Macs should be treated as one possibly shared incident, not two independent reproductions.** That follows directly from the supplied measurement context: the libraries/accounts may share migrated or iCloud-derived state. The important technical observation is therefore **cross-architecture persistence within one potentially shared data lineage**, not prevalence.

**The safest decision today is continued diagnosis plus a high-quality Apple bug report, rather than immediate Repair Library. [emerging]** Apple explicitly recommends Repair Library when Photos behaves unexpectedly, but says the tool analyzes and repairs database inconsistencies, warns users to back up first, and notes that iCloud rechecks the repaired library afterward. Nothing currently establishes a database inconsistency here. ([Apple Support, “How to use the Photos Repair Library tool on your Mac,” undated/current](https://support.apple.com/en-ie/118221)) citeturn613637search0 A repair could therefore be useful later as an A/B intervention, but performing it now would destroy diagnostic information and could trigger extensive reprocessing without proving the root cause.

**Normal post-upgrade or model-migration work remains plausible, but the periodic repeated mismatch bursts make a retry/scheduling defect plausible enough to investigate. [emerging]** Apple explicitly says Photos can take hours or days to re-index large libraries and that recognition proceeds while the Mac is not actively using Photos; public research on embedding systems also shows that replacing an embedding model often requires “backfilling” previously stored vectors because old and new vector spaces need not be mutually comparable. ([Apple Support, “If you can’t find pets in the People & Pets album in Photos,” 2025](https://support.apple.com/en-us/105081); Shen et al., CVPR 2020](https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html)) citeturn864552search0turn164274search1 The unresolved question is whether your machine is **making migration progress** or **recomputing and rejecting the same expensive work**.

**Recommended option: “Instrument first, report now, repair only after a controlled baseline.” [emerging]** The main assumption is that preserving evidence from the currently reproducible state is more valuable than an unvalidated intervention. Apple’s Feedback Assistant guidance specifically asks for reproduction steps, exact times, sysdiagnose, and performance evidence; that matches this case unusually well. ([Apple Developer, Feedback Assistant, undated](https://developer.apple.com/feedback-assistant/); [Apple Developer, “How to file great bug reports,” 2 June 2022](https://developer.apple.com/news/?id=vvrgkboh)) citeturn531143search0turn531143search6

## B. Key Findings

### Evidence matrix

| Finding | Classification | Confidence | Exact-match status | Evidence/date |
|---|---|---:|---|---|
| Photos uses embeddings/feature vectors for face/person recognition and grouping. | VERIFIED | [established] | Mechanistic, not exact log | Apple ML Research, 2021; [source](https://machinelearning.apple.com/research/recognizing-people-photos) citeturn864552search10 |
| Apple frameworks expose algorithm/request revisions because implementations change over OS/framework releases. | VERIFIED | [established] | Semantic match | Apple Vision docs, undated; [source](https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1) citeturn617076search4 |
| Stored embeddings from different models/revisions need not be directly comparable; model upgrades commonly require re-embedding/backfill unless explicit backward compatibility is engineered. | VERIFIED in general ML systems; INFERRED for private Photos representation | [established] general / [speculative] Photos-specific | Semantic match | Shen et al., CVPR 2020; Pan et al., CVPR 2023; Jang & Lim, ICCV 2025; [2020](https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html), [2023](https://openaccess.thecvf.com/content/CVPR2023/html/Pan_Boundary-Aware_Backward-Compatible_Representation_via_Adversarial_Learning_in_Image_Retrieval_CVPR_2023_paper.html), [2025](https://openaccess.thecvf.com/content/ICCV2025/html/Jang_Towards_Cross-modal_Backward-compatible_Representation_Learning_for_Vision-Language_Models_ICCV_2025_paper.html) citeturn164274search1turn164274search2turn164274search3 |
| Public mapping of `2`, `3737841665`, or `3737841669` to named Apple Photos/Vision models. | SEARCHED; none found | [established] that mapping was not found; mapping itself UNKNOWN | **No public mapping found** | Exact phrase/value searches, 14 Nov 2026; [Apple public Vision namespace for contrast](https://developer.apple.com/documentation/vision/vnrequestrevisionproviding) citeturn617076search9 |
| Public report containing the exact provided-2 → current-3737841669 signature. | SEARCHED; none found | [established] for this search corpus | **No public exact match found** | Apple/Developer/Web/Reddit/MacRumors searches, 14 Nov 2026. [no source found for an external matching incident] |
| Generic `mediaanalysisd` high-CPU reports exist on several macOS generations. | VERIFIED as anecdotal incident evidence | [established] | Not exact | Reddit Feb 2025; MacRumors 2024–2026; 9to5Mac June 2026. [Reddit](https://www.reddit.com/r/MacOS/comments/1io0zdb), [MacRumors](https://forums.macrumors.com/threads/macos-tahoe-26-2-bug-fixes-changes-and-more.2474608/page-3), [9to5Mac](https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/) citeturn235624reddit11turn235624search6turn235624search0 |
| Some indexing incidents eventually settle after analysis/update/waiting. | OBSERVED in first-person reports only | [emerging] | Not exact | MacRumors 2024 and Reddit 2020; [MacRumors](https://forums.macrumors.com/threads/com-apple-mediaanalysisd-filling-out-my-entire-ssd-on-my-mac-mini-what-to-do.2444392/), [Reddit](https://www.reddit.com/r/MacOS/comments/e6o4xk) citeturn648357search6turn648357reddit8 |
| Repair Library is an official Apple tool that rebuilds/analyzes the Photos database; backup is required/recommended and iCloud validates afterward. | VERIFIED | [established] | Remediation, not exact | Apple Support, undated/current; [source](https://support.apple.com/en-ie/118221) citeturn613637search0 |
| macOS 27 is Apple-silicon-only; the 2019 Intel MacBook Pro remains on macOS 26-family support. | VERIFIED | [established] | Platform constraint | Apple Support, 14 Sep 2026; [macOS 27 compatibility](https://support.apple.com/en-us/127255), [macOS 26 compatibility](https://support.apple.com/en-gb/122867) citeturn748325search1turn748325search3 |
| No Apple release note found acknowledging this exact embedding mismatch. | SEARCHED; none found | [established] for examined releases | **No exact acknowledgement** | Apple macOS 26.3 and 27 release notes; [26.3](https://developer.apple.com/documentation/macos-release-notes/macos-26_3-release-notes), [27](https://developer.apple.com/documentation/macos-release-notes/macos-27-release-notes) citeturn311592search1turn311592search2 |

### What “revision” most likely means

**VERIFIED:** In Apple Vision, `revision` denotes the specific version of an algorithm or implementation used by a request. Apple says revisions can increment when an algorithm changes in recognition speed, accuracy, supported languages, or related behavior, and exposes `currentRevision`, `defaultRevision`, and `supportedRevisions`. ([Apple Vision, undated](https://developer.apple.com/documentation/vision/vnrequest); [Apple feature-print revision docs, undated](https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1)) citeturn617076search8turn617076search4

**VERIFIED:** Apple Photos internally represents detected people using learned feature vectors, explicitly calling them “embeddings,” and constructs clusters/galleries from distances among those vectors. Apple reports that these computations run locally and that clustering runs periodically. ([Apple ML Research, 2021](https://machinelearning.apple.com/research/recognizing-people-photos)) citeturn864552search10

**INFERRED [emerging]:** A message saying an incoming embedding revision “does not match the current embedding revision” is structurally consistent with a storage-layer or pipeline guard refusing to mix representations from incompatible model generations. That is a normal defensive design in vector systems: without compatibility guarantees, distances between vectors produced by different models can be meaningless. The retrieval literature calls regeneration of old vectors after a model upgrade “backfilling”; several papers exist specifically because avoiding such backfills is difficult. ([Shen et al., CVPR 2020](https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html); [Pan et al., CVPR 2023](https://openaccess.thecvf.com/content/CVPR2023/html/Pan_Boundary-Aware_Backward-Compatible_Representation_via_Adversarial_Learning_in_Image_Retrieval_CVPR_2023_paper.html)) citeturn164274search1turn164274search2

**NOT VERIFIED:** We cannot say that Photos’ `3737841669` is a Vision request revision, a model hash, a schema revision, a model-asset identifier, a deployment generation, or a composite value. `[no source found]` The magnitude and private log wording make a direct equivalence to public Vision revision 2 particularly unjustified.

### Search for comparable incidents, deduplicated by incident

| Incident | Date / hardware / OS | Symptom | Evidence | Outcome | Category |
|---|---|---|---|---|---|
| Current supplied family incident | Nov 2026; Intel MBP16,1 macOS 26.3.1(a); M4 Max Mac Studio macOS 27.0.1 | Exact `2 → 3737841669`; occasional `3737841665 → 3737841669`; repeated bursts | User-provided logs | Unresolved | **Exact signature** |
| Reddit M1 Air incident | 12 Feb 2025; M1 MacBook Air; Sequoia 15.3.1 | `mediaanalysisd` 50% CPU, ~90% GPU, repeated heat after update | First-person post; [source](https://www.reddit.com/r/MacOS/comments/1io0zdb) | No validated cure in thread excerpt | Generic CPU/thermal [low-authority] citeturn235624reddit11 |
| MacRumors external-library incident | Dec 2024; hardware partly unspecified in quoted exchange; macOS 15.0→15.1 | High CPU scanning ~35k Photos assets; later settled | First-person forum report; [source](https://forums.macrumors.com/threads/com-apple-mediaanalysisd-filling-out-my-entire-ssd-on-my-mac-mini-what-to-do.2444392/) | Reporter could not distinguish waiting, intervention, or OS update | Indexing eventually completed [low-authority] citeturn648357search6 |
| Same MacRumors thread, OP | Dec 2024; Mac mini | Repeated writes filling SSD; high CPU around added photos | First-person report; same [source](https://forums.macrumors.com/threads/com-apple-mediaanalysisd-filling-out-my-entire-ssd-on-my-mac-mini-what-to-do.2444392/) | Unresolved in captured excerpt | Suspected retry/storage defect [low-authority] citeturn648357search6 |
| MacRumors Tahoe 26.1/26.2 | 2025–2026; hardware unspecified | `mediaanalysisd` >200% CPU for weeks; force-quit relapsed | First-person post; [source](https://forums.macrumors.com/threads/macos-tahoe-26-2-bug-fixes-changes-and-more.2474608/page-3) | Force quitting did not persist; another poster linked own case to incomplete iCloud restore | Generic CPU/sync interaction [low-authority] citeturn235624search6 |
| 9to5Mac author incident | 3 Jun 2026; hardware not stated in retrieved excerpt; macOS 26.5 | Beachballs, battery use; `mediaanalysisd` 50–150% CPU | Named first-person author observation; [source](https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/) | Recommendation was leave powered overnight; article did not establish exact cause or cure | Generic CPU [single-source] citeturn235624search0 |
| Older photoanalysisd completion report | Feb 2020; ~90k photos + 4k videos | Hundreds of % CPU while idle | First-person Reddit report; [source](https://www.reddit.com/r/MacOS/comments/e6o4xk) | Claimed analysis completed in ~14–15 h | Normal indexing completion [low-authority] citeturn648357reddit8 |

These reports **must not be aggregated into a prevalence estimate**. They differ in daemon, OS generation, Photos-library state, iCloud status, storage behavior, and outcome; none of the public incidents above contains the exact revision signature. Forum reply counts, votes, “Me Too” counts, or search-result counts would not provide a defensible denominator.

### Academic and industry prior art

The relevant research literature strongly supports one narrow architectural point: **model upgrades create vector-compatibility problems.** FaceNet introduced learned embedding spaces in which geometric distance represents similarity; ArcFace improved discriminative face embeddings; Apple’s Photos research describes a similar embedding-and-clustering pipeline. ([Schroff et al., CVPR 2015](https://openaccess.thecvf.com/content_cvpr_2015/html/Schroff_FaceNet_A_Unified_2015_CVPR_paper.html); [Deng et al., CVPR 2019](https://openaccess.thecvf.com/content_CVPR_2019/html/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.html); [Apple ML Research, 2021](https://machinelearning.apple.com/research/recognizing-people-photos)) citeturn164274search4turn164274search0turn864552search10

Backward-compatible representation learning is an active research field precisely because replacing a deployed embedding model normally forces recomputation of previously indexed items. Shen et al. explicitly describe re-computation as “backfilling”; 2023 work extends compatibility methods for retrieval, and a 2025 ICCV paper extends backward compatibility to vision-language embeddings. ([CVPR 2020](https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html); [CVPR 2023](https://openaccess.thecvf.com/content/CVPR2023/html/Pan_Boundary-Aware_Backward-Compatible_Representation_via_Adversarial_Learning_in_Image_Retrieval_CVPR_2023_paper.html); [ICCV 2025](https://openaccess.thecvf.com/content/ICCV2025/html/Jang_Towards_Cross-modal_Backward-compatible_Representation_Learning_for_Vision-Language_Models_ICCV_2025_paper.html)) citeturn164274search1turn164274search2turn164274search3

Open-source photo systems expose this migration problem more visibly than Apple. PhotoPrism’s current documentation states that libraries keep their existing face model because vectors generated by different models cannot simply be compared, and changing models requires a deliberate migration that re-embeds faces. ([PhotoPrism, current documentation, undated](https://docs.photoprism.app/getting-started/updates/)) citeturn145204search16 This is **not evidence that Apple Photos works identically**, but it is useful industry prior art supporting versioned embeddings as a normal engineering concern.

## C. Contradictions & Debates

### Expected migration versus persistent defect

The strongest case for **“expected migration”** is that Apple documents several forms of Photos analysis as background, potentially multi-day work; model/revision changes are normal in ML systems; and the supplied logs include successful analysis completions and gallery-state saves rather than a globally stalled pipeline. ([Apple Support, 2025](https://support.apple.com/en-us/105081); [Apple ML Research, 2021](https://machinelearning.apple.com/research/recognizing-people-photos)) citeturn864552search0turn864552search10 The user-supplied three-burst cadence could simply represent scheduled batches encountering stale derived records while other work advances.

The strongest case for **“persistent defect”** is that identical mismatch classes recur many times, including values differing by only four (`3737841665` versus `3737841669`), and the supplied 24-hour sample produced thousands of rejection lines. A robust migration would ideally consume stale data and converge rather than repeatedly encounter the same incompatible state. However, because rejected asset identity and upstream CPU cost are unknown, the same evidence is also compatible with many distinct stale records being encountered once each. `[no source found]` for either interpretation of these specific private values.

A useful falsifier is therefore **identity-aware recurrence**, not raw log count. If the same stable privacy-preserving task/asset identifiers repeatedly undergo a long compute phase before the same rejection, that materially strengthens the scheduler/retry-defect hypothesis. If rejection identifiers are mostly unique and the residual set shrinks while successful-completion counters increase, migration is the better explanation.

### Revision rejection versus corruption

**The mismatch itself does not establish corruption. [established]** Apple’s public APIs deliberately expose revisioned algorithms, which means revision differences can arise from normal software evolution. ([Apple Vision docs, undated](https://developer.apple.com/documentation/vision/vnrequest)) citeturn617076search8 Apple’s official Repair Library documentation frames repair around database inconsistencies or unexpected Photos behavior; it does not say that a private analysis revision mismatch is evidence of a damaged database. ([Apple Support, undated/current](https://support.apple.com/en-ie/118221)) citeturn613637search0

The opposing interpretation deserves consideration: a stale database row that should have been regenerated but remains permanently scheduled could represent a local derived-state inconsistency that Repair Library might indirectly clear. That remains **[speculative]** because Apple has not documented what derived analysis stores Repair Library rebuilds, preserves, or invalidates. `[no source found]`

### High CPU versus the mismatch

The current evidence does **not** support “the mismatch caused 8.99 logical cores of work.” Public reports confirm that `mediaanalysisd` can consume substantial CPU during legitimate analysis, particularly after imports or OS changes, but those incidents do not connect CPU time to this exact log message. ([9to5Mac, 3 June 2026](https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/); [Reddit, 12 February 2025](https://www.reddit.com/r/MacOS/comments/1io0zdb)) citeturn235624search0turn235624reddit11

The 0.24-second visible logging bursts particularly cannot be used as a bound on feature-extraction cost: the log may record only final insertion attempts after computation elsewhere. Conversely, a burst of rejection messages could be extremely cheap if the pipeline validates revision metadata before expensive work. The decisive measurement is **CPU-time attribution around each task**, not wall time of log output.

### Intel versus Apple silicon

The exact signature on the Intel Mac and M4 Max system weakens explanations tied solely to Intel thermals or Intel-only inference code, but it does not create two statistically independent incidents because the Photos/iCloud lineage may be shared. The platform distinction is operationally important because macOS 27 is Apple-silicon-only; Apple lists the 2019 16-inch Intel MacBook Pro as supported by macOS 26 but excludes Intel Macs from macOS 27. ([Apple macOS 26 compatibility, 2026](https://support.apple.com/en-gb/122867); [Apple macOS 27 compatibility, 14 September 2026](https://support.apple.com/en-us/127255)) citeturn748325search3turn748325search1

That creates a valuable natural experiment: if the same data lineage produces the same private current-revision value across macOS 26 Intel and macOS 27 Apple silicon, the “current revision” may be tied to a shared model generation or data representation rather than simply the marketing OS version. This is **[speculative]** and cannot be proven without Apple internals.

## D. Best Practices

### Read-only validation before remediation

The safest investigation is to create **comparable bounded observation windows** while changing nothing that invalidates caches. Apple’s own bug-reporting guidance emphasizes exact reproduction time and diagnostic capture, and says performance regressions should include a macOS process sample where useful. ([Apple Developer, “How to file great bug reports,” 2 June 2022](https://developer.apple.com/news/?id=vvrgkboh); [Feedback Assistant, undated](https://developer.apple.com/feedback-assistant/)) citeturn531143search6turn531143search0

Use identical windows—for example 30 or 60 minutes—under documented machine state: Photos open/closed, AC power, display awake/asleep, library sync status, macOS build, and whether a new import occurred. Record only technical identifiers needed for correlation; do not publish filenames, face/person names, GPS data, or image content.

The key measurements are:

- **Mismatch throughput:** total exact rejection count and counts by `(provided revision, current revision)`.
- **Recurrence structure:** whether stable non-content identifiers repeat across bursts; hash identifiers before external sharing.
- **Success throughput:** completed analysis tasks, successful embedding insertions if visible, gallery-state saves, and any Photos UI progress/status change.
- **Resource attribution:** cumulative CPU time of `mediaanalysisd`, `photoanalysisd`, `photolibraryd`, `cloudphotosd`, Spotlight-related processes, plus process samples during a burst.
- **Work-versus-reject chronology:** whether CPU rises materially *before* a mismatch burst and whether that CPU belongs to the same process/task chain.
- **Convergence:** unique rejected identifiers per window, reappearance rate, and trend over repeated equivalent windows.

That dataset can distinguish two very different systems: **cheap stale-result discard**, where old results are rejected quickly while the queue drains, versus **expensive failed backfill**, where the same inputs repeatedly incur meaningful inference CPU and then fail insertion.

### Ordinary-user options

| Option | Preconditions / risks | What it can tell you | What it cannot establish |
|---|---|---|---|
| **Leave Mac powered and Photos idle** | Time, cooling, sufficient free storage; no destructive change | Whether normal analysis converges | Does not prove why it converged |
| **Install the latest compatible macOS maintenance update** | Full backup recommended; may itself trigger fresh analysis | Whether Apple shipped an unannounced fix or refreshes pipeline state | A disappearance after update does not identify the specific fixed component |
| **Quit and reopen Photos** | Very low risk | Whether Photos frontend/session state influences scheduling | Does not reset private daemon state or prove a fix |
| **Test a new clean Photos library** | Keep it non-System unless deliberately changing configuration; import only a small non-sensitive test set | Separates library-specific from machine-wide behavior | A clean library cannot prove the original library is corrupt |
| **Test another macOS user account** | Avoid changing iCloud configuration unless deliberately designed | Separates per-user state from system-wide behavior | Does not isolate an iCloud-shared data lineage if same data are reintroduced |
| **Repair Library** | **Complete library backup first**; potentially long rebuild/recheck; iCloud checks library afterward | Tests whether library-database inconsistency participates | Does not prove what internal structure was fixed |
| **Apple Support / Feedback Assistant** | Requires diagnostic sharing under Apple’s workflow | Can reach engineers with private symbols/internal knowledge | No guarantee of acknowledgement or fix |

Apple explicitly recommends first updating macOS and, for unexplained Photos behavior, testing a new library or user account before Repair Library. ([Apple Support, “Get help with the Photos app on your Mac,” 25 February 2026](https://support.apple.com/en-my/108793)) citeturn613637search5 Apple also requires/recommends backing up before Repair Library and says iCloud Photos rechecks the repaired library after completion. ([Apple Support, undated/current](https://support.apple.com/en-ie/118221)) citeturn613637search0

For the active Intel Mac, **macOS 27 is not an update option** because Apple restricts macOS 27 to Apple-silicon Macs; the latest compatible macOS 26 maintenance release is the relevant branch. ([Apple Support, 14 September 2026](https://support.apple.com/en-us/127255)) citeturn748325search1

### Bug-report minimum viable artifact

A useful Apple report should contain: exact macOS build; exact hardware model; Photos-library size; whether iCloud/System Photo Library is involved; exact mismatch string and both revision pairs; UTC/local timestamps for at least two bursts; a short bounded log extract; process CPU-time data; and a sysdiagnose captured immediately after a representative burst. Apple says Feedback Assistant automatically attaches a sysdiagnose and recommends exact reproduction steps, observed result, expected result, and relevant environmental details. ([Apple Feedback Assistant, undated](https://developer.apple.com/feedback-assistant/)) citeturn531143search0

A particularly useful title would be narrowly descriptive rather than causal: **“Photos/mediaanalysisd repeatedly rejects embedding revision 2 and 3737841665 against current 3737841669; possible repeated analysis on large migrated library.”** That avoids asserting corruption, looping, or CPU causation.

## E. Failure Modes

### Competing mechanism map

| Hypothesis | Confidence now | Observations that support it | Observations that would argue against it |
|---|---|---|---|
| **Normal OS/model migration; stale derived embeddings are being discarded and regenerated** | [emerging] | Rejection set shrinks; IDs mostly unique; successful analyses increase; CPU eventually falls; Photos progress advances | Same IDs recur indefinitely with no progress |
| **Mixed runtime/model generations** | [speculative] | Different processes emit old and new revisions contemporaneously; signature appears immediately after OS/model update; restart/session boundary changes behavior | All producers consistently emit same revision yet rejection persists |
| **Failed regeneration/backfill** | [emerging] | Same IDs repeatedly consume CPU before rejection; no decrease over days; successful output count stagnates | Old IDs disappear after one pass |
| **Malformed/unsupported media triggers repeat scheduling** | [speculative] | Small stable asset subset dominates failures; failures correlate with codec/decode errors | Large, diverse rotating set with no media-decoding errors |
| **Persistent scheduler/retry defect** | [emerging] | Fixed periodic cadence, same task IDs, identical expensive work repeatedly, survives idle completion | Queue demonstrably drains and cadence disappears |
| **Database inconsistency** | [speculative] | Apple Repair Library changes the phenomenon while OS/model/runtime are unchanged | Clean diagnostic progress without repair; repair produces no difference |
| **Unrelated CPU/thermal/power issue** | [emerging] | High CPU windows occur without mismatch events; other processes dominate; mismatch bursts are CPU-cheap | CPU consistently precedes and scales with the same repeated failed tasks |

### Why common internet “fixes” are weak evidence

**Force-killing the daemon is not a demonstrated cure.** A macOS 26.1/26.2 first-person MacRumors report says `mediaanalysisd` returned and the high CPU persisted after force quitting. ([MacRumors, 2025–2026](https://forums.macrumors.com/threads/macos-tahoe-26-2-bug-fixes-changes-and-more.2474608/page-3)) citeturn235624search6 That is only anecdotal evidence, but it directly contradicts claims that killing the process permanently resolves the underlying queue.

**Deleting caches/containers is not validated for this exact symptom.** Current web guides make claims about clearing or deleting `mediaanalysisd` caches, but none identified in this research supplied a controlled before/after showing disappearance of the exact embedding-revision mismatch with meaningful relapse follow-up. Some guides recommend destructive or semi-destructive actions based primarily on inferred process behavior rather than Apple documentation. [no validated exact-signature source found]

**Disabling launch agents or SIP is particularly poor practice here.** One older AppleInsider article recommends disabling System Integrity Protection in order to interfere with `mediaanalysisd`; this changes the system’s security posture and does not establish that the analysis queue or underlying revision state has been repaired. ([AppleInsider, 2023](https://appleinsider.com/inside/macos-ventura/tips/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos/amp/)) citeturn235624search4 This is not appropriate as a diagnostic or ordinary-user remedy for the present case.

**“Reset analysis” recipes have the same causal problem:** deleting derived state may cause the symptom to disappear because everything is rebuilt, but without a controlled baseline and follow-up it is impossible to distinguish a genuine fix from a temporary cache reset followed by recurrence. Open-source photo systems explicitly warn that model migration can involve complete re-embedding, demonstrating why destructive resets can create substantial extra work. ([PhotoPrism, current docs, undated](https://docs.photoprism.app/getting-started/updates/)) citeturn145204search16

### Repair Library failure mode

Repair Library is official and appropriate when a library will not open or Photos behaves unexpectedly, but it is not a neutral diagnostic. Apple says it analyzes the database and repairs detected inconsistencies, and large libraries can take substantial time; iCloud then checks the library again. ([Apple Support, undated/current](https://support.apple.com/en-ie/118221)) citeturn613637search0 Therefore an early repair can destroy the clean “before” state, trigger new analysis, and make causal attribution impossible.

The correct interpretation after a successful repair would be: **“Repair changed the condition.”** It would not yet justify: **“The database was corrupt and Repair Library permanently fixed the embedding bug.”** To claim the latter requires recurrence testing and preferably a matched control or reproducible pre/post signature.

## F. Strategic Recommendations

### Decision hierarchy

**Option: continue diagnosis without altering derived state — [established] safest.** Trade-off: it does not immediately reduce workload. Benefit: preserves the current reproducible condition and can distinguish harmless rejection from expensive failed retry.

**Option: file an Apple bug now — [emerging] high value, low technical risk.** Your current evidence already exceeds the quality of typical generic “high CPU” reports because it contains an exact private error string, exact revision pairs, cross-platform occurrence, bounded counts, cadence information, successful-work observations, and a large-library context. Apple explicitly asks for exact reproduction timing and diagnostics, and Feedback Assistant can attach sysdiagnose automatically. ([Apple Feedback Assistant, undated](https://developer.apple.com/feedback-assistant/)) citeturn531143search0

**Option: ordinary idle migration period — [emerging].** Apple documents that Photos’ recognition/indexing may require days on large libraries and works while Photos is not actively being used. ([Apple Support, 2025](https://support.apple.com/en-us/105081)) citeturn864552search0 It is therefore reasonable to allow a bounded observation period, but “wait indefinitely” is not a useful recommendation unless progress is measured.

**Option: update to the latest supported maintenance release — [emerging].** Apple says keeping macOS current can resolve Photos problems, but Apple’s public 26.3/27 release notes contain no exact fix for this signature. ([Apple Support, 25 February 2026](https://support.apple.com/en-my/108793); [Apple 26.3 release notes](https://developer.apple.com/documentation/macos-release-notes/macos-26_3-release-notes); [Apple 27 release notes](https://developer.apple.com/documentation/macos-release-notes/macos-27-release-notes)) citeturn613637search5turn311592search1turn311592search2 Treat improvement after updating as evidence, not proof of a documented fix.

**Option: Repair Library — [speculative] as a targeted later intervention.** Use only after a complete backup and after capturing a strong pre-repair baseline. Apple’s tool is legitimate; the timing is the issue. ([Apple Support, undated/current](https://support.apple.com/en-ie/118221)) citeturn613637search0

### Competitive and state-of-the-art implications

The broader ML field increasingly treats representation versioning as an explicit deployment problem rather than assuming vectors are timeless. Backward-compatible representation research from 2020 through 2025 attempts to let new models coexist with old vectors without reprocessing every indexed item. ([Shen et al., 2020](https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html); [Jang & Lim, 2025](https://openaccess.thecvf.com/content/ICCV2025/html/Jang_Towards_Cross-modal_Backward-compatible_Representation_Learning_for_Vision-Language_Models_ICCV_2025_paper.html)) citeturn164274search1turn164274search3

PhotoPrism makes model migration explicit by retaining the old face model until a deliberate re-embedding migration is run; Immich exposes separate Smart Search, face-detection, and facial-recognition jobs and warns that machine-learning jobs can consume substantial processing resources. ([PhotoPrism, current docs, undated](https://docs.photoprism.app/getting-started/updates/); [Immich System Settings, current docs](https://docs.immich.app/administration/system-settings/)) citeturn145204search16turn145204search31 Google Photos publicly describes face models as numerical representations used to assess similarity, but does not publish an equivalent private-version migration interface. ([Google Photos Help, current/undated](https://support.google.com/photos/answer/6128838?hl=en)) citeturn145204search0

The industry lesson is not that Apple is necessarily defective. It is that **embedding migration is a real systems problem, and observability—model version, migration progress, failed-item identity, retry policy—is the difference between a diagnosable migration and an opaque high-CPU complaint.**

### Publication threshold

A **safe diagnostic guide** is publishable once the read-only workflow is reproduced on at least one additional independently sourced library—or on the same library with multiple clearly comparable windows—and the guide makes no causal claim beyond what the measurements show.

A **bug report** is justified already. Threshold: exact message, builds, timestamps, bounded recurrence data, process samples/sysdiagnose, and statement that the same family data lineage may underlie both machines.

A **mitigation claim** should require a predefined metric such as median process CPU-time or rejection rate dropping materially after the intervention relative to matched pre-intervention windows, while Photos remains functional. A reversible priority reduction can be described only as **contention mitigation** unless it also causes the mismatch itself to stop.

A **permanent cure claim** should require: reproducible pre-intervention mismatch; one clearly defined intervention; zero exact-signature recurrence through multiple comparable idle-analysis cycles; no return after new imports and at least one routine Photos reanalysis trigger; preserved search/People functionality; and meaningful follow-up. For a phenomenon with roughly hourly bursts in the supplied data, **a minimum seven-day clean follow-up is defensible for an initial claim, and 30 days plus an OS maintenance update/import cycle is substantially stronger.** The exact follow-up thresholds are **PROPOSED**, not Apple standards. `[no source found]`

## G. Action Plan

The next experiment should be designed to falsify either **“normal migration is converging”** or **“the same expensive work is being retried.”**

**Phase one — baseline, no state changes.** Collect three or more identical 60-minute windows on the active Intel Mac under the same conditions: AC connected, no import, Photos state recorded, same foreground workload, and no reboot/repair/cache deletion. For each window record mismatch count by revision pair, unique stable task/asset identifiers if logs expose them, recurrence of those identifiers, successful-completion/gallery-save events, cumulative CPU time for the media-analysis process group, and a process sample around at least one burst. Apple recommends noting exact times and collecting diagnostics close to reproduction. ([Apple Developer, Feedback Assistant, undated](https://developer.apple.com/feedback-assistant/); [Apple Developer Forums bug-report guidance, 2022](https://developer.apple.com/forums/thread/712889)) citeturn531143search0turn531143search7

**Pre-registered interpretation:** if unique rejected identifiers decline and repeated-ID fraction is low while successful completions accumulate, treat the symptom provisionally as migration/stale-cache cleanup. If the same identifiers return across windows with similar pre-rejection CPU expenditure and no progress indicator changes, escalate the scheduler/failed-backfill hypothesis. If high CPU occurs outside mismatch windows, decouple the generic CPU investigation from the revision issue.

**Phase two — family-Mac comparison.** On the M4 Max Mac Studio, collect the same bounded metrics without assuming independence. Compare *identifier recurrence and revision pairs*, not raw counts. If hashed identifiers overlap substantially across the two machines, shared library/iCloud lineage becomes a stronger explanation for the matching signature. If they do not overlap, the result is more interesting but still not independent until library/account sharing is verified.

**Phase three — bug artifact.** File the issue while the state is still reproducible. Include the exact message, the two revision pairs, the Intel/macOS 26 and M4 Max/macOS 27 observations, the explicit caveat that the machines may share Photos/iCloud lineage, and a sysdiagnose captured immediately after a representative burst. Apple says Feedback Assistant automatically attaches sysdiagnose and allows additional files or device diagnostics. ([Apple Feedback Assistant, undated](https://developer.apple.com/feedback-assistant/); [Apple Support, Feedback Assistant user guide, current/undated](https://support.apple.com/en-by/guide/feedback-assistant/fbad45b5bb25/mac)) citeturn531143search0turn531143search3

**Phase four — reversible/official interventions, one at a time.** First allow a measured idle-analysis period. Next, if a newer maintenance release is available for the respective OS branch, update and repeat the exact baseline protocol. Next test a small clean library/user environment. Only after that, and with a complete photo-library backup, consider Repair Library on the affected main library. Apple’s current troubleshooting sequence supports update → new library/account testing → Repair Library for persistent abnormal Photos behavior. ([Apple Support, 25 February 2026](https://support.apple.com/en-my/108793)) citeturn613637search5

**Do not include in the experiment:** manually editing Photos SQLite/private stores, deleting Photos/media-analysis containers, disabling SIP, permanently disabling launch agents, or resetting private analysis databases. Those interventions are poorly characterized, can trigger broad reprocessing, and would make it difficult to infer which state actually mattered.

**Single recommendation: file an Apple bug now while continuing a short, instrumented read-only convergence test before any repair. [emerging]** The main assumption is that the present state is reproducible enough to capture one or two additional high-quality windows. If those windows show repeated expensive work on the same identifiers, the case moves from “interesting migration signal” toward a reproducible scheduler/backfill defect; if they show monotonic progress, normal migration becomes substantially more likely.

## H. Sources

Apple, **“VNGenerateImageFeaturePrintRequestRevision1”**, Apple Developer Documentation, publication date unavailable/undated. https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1 citeturn617076search4

Apple, **“VNRequest”**, Apple Developer Documentation, publication date unavailable/undated. https://developer.apple.com/documentation/vision/vnrequest citeturn617076search8

Apple, **“VNRequestRevisionProviding”**, Apple Developer Documentation, publication date unavailable/undated. https://developer.apple.com/la/documentation/vision/vnrequestrevisionproviding citeturn617076search9

Apple, **“VNFeaturePrintObservation”**, Apple Developer Documentation, publication date unavailable/undated. https://developer.apple.com/documentation/vision/vnfeatureprintobservation citeturn617076search0

Apple, **“Recognizing People in Photos Through Private On-Device Machine Learning,”** Apple Machine Learning Research, 2021. https://machinelearning.apple.com/research/recognizing-people-photos citeturn864552search10

Apple Support, **“If you can’t find pets in the People & Pets album in Photos,”** 2025. https://support.apple.com/en-us/105081 citeturn864552search0

Apple Support, **“How to use the Photos Repair Library tool on your Mac,”** publication date unavailable/current page. https://support.apple.com/en-ie/118221 citeturn613637search0

Apple Support, **“Get help with the Photos app on your Mac,”** 25 February 2026. https://support.apple.com/en-my/108793 citeturn613637search5

Apple Support, **“About Enhanced Visual Search in Photos,”** 2025. https://support.apple.com/en-ie/122033 citeturn864552search2

Apple, **“Photos & Privacy,”** updated 2025/2026. https://www.apple.com/legal/privacy/data/en/photos/ citeturn864552search4

Apple Developer, **“Feedback Assistant,”** publication date unavailable/undated. https://developer.apple.com/feedback-assistant/ citeturn531143search0

Apple Developer, **“Enhance bug reports with debug profiles and logging,”** 2 June 2022. https://developer.apple.com/news/?id=2o2p68bq citeturn531143search5

Apple Developer, **“How to file great bug reports,”** 2022. https://developer.apple.com/news/?id=vvrgkboh citeturn531143search6

Apple Support, **“Create new feedback in Feedback Assistant on Mac,”** publication date unavailable/current page. https://support.apple.com/en-by/guide/feedback-assistant/fbad45b5bb25/mac citeturn531143search3

Apple Developer, **macOS Tahoe 26.3 Release Notes**, 2026. https://developer.apple.com/documentation/macos-release-notes/macos-26_3-release-notes citeturn311592search1

Apple Developer, **macOS 27 Golden Gate Release Notes**, 2026. https://developer.apple.com/documentation/macos-release-notes/macos-27-release-notes citeturn311592search2

Apple Support, **“macOS 27 Golden Gate is compatible with these computers,”** 14 September 2026. https://support.apple.com/en-us/127255 citeturn748325search1

Apple Support, **“macOS Tahoe 26 is compatible with these computers,”** 2026. https://support.apple.com/en-gb/122867 citeturn748325search3

Schroff, Kalenichenko & Philbin, **“FaceNet: A Unified Embedding for Face Recognition and Clustering,”** CVPR, June 2015. https://openaccess.thecvf.com/content_cvpr_2015/html/Schroff_FaceNet_A_Unified_2015_CVPR_paper.html citeturn164274search4

Deng et al., **“ArcFace: Additive Angular Margin Loss for Deep Face Recognition,”** CVPR, June 2019. https://openaccess.thecvf.com/content_CVPR_2019/html/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.html citeturn164274search0

Shen et al., **“Towards Backward-Compatible Representation Learning,”** CVPR, 2020. https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html citeturn164274search1

Pan et al., **“Boundary-Aware Backward-Compatible Representation via Adversarial Learning in Image Retrieval,”** CVPR, June 2023. https://openaccess.thecvf.com/content/CVPR2023/html/Pan_Boundary-Aware_Backward-Compatible_Representation_via_Adversarial_Learning_in_Image_Retrieval_CVPR_2023_paper.html citeturn164274search2

Jang & Lim, **“Towards Cross-modal Backward-compatible Representation Learning for Vision-Language Models,”** ICCV, October 2025. https://openaccess.thecvf.com/content/ICCV2025/html/Jang_Towards_Cross-modal_Backward-compatible_Representation_Learning_for_Vision-Language_Models_ICCV_2025_paper.html citeturn164274search3

PhotoPrism, **“Getting Updates / Face-model migration,”** current documentation, publication date unavailable/undated. https://docs.photoprism.app/getting-started/updates/ citeturn145204search16

Immich, **“System Settings — Machine Learning,”** current documentation, publication date unavailable/undated. https://docs.immich.app/administration/system-settings/ citeturn145204search31

Google Photos Help, **“Set up & manage your face groups,”** current documentation, publication date unavailable/undated. https://support.google.com/photos/answer/6128838?hl=en citeturn145204search0

9to5Mac, **“Mac beachballs or lagging performance? AI photo scanning may be the reason,”** 3 June 2026. https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/ citeturn235624search0

Reddit r/MacOS, **“M1 MacBook Air Heating up After Update - mediaanalysisd,”** 12 February 2025. [low-authority] https://www.reddit.com/r/MacOS/comments/1io0zdb citeturn235624reddit11

MacRumors Forums, **“com.apple.mediaanalysisd filling out my entire SSD on my Mac mini! What to do??”** 2024. [low-authority] https://forums.macrumors.com/threads/com-apple-mediaanalysisd-filling-out-my-entire-ssd-on-my-mac-mini-what-to-do.2444392/ citeturn648357search6

MacRumors Forums, **“macOS Tahoe 26.2 — Bug fixes, changes, and more,”** 2025–2026 thread. [low-authority] https://forums.macrumors.com/threads/macos-tahoe-26-2-bug-fixes-changes-and-more.2474608/page-3 citeturn235624search6

AppleInsider, **“How to stop mediaanalysisd from hogging your CPU in macOS,”** 2023. [single-source; secondary source] https://appleinsider.com/inside/macos-ventura/tips/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos/amp/ citeturn235624search4

**Search coverage and gaps:** exact full-message search; partial-message search; searches for `3737841669`, `3737841665`, and revision `2`; Apple Developer documentation and release notes; Apple Support; Apple Developer Forums; Apple Support Communities; Reddit; MacRumors; indexed public GitHub/web repositories; generic `mediaanalysisd`/`photoanalysisd` CPU and migration reports; primary ML literature on embedding compatibility; and open-source photo-indexing systems. No public exact-signature match, public mapping for the private revision values, Apple acknowledgement, release-note fix, developer-forum answer, reproducible upstream issue, or independently validated permanent cure was found. That negative result should be interpreted as **“not found in the searched public corpus as of 14 November 2026,” not “proven nonexistent.”**

## Sources (URLs from report metadata)
- https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/
- https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/?utm_source=chatgpt.com
- https://appleinsider.com/inside/macos-ventura/tips/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos/amp/
- https://appleinsider.com/inside/macos-ventura/tips/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos/amp/?utm_source=chatgpt.com
- https://developer.apple.com/documentation/macos-release-notes/macos-26_3-release-notes?changes=_3
- https://developer.apple.com/documentation/macos-release-notes/macos-26_3-release-notes?changes=_3&utm_source=chatgpt.com
- https://developer.apple.com/documentation/macos-release-notes/macos-27-release-notes?changes=l_2
- https://developer.apple.com/documentation/macos-release-notes/macos-27-release-notes?changes=l_2&utm_source=chatgpt.com
- https://developer.apple.com/documentation/vision/vnfeatureprintobservation?changes=_____8&language=objc
- https://developer.apple.com/documentation/vision/vnfeatureprintobservation?changes=_____8&language=objc&utm_source=chatgpt.com
- https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1?changes=l__4_5%2Cl__4_5
- https://developer.apple.com/documentation/vision/vngenerateimagefeatureprintrequestrevision1?changes=l__4_5%2Cl__4_5&utm_source=chatgpt.com
- https://developer.apple.com/documentation/vision/vnrequest?language=objc
- https://developer.apple.com/documentation/vision/vnrequest?language=objc&utm_source=chatgpt.com
- https://developer.apple.com/feedback-assistant/
- https://developer.apple.com/feedback-assistant/?utm_source=chatgpt.com
- https://developer.apple.com/forums/thread/712889
- https://developer.apple.com/forums/thread/712889?utm_source=chatgpt.com
- https://developer.apple.com/la/documentation/vision/vnrequestrevisionproviding
- https://developer.apple.com/la/documentation/vision/vnrequestrevisionproviding?utm_source=chatgpt.com
- https://developer.apple.com/news/?id=2o2p68bq
- https://developer.apple.com/news/?id=2o2p68bq&utm_source=chatgpt.com
- https://developer.apple.com/news/?id=vvrgkboh
- https://developer.apple.com/news/?id=vvrgkboh&utm_source=chatgpt.com
- https://docs.immich.app/administration/system-settings/
- https://docs.immich.app/administration/system-settings/?utm_source=chatgpt.com
- https://docs.photoprism.app/getting-started/updates/
- https://docs.photoprism.app/getting-started/updates/?utm_source=chatgpt.com
- https://forums.macrumors.com/threads/com-apple-mediaanalysisd-filling-out-my-entire-ssd-on-my-mac-mini-what-to-do.2444392/
- https://forums.macrumors.com/threads/com-apple-mediaanalysisd-filling-out-my-entire-ssd-on-my-mac-mini-what-to-do.2444392/?utm_source=chatgpt.com
- https://forums.macrumors.com/threads/macos-tahoe-26-2-bug-fixes-changes-and-more.2474608/page-3
- https://forums.macrumors.com/threads/macos-tahoe-26-2-bug-fixes-changes-and-more.2474608/page-3?utm_source=chatgpt.com
- https://machinelearning.apple.com/research/recognizing-people-photos
- https://machinelearning.apple.com/research/recognizing-people-photos?utm_source=chatgpt.com
- https://openaccess.thecvf.com/content/CVPR2023/html/Pan_Boundary-Aware_Backward-Compatible_Representation_via_Adversarial_Learning_in_Image_Retrieval_CVPR_2023_paper.html
- https://openaccess.thecvf.com/content/CVPR2023/html/Pan_Boundary-Aware_Backward-Compatible_Representation_via_Adversarial_Learning_in_Image_Retrieval_CVPR_2023_paper.html?utm_source=chatgpt.com
- https://openaccess.thecvf.com/content/ICCV2025/html/Jang_Towards_Cross-modal_Backward-compatible_Representation_Learning_for_Vision-Language_Models_ICCV_2025_paper.html
- https://openaccess.thecvf.com/content/ICCV2025/html/Jang_Towards_Cross-modal_Backward-compatible_Representation_Learning_for_Vision-Language_Models_ICCV_2025_paper.html?utm_source=chatgpt.com
- https://openaccess.thecvf.com/content_CVPR_2019/html/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.html
- https://openaccess.thecvf.com/content_CVPR_2019/html/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.html?utm_source=chatgpt.com
- https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html
- https://openaccess.thecvf.com/content_CVPR_2020/html/Shen_Towards_Backward-Compatible_Representation_Learning_CVPR_2020_paper.html?utm_source=chatgpt.com
- https://openaccess.thecvf.com/content_cvpr_2015/html/Schroff_FaceNet_A_Unified_2015_CVPR_paper.html
- https://openaccess.thecvf.com/content_cvpr_2015/html/Schroff_FaceNet_A_Unified_2015_CVPR_paper.html?utm_source=chatgpt.com
- https://support.apple.com/en-by/guide/feedback-assistant/fbad45b5bb25/mac
- https://support.apple.com/en-by/guide/feedback-assistant/fbad45b5bb25/mac?utm_source=chatgpt.com
- https://support.apple.com/en-gb/122867
- https://support.apple.com/en-gb/122867?utm_source=chatgpt.com
- https://support.apple.com/en-ie/118221
- https://support.apple.com/en-ie/118221?utm_source=chatgpt.com
- https://support.apple.com/en-ie/122033
- https://support.apple.com/en-ie/122033?utm_source=chatgpt.com
- https://support.apple.com/en-my/108793
- https://support.apple.com/en-my/108793?utm_source=chatgpt.com
- https://support.apple.com/en-us/105081
- https://support.apple.com/en-us/105081?utm_source=chatgpt.com
- https://support.apple.com/en-us/127255
- https://support.apple.com/en-us/127255?utm_source=chatgpt.com
- https://support.google.com/photos/answer/6128838?hl=en
- https://support.google.com/photos/answer/6128838?hl=en&utm_source=chatgpt.com
- https://www.apple.com/legal/privacy/data/en/photos/
- https://www.apple.com/legal/privacy/data/en/photos/?utm_source=chatgpt.com
- https://www.reddit.com/r/MacOS/comments/1io0zdb
- https://www.reddit.com/r/MacOS/comments/1io0zdb?utm_source=chatgpt.com
- https://www.reddit.com/r/MacOS/comments/e6o4xk
- https://www.reddit.com/r/MacOS/comments/e6o4xk?utm_source=chatgpt.com
