---
dr_id: DR26-06-28-HUB-05
title: "Facebook personal-account automation: ban risk, rate limits, and safe methods for posting/"
date: 2026-06-28
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-28-HUB-05): Facebook personal-account automation: ban risk, rate limits, and safe methods for posting/comments/DMs

> Deep research into how to safely automate posting, replying to comments, and sending Messenger DMs from a personal Facebook account without triggering bans or throttles.

## Ключевые выводы
- No official Graph/Business API works for personal profiles — only Pages; ManyChat/Chatfuel/Composio require a Page, not a personal account.
- Five methods compared: full-browser automation (medium-high ban risk, high complexity), browser extension/userscript (lowest risk, uses real session), in-session GraphQL via fb_dtsg tokens (highest risk, actively flagged as non-human traffic), official APIs (N/A for personal accounts), manual human operation (zero risk).
- Comments/likes: rapid-fire (>3 in a row) can trigger a ~2-day 'slowdown' ban; spacing 4-5 min apart allows dozens/day; stay under ~50 comments/day.
- DMs to non-friends are highest risk: even ~5-10 new message requests/day can trigger a 24-72h block on all new messages; replying within existing threads is much safer than initiating new ones.
- Human-in-the-loop review (draft-first) does not fool Facebook's detection systems but does slow cadence to human-like timing and improves quality/reduces obvious spam triggers.
- For a single account on one home machine, consistent browser profile/IP with persistent cookies is safer than proxies or antidetect tools — frequent IP/cookie changes increase suspicion.
- Commenter data extraction is feasible via GraphQL queries (e.g. ProdashCometCommentsManagerCommentsListPaginationQuery, as used by the open-source 'fbpost' CLI) or simulated browsing; reposts' comments are separate from the original post's comments.
- Messaging a non-friend commenter routes to their Message Requests folder; there is no public API for personal-to-person messaging, only browser/GraphQL automation.
- Personalized, varied message content is essential — identical/templated messages to many recipients are a strong spam signal and get flagged.
- Recommended safe-activity guidelines (community-derived, not official): ~5-10 posts/day, <50 comments/day (no more than 2-3 in a row), single-digit new DM requests/day, immediate pause on any throttle warning.

## Рекомендации / решения
- Use a hybrid workflow: AI (Claude/ChatGPT) drafts posts/comments/DMs, a human reviews and clicks send — especially mandatory for DMs to strangers (task C).
- Prefer browser extension/userscript automation over raw GraphQL calls or headless browser automation, since it runs inside the real user session and is least detectable.
- Keep a single consistent browser profile and home IP for the account; avoid proxies, VPNs, or frequent cookie resets.
- Cap daily volume: ~5-10 wall posts, <50 comments (spaced out, no bursts), and only single-digit new-thread DM requests per day.
- Prioritize replying on your own posts (task B, lower risk) over outbound cold DMs to non-friends (task C, highest risk) whenever possible.
- If a message-request block or comment throttle hits, pause all automation immediately for the 24-72h cooldown rather than retrying.
- Log all automated actions and monitor for Facebook warnings as an ongoing safety mechanism.
- Use OS-level scheduling (cron/Task Scheduler) with human-like random intervals rather than tight automation loops.
- Never log into multiple Facebook accounts from the same browser/device/IP, since this can link accounts and trigger cross-account bans.

## Сущности
- **Люди:** —
- **Компании:** Facebook, ManyChat, Chatfuel, Composio, Multilogin
- **Продукты/инструменты:** Playwright, Selenium, Puppeteer, fbpost CLI, facebook-messenger-puppeteer, Facebook Business Suite, Facebook Graph API, GraphQL (fb_dtsg tokens), Claude, ChatGPT

## Открытые вопросы
- Facebook does not publish official personal-account rate limits — all thresholds are inferred from anecdotal user reports and may vary by account age/history/content.
- Unclear how durable GraphQL-based methods are against Facebook changing internal query IDs over time.
- No confirmed data on whether stealth-plugin/anti-detection measures meaningfully reduce ban risk versus plain headful automation.
- Exact detection mechanism (device/IP fingerprinting vs. behavioral pattern analysis vs. missing 'heartbeat' events) not fully disclosed — only inferred from community reports.

## Источник
- DR-ID `DR26-06-28-HUB-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- chrome-autonomy-self-drive
- fb-post
- fb-reply
- credential-store
- facebook-diary-auto
