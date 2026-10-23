# DR26-07-01-ZB-02 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Codebase Memory for Agents"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4adc2c-d698-83eb-9db2-f55a88b1d908
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-05
date_added: 2026-07-07
valid_as_of: 2026-07-05
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 3
related_concepts: []
revisit_if: ""
---

# Codebase Memory for Agents

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

DR26-07-01-ZB-02

Conduct a deep research investigation on the following topic.



TOPIC:

Best approach to give a coding agent (Claude Code / Codex) a persistent, auto-updated STRUCTURAL memory of a codebase — symbol graph, call graph, and change-impact ("blast radius") analysis — to cut token usage and prevent breaking shared code, for a solo operator's repos (Python automation scripts, a CRM app, agent skills, n8n workflows).



CONTEXT (what exists / what was found):

- Today the agent reads whole files + greps to understand "who calls X" or "what breaks if I change shared script Y" — token-heavy, occasionally misses callers. There is a standing rule "read the code before fixing safety-critical infra".

- Reference tool studied: codegraph (github.com/colbymchenry/codegraph, MIT) — tree-sitter over 20+ languages → symbols/call-graph/imports → SQLite+FTS5, WAL; native OS file-watcher (2s debounce) keeps it current; MCP tools codegraph_explore/search/callers/callees/impact/node/files/status; ONE-line install; auto-configures CC/Cursor/Codex/opencode/Gemini/Hermes. Self-reported (not audited) ~58% fewer tool calls, 23-64% token reduction, 8-40% cheaper/query on 7 OSS repos.

- Orthogonal to the vault's concept-RAG (RAG = knowledge, codegraph = code structure).



OBJECTIVE:

Find SOTA and trade-offs for code-structure memory / semantic code indexing for agents: adopt an existing tool (codegraph, Sourcegraph, Serena, ast-grep-based, LSP-backed, Aider's repo-map, tree-sitter homemade) vs build minimal; token/accuracy economics; how impact-analysis prevents regressions; maintenance cost of a persistent watcher.



RESEARCH REQUIREMENTS:

1. Academic — code representation for LLM agents, repo-level context, graph-vs-embedding code retrieval, impact analysis.

2. Industry practice — Aider repo-map, Sourcegraph/Cody, Serena, Continue, Cursor codebase index, codegraph, LSP/tree-sitter approaches; measured token savings.

3. Competitive landscape — direct code-graph tools, indirect (grep/RAG-over-code), substitutes (bigger context windows).

4. Failure analysis — where code-index tools mislead agents (stale index, wrong-symbol match), watcher/perf problems, multi-repo pain.

5. Emerging trends — last 12 months.

6. Implementation recommendations — adopt-vs-build decision + which repos to index first + where to host the watcher (heavy/persistent → desktop hub, not laptop) + how to wire impact-check into the "read-before-fix" rule.



OUTPUT FORMAT:

- Executive summary.

- Adopt-vs-build matrix (tool × {token saving evidence-quality, setup cost, maintenance, repairability, license, Windows support}).

- Recommendation + rollout order (repos) + hosting placement.

- Gotchas (stale index, false confidence).

- Sources.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"task":"Conduct a deep research investigation on the best approach to give a coding agent (Claude Code / Codex) a persistent, auto-updated structural memory of a codebase (symbol graph, call graph, impact analysis) for solo operator repos. Include academic literature, industry practice (codegraph, Aider repo-map, Sourcegraph, Serena, Continue, Cursor, LSP/tree-sitter), competitive landscape, failure modes, emerging trends (last 12 months), adopt-vs-build matrix, rollout recommendations, watcher hosting, impact-check integration, and sources."}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-01-ZB-02: Conduct a deep research investigation on the best approach to give a coding agent (Claude Code/Codex) a persistent, auto-updated structural memory of a codebase (symbol graph, call graph, blast-radius analysis) for solo operator repositories. Include academic research, industry practice, competitive landscape, failure analysis, emerging trends, adopt-vs-build matrix, rollout order, hosting recommendation, and sources."}}
```
