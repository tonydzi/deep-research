---
dr_id: DR26-07-02-HUB-10
title: "Building a role-tailored starter PKM kit (Obsidian + Claude Code plugin marketplace) for o"
date: 2026-07-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-02-HUB-10): Building a role-tailored starter PKM kit (Obsidian + Claude Code plugin marketplace) for onboarding friends

> How to package a minimal Obsidian vault plus persona-specific Claude Code plugin bundles into a starter kit that onboards non-technical friends safely and with low friction.

## Ключевые выводы
- Pre-built minimal vault templates (e.g. Obsidian Beginner Vault Template, Obsidian Starter Kit) with predefined folders, a few sample notes and a Getting Started page eliminate painful from-scratch setup.
- A universal base scaffold plus 2-3 role-specific plugin bundles (e.g. Writer, Researcher/Engineer, Creative) cuts cognitive load and speeds time-to-value versus a one-size-fits-all kit.
- Guided onboarding (interactive wizards/first-run walkthroughs) significantly raises perceived usefulness and intention to use versus manuals alone, per Digital Adoption Platform research.
- Claude Code's GitHub-synced plugin marketplace (`.claude-plugin/marketplace.json`, `/plugin marketplace add owner/repo`) is a proven distribution model analogous to VS Code extension packs, and organizations already use it this way (Thoughtworks).
- Marketplace is brittle: a single malformed manifest entry (e.g. bad 'source' field) can make the ENTIRE marketplace fail to load; a `git-subdir` source change once broke clients below v2.1.69 - lesson: validate JSON schema and consider pinning client versions via stable/next branches.
- Privacy/local-first matters: personal AI market growing ~42% CAGR with privacy-first tools (Jan.ai: 5.3M downloads in 2026); starter kit must ship zero personal data, keep API keys/CRM accounts as user-supplied config only.
- Adoption research shows perceived usefulness and low complexity drive uptake; overly complex 'second brain' systems commonly get abandoned under real work pressure (cited failures across Notion/Obsidian/Roam testers, and a writer who deleted her Obsidian vault calling it a 'dusty mausoleum').
- Open source is not a business model: ~67% of developers who release OSS projects end up fielding support requests anyway - plan explicitly for ad-hoc vs formal support and choose license (MIT/Apache vs GPL/AGPL) accordingly.
- Secret hygiene requires automation, not just discipline: pre-commit hooks (detect-secrets, git-secrets) plus GitHub Secret Scanning are needed to prevent credential leaks into a shared repo.

## Рекомендации / решения
- Build the base vault as a separate Git repo: minimal folders, one sample note, a GETTING STARTED.md, core plugins only (Daily Notes, Templates), everything else disabled by default.
- Package each persona (Writer, Engineer/Researcher, Creative, etc.) as its own Claude Code plugin repo referenced from a single marketplace.json, so users install only the bundle matching their role.
- Ship an onboarding wizard as a Claude skill (triggered by `/wizard` or `/onboard`) that asks the user's role, installs the matching bundle, and walks them through trusting the marketplace/folder.
- Add CI (GitHub Actions) to the marketplace repo to validate marketplace.json schema and run secret scans (detect-secrets) before merge, preventing a bad entry from bricking the whole marketplace.
- Maintain two branches - 'stable' for general distribution and 'next'/early-adopter - and let less technical users pin to stable via Claude's extraKnownMarketplaces setting.
- Explicitly choose and publish a license (MIT/Apache for ease of adoption, GPL/AGPL if copyleft protection is wanted) plus a support plan (ad-hoc vs community forum) before wide distribution.
- Keep the base pack to only 3-5 essential skills (capture, query, summarize) to avoid feature bloat; put everything else into optional persona packs.
- Test the scaffold with one novice per target role before wider rollout, and iterate on friction points observed rather than relying on documentation.

## Сущности
- **Люди:** Andrew Mcodes, Ivan Magda
- **Компании:** Thoughtworks, Heavybit, Jan.ai, Vellum.ai
- **Продукты/инструменты:** Obsidian, Claude Code, Claude Code plugin marketplace, Obsidian Starter Kit, Obsidian Beginner Vault Template, detect-secrets, git-secrets, GitHub Secret Scanning, GitHub Actions, Notion, Roam

## Открытые вопросы
- How to formally decide the split between ad-hoc friend support vs a formal community/support channel as adoption grows.
- Whether to eventually dual-license or move to an open-core model if the kit is monetized later.
- How to handle version drift once users heavily customize their personal vaults (merge conflicts, duplicate note IDs, plugin setting clashes) - no concrete resolution mechanism defined yet.
- Exact set of persona bundles to launch with (only Writer/Engineer/Researcher/Creative sketched as examples, not finalized).
- How much investment to make in a stable/next branch strategy given Claude Code marketplace's demonstrated brittleness to client-version mismatches.

## Источник
- DR-ID `DR26-07-02-HUB-10` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\2026-07-02-friends-starter-kit-DR1-generic.md`

## Связано
- [[obsidian-ingest]]
- [[claude-code-plugin-marketplace]]
- [[onboarding-wizard-design]]
- [[second-brain-northstar]]
- [[ak47-simplicity]]
- [[credential-store]]
- [[vault-data-architecture]]
