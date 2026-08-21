---
dr_id: DR26-07-14-HUB-13
title: "Personal Site Strategy for an AI Builder Targeting Frontier Labs"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-13): Personal Site Strategy for an AI Builder Targeting Frontier Labs

> What structure, platform, and content a personal site needs to be an effective hiring proof-surface for DevRel, Applied AI, Developer Education, and Forward-Deployed roles at frontier AI labs (OpenAI, Anthropic, Google).

## Ключевые выводы
- The strongest 2025–2026 personal site for these roles is a fast-scannable proof surface, not a bio page: one screen stating role fit, links to best proof-of-work, and measurable evidence others used/adopted what you built.
- Portfolios split by role family: engineering/Applied AI/forward-deployed is judged on systems, code, demos, architecture, and outcomes; DevRel/Developer Education is judged on technical depth plus outward leverage (tutorials, docs, videos, talks, workshops, ecosystem credibility).
- GitHub Pages + custom domain is the strongest default primary site (versioned, code-owned, portable); Notion Sites is faster to ship but weaker on control; Linktree is only a routing layer, not a primary evaluation surface; Read.cv is no longer viable since Perplexity acquired and wound it down in 2025.
- No public evidence shows labs spend a fixed number of seconds on personal sites; recruiter screening is compressed and looks for relevance/impact, with the site as complementary proof, not a substitute for a clear application.
- Anthropic and OpenAI recruiting guidance both push toward AI-as-collaborator not ghostwriter: no fabricated experience, quantified impact, visible human ownership of problem framing, architecture, evaluation, and final judgment.
- Recommended first-screen structure: hero with 3 proof links (flagship demo/project, GitHub, writing/talks) → selected proof-of-work (1-3 flagship projects, not a gallery) → writing → teaching/speaking (higher priority for DevRel, lower for engineering roles) → metrics/evidence → concise background → contact.
- Right metrics for these roles are activation/adoption/workflow-impact figures (users, developers reached, companies, eval lift, workshop attendees, GitHub stars/downloads) rather than vanity reach like follower counts, unless the role is explicitly audience-led.
- For AI-crawler/SEO discoverability: add ProfilePage+Person structured data, a sitemap, clean static HTML, consider an llms.txt (emerging, optional), and set robots.txt deliberately (allow OAI-SearchBot/Claude-SearchBot for AI search visibility, decide separately on GPTBot/training bots).
- Live examples worth modeling: Lilian Weng, Eugene Yan, Hamel Husain, Simon Willison, Sebastian Raschka, Addy Osmani, Lee Robinson, Shawn Wang (swyx), Cassidy Williams, Jason Lengstorf, Angie Jones — common pattern is clear positioning, immediate proof, recent activity, minimal decorative complexity.
- Nonstandard background (e.g., Web3/community work) should be translated into frontier-lab language (community → developer ecosystem insight, token/network growth → adoption mechanics, hackathons → technical education) but must not become the site's main character if targeting broader frontier AI roles.

## Рекомендации / решения
- Build primary site on GitHub Pages with a custom domain; add a compressed GitHub profile README pointing into it; create an org/project page for the flagship public system.
- Structure the homepage as: hero + 3 proof links → 1-3 flagship projects with outcome blurbs → writing → talks/teaching → metrics → short background → contact — proof before autobiography.
- Use suggested positioning line: 'AI builder shipping agentic systems in public. Building [flagship system], publishing the operating manual on GitHub, and turning frontier-AI patterns into demos, docs, and lessons developers can actually use,' with buttons 'See the system / Read the book / GitHub'.
- Add ProfilePage/Person structured data, an XML sitemap, RSS, and consider llms.txt as an emerging (non-critical) addition.
- Set robots.txt deliberately: allow OAI-SearchBot and Claude-SearchBot for AI search discoverability; decide separately whether to allow GPTBot for training.
- Publish an explicit AI-collaboration policy statement on the site (e.g., 'Built with Claude/Codex/GPT as implementation collaborators; I owned problem framing, architecture, evaluation, product decisions, and final QA').
- Do not use Linktree or Read.cv as the primary hiring asset; keep Linktree only as a secondary routing layer for social bios.
- Quantify achievements with role-relevant metrics (adoption, activation, eval lift, workflow impact) using patterns like 'Built X for Y, resulting in Z' rather than vanity follower counts or unquantified self-labels.
- Translate Web3/community background into frontier-lab operating language, but keep it as supporting evidence, not the site's central narrative, when targeting broader AI roles.
- Publish a machine-readable CV (e.g., JSON Resume as resume.json/cv.json) as an optional, emerging-best-practice artifact.

## Сущности
- **Люди:** Lilian Weng, Eugene Yan, Hamel Husain, Simon Willison, Sebastian Raschka, Addy Osmani, Lee Robinson, Shawn Wang, Cassidy Williams, Jason Lengstorf, Angie Jones
- **Компании:** OpenAI, Anthropic, Google, Perplexity, TheLadders
- **Продукты/инструменты:** GitHub Pages, GitHub profile README, Notion Sites, Linktree, Read.cv, ProfilePage/Person structured data, llms.txt, robots.txt, OAI-SearchBot, GPTBot, Claude-SearchBot, JSON Resume, sitemap, RSS

## Открытые вопросы
- No confirmed lab-specific data on exact click order or time-on-page for personal sites during screening (labeled Emerging, not Established).
- Whether llms.txt provides real SEO/AI-crawler benefit is still an unproven, emerging convention, not a standard.
- Machine-readable CV (JSON Resume) is not yet an established hiring standard, just a viable optional artifact.
- The exact disclosure pattern for AI-assisted work on personal sites is not standardized across labs (Emerging).
- Whether a custom domain materially changes hiring outcomes vs. github.io subdomain is inferred, not directly evidenced (Emerging).

## Источник
- DR-ID `DR26-07-14-HUB-13` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DL-2026-07-14--Personal-Site-Strategy-for-an-AI-Builder-Targeting.md`

## Связано
- [[DevRel career positioning]]
- [[personal branding for AI roles]]
- [[GitHub Pages portfolio]]
- [[build-in-public strategy]]
- [[frontier lab hiring process]]
- [[second-brain-northstar]]
- [[everything-becomes-content]]
- [[insight-DR-DR26-07-14-HUB-14-personal-site-strategy-for-an-ai-builder-targeting]] — почти идентичный DR того же дня (HUB-13 vs HUB-14), один и тот же вопрос про personal site strategy
- [[insight-DR26-07-17-ZB-01-recruiter-onepager-synthesis]] — тот же кластер DR про личный сайт под наём в frontier labs
