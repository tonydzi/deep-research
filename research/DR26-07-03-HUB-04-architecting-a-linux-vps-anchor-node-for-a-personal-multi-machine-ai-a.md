---
dr_id: DR26-07-03-HUB-04
title: "Architecting a Linux VPS anchor node for a personal multi-machine AI agent fleet"
date: 2026-07-03
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-03-HUB-04): Architecting a Linux VPS anchor node for a personal multi-machine AI agent fleet

> The report answers how to design a small always-on Linux VPS as a control-plane anchor for Anton's multi-machine AI agent fleet — covering vault sync scope, Claude Code subscription risk, Telegram user-automation risk, ARM64 viability, consolidation vs. dedicated hosting, secrets, and backups.

## Ключевые выводы
- The VPS should be a minimal always-on control-plane 'anchor and witness' (consensus witness, message-bus shard, watchdogs, approval relay), not a full replica of the personal AI stack or the 349k-file vault.
- Full-vault Syncthing replication is discouraged: real-world reports show 380k-file/24GB trees on ARM Linux took hours to scan (projected 8 days in one case), and Syncthing's databaseTuning kicks in around 200MiB DB size with real RAM/CPU cost; a small dedicated bus/witness share is the recommended default, with receive-only or receive-encrypted modes as options and Tailscale reach-back for anything broader.
- Claude Code's `claude setup-token` gives a one-year OAuth token officially meant for native personal/CI use of Claude Code, NOT for third-party products routing requests through Free/Pro/Max credentials — Anthropic explicitly reserves enforcement rights against the latter.
- Subscription usage is bound by rolling session + weekly allowances that can silently block scheduled jobs if consumed elsewhere in the fleet; there's no publicly documented hard concurrency/device limit, but known issues include OAuth token revocation/expiry, scope mismatches, and version-specific auth bugs — so Claude Code must be treated as non-critical/optional on the anchor, never a dependency for quorum or liveness.
- Telegram's main risk is opaque anti-spam heuristics, not Telethon itself: third-party libraries are flagged as more ban-prone, session files must never be reused across machines/IPs (triggers AUTH_KEY_DUPLICATED), and community reports show VPN/cloud IPs causing friction even during my.telegram.org registration — recommendation is to create/authenticate the session on the same network where it will run, use one stable session per identity, and keep low-risk read/poll automation on the VPS while pushing high-volume/outbound automation to residential/mobile egress.
- ARM64 (Hetzner CAX31) is viable for the named stack: Claude Code, Node.js, Syncthing, Telethon (Python), and Restic all support/run fine on Linux ARM64; the real risk is future dependency drift (x86-only binaries, single-arch Docker images, niche wheels), not the current stack.
- A fresh dedicated VPS is recommended over consolidating with the existing CRM server (production Docker Swarm + MongoDB with sensitive lead/money data) or the n8n VPS, to minimize blast radius given Claude routines run autonomously with no in-run approval prompts.
- Backup strategy: git-bundle and Restic are complementary, not redundant — git-bundle covers repo/history-level recovery, Restic covers host/state-level recovery (etc, cron, SQLite DBs, Syncthing state, Telethon sessions); baseline retention 7 daily/5 weekly/12 monthly with regular `restic check` plus periodic real restore drills.
- Hetzner account lockout is a real, if uncommon, risk — fraud-prevention guidance stresses accurate registration info, avoiding VPN during signup, 2FA, and clean billing, since data isn't guaranteed recoverable after account lock/cancellation.

## Рекомендации / решения
- Provision a fresh dedicated Hetzner CAX31 (ARM64, Ubuntu 24.04), not the CRM or n8n servers, with Tailscale-only SSH/admin access and no public ports.
- Sync only a small Syncthing share (bus/witness files, approval requests, heartbeats) to the VPS first — do not replicate the full vault; add narrow task-specific shards only if a proven workload needs them.
- Implement the consensus witness tick as a plain Python script on a systemd timer/cron (~every 20 min) with zero dependency on Claude or Telegram — this must be the most robust, always-working component.
- Use Claude Code on the VPS only for non-critical, interruptible jobs (summarization, maintenance, triage); wrap every scheduled Claude job in retry/backoff with fail-closed 'queue and notify' behavior; store the setup-token under a dedicated non-root service account.
- Run Telegram automation via a single stable Telethon session created/authenticated on the VPS itself, restricted to low-risk read/poll/approval-relay tasks; move any high-volume or outbound activity to a residential/mobile egress path; never share the session file across machines.
- Stay on ARM64 by default; switch to x86 only when a specific, named x86-only dependency is introduced — don't preemptively pay the x86 cost.
- Layer backups: git-bundle for vault/Git history, Restic (7 daily/5 weekly/12 monthly retention) for VPS host/state, plus reproducible IaC bootstrap docs; schedule monthly test restores, quarterly full rebuild drills, and annual total-loss rehearsals.
- Enforce a hard separation between the agent's service account (which can act) and backup/credential-deletion permissions (which it cannot touch), plus an owner-approval gate for any outbound or irreversible action.
- Register and manage the Hetzner account from a normal (non-VPN) network, enable 2FA immediately, and keep billing/contact info clean to avoid fraud-prevention lockout risk.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Hetzner, Telegram
- **Продукты/инструменты:** Claude Code, Syncthing, Telethon, Tailscale, Restic, Hetzner CAX31, Ubuntu, Node.js, SQLite, Docker Swarm, MongoDB, n8n, Git bundle, systemd, cron

## Открытые вопросы
- No public Anthropic documentation defines a hard concurrency/device limit for subscription (Max) accounts running Claude Code headlessly across a multi-machine fleet.
- No official Telegram documentation confirms datacenter/cloud IPs (e.g., Hetzner) are penalized for Telethon user automation — evidence is community reports and inference from anti-spam posture, not certainty.
- Anthropic's OAuth/setup-token policy and enforcement stance has shown 'churn' in 2026 (GitHub issues on token revocation/expiry, scope mismatches) — long-term stability of headless native Claude Code auth is unresolved.
- Syncthing v2 large-folder scaling behavior (WAL size, long 'preparing to sync' phases) is still an active unresolved debate among maintainers as of 2026.
- Exact threshold at which Telegram account/session friction becomes an active problem (vs. tolerable) on cloud IPs is not quantified — only qualitative risk signals exist.

## Источник
- DR-ID `DR26-07-03-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- machine-bus-telegram-rail
- consensus-active-session-marking
- credential-store
- machine-governance-leader-follower
- telegram-account-identities
- one-system-propagate
- vault-data-architecture
- ak47-simplicity
- insight-DR-DR26-07-02-HUB-14-architecting-an-always-on-linux-vps-anchor-node-fo — сиблинг-DR того же вопроса VPS-anchor для флота
- insight-DR-DR26-07-25-HUB-01-2126-доктрина-размещения-рутин-в-личном-ai-флоте-vps-як — предшествующий DR по архитектуре VPS-якоря флота, прямая основа доктрины размещения рутин
