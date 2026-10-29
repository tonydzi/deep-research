---
dr_id: DR26-07-17-HUB-04
title: "Conditional Rule Activation in LLM Agents (lazy-loading rules/context vs always-on)"
date: 2026-07-17
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-17-HUB-04): Conditional Rule Activation in LLM Agents (lazy-loading rules/context vs always-on)

> How should AI coding agents (Claude Code and peers) load rules/instructions conditionally instead of dumping everything into context at session start?

## Ключевые выводы
- Consensus architecture: keep a small always-loaded 'index' (rule names/triggers/short descriptions) and fetch full rule 'bodies' on demand — described as 'the right structure' for scalability (Nicolas Bustamante).
- Claude Code has no native two-level lazy loader as of the report date: it forces full CLAUDE.md into context every turn, with the only built-in conditional mechanism being path-scoped glob rules (paths: frontmatter) that load only when matching files are touched.
- A public Anthropic feature request (Apr 2026) asks for lazy-loading of all context components (skills, MCP tools, rules) but is not implemented; envisions e.g. loading .claude/instructions/on-test.md only during test runs.
- Competing tools differ: Cursor/Windsurf support frontmatter-based rule modes (always/auto/glob/manual) and 'Intelligent' model-selected rule inclusion; GitHub Copilot loads custom instructions once per session with no per-turn reevaluation.
- Empirical size-vs-compliance data: 30–150 lines of rules tends to maximize performance; beyond ~150 lines extra rules often hurt accuracy. One practitioner (Sohit Kumar) raised first-try success from 70% to 99% by shrinking CLAUDE.md from 470 to 94 lines. An ETH Zurich study found LLM-generated context files hurt performance (-0.5–2%) unless kept concise (<150 lines).
- Custom semantic routing exists as a workaround: e.g. a PreToolUse hook scanning each turn for keywords and injecting only matching rule docs ('Primeline' router), reportedly ~80% token savings — but this is a homegrown, single-source solution with no formal 'missed-rule rate' benchmark.
- Rule taxonomy best practice: cluster into (a) tiny always-on core CLAUDE.md, (b) path-scoped glob rules, (c) on-demand knowledge/guideline docs, (d) skills for workflows, (e) isolated subagents for complex tasks; decide type via 'breaks build → rule, action-triggered → skill, reference → guideline doc, else skip'.
- Failure modes identified: missed rule trigger (glob/keyword miss), too-late injection (PreToolUse fires after the turn's key decision), stale pointer summaries, token bloat since Claude re-injects rules every turn without auto-unload, and cache/context disruption from mid-session rule injection.
- Debate unresolved: whether larger context windows (~1M tokens) will let hundreds of rules coexist without degradation, and whether instruction position (front vs. after input, per an ACL 2024 finding) still matters for compliance — no definitive 2026 evidence either way.

## Рекомендации / решения
- Shrink CLAUDE.md (or equivalent always-loaded index) to ~30-100 lines containing only mission-critical invariants, rule names/triggers, and pointers — not full rule text.
- Convert any non-universal rule into a path-scoped .claude/rules/*.md file with paths: frontmatter so it only loads when matching files are edited; verify via /context that out-of-scope edits omit it.
- For general (non-path-bound) domain rules, consider building/adopting a PreToolUse keyword or embedding-based router that injects only relevant docs per turn — but treat as an emerging/unproven practice requiring careful testing.
- Store full rule bodies/procedures externally (knowledge-base docs, skills) and reference them from the index; validate with 'cold-reader' tests — fresh agent given only pointers should still behave correctly.
- Monitor context budget continuously (e.g. Claude's /context) and keep rule content under roughly 5-10% of the window; if rules exceed ~150KB, plan dynamic/router strategies.
- Build a regression test suite of 'should'/'should not' trap prompts per rule category, run on fresh/cold agents, to catch missed triggers and false positives before shipping rule changes.
- Track Anthropic/competitor platform updates (the lazy-loading feature request status, AGENTS.md cross-platform format) since native support may change this landscape.

## Сущности
- **Люди:** Nicolas Bustamante, Roland Huß, Anand Rao, Sohit Kumar
- **Компании:** Anthropic, ETH Zurich, Primeline
- **Продукты/инструменты:** Claude Code, CLAUDE.md, Cursor, Windsurf, GitHub Copilot, Devin, AGENTS.md, PreToolUse hook, /context

## Открытые вопросы
- Whether much larger context windows will allow hundreds of rules without compliance loss, or whether the 30-150 line 'sweet spot' is a durable limit independent of window size.
- Whether instruction position (front-loaded vs. injected after input) meaningfully affects rule-following in current-generation models — cited ACL 2024 work conflicts with Claude's default system-first ordering, and no controlled 2026 study resolves it.
- No formal benchmark exists for semantic/keyword rule routers' 'missed-rule' rate vs. token savings — all evidence is single-source/homegrown (e.g. Primeline).
- Whether Anthropic will ship native lazy-loading for skills/MCP tools/rules (the Apr 2026 feature request status/timeline is unresolved).

## Источник
- DR-ID `DR26-07-17-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- memory-index-hygiene
- claude-md-compression-contract
- write-service-files-tight-no-recompress
- skill-authoring-portable-paths
- test-after-build-skill
