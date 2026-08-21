---
dr_id: DR26-07-05-HUB-07
title: "Distributed 'ON AIR' work-declaration board for a small multi-machine AI agent fleet"
date: 2026-07-05
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-05-HUB-07): Distributed 'ON AIR' work-declaration board for a small multi-machine AI agent fleet

> Surveyed cross-industry precedents (LOTO, NOTAMs, Chubby/ZooKeeper/K8s leases, OSS stale-claim norms, multi-agent frameworks, DevOps deploy locks) to design a minimal advisory declaration board so parallel Claude sessions across machines don't silently collide on large structural work, given only Syncthing (eventually-consistent) + Telegram and no central coordination store.

## Ключевые выводы
- The board must be an advisory, coarse-grained declaration layer, not a lock service: true single-writer exclusivity requires a strongly-consistent substrate (Chubby/ZooKeeper/K8s), which this fleet deliberately lacks; Syncthing only resolves conflicts at file granularity, after the fact, via .sync-conflict copies.
- Storage should be one flat file per declaration (e.g. onair/active/<id>.json) with a derived ACTIVE_NOW.md summary as a cache — never a single mutable ledger file, since concurrent edits to one shared file/DB over Syncthing reliably produce conflict copies or corruption (all 3 vendors converge on this).
- 3-mode schema is right for this scale: EXCLUSIVE ("do not touch"), COLLAB ("talk to me first"), FYI ("heads up, proceed") — 2 modes collapses a real distinction, 4+ is taxonomy debt for a 4-6 machine fleet; add a separate board-wide freeze flag later if ever needed, not a 4th per-declaration mode.
- TTLs must be long relative to Syncthing's seconds-to-minutes lag and cross-machine clock skew: ChatGPT recommends 12h (interactive) / 24h (headless) with heartbeat ≈ TTL/4; Grok recommends 24h-7d base TTL with 4-12h heartbeat renewal; for multi-day work, prefer daily renewal of a 24h claim over one long static claim (anti-zombie, anti-squat compromise borrowed from DHCP/Kubernetes renew-before-expiry pattern).
- Holder is normally the only writer who closes/renews their own declaration (straight from OSHA LOTO's "only the applier removes their own lock" rule and lease-ownership norms in ZooKeeper/Kubernetes); others read, reference, or wait for expiry.
- Catalogued failure modes + mitigations: stale/zombie claims → TTL+heartbeat+stale-bot-style warn-then-expire; crashed holders → ephemeral/lease auto-expiry (no manual cleanup); clock skew/eventual consistency → hour-scale TTLs, conservative grace periods; over-claiming → small path-like zone taxonomy (~8-15 zones) with narrowest-scope norm; under-claiming → near-zero-friction declaration hooked into startup/pre-edit/pre-job wrappers; board nobody reads → placement at point of action (session bootstrap, pre-risky-edit, pre-headless-job) plus Telegram lifecycle pings, not a forgotten dashboard; alert fatigue → message Telegram only on create/mode-change/close/expiry/conflict, never per-heartbeat; deadlock between two EXCLUSIVE claims → escalation ladder (direct contact → designated tie-break leader/machine priority list); split-brain during sync outage → irreducible risk, mitigated only by post-reconnect conflict detection + deterministic loser election (later declaration provisionally loses) + fast human reconciliation, never by stronger locking.
- Modern multi-agent AI frameworks (AutoGen, LangGraph, CrewAI, OpenAI Agents SDK handoffs, Anthropic's orchestrator-worker research system) mostly solve task routing/decomposition via a central orchestrator or shared runtime state — they do NOT solve mutual exclusion on shared external artifacts between autonomous peers, which is exactly the gap the ON AIR board fills; the closest architectural precedent is the classic blackboard pattern (shared repository + scheduler deciding who posts next).
- Named precedents worth stealing literally: OSHA 1910.147 LOTO (group hasp, personal locks, shift-handover via direct or supervisor handover, and "pencil-whipping" as the #1 cultural adoption-failure risk); FAA NOTAM system (declaration separated from enforcement, originator accountability, staleness reviewed at 30+ days, known alert-fatigue failure from unprioritized dumps); Google Chubby paper (coarse leases held hours/days, lock-delay, sequencers against reordered messages); Kubernetes Lease objects (holderIdentity/renewTime/leaseDurationSeconds, optional release-on-exit); GitHub's actions/stale bot (warn-then-close, resets on activity) as the honesty mechanism against issue/claim squatting.
- Gemini (partially recovered from its reasoning trail, not verbatim report) proposed a novel decentralized fencing-token idea: Telegram message IDs are globally ordered/unique within a chat, so linking each declaration to a Telegram message ID gives a serverless logical clock for tie-breaking — plus cited Hermes Kanban (decentralized SQLite + active dispatcher reclaiming stale worker claims) as the closest real-world precedent for cross-agent ownership without a central server.

## Рекомендации / решения
- Implement onair/active/<id>.json per-declaration files (schema: id, status, mode, zone, also_touches, holder{person,machine,session_id,agent_kind,contact}, created_at, renewed_at, ttl_sec, links{telegram_thread,journal,branch}) plus a locally-generated ACTIVE_NOW.md cache; archive closed/expired declarations to onair/archive/.
- Keep exactly 3 modes (EXCLUSIVE/COLLAB/FYI); do not add a 4th until 3 demonstrably fails; add board-wide freeze only as a separate global flag if a real need appears.
- Set TTL ~12-24h for interactive work / 24h for headless work with heartbeat ≈ TTL/4 (or 4-12h per Grok), renew on resume, close immediately on clean exit; multi-day work renews daily rather than taking one long lease.
- Wire board-reads into 3 hook points: session startup, pre-risky-edit wrapper, pre-headless-job start — this is the actual "make it read" mechanism, borrowed from LOTO's physical-tag-placement lesson.
- Only push Telegram messages on create/mode-change/significant-extension/close/expiry/conflict — never on every heartbeat — to avoid alert fatigue (per NOTAM/stale-bot lessons).
- Use a deterministic, boring intersection rule (exact zone match, parent-prefix, or also_touches overlap) and an escalation ladder for conflicts: FYI→proceed+link, COLLAB→announce+require ack, EXCLUSIVE→page holder/wait grace/re-declare; on split-brain EXCLUSIVE/EXCLUSIVE collisions, flag both conflicted and let the later declaration provisionally lose, with a fixed machine-priority list or named human as final tie-break.
- Keep the zone taxonomy small (~8-15 durable, path-like zones) to avoid the blackboard/ontology-explosion failure mode.
- Explicitly avoid over-engineering: no mandatory locking, no etcd/ZooKeeper/central DB, no consensus/quorum logic, no replacement of the existing fine-grained per-file lease, no heavy dashboard/UI, no declaring every minor edit (large structural work only).
- Optionally prototype Gemini's Telegram-message-ID-as-fencing-token idea for decentralized ordering, though ChatGPT/Grok's design already converges on a workable scheme without it.

## Сущности
- **Люди:** —
- **Компании:** OSHA, FAA, Google, CNCF/Kubernetes, GitHub, GitLab, Jenkins, Anthropic, OpenAI
- **Продукты/инструменты:** Syncthing, Telegram, Google Chubby, ZooKeeper, Kubernetes Lease, DHCP, GitHub stale-bot/draft PRs/assignees, GitLab merge queue / deploy freeze, Jenkins Lockable Resources plugin, AutoGen, LangGraph, CrewAI, OpenAI Agents SDK, Hermes Kanban, NOTAM/FAA flight-plan system, LOTO / OSHA 1910.147

## Открытые вопросы
- Gemini's Part 2/Part 3 and 'steal this' summary never rendered into the extractable DOM — only Intro→§1.2 is verbatim, the rest is reconstructed from its reasoning trail; the full report still sits unread in Gemini's chat history (title 'Distributed AI Work Declaration Board', 2026-07-05 7:48 PM).
- Whether the Telegram-message-ID-as-fencing-token mechanism is actually needed/implementable, given ChatGPT and Grok's designs converge on a workable escalation-ladder tie-break without it.
- The concrete zone taxonomy (which ~8-15 zones: sync/protocol, canon/skills, content-pipeline, etc.) for this specific fleet has not yet been drafted, only the count range recommended.
- Vendors differ slightly on tie-break mechanics: ChatGPT suggests an ordered machine-priority list with human override; Grok suggests a single designated leader machine (VPS or founder session) — which exact rule to adopt is undecided.

## Источник
- DR-ID `DR26-07-05-HUB-07` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-HUB-07-deep-research-dr26-07-05-hub-6a4aa49e-chatgpt.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-HUB-07-onair-chatgpt.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-HUB-07-onair-gemini.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-HUB-07-onair-grok.md`

## Связано
- [[onair-board]]
- [[coordinate-sessions-before-sensitive-edit]]
- [[machine-bus-telegram-rail]]
- [[machine-governance-leader-follower]]
- [[one-system-propagate]]
- [[ak47-simplicity]]
- [[multi-machine-auto-consensus]]
- [[sync-via-telegram-03-mandatory]]
