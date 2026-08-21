---
dr_id: DR26-07-28-HUB-17-2339
title: "Safety action-gate architecture for a solo-operator multi-agent stack (MCP + non-MCP)"
date: 2026-07-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-17-2339): Safety action-gate architecture for a solo-operator multi-agent stack (MCP + non-MCP)

> Determines the best safety-gate architecture for deciding execute/require-approval/block on risky actions spanning both MCP calls and non-MCP surfaces (Telegram-as-user, WhatsApp, Gmail, Calendar, n8n, browser automation) for a solo operator, without over-engineering.

## Ключевые выводы
- A pure MCP proxy/gateway is NOT the right primary safety boundary for a solo operator because much of the highest-risk surface (Telegram-as-user, WhatsApp, Chrome/browser automation, direct Python API scripts) never flows through MCP — treating a gateway as 'the safety layer' creates false confidence.
- The recommended architecture is a shared, mandatory action-approval 'chokepoint' function that every risky script/tool wrapper must call before any side effect, regardless of transport or framework — this matches how modern SDKs (LangChain HITL middleware, OpenAI Agents SDK, Microsoft Agent Framework) already gate at the function/tool-execution layer.
- Decision matrix across 4 options (scan-only, shared chokepoint, runtime MCP proxy, full gateway) scored on MCP+non-MCP coverage, new failure points, maintenance, repairability, cost: shared chokepoint wins on coverage (High), repairability (Very high) and cost (Low); full gateway scores High on MCP coverage but Low on non-MCP coverage, High maintenance, Lower repairability, High cost.
- Scan-only tools (e.g. Snyk Agent Scan, formerly Invariant mcp-scan) detect prompt injection, tool poisoning, tool shadowing, toxic flows, and hardcoded secrets, but only as supply-chain/metadata hygiene — they do not mediate live execution and are not a substitute for a runtime gate.
- A runtime MCP proxy (Invariant Gateway/Guardrails, Docker MCP Gateway interceptors) is a valid additive layer for live inspection of MCP traffic only; it does not cover direct Python/API/browser paths unless separately wired, and the proxy itself becomes new attack surface (e.g. CVE-2025-49596, an MCP Inspector RCE from missing client-proxy authentication).
- Build/adopt a full gateway (Microsoft MCP Gateway, IBM ContextForge, Docker MCP Gateway) only when MOST of these are simultaneously true: multiple MCP clients/runtimes need shared server inventory/auth/policy; enough MCP servers that lifecycle/discovery/RBAC become painful per-client; identity-aware access control/centralized audit is needed across machines/users; MCP traffic is important enough that runtime inspection materially reduces risk; willingness exists to standardize non-MCP actions behind the same plane (or accept MCP-only coverage).
- Academic/industry evidence (AgentDojo, tool-description poisoning research, Anthropic/OpenAI/NCSC statements) shows prompt injection is a persistent, unsolved weakness for tool-using agents — no browser agent is immune (Anthropic), it's an 'open challenge' (OpenAI), and agents should be treated as 'inherently confusable deputies' (UK NCSC) — meaning backend execution controls must exist independently of whether the model 'wants' to obey them.
- OpenClaw and Hermes agent gateways explicitly run high-privilege host tools (browser, cron, messaging channels, shell) OUTSIDE a pure MCP hop by default, proving that centralizing only MCP traffic leaves the actual highest-risk side-effect surface uncovered.
- Approval-fatigue research (daily-assistant HCI studies) shows humans develop miscalibrated trust or rubber-stamp behavior when approval prompts fire too often, too early, or too vaguely — approval should occur right before irreversible execution with a compact, structured action summary, not scattered throughout the pipeline.
- Last-12-months trend: MCP security is moving toward centralized auth/governance (Enterprise-Managed Authorization/EMA extension), more runtime scanning/proxy products, and official HITL/tool-approval features in mainstream frameworks — but the parallel incident stream (proxies, SDKs, inspectors, servers all becoming new attack surface) shows no single perfect central control is emerging; the real trend is defense-in-depth with explicit trust boundaries.

## Рекомендации / решения
- This month: build one mandatory library/function (e.g. `guard_action(...)`) taking a structured request (surface, action_type, target, arguments, risk_tags, provenance, dry_run_preview, idempotency_key, undo_plan) that returns exactly one of execute / require_human_approval / block, and make it the ONLY permitted path for any side-effecting code (MCP tool wrappers, Gmail/Calendar/Telegram/WhatsApp/n8n scripts, browser automation, shell execution) — call it BEFORE the action, not after.
- This month: run periodic scan-mode (e.g. Snyk Agent Scan) on all MCP servers and agent skills as baseline hygiene, and re-run after any tool/server/skill change — but never treat scan results as a substitute for the runtime execution gate.
- This month: add sandboxing/containment for the highest-privilege runtimes — model-written/triggered shell/Python in a sandbox, browser automation with logged-in personal accounts in a separate isolated browser/account, and public/shared messaging agents kept on a separate trust boundary from the personal high-privilege runtime.
- Later (optional): add a runtime MCP proxy (Invariant Gateway/Guardrails, Docker interceptors) only if live inspection/rewriting of MCP tool descriptions, arguments and responses is wanted — treat it as additive, not canonical.
- Later (only if threshold crossed): adopt a full MCP gateway (Microsoft MCP Gateway / IBM ContextForge / Docker MCP Gateway) for fleet-scale governance — but KEEP the shared action chokepoint even after adopting a gateway; the gateway is infrastructure governance, the chokepoint remains the last-mile execution gate.
- Keep human-approval prompts narrow and high-value (money, outbound messages, irreversible writes, secrets, account linking, public posting, broad browser submissions) to avoid approval fatigue and silent rubber-stamping.

## Сущности
- **Люди:** —
- **Компании:** Invariant, Snyk, Docker, IBM, Microsoft, Anthropic, OpenAI, LangChain, LlamaIndex, UK NCSC, NSA, NVD, GitHub
- **Продукты/инструменты:** mcp-scan / Snyk Agent Scan, Docker MCP Gateway, IBM ContextForge, Microsoft MCP Gateway, Invariant Gateway/Guardrails, OpenClaw, Hermes Agent, AgentDojo, MCP Inspector, Anthropic Git MCP server, OpenAI Agents SDK, Microsoft Agent Framework, n8n, Claude Code permission modes, approval.py (QQQ)

## Открытые вопросы
- No single canonical schema was prescribed for gating Telegram-as-user or WhatsApp browser automation specifically — the right granularity depends on whether those run via official APIs, browser automation, or OpenClaw/Hermes channels; the report says the chokepoint architecture stays the same but leaves schema-level detail unresolved.
- Product naming/packaging in this space is in flux (Invariant's mcp-scan branding partially shifted to Snyk Agent Scan while Invariant Gateway/Guardrails remain separately published) — verify current vendor/product names before implementation.
- Exact mapping of Anton's existing scattered gates (Tier-2 policy, QQQ phone flow, rate-limiter/kill-switch, Claude Code permission modes) into the single `guard_action()` chokepoint was not concretely specified in the report and needs local design work.

## Источник
- DR-ID `DR26-07-28-HUB-17-2339` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-28-HUB-17-2339-safety-action-gate-architecture-chatgpt.md`
- оригинал: `E:\Obsidian\Anton-Knowledge\01-Conversations\ChatGPT\conversations\2026-07-05-safety-action-gate-architecture-6a499674.md`

## Связано
- [[mcp-security]]
- [[prompt-injection-defense]]
- [[human-in-the-loop-gating]]
- [[tier-2-policy]]
- [[approval-fatigue]]
- [[agent-sandboxing]]
- [[tool-poisoning]]
