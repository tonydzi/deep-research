---
dr_id: DR26-07-21-HUB-02-2124
title: "Grok Heavy as a Deep Research engine: architecture, prompting, limits, risks"
date: 2026-07-21
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-21-HUB-02-2124): Grok Heavy as a Deep Research engine: architecture, prompting, limits, risks

> Investigated how to get DR-grade research out of Grok 4 Heavy (no dedicated Deep Research button), covering its multi-agent architecture, best prompting patterns, hallucination risks, quotas, UI mechanics, privacy pitfalls, and how it compares to ChatGPT Pro/Gemini Deep Research.

## Ключевые выводы
- Grok.com has no dedicated 'Deep Research' button — the mode selector (Auto/Fast/Expert/Heavy) is the interface; Heavy (SuperGrok Heavy, ~$300/mo) is the closest equivalent, using parallel test-time compute with 4-16 sub-agents that research, debate and synthesize simultaneously (roles cited by Gemini's report: Coordinator/Captain, Research Engine/Harper, Logician/Benjamin, Contrarian/Lucas).
- DeepSearch/DeeperSearch (Grok 3-era dedicated agentic-search toggles) were deprecated/removed as UI buttons by ~August 2025 and folded into native tool use + Heavy multi-agent orchestration.
- Citation hallucination is the main residual weakness: benchmarks cited put standard Grok 4 at 64% and Grok 4.5 at 54% hallucination rate on the AA-Omni benchmark vs Claude 3.5 Sonnet's 37.3%; Grok 4.3 high-reasoning drops to ~25%. General citation-hallucination rates across LLMs range 14-95% depending on task.
- Effective prompting requires explicit anti-hallucination guardrails: demand tool use, set a minimum source count (12-25+), require verbatim quotes + exact URLs actually opened by the tool, forbid inventing URLs/DOIs, and force a post-draft verification pass flagging unverified citations.
- Heavy is worse than Expert/other vendors for: rigid/compliance-style linear deliverables, time-sensitive quick tasks (Heavy can take 5-20 min, up to 10 min per query), and low-value queries that waste quota.
- Quotas are a shared weekly usage pool (% shown in Settings→Usage) rather than fixed hard caps; one vendor section cites practitioner estimates of ~500 general messages/day and ~50 heavy/deep-research queries/day (soft caps, reset at midnight UTC), context window 256k-428k tokens depending on build.
- A documented 'silent degradation' bug exists: the UI can silently revert the mode selector from Heavy/Expert back to Auto/Fast under load or after a complex task, so users must manually re-verify the mode and check for a 'Thought for X minutes' trace before trusting output as genuine Heavy research.
- Grok.com lacks native bulk export; long research must be exported via 'export as PDF' prompts or third-party browser extensions (AI Toolbox, AI Chat Exporter); the Share button creates a PUBLIC link that has historically been indexed by search engines — Anton-relevant privacy risk requiring 'Allow chat link sharing' to be disabled and links at grok.com/share-links to be revoked.
- xAI's Acceptable Use Policy (effective ~26 June 2026) bans automated/scripted/bot access to grok.com (including Selenium/Puppeteer or 'Auto Grok' extensions), circumventing rate limits, and using outputs to distill competing models; legitimate automation must go through the paid API.
- Head-to-head: Grok Heavy wins on real-time X/Twitter data, search speed, and multi-agent lateral/contrarian analysis; ChatGPT Pro Deep Research wins on structured formatting and lowest hallucination/compliance; Gemini Deep Research wins on Google Workspace integration and Search-index-grounded recency. Best practice is a 3-vendor DR pipeline with human citation spot-checks.

## Рекомендации / решения
- Use the reusable anti-hallucination prompt template (decompose into sub-questions, mandate 15-25+ sources, require quote+URL per claim, demand a post-draft verification pass) whenever running a DR-grade query in Grok Heavy.
- Before trusting any Heavy output, manually confirm the mode still shows 'Heavy' (not silently reverted to Auto/Fast) and that a multi-minute 'Thought for X' / multi-agent trace is present — an instant citation-free answer means re-run.
- Disable 'Allow chat link sharing' and 'Improve the model' in Settings→Data Controls before doing proprietary research on Grok, and periodically audit/revoke links at grok.com/share-links.
- Route real-time/X-centric/OSINT/contrarian-angle research to Grok Heavy; route rigid-format, low-hallucination-tolerance, or Workspace-integrated tasks to ChatGPT Pro / Gemini Deep Research instead.
- Keep usage human-paced (no scripting/browser automation against grok.com) to stay compliant with xAI's AUP; monitor Settings→Usage % to pace Heavy runs against the shared weekly pool.
- Always manually verify 3-5 key citations/quotes after any Heavy report before treating it as ground truth.

## Сущности
- **Люди:** —
- **Компании:** xAI, OpenAI, Google
- **Продукты/инструменты:** Grok 4 Heavy, SuperGrok Heavy, Grok Expert mode, Grok Auto/Fast mode, DeepSearch, DeeperSearch, Grok Imagine, ChatGPT Pro Deep Research, Gemini Advanced/Pro Deep Research, AI Toolbox, AI Chat Exporter

## Открытые вопросы
- Exact current per-day query/message caps are not officially published by xAI — figures cited (~500 messages/day, ~50 heavy queries/day) are practitioner estimates, not confirmed limits.
- Whether the 'silent degradation' fallback-to-Auto bug has been fixed since these reports were written is unverified.
- GDPR-driven suspension of EU training-data use (via Irish DPC/noyb complaint) may have changed by the time of use — needs re-check for current jurisdictional data-handling status.
- Actual current context window (256k vs 428k tokens) differs between the two vendor sections and may depend on specific model build/version at time of use.

## Источник
- DR-ID `DR26-07-21-HUB-02-2124` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-21-HUB-02-grok-heavy-playbook-grok.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-21-HUB-02-grok-heavy-playbook-gemini.md`

## Связано
- [[alpha-protocol-recall-plus-dr]]
- [[dr-numbering-registry]]
- [[epistemic-neutrality]]
- [[credential-store]]
