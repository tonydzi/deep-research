---
dr_id: DR26-07-02-HUB-05
title: "Reliable programmatic extraction of Granola meeting notes and transcripts"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-05): Reliable programmatic extraction of Granola meeting notes and transcripts

> Determines the most reliable way to programmatically back up and incrementally sync Granola meeting notes/transcripts, and whether unattended recording coverage can be guaranteed.

## Ключевые выводы
- Granola's official public API (launched Feb 2026) is now the primary, recommended extraction path, with List Notes, Get Note, List Folders endpoints, API-key auth (created in-app under Settings → Connectors), and a published rate limit of 5 requests/sec sustained; workspace/user API key limit was raised from 5 to 25 on 2026-04-27.
- MCP is explicitly browser-OAuth-only per Granola's docs, and real MCP clients in 2026 show recurring token-refresh failures — fine for interactive analyst use, but not a reliable foundation for unattended nightly ETL.
- Recording coverage, not extraction, is the weak link: Granola does not officially support auto-start for all meetings (feature is 'under consideration'); it only records when a user opens a note, clicks the meeting notification, or starts a Quick Note — app just being open/in tray is not enough.
- Local cache and the old reverse-engineered private API are increasingly brittle and explicitly discouraged by Granola: cache is being encrypted (April 2026 changelog), formats have churned from cache-v3 to v4 to v6, and Granola's own docs point users to the public API and MCP instead.
- Historical CSV export exists but is a poor primary ingestion source: manual request, emailed within hours, includes title/summary/transcript/basic details only for notes you own with a summary, excludes deleted notes, expires in 24h, and can only be generated once per 24h.
- Push-style extraction exists via folder-based auto-post to Slack/HubSpot/Attio/Zapier ('Note Added to Granola Folder', 'Note Shared to Zapier' triggers with rich payloads including transcript) but these are prospective only — they never fire retroactively for notes added before the rule existed.
- Windows is a fully supported platform (installer, auto-updater, version-check endpoints), relevant to multi-machine setups (Windows hub, Windows laptop, macOS assistant).
- An Enterprise/admin API and the ability to force-start capture policy-wide are hinted at in Granola's March 2026 blog and admin settings but are not publicly documented — worth asking Granola directly if Enterprise access is available.
- Competitors (Fathom, Fireflies, tl;dv) have cleaner webhook/API architectures purpose-built for unattended server-side ingestion than Granola, and are the recommended fallback if unattended capture is a hard requirement Granola cannot meet; Otter has a public API but Enterprise-only.

## Рекомендации / решения
- Build the one-time full backfill on the official API: paginate folders/notes via cursor pagination, persist immutable note IDs and sync cursors in SQLite; use CSV export only as a reconciliation/backup check, not as the primary ingestion source.
- Build nightly incremental sync purely on official API keys, respecting the 5 rps cap with exponential backoff on 429/5xx, using periodic re-fetch/watermarks since notes can keep changing after creation (no documented webhook/update guarantee).
- Set up folder-based auto-add for recurring meetings plus a Zapier folder trigger forwarding to your own ingestion endpoint as a pseudo-push channel that reduces polling and gives early-warning if it disagrees with polling results.
- Treat MCP as an interactive-analyst tool only, not for unattended pipelines, due to real-world OAuth/token-refresh fragility.
- Treat local cache/private-API access as a quarantined break-glass fallback only, isolated behind feature flags, since Granola is actively changing cache formats and encrypting the cache.
- Harden recording coverage operationally, not architecturally: OS-level auto-launch on login, process health monitoring, controlled updates via Granola's public installer/version endpoints, and a strict rule of exactly one designated capture-host machine per meeting (no documented dual-device capture semantics).
- Ask Granola directly (if Enterprise channel access exists) whether a documented admin/export API exists and whether policy-based force-start capture is possible — this determines whether the unattended-capture requirement is even achievable on Granola alone.
- If unattended capture truly cannot be guaranteed on Granola, evaluate migrating to a provider (Fathom/Fireflies/tl;dv) with first-class webhooks and server-side API design for that specific requirement, while keeping Granola for its notetaking quality elsewhere.

## Сущности
- **Люди:** Joseph Thacker
- **Компании:** Granola, Zapier, Slack, HubSpot, Attio, Fathom, Fireflies, tl;dv, Otter, Anthropic, OpenAI
- **Продукты/инструменты:** Granola public API, Granola MCP, Granola CSV export, Granola folder auto-post, granola-ts-client, obsidian-granola-sync, Granola Sync Plus, armsteadj1/granola-sync, proofsh/granola-mcp-server, Model Context Protocol (MCP) authorization spec, Claude Code, Codex, Windows DPAPI, macOS Keychain, cache-v3/v4/v6.json, supabase.json, stored-accounts.json(.enc)

## Открытые вопросы
- No public technical documentation for the promised Enterprise-admin API surface hinted at in Granola's March 2026 blog.
- No official definition of conflict semantics when two desktop devices try to capture the same meeting simultaneously.
- No official Granola statement on enforcement/ban risk for using private/reverse-engineered endpoints.
- Whether Granola will ever officially support policy-based/force-start unattended recording across all meetings remains unresolved ('under consideration').

## Источник
- DR-ID `DR26-07-02-HUB-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- second-brain-northstar
- vault-data-architecture
- meeting-transcription-pipeline
- unattended-etl-reliability
- mcp-oauth-token-refresh-limitations
