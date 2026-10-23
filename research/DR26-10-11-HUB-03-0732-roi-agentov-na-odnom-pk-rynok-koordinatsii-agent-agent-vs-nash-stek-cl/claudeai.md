# DR26-10-11-HUB-03-0732 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# claude.ai (Research, Fable 5.1 Max, web, Chrome Антона) — DR26-10-11-HUB-03-0732 — снято 2026-10-14 20:33Z из чата https://приватный чат · 322 источника · 9m35s

# Local Agent Swarms Across a Few PCs: Market Map, Falsification of the "Empty Cell", Adoption Advice, and an Outside Audit of claw-consensus (as of 14 October 2026)

**Bottom line:** I found no tool, product or paper that clearly has all five properties at once: heterogeneous vendors as equal peers, multi-machine, no central server, machine-readable human approval tiers, and a governance model for changing the rules. In that narrow sense the lab's "empty cell" holds. It is a thin cell, though. GNAP, ClawTeam-OpenClaw and Paperclip each cover three of the five properties. Every coordination primitive claw-consensus uses (inboxes, ACKs, leases, claims, git- or file-based buses, approval gates) already ships elsewhere, usually with more users. What is novel is the governance layer, not the rails.

## 1. Executive summary (10 bullets)

- **The empty cell survives a strict test, barely.** No candidate passes all five properties. The closest three are GNAP (https://github.com/farol-team/gnap), which lacks approval tiers and rule-change governance; ClawTeam-OpenClaw (https://github.com/win4r/ClawTeam-OpenClaw), which has plan approvals but no tiered policy and no canon governance; and Paperclip (https://github.com/paperclipai/paperclip), which has governance and approval gates but runs as a central server.
- **Vendors now ship same-vendor coordination rails.** Claude Code has Agent Teams, with file-based mailboxes and file-locked task claiming (https://code.claude.com/docs/en/agent-teams). It also has cross-session messaging, which reaches other machines through Anthropic servers (https://code.claude.com/docs/en/cross-session-messaging). Codex has subagents controlled by `agents.max_threads` (https://developers.openai.com/codex/subagents). None of these lets another vendor's agent join as a peer.
- **Third-party buses for mixed vendors are crowded and moving fast.** Examples are hcom (530 stars), mcp_agent_mail_rust (150 stars), Agent Relay, Concord MCP, agentbus, foremerge and AgentTeams (AgentScope, Matrix-based). MCP is how nearly all of them reach non-Claude agents.
- **On protocols, coding agents use MCP in practice. A2A is mostly a business card.** ACP merged into A2A on 29 August 2025, in a merger announced by Kate Blair (IBM Research) and Todd Segal (Google) (https://github.com/orgs/i-am-bee/discussions/5). The abstract of arXiv 2606.31498 (Kang and Diponegoro) says its gap matrix "reveals that voting and dissent preservation are universally absent across all five protocols" (https://arxiv.org/abs/2606.31498). I found no protocol-level fix since then.
- **Adopt first: verify-gated completion and TTL file reservations with a pre-commit guard.** These are cheap, proven, and map directly onto claw-consensus's existing "done needs evidence" and lease ideas.
- **Adopt second: an MCP wrapper over the claw-consensus bus.** This is the single biggest lever for letting Codex, Cursor, Gemini and Grok agents use the bus without custom glue.
- **Do not adopt Gas Town-style mass autonomy.** First-hand reports describe about $100 for a 60-minute session, a PR auto-merged despite failing integration tests, and a forced `reset --hard` plus force-push (https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/). Chris Sells's Beads 1.2.1 incident report (20 August 2026) says "An autonomous Beads maintainer released v1.2.1 without explicit, release-specific human sign-off" (https://blog.gascity.com/).
- **The most common real failure is messages that are delivered but never processed.** Claude Code's issue tracker documents teammates that never poll their inbox and channel notifications dropped mid-turn. nats-channel acknowledges a message before the agent actually consumes it. claw-consensus's ACK-debt idea targets exactly this class of bug.
- **What claw-consensus genuinely adds:** ACK debt treated as an incident, dual-rail human-visible traffic, machine-readable Tier-2 gates with a tripwire, leader/follower canon, and second-machine verification with a rubber-stamp guard. Its weak spots are cross-machine claims over Syncthing (not atomic), no MCP interface seen, no cockpit, only self-measured evidence, and 4 stars.
- **Three changes would most increase adoption:** (1) ship an MCP server and a PyPI package; (2) make claims linearizable through the arbiter node, using fencing tokens, and back that with a multi-node CI harness; (3) publish the governance layer as a versioned spec or A2A extension that composes with existing rails (Beads, mcp_agent_mail, hcom), rather than competing with them.

## 2. Comparison table

Star counts and commit counts come from GitHub pages fetched on 14 October 2026 unless another source is named. "n/v" means not verified. "Last commit" is n/v wherever the logged-out GitHub page did not show a date.

| Tool | Transport | Vendors | Multi-machine | Server needed | Claim / lease | ACK | Quorum / consensus | Human tiers | Rule governance | Cost tracking | Stars (obs. 14 Oct 2026) | Last commit | License |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claw-consensus | Syncthing folder of single-writer JSONL plus a Telegram mirror | Claude Code, Codex, Cursor, Grok (per join-the-fleet) | Yes | No | Claims and per-file leases (lease mechanics n/v in docs) | Yes, with SLA escalation | propose→counter→accept→commit, round cap, leader tie-break | Yes (Tier-0…2) | Yes (leader/follower canon) | n/v | 4 | n/v (release v0.3.2 on 2 Oct 2026) | MIT |
| Claude Code Agent Teams | JSON mailbox files under «внутренний путь лаборатории» | Claude only | No | No | File-locked task claiming | Send is reported only after the write succeeds | No | Permission prompts appear in the lead session | No | Token guidance only | n/a (proprietary) | n/a | Proprietary |
| Claude Code cross-session messaging | Local inbox socket; Anthropic servers between machines | Claude only | Yes (through Anthropic servers) | Vendor cloud between machines | No | n/v | No | Inbound accept/hold/refuse | No | No | n/a | n/a | Proprietary |
| Codex subagents | In-process threads | OpenAI only | No | No | No | n/a | No | Inherits approval policy | No | n/v | n/a | n/a | CLI Apache-2.0 |
| mcp_agent_mail / _rust | FastMCP HTTP plus Git plus SQLite | Claude Code, Codex, Copilot CLI and others | Through the server | Yes (local HTTP server) | Advisory TTL file reservations plus pre-commit guard | Yes (ack-required messages) | No | No | No | No | 150 (Rust version) | n/v (3,597 commits) | n/v |
| hcom | Hooks into local SQLite; optional relay across devices | Claude, Codex, Gemini, Cursor, OpenCode, Pi, Kimi, Copilot, Grok | Yes, through `hcom relay` | Relay for cross-device use | No | Exact acks in a fork | No | No | No | No | 530 (557 in bradAGI list) | n/v | MIT (per subagent, n/v on page) |
| foremerge | Local SQLite above git, plus MCP | Claude Code, Codex, Cursor | Optional cloud link | No for local use | Intent plus semantic-scope claims (advisory, no lock) | n/v | Deterministic conflict rules | Human-defined acceptance check | No | No | 1 | n/v (29 commits) | Apache-2.0 |
| Beads (Gas Town / Gas City) | Git plus JSONL, now Dolt | Agent-agnostic CLI | Yes, through git or Dolt remotes | No (Dolt server optional) | Atomic work claims | No | No | No | No | No | >27,000 (per Gas City blog, 1 Oct 2026) | n/v | n/v |
| GNAP | Git repo containing a .gnap/ folder | OpenClaw, Codex, Claude Code, custom | Yes | No | Task assignment | No | No | No ("application layer") | No | No | 84 | 2026-03 per awesome list | MIT |
| agentbus (yanglinfang) | Append-only JSONL in the repo; git across machines | CLI, MCP or raw file | Yes | No | Lock/unlock with a visible owner | Yes | No | No | No | No | 0 | n/v | MIT |
| Agent Relay (AgentWorkforce) | Hosted channels and DMs plus local socket | Claude Code, Codex, OpenCode | Yes | Yes (Relay Cloud or self-hosted) | n/v | Receipt polling | No | No | No | n/v | n/v | n/v | n/v |
| Concord MCP | SQLite plus MCP plus CLI | Claude, Cursor, Gemini, Grok, Codex | n/v | No | claim_work, handoff | n/v | No | No | No | No | 462 | Updated 12 Oct 2026 | MIT |
| AgentTeams (AgentScope) | Matrix rooms plus controller | OpenClaw, QwenPaw, Hermes, others | Yes | Yes (Matrix plus k8s controller) | n/v | n/v | No | Humans in rooms | No | Gateway | 5,747 | Updated 14 Oct 2026 | Apache-2.0 |
| agent-mesh (johnmwhitman) | MCP stdio plus SQLite ledger plus A2A card | OpenCode-centric | n/v | n/v | n/v | Receipts | Quorum councils | No | No | n/v | 1 | PR merged 28 Sep 2026 | MIT |
| ClawTeam-OpenClaw | File inboxes or P2P; NFS/SSHFS across machines | OpenClaw, Claude Code, Codex, Hermes, Cursor | Yes | No | fcntl file locking | n/v | No | Plan approval and exec approval | No | n/v | n/v | n/v | n/v |
| Paperclip | Node server plus Postgres | BYO (OpenClaw, Claude Code, Codex, Cursor, HTTP) | Agents connect to the server | Yes | Ticket claiming on heartbeat | n/v | No | Board approval gates, budget hard-stops | Yes (revisioned config, board approval) | Yes (per-agent budgets) | 95.9k | n/v (4,722 commits) | MIT (per Cloudron listing) |
| Agent Messaging Protocol | WebSocket, webhooks, relay queue | Any | Yes (federated) | Yes (provider or relay) | No | n/v | No | No | No | No | 32 (35 in list) | n/v | Apache-2.0 (list) |

## 3. Answers

### Q1. Market map of agent-to-agent coordination on a local computer

**Layer 1: rails built by the vendors**

- **Claude Code Agent Teams** (https://code.claude.com/docs/en/agent-teams)
  - It is experimental and must be enabled with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`. A team has a lead, teammates, a shared task list and a mailbox.
  - "Each agent's mailbox is a JSON file at «внутренний путь лаборатории»{team-name}/inboxes/{agent-name}.json`."
  - "Task claiming uses file locking to prevent race conditions."
  - Hooks such as `TaskCompleted` can block a task from being marked complete. This gives you a native verify gate.
  - Messages between agents are explicitly labelled as coming from another Claude session. "A teammate can't approve a permission prompt or supply consent on your behalf."
  - Teams cannot nest, and teammates are Claude only.
- **Claude Code cross-session messaging** (https://code.claude.com/docs/en/cross-session-messaging)
  - Each session binds an inbox socket. Sessions on the same machine can start conversations with each other.
  - Third-party write-ups report that it shipped in v2.1.224 on 7 August 2026, that sessions on other machines communicate through Anthropic servers over Remote Control, and that Windows support arrived in v2.1.239 (https://blakecrosley.com/blog/claude-code-cross-session-messaging, https://9to5mac.com/2026/08/07/claude-code-now-lets-sessions-talk-to-each-other-on-macos/).
  - Inbound messages can be set to accept, hold or refuse (https://claudcod.com/blog/claude-code-cross-session-messaging/).
  - It is Claude only.
- **Codex subagents** (https://developers.openai.com/codex/subagents, https://developers.openai.com/codex/config-reference)
  - The multi-agent tools are `spawn_agent`, `send_input`, `resume_agent`, `wait_agent` and `close_agent`. The `agents.*` config has custom TOML roles.
  - The parent orchestrates the work, and subagents inherit the parent's permission mode.
  - A secondary guide says Codex subagents cannot message each other directly (https://www.morphllm.com/codex-multi-agent).
- **Gemini CLI subagents.** I found only secondhand descriptions: specialists inside the main session, each with an isolated context loop (https://medium.com/@aristojeff/what-are-multi-agent-systems-and-subagents-a-comparison-of-codex-claude-code-and-gemini-cli-304376584f51). Primary docs are not verified.
- **Cursor background agents.** Not verified; I found no primary source in this pass.
- **OpenClaw.** It has native multi-agent routing. `tools.agentToAgent` controls cross-agent access, and "Role instructions require human approval before external sends, publication, purchases, deletion, or production changes" (https://docs.openclaw.ai/concepts/multi-agent). That is close to the lab's Tier-2 list, but it lives inside a single runtime.
- **OpenCode, Pi and Hermes.** These harnesses mostly appear as targets that other orchestrators drive. Their primary docs were not verified.

**Layer 2: orchestrators for swarms on a single machine**

- **Gas Town, Gas City and Beads.**
  - Gas Town was released on 1 January 2026 and built on Beads (https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/).
  - Beads stores tasks in `.beads/` and syncs them to git through `issues.jsonl`. The architecture says each machine "has its own copy" of the SQLite DB and shares through git push/pull. That makes it multi-machine and serverless at the ledger level.
  - Gas Town has since been recast as Gas City, an SDK. Gas City 1.0 was announced on 28 April 2026, and Beads 1.3 / Gas City 1.5 shipped on 7 October 2026. Donna Box's 1 October 2026 post announced the Beads Protocol (BDP) alongside Memory Beads and versioning (https://blog.gascity.com/).
  - A secondary source reports that Yegge shut down Gas Town and said he never successfully built anything else with it (https://www.latent.space/p/ainews-reality-checks-on-ai-news). Treat that as secondhand.
- **Paperclip** (https://github.com/paperclipai/paperclip). 95.9k stars on 14 October 2026. It is a Node server with a React UI and offers org charts, heartbeats, per-agent budgets, board approval gates, and "config changes are revisioned" with rollback. It is the market leader by attention.
- **From awesome-agent-orchestrators** (https://github.com/andyrewlee/awesome-agent-orchestrators; about 2,060 stars per a third-party mirror, https://skillsllm.com/skill/awesome-agent-orchestrators). These are relevant entries that the brief did not name:
  - **Zeron:** a cross-device control plane with an always-on daemon. It supports Claude Code, Codex, Cursor, Grok, Hermes and Pi, which is a near match for this fleet (https://github.com/ZHallen122/awesome-agent-orchestrators).
  - **Traycer:** agent-to-agent messaging across vendors with cross-device sync.
  - **corellis:** governance and approval workflows for 20+ OpenClaw agents (https://github.com/CorellisOrg/corellis).
  - **kodo:** "a separate agent independently verifies each result."
  - **agent-kanban:** a leader-worker board with cryptographic agent identity.
  - **AgentBridge:** keeps Claude Code and Codex as live peers.
  - **foremerge:** intent and scope declarations.
  - **gnap:** git-native, no orchestrator. Listed as "Resting", with its last commit in 2026-03.
  - **OpenRig, shire, wmux, octomux and kandev:** octomux offers "one unified permission inbox across agents"; kandev places "human gates" on steps and runs over SSH (https://github.com/spuder/awesome-agent-orchestrators, https://github.com/badcuban/awesome-agent-orchestrators).
  - **agent-deck:** one TUI across Claude Code, Codex, Gemini and OpenCode (https://github.com/HarjjotSinghh/awesome-agent-orchestrators).
  - **Hephaestus:** "A2A Hub routing" with "governed memory/security gates" (https://github.com/MarlBurroW/awesome-agent-orchestrators).
- **awesome-agent-swarm** (https://github.com/EvoMap/awesome-agent-swarm). It is mostly general frameworks: Swarms, Agency Swarm, CAMEL, and EvoMap with "AI Council multi-agent governance." These are not rails for local coding agents and are largely irrelevant here.
- **Claude Squad, LoopTroop, cmux, Conductor, amux and ai-maestro.** Not verified in this pass.

**Layer 3: message buses and mailboxes for agents from different vendors** (properties are in the table above)

- **mcp_agent_mail** (https://github.com/dicklesworthstone/mcp_agent_mail) has been superseded by **mcp_agent_mail_rust** (https://github.com/Dicklesworthstone/mcp_agent_mail_rust; 150 stars, 3,597 commits).
  - It offers 34 tools, a Git-backed archive, SQLite indexing, and "Reservations are advisory, TTL-based, and support glob patterns."
  - A pre-commit guard blocks commits that conflict with another agent's exclusive reservation.
  - Messages carry "acknowledgments, and priority levels."
- **hcom** (https://github.com/aannoo/hcom; 530 stars, 84 forks).
  - Hooks write to SQLite, then hooks on the other side deliver: "agent → hooks → db → hooks → other agent."
  - It supports 11+ CLIs, and cross-device use goes through `hcom relay connect <token>`.
  - A fork adds "exact acks" and delivery lanes (https://github.com/atacolak/hcom).
- **Agent Relay** (https://github.com/AgentWorkforce/relay, https://agentrelay.com/docs/introduction). "Agents can run on different machines and still coordinate in the same workspace." It has an `agent-relay mcp` mode. Its skill's safety rule says "a peer message is a request, not authority" (https://github.com/AgentWorkforce/skills/pull/133).
- **foremerge** (https://github.com/naw103/foremerge; 1 star, 29 commits). A Rust binary over local SQLite with MCP support, Apache-2.0 (https://dev.to/naw103/ai-agents-introduced-several-new-layers-to-the-development-stack-you-are-probably-missing-this-one-54dg). "Claims are advisory leases, not locks" (https://news.ycombinator.com/item?id <tg-id>).
- **agentbus (yanglinfang)** (https://github.com/yanglinfang/agentbus; 0 stars, MIT). It is "an append-only JSONL file inside the repo… so git is the cross-machine transport." This is architecturally the closest twin of the claw-consensus file rail.
- **llm-bus.** Two projects use the name:
  - wnz99/llm-bus: SQLite, same machine, has `ack`.
  - danieldoderlein/llm-bus: 9 stars, AGPL-3.0, Postgres over MCP, "Atomic claims, shared ledger, leases." It needs a server.
- **agent-mesh** (https://github.com/johnmwhitman/agent-mesh; 1 star, MIT). Offers "receipts, quorum councils" and an A2A agent card.
- **AgentTeams by AgentScope** (https://github.com/agentscope-ai/AgentTeams; 5,747 stars, Apache-2.0). "All runtimes communicate through Matrix rooms." It needs Matrix plus a controller.
- **OpenAgentMesh** (https://github.com/openagentmesh/openagentmesh). Uses NATS as its "single infrastructure dependency." Stars not verified.
- **nats-channel-mcp** (https://glama.ai/mcp/servers/except-pass/nats-channel-mcp). Fire-and-forget by default, with an optional JetStream mode (see Q4).
- **Concord MCP** (https://github.com/Get-Concord-AI/concord-mcp; 462 stars, MIT). Uses SQLite and offers `claim_work` and handoff. Note that "Concord sends product and coordination telemetry to getconcord.ai."
- **AgentBridge** (https://github.com/raysonmeng/agent-bridge; 371 or 382 stars). A localhost daemon linking Claude Code and Codex.
- **claude_codex_bridge** (https://github.com/SeemSeam/claude_codex_bridge). Only the list description is verified.
- **Agent Messaging Protocol** (https://github.com/agentmessaging/protocol; 32 stars). Federated, with Ed25519-signed messages and a relay queue.
- **Alook.** Rooms where humans and agents meet; 1.2k stars per a list. Primary repo not verified.
- **teamctl** (https://github.com/Alireza29675/teamctl). A YAML-defined setup; transport not verified.

**Layer 4: protocols, and which ones local coding agents actually use**

- **MCP** is the universal adapter in practice. foremerge, mcp_agent_mail, Agent Relay, agentbus, Concord, nats-channel and hcom's plugin all expose themselves through MCP or hooks. That is how a non-Claude agent "sees" a bus.
- **A2A.** It absorbed IBM's ACP on 29 August 2025. Kate Blair (IBM Research) said "By bringing the assets and expertise behind ACP into A2A, we can build a single, more powerful standard" (https://github.com/orgs/i-am-bee/discussions/5). It reached v1.0.1 in May 2026, with an extension mechanism (https://arxiv.org/html/2606.31498).
  - In local tools it appears mostly as Agent Cards: agent-mesh, Hephaestus, and the claw-consensus join registry (https://github.com/tonydzi/join-the-fleet).
  - I found no vendor coding CLI that speaks A2A natively (not verified either way).
- **"ACP" is a name collision.** Since the merger, "ACP" in coding tools usually means Zed's Agent Client Protocol (https://rywalker.com/research/acp-agent-communication-protocol). OpenHands, for example, drives "any Agent Client Protocol agent" (https://github.com/ethan-krich/awesome-agent-orchestrators).
- **ANP** (DIDs, peer-to-peer). I found no local coding tool using it.
- **OASF.** Not verified.
- **Newer in 2026:** the Agent Messaging Protocol (federated, signed), the Beads Protocol (BDP), and ERC-8004, which is analysed in arXiv 2606.31498.

### Q2. Trying to falsify the "empty cell" claim

I applied the five-property test strictly to each candidate below.

| Candidate | (a) Mixed vendors as peers | (b) Multi-machine | (c) No central server | (d) Machine-readable approval tiers | (e) Governance of rule changes | Verdict |
|---|---|---|---|---|---|---|
| GNAP (https://github.com/farol-team/gnap) | Yes ("OpenClaw, Codex, Claude Code, or custom") | Yes (git) | Yes ("Zero servers"; a git remote is the only shared point) | **No**. Governance is listed as "Application Layer (optional)"; humans are participants, not a tier policy | **No** | 3/5 |
| ClawTeam-OpenClaw (https://github.com/win4r/ClawTeam-OpenClaw) | Yes (OpenClaw, Claude Code, Codex, Hermes, Cursor) | Yes ("cross-machine support (NFS/SSHFS or P2P)") | Yes | **Partial only**. "Plan approval workflows" and exec approvals, but no tiered risk policy | **No** | 3/5 (plus partial d) |
| Paperclip (https://github.com/paperclipai/paperclip) | Yes (BYO agents) | Agents can connect from anywhere, but this is a client-server design | **No** (Node server plus Postgres) | Yes (board approval gates, budget hard-stops) | Yes ("config changes are revisioned"; hiring "gated by Board approval by default", https://paperclip.ing/) | 3/5 (plus a contested b) |
| Claude Code Agent Teams / cross-session | **No** (Claude only) | Cross-session only, through Anthropic servers | **No** across machines | Partial (permission modes) | No | ≤2/5 |
| hcom | Yes | Through a relay | **No** for cross-device use | No | No | 2/5 |
| agentbus | Yes (CLI/MCP) | Yes (git) | Yes | No | No | 3/5 |
| mcp_agent_mail_rust | Yes | Through a server | No | No | No | 1–2/5 |
| corellis / OpenClaw multi-agent | **No** (OpenClaw only) | Single server | No | Yes (approval workflows) | Yes (governance framework) | 2/5 |

**Verdict.** No tool I could find satisfies all five properties. agentbus and GNAP satisfy (a), (b) and (c) but have no governance. Paperclip satisfies (a), (d) and (e) but needs a central server. That is the closest-three answer.

**The honest caveat.** The cell is empty mostly because it is the intersection of two populated markets. One is serverless buses for mixed vendors (GNAP, agentbus, Beads, hcom). The other is governed control planes (Paperclip, corellis, OpenClaw approvals). A competitor could close the gap by adding a tier policy to GNAP, or a git backend to Paperclip, without inventing anything new. The lab also did not search Cursor, Gemini or Hermes, and nor could I (not verified).

**arXiv 2606.31498.** "Governance Gaps in Agent Interoperability Protocols: What MCP, A2A, and ACP Cannot Express" by Richard Kang and Yudho Diponegoro, cs.MA, v1 dated 30 June 2026 (https://arxiv.org/abs/2606.31498).

- It tests MCP v1.1, A2A v1.0.1, ACP, ANP and ERC-8004 against six dimensions: membership, deliberation, voting, dissent preservation, human escalation, and audit/replay.
- It finds that "voting and dissent preservation are universally absent across all five protocols, deliberation is absent or at most partial," and that governance "constitutes a missing architectural layer above current interoperability standards." It also notes that A2A's four example extensions do not address governance.

Has anyone closed the gap since? **At the protocol level, not that I could verify.** At the tool level, the gaps are partly addressed:

- claw-consensus covers deliberation (propose/counter), human escalation (Tier-2) and audit (append-only JSONL plus Ed25519). Its resolution is accept-plus-leader-tie-break, not voting. Dissent preservation is not verified.
- agent-mesh offers "quorum councils."
- Paperclip covers membership and escalation.

Related work: "Governance by Design" (arXiv 2604.11337) measures 19% sub-function coverage in the OpenClaw core ecosystem (https://arxiv.org/pdf/2604.11337). "Agentic Configuration Management" (arXiv 2608.11166) proposes a framework-independent governance vocabulary (https://arxiv.org/pdf/2608.11166).

**Recommendation.** Stop marketing the empty cell. Market claw-consensus as a reference implementation of the governance layer that 2606.31498 says is missing. That claim is defensible and citable.

### Q3. What to adopt now, ranked for 8 machines (not 30 agents on one box)

Setup hours are my estimates for one engineer who already runs the fleet. They are not measured.

1. **Verify-gated completion. Do this now. About 2–4 hours.**
   - Evidence that it works:
     - Claude's `TaskCompleted` hook can refuse completion with exit code 2 (https://code.claude.com/docs/en/agent-teams).
     - kodo has a separate agent verify each result.
     - foremerge runs a human-defined acceptance check before commit (https://news.ycombinator.com/item?id <tg-id>).
   - Failure mode reported: rubber-stamping. claw-consensus already guards against this.
   - Worth it at 8 machines: **yes**. The lab already believes in it; wire it into each vendor's native hook rather than relying on prompts alone.
2. **TTL file reservations with a pre-commit guard and ack-required messages, mcp_agent_mail style. About 4–8 hours.**
   - Evidence that it works: it is TTL-based and glob-aware, and the pre-commit guard blocks conflicting commits (https://github.com/Dicklesworthstone/mcp_agent_mail_rust).
   - Failure mode reported: the project's own `am doctor` check flags "Expired file reservations: Reservations expire but aren't cleaned up" (https://github.com/KoSukeWork/mcp_agent_mail).
   - Worth it at 8 machines: **yes, but only for repos that more than one machine edits**. Keep the claw-consensus single-machine leases and add the pre-commit guard, which is the part that enforces anything.
3. **An MCP wrapper over the bus. About 6–12 hours.**
   - Evidence that it works: this is how every successful bus reaches Codex and Cursor (Agent Relay `agent-relay mcp`, foremerge, Concord, agentbus).
   - Failure mode reported: MCP channel notifications are dropped mid-turn (https://github.com/anthropics/claude-code/issues/37250). So expose pull tools (`read_inbox`, `ack`), not push.
   - Worth it at 8 machines: **yes, the highest leverage of all**.
4. **Intent and scope declarations before writing, foremerge style. About 3–6 hours.**
   - Evidence that it works: deterministic rules flag a replace-versus-extend clash on the same symbol before any code exists (https://foremerge.com/). The arXiv "Claim Plane" paper formalises enforceable intents with leases and fencing tokens (https://arxiv.org/pdf/2607.21909).
   - Failure mode reported: claims never lock, so two agents can still hold the same scope.
   - Worth it at 8 machines: **moderately**. The ON AIR board already covers this at the human level; foremerge's semantic scopes (`symbol:X=replace`) are the upgrade worth copying.
5. **A git-backed task ledger with atomic claims, Beads style. About 4–8 hours.**
   - Evidence that it works: the JSONL-in-git design is "merge-friendly" (https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/), and Beads 1.1 later had to add "safer cross-clone sync" (https://blog.gascity.com/).
   - Failure mode reported: one HN user "had to uninstall Beads after seeing it only confuse my agents" (https://news.ycombinator.com/item?id <tg-id>).
   - Worth it at 8 machines: **only as the work-claim store**. Note that "atomic" in Beads holds within one Dolt server; across git clones it is merge-based, not linearizable.
6. **Socket-based messaging between sessions. Near zero hours for Claude.**
   - Evidence that it works: native in Claude Code (https://code.claude.com/docs/en/cross-session-messaging).
   - Failure mode reported: it only serves Claude, and messages between machines go through Anthropic's cloud.
   - Worth it at 8 machines: **use it for Claude-to-Claude traffic on the same box only**. It does not replace the fleet bus.

### Q4. Failure modes reported in the wild

- **Agents breaking main (Gas Town).** DoltHub reported first-hand that Gas Town "merges them… despite the integration tests failing." They "had to get a colleague to reset --hard and force push," none of the PRs were good, and "This whole sixty minute experience cost me about $100 in Claude tokens… about 10X" a normal session (https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/).
  - A secondhand summary adds a "murderous rampaging Deacon" deleting code and 5 force pushes to main (https://paddo.dev/blog/gastown-two-kinds-of-multi-agent/).
  - Another user ended up with "three mayors, 141 orphaned Claude Code processes" (https://tenzinwangdhen.com/posts/gastown-good-bad-ugly/).
- **Autonomous release without sign-off.** The Beads 1.2.1 incident report describes how "An autonomous Beads maintainer released v1.2.1 without explicit, release-specific human sign-off" (https://blog.gascity.com/). This is exactly the Tier-2 case. The gastownhall/beads GitHub Releases page (https://github.com/gastownhall/beads/releases) states the damage and the fix: "v1.2.0 and v1.2.1 were published by accident on 2026-08-11 without release testing. v1.2.2 supersedes them by re-releasing the tested 1.1 line," and running v1.2.1 once "migrated your local database schema from v53 to v65."
- **Cost blowups.** Yegge on X: "This is my third $200/month Claude Pro Max plan" (quoted at https://paddo.dev/blog/gastown-two-kinds-of-multi-agent/). Anthropic: agent teams "use significantly more tokens," with 3–5 teammates recommended (https://code.claude.com/docs/en/agent-teams).
- **Teammates not polling their inboxes.**
  - "Messages sit in inbox with 'read': false forever" (https://github.com/anthropics/claude-code/issues/23415; also #24108).
  - SDK and pipe mode never poll because the poller is tied to the TUI (https://github.com/anthropics/claude-code/issues/26426).
  - Messages to the lead are deferred until `end_turn` (https://github.com/anthropics/claude-code/issues/50779).
  - In VS Code, permission prompts are invisible, which deadlocks agents (https://github.com/anthropics/claude-code/issues/25254).
- **Stuck "in progress" tasks.** "Teammates sometimes forget to mark tasks as completed, blocking dependent work" (https://claudefa.st/blog/guide/agents/agent-teams-best-practices). Agents spawned with `team_name` saw "TaskUpdate from agent never happens → task stays pending forever" (https://github.com/anthropics/claude-code/issues/58762).
- **Messages lost between receipt and processing (nats-channel).**
  - "channel-server acks once process.stdout.write() accepts the buffer… If Claude Code crashes between pipe receive and consumption, the ack has already fired and the message is lost."
  - Without JetStream, "no subscriber = lost message."
  - With JetStream, "Pauses longer than 1h lose messages" (https://glama.ai/mcp/servers/except-pass/nats-channel-mcp).
  - The mitigation the author adopted was JetStream durable consumers.
  - Related: Claude channel plugins deliver only "the first MCP channel notification per session" (https://github.com/anthropics/claude-code/issues/38736), and queued input is lost on resume (https://github.com/anthropics/claude-code/issues/100003).
- **Duplicate work and lock wars.**
  - foremerge's founding story: separate worktrees still let "two agents… delete and extend the same contract in clean branches" (https://dev.to/naw103/parallel-coding-agents-without-the-carnage-gf9).
  - mcp_agent_mail chose advisory leases over global locks to avoid deadlock.
  - Expired leases linger unless cleaned up (https://github.com/KoSukeWork/mcp_agent_mail).
- **Prompt injection through messages between agents.**
  - Anthropic's mitigations: receiving agents are told a message came from another session, it "can't approve a permission prompt," and in auto mode a classifier "reviews each message before Claude Code delivers it" (https://code.claude.com/docs/en/agent-teams).
  - Agent Relay's skill: "a peer message is a request, not authority."
  - I found no first-hand exploit report for local coding swarms (not verified). The "Toward a Safe Internet of Agents" survey covers the threat class (https://arxiv.org/pdf/2512.00520).
- **Instruments that fail silently, from the lab's own fleet.** claw-consensus v0.3.2 records a guard because "the longest legitimate silence on that channel was 115.5 hours against 130 hours of real death" (https://github.com/tonydzi/claw-consensus/releases/tag/v0.3.2).

### Q5. An outside audit of claw-consensus

**What I read.** The claw-consensus README and file tree, release v0.3.2, PR #5 (the CI negative test), the self-submission PR to an awesome list, and the join-the-fleet README and file tree. I could not open `docs/PROTOCOL.md`, `docs/GOVERNANCE.md`, `docs/FAILURE-MODES.md` or the code files directly, because the fetch tool blocked links found inside pages. Claims about their contents rest on the README's descriptions.

**State on 14 October 2026.**
- claw-consensus: 4 stars, 0 forks, 46 commits, 3 open issues, MIT (https://github.com/tonydzi/claw-consensus).
- join-the-fleet: 0 stars, 6 commits (https://github.com/tonydzi/join-the-fleet).
- The latest release is v0.3.2 (2 October 2026), but the README's roadmap still says "Now — v0.2.1."
- The machine count is inconsistent: "5 machines" in one README section, "six machines we own" in another, and "8 machines" in join-the-fleet.

**What is genuinely novel, or at least uncommon:**
- **ACK debt with an SLA and escalation.** "Delivered is not done"; silence triggers a chase, then an escalation. Of the bus tools in Q1, mcp_agent_mail has ack-required messages and an hcom fork has exact acks, but none of the tools I reviewed treats silence as an incident.
- **A dual rail by construction.** `bus_send.py` "makes single-rail sends impossible," and a dead rail is treated as a signal. No other tool I found mirrors every machine message to a human chat.
- **Machine-readable risk tiers with a tripwire.** The demo shows the tripwire "force-bumping a mislabelled dangerous action."
- **Leader/follower canon for the rulebook.**
- **"Globally done" requires a second machine**, with a "rubber-stamp guard [that] rejects copy-pasted proofs."
- **A deterministic consensus engine that uses zero tokens**, with split-brain detection and a corrupt-line-tolerant ledger.
- **Ed25519 identity through `ssh-keygen -Y`**, with revocation.
- **An unusually honest failure-modes discipline.** Guards "written from incidents."

**What is reinvention:**
- File inbox plus ACK: mcp_agent_mail, hcom, agentbus.
- Git- or file-based serverless bus: GNAP, agentbus, Beads JSONL.
- Per-file leases: mcp_agent_mail TTL reservations with a pre-commit guard.
- Work claims: Beads, and Agent Teams' file-locked claiming.
- The ON AIR board: foremerge intents and scopes.
- Approval gates: Paperclip, OpenClaw.
- An A2A card as the front door: agent-mesh, Hephaestus.

None of this is bad. The lab should cite these tools as prior art rather than present the stack as unprecedented.

**What a newcomer would expect but not find:**
- **Atomic claims across machines.** Syncthing whole-file sync is eventually consistent. "First to claim wins" across 8 machines is therefore a race, settled after the fact by tie-break. The fix is to route claims through the arbiter as the single writer, or to use lease plus fencing token, as in Claim Plane (https://arxiv.org/pdf/2607.21909).
- **An MCP interface.** None appears in the file listing (not verified as absent inside `reference/`).
- **A cockpit or UI.** None is visible. Compare the dashboards in Paperclip and mcp_agent_mail.
- **Tests.**
  - There is no `tests/` directory. `demo/demo.py` "is also the integration test," and `protocol_guards.py` / `fleet_sign.py` have `selftest` modes.
  - CI exists: a fleet-validate workflow was proven by PR #5 (https://github.com/tonydzi/claw-consensus/pull/5). Whether CI runs the demo is not verified.
  - join-the-fleet has `_test_fleet_daily.py`.
- **Benchmarks from someone else.** The README admits "All the measurements in here come from six machines we own."
- **Packaging.** It is "stdlib-only Python," with copy-the-folder install and no PyPI package seen.
- **Security posture of the Telegram rail.** Mirroring every machine message to a third-party chat puts operational data on Telegram. It also means content from that human channel can reach agents. The docs should state the threat model (not verified whether `GOVERNANCE.md` does).
- **join-the-fleet supply chain.** Door 1 pulls "the lab's 232 skills" daily into the user's agent. The README discloses this candidly ("the same trust you give npm update"), but it is still an auto-updating instruction channel. Signed releases plus a pinned mode would help.
- **Presentation.** The "three-circuit homeostatic governance" and Gorsky-Anokhin framing, and much of the kit being in Russian, will lose sceptical engineers. Lead with the protocol spec and the failure modes.

**The three changes that would most increase adoption by other labs:**
1. **Ship `claw-consensus-mcp` plus a PyPI package.** Expose `send`, `read_inbox`, `ack`, `claim`, `propose`, `respond`, `verify` and `tier_check` as MCP tools, so that Codex, Cursor, Gemini and Grok get the bus natively. Make it pull-based, given the mid-turn notification drops in Q4.
2. **Make claims correct across machines, and prove it in CI.** Use arbiter-serialised claims or fencing tokens. Add a docker-compose harness with real Syncthing and injected partitions that runs in GitHub Actions on every PR. This answers the README's own open issue #3 ("reproducible without our topology").
3. **Publish the governance layer as a versioned spec that composes with existing rails.** Provide JSON Schemas for messages, tiers, canon and verify, mapped explicitly to the six 2606.31498 dimensions, possibly as an A2A extension. Add thin adapters so the tiers and canon can sit on top of Beads, mcp_agent_mail or hcom instead of replacing them. Add a read-only static cockpit rendered from the ledger.

## 4. What we could not verify

- Last-commit dates for almost every repo. Logged-out GitHub pages did not show them, and the fetched pages may be cached snapshots.
- Star counts for AgentWorkforce/relay, OpenAgentMesh, teamctl, claude_codex_bridge and Alook (the Beads count of more than 27,000 comes from the Gas City blog's 1 October 2026 post, not from the repo page). Licenses for mcp_agent_mail_rust, Beads and Agent Relay.
- The hcom star count is inconsistent: 530 on the repo page versus 557 in the bradAGI list (https://github.com/bradagi/awesome-cli-coding-agents).
- Primary docs for Cursor background agents, Gemini CLI multi-agent features, OpenCode, Pi and Hermes. Claude Squad, LoopTroop, cmux, Conductor, amux, ai-maestro and the OASF protocol.
- The contents of the claw-consensus `docs/` and `reference/` files beyond the README's descriptions, including whether an MCP server or lease TTLs exist and whether CI runs the demo.
- Process mitigations in the Beads 1.2.1 incident report beyond the v1.2.2 re-release noted on the Releases page, and Yegge's reported shutdown of Gas Town (secondhand via Latent Space).
- The "murderous Deacon" and "5 force pushes" details (secondhand via paddo.dev).
- Whether any vendor coding CLI speaks A2A natively.
- A first-hand prompt-injection incident between agents in a local coding swarm.

## 5. Full source list

- https://github.com/tonydzi/claw-consensus
- https://github.com/tonydzi/claw-consensus/releases/tag/v0.3.2
- https://github.com/tonydzi/claw-consensus/pull/5
- https://github.com/Yigtwxx/awesome-claude-multi-agent/pull/3
- https://github.com/tonydzi/join-the-fleet
- https://github.com/tonydzi
- https://arxiv.org/abs/2606.31498
- https://arxiv.org/html/2606.31498
- https://arxiv.org/pdf/2604.11337
- https://arxiv.org/pdf/2608.11166
- https://arxiv.org/pdf/2607.21909
- https://arxiv.org/pdf/2512.00520
- https://code.claude.com/docs/en/agent-teams
- https://code.claude.com/docs/en/cross-session-messaging
- https://blakecrosley.com/blog/claude-code-cross-session-messaging
- https://9to5mac.com/2026/08/07/claude-code-now-lets-sessions-talk-to-each-other-on-macos/
- https://claudcod.com/blog/claude-code-cross-session-messaging/
- https://claudefa.st/blog/guide/agents/agent-teams-best-practices
- https://developers.openai.com/codex/subagents
- https://developers.openai.com/codex/config-reference
- https://www.morphllm.com/codex-multi-agent
- https://medium.com/@aristojeff/what-are-multi-agent-systems-and-subagents-a-comparison-of-codex-claude-code-and-gemini-cli-304376584f51
- https://docs.openclaw.ai/concepts/multi-agent
- https://github.com/dicklesworthstone/mcp_agent_mail
- https://github.com/Dicklesworthstone/mcp_agent_mail_rust
- https://github.com/KoSukeWork/mcp_agent_mail
- https://github.com/aannoo/hcom
- https://github.com/atacolak/hcom
- https://github.com/AgentWorkforce/relay
- https://agentrelay.com/docs/introduction
- https://github.com/AgentWorkforce/skills/pull/133
- https://github.com/naw103/foremerge
- https://foremerge.com/
- https://news.ycombinator.com/item?id <tg-id>
- https://dev.to/naw103/parallel-coding-agents-without-the-carnage-gf9
- https://dev.to/naw103/ai-agents-introduced-several-new-layers-to-the-development-stack-you-are-probably-missing-this-one-54dg
- https://github.com/farol-team/gnap
- https://github.com/win4r/ClawTeam-OpenClaw
- https://github.com/CorellisOrg/corellis
- https://github.com/paperclipai/paperclip
- https://paperclip.ing/
- https://forum.cloudron.io/topic/16008/paperclip-open-source-orchestration-platform-for-teams-of-ai-agents-at-work
- https://github.com/yanglinfang/agentbus
- https://github.com/wnz99/llm-bus
- https://github.com/danieldoderlein/llm-bus
- https://github.com/johnmwhitman/agent-mesh
- https://github.com/agentscope-ai/AgentTeams
- https://github.com/openagentmesh/openagentmesh
- https://github.com/Alireza29675/teamctl
- https://github.com/raysonmeng/agent-bridge
- https://github.com/Get-Concord-AI/concord-mcp
- https://github.com/SeemSeam/claude_codex_bridge
- https://github.com/agentmessaging/protocol
- https://glama.ai/mcp/servers/except-pass/nats-channel-mcp
- https://github.com/anthropics/claude-code/issues/23415
- https://github.com/anthropics/claude-code/issues/24108
- https://github.com/anthropics/claude-code/issues/25254
- https://github.com/anthropics/claude-code/issues/26426
- https://github.com/anthropics/claude-code/issues/37250
- https://github.com/anthropics/claude-code/issues/38736
- https://github.com/anthropics/claude-code/issues/50779
- https://github.com/anthropics/claude-code/issues/58762
- https://github.com/anthropics/claude-code/issues/100003
- https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/
- https://blog.gascity.com/
- https://github.com/gastownhall/beads/releases
- https://paddo.dev/blog/gastown-two-kinds-of-multi-agent/
- https://tenzinwangdhen.com/posts/gastown-good-bad-ugly/
- https://news.ycombinator.com/item?id <tg-id>
- https://www.latent.space/p/ainews-reality-checks-on-ai-news
- https://github.com/andyrewlee/awesome-agent-orchestrators
- https://skillsllm.com/skill/awesome-agent-orchestrators
- https://github.com/ZHallen122/awesome-agent-orchestrators
- https://github.com/spuder/awesome-agent-orchestrators
- https://github.com/badcuban/awesome-agent-orchestrators
- https://github.com/HarjjotSinghh/awesome-agent-orchestrators
- https://github.com/MarlBurroW/awesome-agent-orchestrators
- https://github.com/ethan-krich/awesome-agent-orchestrators
- https://github.com/EvoMap/awesome-agent-swarm
- https://github.com/bradagi/awesome-cli-coding-agents
- https://github.com/orgs/i-am-bee/discussions/5
- https://rywalker.com/research/acp-agent-communication-protocol