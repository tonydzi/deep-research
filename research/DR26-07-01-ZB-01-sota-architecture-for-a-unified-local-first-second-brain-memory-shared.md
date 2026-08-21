---
dr_id: DR26-07-01-ZB-01
title: "SOTA architecture for a unified local-first second-brain memory shared across Claude Code,"
date: 2026-07-01
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-01-ZB-01): SOTA architecture for a unified local-first second-brain memory shared across Claude Code, Codex, Gemini CLI and MCP agents

> ⚠️ **UPDATE 2026-07-14:** эта заметка была сгенерена по ПРОМПТУ (тела отчёта тогда не было — ChatGPT не отдаёт DR через экспорт). Полный отчёт СОБРАН 14.07 (Export→Markdown), лежит в `_originals\deep-research\` (путь в «Источник»). Актуальный синтез: [[decision-agent-stack-dr-bundle-synthesis-2026-07-14]].


> A deep-research request (not yet returning findings in this record) asking how to unify a single-user markdown vault as shared memory across heterogeneous agent runtimes via MCP contracts, git-first history, deterministic rule promotion, an AGENTS.md onboarding doc, and note-type schemas.

## Ключевые выводы
- This record only captures the DR request being dispatched to ChatGPT's Deep Research tool (three short tool-call messages); no actual research findings, options matrix, or sources were returned in this vendor section.
- Existing baseline explicitly excluded from re-recommendation: ~128k-note Obsidian vault (PARA folders, wiki-links, MOC notes, provenance frontmatter), retrieval via local e5 embeddings + reranker RAG + SQLite facts + grep ('SQL/grep/RAG before LLM'), and a git-commit-before-write backup script (not git-as-memory).
- Reference tool tolaria (refactoringhq, AGPL, Tauri app): files-first design, git-per-vault as linear audit history, an AGENTS.md root convention readable by any agent, an in-repo MCP server exposing open_note/create_note/search_notes/edit_note_frontmatter/link_notes, and 'type-document-as-schema' typing.
- Reference tool open-second-brain (itechmeat, MIT): a Brain/ folder inside the vault, per-runtime install manifests (install.lock.json), an MCP read/write contract (brain_context/brain_note), and a deterministic nightly 'dream pass' (counter-based, atomic file moves, no LLM) that promotes repeated corrections into confirmed rules and retires unused ones.
- The requested research scope spans five areas: (1) MCP contract vs shared files for cross-runtime memory unification, (2) git-first vaults/history-as-memory, (3) deterministic non-LLM signal-to-rule promotion pipelines, (4) AGENTS.md-style single onboarding docs for behavioral consistency across agents, (5) note-typing/schema validation at vault scale.

## Рекомендации / решения
- Any implementation framework should be additive layers on top of the existing e5/RAG/SQLite/grep vault, not a replacement of it.
- Investigation should explicitly cover failure modes of prior 'unified agent memory' projects (write races, cross-agent drift, memory poisoning, stale facts) before proposing a design.

## Сущности
- **Люди:** —
- **Компании:** refactoringhq, itechmeat
- **Продукты/инструменты:** tolaria, open-second-brain, Claude Code, Codex, Gemini CLI, MCP, Obsidian, e5 embeddings, SQLite, Mem0, Zep, Letta/MemGPT, basic-memory, Hermes, Reflect, AGENTS.md

## Открытые вопросы
- The actual Deep Research output (executive summary, options matrix, recommendation, roadmap, anti-patterns, sources) was not present in this vendor section and needs to be retrieved/re-run to be captured.
- Whether other vendor sections (Gemini, Claude, etc.) for this same DR-ID contain the completed findings that should be merged in.

## Источник
- DR-ID `DR26-07-01-ZB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-01-ZB-01-state-of-the-art-second-brain-6a4adc1d-chatgpt.md`

## Связано
- [[second-brain-northstar]]
- [[vault-data-architecture]]
- [[agent-teams-scoped-adoption]]
- [[machine-bus-telegram-rail]]
- [[one-system-propagate]]
- [[capture-rules-into-bible]]
- [[insight-DR-DR26-07-28-HUB-18-2339-sota-architecture-for-shared-local-first-agent-mem]] — тот же вопрос unified local-first memory; ранняя заметка сама пишет что вывод нужно перезабрать
