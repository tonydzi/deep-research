---
dr_id: DR26-07-02-HUB-04
title: "Quorum Design for a Sleeping Personal Fleet"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-04): Quorum Design for a Sleeping Personal Fleet

> The research answers how a personal AI fleet with mostly-sleeping machines (N=4 with 1 always-on hub, or N=10 with 2 always-on hubs) should design tiered quorum/consensus rules so strict voting requirements don't become permanently unreachable.

## Ключевые выводы
- Sleepy-consensus theory (Pass and Shi's sleepy model; later fluctuating-participation work) proves classical fixed-participant quorum protocols break under sporadic participation — liveness must adapt to who is actually online now, not nominal fleet size; there is no accounting trick that rescues a strict quorum from absent participants.
- Blockchain systems converge on the same pattern via different mechanisms: Ethereum keeps a fixed 2/3 supermajority and recovers liveness slowly via the inactivity leak; Tendermint retries with growing timeouts + PoLC-Round evidence; Algorand samples committees from currently-online participation keys; Avalanche opens block production to anyone after bounded proposer windows; Solana's Tower BFT gives fast optimistic progress but has needed coordinated manual restarts (17-hour 2021 outage, ~5-hour 2024 outage).
- The common industry pattern is: scope quorum to actually-online participants, add a visible/explicit rescue path, and separate soft progress from hard finality — never silently redefine quorum at the deadline.
- For N=4 with only one always-on hub, a strict 2-vote rule is only reachable when at least one sleeping peer happens to be awake at proposal snapshot time; otherwise strict tier-1 quorum is mathematically unreachable, not just slower.
- For N=10 with two always-on hubs, tier-1 quorum becomes genuinely and permanently reachable via 'primary hub + secondary hub' as a live baseline certificate, even if all 8 sleepers are down — this is the key structural difference between the two fleet sizes.
- Committee membership must be frozen at proposal-snapshot time (like OpenZeppelin governance checkpointing quorum parameters), never recomputed at the deadline — otherwise late wake-ups/dropouts create a 'moving denominator' that changes the outcome rule mid-round.
- 'Leader counts as two votes' is a bad pattern — it hides a single-machine override inside a quorum label; better to keep leader = 1 vote plus a separately logged, visible 'degraded leader path' (mirrors Amazon's leader-election guidance on blast radius and Squads/Safe multisig signer-availability planning).
- Validator/KMS practice (Prysm slashing-protection, Cosmos KMS) shows that double-votes, replayed accepts, and restored-backup ghosts are common non-adversarial failure modes — every node needs a persisted vote high-water mark that must be imported before a restored/migrated node can vote again.
- Splitting acceptance from finality (accepted → committed → finalized, echoing Ethereum's justified/finalized checkpoints and optimistic-rollup challenge windows) lets the system keep liveness on the accept path while catching buggy-agent/false-verify cases later without slowing normal operation.
- High quorum requirements are documented to kill proposals through apathy/monitoring overload (2026 DAO research) even among honest actors — in a sleepy personal fleet this manifests as honest-but-absent machines, not malicious holdouts.

## Рекомендации / решения
- Add a signed 'presence'/heartbeat event to the JSONL bus; define an 'awake lease' (fresh presence within 15-30 min) so quorum is computed only against machines actually online now, never the whole nominal fleet.
- Freeze the eligible voting committee at proposal-snapshot time; late wake-ups join the next round or the challenge/verify window, never retroactively change the current round's quorum.
- N=4 tiered rules: Tier-0 leader-proposes/60min-no-counter→commit_soft+24h challenge window; Tier-1 strict needs leader+1 awake peer OR 2 awake peers (60min wait); if no peer awake at snapshot, strict tier-1 is declared unreachable — reversible actions may get a logged tentative_commit after 60min with 24h challenge, irreversible actions must escalate to human within 24h; Tier-2 always requires human_approve.
- N=10 tiered rules: primary+secondary hub (or either hub + any awake reviewer) as the live baseline for strict tier-1; degraded path only if the secondary hub itself is down; Tier-2 requires human_approve + both hubs.
- Never let a leader's vote count as multiple votes — implement any leader-only rescue as a separately logged 'degraded_commit_by_leader' state instead of inflating vote weight.
- Introduce explicit decision states (propose, counter, accept, commit_soft, tentative_commit, finalize, supersede, human_approve) in the JSONL log so a non-technical owner can read which path was used without understanding consensus theory.
- Persist a per-node vote watermark (highest signed term/round/step/proposal_hash); require any restored or migrated node to import this watermark before it's allowed to emit accept/commit/verify, preventing replay and stale-backup double-votes.
- Auto-escalate: if a workflow repeatedly hits tentative_commit/degraded paths beyond a small weekly threshold, automatically raise it to tier-2 until the owner reviews the root cause.
- If N=4 tier-1 liveness matters a lot, the cheapest decisive fix is adding a second always-on hardware witness (mini-PC/NAS), converting N=4 into a smaller version of the always-reachable N=10 design.
- Quarantine any machine with a regenerated/reset identity as a new observer until the old identity is explicitly retired or rebound by a durable registry — never let it silently inherit voting power.
- Use at least one verification path built on a different agent build than the proposer, to avoid correlated/monoculture bugs producing false quorum agreement.

## Сущности
- **Люди:** Pass and Shi, Efron-Neu-Pitassi
- **Компании:** Ethereum, Tendermint, Algorand, Avalanche, Solana, Safe, Squads, Cosmos, Prysm, OpenZeppelin, RabbitMQ, Amazon, EigenLayer, Google, Linux Foundation, OpenAI, Optimism
- **Продукты/инструменты:** Tower BFT, Casper FFG, RLMD-GHOST, Goldfish protocol, VRF sortition, A2A protocol, MCP, AVS (EigenLayer), inactivity leak, PoLC-Round

## Открытые вопросы
- How to concretely resolve the trade-off between dynamic-quorum liveness under fluctuating participation and resilience under temporary asynchrony (research shows these pull in opposite directions, no clean resolution given).
- Whether Anton will invest in a second always-on hardware witness for the N=4 fleet to make strict tier-1 reachable by construction, versus tolerating the 'unreachable when all sleepers are down' degraded/human-escalation path.
- What threshold of repeated degraded-path usage should trigger auto-promotion to tier-2 for a given workflow (report suggests 'small rolling weekly threshold' without a fixed number).
- How to concretely implement drills/tabletop tests (all sleepers offline at propose, restored backup replay, dual-instance same key, clock jump, identity regeneration) before rollout — not yet executed, only recommended.

## Источник
- DR-ID `DR26-07-02-HUB-04` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-02-HUB-04-quorum-design-for-a-sleeping-personal-fleet.md`

## Связано
- [[multi-machine-auto-consensus]]
- [[machine-bus-telegram-rail]]
- [[remote-approval-qqq]]
- [[machine-governance-leader-follower]]
- [[sleepy-consensus]]
- [[consensus-active-session-marking]]
- [[one-system-propagate]]
