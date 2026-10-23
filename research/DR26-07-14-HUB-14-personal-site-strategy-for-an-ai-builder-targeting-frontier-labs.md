---
dr_id: DR26-07-14-HUB-14
title: "Personal Site Strategy for an AI Builder Targeting Frontier Labs"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-14): Personal Site Strategy for an AI Builder Targeting Frontier Labs

> What should a personal website look like for someone targeting DevRel, Applied AI, Developer Education, or Forward-Deployed roles at frontier AI labs (OpenAI, Anthropic, Google)?

## Ключевые выводы
- The strongest personal site for these roles is a fast-scannable 'proof surface' — one screen stating role fit, linking to best proof-of-work, and showing measurable evidence of adoption — not a bio page.
- Portfolio judgment splits by role family: engineering/Applied AI/forward-deployed is judged on systems, code, demos, architecture, production outcomes; DevRel/Developer Education is judged on tutorials, docs, videos, talks, workshops, ecosystem credibility.
- GitHub Pages plus a custom domain is the strongest default platform: versioned, code-owned, portable, recruiter- and machine-readable. Notion is faster to ship but weaker on control; Linktree is only a routing layer, not a primary evaluation surface; Read.cv is no longer viable (acquired by Perplexity, wound down in 2025).
- No evidence of a lab-specific fixed number of seconds recruiters spend on a personal site, but screening is heavily compressed — the site must be understandable in seconds then rewarding on deeper clicks; it reinforces the application, it doesn't replace it (first gate is the recruiter/hiring manager, not the website).
- Anthropic and OpenAI recruiting guidance converge: use AI as a collaborator not a ghostwriter, never fabricate experience, quantify impact, keep the candidate's real judgment and ownership visible.
- Recommended AI-era discoverability stack: ProfilePage/Person structured data, an XML sitemap, RSS, an optional (emerging, non-standard) llms.txt, and deliberate robots.txt rules — e.g. allow OAI-SearchBot for ChatGPT search visibility while separately deciding on GPTBot for training, similarly for Claude-SearchBot.
- Eleven live sites are held up as models of the pattern (clear positioning, immediate proof, recent activity, minimal decoration): Lilian Weng, Eugene Yan, Hamel Husain, Simon Willison, Sebastian Raschka, Addy Osmani, Lee Robinson, Shawn Wang (swyx), Cassidy Williams, Jason Lengstorf, Angie Jones.
- Recommended page structure: hero with 3 proof links (flagship demo/project, GitHub, writing/talks) → 1-3 flagship projects with outcome blurbs → writing/thinking → teaching/speaking (weighted higher for DevRel roles) → quantified metrics/evidence → concise background → contact/resume links.
- Right metrics are role-relevant outcomes (users, adoption, eval lift, workflow impact, students/companies reached) rather than vanity reach numbers, except when the role itself is audience-led.
- A nonstandard background (e.g. Web3/community work) should be translated into frontier-lab language (community → developer ecosystem insight, governance → stakeholder alignment) rather than left as a subculture marker, and should not become the page's main character if targeting broader frontier AI roles.

## Рекомендации / решения
- Build the primary site on GitHub Pages with a custom domain; maintain a compressed GitHub profile README pointing into it; create an org/project page for the flagship public system.
- Lead the homepage with proof-of-work (1-3 flagship projects, outcome-oriented) above the fold, not autobiography — bio goes near the bottom, subordinate to proof.
- Add ProfilePage/Person structured data, sitemap, RSS, and deliberate robots.txt rules (allow OAI-SearchBot, decide separately on GPTBot/Claude-SearchBot); treat llms.txt and a machine-readable JSON résumé (JSON Resume) as optional/emerging extras.
- Quantify claims with role-relevant evidence (adoption, activation, eval lift, workflow impact) instead of self-labels or vanity metrics.
- State an explicit AI-collaboration policy on the page: which tools were used, and which parts (framing, architecture, evals, product decisions, QA) were personally owned.
- Avoid Linktree, Notion, or Read.cv as the primary hiring surface — use them only as secondary routing/speed tools.
- For the specific 'agent-builder + GitHub book + community background' profile, lead with a positioning line like 'AI builder shipping agentic systems in public' plus three buttons: See the system / Read the book / GitHub.
- Translate Web3/community experience into frontier-lab operating language rather than presenting it as a subculture identity.

## Сущности
- **Люди:** Lilian Weng, Eugene Yan, Hamel Husain, Simon Willison, Sebastian Raschka, Addy Osmani, Lee Robinson, Shawn Wang (swyx), Cassidy Williams, Jason Lengstorf, Angie Jones
- **Компании:** OpenAI, Anthropic, Google, Perplexity, GitHub, Notion, Linktree, Read.cv, TheLadders
- **Продукты/инструменты:** GitHub Pages, GitHub profile README, Notion Sites, Linktree, Read.cv, ProfilePage/Person structured data, sitemap.xml, llms.txt, robots.txt, OAI-SearchBot, GPTBot, Claude-SearchBot, JSON Resume

## Открытые вопросы
- Exact click order reviewers follow on a personal site (labeled Emerging, not Established, in the report)
- Whether llms.txt provides measurable benefit — currently an optional, non-universal convention
- The precise disclosure pattern for AI co-authorship on personal sites (Emerging confidence)
- Whether a custom domain materially outperforms a default github.io subdomain for hiring outcomes (inference, not directly evidenced)

## Источник
- DR-ID `DR26-07-14-HUB-14` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- personal-branding
- devrel-hiring
- ai-builder-portfolio
- github-pages
- frontier-lab-recruiting
- second-brain-northstar
- everything-becomes-content
