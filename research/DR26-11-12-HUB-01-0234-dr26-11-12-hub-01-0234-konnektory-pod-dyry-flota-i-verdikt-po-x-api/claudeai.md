# DR26-11-12-HUB-01-0234 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Connectors for the fleet's gaps: Luma, Facebook/Messenger, X, YC WaaS, WhatsApp. Official API vs browser (H76340, November 2026)

Only X has an official API and MCP server that covers most of what the agent does today. Luma's official MCP covers event discovery but not registering for events you don't host. Facebook personal profiles, Messenger DMs from a personal account, YC Work at a Startup and personal WhatsApp have no official route at all. The best moves are:
- **X:** fix billing (auto-recharge plus a spending cap) and stay on the API.
- **Luma:** use the official MCP for discovery and keep the browser only for the final "Register" click.
- **WhatsApp:** replace the stale bridge with a maintained fork.
- **Facebook and YC:** keep the browser with a human approving each action, or drop them.

## TL;DR
- **Official coverage, November 2026:**
  - X: yes. Pay-per-use API plus the official XMCP server. Replying to strangers is blocked on self-serve.
  - Luma: partial. The official MCP at mcp.luma.com finds events and lists the ones you attend, but has no attendee-registration tool. The REST API is host-only and needs Luma Plus.
  - Facebook personal profile and Messenger DMs: no. The Graph API publishes to Pages only, the Messenger Send API needs a Page token, and Meta's official MCP covers ads only.
  - YC Work at a Startup: no.
  - Personal WhatsApp: no. The Cloud API is for business numbers, and general-purpose AI assistants have been banned on it since 15 January 2026.
- **X cost:** about $90/month for 5 plain posts, 50 summoned replies and 500 third-party post reads a day. It drops to about $30/month if most reads are your own mentions or timeline (billed as "owned reads" at $0.001) and rises to about $200/month if every post carries a URL ($0.20 each) and the reads are user lookups. The 34-hour outage was an empty prepaid balance (HTTP 402) with no auto-recharge firing. It was not a rate limit.
- **Risk ranking for the unofficial routes, highest first:**
  - WhatsApp bridges: documented ban waves, including whatsmeow issue #810 (May 2025) and Baileys issue #2850 (October 2026).
  - X browser automation: on 14 February 2026, X head of product Nikita Bier posted: "If a human is not tapping on the screen, the account and all associated accounts will likely be suspended". On 24 July 2026 he posted: "We found 42,000 accounts automating replies using chatbots and have removed them from the platform."
  - Facebook: its terms ban automated collection without permission. I found no 2025–2026 personal-account enforcement case.
  - YC: its Terms of Use prohibit scraping and robots. No enforcement cases are known.
  - Luma: terms unknown, and no enforcement cases known.

## Decision table

| Item | Official option (Nov 2026) | Price | ToS risk of unofficial route | Best non-official | Recommendation |
|---|---|---|---|---|---|
| **Luma** (204 navigations / 66 sessions) | Official MCP `https://mcp.luma.com`, OAuth with any Luma account, Luma Plus not needed. It can discover public events and list "what you're attending or hosting", but has no attendee-registration tool. The REST API (`public-api.luma.com`, `x-luma-api-key`) is scoped to calendars you manage. | MCP: free. API: Luma Plus, "$59/month billed annually (or $69 month to month)" according to Luma's own help center, which says API access is "included with Luma Plus". | Low to medium. Luma's terms on automation were not verified; enforcement cases unknown. | Host-side only: Blackie360/luma-events-mcp (23 stars, updated 23 Aug 2026). No open-source attendee-registration server found. Reduck (commercial) claims to register attendees through your own Chrome. | **Official MCP for discovery, browser for the final register click.** Decisive fact: the MCP lists discovery but no registration. |
| **Facebook + Messenger** (138 / 59) | None for personal profiles. `publish_actions` was removed in 2018, so the Graph API publishes to Pages only. The Messenger Send API needs a Page ID and Page access token. The official Meta MCP (`mcp.facebook.com/ads`, open beta since 29 April 2026) covers ads only. | n/a (Graph and Messenger are free, but Page-only) | Medium to high. Meta's Automated Data Collection Terms ban automated collection without written permission, logged in or out (from 1 January 2025). No 2025–2026 case against a personal-account agent found (unknown). | m008v/fbchat-v2 (cookie-based personal account, release v2.3.4 on 21 October 2026, E2EE Messenger support); fukata/messenger-mcp wraps it and warns accounts can be checkpointed or disabled. Star counts unknown. | **Keep the browser, with a human approving each action. Do not run cookie libraries on the main account.** Decisive fact: no official API reaches personal-profile comments or personal DMs. |
| **X** (99 / 40 browser, plus the API rail) | X API v2 pay-per-use plus the official XMCP server (`xdevplatform/xmcp`, hosted at `api.x.com/mcp` since 30 June 2026). It covers posting, search, mentions and users. Replies are allowed only when the author @-mentioned or quoted you (since 23 February 2026). | $0.015 per post, $0.20 per post with a URL, $0.010 per summoned reply, $0.005 per post read, $0.010 per user read, $0.001 per owned read. About $30–$200/month for this workload (calculation below). | High for the browser. X warned on 14 February 2026 that automation not driven by a human would be suspended. Its terms set $15,000 in liquidated damages per 1M posts scraped. It sent cease-and-desist letters to Nitter and XCancel on 24 August 2026. | xdevplatform/xmcp (863 stars, last updated 9 April 2026, PRs active in September 2026). Cookie clients such as d60/twikit and agent-twitter-client violate the ToS and their forks are fragmented. | **Official API plus XMCP. Enable auto-recharge and a spending cap. Drop unsolicited replies.** Decisive fact: 402 means an empty balance, and the API rejects unsummoned replies. |
| **YC Work at a Startup** (37 / 12) | None. There is no public candidate API or MCP (unknown for anything private). | n/a | Medium. YC's Terms of Use prohibit scraping, data mining and robots; a login does not override this. Enforcement cases unknown. | Only Apify job-listing scrapers (read-only, no applying or messaging). | **Keep the browser with human approval for each application and founder message, or drop it.** Decisive fact: no API exists, the ToS bans robots, and the volume is tiny (12 sessions/month). |
| **WhatsApp (personal)** (about 1,100 calls, about 18% errors) | None for personal accounts. The Cloud API runs on a business number and cannot attach to your consumer account. Since 15 January 2026, AI providers cannot offer general-purpose assistants on it (except in the EEA, Italy and Brazil, where the law requires it). | Cloud API: per-message (n/a here) | High. WhatsApp's terms prohibit unofficial clients. Ban waves are documented in whatsmeow issue #810 (May 2025) and Baileys issue #2850 (7 October 2026). No lawsuits against individual users found. | lharries/whatsapp-mcp (about 6.2k–6.7k stars, last commit 13 July 2025, **abandoned**; fails with "Client outdated (405)"). verygoodplugins/whatsapp-mcp is the maintained fork. Underlying libraries: tulir/whatsmeow (about 7.5k stars, commits 7 November 2026) and WhiskeySockets/Baileys (11.6k stars, updated 1 November 2026). | **Switch to the maintained fork now. Keep usage mostly read-only and human-paced.** Decisive fact: the 18% error rate likely comes from the bridge being frozen since July 2025. |

## Details by platform

### 1. Luma
- **Official MCP.** Luma's help center says the server at `https://mcp.luma.com` lets assistants "Discover events — search upcoming public events on Luma by keyword, location, and date range" and list "what you're attending or hosting". It also says "Anyone with a Luma account can connect — you don't need Luma Plus."
- **No registration tool.** Every write capability the page lists is host-side: creating events, approving guests, tickets, coupons, blasts, calendars. The only personal write is editing your own profile. Attendee self-registration is not listed, so treat it as unsupported.
- **Setup.** Claude Code: `claude mcp add --transport http luma https://mcp.luma.com`. Codex works through OAuth Client ID Metadata Documents. Clients that rely on dynamic client registration fail.
- **REST API.** Help center: "you'll need an active Luma Plus subscription on the calendar you want to access." Keys are per calendar, with limits of 200 requests/minute per calendar key and 500 requests/minute per organization key. Its guest endpoints add guests to events you manage, so it cannot register you for someone else's event.
- **Price.** Luma's own help center ("Luma vs Splash", pricing checked September 2026) says: "Luma Plus is $59/month billed annually (or $69 month to month)". It adds that the API, webhooks and Zapier are "all included with Luma Plus".
- **What it means for the fleet.** Most of the 204 navigations are probably discovery and reading event pages. Those move to the free official MCP. The browser is left with a single click per registration.
- **Watch the name collision.** `lumalabs/luma-api-mcp` and similar servers belong to Luma AI, the video-generation company, not lu.ma events.

### 2. Facebook + Messenger (personal profile)
- **Posting.** Meta deprecated `publish_actions` in 2018 and the last extensions ended on 1 February 2020. The current Graph API (v25/v26) publishes to Pages only.
- **Comments.** Reading and replying use Page permissions (`pages_read_user_content`, `pages_manage_engagement`). The question's assumption that `user_posts` plus publish permissions would work for personal posts is refuted for publishing. Reading your own posts through `user_posts` was not verified for 2026 and would still not allow replies.
- **Messenger.** Meta's Send API documentation (updated 11 August 2026) requires "The Page ID for the Facebook Page… [and] A Page access token". Conversations must be started by the person. The Messenger Platform overview describes it as letting "a business' Facebook Page or Instagram Professional account respond". There is no personal-to-personal DM API.
- **Official Meta MCP.** Only the Ads MCP exists (`mcp.facebook.com/ads`, open beta since 29 April 2026, 29 tools). It does not cover profiles, comments or Messenger.
- **Enforcement.**
  - Meta's terms prohibit automated collection without prior written permission, and the 1 January 2025 update covers both logged-in and logged-out access.
  - Meta has sued scraping vendors before (Bright Data, Octopus), but those cases are from 2022–2024.
  - For 2025–2026, I found no case against an individual's agent driving their own personal account (unknown).
  - In October 2026 Meta delayed its own new business terms, which would have let Meta scrape business websites. That shows a shifting policy stance, not enforcement against users.
- **Third-party route.** fbchat-v2 is active: v2.3.4 on 21 October 2026, and it has decrypted E2EE Messenger messages through a mautrix-meta Go bridge since 12 May 2026. It is cookie-based, though, and fukata/messenger-mcp's own README warns of checkpoints and account disabling.
- **Recommendation.** At 59 sessions/month, losing the person's real Facebook account costs more than 10% browser failures do. Keep the browser with a human confirming each reply or DM.

### 3. X
- **Pricing model.** X's pricing page says: "The X API uses pay-per-usage pricing. No subscriptions." Pay-per-use launched on 6 February 2026, and Basic and Pro closed to new signups. Basic subscribers were auto-migrated after 1 June 2026, with defaults of $200 auto-recharge below a $10 balance and a $400 spend cap per cycle. Pro subscribers migrate after 1 September 2026. Enterprise has no public price ("contact sales"); Data365 reports that the Monthly Budget field on X's Enterprise application form offers options "from $42,000 to $210,000", which is still a secondary source. Pay-per-use is capped at 3 million post reads per billing cycle.
- **Rate card** (docs.x.com):

| Operation | Price | Unit |
|---|---|---|
| Posts: Read | $0.005 | per post returned |
| User: Read | $0.010 | per user returned |
| Owned Reads | $0.001 | per resource |
| Post create | $0.015 | per request |
| Post create with URL | $0.20 | per request |
| Summoned reply | $0.010 | per request |
| DM create | $0.015 | per request |
| Counts: recent | $0.005 | per request |

- **What each endpoint costs.** Owned Reads ($0.001, since 20 April 2026) apply to `/2/users/{id}/tweets`, `/mentions`, bookmarks, followers, following and similar endpoints, when `{id}` is the authenticated user who also owns the developer app. Search recent returns posts, so it bills at $0.005 per post returned. The same resource is charged once per UTC day.
- **The 402.** HTTP 402 means the balance is zero, and requests fail until you top up or auto-recharge fires. Auto-recharge runs at most once every 5 minutes and pauses at a zero or negative balance. A separate per-cycle spending limit you set also blocks requests once reached. A 34-hour stall means auto-recharge was off or blocked, or the cap was hit. Free credits: $20 for saving a first card plus a match of the first auto-recharge up to $50, live since 2 October 2026 and expiring after 3 months.
- **Reply restriction.** Since 23 February 2026, POST /2/tweets replies on self-serve tiers are "only permitted if the replier has been explicitly summoned by the original post's author", meaning an @mention or a quote. Enterprise and Public Utility apps are exempt. Of the 50 replies/day, only replies to people who mentioned the account (or replies inside its own threads) will go through.
- **Official MCP.** XMCP is "an official MCP server that exposes X API endpoints as callable tools". It builds tools from the OpenAPI spec (200+). It is hosted at `api.x.com/mcp` since 30 June 2026 (`xurl mcp` bridge for OAuth 2.0 PKCE), or self-hosted from `xdevplatform/xmcp`. It needs your own developer app and credits. The MCP layer itself is free.
- **Monthly cost (30 days), with "reads" meaning resources returned, not API calls:**

| Line | Volume/month | Unit | Cost |
|---|---|---|---|
| 5 plain posts/day | 150 | $0.015 | $2.25 |
| 50 summoned replies/day | 1,500 | $0.010 | $15.00 |
| 500 third-party post reads/day | 15,000 | $0.005 | $75.00 |
| **Base case** | | | **≈ $92/month** |
| Variant: reads mostly own mentions/timeline (owned) | 15,000 | $0.001 | $15 → total ≈ $32 |
| Variant: every post has a URL, replies billed as normal posts, reads are user lookups | | $0.20 / $0.015 / $0.010 | $30 + $22.50 + $150 ≈ $203 |

- **Browser risk.** On 14 February 2026, X head of product Nikita Bier posted: "If a human is not tapping on the screen, the account and all associated accounts will likely be suspended—even if you're just experimenting… If it's critical, you can use the official API." Bier reported about 208 suspensions per minute at the peak of the April 2026 purge. On 24 July 2026 he posted: "We found 42,000 accounts automating replies using chatbots and have removed them from the platform." X sued engagement manipulators on 17 September 2026 after suspending their accounts on 18 August. The 102 parked browser posts should go through the API instead.

### 4. YC Work at a Startup
- **No API.** No public or official candidate API or MCP exists as of November 2026. The best public evidence is a 2023 Ask HN thread ("no API… quite strong measures to restrict machine access"), which is old. For 2026 the status is unknown, and no evidence of an official API turned up.
- **Terms.** Two 2026 open-source projects (open-vacancy-radar #619, joule #113) independently read YC's Terms of Use as prohibiting "scraping, data mining, robots, and similar extraction methods". They concluded that a user's own logged-in session does not override this. `workatastartup.com/robots.txt` disallows nothing, but that does not grant permission.
- **Enforcement.** No 2025–2026 cases known.
- **Third-party.** Apify actors read public job listings only. None applies or messages founders.
- **Recommendation.** At 12 sessions/month, keep a browser-assisted flow where the agent drafts and a human clicks Send. Founder messaging is reputational, and automated outreach is exactly what the terms target.

### 5. WhatsApp (personal)
- **Cloud API.** The Cloud API is the WhatsApp Business Platform: it needs a business portfolio and a registered business phone number, not a personal account.
- **AI-provider ban.** Meta added an "AI Providers" section to the Business Solution Terms in October 2025. It applied immediately to new API users from 15 October 2025 and to everyone from **15 January 2026**, with a 90-day grace period. Exceptions are the EEA, Italy and Brazil. Two providers pulled their assistants: Microsoft's Copilot blog (November 2025) said Copilot on WhatsApp would be discontinued on 15 January 2026, and OpenAI said on 21 October 2025 that ChatGPT would not be available on WhatsApp after that date. Even a business number could not legally host a general personal assistant outside those markets.
- **Unofficial clients.** WhatsApp's help center says linking to unofficial apps "violates our Terms of Service". Documented enforcement:
  - whatsmeow issue #810 (May 2025): "your account may be at risk… unauthorized tools" warnings and bans for whatsmeow and Baileys users.
  - Baileys issue #2850 (7 October 2026): bans after bulk sends.
  - Lawsuits against individual users of these bridges: unknown, none found.
  - Vendor-published ban rates, such as Kraya AI's claim that 68% of Indian SMBs on unofficial tools were banned within 12 months, are marketing for official-API vendors. Treat them as directional only.
- **Bridge status.**
  - lharries/whatsapp-mcp's last commit was 13 July 2025. Its pinned whatsmeow version fails with "Client outdated (405)" when WhatsApp changes the protocol, which plausibly explains much of the 18% error rate.
  - Star counts conflict across directories (5.7k–6.7k). The October 2026 figure was ">6.6k".
  - The maintained continuation is verygoodplugins/whatsapp-mcp (whatsmeow plus FastMCP, webhooks, HTTP transport, Go 1.25+); its star count is unknown, with one listing showing 252 in October 2026.
  - jlucaso1/whatsapp-mcp-ts (Baileys, about 75 stars, last push 23 January 2026) is stale.
- **Recommendation.** Migrate to the maintained fork and pin whatsmeow to latest. Rate-limit sends, avoid bulk or new-contact messaging, and keep the agent mostly reading and drafting.

## Recommendations (one line each)
1. **Luma.** Add `mcp.luma.com` for discovery and attendance lookups now. Keep the browser for the registration click only; don't buy Luma Plus, because the API can't register you.
2. **Facebook/Messenger.** Keep the browser with a human confirming each comment reply or DM. No official API exists, and cookie libraries put the person's real account at risk.
3. **X.** Stay on the official API, through XMCP if convenient. Turn on auto-recharge (for example $25 below a $5 floor) and a monthly cap of about $150, and alert on `GET /2/usage/credits`. Route the 102 parked posts through the API and only reply when summoned.
4. **YC WaaS.** Keep a human-approved browser flow, or drop automation of founder messages. There is no API and the terms ban robots.
5. **WhatsApp.** Replace lharries/whatsapp-mcp with verygoodplugins/whatsapp-mcp this week and throttle sends. There is no official personal-account route, and the old bridge is abandoned.

## Caveats
- Most X pricing figures were cross-checked across docs.x.com snippets and several 2026 vendor blogs. X has changed prices repeatedly (pilot → February → April 2026), so re-check the Developer Console before budgeting. One X Enterprise page still says a "2 million" read cap while the pricing page says 3 million. I used the pricing page.
- Some GitHub star counts and last-commit dates come from directories and snippets, not the repo pages, and some are missing ("unknown"). Verify them before relying on them.
- Luma's Terms of Use on automated access were not verified, and third-party scraper vendors contradict each other.
- The Luma Plus price ($59/month billed annually, $69 month to month) is confirmed by Luma's own help center as of September 2026.
- I found no 2025–2026 enforcement cases for Luma or YC, and none against individual personal-profile Facebook automation. That is "unknown", not "safe".
- The 42,000-account removal and the February 2026 warning come from Nikita Bier's own posts on X. The 208-per-minute figure comes from a secondary report.

## Sources
https://reduck.ai/use-cases/auto-rsvp-luma-events
https://awesomeclaude.ai/how-to/use-whatsapp-with-claude
https://help.luma.com/p/mcp
https://docs.luma.com/reference/getting-started-with-your-api
https://glama.ai/mcp/servers/montaguegabe/luma-events-mcp
https://github.com/Blackie360/luma-events-mcp
https://www.outstand.so/blog/facebook-api
https://www.upload-post.com/facebook-graph-api/
https://www.breakreach.com/developers/facebook-api
https://developers.facebook.com/documentation/business-messaging/messenger-platform/send-messages
https://soku.ai/blog/meta-ads-mcp-guide
https://www.blotato.com/blog/facebook-api-pricing
https://facebookscraperapi.com/blog/is-scraping-facebook-legal
https://github.com/fukata/messenger-mcp
https://github.com/m008v/fbchat-v2
https://github.com/m008v/fbchat-v2/releases/tag/v2.3.4
https://x.com/XDevelopers/status/2026084506822730185
https://docs.x.com/tools/mcp
https://mcpplaygroundonline.com/mcp-servers/x-twitter
https://docs.x.com/overview
https://docs.x.com/x-api/getting-started/pricing
https://docs.x.com/changelog
https://crypto.news/x-expands-content-to-ai-prompts-outputs-in-2026-terms-update/
https://youreputationsolution.com/blog/x-account-suspended-inauthentic-behavior-appeal/
https://korben.info/en/nitter-xcancel-dead-explanation-in-code.html
https://github.com/orgs/xdevplatform/repositories
https://github.com/xdevplatform/xmcp
https://github.com/xdevplatform/xmcp/pull/26
https://github.com/JacobFV/agent-twitter-client
https://github.com/unclecode/twikit
https://github.com/d60/twikit
https://www.postzen.dev/blog/twitter-api-pricing
https://devcommunity.x.com/t/update-to-reply-behavior-in-x-api-v2-restricting-programmatic-replies/257909
https://github.com/jortega0033/open-vacancy-radar/issues/619
https://apify.com/automation-lab/ycombinator-jobs-scraper
https://apify.com/nomad-agent/ycombinator-was-scraper
https://www.visitoai.com/blog/whatsapp-ai-assistant-policy
https://wa.genuka.com/en/docs/compare/unofficial-whatsapp-apis
https://whatsapp.checkleaked.cc/blog/whatsmeow-go-whatsapp-libraries
https://205055.xyz/skill/lharries/whatsapp-mcp/
https://github.com/whiskeysockets/Baileys
https://github.com/WhiskeySockets
https://github.com/iFixRobots/whatsmeow/pull/3
https://github.com/jlucaso1
https://help.luma.com/p/luma-api
https://www.metabase.com/integrations/luma
https://www.basedlabs.ai/articles/luma-mcp
https://bundle.social/blog/facebook-graph-api
https://postproxy.dev/blog/facebook-graph-api-posting-guide/
https://www.postzen.dev/blog/facebook-graph-api-post-to-page
https://developers.facebook.com/documentation/business-messaging/messenger-platform/overview
https://postproxy.dev/how-to/send-facebook-messenger-api/
https://www.usecarly.com/blog/meta-ads-mcp/
https://scrapeops.io/websites/facebook/
https://webscraping.cc/blog/how-to-scrape-facebook/
https://techcrunch.com/2022/07/06/meta-sues-chinese-companys-us-subsidiary-for-scraping-facebook-and-instagram-data
https://techxplore.com/news/2026-11-meta-delays-business-terms-outcry.html
https://www.niemanlab.org/2026/10/metas-new-commercial-terms-will-allow-the-scraping-of-news-sites-legal-experts-say/
https://twexapi.io/x-api-pricing
https://socialnexis.com/guides/x-api-basic-enterprise-automation-rules
https://docs.x.com/fundamentals/developer-portal
https://opentweet.io/x-api-pricing
https://api.sorsa.io/blog/twitter-api-pricing-2026
https://replysurge.com/blog/x-api-reply-restriction-what-still-works
https://kalen.si/blog/x-api-pricing-per-post
https://skills-hub.ai/blog/x-xmcp-official-mcp-server-2026
https://socialnexis.com/guides/x-june-2026-bot-purge-anatomy
https://bitcoinethereumnews.com/bitcoin/x-lawsuit-engagement-manipulation-targets-bitcoin-influencers/
https://news.ycombinator.com/item?id <tg-id>
https://github.com/Rhovian/joule/issues/113
https://apify.com/scrapepilot/yc-startup-jobs-scraper----companies-jobs-founders
https://medium.com/@shubhodey/why-whatsapp-bans-accounts-and-how-to-make-sure-it-never-happens-to-you-5790a5626a0d
https://blog.kraya-ai.com/whatsapp-automation-ban-risk
https://socialmate.app/blog/baileys-whatsapp/
https://blueticks.co/blog/whatsapp-api-for-ai-agents
https://enterprisedna.co/directories/mcp/lharries-whatsapp-mcp/
https://translate.google.com/translate?u=https%3A%2F%2Fgithub.com%2Flharries%2Fwhatsapp-mcp&hl=ar&sl=en&tl=ar&client=srp
https://github.com/verygoodplugins/whatsapp-mcp
https://github.com/DimitrisPasakaleris/whatsapp-mcp
https://github.com/mi4uu/whatsupd
https://blueticks.co/blog/best-whatsapp-mcp-servers
https://docs.x.com/x-api/usage/introduction
https://docs.x.com/enterprise-api/getting-started/pricing
https://apify.com/nexgensignal/luma-events-scraper
https://apify.com/pumpkinapi/luma-events-scraper
https://mcp.luma.com`,
https://mcp.luma.com`
https://mcp.luma.com`.