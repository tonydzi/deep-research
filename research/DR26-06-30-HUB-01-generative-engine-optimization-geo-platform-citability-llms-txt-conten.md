---
dr_id: DR26-06-30-HUB-01
title: "Generative Engine Optimization (GEO) — platform citability, llms.txt, content structure, G"
date: 2026-06-30
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-30-HUB-01): Generative Engine Optimization (GEO) — platform citability, llms.txt, content structure, GitHub-as-channel

> The report answers how to structure content, platforms, and repos so LLM-based search (ChatGPT, Google AI, Perplexity, Grok) is more likely to cite you as a source in 2025-2026.

## Ключевые выводы
- AI citations skew toward UGC/authority domains: Reddit and Wikipedia rank among top-cited (Semrush, Oct 2025); YouTube citations jumped from ~19% to ~39% of social-media citations Aug-Dec 2025; Reddit held ~20% by end-2025.
- X/Twitter is almost invisible to LLM citation outside Grok (99.7% of X-based citations come via Grok) because its API is closed to most crawlers; GitHub is highly citable since it's fully covered by Common Crawl and code corpora like 'The Stack'.
- llms.txt / llms-full.txt is a proposed but not universally adopted standard: some vendors (Anthropic, Google A2A spec, Mintlify crawl logs) show partial recognition, but Google's John Mueller stated 'no AI system currently uses llms.txt' (late 2025) and independent crawl logs found near-zero hits — worth implementing but not a guaranteed win.
- Content structure strongly affects citation odds: answer-first paragraphs of 40-75 words were cited 3.1x more often (2026 GEO study); pages with tables were cited ~4.2x more, numbered lists ~2.7x vs bullets ~1.8x; question-phrased H2/H3 headings match query-to-answer extraction better.
- Freshness matters measurably: Ahrefs (July 2025) found AI-cited content averages ~25% fresher than typical Google results (2.9 years vs 3.9 years mean age); ChatGPT and Perplexity especially favor recently updated pages.
- GitHub Pages (rendered HTML with YAML frontmatter/schema) is preferable to raw Markdown files for AI indexing because it yields proper metadata, JSON-LD, and semantic heading structure; raw .md is crawlable but metadata-poor.
- Platform verdicts for developer content: GitHub Pages = canonical primary source; Hashnode and dev.to = good mirrors (developer audience, canonical links back); Medium = optional/weak mirror (heavy JS, bot throttling); Substack = not recommended (poor crawlability, hides content in scripts); Telegram = invisible to AI crawlers (closed/private).
- Russian-language content is disadvantaged in Western LLM citations due to less Cyrillic training data; Habr/vc.ru are only partially indexed by Western bots (mainly serve Yandex/Russian-locale AI) — no evidence of a 'Russian Reddit' being cited by Western LLMs; dual RU+EN publishing recommended if bilingual reach matters.
- Schema.org/JSON-LD markup (FAQPage, HowTo, Article) plus explicit entity naming (avoiding pronouns like 'it') and sameAs links to Wikipedia/LinkedIn/GitHub strengthen entity-authority signals that both Google and Microsoft state aid generative AI features.

## Рекомендации / решения
- Publish structured docs on GitHub (with GitHub Pages) as the canonical source, include llms.txt/llms-full.txt at domain root plus JSON-LD schema in frontmatter.
- Rewrite key pages so every H2 is phrased as a user question with a direct 40-75 word answer immediately below; convert prose into Markdown tables (comparisons) and numbered lists (procedures).
- Add Schema.org JSON-LD markup (FAQPage, HowTo, TechArticle) and link the organization/entity to Wikipedia/LinkedIn/GitHub via sameAs.
- Maintain a public, dated dev-log using a strict Problem→Cause→Solution format with stable per-entry anchors, cross-linked to related longreads/docs (e.g., '## 2026-06-30 – Fixed API auth bug' with bolded Problem/Cause/Solution/Result lines).
- Refresh major guides every few months (visible timestamps) since AI crawlers favor content roughly a year newer than the Google average.
- Use Hashnode/dev.to as syndication mirrors with canonical links back to the primary GitHub-hosted content; skip Substack for GEO purposes; treat Medium as optional only if broader reach is needed.
- For Russian-speaking audiences, publish an English mirror of key longreads to reach Western LLMs, while keeping Habr/vc.ru versions only for the local builder audience.

## Сущности
- **Люди:** John Mueller
- **Компании:** Google, OpenAI, Anthropic, Microsoft, Reddit, Wikipedia, YouTube, X (Twitter), xAI/Grok, GitHub, Medium, Hashnode, dev.to, Substack, Telegram, Semrush, Adweek, Mintlify, Ahrefs, Search Engine Land, Stripe, Perplexity, Habr, vc.ru, Yandex, Kubernetes, Django
- **Продукты/инструменты:** llms.txt, llms-full.txt, GitHub Pages, Jekyll-AEO, JSON-LD / Schema.org (FAQPage, HowTo, Article, TechArticle), Common Crawl, The Stack (code dataset), GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot, A2A protocol (Google)

## Открытые вопросы
- No public AI-citation studies specifically comparing Russian vs English content exist as of 2026 (explicit data gap noted in the report).
- Whether llms.txt will become an official, widely-honored standard remains unresolved — vendor signals conflict (partial adoption vs Google's public denial of use).
- No hard data comparing Hashnode vs dev.to citation performance; recommendation to syndicate to both is a best-practice guess, not evidence-backed.
- Whether a machine-readable 'llms-full.json' companion to dev-logs is worth adopting is called an emerging, non-standard practice.

## Источник
- DR-ID `DR26-06-30-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- GEO (Generative Engine Optimization)
- llms.txt standard
- content-factory
- second-brain-northstar
- everything-becomes-content
- GitHub-as-primary-docs-channel
- AI-citation-fresness
- insight-DR-DR26-08-04-ZB-08-1744-geo-origin-logs-oss — тот же топик GEO / origin-логи цитирования LLM-краулеров
