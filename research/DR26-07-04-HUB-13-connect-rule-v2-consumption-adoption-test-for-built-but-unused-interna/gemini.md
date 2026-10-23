# DR26-07-04-HUB-13 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!--
source: Gemini (gemini.google.com, Deep Research, Pro) — account почтовый ящик (Punk Crypto)
topic: DR26-07-04-HUB-13 — Connect rule v2: adoption/consumption test
date: 2026-07-16

url: https://приватный чат
status: complete (full report)
-->

# DR26-07-04-HUB-13 — Gemini report (verbatim)

## Research Plan (methodology preamble)

**Prioritization of Sources:** primary foundational frameworks (Kaoru Ishikawa's Total Quality Control, Goldratt's Theory of Constraints, Robert G. Cooper's Stage-Gate) + established empirical research from standard bodies (DORA/DevEx 2024-2026). Secondary commentary and vendor docs (specific DLQ implementations) used strictly as applied examples.
**Failure Case Allocation:** explicit steps mapping failure modes of the "Consumption Test" — chilling effect of strict "pull" on exploratory R&D ("Fuzzy Front End"), susceptibility of adoption metrics to Goodhart's Law, friction of premature optimization.
**Head-to-Head Comparison:** decision compares 3-5 named architectural alternatives (Strict Kanban/Pull, Time-Boxed Garbage Collection, Active Consumer Handshake).

---

## Report

Producing an artifact does not equate to delivering value. For a solo operator orchestrating pipelines + AI assistants, artifact-generation velocity can exceed human processing capacity by orders of magnitude; accumulation without downstream utilization (a queue dormant 2 weeks) indicates systemic collapse of the feedback loop. This report analyzes "Connect" rule v2 — transition from accountability for the act of handoff to strict accountability for downstream consumption — synthesizing TPS, ToC, 2025-2026 DevEx metrics, and SRE telemetry.

### Output vs. Outcome: The Maturation of "Done"
Output = compiled script / populated table / generated dashboard. Outcome = downstream agent successfully executing that script to resolve a constraint.
- **Value Realization Framework (2025-2026):** enterprise tech pivoted from PMOs to **Value Realization Offices (VROs)** — "strategy-to-value gap"; transformation measured by accountability, not activity (kyndryl.com, Feb 2026) [established]. Scaling AI often yields "stalled adoption, governance bottlenecks, or process friction—all of which quietly lower expected returns."
- Solo operator: set **outcome-KRs** (e.g., % reduction in manual processing time by an AI agent using a script), not output counts. A mature **Definition of Done** requires the increment be "ready to be accepted by a user, customer, team, or **consuming system**" (playbook.truss.works) [established].
- **Vanity-metrics trap in AI tooling:** DORA 2024-2025 paradox — a **25% increase in AI adoption correlated with a 7.2% DECREASE in software delivery stability** (augmentcode.com, 2025) [established]. AI lets operators generate batch sizes that can't be reviewed/integrated/consumed downstream; output metric climbs, outcome metric flatlines. Connect forces measuring integration + execution, bypassing vanity of raw generation.

### Adoption, Activation, and the Internal "Last Mile"
Perri's **Build Trap** (output-measured orgs) applies to internal **Platform as a Product**: operator = PM, downstream AI agent/pipeline = customer. A queue of 100 untouched suggestions = internal build trap (optimized for generation, not **activation**). Track last-mile via internal product metrics:
- **Activation Rate:** % of generated queue items executed by downstream agent within an operational window.
- **Time-to-Value (TTV):** latency from generation to successful consumption.
- **Feature Adoption:** internal scripts repeatedly executed in production vs abandoned after one run.
- **DevEx "Core 4"** (Speed, Effectiveness, Quality, Business Impact) measures friction (getdx.com, 2025) [emerging]. If an agent can't parse a JSON payload because upstream changed schema, "Effectiveness" → 0. Consumption test requires the upstream sender own the schema contract.

### Pull vs. Push: Lean, TPS, Theory of Constraints
- **"The next process is your customer"** — Ishikawa, 1981: every entity is both internal supplier AND internal customer; treat the immediate receiver with the rigor of an external paying client (compliantltd.com, 2025) [established]. Upstream script not successful until downstream AI executes its payload.
- **Theory of Constraints + Drum-Buffer-Rope** (Goldratt): unprocessed queue = excess factory inventory (wasted compute/tokens/awareness). **Drum** = bottleneck (AI context-window limit / operator review time) sets pace; **Buffer** = small strictly-capped queue before the constraint; **Rope** = signal releasing new upstream work ONLY when constraint consumed an item (SAP ToC doc, 2024). Strict WIP limits (Buffer) + demand-triggered generation (Rope) = **Pull System** that structurally prevents building into the void.

### Instrumenting Consumption (solo operator)
- **Dead Letter Queue (DLQ) with mandatory triage:** failed messages quarantined after retries (conduktor.io) [established]. Anti-pattern = passive graveyard; "a DLQ is not a destination for bad messages; it is a quarantine that demands a human or automated resolution workflow" (abstractalgorithms.dev, 2026) [emerging]. DLQ must have a **mandatory consumer**: a script that alerts the operator daily → forces triage → prevents 2-week silent pileup.
- **Orphan / staleness sweeps:** for static artifacts (dashboards, docs, scripts) scan for assets lacking recent interaction telemetry (bugagent.com, 2026) [single-source][low-authority]; if last_accessed > threshold (14 days) → flag stale → repair delivery or delete.
- **Telemetry heartbeats:** lightweight appending logger or SQLite tracker (no OpenTelemetry needed); upstream generation validated only when heartbeat registers a downstream ping.

### Diagnosing "Not Used": Delivery Break vs Zero Demand
- **Root A — Delivery Break (not connected):** artifact has high value but systemic friction blocks consumption (malformed JSON, rate-limited endpoints, insufficient prompt context). **Resolution:** triage DLQ exception headers, repair integration, enforce schema contracts. It was desired; the handoff failed.
- **Root B — Zero Demand (shouldn't have been built):** artifact flawless but consumer ignores it (solves no constraint). Continuing = **Sunk-Cost Fallacy / Escalation of Commitment** (stratrix.com) [established]. **Resolution: Kill Criteria** — predefined objective thresholds (Robert G. Cooper's Stage-Gate, 1980s) that trigger termination, shifting from emotional judgment to mathematical certainty. E.g., 0% activation over a 14-day rolling window → disable upstream script, archive repo, reallocate to pipelines with actual pull.

### Where the Consumption Test Harms Itself
- **Risk 1 — Premature optimization / death of exploratory R&D:** strict "build on demand only" assumes the consumer has perfect knowledge of need — false in high-variance exploration. 5th-Gen **Agile-Stage-Gate** integrates spiral development for the "Fuzzy Front End" where artifacts test hypotheses, not immediate consumption (stage-gate.com) [established]. **Balance:** a **"Proof of Concept" exemption class** — speculative artifacts get a time-boxed window with the consumption test suspended; no organic consumption by expiry → kill criteria auto-execute.
- **Risk 2 — Goodhart's Law / gamed usage metrics:** when a measure becomes a target it ceases to be a good measure. Teams redefine "failure" to exclude minor degradations or deploy feature-flagged empty code to inflate deployment frequency (imrul.tech, 2024) [emerging]; an AI agent could just "ACK" a queue item and drop the payload, satisfying the test synthetically. **Balance:** shift instrumentation from measuring **activity** to measuring **state changes** — demand verifiable proof of outcome (updated DB record, closed ticket, compiled app), not a read-receipt.

### Strategic Framework Mapping (table)
| Framework | Core Axiom | Application to Connect | Source |
|---|---|---|---|
| Value Realization (VRO) | Measured by economic/behavioral impact, not deployment of tech | Task "done" only when AI utilizes it to achieve a measurable goal | Kyndryl 2026 |
| The Build Trap | Failure = measuring output volume not outcome value | Evaluate pipeline by activation rate, not raw volume pushed | Melissa Perri |
| Next Process is Customer | Internal handoffs get external-client quality rigor | Upstream script accountable for formatting to downstream AI's exact schema | Ishikawa 1981 |
| Theory of Constraints (DBR) | Pace by bottleneck (Drum), protect w/ small queue (Buffer), feed on demand (Rope) | Strict WIP limits; generator runs only when AI clears buffer | Goldratt |
| Dead Letter Queue | Unprocessable msgs quarantined for accountable triage | Task past TTL → DLQ → mandatory daily operator alert | Confluent; AbstractAlgorithms 2026 |
| Kill Criteria (Stage-Gate) | Objective thresholds terminate failing initiatives, beat sunk-cost | Output unconsumed 14 days → auto-disable pipeline + archive | Robert G. Cooper |

### Top 5 Concrete Mechanisms (<1 day)
1. **"TTL-to-DLQ" Triage Alarm:** strict TTL (48h) on queue/DB/SQLite; unprocessed → auto-move to DLQ table; cron script pushes DLQ count to phone. Rule: day can't end until DLQ triaged to zero.
2. **Hard WIP Limits (the Pull Rope):** upstream generators query downstream queue depth before running; if queue_count >= 5, exit without generating. Forces the AI to pull; surfaces blockages.
3. **"Orphan Sweeper" script:** weekly cron checks last_accessed of output files/dashboards/logs; unread 7 days → move to /deprecated_holding pending deletion.
4. **Redefine the "Done" telemetry flag:** two-phase commit — upstream creates status='built'; only downstream AI writing a successful execution payload sets status='done'. Dashboard monitors the delta 'built'→'done' (last-mile latency).
5. **Binary Kill Criteria config:** plain-text kill_criteria.json with objective thresholds; AI reads it against weekly telemetry sweep; if met, AI auto-generates a PR to delete offending pipelines → operator accepts or formally justifies exception.

### Decision-Oriented Conclusion — 3 named options
- **Option A: Strict Kanban / Zero-Speculation Pull** — data generated only via synchronous blocking calls on explicit downstream request. Eliminates all unused inventory (100% consumption) but tightly couples systems, raises latency, prevents speculative pre-computation.
- **Option B: Time-Boxed Garbage Collection (passive sweeping)** — speculative async Push + aggressive TTLs + Orphan Sweeper auto-delete after ~48h. Preserves decoupling + high-velocity async generation; tolerates baseline wasted compute; passive deletion risks hiding systemic delivery breaks.
- **Option C: The Active Consumer Handshake (DLQ Routing)** — async queue + strict WIP limits; unconsumed/failed within threshold → DLQ → high-priority mandatory human-in-the-loop alert.

**Recommendation: Option C (Active Consumer Handshake / DLQ Routing).**
**Confidence: [established]** — corroborated by modern SRE patterns + Value Realization governance.
**Core assumption:** the operator has the discipline to treat a DLQ alert as a **"stop-the-line" Andon-cord** event, not letting the DLQ degenerate into an ignored graveyard.
**Rationale:** balances async speed (needed for AI pipelines) with strict accountability; ToC-style small capped buffer + DLQ makes last-mile failures aggressively observable; differentiating successful consumption / retryable errors / unconsumed stagnation lets kill criteria apply dispassionately.
