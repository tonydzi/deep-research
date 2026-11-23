---
dr_id: DR26-11-23-HUB-01
title: "Prior art for rule-table AI instruction canons (per-rule versioning, judge typing, restate"
date: 2026-11-23
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-23-HUB-01): Prior art for rule-table AI instruction canons (per-rule versioning, judge typing, restatement exam)

> Does any public system or paper already combine the nine elements of Anton's table-based, versioned, multi-LLM-tested AI instruction canon, and where is the unclaimed territory?

## Ключевые выводы
- All six arXiv papers Anton cited exist; two corrections were needed: Sparrow is arXiv 2209.14375, and 'Context Rot' is a Chroma report, not arXiv 2504.15977.
- No public system combines more than four of the nine elements of the canon design.
- The closest prior art is boshu2/agentops (practice ledger), the OpenAI Model Spec, and rulesync.
- Unclaimed territory: per-rule version columns, three-vendor judge typing, and a compile-gated multi-LLM restatement exam.
- Main risk: the ~850-rule always-loaded canon is far beyond every tested instruction-density range in the literature.
- The report is 40 pages with 152 web-verified sources, sections A–H, and a 9x12 comparison table in landscape.
- Two leads could not be verified because the web-tool call limit was exhausted, so they are marked unverified in B(f)/H and not cited.

## Рекомендации / решения
- Before publishing, check the two unverified leads flagged in B(f)/H.
- Run the two-week adherence-instrumentation pilot, which gates the G.3 experiment.
- Position the work around the unclaimed elements: per-rule version columns, three-vendor judge typing, and the compile-gated multi-LLM restatement exam.
- Correct the Sparrow ID (2209.14375) and the Context Rot attribution (Chroma report) in any published citations.

## Сущности
- **Люди:** —
- **Компании:** OpenAI, Chroma
- **Продукты/инструменты:** boshu2/agentops, OpenAI Model Spec, rulesync, Sparrow (arXiv 2209.14375), Context Rot (Chroma report), docx skill, VLM CLI, python-docx

## Открытые вопросы
- Two promising leads in B(f)/H remain unverified because of the web-call cap.
- How does an always-loaded canon of about 850 rules behave on adherence, given that it exceeds all tested density ranges?
- Does the G.3 experiment show measurable benefit once adherence instrumentation exists?

## Источник
- DR-ID `DR26-11-23-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- rule-table canon
- instruction density and adherence
- LLM-as-judge typing
- multi-vendor restatement exam
- per-rule versioning
- always-loaded context cost
- prior art survey

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-23 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
