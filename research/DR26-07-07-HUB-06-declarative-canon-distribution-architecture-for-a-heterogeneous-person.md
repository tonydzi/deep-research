---
dr_id: DR26-07-07-HUB-06
title: "Declarative Canon Distribution Architecture for a Heterogeneous Personal Agent Fleet"
date: 2026-07-07
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-HUB-06): Declarative Canon Distribution Architecture for a Heterogeneous Personal Agent Fleet

> Deep research comparing 4 architectures (signed manifest+agent, receive-only Syncthing, git-pull-everywhere, canary rollout) for distributing canon/config to a multi-machine personal agent fleet, and recommending a concrete design.

## Ключевые выводы
- Four options map to different layers, not competing solutions: (A) signed manifest+hash+apply-agent = desired-state engine, (B) receive-only Syncthing = transport primitive only, (C) git pull on every node = collaboration/history primitive, (D) canary-then-fleet rollout = a policy layer that must sit on top of whichever state engine is chosen.
- Verdict: B alone is too weak (no signed desired-state, no rollback command, no canon-clean attestation), C is operator-hostile on appliance-like nodes (git pull conflicts on dirty trees turn into ops junk), D is necessary but not standalone.
- Recommended architecture: A (signed manifest + node apply agent) as the state engine, delivered over B (Syncthing as dumb transport only), with D (canary channel before stable) as rollout policy; Git stays on the hub side only, for authoring/review/release history, never on fleet nodes.
- Core design rule: never execute directly from the synced transport folder — the node agent verifies signed metadata/hashes first, then copies the approved payload into a separate managed 'live base' directory, and only that live base is referenced by Task Scheduler/launchd (prevents a compromised or stale transport view from becoming automatic code execution).
- Releases must be immutable (releases/<release_id>/...); a small signed channel file (stable.json/canary.json/pins/<node-id>.json) points which release a node should run — fleet rollback becomes 'one command': publish a newer signed channel doc pointing back to an older immutable release.
- Key separation for signing (TUF-inspired): offline root/trust-anchor key (rotated rarely), targets/release key signing each immutable manifest, online channel key signing stable/canary/pin docs with short expiration, and node Ed25519 keys used only for attestations/submissions, never for signing releases.
- Stop using MD5 as the fleet integrity signal — use SHA-256 for target files, manifests, scheduler definitions, and attestations, with canonical JSON hashing so all nodes compute identical manifest_hash.
- Live runtime must be split into live/base (agent-owned, override-first resolution) and live/overrides (node-local hotfixes); if a canon-managed file is edited directly on a node, the agent must NOT silently overwrite it — it snapshots, quarantines, reports status=blocked_local_edit, and waits for promote-to-overlay / submit-and-wait / force-apply.
- Node attestation reports must distinguish 'canon-clean' (base matches release, no overrides) from 'canon-base + overrides' (base matches, but overrides active) — a signed report includes node_id, platform, channel, desired/base release ids, manifest_hash, base_tree_hash, overlay_hash, scheduler_hash, canon_clean flag, status, and node signature.
- Reverse pipeline (node fix → canon): local hotfix lives in override tree, not canon tree; node emits signed 'non-canon-clean but healthy' attestation; on request, node packages a signed submission bundle (files + metadata + hashes + diff) to outbox/submissions/<node-id>/...; hub (which lives in Git) verifies node signature, reviews, merges, and cuts a new immutable release — after which the node override is removed and the node becomes canon-clean again.
- Platform scheduler specifics: on Windows use StartWhenAvailable for timed tasks and avoid Registration Triggers (a task with a registration trigger executes immediately when updated — dangerous for a rollout engine); on macOS keep the apply agent scheduled (not kept-alive) via launchd StartCalendarInterval + RunAtLoad, with system jobs in /Library/LaunchDaemons and user jobs in ~/Library/LaunchAgents, controlled via launchctl bootstrap/bootout.
- 9-phase live migration plan proposed: (1) inventory via read-only scanner before any enforcement, (2) stand up Syncthing transport + trust/keys in parallel with existing manual setup, (3) install apply agent in observe-only mode, (4) split current reality into base+overrides without breaking anything, (5) publish first release R1 matching majority state and preload before flipping channel pointer, (6) cut over one canary node first and require a signed ok report before expanding, (7) migrate schedulers one path at a time not all at once, (8) enable enforcement only after two successful rollback drills, (9) keep Git hub-only, keep nodes boring.

## Рекомендации / решения
- Build a 'TUF-lite' signed manifest/hash system as the desired-state engine; do not rely on Syncthing receive-only folders or git-pull-per-node as the primary distribution mechanism.
- Use Syncthing purely as a byte-transport cache for immutable release bundles and signed channel metadata — never execute or schedule directly from the synced folder.
- Implement a small cross-platform node apply agent that verifies signatures/hashes, materializes releases into a managed live/base directory, renders per-platform scheduler specs, and emits signed attestation reports.
- Separate signing keys by role (root/targets/channel/node) per TUF best practice; keep root and targets keys offline or restricted.
- Replace MD5 with SHA-256 (and canonical JSON) for all integrity hashing across manifests, files, scheduler defs, and attestations.
- Maintain a strict live/base vs live/overrides split on every node, with override-first precedence for explicitly overridable files, and treat any direct edit to a canon-managed file as a safe-stop condition (blocked_local_edit), not silent auto-overwrite.
- Route all node-originated hotfixes upward as signed submission bundles to a hub outbox; keep Git only on the hub for authoring, review, and release history — do not clone the canon repo onto fleet nodes.
- Adopt canary-then-stable channel promotion for every release (config and data changes, not just code), requiring a signed ok/canon_clean report from the canary node before wider rollout.
- On Windows, use StartWhenAvailable and avoid Registration Triggers for the apply-agent's scheduled task; on macOS, use launchd with StartCalendarInterval + RunAtLoad and manage jobs via bootstrap/bootout rather than manual plist edits.
- Follow the phased migration plan (inventory → parallel transport/trust setup → observe-only agent → base/override split → first release preload → single canary cutover → gradual scheduler migration → enforcement only after rollback drills → Git stays hub-only) rather than a single big-bang cutover.

## Сущности
- **Люди:** —
- **Компании:** Google (SRE), Microsoft, Apple, Python Software Foundation
- **Продукты/инструменты:** Syncthing, Git, TUF (The Update Framework), Windows Task Scheduler, macOS launchd, Ed25519, SHA-256, PowerShell, schtasks, launchctl

## Открытые вопросы
- How exactly should the node agent's promote-to-overlay / submit-and-wait / force-apply decision be triggered and by whom in practice (automated policy vs. manual operator action)?
- What is the concrete process/tooling for the hub to verify and merge incoming signed submission bundles from nodes (manual review only, or optional Git-patch ingestion for power users)?
- How should key rotation and offline root/targets key custody be operationalized for a small personal fleet (who holds the offline key, how often rotated)?
- What thresholds/criteria define 'two successful rollback drills' before flipping nodes from observe-only to enforce mode?
- How does this architecture interoperate with the existing multi-machine consensus/bus mechanisms (e.g., machine_bus, consensus.py) already in use across the fleet?

## Источник
- DR-ID `DR26-07-07-HUB-06` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-07-MACANTON-02-canon-dist-chatgpt.md`

## Связано
- [[machine-governance-leader-follower]]
- [[one-system-propagate]]
- [[config-safety-backup-and-migration-check]]
- [[machine-bus-telegram-rail]]
- [[vps-anchor-node]]
- [[own-fleet-peer-equality]]
- [[session-machine-tagging]]
- [[credential-store]]
- [[insight-DR-DR26-07-07-HUB-07-канон-дистрибуция-скриптов-на-гетерогенном-флоте-w]] — сиблинг-дистилляция того же DR (ChatGPT-версия того же MACANTON-02 canon-dist)
