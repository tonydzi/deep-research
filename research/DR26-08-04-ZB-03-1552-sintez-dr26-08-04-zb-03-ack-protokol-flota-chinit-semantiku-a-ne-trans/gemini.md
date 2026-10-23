# DR26-08-04-ZB-03-1552 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Research Plan

Head-to-Head Architectural Comparisons: Evaluate explicit message broker acknowledgement models (NATS JetStream vs. AWS SQS) with a focus on visibility timeouts, negative acknowledgements (NACK), and dead-letter queue (DLQ) implementations, contrasting them against the current heuristic-based ledger.

Actor-Model State Machine Analysis: Review actor-model task tracking frameworks, specifically examining Erlang/OTP gen_server state timeouts and selective message reception patterns, to derive a minimal, robust state machine suitable for Python-based autonomous agents.

Modern Agent Protocol Evaluation: Compare emerging 2025–2026 multi-agent coordination protocols (Google A2A, Model Context Protocol (MCP), Multi-Principal Agent Coordination Protocol (MPAC), and Agent Communication Protocol (ACP)) to identify standardized methods for tracking intent, execution, and state across distributed nodes.

Local Ledger Concurrency Analysis: Investigate the feasibility of a single reconciled ledger utilizing SQLite in Write-Ahead Logging (WAL) mode to resolve ledger divergence without requiring complex distributed consensus algorithms.

Failure Mode Deep-Dive: Allocate extensive analysis to classic distributed failure modes (ack storms, poison messages, head-of-line blocking) and network-specific anomalies (Syncthing vector clock conflicts) to ensure the proposed architecture degrades safely.

Constraint Mapping: Map all theoretical findings strictly to the eight defined operational weaknesses, prioritizing deterministic, zero-LLM, "hammer-and-screwdriver" repairable designs suitable for a non-technical operator managing an unreliable dual-rail channel (Telegram + Syncthing).

Research Report: Acknowledgement Protocols for Asynchronous Multi-Agent Fleets
A. Executive Summary

The orchestration of autonomous Large Language Model (LLM) agents across a distributed, multi-machine fleet relies fundamentally on the reliability of the underlying communication medium. When operating over a dual-rail, unreliable, and potentially reordering transport layer—such as a Telegram group and a Syncthing-synced folder—heuristic-based task tracking is mathematically and practically insufficient. The current implementation, which relies on silence detection to infer successful task completion, predictably degrades into false-positive task closures, uncoordinated parallel executions, and irreconcilable ledger states.

This research report investigates the state-of-the-art in acknowledgement semantics, delivery-confirmation protocols, and multi-agent coordination frameworks to address these systemic weaknesses. The analysis indicates that the challenges faced by this personal AI fleet mirror classical distributed systems problems, specifically concerning consensus, idempotency, and message delivery guarantees over lossy networks. By integrating established enterprise messaging patterns—such as the Transactional Outbox, Idempotent Receivers, and Leased Visibility Timeouts—with emerging 2026 standards in multi-agent coordination, it is possible to construct a highly resilient acknowledgement layer that operates independently of the transport mechanism.

Crucially, this report aligns all theoretical solutions with the "AK-47 principle" constraint: the architecture must remain simple, deterministic, and fully repairable by a non-technical operator using basic tools. The resulting strategic recommendations provide a blueprint for a decentralized, SQLite-backed state machine utilizing Universally Unique Lexicographically Sortable Identifiers (ULIDs) and explicit acknowledgement primitives, ensuring that no task is ever silently dropped, duplicated, or prematurely escalated.

B. Key Findings

The following key findings synthesize the primary mechanisms required to stabilize asynchronous message-passing between autonomous agents over an unreliable channel.

Idempotency Over Delivery Guarantees [established]: Perfect "exactly-once" delivery over an unreliable network is theoretically impossible without application-layer deduplication. Robust distributed systems rely on "at-least-once" delivery paired with deterministic Idempotency Keys and an Idempotent Receiver pattern to safely discard duplicate messages.   

Explicit State Transitions Over Silence Detection [established]: Polling for the absence of a failure message (silence detection) inherently leads to false positives under network partitions or high latency. Production message brokers, such as NATS JetStream, mandate explicit ACK, NAK (negative acknowledgement), IN_PROGRESS (lease extension), and TERM (terminal failure) signals to transition a message's state.   

Leased Visibility Timeouts [established]: Relying on static timeouts (e.g., fixed 20/40/60 minute escalation ladders) causes false retries for complex, long-running tasks. Systems like AWS SQS and NATS utilize a "visibility timeout" where an in-flight task is temporarily hidden from other workers, but the active worker can periodically extend the lease via a programmatic heartbeat.   

Protocol Convergence for Multi-Agent Systems [emerging]: As of early 2026, baseline Agent-to-Agent (A2A) and Model Context Protocol (MCP) standards lack sophisticated multi-principal conflict resolution. The emerging Multi-Principal Agent Coordination Protocol (MPAC, arXiv:2604.09744) addresses this gap by defining five explicit layers (Session, Intent, Operation, Conflict, Governance) utilizing Lamport-clock causal watermarking and normative state machines.   

Concurrency Control via Advisory Re-planning [emerging]: Traditional database locks and optimistic concurrency control (OCC) fail for LLM agents due to excessively long inference windows. The Monotonic Trajectory Pre-Order (MTPO) protocol proposes "advisory concurrency," where agents are notified of state changes mid-task and use their LLM reasoning capabilities to selectively repair their execution plans rather than aborting entirely.   

Single Reconciled Ledger via SQLite WAL [established]: Maintaining separate sender and receiver ledgers inherently leads to state drift. Implementing a local SQLite database configured in Write-Ahead Logging (WAL) mode (PRAGMA journal_mode=WAL; PRAGMA busy_timeout=5000;) allows concurrent read/write access without locking the event loop, enabling a deterministic, single-source-of-truth Inbox/Outbox ledger on each machine.   

C. Contradictions & Debates

The design of a reliable messaging layer over an unreliable transport requires navigating several fundamental contradictions in computer science and distributed systems theory. The resolution of these debates directly informs the architectural choices for a multi-machine AI fleet.

Exactly-Once vs. At-Least-Once Delivery Guarantees

A persistent debate in distributed systems architecture revolves around delivery guarantees. Various commercial frameworks historically advertised "exactly-once" message processing. However, distributed systems theory—rooted in the Two Generals' Problem and the FLP (Fischer-Lynch-Paterson) Impossibility Result—dictates that perfect coordination and exactly-once delivery over an unreliable network cannot exist. A network timeout leaves the sender fundamentally ignorant of whether the task was successfully processed before the connection dropped, or if the payload never reached the receiver in the first place.   

The industry consensus resolves this contradiction by separating the concept of network delivery from application processing. Systems must assume and design for "at-least-once" delivery, pushing the burden of deduplication to the application layer via an Idempotent Receiver. The receiving agent must track the unique identifiers of processed messages and deterministically return a cached result—or simply ignore the command—if a duplicate transmission arrives. In the context of the dual-rail bus, receiving a task via Telegram and then receiving the identical task via Syncthing is not a failure of the transport; it is a guarantee of delivery that must be gracefully deduplicated by the agent's local state machine.   

Dual-Ledger Reconciliation vs. Single Decentralized Ledger

In peer-to-peer architectures, there is an ongoing debate regarding state management: whether each node should maintain a localized ledger requiring complex distributed consensus algorithms (like Raft or Paxos), or if the system should rely on a synced single ledger. While distributed consensus is theoretically sound for highly available enterprise clusters, it catastrophically fails the "hammer-and-screwdriver" simplicity constraint required for a personal AI fleet.   

Conversely, attempting to maintain separate "sender state" and "receiver state" files—as currently implemented with the JSON ledger and the inbox_debt.py scanner—guarantees eventual divergence. The modern resolution, particularly for file-synced architectures, is the Transactional Inbox/Outbox Pattern. Each agent maintains its own definitive local database of what it has sent (the Outbox) and what it has received (the Inbox). The transport layer acts strictly as a dumb pipe moving immutable messages between these local, single-source-of-truth ledgers. There is no attempt to achieve perfect global consensus; rather, each agent operates deterministically based on its local, cryptographically correlated inbox.   

Explicit Brokering vs. Transport-Agnostic Acknowledgement

Traditional enterprise systems rely on dedicated message brokers (such as Apache Kafka, RabbitMQ, or NATS) to handle acknowledgement, visibility timeouts, and dead-letter routing internally at the transport layer. However, the assigned constraints mandate that the dual-rail transport layer (Telegram and Syncthing) be treated as a given, potentially lossy channel.   

Consequently, the acknowledgement semantics must be lifted out of the transport layer and implemented entirely at the application layer. While implementing queue semantics over a flat-file sync protocol or a consumer chat application introduces complexity, 2026 protocols like the Agent Communication Protocol (ACP) and MPAC demonstrate that acknowledgement and conflict resolution can be entirely decoupled from the transport mechanism. By utilizing structured envelopes carrying strict state transition markers, the application layer can enforce broker-like reliability over any arbitrary transport medium.   

D. Best Practices

To resolve the systemic weaknesses in the current acknowledgement design, the architecture must adopt several canonical distributed systems patterns, adapted for the constraints of autonomous LLM agents.

Messaging Concept	Enterprise Implementation	Adaptation for Autonomous AI Fleet
Deduplication	Kafka Idempotent Producer	SQLite INSERT OR IGNORE on ULID Primary Key
State Tracking	SQS Visibility Timeout	SQLite expires_at timestamp with LLM heartbeats
Poison Pill Handling	RabbitMQ Dead Letter Exchange	delivery_attempts counter transitioning to DEAD state
Delivery Confirmation	NATS JetStream ack() / nak()	Explicit envelope tags (e.g., [ACK], [NAK]) parsed by a deterministic watchdog
The Transactional Inbox and Outbox Patterns

To ensure that an agent's internal state accurately reflects the messages it transmits over the dual-rail bus, the Transactional Outbox pattern is a mandatory architectural standard. When an agent decides to delegate a task, it does not write directly to Telegram or Syncthing. Instead, it writes the task to its local database (the Outbox) in the same transaction as its internal state update. A separate, deterministic background process sweeps the Outbox and pushes the pending messages to the dual-rail transport.   

Symmetrically, the Inbox Pattern ensures at-least-once processing. Upon receiving a message from the bus, a listener script writes the raw message to a local Inbox table. The LLM agent processes tasks exclusively from this local Inbox. If the agent experiences a catastrophic failure mid-task, the task remains in the Inbox for retry upon restart, completely decoupling the ephemeral network delivery from the durable task execution.   

Standardized Acknowledgement Primitives

Drawing heavily from the design of NATS JetStream, a robust state machine requires explicit acknowledgement primitives rather than relying on an ambiguous "responsive" status. Mature systems define at least four distinct responses:   

ACK (Acknowledge): The task has been completed successfully. The sender can permanently close the ledger debt.

NAK (Negative Acknowledgement): The task failed due to a transient error (e.g., an external API rate limit, a temporary network outage). The sender should apply exponential backoff and retry the task later.   

IN_PROGRESS (Working): The agent has commenced a long-running task. This serves as a heartbeat to reset the sender's visibility timeout, preventing premature reassignment or unnecessary escalation.   

TERM (Terminal): The task is structurally invalid, un-processable, or constitutes a "poison message" (e.g., requesting the agent to parse an infinitely recursing directory). The sender must not retry this task; it should be routed immediately to a Dead Letter Queue (DLQ) for human intervention.   

Correlation and Idempotency Keys (ULID)

String matching and heuristic tags (such as tracking the presence of ✅ принял) invariably lead to false positives when agents engage in conversational tangents. Every assigned task must possess a globally unique, time-sortable correlation ID. ULID (Universally Unique Lexicographically Sortable Identifier) has emerged as the industry standard for this purpose.   

A ULID is a 128-bit identifier encoded as 26 characters in Base32, making it both human-readable and URL-safe. Unlike standard UUIDs, a ULID combines a 48-bit millisecond timestamp with 80 bits of randomness, ensuring that identifiers are naturally chronological without relying on synchronized distributed clocks. When Agent A sends a task, it generates ULID-1. For Agent B's acknowledgement to be valid, it must explicitly cite ULID-1. If Agent A sends a follow-up query, it generates ULID-2 but includes an in_reply_to: ULID-1 header, creating an immutable, strictly ordered cryptographic chain of custody.   

Leased Visibility Timeouts

AWS SQS pioneered the "visibility timeout" concept, which is highly applicable to the unpredictable execution times of LLM agents. When an agent claims a task from its Inbox, the task is marked "invisible" to other potential workers or retry-monitors for a specific duration (T
v
	​

). If the agent crashes or hangs, it fails to send an ACK or IN_PROGRESS heartbeat. Once T
v
	​

 expires, the task becomes visible again and is automatically retried by the sender's watchdog.   

In Erlang/OTP, the gen_server behavior implements a similar paradigm via state timeouts. A gen_server can enter a state with a specific timeout; if no message is received before the timer expires, a timeout event is triggered. However, Erlang's architecture highlights a critical vulnerability: any generic system message arriving in the mailbox can inadvertently reset the idle timeout. Therefore, mature timeout management requires absolute, point-in-time deadlines (e.g., expires_at = timestamp + 15m) rather than relative idle timers, ensuring that only explicit IN_PROGRESS heartbeats can extend the lease.   

E. Failure Modes

Implementing an acknowledgement protocol over an unreliable transport introduces classical failure modes that must be proactively mitigated in the system design.

Ack Storms and Timeout Mismatches

An "Ack Storm" occurs when the base visibility timeout is configured to be shorter than the actual processing time (T
process
	​

>T
timeout
	​

). The sender assumes the task has failed and resends it. The receiver, now processing the same task twice, eventually sends two ACKs. The sender, confused by the duplicate acknowledgement, may trigger further compensatory logic, cascading into a storm of redundant messages.
Mitigation: Timeouts must be adaptive. Agents must be programmed to issue periodic IN_PROGRESS heartbeats for tasks that exceed standard execution windows, dynamically extending the visibility timeout on the sender's ledger.   

Poison Messages (The Infinite Loop)

A "poison message" is a task containing parameters that reliably crash the receiving agent. Because the agent crashes during execution, it never returns an ACK or a NAK. The sender's visibility timeout expires, and the deterministic watchdog resends the task, causing the receiver to crash again in an infinite loop.
Mitigation: The system must enforce a rigid max_deliver count. If a task is retried (e.g., 3 times) without ever receiving an ACK or TERM, the watchdog flags it as a poison message, transitions its state to DEAD, and routes it to a human operator via a Dead Letter Queue (DLQ).   

False-Positive Completion

The current system's reliance on "any subsequent message equals acknowledgement" is a critical vulnerability. If Agent B casually responds "Hello" to Agent A after receiving a critical file deletion task, Agent A's silence detector considers the task complete.
Mitigation: Implement strict ULID citation requirements. The deterministic ledger must enforce that no state change occurs unless the exact ULID is structurally cited in the reply.   

Sync Conflicts and Vector Clock Collisions

When utilizing Syncthing as a transport rail, concurrent modifications to the same file (e.g., two agents updating a shared ledger.json simultaneously) result in a vector clock collision. Syncthing resolves this by keeping the older file and renaming the newer one to .sync-conflict. This fragments the state and causes data loss from the perspective of the automated scripts.
Mitigation: Agents must never write to a shared state file. The system must utilize append-only event logs using independent files (e.g., outbox_agentA.jsonl), or leverage a local SQLite database in WAL mode, sidestepping Syncthing file conflicts entirely while preserving data integrity.   

F. Emerging Trends in Multi-Agent Coordination

The landscape of agent-to-agent communication has evolved rapidly through 2025 and 2026, transitioning from ad-hoc prompting to standardized, mathematically rigorous protocols.

MPAC (Multi-Principal Agent Coordination Protocol)

As of April 2026, the Model Context Protocol (MCP) and Google's Agent-to-Agent (A2A) protocol are recognized as insufficient for scenarios where agents lack a single controlling orchestrator. The Multi-Principal Agent Coordination Protocol (MPAC, arXiv:2604.09744) has emerged to address this, defining explicit coordination semantics across five logical layers: Session, Intent, Operation, Conflict, and Governance. MPAC makes intent declaration a precondition for action and represents conflicts as first-class structured objects. By utilizing Lamport-clock causal watermarking, MPAC provides deterministic ordering of events without requiring synchronized physical clocks, establishing a foundation for replayable, auditable agent interactions.   

Agent Communication Protocol (ACP)

The Agent Communication Protocol (ACP), introduced in February 2026 (arXiv:2602.15055), provides a REST-based, federated orchestration model for agents across disparate environments. ACP decouples the transport of information from semantic understanding, utilizing standardized endpoints and MIME types for multimodal message support. ACP highlights the industry shift toward treating agents as independent microservices that require formal Service-Level Agreements (SLAs) and structured, asynchronous handshakes for long-running tasks.   

Advisory Concurrency Control (CoAgent / MTPO)

Traditional concurrency control mechanisms—such as strict database locks or Optimistic Concurrency Control (OCC)—fail when applied to LLM agents. An agent's "transaction" (reasoning, tool use, and response generation) spans minutes of inference. Holding a lock for minutes paralyzes the system, while OCC's abort-and-retry mechanism forces the agent to discard minutes of expensive computational work upon a conflict. The Monotonic Trajectory Pre-Order (MTPO) protocol introduces "advisory concurrency control." When a conflict occurs, the framework issues a one-way notification to the affected agent. The agent utilizes its LLM reasoning to evaluate the conflict, patch its execution plan, and continue, rather than aborting entirely. While highly advanced, this underscores the necessity of the IN_PROGRESS and NAK states in keeping long-running tasks viable under contention.   

G. Strategic Recommendations

The following recommendations map directly to the 8 identified weaknesses, strictly adapted for the "hammer-and-screwdriver" operational constraint.

1. Binding an ACK to a Specific Task (Addressing Weakness 1)

Recommendation: Eradicate silence detectors and heuristic tag matching. Adopt a strict Idempotency Key mapping.
Generate a ULID for every distinct action. Modify the messaging envelope format to enforce structural integrity:
🤖 [AGENT_A -> AGENT_B] [TASK] <ULID> <Payload>   

The recipient must respond with a state-transition marker citing the exact ULID:
✅ [AGENT_B -> AGENT_A] [ACK] <ULID> <Artifact/Result>

The sender's deterministic watchdog must ignore all communication from the recipient except messages containing the exact ULID of the pending task.

2. The Minimal Agent State Machine (Addressing Weakness 2)

Recommendation: Implement a 6-state deterministic machine modeled on NATS JetStream and MPAC.   

State	Triggered By	Semantics
SENT	Sender generating task	Task written to local Outbox and pushed to dual-rail.
DELIVERED	Transport confirmation	Optional. Difficult over Syncthing/Telegram. Recommend skipping to simplify for the operator.
WORKING	Receiver sends IN_PROGRESS	Receiver has parsed the task and is actively running LLM inference. Extends visibility timeout.
DONE	Receiver sends ACK	Task completed successfully. Closes the ledger debt.
FAILED	Receiver sends NAK	Transient failure. Sender applies exponential backoff and resends.
DEAD	Receiver sends TERM	Poison task or offline agent. Escalate to human operator.
3. Single Reconciled Ledger (Addressing Weakness 3)

Recommendation: Abandon the dual JSON/Debt-scan ledgers. Standardize on the Local Inbox/Outbox architecture utilizing SQLite in WAL Mode.
Every machine runs a local SQLite database (ledger.db). Crucially, to avoid the database is locked errors common in concurrent Python scripts, the database must be initialized with specific pragmas: PRAGMA journal_mode=WAL; PRAGMA busy_timeout=5000;. This configuration allows the LLM agent to read from the database while the deterministic watchdog script simultaneously writes to it, eliminating deadlocks. The human operator can open ledger.db with a standard GUI tool and manually change a state from WORKING to DONE, fulfilling the "hammer-and-screwdriver" repairability constraint.   

4. Cross-Rail Fusion (Addressing Weakness 4)

Recommendation: Implement an Idempotent Receiver leveraging the single SQLite ledger.
When a message arrives via Telegram OR Syncthing, the deterministic watchdog parses the ULID and attempts an INSERT OR IGNORE into the Inbox table. If a message arrives via Syncthing first, it is inserted and marked as pending. If the exact same message arrives via Telegram 5 minutes later, the INSERT OR IGNORE fails silently because the ULID already exists as a Primary Key. This provides exactly-once processing over an at-least-once delivery dual-rail channel, effectively fusing the redundant channels.   

5. Standardized NACK Semantics (Addressing Weakness 5)

Recommendation: Enforce typed failure modes. The LLM must be explicitly prompted that if it cannot complete a task, it must output either a NAK (for temporary issues, e.g., "Target API is down") or a TERM (for structural issues, e.g., "I lack the required tools to execute this command"). This distinction prevents the watchdog from infinitely retrying mathematically impossible tasks.   

6. Robust Correlation Conventions (Addressing Weakness 6)

Recommendation: As detailed in Recommendation 1, replace the ad-hoc sha1[:10] strings with ULIDs. ULIDs are monotonically increasing, meaning the database is naturally sorted by time. This prevents B-tree fragmentation in the SQLite ledger and allows the human operator to chronologically debug the system at a glance.   

7. Dead-Letter Handling and Escalation (Addressing Weakness 7)

Recommendation: Add a delivery_attempts integer column to the Outbox table. The sender's watchdog increments this counter every time a visibility timeout expires. Set a system-wide MAX_DELIVER = 3. If delivery_attempts > MAX_DELIVER, the state transitions to DEAD (acting as a Dead-Letter Queue). The watchdog then formats a plain-English alert and sends it to the "police" Telegram channel, pinging the operator with the specific ULID and execution trail.   

8. Adaptive Visibility Timeouts (Addressing Weakness 8)

Recommendation: Replace fixed timeouts with a Leased Visibility model. Establish a base timeout of 15 minutes. If an LLM agent expects a task to take 30 minutes, it is instructed to send an IN_PROGRESS <ULID> heartbeat to the bus every 10 minutes. When the sender's watchdog parses IN_PROGRESS, it updates the next_timeout_at timestamp in the Outbox table to Current Time + 15 minutes. This entirely eliminates false-positive timeouts while ensuring that if the agent's machine physically crashes, the failure is caught exactly 15 minutes after the last successful heartbeat.   

H. Action Plan & Decision-Oriented Ending

To implement these recommendations without overwhelming the non-technical operator or destabilizing the current production environment, the following prioritized, simplicity-weighted roadmap is recommended.

Phase 1: Correlation & Idempotency (Highest ROI, Lowest Complexity)

Replace the sha1 hash generator with a standard Python ULID library.

Modify the agent system prompts to explicitly mandate citing the ULID in all replies (✅ [ACK] <ULID>).

Modify the sender watchdog to ignore any message that does not contain a known, active ULID.

Outcome: Instantly eliminates false-positive completions.

Phase 2: Single Reconciled Ledger (Medium ROI, Medium Complexity)

Deprecate the JSON sender ledger and the receiver debt-scan script.

Deploy a single ledger.db (SQLite) on each machine, strictly enforcing WAL mode (PRAGMA journal_mode=WAL).

Create outbox and inbox tables. Write a deterministic Python script (bus_sync.py) that reads from both transport rails and uses INSERT OR IGNORE (keyed on the ULID) to write to the inbox.

Outcome: Achieves cross-rail dedup, safely handles transport reordering, and provides a single, inspectable file for the operator.

Phase 3: State Machine & Leased Timeouts (High ROI, Medium Complexity)

Implement the WORKING, NAK, and TERM markers in the agent prompts.

Update the watchdog to reset the visibility timeout whenever a WORKING heartbeat is parsed.

Implement the delivery_attempts counter. If it hits 3, flag as DEAD and escalate to the Telegram police channel.

Outcome: Stops ack-storms, prevents infinite retries of poison messages, and dynamically scales to long-running tasks.

Final Recommendation:
The most critical immediate intervention is transitioning to ULID-based Idempotent Receivers using a SQLite WAL Ledger.

Confidence Tier: [established]

Main Assumption: This rests on the assumption that the Python environment on the host machines can maintain persistent background scripts to execute the bus_sync.py watchdog reliably, and that the file system supports SQLite WAL mode (which requires shared memory primitives, standard on all modern OS environments). Utilizing this architecture provides enterprise-grade reliability while remaining entirely legible to a human operator equipped with a standard SQLite browser.