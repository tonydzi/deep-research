---
dr_id: DR26-07-01-ZB-03
title: "Architecture for a safety/action-gate layer over a personal multi-agent setup (MCP gateway"
date: 2026-07-01
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-01-ZB-03): Architecture for a safety/action-gate layer over a personal multi-agent setup (MCP gateway vs shared chokepoint vs interceptors)

> ⚠️ **UPDATE 2026-07-14:** эта заметка была сгенерена по ПРОМПТУ (тела отчёта тогда не было — ChatGPT не отдаёт DR через экспорт). Полный отчёт СОБРАН 14.07 (Export→Markdown), лежит в `_originals\deep-research\` (путь в «Источник»). Актуальный синтез: decision-agent-stack-dr-bundle-synthesis-2026-07-14.


> This document only captures the deep-research request being dispatched to ChatGPT's Deep Research tool; no completed findings, analysis, or recommendations were returned in the report text.

## Ключевые выводы
- The report body consists solely of Anton's research brief and the tool-call stubs that hand the query to ChatGPT's Deep Research App — no synthesized findings, decision matrix, or sourced conclusions are present in this text.
- The brief itself notes that existing safety gates are scattered: a Tier-2 policy (money/outbound/irreversible/secrets pause for human OK), a phone approval flow (QQQ token via approval.py), a rate-limiter+kill-switch for auto-posting, and Claude Code native permission modes — enforcement duplicated by convention, not centralized.
- The brief flags a key risk: a large share of the highest-risk action surface (Telegram/WhatsApp/Chrome/raw Python scripts) does not flow through MCP tool calls, so a pure MCP-proxy/gateway would leave gaps and create false confidence.
- Reference tools named in the brief for further research (not yet evaluated in this text): Invariant mcp-scan (Apache-2.0, now Snyk Agent Scan, static scan + runtime proxy/guardrails mode), Docker MCP Gateway (interceptors, container-per-server sandboxing), IBM ContextForge, Microsoft MCP Gateway (K8s/enterprise).

## Рекомендации / решения
_нет_

## Сущности
- **Люди:** Anton
- **Компании:** OpenAI (ChatGPT), Invariant, Snyk, Docker, IBM, Microsoft
- **Продукты/инструменты:** mcp-scan / Snyk Agent Scan, Docker MCP Gateway, IBM ContextForge, Microsoft MCP Gateway, approval.py (QQQ token flow), Claude Code permission modes, LangChain/LlamaIndex callbacks (referenced, not analyzed)

## Открытые вопросы
- The actual deep-research output (executive summary, decision matrix, gateway-adoption threshold, monthly action plan, sources) was not captured in this vendor section and needs to be retrieved/re-run to get real findings.
- Whether a shared chokepoint function + periodic tool-description scanning is sufficient vs when a full runtime MCP proxy/gateway becomes justified remains unanswered pending the actual report.
- How to uniformly gate non-MCP action surfaces (Telegram-as-user, WhatsApp, Gmail, Calendar, n8n, browser automation) alongside MCP tool calls is posed as the core question but not yet answered in this text.

## Источник
- DR-ID `DR26-07-01-ZB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- Tier-2 policy / remote-approval-qqq
- machine-bus / multi-agent orchestration
- MCP security (tool-poisoning, prompt-injection)
- agent-stack safety architecture (DR-C bundle)
- insight-DR-DR26-07-28-HUB-17-2339-safety-action-gate-architecture-for-a-solo-operato — тот же исходный DR-брифинг (safety/action-gate), наш сирота — его завершённый ответ
