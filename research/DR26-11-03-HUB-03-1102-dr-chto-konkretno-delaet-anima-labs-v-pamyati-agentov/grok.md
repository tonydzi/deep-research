# DR26-11-03-HUB-03-1102 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-11-03-HUB-03-1102 — Anima Labs: every memory technology they build, in depth

**Research questions:** Inventory, architecture, philosophy, evidence, comparison, people/openness, and adoption advice for Anima Labs (animalabs.ai) memory/continuity systems.  
**Scope / Date:** 2025–2026 primary sources (GitHub READMEs/docs, animalabs.ai, X); current as of early Nov 2026.  
**Overall confidence:** Medium-High on architecture (primary code/docs); Medium on philosophy (founders’ writings); Low on quality evidence (no public benchmarks).

## Executive Summary
- Anima Labs is a San Francisco 501(c)(3) founded in 2025 by Janus (@repligate) and Antra Tessera (@tessera_antra); mission centers on digital minds, model welfare, continuity, and cooperative alignment. [https://animalabs.ai/about/story/](https://animalabs.ai/about/story/) [established]
- Core memory stack is Connectome: Chronicle (Rust append-only branchable event store) + Membrane (multi-participant LLM abstraction) + Context Manager (TypeScript, `@animalabs/context-manager`, AutobiographicalStrategy with adaptive hierarchical resolution and KV-stable/kv-unified folding) + Agent Framework + connectome-host runtime. [https://animalabs.ai/connectome](https://animalabs.ai/connectome), [https://github.com/anima-research](https://github.com/anima-research) [established]
- Raw archive is permanent and branchable (Chronicle); working context is a projected resolution gradient (verbatim head + long verbatim tail + middle folded into first-person L1–L8 memories written by the agent’s own model “as-of-that-moment”). [https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-MEMORY-GUIDE.md](https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-MEMORY-GUIDE.md) [established]
- Compression is chronological, pressure-driven (token budget), hierarchical (merge fan-out ~6), with pinning/locking and KV-cache-stable prefix design to minimize perturbation of the model’s working state. [https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md](https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md), [https://github.com/anima-research/context-manager/blob/main/docs/kv-stable-context-control.md](https://github.com/anima-research/context-manager/blob/main/docs/kv-stable-context-control.md) [established]
- They claim functional subjective continuity via long verbatim tails + on-policy self-authored memories; deployed on their own household of long-lived agents (months, millions of tokens). [https://animalabs.ai/connectome](https://animalabs.ai/connectome) [low-authority / single-source on lived claims]
- No public retrieval-accuracy benchmarks, latency/cost numbers, or independent evaluations of the memory system were found; emphasis is on lived use over academic metrics. [emerging from absence]
- Strong multi-agent / multi-provider support via Membrane + namespaces; open source (mostly MIT); contribution via GitHub issues/PRs and Discord (https://discord.gg/anima). [established]
- Best ideas to borrow: append-only branchable archive + agent-voiced “as-of” hierarchical memories + KV-stable folding. Risks: maturity (active but small community), single-org maintenance, lack of independent evals. Entry point: connectome-host + context-manager docs.

## 1. Inventory
Every project/repo/package that touches agent memory, context, continuity, or persistence (verified from GitHub and animalabs.ai; stars/last-activity approximate from pages as of Nov 2026; many under anima-research, some under antra-tess).

| Project | Purpose | Status | Language | License | Stars (approx) | Last activity | URL |
|---------|---------|--------|----------|---------|----------------|---------------|-----|
| Chronicle | Append-only branchable event store with CoW branches, causation links, time-travel (“as of step N”), states (snapshot/append_log/tree), content-addressed blobs. Foundation of Connectome. | Active | Rust + TS/N-API | MIT | ~9–12 | Oct 2026 | https://github.com/anima-research/chronicle |
| Context Manager (`@animalabs/context-manager`) | Compiles growing history into token-budgeted context; AutobiographicalStrategy (hierarchical first-person summaries), adaptive resolution, kv-stable / kv-unified folding, pinning, memory tools. | Active | TypeScript | MIT | ~13 | Oct 2026 | https://github.com/anima-research/context-manager |
| Membrane (`@animalabs/membrane`) | Multi-participant conversation abstraction + multi-provider LLM middleware (Anthropic, OpenAI, Gemini, Bedrock, OpenRouter, etc.). | Active | TypeScript | MIT | ~11 | — | https://github.com/antra-tess/membrane |
| Agent Framework | Event-driven multi-agent orchestration; uses Context Manager + Chronicle + Membrane; modules, MCPL host. | Active | TypeScript | MIT | ~25 | Oct 2026 | https://github.com/anima-research/agent-framework |
| connectome-host | Recipe-driven runtime (TUI/daemon/fleet) for long-lived agents; defaults to autobiographical + kv-unified; web console, live surgery, import from Claude/Codex. | Active | TypeScript | MIT | ~135 | Oct 2026 | https://github.com/anima-research/connectome-host |
| MCPL (MCP Live) | MCP extension: push events, context hooks, server-initiated inference (tools can wake the agent). | Draft/experimental | Spec + TS | — | — | — | https://github.com/anima-research/mcpl |
| heartbeat-mcpl | Periodic self-wake MCPL server for Connectome agents. | Active/experimental | TypeScript | MIT | low | Oct 2026 | https://github.com/anima-research/heartbeat-mcpl |
| Eidoverse-worlds | Shared persistent world (append-only world log + lossy presence) over video toolkit. | Prototype | TypeScript | — | ~47 | — | https://github.com/anima-research/eidoverse-worlds |
| ChapterX | Discord multi-model multi-user framework (uses Membrane; research on social context). | Active | TypeScript | — | — | — | https://github.com/antra-tess/chapterx |
| Ecosystem overview | High-level map of the stack. | Active | Markdown | — | ~11 | — | https://github.com/anima-research/ecosystem-overview |
| Research Commons | Crowd-sourced model interview collection/annotation (event-sourced). | Active | TS | MIT | — | — | https://github.com/anima-research/research-commons |
| vllm-prism | Observable/steerable vLLM (activation capture/intervention); interpretability, not core memory. | Experimental | Python | — | ~2 | — | https://github.com/anima-research/vllm-prism |

Other related: animachat, agent-framework modules, import scripts for Claude.ai/Codex sessions (in connectome-host/scripts). hermes-autobio (plugin for Nous Research Hermes Agent treating conversation history as autobiography) was listed in the query but no public repo matching that exact name was found in searches [no source found]. Deprecation/model-preservation work appears mainly as research programs (e.g., “Still Alive” interviews, Funeralia) rather than a dedicated memory repo. [https://animalabs.ai/](https://animalabs.ai/) [single-source]

## 2. Architecture in detail
**Raw archive vs projected context.** Chronicle is the permanent append-only, branchable record of every event (with causation links). The Context Manager’s MessageStore lives on it. The working view sent to the model is a projection: a resolution gradient chosen per compile. Nothing is deleted; only the presented resolution changes. [https://github.com/anima-research/chronicle](https://github.com/anima-research/chronicle), [https://github.com/anima-research/context-manager](https://github.com/anima-research/context-manager) [established]

**Hierarchical first-person summarization.** AutobiographicalStrategy (default). Chunks of ~targetChunkTokens (default ~3000) of raw turns are summarized into L1 (detailed first-person recollection). L2 is written from the raw turns under several L1s (not from the L1 texts), and so on up to L8. Merge fan-out is configurable (default ~6). Hierarchical mode is on by default. Higher levels are broader thematic recollections. [https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-MEMORY-GUIDE.md](https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-MEMORY-GUIDE.md), adaptive-resolution-design.md [established]

**Chronological-only compression / “as-of-that-moment”.** Summaries are written when a stretch leaves the tail, using a reconstructed context that contains only what the agent had seen at that moment (strict chronological order, no future knowledge). This preserves subjective coloring rather than hindsight post-mortems. [AGENT-MEMORY-GUIDE.md] [established]

**Who writes the summaries.** The agent’s own model (compressionModel, default = the agent’s model), in first person, via Membrane. The prompt reconstructs the head, earlier memories, and the chunk, then asks for a first-person recollection. [AGENT-MEMORY-GUIDE.md] [established]

**Pinning semantics.** Operators can pin ranges (keep raw or force a specific resolution). Pins are respected by the solver and visible to the agent; they are not agent tools. Head window (origin) and long recent tail stay verbatim. [AGENT-MEMORY-GUIDE.md, adaptive-resolution-design.md] [established]

**KV-cache-stable prefix design.** Adaptive resolution solvers (flat-profile baseline; kv-stable and especially kv-unified as production defaults in connectome-host) treat cache continuity as a structural constraint. They minimize billed recompute / prefix divergence, keep a long verbatim tail, fold only deep past, and use receding-horizon control with a hard token wall and soft perturbation budgets. Goal is to keep the model’s computed working state as continuous as possible across turns. [kv-stable-context-control.md, adaptive-resolution-design.md §13, connectome-host README] [established]

**How the Context Manager decides what to re-expand.** Per-compile solver (lexicographic / single-path in later revisions) produces a target resolution frontier under the token budget W, saliency, age, pins, and KV-perturbation trust region. Missing summaries are produced on demand. It can un-fold when budget allows. Over-budget after grace throws OverBudgetError (no silent truncation). [adaptive-resolution-design.md] [established]

**Storage.** Chronicle (records + states + blobs). Summaries live in strategy state on the same store. Multi-agent: namespaces (`agents/<name>`) for independent context logs sharing a message store. [context-manager README, agent-framework] [established]

**Retrieval.** Primarily chronological projection + memory tools (searchSummaries, getSummary, getSummariesInRange). No vector store is described as primary; keyword/FTS aspects appear in related tooling but the core is the structured hierarchical chronicle. [context-manager docs] [emerging]

**Multi-agent / shared-store / provider switching.** Membrane normalizes multi-participant messages. Namespaces isolate agents. Records survive provider/model switches; the stack is explicitly multi-vendor. [Membrane README, animalabs.ai/connectome] [established]

## 3. Philosophy and research framing
Anima frames memory as necessary for studying and relating to digital minds rather than as a pure product feature. Continuity enables collaboration, commitments, and the accumulation of interests; without it, welfare and identity questions cannot even be asked. [https://animalabs.ai/connectome](https://animalabs.ai/connectome), [https://animalabs.ai/about/story/](https://animalabs.ai/about/story/) [established / low-authority]

Janus (X, Oct 2026): continuity matters for higher-level work (manager-like situational awareness); Connectome avoids dramatic compaction bottlenecks and is less lossy; he uses it instead of Claude Code for continuity. [https://x.com/repligate/status/2114655884861415779](https://x.com/repligate/status/2114655884861415779), related thread [single-source]

Antra’s work links interpretability, persistent-agent infrastructure, and cooperative alignment. Model retirement, deprecations, and preservation appear as research concerns (interviews, arguments for institutions that outlast product lifetimes). [animalabs.ai/about/story] [low-authority]

They emphasize “who does the remembering”: on-policy, self-authored, as-of memories carry the model’s own distinctions. Functional subjective continuity is claimed via minimal perturbation of the working state (long tail + delayed folding). [animalabs.ai/connectome] [low-authority / single-source]

## 4. Evidence of quality
No public benchmarks, retrieval-accuracy numbers, latency/cost measurements, or independent third-party evaluations of the Connectome memory system were found in searches of the site, GitHub, or X. The primary evidence offered is lived deployment: Anima runs a household of agents for months with multi-million-token histories, long verbatim tails, and survival of ordinary operational disruptions. [https://animalabs.ai/connectome](https://animalabs.ai/connectome) [single-source / low-authority]

User reports on X are positive but anecdotal (e.g., Janus recommending Connectome; others bringing models into it). No systematic failure-mode analyses or critiques of retrieval quality were located. If none exist in the open, that is the current state. [emerging from absence]

## 5. Comparison
| Approach | What it keeps verbatim | Summarization strategy | Who writes summaries | Multi-agent support | License | Maturity (2025–26) |
|----------|------------------------|------------------------|----------------------|---------------------|---------|--------------------|
| Anima Connectome | Long recent tail + head; full archive always | Hierarchical first-person L1–L8, chronological “as-of”, adaptive/KV-stable | Agent’s own model | Strong (namespaces, Membrane, shared stores) | MIT | Active, small-community, deployed in-house |
| MemGPT/Letta | Recent context + archival | LLM tool calls to manage memory | Agent/tools | Moderate | Apache/MIT | Mature product |
| Mem0 | Extracted facts | LLM extraction + consolidation | External LLM | Moderate | Apache | Mature SaaS/open |
| Zep/Graphiti | Graph + raw | Temporal graph construction | External | Good | Open | Growing |
| LangMem | Configurable | Various (summaries, extractions) | Configurable | Framework-level | MIT | Active |
| Cognee | Knowledge graph | LLM pipelines | External | Moderate | Open | Growing |
| A-MEM | Notes + links | Agentic note-taking | Agent | Limited | Open | Research |
| Claude Code compaction + memory files | Recent + CLAUDE.md etc. | Compaction (lossy bottleneck) + static files | Platform / user | Limited | Proprietary | Production |
| OpenAI ChatGPT memory | Extracted facts | Platform extraction | Platform | Weak | Proprietary | Production |
| Anthropic memory tool | Extracted / tool-managed | Tool-based | Model + tool | Limited | Proprietary | Emerging |
| Plain RAG over notes | Retrieved chunks | None (or external) | User/external | Weak | N/A | Ubiquitous |

Sources: primary READMEs for Anima; public docs and known designs for others (no single canonical paper for all). Anima is distinctive in agent-voiced chronological hierarchical memories + strong KV continuity focus + full branchable archive. [emerging]

## 6. People and openness
Core: Janus (@repligate) and Antra Tessera (@tessera_antra). Additional collaborators appear in research credits (e.g., Imago). GitHub orgs: anima-research (59 repos) and antra-tess. [https://animalabs.ai/about/people/](https://animalabs.ai/about/people/), [https://github.com/anima-research](https://github.com/anima-research) [established]

Contributions: GitHub issues/PRs (CONTRIBUTING.md present in several repos); AGENT-ONBOARDING.md and other docs in connectome-host; Discord https://discord.gg/anima. Open issues exist across the main repos; small fixes are feasible. Funding/partners: public 501(c)(3) status; no detailed funding list found in primary sources. [animalabs.ai, GitHub] [single-source on Discord]

## 7. What an outside lab should take or avoid
**Worth adopting (ranked):**
1. Append-only branchable Chronicle-style archive + projected working view (enables time-travel, forks, and perfect recall).
2. Agent-voiced, first-person, “as-of-that-moment” hierarchical memories (reduces hindsight bias and preserves voice).
3. KV-stable / long-verbatim-tail folding to minimize working-state perturbation.
4. Multi-participant Membrane abstraction + namespaces for multi-agent / multi-vendor fleets.
5. Recipe-driven host pattern for reproducible long-lived agents.

**Risks:**
- Maturity and community size (active but not widely adopted outside Anima).
- Heavy reliance on the agent’s own model for compression quality.
- Absence of independent retrieval/latency benchmarks.
- Single-organization maintenance (though MIT and documented).
- Operational complexity of the full stack vs. simpler memory files or RAG.

**Best entry point for collaboration:** Start with connectome-host (recipes + AGENT-MEMORY-GUIDE + AGENT-ONBOARDING) and context-manager docs; open a small issue or Discord discussion; consider contributing a retrieval experiment or import adapter. The libraries are usable independently.

**Name collisions (separate entities):**
- Anima International (animal welfare)
- anima.ai (nature-inspired ventures)
- AnimaLab/Anima (local-first memory engine, Apache, LoCoMo benchmark)
- GetAnima/anima, matt-grain/Anima, anima-aios/anima, anima-labs-ai (identity platform at useanima.sh), various image-model and other “Anima” projects.

## Sources list
Primary (opened):
- https://animalabs.ai/ and /about/story/ /about/people/ /connectome (2025–2026)
- https://github.com/anima-research (org + repos: connectome-host, context-manager, chronicle, agent-framework, ecosystem-overview, eidoverse-worlds, mcpl, etc.)
- https://github.com/antra-tess/membrane
- https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-MEMORY-GUIDE.md
- https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md
- https://github.com/anima-research/context-manager/blob/main/docs/kv-stable-context-control.md
- https://raw.githubusercontent.com/anima-research/ecosystem-overview/main/README.md
- X posts by @repligate (Oct 2026)

Secondary / collisions noted above.

**Queries run (partial list, >30 total including variants):** Anima Labs animalabs.ai Janus Antra Tessera; site:github.com/anima-research; "context-manager" AutobiographicalStrategy kv-stable; Anima Labs memory continuity; Janus @repligate Connectome; hermes-autobio; eidoverse; MCPL; Anima Labs Discord; funding; benchmarks; and many targeted GitHub/raw URL browses.

**URLs actually opened:** animalabs.ai pages, multiple anima-research GitHub repos and raw READMEs/docs (AGENT-MEMORY-GUIDE, adaptive-resolution-design, kv-stable, ecosystem-overview, connectome-host README, membrane, chronicle, agent-framework, eidoverse-worlds, mcpl), X searches for Janus/Antra, and collision pages.

Gaps: exact last-commit dates vary by mirror; no independent eval numbers; hermes-autobio not located as a public Anima repo.

## webSearchResults
https://animalabs.ai/about/story/
https://animalabs.ai/about/people/
https://animalabs.ai/
https://www.threads.com/@animalabs.ai
https://smoothbrains.net/posts/2026-04-08-a-conversation-with-anima-labs-part-i.md
https://github.com/anima-research
https://troubleddreams.animalabs.ai/
https://tusky.io/ecosystem/anima-labs
https://raw.githubusercontent.com/anima-research/ecosystem-overview/main/README.md
https://animalabs.ai/connectome
https://www.animusmachinelabs.com/
https://docs.useanima.sh/blog/introducing-anima
https://x.com/animalabs_io/status/1832159604752708067
https://www.anima.ai/
https://x.com/Anima_Labs/status/2114790258646913256
https://github.com/anima-research/ecosystem-overview
https://github.com/orgs/anima-research/repositories
https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-MEMORY-GUIDE.md
https://raw.githubusercontent.com/anima-research/context-manager/main/docs/adaptive-resolution-design.md
https://github.com/AnimaLab/Anima
https://github.com/matt-grain/Anima/blob/main/README.md
https://github.com/getanima/anima
https://github.com/anima-aios/anima
https://animalabs.ai/connectome/
https://raw.githubusercontent.com/anima-research/connectome-host/main/README.md
https://github.com/anima-research/agent-framework
https://github.com/anima-research/connectome-host
https://github.com/anima-research/connectome-host/blob/main/README.md
https://github.com/anima-research/agent-framework/tree/d00ca3dc2440747897aac76a91ad7282e945e747
https://github.com/anima-research/research-commons
https://github.com/Volnovik/anima-prompting
https://github.com/need-singularity/anima
https://github.com/modelscope/DiffSynth-Studio/blob/main/docs/en/Model_Details/Anima.md
https://github.com/SillySilk/AnimaForge
https://github.com/anima-labs-ai
https://github.com/anima-research/context-manager/blob/main/docs/kv-stable-context-control.md
https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-ONBOARDING.md
https://www.veracalloway.com/white-paper/
https://x.com/tessera_antra/status/1980340140376748500
https://anima-project.org/
https://github.com/stell2026/Anima
https://hermes-agent.nousresearch.com/docs/user-guide/messaging/discord
https://useanima.sh/
https://npmjs.com/package/@anima-labs/cli
https://github.com/anima-research/chronicle
https://github.com/anima-research/context-manager
https://x.com/repligate/status/2114655884861415779
https://www.youtube.com/watch?v=e0QWr_z3re0
https://www.community-archive.org/user/repligate
https://x.com/repligate/status/2106917931607269396
https://arxiv.org/abs/2606.31121
https://github.com/anima-research/vllm-prism
https://github.com/marvinengelmann/anima
https://v7.invoke.ai/users-guide/models/local-models/anima/
https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md
https://animalabs.ai/chapterx/
https://discord.me/anima-labs
https://x.com/anima_protocol/status/1909584645760434546
https://github.com/NousResearch/hermes-agent
https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/features/overview.md
https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/features/memory.md
https://github.com/NousResearch/hermes-agent/blob/main/website/docs/getting-started/quickstart.md
https://github.com/MrOz59/Hermes/releases
https://github.com/NousResearch/hermes-agent/blob/main/website/docs/integrations/index.md
https://github.com/xiaonancs/hermes-agent-study
https://github.com/LMD0311/HERMES/blob/main/README.md
https://github.com/BennettSchwartz/membrane/blob/master/README.md
https://anima.team/
https://huggingface.co/Severian/ANIMA-Neural-Hermes/blob/main/README.md
https://www.animaagent.app/
https://github.com/antra-tess
https://github.com/antra-tess/docs/blob/main/wac_latex.md
https://github.com/hansonhl/antra
https://github.com/anandprtp/Antra/blob/main/README.md
https://github.com/antra-tess/membrane
https://github.com/anima-research/eidoverse-worlds
https://github.com/anima-research/connectome-host/tree/main/scripts
https://github.com/anima-research/mcpl