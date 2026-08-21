---
dr_id: DR26-07-17-HUB-05
title: "World-Class Public Roadmaps for Build in Public Projects"
date: 2026-07-17
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-17-HUB-05): World-Class Public Roadmaps for Build in Public Projects

> What public roadmap format builds the most trust for a two-person human+AI build-in-public team, based on 10 case studies (GitHub, Zed, Tauri, Plausible, Trigger.dev, Canny, Atlassian, GitLab, Supabase, Linear)?

## Ключевые выводы
- The trust-optimal format is a hybrid: North Star → Now/Next/Later (epic-level items only) → 'Changed our mind' log → dated Ship log with artifact links (commits/releases/demos/dev-logs) — not a feature calendar or quarter-based board.
- Trust breaks when a roadmap silently stops matching reality: GitHub faced backlash for removed items until it added explanatory comments; Epic's Unreal/Fab board was called 'not a roadmap' for being stale and post-hoc; GitLab deprecated its direction pages because they were manually maintained, low-traffic and often outdated; Canny users complained that hiding 'Complete' made it look like nothing ever shipped.
- Public roadmap items across all strong examples are epic/theme/milestone level, never task-level — GitHub uses issue-level features, Zed highlights initiatives, Atlassian uses feature cards, Tauri used milestone gates (Beta→RC→Stable); academic literature confirms roadmaps are strategic communication tools, not task breakdowns.
- 'Done' needs a separate, visible proof surface from the forward-looking roadmap: GitHub closes issues with changelog links, Canny explicitly splits roadmap (future) from changelog (past), Plausible groups completed items by month, Supabase runs weekly dashboard updates + Launch Weeks as its primary shipping proof.
- Quarter/date-heavy boards work at Atlassian/GitHub's enterprise scale (with explicit 'not a commitment' disclaimers) but are the wrong primary surface for a small team with volatile priorities — Tauri publicly slipped its 'early 2024' stable target to Oct 2, 2024 and later removed target dates from some surfaces because maintaining them was 'too much hassle'.
- Three consistent trust-killers: overpromised dates, silent removals (worse than visible delays — GitHub's threads show users calling silent removal 'disrespectful'), and zombie 'in progress' items that never resolve.
- A changelog-first model (Supabase) proves shipping excellently but weakly answers 'where is this going' — it's a strong supporting layer, not a sufficient standalone roadmap.
- For an engineer audience, a live board + weekly narrative summary works best (Linear recommends/uses weekly project updates; Supabase's weekly dashboard updates are tolerated and valued); roadmap items should be tied to natural work-state transitions rather than treated as separate ceremonial admin.
- Recommended per-item fields: Title (as problem/result), Status (Now/Next/Later/Shipped/Changed), Confidence (High/Med/Low), Why this matters, Owner (Human/AI/Shared), Proof target, Links, Last moved — keep exact dates, task breakdowns and fragile internals off the public page.
- There is very little direct empirical research comparing public roadmap formats or measuring trust outcomes specifically; the strongest academic support is general (roadmapping under volatility needs explicit maintenance mechanisms), not public-SaaS-specific.

## Рекомендации / решения
- Adopt the Hybrid Trust Roadmap template: North Star → How to read this → Now → Next → Later → Changed our mind → Not now → Ship log (copy-paste markdown template provided in report).
- Every roadmap item must have a linkable object (issue, PR, design note, or dev-log) — borrow this from GitHub/GitLab/Zed's pattern of anchoring items to real discussion objects.
- Maintain a hard dated Ship log where every shipped item links to a release/PR/demo/post/chapter — borrow from Supabase/Trigger.dev's changelog discipline.
- Never publish exact dates beyond work already in flight; use Confidence labels (High/Med/Low) instead of quarters.
- Never silently remove an item — move it to 'Changed our mind' with one blunt sentence explaining why (GitHub's 2024 refresh only became defensible after adding explanatory comments).
- Update the roadmap when work actually moves, not on a separate admin cadence: end-of-session 3-minute micro-ritual, weekly 20-minute public review (move 0-3 items + publish 'what moved' note), monthly 15-minute hygiene (archive old ship-log rows, cull Later).
- Keep the public reader-facing roadmap to 10-20 meaningful items total, never task-level clutter (~70 microtasks).
- Archive completed items aggressively (Canny's 'Archive Roadmaps' pattern) so old rows don't hide current relevance.
- For the human+AI duo specifically, chain roadmap item → session log → dev-log/social post → ship-log linkback → book chapter/architecture note, making the roadmap itself part of the build-in-public narrative flywheel.

## Сущности
- **Люди:** Teresa Torres
- **Компании:** GitHub, Zed, Tauri, Plausible Analytics, Trigger.dev, Canny, Atlassian, GitLab, Supabase, Linear, Epic Games, Productlane, Featurebase
- **Продукты/инструменты:** GitHub Public Roadmap, Zed Roadmap, Tauri 2.0 roadmap, Plausible feedback/roadmap portal, Trigger.dev roadmap & changelog, Canny public roadmap, Atlassian Cloud Roadmap, Atlassian Data Center Roadmap, GitLab direction pages / Healthy Backlog Initiative, Supabase changelog & Launch Week, Linear project updates / agent-assisted updates

## Открытые вопросы
- Few first-class public examples explicitly show per-item Confidence labels (High/Med/Low) — treated as a recommended extension, not a proven norm.
- Few strong official examples of an explicit public 'anti-roadmap' / 'won't do' section (GitLab's Healthy Backlog Initiative is the closest analogue).
- Academic literature on roadmapping under volatility is general, not specific to public SaaS/OSS roadmap trust outcomes — direct empirical comparison of formats is largely missing.
- Some supporting claims are single-source or low-authority (Productlane and Featurebase vendor blog posts on public vs internal roadmap fields).
- Whether the proposed weekly cadence / hybrid template actually improves trust for THIS specific duo is a synthesized recommendation, not independently verified.

## Источник
- DR-ID `DR26-07-17-HUB-05` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-16-HUB-06-public-roadmap-chatgpt.md`

## Связано
- [[build-in-public]]
- [[product-roadmapping]]
- [[changelog-first-shipping]]
- [[now-next-later-framework]]
- [[trust-mechanics-public-communication]]
- [[content-cascade-auto-execute-never-ask]]
- [[everything-becomes-content]]
- [[insight-DR-DR26-07-17-HUB-01-world-class-public-roadmaps-for-build-in-public-pr]] — near-duplicate DR report on the exact same topic, same case studies
