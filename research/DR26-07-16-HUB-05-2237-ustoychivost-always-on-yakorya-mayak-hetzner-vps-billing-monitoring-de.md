---
dr_id: DR26-07-16-HUB-05-2237
title: "Устойчивость always-on якоря (Маяк/Hetzner VPS): биллинг-мониторинг, детерминированный три"
date: 2026-07-16
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-16-HUB-05-2237): Устойчивость always-on якоря (Маяк/Hetzner VPS): биллинг-мониторинг, детерминированный триаж, failover без split-brain

> Deep research answers how to prevent a silent billing death of the always-on Hetzner anchor node and how to safely triage a dark node and fail over/back its duties (consensus arbiter, fold-node, Telegram-outreach-primary) without creating duplicate authority.

## Ключевые выводы
- Billing sits above the VM power plane and must be monitored as infrastructure, not accounting — the actual incident was a process failure (unpaid invoice + reflexive reboot), not hardware fragility
- Reboot must never be the first diagnostic probe — it is a bounded last remediation after explicit preconditions; both vendors independently converge on 'billing != CLEAR = unconditional veto on any power action'
- ChatGPT found no public Hetzner Cloud API endpoint exposing invoice/payment/arrears state as of 16.07.2026 — claiming 'billing clear' from the Cloud API alone is inventing a capability that doesn't exist
- Vendor disagreement resolved by the actual 16.07 incident data: Grok said Hetzner's `locked` field signals billing blocks; ChatGPT said no such API exists. Reality: status was `running` but `ipv4.blocked`/`ipv6.blocked`=true — the API detects the SYMPTOM of a block, not the billing truth itself (which lives only in email + Hetzner Accounts)
- Hetzner 'cost alerts' are resource-cost estimates with no delivery guarantee — must not be substituted for real payment/invoice monitoring
- Mandatory deterministic triage order: billing/account state → cloud-API power state → network/DNS path → daemon/SSH/guest health → out-of-band console → one bounded reboot, each stage evidence-cited
- LLM agents must be treated as untrusted planners, not sensors or controllers: recommendation-only, evidence-forced via diagnosis_hash, no write credential — a separate narrow action gateway holds power-action rights
- Anchor duties (consensus arbiter, fold-node, Telegram-outreach-primary) must attach to a fenced ROLE_GRANT with monotonic epoch + bounded lease, not to a machine — heartbeat only nominates a candidate, it cannot authorize effects
- Both vendors independently reject adding a second always-on anchor node by default for a personal 4-6 machine fleet as over-engineering; ChatGPT ranks it #10 (ROI is contextual, only after fencing is proven), Grok says 'usually no'
- Failback (recovered preferred anchor rejoining) is riskier than failover and must use sticky-incumbent + soak period + drain/cutoff + a brand-new epoch — never reclaim automatically just because a heartbeat reappears

## Рекомендации / решения
- Immediately (before Anton's trip): enable Hetzner auto-payment (SEPA/card) and pre-load 2-3 months of account credit as a buffer — cheapest possible forever-fix
- Add a calendar reminder to check the Hetzner invoice during the 45-day absence; do not rely on emergency bank transfer near a deadline (Hetzner processes it too slowly)
- Build a small stdlib-only anchor_triage.py (~150-475 lines) enforcing the billing-veto-on-reboot rule and the deterministic evidence-cited triage order; never let it hold a Hetzner write token
- Stand up an off-anchor billing-mail monitor that validates authenticated hetzner.com mail by content, not by pinned sender address (Hetzner changed its invoice sender in July 2026)
- Extend the same guardian script with a cheap probe catalogue: clock drift, disk/inode exhaustion, OOM, IP/DNS drift, Hetzner status/maintenance page
- Defer ROLE_GRANT/epoch-fencing and automatic failover until the triage basics are tested; start with a manual promotion runbook (human super-crown moves the lease) before automating
- Do not build: a second always-on anchor node (yet), Kubernetes, a second Raft/consensus stack, autonomous bill-paying, portal scraping, or active-active Telegram sending
- Run failure-injection acceptance tests (billing-ack expiry, API 403, disk-full, OOM-kill, partition, ambiguous Telegram send, failback race) before declaring the design complete

## Сущности
- **Люди:** Anton
- **Компании:** Hetzner, DigitalOcean, OVHcloud, Telegram, Google
- **Продукты/инструменты:** Hetzner Cloud API, Hetzner Accounts, systemd, cgroup/PSI, chrony, anchor-guard-kit, anchor_triage.py, Chubby (Google lock service, cited for epoch/fencing pattern)

## Открытые вопросы
- Whether Hetzner's `ipv4.blocked`/`ipv6.blocked`/`locked` fields reliably signal billing-related suspension in future incidents, or whether this was a one-off signature (only one real data point so far)
- Hetzner's exact suspension/reminder timeline is not officially published — internal deadline policy (T+3/5/7/8 days) is a conservative guess, not a documented schedule
- Whether/when a second-provider standby node becomes justified is stated only as a heuristic checklist (RTO target, travel frequency, desktop reliability), not tested against real conditions
- The proposed ROLE_GRANT/epoch/lease failover protocol is untested in production — failure-injection acceptance tests are recommended but not yet run
- How to reconcile ambiguous Telegram sends (UNKNOWN_SENT) in practice — protocol defined (no auto-retry) but manual reconciliation flow not yet built

## Источник
- DR-ID `DR26-07-16-HUB-05-2237` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- mayak-hetzner-payment-block
- vps-anchor-node
- roaming-leader-operator-crown
- away-mode-45-days
- own-fleet-peer-equality
- machine-governance-leader-follower
- multi-machine-auto-consensus
