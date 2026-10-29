---
dr_id: DR26-07-28-HUB-21-2339
title: "Persistent structural code memory for coding agents (symbol/call graph, blast-radius analy"
date: 2026-07-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-21-2339): Persistent structural code memory for coding agents (symbol/call graph, blast-radius analysis)

> Deep-research answer on how to give Claude Code / Codex a persistent, auto-updated structural memory of a codebase (symbol graph, call graph, change-impact) to cut tokens and prevent breaking shared code, and whether to adopt an existing tool or build one.

## Ключевые выводы
- Adopt, don't build first: CodeGraph is the best immediate fit for local, persistent structural memory (MIT, 100% local SQLite+FTS5+WAL, one-line install, auto-configures Claude Code/Codex/Cursor/etc., OS file-watcher with 2s debounce, Windows-native via ReadDirectoryChangesW).
- CodeGraph's self-reported (unaudited) benchmark across 7 open-source repos: median 58% fewer tool calls, 22% faster completion, 23-64% fewer total tokens, near-zero file reads when the graph is available.
- Serena is the best complement for IDE-grade symbol refactors (LSP-backed find-symbol/references/implementations, safe renames/moves) but its evidence is qualitative/self-evaluative, not benchmarked; setup is moderate and language-server-dependent.
- Academic literature (RepoBench, ContextBench, SWE-ContextBench, SWE-Explore) shows repo-level coding is a retrieval/navigation bottleneck before it's a generation problem; unfiltered/poor context can hurt, not just fail to help.
- Graph-based retrieval papers (GraphCoder, RepoGraph, CodexGraph, SGKR, Prometheus) consistently show structure beats flat similarity/embedding search for caller/callee, dependency-path, and multi-hop navigation tasks; pure semantic RAG has low recall on these.
- A deterministic-anchoring study found lightweight call/inheritance topology improved localization, roughly halved run-to-run variance, and improved Pass@1, at the cost of ~10% more input tokens -- structure mainly reduces stochasticity, not just adds smarts.
- Bigger context windows do not replace retrieval/structure: 'Lost in the Middle' and 'Prometheus' show even million-token windows suffer needle-in-haystack degradation on large codebases; long context is a complement, not a substitute.
- Classical change-impact-analysis literature: static analysis (call graphs) is conservative/approximate, dynamic analysis (traces) is more precise but incomplete outside traced runs, and exact effects are not tractable in general -- blast-radius tools are decision support, not proof.
- Cursor native index and Sourcegraph/Cody are strong semantic/concept layers (embeddings, code search, SCIP cross-repo intelligence) but are not substitutes for explicit local blast-radius/call-graph analysis; Sourcegraph is the right tool only once cross-repo pain becomes real.
- Main operational failure modes: stale index (CodeGraph mitigates via pending-file warnings + connect-time reconciliation, but staleness never hits zero), wrong/partial symbol resolution (tree-sitter alone is weak on semantic/external-dependency resolution vs LSP/SCIP), watcher hygiene (must exclude node_modules/vendored/generated dirs), multi-repo blind spots, and domain mismatch for n8n (workflow JSON graphs need a custom extractor, not ordinary code-graph tooling).

## Рекомендации / решения
- Adopt CodeGraph first as the default structural memory layer; do not build a bespoke multi-language semantic graph from scratch initially.
- Rollout order: (1) Python automation repos with shared scripts/cron/infra logic -- highest blast-radius risk; (2) CRM app; (3) agent skills repos (semantic layer carries more weight here); (4) n8n workflows last, only with a custom JSON extractor (nodes/connections graph).
- Add Serena as a second layer only after month-one CodeGraph usage shows the pain is specifically safe multi-file refactors/renames/moves, not just navigation.
- Host the watcher/indexer on the persistent always-on desktop/dev workstation (local disk, not network share, not WSL-mounted paths where SQLite WAL can degrade) -- consistent with the 'desktop hub = heavy/persistent' hosting rule.
- Wire a mandatory 'impact preflight' into the existing 'read-before-fix' rule: for edits touching shared utilities, public/exported symbols, integration adapters, workflow entry points, or auth/billing/scheduling/infra code -- query the structure layer for callers/callees/impact radius FIRST, then read only the surfaced files, name the blast radius in the plan, and run targeted tests after.
- Layered retrieval order for token economy: structural query first -> semantic search second -> literal grep third -> read live code last (only the few files/symbols the first three steps surface).
- Add ignore rules for vendored/generated/large directories before judging watcher cost or performance.
- Treat structural-memory output as approximate/decision-support, never as proof -- always verify with targeted tests after edits, and keep the standing 'read code before fixing safety-critical infra' rule in force.

## Сущности
- **Люди:** —
- **Компании:** Sourcegraph, Cursor, Anthropic
- **Продукты/инструменты:** codegraph (colbymchenry/codegraph), Claude Code, Codex, Serena, Aider (repo-map), Sourcegraph/Cody, Continue, Cursor (native index), tree-sitter, LSP, SCIP, n8n, RepoBench, ContextBench, SWE-ContextBench, SWE-Explore, GraphCoder, RepoGraph, CodexGraph, SGKR, Prometheus

## Открытые вопросы
- CodeGraph's token/tool-call savings numbers are self-reported and unaudited -- need Anton's own month-one measurement (tool calls, file reads, total tokens, missed callers, regression rate) to confirm ROI on his actual repos.
- Windows behavior of Serena depends on which underlying language server is used per language -- not uniformly verified.
- No public head-to-head benchmark isolating Aider repo-map's token savings the way CodeGraph has one.
- Whether/how to build the n8n custom JSON extractor (node/connection graph) remains undecided -- explicitly deferred to last in rollout.
- Report flags a security caveat (agentic workflows / n8n templates can be hijacked via crafted contextual inputs) without full analysis -- reinforces but doesn't resolve the read-before-fix safety rule.

## Источник
- DR-ID `DR26-07-28-HUB-21-2339` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- codegraph
- concept-RAG
- vault-data-architecture
- fix-root-cause-not-symptoms
- one-system-propagate
- node-doctrine-thin-anchor
- blast-radius-analysis
