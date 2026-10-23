# DR26-06-27-HUB-03 — raw report from findings

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Executive Summary  
Facebook’s official Graph API **cannot** read comments on a personal profile’s own posts (only Pages or Business assets are supported).  Converting a profile to a Page would create a separate Page entity (allowing Graph API), but that means migrating content and operating on a new object.  Professional Mode does **not** magically turn the profile into an API-accessible page – it only enables creator insights.  In practice, robust solutions rely on either (1) **internal GraphQL calls** or (2) **full-browser automation**.  Both require a real logged-in session.  For example, one can use a browser script to call Facebook’s hidden `/api/graphql/` endpoint (supplying the current `fb_dtsg` token) to fetch comments, or use Playwright/Puppeteer to simulate user scrolling and clicking.  These methods must handle Facebook’s virtualized DOM and “View more comments” links, and avoid triggering anti-bot defenses.  Third-party scrapers (like Apify’s **Facebook Comments Scraper**) use their own infrastructure and proxies to gather **public** comments without your account, which is safest for the account but may miss friends-only content.  In summary: the *safest* approach for a personal profile is a controlled browser automation (e.g. Playwright or Airtop) under your account, carefully paced to avoid detection, or offloading to a professional scraping service if you only need public comments.  Below we elaborate on the findings, trade-offs, and a recommended implementation plan.

# Key Findings  
- **Graph API (Profile vs Page)**: In 2026 Facebook’s Graph API only supports Page content (and certain public data via the Meta *Content Library*).  Personal-profile posts and comments are *not* accessible to Graph API.  Professional Mode (creator profile) does **not** expose new Graph API endpoints – only Pages do.  Converting a profile to a Facebook Page (or creating a Page for yourself) could allow Graph API access to posts/comments, but this creates a separate object (and posts made on the original profile would not automatically appear there).  Thus for a “real” personal account, the official API is effectively closed.  

- **Internal GraphQL Endpoints**: Facebook’s web and mobile interfaces use hidden GraphQL APIs under the hood.  For example, developers have built scrapers that issue HTTP POSTs to `https://www.facebook.com/api/graphql/` with the user’s session cookies and `fb_dtsg` token to retrieve posts, reactions, and comments.  This can yield structured JSON of comments (author name, text, timestamp, likes, etc.) without rendering the full UI.  The challenge is that Facebook uses “document IDs” (`doc_id` or `query_hash` values) that change periodically.  In practice, one must capture the correct query ID (e.g. by inspecting the Network panel during normal browsing) and update it if Facebook rolls it.  **Example**: an Apify actor (“Facebook Review Scraper”) sends POSTs to `/api/graphql/` with no browser, fetching public reviews by ID – a similar approach can be used for comments.  Key requirements: you must supply valid auth cookies (`c_user`, `xs`), the current `fb_dtsg` token (found in the page source or HTML), and the right GraphQL parameters.  This method is very fast and cost-effective (no browser overhead), but somewhat brittle since Facebook can change GraphQL schemas or doc IDs, breaking scrapers.

- **Full-Browser Automation (Playwright/Puppeteer/Airtop)**: Running a real browser (or headless Chromium) under your logged-in account is highly robust to dynamic content.  It can execute JavaScript, handle infinite scroll, and click on “View more comments” buttons like a human.  For example, one can launch Playwright in Python and do: open the profile feed URL, scroll gradually, locate each `div[role="article"]` (post), and click “View more comments” repeatedly until all are loaded.  Playwright can then query `document.querySelectorAll('div[role="article"][aria-label^="Comment by "]')` to extract comment text and author.  The advantage is maximum reliability: it will work even if Facebook’s class names change (we rely on roles/aria-labels) and we avoid GraphQL maintenance.  The downside is complexity and resource usage: you must script the UI carefully (to avoid accidentally navigating away, e.g. by clicking the wrong link) and throttle your actions.  This method requires maintaining a real browser session (so cookies stay fresh) and simulating human-like timing.  

- **Open-Source Scrapers**: A number of community tools exist.  For instance, the Python library **`facebook-scraper`** (kevinzg) can fetch posts and, if given `options={"comments": True}`, can retrieve comments and replies.  FaustRen’s *facebook-graphql-scraper* uses a headless browser (via ChromeDriver) to query GraphQL and extracts post metadata (including comment counts).  These tools show it’s possible to get comments with code, but they are often fragile.  Indeed, one guide notes facebook-scraper has *~400 open issues* and DOM changes will break it.  In practice, these libraries may work for small public pages and with cookies, but they break frequently as Facebook changes layout.  

- **Commercial Scrapers / SaaS**: Services like Apify provide “Facebook Comments Scraper” actors that handle the heavy lifting.  Apify’s official Comments Scraper advertises: *“Unlike the limited Facebook API, this scraper gives you access to publicly visible comments in bulk”*.  It claims to extract comment text, author info, timestamps, likes, and even sort/filter comments, all via a web interface or API.  These tools typically run headless browser sessions with proxy rotation.  They are robust (multiple developers maintain them), but cost money (e.g. ~$1.40 per 1000 comments on Apify) and usually only handle public posts (since they don’t log into your personal account).  For a personal profile, you could use such a service on any *public* posts on your timeline, but would need a login-based scraper (or your own profile cookies) for non-public content.  

- **Anti-Bot and Rate Limiting**: Facebook aggressively monitors scraping patterns.  We found explicit warnings in scraper docs: *“If you scrape too much, Facebook might temporarily ban your IP”*.  Modern defenses (CAPTCHAs, login checkpoint screens, hidden token requirements, AI bot-detection) are common.  Best practice is to throttle requests (randomized intervals between scrolls or GraphQL calls), reuse the same session/profile, use high-quality residential proxies if making many requests, and even “hydrate” JSON instead of re-rendering.  For example, Coronium notes: *“Plan for: rate limits, mobile proxies, session rotation, and graceful degradation”* when scraping.  

- **Incremental Sync Architecture**: To avoid duplicates, one should track the latest fetched comment timestamp or ID per post.  On each run, only new comments (later timestamps) are added.  This can be implemented by storing state (e.g. last comment ID in a database or file).  Each extraction step should filter out known IDs.  Filtering by author can be done on the fly: when parsing comments, simply ignore any not by the target user.  

# Contradictions & Debates  
- *Graph API vs Scraping*: Some developers still hope for a “hidden” Graph API path for personal feeds, but official sources and blog audits make it clear that **only Pages (and related Business/Creator entities)** are supported in 2026.  Any solution pretending to be an “official API” for personal posts is actually just HTML/GraphQL scraping.  Facebook’s ToS forbids scraping, so third-party scrapers are always against terms (though data is publicly visible).  Official APIs (Graph API or Meta’s Content Library) are legal and stable but don’t apply to personal profiles.  

- *Professional Mode Confusion*: There is debate on whether enabling Professional Mode on a profile unlocks API access. The consensus is **no** – it is purely a front-end feature.  Some posts suggest insights or live video endpoints, but standard Graph API calls for comments still require a Page token.  Thus relying on Professional Mode is misleading; if API access is critical, one would have to use a Page or App.  

- *Tools and Maintenance*: Many open-source scrapers claim to do “everything” (posts, comments, reactions), but in 2026 most are outdated. For example, facebook-scraper’s last release was 2022 and its developers warn it often breaks. In practice, a headless browser approach is more stable long-term.  There’s debate on whether writing a custom scraper (code) or using an AI-driven tool (like Claude-in-Chrome) is more maintainable.  Our approach combines both: use AI/Claude to orchestrate the browser and parse results, while handling edge cases in code.  

- *Account Safety vs Completeness*: Using your own Facebook login (GraphQL or browser automation) means you can see non-public comments, but risks account action if Facebook detects unusual activity. Using a third-party (Apify) avoids touching your account (safer personally) but cannot fetch private/friends-only comments.  The trade-off depends on content privacy.  

# Best Practices  
- **Official APIs First**: If any Page or business handles your content, prefer Graph API (stable, quota’d, no UI churn). For a personal profile, skip to fallback methods.  

- **Use Authenticated Browser Context**: Whether using GraphQL or UI automation, run requests *within a real logged-in browser session* to reuse cookies and avoid login CAPTCHAs.  For GraphQL, you can inject JS via `window.fetch`; for UI, use Playwright with your profile (or Airtop in n8n) at headful mode for stealth.  

- **Token Handling**: Always retrieve the current `fb_dtsg` CSRF token before issuing GraphQL posts.  The token is usually found in a hidden input or in the HTML.  For example:  
  ```js
  let fb_dtsg = document.querySelector('[name="fb_dtsg"]').value;
  ```  
  Include it (and form data flags like `__a=1` or `__rev`) exactly as Facebook expects.  Validate after each run that tokens haven’t expired (reload the main page if needed).  

- **GraphQL Query IDs**: Identify the correct GraphQL `doc_id` for comments.  You can find it by monitoring a normal comment-load action in the browser’s Network tab (filter for `/api/graphql/`).  If results suddenly stop, re-capture `doc_id` and update your script.  Apify’s advice: *“filter for `graphql`, click the POST, look for `doc_id=XXXXXXXXX`”*.  In code, plan to make this doc_id configurable.  

- **Use Cursor Pagination**: Facebook returns comment lists with cursors. In GraphQL, handle `has_next_page` and `end_cursor` to loop through all comments.  In UI automation, you loop until no more “View more comments” buttons appear.  Set a sensible maximum or timeframe to stop (e.g. comments older than last run).  

- **Simulate Human Pace**: Introduce delays and randomness between actions.  For headless Chrome, set `wait_for` and `sleep` between scrolls/clicks.  For example, scroll a few posts then sleep 2–5 s.  This reduces the chance of triggering anti-bot flags.  Avoid continuous rapid queries – group GraphQL calls or browser interactions into batches.  

- **Proxy and IP Hygiene**: If doing large-scale scraping, use residential or mobile proxies. Facebook aggressively blocks datacenter IPs.  Route requests through a pool and rotate IPs to avoid rate limits.  However, for a single personal account on a trusted device, you may simply throttle requests and stay on one IP.  

- **Leverage Hydration JSON**: When using browser automation, consider parsing the page’s embedded JSON (hydration data) instead of DOM.  Facebook includes GraphQL results inside `<script type="application/json">` tags.  Extracting and parsing that JSON can be more reliable than brittle CSS selectors.  For example, find `window.__PRELOADED_STATE__` or similar in the page source to get comment data.  

- **Incremental Updates**: Store the ID or timestamp of the last-comment seen per post.  On each run, only request comments newer than that marker.  Both GraphQL and UI flows should compare and skip old comments.  This avoids duplication and minimizes data transfer.  (E.g. Facebook comment objects have unique IDs that can be compared.)  

- **Filter in Code**: After collecting comments, immediately filter to your author(s) of interest.  In practice, extract all comments then do `if comment.author_name == "Target Name"` (or compare author IDs).  This is simpler than trying to pass a filter to Facebook (not usually possible).  

- **Error Handling and Fallbacks**: Build retry logic.  If a GraphQL call fails (e.g. returns an error) or returns empty unexpectedly, fall back to a browser scroll for that post or vice versa.  Monitor for login pages or CAPTCHAs.  If blocked, pause and re-authenticate or switch proxies.  For example, if an Airtop step yields no posts, reload the profile URL.  

- **Use the Right Tools**:  
  - *n8n with Airtop*: This can orchestrate browser sessions via Puppeteer.  It’s useful to scroll and gather raw HTML or element text.  For example, an Airtop node can open the profile URL, scroll, and use a JS snippet (`document.querySelectorAll(...)`) to grab post IDs.  It can also open posts and extract comments.  
  - *Claude-in-Chrome/Claude Code*: Use it to write and inject the precise JS needed (e.g. GraphQL fetch scripts) or to parse JSON and format data.  Claude Code can run in the browser context (via CDP) to process data returned by Airtop.  
  - *Playwright/Python*: On the local GPU box, you can run headless Playwright scripts for more control and logging (useful for debugging complex flows).  E.g. use Playwright for incrementally scrolling the feed or loading comment threads in a headless Chrome, then send results to n8n.  

- **Stay Ethical and Legal**: Scraping Facebook violates its TOS and may infringe privacy laws if done improperly.  Always limit yourself to content you have rights to (e.g. your own posts) and anonymize or delete data as required by GDPR/CCPA.  For a personal knowledge base, this may not be a big issue, but be cautious about sharing scraped data.

# Failure Modes  
- **DOM/Selector Breakage**: Facebook’s UI classes are obfuscated and change frequently.  If you rely on CSS classes or text labels (e.g. “View more comments”), they may break.  Example: kevinzg’s scraper warns *“The vast majority of IDs on facebook… can be appended to facebook.com …”* but also notes that many fields can be `None`.  We saw that “comments” fields sometimes require login/cookies.  Always have a way to update selectors (e.g. switch to role/aria-based queries).  

- **Virtualization and Pagination**: The feed only loads elements in view.  If your script scrolls too fast or navigates away, some comments/posts may never render.  In automation, you must scroll the feed slowly enough and wait after each scroll to let React load content.  In a collapsed comment thread, only a subset of comments is in the DOM at a time; you must repeatedly click “View more comments” and wait.  If you miss one “More” click, deeper replies stay hidden.  

- **Accidental Navigation**: The “View more comments” link is an `<a>` tag.  Clicking it at the wrong time can navigate you away (to a standalone comments page or a photo).  In practice, one must either intercept link clicks (use `preventDefault`) or carefully target the button element via its container.  Failure to do this may send your browser out of sequence.  

- **Authentication Issues**: If your session expires or two-factor kicks in, scripts will start receiving login pages instead of data.  Always check that you’re still logged in (e.g. check for presence of the user menu DOM or the `c_user` cookie) before proceeding.  If not, stop the flow and re-authenticate manually.  

- **API Changes (GraphQL)**: Facebook might rotate GraphQL doc IDs or require new parameters.  When this happens, your GraphQL calls will silently return empty data.  The review scraper notes: “If the Actor suddenly returns no reviews, the `docId` may have changed” and needs updating.  Monitor for empty or partial results as a sign the internal API changed.  

- **Rate Limit & Blocks**: Too many requests too fast will get flagged.  We saw explicit advice: *“scrape too much, Facebook might temporarily ban your IP”*.  A block may manifest as an HTTP 429, a CAPTCHA, or the infamous “Action Blocked” page.  If blocked, back off (pause a few hours or switch to another IP) and resume carefully.  Remember to respect **graph.facebook.com** rate limits too (if using Graph API at all, e.g. with an app).  

- **Scraper Abandonment**: Many GitHub scrapers are no longer maintained.  For example, the `facebook-scraper` library has ceased active development (last release 2022, hundreds of open issues).  If you rely on an open-source tool, be prepared to patch it yourself or switch to a maintained alternative.  

- **CSS Obfuscation & Localization**: If Facebook changes language or interface text, string-based selectors (“View previous comments”, “Load more replies”) will fail.  It’s safer to use structural selectors (role, data-testid, or query by label start).  If all else fails, parse the HTML or JSON rather than relying on visible text.  

# Strategic Recommendations  
Based on reliability, maintenance, and account safety, we **rank** the approaches for a personal-profile comments use-case:  

1. **Full-Browser Automation (Playwright/Puppeteer)** – *Best reliability (but moderate account risk)*. This simulates a real user; it handles dynamic content, so it reliably expands all comments and works even when Facebook updates the UI. Set up your scraper to run in a normal (non-headless) Chrome with your logged-in profile and human-like delays. Use n8n’s Airtop node or Playwright scripts to scroll the feed and click every “View more comments” until exhaustion. Parse the DOM for comments (role/aria-based selectors). The downside is higher maintenance (Facebook UI changes require script updates) and it directly uses your account (so follow rate limiting vigilantly to avoid blocks).  

2. **Internal GraphQL Scraping with Authenticated Session** – *Fast and low-overhead, but brittle and some risk.* Here you leverage Claude Code or Python to send GraphQL POST requests from within the browser context (so you reuse the session cookies and `fb_dtsg`). You retrieve comments as JSON (faster than DOM parsing) and can filter by author directly in code. This method bypasses most UI churn (since you don’t parse HTML). However, it requires careful maintenance of query IDs and tokens. It still uses your account, so pace requests and honor pagination. If implemented well, it can be faster than clicking through the UI, but if Facebook changes their GraphQL schema or tokens, it breaks.  

3. **Third-Party Scraper (Apify or Similar)** – *Safest for your account, but limited to public content.* Services like Apify’s Facebook Comments Scraper use their own infrastructure and logins to fetch comments. You simply feed it your post URLs and it returns structured data (text, author, timestamp, likes). This requires no cookies or risk on your end. However, it only sees comments visible to the public (if your profile has private posts, it won’t capture those). The cost is per-comment or per-run (Apify is ~$1–5 per 1000 comments). Use this for any posts you know are public; it will relieve maintenance burdens and avoid using your account directly.  

4. **`facebook-scraper` Library (kevinzg) or Similar Lightweight Tools** – *Quick to try, but fragile.* This Python library can fetch comments from a public post if provided cookies. It’s easy to set up and returns JSON, but it relies on parsing Facebook’s (ever-changing) HTML/JSON responses. In 2026 it often breaks (the docs themselves note many issues). If you already have a session cookie jar, it can be a backup solution for quickly grabbing small numbers of comments. But for production use on a high-value account, it is riskier and less sustainable than a headless browser or managed service.  

5. **Convert Profile to Page + Graph API** – *Mostly theoretical for profiles.* If you turned your profile into a Facebook Page (the “Page” feature on profiles) then in principle you could use a Page Access Token to call Graph API endpoints for posts and comments. This would be the safest from an account-hygiene standpoint (official API, no DOM issues). **However**, this is heavy-handed: it means migrating your posting activity to a Page, which may not be acceptable if you want to maintain a normal friend-profile. Also, comments on the old profile would remain there. Only consider this if you are also open to using your profile as a creator Page.  

**Safety Note**: Methods #1 and #2 involve directly using your personal account to navigate or query. If done too aggressively, Facebook may flag the account (checkpoints, bans). Always simulate normal browsing (random waits, moderate frequency). Method #3 avoids that risk entirely but at the cost of only public data.  

# Action Plan (Claude Code + n8n)  

**1. Prepare Authentication:** Ensure your browser automation (Airtop/Playwright) is logged into your Facebook profile. Retrieve session cookies (`c_user`, `xs`) and confirm `fb_dtsg`. You can extract `fb_dtsg` by executing in the page context:  
```js
let fb_dtsg = document.querySelector('[name=fb_dtsg]').value;
```  
Store these tokens securely in n8n or as environment variables.  

**2. Identify New Posts:** In n8n, use an **Airtop (browser) node** to navigate to `https://www.facebook.com/me` (your profile). Scroll through the feed repeatedly (e.g. in a loop: `page.evaluate(() => window.scrollBy(0, 1000)); await new Promise(r=>setTimeout(r,2000));`) until you detect posts beyond your last-synced timestamp or ID. Extract identifiers for each post (for example, parse the `data-ft` attribute or timestamp link). Save new post IDs/URLs.  

**3. Expand Comments per Post:** For each new post ID:  
- *GraphQL approach:* Use a **Claude Code** or custom JS/Python step that issues `fetch('/api/graphql/', {method:'POST', ...})`. In the request payload, include the identified `doc_id` for comments, the `fb_dtsg` token, and variables (post id, pagination cursor, comments limit). Loop fetching until `page_info.has_next_page` is false. Append all returned comments into a JSON array.  
- *Browser approach:* Alternatively, in Airtop, open the post’s permalink (or click the post’s timestamp link to open it). Then in that context, repeatedly find and click the “View more comments” element (e.g. by `page.click('div[role=button]:has-text("More comments")')` or query by aria-label). Wait after each click for new comments to load. Continue until no more expanders.  

**4. Extract and Filter Comments:** Once comments are loaded (via GraphQL or DOM), extract needed fields: author name/ID, comment text, timestamp. If using Airtop DOM, you might `document.querySelectorAll('div[role="article"][aria-label^="Comment by "]')` and for each element read `innerText` or attribute parts. In Claude Code or Python, parse the JSON or HTML to get a list of comment objects. Immediately filter this list to keep only those by the target author(s).  

**5. Deduplicate & Store:** Compare each candidate comment’s unique ID or text+timestamp to your storage of already-seen comments (a database or a simple file of IDs). Discard duplicates. For new comments, format them as desired (e.g. Markdown note or JSON) and append to your Obsidian vault or CRM. If using n8n, you could write to a local file or use an API node to push into your knowledge base.  

**6. Implement Pacing and Monitoring:** In every step, insert delays and checks. For example, after extracting comments from one post, pause 5–10 seconds before the next to mimic human behavior. Monitor for errors (e.g. if a fetch returns empty or if the Airtop page shows a login prompt). If encountered, have logic to pause the workflow, refresh the session, or alert you. Optionally use mobile emulation or a random `User-Agent` from a list to reduce bot fingerprinting.  

**7. Schedule and Iterate:** Use n8n’s scheduling to run this workflow incrementally (e.g. once per hour/day). On each run, it should pick up only new posts or comments since the last run. Log actions (e.g. number of new comments found). If Facebook changes something (e.g. GraphQL doc_id), you will notice empty results and must update the code. Keep your setup modular so you can swap between the GraphQL vs DOM approach if one breaks.  

**8. Evaluate Third-Party Option:** As a parallel path, you could test Apify’s Facebook Comments Scraper for some public posts (especially for high-volume ones). Use its JSON output to cross-check your own results or capture any public comments you missed. This can serve as a safety net.  

By combining Claude-controlled browser automation with n8n orchestration, you get both flexibility (writing custom JS/GraphQL steps via Claude) and reliability (n8n handling scheduling, error catching, data flow). This hybrid approach leverages your existing stack fully. 

# Sources  
- Meta Graph API documentation and developer discussions (Graph API v25.0, user_posts)  
- Coronium “Facebook Scraper Python” guide (2026)  
- Apify Facebook Comments Scraper documentation  
- FaustRen *facebook-graphql-scraper* (GitHub) usage examples  
- `facebook-scraper` (kevinzg) README and examples  
- AlterLab blog “How to Scrape Facebook Data” (2026)  
- Apify Facebook Review Scraper info  
- Coronium guide on anti-bot and proxies.