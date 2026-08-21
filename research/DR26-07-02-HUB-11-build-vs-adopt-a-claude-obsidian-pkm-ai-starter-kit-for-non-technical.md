---
dr_id: DR26-07-02-HUB-11
title: "Build vs adopt a Claude+Obsidian PKM/AI starter kit for non-technical friends & family"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-11): Build vs adopt a Claude+Obsidian PKM/AI starter kit for non-technical friends & family

> Investigates whether to build a custom PKM/AI vault system from scratch or fork an existing MIT-licensed engine to roll out to non-technical friends, and how to maximize their retention.

## Ключевые выводы
- Software retention is weak without immediate value: ~39% of users remain after 1 month, ~30% after 3 months industry-wide; manual Obsidian setups are typically abandoned within about a week without an AI agent maintaining them.
- PKM skill level predicts tool usage (β=0.398, p<0.05) — non-technical users without existing PKM habits are at high risk of abandoning a new knowledge tool unless workflows match their comfort zone.
- Three mature MIT-licensed 'AI second brain' engines already exist: claude-obsidian (8.4k★, 969 forks), obsidian-second-brain (2.9k★, 341 forks), and obsidian-skills (38.7k★, by Obsidian's CEO) — forking one of these is far faster than building from scratch.
- Karpathy's maintenance-burden principle applies: 'Humans abandon wikis because the maintenance burden grows faster than the value' — without an AI automating upkeep (linking, summarizing), a personal vault stagnates and gets abandoned.
- Concierge (white-glove, done-with-you) onboarding materially improves early retention for non-technical first users; pure self-serve installers risk confusion and drop-off.
- Version/update risk is real and non-trivial: even Obsidian's own point releases (e.g. 1.7.4) have broken users' setups; unmanaged auto-updates can leave a friend's vault unusable overnight.
- The strategic 'moat' for a friends-and-family rollout is curation and personal support, not the underlying code — private/invite-only distribution reduces support load versus going public.
- Recommended layering: fork proven engine (code) + thin per-role 'vault templates' (structure/content only, not separate code) + guided onboarding docs + hands-on concierge setup + governed update cadence (e.g. quarterly, tested in a sandbox before rollout).

## Рекомендации / решения
- Fork an MIT-licensed engine (e.g. claude-obsidian) rather than building a custom PKM/AI system from scratch.
- Build thin 'role packs' (folder structure, CLAUDE.md/USER.md identity files, sample content) per friend's role/persona on top of one shared core engine — avoid maintaining separate codebases per user.
- Write short (3-5 step) 'Getting Started' onboarding docs and pair them with a one-hour concierge setup session per friend, targeting one immediate 'aha' win (e.g. ingest a note, get a cited answer back).
- Keep initial scope minimal — core skills only (note ingestion, daily notes, one semantic query); introduce advanced features later only if the user sticks with it.
- Distribute privately (private repos/invite-only), not publicly, to limit support burden and control the user base.
- Pin versions and never auto-update; test every upstream release in a sandbox vault before rolling it out to friends, and schedule a rollback-ready update cadence (e.g. quarterly) with advance notice.
- Do a 1-2 week follow-up check-in per friend to catch silent abandonment and simplify the setup based on real feedback.
- Be transparent about data ownership (local plain Markdown, no cloud lock-in) to build trust with non-technical users.

## Сущности
- **Люди:** Karpathy, Sebastien Dubois
- **Компании:** Pendo, Reddit
- **Продукты/инструменты:** claude-obsidian (AgriciDaniel), obsidian-second-brain, obsidian-skills, Obsidian, Claude Code CLI, CLAUDE.md, USER.md, AGENTS.md

## Открытые вопросы
- How to handle fork fragmentation if friends independently customize their vaults, making a single update path impossible over time.
- Whether/when to widen distribution beyond private friends-and-family if a user becomes a power user wanting more autonomy.
- How exactly to balance hiding complexity (simple defaults) with providing growth paths for users who want advanced features, without triggering churn.
- No concrete data on actual retention rates achieved with this fork-and-concierge approach specifically (only industry benchmarks and anecdotes cited).

## Источник
- DR-ID `DR26-07-02-HUB-11` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\2026-07-02-friends-starter-kit-DR2-build-vs-adopt.md`

## Связано
- [[second-brain-northstar]]
- [[ak47-simplicity]]
- [[obsidian-ingest]]
- [[concierge-onboarding]]
- [[fork-vs-build]]
- [[vault-conventions]]
