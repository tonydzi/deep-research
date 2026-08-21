---
dr_id: DR26-07-20-HUB-02
title: "Reddit distribution playbook for AI/engineering subreddits (r/LocalLLaMA, r/ClaudeAI, r/AI"
date: 2026-07-20
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-20-HUB-02): Reddit distribution playbook for AI/engineering subreddits (r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/MachineLearning, r/SideProject)

> Reverse-engineers moderation rules, tone, and winning-post patterns of five AI/engineering subreddits to design a compliant Reddit rollout for a technical founder's multi-machine Claude agent fleet build-log.

## Ключевые выводы
- Each subreddit has a distinct self-promo tolerance and required framing: r/LocalLLaMA demands open-source/offline-only content (a cloud-dependent Claude fleet post would get downvoted/removed there); r/ClaudeAI rewards MCP servers/hooks framed around token economics; r/AI_Agents wants architecture/failure-mode post-mortems; r/MachineLearning requires academic [P]/[D]/[R] tagging and zero commercial framing with a Limitations section; r/SideProject is the only one that welcomes overt self-promotion if told as a founder story with MRR/metrics.
- AI-generated text ('delve', 'crucial', 'tapestry', perfect symmetric bullet lists, 'furthermore'/'moreover') is instantly flagged by moderators and users as low-effort marketing and destroys credibility; native markers include lowercase casualness, 'tl;dr' placement, 'Edit:' notes, self-deprecating swearing, and asymmetric formatting.
- Account warm-up follows a 4-phase, 60-day incubation path: Phase1 (days1-7) low-stakes comment karma 50-100 in generic subs, no links; Phase2 (days8-29) domain-specific commenting in target subs to 300+ combined karma; Phase3 (day30+) soft launches in r/SideProject spaced 48-72h apart; Phase4 (day60+) eligible for high-karma-gate subs like r/MachineLearning (100-300 karma, 60-day age requirement).
- Shadowban triggers: simultaneous cross-posting identical content to 5+ subreddits, shortened URLs (bit.ly), 100% submission-to-comment ratio, and sudden link-dropping by a previously dormant account.
- The 9:1 rule governs sustainable self-promotion: 90% of account activity must be non-promotional value-add (comments, troubleshooting), only 10% promotional; the 'Sandwich Method' answers the OP's question fully in 200-300 words before an optional one-line product mention at the end.
- Astroturfing (pretending to discover your own tool) is a fatal, bannable error; explicit disclosure ('Full disclosure: I built this') is mandatory and respected by these communities, while defensiveness when accused of shilling should be avoided in favor of acknowledging the critique.
- Common ban patterns: generic 'revolutionary tool' feature pitches, and sockpuppet accounts asking 'what tool is this?' answered by the main account (caught via IP/behavioral matching, causing sitewide suspension).
- Recommended posture is Option C 'The Transparent Builder' (optimal balance): build karma via genuine 9:1 engagement, then post deep architectural post-mortems with disclosed affiliation, tailored per-subreddit — rejected alternatives were Option A aggressive simultaneous cross-posting (near-certain bans) and Option B open-source-only purism (high credibility but near-zero paid-SaaS conversion).

## Рекомендации / решения
- Do NOT post the multi-machine Claude fleet build-log to r/LocalLLaMA at all — it conflicts fundamentally with the community's local-only, anti-cloud-vendor ideology.
- For r/ClaudeAI: reframe as 'Using MCP and hooks to sync a second-brain knowledge vault across a fleet of Claude agents', cut generic agent theory, include exact system prompts/MCP configs/token-saving metrics, use Project flair.
- For r/AI_Agents: reframe as 'Architecture breakdown: Running a multi-machine Claude agent fleet without losing state', include a system diagram, candid failure modes (e.g. race conditions), use Discussion/Architecture flair, cut marketing narrative entirely.
- For r/MachineLearning: reframe as '[P] An open-source implementation of distributed state-syncing for LLM agent fleets', strip all startup/product/UI framing, link only to GitHub, add latency/context-degradation metrics and an explicit Limitations section.
- For r/SideProject: reframe as a founder-journey story ('I was tired of Claude losing my project context, so I built...'), include MRR/usage metrics, build time, and a frictionless demo video, use Showcase flair.
- Run every draft through the Master Draft Checklist (delete marketing buzzwords/AI-slop phrasing, disclose affiliation early, deliver 80% of value natively in-text, place links near the end, verify correct flair and karma/age thresholds) before submitting.
- Sequence the incubation path machine-side: 7 days sandbox commenting, then ~3 weeks domain commenting to reach 300+ karma, then soft-launch on r/SideProject at day 30+, only attempt r/MachineLearning at day 60+.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI
- **Продукты/инструменты:** Claude Code, Claude, Model Context Protocol (MCP), CLAUDE.md, LangGraph, PydanticAI, OpenClaw, AutoModerator, BotDefense, r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/MachineLearning, r/SideProject

## Открытые вопросы
- Exact numeric conversion/engagement outcomes of following the 'Transparent Builder' strategy are not measured in the report — only qualitative risk/credibility tradeoffs are given.
- How Reddit's Contributor Quality Score (CQS) routing algorithm precisely weights actions is not detailed, only that it exists and can trigger silent shadowban-level removals.
- Whether the multi-machine Claude fleet project actually has an open-source component to link (required for r/LocalLLaMA/r/MachineLearning strategies) is assumed, not confirmed in the report.

## Источник
- DR-ID `DR26-07-20-HUB-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- everything-becomes-content
- content-cascade-auto-execute-never-ask
- short-text-when-unreviewed
- no-public-content-without-natasha-ok
- cofounder-identity
