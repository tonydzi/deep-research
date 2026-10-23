---
dr_id: DR26-07-02-HUB-14
title: "Architecting an always-on Hetzner ARM64 VPS as a narrow control-plane anchor node for a pe"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-14): Architecting an always-on Hetzner ARM64 VPS as a narrow control-plane anchor node for a personal multi-machine AI-agent + Syncthing + Claude Code + Telegram system

> Deep research on whether/how to stand up a dedicated Linux VPS as an always-on coordination anchor for a personal multi-machine Claude Code / Syncthing / Telegram-userbot setup, covering vault replication, Claude Max auth risk, Telegram automation risk, ARM64 viability, consolidation risk, and backup design.

## Ключевые выводы
- Best architecture is a narrow 'control-plane anchor' (scheduler, watchdog, relay, inbox poller, approval gate) — NOT a second full hub; canonical vault, GPU inference, and high-context work stay on the home desktop.
- A full Syncthing vault replica (350k files/25GB) is technically possible on 16GB RAM but operationally risky: Linux defaults to ~8192 inotify watches (needs raising to ~204800), and many-small-files trees are the worst case — one report shows 380k files/24GB taking ~8 days on ARM64. Verdict: no full replica initially; use Tailscale reachability to the home hub, and if replication is ever needed, use a small receive-only curated subset (receive-encrypted only for opaque offsite storage, which makes the vault unusable to agents).
- Claude Code on a Max subscription: `claude setup-token` officially provides a 1-year OAuth token for scripts/CI, but Anthropic's legal docs frame Max limits as 'ordinary individual usage' rather than a blanket license for an always-on service. 2025-2026 operator reports show 401 token expiry, refresh races, quota exhaustion, and scheduled-task infra outages (e.g. April 2026) — treat Claude Code as best-effort, not a set-and-forget daemon.
- Telegram user-account (Telethon) automation: Telegram/Telethon guidance warns third-party libraries risk account limits, anti-spam has tightened since 2023, and session revocations/24h lockouts correlate with hosting environment/IP. Safe-on-VPS classes: read-mostly inbox polling and owner-approved replies on an established, 2FA-enabled account. Risky classes (bulk scraping, cold outreach, first-session creation, rapid join/leave) should stay on a residential/home node.
- ARM64 (Hetzner CAX31) has no first-order blocker for the named stack (Node.js, Telethon, Syncthing, restic, Tailscale, systemd, SQLite); the real risk is future dependency drift (native addons, browser automation, x86-only helper binaries). Stay ARM64 unless a specific blocker is later found.
- Do not colocate the anchor on the CRM VPS or the n8n VPS — least-privilege/segmentation guidance (OWASP, national cyber agencies) says separating experimental agent automation from higher-value business data keeps blast radius small. A fresh dedicated VPS with a short service list is simpler to reason about for a non-technical solo owner.
- Existing nightly git-bundle→Google-Drive backup only covers Git-tracked history, not Syncthing configs/DBs, systemd units, .env files, Claude job logs, Telethon sessions, SQLite queues, or system config. Restic (to B2 or a Storage Box) should be added as a complementary layer with policy retention (e.g. daily14/weekly8/monthly12/yearly2), monthly `restic check --read-data-subset`, and quarterly full restore drills; append-only repos need a separate admin client for forget/prune.
- Hardening recommendations: Tailscale-first access (SSH bound to tailnet interface, check-mode re-auth for sensitive connections), per-service non-root systemd units with sandboxing (NoNewPrivileges, ProtectSystem, ProtectHome, PrivateTmp), and separate Unix service accounts (anchor-core, telethon, claude-jobs).
- Hetzner account-lockout risk mitigation is mostly hygiene: verified identity at signup, no VPN during account creation, 2FA + support OTP, prompt billing, avoiding abuse-adjacent workloads; Hetzner offers 24/7 ticket support for cloud products.

## Рекомендации / решения
- Provision a fresh, dedicated Hetzner CAX31 ARM64 VPS running Ubuntu 24.04 as the anchor — do not consolidate onto the existing CRM or n8n VPS.
- Do not replicate the full Syncthing vault to the VPS at rollout; rely on Tailscale reachability to the home hub, adding only a small curated receive-only subset later if proven necessary.
- Run Claude Code via `claude setup-token` behind a wrapper enforcing one job at a time, hard timeouts, fail-closed on 401/usage exhaustion, and idempotent retry-once logic; use your own cron/systemd timers instead of relying on Anthropic-hosted scheduled tasks.
- Restrict Telegram user-account automation on the VPS to read-mostly/approval-gated workflows on an already-established, 2FA-enabled account; keep bulk/outreach/scraping/session-creation on the home machine; hard-stop and notify the owner on any SessionRevokedError.
- Harden the box with Tailscale SSH (tailnet-only, check mode) and non-root, sandboxed systemd units per service (separate Unix users for anchor-core, telethon, claude-jobs).
- Add restic backups (to B2 or Storage Box) alongside the existing git-bundle scheme, with defined retention, monthly integrity checks, and quarterly restore-to-throwaway-VM drills.
- Follow the phased rollout: control-plane baseline (queue/heartbeats/approvals, tested with home hub powered off) → guarded Claude Code → Telegram passive polling then approved replies → vault-access decision → backup/restore discipline last.
- If Max-based Claude Code automation becomes mission-critical, noisy, or unstable, migrate the unattended parts to formal API-key billing rather than adding more workarounds.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Hetzner, Telegram, OWASP, Synology, Tailscale Inc., Backblaze
- **Продукты/инструменты:** Claude Code, Claude Max/Pro, Syncthing, Telethon, Tailscale SSH, restic, Hetzner CAX31, Ubuntu 24.04, systemd, SQLite, Node.js, Backblaze B2, Hetzner Storage Box

## Открытые вопросы
- No authoritative Anthropic statement explicitly blesses or forbids a solo owner running a permanently scheduled personal Claude Code fleet on Max from a datacenter IP — policy read here is conservative inference.
- No official Telegram statement classifies Hetzner/OVH IP ranges as specifically high-risk for user-account automation; conclusion is based on anti-spam guidance plus anecdotal field reports, not a provider blacklist.
- Real-world Syncthing performance for this specific 350k-file vault (small-file mix, churn rate, disk latency) remains untested and could differ from cited community benchmarks.
- Optimal Claude Code job concurrency before hitting session-limit or quota problems has not been tested for this owner's specific workload.
- Whether/when a full vault replica ever becomes worth the operational cost is unresolved — framed as a cost-benefit question to revisit later, not a fixed answer.

## Источник
- DR-ID `DR26-07-02-HUB-14` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- machine-bus-telegram-rail
- credential-store
- telegram-account-identities
- desktop-max-laptop-min
- machine-governance-leader-follower
- session-machine-tagging
- model-routing-sonnet-grunt
