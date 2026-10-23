# DR26-07-14-ZB-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!--
source: ChatGPT Deep Research (chatgpt.com, Anton Pro account)
dr_id: DR26-07-14-ZB-01
topic: Agent-fleet message bus - Telegram+Syncthing vs dedicated transport (NATS/JetStream)
collected: 2026-07-14 ~10:00 Lisbon, machine ноутбук, session 72cb4807

extraction: verbatim from conversation widget_state.report_message (sandbox-iframe bypass)
chat_url: https://приватный чат
-->

# DR26-07-14-ZB-01 Agent Fleet Message Bus Migration

## Executive Summary

For a **sleepy, heterogeneous, sub-15-node fleet** run by a **non-technical solo operator**, Telegram plus Syncthing stops being “enough” **before** raw scale becomes the problem. The hard trigger is not throughput alone; it is the moment you need all four of these at once: **lossless catch-up after hours or days offline, explicit acknowledgment, transport-level replay independent of file-sync lag, and repairable operations on Windows/macOS/Linux without specialized admin knowledge**. Your current fleet has already crossed that line because the observed failure modes are exactly the ones dedicated transports were built to remove: offline lag from file sync, session invalidation risk on Telegram user sessions, and transport semantics being reimplemented in custom code. citeturn32search4turn32search1turn39search0turn38search1

On Telegram itself, the **Bot API** has published limits that are tight enough to make it a poor primary machine bus for chatty autonomous systems: Telegram’s own Bot FAQ says to avoid sending more than **1 message/second in a single chat**, states that bots in a **group** cannot send more than **20 messages/minute**, and says bulk notifications are limited to about **30 messages/second** unless paid broadcast is enabled; the Bot API documentation adds that paid broadcast can raise the broadcast limit to **1000 messages/second** for a fee. Those numbers are good enough for a human-visible mirror, but they are not the semantics you want your machine bus to depend on. citeturn36search0turn36search2

For the dedicated transport, the best fit is **NATS + JetStream on the VPS**, initially as a **single server with file-backed JetStream**, not a cluster. That recommendation rests on the combination of: single static binary deployment, official Windows service support, package-manager installation on macOS and Linux, built-in durable streams, durable consumers, key/value and object-store primitives, small footprint, straightforward monitoring, and a security model based on **Ed25519-derived NKeys**. For this specific fleet, **Mosquitto/MQTT** is the strongest alternative if you want maximal protocol conservatism and offline queueing, but it gives you less help with coordination primitives and more application work around auditability and rollout workflows. **Redis Streams** is credible and improving quickly, but Windows support and durability/ops trade-offs make it a worse fit for a one-person cross-platform fleet. RabbitMQ Streams remains technically credible, but its Erlang dependency and heavier operational surface are a poor match to the stated simplicity constraint. citeturn10search3turn13search0turn10search6turn8search3turn32search4turn9view0turn9view1turn31view2turn14view1turn27search10turn34view0turn34view1turn33search1turn33search3

The right migration path is **dark launch first, then shadow-read validation, then selective promotion**, while **Telegram remains the permanent human-readable mirror**. The industry pattern is well established: keep the old path authoritative, dual-write or traffic-shadow to the new path, compare correctness and latency under production load, and only then flip authority. For your case, that means NATS/JetStream should first run as a **silent second rail** behind Syncthing, then become the authoritative transport only after measurable gates are met. citeturn20view0turn20view1turn20view2turn20view3

My bottom-line recommendation is: **proceed with the approved Phase 2 dark pilot of NATS + JetStream now**, but keep the service model deliberately boring: one VPS, one file-backed JetStream store, one Telegram mirror bot, static NKeys and subject ACLs first, no JWT chain-of-trust ceremony yet, no leaf nodes yet, and no attempt to replace semantic approval/signing with transport auth. Recommendation confidence: **[established]**. Main assumption: the fleet’s central availability anchor remains the always-on VPS over Tailscale, and the laptops continue to be mostly sleeping endpoints rather than always-on edge islands. citeturn32search15turn31view1turn10search12turn35view0turn15search15

## Key Findings

The table below isolates the decision-critical findings.

| Finding | Confidence | Why it matters |
|---|---|---|
| **Telegram Bot API is suitable as a mirror, not as a load-bearing transport.** Telegram documents **1 msg/s per chat**, **20 msgs/min per group**, and about **30 msgs/s** broadcast without paid broadcast; paid broadcast can raise this to **1000 msgs/s** for a fee. | **[established]** | These are operational ceilings, not delivery semantics. Good for a readable mirror, bad as the core bus for autonomous coordination. citeturn36search0turn36search2 |
| **Telegram MTProto user-session limits are intentionally opaque; exact numeric send thresholds are not published.** Telegram documents `FLOOD_WAIT_X` and session invalidation on duplicate main sessions; Telethon’s current docs explicitly say nobody knows the exact limits for all requests and they depend on many factors. | **[established]** | If you need predictable transport behavior, “nobody knows exact limits” is already a disqualifier for core bus duty. citeturn39search0turn38search1turn37search1 |
| **A single authorized Telegram main session should not be reused in parallel beyond the server-advertised `tmp_sessions`; when absent or ≤1, a single main session must be used, and violating this can trigger `AUTH_KEY_DUPLICATED` and invalidate the authorization key.** | **[established]** | This directly matches your fleet’s observed failure with shared user-session strings. citeturn39search0turn39search1turn37search2 |
| **JetStream directly matches the required offline-first semantics.** Streams persist messages; durable consumers persist delivery progress; `AckWait` and redelivery handle temporary failures; pull consumers let sleepy nodes catch up on reconnection. | **[established]** | This is the cleanest replacement for “file sync eventually catches up.” citeturn32search4turn32search1turn32search0turn32search10 |
| **NATS is operationally light by design.** Official docs describe it as a single binary with no external dependencies, easy installation, and typical memory use under **20 MB** [single-source for the RAM figure]. | **[established]** | This is unusually aligned with the “non-technical owner” constraint. citeturn10search3turn10search6turn8search3 |
| **Mosquitto is a credible alternative, but the model is narrower.** MQTT durable sessions hold subscriptions and queued **QoS 1/2** messages for offline clients, but you must manage stable client IDs, session expiry, and higher-level coordination yourself. | **[established]** | Strong transport, weaker substrate for coordination and rollout control. citeturn27search10turn27search14turn27search0turn27search17 |
| **Redis Streams has become more credible in 2025–2026, but it still does not give exactly-once end-to-end semantics on its own and remains a worse Windows fit.** Redis docs explicitly require idempotent consumer logic because `XAUTOCLAIM` can re-deliver after crashes, and official Windows guidance routes users to Memurai or WSL. | **[established]** | Good tool; wrong operational center of gravity for this fleet. citeturn28search0turn28search6turn34view0turn34view1 |
| **RabbitMQ Streams remains heavier than necessary here.** RabbitMQ Streams is technically solid, but Windows deployment requires Erlang and RabbitMQ’s own Windows docs maintain a companion page for known service/configuration issues. | **[established]** | Operational burden is misaligned with your simplicity requirement. citeturn33search0turn33search1turn33search3turn33search13 |
| **JetStream KV and Object Store can replace a large fraction of custom coordination plumbing, but not the semantic governance layer.** KV provides immediate consistency for monotonic writes/reads plus compare-and-set; Object Store can distribute artifacts; Streams provide append-only audit logs and durable consumption. | **[established]** | You can retire transport-coupled protocol code without pretending transport auth is a substitute for human approval or semantic signatures. citeturn9view0turn9view1turn32search7turn40search4 |
| **Do not start with NATS JWT decentralized auth unless you actually need delegated multi-tenant administration.** NATS’ own security docs say to pick the simplest deployment model that meets the need. | **[established]** | For a one-person fleet, static NKeys plus ACLs beats premature JWT ceremony. citeturn31view1turn10search12turn30search14 |
| **Dark-launch migration patterns are mature and map well to this problem.** LaunchDarkly documents staged migrations with `dualwrite`, `shadow`, `live`, `rampdown`, and `complete`; Microsoft and Google both document shadowing traffic to validate correctness and load before promotion. | **[established]** | This is the correct migration shape for swapping a live bus. citeturn20view0turn20view1turn20view2turn20view3 |
| **There is a real but bounded durability debate around JetStream.** Jepsen’s 2025 analysis reported acknowledged-write loss scenarios under correlated failures and recommended stronger `fsync` defaults [single-source]; meanwhile NATS continues to improve correctness, including 2.14 fixes for mirroring/sourcing correctness and a 2026 consumer-loss regression acknowledged by maintainers [low-authority][single-source]. | **[emerging]** | For a single VPS pilot, this argues for conservative configuration and backup discipline, not abandonment of the platform. citeturn23view0turn26view0turn23view3 |
| **A messenger+file-sync bus objectively stops being enough once offline recovery, replay, and coordination correctness matter more than “it usually gets there eventually.”** That is already the case in your fleet because the dominant pain points are not UI visibility but transport catch-up, replay, and state convergence. | **[established]** | This is the actual migration trigger, and it has already fired. citeturn32search4turn32search1turn39search0turn38search1 |

A practical transport comparison is below.

| Option | Offline catch-up | Cross-platform ops | Windows story | macOS story | VPS footprint | Coordination primitives | Operator burden | Verdict |
|---|---|---|---|---|---|---|---|---|
| **NATS + JetStream** | Strong: persisted streams + durable consumers + replay + redelivery. citeturn32search4turn32search1turn32search0 | Single binary, no external deps. citeturn10search3turn10search6 | Official Windows service via `sc.exe`. citeturn13search0 | Official package-manager install via Homebrew; first-party launchd instructions **[no source found]**. citeturn10search6 | Small; typical RAM under 20 MB [single-source]. citeturn8search3 | Streams, KV CAS, Object Store, subjects, pull consumers. citeturn9view0turn9view1turn32search7 | Low to moderate | **Best fit** |
| **Mosquitto / MQTT persistent sessions** | Strong for QoS1/2 + persistent sessions; weaker replay/audit semantics. citeturn27search10turn27search14 | Lightweight broker. citeturn12search1turn8search1 | Official Windows binaries and service support. citeturn14view1turn25view3 | Homebrew install; first-party launchd instructions **[no source found]**. citeturn14view1 | Small | Mostly transport only; app must build more. | Low to moderate | **Best alternative** |
| **Redis Streams** | Strong log + consumer groups + reclaim; exactly-once requires app idempotency. citeturn28search1turn28search16turn28search0 | Simple if you already run Redis; otherwise another database to babysit. | Official Windows route is Memurai or WSL. citeturn34view0 | Native installs supported. citeturn25view0 | Moderate; in-memory design with AOF/RDB trade-offs. citeturn34view1 | Streams yes; KV yes; rollout artifact story weaker. | Moderate | **Plausible, but weaker fit** |
| **RabbitMQ Streams** | Strong persistent stream semantics. citeturn33search0 | Heavier, multi-component mental model. | Requires Erlang; Windows docs call out service/config issues. citeturn33search3turn33search13 | Supported, but more admin surface | Higher | Good queue/stream features; not single-binary | Higher | **Not recommended here** |

## Contradictions & Debates

The main technical debate is **NATS/JetStream versus MQTT/Mosquitto**. The pro-NATS argument is architectural breadth: one fabric gives you request/reply, pub/sub, persistent streams, queue semantics, KV, Object Store, and increasingly agent-specific ecosystem work. NATS’ own docs position it as a single technology spanning services, devices, and agents, and a recent NATS-native agent protocol has appeared in 2026 [single-source]. The counterargument is that MQTT is older, narrower, and therefore easier to reason about for offline command-and-control, with durable sessions that have been battle-tested for years. That counterargument is strongest when the workload is mostly telemetry and command fan-out, not general coordination. citeturn17search0turn25view4turn27search10turn27search14

The best steelman for MQTT is this: if you merely need **offline message queueing** for sleepy endpoints, MQTT already does that with durable sessions, stable client IDs, message expiry, and QoS. You avoid adopting a broader substrate than necessary, and Mosquitto in particular is operationally tiny. Academic 2026 benchmarking also found that Mosquitto is extremely efficient at low-to-moderate scale, even if it saturates earlier than NATS at very high connection counts because it is single-threaded. For a 6–15 node fleet, that saturation result is mostly irrelevant. citeturn27search10turn27search14turn27search17turn29view0

The best steelman for NATS is stronger in your exact case: your current bus is not only moving messages, it is also doing **coordination, replay, audit, catch-up, rollout control, and custom consensus-like workflow**. MQTT gives you transport guarantees, but not the coordination substrate. JetStream gives you a persistent append-only log, durable consumers, compare-and-set in KV, object distribution, work-queue patterns, replay, and subject-level ACLs in one deployment unit. That lets you delete custom bus code rather than merely relocating it. citeturn32search4turn9view0turn9view1turn32search7turn40search11

There is also a **durability debate around JetStream itself**. Jepsen’s 2025 analysis found acknowledged-write loss scenarios under correlated failures and criticized the durability default behavior [single-source]. That matters. At the same time, the relevant counterpoint is scope: your proposed Phase 2 is a **single VPS dark pilot**, not a multi-AZ financial transaction system, and NATS continues to ship correctness work, including 2.14 fixes around sourcing and mirroring and explicit operational guidance around monitoring and backup. The sober reading is not “JetStream is unsafe,” but “JetStream requires conservative settings and explicit backup/restore discipline if you care about durability.” citeturn23view0turn26view0turn9view3turn9view4

A smaller but important debate concerns **NKeys/JWT versus keeping SSH Ed25519 signatures for approvals**. Cryptographically, NKeys are also Ed25519-based, and NATS can authenticate without ever storing private keys on the server. But transport authentication and semantic authorization are different things. NKeys/JWT can replace “who may connect and publish/subscribe where”; they should not automatically replace “who approved this irreversible action.” The less convincing view is that one can collapse both into the broker. The stronger view is to use broker auth for transport and keep explicit application-level signatures or human approvals for irreversible actions. citeturn31view2turn31view1turn31view3

Finally, there is a process debate: **dual-write versus shadow-read**. LaunchDarkly’s staged model explicitly treats `dualwrite` and `shadow` as distinct migration stages. The case for dual-write is stronger when you need the new system warmed with real state. The case for shadow-read is stronger when you fear side effects or divergence from two authoritative writers. In your case, the right sequence is not one or the other; it is **dual-write with old-path authority, plus shadow-read verification from the new path**, because you need both production traffic and a clean rollback. citeturn20view0turn20view1turn20view3

## Best Practices

A small autonomous fleet should treat **human-readable chat as an interface, not as the source of truth**. Modern ChatOps examples keep chat as the collaboration surface while transport and state live elsewhere. Atlassian frames ChatOps as the place where alerts, reports, and incident conversation are centralized; AWS Chatbot routes operational events into Slack/Teams rather than using chat itself as the event bus; PagerDuty’s Slack integration continuously updates an incident card in-channel instead of making Slack the underlying transport. Your standing decision to keep Telegram as the mirror while moving load-bearing transport elsewhere is consistent with current practice. citeturn20view4turn21search0turn21search13

The dedicated bus should be **boring by default**. For NATS, that means one file-backed JetStream server on the VPS, a fixed `store_dir` on local SSD, monitoring on a local or firewalled port, and explicit binding to the Tailscale interface or Tailscale-routable host instead of `0.0.0.0`. NATS’ config supports explicit `host`/`listen`, and its monitoring docs warn that monitoring binds to all interfaces by default unless you restrict it. citeturn35view0turn35view1turn15search15

Offline-first consumers should be **durable, pull-based, and explicit-ack**. JetStream durable consumers persist delivery progress server-side, and `AckWait` governs redelivery when an acknowledgment is not received. Pull consumers are the better fit for sleepy laptops because they reconnect, fetch from last durable position, and drain backlog at their own pace. This is materially different from Core NATS, where slow consumers are dropped to protect the system. citeturn32search1turn32search0turn23view1turn40search4

For chat mirrors, use **coalescing and summarization** rather than one Telegram message per low-level event. That is not just aesthetic; it is the only sane way to stay far from Telegram’s published group limits. The machine bus can carry full-fidelity events; the Telegram bridge should emit condensed human-readable lines, rollups, and status edits, not raw broker chatter. Telegram’s documented group and chat limits make that a hard design constraint. citeturn36search0turn36search2

Configuration and rollout should be treated as **artifacts plus manifests**, not “the latest synced file won.” JetStream Object Store can hold versioned bundles or signed tarballs; KV can hold the rollout manifest and desired-version pointer; the event stream can hold rollout intents and node acknowledgments. This is exactly the pattern that prevents engine-version drift: nodes fetch by immutable artifact ID or hash, verify locally, then publish explicit adoption acknowledgments before a policy switch is armed. Object Store is not a general distributed filesystem, but it is entirely adequate for fleet artifacts that fit on the VPS filesystem. citeturn9view1turn9view0turn40search3

Backups must be **first-class**, not an afterthought. JetStream has built-in stream and account backup/restore commands for file-backed streams, and its docs explicitly recommend configuration management rather than purely interactive administration. Redis has strong persistence options too, but the official docs also warn that careless AOF migration can cause data loss. Between the two, NATS offers the cleaner operational story for your narrow use case. citeturn9view3turn35view2turn34view1

Security should start with the **simplest viable chain of trust**. NATS’ own JWT guide says to pick the simplest deployment model that does what you need; for a one-person fleet, static NKeys with subject ACLs are simpler than running decentralized JWT account issuance on day one. Because NKeys are based on Ed25519 and use challenge-response, they align nicely with your existing Ed25519 comfort level, but you should still keep irreversible approvals at the application layer and out of the Telegram mirror. citeturn31view1turn31view2turn31view3

## Failure Modes

**Telegram-as-transport failure modes** are not subtle anymore. On the Bot API side, the rate limits are published and easy to exceed during event storms: more than **1 msg/s** in a single chat eventually yields `429`, groups are capped at **20 msgs/min**, and bursts that are harmless to a machine bus can still become chat throttling. On the MTProto side, the deeper problem is unpredictability: Telegram exposes flood waits but not stable per-method or per-account ceilings, and Telethon’s own 2026 docs say exact limits are unknown and factor-dependent. That is acceptable for a human client; it is not acceptable for a transport substrate you are supposed to reason about analytically. citeturn36search0turn36search2turn38search1turn37search1

User-session reuse across machines is a **hard red flag**, not normal wear-and-tear. Telegram’s official API docs say that main sessions to the home DC are limited by `tmp_sessions`, which defaults effectively to one when absent or ≤1, and opening more concurrent main sessions can produce `AUTH_KEY_DUPLICATED`, invalidate the authorization key, and force re-login. That means a shared user session string is structurally incompatible with a parallel multi-node transport role. This alone is enough reason to demote user sessions to optional tooling rather than core bus infrastructure. citeturn39search0turn39search1

NATS has its own failure modes, and they should be treated seriously. In **Core NATS**, slow consumers are dropped; this is why sleepy-node catch-up belongs on JetStream, not on bare pub/sub. In JetStream, retention and consumer position must be managed carefully: if messages are evicted and a consumer falls behind, misleading “healthy but stuck” states are possible [single-source]. JetStream also had real durability criticism in Jepsen’s 2025 analysis under correlated failure scenarios [single-source], and maintainers acknowledged a 2026 clustered consumer-loss regression [low-authority][single-source]. None of this rules out JetStream for your pilot, but it does rule out naive “set and forget” administration. citeturn23view1turn22search3turn23view0turn23view3

Mosquitto/MQTT’s main failure mode in this context is **deceptive simplicity**. Durable sessions work only if client IDs are stable and the session mode is configured correctly; MQTT 5 session expiry and message expiry help, but badly designed durable-session clients can accumulate stale broker state. Offline queueing is also bounded by QoS and broker storage policy, so the transport works well only when the application is disciplined. You do not get an event ledger, compare-and-swap config state, or artifact distribution for free. citeturn27search10turn27search0turn27search17

Redis Streams fails differently. The stream itself is fine; the main trap is assuming it gives more than it does. Redis’ own docs explicitly say `XAUTOCLAIM` can re-deliver after crashes and therefore consumer logic must be idempotent. Persistence is powerful, but configuration mistakes matter: the Redis persistence docs warn that careless switching to AOF can cause data loss, and the Windows support story for open-source Redis is not native in the way NATS or Mosquitto are. For a one-person operator, that extra set of operational foot-guns matters. citeturn28search0turn28search6turn34view1turn34view0

A final class of failure is **human-system coupling**. Projects that keep chat integral to workflows generally do so as a visibility/control layer, not as the transport of record. When chat becomes the transport, rate limiting, auth churn, and moderation/abuse rules leak directly into system behavior. Your current architecture already shows that coupling cost. The permanent Telegram mirror is a good idea; the mistake would be keeping Telegram as the authoritative delivery layer beneath it. citeturn20view4turn21search0turn36search0turn39search0

## Strategic Recommendations

There are four concrete options.

**Option NATS Core** is to keep Syncthing as the authoritative bus and merely harden it. That is the lowest-effort path and respects operator familiarity, but it does not solve the decisive problem: lossless replay and fast catch-up for nodes that sleep for hours or days. It also leaves a custom coordination layer load-bearing for semantics that a message substrate can already provide. This is the best option only if the real requirement is “good enough eventually” rather than “lossless and explainable.” citeturn32search4turn32search1

**Option NATS JetStream Pilot** is to move the machine transport to one VPS-hosted NATS server with file-backed JetStream while keeping Telegram as a permanent mirror and Syncthing as fallback. This option gives you streams for append-only audit, durable pull consumers for sleepy-node catch-up, KV for rollout manifests and leader lease/CAS patterns, Object Store for agent bundles, built-in monitoring, and formal backup/restore. It also preserves the single-binary, low-footprint, cross-platform repairability you asked for. Its main risks are configuration discipline, conservative durability settings, and avoiding overengineering with clusters, leaf nodes, or JWT ceremony too early. citeturn32search4turn32search1turn9view0turn9view1turn9view3turn9view4turn8search3turn13search0

**Option MQTT Durable Sessions** is to adopt Mosquitto with durable sessions and QoS-based offline queueing, then build the Telegram mirror bridge above it. This is the strongest alternative if you want the smallest possible conceptual jump from “messages over unreliable links” to “messages plus offline queueing.” It is weaker if you want the bus itself to help with audit, rollout control, and custom protocol deletion. This option can absolutely work, but it shifts more state-machine burden back into your application code. citeturn27search10turn27search14turn12search1turn25view3

**Option Redis Streams** is to centralize on Redis for stream semantics, consumer groups, and idempotent recovery. This is viable when a team already wants Redis for multiple other reasons. For this fleet, it is hard to justify over NATS because Windows support is second-class in open-source form, exactly-once is still an application responsibility, and the ops model is more “database you must babysit” than “lightweight bus you can reason about.” citeturn28search1turn28search16turn28search0turn34view0

My recommendation is **Option NATS JetStream Pilot** with the following strategy:

Use **static NKeys + subject ACLs first**, not JWT account delegation. Keep your existing **application-level Ed25519 signatures** for irreversible approvals. Represent the machine bus with three broker-native layers: one **events stream** as the append-only ledger, one **KV bucket** for desired state and rollout manifests, and one **Object Store** for versioned artifacts. Keep Telegram as a **one-way human-readable mirror bridge** fed from the event stream, with coalescing to stay well below Telegram’s group limits. Keep Syncthing during the pilot as rollback/fallback, not as a peer authoritative rail. Confidence: **[established]**. Main assumption: your operator constraint matters more than theoretical multi-node broker HA, so one very understandable server is better than a “more robust” architecture that is harder to repair. citeturn31view1turn31view2turn9view0turn9view1turn36search0turn9view3

## Action Plan

The migration should use a staged dark-launch pattern.

First, make the **transport split explicit**. Define Telegram as the permanent mirror only. Define the machine transport of record as either `SYNCTHING_PRIMARY` or `NATS_PRIMARY`, never both at once. That sounds trivial, but it prevents the most common dual-rail migration error: two rails silently becoming co-authoritative. The migration literature is consistent here—authority needs to be stage-specific and reversible. citeturn20view0turn20view1turn20view3

Second, deploy a **minimal NATS/JetStream server** on the VPS:
- single server, file-backed JetStream, local SSD store;
- bind client and monitoring ports to the Tailscale-reachable address or localhost-plus-firewall, not open internet;
- enable monitoring on `8222` but restrict exposure;
- create a static NKey user per node with subject ACLs;
- define one events stream, one work-queue/task stream if needed, one KV bucket for manifests/state, one Object Store bucket for artifacts. citeturn35view0turn15search15turn13search0turn9view0turn9view1

Third, start with **dual-write but old-path authority**:
- existing agents continue to append to Syncthing JSONL as the authoritative path;
- a bridge republishes each accepted event into JetStream using `Nats-Msg-Id` for deduplication;
- the Telegram mirror consumes from the old authoritative rail initially, not the new one;
- JetStream consumers are only observers in this stage.  
This matches the documented `dualwrite` and `shadow` stages: old remains authoritative while the new system is fed real traffic and compared. citeturn20view0turn40search3turn40search1

Fourth, add **shadow-read verification**:
- for each node, create a durable pull consumer scoped to the subjects it should own;
- on reconnect, the node reads from its durable consumer position and reports backlog depth and catch-up latency;
- compare Syncthing-observed ordering with JetStream stream sequence and consumer acknowledgment state;
- compare mirror output rendered from JetStream against the existing Telegram mirror text, but keep the old mirror authoritative until parity is proven. citeturn32search1turn32search0turn9view4turn9view5

Fifth, test the **promotion gates** in the order below. Promotion should not happen unless all are true.

| Gate | Pass condition | Why it exists |
|---|---|---|
| **Losslessness** | Zero missing events across authoritative Syncthing ledger vs JetStream stream sequence during pilot window. | Migrating transport without losslessness defeats the purpose. citeturn32search4turn40search3 |
| **Sleepy-node catch-up** | A node sleeping for hours or days reconnects and drains backlog from its durable consumer without manual intervention. | This is the primary migration driver. citeturn32search1turn32search0 |
| **Crash recovery** | Process kill/restart hammering of publisher, consumer, and broker does not lose acknowledged events and does not wedge consumers. | Matches shadow-testing logic and surfaces real broker/consumer bugs. citeturn20view1turn23view1turn9view3 |
| **Mirror safety** | Telegram mirror stays within published group/chat pacing limits even during event storms because batching/coalescing is in place. | Prevents transport migration from recreating messenger failures in the mirror layer. citeturn36search0turn36search2 |
| **Code deletion** | At least **30%** of custom bus/coordination transport code is removed, not merely wrapped. | If the broker does not reduce custom state-machine burden, TCO has not improved. |
| **Rollback** | Switching authority back to Syncthing requires only one config flip and no data surgery. | Staged migrations must be reversible. citeturn20view0turn20view2 |

Sixth, replace the custom propose/accept/commit transport pieces **selectively**, not ideologically. A good mapping is:
- **Stream** = immutable event ledger;
- **KV CAS key** = leader lease or rollout manifest pointer;
- **Object Store** = artifact/code bundle;
- **Durable consumer ACKs** = per-node execution receipts;
- **App-level Ed25519 signatures** = semantic approvals for irreversible actions.  
That lets you retire transport scaffolding while keeping the governance layer that matters. citeturn9view0turn9view1turn32search7turn31view2

Seventh, defer **leaf nodes**. Leaf nodes are valuable when you have an always-on local island that must keep working while disconnected from the hub. NATS documents exactly that use case. But your laptops are sleepy endpoints, not always-on islands, and NATS docs also still carry caveats around WorkQueue/Interest mirroring over leaf connections. So leaf nodes are a later optimization, not part of the first migration. citeturn32search15turn32search5turn32search9

Eighth, document two boring backups:
- nightly `nats account backup` for all streams and consumer state;
- separate filesystem backup of the JetStream store and config bundle.  
Also document one boring recovery drill: restore to a fresh VPS and verify that a dormant laptop can reconnect and resume from durable consumer state. The official NATS disaster-recovery docs support this directly. citeturn9view3

## Sources

The list below includes every source used materially in the analysis. Dates are shown when the source published one; otherwise the source is marked **undated**.

- **Telegram Bots FAQ** — Telegram Core, **undated**. Bot limits for single chat, group, and bulk notifications. citeturn36search0
- **Telegram Bot API** — Telegram Core, **undated**. `allow_paid_broadcast` / paid broadcast up to 1000 msg/s. citeturn36search2
- **Telegram API Errors** — Telegram Core, **undated**. `AUTH_KEY_DUPLICATED`, `FLOOD_WAIT_X`. citeturn39search0
- **Working with Different Data Centers** — Telegram Core, **undated**. `tmp_sessions`, single main session default, update delivery across sessions. citeturn39search1
- **Telethon Documentation PDF** — Telethon, **2026-06-15**. “Nobody knows the exact limits” language and rate-limit caveats. citeturn38search1
- **Telethon RPC Errors** — Telethon docs, **undated**. `FloodWaitError` handling. citeturn37search1
- **Telethon API Errors** — Telethon docs, **undated**. `AUTH_KEY_DUPLICATED` exposed as auth-key error. citeturn37search2
- **Telethon client docs** — Telethon docs, **undated**. Flood-sleep behavior and request throttling behavior. citeturn37search4
- **NATS Docs Welcome** — NATS docs, **2025-08-01**. Single binary, overview of JetStream capabilities. citeturn10search3
- **Installing a NATS Server** — NATS docs, **2025-10-01**. Install simplicity. citeturn10search6
- **About NATS** — NATS.io, **undated**. Typical RAM under 20 MB [single-source]. citeturn8search3
- **Windows Service** — NATS docs, **2024-09-04**. Official Windows service support. citeturn13search0
- **Configuring NATS Server** — NATS docs, **2026-05-12**. Bind/listen/host/client_advertise configuration. citeturn35view0turn35view1turn35view2
- **Monitoring** — NATS docs, **2026-05-06**. Monitoring endpoints and exposure warning. citeturn9view4turn15search15
- **Monitoring JetStream** — NATS docs, **undated**. JetStream advisories. citeturn9view5
- **Disaster Recovery** — NATS docs, **undated**. Stream/account backup and restore. citeturn9view3
- **JetStream** — NATS docs, **2025-12-19**. Persistence, replay, delivery guarantees. citeturn32search4turn40search1
- **Consumers** — NATS docs, **2026-04-30**. `AckWait`, durable consumers, at-least-once semantics. citeturn32search0turn40search4
- **Consumer Details** — NATS docs, **2026-06-10**. Durable consumer progress persisted on server. citeturn32search1
- **Streams** — NATS docs, **2026-04-30**. Stream semantics and persistence. citeturn30search5turn32search2
- **JetStream Model Deep Dive** — NATS docs, **2026-04-30**. Retention policies, queues, exactly-once semantics. citeturn32search7turn40search0
- **Headers** — NATS docs, **2026-05-12**. `Nats-Msg-Id` deduplication. citeturn40search3
- **Key/Value Store** — NATS docs, **last updated ~2026-02**. Immediate consistency qualifiers, CAS ops. citeturn9view0
- **Object Store** — NATS docs, **last updated ~2025-07**. File bucket semantics and caveat that it is not a distributed filesystem. citeturn9view1
- **NKeys** — NATS docs, **undated**. Ed25519 basis and challenge-response auth. citeturn10search12turn31view2
- **In Depth JWT Guide** — NATS docs, **2024-10-28**. JWT hierarchy, simplest deployment model guidance. citeturn31view1
- **Authenticating with a Credentials File** — NATS docs, **2024-10-28**. JWT + NKey creds files. citeturn31view3
- **Account lookup using Resolver** — NATS docs, **2025-08-22**. Built-in resolver and eventual consistency for auth data. citeturn10search11
- **NATS Server 2.14 Release** — NATS blog, **2026-04-30**. Correctness fixes and new JetStream publish behavior. citeturn26view0
- **What’s old is new: A NATS-native protocol for AI agents** — NATS blog, **2026-05-25** [single-source]. citeturn25view4
- **JetStream on Leaf Nodes** — NATS docs, **2024-09-17**. Edge/disconnected-island leaf-node use case. citeturn32search15
- **Source and Mirror Streams** — NATS docs, **2026-04-30**. Caveat on WorkQueue/Interest over leaf nodes. citeturn32search5
- **MQTT support in NATS** — NATS docs, **2026-05-21**. When MQTT support is intended to be used. citeturn32search16turn30search0
- **Benchmarking Message Brokers for IoT Edge Computing** — arXiv, **2026-03-23**. Comparative benchmark across Mosquitto, EMQX, HiveMQ, RabbitMQ, NATS, Redis Pub/Sub, Zenoh. citeturn29view0
- **Eclipse Mosquitto Download** — Mosquitto, **undated page**. Windows binaries and macOS Homebrew install. citeturn14view1
- **Eclipse Mosquitto homepage** — Mosquitto, **undated**. Lightweight positioning. citeturn12search1turn8search1
- **Mosquitto 2.1.0 release notes** — Mosquitto blog, **2026-01-29**. Windows services, connection-count increase. citeturn25view3
- **mosquitto.conf man page** — Mosquitto, **~2026-02**. Bind/listener options, persistent client expiration. citeturn12search5turn15search1turn27search0
- **MQTT man page** — Mosquitto, **~2026-02**. Durable sessions and offline QoS1/2 storage. citeturn27search10
- **mosquitto_pub / mosquitto_sub man pages** — Mosquitto, **~2026-02**. Persisted sessions and queued delivery on reconnect. citeturn27search3turn27search14
- **Redis Streams docs** — Redis, **2026-04**. Stream data type and command surface. citeturn28search1turn8search2
- **Redis XREADGROUP docs** — Redis, **2026-07-14 page date**. Blocking reads, pending access, `CLAIM` option. citeturn28search2
- **Redis XAUTOCLAIM docs** — Redis, **2026-07-14 page date**. Crash recovery/reclaim semantics. citeturn28search16
- **Redis streaming guides** — Redis docs, **2026-07-14 page date**. Idempotency requirement after re-delivery. citeturn28search0turn28search6
- **Redis Open Source 8.2 release notes** — Redis docs, **2025-08 GA; page updated 2026**. Streams enhancements and performance improvements. citeturn25view0
- **Redis 8.2 streams and bitmap enhancements** — Redis blog, **2025-08-12**. `XACKDEL` / `XDELEX`. citeturn25view1
- **Install Redis on Windows** — Redis docs, **undated page**. Official guidance to Memurai or WSL. citeturn34view0
- **Redis persistence** — Redis docs, **undated page**. AOF/RDB behavior and migration warnings. citeturn34view1
- **Redis security** — Redis docs, **2026-07-14 page date**. Bind/firewall/TLS/ACL guidance. citeturn34view2
- **RabbitMQ Streams** — RabbitMQ docs, **undated**. Stream semantics. citeturn33search0
- **RabbitMQ install on Windows** — RabbitMQ docs, **undated**. Official Windows install guide. citeturn33search1
- **RabbitMQ install on Windows manually** — RabbitMQ docs, **undated**. Erlang requirement. citeturn33search3
- **RabbitMQ Windows Configuration** — RabbitMQ docs, **undated**. Known Windows service/config conditions. citeturn33search13
- **Performing multi-stage migrations with migration flags** — LaunchDarkly docs, **undated page**. `dualwrite`, `shadow`, `live`, `rampdown`, `complete`. citeturn20view0
- **Shadow Testing** — Microsoft Engineering Playbook, **2024-09-17**. Definition and behavior of shadow testing. citeturn20view1
- **AWS Prescriptive Guidance deployment strategies** — AWS, **undated**. Blue/green, canary, shadow. citeturn20view2
- **Dark launches: what they are and how to do them** — Google Cloud Blog, **2017-08-03**. Older but still canonical dark-launch explanation; older source explicitly acknowledged. citeturn20view3
- **ChatOps for incident management** — Atlassian, **undated**. Chat as collaboration surface and durable written record. citeturn20view4
- **AWS Chatbot** — AWS, **undated**. Event-to-chat operational model. citeturn21search0
- **PagerDuty Slack user guide / integration** — PagerDuty, **undated + 2026-05-06 article**. Incident cards continuously updated in chat. citeturn21search13turn21search5
- **AutoGen docs** — Microsoft, **undated**. Event-driven pub/sub agent runtime. citeturn18search0turn18search1turn18search4
- **CrewAI Event Listeners docs** — CrewAI, **undated**. Singleton event bus architecture. citeturn18search2turn18search18
- **LangChain Agent Server docs** — LangChain, **undated**. Redis-backed signaling and durable runtime. citeturn18search3turn18search27turn18search21
- **Jepsen: NATS 2.12.1** — Jepsen, **2025-12-08** [single-source]. Durability critique under correlated failures. citeturn23view0
- **NATS consumer-loss regression discussion** — GitHub discussion, **2026-03-18** [low-authority][single-source]. Maintainer acknowledgment of clustered consumer-loss regression. citeturn23view3
- **Matrix.org outage postmortem** — Matrix.org, **2025-10-29**. Included as contextual evidence of self-hosted chat complexity; already rejected in your roadmap. citeturn23view2