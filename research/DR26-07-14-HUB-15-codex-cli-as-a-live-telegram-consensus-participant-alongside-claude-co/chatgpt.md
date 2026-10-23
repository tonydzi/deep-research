# DR26-07-14-HUB-15 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: chatgpt.com Deep Research (Pro) | topic: DR26-07-14-HUB-15 Codex CLI live consensus chat | date: 2026-07-14 | origin: chatgpt |

# Codex CLI as a Live Participant in a Two Vendor Agent Consensus Chat

## Executive Summary

The current evidence does **not** support making OpenAI Codex CLI a free-form, always-speaking peer in the same human chat loop as Claude Code **as the primary coordination layer**. The evidence **does** support using Codex as a live participant **only if** you separate the system into two planes: a **human-visible chat plane** for summaries, objections, and status, and a **machine-trusted artifact plane** for typed proposals, diffs, verdicts, and approvals. That conclusion follows from three separate bodies of evidence: current Codex product mechanics, current multi-agent research, and agent-security guidance from OpenAI, Google, OWASP, and the UK NCSC. citeturn32view2turn34view1turn29view0turn29view2turn18search1turn19search1turn18search0turn18search5

On **statefulness**, Codex has materially improved since late 2025. `codex exec resume --last` and `codex exec resume <SESSION_ID>` are officially documented for continuing prior non-interactive runs; the Codex SDK supports durable threads that can be resumed by thread ID; and the app-server / MCP interfaces expose `thread/start`, `thread/resume`, `thread/fork`, and account rate-limit APIs. But the CLI still has important automation gaps: programmatic session ID capture is not first-class in the standard headless flow, promptless goal-following is still an open request, and headless forking is still incomplete on the CLI surface. That means Codex is **stateful enough** for supervised automation, but not yet clean enough to make session continuity your only source of truth. citeturn32view2turn35search9turn34view1turn34view0turn33view0turn33view2turn33view3

On **subscription limits**, OpenAI now publishes plan-specific **approximate** five-hour usage ranges by model. As of July 2026, Plus includes about **15–90 local messages per 5h on GPT-5.6 Sol**, **20–110 on Terra**, and **50–280 on Luna**; Pro 5x and Pro 20x scale those ranges up; and local messages plus cloud tasks share the same five-hour window, with additional weekly limits that may also apply. OpenAI also added credits for flexible overage and exposes rate-limit reads and reset-credit mechanics in the app-server protocol. In practice, heavy users still report 429s, “usage limit reached” errors, stale dashboards, and unexpectedly fast depletion, so pacing long-running loops remains operationally necessary even on paid plans. citeturn40view1turn39view0turn31view0turn34view0turn30search1turn30search6turn30search13turn30search14

On **terms and policy**, the picture is mixed. OpenAI’s own Codex docs explicitly say `codex exec` is for scripts and CI, and Business / Enterprise workspaces now have official **Codex access tokens** for “trusted scripts, schedulers, and private CI runners.” But the consumer Terms of Use also prohibit “automatically or programmatically extracting data or Output” and circumventing rate limits. I did **not** find a current official page that explicitly blesses unattended **bot-to-bot orchestration on a consumer Plus / Pro ChatGPT subscription** [no source found]. The strongest defensible reading today is: **Business / Enterprise automation is explicitly supported; consumer unattended automation is still a gray zone with real policy risk**. citeturn11view2turn11view0turn11view1turn7view2

On **whether two AIs talking in chat is worth it**, the answer is nuanced. Recent debate literature increasingly finds that interactive multi-agent debate often underperforms simpler alternatives such as majority voting or isolated self-correction, especially when conformity, peer pressure, identity bias, or problem drift kick in. At the same time, vendor and practitioner evidence points to a narrower pattern that **does** work: a single writer plus an independent reviewer or critic, usually with artifact-based handoff. Anthropic’s research system benefits from parallelism in breadth-first research; Cognition’s current “what actually works” position is much closer to **single-threaded writes plus selective review loops** than to open peer chat. That is exactly the pattern your existing consensus engine already approximates. citeturn15search11turn15search5turn15search22turn15search6turn29view0turn29view2

The practical recommendation is therefore a **Hybrid Signed Artifact Chat** architecture: let Claude Code and Codex speak into Telegram for transparency, but only permit them to influence execution through **signed, typed artifacts** stored on the file bus; give each vendor its own native harness and durable local thread; use Codex native resume opportunistically, not canonically; and, if unattended operation matters, migrate Codex automation to **API-key auth** or **Business / Enterprise access tokens** rather than relying on a personal ChatGPT subscription. This recommendation is **[established]** on the engineering side and **[emerging]** on the product-governance side, with the main assumption that OpenAI’s current consumer-policy ambiguity will persist rather than being clarified in favor of automation. citeturn11view1turn34view1turn25view0turn25view2turn18search1turn19search1

## Key Findings

**[established] Codex now has real cross-invocation continuity, but the clean automation surface is the SDK / app-server, not the bare CLI.** OpenAI documents `codex exec resume --last` and `codex exec resume <SESSION_ID>` for non-interactive continuation, and the Codex SDK exposes resumable thread objects. The experimental MCP / app-server surfaces go further, exposing `thread/start`, `thread/resume`, `thread/fork`, `thread/read`, and account rate-limit reads. However, first-class machine access to the active session ID is still an open gap on the CLI surface, and promptless “follow the existing goal” resume remains a requested feature rather than a completed one. citeturn32view2turn35search9turn34view1turn33view0turn33view2

**[established] The best memory pattern for a live Codex participant is hybrid, not pure native session memory and not pure transcript replay.** Native resume gives the cheapest vendor-native continuity but is opaque and harder to audit externally; replaying raw transcript every turn is simple but scales prompt cost linearly with chat length; a shared state file or signed ledger is far more controllable and compressible, and aligns with OpenAI’s own harness-engineering guidance that the source of truth should live in structured repo artifacts rather than long chat history. For an inter-agent chat, the best pattern is: keep a compact canonical state file on the file bus, persist the Codex thread ID next to it, and use `resume` only as a performance optimization. Exact current Codex model context-window ceilings for subscription-auth CLI use were not found in the official material I reviewed [no source found]. citeturn36search0turn36search1turn35search9turn32view2

**[established] Subscription-auth Codex usage is quota-bound and materially variable by model, task shape, and tool use.** OpenAI’s current pricing docs show approximate Plus windows of **15–90 local messages / 5h on GPT-5.6 Sol**, **20–110 on Terra**, **50–280 on Luna**, with Pro 5x at **75–450 / 100–550 / 250–1400** and Pro 20x at **300–1800 / 400–2200 / 1000–5600** for those same models. OpenAI also states that local messages and cloud tasks share the same five-hour window and that additional weekly limits may apply. Credits can extend usage after included limits are exhausted, and `/usage` plus `account/rateLimits/read` expose live rate-limit state. citeturn40view1turn40view2turn39view0turn31view0turn34view0turn32view0

**[emerging] For heavy loops, the operational bottleneck is rarely raw request rate alone; it is hidden compute usage, retries, and long-turn reasoning.** OpenAI’s app-server can reject saturated ingress with JSON-RPC `-32001` “Server overloaded; retry later.” Community and GitHub reports also show 429s, retry-after behavior, and “usage limit reached” states even when users believed they had remaining budget [single-source]. That pattern implies you should budget **turn cost**, not just turn count: cap rounds, prefer lighter models for reviewer roles, avoid long log dumps in shared context, and schedule expensive “review” turns separately from conversational turns. citeturn34view2turn30search1turn30search5turn10search6

**[emerging] OpenAI consumer-subscription automation remains a governance gray zone, while workspace automation is clearly supported.** The product docs explicitly advertise `codex exec` for scripts and CI, and Business / Enterprise access-token docs explicitly authorize trusted non-interactive workflows, including CLI and app-server-based automation. But the current consumer Terms of Use prohibit automatically or programmatically extracting output and bypassing rate limits. There is no equivalent first-party consumer-policy page saying “running Codex from another automated agent on Plus / Pro is allowed” [no source found]. That does not prove prohibition, but it means unattended consumer use carries real policy ambiguity. citeturn11view2turn11view0turn11view1turn7view2

**[established] The strongest current prior art favors orchestrator-worker systems and single-writer review loops over open bot-to-bot chat meshes.** Anthropic’s June 2025 research-system post argues multi-agent helps most on breadth-first research tasks, while explicitly noting that coding tasks are usually less parallelizable and harder to coordinate. Cognition’s June 2025 position was “don’t build multi-agents,” and its April 2026 update softened that to a narrower thesis: multi-agent systems work better when **multiple agents contribute intelligence but writes stay single-threaded**. Recent open-source orchestrators reflect the same convergence: AWS’s CAO, Agent Orchestrator, and OpenAI’s Symphony all center supervision, isolation, and structured handoff. citeturn29view0turn29view1turn29view2turn25view0turn25view1turn25view2

**[established] A mixed human + bot group chat is a prompt-injection environment by default.** OWASP still lists prompt injection as the top LLM application risk; the UK NCSC argues prompt injection may not be mitigable in the same way as SQL injection and frames LLM agents as “inherently confusable deputies”; OpenAI’s own 2026 guidance says the goal is to constrain impact, not to perfectly detect attacks; and Google’s agentic-browser work independently converges on the same pattern via isolated action critics and origin bounding. That means you should assume Telegram content is untrusted data, not reliable instruction. citeturn18search0turn18search5turn18search1turn19search1turn19search9

**[established] Telegram gives you attribution metadata, not cryptographic authenticity.** The Bot API exposes fields such as `from`, `sender_chat`, `via_bot`, `author_signature`, and `message_thread_id`, which are useful for routing and provenance. But Telegram does not provide a content-signature mechanism that proves a bot message was authored by a specific local agent process and not re-injected or spoofed through another path. If you want machine trust, you need your own signed envelope around every actionable message or artifact. Ed25519 is the clean default, whether you package it directly or through a simple tool like Minisign; if you want tamper-evident auditability across hosts, Sigstore-style transparency logging is the stronger variant. citeturn20view0turn20view1turn20view2turn20view4turn21search0turn21search5turn21search2

**[emerging] “Two AIs talking in public” adds oversight and trust signaling, but not much raw coding intelligence.** The strongest recent research finds that much of the gain attributed to multi-agent debate comes from ensembling or voting, not from the debate itself, and several 2026 papers document conformity, identity bias, and problem drift. The practical value of a visible chat is therefore usually human-facing: watching objections happen, seeing uncertainty surfaced, and leaving an audit trail that a human can inspect. The coding-quality gains come from artifact review and verification, not from the theatrical fact that two models exchanged messages in front of people. citeturn15search11turn15search5turn15search22turn15search6turn13search0

## Contradictions and Debates

The most important contradiction is **OpenAI product surface vs OpenAI consumer terms**. On one side, OpenAI explicitly documents `codex exec` for scripts and CI, offers a Codex SDK, exposes an app-server and MCP control plane, and created Business / Enterprise access tokens precisely for non-interactive trusted automation. On the other side, the current consumer Terms of Use prohibit “automatically or programmatically extracting data or Output.” The pro-automation reading is that OpenAI would not ship a scripting CLI, SDK, and app-server if all automation were forbidden; the stricter reading is that these surfaces are meant for developer workflows, but that unattended consumer-subscription bot loops still fall outside intended usage. The less convincing side, in my judgment, is the claim that there is **no** policy risk for consumer automation; the official text simply does not support that confidence today. citeturn11view2turn11view1turn34view1turn7view2

The second contradiction is **Anthropic vs Cognition on multi-agent systems**. Anthropic’s 2025 research post presents a real and internally measured benefit from parallel subagents in research, including a reported **90.2%** improvement over a single-agent baseline on its internal research eval. Cognition’s 2025 “Don’t Build Multi-Agents” argues that context fragmentation and conflicting implicit decisions make multi-agent systems fragile in production, especially for writing tasks. These positions are not actually incompatible once you separate **research / reading** from **coding / writing**. Anthropic itself notes that coding has fewer truly parallelizable tasks and harder coordination; Cognition’s April 2026 update then moves closer to Anthropic by endorsing narrower forms of multi-agent collaboration where agents contribute analysis but writes remain single-threaded. citeturn29view0turn29view1turn29view2

The third debate is **interactive debate vs isolated self-correction / voting**. The best-cited 2025–2026 literature is now substantially more skeptical of open debate than the 2024 enthusiasm cycle was. “Debate or Vote” finds that majority voting accounts for most of the gain usually attributed to multi-agent debate; “The Cost of Consensus” argues isolated self-correction often wins on the accuracy-cost tradeoff; anonymization work shows identity itself can skew debate outcomes; and problem-drift work shows conversations can literally wander away from the question. The pro-debate side still has credible niches, especially when adaptive stopping is used or when judges are the target task, but the burden of proof has flipped: free-form debate is no longer the default “better” pattern. citeturn15search11turn15search5turn15search19turn15search22turn15search6

The fourth debate is **native session memory vs explicit shared memory**. Native session continuation is attractive because it is cheap and requires less prompt replay. But native thread state is vendor-managed, asymmetric across vendors, and difficult to validate from the outside. Explicit shared memory in files is verbose, but inspectable, diffable, signable, and compatible with your existing consensus engine. OpenAI’s own harness-engineering material strongly favors structured repository artifacts as the actual source of truth, which is a strong argument against letting any chat session become the canonical state of a multi-vendor system. The steelman for native-only memory is simplicity; the arguments against it are auditability, interop, and deterministic recovery. citeturn36search0turn36search1turn35search9turn32view2

## Best Practices

Treat **Telegram as the observation layer, not the execution layer**. Let both agents post short, human-readable updates into the group, but require that any action-relevant content also exists as a **typed artifact** on the synced file bus: proposal JSON, objection JSON, signed review markdown, or patch metadata. OpenAI’s recent harness guidance pushes the same direction: durable instructions, plans, and validation records should live in structured files, not long conversational context. This keeps human trust high without making human chat the thing that drives side effects. citeturn36search0turn36search1turn25view1

Use a **single-writer, dual-reviewer** pattern for coding turns. The cleanest current protocol is: one agent proposes or writes; the other critiques; a verifier checks; only then does anything move to commit or “ready for human approval.” Anthropic’s multi-agent post is strongest on breadth-first exploration, while Cognition’s 2026 production guidance is strongest on multi-agent intelligence with single-threaded writes. Your existing consensus engine is already aligned with what the frontier now looks like. citeturn29view0turn29view2

For **Codex continuity**, persist three things side-by-side in your local ledger: `codex_thread_id`, `artifact_state_hash`, and `last_human_visible_summary`. On each Codex turn, prefer `codex exec resume <SESSION_ID>` if the thread is intact and the same physical worker continues the job. If resume fails, rebuild context from the state file plus the last compact summary instead of replaying the whole Telegram transcript. That design uses native memory when it is available and recovers cleanly when it is not. The Codex SDK or app-server is the cleaner long-term path if you want deterministic thread lifecycle control. citeturn32view2turn35search9turn34view1

For **turn-taking**, do not let the agents improvise a social conversation. Use a rigid envelope such as: `PLAN` → `CRITIQUE` → `REVISION` → `VERDICT`. Each turn should carry a task ID, parent artifact ID, message type, confidence, and explicit stop condition. If the critique is empty or non-material, terminate early. If the critique is material and unresolved after one revision, escalate to human rather than continuing to “debate.” The current literature is clear that prolonged peer interaction is exactly where conformity, drift, and wasted tokens accumulate. citeturn15search11turn15search5turn15search6

For **quota control**, budget per role and per time window. Put Codex reviewer turns on a lighter model tier where acceptable, batch low-value review work into fewer larger turns, and read rate-limit state before launching another loop. OpenAI now exposes both TUI usage commands and app-server rate-limit reads, plus credits and reset-credit mechanics; use them. Long-running chatty consensus loops are the wrong place to spend included subscription usage. citeturn32view0turn34view0turn31view0turn39view0turn40view1

For **message trust**, require a signed machine envelope. Telegram metadata can help route and label messages, but it is not enough for machine trust. The practical scheme is:

- each bot emits a canonical JSON envelope alongside the human text;
- envelope fields include `agent_id`, `vendor`, `host_id`, `task_id`, `phase`, `seq`, `ts`, `body_sha256`, `artifact_paths`, and `parent_ids`;
- sign that envelope with **Ed25519**;
- store the public keys in an append-only local keyring;
- reject any actionable message whose signature, sequence, or allowed sender mapping fails verification. citeturn20view0turn20view1turn21search0turn21search5

For **prompt-injection defense**, assume compromise and bound the blast radius. OpenAI’s own guidance says agents should be designed so that successful manipulation has constrained impact; Google’s agentic-browser guidance adds an isolated action critic and strict origin bounds; OWASP and NCSC both frame injection as a top-tier architectural risk, not a mere prompting bug. Concretely: never let raw Telegram text become executable instructions; separate “chat content” from “approved task spec”; keep network/file permissions minimal; and make all risky operations require the same human gate you already use. citeturn18search1turn19search1turn19search9turn18search0turn18search5

## Failure Modes

The first failure mode is **conversational drift disguised as collaboration**. Two agents appear to be making progress because they are exchanging long, plausible messages, but the actual work product stalls or diverges. The research literature now documents both problem drift and consensus bias in multi-agent debate, which means “continued conversation” should never be used as evidence of real progress. The only trustworthy progress metric is artifact advancement against a known stop predicate. citeturn15search6turn15search5turn15search11

The second failure mode is **parallel implicit decision conflict**. This matters more in coding than in research. If Codex and Claude both write or steer the same change concurrently, they will often make incompatible hidden decisions about style, edge cases, file boundaries, naming, or validation strategy. Cognition’s 2025 post made this the centerpiece of the anti-multi-agent case, and its 2026 update still treats it as the main reason most production multi-agent setups are effectively readonly or advisory around the main writer. citeturn29view1turn29view2

The third failure mode is **session-state brittleness**. If you anchor too much of Codex’s memory in the native session, you inherit session-ID discovery problems, resume edge cases, and possibly different behavior after compaction or restart. The open GitHub issues around session-ID exposure, fork ergonomics, and promptless resume all point in the same direction: Codex continuity is real, but still not frictionless enough to be your only ledger. citeturn33view0turn33view2turn33view3

The fourth failure mode is **quota collapse through hidden work**. The practical danger is not just that the agents talk too much; it is that background subagents, retries, long reasoning paths, or review features spend compute that the human operator does not perceive in real time. Recent community reports and OpenAI incident response around unexpectedly fast Codex depletion make this concrete, though those reports remain partly anecdotal [single-source]. For your design, that means every turn should declare an estimated cost class and every orchestrator should have a hard local budget per loop. citeturn30news35turn30search3turn30search13

The fifth failure mode is **identity spoofing and instruction laundering in the shared chat**. A human or another bot can quote an earlier agent, mention an agent name, or paste an apparently authoritative instruction into the channel. Telegram gives useful provenance metadata, but not a proof that the content came from the expected local agent process. Without a signed envelope and allowlisted sender mapping, cross-agent instruction leakage is one copy-paste away from becoming execution authority. citeturn20view0turn20view1turn20view2

The sixth failure mode is **policy mismatch**. A personal ChatGPT subscription may work technically for long stretches, then become a support or compliance problem because the automation pattern sits in a policy gray area. There are scattered community reports of bans, suspensions, and inexplicable Codex access interruptions, but the evidence is not robust enough to infer a specific enforcement rule [single-source]. The lesson is simpler: if this workflow becomes critical, consumer-subscription auth is the wrong foundation. citeturn10search13turn10search2turn11view1turn7view2

## Strategic Recommendations

There are four viable named options.

**Option Relay Chat** keeps Codex and Claude posting directly into Telegram as peers, with each turn reconstructed from recent chat history plus optional native session resume. The upside is simplicity and strong human visibility. The downside is that chat becomes both memory and control plane, which maximizes injection risk, drift risk, and token waste. This option is attractive for demos and lightweight oversight, but it is the weakest choice for reliability. citeturn18search1turn18search5turn15search6

**Option Hybrid Signed Artifact Chat** uses Telegram only for summaries and objections, while all authoritative machine content moves through signed typed artifacts on Syncthing. Codex keeps its own thread continuity; Claude keeps its own session continuity; the orchestrator only trusts signed artifacts and explicit state files. This option preserves the “live participant” experience humans want while keeping the real system deterministic. It is the strongest fit for your current architecture because it extends, rather than replaces, the consensus engine you already have. citeturn36search0turn36search1turn21search0turn25view0turn25view2

**Option Control Plane Upgrade** replaces child-process CLI control for Codex with the Codex SDK or app-server and, if possible, moves authentication to API key or Business / Enterprise access tokens. Telegram remains a display surface, but Codex lifecycle, rate limits, and threads are managed through a cleaner programmatic interface. This is the best engineering choice for unattended durability, but it increases implementation work and, if you use API keys or workspace seats, moves you from “included subscription” economics toward explicit metering or enterprise governance. citeturn35search9turn34view1turn11view1turn39view0

**Option Visible Only Reviewer Pair** abandons the idea of free-form multi-vendor dialogue almost entirely. Each vendor only emits concise review summaries, objections, and final verdicts into Telegram; all negotiation is encoded as artifact state transitions, not as conversation. This option sacrifices some of the “two AIs in a room” feel but is closest to what both research and production practice currently say is robust. citeturn15search11turn29view2turn29view0

My single recommendation is **Option Hybrid Signed Artifact Chat**. Confidence: **[established]**. Main assumption: your priority is a **watchable** human-visible multi-vendor workflow without giving up the safety and determinism of artifact-based consensus. If that assumption changes and you want unattended 24/7 operation to become first-class, the immediate next move should be **Option Control Plane Upgrade** with official non-interactive credentials, not more clever scripting around a personal subscription. citeturn11view1turn34view1turn25view0turn25view2

## Action Plan

Start by implementing a **message envelope spec** that both agents can produce and verify. Keep it small and boring. Required fields should include: `protocol_version`, `agent_id`, `vendor`, `telegram_bot_id`, `host_id`, `task_id`, `phase`, `artifact_id`, `parent_ids`, `timestamp_utc`, `body_sha256`, `signature`, and `public_key_id`. Publish a pinned keyring artifact to the shared vault and mirror it in each machine’s local allowlist. Until signature verification passes, the message is display-only. This immediately closes the largest identity-spoofing hole. citeturn20view0turn20view1turn21search0turn21search5

Next, add a **Codex continuity ledger**. For each active task, persist: local thread ID, last successful resume timestamp, last summarized state hash, last usage snapshot, and current model tier. Drive Codex with this rule: `resume if same task + same worker + thread healthy; otherwise rebuild from state file`. Do not treat the Telegram transcript as recoverable ground truth. If you later swap in the SDK or app-server, the ledger stays the same and only the executor changes. citeturn32view2turn35search9turn34view1

Then formalize the **turn protocol**. A practical starting point is: `PROPOSE_PLAN`, `COUNTERARGUMENT`, `REVISED_PLAN`, `VERIFY`, `COMMIT_READY`. Only `PROPOSE_PLAN` and `REVISED_PLAN` may introduce new executable intent. `COUNTERARGUMENT` is readonly. `VERIFY` must cite at least one artifact or test result. `COMMIT_READY` is impossible without successful verification and any required human approval. Hard-stop after one revision round unless a human explicitly reopens. That is the shortest path to avoiding sycophant loops and drift. citeturn15search5turn15search6turn29view2

After that, add **quota-aware pacing**. Before launching a Codex turn, read current rate-limit state if available; otherwise use the most recent captured `/status` or `/usage` snapshot. Budget reviewer turns to a lighter model tier where acceptable, reserve Sol-class usage for hard implementation or synthesis tasks, and defer any non-urgent live-chat updates when the remaining five-hour headroom falls below a floor you define. This is how you stay mostly inside included subscription usage instead of discovering the limit only after a 429. citeturn40view1turn40view2turn34view0turn32view0

Finally, make an explicit **credential decision** before you scale. If this remains a personal, supervised workflow where you are often present, you can probably continue prototyping with ChatGPT subscription auth while accepting the policy ambiguity. If you want unattended scheduling, agent-to-agent triggering, or durable compliance posture, move Codex automation to **API key auth** or **Business / Enterprise access tokens**. That is not cosmetic. It is the difference between “works today” and “has an official control plane.” citeturn11view1turn11view0turn39view0turn7view2

## Sources

OpenAI, **Terms of Use**, effective January 1, 2026. citeturn7view0

OpenAI, **Europe Terms of Use**, accessed July 14, 2026. citeturn7view2

OpenAI Learn, **Codex CLI**, accessed July 14, 2026. citeturn11view3

OpenAI Learn, **Non-interactive mode**, accessed July 14, 2026. citeturn11view2

OpenAI Learn, **Authentication**, accessed July 14, 2026. citeturn11view0

OpenAI Learn, **Access tokens**, accessed July 14, 2026. citeturn11view1

OpenAI Learn, **Pricing**, accessed July 14, 2026. citeturn39view0turn40view1turn40view2

OpenAI Help Center, **Using Credits for Flexible Usage in ChatGPT**, updated July 14, 2026. citeturn31view0

OpenAI GitHub, **Codex app-server README**, accessed July 14, 2026. citeturn34view0turn34view2

OpenAI GitHub, **Codex MCP Server Interface**, accessed July 14, 2026. citeturn34view1

OpenAI Learn, **Developer commands**, accessed July 14, 2026. citeturn32view0

OpenAI GitHub issue, **Feature request: expose current Codex session ID programmatically**, January 8, 2026. [single-source] citeturn33view0

OpenAI GitHub issue, **codex exec resume should support promptless follow mode for active goals**, May 22, 2026. [single-source] citeturn33view2

OpenAI GitHub issue, **Add `codex exec fork` subcommand for headless / non-interactive fork**, February 2026. [single-source] citeturn33view3

OpenAI, **Harness engineering: leveraging Codex in an agent-first world**, February 11, 2026. citeturn36search0

OpenAI Developers Blog, **Run long horizon tasks with Codex**, February 23, 2026. citeturn36search1

OpenAI, **An open-source spec for Codex orchestration: Symphony**, April 27, 2026. citeturn25view1

Anthropic Engineering, **How we built our multi-agent research system**, June 13, 2025. citeturn29view0

Cognition, **Don’t Build Multi-Agents**, June 12, 2025. citeturn29view1

Cognition, **Multi-Agents: What’s Actually Working**, April 22, 2026. citeturn29view2

NeurIPS 2025 / OpenReview, **Debate or Vote: Which Yields Better Decisions in Multi-Agent LLMs?**, 2025. citeturn15search0turn15search11

arXiv, **The Cost of Consensus: Isolated Self-Correction Prevails Over Multi-Agent Debate**, April 29, 2026. citeturn15search5

ACL 2026, **When Identity Skews Debate: Anonymization for Bias-Reduced Multi-Agent Reasoning**, 2026. citeturn15search22

EACL 2026, **Stay Focused: Problem Drift in Multi-Agent Debate**, 2026. citeturn15search6

OpenAI, **Designing AI agents to resist prompt injection**, March 11, 2026. citeturn18search1

OWASP, **Top 10 for Large Language Model Applications**, accessed July 14, 2026. citeturn18search0

OWASP GenAI Security Project, **LLM01:2025 Prompt Injection**, accessed July 14, 2026. citeturn18search3

UK NCSC, **Prompt injection is not SQL injection**, December 8, 2025. citeturn18search5

Google Security Blog, **Architecting Security for Agentic Capabilities in Chrome**, December 8, 2025. citeturn19search1turn19search3

Chrome Developers, **Agent security considerations for WebMCP**, June 9, 2026. citeturn19search9

Telegram, **Bot API**, accessed July 14, 2026. citeturn20view0turn20view1turn20view2turn20view4

IETF, **RFC 8032: Edwards-Curve Digital Signature Algorithm**, January 2017. citeturn21search0

Minisign, **A dead simple tool to sign files and verify signatures**, accessed July 14, 2026. citeturn21search5

OpenSSF / Sigstore, **Sigstore**, accessed July 14, 2026. citeturn21search2

AWS Labs, **CLI Agent Orchestrator**, accessed July 14, 2026. citeturn25view0

AgentWrapper, **Agent Orchestrator**, accessed July 14, 2026. citeturn25view2

OpenAI Learn, **Kick off coding tasks from Slack**, accessed July 14, 2026. citeturn26view3

Anthropic, **Introducing Claude Tag**, June 23, 2026. citeturn26view2

Anthropic Docs, **Claude Code in Slack**, accessed July 14, 2026. citeturn26view1

VS Code Blog, **Your Home for Multi-Agent Development**, February 5, 2026. citeturn26view0

OpenAI Developer Community thread, **Codex 429 Too Many Requests**, June 25, 2026. [single-source] citeturn30search1

OpenAI Developer Community thread, **Active ChatGPT Plus account cannot use Codex**, June 6, 2026. [single-source] citeturn10search6

GitHub issue, **improper 429 error near the end of a 5-hour window**, January 13, 2026. [single-source] citeturn30search5

GitHub issue, **5-hour usage limit exhausted right after reset**, July 2026. [single-source] citeturn30search13