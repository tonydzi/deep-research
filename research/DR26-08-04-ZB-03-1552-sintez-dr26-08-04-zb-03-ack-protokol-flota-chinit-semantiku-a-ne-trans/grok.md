# DR26-08-04-ZB-03-1552 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Thought for 1m 36s

A. Executive Summary

Best current practice for acknowledgement over unreliable, possibly-reordering channels between autonomous agents is at-least-once delivery + application-level (business) ACK that cites a specific correlation ID + idempotent processing. Pure end-to-end exactly-once is impossible (Two Generals’ Problem) and is never the right target for a small personal fleet.⁠YouTube

Mature systems (SQS, NATS JetStream, RabbitMQ, Akka AtLeastOnceDelivery, Google A2A Protocol v1.0) cleanly separate delivery from processing. Visibility timeouts / AckWait / progress heartbeats keep a message invisible or leased while work proceeds; only an explicit positive ACK (or DeleteMessage / terminal status) retires it. Negative outcomes are first-class (NACK / REJECTED / FAILED / Term). A single shared append-only ledger (or tightly reconciled dual ledgers keyed by ID) is preferred over independent sender/receiver books that can diverge.

For the 4–6-machine Telegram + Syncthing fleet the minimal correct design that satisfies the AK-47 constraint is:

Every actionable message carries a unique, time-sortable ID (ULID or UUIDv7).
Only a later message that explicitly cites that ID (ACK / NACK / PROGRESS / DONE) can change state.
Shared append-only ledger.md (Syncthing primary, Telegram mirror for alerts) is the single source of truth.
States: SENT → ACCEPTED → IN-PROGRESS → DONE | NACK | EXPIRED.
Cross-rail fusion is automatic (first valid citing message on any rail closes the debt).
Fixed but extendable SLA ladder + optional IN-PROGRESS heartbeat; dead-letter = EXPIRED after max attempts + human escalate.

This directly eliminates the eight listed weaknesses with almost no new moving parts. Confidence: [established] for the patterns; [emerging] for the exact minimal mapping to a non-technical personal fleet (assumption: operator can edit a Markdown file and a small JSON/YAML SLA table).

B. Key Findings

Delivery vs processing is the central distinction. SQS VisibilityTimeout, NATS AckWait + InProgress, Temporal Activity heartbeats, and A2A Task status all implement it. “Read cursor advanced” must never equal “handled”. [established]
Business-level ACK citing a concrete ID is the only reliable binder. Silence detectors produce false positives. Akka, EIP Correlation Identifier, A2A taskId, and NATS sequence all require the cite. [established]
At-least-once + idempotency is the production default everywhere (SQS, Kafka, RabbitMQ, NATS JetStream, Temporal). Exactly-once claims are always scoped (Kafka-to-Kafka transactions, or “effectively once” via deduplication). [established]
Explicit finite state machines with terminal states are universal in modern agent protocols (A2A: SUBMITTED / WORKING / INPUT_REQUIRED / COMPLETED / FAILED / CANCELED / REJECTED; MCP Tasks extension; Temporal Workflow + Activity states). [established / emerging 2025-26]
Single ledger or ID-keyed reconciliation prevents divergence. Dual independent ledgers (sender JSON + receiver debt scan) are an anti-pattern. Append-only event log is the simplest correct form. [established]
Cross-channel fusion is solved by treating the correlation ID as the sole identity; any rail that supplies a valid citing message is authoritative. Idempotent union. [established]
NACK / defer / partial / progress are first-class in NATS (Nak, InProgress, Term), RabbitMQ (basic.nack/reject), A2A (REJECTED/FAILED), and MCP (progress notifications + task status). Free-text only is insufficient. [established]
Correlation IDs: ULID / UUIDv7 preferred (time-ordered, collision-resistant). Short hashes are heuristic and error-prone. [established]
Dead-letter / poison / retry: maxReceiveCount (SQS), MaxDeliver (NATS), delivery-count + DLX (Rabbit), explicit EXPIRED + human channel. Fixed ladders work; adaptive via heartbeat extension is better and still simple. [established]
Multi-agent frameworks: LangGraph checkpointers give step durability but still need an external supervisor for true auto-resume; Temporal is the production durable-execution gold standard; A2A is the cleanest inter-agent task lifecycle; MCP is primarily tool-oriented with solid progress semantics. DIY fleets (Telegram + files) almost never solve the ACK layer cleanly. [established / emerging]

C. Contradictions & Debates

Exactly-once: Marketing claims vs theory. Kafka, Ably, etc. advertise EO; formal results and Two Generals show end-to-end EO is impossible without perfect channels. Consensus resolution: “effectively once” = ALS + idempotent consumer + deduplication window. What would settle it: formal proof under the system’s failure model. [established]
Checkpoints vs durable execution: LangGraph (and similar) persist state; Temporal (and Diagrid Catalyst) automatically re-drive failed workers. Critics (Diagrid) correctly note that checkpoints alone leave the process dead until an external watcher notices. [emerging debate 2025-26]
Adaptive vs fixed timeouts: Academic load-adaptive schemes exist; production systems favour fixed base + heartbeat extension (SQS ChangeMessageVisibility, NATS +WPI, Temporal Heartbeat). For non-technical operators fixed + optional progress is safer. [emerging]
Broker vs DIY: For 4–6 personal machines a full NATS/Temporal cluster adds operational surface that violates the hammer-and-screwdriver rule. Keep the dual-rail transport; harden only the ACK layer. [consensus for this scale]

D. Best Practices

Always emit a unique correlation ID at send time; require every status change to cite it.
Prefer at-least-once delivery; make every side-effect idempotent with respect to that ID.
Separate delivery confirmation from processing confirmation (visibility / lease / AckWait model).
Maintain an explicit, human-readable state machine with terminal states.
Single shared append-only ledger (or dual ledgers that reconcile by ID).
Support positive ACK, NACK (with reason), PROGRESS / IN-PROGRESS, and EXPIRED.
Dead-letter after bounded attempts; escalate to human channel.
Cross-rail: accept the first valid citing message on any rail; ignore subsequent duplicates.
Timeouts: fixed ladder + optional progress heartbeat that extends the deadline.
Zero-LLM bookkeeping; deterministic parsers only.

E. Failure Modes

Lost ACK → duplicate delivery (mitigated by idempotency).
False-positive “any subsequent message = ACK” (the current design’s core bug).
Dual ledgers diverge (unpaid on one side, paid on the other).
Single-rail detection fails when that rail is down.
Poison messages without DLQ → infinite redelivery or head-of-line blocking.
ACK storms from aggressive fixed ladders with no backoff or progress signal.
Heuristic tags / short hashes → false PAID / UNPAID.
Two Generals residual uncertainty (accepted; never claim certainty beyond the citing ACK).
Documented in broker post-mortems, Akka reliability notes, X practitioner threads on agent duplicate side-effects, and multi-agent coordination studies (coordination failures ~37 % of production agent incidents). [established / low-authority social]

F. Strategic Recommendations (mapped to weaknesses 1–8)

False-positive silence detector → Require explicit citation of the originating ID in every ACK / NACK / PROGRESS / DONE message. Silence is never success. (Akka deliveryId, EIP Correlation Identifier, A2A taskId pattern.)
No explicit state machine → Adopt the minimal machine:
SENT → ACCEPTED (cite-ID ACK) → IN-PROGRESS (optional progress cite) → DONE | NACK (reason) | EXPIRED.
Optional DELIVERED if a transport-level receipt is available. Maps 1:1 onto A2A TaskState.
Unreconciled dual ledgers → Move to a single shared append-only ledger.md (Syncthing) that both sender and receiver append to. Telegram becomes the alert / human channel only. Reconciliation = last valid transition by ID.
Single-rail detection → Fuse by ID: any rail that supplies a valid citing message closes the debt. Idempotent; order of arrival irrelevant.
Only free-text NACK → Standardize envelope tags:
ACK <id>, NACK <id> reason:reject|defer|partial|failed <detail>, PROGRESS <id> eta:<duration>.
Ad-hoc correlation → Switch to ULID or UUIDv7 generated at send time. Short human-readable form may be derived but the full ID is authoritative.
No dead-letter / retry contract / idempotency keys → After N escalations mark EXPIRED, write to a dead-letter section of the ledger, and post once to the human/police channel. Message ID is the idempotency key; receiver skips already-terminal IDs.
Fixed constants → Keep the 20/40/60/90 ladder as default; allow an IN-PROGRESS message to extend the remaining SLA (exactly like SQS ChangeMessageVisibility or NATS +WPI). Optional per-task-type SLA table in a one-page YAML.

G. Action Plan (prioritized, simplicity-weighted)

Phase 0 – Immediate (1–2 days, zero new deps)

Change envelope to carry full ULID.
Change ACK convention to mandatory citation of that ID.
Update debt-scanner and watchdog to ignore non-citing messages.
(eliminates weakness 1 and most of 6)

Phase 1 – Single ledger (3–5 days)

Introduce shared ledger.md (append-only blocks keyed by ID).
Both inbox_debt.py and ack_watchdog.py read/write the same file.
Telegram becomes pure notification.
(eliminates 3 and 4)

Phase 2 – Explicit states + NACK/PROGRESS (1 week)

Encode the five-state machine.
Add NACK / PROGRESS parsers.
Implement EXPIRED → dead-letter section + one human alert.
(eliminates 2, 5, 7)

Phase 3 – Soft adaptive timeouts (optional, 2–3 days)

IN-PROGRESS extends remaining SLA.
Tiny human-editable SLA table.
(softens 8)

Phase 4 – Only if scale or pain demands

Evaluate NATS JetStream or Temporal as a drop-in replacement for the dual-rail transport.
Or adopt A2A Task objects if inter-framework interoperability becomes necessary.
(Do not do this while the Markdown design still works.)

All phases remain deterministic Python, human-readable files, and degrade safely (missing rail simply delays the citing ACK; the task stays unpaid until one appears or EXPIRED fires).

H. Sources

Kleppmann, Designing Data-Intensive Applications (2017, still canonical for outbox, dual-write, stream processing).
Hohpe & Woolf, Enterprise Integration Patterns (Correlation Identifier, Dead Letter Channel, Idempotent Receiver).
Amazon SQS Visibility Timeout & Dead-Letter Queues docs (2026). https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html
NATS JetStream ACK model (Ack / Nak / InProgress / Term, AckWait). https://docs.nats.io/using-nats/developer/develop_jetstream/model_deep_dive
RabbitMQ Consumer Acknowledgements & basic.nack. https://www.rabbitmq.com/docs/confirms
Kafka delivery semantics / offset commit. Conduktor & Confluent docs (2026).
Akka Message Delivery Reliability & AtLeastOnceDelivery (2026). https://doc.akka.io/libraries/akka-core/current/general/message-delivery-reliability.html
Google A2A Protocol / Linux Foundation (v1.0 Mar 2026). Task lifecycle. https://github.com/a2aproject/A2A/blob/main/docs/topics/life-of-a-task.md ; a2a-protocol.org
MCP Progress notifications & Tasks extension (2025-11 / 2026-07). modelcontextprotocol.io
Temporal Workflow / Activity / Heartbeat / Retry docs (2026). temporal.io
LangGraph durable execution / checkpointers (2026). docs.langchain.com
Erlang/OTP message-passing notes (2021). https://www.erlang.org/blog/message-passing/
Two Generals’ Problem (standard reference).
Practitioner X threads on agent idempotency failures and inter-agent coordination (Aug 2026, low-authority).
AGNTCY / SLIM (discovery & transport layer, not core ACK). docs.agntcy.org

All non-obvious claims above are backed by at least one (usually two or more) of the sources listed. Confidence tiers are stated inline. Remaining unknowns are mainly operational (exact p95 task durations on the specific fleet) and can be measured after Phase 0–1.

517 sources