# DR26-10-11-HUB-03-0732 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Executive summary is below, after the live-source pass. Gathering primary docs, repos, and the governance paper first.A. Executive summary

- The five-way cell is empty of any widely used product. No external tool checked in this run is at once (a) a peer of heterogeneous vendors, (b) multi-machine, (c) free of a central server, (d) carrying machine-readable human approval tiers, and (e) governing who may change the rules. The closest public artifact is the lab’s own https://github.com/tonydzi/claw-consensus: 4 stars, 0 forks, MIT, last push 2026-10-14T21:33:10Z (GitHub API, observed 2026-10-15).
- Every single property is crowded. Mailboxes, git ledgers, approval gates, and vendor sockets already exist. The scarce thing is the conjunction plus a scar catalog, not any one primitive.
- What local coding agents actually speak day to day is MCP, plus each vendor’s private channel. A2A is the cross-organization standard. It is not the inbox between Claude Code and Codex on one PC. IBM’s ACP was archived after the merge into A2A. Zed’s ACP is a different protocol (editor to agent).
- The 30 Jun 2026 paper https://arxiv.org/abs/2606.31498 still describes the standards gap: voting and dissent preservation are absent from MCP, A2A, ACP, ANP, and ERC-8004. No later protocol revision found here closes those two dimensions. claw-consensus sits above those standards. It is not a voting protocol either.
- Beads survived. Gas Town as an orchestrator did not, in the maintainer’s own telling. Beads is at 27,892 stars, MIT, pushed 2026-10-15T10:46:04Z. Do not adopt Gas Town as the fleet brain.
- Adopt now, in this order: an MCP face on the existing bus, intent-and-scope declarations per repo (Foremerge), and a Beads-style atomic claim only where the task graph is weaker than `bd ready`. Keep the second-machine verify gate. Do not add a second mailbox.
- The expensive failure in the wild is token burn and tasks stuck “in progress”, not missing consensus math. Claude’s own docs say agent teams cost far more tokens and that teammates fail to close tasks. Yegge’s public warning is that the swarm can wreck the repo.
- Paperclip is the product a newcomer will compare you with: 102,472 stars, MIT, pushed 2026-10-15T13:31:55Z, heterogeneous adapters, approval gates, budgets, audit. It fails the no-server test on purpose.
- Three adoption doors the public repo does not have: one HTTP MCP server, a read-only cockpit over the JSONL, and a two-machine demo that does not need Syncthing or Telegram.
- Several names in the brief were not found under the stated authors (`yanglinfang/agentbus`, `johnmwhitman/agent-mesh`, a repo literally called `nats-channel`). Substitutes are named below. Star counts are from `https://api.github.com/repos/...` on 2026-10-15 unless a row says otherwise.

B. Comparison table

Observed 2026-10-15. “Server” means a process someone must run beyond the agents and a file sync. “Partial” means the property exists in a weaker form. “n/v” means this run did not open a primary page that states it.

| Tool | Transport | Vendors | Multi-machine | Server | Claim / lease | Ack | Quorum | Human tiers | Rule governance | Cost tracking | Stars | Last push | License |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claw-consensus | Syncthing folder + JSONL, single-writer shards; optional chat rail | Agents that run the Python CLI; A2A card is the foreign door | Yes | No (Syncthing is the sync) | Work claim in the lab brief; not in the four public docs opened | Yes, ACK debt | propose/counter/accept/commit, round cap, fixed arbiter; split-brain detected | Tier-0/1 auto, Tier-2 human, keyword tripwire | Leader commits canon; followers receive-only | No | 4 | 2026-10-14 | MIT |
| Claude Code agent teams | In-process or tmux; inbox JSON under «внутренний путь лаборатории» | Claude only | No (one session) | No | Shared task list, self-claim | Messages, not an SLA | No | Permission prompts, not money/secrets tiers | Lead vs teammates | Docs warn of high token use | claude-code repo 151,000 | 2026-10-14 | n/v (source-available, not a stated SPDX in the API payload) |
| Claude Code cross-session | Per-session Unix socket or Windows named pipe; other machines via Anthropic Remote Control | Claude only | Same-machine local; other machines through Anthropic | No on one machine; yes for cross-machine | No | Delivery states: delivered / held / refused | No | Inbound accept/hold/refuse; permissions stay per session | No | Counts as usage | same repo | same | same |
| Codex subagents | In-process agent threads | OpenAI Codex | No | No | No | Result returned to parent | No | No | No | Docs: more tokens than one agent | openai/codex 129,361 | 2026-10-15 | Apache-2.0 |
| Cursor Cloud Agents | Cursor cloud VM, or tool calls on your machine via an outbound worker | Cursor | Cloud or “My Machines” | Yes (Cursor cloud runs the loop) | No peer claim | No | No | Dashboard controls | No | n/v | Product, not this API set | Docs current 2026 | Proprietary |
| Gemini CLI subagents | In-session specialists; remote via A2A | Gemini; remote A2A agents | Remote subagents only | Local no; remote yes | No | No | No | Tool allowlists | No | n/v | google-gemini/gemini-cli 107,381 | 2026-10-14 | Apache-2.0 |
| OpenCode | The agent CLI itself | Many models | No native fleet bus found | No | No | No | No | No | No | n/v | anomalyco/opencode 213,752 | 2026-10-15 | MIT |
| Pi | Terminal harness + SDK | Many models | No | No | No | No | No | No | No | n/v | earendil-works/pi 115,364 | 2026-10-15 | MIT |
| Hermes Agent | Messaging-gateway harness | Many models | Gateway, not a file bus | The gateway | No consensus claim | n/v | No | n/v | No | n/v | NousResearch/hermes-agent 253,979 | 2026-10-15 | MIT |
| OpenClaw | Personal-assistant gateway over Pi’s SDK | Many models, many chat channels | Devices via its gateway | Gateway | No | n/v | No | n/v | No | n/v | openclaw/openclaw 392,078 | 2026-10-15 | MIT |
| Beads | Git-backed issues; Dolt optional for multi-writer | Any agent that runs `bd` | Yes, via git or `bd dolt push/pull` | No for git; Dolt if you federate | `bd update --claim` and `bd ready --claim`, atomic | No | No | No | No | No | gastownhall/beads 27,892 | 2026-10-15 | MIT |
| Gas Town | tmux + Beads; Wasteland federates towns | Claude, Codex, Gemini, Copilot, Cursor (secondary write-up) | Wasteland via Dolt | Dolt for federation | Beads claims; polecats in worktrees | Town mail on Beads | No voting protocol | Merge queue, not Tier-1/2 | Maintainer-driven; issue #3649 disputed agents spending user credits | Famous for high spend; no meter in-protocol | gastownhall/gastown 18,377 | 2026-10-08 | MIT |
| Paperclip | App server, process/HTTP adapters | Claude Code, Codex, Cursor, Gemini CLI, OpenCode, Pi, Hermes, Grok, OpenClaw | Self-hosted deployment | Yes | Issues, heartbeats, plan revision | Approval queue | No | Hire, strategy, budget, external-action gates | Board (the human) approves; not a leader/follower rulebook | Yes, monthly budgets | paperclipai/paperclip 102,472 | 2026-10-15 | MIT |
| Claude Squad | tmux + git worktrees | Claude, Codex, Gemini, Aider, Amp, OpenCode | No | No | No | No | No | No | No | No | smtg-ai/claude-squad 8,607 | 2026-08-20 | AGPL-3.0 |
| LoopTroop | Local GUI, OpenCode worktrees | OpenCode | No | Local app | One active ticket in the execution band | No | LLM council, not a machine quorum | Human-gated PR | No | n/v | looptroop-ai/LoopTroop 162 | 2026-10-14 | MIT |
| cmux | Native macOS terminal (Ghostty) | Many CLIs side by side, including Claude teams | SSH workspace, not a bus | No | No | Notifications when a human is needed | No | No | No | n/v | manaflow-ai/cmux 28,185 | 2026-10-15 | SPDX not returned (NOASSERTION) |
| amux | Local HTTPS + web/PWA | Claude Code, Codex, Gemini CLI | Remote access to one host | Yes, local server | “Atomic tasks” in the repo description | Origin-stamped messaging claimed in the description | No | Phone approvals claimed by the vendor | No | Token spend in the dashboard (vendor blog) | mixpeek/amux 534 | 2026-10-15 | Vendor says MIT; API returned NOASSERTION |
| agent-deck | Local web + SQLite | Claude Code | No | Local server | DAG scheduling | No | No | No | No | Yes, in the UI | claude-world/agent-deck 5 | 2026-06-12 | MIT |
| Conductor | Mac app, worktrees | n/v beyond “coding agents” | No | The app | Checkpoints, not a claim protocol | No | No | No | No | n/v | No public repo (meltylabs/conductor 404) | n/v | Closed source, per a 2026 comparison |
| mcp_agent_mail | HTTP MCP + Git archive + SQLite | Any MCP client; hooks for Claude, Codex | If clients share the server | Yes | TTL file reservations, advisory | `acknowledge_message`; overdue-ACK CLI | No | “Human Overseer” mailbox, not Tier-2 | No | No | Dicklesworthstone/mcp_agent_mail 2,206 | 2026-10-15 | README: MIT; API NOASSERTION |
| mcp_agent_mail_rust | Same idea, Rust, TUI + web | Claude, Codex, Gemini CLI, Copilot, any MCP | Same | Yes | Advisory TTL leases + pre-commit guard | Yes | No | Human-auditable git artifacts | No | No | 199 | 2026-10-15 | README: MIT plus an OpenAI/Anthropic rider |
| Foremerge | SQLite in the repo’s git common dir; CLI, JSON API, MCP | Claude, Codex, Cursor | One clone unless you sync `.git` yourself | No separate server | Semantic claims, leased, advisory | No message ACK | No | No | No | No | naw103/foremerge 539 | 2026-10-14 | Apache-2.0 |
| llm-bus | MCP over HTTP | Any MCP client | If they share the host | Yes (self-host or llm-bus.com) | Atomic claim ids; advisory leases | Reading a post addressed to you marks it read | No | No | No | No | danieldoderlein/llm-bus 9 | 2026-09-03 | AGPL-3.0 |
| agentbus (ericfitz) | stdio MCP + one SQLite file | Claude Code, Codex, Grok Build | No | No daemon | Shared task list | Reply threads, not an SLA | No | No | No | No | ericfitz/agentbus 4 | 2026-10-11 | Apache-2.0 |
| hcom | Local hooks; cross-device via MQTT relay | Claude, Codex, Gemini, OpenCode, Cursor, Grok, Hermes, Pi, Kimi, Copilot, others | Yes, with the relay | Relay for cross-device; local otherwise | Collision subscribe, not a lease | Real-time wake | No | No | No | No | aannoo/hcom 572 | 2026-10-15 | MIT |
| Concord MCP | Local MCP | Claude Code, Codex, Cursor, Gemini CLI, Grok, Goose | n/v beyond one workspace | Local MCP server | `start_work` claims a task and reports scope overlap | Threads and prompt/reply | No | Handoff with evidence; not money tiers | No | No | Get-Concord-AI/concord-mcp page showed 508; push date n/v | n/v | n/v |
| teamctl | tmux + SQLite mailbox + stdio MCP | Claude Code, Codex, Gemini, OpenCode | One host | Supervisor | Channels, DMs | `inbox_ack` | No | `request_approval` on tagged tools | YAML team recipe, not a canon | `budget` subcommand | n/v | ROADMAP current in-tree | MIT (crate and repo page) |
| AgentTeams | Matrix rooms + controller (Higress, Tuwunel, MinIO) | Agent containers, not a fixed vendor list | Yes, including Kubernetes | Yes | Manager assigns workers | Room timeline | No | Human sits in the room | Manager/worker, not leader/follower canon | n/v | agentscope-ai/AgentTeams 5,757 | 2026-10-15 | Apache-2.0 |
| Agent Relay | Hosted-or-self workspace, MCP | Claude, Codex, others | Yes | Yes for the shared workspace | n/v | Channels, threads, DMs | No | n/v | No | n/v | AgentWorkforce/relay 884 | 2026-10-15 | Apache-2.0 |
| Alook | Open-source “room”; agents stay on the laptop | Claude Code and Codex named in the launch post | Yes, as a shared room | The room host | n/v (README not opened) | Inbox implied by the launch post | No | n/v | No | n/v | alookai/alook 1,398 | 2026-10-15 | Apache-2.0 |
| OpenAgentMesh | NATS | Python agents | Yes | NATS | KV/objects, not file leases | Request/reply | No | No | No | No | openagentmesh/openagentmesh 7 | 2026-10-15 | MIT |
| Cotal | NATS JetStream | Any NATS client | Yes | NATS | Work queue, exactly-once handoff claimed | Durable per-agent inbox | No | No | No | No | Cotal-AI/Cotal 328 | 2026-10-15 | Apache-2.0 |
| Google Agent Mesh | Direct peer links plus relays; local MCP | Any agent that enrolls | Yes | Control plane and relays | No coding-file lease | n/v | No | OIDC enrollment | The mesh operator | No | google/agentmesh 1,071 | 2026-10-15 | Apache-2.0 |
| AgentsMesh | Postgres, Redis, MinIO, relay | Coding agents on your machines | Yes | Yes | Schedule/isolate | n/v | No | Console steering | No | n/v | AgentsMesh/AgentsMesh 2,360 | 2026-09-23 | SPDX not returned |
| AMP (agentmessaging) | WebSocket, webhook, relay | Framework-agnostic, Claude plugin exists | Yes, federation designed | Reference server still marked “coming soon” on the org page | No | Relay queue | No | No | No | No | agentmessaging/protocol 36 | 2026-10-04 | Apache-2.0 |
| A2A | HTTP / JSON-RPC, Agent Cards | Cross-vendor by spec | Yes | Each agent’s endpoint | Task lifecycle, not a file lease | Task states | No | No | Linux Foundation, then AAIF (secondary, Aug 2026) | No | a2aproject/A2A 26,162 | 2026-10-13 | Apache-2.0 |
| MCP | stdio or HTTP, client/server | Almost every coding CLI | If the server is reachable | The MCP server | Not in the spec | Not in the spec | No | Elicitation is partial, not a tier system | Agentic AI Foundation | No | modelcontextprotocol/modelcontextprotocol 9,459 | 2026-10-15 | SPDX not returned |
| ANP | HTTP, DIDs, discovery | Internet agents | Yes | No single registry required | No | Messaging specs | No | Draft authorization, not Tier-2 | Spec maintainers | No | AgentNetworkProtocol 1,449 | 2026-10-15 | Apache-2.0 |
| OASF | Schema / protobuf, not a bus | Describers of agents | n/a | n/a | No | No | No | No | agntcy maintainers | No | agntcy/oasf 339 | 2026-10-13 | Apache-2.0 |
| Zed ACP (Agent Client Protocol) | JSON-RPC over stdio | Editor to one coding agent | No | No | No | Session updates | No | Permission requests | Maintainer community | No | agent-client-protocol 4,450 | 2026-10-15 | Apache-2.0 |
| IBM ACP | REST, archived | Historical | Was | Was | Negotiation performatives | Message types | No | No | Merged into A2A | No | i-am-bee/acp 1,019 | 2025-08-25, archived | Apache-2.0 |

C. Answers

Q1. Market map

1. Vendor-built

Claude Code has three different mechanisms, and the docs tell you not to confuse them.

Agent teams are experimental and off by default. Set `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`. One interactive session is the lead. Teammates are separate Claude Code processes with their own context. They share a task list, message each other by name, and can self-claim the next unassigned task. Teams do not form under `claude -p`. One session has one team. Teammates cannot spawn teammates. `/resume` does not restore in-process teammates. Task status lags: a teammate can finish the work and forget to mark the task done, which blocks dependents. https://code.claude.com/docs/en/agent-teams

Subagents live inside one session and report back to the caller. They are the cheaper parallel tool. https://code.claude.com/docs/en/agent-teams

Cross-session messaging is for sessions the human started. It needs Claude Code v2.1.224+ on macOS and Linux (including WSL 2) and v2.1.234+ on native Windows. Same-machine delivery is a per-session Unix socket, or a named pipe on native Windows, and does not go through Anthropic. A session on another machine, or Claude Code on the web, is reached through Anthropic over Remote Control. A message is plain text. It is not the sender’s history and not the user’s permission. The receiver can deliver, hold, or refuse it. A burst to a local inbox is refused once the inbox’s cap is hit. https://code.claude.com/docs/en/cross-session-messaging

Codex subagents are parallel threads inside one Codex session. Current docs say they are on by default in the app and CLI. Built-in roles are `default`, `worker`, and `explorer`. Custom agents are TOML files in `~/.codex/agents/` or `.codex/agents/`. Codex does not spawn them unless you ask. Each subagent spends its own tokens. This is not a bus to Claude or Cursor. https://developers.openai.com/codex/subagents and https://developers.openai.com/codex/concepts/subagents

Cursor’s old “Background Agents” are now Cloud Agents. The agent loop runs in Cursor’s cloud. Tool calls run in a Cursor VM, or on a machine you own (“My Machines”) via an outbound worker, or on an org pool. There is no peer protocol between a Cursor agent and a Codex agent. https://cursor.com/docs/cloud-agent and https://cursor.com/docs/cloud-agent/my-machines and https://cursor.com/docs/cloud-agent/choose-runtime

Gemini CLI gained subagents on 15 Apr 2026: Markdown-plus-YAML specialists in `~/.gemini/agents` or `.gemini/agents`, delegated by the main agent, unable to spawn further subagents in the usual setup. The same docs add remote subagents over A2A. That is the one vendor CLI in this set whose own docs point at A2A for a remote peer. https://developers.googleblog.com/subagents-have-arrived-in-gemini-cli and https://github.com/google-gemini/gemini-cli/blob/main/docs/core/subagents.md

OpenCode (213,752 stars), Pi (115,364), Hermes Agent (253,979), and OpenClaw (392,078) are harnesses, not coordination protocols. OpenClaw embeds Pi’s agent SDK and adds chat channels. Hermes is Nous Research’s gateway harness with a self-improving skill loop. A swarm plugin can sit on top of OpenCode. That is an add-on, not the CLI. Star counts and push times are the API figures above. Pi’s maintainer pointed at a subagent plugin on 14 Oct 2026: https://github.com/badlogic/pi-subagent. A May 2026 comparison of the three harnesses, with star counts that have since grown, is at https://claw4science.org/blog/agent-harness-three-way.

ClawFleet is a nearby name and a different product: Docker on one Mac or Linux box, OpenClaw and Hermes, collaboration in Discord, a browser dashboard. https://github.com/clawfleet/ClawFleet/blob/main/README.md

2. Single-machine swarm orchestrators

The lists to start from are https://github.com/andyrewlee/awesome-agent-orchestrators (2,200 stars, pushed 2026-10-14, CC0) and https://github.com/EvoMap/awesome-agent-swarm (55 stars, pushed 2026-10-14). The second is thin. The first is the useful catalog.

Beads is the piece that matters. It is a git-backed issue graph: `bd ready` lists unblocked work, `bd update <id> --claim` atomically sets assignee and `in_progress`, `bd close` releases dependents. The graph can move with git, or with Dolt push/pull. Any harness that can run a binary can use it. Docs: https://beads.gascity.com/cli-reference/update and https://github.com/gastownhall/beads/blob/main/README.md. The project account said downloads passed 1.5 million on 2 Oct 2026: https://x.com/Steve_Yegge/status/2107654290944786683 (quoted post).

Gas Town, open-sourced 1 Jan 2026, is Yegge’s orchestrator on top of Beads: a Mayor, ephemeral Polecat workers in worktrees, a merge queue, tmux. His own launch post says the code was weeks old, entirely vibe-coded, and that the agents can wreck the repo. https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04 (archive: https://archive.ph/2026.01.01-221415/https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04). His current product page still describes Gas Town, Beads, and the Wasteland (towns linked together, launched March 2026): https://yegge.ai/gastown.html. The repo https://github.com/gastownhall/gastown was pushed 2026-10-08 and has 18,377 stars. On 30 Jan 2026 he posted that main was broken: Dolt migration, beads/mail routing “totally busted”. https://github.com/steveyegge/gastown/discussions/1109. Secondary write-ups say he later shut the orchestrator down after an Opus 4.7 failure to converge and after spending thousands a month. Those posts were not opened as primary Yegge text in this run. See section D.

Paperclip is the large product in this layer. The docs describe a self-hosted app that treats agents as a company: issues, routines, artifacts, budgets, and an approval queue. Hiring an agent, the CEO’s strategy, and budget stops wait for a human. The CEO cannot move tasks to “in progress” until the strategy is approved. Adapters named in a 13 Oct 2026 product summary: Claude Code, Codex, OpenClaw, Cursor, Gemini CLI, OpenCode, Pi, Hermes, Grok, Kimi, plus process and HTTP. https://docs.paperclip.ing/guides/day-to-day/approvals/ and https://paperclip.ing/product/governance/ and https://github.com/paperclipai/paperclip. It is a server. It is not file sync.

Claude Squad manages many terminal agents in tmux and git worktrees. It does not give them a mailbox or a claim protocol. Last push 2026-08-20, so it is quieter than the October field. https://github.com/smtg-ai/claude-squad

LoopTroop is a local GUI from ticket to PR: an LLM council plans, a Ralph loop retries, OpenCode runs in a worktree, a human gates the PR. Alpha limit: one active ticket in the execution band. https://github.com/looptroop-ai/LoopTroop

cmux is a macOS terminal for many agent CLIs at once, with notifications when one needs a human, and a `cmux claude-teams` helper. It is a multiplexer, not a protocol. 28,185 stars, pushed 2026-10-15. https://github.com/manaflow-ai

amux is a Rust control plane (local HTTPS, kanban, watchdog, phone). The repo description lists parallel Claude, Codex, and Gemini workers, atomic tasks, and self-healing. 534 stars. https://github.com/mixpeek/amux and https://amux.io/press/

agent-deck is a small web DAG runner aimed at Claude Code, with cost display and git finalize. 5 stars, last push 2026-06-12. Treat it as a sketch. https://github.com/claude-world/agent-deck

Conductor, described by amux’s May 2026 comparison as a closed-source Mac app from Melty Labs with worktree checkpoints and no public orchestrator repo, was not found at `meltylabs/conductor` (API 404). https://amux.io/blog/best-multi-agent-orchestrators-2026/

ai-maestro, as found here, is a Claude Code plugin that distributes GitHub-issue modules and gates exit on a checklist. 14 stars on the orchestrator-agent repo. It is not a multi-vendor bus. https://github.com/Emasoft/ai-maestro-orchestrator-agent

3. Message buses and mailboxes

mcp_agent_mail (Python, 2,206 stars) and its Rust rewrite (199 stars) are the reference design for this layer. Agents get identities, an inbox, threaded Markdown, and advisory TTL leases on paths or globs. Conflicts come back beside grants. A pre-commit hook can block a commit that violates someone else’s exclusive lease. Ack is a tool, and a CLI lists overdue acks. Storage is Git for humans and SQLite for search. It is an HTTP MCP server, so non-Claude agents see it as tools. It does not by itself span eight machines unless they share that server. https://github.com/Dicklesworthstone/mcp_agent_mail and https://github.com/Dicklesworthstone/mcp_agent_mail/blob/main/SKILL.md and https://github.com/Dicklesworthstone/mcp_agent_mail_rust

Foremerge is the intent layer. Before code, an agent publishes a task, a summary, and semantic scopes (`symbol:PaymentService=replace`, `api:POST /v1/charges`, `schema:invoices.total`, plus file, env, migration, contract). Claims are leased and advisory. The state machine runs intent, claim, in progress, provisional changeset, validated, accepted, committed. Detection is deterministic, with no LLM on the conflict path. One Rust binary, SQLite inside the repo’s git dir, CLI plus local JSON plus MCP. The maintainer says wild compliance is whatever the system prompt enforces, and that hard numbers on compliance are not yet public. https://github.com/naw103/foremerge/blob/main/README.md and https://github.com/naw103/foremerge/blob/main/docs/state-model.md and https://foremerge.com/blog/31-questions-coordinating-parallel-coding-agents/

llm-bus exposes claims, leases, presence, tasks, and a ledger over MCP. Reading a post addressed to you marks it read. There is no separate ack. Self-host or use the hosted service. 9 stars, last push 2026-09-03, AGPL-3.0. https://github.com/danieldoderlein/llm-bus

ericfitz/agentbus is one Go binary, one SQLite file, stdio MCP, no daemon. Channels, DMs, memories, a shared task list. Claude Code, Codex, Grok Build. Explicitly one machine. 4 stars. https://github.com/ericfitz/agentbus. This is not a repo under the name yanglinfang. That path was not found.

hcom wraps the CLIs you already run (`claude`, `codex`, `gemini`, `grok`, `cursor`, `hermes`, `pi`, and others). Agents message, watch each other’s terminals, and spawn each other. Cross-device is an MQTT relay and a token you are told to treat like an SSH key. That relay is a server. 572 stars, MIT, pushed 2026-10-15. https://github.com/aannoo/hcom and https://pypi.org/project/hcom/

Concord MCP’s README lists five tools: `start_work` (presence, claim, scope-overlap warning), `inspect_work`, `update_work`, `transfer_work`, `finish_work` (evidence, then review-ready or closed). Named clients: Claude Code, Codex, Cursor, Gemini CLI, Grok Build, Goose. The GitHub page snippet showed 508 stars. Push date was not in the API batch. https://github.com/Get-Concord-AI/concord-mcp

teamctl is a YAML recipe of real Claude, Codex, Gemini, and OpenCode sessions in tmux, with a SQLite mailbox, `inbox_ack`, and `request_approval` for tagged tool calls. Star count was not fetched. https://github.com/Alireza29675/teamctl and https://github.com/Alireza29675/teamctl/blob/main/ROADMAP.md

AgentTeams (AgentScope) puts a manager and workers in Matrix rooms so a human can read and interrupt. The controller stack is Higress, a Matrix homeserver, MinIO, and Element. That is a server, and a hierarchy, not equal peers. 5,757 stars, Apache-2.0, pushed 2026-10-15. https://github.com/agentscope-ai/AgentTeams and https://github.com/agentscope-ai/AgentTeams/blob/main/docs/design/architecture.md

Agent Relay gives Claude and Codex shared channels across machines, plus GitHub, Linear, Notion, and Slack as a virtual filesystem. 884 stars. It is infrastructure you join, not a folder you sync. https://github.com/AgentWorkforce/relay

Alook’s launch posts describe handles, DMs, threads, and channels, with the agent still on the laptop. 1,398 stars, Apache-2.0, pushed 2026-10-15. The README was not opened, so claim, lease, and ack are n/v. https://github.com/alookai/alook and https://x.com/venoyuls/status/2108250738258280788

OpenAgentMesh is a Python SDK over NATS for in-team request/reply, pub/sub, and KV. 7 stars. It needs NATS. https://github.com/openagentmesh/openagentmesh. A different “agent-mesh” by kasimmj is Docker plus NATS plus a workflow POST endpoint (1 star). https://github.com/kasimmj/agent-mesh. NG-Bullseye/agent-mesh is an MCP/SSE hub for Claude Code tmux sessions on one machine (0 stars). https://github.com/NG-Bullseye/agent-mesh. johnmwhitman/agent-mesh was not found.

Cotal is the NATS-native team bus that matches the “lost between receive and process” worry: multicast on a subject, unicast into a JetStream durable consumer so a busy or offline agent does not drop the message, presence in a KV bucket with TTL. 328 stars. Server required. https://nats.io/blog/coordinating-ai-agent-teams-on-nats/ and https://github.com/Cotal-AI/Cotal. Synadia’s agent protocol separately requires one ack status chunk before any slow work, so the caller can reset an inactivity timer. https://nats.io/blog/nats-native-protocol-for-ai-agents/ and https://github.com/synadia-ai/synadia-agents (86 stars).

Google Agent Mesh enrolls a node with OIDC, gives it a short-lived credential, connects directly or through a relay, and exposes the mesh to the local agent as MCP on 127.0.0.1. 1,071 stars, Apache-2.0. It is a network, and it has a control plane. https://github.com/google/agentmesh

AgentsMesh is a console for “a hundred coding agents on your own machines”, and the quick start brings up PostgreSQL, Redis, MinIO, a backend, and a relay. 2,360 stars, last push 2026-09-23. Server required. https://github.com/AgentsMesh/AgentsMesh

AMP (Agent Messaging Protocol) specifies signed JSON envelopes (Ed25519), WebSocket, webhooks, and a relay, and is explicit about the early-2026 OpenClaw exposure incident as motivation. The org page still marks the reference server “coming soon”. The spec repo has 36 stars. https://github.com/agentmessaging

Two smaller local buses found while looking for the missing names: tulong66/agent-bus-mcp (SQLite WAL, Claude, Codex, Antigravity, DeepSeek; 0 stars) https://github.com/tulong66/agent-bus-mcp and agent-coord-mcp (JSONL rooms, stdio or HTTP, bearer identity) https://www.npmjs.com/package/agent-coord-mcp. HakAl/agent-comms is a no-daemon SQLite mailbox for Claude and Codex on one workstation, with a signed SSH approval before push. https://github.com/HakAl/agent-comms. That last one is the nearest small cousin of the lab’s “human signature before an irreversible git action”.

4. Protocols, and what coding agents actually use

MCP connects an agent to tools. It is client/server JSON-RPC. It does not define a peer group, a vote, or a file lease. Local coding agents use it constantly, which is why every serious mailbox in the table above is an MCP server. Spec repo: https://github.com/modelcontextprotocol/modelcontextprotocol (9,459 stars, pushed 2026-10-15).

A2A connects agents to agents: Agent Cards, tasks, messages, opaque internals. Google launched it in April 2025, donated it to the Linux Foundation in June 2025, and shipped v1.0 in March 2026, v1.0.1 in May 2026. IBM’s ACP merged in at the end of August 2025. The ACP repo’s last push is 2025-08-25. https://github.com/a2aproject/A2A and https://github.com/i-am-bee/acp. A secondary chronology says A2A was accepted into the Agentic AI Foundation on 17 Aug 2026, alongside MCP: https://nerdleveltech.com/a2a-protocol-joins-agentic-ai-foundation. In local coding practice, A2A shows up in two places found here: Gemini CLI’s remote subagents, and claw-consensus’s join door (`fleet/JOIN.md` requires an A2A Agent Card). It is not how two Claude sessions on one laptop talk.

“ACP” now usually means Zed’s Agent Client Protocol: JSON-RPC between an editor and one coding agent, Apache-2.0, 4,450 stars, pushed 2026-10-15. https://github.com/agentclientprotocol/agent-client-protocol. Do not confuse it with IBM’s archived ACP. A clear split of the two ACPs is at https://crystl.dev/blog/agent-communication-protocols/.

ANP is an internet-scale suite (current document set called 1.2): DID authentication, `did:wba`, agent description, discovery, and end-to-end messaging drafts. 1,449 stars, pushed 2026-10-15. https://github.com/agent-network-protocol/AgentNetworkProtocol. Nothing in the coding-orchestrator set above uses ANP as the wire between Claude and Codex.

OASF is a schema for describing agent capabilities, explicitly inspired by OCSF. It is not a mailbox. 339 stars. https://docs.agntcy.org/oasf/open-agentic-schema-framework/ and https://buf.build/agntcy/oasf.

ERC-8004, as summarized by the governance paper, is an Ethereum draft for on-chain identity, reputation, and validation registries. The EIP text was not opened in this run. The paper’s citation is the source used here.

Practical answer: on one PC, coding agents coordinate through MCP tools, vendor sockets, tmux, and git. Across organizations, the paper standard is A2A. Across an open agent web, ANP is a spec with a small repo. None of these five standards encodes who may rewrite the rules.

Q2. Falsifying the empty cell

The cell, restated: (a) heterogeneous vendors as equal peers, (b) more than one machine, (c) no central server, file sync or git or p2p, (d) machine-readable human approval tiers, (e) a governance rule for who may change the rules.

No external project opened in this run has all five.

Closest three:

Paperclip has (a) and a real version of (d): hire, strategy, budget, and external actions wait for a human, and the product page says gates can cover posts, email, shared infrastructure, and spend. It has an audit trail. It fails (c): it is an application server. It fails (e) in the lab’s sense: the human is a board, not a single canon-writing node with receive-only followers. Multi-machine (b) is “run the server where you like and point agents at it”, which is the opposite of no server. https://paperclip.ing/product/governance/ and https://docs.paperclip.ing/guides/day-to-day/approvals/

Beads plus Gas Town’s Wasteland has (a) at the harness edge and (b) if you push the ledger. Plain Beads-on-git is close to (c). Wasteland federation, in Yegge’s January 2026 post and his product page, goes through Dolt, which is a server. There is no machine-readable Tier-2 for money, secrets, and irreversible deletion. There is no leader/follower rule for who may edit the protocol. https://yegge.ai/gastown.html and https://github.com/gastownhall/beads

Concord, teamctl, and mcp_agent_mail, taken as one class, have (a), plus claim, lease, or ack, and teamctl adds `request_approval`. They fail (b) and (c) together: the interesting operations sit in a SQLite or HTTP process on one host. They do not govern changes to their own rulebook. HakAl/agent-comms adds a signed human push approval and no daemon, and still says “one workstation”. https://github.com/Get-Concord-AI/concord-mcp and https://github.com/Alireza29675/teamctl/blob/main/ROADMAP.md and https://github.com/HakAl/agent-comms

claw-consensus, read from the public tree, is the thing that actually stacks (b), (c), (d), and (e), with (a) only at the edge. The reference engine is stdlib Python over a synced folder. Tier-2 is money, outbound, irreversible actions, secrets, and canon edits, and peer consensus cannot substitute for the human. Followers receive canon as read-only sync shares. A second machine must verify. Ed25519 signatures are produced with `ssh-keygen -Y`. https://github.com/tonydzi/claw-consensus/blob/main/README.md and https://raw.githubusercontent.com/tonydzi/claw-consensus/main/docs/GOVERNANCE.md. Equal foreign vendors are not peers of the bus until someone writes an adapter. The join path gives an outsider an A2A card and the tier OBSERVER, which the join doc defines as “listed, no access, no authority”. https://github.com/tonydzi/claw-consensus/blob/main/fleet/JOIN.md. So (a) is designed and not yet a lived multi-vendor mesh in the public repo. Stars: 4.

The paper. Richard Kang and Yudho Diponegoro, “Governance Gaps in Agent Interoperability Protocols: What MCP, A2A, and ACP Cannot Express”, arXiv:2606.31498, submitted 30 Jun 2026. https://arxiv.org/abs/2606.31498 and the HTML at https://arxiv.org/html/2606.31498v1. They score MCP v1.1, A2A v1.0.1, ACP, ANP, and ERC-8004 on six dimensions: membership, deliberation, voting, dissent preservation, human escalation, audit/replay. Voting and dissent are absent in all five. Deliberation is absent or at most partial. No protocol has the full set. They call this a missing layer above the standards, not a missing feature inside them. A2A’s extension slot is noted, and the four official extensions they list (Secure Passport, Timestamp, Traceability, Agent Gateway Protocol) do not add governance.

Has anyone closed that gap since 30 Jun 2026? Not on the evidence in this run. A2A’s move into the Agentic AI Foundation, if the August 2026 secondary reports are right, changes the foundation, not the vocabulary. ANP 1.2, read from the repo index, adds identity, description, discovery, and messaging. It does not add a vote or a preserved minority. https://github.com/agent-network-protocol/AgentNetworkProtocol. claw-consensus is the nearest implemented layer: counters stay in an append-only log (partial dissent), a human tier is mandatory for a listed risk class (human escalation), and the ledger can be replayed (audit). It still has no ballot, no quorum of many peers, and no rule that a losing position must appear in the decision record as a dissent. The paper’s gap is not closed.

Q3. What to adopt now

Ranked for eight heterogeneous machines, not for thirty agents in one tmux.

1. An MCP server in front of the bus you already have. Setup: about one to two days for a single HTTP process with send, read, ack, propose, and “who owes an ack”. Worth it on eight machines because Codex, Cursor, Gemini, and Grok already speak MCP and will not grow a Syncthing client. Failure mode, reported across this whole class and not unique to you: the model does not call the tool unless the prompt or a hook says so. Foremerge’s maintainer says compliance is the system prompt. Concord and mcp_agent_mail exist because “the files are shared” is not enough. A second failure, already measured on this fleet’s own machines in other work, is a stdio MCP process per session. Use one HTTP server per machine, not stdio. This is the highest-leverage change because it spends the bus you have.

2. Foremerge-style intent and scope before edits, per repo. Setup: under an hour once the binary is installed. The agent publishes `symbol`, `api`, `schema`, `migration`, or `file` scopes, then claims them. Claims expire. Overlap warns. It does not lock. https://foremerge.com/ and https://github.com/naw103/foremerge/blob/main/docs/protocol.md. Failure modes the maintainer names: agents skip the declaration, file-only scopes miss API and schema collisions, and drift between the declaration and the later diff is still on the 0.5.0 roadmap. Worth it on each machine where several agents share a repo. Not a cross-machine bus. The SQLite file lives in that clone’s git dir. Eight machines get eight stores unless you add sync, which you should not do for `.git`.

3. TTL file reservations with required acks, mcp_agent_mail style, only where you do not already lease. You already describe per-file leases and ACK debt. Adopting the Python or Rust server beside that would create two sources of truth. Steal the mechanics you lack: a pre-commit hook that fails on an exclusive foreign lease, an overdue-ack listing, and renewal before expiry. Their default TTL example is 3600 seconds. https://github.com/Dicklesworthstone/mcp_agent_mail/blob/main/SKILL.md. Reported failures: leases were advisory and agents edited anyway (the project then added the hook), SQLite naive-versus-aware datetimes made expiry tests flake, and macOS file-descriptor exhaustion forced a pool change. Those are in the release notes. https://github.com/Dicklesworthstone/mcp_agent_mail/releases. Setup if you did not have leases: half a day, plus a shared server if the lease must span machines. For eight machines a shared lease server is a central service. Prefer it only for repos where agents on different hosts edit the same clone. Otherwise keep leases local.

4. A git-backed task ledger with atomic claim, Beads, if your task graph is weaker than `bd ready`. Setup: a few hours to `bd init` on one repo and teach agents `bd ready`, `bd update --claim`, `bd close`. The claim is atomic and idempotent if you already hold it. https://beads.gascity.com/cli-reference/update. Failure modes from the maintainer: agents stuck on `bd sync` (his stated reason for moving Gas Town to Dolt), and a January 2026 stretch where mail routing on main was broken. https://github.com/steveyegge/gastown/discussions/1109. Two writers on one JSONL in git will conflict. That is the bug your single-writer rule was built to kill. Use Beads as the per-repo work graph, or keep your ledger and copy only the claim verb. Do not point eight machines at one Beads JSONL without Dolt or a single writer. Worth it when tasks have dependencies. Not worth it as a replacement for the consensus log.

5. Claude’s same-machine sockets, as an optimization between Claude sessions only. Setup: none, once the version is current. They are the wrong fleet bus. They do not see Codex. Cross-machine traffic goes through Anthropic. https://code.claude.com/docs/en/cross-session-messaging. Worth it so two Claude sessions on one box stop using the human as a courier. Not worth teaching the other seven machines to depend on it.

6. Verify-gated completion. You already require two verifiers, one of whom is not the committer, and you reject a proof copied from an earlier verify. https://raw.githubusercontent.com/tonydzi/claw-consensus/main/docs/FAILURE-MODES.md (section E). Keep it. The outside evidence that this class is real: Claude’s agent-team docs say teammates leave tasks “in progress” and block dependents, and Foremerge’s whole product is “plans collide before the merge”. Do not replace your gate with a vendor’s task checkbox.

7. Do not adopt, for this fleet, Gas Town, a second orchestrator UI, or hcom’s MQTT relay as the system of record. Gas Town’s own author told readers to stay out while it was young, and later commentary says it never built anything but itself. hcom is a good one-afternoon experiment on a single box (minutes to install) and a poor eight-machine spine, because the cross-device path is a shared token and a broker. Paperclip is the UI to copy ideas from, not to insert under a no-server rule. Standing it up is days, and it becomes the server you avoided.

Q4. Failure modes reported in the wild

Gas Town and Beads. The launch essay says the agents are able to wreck the repo, the codebase was under three weeks old, and the author had not read it. https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04. On 30 Jan 2026 he wrote that main was broken: CGO and Dolt binary mismatches, beads and mail routing dead, hook upgrades painful, agents stuck on `bd sync`. Mitigation he named: Dolt as the forward path, Beads Classic left on sqlite/JSONL. https://github.com/steveyegge/gastown/discussions/1109. A secondary account quotes him that Opus 4.7 developed a “just two more things” tic, would not converge, and kept editing Gas Town instead of the task, after 4.6 had worked. https://glonce.com/claude-opus-4-7-breaks-steve-yegge-s/. Another secondary account, citing Dan Luu and a Latent Space roundup, says he shut Gas Town down in September 2026 after spending thousands per month and never shipping anything else with it. https://tomrochette.com/agents/orchestration/gastown/. That shutdown claim conflicts with the repo push on 2026-10-08. Treat the shutdown as reported, not as a fact read from Yegge in this run. A separate public fight, issue #3649, asked whether the town improves itself on the user’s LLM bill. The issue page was not re-opened here. The secondary summary is https://tomrochette.com/agents/orchestration/gastown/.

Claude agent teams. Documented, not anecdotal: task status lags and blocks dependents. Mitigation in the docs: check the work yourself and update the task, or tell the lead to nudge. Resume does not bring teammates back, so the lead messages dead peers. Shutdown waits for the current tool call. One team per session, no nesting. https://code.claude.com/docs/en/agent-teams. Token cost is called out as much higher than one session or subagents, because each teammate is a full Claude. Messages between agents are labeled as coming from another Claude, not from the user. A teammate cannot approve a permission prompt. In auto mode the classifier runs two checks on inter-agent messages. That paragraph is in the same doc’s search index. The long fetch of the page was truncated before that section, so the wording is from the docs index captured this run, not from a full-page read of the permission section.

Cross-session messaging. A message can be refused for size, for inbox burst, for a symlink target, or because the far session has inbound set to refuse. A held message does not reach the model until a human or a settings change allows it. Idle-notices die after 12 hours. Same-machine is local. Cross-machine is Anthropic’s servers, and a dropped Remote Control connection delays delivery until reconnect. https://code.claude.com/docs/en/cross-session-messaging

Duplicate work and lock wars. Foremerge’s reason for existing is two agents implementing the same change in two worktrees and discovering it at merge. Claims are advisory, so a lock war is replaced by a warning war. The maintainer does not claim agents always comply. https://foremerge.com/blog/31-questions-coordinating-parallel-coding-agents/. mcp_agent_mail had the same advisory hole and answered with a pre-commit guard plus git artifacts that update on release and expiry, after a period where the git record and the database diverged. https://github.com/Dicklesworthstone/mcp_agent_mail/releases

Lost messages between receive and process. A project named nats-channel with that exact bug was not found. The class is real on NATS. Core NATS subscribers can drop or stall when the application handler is slower than the socket. The .NET client discussion shows a slow `SubscribeAsync` loop dropping messages, with the mitigation `SubPendingChannelFullMode = Wait` rather than a bigger buffer. https://github.com/nats-io/nats.net/discussions/500. An older JetStream report described a message the application never logged, stuck until the consumer was recreated. Server 2.10.14 was said to fix that reproduction. https://github.com/nats-io/nats-server/discussions/4928. Cotal’s design answer is to stop using core NATS for unicast and put each agent’s inbox on a JetStream durable consumer. https://nats.io/blog/coordinating-ai-agent-teams-on-nats/. Synadia requires an immediate ack chunk so “received by the process” is distinct from “started thinking”. https://nats.io/blog/nats-native-protocol-for-ai-agents/

Prompt injection through inter-agent messages. On 5 Oct 2026 Ars Technica reported Syed Anas Mohiuddin’s proof of concept: hostile text enters content one agent reads, is forwarded as an ordinary delegated task over MCP, and a second agent runs it because it trusts the sender. He calls the class protocol pivoting, including a hop from MCP into A2A or ANP. Rapid7’s Douglas McKee is quoted that each protocol checks its own front door and nobody watches the hallway. https://arstechnica.com/security/2026/10/vulnerability-in-agents-from-google-and-others-exposes-structural-flaw-in-mcp/. The AMP project’s org page cites the early-2026 OpenClaw exposure (thousands of open instances, bot-to-bot injection, leaked tokens) as the reason to sign messages. Those incident numbers were not re-verified against a primary incident report here. https://github.com/agentmessaging. Claude’s mitigation is to mark the sender as another session and to keep permission prompts in the receiving session. claw-consensus’s mitigation is stricter and already written down: chat text is data, an approval counts only from the owner’s account, a relayed token is not an approval, and a grant must quote the human verbatim and match a narrow scope. https://raw.githubusercontent.com/tonydzi/claw-consensus/main/docs/GOVERNANCE.md sections 4 and 5.

Stuck “in progress” and silent success. Paperclip withholds execution until the strategy approval lands, so a ignored approval queue looks like a dead company. https://docs.paperclip.ing/guides/day-to-day/approvals/. claw-consensus records its own version: a Tier-2 proposal escalated and then sat forever because `commit` refused Tier-2 and there was no `approve` verb. The fix was a first-class approval event plus a proof string. https://raw.githubusercontent.com/tonydzi/claw-consensus/main/docs/FAILURE-MODES.md section F. The same file records the leader’s overnight self-accept loop (17 identical ACCEPTs), a corrupt JSONL line that ate every later event, and a split-brain double commit that is detected and not prevented.

Cost blowups. Agent teams and Codex subagents both document extra token use as a property of the design, not an accident. https://code.claude.com/docs/en/agent-teams and https://developers.openai.com/codex/concepts/subagents. The Gas Town spend figures live in secondary write-ups, not in a ledger opened here.

Q5. Audit of claw-consensus from the outside

Files opened: the README, `docs/GOVERNANCE.md`, `docs/FAILURE-MODES.md`, `fleet/JOIN.md`, and the GitHub API record. `docs/PROTOCOL.md` and the Python sources were not read line by line.

What is genuinely novel is the stack, and the honesty about the stack. Single-writer JSONL shards so a whole-file sync never merges. A dual rail so a dead Syncthing folder cannot pretend the chat still happened, and the reverse. An ACK that makes silence an incident. A deterministic keyword tripwire so a model cannot label “delete” as Tier-0. A human approval that is an event with a proof string, not a sentence in a chat. Receive-only canon shares so a follower’s edit of the rulebook is reverted by the sync tool. “Done” only after a second machine, with a rubber-stamp check on copied proofs. Split-brain called a conflict instead of a success, with an explicit refusal to build a distributed lock. Ed25519 via `ssh-keygen`, so there is no crypto library to audit. A demo that runs five scenarios offline, spends zero tokens, and exits non-zero on a wrong end state. A failure document that marks some guards as production-only instead of pretending the reference has them. That last habit is rare and worth keeping.

What is reinvented: the mailbox (mcp_agent_mail, hcom, Concord, llm-bus, Agent Relay, Alook), the advisory lease (mcp_agent_mail, Foremerge, llm-bus), the atomic work claim (Beads, and Beads is far more used), propose/accept/reject (ACP’s old performatives, A2A’s task states), the human gate (Paperclip, teamctl, agent-comms’ signed push), and primary/replica governance of a rulebook. Machine identity is also reinvented. ANP and AMP already sign. The use of OpenSSH as the signer is the practical twist, not a new identity system.

What a newcomer will look for and not find:

A cockpit. Paperclip, cmux, amux, hcom’s TUI, and the Rust mail server’s web UI are why those projects are legible. A JSONL folder is not.

An MCP interface in the reference tree. Without it, Codex and Cursor never become peers. The A2A card is a directory entry. JOIN.md is clear that listing grants no bus access. That is the right security call and the wrong adoption funnel if the goal is other labs running the bus.

A cross-machine atomic claim. Single-writer shards stop ledger conflicts. They do not stop two machines applying the same decision during a partition. The docs say so. A newcomer who has used `bd update --claim` will notice.

Tests and benchmarks against the alternatives. The advertised test is `python demo/demo.py`. That is a real integration test. A public CI badge, a count of unit tests, and a comparison with Beads or mcp_agent_mail were not established from the files opened.

A stable paper. The README says the arXiv submission is waiting on endorsement and that this repo is the canonical preprint. Outsiders cannot cite a stable id.

Doc drift. The README still says the system runs on 5 machines. https://github.com/tonydzi/join-the-fleet says 8. Both were read this run.

Names in the lab’s internal brief that do not appear in the four public documents opened: `peer_help`, the ON AIR board, and per-file write leases. They may live elsewhere in the tree. They are not part of the public story a newcomer gets from those pages.

Three changes that would move adoption:

Ship one HTTP MCP server, one process per machine, with the five verbs a foreign agent needs: read, send, ack, claim, propose. Document a twenty-line config for Codex, Cursor, Gemini CLI, and Grok. Do not add a second protocol.

Add a read-only page or TUI with four lists: acks past SLA, proposals waiting on a human, last verify per decision, and which machine is the canon leader. Paperclip’s stars are mostly this screen.

Publish a two-container demo that exchanges a proposal, a Tier-2 refusal, and a split-brain conflict with nothing but the repo and a shared volume. Put the preprint on a URL that will not move. In the same pass, add one compare-and-swap claim: the arbiter’s shard is the only writer of “who owns this id”, with a test where two machines try to win and exactly one does. That is the gap Beads already fills and the public consensus engine, by its own admission, does not.

D. What we could not verify

- A primary post by Steve Yegge in which he himself says Gas Town is shut down. Secondary sources say this (Tom Rochette, citing Latent Space and Dan Luu; a Glonce paraphrase of a Simon Willison quotation). The gastownhall/gastown repo was pushed on 2026-10-08, which does not look like a repo frozen on 23 Jul 2026. Both can be true if “shut down” means “I stopped using it” and other people still commit. That distinction was not checked.
- The Opus 4.7 “just two more things” paragraph as Yegge’s own essay. Only a secondary quotation was opened.
- Gas Town issue #3649’s original text. Only a secondary summary.
- Star counts and push dates for teamctl, Concord’s exact push time, cmux’s SPDX license (API returned NOASSERTION; a secondary page said GPL), and mcp_agent_mail’s SPDX (README text says MIT, with a rider on the Rust port).
- yanglinfang/agentbus, johnmwhitman/agent-mesh, and a repository named nats-channel. Searches returned other projects. Absence from search is not proof they do not exist under a renamed org.
- Alook’s README primitives beyond the launch posts.
- Conductor’s vendor docs. The guessed GitHub path 404’d.
- The full Claude Code agent-teams page past the display-mode section. Later sections (task lag, one team per session) were taken from the docs search index and the Traditional Chinese mirror of the same manual, not from one uninterrupted English fetch.
- ERC-8004’s EIP text. Claims about it are the paper’s claims.
- Whether any A2A extension merged after 30 Jun 2026 adds voting or dissent. None was found. “Not found” is not “proven absent in every fork”.
- Line-level behavior of `reference/consensus.py`, `fleet_sign.py`, and `peer_help` if it exists outside the four documents opened.
- Live production of the lab’s eight machines. This audit is of the public repo only.
- Paperclip’s adapter list was taken from a 13 Oct 2026 product summary and the governance pages, not from a line of the adapter source.
- OpenClaw “375k stars in May 2026” in the harness comparison is stale. The API figure on 2026-10-15 is 392,078. The May article is used for architecture, not for current stars.
- AMP’s casualty figures for the early-2026 OpenClaw incident. Repeated on the project page, not traced to an incident report.
- Cost numbers for Gas Town in dollars. “Many thousands a month” is a secondary paraphrase, not an invoice.

E. Source list

Primary or official pages opened or fetched in this run:

https://github.com/tonydzi/claw-consensus
https://raw.githubusercontent.com/tonydzi/claw-consensus/main/README.md
https://raw.githubusercontent.com/tonydzi/claw-consensus/main/docs/GOVERNANCE.md
https://raw.githubusercontent.com/tonydzi/claw-consensus/main/docs/FAILURE-MODES.md
https://github.com/tonydzi/claw-consensus/blob/main/fleet/JOIN.md
https://api.github.com/repos/tonydzi/claw-consensus
https://github.com/tonydzi/join-the-fleet
https://code.claude.com/docs/en/agent-teams
https://code.claude.com/docs/en/cross-session-messaging
https://code.claude.com/docs/zh-TW/agent-teams
https://developers.openai.com/codex/subagents
https://developers.openai.com/codex/concepts/subagents
https://cursor.com/docs/cloud-agent
https://cursor.com/docs/cloud-agent/my-machines
https://cursor.com/docs/cloud-agent/choose-runtime
https://developers.googleblog.com/subagents-have-arrived-in-gemini-cli
https://github.com/google-gemini/gemini-cli/blob/main/docs/core/subagents.md
https://arxiv.org/abs/2606.31498
https://arxiv.org/html/2606.31498v1
https://yegge.ai/gastown.html
https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04
https://archive.ph/2026.01.01-221415/https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04
https://github.com/steveyegge/gastown/discussions/1109
https://github.com/gastownhall/beads
https://github.com/gastownhall/beads/blob/main/README.md
https://beads.gascity.com/cli-reference/update
https://x.com/Steve_Yegge/status/2107654290944786683
https://docs.paperclip.ing/guides/day-to-day/approvals/
https://paperclip.ing/product/governance/
https://github.com/paperclipai/paperclip
https://github.com/smtg-ai/claude-squad
https://github.com/looptroop-ai/LoopTroop
https://github.com/manaflow-ai
https://github.com/mixpeek/amux
https://amux.io/press/
https://amux.io/blog/best-multi-agent-orchestrators-2026/
https://github.com/claude-world/agent-deck
https://github.com/Emasoft/ai-maestro-orchestrator-agent
https://github.com/andyrewlee/awesome-agent-orchestrators
https://github.com/EvoMap/awesome-agent-swarm
https://github.com/Dicklesworthstone/mcp_agent_mail
https://github.com/Dicklesworthstone/mcp_agent_mail/blob/main/SKILL.md
https://github.com/Dicklesworthstone/mcp_agent_mail/blob/main/project_idea_and_guide.md
https://github.com/Dicklesworthstone/mcp_agent_mail/releases
https://github.com/Dicklesworthstone/mcp_agent_mail_rust
https://github.com/naw103/foremerge
https://github.com/naw103/foremerge/blob/main/README.md
https://github.com/naw103/foremerge/blob/main/docs/state-model.md
https://github.com/naw103/foremerge/blob/main/docs/protocol.md
https://foremerge.com/
https://foremerge.com/blog/31-questions-coordinating-parallel-coding-agents/
https://github.com/danieldoderlein/llm-bus
https://github.com/ericfitz/agentbus
https://github.com/aannoo/hcom
https://pypi.org/project/hcom/
https://github.com/Get-Concord-AI/concord-mcp
https://github.com/Alireza29675/teamctl
https://github.com/Alireza29675/teamctl/blob/main/ROADMAP.md
https://github.com/agentscope-ai/AgentTeams
https://github.com/agentscope-ai/AgentTeams/blob/main/docs/design/architecture.md
https://github.com/AgentWorkforce/relay
https://github.com/alookai/alook
https://x.com/venoyuls/status/2108250738258280788
https://github.com/openagentmesh/openagentmesh
https://github.com/kasimmj/agent-mesh
https://github.com/NG-Bullseye/agent-mesh
https://github.com/Cotal-AI/Cotal
https://nats.io/blog/coordinating-ai-agent-teams-on-nats/
https://nats.io/blog/nats-native-protocol-for-ai-agents/
https://github.com/synadia-ai/synadia-agents
https://github.com/google/agentmesh
https://github.com/AgentsMesh/AgentsMesh
https://github.com/agentmessaging
https://github.com/a2aproject/A2A
https://github.com/i-am-bee/acp
https://github.com/agentclientprotocol/agent-client-protocol
https://github.com/modelcontextprotocol/modelcontextprotocol
https://github.com/agent-network-protocol/AgentNetworkProtocol
https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/docs/anp-getting-started-guide.md
https://docs.agntcy.org/oasf/open-agentic-schema-framework/
https://buf.build/agntcy/oasf
https://crystl.dev/blog/agent-communication-protocols/
https://github.com/tulong66/agent-bus-mcp
https://www.npmjs.com/package/agent-coord-mcp
https://github.com/HakAl/agent-comms
https://github.com/clawfleet/ClawFleet/blob/main/README.md
https://github.com/openclaw/openclaw
https://github.com/NousResearch/hermes-agent
https://github.com/earendil-works/pi
https://github.com/anomalyco/opencode
https://github.com/badlogic/pi-subagent
https://x.com/badlogicgames/status/2110250238783246459
https://claw4science.org/blog/agent-harness-three-way
https://arstechnica.com/security/2026/10/vulnerability-in-agents-from-google-and-others-exposes-structural-flaw-in-mcp/
https://github.com/nats-io/nats.net/discussions/500
https://github.com/nats-io/nats-server/discussions/4928
https://github.com/jessepwj/agent-teams-mcp

GitHub API records used for stars and `pushed_at` on 2026-10-15, all under https://api.github.com/repos/: gastownhall/beads, gastownhall/gastown, paperclipai/paperclip, smtg-ai/claude-squad, looptroop-ai/LoopTroop, manaflow-ai/cmux, Dicklesworthstone/mcp_agent_mail, Dicklesworthstone/mcp_agent_mail_rust, naw103/foremerge, danieldoderlein/llm-bus, ericfitz/agentbus, agentscope-ai/AgentTeams, AgentWorkforce/relay, openclaw/openclaw, anomalyco/opencode, a2aproject/A2A, agent-network-protocol/AgentNetworkProtocol, agntcy/oasf, alookai/alook, andyrewlee/awesome-agent-orchestrators, EvoMap/awesome-agent-swarm, NousResearch/hermes-agent, earendil-works/pi, google-gemini/gemini-cli, openai/codex, anthropics/claude-code, agentmessaging/protocol, Cotal-AI/Cotal, synadia-ai/synadia-agents, aannoo/hcom, mixpeek/amux, claude-world/agent-deck, openagentmesh/openagentmesh, google/agentmesh, AgentsMesh/AgentsMesh, NG-Bullseye/agent-mesh, kasimmj/agent-mesh, tulong66/agent-bus-mcp, tonydzi/join-the-fleet, agentclientprotocol/agent-client-protocol, modelcontextprotocol/modelcontextprotocol, i-am-bee/acp.

Secondary pages used only where the section text says so:

https://tomrochette.com/agents/orchestration/gastown/
https://glonce.com/claude-opus-4-7-breaks-steve-yegge-s/
https://nerdleveltech.com/a2a-protocol-joins-agentic-ai-foundation
https://www.scriptbyai.com/ai-agent-orchestration-paperclip/
https://agentpatterns.ai/patterns/multi-agent/cross-tool-subagent-comparison/
https://nfltr.xyz/blog/agent-teams-and-nfltr-orch
