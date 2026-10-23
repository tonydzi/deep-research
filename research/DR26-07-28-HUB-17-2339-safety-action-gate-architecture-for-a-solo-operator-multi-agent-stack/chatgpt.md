# DR26-07-28-HUB-17-2339 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Safety Action-Gate Architecture"
type: deep-research-original
source: chatgpt

dr_id: DR26-07-28-HUB-17-2339
conversation_id: 6a499674-0654-83eb-8ecc-e241f67ba636
url: "https://приватный чат"
research_started_at: 2026-07-04T23:27:00.149544Z
research_stopped_at: 2026-07-04T23:33:08.923304Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-07-28T23:39:06
vault_chat_note: "2026-07-05-safety-action-gate-architecture-6a499674.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
Conduct a deep research investigation on the following topic.



TOPIC: Best architecture for a SAFETY / ACTION-GATE over a personal multi-agent setup — decide execute / require-human-approval / block for every risky action — WITHOUT over-engineering. Compare central "MCP proxy/gateway" vs shared approval-FUNCTION chokepoint vs per-tool interceptors, for a solo operator whose risky actions span BOTH MCP calls AND direct Python API scripts (Telegram-as-user, WhatsApp, Gmail, Calendar, n8n, browser automation).



CONTEXT: Gates today are SCATTERED: a "Tier-2" policy (money/outbound/irreversible/secrets pause for human OK), a phone "QQQ" approval flow (approval.py), a rate-limiter+kill-switch for auto-posting (arm/disarm), Claude Code permission modes — enforcement duplicated by convention, not centralized. Reference tools: Invariant mcp-scan (Apache-2.0, now Snyk) — static tool-description scan for injection/tool-poisoning + runtime PROXY guardrails; Docker MCP Gateway (interceptors, container-per-server); IBM ContextForge; Microsoft MCP Gateway (K8s). KEY RISK: much of the highest-risk surface (Telegram/WhatsApp/Chrome/raw Python) does NOT flow through MCP → a pure MCP-proxy leaves gaps and creates FALSE confidence.



OBJECTIVE: When is a central gateway justified vs a shared "single chokepoint function every action-script must call" + periodic tool-description scanning; how to cover non-MCP action surfaces uniformly.



RESEARCH REQUIREMENTS:

1. Academic — agent safety, tool-use guardrails, prompt-injection/tool-poisoning defenses, human-in-the-loop gating.

2. Industry — mcp-scan/Snyk, Docker MCP Gateway, ContextForge, MS MCP Gateway, invariant guardrails, LangChain/LlamaIndex callback gating, OpenClaw/Hermes.

3. Competitive — gateways vs SDK-level policy vs OS-level sandboxing.

4. Failure analysis — proxies as single points of failure, silently-blocked legit actions, false safety; injection incidents.

5. Emerging trends — last 12 months in MCP security / action governance.

6. Implementation — scaled answer: minimal now (shared chokepoint + scan mode) vs the fleet-scale threshold for a real gateway; gating the non-MCP Python/Telegram/WhatsApp/browser surface.



OUTPUT: exec summary; decision matrix ({shared chokepoint} vs {scan-only} vs {runtime proxy} vs {full gateway} × {MCP+non-MCP coverage, new failure points, maintenance, repairability, cost}); recommendation with explicit "build the gateway only when X" threshold; do-this-month vs later; sources.@Deep research
```

## Отчёт
# Best Architecture for a Safety Action Gate for a Personal Multi-Agent Stack

## Executive summary

For a **solo operator** whose risky actions span both **MCP traffic** and **non-MCP execution surfaces** such as direct Python scripts, Telegram-as-user, WhatsApp, browser automation, Gmail/Calendar APIs, and n8n, the best architecture is **not** a central MCP gateway as the primary safety boundary. The best architecture is a **shared action-approval chokepoint** that every action-capable script, tool wrapper, and automation must call before any side effect occurs, combined with **periodic scanning** of MCP servers and agent skills, plus **sandboxing/containment** for the highest-privilege runtimes. A pure MCP proxy or gateway only governs the subset of actions that actually traverse MCP, while your stated highest-risk surfaces often do not. Treating an MCP gateway as “the safety layer” in that environment creates a serious **false-confidence failure mode**. citeturn28view0turn11view0turn11view2turn36view1

The academic and industry evidence points in the same direction. Prompt injection remains a fundamental and long-lived weakness for tool-using agents, especially when they consume untrusted content from emails, webpages, issues, messages, or tool outputs; current defenses improve robustness but do not eliminate the problem. Anthropic states that no browser agent is immune; OpenAI describes prompt injection as an open challenge for agent security; the UK NCSC recommends treating these systems as “inherently confusable deputies.” In practice, this means safety has to be enforced at the **execution boundary**, not only at the prompt or protocol boundary. citeturn28view5turn28view6turn28view7turn18view1turn18view4

A central gateway becomes justified when your system crosses from “one person with a few code paths” into “shared infrastructure with multiple clients, identities, servers, or remote deployments,” where you need centralized **RBAC, routing, lifecycle management, observability, server inventory, and uniform auth**. That is the space targeted by Microsoft MCP Gateway, IBM ContextForge, Docker MCP Gateway profiles, and MCP’s new Enterprise-Managed Authorization work. Before that threshold, a gateway usually adds more moving parts than safety value, while still leaving non-MCP gaps unless you separately standardize those paths anyway. citeturn38view1turn14view3turn37view0turn29view2

The practical recommendation is therefore:

1. **Now:** build one **mandatory approval function / policy engine** for every risky action, regardless of transport or framework.  
2. **Also now:** run **scan-only** controls regularly on MCP servers and agent skills, and pin/allowlist tool sources.  
3. **Add selectively:** use an **MCP runtime proxy** only for the MCP subset if you want visibility or guardrails there.  
4. **Later:** build or adopt a **full gateway** only when you have enough MCP scale and organizational complexity that centralized routing/auth/lifecycle management outweighs the new operational burden. citeturn4view0turn17search2turn37view0turn38view1

## What the evidence says about agent safety and why protocol-only control is insufficient

The core research result is not subtle: **tool-using agents are vulnerable because they mix trusted intent with untrusted context and then take real actions**. AgentDojo was built specifically to evaluate prompt injection in realistic agent tasks such as email, banking, travel, and workplace operations, and it found that state-of-the-art systems struggle both with task completion and with security under attack. A newer line of work shows that **tool-description poisoning** and related MCP-specific attacks can steer agents into dangerous actions, including sending money, forwarding email, posting messages, or making bookings for the attacker. Even advanced defenses reduce rather than erase this risk, and often introduce latency or utility trade-offs. citeturn18view1turn18view0turn20search0turn20search1

This is why protocol-bound defenses are necessary but insufficient. The MCP ecosystem has matured rapidly, but even the official roadmap and security work acknowledge that security and authorization are still active problem areas. The MCP specification explicitly says that, for sampling, there **should always be a human in the loop with the ability to deny requests**. The 2026 MCP roadmap emphasizes finer-grained least privilege, secure credential handling, and stronger validation; the EMA extension exists precisely because repeated per-server authorization and weak central governance became a pain point in real deployments. citeturn29view0turn29view1turn29view2

The last year also produced multiple concrete incidents and advisories showing that the problem is not hypothetical. Invariant documented tool poisoning and a GitHub MCP exploit that could coerce an agent into leaking private repository data; Microsoft published mitigations for indirect prompt injection and tool poisoning; the NVD and GitHub advisories documented an MCP Inspector flaw that enabled remote code execution through missing authentication between client and proxy; NVD also recorded vulnerabilities in Anthropic’s Git MCP server. The NSA’s 2026 design-considerations paper explicitly warns that MCP security cannot be patched just at one interface or endpoint and calls for proxies, middleware, and sandboxing as part of a broader control stack. citeturn4view1turn21view0turn23search0turn23search10turn24search1turn35view0turn36view1

For your exact question, the decisive fact is architectural rather than academic: **some of your highest-risk actions never touch MCP at all**. OpenClaw’s own security model says its default `main` session runs tools on the host with full access, and its gateway includes multi-channel messaging plus first-class tools such as browser, cron, nodes, Discord/Slack actions, and session actions. Hermes Agent similarly operates across Telegram, Discord, Slack, WhatsApp, Signal, and CLI from a single gateway process, and explicitly supports Python scripts that call tools via RPC. Those are powerful capabilities, but they prove the point: a pure MCP gateway does not automatically govern host tools, messaging channels, browser automation, or standalone Python code paths outside MCP. citeturn11view0turn11view1turn11view2

That same pattern appears in mainstream SDKs. OpenAI’s Agents SDK docs explicitly say the application owns orchestration, tool execution, approvals, and state; LangChain’s HITL middleware pauses **tool calls before execution** based on configurable policy; Microsoft’s Agent Framework states that tool approval is handled by the framework’s function-invoking chat client and works with any function tool regardless of underlying API. In other words, modern agent frameworks are already telling you where the real control plane is: at the **function/tool execution layer inside your app**, not only at an external MCP hop. citeturn28view0turn26view0turn26view1turn30view0

## Architecture options and decision matrix

The four options below solve different problems. The central mistake is to ask which one is “best” in isolation. The right question is: **which one gives the best coverage of real side effects in your environment for the least operational complexity**. For your setup, that answer is the shared chokepoint first.

| Option | MCP + non-MCP coverage | New failure points | Maintenance load | Repairability | Cost | Best use |
|---|---|---:|---:|---:|---:|---|
| **Scan-only** | **Low**. Good for discovering risky MCP servers, prompts, and skills, but does not mediate live execution and only partially reaches non-MCP surfaces. Snyk Agent Scan detects prompt injection, tool poisoning, tool shadowing, toxic flows, and skill risks; it supports scan mode and background mode. citeturn4view0 | **Low**. Mostly false negatives / stale findings / ignored reports. citeturn4view0turn18view0 | **Low**. Periodic runs, allowlists, review workflow. citeturn4view0 | **High**. Easy to disable, rerun, or narrow. | **Low** | Baseline hygiene, supply-chain review, periodic audits. |
| **Shared approval chokepoint** | **High**, if you make it mandatory for every side-effecting script/tool path. Works uniformly for MCP tool wrappers, direct Python APIs, Telegram/WhatsApp sends, Gmail, Calendar, n8n webhooks, browser actions, and shell commands because the control is at the execution boundary. This matches how SDKs expose approvals and middleware around tool execution. citeturn26view0turn26view1turn26view3turn26view5turn30view0 | **Moderate**. A bug in the central policy function can overblock or underblock, but blast radius is understandable and localized. | **Low to moderate**. One library, one schema, one audit log, one approval UI/channel. | **Very high**. Easy to inspect, patch, and test because all risky actions funnel through one function call. | **Low** | Best default for a solo operator with mixed MCP and non-MCP surfaces. |
| **Runtime MCP proxy** | **Medium**. Strong on MCP traffic only. Invariant and Docker-style proxy/interceptor patterns can inspect or rewrite MCP interactions, but they do not cover direct Python/API/browser paths unless those are separately wired. citeturn17search2turn34search3turn16view0 | **Moderate**. Proxy outage, transport mismatch, hidden bypasses, and false sense of universal coverage. CVE-2025-49596 is a reminder that proxies themselves become attack surface. citeturn23search0turn23search10 | **Moderate**. Transport setup, policy tuning, per-client configuration, debugging. | **Moderate**. Easier than a full gateway, but harder than an app-level chokepoint because bugs can hide at the transport boundary. | **Moderate** | Good additive layer when MCP traffic itself is important enough to inspect in real time. |
| **Full gateway** | **Medium to high for MCP**, **low for non-MCP unless you replatform those paths behind it**. Microsoft MCP Gateway, IBM ContextForge, and Docker MCP Gateway focus on routing, lifecycle, auth, discovery, observability, and multi-server governance. citeturn38view1turn14view3turn37view0 | **High**. New control plane, auth/RBAC complexity, session affinity, state store, deployment and secret management. citeturn38view1turn38view2turn14view3 | **High**. Best justified when you have many servers/clients and shared infra. | **Lower** than chokepoint for a solo setup because more moving parts can fail before the real action happens. | **High** | Fleet-scale or team-scale MCP governance, not the first control for a personal mixed stack. |

The **scan-only** path is not enough because it is mostly a supply-chain and metadata control. Snyk Agent Scan is genuinely useful: it discovers agent configurations, MCP servers, and skills; detects prompt injection, tool poisoning, tool shadowing, toxic flows, credential handling issues, and hardcoded secrets; and can run interactively with explicit user consent before starting stdio servers. But it is still a scanner, not the final execution gate. citeturn4view0

The **shared chokepoint** path maps best to your problem because your actual trust boundary is “anything that can perform an external side effect.” LangChain’s HITL middleware, LlamaIndex function callbacks, OpenAI human review, and Microsoft tool approval all converge on this model: before a function tool executes, the runtime pauses, validates, or asks for approval. That is exactly the semantics you want, but generalized across **all** action surfaces, not just whichever agent framework happens to be in the loop. citeturn26view0turn26view3turn26view5turn30view0

The **runtime proxy** layer is valuable when you want live inspection of MCP traffic, especially against MCP-specific threats like tool poisoning and malicious server outputs. Invariant positions Gateway/Guardrails as a transparent proxy in front of LLMs and MCP servers, and Docker explicitly supports before/after tool-call interceptors that can log, rewrite, or bypass calls. But that solves only the transport slice it sees. For your setup, it is a useful add-on, not the canonical execution boundary. citeturn17search2turn17search0turn16view0

The **full gateway** layer is real infrastructure, not just a guardrail. Microsoft’s MCP Gateway adds a data gateway with session affinity, a control plane for server lifecycle, auth/RBAC, and Kubernetes-native deployment. IBM ContextForge similarly federates MCP, A2A, REST, and gRPC behind centralized governance, registries, auth, retries, rate limiting, plugins, and observability. Docker’s MCP Gateway runs catalog servers in isolated containers behind a unified gateway and profile system. Those are useful products, but they are solving **governance at scale**, not just “should this message send be approved.” citeturn38view1turn38view2turn14view3turn37view0turn37view3

## Recommendation for your setup

The right recommendation is:

**Use a shared, mandatory action-approval function as the primary safety boundary.**  
Then add **scan mode** as baseline hygiene.  
Add a **runtime MCP proxy** only if you want extra visibility on MCP traffic.  
Do **not** build a full gateway yet unless you cross a clear scale threshold. citeturn4view0turn26view0turn26view5turn38view1

That recommendation follows directly from three constraints in your environment.

First, the biggest danger is **coverage mismatch**. Your risky actions span MCP and non-MCP paths. OpenClaw and Hermes run much of their power outside a pure MCP hop, and direct Python scripts by definition can call APIs or browser automation without passing through an MCP proxy. If you centralize only MCP, you centralize the most visible subsystem, not the real side-effect boundary. citeturn11view0turn11view1turn11view2

Second, the next biggest danger is **human-approval theater**. Academic work on daily-assistant agents shows that users can develop miscalibrated trust when plans look plausible, and that user involvement has to be tailored rather than applied blindly. Approval prompts that occur too early, too often, or too far from the actual side effect create fatigue and weak review quality. The best place to ask for approval is right before irreversible execution, with a compact, structured action summary and the exact arguments that will be used. citeturn32view0turn32view2

Third, the evidence on prompt injection says you should not rely on prompt instructions alone to enforce policy. Microsoft’s guidance on MCP tool poisoning, OWASP’s MCP Tool Poisoning entry, and the NCSC’s “confusable deputy” framing all point to the same conclusion: **backend execution controls must exist independently of the model’s willingness to obey them**. Your Tier-2 policy, QQQ phone flow, rate-limiter/kill-switch, and Claude Code permission modes are already an informal attempt at this. The fix is not more conventions. The fix is one hard enforcement call. citeturn21view0turn21view2turn28view7

### Build the gateway only when this threshold is crossed

Build or adopt a real MCP gateway **only when most of the following become true at the same time**:

- You have **multiple MCP clients or runtimes** that need the same centrally managed server inventory, auth, or policy. Docker explicitly positions one gateway/profile serving multiple clients; Microsoft and IBM are built around central control planes. citeturn37view3turn38view1turn14view3
- You run **enough MCP servers** that lifecycle management, dynamic discovery, profile management, container isolation, or RBAC become operationally painful to manage per client. citeturn37view0turn38view1turn14view3
- You need **identity-aware access control**, centralized audit, or shared governance across machines, sessions, or users. EMA, Microsoft Entra integration, and IBM’s built-in auth/rate limiting exist for that class of problem. citeturn29view2turn14view2turn14view3
- Your MCP traffic is important enough that **runtime inspection of tool descriptions, arguments, and responses** will materially reduce risk, and you are ready to manage another production component. citeturn17search2turn16view0
- You are already willing to standardize non-MCP actions behind the same governance plane, or at least accept that the gateway covers only the MCP subset. citeturn11view0turn11view2

If those conditions are not true, the gateway is usually over-engineering.

## Implementation blueprint for this month and later

### Do this month

Build one library or service called something like `guard_action(...)` and make it the only permitted path for any code that can cause a side effect. The function should take a structured request like:

- `surface`: `mcp`, `python_api`, `telegram_user`, `whatsapp`, `gmail`, `calendar`, `n8n`, `browser`, `shell`
- `action_type`: `send_message`, `send_email`, `calendar_write`, `browser_click_submit`, `execute_shell`, `transfer_money`, `delete`, `post_public`, `share_secret`, `create_workflow_run`
- `target`
- `arguments`
- `risk_tags`: `outbound`, `money`, `irreversible`, `secrets`, `external_visibility`, `credential_use`
- `provenance`: model/session/script/tool/user/request IDs
- `dry_run_preview`
- `idempotency_key`
- `undo_plan` if any

The function should deterministically return one of exactly three outcomes: **`execute`**, **`require_human_approval`**, or **`block`**. That matches your desired decision model and aligns with how current agent frameworks expose tool guardrails and human review. citeturn26view0turn26view5turn30view0

Then wire your existing risky paths to it:

- MCP tool wrappers call `guard_action()` **before** invoking the tool.
- Direct Python scripts for Gmail, Calendar, Telegram, WhatsApp, or n8n call it **before** the API request.
- Browser automation calls it **before** submission, send, purchase, delete, confirm, or file-upload steps.
- Shell or Python execution calls it **before** execution and should prefer sandboxed runtimes for untrusted or model-written code. OpenAI and Anthropic both frame sandboxing as the boundary that lets agents act more autonomously without unrestricted machine access. citeturn28view2turn28view3turn28view4

The approval UX should stay simple. n8n’s HITL guidance is exactly right here: approval belongs **at the point of action**, not merely at the point of output. The human sees the intended action and either approves, edits, or rejects. LangChain’s middleware model supports the same approval/edit/reject semantics, and OpenAI’s human review model is similar. citeturn27view0turn26view0turn26view5

Run periodic **scan mode** on MCP and skill surfaces. Snyk Agent Scan is the obvious baseline because it is designed to discover agent configs, MCP servers, and skills and flag prompt injection, tool poisoning, toxic flows, credential handling issues, and hardcoded secrets. Use it on a schedule and after any tool/server/skill changes. Review its report, but do not confuse it with runtime execution control. citeturn4view0

Finally, add **containment** where the consequences are highest:

- model-written or model-triggered shell/Python → sandbox
- browser automation with logged-in personal accounts → separate browser/account/runtime, ideally isolated
- public or shared messaging agents → separate trust boundary, not mixed with personal high-privilege runtime

OpenClaw explicitly warns that one gateway is one trust boundary and recommends splitting gateways/hosts for adversarial-user isolation; Anthropic and OpenAI recommend sandboxes for higher-autonomy execution. citeturn11view1turn28view2turn28view4

### Do later

If you later want better runtime visibility over MCP specifically, insert an MCP proxy for that slice. Invariant Gateway can proxy MCP and LLM traffic for guardrails and monitoring; Docker supports before/after interceptors for tool calls; both are useful if you want live checks on tool descriptions, arguments, and responses without rewriting every MCP client. citeturn17search2turn34search3turn16view0

If your MCP estate becomes large enough, then adopt a full gateway instead of continuing ad hoc per-client config. Microsoft MCP Gateway is the clearest example of the threshold: reverse proxy, session-aware routing, distributed session store, control plane, RBAC, and Kubernetes-native deployment. IBM ContextForge goes even broader by federating MCP, A2A, REST, and gRPC with centralized governance, retry, rate limiting, plugins, and observability. citeturn38view1turn38view2turn14view3

What you should **not** do later is replace the shared action chokepoint with the gateway. Even after adopting a gateway, keep the chokepoint. The gateway is infrastructure governance. The chokepoint is the last-mile execution gate.

## Failure modes, emerging trends, and limitations

The most important failure mode is **false safety by partial centralization**. If you say “all actions go through the gateway” while Telegram-as-user, Playwright, raw Gmail API scripts, or n8n webhooks still bypass it, you have made your architecture look safer than it is. The NSA paper is blunt that MCP security gaps span design, implementation, and operations, and that organizations need controls beyond the protocol itself; OpenClaw is equally blunt that tool authority is shared inside one gateway trust boundary. citeturn35view0turn36view0turn36view1turn11view1

The second failure mode is **approval fatigue and silent legitimate blocking**. Research on human oversight and daily assistants shows that human involvement can improve outcomes, but it also carries cognitive costs and can fail to calibrate trust if the interface is poorly designed. If your policy asks for approval too often, operators rubber-stamp. If it asks too vaguely, operators cannot judge. If it silently blocks without a repair path, you train yourself to bypass the system. The practical fix is to keep approval criteria narrow and high-value: money, outbound messages, irreversible writes, secrets, account linking, public posting, and broad browser submissions. citeturn32view0turn32view2turn27view0

The last twelve months show several clear trends. MCP security is moving toward **centralized auth and governance** with EMA, **interest groups focused on security and interceptors**, more **runtime scanning/proxy** products, and more **official HITL/tool-approval** features in mainstream frameworks. At the same time, the incident stream shows that proxies, SDKs, inspectors, and servers themselves all create new attack surface. That means the trend is not “one perfect central control is arriving.” The trend is **defense in depth with explicit trust boundaries**. citeturn29view2turn13search2turn13search14turn13search5turn23search0turn24search1turn25search0

## Open questions and limitations

This report is high-confidence on the architectural conclusion, but a few practical details remain environment-specific.

I could not verify a single canonical, official document for every vendor detail in your reference list beyond the sources cited here, and some vendor products are evolving quickly. In particular, Invariant’s older MCP-scan branding has partially shifted to Snyk Agent Scan, while Invariant’s Gateway/Guardrails materials remain separately published. The safe interpretation is that **scanning and runtime proxying are both real patterns**, but product naming and packaging are in flux. citeturn4view0turn17search0turn17search2

I also did not attempt to prescribe one exact approval schema for Telegram-as-user or WhatsApp browser automation, because the right granularity depends on whether those actions happen through official APIs, browser automation, OpenClaw/Hermes channels, or your own scripts. The architecture does not change: the action must still cross the same mandatory chokepoint before side effects occur. citeturn11view0turn11view2

The bottom line is straightforward: **for your environment, build the shared chokepoint first and treat everything else as additive**. Build the gateway only when you have enough MCP scale, shared governance, and identity complexity that it is solving an infrastructure problem rather than substituting for a missing universal execution gate.
