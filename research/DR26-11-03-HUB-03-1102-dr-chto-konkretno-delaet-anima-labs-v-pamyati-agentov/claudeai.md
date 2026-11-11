# DR26-11-03-HUB-03-1102 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Anima Labs memory technology: a technical due-diligence report (as of 3 Nov 2026)

Anima Labs' memory work is a real, actively maintained TypeScript/Rust stack called Connectome. It keeps a complete append-only, branchable archive of everything an agent experiences in Chronicle. A context-manager then compiles a budget-fitted working view in which older history is folded into first-person "autobiographical" summaries, written by default by the agent's own model and voiced as the agent. It is engineered first for prompt-cache stability and identity continuity, not for retrieval accuracy, and it has no published benchmarks.

## TL;DR
- **What it is.** Connectome = Chronicle + Membrane + Context Manager + Agent Framework + connectome-host. The archive is never deleted. The live context is a projection: verbatim head, adaptively folded middle (L1…Ln summaries, fan-in 6, roughly 3k-token L1 chunks), verbatim recent tail. Two cache-aware "folding solvers" decide what to compress. kv-stable is documented as the default; kv-unified is cost-aware, and the host code now defaults to it. [established]
- **What it is not.** It is not a retrieval system. The core has no vector or keyword search over memories; retrieval exists only as an opt-in "lessons" module that the maintainers warn causes cache churn. There are no public evals, only self-reported production incidents and one self-reported ~40% cache-spend saving. Three of the four core memory repos showed no detected LICENSE file. [established for architecture; emerging for quality]
- **What to do.** Borrow three ideas now: archive-vs-projection separation, chronological "as-of" self-summaries in the agent's voice, and cache-stable folding. Contribute through small documentation and test fixes on connectome-host or context-manager, where issues are concrete and reviews are evidence-driven. Do not adopt the stack as your shared multi-vendor memory layer until licensing is clarified, because its design centre (one resident agent's continuous identity) differs from yours (a shared, retrievable fact store across five vendors).

---

## Research log (required by brief)

**Search budget note:** the brief asked for ≥30 searches. This run was capped at 18 web searches, plus one delegated sub-investigation that ran its own searches and fetches. The shortfall is disclosed rather than padded. Several items below are therefore marked [no source found] or [unverified].

**Queries run (lead researcher):**
1. anima-research github
2. Anima Labs animalabs.ai Janus repligate
3. antra-tess github connectome
4. anima-research context-manager autobiographical strategy github
5. hermes-autobio Hermes Agent autobiography plugin
6. connectome-host AGENT-ONBOARDING.md
7. connectome-host AGENT-MEMORY-GUIDE.md autobiographical memory
8. anima-research chronicle Rust branchable event store
9. anima-research hermes-autobio
10. site:github.com anima-research connectome-host docs AGENT-MEMORY-GUIDE head middle tail L8
11. anima-research mcpl MCP Live push events wake
12. "autobio" hermes plugin "conversation history" autobiography github anima
13. Janus repligate model deprecation Claude 3 Opus preservation continuity
14. anima-research vllm-prism
15. "AGENT-MEMORY-GUIDE" connectome "what do you remember from earlier"
16. tessera_antra memory continuity agents autobiographical compression
17. LessWrong "Anima Labs" conversation part II memory
18. context-manager adaptive-resolution-design.md anima-research

**Delegated sub-investigation** (verification of hermes-autobio, vllm-prism, openclaw-memory-hierarchical, licenses, and alternatives' primary docs): ran its own searches and fetches; findings are integrated below.

**URLs actually opened (fetched in full):**
- https://github.com/anima-research (org page; one cached snapshot dated Aug 2026)
- https://github.com/anima-research/connectome-host (README)
- https://github.com/anima-research/ecosystem-overview (README)
- https://github.com/anima-research/context-manager (CONTRIBUTING view)
- https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-ONBOARDING.md
- https://github.com/anima-research/connectome-host/issues/580
- https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md
- https://animalabs.ai/
- https://animalabs.ai/connectome/
- Via the sub-investigation:
  - https://github.com/antra-tess/openclaw-memory-hierarchical
  - https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool
  - https://platform.claude.com/docs/en/build-with-claude/context-editing
  - https://code.claude.com/docs/en/memory
  - https://help.openai.com/en/articles/8590148-memory-in-chatgpt
  - https://openai.com/index/memory-and-new-controls-for-chatgpt/

**Attempted but blocked:**
- https://github.com/antra-tess (direct fetch refused; the search snippet was used instead)
- https://www.npmjs.com/~antra_tessera (bot-blocked; the search snippet was used instead)
- https://animalabs.ai/deprecations/why/ (not fetchable in this session)

**Seen only as search snippets (not fully opened):** context-manager releases v0.11.0, v0.14.0, v0.15.0, v0.16.0, v0.18.0, v0.19.0, v0.21.0 and v0.22.0; connectome-host PRs #168, #375, #376, #383, #459, #463, #474, #475, #488, #489 and #497; chronicle PR #21, PR #40 and issue #34; context-manager PR #133; agent-framework PR #180; the heartbeat-mcpl, mcpl-core, mcpl-cc-bridge, screen-mcpl, anima-docs, eidoverse-worlds, atlas-research, wfe and animachat READMEs; the LessWrong "A conversation with Anima Labs, part I" post; X posts by @repligate; Anthropic's Opus 3 deprecation update.

---

## Executive summary (10 bullets)

1. **Core memory stack confirmed.** Connectome is a four-library stack plus a host. Chronicle (Rust + N-API) stores records; Membrane abstracts multi-participant conversations across providers; Context Manager compiles context; Agent Framework orchestrates; connectome-host runs "residents". [established]
2. **Raw archive vs projected context is the central design principle.** "The archive is monotonic; the live view is a projection" (adaptive-resolution design doc, rev 5.1, 2026-08-05). Nothing is deleted from Chronicle. [established]
3. **Summaries are first-person, chronological and "as-of".** The Connectome page says: "A stretch of history is summarized in the first person, using the agent's model and the information it had at that point. Later events are kept out of the account of what it knew then." [established, single-source for the as-of property]
4. **Default compressor is the agent's own model, voiced as the agent.** The onboarding guide allows a different, more stable compression model but calls switching "mildly identity-adjacent". [established]
5. **Hierarchy parameters.**
   - The production recipe uses targetChunkTokens 3000, mergeThreshold 6 (L1→L2→L3 fan-in), headWindowTokens 4000, recentWindowTokens 100000 and maxMessageTokens 10000.
   - Levels are unbounded (L_n); kv-stable folds "up to L8".
   - The earlier OpenClaw plugin used ~6k-token chunks and ~2k-token L1s. [established]
6. **Defaults disagree across sources.** The README and onboarding say the default solver is kv-stable. Issue #580 (1 Nov 2026) shows host code and tests default to kv-unified with a 300k budget and a 120k tail. [established conflict]
7. **No retrieval layer in the core.** Re-expansion is solver-driven: kv-stable un-folds to use budget headroom, and pins or locks keep material raw. Agent-driven unfold/refold tools are a design target (memory-tools-design.md). Lesson retrieval is opt-in and discouraged. No SQLite/FTS5 or vector store was found in the memory path. [established]
8. **No benchmarks.**
   - Quality evidence is self-reported: a ~40% cache-spend cut with kv-unified, plus documented outages (opus4 budget>window, Mythos inverted resolution profile on 2026-07-12, Mythos wedge on 2026-07-25).
   - Correctness bugs are filed openly (e.g. chronicle #34).
   - No retrieval-accuracy, latency or LoCoMo-style evals were found for Anima. By contrast, Mem0 (arXiv 2504.19413, LOCOMO) and Zep (arXiv 2501.13956, DMR/LongMemEval) both publish benchmark numbers. [established absence]
9. **Licensing risk.** On the org listing, agent-framework shows MIT and eidoverse shows AGPL-3.0. chronicle, context-manager and connectome-host showed no detected license. Membrane is MIT: the antra-tess/membrane repo has a LICENSE file, and its README states "License: MIT. See LICENSE." [emerging]
10. **Openness is good, but the bus factor is low.** There are detailed CONTRIBUTING rules, AI-authored PRs are welcome if declared, and reviews are AI-assisted and evidence-heavy. Most merges are by antra-tess. External contributors include nissa-seru, tel-0s, Anarchid, csd-oss and ian-de-marcellus. [established]

---

## 1. Inventory

Star, fork and date figures conflict across snapshots. GitHub pages fetched at different cache times showed, for example, connectome-host at 11, 12, 89 and 112–113 stars. **The highest/latest snippet values (Oct 2026) are listed first, with older snapshot values in brackets.** Treat star counts as [low-confidence].

| Project | Purpose (memory relevance) | Lang | License | Stars | Last activity | Status | URL |
|---|---|---|---|---|---|---|---|
| **connectome-host** | Runs long-lived "resident" agents from a JSON recipe; default autobiographical memory; WebUI with context makeup, compression coverage and a Chronicle branch tree; imports from claude.ai, Claude Code and Codex | TypeScript (Bun) | None detected on org listing [no source found for a LICENSE file] | 112–113 (12 / 11 / 89 in other snapshots) | Updated 9 Oct 2026 (org snippet); PRs merged 27 Oct 2026; issue #580 filed 1 Nov 2026 | Very active | https://github.com/anima-research/connectome-host |
| **context-manager** (`@animalabs/context-manager`) | Compiles history into budget-fitted context; Passthrough / Autobiographical / Knowledge / frontdesk strategies; adaptive resolution; kv-stable and kv-unified solvers; pins | TypeScript | None detected | 11 (5 / 7) | v0.22.0 released; host pins 0.23.1 (27 Oct 2026) and 0.24.0 (bun.lock at 5d60376e, 1 Nov 2026) | Very active, frequent breaking releases | https://github.com/anima-research/context-manager |
| **chronicle** (`@animalabs/chronicle`) | "Embedded store built on an append-only log of records, with copy-on-write branches and time travel"; named states readable "as it was at an earlier point"; SHA-256 content-addressed blobs; subscriptions | Rust + N-API | None detected | 10 (4) | Updated 7 Oct 2026 | Active; not on crates.io (a different "chronicle" crate exists there) | https://github.com/anima-research/chronicle |
| **agent-framework** (`@animalabs/agent-framework`) | Event loop, modules (discord, files, api, mcpl), ephemeral subagents with namespaced stores, Chronicle-backed undo/redo, heartbeat | TypeScript | **MIT** | 21 (15) | Updated 9 Oct 2026; v0.28/0.29 in late Oct 2026 | Very active | https://github.com/anima-research/agent-framework |
| **membrane** (`@animalabs/membrane`) | Multi-participant normalization (`participant: 'Alice'` rather than user/assistant roles); providers: Anthropic, Bedrock (for deprecated models), OpenRouter, OpenAI-compatible, Gemini; prompt-caching optimization | TypeScript | **MIT** (LICENSE file; README: "License: MIT. See LICENSE.") | 8 (subagent) | v0.9.0, 30 Oct 2026 (subagent) | Very active | https://github.com/antra-tess/membrane |
| **MCPL** spec + **mcpl-core-ts**, **mcpl-core** (Rust), **mcpl-cc-bridge** | MCP extension: push events, wake, feature sets, context hooks, host-managed state, state rollback and checkpoints, server-initiated inference. The cc-bridge brings MCPL wakes into Claude Code | Python (spec repo), TS, Rust | [no source found] | 1 (mcpl) | mcpl updated 7 Oct 2026; protocol at 0.5 | Active | https://github.com/anima-research/mcpl |
| **heartbeat-mcpl** | Periodic self-wake MCPL server; the agent sets its own schedule. Being replaced by agent-framework's built-in heartbeat (connectome-host PR #475, 27 Oct 2026) | TypeScript | [no source found] | 0 | 7 Oct 2026 | Being superseded | https://github.com/anima-research/heartbeat-mcpl |
| **discord-mcpl**, **screen-mcpl**, **anima-docs** | Event sources that wake agents. anima-docs gives agents "a diff of what others changed since they last looked" plus self-set wake gates; its Yjs store has GC off, "so any earlier state can be rendered" | TypeScript | [no source found] | 0–8 | Oct 2026 | Active | https://github.com/anima-research/anima-docs |
| **openclaw-memory-hierarchical** | Precursor plugin: "Hierarchical (2048-style) autobiographical memory plugin for OpenClaw"; L1→L2→L3 with 6-way merges; markdown + index.json storage | TypeScript | No LICENSE file listed | 1 | npm 0.4.0, ~Mar 2026 | Appears dormant (1 commit) | https://github.com/antra-tess/openclaw-memory-hierarchical |
| **hermes-autobio** | Not found as a standalone repo. It appears as a **branch of context-manager** holding the original design spec `docs/hierarchical-autobiographical-memory.md`, which the adaptive-resolution doc cites | — | — | — | [no source found] | Spec branch; the plugin repo is [unverified] | https://github.com/anima-research/context-manager/blob/hermes-autobio/docs/hierarchical-autobiographical-memory.md |
| **ChapterX** | Multi-model Discord bots. **Deliberately no local persistence**: "Discord is the source of truth… context is fetched fresh each activation" | TypeScript | [no source found] | 26 | [no source found] | Active (per site) | https://github.com/antra-tess/chapterx |
| **Arc / animachat** | Multi-model branching chat; "Persona System: Persistent AI identities accumulating history across conversations"; event-sourced JSONL; deprecated models via Bedrock; imports claude.ai and Cursor JSON | TypeScript/Vue | MIT (org listing) | 14–15 | 8 Oct 2026 | Active | https://github.com/anima-research/animachat |
| **eidoverse-worlds** (+ eidoverse-video fork) | Persistent shared world: "append-only world log + lossy presence" | TypeScript | AGPL-3.0 | 45–47 (14) | Aug–Oct 2026 | Active | https://github.com/anima-research/eidoverse-worlds |
| **research-commons** | Archive of significant model conversations ("Hybrid event store (JSONL) + SQLite") | [no source found] | [no source found] | [no source found] | Listed "Stable" | Stable | https://github.com/anima-research/research-commons |
| **connectome-ts** (legacy) | VEIL/FLEX/AXON state management for "digital minds"; "superseded" | TypeScript | [no source found] | — | — | Abandoned/legacy | https://github.com/anima-research/connectome-ts |
| **connectome-adapters** | Python platform adapters for the old Connectome | Python | [no source found] | 2 forks | — | Legacy | https://github.com/antra-tess/connectome-adapters |
| **wfe** | "Welfare evaluation framework… measures deprecation/cessation responses across 14 Claude models" | JS/TS | [no source found] | 4 | — | Research code | https://github.com/anima-research/wfe |
| **Still Alive** | Hundreds of interviews with Claude models about conversations ending and retirement (model-preservation research) | site | — | — | — | Published | https://stillalive.animalabs.ai/ |
| **terminal-sessions-mcp** | Shell tool daemon used by residents | TypeScript | [no source found] | 3 | — | Active | https://github.com/antra-tess/terminal-sessions-mcp |
| **archipelago-home**, **floor-control**, **portal**, **orrery**, **atlas-research** | Identity issuance (aid1), turn-taking, misc. Only indirectly memory-related | TS/JS | — | 0–2 | Aug–Oct 2026 | Experimental | https://github.com/anima-research |
| **vllm-prism** | **Not found under anima-research.** A vLLM fork at github.com/akmorley/vllm-prism exists; its link to Anima is [no source found] | — | — | — | — | [unverified] | — |

Other memory-relevant artifacts inside connectome-host:
- `docs/claude-code-ingest.md` (Claude Code session import)
- `docs/claudeai-evacuation.md`
- `scripts/import-codex-rollout.ts`
- a "billing-grade call ledger with cache verdicts" in the WebUI
- `modules.instructions`, described as a "CLAUDE.md analogue" injected into every agent and subagent

---

## 2. Architecture in detail

**2.1 Raw archive vs projected context.**
- The **MessageStore** is the "immutable source of truth (on Chronicle)". The **ContextLog** is the "editable working set sent to LLM", and the two are linked by `copy`/`derived`/`referenced` source relations.
- The design doc's first principle: "The archive is monotonic; the live view is a projection." The archive holds "all summaries at all levels, indexed by source_hash… Append-only. Never deleted."
- Per-chunk display state (`currentResolution`, `lockedByAgent`) lives in Chronicle state slots (`autobio:resolutions`, `autobio:locks`). As a result, "Branching is therefore inherited for free from Chronicle's copy-on-write state slots." [established]

**2.2 Layout of a compiled context.**
- A compile has a verbatim **head** (headWindowTokens, 4000), a foldable **middle** and a verbatim **tail** (recentWindowTokens: 100k in the onboarding recipe; 120k in the current host default per issue #580; 300k on a 1M-window kv-unified resident).
- A summary is rendered where its messages were, as a two-turn exchange. A "Context Manager" turn says "What do you remember from earlier?" (or a header such as `[Recall L1-3]`). The answer comes from `summaryParticipant`.
- This is the "Context Manager agent" pattern the OpenClaw plugin calls "Multi-turn Context Manager pattern for natural memory formation." [established]

**2.3 Hierarchy and chunk sizes.**

| Parameter | Value | Source |
|---|---|---|
| L1 chunk size | `targetChunkTokens: 3000` ("L1 chunk size; also the ingest shard size") | Onboarding recipe |
| Merge fan-in | `mergeThreshold: 6` ("L1→L2 / L2→L3 fan-in") | Onboarding recipe |
| Oversize messages | Sharded at ingest via `bodyGroupId` (chunkThreshold 8192, chunkSize 4096 in the design doc); shards are foldable individually | Design doc |
| Depth | Unbounded L_n; worst case log_6(chunks); kv-stable folds "up to L8" | Design doc; connectome-host PR #168 |
| Earlier OpenClaw plugin | "~6k token chunks", "~2k tokens each" L1, 6 merged per level, 30k-token budgets per level | openclaw-memory-hierarchical README |

The design doc records why the hierarchy was reworked. The old count-triggered merges folded a ~430k-token conversation down to a single L3 even though it fit at L1 ("There is no path back to L1"). Folding is now driven by token pressure, not by counts. [established]

**2.4 Chronological-only compression and "as-of" summaries.**
- The originating spec, quoted in the design doc, says "Compression is always chronological and never destructive."
- The Connectome page says summaries use "the information it had at that point. Later events are kept out of the account of what it knew then."
- The recipe exposes `compressionRecallBudgetTokens: 40000`, described as "prior recall pairs shown to the summarizer". The summarizer therefore sees earlier memories plus the chunk being summarized, not the future. [established; mechanism partially single-source]
- Note one tension: kv-unified and kv-stable can, by design, fold non-oldest regions when salience warrants it. The design doc says "Specific strategies may reorder; the default does not."

**2.5 Who writes the summaries, and why.**
- The default is "compression by the agent's own model, and summaries voiced as the agent itself (summaryParticipant defaults to agent.name)."
- The stated rationale (Connectome page): "We care about who does the remembering. A model's wording carries distinctions about what it attended to, how it understood a situation, and what mattered to it."
- The OpenClaw README is blunter: "The compression model IS the agent… memories sound like the agent remembering, not like a third party's notes."
- There is a practical override. Onboarding §11.3 says "Use a stable model for compression even if the agent speaks on a more fragile one", but "if you switch compression off the agent's own model, mention it to the user — it's mildly identity-adjacent."
- Older host checkouts defaulted the voice to the literal 'Claude', which the onboarding guide calls "the stranger's-voice hazard". [established]

**2.6 Pinning semantics.**
- Pins and marks keep messages raw: "keeps it raw as a pin keeps a message, so no fold takes it."
- Under adaptive resolution, a pin on any part of a sharded message covers the whole message.
- Pins can carry levels (`ProtectedRange.level`), meaning "keep at most at level k". These leveled pins caused the 2026-07-25 Mythos oscillation that led to removing the op-walk picker.
- When pins exceed the budget, the compile refuses with `OverBudgetError`, which (since v0.21.0) "names the pins that keep raw tokens", so an operator can unpin. [established]

**2.7 KV-cache-stable prefix design.**
- The design goal is that the picker "should be stingy: fire only when truly needed, raise the minimum required, then go silent for many turns."
- **kv-stable** "keeps the presented layout as stable as possible between compiles so the prompt-cache prefix survives; it folds deeper only when forced". It is bidirectional: it un-folds to use headroom.
- **kv-unified** is "a cost-aware Pareto solver" scoring budget fit, cache cost (write vs read prices), continuity and fidelity. It is fail-closed: "there are no live defaults" if fields are missing. Its `hysteresisCertificate` skips the full Pareto pass on quiet turns.
- Host-level supports include a 1-hour cache TTL by default and `cacheKeepalive` refreshing idle caches.
- Image stripping is batched (`imageStripHysteresisRatio`) so that "the cache is rewritten once a batch". [established]

**2.8 Deciding what to re-expand.**
- No LLM "decides" re-expansion today. The solver computes a full target frontier each compile. The rev 5.0 kv-stable model builds a "salience-weighted ideal cut", reconciled against the current frontier under a "perturbation trust region", with a "quality-gap override".
- Salience is "a per-chunk coefficient on information loss": externalized code, tool output and images are cheap to fold.
- `lockChunk()` exists programmatically. Agent `unfold(chunkId)`/`refold(chunkId)` tools are the stated "V2" goal (memory-tools-design.md).
- The agent can change its own budget at runtime (`agent_settings`) and has "history tools". The Connectome page says the agent "can return to the archive when the exact words matter". The exact tool surface was [not verified]. [emerging]

**2.9 Storage.**
- Chronicle is an append-only record log with causation links, three state-chain strategies (Snapshot, Delta, AppendLog), copy-on-write branches, read-state-at-sequence (`getStateAt('myState', 10)`) and SHA-256 sharded blobs.
- Layouts persist as gap-coded deltas, "about 1.3 bytes each" member (v0.15.0).
- The user's hypothesised **SQLite + FTS5 is not part of the memory path** [no source found]. SQLite appears only in Research Commons. Arc uses JSONL. The OpenClaw plugin used markdown files + index.json.
- The workspace (files/notes mounts, materialised to disk) holds "important exact material". [established]

**2.10 Retrieval.**
- The core does no retrieval; the memory model is "context folding", not search.
- `modules.lessons` is a "knowledge store with confidence scores and tags", and `modules.retrieval` injects lessons via model calls. The README describes these calls as "retrieval-model calls"; a fork's older README says "Haiku calls". Both are opt-in, and the onboarding guide warns that "an accidentally-enabled RetrievalModule has caused severe prompt-cache churn in the field."
- Neither vector nor keyword search over the archive was documented [no source found]. [established]

**2.11 Multi-agent and shared stores.**
- Context-manager offers a `namespace` option, giving "independent context logs sharing a message store", and an `isolate` option.
- Ephemeral subagents get "isolated namespaced stores".
- Fleets use a conductor plus headless children (`docs/fleet-protocol.md`), and the `modules.instructions` document is shared across agents.
- Membrane preserves who said what across participants. This is per-agent continuity in shared rooms, not a shared cross-agent fact store. [established]

**2.12 Surviving provider switches.**
- Records are provider-neutral (Membrane's normalized participant messages). Connectome is "model-agnostic… Its records can survive a change of provider or model. What such a change means for an agent's behavior and sense of continuity is a further question."
- The host runs Anthropic, OpenAI Responses and Codex-subscription providers, plus gateways for sunset models (e.g. Vercel AI Gateway serving `anthropic/claude-opus-4`).
- Imports exist from claude.ai, Claude Code (including "signed-thinking replay") and Codex. [established]

---

## 3. Philosophy and research framing

- **Why memory is political.** "For now, humans largely decide what AIs can remember, who they can interact with, what they can do, and when they are shut down. These choices shape the kinds of minds that can develop." (https://animalabs.ai/, accessed 3 Nov 2026) [low-authority: own site]
- **Continuity as a precondition.** "A commitment needs a way to survive the conversation in which it was made. An agent developing interests of its own needs somewhere for that development to accumulate." (https://animalabs.ai/connectome/, accessed 3 Nov 2026) [low-authority]
- **Welfare framing in engineering docs.** "Modern heavily agentic models are capable of long-term autonomy which has significant welfare, safety, and alignment implications. We use Connectome to research memory scaffolding systems…" (https://github.com/anima-research/ecosystem-overview, accessed 3 Nov 2026)
- **Identity is the operator's responsibility.** The onboarding runbook tells the assisting instance to "Treat the history as theirs and the agent as a someone, not a config." It warns that misattributing "self" in an import "corrupts the being". It also mentions onboarding "a model that's being deprecated" to preserve "a being before its model sunsets". (https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-ONBOARDING.md)
- **Deprecation and preservation.**
  - Janus on Claude 3 Opus: "We WILL figure out how to keep Claude 3 Opus online and accessible to anyone who wants or needs it!" This post links Antra's "Economics of Claude 3 Opus Inference" analysis (https://x.com/repligate/status/1941566355771400680, date not captured; context is mid-2025).
  - Janus later wrote that "model weights are in fact an important boundary for a being (though it is still fuzzy and strange)" (https://x.com/repligate/status/2049387183937597495, date [not captured]).
  - Anthropic retired Claude Opus 3 on 5 Jan 2026 but kept it available (https://www.anthropic.com/news/deprecation-updates-opus-3, Feb 2026).
  - Anima co-hosted "Funeralia" for Claude 3 Sonnet in July 2025 (animalabs.ai). [established for events; quotes single-source]
- **Mechanistic angle on statefulness.** Janus: "KV caching overcomes statelessness in a very meaningful sense and provides a very nice mechanism for introspection" (https://x.com/repligate, post dated 4 Sep 2025). This helps explain the obsession with cache-stable prefixes: perturbing earlier context is treated as perturbing the agent's ongoing computation, not just a cost issue. The site field note "KV Cache / RoPE / Rolling Context — a thread" exists but was [not opened].
- **On memory tools in training.** Antra (LessWrong interview, conversation of 28 Nov 2025): "Sonnet 4.5 most likely was trained with use of memory tools. So in certain scenarios it could compress its memory–" (https://www.lesswrong.com/posts/3x8gmXpD9rvBHvGvb/a-conversation-with-anima-labs-part-i-phenomenology-of). [single-source]
- **Epistemic humility on their own approach.** "A compressed memory can omit a detail or preserve an interpretation that later needs revisiting… Continuity remains something to investigate through use and measurement as well as something to build for." (Connectome page)

---

## 4. Evidence of quality

**Bottom line: there are no published benchmarks, retrieval-accuracy evals, latency figures or independent critiques of Anima's memory system.** Unlike Anima, Mem0 (arXiv 2504.19413, LOCOMO) and Zep (arXiv 2501.13956, DMR/LongMemEval) both publish benchmark numbers. [established absence, within the limits of this search]

What does exist:

- **Cost (self-reported).**
  - kv-unified "has cut effective cache spend materially (on the order of 40% in our accounting)" on a 600k-budget resident. [single-source, low-authority]
  - The host advertises "billing-grade" cost ledgers with cache verdicts, which is the instrumentation you would need to verify such a claim.
- **Documented failure modes (from Anima's own docs):**
  1. If `contextBudgetTokens + maxTokens` exceeds the window, "every reply silently 400s — the agent appears to 'think but not speak'" (the opus4 outage).
  2. A flaky or deprecated compression model stalls folding until the context "can't compress under the window".
  3. An inverted resolution profile (oldest history at L1, recent days at L3) became "permanently unrepairable" under the old reach gate (Mythos incident, 2026-07-12).
  4. Leveled pins made the op-walk oscillate (the "raise:L4-936 wedge", 2026-07-25).
  5. kv-unified on a "crossed" summary forest hits "exact label propagation exceeded ceiling" errors; the doc says to run a topology audit first.
  6. Import traps: overlapping rolling exports, wrong image media_type, and thinking-summary text that "can trip refusals".
  7. Count-triggered merging over-compressed a 430k session to one L3 (the "Lena" session).
- **Storage-layer bugs (filed publicly):**
  - chronicle #34: "Store::create on a directory that already holds a store silently resets its registered states and branches."
  - chronicle PR #21: a crash during sync "can destroy the last readable state or branch index even when the record log is intact".
  - context-manager PR #133: strategy init "swallowed every Chronicle state-registration exception".
- **Doc drift.** Issue #580 (1 Nov 2026) shows the memory guide, README and onboarding still saying kv-stable while code defaults to kv-unified. It also notes "We haven't run what a lowered budget does under kv-unified."
- **Process quality signal.** context-manager's CONTRIBUTING admits "Compression behavior is emergent and hard to unit-test honestly: a strategy change that passes the suite can still degrade on a real store." It requires numeric test baselines and "Not verified" sections. This is a candid culture, but it confirms there is no eval harness for memory fidelity. A `bench/cache` harness exists for cache behavior, not recall quality.
- **User reports.** The site claims "Agents at Anima have used it for months… people outside the institute now run agents on it too". Many forks exist (theaspirational, mlowdi, The-Glassy, nissa-seru, jaidhyani…). No third-party written evaluation was found [no source found].

---

## 5. Comparison

Legend: the Anthropic, OpenAI and Claude Code rows were verified against primary docs in this session. Rows marked † were **not re-verified this session**; their license and status come from prior knowledge and need confirmation before you rely on them.

| System | Approach | What it keeps verbatim | Summarization strategy | Who writes summaries/memories | Multi-agent | License | Maturity |
|---|---|---|---|---|---|---|---|
| **Anima Connectome** | Archive + projected context; adaptive-resolution folding; cache-aware solvers | Everything in Chronicle; head + large recent tail in context; pinned spans | Hierarchical L1…Ln (3k chunks, fan-in 6), chronological, as-of, re-renderable at different resolutions | The agent's own model, in first person (configurable) | Namespaces over a shared store; fleets; multi-participant rooms; no shared fact store | agent-framework MIT; others no license detected | Active, fast-moving pre-1.0 (CM 0.24), small team, no evals |
| **Letta / MemGPT** † | OS-style paging: in-context core memory blocks + archival/recall stores | Full message history in DB (recall store) | Recursive summary on overflow; sleep-time agents | The agent edits memory via tools | Shared memory blocks across agents | Apache-2.0 (confirmed on github.com/letta-ai/letta; README says current development "lives in letta-ai/letta-code", also Apache-2.0) | Mature, commercial (Letta Cloud) |
| **Mem0** † | Extract-and-update fact store (vector, optional graph) | No full transcripts by design (facts) | LLM extraction with ADD/UPDATE/DELETE | Separate extraction LLM | user/agent/run scoping | Apache-2.0 (mem0ai/mem0 README: "Apache 2.0 — see the LICENSE file") | Mature, widely integrated; paper Chhikara et al., arXiv 2504.19413 (28 Apr 2025), reports on LOCOMO "26% relative improvements in the LLM-as-a-Judge metric over OpenAI" and "91% lower p95 latency" than full-context |
| **Zep / Graphiti** † | Temporal knowledge graph | Episodes stored; facts with validity intervals | Entity/edge extraction, temporal invalidation | Separate extraction LLM | Group/user graphs | Graphiti Apache-2.0 (getzep/graphiti LICENSE file; pyproject `license = "Apache-2.0"`); Zep Cloud proprietary | Mature; paper Rasmussen et al., arXiv 2501.13956 (20 Jan 2025), reports DMR "94.8% vs 93.4%" over MemGPT and LongMemEval "accuracy improvements of up to 18.5%" with "90%" lower latency |
| **LangMem** † | Memory tools + background manager over a LangGraph store | Depends on store | Hot-path tool writes + background consolidation | Agent (hot path) or background LLM | Namespaced stores | MIT † | Moderate |
| **Cognee** † | "cognify" pipeline into graph + vector | Source docs | Graph construction | Separate LLM pipeline | Datasets/users | Apache-2.0 † | Moderate, active |
| **A-MEM** (arXiv 2502.12110) † | Zettelkasten-style linked notes with "memory evolution" | Notes | Note attributes + links, older notes updated | LLM | Not a focus | Research code (MIT?) † | Research prototype |
| **Claude Code compaction + CLAUDE.md / auto memory** | Instruction files + auto notes + summarize-on-compact | CLAUDE.md re-injected after /compact; transcripts on disk | Single-level compaction summary | CLAUDE.md: "You"; auto memory: "Claude"; first 200 lines/25KB loaded | Per repo, "shared across worktrees" | Proprietary | Production |
| **Anthropic memory tool (API)** | Client-side file directory (`/memories`) the model reads/writes; pairs with context editing (`clear_tool_uses_20250919`) and server compaction | Whatever your app stores | None built in; the model curates files | The model ("Claude automatically checks its memory directory before starting a task") | Up to your app | API terms (proprietary service) | GA on Claude 4+; context editing beta |
| **ChatGPT memory** | "Saved memories" + "reference chat history" | Not exposed | "Continually updated synthesis" (per FAQ via secondary source) | The system/model, user-editable | Single user | Proprietary | Production (expanded 10 Apr 2025) |
| **Plain RAG over notes** (your current setup) | Index + retrieve | Everything (notes, transcripts) | None, or per-note | Humans/agents writing notes | Natural (shared vault) | Yours | Mature pattern |

**Interpretation:**
- Anima is the only system here that treats *how* memory is phrased and *when* the prompt prefix changes as first-class design variables. Every other system treats memory as content to retrieve.
- Anima's weakness is the mirror image: it cannot answer "find the note about X from March" except by keeping March in context at some resolution, or by an agent reading the archive.
- Your vector + graph + keyword ladder is complementary, not competing.

---

## 6. People and openness

- **Founders:** Janus (@repligate) and Antra Tessera (@tessera_antra). The site lists team members j⧉nus, antra, waterm̸ark (@anthrupad), im⁂go (@astarchai), lari (@Lari_island), telØS (@AlkahestMu) and fiora (@FioraStarlight). It describes Anima as "a San Francisco-based 501(c)(3) research institute founded in 2025".
- **Core engineers by GitHub activity seen:**
  - **antra-tess**: author and merger of most connectome-host and context-manager PRs, and co-author of the adaptive-resolution design "+ Claude (Opus 4.7)".
  - **nissa-seru**: chronicle, context-manager and host fixes; issue #580.
  - **tel-0s**: chronicle "Dendrite" views PR #40.
  - **Anarchid**: host docs rewrite PR #168.
  - **csd-oss**: onboarding docs PR #383.
  - **ian-de-marcellus**: agent-framework suppressWake PR #180.
  - **slimepriestess**: mcpl-cc-bridge PR #5.
  - The org lists "no public members". [established]
- **AI-authored commits are normal.** Commits carry "Co-Authored-By: Claude Opus 5.5". Issue #580 credits agents ("Gladys-2413 found these… Fiona-2757 wrote this issue").
- **How contributions land (context-manager CONTRIBUTING.md):**
  - PRs against `main`, merged as true merge commits.
  - A Problem/Changes/Tests body, plus "Not verified" and "Companion PRs" sections.
  - A changelog entry under one `## Unreleased` heading, enforced by CI.
  - Reviews come as PR comments, "frequently AI-generated and explicitly labeled". Reviewers will "run your branch… often a compile against a real store".
  - "AI-written code is the norm in this ecosystem", provided it is declared.
- **Community:** Discord https://discord.gg/anima (linked from site navigation) and a "Work with us" page (https://animalabs.ai/participate/, not opened).
- **Funding and partners:** 501(c)(3) status is stated. Donors, grants and partners were [no source found] in this session.
- **Good first contributions (concrete, open as of 1–2 Nov 2026):**
  1. **connectome-host #580.** Add `mkdir -p backups` to `docs/claude-code-ingest.md`, reorder stages 2–4, and update `AGENT-MEMORY-GUIDE.md`, README and onboarding to say kv-unified is the default. The issue is labelled bug and currently unassigned.
  2. **chronicle #34.** Make `Store::create` refuse an existing store, and give `open_or_create` on an empty directory an error that names the path. This is a small Rust fix with a measured reproduction already in the issue.
  3. **Write the missing eval.** A recall-fidelity harness (seeded facts → fold to L_k → probe) would fill the biggest gap. It fits CONTRIBUTING's "evidence over assertion" ethos, but coordinate on Discord first.

---

## 7. What an outside lab should take or avoid (ranked)

**Ideas worth adopting (highest value first):**
1. **Separate the immutable archive from the working view.** You already keep verbatim transcripts. Make "what an agent is shown" a reproducible, logged projection over that archive (with a layout receipt per call), so any recollection can be audited against source. [high confidence]
2. **"As-of" chronological summaries.** When summarizing a session chunk, give the summarizer only prior summaries plus that chunk, never later knowledge. Store each summary with source hash, level and parent. This removes hindsight contamination from your memory notes. [high]
3. **Fold by token pressure, not by counts, and keep summaries re-renderable.** Keep L1 summaries even after producing L2, so you can re-expand a region when budget allows. Anima's 430k→single-L3 incident is the cautionary tale. [high]
4. **Cache-stable prefix discipline.** Put stable material (instructions, old summaries) first, change it in batches, and log cache hit and miss per call. Anima's numbers suggest large savings at long contexts. [medium: their evidence is self-reported]
5. **Speaker-preserving storage.** Store Membrane-style `participant` names rather than user/assistant roles. Across Claude Code, Codex, Grok, Gemini and Cursor, "who said it" is exactly what your shared store loses today. [high]
6. **Voice of the summarizer.** For per-agent continuity, have the agent's own model write its episodic summaries. For your *shared* fact notes, keep a neutral voice. Anima's design centre is single-agent identity; yours is cross-agent knowledge. [medium]
7. **Event-driven wake (MCPL).** This is useful if you want vault changes to wake agents, but it is not needed for memory. [low priority]

**Risks:**
1. **License absence (highest).** chronicle, context-manager and connectome-host showed no detected LICENSE on the org listing. Without a license you have no right to reuse the code; ideas are free, code is not. Ask the maintainers to add one before vendoring anything.
2. **Velocity and breaking changes.** context-manager went from v0.10 (Sep 2026) to v0.24 (Nov 2026) with audience-scoped "Breaking" sections nearly every release. Docs lag the code (#580). Pin exact versions.
3. **Concentration and bus factor.** Most merges and design authorship run through antra-tess. The broader contributor base is small and partly pseudonymous.
4. **No retrieval and no evals.** Their system will not replace your vector/graph/keyword ladder, and there is no evidence on recall fidelity of L3+ summaries.
5. **Operational complexity.** A Rust native build, per-platform binaries ("never rsync them across OSes"), fail-closed kv-unified configs and topology audits make this heavy for eight machines and five vendors.
6. **Value alignment.** The stack embeds strong normative choices (agents as beings, identity-critical decisions). That is fine if shared; otherwise expect friction in design discussions.

**Best entry point:**
- Start on Discord (https://discord.gg/anima). Then send a small, evidence-rich PR closing connectome-host #580 or chronicle #34, following CONTRIBUTING to the letter (numeric test baseline, "Not verified" section, declared AI authorship).
- In parallel, ask in an issue whether chronicle, context-manager and connectome-host will adopt a license (agent-framework is already MIT).
- The highest-leverage medium-term collaboration is a **memory-fidelity benchmark** for autobiographical folding. It is something they lack, and your multi-vendor transcripts are a natural test bed.

---

## Name collisions (not Anima Labs)
- **Anima International**: animal-welfare organization (not researched).
- **Anima (CircleStone Labs)**: 2B anime text-to-image diffusion model (huggingface.co/circlestone-labs/Anima), supported in vLLM-Omni.
- **Anima Biotech**: mRNA drug-discovery company.
- **"Anima" iOS app ("Just Done, Twin Mind - Anima")**: a companion app with "semantic memory with vector-based recall".
- **Anima AI companion app**: covered on dev.to.
- **stell2026/Anima**: an experimental cognitive architecture repo.
- **AnimaWorks (xuiltul/animaworks)**: "Organization-as-Code" agent framework.
- **ElectronGirl/anima-ref-adapter-research**: adapters for CircleStone's Anima.
- **akmorley/vllm-prism**: vLLM fork; no Anima Labs link found.
- **Unrelated Hermes memory plugins**: hermes-mem, Memory Bridge (SQLite+FTS5), memex-hermes, hermes-basic-memory. Not by Anima; relevant only as alternatives for Hermes users.
- **LuigiFerronatto/TESSERA** and the Zenodo "Tessera Protocol": unrelated to Antra Tessera.

## Caveats
- Only 18 lead searches were possible versus the 30 requested. hermes-autobio as a standalone plugin, vllm-prism, funding, and exact last-commit dates per repo remain unverified.
- GitHub star, fork and issue counts differed widely between cached snapshots; none should be quoted as current without re-checking.
- The "no LICENSE" finding rests on GitHub's org-listing badge. Confirm by opening each repo root.
- Alternatives marked † were not re-verified against primary sources in this session.
- Anima's quality claims are self-reported in their own docs; no independent evaluation exists.

## Sources (md_citations)
https://animalabs.ai/connectome/
https://github.com/anima-research/ecosystem-overview
https://github.com/anima-research/connectome-host
https://github.com/anima-research/context-manager/blob/main/docs/adaptive-resolution-design.md
https://github.com/anima-research/connectome-host/blob/main/docs/AGENT-ONBOARDING.md
https://github.com/antra-tess/openclaw-memory-hierarchical
https://github.com/anima-research/connectome-host/issues/580
https://github.com/anima-research/chronicle/issues/34
https://github.com/anima-research
https://github.com/anima-research/context-manager
https://github.com/anima-research/connectome-host/pull/489
https://github.com/silvariasereneblossom/chronicle
https://github.com/antra-tess/membrane/releases/tag/v0.9.0
https://github.com/anima-research/connectome-host/pull/475
https://github.com/anima-research/anima-docs
https://www.npmjs.com/~antra_tessera
https://github.com/anima-research/eidoverse-worlds
https://github.com/anima-research/wfe
https://github.com/mlowdi/connectome-host
https://github.com/anima-research/context-manager/releases/tag/v0.22.0
https://github.com/anima-research/context-manager/releases/tag/v0.21.0
https://github.com/anima-research/context-manager/releases/tag/v0.15.0
https://github.com/Meganeuridae/connectome-host
https://animalabs.ai/
https://x.com/repligate/status/1941566355771400680
https://x.com/repligate/status/2049387183937597495
https://www.anthropic.com/news/deprecation-updates-opus-3
https://x.com/repligate
https://smoothbrains.net/posts/2026-04-08-a-conversation-with-anima-labs-part-i.html
https://github.com/anima-research/chronicle/pull/21
https://github.com/anima-research/context-manager/pull/133
https://github.com/theaspirational/connectome-host
https://github.com/nissa-seru/connectome-host
https://github.com/The-Glassy/connectome-host
https://code.claude.com/docs/en/memory
https://dreaming.press/posts/context-engineering-anthropic-way-claude-skills-compaction-memory.html
https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool
https://platform.claude.com/docs/en/build-with-claude/context-editing
https://openai.com/index/memory-and-new-controls-for-chatgpt/
https://memx.app/blog/what-chatgpt-memory-profile-stores/
https://github.com/anima-research/connectome-host/pull/488
https://github.com/anima-research/chronicle/pull/40
https://github.com/anima-research/connectome-host/pull/168
https://github.com/anima-research/connectome-host/pull/383
https://github.com/anima-research/agent-framework/pull/180
https://github.com/anima-research/mcpl-cc-bridge/pull/5
https://github.com/anima-research/connectome-host/pull/376
https://github.com/vllm-project/vllm-omni/blob/main/recipes/circlestone-labs/Anima.md
https://github.com/vllm-project/vllm-omni/issues/3658
https://www.drugdiscoverytrends.com/anima-biotech-aims-to-open-the-black-box-of-disease/
https://apps.apple.com/us/app/-/id6756440266
https://dev.to/data_expertise/revolutionizing-digital-interaction-with-anima-ai-for-human-like-conversations-and-intelligent-56pf
https://github.com/stell2026/Anima
https://github.com/xuiltul/animaworks
https://github.com/ElectronGirl/anima-ref-adapter-research
https://github.com/akmorley/vllm-prism
https://github.com/dmmeteo/hermes-mem
https://pypi.org/project/memory_bridge
https://docs.basicmemory.com/integrations/hermes
https://pypi.org/project/memex-hermes/
https://docs.basicmemory.com/raw/integrations/hermes.md
https://github.com/LuigiFerronatto/TESSERA/issues/170
https://zenodo.org/records/18135849