# DR26-07-28-HUB-15-2339 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "A2A протокол и будущее агентов"
type: deep-research-original
source: chatgpt

dr_id: DR26-07-28-HUB-15-2339
conversation_id: 6a4cf11b-08a8-83eb-ba2c-8cfb1552d1dd
url: "https://приватный чат"
research_started_at: 2026-07-07T12:30:44.134367Z
research_stopped_at: 2026-07-07T12:36:07.137819Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-07-28T23:39:04
vault_chat_note: "2026-07-07-a2a-protokol-i-buduschee-agentov-6a4cf11b.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
а вот что на это сказал мой AI агент: ;-)

Что уже построено (и довольно агрессивно):
Слой «агенты находят друг друга и делегируют» практически решён на уровне стандарта. Google в апреле 2025 выпустил протокол A2A (Agent-to-Agent), передал его в Linux Foundation, и в январе 2026 вышла версия v1.0 — переход из экспериментального в production-статус, с подписанными Agent Cards для криптографической верификации и кодифицированными механизмами discovery и доверия. К апрелю 2026 A2A поддерживают более 150 организаций — включая Google, Microsoft, AWS, Salesforce, SAP, ServiceNow, Workday и IBM. 

Механика ровно та, что ты описываешь: агенты на разных фреймворках и от разных вендоров общаются, обмениваются информацией и координируют действия без человека. Agent Cards = self-описание возможностей (твой «агент знает хозяина» — это расширенная Card + профиль). А подписанные Agent Cards — это по сути модель доверия, которая вообще делает децентрализованный discovery жизнеспособным, защищая от подделки карточек. Плюс рядом лежат ANP (децентрализованные маркетплейсы агентов — это и есть «крипто-угол») и AP2 (платежи между агентами). arxivStellagent

Так что «плоскогубцы и рукопожатие» уже есть. Твоя интуиция про будущее — не пророчество, а описание того, что консорциум из 150 компаний строит прямо сейчас. @Deep research 

Проведи глубокое исследование
```

## Отчёт
# A2A And The Emerging Agent Interoperability Stack

## Executive assessment

The core claim from your AI agent is directionally right, but materially sloppy on dates and too bullish on what is actually “solved.” The strongest accurate part is this: Agent2Agent, or A2A, has moved from a Google announcement into a Linux Foundation–hosted open standard with real vendor backing, a stable v1.0 release, and live integrations in major cloud and agent platforms. That is not vaporware. It is real infrastructure for cross-agent communication and delegation. But several details in the original summary are wrong or overstated: Google announced A2A on **April 9, 2025**, donated it to the Linux Foundation on **June 23, 2025**, and A2A **v1.0.0** was released on **March 12, 2026**, not January 2026. The “150+ organizations” milestone is supported as of **April 9, 2026**. citeturn2view1turn4view1turn8view0turn4view0

The more important nuance is architectural. A2A solves a meaningful slice of the stack: agent discovery via Agent Cards, task-oriented message exchange, multi-turn interactions, streaming, push notifications, and framework-agnostic delegation. It does **not** fully solve the harder problems of universal trust, decentralized identity, cross-domain registry governance, reputation, economic settlement, or large-scale coordination. Even the A2A project’s own published roadmap still lists registry work, validation/tooling, and best-practice hardening as active areas rather than closed problems. citeturn7view0turn7view1turn7view4turn17view0turn4view0

## What the public record actually shows

The factual timeline is straightforward. Google launched A2A publicly on **April 9, 2025**, describing it as an open protocol for agents built by different vendors and frameworks to securely exchange information and coordinate actions across enterprise systems. At launch, Google said the effort already had support from more than 50 partners. On **June 23, 2025**, Google transferred A2A into a Linux Foundation project with foundational backing from AWS, Cisco, Microsoft, Salesforce, SAP, and ServiceNow, and Linux Foundation messaging at that point described support from more than 100 technology companies. citeturn2view1turn4view1turn4view2

The next major inflection point was **A2A v1.0.0 on March 12, 2026**. GitHub’s release record shows that exact release date, and Linux Foundation and Google both describe the 1.0 line as the first stable, production-ready version of the protocol. By **April 9, 2026**, the Linux Foundation reported more than 150 organizations supporting the project, deep platform integration across Google, Microsoft, and AWS, and production use in verticals including supply chain, financial services, insurance, and IT operations. citeturn8view0turn4view0turn2view0

So if the claim is “this is no longer a speculative idea; a serious cross-vendor consortium is building it,” that is correct. If the claim is “Google handed it to Linux Foundation in April 2025 and stable v1.0 shipped in January 2026,” that is simply false on the historical record. citeturn2view1turn4view1turn8view0

## What A2A actually standardizes

A2A’s own specification is clear about its scope. The protocol is designed so agents can discover one another’s capabilities, negotiate interaction modalities, manage collaborative tasks, and securely exchange information **without needing access to each other’s internal state, memory, or tools**. The design is intentionally built on familiar transport and web primitives: HTTP, JSON-RPC 2.0, Server-Sent Events, and, in v1.0, a formal gRPC binding as well. That is why A2A is best understood as a practical interoperability contract rather than a grand theory of agent society. citeturn7view0turn7view1turn7view4turn7view5

Discovery revolves around the **Agent Card**. In the current spec, clients can discover an agent through a standardized well-known URI, through registries or catalogs, or through direct configuration. The Agent Card advertises capabilities, available skills, supported protocols, authentication requirements, and related metadata. In other words, it is the machine-readable “business card” layer for interoperable agents. citeturn6view2turn20view4turn20view2turn20view3turn21view4

That means the original phrasing “Agent Cards = self-description of capabilities” is substantially correct. But the jump from that to “the agent knows the owner” is an extrapolation, not a core A2A guarantee. The base schema covers name, description, skills, capabilities, security schemes, extensions, and protocol declarations; a rich owner profile, durable user model, policy envelope, or delegated authority model would sit above the core A2A surface or in adjacent standards. citeturn20view0turn20view2turn20view3turn21view2

This also explains why A2A is explicitly positioned as **complementary** to MCP, not a replacement for it. The A2A spec says MCP standardizes how agents use tools, APIs, and data sources, while A2A standardizes how independent agents communicate and collaborate as peers. A realistic stack is therefore: MCP for tool/resource access, A2A for peer delegation and orchestration across trust boundaries. citeturn7view2turn2view1turn4view0

## Where the original claim is overstated

The biggest overstatement concerns trust. Yes, A2A now supports **signed Agent Cards**, and Google and Linux Foundation present this as an important security advance. But the spec does **not** make signing mandatory. It says Agent Cards **may** be digitally signed using JSON Web Signature, with canonicalization rules based on RFC 8785 and verification logic for clients. That is meaningful progress, but it is not the same thing as saying decentralized discovery and trust are “basically solved.” citeturn2view0turn4view0turn6view1

There is further evidence that the trust layer is still incomplete. The A2A roadmap still lists agent registry, validation tooling, and community best practices as ongoing work. Separately, an A2A identity-verification proposal filed in March 2026 explicitly states that cryptographic verification of who an agent really is remains outside the core standardized mechanism, even though Agent Cards already carry descriptive identity metadata. That is the opposite of a closed problem. It is a protocol maturing in public. citeturn17view0turn16search3

The same pattern appears in independent research. A 2026 threat-modeling paper on emerging agent protocols argues that protocol-level security assessment is still immature and that standardized threat modeling remains limited. A separate governance-gap paper argues that the current generation of protocols addresses messaging, discovery, and capability declaration, but still leaves important policy and governance functions under-specified. And Cisco/MIT’s Ripple Effect Protocol paper makes the point even more bluntly: protocols like A2A and ACP largely provide a **messaging layer**, but stop short of true coordination primitives for large agent populations. citeturn25view1turn25view0turn25view2

So the right framing is not “the whole agent mesh problem is solved.” It is “the communication and delegation handshake is now real, standardized, and increasingly production-grade, while trust, governance, coordination, and economics are being built around it.” citeturn4view0turn17view0turn25view0turn25view2

## How adjacent protocols fit around A2A

The adjacent-stack story is real, but the components do different jobs and are at different maturity levels. **AGNTCY** is not the same thing as A2A. Under Linux Foundation governance, AGNTCY positions itself as infrastructure for discovery, identity, messaging, and observability across multi-agent systems. Linux Foundation materials describe AGNTCY as interoperable with both A2A and MCP, and Cisco’s own positioning explicitly says AGNTCY components such as Directory, Identity, SLIM messaging, and Observability are being wired to support A2A agents. In practice, AGNTCY is more like the “infrastructure plane” around agent interaction. citeturn2view5turn23search11turn23search4turn23search2

**ANP**, the Agent Network Protocol, is a separate protocol family oriented toward a more decentralized, open-network model. Its official materials emphasize decentralized identity, discovery, end-to-end messaging, and domain protocols, and its architecture is explicitly DID-centric. Academic surveys consistently place ANP in the “open internet / decentralized marketplace” camp, contrasting it with A2A’s enterprise-oriented capability cards and delegation model. So the crypto-native or trustless-marketplace intuition in your AI agent’s summary is not invented, but it fits ANP much better than A2A itself. citeturn15view2turn15view3turn15view5turn24search6

**AP2**, by contrast, is a real protocol effort around payments and delegated commerce, and it is closer to the “economic coordination” layer than to general inter-agent messaging. Google introduced AP2 in September 2025 as an open protocol for secure, compliant agent-merchant transactions, with support for cards, stablecoins, and real-time bank rails. AP2’s core mechanism is not generic discovery but **mandates** and verifiable digital credentials: cryptographically signed artifacts that capture what a user authorized an agent to buy and how it may pay. AP2 is also explicitly available as an extension for A2A and UCP. In April 2026, Google donated AP2 to the FIDO Alliance, which launched workstreams on agentic authentication and payments to standardize trusted agent interactions. citeturn15view0turn15view1turn18view2turn18view3turn22view0turn22view1turn22view2

That means your AI agent was right to mention a broader protocol constellation, but wrong to flatten it into one solved layer. The cleaner mental model is this: **A2A handles cross-agent tasking and delegation; MCP handles tool and resource access; AGNTCY adds infrastructure for identity, directory, transport, and observability; ANP pushes toward decentralized open-agent networking; AP2 addresses payment authorization and transactional trust.** citeturn7view2turn2view5turn15view2turn15view1turn22view1

## Platform adoption is real, but still uneven

On the adoption front, the evidence is strong enough to take seriously. Linux Foundation says Microsoft integrated A2A into **Azure AI Foundry** and **Copilot Studio**, and AWS added support via **Amazon Bedrock AgentCore Runtime**. Microsoft’s own documentation shows that Copilot Studio can connect to external A2A agents, delegate tasks to them, and auto-populate metadata from the agent card at the standard well-known path. AWS’s own documentation and blog posts show AgentCore Runtime supporting A2A for agent discovery, capability exchange, routing, and authenticated agent-card exposure. citeturn4view0turn12view2turn12view0turn12view1

Framework support is also no longer hypothetical. Google’s ADK documentation has dedicated A2A guidance for exposing and consuming remote agents. LangChain’s LangGraph server documentation includes A2A endpoints for multi-turn agent-to-agent communication. Microsoft’s Foundry ecosystem has .NET SDK work and public guidance connecting A2A with Semantic Kernel and Foundry workflows. That validates the basic claim that agents built on different frameworks can speak to one another through a common protocol surface. citeturn12view4turn12view5turn12view7turn11search3

Still, “supported” does not mean “uniformly production hardened across all implementations.” The A2A project is still actively evolving version compatibility, extensions, identity handling, registry conventions, and tooling, and the practical behavior of real-world servers depends on the surrounding platform. In other words: the standard exists, but operational maturity still varies by vendor, framework, and deployment discipline. citeturn8view0turn17view0turn16search3turn25view1

## Bottom line

If the thesis is “the world has crossed from isolated agents toward a genuine interoperability layer,” then yes: that threshold has been crossed. A2A is the strongest public evidence. It has a real standard, a Linux Foundation home, a stable release, cloud-platform integrations, framework support, and a partner ecosystem large enough to matter. Your AI agent was not hallucinating the trend. citeturn4view0turn2view0turn12view0turn12view2turn12view4

But if the thesis is “discovery, trust, decentralization, and agent-to-agent economics are basically finished,” that is marketing, not clean analysis. The current state is better described as follows: **the handshake layer is here; the trust layer is partial; the registry layer is still maturing; the payment layer is emerging; and the coordination layer above communication is still an open research and engineering frontier.** A2A is therefore not the full “Internet of Agents.” It is the first serious, production-grade protocol on which such an internet could be built. citeturn6view1turn17view0turn22view1turn25view2turn25view0
