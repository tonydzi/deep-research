# DR26-10-19-MACANTON-01-0501 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

sqlite-graph-memory: путь от 100 к 10 000 установок в неделю — what the 2025–2026 evidence actually supports

I could not verify a single package that strictly meets your criterion: under 100 stars and under 500 downloads a week before the bend, then 10,000+ a week, with dated download data from January 2025 to October 2026. The packages that did reach 10k+/week (task-master-ai, blender-mcp, Desktop Commander, serena-agent, Context7) either started with a founder who already had an audience, had a funded company behind them, already had thousands of stars before PyPI downloads took off, or have a bend I can only date from the maintainer’s own posts. Plan for 1,000 real (non-mirror, non-CI) downloads a week as the next milestone, not 10,000. Your closest comparable, basic-memory, is still below 10k/week.

TL;DR
Strict cases: 0 verified. Near-misses: 7. The two fastest cases, task-master-ai and blender-mcp, went from first release to 10k+/week within roughly 5–14 weeks in spring 2025. In both, the trigger was the author’s own launch post with a demo on X, plus one-paste setup for Cursor and Claude. The timing comes only from the maintainers’ posts; I found no dated weekly download series for either.
What failed or backfired is better documented than what worked. Bought stars have a promotion effect of less than two months and become a burden afterwards (StarScout). 
beehaw

 Agent-generated PRs to awesome-mcp-servers are now flagged and deprioritised. In Ahrefs’ May 2026 server logs from 137,210 domains, 97% of llms.txt files got zero requests. The Zep-vs-Mem0 LoCoMo benchmark attack ended in a public self-correction from 84% to 75.14% by Zep and a counter-run at 58.44% by Mem0’s CTO, both in getzep/zep-papers issue #5.
Your release-day 78 is mostly automation. pypistats documents that mirrors download every new release on release day. 
pypistats

 Your ~4/day tail, about 28 a week, is your real baseline. The five cheapest moves, in order: measure properly, list in the official MCP Registry, ship one-command installs, publish one demo-led launch post, and target Obsidian users. Each has a kill criterion at 30 days.
Key Findings
Cases table

Strictly qualifying cases (under 100 stars and under 500/week before the bend, 10k+/week reached between Jan 2025 and Oct 2026, dated download source): none found.

Near-misses, each with the reason it misses:

package	base	peak	week of bend	cause	evidence URL	confidence
task-master-ai (npm)	0 downloads at launch (~late Mar 2025); stars before the bend not found	10k/week by 2025-05-03; 40k/week by 2025-06-06 (maintainer); 16,081/week now (npm) 
npmjs
 +2

	ISO 2025-W14 to W18 (maintainer story only)	Founder launch post on X (intro post shows 343K views); MCP mode for Cursor shipped 2025-04-11; 
x

x

 editor integrations (Cursor, Windsurf, Lovable, Claude Code). Well-known maintainer with an audience. Misses: no dated download series; base stars unknown	https://threadreaderapp.com/thread/1924642024566706304.html ; https://x.com/EyalToledano/status/1930998914192158899 ; https://www.npmjs.com/package/task-master-ai	Medium (self-reported)
blender-mcp (PyPI, renamed mcp-for-blender)	First PyPI release 0.1.0 on 2025-03-08; repo created 2025-03-07 
star-history

piwheels

	~200k cumulative by 2025-06-12 (author); 190,831 last week (pypistats, undated snapshot); 646,123 last 30 days (pepy, undated) 
pypistats
 +2

	ISO 2025-W11 (week of 2025-03-10), inferred from the author’s launch post on 2025-03-11	Author’s demo video on X (“Claude talk directly to Blender”); uvx one-line config for Claude Desktop and Cursor. 
x

pypi

 Solo maintainer, no prior company. Misses: no dated weekly series, so the bend week is inferred	https://pypi.org/project/blender-mcp/1.1.1/ ; https://x.com/sidahuj/status/1899460492999184534 ; https://pypistats.org/packages/blender-mcp ; https://pepy.tech/projects/blender-mcp	Medium-low
@wonderwhy-er/desktop-commander (npm)	Released Dec 2024 (before window); 
medium

 base not found	178,685/week (npm); “325k+ weekly” (maintainer site) 
desktopcommander

npmjs

	~2025-04 (HN + Product Hunt per maintainer post dated 2025-04-08); exact week not found	Maintainer content (80K views), Hacker News, Product Hunt; later Smithery, Docker MCP catalog, Claude connectors. Became a funded company (€1.1M pre-seed, Oct 2025). 
ain

medium

 Misses: started before 2025; no dated series	https://wonderwhy-er.medium.com/from-80k-views-to-hacker-news-and-product-hunt-desktop-commander-catches-the-mcp-wave-199ebf935840 ; https://www.npmjs.com/package/@wonderwhy-er/desktop-commander ; https://en.ain.ua/2025/10/28/desktop-commander-raises-eur11m/	Low for bend timing
serena-agent (PyPI)	166/week with 9.3k stars (ClickPy snapshot, undated, version 0.1.4) 
clickhouse

	24,842/week (mcpbeat); ~136,000/month (GitLab, Aug 2026) 
mcpbeat

gitlab

	not found	Usage existed long before PyPI counted it (git/uvx installs). Backed by a company (Oraios). 
github

 Misses: stars far above 100 at base	https://clickpy.clickhouse.com/dashboard/serena-agent ; https://mcpbeat.com/mcp-servers/oraios/serena/ ; https://about.gitlab.com/blog/critical-rce-in-serena/	Low
@upstash/context7-mcp (npm)	not found	429,013/week (npm); ~554k/week (third-party, Aug 2026) 
npmjs

agenticcontrolplane

	not found	Funded vendor (Upstash); README install snippets for every client; Claude Code plugin and .mcpb bundle. 
npmjs

decodo

 Misses: vendor-backed; no base data	https://www.npmjs.com/package/@upstash/context7-mcp ; https://agenticcontrolplane.com/mcp-controls/context7	Low for bend
claude-mem (npm)	not found	~95–101k stars; 
star-history

skillsllm

 npm weekly not reliably reported	not found	Claude Code plugin marketplace; #1 GitHub Trending repository of the day. Misses: the main install path (Claude Code /plugin) does not register as npm installs, so npm counts understate adoption 
npmjs

star-history

	https://www.star-history.com/thedotmack/claude-mem/ ; https://npm.io/package/claude-mem	Low
basic-memory (PyPI), your comparable	not found for 2025	4,927/week (pypistats snapshot when latest version was 0.16.2, ~Nov 2025) → 7,671/week (pypistats, fetched Oct 2026) 
pypistats

pypistats

	n/a (never crossed 10k/week)	Company-backed (Basic Machines); steady release cadence. 
pypi

pypistats

 Misses: below 10k/week	https://pypistats.org/packages/basic-memory	High for current figure; medium for the earlier snapshot date

## LINKS
https://beehaw.org/post/17683690
https://pypistats.org/faqs
https://www.npmjs.com/package/task-master-ai
https://x.com/eyaltoledano
https://x.com/EyalToledano/status/1910782896870137944
https://threadreaderapp.com/thread/1924642024566706304.html
https://x.com/EyalToledano/status/1930998914192158899
https://www.star-history.com/ahujasid/mcp-for-blender/
https://www.piwheels.org/project/blender-mcp/
https://pypistats.org/packages/blender-mcp
https://x.com/sidahuj/status/1899460492999184534
https://pypi.org/project/blender-mcp/1.1.1/
https://pepy.tech/projects/blender-mcp
https://wonderwhy-er.medium.com/from-80k-views-to-hacker-news-and-product-hunt-desktop-commander-catches-the-mcp-wave-199ebf935840
https://desktopcommander.app/mcp/
https://www.npmjs.com/package/@wonderwhy-er/desktop-commander
https://en.ain.ua/2025/10/28/desktop-commander-raises-eur11m/
https://clickpy.clickhouse.com/dashboard/serena-agent
https://mcpbeat.com/mcp-servers/oraios/serena/
https://about.gitlab.com/blog/critical-rce-in-serena/
https://github.com/oraios/serena
https://www.npmjs.com/package/@upstash/context7-mcp
https://agenticcontrolplane.com/mcp-controls/context7
https://decodo.com/blog/context7-mcp
https://www.star-history.com/thedotmack/claude-mem/
https://skillsllm.com/skill/claude-mem
https://www.npmjs.com/package/claude-mem
https://npm.io/package/claude-mem
https://pypistats.org/packages/basic-memory
https://pypi.org/project/basic-memory/0.2.1/
https://dev.to/yonatan_naor_5642e43447ea/building-an-mcp-calculator-server-from-zero-to-100-downloadsweek-27i5
https://dev.to/om_shree_0709/i-built-an-mcp-agent-framework-for-my-btech-major-project-it-got-750-npm-downloads-in-week-one-14f1
https://dev.to/marras0914/how-i-got-1600-weekly-npm-downloads-with-mcp-tools-292m
https://x.com/sidahuj/status/1933162384190746721?lang=en
https://desktopcommander.app/
https://gravity.fast/data/mcp-adoption-statistics/
https://www.bleepingcomputer.com/news/security/over-31-million-fake-stars-on-github-projects-used-to-boost-rankings/
https://arxiv.org/html/2412.13459v1
https://glama.ai/blog/2026-03-19-open-source-has-a-bot-problem
https://prosopo.io/blog/llms-txt/
https://dev.to/yappatyih/does-llmstxt-work-in-30-days-no-ai-crawler-read-mine-1ajn
https://ariashaw.com/does-llms-txt-actually-work
https://essays.bloo-mind.ai/posts/2026-05-20-mem-eval/
https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/
https://discuss.python.org/t/pypi-downloads-statistics-and-continuous-integration/91810
https://pepy.tech/pepy-api
https://www.tenable.com/blog/how-cyberattackers-inflate-malicious-package-npm-download-counts
https://github.com/ClickHouse/clickpy
https://modelcontextprotocol.io/registry/package-types
https://dev.to/ricardo_cuba/how-i-registered-an-mcp-server-for-3760-retailers-and-what-i-learned-4npm
https://www.securecoders.com/blog/mcp-server-registry-guide
https://roadcoderr.com/publishing-your-mcp-server-to-the-official-registry-a-pragmatic-guide/
https://github.blog/ai-and-ml/generative-ai/how-to-find-install-and-manage-mcp-servers-with-the-github-mcp-registry/
https://www.pulsemcp.com/servers/upstash-context7
https://www.pulsemcp.com/servers/desktop-commander