---
dr_id: DR26-07-04-HUB-13
title: "Connect rule v2: consumption/adoption test for built-but-unused internal artifacts"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-13): Connect rule v2: consumption/adoption test for built-but-unused internal artifacts

> Researches how to harden the 'Connect' rule (accountability doesn't end at handoff) with a consumption test that separates built-but-unused artifacts from genuinely valuable ones, for a solo operator running AI-driven pipelines.

## Ключевые выводы
- Build Trap framing (Melissa Perri): an artifact is not 'done' at build/handoff — only at observed downstream consumption + effect; unconsumed items must be surfaced, not buried in a backlog.
- DORA 2024-2025 paradox: a 25% increase in AI adoption correlated with a 7.2% decrease in software delivery stability — AI lets an operator generate more artifacts than can be reviewed/integrated/consumed, so output metrics climb while outcome metrics flatline.
- 'Next process is your customer' (Ishikawa, TPS/Lean, 1981) reframes internal handoffs as external-client-grade relationships — upstream is not done until the downstream consumer (human or AI agent) successfully uses the output.
- Theory of Constraints / Drum-Buffer-Rope: an unconsumed queue is excess inventory (wasted compute/tokens/attention); the bottleneck (Drum) sets pace, a small capped buffer protects it, and new work releases only when the constraint clears an item (Rope) — a structural pull system against building into the void.
- Two distinct root causes for 'unused': Delivery Break (wanted but broken handoff — fix the connection) vs Zero Demand (flawless but nobody needs it — apply predefined Kill Criteria per Stage-Gate to beat sunk-cost/escalation-of-commitment bias).
- Dead Letter Queue pattern (AWS SQS, Confluent) is the concrete mechanism: unconsumed/failed items quarantine after a TTL and demand mandatory triage — a DLQ that nobody checks becomes an ignored graveyard, defeating its purpose.
- The consumption test can harm itself in two ways: (1) it chills exploratory/R&D work that has no consumer yet (the 'Fuzzy Front End' of Agile-Stage-Gate) — needs a time-boxed Proof-of-Concept exemption with auto-kill on expiry; (2) it's gameable via Goodhart's Law (an agent can fake an 'ACK' without real use) — fix by measuring state-change/outcome, not activity/read-receipts.
- Value Realization Offices (VROs, 2025-2026 enterprise trend, Kyndryl) are replacing PMOs — accountability is measured by economic/behavioral impact, not by tech shipped, reinforcing the outcome-over-output shift industry-wide.
- Recommended architecture (Gemini's synthesis): 'Active Consumer Handshake' — async generation + strict WIP limits + mandatory DLQ alert on anything unconsumed past threshold, treated as a stop-the-line Andon-cord event, not a passive log.
- Recommended policy (Grok's synthesis): tiered ~80/20 allocation — Production track under a strict consumption gate + Exploration track that is timeboxed, tracked on learning metrics, with mandatory upfront kill criteria — protects both reliability and innovation.

## Рекомендации / решения
- Adopt an Active Consumer Handshake / DLQ-routing pattern: TTL-to-DLQ triage alarm (e.g. 48h) on queues/suggestions, with a daily forced-triage habit — 'day can't end until DLQ is zero'.
- Add a mandatory 'who is the consumer + pull signal + kill criteria' field before building any new artifact/automation — enforces pull and surfaces speculative work early.
- Instrument a two-phase status flag (status='built' → status='done' only when the downstream consumer executes successfully) and dashboard the built→done latency (last-mile time-to-value).
- Run a weekly/periodic Orphan Sweep: flag artifacts (scripts/dashboards/queue items) with no execution/view in 14-30 days (production) or 90 days (experimental) for review/kill/archive.
- Set explicit, predeclared Kill Criteria (e.g., 0% activation over a rolling 14-day window → auto-disable pipeline/archive) so termination is a threshold decision, not an emotional/sunk-cost one.
- Carve out a protected exploration budget (~20-30%) with its own learning-metric track and a hard expiry/kill so R&D isn't strangled by the consumption gate.
- Track outcome-style KRs (activation rate, time-to-value, sustained-use rate) instead of raw output counts, and verify outcomes via actual state-changes (DB record updated, ticket closed) rather than trusting a self-reported 'used' flag, to resist Goodhart's-Law gaming.
- Treat the ChatGPT vendor section as incomplete (DR was interrupted at step 2/5, delivered only the scoping/framework map) — consider re-running it for the missing six analytical sections and final decision.

## Сущности
- **Люди:** Melissa Perri, Teresa Torres, John Doerr, Eliyahu Goldratt, Robert G. Cooper, Kaoru Ishikawa, Eric Ries
- **Компании:** DORA, PMI, Toyota (TPS), AWS, Microsoft, Kyndryl, Confluent, Stage-Gate/PDMA, Lean Enterprise Institute, getdx.com, AugmentCode
- **Продукты/инструменты:** Scrum Guide / Definition of Done, OKRs, OpenTelemetry, AWS SQS Dead Letter Queue, Power BI usage metrics, Microsoft Adoption Score, Mixpanel, Pendo, DevEx Core 4, Opportunity Solution Tree

## Открытые вопросы
- No canonical Toyota-primary citation found for 'the next process is your customer' — currently only a [low-authority] secondary source (all three vendors flagged this gap identically).
- No research-backed (non-vendor) source distinguishing activation vs adoption vs engagement vs time-to-value for INTERNAL artifacts — current definitions come only from PLG vendors (Mixpanel/Pendo/Product School), flagged [emerging]/[low-authority].
- Outcome-style OKR Key Results vs task-style fake KRs still rests on practitioner-only sourcing (Doerr's 'What Matters'), no stronger institutional treatment found.
- Exact governance boundary for when the consumption-test should be suspended for exploratory/probe work is directionally supported (Lean set-based design, Agile-Stage-Gate Fuzzy Front End) but not fully nailed down.
- ChatGPT's report was interrupted mid-run (step 2/5) — the six analytical sections, Top-5 mechanisms, and decision-ending it planned were never produced; only Gemini and Grok delivered complete syntheses.

## Источник
- DR-ID `DR26-07-04-HUB-13` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-04-HUB-13-connect-rule-v2-chatgpt.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-04-HUB-13-connect-rule-v2-gemini.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-04-HUB-13-connect-rule-v2-grok.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-16-HUB-05-research-plan-for-dr26-07-04-hub-13.md`

## Связано
- [[connect-rule-pipeline-ownership]]
- [[build-trap]]
- [[theory-of-constraints]]
- [[dead-letter-queue-pattern]]
- [[kill-criteria-stage-gate]]
- [[pull-system-lean-tps]]
- [[value-realization-office]]
- [[goodharts-law]]
