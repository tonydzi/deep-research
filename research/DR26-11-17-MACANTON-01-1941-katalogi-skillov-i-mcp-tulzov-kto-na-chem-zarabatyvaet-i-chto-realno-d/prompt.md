# DR26-11-17-MACANTON-01-1941 — the exact prompt sent to every model

# DR26-11-17-MACANTON-01-1941 — Каталоги скиллов и MCP-тулзов: типы, бизнес-модель, трафик, входы

# Deep Research: the economics and mechanics of AI-agent skill & tool catalogs (MCP registries, skill marketplaces, awesome-lists), November 2026

## Who asks and why
We are a two-person open-source lab (GitHub: tonydzi, ~55 public repos) that gives away everything for free: Agent Skills (SKILL.md folders; one bundle repo `tonydzi/join-the-fleet` with 232 skills) and tools/MCP servers (e.g. `tonydzi/sqlite-graph-memory`, a local SQLite graph-memory MCP server, already submitted to ~12 catalogs). We want to design three recurring routines: (1) find new placement venues, (2) package each artifact well, (3) distribute it. We need hard facts about the catalog market so the routines target venues that actually bring users, and skip dead or pay-to-play ones.

## Questions (answer each with dated, cited facts)
1. **Taxonomy.** What kinds of catalogs exist as of late 2026 and what does each accept (skills, MCP servers/tools, plugins, or several)? Cover at least:
   - MCP registries/directories: official registry.modelcontextprotocol.io, Glama, Smithery, PulseMCP, mcp.so, mcpservers.org, mcp.directory, Cline/VS Code/Docker MCP catalogs, GitHub MCP registry.
   - Agent-skill catalogs: Anthropic/Claude plugin marketplaces and the Claude plugin directory, skills.sh (Vercel), agentskills.io, OpenAI Codex/ChatGPT plugin & skill submission, Cursor marketplace/directory, other "skills hubs".
   - awesome-lists (awesome-mcp-servers, awesome-claude-skills, awesome-claude-code etc.).
   - Package indexes (PyPI, npm) and GitHub Marketplace.
   Which of these are mirrors/aggregators that auto-import from the official MCP registry or from GitHub?
2. **Business model of each class.** How does each catalog make (or plan to make) money: paid listing / fast-track fees (e.g. mcp.so web-form tier), featured/premium placement, advertising, hosting and proxying of servers (remote MCP hosting), usage telemetry/data, enterprise private registries and governance, revenue share/commission, venture funding and the strategic play (which VC-backed, how much raised, by whom). Separate confirmed facts from inference. Note recent pivots or shutdowns (e.g. Smithery ending its proxy/managed connections in Oct 2026, PulseMCP pausing manual submissions) and why.
3. **Real traffic and impact.** What evidence exists about how much traffic, installs, or GitHub stars a listing actually brings? Find case studies, maintainer blog posts, referrer statistics, Hacker News / Reddit / X discussions, SimilarWeb-type traffic estimates for the catalogs. Which catalogs are dead, spam-flooded, or so overcrowded that a listing is invisible? How do catalogs rank (stars, installs, recency, quality scores, security scans)?
4. **How to get in.** For each important catalog: the open door (PR, form, GitHub issue, auto-import from official registry, CLI publish), requirements (manifest such as server.json / plugin.json / SKILL.md frontmatter, license, README, badges, minimum stars, namespace verification via GitHub/DNS, "human-only" or anti-AI-spam rules), review time, and any rules against automated or bulk submissions. Which ones penalize or ban automated submissions?
5. **Implications for a free open-source giver.** Given we distribute ~55 repos and 232 skills for free: where should skills go versus tools/servers; whether to bundle skills as a Claude Code plugin marketplace; which 5-10 venues give the best reach per hour of effort; what to measure (views vs installs vs first successful use vs stars) and how to measure it; what packaging (one repo per skill vs bundle) works best; what mistakes get people delisted or flagged as spam.

## Output format
- Executive summary (10 bullets max).
- Table: catalog | class | accepts | business model | entry door | requirements | evidence of traffic | status (alive/dead/paused) | source URLs.
- Section per question above.
- "Recommendations for a free open-source distributor" with a ranked venue list.
- Explicitly mark: CONFIRMED (primary source) vs INFERRED vs UNKNOWN. Do not invent revenue, install or traffic numbers; say "not public" where so.
- Every claim with a full https:// URL you actually opened. At least 30 distinct sources, at least 30 searches. Prefer primary sources (official docs, pricing pages, submission pages, GitHub repos/issues, founder posts) and dates in 2025-2026.
- Deliver the complete report as Markdown text directly in this chat (no PDF, no files).
