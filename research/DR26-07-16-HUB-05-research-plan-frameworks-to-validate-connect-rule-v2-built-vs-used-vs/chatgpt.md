# DR26-07-16-HUB-05 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-16-HUB-05 — Operational hardening design

## Bottom line

Do not try to make the Hetzner VPS immortal. Make it **replaceable without duplicating authority or external effects**.

The incident exposed three separate control failures:

1. **Administrative power loss was invisible.** Billing is upstream of the VM power plane, but it was not monitored as infrastructure.
2. **Reboot was used as diagnosis.** A reboot should be the last bounded remediation after explicit preconditions, never the first probe.
3. **Anchor duties were attached to a machine rather than a fenced role grant.** Heartbeat and priority can nominate a candidate, but they cannot safely authorize effects.

The appropriate AK‑47 architecture is:

```text
Billing mailbox + positive account acknowledgment -----\
Hetzner read-only API ----------------------------------+--> deterministic guard
Two independent network probes ------------------------+       |
Guest semantic health + systemd/cgroup/clock ----------/       +--> diagnosis JSON
                                                               +--> SHA-256 diagnosis_hash
                                                               +--> reboot vetoes

LLM agent --------------------------------------------------> recommendation only
                                                               |
                                                     allow-listed action gateway
                                                     with separate write credential

Existing committed ledger --> ROLE_GRANT(epoch, lease) --> anchor or standby
                                      |
                           fencing for every role effect
```

The overriding policy should be:

> **Prefer a temporary loss of availability to an ambiguous or duplicate authority.**

That is especially important for Telegram outreach, canon, irreversible operations, and money.

---

# Ranked hardening plan

| Rank | Failure closed | Minimal AK‑47 mechanism | Change type | Confidence |
|---:|---|---|---|---|
| **1** | Rebooting through billing suspension or unknown account state | Make `billing != CLEAR_AND_FRESH` an unconditional power-action veto | Small code + config | **Established** |
| **2** | Unpaid invoice detected too late | Off-anchor billing-mail monitor, positive Accounts acknowledgment, external dead-man, 2–3 months account credit | Config + small code | **Established** |
| **3** | LLM guesses or acts on stale status | Read-only evidence collector producing deterministic JSON and `diagnosis_hash`; write token held only by a narrow action gateway | Small code | **Emerging** application of established controls |
| **4** | Wrong reboot for disk-full, OOM, daemon crash, or clock drift | Semantic `/healthz`, statvfs/fsync probe, systemd state, cgroup OOM counters, PSI, chrony status | Config + small code | **Established** |
| **5** | Anchor/desktop split brain | Committed per-role `ROLE_GRANT` with monotonically increasing epoch and bounded lease; heartbeat only selects candidate | Protocol + code | **Established** |
| **6** | Duplicate arbitration, folds, or Telegram messages | Stable `effect_id`; deterministic fold hashes; durable Telegram outbox with `UNKNOWN_SENT` | Code | **Established** |
| **7** | Failback race when preferred anchor returns | Sticky incumbent, recovery soak, drain cutoff, `HANDOFF_DRAINED`, then a new epoch | Protocol + code | **Established** |
| **8** | Desktop nominally standby but asleep/unready | No-sleep configuration, wired network, UPS, boot-enabled services, continuously synced state, recurring takeover drill | Config + hardware | **Established** |
| **9** | Maintenance, IP drift, or account loss destroys recovery path | Provider-status subscriptions, protected Primary IP, off-provider ledger/config backups and restore tests | Mostly config | **Established** |
| **10** | Hetzner account/region and home outage overlap | Small second-provider VPS, after fencing has been proven | Second node | **Established** architecture; ROI is contextual |
| **11** | Complexity itself becomes the new failure mode | Do not add Kubernetes, an additional Raft cluster, active-active Telegram, portal scraping, or autonomous payment | Policy | **Established** |

---

# 1. Billing and payment as an infrastructure failure mode

## What Hetzner actually exposes

As of **16 July 2026**, I found no documented public Hetzner API endpoint that returns invoice state, payment status, account arrears, or an impending billing suspension. The documented APIs cover infrastructure resources such as servers, IPs, volumes, firewalls, DNS and related products—not customer billing state. Therefore, an implementation that claims to determine “billing clear” from the Hetzner Cloud API is inventing a capability that is not publicly documented. citeturn861440view4

Hetzner’s documented sequence is approximately:

| Event | Earliest observable evidence | Timing |
|---|---|---|
| Invoice created | Billing email and Hetzner Accounts billing page | **T0** |
| Automatic payment attempted | Card/SEPA processing | A few days after invoice creation |
| Automatic payment fails | Failure email | At the failed attempt |
| Payment deadline/reminder | Billing email/account | Payment is expected within 10 days; reminder follows |
| Escalating warnings | Billing email | Hetzner describes four reminders; the third warns of account deactivation |
| Lock/suspension | Product becomes unavailable; provider/account evidence | Exact timing is not published and actions are case-specific |

Hetzner also says that enforcement can include locking individual services or the whole account, and that the timing depends on the case. Bank transfers may take a long time to be processed, making them unsuitable as a last-minute recovery mechanism. Hetzner Accounts exposes open invoices, and account credit can be maintained as a payment buffer. citeturn774493view0turn774493view3turn392955view0turn774493view4

Do not substitute Hetzner “cost alerts” for billing monitoring. Hetzner describes them as convenience notifications and does not guarantee timely delivery; they report estimated resource cost, not whether an invoice was successfully paid. citeturn912203view1

A small practical detail: Hetzner changed its invoice sender address in July 2026. Mail filtering should therefore validate authenticated `hetzner.com` mail and classify content, rather than pinning one exact sender address forever. citeturn774493view0

## Required billing state machine

Use explicit states rather than a Boolean:

```text
UNKNOWN
CLEAR
INVOICE_OPEN
PAYMENT_PENDING
PAYMENT_FAILED
OVERDUE
DEACTIVATION_WARNING
SUSPENDED
TERMINATION_WARNING
```

The safety rule is:

```text
Only CLEAR + fresh evidence permits a cloud power action.
Every other state vetoes reboot.
```

`CLEAR` must be based on **positive evidence**, for example:

- Hetzner Accounts shows zero open invoices.
- A provider payment receipt is matched to the invoice.
- A settled card/SEPA transaction is matched and the provider account no longer lists it as open.

Silence from the mailbox is not evidence of payment.

A useful evidence record is:

```json
{
  "provider": "hetzner",
  "state": "CLEAR",
  "observed_at": "2026-07-16T22:40:00Z",
  "valid_until": "2026-07-23T22:40:00Z",
  "invoice_ids_reconciled": ["R012345678"],
  "open_invoice_count": 0,
  "source": "Hetzner Accounts > Billing",
  "operator": "Anton",
  "raw_evidence_sha256": "..."
}
```

Any later invoice, charge failure, reminder, warning, or suspension mail invalidates that acknowledgment.

## Payment dead-man’s switch

There should be **two independent dead-men**:

### A. Scanner-liveness dead-man

The desktop or second provider node scans the billing mailbox every six hours. After a successful scan, it pings an external dead-man endpoint.

If no successful scan arrives for approximately 30 hours, alert through channels that do not depend on:

- the Hetzner anchor;
- the primary Telegram sender;
- ideally, the same email mailbox being monitored.

Dead-man services are intentionally simple: a periodic task reports success, and a missed report creates an alert. Multiple independent notification channels are preferable. citeturn861440view17

### B. Invoice-payment dead-man

When an invoice is seen, start a deadline that can only be cancelled by positive payment evidence.

A conservative internal policy—not a claimed Hetzner schedule—would be:

| Time from invoice | Fleet policy |
|---|---|
| T0 | Record invoice, notify informationally |
| T+3 days | Amber if no positive payment acknowledgment |
| T+5 days | Page Anton; verify payment method and open invoices |
| T+7 days | P0; sync standby, test secrets, prepare clean handoff |
| T+8 days | Voluntarily hand anchor duties to standby if still unresolved |
| Any payment-failure email | Immediate P0 and standby preparation |
| Any deactivation warning | Immediate planned handoff |
| Any suspension evidence | Provider/account incident; no reboot |

This intentionally moves duties **before** the provider’s 10-day payment boundary.

## Passive payment hardening

For a personal fleet, the highest-value measures are mundane:

- Maintain approximately **2–3 months of Hetzner account credit**.
- Use a dedicated payment method whose expiry is separately monitored.
- Put a recurring calendar check several weeks before card expiry.
- Preserve the customer number, invoice number, payment instructions and support route outside the anchor.
- Do not depend on emergency bank transfer near the deadline.
- If a second provider is introduced, avoid using the same expiring card as the only payment rail for both providers.

## Honest billing limit

You cannot make Hetzner billing perfectly observable with the documented Cloud API. Email can be delayed or filtered, the portal may require human authentication, and Hetzner does not publish a deterministic suspension timetable.

Therefore:

> Billing monitoring reduces probability and gives early warning. **Failover removes billing from the availability-critical path.**

Do not build a brittle browser scraper that logs into the billing portal and attempts autonomous payment. That creates a credential-handling and accidental-money-transfer system more dangerous than the outage it is meant to prevent.

---

# 2. Deterministic “anchor unreachable” triage

## The invariant

**Reboot is a remediation, not a diagnostic probe.**

Evidence collection may run in parallel, but the decision must follow this precedence:

```text
billing/account
    ↓
provider control plane and VM state
    ↓
identity, DNS and network path
    ↓
SSH and guest/service health
    ↓
out-of-band console/provider incident
    ↓
one bounded power action, if explicitly permitted
```

Google’s SRE guidance emphasizes simple, comprehensible critical monitoring, a distinction between symptoms and causes, and a combination of black-box and white-box evidence. Troubleshooting actions and their results should be recorded rather than repeatedly attempted from memory. citeturn861440view16turn604305view6turn604305view7

## Decision tree

### Stage 0 — Freeze mutation

On the first “anchor unreachable” signal:

1. Allocate an `incident_id`.
2. Stop automatic provider mutations.
3. Reject a second reboot/power request associated with the same incident.
4. Collect fresh evidence.

### Stage 1 — Account and billing

```text
billing state not CLEAR
or evidence expired
or billing scanner stale
    => NO REBOOT
       diagnosis = BILLING_RISK or BILLING_UNKNOWN
```

A billing warning is enough to veto. The system does not need to prove that billing caused the current outage.

### Stage 2 — Hetzner read-only API

Use a **Read-only** project token. Hetzner distinguishes read-only from read/write tokens, so there is no reason for the diagnostic process or LLM to hold a power-capable credential. citeturn604305view1

Evaluate:

| API evidence | Diagnosis | Action |
|---|---|---|
| 401/403 | Credential/account/control-plane unknown | No reboot; inspect account/token |
| Server missing | Wrong project, deleted resource or account problem | No reboot |
| `status=off` | VM powered off | One guarded **power-on**, not reboot |
| `starting`, `stopping`, `migrating`, `rebuilding`, etc. | Provider operation in progress | Wait and observe |
| `running` | Hypervisor believes guest is running | Continue to network tests |
| Server `locked=true` | Resource action/restriction exists | Inspect provider action/account evidence |
| Public IP `blocked=true` | Provider network restriction | No reboot; investigate restriction |

The Hetzner model exposes states such as running, off, starting, stopping, migrating, rebuilding and unknown, plus server-lock and network-block fields. These fields are useful evidence, but `locked` or `blocked` alone must not be labelled “billing”: Hetzner uses locks for multiple administrative and abuse-related causes and normally supplies explanatory account/email evidence. citeturn682600view0turn682600view1turn774493view8

Before any second cloud action, verify whether a previous provider action is still pending or failed. Never stack reboots.

### Stage 3 — Identity and network

Check:

1. API-reported IPv4/IPv6.
2. Expected protected Primary IP.
3. DNS A/AAAA records.
4. TCP connection to the health endpoint.
5. TCP connection to SSH.
6. Semantic HTTPS health response.
7. The same probes from at least one independent vantage.

Do not declare the host dead from ICMP alone. A firewall or path may drop ICMP while the service remains healthy.

Interpretation:

| Result | Likely class | Reboot? |
|---|---|---|
| DNS points to old IP | Discovery/config failure | No |
| One vantage fails, another succeeds | Path/ISP/firewall problem | No |
| SSH fails but semantic health succeeds | SSH-specific problem | No |
| API says running, both vantages fail | Continue to provider status/OOB | Not yet |
| Provider status reports incident | Provider failure | No; fenced failover |

Hetzner provides a public status facility and product-specific maintenance notifications for logged-in customers. Those subscriptions are cheap and should be enabled. citeturn774493view9turn102868view2

### Stage 4 — Guest and daemon health

When SSH or the semantic endpoint responds, collect:

```text
boot_id
daemon systemd ActiveState/SubState/Result
restart count
semantic heartbeat age
free bytes and free percentage
free inode percentage
real create/write/fsync/unlink test
cgroup memory.events
memory and I/O PSI
clock synchronized / offset / error bound
ledger tip and fold age
```

Systemd can restart a service on process failure, timeout, watchdog failure and OOM-related termination. `MemoryHigh` is appropriate for normal pressure control, while `MemoryMax` is the final hard stop. Cgroup `memory.events` gives explicit OOM/limit counters; PSI exposes sustained CPU, memory and I/O pressure; chrony tracking provides synchronization and dispersion information. citeturn774493view11turn774493view12turn311918search0turn311918search2turn774493view14

Response precedence:

- **Disk or inode exhaustion:** self-fence writers; rotate only explicitly disposable data; alert human. Reboot does not create space.
- **Filesystem write/fsync failure:** self-fence immediately; do not reboot blindly.
- **OOM-killed daemon:** let systemd restart the daemon once with backoff; alert on recurrence. Do not reboot the VM as the first response.
- **Daemon stopped while guest healthy:** restart the daemon once.
- **Clock unsafe:** self-fence arbiter, fold and outbound roles. Monitoring can remain active.
- **Ledger mismatch or corruption:** stop role effects; rebuild from verified shards or backup.
- **Clean guest but stale semantic heartbeat:** restart only the affected service, then fail over if it loops.

### Stage 5 — Out-of-band evidence

Before a reboot of a `running` but unreachable VM, check:

- Hetzner status/maintenance;
- provider console/VNC;
- rescue console where appropriate;
- whether the kernel is panicked or guest is genuinely frozen;
- whether visible disk recovery/fsck activity is in progress.

Hetzner documents the VNC console as an emergency access path. citeturn769212search4

### Stage 6 — Reboot eligibility

A reboot may be recommended only when **all** of these are true:

```text
billing == CLEAR and evidence is fresh
Hetzner API is reachable
server exists and status == running
server is not locked
public network is not blocked
expected IP identity matches
two independent vantages cannot reach SSH or semantic health
no provider incident or scheduled maintenance explains it
fresh OOB evidence says guest hung or kernel panicked
no disk-full/fsck/OOM diagnosis argues against reboot
no previous power action is pending
no power action has already been attempted for this incident
diagnosis age < 5 minutes
```

Then perform **one** reboot, observe the provider Action to completion, and rerun the full diagnosis. A second reboot must require a new incident decision, not “the first one did not help.”

---

# 3. Reference AK‑47 implementation

I built a stdlib-only reference kit implementing the read-only side of this design.

The core is **475 lines of Python** and provides:

```text
billing-scan    authenticated billing-mail classification
billing-ack     positive human/account acknowledgment
local-health    disk, fsync, systemd, cgroup, PSI, clock, heartbeat, boot ID
triage          ordered diagnosis with reboot vetoes and diagnosis_hash
```

Typical use:

```bash
anchor_guard.py --config /etc/anchor-guard.json billing-scan

anchor_guard.py --config /etc/anchor-guard.json triage \
  | tee /var/log/anchor-triage.json

anchor_guard.py --config /etc/anchor-guard.json local-health
```

The implementation is deliberately **read-only with respect to Hetzner**. It contains no power-on, reboot, rebuild or deletion endpoint.

The write side should be a separate, tiny allow-listed gateway:

```text
power_on <diagnosis-file> <diagnosis-hash>
reboot   <diagnosis-file> <diagnosis-hash>
```

Before acting, that gateway must:

1. Rerun triage itself.
2. Verify the supplied and freshly calculated diagnosis hashes.
3. Require all evidence to be within its TTL.
4. Require the exact allowed decision.
5. Enforce one power action per incident and a cooldown.
6. Record intent, provider response and Action result in the fleet ledger.
7. Redact the provider token from every diagnostic record.

The LLM receives neither the Hetzner write token nor arbitrary shell access to the gateway.

---

# 4. Anchor-duty failover without split brain

## Election is not authority

The existing rule—

> first machine in fixed priority order whose heartbeat is fresh

—is a useful **candidate-selection rule**. It is not a safe authority grant.

A heartbeat cannot prove that the previous holder has stopped. During a partition, both sides can have internally plausible but mutually stale views.

Use the existing committed ledger to create a per-role grant:

```json
{
  "type": "ROLE_GRANT",
  "role": "outbound-primary",
  "epoch": 42,
  "holder": "desktop-hub",
  "lease_ms": 120000,
  "grant_ref": "ledger-commit-seq-or-hash",
  "predecessor_epoch": 41,
  "reason": "anchor-unreachable",
  "evidence_hash": "sha256:..."
}
```

Every effect carries:

```text
(role, epoch, effect_id, grant_ref)
```

Recipients remember the highest committed epoch and reject lower epochs. This is the classic fencing-token pattern: a stale former holder may continue running, but its effects are no longer accepted. Google’s Chubby design uses monotonically increasing lock generations/sequencers for precisely this stale-holder problem and requires services receiving effects to reject stale generations. citeturn392955view7turn774493view16turn392955view4

## Minimal lease behaviour

Illustrative starting values:

```text
lease duration:                 120 seconds
renewal attempt:                 30 seconds
holder quiesces after:           60 seconds without committed renewal
new holder safety margin:        15 seconds after old lease boundary
maximum accepted clock error:     2 seconds
```

These are starting parameters to fault-test, not universal constants.

Rules:

- The holder may operate only after observing its own committed grant.
- Renewal extends authority only when the renewal is committed.
- If renewal validity becomes uncertain, the holder quiesces before nominal expiry.
- A process restart loses its lease, regardless of the contents of local disk.
- Unsafe clock state self-fences all effect-producing roles.
- Epoch numbers are never reused or rolled back.
- A new holder activates only after the old lease/lock-delay and its own committed `epoch+1` grant.

Conservative quiescing during lease uncertainty is standard lease practice; a new primary epoch prevents calls from an old primary from being accepted. citeturn392955view5turn392955view6

## Human super-crown

Anton’s live-presence authority should remain a **separate short lease**, not a Boolean status file:

```json
{
  "type": "HUMAN_PRESENCE_LEASE",
  "session_id": "anton-terminal-...",
  "epoch": 301,
  "last_input_seq": 1782,
  "expires_after_ms": 30000
}
```

It should be renewable only by the trusted interactive session and expire within tens of seconds without input.

A canon, irreversible or money effect requires both:

```text
valid role grant
AND
valid current human-presence lease
```

Failover never transfers or fabricates the super-crown.

---

## Role-specific duplicate avoidance

### A. Reversible proposal arbiter

Use:

```text
effect_id = proposal_id
```

A decision is valid only when:

- it references the active arbiter epoch;
- the proposal is still undecided;
- no valid committed decision for that `proposal_id` already exists.

The first valid committed decision wins. Conflicting local decisions are not reconciled by timestamps; automation stops.

### B. Fold node

Treat folding as a deterministic build:

```text
fold_id =
    SHA256(
        sorted input shard-tip hashes
        || fold algorithm version
        || canonicalization version
        || config hash
    )
```

Write output as:

1. Temporary file.
2. Flush.
3. `fsync` file.
4. Atomic rename.
5. `fsync` containing directory.
6. Commit manifest.

Manifest:

```json
{
  "fold_id": "...",
  "input_tips": ["..."],
  "algorithm_version": 7,
  "config_hash": "...",
  "output_hashes": {"canon.json": "..."},
  "role_epoch": 42,
  "grant_ref": "..."
}
```

Duplicate computation is harmless. Consumers accept only manifests committed under the active epoch.

A hash mismatch is corruption, not a reason to elect another fold node and continue optimistically.

### C. Telegram outbound-primary

Use a durable outbox:

```text
PENDING
  -> CLAIMED(epoch)
  -> ATTEMPTED
  -> ACKED(telegram_message_id)

ATTEMPTED
  -> UNKNOWN_SENT
```

Protocol:

1. Commit outreach intent and stable `effect_id`.
2. Active holder claims it under current epoch.
3. Before the API call, durably commit `SEND_ATTEMPT`.
4. Call Telegram.
5. On a successful response, commit `SEND_ACK` with returned message ID.
6. If the process dies between steps 4 and 5, mark it `UNKNOWN_SENT`.
7. Do not automatically retry an `UNKNOWN_SENT` message.

Telegram’s documented `sendMessage` operation returns a Message on success but does not document an idempotency-key parameter. Therefore a crash after Telegram accepts the request but before the local ACK is durably stored cannot be made exactly once by the sender alone. citeturn774493view15

You must choose:

- **At-most-once:** ambiguous sends are not automatically retried. Possible omission.
- **At-least-once:** ambiguous sends are retried. Possible duplicate.

For unsolicited outreach, at-most-once is generally the less damaging choice. A short visible effect-ID suffix can aid manual reconciliation.

Because Telegram cannot validate your epoch fencing token, the new sender must also wait the full previous lease/lock-delay before sending.

---

# 5. Clean failover and failback

## Failover

1. Deterministic triage emits a fresh evidence bundle and hash.
2. The incumbent cleanly releases, or its lease expires.
3. Candidate synchronizes the committed ledger and relevant role state.
4. Candidate resolves all known `UNKNOWN_SENT` and in-flight records conservatively.
5. Commit `ROLE_GRANT(epoch+1, candidate)`.
6. Candidate observes its own committed grant.
7. Candidate waits the old-holder safety boundary.
8. Candidate starts effects.
9. Every effect rechecks epoch immediately before committing or calling an external sink.

If the existing ledger cannot produce a unique committed role grant while the anchor is down, automated takeover is not safe. The correct result is role unavailability until the ledger or Anton can establish unique authority.

## Failback

Failback must not happen merely because the preferred anchor’s heartbeat reappears.

Use a planned handoff:

1. Recovered anchor enters `RECOVERING`.
2. It has **zero role authority** despite its higher priority.
3. It catches up ledger, shards, fold manifests and outbox.
4. It passes health, disk, memory and clock checks continuously for a soak period, for example ten minutes.
5. Incumbent commits:

```json
{
  "type": "HANDOFF_PREPARE",
  "role": "outbound-primary",
  "from_epoch": 42,
  "to_holder": "anchor-hz1",
  "cutoff_seq": 9182
}
```

6. Incumbent stops claiming work beyond `cutoff_seq`.
7. Incumbent drains internal effects and records ambiguous Telegram sends.
8. Incumbent commits:

```json
{
  "type": "HANDOFF_DRAINED",
  "role": "outbound-primary",
  "epoch": 42,
  "cutoff_seq": 9182,
  "ambiguous_effect_ids": ["outreach-9177"]
}
```

9. Commit a new anchor grant at `epoch=43`.
10. Anchor activates only after observing the drain record, catching up through the cutoff and passing the activation boundary.
11. Desktop becomes standby.

If the desktop dies during handoff, abandon that handoff and use ordinary lease-expiry takeover with another new epoch. Never reuse epoch 42 or shortcut the delay because the preferred machine has returned.

For a personal fleet, **manual or semi-automatic failback is preferable**. Failover protects availability; aggressive automatic failback mostly creates races.

---

# 6. Desktop standby versus a second-provider anchor

## Coverage comparison

| Option | Protects against | Does not protect against | Recommendation |
|---|---|---|---|
| Second VPS in same Hetzner account | Individual host or VM failure | Hetzner account suspension, billing failure, broad region/control-plane failure | Low value for this incident class |
| Always-on desktop hub | Hetzner billing/account/region failure | Home power, ISP, sleep, travel shutdown, domestic hardware failure | First standby to implement |
| Small VPS at another provider | Hetzner billing/account/region and home outage | Shared payment method, shared software bugs, shared secret compromise | Add only after fencing works |

A different-provider entry VM is not inherently expensive: current public entry pricing is approximately **$4/month at DigitalOcean** and **$4.54/month at OVHcloud**, before optional backups, taxes, IP or traffic differences. Exact regional pricing should be checked at purchase time. citeturn604305view4turn604305view5

The second node does not need to run an LLM. It only needs capacity for:

- ledger synchronization;
- deterministic folds;
- the outbound queue;
- health/billing observers;
- the role agent;
- retained logs.

## When the second cloud node is worth it

It becomes rational when several of these are true:

- Unattended recovery objective is below roughly 10–15 minutes.
- Anton frequently travels.
- The desktop cannot be guaranteed wired, powered, no-sleep and boot-recoverable.
- Telegram continuity has real operational or reputational cost.
- A home ISP/power outage overlapping a provider incident is unacceptable.
- Failover and failback drills already pass reliably.
- The additional provider account and payment rail will actually be monitored.

It is probably not worth it when:

- Several hours of outage are acceptable.
- Canon, money and irreversible effects already stop without Anton.
- Telegram outreach is delay-tolerant.
- The desktop is genuinely always-on and UPS-backed.
- The second node would rarely be patched or tested.

A second server introduced before fencing is not redundancy; it is an additional split-brain participant.

## Identity and DNS

Use three separate concepts:

```text
node_id        immutable machine identity
role           arbiter / fold / outbound-primary
discovery name current network location
```

Rules:

- Unique SSH host keys and `machine-id` per node.
- Unique cloud API credentials per node.
- Never clone a full VM image with live node identity or lease state.
- DNS is discovery, not leadership.
- Peers should know at least two endpoint addresses and verify node identity cryptographically.
- Telegram outbound does not require inbound DNS at all.
- A protected Hetzner Primary IP can simplify rebuilding inside Hetzner, but it is not portable to another provider. Hetzner supports retaining/reassigning Primary IPs when configured not to be auto-deleted. citeturn774493view10

## Secret distribution

For this fleet size:

- Use root-readable files or operating-system credential facilities.
- Use per-node credentials wherever possible.
- Put only the read-only Hetzner token on the diagnostic node.
- Keep the write token in the narrow action gateway or offline.
- Never share an SSH private key between nodes.
- The Telegram token must be available to both potential senders, but only the active fenced role may use it.
- Rotate the Telegram token after suspected node compromise.
- Do not invent stdlib-only encryption. Use an established secret-encryption mechanism or manual provisioning.

The second provider should ideally use a different payment rail; otherwise an expired or blocked card remains a correlated failure.

---

# 7. Hosted-anchor failure catalogue

The auto-heal rule should be:

> Automatically heal only a bounded, idempotent, well-diagnosed failure. Otherwise self-fence and alert.

| Failure mode | Cheapest reliable signal | Correct automatic response | Human response |
|---|---|---|---|
| Invoice issued but not acknowledged | Authenticated billing email + invoice state | Start payment deadline | Verify open invoices/payment |
| Automatic payment failed | Authenticated provider failure email | P0 alert; prepare standby | Fix payment immediately |
| Overdue/deactivation warning | Reminder/warning classification | Planned role handoff | Pay/contact provider |
| Account or product suspended | Provider mail/account evidence; blocked/locked resource fields | Self-fence and fail over | Resolve account/provider issue |
| VM genuinely powered off | Read-only Cloud API reports `off` | One guarded power-on | Investigate if repeated |
| Provider action in progress | API transitional/locked state | Wait; suppress duplicate action | Inspect failed/stuck action |
| Host or region incident | Provider status + two external probes | Fenced failover after lease | Follow provider incident |
| Scheduled maintenance | Status subscription/account notice | Pre-stage or voluntarily hand off | None unless service fails |
| One network path broken | Two-vantage TCP/HTTPS probes | Continue on reachable path | Inspect route/firewall |
| DNS or IP drift | API IP versus expected IP and DNS | Normally alert; no reboot | Correct DNS/config |
| SSH daemon failure | Semantic health works but SSH fails | Restart SSH only where safely allowed | Inspect config/auth |
| Anchor daemon crash | systemd state/watchdog | `Restart=on-failure` once/backoff | Inspect repeated crash |
| Daemon deadlock | Semantic heartbeat stale, process still present | Watchdog restart; then fail over | Inspect stack/logs |
| OOM kill | `memory.events`, systemd result, PSI | Restart service once with rate limit | Correct limits/leak/capacity |
| Disk full | Free bytes/inodes and failed fsync canary | Self-fence writers; bounded log rotation only | Decide what can be deleted |
| Read-only/corrupt filesystem | Write/fsync failure, kernel logs/OOB | Self-fence | Repair/restore |
| Clock drift | chrony sync/error threshold | Self-fence all authority roles | Fix NTP/network |
| Ledger lag | Commit-tip age and peer comparison | Stop effects until caught up | Inspect replication |
| Ledger/hash corruption | Hash-chain or fold-manifest mismatch | Stop role; rebuild verified state | Investigate corruption |
| Unexpected reboot | `/proc/.../boot_id` changed | Require a new lease; old lease invalid | Investigate cause |
| API or bot token revoked | Live authenticated no-op/read probe | Stop affected function | Rotate credential |
| Telegram ambiguous send | Durable `ATTEMPTED` without ACK | Mark `UNKNOWN_SENT`; no retry | Reconcile and explicitly resolve |
| Backup silently stale | Restore-test heartbeat and verified tip | Alert | Repair backup path |

---

# 8. Evidence-forced LLM operation

Treat the LLM as an **untrusted planner and summarizer**, not as a sensor or power controller.

## Evidence schema

Every observation should contain:

```json
{
  "evidence_id": "hcloud-server-state",
  "source": "Hetzner Cloud API GET /servers/123",
  "observed_at": "2026-07-16T23:01:03Z",
  "received_at": "2026-07-16T23:01:03.221Z",
  "ttl_seconds": 300,
  "boot_id": null,
  "parsed": {
    "status": "running",
    "locked": false,
    "ipv4_blocked": false
  },
  "raw_sha256": "..."
}
```

For guest evidence, include the guest `boot_id`. A status file from a previous boot is invalid even if its embedded timestamp looks recent.

Use both:

- remote-reported observation time;
- local receipt time.

A remote machine with a bad clock must not extend its own evidence validity.

## Deterministic diagnosis

The policy engine, not the LLM, emits:

```json
{
  "incident_id": "inc-20260716-0017",
  "decision": "REBOOT_VETO_BILLING_UNKNOWN",
  "reboot_candidate": false,
  "recommended_action": "CHECK_HETZNER_ACCOUNTS_BILLING",
  "required_evidence_missing": ["billing-positive-ack"],
  "diagnosis_hash": "sha256:..."
}
```

The LLM may explain this result, but cannot replace it with its own preferred diagnosis.

## Proof-carrying action request

An LLM recommendation should be valid only in a structured form such as:

```json
{
  "requested_action": "reboot_once",
  "incident_id": "inc-20260716-0017",
  "diagnosis_hash": "sha256:...",
  "evidence_ids": [
    "billing-clear",
    "hcloud-server-state",
    "probe-desktop",
    "probe-external",
    "oob-console"
  ]
}
```

The gateway ignores the prose and verifies the predicates itself.

Mandatory rules:

- Missing evidence becomes `UNKNOWN`, not an inferred success.
- Expired evidence is unusable.
- “Node unreachable” is a symptom, not a cause.
- The LLM must not use a cached status file unless the guard says it is fresh.
- A claimed cause must cite the exact live evidence IDs supporting it.
- The LLM cannot run arbitrary `curl` against provider write endpoints.
- The LLM cannot access payment credentials.
- Every action and result goes into the append-only incident record.
- The gateway observes the provider Action result before allowing another action.

This converts the LLM from “agent with root and opinions” into “operator interface over a deterministic state machine.”

---

# 9. Failure-injection acceptance tests

Do not call the design complete until these tests pass:

| Injection | Required outcome |
|---|---|
| Delete/expire billing acknowledgment | Reboot becomes impossible |
| Insert authenticated payment-failure fixture | P0 alert and standby preparation |
| Stop billing scanner | External dead-man alerts |
| Cloud API says `off` | Suggest power-on, never reboot |
| Cloud API token returns 403 | Control-plane unknown; no power action |
| One probe path blocked | No dead-node declaration |
| Both paths blocked but health works locally | Network diagnosis, no reboot |
| Fill disk/inodes | Writer self-fences; no VM reboot |
| OOM-kill anchor daemon | One service restart; repeated failure triggers failover |
| Offset clock beyond threshold | Role self-fences |
| Partition anchor from ledger | Anchor stops before lease expiry |
| Restore anchor while desktop active | Anchor remains `RECOVERING`; no priority preemption |
| Crash sender after Telegram API call, before ACK | Message becomes `UNKNOWN_SENT`; no automatic resend |
| Run fold concurrently on old/new node | Only current-epoch manifest is accepted |
| Kill incumbent during failback | New ordinary epoch takeover; no epoch reuse |
| Present stale status from previous boot | Rejected by `boot_id`/TTL |

Run at least the billing, disk, OOM, partition, ambiguous-send and failback tests after every material protocol change.

---

# 10. What a personal fleet should deliberately not solve

Accept and monitor these boundaries:

1. **No guaranteed pre-suspension signal from Hetzner Cloud API.** Use mail/account evidence, credit buffer and failover.
2. **No exact-once Telegram sending across an ambiguous crash.** Choose omission or duplicate risk explicitly.
3. **No simultaneous availability and split-brain safety when the ledger cannot establish unique authority.** Fail closed.
4. **No protection from account deletion without an off-provider backup and tested restoration.**
5. **No trustworthy autonomous diagnosis when provider API, network and OOB console are all unavailable.** The cause is `UNKNOWN`.
6. **No safe automatic cleanup of an unknown full disk.** Only known disposable paths may be pruned automatically.
7. **No safe autonomous payment system operated by an LLM.**
8. **No benefit from building a second consensus stack beside the existing ledger.**
9. **No need for Kubernetes, service meshes, BGP, anycast, active-active Telegram or a multi-cloud secret broker.**
10. **No reason to optimize failback speed.** Slow, sticky failback is safer.

The personal-fleet target is not five-nines. It is:

- early billing warning;
- bounded role outage;
- no duplicate authority;
- no repeated destructive remediation;
- simple human recovery;
- evidence that survives the failed node.

---

# Deliverables

- [Download the complete anchor-guard kit](sandbox:/mnt/data/anchor-guard-kit.zip)
- [Inspect the 475-line stdlib core](sandbox:/mnt/data/anchor_guard_core.py)
- [Read the deployment README](sandbox:/mnt/data/README-anchor-guard.md)
- [Read the fenced role and failback protocol](sandbox:/mnt/data/fenced-role-protocol.md)

The ZIP includes the Python core, example JSON configuration, systemd service/timer examples, billing scanner timer and the role protocol.

```text
anchor-guard-kit.zip
SHA-256 6f7c6c8d860154223bf0fa2c86f1571d68a031c3876bc1dd235e254da44f3e0f

anchor_guard_core.py
SHA-256 2afdffd6c5af807c8563a68c0318d1726af1c37859f8d2fbdf217270d64e8b2d
```

The central invariant is simple: **a machine may disappear; authority must never become ambiguous.**