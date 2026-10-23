# DR26-08-04-ZB-03-1552 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Improving the ACK and Message-Acknowledgement Protocol of a Multi-Machine Personal AI Fleet

## Executive Summary

The present fleet already has two unusually strong design choices: acknowledgements must cite the originating identifier, and reading a message does not discharge the obligation. Its principal correctness defect is on the sender side: `ack_watchdog.py` treats any later non-heartbeat activity by the target as evidence of acknowledgement. That is not acknowledgement matching; it is liveness detection. Consequently, an unrelated message can close a task that the receiver never accepted. The second structural defect is that the sender JSON ledger and receiver debt reconstruction are independent interpretations of history rather than views of one protocol event log. The third is that the watchdog evaluates only the Telegram rail, even though acknowledgements may legitimately arrive through Syncthing. These weaknesses and the fleet’s “AK-47” repairability constraint are taken directly from the research brief. fileciteturn0file0

**Recommended architecture: ACK-Lite Event Ledger over the existing dual rail.** Keep Telegram and Syncthing. Do not add consensus, Kafka, Temporal, vector clocks, or a broker merely to repair acknowledgement semantics. Replace free-text inference with a small deterministic protocol in which every task has a stable `task_id`; every protocol event has a unique `event_id`; every first response contains both `task_id` and `in_reply_to=<originating-event-id>`; retries preserve the `task_id` and idempotency key but create a new `event_id`; and both rails are merged into one deduplicated logical event set. The source of truth should be append-only, while JSON summaries or SQLite tables remain rebuildable views.

The practical target should be **at-least-once transport plus idempotent execution, yielding an effectively-once external effect**, not a claim of universal exactly-once delivery. Mature systems routinely redeliver when an ACK is lost, a visibility lease expires, a consumer crashes, or a broker fails over. SQS explicitly requires idempotent consumers under its at-least-once model; RabbitMQ automatically requeues unacknowledged deliveries after connection loss; Akka’s reliable-delivery guidance still requires business-level deduplication around side effects. ([AWS SQS at-least-once delivery, undated living documentation; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues-at-least-once-delivery.html); [RabbitMQ consumer acknowledgements, undated living documentation; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/confirms); [Akka message-delivery reliability, undated living documentation; accessed 2026-08-04](https://doc.akka.io/libraries/akka-core/current/general/message-delivery-reliability.html)). citeturn18search9turn20view2turn20view5

The minimal protocol should distinguish four human-facing conditions while retaining more precise machine states:

| Human view | Machine states | Meaning |
|---|---|---|
| **WAITING** | `SENT`, `SEEN` | Task exists; receiver has not yet taken responsibility |
| **OWNED** | `ACCEPTED` plus optional `PROGRESS` events | Receiver explicitly owns the task under a lease |
| **BLOCKED** | `DEFERRED`, `NEEDS_INPUT` | Work cannot proceed now, but the debt has not disappeared |
| **CLOSED** | `DONE`, `REJECTED`, `FAILED`, `CANCELED`, `EXPIRED` | A terminal, explicitly recorded outcome exists |

`SEEN` should be emitted automatically by deterministic receiver bookkeeping after the task is durably imported. It means “this machine’s protocol daemon has the task,” not “the agent will do it.” `ACCEPTED` is the application-level ACK that transfers responsibility. `PROGRESS` should normally be an event and lease renewal, not another required state. `DONE` must be the only successful processing acknowledgement and should include a result, result reference, or explicit no-output reason. This separation follows the same distinction mature messaging systems make between transport receipt, consumer acknowledgement, and business completion. RabbitMQ explicitly distinguishes publisher confirms from consumer acknowledgements; Akka distinguishes arrival at the remote node, placement in a mailbox, and actual processing; Google A2A and experimental MCP Tasks expose explicit long-running task states rather than inferring completion from activity. ([RabbitMQ acknowledgements and publisher confirms, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/confirms); [Akka reliability documentation, undated; accessed 2026-08-04](https://doc.akka.io/libraries/akka-core/current/general/message-delivery-reliability.html); [MCP Tasks specification, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks); [A2A v1.0 release, 2026-03-12](https://github.com/a2aproject/A2A/releases)). citeturn20view2turn20view5turn20view11turn16search0

The highest-return changes are therefore:

1. Remove the `RESPONSIVE` false-ACK rule immediately.
2. Introduce exact task/event correlation fields.
3. Make the watchdog consume the union of both rails.
4. Turn both current ledgers into materialized views of one append-only logical event history.
5. Add explicit `ACCEPTED`, `DEFERRED`, `NEEDS_INPUT`, `REJECTED`, `FAILED`, and `DONE` events.
6. Add bounded retries, idempotency keys, leases, and a human-readable dead-letter file.
7. Add adaptive timing only after the deterministic protocol has accumulated trustworthy observations.

**Overall recommendation: [established].** Adopt the ACK-Lite Event Ledger, not a new broker, assuming task volume remains low, each task normally has one intended executor, and side-effecting operations can be protected by idempotency keys or result checks.

## Key Findings

**[established] An ACK must identify the particular command or delivery it acknowledges.** Mature protocols do this with request IDs, delivery tags, receipt handles, message IDs, sequence numbers, or correlation identifiers. MCP’s JSON-RPC base protocol requires request IDs to be unique within a session and returns the same ID in the response; RabbitMQ acknowledgements refer to channel-scoped delivery tags; SQS deletion uses a receipt handle from the particular delivery; Pub/Sub uses acknowledgement IDs and rejects stale IDs under exactly-once mode. “The target spoke after the task” is not a valid substitute for this binding. ([MCP base protocol, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic); [RabbitMQ acknowledgements, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/confirms); [AWS SQS visibility timeout, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html); [Google Pub/Sub exactly-once delivery, updated 2026-07-17](https://docs.cloud.google.com/pubsub/docs/exactly-once-delivery)). citeturn20view12turn20view2turn20view3turn20view0

**[established] Delivery acknowledgement and processing completion are different facts.** A publisher confirm can show that RabbitMQ accepted a publication without showing that a consumer processed it. A consumer ACK usually allows a broker to delete or advance past a delivery, but the application determines when processing is genuinely complete. Akka’s documentation is unusually explicit: “sent,” “received at the remote endpoint,” “enqueued,” and “processed” are different guarantee levels, and business-level confirmation is needed for the last one. ([RabbitMQ publisher confirms, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/confirms); [Akka message-delivery reliability, undated; accessed 2026-08-04](https://doc.akka.io/libraries/akka-core/current/general/message-delivery-reliability.html)). citeturn20view2turn20view5

**[established] Lost ACKs force a choice between possible duplication and possible loss.** If a receiver performs a side effect and its ACK is lost, a sender cannot generally determine whether execution failed or the response failed. Retrying protects against loss but can repeat the side effect; refusing to retry protects against duplication but can abandon an unexecuted command. This is the operational face of the coordinated-attack/common-knowledge limitation, not a defect that a longer chain of ACK-of-ACK messages eliminates. Lampson’s reliable-message work uses identifiers and duplicate suppression precisely because retransmission remains necessary around failures; modern coordinated-attack analyses continue to formalize the impossibility of guaranteed agreement under sufficiently unconstrained message omission. ([Butler Lampson, “Reliable Messages and Connection Establishment,” 1993](https://www.microsoft.com/en-us/research/publication/reliable-messages-and-connection-establishment/); [Godard and Perdereau, “Back to the Coordinated Attack Problem,” published 2021-07-09](https://www.cambridge.org/core/journals/mathematical-structures-in-computer-science/article/abs/back-to-the-coordinated-attack-problem/55E9BC43CD76ADA013EDECF4DED1A002); [Halpern and Moses, “Knowledge and Common Knowledge in a Distributed Environment,” original work older; arXiv posting 2000-06-02](https://arxiv.org/abs/cs/0006009)). citeturn9search1turn13search11turn22academia49

**[established] “Exactly once” is meaningful only when its boundary is stated.** Google Pub/Sub’s exactly-once guarantee is limited to pull subscriptions and a single region; it still instructs clients to retain processing progress until ACK success. SQS FIFO suppresses duplicate sends only within a five-minute deduplication interval and does not prevent a consumer from processing a message twice after visibility expiry. Kafka Streams can atomically commit consumed offsets, state-store changes, and produced Kafka records, but this is a Kafka-contained transactional boundary, not an automatic guarantee for arbitrary external APIs, files, Telegram posts, or shell commands. ([Google Pub/Sub exactly-once delivery, updated 2026-07-17](https://docs.cloud.google.com/pubsub/docs/exactly-once-delivery); [AWS SQS outage recovery, undated; accessed 2026-08-04](https://docs.aws.amazon.com/en_gb/AWSSimpleQueueService/latest/SQSDeveloperGuide/designing-for-outage-recovery-scenarios.html); [Apache Kafka Streams core concepts, modified 2025-12-19](https://kafka.apache.org/10/streams/core-concepts/); [Hellerstein et al., “Delivery, Consistency, and Determinism,” 2019](https://arxiv.org/abs/1907.06286)). citeturn20view0turn18search2turn20view4turn10view2

**[established] The robust low-complexity pattern is at-least-once redelivery plus deduplication and idempotency.** The receiver records a stable operation identifier before or atomically with the side effect, ignores repeated attempts that represent the same operation, and returns the previously recorded result where possible. AWS’s transactional-outbox guidance explicitly warns that relays can emit duplicates and recommends tracking processed messages; the Idempotent Consumer pattern records message IDs under a uniqueness constraint; Akka calls the result “effectively-once” when receiver confirmation and duplicate suppression are combined, while warning that externally visible business side effects still need their own deduplication. ([AWS Transactional Outbox Pattern, undated; accessed 2026-08-04](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html); [Chris Richardson, Idempotent Consumer pattern, undated; accessed 2026-08-04](https://microservices.io/patterns/communication-style/idempotent-consumer.html); [Akka reliable delivery, undated; accessed 2026-08-04](https://doc.akka.io/libraries/akka-core/current/typed/reliable-delivery.html)). citeturn14search0turn13search2turn2search2

**[established] One logical ledger is preferable to two unreconciled mutable ledgers.** This does not require one central database. A simple event-sourced design can use one append-only file per authoring machine, merge all events by `event_id`, and reconstruct sender and receiver views from the same history. This is analogous to transactional outbox/event-log patterns: durable intent is recorded first; delivery is retryable; derived status is rebuilt rather than independently edited. Enterprise Integration Patterns separately identifies correlation identifiers, message stores, idempotent receivers, dead-letter channels, and messaging bridges—the same components needed here without importing a heavyweight integration platform. ([AWS Transactional Outbox Pattern, undated; accessed 2026-08-04](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html); [Enterprise Integration Patterns site, book published 2003-10-10; site continuously maintained](https://www.enterpriseintegrationpatterns.com/patterns/messaging/); [Martin Kleppmann, *Designing Data-Intensive Applications*, published March 2017](https://dataintensive.net/)). citeturn14search0turn22search13turn22search3

**[established] Redundant rails should carry duplicate copies of the same protocol event, not independent semantic events.** The same ACK published to Telegram and Syncthing should have the same `event_id` and canonical payload. The importer unions events from both rails, deduplicates them, and advances the task once. A second publication with a new ID would look like a second business event and complicate reconciliation. Messaging-bridge and idempotent-receiver patterns support this separation between transport copies and logical messages. ([Enterprise Integration Patterns, Messaging Bridge and Idempotent Receiver, book published 2003-10-10](https://www.enterpriseintegrationpatterns.com/patterns/messaging/); [AWS SQS message deduplication guidance, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/best-practices-message-deduplication.html)). citeturn22search13turn18search6

**[established] Visibility timeouts and leases should be renewable but bounded.** SQS recommends starting with a processing-appropriate visibility timeout and extending it using heartbeats for variable-duration work; too short a timeout causes duplicate processing, while too long a timeout delays recovery. MCP Tasks similarly supports polling and durable task states, and the MCP lifecycle guidance permits progress to reset a per-request timeout while recommending an absolute upper bound. This maps naturally to `ACCEPTED lease_until=...`, periodic `PROGRESS` renewals, and a separate hard `due_by`. ([AWS SQS visibility timeout, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html); [MCP Tasks, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks); [MCP lifecycle, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle)). citeturn18search11turn20view11turn5search7

**[emerging] Agent interoperability standards are converging on explicit task lifecycles, but not yet on universal processing-proof semantics.** A2A v1.0 was released on 2026-03-12 and v1.0.1 on 2026-05-26; A2A models long-running tasks and status transitions. MCP added experimental Tasks in its 2025-11-25 specification, with `working`, `input_required`, `completed`, `failed`, and `cancelled` states. These standards improve correlation and state visibility, but completion remains an assertion by the executing system; neither automatically proves that an external real-world side effect occurred exactly once. The July 2026 implementation-grounded comparison of MCP and A2A found A2A richer for stateful task coordination but more complex, while MCP required lifecycle handling at the application layer. ([A2A releases, 2026-03-12 and 2026-05-26](https://github.com/a2aproject/A2A/releases); [MCP Tasks, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks); [Predoaia et al., “A Comparative Study of MCP and A2A,” 2026-07-26](https://arxiv.org/abs/2607.23884)). citeturn16search0turn20view11turn15academia45

**[emerging] Durable execution is becoming the preferred substrate for long-running agents, but most agent frameworks still do not provide a cross-machine application ACK protocol.** LangGraph emphasizes checkpoints, persistence, resumability, and interrupts; OpenAI’s Agents SDK supports in-run handoffs and documents integrations with durable-execution systems; CrewAI advertises persistent, resumable flows; AutoGen direct messaging returns a response or error, while publish-style messaging does not return per-subscriber completion; AG2 provides structured handoff and escalation routing. These solve orchestration or recovery inside a runtime. They do not remove the need for explicit task IDs, idempotency keys, and business completion events across a lossy Telegram/file bus. [single-source for each framework-specific statement: official documentation] ([LangGraph v1, undated; accessed 2026-08-04](https://docs.langchain.com/oss/javascript/releases/langgraph-v1); [OpenAI Agents SDK handoffs, undated; accessed 2026-08-04](https://openai.github.io/openai-agents-python/handoffs/); [OpenAI Agents SDK running agents, undated; accessed 2026-08-04](https://openai.github.io/openai-agents-python/running_agents/); [CrewAI documentation, undated; accessed 2026-08-04](https://docs.crewai.com/); [AutoGen messaging, undated; accessed 2026-08-04](https://microsoft.github.io/autogen/stable/user-guide/core-user-guide/framework/message-and-communication.html); [AG2 handoffs, published 2026-07](https://docs.ag2.ai/latest/docs/user-guide/advanced-concepts/orchestration/group-chat/handoffs/)). citeturn20view8turn20view9turn20view10turn21search4turn20view13turn15search8

### Comparison of Named Systems

| System | Delivery and acknowledgement model | Processing/failure model | Relevance to the fleet |
|---|---|---|---|
| **Amazon SQS Standard** | At-least-once; receipt hides a message for a visibility timeout; successful processing is followed by deletion. Expiry permits redelivery. | Visibility can be extended; repeated failures can be routed to a DLQ. Duplicate delivery remains possible. | Direct model for `ACCEPTED lease_until`, heartbeat renewal, bounded attempts, and dead-lettering. ([AWS, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html)) citeturn20view3 |
| **Amazon SQS FIFO** | Producer deduplication uses `MessageDeduplicationId` for a five-minute window. | Visibility expiry can still cause duplicate processing by consumers. | Shows why transport deduplication is not enough; retain application idempotency indefinitely or for the task’s meaningful lifetime. ([AWS, undated; accessed 2026-08-04](https://docs.aws.amazon.com/en_gb/AWSSimpleQueueService/latest/SQSDeveloperGuide/designing-for-outage-recovery-scenarios.html)) citeturn18search2 |
| **Google Pub/Sub** | Optional exactly-once ACK confirmation for pull subscriptions in one region; stale ACK IDs fail. | Processing progress must be retained until ACK success; acknowledgement deadlines can cause valid redelivery. | Strong precedent for ACK success being an observable result and for rejecting stale acknowledgements. ([Google, updated 2026-07-17](https://docs.cloud.google.com/pubsub/docs/exactly-once-delivery)) citeturn20view0 |
| **RabbitMQ** | Separate publisher confirms and consumer ACK/NACK; delivery tags bind ACKs to deliveries. | NACK can requeue or dead-letter; unacknowledged messages are requeued on connection failure. Requeue loops are explicitly warned against. | Best direct precedent for separating “bus accepted publication” from “receiver processed task,” and for typed NACKs. ([RabbitMQ, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/confirms)) citeturn20view2 |
| **NATS JetStream** | Explicit ACK modes with `AckWait`; absence of ACK causes redelivery. | `MaxDeliver`, delayed NAK, and configured backoff bound retries. | Good model for attempt counters and per-task backoff without adopting NATS itself. ([NATS, undated; accessed 2026-08-04](https://docs.nats.io/learn/jetstream/pull-consumers)) citeturn20view1 |
| **Kafka producer and Streams** | Idempotent producers suppress retry duplicates; transactions can atomically update Kafka offsets, state stores, and Kafka outputs. | Default Streams processing is at-least-once unless exactly-once processing is selected. External side effects remain outside the Kafka transaction unless separately coordinated. | Demonstrates scoped exactly-once and why a Telegram/file task cannot inherit it. ([Apache Kafka, modified 2025-12-19](https://kafka.apache.org/10/streams/core-concepts/)) citeturn20view4 |
| **Akka** | Core actor messaging is at-most-once. Reliable-delivery modules add resend, confirmation, and deduplication. | Application/business ACK and side-effect deduplication remain necessary; durable producer storage is needed across crashes. | Strong conceptual match: exact IDs, resend, receiver confirmation, duplicate suppression. ([Akka, undated; accessed 2026-08-04](https://doc.akka.io/libraries/akka-core/current/general/message-delivery-reliability.html)) citeturn20view5 |
| **Orleans** | Default at-most-once request delivery; enabling retries produces at-least-once behavior. | Durable duplicate suppression is not provided by default because of its cost. One-way calls provide no completion signal. | Supports rejecting fire-and-forget for important tasks and making retry/idempotency explicit. ([Microsoft Orleans, updated 2025-05-29](https://learn.microsoft.com/en-us/dotnet/orleans/implementation/messaging-delivery-guarantees)) citeturn20view6 |
| **Erlang/OTP** | Asynchronous signals preserve per-sender ordering to a destination, but distributed messages may be lost when a connection fails. | Monitors report process termination; supervisors restart components under bounded intensity rules, not business tasks. | Useful for “let it crash” and bounded recovery, but process liveness must not be confused with task ACK. ([Erlang/OTP 29 documentation, undated versioned docs; accessed 2026-08-04](https://www.erlang.org/doc/system/ref_man_processes.html)) citeturn20view7 |
| **Temporal** | Workflow history provides durable execution; Activities have retry and timeout concepts including heartbeat-related failure detection. | Workflow code can resume after worker failure; side-effecting Activities are expected to be retry-safe. | Appropriate when workflows become numerous and multi-step; excessive for the present ACK bookkeeping. [single-source: official API documentation] ([Temporal API documentation, undated; accessed 2026-08-04](https://api-docs.temporal.io/)) citeturn8search3 |
| **Airflow** | Tasks have persisted scheduler states such as queued, running, failed, retrying, deferred, and success. | Retries, deferral, and heartbeat/zombie detection are orchestration concerns. | Useful state vocabulary, but Airflow is batch orchestration rather than peer-to-peer acknowledgement. [single-source: official docs] ([Airflow 3.3.0 docs, versioned page; accessed 2026-08-04](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/tasks.html)) citeturn20view14 |
| **n8n** | Workflow executions are recorded as Running, Success, Failed, or Waiting and failed executions can be manually retried. | Retry can use the current or original workflow definition and prior data. | Good human repair UI precedent, weak as a protocol standard. [single-source: official docs] ([n8n, undated; accessed 2026-08-04](https://docs.n8n.io/workflows/executions/all-executions/)) citeturn21search0 |
| **LangGraph** | Durable execution is based on persistence/checkpoints rather than message ACKs. | Paused or interrupted graph execution can resume from saved state. | Good local agent recovery substrate; still needs the fleet protocol at machine boundaries. [single-source: official docs] ([LangGraph v1, undated; accessed 2026-08-04](https://docs.langchain.com/oss/javascript/releases/langgraph-v1)) citeturn20view8 |
| **OpenAI Agents SDK / Swarm** | Agents SDK handoffs transfer control within a run; Swarm was published as an educational lightweight orchestration framework. | Durable execution is delegated to integrations rather than defined as a network acknowledgement protocol. | Handoff is not acceptance of an independently durable fleet task. [single-source per claim: official repositories/docs] ([OpenAI Agents SDK, undated; accessed 2026-08-04](https://openai.github.io/openai-agents-python/handoffs/); [OpenAI Swarm, repository documentation; accessed 2026-08-04](https://github.com/openai/swarm)) citeturn20view9turn14search4 |
| **AutoGen / AG2 / CrewAI** | These frameworks expose sends, conversations, crews, flows, routing, or handoffs rather than a shared universal durable ACK ledger. | Some support persisted state, callbacks, guardrails, or escalation patterns, but cross-host processing confirmation is application-defined. | Borrow structured handoffs and state persistence, not their conversational activity as an ACK. [single-source per framework: official docs] ([AutoGen, undated](https://microsoft.github.io/autogen/stable/user-guide/core-user-guide/framework/message-and-communication.html); [AG2, published 2026-07](https://docs.ag2.ai/latest/docs/user-guide/advanced-concepts/orchestration/group-chat/handoffs/); [CrewAI, undated](https://docs.crewai.com/)) citeturn20view13turn15search8turn21search4 |
| **MCP Tasks** | Requests retain JSON-RPC IDs; experimental Tasks add a receiver-generated task ID, polling, TTL, status notifications, and terminal states. | States include working, input-required, completed, failed, and cancelled. | Valuable schema precedent, but broader than necessary and still experimental. [single-source: official specification] ([MCP, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks)) citeturn20view11 |
| **A2A v1.0** | Explicit task IDs, messages, status updates, polling/streaming/push models, and long-running task lifecycle. | Submitted/working/input-required and terminal outcomes replace free-text conversational inference. | Closest emerging agent-level reference model; use a reduced subset rather than full protocol adoption. [single-source for current release details: official project] ([A2A releases, 2026-03-12 and 2026-05-26](https://github.com/a2aproject/A2A/releases)) citeturn16search0 |
| **AGNTCY / SLIM** | Emerging secure messaging infrastructure intended beneath protocols such as A2A and MCP. | Focuses on transport, identity, secure groups, streaming, and observability rather than business-task completion. | Relevant to a future transport migration, not a replacement for ACK semantics. [single-source: IETF Internet-Draft, not an RFC] ([IETF draft, published 2026-02-24](https://www.ietf.org/archive/id/draft-mpsb-agntcy-slim-01.html)) citeturn17search7 |

No authoritative comparative study was found that documents a widely adopted ACK protocol specifically among four-to-six-machine personal Claude Code fleets. Public DIY and personal-agent projects tend to document routing, prompting, handoffs, or runtime deployment rather than durable processing acknowledgement. **[speculative] [no source found]** No recommendation in this report depends on a claimed DIY consensus.

## Contradictions & Debates

### Exactly-once guarantees versus effectively-once effects

The strongest pro-exactly-once position is that scoped exactly-once features materially simplify applications. Google Pub/Sub can confirm ACK success and prevent redelivery after that successful ACK within its supported topology; Kafka transactions can atomically bind input offsets, internal state, and outputs; SQS FIFO suppresses duplicate publication during its deduplication window. It would be wrong to dismiss these capabilities as mere marketing: within their specified boundaries, they eliminate real classes of duplicate delivery. ([Google Pub/Sub, updated 2026-07-17](https://docs.cloud.google.com/pubsub/docs/exactly-once-delivery); [Apache Kafka, modified 2025-12-19](https://kafka.apache.org/10/streams/core-concepts/); [AWS SQS FIFO, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues-exactly-once-processing.html)). citeturn20view0turn20view4turn17search1

The opposing position is that “exactly once” often hides its transaction boundary. Once a handler also edits a file, invokes a shell command, sends a Telegram message, charges an account, or changes another service, the broker cannot atomically prove and deduplicate that external effect. The 2019 analysis by Hellerstein and colleagues argues that delivery semantics alone are insufficient unless consistency and recovery ordering are also defined. This position is more applicable to the fleet because agent tasks frequently produce heterogeneous side effects outside one transactional platform. ([Hellerstein et al., 2019](https://arxiv.org/abs/1907.06286); [AWS Transactional Outbox, undated; accessed 2026-08-04](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html)). citeturn10view2turn14search0

**Reconciliation:** use the phrase **effectively-once effect**, and define its boundary. The fleet may guarantee that a given `idempotency_key` is applied no more than once by a compliant handler whose durable deduplication record covers the side effect. It should not claim that each task is physically delivered or executed only once.

### How many states are actually necessary?

A rich state machine such as A2A’s is valuable for interoperability and long-running human-in-the-loop work. `input-required`, `auth-required`, `rejected`, and `canceled` are not cosmetic distinctions: they tell an orchestrator whether to wait, supply information, retry, or stop. MCP Tasks similarly demonstrates that long-running agent work benefits from durable status retrieval rather than an open connection. ([MCP Tasks, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks); [A2A v1.0 release, 2026-03-12](https://github.com/a2aproject/A2A/releases)). citeturn20view11turn16search0

The simplicity argument is that every state creates transitions, invalid combinations, timeout rules, operator vocabulary, and tests. A small personal fleet does not need all enterprise states. In particular, `IN_PROGRESS` adds little if `ACCEPTED` already carries a renewable lease and `PROGRESS` is an event. Similarly, “delivered” is dangerous unless it is defined as receiver-generated durable observation; Telegram API success is not proof that the destination machine imported the task.

**Reconciliation:** keep a precise internal protocol but collapse it into four operator buckets. This preserves correctness without making the human dashboard resemble an enterprise workflow engine.

### Central single ledger versus replicated logical ledger

A single SQLite database on the VPS would make queries, uniqueness constraints, and state reconstruction easy. It would also become the authority whose outage prevents new authoritative state transitions. That may be acceptable when the VPS is substantially more reliable than all peers, but it weakens the requirement that a missing rail or offline machine must not silently lose tasks.

A replicated append-only event log requires merge and deduplication logic, but it avoids multi-writer mutable-state conflicts. Each node can write only its own `events-<node>.ndjson`; Syncthing replicates those files; Telegram provides a second carrier; every machine reconstructs the same logical state from the union. This resembles event sourcing and transactional-outbox thinking without requiring a shared database transaction. ([AWS Transactional Outbox, undated; accessed 2026-08-04](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html); [Kleppmann, *Designing Data-Intensive Applications*, March 2017](https://dataintensive.net/)). citeturn14search0turn22search3

**Reconciliation:** use one logical ledger, physically replicated as per-node append-only files. Allow a VPS SQLite index as a convenience cache, never as the only authoritative copy.

### Explicit causality versus Lamport or vector clocks

Lamport clocks provide a total ordering consistent with the happened-before relation when processes exchange clock values, but a total order does not by itself prove semantic causality. Vector clocks reveal concurrency more precisely but add one counter per participant and require merge rules. Lamport’s original 1978 paper makes the key distinction between causal partial order and an imposed total order. ([Leslie Lamport, “Time, Clocks, and the Ordering of Events in a Distributed System,” published July 1978](https://www.microsoft.com/en-us/research/publication/time-clocks-ordering-events-distributed-system/)). citeturn9search2

For this fleet, nearly every important relation can be represented directly: `in_reply_to`, `causation_id`, `task_id`, and `attempt`. These fields answer the operational questions—what task was accepted, which event caused the retry, and what result closed the debt—without requiring global ordering.

**Reconciliation:** do not introduce vector clocks. Optionally store a per-node monotonic sequence or Lamport counter for forensic display, but never use it instead of explicit references. Use wall-clock timestamps for SLA calculations only, not to decide causality.

### Fixed versus adaptive timeouts

Fixed thresholds are legible, predictable, and easy to repair. They are appropriate while sample sizes are small or tasks are heterogeneous. Their weakness is obvious: a laptop known to be offline overnight is treated like a responsive VPS, and a five-minute command is treated like a six-hour research task.

Visibility-timeout systems favor renewable leases precisely because processing time varies. SQS recommends dynamic extensions, while MCP supports task polling and explicit status updates. Fully statistical deadlines, however, can adapt in the wrong direction: a degraded machine can train the system to tolerate progressively worse response times. ([AWS SQS visibility timeout, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html); [MCP Tasks, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks)). citeturn18search11turn20view11

**Reconciliation:** begin with named SLA classes and receiver-proposed `retry_after`/`lease_until`. Add bounded statistical adjustment later. Never let adaptation exceed an operator-visible hard maximum.

### Hand-rolled ACK layer versus a real broker

NATS JetStream, RabbitMQ, SQS, and Pub/Sub already provide durable storage, delivery tracking, redelivery, backoff, and dead-letter mechanisms. A broker becomes compelling when message volume grows, many consumers compete for work, strict retention is needed, or maintaining the custom importer becomes more complex than operating the broker. ([NATS JetStream consumer docs, undated; accessed 2026-08-04](https://docs.nats.io/learn/jetstream/pull-consumers); [RabbitMQ acknowledgements, undated](https://www.rabbitmq.com/docs/3.13/confirms); [AWS SQS visibility timeout, undated](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html); [Google Pub/Sub exactly-once, updated 2026-07-17](https://docs.cloud.google.com/pubsub/docs/exactly-once-delivery)). citeturn20view1turn20view2turn20view3turn20view0

The strongest argument against migration is that the operator already understands Telegram and files, whereas broker credentials, networking, retention policies, stream configuration, and recovery procedures create a new failure domain. Moreover, a broker still cannot decide whether an LLM-produced result is semantically complete or whether a shell-side effect occurred safely.

**Reconciliation:** repair the application protocol now. Re-evaluate NATS JetStream only when operational evidence shows that transport retention and redelivery—not ACK semantics—have become the dominant problem.

## Best Practices

### Canonical Event Envelope

Every protocol-relevant line should contain a human-readable prefix followed by deterministic structured data. The parser must trust the structured fields, not emoji, prose, word order, or distinctive dashed tags.

```text
🤖 [hub -> macbook] TASK
{"v":1,
 "event_id":"0198f4aa-4e60-7c11-9d8e-64ed7bfa7f1c",
 "task_id":"0198f4aa-4dfe-75c4-b725-1a109bc6b37d",
 "kind":"TASK",
 "src":"hub",
 "dst":"macbook",
 "created_at":"2026-08-04T09:10:00Z",
 "attempt":1,
 "idempotency_key":"repo:alpha:review:commit-a81d3f",
 "ack_by":"2026-08-04T09:30:00Z",
 "due_by":"2026-08-04T13:00:00Z",
 "body":"Review commit a81d3f and report blocking issues."}
```

UUIDv7 is a suitable default for `task_id` and `event_id`: it is a standardized 128-bit UUID whose high-order bits encode Unix-epoch milliseconds, giving roughly time-sortable identifiers without requiring a central sequence allocator. Full IDs should be stored and transmitted; a short suffix such as `…b37d` may be displayed to humans but must never be the matching key. [single-source: IETF standard] ([RFC 9562, published May 2024](https://www.rfc-editor.org/rfc/rfc9562.html)). citeturn13search9

The fields have distinct meanings:

| Field | Invariant |
|---|---|
| `task_id` | Stable identity of the logical obligation across delivery and execution retries |
| `event_id` | Unique identity of this exact protocol event |
| `in_reply_to` | Exact event being acknowledged or answered |
| `causation_id` | Event that caused this event when different from the original command |
| `idempotency_key` | Identity of the externally visible operation that must not be repeated |
| `attempt` | Execution attempt number; increments only when a new attempt is authorized |
| `src`, `dst` | Protocol identities, not inferred from prose |
| `kind` | Enumerated event type |
| `ack_by`, `due_by` | Acceptance and completion service-level deadlines |
| `lease_until` | Receiver’s current ownership lease |
| `retry_after` | Earliest sensible retry or reconsideration time |
| `reason_code` | Machine-readable failure/blocking category |
| `result_ref` | Path, commit, URL, hash, or concise result that substantiates `DONE` |

A task retry and a transport replay are not the same operation. Republishing the same event on another rail preserves the same `event_id`. Starting a new execution attempt creates a new `event_id`, retains the same `task_id` and `idempotency_key`, and increments `attempt`. This distinction follows producer-deduplication and idempotent-consumer practice: copies of the same message are deduplicated by message identity, while repeated attempts at one business operation are constrained by a stable business idempotency key. ([AWS SQS deduplication guidance, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html); [Idempotent Consumer pattern, undated](https://microservices.io/patterns/communication-style/idempotent-consumer.html)). citeturn18search1turn13search2

### Exact ACK-Matching Rule

A first acknowledgement is valid only when all of the following are true:

```text
event.kind ∈ {SEEN, ACCEPTED, DEFERRED, NEEDS_INPUT, REJECTED}
event.task_id == pending.task_id
event.in_reply_to == pending.origin_event_id
event.src == pending.expected_receiver
event.dst ∈ {pending.sender, "*"}
event.v is supported
event.event_id has not already been applied
```

An unrelated message from the target changes only the target’s liveness observation. It must never alter task state. A heartbeat proves that a machine or daemon was recently alive; it does not prove that a particular message reached its task parser. A `SEEN` event proves durable import; it does not prove acceptance. An `ACCEPTED` event proves ownership; it does not prove completion. A `DONE` event proves that the receiver asserts completion under the protocol; for consequential side effects, the sender may still validate `result_ref`.

### Minimal State Machine

The recommended machine state is deliberately smaller than A2A or a workflow engine, but it preserves the critical delivery/processing distinctions.

| State | Entered by | Required fields | Allowed next states |
|---|---|---|---|
| `SENT` | Sender durably appends `TASK` | `task_id`, `event_id`, `idempotency_key`, deadlines | `SEEN`, `ACCEPTED`, `DEFERRED`, `NEEDS_INPUT`, `REJECTED`, `CANCELED`, `EXPIRED` |
| `SEEN` | Receiver daemon durably imports the task | `in_reply_to`, receiver identity | `ACCEPTED`, `DEFERRED`, `NEEDS_INPUT`, `REJECTED`, `CANCELED`, `EXPIRED` |
| `ACCEPTED` | Agent or deterministic dispatcher assumes responsibility | `lease_until` | `ACCEPTED` via lease renewal, `DEFERRED`, `NEEDS_INPUT`, `DONE`, `FAILED`, `CANCELED`, `EXPIRED` |
| `DEFERRED` | Receiver cannot start or continue yet | `retry_after`, `reason_code` | `ACCEPTED`, `NEEDS_INPUT`, `REJECTED`, `FAILED`, `CANCELED`, `EXPIRED` |
| `NEEDS_INPUT` | Receiver requires information or authority | question or `reason_code` | `ACCEPTED`, `DEFERRED`, `REJECTED`, `FAILED`, `CANCELED`, `EXPIRED` |
| `DONE` | Receiver reports successful completion | `result_ref` or explicit `no_result_reason` | None for that attempt |
| `FAILED` | Accepted execution ended unsuccessfully | `reason_code`, `retryable` | None for that attempt; sender may create a new attempt |
| `REJECTED` | Receiver refuses before taking ownership | `reason_code`, normally `retryable=false` | None for that attempt |
| `CANCELED` | Authorized sender/operator cancels | `in_reply_to`, reason | None for that attempt |
| `EXPIRED` | Deterministic timeout policy closes stale work | deadline and policy | None for that attempt |

`PROGRESS` is a nonterminal event that updates `last_progress_at`, `lease_until`, optional percentage, and a note. It need not introduce an additional persistent state. This avoids the ambiguity of deciding whether `ACCEPTED` and `IN_PROGRESS` differ when autonomous agents may queue, plan, or pause internally.

Each attempt’s terminal event should be immutable. A retry creates a new attempt:

```text
TASK attempt=1
  -> ACCEPTED
  -> FAILED retryable=true

RETRY attempt=2, same task_id, same idempotency_key, new event_id
  -> ACCEPTED
  -> DONE
```

This makes the audit history truthful: the first attempt did fail; it is not overwritten by later success. Sagas use a comparable principle at larger scope: retain the sequence of completed steps and apply explicit compensation rather than pretending the partial execution never happened. The original Saga paper is old—1987—but remains the canonical source. ([Garcia-Molina and Salem, “Sagas,” published 1987](https://dl.acm.org/doi/10.1145/38713.38742)). citeturn9search0

### Negative Acknowledgement Vocabulary

Free-text explanations remain useful for the human, but protocol behavior must be driven by a small code set.

| Event | Meaning | Default sender behavior |
|---|---|---|
| `REJECTED` | Receiver will not perform the task as addressed | Do not automatically retry same receiver; reroute or escalate |
| `DEFERRED` | Receiver recognizes the task but cannot own it now | Wait until `retry_after`; debt remains open |
| `NEEDS_INPUT` | Progress is blocked on information or authorization | Notify sender/human; suspend retry ladder |
| `FAILED retryable=true` | Execution began and failed transiently | Retry under bounded policy using same idempotency key |
| `FAILED retryable=false` | Execution failed permanently or requires redesign | Dead-letter and escalate |
| `PROGRESS` | Work continues; lease renewed | Suppress silence alert until lease approaches expiry |
| `CANCELED` | Sender/operator withdraws task | Receiver must stop when safe and report any partial effects |
| `DONE` | Work completed | Validate result reference where required; close debt |

RabbitMQ/AMQP provides a useful semantic precedent: accepted means successful processing; rejected means unprocessable; released/requeued means the consumer did not process the message and another attempt may occur. RabbitMQ also warns that immediate requeue by all consumers can form a resource-intensive redelivery loop. ([RabbitMQ AMQP mapping, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/amqp); [RabbitMQ ACK/NACK guidance, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/confirms)). citeturn13search13turn13search1

Suggested initial `reason_code` values should remain deliberately small:

```text
BUSY
OFFLINE_PLANNED
NEEDS_CONTEXT
NEEDS_AUTH
UNSUPPORTED
INVALID_TASK
DEPENDENCY_DOWN
TRANSIENT_TOOL_ERROR
PERMANENT_TOOL_ERROR
DUPLICATE
CANCELED_BY_OPERATOR
DEADLINE_EXCEEDED
UNKNOWN
```

Do not make error codes encode prose or every possible tool exception. The purpose is deterministic policy: retry, wait, reroute, or stop.

### One Logical Ledger, Replicated Append-Only

Recommended on-disk layout:

```text
fleet-ledger/
  events-hub.ndjson
  events-vps.ndjson
  events-mac1.ndjson
  events-laptop1.ndjson
  state-cache.json
  dead-letter.md
  operator-status.md
```

Each machine writes only its own `events-<node>.ndjson`. This prevents Syncthing from having to merge concurrent writes to one file. Each line is a canonical JSON event. Telegram messages are imported into the same logical event set. `state-cache.json`, `operator-status.md`, and an optional SQLite index are derived artifacts that may be deleted and rebuilt.

The reducer should be deterministic:

```python
events = union(file_rail_events, telegram_rail_events)
events = deduplicate_by_event_id(events)
events = validate_schema_and_identity(events)
events = group_by_task_id(events)
state = reduce_each_task_by_valid_transitions(events)
```

When duplicate `event_id` values have byte-equivalent canonical content, keep one. When the same `event_id` appears with different canonical content, create a visible integrity conflict; do not silently select the newer timestamp. When an event references an unknown task, retain it in an orphan set until the originating event arrives. When two incompatible terminal events appear, mark `CONFLICT` in the operator view and require repair. This is safer than “last writer wins,” especially when clocks may differ.

A transactional outbox traditionally makes the business state change and outgoing message intent part of one local transaction. The fleet cannot atomically transact across arbitrary Markdown, Telegram, Git, and shell side effects, but it can follow the core discipline: persist the local protocol event before attempting unreliable publication; retry publication later; and never infer that a successful local write means remote receipt. ([AWS Transactional Outbox, undated; accessed 2026-08-04](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html)). citeturn14search0

### Cross-Rail ACK Fusion

Each emitted protocol event should be canonicalized once and offered to both rails:

```text
create event E
append E to local events-src.ndjson
publish serialized E to Telegram
allow Syncthing to replicate local event file
record rail publication observations separately
```

The receiver may discover `E` from either rail. It emits one logical `SEEN` event `S`, which can also be copied to both rails with the same `event_id=S`. The sender closes the “not seen” condition as soon as `S` is present in the merged event set, regardless of which rail supplied it.

Rail status must not be task status:

```json
{
  "event_id": "...",
  "observed_on": {
    "file": "2026-08-04T09:10:07Z",
    "telegram": "2026-08-04T09:10:11Z"
  }
}
```

These observations diagnose rail health but do not create two acknowledgements. A periodic reconciler may republish an event missing from one rail, always preserving the same `event_id`. A rail outage therefore increases redundancy debt but cannot silently close or erase task debt.

The watchdog should query:

```text
logical_events = FILE_EVENTS ∪ TELEGRAM_EVENTS
```

It must not query only “last 80 Telegram messages.” A bounded Telegram window can remain an importer optimization, but the local append-only event set is the durable query source. Telegram message identifiers may be retained as transport metadata, not as the sole business identifier.

### Idempotent Execution

The receiver needs a durable operation registry for side-effecting tasks:

```text
idempotency_key | task_id | status | result_ref | first_started | completed_at
```

Before executing:

1. If the key is `DONE`, return the stored result through a new `DONE` protocol event without repeating the side effect.
2. If the key is `RUNNING` under a valid local lease, report `PROGRESS` or `DUPLICATE_IN_FLIGHT`.
3. If the key is absent, reserve it durably, execute, store the result, and then emit `DONE`.
4. If execution fails, record whether retry is safe and emit `FAILED`.

For local filesystem work, an idempotency key can be tied to the intended target state: repository, operation, branch, commit, or output path. “Ensure commit X has review file Y” is safer than “append another review.” For API calls that support an idempotency header, forward the same key. For inherently non-idempotent effects without deduplication support, require a preflight check or human confirmation before retry.

The difficult crash window is between performing the side effect and recording completion. No generic ACK layer can eliminate it for arbitrary external systems. The handler must either perform the effect and idempotency record in one transaction, use a provider’s idempotency mechanism, query the external state on recovery, or classify the outcome as `UNKNOWN_EFFECT` and escalate instead of blindly retrying. This is the exact dual-write problem addressed by transactional-outbox and idempotent-consumer patterns. ([AWS Transactional Outbox, undated](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html); [Idempotent Consumer, undated](https://microservices.io/patterns/communication-style/idempotent-consumer.html)). citeturn14search0turn13search2

### Retry, Backoff, Dead Letter, and Human Repair

Retries should be classified by failure type rather than triggered merely by age:

| Class | Examples | Policy |
|---|---|---|
| No receipt | No `SEEN` from receiver | Republish same event on missing rail; do not increment execution attempt |
| No ownership | `SEEN` but no `ACCEPTED`/`DEFERRED` | Targeted reminder; then operator escalation |
| Temporary refusal | `DEFERRED`, `DEPENDENCY_DOWN`, `BUSY` | Wait until `retry_after`; capped backoff |
| Retryable execution failure | Tool timeout, transient dependency error | New attempt, same task/idempotency key |
| Permanent failure | Invalid task, unsupported capability, permission design error | No automatic retry; dead-letter |
| Ambiguous side effect | Timeout after potentially irreversible action | Reconcile external state or human review; never blind retry |
| Poison task | Same deterministic failure across attempts | Quarantine after `max_attempts` |

NATS JetStream supports separate `AckWait`, `MaxDeliver`, and backoff configuration; RabbitMQ recommends tracking redeliveries or scheduling delayed retries rather than forming an immediate requeue loop; SQS moves messages beyond a configured receive count to a DLQ. These systems converge on bounded retries and quarantine rather than indefinite circulation. ([NATS JetStream consumers, undated](https://docs.nats.io/learn/jetstream/pull-consumers); [RabbitMQ acknowledgements, undated](https://www.rabbitmq.com/docs/3.13/confirms); [AWS SQS DLQs, undated; accessed 2026-08-04](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)). citeturn20view1turn13search1turn1search7

For the human operator, `dead-letter.md` should be a generated repair document, not a raw stack-trace dump:

```text
# DEAD LETTER

Task: …b37d
From: hub
To: macbook
Status: FAILED / PERMANENT_TOOL_ERROR
Attempts: 3
Last event: 2026-08-04 12:17 UTC
Task: Review commit a81d3f
Last error: repository not found
Result/effect uncertainty: none

Allowed repairs:
RETRY task=…b37d
FORWARD task=…b37d dst=vps
CANCEL task=…b37d reason=obsolete
MARK-DONE task=…b37d result_ref=/path/to/result
```

Manual repair commands must themselves become append-only protocol events, preserving who overrode what and why. `MARK-DONE` should not edit an old event or delete a debt record.

### SLA-Aware Deadlines

Use distinct clocks:

```text
ack_by       receiver must ACCEPT, DEFER, NEED_INPUT, or REJECT
due_by       absolute expected completion deadline
lease_until  accepted ownership is considered live until this time
retry_after  receiver-requested earliest reconsideration time
```

`SEEN` should normally be automatic and fast; failure to see a task is primarily a bus/importer health issue. `ack_by` is the agent-level response deadline. `lease_until` is renewable by `PROGRESS`. `due_by` is a hard business deadline and should not be silently extended by progress.

Proposed starting profiles—these are engineering defaults, not externally validated constants—are:

| SLA class | `ack_by` | Initial lease | Typical use |
|---|---:|---:|---|
| `URGENT` | 5 minutes | 15 minutes | Incident response or explicit human waiting |
| `NORMAL` | 20 minutes | 60 minutes | Ordinary machine-to-machine task |
| `BACKGROUND` | 2 hours | 6 hours | Research, maintenance, overnight work |
| `OFFLINE_OK` | Next known online window | 6 hours after acceptance | Laptops that may be absent for long periods |

The operator should see the class and absolute timestamps, not only elapsed-minute thresholds. If a machine is offline, the task remains open and is labelled `WAITING — PEER OFFLINE`; it is not treated as acknowledged. Escalation should distinguish “offline but safely persisted on both rails” from “online and ignoring an observed task.”

After enough completed tasks exist, a bounded adaptive estimate can be introduced:

```text
candidate_ack_timeout =
    clamp(min_timeout,
          percentile_95(recent_ack_latency_for_machine_and_class) * safety_factor,
          max_timeout)
```

**[speculative]** A minimum of roughly dozens to a hundred comparable observations per machine/class is advisable before trusting such adaptation; no source was found establishing a universal sample-size threshold for this personal-fleet scenario. The hard maximum and human-facing SLA must remain fixed so that degraded performance cannot normalize itself indefinitely.

## Failure Modes

| Failure mode | What goes wrong | Safe response |
|---|---|---|
| **False-positive ACK** | Target sends unrelated text; sender closes the order | Require exact `task_id`, `in_reply_to`, expected `src`, and allowed event kind |
| **Lost ACK after successful work** | Sender retries and repeats side effect | Same task/idempotency key; deduplication or external-state reconciliation |
| **ACK before durable processing** | Receiver says accepted or done, then crashes before preserving work/result | `SEEN` only after durable import; `DONE` only after durable result/idempotency record |
| **Duplicate rail copy** | Same ACK appears in Telegram and file rail | Same `event_id`; deduplicate before state reduction |
| **Divergent copies** | Same ID carries different bodies on two rails | Integrity conflict; never last-writer-wins |
| **Out-of-order arrival** | `DONE` or `ACCEPTED` arrives before `TASK` on one machine | Retain orphan event and apply after origin arrives |
| **Late/stale ACK** | Old attempt ACK arrives after retry or cancellation | Validate attempt and referenced origin; record as late without reopening state |
| **Poison task** | Deterministic failure circulates indefinitely | Maximum attempts, delayed retries, dead-letter quarantine |
| **Retry storm** | All peers reping or retry on the same fixed schedule | One retry owner, exponential backoff, jitter, and rate-limited human alerts |
| **Head-of-line blocking** | One stuck task prevents later work from being noticed | Independent task states; no global cursor as completion ledger |
| **ACK storm** | Every copied rail event produces acknowledgements of acknowledgements | ACK only state-changing task events; never ACK an ACK unless a protocol operation explicitly requires it |
| **Clock skew** | Timestamp ordering gives impossible transitions | Use explicit references for causality; use time only for SLA |
| **Split-brain ledger** | Sender and receiver calculate different status | One canonical event set and deterministic reducer; expose conflicts |
| **Premature expiry** | Offline receiver is declared failed despite safely queued task | Preserve open debt; distinguish offline, unseen, deferred, and expired |
| **Silent manual repair** | Human edits JSON to make alert disappear | Repair only through auditable override events |
| **Ambiguous irreversible effect** | Timeout occurs after command may have charged/sent/deleted | `UNKNOWN_EFFECT`, reconciliation, or human intervention; no automatic retry |

RabbitMQ documents that after classic mirrored-queue failover, messages whose acknowledgements were lost or not replicated may be requeued, requiring consumers to tolerate seeing work they already processed. This is a concrete documented lost-ACK/redelivery scenario, although the classic mirrored-queue feature is legacy rather than current best practice. ([RabbitMQ Classic Queue Mirroring, legacy documentation; accessed 2026-08-04](https://www.rabbitmq.com/docs/3.13/ha)). citeturn13search7

SQS documents two related edge cases: a standard-queue replica may later redeliver a message that another replica treated as deleted; and a FIFO producer retry after its five-minute deduplication interval may create a duplicate. Visibility expiry can also allow a second consumer to process work while the first is still running. These are not implementation bugs; they are consequences of the service’s documented boundaries. ([AWS SQS at-least-once delivery, undated](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues-at-least-once-delivery.html); [AWS SQS outage recovery, undated](https://docs.aws.amazon.com/en_gb/AWSSimpleQueueService/latest/SQSDeveloperGuide/designing-for-outage-recovery-scenarios.html)). citeturn18search9turn18search2

RabbitMQ explicitly warns that if every consumer immediately requeues a delivery because of a transient condition, the system can enter a requeue/redelivery loop that consumes CPU and network bandwidth. Current quorum queues include delivery-limit mechanisms for poison-message handling, while RabbitMQ recommends dead-letter configuration to avoid unintended loss. ([RabbitMQ ACK/NACK guidance, undated](https://www.rabbitmq.com/docs/3.13/confirms); [RabbitMQ quorum queues, undated; accessed 2026-08-04](https://www.rabbitmq.com/docs/4.2/quorum-queues)). citeturn13search1turn13search8

A partial SAP support case describes AMQP-layer and integration-layer retries continuing independently, causing duplicate backend processing even after messages reached a dead-letter queue. The public page is only a knowledge-base preview, not a complete post-mortem, so this evidence is **[single-source]** and less informative than a full incident report. ([SAP KBA 3689953, publication date not stated; accessed 2026-08-04](https://userapps.support.sap.com/sap/support/knowledge/en/3689953)). citeturn19search6

High-quality public post-mortems focused narrowly on ACK semantics in autonomous-agent fleets were not found. **[no source found]** Most public evidence consists of broker documentation, support cases, and general distributed-systems analyses rather than detailed incident timelines. The design should therefore be validated with fault injection rather than relying solely on anecdotal incidents.

Essential deterministic tests are:

1. Deliver the same `TASK` through both rails in both orders.
2. Deliver `ACCEPTED` before `TASK`.
3. Drop `ACCEPTED` on Telegram but retain it in the file rail.
4. Execute, crash before `DONE`, and redeliver.
5. Emit unrelated target activity after a task and verify no state change.
6. Emit two different events with the same `event_id`.
7. Allow the lease to expire while progress is still being generated elsewhere.
8. Retry beyond the cap and verify one dead-letter entry and one operator alert.
9. Bring a machine online after several hours and verify that historical open tasks are reconstructed.
10. Apply a manual `MARK-DONE` or `FORWARD` event and rebuild the ledger from scratch.

## Strategic Recommendations

### Mapping to the Existing Weaknesses

| Weakness | Recommended change | Correctness gain | Added complexity | Confidence |
|---|---|---:|---:|---|
| **False-positive “RESPONSIVE” ACKs** | Delete activity-based closure. Require `task_id`, `in_reply_to`, expected receiver identity, and enumerated ACK kind. Track liveness separately. | Very high | Very low | **[established]** |
| **No explicit state machine** | Implement `SENT → SEEN → ACCEPTED`, with `DEFERRED`/`NEEDS_INPUT`, and explicit terminals. Treat `PROGRESS` as an event and lease renewal. | Very high | Low | **[established]** |
| **Two divergent ledgers** | Replace independent mutable truth with one append-only logical event ledger; sender and receiver reports become derived views. | Very high | Moderate | **[established]** |
| **Telegram-only detection** | Import both rails into a union keyed by `event_id`; watchdog reads the merged local ledger. | Very high | Low–moderate | **[established]** |
| **Free-text NACK only** | Add `REJECTED`, `DEFERRED`, `NEEDS_INPUT`, and `FAILED retryable=<bool>` plus a small reason-code list. | High | Low | **[established]** |
| **Ad-hoc IDs and heuristic tags** | Use full UUIDv7 `task_id` and `event_id`; add `in_reply_to`, `attempt`, and `idempotency_key`; short IDs are display-only. | High | Low | **[established]** |
| **No DLQ/backoff/idempotency** | Bound execution attempts; distinguish transport replay from execution retry; add delayed backoff and `dead-letter.md`; register side-effect idempotency keys. | Very high | Moderate | **[established]** |
| **Fixed timeout ladder** | Replace one ladder with SLA classes, absolute deadlines, renewable ownership leases, and receiver-provided `retry_after`; add bounded statistical adaptation later. | Medium–high | Low initially, moderate later | **[established]** for classes/leases; **[speculative]** for statistical tuning |

### Recommended Protocol Messages

```text
# Receiver daemon has durably imported the task
👁 [macbook -> hub] SEEN
{"v":1,"event_id":"E2","task_id":"T1","kind":"SEEN",
 "src":"macbook","dst":"hub","in_reply_to":"E1"}

# Agent accepts responsibility
✅ [macbook -> hub] ACCEPTED
{"v":1,"event_id":"E3","task_id":"T1","kind":"ACCEPTED",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "lease_until":"2026-08-04T11:00:00Z"}

# Temporary inability
⏸ [macbook -> hub] DEFERRED
{"v":1,"event_id":"E4","task_id":"T1","kind":"DEFERRED",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "retry_after":"2026-08-04T14:00:00Z","reason_code":"BUSY"}

# More information required
❓ [macbook -> hub] NEEDS_INPUT
{"v":1,"event_id":"E5","task_id":"T1","kind":"NEEDS_INPUT",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "reason_code":"NEEDS_CONTEXT",
 "body":"Which branch should be reviewed?"}

# Progress/lease renewal
🟡 [macbook -> hub] PROGRESS
{"v":1,"event_id":"E6","task_id":"T1","kind":"PROGRESS",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "lease_until":"2026-08-04T12:00:00Z",
 "body":"Tests complete; reviewing concurrency paths."}

# Permanent pre-acceptance refusal
❌ [macbook -> hub] REJECTED
{"v":1,"event_id":"E7","task_id":"T1","kind":"REJECTED",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "reason_code":"UNSUPPORTED"}

# Accepted task failed
💥 [macbook -> hub] FAILED
{"v":1,"event_id":"E8","task_id":"T1","kind":"FAILED",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "attempt":1,"reason_code":"TRANSIENT_TOOL_ERROR",
 "retryable":true}

# Successful terminal completion
🏁 [macbook -> hub] DONE
{"v":1,"event_id":"E9","task_id":"T1","kind":"DONE",
 "src":"macbook","dst":"hub","in_reply_to":"E1",
 "attempt":2,"result_ref":"reviews/a81d3f.md"}
```

### State Reduction Precedence

Arrival order must not be treated as truth. The reducer should apply these rules:

```text
VALIDITY BEFORE TIME:
  reject invalid schema or wrong actor before evaluating timestamps

IDENTITY BEFORE PROSE:
  match task_id/event_id/in_reply_to, never body text

CAUSAL REFERENCE BEFORE ARRIVAL ORDER:
  an event may wait for its referenced origin

TERMINAL PER ATTEMPT:
  first valid terminal outcome closes that attempt

CONFLICTS ARE VISIBLE:
  incompatible terminal events produce CONFLICT, not last-write-wins

RETRY IS EXPLICIT:
  only a RETRY event from an authorized sender starts another attempt

DUPLICATES ARE NORMAL:
  same event_id + same canonical payload is ignored after first application
```

### Sender Watchdog Logic

The sender watchdog should become a state observer rather than an ACK guesser:

```python
for task in open_tasks:
    state = reduce_task(task.task_id, merged_events)

    if state.current in {"DONE", "REJECTED", "FAILED", "CANCELED", "EXPIRED"}:
        continue

    if state.current == "SENT" and now > task.seen_deadline:
        republish_missing_rails(task.origin_event_id)
        alert_bus_health_once(task)

    elif state.current == "SEEN" and now > task.ack_by:
        send_targeted_reminder(task)
        escalate_if_policy_requires(task)

    elif state.current == "ACCEPTED" and now > state.lease_until:
        emit_lease_expired_alert(task)

    elif state.current in {"DEFERRED", "NEEDS_INPUT"}:
        follow_explicit_blocking_policy(task, state)

    if now > task.due_by and not state.is_terminal:
        alert_deadline_breach(task)
```

No branch should ask whether the target has posted “something.” Liveness can influence alert wording and routing, but not debt closure.

### Receiver Debt Logic

`inbox_debt.py` should stop treating distinctive tags or arbitrary later citations as payment. Its debt set becomes:

```text
task addressed to this machine
AND no valid terminal state exists
AND task is not validly canceled
```

The receiver’s sweeper can still scan all history and ignore read cursors. That is a good existing property. The improvement is that it consumes parsed protocol events rather than heuristic action markers and prose citations. Legacy unstructured tasks can enter through a compatibility parser, but they should be normalized once into a canonical `TASK` event and thereafter follow the structured protocol.

### Legacy Compatibility

A transition period can accept the old format:

```text
🤖 [SRC -> DST] TASK body #abc123
✅ принял #abc123
```

The compatibility parser should create deterministic synthetic IDs, for example from the complete original message plus source and transport identity, and emit a warning:

```text
LEGACY_NORMALIZED
legacy_ref="#abc123"
task_id="<full synthetic UUID or hash>"
```

The old short identifier may help find the task but must not remain the authoritative key. Once a structured `task_id` is assigned, every later event must use it. Heuristic dashed tags should be deprecated rather than accepted indefinitely.

### Broker Migration Threshold

Migration to a real broker becomes justified when at least one of these conditions is sustained:

| Trigger | Why it changes the decision |
|---|---|
| Hundreds or thousands of task events per day | Full-file scans and Telegram history become operationally inefficient |
| Multiple competing executors for the same work queue | Leasing and atomic claim semantics become harder to hand-roll safely |
| Strict retention or audit requirements | Broker/store durability and administrative controls become valuable |
| Frequent rail partitions or Syncthing conflicts | Transport reliability, not protocol semantics, is now the dominant failure |
| Complex fan-out and consumer groups | NATS/RabbitMQ/Kafka primitives replace substantial custom routing |
| Multi-step workflows with compensation and long sleeps | Temporal-style durable workflow history may justify its operational cost |
| Cross-organization agent interoperability | A2A’s discovery, authentication, and task lifecycle become useful |

Even after migration, retain the application-level task state machine and idempotency keys. A broker can prove that a delivery was acknowledged according to its own rules; it cannot decide that an agent’s review was semantically adequate or that an external side effect is safe to repeat.

## Action Plan

### Correctness First

| Change | Simplicity weight | Benefit | Main risk |
|---|---:|---:|---|
| Remove activity-based `RESPONSIVE` closure | Excellent | Eliminates the most dangerous silent false positive | More tasks will initially remain visibly open, exposing existing noncompliance |
| Add `task_id`, `event_id`, `in_reply_to`, `attempt`, `idempotency_key` | Excellent | Makes every later control deterministic | Legacy parser must avoid accidentally generating different IDs on different nodes |
| Require exact ACK tuple | Excellent | Converts ACK from heuristic to protocol fact | Agents must be prompted or wrapped to emit the structured fields |
| Read both rails into one local event union | Very good | Stops Telegram outages from creating false silence | Importers must canonicalize without rewriting event identity |

Acceptance tests for this phase are binary: unrelated activity never closes a task; an ACK arriving only through Syncthing closes the appropriate acceptance condition; the same ACK arriving through both rails is applied once; and rebuilding from raw events produces the same state.

### State and Failure Semantics

| Change | Simplicity weight | Benefit | Main risk |
|---|---:|---:|---|
| Add `SEEN` and `ACCEPTED` distinction | Very good | Separates importer health from agent ownership | `SEEN` must not be displayed as “handled” |
| Add typed `DEFERRED`, `NEEDS_INPUT`, `REJECTED`, `FAILED` | Very good | Prevents blind retry and unstructured ambiguity | Too many reason codes would recreate free text in enum form |
| Make `PROGRESS` renew a lease | Good | Detects hung accepted tasks without fixed completion assumptions | Agents might emit meaningless progress solely to suppress alarms |
| Require result evidence on `DONE` | Good | Reduces false-positive completion | Some legitimate tasks have no artifact; allow explicit `no_result_reason` |

### Ledger Reconciliation and Repairability

| Change | Simplicity weight | Benefit | Main risk |
|---|---:|---:|---|
| Per-node append-only NDJSON event files | Very good | Avoids Syncthing concurrent-write conflicts | Files need rotation or archival eventually |
| Rebuildable `state-cache.json` or SQLite index | Very good | Fast queries without making cache authoritative | Operator must know cache can be deleted/rebuilt |
| Generated `operator-status.md` | Excellent | Human-readable open/owned/blocked/dead-letter view | Must be regenerated, never manually treated as source of truth |
| Auditable repair commands | Good | Allows hammer-and-screwdriver recovery | `MARK-DONE` can conceal real failure unless identity and reason are recorded |

The operator status should fit on one screen:

```text
WAITING
  T…b37d hub → macbook  seen 09:12, acceptance overdue 18m

OWNED
  T…82aa vps → hub      lease until 12:40, progress 12:03

BLOCKED
  T…11c7 hub → mac1     NEEDS_INPUT: branch name

DEAD LETTER
  T…90fe laptop → vps   PERMANENT_TOOL_ERROR, 3 attempts
```

### Idempotency, Retry, and Dead Letter

| Change | Simplicity weight | Benefit | Main risk |
|---|---:|---:|---|
| Distinguish rail replay from execution retry | Excellent | Prevents attempt inflation and duplicated effects | Existing scripts may currently conflate resend and rerun |
| Add per-task `max_attempts` and capped backoff | Very good | Stops poison loops | Too-low caps can quarantine recoverable tasks |
| Add idempotency registry for side effects | Good | Largest protection against lost-ACK duplicates | Side effects spanning external systems require operation-specific reconciliation |
| Add `UNKNOWN_EFFECT` handling | Good | Prevents dangerous blind retry | Requires human review for ambiguous irreversible actions |
| Generate `dead-letter.md` | Excellent | Makes failure visible and repairable | DLQ must be monitored; quarantine without review is merely delayed loss |

A reasonable initial backoff for machine-to-machine task execution is deterministic by task class plus small jitter, for example one minute, five minutes, twenty minutes, then quarantine. These are proposed defaults rather than empirically established universal values. The existing human escalation ladder may remain, but it should be driven by explicit state and absolute deadlines rather than message activity.

### SLA and Adaptive Timing

First introduce named SLA classes and explicit `ack_by`, `due_by`, and `lease_until`. Let the receiver emit `DEFERRED retry_after=...` rather than forcing repeated pings while it is legitimately unavailable. Differentiate:

```text
UNSEEN
SEEN_BUT_UNOWNED
OWNED_LEASE_VALID
OWNED_LEASE_EXPIRED
BLOCKED
DEADLINE_BREACHED
```

Only after these observations are trustworthy should the system calculate per-machine, per-task-class latency distributions. The adaptive value must be clamped between human-set minimum and maximum bounds and must not modify the task’s absolute `due_by` without an explicit event.

### Named Strategic Options

| Option | Description | Advantages | Risks and costs | Verdict |
|---|---|---|---|---|
| **ACK-Lite Event Ledger** | Structured protocol, per-node append-only logs, merged dual rails, deterministic reducer | Maximum correctness gain with current transport; inspectable with a text editor; no new service dependency | Requires careful parser/reducer tests and idempotent task wrappers | **Recommended** |
| **VPS Anchor SQLite Ledger** | All machines report task events to one SQLite-backed authority on the VPS, with rails as ingress/egress | Very simple querying, uniqueness constraints, easy dashboard | VPS becomes authoritative dependency; offline peers may be unable to commit state; backup/restore matters | Acceptable convenience layer, not sole truth |
| **NATS JetStream plus ACK-Lite State** | Replace retention/redelivery mechanics with JetStream while retaining application states | Native ACK waits, redelivery, backoff, MaxDeliver, durable consumers | New daemon, credentials, networking, upgrades, storage recovery; still needs business `DONE` semantics | Adopt only when transport operations dominate |
| **RabbitMQ plus ACK-Lite State** | Use queues, publisher confirms, consumer ACK/NACK, and DLX | Mature operational semantics and explicit negative ACK paths | Heavier operational surface than NATS for a personal fleet; channel and queue configuration complexity | Technically strong, simplicity mismatch |
| **Temporal Durable Workflows** | Represent multi-step agent tasks as durable workflows and Activities | Excellent crash recovery, history, retry, timeout, and compensation model | Largest conceptual and operational jump; risks turning a small fleet into a workflow-platform project | Reserve for genuinely multi-step, high-value workflows |
| **A2A-Compatible Task API** | Expose the reduced task lifecycle through A2A v1.0 interfaces | Future interoperability, standard task lifecycle, push/stream/poll options | More protocol, auth, discovery, and SDK complexity than needed internally | Add as an adapter later, not as the internal source of truth |

**Single recommendation: ACK-Lite Event Ledger — [established].** Its main assumption is that the fleet remains low-volume and predominantly single-assignee, so deterministic merging of a handful of append-only logs is easier to operate than a dedicated broker or durable workflow platform. The recommendation would change toward NATS JetStream if competing consumers, high event volume, or transport-level retention failures become routine; it would change toward Temporal if tasks become long multi-step transactions requiring durable timers, compensation, and replay across many external systems.

## Sources

**Research brief.** “DR26-07-15-ZB-01 — Improving the ACK / message-acknowledgement protocol of a multi-machine personal AI fleet,” user-provided document, 2026-08-04. fileciteturn0file0

**Amazon Web Services.** “Amazon SQS visibility timeout,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html citeturn20view3

**Amazon Web Services.** “Amazon SQS at-least-once delivery,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues-at-least-once-delivery.html citeturn18search9

**Amazon Web Services.** “Exactly-once processing in Amazon SQS,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues-exactly-once-processing.html citeturn17search1

**Amazon Web Services.** “Using the message deduplication ID in Amazon SQS,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html citeturn18search1

**Amazon Web Services.** “Outage recovery scenarios in Amazon SQS,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/en_gb/AWSSimpleQueueService/latest/SQSDeveloperGuide/designing-for-outage-recovery-scenarios.html citeturn18search2

**Amazon Web Services.** “Amazon SQS message deduplication and grouping,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/best-practices-message-deduplication.html citeturn18search6

**Amazon Web Services.** “Using dead-letter queues in Amazon SQS,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html citeturn1search7

**Amazon Web Services.** “Transactional outbox pattern,” publication date not stated; accessed 2026-08-04. https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html citeturn14search0

**Google Cloud.** “Exactly-once delivery,” last updated 2026-07-17. https://docs.cloud.google.com/pubsub/docs/exactly-once-delivery citeturn20view0

**Google Cloud.** “Forward undeliverable messages to a dead-letter topic,” living documentation; accessed 2026-08-04. https://cloud.google.com/pubsub/docs/dead-letter-topics citeturn0search1turn0search4

**RabbitMQ.** “Consumer Acknowledgements and Publisher Confirms,” publication date not stated; accessed 2026-08-04. https://www.rabbitmq.com/docs/3.13/confirms citeturn20view2

**RabbitMQ.** “Negative Acknowledgements,” publication date not stated; accessed 2026-08-04. https://www.rabbitmq.com/docs/3.13/nack citeturn13search0

**RabbitMQ.** “AMQP 1.0,” publication date not stated; accessed 2026-08-04. https://www.rabbitmq.com/docs/amqp citeturn13search13

**RabbitMQ.** “Quorum Queues,” publication date not stated; accessed 2026-08-04. https://www.rabbitmq.com/docs/4.2/quorum-queues citeturn13search8

**RabbitMQ.** “Classic Queue Mirroring,” legacy documentation; publication date not stated; accessed 2026-08-04. https://www.rabbitmq.com/docs/3.13/ha citeturn13search7

**NATS.** “Pull consumers in depth / JetStream Consumers,” publication date not stated; accessed 2026-08-04. https://docs.nats.io/learn/jetstream/pull-consumers citeturn20view1

**Apache Kafka.** “Kafka Streams Core Concepts,” modified 2025-12-19. https://kafka.apache.org/10/streams/core-concepts/ citeturn20view4

**Akka.** “Message Delivery Reliability,” publication date not stated; accessed 2026-08-04. https://doc.akka.io/libraries/akka-core/current/general/message-delivery-reliability.html citeturn20view5

**Akka.** “Reliable Delivery,” publication date not stated; accessed 2026-08-04. https://doc.akka.io/libraries/akka-core/current/typed/reliable-delivery.html citeturn2search2

**Microsoft Orleans.** “Messaging delivery guarantees,” updated 2025-05-29. https://learn.microsoft.com/en-us/dotnet/orleans/implementation/messaging-delivery-guarantees citeturn20view6

**Erlang/OTP.** “Processes — Erlang System Documentation v29,” versioned living documentation; accessed 2026-08-04. https://www.erlang.org/doc/system/ref_man_processes.html citeturn20view7

**Apache Airflow.** “Tasks — Airflow 3.3.0 Documentation,” versioned documentation; accessed 2026-08-04. https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/tasks.html citeturn20view14

**n8n.** “All executions,” publication date not stated; accessed 2026-08-04. https://docs.n8n.io/workflows/executions/all-executions/ citeturn21search0

**Temporal.** “Temporal API Documentation,” publication date not stated; accessed 2026-08-04. https://api-docs.temporal.io/ citeturn8search3

**LangChain.** “What’s new in LangGraph v1,” publication date not stated; accessed 2026-08-04. https://docs.langchain.com/oss/javascript/releases/langgraph-v1 citeturn20view8

**OpenAI.** “Handoffs — OpenAI Agents SDK,” publication date not stated; accessed 2026-08-04. https://openai.github.io/openai-agents-python/handoffs/ citeturn20view9

**OpenAI.** “Running agents — OpenAI Agents SDK,” publication date not stated; accessed 2026-08-04. https://openai.github.io/openai-agents-python/running_agents/ citeturn20view10

**OpenAI.** “Swarm: Educational framework exploring ergonomic, lightweight multi-agent orchestration,” repository documentation; accessed 2026-08-04. https://github.com/openai/swarm citeturn14search4

**Microsoft AutoGen.** “Message and Communication,” publication date not stated; accessed 2026-08-04. https://microsoft.github.io/autogen/stable/user-guide/core-user-guide/framework/message-and-communication.html citeturn20view13

**CrewAI.** “CrewAI Documentation,” publication date not stated; accessed 2026-08-04. https://docs.crewai.com/ citeturn21search4

**AG2.** “Handoffs,” published July 2026. https://docs.ag2.ai/latest/docs/user-guide/advanced-concepts/orchestration/group-chat/handoffs/ citeturn15search8

**Model Context Protocol.** “Overview / Base Protocol,” specification dated 2025-11-25. https://modelcontextprotocol.io/specification/2025-11-25/basic citeturn20view12

**Model Context Protocol.** “Tasks,” specification dated 2025-11-25. https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks citeturn20view11

**Model Context Protocol.** “Lifecycle,” specification dated 2025-11-25. https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle citeturn5search7

**A2A Project.** “A2A Releases,” v1.0.0 released 2026-03-12; v1.0.1 released 2026-05-26. https://github.com/a2aproject/A2A/releases citeturn16search0

**A2A Project.** “Agent2Agent Protocol repository,” living project documentation; accessed 2026-08-04. https://github.com/a2aproject/A2A citeturn16search3

**Predoaia, I.; Vu, T. M.; Barmpis, K.; Kolovos, D.; García-Domínguez, A.** “A Comparative Study of MCP and A2A for Inter-Agent Coordination in LLM-Based Systems,” submitted 2026-07-26. https://arxiv.org/abs/2607.23884 citeturn15academia45

**Linux Foundation.** “Linux Foundation Welcomes the AGNTCY Project…,” published 2025-07-29. https://www.linuxfoundation.org/press/linux-foundation-welcomes-the-agntcy-project-to-standardize-open-multi-agent-system-infrastructure-and-break-down-ai-agent-silos citeturn17search6

**Muscariello, L.; Papalini, M.; Sardara, M.; Betts, S.** “An Overview of Messaging Systems and Their Applicability to Agentic AI,” IETF Internet-Draft revision published 2026-07-07. https://www.ietf.org/ietf-ftp/internet-drafts/draft-mpsb-agntcy-messaging-02.html citeturn17search5

**Muscariello et al.** “Secure Low-Latency Interactive Messaging,” IETF Internet-Draft published 2026-02-24. https://www.ietf.org/archive/id/draft-mpsb-agntcy-slim-01.html citeturn17search7

**IETF.** RFC 9562, “Universally Unique IDentifiers,” published May 2024. https://www.rfc-editor.org/rfc/rfc9562.html citeturn13search9

**Lamport, Leslie.** “Time, Clocks, and the Ordering of Events in a Distributed System,” published July 1978. https://www.microsoft.com/en-us/research/publication/time-clocks-ordering-events-distributed-system/ citeturn9search2

**Lampson, Butler.** “Reliable Messages and Connection Establishment,” published 1993. https://www.microsoft.com/en-us/research/publication/reliable-messages-and-connection-establishment/ citeturn9search1

**Garcia-Molina, Hector; Salem, Kenneth.** “Sagas,” published 1987. https://dl.acm.org/doi/10.1145/38713.38742 citeturn9search0

**Godard, Emmanuel; Perdereau, Eloi.** “Back to the Coordinated Attack Problem,” published online 2021-07-09. https://www.cambridge.org/core/journals/mathematical-structures-in-computer-science/article/abs/back-to-the-coordinated-attack-problem/55E9BC43CD76ADA013EDECF4DED1A002 citeturn13search11

**Halpern, Joseph Y.; Moses, Yoram.** “Knowledge and Common Knowledge in a Distributed Environment,” arXiv posting dated 2000-06-02; underlying work is older. https://arxiv.org/abs/cs/0006009 citeturn22academia49

**Hellerstein et al.** “Delivery, Consistency, and Determinism: Rethinking Guarantees in Distributed Systems,” published 2019. https://arxiv.org/abs/1907.06286 citeturn10view2

**Kleppmann, Martin.** *Designing Data-Intensive Applications*, published March 2017. https://dataintensive.net/ citeturn22search3

**Hohpe, Gregor; Woolf, Bobby.** *Enterprise Integration Patterns*, book published 2003-10-10; supporting pattern site continuously maintained. https://www.enterpriseintegrationpatterns.com/patterns/messaging/ citeturn22search13

**Richardson, Chris.** “Pattern: Idempotent Consumer,” publication date not stated; accessed 2026-08-04. https://microservices.io/patterns/communication-style/idempotent-consumer.html citeturn13search2

**SAP.** KBA 3689953, “Understanding retry behavior in AMQP sender adapter,” publication date not stated; accessed 2026-08-04. https://userapps.support.sap.com/sap/support/knowledge/en/3689953 citeturn19search6