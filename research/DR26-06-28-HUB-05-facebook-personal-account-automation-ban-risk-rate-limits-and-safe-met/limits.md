# DR26-06-28-HUB-05 — raw report from limits

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Methods Comparison

| Method                               | Ban Risk        | Reliability               | Complexity       | Suitable for (A/B/C)           |
|--------------------------------------|-----------------|---------------------------|------------------|-------------------------------|
| **Full-browser automation** (e.g. Playwright, Selenium, headful Chrome) | **Medium–High:** low-level control but detectable if not carefully masked. | High (can mimic user actions) | High (requires scripting, handles login state) | A, B, C (can post, comment, and send DMs via UI) |
| **Browser extension / user script** (runs in a real browser tab) | **Low–Medium:** actions originate in user session, but still automated.  | High (uses Facebook’s own UI) | Medium (must develop extension/JS)  | A, B (posts, replies); C is hard (no built-in quick send to new threads) |
| **In-session GraphQL API** (using `fb_dtsg` tokens + HTTP calls) | **High:** Facebook actively flags “non-human” GraphQL traffic. | High (bypasses UI, but endpoints may change) | Very High (reverse-engineer queries, handle tokens) | A, B, C (can call internal post/comment/messaging APIs) |
| **Official APIs** (Graph API / Business tools) | N/A (personal accounts have no such API) | N/A | N/A | – |
| **Manual/human operation** (no automation) | **None:** safest, controlled by real user timing. | N/A | N/A | A, B, C (all tasks, but no automation) |

- **Risk:** Facebook’s systems analyze device/IP fingerprints and patterns. Fully headless or identical bots are flagged. In-browser extension (running in a real user session) is least suspicious. In-session GraphQL calls often lack FB’s micro-analytics and are high-risk.  
- **Reliability:** UI automation and GraphQL can both reliably execute tasks. Extensions leverage official UI (less breakage from redesigns), whereas GraphQL calls depend on internal queries (can break if FB changes them).  
- **Complexity:** Writing robust GraphQL scripts is hardest (need to extract `fb_dtsg`, build queries). Browser automation (Playwright/Puppeteer) is easier but still needs handling login, dynamic content. Extensions require JS expertise but run inside stable DOM.  

**Redesign resistance:** Using the **GraphQL API** tends to survive UI changes (since it uses backend calls) but FB may change query IDs. UI-based methods (browser automation or extension) break if Facebook’s frontend changes drastically. The extension approach requires updating the DOM selectors. Full browser is most resilient if coded flexibly (e.g. clicking by aria-label).  

# Account Safety & Limits

- **Comments/Reactions:** Facebook imposes unstated caps on comments/likes per hour/day to deter bots. Anecdotally, users report that doing *multiple comments rapidly* (e.g. >3 in a row) triggers a temporary “slowdown” ban (~2 days). Spacing comments 4–5 minutes apart can avoid this, allowing dozens per day. As a guideline, keep replies to your own posts well under ~50/day and spread throughout the day. Avoid posting identical comments or likes repeatedly; identical/spammy content is a red flag.  

- **Direct Messages (DMs):** Personal profiles have very tight limits for messaging non-friends. FB’s own warnings mention a “message request limit” (new thread requests per day). In practice, even a handful (~5–10) of new contact messages per day can trigger a “message request limit” block. When hit, FB may prevent *all* new messages for 24–72 hours. (After the block lifts, sending more requests can immediately re-trigger it.) Replies within an existing conversation (especially with friends or recent commenters) are safer than *initiating* new threads to strangers.  

- **Own Posts vs Outreach:** Answering comments on your own posts (task B) carries lower risk because you’re engaging as the content owner. Outbound DMs to non-friends (task C) is highest risk, as FB treats new threads as potential spam. Always err on the side of response (reply on-wall) rather than unsolicited messaging when possible.  

- **Human-in-the-Loop:** Letting a person review each draft before send (e.g. creating a post draft or comment and requiring a click) does **not** eliminate risk, but it can slow down the process to human-like timing. However, Facebook detects underlying call signatures and timing patterns. A human click may add a tiny pause, but does not fundamentally fool FB’s analytics. The real benefit of “draft-first” is ensuring quality and avoiding obvious spam triggers.  

- **Environment (Fingerprint/IP):** For a *single* personal account on a home machine, use a consistent browser profile and IP. There’s no strong need for proxies or “antidetect” tools if only one account is used. In fact, unusual IP or frequent cookie resets can *increase* suspicion. Keep cookies and login state persistent; avoid deleting them or switching IPs often, as FB uses these signals for trust. Disable obvious headless flags if using automation (or use Playwright’s headful mode).  

# Data Extraction (Commenters → Messenger)

- **Fetching commenters:** The most reliable method is to simulate a logged-in user’s view or use Facebook’s internal GraphQL for comments. For example, open-source tools (e.g. the *fbpost* CLI) use the GraphQL query `ProdashCometCommentsManagerCommentsListPaginationQuery` to retrieve comments on recent posts. In practice, one can (via automation or GraphQL) scroll through the post, extract commenter names/IDs, and dedupe them. Be aware that comments on *shared copies* of a post (reposts) are not shown under the original post – those commenters are separate. For MVP, focus on comments under the original post.  

- **Linking to Messenger thread:** Each commenter has a user ID. To message them, open their Messenger thread (e.g. navigating to `facebook.com/messages/t/<userID>`). If you are **not friends**, the first message will go to their *Message Requests*. You can initiate a chat this way, but be aware it counts as a “message request.” The only way to automate sending that message is via UI or internal API calls – the public Graph API does *not* support personal-to-person messaging. In practice, an automated script (or extension) would: 1) look up the commenter’s ID, 2) navigate to their Messenger thread, and 3) send a prewritten message. (Tools like fbpost allow sending by user name or thread ID, implying this process is possible via GraphQL+browser automation.)  

# Maintaining Conversations in Messenger

- **Personalization:** Write genuinely personal replies. Reference the commenter’s name and the context of their comment. Vary the message content across users. Identical or templated messages to many people will be detected as spam. For example, instead of a batch message, customize each one (“Hi <Name>, thanks for your comment about XYZ…”).  

- **Frequency & Cadence:** Space out conversations. Don’t fire off dozens of new chats in a short span. After sending one DM, wait at least tens of minutes (or hours) before sending the next to a different person. Even within a single conversation, avoid sending rapid-fire multiple messages – mimic a human chat pace. 

- **Spam Indicators:** Facebook’s spam filter treats bulk messaging differently from normal replies. Red flags include: sending many messages with the same text, initiating threads to many non-friends rapidly, or sending unsolicited links/promotion. As a rule, keep each thread sparse (a few messages per day) and watch for any system warnings. 

- **Ending or Pausing Chat:** If a conversation is not reciprocated or the user does not engage, do not aggressively follow up. A single friendly follow-up is fine, but repeated unreturned messages can get your thread flagged. Respect user cues (if they stop replying, stop messaging). 

# Case Examples & Known Tools

- **Open-source tools:** A recent open-source CLI *“fbpost”* (Python) explicitly supports automated posting, comment replies, and Messenger messages via GraphQL + a real browser (Playwright). Its existence shows these tasks *can* be scripted, but it also implies the developer faced the complexity of GraphQL and UI scripting. Similarly, the `facebook-messenger-puppeteer` library exists to integrate with Messenger via a full Chromium instance. These projects highlight that custom solutions are possible but not trivial. 

- **Platform/official tools:**  All official Facebook automation tools (Business Suite, ManyChat, Composio) require a Facebook Page or business account, not a personal profile. They cannot be used for a personal profile’s Messenger or posting. This means popular chatbot tools (ManyChat, Chatfuel) and even Composio’s Facebook connector work *only* with Pages. The multilogin blog explicitly notes that legitimate automation for comments should use business-approved tools (e.g. ManyChat on a Page). 

- **Failed approaches:** Forum reports indicate that aggressive automation (even using LLMs) often leads to bans. For example, users automating marketing DMs from a personal account found their message privileges blocked after just a few requests. Another report showed a user hit a comments limit after many rapid replies (needing a 72-hour break). The takeaway is that **any** non-human pattern tends to trigger throttles or bans. 

- **Human + AI workflows:** Given the risks, most recommend a “human-in-the-loop” workflow: use AI (Claude/ChatGPT) to *draft* replies or posts, then have a person review and send them. Fully headless posting without review is very risky. One practical solution is to use a browser extension or user script to inject AI-generated text into the Facebook UI, so that a human still clicks “Send.” This hybrid approach maintains natural timing and oversight. 

# Recommendations

- **Draft-First vs Full Automation:**  For **Posts (A)**, automated scheduling or direct posting is feasible if you trust the content (e.g. an AI-written post you vetted). It’s moderately safe to automate posting prepared content via the browser (but still keep volume moderate). For **Comment replies (B)**, you *could* automate response templates since these happen on your own posts, but a final human glance is wise. For **DMs (C)**, treat it like email: always draft and check each message yourself. The risk of messaging strangers is high, so do not fire off DMs completely autonomously. In practice: *AI writes it, you review and hit send.*  

- **Safe Activity Limits:** No official “hard limits” are published, but community experience suggests **keeping all actions in the low dozens per day**. As a rule of thumb:  
  - **Posts:** Stay under ~5–10 personal-wall posts per day.  
  - **Comments/Replies:** Aim for <50 comments/likes per day (spread out). Avoid comment bursts – e.g. no more than 2–3 replies in a row without a pause.  
  - **DMs:** Keep new message requests to *single-digit counts* per day. In practice, even ~5–10 new chats can trigger a block. After hitting the block, FB enforces a 24–72h cooldown.  
  These are guidelines; the safe threshold depends on your account history and content. The key is moderation.  

- **Red Flags (to avoid):**  
  - **Rapid-fire activity:** posting/commenting multiple times in quick succession.  
  - **Identical/spammy text:** copying the same comment or message repeatedly.  
  - **Excessive DMs to strangers:** approaching many non-friends in a day.  
  - **Multiple accounts on one device/IP:** logging into different Facebook accounts from the same browser or VPN can link them and trigger bans.  
  - **Obvious bot behavior:** headless browser fingerprints, missing web “heartbeat” events, or impossible scrolling patterns. Always simulate real browsing (mouse movement, realistic delays).  

- **Minimal Safe Stack:** For one account on a home machine, a simple setup suffices:  
  - **Browser:** Use your normal Chrome/Firefox with a dedicated profile for FB, kept logged in.  
  - **Automation Tool:** Python + Playwright or Node + Puppeteer in headful (non-headless) mode. Ensure a real UA string and disable any “automated” flags. Optionally use stealth plugins if needed.  
  - **Data Access:** Use the same browser session’s cookies (`fb_dtsg` token, etc.) if making GraphQL calls. Libraries like the [fbpost CLI](#) handle this, or you can script it yourself.  
  - **Network:** No proxy needed. Operate from your usual home IP so that activity appears consistent.  
  - **AI Integration:** For generating text, you could embed an LLM call in your script or extension. For example, a Chrome extension could call Claude/OpenAI APIs to draft replies and inject them into the page fields (this ensures a person still clicks “Send”).  
  - **Scheduling:** Use OS scheduling (cron/Task Scheduler) to trigger actions at human-like intervals, rather than a tight loop.  
  - **Monitoring:** Log all automated actions and watch for Facebook warnings. If any throttle limit is reached, pause automation immediately.  

**Sources:** Facebook does not publish personal-account limits, so we rely on recent analyses and user reports.  For example, one summary notes built-in throttles on likes/comments and enforcement of unusual patterns. Technical explorations (fbpost CLI, Puppeteer libraries) demonstrate that GraphQL and browser methods can perform A/B/C. And numerous user accounts confirm that rapid messaging or commenting triggers blocks (e.g. blocks appear after a few DMs or comment bursts). Use this data to guide a cautious, slow rollout.