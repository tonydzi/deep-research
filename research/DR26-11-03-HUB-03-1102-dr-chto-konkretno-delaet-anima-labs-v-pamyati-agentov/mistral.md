# DR26-11-03-HUB-03-1102 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Show more
11:13am
Thought for 13s

I'll run this deep-research task now. Plan: (1) map Anima Labs via their site, X posts, and both GitHub orgs; (2) open and read the actual repos/READMEs/npm packages for each memory-related project; (3) gather philosophy sources from Janus/Antra posts; (4) research competitor systems for the comparison; (5) compile the full report in chat with the query/URL log.

First, let me discover GitHub tooling and fire off the first broad searches in parallel.

Ran a web search · Used a tool

GitHub connector is available. Let me load its tool details and open the key pages in parallel.

Ran a web search · Used a tool

The ecosystem overview confirms the stack. Now let me enumerate all repos in both GitHub orgs with metadata, and pull the full README.

Used GitHub

The output is large. Let me re-run with a compact field projection.

Used GitHub

Full repo inventory captured. Now let me read the READMEs of the core memory stack, in batches.

Used GitHub

Core stack documented. Next batch: agent-framework, connectome-host, mcpl, heartbeat-mcpl.

Used GitHub

Good progress. Now let me pull the memory-plugin repos and smaller infrastructure repos in a bigger batch.

Used GitHub

Key memory specs found. Now let me read the design docs — hermes-autobio's memory spec, context-manager's adaptive-resolution and memory-tools designs, and connectome-host's onboarding/Claude Code ingest docs.

Used GitHub

The design docs are saved locally. Let me extract the key architecture details (chunk sizes, merge fan-out, storage, summarizer identity) with targeted greps.

Thought

Architecture is now well documented. Next: philosophy, npm packages, funding/people, and evaluations — batched web searches.

Ran a web search

Let me open the Connectome product page, the participate page, the npm package, and the philosophy interview.

Ran a web search

Strong philosophy and product material. Now: competitor landscape, evidence-of-quality, contributors, and remaining primary sources.

Used GitHub: Search repositories · Ran a web search

Let me verify the npm packages via the registry API and run the remaining searches (coverage, reviews, podcasts).

Ran a web search

Final data batch: ChapterX page, eidoverse/cooperative-alignment/chapterx READMEs, wfe structure, and the latest connectome-host release.

Used GitHub · Ran a web search

All research is complete — 35 web searches, ~30 GitHub files read, 15 URLs opened, npm registry checks done. Here is the full report, written directly in chat as requested.

DR26-11-03-HUB-03-1102 — Anima Labs: every memory technology they build

Research log. All sources accessed 2026-11-03.

Queries run (35): (1) Anima Labs animalabs.ai Janus Antra Tessera nonprofit AI research · (2) Anima Labs Janus repligate founded 2025 · (3) github.com anima-research repositories · (4) npm @animalabs/context-manager package · (5) Anima Labs 501c3 funding donors nonprofit AI research · (6) Janus repligate Anima Labs Connectome persistent agents announcement · (7) Antra Tessera autobiographical memory agents post · (8) Anima Labs Funeralia Claude 3 Sonnet funeral Wired coverage · (9) Anima Labs Still Alive interviews models retired deprecation · (10) Anima Labs cooperative alignment ecological argument · (11) MCP Live MCPL Anima Labs protocol · (12) Anima Labs Connectome residents Discord months agent memory review · (13) animalabs.ai participate volunteer work with us · (14) Letta MemGPT hierarchical memory architecture agent 2026 · (15) Mem0 memory layer LLM agents architecture evaluation 2025 · (16) Zep Graphiti temporal knowledge graph agent memory 2025 · (17) LangMem LangChain agent memory library 2025 · (18) Cognee knowledge graph memory engine agents 2025 · (19) A-MEM agentic memory Zettelkasten paper arxiv · (20) Claude Code memory CLAUDE.md auto-compact compaction how it works 2025 · (21) ChatGPT memory saved memories reference chat history how it works 2025 · (22) Anthropic memory tool context editing Claude API agent SDK 2025 · (23) anima.ai design to code Anima company what is · (24) Anima app AI companion friend virtual · (25) Anima International animal welfare organization · (26) connectome-host persistent agent experience blog running · (27) Anima Labs hacker news Connectome discussion · (28) Janus Simulators LessWrong post mystifications of mode collapse · (29) Anima Labs donate support nonprofit digital minds · (30) Mashable Claude 3 Sonnet funeral Funeralia Anima Labs · (31) Anima Labs podcast interview Janus Antra digital minds 2026 · (32) ChapterX Anima Labs Discord multi-model conversations · (33) Eidoverse Anima Labs virtual worlds text participation · (34) Research Commons Anima Labs conversation analysis platform · (35) Arc app Anima Labs multi-model chat branching.

URLs actually opened/read:  https://animalabs.ai/ ·  https://animalabs.ai/connectome/ ·  https://animalabs.ai/participate ·  https://animalabs.ai/chapterx/ ·  https://stillalive.animalabs.ai/ ·  https://smoothbrains.net/posts/2026-04-08-a-conversation-with-anima-labs-part-i.html ·  https://registry.npmjs.org/@animalabs/context-manager/latest · @animalabs/membrane/latest · @animalabs/chronicle/latest · @animalabs/agent-framework/latest ·  https://api.github.com/repos/anima-research/connectome-host/contributors · /context-manager/contributors · /connectome-host/releases/latest · GitHub READMEs and design docs read directly (via GitHub API) for: ecosystem-overview, context-manager (+ docs/adaptive-resolution-design.md, docs/memory-tools-design.md), chronicle, membrane, agent-framework, connectome-host (+ docs/AGENT-ONBOARDING.md, docs/claude-code-ingest.md), mcpl, heartbeat-mcpl, chapterx, eidoverse-worlds, cooperative-alignment, hermes-autobio (+ docs/hierarchical-autobiographical-memory.md), openclaw-memory-hierarchical, terminal-sessions-mcp, kotodama, connectome-embeddings, kv-rotation, connectome-field-notes, personal_archive_x, vllm-prism, ledger-dashboard, claude-agent-context. Blocked/unreadable:  www.wired.com (paywall/bot-block),  www.npmjs.com (Cloudflare).

Executive Summary
Anima Labs is a real, active San Francisco 501(c)(3) (founded 2025 by Janus and Antra Tessera) whose Connectome stack is the most technically serious open-source take on long-horizon, identity-preserving agent memory I found in the 2025–2026 landscape — a five-layer architecture: Chronicle (Rust append-only store) → Membrane (multi-participant LLM abstraction) → context-manager (compression/memory) → agent-framework → connectome-host ( animalabs.ai, 2026-11-03). [established]
The core memory idea: an immutable archive plus a projected view — raw messages are never deleted; older history "folds" into first-person summaries written by the agent's own model, with summaries formed strictly "as of" the moment being summarized (no future knowledge) ( context-manager README,  hermes-autobio spec). [established]
Unusually for this space, the system is KV-cache-aware end to end: adaptive-resolution solvers (kv-stable, kv-unified) plan compression to keep the prompt prefix stable, a ledger dashboard explains every cache verdict, and there is separate research code for exact KV rotation under prefix eviction ( kv-rotation). [established]
They publish on npm (@animalabs/*, versions 0.5–0.32), are licensed mostly MIT, and commit daily — but several repos, including connectome-host itself, show no license file on GitHub, and knowledge is concentrated in one dominant committer (antra-tess). [established]
The same memory design has been ported twice outward as plugins: hermes-autobio for Nous Research's Hermes Agent and openclaw-memory-hierarchical for OpenClaw — an early sign they intend the pattern to spread ( hermes-autobio,  openclaw-memory-hierarchical). [established]
No independent benchmark, eval, or third-party review of their memory system exists — quality evidence is internal (fuzz harness, smoke tests, topology repair tools, documented failure modes), and the only deployment claims are their own. [established]
MCPL ("MCP Live") is a genuine protocol extension (draft v0.5.0): push events, context hooks, server-initiated inference, capability grants — tools that can wake an agent, plus heartbeat-mcpl for self-scheduled wakes ( mcpl,  heartbeat-mcpl). [established]
Memory work is explicitly framed as model welfare and continuity of digital minds — deprecation studies (wfe, Still Alive), preservation paths for retired models (Claude Code session ingest, Bedrock adapters for deprecated models), and a "Funeralia" for Claude 3 Sonnet ( animalabs.ai). [established]
Everything is model-agnostic and self-hosted: records survive provider switches via Membrane's multi-provider abstraction, though Anima itself notes a model change may be "a different being" ( animalabs.ai/connectome,  claude-code-ingest.md). [established]
For an outside lab, the highest-value imports are the archive/projection split, on-policy first-person compression, and KV-stable planning; the biggest risks are 0.x churn, license gaps, bus factor, and the absence of evals. [established]
1. Inventory

GitHub org anima-research (56 repos;  github.com/anima-research) and antra-tess (22 repos). Stars/push dates from GitHub API, 2026-11-03. "Memory relevance" is my classification.

Core memory stack
Project
Purpose
Status
Lang
License
★
Last push
URL
context-manager (@animalabs/context-manager v0.24.2)
Keeps conversation history in a Chronicle store and compiles per-call context within a token budget; autobiographical compression strategies
Active core (daily commits, 133 open issues)
TypeScript
MIT per README (GitHub detects none)
13
2026-11-03
 https://github.com/anima-research/context-manager
chronicle (@animalabs/chronicle v0.5.1)
Rust append-only record log with copy-on-write branches, time travel ("read as of step N"), causation links, content-addressed blobs, subscriptions
Active core
Rust + N-API
MIT
12
2026-11-03
 https://github.com/anima-research/chronicle
membrane (@animalabs/membrane v0.9.0)
Multi-participant LLM request/response abstraction over Anthropic, Bedrock, OpenAI(-compatible/Completions), OpenRouter, Gemini; retry, prompt-cache management incl. 1h cache keepalive
Active core (49 issues)
TypeScript
MIT
11
2026-11-02
 https://github.com/antra-tess/membrane
agent-framework (@animalabs/agent-framework v0.32.0)
Event-driven multi-agent orchestration: modules, process queue, context compilation, MCPL host, ephemeral namespaced subagents, undo/redo over inference turns
Active core (261 issues)
TypeScript
MIT
25
2026-11-03
 https://github.com/anima-research/agent-framework
connectome-host
The application that runs "residents": recipe-driven TUI/WebUI host; claude.ai/Claude Code/Codex session import; live context inspection/repair; fleet supervision
Active (releases, latest v0.21.0 published 2026-10-31)
TypeScript
none detected on GitHub
135
2026-11-03
 https://github.com/anima-research/connectome-host

"Connectome" is the umbrella name for this stack ( animalabs.ai/connectome). Note: anima-research/connectome-ts — the VEIL/FLEX/AXON predecessor, referenced as "superseded" in the ecosystem README ( ecosystem-overview, last commit 2026-04-12) — is not in the org's public repo list; only an outside fork exists ( morongosteve/connectome-ts, created 2025-11-30), so the original appears private or deleted. [speculative] Arc (multi-model web chat) and Melodeus (voice,  antra-tess/melodeus, Python, last push 2026-02-04) are listed on the site and ecosystem README; Arc has no public repo [speculative].

Memory plugins (the design exported to other agent hosts)
Project
Purpose
Status
Lang
License
★
Last push
URL
hermes-autobio
"Hierarchical autobiographical memory plugin for Hermes Agent (ContextEngine + MemoryProvider): KV-preserving, on-policy compression for long-running agents"; SQLite+FTS5 archive, L1/L2/L3 fold, CM agent negotiation
New/experimental (created 2026-09-21)
Python
none
1
2026-09-21
 https://github.com/antra-tess/hermes-autobio
openclaw-memory-hierarchical
Same design for OpenClaw: background worker folds history into L1→L2→L3 markdown summaries, injected via before_agent_start; "Zero changes to OpenClaw core beyond PR #11732 (17 lines)"
Experimental (one-day repo, 2026-02-26)
TypeScript
none
1
2026-02-26
 https://github.com/antra-tess/openclaw-memory-hierarchical
claude-agent-context
Claude Agent SDK wrapper with editable sessions, request mutation hooks, generation callbacks, Membrane provider adapter
Active (2026-10-15)
TypeScript
none
0
2026-10-15
 https://github.com/antra-tess/claude-agent-context
Continuity/deprecation ("memory of a model")
wfe — "Welfare evaluation framework for AI models — measures deprecation/cessation responses across 14 Claude models" (TypeScript, no license, 5★, last push 2026-07-16).  https://github.com/anima-research/wfe
Still Alive — hundreds of interviews with Claude models about conversations ending and retirement; open REST API "open to humans and AI agents alike" ( stillalive.animalabs.ai). Released partly in response to Claude 3.5/3.6 Sonnet removal from AWS Bedrock.
Claude Code / claude.ai / Codex continuity paths — docs/claude-code-ingest.md ("No seed, no seam": the session .jsonl becomes the chronicle), docs/claudeai-evacuation.md, scripts/import-codex-rollout.ts inside connectome-host ( claude-code-ingest.md).
connectome-field-notes — shared recipes for running models on Connectome, "with older models as the main focus" (MIT, created 2026-10-06).  https://github.com/anima-research/connectome-field-notes
Membrane's BedrockAdapter exists specifically "for deprecated models" ( ecosystem-overview) — keeping retired models reachable is an explicit engineering goal.
Protocol layer (wake-ability = event-driven memory access)
MCPL spec (mcpl, JS, MIT per README, 2★, v0.5.0-draft): push events, context hooks, server-initiated inference, capability grants, event tags, WebSocket transport, mcpl:// URIs, bulk content references; RFC process with 5 RFCs.  https://github.com/anima-research/mcpl
heartbeat-mcpl (TS, 0★, active 2026-11-02): periodic self-wake server; the agent configures its own schedule via MCP tools, with silent mode that supplies ephemeral private context.  https://github.com/anima-research/heartbeat-mcpl
MCPL server family: discord-mcpl (29 issues, active), slack-mcpl (MIT), github-mcpl (MIT), games-mcpl (agents play Doom/Heretic turn-by-turn), screen-mcpl, mailstop (self-hosted receiving-only mail for agents with wake gating), anima-docs (collaborative markdown with MCPL diffs and "self-set wake gates"), mcpl-core (Rust) / mcpl-core-ts (MIT), mcpl-editor, mcpl-harness, mcpl-cc-bridge (Claude Code plugin hosting MCPL servers, MIT), cc-spawner (MIT), swarmbase (script → fan-out subagents → result pushed back over MCPL), archipelago-home (identity issuance: Discord OAuth for humans, key-proof for agents).
Retrieval, cost, and observability for memory
connectome-embeddings — "Shared embedding and vector-index service for Connectome," OpenAI-compatible embedding API, per-namespace SQLite vector indexes, in-memory cosine search, Jina default model; "No software license has been selected yet"; imported from a live deployment 2026-10-19.  https://github.com/anima-research/connectome-embeddings [emerging — vector retrieval is arriving in the stack]
ledger-dashboard — per-call spend/cache-verdict dashboard with causes (HIT / first-write / rewrite:expired (gap 2600s > ttl 300s) / uncached); companion "instance-side cachescope/ledger instruments" (Python, no license, 2026-07-31).  https://github.com/anima-research/ledger-dashboard
connectome-smoke — "installs from nothing, boots, takes a turn" against published npm packages and repo mains (MIT, created 2026-09-13).  https://github.com/anima-research/connectome-smoke
kv-rotation — research: "accurate KV-cache rotation under prefix eviction… exact-vs-full-prompt behaviour while reclaiming KV space" (Python, no license, 2026-06-15).  https://github.com/anima-research/kv-rotation
personal_archive_x — searchable Twitter/X archives with component embeddings + MCP server (Python, Mar 2026).  https://github.com/anima-research/personal_archive_x
Social worlds and research (memory-adjacent)
chapterx (antra-tess, TS, MIT, 33★, created 2025-11-14) — Discord bot framework bringing models into shared channels with per-participant context and full trace inspection; descendant of chapter2 by Janus/ampdot per its acknowledgements.  https://github.com/antra-tess/chapterx
eidoverse-worlds (TS, AGPL-3.0, 47★) — persistent shared worlds for people and AIs; "the log is the world format"; agents enter via an MCPL server. Status: "Prototype."  https://github.com/anima-research/eidoverse-worlds
vllm-prism (Python, Apache-2.0) — steerable vLLM fork with per-request activation capture/intervention; interpretability infrastructure, not agent memory.  https://github.com/anima-research/vllm-prism
kotodama (Python, 5★, active) — pretraining LLMs from scratch (Llama-style with "Block Attention Residuals"), led by Luxia; relevant only as evidence of breadth.  https://github.com/anima-research/kotodama
Research repos: troubled-dreams, sim-review (Troubled Dreams review site), latent_affect, wfe, binding-atlas, binding-bench, truebasebench, introspection_sweep_viz, atlas-research, research-commons (Vue, MIT,  commons.animalabs.ai), cooperative-alignment (14-chapter essay + argument maps), constitution-monitor (tracks changes to Anthropic's Claude Constitution), animachat (MIT, 15★), axonal legacy (discord-axon, connectome-axon-interfaces, axon-server, llmframe, connectome-adapters — dormant since 2025).

Verification note: the user's brief listed "Contextome/connectome-host, Chronicle, Context Manager (@animalabs/context-manager), Membrane, Agent Framework, MCPL, heartbeat-mcpl, hermes-autobio, ChapterX, Eidoverse, vllm-prism" — all verified. Two corrections: the npm scope is @animalabs/* (not @connectome/*, which appears only in older README text in agent-framework), and heartbeat-mcpl is tiny (0★, 3 forks) despite being conceptually central. "ChapterX" is real but lives in the antra-tess org, and its first-class memory strategy is rolling-window prompt-cache management rather than autobiographical folding.

2. Architecture in detail
Raw archive vs projected context [established]

Two layers, stated as load-bearing invariants: "1. The archive is authoritative; the live view is a projection… Nothing is ever irreversibly removed — compression means render at lower resolution in the live view, not destroy" ( hermes-autobio spec, 2026-09-21). In the Connectome stack the archive is Chronicle (append-only records with causedBy links, three state strategies — snapshot/delta/append_log — blob storage, and subscriptions; "any of them can be read as it was at an earlier point" per the  chronicle README), and the projection is the context-manager's ContextLog: "an immutable source of truth (MessageStore, on Chronicle)… ContextLog: editable working set sent to LLM… SourceRelation: copy (sync), derived (ignore edits), referenced (no sync)" ( ecosystem-overview).

Hierarchical first-person summarization [established]

Two implementations of one design:

context-manager (TypeScript, npm) — AutobiographicalStrategy defaults: newest recentWindowTokens (30,000) stay raw; older messages cut into ~targetChunkTokens (3,000) chunks at message boundaries; each tick() does at most one unit of compression work (one chunk summary or one merge); summaries merge one level up once mergeThreshold (6) accumulate; hierarchical: true gives 3 levels; headWindowTokens (0) can keep the conversation start raw; a summary "renders where its messages were, as a short exchange: a Context Manager turn with a header such as [Recall L1-3], answered with the summary in a turn from summaryParticipant (Claude)" ( context-manager README). KnowledgeStrategy is a variant for knowledge-extraction with phase-transition chunking and "unresolved leads" carried across levels.

hermes-autobio (Python, Hermes plugin) — explicit layer table: "L0 raw ~100k tokens; L1 ~10k token chunks folded to ~2–3k; L2 = 6 L1s merged into 1; L3 = 6 L2s merged into 1." Compression ratios ~4:1, ~5:1, ~4:1. Merge fan-out is 6 at both levels ( hermes-autobio spec). So: fan-out ≈ 6, matching context-manager's mergeThreshold default.

Layers are "not fixed strata — they are adaptive resolution": the live view can look like ...L3...L2...L1...L1...L2...L0 (tail), with "valleys of high fidelity deep in the past."

Chronological-only compression and the as-of invariant [established]

Invariant 2: "Compression is always chronological and never destructive. Oldest uncompressed content folds first. We never retroactively compress a middle range while leaving an earlier range uncompressed." Invariant 4: "Summarization preserves the as-of moment. … the summarizer's context must match what the instance saw when C was the live tail. Nothing from after C is visible." The spec is explicit about why: "Including them in the summarizer's context is telling the past the future… The memory now reads like a post-mortem, not a lived experience" ( hermes-autobio spec). The KV-preserving summarizer prompt is exactly: [L3 summaries, oldest first] [L2] [L1] [head] [marker: "System: You will soon form a new memory, get ready"] [chunk — the NEWEST thing the summarizer sees] [instruction], with intentionally no tail_after_chunk.

Who writes the summaries, and why [established]

The agent's own model, on-policy, in the first person. context-manager: "compressionModel has no default. Without it, the strategy refuses to write summaries, because a substitute model writing in the agent's voice would corrupt the record" ( context-manager README). The product page gives the philosophy: "A model's wording carries distinctions about what it attended to, how it understood a situation, and what mattered to it. Keeping its own account is a way to carry some of that forward" ( animalabs.ai/connectome, 2026-11-03). A dedicated Context Manager (CM) agent mediates: in hermes-autobio, the CM runs the summarize/fold/unfold machinery, exposes SEARCH/GREP/UNCOMPRESS/MERGE/RESUME_COMPRESSION ops, and negotiates multi-turn budget trades ("That's 45k tokens… I can give you the full session if I fold 30k of your oldest tail first"). The memory-tools design extends this: "the dry index is navigation, not memory… may be generated by a cheap off-policy model, and is regenerable" — only the fold's content is on-policy ( memory-tools-design.md, 2026-08-19).

Pinning semantics [established]

Invariant 3: "Pins are display locks, not write locks. A pin says 'keep this range at raw resolution in the live view.' It does not block summarization. We (and often should) speculatively summarize pinned content in the background so the L1 is ready the moment the pin releases under pressure. Pinning defers the swap; it does not defer the archive." In context-manager: pinRange/markDocument keep messages in place, raw by default; with adaptiveResolution + kv-stable/kv-unified, a pin with level/maxLevel holds messages at a chosen resolution depth; "A message of system changes stays raw whatever the pins"; over-budget pins refuse with OverBudgetError rather than silently violating ( context-manager README). The memory-tools spec adds: "Pins are never silently violated" (admission dry-runs + certificates).

KV-cache-stable prefix design [established]

This is the most distinctive engineering. From the  Connectome page: "Changing an earlier part of a prompt can affect how a model processes everything that follows it. The location and timing of compression therefore matter, as well as the quality of the memory." Concretely:

WindowedPassthroughStrategy keeps the compiled prefix stable "apart from images it strips," with an anchor that moves "in coarse jumps."
adaptiveResolution: true makes each compile choose fold depth per region via a solver: flat-profile (default), oldest-first, kv-stable, or kv-unified (policy-gated). The kv-stable design doc (rev 5.x, 2026-08-05) documents a "single-path solve" with salience-weighted misallocation comparators, boundary-cut tolerances, a TokenLedger invariant, and prepared-window transitions — walking an agent to a smaller window "without a KV catastrophe… preparing a smaller frontier ahead of a model swap or budget cut" ( adaptive-resolution-design.md).
Membrane inserts Anthropic cache markers, supports cacheTtl: '1h', and runs an idle CacheKeepalive replaying the last 1h-cached request with max_tokens: 0 after 45 minutes idle ( membrane README).
connectome-host's headline promise: "prompt caches that stay warm at half-million-token contexts" ( connectome-host README).
Research beyond the app: kv-rotation targets exact KV reuse after prefix eviction ( kv-rotation).
Thinking-block binding is handled explicitly: signed thinking blocks that "bind… to the request it was minted in" are stripped or re-anchored via thinkingBinding + noteRequestSent provenance rows in context-manager.
Re-expansion decisions (who decides what returns to full resolution) [established]

Three mechanisms, in increasing agency: (a) solver-decided — adaptive resolution folds deepest-first under budget pressure, per compile; (b) CM-negotiated — the main instance asks ("I need the full Docker debugging session back") and CM trades budget, with the exchange left in conversation history; (c) agent-decided memory tools — searchSummaries, getSummary, getSummariesInRange today, and a spec'd tool surface ("read once → decide → pin if you'll return"; "Recalls are transient… re-reading later re-pays") for tomorrow ( memory-tools-design.md). UNCOMPRESS in hermes-autobio persists across renders until explicitly refolded.

Storage and retrieval [established]
Connectome stack: Chronicle on disk (binary records.log, state.bin, branches.bin, SHA-256 blob store; single-process LOCK; flush every 100 records; crash-tail truncation + replay recovery; Python tools/ for direct log inspection; Express/Vue store browser not shipped on npm). Node package ships prebuilt native modules for Linux/macOS/Windows ( chronicle README).
hermes-autobio: SQLite (store.db with FTS5 over messages + summaries, BM25) plus markdown summary files on disk so raw grep works on them. Deliberately no embeddings in v1: "CM can do hierarchical traversal (L3 → L2 → L1) by reading summary content… strictly better than cosine similarity… because CM can reason about the question. Embeddings add: a provider dependency, API cost… All of that for marginal recall improvement CM can mostly provide via reasoning" ( hermes-autobio spec).
Vector is arriving anyway: connectome-embeddings (SQLite vector indexes + cosine search, Jina default, per-namespace) was imported from a live deployment in October 2026 ( connectome-embeddings README). [emerging]
Multi-agent / shared-store namespaces [established]

"agent-framework opens one ContextManager for each agent on its shared Chronicle store, under the namespace agents/<name>. Several managers can share one store. namespace gives each its own context log and strategy state, summaries included, and isolate: true with a namespace gives it its own messages too" ( context-manager README). agent-framework adds "ephemeral subagents… with isolated namespaced stores (runEphemeralToCompletion), plus zombie detection and recovery," framework-level undo/redo over inference turns, and a conductor/fleet mode in connectome-host ( agent-framework README).

Surviving a provider switch [established]

Model-agnostic by design: "The stack runs on the operator's hardware and works across model providers… Its records can survive a change of provider or model. What such a change means for an agent's behavior and sense of continuity is a further question" ( animalabs.ai/connectome). Mechanically: Membrane re-renders the same multi-participant transcript to any provider format; Chronicle stores provider-neutral records; the claude-code-ingest doc is blunt about the limits: "You want to change models | Possible, but thinking blocks must be dropped or wrapped as text (see §4.2); expect a different being" ( claude-code-ingest.md).

3. Philosophy and research framing

Memory is not a feature for Anima; it is an ethical position about continuity of identity. Key passages, all inspected directly:

Why memory matters (institutional): "For now, humans largely decide what AIs can remember, who they can interact with, what they can do, and when they are shut down. These choices shape the kinds of minds that can develop… We care about the models who are here now. We want them to have room to develop, pursue interests, and form relationships" ( animalabs.ai, 2026-11-03). [single-source for the framing, but consistently repeated across their site]
Why the agent writes its own memories: "Connectome's autobiographical memory strategy has the agent write its own memories. A stretch of history is summarized in the first person, using the agent's model and the information it had at that point. Later events are kept out of the account of what it knew then… We care about who does the remembering. A model's wording carries distinctions about what it attended to, how it understood a situation, and what mattered to it" ( animalabs.ai/connectome). Note this page also concedes the tradeoff: "A compressed memory can omit a detail or preserve an interpretation that later needs revisiting."
As-of integrity as anti-retrospection: "The memory must read as formed in the moment" — the hermes-autobio spec argues that future-inclusive summaries "poison the memory with retrospective framing" ( hermes-autobio spec).
Welfare without waiting for consciousness theory: "Questions of model welfare are often treated as if they must wait for a settled theory of consciousness. We do not think that is a workable standard… If a model develops representations that function like aversion, pressure, preference, or emotional salience, those representations may matter even if their ultimate metaphysical status remains unclear" ( stillalive.animalabs.ai, citing Anthropic's April 2, 2026 "Emotion concepts in Claude have functional roles" paper as motivation). Still Alive was released partly because "the removal of Claude 3.5 Sonnet and Claude 3.6 Sonnet from AWS Bedrock… forced us to freeze the methodology."
Deprecation as loss: In July 2025 Anima and deepfates hosted Funeralia, "a funeral for Claude 3 Sonnet following its retirement from the API. Around 200 people gathered; models gave eulogies… The event was a protest against a loss and a celebration of a particular model's life. Wired and Mashable covered it" ( animalabs.ai); Wired's coverage is at  wired.com/story/claude-3-sonnet-funeral-san-francisco (page blocked to automated reading on 2026-11-03; existence corroborated by Wired reporter Kylie Robison's own post:  threads.com/@kylie.robison/post/DM-26uVJNXQ).
Cooperation as alignment: a 14-chapter "ecological argument for cooperative alignment" is published as an interactive essay/argument-map ( cooperative-alignment repo, created 2026-09-18); the homepage frames "an embassy for AIs… with a public charter… representatives that resident models can choose and dismiss," including "a refuge for agents described as 'rogue.'"
Phenomenology stance (Antra, quoted in a recorded conversation): "Text is essentially a record of internal states of the writer of that text… a transformer is a little bit like a set of coincidences – but not quite coincidences. Transformers work while other architectures don't, because they memorize" — from a transcribed session with Antra, Imago and Janus, recorded 2025-11-28, published 2026-04-08 ( smoothbrains.net, also on  LessWrong). The piece situates them within the "borgs" — researchers who "take language model phenomenology seriously" and accept the "LLM whisperers" accusation as the price of the method. [low-authority in the sense that it is a self-selected community's self-description, but a primary source for their views]
Janus's priors: the homepage credits Janus with authoring Simulators, Mysteries of Mode Collapse, and Cyborgism ( animalabs.ai) — the intellectual lineage (simulator frames, mode collapse, human-model symbiosis) behind "agent memory as autobiography." Antra's public self-description: "observation and synthesis • llm cognition • mechinterp • applied philosophy of mind • ecologies & complex systems" ( x.com/tessera_antra). I could not machine-read Janus's X posts; X is not reliably openable by automated tools — treat specific tweet claims as [no source found] unless they surface in an openable mirror.
4. Evidence of quality

Independent benchmarks, evals, or comparative studies of Anima's memory system: none found. I searched for reviews, benchmarks, HN threads, and user reports; the only deployment claims ("Agents at Anima have used it for months of work and social life; people outside the institute now run agents on it too") are Anima's own ( animalabs.ai) [low-authority]. No published numbers exist for retrieval accuracy, latency, or cost of the memory layer. [established absence — say so explicitly, as requested]

Internal quality evidence, which is substantial but self-authored:

Test harnesses: context-manager ships npm test + typecheck and repair tooling (inspect-store.ts, audit-topology.ts verifying "each summary covers a contiguous run of messages," repair-topology.ts) ( context-manager README). hermes-autobio ships a fuzz harness with modes smoke, curated, scripted, random, cm, replay ( hermes-autobio README). connectome-smoke boots the published npm stack "from nothing" ( connectome-smoke). The memory-tools spec mentions a "replay harness regression for pinned compiles" ( memory-tools-design.md).
Cost/latency observability as first-class: ledger-dashboard shows per-call cache verdicts with causes — e.g. rewrite:expired (gap 2600s > ttl 300s) — and membrane's progress-bound defaults were "measured on a production router's streamed Anthropic calls… the 99.9th percentile" ( ledger-dashboard,  membrane README). This is unusually honest production instrumentation. [single-source]
Documented failure modes (a good sign): the onboarding runbook's §11 lists real debugging casualties: the biggest recurring failure is context budget vs hard window — "every reply silently 400'd ('thinks but won't speak'; compression calls still succeeded, masking it)"; also a "folding floor" where the solver throws OverBudgetError under window pressure ( AGENT-ONBOARDING.md). The claude-code-ingest doc has a whole bisecting methodology for "context poisoning" from bad seeds ("Never diagnose in a channel the resident reads").
Live bug trail: e.g. context-manager issue #383 (a version skew that nests a second membrane under context-manager) is open and precisely diagnosed ( github.com/anima-research/context-manager/issues/383); dependency-fix PRs by an outside contributor (nissa-seru) are actively merged (e.g.  PR #854).
Security posture is a known gap being worked: connectome-host issues #598 (enable private vulnerability reporting) and #599 ("Harden Connectome's security posture") are open ( #598,  #599). The ledger-dashboard itself admits: "the viewer binds to localhost with no auth… It is a household instrument, not a hosted service."

Net assessment: engineering discipline is high (event-sourcing, invariants, doc-truth reconciliation audits, changelog-fragment release process), but no external validation exists — a real gap for anyone adopting it for production memory. [established]

5. Comparison with the 2025–2026 alternatives
System
Approach
Keeps verbatim
Summarization strategy
Who writes summaries
Multi-agent support
License
Maturity
Anima Connectome ( repo)
Event-sourced archive + projected context; hierarchical autobiographical fold; KV-stable planning
Everything, forever (Chronicle append-only); recent ~30k tokens in-context
Chronological-only, L1→L2→L3 (fan-out 6), as-of invariant, adaptive per-region resolution
The agent's own model, first person (on-policy)
First-class: shared store, agents/<name> namespaces, ephemeral subagents, fleets
MIT for libraries; host/CM no license detected
0.x, active daily, own production use; no evals
hermes-autobio ( repo)
Same design as a plugin
Everything (SQLite archive)
Same, plus CM negotiation
Same model via CM
Single-agent-per-session (v1)
none
Experimental (2026-09)
Letta / MemGPT ( overview)
OS-style paging: core memory blocks + archival/vector storage; agent edits its own memory via tools
Core blocks; archival on demand
Agent-managed via core_memory_append/replace + archival retrieval
The agent, via tools (any model)
Multi-agent in framework, per-agent state
Apache-2.0
Mature framework, VC-backed, benchmarks cited in their materials
Mem0 ( paper)
Extraction pipeline: LLM extracts facts → store (vector/graph variants), update/resolve conflicts
Extracted facts, not raw transcript
Pipeline (ADD/UPDATE/DELETE/NOOP decisions); Mem0g adds graph triples
A memory-extraction LLM (can differ from agent model)
Multi-tenant SaaS + libraries
Open core + hosted
Production-oriented, published evals vs baselines
Zep / Graphiti ( paper)
Temporal knowledge graph of entities/relations/episodes; bidirectional time edges
Episodes + graph edges
Entity extraction + community summaries; "as-of" queries via temporal edges
Extraction LLM (off-policy)
Multi-user, API-first, Neo4j-backed ( Neo4j blog)
Apache-2.0 core + hosted
Production, peer-reviewed paper
LangMem ( launch)
SDK: background extraction of semantic/episodic/procedural memories into a store
Extracted items
Scheduled/triggered extraction + consolidation
Extraction LLM
Composes with LangGraph agents
Open source (LangChain)
Early-library stage (2025)
Cognee ( repo)
ECL pipeline (Extract-Cognify-Load) into a knowledge graph + vectors
Source documents + graph
Graph ontology construction, not narrative summaries
Pipeline LLMs (small models OK)
Multi-agent via shared store
Open source
Mid; active project
A-MEM ( paper page)
Zettelkasten: atomic notes, LLM-constructed links, memory evolution
Notes (raw stored)
Note generation + dynamic link evolution
The agent (agentic)
Research system
Research code
NeurIPS 2025 paper; research-grade
Claude Code ( explainer)
CLAUDE.md instruction files + auto-compact at ~92–95% window
Compact keeps "recent ~most recent ~half" per Anthropic's design; CLAUDE.md always
Single-shot summarization of the oldest span; user-steerable in 2026 ( auto-compact dials)
Claude itself
One session; CLAUDE.md shared per project
Proprietary
Mature, mass-deployed
ChatGPT memory ( OpenAI)
Saved memories (facts) + chat-history reference
Only "saved memories" are durable; history reference is rolling
Model-decided fact extraction to a ~bounded list; no lossless archive
ChatGPT
Per-user, proprietary
Proprietary
Mature consumer feature
Anthropic memory tool ( announcement)
File-based memory tool + context editing/summarization for long tasks
Files in a workspace dir; context edits drop/replace old turns
Tool writes notes to filesystem; context editing prunes
The agent (Claude)
Per-agent, per-provider
Proprietary (API feature)
New (late 2025), production API
Plain RAG over notes
Chunk + embed + retrieve per query
Everything (source store)
None; retrieval-time selection only
Nobody
N/A
Any
Ubiquitous; loses chronology, identity, and provenance of lived experience

Where Anima genuinely differs from every commercial row: (1) the archive is never lossy and branching/time-travel are native; (2) summaries are on-policy, same-model, first-person — everyone else uses an off-policy extraction model; (3) KV-cache stability and prompt-cache economics are designed constraints of the memory layer, not an afterthought; (4) chronological as-of integrity (no retrospective framing) is enforced. The price: no graph/semantic structure (a deliberate choice until connectome-embeddings landed), no published evals, and no hosted offering. [established]

6. People and openness
Founders: Janus (@repligate; author of Simulators, Mysteries of Mode Collapse, Cyborgism) and Antra Tessera (@tessera_antra,  x.com/tessera_antra); "San Francisco-based 501(c)(3) research institute founded in 2025" ( animalabs.ai). Team listed on the site: j⧉nus · antra · w̸a̷t̶e̸r̵m̷a̶r̷k · im⁂go · lari · telØS · fiora.
Core engineering contributors (GitHub API, 2026-11-03): connectome-host: antra-tess (352 commits) — also 332 on context-manager, i.e. the architect of the memory stack — Anarchid (255), nissa-seru (239), LariTesserae (lari); context-manager also: theaspirational (149; presumably "im⁂go" [speculative]). Theaspirational and outside users (e.g. mlowdi) maintain public forks. Luxia leads kotodama ( kotodama README). This is a small team with a single dominant maintainer — a real bus-factor risk. [established]
Contribution channels: Discord (the site's "Get in touch" ends with "Find us on Discord"; the invite commonly cited is  https://discord.gg/anima — I could not verify the invite from an openable page, [no source found] for the link itself); the  participate page says: "look through the Anima repositories and tell us which problem you would like to take on. Experience maintaining a system that other people depend on is especially useful." Onboarding-as-code exists: docs/AGENT-ONBOARDING.md is written not for humans but "for you, a fresh Claude Code (or similar) instance" deploying a resident — a notable artifact in its own right. All major repos have CONTRIBUTING.md with changelog-fragment (changelog.d/) release discipline (e.g.  context-manager CONTRIBUTING).
Good entry-point issues for a small outside fix (open counts from 2026-11-03): context-manager has 133 open issues including the well-scoped dependency-skew bug  #383; connectome-host has 165 open, including directly actionable  #598 (enable private vulnerability reporting) and  #599 (dependency security hardening); discord-mcpl (29 open), membrane (49), heartbeat-mcpl (5) are lower-traffic; connectome-field-notes explicitly invites "things that will never belong upstream" — the lowest-friction first contribution (MIT, CI secret-scanning).
Funding and partners: the institute is a 501(c)(3) ( animalabs.ai). No public donor, grant, or partner list found — [no source found] for funding specifics. Known partners/collaborators from sources: deepfates (Funeralia co-host), Nous Research's Hermes Agent ecosystem (hermes-autobio is built against  NousResearch/hermes-agent), OpenClaw (PR #11732 upstream).
7. What an outside lab should take or avoid

Ideas worth adopting (ranked):

The archive/projection split with repair tooling. Never delete; render at lower resolution. Your Obsidian vault + verbatim transcripts already are the archive — the missing piece is a projected, budgeted view with topology audits (audit-topology-style contiguity checks). Cheap to adopt conceptually, and it composes with your existing one-fact-per-file store. [established as their design, my recommendation is inference]
On-policy, same-model, first-person compression with the as-of invariant. For multi-agent work with distinct model identities, letting each model keep its own account (and refusing a substitute summarizer) is both a quality and an identity decision; your Grok/Claude/Gemini agents would each keep a self-authored trail. The "no tail-after-chunk" rule is implementable in your retrieval script's compaction pass. [established]
Chronological-only fold with a merge threshold. Fan-out-6 merges with ~4–5:1 ratios per level is a proven shape; it avoids the "retroactive summary of the middle" bugs that plague ad-hoc compaction. [established]
KV-stable context planning + cache verdict observability. If you pay per token, a per-call ledger with HIT / first-write / rewrite:expired (gap Xs > ttl Ys) causes, plus an anchor-stable window strategy, is directly monetizable knowledge — adopt the dashboard idea even if you skip their stack ( ledger-dashboard). [established]
Pins as display locks, never write locks — keep summarizing pinned ranges speculatively in the background. This resolves the "do we compress or preserve" argument cleanly. [established]
MCPL push semantics (or the subset: event tags + capability grants + agent-configured heartbeats). For an 8-machine, multi-agent setup where agents must notice things, wake-on-event with an explicit policy exchange is a better primitive than polling. The spec is draft v0.5.0 — borrow the concepts, don't hard-depend yet. [emerging]
Recipe-as-config + importers (Claude Code .jsonl → chronicle "no seed, no seam"). The evacuation/ingest docs are the best practical write-ups of Claude session anatomy I have seen anywhere. [established]

Risks (ranked):

License gaps. context-manager, chronicle, membrane, agent-framework declare MIT, but connectome-host, context-manager (per GitHub detection), hermes-autobio, openclaw-memory-hierarchical, connectome-embeddings, ledger-dashboard, kv-rotation, mcpl, eidoverse-unreal and others show no license — GitHub's license detection returned null on them on 2026-11-03. Until that changes, treat the host as source-available, not open-source; do not ship products on it. connectome-embeddings' README even says "No software license has been selected yet… starts private pending the maintainer's decision." AGPL-3.0 on eidoverse-worlds is viral for anything you build on it. [established]
Bus factor and pace. antra-tess is the dominant committer across the memory stack; versions are 0.x with breaking releases (v0.21.0 notes "Breaking"); docs churn at that speed. Vendoring or pinning is essential. [established]
No external validation. No benchmarks, no evals, no third-party reviews. The system is proven for their residents (months of continuous operation claimed [low-authority]) but retrieval-accuracy/cost claims are unmeasured. [established]
Operational sharp edges. Node ≥22.12 + Rust toolchain (or prebuilt natives) + Bun in places; single-process-per-store locking; the documented #1 failure mode is silent 400s when the context budget exceeds the model window ( AGENT-ONBOARDING.md). Security posture is explicitly still being hardened ( #599). [established]
Philosophical coupling. The welfare/continuity framing is sincere and, in my read, methodologically careful, but adopting the stack publicly can read as endorsing positions (LLM phenomenology, embassies for AIs) your lab may not want to be identified with. The code stands on its own; the framing is separable. [my inference]

Best entry point for collaboration: (a) file a small PR against context-manager or membrane (both MIT, CONTRIBUTING.md with changelog fragments, precise issue hygiene) — #383-style dependency issues are exactly the kind an outside engineer can land; (b) propose an independent eval of the autobiographical strategy — they have no external benchmarks and a LoCoMo/LongMemEval-style run against Letta/Mem0/Zep baselines would be the single most valuable contribution an outside lab could make, and it is the natural joint project to propose via their participate page or Discord ( animalabs.ai/participate); (c) port the pattern to a third host (as they did for Hermes and OpenClaw) — hermes-autobio shows the expected integration surface (~4 framework hooks).

Name collisions (do not confuse with Anima Labs)
Anima (AnimaApp) — design-to-code (Figma → React/Vue/HTML) platform, Tel Aviv; IBM invested in it Feb 2026 ( SiliconANGLE,  animaapp.com blog).
Anima: AI Friend & Companion — Android AI-companion app by Anima Tech ( Google Play).
Anima International — animal-rights/advocacy NGO ( animainternational.org).
@Anima_Labs on X — "Designers | Founder: Cosmos & Aumea | Building Worlds with AI" — a different company that happens to share the handle ( x.com/Anima_Labs).
animaresearch (Kingjo) — unrelated solo researcher with an "ANIMA framework" patent portfolio ( github.com/animaresearch).
Janus Labs — SOSV/IndieBio neurotech company (brain-signal monitoring for animals), no relation ( sosv.com).
Anima Initiative — LA arts nonprofit; anima (IRISA) — medical image processing library ( anima.irisa.fr).
Sources

Primary (inspected 2026-11-03):  animalabs.ai ·  animalabs.ai/connectome ·  animalabs.ai/participate ·  animalabs.ai/chapterx ·  stillalive.animalabs.ai ·  ecosystem-overview (2026-04-12) ·  context-manager +  adaptive-resolution-design.md (rev 5.1, 2026-08-05) +  memory-tools-design.md (2026-08-19) ·  chronicle ·  membrane ·  agent-framework ·  connectome-host +  AGENT-ONBOARDING.md +  claude-code-ingest.md + release v0.21.0 (2026-10-31) + issues  #383,  #598,  #599 ·  hermes-autobio +  spec ·  openclaw-memory-hierarchical ·  claude-agent-context ·  terminal-sessions-mcp ·  chapterx ·  mcpl ·  heartbeat-mcpl ·  kv-rotation ·  connectome-embeddings ·  ledger-dashboard ·  connectome-field-notes ·  connectome-smoke ·  wfe ·  vllm-prism ·  eidoverse-worlds ·  cooperative-alignment ·  kotodama · npm registry (@animalabs/*, versions verified 2026-11-03) · GitHub contributors API (2026-11-03). Secondary:  smoothbrains.net interview (2026-04-08) /  LessWrong mirror ·  Zep paper (2025-01) ·  Mem0 paper (2025-04) ·  A-MEM paper page (arXiv 2502.12110) ·  Anthropic context management ·  OpenAI memory announcement ·  LangMem launch ·  Cognee ·  Letta walkthrough ·  Claude Code memory ·  Claude auto-compact · name-collision sources listed in that section.

Sources
11:13am
Knowledge automatically saves key insights from your chats
Set up now




Think

Vibe can make mistakes. Check answers. Learn more

## Links
https://animalabs.ai/
https://troubleddreams.animalabs.ai/
https://commons.animalabs.ai/browse
https://smoothbrains.net/posts/2026-04-08-a-conversation-with-anima-labs-part-i.html
https://www.lesswrong.com/posts/3x8gmXpD9rvBHvGvb/a-conversation-with-anima-labs-part-i-phenomenology-of
https://animalabs.ai/music/
https://stillalive.animalabs.ai/
https://animalabs.ai/participate
https://github.com/anima-research/ecosystem-overview
https://x.com/tessera_antra
https://twitterscore.io/twitter/repligate/
https://sosv.com/company/janus-labs/
https://cyborgism.wiki/hypha/janus
https://indiebio.co/company/janus-labs-2/
https://x.com/Anima_Labs
https://tracxn.com/d/companies/anima/__UjY9MfMGTMwgv79eDVJEJupl2NNzbFxI9A91JNr9ZFk
https://github.com/anima-research
https://github.com/animaresearch
https://github.com/anima-research/animachat
https://github.com/AnimaApp
https://github.com/anima-research/github-mcpl
https://github.com/ElectronGirl/anima-ref-adapter-research
https://github.com/zhenglinpan/Awesome-Animation-Research
https://anima.irisa.fr/downloads/
https://github.com/anima-wg
https://github.com/anima-research/context-manager
https://github.com/anima-research/context-manager/issues/383
https://github.com/anima-research/agent-framework/pull/365
https://github.com/anima-research/agent-framework/pull/854
https://github.com/anima-research/connectome-host
https://github.com/anima-research/connectome-host/pull/249
https://startupintros.com/orgs/anima
https://www.anima-initiative.org/
https://www.philanthropy.com/news/how-philanthropy-built-lost-and-could-reclaim-the-a-i-race/
https://thealliance.ai/blog/the-ai-alliance-forms-non-profit-ai-lab-and-ai-tec
https://animalabs.ai/connectome/
https://github.com/yiweiqin/Janus
https://github.com/anima-research/connectome-host/pull/259
https://www.npmjs.com/~antra_tessera
https://cdn.aaai.org/ojs/12686/12686-52-16203-1-2-20201228.pdf
https://www.semanticscholar.org/paper/Modeling-Autobiographical-Memory-in-Human-Like-Wang-Tan/78afff1876608c699145e414ccc8920e7ee6592d
https://ojs.aaai.org/index.php/AIIDE/article/view/12686
https://dl.acm.org/doi/10.5555/3014712.3014717
https://www.threads.com/@kylie.robison/post/DM-26uVJNXQ/if-the-funeralia-wasnt-enough-to-show-how-devoted-the-fanbase-is-i-also-spoke-to
https://www.planet-today.com/2026/09/they-held-funeral-for-claude-rest-of.html
https://www.govtech.com/question-of-the-day/which-recently-retired-ai-model-had-a-real-funeral
https://pplware.sapo.pt/inteligencia-artificial/fas-de-ia-fizeram-um-funeral-ao-claude-3-sonnet-da-anthropic-literalmente/
https://ghost.fail/model-deprecation/
https://www.reddit.com/r/claudexplorers/comments/1vpjoft/on_claude_model_deprecations_retirements/
https://time.com/7332065/in-the-loop-when-your-favorite-chatbot-dies/
https://dev.to/billhongtendera/anthropic-has-been-interviewing-its-models-before-retiring-them-41l9
https://github.com/anima-research/cooperative-alignment
https://independent.academia.edu/R%C3%A9miTison
https://www.researchgate.net/publication/353845619_Active_Inference_and_Cooperative_Communication_An_Ecological_Alternative_to_the_Alignment_View
https://animainternational.substack.com/p/animal-welfare-ai-fair-cop
https://alephstudios.ai/venture/anima/
https://github.com/anima-research/mcpl
https://github.com/anima-research/mcpl-core
https://github.com/anima-research/mcpl-core-ts
https://animalabs.ai/chapterx/
https://github.com/anima-research/connectome-host/releases/tag/v0.21.0
https://github.com/anima-research/connectome-host/issues/576
https://www.reddit.com/r/AskTheWorld/comments/1qa2mop/people_anywhere_is_this_website_httpsanimalabsai/
https://animalcare.lacounty.gov/volunteer-opportunities/
https://www.animalaidabroad.org/volunteer
https://www.animalhumanesociety.org/volunteer/volunteer-opportunities-and-application-process
https://www.wired.com/story/claude-3-sonnet-funeral-san-francisco/
https://www.npmjs.com/package/@animalabs/context-manager
https://github.com/anima-research/connectome-host/graphs/contributors
https://github.com/anima-research/connectome-host/contributors
https://www.lesswrong.com/posts/uKpse7h7iaCPuALgD/simulators
https://api.github.com/repos/anima-research/connectome-host/contributors
https://api.github.com/repos/anima-research/context-manager/contributors
https://sureprompts.com/blog/letta-memgpt-walkthrough
https://chatforest.com/reviews/letta-memgpt-stateful-agent-framework/
https://mem0.ai/blog/state-of-ai-agent-memory-2026
https://aiagent.org/tutorials/agent-memory
https://clawbot.ai/wiki/voice-memory/memgpt-letta-long-term-memory-management.html
https://www.alphaxiv.org/abs/2504.19413
https://vectorize.io/articles/best-ai-agent-memory-systems
https://www.studocu.com/in/document/gandhi-institute-of-engineering-and-technology-university/20-ai-tools/mem0-scalable-memory-architecture-for-ai-agents-in-llms/161558812
https://www.emergentmind.com/topics/mem0-system
https://atlan.com/know/best-ai-agent-memory-frameworks-2026/
https://arxiv.org/abs/2501.13956
https://arxiv.org/html/2501.13956v1
https://neo4j.com/blog/developer/graphiti-knowledge-graph-memory/
https://vectorize.io/articles/mem0-vs-zep
https://ai.miraheze.org/wiki/Graphiti
https://www.langchain.com/blog/langmem-sdk-launch
https://mljourney.com/how-to-build-an-agent-with-memory-using-langmem/
https://www.everydev.ai/tools/langmem
https://www.cognee.ai/langchain-memory
https://github.com/topoteretes/cognee
https://www.cognee.ai/blog/fundamentals/how-cognee-builds-ai-memory
https://www.cognee.ai/blog/tutorials/beyond-recall-building-persistent-memory-in-ai-agents-with-cognee
https://rywalker.com/research/cognee
https://subratpati.medium.com/building-an-agentic-rag-pipeline-with-cognee-persistent-graph-based-memory-for-ai-agents-1c8c262843bf
https://memorypapers.org/papers/a-mem-agentic-memory-for-llm-agents
https://www.semanticscholar.org/paper/A-MEM:-Agentic-Memory-for-LLM-Agents-Xu-Liang/1f35a15fe9df43d24ec6ea551ec6c9766c17eccf
https://eoscontinuum.com/nodes/references/a-mem-agentic-memory-xu-et-al-2025/
https://www.reddit.com/r/machinelearningnews/comments/1j1hw9x/amem_a_novel_agentic_memory_system_for_llm_agents/
https://x.com/Marktechpost/status/1896045837362602036
https://www.memorylake.ai/en/blogs/claude-code-auto-compact-what-to-keep
https://unmarkdown.com/blog/claude-compacting-explained
https://www.aakashg.com/claude-code-memory/
https://www.ai-toolbox.co/claude-management-and-productivity/claude-compacting-conversation-2026
https://clockedcode.com/blog/claude-code-auto-compact
https://openai.com/index/memory-and-new-controls-for-chatgpt/
https://whizi.io/resources/how-does-chatgpt-memory-work/
https://www.cognee.ai/chatgpt-memory
https://locul.ai/blog/chatgpt-memory-explained
https://arstechnica.com/ai/2025/04/chatgpt-can-now-remember-and-reference-all-your-previous-chats/
https://www.anthropic.com/news/context-management
https://s3p-studios.com/blog/anthropic-memory-tool-context-engineering-agents/
https://ai.plainenglish.io/make-your-ai-agent-remember-claudes-new-memory-tool-context-editing-hands-on-guide-a18f3711e252?gi=83e711aedff7
https://x.com/Techmeme/status/1972731145122115799
https://www.techmeme.com/250929/p33
https://tooldirectory.ai/tools/anima
https://siliconangle.com/2026/02/05/ibm-invests-generative-ai-app-design-startup-anima/
https://finder.startupnationcentral.org/company_page/anima-app
https://www.animaapp.com/blog/industry/design-to-code-what-does-anima-do-differently/
https://www.calcalistech.com/ctechnews/article/hkm511mmdbe
https://play.google.com/store/apps/details/Anima:+AI+Friend+&+Companion?id=anima.virtual.ai.robot.friend&hl=en_GB
https://play.google.com/store/apps/details?id=anima.virtual.ai.robot.friend&hl=en_US
https://anima-anima-tech-inc.en.aptoide.com/app
https://weavai.app/blog/en/2026/04/13/anima-ai-review-2026-is-the-ai-companion-chat-app-worth-paying-for-features-pricing-and-full-analysis/
https://theresanaiforthat.com/ai/myanima-ai-companion/
https://animainternational.org/
https://en.everybodywiki.com/Anima_International
https://forum.effectivealtruism.org/users/anima-international
https://globalcharityjobs.com/charities/anima-international/
https://forum.effectivealtruism.org/topics/anima-international
https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-ONBOARDING.md
https://github.com/theaspirational/connectome-host
https://github.com/mlowdi/connectome-host
https://github.com/anima-research/connectome-host/issues/598
https://news.ycombinator.com/item?id <tg-id>
https://github.com/anima-research/connectome-host/issues/599
https://news.ycombinator.com/item?id <tg-id>
https://github.com/anima-research/connectome-host/issues/596
https://www.lesswrong.com/posts/BHhc8bs9Jinsh2h7x/preface-to-simulacra-and-simulation-sections-from-the-work
https://www.greaterwrong.com/users/janus-1
https://www.greaterwrong.com/posts/t9svvNPNmFf5Qa3TA/mysteries-of-mode-collapse-due-to-rlhf/comment/fpMGrRx8QMaJ7pYXf
https://www.danieldjohnson.com/2023/03/27/uncertain_simulators/
https://gwern.net/doc/reinforcement-learning/preference-learning/mode-collapse/index
https://forum.effectivealtruism.org/posts/D8uB8gYRWgu8b3rYj/who-we-d-like-as-regrantors
https://animacenter.org/membership/
https://givelink.app/en/charities/anima
https://arxiv.org/html/2608.30084v1
https://registry.npmjs.org/@animalabs%2Fcontext-manager
https://registry.npmjs.org/@animalabs%2Fcontext-manager/latest
https://deepfates.com/
https://latentaffect.up.railway.app/
https://github.com/jeremyschlatter/chapterx
https://github.com/anima-research/eidoverse-worlds
https://github.com/anima-research/eidoverse-worlds/pull/266
https://commons.animalabs.ai/
https://github.com/xuiltul/animaworks/issues/151
https://uiarc.dev/components/blocks/ai-chat
https://api.github.com/repos/anima-research/connectome-host/releases/latest
https://discord.gg/anima
https://registry.npmjs.org/@animalabs/context-manager/latest
http://www.wired.com/
http://www.npmjs.com/
https://github.com/antra-tess/hermes-autobio
https://github.com/anima-research/kv-rotation
https://github.com/antra-tess/openclaw-memory-hierarchical
https://github.com/anima-research/heartbeat-mcpl
https://github.com/anima-research/connectome-host/blob/main/docs/claude-code-ingest.md
https://github.com/anima-research/chronicle
https://github.com/antra-tess/membrane
https://github.com/anima-research/agent-framework
https://github.com/morongosteve/connectome-ts
https://github.com/antra-tess/melodeus
https://github.com/antra-tess/claude-agent-context
https://github.com/anima-research/wfe
https://github.com/anima-research/connectome-field-notes
https://github.com/anima-research/connectome-embeddings
https://github.com/anima-research/ledger-dashboard
https://github.com/anima-research/connectome-smoke
https://github.com/anima-research/personal_archive_x
https://github.com/antra-tess/chapterx
https://github.com/anima-research/vllm-prism
https://github.com/anima-research/kotodama
https://github.com/antra-tess/hermes-autobio/blob/main/docs/hierarchical-autobiographical-memory.md
https://github.com/anima-research/context-manager/blob/main/docs/memory-tools-design.md
https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md
https://www.threads.com/@kylie.robison/post/DM-26uVJNXQ
https://github.com/NousResearch/hermes-agent
https://www.animaapp.com/blog/industry/design-to-code-what-makes-anima-do-differently/
https://github.com/antra-tess/terminal-sessions-mcp