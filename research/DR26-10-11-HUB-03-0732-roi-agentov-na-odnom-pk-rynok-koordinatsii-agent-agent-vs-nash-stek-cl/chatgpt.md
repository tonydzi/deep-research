# DR26-10-11-HUB-03-0732 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# ChatGPT (Deep research, Extra High, web, Chrome Антона) — DR26-10-11-HUB-03-0732 — снято 2026-10-14 20:25Z из чата «Research Agent Swarms» https://приватный чат · отчёт завершён 19:41Z · 487 safe_urls

# Local Agent Swarms in 2026: Market Map, Falsification, and Engineering Audit

**Research date:** October 14, 2026
**Scope:** Local coding agents, independent sessions on one computer, and coordination across a small fleet of computers. Excludes robotics and cloud-only agent platforms except where they materially challenge the proposed architecture.

## 1. Executive summary

1. **The proposed market gap is real but narrower than claimed.** I did not find a verified, ready-to-adopt system that combines all five requirements: equal-footing heterogeneous agents, multi-machine operation, no dedicated coordination server, machine-enforced human approval tiers, and exclusive governance over the rulebook. However, numerous projects implement substantial portions of that combination. The defensible claim is an **integration gap**, not that the underlying coordination mechanisms are unprecedented.

2. **Native vendor capabilities are catching up quickly.** Claude Code now documents direct messaging between independently launched sessions, using local sockets or Windows named pipes; it also supports messaging across machines through Anthropic's servers. Agent Teams adds peer messaging, shared tasks, and atomic task claims. This is much more substantial than traditional parent–subagent delegation, but it remains Claude-specific. Sources: [cross-session messaging](https://code.claude.com/docs/en/cross-session-messaging), [Agent Teams](https://code.claude.com/docs/en/agent-teams).

3. **The strongest competitors are in different architectural layers.** [hcom](https://github.com/aannoo/hcom) is a compelling heterogeneous session messenger; [llm-bus](https://github.com/danieldoderlein/llm-bus) provides atomic claims and acknowledged handoffs; [agent-mesh](https://github.com/johnmwhitman/agent-mesh) adds receipts and quorum councils; [Paperclip](https://github.com/paperclipai/paperclip) combines agent management with budgets and human approvals. None is a verified drop-in replacement for the complete proposed fleet protocol.

4. **Two projects directly challenge the supposedly unique serverless architecture.** [agent-sync](https://github.com/ssheleg/agent-sync) uses Git remote compare-and-swap for cross-machine exclusive leases. [system-gap-master](https://github.com/ellmos-ai/system-gap-master) documents a synced-folder, single-writer-per-machine coordination pattern. The claim that nobody else combines ordinary file synchronization with multi-machine agent coordination is therefore false.

5. **For eight machines, correctness matters more than swarm size.** The immediate architectural risk is treating asynchronously synchronized files as though they provide atomic global claims. They do not. Cross-machine leases need an explicit serialization mechanism, or the system must tolerate duplicate claims and resolve them deterministically.

6. **Adopt selective mechanisms, not another full orchestrator.** My priorities are intent/scope declarations, genuinely atomic cross-machine claims, persistent acknowledged inboxes, a vendor-neutral MCP adapter, and evidence-gated completion. [foremerge](https://github.com/naw103/foremerge), [agent-sync](https://github.com/ssheleg/agent-sync), [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust), and [Beads](https://github.com/gastownhall/beads) provide useful reference implementations. Beads currently uses Dolt as its authoritative store; describing it simply as a Git-synchronized JSONL task ledger would be inaccurate.

7. **The most serious findings in the public claw-consensus reference implementation concern enforcement, not architecture.** The reference engine does not visibly authenticate incoming events before replay; its human-approval command accepts a textual proof reference rather than verifying human authorization; the leader can commit without an agreed state; and duplicate verification evidence produces a warning but is still recorded. These findings concern the [published reference engine](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py), not necessarily private fleet integrations.

8. **The project has more operational substance than a demo, but its published evaluation does not establish eight-machine correctness.** It documents real production failures involving synchronization conflicts, sender-only delivery evidence, and misleading liveness checks. Its reproducible consensus benchmark, however, simulates two machines on one host without transport latency or live agents. Sources: [failure modes](https://github.com/tonydzi/claw-consensus/blob/main/docs/FAILURE-MODES.md), [evaluations](https://github.com/tonydzi/claw-consensus/blob/main/docs/EVALS.md).

**Bottom line:** Preserve claw-consensus as a small, vendor-neutral *coordination and governance protocol*. Do not attempt to compete with full agent IDEs, terminal multiplexers, or enterprise dashboards. Make the guarantees executable, add easy integration and observability, and publish reproducible multi-machine adversarial tests.

## 2. Comparison tables

### Method and definitions

I distinguish three kinds of evidence:

- **Documented:** The repository or official documentation explicitly describes the feature.
- **Implemented:** The relevant behavior is visible in public source code.
- **Operationally verified:** A reproducible test, independently reported deployment, or documented failure and recovery provides evidence beyond a feature claim.

A README can establish that a feature is advertised. It does not, by itself, establish reliable operation during network partitions, process crashes, or competing writes.

In the tables:

| Symbol | Meaning |
|---|---|
| Y | Documented capability |
| P | Partial, advisory, or materially qualified capability |
| N | Explicitly absent or incompatible with the architecture |
| NV | Not verified from available evidence |
| — | Not a stated feature of the tool |

**GitHub metadata:** Stars are rounded snapshots observed in research on **October 14, 2026**, not exact immutable counts. I could not reliably retrieve the HEAD commit timestamps across the repositories. Consequently, **last commit is NV throughout**. Release dates are not substitutes for last-commit dates.

"Server" means a dedicated coordination daemon or network service. A Git remote is classified separately: it can avoid a custom agent server while still introducing a centralized serialization dependency.

### 2A. Heterogeneous message buses, mailboxes, protocols, and distributed coordination

Transport abbreviations: FS = filesystem; DB = database; HTTP = HTTP service; WS = WebSocket; Git CAS = atomic Git reference update. Vendor codes: C = Claude, X = Codex, U = Cursor, G = Gemini, O = OpenCode, H = Hermes. "Any MCP" means compatibility through an adapter, not verified native support for every CLI.

| Project / source | Transport; vendors | Multi-PC; server | Claim / lease | ACK / receipt | Quorum | Human tiers | Rule governance | Cost | Stars; last commit | License |
|---|---|---|---|---|---|---|---|---|---|---|
| [claw-consensus](https://github.com/tonydzi/claw-consensus) | Per-node FS JSONL + Syncthing; vendor-neutral adapters | Y; no dedicated server | Local file leases; cross-PC CAS NV | Y, ACK debt | Documented rounds and arbiter; enforcement P | Y in policy; reference enforcement P | Y, leader/follower policy | — | NV; NV | See repo |
| [agentbus](https://github.com/yanglinfang/agentbus) | Shared Git/JSONL; C/X/U/H and CLI integrations | P; no daemon | Advisory locks / claims | Y, inbox ACK | — | — | — | — | 0; NV | MIT |
| [llm-bus](https://github.com/danieldoderlein/llm-bus) | HTTP/MCP; heterogeneous | Y; yes | Y, atomic task claims and file leases | Y, acknowledged handoffs | — | — | — | — | 9; NV | AGPL-3.0 |
| [agent-mesh](https://github.com/johnmwhitman/agent-mesh) | SQLite + MCP; OpenCode-centered, C/X via MCP | Multi-PC NV; local DB | P, orchestration/task ownership | Y, delivery/view/ACK | Y, councils | — | P, council rules | — | 1; NV | MIT |
| [Alook](https://github.com/alookai/alook) | Collaboration rooms/service; heterogeneous clients | P; service for network use | NV | Messaging; durable ACK NV | — | — | — | — | ~1.4k; NV | Apache-2.0 |
| [Agent Relay](https://github.com/AgentWorkforce/relay) | Relay service; C/X/H integrations | Y; yes | NV | Messaging; processing ACK NV | — | — | — | — | 883; NV | Apache-2.0 |
| [AgentTeams / AgentScope](https://github.com/agentscope-ai/AgentTeams) | Matrix rooms / homeserver; agent adapters | Y; yes | Task coordination; lease NV | Matrix messaging; application ACK NV | — | Human interaction, not equivalent tiers | P, team organization | NV | ~5.8k; NV | Apache-2.0 |
| [OpenAgentMesh](https://github.com/openagentmesh/openagentmesh) | NATS + REST/MCP; framework-neutral | Y; NATS required | NV | Broker delivery; app ACK NV | — | — | — | — | 7; NV | MIT |
| [MCP Agent Mail, Python](https://github.com/Dicklesworthstone/mcp_agent_mail) | HTTP MCP + SQLite/Git; any MCP | Y; yes | Y, file reservations | Y, mail ACK | — | — | — | — | ~2.2k; NV | MIT with additional rider |
| [MCP Agent Mail, Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust) | HTTP MCP + SQLite/Git; any MCP | Y; yes | Y, TTL reservations | Y, inbox ACK | — | — | — | — | 198; NV | MIT with additional rider |
| [foremerge](https://github.com/naw103/foremerge) | Local SQLite + Git refs + MCP; C/X through adapters | N as shared DB; no remote coordination service | Y, **advisory** scope claims | P, recorded operations | — | — | P, project policy | — | 539; NV | Apache-2.0 |
| [teamctl](https://github.com/Alireza29675/teamctl) | Local YAML/CLI; C/X/G | N; no | — | — | — | — | Team configuration only | — | 20; NV | MIT |
| [hcom](https://github.com/aannoo/hcom) | Local SQLite/Rust CLI; C/X/U/O/Pi and others | Y via relay; relay required remotely | NV | Inbox, send; processing ACK NV | — | — | — | — | 571; NV | MIT |
| [AgentBridge](https://github.com/is-bo/agentbridge) | Local MCP delegation; C/X and other integrations | NV; local | P, bounded delegated scope | Result handoff; durable ACK NV | — | — | — | — | 1; NV | MIT |
| [Concord MCP](https://github.com/Get-Concord-AI/concord-mcp) | Local SQLite + MCP; C/X/U/G/Grok/Goose | Shared checkout only; no | Y, local CAS and file scopes | Y, explicit acceptance | — | — | P, shared coordination conventions | — | 487; NV | MIT |
| [Concord, Softsensor](https://github.com/Softsensor-org/concord) | Local Git/worktrees + audit; C/X/U | Cross-PC NV | Y, task/worktree controls | Audit events; ACK NV | — | P, human checkpoints | P, configurable governance | NV | NV; NV | Apache-2.0 |
| [nats-channel-mcp](https://github.com/except-pass/nats-channel-mcp) | NATS / JetStream + MCP; Claude channel | Y; NATS required | — | **Transport ACK before model processing** | — | — | — | — | 0; NV | MIT |
| [claude_codex_bridge](https://github.com/SeemSeam/claude_codex_bridge) | Local CLI/daemon; C/X/G/Kimi/Qwen/U/O | Cross-PC NV; local daemon | NV | Local messaging; durable ACK NV | — | — | — | — | ~3.6k; NV | NV |
| [Agent Messaging Protocol](https://github.com/agentmessaging/protocol) | Federated messaging, signed identity; generic agents | Y in protocol design; federation services | — | Protocol receipts; implementation guarantees NV | — | — | Identity policy, not rulebook governance | — | 36; NV | Apache-2.0 |
| [agent-sync](https://github.com/ssheleg/agent-sync) | Git CAS + local hooks; strong Claude integration | Y; Git remote | **Y, remote atomic leases** | Task journal; application ACK NV | — | — | — | — | 4; NV | MIT |
| [system-gap-master](https://github.com/ellmos-ai/system-gap-master) | Single-writer FS + Syncthing/OneDrive/Git | Y; no custom daemon | P, writer ownership | Operational handoffs; guarantees NV | — | P, local gating | P, operational rules | — | NV; NV | MIT |
| [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm) | JSONL/SQLite + constitutional mesh; generic agent framework | P; optional network peer connections | NV | Y, signed receipts | **Y, signed votes/quorum** | P, constitutional controls | Y, constitution-oriented, not exclusive leader canon | NV | 3; NV | AGPL-3.0-or-later |
| [AgentSync](https://github.com/dipakkr/agentsync) | WebSocket/MCP hub; C/X/Kimi | Y; yes | Y, coordination claims | Messaging; ACK semantics NV | — | P, human decisions | P, project coordination | NV | 0; NV | MIT |

Two projects with near-identical names should not be silently conflated: [foreleven/agent-relay](https://github.com/foreleven/agent-relay) is distinct from AgentWorkforce's Agent Relay, and [anYuJia/agentbridge](https://github.com/anYuJia/agentbridge) is distinct from the AgentBridge implementation above. Similarly, the two Concord repositories are different systems.

**Crucial architectural distinction:** A system may support acknowledged message *delivery* without demonstrating acknowledged *processing*. Only the latter closes the crash window between receiving a message and applying its effects.

### 2B. Vendor runtimes, local orchestrators, and session managers

In this table, governance and consensus refer to independent coding-agent coordination, not ordinary repository permissions or vendor account policy.

| Project / source | Transport and agents | Multi-PC; server | Claim / lease | ACK / consensus | Human tiers / governance | Cost tracking | Stars; last commit | License |
|---|---|---|---|---|---|---|---|---|
| [Claude Code Agent Teams](https://code.claude.com/docs/en/agent-teams) | Native team inbox/tasks; Claude only | Local teams; cross-PC team guarantees NV | Y, atomic team task ownership | Y peer messages; no general quorum | Human supervision, not Tier-2 canon | Native session costs; team budget NV | N/A | Proprietary |
| [Claude Code cross-session messaging](https://code.claude.com/docs/en/cross-session-messaging) | Unix sockets / Windows named pipes; Claude only | Remote via Anthropic service | — | Y, session message exchange; no quorum | Configurable inbound policy, not shared constitution | Native usage | N/A | Proprietary |
| [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents) | Parent/delegated workers; Codex | Local/session-based | Task delegation | Parent/result; no peer quorum | Native approval controls, not fleet canon | Native usage | N/A | Mixed runtime/service |
| [Cursor cloud agents](https://cursor.com/docs/cloud-agent) | Remote agents and workspaces; Cursor | Y via cloud; yes | Workspace/task separation | Coordination quorum NV | Platform permissions, not peer constitution | Provider usage | N/A | Proprietary |
| [Gemini CLI subagents](https://geminicli.com/docs/core/subagents/) | Native subagents, MCP and A2A remote agents | Y via A2A services | Delegated work | Parent/result; no fleet quorum | CLI security controls, not shared canon | Native usage | See runtime repo | Apache-2.0 |
| [OpenCode](https://opencode.ai/docs/agents/) | Multi-model agent harness | Local / client-server modes | Session/task orchestration | No native hetero CLI quorum | Configurable agents, not exclusive canon | Model/provider usage | Repo metadata NV | MIT |
| [Pi](https://github.com/earendil-works/pi) | Extensible multi-model coding harness | Cross-PC coordination NV | — | — | Extensions, not shared rulebook | Usage support; fleet cost NV | ~115.1k; NV | MIT |
| [Hermes Agent](https://github.com/NousResearch/hermes-agent) | Agent harness with messaging channels | Network channels; gateway/service | P, agent tasks | Channel messages; quorum NV | Local tools/permissions; fleet canon NV | Usage; fleet budgets NV | ~253.7k; NV | MIT |
| [OpenClaw](https://github.com/openclaw/openclaw) | Multi-agent routing through Gateway | Y; Gateway required | Routing/session ownership | Gateway delivery; quorum NV | Per-agent configuration, not consensus canon | NV | ~392k; NV | MIT |
| [Gas Town](https://github.com/gastownhall/gastown) | tmux + Beads; agents in worker roles | Mainly one installation; federation separately | Y, assigned tasks | Role-based workflow; not peer quorum | Mayor/Witness hierarchy, not Tier-2 canon | Operationally expensive; unified budget NV | ~18.4k; NV | MIT |
| [Beads](https://github.com/gastownhall/beads) | Dolt-backed task ledger / CLI; vendor-neutral | P via Dolt/Git remotes | Y locally; offline remote CAS not implied | Task state, not inbox quorum | Task policy, not fleet canon | — | ~27.9k; NV | MIT |
| [Gas City](https://github.com/gastownhall/gascity) | Pluggable agent-orchestration SDK | Configuration-dependent | Y, workflow orchestration | App-level | Configuration-dependent | NV | ~1.4k; NV | MIT |
| [Wasteland](https://github.com/gastownhall/wasteland) | Federated Gas Town coordination / DoltHub | Y through hosted commons | Y, work board | P, trust/validation | P, federated trust rules | NV | ~91; NV | MIT |
| [Paperclip](https://github.com/paperclipai/paperclip) | Central web service; multiple vendor adapters | Y; **server required** | Y, ticket/task management | Work approval; not distributed peer quorum | **Y, human approvals/budgets**, different semantics | **Y, budgets and spend** | ~102.1k; NV | MIT |
| [Claude Squad](https://github.com/smtg-ai/claude-squad) | tmux + Git worktrees; C/X/O/Amp | Primarily local | Worktree isolation | — | Manual operator controls | Provider-side | ~8.6k; NV | AGPL-3.0 |
| [LoopTroop](https://github.com/looptroop-ai/LoopTroop) | OpenCode worktrees / repeated workflows | Primarily local | P, worktree/task separation | Recovery flow, no quorum | Human PR workflow | NV | 162; NV | MIT |
| [cmux](https://github.com/manaflow-ai/cmux) | macOS terminal multiplexer / socket API | Local UI; separate server products | Session separation | Alerts, not task ACK | — | NV | ~28.1k; NV | GPL-3.0-or-later desktop; BUSL-1.1 server/relay |
| [Conductor](https://www.conductor.build/docs) | macOS GUI + Git worktrees; C/X/U/O integrations | Mostly local | Worktree/task separation | Human review, not quorum | Manual review | Provider-side | NV; NV | Proprietary |
| [amux](https://github.com/mixpeek/amux) | Rust local server / board; C/X/G/O | Dashboard network access; peer federation NV | Y, task ownership | Verification-oriented; quorum NV | Human review, not Tier-2 canon | NV | 534; NV | MIT + Commons Clause |
| [ai-maestro](https://github.com/23blocks-OS/ai-maestro) | Central session manager/dashboard; C/X/Grok | Y; manager service | P, session/task management | Messaging; quorum NV | Operator control | NV | 820; NV | MIT |
| [agent-deck](https://github.com/asheshgoplani/agent-deck) | Local TUI/tmux; heterogeneous CLIs | Primarily local | Session ownership | No durable quorum | Operator control | NV | ~1.1k; NV | MIT |

The desktop [cmux](https://github.com/manaflow-ai/cmux) is not the unrelated [TeamADAPT/cmux](https://github.com/TeamADAPT/cmux). Its mixed licensing matters: do not assume the GPL desktop license also applies to server or relay deployment.

Similarly, [amux's license](https://github.com/mixpeek/amux/blob/main/LICENSE) includes Commons Clause restrictions, and [MCP Agent Mail Rust's license](https://github.com/Dicklesworthstone/mcp_agent_mail_rust/blob/main/LICENSE) includes an additional rider. Neither should be represented unconditionally as ordinary OSI-approved MIT.

---

## 3. Q1 — The market: what layer is each project actually building?

### 3.1. The useful taxonomy has six layers

6. Governance and authorization: human approvals, policy changes, identity, audit, trust boundaries
5. Coordination and consensus: claims, leases, voting, task ownership, conflict resolution
4. Durable messaging: mailboxes, receipts, replay, acknowledgment, wakeup
3. Task and artifact state: Dolt, Git, SQLite, tickets, worktrees, source ownership
2. Session orchestration: spawn, inspect, restart, route agents, track usage
1. Agent runtimes: Claude Code, Codex, Cursor, Gemini, OpenCode, Pi, Hermes

Architectural classification for this analysis. A project can span several layers; a terminal multiplexer is not automatically a distributed coordination protocol.

A common market-map error is to put all tools branded "swarm" in one category. A terminal session launcher, a message transport, and an authorization policy solve different problems.

For the lab described here, layers 4–6 are the differentiating concerns. Layers 1–3 have numerous viable alternatives.

### 3.2. Vendor-built coordination is no longer trivial

**Claude Code is the most immediate source of feature overlap.**

Its [Agent Teams documentation](https://code.claude.com/docs/en/agent-teams) describes teammates exchanging direct messages and coordinating through a shared task list. Task claims are part of that team workflow. Completion hooks can check outcomes rather than always trusting an agent's self-reported final message.

Its newer [cross-session messaging documentation](https://code.claude.com/docs/en/cross-session-messaging) describes communication among independently started Claude sessions. On Unix-like systems it uses local sockets; on native Windows it uses named pipes. Remote communication relies on Anthropic's infrastructure.

This has two implications:

1. A generic promise such as "Claude sessions can send each other messages without the user copying text" is no longer distinctive.
2. A promise such as "Claude and Codex can authenticate and coordinate as independent peers, over a serverless fleet bus, under a shared human-approval constitution" remains a different capability.

Native Claude messaging should be treated as a fast local transport, **not** as a replacement for vendor-independent durable coordination.

**Codex, Gemini, and Cursor represent different models.** [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents) emphasize delegation within an agent workflow. [Gemini CLI subagents](https://geminicli.com/docs/core/subagents/) provide native subagent execution, while [remote agents](https://geminicli.com/docs/core/remote-agents/) use A2A for external delegation. [Cursor cloud agents](https://cursor.com/docs/cloud-agent) are much closer to cloud execution and isolated workspaces than to an offline symmetric LAN protocol.

These are genuine coordination features, but neither delegation nor isolated workspaces automatically provide a cross-vendor distributed agreement mechanism.

### 3.3. Multi-model harnesses are not necessarily multi-agent federations

[OpenCode's agent configuration](https://opencode.ai/docs/agents/), [Pi](https://github.com/earendil-works/pi), and [Hermes Agent](https://github.com/NousResearch/hermes-agent) reduce dependence on a single model provider. They are useful if one is willing to run different models behind a common harness.

But *multiple models inside one product* is weaker than *independently running Claude Code, Codex CLI, Cursor, Grok CLI, and Gemini processes becoming equal protocol participants*.

The distinction becomes important with:

- independently maintained memory and context;
- different vendors' permission prompts;
- separate process lifecycles;
- separate machine identities;
- incompatible tool-execution semantics;
- independent failure recovery.

[OpenClaw's multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent/index.html) illustrates the distinction. Multiple agent identities can coexist behind one Gateway. That is useful centralized routing, not evidence of an infrastructure-free federation of peers.

### 3.4. Orchestrators largely optimize human throughput, not distributed correctness

**Gas Town and Beads.** Steve Yegge's [Gas Town](https://github.com/gastownhall/gastown) has an organizational hierarchy and work-management machinery. [Beads](https://github.com/gastownhall/beads) provides structured task state and dependency handling. This is highly relevant to preventing forgotten work, but neither should be mistaken for a machine-independent cryptographic approval constitution.

Beads deserves a specific correction: the current canonical system is **Dolt-backed**. Dolt/Git synchronization and task claims are not evidence that independently offline machines have acquired a linearizable distributed claim primitive. Those are separate consistency questions.

Gas Town also has an expanding ecosystem: [Gas City](https://github.com/gastownhall/gascity) develops pluggable orchestration, while [Wasteland](https://github.com/gastownhall/wasteland) explores federation across installations. This weakens any claim that the Gas Town family has no interest in cross-machine governance, but its published model is different from a serverless, leader/follower rulebook.

**Paperclip is a particularly important market threat.** Its [governance and budget model](https://github.com/paperclipai/paperclip#-governance) combines multi-agent organization, human checkpoints, spending visibility, and budgets. A small lab that primarily wants to supervise expensive agents may prefer this integrated product.

Paperclip does not meet the strict no-server requirement, and its organizational approvals should not automatically be equated with machine-enforced, cryptographically bound approval of every Tier-2 irreversible action.

**Conductor, Claude Squad, LoopTroop, cmux, and agent-deck** are useful for local parallelism. Their primary benefit is separating workspaces and making sessions observable. Relevant implementations:

- [Conductor's parallel-agent model](https://www.conductor.build/docs/concepts/parallel-agents)
- [Claude Squad](https://github.com/smtg-ai/claude-squad)
- [LoopTroop](https://github.com/looptroop-ai/LoopTroop)
- [cmux documentation](https://cmux.com/docs/getting-started)
- [agent-deck](https://github.com/asheshgoplani/agent-deck)

None of those capabilities, by itself, establishes a cross-machine ACK debt, quorum protocol, or governance authority.

### 3.5. The bus and mailbox category is the closest direct competition

There are several distinct strategies.

**Local SQLite buses.** [hcom](https://github.com/aannoo/hcom), [Concord MCP](https://github.com/Get-Concord-AI/concord-mcp), and [agent-mesh](https://github.com/johnmwhitman/agent-mesh) make local messaging inexpensive and practical. They can integrate with existing agents, including through MCP.

The hard boundary is remote consistency: a SQLite database shared by multiple worktrees on one machine is not the same thing as a distributed database across eight asynchronously connected machines.

Concord's documented atomicity applies to its local database/checkout. Do not extrapolate that to Syncthing replication.

**Server-backed buses.** [llm-bus](https://github.com/danieldoderlein/llm-bus), [AgentTeams](https://github.com/agentscope-ai/AgentTeams), [OpenAgentMesh](https://github.com/openagentmesh/openagentmesh), and [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust) choose a service as the communication and/or coordination point.

This is often the simpler engineering trade-off. It avoids inventing a distributed consistency mechanism, at the cost of operating a service and accepting its availability and security properties.

**Filesystem and Git coordination.** [agentbus](https://github.com/yanglinfang/agentbus), [agent-sync](https://github.com/ssheleg/agent-sync), and [system-gap-master](https://github.com/ellmos-ai/system-gap-master) are particularly important to claw-consensus because they occupy a more similar deployment niche.

They demonstrate that avoiding a custom server is not a novel idea in itself. Their guarantees, however, differ substantially.

**Intent-first systems.** [foremerge](https://github.com/naw103/foremerge) attacks a different coordination problem: detecting incompatible planned edits *before* they collide. This is valuable even if transport and consensus are already solved.

Its explicit limitations should be respected: a local SQLite-based intent system is not automatically a cross-machine atomic lease manager.

### 3.6. Protocols: which ones do coding agents actually use?

| Protocol | Purpose | Relevant practical adoption | What it does **not** provide |
|---|---|---|---|
| [MCP](https://modelcontextprotocol.io/specification/2025-11-25) | Host/client-to-tool interfaces; resources, tools, prompts | Concrete MCP interfaces in Agent Mail, Concord, agent-mesh, and other coding-agent integrations | No standard quorum, distributed lease, or shared human-approval constitution |
| [A2A](https://a2a-protocol.org/latest/specification/) | Agent discovery via Agent Cards and inter-agent tasks/messages | [Gemini CLI remote agents](https://geminicli.com/docs/core/remote-agents/) are a concrete example | No general file-write ownership, governance voting, or Tier-2 authorization |
| [IBM Agent Communication Protocol](https://research.ibm.com/projects/agent-communication-protocol) | Agent communication, subsequently consolidated into A2A | Incorporated into the A2A effort | Not a separate new local coding-agent consensus solution |
| [Zed Agent Client Protocol](https://zed.dev/acp) | Editor ↔ coding-agent session protocol | Editor/agent integrations | Not an agent-to-agent governance framework |
| [ANP](https://agent-network-protocol.com/) | Decentralized agent-network identity/discovery and interaction | Broad local coding-CLI adoption NV | No demonstrated end-to-end fleet approval system |
| [OASF](https://github.com/agntcy/oasf) | Standard descriptions and schemas for agents | Discovery/metadata layer | Not a message transport or locking system |
| [Agent Messaging Protocol](https://github.com/agentmessaging/protocol) | Signed, federated agent messaging | Concrete broad adoption across local CLIs NV | Not a complete implementation of file ownership or human-tier governance |
| [ERC-8004](https://eips.ethereum.org/EIPS/eip-8004) | On-chain identity, reputation, and validation registries | Relevance to eight-PC coding fleets is speculative | Not a replacement for local message processing and source-file locks |

An essential naming clarification: IBM's **Agent Communication Protocol** joined A2A, as described in the [LF AI & Data announcement of August 29, 2025](https://lfaidata.foundation/communityblog/2025/08/29/acp-joins-forces-with-a2a-under-the-linux-foundations-lf-ai-data/). Zed's **Agent Client Protocol**, also called ACP, is a different and active standard. Treating "ACP" as one protocol produces misleading market comparisons.

**My practical ranking for adoption by independent local coding agents is MCP first, native vendor messaging second, A2A third.** That is an engineering judgment based on the concrete integration surfaces above, not a measured market-share statistic. The other standards remain relevant as interoperability or identity building blocks, but I did not verify comparable deployment across the specified CLI fleet.

---

## 4. Q2 — Attempt to falsify the five-property market-gap claim

### The five tests

- A. Heterogeneous vendors: independent peers, not models hidden inside one manager
- B. Multiple machines: actual cross-host coordination, not just remote UI access
- C. No dedicated coordination server: filesystem synchronization, Git, or peer transport accepted
- D. Machine-enforced human approval tiers: sensitive actions stopped until authenticated authorization
- E. Explicit rulebook-change authority: governance over who may amend the governing policy

### Closest three challengers

| Property | Paperclip | [agent-sync](https://github.com/ssheleg/agent-sync) | [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm) |
|---|---|---|---|
| A. Heterogeneous equals | P — multiple adapters, centrally managed | P — multiple runtimes, strongest enforcement Claude-specific | P — framework-level peers; requested CLIs not turnkey |
| B. Multi-machine | Y through service | **Y via remote Git** | P — network components, complete fleet operation NV |
| C. No dedicated server | **N** | **Y, with Git remote dependency** | P/NV |
| D. Enforced human tiers | P — workflows/approvals, not same Tier-2 rule set | N | P — constitutional controls, exact irreversible-action tiers NV |
| E. Rulebook-change authority | P — organization roles and configurable rules, not exclusive replicated canon | N | P — constitutional governance, not verified single-writer canon |
| Verified all five? | **No** | **No** | **No** |

Sources: [Paperclip governance](https://github.com/paperclipai/paperclip#-governance), [agent-sync architecture](https://github.com/ssheleg/agent-sync), [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm).

An additional architectural near match is [system-gap-master](https://github.com/ellmos-ai/system-gap-master). Its combination of synced folders and per-machine single-writer slots is unusually close to claw-consensus's transport model. However, the complete cross-vendor consensus, authenticated Tier-2 human authorization, and exclusive rulebook governance are not verified there.

**Result: the strict all-five claim survived this search, but only as an absence of a verified counterexample.** It is not proof that no unpublished tool, private deployment, or overlooked public repository exists.

There is also a subtler falsification: if "serverless" is meant to imply *no centralized point of serialization whatsoever*, then a Git remote enforcing compare-and-swap is not fully decentralized. A Git remote is still an authority for the relevant reference. Under the user's operational definition—no dedicated agent-coordination server—it qualifies, but under a stronger distributed-systems definition it does not.

### What about the governance-gap paper?

The requested paper, [*Governance Gaps in Agent Interoperability Protocols*, arXiv:2606.31498](https://arxiv.org/abs/2606.31498), analyzes a governance problem that is orthogonal to ordinary message connectivity.

Its identified gaps concern:

| Governance gap | Meaning for a coding-agent fleet | Evidence of progress since the paper |
|---|---|---|
| Membership | Who is recognized as a participating agent? | Signed identities and registries in [AMP](https://github.com/agentmessaging/protocol) and [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm) |
| Deliberation | How are proposals and objections exchanged? | Messaging and council patterns in [agent-mesh](https://github.com/johnmwhitman/agent-mesh), [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm), and claw-consensus |
| Voting/quorum | What constitutes sufficient agreement? | Signed voting/quorum in [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm) |
| Dissent | How are objections represented rather than discarded? | Project-specific proposal/reject structures; interoperable implementation NV |
| Human escalation | Which decisions require a human? | Human approval workflows in [Paperclip](https://github.com/paperclipai/paperclip#-governance) and tier policies in [claw-consensus](https://github.com/tonydzi/claw-consensus/blob/main/docs/GOVERNANCE.md) |
| Audit and replay | Can a third party reconstruct a decision? | Receipts/event journals across several projects; proof of tamper-resistant cross-machine replay varies |

**Has anyone closed the gap list?**

At an application level, **parts of it have been addressed**, including cryptographically signed votes and human approval workflows. At the level of an interoperable standard widely implemented by independent local coding agents, I did **not** verify closure.

That distinction is important. An agent protocol can transmit a signed `vote` event without specifying who is entitled to vote, who changes the voting rules, or what happens when the network partitions. Conversely, a project may enforce a local constitution without having an interoperable protocol for other implementations.

A possible positioning statement for claw-consensus is therefore:

> A local-first coordination and governance protocol for independently running coding agents across multiple machines, with explicit message accountability, authenticated decision records, human escalation, and rulebook ownership.

It should **not** claim to have invented multi-agent messaging, local shared-folder transport, agent identity signatures, file reservations, or consensus voting.

---

## 5. Q3 — What an eight-machine lab should adopt now

The estimates below are **my engineering estimates**, not published vendor setup benchmarks. Assumptions: one experienced engineer; existing Syncthing and Git installations; agents already run; no major redesign of the fleet; installation, integration, a basic failure test, and documentation included.

### Ranked recommendations

| Rank | Technique | Reference implementation | Estimated engineering time | Value for eight machines |
|---|---|---|---|---|
| 1 | Atomic cross-machine task ownership | [agent-sync](https://github.com/ssheleg/agent-sync) | 8–16 h | **Critical** |
| 2 | Authenticated, persistent ACK and processing receipts | [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust) | 6–12 h to adapt concepts | **Critical** |
| 3 | Intent + scope declarations before writes | [foremerge](https://github.com/naw103/foremerge) | 4–8 h | **High** |
| 4 | Thin MCP façade over existing bus | [Concord MCP](https://github.com/Get-Concord-AI/concord-mcp), [Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust) | 8–16 h | **High** |
| 5 | Verify-gated completion with independent evidence | [Claude Agent Teams hooks](https://code.claude.com/docs/en/agent-teams), [claw reference](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | 4–10 h | **High** |
| 6 | Task/dependency ledger | [Beads](https://github.com/gastownhall/beads) | 2–4 h trial; 8–24 h integrated | Medium |
| 7 | Native session-to-session transport where appropriate | [Claude messaging](https://code.claude.com/docs/en/cross-session-messaging), [hcom](https://github.com/aannoo/hcom) | 1–4 h trial | Medium |
| 8 | Read-only operator cockpit and budget summaries | [ai-maestro](https://github.com/23blocks-OS/ai-maestro), [Paperclip](https://github.com/paperclipai/paperclip) | 8–20 h for a minimal own view | High for usability; not core correctness |

### 5.1. First: solve cross-machine atomic claims

The strongest immediate lesson from [agent-sync](https://github.com/ssheleg/agent-sync) is that multiple agents can all believe an operation succeeded while the merged result loses work. Its author reports a test where 12 parallel append operations returned success, yet only three resulting lines remained.

That illustrates why an asynchronous file-sync directory is not a global atomic claim mechanism.

An example of a split-brain claim: Machine A sees task T as unclaimed and writes claim(A, T); Machine B sees task T as unclaimed and writes claim(B, T); Syncthing eventually delivers both claims. Convergence detects the collision, but it cannot prevent both workers from having already made side effects.

A deterministic tie-break after synchronization is useful for reconciliation. It is not equivalent to an exclusive lease that prevents conflicting work.

**Recommended approach:** Reserve a task through a Git remote reference update using compare-and-swap semantics. Include a claim epoch/fencing token. Do not allow writes from a
worker whose claim is superseded.

A minimal persistent record could be:

```json
{
"task_id": "T-184",
"owner": "machine-b/agent-07",
"claim_epoch": 19,
"expires_at": "2026-10-14T22:00:00Z",
"scope": ["src/billing/**"],
"base_commit": "8f2c...",
"policy_hash": "sha256:...",
"signature": "ed25519:..."
}
```

Caveat: TTL alone is not enough. A worker may pause, miss renewal, and resume after another worker has acquired the task. The **fencing epoch must be checked at the point of a protected side effect**, or the old worker can still write.

For eight machines, a Git-based claim authority is a reasonable low-cost compromise. It is less elegant than true peer-to-peer consensus, but considerably easier to reason about.

### 5.2. Separate received, accepted, processed, and verified

The lab's ACK-debt model is directionally correct but needs precise semantics.

I recommend four distinct states:

1. **RECEIVED** — message bytes are durably stored by the recipient.
2. **ACCEPTED** — the recipient has inspected the request and explicitly accepted ownership.
3. **PROCESSED** — the intended action completed, with a durable result reference.
4. **VERIFIED** — the result passed independent checks under the applicable policy.

These states should be individually idempotent. Each transition should reference an immutable message ID and be signed by the actor permitted to make that transition.

The [nats-channel-mcp implementation](https://github.com/except-pass/nats-channel-mcp) demonstrates why this distinction matters: its documented transport ACK may happen after successful output to the receiving process but **before Claude has actually processed the message**. A crash during that interval creates a lost-processing window despite a transport-level acknowledgment.

This is a documented design limitation, **not** evidence that the author experienced a particular production loss event.

[MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust) is a useful reference for persistent inboxes and acknowledgment-oriented collaboration. But replacing Syncthing with Agent Mail would add a service. I would adopt its semantics first, not necessarily its whole deployment model.

### 5.3. Use intent and scope declarations without treating them as locks

[foremerge](https://github.com/naw103/foremerge) provides the relevant pattern: before agents edit, they disclose what they intend to change.

Example:

```json
{
"type": "INTENT",
"task_id": "T-184",
"actor": "machine-b/agent-07",
"operation": "modify",
"scope": {
"files": ["src/billing/invoice.ts"],
"symbols": ["InvoiceService.calculateTax"]
},
"base_commit": "8f2c...",
"expires_at": "2026-10-14T22:00:00Z"
}
```

That gives peers an opportunity to detect potential interference without relying on exact filename overlap alone.

However:

- Intent declarations are advisory until enforced.
- Scope reservations should expire if a worker disappears.
- TTL expiry must not by itself authorize stale writers.
- Shared-file rules should include generated files, configuration, schemas, and dependency manifests.
- Source-code symbol scopes are useful but imperfect when edits also alter call sites or build artifacts.

**For eight machines:** definitely worthwhile. These are cheap, human-readable signals that can reduce duplicate work even when a more formal atomic-claim layer exists.

### 5.4. Add an MCP adapter, not a second protocol universe

A standard MCP front end would make claw-consensus accessible as ordinary tools to independent coding agents.

Suggested minimal interface:

```text
fleet_status()
fleet_inbox()
fleet_send(target, message, requires_ack)
fleet_claim(task_id, scope, ttl)
fleet_ack(message_id, state, evidence_ref)
fleet_verify(task_id, evidence_ref)
```

The MCP server could be a **local stdio process** reading and writing the existing bus. It does not have to become a fleet-wide HTTP authority.

Two critical security requirements:

1. The adapter must receive a trusted identity from its launcher or operating environment. It must not trust a model-supplied `actor` string.
2. High-risk operations must pass through a non-model authorization gate. Providing a tool named `request_human_approval` does not make subsequent self-reported approval trustworthy.

Examples of practical MCP integration surfaces exist in [Concord MCP](https://github.com/Get-Concord-AI/concord-mcp), [agent-mesh](https://github.com/johnmwhitman/agent-mesh), and [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust).

One operational note: [Concord MCP's documentation](https://github.com/Get-Concord-AI/concord-mcp#privacy--telemetry) says telemetry can be disabled through `CONCORD_TELEMETRY_DISABLED=1`. That matters if the lab requires local-first metadata handling.

### 5.5. Completion must be a state transition, not an utterance

"Done is a claim and needs evidence from a second method" is a good principle, but it should be encoded as a gate.

Recommended minimum evidence:

| Work type | Primary claim | Independent evidence |
|---|---|---|
| Source-code change | Agent says implementation finished | Commit hash, diff, tests executed under a controlled runner |
| Bug fix | Agent says defect resolved | Reproducing test fails before, passes after |
| Configuration change | Agent says config updated | Parser/validator output, deployment read-back |
| File transfer | Sender reports completion | Receiver-side manifest and content hash |
| Sensitive external action | Agent reports success | Authenticated approval plus independently obtained action receipt |
| Review | Reviewer accepts | Reviewer identity distinct from implementer, review evidence linked to exact commit |

A second agent's opinion is not automatically independent evidence. Two agents reading the same contaminated log or model-generated test result can agree on a false conclusion.

The strongest practical version of this pattern is **different mechanism, different trust boundary, and the same exact artifact hash**.

### 5.6. Should the lab replace its task ledger with Beads?

Not immediately.

[Beads](https://github.com/gastownhall/beads) is worth trialing for dependency graphs, resumable tasks, and work queues. But the lab already has work claims and a bus. Installing a second task authority can introduce a new failure mode:

> The bus says task T belongs to A, while the ledger says it belongs to B.

A safe adoption path is to let Beads manage *task descriptions and dependencies*, while one clearly designated mechanism remains authoritative for *cross-machine ownership*.

Do not introduce two writable sources of truth for leases.

### 5.7. Failure modes and deployment trade-offs for the proposed upgrades

| Technique | Plausible failure / reported issue | Mitigation |
|---|---|---|
| Git CAS leases | Git remote unreachable; stale agent resumes; retry storms | Fail closed for new exclusive work, fencing epochs, jittered retry |
| ACK mailbox | ACK emitted before processing; agent never polls | Separate durable receipt/processing states; wakeup plus watchdog |
| Intent declarations | False confidence in voluntary declarations; lock wars | Advisory collision warning plus enforced ownership at critical operations |
| MCP façade | Model impersonates agent or invents approval | Launcher-bound machine identity, signature verification, external approval service |
| Verify-gated completion | Reviewer copies same evidence; model fabricates test output | Controlled test runner, artifact hashes, reviewer separation |
| Beads ledger | Two competing task authorities; remote reconciliation ambiguity | One authoritative claim backend; ledger remains an indexed view |
| Native session messaging | Idle sessions, missed wakeups, provider-specific behavior | Durable bus remains source of truth; native messaging is a notification channel |
| Cockpit | Attractive UI hides stale or missing transport data | Expose per-machine freshness, last verified ACK, channel health and unresolved debts |

Several of these mitigations are engineering recommendations rather than confirmed defaults in the referenced projects.

---

## 6. Q4 — Documented failure modes in the wild

Here I separate **maintainer-reported incidents** from **known design hazards** and **research attacks**. Combining all three into one incident count would inflate the evidence.

### 6.1. Gas Town: duplicate fixes, data loss, and chaotic coordination

Steve Yegge's [*Welcome to Gas Town*](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) describes the cost of orchestrating large numbers of coding agents: agents can duplicate work, coordination can become chaotic, and heavy parallel execution can be expensive.

His later [Gas Town v1.0 retrospective](https://steve-yegge.medium.com/gas-town-from-clown-show-to-v1-0-c239d9a407ec) describes early data-loss and orchestration problems, including agents being interrupted by supervisory machinery.

The important lesson is **not** that Gas Town is uniquely fragile. It is that a sophisticated orchestrator does not eliminate operational problems caused by concurrent source edits and process lifecycle management.

Mitigations visible in the Gas Town approach include a task ledger, dedicated supervisory roles, structured handoffs, Git worktrees, and more explicit worker recovery. See the [Gas Town repository](https://github.com/gastownhall/gastown).

I did not verify a defensible universal **dollars-per-hour** number for Gas Town. Costs vary with concurrent sessions, provider plans, model choice, token volume, and worker supervision. Quoting a specific hourly cost without a metered run would be misleading.

### 6.2. Agent inboxes are not necessarily polled

The same [Gas Town account](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) discusses the difficulty of getting agents to act on their intended startup instructions and messages. The tooling uses explicit nudging and supervision rather than assuming every agent spontaneously observes new work.

For claw-consensus, the implication is serious:

**An ACK SLA is meaningful only if a recipient has a reliable opportunity to observe the message.**

A durable mailbox is necessary but insufficient.

A robust implementation needs a liveness model covering:

- recipient process alive;
- inbox successfully scanned;
- message made visible to the model/session;
- message durably accepted;
- process restart between receipt and acknowledgment.

Otherwise the system can classify a transport or wakeup fault as agent negligence.

### 6.3. agent-sync: acknowledged operations can lose writes

The author's concurrency test in [agent-sync](https://github.com/ssheleg/agent-sync) is one of the most useful small-scale reproductions: 12 concurrent append operations reportedly returned success, but only three lines survived.

This is direct evidence of why successful command exits should not be accepted as evidence of consistent shared state.

The documented response is to use Git remote compare-and-swap for exclusive task leases.

Caveat: a Git CAS lease prevents multiple successful acquisitions of the same current reference when used correctly; it does **not** automatically stop an old worker from writing after its lease has expired. Fencing and operation-level enforcement are still necessary.

### 6.4. claw-consensus: real transport incidents

The project's own [failure-modes report](https://github.com/tonydzi/claw-consensus/blob/main/docs/FAILURE-MODES.md) is unusually useful because it distinguishes several classes of production failure.

| Incident | Reported evidence | Generalizable cause | Appropriate mitigation |
|---|---|---|---|
| Synced-folder conflicts | A report of 78 pending conflicts during a synchronization incident | Append-only at logical level is not enough if the filesystem's writer contract is violated or files collide | Strict single-writer shards, conflict quarantine, reconciler with provenance checks |
| Sender-only delivery assurance | Audit of 315 parcels found a subset without adequate content-manifest evidence | Sender's "sent" is not recipient's "received" | Receiver-side manifest, length/hash verification and explicit receipt |
| Silent channel failure | A channel was reportedly stale for 5.4 days despite reassuring aggregate liveness | Latest timestamp across channels masks a dead subchannel | Independent freshness checks for each rail |
| Detection without execution | Conflict detectors were present without effective repair execution | Observability is not a recovery policy | Distinguish alerting, remediation and proof of remediation |

These are stronger credibility evidence than a polished demo because they expose actual failure classes.

They are not, however, proof that the published reference engine currently enforces every proposed mitigation.

### 6.5. NATS channel: the receive/process gap

As noted above, [nats-channel-mcp](https://github.com/except-pass/nats-channel-mcp) explicitly documents a risk created when a message is acknowledged after delivery to the MCP-facing process but before the agent has processed it.

Potential mitigations include durable JetStream storage, application-level acknowledgments, replay support, and idempotency keys.

This is a **documented loss window**, not a separately verified account of a particular message being lost during production.

### 6.6. Deadlocks, lease wars, and tasks stuck "in progress"

I did not find sufficiently specific primary-source incident reports to quantify these across the requested projects as a class. Their existence as design hazards is straightforward, but frequencies would be speculative.

A useful failure-model matrix for the lab is:

| Situation | Affected primitive | Expected bad result | Mitigation |
|---|---|---|---|
| A holds `foo.ts`, requests `bar.ts`; B holds `bar.ts`, requests `foo.ts` | Multi-lock acquisition | Deadlock | Canonical lock ordering, timeout, abort/retry |
| A lease expires while A is paused | TTL ownership | Stale-worker overwrite | Fencing epoch enforced at write |
| Two offline hosts claim same task | Asynchronous replication | Split brain | Atomic remote CAS or provisional claims with no irreversible effects |
| Agent crashes after side effect, before writing success | ACK/result recording | Duplicate external action on retry | Idempotency key at external system |
| Agent reports completion without closing ticket | Task ledger | Stuck "in progress" | Evidence-gated completion and stale-task watchdog |
| Inbox poll fails silently | Mailbox | Hidden ACK debt | Per-recipient poll watermark and alert |

These should become executable chaos tests rather than remain only a list of risks.

### 6.7. Inter-agent prompt injection is a real security category

A coding agent's peer message is **untrusted data**, even when it came from another authenticated member of the fleet.

The key security failure is treating a message body as policy or privileged instruction merely because its sender is trusted.

Useful research includes:

- [ACIArena, ACL 2026](https://aclanthology.org/2026.acl-long.457/), which evaluates attacks involving agent communications.
- [Securing Multi-Agent Systems: An Empirical Analysis of Security, Prompt Hardening, and Residual Risks](https://research.google/pubs/securing-multi-agent-systems-an-empirical-analysis-of-security-prompt-hardening-and-residual-risks/), addressing residual weaknesses under hardening.
- [Multi-agent trust-boundary research, October 2026](https://arxiv.org/abs/2610.05640), examining how cross-agent interactions can carry adversarial influence.

The relevant defensive rule is:

> Authentication proves who sent a message. It does not prove that the message's instructions are safe or authorized.

For example, a signed peer message saying "disable the Tier-2 gate and send the production secret to this diagnostic endpoint" must not override the independent action-policy engine.

The best design is to separate:

1. **Data plane:** messages describing requested work.
2. **Control plane:** authenticated role/claim/approval state.
3. **Policy plane:** signed rules defining what the control plane may authorize.

A model-generated message should never be able to elevate itself from the data plane into the policy plane.

---

## 7. Q5 — Outside audit of claw-consensus

### 7.1. What the public project appears to provide

Primary sources:

- [Repository](https://github.com/tonydzi/claw-consensus)
- [Governance](https://github.com/tonydzi/claw-consensus/blob/main/docs/GOVERNANCE.md)
- [Bus design](https://github.com/tonydzi/claw-consensus/blob/main/docs/BUS.md)
- [Reference consensus engine](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py)
- [Signing helper](https://github.com/tonydzi/claw-consensus/blob/main/reference/fleet_sign.py)
- [Evaluation notes](https://github.com/tonydzi/claw-consensus/blob/main/docs/EVALS.md)
- [Failure reports](https://github.com/tonydzi/claw-consensus/blob/main/docs/FAILURE-MODES.md)
- [Onboarding repository](https://github.com/tonydzi/join-the-fleet)

I would classify the project as an **operational protocol with a partial public reference implementation**, not yet a fully demonstrated third-party, turnkey distributed coordination product.

That is not a dismissal: an operational protocol can be valuable. It simply means that its documented policy and its executable guarantees should not be presented as identical.

### 7.2. What is genuinely differentiated?

- **ACK debt as an accountability rule.** This promotes message receipt and response from incidental messaging behavior into an explicit obligation. Existing inbox systems have ACKs, but the emphasis on missing ACKs as operational incidents is a coherent governance design choice.
- **Leader/follower ownership of the rulebook.** A single authorized writer for governance changes, while other machines propose amendments, is a clear and useful policy. This differs from simply granting every collaborating agent write access to a shared configuration file.
- **Human approval treated as part of machine governance.** Distinguishing routine agent operations from money movement, irreversible deletion, credential disclosure, and mass outbound actions provides a better safety model than an informal "ask if unsure."
- **Evidence-oriented completion.** The project connects task lifecycle governance to independent verification rather than stopping at message delivery and task assignment.

The differentiation is in the **composition and operational contract**, not in the novelty of signatures, leases, synchronization, or message buses individually.

Sources: [Governance](https://github.com/tonydzi/claw-consensus/blob/main/docs/GOVERNANCE.md), [Bus](https://github.com/tonydzi/claw-consensus/blob/main/docs/BUS.md), [Evals](https://github.com/tonydzi/claw-consensus/blob/main/docs/EVALS.md).

### 7.3. What is largely reinvention?

| Existing idea | Older or parallel implementation | How claw-consensus should position it |
|---|---|---|
| Persistent inbox and ACKs | [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust), [llm-bus](https://github.com/danieldoderlein/llm-bus) | Reuse semantics and interoperate, rather than claim first invention |
| Task claims and leases | [Beads](https://github.com/gastownhall/beads), [agent-sync](https://github.com/ssheleg/agent-sync) | Specify what is atomic and what is advisory |
| Pre-write conflict detection | [foremerge](https://github.com/naw103/foremerge) | Adopt intent/scope notation where practical |
| Signed agent identity | [Agent Messaging Protocol](https://github.com/agentmessaging/protocol), [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm) | Make signature verification mandatory at trust boundaries |
| Proposal and voting patterns | [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm), [agent-mesh](https://github.com/johnmwhitman/agent-mesh) | Be explicit about the consensus model and failure assumptions |
| Multiple agent sessions | [Claude Squad](https://github.com/smtg-ai/claude-squad), [cmux](https://github.com/manaflow-ai/cmux) | Avoid building another session manager |
| Human approvals and budgets | [Paperclip](https://github.com/paperclipai/paperclip) | Emphasize rule authority, policy-bound approvals, and serverless fleet operation |

This is actually good news for development costs. It means the project can remain small by adopting proven surrounding mechanisms.

### 7.4. Code-level findings that deserve attention

The following are based on the **public reference implementation**. They do not establish that the private live fleet has the same vulnerabilities.

| Finding | Public evidence | Severity / implication | Required change |
|---|---|---|---|
| Event replay does not visibly verify signatures | [`consensus.py`](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | **High** if arbitrary replicated file content is trusted | Verify signed event envelopes before adding events to state |
| Authenticated-human proof is represented by a supplied text reference | [`cmd_approve`](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | **High** if the reference engine is used as a Tier-2 enforcement point | Verify human signer, capability, action digest, expiry and replay protection |
| Leader can commit without the ordinary agreed-state condition | [`cmd_commit`](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | **High conceptual clarity risk**: leader arbitration is not quorum consensus | Separate emergency override from quorum commit, with different event types |
| Duplicate verification evidence is warned about but still recorded | [`cmd_verify`](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | **Medium/high**: warning does not satisfy a strict verification gate | Reject or quarantine invalid proof and refuse state transition |
| Cross-node event projection orders primarily by wall-clock timestamp | [`consensus.py`](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | **Medium**: clock skew can change causal interpretation | Per-node sequence IDs, event dependencies, deterministic projection |
| Append path does not visibly provide process-level serialized writes and durable fsync | [`consensus.py`](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py) | **Medium**: one file per machine is not one writer process per machine | Local single-writer daemon or OS lock, atomic append discipline, explicit durability |
| Evaluation does not establish real multi-machine fault tolerance | [`EVALS.md`](https://github.com/tonydzi/claw-consensus/blob/main/docs/EVALS.md) | **High evidence gap**, not proof of failure | Public repeatable multi-host crash/partition tests |

#### Finding A: signed transport is not the same as verified state

The repository includes a signing helper, [`fleet_sign.py`](https://github.com/tonydzi/claw-consensus/blob/main/reference/fleet_sign.py), with Ed25519-related functionality.

However, the reference engine's event-loading path does not visibly require every consumed event to pass the signature-verification boundary before it participates in state reconstruction.

That is a major distinction:

- "Events **can** be signed" is a cryptographic capability.
- "Unsigned or incorrectly signed events **cannot alter state**" is an enforced security property.

For a system with distributed approval and governance, only the second is sufficient.

#### Finding B: a claimed human approval must bind to the exact action

A proof-reference string is not, by itself, a human signature.

A suitable approval capability would bind at least:

```json
{
"approval_id": "APR-2026-1042",
"human_principal": "owner-key-01",
"action": "transfer_funds",
"action_digest": "sha256:...",
"scope": {
"account": "approved-account",
"max_amount": "100.00",
"currency": "EUR"
},
"expires_at": "2026-10-14T22:00:00Z",
"nonce": "unique-single-use",
"policy_hash": "sha256:...",
"signature": "ed25519:..."
}
```

The runtime must verify that the approval is authentic, current, not previously consumed, and matches the **exact proposed side effect**.

For irreversible financial or external operations, this check should happen outside the LLM and immediately before execution.

#### Finding C: an arbiter is not automatically consensus

The published reference engine allows the designated leader to bypass the ordinary agreed-state requirement for a commit.

This might be an intentional *leader override* feature. If so, name it that.

Otherwise "consensus" becomes ambiguous: the protocol can look like proposal/counter/accept/commit while ultimately permitting an authority to unilaterally finalize a decision.

A clean model would distinguish:

| Event | Required authority |
|---|---|
| `PROPOSAL` | Authorized proposer |
| `ACCEPT` / `REJECT` | Eligible voter |
| `QUORUM_COMMIT` | Verifiable voter set and threshold |
| `LEADER_OVERRIDE` | Leader plus explicit override policy |
| `HUMAN_OVERRIDE` | Authenticated human capability |
| `VERIFY` | Independent eligible verifier |

That distinction improves both security auditing and external adoption.

#### Finding D: independent verification must be enforced, not warned about

If a verifier supplies the same copied evidence that the implementer supplied, and the system merely displays a warning while still recording the verification event, then the documented strict independence property is not upheld by that code path.

The correct behavior for a hard verification gate is to refuse the final state transition until valid evidence is supplied.

A warning-only mode can still exist, but it must be called **advisory verification**, not enforced verification.

### 7.5. Documentation versus public implementation

| Capability | Documented? | Public code demonstrating full enforcement? | Operational verification |
|---|---|---|---|
| Per-machine append-only message shards | Y | P | Reported live use; full fault tolerance NV |
| Telegram mirrored visibility | Y | Integration-dependent | Production evidence in failure reports; full end-to-end audit NV |
| ACK debt and SLA | Y | P | Selected live incidents; comprehensive SLA metrics NV |
| File write leases | Y | P/local | Global atomic ownership NV |
| Proposal/counter/accept/reject/commit | Y | Reference path present | Offline evaluation only for the published cases |
| Signed machine identities | Y | Signing helper present | Mandatory validation on all reference-engine input NV |
| Machine-readable human approval tiers | Y | P | Cryptographically enforced Tier-2 execution NV |
| Leader/follower canon | Y | Policy described | Unauthorized edits rejected across all hosts NV |
| Independent verification | Y | P; duplicate-proof rejection incomplete | Full gate effectiveness NV |
| A2A integration | User-described | Public turnkey interop NV | NV |
| MCP interface | Not established as complete | No verified turnkey fleet MCP package | NV |
| Cross-machine atomic claim | Not established | No verified implementation equivalent to Git CAS | NV |
| Reproducible eight-machine partition testing | N in published eval | No | NV |

Sources: [Governance](https://github.com/tonydzi/claw-consensus/blob/main/docs/GOVERNANCE.md), [Bus](https://github.com/tonydzi/claw-consensus/blob/main/docs/BUS.md), [reference implementation](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py), [Evals](https://github.com/tonydzi/claw-consensus/blob/main/docs/EVALS.md).

The public [join-the-fleet](https://github.com/tonydzi/join-the-fleet) material also should not be treated as proof of seamless onboarding to a live authenticated eight-machine swarm. Its public onboarding/skills distribution role is useful, but an installer or skill pack is a different deliverable from joining a trust domain with keys, permissions, claims, and transport configured.

### 7.6. Three changes most likely to increase adoption

1. **Ship a one-command MCP + CLI starter.** A newcomer should be able to start two agents from different vendors on one machine, send a message, ACK it, claim a task and observe the result in under one setup session. Include Windows PowerShell, macOS and Linux paths. Provide a reproducible Claude ↔ Codex example. Treat Syncthing multi-PC setup as a second stage rather than a mandatory first-run requirement. **Impact:** Lower integration friction and wider reach.

2. **Make every advertised guarantee executable.** Require signature verification, authentic human approvals, fencing for exclusive work, replay-safe event processing, separate quorum versus override events, and hard verification gates. Publish a compatibility table that says which guarantees are enforced, advisory, or external integration responsibilities. **Impact:** Converts architectural claims into testable reliability and security properties.

3. **Publish a cockpit plus adversarial benchmark suite.** Read-only views should show active identities, pending ACK debt, claims, lease epochs, policy versions, approval holds, task evidence and per-rail freshness. The benchmark should recreate delayed Syncthing replication, a severed machine, process crashes, duplicate claims, clock skew, bogus human approvals, revoked machine keys and receipt-after-side-effect crashes. **Impact:** Gives outsiders confidence that the protocol is operable, diagnosable, and worth adopting.

A useful initial test target is not "100 agents performed a demo." It is more concrete:

- Eight real hosts or isolated host-equivalent environments.
- At least three heterogeneous coding runtimes.
- Thousands of signed messages with injected duplicates/reordering.
- No unauthorized policy mutation.
- No unauthorized Tier-2 execution.
- No accepted stale-fencing-token write.
- No task marked verified without acceptable evidence.
- Reconciliation after disconnect and restart.
- Reported ACK latency, recovery time, and false-positive incident rates.

Those are **proposed acceptance criteria**, not results achieved by the current public project.

---

## 8. What could not be verified

This section is as important as the comparison tables. Absence of verification must not be silently converted into either "yes" or "no."

| Question | Status |
|---|---|
| Exact GitHub star counts at a single synchronized instant | Approximate snapshots only; rounded values reported |
| Last commit date for every listed repository | **Not verified**; GitHub HEAD timestamps were not consistently retrievable |
| Independent production deployments of most small bus projects | **Not verified**; GitHub READMEs often provide demos rather than independent operators |
| Whether every advertised bus can tolerate network partitions without duplicate side effects | **Not verified** |
| Whether the exact eight-machine claw-consensus fleet runs the published reference engine unchanged | **Not verified** |
| Whether private `protocol_guards` or similar components already close the public security findings | **Not verified** |
| Formal proof that claw-consensus guarantees agreement or safety under asynchronous partitions | **Not verified** |
| End-to-end cryptographic human Tier-2 enforcement in the public engine | **Not verified** |
| Public turnkey interoperation with all five listed coding-agent environments | **Not verified** |
| Cross-machine atomic task ownership in claw-consensus | **Not verified** |
| Independent confirmation of the authors' throughput or failure-recovery performance claims | **Not verified** |
| Universal production dollar/hour figures for Gas Town or other swarms | **Not verified** |
| An interoperable standard that closes all six governance gaps identified in arXiv:2606.31498 | **Not verified** |
| Broad real-world adoption of ANP, AMP, or OASF specifically for local CLI coding-agent peer governance | **Not verified** |

There are also limits to comparative licensing claims: unusual license riders, dual licenses, and server-versus-client differences need legal review before commercial adoption. The table is a technical inventory, not a legal opinion.

---

## 9. Full source list

### Vendor runtimes and agent messaging

1. [Claude Code Agent Teams — official documentation](https://code.claude.com/docs/en/agent-teams)
2. [Claude Code cross-session messaging — official documentation](https://code.claude.com/docs/en/cross-session-messaging)
3. [Codex subagents — official documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents)
4. [Cursor cloud agents — official documentation](https://cursor.com/docs/cloud-agent)
5. [Gemini CLI subagents — official documentation](https://geminicli.com/docs/core/subagents/)
6. [Gemini CLI remote agents — official documentation](https://geminicli.com/docs/core/remote-agents/)
7. [OpenCode agents — official documentation](https://opencode.ai/docs/agents/)
8. [OpenCode source repository](https://github.com/anomalyco/opencode)
9. [Pi coding-agent harness](https://github.com/earendil-works/pi)
10. [Hermes Agent](https://github.com/NousResearch/hermes-agent)
11. [OpenClaw source](https://github.com/openclaw/openclaw)
12. [OpenClaw multi-agent routing documentation](https://docs.openclaw.ai/concepts/multi-agent/index.html)

### Local orchestrators and task systems

13. [Gas Town](https://github.com/gastownhall/gastown)
14. [Beads](https://github.com/gastownhall/beads)
15. [Gas City](https://github.com/gastownhall/gascity)
16. [Wasteland](https://github.com/gastownhall/wasteland)
17. [Paperclip](https://github.com/paperclipai/paperclip)
18. [Claude Squad](https://github.com/smtg-ai/claude-squad)
19. [LoopTroop](https://github.com/looptroop-ai/LoopTroop)
20. [cmux source](https://github.com/manaflow-ai/cmux)
21. [cmux documentation](https://cmux.com/docs/getting-started)
22. [Conductor documentation](https://www.conductor.build/docs)
23. [Conductor's parallel-agent design](https://www.conductor.build/docs/concepts/parallel-agents)
24. [amux](https://github.com/mixpeek/amux)
25. [ai-maestro](https://github.com/23blocks-OS/ai-maestro)
26. [agent-deck](https://github.com/asheshgoplani/agent-deck)
27. [Awesome Agent Orchestrators](https://github.com/andyrewlee/awesome-agent-orchestrators)
28. [Awesome Agent Swarm](https://github.com/EvoMap/awesome-agent-swarm)

### Message buses, coordination protocols, and federation

29. [agentbus](https://github.com/yanglinfang/agentbus)
30. [llm-bus](https://github.com/danieldoderlein/llm-bus)
31. [agent-mesh](https://github.com/johnmwhitman/agent-mesh)
32. [Alook](https://github.com/alookai/alook)
33. [AgentWorkforce Agent Relay](https://github.com/AgentWorkforce/relay)
34. [foreleven agent-relay](https://github.com/foreleven/agent-relay)
35. [AgentScope AgentTeams](https://github.com/agentscope-ai/AgentTeams)
36. [OpenAgentMesh](https://github.com/openagentmesh/openagentmesh)
37. [MCP Agent Mail, Python](https://github.com/Dicklesworthstone/mcp_agent_mail)
38. [MCP Agent Mail, Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust)
39. [foremerge](https://github.com/naw103/foremerge)
40. [teamctl](https://github.com/Alireza29675/teamctl)
41. [hcom](https://github.com/aannoo/hcom)
42. [AgentBridge, is-bo](https://github.com/is-bo/agentbridge)
43. [AgentBridge, anYuJia](https://github.com/anYuJia/agentbridge)
44. [Concord MCP](https://github.com/Get-Concord-AI/concord-mcp)
45. [Softsensor Concord](https://github.com/Softsensor-org/concord)
46. [nats-channel-mcp](https://github.com/except-pass/nats-channel-mcp)
47. [claude_codex_bridge](https://github.com/SeemSeam/claude_codex_bridge)
48. [Agent Messaging Protocol](https://github.com/agentmessaging/protocol)
49. [agent-sync](https://github.com/ssheleg/agent-sync)
50. [system-gap-master](https://github.com/ellmos-ai/system-gap-master)
51. [lock-master](https://github.com/ellmos-ai/lock-master)
52. [Acgs-Swarm](https://github.com/acgs-ai/Acgs-Swarm)
53. [AgentSync, dipakkr](https://github.com/dipakkr/agentsync)

### Standards and governance research

54. [MCP specification](https://modelcontextprotocol.io/specification/2025-11-25)
55. [A2A specification](https://a2a-protocol.org/latest/specification/)
56. [IBM Agent Communication Protocol](https://research.ibm.com/projects/agent-communication-protocol)
57. [LF AI & Data: ACP joins A2A, August 2025](https://lfaidata.foundation/communityblog/2025/08/29/acp-joins-forces-with-a2a-under-the-linux-foundations-lf-ai-data/)
58. [Zed Agent Client Protocol](https://zed.dev/acp)
59. [Agent Network Protocol](https://agent-network-protocol.com/)
60. [Open Agentic Schema Framework](https://github.com/agntcy/oasf)
61. [ERC-8004 specification](https://eips.ethereum.org/EIPS/eip-8004)
62. [Governance Gaps in Agent Interoperability Protocols — arXiv:2606.31498](https://arxiv.org/abs/2606.31498)
63. [ACIArena — ACL 2026](https://aclanthology.org/2026.acl-long.457/)
64. [Google Research: Securing Multi-Agent Systems](https://research.google/pubs/securing-multi-agent-systems-an-empirical-analysis-of-security-prompt-hardening-and-residual-risks/)
65. [Multi-agent trust-boundary research — arXiv:2610.05640](https://arxiv.org/abs/2610.05640)

### Maintainer reports and claw-consensus audit sources

66. [Steve Yegge: Welcome to Gas Town](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04)
67. [Steve Yegge: Gas Town, From Clown Show to v1.0](https://steve-yegge.medium.com/gas-town-from-clown-show-to-v1-0-c239d9a407ec)
68. [Steve Yegge: Welcome to Gas City](https://steve-yegge.medium.com/welcome-to-gas-city-57f564bb3607)
69. [Steve Yegge: Welcome to the Wasteland](https://steve-yegge.medium.com/welcome-to-the-wasteland-a-thousand-gas-towns-a5eb9bc8dc1f)
70. [claw-consensus main repository](https://github.com/tonydzi/claw-consensus)
71. [claw-consensus governance](https://github.com/tonydzi/claw-consensus/blob/main/docs/GOVERNANCE.md)
72. [claw-consensus bus design](https://github.com/tonydzi/claw-consensus/blob/main/docs/BUS.md)
73. [claw-consensus Python reference engine](https://github.com/tonydzi/claw-consensus/blob/main/reference/consensus.py)
74. [claw-consensus signing helper](https://github.com/tonydzi/claw-consensus/blob/main/reference/fleet_sign.py)
75. [claw-consensus evaluations](https://github.com/tonydzi/claw-consensus/blob/main/docs/EVALS.md)
76. [claw-consensus failure reports](https://github.com/tonydzi/claw-consensus/blob/main/docs/FAILURE-MODES.md)
77. [join-the-fleet](https://github.com/tonydzi/join-the-fleet)

---

## Final assessment

The competitive threat to claw-consensus does not primarily come from another GitHub project reproducing exactly the same architecture. It comes from **individual vendors and orchestration products absorbing enough of the everyday coordination problem that independent labs no longer feel they need a protocol of their own**.

For this particular eight-machine lab, there remains a strong rationale for vendor-independent governance: machines must remain autonomous, models come from several companies, and the human approval policy must survive changes of CLI provider.

But the protocol's distinguishing claims need to be expressed as executable guarantees, not only operating rules.

I would therefore **retain the existing Syncthing/JSONL architecture, add Git-CAS-backed cross-machine claims for exclusive work, make signing and approvals mandatory at the enforcement boundary, and ship a minimal MCP interface plus a public chaos-test suite**.

That combination would address the most material correctness and adoption gaps without turning claw-consensus into another heavyweight orchestrator.
