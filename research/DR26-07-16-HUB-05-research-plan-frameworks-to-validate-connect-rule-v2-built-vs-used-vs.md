---
dr_id: DR26-07-16-HUB-05
title: "Research plan: frameworks to validate 'Connect Rule v2' (built vs used vs valuable artifac"
date: 2026-07-16
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-16-HUB-05): Research plan: frameworks to validate 'Connect Rule v2' (built vs used vs valuable artifacts)

> Lays out a five-pass research plan comparing DORA, PMI Benefits Realization, Scrum, Lean/JIT pull, Theory of Constraints/Stage-Gate, OpenTelemetry and DLQ patterns to ground the Connect Rule's demand that senders own downstream consumption of what they build.

## Ключевые выводы
- Three distinct artifact states must be separated: built-but-not-consumed, consumed-but-not-effective, and effective-enough-to-keep-funding — 'used' does not automatically mean 'valuable'.
- Nielsen Norman Group warns usage metrics become vanity metrics unless tied to a KPI or system health signal; PMI's Benefits Realization Management framework explicitly separates outputs from realized benefits.
- Lean/TPS JIT and pull-system logic (build only what's needed, when needed, in the amount needed) is the cleanest ancestor of an anti-overproduction/anti-void rule for internal artifacts.
- DORA 2024 findings show internal platforms can raise individual/team productivity while simultaneously worsening change stability and throughput — 'more generated stuff' can still harm the system.
- Exploratory/learning work (set-based design, Toyota Woven City precedent) must be treated as a distinct class exempt from the consumption requirement, not a rule violation by default — the report will draw a hard line between delivery work and probe/learning work.
- The Toyota phrase 'the next process is your customer' — key bridge from TPS to internal pipeline thinking — currently only has a low-authority secondary source; a stronger primary Toyota/TQC citation is still needed.
- OpenTelemetry causal tracing (traces/spans) and AWS SQS dead-letter queues are identified as the cleanest named mechanisms for making silent/asynchronous internal artifact consumption observable.
- Source hierarchy for the final report: primary standards/institute research (DORA, PMI, Scrum Guide, AWS, OpenTelemetry, Microsoft docs) rank above recognized institutes (Lean Enterprise Institute, PDMA/Stage-Gate), which rank above practitioner books/vendor glossaries (Build Trap, Opportunity Solution Tree, What Matters/OKRs, Mixpanel/Pendo) — the latter are flagged [single-source]/[low-authority].

## Рекомендации / решения
- Build an output-to-outcome comparison matrix across Scrum's usable increment, PMI benefits realization, OKR key results, DORA user-centricity, and 'build trap' framing to see where each framework draws the completion→value line.
- Create an internal-artifact metric translation layer: script activation = first successful invocation by intended consumer; dashboard adoption = repeat use by target operator; queue time-to-value = enqueue→first-accepted-item latency; recommendation adoption = applied/surfaced ratio; recommendation realized value = downstream metric change after apply.
- Build a pull-vs-push control model using WIP limits, ticket aging caps, explicit request tokens, capacity-based release, and Stage-Gate/TOC go-kill-hold-recycle checkpoints to stop an AI assistant from producing speculative inventory faster than it can be consumed.
- Implement a minimal solo-operator observability pattern set: usage event logging, explicit consumer acknowledgment, staleness detection, dead-letter-queue/redrive, and periodic orphan sweeps.
- Write explicit decision logic distinguishing a 'delivery break' from 'no demand' for unused artifacts, using Stage-Gate's go/kill/hold/recycle outputs plus benefits ownership, elapsed time-to-first-use, and predeclared kill criteria.
- Ship the Top-5 one-day-implementable mechanisms: consumer-acknowledgment events, queue TTL + orphan sweep, an explicit 'who is the consumer?' field, apply-rate telemetry for suggestions, and a dead-letter path for unconsumed artifacts.

## Сущности
- **Люди:** Melissa Perri, Teresa Torres, John Doerr
- **Компании:** DORA, PMI (Project Management Institute), AWS, Microsoft, Toyota, Nielsen Norman Group, PDMA/Stage-Gate, Lean Enterprise Institute, Mixpanel, Pendo, Product School
- **Продукты/инструменты:** DORA 2024 report, PMI Benefits Realization Management Framework, Scrum Guide 2020, OKRs (What Matters), The Build Trap, Opportunity Solution Tree, JIT/pull system (TPS), Theory of Constraints, Stage-Gate, OpenTelemetry, AWS SQS dead-letter queues, Power BI usage metrics, Microsoft Adoption Score

## Открытые вопросы
- No strong primary Toyota/TQC-quality source yet for 'the next process is your customer' — currently only a low-authority secondary reference.
- Activation/adoption/engagement/time-to-value definitions are currently only backed by vendor sources (Mixpanel, Pendo, Product School) and need a research-backed source.
- OKR key-result-vs-task distinction relies on practitioner guidance (What Matters); a stronger institutional or original Grove/Doerr-adjacent source is still needed.
- The precise boundary where Connect Rule v2 should NOT apply (learning probe, hypothesis test, set-based exploration) needs one more source so the 'where this rule harms itself' section isn't hand-wavy.

## Источник
- DR-ID `DR26-07-16-HUB-05` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-16-HUB-05-research-plan-for-dr26-07-04-hub-13.md`

## Связано
- [[connect-rule-pipeline-ownership]]
- [[one-system-propagate]]
- [[DORA metrics]]
- [[benefits-realization-management]]
- [[lean-jit-pull-system]]
- [[theory-of-constraints]]
- [[stage-gate-process]]
- [[opentelemetry-observability]]
