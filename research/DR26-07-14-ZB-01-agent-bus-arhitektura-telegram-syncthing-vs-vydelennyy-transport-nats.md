---
dr_id: DR26-07-14-ZB-01
title: "Agent-bus архитектура: Telegram+Syncthing vs выделенный транспорт (NATS/JetStream) при рос"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-ZB-01): Agent-bus архитектура: Telegram+Syncthing vs выделенный транспорт (NATS/JetStream) при росте флота

> Determines when a Telegram+Syncthing message bus stops being sufficient for a small non-technical agent fleet and recommends migrating to a dedicated NATS+JetStream transport via a staged dark-launch/shadow-read process.

## Ключевые выводы
- The migration trigger isn't raw scale — it's needing all four at once: lossless catch-up after long offline periods, explicit acknowledgment, transport-level replay independent of file-sync lag, and cross-platform repairability without admin expertise; this fleet has already crossed that line.
- Telegram Bot API has hard published limits unsuitable as a core bus: 1 msg/s per chat, 20 msgs/min per group, ~30 msgs/s bulk (paid broadcast can raise this to 1000 msgs/s for a fee) — fine for a human mirror, not for machine coordination.
- Telegram MTProto user-session limits are undocumented/unpredictable (FLOOD_WAIT_X, no published per-method ceilings per Telethon docs); reusing a shared user session across nodes beyond tmp_sessions risks AUTH_KEY_DUPLICATED and session invalidation — a structural incompatibility with a parallel multi-node bus role.
- NATS + JetStream (single VPS server, file-backed, not clustered) is the recommended dedicated transport: single static binary, official Windows service support, Homebrew on macOS, <20MB typical RAM, durable streams/consumers, KV with compare-and-set, Object Store, and Ed25519-based NKeys auth.
- Mosquitto/MQTT is the strongest alternative for pure offline-queueing conservatism (durable sessions, QoS1/2) but provides far fewer coordination primitives, leaving more state-machine work in application code.
- Redis Streams and RabbitMQ Streams are weaker fits: Redis's Windows story routes to Memurai/WSL (non-native) and requires idempotent consumers (XAUTOCLAIM can re-deliver); RabbitMQ Streams requires an Erlang dependency and has known Windows service/config issues.
- A real durability debate exists around JetStream: Jepsen's 2025 analysis found acknowledged-write loss under correlated failures (single-source), and a 2026 clustered consumer-loss regression was acknowledged by maintainers — argues for conservative config and explicit backup/restore discipline, not abandoning the platform, especially since the pilot is a single VPS, not multi-AZ.
- Recommended migration shape: dark-launch first (dual-write with old path authoritative), then shadow-read verification (compare Syncthing ledger vs JetStream stream sequence), then selective promotion via gates (losslessness, sleepy-node catch-up, crash recovery, mirror-rate safety, ≥30% custom bus code deleted, one-flip rollback) — Telegram remains the permanent human-readable mirror throughout.
- Transport auth (NKeys/JWT) should not replace semantic governance: keep application-level Ed25519 signatures/human approval for irreversible actions separate from broker-level connect/publish/subscribe authorization.

## Рекомендации / решения
- Proceed with a Phase 2 dark pilot: single VPS, single file-backed JetStream server (no cluster), bound to Tailscale-only interfaces, not clustered or exposed to open internet.
- Start with static NKeys + subject ACLs; defer NATS JWT decentralized auth and leaf nodes until genuinely needed (laptops are sleepy endpoints, not always-on islands).
- Model the bus as three broker-native layers: an events stream (append-only ledger), a KV bucket (rollout manifests/desired state/leader lease via CAS), and an Object Store bucket (versioned artifacts/bundles).
- Keep Telegram as a one-way coalesced/summarized mirror fed from the event stream (not raw broker chatter) to stay under Telegram's group/chat rate limits.
- Keep Syncthing as fallback/rollback during the pilot, not as a co-authoritative rail — only one system (SYNCTHING_PRIMARY or NATS_PRIMARY) should be authoritative at any time.
- Use durable, pull-based, explicit-ack JetStream consumers for sleepy laptop nodes rather than Core NATS pub/sub (which drops slow consumers).
- Establish first-class backup discipline: nightly `nats account backup` plus filesystem backup of the JetStream store, with a periodic restore-to-fresh-VPS recovery drill.

## Сущности
- **Люди:** —
- **Компании:** NATS.io, Eclipse Mosquitto, Redis, RabbitMQ, LaunchDarkly, Microsoft, AWS, Google Cloud, Atlassian, PagerDuty, Jepsen, Telegram, Telethon, Matrix.org
- **Продукты/инструменты:** NATS, JetStream, Mosquitto, MQTT, Redis Streams, RabbitMQ Streams, Telegram Bot API, Syncthing, Tailscale, NKeys, JWT, AutoGen, CrewAI, LangChain Agent Server

## Открытые вопросы
- How severe is JetStream's Jepsen-reported acknowledged-write loss under correlated failures for this specific single-VPS deployment, and does conservative fsync/config fully mitigate it?
- Whether the 2026 clustered consumer-loss regression (GitHub discussion, low-authority/single-source) affects a single-server (non-clustered) JetStream deployment.
- No first-party source found for launchd (macOS) service instructions for either NATS or Mosquitto — only Homebrew install confirmed.
- When (if ever) leaf nodes become necessary if laptops evolve from 'sleepy endpoints' into always-on local islands.
- Whether the emerging NATS-native AI-agent protocol (2026, single-source) is worth adopting once it matures.

## Источник
- DR-ID `DR26-07-14-ZB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-14-ZB-01-agent-bus-chatgpt.md`

## Связано
- [[machine-bus-telegram-rail]]
- [[one-system-propagate]]
- [[multi-machine-auto-consensus]]
- [[vps-anchor-node]]
- [[own-fleet-peer-equality]]
- [[credential-store]]
- [[config-safety-backup-and-migration-check]]
