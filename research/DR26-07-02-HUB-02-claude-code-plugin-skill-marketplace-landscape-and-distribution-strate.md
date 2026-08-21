---
dr_id: DR26-07-02-HUB-02
title: "Claude Code plugin/skill marketplace landscape and distribution strategy"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-02): Claude Code plugin/skill marketplace landscape and distribution strategy

> Which discovery channels exist for Claude Code plugins/skills, how much reach each has, and how a new third-party tool should get listed across them.

## Ключевые выводы
- Anthropic's official 'claude-plugins-official' marketplace is the highest-trust channel (31.4k GitHub stars, 3.4k forks); top plugins show huge install counts (GitHub MCP: 291,655; Slack MCP: 80,897), but listing requires an application, automated screening and manual review, earning an 'Anthropic Verified' badge.
- Alireza Rezvani's community marketplace (alirezarezvani/claude-skills) has 266 skills, ~19.6k GitHub stars, and ~184.3k total installs (per CrossAI Tools) — installed via `/plugin marketplace add alirezarezvani/claude-skills`; submission is informal PR-based with no formal vetting.
- Jeremy Longshore's TonsOfSkills (jeremylongshore/claude-code-plugins-plus-skills) has 453 plugins/3,032 skills, 425 npm packages, 14,130 monthly npm downloads, and ~7.5k CrossAI-tracked installs across 13 listed skills; enforces a strict 100-point rubric and 8-field frontmatter schema validated via the `ccpi` CLI.
- CrossAI Tools (claudemarketplaces.com, run by Mert Düzgün) is an auto-updated aggregator with 300k+ monthly visitors, indexing ~21,600 skills and 2,500 marketplaces by scanning GitHub for valid marketplace.json/SKILL.md; no direct submission process, ranks by usage ('only actively used extensions are listed'), monetized via ads.
- Travisvn's 'awesome-claude-skills' GitHub list (13.9k stars) is a curated pointer list, not a distribution channel — requires 'clear value' and social proof (~10+ GitHub stars) or PRs are auto-closed; explicitly rejects promotional/SaaS-wrapper submissions.
- All five channels are free to list on (no paid placements anywhere); monetization only appears indirectly via CrossAI's ad revenue.
- Core tension identified: curated/gated channels (Anthropic, Longshore, Travis) optimize for quality/trust but risk excluding differentiated-but-low-traction tools, while open aggregators (CrossAI, Alireza) optimize for reach/quantity and can let generic clones through.
- Common failure modes: zero-install tools get algorithmically buried (CrossAI, Travis auto-reject), rule-violating submissions (SaaS wrappers, missing frontmatter) get rejected outright, poor docs/security issues kill trust, and abandoned community marketplaces cause listed plugins to break on new Claude versions.

## Рекомендации / решения
- Prioritize Anthropic's official marketplace first if the tool meets its security/quality bar — prepare a complete plugin.json, SKILL.md, documentation, and any required security/privacy audit before submitting via the official form.
- List on Alireza Rezvani's claude-skills marketplace second (proven ~184k installs, easy CLI install, PR-based submission with his repo's documentation style).
- Submit to Jeremy Longshore's TonsOfSkills third, but only if able to meet its 100-point rubric and 8-field frontmatter schema; validate locally with `ccpi validate`/`ccpi install` before the PR.
- Ensure the repo has a valid marketplace.json or root-level SKILL.md so CrossAI Tools auto-indexes it (no manual submission possible) — check listing after publishing and contact the maintainer if not indexed within a day.
- Submit to Travis's awesome-claude-skills list last, only once the tool has ~10+ GitHub stars or other social proof, phrased non-promotionally.
- Cross-post to all relevant channels simultaneously rather than relying on one, keep versions/metadata synchronized, and maintain the listing (respond to issues, re-validate against new Claude Code versions) to avoid staleness or delisting.

## Сущности
- **Люди:** Alireza Rezvani, Jeremy Longshore, Mert Düzgün, travisvn
- **Компании:** Anthropic
- **Продукты/инструменты:** claude-plugins-official, alirezarezvani/claude-skills, jeremylongshore/claude-code-plugins-plus-skills (TonsOfSkills), CrossAI Tools (claudemarketplaces.com), travisvn/awesome-claude-skills, ccpi CLI, GitHub MCP, Slack MCP

## Открытые вопросы
- Exact acceptance criteria and turnaround time for Anthropic's official marketplace submission form are not detailed beyond 'automated screening and manual review'.
- Whether CrossAI Tools' usage-based ranking algorithm can be gamed or how ties/low-usage-but-high-quality tools are handled remains unclear.
- Long-term maintenance risk for community marketplaces (Alireza, Longshore) if maintainers stop updating is flagged but not quantified.
- Reddit reports of '50+ hidden official plugins' in Anthropic's configs suggest an unofficial/undocumented discovery layer not fully explained in the report.

## Источник
- DR-ID `DR26-07-02-HUB-02` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-02-HUB-02-key-findings.md`

## Связано
- [[Claude Code plugin ecosystem]]
- [[skill marketplace distribution strategy]]
- [[developer tool discoverability]]
- [[open vs curated ecosystem governance]]
- [[plugin submission schema (marketplace.json / SKILL.md)]]
