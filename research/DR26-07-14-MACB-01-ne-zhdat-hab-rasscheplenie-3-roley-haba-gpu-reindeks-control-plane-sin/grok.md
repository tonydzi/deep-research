# DR26-07-14-MACB-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok deep research (Expert mode, live Chrome-MCP extraction)
dr_id: DR26-07-14-MACB-01
topic: engines-sync topology + roaming single-writer (re-issue enriched 2026-07-16)
collected: 2026-07-16

chat_url: https://приватный чат
note: эхо-промпт отрезан; «97 sources» — счётчик Grok UI, полный список источников в веб-чате
---

Thought for 2m 36s

The core challenge is controlled multi-writer distribution of ~600 small Python scripts (~3 MB total) across a heterogeneous, partially sleeping 4-7 node fleet under roaming-leader governance, without a central CI server, while preserving AK-47 repairability (grep + editor) and deterministic behavior.

Syncthing remains excellent and non-negotiable for the data plane (per prior verdict). The engines/code layer must evolve from hub-centric single-writer + receiveonly peers (with patch shipping) to peer-equal roaming writer, with explicit blast-radius controls because a bad or conflicting engine can silently affect every running agent.

1. Academic & Technical Foundations

Optimistic replication (classic distributed-systems work) assumes eventual convergence via version vectors or similar; conflicts are detected and resolved at application level rather than prevented. For file-level artifacts like source code, this works for independent files but struggles with semantic dependencies (imports, shared state, execution order).

CRDTs (Conflict-free Replicated Data Types) guarantee automatic convergence for supported structures (counters, sets, text sequences via OT-like or tree-based ops) without central coordination or manual merges. They shine in local-first collaborative editing (e.g., Automerge, Yjs for JSON/text). However, applicability to whole source-code files or script folders is sharply limited: per-character or line-based merges frequently violate syntactic/semantic invariants of programs (e.g., broken imports, unbalanced blocks, or semantically wrong but mergeable Python). Metadata overhead grows; intent preservation for code is poor compared to plain text. Research and patents explicitly note that general CRDTs/OT for natural-language text do not transfer cleanly to source code without additional quality gates (syntax checks, semantic analysis) or custom "App CRDT" wrappers.

For a 600-script fleet with import graphs and runtime behavior, full CRDT layering would require heavy custom integration (not deterministic shell/grep-friendly) and is speculative/fringe for this exact use case. It is emerging and credible for structured data or real-time collab apps, but not a drop-in replacement for file sync or VCS here.

Leader-lease / roaming leader for small static clusters (3-7 nodes): Full consensus (Raft/Paxos) is overkill and adds complexity/libs forbidden by simplicity constraints. Established patterns for small/fixed fleets use deterministic priority order + heartbeat/lease with TTL + fencing. The lowest-ID or first-alive node in a static list claims/renews a time-bounded lease; split-brain is mitigated by lease expiration + explicit fencing tokens or majority checks on a small always-on anchor (here, the VPS control-plane). Kubernetes-style leases and simple MANET-inspired priority algorithms demonstrate this works reliably when node churn is low and the set is known a priori.

Tie-break by deterministic first-alive order (as proposed) is sound; the main risk is prolonged partition (e.g., VPS bill outage) — mitigated by local lease caching + explicit operator claim via control plane.

2. Industry Practice (Homelab, Dotfiles, Small Fleets, Edge)

Homelab and multi-machine personal fleets commonly mix tools:

Git bare repo on a stable anchor (VPS or always-on desktop) + clones on each machine is the dominant pattern for code/scripts. Operators edit locally, commit, push; others pull on demand or via lightweight automation (cron, watcher, or control-plane trigger). Pre-push/commit hooks serve as poor-man's CI (lint, tests, path checks). This matches the "no central CI" constraint perfectly and provides history/rollback for free.
Syncthing is widely used for continuous folder sync (including some code/dotfiles), but power users almost always add custom conflict-resolution scripts (often wrapping git merge or diff tools) precisely because native behavior is mtime + device-ID based, not semantic. Recent practitioner reports (2025–early 2026) confirm viable personal fleets using "Syncthing + python/git-merge autoscript" for nix repos, dotfiles, and scripts across phone/laptop/PC — they explicitly ignore node_modules-style dirs and accept manual or scripted hygiene.
Tailscale + git or SSH-based push/pull is common for secure, direct access without exposing ports.
Dotfiles often use chezmoi (git-backed with templating) or pure git; full script folders lean toward git or controlled Syncthing.
Small robotics/edge fleets favor git or rsync+script rituals for code, with canary/staged rollout to limit blast radius. Ansible-pull from a git repo is another lightweight pattern.

Hybrids (git for code/engines + Syncthing for data or large/binary assets) are frequently recommended and used successfully. Putting a live .git directory or bare repo under continuous Syncthing/Dropbox-style sync is widely regarded as dangerous (partial syncs or concurrent ops corrupt index/pack files).

Unison offers bidirectional sync over SSH with explicit conflict detection and rule- or interactive resolution — more deliberate than Syncthing's automatic propagation but less "always-on" seamless. It is viable for two-way code but adds ritual compared to git or Syncthing. Fossil (single-file repo + built-in wiki/web server) is simpler than git for very small teams and has autosync to reduce forks, but has a smaller ecosystem and less mindshare than git for Python-heavy workflows. Mercurial is git-like but less dominant.

3. Documented Failures & Blast-Radius Patterns

Syncthing-specific:

Conflict proliferation and hygiene burden: .sync-conflict-<date>-<time>-<deviceID> files are created on content-differing simultaneous edits (or metadata/permission mismatches, especially cross-OS). They propagate as normal files; manual or scripted resolution is required. Common triggers: fast successive saves, Windows/macOS/Linux permission/.DS_Store/case issues, ignore-permissions misconfiguration.
Silent data loss / no-conflict overwrite: Edits on one machine before full propagation can overwrite another without creating a conflict file (documented 2025 case: laptop save destroyed a week's desktop work).
LWW-like mtime behavior clobbering fixes (matches the "v3 rollback" incident).
Git-in-Syncthing corruption: Strongly warned against; concurrent or partial syncs break .git/index, HEAD, etc.
Local-additions limbo in receiveonly (exactly the current engines problem) and staged robocopy stalls producing fleet-wide stale engines.
Single-point fragility: VPS anchor downtime (bill) or hub robocopy stall left peers on stale code.

Git-related: Mainly user-error (concurrent edits without pull, or syncing the .git dir itself). Proper bare-repo + clone + hooks avoids most issues and provides easy history-based recovery.

Blast-radius patterns observed:

Continuous auto-propagation (Syncthing sendreceive) amplifies bad edits or unresolved conflicts fleet-wide quickly.
Mitigations that recur: canary node (test changes on one machine first), staged rings (writer → verified anchor/hub → others), versioned rollback (git tags or .stversions + explicit revert), kill-switch (control-plane marker or script that forces revert to known-good), and guard hooks that revert unauthorized local changes (proven in the SKILLS precedent).

4. Option Comparison (A/B/C/D) Against Constraints

(A) Syncthing sendreceive multi-writer on engines
Pros: Zero new tools; continuous sync; leverages existing mesh and quality gates.
Cons/Risks: High conflict rate on mixed-OS fleet and near-simultaneous human edits; mtime/ID resolution is not semantic (risk of broken imports or clobbered fixes); proliferation clutters the folder; silent-loss cases documented; blast radius large without extra guards. Hygiene requires ongoing scripts or manual work (burdens non-technical owner).
Fit: Poor as primary for code; viable only with heavy augmentation (custom merge scripts, as some X practitioners do).

(B) Keep receiveonly + roaming designated writer (leader-lease / first-alive + guard hook)
Pros: Minimal deviation from current + SKILLS precedent (namespace split + revert guard for canonical); explicit single-writer control reduces conflicts; lease via VPS control-plane is simple deterministic script (priority list + TTL + claim/release); sync-freshness + HMAC + propagate-verify already planned. Blast radius contained (only leader writes; others revert or ignore).
Cons/Risks: Single edit point at a time (acceptable for one human operator); lease edge cases on partitions (VPS down) require fallback; still relies on Syncthing's mtime semantics underneath.
Fit: Strong incremental path. Matches "roaming leader" governance directly.

(C) Git bare repo on VPS/hub for engines + Syncthing only for data
Pros: Industry-standard for code; excellent semantic merges, full history, blame, easy rollback/revert (directly solves v3-style and stale-engine problems); pre-push hooks implement all existing gates (lint, portability, import probe, live-run ritual, md5 verify) as poor-man's CI with zero server; heterogeneous paths handled by existing nightly lint + shebang discipline; peer-equal (any node can push when it holds the "pen"); VPS as bare anchor fits "control-plane to VPS". Distribution via pull on demand or lightweight automation (control-plane notify post-push).
Cons/Risks: Not continuous like Syncthing (requires pull step or watcher — mitigable with scripts); learning curve for pure git commands (wrapper scripts or "edit session" ritual keeps it AK-47); do not put .git under Syncthing. Concurrent pushes need coordination (lease or human via Telegram).
Fit: Excellent for code layer. Widely used in exactly this homelab/multi-machine pattern.

(D) Hybrids
Git-engines + Syncthing-data: Best of both (recommended pattern in many threads).
Syncthing + fold-style per-node staging + merger (like fold_all.py for data): Possible but adds complexity for non-append code; less natural than git merges.
Git inside Syncthing folder: Anti-pattern — avoid.
Unison/Fossil as engine layer: Viable alternatives but lower ecosystem fit than git for Python scripts.
Fit: Good when combining strengths; avoid unnecessary layering.

X practitioner signal (last ~30 days / recent 2025-2026 posts): Users running personal heterogeneous fleets (phone/laptop/PC, nix repos, scripts) successfully combine Syncthing with custom Python or git-merge conflict resolvers and report it "works well enough." They explicitly manage ignores and accept scripted hygiene. This corroborates that (A) can be made workable with extra tooling, but also shows people reach for git-like merging when conflicts appear.

Decision-Oriented Recommendation

Adopt a hybrid leaning on (C) as the engines layer — bare Git repo on the VPS anchor for canonical code, with Syncthing retained strictly for data — augmented by (B)-style roaming-leader lease/guard mechanism via the control-plane for explicit "who may write now." This is the single cleanest evolution that gives versioning/history (fixes the documented clobber/rollback/stale cases), semantic merges superior to mtime, hook-enforced quality gates, and peer-equal roaming without a "boss" hub.

Migration order (incremental, reversible, testable):
1. Snapshot current engines; initialize bare repo on VPS from it.
2. Implement mandatory pre-push (and optionally pre-commit) hook mirroring all existing gates + new propagate-verify/md5.
3. On each node: git clone the bare repo to a local working dir (or replace _imports contents); update import paths or use symlink/wrapper if needed. Keep engines receiveonly in Syncthing temporarily.
4. Add lightweight pull automation (cron/watcher or control-plane triggered git pull --ff-only or safe merge).
5. Implement simple lease/claim script on VPS control-plane (static priority list + file-based lease with TTL/renew + operator claim via Telegram or file bus). Guard hook on non-leader nodes (or all peers) that reverts unauthorized local edits in the engines dir (pattern already proven for SKILLS).
6. Operator ritual: arrive at machine → claim lease (or auto via presence) → git pull → edit/test → run gates via hook on push → release. Peers pull or receive notify.
7. Phase out Syncthing engines folder or keep it as read-only mirror/export from git if desired.
8. Add canary/staged rollout (e.g., push to canary branch or test node first) and easy rollback paths.

Single most important safeguard against fleet-wide blast radius: Mandatory pre-edit/pre-push verification ritual (existing gates + git hook) combined with git's history/rollback + staged/canary propagation. This is more robust than pure Syncthing mtime resolution and directly addresses the documented failure modes (stale engines, clobbered fixes, silent propagation). Pair it with the lease/guard so only the designated roaming writer can introduce changes that propagate.

Trade-offs & why this over pure (B) or (A): Git adds versioning and better merge semantics for code at modest operational cost (hooks + pull step); pure receiveonly+lease keeps more of the status quo but inherits Syncthing's weaker conflict model for scripts. Full (A) multi-writer maximizes conflict surface. The assumption this rests on is that the single human operator (plus occasional colleagues) coordinates via Telegram/control-plane so true concurrent writes are rare or explicitly merged in git — which aligns with "roaming leader" intent and existing practices.

This setup is deterministic, scriptable, grep-repairable, leverages the VPS anchor without making it a single point of write authority, and scales the proven SKILLS guard pattern while giving code the tool (git) designed for exactly this problem.
