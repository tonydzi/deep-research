# DR26-07-16-HUB-05 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (Grok, Expert mode, 75 sources)
topic: "DR26-07-16-HUB-05 — Operational resilience & failure-mode hardening of an always-on anchor node"
dr_id: DR26-07-16-HUB-05-2237
date: 2026-07-17


chat_url: https://приватный чат
note: "verbatim; извлечено фолбэком (Blob-download тихо не долетел — грабля скилла), echo-промпт и хвост '75 sources/Expert' отрезаны"
---

Thought for 1m 41s

Ranked hardening steps for the anchor (high-value/low-effort first). These keep the AK-47 spirit: deterministic, human-auditable stdlib Python (or Console config) where possible, no K8s/Raft daemons, evidence-forced decisions, and runnable/understandable by a non-technical owner with basic SSH + text editor skills. The core deliverable is a small family of scripts (one primary "guardian"/triage script ~150-250 lines) plus Console tweaks and ledger extensions.

All steps assume the existing roaming leadership (priority list + fresh heartbeat) and single-writer append-only ledger. The human super-crown remains the ultimate arbiter for canon/irreversible/money changes. LLM agents are forced to cite live probe/API output before proposing any action.

## 1. Billing/payment as first-class monitored failure mode + pre-suspension guard (highest ROI)

**Failures closed:** Billing/payment death, silent suspension leading to wasted reboots, wrong triage order.

**Minimal AK-47 mechanism:**

- **Hetzner Console config (config-only, 5 minutes):** Enable automatic payment (SEPA direct debit or credit card — auto-charges a few days after invoice). Set cost/spend alerts under Account → Billing (email on threshold per project). Invoices are predictable (monthly in arrears, due immediately, reminders after ~10 days). [Wiki.hetzner]
- **Minimal guardian/monitor script** (or GitHub Action / cron on any always-reachable machine): Every 15-60 min, use `urllib.request` + `json` (stdlib) to GET `https://api.hetzner.cloud/v1/servers/{SERVER_ID}` with `Authorization: Bearer $HETZNER_TOKEN`. Parse `status` and `locked` (bool; true when locked, including for billing/account reasons). If `locked=True` or status anomalous (or recent power action failed with 423 locked), immediately Telegram-alert with evidence snippet + "CHECK INVOICES / PAYMENT IN CONSOLE — possible suspension path. Do NOT reboot." Supplement with calendar recurring reminder ("Hetzner invoice due — verify auto-pay or balance") and optional cheap IMAP watcher for `[email hidden]` subject lines (security note: only if you accept credential storage risk). [GitHub +1]
- **Payment dead-man's switch:** the periodic API + lock detection + alerts serve this role. Failed auto-pay triggers email + this catches the downstream lock.

**config-only vs code vs second-node:** Mostly Console + ~80-line reusable stdlib Python (or Action). No second node required.

**Confidence: established** (Hetzner docs + SDKs confirm `locked` field, auto-pay, cost alerts, email reminders, and eventual lock on prolonged non-payment; community patterns exist for API polling). [Wiki.hetzner]

**Honest limit (personal fleet):** No public Hetzner Cloud API endpoint exposes raw invoice list, exact days-overdue, or pre-lock balance (billing lives in Console/Accounts + email). You cannot fully auto-pay or pre-alert at the invoice level without scraping (TOS risk, fragile, stores credentials). Hybrid (auto-pay + alerts + status/lock detection + human calendar) is the practical ceiling. Full pre-suspension automation is not worth the complexity/security surface for a personal setup.

## 2. Deterministic dead-node triage ORDER script with mandatory evidence citation

**Failures closed:** Wrong triage order (rebooting suspended/locked/disk-full/OOM nodes), LLM-agent pitfalls (blind actions), wasted effort on unfixable states.

**Minimal AK-47 mechanism:**

Single standalone (or extended guardian) Python script `anchor-triage.py` with hardcoded order and evidence-forced output:

1. **Account/billing/cloud-API state first (highest priority):** Fetch server object via stdlib HTTPS. Cite raw snippet: `locked`, `status` (running/initializing/starting/stopping/off/deleting/migrating/rebuilding/unknown), recent actions. If `locked=True` or status indicates account-level issue → diagnose "billing/account lock or protection likely — check Console Invoices/emails. Power actions will fail or be meaningless. Evidence: {json}". **Stop here. Never suggest or perform power action.**
2. Cloud power state confirmed via API.
3. Network reachability (socket connect to public IP:22 or simple ping via subprocess with timeout; cite result).
4. Daemon/SSH (banner grab or known port + optional heartbeat file/timestamp in ledger or on-disk).
5. Only then: "All prior checks passed with evidence X/Y/Z. Safe to consider reboot? Human confirmation still required for production."

Output: Human-readable Markdown report with fenced evidence blocks for every probe. LLM agents (or humans) must quote it. Script never auto-reboots destructively (optional `--execute-reboot` flag only for testing or explicit human override).

Run from any always-reachable context (other fleet machine on charger, laptop cron, cheap secondary, or free GitHub Actions). Integrates with existing Telegram outbound.

**config-only vs code vs second-node:** Pure code (~150-250 lines stdlib: urllib, json, socket, subprocess, datetime, argparse + env/config file for tokens/IDs). Reusable across checks.

**Confidence: established** (standard ops triage layering; Hetzner server fields and action semantics are documented in official SDKs and API reference).

**Honest limit:** Deeper daemon health (e.g., specific process liveness) may need minimal in-guest checks or heartbeat files. Pure stdlib SSH is limited (banner or subprocess `ssh -o BatchMode`); for richer interaction consider a tiny audited wrapper or accept "port open + recent ledger activity" as proxy. Never let an LLM agent act without the evidence block.

## 3. Anchor-duty failover/failback with lease/epoch fencing (lightweight, ledger-native)

**Failures closed:** Anchor-duty loss (arbiter/fold/outbound-primary) when primary goes dark; split-brain or duplicate effects on recovery/failover; failback races.

**Minimal AK-47 mechanism** (builds on existing roaming election + single-writer ledger):

- Extend the ledger with a simple lease record type (few lines): `{"type": "anchor_lease", "epoch": N, "holder": "node-uuid-or-priority-name", "expiry": unix_ts, "duties": ["arbiter", "fold", "telegram-primary"]}`.
- **Primary anchor** (on startup + periodic renewal): If no valid unexpired lease or expired, atomically append a claim (single-writer serializes this; existing consensus prevents split-brain).
- **Warm standby / other nodes:** Watch ledger + presence heartbeats. On primary dark + lease expired (configurable TTL, e.g., 1h) + confirmation from ≥2 nodes (or human), claim with incremented epoch.
- **Duplicate-effect avoidance:** Every duty action first reads current lease holder/expiry from ledger and aborts or no-ops if not holder (idempotent folds, timeout proposals already reversible, Telegram outreach can be de-duplicated by message ID or content hash).
- **Failback:** Recovering primary sees expired lease or its own higher priority → reclaims (or waits for human super-crown to transfer cleanly). Human can always force via super-crown + ledger entry.
- **Integration:** Ties into existing priority list for roaming arbiter; duties remain portable.

**config-only vs code vs second-node:** Code extension to ledger/consensus handling (dozens of lines) + config for `lease_ttl`, `standby_candidates`, priorities. Optional cheap warm-standby node (see #4).

**Confidence: emerging** (lease/epoch patterns are classic and proven; implementation here is custom to your single-writer append-only setup and must be tested carefully for races).

**Honest limit (personal fleet):** Robust auto-claim/failback without bugs is non-trivial. For a small personal fleet, start with **manual promotion documented in a runbook**: triage diagnoses dark anchor → human (super-crown) appends lease transfer or promotes next priority → updates any DNS/IP refs. Auto-failover is nice-to-have once the basic guardian + evidence script is solid. Over-automating here risks the exact split-brain/duplication you want to avoid.

## 4. Single-provider / single-instance risk — warm-standby doctrine and when (not) to add a second node

**Failures closed:** Correlated provider outage, account-wide issues, DC maintenance, or full suspension.

**Minimal AK-47 mechanism + contrarian doctrine:**

- **For a personal (non-enterprise) 4–6 machine LLM-agent fleet: do NOT deploy a second dedicated always-on anchor node by default.** This is usually over-engineering. Hetzner Cloud is reliable for the price; **the original incident was a process failure (unpaid invoice + bad triage) more than raw infrastructure fragility.** Doubling the cheap VPS cost adds management surface, another token/secret, potential IP/DNS sync, and marginal uptime gain for most personal use cases. Roaming election + portable duties + the guardian script already give good resilience. Human super-crown exists for crown changes anyway.
- **Better cheap diversity (if desired):** Make duties runnable on a secondary context (Oracle Cloud free tier, home mini-PC/UPS + dynamic DNS, or even a spare always-plugged laptop). Or offload non-critical pieces (e.g., Telegram bot) to external simple hosting/CI.
- **When a second cheap node IS worth it:** High-stakes duties (financial outbound, production money tx, strict automated uptime SLA) or fleet growth. Then adopt warm-standby: second node in different Hetzner location/DC (or different cheap provider), minimal state sync (ledger tail), lease-based promotion. Use floating IP or DNS for outbound if needed. Still keep the guardian/triage on both or external.
- **Hub-covers or multi-anchor:** Only if ops is fun; otherwise document a clear one-page manual failover runbook.

**config-only vs code vs second-node:** Decision is architectural (second-node: usually **no**). Lease code from #3 applies if you do add one.

**Confidence: contrarian but grounded** (personal-scale economics + observed Hetzner reliability; enterprise patterns do not directly translate).

**Honest limit:** True provider diversity or geo-redundancy for a tiny personal anchor is rarely justified by risk vs. ongoing cost/complexity. The bigger wins are fixing billing visibility + triage evidence (steps 1-2). Occasional manual intervention is acceptable and cheaper than constant dual-node ops.

## 5. Hosted-anchor failure catalogue — cheapest detection + selective auto-heal vs alert

**Failures closed:** Silent suspension (covered in 1-2), clock drift, disk-full, OOM, provider maintenance, IP change, general silent dark.

**Minimal AK-47 mechanism:** Extend the single guardian/triage script (or separate lightweight cron job) with cheap probes:

- **Clock drift:** Compare local time to HTTP `Date` header from a reliable server or simple NTP query; alert if >5 min. Auto-heal (optional): `ntpdate` or `chronyc` (with care).
- **Disk-full / inodes:** `shutil.disk_usage` or `df` subprocess; alert ≥85% or inode pressure. Selective auto-heal: prune safe old logs/tmp if policy allows; else alert.
- **OOM:** Parse `/proc/meminfo` or `dmesg | grep -i oom`; alert. Prefer alert over auto-reboot.
- **Provider maintenance:** Poll `https://status.hetzner.com/` (or simple RSS parse) or rely on email announcements; alert on scheduled events affecting your DC.
- **IP change:** Fetch current public IP (e.g., from metadata or external service) vs. known/golden; alert + update floating IP or DNS records if configured.
- **Silent dark / heartbeat:** The guardian itself (or external ping service) + existing fleet presence heartbeats. Ledger tail or on-disk timestamp as cheap liveness signal.

**Philosophy:** Alert + human decision for anything destructive or money-related. Auto-heal only safe, reversible, local actions (service restart, log prune). All alerts via existing Telegram primary (or fallback).

**config-only vs code vs second-node:** Code in guardian script + config thresholds/timeouts. No second node needed for detection.

**Confidence: established** (standard cheap Linux + Python monitoring patterns).

## 6. LLM-agent pitfalls — evidence-forced triage as mandatory context

**Failures closed:** LLM agents proposing blind reboots or actions without diagnosis; hallucinated root causes.

**Mechanism:** The triage/guardian script output (with cited API JSON snippets, probe results, and explicit "Diagnosis: …" + "Evidence: …") becomes **required context injected into any LLM coding-agent prompt that touches the anchor**. Agent must quote the evidence block before suggesting a fix. Combine with human super-crown gate for any canon/irreversible change. Script can be invoked by the agent itself ("run anchor-triage and report back").

This directly prevents the incident pattern.

## Overall implementation order & honest personal-fleet guidance

Start with **1 and 2** (billing guard + evidence triage script) — these would have prevented or immediately diagnosed the exact incident with minimal effort. Add catalogue checks (#5) into the same script. Layer lease/failover (#3) and second-node thinking (#4) only after the basics are solid and tested.

**What a personal fleet should NOT try to solve:**

- Full autonomous pre-suspension billing/payment without human/email/calendar element (API limits).
- Complex auto-failover with zero risk of split-brain or duplicate effects (test thoroughly or keep human-in-loop).
- Dual always-on anchors purely for redundancy in a 4-6 machine personal setup (over-engineering; focus on portability + monitoring + runbooks instead).
- Heavy external deps or orchestration (stick to stdlib + Console + existing ledger).

The result stays deterministic, auditable, and owner-runnable while closing the specific failure modes. **The original incident was largely a combination of missing billing visibility and non-deterministic triage — fixing those two yields the biggest operational resilience jump for the least ongoing cost and complexity.**

Implement the guardian script once; it becomes the single source of truth for "what is wrong with the anchor and what evidence proves it." Everything else builds on that foundation.
