# DR26-11-12-HUB-01-0234 — the exact prompt sent to every model

# DR26-11-12-HUB-01-0234 — Коннекторы под дыры флота: Luma, Facebook/Messenger, X API цена, YC WaaS, WhatsApp — официальный API vs браузер (замер 10000 сессий H76340)

Context: a small team runs a fleet of AI coding agents (Claude Code, Codex) that act for one person: job search, event registration, social posting, messaging. We measured 10,000 agent sessions (13 Oct to 12 Nov 2026). Where no official connector exists, agents drive a real browser by clicks, which fails ~10% of calls and needs a live logged-in browser.

Measured demand without a connector (browser navigations / distinct sessions):
1. Luma (luma.com, lu.ma): 204 / 66 — discovering events and registering as an attendee.
2. Facebook + Messenger: 138 / 59 — reading comments on own personal-profile posts, replying, DMs (personal profile, not a Page).
3. X (x.com): 99 / 40 browser, plus an official X API v2 posting rail that delivered 49 posts and then stopped for 34 h on HTTP 402 "credits depleted"; 102 browser posting attempts were parked because the browser extension was not connected.
4. YC Work at a Startup (workatastartup.com, ycombinator.com): 37 / 12 — applying and messaging founders.
5. WhatsApp via an unofficial bridge MCP: ~1,100 calls with ~18% errors.

Questions, answer each with sources dated 2026 and say "unknown" where you cannot verify:
A. For each of the 5, is there an OFFICIAL API or official MCP server as of November 2026 that covers the agent's actual job (attendee registration on Luma; personal-profile comments and Messenger DMs on Facebook; posting, replying, reading mentions and search on X; candidate applications on YC WaaS; personal WhatsApp)? Exact scopes, approval process, account type required.
B. Price: exact current X API pricing (pay-per-use credits vs tiers), cost per post, per read, per search; a realistic monthly cost for 5 posts/day, 50 replies/day, 500 reads/day. Same for any paid option in A.
C. Terms-of-service risk of the unofficial route for each (browser automation, unofficial bridges), with concrete enforcement cases from 2025-2026.
D. Best maintained third-party MCP servers or SDKs (GitHub stars, last commit date) for each of the 5.
E. Recommendation per item: official API / third-party / keep browser / drop. One line each with the decisive fact.

Output a table: item | official option | price | ToS risk | best non-official | recommendation. Cite every row.
