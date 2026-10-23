# DR26-07-14-MACB-01 — raw report from plan

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt deep research — research PLAN message (export via Downloads)
dr_id: DR26-07-14-MACB-01
collected: 2026-07-16

---

# Research Plan for DR26-07-14-MACB-01

## Operating Assumptions

I am proceeding on three explicit assumptions so the research does not stall on avoidable ambiguity. First, the engines layer is allowed to diverge architecturally from the data layer, meaning `_imports` can be isolated from the rest of the Syncthing mesh if the evidence favors `git-for-engines + Syncthing-for-data`. Second, the owner’s “AK-47 simplicity” rule means any candidate that requires a real CI server, a hosted forge, Kubernetes, or opaque controller logic is out unless it can be reduced to plain shell / PowerShell / Python scripts plus one understandable state store. Third, “roaming leader” means temporary write authority, not permanent primacy, so I will evaluate lease-like handoff and split-brain prevention as first-class concerns rather than as UI policy only. These assumptions align with the technical behavior documented for Syncthing folder modes, Git’s fast-forward and hook controls, and lease-based single-writer coordination patterns in etcd and Kubernetes. citeturn11view0turn10search6turn11view4turn13search0

## Concrete Comparison Set

I will compare four named architectures, not vague categories. For **A**, the target is plain **Syncthing send-receive multi-writer on the engines folder**, using official Syncthing behavior for conflict copies, receive-only local changes, versioning, and `maxConflicts`, plus recent issue/forum evidence on false local-additions states, conflict behavior, and git-repo corruption when `.git` is synced. For **B**, the target is **Syncthing receive-only on all non-writer nodes + a roaming designated writer controlled by a lease / deterministic first-alive tie-break**, using etcd/Kubernetes lease models as the reference design for safe pen handoff and stale-leader expiry. For **C**, the target is **Git for engines with a bare repository on VPS or hub, per-machine branches, fast-forward-only pulls, server-side `pre-receive` / `update` hooks, and client-side `pre-push` checks**, while Syncthing continues to carry only data. For **D**, I will evaluate two hybrids only: **D1 = git-for-engines + Syncthing-for-data**, and **D2 = per-node engine staging dirs + one deterministic folding merger**, explicitly testing whether fold-style semantics that work for append-only data fail for mutable code. citeturn11view0turn11view1turn3search1turn9search12turn17search9turn4search3turn13search5turn13search10turn10search2turn10search9turn0search3

The substitute set will be concrete as well. I will benchmark **Unison** as the closest “two-way sync with explicit conflict detection” alternative; **Fossil SCM** as the strongest “simple self-hosted DVCS with autosync and one-binary install” candidate; **Mercurial** as a mature DVCS with phases and flexible collaboration models; **git-annex** mainly as a negative control for “data, not code”; **chezmoi** as a dotfile-state tool rather than a fleet code-distribution system; **ansible-pull** as a pull-model exemplar for decentralized application; and **Rancher Fleet** only as a large-scale pull-based design reference, not as a deployable recommendation for this small fleet. citeturn1search6turn1search14turn14search11turn14search8turn15search3turn15search16turn6search5turn6search2turn6search13turn6search6turn11view3turn14search2

## Full Reading Set

I will read full pages and PDFs in five evidence buckets before drafting conclusions.

**Foundations for replication and conflict semantics.**  
I will use Saito and Shapiro’s *Optimistic Replication* as the baseline theory for ordering, conflict detection, propagation, and bounding divergence; Kleppmann et al.’s *Local-First Software* as the strongest concise argument for why free concurrent editing of source-like text remains painful even in local-first systems; and Kleppmann’s *CRDTs: The Hard Parts* plus current collaborative-editing work only to determine where CRDT-style merging stops being practical for ordinary script trees and inter-file invariants. These are older sources, but they are still the canonical theory layer; I will mark them explicitly as foundational rather than “current 2026 practice.” citeturn11view7turn11view6turn2search1turn2search5

**Syncthing behavior and failure evidence.**  
I will read the full Syncthing docs for folder types, versioning, GUI local-additions semantics, ignoring rules, and sync/conflict behavior; then pair them with recent GitHub issues and forum threads on conflict copies, bogus or persistent local-additions states, send-only/receive-only drift, rename inefficiency, and the repeated anti-pattern warning against syncing `.git` directories. The official docs are primary evidence for semantics; the issues/forum material will be used only as operational failure evidence and explicitly tagged `[single-source]` or `[low-authority]` where corroboration is thin. citeturn11view0turn11view1turn0search1turn3search12turn3search7turn9search12turn17search9turn17search8turn17search4turn4search3turn4search13

**Lease, leader, and split-brain mechanics.**  
I will read Kubernetes Lease and Coordinated Leader Election docs, etcd lease/election/tutorial/failure/tuning docs, and a small amount of modern database literature on leader-leases only to extract patterns that can be reduced to a shell/Python implementation: lease TTL, renew deadline, deterministic tie-break, stale-leader expiry, quorum assumptions, and the conditions under which a single-writer guarantee actually fails. I am not going to recommend “run etcd” unless the evidence says the operational burden is justified; these sources are there to steal the right invariants, not the whole stack. citeturn11view4turn7search5turn13search0turn13search1turn13search5turn13search10turn7search1turn7search15turn7search7

**Git mechanics for a no-CI fleet.**  
I will read Git’s official docs for `git init --bare`, `receive.denyNonFastForwards`, `git-receive-pack`, hook points, `git pull --ff-only`, and example server-enforced policy. That gives me the exact control surface for “poor-man’s CI”: reject force-pushes, reject non-fast-forward updates, run portability/import checks in `pre-receive`, and keep local nodes on fast-forward-only pulls so divergence is loud rather than silent. citeturn10search0turn10search6turn0search3turn10search2turn10search9turn10search14

**Industry pull-model references and substitutes.**  
I will read official `ansible-pull` documentation, Rancher Fleet pull-model guidance, Fossil’s concepts/autosync documentation, Unison’s manual, and Mercurial’s collaboration/phases material. The point is not to import enterprise tooling wholesale; the point is to see how mature systems constrain write authority, surface conflict, and keep offline work tolerable without pretending that mutable code is append-only data. citeturn11view3turn14search2turn14search8turn14search11turn1search6turn15search2turn15search3

## Source Quality Rules and Conflict Reconciliation

The evidence hierarchy will be strict. Primary sources first: official docs, source-controlled issue trackers, and peer-reviewed / archival papers. Secondary sources next: vendor engineering blogs where they describe concrete mechanisms rather than marketing. Community forums and personal blogs will only be used for failure cases that official docs rarely document, and every such claim will be marked `[single-source]` or `[low-authority]` unless I find independent corroboration. Where sources disagree, I will not average them away; I will state the disagreement, identify which source is closer to ground truth, and explain whether the disagreement is about semantics, operator experience, or a bug/regression window. That matters here because Syncthing’s official model can be correct while recent field reports still show pathological edge cases in receive-only and conflict handling. citeturn11view0turn9search12turn17search9turn17search16turn4search3

For recency, I will overweight 2025–2026 material for product behavior, bugs, and current practice, but I will keep older theory sources where the field still relies on them. In the final write-up, I will say explicitly when the best source is old rather than laundering it as current doctrine. That is especially important for optimistic-replication theory, CRDT literature, and some DVCS alternatives like Fossil and Unison, where the ideas are mature but not “new.” citeturn13search13turn17search0turn17search9turn11view7turn11view6turn1search6turn14search11

## Evaluation Rubric and Stress Tests

Every candidate will be scored against the same concrete rubric: concurrent-edit safety for `.py` files; how conflicts surface and how much silent damage is possible; whether write authority is explicit or emergent; repairability with grep/text editor only; cross-platform path and encoding friction across Windows/macOS/Linux; offline tolerance when nodes sleep; rollback speed; how easily one bad engine propagates fleet-wide; and whether the design supports staged rollout, canarying, and a hard kill-switch. I will also test each option against your named incidents: stale engines after propagation failure, receive-only local-fix limbo, single-writer host failure, Syncthing last-writer / conflict overwrite patterns, and near-simultaneous edit sessions creating `*.sync-conflict` copies. The final recommendation will only be accepted if it has a clear answer to all five incident classes. citeturn11view0turn11view1turn3search7turn17search8turn13search10turn10search2

I am also going to run one specific “anti-pattern audit.” That audit will test whether the allegedly simple approach is merely shifting complexity into hidden states: for example, syncing a working tree plus `.git` through Syncthing, relying on uncontrolled send-receive merges for code, or building a fold-style merger for code files whose correctness depends on import graphs and inter-file invariants rather than append semantics. Fossil autosync, Unison conflict prompting, and Git FF-only workflows will be compared directly on that axis rather than treated as stylistic preferences. citeturn4search3turn4search13turn14search8turn1search6turn10search2

## Final Output Structure

The final deliverable will have this structure:

- **System model and threat model** — what counts as canon, who may write, what can fail, and what “blast radius” means in this fleet.  
- **Evidence review by mechanism** — optimistic replication, CRDT limits for source trees, and lease/single-writer patterns.  
- **Option-by-option architecture analysis** — A, B, C, D, each with operational semantics, failure modes, and implementation sketch.  
- **Failure catalogue** — documented Syncthing, git-on-sync-folder, and fleet-wide bad-rollout incidents, with mitigations.  
- **Comparison table** — named options scored on merge semantics, split-brain resistance, offline behavior, repairability, rollback, and blast-radius containment.  
- **Decision section** — concrete named options, migration order, one recommended choice with confidence tier, and the single safeguard that matters most against fleet-wide damage.  
- **Source list** — every source used, with date if available, and explicit `[single-source]` / `[low-authority]` flags where applicable. citeturn11view0turn10search0turn14search2turn1search6

That is the plan I will execute. I already have the core source map in hand, and the next step is full-page / full-PDF reading plus contradiction resolution, not more broad searching. citeturn11view7turn11view6turn11view3turn14search8