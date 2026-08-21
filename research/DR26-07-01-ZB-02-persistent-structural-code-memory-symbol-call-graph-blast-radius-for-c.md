---
dr_id: DR26-07-01-ZB-02
title: "Persistent structural code memory (symbol/call-graph, blast-radius) for coding agents like"
date: 2026-07-01
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-01-ZB-02): Persistent structural code memory (symbol/call-graph, blast-radius) for coding agents like Claude Code/Codex

> ⚠️ **UPDATE 2026-07-14:** эта заметка была сгенерена по ПРОМПТУ (тела отчёта тогда не было — ChatGPT не отдаёт DR через экспорт). Полный отчёт СОБРАН 14.07 (Export→Markdown), лежит в `_originals\deep-research\` (путь в «Источник»). Актуальный синтез: [[decision-agent-stack-dr-bundle-synthesis-2026-07-14]].


> Research request (deep-research not yet returned in this capture) on whether a solo operator should adopt an existing code-graph tool like codegraph vs alternatives to give coding agents persistent structural memory of a codebase and cut token usage while preventing regressions.

## Ключевые выводы
- This capture contains only the DR task-initiation prompt sent to ChatGPT's Deep Research tool — no synthesized findings, sources, or adopt-vs-build matrix were returned in the report text.
- Reference tool named for investigation: codegraph (github.com/colbymchenry/codegraph, MIT license) — uses tree-sitter across 20+ languages to build symbols/call-graph/imports, stored in SQLite+FTS5 with WAL mode.
- codegraph keeps its index current via a native OS file-watcher with 2s debounce, exposes MCP tools (codegraph_explore/search/callers/callees/impact/node/files/status), and auto-configures Claude Code/Cursor/Codex/opencode/Gemini/Hermes with a one-line install.
- codegraph's self-reported (unaudited) benchmarks across 7 OSS repos: ~58% fewer tool calls, 23-64% token reduction, 8-40% cheaper per query.
- The current baseline workflow (reading whole files + grepping to find callers or blast-radius of a change) is token-heavy and occasionally misses callers, motivating the need for a persistent structural index.
- Code-structure memory (codegraph) is framed as orthogonal to the vault's concept-RAG: RAG covers knowledge/concepts, codegraph covers code structure.

## Рекомендации / решения
- Evaluate adopt-vs-build: prioritize adopting an existing tool (codegraph, Sourcegraph/Cody, Serena, Aider repo-map, ast-grep, LSP-backed indexers) over building a homegrown tree-sitter solution, pending the actual deep-research comparison.
- Any persistent watcher process (heavy, always-on) should be hosted on the desktop hub rather than the laptop, consistent with the standing 'minimize laptop, maximize hub' resource rule.
- Wire impact/blast-radius analysis into the existing 'read the code before fixing safety-critical infra' standing rule so agents check call-graph impact before editing shared scripts.
- Obtain and review the actual completed deep-research output (this file only captures the prompt) before making a final adopt/build decision.

## Сущности
- **Люди:** Colby McHenry
- **Компании:** Sourcegraph, OpenAI
- **Продукты/инструменты:** codegraph, Claude Code, Codex, Cursor, opencode, Gemini, Hermes, Sourcegraph/Cody, Serena, Continue, Aider, ast-grep, tree-sitter, SQLite, FTS5

## Открытые вопросы
- The actual deep-research report (academic literature, competitive landscape, failure analysis, emerging trends, adopt-vs-build matrix, rollout order, sources) was not present in this capture and needs to be retrieved separately.
- Whether codegraph's self-reported token-saving benchmarks (58% fewer tool calls, 23-64% token reduction) hold up under independent audit.
- Which repos (Python automation scripts, CRM app, agent skills, n8n workflows) should be indexed first and in what order.
- How stale-index and false-symbol-match failure modes should be mitigated in practice for a solo operator's multi-repo setup.

## Источник
- DR-ID `DR26-07-01-ZB-02` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-01-ZB-02-codebase-memory-for-agents-6a4adc2c-chatgpt.md`

## Связано
- [[codegraph]]
- [[vault-data-architecture]]
- [[verify-existing-before-proposing]]
- [[desktop-max-laptop-min]]
- [[agent-stack-bundle]]
- [[insight-DR-DR26-07-28-HUB-21-2339-persistent-structural-code-memory-for-coding-agent]] — тот же вопрос (structural code memory/CodeGraph); новый отчёт — полноценное продолжение
