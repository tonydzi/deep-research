# DR26-07-10-MACB-01 — raw report from syncthing

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: in-house Claude subagent (DR-A), session 48e95fc8, Mac -->
<!-- topic: DR26-07-10-MACB-01 — как чинить Syncthing (180k файлов, флот 4 машин) -->
<!-- date: 2026-07-10 · origin: claude-inhouse -->

# Fixing Syncthing for a 180k-file Obsidian fleet — cited research report

**Scope:** keeping Syncthing, tuning it for ~180k small `.md` files (6.1 GB) across 4 nodes (Win hub + Win laptop + 2 macOS, one receive-only), plus a «внутренний путь лаборатории» config dir and a message-bus folder. Sources are 2022–2026, weighted to 2024–2026. Confidence tags: **[confirmed]** = official docs or maintainer/multi-user; **[anecdote]** = single forum/issue report.

---

## 0. Headline findings (read this first)

1. **Your file count is at the edge of a real cliff.** Independent reports show throughput collapsing past ~40–60k files: from ~90 files/s down to <2 files/s around 60k during the v2.0 SQLite migration, and general "very poor performance with lots of small files" going back to v1.22. Your 180k is well past where people report pain. **[confirmed]** ([#10264](https://github.com/syncthing/syncthing/issues/10264), [#8602](https://github.com/syncthing/syncthing/issues/8602))
2. **The `.sync-conflict` flood is a topology + writer-discipline problem, not a bug.** Syncthing creates one conflict file per simultaneous divergent save; apps that autosave (Obsidian) multiply this. The only durable fixes are single-writer discipline, per-writer sharding, and `.stignore`. **[confirmed]** ([forum 22605](https://forum.syncthing.net/t/cause-and-can-i-delete-sync-conflict-files/22605), [forum 17466](https://forum.syncthing.net/t/multiple-sync-conflicts-when-writing-text-files/17466))
3. **On v2.x: pin to ≥ 2.0.7, ideally 2.1.2.** v2.0.0–2.0.5 have a migration OOM (WAL grows to ~9 GB, dies on the blocklist step). 2.0.7 reduced the page-cache to fix it. **Avoid 2.0.0–2.0.5 for a large setup.** **[confirmed]** ([v2.0.7 compare](https://github.com/syncthing/syncthing/compare/v2.0.6...v2.0.7), [forum 25194](https://forum.syncthing.net/t/2-0-3-to-2-0-6-database-migration-again/25194))
4. **You do not need custom watchdogs.** Syncthing has a native `/metrics` Prometheus endpoint (since v1.24.0) and a REST `/rest/db/status` endpoint; a ready-made alert (`SyncthingFolderOutOfSync`, fires after 1h of `need bytes > 0`) already exists. **[confirmed]** ([metrics docs](https://docs.syncthing.net/users/metrics.html), [syncthing-mixin](https://github.com/adinhodovic/syncthing-mixin/blob/main/prometheus_alerts.yaml))

---

## 1. Tuning for 180k small files on Win + Mac

All parameters below are from the [official Configuration Tuning docs](https://docs.syncthing.net/users/tuning.html) unless noted. Folder-level settings are per-folder in the config/GUI advanced settings.

### Memory / GC — the single most important lever
- **`GOMEMLIMIT`** (env var): "Setting `GOMEMLIMIT` to the desired max amount of memory Syncthing should use can improve performance by reducing GC frequency" for large setups. **Set it explicitly** — e.g. `GOMEMLIMIT=2GiB` (or `1500MiB` on the laptop). Syncthing builds and *sorts* the sync queue in RAM, "which for millions of files can use a lot of RAM." **[confirmed]**
- Set via env: Windows → machine/user env var `GOMEMLIMIT=2GiB`; macOS → `launchd` plist `EnvironmentVariables` or `~/.zshenv` if launched from shell.

### Scan / watch
- **`fsWatcherEnabled = true`** on every node — "using filesystem notifications avoids unnecessary I/O in large periodic scans." **[confirmed]**
- **`fsWatcherDelayS`**: default **10s**. For a message-bus folder → **`1`** on the *sending* side. On a folder with thousands of rapid changes, `=1` raises CPU. Keep the big vault at the **10s default**; only drop the *bus* folder to 1s (see §4 — better yet split the bus out). **[confirmed / anecdote]** ([DEV.to, 2024](https://dev.to/evoleinik/speed-up-syncthing-file-sync-discovery-from-11-seconds-to-2-345a))
- **`rescanIntervalS`**: keep periodic scans enabled even with the watcher. For 180k files, **raise from 3600s default to 86400s (24h)** to cut idle I/O, relying on fsWatcher for real-time. **[confirmed]** ([forum 10982](https://forum.syncthing.net/t/fs-watcher-and-rescan-interval/10982))
- **`scanProgressIntervalS = -1`** to disable scan-progress emission. **[confirmed]**

### Concurrency / throughput (folder-level)
- **`copiers`** / **`hashers`**: with small files the bottleneck is per-file overhead. On the **hub with fast SSD** raising helps; on **RAM-tight nodes set both to `1`**. **[confirmed]**
- **`maxConcurrentWrites`**: default **16**. Increase only alongside copiers/incoming buffers. **[confirmed]**
- **`pullerMaxPendingKiB`**: default **32768 KiB**. To reduce memory on laptop/receive-only node: **16384 KiB**. **[confirmed]**
- **`maxFolderConcurrency = 1`** if vault + «внутренний путь лаборатории» + bus share one physical disk. **[confirmed]**
- **`progressUpdateIntervalS = -1`** — "time spent on progress updates is time not spent on syncing." **[confirmed]**
- **`maxConcurrentIncomingRequestKiB`**: default 256 MiB; raise to 1024 MiB only on the hub. **[confirmed]**

### CPU cap (optional, laptop/Mac)
- **`GOMAXPROCS`** (env): setting it low reduces CPU during the initial 180k scan. **[confirmed]**

### Index DB on SSD
- v2 SQLite `index-v2/` can reach **1.4–6.8 GB**. Keep the Syncthing home/database dir on the fastest local SSD/NVMe, never network/spinning disk. **[confirmed]** ([#10264](https://github.com/syncthing/syncthing/issues/10264), [#10249](https://github.com/syncthing/syncthing/issues/10249))

> **Reality check [anecdote]:** even with SSDs, 32 GB RAM, 1 Gbps LAN, one user gave up on ~654k small files and moved to `rsync` (v1.22, 2022; issue closed "not planned"). Tuning helps but per-file overhead at 180k+ is a structural limit — see §7. ([#8602](https://github.com/syncthing/syncthing/issues/8602))

---

## 2. `.stignore` for an Obsidian vault — and how much it helps

**Load reduction comes from cutting the *file count*, which is exactly the bottleneck.** Excluding `.git` alone often removes thousands of tiny loose objects; excluding `.stversions` prevents a versioning feedback loop; excluding `.obsidian/workspace*` removes the #1 cause of device-specific churn/conflicts. **[confirmed]** ([Obsidian Hub](https://publish.obsidian.md/hub/02+-+Community+Expansions/02.05+All+Community+Expansions/Auxiliary+Tools/Syncthing), [dsebastien.net](https://www.dsebastien.net/how-i-synchronize-and-backup-my-obsidian-notes/))

Recommended `.stignore`:

```
// Device-specific Obsidian state — never sync (top cause of conflicts)
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/workspace*
.obsidian/cache

// Syncthing's own versioning + conflicts (prevents feedback loops)
.stversions
.stfolder
(?d).DS_Store
Thumbs.db
desktop.ini

// Git — huge loose-object count, sync it out-of-band if you need it
.git
.gitignore

// Plugin-generated caches / derived data (regenerated locally)
.obsidian/plugins/metadata-extractor/allExceptMd.json
.obsidian/plugins/metadata-extractor/metadata.json
.obsidian/plugins/metadata-extractor/tags.json
.smart-connections
.vault-stats
.trash

// Optional: heavy media better sent out-of-band (see §7)
// *.mp4
// *.zip
```

Notes:
- **`.stignore` is NOT synced** — place on *each device* manually. Keep a master copy in the vault (e.g. `_meta/stignore.txt`). **[confirmed]** ([docs: Ignoring Files](https://docs.syncthing.net/users/ignoring.html))
- `(?d)` prefix = "delete if this is the only thing left in a dir" — safe for `.DS_Store`. **[confirmed]**
- Consider ignoring the whole `.obsidian/` except snippets/plugin *lists* — trade uniformity for far fewer files. **[anecdote]** ([davidkopp notes](https://notes.davidkopp.de/01-pkm/obsidian-sync/))

**Quantified effect:** no universal %, but excluding `.git` + `.stversions` + caches routinely removes the majority of *inode* churn; the mechanism (fewer files scanned/indexed) directly attacks the scaling wall. **[confirmed mechanism, unquantified]**

---

## 3. Living with receive-only for config distribution («внутренний путь лаборатории»)

How receive-only actually behaves ([Folder Types docs](https://docs.syncthing.net/users/foldertypes.html), [Kastelo blog](https://kastelo.net/blog/2018-08/syncthing-recv-only-folders)) **[confirmed]**:
- All cluster changes are applied and re-distributed; **local changes are detected but NOT sent**, and **not auto-undone**.
- Local edit → red **"Revert Local Changes"** button + "Locally Changed Items". The edit isn't deleted, but it's **excluded from sync and invisible unless you look**.
- Locally-modified file later also modified by cluster → **sync conflict, cluster wins.**

Correct patterns:
1. **Treat receive-only as a strict mirror.** Never edit on a follower. Follower customization → separate non-synced overlay dir. **[confirmed]**
2. **Detect drift without a custom watchdog:** `GET /rest/db/status?folder=<id>` → `receiveOnlyChangedItems` / `receiveOnlyChangedBytes` > 0 = follower drifted. Also exposed via Prometheus `syncthing_model_folder_summary`. **[confirmed]**
3. **Re-baseline follower:** "Revert Local Changes" or `POST /rest/db/revert?folder=<id>`. Script as the standard action. **[confirmed]**
4. **Gotcha:** deletions made on other devices can be ignored / files "put back" in edge cases (xattr/ownership). Verify propagation explicitly. **[anecdote]** ([#9485](https://github.com/syncthing/syncthing/issues/9485), [#9371](https://github.com/syncthing/syncthing/issues/9371))

---

## 4. Anti-conflict strategy in production (kill the 959-file flood)

**Root cause:** two nodes modify the same file before convergence → older-mtime copy renamed `*.sync-conflict-<date>-<time>-<deviceID>.<ext>`, which then propagates as a normal file. Autosaving editors generate "one sync conflict for each save." Multi-writer coordination files are the worst case. **[confirmed]** ([forum 22605](https://forum.syncthing.net/t/cause-and-can-i-delete-sync-conflict-files/22605), [conflict docs](https://docs.syncthing.net/users/syncing.html))

Strategies, best→worst:
1. **One-file-per-writer / directory-per-writer (append-only shards).** For the bus and any multi-writer file: **never let two nodes write the same path** (`bus/from-hub.ndjson`, `bus/from-mac16.ndjson`, …). Readers merge; writers never collide → **structurally impossible to conflict.** **[confirmed principle]**
2. **Single-writer discipline for always-loaded files.** `CLAUDE.md`, `MEMORY.md`, canon → exactly one authoritative writer (hub); others receive-only. Matches the "star topology" recommendation. **[confirmed]**
3. **Lower fsWatcherDelayS on genuinely shared writable folders** → ~1s propagation shrinks the collision window — mitigation, not cure. **[anecdote]**
4. **Auto-merge for text notes:** cron script `git merge-file --union` on any `.sync-conflict-` file, "won't lose data," then delete the conflict copy. Run on one node. **[anecdote — one production user]** ([mavnn blog, 2025](https://blog.mavnn.eu/2025/08/15/conflict_free_syncthing_notes.html))
5. **Auto-clean existing 959:** conflict files are ordinary files, **safe to delete** after confirming the survivor. `find <vault> -name '*.sync-conflict-*'` → review → delete on one node (deletion propagates). Ignoring them in `.stignore` hides real divergence — prefer merge-then-delete. **[confirmed / anecdote]**
6. **`maxConflicts = 0`** disables conflict copies — loser silently overwritten. **Only** safe with true single-writer discipline. **[confirmed, use with care]**

---

## 5. Known Syncthing failures 2024–2026 — versions to avoid

| Failure | Versions | Detail | Confidence |
|---|---|---|---|
| SQLite migration OOM | **2.0.0–2.0.5** | WAL ~9.08 GB, dies on `INSERT INTO blocklists_v4`. Fixed in **2.0.7**. | [confirmed] ([forum 25194](https://forum.syncthing.net/t/2-0-3-to-2-0-6-database-migration-again/25194)) |
| Migration stalls at scale | 2.0.x | 90 → <2 files/s around 40–60k files; 60k→61k took >12h. | [confirmed] ([#10264](https://github.com/syncthing/syncthing/issues/10264)) |
| "database disk image is malformed" | 2.0.0 | Initial scan fails post-migration; delete `index-v2` to force rescan. | [anecdote] ([#10249](https://github.com/syncthing/syncthing/issues/10249)) |
| Migration fails outright | 2.0.x | Env-specific failures. | [anecdote] ([#10344](https://github.com/syncthing/syncthing/issues/10344)) |
| Dropped platform support | 2.0 | SQLite (cgo) dropped older OS/arch — check macOS versions before upgrading. | [confirmed] ([BigGo](https://biggo.com/news/202508140753_Syncthing_2.0_Drops_Platform_Support)) |
| Silent delete / "put back" edge cases | 2024, various | Delete during in-flight create/delete can delete the wrong file; empty dir trees "put back"; recreated files re-deleted. | [anecdote] ([#9868](https://github.com/syncthing/syncthing/issues/9868), [#9371](https://github.com/syncthing/syncthing/issues/9371), [ctrl.blog](https://www.ctrl.blog/entry/syncthing-deletes-recreated-files.html)) |
| "unexpected type in block list map" after RO→SR flip | 2.0.x | Folder-type change post-migration triggers warnings. | [anecdote] ([#9985](https://github.com/syncthing/syncthing/issues/9985)) |

**Version recommendation:** run **2.1.2** (latest stable) or ≥ **2.0.7**. Do NOT first-migrate a 180k setup on 2.0.0–2.0.5. If on stable v1.29.x, staying is defensible until v2 migration tested on a copy. **Back up config/db dir before any v2 migration**; expect multi-hour first launch. **[confirmed]**

---

## 6. Monitoring divergence / stuck sync — no custom watchdog needed

- **`/metrics` Prometheus endpoint**, built-in since v1.24.0. **[confirmed]** ([metrics docs](https://docs.syncthing.net/users/metrics.html))
- **REST `GET /rest/db/status?folder=<id>`** → `state`, `needBytes`, `inSyncBytes`, `receiveOnlyChangedItems`. **[confirmed]**
- **Ready-made alerts — [syncthing-mixin](https://github.com/adinhodovic/syncthing-mixin/blob/main/prometheus_alerts.yaml):** `SyncthingFolderOutOfSync` (need bytes > 0 for 1h), `SyncthingEventsDropped`. **[confirmed]**
- **Caveat:** no metric exactly equals "out of sync" — `needBytes>0` misses delete-only cases. Combine with `receiveOnlyChangedItems` + connection state. **[confirmed]** ([forum 26059](https://forum.syncthing.net/t/prometheus-metrics-and-grafana-dashboard/26059), [#9519](https://github.com/syncthing/syncthing/issues/9519))
- Without Prometheus: poll `/rest/db/status` directly ([Advanced Web Machinery guide](https://advancedweb.hu/how-to-monitor-syncthing/), [syncthing_exporter](https://github.com/f100024/syncthing_exporter)).

---

## 7. Two structural moves worth doing regardless of tuning

- **Split the 180k vault into several Syncthing folders** (by top-level dir). Multiple 30–50k folders sit *below* the observed ~40–60k cliff. The most effective single change at this scale. **[confirmed mechanism]**
- **Separate the message-bus and heavy media out of the vault folder.** Bus wants 1s watch + per-writer shards; vault wants 24h rescan + 10s watch. Media balloons transfer memory.
- **LAN-only / NAT:** discovery/relay issue, not tuning — enable global discovery + relaying, or a private always-reachable node.

---

## FIX-IT-TONIGHT CHECKLIST

**Version**
- [ ] If on 2.0.0–2.0.5 → upgrade to 2.0.7+ (prefer 2.1.2). **Back up config/db dir first.** If on v1.29.x stable, defer v2 until tested on a copy.

**Per-node env vars**
- [ ] `GOMEMLIMIT=2GiB` (hub), `1500MiB` (laptop/Mac). Optional laptop `GOMAXPROCS=2`.

**Vault folder**
- [ ] `fsWatcherEnabled=true`, `fsWatcherDelayS=10`, `rescanIntervalS=86400`, `scanProgressIntervalS=-1`, `progressUpdateIntervalS=-1`
- [ ] RAM-tight nodes: `copiers=1`, `hashers=1`, `pullerMaxPendingKiB=16384`
- [ ] Same-disk folders: `maxFolderConcurrency=1`; index DB on local SSD.

**`.stignore`** (copy to every node — NOT synced)
- [ ] §2 block; master at `_meta/stignore.txt`.

**Conflicts**
- [ ] Sweep 959: find → review → delete on one node.
- [ ] Bus → per-writer files/dirs, 1s watch, own folder.
- [ ] Always-loaded canon → single writer = hub, others receive-only.
- [ ] Optional hub cron: `git merge-file --union` auto-merge.

**Receive-only followers**
- [ ] Never edit in-place; re-baseline via `POST /rest/db/revert`.
- [ ] Alert on `receiveOnlyChangedItems > 0`.

**Monitoring**
- [ ] `/metrics` + syncthing-mixin alerts; check `state != idle` for stuck.

**Structural**
- [ ] **Split vault into 4–6 folders of ≤~40k files.**
