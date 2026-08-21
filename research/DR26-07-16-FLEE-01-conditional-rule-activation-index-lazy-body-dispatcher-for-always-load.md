---
dr_id: DR26-07-16-FLEE-01
title: "Conditional rule activation: index+lazy-body dispatcher for always-loaded CLAUDE.md-style"
date: 2026-07-16
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-16-FLEE-01): Conditional rule activation: index+lazy-body dispatcher for always-loaded CLAUDE.md-style agent instructions

> Investigated whether a large always-loaded agent instruction file (e.g. CLAUDE.md) can be safely restructured into a thin always-loaded trigger index plus lazily-loaded rule bodies without losing proactive-rule fidelity, and how existing coding-agent harnesses support this.

## Ключевые выводы
- Index+lazy-body architecture (thin always-loaded triggers/pointers + full rule bodies fetched on demand) is the consensus-recommended, harness-proven pattern — implemented natively via Claude Code Skills (description always visible, full SKILL.md loaded only on invocation), path-scoped Rules in .claude/rules/ (glob frontmatter), Hooks (deterministic, additionalContext injection), subdirectory CLAUDE.md, and subagents; Cursor's 'Agent Requested' and Windsurf's 'model_decision' modes do the same via model-judged descriptions.
- Claude Code has no native two-level lazy loader for the global always-loaded file itself — global «внутренний путь лаборатории» is always loaded in full each turn; only path-scoped rules and Skills give conditional loading. An April 2026 Anthropic feature request for full lazy-loading of context components (skills/MCP tools/rules) was reportedly closed 'not planned' per chatgpt vendor section.
- A rigorous 2026 factorial study on Claude Code (Sonnet/Opus 4.x, ~1,650 sessions, arXiv:2605.10039) found NO detectable adherence degradation from file size (25–500 lines), instruction position, single-vs-multi-file architecture, or internal conflicts (Bayes factors support the null) — contradicting anecdotal/official 'bloat dilutes adherence' warnings; within-session decay (~5.6% lower compliance odds per added function generated) and task complexity were the actual dominant predictors.
- Contradicting the null-result study, practitioner reports describe a much lower practical ceiling: 30–150 lines as the sweet spot, performance dropping past ~150 lines; one cited case (Sohit Kumar) went from 70% to 99% first-try success after shrinking CLAUDE.md from 470 to 94 lines; ETH-Zurich research found LLM-generated context files often hurt performance (-0.5–2%) unless kept concise.
- Semantic/keyword-based rule routing exists natively in Cursor/Windsurf (description-based model self-selection) and can be custom-built for Claude Code via a PreToolUse hook that scans each turn for keywords and injects only matching rule docs (a cited example, 'Primeline's Evolving Lite' router, claims ~80% token savings) — but this is a single-source/homegrown result with no formal published benchmark of missed-rule rate vs. tokens saved.
- Stable rule taxonomy is two-facet: primary = activation mode (always-loaded proactive condition / reactive lazy body / deterministic hook), secondary = domain/scope (path-glob, topic like security/testing/deployment). A decision framework: build-breaking → rule; action-triggered → skill; reference info → guideline doc; otherwise skip.
- Missed/wrong rule activation is the dominant documented failure mode, traced almost entirely to vague rule descriptions or malformed YAML/frontmatter — not to the lazy-loading architecture itself; proactive/must-fire guardrails should stay in the always-loaded thin layer or in deterministic Hooks, never rely solely on the model 'noticing' to fetch them.
- Recommended validation method: 'cold-reader' trap-scenario testing (fresh agent given only the compressed index/pointers plus adversarial prompts, checked for correct behavior) plus activation-audit logging (which rule groups fired per turn) and retrieval-regression suites simulating topic shifts; the user's own prior cold-reader test (5/5 trap scenarios passed) is cited as positive supporting evidence for the pointer-form approach.

## Рекомендации / решения
- Prioritize native harness primitives over a custom dispatcher: thin always-loaded CLAUDE.md/index (core identity + taxonomy pointers + proactive-rule conditions) + migrate procedural/multi-step content to Skills + move path-specific constraints to .claude/rules/ with paths frontmatter + use Hooks for deterministic must-fire logic.
- Keep the always-loaded core file small (target roughly 30–150 lines per practitioner reports, though the controlled study suggests up to 500 lines may not hurt adherence) — treat the lower bound as the safer default given anecdotal severity of degradation reports.
- Extend the user's existing 3-tier hot MEMORY.md dispatcher pattern (hot index → warm domain hubs → cold bodies) to the rules corpus itself.
- Only build a custom semantic router (PreToolUse hook keyword/embedding classifier, or DSPy/LangGraph classifier injecting via additionalContext) if native activation coverage proves insufficient after measurement — it adds real per-turn cost/latency and maintenance burden.
- Write rule/skill descriptions as precise, specific 'when to use' triggers — description quality is the primary lever against missed-activation failures.
- Build an activation-audit harness (log which rule groups were active per turn) plus a cold-reader trap-scenario test suite and retrieval-regression suite before and after any restructuring; track token usage delta explicitly.
- Never rely on semantic/lazy fetch alone for proactive/critical guardrails — encode those as always-loaded conditions or deterministic Hooks.

## Сущности
- **Люди:** Nicolas Bustamante, Sohit Kumar, D. McMillan, S. Nilsson, Roland Huß, Anand Rao
- **Компании:** Anthropic, Cursor, Windsurf/Codeium, GitHub Copilot, Devin, ETH Zurich, Primeline
- **Продукты/инструменты:** Claude Code, CLAUDE.md, Claude Skills, .claude/rules/ (path-scoped rules), Hooks (PreToolUse, additionalContext), Cursor .mdc rules (Agent Requested mode), Windsurf Cascade Memories/Rules (model_decision mode), AGENTS.md, DSPy, LangGraph, vLLM Semantic Router, Primeline Evolving Lite plugin, arXiv:2605.10039 factorial study

## Открытые вопросы
- No large-scale public benchmark reconciles the arXiv null-result study (size up to 500 lines doesn't hurt adherence) with practitioner anecdotes of sharp degradation past ~100-150 lines — unclear whether the gap is model strength, task realism, or rule signal quality.
- No formal published measurement exists of 'missed-rule rate' vs. token savings for semantic/keyword routing (Cursor/Windsurf native or custom hook-based) — all cited router success numbers (e.g. ~80% savings) are single-source/homegrown.
- Unclear whether position effects (primacy/recency) still matter in 2026 models — the factorial study found none across 5 positions in 250-line files, but this contradicts older prompting lore and some ACL 2024 findings that placing instructions after input can help long contexts.
- Anthropic's April 2026 feature request for native lazy-loading of all context components (skills/MCP/rules) was reportedly closed 'not planned' (chatgpt section) — status/roadmap unconfirmed and worth re-checking.
- No public failure-rate benchmark for rule-specific semantic routing at fleet scale; large-fleet scaling of the native-first approach is rated only 'emerging' confidence, not established.

## Источник
- DR-ID `DR26-07-16-FLEE-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- memory-index-hygiene
- claude-md-compression-contract
- write-service-files-tight-no-recompress
- capture-rules-into-bible
- test-after-build-skill
- always-on-memory-pilot
- insight-DR-DR26-07-17-HUB-04-conditional-rule-activation-in-llm-agents-lazy-loa — twin DR report on the same conditional rule activation / lazy-loading topic
