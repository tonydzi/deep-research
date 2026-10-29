---
dr_id: DR26-07-28-HUB-15-2339
title: "A2A protocol and the emerging agent-interoperability stack (vs original agent's overstated"
date: 2026-07-28
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-15-2339): A2A protocol and the emerging agent-interoperability stack (vs original agent's overstated claims)

> Deep research checked Anton's AI agent's claims about Google's A2A protocol and the broader agent-interoperability stack (A2A, MCP, AGNTCY, ANP, AP2), correcting dates and separating what is actually standardized from what is still overstated marketing.

## Ключевые выводы
- Original agent's timeline was wrong: Google announced A2A on 2025-04-09 (not with immediate LF handoff), donated it to the Linux Foundation on 2025-06-23, and A2A v1.0.0 shipped 2026-03-12 — not January 2026 as claimed.
- The '150+ organizations' adoption milestone is accurate but dated 2026-04-09, including Google, Microsoft, AWS, Salesforce, SAP, ServiceNow, Workday, IBM.
- A2A genuinely standardizes: agent discovery via Agent Cards, task-oriented multi-turn messaging, streaming, push notifications, framework-agnostic delegation — built on HTTP, JSON-RPC 2.0, SSE, and (in v1.0) gRPC.
- A2A is explicitly complementary to MCP, not a replacement: MCP standardizes agent-to-tool/resource access, A2A standardizes agent-to-agent peer communication and delegation across trust boundaries.
- Trust layer is only partial: Agent Cards MAY be signed (JWS + RFC 8785 canonicalization) but signing is not mandatory — decentralized trust/discovery is NOT 'basically solved' as the original agent claimed.
- Registry, validation tooling, identity-verification (cryptographic proof of 'who an agent really is'), governance, and large-scale coordination remain open/roadmap items, not closed problems — confirmed by A2A's own roadmap and a March 2026 identity-verification proposal.
- Independent 2026 research (threat-modeling paper, governance-gap paper, Cisco/MIT Ripple Effect Protocol paper) argues A2A/ACP-class protocols are still just a 'messaging layer' lacking true coordination primitives for large agent populations, and protocol-level security assessment remains immature.
- Adjacent stack has distinct roles, not one solved layer: AGNTCY (Linux Foundation, Cisco-driven) = infrastructure plane for discovery/identity/messaging/observability; ANP = decentralized, DID-centric open-network/marketplace model (matches the 'crypto angle' better than A2A does); AP2 (Google, donated to FIDO Alliance April 2026) = payment/commerce authorization via signed 'mandates', available as an A2A/UCP extension.
- Platform adoption is real but uneven: Microsoft (Azure AI Foundry, Copilot Studio) and AWS (Bedrock AgentCore Runtime) have production A2A integrations; Google ADK, LangChain/LangGraph, and Microsoft Foundry/.NET SDK have A2A support, but operational maturity varies by vendor/framework.

## Рекомендации / решения
- Treat A2A as validated production infrastructure for agent discovery and delegation — safe to build on for the 'handshake' layer.
- Do not treat trust, decentralized identity, governance, or agent-to-agent economics as solved — budget separate work/vendor selection for those layers (AGNTCY for infra, ANP for decentralized discovery, AP2 for payments).
- When positioning AAA C(H+A)RM against this stack, position differentiation above the commoditized communication layer (coordination, governance, economics) rather than competing with A2A itself on discovery/delegation, which is becoming a standardized commodity.

## Сущности
- **Люди:** —
- **Компании:** Google, Microsoft, AWS, Salesforce, SAP, ServiceNow, Workday, IBM, Cisco, Linux Foundation, FIDO Alliance, MIT
- **Продукты/инструменты:** A2A (Agent2Agent), MCP, AGNTCY, ANP (Agent Network Protocol), AP2, Agent Cards, Azure AI Foundry, Copilot Studio, Amazon Bedrock AgentCore Runtime, Google ADK, LangChain/LangGraph, Semantic Kernel, Ripple Effect Protocol

## Открытые вопросы
- How mature is signed-Agent-Card adoption in practice given signing is optional, not mandatory?
- What does the still-unfinished A2A registry/identity-verification work look like once shipped, and when?
- How exactly does AAA C(H+A)RM's swarm-orchestration vision map onto / differentiate from AGNTCY's infrastructure plane and the still-open coordination-primitives gap identified by the Ripple Effect Protocol paper?
- This DR was retro-registered from a ChatGPT conversation on 2026-07-07; the harvested vault note for the dialogue itself was truncated (report body missing there) — verify no additional vendor sections (Gemini/Grok) exist elsewhere for this DR-ID.

## Источник
- DR-ID `DR26-07-28-HUB-15-2339` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- A2A protocol
- MCP
- AGNTCY
- ANP
- AP2
- agent interoperability
- swarm orchestration
- AAA C(H+A)RM
