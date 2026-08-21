---
dr_id: DR26-07-06-HUB-07
title: "Distributed ON AIR Board for a Small AI Fleet"
date: 2026-07-06
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-06-HUB-07): Distributed ON AIR Board for a Small AI Fleet

> Deep research into how a small fleet of AI agents/machines syncing over Syncthing should coordinate structural work claims without a strongly-consistent lock service.

## Ключевые выводы
- The right design is an advisory, coarse-grained declaration board (like LOTO tags, NOTAMs, GitHub draft PRs/assignees) sitting above the fine-grained file lease and below the after-the-fact journal — not a distributed lock service.
- Syncthing is an eventually-consistent, file-level sync with no strongly consistent coordination store, so the board can reduce collisions and speed deconfliction but cannot guarantee single-writer exclusivity during partitions or stale reads.
- Use one file per declaration in onair/active/ (never a single mutable ledger file) — Syncthing resolves concurrent edits via conflict copies at file granularity, so per-file ownership localizes conflicts.
- The holder should normally be the only one who closes/edits its own declaration; others read, reference, or wait for expiry (per LOTO removal rules and Chubby/ZooKeeper/Kubernetes lease ownership norms).
- Keep exactly three modes: EXCLUSIVE, COLLAB-talk-first, FYI — two is too blunt, four is premature process creep at this scale; add a separate board-wide freeze flag only if truly needed later.
- TTL/heartbeat baseline: ~12h for interactive structural work, ~24h for long headless work, heartbeat ≈ TTL/4, renew on resume/wake, close immediately on clean exit; multi-day work should use daily renewal of a 24h claim rather than a multi-day static claim.
- Keep the zone taxonomy small (roughly 8-15 durable zones, path-like with parent/child prefixes); intersection test = exact match, parent-prefix match, or also_touches overlap — avoid a large ontology (blackboard/repository systems become unreadable when categories proliferate).
- Conflict handling: escalate by mode (FYI=proceed+link, COLLAB=announce+require ack, EXCLUSIVE=don't start, page holder) and use a deterministic tie-break (ordered machine priority list or designated human owner) for split-brain overlapping EXCLUSIVE claims post-reconnect, with the later claim losing provisionally.
- Surface the board at points of action (session start, pre-risky-edit, pre-headless-job) and echo only lifecycle events (create/mode-change/close/expiry/conflict) to Telegram — never heartbeats — to avoid alert fatigue while keeping freshness in files.
- Explicit anti-patterns: don't emulate Chubby/ZooKeeper semantics over Syncthing, don't use one mutable ledger file, don't add 4th/5th modes prematurely, don't use minute-scale heartbeats, don't auto-delete others' live claims, don't build a 50-zone ontology.

## Рекомендации / решения
- Implement onair/active/<id>.json declaration files with schema fields: id, status, mode, zone, also_touches, holder (person/machine/session_id/agent_kind/contact), created_at, renewed_at, ttl_sec, expected_end, links (telegram_thread/journal/branch).
- Derive expiry from renewed_at + ttl_sec rather than storing a separate expires_at field to avoid drift; derive display states (expiring/stale/expired) in the renderer rather than storing them.
- Generate ACTIVE_NOW.md as a cached human-readable summary, not the source of truth.
- Wire declaration checks into session bootstrap, pre-risky-edit wrappers, and scheduled/headless job start; wire lifecycle Telegram notifications for create/mode-change/extension/close/expiry/conflict only.
- Archive closed/expired declarations to onair/archive/ for audit without requiring merge into the active record.
- If a designated tie-break leader machine is used, apply it only for deterministic loser election after a conflict is detected (e.g., ordered priority list like vps > founder-laptop > others), never as a hidden central coordinator; always allow human override.

## Сущности
- **Люди:** —
- **Компании:** OSHA, CCOHS, FAA, Google, GitHub, GitLab, Jenkins, Anthropic, OpenAI, Microsoft
- **Продукты/инструменты:** Chubby, ZooKeeper, Kubernetes Leases, Syncthing, flock(), DHCP, AutoGen, LangChain, LangGraph, CrewAI Flows, OpenAI Agents SDK, GitHub actions/stale, GitHub merge queues, GitLab deploy freeze windows, Jenkins Lockable Resources plugin, client-go leader election

## Открытые вопросы
- Whether a lightweight acknowledgement pattern for COLLAB mode (named coordinator + collaborator attach via Telegram ack) is needed in practice — flagged as emerging, not established.
- Whether a board-wide freeze flag will actually be needed — deferred until a real 'nobody touches prod-like shared infra' scenario arises.
- Whether path-aware prompts in wrappers ('you are about to touch X, declare or acknowledge?') meaningfully reduce under-claiming — flagged as speculative convenience layer.
- How the design should evolve if the fleet grows beyond current small scale — speculated next upgrade is better observability/conflict surfacing, not faux-consensus, but unverified.

## Источник
- DR-ID `DR26-07-06-HUB-07` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-06-HUB-07-distributed-on-air-board-for-a-small-ai-fleet.md`

## Связано
- [[onair-board]]
- [[machine-bus-telegram-rail]]
- [[coordinate-sessions-before-sensitive-edit]]
- [[machine-governance-leader-follower]]
- [[multi-machine-auto-consensus]]
- [[one-system-propagate]]
- [[ak47-simplicity]]
- [[consensus-active-session-marking]]
