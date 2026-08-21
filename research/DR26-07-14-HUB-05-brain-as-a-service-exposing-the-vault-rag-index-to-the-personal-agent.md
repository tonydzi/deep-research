---
dr_id: DR26-07-14-HUB-05
title: "Brain as a Service: exposing the vault RAG index to the personal agent fleet"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-05): Brain as a Service: exposing the vault RAG index to the personal agent fleet

> How should Anton expose his hub's vault-search/RAG index to satellite machines and agents — plain HTTP API, MCP server, self-hosted RAG platform, or Syncthing file queue?

## Ключевые выводы
- Recommended core: a lean single-process FastAPI service on the hub exposing /healthz, /readyz, and POST /v1/search, reusing the existing embedding+reranker pipeline as-is (no re-indexing, no architecture change in v1).
- Must run with exactly one Uvicorn worker — multiple workers would each load a separate copy of the index/reranker in memory, which defeats the point of a single shared GPU-backed retrieval service.
- MCP should be added later as an edge adapter (mounted into the same ASGI process at /mcp), not as the primary transport — Claude Code recommends remote HTTP MCP servers, and the Python MCP SDK supports Streamable HTTP and mounting into an existing app.
- A Syncthing file queue is unsuitable as primary transport: watcher changes batch ~10s by default, deletes lag further, periodic rescans still happen, and simultaneous edits create .sync-conflict files — fine for documents, bad for request/response RPC. Keep it only as an emergency fallback.
- Full self-hosted RAG platforms (Onyx, Open WebUI, AnythingLLM) were evaluated and rejected as the core: Onyx Standard needs min 4 vCPU/10GB RAM/32GB disk + ~2.5x indexed data and a heavy multi-component stack (Lite mode drops the indexing stack entirely); Open WebUI and AnythingLLM are full platforms/products, not thin retrieval daemons over an externally-managed index — better used as consumers of a custom API than replacements for it.
- Network/security model: expose only over Tailscale using MagicDNS + tag-based grants (deny-by-default), e.g. tag:agent -> tag:rag-hub:tcp:8787, plus a static bearer token for revocation/audit even though all nodes are personally trusted; Tailnet Lock optional for stronger node-admission guarantees.
- Degradation plan: hub ready -> semantic search; hub reachable but index not ready -> 503 mode=warming; hub unreachable/timeout -> local ripgrep lexical fallback over the Syncthing vault mirror (ripgrep works cross-platform with --json output); never queue requests through Syncthing as normal behavior.
- Windows process management: use NSSM as the service wrapper (not Task Scheduler) for restart-on-crash + Event Log logging, run under a dedicated unprivileged service account (not LocalSystem), and layer Windows SCM 'sc failure' restart actions on top.
- A service manager alone doesn't catch a hung-but-alive process (e.g. wedged in GPU/model code) — add a separate Task Scheduler watchdog every 1-5 min polling /healthz and /readyz, restarting the service after N consecutive failures.
- Rollout sequence recommended: freeze one read-only endpoint -> test locally on hub -> move to Tailscale address with a narrow grant -> install as NSSM service and verify crash+hang recovery -> ship a Python client with fallback -> canary one satellite node for a week (measure latency, false negatives, timeout/fallback rate) -> only then add MCP; keep index updates atomic via a versioned directory + CURRENT pointer.

## Рекомендации / решения
- Ship a single FastAPI process on the hub as the canonical /v1/search interface first; do not touch ingestion/embedding/index layout in v1.
- Do not adopt Onyx/Open WebUI/AnythingLLM as the core retrieval service — only consider Onyx if deliberately replacing the entire stack with a full search platform.
- Gate access with Tailscale tag-based grants (hub tagged rag-hub, satellites tagged agent) plus a bearer token; do not rely on Tailscale app-layer capabilities on day one.
- Implement the ripgrep lexical fallback on every satellite node from the start, clearly labeled mode=lexical-fallback vs mode=semantic.
- Wrap the service with NSSM (dedicated service account, log rotation, AppExit Restart) and add SCM sc failure restart actions, plus a separate scheduled-task health-probe watchdog for hung processes.
- Add MCP (/mcp in the same process) only after the plain HTTP path has run boring/stable for a canary period — do not build MCP as the first transport.
- Keep index updates atomic (versioned dirs + CURRENT pointer/manifest) once the hub becomes shared infrastructure so the service never reads a half-written index.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Tailscale
- **Продукты/инструменты:** FastAPI, Uvicorn, Flask, MCP (Model Context Protocol), Claude Code, Onyx, Onyx Lite, Open WebUI, AnythingLLM, Syncthing, ripgrep, NSSM, Windows Task Scheduler, Windows Service Control Manager (SCM), Tailscale MagicDNS, Tailnet Lock, Pydantic

## Открытые вопросы
- Actual end-to-end latency, false-negative rate vs. manual local search, timeout frequency, and fallback rate once canaried on a real satellite node — not yet measured.
- Whether rerank concurrency should be capped at 1 or 2 concurrent searches under real load.
- Whether Tailnet Lock is worth enabling once the tailnet is stable (node-admission hardening trade-off not fully resolved).
- Long-term question of whether/when to bind directly to the Tailscale IP vs. localhost + reverse proxy scoped to Tailscale traffic.

## Источник
- DR-ID `DR26-07-14-HUB-05` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DL-2026-07-14--Brain-as-a-Service-for-a-Personal-Agent-Fleet.md`

## Связано
- [[vault-data-architecture]]
- [[always-on-memory-pilot]]
- [[system-architect]]
- [[own-fleet-peer-equality]]
- [[machine-bus-telegram-rail]]
- [[credential-store]]
- [[config-safety-backup-and-migration-check]]
- [[insight-DR-DR26-07-07-HUB-08-brain-as-a-service-exposing-the-hub-s-rag-vault-se]] — тот же вопрос Brain-as-a-Service, более поздний DR на ту же тему
