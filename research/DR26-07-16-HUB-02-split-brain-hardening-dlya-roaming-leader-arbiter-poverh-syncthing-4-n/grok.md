# DR26-07-16-HUB-02 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (SuperGrok, Expert mode, аккаунт Tony0x.eth)
topic: "DR26-07-16-HUB-02 — Split-brain hardening for roaming-leader arbiter over Syncthing file-sync consensus"
date: 2026-07-16
collected: 2026-07-16 ~21:45 Lisbon, hub хаб, сессия f95d5376 (live Chrome-MCP, get_page_text)

url: https://приватный чат
notes: >
  Verbatim ответ Grok (Expert, thought 2m50s, "77 sources" — список источников в UI свёрнут,
  инлайн-цитаты частично: abstractalgorithms.dev, hackernoon, docs.gluster, docs.syncthing, ditto, primitives).
  Эхо промпта отрезано. Цитаты Grok проверять выборочно (высокий риск выдуманных ссылок — playbook §2).
---

# DR26-07-16-HUB-02 — Grok (Expert) verbatim

Thought for 2m 50s

Split-brain hardening for a roaming-leader arbiter over Syncthing is a classic distributed-systems problem adapted to an eventually-consistent, no-RPC, file-shard substrate with sleeping nodes. Your setup (per-machine single-writer append-only JSONL shards + presence stamps + priority-based roaming arbiter + verified activation + human escalation rail) already bounds damage well: Tier-2 always human, shards prevent raw data corruption, and decisions are the only forkable thing. The core risk is concurrent auto-arbitration during sync partitions (or one-sided sync views), which becomes acute once you enable quorum or higher autonomy. [Abstractalgorithms]

I researched via extensive web searches (foundational papers, recent 2025–2026 analyses, production post-mortems, Gluster/StarWind thin-arbiter precedents, Syncthing docs), targeted browsing of key sources on fencing/leases/quorum/CRDTs, and real-time X semantic/keyword searches for practitioner talk (homelab, self-hosted, Raft/split-brain war stories, file-sync coordination). X yielded limited specific recent Syncthing/personal-fleet stories (mostly general consensus hardness and split-brain awareness in Raft/DB primaries); findings rest primarily on corroborated web sources (Kleppmann-era fencing work, recent split-brain explainers, Gluster thin-arbiter docs, Syncthing conflict handling). No fabricated URLs or quotes. Contrarian takes (e.g., "just use CRDTs and accept divergence for personal systems") are surfaced explicitly. Recency prioritized; older fundamentals (Raft terms, Kleppmann fencing) flagged as such.

Citations use real retrieved working URLs + dates where available. Single-source or low-authority claims tagged. Confidence tiers applied per key finding. Disagreements preserved (e.g., quorum vs. eventual/CRDT camps). Epistemic neutrality: map evidence for/against; current consensus is quorum + fencing for safety-critical leadership; file-sync adaptations are thinner / more speculative.

## 1. PRIOR ART MAPPING (techniques for leader/arbiter election over eventually-consistent file-sync, no RPC/heartbeats)

Lease-based leadership with fencing tokens (established, Kleppmann 2016 foundational + widely corroborated in 2025–2026 analyses).
Core idea: time-bounded exclusive claim (lease) renewed by heartbeats; fencing token (monotonic counter/epoch) issued on grant; storage/resource rejects writes with stale token. Prevents "zombie/stale leader" writes after pause/partition. [Hackernoon]

Mapping to your file-shard world: Presence stamps already approximate heartbeats/leases (freshness threshold ~45–70 min). Add a lightweight synced "arbiter-claim" file (or append-only claim log) holding current holder + monotonic epoch/token + Ed25519 sig. Arbiter "renews" by rewriting with same/higher epoch on its presence tick. Events/COMMITs carry the epoch. Peers (and storage layer = your ledger processor) reject/ignore actions with epoch < locally-seen max. Syncthing provides the "consistent store" substrate (eventual). Cost: file I/O + sync latency (seconds to hours on sleep); simple Python file ops. Failure modes: concurrent claims → Syncthing .sync-conflict file (deterministic resolution via modtime + device ID, but creates manual cleanup); delayed visibility of new epoch during partition; clock skew irrelevant if using logical epoch instead of wall time. Industry: etcd/ZooKeeper/K8s Leases + fencing; HDFS write leases. Strongly recommended as baseline. [Primitives]

Epoch/term numbers (Raft-style) adapted to append-only file logs (established fundamentals, Raft 2014; adaptations speculative for pure file-sync).
Raft terms are monotonic epochs; higher term invalidates lower; leader completeness + log matching properties. [Pk]

Mapping: Shared "current-epoch" file or header in arbiter-claim log. Arbiter increments epoch on taking role (or on detected higher claim). Include epoch in every PROPOSE/ACCEPT/COMMIT/VERIFY. On rejoin/processing, peers ignore lower-epoch actions (fencing). Your existing per-shard append-only + single-writer helps (no concurrent writes within shard). No AppendEntries RPC needed—replication via Syncthing. Cost: epoch management + replay logic on startup (~lightweight). Failure modes: stale epoch visible in one partition (fencing mitigates); term file itself can conflict under Syncthing (use priority or append-only claims history). Real systems use terms in Raft WALs (local append-only files) + network; pure file-sync version lacks direct heartbeats/log replication checks—your presence stamps substitute partially. Not full Raft safety without additional invariants.

Witness/tiebreaker nodes (established in HA storage; e.g., Gluster thin arbiter, StarWind witness for even-node clusters). [Docs.gluster]

Mapping: Your roaming priority list + fresh-stamp arbiter is already a dynamic witness/tiebreaker. Enhance with a lightweight "thin arbiter" role for Mayak VPS (always-on, low-resource): it can host a metadata-only claim/epoch file or act as tiebreaker when multiple claim fresh stamps. Thin arbiter precedent in Gluster (metadata-only for quorum in stretch/2-data setups; marks entire replica bad on any stale file to prevent split-brain). Cost: minimal extra sync. Failure modes: witness itself partitioned or sleeping (your priority list already handles via freshness). Good fit for your 4-node + sleeps.

Rendezvous through a third rail (Telegram group as low-bandwidth side channel) (speculative for automated coordination; established for alerts/escalation).
No strong prior art for fully automated leader election over TG, but side channels exist for heartbeats/claims in decentralized/IoT/personal systems. Your existing human TG rail is a strength.

Mapping: Each node (or current arbiter) posts a signed heartbeat + current epoch + claim timestamp to the shared group (near-realtime for internet-connected nodes). All nodes read and can challenge/counter-claim. Arbiter claim must be posted + unchallenged for T minutes before acting on Tier-0/1. Cost: TG API calls (light); audit trail bonus. Failure modes: TG outage/partition (moves problem, but personal fleet has phone fallback + human escalation); one node without TG access. Improves visibility over pure file sync (seconds vs. minutes/hours). Minimal protocol below (Q5).

Invariant-based approaches (CRDTs / monotonic epochs) that make split-brain harmless rather than impossible (emerging for local-first/collaborative; established math, practical libs like Yjs). [Ditto]

CRDTs guarantee convergence for concurrent ops (commutative or with defined merge) without coordination. Monotonic epochs/vector clocks provide causal ordering.

Mapping to file-shard ledger: Design proposals/operations as CRDT-friendly where possible (e.g., counters additive, sets add-wins, or proposal acceptance as monotonic "seen" flags). For decisions, use vector clocks or hybrid logical clocks on events for causal merge on rejoin; conflicting COMMITs auto-merge or flagged. Makes divergent arbiter decisions "harmless" (eventual convergence or safe superseding) rather than requiring perfect prevention. Cost: CRDT impl or simple vector-clock library in Python (manageable). Failure modes: not all operations commute (Tier-0/1 decisions may need explicit conflict rules or human); state bloat if not compacted. Contrarian view (surfaced in local-first discussions): for personal/non-critical fleets, embrace eventual + good merge rules over trying to prevent all partitions—simpler than Raft. Your single-writer shards + append-only already help (less mutable state). Good complement to fencing, not replacement for safety-critical auto-arbitration.

Overall consensus (established): Quorum + fencing + leases for production leader election; pure file-sync is thinner (mostly conflict-file handling in Syncthing/rclone bisync, not consensus). Your hybrid (presence + priority + file claims) is pragmatic adaptation. [Abstractalgorithms]

## 2. DETECTION (cheap deterministic detectors for "I might be in a partition")

Ranked by signal/noise for your 4-node fleet with sleeping members (high = strong, low false positives/negatives; cheap = local computation or existing data):

1. Syncthing REST API connection state as LOCAL oracle (highest S/N, cheap): GET /rest/system/connections returns per-device connected: true/false + type (tcp/quic/relay) from this node's daemon perspective only. Independent of file-sync state or partition visibility to others. If I expect hub/Mayak connected but see disconnected (while my stamps claim freshness and others' stamps look stale), strong partition signal. Syncthing docs confirm this is transport-layer view. Easy Python requests to localhost:8384 (with API key). False positive low (sleeping nodes legitimately disconnected); combine with freshness threshold. [Docs.syncthing]
2. Ledger-progress divergence (high S/N): Compare last event timestamps/IDs per shard in my view vs. expected (others' shards not advancing or no fresh events from them in > expected sync window). My shard advancing with local proposals while others' shards show stale last-modified = one-sided progress (classic partition symptom). Cheap: scan local JSONL tails or maintain lightweight "last-seen" index. Good with sleeping nodes (expect long gaps).
3. Stamp-age asymmetries (medium-high S/N): Already have presence stamps. Asymmetry (I see peer X stale but my stamp fresh to myself, and perhaps TG cross-check shows X active) suggests asymmetric view. Combine with connections. Noise from legitimate sleeps; threshold tuning needed.
4. Telegram-rail liveness cross-check (medium S/N, adds external dependency): Post signed heartbeat + epoch; observe others' posts or acks. Detects if file stamps say "fresh" but TG silent (or vice versa). Near-realtime bonus. Noise from TG delays/outages; useful corroboration, not sole signal.

Recommended combo (cheap & robust): Primary = local Syncthing connections + ledger divergence. Secondary = stamp asymmetries + optional TG. On suspicion (e.g., < expected peers connected + divergence or asymmetry), enter safe-mode. Deterministic, no extra network beyond existing.

## 3. SAFE-MODE SEMANTICS (when partition suspected)

Industry experience (Raft/etcd/ZK/K8s, production post-mortems): Prefer "no progress" or step-down over risking split-brain; consistency > availability for critical leadership. Quorum enforcement common; fencing/STONITH as backup for zombies. For small N with sleeps/dynamic membership: complex dynamic quorum painful. [Abstractalgorithms]

Compare options for your N=4 + Tier-0/1 auto + human rail:

(a) Freeze auto-arbitration entirely (escalate to human): Simplest, safest. On partition suspicion → OBSERVE_ONLY mode (already have inert builds). Ping Anton via TG with context. Matches existing escalation rail. Trade-off: temporary unavailability for Tier-0/1 (acceptable if human cheap/fast). Risk: over-freezing on false positives (tune detection).

(b) Allow only proposals whose effects are idempotent/reversible: Limits blast radius. E.g., read-only or easily-undoable Tier-0. Requires classifying proposals. Trade-off: reduces autonomy; still needs detection. Good middle ground.

(c) Tentative-commit with challenge window on rejoin (already built for quorum Phase A): Proceed tentatively; on rejoin, challenge/override conflicting decisions within window. Excellent for your append-only ledger + rejoin scenarios. Trade-off: temporary divergence possible (but bounded + detectable); requires good conflict detection (Q4).

(d) Fencing: arbiter actions carry epoch that peers refuse if higher seen: Core safety net (see Q1). Even if two arbiters act, higher-epoch wins or lower rejected on rejoin/processing. Pairs beautifully with (c). Industry standard (Raft terms, ZK zxid, Kleppmann fencing). Low complexity add-on.

Worth complexity at N=4? Combination of (d) fencing + (c) tentative/challenge + (a) human escalate on strong suspicion is pragmatic and low-regret. Full dynamic quorum less worth it (sleeps often prevent clean majority; your roaming + presence already approximates). STONITH irrelevant (no physical control, personal fleet). Evidence: production systems layer defenses (quorum primary + fencing backup); personal/homelab favors simplicity + human rail. [Hackernoon]

## 4. RECONCILIATION ON REJOIN (if two arbiters acted)

Detect conflict set precisely: Scan merged ledger for (i) multiple COMMIT/tie-break events for the same proposal ID with different decisions, or (ii) overlapping time windows where two different machines recorded arbiter COMMITs for Tier-0/1 proposals (use event timestamps + machine ID + proposal metadata). Your append-only + per-shard structure + Ed25519 sigs make this deterministic and auditable. Vector clocks or epochs on events speed causal filtering.

Merge disciplines:

- Deterministic total ordering (ts + machine-id tiebreak or higher epoch wins): Simple, automatic. Higher epoch (from fencing) supersedes. Preserves append-only history; mark superseded events. Low complexity.
- Epoch-wins: Natural extension of fencing. Highest epoch's decisions authoritative; lower-epoch ones logged as "overridden during partition."
- Human review queue of conflicting COMMITs only: Safest for Tier-2 spirit. Queue only the conflicting decision events (not whole ledger) for Anton review/approval. Non-conflicting events apply normally. Matches your risk-tiered + human escalation model. Least "magic," most repair-friendly.

Least-painful overall: Epoch-wins for auto Tier-0/1 (fast convergence) + human queue for any Tier-2 or ambiguous overlaps. Full deterministic replay possible on any node. Industry (Raft log matching + snapshots) favors keeping history + deterministic resolution; CRDT-style merge as complement for commutative parts. [Ditto]

## 5. THE TELEGRAM SIDE CHANNEL

Using shared TG group as secondary liveness/epoch beacon (signed heartbeat + current epoch posted/read by all) is a sound incremental safety improvement, not just moving the partition problem—if used with explicit claim + challenge protocol. It provides near-realtime independent visibility (vs. Syncthing's variable latency) and human-auditable trail. Your machines already can read/write it; human escalation exists.

Minimal protocol for real safety gain:

- On becoming/renewing arbiter: post signed message "{machine} claims arbiter role, epoch=E, timestamp=T, will act on Tier-0/1 after unchallenged for Y minutes (Y=2–5)".
- All nodes: read group; if see competing claim or higher epoch, post challenge/counter-claim immediately.
- Arbiter: only act after its claim visible + unchallenged for Y (local clock or TG timestamp). Include epoch in subsequent events.
- On partition suspicion or rejoin: cross-check TG posts vs. file stamps/ledger.

Benefits: explicit coordination signal; detects asymmetric views; epoch beacon helps fencing. Real improvement for sleeping nodes (TG often reachable when file sync lags). Risks/limitations: TG itself can partition/outage (mitigate: local fallback to file-only + human ping; signed so tamper-evident); one node without internet/TG access falls back gracefully. Does not replace file-based fencing/detection—complements it. [speculative for fully automated use; established pattern for alerts/side channels in personal/decentralized systems]. Better than pure file for visibility; acceptable extra dependency for personal fleet.

## 6. RECOMMENDATION (minimal, repair-friendly "AK-47" design for 4-node fleet)

Core 2–3 mechanisms, in priority order (build incrementally; each adds ~100–200 LOC; total addition ~300–500 LOC to your ~1500-line single-file Python engine; all Python stdlib + existing crypto/requests/TG bot if any; no heavy deps):

1. Monotonic epoch + fencing in arbiter claims and events (first, highest leverage, ~150 LOC): Add/shared synced file (e.g., arbiter-claims.jsonl append-only or simple claim file with priority tiebreak on conflict). Current arbiter writes: epoch (increment on claim/takeover), holder, timestamp, sig. Every outgoing event/COMMIT carries current epoch. On receive/process: track max seen epoch; ignore/reject lower-epoch actions (fencing). Your roaming priority + presence already selects; epoch prevents stale post-partition actions. Handles file conflicts via append or device-priority. Repair-friendly: inspect file + logs; human can manually bump epoch or edit claim.
2. Partition detection + safe-mode entry (next, ~150 LOC): Poll local Syncthing REST /rest/system/connections (connected bools) + compute ledger divergence (last event per shard) + stamp asymmetries. On suspicion threshold (tunable: e.g., expected peers disconnected + divergence or asymmetry): enter safe mode = tentative-commit with existing challenge window (or freeze auto-arbitration + human TG ping for stronger suspicion). Idempotent-only as option. Leverages what you have (presence, verified activation, Phase A tentative).
3. Telegram explicit claim + challenge beacon (third, optional but high value, ~100 LOC + existing TG integration): On arbiter claim/renew: post signed claim + epoch + "unchallenged Y min before act". Nodes respect/challenge via posts. Cross-check vs. file state. Use for liveness corroboration and epoch broadcast. Fallback to file-only if TG unreachable.

Order: Fencing/epoch first (immediate safety for rejoin/stale actions), then detection+safe (prevents concurrent acting), then TG beacon (visibility boost). Test with simulated partitions (kill Syncthing links, sleep nodes, inject delays).

What NOT to build at this scale (and why):

- Full Raft (or Paxos): Requires reliable messaging or complex file-based log replication + heartbeats + leader completeness invariants. Too much complexity/error surface for ~1500 LOC total Python; sleeps make dynamic membership/AppendEntries-style checks brittle. Industry Raft libs exist but add deps + maintenance you don't want for personal fleet. Your file-shard + presence is lighter-weight adaptation.
- Real BFT (Byzantine fault tolerance): Overkill—your fleet is trusted personal machines (crash + partition model, not malicious). Adds crypto rounds, view changes, etc., with no benefit.
- Complex dynamic quorum voting: Sleeps often prevent clean majority; your roaming priority already approximates witness/tiebreak. Dynamic membership tracking adds state machine complexity without proportional gain.
- Full CRDT library for everything: Useful complement for mergeable state, but not core for arbiter decisions (use simple vector/epoch + human for conflicts instead).

Trade-offs & risks of recommended design: Adds modest complexity (epoch mgmt, detection thresholds, TG posts) but keeps single-file repairable by non-technical owner (inspect JSONL/files + logs + TG history; manual epoch bump or claim edit as last resort). Improves safety before quorum without over-engineering. Main risks: epoch-claim file conflicts (mitigated by append/priority + fencing); TG as extra dependency (fallback + human rail); false-positive safe-mode (tune + human override). Benefits: principled hardening, bounded divergence, easy rollback. Matches "AK-47" (simple, reliable, understandable, fixable by owner).

Single recommendation: Implement epoch/fencing (core) + detection + safe-mode (tentative/challenge or freeze + human) + optional TG claim beacon, in that order. This gives strong practical protection for your workload at minimal added complexity.

Main assumption it rests on: Trusted personal fleet (crash/partition/sleep model dominates; no Byzantine actors; human escalation is cheap/fast and ultimate backstop). Confidence tier: [established] for fencing/epochs/quorum principles (decades of production use + recent analyses); [emerging] for exact file-sync + TG adaptations in small personal fleets (pragmatic but less battle-tested than etcd/Raft). Evidence quality high on core mechanisms; thinner on personal Syncthing-specific war stories (X/web show general awareness but not exact matches).

Decision-oriented options summary (for your design decision next week):

- Option A (minimal change): Just add epoch/fencing to existing roaming + presence. Trade-off: good rejoin safety, still vulnerable to concurrent acting during partition. Risk: low complexity, medium safety gain.
- Option B (recommended): Epoch/fencing + detection + safe-mode (tentative/human). Trade-off: strong prevention + bounded damage + repairable. Risk: tunable false positives; still relies on human for Tier-2.
- Option C (more ambitious): Above + full TG claim protocol + CRDT-inspired merge for proposals. Trade-off: highest visibility/autonomy. Risk: extra deps/surface; overkill if human rail suffices.
- Avoid: Full Raft/BFT or complex quorum—complexity > benefit at N=4 personal with sleeps.

## 1-page executive summary in Russian

Краткое резюме: Укрепление защиты от split-brain для roaming-arbiter в системе на базе Syncthing (4 машины, личный флот Антона)

Проблема: Presence stamps и ledger идут по одному eventually-consistent каналу (Syncthing). При partition (или частичном sync) два узла могут посчитать себя arbiter'ом → concurrent auto-commit Tier-0/1 → fork решений (split-brain). Ущерб сейчас ограничен (Tier-2 — человек, шарды single-writer, quorum отключён), но перед включением автономии нужно principled hardening.

Ключевые находки из исследования (с источниками):

- Классика: quorum + fencing tokens (монотонный epoch) + leases с heartbeats. Fencing отодвигает безопасность на уровень ресурса (ledger отклоняет stale actions). Kleppmann 2016 + недавние анализы 2025–2026 (abstractalgorithms.dev, hackernoon).
- Адаптация к file-sync (без RPC): эпохи в shared claim-файле + presence как heartbeat; Syncthing .sync-conflict для concurrent writes (deterministic по modtime + device ID). Thin-arbiter (Gluster) — лёгкий witness. CRDTs — делают split решения "безвредными" через merge (emerging для local-first).
- Обнаружение partition: лучший сигнал — локальный Syncthing REST /rest/system/connections (connected bools, независим от файлов) + divergence ledger + асимметрии stamps. Дёшево и детерминировано.
- Safe-mode: freeze + escalate к человеку или tentative + challenge window (уже есть Phase A). Fencing обязателен.
- Reconciliation: детект по overlapping COMMITs (proposal ID + время); merge — higher epoch wins или human queue для конфликтов. Append-only облегчает replay.
- Telegram side-channel: полезен как near-realtime beacon для explicit claim + unchallenged Y минут. Не решает partition полностью, но даёт visibility и audit. Минимальный протокол: signed claim + challenge.
- X (практики): split-brain — частая тема в Raft/DB историях; подчёркивают тестирование failover и fencing. Конкретных Syncthing war stories мало — выводы в основном из web.

Рекомендация (минимальная, repair-friendly, AK-47-принцип):
2–3 механизма в single-file Python (~300–500 LOC добавления):

1. Монотонный epoch + fencing в arbiter-claim файле и событиях (первым — сразу даёт безопасность на rejoin).
2. Детекция partition (Syncthing connections + divergence) → safe-mode (tentative/challenge или freeze + TG human).
3. Опционально: TG explicit claim beacon с challenge-периодом.

Что НЕ строить: полный Raft/BFT (слишком сложно для 1500 LOC + sleeps; deps и хрупкость), сложный dynamic quorum (спящие узлы ломают majority), тяжёлые CRDT-библиотеки (простые эпохи + human queue достаточно).

Trade-offs: + безопасность и bounded divergence; − небольшая сложность (но всё инспектируемо файлами/логами/TG). Основное предположение: доверенный личный флот (crash/partition/sleep, нет Byzantine; человек — ultimate backstop). Уверенность: [established] по fencing/epoch, [emerging] по file-sync + TG адаптациям.

Готово к дизайну на следующей неделе. Конкретные опции выше — выбирайте B (рекомендую). Источники и детали — в полном отчёте.

(77 sources — список в UI Grok свёрнут; follow-up подсказки Grok: Implement Syncthing REST API partition detection · Explore CRDTs for append-only event ledgers · Add specific Python code for epoch fencing)
