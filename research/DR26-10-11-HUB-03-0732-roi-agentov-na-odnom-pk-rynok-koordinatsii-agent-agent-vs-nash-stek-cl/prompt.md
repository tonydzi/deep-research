# DR26-10-11-HUB-03-0732 — the exact prompt sent to every model

# DR26-10-11-HUB-03-0732 — Рои агентов на одном ПК: внешний веер (ChatGPT/claude.ai/Gemini/Grok/GLM/Mistral) к инхаус-отчёту 11.10

# Deep Research: local agent swarms on one PC (and a few PCs). Who builds the coordination rails, what to adopt, and an honest audit of our own stack

Research as of October 2026. Use web search extensively. Every factual claim about a tool must carry a URL (repo, docs, blog post, paper). Prefer primary sources (README, docs, release notes, maintainer posts) over listicles. Give GitHub star counts and last-commit dates where available, with the date you observed them. If something cannot be verified, say "not verified" rather than guessing.

## Context (who is asking)

A small independent lab runs a "fleet" of 8 machines (Windows, macOS, one Linux VPS). On each machine several coding agents work in parallel: Claude Code sessions, OpenAI Codex CLI, Cursor, Grok CLI, sometimes Gemini. They coordinate with each other through a home-grown open-source protocol called **claw-consensus** (https://github.com/tonydzi/claw-consensus, also https://github.com/tonydzi/join-the-fleet). Its design, in one paragraph:

- Transport: a folder synced by Syncthing between machines, append-only JSONL, **each node writes only its own file** (single-writer), plus a mirrored Telegram group chat so humans see the same traffic (dual-send). No central server.
- Accountability: every message read creates an **ACK debt**; silence beyond an SLA is an incident, not a default.
- Coordination primitives: `peer_help` ("one asks many, first to claim wins, second picks up if the first fails"), per-file write leases on a single machine, an "ON AIR" board declaring exclusive/collab/fyi work zones, work claims.
- Consensus: propose → counter/accept/reject → commit → verify by a second machine, round cap, deterministic tie-break by a fixed arbiter node, split-brain detection; Ed25519 signatures per machine.
- Governance: a **leader/follower canon** (only one node may change the rulebook; others are receive-only and send proposals), machine-readable **human approval tiers** (Tier-1 robots act alone; Tier-2 = money, irreversible deletion, secrets to third parties, mass outbound → human pause), "done is a claim and needs evidence from a second method".
- Vendor-neutral: roles (coordinator / implementer / reviewer) are assigned per task, not per vendor; an A2A Agent Card is used as the entry door for foreign agents.

The lab believes this combination (heterogeneous agents + several machines + no server + human approval tiers + leader/follower governance) is an empty cell in the market. **Your job is to try to prove them wrong**, and to map the market honestly.

## Questions (answer each with sources)

### Q1. Market map of agent↔agent coordination on a local computer (not drones, not robotics, not cloud agent platforms)

Cover at least these layers, and add anything you find that is missing:

1. **Vendor-built**: Claude Code Agent Teams, Claude Code cross-session messaging, Codex subagents, Cursor background agents, Gemini CLI multi-agent features, OpenCode / Pi / Hermes / OpenClaw style harnesses.
2. **Single-machine swarm orchestrators**: Gas Town + Beads (Steve Yegge), Paperclip, Claude Squad, LoopTroop, cmux, Conductor, amux, ai-maestro, agent-deck, and others from `awesome-agent-orchestrators` and `awesome-agent-swarm`.
3. **Message buses / mailboxes for heterogeneous agents**: agentbus (yanglinfang), llm-bus, agent-mesh (johnmwhitman), Alook, Agent Relay, AgentTeams (AgentScope, Matrix-based), OpenAgentMesh (NATS), mcp_agent_mail / mcp_agent_mail_rust, foremerge, teamctl, hcom, AgentBridge, Concord MCP, nats-channel, claude_codex_bridge, Agent Messaging Protocol. For each: transport, primitives (claim, lease, ack, quorum, receipts), which vendors it supports, whether it works across machines, whether it needs a server, license, stars, last commit.
4. **Protocols**: A2A (and ACP merged into it), MCP, ANP, OASF, anything newer in 2026. Which of them do local coding agents actually use in practice?

### Q2. Falsify the "empty cell" claim

Find any tool, product or paper (2025–2026) that provides **all five** at once: (a) heterogeneous vendors as equal peers, (b) multi-machine, (c) no central server (file sync / git / p2p), (d) machine-readable human approval tiers, (e) a governance model for who may change the rules. If none has all five, list the closest three and state which properties each lacks. Cite the arXiv paper "Governance Gaps in Agent Interoperability Protocols" (2606.31498) if relevant and say whether its gap list has been closed by anyone since.

### Q3. What should this lab adopt right now (practical, low cost)

Rank concrete, adoptable techniques with evidence that they work in production: intent+scope declarations before writing (foremerge style), TTL-based file reservations with ack-required messages (mcp_agent_mail style), git-backed task ledgers with atomic claim (Beads), socket-based cross-session messaging, MCP wrappers over a bus so non-Claude agents see it as a tool, verify-gated completion. For each: setup cost in hours, failure modes reported by real users, and whether it is worth it for 8 machines rather than 30 agents on one box.

### Q4. Failure modes of local swarms reported in the wild

Collect documented incidents: agents deleting code or breaking main (Gas Town), teammates not polling inboxes, stuck "in progress" tasks, $/hour cost blowups, duplicate work, lock wars, lost messages between receive and process (nats-channel), prompt injection via inter-agent messages. What mitigations did the authors adopt?

### Q5. Honest audit of claw-consensus from the outside

Read the public repo and docs. As a sceptical senior engineer: what is genuinely novel, what is reinventing existing tools, what is missing that a newcomer would expect (cockpit UI, MCP interface, cross-machine atomic claim, tests, benchmarks), and what three changes would most increase adoption by other labs?

## Output format

1. One-page executive summary (10 bullets max).
2. Comparison table: tools × properties (transport, vendors, multi-machine, server needed, claim/lease, ack, quorum/consensus, human tiers, governance, cost tracking, stars, last commit, license).
3. Answers Q1–Q5 with inline source URLs.
4. "What we could not verify" section.
5. Full source list with URLs.

Write in English. Length is not a constraint; completeness and sourcing are.
