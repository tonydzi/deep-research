# DR26-11-16-MACANTON-01-1522 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Cheapest and Safest X/Twitter Rail for a Claude Code Agent — November 2026

## Executive conclusion

For the workload you specified, the best current architecture is **not** “replace the X API completely.” The cheapest low-risk design I found is:

> **Buffer Free for the 750 ordinary outbound posts + your own official X API app for replies, mentions, monitoring and snapshots.**

That gives you a **$0 marginal scheduler cost for the 750 ordinary posts**, while retaining the official X API for the pieces where Buffer’s public API/MCP is currently incomplete. Buffer’s Free plan supports three channels, its API is available on Free, and its official MCP can schedule posts and retrieve post analytics. Its API/MCP quota is **1,000 requests per channel per rolling 30 days on Free**, which is just enough for ~750 deterministic post-create calls if you keep your own 418-item queue outside Buffer and only feed the next few scheduled posts into Buffer. Buffer’s own API roadmap, updated **2026-11-15**, explicitly says a Community API for automated comment replies is still only “Exploring,” so I would **not** route the 1,000 automated replies through Buffer yet. citeturn349136view0turn349136view2turn349136view3  
Sources checked 2026-11-16:  
https://buffer.com/api  
https://developers.buffer.com/guides/integrations/mcp.html  
https://developers.buffer.com/roadmap.html

Using the official X pay-per-use prices currently published by X, my base-case estimate is approximately **$19.55/month ongoing** for the hybrid, or **$20.05 in month one** if you resolve and cache 50 monitored-account IDs once. That assumes the 50 watched accounts publish an average of **one unique post/account/day**. The monitoring component scales linearly with their actual post rate, because X bills post reads by resource, not by polling request. X currently lists ordinary Post Create at **$0.015**, URL Post Create at **$0.200**, Posts Read at **$0.005**, User Read at **$0.010**, and qualifying Owned Reads at **$0.001/resource**. X’s 24-hour UTC deduplication is also still documented. citeturn217039view2  
Source checked 2026-11-16; page exposes no publication date:  
https://docs.x.com/x-api/getting-started/pricing

There is a more important constraint than price: **your intended AI replies require policy approval from X.** X’s Automation Rules, updated **April 2026**, now state that deploying or operating an **AI-powered automated reply bot requires prior written and explicit approval from X**. They also prohibit unsolicited automated replies based merely on monitoring/search. Separately, X’s API changelog says that since **2026-02-23**, self-serve `POST /2/tweets` programmatic replies are only permitted when the original author has “summoned” the replier by mentioning the account or quoting one of its posts. So function 2 is potentially viable for genuine inbound engagement, but function 3 must **not** turn into automatic cold replies to watched accounts. citeturn106470view1turn850382view0  
Sources:  
https://help.x.com/en/rules-and-policies/x-automation — updated April 2026  
https://docs.x.com/changelog — event dated 2026-02-23

The recommendation therefore is:

**Claude Code → Buffer API/MCP for standalone publishing; Claude Code → official X MCP/xurl or direct X API for reads and eligible replies.** Keep monitoring informational. Let the agent propose a reply or quote, but do not auto-send a cold reply unless the interaction qualifies under X’s rules. For AI-generated inbound replies, get X’s written approval before unattended deployment.

## What X actually charges and permits in November 2026

Your remembered pay-per-use numbers are essentially correct.

| X operation | Current official price | Consequence for this project | Source/date |
|---|---:|---|---|
| Ordinary Post Create | **$0.015/request** | 750/month = **$11.25** direct | https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16; no page date shown citeturn217039view2 |
| Post Create containing URL | **$0.200/request** | No longer relevant under your no-links rule | same source citeturn217039view2 |
| “Summoned” reply | **$0.010/request** | 1,000 eligible replies = **$10.00** | https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16; https://docs.x.com/changelog — rule dated 2026-02-23 / pricing change dated 2026-04-16 citeturn217039view2turn850382view0turn850382view1 |
| Posts Read | **$0.005/resource** | Foreign-account monitoring | https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16 citeturn217039view2 |
| User Read | **$0.010/resource** | Daily profile/follower snapshot | same source citeturn217039view2 |
| Following / Followers Read | **$0.010/resource** | Reading membership records individually is more expensive than just reading profile counters | same source citeturn217039view2 |
| Owned Read | **$0.001/resource** | Own posts, own mentions, own followers/following where qualification conditions apply | same source citeturn217039view2 |
| X Activity API `post.create` webhook event | **$0.005/event** | More expensive than polling owned mentions if you only need periodic ingestion | same source citeturn217039view2 |

X explicitly documents deduplication of billable resource reads within the same **24-hour UTC billing window**, reset at midnight UTC, although it calls the mechanism a “soft guarantee.” citeturn217039view2  
Source: https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16.

Your hypothesis about the February reply restriction is also confirmed. The **2026-02-23** changelog entry says self-serve programmatic replies via `POST /2/tweets` are allowed only when the original author has “summoned” the replying account by **@mentioning it or quoting one of its posts**. Enterprise is exempt from that particular self-serve restriction. citeturn850382view0turn850382view3  
Source: https://docs.x.com/changelog — entry dated 2026-02-23.

There was another significant tightening on **2026-04-20**: X’s changelog says self-serve access removed API functionality for **Following, Likes and Quote Posts**, while introducing the current owned-read and write pricing. That matters for your “monitor → quote” workflow: a self-serve X app cannot simply automate quote-post creation the way older integrations did. citeturn850382view1  
Source: https://docs.x.com/changelog — announcement dated 2026-04-16, effective 2026-04-20.

The automation policy is stricter than the raw endpoint restriction. X’s Automation Rules say unsolicited automated replies are prohibited, including replies triggered solely by keyword/search monitoring; following someone does not constitute opt-in. Automated replies are allowed when the user has clearly indicated intent—for example by replying to your post or mentioning you—but **AI-powered automated reply bots require prior written explicit X approval**. citeturn106470view1  
Source: https://help.x.com/en/rules-and-policies/x-automation — updated April 2026.

X also explicitly warns against scripting its website and says non-API website automation can result in permanent suspension. Your “no Playwright/Selenium” constraint is therefore the correct one. citeturn106470view1  
Source: https://help.x.com/en/rules-and-policies/x-automation — updated April 2026.

The “Automated” account label is available as a transparency mechanism; the Help page describes labels as a way for automated accounts to identify themselves. I did **not** find wording on that page making the label universally mandatory for every partially automated account, so I would not state that @Tony_Stef_ is legally required to carry it. citeturn106470view2  
Source checked 2026-11-16: https://help.x.com/en/using-x/automated-account-labels.

For Claude Code, X now has a first-party hosted MCP endpoint:

`https://api.x.com/mcp`

It uses Streamable HTTP. Full write access is routed through X’s `xurl` OAuth flow and your own X developer application; bearer-only app authentication is read-only. The official docs also support headless OAuth setup through `xurl auth oauth2 --headless`. This does **not** bypass pay-per-use—the MCP is merely another interface to your own X app. citeturn217039view3  
Source checked 2026-11-16: https://docs.x.com/tools/mcp.

X added `?read_only=true` to its MCP on **2026-11-10**, useful if you want a hard safety boundary for your monitoring agent. citeturn217039view4  
Source: https://docs.x.com/changelog — entry dated 2026-11-10.

## Candidate comparison

“Service app” below means the user connects the X account to the vendor and does not supply an X developer key. Where the vendor does not explicitly document the legal/contractual status of that app, I mark it **service-side / exact X commercial arrangement unverified** rather than assuming they have an Enterprise exemption.

| Candidate | Publishing cost / limit | Agent replies & inbox | Foreign monitoring / metrics | Claude interface | Whose X app? | Risk / 2026 evidence |
|---|---|---|---|---|---|---|
| **X API v2 direct** | $0.015 ordinary post; $0.20 URL post. No scheduler subscription. | Summoned reply $0.010. Self-serve cold programmatic replies blocked since 2026-02-23. AI reply bot additionally needs written X approval. | Full official route; $0.005/post foreign reads, $0.001 qualifying owned reads, $0.010 user read. | Official MCP `https://api.x.com/mcp`, xurl, REST/XDK. | **Your app.** | **Lowest ToS risk.** Primary current docs. https://docs.x.com/x-api/getting-started/pricing; https://docs.x.com/changelog; https://docs.x.com/tools/mcp; https://help.x.com/en/rules-and-policies/x-automation. citeturn217039view2turn217039view3turn106470view1 |
| **Buffer Free** | **$0 plan**, 3 channels; Free API limit 1,000 requests/channel/30d. Queue limit is 10 scheduled posts/channel, not 10 total monthly posts. | Community exists in Buffer UI, but **Community API is not yet available**; roadmap lists comment-reply API as “Exploring” on 2026-11-15. | MCP can retrieve post analytics. No public MCP/API for arbitrary X-account monitoring. | Official HTTP MCP `https://mcp.buffer.com/mcp`; GraphQL API; CLI. | Service-side OAuth integration; Buffer identifies itself as an official API partner with X. Exact underlying X app/enterprise contract not exposed in docs I opened. | **Low risk / strongest third-party choice.** X publishing, analytics and engagement are current product features. https://buffer.com/api; https://developers.buffer.com/guides/integrations/mcp.html; https://developers.buffer.com/roadmap.html; https://support.buffer.com/articles/using-xtwitter-with-buffer-nA84XnWtuU — checked 2026-11-16. citeturn349136view0turn349136view2turn349136view3turn349136view4 |
| **Buffer Essentials** | **$6/channel/month** for channels 1–10. Higher API allowance: 3,000 requests/channel/30d. | Same Community-API limitation. | Same post analytics; arbitrary watchlist still requires X API. | Same official MCP/API/CLI. | Same as above. | Low risk. Pricing: https://support.buffer.com/en-us/articles/buffer-pricing-and-features-6pJrOPuzIt — checked 2026-11-16. citeturn349136view1 |
| **Typefully Creator** | Official pricing page is JS-rendered and its crawl does not expose dollars. Two independent checks of the rendered 2026 card report **$99/year**, 1,000 posts/month. | Typefully can reply/quote in its UI, but its public API/MCP docs I opened describe draft/publish workflows, not an engagement inbox tool. | X analytics on Creator according to current rendered-card reports; no arbitrary watchlist API documented. | Official Streamable HTTP MCP `https://mcp.typefully.com/mcp`; REST API; media upload. | Service-side X connection; exact X commercial arrangement not stated in opened docs. | Reasonable publishing rail, but less verifiable than Buffer for this specific architecture. MCP page dated 2026-08-28; API page dated 2026-05-05. citeturn286879view0turn286879view1 |
| **Postiz Cloud** | Standard **$29/mo**, 5 channels, unlimited posts; MCP/API/CLI included. | Engagement capability exists in the product, but I did not verify an X comment-reply agent endpoint strong enough to substitute for official X reads. | Analytics available; no verified arbitrary X watchlist solution. | Official remote MCP. | **Unclear from Cloud docs.** Self-host clearly uses instance credentials; do not assume Cloud does. | Medium until Postiz answers which X app/contract Cloud uses after X’s 2026 service-bureau changes. https://postiz.com/pricing; https://postiz.com/mcp — checked 2026-11-16. citeturn349136view5turn349136view6 |
| **Postiz self-host** | Software can be self-hosted, but X costs remain yours. | Depends on X API capabilities. | Depends on your X API. | Self-host API/MCP. | **BYO X credentials** in Postiz’s X provider configuration. | Similar X risk/cost to direct API; mainly adds UI/queue infrastructure rather than saving X fees. Source opened: https://github.com/gitroomhq/postiz-docs/blob/main/self-host/providers/x-twitter.mdx — checked 2026-11-16. |
| **Mixpost Lite self-host** | **Free software**; Lite explicitly supports X profiles and basic analytics. | Engagement Inbox is Pro, not Lite. | X data still goes through your developer app. | Built-in Streamable HTTP MCP at `https://your-instance/mcp`, including Claude Code personal-key auth. | **BYO X app explicitly required.** | Safe from a credential-model perspective but no X cost reduction. https://mixpost.app/pricing; https://docs.mixpost.app/services/social/x/; https://docs.mixpost.app/mcp/ — checked 2026-11-16. citeturn286879view3turn286879view4turn286879view5 |
| **Ayrshare Premium** | **$149/mo** for one social profile; unlimited posts/API calls advertised on paid plans. | Engagement API exists. | Analytics API exists; broader creator/history functionality depends on plan/API. | API + MCP. | **BYO X app is now Ayrshare’s documented model.** | Financially poor for one account because you pay Ayrshare **and** X PPU. Ayrshare says X forced a BYO pattern from 2026-03-31. I confirmed the X Developer Agreement’s service-bureau restriction, but did **not** find an X primary source explicitly announcing that March 31 date. https://www.ayrshare.com/pricing/ — checked 2026-11-16; https://www.ayrshare.com/blog/x-api-compliance-byo-app/ — dated 2026-05-15; https://docs.x.com/developer-terms/agreement — updated 2026-04-27. citeturn349136view7turn349136view8turn742697view0 |
| **Upload-Post** | Basic **$24/mo**, Professional $50, Advanced $147; current pricing page says all paid plans have unlimited uploads. | “First comment” support is documented, but that is not equivalent to a programmable X inbox. | MCP includes `get_analytics` / `get_post_analytics`; no arbitrary account monitor verified. | Hosted MCP `https://mcp.upload-post.com/mcp`; API. | Service-side connection; no own X developer account needed. | Medium until X contractual status confirmed. MCP page explicitly says no own X API. Pricing page and X pricing explainer are current 2026. https://www.upload-post.com/pricing/ — checked 2026-11-16; https://www.upload-post.com/mcp/x/ — FAQ updated June 2026. citeturn691574view1turn691574view4 |
| **bundle.social** | Pro **$100/mo**, 10,000 posts / 5,000 comments; its X page says X API charges are passed through at cost. | Comment allowance exists, but X-specific automated-reply eligibility still governs. | Analytics available; arbitrary monitoring not demonstrated as cheaper than direct X. | Official package `bundlesocial-mcp` / API. | Uses official APIs; exact tenant/X-app arrangement not fully established from opened pages. | Expensive here. Primary pages dated July 2026: https://bundle.social/pricing; https://bundle.social/x; https://bundle.social/social-media-mcp-server. |
| **Blotato Starter** | **$29/mo flat**, API/MCP included; vendor says no separate X API bill. | Pricing advertises “active contacts” including comment replies, but I did not verify enough X-specific inbox semantics to price 1,000 compliant replies through it. | Own analytics yes; arbitrary watchlist no verified endpoint. | Hosted MCP `https://mcp.blotato.com/mcp`; unattended Claude Code uses API-key-header configuration. | **Blotato holds the X connection and covers X metering.** | Medium: operationally current, but exact X commercial agreement not disclosed. Vendor guide published 2026-08-10, updated 2026-11-06. https://www.blotato.com/blog/how-to-post-to-x-with-claude-code; https://www.blotato.com/pricing. citeturn220145view7turn691574view6turn691574view7 |
| **Post for Me** | **$10/mo**, up to 1,000 successful posts/month. | API markets responses/conversation workflows, but whether X replies count toward the 1,000 quota and how the February summoned-reply rule is enforced is not explicit enough to use it as the reply rail without support confirmation. | Explicitly offers social feeds and post analytics; X page advertises user timelines and engagement metrics. | API; site links an MCP integration guide, but I could not open a stable direct MCP documentation URL. | Supports **either BYO credentials or “our” developer credentials**. | Very attractive price but medium compliance uncertainty because of the service-owned-credentials option after X’s 2026 service-bureau restriction. https://www.postforme.dev/pricing; https://www.postforme.dev/integrations/x — checked 2026-11-16. citeturn871037view0turn220145view5 |
| **OpenTweet Advanced** | **$29/mo**, 100 scheduled posts/day. Pro is $19.99 but capped at 20/day, so it does not fit 25/day. | MCP has many X tools, but I would require explicit confirmation of reply-rule enforcement before using it for unattended replies. | Analytics yes; inspiration/search tools exist, but not a verified replacement for your 50-account 4-hour monitor. | Hosted Streamable HTTP MCP `https://mcp.opentweet.io/mcp`; optional `npx @opentweet/mcp-server`. | Vendor says “OpenTweet handles the X connection” and no X developer account is needed. | Medium. Current help page updated **2026-10-12**. https://opentweet.io/help/pricing-plans; https://opentweet.io/twitter-mcp-server. citeturn220145view3turn220145view4 |
| **twitterapi.io** | Current live price file v2026-10-12 says Post Tweet **$0.0006**, media upload **$0.0006**, minimum call $0.00015. | Technically capable of write actions. | Extremely cheap public reads. | HTTP API; third-party/community MCP wrappers exist. | **Not official X OAuth API model.** | **High account/security risk for writes; high ToS/legal-compliance risk. Do not give it @Tony_Stef_ session credentials.** Current price: https://twitterapi.io/pricing/details — pricing.json version 2026-10-12. citeturn220145view8 |
| **SocialData** | Primarily a third-party data/read service rather than safe publishing rail. | Not a recommended write rail. | Typical tweet/user reads about **$0.0002/resource**; user monitor pricing for 11–100 monitors is about **$0.0062/hour/monitor**. | API; MCP documentation exists. | No Tony X app needed for public data. | Low direct credential risk if used read-only without Tony login, but materially higher X data-licensing/ToS risk than official X API. https://docs.socialdata.tools/getting-started/pricing/; https://docs.socialdata.tools/monitoring/pricing/ — checked 2026-11-16. citeturn961201view1turn961201view0 |

### Typefully price conflict / evidence quality

Typefully is the clearest case where I would **not pretend the primary scrape told us the price**. The official pricing URL was opened today but its dollar cards are rendered client-side and were absent from the extracted page:

https://typefully.com/pricing — checked 2026-11-16. citeturn286879view2

Two independent October 2026 captures of that rendered card agree on:

- Free: $0, 10 posts/month.
- Creator: **$99/year**, annual-only, 1,000 posts/month.
- Business: **$20/month/social set**, or **$18/month** billed yearly, 1,500 posts/month.

Sources:
https://justpricing.com/typefully-pricing — updated 2026-10-23. citeturn682476view0  
https://www.postel.app/typefully-pricing — updated 2026-10-30, says Typefully page checked 2026-10-30. citeturn682476view1

I therefore use **$99/year** for cost modeling, but classify the dollar figure as corroborated secondary evidence rather than a machine-readable primary price.

## Monthly cost model

The unavoidable ambiguity is monitoring volume. “50 accounts every four hours” determines request cadence but not billed X resources. X bills **returned unique posts**, and same-day repeated reads may deduplicate. Therefore I model:

`p = average number of new posts per monitored account per day`

Monthly foreign posts read:

`50 × 30 × p = 1,500p`

Official X cost:

`1,500p × $0.005 = $7.50p/month`

Source for $0.005/resource and 24-hour UTC dedup: https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16. citeturn217039view2

I use **p = 1** for the comparison table and give the sensitivity afterward.

### Direct official X baseline

For 750 ordinary no-link posts, 1,000 eligible inbound/summoned replies, 1,000 incoming mention resources, 50 watched accounts averaging one new post/day, one daily profile snapshot, and one metrics read of each of the 750 own posts:

| Component | Calculation | Monthly |
|---|---:|---:|
| 750 ordinary posts | 750 × $0.015 | **$11.25** |
| 1,000 eligible/summoned replies | 1,000 × $0.010 | **$10.00** |
| 1,000 owned mention reads | 1,000 × $0.001 | **$1.00** |
| Foreign monitoring, `p=1` | 1,500 × $0.005 | **$7.50** |
| Own-post metrics | 750 × $0.001 | **$0.75** |
| Daily follower/following profile snapshot | 30 × $0.010 | **$0.30** |
| **Ongoing total** |  | **$30.80/month** |
| Resolve 50 watchlist users once | 50 × $0.010 | **+$0.50 first month** |

All X unit prices: https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16. citeturn217039view2

So direct X is approximately **$31.30 in month one / $30.80 thereafter** at `p=1`.

This is actually more favorable than your historical bill because the 59 × $0.20 URL replies disappear entirely, and eligible replies now price at $0.010 rather than $0.20 URL-post pricing. citeturn217039view2turn850382view1

### Working hybrid configurations

The following all retain the official X API for the engagement/monitoring layer because none of the scheduler APIs has a sufficiently verified agent-accessible replacement for all four functions.

| Configuration | Publishing rail | Remaining official-X workload | Ongoing monthly at `p=1` | Normalized / 750 outbound posts |
|---|---:|---:|---:|---:|
| **Buffer Free + X** | **$0** | $10 replies + $1 mentions + $7.50 monitoring + $0.75 own metrics + $0.30 profile | **$19.55** | **$0.0261 all-in** |
| **Buffer Essentials + X** | $6 | same $19.55 | **$25.55** | **$0.0341 all-in** |
| **Typefully Creator + X** | $99/year = $8.25 economic monthly equivalent | same $19.55 | **$27.80 equivalent** | **$0.0371 all-in** |
| **Post for Me + X** | $10 | same $19.55 | **$29.55** | **$0.0394 all-in** |
| **Direct X only** | $11.25 | rest $19.55 | **$30.80** | **$0.0411 all-in** |
| **Postiz Cloud + X** | $29 | same $19.55 | **$48.55** | **$0.0647 all-in** |
| **Blotato + X** | $29 | same $19.55 | **$48.55** | **$0.0647 all-in** |
| **OpenTweet Advanced + X** | $29 | same $19.55 | **$48.55** | **$0.0647 all-in** |
| **Ayrshare Premium/BYO + X** | $149 | direct PPU still applies to the underlying X activity; using it purely as middleware provides no cost advantage | **≈$179.80** if workload remains equivalent | **≈$0.2397 all-in** |
| **Mixpost Lite self-host + X** | $0 software, excluding server | Same X usage as direct | **$30.80 + hosting** | **≥$0.0411** |
| **Postiz self-host + X** | self-host overhead | Same X usage as direct | **≥$30.80 + hosting** | **≥$0.0411** |

First month adds approximately **$0.50** to the official-X combinations if you resolve and persist the 50 watchlist account IDs.

The most useful comparison is **not** the “all-in divided by 750” number, because replies and monitoring are separate workloads. For standalone publishing alone:

- direct X: **$0.015/post**;
- Buffer Free: **$0/post in scheduler fees**;
- Buffer Essentials at 750 posts: **$0.008/post** on monthly $6 billing;
- Typefully Creator: **$0.011/post economic equivalent**, although the real invoice is $99 upfront annually;
- Post for Me: **$0.0133/post**;
- Postiz/Blotato/OpenTweet Advanced: **$0.0387/post** at $29/month.

Buffer Free is therefore the only candidate I found that clearly beats the official $0.015 write cost **without introducing an obviously unofficial X transport**.

### Monitoring sensitivity

For the recommended Buffer + X hybrid:

`monthly = $12.05 + $7.50p`

where $12.05 is replies + mentions + own-post metrics + profile snapshot.

Thus:

| Average foreign posts/account/day | Foreign monitoring | Buffer Free + X total |
|---:|---:|---:|
| 0.5 | $3.75 | **$15.80** |
| 1 | $7.50 | **$19.55** |
| 2 | $15.00 | **$27.05** |
| 4 | $30.00 | **$42.05** |

The four-hour polling interval itself does not imply 9,000 × $0.005 billing; what matters is the number of billable returned resources and the 24-hour dedup behavior. Use `since_id`/last-seen IDs and never repeatedly request large timeline windows. Pricing/dedup source: https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16. citeturn217039view2

## Recommended architecture

I would deploy this:

```text
Claude Code
   │
   ├── outbound publisher
   │     └── Buffer GraphQL API
   │           └── X via Buffer
   │
   ├── occasional/manual agent control
   │     └── Buffer MCP
   │         https://mcp.buffer.com/mcp
   │
   └── X intelligence / engagement worker
         └── official X app
             ├── X REST/XDK or xurl
             └── official X MCP
                 https://api.x.com/mcp
                 optionally ?read_only=true for monitor agent
```

For Buffer Free, do **not** upload your 418-post backlog into Buffer at once. Free has a small scheduled queue, while your own source queue is effectively unlimited. Keep the authoritative queue in SQLite/Postgres/JSON, and maintain perhaps 5–10 future posts in Buffer. Buffer’s current Free plan has three channel connections, while its public API/MCP is available on Free. citeturn349136view0turn349136view1  
Sources checked 2026-11-16:  
https://buffer.com/api  
https://support.buffer.com/en-us/articles/buffer-pricing-and-features-6pJrOPuzIt

Also prefer one deterministic Buffer API mutation per post rather than letting an LLM repeatedly browse queue state through MCP. Free’s 1,000-request monthly/channel allowance leaves only ~250 calls after 750 creates. Buffer documents MCP and API as sharing the quota. citeturn349136view0turn349136view2  
Sources:  
https://buffer.com/api  
https://developers.buffer.com/guides/integrations/mcp.html  
checked 2026-11-16.

For images, one practical limitation is confirmed by Buffer’s roadmap: API-attached image assets currently need to be **already hosted at a URL**; direct binary upload is still an explored feature. That means Claude Code should first put the image into S3/R2/a public object endpoint, then hand that URL to Buffer. citeturn349136view3  
Source: https://developers.buffer.com/roadmap.html — updated 2026-11-15.

If 1,000 Buffer requests becomes tight, **Essentials at $6/month for one channel** is the sensible upgrade because it raises the API allowance to 3,000 requests/channel/month and removes the operational pressure. citeturn349136view1turn349136view0  
Sources checked 2026-11-16:  
https://support.buffer.com/en-us/articles/buffer-pricing-and-features-6pJrOPuzIt  
https://buffer.com/api

The reply worker should have two gates before posting:

```text
1. interaction_is_user_initiated == true
2. x_self_serve_summon_rule_satisfied == true
3. ai_reply_approval_from_x == true   # before unattended AI replies
```

The first and third conditions come from X’s April 2026 Automation Rules; the second comes from the 2026-02-23 API changelog. citeturn106470view1turn850382view0

For the 50-account monitor, I would run the official X MCP in **read-only mode** or use REST with a separate read-only token/worker, then emit “reply candidate” objects into a review queue. Do not let that monitor possess write credentials. X added MCP `read_only=true` on 2026-11-10. citeturn217039view4  
Source: https://docs.x.com/changelog — 2026-11-10.

## Free follower snapshot: what actually survives scrutiny

I found the old unauthenticated X/Twitter syndication endpoint:

`https://cdn.syndication.twimg.com/widgets/followbutton/info.json?screen_names=Tony_Stef_`

and a current GitHub source still contains code using that endpoint to retrieve `followers_count`:

https://github.com/adafruit/Adafruit_Learning_System_Guides/blob/main/PyPortal/PyPortal_TwitterFollowers/code.py — repository page crawled 2026. citeturn359425search5

However, when I attempted to open the endpoint itself for `Tony_Stef_` during this research, the web fetch **failed**. I therefore cannot honestly classify that endpoint as “confirmed working on 2026-11-16.” It is undocumented and historically has broken before. I would not build the production snapshot on it.

The practical answer is therefore:

> **I did not verify a reliable, documented, genuinely free X follower-count endpoint in November 2026.**

The robust official solution costs only approximately:

`30 days × $0.010 User Read = $0.30/month`

using the official X API profile/user read. Source for User Read pricing: https://docs.x.com/x-api/getting-started/pricing — checked 2026-11-16. citeturn217039view2

If you still want a zero-dollar best-effort secondary collector, you can probe the syndication endpoint once daily with plain HTTP—this is **not browser automation**—but treat it as an opportunistic fallback, not production infrastructure. Do not add cookies or simulate the X website when it fails; X explicitly prohibits non-API website automation. citeturn106470view1  
Source: https://help.x.com/en/rules-and-policies/x-automation — updated April 2026.

Given the size of your whole system, spending **$0.30/month** for a stable official daily follower snapshot is preferable to engineering around an unsupported endpoint.

## Risks, contradictions, and checks before paying

The largest risk is **not “X notices 25 scheduled posts/day.”** X’s rules explicitly permit automated informational posts as long as they are not spammy, duplicative or otherwise prohibited. The serious risk is the reply worker: unsolicited automated replies are prohibited, followers alone are not deemed opted in, and an AI-generated automated reply bot requires prior written X approval. citeturn106470view1  
Source: https://help.x.com/en/rules-and-policies/x-automation — updated April 2026.

There is also a vendor-level compliance issue. X’s Developer Agreement, last updated **2026-04-27**, prohibits certain service-bureau/rental/managed-service API arrangements without appropriate authorization. Ayrshare explicitly says this is why it moved to BYO X applications and attributes the effective shift to **2026-03-31**. I verified the service-bureau restriction in X’s agreement, but I did not find an X-owned page that independently states the exact March 31 migration date. Treat the date as **Ayrshare’s assertion**, not independently confirmed X fact. citeturn742697view0turn349136view8  
Sources:  
https://docs.x.com/developer-terms/agreement — last updated 2026-04-27  
https://www.ayrshare.com/blog/x-api-compliance-byo-app/ — 2026-05-15

This makes the following question mandatory for any service-side-X-app vendor such as Postiz Cloud, Blotato, Upload-Post, Post for Me or OpenTweet:

> “For X publishing in November 2026, is my account authorized through your own X application under an X-approved Enterprise/partner arrangement that permits you to provide publishing as a service to customers, or are you relying on another mechanism? Please confirm that I do not need to supply my own X developer app and that this model complies with the current X Developer Agreement service-bureau restriction.”

Do not accept “we use the official API” as the answer. Ask **whose app, what authorization model, and whether it is explicitly permitted for multi-tenant customer publishing**.

For Buffer, I would send four precise questions before switching the production account:

1. “Does X publishing for Buffer customers in November 2026 run through Buffer’s own X-approved app/partner agreement, with no BYO X developer app required?”
2. “Does a GraphQL `createPost` call against one X channel count as exactly one request against the Free plan’s 1,000 requests/channel/30-day limit?”
3. “Can a Free-plan X channel publish approximately 25 posts/day if only 10 are queued simultaneously, or is there a separate daily X publishing cap not shown in the API quota documentation?”
4. “Is there any public or private Community API/MCP endpoint today for reading and replying to X comments, or should I assume that capability is UI-only until the roadmap item ships?”

The fourth answer should currently be “no public Community API,” because Buffer’s roadmap dated **2026-11-15** explicitly lists that API as under exploration. citeturn349136view3  
Source: https://developers.buffer.com/roadmap.html.

Then run one low-risk test sequence before moving the 418-item queue:

- publish one unique plain-text post through Buffer;
- publish one image post using a temporary hosted image URL;
- confirm the post is visible on X and returned by Buffer analytics;
- delete neither the image object nor URL until the post is confirmed published;
- inspect X → Settings → Security / Apps and Sessions to confirm the OAuth application identity;
- verify no charge appears in your own X developer balance for the Buffer-created post;
- wait through one scheduling cycle and verify timestamps and text exactly;
- only after that feed 5–10 queued posts.

For your own X reply rail, separately test one **genuine inbound mention/reply** and one deliberately non-summoned cold reply. The former should be the only workflow you productionize. The February changelog provides the relevant distinction. citeturn850382view0  
Source: https://docs.x.com/changelog — 2026-02-23 entry.

I would **not** perform the cold-reply test from the production @Tony_Stef_ account if it means intentionally violating the automation policy; you can validate endpoint behavior with a controlled test account/interaction instead.

Finally, do not use twitterapi.io or equivalent cookie/session-based unofficial write APIs merely because the sticker cost is attractive. twitterapi.io currently lists a write at **$0.0006**, more than 20× cheaper than X’s $0.015 official write, but that price advantage comes from leaving the official developer/API trust boundary. citeturn220145view8  
Source: https://twitterapi.io/pricing/details — live price data version 2026-10-12.

For a 22.8k-follower production identity, the expected-value trade is poor: a few dollars per month of savings are not worth session-cookie exposure, X enforcement risk, or dependence on reverse-engineered behavior.

**Final choice:** **Buffer Free + official X API/MCP**, approximately **$19.55/month ongoing at one foreign post/account/day**, versus **$30.80/month** for doing the entire modeled workload directly through X. Standalone outbound publishing drops from **$0.015/post to $0.00/post in platform fees**. If Free’s 1,000 API-request budget proves operationally tight, move only the publishing side to **Buffer Essentials at $6/month**, giving an expected hybrid total around **$25.55/month** while retaining the same low-risk official-X engagement layer. citeturn349136view0turn349136view1turn217039view2

The one thing I would resolve **before enabling unattended replies at all** is written X approval for the AI reply bot, because that requirement is explicit in X’s April 2026 Automation Rules and is materially more important than choosing between a $0, $6 or $29 scheduler. citeturn106470view1

## Sources (safe_urls/citations)
https://buffer.com/api
https://developers.buffer.com/guides/integrations/mcp.html
https://developers.buffer.com/roadmap.html
https://docs.mixpost.app/mcp/
https://docs.mixpost.app/services/social/x/
https://docs.socialdata.tools/getting-started/pricing/
https://docs.socialdata.tools/monitoring/pricing/
https://docs.x.com/changelog
https://docs.x.com/developer-terms/agreement
https://docs.x.com/tools/mcp
https://docs.x.com/x-api/getting-started/pricing
https://github.com/adafruit/Adafruit_Learning_System_Guides/blob/main/PyPortal/PyPortal_TwitterFollowers/code.py
https://help.x.com/en/rules-and-policies/x-automation
https://help.x.com/en/using-x/automated-account-labels
https://justpricing.com/typefully-pricing
https://mixpost.app/pricing
https://opentweet.io/help/pricing-plans
https://opentweet.io/twitter-mcp-server
https://postiz.com/mcp
https://postiz.com/pricing
https://support.buffer.com/articles/using-xtwitter-with-buffer-nA84XnWtuU
https://support.buffer.com/en-us/articles/buffer-pricing-and-features-6pJrOPuzIt
https://support.typefully.com/en/articles/13128440-typefully-mcp-server
https://support.typefully.com/en/articles/8718287-typefully-api
https://twitterapi.io/pricing/details
https://typefully.com/pricing
https://www.ayrshare.com/blog/x-api-compliance-byo-app/
https://www.ayrshare.com/pricing/
https://www.blotato.com/blog/how-to-post-to-x-with-claude-code
https://www.postel.app/typefully-pricing
https://www.postforme.dev/integrations/x
https://www.postforme.dev/pricing
https://www.upload-post.com/mcp/x/
https://www.upload-post.com/pricing/