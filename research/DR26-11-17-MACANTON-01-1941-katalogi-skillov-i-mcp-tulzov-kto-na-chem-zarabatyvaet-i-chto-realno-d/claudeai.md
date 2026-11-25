# DR26-11-17-MACANTON-01-1941 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# AI-Agent Skill & Tool Catalogs, November 2026: What Brings Users, What Is Pay-to-Play, and How Two People Should Distribute 55 Repos and 232 Skills

For a free open-source distributor, almost all of the reachable value now sits in a few "upstream" doors: the official MCP Registry for servers, GitHub-native skill installs (skills.sh telemetry plus your own Claude Code plugin marketplace) for skills, and one or two first-party app directories (Anthropic, OpenAI). Most of the dozens of web directories are SEO-driven, increasingly pay-to-skip-the-queue, and have no published evidence that a listing drives installs.

## TL;DR
- **Publish once upstream, let mirrors copy you.** The official MCP Registry (free, CLI, no human review) is now the feed for PulseMCP (when it reopens), mcp.directory, Glama and, after a manual onboarding, GitHub's MCP Registry. For skills, skills.sh has no submission at all: it ranks by `npx skills add` install telemetry, and a `.claude-plugin/marketplace.json` in your own repo publishes a Claude Code marketplace with no form.
- **Both pivots in your brief are real, with nuances.** Smithery was acquired by Arcade.dev on August 5, 2026 and shut down its proxy, managed identity, accounts, API, CLI and SDKs on October 31, 2026; the directory stays as a non-proxied registry. PulseMCP has paused new submissions *and* listing changes "while we rework how we ingest and manage listings", and says it will import from the official registry when it reopens. The mcp.so claim is also confirmed: its submit page offers a $39 one-time paid listing (instant publish, verified badge, featured placement, dofollow link).
- **Nobody has published hard evidence that a directory listing brings meaningful installs.** The only first-hand data we found are Measure.events' self-reported 2x docs traffic after an awesome-mcp-servers listing, and lichris's DEV Community post "Six Directories, Three Weeks of Checks", which found "zero visible discovery events we can attribute" across six directories. Treat directories as cheap SEO/backlinks, not a user channel, and measure installs at the source (npm/PyPI downloads, skills.sh counts, GitHub referrers).

## Executive summary
1. **Taxonomy has consolidated into four layers:** (a) protocol-level registries (official MCP Registry), (b) first-party in-app directories (Claude directory, ChatGPT/Codex plugin directory, Cursor Marketplace, GitHub MCP Registry/VS Code, Docker MCP Catalog, Gemini CLI extensions), (c) third-party web directories (mcp.so, mcpservers.org, MCP Market, Glama, Smithery, PulseMCP, LobeHub, mcp.directory, dozens of long-tail sites), and (d) GitHub awesome-lists. Skills have their own track: the agentskills.io SKILL.md spec, skills.sh, and plugin bundles in Claude/OpenAI/Cursor.
2. **Mirrors/aggregators (CONFIRMED or self-described):** PulseMCP, mcp.directory, Glama and GitHub's MCP Registry ingest from the official registry; Glama also auto-indexes public GitHub repos; skills.sh auto-lists from install telemetry; Gemini CLI's gallery crawls GitHub daily for a topic.
3. **Business models split three ways:** paid fast lanes/badges (mcp.so $39, mcpservers.org $39, MCP Market $29/$69), hosting/gateway SaaS that subsidises a free directory (Glama), and strategic land-grabs by infrastructure companies (Arcade buying Smithery; Vercel running skills.sh; Anthropic/OpenAI/GitHub running first-party directories to own distribution and enterprise governance).
4. **Smithery's managed-connection business ended** on October 31, 2026, with refunds of every charge since August 5; its CLI-publish route is gone.
5. **PulseMCP is frozen** (no new listings, no edits); dates in secondary sources range from August to September 2026 for when the pause began.
6. **Traffic is concentrated and mostly unknown.** One October 2026 SEO estimate (US Google search only) put MCP Market at ~81k visits/month, mcpservers.org ~45k, mcp.so ~18k, and most others in the hundreds or tens. mcp.so self-reports 2.2M unique visitors over 12 months. No catalog publishes referral or install data per listing.
7. **Overcrowding is extreme:** Glama lists ~113k servers, the official registry ~53.5k entries, PulseMCP ~21.6k, and awesome-mcp-servers carries thousands of open PRs. A single listing in a flat list is effectively invisible without ranking signals (stars, installs, quality score).
8. **Anti-automation rules are real and enforced:** awesome-claude-code requires a human to use the web issue form, one resource at a time, and threatens temporary restriction for violations; awesome-mcp-servers instead *invites* agents to self-label PRs with 🤖🤖🤖, but a CI bot blocks entries without a Glama score badge.
9. **Claude's channel changed in September 2026:** third-party plugins now go only through the developer portal at claude.ai/directory/manage (paid plan required); PRs to `claude-plugins-official` are not monitored and its marketplace.json is being cut back to Anthropic's own plugins.
10. **For you:** registry-publish every server from CI, claim them on Glama, ship the 232 skills as a handful of themed Claude Code plugins inside one self-hosted marketplace repo that also works with `npx skills add`, submit one curated bundle to the Anthropic and OpenAI directories, and stop paying or bulk-submitting to long-tail directories.

## Master table

Status key: ALIVE / PAUSED / CHANGED / CLOSED. Evidence key: **C** = CONFIRMED from the catalog's own page or docs; **I** = INFERRED or from a secondary source; **U** = UNKNOWN / not public.

| Catalog | Class | Accepts | Business model | Entry door | Requirements | Evidence of traffic | Status |
|---|---|---|---|---|---|---|---|
| Official MCP Registry (registry.modelcontextprotocol.io) | Protocol registry (metadata only) | MCP servers (local packages + remotes) | None; community/open-source project (C) | `mcp-publisher publish` CLI, incl. GitHub OIDC in CI (C) | `server.json`; namespace `io.github.<user>/` via GitHub login or reverse-DNS via domain proof; package marker (`mcpName` in package.json, `mcp-name:` line in PyPI README) (C) | ~53,541 entries (I, Oct 31 2026); traffic not public (U) | ALIVE; no manual review (C) |
| GitHub MCP Registry (github.com/mcp) | First-party in-IDE directory | MCP servers | Strategic: Copilot/VS Code distribution + enterprise allowlists (C) | Publish to official registry, then request onboarding in a GitHub discussion; initial onboarding is manual curation (I) | Public repo with license (I) | Sorted by stars/activity; traffic not public (U) | ALIVE, curated |
| Docker MCP Catalog | First-party client catalog | Local + remote MCP servers | Strategic (Docker Desktop MCP Toolkit, Docker Hub) (C) | PR with `server.yaml` to docker/mcp-registry (C) | Permissive license, working Docker deployment, docs, security contact; Docker team review; live ~24h after approval (C) | ~3.2k US search visits/mo (I) | ALIVE |
| Cline MCP Marketplace | Client catalog | MCP servers | Strategic | GitHub issue with repo URL + 400×400 logo (I) | Usefulness rationale (I) | Repo reportedly not updated since June 2025 (I) | Effectively stale (I) |
| Glama | Web directory + hosting/gateway | MCP servers, remote connectors | Free directory funded by AI gateway + MCP hosting (C, founder); three paid offers from $9 to $80 per month listed in glama.ai's own structured data (C); boosted listings $49/mo (I); creator revenue share "coming soon" since Dec 2025 (C, not launched as far as found) | Auto-imports GitHub + official registry; "Add Server"; claim via domain (I) | Dockerfile for checks; quality score (TDQS) and health (I) | ~113k servers (I); ~2.1k US search visits/mo (I) | ALIVE |
| Smithery (now Arcade) | Web directory | MCP servers (verified ones show direct vendor URLs) | Former hosted proxy/Connect usage plans ended; now a funnel to Arcade (C) | Unclear after Oct 31 shutdown; CLI/SDK deprecated (C) | — | ~27k servers (I); ~4.5k US search visits/mo (I) | CHANGED: directory only (C) |
| PulseMCP | Web directory + newsletter | MCP servers, clients | Not public (U) | None; will import from official registry on reopening (C) | — | ~21.6k servers (C/I); ~3.6k US search visits/mo (I) | PAUSED (C) |
| mcp.so | Web directory | Servers, remote servers, clients, AI products, CLI tools, Agent Skills, "Loops" (C) | $39 one-time paid listing; "Advertise" page (C) | Web form; paid path is "Pay and submit automatically" (C) | Repo URL + name (C) | Self-reported 2.2M unique visitors / 6M pageviews (12 mo), 266K MAU, DR 74 (C, self-reported); ~18k US search visits/mo (I) | ALIVE, pay-to-skip |
| mcpservers.org (wong2 list) | Web directory | MCP servers | Free (review ≤2 weeks) or $39 premium (24h review, Official badge, search priority) (C) | Web form only; wong2/awesome-mcp-servers "do not accept PRs" (C) | Name, category, repo/website (I) | ~45k US search visits/mo (I) | ALIVE |
| MCP Market | Web directory | MCP servers, skills (I) | Free queue 4–6 weeks or $29; remote servers $69 (I) | Web form (I) | GitHub repo URL (I) | ~81k US search visits/mo, highest measured (I) | ALIVE, pay-to-skip |
| mcp.directory | Web directory | MCP servers | Not public (U) | Paste GitHub URL; also imports from official registry (I) | Repo; ~1 day review (I) | ~220 US search visits/mo (I) | ALIVE |
| LobeHub MCP | App marketplace | MCP servers | Strategic (LobeHub chat app) (I) | Sign in and submit (I) | — | ~2.9k visits/mo (I) | ALIVE |
| Claude directory (claude.ai/directory) | First-party app directory | Plugins (skills, commands, hooks, MCP) and MCP connectors (C) | Strategic platform play; free to list (C/I) | Developer portal claude.ai/directory/manage (C) | Paid claude.ai plan; `claude plugin validate --strict`; pre-submission checklist; GitHub repo (C) | Not public (U); public reports of multi-week waits (I) | ALIVE (portal since Sep 25 2026, I) |
| Own Claude Code marketplace | Self-hosted | Plugins | Free | Commit `.claude-plugin/marketplace.json` (C) | plugin.json, permanent kebab-case name, versioning (C) | Only what you drive | ALIVE, no review |
| claude-plugins-official / -community | Anthropic repos | Anthropic plugins / read-only mirror | — | PRs not monitored / auto-closed (C) | — | ~35k stars (I) | CLOSED to third-party PRs (C) |
| ChatGPT/Codex plugin directory | First-party app directory | Plugins with skills and/or MCP; "Skills only" path (C) | Strategic | OpenAI Platform portal, ZIP upload (C) | Verified developer identity; `.codex-plugin/plugin.json` or portable `plugin.json`; skills as immediate children of `skills/` (C); human review with test cases and video (I) | Not public (U) | ALIVE |
| Cursor Marketplace | First-party | Plugins (plugin.json + mcp.json) (I) | Strategic | Submit public repo link (I) | Free, MIT/BSD/Apache, hand-picked, weeks with no status (I) | Not public (U) | ALIVE, curated |
| cursor.directory | Community | Rules, MCP, plugins (I) | Not public | GitHub/Google sign-in (I) | — | ~1.6k visits/mo (I) | ALIVE |
| skills.sh (Vercel) | Skill leaderboard | Agent Skills from any GitHub repo (C) | None direct; strategic for Vercel; "packs" (C) | No submission: listed once users run `npx skills add owner/repo` (C) | SKILL.md per agentskills.io spec | Install counts public per skill; top skill ~3.4M installs (I, Sep 2026) | ALIVE |
| agentskills.io | Spec, not a catalog | Defines SKILL.md format (C) | — | — | `name` ≤64 chars, lowercase/hyphens, matches folder; `description` ≤1024 chars (C) | — | ALIVE |
| AgenticSkills.io | Web directory | Skills + MCP (I) | Not public | Web form, ~48h review (I) | Published inclusion criteria (I) | ~2.2k visits/mo (I) | ALIVE |
| Gemini CLI Extensions | First-party gallery | Extensions (I) | Strategic | `gemini-extension.json` + `gemini-cli-extension` topic; daily crawler (I) | Public GitHub repo (I) | U | ALIVE |
| Goose Extensions | First-party | Local MCP | — | — | — | — | CLOSED since Jul 30 2026 (I) |
| punkpeye/awesome-mcp-servers | Awesome-list | Local servers with public repo (C) | Feeds Glama (owner's company) (I) | One-line PR; 🤖🤖🤖 agent opt-in (C) | Glama listing + score badge enforced by CI bot (C, bot comments) | ~97.8k stars; thousands of open PRs (C) | ALIVE, overcrowded |
| punkpeye/awesome-remote-mcp-servers | Awesome-list | Remote servers (C) | Same | PR | Glama connector badge with Healthy status (C) | ~3.3k stars (I) | ALIVE |
| hesreallyhim/awesome-claude-code | Awesome-list | Skills, plugins, tools for Claude Code (C) | None | Human-only web issue form, no PRs (C) | 14 days old + active, or ≥100 stars; one at a time (C) | ~54.8k stars (I) | ALIVE, selective |
| npm / PyPI | Package indexes | Anything | None for you | Publish | Needed for registry ownership markers (C) | Download counts public (C) | ALIVE |

## 1. Taxonomy: what exists and what each accepts

**MCP registries and directories.** The official MCP Registry is a metadata service: you push a `server.json` with `mcp-publisher`, it verifies that you own the namespace (`io.github.<user>/…` via GitHub login, or reverse-DNS via a domain), and it checks that the referenced npm/PyPI/NuGet package carries a matching ownership marker. It has no human review: once schema, namespace and ownership checks pass, the server is listed. Its REST API entered a version freeze on October 24, 2025 ahead of general availability. GitHub's MCP Registry launched on September 16, 2025 as a curated directory with one-click VS Code install and star-based sorting, promising that self-published official-registry entries would "automatically appear"; in practice, community notes in GitHub's discussion #1257 (October 2026) say initial onboarding is manual curation, after which new versions sync. GitHub also lets enterprises point Copilot/VS Code at a private registry and enforce a registry-only allowlist (public preview since November 18, 2025). Docker's catalog accepts local and remote servers through PRs to `docker/mcp-registry`.

**Web directories.** mcp.so is the broadest by type: servers, remote servers, clients, AI products, CLI tools, Agent Skills and "Loops". mcpservers.org (the site behind wong2/awesome-mcp-servers) and MCP Market take web forms. Glama auto-indexes. Smithery is now a non-proxied directory. PulseMCP is frozen. A long tail (MCP Server Hub, MCP Atlas, MCP Server Spot, MCP Pub, Claude Market, Launch MCP and others) each draw hundreds of visits a month or fewer, according to one October 2026 estimate.

**Agent-skill catalogs.** The agentskills.io spec defines a skill as a folder with `SKILL.md`, whose YAML frontmatter requires `name` (max 64 characters, lowercase and hyphens, matching the folder) and `description` (max 1,024 characters, saying what the skill does and when to use it); bodies are recommended under ~5,000 tokens. skills.sh (launched by Vercel in January 2026) is a leaderboard built from CLI install telemetry, with no submission flow. Anthropic's directory and OpenAI's unified ChatGPT/Codex plugin directory both accept skills only inside plugins (OpenAI also has a "Skills only" path that takes skill ZIPs). Cursor's Marketplace takes plugins. mcp.so, MCP Market and AgenticSkills now list skills alongside servers.

**Awesome-lists.** punkpeye/awesome-mcp-servers (~97.8k stars) and its remote sibling, wong2/awesome-mcp-servers (form-only via mcpservers.org), hesreallyhim/awesome-claude-code (~54.8k stars), plus ranked lists such as best-of-mcp-servers (weekly automated quality score) and many small forks.

**Which are mirrors/aggregators (CONFIRMED unless noted):** PulseMCP (will ingest from the official registry), mcp.directory (imports from the registry), Glama (imports from the registry and GitHub), GitHub MCP Registry (syncs from the official registry after onboarding), skills.sh (auto-lists from telemetry), Gemini CLI gallery (crawls GitHub, I). Some guides also claim mcpservers.org mirrors the registry (I; its own page only shows a form).

## 2. Business models

**Paid fast lanes and badges (CONFIRMED on the catalogs' own pages).** mcp.so: "$39 one-time publishing fee", instant publish without review, verified badge, featured/priority placement, dofollow link; it also runs an "Advertise" page and sells the listing on its domain authority and traffic. Its page metadata says you can "choose free review or publish immediately with Premium", but the form we saw only shows the paid path; third-party guides conflict on whether a free route (form or GitHub issue) still exists, so treat free mcp.so listing as UNKNOWN. mcpservers.org: free with review within two weeks, or $39 for 24-hour review, an Official badge and search priority. MCP Market (I): free 4–6-week queue or $29, $69 for remote servers. The long tail goes further: AI Agent Store charges $49.99 with no free option, and several directories give a "free" listing only if you put their badge and a dofollow backlink on your site (I). The product being sold is SEO (dofollow links, domain rating) and queue position, not users.

**Hosting/gateway SaaS subsidising a free directory.** Glama founder Frank Fiegel (@punkpeye) wrote on X on April 26, 2025 that its "business offerings—including AI gateway and MCP server hosting—allow us to keep the MCP directory and its data free and open for everyone", and that its data is available via API (C). glama.ai's own structured data lists three paid offers priced from $9 to $80 per month; secondary sources add free hosting for open-source servers and $49/month boosted listings (I). In December 2025 Glama said it was "working on a revenue sharing model for open-source MCP server authors" to be announced soon (C); we found no evidence it launched (UNKNOWN).

**Smithery: what actually happened (CONFIRMED).** Arcade.dev announced on August 5, 2026 that it had acquired Smithery, "the leading public registry and hosting platform for MCP", with co-founder Anirudh Kamath joining Arcade. Smithery's own notice says that on October 31, 2026 it shut down its "connection management services": run.tools and server.smithery.ai URLs, the API, Connect, Toolbox, accounts, API keys, and the CLI and SDKs (marked deprecated). smithery.ai stays as "a public directory of MCP servers, now powered by Arcade.dev", with verified servers showing direct vendor URLs. Every charge since August 5 is being refunded. Developers received the notice by email on October 19, 2026 (per a maintainer PR). *Why* (INFERRED): Arcade sells an enterprise "actions runtime" with authorization and governance, so the overlapping consumer proxy was shut down and the directory kept as a top-of-funnel; the notice itself points users to "sign up for Arcade". Funding: PitchBook lists Smithery's investors as South Park Commons, Vertex Ventures US, Hawk Hill Ventures and the Vercel AI Accelerator, with amounts not public; Arcade's blog cites $12M from Laude Ventures and a $60M Series A "led by SYN Ventures with strategic investment from Morgan Stanley and Wipro", and Dealroom puts Arcade's total funding at $72M. Dealroom reports that the deal's "financial terms were not disclosed".

**PulseMCP (CONFIRMED pause, cause partly stated).** The site says "New server submissions and listing changes are still paused while we rework how we ingest and manage listings. We will reopen as soon as it is ready." Secondary sources say the submit page was last updated September 3, 2026, still paused on November 11, and asks publishers to use the official registry so PulseMCP can import them on reopening. One guide dates the pause to August 2026; the exact start date is uncertain. PulseMCP is run by Tadas Antanavicius, who also works on the official registry (I), which makes registry ingestion the obvious end state. PulseMCP's revenue model is not public.

**Strategic first-party directories.** Anthropic, OpenAI, GitHub, Docker, Cursor and Google charge nothing to list. Their play is platform lock-in and enterprise governance: GitHub's private-registry allowlists, OpenAI admins managing private marketplaces, Anthropic syncing directory plugins into Claude Code accounts (CONFIRMED features; motive INFERRED).

**skills.sh.** Free; ranking comes from anonymous CLI telemetry that is on by default (opt out with `DISABLE_TELEMETRY=1`). Vercel controls the ranking surface (I). It now shows Gen/Socket/Snyk audit columns (I).

## 3. Real traffic and impact

**What exists (all thin):**
- **Directory-level traffic estimates.** Submit MCP (checked October 27, 2026; DataForSEO, US Google search, MCP pages only) estimates MCP Market ~81k visits/mo, mcpservers.org ~45k, mcp.so ~18k, Smithery ~4.5k, PulseMCP ~3.6k, Docker ~3.2k, LobeHub ~2.9k, AgenticSkills ~2.2k, Glama ~2.1k, cursor.directory ~1.6k, mcp.directory ~220, Claude Market ~15/mo, and notes that "only 3 of the 21 get more than 10,000". These are search-traffic proxies and badly understate sites with direct, in-app or non-US traffic (mcp.so self-reports 266K monthly active users). Treat them as relative ranking only.
- **Listing-level evidence.** Measure.events wrote on its blog that its "/docs/ai-agents page suddenly got 2x more traffic after our MCP server was listed in awesome-mcp-servers" (self-reported, no absolute numbers). lichris's November 4, 2026 DEV Community post, "Six Directories, Three Weeks of Checks", checked one server's listings on Glama, Smithery, MCPize, ModelScope, mcp-marketplace and the official registry almost daily for three weeks and found "zero visible discovery events we can attribute. Listings existing is not the same as listings working." Apideck reported that its entry was merged into awesome-mcp-servers in April and later disappeared from the README.
- **Skills.** skills.sh shows per-skill install counts; the top skill (`find-skills`) was reported at ~3.4M installs in September 2026, and one Claude plugin tracker counted 109 new directory plugins in a single week. Install counts measure CLI runs and can be inflated ("measure fashion, not fitness").
- **Not found:** per-listing referral statistics from any catalog, or before/after GitHub star data attributable to a listing. **NOT PUBLIC:** revenue for any directory, Smithery's acquisition price, PulseMCP/Glama traffic from first-party analytics.

**Overcrowding and spam.** Glama ~113k servers, official registry ~53.5k, MCP Market ~51k, PulseMCP ~21.6k, Smithery ~27k (Apideck, October 31, 2026). awesome-mcp-servers shows thousands of open PRs (3,857 on October 31, per Apideck). Trend Micro's May 28, 2026 sweep of ~19,000 open-source MCP repos ("Hunt Them All") found that only 4.1% of AI-flagged vulnerabilities were genuinely exploitable, and that 42.6% of the manually classified vulnerable repositories showed signs of AI code generation. In a flat list your entry is one of tens of thousands; visibility comes from ranking signals.

**How catalogs rank (CONFIRMED where stated):** GitHub MCP Registry by stars and activity; skills.sh by install telemetry; Glama by quality/health scores (tool-definition grade, uptime); best-of-mcp-servers by an automated project-quality score; mcp.so and mcpservers.org sell featured/priority placement; Docker emphasises Docker-built, signed images.

**Dead or near-dead:** PulseMCP (paused), Smithery (directory only, no publish CLI), Goose extensions (closed July 30, 2026, I), Cline marketplace (stale, I), Mistral (no public submission, I), and micro-directories with double-digit monthly visits.

## 4. How to get in

| Catalog | Door | Key requirements | Review time | Automation stance |
|---|---|---|---|---|
| Official MCP Registry | `mcp-publisher login github` (or `github-oidc` in Actions), `publish` | `server.json` name must equal npm `mcpName` / PyPI `mcp-name:` marker; namespace must match login | Instant | CI publishing explicitly supported (C) |
| GitHub MCP Registry | Onboarding request in github/github-mcp-server discussion #1257 after registry publish | Public, licensed repo | Manual, undated (I) | — |
| Glama | Auto-index; claim via domain/GitHub; Add Server | Must pass build checks (Dockerfile) for a score | Automatic | — |
| punkpeye/awesome-mcp-servers | One-line PR, alphabetical, one server per line | Public repo, installable (remote goes to the remote list); Glama score badge after the description (CI bot) | Unstated; huge backlog | Agents invited to add 🤖🤖🤖 to the PR title for a "fast-tracked" merge (C) |
| awesome-claude-code | Web UI issue form only | 14+ days old with active commits, or ≥100 stars; one resource at a time; no signup/paywall | Unstated; selective | Human-only; no PRs, no `gh` CLI; violators "risk being restricted… temporarily" (C) |
| mcpservers.org | Web form | Name, category, repo | ≤2 weeks free / 24h paid | Form is JS-driven (I) |
| mcp.so | Web form | Repo URL + name | Instant if paid | — |
| Docker MCP Catalog | PR with `server.yaml` (wizard or `cat add_mcp_server.md \| claude`) | Permissive license, Dockerfile, docs, security contact; test credentials form for auth servers | Docker review, then ~24h | Docker itself suggests Claude Code to draft the entry (C) |
| Claude directory | claude.ai/directory/manage | Paid plan; `claude plugin validate --strict`; pre-submission checklist; tool annotations required for connectors (I) | Weeks; public reports of 40+ waits (I) | PRs to official repos ignored/closed (C) |
| Own Claude Code marketplace | Commit `.claude-plugin/marketplace.json` | Permanent kebab-case plugin name; bump `version` or omit it to use commit SHA | None | — |
| ChatGPT/Codex directory | OpenAI Platform portal | Verified developer identity; manifest limits (e.g., ≤20 capabilities); skills as direct children of `skills/`; no lifecycle hooks | Automated scan + human review (I) | — |
| Cursor Marketplace | cursor.com/marketplace/publish | plugin.json + mcp.json; free; MIT/BSD/Apache (I) | Weeks, no status (I) | — |
| skills.sh | None: users run `npx skills add tonydzi/<repo>` | Spec-valid SKILL.md | Instant on first install | Self-installs count, so the count is gameable (I) |

**Who penalises automation:** awesome-claude-code explicitly (human-only, restriction threat, one-at-a-time, auto-close). Anthropic's Claude repos ignore or auto-close PRs. awesome-mcp-servers does the opposite and labels agent PRs; we found no rule against bulk PRs, but the Glama-badge gate filters most of them, and the 🤖🤖🤖 label may serve to sort agent PRs (INFERRED, unconfirmed). Directories with badge-for-listing schemes check your site for the backlink.

## 5. Implications for a free open-source distributor

**Skills vs. servers.** Servers go to the MCP channel (registry → Glama → awesome-mcp-servers → Docker → GitHub onboarding). Skills go to the skill channel (spec-valid SKILL.md → your own Claude Code marketplace → skills.sh via installs → Anthropic and OpenAI directories as curated bundles → awesome-claude-code). mcp.so and MCP Market accept both but are pay-to-skip.

**Bundle, but not as one 232-skill blob (INFERRED from the spec).** Agents load every installed skill's name and description at startup (~50–100 tokens each), so 232 skills would add roughly 12k–23k tokens of catalog to every session. Split `join-the-fleet` into 8–15 themed plugins inside one marketplace repo, so users install `devops@join-the-fleet` rather than everything. skills.sh still lets users pick single skills from a bundle repo (`--skill name`), so one-repo-per-skill buys nothing except maintenance cost. Keep plugin names permanent; renames break every existing install unless you use the marketplace `renames` map.

**Measure what matters (ordered by signal quality):** (1) first successful use: hard to see in local tools; add an opt-in "first run" log line or ask in the README; (2) installs at the source: npm/PyPI download counts, skills.sh per-skill install counts, Docker pulls; (3) GitHub traffic referrers, to see which catalog actually sends visitors; (4) stars, a lagging vanity signal that GitHub's registry and awesome-claude-code nonetheless use as gates. Ignore directory "views". Claude Code documents a way to measure plugin cost and usage, which is worth wiring in for the bundle.

**What gets you flagged or delisted:** bulk or agent submissions to human-only lists; more than one resource per issue at awesome-claude-code; entries without a Glama badge (auto-blocked); servers that fail health checks (Glama connector "Healthy" required for the remote list); non-permissive licenses (Docker, Cursor); vague tool names or descriptions (rejected by Slack, I); missing tool annotations (Anthropic connectors, I); stale or duplicate listings after renames; entries that vanish after merge, so re-check them.

## Recommendations for a free open-source distributor (ranked by reach per hour)

1. **Official MCP Registry, automated (≈1h setup, then zero).** Add a GitHub Actions job that runs `mcp-publisher login github-oidc && mcp-publisher publish` after each npm/PyPI release for every server, starting with `sqlite-graph-memory` (namespace `io.github.tonydzi/…`). This is the feed for Glama, mcp.directory, GitHub onboarding and PulseMCP's eventual reopening.
2. **Your own Claude Code marketplace in `join-the-fleet` (≈2–4h).** Restructure into themed plugins, add `.claude-plugin/marketplace.json`, run `claude plugin validate --strict` in CI, and put one-line install commands for both `claude plugin install …` and `npx skills add tonydzi/join-the-fleet` at the top of the README. This also makes skills.sh list you as soon as people install.
3. **Glama claim + badges (≈15 min per server).** It unlocks awesome-mcp-servers and gives you a public quality and health score.
4. **Anthropic directory portal (≈2h per bundle; requires a paid plan).** Submit 1–3 of your best themed plugins, not all 232 skills, then wait weeks.
5. **punkpeye/awesome-mcp-servers PR (≈15 min per server, with badge).** One server per PR, submitted by a human. Use 🤖🤖🤖 only if an agent truly wrote the PR, and check after merge that the entry is still there.
6. **awesome-claude-code (≈15 min, once).** One human issue-form submission for the bundle once it is 14+ days old with active commits, or has 100+ stars. Never script it.
7. **OpenAI ChatGPT/Codex directory (≈3–4h once identity is verified).** Use the "Skills only" path for the same curated bundle.
8. **Docker MCP Catalog (≈1–2h per server).** Good fit for local servers such as `sqlite-graph-memory`; Docker builds and signs the image.
9. **mcpservers.org free form (≈10 min per server).** It is the second-highest-traffic directory by the October estimate, and free if you can wait two weeks.
10. **GitHub MCP Registry onboarding request (≈10 min).** Post in discussion #1257 after registry publishing.

**Skip or deprioritise:** mcp.so and MCP Market paid tiers ($39 × 55 repos ≈ $2,145 for SEO links and badges, with no install evidence); badge-for-backlink and pay-only directories; Smithery (no publish path after the shutdown); PulseMCP (wait for registry import); Cline (stale); Goose (closed); micro-directories under ~1k visits/month. Spend the saved hours on one good write-up per flagship tool, since content is what feeds search and LLM answers.

## Caveats
- Several figures (traffic estimates, Glama pricing and boosted listings, MCP Market prices, Cursor rules, OpenAI review requirements, directory counts) come from third-party guides and are marked INFERRED. Many of these guides are themselves SEO products, and some list directories they operate.
- Traffic estimates measure US Google search only and conflict sharply with self-reported figures (mcp.so). No catalog publishes per-listing referrals; all install and traffic impact claims here are anecdotal.
- The date the PulseMCP pause began is reported as both August and September 2026. Whether mcp.so still has any free route is unresolved because its own page metadata and form disagree.
- The meaning of awesome-mcp-servers' 🤖🤖🤖 "fast-track" (genuine speed-up vs. a sorting label) is not documented beyond the one-sentence rule.
- Source links for each claim are attached separately by the citation pass rather than inline.

## Sources (md_citations)
- https://mcp.so/submit
- https://submitmcp.com/
- https://x.com/punkpeye/status/1916160450753872238
- https://linklyhq.com/blog/mcp-server-directories
- https://www.smithery.ai/future
- https://github.com/cbetz/last-ehr/pull/225
- https://www.pulsemcp.com/servers?other%5B%5D=remote
- https://mentionagent.ai/blog/mcp-server-directories/
- https://www.apideck.com/blog/mcp-server-directories
- https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md
- https://github.com/punkpeye/awesome-mcp-servers/blob/main/CONTRIBUTING.md
- https://github.com/punkpeye/awesome-mcp-servers/pull/19288
- https://github.com/anthropics/claude-plugins-official
- https://reviewtimes.fyi/claude-plugins
- https://github.blog/ai-and-ml/generative-ai/how-to-find-install-and-manage-mcp-servers-with-the-github-mcp-registry/
- https://modelcontextprotocol.io/registry/quickstart
- https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/server-json/official-registry-requirements.md
- https://dev.to/agentoolrank/where-to-list-an-mcp-server-or-ai-agent-tool-free-vs-paid-checked-oct-2026-1eak
- https://composio.dev/content/how-to-publish-mcp-server
- https://atlan.com/know/ai-agent/what-is-an-mcp-registry/
- https://github.blog/ai-and-ml/github-copilot/meet-the-github-mcp-registry-the-fastest-way-to-discover-mcp-servers/
- https://github.blog/changelog/2025-11-18-internal-mcp-registry-and-allowlist-controls-for-vs-code-stable-in-public-preview/
- https://github.com/docker/mcp-registry/blob/main/CONTRIBUTING.md
- https://github.com/wong2/awesome-mcp-servers
- https://agentskills.io/specification
- https://rywalker.com/research/skills-sh
- https://developers.openai.com/codex/plugins/build
- https://developers.openai.com/plugins/deploy/submission-errors
- https://github.com/punkpeye/awesome-mcp-servers
- https://github.com/tolkonepiu/best-of-mcp-servers
- https://capitalandcompute.net/blog/best-mcp-servers/
- https://www.corpusiq.io/docs/hermes/mcp/directories/submission-guide-2026
- https://crakseo.com/submit-mcp
- https://automationswitch.com/ai-workflows/where-to-find-mcp-servers-2026
- https://github.com/Deke1Yahav/agents-hive/issues/39
- https://mcpservers.org/submit
- https://mcpservers.org/servers/submitmystartup-com
- https://glama.ai/blog/2025-12-07-the-state-of-mcp-in-2025
- https://www.seosiri.com/2026/09/mcp-monetization-gap-glama-smithery-developer-losses.html
- https://www.arcade.dev/blog/
- https://www.arcade.dev/blog/smithery-joins-arcade/
- https://github.com/alxspiker/mcp-server-ftp/pull/24
- https://pitchbook.com/profiles/company/907981-48
- https://code.claude.com/docs/en/plugins/publish
- https://en.wikipedia.org/wiki/OpenAI_Codex_(AI_agent)
- https://tomrochette.com/agents/skills-sh/
- https://www.skills.sh/docs/cli
- https://www.skills.sh/docs/faq
- https://devops.com/github-mcp-registry-launches-as-central-hub-for-ai-development-tools/
- https://github.com/anthropics/claude-plugins-community
- https://www.webfuse.com/agent-skills-cheat-sheet
- https://agentskills.io/client-implementation/adding-skills-support
- https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context
- https://github.com/punkpeye/awesome-remote-mcp-servers/pull/4155
- https://github.com/docker/mcp-registry
- https://github.com/github/github-mcp-server/discussions/1257