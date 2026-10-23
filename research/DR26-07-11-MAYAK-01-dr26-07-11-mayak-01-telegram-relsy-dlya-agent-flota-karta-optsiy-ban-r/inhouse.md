# DR26-07-11-MAYAK-01 — raw report from inhouse

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-11-MAYAK-01 — Telegram connectivity for the agent fleet (IN-HOUSE deep-research, verbatim result)
# Run: workflow wf_66ef03b3-d54, 105 agents, 23 sources, 113 claims -> 25 verified (23 confirmed / 2 refuted), 2026-07-10 23:25-23:49 UTC, machine Маяк (Mayak), operator Anton

```json
{
  "question": "Programmatic Telegram connectivity options in 2026 for an autonomous AI agent fleet (headless Linux VPS node + Windows/Mac peers), which must both READ and WRITE messages in private groups and DMs. We already run two rails: (1) a Telethon-based local MCP server with a user-account StringSession, (2) a deterministic Telethon one-shot script (separate session, same user account). Research question: what are ALL the other viable connection methods beyond these two, and how do they compare? Cover at minimum: official Bot API (BotFather bots: long-polling vs webhooks, group privacy mode, what bots can/can't read), TDLib and tdlight (official client library, resource cost, bindings), other MTProto client libraries (Pyrogram fork status in 2025-2026, GramJS/Node, gotd/Go, grammers/Rust, MadelineProto/PHP) and their maintenance health, Telegram Business \"business connection\" feature for bots acting in a user's DMs (capabilities/limits 2025-2026), hosted/SaaS Telegram APIs and gateway services, bridges like matterbridge, web-view scraping of t.me/s public preview, and browser automation of Telegram Web as a last resort. For each: read/write capability matrix (groups, DMs, channels, history access), auth model and session storage security, reliability for 24/7 headless use, rate limits, and crucially the account-ban/flag risk for USER-account sessions (userbots) running from datacenter/VPS IPs (Hetzner) in 2025-2026 — Telegram's current enforcement reality, plus mitigation practices (login patterns, session reuse discipline, message tempo). End with a recommendation: for a fleet coordination chat (our own private group), which combination is safest and most robust, and whether adding a BotFather bot as a third rail is worth it (pros/cons vs the risks of userbot-only). Russian or English output both fine; cite sources.",
  "summary": "Verified evidence maps four additional connection families beyond the fleet's two Telethon user-session rails: (1) an official BotFather bot over the Bot API (long-polling or webhook, mutually exclusive) — ToS-sanctioned and ban-inert toward the user account, but group reading is gated by privacy mode/admin status, send tempo is capped (1/s per chat, 20/min per group), and there is no history access with updates lost after 24h offline; (2) MTProto client libraries with sharply divergent health — Kurigram is the actively maintained Pyrogram successor (users+bots, releases through May 2026), while Node's dominant GramJS has been dormant roughly 18 months; (3) the TDLib family — tdlight at version parity with upstream TDLib 1.8.65 plus server-oriented memory-reduction options, and tdlight-telegram-bot-api's experimental user mode that exposes a userbot through the Bot API HTTP surface on a headless server; and (4) Telegram Business connections (Bot API 7.2 through 10.1) letting a bot read and send inside a Premium user's DMs. On ban risk: Telegram's own docs state every unofficial-client login is automatically \"put under observation\", the Telethon maintainers date stricter enforcement to 2023 onwards, fresh session creation is itself a trigger event even on aged officially-created accounts, and the documented risk factors are phone-number provenance (VoIP/limited-country) and call tempo — datacenter/VPS IP appears in no primary risk list. Recommendation: for the fleet's own private coordination group, adding a BotFather bot (privacy mode disabled or added as admin) as a third rail is worth it — it decouples fleet plumbing from the user account so a userbot flag cannot sever coordination, and its limits (no history backfill, 20 msg/min per group) are immaterial for coordination traffic; keep the existing aged user-account sessions for history access and DM work under strict session-reuse discipline (persist StringSessions, never re-login fresh), moderate message tempo, and periodic @SpamBot checks — while noting that bot-to-bot visibility in a shared group failed verification and must be tested empirically before running one bot per node in a single chat.",
  "findings": [
    {
      "claim": "The official Bot API offers exactly two mutually exclusive update-delivery mechanisms — getUpdates long polling (pull) and setWebhook HTTPS push — and getUpdates is disabled (HTTP 409 Conflict) while a webhook is set, so a bot rail must commit to one mode at a time (switchable via deleteWebhook). No third mechanism exists even on self-hosted Bot API servers.",
      "confidence": "high",
      "sources": [
        "https://core.telegram.org/bots/api",
        "https://core.telegram.org/bots/webhooks"
      ],
      "evidence": "Live-verified 2026-07-10 against Bot API 10.1 docs: \"There are two mutually exclusive ways of receiving updates for your bot - the getUpdates method on one hand and webhooks on the other\"; webhooks guide: \"getUpdates is a pull mechanism, setwebhook is push.\" Enforcement corroborated by real 409 errors (\"can't use getUpdates method while webhook is active\") in activepieces #7267, opsdroid #853.",
      "vote": "3-0, 3-0 (claims 2+6 merged)"
    },
    {
      "claim": "A bot's read scope in groups is governed by the per-bot privacy-mode toggle (default ON): with privacy mode a bot receives only commands addressed to it, replies meant for it, messages sent via it, general commands if it posted last, and service messages — never ordinary group chatter. Disabling privacy mode (surfaced as can_read_all_group_messages in getMe) or adding the bot as group ADMIN gives it all messages; a toggle change takes effect only after re-adding the bot to each group. For the fleet coordination group, the bot must be made admin or have privacy mode disabled to read agent chatter.",
      "confidence": "high",
      "sources": [
        "https://core.telegram.org/bots/faq",
        "https://core.telegram.org/bots/api",
        "https://core.telegram.org/bots/features"
      ],
      "evidence": "FAQ verbatim on the restricted subset (\"Commands explicitly meant for them... Replies to any messages implicitly or explicitly meant for this bot\"); features page: \"Privacy mode is enabled by default for all bots, except bots that were added to a group as admins (bot admins always receive all messages)\"; API: \"can_read_all_group_messages... True, if privacy mode is disabled for the bot. Returned only in getMe.\" No 2025-2026 semantic changes found.",
      "vote": "3-0, 3-0 (claims 0+4 merged)"
    },
    {
      "claim": "Bot API write tempo is capped at roughly 1 message/second per individual chat, 20 messages/minute to the same group, and ~30 messages/second aggregate broadcast (extendable to 1000/s only via paid broadcasts at 0.1 Stars per message beyond the free 30/s); violations surface as HTTP 429 after tolerated short bursts. For a fleet coordination chat the binding constraint is the 20/min per-group cap on each bot.",
      "confidence": "high",
      "sources": [
        "https://core.telegram.org/bots/faq",
        "https://core.telegram.org/bots/api"
      ],
      "evidence": "FAQ verbatim: \"avoid sending more than one message per second... not able to send more than 20 messages per minute [per group]... not able to broadcast more than about 30 messages per second, unless they enable paid broadcasts\" (up to \"1000 messages per second\" at \"0.1 Stars per message\"). Independently confirmed by the allow_paid_broadcast parameter in the API reference; corroborated by gramio.dev/rate-limits and python-telegram-bot issue #4578.",
      "vote": "3-0"
    },
    {
      "claim": "Bot API bots have zero chat-history access: no history-fetch method exists anywhere in the API surface, unretrieved updates are stored on Telegram's servers at most 24 hours, and a bot offline longer than 24h permanently misses those messages. This contrasts with MTProto user sessions, which can pull full history (even a bot token over raw MTProto gets BOT_METHOD_INVALID on messages.getHistory). A bot rail therefore needs its own persistent local log and can never backfill.",
      "confidence": "high",
      "sources": [
        "https://core.telegram.org/bots/api",
        "https://github.com/tdlib/telegram-bot-api/issues/561",
        "https://github.com/LonamiWebs/Telethon/issues/1065"
      ],
      "evidence": "Verbatim from live docs (July 2026): \"Incoming updates are stored on the server until the bot receives them either way, but they will not be kept longer than 24 hours.\" Grep of full current docs shows no getHistory-type method; Telethon #1065 confirms BOT_METHOD_INVALID for bot tokens on messages.getHistory. Nuance: forwardMessage can re-materialize a message with a known message_id but cannot recover updates the bot never saw.",
      "vote": "3-0"
    },
    {
      "claim": "Telegram Business connections (introduced Bot API 7.2, March 2024; present through Bot API 10.1, June 2026) let a BotFather bot act inside a connected user's private DMs: Update carries business_connection / business_message / edited_business_message / deleted_business_messages, each such Message includes business_connection_id, and send methods accept business_connection_id to reply on the user's behalf. Scope limits: DMs only (not groups), the user must hold Telegram Business/Premium, the user chooses which private chats the bot sees, and BusinessBotRights/can_reply govern sending.",
      "confidence": "high",
      "sources": [
        "https://core.telegram.org/bots/api",
        "https://core.telegram.org/bots/api-changelog"
      ],
      "evidence": "Changelog Bot API 7.2 (2024-03-31): \"Added the parameter business_connection_id to the methods sendMessage, sendPhoto... allowing bots to send messages on behalf of a business account.\" Live 10.1 docs verified verbatim: \"business_connection_id... Unique identifier of the business connection from which the message was received.\"",
      "vote": "3-0"
    },
    {
      "claim": "Userbot (user-account) sessions are flagged by default, not only when misbehaving: Telegram's official API docs state that all accounts logging in via unofficial API clients are \"automatically put under observation\" as an anti-abuse measure; the Telethon maintainers confirm ban-proneness is generic to ANY third-party MTProto library (Telegram has learned spam patterns from widespread misuse), and they date a marked tightening of enforcement to 2023 onwards (\"anti-spam measures have gotten more aggressive\"). Both existing Telethon rails and any new MTProto rail operate under this heightened-scrutiny regime.",
      "confidence": "high",
      "sources": [
        "https://core.telegram.org/api/obtaining_api_id",
        "https://docs.telethon.dev/en/stable/quick-references/faq.html",
        "https://mtcute.dev/guide/intro/faq"
      ],
      "evidence": "Official docs live 2026-07-10: \"all accounts that log in using unofficial Telegram API clients are automatically put under observation to avoid violations of the Terms of Service.\" Telethon FAQ: \"this is not a problem exclusive to Telethon. Any third-party library is prone to cause the accounts to appear banned\"; \"More recently (year 2023 onwards), Telegram has started putting a lot more measures to prevent spam.\" Independently mirrored by gotd/td SUPPORT.md and MadelineProto docs; corroborated by Telethon issues #824, #3861, #3955.",
      "vote": "3-0, 3-0, 3-0 (claims 7+8+19 merged)"
    },
    {
      "claim": "Session lifecycle is the key controllable ban-risk variable: merely creating a new session (logging in with an unofficial client) can itself trigger the anti-abuse system even on an aged account created with an official app. The maintainers' mitigation playbook: run userbots only on well-established (never freshly created) accounts, persist and reuse one session per rail instead of re-logging in, avoid abuse-like tempo (many API requests in rapid succession), check account standing via @SpamBot, and treat PeerFloodError as the flagged/limited signal.",
      "confidence": "high",
      "sources": [
        "https://mtcute.dev/guide/intro/faq",
        "https://docs.telethon.dev/en/stable/quick-references/faq.html"
      ],
      "evidence": "mtcute FAQ verbatim: \"even logging in with an unofficial client to an account created using an official one may trigger the system.\" Telethon FAQ verbatim: \"use the library only on well-established accounts (and not an account you just created)\"; \"not abuse the API, like calling many requests really quickly\"; \"send a private message to @SpamBot\"; \"errors like PeerFloodError, which means you're limited.\" Corroborated by Telethon issues #3843/#3861 (bans within minutes of fresh session creation). Documented insta-bans skew heavily to fresh/VoIP-number accounts; for aged accounts new-session login is a low-but-nonzero risk event.",
      "vote": "3-0, 3-0 (claims 10+20 merged)"
    },
    {
      "claim": "The documented ban-risk factors are phone-number provenance and usage patterns — numbers from limited countries (Iran, Russia) and virtual/VoIP numbers face elevated risk because they are commonly used for spam, plus rapid API calls — while datacenter/VPS IP location (the Hetzner concern) appears nowhere in the Telethon maintainers' risk list: an exhaustive keyword check of the FAQ page found zero mentions of datacenter/VPS/hosting as a risk factor. There is no primary-source evidence that a Hetzner IP alone is a listed ban factor; only community threads speculate IP matters.",
      "confidence": "high",
      "sources": [
        "https://docs.telethon.dev/en/stable/quick-references/faq.html"
      ],
      "evidence": "Verbatim: \"limited country, such as Iran or Russia... Telegram is much more likely to ban these numbers, as they are often used to spam\" and \"Telegram may also ban virtual (VoIP) phone numbers, as again, they're likely to be used for spam.\" Verifier ran raw-HTML grep: 0 hits for datacenter/data center/VPS/hosting/Hetzner; proxy mentioned only for connectivity (Kazakhstan/China), never as ban factor. Note: absence from this page is not proof of real-world safety (see caveats).",
      "vote": "3-0"
    },
    {
      "claim": "The Pyrogram lineage has a maintained successor: Kurigram, a self-described \"actively maintained pyrogram fork... designed as a drop-in replacement\", shows real ~monthly release cadence into 2026 (v2.2.13 on 2025-10-10 through v2.2.23 on 2026-05-21, repo pushed 2026-07-08, PyPI in lockstep) and is a full MTProto framework \"for users and bots\" — one Python library covering both a userbot rail and a BotFather-bot rail (Client with bot_token vs phone sign-in, inherited from Pyrogram). Its README asserts its own maintenance but does not itself document upstream Pyrogram's abandonment.",
      "confidence": "high",
      "sources": [
        "https://github.com/KurimuzonAkuma/kurigram",
        "https://pypi.org/project/kurigram/"
      ],
      "evidence": "README verbatim: \"Kurigram is an actively maintained pyrogram fork for Python designed as a drop-in replacement for Pyrogram\"; tagline: \"Elegant, modern and asynchronous Telegram MTProto API framework in Python for users and bots.\" GitHub API verified every release date exactly (latest v2.2.23 published 2026-05-21T15:12:34Z, ~7 weeks before 2026-07-10; repo archived=false, pushed_at 2026-07-08, 5,215 commits); PyPI upload timestamps match. No abandonment/deprecation evidence found.",
      "vote": "3-0, 3-0, 3-0 (claims 11+12+13 merged)"
    },
    {
      "claim": "tdlight is a TDLib fork whose differentiator is lower RAM/CPU cost for long-running server-side clients, adding memory-reduction options stock TDLib lacks: disable_minithumbnails, disable_document_filenames, disable_notifications, disable_group_calls, ignore_inline_thumbnails. Strictly 4 of 5 are absent upstream — stock TDLib does have an ignore_inline_thumbnails option, but with disk-focused rather than RAM-focused semantics.",
      "confidence": "medium",
      "sources": [
        "https://github.com/tdlight-team/tdlight",
        "https://core.telegram.org/tdlib/options"
      ],
      "evidence": "README verbatim: \"This setting removes minithumbnails everywhere. It reduces memory usage because tdlib keeps them in RAM... Completely disables group calls support to save memory and CPU.\" Verifier cross-checked official TDLib options page and confirmed 4/5 options absent in stock TDLib. Marked medium per the 2-1 split vote (dissent over the ignore_inline_thumbnails naming overlap), though verifier evidence quality was high and the core thesis stands.",
      "vote": "2-1 (split)"
    },
    {
      "claim": "tdlight is actively maintained and tracks upstream TDLib closely as of mid-2026: last commit 2026-06-14 merges TDLib master, upstream version bumped to TDLib 1.8.65 on 2026-06-11, merge cadence is roughly monthly through 2026 (Apr 2, Apr 19, May 13, Jun 14), and upstream tdlib/td master itself declares version 1.8.65 — so tdlight is at version parity and choosing it does not mean falling behind official protocol updates. Practical consumers typically use tdlight-java bindings, which have their own release cadence and can lag a few weeks.",
      "confidence": "high",
      "sources": [
        "https://github.com/tdlight-team/tdlight",
        "https://github.com/tdlib/td"
      ],
      "evidence": "GitHub API live 2026-07-10: pushed_at=2026-06-14T17:55:34Z, head commit \"Merge remote-tracking branch 'td/master'\"; commit \"Update version to 1.8.65.\" (levlam, 2026-06-11) in merged history; upstream tdlib/td master CMakeLists.txt declares project(TDLib VERSION 1.8.65). Repo not archived; no unmaintained/deprecated reports found.",
      "vote": "3-0"
    },
    {
      "claim": "A hybrid rail exists between plain Bot API and raw MTProto: tdlight-telegram-bot-api, an actively maintained fork of the official Bot API server (fork of tdlib/telegram-bot-api, synced through Bot API 9.5 in Feb-Mar 2026), adds experimental USER-account support — enable with --allow-users (or TELEGRAM_ALLOW_USERS=1), authenticate over HTTP (POST /userlogin with phone_number returns user_token, then /user{token}/authcode, optional /user{token}/2fapassword), then drive the user account through standard Bot API methods at /user{token}/ paths on a headless server. Trade-offs: bot-only capabilities are lost (answerCallbackQuery, setMyCommands, sticker management, sendInvoice/payments, games) and \"It is also not possible to attach a reply_markup to any message.\" Being an unofficial-client login, it still falls under the userbot observation regime.",
      "confidence": "high",
      "sources": [
        "https://github.com/tdlight-team/tdlight-telegram-bot-api",
        "https://tourmaline.dev/usage/features/tdlight/"
      ],
      "evidence": "README verbatim: \"The TDLight Telegram Bot API is an actively enhanced fork of the original Bot API, featuring experimental user support\"; \"You can allow user accounts to access the bot api with the command-line option --allow-users\"; \"replace the /bot{bot_token}/ in your urls with /user{token}/\"; full bot-only-methods exclusion list and the reply_markup sentence verified in raw README. GitHub API: fork:true, parent tdlib/telegram-bot-api, archived:false, version 9.5 bump 2026-02-28, upstream merge 2026-03-02. Tourmaline docs independently corroborate the phone→user_token→code flow. Caveats: explicitly experimental, 2FA over HTTPS only, small community (165 stars), last push ~4 months before 2026-07-10.",
      "vote": "3-0, 3-0, 3-0 (claims 16+17+18 merged)"
    },
    {
      "claim": "GramJS (npm package \"telegram\") is the dominant Node.js-ecosystem MTProto option — a JavaScript/TypeScript client whose core is a port of Telethon, supporting full user-account (userbot) access and bot tokens, with ~253k weekly downloads (~29x mtcute's) — but it is dormant: last GitHub push 2024-12-26 (\"revert export changes.\", maintainer Painor), last tagged GitHub release v2.17.4 from 2023-05-14, last npm publish v2.26.22 on 2025-02-12, i.e. ~17-18 months of silence as of 2026-07-10. The repo is not archived, so it is dormant rather than officially abandoned — a significant maintenance-health red flag for adopting it as a NEW 24/7 fleet rail.",
      "confidence": "high",
      "sources": [
        "https://github.com/gram-js/gramjs",
        "https://www.npmjs.com/package/telegram"
      ],
      "evidence": "README verbatim: \"A Telegram client written in JavaScript for Node.js and browsers, with its core being based on Telethon\"; quickstart documents phone-number user login. GitHub API re-verified 2026-07-10: pushed_at=2024-12-26T18:00:21Z, archived=false, head commit 324820f by Painor, latest release v2.17.4 published 2023-05-14. npm registry: last modified 2025-02-12 (v2.26.22). api.npmjs.org week 2026-07-02..08: telegram=253,224 vs @mtcute/node=8,711 vs @mtproto/core=2,398 weekly downloads.",
      "vote": "3-0, 3-0 (claims 21+22 merged)"
    }
  ],
  "caveats": "Coverage gaps: no claims about gotd/Go, grammers/Rust, or MadelineProto/PHP maintenance health, hosted/SaaS Telegram gateway services, matterbridge, t.me/s public-preview scraping, or Telegram Web browser automation survived the 3-vote verification — those parts of the research question remain unanswered here (gotd and MadelineProto appear only incidentally, mirroring the \"under observation\" warning in their own docs). Two refuted claims are design-relevant: \"two Bot API bots in one group can never read each other's posts\" failed 1-2, so bot-to-bot visibility in a shared coordination group is unresolved and must be tested before running one bot per fleet node in a single chat; and mtcute's \"essentially nothing else in TS/JS\" characterization failed 0-3, implying actively maintained mtcute is itself a viable Node alternative to dormant GramJS. Ban-risk evidence rests on maintainer FAQs plus one sentence in Telegram's official docs; Telegram publishes no formal enforcement rules, \"under observation\" has no defined consequences, and the absence of datacenter-IP from the Telethon FAQ is absence-of-evidence, not proof Hetzner IPs are safe (community threads speculate IP matters, and real-world enforcement is opaque and shifts without notice). The tdlight memory-options finding carried the only split vote (2-1; strictly 4/5 options absent upstream). All version/maintenance facts (Bot API 10.1, Kurigram v2.2.23, tdlight 1.8.65 parity, GramJS dormancy, tdlight-bot-api at 9.5) are snapshots as of 2026-07-10 and decay quickly — verifier notes even disagreed on the current Bot API version label (9.6 vs 10.1), immaterial to the substance but a sign of doc churn. Telegram Business connections require ongoing Premium/Business on the connected account plus per-chat user consent, and cover DMs only — not the group-coordination use case. The tdlight-bot-api user mode is explicitly experimental and, being an unofficial-client login, inherits the full userbot observation/ban-risk regime despite its Bot-API-shaped surface.",
  "openQuestions": [
    "What is the 2025-2026 maintenance health and ban-risk posture of gotd (Go), grammers (Rust), and MadelineProto (PHP), and are any hosted/SaaS Telegram gateways or matterbridge configurations viable and safe as a fleet rail? None survived verification.",
    "Can two BotFather bots in the same private group see each other's messages under current 2026 rules? The classic 'bots cannot see other bots' assumption was refuted in verification (1-2), and the answer determines whether each fleet node can run its own bot identity in one shared coordination chat versus routing all bot traffic through a single token.",
    "Does a Hetzner/datacenter source IP measurably raise flag probability in practice for an aged, session-reusing user account? Official risk lists omit IP entirely, but community reports speculate it matters and no controlled evidence emerged either way.",
    "Do t.me/s public-preview scraping and Telegram Web automation still function in 2026, and what read-only fallback (if any) exists for private groups given t.me/s covers only public channels?"
  ],
  "refuted": [
    {
      "claim": "A BotFather bot with privacy mode disabled, or one made a group admin, receives all messages in the group EXCEPT messages sent by other bots — so two Bot API bots in the same fleet group can never read each other's posts.",
      "vote": "1-2",
      "source": "https://core.telegram.org/bots/faq"
    },
    {
      "claim": "The TypeScript/JavaScript MTProto client ecosystem is thin: per the mtcute maintainer, the only notable alternatives are GramJS (a port of Telethon) and @mtproto/core (extremely low-level, not TS-friendly), with essentially nothing else — relevant to assessing GramJS/Node as a rail option.",
      "vote": "0-3",
      "source": "https://mtcute.dev/guide/intro/faq"
    }
  ],
  "unverified": [],
  "sources": [
    {
      "url": "https://core.telegram.org/bots/faq",
      "quality": "primary",
      "angle": "official-bot-api-capabilities",
      "claimCount": 5
    },
    {
      "url": "https://core.telegram.org/bots/api",
      "quality": "primary",
      "angle": "official-bot-api-capabilities",
      "claimCount": 5
    },
    {
      "url": "https://core.telegram.org/bots/webhooks",
      "quality": "primary",
      "angle": "official-bot-api-capabilities",
      "claimCount": 5
    },
    {
      "url": "https://grammy.dev/guide/deployment-types",
      "quality": "secondary",
      "angle": "official-bot-api-capabilities",
      "claimCount": 5
    },
    {
      "url": "https://grokipedia.com/page/Telegram_Bot_API_Limitations",
      "quality": "blog",
      "angle": "official-bot-api-capabilities",
      "claimCount": 5
    },
    {
      "url": "https://www.teleme.io/articles/group_privacy_mode_of_telegram_bots?hl=en",
      "quality": "blog",
      "angle": "official-bot-api-capabilities",
      "claimCount": 4
    },
    {
      "url": "https://docs.telethon.dev/en/stable/quick-references/faq.html",
      "quality": "primary",
      "angle": "userbot-ban-enforcement-reality",
      "claimCount": 5
    },
    {
      "url": "https://tginfo.me/frozen-account-en/",
      "quality": "secondary",
      "angle": "userbot-ban-enforcement-reality",
      "claimCount": 5
    },
    {
      "url": "https://github.com/LonamiWebs/Telethon/issues/4150",
      "quality": "forum",
      "angle": "userbot-ban-enforcement-reality",
      "claimCount": 5
    },
    {
      "url": "https://www.kookeey.net/en/news/datacenter-proxies/2025-telegram-mass-messaging-anti-ban-guide",
      "quality": "blog",
      "angle": "userbot-ban-enforcement-reality",
      "claimCount": 4
    },
    {
      "url": "https://github.com/KurimuzonAkuma/kurigram",
      "quality": "primary",
      "angle": "mtproto-library-ecosystem-health",
      "claimCount": 5
    },
    {
      "url": "https://github.com/tdlight-team/tdlight",
      "quality": "primary",
      "angle": "mtproto-library-ecosystem-health",
      "claimCount": 5
    },
    {
      "url": "https://github.com/tdlight-team/tdlight-telegram-bot-api",
      "quality": "primary",
      "angle": "mtproto-library-ecosystem-health",
      "claimCount": 5
    },
    {
      "url": "https://mtcute.dev/guide/intro/faq",
      "quality": "primary",
      "angle": "mtproto-library-ecosystem-health",
      "claimCount": 5
    },
    {
      "url": "https://github.com/gram-js/gramjs",
      "quality": "primary",
      "angle": "mtproto-library-ecosystem-health",
      "claimCount": 5
    },
    {
      "url": "https://docs.telethon.dev/en/stable/concepts/botapi-vs-mtproto.html",
      "quality": "primary",
      "angle": "mtproto-library-ecosystem-health",
      "claimCount": 5
    },
    {
      "url": "https://core.telegram.org/api/bots/connected-business-bots",
      "quality": "primary",
      "angle": "business-connection-bots-in-dms",
      "claimCount": 5
    },
    {
      "url": "https://grammy.dev/advanced/business",
      "quality": "secondary",
      "angle": "business-connection-bots-in-dms",
      "claimCount": 5
    },
    {
      "url": "https://core.telegram.org/bots/api-changelog",
      "quality": "primary",
      "angle": "business-connection-bots-in-dms",
      "claimCount": 5
    },
    {
      "url": "https://github.com/42wim/matterbridge",
      "quality": "primary",
      "angle": "gateways-bridges-lastresort",
      "claimCount": 5
    },
    {
      "url": "https://github.com/mautrix/telegram",
      "quality": "primary",
      "angle": "gateways-bridges-lastresort",
      "claimCount": 5
    },
    {
      "url": "https://www.unipile.com/communication-api/messaging-api/telegram-api/",
      "quality": "primary",
      "angle": "gateways-bridges-lastresort",
      "claimCount": 5
    },
    {
      "url": "https://clura.ai/blog/telegram-scraper",
      "quality": "blog",
      "angle": "gateways-bridges-lastresort",
      "claimCount": 5
    }
  ],
  "stats": {
    "angles": 5,
    "sourcesFetched": 23,
    "claimsExtracted": 113,
    "claimsVerified": 25,
    "confirmed": 23,
    "killed": 2,
    "unverified": 0,
    "afterSynthesis": 13,
    "urlDupes": 0,
    "budgetDropped": 7,
    "agentCalls": 105
  }
}
```
