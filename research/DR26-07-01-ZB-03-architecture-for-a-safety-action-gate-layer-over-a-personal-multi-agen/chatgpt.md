# DR26-07-01-ZB-03 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Safety Action-Gate Architecture"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4adc37-d96c-83eb-9915-25ceb61cb53a
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-05
date_added: 2026-07-07
valid_as_of: 2026-07-05
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 3
related_concepts: []
revisit_if: ""
---

# Safety Action-Gate Architecture

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

DR26-07-01-ZB-03

Conduct a deep research investigation on the following topic.



TOPIC:

Best architecture for a SAFETY / ACTION-GATE layer over a personal multi-agent setup — deciding execute / require-human-approval / block for every risky action — WITHOUT over-engineering. Compare a central "MCP proxy/gateway" chokepoint vs a shared approval-function chokepoint vs per-tool interceptors, for a solo operator whose risky actions span BOTH MCP tool calls AND direct Python API scripts (Telegram-as-the-user, WhatsApp, Gmail, Calendar, n8n, browser automation).



CONTEXT (what exists / what was found):

- Existing gates are SCATTERED: a "Tier-2" policy (money/outbound/irreversible/secrets pause for human OK), a phone approval flow ("QQQ" token via an approval.py), a rate-limiter+kill-switch for auto-posting (arm/disarm), and Claude Code native permission modes. Enforcement is duplicated by convention, not centralized.

- Reference tools studied: Invariant mcp-scan (Apache-2.0, now Snyk Agent Scan) — static scan of tool descriptions for prompt-injection/tool-poisoning + a runtime PROXY mode with guardrails; Docker MCP Gateway (interceptors, container-per-server sandbox); IBM ContextForge; Microsoft MCP Gateway (K8s/enterprise).

- Key risk finding: a big chunk of the actual highest-risk surface (Telegram/WhatsApp/Chrome/raw Python) does NOT flow through MCP tool calls, so a pure MCP-proxy would leave gaps and create FALSE confidence.



OBJECTIVE:

Determine when a central gateway/proxy is justified vs when a shared "single chokepoint function every action-script must call" + periodic tool-description scanning is the right, simpler answer; and how to cover non-MCP action surfaces uniformly.



RESEARCH REQUIREMENTS:

1. Academic — agent safety, tool-use guardrails, prompt-injection/tool-poisoning defenses, human-in-the-loop gating.

2. Industry practice — mcp-scan/Snyk, Docker MCP Gateway, ContextForge, Microsoft MCP Gateway, invariant guardrails, LangChain/LlamaIndex callback gating, OpenClaw/Hermes approaches.

3. Competitive landscape — gateways vs SDK-level policy vs OS-level sandboxing.

4. Failure analysis — where central proxies became single points of failure, silently blocked legit actions, or gave false safety; injection incidents.

5. Emerging trends — last 12 months in MCP security / agent action governance.

6. Implementation recommendations — a scaled answer: minimal now (shared chokepoint + scan mode) vs the fleet-scale threshold where a real gateway pays off; how to also gate the non-MCP Python/Telegram/WhatsApp/browser surface.



OUTPUT FORMAT:

- Executive summary.

- Decision matrix: {shared chokepoint function} vs {scan-only} vs {runtime MCP proxy} vs {full gateway} × {coverage of MCP+non-MCP actions, new failure points, maintenance, repairability, cost}.

- Recommendation with an explicit "build the gateway only when X" threshold.

- What to do THIS month (cheap) vs later.

- Sources.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"task":"Conduct deep research on architecture for a SAFETY/ACTION-GATE layer over a personal multi-agent setup, covering the requested academic research, industry practices, competitive landscape, failure analysis, emerging trends, implementation recommendations, decision matrix, threshold for gateway adoption, and sources."}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-01-ZB-03 Conduct a deep research investigation on the following topic.\n\nTOPIC: Best architecture for a SAFETY / ACTION-GATE layer over a personal multi-agent setup \u2014 deciding execute / require-human-approval / block for every risky action \u2014 WITHOUT over-engineering. Compare a central MCP proxy/gateway chokepoint vs a shared approval-function chokepoint vs per-tool interceptors, for a solo operator whose risky actions span BOTH MCP tool calls AND direct Python API scripts (Telegram-as-the-user, WhatsApp, Gmail, Calendar, n8n, browser automation).\n\nInclude academic research, industry practice (Invariant/Snyk mcp-scan, Docker MCP Gateway, IBM ContextForge, Microsoft MCP Gateway, LangChain/LlamaIndex callbacks, OpenClaw/Hermes), competitive landscape, failure analysis, emerging trends in last 12 months, implementation recommendations, decision matrix, explicit threshold for when to build a gateway, monthly plan, and sources."}}
```
