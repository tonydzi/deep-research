---
dr_id: DR26-07-28-HUB-18-2339
title: "SOTA architecture for shared, local-first agent memory across Claude Code, Codex, Gemini C"
date: 2026-07-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-18-2339): SOTA architecture for shared, local-first agent memory across Claude Code, Codex, Gemini CLI and generic MCP hosts

> Deep Research answers how a single-user, 128k-note Obsidian vault can become one shared, unified memory read/written by multiple heterogeneous agent runtimes without forking memory per runtime.

## Ключевые выводы
- The strongest fit is a three-layer design: the vault stays the canonical substrate (files + existing e5/RAG/SQLite/grep stack unchanged), a new local 'memory kernel' owns writes (validation, provenance, atomic creation, schema checks, promotion, conflict queues), and MCP is the single cross-runtime contract every agent talks to — not raw shared-file editing and not a separate agent database.
- Research (2026 surveys 'Memory for Autonomous LLM Agents' and others) frames agent memory as a write–manage–read data system, not a bigger prompt; no single memory architecture dominates — performance depends on matching representation/extraction/retrieval/maintenance to the workload bottleneck.
- Benchmarks (LongMemEval, LongMemEval-V2, MemoryAgentBench) show sustained multi-session memory is weak on temporal reasoning, knowledge updates and selective forgetting; LongMemEval-V2's coding-agent setup improved via workflow guidance, manifests and inspection tools — directly relevant to AGENTS.md-style onboarding and typed memory artifacts.
- Industry systems split into two families: file-first KBs (Tolaria, Basic Memory, Open Second Brain — filesystem authoritative, indexes derived) vs service-style memory layers (Mem0 factual/episodic/semantic layers, Letta core-vs-archival memory, Zep/Graphiti temporal knowledge graph). Service-style stores are stronger for app personalization but less inspectable/repairable for a single-user Markdown KB.
- Five concrete failure modes identified: (1) write races from concurrent agents editing the same file — mitigate with append-only/leaf-note writes plus content-hash/revision preconditions for in-place patches, not free multi-writer edits; (2) agent drift from divergent instruction files across runtimes (Claude Code reads CLAUDE.md/@AGENTS.md, Codex reads AGENTS.md directly, Cursor has its own rules) — fix by making root AGENTS.md canonical and generating runtime shims from it; (3) memory poisoning — persistent memory lets an attacker plant content in one session that steers a later action; content-based and lineage-only trust can both be laundered via summarization — needs trust separation (untrusted vs confirmed classes) and explicit promotion before privileged use; (4) hallucinated/stale memory — errors concentrate in extraction/update stages (HaluMem); fix with explicit temporal fields (valid_from/valid_to/supersedes/last_verified_at/confidence) and a visible conflict queue instead of silent overwrite; (5) framework churn/abandonment risk (LangChain v1 moved logic to langchain-classic, Letta/MemGPT reframed) — durable asset should be files + open schema + rebuildable indexes, not framework-native state.
- Deterministic, non-LLM signal→rule promotion (modeled on Open Second Brain's nightly 'dream pass') is preferred over LLM-driven reflection: a suggested concrete policy is a rule candidate created only after the same correction appears in ≥2 distinct sessions, confirmed after 3 confirmations (or 2 from different runtimes/days) with no unresolved contradiction; contradictions go to a Conflict queue; decay only for non-pinned, unreferenced items.
- Note-typing/schema should NOT be applied to the whole 128k-note vault at once — start with high-value classes only (Rule, Decision, Session, Source, Procedure, Project, Person, optionally Fact), using Tolaria's 'Type-document-as-schema' idea and Basic Memory's schema inference/validation/diff approach.
- Git's correct role is narrower than 'git-native memory' pitches suggest: good for audit, rollback, branching, and isolated agent worktrees, but commit history is a poor semantic-recall interface (sparse logs, low-level diffs) — keep existing commit-before-write snapshots plus checkpoint tags for promotion runs, don't make Git the retrieval layer.

## Рекомендации / решения
- Do not replace the vault or existing e5/reranker/SQLite/grep retrieval stack; wrap it.
- Introduce one canonical local memory kernel/gateway that every runtime accesses via MCP (resources for read-only artifacts like memory://agents, memory://active, memory://schemas, memory://conflicts; prompts for maintenance workflows; tools only for side-effecting ops like search_memory/create_note/patch_note/append_signal/promote_rule/rollback_snapshot).
- Run one canonical local kernel process (loopback HTTP/Unix socket, or a thin stdio bridge per host) rather than each client independently writing straight to the vault, to avoid multi-process write races.
- Make root AGENTS.md the single human-maintained onboarding document; have CLAUDE.md import @AGENTS.md, let Codex read AGENTS.md directly, and generate any Cursor/Gemini-specific deltas as small shims from it.
- Add a small typed namespace (e.g. Brain/) for Rule/Decision/Session/Source/Conflict/Signal/Schema notes with frontmatter fields (type, id, created_at, updated_at, provenance, source_refs, confidence, valid_from, valid_to, supersedes, review_state) — do not schema the whole vault.
- Keep memory consolidation fully deterministic (counters + atomic moves); if LLM assistance is wanted, run it outside the kernel as a proposal step that writes a candidate signal, never as the authoritative promoter.
- Restrict write tools initially to three: create_note (atomic, path-safe), patch_note (requires prior content hash/revision), append_signal (idempotent, append-only log); add a rebuild command so the sidecar SQLite (WAL mode) DB can always regenerate from Markdown.
- Phased roadmap: (1) contract phase — AGENTS.md + typed Brain/ namespace; (2) memory kernel phase — wrap retrieval stack behind one local MCP service with the three write tools; (3) deterministic maintenance phase — nightly promotion/staleness/conflict jobs from the signal ledger; (4) typed-overlay phase — derive a graph index (SQLite/DuckDB) from wikilinks/frontmatter as a projection, not canonical store; (5) hardening phase — restrict runtime roots, gate privileged/destructive writes behind approval, separate trust classes, add CI/pre-commit schema validation for Brain/.
- Avoid anti-patterns: per-runtime memory forks; free-editing of arbitrary existing human notes by agents (use append/leaf notes instead); hiding canonical memory in a vector DB or framework-native store; treating Git history as semantic memory; promoting rules from a single correction; mixing trusted rules with untrusted imported excerpts; schema-izing the whole legacy vault at once.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Zep, Letta, MemGPT, LangChain
- **Продукты/инструменты:** Tolaria, Open Second Brain, Basic Memory, Hermes, agentmemory, Mem0, Zep/Graphiti, MCP, Claude Code, Codex, Gemini CLI, Cursor, Obsidian, SQLite (WAL), write-file-atomic, AGENTS.md, CLAUDE.md, LongMemEval, LongMemEval-V2, MemoryAgentBench, HaluMem, DMR benchmark

## Открытые вопросы
- Exact implementation details of the local memory kernel (sidecar DB schema, promotion-policy tuning) are proposed as a synthesis, not empirically validated for this specific vault.
- Whether/how to bind the graph/temporal overlay (Graphiti-style) into the vault without adopting Graphiti itself as primary store is left as a design reference, not a worked implementation.
- How trust-separation for memory poisoning defense (lower-trust vs confirmed classes) should be enforced mechanically wasn't fully specified beyond 'require explicit approval or deterministic promotion rule'.
- No empirical benchmark was run comparing this proposed 3-layer architecture against alternatives specifically on the 128k-note vault — recommendation is synthesis-based, grounded in cited external research/tooling.

## Источник
- DR-ID `DR26-07-28-HUB-18-2339` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- one-system-propagate
- second-brain-northstar
- self-bible-identity-layer
- machine-bus-telegram-rail
- system-architect
- coverage-and-docs-every-part
- ak47-simplicity
