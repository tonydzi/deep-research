---
dr_id: DR26-07-02-HUB-12
title: "Claude Code plugin/skill marketplace distribution channels compared"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-12): Claude Code plugin/skill marketplace distribution channels compared

> Which discovery/distribution channels exist for Claude Code plugins and skills, and how should a new tool prioritize submission across them?

## Ключевые выводы
- Anthropic's official marketplace (claude-plugins-official) is highest-trust but gated: 31.4k stars, 3.4k forks; top plugins reach 100k+ installs (GitHub MCP 291,655; Slack MCP 80,897); submissions go through automated + manual review and earn an 'Anthropic Verified' badge.
- Alireza Rezvani's claude-skills marketplace has 266 skills, ~19.6k GitHub stars, and ~184.3k total installs; installed via `/plugin marketplace add alirezarezvani/claude-skills`; vetting is light (PR review by Alireza, no formal gate).
- Jeremy Longshore's TonsOfSkills (claude-code-plugins-plus-skills) has 453 plugins/3,032 skills, 14,130 monthly NPM downloads across 425 packages, and 7.5k installs tracked by CrossAI for a 13-skill subset; enforces a strict 100-point rubric and 8-field frontmatter schema validated via `ccpi validate`.
- CrossAI Tools (claudemarketplaces.com, run by Mert Düzgün) is an auto-updated aggregator with 300k+ monthly visitors indexing ~21,600 skills and 2,500 marketplaces; no submission process — it auto-detects valid marketplace.json/SKILL.md files and ranks by usage, funded by ads.
- travisvn/awesome-claude-skills is a curated GitHub list (13.9k stars, 532 open PRs) that requires 'clear value' and social proof (~10+ stars) for inclusion, rejects promotional/SaaS-wrapper submissions, and provides visibility but no install mechanism.
- All five channels are free for developers; no paid placements exist anywhere, though CrossAI Tools sustains itself via ad revenue.
- Community debate: curated channels (Anthropic, TonsOfSkills, Travis) optimize for quality/signal while aggregators (CrossAI, generic lists) optimize for quantity/usage metrics — a highly differentiated but low-traction tool can be excluded by Travis's social-proof rule or buried in CrossAI's data.
- Reddit users reportedly found 50+ hidden official Anthropic plugins by inspecting configs, highlighting that the official gated marketplace limits exposure even as it maximizes trust.
- Common failure modes: zero initial traction keeps a tool invisible on usage-ranked channels; ignoring channel-specific submission rules (e.g., Travis's anti-promotional rule, Longshore's frontmatter validator) gets submissions rejected; generic 'PKM clone' skills get filtered out by curators; lack of ongoing maintenance risks delisting or breakage on new Claude versions.

## Рекомендации / решения
- Prioritize Anthropic's official marketplace first if the tool meets security/quality criteria — it has the largest built-in user base, highest trust, and verified-badge exposure, despite a longer review cycle.
- Submit to Alireza Rezvani's claude-skills marketplace second — proven ~184k installs, low-friction PR-based submission, easy CLI install mechanics.
- Submit to Jeremy Longshore's TonsOfSkills third, but only if able to meet its 100-point rubric and 8-field frontmatter schema (validate via `ccpi validate`) — it filters low-effort entries but reaches a quality-conscious audience.
- Ensure inclusion in CrossAI Tools by publishing a valid marketplace.json or SKILL.md at repo root — no formal submission needed, it's an easy win given 300k+ monthly visitors.
- Use travisvn's awesome-claude-skills list as an ancillary listing after building some credibility/stars (~10+) — it signals quality but doesn't drive installs directly; avoid any promotional tone.
- Cross-post across all channels simultaneously rather than relying on one, and keep plugin.json/version metadata synchronized across listings.
- Prepare production-ready packaging before any submission: plugin.json/SKILL.md, README, examples, tests, and channel-specific schema compliance.
- Plan for ongoing maintenance (respond to PR comments, re-validate compatibility on new Claude releases) to avoid staleness or delisting.

## Сущности
- **Люди:** Alireza Rezvani, Jeremy Longshore, Travis (travisvn), Mert Düzgün
- **Компании:** Anthropic, CrossAI Tools
- **Продукты/инструменты:** claude-plugins-official, alirezarezvani/claude-skills, jeremylongshore/claude-code-plugins-plus-skills (TonsOfSkills), CrossAI Tools (claudemarketplaces.com), travisvn/awesome-claude-skills, ccpi CLI, GitHub MCP, Slack MCP

## Открытые вопросы
- Exact adoption/install data for the official Anthropic marketplace remains private beyond what the public UI reveals.
- Whether GitHub stars or install/download counts are the better popularity signal remains debated among the channels.
- Extent and content of the 50+ hidden official Anthropic plugins found via config inspection is unverified/unclear.
- How CrossAI Tools' usage-based ranking algorithm precisely weights 'actively used' extensions is not fully disclosed.
- Long-term sustainability of community marketplaces (Alireza's, Longshore's) if maintainers stop updating is unresolved.

## Источник
- DR-ID `DR26-07-02-HUB-12` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- claude-code-plugin-ecosystem
- skill-distribution-channels
- marketplace-curation-vs-aggregation
- claude-plugins-official
- ak47-simplicity
- insight-DR-DR26-07-02-HUB-02-discovery-and-distribution-channels-for-claude-cod — почти-дубликат инсайта из того же DR-B, тот же список каналов и выводы
