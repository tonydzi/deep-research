# DR26-07-01-ZB-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "State-of-the-art second brain"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4adc1d-29e0-83ed-9fae-c9b4fa70ebe1
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

# State-of-the-art second brain

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

DR26-07-01-ZB-01

Conduct a deep research investigation on the following topic.



TOPIC:

State-of-the-art architecture for a SINGLE-USER, local-first personal "second brain" (markdown knowledge base) that serves as SHARED, UNIFIED MEMORY across multiple coding/agent runtimes (Claude Code, Codex, Gemini CLI, and MCP-generic hosts) — so every agent reads and writes the SAME memory with no per-runtime fork.



CONTEXT (what already exists — do not re-recommend it, build beyond it):

- A ~128k-note Obsidian vault, PARA-like folders, wiki-links, MOC index notes, frontmatter with provenance.

- Retrieval = local e5 embeddings + reranker RAG + SQLite facts + grep. Principle: "SQL/grep/RAG before LLM".

- Git-commit-before-write backup already in place (a script), NOT per-vault git-as-memory.

- Two reference tools studied: (a) tolaria (refactoringhq, AGPL, Tauri desktop app) — files-first, git-per-vault as linear audit-history, an AGENTS.md root convention read by any agent, an in-repo MCP server exposing open_note/create_note/search_notes/edit_note_frontmatter/link_notes, "Type-document-as-schema" typing; (b) open-second-brain (itechmeat, MIT) — Brain/ folder inside the vault, per-runtime install manifests (install.lock.json), an MCP read/write contract (brain_context/brain_note), and a DETERMINISTIC nightly "dream pass" (counters + atomic file moves, no LLM) that promotes repeated corrections into confirmed rules and retires unused ones.



OBJECTIVE:

Find the best current practices, SOTA approaches, failures, trade-offs, and an implementation framework for (1) unifying memory across heterogeneous agent runtimes via an MCP contract vs shared files, (2) git-first vaults and history-as-memory, (3) a deterministic (non-LLM) signal→rule promotion pipeline, (4) an AGENTS.md-style single onboarding doc that keeps multiple agents behaviorally consistent, (5) note-typing/schema-validation for a large markdown vault.



RESEARCH REQUIREMENTS:

1. Academic research — papers/benchmarks on agent memory architectures, memory consolidation, retrieval-augmented agents, knowledge-graph-over-markdown.

2. Industry practice — tools (Obsidian ecosystem, Reflect, Mem0, Zep, Letta/MemGPT, basic-memory, tolaria, open-second-brain, Hermes), how serious solo builders wire CC/Codex/Gemini to one memory.

3. Competitive landscape — direct (personal-KM tools with agent memory), indirect (vector memory services), substitutes (plain files + RAG).

4. Failure analysis — where "unified agent memory" projects broke (write races, drift between agents, memory poisoning, stale facts), what was abandoned and why.

5. Emerging trends — last 12 months: MCP memory servers, deterministic memory hygiene, git-native knowledge bases, cross-agent context contracts.

6. Implementation recommendations — an actionable framework + phased roadmap + priorities SPECIFIC to the CONTEXT above (do not propose replacing the existing e5/RAG vault; propose additive layers).



OUTPUT FORMAT:

- Executive summary (5-8 bullets).

- Options matrix: each candidate approach × {what it adds over current, effort, ongoing cost, failure risk, repairability by a non-technical owner}.

- Explicit recommendation + phased roadmap (what to do first, what to defer).

- Anti-patterns / what NOT to do.

- Sources with links.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"task":"Conduct deep research on state-of-the-art architecture for a SINGLE-USER, local-first personal second brain (markdown knowledge base) serving as shared unified memory across Claude Code, Codex, Gemini CLI and MCP hosts. Include academic research, industry practice, competition, failure analysis, trends, implementation recommendations, options matrix, roadmap, anti-patterns, sources. Incorporate provided context: existing Obsidian vault with e5+RAG+SQLite+grep, git-before-write, tolaria and open-second-brain already studied; build beyond them."}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-01-ZB-01 Conduct the deep research investigation exactly as specified in the user's prompt, preserving all requirements, output format, and constraints."}}
```
