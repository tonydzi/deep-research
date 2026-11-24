# DR26-11-13-MACANTON-01-0708 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: https://chat.mistral.ai/work/5cdee969-d378-4344-8ced-ac9ab77eff56
dr_id: DR26-11-13-MACANTON-01-0708
rail: mistral
mode: Mistral Vibe Work, Think + deep-research skill, canvas report
topic: Apple Photos embedding revision mismatch
collected: 2026-11-14 06:29 WET

---

Apple Photos / mediaanalysisd embedding-revision mismatch: migration guard or persistent defect?

Case ID: DR26-11-13-MACANTON-01-0708 · Report date: November 14, 2026 · Scope: Apple Photos / mediaanalysisd on Intel and Apple silicon Macs

Question

An Intel MacBook Pro 16,1 (i9, 32 GB, macOS 26.3.1(a), 187,840 asset records) and a Mac Studio M4 Max (macOS 27.0.1, build 26A434) belonging to the same owner both log:

"Unable to add embedding: provided embedding revision (2) does not match the current embedding revision (3737841669)"

and sometimes provided 3737841665 vs current 3737841669. Three bursts of ~620 insertion rejections occurred in a 3-hour window, roughly an hour apart, each burst visible in logs for ~0.24 s. A bounded 24-hour log query found at least 3,627 matching lines (lower bounds only; the query timed out). The research question: is this a benign migration guard, a persistent defect worth repairing, and what safe, validated remediation path exists for an ordinary owner?

Explicit statement: No public exact-signature match for this log line was found anywhere in the searchable public web as of November 14, 2026, and no validated cure for this exact signature exists. Several close siblings and one well-documented analogous defect family exist and are analyzed below.

A. Executive Summary

The message is a consistency guard, not proof of corruption [established as a mechanism, inferred for this case]. Apple's public Vision framework formalizes exactly this pattern: every Vision algorithm request carries a revision number, with currentRevision, defaultRevision and supportedRevisions properties, precisely because feature outputs from different algorithm versions are not interchangeable (Apple Developer Documentation, Vision). Photos' private embedding store appears to apply the same principle: embeddings (numeric vectors Photos computes from photos to power semantic search, captions, memories and People) are rejected when their producer's model revision does not match the store's current revision. Rejection is the safe behavior for a vector database; mixing vectors from incompatible feature spaces would silently corrupt similarity search.

The private constants decoded. 3737841669 = 0xDECAF005 and 3737841665 = 0xDECAF001 (plain arithmetic, verified computationally in this research). These are hex "magic tag" constants in a 0xDECAF000 family, not hashes and not corruption. No public mapping of these values to a named model or OS version exists: UNKNOWN. The presence of both 2 and 0xDECAF001 as provided revisions against current 0xDECAF005 suggests two distinct producers of stale embeddings: one small-integer scheme (resembling, but not proven to be, Vision-style public request revisions such as VNDetectFaceRectanglesRequestRevision2 [speculative]) and one older tag in the same DECAF family (consistent with embeddings several model generations old being re-published) [inferred].

Same signature on two OS generations argues against a single-OS regression. The MacBook (macOS 26.3.1) and Mac Studio (macOS 27.0.1, different build train) show identical values. Since the Macs may share migrated Photos/iCloud data, they are one incident, not two independent reproductions; but the cross-generation match makes "the current OS simply expects a newer model, and some old component keeps publishing" more plausible than "one broken update" [inferred].

The closest public sibling is well documented and was a real defect. On macOS 15.5 (June 2025), developer Scott (zerosleeps) captured mediaanalysisd emitting 918,968 occurrences of "Embedding version: 0 not supported, skip embedding publishing" in ~3 hours while re-visiting 675 files, with ~20% sustained CPU (zerosleeps, June 15, 2025). Same guard family, different values, and it was paired with a wasteful retry loop. His follow-up reported the machine "delightfully quiet" after upgrading to macOS 26 (zerosleeps, September 17, 2025) [single-source outcome, no Apple confirmation].

Apple has a track record of fixing mediaanalysisd/Spotlight-analysis defects silently, without release notes. The macOS 15.1+ runaway cache/CPU bug (reported by many users starting November 2024, with users reporting improvement across 15.2-15.5 for some but not all) has "no official Apple fix note" (Apple Support Community thread 255835917; Michael Tsai, July 7, 2025; CoreGuard, July 7, 2026). In 2026, macOS 26.5 beachballs traced to mediaanalysisd at 50-150% CPU were widely reported (9to5Mac, June 3, 2026), and Apple's macOS 26.6 update "optimizes the Spotlight index in preparation for macOS 27" (Apple via MacRumors); Golden Gate 27.0 "fixed longstanding bugs in the indexing of some plain text files" (Howard Oakley, October 20, 2026). No release note ever named mediaanalysisd.

Real prevalence is unknown; no prevalence percentage can be honestly stated. Forum thread sizes, votes and "me too" counts are not population estimates. Apple's Support Community is closing November 20, 2026 (announced on the forums; read-only as of mid-November 2026), which will make future public incident discovery harder (MacRumors, November 11, 2026).

Recommended decision: treat the case as expected-but-unproven migration behavior until cheap, read-only evidence says otherwise. Do not run Repair Library, delete caches, or disable daemons yet. Run the bounded observability experiment (Section G), then file a precise Feedback Assistant report with sysdiagnose. Confidence: [emerging] that no repair is warranted yet; the main assumption is that the rejections are not accompanied by high CPU waste, which the experiment falsifies directly.

Methodology

Search angles (English public web, executed November 14, 2026): exact message and phrase variants ("Unable to add embedding", "does not match the current embedding revision", "provided embedding revision"); the decimal constants 3737841669 / 3737841665; sibling strings ("Embedding version: 0 not supported, skip embedding publishing", "com.apple.spotlightembedding", "MADTextEmbeddingTask"); mediaanalysisd / photoanalysisd CPU and cache-bloat symptoms; Apple primary documentation (Vision revision APIs, Photos Repair tool, library status, release notes, Feedback Assistant); independent technical write-ups (Howard Oakley/Eclectic Light, Michael Tsai, zerosleeps, Glyph); first-person forum incidents (Apple Support Community, MacRumors Forums, Ask Different, Reddit, 9to5Mac comment reports).

Source types: Apple official documentation and developer docs (highest weight for mechanisms), independent macOS internals researchers (Oakley, Tsai: high technical credibility), first-person incident reports (primary evidence only for that person's experience), and aggregator/SEO articles (low weight, used only to locate incidents, never for mechanism claims).

Deduplication rule: each first-person report counted once per incident; multi-comment threads treated as one incident with sub-reports.

Limitations: Apple Support pages rate-limited full fetch (content corroborated via search snippets and independent secondary sources); Ask Different blocked by Cloudflare (snippets only); Reddit and one 2026 blog unreadable via automated fetch; Apple internal Feedback reports are not public, so any existing Apple reproduction of this signature is invisible to this research. No prevalence claim is possible from any of these sources. macOS 27 "Golden Gate" shipped September 14, 2026, so 27.x public evidence is only ~2 months old (MacRumors roundup).

B. Key Findings
B1. What feature/embedding revisions mean (primary technical sources)

Apple's Vision framework is explicitly built around versioned algorithms: VNRequestRevisionProviding is "a protocol for specifying the revision number of Vision algorithms", and each request class exposes currentRevision ("the current revision supported by the request"), defaultRevision ("the revision of the latest request for the particular SDK linked with the client application") and supportedRevisions (Apple Developer Documentation; currentRevision). Public revision constants exist per request, e.g. VNDetectFaceRectanglesRequestRevision2, VNGenerateImageFeaturePrintRequestRevision1. This establishes as [established] that Apple's media-ML pipeline versions its algorithms and that outputs are tied to a revision.

Apple explicitly designs cross-revision behavior deliberately rather than mixing outputs: at WWDC22 Apple explained that revision 1 requests are satisfied by revision 2 detectors while revision 1 is deprecated, i.e., the framework manages compatibility at the boundary rather than letting mismatched outputs coexist (WWDC22, What's new in Vision).

Photos' search and Spotlight's semantic indexing demonstrably run on embeddings produced by mediaanalysisd. On macOS 27.0.1, Oakley traced mediaanalysisd running seven text-recognition sessions that "generated 0-2 text embeddings each" logged as com.apple.spotlightembedding Successfully generated 2 text embeddings, at background QoS 9 (Howard Oakley, October 20, 2026). Community inspection of the daemon's caches on macOS build 26A434 (the same build train as this case's Mac Studio) shows model families including mubb_md8, which "extracts visual embeddings", plus SystemSearch token and text encoders that "convert text into embeddings for searching by meaning" (MediaAnalysisExplorer, GitHub, October 2026) [single-source tool README, but consistent with Oakley's independent log evidence].

Why rejection is safe: embeddings are vectors in a model-specific feature space. Similarity search across vectors from different spaces produces garbage results. A store that refuses to insert vectors tagged with a different producer revision is behaving correctly; the defect, if any, is whatever keeps producing or re-offering the stale vectors, or doing expensive work before discarding them. A mismatch message alone does not establish database corruption, and public Vision "revision 2" must not be assumed to equal Photos' private current model revision: the private current revision (0xDECAF005) lives in a different numbering space [established concept, inferred application].

photoanalysisd (Photos library background processing for "Memories, People and scene or object based search", per its man page) and mediaanalysisd (the system-wide VisionKit/Live Text/Visual Look Up engine, binary at /System/Library/PrivateFrameworks/MediaAnalysis.framework/...) are distinct but cooperating daemons; the log message's owning process should be confirmed from the log's process field before attributing work (MacRumors Forums quoting man pages; AppleInsider; CoreGuard).

B2. The private revision values

3737841669 = 0xDECAF005; 3737841665 = 0xDECAF001 (computed; base tag 0xDECAF000). These are deliberately human-readable hex constants. Public mapping of these exact values: UNKNOWN. [no source found] after exact-value searches across the public web.

Interpretation options, all [speculative] except where noted: (a) internal model-space tags incremented per model generation (DECAF001 being four generations behind DECAF005) [plausible, inferred from sequential values]; (b) "decaf" chosen as an engineering in-joke, possibly nodding to DeCAF, the 2013 "Deep Convolutional Activation Feature" paper that popularized CNN activations as visual features (arXiv 1310.1531) [fringe]; (c) "provided embedding revision (2)" coming from a Vision-style public request revision constant rather than the DECAF space, implying a unit mismatch between two components [speculative].

What the two-machine data does support [inferred]: identical (current) values across macOS 26.3.1 and macOS 27.0.1 means the current model space is shared across both OS generations; a producer emitting revision 2/DECAF001 is stale relative to both. Because the machines may share migrated Photos/iCloud data, they cannot be counted as independent reproductions (per the case brief, unverified whether libraries/accounts are shared).

B3. Exact-signature and near-signature search: deduplicated incidents

Exact signature ("Unable to add embedding ... provided embedding revision (2) ... 3737841669/3737841665"): no public report found. [no source found] This includes exact-phrase, partial-phrase, and raw-decimal searches. Any matching reports would also be invisible if filed privately to Apple via Feedback Assistant.

Same guard family ("Embedding version: 0 not supported, skip embedding publishing"):

Incident

	

Hardware / OS

	

Symptom & evidence

	

Outcome




zerosleeps (Scott), June 2025

	

M1 MacBook Pro, macOS 15.5 (24F74)

	

918,968 occurrences of the sibling message in ~3 h; 992,607 error lines of 1.64 M total; 675 unique files re-opened 271,375 times; sustained ~20% CPU

	

Workaround: exclude offending folders from Spotlight (not a fix, author's words). Later: quiet after macOS 26 upgrade; message still appears in logs occasionally but the loop stopped (zerosleeps June 15, 2025; zerosleeps September 17, 2025)




Glyph, February 2025

	

Mac (Sequoia-era), exact HW unposted

	

mediaanalysisd ~4 GB RAM; same sibling message "zillions"; re-opening same few-dozen files; "Person not in Photos DB" errors

	

Killing the daemon restored normal temporarily; no permanent cure reported (Mastodon); a second user replied with the identical signature on their machine




Michael Tsai roundup, July 2025

	

Multiple

	

Collects macOS 15.x cache bloat (reported 15-143 GB for a 27 GB library) and Paul Hudson's "~100% CPU for over a week; rebooting didn't help"

	

Aggregate of unresolved reports; trigger "currently unknown" (Michael Tsai)




Apple Support Community thread 255835917, Nov 2024 onward

	

Mac mini M1 and others, macOS 15.1+

	

Cache grows ~67.9 MB/hour idle; 17.1 GB in a week; one user reports >300 GB disk writes/day and two failed SSDs; bug report filed by OP

	

Mixed outcomes by version: improved for some at 15.2/15.3, "does NOT fix" at 15.3.2 for one, "seem to have fixed" at 15.5 for another; no Apple acknowledgement (thread)

Generic high-CPU / thermal reports (no embedding strings): macOS 26 beta/26.0 overheating report (Sept 2025, unresolved, thread 256138728); macOS 26.5 AI photo scanning 50-150% CPU with kernel_task and corespotlightd (9to5Mac, June 3, 2026); macOS 26.5 beachballs resolved (2-week follow-up, n=2) by deleting ~/Library/Metadata/CoreSpotlight and letting Spotlight rebuild (9to5Mac, July 20, 2026); an Intel MacBook Pro (2020) case on Tahoe 26.4.1 plaguing for weeks with a "relatively clean workaround" posted mid-2026 (Ask Different, snippets only, Cloudflare-blocked full page); a 26.1 case triggered massively by attaching a new NAS drive, worked around by keeping Photos open (comment on Tsai's blog, December 3, 2025); a September 2026 blog reporting mediaanalysisd log storms on macOS 26 silenced by disabling Spotlight features (filiph.net) [low-authority, snippets only].

Indexing that eventually completed (counter-evidence to "everything is a loop"): multiple users in the "endlessly analyzing" threads report completion after days-to-weeks of idle, powered time; a 40,000-image library took over a week of 24/7 uptime to analyze (Apple Support Community thread 255823627, Oct 2024); Apple's own guidance is that analysis runs in the background and can take days (Apple Support: check your library status).

Prevalence: unknown. No honest rate can be computed from any of the above; counts of forum threads are not population estimates. Apple never published incident statistics.

B4. Evidence matrix

#

	

Finding

	

Exact-match to case signature?

	

Confidence

	

Source dates




1

	

Vision-style revision guard mechanism is real and public

	

No (mechanism analog)

	

[established]

	

Apple docs, current (fetched Nov 14, 2026)




2

	

Constants are 0xDECAF001/0xDECAF005 hex tags

	

Yes (values match; meaning unknown)

	

[established] arithmetic; interpretation [speculative]

	

computed Nov 14, 2026




3

	

Sibling "Embedding version 0" message exists with runaway loop

	

No (different string, same guard family)

	

[established]

	

Jun 2025 (zerosleeps), Feb 2025 (Glyph)




4

	

macOS 15.1+ mediaanalysisd runaway bug real, version-variable, unacknowledged by Apple

	

No (cache/CPU, not revision strings)

	

[established] as reported; single reports within it [single-source]

	

Nov 2024-Jun 2025




5

	

macOS 26.5 mediaanalysisd CPU storm; CoreSpotlight rebuild workaround

	

No

	

[emerging]

	

Jun-Jul 2026




6

	

macOS 26.6 "optimizes Spotlight index"; 27.0 fixes text-file indexing bugs

	

No

	

[established] (Apple's own words via release notes/articles)

	

Oct-Nov 2026




7

	

Identical signature on two OS generations in one household

	

Yes

	

[established] observation; independence unverified

	

case brief, Nov 2026




8

	

Public mapping of DECAF001/DECAF005 to named models

	

n/a

	

[no source found]

	

searched Nov 14, 2026




9

	

Apple acknowledgement of this exact signature

	

n/a

	

[no source found]

	

searched Nov 14, 2026

B5. Competing explanations, distinguishing observations, and current evidence

Hypothesis

	

What would distinguish it

	

Evidence for / against (as of Nov 14, 2026)




H1. Normal model migration guard; stale derived cache drains out

	

Rejection counts decay over days-weeks; analysis completes; Photos search quality improves; near-zero CPU cost per rejection burst

	

For: guard-style wording; successful completions observed; bursts are short (~0.24 s of logging). Against/unknown: bursts recur hourly over months(?); decay rate unmeasured. Verdict: plausible, currently favored [emerging]




H2. Mixed runtime/model versions (one component publishes revision 2 / DECAF001 while store expects DECAF005)

	

Signature persists across OS updates on both machines; appears in a clean new library on the same OS; disappears in a clean new user account

	

For: two different "provided" values; same current value across macOS 26 and 27; two machines. Against: could equally be per-library stale data (H3). Untested. [speculative]




H3. Failed regeneration of a specific library's stale embeddings (data-dependent)

	

Signature absent in clean library/account on same machine; rejections correlate with the same asset set each burst

	

For: large migrated library (187,840 records), two machines possibly sharing data. Against: exact asset identity across bursts unknown. Untested. [speculative]




H4. Unsupported/malformed media in the library

	

Rejections correlate with specific asset UUIDs; sibling errors (decode failures, tiny dimensions) nearby in logs

	

Sibling cases (2025) were triggered by undecodable images, but those logs show decode errors, not revision mismatches; no decode errors reported in this case. Against (weak). [speculative]




H5. Persistent scheduler/retry defect (wasteful recomputation before rejection)

	

High CPU time accrued per burst that produces only rejections; same assets re-rejected indefinitely; no completion progress

	

For: hourly recurrence pattern; historical 8.99-core load. Against: brief 0.24 s log bursts; successful analysis completions also observed; burst brevity does not bound upstream compute, so CPU-time deltas between bursts must be measured. Precedent shows Apple has shipped exactly this class of loop (2025). [speculative]




H6. Unrelated CPU/thermal issue; rejections are noise

	

CPU load uncorrelated in time with rejection bursts; analysis otherwise progresses

	

For: no proven causal link in the case data (the brief itself warns of this). Against: temporal co-occurrence across two machines. [speculative]

C. Contradictions & Debates

"Just a log line" vs "symptom of a loop." The 2025 sibling case proves a version-mismatch message can ride on a genuinely wasteful loop (918,968 repeats, 20% CPU sustained, cache ballooning). But the same message also persisted harmlessly (a couple thousand occurrences) on the same author's machine after the loop was fixed by the macOS 26 upgrade (zerosleeps, both posts). Steelman of the benign side: Apple's guard may simply be doing its job on old embeddings, and short bursts with successful completions nearby are what a draining migration queue looks like. Steelman of the defect side: hourly recurrence over a 3-hour window with ~620 rejections per burst is a retry pattern, and the 2025 precedent shows Apple's scheduler does get stuck in this subsystem. Both cannot be settled by wording; only the Section G experiment separates them.

"Update macOS fixes it" vs "updates trigger it." For the update-fixes side: zerosleeps' loop stopped at macOS 26; the 15.x cache bug faded across 15.2-15.5 for many; macOS 26.6 explicitly optimized Spotlight indexing. For the update-triggers side: multiple incidents begin after an update (15.1 onset reports; 26.5 reports; 14.7.2 daemon activity complaints (MacRumors Forums, Dec 2024)). Both are consistent with the same mechanism: each OS refresh changes model spaces and re-queues analysis; when the pipeline is healthy the work drains, when it is not it loops. Note: this case's Intel MacBook cannot leave macOS 26 (macOS 27 Golden Gate ends Intel support, (MacRumors roundup)), so "upgrade and see" is only available on the Mac Studio.

Kill/disable the daemon vs leave it alone. AppleInsider documents the disable route but notes it requires turning off SIP and "can leave your computer vulnerable to malicious code" (AppleInsider); disabling also breaks Visual Look Up, Live Text selection, and photos search, and SIP re-enable resurrects the daemon (MacRumors Forums). Others (Mole, CoreGuard) argue killing it is safe but futile: macOS relaunches it and it "picks up the same queue" (mole.fit, Nov 2026). This debate resolves to: killing is a diagnostic pause, not a cure; disabling requires disproportionate risk for a reversible symptom.

Delete the cache vs cache deletion resets everything. Practical guides recommend deleting ~/Library/Containers/com.apple.mediaanalysisd/Data/Library/Caches to reclaim space (OS X Daily, Jan 2025; Mac Observer); careful write-ups distinguish the Caches folder (rebuildable) from ~/Library/MediaAnalysis (finished analysis results; deleting restarts analysis from zero) and warn that rm -rf of the whole container resets all completed work (mole.fit). Community hacks (replace Caches with a symlink to /dev/null, thread 255835917) are destructive improvisations with no follow-up evidence. None of these addresses embedding-revision mismatches, which live in analysis databases, not caches [inferred].

Privacy claims around mediaanalysisd are settled and separate. The 2023 "Apple is scanning your files" claim was tested by multiple parties: Finder browsing produces no mediaanalysisd log activity at all; the observed network call was an empty request (Mysk), later called a bug fixed in 13.2; only user-initiated Visual Look Up sends a computed hash to Apple for matching (Howard Oakley, Jan 18, 2023; Michael Tsai roundup; pxlnv). This matters only to keep the case clean of unrelated moral panic.

D. Best Practices: safe GUI-based options for an ordinary Mac owner

Option

	

How (GUI only)

	

Prerequisites

	

Likely extra processing

	

iCloud effects

	

Data/metadata risk

	

What it does NOT establish




1. Let idle analysis finish

	

Keep Mac on power, awake at night; check status bar at bottom of Library view in Photos ("Analyzing...", counts, last update) (Apple Support)

	

Patience; days to weeks for large libraries (a 40k-image library took >1 week of 24/7 uptime, thread 255823627)

	

High but self-limiting; runs at background priority (Oakley, Oct 2026)

	

None added

	

None

	

Does not prove the mismatch line will stop; normal completion of other tasks does not clear revision-mismatched items




2. Update macOS

	

System Settings > General > Software Update

	

Backup first (always); MacBook capped at macOS 26.x (Intel support ends at macOS 27, MacRumors); Mac Studio can take 27.x

	

Each update can re-queue analysis work; precedent shows updates both fixed and triggered analysis storms

	

None expected

	

Low; a Tahoe upgrade alters the Photos library format irreversibly for older apps, so a pre-update library backup is essential (DPReview forums)

	

Absence of release-note mention means nothing; Apple fixed this subsystem silently before




3. Quit and reopen Photos

	

Normal app restart; also sign out/in of iCloud Photos is not recommended as a first step (it can trigger full re-sync)

	

None

	

Minimal

	

None

	

None

	

Does not touch the daemon's queue; mediaanalysisd runs independently of Photos being open




4. Test a separate clean library / user account

	

Photos > File > Switch Library (create new); or System Settings > add a test user, import ~100 known-good photos

	

~30-60 min; some disk space

	

Small

	

Keep test library off iCloud to avoid contaminating the experiment

	

None if the original library is untouched

	

A clean library without the signature suggests data/library-dependence (H3); it does not identify which data




5. Photos Repair Library tool

	

Hold Command+Option while opening Photos; click Repair; enter account password (Apple Support; Photos User Guide)

	

Complete current photo-library backup first (Apple-adjacent guidance and MacRumors both stress this; repair "isn't guaranteed to fix all problems") (MacRumors how-to)

	

Repair rebuilds the library database; expect re-analysis of at least parts of the library afterward

	

If iCloud Photos is on, iCloud "will check the library after the repair" for consistency (MacRumors how-to)

	

Low but nonzero; database rebuild on a 187k-asset library is the heaviest safe operation available

	

Does not prove embeddings regenerate; does not touch daemon model caches; may re-trigger the same migration path that produced the mismatch




6. Apple Support + Feedback Assistant

	

Feedback Assistant (or Product Feedback page) auto-attaches a sysdiagnose; Apple Support via getsupport.apple.com for case escalation

	

Apple ID; a sysdiagnose taken while rejections are occurring

	

None

	

None

	

None (log data goes to Apple)

	

Apple almost never replies individually; filing does not fix anything by itself, but it is the only channel that can produce an authoritative answer

Ordered recommendation for this owner: 1 (observe, in parallel with G's measurements) → 4 (clean-library differential, cheapest informative test) → 2 (update the Mac Studio to latest 27.x; keep MacBook on latest 26.x security release) → 6 (Feedback Assistant with evidence) → 5 only if search features are visibly broken or Apple Support directs it, always after backup. Reboot remains a later, out-of-scope step per the case brief.

E. Failure Modes: claimed internet cures, outcomes, and relapses

Claimed cure

	

Source type

	

Reported before/after

	

Relapse?

	

Verdict




Kill the daemon (killall / Force Quit)

	

Forums, blogs

	

Immediate CPU relief, minutes-scale

	

Yes; macOS relaunches it and it resumes the same queue (mole.fit)

	

Reversible diagnostic pause only; not a cure [established for the pause, single-source anecdotes for relief]




launchctl disable of mediaanalysisd/photoanalysisd/photolibraryd

	

Forums, AppleInsider

	

Works only with SIP disabled; kills Visual Look Up, Live Text, photos search

	

Yes if SIP re-enabled

	

Disproportionate risk (Apple: disabling SIP leaves the Mac "vulnerable to malicious code", AppleInsider); breaks features; not recommended




Delete com.apple.mediaanalysisd Caches folder

	

SEO guides, forums

	

Reclaims disk; cache regrows while work runs

	

Cache regrowth expected; one Sequoia bug victim had it regrow at ~67.9 MB/hour until the underlying bug was fixed in later 15.x (thread 255835917)

	

Safe-ish for disk symptoms; does nothing proven for embedding revision mismatches [inferred]




rm -rf whole container; symlink Caches to /dev/null

	

Forum hacks

	

Reports of it "working so far" with "no side effects noticed"

	

Untracked; deletes completed analysis state, restarting full analysis (mole.fit)

	

Destructive recipe; do not publish as guidance




Delete ~/Library/Metadata/CoreSpotlight

	

Macworld via 9to5Mac

	

Two reporters, no beachball recurrence after 2 weeks (9to5Mac, Jul 20, 2026)

	

No relapse within 14 days reported

	

Anecdotal mitigation [single-source each, low n]; triggers full Spotlight rebuild; plausible for Spotlight-embedding loops, unproven for this signature




Exclude stuck folder from Spotlight (Privacy list)

	

zerosleeps, Ask Different

	

Loop stopped for the author

	

Not applicable once excluded

	

Reasonable containment for identified file-triggered loops; author explicitly: "not a fix" (zerosleeps)




Re-encode offending images (Handbrake)

	

Ask Different comment

	

Untested at scale

	

Unknown

	

Speculative; only rational if decode errors name specific files, which this case does not show




Trash com.apple.mediaanalysisd.plist from LaunchAgents, reboot

	

Old Apple Community post (thread 7965601)

	

Claims regeneration solves it

	

Unknown; plist lives on the sealed system volume on modern macOS, so the recipe is stale

	

Outdated recipe; not viable on macOS 26/27 [inferred]

Lesson from failures: every durable improvement in the documented record came from an Apple OS update (15.2/15.5 for some, macOS 26 for zerosleeps, plausibly 26.6's Spotlight optimization), never from user-side deletion or disabling. User-side actions at best contained symptoms.

F. Strategic Recommendations (decision-oriented)

Named options with trade-offs:

Observe-and-measure (no system change). Trade-off: costs ~1-2 hours of attention over 2 weeks; zero risk. Risk: none; possible mild ongoing CPU cost while the queue drains. Establishes whether this is H1/H5 definitively.

Clean-library differential on the Mac Studio. Trade-off: ~1 hour setup; zero risk to real library. Discriminates data-dependent (H3) vs runtime (H2/H5) causes, which changes everything downstream.

Update both Macs to the latest point releases (26.7.x on the MacBook; 27.2.x on the Studio). Trade-off: pre-update library backup mandatory; may re-queue analysis. Risk: low, and it is the only intervention with a documented track record of fixing this subsystem's bugs (silently).

File a Feedback Assistant report now, in parallel. Trade-off: 30-60 minutes with sysdiagnose capture. Risk: none. Likely no reply, but it is the only path to an authoritative mapping of 0xDECAF001/005 and the only route to an eventual real fix.

Repair Library (only after 1-4 and only with backup). Trade-off: hours of rebuild on a 187k-asset library plus re-analysis; small risk on a database of that size; iCloud will re-check the library. What it buys: rules out database inconsistency cheaply-to-moderately; what it costs if wrong: nothing irreversible with a backup.

Do NOT: disable SIP/daemons, delete containers, symlink caches to /dev/null, edit private databases, or reboot (out of scope per the brief). No evidence supports any of these for a revision-mismatch signature; several destroy completed analysis work.

Single recommendation: Run option 1 + 2 concurrently, update both machines (option 3), and file the Feedback report (option 4) with the measurements attached. Hold Repair Library in reserve. Confidence: [emerging], resting on one main assumption: that the ~0.24 s rejection bursts are the tail of a draining migration queue rather than the visible tip of expensive repeated recomputation. The Action Plan below is designed to falsify exactly that assumption within two weeks.

G. Action Plan
G1. Falsifiable experiment (read-only, privacy-safe)

E1, cost-per-rejection measurement (falsifies H1 vs H5). For 72 hours, capture bounded log windows around each burst (log show --last 1h style windows rather than one 24 h query, which already proved to time out) recording: rejection-line timestamps and counts, the process field of each rejection line, and the daemon's CPU-time counter deltas (Activity Monitor "CPU Time" column, sampled at burst start/end). Record only aggregate counts and asset UUIDs (identifiers, not image content or person names). Falsifiable predictions: Benign drain (H1): rejection counts decay day over day, CPU time per burst is small relative to completed-work CPU, completed-analysis lines grow, and the affected asset set changes as the queue advances. Persistent defect (H5): CPU time per burst is large (features recomputed then rejected), the same asset UUIDs recur across bursts for 2+ weeks, and counts do not decay. Either outcome is decisive evidence for the bug report.

E2, clean-library differential (falsifies H2 vs H3). On the Mac Studio (secondary machine), create a new Photos library in a new user account, import ~100 known-good images, run the same bounded log capture for 48 hours. Signature present in a clean library → runtime/model-version issue (H2). Absent → the mismatch travels with the existing library/account data (H3). Both outcomes are publishable observations.

E3, update-and-reobserve. After updating the Studio to the latest macOS 27.x and the MacBook to the latest 26.x, rerun E1 for 72 hours on each. Given the 26.6 Spotlight-index optimization and the 27.0 indexing-bug fixes (Oakley, Oct 2026), a genuine change in signature frequency is itself a finding.

Bug report contents: exact log lines with both provided values, hardware/OS matrix (both Macs), E1/E2 results, asset counts (not content), CPU-time deltas, and a sysdiagnose captured during a burst. File via Feedback Assistant, which auto-attaches sysdiagnose (Apple Developer).

G2. Publication thresholds (what would justify each artifact)

Artifact

	

Threshold (all required)

	

Prior art relied on




Safe diagnostic guide (public)

	

E1 protocol validated on both machines; at least 14 days of data; clear separation of loop vs drain cases; no destructive steps recommended

	

zerosleeps' fs_usage/log-stream method; Oakley's log-waypoint method (Spotlight indexing); Apple's library-status doc




Bug report

	

Everything above + sysdiagnose + exact constants and hex decode + reproduction or non-reproduction in clean library

	

Apple's Feedback Assistant process; 2025 precedent of user-filed bug reports (thread 255835917)




Mitigation claim (e.g., "updating to macOS X reduced rejections")

	

Before/after on the same machine, same measurement protocol, ≥30 days follow-up (the 2026 CoreSpotlight workaround had only 14 days, n=2, and should be treated as the minimum bar, not the gold standard)

	

9to5Mac's 14-day anecdote as the floor; zerosleeps' upgrade outcome as single-source precedent




Permanent cure claim

	

Either an Apple release note naming the fix, or ≥3 independent households with verified before/after over ≥30 days post-fix

	

Apple's silent-fix track record means release notes may never appear; independent replication is the substitute standard

Follow-up duration: 14 days minimum for any public claim, 30 days preferred; re-check monthly until the queue provably drains (per E1's decay signal).

H. Source Notes

Source

	

Credibility

	

Last updated




Apple Developer Documentation: VNRequestRevisionProviding

	

5/5

	

Nov 2026 (fetched)




Apple Developer Documentation: VNRequest.currentRevision

	

5/5

	

Nov 2026 (fetched)




Apple Developer Documentation: VNRequest.defaultRevision

	

5/5

	

Nov 2026 (fetched)




WWDC22: What's new in Vision (Apple)

	

5/5

	

Jun 2022 (older but authoritative)




Apple Support: How to use the Photos Repair Library tool

	

5/5

	

current (fetch rate-limited; content corroborated)




Apple Support: Repair your Photos library (User Guide)

	

5/5

	

current




Apple Support: Check the status of your library in Photos for macOS

	

5/5

	

current




Apple Support: If your iCloud Photos aren't syncing

	

5/5

	

current




Apple Support: What's new in the updates for macOS Tahoe 26

	

5/5

	

Nov 2026




Apple Developer: Feedback Assistant

	

5/5

	

current




macOS Tahoe 26.1 Release Notes (Apple Developer)

	

5/5

	

Nov 2025




zerosleeps: mediaanalysisd has gone rogue

	

4/5 (first-person, technically rigorous, raw data)

	

Jun 15, 2025




zerosleeps: macOS 26 fixes my mediaanalysisd problem?

	

4/5 (single-source outcome)

	

Sep 17, 2025




Glyph on Mastodon: mediaanalysisd logs

	

3/5 (first-person, no follow-up)

	

Feb 2025




Michael Tsai: Fixing mediaanalysisd Storage and CPU Use

	

4/5 (respected aggregator, cites originals)

	

Jul 7, 2025 (comments through Jun 2026)




Michael Tsai: Network Connections From mediaanalysisd

	

4/5

	

Jan 25, 2023




Howard Oakley: How Spotlight indexes in Golden Gate

	

4/5 (independent macOS internals researcher, log evidence)

	

Oct 20, 2026




Howard Oakley: Is Apple checking images we view in the Finder?

	

4/5

	

Jan 18, 2023




Apple Support Community: com.apple.mediaanalysisd files filling drive (thread 255835917)

	

3/5 (multi-user first-person thread)

	

Nov 2024 - Jun 2025




Apple Support Community: Photos is endlessly analyzing (thread 255823627)

	

3/5

	

Oct 2024 - Feb 2025




Apple Support Community: MacBook Pro overheating with Tahoe 26 (thread 256138728)

	

3/5

	

Sep 2025




9to5Mac: AI photo scanning may be the reason

	

3/5 (tech press, first-person author account)

	

Jun 3, 2026




9to5Mac: beach balls fix (CoreSpotlight)

	

3/5

	

Jul 20, 2026




AppleInsider: How to stop mediaanalysisd from hogging your CPU

	

3/5 (documents the SIP-disable route; risk profile noted)

	

Ventura-era (2022-2023)




MacRumors: How to Use the Photos Repair Library Tool

	

3/5

	

Apr 21, 2023




MacRumors Forums: mediaanalysisd, photoanalysisd and photolibraryd

	

2/5 (forum, but quotes man pages)

	

Dec 2024 onward




MacRumors Forums: com.apple.mediaanalysisd taking space

	

2/5

	

2025




Ask Different: Why is mediaanalysisd taking a lot of CPU

	

3/5 (snippets only; full page blocked)

	

2024 - Jun 2026




MediaAnalysisExplorer (GitHub, skimmilk)

	

3/5 (hobbyist RE tool, consistent with independent evidence)

	

Oct 14, 2026




mole.fit: mediaanalysisd high CPU and cache

	

3/5 (vendor blog, careful sourcing)

	

Nov 3, 2026




CoreGuard: What is mediaanalysisd

	

3/5 (vendor blog, aggregates primary reports with citations)

	

Jul 7, 2026




OS X Daily: com.apple.mediaanalysisd cache bloat fix

	

2/5 (SEO-style guide)

	

Jan 8, 2025




The Mac Observer: mediaanalysisd files filling drive

	

2/5

	

2025




filiph.net: What grinds my gears: macOS 26 Tahoe

	

2/5 (single blog, page blocked from full read)

	

Sep 2026




MacRumors: macOS 27 Golden Gate roundup

	

3/5

	

Nov 2026




MacRumors: macOS Tahoe roundup

	

3/5

	

Nov 2026




Macworld: macOS 26 features and updates

	

3/5

	

Nov 9, 2026




DPReview forums: Backup Photos Library before upgrading to Tahoe

	

2/5 (forum)

	

2025




sneak.berlin: Apple Has Begun Scanning Your Local Image Files

	

2/5 (origin of 2023 claim, superseded by testing)

	

Jan 15, 2023




Pixel Envy: Finder bug sending empty API calls (Mysk analysis)

	

3/5

	

Jan 2023




DeCAF paper (arXiv 1310.1531)

	

4/5 (for the ML-term heritage only)

	

Oct 2013




Apple Support Community: mediaanalysisd=slow mac (thread 7965601)

	

1/5 (outdated recipe)

	

~2017

Conflicts and caveats:

No source anywhere maps 0xDECAF001/0xDECAF005 to a named Apple model; every specific meaning assigned to them in this report is explicitly labeled inference.

Apple never acknowledged the 2024-2025 mediaanalysisd cache/CPU bug in any release note found; "fixed in 15.x / 26" statements rest on user reports of varying quality and are labeled single-source where applicable.

The 9to5Mac CoreSpotlight fix has 14 days of follow-up on two machines: adequate for "worth trying", inadequate for "cure".

Apple Support Community closes November 20, 2026; all discussions.apple.com links may become unavailable, weakening future verification.

The two case machines are one household with possibly shared Photos/iCloud data and are counted as one incident throughout.

Open Questions

What do 0xDECAF001 and 0xDECAF005 actually denote, and which Apple component emits "revision (2)" embeddings? Only Apple (or binary RE of MediaAnalysis.framework) can answer; UNKNOWN publicly.

Is the same asset set rejected in every burst, or does the queue advance? (E1 answers this with UUID-only correlation.)

Did the rejection series begin at a datable event (OS update, iCloud merge, library migration)? A longer historical log window on the Mac Studio, which likely has a cleaner timeline, would answer this.

Do the rejections degrade any user-visible feature (semantic search quality, Memories, wallpaper suggestions, "some results may not appear" messages, as in the 2024 endless-analysis reports (thread 255823627))? If no feature is degraded, the publication case for a guide weakens to a log-hygiene note.

Is there an existing (invisible) Apple Feedback duplicate? Unknowable publicly; re-filing with the hex decode and the two-machine matrix is the best available action.

Does macOS 27.2.x change the current embedding revision on the Mac Studio? A single bounded log window after updating answers this and would be the first public observation of a DECAF-space version bump.

OBSERVED / INFERRED / PROPOSED / VERIFIED (summary ledger)

OBSERVED (case data): rejection bursts with both revision values on two machines; ~0.24 s bursts; 3,627+ matching lines over 12+ hours (lower bound); successful analysis completions in the same window; historical media-analysis process group using up to 8.99 cores; no repairs, resets or reboots performed.

OBSERVED (public record): Vision's public revision system; mediaanalysisd producing text/visual embeddings for Photos and Spotlight (Oakley, MediaAnalysisExplorer); the 2025 sibling-message runaway loop with quantified logs; version-variable fixes without Apple acknowledgement; the 2026 26.5 CPU storm and CoreSpotlight workaround.

INFERRED: DECAF001/DECAF005 are model-space tags; the guard is functioning as designed in some sense; the two machines are one incident; the rejection is plausibly migration-related; nothing yet proves wasted recomputation or corruption.

PROPOSED: the E1/E2/E3 experiment set; the option ladder in Section F; the Feedback Assistant report with hex decode.

VERIFIED (by this research): the hex arithmetic (0xDECAF001 / 0xDECAF005); the absence of any public exact-signature match as of November 14, 2026; the absence of any Apple acknowledgement of this signature; the absence of any validated cure.




## Sources (links harvested from canvas and chat)
- https://developer.apple.com/documentation/vision/vnrequestrevisionproviding
- https://zerosleeps.com/blog/2025/6/15/mediaanalysisd-has-gone-rogue/
- https://zerosleeps.com/blog/2025/9/17/macos-26-fixes-my-mediaanalysisd-problem/
- https://discussions.apple.com/thread/255835917
- https://mjtsai.com/blog/2025/07/07/fixing-mediaanalysisd-storage-and-cpu-use/
- https://coreguard.app/insights/what-is-mediaanalysisd-mac/
- https://9to5mac.com/2026/06/03/mac-beachballs-or-lagging-performance-ai-photo-scanning-may-be-the-reason/
- https://www.macrumors.com/roundup/macos-26/
- https://eclecticlight.co/2026/10/20/how-spotlight-indexes-in-golden-gate/
- https://www.macrumors.com/
- https://www.macrumors.com/roundup/macos-27/
- https://developer.apple.com/documentation/vision/vnrequest/currentrevision
- https://developer.apple.com/videos/play/wwdc2022/10024/
- https://github.com/skimmilk/MediaAnalysisExplorer
- https://forums.macrumors.com/threads/mediaanalysisd-photoanalysisd-and-photolibraryd.2445597/
- https://appleinsider.com/inside/macos-ventura/tips/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos
- https://arxiv.org/pdf/1310.1531
- https://mastodon.social/@glyph/113932588619810145
- https://discussions.apple.com/thread/256138728
- https://9to5mac.com/2026/07/20/mac-beach-balls-and-unresponsive-trackpad-this-fix-seems-to-work/
- https://apple.stackexchange.com/questions/477679/why-is-the-mediaanalysisd-taking-a-lot-of-cpu-on-macos
- https://filiph.net/text/fun-times-with-macos.html
- https://discussions.apple.com/thread/255823627
- https://support.apple.com/en-us/119921
- https://forums.macrumors.com/threads/com-apple-mediaanalysisd-taking-space.2446697/
- https://mole.fit/blog/mediaanalysisd-high-cpu-mac
- https://osxdaily.com/2025/01/08/com-apple-mediaanalysisd-cache-mac-storage-bloat-fix/
- https://www.macobserver.com/tips/how-to/com-apple-mediaanalysisd-files-filling-drive-macos-sequoia/
- https://eclecticlight.co/2023/01/18/is-apple-checking-images-we-view-in-the-finder/
- https://mjtsai.com/blog/2023/01/25/network-connections-from-mediaanalysisd/
- https://pxlnv.com/blog/everybody-panic-mediaanalysisd/
- https://www.dpreview.com/forums/threads/backup-photos-library-before-upgrading-to-macos-tahoe.4816428/
- https://support.apple.com/en-us/118221
- https://support.apple.com/guide/photos/repair-your-photo-library-pht6be18f93/mac
- https://www.macrumors.com/how-to/mac-photos-repair-library-tool/
- https://discussions.apple.com/thread/7965601
- https://developer.apple.com/feedback-assistant/
- https://developer.apple.com/documentation/vision/vnrequest/defaultrevision
- https://support.apple.com/en-us/101559
- https://support.apple.com/en-us/122868
- https://developer.apple.com/documentation/macos-release-notes/macos-26_1-release-notes
- https://www.macworld.com/article/2644146/macos-26-features-latest-update-release-date-beta.html
- https://sneak.berlin/20230115/macos-scans-your-local-files-now/
- https://github.com/Macrophage87/PhotoAlbum/issues/89
- https://github.com/LibrePhotos/librephotos/issues/758
- https://github.com/chroma-core/chroma/issues/870
- https://www.reddit.com/r/StableDiffusion/comments/10ettxl/cant_seem_to_get_embeddings_right/
- https://github.com/immich-app/immich/issues/28340
- https://github.com/openzigs/metis/issues/1918
- https://www.reddit.com/r/StableDiffusion/comments/18kizqy/stable_diffusion_embeddings_suddenly_not_loading/
- https://forum.obsidian.md/t/not-able-to-embed-images-in-notes/14447
- https://community.canvaslms.com/t5/Archived-Questions/ARCHIVED-Embedded-images-not-showing-up/m-p/61936
- https://www.reddit.com/r/logseq/comments/rhtr1b/embedded_image_didnt_show_up/
- https://github.com/kajisho5/media-analysis-skill/pull/19
- https://mastodon.social/@glyph/113925179948463409
- https://dortania.github.io/OpenCore-Legacy-Patcher/FAQ.html
- https://github.com/Yukoval-Dakia/the-learning-project/pull/2195
- https://github.com/CultureBotAI/proteintraitsmech/issues/721
- https://github.com/mokume-metal/mokume/issues/3631
- https://github.com/AM-Digital-Research-Environment/DRE-Visualizations/issues/15
- https://github.com/nathanpond/Epistrel-Console/issues/150
- https://macsecurity.net/view/661-photoanalysisd-high-cpu-process-macos
- https://apple.stackexchange.com/questions/310594/what-is-photoanalysisd-and-why-is-it-using-77-of-my-cpu
- https://mole.fit/blog/photoanalysisd-high-cpu-mac
- https://myspybot.com/photoanalysisd-high-cpu-process-macos/
- https://cloudhousetechnologies.com/blog/fix-photoanalysisd-photolibraryd-high-cpu-mac-2026
- https://iboysoft.com/wiki/photoanalysisd.html
- https://coreguard.app/insights/what-is-photoanalysisd-mac/
- https://forums.macrumors.com/threads/high-cpu-consumption-of-photos-processes-photolibraryd-and-photoanalysisd.2376267/
- https://macsecurity.net/view/671-mediaanalysisd
- https://pawisoon.medium.com/debunked-the-truth-about-mediaanalysisd-and-apples-access-to-your-local-photos-on-macos-a42215e713d1
- https://github.com/kajisho5/media-analysis-skill
- https://forums.macrumors.com/threads/trouble-importing-photos-into-photos.2433862/
- https://www.reddit.com/r/MacOS/comments/1hivi4i/how_to_embed_images_in_apple_mail_rather_than/
- https://helpx.adobe.com/lightroom-cc/kb/known-issues.html
- https://support.apple.com/guide/photos/if-you-see-unable-to-upload-in-photos-on-mac-phted9567423/mac
- https://www.getmailbird.com/attach-files-in-mac-mail-without-embedding/
- https://forums.macrumors.com/threads/unable-to-embed-images-into-mail.2462923/
- https://www.mac-help.com/threads/macbook-m2-when-using-messages-if-i-try-to-add-a-photo-using-the-photo-icon-i-get-message-unable-to-load-items.230118/
- https://apple.stackexchange.com/questions/473745/unknown-error-when-importing-photos-on-macos-sonoma
- https://discussions.apple.com/thread/256084597
- https://forums.macrumors.com/threads/mediaanalysisd-photoanalysisd-and-photolibraryd.2445597/page-2
- https://appleinsider.com/articles/23/01/21/tests-confirm-macos-finder-isnt-scanning-for-csam-images
- https://eclecticlight.co/about/
- https://x.com/howardnoakley?lang=en
- https://yespress.io/howard-oakley
- https://muckrack.com/howard-oakley/articles
- https://forums.macrumors.com/threads/louis-rossmann-says-macos-is-scanning-your-files-without-consent.2377433/
- https://support.apple.com/en-asia/HT204967
- https://discussions.apple.com/docs/DOC-250002602
- https://support.apple.com/guide/photos/repair-the-library-pht6be18f93/5.0/mac
- https://support.apple.com/en-ca/guide/photos/pht6be18f93/mac
- https://en.wikipedia.org/wiki/Freeform_(Apple)
- https://developer.apple.com/go/?id=macos-26-rn
- https://en.wikipedia.org/wiki/MacOS_Tahoe
- https://en.wikipedia.org/wiki/MacOS
- https://9to5mac.com/2025/07/10/macos-26-three-new-mac-apps-and-new-features-in-photos-messages-notes-and-more/
- https://www.reddit.com/r/MacOS/comments/u17hsa/please_help_me_stop_mediaanalysisd_process/
- https://gist.github.com/huksley/564be2c903312bcee7dffe415d128f90?permalink_comment_id=3632304
- https://www.tonymacx86.com/threads/idling-for-10-minutes-causes-sierra-to-lag-freeze-for-a-couple-of-minutes.209661/page-42
- https://gist.github.com/huksley/564be2c903312bcee7dffe415d128f90?permalink_comment_id=3684583
- https://droidwin.com/how-to-disable-mediaanalysisd-photoanalysisd-and-photolibraryd/
- https://news.ycombinator.com/item?id <tg-id>
- https://defcon.social/@mysk/109735335738390119
- https://defcon.social/@mysk/109735319047486114
- https://www.hckrnws.com/stories/34392391
- https://sideofburritos.com/blog/apple-has-not-begun-scanning-local-image-files/
- https://appletoolbox.com/why-does-photos-say-analyzing-library-on-my-apple-devices/
- https://www.reddit.com/r/ApplePhotos/comments/1fxkn8y/where_to_find_icloud_upload_progress/
- https://discussions.apple.com/thread/252577313
- https://www.reddit.com/r/mac/comments/8sz74s/apple_photos_any_way_to_see_progress_of_icloud/
- https://www.reddit.com/r/MacOS/comments/e2i9br/photos_app_has_been_analyzing_photos_library_for/
- https://github.com/cdeust/Cortex/pull/966
- https://developer.apple.com/documentation/naturallanguage/nlembedding/wordembedding(for:revision:)
- https://github.com/shreeya-g10/concept-dependency-learning-path/pull/88
- https://github.com/hanstep-lab/hanstep-AI/pull/12
- https://github.com/X1aoB/Project_Snow/pull/114
- https://github.com/hanstep-lab/hanstep-AI/issues/11
- https://github.com/cdeust/Cortex/pull/974
- https://www.reddit.com/r/MacOS/comments/1px1a3m/rolled_back_from_tahoe_to_sequoia_and_now_my/
- https://www.reddit.com/r/MacOSBeta/comments/1m6ascd/changes_to_photos_app/
- https://www.reddit.com/r/mac/comments/1l9riyr/macos_tahoe_comparison_slideshow_13_photos/
- https://www.reddit.com/r/MacOS/comments/1qmywgw/is_it_just_me_or_is_tahoe_the_absolute_worst/
- https://www.reddit.com/r/MacOS/comments/1p4ry1h/i_just_updated_to_tahoe_to_be_fair_i_hate_this/
- https://www.reddit.com/r/MacOS/comments/1nehu53/you_can_set_macos_26_tahoe_wallpapers_without/
- https://www.reddit.com/r/MacOS/comments/1qh4bpe/mac_tahoe_262_ruined_my_desktop/
- https://developer.apple.com/documentation/vision/vnrequest/supportedrevisions?language=objc
- https://stackoverflow.com/questions/58219769/which-languages-are-available-for-text-recognition-in-vision-framework
- https://pkg.go.dev/github.com/deploymenttheory/go-bindings-macosplatform/bindings/frameworks/vision
- https://developer.apple.com/documentation/vision/trackopticalflowrequest/supportedrevisions
- https://developer.apple.com/documentation/vision/vnrequest/revision
- https://developer.apple.com/videos/play/wwdc2025/286/
- https://developer.apple.com/videos/play/wwdc2019/232/
- https://developer.apple.com/videos/play/wwdc2025/360/
- https://machinelearning.apple.com/research/introducing-apple-foundation-models
- https://developer.apple.com/wwdc26/guides/apple-intelligence/
- https://wwdc.ai/2026/312
- https://developer.apple.com/videos/play/wwdc2026/326/
- https://air.build/wwdc/topics/ai-machine-learning/
- https://www.reddit.com/r/MacOS/comments/1qlnqaz/mediaanalysisd_high_cpu_fix_tahoe_262/
- https://support.apple.com/en-us/149522
- https://usekudu.com/programs/mediaanalysisd
- https://vitalsmac.com/glossary/mediaanalysisd
- https://mole.fit/pt-br/blog/mediaanalysisd-high-cpu-mac
- https://www.cultofmac.com/how-to/apple-bug-report
- https://developer.apple.com/forums/thread/712889
- https://www.idownloadblog.com/2016/04/15/how-to-submit-feedback-and-bug-reports-to-apple/
- https://discussions.apple.com/thread/250403329
- https://geekschalk.com/how-to-file-apple-bug-reports-a-step-by-step-guide/
- https://betaprofile.dev/how-to-report-a-bug-to-apple/
- https://www.bugster.dev/apple-software-bug-report
- https://dev.to/gjbianco/controlling-photoanalysisd-3j2a
- https://apple.stackexchange.com/questions/429333/apple-photos-curation-and-face-detection-stuck-forever-since-macos-big-sur-how
- https://discussions.apple.com/thread/250894530
- https://lensespro.org/what-is-photoanalysisd-on-mac/
- https://github.com/Blue-Dots-Economy/signals-search/issues/102
- https://dev.to/notaflightrisk/pinning-your-embedding-model-doesnt-pin-its-code-nj3
- https://github.com/infiniflow/ragflow/pull/22270
- https://github.com/jubishop/podhaven/issues/979
- https://github.com/benthomasson/reasons-service/issues/115
- https://github.com/int19h/jbotci/issues/959
- https://gist.github.com/huksley/564be2c903312bcee7dffe415d128f90
- https://www.positioniseverything.net/how-to-stop-mediaanalysisd-from-hogging-your-cpu-in-macos-2/
- https://mastodon.social/@glyph/113936999886150110
- https://github.com/davideast/mdmedia/pull/78
- https://www.phpbb.com/customise/db/extension/mediaembed/
- https://github.com/MaartenGr/BERTopic/issues/1707
- https://en.wikipedia.org/wiki/MacOS_Golden_Gate
- https://developer.apple.com/documentation/macos-release-notes/macos-26-release-notes
- https://support.apple.com/en-us/124963
- https://www.macoscompatible.com/story/macos-tahoe-2601-ios-2601-and-more-updates-released/
- https://macnative.com/fix-mediaanalysisd-storage-issues/
- https://spaceupmac.com/folders/com-apple-mediaanalysisd/
- https://discussions.apple.com/thread/256152410
- https://discussions.apple.com/thread/256214611
- https://www.techbloat.com/macos-tahoe-26-troubleshooting-guide-2025-common-bugs-fixes-should-you-upgrade.html
- https://discussions.apple.com/thread/256183195
- https://discussions.apple.com/thread/256143805
- https://www.reddit.com/r/ApplePhotos/comments/1nicozh/backwards_compatibility_of_photo_library/
- https://blakecrosley.com/blog/macos-27-release-notes
- https://mac.install.guide/macos/macos27
- https://setapp.com/how-to/full-list-of-all-macos-versions
- https://shattered.io/macos-27-golden-gate-arrives-intel-mac-support-ends-2026/
- https://openaccess.thecvf.com/content/CVPR2024/papers/Li_PhotoMaker_Customizing_Realistic_Human_Photos_via_Stacked_ID_Embedding_CVPR_2024_paper.pdf
- https://www.gettyimages.com/photos/embedding
- https://www.msk.com/newsroom-alerts-embedding-copyright
- https://www.gettyimages.com/resources/embed
- https://www.pexels.com/search/embed/
- https://www.projecttracks.be/en/toolbox-overview/ordenen-en-beschrijven/embedded-metadata-for-photographs
- https://embedsocial.com/blog/embed-google-reviews-photos/
- https://www.wikihow.com/Embed-Pictures
- https://www.reddit.com/r/MacOS/comments/1nubdnj/tahoe_2601_bigger_memory_hog_mediaanalysisd_200/
- https://forums.macrumors.com/threads/constant-kernel-panics-userspace-watchdog-timeout-no-successful-checkins-from-com-apple-windowserver.2222878/page-44
- https://aimultiple.com/multimodal-embeddings
- https://huggingface.co/anemll/anemll-embeddinggemma-2-ane
- https://towardsdatascience.com/multimodal-large-language-models-apples-mm1-c1e94d87a161/
- https://huggingface.co/WC-ECL/WeMM-Embedding-2B-Apple-Silicon-Quantized
- https://github.com/Maolon/embed-ane
- https://dev.to/backlog/building-a-private-classifier-with-apples-on-device-model-and-nlcontextualembedding-4ago
- https://www.reddit.com/r/swift/comments/1ouoo7c/best_multimodal_embedding_model_already_converted/
- https://developer.apple.com/forums/tags/simulator?page=5
- https://developer.apple.com/forums/thread/78875
- https://theapplewiki.com/wiki/Models
- https://developer.apple.com/machine-learning/models/
- https://support.apple.com/en-us/125375
- https://developer.apple.com/forums/tags/vision
- https://developer.apple.com/documentation/vision
- https://developer.apple.com/go/?ext=true&id=wwdc26-image-processing-q-and-a
- https://developer.apple.com/documentation/vision/vncoremlrequest/model?language=objc
- https://cleanwithcrumb.com/blog/com-apple-mediaanalysisd-cache-eating-storage-on-sequoia-the-64mb-per/
- https://talk.tidbits.com/t/howard-oakley-today-on-mac-malware-protection/34581
- https://eclecticlight.co/2026/02/10/in-the-background-spotlight-indexing/
- https://pcglance.com/mediaanalysisd-high-cpu-mac/
- https://forums.whirlpool.net.au/archive/3pxj8v8x
- https://www.mactrast.com/2026/06/are-you-seeing-beachballs-or-slow-performance-on-your-mac-apple-intelligence-could-be-scanning-you-photos/
- https://www.starryhope.com/ai/on-device-ai-cpu-tax-2026/
- https://www.ithinkdiff.com/macos-26-5-beach-ball-fix-mediaanalysisd/
- https://github.com/AndrewPopesku/video-reverse-engineering
- https://gist.github.com/IdanBanani/5be0442ad390f89259b494098f450bfd
- https://github.com/ranfysvalle02/hacking-vectors
- https://gist.github.com/whitequark/2bf913cdb65c1c313bf2d747f79223c5
- https://github.com/wtsxdev/reverse-engineering
- https://github.com/morluto/rea
- https://github.com/tylerha97/awesome-reversing
- https://github.com/morluto/awesome-reverse-engineering
- https://gist.github.com/applch/7dbff396b62d243b121d9460ed174aea
- https://developer.apple.com/documentation/macos-release-notes
- https://mjtsai.com/blog/2025/11/04/macos-26-1/
- https://forums.macrumors.com/threads/macos-tahoe-26-1-bug-fixes-changes-and-more.2470377/page-2
- https://developer.apple.com/forums/tags/photokit?page=2
- https://apple.fandom.com/wiki/MacOS_26.1
- https://stackprinter.appspot.com/export?question=477679&service=apple&language=en&hideAnswers=false&width=640
- https://zerosleeps.com/archives/
- https://archiveofourown.org/users/ZeroSleep/pseuds/ZeroSleep
- https://x.com/0sleep
- https://www.reddit.com/r/nosleepindex/wiki/archive/
- https://archives-of-zero.blogspot.com/
- https://mastodon.social/@mjtsai/114813481767125656
- https://www.cultofmac.com/news/apple-isnt-spying-on-you-in-this-particular-way-at-least
- https://mjtsai.com/blog/tag/siri/
- https://forums.macrumors.com/threads/spotlight-indexing-enabled-for-all-volumes-after-15-1-update.2441371/
- https://developer.apple.com/documentation/vision/vnrequest/supportedrevisions
