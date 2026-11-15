---
dr_id: DR26-09-15-MACANTON-01-0806
title: "Nango vs Composio/Paragon/Merge/Pipedream/Arcade/Klavis — unified auth layer for AI agents"
date: 2026-09-15
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-09-15-MACANTON-01-0806): Nango vs Composio/Paragon/Merge/Pipedream/Arcade/Klavis — unified auth layer for AI agents

> Compares Nango against 12+ 'unified integration/auth layer for AI agents' competitors to decide whether the 5-machine Claude Code fleet should replace its hand-rolled Gmail/Calendar/Trello/Calendly connectors with one layer, plus prep material for a Nango CTO conversation.

## Ключевые выводы
- Nango is the only major player in this product class with a genuinely self-hostable core (Docker, Postgres-backed per-connection encrypted token storage, auto-refresh, proxy passthrough) — the decisive constraint for keeping secrets on the fleet's own VPS.
- Recommended architecture: self-hosted Nango on the Linux VPS + a thin custom MCP wrapper (~150 lines, Node + MCP SDK) exposing Nango's REST/proxy API as tools to Claude Code on all 5 machines, replacing hand-rolled Gmail OAuth, Google Calendar, Trello and Calendly connectors.
- Telegram MTProto (Telethon) user sessions and the Chrome-extension MCP cannot be represented by ANY vendor in this class — none manage MTProto session files or local browser automation; both must remain custom nodes, with Telethon sessions centralized on the VPS.
- Composio, Arcade.dev and Klavis AI are MCP-native and fastest to wire (native tool schemas, less setup) but store secrets in vendor cloud, not self-hostable — good as a one-machine pilot/hedge, not as the primary layer given the secrets-on-VPS requirement.
- Merge.dev, Paragon, Nylas, Unified.to and Apideck are category-constrained (HR/payroll, embedded iPaaS, email/calendar-only) or cloud-only proprietary, scoring low (1-3/5) fleet-fit for a generic agent connector layer.
- Pipedream Connect has the largest claimed catalog (~2,600+ APIs, unverified) and hosted MCP, but is cloud-only with per-task billing — disqualifying given the token-custody requirement.
- Nango's license is disputed/unverified: both vendor passes recall Elastic License 2.0 (not AGPL as the original prompt hypothesized), with an earlier Apache-2.0 origin per GLM's training memory; ELv2 permits internal self-hosting without restriction, only bars re-offering Nango itself as a managed service.
- Nango has no native first-class MCP server (per GLM's last training knowledge) — a custom wrapper is required to expose it to Claude Code, unlike Composio/Arcade/Klavis/Zapier MCP which ship MCP natively.
- The biggest strategic risk to the whole product class is commoditization by identity giants and platforms (Auth0 Token Vault, Okta Cross-App Access, Cloudflare Agents+OAuth, Anthropic's MCP registry, OpenAI's Apps SDK) — the real moat is connector-maintenance depth and sync engines, not 'auth for agents' per se.
- GLM's entire report ran on ZERO live web searches/URL opens this session (training-knowledge only, cutoff ~mid-2025) — every funding figure, pricing number, catalog count, and license claim is explicitly flagged unverified/no-source-found; Gemini's section (only partially captured/truncated) gives some differing concrete numbers (e.g. 900+ APIs, $0.29/connection/mo) that were not cross-verified against GLM's.

## Рекомендации / решения
- Adopt self-hosted Nango on the VPS + custom thin MCP wrapper as the primary architecture (Option A); run a one-machine Composio cloud pilot in parallel as a hedge (Option B).
- Before committing, re-verify live (via web search) Nango's exact license SPDX, current catalog size/AI-provider coverage, pricing tiers, and self-host feature scope — none of GLM's numbers were checked against a live source.
- Budget for Google OAuth verification pain: Gmail scopes are restricted, so use an 'internal' Google Workspace app if available to avoid the 7-day refresh-token expiry that applies to unverified apps in testing mode; otherwise plan for the CASA security assessment.
- Keep Telethon (Telegram MTProto) sessions and the Chrome-extension MCP as separate custom nodes centralized on the VPS — no vendor in this class can absorb them.
- Follow the 1-day adoption runbook: stand up Nango via Docker on the VPS → register a Google OAuth app with Gmail+Calendar scopes → connect Trello and Calendly → write the MCP wrapper → register it in Claude Code on all 5 machines via `claude mcp add` → migrate off hand-rolled tokens → centralize Telethon sessions.
- Prepare the 5 hard questions for Nango's CTO (license drift vs self-host demographic, what's cloud-only today and its 12-month roadmap, why AI/LLM providers are absent from the catalog, first-class MCP roadmap, slimming self-host footprint for small fleets) plus 5 DevRel-pitch offerings (reference repo for the migration, providers.yaml contribution drive for AI providers, docs fixes, self-host guide/benchmark content, community ops).

## Сущности
- **Люди:** Bastien Beurier, Robin Guldener, Alex Salazar, Soham Ganatra, Gil Feig, Shobhit Bakshi
- **Компании:** Nango, Composio, Paragon, Merge.dev, Pipedream, Arcade.dev, Unified.to, Apideck, Klavis AI, Membrane/Integration.app, Truto, Panora, Supaglue, Vessel, Finch, Knit, Nylas, Anon, Keet, Tray.io, Workato, Zapier, Cloudflare, Auth0, Okta, Stytch, Descope, Pica, Toolhouse, Anthropic, OpenAI
- **Продукты/инструменты:** Nango, Composio, Paragon ActionKit, Merge.dev, Pipedream Connect, Arcade.dev, Unified.to, Apideck Vault, Klavis AI, Zapier MCP, Auth0 Token Vault, Okta Cross-App Access, Cloudflare Agents+OAuth, Model Context Protocol (MCP), Claude Code, Telethon, Temporal, PostgreSQL, Redis, Docker, RFC 9728, RFC 7591

## Открытые вопросы
- Exact current Nango license SPDX (Elastic 2.0 vs AGPL vs Apache) and the date of any license change — GLM, Gemini and the original prompt hypothesis all disagree/are unverified.
- Nango's actual current provider catalog size and whether Mistral/DeepSeek/Groq/Together/Fireworks/Cohere/Hugging Face are present — not verified live in either captured vendor section.
- Current Nango pricing (self-host vs cloud tiers, per-connection cost) — GLM found no concrete numbers; Gemini's $0.29/connection/mo figure is uncorroborated.
- Whether Nango has since shipped a native MCP server — flagged as a roadmap gap in GLM's training-cutoff knowledge, needs re-check before the CTO conversation.
- Funding history and headcount for Nango and most competitors — flagged unverified/best-recollection-only across the board.
- The Gemini vendor section (comparative table, fleet-fit analysis, 1-day plan, risks) was truncated in the captured harvest and could not be fully cross-merged with GLM's conclusions.

## Источник
- DR-ID `DR26-09-15-MACANTON-01-0806` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- Model Context Protocol (MCP)
- OAuth token custody
- self-hosted vs SaaS integration layer
- Claude Code fleet connectors
- agent-auth commoditization
- Telethon MTProto sessions
