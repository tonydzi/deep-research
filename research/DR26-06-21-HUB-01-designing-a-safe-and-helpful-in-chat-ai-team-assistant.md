---
dr_id: DR26-06-21-HUB-01
title: "Designing a Safe and Helpful In-Chat AI Team Assistant"
date: 2026-06-21
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-21-HUB-01): Designing a Safe and Helpful In-Chat AI Team Assistant

> The report answers how to make a founder/team-ops AI assistant materially more useful (able to 'do' things) without becoming an intrusive or unsafe policy-enforcement bot.

## Ключевые выводы
- Workplace AI adoption depends on removing real work (research, drafting, comparison, coordination), not on enforcing procedure; leading enterprise products converge on 'search + draft + workflow assistance,' matching the observed assistant workload.
- The safe path to more autonomy is separating work generation (search, compare, summarize, draft, maintain task state) from world-changing execution (spending money, placing orders, transmitting credentials, external commitments) — never jump straight to full autonomy.
- Unsolicited/proactive AI help can backfire: 2025 research shows it can reduce satisfaction via 'competence threat,' and poorly timed interruptions increase task time, errors, anxiety and workload — proactivity must be risk- and interrupt-cost-aware, not maximized.
- RAG alone is not memory: long-term task continuity needs persistent structured state (task/vendor/purchase entities, execution logs) across sessions, not just document retrieval; naive vector stores fail to answer 'who owes what' or 'what stage is this request in.'
- Fully autonomous browser/computer-use agents are unsafe for procurement: tested browser agents can be steered by malicious web content into credential theft, camera activation, impersonation, or file exfiltration with high attack success rates; dozens of privacy vulnerabilities were found in browser agents.
- Governance must be graduated/tiered, not binary — Gartner and NIST guidance both warn that 'fully locked down or fully trusted' agent governance fails; capability tiers with different approval/observability requirements are needed.
- 'Approval theater' is a real failure mode: if the human only approves the agent's summary of an action rather than the exact action, a compromised or misleading agent can describe one thing and execute another (consent-integrity research) — the approval UI must render the exact action from a trusted control plane.
- No dominant product owns the niche of a founder's Telegram-native team-assistant bot for household/ops/procurement coordination; nearest analogues are AI executive assistants (Lindy, Llewra), enterprise search/agents (Glean, Moveworks, Slack AI, MS 365 Copilot), procurement platforms (Zip, Levelpath), and orchestration infra (LangGraph, CrewAI, MCP).
- Algorithmic-management/surveillance research shows hidden productivity scoring or covert monitoring destroys trust; the bot must track work objects (tasks, vendors) transparently, not rank or surveil people.

## Рекомендации / решения
- Position the bot as a 'safe doer': researcher, packager, drafter, tracker, coordinator — never final spender, signer, credential holder, or external committer without approval.
- Adopt a 4-tier capability model: Read (no approval) → Prepare (build comparisons/drafts/packets, no approval) → Propose (suggest next steps, human can ignore/edit) → Execute (send/submit/spend — approval required only at true trust boundaries).
- Adopt a strict 4-mode behavioral policy: Act-and-report (reversible/low-risk), Offer-and-ask (interruptive/medium ambiguity), Ask-before-acting (money/credentials/external commitments/low confidence), Stay-silent (low-value nudges, duplicate reminders).
- Build a deterministic control plane around the LLM: use the LLM for interpretation/search/drafting/ranking, and a separate deterministic layer for policy checks, tool access, task-state transitions, logging, and approval interrupts (reference: LangGraph interrupt model, OpenAI Operator's handoff-on-sensitive-input approach).
- Split memory into four layers: knowledge retrieval (RAG w/ citations+permissions), task memory (open requests/owners/deadlines/blockers), commercial memory (vendors/prices/purchase history/rejections), and preference memory (founder/team preferences, tone, escalation rules).
- Prefer API-grade, read-only tools (price-extraction APIs, structured search/parsers) over unrestricted browser automation for shopping/vendor research; reserve browser automation for evidence-gathering or human-supervised preview only.
- Use a trusted approval mediator that shows exact action type, recipient, payload, attachments/tool args, cited evidence, and a diff on edits — the agent must never be the sole narrator of its own action.
- Roll out 'doing' capability incrementally: (1) add structured task state (Request/Task/Vendor/Product/Quote/Approval/DraftMessage/Decision entities), (2) add a 'Prep Pack' workflow (comparison + pricing + vendor notes + recommended option + draft approval message) as the first doer feature, (3) add internal-only write actions (create/update tasks, tentative assignments, draft saving) before any external actions, (4) only then add approval-mediated consequential actions (send drafted email, submit form) with full pre/post payload logging.
- Measure success via adoption/workflow metrics (share of requests converted to structured tasks, time-to-approval-ready packet, approval turnaround, % packets accepted with minor edits, manual rework rate, proactive-message engagement ratio) rather than answer quality alone.

## Сущности
- **Люди:** —
- **Компании:** Moveworks, Glean, Levelpath, Zip, Lindy, Llewra, Slack, Microsoft, OpenAI, LangChain, CrewAI, Gartner, NIST, OWASP, Meta
- **Продукты/инструменты:** LangGraph, AutoGen, MCP (Model Context Protocol), Slack AI, Microsoft 365 Copilot/Agent Store, OpenAI Operator, Glean agent memory/orchestration, Moveworks Enterprise Search

## Открытые вопросы
- Public evidence on Telegram-native enterprise assistants remains thin compared to Slack/Teams ecosystems — unclear how well findings transfer to a Telegram-based founder-ops bot.
- Several key 2025–2026 sources (memory architectures, browser-agent security, proactive-assistant studies) are preprints, not peer-reviewed — findings could shift.
- Whether/when to allow post-approval autonomous execution through a 'trusted mediator' for outbound messages is left as a future decision, not resolved now.
- The agent-protocol/browser-tool market is moving quickly, so specific product/tool recommendations (e.g., which MCP servers, which browser-agent framework) may become outdated fast.

## Источник
- DR-ID `DR26-06-21-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-21-HUB-01-designing-a-safe-and-helpful-in-chat-ai-team.md`

## Связано
- [[human-in-the-loop approval design]]
- [[agent governance tiers]]
- [[structured task/operational memory vs RAG]]
- [[proactive AI / interruption cost]]
- [[consent integrity for agent actions]]
- [[MCP security and tool poisoning]]
- [[browser-use agent safety]]
- [[algorithmic management and surveillance trust]]
