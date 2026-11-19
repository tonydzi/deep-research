# DR26-11-12-HUB-01-0234 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

**# DR26-11-12-HUB-01-0234 — Коннекторы под дыры флота: Luma, Facebook/Messenger, X API, YC WaaS, WhatsApp**

**Research questions:** For each of Luma (attendee discovery + registration), Facebook personal-profile comments + Messenger DMs, X (posting/replies/mentions/search), YC Work at a Startup (applications + founder messaging), and personal WhatsApp: official API/MCP coverage of the exact agent job, pricing, ToS risk of unofficial/browser routes with 2025–2026 enforcement, best maintained third-party MCP/SDK, and a one-line recommendation.  
**Scope / Date:** November 2026; primary sources opened via search + full-page fetch (docs.x.com, docs.luma.com, help.luma.com, developers.facebook.com, Meta WhatsApp docs, GitHub/npm listings, secondary pricing round-ups that quote the same official pages). 20+ primary/high-authority sources used.  
**Overall confidence:** High on existence/scopes/pricing of official rails (directly from vendor docs); Medium on concrete 2025–2026 ban/enforcement counts (patterns are clear and repeated across multiple independent reports; named case volumes are thinner); Low/unknown where no 2026 source confirms a detail.

## Executive Summary
- Luma has a real official public API (host-scoped, Luma Plus) and an official free MCP (`https://mcp.luma.com`) that covers discovery and host/own-event management, but not automated self-registration as an attendee on arbitrary third-party events. Confidence: High.  
- Facebook personal-profile comments, replies, and personal Messenger DMs have **no** official Graph/Messenger API path in 2026; Pages/Messenger Platform is Page-only. Confidence: High.  
- X API v2 is fully official, pay-per-use only (no free tier for new accounts), with clear unit prices and a 3 M post-read monthly cap. Official MCP exists. Confidence: High.  
- YC Work at a Startup has **no** official public API or MCP for candidate applications or founder messaging. Confidence: High.  
- WhatsApp official API is Business Platform / Cloud API only (business numbers); personal WhatsApp has none. Unofficial bridges carry documented permanent-ban risk. Confidence: High.  
- Browser/unofficial routes for all five violate typical platform ToS on automated access; enforcement is real but not uniformly quantified in public 2025–2026 case counts.  

## Detailed Findings

### A. Official API / official MCP coverage of the agent’s actual job (Nov 2026)

**1. Luma (luma.com / lu.ma) — discover events + register as attendee**  
Official public REST API exists at `https://public-api.luma.com` (OpenAPI available). It is calendar-scoped and requires Luma Plus. Capabilities are host/management-oriented: create/update events, add/approve/waitlist guests, list guests, send invites/blasts, ticket types, coupons, memberships, etc. Self-registration as a guest on an arbitrary third-party event is **not** exposed; guests register via the public event page form (name + email required; no account mandatory).  
Official MCP server at `https://mcp.luma.com` is free for any Luma account (no Plus needed). It supports public-event discovery (keyword/location/date), listing events you attend or host, and full management of events you host (including guest approval). It does not provide a documented “register myself as attendee on this event” tool.  
Sources: https://docs.luma.com/reference/getting-started-with-your-api (Plus required, base URL, host management); https://help.luma.com/p/mcp (MCP capabilities and free access); https://help.luma.com/p/luma-api; https://docs.luma.com/reference/post_v1-events-guests-add and list endpoints.  

**2. Facebook + Messenger (personal profile, not Page)**  
No official API covers reading/replying to comments on a personal-profile post or personal Messenger DMs. Graph API Pages endpoints and Messenger Platform are Page-scoped; attempting to comment as a User returns error 1705. Personal timeline publishing is gone. User-profile data is heavily restricted (app-scoped, consent-required, or removed post-2018).  
Sources: https://developers.facebook.com/docs/pages-api/comments-mentions (explicit User-comment failure); Messenger Platform docs (Page-only); multiple 2026 Graph API references confirming personal-profile write/read limits.  

**3. X (x.com)**  
Official API v2 fully covers posting, replying, reading mentions, timelines, and search (subject to scopes and pay-per-use). Official hosted MCP at `https://api.x.com/mcp` plus open-source `xmcp` exist and expose the same endpoints via OAuth 2.0 / Bearer.  
Sources: https://docs.x.com/x-api/getting-started/pricing (and linked developer docs); community round-ups that quote the same page (e.g. https://opentweet.io/x-api-pricing, https://kalen.si/blog/x-api-pricing-per-post).  

**4. YC Work at a Startup (workatastartup.com)**  
No official public API or MCP for candidate applications, job search under login, or messaging founders. FAQ describes a single human profile that companies can browse; applications and conversations stay on the site. Unofficial community MCPs exist that reuse a logged-in session cookie.  
Sources: https://www.workatastartup.com/about/ (FAQ); https://www.netrows.com/blog/best-ycombinator-startup-data-apis-2026 (explicit “no public API”); community MCP listings (npm `mcp-workatastartup`, Glama/LobeHub entries) that self-describe as unofficial.  

**5. WhatsApp (personal)**  
No official API for personal WhatsApp. Official product is the WhatsApp Business Platform / Cloud API, which requires a business phone number, Meta Business portfolio, display-name review, and (for higher limits) business verification. Personal numbers must be deleted from personal WhatsApp before they can be moved to the Business API.  
Sources: https://developers.facebook.com/docs/whatsapp/cloud-api/get-started and Cloud API overview; multiple 2026 comparisons (https://atmet.app/en/blog/whatsapp-business-vs-personal, https://theautomate.ai/blog/whatsapp-business-api/, https://voltade.com/blog/how-to-get-whatsapp-api).  

### B. Price (exact current, Nov 2026)

**X API (pay-per-use credits only; legacy Free/Basic/Pro closed to new sign-ups):**  
- Post create: $0.015 (plain) / $0.20 (contains URL) / $0.010 (summoned reply).  
- Post read: $0.005 per post returned; User read $0.010; Owned reads (own posts/mentions/etc.) $0.001.  
- Cap: 3 million post reads per monthly billing cycle; above that → Enterprise (custom).  
- No subscription, no minimum; credits bought in Developer Console; auto-recharge and spend caps available. Deduplication within 24 h UTC day.  
Realistic monthly for 5 plain posts/day + 50 replies/day + 500 reads/day ≈ (55 × 30 × $0.015) + (500 × 30 × $0.005) ≈ $24.75 + $75 = **~$100** (higher if any URLs or expansions after the Nov 22 2026 billing change on expansions).  
Sources: https://docs.x.com/x-api/getting-started/pricing (full rate card); corroborating tables on https://opentweet.io/x-api-pricing, https://social-sdk.dev/guides/x-api-pricing, https://www.twittrapi.com/blog/x-api-pricing-2026 (all dated 2026 and quoting the official page).  

**Luma:** API requires Luma Plus ($59/mo on the calendar). MCP itself is free.  
**Facebook personal / YC WaaS / personal WhatsApp:** no paid official option that covers the job. WhatsApp Business Cloud API is per-delivered-message (country- and category-dependent; first 1 000 service replies/month free per number since 1 Oct 2026); exact rates on Meta’s card.  

### C. ToS risk of unofficial / browser routes (2025–2026)

All five platforms prohibit unauthorized automated access, reverse-engineering, or bulk/auto-messaging in their terms. Browser-driven sessions and unofficial bridges (Baileys, whatsapp-web.js, session-cookie MCPs, Playwright) are therefore non-compliant.  

- **WhatsApp:** Highest documented risk. Terms ban automated/bulk messaging and unofficial clients; Help Center and library READMEs warn of temporary or permanent bans. Multiple 2026 reports (Baileys issues, industry blogs) describe sudden permanent bans without appeal; malicious npm forks of Baileys were also published in late 2026. Sources: https://theautomate.ai/blog/whatsapp-web-api/, https://vectalk.com.br/blog/en/official-vs-unofficial-whatsapp-api/, https://squarecloud.app/en/learning/bots/whatsapp-bot-banned, https://sporesec.com/en/blog/whatsapp-unofficial-api-ban-risk, GitHub advisories on trojanized Baileys (Nov 2026).  
- **Facebook personal:** Browser automation of personal profiles/Messenger is against Platform Terms; account locks for automation are reported.  
- **X:** Browser/session automation risks suspension; official API is the compliant path.  
- **Luma / YC WaaS:** Specific 2025–2026 public enforcement case counts are sparse (“unknown” for volume); general automated-access clauses apply and community tools explicitly disclaim affiliation and warn of breakage/ToS.  

### D. Best maintained third-party MCP / SDK (stars + last activity, Nov 2026)

- **Luma:** Official MCP (`https://mcp.luma.com`) is the best and actively supported. Community wrappers exist (e.g. adelaidasofia/luma-mcp) but are secondary.  
- **X:** Official hosted MCP + `xdevplatform/xmcp` (hundreds of stars, maintained). Community servers using official API keys (e.g. rafaljanicki/x-twitter-mcp-server ~35 stars) are preferable to pure session/scraper ones.  
- **YC WaaS:** Unofficial only — `mcp-workatastartup` (npm, Oct 2026) and similar cookie-based servers. No high-star official-equivalent.  
- **Facebook personal / WhatsApp personal:** No safe high-quality official MCP. Community options exist but inherit the ToS/ban risk above.  

### E. Recommendation (one line each)

- **Luma:** Official MCP for discovery + own-event management; keep carefully rate-limited browser only for third-party self-registration (API does not cover it). Decisive fact: MCP free and covers discovery; API is host-only + Plus.  
- **Facebook personal + Messenger:** Drop or accept high ban risk of browser; no official rail. Decisive fact: Pages/Messenger API is Page-only; User comment returns 1705.  
- **X:** Official API / official MCP. Decisive fact: full coverage + transparent pay-per-use (~$100/mo for the stated volume of plain posts/replies/reads).  
- **YC WaaS:** Keep browser with human confirmation gates or drop; no official option. Decisive fact: no public API.  
- **WhatsApp personal:** Drop unofficial bridges; move to Business Cloud API only if the number can be a business number. Decisive fact: personal has no official API; unofficial carries documented permanent-ban risk.  

## Synthesis and Implications
The fleet’s measured browser volume is highest where official coverage is weakest or absent (WhatsApp unofficial, Facebook personal, YC, Luma self-registration). X is the clearest “switch to official” win. Luma MCP closes most of the discovery gap at zero marginal API cost. Personal Facebook and personal WhatsApp remain structural holes: the only durable options are (a) convert the use-case to a Page/Business number or (b) accept elevated account-loss risk.  

## Limitations and Open Questions
- Exact Luma Plus price and feature gating should be re-checked on the live billing page (one 2026 source lists $59).  
- Concrete public counts of 2025–2026 account bans for Luma/YC browser automation are thin.  
- X expansion billing change (22 Nov 2026) may increase effective read cost; verify current Developer Console rates.  
- WhatsApp Business message rates are country-specific and change; use Meta’s live card.  

## Source Catalog (key opened URLs)
- https://docs.x.com/x-api/getting-started/pricing  
- https://docs.luma.com/reference/getting-started-with-your-api  
- https://help.luma.com/p/mcp  
- https://help.luma.com/p/luma-api  
- https://developers.facebook.com/docs/pages-api/comments-mentions  
- https://developers.facebook.com/docs/whatsapp/cloud-api/get-started  
- https://www.workatastartup.com/about/  
- https://opentweet.io/x-api-pricing  
- https://theautomate.ai/blog/whatsapp-web-api/  
- https://vectalk.com.br/blog/en/official-vs-unofficial-whatsapp-api/  
- plus the additional 2026 pricing, MCP, and ban-risk pages cited inline above.  

## Table

| Item | Official option | Price | ToS risk (unofficial/browser) | Best non-official | Recommendation |
|------|-----------------|-------|-------------------------------|-------------------|----------------|
| Luma | API (host-only, Plus) + MCP (discovery + own events) | Plus ~$59/mo for API; MCP free | Medium (automation clauses; sparse public 2025–26 cases) | Official MCP itself | Official MCP for discovery/own; browser only for 3rd-party self-reg |
| Facebook personal + Messenger | None for personal profile | N/A | High (Platform Terms; account locks reported) | Various scrapers/MCPs (high risk) | Drop or accept high ban risk |
| X | Full API v2 + official MCP | Pay-per-use: post $0.015/$0.20, read $0.005; ~$100/mo for 5p+50r+500reads/day | Medium-high (session automation) | Official MCP / API-key community servers | Official API/MCP |
| YC WaaS | None | N/A | Medium (session reverse-eng; self-described unofficial) | mcp-workatastartup (cookie) | Keep browser + human gates or drop |
| WhatsApp personal | None (Business Cloud API only) | Per-message (Business); free for first 1k service replies | High (documented permanent bans) | Baileys / whatsapp-web.js (explicit ToS violation) | Drop unofficial; Business API only if number can be business |

Every row is supported by the opened primary sources listed above. Where a 2026 source does not confirm a detail, the text states “unknown”.

## Sources (webSearchResults)
https://www.ycombinator.com/companies/industry/apis
https://www.netrows.com/blog/best-ycombinator-startup-data-apis-2026
https://raw.githubusercontent.com/yc-oss/api/main/README.md
https://www.ycombinator.com/companies/industry/api/san-francisco-bay-area/hiring
https://www.youtube.com/shorts/c3TxAUir2R8
https://developers.facebook.com/docs/messenger-platform/identity/
https://searchshiftreport.com/articles/facebook-comment-dm-auto-replies-5-limits-2026
https://developers.facebook.com/docs/messenger-platform/send-messages/personas/
https://postproxy.dev/blog/direct-messages-now-available/
https://developers.facebook.com/docs/messenger-platform/reference/
https://developers.facebook.com/docs/messenger-platform/identity/user-profile
https://developers.facebook.com/docs/messenger-platform/reference/messenger-profile-api
https://www.outstand.so/changelog/changelog-2026-10-22
https://social-api.ai/platforms/facebook
https://developers.facebook.com/documentation/pages-api/comments-mentions
https://opentweet.io/x-api-pricing
https://kalen.si/blog/x-api-pricing-per-post
https://social-sdk.dev/guides/x-api-pricing
https://www.twittrapi.com/blog/x-api-pricing-2026
https://www.upload-post.com/x-api-pricing/
https://www.outstand.so/blog/x-api-pricing
https://ampost.io/blog/x-api-pricing
https://opentweet.io/answers/does-x-premium-business-include-api-access
https://www.monocrawl.com/guides/x-api-pricing
https://www.postzen.dev/blog/twitter-api-pricing
https://buzzyx.com/guides/x-api-pricing
https://opentweet.io/answers/twitter-api-200-tier-replacement
https://www.twittrapi.com/blog/cheapest-way-to-post-to-x-programmatically
https://www.lilachbullock.com/x-api-pricing/
https://kalen.si/blog/twitter-api
https://help.lu.ma/p/event-registration-process
https://help.luma.com/p/webhooks
https://docs.luma.com/reference/post_v1-events-create
https://docs.luma.com/reference/post_v1-events-guests-add
https://help.lu.ma/t/events
https://docs.sim.ai/integrations/luma
https://docs.luma.com/llms.txt
https://help.lu.ma/
https://apis.io/apis/luma/luma-events-api/
https://docs.luma.com/reference/post_v1-calendars-events-add
https://dev.saasworthy.com/product/lu-ma
https://x.com/lagunacarta/status/2117296956582056137
https://www.apkmirror.com/apk/luma-labs-inc/luma-events-invites/
https://apis.io/providers/luma/
https://developers.facebook.com/docs/whatsapp/cloud-api/get-started
https://atmet.app/en/blog/whatsapp-business-vs-personal
https://developers.facebook.com/docs/whatsapp/cloud-api
https://theautomate.ai/blog/whatsapp-business-api/
https://voltade.com/blog/how-to-get-whatsapp-api
https://blog.mercately.com/en/whatsapp-vs-whatsapp-business
https://voltade.com/learn/whatsapp-business-api
https://www.kodekalabs.com/en/blog/whatsapp-business-api-vs-aplikasi/
https://voltade.com/blog/whatsapp-vs-whatsapp-business
https://theautomate.ai/blog/whatsapp-vs-whatsapp-business/
https://help.luma.com/p/mcp
https://lumalabs.ai/news/new-luma-mcp
https://github.com/lumalabs/luma-api-mcp
https://x.com/LumaLabsAI/status/2120213590510907820
https://mcp.film/mcps/luma-official.md
https://x.com/LumaLabsAI/status/2118372770954313898
https://github.com/adelaidasofia/luma-mcp
https://github.com/JochenYang/luma-mcp
https://libraries.io/pypi/mcp-luma
https://github.com/bobtista/luma-ai-mcp-server
https://docs.x.com/x-api/getting-started/pricing
https://devcommunity.x.com/t/important-update-legacy-x-api-basic-plans-are-moving-to-pay-per-use-ppu/266305
https://blog.lots.social/x-api-pricing-and-access
https://futuretweets.com/x-api-expansions-billing-change-2026/
https://climbx.so/blog/x-api-limits
https://developers.facebook.com/docs/content-library-and-api/content-library-api/guides/fb-profiles/
https://developers.facebook.com/docs/graph-api/reference/comment/comments/
https://socialtoai.com/platforms/facebook/
https://cdn.cdata.com/help/RBN/rcmd/pg_table-comments.htm
https://www.volcengine.com/article/3112611
https://scrapecreators.com/blog/facebook-comments-api
https://developers.facebook.com/docs/marketing-api/reference/user/
https://developers.facebook.com/docs/content-library-and-api/content-library-api/guides/fb-comments/
https://developers.facebook.com/docs/graph-api/reference/v2.1/object/comments
https://www.workatastartup.com/about/
https://www.job-search-tools.site/research/how-to-get-a-job-at-a-yc-startup/
https://github.com/yc-software/recruiting
https://cuberfy.com/en/resources/y-combinator-work-at-a-startup
https://zerogtalent.com/vc-jobs/y-combinator/gtm-work-at-a-startup-111158
https://ycombinator.com/blog/announcing-ycs-work-at-a-startup/
https://www.ycombinator.com/howtoapply/
https://npmjs.com/package/mcp-workatastartup
https://www.ycombinator.com/blog/jared-friedman-and-matt-long-discuss-work-at-a-startup
https://theautomate.ai/blog/whatsapp-web-api/
https://vectalk.com.br/blog/en/official-vs-unofficial-whatsapp-api/
https://helix.ml/threat-watch/i/trojanized-baileys-forks-zhengliyoux-baileys-lekzsync-baileys-raiden-baileys-auto-follow-attacker-whatsapp-channels-on-c
https://squarecloud.app/en/learning/bots/whatsapp-bot-banned
https://whatsapp.checkleaked.cc/blog/avoid-whatsapp-ban
https://whatsapp.checkleaked.cc/blog/whatsapp-cloud-api-vs-unofficial
https://sporesec.com/en/blog/whatsapp-unofficial-api-ban-risk
https://richautomate.in/blog/whatsapp-unofficial-api-ban-risk-india-2026
https://www.empleado.uno/blog/api-no-oficial-de-whatsapp-riesgos-y-alternativas
https://doubletaponair.com/double-tap-warns-accessible-whatsapp-apps-could-get-your-number-banned-by-meta/
https://xautodm.com/blog/twitter-mcp-server-connect-claude-chatgpt-agents-to-x
https://glama.ai/mcp/connectors/io.github.TwitterAPIs/twitterapis-mcp
https://github.com/tiovikram/x.com-mcp-server
https://github.com/topics/twitter-mcp
https://insightfulpipe.com/blog/best-x-twitter-mcp-servers
https://glama.ai/mcp/servers?query=agent-twitter-client-mcp
https://github.com/Barresider/x-mcp
https://glama.ai/mcp/servers?query=Twitter+MCP+%28Model+Context+Protocol%29+Server
https://github.com/rafaljanicki/x-twitter-mcp-server
https://github.com/0xhijo/mcp_twitter
https://docs.luma.com/reference
https://glama.ai/mcp/servers/moutasem-isentemiz/workatastartup-mcp/tree
https://supergood.ai/blog/mcp-for-software-without-a-public-api
https://lobehub.com/mcp/mit9-workatastartup-mcp
https://www.ycombinator.com/companies/mcp-use
https://glama.ai/mcp/servers/MIt9/workatastartup-mcp/tree
https://cotlet.dev/
https://lobehub.com/nl/mcp/moutasem-isentemiz-workatastartup-mcp
https://uphunt.io/blog/can-you-get-banned-upwork-mcp-automation-2026
https://modelcontextprotocol.io/registry/terms-of-service
https://docs.luma.com/reference/getting-started-with-your-api
https://help.luma.com/p/event-registration-process
https://help.luma.com/p/luma-api
https://docs.luma.com/reference/get_v1-events-guests-list
https://pipedream.com/apps/luma
https://glama.ai/mcp/servers/Blackie360/luma-events-mcp/tools/add_guests
https://developers.facebook.com/community/threads/1020927066824479/
https://developers.facebook.com/community/threads/1043357558345214/
https://developers.facebook.com/community/threads/352931387802696/
https://geekblog.net/blog/how-to-post-content-on-a-users-facebook-timeline/
https://developers.facebook.com/community/threads/922861757266796/
https://developers.facebook.com/docs/graph-api/reference/profile/
https://developers.facebook.com/community/threads/277949794555858/
https://www.netrows.com/blog/best-facebook-data-apis-2026
https://www.socialcrawl.dev/blog/facebook-data-api-2026