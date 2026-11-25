# DR26-11-17-MACANTON-01-1941 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Show more
7:46pm
Thought for 8s

I'll run this deep research on the AI-agent skill & tool catalog market. Given the scope (5 question areas, 30+ sources required), I'll work in structured search batches: first mapping the catalogs, then business models, traffic evidence, submission mechanics, and finally synthesizing recommendations.

Ran 8 searches

Strong start. Batch 2: agent-skill catalogs, IDE catalogs, and the official registry details.

Ran 10 searches

Rich results. Batch 3: business models, funding, pivots/shutdowns, and traffic evidence.

Ran 10 searches

Now I'll open the key primary sources. Batch 4: official registry, Smithery, PulseMCP, mcp.so, Glama, Claude directory, Vercel skills.

Ran a web search

Key primary sources confirmed. Batch 5: opening remaining major catalog sources and audits.

Ran a web search

Batch 6: awesome-lists, more skill hubs, packaging, enterprise registries, and the user's own repos.

Ran 10 searches

Batch 7: opening funding, GitHub registry, mcp.so economics, Vercel launch, and audit sources.

Ran a web search

Batch 8: final opens for traffic evidence, GitHub registry governance, and remaining comparisons.

Ran a web search

Batch 9: final verification opens (funding details, paused directories, submission requirements, awesome-lists).

Ran a web search

All research complete: 38 search queries and ~45 pages opened across the major catalogs, official docs, funding press, and independent audits. Here is the full report, delivered in chat as requested.

DR26-11-17-MACANTON-01-1941: The Economics and Mechanics of AI-Agent Skill & Tool Catalogs (MCP Registries, Skill Marketplaces, Awesome-Lists), November 2026

Question: What catalogs exist in late 2026 for distributing free Agent Skills and MCP servers, how does each make money, what real reach does a listing bring, how do you get in, and which venues deserve a small free-only open-source lab's limited hours?

Tags used: CONFIRMED = read on a primary source; REPORTED = stated by a named secondary source I inspected; INFERRED = my synthesis; UNKNOWN / not public = no evidence found.

Executive Summary
One publication now fans out to most of the ecosystem. The official MCP Registry (launched 8 Sep 2025, mcp-publisher CLI, no review, free) is copied by Glama, PulseMCP, mcp.directory and GitHub's registry, so a single server.json publish is the highest-leverage action for any MCP server (CONFIRMED).
Smithery exited the hosting business. Arcade acquired Smithery on 5 Aug 2026; the MCP proxy and managed identity shut down 31 Oct 2026, and Smithery survives only as a public non-proxied directory (CONFIRMED). Arcade had reported a $60M Series A led by SYN Ventures with Morgan Stanley and Wipro participation weeks earlier (REPORTED).
PulseMCP has paused manual submissions (since ~mid-August 2026) while it reworks ingestion, and says it will import official-registry entries when it reopens (CONFIRMED). Listing there is currently not an action you can take.
Skill distribution has consolidated around skills.sh (Vercel, launched 20 Jan 2026): ~1 million skills and ~280M installs in 7 months, ranked purely by anonymous CLI install telemetry, with no submission form at all (CONFIRMED). But 375 skills (0.04%) captured 62% of installs and nearly half of all skills were installed exactly once: a listing without distribution motion is invisible (CONFIRMED).
Anthropic opened its directory to everyone on 25 Sep 2026 via claude.ai/directory/manage: any paid-plan user can submit a GitHub-repo plugin bundle (skills + local MCP) or a remote MCP connector, with automated validation plus human check, Git-based updates, and post-launch usage analytics (CONFIRMED).
Paid fast-lanes are the main directory revenue model: mcp.so $39 one-time (publish immediately, dofollow, featured), mcpservers.org $39 (24h review), MCP Market $29 (24h) vs a free 4–6 week queue. These are SEO businesses selling links and placement, not curated stores (CONFIRMED).
Directory traffic is small and self-reported numbers are inflated. A third-party audit (DataForSEO, 27 Oct 2026) found only 3 of 21 MCP directories exceed 10,000 search visits/month; the biggest (MCP Market) got ~81k/month, far below its own "1M+ monthly visitors" claim (CONFIRMED, conflict noted).
Catalog counts are heavily duplicated. One audit collapsed 17,320 registry rows to 11,447 unique GitHub-backed servers; 32.8% had no commit in 180+ days (CONFIRMED). Dead and stale listings are endemic; a merged community list found 94 entries pointing at dead repos (REPORTED).
Client-embedded catalogs (Claude directory, ChatGPT/Codex plugin directory, Cursor Marketplace, Cline, Docker, VS Code gallery via GitHub's registry) have the real users but the hardest gates: identity verification, test accounts, per-tool annotations, privacy policies, human review (CONFIRMED).
For a free giver, the winning play is: publish every MCP server to the official registry once, put each flagship skill in its own clean repo that npx skills add can install, submit a small number of curated plugin bundles to the Claude directory, take the free GitHub-list PRs, and skip pay-to-play venues until they prove referral traffic (INFERRED).
Methodology
38 search queries across web and news corpora, plus ~45 pages opened directly (official docs, registry quickstarts, submission pages, a shutdown notice, funding press, and three independent directory audits).
Primary sources prioritized: modelcontextprotocol.io, claude.com, developers.openai.com, cursor.com, smithery.ai, glama.ai, mcp.so, GitHub repos and the GitHub Blog.
Limitations: directories self-report traffic (only one third-party traffic estimate was found, DataForSEO via submitmcp.com, search-only US visits); pulsemcp.com and forbes.com blocked automated access (their content was captured via search snippets and secondary sources); no catalog publishes revenue; install/visit numbers change weekly. All figures are dated.
Master Table
Catalog
Class
Accepts
Business model
Entry door
Requirements
Evidence of traffic
Status (17 Nov 2026)
Sources
Official MCP Registry
Community metaregistry (source of truth)
MCP servers (packages: npm/PyPI/OCI; or remote-only)
None; open-source MCP project
mcp-publisher CLI publish, no review, free
server.json, namespace verification via GitHub or DNS TXT; registry hosts metadata only
Feeds Glama, PulseMCP, mcp.directory, GitHub registry
Alive (preview)
 MCP blog,  Quickstart
GitHub MCP Registry
Curated registry layer (github.com/mcp)
MCP servers
Free; GitHub ecosystem play
Publish to official registry first, then onboarding request in GitHub discussion; curated by hand
Public repo, license; ranked by stars
~65 servers Jan 2026, grows slowly; feeds VS Code one-click gallery
Alive
 GitHub Blog,  Composio,  Descope
Glama
Independent mega-directory + hosting + gateway
MCP servers, connectors, tools
Freemium: free directory; paid one-click hosting (isolated VMs) and gateway
Auto-imports official registry; "Add Server" for hosting; claim via well-known file
Scanned, scored (security, compatibility); README, license
122,771 servers, 50k+ developers, 1M+ tool calls/month (self-report); 2.2M?? no, see mcp.so
Alive
 Glama,  Hosting blog,  MentionAgent audit
Smithery
Independent registry, was proxy/hosting
MCP servers
Was: paid hosting/proxy plans; now: lead-gen for Arcade
Directory remains public; new-listing process post-acquisition unclear
Verified set shows direct MCP URLs
Was "one of the most used public registries" (press); installs not public
Alive as directory; proxy dead 31 Oct 2026
 Smithery future page,  RuntimeWire
PulseMCP
Independent directory + newsletter
MCP servers
Free; newsletter; funding not public
Paused: no new submissions or listing changes
Will import official registry entries on reopen
~7k–21k servers (sources conflict); ~81k visits/mo is mcp.so's rival claim, not this; weekly newsletter reach not public
Paused (since ~Aug 2026)
 PulseMCP,  MCPize guide,  BestMCPMarketplace
mcp.so
Independent mega-directory (SEO/link sales)
MCP servers, clients, CLIs, Agent Skills, "Loops"
$39 one-time premium (instant, dofollow, featured) + ads
Web form; free-with-review (behind sign-in) or $39
Repo or endpoint URL; anonymous remote submission only reachable via $39
Self-reports 2.2M unique visitors/12mo, DR 74, 6M pageviews
Alive
 mcp.so/submit,  mcpso issue #5418
mcpservers.org (ex awesome-mcp-servers wong2)
Independent directory grown from an awesome-list
MCP servers
$39 premium (24h review, badge, priority, dofollow); sponsor slots existed
Web form (repo explicitly says: no PRs)
Free queue up to ~2 weeks
21,841 servers listed; DR 71 per audit
Alive
 mcpservers.org,  wong2 repo,  MentionAgent
MCP Market
Independent directory
MCP servers + agent skills
$29/24h (badge, "Try Now" link) vs free 4–6 week queue; $69 remote
Web form, GitHub repo URL
Email for notification
Self-claims 1M+ monthly visitors; third-party: 81,100 search visits/mo
Alive
 MCP Market submit,  SubmitMCP
mcp.directory
Independent directory, one-click install
MCP servers + agent skills
Free; monetization not visible
Form (GitHub repo URL), 24h review; auto-imports official registry; claim via email
Auto-pulls name/desc/license/tools from GitHub
~3,000 servers; small
Alive
 mcp.directory/submit,  Top MCPs
Claude directory + Claude Code plugins (Anthropic)
Client-embedded directory
Plugins (skills, commands, hooks, agents, MCP connectors)
Free listing; monetizes Claude plans
Portal claude.ai/directory/manage (open 25 Sep 2026)
Paid Claude plan, GitHub repo (public at launch), claude plugin validate, safety scan + human check, privacy policy
"Millions of people connect Claude" (Anthropic); per-listing views/installs shown to publisher
Alive
 Anthropic blog,  Docs,  claude-plugins-official
ChatGPT + Codex plugin directory (OpenAI)
Client-embedded directory
Plugins: MCP connections, skills, or both
Free listing; monetizes ChatGPT/Codex
"With MCP" submission in OpenAI Platform
Stable public HTTPS endpoint, identity verification, Apps Management write access, tool annotations, 5 positive + 3 negative test cases, video, test account
Not public
Alive
 OpenAI docs,  Flowlines guide,  SubmitMCP
Cursor Marketplace (+ cursor.directory)
Client-embedded marketplace
Plugins: skills, subagents, rules, hooks, MCP servers
Free listing; monetizes Cursor
Submit public repo link (plugin.json + mcp.json)
Hand-picked, mostly companies, must be free and MIT/BSD/Apache; weeks, no status page
Featured slots only; reach not public
Alive
 Cursor Marketplace,  SubmitMCP
Cline MCP Marketplace
Client-embedded marketplace
MCP servers
Free; open-source project
GitHub issue in cline/mcp-marketplace
Repo URL, 400x400 PNG logo, justification; approval process
"Millions of developers" (Cline's own README claim)
Alive
 cline/mcp-marketplace
Docker MCP Catalog
Client/runtime-embedded catalog
MCP servers (as images)
Free listing; monetizes Docker Hub/enterprise
Pull request to docker/mcp-registry
Docker builds, signs, publishes to mcp/your-server-name; catalog entry ~24h
Enterprise install base; counts not public
Alive (commits 10 Nov 2026)
 docker/mcp-registry,  Docker docs
VS Code MCP gallery
Client-embedded
MCP servers
Free; Microsoft ecosystem
Via GitHub MCP Registry
One-click install configs
Fed by GitHub registry
Alive
 GitHub Blog
skills.sh (Vercel)
Agent-skill registry + leaderboard
SKILL.md packages in any public GitHub repo
Free, MIT CLI, no paid tiers, no published model; ecosystem play
None: automatic via install telemetry
SKILL.md in repo; npx skills add owner/repo
1M skills, ~280M installs in 7 months; top skill 3.4M installs
Alive
 Vercel changelog,  State of agent skills,  skills.sh docs
agenticskills.io (AgenticSkills)
Curated skill directory
Skills + MCP servers
Free + paid featured placement/newsletter
Web form
Public source, valid SKILL.md format, security check; selective, triaged weekly
197+ skills, 142 MCP servers indexed
Alive
 AgenticSkills submit
ClawHub, awesomeskill.ai, aiskill.market, skillsmp.com, skillselion, awesomeclaude.ai
Third-party skill hubs/aggregators
SKILL.md skills
Mixed; mostly not public
Various forms; ClawHub via CLI publish
Varies
skillsmp.com claims ~4M skills scraped from GitHub; awesomeclaude.ai dedupes and found 94 dead entries
Alive but shallow
 ClawHub,  LinklyAI/best-skills,  aiskill.market
awesome-mcp-servers (punkpeye)
Awesome-list
MCP servers
None (free, MIT)
PR per CONTRIBUTING.md
One line, category, alphabetical; curated
97.8k stars, 20.2k forks; referral traffic real but unquantified
Alive
 punkpeye/awesome-mcp-servers
awesome-claude-skills (ComposioHQ), awesome-claude-code (hesreallyhim), awesome-claude-code-plugins (ccplugins)
Awesome-lists
Skills, plugins, hooks, agents
None; Composio uses it for marketing
PR
Curated; subinium's variant demands 1,000+ stars
52.9k stars (awesome-claude-code); Composio list claims 1,000+ entries
Alive
 ComposioHQ,  hesreallyhim,  ccplugins
PyPI / npm / Docker Hub
Package indexes
The actual artifacts (referenced by server.json)
Hosting/registry fees for publishers, free for OSS
Normal package publish
Registry schema points at them; npm account needed for TS servers
Download stats available (pip/npm)
Alive
 Registry quickstart,  Composio
ToolHive (Stackloit/Stacklok) registry server
Enterprise private registry/governance
MCP servers, skills, plugins
Open-source core; enterprise offering
Self-host; aggregates official registry + internal sources
Policy, provenance, signing
Not public
Alive
 stacklok/toolhive,  ToolHive docs
1. Taxonomy: What Kinds of Catalogs Exist (late 2026)

A. The official metaregistry. registry.modelcontextprotocol.io, live since 8 September 2025 in preview, is the MCP project's own registry: an open-source catalog and API holding self-reported server metadata only, never artifacts, with community-driven moderation and explicit support for public "sub-registries" (opinionated marketplaces) and private enterprise ones (CONFIRMED). ( MCP Registry announcement)

B. Curated registry layers. The GitHub MCP Registry (github.com/mcp, launched 16 September 2025) is a curated directory sorted by GitHub stars with one-click VS Code install; it is governed by a community working group under the MCP charter, not by GitHub alone (CONFIRMED). ( GitHub Blog,  Composio)

C. Independent mega-directories/aggregators. Glama (122,771 servers, calls itself "a superset of the official MCP Registry"), PulseMCP, mcp.so (~21,000), mcpservers.org (21,841), MCP Market (35k+), mcp.directory (~3,000), Skiln (75k+ federated from 10+ registries), Top MCPs (~280 curated with a published 0–100 rubric) (CONFIRMED). ( Glama,  Top MCPs comparison,  Skiln,  mcpservers.org)

D. Client-embedded marketplaces (where end users actually click Connect): Anthropic's Claude directory (plugins bundling skills + MCP connectors), OpenAI's ChatGPT/Codex plugin directory, Cursor Marketplace plus the community cursor.directory, Cline's MCP Marketplace, Docker's MCP Catalog, and VS Code's gallery fed by GitHub's registry (CONFIRMED). ( Claude blog,  OpenAI,  Cursor,  Cline,  Docker)

E. Agent-skill catalogs. skills.sh is the dominant one: any public GitHub repo with SKILL.md files becomes installable via npx skills add owner/repo across 20+ agents, listed automatically with zero submission (CONFIRMED). ( skills.sh docs,  Vercel changelog) agentskills.io is the open Agent Skills specification (maintained by Anthropic), not a marketplace; agenticskills.io is a third-party curated directory (197+ skills, 142 MCP servers) taking free selective submissions (CONFIRMED). ( AgenticSkills) Below these sits a shelf of thin third-party hubs (ClawHub, awesomeskill.ai, aiskill.market, skillsmp.com, skillselion, awesomeclaude.ai) that scrape or republish SKILL.md repos (CONFIRMED for their existence; quality INFERRED to be low). ( ClawHub,  LinklyAI/best-skills)

F. Awesome-lists. punkpeye/awesome-mcp-servers (97.8k stars) remains the reference list; wong2/awesome-mcp-servers stopped accepting PRs entirely and redirects to its website mcpservers.org, showing lists evolving into commercial directories (CONFIRMED). ( punkpeye,  wong2) For skills: ComposioHQ/awesome-claude-skills, hesreallyhim/awesome-claude-code (52.9k stars), ccplugins/awesome-claude-code-plugins, and variants with star thresholds (subinium requires 1,000+) (CONFIRMED). ( hesreallyhim,  ccplugins)

G. Package indexes and bundles. PyPI/npm/Docker Hub host the artifacts that server.json points at; the registry never hosts code (CONFIRMED). ( Quickstart) The .mcpb MCP Bundle format (adopted 20 November 2025) packages local servers as one-click zip archives for desktop clients (CONFIRMED via official blog snippet). GitHub Marketplace itself remains for Apps/Actions, not skills or MCP servers; GitHub-side discovery happens through the GitHub MCP Registry, stars and trending (INFERRED from ecosystem reporting).

Which are mirrors? Auto-import from the official registry is now the norm: submitmcp.com states flatly that "Glama, PulseMCP, mcp.directory and GitHub copy from it"; mcp.directory's own submit page says entries are "added via auto-discovery from the official MCP Registry"; Linkly found itself listed on Glama, mcp.directory and PulseMCP "without doing anything" after one registry publish; and an audit found 17,320 raw registry rows deduplicating to 11,447 unique servers, i.e. directories republish each other heavily (CONFIRMED). ( SubmitMCP,  mcp.directory/submit,  Linkly,  AwesomeAgents audit)

2. Business Models of Each Class

Paid listing / fast-track fees (the dominant indie model). mcp.so sells a $39 one-time "publish immediately without review" tier with verified badge, featured placement and a dofollow link, marketed on an "Advertise" page boasting DR 74, 2.2M unique visitors/12mo and 144k backlinks (CONFIRMED). ( mcp.so/submit) mcpservers.org sells the same $39 package (24h review, badge, priority search, dofollow) (CONFIRMED). ( MentionAgent audit) MCP Market charges $29 for 24-hour listing with an "Official" badge versus a free 4–6 week queue, and $69 for remote servers (CONFIRMED). ( MCP Market) These are SEO link-and-placement businesses; the listing itself, not curation, is the product (INFERRED).

Hosting and proxying (pivoting away). Glama's model is freemium hosting: free directory browse and local install, paid one-click deployment of any indexed server in an isolated Firecracker VM with persistent state, plus a gateway for auth and logs; its homepage reports 50,000+ developers and 1M+ tool calls/month (CONFIRMED). ( Glama hosting blog,  Glama) Smithery built the same proxy/managed-identity business and shut it down: Arcade acquired Smithery on 5 August 2026 (terms undisclosed; co-founder Anirudh Kamath joined Arcade), and on 31 October 2026 Smithery's connection management, API, accounts and CLI died, with paid subscribers refunded back to 5 August; the site now pitches Arcade for out-of-the-box connectors (CONFIRMED). ( Smithery future page,  RuntimeWire) Arcade had announced a $60M Series A in June 2026 led by SYN Ventures with strategic investment from Morgan Stanley and Wipro, bringing reported total funding to $72M (REPORTED). ( Dealroom,  Financial News 247)

Media/newsletter. PulseMCP (founded by Tadas Antanavicius, an MCP Steering Committee member, and Mike Coughlin) runs a free directory plus "The Agentic Loop" weekly newsletter and a REST API; its revenue model is not public (CONFIRMED for facts, UNKNOWN for revenue). ( MCPize guide)

Ecosystem plays (monetize the platform, not the listing). The official registry is free and open source (CONFIRMED). ( MCP blog) Anthropic's and OpenAI's directories are free to list but require a paid plan (Anthropic) or verified identity (OpenAI), monetizing the assistants themselves (CONFIRMED). ( Claude blog,  SubmitMCP) Vercel's skills.sh has no paid tiers and no published business model; it is an ecosystem play with anonymous telemetry, routine security audits and a Snyk scanning partnership (CONFIRMED). ( Ry Walker research,  skills.sh docs)

Revenue share. MCPize advertises an 80% revenue share for developers who let it host and meter their MCP servers, the clearest commission model found (CONFIRMED). ( MCPize)

Enterprise private registries. The MCP project explicitly designed for private sub-registries inside enterprises (CONFIRMED); ToolHive (Stacklok) sells the governance layer: its registry server aggregates MCP servers, skills and plugins from Git, Kubernetes, upstream registries and internal APIs into governed catalogs with policy, provenance and signing (CONFIRMED). ( MCP blog,  ToolHive)

Recent pivots/shutdowns. Smithery (proxy dead 31 Oct 2026, absorbed by Arcade) and PulseMCP (manual submissions and listing edits paused since roughly mid-August 2026 while it "reworks how we ingest and manage listings", with official-registry import planned on reopen) are the two big 2026 disruptions (CONFIRMED). ( Smithery,  PulseMCP,  BestMCPMarketplace)

VC funding scorecard. Arcade: $60M Series A, ~$72M total (SYN Ventures, Morgan Stanley, Wipro) (REPORTED). Glama, PulseMCP, mcp.so, MCP Market, mcpservers.org: funding not public (UNKNOWN/not public).

3. Real Traffic and Impact

The concentration law. skills.sh's own report (25 Sep 2026): 1 million skills, ~280 million installs in seven months, but 375 listings (0.04% of the registry) generated 62% of all installs, the top 1.2% captured 94%, and nearly half of all listed skills were installed exactly once (CONFIRMED). ( Vercel, State of agent skills,  Superpower Daily) In plain terms: a listing without external momentum (HN, X, a newsletter, a leaderboard climb) is invisible (INFERRED).

Directory traffic is small. The only third-party estimate found (DataForSEO US search visits, 27 Oct 2026): the biggest directory, MCP Market, gets ~81,100 visits/month from search, and only 3 of 21 directories exceed 10,000 (CONFIRMED). ( SubmitMCP) Compare self-claims: MCP Market says "1M+ monthly visitors" and mcp.so's sales page says "2.2M unique visitors (12 mo)": both are self-reported marketing numbers on their own pricing pages, versus a search-only third-party methodology (CONFIRMED conflict, treat self-reports skeptically). ( MCP Market,  mcp.so/submit)

Field experiments. MentionAgent submitted a real server to 30 sites on 13 Sep 2026: 22 loaded, and their server ended up listed on only 3 (official registry, Glama, LobeHub), while being "not found on 10" where search worked, including Smithery, mcp.so, PulseMCP and three GitHub lists (CONFIRMED). ( MentionAgent) Linkly's submission round found "one submission that quietly went nowhere for six weeks, and one that wanted write access to all of our GitHub repos in exchange for a listing with eleven installs" (CONFIRMED). ( Linkly)

Spam, staleness, dead listings. An audit of 11,447 unique MCP server repos across four registries found 32.8% with no commit in 180+ days, 405 "zombie" servers (50+ stars and stale), and only 32.6% of the top 100 offering a zero-config MCP handshake; registries "lift anything with mcp in the name" (CONFIRMED). ( AwesomeAgents audit) A merged community list of Claude skills found 94 entries pointing at repos or folders that no longer exist (REPORTED). ( awesomeclaude.ai) So the long tail of every mega-directory is dead weight, which is exactly why curated venues (Claude directory, Cursor, GitHub registry, awesome-lists) carry disproportionate trust (INFERRED).

How catalogs rank. Official registry: no ranking, self-published metadata with namespace authentication specifically to prevent spam (CONFIRMED). ( Composio) GitHub MCP Registry: stars and community activity (CONFIRMED). ( GitHub Blog) Glama: automated scans scoring security, compatibility and ease of use (CONFIRMED). ( Glama) PulseMCP: a composite "Pulse Score" of stars, install volume and update frequency (CONFIRMED as described by Skiln). ( Skiln) skills.sh: anonymous install telemetry only, which is gameable per independent researchers who note Trail of Bits bypassed its malicious-skill detector (CONFIRMED). ( skills.sh docs,  Ry Walker)

4. How to Get In (Doors, Requirements, Review Times)

Official MCP Registry. Write a server.json (schema 2025-12-11; three mandatory fields: name, version, description), verify your namespace (GitHub sign-in for io.github.*, DNS TXT for a domain, npm/PyPI/OCI ownership for package-backed servers), run mcp-publisher publish; no review, free, remote-only servers supported with a remotes block (CONFIRMED). ( Quickstart)

GitHub MCP Registry. Publish to the official registry first, then request onboarding in the maintainers' discussion thread; curation is manual and slow (~65 servers as of Jan 2026) (CONFIRMED for the requirement, REPORTED for the discussion route). ( Descope,  GitHub discussion #1257)

Claude directory. Submit via the developer portal claude.ai/directory/manage (open since 25 Sep 2026) as either a plugin bundle (GitHub repo holding skills, commands, hooks, agents, and optionally a local MCP server) or a single remote MCP connector (URL). Requirements: paid Claude plan (Pro/Max/Team/Enterprise), connected GitHub account with push rights, public repo at launch, claude plugin validate, privacy policy, per-tool annotations (readOnlyHint/destructiveHint), a reviewer test account; review is an automated safety scan plus a human check with no fixed SLA; updates ship from Git merges without resubmission; you get installs-by-surface, views and search-term analytics. Pull requests to claude-plugins-official are explicitly not monitored, and the old marketplace.json third-party flow is being phased out (CONFIRMED). ( Anthropic docs,  Anthropic blog,  claude-plugins-official,  AI Codex guide,  Linkly)

OpenAI (ChatGPT + Codex, one shared directory). Deploy at a stable public HTTPS endpoint, verify publisher identity, hold Apps Management write access, then a "With MCP" submission with tool safety annotations and reproducible tests; human review wants 5 positive and 3 negative test cases, a video walkthrough and a test account; free (CONFIRMED). ( OpenAI,  Flowlines,  SubmitMCP)

Cursor Marketplace. plugin.json + mcp.json in a public Git repo, submit the link; hand-picked (mostly companies), plugin must be free and MIT/BSD/Apache licensed, expect weeks with no confirmation or status page; community items go to cursor.directory (CONFIRMED). ( SubmitMCP,  Cursor)

Cline. Open a GitHub issue with repo URL, a 400x400 PNG logo and a justification; there is an approval process (CONFIRMED). ( cline/mcp-marketplace)

Docker MCP Catalog. Pull request to docker/mcp-registry; on approval Docker builds, signs and publishes your image to mcp/your-server-name on Docker Hub and the catalog entry appears within ~24 hours (CONFIRMED). ( docker/mcp-registry)

Glama / mcp.directory / PulseMCP. Publish to the official registry and wait for auto-import (Glama and mcp.directory confirmed to copy; PulseMCP will when it reopens), then claim your listing (Glama via a well-known file, mcp.directory via [email hidden]) (CONFIRMED). ( SubmitMCP,  mcp.directory/submit,  MentionAgent)

mcp.so / mcpservers.org / MCP Market. Web forms. mcp.so: free review exists behind sign-in, but the anonymous remote-server path exposes only the $39 pay control (measured 15 Oct 2026); mcpservers.org: free queue up to ~2 weeks or $39 for 24h; MCP Market: free 4–6 weeks or $29 in 24h (CONFIRMED). ( mcpso issue #5418,  MentionAgent,  MCP Market)

skills.sh. Nothing to submit: publish SKILL.md folders in any public repo and installs are counted automatically via the CLI; add the skills.sh/b/owner/repo badge to your README (CONFIRMED). ( skills.sh docs)

Awesome-lists. One PR per list, one line in the stated format, alphabetical, correct category; wong2's list is the cautionary tale: it added an auto-close-PR action and now redirects to its website (CONFIRMED). ( SubmitMCP instructions,  wong2)

Anti-automation posture. Nobody posts a formal "no bots" rule, but the gates are structural: namespace verification (registry), identity verification + test accounts (OpenAI), paid plan + human review (Anthropic), hand-picked curation (Cursor, GitHub registry), PR friction (Docker, awesome-lists), and paywalls (mcp.so). SubmitMCP's own agent instructions order the agent to "ask before creating accounts, paying for anything, or getting past a captcha", evidence the ecosystem treats unattended bulk submission as spam (CONFIRMED). ( SubmitMCP)

5. Implications for a Free Open-Source Giver (tonydzi, ~55 repos, 232 skills)

Skills vs tools: different pipelines. Skills (SKILL.md) have exactly one high-volume venue that matters, skills.sh, and it requires no submission: what it requires is per-skill repos (or clean per-skill paths) that npx skills add owner/repo can install, plus README badges (CONFIRMED mechanics). MCP servers have a two-step pipeline: official registry first (which auto-populates Glama, mcp.directory, PulseMCP-on-reopen, and is the prerequisite for GitHub's registry), then the client-embedded catalogs (CONFIRMED). Your sqlite-graph-memory is already on Glama and indexed by ClaudePluginHub, so the auto-import fan-out is already working for you (CONFIRMED). ( Glama listing,  ClaudePluginHub)

Bundle vs per-skill repos. The data says do not bet on one 232-skill bundle: skills.sh ranks per repo/skill and its demand is hyper-concentrated (375 skills = 62% of installs), Anthropic's portal treats each plugin folder as its own submission (a repo can hold several plugins, but each gets separate review and analytics), and curated venues reject bulk. Split the bundle into 10–20 flagship skills with individual polish, keep join-the-fleet as the monorepo for long-tail discovery, and never bulk-submit all 232 anywhere (CONFIRMED mechanics, INFERRED strategy). ( Vercel,  Developers Digest)

Should you run your own Claude Code plugin marketplace? The old third-party marketplace.json flow is being phased out in favor of the centralized directory, so building a marketplace is now low-value; submit plugins to the portal instead, and use the directory's usage analytics (installs by surface, views, search terms) as your measurement instrument (CONFIRMED). ( claude-plugins-official,  Claude blog)

What to measure, in order of honesty. (1) Installs/first successful use: skills.sh install counts, Claude directory Usage tab, pip download stats for sqlite-graph-memory; installs show "what gets picked, not whether it works" per Vercel itself, so add a --check/quickstart command that proves first-use success and watch GitHub traffic insights for clone spikes (CONFIRMED for the instruments, INFERRED for the funnel). (2) Listing views and search terms inside the Claude directory (CONFIRMED available). (3) Stars: vanity, but they gate real venues (GitHub registry ranking, subinium's 1,000-star list) (CONFIRMED). (4) Directory referral traffic: add UTM-tagged links in READMEs so any "Try Now"-style listing clicks are attributable (INFERRED best practice).

Mistakes that get you delisted or flagged. Bulk/automated submissions (structural paywalls and human reviews exist precisely for this), missing LICENSE (an explicit requirement at Anthropic, Docker, GitHub registry, awesome-lists), missing privacy policy (automatic rejection at Anthropic), no reviewer test account (OpenAI/Anthropic human review stalls), dead links (94 dead entries found in one merged list; 32.8% of registry servers are stale, so freshness is a differentiator), and gaming skills.sh install counts (telemetry is known to be gameable and third-party security scanners are watching the registry) (CONFIRMED). ( Linkly,  AwesomeAgents,  skills.sh docs)

Recommendations for a Free Open-Source Distributor: Ranked Venue List

Reach per hour of effort, for a zero-budget giver, as of 17 November 2026:

Rank
Venue
Artifact
Why
Effort
1
 Official MCP Registry
Every MCP server/tool
One publish fans out to Glama, mcp.directory, PulseMCP (on reopen) and is the gate for GitHub's registry; free, no review
Low (CLI, one-time per server)
2
 Claude directory portal
5–10 flagship skills as plugin bundles + sqlite-graph-memory (Claude Code plugin)
The only catalog with a first-party install funnel into Claude.ai, Cowork and Claude Code, plus real usage analytics; needs one paid plan between the two of you
Medium (packaging, validation, human review)
3
 skills.sh
All skills, as installable SKILL.md repos
280M installs of attention; automatic, free; per-skill READMEs + badges + one HN/X push per flagship skill
Low (structure repos well)
4
GitHub awesome-list PRs:  punkpeye/awesome-mcp-servers,  ComposioHQ/awesome-claude-skills,  hesreallyhim/awesome-claude-code,  ccplugins
Best servers + skills
Free, durable, high-trust, star-backed reach into every AI-coding audience
Low (one line per list)
5
 Cline MCP Marketplace
MCP servers
One GitHub issue with a logo; embedded install UI in a large agent user base
Low
6
 Docker MCP Registry
sqlite-graph-memory and other servers
Docker builds, signs and hosts the image for you; enterprise-grade distribution for free
Medium (PR + image build)
7
 mcp.directory +  mcpservers.org
MCP servers
Free queues, fast (24h–2wk), one-click install configs; also claim your auto-imported Glama listing
Low
8
 agenticskills.io +  GitHub MCP Registry request
Flagship skills; best servers
Curated, selective, cheap to try; GitHub registry feeds the VS Code gallery
Low–medium
9
 OpenAI plugin directory
A remote-hosted flagship + skills
Huge user base, but requires a stable public HTTPS endpoint, identity verification and test cases; only worth it once you host something remote
High
10
Paid fast-lanes ( mcp.so $39,  MCP Market $29)
Only after evidence
Traffic is uncertain (81k/mo search visits at best, self-claims inflated); a dofollow link on a DR-74 page has SEO value, but for a free giver this is the last resort, not the default
Cash

Skip for now: PulseMCP (paused), Smithery submissions (post-acquisition process unclear), thin skill-scrapers (skillsmp, aiskill.market et al.; they index GitHub anyway), and any venue that demands repo write access or captcha for a listing.

The three routines you asked for. (1) Venue discovery: re-check this landscape quarterly; the Smithery and PulseMCP events show half-lives under six months; watch the official registry's partners, Vercel's skills changelog, and new client marketplaces. (2) Packaging: a standard repo kit per artifact, meaning LICENSE, README with homepage link first, logo, server.json for servers, per-skill SKILL.md frontmatter, npx skills add compatibility, privacy policy and a test account ready for reviewers. (3) Distribution: registry publish, then claims/forms, then curated venues, then announce once per artifact on HN/X/Reddit to feed the telemetry that leaderboards actually rank on (INFERRED synthesis of all confirmed mechanics above).

Source Notes
Source
Credibility
Last updated
 MCP Registry announcement (official blog)
5/5
8 Sep 2025
 MCP Registry publish quickstart (official docs)
5/5
checked 17 Nov 2026
 GitHub Blog: Meet the GitHub MCP Registry
5/5
16 Sep 2025
 Smithery: The future of Smithery
5/5
31 Oct 2026
 RuntimeWire: Arcade acquires Smithery
4/5
11 Aug 2026
 Dealroom: Arcade/Smithery deal note
3/5
Aug 2026
 Claude docs: Submit your plugin
5/5
checked 17 Nov 2026
 Anthropic blog: Build plugins for Claude
5/5
25 Sep 2026
 anthropics/claude-plugins-official (GitHub)
5/5
checked 17 Nov 2026
 AI Codex: Claude directory submission guide
4/5
25 Sep 2026
 Developers Digest: Claude portal guide
4/5
17 Oct 2026
 OpenAI: Upload and submit your plugin
5/5
checked 17 Nov 2026
 Flowlines: OpenAI plugin directory guide
4/5
Sep 2026
 Vercel: State of agent skills
5/5
25 Sep 2026
 Vercel changelog: introducing skills
5/5
20 Jan 2026
 skills.sh docs
5/5
checked 17 Nov 2026
 Superpower Daily on Vercel's report
3/5
25 Sep 2026
 Ry Walker: skills.sh research
4/5
11 Jun 2026
 mcp.so/submit
3/5 (self-interested sales page)
checked 17 Nov 2026
 chatmcp/mcpso issue #5418
4/5 (primary measurement)
15 Oct 2026
 Glama homepage
4/5 (self-reported counts)
17 Nov 2026
 Glama hosting blog
4/5
10 Sep 2025
 PulseMCP homepage
4/5 (blocked to crawler; via search snippet)
checked Nov 2026
 MCPize: PulseMCP guide
3/5
2026
 BestMCPMarketplace: directories that changed
3/5
17 Nov 2026
 MCP Market submit page
3/5 (self-interested)
checked 17 Nov 2026
 mcp.directory/submit
4/5
checked 17 Nov 2026
 mcpservers.org
3/5
checked Nov 2026
 wong2/awesome-mcp-servers (GitHub)
5/5
13 Jul 2026
 punkpeye/awesome-mcp-servers (GitHub)
5/5
checked 17 Nov 2026
 hesreallyhim/awesome-claude-code (GitHub)
5/5
17 Nov 2026
 ComposioHQ/awesome-claude-skills (GitHub)
4/5
checked 17 Nov 2026
 ccplugins/awesome-claude-code-plugins (GitHub)
4/5
checked Nov 2026
 cline/mcp-marketplace (GitHub)
5/5
checked 17 Nov 2026
 docker/mcp-registry (GitHub)
5/5
10 Nov 2026
 Cursor Marketplace
4/5
checked 17 Nov 2026
 AgenticSkills submit
3/5
17 Nov 2026
 SubmitMCP directory tracker
4/5 (independent, methodology stated)
27 Oct 2026
 MentionAgent directory audit
4/5 (first-hand experiment)
13 Sep 2026
 Linkly: where to list your MCP server
4/5 (first-hand experiment)
17 Aug 2026
 VentureList: registries and directories guide
3/5
2 Oct 2026
 AwesomeAgents: MCP marketplace audit
4/5 (methodology documented)
5 Oct 2026
 Top MCPs: directories compared
3/5 (competitor content)
27 May 2026
 Skiln: best MCP directories 2026
3/5 (competitor content)
4 Jun 2026
 Descope: best MCP directories
3/5
2026
 Composio: GitHub MCP Registry explained
4/5
30 Oct 2026
 ToolHive (Stacklok, GitHub)
5/5
checked Nov 2026
 Glama listing of tonydzi/sqlite-graph-memory
4/5
checked 17 Nov 2026
 ClaudePluginHub: sqlite-graph-memory plugin
3/5
checked 17 Nov 2026

Conflicts and caveats: Directory server counts conflict wildly (PulseMCP reported as ~7k, ~13k, ~16k and ~21.5k by different sources in 2026) because of deduplication and scraping differences; treat all counts as order-of-magnitude. MCP Market's "1M+ monthly visitors" and mcp.so's "2.2M unique visitors/12mo" are self-reported marketing claims that conflict with the only independent search-traffic estimate found (~81k/month for the biggest site). Arcade's funding figures come from trade press snippets (Dealroom, Financial News 247, Forbes headline), not from an opened primary filing. Smithery's post-acquisition intake process for new listings is undocumented. Vercel's install figures are registry counters, not unique users or proof of usefulness (Vercel says this itself).

Open Questions
Smithery's new-listing door: after the 31 Oct 2026 proxy shutdown, whether Arcade will accept community submissions, and on what terms, is not public (UNKNOWN).
PulseMCP's reopen date and criteria: "as soon as it is ready" is all that is published (UNKNOWN).
Actual install attribution: no directory publishes per-listing referral or install data publicly; only the Claude directory (to the publisher) and skills.sh (aggregate) expose numbers. First-hard numbers for your own artifacts require your own instrumentation (UNKNOWN).
Glama's, PulseMCP's and mcp.so's funding and revenue: not public.
Whether Cursor expands its hand-picked marketplace to community plugins: unknown; the docs still redirect community items to cursor.directory.

Method note: 38 search queries and ~45 distinct sources were inspected for this report; every load-bearing claim carries a link to a page actually opened, and the handful of snippet-level claims (Arcade funding figures, the GitHub discussion route, the .mcpb adoption) are marked REPORTED or noted as coming from search snippets above.

Sources
7:46pm
Knowledge automatically saves key insights from your chats
Set up now




Think
Vibe can make mistakes. Check answers. Learn more
AI-Agent Catalog Marketplace Landscape

## Sources (links in chat)
- https://modelcontextprotocol.io/registry/quickstart
- https://github.com/modelcontextprotocol/registry
- https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/quickstart.mdx
- https://changegamer.ai/articles/mcp-server-registry-publishing-playbook
- https://www.stackone.com/blog/mcp-registry-how-to-get-listed/
- https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/
- https://glama.ai/mcp
- https://glama.ai/
- https://glama.ai/blog/2025-09-10-glama-mcp-server-hosting
- https://eseospace.com/blog/glama-mcp/
- https://glama.ai/mcp/servers?query=Business
- https://glama.ai/mcp/servers?query=business+data
- https://x.com/stretchcloud/status/2119840692021481955
- https://app.dealroom.co/news/feed/arcade-acquires-smithery-to-control-mcp-registry-and-runtime-layer
- https://www.forbes.com/sites/janakirammsv/2026/08/10/arcade-acquires-smithery-to-own-the-agent-tool-supply-chain/
- https://thefinancialnews247.com/arcade-acquires-smithery-to-own-the-agent-tool-supply-chain/
- https://runtimewire.com/article/arcade-acquires-smithery-agent-tool-supply-chain
- https://app.dealroom.co/news/note/arcade-acquires-smithery-to-control-the-agent-tool-supply-chain
- https://flowpast.com/n8n/pulsemcp-and-openai-pick-the-right-mcp-server-fast/
- https://skywork.ai/skypage/en/MCP-Server-Deep-Dive-The-Ultimate-Guide-to-the-PulseMCP-Server-for-AI-Engineers/1972827648458551296
- https://skiln.co/blog/best-mcp-directories-2026
- https://www.developersdigest.tech/tools/pulsemcp
- https://top-mcps.com/guides/mcp-directories-compared
- https://www.descope.com/blog/post/mcp-directories
- https://mcp.so/submit
- https://mcp.so/
- https://venturelist.app/guides/mcp-server-and-ai-agent-directories
- https://linklyhq.com/blog/mcp-server-directories
- https://mcp.directory/submit
- https://submitmcp.com/
- https://mcpservers.org/all
- https://mcpservers.org/
- https://mcpservers.org/remote-mcp-servers
- https://mcpservers.org/official
- https://mcpservers.org/category/search
- https://mcpservers.org/servers/ai-directories-mcp
- https://mcp.directory/
- https://mcp.directory/publishers
- https://mcp.developersdigest.tech/about
- https://obot.ai/resources/learning-center/mcp-catalog/
- https://github.com/cline/mcp-marketplace
- https://github.com/cline/mcp-marketplace/blob/main/README.md
- https://raw.githubusercontent.com/cline/mcp-marketplace/main/README.md
- https://www.mcpserverfinder.com/servers/cline/mcp-marketplace
- https://github.com/syntax-syndicate/cline-mcp-marketplace
- https://github.com/ahernandez-developer/mcp-marketplace
- https://claude.com/docs/plugins/submit
- https://github.com/anthropics/claude-plugins-official
- https://www.developersdigest.tech/blog/build-plugins-for-claude
- https://www.aicodex.to/articles/submit-plugin-to-claude-directory
- https://claude.com/blog/build-plugins-for-claude
- https://kevinwelter.com/en/blog/claude-code-plugins
- https://vercel.com/changelog/introducing-skills-the-open-agent-skills-ecosystem
- https://www.skills.sh/
- https://github.com/vercel-labs/skills
- https://rywalker.com/research/skills-sh
- https://vercel.com/blog/state-of-agent-skills
- https://www.infoq.com/news/2026/02/vercel-agent-skills/
- https://agenticskills.io/submit
- https://agentskills.io/integrate-skills
- https://agenticskills.io/skills
- https://aiskill.market/skills/agentskills-io
- https://strapi.io/blog/what-are-agent-skills-and-how-to-use-them
- https://github.com/agentskills
- https://github.com/openai/skills/blob/main/skills/.curated/chatgpt-apps/SKILL.md
- https://developers.openai.com/plugins/deploy/submission
- https://flowlines.ai/blog/how-to-get-listed-openai-plugin-directory
- https://www.mcpjam.com/blog/publish-chatgpt-codex-plugin-directory
- https://developers.openai.com/plugins
- https://developers.openai.com/codex/use-cases/chatgpt-apps/
- https://mcp.directory/blog/cursor-team-mcp-marketplace-2026
- https://cursor.com/changelog/team-marketplace-updates
- https://www.apideck.com/blog/mcp-server-directories
- https://baeseokjae.github.io/posts/cursor-mcp-v2-1-setup-guide-2026/
- https://cursor.com/docs/mcp
- https://cursor.com/marketplace
- https://marketplace.visualstudio.com/items?itemName=AmitPatole.mcp-marketplace
- https://marketplace.visualstudio.com/items?itemName=vscode-mcp.vscode-mcp
- https://marketplace.visualstudio.com/items?itemName=AutomataLabs.copilot-mcp
- https://marketplace.visualstudio.com/items?itemName=autohome.mcp-store
- https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azure-mcp-server
- https://marketplace.visualstudio.com/items?itemName=SemanticWorkbenchTeam.mcp-server-vscode
- https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog/
- https://docs.docker.com/reference/cli/docker/mcp/catalog/server/add/
- https://docs.docker.com/ai/mcp-catalog-and-toolkit/get-started/
- https://docs.docker.com/reference/cli/docker/mcp/catalog/catalog_add/
- https://github.com/docker/mcp-registry
- https://www.docker.com/blog/build-custom-mcp-catalog/
- https://github.com/github/github-mcp-server/discussions/1257
- https://composio.dev/content/github-mcp-registry
- https://devactivity.com/insights/streamlining-software-engineering-efficiency-navigating-githubs-curated-mcp-registry/
- https://github.com/mcp
- https://github.blog/ai-and-ml/github-copilot/meet-the-github-mcp-registry-the-fastest-way-to-discover-mcp-servers/
- https://github.blog/ai-and-ml/generative-ai/how-to-find-install-and-manage-mcp-servers-with-the-github-mcp-registry/
- https://github.com/ComposioHQ/awesome-claude-skills
- https://github.com/BehiSecc/awesome-claude-skills
- https://github.com/travisvn/awesome-claude-skills
- https://github.com/karanb192/awesome-claude-skills
- https://designrevision.com/blog/awesome-claude-code-skills
- https://awesomeclaude.ai/awesome-claude-skills
- https://github.com/punkpeye/awesome-mcp-servers
- https://github.com/gigamori/punkpeye-awesome-mcp-servers
- https://www.mcpserverfinder.com/servers/punkpeye/awesome-mcp-servers
- https://github.com/punkpeye/awesome-mcp-clients
- https://github.com/punkpeye/awesome-remote-mcp-servers
- https://github.com/punkpeye/awesome-mcp-servers/activity
- https://www.smithery.ai/future
- https://o11.ai/blog/smithery-connect-migration
- https://kapyn.app/blog/smithery-shutdown-mcp-migration
- https://www.smithery.ai/
- https://aiagentstore.ai/ai-agent/smithery
- https://www.stork.ai/blog/smithery-shutdown-mcp-server-urls
- https://www.pulsemcp.com/
- https://www.pulsemcp.com/servers
- https://bestmcpmarketplace.com/directories-that-changed/
- https://innvesti.com/reports/mcp-server-cards-roadmap-discovery-tools-2026/
- https://blastra.io/directories/pulsemcp/
- https://github.com/Deke1Yahav/agents-hive/issues/39
- https://mentionagent.ai/blog/mcp-server-directories/
- https://submitmap.com/lists/mcp-server-directories/
- https://github.com/chatmcp/mcpso/issues/5418
- https://glama.ai/mcp/connectors/com.recentfunding/mcp
- https://glama.ai/mcp/connectors/fund.f4/raise
- https://glama.ai/mcp/connectors/com.meridianlabssoftware/sec-form-d-funding-rounds
- https://glama.ai/mcp/servers/mambalabsdev/mcp-recent-funding-rounds-feed
- https://glama.ai/mcp/connectors/com.modevc/stage-series-a
- https://glama.ai/mcp/connectors/io.github.aventurevc/aventure-mcp
- https://www.pulsemcp.com/mcp-apps
- https://www.pulsemcp.com/newsletter
- https://mcpize.com/alternatives/pulsemcp
- https://www.pulsemcp.com/posts/newsletter-anthropic-raise-ai-usage-mcp-clients
- https://newsletter-choice.com/revenue-model-examples/
- https://contextbolt.com/blog/similarweb-mcp/
- https://developers.similarweb.com/docs/similarweb-mcp
- https://apis.io/providers/similarweb/
- https://developers.similarweb.com/docs/claude-mcp-integration
- https://docs.similarweb.com/api-v5/similarweb-mcp/mcp-setup
- https://apify.com/cliqtomedia/website-traffic-estimator/api/mcp
- https://www.infoq.com/news/2025/10/github-mcp-registry/
- https://github.com/SamoTech/mcp-atlas
- https://deepwiki.com/microsoft/mcp-for-beginners/7.3-github-mcp-registry
- https://dev.to/descope/best-mcp-server-directories-for-developers-502e
- https://clelp.ai/blog/73000-mcp-servers-nobody-knows-which-work
- https://awesomeagents.ai/news/mcp-marketplace-audit/
- https://dev.to/anclum/i-audited-8900-mcp-servers-and-agent-skills-for-clean-links-and-licence-data-2khh
- https://directoryready.com/blog/directory-spam-prevention-best-practices-guide
- https://vibecoding.app/blog/skills-sh-review
- https://skillselion.com/leaderboard
- https://github.com/LinklyAI/best-skills
- https://tomrochette.com/agents/skills-sh/
- https://www.skills.sh/docs
- https://superpowerdaily.com/posts/vercel-finds-agent-skill-installs-concentrated-in-a-tiny-share-of-its-registry
- https://daily.dev/posts/state-of-agent-skills-gvnocahqj
- https://creatorstoolbox.com/blog/vercel-state-of-agent-skills
- https://apis.io/2026/10/08/vercel-counts-a-million-skills-and-publishes-none-for-its-own-api/
- https://www.somkiat.cc/state-of-agent-skills-from-vercel/
- https://github.blog/ai-and-ml/github-copilot/meet-the-github-mcp-registry
- https://github.com/hesreallyhim/awesome-claude-code
- https://github.com/hesreallyhim/awesome-claude-code/tree/main/assets
- https://github.com/hesreallyhim/awesome-claude-code/tree/main/tools
- https://github.com/hesreallyhim/awesome-claude-code/tree/main/resources
- https://github.com/hesreallyhim/awesome-claude-code/blob/main/README.md
- https://github.com/hesreallyhim/a-list-of-claude-code-agents
- https://github.com/ccplugins/awesome-claude-code-plugins
- https://github.com/ccplugins/awesome-claude-code-plugins/pulls
- https://github.com/subinium/awesome-claude-code
- https://github.com/rohitg00/awesome-claude-code-toolkit
- https://github.com/composio-community/awesome-claude-plugins
- https://github.com/quemsah/awesome-claude-plugins
- https://felo.ai/blog/clawhub-skills-marketplace-claude-code/
- https://clawhub.ai/skills
- https://clawhub.ai/
- https://www.gradually.ai/en/claude-marketplaces/
- https://awesomeskill.ai/skill/steipete-clawdis-clawhub
- https://aiskill.market/clawhub
- https://github.com/tonydzi/sqlite-graph-memory
- https://glama.ai/mcp/servers/tonydzi/sqlite-graph-memory
- https://www.claudepluginhub.com/plugins/tonydzi-sqlite-graph-memory
- https://github.com/tonydzi/sqlite-graph-memory/releases/tag/v0.3.0
- https://github.com/tonydzi/sqlite-graph-memory/releases/tag/v0.3.7
- https://github.com/mnemoverse/awesome-agent-memory/pull/35
- https://blog.modelcontextprotocol.io/posts/2025-11-20-adopting-mcpb/
- https://github.com/modelcontextprotocol/mcpb
- https://www.mcpbundles.com/docs/concepts/mcpb-files
- https://github.com/modelcontextprotocol/mcpb/blob/main/README.md
- https://www.mcpbundles.com/blog/2025/10/05/what-is-mcpb-file
- https://data-wise.github.io/himalaya-mcp/reference/mcpb-format-reference/
- https://github.com/stacklok/toolhive
- https://github.com/stacklok/toolhive-registry-server
- https://docs.stacklok.com/toolhive/
- https://aicybr.com/blog/toolhive-mcp-server-security-deployment-guide
- https://dev.to/kuldeep_paul/top-5-mcp-registry-tools-for-enterprise-ai-governance-2026-52ap
- https://stacklok.com/blog/mcp-server-governance-starts-with-discovery/
- https://news.ycombinator.com/item?id <tg-id>
- https://news.ycombinator.com/item?id <tg-id>
- https://dev.to/neopotato/the-mcp-server-crisis-how-open-standard-created-a-wild-west-of-broken-implementations-115n
- https://www.reddit.com/r/mcp/comments/1ry3xv7/we_graded_over_200000_mcp_servers_both_stdio/
- https://dev.to/0coceo/mcp-won-mcp-might-also-be-dead-4a8a
- https://growthmethod.com/is-mcp-dead/
- https://www.sean-weldon.com/blog/2026-10-03-mcp-doesnt-suck-your-agent-does-jan-%C4%8Durn-apify
- https://news.ycombinator.com/item?id <tg-id>
- https://agentconn.com/blog/agent-skills-marketplace-land-grab-2026/
- https://github.com/latestaiagents/agent-skills
- https://github.com/dukelyuu/skills-marketplace
- https://awesomeskill.ai/
- https://skillsmp.com/
- https://chris-ayers.com/posts/agent-skills-plugins-marketplace/
- https://www.youtube.com/watch?v=jI9NTVX68T0
- https://github.com/pulsemcp/mcp-servers
- https://www.reddit.com/r/ClaudeAI/comments/1iimej5/newly_launched_witness_the_power_of_mcps_with/
- https://dev.to/descope/best-mcp-server-directories
- https://www.reddit.com/r/mcp/comments/1ry3xv7/
- https://superpowerdaily.com/posts/vercel-finds-agent-skill-installs-concentrated
- https://descope.com/blog/post/mcp-directories
- https://submitmcp.com/llms.txt
- https://www.mcpserverfinder.com/
- https://github.com/wong2/awesome-mcp-servers
- https://mcpmarket.com/submit
- https://mcpservers.org/submit
- https://claude.ai/directory/manage