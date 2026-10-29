---
dr_id: DR26-07-17-HUB-02
title: "Prior-art: защита 4-узлового консенсуса от split-brain (fencing/epoch, partition detection"
date: 2026-07-17
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-17-HUB-02): Prior-art: защита 4-узлового консенсуса от split-brain (fencing/epoch, partition detection, TG-witness)

> Deep research maps prior-art distributed-systems techniques (fencing tokens, epoch/term numbers, witness nodes, CRDTs) and recommends a minimal split-brain defense for Anton's 4-node personal Claude fleet running on Syncthing eventual-consistency file sync.

## Ключевые выводы
- Fencing tokens / epoch (term) numbers, as used in ZooKeeper, etcd and Raft, are the established prior-art pattern: tag every COMMIT/arbiter decision with a monotonically increasing epoch, and reject any commit whose epoch is lower than the highest seen — this alone blocks a stale ex-leader's writes.
- Syncthing's own REST API (/rest/system/connections) is rated the cleanest, highest-confidence local partition-detection signal — a sudden drop from 'connected' to 'disconnected' on a normally-live peer (especially losing ≥2 of 4 nodes) is a strong partition indicator; sleeping nodes (e.g. MacBook) must be distinguished as expected-offline, not partition.
- Recommended safe-mode is 'freeze': on suspected partition, halt all Tier-0/1 automatic commits and only allow Tier-2 (human-approved) proposals, mirroring how ZooKeeper/etcd/Raft minority partitions simply go read-only rather than attempt risky auto-merge.
- Reconciliation after a healed partition should use an 'epoch-wins' rule: for any proposal ID with duplicate COMMIT events across branches, keep the commit from the higher epoch and discard/flag the lower-epoch one for human review; full automatic lossless merging (e.g. via deterministic total ordering or CRDTs) is judged too complex/unreliable at this scale.
- CRDT/invariant-based approaches were evaluated and rejected: they only guarantee convergence for specially designed commutative operations (grow-only sets, counters) and cannot resolve two arbiters taking genuinely conflicting actions without tombstones/vector clocks — high implementation cost for limited benefit.
- Full Raft/Paxos/BFT protocols are explicitly NOT recommended — they require multi-phase RPC and would add likely thousands of lines of code and new failure modes, which is overkill for a single-owner, 4-node personal fleet.
- A Telegram side-channel (the existing shared group) can serve as an auxiliary witness/tie-breaker (nodes post signed 'I am arbiter, epoch=E' claims and wait ~30s for contest before proceeding), but this is rated speculative/emerging confidence — Telegram itself can partition asymmetrically from Syncthing, giving false confidence, so it must stay secondary to fencing+freeze, never the primary guarantee.
- Witness/passive-arbiter nodes (e.g. requiring a confirmation stamp from the always-on Mayak VPS before auto-commit) are a low-cost strengthening option, but stall progress if the witness itself is unreachable.
- Presence-stamp asymmetry (comparing `.robot-alive-<M>.log` freshness between nodes) is a moderate/'Emerging' confidence detection signal — useful as a red flag ('I think B is dead but B thinks A is alive') but can lag behind real-time partition state; 'ledger-progress divergence' (no new events flowing) is rated 'Speculative' with no clear automatable metric.
- Industry precedent (Elasticsearch, quorum systems) shows that essentially no production system attempts full automatic split-brain merge — they discard one side's diverged data and require operator intervention, reinforcing that human-in-the-loop conflict resolution is the safer default at small scale.

## Рекомендации / решения
- Implement an epoch counter in consensus.py (~100-200 lines): increment on each arbiter (re-)election, tag every PROPOSE/ACCEPT/COMMIT event with epoch+arbiter-ID, and reject any commit from a lower epoch than the highest seen.
- Poll the local Syncthing REST endpoint (/rest/system/connections) periodically; treat sudden loss of connection to ≥2 of 4 nodes as a suspected partition and switch to safe mode.
- In safe mode, freeze all Tier-0/1 auto-commits, allow only Tier-2 human-approved proposals, and mark the log 'STALLED_DUE_TO_PARTITION' with an alert to the owner.
- Build a small post-merge reconciliation routine that scans COMMIT events for duplicate proposal IDs, applies the epoch-wins rule (higher epoch wins, tie-break by node ID if needed), and logs/flags dropped lower-epoch commits for human review.
- Use the existing Telegram group only as a secondary cross-check/witness (announce arbiter intent + short challenge window) — do not rely on it as the sole or primary split-brain defense.
- Do not build full Raft/Paxos/BFT consensus or a CRDT-based merge model — not worth the complexity/risk tradeoff for a 4-node, single-owner personal fleet; keep the existing single-writer-shard + manual Tier-2 architecture and add only fencing+freeze as minimal fail-safes.

## Сущности
- **Люди:** Anton
- **Компании:** —
- **Продукты/инструменты:** ZooKeeper, etcd, Raft, Paxos, Syncthing, Telegram, Elasticsearch, consensus.py, Mayak (VPS witness node), CRDT

## Открытые вопросы
- No tie-break rule specified for the case where two isolated nodes independently increment the epoch to the same value (proposed fix: tie-break by higher node ID, but not finalized).
- Unclear how to prevent Telegram itself from giving false confidence when it partitions asymmetrically from the local network (one side reachable, other not).
- 'Ledger-progress divergence' as a detection signal has no concrete automatable metric — rated purely speculative, not resolved into an implementable check.
- No defined process for what happens to non-conflicting but divergent data during reconciliation beyond the binary epoch-wins rule — full lossless merge deemed infeasible but not replaced with a fallback salvage procedure.
- Cost/complexity of enforcing the epoch check across all peers' log-parsing logic is estimated (~100-200 lines) but not yet implemented or tested.

## Источник
- DR-ID `DR26-07-17-HUB-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- multi-machine-auto-consensus
- machine-governance-leader-follower
- remote-approval-qqq
- vps-anchor-node
- own-fleet-peer-equality
- split-brain
- fencing-tokens
- syncthing-partition-detection
