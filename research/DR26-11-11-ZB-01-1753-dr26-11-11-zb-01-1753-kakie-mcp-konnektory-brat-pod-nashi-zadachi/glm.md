# DR26-11-11-ZB-01-1753 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chat.z.ai GLM-5.3 (Task UI, Effort Max, title shows GLM-5.3-Flash)
chat: https://приватный чат
dr_id: DR26-11-11-ZB-01-1753

---

DR26-11-11-ZB-01-1753 — MCP Connector Audit for a One-Person AI Lab (Nov 2026)

Method: ~91 web searches (5 per batch via built-in search + CLI fallback after the built-in web-tool cap hit) + 2 full-page fetches (Buffer MCP docs, OpenRouter MCP docs). Research date: 12 Nov 2026. Preference given to 2026 sources. Confidence: H = vendor doc/blog or fetched page; M = reputable third-party writeup or official-page snippet; L = forum/aggregator claim. "Unknown" is written where I could not verify. Your fleet rule is applied throughout: permanent config = remote HTTP/SSE only; stdio = ad hoc ("add, use, remove") or self-hosted behind a bridge.

1. Master table

Sorted by your measured demand. "Freq" = sessions (and calls where you gave them), 27 May – 11 Nov 2026.

CONNECTOR
	
CLUSTER
	
FREQ
	
VERDICT
	
WHY (ONE LINE)
	
TRANSPORT
	
COST
	
SOURCE
	
CONF.

GitHub official remote MCP (api.githubcopilot.com/mcp)	GitHub	2,349 gh calls/305 + 227 shell	TAKE	OAuth, toolsets + tool search cut ~55k→4.2k tokens; works in Claude/ChatGPT/Cursor where gh CLI can't reach	Hosted remote HTTP + OAuth (self-host Docker + stdio also possible)	Free, all GitHub users; inherits REST 5,000 req/h	https://github.com/github/github-mcp-server · https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview · https://www.scalekit.com/blog/github-mcp-server-per-user-oauth	H
OpenRouter official MCP (mcp.openrouter.ai/mcp)	LLM vendors / OpenRouter	openrouter.ai shell 10; UI fan-out 222 total	TAKE (catalog + test calls; keep API for real fan-out)	Live model/price/benchmark lookups + send-message cross-model tests; sandboxed 7-day key w/ $10 cap; replaces "openrouter.ai via shell"	Hosted remote HTTP + OAuth (PKCE)	Free to connect; inference billed per call to your key	https://openrouter.ai/docs/guides/overview/mcp-server	H (fetched)
X official hosted MCP (devcommunity, Jun 30 2026)	X/Twitter	x.com 26+19	TAKE (finish the pending OAuth)	First-party = lowest ban risk; but pay-per-use pricing and no scheduling — pair with Typefully	Hosted remote ("any MCP-compatible tool… with your own account's permissions")	MCP free; X API billing applies: $0.015/post, $0.20/post-with-link	https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558 · https://techcrunch.com/2026/06/30/x-now-offers-an-mcp-server-to-make-its-platform-easier-for-ai-tools-to-use · https://docs.x.com/x-api/getting-started/pricing	H/M
Typefully MCP (official)	Social scheduling	(posting arm of X cluster)	TAKE	Draft/schedule X threads + Bluesky/Mastodon/Threads cross-post from a free plan	Hosted remote (works with Claude, ChatGPT, Cursor, Windsurf, Notion)	Free plan; paid tiers on pricing page (price unverified — unknown)	https://support.typefully.com/en/articles/13128440-typefully-mcp-server · https://typefully.com/changelog	M
Buffer MCP (mcp.buffer.com/mcp)	Social scheduling	cross-posting hub	LATER	Cross-platform (14 networks) but X posts via MCP are text-only (no media) — weak for your primary network	Hosted remote HTTP + OAuth	Free plan: 3 channels, 10 scheduled posts/channel; paid ~$5–6/channel/mo; MCP on every plan incl. Free	https://developers.buffer.com/guides/integrations/mcp.html · https://pallyy.com/blog/social-media-mcp-servers · https://www.softwaresuggest.com/buffer/pricing	H (fetched) / M
Firecrawl MCP (mcp.firecrawl.dev/v2/mcp)	Web fetch/scrape	jina.ai 5, algolia 6, plus implicit fetch everywhere	TAKE	One hosted server for scrape/search/crawl/parse with a keyless free tier; its research index (arXiv, PubMed, bioRxiv) is free	Hosted remote: keyless (3 tools), OAuth or API-key (29 tools)	1,000 free credits/mo (≈1,000 pages), no card; pay-as-you-go after	https://docs.firecrawl.dev/mcp-server · https://github.com/firecrawl/firecrawl-mcp-server · https://www.firecrawl.dev/pricing	H
Tavily MCP (https://mcp.tavily.com/mcp/)	Web search	(search demand currently implicit in Chrome 15.3k calls)	TAKE	Hosted Streamable-HTTP search MCP with a keyless option; 1,000 free credits/mo	Hosted remote HTTP; keyless or API key in URL	Free 1,000 credits/mo, no card; pay-as-you-go after	https://docs.tavily.com/documentation/keyless · https://github.com/tavily-ai/tavily-mcp · https://mcp.so/servers/tavily-mcp	H
Exa MCP (https://mcp.exa.ai/mcp)	Web search (neural)	alt. to Tavily	TAKE as 2nd or swap	Open-source server + hosted endpoint; $10 free credits/mo recurring	Hosted remote HTTP (also self-hostable, open-source)	$10 free credits/mo + $10 onboarding; then usage-based	https://exa.ai/docs/get-started/exa-mcp · https://exa.ai/pricing · https://www.scalekit.com/blog/exa-mcp-vs-api	H/M
Brave Search MCP (official repo)	Web search	—	LATER (stdio-only)	Best-in-class index (ranked #1 for agent quality by AIMultiple) but no hosted endpoint — would need supergateway bridge	stdio only (npx, API key); no OAuth, no remote	Up to $20 in free API calls/mo, prepaid credits, no subscription	https://github.com/brave/brave-search-mcp-server · https://www.scalekit.com/blog/brave-search-mcp-vs-api · https://brave.com/search/api	H/M
Jina MCP (mcp.jina.ai)	Web fetch	jina.ai 5	DON'T (duplicate)	Firecrawl covers fetch/search; keep Jina as API fallback only	Hosted remote	Free tier w/ rate limits; free keys at jina.ai	https://github.com/jina-ai/MCP · https://jina.ai/reader	H
OpenAlex official MCP (https://mcp.openalex.org/mcp)	Scholarly	openalex 6 (cluster ~150 total)	TAKE	First-party MCP from OurResearch (nonprofit), OAuth 2.1, covers 270M+ works/authors/institutions — your scholarly backbone	Hosted remote Streamable HTTP + OAuth 2.1	Free	https://github.com/ourresearch/openalex-mcp-server · https://help.openalex.org/access/connector	H
Semantic Scholar MCP (community, e.g. smaniches/semantic-scholar-mcp, PyPI semantic-scholar-mcp)	Scholarly	s2 6	LATER (stdio; bridge or Kahubi)	14–33 tool community servers exist; no official one; S2 API is free with key	stdio (community)	Free (S2 API free tier)	https://github.com/smaniches/semantic-scholar-mcp · https://pypi.org/project/semantic-scholar-mcp	M
arXiv MCP (blazickjp/arxiv-mcp-server)	Scholarly	arxiv 25	LATER (ad hoc stdio)	Best-maintained arXiv server (3.3k stars, v0.8.2 Oct 8 2026); Firecrawl's research-index already covers arXiv search free	stdio (Python; Docker image mcp/arxiv-mcp-server)	Free	https://github.com/blazickjp/arxiv-mcp-server	H
ORCID / Crossref / Zenodo / DataCite MCPs (community: cyanheads/orcid-mcp-server, JackKuo666/crossref-MCP-Server, MSKazemi/mcp-zenodo, alexwade/datacite-mcp)	Scholarly	orcid 58, doi 22, crossref 7, datacite 8, zenodo 21	LATER (bundle via Kahubi or self-host)	No vendor-official remote servers; ORCID server supports Streamable HTTP; ORCID+Crossref+DataCite also available hosted on Kahubi (free plan)	Mixed: ORCID = stdio or Streamable HTTP; others stdio	Free (public APIs)	https://github.com/cyanheads/orcid-mcp-server · https://github.com/JackKuo666/crossref-MCP-Server · https://github.com/MSKazemi/mcp-zenodo · https://github.com/alexwade/datacite-mcp · https://kahubi.com/sources/orcid	M
Kahubi (hosted research MCP bundle)	Scholarly	aggregate	LATER / optional TAKE	One hosted provider running Crossref+ORCID+DataCite (+more) servers itself; free plan	Hosted remote	Free plan included; paid tiers unknown	https://kahubi.com/sources/orcid · https://kahubi.com/sources/crossref · https://kahubi.com/sources/datacite	M
dev.to remote MCP (nickytonline/dev-to-mcp)	Dev blogging	dev.to 27	TAKE (self-host 1 instance on your VPS)	Community-built but purpose-built remote server for the dev.to public API, no auth; one process on the VPS fits your fleet rule	Self-hosted remote HTTP (repo runs at /mcp)	Free (public API)	https://dev.to/nickytonline/introducing-the-devto-mcp-server-42jg · https://github.com/nickytonline/dev-to-mcp	H/M
Hashnode MCP (official)	Dev blogging	0 (you don't use it)	LATER (only if you start cross-posting)	Official vendor MCP ("one URL, no API token"), Sep 29 2026	Hosted remote	Free	https://hashnode.com/blog/hashnode-mcp-server	M
HackerNoon	Dev blogging	6	DON'T	No official or notable community MCP found; keep manual/browser submission	n/a	n/a	(searched "HackerNoon MCP API" — nothing exists)	M
LinkedIn (any MCP/automation)	Social	6 (account banned)	DON'T	ToS explicitly bans third-party software/bots/crawlers; your account is already banned; tools like ConnectSafely's "LinkedIn MCP" are ban-magnet grayware	n/a	n/a	https://www.linkedin.com/help/linkedin/answer/a1341387 · https://www.linkedin.com/pulse/ashbys-mcp-server-goes-live-ai-reshapes-hiring-teams-rethink-funnel-7kjwe (n/a)	H
Facebook/Meta	Social	15	DON'T (keep manual; no personal-profile API since 2018; Pages API needs App Review + Business Verification)	No official MCP; programmatic posting is Pages-only with review overhead	n/a (Pages API via partners like Buffer)	API free; cost is App Review time	https://www.outstand.so/blog/facebook-api · https://developers.facebook.com/documentation/pages-api	M/H
Reddit (community MCPs, e.g. jordanburke/reddit-mcp-server)	Social	6	DON'T	No official MCP; Reddit is actively shutting down public API access (Aug 2026 announcement) and the main community server was audited "after Reddit's API shutdown" (Oct 31 2026 commit)	stdio (community, 320 stars)	Reddit API paid/restricted	https://github.com/jordanburke/reddit-mcp-server · https://www.reddit.com/r/redditdev/comments/1vgbm9c/our_plans_for_the_future_of_reddits_public_data	H/M
Bluesky (community: brianellin/bsky-mcp-server, atproto-mcp, pmcosta 42-tool OAuth server)	Social	5	LATER	AT Protocol is API-first (low ban risk) but all servers are community stdio; Typefully already cross-posts to Bluesky	stdio (community)	Free (atproto)	https://github.com/brianellin/bsky-mcp-server · https://rye.dev/projects/atproto-mcp · https://bluesky.pmcosta.dev	M
Mastodon (community wrappers: Pipedream/Activepieces/viasocket)	Social	5	LATER	Open API, low risk, but no official server and tiny demand; Typefully/Buffer can post	Mostly hosted wrappers	Free	https://mcp.pipedream.com/app/mastodon · https://www.activepieces.com/mcp/mastodon	M
Ashby MCP (official, Jun 29 2026)	Job boards/ATS	ashby 16	DON'T (for your use case)	Official and good — but it's customer-side (recruiter data, 18 tools, opt-in by the hiring company). As an applicant you can't connect to someone else's Ashby	Hosted remote	Beta on every Ashby customer plan	https://www.ashbyhq.com/product-updates/mcp · https://docs.ashbyhq.com/ashby-mcp-server-beta	H
Greenhouse MCP (official, May 7 2026)	Job boards/ATS	greenhouse 10	DON'T (same reason)	Open beta, governed access for hiring teams on Greenhouse's tiers — not candidate-facing	Hosted remote	Included for Greenhouse customers (Core/Plus/Pro)	https://www.greenhouse.com/newsroom/greenhouse-launches-mcp-giving-hiring-teams-a-governed-way-to-connect-ai-tools-to-greenhouse · https://support.greenhouse.io/hc/en-us/articles/52090956642075-Greenhouse-MCP	H
Lever MCP (Composio/Apideck wrappers)	Job boards/ATS	lever 6	DON'T	No official server; wrappers are customer-side ATS ops	stdio/wrapped	Free tiers of wrappers	https://composio.dev/toolkits/lever	M
Wellfound / YC jobs	Job boards/ATS	5 / 9	DON'T (MCP-wise)	No public API/MCP; only scrapers (Apify/Scrapfly) — ToS risk; keep browser automation, use search MCPs to find roles	n/a	Scraper subscriptions	https://scrapfly.io/blog/posts/how-to-scrape-wellfound-aka-angellist · https://apify.com/neverempty/yc-startup-jobs-monitor	M
Hugging Face MCP (https://hf.co/mcp)	Hugging Face	18	TAKE	Official, free; Hub models/datasets/spaces + Gradio apps; Jul 2026 update reduced tool/token footprint	Hosted remote (+ self-hostable repo)	Free	https://huggingface.co/changelog/hf-mcp-server · https://huggingface.co/docs/hub/en/agents-mcp · https://huggingface.co/changelog/mcp-improvements-jul-26	H
Cloudflare managed MCP servers	Cloud/infra	cloudflare 3	LATER	Official managed remote servers (account read/manage) exist; demand too low to bother now	Hosted remote (also self-hostable cloudflare/mcp-server-cloudflare)	Free	https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare · https://github.com/cloudflare/mcp-server-cloudflare	H
Hetzner MCP (dkruyt/mcp-hetzner, lazyants/hetzner-mcp-server 104 tools)	Cloud/infra	hetzner 4+3	LATER (CLI/console is fine)	Community stdio only; 7 sessions don't justify a bridge	stdio (community)	Free (Hetzner API)	https://github.com/dkruyt/mcp-hetzner · https://mcpservers.org/servers/lazyants/hetzner-mcp-server	M
Tailscale (jaxxstorm/tailscale-mcp + Aperture MCP proxy)	Cloud/infra	tailscale 6	LATER — but study the pattern	Tailscale's own Lee Briggs built an HTTP MCP that runs inside your tailnet, and Aperture (Sep 2026) can proxy/grant remote MCP servers — a blueprint for your fleet's private gateway	Self-hosted Streamable HTTP in tailnet	Free	https://tailscale.com/blog/model-for-mcp-connectivity-lee-briggs · https://tailscale.com/docs/aperture/mcp-server	H
Docker MCP Catalog / Toolkit	Cloud/infra	docker CLI 19	LATER	Verified containerized servers (mcp.docker.com) — nice for ad-hoc stdio, but docker CLI already works	Local containers (stdio via Docker Desktop)	Free	https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog · https://mcp.docker.com	H
Calendly MCP (official, Mar 11 2026)	Scheduling/events	calendly 13	TAKE	Official hosted MCP; scheduling-link and event management beats browser driving	Hosted remote Streamable HTTP (requires MCP 2025-03-26+ client)	Included with Calendly (plan-gating unknown)	https://community.calendly.com/developer-faq-62/announcing-the-calendly-mcp-server-5450 · https://developer.calendly.com/docs/mcp/calendly-mcp-server · https://calendly.com/blog/mcp-server	H/M
Luma MCP (official)	Scheduling/events	luma 5	TAKE (small) / LATER	Official server for searching events ("find AI hackathons…"); community luma-events-mcp adds create/manage if ever needed	Hosted remote	Free	https://help.luma.com/p/mcp · https://github.com/Blackie360/luma-events-mcp	M
Partiful	Scheduling/events	3	n/a	No MCP found (unknown/none)	n/a	n/a	(searched; nothing found)	M
Airtable MCP (https://mcp.airtable.com/mcp)	Airtable/Notion	airtable 3	LATER	Official remote, free, respects your permissions — but 3 sessions don't justify OAuth surface	Hosted remote HTTP + OAuth or PAT	Included in current plan; counts against API limits	https://support.airtable.com/articles/9897799762-using-the-airtable-mcp-server · https://www.airtable.com/developers/agents/mcp/getting-started	H
Notion MCP (https://mcp.notion.com/mcp)	Airtable/Notion	0 (not a user)	LATER if adopted	Official hosted MCP (self-host repo now unmaintained); note: a prompt-injection complaint was reported against its connector copy	Hosted remote + OAuth	Free with Notion account (plan-gating unknown)	https://developers.notion.com/guides/mcp/get-started-with-mcp · https://github.com/makenotion/notion-mcp-server · https://www.reddit.com/r/Notion/comments/1w9depq	H/M
Playwright MCP (microsoft/playwright-mcp, ~37.8k stars)	Browser automation	(Chrome 15.3k calls is your current path)	LATER — ad hoc stdio only	The standard for non-Claude agents but stdio-only locally → violates your always-on rule; use "add, use, remove" in Codex/Cursor sessions	stdio (npx/Docker); Azure "Playwright Workspaces remote MCP" exists in preview (Streamable HTTP + Entra ID)	Free (OSS); Azure preview pricing unknown	https://github.com/microsoft/playwright-mcp · https://playwright.dev/docs/getting-started-mcp · https://learn.microsoft.com/azure/app-testing/playwright-cloud-browsers/how-to-playwright-workspaces-remote-mcp · https://playwright.aims-ai.com/blog/microsoft-playwright-mcp	H/M
Browserbase MCP (https://mcp.browserbase.com/mcp)	Browser automation	—	LATER	6-tool hosted cloud browser (Stagehand); paid and no snapshot tool; you already have Claude-in-Chrome	Hosted remote SHTTP	Free: 1 browser-hour; $20/mo for 100 hrs then $0.12/hr	https://github.com/browserbase/mcp-server-browserbase · https://browser-use.com/posts/best-cloud-browser-mcp-servers	H/M
Claude in Chrome (current)	Browser automation	15.3k calls/311	KEEP	Your single highest-value connector; caveat: Claude-only (Codex/Cursor can't use it)	First-party extension	Included	(your existing setup)	H
Slack MCP (https://mcp.slack.com/mcp, GA Feb 17 2026)	Messaging	0 measured	LATER	Mature official remote server (+ Real-time Search API), but you have zero Slack demand	Hosted remote + OAuth	Free tier exists; plan requirements unknown	https://docs.slack.dev/changelog/2026/02/17/slack-mcp · https://slack.com/blog/news/mcp-real-time-search-api-now-available · https://docs.slack.dev/ai/slack-mcp-server	H/M
Discord MCP (community, e.g. BennettSchwartz/discord-mcp)	Messaging	0 measured	LATER	No official server; bot-token community servers only	stdio (community)	Free (bot token)	https://github.com/BennettSchwartz/discord-mcp	M
Signal MCP (signal-cli based)	Messaging	0 measured	DON'T	Unofficial clients violate Signal's stance; signal-cli is fragile; zero demand	stdio (unofficial)	Free	https://github.com/AsamK/signal-cli	M
LLM web-UI automation as "connector" (ChatGPT/Gemini/Grok/GLM/Mistral UIs)	LLM vendors	75/56/49/34/8	DON'T connect; keep as-is, minimized	No vendor ships an MCP to drive their consumer chat UI; browser-driving consumer UIs is against ToS (e.g., OpenAI terms prohibit automated access) — your fan-out belongs on APIs (OpenRouter/official APIs)	n/a	n/a	https://openai.com/policies/row-terms-of-use	M
Hootsuite / Ayrshare / Metricool / Publer MCPs	Social schedulers	—	LATER	They exist (Hootsuite MCP integrations; Ayrshare 14-network API + MCP with 28-day trial) but duplicate Buffer/Typefully for your scale	Hosted remote (varies)	Ayrshare paid after trial; Hootsuite plans	https://www.hootsuite.com/integrations/mcp · https://www.ayrshare.com · https://www.postpeer.dev/blog/top-10-mcp-servers-for-social-media-management	M
2. Top-10 to connect now, ranked by expected value for you
GitHub official remote MCP — https://api.githubcopilot.com/mcp/. Your #1 non-browser workload (2,349 gh calls + 227 shell sessions). Free, OAuth, toolsets + MCP tool search shrink tool overhead from ~55k to ~4.2k tokens, and it's the only way to give GitHub to Claude web/ChatGPT connectors. Keep gh CLI alongside. (Prior art: strong — see Section C.)
Firecrawl MCP — https://mcp.firecrawl.dev/v2/mcp (keyless) or OAuth for all 29 tools. Replaces ad-hoc jina.ai/algolia fetches for every agent, and its research-index search (arXiv, PubMed, bioRxiv — free) directly serves your scholarly cluster. 1,000 free credits/mo. (Prior art: AIMultiple agent-search ranking at https://brave.com/search/api; Firecrawl FAQ "400,000+ MCP installs".)
Tavily MCP — https://mcp.tavily.com/mcp/ (keyless Streamable HTTP; 1,000 free credits/mo). Search for all agents without burning Chrome-control sessions; pair with Firecrawl for fetch. (Prior art: mcp.so Tavily entry; you.com free-search-MCP comparison https://you.com/resources/free-web-search-mcp.)
X official hosted MCP — you already added it; complete the OAuth. First-party posting/search with the lowest account risk; budget for $0.015/post ($0.20 with links). No scheduling — that's #5's job. (Prior art: TechCrunch coverage + OpenTweet guide https://opentweet.io/blog/xmcp-x-official-mcp-server-guide — notes pay-per-use and no scheduling.)
Typefully MCP — official remote server; create/edit/schedule drafts and threads across X, Bluesky, Threads, Mastodon (LinkedIn irrelevant — banned). Free plan. This is your scheduled-thread pipeline for job-market visibility. (Prior art: partial — Blotato vs Typefully https://www.blotato.com/blog/blotato-vs-typefully; pallyy 12-server comparison https://pallyy.com/blog/social-media-mcp-servers.)
OpenAlex official MCP — https://mcp.openalex.org/mcp (OAuth 2.1, Streamable HTTP). Your scholarly cluster is ~150 sessions; OpenAlex metadata (works, authors, citations, institutions) is the graph that links arXiv→DOI→ORCID lookups. Free, nonprofit-run. (Prior art not found beyond vendor docs/help center — I searched OpenAlex help, GitHub, Glama, mcpservers.org.)
Hugging Face MCP — https://hf.co/mcp. 18 sessions of models/datasets digging today; conversational Hub search + Gradio-app access; July 2026 token-diet update. Free. (Prior art: HF changelog + docs only — vendor-sourced.)
Calendly MCP — official hosted server (Mar 2026). 13 sessions of link-creation/event checking move from browser to tools. (Prior art: eSEOspace review https://eseospace.com/blog/calendly-mcp-server.)
dev.to remote MCP (self-hosted) — run nickytonline/dev-to-mcp as one daemon on your Linux VPS; 27 sessions of dev.to reading/writing via its public API, no auth needed. This is the model for any stdio server you can't live without: one process, HTTP, always-on (same as your Telegram/WhatsApp daemons). (Prior art: only the author's post + repo — "prior art not found" beyond that.)
OpenRouter MCP — https://mcp.openrouter.ai/mcp. For a one-person lab comparing frontier models weekly: live model catalog with benchmarks/prices, send-message A/B tests across models, credit checks — with a laudable security design (dedicated key, 7-day expiry, $10 cap). (Prior art: vendor docs only.)

Runner-ups (11–13): Exa MCP (if Tavily's index disappoints on neural/semantic queries), Airtable MCP (zero-effort, but 3 sessions), Kahubi free plan (one connection for Crossref+ORCID+DataCite).

3. Explicit DON'T list
DON'T CONNECT
	
REASON

Any LinkedIn MCP/automation (incl. ConnectSafely "LinkedIn MCP for Claude")	Direct ToS violation (https://www.linkedin.com/help/linkedin/answer/a1341387); account already banned; automation is the thing that gets accounts banned.
Any Facebook/Meta automation beyond partner schedulers	No personal-profile posting since 2018, Groups API dead 2024, Pages API requires App Review + Business Verification (https://www.outstand.so/blog/facebook-api) — 15 sessions don't justify it.
Reddit MCPs (all community)	No official server; Reddit is closing public API access (Aug 5 2026: https://www.reddit.com/r/redditdev/comments/1vgbm9c/); even the maintainer of the main community server audited it after "Reddit's API shutdown."
Ashby / Greenhouse / Lever MCPs for job hunting	All official/reputable ones are recruiter-side (customer data); an applicant cannot connect. Keep browser automation for applications.
Wellfound / YC-job scrapers	No public API; scraper products only (Apify/Scrapfly); ToS risk + automated applications are a bad look to frontier labs.
HackerNoon anything	No MCP exists; manual submission is the only path (searched — nothing found).
Signal MCP	Unofficial signal-cli stack; no official bot API; high fragility; zero demand.
stdio servers in the permanent config (Playwright MCP, arXiv MCP, Hetzner, Tailscale community, S2/Crossref/Zenodo/DataCite community servers, OpenRouter-multimodal community, domdomegg/airtable-mcp-server, etc.)	Your own rule — measured 176 processes/7 GB for one stdio server. Ad hoc only, or bridge via supergateway (https://github.com/supercorp-ai/supergateway) / sparfenyuk/mcp-proxy running once on the VPS.
Random servers from mcp.so listings	Supply-chain risk: MCP tool poisoning is a recorded attack class (CVE-2025-54136; CSA note Jun 24 2026: https://labs.cloudsecurityalliance.org/research/csa-research-note-ai-skill-supply-chain-attacks-20260624-csa; 118-findings audit: https://dev.to/ecap0/the-state-of-mcp-server-security-in-2026-118-findings-across-68-packages-4fkd).
Browserbase now	$20/mo for what Claude-in-Chrome already does free; hosted repo archived; API key sits in the URL query string (https://browser-use.com/posts/best-cloud-browser-mcp-servers). Revisit only if you need parallel cloud browsers.
Jina MCP + Brave MCP now	Jina duplicates Firecrawl; Brave has no hosted endpoint (stdio only — https://www.scalekit.com/blog/brave-search-mcp-vs-api). Both remain good ad-hoc/API options.
Slack/Discord now	Mature options exist (official Slack GA Feb 2026), but you measured zero demand — connecting would only add OAuth surface.
A. Buffer vs Typefully vs X-official MCP for "post to X from any machine"
	
X OFFICIAL HOSTED MCP
	
TYPEFULLY MCP
	
BUFFER MCP

What it is	X's own hosted MCP ("any MCP-compatible AI tool can talk to the X API with your own account's permissions", Jun 30 2026)	Official remote MCP from Typefully (Aug 2026 docs)	Official remote MCP (mcp.buffer.com/mcp)
Post to X	Yes — direct; billed	Yes — via drafts/scheduled posts	Yes — but X posts are text-only via MCP; no direct media (media only as links)
Threads	Unknown (API supports it; unverified for the MCP)	Yes — single tweets or threads	Yes — metadata.twitter.thread
Media	Unknown (not documented in sources I could open)	Unknown (Typefully product supports images; MCP media support unverified)	Text-only for X; other channels support media
Scheduling	No scheduling (OpenTweet guide)	Yes — schedule drafts	Yes — queue + schedule + edit/delete
Networks	X only	X, LinkedIn, Threads, Bluesky, Mastodon	14 networks (IG, FB, X, LinkedIn, Pinterest, YouTube, Google Business, Mastodon, TikTok, Threads, Bluesky, Substack, Start Page)
Pricing	MCP free; X API pay-per-use: $0.015/post, $0.20/post containing a link, reads $0.005/resource (docs.x.com; devcommunity Apr 20 2026 update)	Free plan; paid tiers (price: unknown — check typefully.com/pricing)	Free plan (3 channels, 10 scheduled posts/channel); paid ≈$5–6/channel/mo; MCP included on every plan incl. Free
Rate limits	X API quotas (pay-per-use)	Unknown	15-min / 24-h / 30-day windows per plan; shared between MCP and API keys
Ban risk	Lowest — first-party, your account's own permissions	Low — posts through Typefully's partner integration	Low — official partner APIs; Buffer has posted via official APIs for a decade
Security notes	Posts cost real money per call — set a hard spend cap in the X developer account	OAuth; full posting rights on connected accounts	OAuth covers whole account, no per-org scoping

Recommendation: Finish the X official MCP OAuth (already queued) for interactive posting/reading; add Typefully as the scheduling/threads engine (free plan, cross-posts to Bluesky/Mastodon which you touch at 5 sessions each); keep Buffer connected only if you decide the 14-network hub (FB Pages, Threads, Substack) is worth it — its X-text-only limitation makes it a poor primary for you. Sources: https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558 · https://techcrunch.com/2026/06/30/x-now-offers-an-mcp-server-to-make-its-platform-easier-for-ai-tools-to-use · https://docs.x.com/x-api/getting-started/pricing · https://opentweet.io/blog/xmcp-x-official-mcp-server-guide · https://support.typefully.com/en/articles/13128440-typefully-mcp-server · https://developers.buffer.com/guides/integrations/mcp.html (fetched) · https://pallyy.com/blog/social-media-mcp-servers.

B. The catalogs, and how to spot abandonware
claude.ai Connectors directory — in-app + web directory of remote-only OAuth connectors; ~8,000+ integrations catalogued by trackers (awesome-claude-connectors, Oct 30 2026: https://github.com/rdmgator12/awesome-claude-connectors; official help: https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities). Curated; perfectly aligned with your remote-only rule.
Official MCP Registry — https://registry.modelcontextprotocol.io, launched in preview Sep 8 2025 (https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview), still labeled preview (https://modelcontextprotocol.io/registry/about). It is a metadata catalog + 302 redirect to the publisher's own transport URL — not a hosting service; publisher identity is the key trust signal. Live and actively updated (entries stamped Nov 6 2026).
OpenAI/ChatGPT connectors — ChatGPT "Apps & connectors" directory built on the Apps SDK; custom MCP servers can be added without Developer Mode now (https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt); Developer Mode gives full read/write MCP client support (https://community.openai.com/t/mcp-server-tools-now-in-chatgpt-developer-mode/1357233). Codex CLI takes MCP via ~/.codex/config.toml (stdio and streamable HTTP, bearer/OAuth: https://developers.openai.com/api/docs/guides/custom-mcp-server).
Cursor — Cursor Marketplace (https://cursor.com/marketplace) with verified plugins incl. MCP servers; docs at https://cursor.com/docs/mcp; the old cursor/mcp-servers repo is deprecated (Mar 2026).
Smithery — pivoted: Arcade.dev acquired Smithery Aug 5 2026 (https://www.arcade.dev/blog/smithery-joins-arcade; Forbes: https://www.forbes.com/sites/janakirammsv/2026/08/10/arcade-acquires-smithery-to-own-the-agent-tool-supply-chain); the MCP Proxy + managed identity shut down Oct 31 2026 — Smithery remains only as a non-proxied public registry (https://smithery.ai/future). Don't build anything on its hosted URLs.
mcp.so — ~21k auto-indexed listings, light vetting; discovery only.
Glama (https://glama.ai) — registry + gateway with health checks, quality scores, security audits, and a published methodology (https://glama.ai/mcp/methodology). My default first stop for vetting.
Also useful: PulseMCP (21,550+ servers, daily-updated: https://www.pulsemcp.com/servers) and Docker MCP Catalog (verified signed images: https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog).

Maintained vs abandonware — checklist: (1) vendor-operated remote endpoint on the vendor's own domain with OAuth; (2) release/commit activity in the last ~6 months and a changelog; (3) published in the official MCP Registry under the vendor's verified org; (4) present in claude.ai/Cursor curated directories; (5) Glama quality score + security scan; (6) issue/PR responsiveness; (7) transport hygiene (Streamable HTTP + OAuth 2.1, per Cloudflare's guidance: https://developers.cloudflare.com/agents/model-context-protocol). Academic context: the arXiv measurement study of MCP markets (https://arxiv.org/html/2509.25292v1) found registries differ widely in metadata practices — trust vendor domains first. Negative example: makenotion/notion-mcp-server (self-hosted) is now explicitly "no longer actively maintained" while the hosted mcp.notion.com/mcp is the supported path — same vendor, two very different maintenance statuses.

C. Is the official remote GitHub MCP worth it over gh CLI?

Yes — take it, keep gh CLI too. The facts:

Token cost of tool definitions is a solved problem. Anthropic measured a 5-server setup at ~55K tokens of tool definitions before a word is spoken (GitHub alone: 35 tools ≈ 26K tokens) and reports ~85% input-token reduction with the Tool Search Tool (https://www.anthropic.com/research/advanced-tool-use). GitHub ships MCP tool search + toolsets: an independent walkthrough measured ~55,000 → ~4,200 tokens after enabling toolsets on the GitHub MCP server (https://blog.picassoia.com/github-mcp-server-setup-token-usage-and-rate-limits), and GitHub's own engineering blog quantifies a 40-tool server at 10–15 KB of schema per turn (https://github.blog/ai-and-ml/github-copilot/improving-token-efficiency-in-github-agentic-workflows). Claude Code's A/B with tool search showed a 46.9% total-token reduction in MCP-using runs (https://medium.com/@joe.njenga/claude-code-just-cut-mcp-context-bloat-by-46-9-51k-tokens-down-to-8-5k-with-new-tool-search-ddf9e905f734).
Rate limits are a non-issue at your volume. The remote server inherits GitHub's REST limit of 5,000 requests/hour (https://docs.github.com/rest/using-the-rest-api/rate-limits-for-the-rest-api; https://zuplo.com/blog/never-ship-mcp-server-without-rate-limit). Your measured gh usage (~2,349 calls in ~6 months ≈ 13/day) is nowhere near.
What MCP gives over gh CLI: OAuth (no PAT rotation/leakage), per-toolset scoping (run read-only toolsets by default; Jan 28 2026 update added permission-based automatic tool filtering — https://github.blog/changelog/2026-01-28-github-mcp-server-new-projects-tools-oauth-scope-filtering-and-new-features), structured outputs, and — decisively — availability in clients where no shell exists: Claude web/Desktop connectors, ChatGPT connectors, Cursor, Gemini CLI.
What gh CLI still does better: zero tool-definition overhead, works offline/in scripts, no OAuth dance on new machines, already battle-tested in your n8n flows.
Prior art on the counter-side: Stacklok argues tool-search accuracy can drop and offers an alternative optimizer (https://stacklok.com/blog/stackloks-mcp-optimizer-vs-anthropics-tool-search-tool-a-head-to-head-comparison) — a reason to keep toolsets tight rather than load all 40 tools.

Verdict: connect https://api.githubcopilot.com/mcp/ with a minimal toolset profile (issues/PRs/code first), keep gh CLI as the workhorse in scripts. Cost: free.

4. Prior-art map (or "prior art not found" + where I looked)
RECOMMENDATION
	
PRIOR ART
	
WHERE I LOOKED

GitHub remote MCP + token math	Yes, extensive: Anthropic advanced-tool-use post; GitHub blog (May 13 2026); picassoia walkthrough (Oct 6 2026); zuplo (May 18 2026); Joe Njenga Medium; Stacklok comparison	Anthropic, GitHub blog/docs, zuplo, dev.to, Medium, Stacklok
Tool search / MCP token overhead	Yes (all above + Cloudflare "Code Mode" guidance)	anthropic.com, developers.cloudflare.com
Firecrawl / Tavily / Exa / Brave choice	Yes, partially: AIMultiple agent-search ranking (on brave.com/search/api); you.com free-MCP comparison (Oct 11 2026); scalekit per-vendor "MCP vs API" posts	brave.com, you.com, scalekit.com, mcp.so
X official MCP	Yes: TechCrunch (Jun 30 2026), OpenTweet guide (Jul 4 2026), devcommunity pricing update (Apr 2026)	devcommunity.x.com, techcrunch.com, opentweet.io
Buffer vs Typefully vs others	Yes: pallyy 12-server comparison (Oct 1 2026); PostPeer top-10 (Sep 25 2026); Blotato vs Typefully (Sep 1 2026); Blotato API/MCP pricing research (Oct 7 2026); eSEOspace Buffer review (Oct 28 2026)	pallyy.com, postpeer.dev, blotato.com, eseospace.com
Cloud browser MCPs	Yes: browser-use "Best Cloud Browser MCP Servers (2026)" (Browserbase, Kernel, browser-use compared, with pricing)	browser-use.com
OpenAlex MCP	Not found beyond vendor docs/help center (help.openalex.org, GitHub ourresearch, aiagentstore listing)	help.openalex.org, GitHub, Glama, mcpservers.org, composio
Hugging Face MCP	Vendor-only (HF changelog Jun 2025 + Jul 2026, HF docs)	huggingface.co, GitHub
Calendly MCP	eSEOspace review (Oct 28 2026); Calendly community/docs	calendly.com, eseospace.com
dev.to remote MCP	Not found beyond the author's own post/repo	dev.to, GitHub, goose docs
Typefully MCP specifics (media via MCP, paid price)	Not found / unknown	support.typefully.com, typefully.com/pricing, Reddit r/mcp, Boomi docs
OpenRouter MCP	Vendor docs only (fetched in full)	openrouter.ai
Slack MCP	Yes: official docs/changelog; gamut.so guide (Jun 11 2026); growthmethod (Aug 20 2026) — plan-gating still unknown	docs.slack.dev, slack.com, gamut.so, reclaim.ai
Ashby/Greenhouse as applicant tools	Not found (all coverage is recruiter-side; my conclusion is inference from vendor docs — treat as medium confidence)	ashbyhq.com, greenhouse.com, agentman.ai, Reddit
MCP security (poisoning, injection)	Yes: CSA research note (Jun 24 2026, CVE-2025-54136); "State of MCP Server Security 2026" 118 findings (Feb 16 2026); Notion connector injection complaint (Reddit r/Notion)	cloudsecurityalliance.org, dev.to, reddit.com
Smithery pivot	Yes: arcade.dev blog (Aug 5 2026), Forbes (Aug 10 2026), smithery.ai/future	arcade.dev, forbes.com, smithery.ai
Bottom line

Connect now: GitHub remote, Firecrawl, Tavily (or Exa), X official (finish OAuth), Typefully, OpenAlex, Hugging Face, Calendly, self-hosted dev.to MCP, OpenRouter. Drop/never connect: LinkedIn, Facebook automation, Reddit, ATS MCPs for job hunting, Signal, unvetted mcp.so listings, and every stdio server in the permanent config (bridge them with supergateway on the VPS if you truly need one always-on). Your existing first-party Google connectors + Claude-in-Chrome + your own Telegram/WhatsApp daemons remain the correct backbone — nothing I found supersedes them.

One caveat: the built-in web-search/fetch quota capped mid-research, so the last ~25 lookups ran through the CLI search tool; snippets there are shallower than full-page reads. Items marked M or "unknown" (Typefully paid price, X MCP media/thread specifics, Slack plan-gating, Hashnode endpoint URL) deserve a 2-minute check on the vendor page before you wire them in.

## Sources (hrefs)
https://github.com/github/github-mcp-server
https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview
https://www.scalekit.com/blog/github-mcp-server-per-user-oauth
https://openrouter.ai/docs/guides/overview/mcp-server
https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558
https://techcrunch.com/2026/06/30/x-now-offers-an-mcp-server-to-make-its-platform-easier-for-ai-tools-to-use
https://docs.x.com/x-api/getting-started/pricing
https://support.typefully.com/en/articles/13128440-typefully-mcp-server
https://typefully.com/changelog
https://developers.buffer.com/guides/integrations/mcp.html
https://pallyy.com/blog/social-media-mcp-servers
https://www.softwaresuggest.com/buffer/pricing
https://docs.firecrawl.dev/mcp-server
https://github.com/firecrawl/firecrawl-mcp-server
https://www.firecrawl.dev/pricing
https://docs.tavily.com/documentation/keyless
https://github.com/tavily-ai/tavily-mcp
https://mcp.so/servers/tavily-mcp
https://exa.ai/docs/get-started/exa-mcp
https://exa.ai/pricing
https://www.scalekit.com/blog/exa-mcp-vs-api
https://github.com/brave/brave-search-mcp-server
https://www.scalekit.com/blog/brave-search-mcp-vs-api
https://brave.com/search/api
https://github.com/jina-ai/MCP
https://jina.ai/reader
https://github.com/ourresearch/openalex-mcp-server
https://help.openalex.org/access/connector
https://github.com/smaniches/semantic-scholar-mcp
https://pypi.org/project/semantic-scholar-mcp
https://github.com/blazickjp/arxiv-mcp-server
https://github.com/cyanheads/orcid-mcp-server
https://github.com/JackKuo666/crossref-MCP-Server
https://github.com/MSKazemi/mcp-zenodo
https://github.com/alexwade/datacite-mcp
https://kahubi.com/sources/orcid
https://kahubi.com/sources/crossref
https://kahubi.com/sources/datacite
https://dev.to/nickytonline/introducing-the-devto-mcp-server-42jg
https://github.com/nickytonline/dev-to-mcp
https://hashnode.com/blog/hashnode-mcp-server
https://www.linkedin.com/help/linkedin/answer/a1341387
https://www.linkedin.com/pulse/ashbys-mcp-server-goes-live-ai-reshapes-hiring-teams-rethink-funnel-7kjwe
https://www.outstand.so/blog/facebook-api
https://developers.facebook.com/documentation/pages-api
https://github.com/jordanburke/reddit-mcp-server
https://www.reddit.com/r/redditdev/comments/1vgbm9c/our_plans_for_the_future_of_reddits_public_data
https://github.com/brianellin/bsky-mcp-server
https://rye.dev/projects/atproto-mcp
https://bluesky.pmcosta.dev/
https://mcp.pipedream.com/app/mastodon
https://www.activepieces.com/mcp/mastodon
https://www.ashbyhq.com/product-updates/mcp
https://docs.ashbyhq.com/ashby-mcp-server-beta
https://www.greenhouse.com/newsroom/greenhouse-launches-mcp-giving-hiring-teams-a-governed-way-to-connect-ai-tools-to-greenhouse
https://support.greenhouse.io/hc/en-us/articles/52090956642075-Greenhouse-MCP
https://composio.dev/toolkits/lever
https://scrapfly.io/blog/posts/how-to-scrape-wellfound-aka-angellist
https://apify.com/neverempty/yc-startup-jobs-monitor
https://huggingface.co/changelog/hf-mcp-server
https://huggingface.co/docs/hub/en/agents-mcp
https://huggingface.co/changelog/mcp-improvements-jul-26
https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare
https://github.com/cloudflare/mcp-server-cloudflare
https://github.com/dkruyt/mcp-hetzner
https://mcpservers.org/servers/lazyants/hetzner-mcp-server
https://tailscale.com/blog/model-for-mcp-connectivity-lee-briggs
https://tailscale.com/docs/aperture/mcp-server
https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog
https://mcp.docker.com/
https://community.calendly.com/developer-faq-62/announcing-the-calendly-mcp-server-5450
https://developer.calendly.com/docs/mcp/calendly-mcp-server
https://calendly.com/blog/mcp-server
https://help.luma.com/p/mcp
https://github.com/Blackie360/luma-events-mcp
https://support.airtable.com/articles/9897799762-using-the-airtable-mcp-server
https://www.airtable.com/developers/agents/mcp/getting-started
https://developers.notion.com/guides/mcp/get-started-with-mcp
https://github.com/makenotion/notion-mcp-server
https://www.reddit.com/r/Notion/comments/1w9depq
https://github.com/microsoft/playwright-mcp
https://playwright.dev/docs/getting-started-mcp
https://learn.microsoft.com/azure/app-testing/playwright-cloud-browsers/how-to-playwright-workspaces-remote-mcp
https://playwright.aims-ai.com/blog/microsoft-playwright-mcp
https://github.com/browserbase/mcp-server-browserbase
https://browser-use.com/posts/best-cloud-browser-mcp-servers
https://docs.slack.dev/changelog/2026/02/17/slack-mcp
https://slack.com/blog/news/mcp-real-time-search-api-now-available
https://docs.slack.dev/ai/slack-mcp-server
https://github.com/BennettSchwartz/discord-mcp
https://github.com/AsamK/signal-cli
https://openai.com/policies/row-terms-of-use
https://www.hootsuite.com/integrations/mcp
https://www.ayrshare.com/
https://www.postpeer.dev/blog/top-10-mcp-servers-for-social-media-management
https://you.com/resources/free-web-search-mcp
https://opentweet.io/blog/xmcp-x-official-mcp-server-guide
https://www.blotato.com/blog/blotato-vs-typefully
https://eseospace.com/blog/calendly-mcp-server
https://www.reddit.com/r/redditdev/comments/1vgbm9c/
https://github.com/supercorp-ai/supergateway
https://labs.cloudsecurityalliance.org/research/csa-research-note-ai-skill-supply-chain-attacks-20260624-csa
https://dev.to/ecap0/the-state-of-mcp-server-security-in-2026-118-findings-across-68-packages-4fkd
https://github.com/rdmgator12/awesome-claude-connectors
https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities
https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview
https://modelcontextprotocol.io/registry/about
https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt
https://community.openai.com/t/mcp-server-tools-now-in-chatgpt-developer-mode/1357233
https://developers.openai.com/api/docs/guides/custom-mcp-server
https://cursor.com/docs/mcp
https://www.arcade.dev/blog/smithery-joins-arcade
https://www.forbes.com/sites/janakirammsv/2026/08/10/arcade-acquires-smithery-to-own-the-agent-tool-supply-chain
https://smithery.ai/future
https://glama.ai/mcp/methodology
https://www.pulsemcp.com/servers
https://developers.cloudflare.com/agents/model-context-protocol
https://arxiv.org/html/2509.25292v1
https://www.anthropic.com/research/advanced-tool-use
https://blog.picassoia.com/github-mcp-server-setup-token-usage-and-rate-limits
https://github.blog/ai-and-ml/github-copilot/improving-token-efficiency-in-github-agentic-workflows
https://medium.com/@joe.njenga/claude-code-just-cut-mcp-context-bloat-by-46-9-51k-tokens-down-to-8-5k-with-new-tool-search-ddf9e905f734
https://docs.github.com/rest/using-the-rest-api/rate-limits-for-the-rest-api
https://zuplo.com/blog/never-ship-mcp-server-without-rate-limit
https://github.blog/changelog/2026-01-28-github-mcp-server-new-projects-tools-oauth-scope-filtering-and-new-features
https://stacklok.com/blog/stackloks-mcp-optimizer-vs-anthropics-tool-search-tool-a-head-to-head-comparison