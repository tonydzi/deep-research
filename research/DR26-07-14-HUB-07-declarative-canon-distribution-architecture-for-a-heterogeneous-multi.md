---
dr_id: DR26-07-14-HUB-07
title: "Declarative canon distribution architecture for a heterogeneous multi-machine agent fleet"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-07): Declarative canon distribution architecture for a heterogeneous multi-machine agent fleet

> Compares four update-distribution options (signed manifest+agent, receive-only Syncthing, Git pull, canary rollout) and recommends a concrete TUF-lite architecture for pushing signed canon/config to Windows+macOS nodes with safe local-override handling and rollback.

## Ключевые выводы
- Four options mapped to distinct roles: (A) signed manifest + hash + apply agent = desired-state engine, (B) Syncthing = transport primitive only, (C) Git pull per node = collaboration/version-control primitive, (D) canary rollout = a rollout policy layered on top, not a standalone answer.
- Verdict: B alone is too weak (no signed desired-state, no rollback, no canon-clean attestation), C is operator-hostile on appliance-like nodes (git pull cancels on local uncommitted changes, forcing stash/branch discipline), D is mandatory but orthogonal.
- Recommended architecture = A (signed manifest + node apply agent) as state engine + B (Syncthing) as dumb transport + D (canary-then-stable) as rollout policy; Git stays hub-side only for authoring/review/history, never on fleet nodes.
- Core rule: never execute directly from the synced transport folder — node agent verifies signed metadata/hashes first, then copies into a separate managed 'live base' dir; only that live base is referenced by Task Scheduler/launchd.
- Releases must be immutable (releases/<release_id>/...), never mutate 'current' in place; a small signed channel file (stable.json/canary.json/pins/<node-id>.json) selects which release a node runs — fleet rollback = one hub command publishing a newer signed channel pointing to an older release.
- Key separation per TUF model: root/trust-anchor key offline+rare rotation, targets/release key signs each release manifest (offline or restricted), channel key online on hub with short expiration, node Ed25519 keys used only for attestations/upward submissions (never sign releases).
- Replace MD5 with SHA-256 for all file/manifest/scheduler hashing (Python stdlib guarantees SHA-256 everywhere; MD5/SHA-1 have known collision weaknesses); canonicalize JSON before hashing so all nodes compute identical manifest_hash.
- Live runtime split into base/ (agent-owned, never touched by agent for overrides) and overrides/ (node-local hotfixes, override-first precedence); direct edits to canon-managed files are NOT silently overwritten — flagged as 'unmanaged local edit', snapshotted, and reported (blocked_local_edit state).
- Node attestation must distinguish 'canon-clean' (base matches release, no overrides) vs 'canon-base + overrides' (base matches but overrides active); signed report includes node_id, platform, channel, desired/base release ids, manifest_hash, base_tree_hash, overlay_hash, scheduler_hash, canon_clean flag, status, Ed25519 signature.
- Platform scheduling specifics: Windows Task Scheduler should use StartWhenAvailable (run after missed time) and avoid Registration Triggers (which execute immediately on update); macOS launchd should use StartCalendarInterval + RunAtLoad (runs on wake if asleep, but skips entirely if machine was off), controlled via launchctl bootstrap/bootout.
- Reverse pipeline (node fix → canon): hotfix goes into override tree first (effective immediately, no clobber risk); node emits signed 'non-canon-clean but healthy' report; on request, node packages a signed submission bundle (files+metadata+hashes+diff) to an outbox/submissions/ Syncthing path; hub (Git-based) verifies/reviews/merges into a new immutable release; only then is the node override removed.
- 9-phase migration plan: inventory drift read-only first → stand up transport+trust in parallel (no auto-apply yet) → install apply agent in observe-only mode → split current node state into base+overrides without breaking anything → publish first release matching majority state (not idealized) → cut over exactly one canary node first, require signed ok report → migrate schedulers one path at a time (not all at once) → enable enforcement only after two successful rollback drills → keep Git hub-only, keep nodes 'boring'.

## Рекомендации / решения
- Build a TUF-lite, Syncthing-delivered, manifest-driven release system with immutable releases, signed channel pointers, base/override separation, honest signed attestations, and canary-then-stable promotion.
- Never sync ad hoc scheduler state directly — put scheduler specs in the release manifest and render them per-platform (Windows XML/PowerShell registration, macOS launchd plist) from a single declarative source.
- Adopt SHA-256 + canonical JSON as the fleet's integrity truth signal instead of MD5.
- Keep Git strictly on the hub for authoring/review/release history; do not clone the canon repo onto fleet nodes.
- Roll out enforcement only after canary + small-batch verification and at least two successful rollback drills; do not flip the whole fleet from observe-only to enforce in one step.
- Treat any direct edit to a canon-managed file on a node as a quarantine event (snapshot + auto-submit + block) rather than either silently overwriting it or letting it drift invisibly — require operator choice of promote-to-overlay / submit-and-wait / force-apply.

## Сущности
- **Люди:** —
- **Компании:** Google, Apple, Microsoft
- **Продукты/инструменты:** Syncthing, Git, TUF (The Update Framework), Task Scheduler, launchd, Ed25519, SHA-256, Python standard library, PowerShell, schtasks

## Открытые вопросы
- Exact operational process for offline/at-rest storage of the root and targets signing keys is not fully specified (only that they should be offline / not in the general-purpose hub runtime).
- How the hub handles conflicting submission bundles from multiple nodes proposing overlapping hotfixes is not addressed.
- No concrete choice of bus/chat transport mechanism for publishing signed attestation reports is finalized (referenced as 'the same bus/chat mechanism you already use').
- Advanced Git-patch acceptance path for power-user nodes is mentioned as optional/enhancement but not designed in detail.

## Источник
- DR-ID `DR26-07-14-HUB-07` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- machine-bus-telegram-rail
- one-system-propagate
- machine-governance-leader-follower
- config-safety-backup-and-migration-check
- own-fleet-peer-equality
- credential-store
- apply-deliverables-immediately
- insight-DR-DR26-07-07-HUB-06-declarative-canon-distribution-architecture-for-a- — later duplicate/refined DR on the identical canon-distribution architecture question
