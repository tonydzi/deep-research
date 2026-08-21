---
dr_id: DR26-07-02-HUB-03
title: "Scaling a decentralized personal AI-agent fleet (multi-machine Claude Code) using distribu"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-03): Scaling a decentralized personal AI-agent fleet (multi-machine Claude Code) using distributed systems patterns

> How should a growing fleet of ~5-30 mostly-offline personal AI-agent nodes (Windows/Mac, Syncthing-linked) evolve its consensus, config management, sync topology, governance, security and scheduling to avoid drift and failure at scale?

## Ключевые выводы
- Hybrid consistency model is standard: leader-based consensus (Raft/Paxos majority quorum, ceil(N/2)) for rare critical/config decisions, CRDTs or log-structured CRDTs (append-only logs merged for strong eventual consistency) for frequent commutative events like routine logs.
- CoNICE model offers a 3-tier approach for intermittent/sleeping peers: gossip layer (async log exchange) -> causal ordering -> consensus layer (majority agreement), allowing local progress while offline nodes catch up.
- Syncthing scales poorly as full mesh; devs recommend hub-and-spoke or expander/star topology (2-3 always-on hubs, spokes connect only to hubs) — 10-30 peers is already pushing peer-based sync limits without this redesign.
- Configuration must move to GitOps/Infrastructure-as-Code (Ansible/Chef/Nix, or scripted Homebrew/Chocolatey + Git dotfiles) with a fleet manifest (component -> SHA256 -> hosts) and nightly drift-detection CI, plus staged/canary rollouts instead of manual edits.
- Flat secret-sharing across all machines doesn't scale — recommend per-machine/role-scoped secrets via Vault or SOPS (age/GPG), per-machine identity keys/certs, and signed task commands (HMAC/asymmetric) so a lost node has limited blast radius and can be revoked/rotated individually.
- Human governance should move from a single QQQ-style approval token to role-based access control with multi-signature/multi-approval for Tier-2 (high-risk/irreversible) actions, plus an audit trail of who approved what.
- Observability needs duty-based heartbeats ('last successfully completed X' per obligation, not just alive-check), fleet dashboards (Prometheus/Grafana, FleetDM, osquery-style), and automated drift/hash verification rather than relying on raw chat logs.
- Telegram-style flat chat logging breaks down at scale (message floods, rate limits, missed alerts) — recommend tiered alerting: only Tier-2/errors go to humans directly, everything else batched into digests or a dashboard.
- Event-driven wake (file-change/inotify/fsnotify triggers) should replace fixed-interval polling to cut LLM/API cost, combined with batching and cheaper models for trivial steps, and heavy jobs scheduled for off-peak/night windows.
- Onboarding should be fully automated via golden images or bootstrap provisioners (PowerShell+Chocolatey on Windows, Homebrew/Ansible on Mac) integrated with GitOps so a new node registers into the quorum and pulls its config/secrets automatically.

## Рекомендации / решения
- At N≈5-7: transition the homegrown consensus engine to an established majority-quorum/leader-election approach (e.g. a Raft library), using vector clocks/causal ordering for routine logs to reduce reliance on heavy consensus.
- Immediately adopt GitOps: put all scripts/configs into version control and build a fleet manifest with SHA256 checksums as a CI drift-check job.
- At N≈5-10: restructure Syncthing into 2-3 always-on hub machines (Send Only) with spokes as Receive Only, capping each node's peer connections to 2-3.
- Start filtering Telegram/chat traffic now — forward only Tier-2 events and errors to humans, batch/log the rest (hourly digest or lightweight dashboard) before chat overload becomes a real problem.
- At N≈5: onboard additional human admins with distinct roles/scopes and require 2 independent approvals for Tier-2 actions (simple email/Telegram multi-OTP now, crypto multi-sig later if needed).
- At N≈5-10: migrate secrets out of flat Syncthing sync into a per-machine encrypted store (Vault or SOPS with age/GPG), stop syncing cleartext secret files.
- At N≈5: build a minimal fleet dashboard (spreadsheet or Grafana) tracking last heartbeat, agent/software version, and last config hash per node.
- At N≈5-10: refactor agents to be event-triggered (file-watch/inotify/webhook) instead of fixed cron polling, with a coalescing queue for rapid updates.
- Now: finalize and test a cross-platform bootstrap/provisioning script so adding a new node takes under 1 hour.
- At N≳10: reconsider the whole architecture — evaluate a local server (SQLite on hub), container orchestration, or commercial multi-agent frameworks (Microsoft AutoGen, LangGraph) as management gets unwieldy, while preserving privacy over cloud offload.

## Сущности
- **Люди:** Wang et al. (2020, CoNICE paper), Saquib et al. (2021, UCSB Log-Structured CRDTs)
- **Компании:** Syncthing, HashiCorp (Vault), Mozilla (SOPS), Microsoft (Autopilot, AutoGen), Keeper Security, FleetDM, Tailscale, NixOS
- **Продукты/инструменты:** Raft, Paxos, CRDTs / Log-Structured CRDTs, Syncthing, Ansible, Chef, Nix, GitOps, HashiCorp Vault, SOPS (age/GPG), Prometheus, Grafana, FleetDM, osquery, Tailscale, Homebrew/Chocolatey, watchdog (Python), fswatch, AutoGen, LangGraph

## Открытые вопросы
- How to handle split-brain scenarios where more than one node believes it is leader after a network partition, beyond quorum-rule detection.
- Whether to keep a fixed hub model vs. dynamic leader election (Raft-style) given mostly-sleeping/intermittent nodes — tradeoff between simplicity and single-point-of-failure risk unresolved.
- At what exact node count (N) the current Syncthing/Telegram-based architecture must be replaced by a message broker (MQTT/RabbitMQ) or federated DB — only rough thresholds (10-30) given, not tested.
- How to prevent a single human's phone/device from being a bottleneck or spoofing risk for Tier-2 approvals without adding excessive friction.
- How to test/simulate split-brain and offline-node conditions in practice before they occur in production (action item flagged, not yet executed).
- Long-term architecture choice at N≳10 (local server vs. container orchestration vs. commercial multi-agent frameworks) left as a future evaluation, not decided.

## Источник
- DR-ID `DR26-07-02-HUB-03` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-02-HUB-03-key-findings.md`

## Связано
- [[machine-bus-telegram-rail]]
- [[one-system-propagate]]
- [[machine-governance-leader-follower]]
- [[multi-machine-auto-consensus]]
- [[consensus-active-session-marking]]
- [[credential-store]]
- [[system-architect]]
- [[config-safety-backup-and-migration-check]]
