---
dr_id: DR26-07-04-ZB-03
title: "Dynamic lead temperature bands + RFM-style reactivation scoring for a founder-led relation"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-ZB-03): Dynamic lead temperature bands + RFM-style reactivation scoring for a founder-led relationship sales motion

> The report answers how to design a lead-temperature and dormant-lead reactivation scoring model for a founder-led, Telegram-driven sales motion with stale CRM statuses and low outreach volume.

## Ключевые выводы
- Best-practice CRM vendors (HubSpot, Microsoft, Salesforce) have moved away from single opaque 0-100 scores toward separating fit (static gate) from engagement/recency (dynamic priority), and toward event-driven scores with built-in decay and explainable factor breakdowns.
- Common Room's 2026 account-prioritization guidance recommends ranking by recency, frequency, and depth of signals with decay, and using action bands like 'today/this week/this month' instead of static tiers — directly matching a founder capacity-constrained outreach motion.
- Recommended architecture is a Hybrid Relationship Heat model with two outputs per contact: a 5-band Temperature (Hot/Warm/Lukewarm/Cold/Archived, plus Excluded) for operational state, and a separate Reactivation Priority score (Recency 35%/Frequency 20%/Depth 25%/Resurgence 20%, minus penalties) for ranking the dormant pool — replacing RFM's 'Monetary' with 'Depth' since deal value isn't meaningful at lead stage.
- All time-sensitive signals should use exponential decay (decayed_value = raw_weight * 2^(-days/half_life)) with proposed starting half-lives: direct reply 45 days, completed call/meeting 60 days, lead-initiated DM 75 days, shared-chat activity 21-24 days, historical relationship memory 365 days, unanswered-outbound penalty 30 days — these are implementation defaults requiring calibration on real outcomes, not universal constants.
- Deterministic rules should handle all countable/structural features (reply count, days since last reply/call, who wrote last, unanswered streaks, thread depth, shared-chat activity) because research on enterprise email reply behavior shows historical interaction and temporal features dominate reply prediction — LLMs should only be used for the narrow subset rules can't do: sentiment/intent classification, deferral date extraction ('circle back in September'), referral detection, and ambiguous-tone resolution.
- Predictive/supervised ML scoring should NOT be built now: current CRM statuses are stale/dirty labels, and Microsoft/Salesforce both require a meaningful volume of clean recent qualified/disqualified examples before predictive scoring is reliable — recommendation is to run the rules-based system for 8-12 weeks, log real outcomes (positive_reply, booked_call, opportunity_created), then consider supervised ML on clean labels.
- Reactivation priority bands should map directly to action: Now (score >=75), This week (60-74), This month (45-59), Park (<45) — mirrors signal-based GTM practice better than percentile/decile ranks for a founder who can act on few contacts per day.
- Five identified anti-patterns to avoid: over-engineering with 40+ weights nobody can explain; folding fit and temperature into one number; omitting decay (old trust falsely reads as still-live); using LLMs as a substitute for basic data/event engineering; training predictive ML on dirty/inconsistent status labels.
- Minimum viable rollout: backfill events from SQLite/call logs/Telegram/shared-chat activity -> compute deterministic features nightly -> run one lightweight LLM classifier only on latest inbound messages/ambiguous threads -> write temperature_band + reactivation_priority + a top-3-factor explanation field per person -> route only Hot/Warm/Now-This-week contacts into the founder queue -> review weekly, recalibrate half-lives quarterly -> delay predictive ML.

## Рекомендации / решения
- Implement fit as a binary/gate filter, not a graded score — stop debating fine-grained ICP fit differences.
- Store temperature as the 5 bands (Hot/Warm/Lukewarm/Cold/Archived) driven by event-based promotion/decay rules, not a fabricated precision 0-100 number.
- Build a separate Reactivation Priority score (Recency/Frequency/Depth/Resurgence formula given in report) specifically for ranking the large dormant/old-called-contact pool.
- Use the given pseudocode (compute_temperature, compute_reactivation_priority, daily_founder_queue) as the concrete starting implementation, producing a daily top-5 founder outreach queue.
- Keep an explicit minimal per-contact JSON schema (calls_total, replied_ever, days_since_last_reply, shared_chat_spike, latest_inbound_intent, defer_until, etc.) computed nightly from Telegram + call history.
- Reserve LLM usage strictly for sentiment/intent classification, deferral-date extraction, referral detection, and ambiguous short-reply interpretation — never for counting, recency math, or reply detection (those are deterministic DB operations).
- Do not start predictive ML on current CRM data; first run the rules-based system for 8-12 weeks to accumulate clean labeled outcomes, then reconsider supervised scoring.
- Recalibrate the proposed half-life constants against real reactivation outcomes rather than treating them as fixed.

## Сущности
- **Люди:** —
- **Компании:** HubSpot, Salesforce, Microsoft, Common Room, Outreach, Notion, Zapier, Superhuman, BrightTALK, Microsoft Research/SIGIR
- **Продукты/инструменты:** Einstein Behavior Scoring, Microsoft Dynamics 365 lead/opportunity scoring, Microsoft Customer Insights scoring model builder, HubSpot lead scoring tool / score history, Common Room signals & account-prioritization platform, Outreach sentiment classification

## Открытые вопросы
- Exact half-life values (45/60/75/21-24/365/30 days) are implementation defaults with only medium confidence — need calibration against Anton's own Telegram/call reactivation outcomes.
- When exactly the org will have accumulated enough clean recent labels (positive_reply/booked_call/opportunity_created) to safely move to predictive ML remains undetermined (report suggests 8-12 weeks as a starting point, low confidence).
- The report leans on several vendor-reported case studies (Notion, Zapier, Superhuman, BrightTALK) whose lift numbers are vendor-sourced and not independently verified.

## Источник
- DR-ID `DR26-07-04-ZB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- lead-scoring-model
- rfm-reactivation-scoring
- signal-decay-half-life
- founder-led-sales-motion
- crm-data-hygiene
- telegram-crm-signals
- rules-vs-llm-feature-engineering
- insight-DR-DR26-07-28-HUB-23-2339-плейбук-реактивации-8-644-спящих-1-1-контактов-чер — общая тема RFM/scoring-приоритизации реактивации спящих лидов
