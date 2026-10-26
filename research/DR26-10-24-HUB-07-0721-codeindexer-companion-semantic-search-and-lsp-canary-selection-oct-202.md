---
dr_id: DR26-10-24-HUB-07-0721
title: "CodeIndexer companion: semantic-search and LSP canary selection (Oct 2026 delta)"
date: 2026-10-24
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-24-HUB-07-0721): CodeIndexer companion: semantic-search and LSP canary selection (Oct 2026 delta)

> The task asked which one semantic-search candidate and which one precise navigation baseline deserve a bounded, reversible real-code canary alongside the existing CodeIndexer, updating the 11 Sep 2026 research. The file contains only the research prompt and no vendor findings.

## Ключевые выводы
- The report text contains only the research brief (topic, objective, context, 7 research questions, output format and standing requirements). It has no findings, citations, versions or vendor answers, so no conclusion about any candidate can be drawn from it.
- The registry note says only 1 of 6 vendor legs is on disk (the file header lists targets chatgpt, claudeai, gemini, grok, glm, mistral). That one file is the prompt itself, not an answer. Registry status was corrected from 'running' to match the disk.
- Context stated in the brief: CodeIndexer is the first code-search tool, embeds with intfloat/multilingual-e5-small, uses ORT DirectML on the Windows hub, and has 21 registered projects. Registration does not prove an index is fresh or complete.
- A 24 Oct local canary cross-checked three Python symbol/call questions with CodeIndexer, Tree-sitter and Python AST. All matched the expected lexical locations, but one ambiguous symbol name returned unrelated definitions. This validates neither natural-language semantic retrieval nor dynamic dispatch.
- A 21 Oct Pyright canary on four cases did not establish incremental value, and one dynamic case stayed unresolved. The brief says this must not be read as LSP being useless or Serena having been tested.
- The predecessor DR26-09-11-HUB-01-0909 recommended keeping CodeIndexer and testing Serena (LSP navigation), cocoindex-code (semantic search), and vulture/semgrep (static analysis). Its recommendations were not proof of installation or benefit, and its Mistral leg was weak and partly parametric.
- Constraints from the brief: no private code leaves the machine (including embedding calls, logs and telemetry), CodeIndexer is not removed, no always-on stdio server per agent session, no index databases synced across operating systems, and browser-only execution with no CLI, headless LLM or OpenRouter legs.
- Freshness and candidate-corpus accounting were real false-confidence risks. The brief requires checking source bytes, excluded and generated paths, native indexed-file hashes, and stale, deleted or renamed files. A green process exit is not acceptance.

## Рекомендации / решения
- Do not treat this DR as completed or applied. Re-run the missing legs, or mark it parked, before any decision on Serena or cocoindex-code.
- When re-running, keep four categories separate: embedding semantic retrieval, symbol/LSP navigation, structural AST/Tree-sitter/ast-grep, and dead-code/security analysis (vulture, Semgrep).
- Follow the brief's benchmark contract: a frozen paired real-code set with independently reviewed answers. It covers natural-language discovery, exact definitions, callers/references, rename/delete freshness and ambiguous dynamic cases. Metrics are recall@k, MRR, false-positive/unknown rates, coverage, freshness, cold/warm latency, indexing time, peak memory and context size, with randomized order and no tool used as ground truth for another.
- Verify candidate identities against official repositories (cocoindex-io/cocoindex-code, oraios/serena) with pinned versions, dates and licenses. Do not equate the CocoIndex framework with the cocoindex-code product.
- Pick one semantic candidate and one navigation baseline, or state that evidence is insufficient. The canary must be minimal, reversible, with stop criteria and a rollback, and should reuse existing components before adding a daemon.

## Сущности
- **Люди:** —
- **Компании:** Sourcegraph, Anthropic
- **Продукты/инструменты:** CodeIndexer, cocoindex-code, CocoIndex, Serena (oraios/serena), Pyright, Tree-sitter, Python AST, ast-grep, vulture, Semgrep, SCIP, LSP, intfloat/multilingual-e5-small, ORT DirectML, Codex

## Открытые вопросы
- Which semantic-search candidate (cocoindex-code or another) deserves the canary? No vendor answer is present.
- Which navigation baseline (Serena, Pyright or another LSP/SCIP route) deserves the canary? No vendor answer is present.
- Do cocoindex-code and Serena run natively on Windows, macOS and Linux, and what are their remote-call, telemetry and licensing defaults? These are unverified.
- How do the candidates handle freshness (rename, delete, branch switch, concurrent writers), and can a consumer prove which bytes were indexed?
- Does CodeIndexer's natural-language semantic retrieval work on a real-code set? The 24 Oct canary covered only lexical symbol and call questions.
- Which of the other 5 vendor legs can still be collected, and does a re-run still need the browser-only constraint?

## Источник
- DR-ID `DR26-10-24-HUB-07-0721` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»

## Связано
- CodeIndexer
- semantic code search
- LSP navigation
- Tree-sitter structural search
- index freshness and invalidation
- false-confidence risk in index accounting
- reversible canary and rollback
- local-only embeddings and telemetry

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-10-25 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.

## ⚠️ Корректировка судьи DR (25.10.2026): ИНВЕРСИЯ
Утверждение «только промпт, выводов нет» неверно: `queue/DR26-10-24-HUB-07-0721/results/grok.md` (33 КБ, 24.10 00:16) содержит отчёт. Вывод Grok: оставить CodeIndexer; семантический кандидат на канарейку `cocoindex-code` 0.2.42 (e5-small); навигация `serena-agent` 1.7.0 (MIT; 2.1.0 GPL); уверенность [emerging], одна нога из 6, без кворума. Актуальная версия: insight-codeindexer-companions-20261025. Статус этой заметки: superseded_by эта корректировка.
