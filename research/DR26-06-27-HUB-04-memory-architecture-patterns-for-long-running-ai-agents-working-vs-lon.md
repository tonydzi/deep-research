---
dr_id: DR26-06-27-HUB-04
title: "Memory architecture patterns for long-running AI agents (working vs long-term, active/arch"
date: 2026-06-27
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-27-HUB-04): Memory architecture patterns for long-running AI agents (working vs long-term, active/archive, forgetting)

> How should an AI agent manage working vs long-term memory, detect focus shifts, and move items between active and archived memory without context overflow or drift?

## Ключевые выводы
- Agents should split working memory (session-scoped, aggressively pruned) from long-term episodic/semantic memory persisted externally; summarize/compress what can't be dropped, prune what can be re-fetched.
- A small always-reloaded scratchpad/state file (goal, decisions, next steps) survives summarization and bridges sessions cheaply.
- RAG-style retrieval (embed query → cosine similarity search over a dynamically updated memory store) is the core mechanism for injecting relevant past context without loading everything.
- MemGPT/Letta pattern: a separate 'sleep-time' agent handles memory consolidation/rewriting offline, decoupling heavy memory maintenance from the live conversational agent; higher sleep frequency = more tokens but fresher context.
- Biologically-inspired consolidation (SAGE's Ebbinghaus forgetting curve, SCM's NREM/REM-style replay/dreaming) improves coherence but adds significant complexity — described as research-prototype territory, not drop-in.
- Focus/topic-shift detection has no single reliable method: best practice combines recency/LRU tracking, embedding-similarity drift (threshold-based), explicit user signals (strongest, least ambiguous), and task/metadata changes.
- Active↔archive movement needs promotion triggers (similarity search, keyword match, scheduled re-heating) and demotion triggers (LRU/TTL, context-limit pruning), plus hysteresis (candidate pool, require multiple hits before promoting) to prevent flapping.
- Some items (user_profile, goals, core rules) should be 'critical/procedural' — never auto-evicted, only changed via explicit update.
- Recommended tier: 'Moderate Dynamic Memory' (vector search + LRU/TTL pruning + daily cleanup) — best cost/benefit; baseline (static/manual, AK-47) risks context bloat and rigidity; full adaptive hierarchical/consolidation systems are high-risk, fragile, and overkill for most use cases.
- Named failure modes to guard against: silent memory loss, context overflow, memory staleness/drift (need freshness metadata + TTL + invalidation hooks), oscillation/thrashing (needs cooldown), overgeneralization from fuzzy retrieval thresholds, and memory dependency loops.

## Рекомендации / решения
- Adopt the 'Moderate Dynamic Memory' architecture: keep the always-loaded index (e.g. MEMORY.md) plus a vector DB of archived memories; on each turn embed the query, retrieve top-k, promote above a similarity threshold; evict least-recent active items via LRU when over capacity.
- Run a cheap per-session/daily maintenance pass (summarize, prune by TTL) and reserve deeper 'sleep-time'/weekly consolidation for pivots or scheduled off-hours, tuning frequency against token cost vs staleness tolerance.
- Implement hysteresis: require an archived item to cross a higher similarity threshold AND stay relevant across multiple turns (candidate pool) before fully promoting, to avoid flapping.
- Attach freshness metadata (timestamps/sources) to memory entries and set TTLs/invalidation hooks so stale facts don't silently mislead the agent.
- Designate certain facts/preferences as always-loaded 'procedural memory' with no TTL, changed only by explicit update — analogous to how CLAUDE.md/Bible-index files should be treated: as retrievable procedural knowledge rather than verbatim-loaded text, ideally indexed and queried via the same unified retrieval API as dynamic memories.
- Avoid the full 'Advanced Adaptive Memory' (multi-layer graph, dreaming, consolidation scheduler) unless the use case truly demands it — flagged as too complex/fragile for a non-technical maintainer to run 'with a hammer' (AK-47 test).

## Сущности
- **Люди:** —
- **Компании:** Letta
- **Продукты/инструменты:** MemGPT, Letta, Sleep-Time Compute, SAGE, SCM (memory framework), Reflexion, H-MEM, CLAUDE.md

## Открытые вопросы
- No single reliable method exists for detecting focus/topic shifts — thresholds for embedding drift are heuristic and need calibration or clustering to be robust.
- How to tune promote/demote thresholds (similarity, TTL, idle-turn counts) in practice without deep ML expertise remains open per-application.
- Staleness of stored facts ('the world changed') is called an open problem requiring freshness checks/invalidation hooks that aren't fully specified.
- Whether/how to apply full consolidation cycles (SCM-style NREM/REM dreaming) outside research prototypes is unresolved — cost/fragility tradeoffs unproven at production scale.

## Источник
- DR-ID `DR26-06-27-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- ak47-simplicity
- vault-data-architecture
- memory-index-hygiene
- always-on-memory-pilot
- turnstate-ledger
- verify-existing-before-proposing
