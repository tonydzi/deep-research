---
dr_id: DR26-07-14-HUB-09
title: "Fleet-Wide Memory Architecture for a Multi-Machine AI Agent System over Syncthing"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-09): Fleet-Wide Memory Architecture for a Multi-Machine AI Agent System over Syncthing

> Evaluates four architectures (per-node namespaces+hub fold, Git, SQLite, message-bus relay) for syncing AI agent memory across a Syncthing-connected machine fleet and recommends which to adopt.

## Ключевые выводы
- Architecture A (per-node single-writer namespaces + hub fold) scores highest, 4.45/5 weighted across conflict safety, budget discipline, maintainability, silent-loss risk, propagation speed, supersede handling, hub load.
- Git-based repo with hub review/merge scores second, 3.75/5 — strongest audit/history and supersede handling, but heaviest operator burden (branches, merges, conflict markers, gc/maintenance).
- SQLite or ledger + generated MEMORY.md scores worst as a literal shared-DB design, 2.95/5: SQLite allows only one concurrent writer, WAL requires same-host shared memory, and SQLite explicitly warns against network-filesystem use due to sync/locking corruption risk.
- Message-relay-to-hub (option D) scores 3.60/5 only if backed by a durable broker (NATS JetStream); plain Core NATS pub/sub is temporally coupled and at-most-once, so offline nodes silently miss messages.
- Syncthing only creates a sync conflict when the SAME FILE is modified concurrently on two devices — so if each node writes exclusively under its own path prefix, the dominant conflict class is eliminated by construction.
- Recommended design ('A-plus'): two Syncthing folders — share-canon (hub send-only / nodes receive-only) for canonical facts+decisions, and share-uplink (send-receive on all nodes) where each node writes only into its own namespace as immutable candidate files.
- MEMORY.md should stop being a shared hand-edited file and become a locally-compiled derived artifact per node, built from canonical facts + node-local shadow overrides — this removes the single most dangerous conflict class (two machines editing one summary file) and makes budget enforcement deterministic.
- Fact files use one-fact-per-markdown-file YAML frontmatter (id, scope, kind, status, priority, supersedes, shadow_of) and are immutable after creation; corrections create new files referencing the old one, never in-place edits.
- Deterministic priority lattice for promotion to canon: owner fleet rule > owner node rule (shadows only, doesn't mutate fleet canon) > verified observation > agent inference (never auto-supersedes an owner rule); ties broken by effective_at, then created_at, then lexicographic id.
- Recommended 8-step overlay migration (not a flag day): backup+versioning, create the two folders, backfill canon into fact files, deploy shared .stignore patterns, ship write-wrapper scripts (nodes never write canonical paths directly), run the fold pipeline in dry-run for a week, cut MEMORY.md over to local compilation, then enable canonical publishing.

## Рекомендации / решения
- Adopt architecture A-plus: split Syncthing sync into share-canon (hub-authoritative, downward-only) and share-uplink (per-node upward namespaces), ranked A > B(Git) > D(durable bus) > C(shared SQLite).
- Make MEMORY.md a node-local compiled artifact (never itself synced) rebuilt from canonical facts + node shadows + a small local-reminder slot, enforcing the always-loaded budget deterministically.
- Force all node writes through a small wrapper script that only writes into share-uplink/nodes/<node-id>/candidates (and optionally shadows), never directly into canonical paths.
- Use lowercase filenames and immutable, append-only fact/decision files everywhere (corrections = new file + supersede edge, never destructive edits) to avoid cross-platform case conflicts and preserve history.
- Run the hub's dedupe/promotion/supersede fold pipeline in dry-run-only mode for about a week before letting it write canon, to validate promotion rules before they take effect.
- Optionally add a Git mirror on the hub only (commit share-canon into a local repo) to get rollback/audit history without making every node speak Git as the runtime protocol.
- Enable Syncthing staggered versioning on memory-related folders as a safety net, but still take a manual cold snapshot before migration since versioning doesn't protect against local self-edits.
- Avoid syncing live SQLite database files across machines; if SQLite is used at all, keep it strictly local-only (a cache) and sync only immutable exported snapshots via the backup API/VACUUM INTO.
- Avoid a plain pub/sub message bus for this use case unless a durable broker (NATS JetStream) is already run for other reasons, since disconnected fleet members would otherwise silently miss updates.

## Сущности
- **Люди:** —
- **Компании:** —
- **Продукты/инструменты:** Syncthing, Git, SQLite, NATS, NATS JetStream, git format-patch, git am, git bundle

## Открытые вопросы
- Exact rules for which node-authored facts count as 'reusable enough' to promote to fleet canon remain design judgement, not derived from any vendor documentation.
- Whether to actually add the optional hub-side Git mirror for audit/rollback is left as an option, not a firm decision.
- Retention window and physical-pruning policy for superseded/rejected facts is only sketched ('explicit retention window and separate backup'), not fully specified.
- Behavior when the hub or broker is down under option D is only characterized as 'promotion halts' — recovery/backlog semantics not fully detailed.

## Источник
- DR-ID `DR26-07-14-HUB-09` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- memory-index-hygiene
- machine-bus-telegram-rail
- one-system-propagate
- always-on-memory-pilot
- vault-data-architecture
- config-safety-backup-and-migration-check
- sync-via-telegram-03-mandatory
- insight-DR-DR26-07-07-HUB-04-fleet-wide-memory-architecture-for-a-multi-machine — later duplicate/refined DR on the identical fleet-memory-architecture question
