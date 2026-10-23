---
dr_id: DR26-07-07-HUB-04
title: "Fleet-wide memory architecture for a multi-machine Syncthing-based AI agent system"
date: 2026-07-07
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-HUB-04): Fleet-wide memory architecture for a multi-machine Syncthing-based AI agent system

> Compares four architectures (per-node namespaces+hub-fold, Git repo, SQLite/ledger, message-broker relay) for propagating and reconciling AI-agent memory across a personal multi-machine fleet without creating Syncthing conflicts or breaking the always-loaded MEMORY.md budget.

## Ключевые выводы
- Recommended design 'A-plus': two-lane Syncthing setup — canonical share (hub send-only / nodes receive-only) plus an uplink share (send-receive) where each node writes only into its own namespace as immutable candidate files; MEMORY.md stops being a shared hand-edited file and becomes a locally-compiled derived artifact on each node.
- Weighted scorecard (conflict safety 25, silent-loss 20, AK-47 maintainability 20, budget discipline 10, supersede handling 10, hub load 10, propagation speed 5): A=4.45/5 best fit, B(Git+hub review)=3.75/5 strong second, D(durable broker relay)=3.60/5 third, C(SQLite/ledger synced)=2.95/5 worst unless SQLite kept strictly local-only.
- Syncthing only creates a sync-conflict file when the SAME file path is modified concurrently on two devices with differing content — a single-writer-per-path namespace design removes this conflict class by construction.
- SQLite permits only one concurrent writer, WAL requires all processes on the same host, and SQLite's own docs warn against network-filesystem use — syncing live DB files across intermittently-connected machines risks corruption; the only safe use of SQLite is strictly local plus exported immutable snapshots (which collapses back into architecture A).
- Core/plain NATS pub-sub is temporally coupled and at-most-once (offline nodes miss messages); JetStream adds durable persistence/replay but requires running and operating a central broker continuously — too much plumbing for a 4-6 machine personal fleet unless one is already running.
- Git gives the best audit/history/supersede handling and genuine offline transport (bundles, format-patch/am) but carries materially higher operator burden (merge conflicts, gc/maintenance, fsck) than the 'AK-47' bar requires; recommended only as an optional hub-side Git mirror for rollback/audit, not as the primary node-to-node protocol.
- Four enforced hygiene rules: all fact filenames lowercase (avoids cross-platform Windows/macOS case collisions), facts immutable after creation (corrections create a new file referencing the old one, never in-place edits), compiled MEMORY.md never synced, and a synced shared-ignore text file included via each node's local .stignore.
- Fact promotion/conflict resolution uses a priority lattice: owner fleet rule > owner node rule (shadow, node-scoped only) > verified observation > agent inference (cannot auto-supersede an owner rule); ties break by later effective_at, then later created_at, then lexicographic id — making the compiler fully deterministic.
- Central distinction: 'supersede' replaces an older fact within the SAME scope (retires it fleet-wide), while 'shadow' overrides a wider-scope fact only in a narrower context (e.g., one node) without mutating fleet canon — conflating the two risks a machine-specific lesson silently changing behavior fleet-wide.
- The dominant real-world failure mode is not data corruption but 'semantic staleness': a node offline for a while keeps writing local candidates safely (no path collision), but acts on a stale compiled MEMORY.md until it reconnects, sync converges, the hub fold job processes the backlog, and the node recompiles locally — the fix is procedural (resync → fold → recompile), not surgical.

## Рекомендации / решения
- Adopt architecture A-plus: per-node single-writer namespaces feeding a hub fold/compile pipeline, with MEMORY.md rebuilt locally on each node rather than shared and hand-edited.
- Split today's single memory share into two Syncthing folders: share-canon (hub send-only, nodes receive-only) for canonical facts/decisions, and share-uplink (send-receive on all nodes) where each node writes only its own candidates/shadows subtree.
- Never write agent output directly into canonical paths — ship a small write-wrapper per node that stamps lowercase node ID, UTC timestamp, stable ULID, and content hash, and writes only into that node's own uplink namespace.
- Keep facts as one-fact-per-file, append-only, immutable markdown with YAML frontmatter (id, scope, kind, status, priority, supersedes, shadow_of); record accept/reject/supersede/shadow decisions as separate append-only decision files rather than editing facts in place.
- Migrate as an overlay, not a flag day: (1) cold-snapshot + enable Syncthing staggered versioning, (2) add share-uplink beside the existing canon flow, (3) backfill canon into one-fact-per-file, (4) deploy shared ignore patterns before enabling writers, (5) ship write wrappers, (6) run the hub fold pipeline in dry-run for about a week before it writes canon, (7) cut MEMORY.md over to local compilation, (8) only then enable live canonical publishing.
- If stronger audit/history is wanted, add a Git mirror on the hub only (hub commits share-canon into a local repo) rather than making every node speak Git as the live memory transport.
- Do not sync live SQLite database files across machines; if SQLite is used at all, keep it strictly local-only with exported immutable snapshots as the only synced artifact.
- Avoid a durable message-broker relay (e.g., NATS JetStream) as the primary architecture unless an always-on broker is already part of the stack, since it adds a single point of failure and ongoing operational cost disproportionate to a small personal fleet.
- On namespace-violation (two nodes accidentally writing the same path and producing a Syncthing .sync-conflict file), quarantine the conflicting copies, split into separate candidate facts if both are useful, publish one clean canonical replacement, and patch the write-wrapper guardrail that allowed the collision.

## Сущности
- **Люди:** —
- **Компании:** —
- **Продукты/инструменты:** Syncthing, Git, SQLite, NATS, NATS JetStream, MEMORY.md, git bundle, git format-patch/am

## Открытые вопросы
- How aggressive should the fact-promotion rules be (dry-run week is meant to calibrate this, but the correct threshold is not established a priori)?
- Whether an optional hub-side Git mirror for audit/rollback is worth its added complexity given the AK-47 simplicity bar.
- How the priority-lattice tie-break behaves in practice once real owner-vs-agent vs multi-node collisions occur, versus the theoretical design.
- What the actual retention/pruning window should be before physically deleting superseded facts (report says 'explicit retention window' without specifying one).

## Источник
- DR-ID `DR26-07-07-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- machine-bus-telegram-rail
- one-system-propagate
- own-fleet-peer-equality
- memory-index-hygiene
- vault-data-architecture
- claude-skills-git-backup
- multi-machine-auto-consensus
- machine-governance-leader-follower
