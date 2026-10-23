# DR26-07-10-MACB-01 — raw report from alternatives

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: in-house Claude subagent (DR-B), session 48e95fc8, Mac -->
<!-- topic: DR26-07-10-MACB-01 — альтернативы по трём нагрузкам (склад/конфиг/шина) -->
<!-- date: 2026-07-10 · origin: claude-inhouse -->

# Sync Architecture for a 4-Machine AI-Agent Fleet — Split by Workload

**Prepared:** 2026-07-10 · **Scope:** Windows always-on hub (2 GPU) + Windows laptop + 2 macOS · **Constraint:** AK-47 (non-engineer repairable) · Confidence `[H/M/L]`.

## 0. TL;DR — Recommended Target Architecture

**Stop forcing three different problems through one tool.**

| Workload | Recommended tool | Why | Conf |
|---|---|---|---|
| **STORE** — 180k-file vault | **Keep Syncthing** (near, not over, its ceiling) — or **Seafile** on the hub past ~300k files | <200k files sync "normally"; you're at the edge | H |
| **CONFIG** — «внутренний путь лаборатории» | **Git** (hub=origin, followers pull); **chezmoi** only if per-machine diffs bite | Versioning+rollback+no-silent-delete = literally git | H |
| **BUS** — messaging/consensus/approvals | **Telegram group primary**; self-hosted rail = **NATS** over Tailscale. **Retire the synced folder as a queue.** | Synced folder is eventually-consistent, no ordering/delivery semantics | H |

## 1. STORE — the 180k-file / 6.1 GB vault

Core number: **<~200k files normal; problems 200k–300k; >500k painful** (2h to reach 10% of 654k files reported). Small files = worst case ([forum: scalability](https://forum.syncthing.net/t/how-does-syncthing-scale-in-number-of-files-and-data-volume/24235); [#8602](https://github.com/syncthing/syncthing/issues/8602)) `[H]`. At 180k you're inside the safe band near its top edge — "fine today, plan the exit" `[M]`.

| Option | Ceiling | Conflicts | Real-time | Win+Mac | Loss risk | Simplicity | Price | Verdict |
|---|---|---|---|---|---|---|---|---|
| **Syncthing** | ~200k comfortable | `.sync-conflict` copies, nothing destroyed | Yes | Yes | Low | Medium | Free/OSS | **KEEP now** |
| **Seafile** (hub) | Default 100k client cap → raisable to 1M; 800k-file libraries reported ([forum](https://forum.seafile.com/t/safe-max-size-for-libraries/21856)) | Block-level, server = truth | Yes | Yes | Low–Med | Low-Med (Docker server) | Free CE | **BEST scale-up**: 11 GB in 6 min at 30% load vs Nextcloud 80% ([XDA](https://www.xda-developers.com/completely-uprooted-nextcloud-server-switched-seafile-instead/)) |
| **Resilio Sync** | Large folders OK (BitTorrent) | Versioning built-in | Yes | Yes | Low | High | Freemium | Viable but closed-source; little advantage |
| **Nextcloud** | **Slow/crashes on many small files** ([#691](https://github.com/nextcloud/desktop/issues/691)) | **Data overwritten with older versions reported** ([Cloudron forum](https://forum.cloudron.io/topic/12041/nextcloud-data-loss)) | Yes | Yes | **Higher** | Low-Med | Free | **AVOID for this** |
| **Obsidian Sync** | Vault-oriented, E2E, versions | Purpose-built UI | Yes | Yes | Low | **Highest** | **$8/mo (10 GB) / $16/mo (100 GB)** ([Obsidian, Jan 2024](https://obsidian.md/blog/new-sync-plans/)) | Strong if dropping self-host; vault only |
| **GDrive/Dropbox/iCloud** | Poor for many tiny files; iCloud corruption on Windows ([Obsidian Help](https://obsidian.md/help/sync-notes)) | "conflicted copy" by hand | Near | Mixed | **Med-high** | High | Free→paid | **AVOID** |
| **Mutagen** | Fast, sub-second ([mutagen.io](https://mutagen.io/documentation/synchronization/)) | Two-way-safe | Yes | Yes | Low | **Low** (dev CLI) | Free | NO — dev tool |
| **Unison** | Batch-oriented, 30-yr reconciler | Manual choose | **No** | Yes | Low | **Low** | Free | NO |

**Verdict `[H]`:** keep Syncthing today; written trigger: at ~300k files or annoying rescans → **Seafile CE in Docker on the hub**. Obsidian Sync = fallback if dropping self-hosting.

## 2. CONFIG — «внутренний путь лаборатории» to the fleet

Requirements: versioning, rollback, no silent delete → **a version-control problem, not a sync problem**.

Why Syncthing receive-only is wrong here: locally-modified file later changed by cluster → **conflict, cluster wins**; long-standing unmet FR "ignore local changes for receive-only"; mis-set "Override Changes" can **delete files on peers** ([foldertypes docs](https://docs.syncthing.net/users/foldertypes.html); [#7721](https://github.com/syncthing/syncthing/issues/7721)). No rollback, no history. `[H]`

| Option | Versioning | Silent-delete safe | Per-machine | Simplicity | Verdict |
|---|---|---|---|---|---|
| **Git** (hub=origin, pull via hook/cron) | **Full history, revert** | **Yes** | Manual | "Hub commits, others pull" = one sentence | **RECOMMENDED** |
| **chezmoi** | Yes (git-backed) | Yes | **Best-in-class** templates ([chezmoi](https://www.chezmoi.io/comparison-table/)) | Medium | ADD if Win/Mac diverge |
| **ansible-pull** | Via repo | Yes | Idempotent | "Fell apart in day-to-day" dotfiles ([jonathanbartlett](https://jonathanbartlett.co.uk/2021/05/14/a-brief-history-of-my-dotfiles.html)) | OVERKILL for 4 machines |
| **Syncthing RO** | **None** | **No** ([#7721](https://github.com/syncthing/syncthing/issues/7721)) | No | Easy on, fails core needs | **AVOID** |

**Verdict `[H]`: Git.** Followers pull-only; propose via branch; hub merges — mirrors the existing leader/follower governance with real version control underneath.

## 3. BUS — near-real-time messaging / consensus / approvals

Why a synced folder is a bad bus `[H]`: eventual consistency vs **atomic enqueue, ordering, delivery/ack** ([Solace](https://solace.com/blog/eventual-consistency-in-microservices/); [scalablethread](https://newsletter.scalablethread.com/p/why-event-driven-systems-are-hard)). No atomic write (half-synced reads), no ordering, concurrent-writer races → `.sync-conflict` on "the queue", no ack/redelivery.

| Option | What | Ops burden | Human-visible | Verdict |
|---|---|---|---|---|
| **Telegram group** | Hosted, ordered, durable | **Zero infra** | **Yes** | **KEEP primary** |
| **NATS** | One tiny Go binary; JetStream streams ([Synadia](https://www.synadia.com/blog/nats-vs-mqtt-technical-comparison-iot-fleet-management); ["NATS is tiny"](https://medium.com/@BuildShift/kafka-is-old-redpanda-is-fast-pulsar-is-weird-nats-is-tiny-which-message-broker-should-you-32ce61d8aa9f)) | Low | No | **BEST self-hosted rail** on hub over Tailscale |
| **MQTT (Mosquitto)** | IoT pub/sub | Low | No | OK; NATS simpler + replay |
| **Redis Streams** | Log/stream store | Medium | No | Overkill |
| **SQLite on hub (WAL)** | Durable queue; "100k+ ops/s WAL", ideal for bots ([DEV: SQLite queue for AI-agent orchestration](https://dev.to/minnzen/building-a-durable-message-queue-on-sqlite-for-ai-agent-orchestration-335m)) | **Lowest** | Semi | **GREAT for ledger/state** on one reachable node |
| **Tailscale + HTTP** | WireGuard mesh, free ≤100 devices ([tailscale.com](https://tailscale.com/)) | Low | No | **Transport layer**, not a bus |

**Verdict `[H]`:** Telegram primary (AK-47 answer); retire the synced folder as a queue; optional self-hosted fallback = NATS on hub over Tailscale; consensus/approval **ledger in SQLite on the hub**.

## 4. Hybrid architectures in practice

- **Syncthing files + git versioned backup of same vault** — known Obsidian homelab combo ([DEV](https://dev.to/matsch1/my-obsidian-syncthing-setup-a-self-hosted-cloud-for-notes-backups-and-more-59ic); [dsebastien](https://www.dsebastien.net/how-i-synchronize-and-backup-my-obsidian-notes/)) `[H]`
- **ansible (packages) + chezmoi (dotfiles)** — canonical split ([chezmoi #5083](https://github.com/twpayne/chezmoi/discussions/5083)) `[H]`
- **SQLite durable queue for AI-agent orchestration** — written-up pattern `[M]`
- **NATS for fleet management** ([Synadia](https://www.synadia.com/blog/nats-vs-mqtt-technical-comparison-iot-fleet-management)) `[M]`

Lesson: file-sync tools are excellent for STORE and quietly wrong for CONFIG (no versioning) and BUS (no ordering/delivery).

## 5. Price summary

Syncthing/Seafile CE/Git/chezmoi/Telegram/NATS/SQLite/Tailscale(≤100 dev) — **all free**. Resilio freemium. Obsidian Sync **$8/mo (10 GB)** — fallback only. **Total incremental cost of recommended architecture: $0.**

## 6. Order of operations

1. **CONFIG → Git now** — biggest safety win, cheapest. `[H]`
2. **BUS → off the synced folder** — Telegram primary; NATS+SQLite on hub optional. `[H]`
3. **STORE → keep Syncthing, write the trigger** (~300k files → Seafile CE on hub). `[M]`

Caveats: Syncthing ceiling is workload-dependent (community bands, not hard limits) `[M]`; Nextcloud loss reports are forum anecdotes = risk signal `[M]`; Obsidian Sync pricing in flux — verify before purchase `[M]`.
