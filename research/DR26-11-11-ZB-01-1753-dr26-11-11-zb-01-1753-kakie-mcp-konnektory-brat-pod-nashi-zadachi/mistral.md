# DR26-11-11-ZB-01-1753 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chat.mistral.ai Vibe Work, Think + deep-research
chat: https://chat.mistral.ai/work/5aeb1280-68af-4231-973a-ac313f897329
dr_id: DR26-11-11-ZB-01-1753

---

MISTRAL_REPORT_START
DR26-11-11-ZB-01-1753 — MCP Connector Audit for a One-Person AI Lab (Nov 2026)
Question & scope

Which MCP connectors, per demand cluster from your 9,125-session telemetry (27 May – 11 Nov 2026), are worth adding to an always-on, remote/HTTP-only MCP config; which to reject; and the three special deep-dives (X posting, catalogs, GitHub MCP vs gh). Your hard constraint (stdio only ad hoc) is applied to every row.

Methodology

~50 search queries and ~25 full-page opens on 11 Nov 2026, biased toward 2026 sources and vendor-run primary docs (changelogs, official MCP docs, GitHub repos). Limitations: star counts and install counts not systematically verified (marked "unknown"); SaaS pricing changes fast; some third-party blogs (posteverywhere.ai, redditapis.com, bigideasdb) are vendor-operated and self-interested, noted where used.

1. Master decision table
Connector (cluster)
Your frequency
Verdict
Why (one line)
Transport
Cost
Confidence
Source
GitHub remote MCP (GitHub)
gh CLI 2,349 calls / 305 sess
LATER (selective)
Measured 4–32× token overhead vs gh, 72% vs 100% reliability; keep CLI primary, use MCP only for tools gh lacks
Remote HTTP + OAuth, per-toolset URLs, /readonly variants
Free
High
 GitHub remote-server docs,  Scalekit benchmark
OpenRouter MCP (LLM vendors)
openrouter.ai via shell, 10 sess
TAKE
Live model/pricing/credit data + test chats from any agent; replaces scraping the site
Remote mcp.openrouter.ai/mcp, OAuth (key auto-expires 7d, $10 cap)
Free (pay per test-message tokens)
High
 OpenRouter MCP docs
Vendor chat MCPs (GLM/Mistral/Grok/Gemini/ChatGPT web) (LLM vendors)
222 browser sessions
DON'T
No vendor exposes its chat/deep-research fan-out as MCP; fan-out stays on subscriptions or scripted APIs; browser automation is the only "connector"
n/a
—
Medium
 ChatForest review of Mistral,  Grok connectors docs,  z.ai Vision MCP
X MCP (api.x.com/mcp) (X/Twitter)
x.com 45 sess browser
LATER (read-only)
Strong reads (archive search, trends); posting not in the capability table, API posting is pay-per-use, and OAuth needs a local xurl bridge process
Hosted Streamable HTTP behind local npx bridge
API pay-per-use: $0.015/post ($0.20 with link), $0.005/read
Medium
 docs.x.com/tools/mcp,  X API pricing 2026
Typefully MCP (X + social)
already connected, 0 calls
TAKE (use it)
Threads + media drafts to X/LinkedIn/Threads/Bluesky/Mastodon; drafts-first = lowest ban risk
Remote mcp.typefully.com/mcp, OAuth
Included in plans (limits unknown)
High
 Typefully MCP help
Buffer MCP (social schedulers)
already connected, 0 calls
TAKE (use it)
Queue/schedule posts across networks from any machine via official integrations
Remote mcp.buffer.com/mcp, OAuth
Free on every plan, incl. Free
High
 Buffer MCP docs,  buffer.com/mcp
Metricool / Publer / Ayrshare / Hootsuite (social schedulers)
0
DON'T
Duplicates Buffer+Typefully; Publer needs Business tier, Ayrshare is a paid API with community stdio server, Hootsuite is enterprise-contract only
Metricool remote ai.metricool.com/mcp; Publer hosted URL; Ayrshare stdio
Varies (Metricool plan-gated; Ayrshare API paid)
High
 Metricool MCP FAQ,  Publer MCP,  Post-Bridge API comparison
Bluesky / Mastodon MCPs (fediverse)
bsky 5, mastodon 5
DON'T
Typefully covers posting to both; community servers are stdio npx jobs for 5-session volumes
stdio (community)
Free
High
 kud/mcp-bsky,  VitexSoftware/mastodon-mcp-server
Reddit MCPs (social)
reddit 6
DON'T
Public Data API closes in stages (new requests stop 31 Oct 2026, public access ends Mar 2027); logged-out .json already 403; scrapers are ToS-hostile
stdio community / Apify
API freemium is dying
High
 Reddit shutdown timeline,  redditapis.com
LinkedIn MCPs (LinkedIn)
linkedin 6 (account banned)
DON'T
User Agreement bars automated access; the top community servers drive a logged-in browser and their own docs warn of bans
stdio / local-browser
Free-ish
High
 usecarly LinkedIn MCP,  contentin ToS analysis
dev.to MCP (publishing)
dev.to 27
LATER (ad hoc)
Forem API is a plain API key; stdio server is fine for batch publish sessions, not always-on
stdio ( N-Graves/devto-mcp-server)
Free
Medium
 repo
Meta Ads MCP (Facebook/Meta)
facebook.com 15
DON'T (unless you run ads)
Meta's official server (Aug 2026) is ads-only; there is no sanctioned personal-Facebook posting API
Remote mcp.facebook.com/ads (per reports)
Free with ads account
Medium
 Meta Ads connectors,  contextbolt review
Scholarly bundle: OpenAlex official + cyanheads ORCID/Zenodo/DataCite
orcid 58, zenodo 21, doi 22, arxiv 25, crossref 7, datacite 8, openalex 6 (~150 total)
TAKE (self-host one HTTP daemon)
Replaces your biggest browser-driven cluster with typed tools; OpenAlex is vendor-official, cyanheads suite speaks Streamable HTTP
Self-hosted HTTP (+stdio), one process on the VPS
Free; optional tokens raise rate limits
High
 ourresearch/openalex-mcp-server,  cyanheads/orcid-mcp-server,  cyanheads/zenodo-mcp-server,  cyanheads/datacite-mcp-server
arXiv MCP (blazickjp)
arxiv 25
LATER (ad hoc)
LaTeX section reads + topic watches are genuinely useful, but stdio-only
stdio
Free
Medium
 blazickjp/arxiv-mcp-server
Semantic Scholar MCP
6
DON'T/LATER
OpenAlex + Exa/Tavily paper search covers it
stdio community
Free
Medium
 JackKuo666/semanticscholar-MCP-Server
Job boards: jobs-mcp-server (Greenhouse/Lever/Ashby/Workday)
ashby 16, greenhouse 10, lever 6, yc 9
LATER (ad hoc)
Official Ashby MCP and Wellfound Reach MCP are recruiter-side; this free multi-ATS server matches your job-seeker use
stdio community, no key
Free
Medium
 rezearcher/jobs-mcp-server,  Ashby MCP,  Wellfound Reach MCP
Hugging Face MCP
huggingface 18
TAKE
Vendor-official: Hub search, docs semantic search, papers, Spaces tools
Remote (HF settings page, OAuth/token)
Free
High
 HF MCP docs,  huggingface.co/mcp
Hetzner MCP
4+3
LATER
Community-only; tcpsystems build has -transport http if you ever want it; hcloud CLI is fine at this volume
stdio or self-host HTTP
Free (API token)
Medium
 dkruyt/mcp-hetzner,  tcpsystems/hetzner-mcp
Cloudflare MCP servers
cloudflare 3
LATER
Official remote suite (Workers, Radar, Observability, AI Gateway, Browser Rendering…) with OAuth; take per-project
Remote HTTP + OAuth
Free with CF account
High
 cloudflare/mcp-server-cloudflare,  Cloudflare blog
Tailscale MCP
tailscale 6
DON'T
No official server exists (verified Sept 2026); Aperture's MCP proxy is a different, larger product; CLI fine
none official
—
High
 yaw.sh analysis,  Tailscale Aperture GA
Docker MCP Gateway/Catalog
docker CLI 19
LATER
Gateway is a local process (can run as one daemon on the VPS); docker CLI fine at 19 sessions
Local gateway proxying remote OAuth servers
Free with Docker account
Medium
 Docker MCP Catalog,  Toolkit blog
Calendly MCP
calendly 13
TAKE
Official; availability, links, booking/cancel; works on free plan; useful for job-process scheduling
Remote HTTP + OAuth
Any plan incl. free
High
 Calendly MCP blog
Luma MCP
luma 5
TAKE
Official; event discovery + managing hosted events, guests, approvals
Remote mcp.luma.com
Free (no Plus needed)
High
 Luma MCP help
Partiful MCP
partiful 3
DON'T
No official server; the community one drives your Chrome session, noncommercial license
Self-host stdio/HTTP
Free
Medium
 cotletdev/partiful-api-mcp-server
Airtable MCP
airtable 3
LATER
Official remote on all plans, but your demand is 3 sessions
Remote mcp.airtable.com/mcp, OAuth
Included on all plans
High
 Airtable MCP overview
Notion MCP
no measured demand
LATER
Best-in-class remote MCP (semantic search, Markdown read/edit); take when you adopt Notion
Remote mcp.notion.com/mcp, OAuth
Plan gating unknown
High
 Notion MCP guide
Tavily MCP (web fetch/search)
jina 5 + heavy browser use
TAKE (pick one)
Search/extract/crawl/map/research tools, remote with keyless start; complements Claude-in-Chrome for fan-out
Remote mcp.tavily.com/mcp/, OAuth or key
Freemium; keyless Search/Extract within daily limits
High
 Tavily MCP docs
Exa MCP (web fetch/search)
—
TAKE (alternative)
Research-grade search incl. paper search with citations; hosted mcp.exa.ai/mcp
Remote, key/OAuth
Freemium credits (details unverified)
Medium
 Exa MCP,  review
Firecrawl MCP (web fetch/search)
—
LATER
Deep scraping/crawling when Chrome control is too slow; you already have jina + Chrome
Remote mcp.firecrawl.dev/v2/mcp(-oauth), OAuth or key
Freemium, keyless daily limits
High
 Firecrawl MCP docs
Brave Search MCP (web fetch/search)
—
DON'T
No vendor-hosted remote; self-host + API key; superseded by Tavily/Exa
Self-host (HTTP stateless mode or stdio)
Free API tier limited
Medium
 brave/brave-search-mcp-server
Jina MCP (web fetch/search)
jina 5
LATER
Official remote; grab if you want fetch/search in-agent, but Tavily covers it
Remote mcp.jina.ai
Freemium, rate-limited
Medium
 jina-ai/MCP
Playwright MCP (browser automation)
Chrome control 15.3k calls / 311 sess
DON'T always-on (ad hoc only)
stdio per session violates your fleet rule; Claude-in-Chrome already carries the load; repo very alive (commits Nov 2026)
stdio (+Chrome extension)
Free
High
 microsoft/playwright-mcp
Browserbase MCP (browser automation)
—
LATER
Cloud browsers with anti-bot if you ever need stealth at scale; paid
Remote + API key
Paid credits (tiers unknown)
Medium
 Browserbase MCP
Slack MCP (messaging)
no measured demand
LATER/optional
Official remote (announced 17 Feb 2026), admin-gated, user-token OAuth; take only if you start using Slack
Remote HTTP + OAuth
Free (plan gating unknown)
High
 Slack MCP docs,  Slack dev blog
Discord MCP (messaging)
—
DON'T
No official server; community stdio; bots cover it
stdio community
Free
Medium
 Enterpret guide
Signal MCP (messaging)
—
DON'T
Community stdio via signal-cli; you already run Telegram/WhatsApp HTTP daemons
stdio
Free
Medium
 rymurr Signal MCP
2. Top-10 to connect now, ranked by expected value
Finish OAuth and route X posting through Typefully MCP. Your single biggest browser-automation cluster after Chrome control is X (45 sessions), and Typefully is already connected but unused. Threads, media, scheduling, plus LinkedIn/Bluesky/Mastodon/Threads in the same drafts workflow ( help doc). Prior art: independent 2026 comparisons of social MCPs exist ( Pallyy,  PostEverywhere).
Buffer MCP for the multi-network queue. Already connected, free on every plan, OAuth, write tools land in your queue as drafts/scheduled posts, quota-aware with 429 handling ( API docs). Same prior art as above.
Self-hosted scholarly MCP daemon (OpenAlex official + cyanheads ORCID/Zenodo/DataCite, Streamable HTTP, one process on the Linux VPS). This is ~150 sessions of your measured demand (orcid.org is your #1 scholarly touchpoint) currently done via browser. OpenAlex is vendor-official ( repo); the cyanheads servers explicitly support "STDIO or Streamable HTTP" ( ORCID,  Zenodo,  DataCite). Prior art: the cyanheads servers are themselves published on Zenodo as research artifacts ( orcid-mcp paper); no independent benchmark vs raw API calls found, prior art not found (searched: "ORCID/OpenAlex MCP review/benchmark").
OpenRouter MCP. Free, tiny tool surface, replaces your shell scraping of openrouter.ai, and its per-connection key expires in 7 days with a $10 spend cap, which is a genuinely good security design ( docs). Prior art not found (searched: OpenRouter MCP independent review).
Hugging Face MCP. Vendor-official remote; Hub search, docs search, papers, Spaces-as-tools ( docs). Prior art not found beyond docs (searched: HF MCP review/benchmark).
Tavily (or Exa) MCP as your single sanctioned search/fetch backend. Remote, multi-client (explicit configs for Claude Code, Codex, Cursor, VS Code), keyless start ( Tavily docs;  Exa). This also partially de-risks your LLM-web-UI research fan-out by giving agents a legitimate retrieval path. Prior art: Exa has third-party reviews ( eSEOspace); no head-to-head Tavily-vs-Exa-vs-Firecrawl benchmark found, prior art not found.
Calendly MCP. 13 sessions, official, free plan OK ( blog). Prior art not found (searched: Calendly MCP review).
Luma MCP. 5 sessions but official, free, remote, and it manages hosted events, not just discovery ( help). Prior art not found.
Cloudflare remote MCP servers, per active project only. Official, OAuth, remote ( repo,  blog). Prior art not found (searched: Cloudflare MCP review).
GitHub remote MCP, read-only toolsets, on one machine (see Part C for why this is ranked last despite GitHub being your #2 integration overall).
3. Explicit DON'T list
What
Why not
LinkedIn MCPs (all)
ToS-prohibited automation; your account is already banned; community servers drive a logged-in browser and self-document ban risk ( usecarly,  contentin).
Reddit MCPs
Platform is shutting the public Data API (new requests stop 31 Oct 2026, closure completes Mar 2027; RSS ends 13 Nov 2026; logged-out .json returns 403) ( timeline,  redditapis).
X MCP as a posting path
Posting isn't in the capability table; new-developer API pricing is pay-per-use with a $0.20/link-post rate; OAuth requires a local xurl bridge process, which conflicts with your always-on remote-only rule ( docs.x.com,  pricing).
Standalone Bluesky/Mastodon MCPs
5 sessions each, stdio-only community builds; Typefully already posts to both ( kud/mcp-bsky,  VitexSoftware).
Playwright MCP always-on
stdio spawns per session (your measured 176-process problem); repo is excellent and alive but wrong transport for your rule; Claude-in-Chrome carries 15.3k calls already ( repo).
Docker MCP Toolkit as always-on on desktops
The gateway is a local proxy process; fine as a single daemon on the VPS later, but docker CLI at 19 sessions doesn't justify it ( docs).
Hetzner MCP
Community-only, low frequency; hcloud CLI is safe and free ( dkruyt).
Tailscale MCP
No official server exists as of Sept 2026 ( yaw.sh); Aperture is an enterprise MCP proxy, not what you need.
Partiful community MCP
Drives a personal Chrome session, PolyForm Noncommercial license, unofficial ( repo).
Ayrshare / Publer / Hootsuite / Metricool
Duplicate Buffer+Typefully; Ayrshare is a paid API behind a community stdio server ( vanman2024), Publer MCP needs Business tier ( help), Hootsuite API is enterprise-contract only ( Post-Bridge).
Brave Search MCP
No hosted remote; self-host + API key for something Tavily/Exa give you via OAuth.
Signal / Discord MCPs
No official servers; stdio community builds; your Telegram/WhatsApp HTTP daemons already cover messaging ( Enterpret,  PulseMCP Signal).
LLM-vendor chat MCPs
Nobody sells their own web chat as MCP; your browser fan-out has no connector replacement, only API scripts or each vendor's own agent products (inference, not measured).
A. Posting to X from any machine: Buffer vs Typefully vs X MCP
Typefully MCP is the best fit for your actual job: threads, multi-platform posts, image/media upload, queue viewing, all via OAuth at https://mcp.typefully.com/mcp, works with Claude, ChatGPT (developer mode), Claude Code, Cursor ( support doc). Content lands as drafts/queue items, keeping a human in the loop. Pricing: included; which features need paid plans is unknown, verify in-app.
Buffer MCP is the best fit for scheduled multi-network publishing: read channels, browse queue, schedule/edit posts, analytics; every write returns the saved record, and responses carry remaining quota with 429 Retry-After ( API docs). Free on every plan including Free ( buffer.com/mcp). Media support specifics: unknown from the docs I opened.
X MCP is the official X route: hosted Streamable HTTP at https://api.x.com/mcp, reached through the open-source xurl mcp bridge that owns your developer-app OAuth; capabilities listed are read-heavy (archive search, users, bookmarks, trends, Articles) and post creation does not appear in the capabilities table ( docs.x.com). API posting costs $0.015/post ($0.20 if it contains a link) and $0.005/read on the pay-per-use model that replaced tiers for new developers in Feb 2026 ( Postproxy).
Ban risk ranking (inference, medium confidence): posting through Buffer/Typefully's official channel integrations (sanctioned commercial API relationships, drafts-first) < your own developer app via X MCP/API (sanctioned but your app's reputation is yours alone) < browser automation on x.com (what you do today, highest risk). For reads (trends, search for paper-promotion timing), the X MCP read path is actually lower-risk than scraping.
Prior art: yes, 2026 comparisons exist ( Pallyy: 12 social MCP servers compared,  PostEverywhere comparison), though both are content-marketing blogs, not measurements.
B. The catalogs, and how to spot abandonware
claude.ai Connectors Directory: one catalog across claude.ai, Desktop, Mobile, Claude Code, and Cowork; verified + community labels (verification is not a security audit); usage-based ranking; notably, no DNS/.well-known domain-ownership proof is required (that applies only to the open MCP Registry) ( docs). A third-party tracker counts 8,046 connectors as of 30 Oct 2026 ( tracker).
Official MCP Registry: launched in preview 8 Sept 2025 at registry.modelcontextprotocol.io; self-reported by maintainers, namespace-based, with a public OpenAPI spec so marketplaces can build sub-registries, plus community flagging and denylisting for spam/malware/impersonation ( launch post,  about).
ChatGPT/OpenAI: plugins (2023) died; connectors were renamed apps on 17 Dec 2025 after OpenAI adopted MCP at DevDay Oct 2025; vendor apps ship in the apps directory, and custom remote MCP connectors sit behind Developer Mode in beta on Plus/Pro/Business/Enterprise/Edu ( usecarly guide). The Typefully doc confirms the same Developer Mode flow ( doc).
Cursor: official plugins live in the Cursor Marketplace; community MCP servers at cursor.directory ( Cursor docs,  cursor.directory/mcp/new).
Smithery: largest community registry; CLI installs into Claude Desktop/Code, Cursor, Cline, Windsurf, etc.; config values like tokens are not stored long-term ( WorkOS overview,  2026 guide).
Glama: ~119k servers, "all scanned and scored," positioned as a superset of the official registry with test-in-browser and gateway/hosting products ( glama.ai,  directory comparison). mcp.so: an aggregator with per-server config pages (see e.g.  Tavily); no curation guarantee.
Maintained vs abandonware: a 2026 audit of 1,847 public servers found ~52% abandoned ( Rapid Claw). Practical signals: (1) prefer vendor-operated remote servers, maintenance is structural; (2) check the server still tracks current MCP spec revisions (the spec moved through 2025-06-18 and 2026-07-28 revisions; GitHub's server advertises schemas only to clients on 2026-07-28+ ( changelog)); (3) release cadence vs 8+ month silence, which reliably breaks as clients update ( SkillsIndex,  dev.to guide,  maintained rankings); (4) registry presence + verified badges, remembering verification is not a security audit.
C. GitHub official remote MCP vs gh CLI
What it is: hosted at https://api.githubcopilot.com/mcp/, remote HTTP + OAuth, with each toolset exposed as its own URL and /readonly variants so you can mix-and-match; it also has remote-only tools like create_pull_request_with_copilot ( remote-server docs,  launch changelog). Actively developed: Projects tools + OAuth scope filtering (Jan 2026), secret scanning GA (May 2026), next-spec support (Jul 2026) ( changelog,  GA).
Token cost is the decisive measured fact: a March 2026 benchmark of 75 runs on identical read-only tasks found the MCP path (43 injected tool schemas) used 4–32× more tokens than raw gh (e.g., 44,026 vs 1,365 tokens for "what language is this repo"), and failed 7/25 runs on connect timeouts (72% reliability vs CLI's 100%) ( Scalekit). Similar conclusions elsewhere: tool definitions alone can reach 150k+ tokens in enterprise stacks ( Jannik Reinhard), and practitioner guidance converges on "gh for GitHub, MCP where OAuth/structured writes matter" ( Firecrawl blog,  vensas,  Substack comparison).
Verdict for you: high. Your gh CLI does 2,349 calls across 305 sessions reliably and free. Don't put the default GitHub toolset in the always-on config on all 8 machines. If you take it at all, add the /readonly issues+pull-requests toolset URLs on one machine, and reach for the remote MCP only for things gh does poorly (Copilot coding agent invocation, secret scanning queries, cross-org Projects work). Prior art: strong and directly on point (Scalekit, above).
Security notes (cross-cutting)
Prefer /readonly variants and scoped keys. GitHub's read-only toolset URLs ( docs) and OpenRouter's 7-day, $10-capped per-connection keys ( docs) are the pattern to demand from every connector; Slack gates everything behind admin approval ( docs).
Prompt injection is real even for official servers: injection bugs were found in an official Anthropic Git MCP server ( Infosecurity Magazine), and Anthropic itself advises only running MCP servers you trust plus per-server permission configuration ( Claude Code security docs). Treat every social/search connector's returned content as untrusted input; your fleet's HTTP-daemon rule already removes the worst vector (stdio servers sharing your user environment).
Write exposure: Buffer/Typefully writes land in queues/drafts by design; Luma and Airtable expose real event/record writes; keep human approval on for all MCP write tools (all three vendors note client-side approval flows).
Open questions
Does the X MCP surface post-creation tools (auto-generated from the OpenAPI spec) despite the capabilities table? Needs a hands-on xurl mcp session. Unknown.
Which Typefully/Metricool/Notion/Slack plan tiers gate MCP features. Unknown, not stated in the docs opened.
Tavily/Exa/Firecrawl free-tier volume limits for your fan-out loads. Not verified.
Whether Reddit's Devvit will ever serve off-platform agent reads (currently: explicitly no path) ( timeline).
Wellfound Reach MCP after the free period ends, and whether Ashby ever ships a candidate-side MCP ( blog). Unknown.
GitHub repo star counts for the community servers recommended: unknown, not verified in this pass.
Source notes
Source
Credibility
Last updated
 GitHub remote-server docs
5/5
-
 Scalekit MCP vs CLI benchmark
4/5
2026-03-11
 docs.x.com/tools/mcp
5/5
-
 Postproxy X API pricing 2026
3/5 (vendor blog, but figures internally consistent)
2026-02
 Buffer MCP API docs
5/5
-
 Typefully MCP help doc
5/5
2026-08-28
 OpenRouter MCP docs
5/5
-
 Hugging Face MCP docs
5/5
-
 ourresearch/openalex-mcp-server
4/5
-
 cyanheads ORCID MCP (and Zenodo/DataCite siblings)
3/5
-
 Reddit shutdown timeline (bigideasdb)
3/5 (vendor-operated scraper; dates cross-checked with  redditapis)
2026-10-27
 usecarly LinkedIn MCP +  contentin ToS
3/5
2026
 Meta Ads connectors announcement (blocked to bots; corroborated by  admakeai,  contextbolt)
3/5
2026-08
 Slack MCP docs +  dev blog
5/5
2026-02-17
 Calendly MCP blog
4/5
2026-10-07
 Luma MCP help
4/5
-
 Airtable MCP overview
5/5
-
 Notion MCP guide
5/5
-
 Tavily MCP docs /  Firecrawl MCP docs /  Exa MCP
5/5 / 5/5 / 4/5
-
 claude.com connectors directory docs
5/5
-
 MCP Registry launch post
5/5
2025-09-08
 usecarly ChatGPT connectors guide
3/5
2026-07-19
 Rapid Claw maintained-server audit
3/5
2026
 yaw.sh Tailscale MCP analysis
3/5
2026-09
 cloudflare/mcp-server-cloudflare +  Cloudflare blog
5/5
-
 microsoft/playwright-mcp
5/5
2026-11-11
 Docker MCP Catalog +  Toolkit blog
4/5
-

Bottom line: your already-connected-but-idle Buffer and Typefully are the two highest-value moves (finish OAuth, route X/multi-platform posting through them); then one self-hosted scholarly HTTP daemon covering ORCID/OpenAlex/Zenodo/DataCite kills your largest browser-driven cluster; OpenRouter, Hugging Face, Tavily-or-Exa, Calendly and Luma are cheap official remote wins; and GitHub's official MCP stays a selective tool rather than a fleet-wide default, because the measured token and reliability cost of its tool schemas exceeds what gh already delivers.

## Sources (hrefs)
https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md
https://www.scalekit.com/blog/mcp-vs-cli-use
https://openrouter.ai/docs/guides/overview/mcp-server
https://chatforest.com/reviews/mistral-mcp-server/
https://docs.x.ai/grok/connectors
https://docs.z.ai/devpack/mcp/vision-mcp-server
https://docs.x.com/tools/mcp
https://postproxy.dev/blog/x-api-pricing-2026/
https://support.typefully.com/en/articles/13128440-typefully-mcp-server
https://developers.buffer.com/guides/integrations/mcp.html
https://buffer.com/mcp
https://help.metricool.com/faqs-about-the-metricool-mcp-1i3w0
https://publer.com/help/en/article/how-to-set-up-and-use-publers-mcp-server-14orlq0/
https://www.post-bridge.com/best-social-scheduling-api
https://github.com/kud/mcp-bsky
https://github.com/VitexSoftware/mastodon-mcp-server
https://bigideasdb.com/reddit-api-shutdown-2027
https://www.redditapis.com/blogs/reddit-data-api-october-31-cutoff-2026
https://www.usecarly.com/blog/linkedin-mcp/
https://contentin.io/blog/linkedin-mcp-terms-of-service/
https://github.com/N-Graves/devto-mcp-server
https://www.facebook.com/business/news/meta-ads-ai-connectors
https://contextbolt.com/blog/meta-ads-mcp/
https://github.com/ourresearch/openalex-mcp-server
https://github.com/cyanheads/orcid-mcp-server
https://github.com/cyanheads/zenodo-mcp-server
https://github.com/cyanheads/datacite-mcp-server
https://github.com/blazickjp/arxiv-mcp-server
https://github.com/JackKuo666/semanticscholar-MCP-Server
https://github.com/rezearcher/jobs-mcp-server
https://www.ashbyhq.com/product-updates/mcp
https://wellfound.com/blog/wellfound-reach-mcp-ai-assistant
https://huggingface.co/docs/hub/agents-mcp
https://huggingface.co/mcp
https://github.com/dkruyt/mcp-hetzner
https://github.com/tcpsystems/hetzner-mcp
https://github.com/cloudflare/mcp-server-cloudflare
https://blog.cloudflare.com/thirteen-new-mcp-servers-from-cloudflare
https://yaw.sh/blog/tailscale-mcp-server/
https://tailscale.com/blog/aperture-ga
https://hub.docker.com/mcp
https://www.docker.com/blog/announcing-docker-mcp-catalog-and-toolkit-beta/
https://calendly.com/blog/mcp-server
https://help.luma.com/p/mcp
https://github.com/cotletdev/partiful-api-mcp-server
https://airtable.com/developers/agents/mcp/getting-started
https://developers.notion.com/guides/mcp/get-started-with-mcp
https://docs.tavily.com/documentation/mcp
https://exa.ai/mcp
https://eseospace.com/blog/exa-mcp-server/
https://docs.firecrawl.dev/mcp-server
https://github.com/brave/brave-search-mcp-server
https://github.com/jina-ai/MCP
https://github.com/microsoft/playwright-mcp
https://www.browserbase.com/mcp
https://docs.slack.dev/ai/slack-mcp-server/
https://slack.dev/secure-data-connectivity-for-the-modern-ai-era/
https://www.enterpret.com/guides/the-5-best-mcp-servers-for-discord-community-feedback
https://www.pulsemcp.com/servers/rymurr-signal
https://pallyy.com/blog/social-media-mcp-servers
https://posteverywhere.ai/blog/social-media-mcp-servers
https://zenodo.org/records/20024303
https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/
https://github.com/vanman2024/ayrshare-mcp
https://claude.com/docs/connectors/directory
https://github.com/rdmgator12/awesome-claude-connectors
https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/
https://modelcontextprotocol.io/registry/about
https://www.usecarly.com/blog/chatgpt-connectors/
https://cursor.com/docs/mcp
https://cursor.directory/mcp/new
https://workos.com/blog/smithery-ai
https://skiln.co/blog/smithery-mcp-guide-2026
https://glama.ai/
https://gingerlabs.ai/blog/find-evaluate-mcp-servers-smithery-glama-mcp-so
https://mcp.so/servers/tavily-mcp
https://rapidclaw.dev/blog/mcp-servers-dead-what-it-means-2026
https://github.blog/changelog/2026-07-23-github-mcp-server-supports-the-next-mcp-specification/
https://skillsindex.dev/blog/how-to-choose-the-right-mcp-server/
https://dev.to/wwqking/how-i-find-mcp-servers-that-are-actually-maintained-3omn
https://maketocreate.com/awesome-mcp-servers-65-ranked-by-whats-maintained-2026/
https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
https://github.blog/changelog/2026-01-28-github-mcp-server-new-projects-tools-oauth-scope-filtering-and-new-features/
https://github.blog/changelog/2026-05-05-secret-scanning-with-github-mcp-server-is-now-generally-available/
https://jannikreinhard.com/why-cli-tools-are-beating-mcp-for-ai-agents/
https://www.firecrawl.dev/blog/mcp-vs-cli
https://vensas.de/en/blog/mcp-vs-cli-cost-comparison
https://manveerc.substack.com/p/mcp-vs-cli-ai-agents
https://www.infosecurity-magazine.com/news/prompt-injection-anthropic/
https://docs.anthropic.com/en/docs/claude-code/security
https://admakeai.com/blog/meta-ads-updates-august-2026
MISTRAL_REPORT_END