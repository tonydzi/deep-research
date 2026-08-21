---
dr_id: DR26-07-17-HUB-01
title: "World-class public roadmaps for build-in-public projects"
date: 2026-07-17
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-17-HUB-01): World-class public roadmaps for build-in-public projects

> What roadmap structure, granularity, and update discipline actually build trust with a technical public audience, based on 10 named case studies (GitHub, Zed, Tauri, Plausible, Trigger.dev, Canny, Atlassian, GitLab, Supabase, Linear).

## Ключевые выводы
- The trust-optimal format is a hybrid: North Star → Now/Next/Later stack (epic-level only) → 'Changed our mind' log → dated Ship log with artifact links (commits, releases, demos, dev-logs) — not a feature calendar or quarter-based board.
- Trust breaks most reliably when a roadmap silently stops matching reality: GitHub's Nov 2024 refresh caused backlash until it added explanatory comments to removed items; GitLab deprecated its direction pages entirely because they were manually maintained, low-traffic and outdated; Epic's Unreal/Fab board was called 'not a roadmap' because it updated only after releases; Canny users complained that hiding 'Complete' made it look like nothing shipped.
- Public roadmap items across all high-trust examples stay at epic/theme/feature/milestone level, never task-level — GitHub uses issue-level features, Zed highlights contributor-worthy initiatives, Atlassian uses feature cards, Tauri used milestone gates (Beta→RC→Stable).
- 'Done' needs a separate proof surface distinct from the forward-looking roadmap: GitHub closes issues with changelog links, Canny explicitly moves 'Complete' items to a separate changelog, Supabase runs weekly dashboard updates plus Launch Week narrative bursts, Plausible groups completed items by month.
- Quarter/date-based roadmaps work only at large-company scale with an explicit non-commitment disclaimer (Atlassian: 'informational only, not a commitment to deliver'); for small volatile teams they backfire — Tauri's 'early 2024' target slipped to Oct 2, 2024 stable release, and Tauri later removed target dates from some public surfaces because maintenance was 'too much hassle'.
- Overpromised dates, silent removals, and 'zombie in-progress' items are the three most damaging trust failures observed across the sample; explanatory change-logging (stating why something moved/was removed) is the most consistent trust repair mechanism.
- High-trust teams update the roadmap as a byproduct of natural work transitions (status change, issue close) rather than as a separate ceremonial task — e.g. GitLab reviews backlog alignment weekly, Linear posts scheduled project updates, Atlassian stamps 'Last Updated' with quarterly subscription emails.
- Academic literature on roadmapping is thin and mostly about volatile-environment strategic communication, not public SaaS/OSS roadmap formats specifically — conclusions here are synthesized from vendor case studies, not controlled studies, and several claims are flagged [emerging]/[speculative]/[single-source].

## Рекомендации / решения
- Adopt the Hybrid Trust Roadmap template: North Star → How to read this → Now → Next → Later → Changed our mind → Not now → Ship log (full copy-paste markdown template included in the report).
- Keep the public roadmap to 10-20 meaningful items total (epics/themes), never task-level clutter (~70 microtasks reads as noise or theater).
- Give every item public fields: Title (as problem/result, not task), Status, Confidence (High/Medium/Low), Why this matters, Owner (Human/AI/Shared — notable value for a visible human+AI duo), Proof target, Links, Last moved.
- Never publish exact dates beyond work already in flight; use confidence labels instead of quarter labels for a small volatile team.
- Never silently remove an item — move it to a 'Changed our mind' section with one blunt sentence explaining why.
- Attach a linkable artifact object (issue, PR, design note, dev-log) to every roadmap item so the roadmap is inspectable, not decorative.
- Run a 3-tier update ritual: 3-min end-of-session state check, 20-min weekly public review (move ≤3 items + publish a 'what moved' note + promote one shipped item into a deeper artifact), 15-min monthly hygiene (archive old ship-log rows, cull Later).
- For the human+AI duo, chain roadmap item → session log → dev-log/social post → ship-log linkback → book chapter/architecture note, turning the roadmap into the coordination script between human judgment and AI execution.
- Keep internal-only fields (exact narrow dates, account names, fragile dependencies, financial sensitivities) off the public page.

## Сущности
- **Люди:** Teresa Torres
- **Компании:** GitHub, Zed, Tauri, Plausible Analytics, Trigger.dev, Canny, Atlassian, GitLab, Supabase, Linear, Epic Games (Unreal/Fab), Productlane, Featurebase
- **Продукты/инструменты:** GitHub Public Roadmap, Zed Roadmap, Tauri 2.0 roadmap, Plausible feedback/roadmap portal, Trigger.dev roadmap + changelog, Canny public roadmap/changelog, Atlassian Cloud Roadmap, Atlassian Data Center Roadmap, GitLab direction pages / Healthy Backlog Initiative, Supabase changelog / Launch Week, Linear project updates / agent-assisted updates

## Открытые вопросы
- No direct empirical research compares public roadmap formats or measures customer trust outcomes quantitatively — conclusions are inferred from case study patterns, not controlled studies.
- Whether explicit per-item confidence labels (High/Medium/Low) are a proven norm is unverified — very few real public roadmaps do this explicitly; treated as a recommended extension only.
- Whether an explicit public 'anti-roadmap'/'won't do' section helps is unverified — only GitLab's Healthy Backlog Initiative approximates it, and that's single-source.
- The exact operational chain (roadmap item → session log → dev-log → ship-log → book chapter) proposed for a human+AI duo is a synthesized extrapolation, not directly documented by any single source.
- Productlane and Featurebase sourced claims about public-vs-internal roadmap field separation are vendor blog content, flagged low-authority/single-source.

## Источник
- DR-ID `DR26-07-17-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-17-HUB-01-world-class-public-roadmaps-for-build-in-publ.md`

## Связано
- [[build-in-public]]
- [[content-cascade-auto-execute-never-ask]]
- [[changelog-first-shipping]]
- [[now-next-later-framework]]
- [[ship-log]]
- [[developer-trust-signals]]
- [[product-roadmapping-volatility]]
- [[insight-DR-DR26-07-17-HUB-05-world-class-public-roadmaps-for-build-in-public-pr]] — near-duplicate DR report on the exact same topic, same case studies
