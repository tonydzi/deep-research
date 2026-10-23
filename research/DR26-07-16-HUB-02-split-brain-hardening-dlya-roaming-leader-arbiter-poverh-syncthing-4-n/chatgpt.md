# DR26-07-16-HUB-02 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt.com (Deep Research, Pro план, аккаунт Anton Dz)
topic: "DR26-07-16-HUB-02 — Split-brain hardening for roaming-leader arbiter over Syncthing file-sync consensus"
date: 2026-07-16
collected: 2026-07-16 ~22:49 Lisbon, hub хаб, сессия f95d5376 (backend JSON widget_state -> blob)

url: https://приватный чат
notes: verbatim report_message из widget_state (40.5K). На аккаунте в момент сбора висел баннер "Suspicious activity detected" (доложено Антону).
---

# DR26-07-16-HUB-02 — ChatGPT Deep Research verbatim

# Split-brain hardening for a roaming-leader arbiter over Syncthing file-sync consensus

## Scope and key findings

This is a design review for a four-node personal fleet running over an **eventually consistent file-sync substrate**, not a conventional quorum RPC fabric. The most important baseline is theoretical and old, not recent: in a **fully asynchronous system with crash-like failures**, deterministic consensus cannot be guaranteed to terminate even with one faulty process, and practical consensus requires some form of failure detector or synchrony assumption. Chandra and Toueg’s result is the next layer: with a majority of correct processes, consensus becomes solvable only with an eventually accurate failure detector. In your setting, **presence stamps replicated over the same partitionable Syncthing channel are not an independent failure detector**; they are just more eventually replicated state. So they are good for hints, not for proving a unique acting arbiter. That is the core reason your current S9 roaming-arbiter rule can split-brain. **Confidence: [established].** (Foundational sources are older than the requested 12–18 months; I am stating that explicitly because there is no newer theorem that supersedes them.) citeturn18view2turn18view0turn18view1

The engineering consequence is blunt: the winning design is not “cleverer stamp logic.” It is a layered safety design that does four things: **detect suspected partition using local oracles that do not replicate through Syncthing; fail closed for risky auto-actions; attach fencing epochs to every arbiter decision; and make rejoin repair cheap and explicit.** This matches mainstream HA practice more than availability-first eventually consistent systems: Microsoft quorum guidance says nodes should go offline if they cannot verify majority, specifically to prevent split-brain; Consul’s session docs state the same safety/liveness trade-off explicitly, even allowing operator intervention to avoid split-brain altogether. **Confidence: [established].** citeturn16view0turn16view2turn32view3

A second key finding is that **term or epoch numbers are still worth adding**, but you should treat them as **fencing clocks**, not as proof that election was safe. In Raft, term numbers work because elections are backed by majority voting and heartbeat RPCs; Raft’s “at most one leader per term” property depends on that majority-vote rule, not on the existence of a term field by itself. Adapted to append-only per-machine logs without fresh quorum communication, epochs help you reject stale actions later, but they do **not** prevent two isolated nodes from acting during the partition. **Confidence: [established].** citeturn17view1turn17view2turn31search1turn32view4

## Prior art mapping

The closest prior art does not come from a single product. It comes from five families of mechanisms that map differently to your file-shard world.

| Mechanism | What the technique is | How it maps to your logs | Main value | Main failure mode | Confidence |
|---|---|---|---|---|---|
| Lease + fencing token | Lock/lease with a monotonically increasing generation token that downstream consumers reject if stale. Chubby calls the token a *sequencer*. citeturn14view0turn14view1 | Add `arbiter_epoch` to every tie-break / auto-commit event. Peers permanently refuse lower-epoch arbiter actions once a higher epoch is seen. | Stops stale arbiters from continuing after rejoin. | Does **not** stop dual action during a live partition if epochs are self-issued. [single-source for Chubby sequencer specifics] | [established] |
| Raft-style terms in append-only logs | Monotonic term numbers act as logical clocks; safety comes from majority vote per term. citeturn17view1turn17view2 | Store `TERM_BUMP` / `ARB_CLAIM` events in each node’s own shard; require every decision to carry `(epoch, claimant, claim_hash)`. | Excellent auditability and stale-write fencing. | Without majority vote / heartbeats, terms are not unique-leader proofs. | [established] |
| Witness / qdevice / quorum vote | Add a third-party vote so even-node splits do not produce two “primaries.” Modern HA stacks still recommend this. citeturn16view0turn16view2turn22view1turn22view2 | Mayak can act as an independent tie-breaker **only if it is reached over an independent path**, not via a Syncthing-replicated file. | Real safety improvement in even-node clusters. | If witness shares the same failure domain or storage substrate, it is fake safety. | [established] |
| Weakly connected tentative commit | Bayou accepts updates quickly, marks them tentative, and later commits them in a primary commit order propagated by anti-entropy. citeturn24view0turn24view1turn24view3 | Your existing tentative-commit challenge window is already in this family. | Preserves offline availability. | Increases semantic complexity: rollback, replay, and merge burden. | [established] |
| Invariant-based harmlessness | CRDT / SEC / I-confluence approaches avoid coordination only when local commits remain globally valid after merge. citeturn26view1turn26view3 | Restrict auto-arbitration to actions whose effects are commutative, idempotent, or explicitly compensatable. | Makes split-brain less harmful instead of “impossible.” | Misclassify one non-commutative action as safe and you reintroduce semantic corruption. | [established] |

The **lease + fencing** pattern is the most directly reusable element. In Google Chubby, the lock-holder can ask for a *sequencer* that contains the lock generation; downstream servers are expected to reject requests carrying stale sequencers. Chubby also had a *lock-delay* to protect systems that could not validate sequencers directly. That is the canonical shape of what you need: not just “who thinks it is leader,” but a **monotonic proof attached to every protected action**. In your system, that means each auto-arbitrated Tier-0/1 decision should carry an `arbiter_epoch`, and any node that later observes a higher epoch should reject all older epoch arbiter actions going forward. The catch is decisive: **if the epoch is minted only inside the partitioned file-sync world, fencing limits stale continuation but cannot by itself prevent two live arbiters during the partition.** **Confidence: [established].** citeturn14view0turn14view1turn17view1turn17view2

The **Raft term adaptation** is still useful, but narrowly. Raft treats terms as logical clocks; servers update their current term monotonically, and election safety comes from “vote at most once per term” plus majority voting. In your file-shard world, you can keep the useful half — monotonic audit clocks and stale-term rejection — but you do **not** get the election-safety theorem because you do not have majority vote RPCs or fresh quorum reads. The right mental model is: *Raft terms without Raft quorum are fence labels, not consensus.* **Confidence: [established].** citeturn17view1turn17view2turn32view4

The **witness / qdevice** family is the production answer for even-numbered HA clusters. Microsoft’s current Windows Server guidance says a witness is required for two nodes and strongly recommended for three or four; if a surviving partition cannot verify majority, it should stop running. Red Hat and corosync-qdevice make the same point in different implementation language: an external arbitrator casts the deciding vote, and in a 2-way split the vote goes to one side only. The decisive mapping for you is negative as much as positive: Microsoft explicitly warns that **replicated storage technologies are not supported for file share witnesses because they can create partition-in-space or partition-in-time and cause data loss**. That is almost a direct analogy for “do not keep the witness inside Syncthing.” **Confidence: [established].** (The DFS / replicated-storage warning is from one official Microsoft source only, so the exact wording is **[single-source]**.) citeturn16view0turn16view1turn16view2turn22view1turn22view2

The argument for **Bayou-like tentative commit** is the best steelman for the availability-first side. Bayou was built specifically for **weak connectivity** and allowed writes to be accepted tentatively, reordered later, and eventually committed by a designated primary through anti-entropy. Bayou is especially relevant because it explicitly notes that the designated primary could be on a disconnected laptop. If your top priority were “keep making progress while traveling and offline,” Bayou is the correct historical family. But the price is severe: tentative order, undo/redo, semantic merge procedures, conflict surfacing, and the possibility that humans need to reconcile the difficult cases. For a one-owner personal fleet with a cheap escalation rail, this is not the simplest baseline. **Confidence: [established].** citeturn24view0turn24view1turn24view3

The **CRDT / invariant-confluence** family is the right lens for deciding which proposal classes can safely remain autonomous during partitions. CRDT work shows that conflict-free objects can converge without coordination if operations are monotonic or commutative; the I-confluence result is even more blunt: coordination can only be avoided if every local commit decision remains globally valid after merge. Applied to you, this means there is no single answer for “can Tier-0 run during partitions?” The answer is per-action-class. If an action is truly idempotent, commutative, or cheaply compensatable, you can keep some autonomy. If it allocates exclusive ownership, consumes a one-shot resource, or causes hard-to-reverse side effects, it is not partition-safe and should fail closed. **Confidence: [established].** citeturn26view1turn26view3

## Detection

For a four-node fleet with two habitual sleepers, the best detectors are the ones that rely on **local, non-replicated observations** and that privilege the **always-on anchor set** (`hub`, `Mayak`) over sleeping devices. Treat `HP17` and `Mac16` as non-authoritative for availability inference when they are expected to sleep. That priority rule is a context-specific engineering inference, not a literature claim **[no source found]**.

The highest signal detector is **Syncthing local connection state**. `GET /rest/system/connections` exposes whether each configured device is currently connected, along with metadata such as connection type and timestamps; `GET /rest/stats/device` exposes `lastSeen` and last connection duration; the event API emits `DeviceConnected` and `DeviceDisconnected`. All of that is queried from the local Syncthing daemon, so it does **not** depend on presence stamps making it across the partition. For your fleet, that makes it the best “I may be partitioned” oracle. In practice: if `hub` or `Mayak` is configured as always-on and your local Syncthing daemon says both are disconnected or long-unseen while your own shard is still advancing, assume partition and stop auto-arbitration. **Confidence: [established].** (The exact endpoint field semantics are **[single-source]** per endpoint doc, though they are all official Syncthing docs.) citeturn10view0turn29view1turn30search1turn30search3

The second-best detector is a **Telegram liveness cross-check**, but only as an **independent-path corroborator**, not as the sole oracle. If the machine can still read/write a Telegram group in near-real time while Syncthing local connections to always-on anchors have disappeared, that is strong evidence of a **Syncthing-side partition**, not a total network outage. Conversely, if both Syncthing and Telegram are dark, the system should assume “coordination unknown” and fail closed. This detector is strong because witness systems explicitly recommend reaching the arbitrator over a **different physical network / failure domain** than the main cluster path. **Confidence: [established] for the independent-path principle; [speculative] for Telegram as the chosen path.** citeturn22view0turn23view0turn23view2

The third detector is **folder progress asymmetry** using Syncthing folder APIs: `GET /rest/db/completion` returns completion state and can report `remoteState="unknown"` when a remote device is not connected; `GET /rest/db/status` returns folder `need*`, `sequence`, and current folder `state`. This is useful when your ledger shard is moving but shared-state folders are not converging. It is less clean than connection-state because folder lag also happens during normal sleep, rescans, and temporary backlog. That makes it a good corroborator, not your primary kill switch. **Confidence: [established].** (Endpoint specifics are **[single-source]** official docs.) citeturn10view1turn29view0

After that comes **ledger-progress divergence**: “my shard advances, but I see no fresh events from hub/Mayak for longer than expected.” This is useful, but it is downstream of the eventual-sync channel you are already worried about, so by itself it has lower signal. It is basically a smell, not an oracle. Still, when combined with local Syncthing disconnection and stale Telegram heartbeats, it becomes a strong composite detector. **Confidence: [emerging].** (This is an engineering inference grounded in the Syncthing API and your topology rather than a named published algorithm.) citeturn10view0turn29view1turn22view0

The worst detector, by far, is **presence-stamp age alone**. Because those stamps move over the same eventually consistent file-sync substrate as the decision ledger, they fail exactly when you most need them. Microsoft’s “do not place a witness on replicated storage” warning is the closest authoritative analogy here: if the witness metadata rides the same delayed or partitioned replication path as the state you are protecting, it cannot decisively break the split. **Confidence: [established].** (The analogy to your stamps is an inference; the witness warning itself is **[single-source]**.) citeturn16view1

If you want a blunt ranking for implementation:

1. **Local Syncthing connection state to always-on anchors** — best signal/noise. citeturn10view0turn29view1  
2. **Telegram signed heartbeat corroboration** — high value if internet/TG usually survives Syncthing-specific faults. citeturn22view0turn23view0  
3. **Syncthing folder lag / remoteState / needBytes** — useful corroborator, moderate noise. citeturn10view1turn29view0  
4. **Ledger-progress divergence** — helpful smell, not decisive. citeturn29view0turn10view0  
5. **Replicated presence stamps alone** — insufficient for safety. citeturn16view1turn18view0

## Safe-mode semantics

The most conservative option is **freeze auto-arbitration entirely and escalate to Anton** whenever partition is suspected. This is exactly how mainstream HA systems handle loss of quorum: if a partition cannot prove it is the surviving majority, it stops running critical clustered behavior to avoid split-brain. Consul’s session docs make the same safety/liveness trade-off explicit from the operator side: you can bias toward safety and require intervention, precluding split-brain at the cost of progress. Given that your owner is one person, the escalation rail is cheap, and Tier-2 already requires human approval, this option has unusually good fit. **Confidence: [established].** citeturn16view0turn16view2turn32view3

The next option is **allow only idempotent, reversible, or commutative proposal classes**. This is the invariant-based lane. Event-sourcing guidance is directly relevant here: immutable append-only events plus compensating events give you a principled way to reverse decisions after the fact, and CRDT / I-confluence work tells you that coordination is only avoidable when local commits remain globally valid after merge. In plain terms: if an operation can be safely repeated, merged, or canceled with a compensating event, you can keep it in the autonomous lane under suspicion. If not, you freeze it. This is where a real classification table matters more than a “Tier-0 vs Tier-1” label. **Confidence: [established].** citeturn27view0turn26view1turn26view3

The most availability-friendly option is **tentative commit with a challenge window**. This has real prior art: Bayou accepted operations tentatively, let isolated groups keep working, and later propagated the eventual committed order. The steelman case for your fleet is obvious: `HP17` traveling offline is not a pathological event; it is normal. If you require strict online proof for every low-risk action, you will page the human too often. The downside is equally obvious and usually underappreciated: once you allow tentative outcomes, you are committing to visible “maybe” state, later supersession, replay discipline, and a repair UI or repair protocol. Bayou paid that cost because disconnected operation was its mission. Your system may not need to. **Confidence: [established].** citeturn24view0turn24view1turn24view3

**Fencing with epochs** is not an alternative to the above options. It is a baseline you should add regardless. Every arbiter action should carry an epoch, and peers should refuse lower-epoch arbiter actions once a higher epoch has been observed. This is the software analogue of a fencing token. Industry practice is clear that stale leaders are dangerous precisely because they can continue acting after losing their lock; Chubby’s sequencer is the canonical answer. In your design, fencing also gives you a clean rejoin rule: once a node has seen epoch `E+1`, it never accepts epoch `E` arbiter actions again. What fencing **does not** do is stop two live claimants from acting simultaneously before either one sees the other’s higher epoch. **Confidence: [established].** citeturn14view0turn17view1turn17view2

At **N=4 with two sleepers**, the best complexity/benefit combination is:

- **Default:** freeze Tier-0/1 auto-arbitration on partition suspicion and escalate.  
- **Always:** attach epoch fencing to every arbiter decision.  
- **Optional small exception:** allow only a narrow, audited whitelist of reversible actions to stay autonomous under suspicion.  

That combination mirrors the operator wisdom in quorum systems: bias the uncertain state toward safety; add fencing so stale actors cannot continue later; keep availability only where merge semantics are demonstrably safe. By contrast, a full Bayou-style tentative lane for everything is probably too much machinery for a 4-node personal fleet unless you discover in practice that freezes become intolerable. **Confidence: [established] for the general policy; [emerging] for the precise fit to your fleet.** citeturn16view2turn32view3turn24view0turn26view3

## Reconciliation on rejoin

When two arbiters have acted during a partition, the least painful discipline for your append-only sharded ledger is **not** “pick a winner by timestamp and move on.” Deterministic total ordering by `timestamp + machine-id` is useful for **stable replay and presentation**, but it is a poor semantic repair rule because concurrent decisions are exactly what you need to **detect**, not erase. Bayou did use a deterministic tentative order, but it also kept the semantics of tentative versus committed and supported undo/redo. Dynamo is the cleaner caution here: when versions are causally unrelated, it returns multiple branches and requires reconciliation instead of pretending the clock totally ordered them. **Confidence: [established].** citeturn24view1turn25view0turn25view2

The best merge discipline for your scale is therefore: **keep all events, auto-merge the non-conflicting history, and send only conflicting terminal decisions to a human review queue.** That is also the most repair-friendly. It matches Syncthing’s own conflict philosophy — preserve both sides rather than silently overwrite — and event-sourcing’s compensating-event model — never rewrite history, only append “supersede,” “cancel,” or “compensate.” This is exactly the kind of design that one non-technical owner can recover from: “I see both decisions, and the engine has not hidden either one.” **Confidence: [established].** citeturn28view0turn27view0

The hard part is detecting the conflict set **precisely**. The right tool is a **version-vector style causal snapshot** embedded in each event. Dynamo uses vector clocks to detect whether two versions are causally ordered or concurrent; Bayou uses timestamp vectors to identify precisely which writes need exchange during anti-entropy. Your ledger already has the necessary structure: each machine owns a single-writer shard with a monotonic per-shard sequence. That means every event can carry a four-entry causal snapshot such as:

```json
"seen_seq": {
  "HP17": 1204,
  "hub": 8821,
  "Mayak": 4173,
  "Mac16": 209
}
```

At emit time, `seen_seq[M]` is the highest sequence from each shard the emitter had observed. Then two terminal decision events `a` and `b` are:

- **causally ordered** if `a.seen_seq <= b.seen_seq` component-wise or vice versa;
- **concurrent** if neither dominates the other.  

That lets you detect “these two decisions were made without one seeing the other” instead of guessing from wall clocks. **Confidence: [established] for vector-clock causality; [emerging] for this exact adaptation.** citeturn25view0turn25view2turn24view3

With that, the precise conflict predicate can be:

```text
conflict(a, b) :=
  terminal(a) and terminal(b)
  and concurrent(a, b)
  and overlap(scope(a), scope(b))
  and (
       outcome(a) != outcome(b)
       or exclusive_effects(a, b)
      )
```

Where `scope()` is at least `proposal_id`, and ideally also a **resource / mutex key** describing the real-world exclusivity domain affected by the proposal. Two COMMITs for the **same proposal** obviously conflict if concurrent and different. Two COMMITs for **different proposals** should enter the same human queue only if their scopes overlap — for example, if both allocate the same exclusive resource or cause mutually exclusive side effects. The `exclusive_effects()` classifier is domain-specific **[no source found]**, but the need for such a classifier follows directly from invariant-based coordination arguments. citeturn26view3turn27view0

That yields a clean comparison of merge disciplines:

- **Deterministic total order (`ts + machine-id`)**: simplest, stable, good for replay; bad as a semantic winner rule because it can silently discard a legitimate concurrent decision. **Not recommended as the sole repair rule.** citeturn24view1turn25view2  
- **Epoch-wins**: acceptable only as a *post-classification shortcut* when the lower-epoch action is known reversible and all consumers enforce fencing. Dangerous as a blanket merge rule because your epochs are not minted by a linearizable authority. **Use narrowly.** citeturn14view0turn17view1  
- **Human review queue of conflicting COMMITs only**: best fit for your fleet. It minimizes review surface while keeping all raw history. **Recommended.** citeturn28view0turn27view0turn25view2

## Telegram side channel

Using a shared Telegram group as a **secondary liveness / epoch beacon** is not fake. It can produce a **real safety improvement** — but only if you treat it correctly. It does **not** solve the partition problem in the theorem sense; it gives you a **second communication path** that may fail differently from Syncthing. That is exactly why quorum-device guidance recommends the arbitrator be on a separate network / power / rack from the main cluster path. If Syncthing partitions but internet access and Telegram remain healthy for `hub` and `Mayak`, Telegram can serve as an **independent witness path**. If the same underlying failure takes out both, it cannot. **Confidence: [established] for the principle; [speculative] for the exact Telegram instantiation.** citeturn22view0turn22view1

The minimal Telegram protocol that gives genuine value is **claim + challenge + wait**, not just heartbeats:

1. Every awake node posts a **signed heartbeat** every few minutes: node id, local time, highest seen epoch, and a compact local status summary.  
2. A candidate arbiter posts a **signed claim**: `CLAIM(epoch=E, claimant=hub, lease_until=T, priority_rank, basis_hash, syncthing_anchor_state)`.  
3. The claimant waits a **challenge window** before acting. If a conflicting signed claim with higher epoch appears, or if an always-on anchor contradicts the basis, it stands down.  
4. Auto-arbiter actions must reference the winning claim by `claim_hash` or Telegram message id plus the signed payload hash.  
5. On rejoin, any decision whose claim was challenged or superseded is immediately quarantined for review.

This protocol creates a **single visible contest point** on the independent channel. An unchallenged claim is not proof in the formal consensus sense, but it is much better than simultaneous silent self-election from stale file stamps. **Confidence: [emerging].** (The wait-window length itself is a context-specific estimate **[no source found]**.) citeturn22view0turn23view0turn14view0

For your fleet, the operationally sane policy is: **no Telegram, no autonomous tie-break**. In other words, Telegram should be a *required corroborator* for autonomous arbiter action on the always-on nodes. If Telegram is unavailable, stale, or contradictory, the engine falls back to OBSERVE/FREEZE and pings Anton. This does reduce availability, but it does so in the right direction: an unavailable witness must reduce automation, not silently increase it. That mirrors how quorum systems treat loss of arbitrator connectivity. **Confidence: [established].** citeturn16view2turn22view1

There are also two Telegram-specific caveats that matter here. First, the **Bot API stores incoming updates for no longer than 24 hours**; that is fine for live claims and challenge windows, but it is poor as a durable historical source for nodes that sleep for weeks. Second, bots in groups run in **privacy mode by default** and only see commands and explicitly relevant messages unless privacy is disabled or the bot is an admin. So if you use Telegram for arbitration, you either need a bot configured to reliably observe the claim messages, or you need a full Telegram client / TDLib-based reader rather than a minimal Bot API poller. These are not theoretical nitpicks; they change whether a sleeping node can later replay beacon history. **Confidence: [established].** (Both are official Telegram docs but each point is **[single-source]**.) citeturn23view0turn23view1turn23view2

My practical conclusion is:

- **Telegram heartbeats alone**: mild operational value, weak safety improvement.  
- **Telegram claim/challenge with signed epochs and mandatory wait**: meaningful safety improvement.  
- **Telegram as sole source of truth**: bad idea; it just relocates the SPOF and the partition.  

## Recommendation

Here are the concrete build options I think are real for this fleet.

**Option A — Local-oracle freeze.** Add a partition detector based on local Syncthing connection state to `hub` and `Mayak`; if an always-on anchor is not connected or recently seen, auto-arbitration freezes and the engine escalates to Anton. Add epoch fields now, but use them only for audit and stale-write rejection later. This is the smallest and safest change. Estimated implementation: roughly **150–250 LOC** for detector + safe-mode gate and **80–150 LOC** for epoch fields / rejection rules **[no source found]**. The downside is availability: `HP17` traveling will often lose autonomy when connectivity gets weird. **Confidence: [established].** citeturn10view0turn29view1turn16view2

**Option B — Fence-and-freeze with Telegram witness.** Keep Option A, but require a **signed Telegram claim to become acting arbiter**, with a short challenge window, and attach `arbiter_epoch` plus `claim_hash` to every tie-break / commit. Downstream peers permanently refuse lower epochs after seeing a higher one. On suspected partition, only a node that has both clean local Syncthing anchor status **and** an unchallenged Telegram claim may keep auto-arbitrating; otherwise freeze. Estimated implementation: about **350–650 LOC** total on top of today’s engine, depending on whether you already have Telegram integration primitives and signature plumbing **[no source found]**. This is the best balance of safety improvement, operator understandability, and not building a fake consensus system. **Confidence: [emerging].** citeturn14view0turn22view0turn23view0turn23view1

**Option C — Bayou lane for a reversible whitelist.** Build Option B, then allow a narrow class of explicitly reversible actions to enter a tentative-commit lane when partition is suspected. Conflicting results on rejoin are surfaced through a human review queue; repair happens by compensating events, not history rewrite. Estimated implementation: **550–900 LOC** beyond today’s code, plus ongoing semantic classification and debugging cost **[no source found]**. This buys more autonomy, but it is the first option where the semantic complexity curve turns ugly. **Confidence: [emerging].** citeturn24view0turn24view1turn27view0turn26view3

What I would **not** build at this scale:

- **Full Raft over Syncthing.** Raft assumes RequestVote / AppendEntries RPCs, majority elections, and timely heartbeats; emulating that over eventually synced files is rebuilding a broken transport for the wrong algorithm. **Do not do this.** citeturn34view1turn17view1turn17view2turn31search1  
- **Any witness stored inside Syncthing itself.** Official HA guidance explicitly warns against replicated-storage witnesses because they can partition in time/space. That is near-exactly your current failure mode. **Do not do this.** [single-source for the exact DFS warning] citeturn16view1  
- **Timestamp-wins auto-merge of conflicting COMMITs.** Fine for UI order, not for semantic repair. **Do not do this.** citeturn25view2turn24view1  
- **Real BFT / Byzantine quorum machinery.** Your threat model is not “three of four nodes are malicious”; the actual problem is partitions, sleepers, and stale visibility. BFT would be massive complexity for no proportional gain **[no source found]**.  
- **General CRDT-izing of the whole decision layer.** Use invariant logic narrowly to whitelist safe proposal classes; do not try to turn every decision into a CRDT. Most arbiter decisions are not naturally commutative. citeturn26view1turn26view3

My single recommendation is **Option B: Fence-and-freeze with Telegram witness**.

The concrete build order should be:

1. **Local partition oracle** from Syncthing daemon state to always-on anchors, with hard fail-closed behavior on suspicion.  
2. **Epoch-fenced arbiter actions** in the ledger, enforced on read/replay/apply, not just written for audit.  
3. **Signed Telegram claim/challenge protocol** required for autonomous tie-break action on `hub`/`Mayak`.  
4. **Human review queue for concurrent terminal conflicts**, using `seen_seq` vector snapshots to detect concurrency precisely.  

I would **not** enable Bayou-style tentative commits by default in the first hardening pass. Add that only if, after running Option B, you find that the freeze rate is operationally annoying **and** you can name a concrete reversible whitelist. **Recommendation confidence: [emerging].** The main assumption this rests on is that **Telegram/internet failures for `hub` and `Mayak` are sufficiently independent from Syncthing partitions** that the side channel acts as a real second rail. If that assumption is wrong, the design degrades safely by freezing more often, which is acceptable for this fleet. citeturn22view0turn23view0turn10view0turn14view0

### Резюме для решения на следующей неделе

Проблема не в том, что у вас «слабые presence-stamps». Проблема в том, что **stamps и ledger едут по одному и тому же eventually-consistent каналу**. Значит, при partition два узла могут честно и независимо прийти к выводу, что второй «протух», и оба начать acting-arbiter. Это не баг Syncthing; это нормальное следствие асинхронной системы без независимого failure detector или majority quorum. **Следовательно, текущая схема roaming-arbiter сама по себе split-brain не предотвращает.** **Уверенность: [established].** citeturn18view2turn18view0

Минимально правильная цель — не «доказать единственного лидера» внутри Syncthing, а **сильно уменьшить вероятность split-brain и резко упростить ремонт**, если он всё же случился. Для этого нужны три слоя:  
**(а)** локальный detector partition по состоянию собственного Syncthing daemon, а не по файлам-штампам;  
**(б)** fencing epoch на каждом arbiter-действии;  
**(в)** независимый side channel для claim/challenge перед автодействием. Это совпадает с практикой обычных HA-кластеров: при потере подтверждённого quorum критическая автоматика должна останавливаться, а не «смело продолжать». **Уверенность: [established].** citeturn10view0turn29view1turn16view2turn32view3

Что делать практически. Лучший detector для вашей топологии — **локальный Syncthing REST/API**: `system/connections`, `stats/device`, события connect/disconnect. Это даёт независимое от ledger знание о том, с кем Syncthing реально connected прямо сейчас. Для always-on якорей (`hub`, `Mayak`) этого достаточно, чтобы сказать: «если я не вижу ни одного якоря, я не имею права auto-arbitrate». Файловые presence-stamps в этом режиме остаются только вторичным сигналом. **Уверенность: [established].** citeturn10view0turn29view1turn30search1turn30search3

Дальше нужен **epoch fencing**. Каждый `TIEBREAK` / `AUTO-COMMIT` должен нести `arbiter_epoch`. Правило простое: если узел когда-либо увидел более высокий epoch, он **навсегда** отказывается принимать arbiter-действия с меньшим epoch. Это не убирает split-brain во время живого partition, но резко ограничивает ущерб после rejoin: «старый арбитр» уже не сможет продолжать писать как ни в чём не бывало. Это классическая идея fencing token из lock-service систем. **Уверенность: [established].** citeturn14view0turn17view1

Telegram можно использовать не как «ещё один heartbeat», а как **claim/challenge rail**. Минимальный протокол такой: кандидат публикует подписанный `CLAIM(epoch, claimant, lease_until, basis_hash)` в общий TG-групповой канал; ждёт короткое окно challenge; если приходит более высокий epoch или противоречащий claim от always-on peer, он отступает; только **неоспоренный claim** даёт право на авто tie-break. Это не превращает Telegram в настоящий consensus layer, но даёт **реальное повышение безопасности**, если Telegram и Syncthing ломаются по-разному. **Уверенность: [emerging].** citeturn22view0turn23view0

На rejoin не пытайтесь «победить по timestamp». Это плохой merge rule для решений с последствиями. Вместо этого добавьте в каждый event `seen_seq` — вектор «какие номера shard’ов я видел на момент записи». Тогда две terminal decision-записи считаются конкурентными, если ни один `seen_seq` не доминирует другой. Дальше в review queue попадают только те пары COMMIT/DECISION, которые **конкурентны** и затрагивают один и тот же `proposal_id` или один и тот же `scope/resource key`. Это даст точный набор конфликтов без переписывания истории. **Уверенность: [emerging].** citeturn25view0turn25view2turn24view3

Итоговая рекомендация: **строить не Raft и не “умнее stamps”, а схему Fence-and-Freeze with Telegram witness**. Порядок внедрения:  
**сначала** local partition oracle;  
**потом** enforced epoch fencing;  
**потом** Telegram claim/challenge;  
**и отдельно** human review queue для конфликтных terminal events.  
Bayou-style tentative commits пока **не включать по умолчанию**: сначала добейтесь безопасного fail-closed поведения, потом при необходимости добавите маленький whitelist обратимых действий. **Итоговая рекомендация: [emerging].** Главная предпосылка — что у `hub` и `Mayak` Telegram обычно остаётся доступен, когда именно Syncthing ведёт себя как partitioned transport. Если это не так, система будет чаще freeze’иться, но это безопасная деградация. citeturn10view0turn14view0turn22view0turn16view2

## Source list

- Michael J. Fischer, Nancy A. Lynch, Michael S. Paterson, **Impossibility of Distributed Consensus with One Faulty Process**, JACM, 1985. citeturn7search2  
- Tushar D. Chandra, Sam Toueg, **Unreliable Failure Detectors for Reliable Distributed Systems**, JACM, 1996. citeturn7search0  
- Diego Ongaro, John Ousterhout, **In Search of an Understandable Consensus Algorithm**, USENIX ATC, 2014. citeturn0search3  
- Mike Burrows, **The Chubby Lock Service for Loosely-Coupled Distributed Systems**, OSDI, 2006. citeturn2search3  
- Apache ZooKeeper, **ZooKeeper Recipes and Solutions**, current documentation, crawled 2026-07-16. citeturn2search0  
- Microsoft Learn, **Understand cluster and pool quorum on Azure Local and Windows Server clusters**, published 2025. citeturn3search0  
- Microsoft Learn, **Configure a file share witness for Failover Clustering in Windows Server**, published 2026-04-21. citeturn3search1  
- Microsoft Learn, **What is a failover cluster quorum witness in Windows Server?**, published 2025. citeturn3search3  
- Red Hat Documentation, **Configuring fencing in a Red Hat High Availability cluster**, current documentation, crawled 2026-07-16. citeturn4search0  
- SUSE Documentation, **Fencing and STONITH**, published 2026-07-16. citeturn4search2  
- SUSE Documentation, **QDevice and QNetd**, published 2026-07-16. citeturn21search0  
- Red Hat Documentation, **Configuring quorum devices**, current documentation, crawled 2026-07-16. citeturn21search1  
- Debian Manpages, **corosync-qdevice(8)**, published 2026-02-24. citeturn21search11  
- Syncthing Documentation, **REST API**, current documentation, crawled 2026-07-16. citeturn1search4  
- Syncthing Documentation, **GET /rest/system/connections**, current documentation, crawled 2026-07-16. citeturn1search1  
- Syncthing Documentation, **GET /rest/stats/device**, current documentation, crawled 2026-07-16. citeturn11search0  
- Syncthing Documentation, **GET /rest/db/completion**, current documentation, crawled 2026-07-16. citeturn0search0  
- Syncthing Documentation, **GET /rest/db/status**, current documentation, crawled 2026-07-16. citeturn12search0  
- Syncthing Documentation, **Event API**, current documentation, crawled 2026-07-16. citeturn30search0  
- Syncthing Documentation, **DeviceConnected**, current documentation, crawled 2026-07-16. citeturn30search3  
- Syncthing Documentation, **DeviceDisconnected**, current documentation, crawled 2026-07-16. citeturn30search1  
- Syncthing Documentation, **Understanding Synchronization**, current documentation, crawled 2026-07-16. citeturn19search1  
- etcd Documentation, **What is etcd?**, current site, crawled 2026-07-16. citeturn31search2  
- etcd Documentation, **How to conduct leader election in etcd cluster**, last modified 2025-12-18. citeturn31search0  
- etcd Documentation, **etcd3 API / Lease API**, published 2024. citeturn13search0  
- Kubernetes Documentation, **Leases**, published 2026-03-31. citeturn13search3  
- Kubernetes Documentation, **Nodes**, published 2026-05-17. citeturn13search6  
- HashiCorp Developer, **Application leader election**, published 2026. citeturn13search2  
- HashiCorp Developer, **Sessions and distributed locks overview**, published 2026. citeturn13search5  
- Terry et al., **Managing Update Conflicts in Bayou, a Weakly Connected Replicated Storage System**, SOSP-era paper, 1995. citeturn9search0  
- DeCandia et al., **Dynamo: Amazon’s Highly Available Key-value Store**, SOSP, 2007. citeturn8search3  
- Shapiro, Preguiça, Baquero, Zawirski, **Conflict-free Replicated Data Types**, 2011. citeturn5search3  
- Peter Bailis et al., **Coordination Avoidance in Database Systems**, VLDB, 2014. citeturn5search1  
- Microsoft Learn, **Event Sourcing pattern**, published 2026. citeturn8search4  
- Telegram, **Telegram Bot API**, current documentation, crawled 2026-07-16. citeturn6search0  
- Telegram, **Telegram Bot Features**, current documentation, crawled 2026-07-16. citeturn6search13  
- Telegram, **Telegram APIs / TDLib overview**, current documentation, crawled 2026-07-16. citeturn6search19