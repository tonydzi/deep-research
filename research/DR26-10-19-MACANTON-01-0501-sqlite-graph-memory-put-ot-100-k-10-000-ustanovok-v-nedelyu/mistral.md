# DR26-10-19-MACANTON-01-0501 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

From 100 to 10,000 weekly installs — evidence review for sqlite-graph-memory

DR26-10-19-MACANTON-01-0501 · Research date: 2026-10-19 · Scope: January 2025 – October 2026

Question

Which small open-source packages in agent memory, RAG, or MCP infrastructure actually reached 10,000+ weekly installs during 2025–2026, what verifiably caused each growth bend, what order the winners did things in, what measurably did not work, how much of small-package download counts is mirrors/CI/bots — and the five cheapest moves available to a package at your base (113 first-week downloads, 9 stars, no directory listing).

Executive summary

No case of a verified "100 → 10,000/week" jump driven by marketing was found. Every package in this space that reached 10k+/week rode an ecosystem wave (MCP, remote-MCP, graph-memory) and a distribution surface owned by someone else: official SDK/docs provenance (fastmcp, mcp-server-fetch), setup instructions inside a bigger product's docs (mcp-remote via DeepWiki MCP), client plugin flows (basic-memory → Claude Code), or organic "bolt-on" adoption by agent users (serena). Directories and launches alone did not produce a single verified 10k case.

Hacker News is a lottery, and for this niche mostly a losing one. Measured HN moments: mem0 (201 points, 2024-09-04) and Graphiti (142 points, 2024-09-04) hit; Basic Memory's launch got 4 points (2025-03-15), cognee's two Show HNs got 6 and 9 points, LightRAG's submissions got 1 point. The packages that flopped on HN still grew — via other channels.

Directory/registry listings are table stakes, not growth. basic-memory sits on PulseMCP at #142 with ~627k estimated all-time visitors (~8.5k/week) — consistent with its ~7.7k PyPI weekly. But the closest-to-you comparison, solo-maintained mcp-memory-service, is listed in the same directories and has stayed at ~2–5k/week for its visible history, with one suspicious spike. Meanwhile the copy-paste installer economy is dying: @smithery/cli npm installs peaked at ~277k/week in mid-October 2025 and fell to ~3.2k/week by October 2026.

Download counts for tiny packages contain measurable noise. Measured on basic-memory (Apr–Oct 2026): 11.4% of downloads were mirrors (328,085 with mirrors vs 290,642 without, over the same window). Worse, letta shows one-week spikes to 107k and 117k downloads against a ~3–4k/week baseline that revert immediately — a CI/bot signature — and the week of 2026-08-03 shows a synchronized spike across graphiti-core (402k), mcp-server-fetch (451k), mcpo (417k), and cognee that looks like automated evaluation harnesses installing the whole memory stack, not users.

Time to 10k/week is quarters, not weeks, from a base like yours. Measured: basic-memory released 2025-03-15 and first crossed 10k/week in the week of 2026-06-08 (~64 weeks), peaking at 19.3k before settling at ~7.7k. serena-agent went 4.1k → 14.4k in one week (April 2026) but needed ~21 more weeks to reach 45k. Only mcp-remote (npm) shows a fast verified climb: already ~1.3k/week in April 2025, 10k by the week of 2025-05-05, ~214k by the week of 2025-08-25 — and its driver was other products' setup docs, not its own marketing.

Buying stars and drive-by benchmarks have documented backfire risk. The StarScout study (ICSE 2026) found ~6M suspected fake stars on GitHub and GitHub removes detected campaigns. The mem0–Zep LoCoMo benchmark dispute produced public rebuttals and community accusations of flawed methodology on both sides — no download spike for either, only reputational attrition.

Methodology

Search angles: (1) download-history measurement from primary APIs: pypistats.org (/recent and /overall?mirrors=false, daily, aggregated to ISO weeks), npm registry downloads API (daily range, 18-month limit), pepy.tech totals; (2) timestamped cause verification via Hacker News Algolia API (points, comment counts, dates), GitHub repo metadata, product docs (DeepWiki MCP setup, Claude Code plugin repos, PulseMCP server pages); (3) negative-tactic evidence: fake-star research (arXiv), llms.txt adoption studies, Product Hunt outcome analyses, benchmark-dispute primary sources; (4) news search for the August 2026 download spike.

Key limitations — read before using any number below:

The pypistats API retains only ~190 days of daily history (window 2026-04-08 to 2026-10-18). Weekly numbers for 2025 do not exist in any public API I could reach. Wayback holds no archived snapshots of the pypistats API; ClickPy's ClickHouse endpoint was behind bot protection; the BigQuery PyPI dataset was not connected. Where 2025 numbers are unavailable, this report says not found rather than estimating.

npm's downloads API returns only the last 18 months (from 2025-04-18).

"Week" below = ISO week starting Monday, without_mirrors for PyPI.

Download events are not users (see §5).

Findings
1. Case table

All PyPI weekly numbers from the pypistats.org API (window 2026-04-08 → 2026-10-18), npm numbers from the npm downloads API, totals from pepy.tech. "Base" = earliest measured weekly value in the observable window plus first-release evidence. Confidence reflects both the numbers and the causal attribution.

Package

	

Base

	

Peak (observed)

	

Week of bend

	

Cause

	

Evidence

	

Confidence




fastmcp

	

Built "over a weekend in late 2024" (jlowin.dev); FastMCP 1.0 upstreamed into the official MCP Python SDK

	

11.5M/wk (Apr 2026) → 29.4M/wk (wk 2026-06-15); 13.6M/wk now; 741M total (pepy)

	

Continuous growth; no single bend in window

	

MCP spec wave + official-SDK provenance ("FastMCP 1.0 was upstreamed into the official MCP SDK in 2024" — aipotluck.org) + ecosystem tutorials (Firecrawl, Jan 2026)

	

pypistats

	

Numbers: high. Cause: high (provenance documented), but its starting base is not comparable to yours




mcp-remote (npm, solo maintainer geelen)

	

~1,290/wk (wk 2025-04-14)

	

2.9M/wk (wk 2026-08-24); 505k/wk now

	

2025-05-05 (crossed 10k/wk); 2025-08-25: 47,317 → 213,993

	

Remote-MCP OAuth wave; the setup string inside other products' docs — e.g. DeepWiki MCP instructions telling users to run npx mcp-remote https://mcp.deepwiki.com/mcp (DEV walkthrough, Medium guide)

	

npm downloads API

	

Numbers: high. Cause: medium-high (docs verified; attribution to specific docs is inferential)




serena-agent

	

4,111/wk (wk 2026-04-06)

	

45,360/wk (wk 2026-10-12); 772k total

	

2026-04-13: 4,111 → 14,445; 2026-09-07: 23,197 → 45,316

	

Organic bolt-on adoption by agent users ("recurs organically across agent-workflow discussions" — Ry Walker research; 25.2k stars, 170+ contributors; documented integrations with Claude Code, Codex, OpenCode, Gemini CLI, Cursor). No big HN thread exists (verified via HN Algolia)

	

pypistats, pepy

	

Numbers: high. Cause: medium (organic-recurrence source is secondary research)




basic-memory

	

Released 2025-03-15 (PulseMCP); 5,154/wk (wk 2026-04-06); 630k total

	

19,302/wk (wk 2026-06-08); 7,671/wk now

	

2026-04-13: 5,154 → 9,375; 2026-06-08: 9,940 → 19,302 (crossed 10k ~64 weeks after release)

	

Client-integration surface: official Claude Code plugins + per-client install blocks + Smithery badge + directory listings (PulseMCP #142, ~627k est. all-time visitors, ~8.5k/wk) + Cloud/Teams launches (Feb/May 2026) + v0.21.0 May–June 2026 (changelog). Its HN launch on 2025-03-15 got 4 points — not the driver

	

pypistats, PulseMCP, HN Algolia

	

Numbers: high. Cause: medium (multiple plausible contributors; none timestamped to the week by the maintainer)




graphiti-core (Zep)

	

Repo created 2024-08-08; Show HN 2024-09-04: 142 points, 21 comments (HN Algolia)

	

77,437/wk (Apr 2026) → 402,230/wk (wk 2026-08-03); 197.7k/wk now; 11.0M total

	

2025 numbers: not found; in window: 2026-06-15: 209,762 → 383,497

	

Graph-memory wave: arXiv paper (Zep, Jan 2025), HN launch (Sept 2024), influencer/blog cycle ("Stop Using RAG for Agent Memory", AI Engineer World's Faire 2025 — Zep blog)

	

pypistats, pepy

	

Numbers: high (window only). Cause: medium-high. Base is not comparable — VC-backed company




lightrag-hku

	

Repo created 2024-10-02; paper 2024-10-08 (arXiv 2410.05779); first PyPI release 2024-11-11 (PyPI)

	

29,057/wk (Apr 2026) → 34,650/wk now; peak 75,012/wk (wk 2026-07-20); 2.29M total

	

2024–25 numbers: not found

	

Paper virality on X/arXiv — not HN (its HN submissions got 1 point; verified via HN Algolia). 40.2k stars by Oct 2026

	

pypistats, pepy

	

Numbers: high. Cause: medium (X-virality widely reported but not timestamped here). Base: university lab, not comparable




mcpo

	

31,381/wk (wk 2026-04-06); repo stale since 2026-02-27 (open-webui/mcpo)

	

261,067/wk (wk 2026-07-27) → peak 534,430/wk (wk 2026-08-10); 393,962/wk now

	

2026-07-27: 29,575 → 261,067

	

Not found. Viral solo launch Apr 2025 (repo created 2025-03-30, 4.4k stars), adopted into open-webui org, but it is not an open-webui dependency (pyproject.toml checked) and the repo had no commits since Feb 2026 while downloads grew 13×. Pattern consistent with automated installs (see §5)

	

pypistats

	

Numbers: high. Cause: low — unverified, possibly not human users




cognee

	

12,288/wk (wk 2026-04-06); 1.33M total

	

65,932/wk (wk 2026-08-10); 25,074/wk now

	

2026-08-10: 34,577 → 65,932

	

Company-backed (Topoteretes); Show HN launches flopped (6 points Feb 2025, 9 points Jun 2025 — HN Algolia); growth via X/ecosystem; Aug 2026 spike cause not found

	

pypistats, pepy

	

Numbers: high. Cause: low-medium




mcp-server-fetch (Anthropic)

	

Reference server in the official modelcontextprotocol/servers repo

	

450,837/wk (wk 2026-08-03); 213,768/wk now; 33.1M total

	

Steady with ecosystem-wide Aug 2026 spike

	

Being listed in the official MCP docs/servers repo — installs whenever users replicate reference configs

	

pypistats

	

Numbers: high. Cause: high (docs listing is structural). Base not comparable (Anthropic)




mem0ai

	

Show HN 2024-09-04: 201 points, 61 comments (HN Algolia)

	

605,555/wk now; 44.9M total

	

2025 numbers: not found

	

Viral launch + X + company. The only package here whose HN launch demonstrably hit

	

pypistats, pepy

	

Numbers: high. Cause: medium-high. Base not comparable (company from day one)




letta

	

Baseline ~2,967–4,636/wk

	

107,010/wk (wk 2026-06-29), 117,023/wk (wk 2026-08-10) — both revert within a week

	

Spikes, not growth

	

Spikes against a flat baseline with immediate reversion = CI/bot signature, not adoption (§5)

	

pypistats

	

Numbers: high. Interpretation: medium-high




mcp-memory-service (solo dev, closest to your base)

	

3,375/wk (wk 2026-04-06); 387k total

	

21,303/wk (wk 2026-08-24) — one week, then back to ~2,363/wk

	

Never sustained 10k/wk

	

Directory-listed for years; still ~2–5k/wk. Direct evidence that listings alone do not produce 10k/wk

	

pypistats, pepy

	

High

Honest reading of Q1: I could verify 9+ packages at 10k+/week, but none has a verified "under-500/week, under-100-stars" starting point comparable to yours. The closest comparable cases are basic-memory (solo-ish, but with a company behind it and a strong niche fit) and mcp-memory-service (truly solo, still stuck at ~2–5k). The verified winners either started inside a wave with institutional backing, or their "bend" data no longer exists. Treat "10,000/week" as achievable for a solo package only via borrowed distribution (§6, move 4).

2. What caused the bends — taxonomy

Verified by timestamp (measured download curve + dated source):

Listing in official docs / SDK provenance: mcp-server-fetch (official servers repo), fastmcp (1.0 upstreamed into the official MCP SDK; that provenance is stated on its current materials — aipotluck.org).

Install string inside a bigger product's setup docs: mcp-remote — DeepWiki MCP instructions embed npx mcp-remote (DEV walkthrough, Medium guide); its npm curve crossed 10k/wk in May 2025 and exploded in late August 2025, exactly when remote-MCP tutorials spread.

Client plugin flows: basic-memory ships official Claude Code plugins (basic-memory-plugins) and per-client config blocks; its curve crossed 10k/wk only after the Claude-plugin/Cloud/Teams sequence, ~15 months after release.

Big HN thread: verified only for mem0 (201 pts, 2024-09-04) and Graphiti (142 pts, 2024-09-04), both before or at the start of your window and both institution-backed.

Maintainer's own story / secondary accounts (not timestamped against the curve):

Organic agent-user adoption: serena — "no single big HN launch thread; it recurs organically" (Ry Walker).

Paper + X virality: lightrag-hku (arXiv Oct 2024; HN demonstrably not the channel — 1-point submissions).

Company content cycle: Zep's paper and "Stop Using RAG for Agent Memory" talk-cycle (Zep blog) kept graphiti in the conversation.

3. Order of operations and time-to-milestones

What the measured curves say about sequencing:

Directories/listings come first but carry you to ~1–5k/wk at best. Both basic-memory and mcp-memory-service are listed; one is at ~7.7k, the other at ~2.4k. Listings are necessary plumbing, not a bend.

mcp-remote (fastest verified climb): ≥1k/wk by mid-April 2025 → 10k/wk by week of 2025-05-05 (≤3 weeks) → ~214k/wk by week of 2025-08-25 (≈17 weeks later). Driver: being the install string in other products' docs, i.e., borrowed distribution.

basic-memory (slowest, most comparable): released 2025-03-15 → first observed 10k+/wk in week of 2026-06-08 (≈64 weeks; HN launch flopped immediately) → peak 19.3k → settled ~7.7k. Order: HN (flop) → Smithery/PulseMCP listings → Claude Code plugins → Cloud (Feb 2026) → Teams (May 2026) → 10k/wk.

serena-agent: 4.1k → 14.4k in one week (Apr 2026) → 45.4k ~21 weeks later (Sept 2026). Cause of the April jump not documented by the maintainer.

mcpo: the only package with a verified 10× jump (29.6k → 261k in the week of 2026-07-27) — and its cause is not found, with strong bot/CI suspicion. Do not model your plan on it.

From a base of ~100/week, no verified case reaches 10k/week in under ~60 weeks except by being absorbed into someone else's distribution.

4. What did NOT work — with measured evidence

Bought stars: the StarScout study found ~6M suspected fake stars on GitHub, documents star merchants, and reports GitHub removing detected campaigns — a purchased-star history is a liability, and star counts were not the observable driver of download bends in any case here (arXiv 2412.13459, CMU coverage).

Mass directory submissions / copy-paste installers: the @smithery/cli npm package — the "npx @smithery/cli install" line that hundreds of MCP READMEs embedded — peaked at ~277k installs/week (wk of 2025-10-13) and fell to ~3.2k/week by Oct 2026 (npm API). The official MCP Registry launched in preview on 2025-09-08 (registry.modelcontextprotocol.io), clients grew native install commands, and the third-party installer economy deflated. Being in 30 directories buys you the mcp-memory-service curve (~2–5k/wk), not 10k.

Hacker News as a plan: flopped launches measured via HN Algolia: Basic Memory 4 points, cognee 6 and 9 points, LightRAG 1 point. It works occasionally (mem0: 201) but the variance is extreme and you cannot retry.

Product Hunt: an analysis of 500 PH SaaS launches found 487 effectively dead (r/SaaS); practitioner accounts describe PH as no longer driving developer traffic (DEV, LaunchList). No measured case of a pip-install-count bend traced to PH was found.

llms.txt: a 137k-site analysis found 97% of llms.txt files never get read and no correlation with AI citations (analysis); a second study found no statistical effect on AI visibility (study); adoption (~11.5% of top-10k sites) exists but with no demonstrated payoff (2026 adoption data). For a PyPI package, llms.txt is not a channel.

Drive-by benchmark posts against a competitor: the mem0–Zep LoCoMo dispute is the canonical cautionary case. Mem0 claimed SOTA; Zep published "Lies, Damn Lies, & Statistics" with a corrected evaluation putting Zep ahead (Zep blog); the community picked apart both methodologies (r/LangChain); and no download bend is visible for either party around the dispute. Both brands absorbed credibility damage.

Mass PRs into other repos: no measured evidence found either way. Not recommended on priors (maintainers report spam fatigue), but I did not find quantified data.

5. How much of small-package downloads is mirrors, CI and bots?

Mirrors — measured: basic-memory, window 2026-04-08 → 2026-10-18: 328,085 downloads with mirrors vs 290,642 without = 11.4% mirror share (pypistats API, both variants). without_mirrors is the standard filter and is what all numbers above use.

CI/bots — inferable from curve shape, not from public counters. PyPA itself warns download counts are "highly inaccurate": pip's cache lowers them, mirrors raise/lower them, and "unofficial scripts or attempts at download count inflation" raise them (Python Packaging User Guide). Concretely, in this dataset: letta's baseline is ~3–4k/wk with single-week spikes to 107k/117k that revert within days; mcp-memory-service's one 21.3k week; and the synchronized week of 2026-08-03 spike across graphiti-core, mcp-server-fetch, mcpo, cognee, and mcpo's continued 394k/wk on a repo with no commits since Feb 2026. Cross-package, cross-registry synchronization in one week is not how humans install software; it looks like an automated evaluation harness installing the entire memory stack at once.

Filtering practice: pypistats/pepy expose without_mirrors; no public PyPI "human vs bot" classification was found (ClickPy's ClickHouse backend, which might allow such queries, was behind bot protection and could not be inspected — noted as not found). GitHub unique clones are an orthogonal, much smaller signal: your 6 forks with zero commits ahead are consistent with 113 downloads being mostly organic-but-tiny.

Practical number for you: assume your true human installs are somewhere between your without_mirrors count and ~85–90% of it, and expect one-week spikes of 5–20× on any package that an eval harness or popular template starts installing. A spike that reverts within a week is not adoption; check two consecutive weeks before celebrating.

6. Ranked cheapest moves for sqlite-graph-memory at your base

Ranked by expected value per hour, given your position (MCP server, markdown-native, wikilinks, uvx-installable, MIT, 113 first-week downloads, 9 stars, zero directory presence).

Move 1 — Complete the listing plumbing. (~4–6 h)
Publish to the official MCP Registry (preview since 2025-09-08, the discovery layer clients consume — registry site, announcement), ship a server.json (the standardized format directories ingest — PulseMCP on server.json), and submit to Glama, PulseMCP, and the awesome-mcp-servers list.
Expected effect, with evidence: directory-listed memory servers run at ~2–8.5k weekly "estimated visitors" (basic-memory at #142, mcp-memory-service at ~2.4k/wk). Realistic band for you after 30 days: 100 → 300–800 downloads/week.
Kill criterion: if without_mirrors weekly is still <300 after 30 days with registry + 3 directories live, the bottleneck is positioning/naming, not distribution — spend zero more hours on listings.

Move 2 — Make the install path one copy-paste per client. (~3 h)
README blocks: claude mcp add one-liner, Cursor/VS Code JSON, Codex config, plus a Smithery badge and a plain uvx sgm-mcp quickstart. This is exactly the surface basic-memory optimized (per-client blocks + plugins repo).
Expected effect: conversion of existing README views to installs; it compounds with every listing. Evidence: mcp-remote's curve is what happens when the install string lives in other people's docs — the same mechanism, one step removed.
Kill criterion: if GitHub README views rise (check GitHub traffic) but installs don't move after 30 days, the friction is elsewhere (name, pitch, trust signals) — not the install path.

Move 3 — One time-boxed launch: Show HN + r/LocalLLaMA + r/ObsidianMD, framed as a finding, not a release. (~6 h)
Angle that fits your evidence: "agents can remember across your existing markdown notes through hand-written wikilinks — no graph DB, no server." Not "I released v0.3.0."
Expected effect: median ≈ zero (basic-memory: 4 points; cognee: 6–9 points), tail large (mem0: 201 points). It's a lottery ticket priced at 6 hours — buy exactly one.
Kill criterion: if it doesn't reach the HN front page or a 50+ upvote Reddit thread on the first try, do not resubmit; re-allocate to Move 4.

Move 4 — Borrowed distribution: get your install command into 1–2 surfaces you don't own. (~8–16 h per attempt)
Ranked targets for your niche: (1) Obsidian-focused agent/AI-tool guides and the agent-notes community (wikilinks are your unique hook — nobody else in the memory space owns that vocabulary); (2) an agent framework's memory cookbook or template; (3) a Claude Code plugin/skill listing (basic-memory's pattern — plugins repo).
Expected effect, with evidence: this is the only mechanism verified to carry small packages past 10k/wk — mcp-remote (other products' setup docs) and basic-memory (Claude Code plugin flow) both crossed 10k/week on borrowed distribution, not directories or HN.
Kill criterion: if two credible outreach attempts produce no doc mention within 30 days, stop outbound; build the integration (e.g., an Obsidian plugin or skill package) so the surface has a reason to list you, and re-attempt once.

Move 5 — An honest, reproducible comparison against basic-memory and mcp-memory-service. (~8 h)
Public harness, public data, your losses included, and publish the failure modes (e.g., where cross-encoder reranking costs too much). Third-party comparisons already rank this category ("Best Memory Tools for Claude Code: 8 Options Compared" — BrainTube); be the best-cited input rather than claiming SOTA.
Expected effect: small direct installs; main value is becoming the citation target in AI answers and future comparison posts. The mem0–Zep dispute (Zep rebuttal) is the fence to stay on the right side of: contestable claims get publicly corrected.
Kill criterion: if no inbound link, GitHub referral, or third-party mention shows up in 30 days, drop the content-PR channel entirely.

Do-not-do list (measured): bought stars (removal + reputational risk), mass directory blasts beyond the big four, Product Hunt, llms.txt, unsolicited benchmark PRs/posts attacking competitors, reposting HN. Budget for zero on all of these.

Claims I could not verify

2025 weekly download histories for any PyPI package. pypistats retains ~190 days; Wayback holds no archived pypistats API responses; ClickPy's ClickHouse endpoint was bot-blocked; the BigQuery PyPI dataset requires a connection I didn't have. Every 2025 "week of bend" for graphiti-core, lightrag-hku, cognee, fastmcp, basic-memory: not found.

npm history before 2025-04-18 (API 18-month limit), including mcp-remote's first-release-to-1k timeline and its npm package creation date (registry response too large to parse reliably).

The cause of mcpo's 29.6k → 261k jump in the week of 2026-07-27 and of the synchronized spike of 2026-08-03 across graphiti-core, mcp-server-fetch, mcpo, cognee (and mcp-remote on npm). The stall-while-exploding pattern suggests automated installs, but no source confirms it.

That any of the 10k+/week cases started below 500 downloads/week and 100 stars — the comparable-base premise. fastmcp, mem0, graphiti, LightRAG, cognee all had company/university backing at launch; mcp-remote's earliest observable point is already ~1.3k/week.

Which share of fastmcp's 13.6M weekly is CI/templates — plausibly large, unverifiable with public data.

Any public PyPI human-vs-bot classification (e.g., a ClickPy "human" flag): not found; treat it as not existing.

Star-purchase pricing/effectiveness for a package at your base: the StarScout paper documents the market's existence and detection, not per-package ROI.

PulseMCP/Glama/Smithery traffic per listing — only PulseMCP publishes per-server estimates, and its methodology is a proprietary mix ("publicly available data, social signals, and more"), not auditable.

Whether mass PRs into other repos help or backfire: no quantified evidence found.

Open questions

What did the week of 2026-08-03 actually trigger the cross-registry spikes? A named evaluation harness, a client default, or a mirror artifact? If it is an eval harness, memory-package download counts are currently inflated across the board.

Did any solo, no-company, no-university package in this space ever hold 10k+/week for 8+ consecutive weeks? mcpo is the only candidate curve and its numbers are suspect; basic-memory has a company behind it. If the answer is "none", your realistic 2-year ceiling via MCP channels alone is the 2–8k band, and 10k requires the Move-4 mechanism.

Does the official MCP Registry surface installs inside Claude/Codex client UIs in a way that produces measurable download bumps? Registry adoption data is not public; check your own curve after Move 1 as the experiment.

Recommendations / next steps

This week (≈10 h): Move 1 (registry + server.json + three directories) and Move 2 (per-client install blocks). These are prerequisites for everything else.

Weeks 2–4 (≈6 h): one launch attempt (Move 3). Frame it on the wikilink-over-existing-notes finding.

In parallel from week 2 (≈16 h): two outreach attempts for borrowed distribution (Move 4), Obsidian ecosystem first.

Day 30 checkpoint: without_mirrors weekly downloads (pypistats API), GitHub unique clones, README views. Decision rules are in the kill criteria above; the single most informative threshold is <300/week after listings are live → fix positioning before spending another hour on distribution.

Day 60: publish the honest comparison (Move 5) only if Move 4 produced at least one borrowed surface to point at.

Source notes

Source

	

Credibility

	

Last updated




pypistats.org API (overall/recent, per package)

	

4/5

	

2026-10-19 (queried directly)




npm registry downloads API (mcp-remote, @smithery/cli, browser-tools-mcp)

	

4/5

	

2026-10-19 (queried directly)




pepy.tech project pages (totals, last 30 days)

	

4/5

	

2026-10-19




Hacker News Algolia API (story points, comments, dates)

	

4/5

	

2026-10-19 (queried directly)




Python Packaging User Guide — Analyzing PyPI package downloads

	

5/5

	

current (opened 2026-10-19)




PulseMCP — Basic Memory server page (est. visitors, release date, rank)

	

3/5

	

opened 2026-10-19 (estimates are proprietary)




MCP Registry site — "launched in preview on September 8, 2025"

	

4/5

	

opened via search snippet




Introducing the MCP Registry — MCP blog (Feb 2025 grassroots origin)

	

4/5

	

2025-09




jlowin.dev — FastMCP 3.0 launch ("built over a weekend in late 2024", "a million downloads per day")

	

3/5

	

2026-01




aipotluck.org — FastMCP analysis (94M monthly downloads; SDK upstream history)

	

3/5

	

2026-08-13




PyPI — lightrag-hku 1.0.0 (first release 2024-11-11)

	

5/5

	

2024-11-11




arXiv 2410.05779 — LightRAG paper (2024-10-08)

	

5/5

	

2024-10




arXiv 2501.13956 — Zep: Temporal Knowledge Graph Architecture (2025-01)

	

5/5

	

2025-01




arXiv 2412.13459 — StarScout: Six Million (Suspected) Fake Stars (ICSE 2026)

	

5/5

	

2024-12 (v2 for ICSE 2026)




CMU Societal Computing — Fraudsters Use Fake Stars To Game GitHub

	

4/5

	

2025




Zep blog — Lies, Damn Lies, & Statistics: Is Mem0 Really SOTA?

	

3/5

	

2025 (updated with corrected result 75.14% ± 0.17)




r/LangChain — community analysis of the mem0 benchmark claim

	

2/5

	

2025




DEV — Try DeepWiki MCP Server with MCP Clients (npx mcp-remote setup)

	

3/5

	

2025




Ry Walker Research — Serena (25.2k stars, organic-recurrence account)

	

2/5

	

2026-06




basicmachines-co — Basic Memory changelog (v0.21.0, May–June 2026)

	

3/5

	

2026-06




basicmachines-co — basic-memory-plugins (official Claude Code plugins)

	

4/5

	

2026




GitHub — open-webui/mcpo (repo last commit 2026-02-27)

	

4/5

	

opened 2026-10-19




GitHub — open-webui pyproject.toml (mcpo not a dependency — checked)

	

4/5

	

opened 2026-10-19




r/SaaS — "I analyzed 500 Product Hunt SaaS launches. 487 are dead"

	

2/5

	

2025




llms.txt 137k-site analysis — "97% of llms.txt Files Never Get Read"

	

2/5

	

2025–2026




2026 llms.txt adoption data — 11.5% of top 10k sites

	

2/5

	

2026-09-26




Stack Overflow — "PyPi download counts seem unrealistic"

	

2/5

	

-




Zep blog — Stop Using RAG for Agent Memory (AI Engineer WF 2025)

	

3/5

	

2025




BrainTube — Best Memory Tools for Claude Code (2026): 8 Options Compared

	

2/5

	

2026

Conflicts and caveats. PulseMCP "est. visitors" are proprietary estimates, not measurements; pepy totals include mirrors and are all-time sums, not comparable to weekly without_mirrors figures. The jlowin.dev download quotes are maintainer claims. Serena's growth narrative rests on secondary research (Ry Walker), not on maintainer documentation. Where a search-result link points to a snippet-level source, the underlying claim was taken from that snippet, not from a full-page read.

## LINKS
https://pypistats.org/api/packages/basic-memory/overall?mirrors=false
https://api.npmjs.org/downloads/range/2025-04-18:2026-10-18/mcp-remote
https://pepy.tech/projects/basic-memory
https://jlowin.dev/blog/fastmcp-3-launch
https://pepy.tech/projects/fastmcp
https://www.aipotluck.org/product/fastmcp
https://www.firecrawl.dev/blog/fastmcp-tutorial-building-mcp-servers-python
https://pypistats.org/api/packages/fastmcp/recent
https://rywalker.com/research/serena
https://pypistats.org/api/packages/serena-agent/overall?mirrors=false
https://pepy.tech/projects/serena-agent
https://www.pulsemcp.com/servers/basicmachines-memory
https://hn.algolia.com/api/v1/search?query=basic-memory&tags=story
https://hn.algolia.com/api/v1/search?query=graphiti&tags=story
https://arxiv.org/abs/2501.13956
https://pypistats.org/api/packages/graphiti-core/overall?mirrors=false
https://pepy.tech/projects/graphiti-core
https://arxiv.org/abs/2410.05779
https://pypi.org/project/lightrag-hku/1.0.0/
https://pypistats.org/api/packages/lightrag-hku/overall?mirrors=false
https://pepy.tech/projects/lightrag-hku
https://github.com/open-webui/mcpo
https://raw.githubusercontent.com/open-webui/open-webui/main/pyproject.toml
https://pypistats.org/api/packages/mcpo/overall?mirrors=false
https://pypistats.org/api/packages/cognee/overall?mirrors=false
https://pepy.tech/projects/cognee
https://pypistats.org/api/packages/mcp-server-fetch/overall?mirrors=false
https://hn.algolia.com/api/v1/search?query=mem0&tags=story
https://pypistats.org/api/packages/mem0ai/overall?mirrors=false
https://pepy.tech/projects/mem0ai
https://pypistats.org/api/packages/letta/overall?mirrors=false
https://pypistats.org/api/packages/mcp-memory-service/overall?mirrors=false
https://pepy.tech/projects/mcp-memory-service
https://arxiv.org/abs/2412.13459
https://api.npmjs.org/downloads/range/2025-04-18:2026-10-18/@smithery/cli
https://www.reddit.com/r/SaaS/comments/1mnc3nu/i_analyzed_500_product_hunt_saas_launches_487_are/
https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/
https://www.reddit.com/r/LangChain/comments/1kg5qas/lies_damn_lies_statistics_is_mem0_really_sota_in/
https://pypistats.org/api/packages/basic-memory/overall?mirrors=true
https://packaging.python.org/en/latest/guides/analyzing-pypi-package-downloads/
https://pypistats.org/packages/basic-memory