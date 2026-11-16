---
dr_id: DR26-06-27-HUB-03
title: "Extracting comments on a personal Facebook profile (Graph API vs GraphQL vs browser automa"
date: 2026-06-27
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-27-HUB-03): Extracting comments on a personal Facebook profile (Graph API vs GraphQL vs browser automation vs scrapers)

> How can Anton reliably pull comments off his own personal Facebook profile's posts, given that the official Graph API doesn't support personal profiles?

## Ключевые выводы
- Graph API only supports Pages/Business assets in 2026 — personal profile posts and comments are not accessible via official API, and Professional Mode does NOT unlock Graph API access (only creator insights).
- Converting a profile to a Page would enable Graph API but creates a separate object; old profile posts/comments stay behind and don't migrate automatically.
- Internal GraphQL scraping (POST to facebook.com/api/graphql/ with session cookies + fb_dtsg token + doc_id) is fast and low-overhead but brittle — Facebook rotates doc_id/query_hash periodically, silently breaking scrapers.
- Full-browser automation (Playwright/Puppeteer/Airtop) is the most reliable method — handles virtualized DOM, infinite scroll, and 'View more comments' expansion via role/aria-label selectors rather than fragile CSS classes, at the cost of higher complexity and direct account risk.
- Third-party SaaS scrapers (e.g. Apify Facebook Comments Scraper, ~$1-5 per 1000 comments) are safest for account health since they use their own infra/logins, but only capture publicly visible comments — friends-only content is invisible to them.
- Open-source libraries like facebook-scraper (kevinzg) and facebook-graphql-scraper are quick to try but poorly maintained (facebook-scraper last released 2022, ~400 open issues) and break often with Facebook layout changes.
- Anti-bot defenses (CAPTCHAs, checkpoints, IP bans, rate limits) require throttling, human-like pacing, session reuse, and residential proxies for any account-based method.
- Incremental sync should track last-seen comment ID/timestamp per post to avoid duplicates, and author filtering should happen in code after fetching (Facebook doesn't support server-side author filters).

## Рекомендации / решения
- Rank of approaches for personal-profile comment extraction: 1) full-browser automation (Playwright/Airtop) for best reliability, 2) authenticated internal GraphQL scraping for speed/low overhead, 3) third-party scraper (Apify) as a safe fallback for public-only content, 4) lightweight libraries (facebook-scraper) only as a quick/backup option, 5) profile-to-Page conversion + Graph API only if willing to operate as a creator Page.
- Use Claude Code + n8n hybrid: Claude/Claude-in-Chrome writes and injects precise JS/GraphQL fetch scripts and parses JSON; n8n (with Airtop node) orchestrates scheduling, scrolling, error handling, and incremental runs.
- Always operate within an authenticated real browser session to reuse cookies/tokens and avoid login CAPTCHAs; extract fb_dtsg via `document.querySelector('[name=fb_dtsg]').value` before GraphQL calls.
- Prefer parsing embedded hydration JSON (script tags with GraphQL results) over brittle CSS selectors where possible.
- Build retry/fallback logic switching between GraphQL and DOM scraping if one method silently returns empty results (a sign of doc_id rotation or blocking).
- Test Apify's Facebook Comments Scraper in parallel on public posts as a safety net/cross-check against the custom pipeline.
- Pace all account-based requests with randomized delays and monitor for login/CAPTCHA pages to avoid triggering Facebook's anti-bot defenses and account flags.

## Сущности
- **Люди:** —
- **Компании:** Meta, Facebook, Apify, Anthropic
- **Продукты/инструменты:** Graph API, Meta Content Library, Facebook GraphQL (/api/graphql/), Playwright, Puppeteer, Airtop, n8n, Claude Code, Claude-in-Chrome, facebook-scraper (kevinzg), facebook-graphql-scraper (FaustRen), Apify Facebook Comments Scraper, Apify Facebook Review Scraper, Coronium (guide)

## Открытые вопросы
- Exact current GraphQL doc_id for comment-loading queries (must be captured live via Network tab and kept updated as Facebook rotates it).
- Whether Facebook's anti-bot detection would flag Anton's specific account under the proposed pacing/throttling scheme — untested in practice.
- Legal/ToS risk tolerance for scraping his own profile's non-public (friends-only) comments via GraphQL/browser automation vs relying only on public-content-safe third-party scrapers.
- Whether hydration JSON (window.__PRELOADED_STATE__ or similar) reliably contains full comment data across all post types, or only partially.

## Источник
- DR-ID `DR26-06-27-HUB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- chrome-autonomy-self-drive
- sostav-comments
- vault-data-architecture
- second-brain-northstar
- credential-store
