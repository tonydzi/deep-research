---
dr_id: DR26-07-07-HUB-08
title: "Brain as a Service: exposing the hub's RAG/vault-search pipeline to a personal agent fleet"
date: 2026-07-07
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-HUB-08): Brain as a Service: exposing the hub's RAG/vault-search pipeline to a personal agent fleet

> What's the best architecture to expose Anton's hub-based vault search/retrieval pipeline to satellite machines (Claude Code and scripts) — plain HTTP API, MCP server, self-hosted RAG platform, or Syncthing file queue?

## Ключевые выводы
- Recommended design: a lean FastAPI service on the hub as the canonical interface, with an optional MCP facade added later for Claude Code and other MCP-capable agents — not MCP or a file queue as the primary transport.
- Plain HTTP works for everything (shell scripts, cron jobs, curl, debugging), not just LLM agents; FastAPI beats Flask here because it gives typed request/response models and auto-generated OpenAPI docs, and Flask's built-in server is explicitly not for production.
- Run a single Uvicorn worker only — multiple worker processes would each duplicate the in-memory index/reranker state, which is exactly what a heavy retrieval service must avoid.
- Full self-hosted RAG platforms (Onyx, Open WebUI, AnythingLLM) are mature but heavier/architecturally wrong as the core: Onyx Standard needs min 4 vCPU/10GB RAM/32GB disk + ~2.5x indexed data and Lite mode drops the indexing stack; Open WebUI and AnythingLLM want to own the RAG environment rather than being a thin daemon over an externally managed index. Open WebUI is however a good *consumer* of a custom hub API/MCP.
- Syncthing should not be the primary transport: filesystem watcher changes batch for ~10s by default, deletes are delayed further, periodic rescans still happen, and simultaneous edits create propagated .sync-conflict files — fine for documents, bad for request/response RPC. Use only as an emergency fallback.
- MCP is worth adding as an edge adapter, not the core transport: Claude Code recommends remote HTTP MCP servers and Streamable HTTP is the standard remote transport, but plain OpenAPI/HTTP remains the preferred path for most deployments; add MCP later in the same process (mount into the existing ASGI app) so the index isn't loaded twice.
- Security model: Tailscale-only exposure (deny-by-default grants, MagicDNS, Tailnet Lock for node admission) is already a strong boundary for a private single-owner tailnet; add a simple bearer token per node/client class for revocation and blast-radius reduction rather than relying on Tailscale app-layer capabilities (non-trivial to implement).
- Latency is dominated by the retrieval pipeline (embedding+retrieval+reranking), not the transport; Tailscale direct UDP paths give lowest latency, DERP relay is fallback; explicit degradation ladder: semantic search → 503 mode=warming if index not ready → local ripgrep lexical fallback on the Syncthing vault mirror if hub unreachable — never queue requests through Syncthing as normal behavior.
- Process/watchdog plan for Windows hub: NSSM as the service wrapper (not just Task Scheduler) with a dedicated unprivileged service account, sc.exe failure recovery actions for restart-on-crash, plus a separate 1-5min health-probe scheduled task (hits /healthz and /readyz) to catch a hung-but-alive process that a service manager alone won't detect.
- Rollout sequence: freeze one endpoint (POST /v1/search, read-only) → run locally on hub → move to Tailscale address with a narrow grant → install as NSSM service and verify crash+hang recovery → ship a client library with fallback → canary one satellite node for a week measuring latency/false-negatives/timeout/fallback rates → only then add MCP → keep index updates atomic via versioned directories + CURRENT pointer.

## Рекомендации / решения
- Build a single-process FastAPI service on the hub exposing /healthz, /readyz, /v1/search — reuse the existing e5-embedding + reranker pipeline exactly once, do not re-index the vault into a third-party product.
- Bind the service to the hub's Tailscale IP (or localhost behind a Tailscale-restricted reverse proxy), tag hub as tag:rag-hub and satellites as tag:agent, and add one narrow grant tag:agent -> tag:rag-hub:tcp:8787; add a static bearer token for revocation even among fully-trusted personal nodes.
- Give every satellite client a short-connect/longer-read timeout and an automatic ripgrep-based lexical fallback (rg --json -F -g *.md) against the local Syncthing vault mirror when the hub is unreachable or not ready.
- Wrap the FastAPI process with NSSM (not raw Task Scheduler) under a dedicated low-privilege service account, configure sc.exe failure restart actions, and add a separate lightweight scheduled-task health-probe watchdog to catch hangs that NSSM/SCM won't detect.
- Defer MCP: ship the plain HTTP API first, let it run boring for a while (canary one satellite node ~1 week), then mount /mcp into the same ASGI process and register it in Claude Code via `claude mcp add --transport http` once the core is proven.
- Keep index updates atomic — build new index versions into versioned directories and atomically switch a CURRENT pointer/manifest so the service never reads a half-written index.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Tailscale
- **Продукты/инструменты:** FastAPI, Uvicorn, Flask, MCP (Model Context Protocol), Claude Code, Onyx, Onyx Lite, Open WebUI, AnythingLLM, Syncthing, ripgrep, NSSM, Windows Task Scheduler, Windows Service Control Manager (SCM), Tailscale MagicDNS, Tailnet Lock, Pydantic

## Открытые вопросы
- Whether app-level Tailscale capabilities are ever worth implementing versus staying with grants + bearer token (report calls this 'not AK-47 simple' and advises against it on day one).
- Optimal rerank concurrency (1 vs 2) — explicitly flagged as something to measure during the canary rollout, not decided in the report.
- Whether/when to eventually adopt a full self-hosted RAG platform (Onyx) if the goal shifts from 'thin wrapper over existing pipeline' to 'replace the whole stack with connectors ecosystem'.
- Exact latency/false-negative/timeout/fallback-rate numbers from the proposed week-long canary test were not yet measured — this was a forward plan, not results.

## Источник
- DR-ID `DR26-07-07-HUB-08` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-07-MACANTON-03-brain-api-chatgpt.md`

## Связано
- [[vault-data-architecture]]
- [[system-architect]]
- [[raise-sync-autonomous]]
- [[machine-bus-telegram-rail]]
- [[own-fleet-peer-equality]]
- [[vps-anchor-node]]
- [[credential-store]]
- [[insight-DR-DR26-07-14-HUB-05-brain-as-a-service-exposing-the-vault-rag-index-to]] — тот же вопрос (Brain as a Service) заголовок почти идентичен, другой вендор/дата
