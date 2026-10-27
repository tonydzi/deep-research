# DR26-10-19-MACANTON-01-0501 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll pull live download curves and the posts that sit next to them, then write the report from those pages only.The recent-download APIs are live. Next I’ll pull weekly series for the small memory and MCP packages and the posts dated next to each bend.I'll pull the unfinished data job and reopen the sources the report will cite.The unread job came back. Next I’ll recompute the download weeks and open the pages the table will cite.A. What was actually found

The strict filter was met for zero packages. A case had to be an agent-memory, RAG, or MCP package, reach at least 10,000 downloads in one week during January 2025 to October 2026, and start from under 100 GitHub stars and under 500 downloads in a week. No package had both the star base and the download base verified from a page opened in this run.

Four npm packages have a download series that starts under 500 and later crosses 10,000 in a week. Three of them are reference servers in one official repository, `modelcontextprotocol/servers`, created 2024-11-19 and holding 91,381 stars on 2026-10-19. Their first jump sits on the week of 2024-11-25. That is a protocol launch, not a quiet pilot that later bent. The fourth, `mcp-remote`, is a standalone package. Its download base is verified. Its star count in the week it was under 500 downloads was not retrieved.

Every Python package queried on pypistats was already above 500 downloads in the first returned week. The `overall` endpoint, pulled 2026-10-19 with `mirrors=false`, starts on 2026-04-08 and ends on 2026-10-18. Weeks before 2026-04-08 are not in that response. The first bucket, 2026-W15, is Wednesday 2026-04-08 through Sunday 2026-04-12, five days. Later buckets below are seven days unless noted.

`sqlite-graph-memory` on the same day: last day 4, last week 113, last month 0. The recent endpoint returned those three numbers together. No pypistats page opened here explains a last-month of 0 beside a last-week of 113. Source: https://pypistats.org/api/packages/sqlite-graph-memory/recent

Week totals below are sums of daily rows, computed in this run. They are not a weekly field in the API. Week labels use Monday-start, first-four-day weeks. The date span is the evidence. The label is only a name.

On 2026-08-24 PyPI stopped logging fetches that are not a `.whl`, `.tar.gz`, or `.zip`. For `urllib3`, about 39 percent of counted rows in the days 2026-08-18 through 2026-08-23 were not distribution files. Counts before and after 2026-08-24 do not measure the same thing. Source: https://blog.pypi.org/posts/2026-08-31-download-counts/

B. Table

| Package | Base | Peak in the window pulled | Week of the 10k cross | Cause | Evidence | Confidence |
| --- | --- | --- | --- | --- | --- | --- |
| mcp-remote (npm) | 337 in 2025-03-17..2025-03-23. Stars that week: not found. Repo now 1,635 stars, created 2025-03-20. | 61,413 in 2025-06-02..2025-06-08 inside the early window. 2,901,098 in 2026-08-24..2026-08-30 inside the late window. July 2025 through July 2026 not pulled. | 11,228 in 2025-05-05..2025-05-11. 1,958 the week before the cross was already past, in 2025-03-24..2025-03-30. | README install line for Claude Desktop, Cursor, and Windsurf (`npx` plus `mcp-remote`). Not matched to a dated external post in the bend week. | https://api.npmjs.org/downloads/range/2025-03-01:2025-06-30/mcp-remote and https://api.npmjs.org/downloads/range/2026-08-01:2026-10-18/mcp-remote and https://raw.githubusercontent.com/punkpeye/mcp-remote/main/README.md and https://api.github.com/repos/punkpeye/mcp-remote | High on the two windows. Low on the cause. Star base unverified. |
| @modelcontextprotocol/server-filesystem | 124 in 2024-11-18..2024-11-24. Parent repo stars now 91,381. Stars that week: not found. | 66,359 in 2025-03-24..2025-03-30. Series pulled only through 2025-03-31. | 10,627 in 2025-02-24..2025-03-02. Passed 1,000 at 4,542 in 2024-11-25..2024-12-01. | Shipped in the official servers repo. The 1,000-download week starts 2024-11-25. A primary launch-announcement URL was not opened in this run. | https://api.npmjs.org/downloads/range/2024-11-15:2025-03-31/%40modelcontextprotocol%2Fserver-filesystem and https://api.github.com/repos/modelcontextprotocol/servers | High on downloads inside the window. The package fails the small-repo filter. |
| @modelcontextprotocol/server-memory | 124 in 2024-11-18..2024-11-24. Same parent repo. | 17,616 in 2025-04-21..2025-04-27, which is also the 10k week. Series pulled only through 2025-05-31, so a later peak is not in this pull. | 17,616 in 2025-04-21..2025-04-27. Passed 1,000 at 1,831 in 2024-11-25..2024-12-01. | Same repository and the same first two weeks as server-filesystem. | https://api.npmjs.org/downloads/range/2024-11-15:2025-05-31/%40modelcontextprotocol%2Fserver-memory | High on this window. Same launch, not a second growth story. |
| @modelcontextprotocol/server-everything | 202 in 2024-11-18..2024-11-24. Same parent repo. | 10,814 in 2025-06-23..2025-06-29. Series pulled through 2025-07-15. | 10,696 in 2025-06-16..2025-06-22. Passed 1,000 at 2,946 in 2025-03-17..2025-03-23. | Same repository. Slower slope after the same first week. No post was matched to the June 2025 cross. | https://api.npmjs.org/downloads/range/2024-11-15:2025-07-15/%40modelcontextprotocol%2Fserver-everything | High on this window. Fails the small-repo filter. |
| basic-memory (PyPI, without mirrors) | First returned bucket already 5,154 in 2026-04-08..2026-04-12 (5 days). 1,004 stars by 2025-06-30, maintainer's count. 4,279 stars on 2026-10-19. | 19,659 in 2026-05-25..2026-05-31. Last full week 7,671 in 2026-10-12..2026-10-18. | 10,227 in 2026-04-20..2026-04-26. | Maintainer's account of the star milestone is six months of releases. No download number is in that post. No cause was matched to the April 2026 cross. | https://pypistats.org/api/packages/basic-memory/overall?mirrors=false and https://pypistats.org/api/packages/basic-memory/recent and https://basicmachines.co/blog/100-github-stars/ and https://api.github.com/repos/basicmachines-co/basic-memory | High on 2026 weeks. The pre-2026 download base is not in the endpoint. Fails both base tests at the dates we can see. |

Neighbours, current week, `without_mirrors`, pulled 2026-10-19. These are not bend cases. Their first returned week is already far above 500.

- `mem0ai` last week 605,555. First bucket in the window, the 5-day week of 2026-04-08, is 416,042. https://pypistats.org/api/packages/mem0ai/recent
- `graphiti-core` last week 197,676. First bucket 77,437. Repo created 2024-08-08, 32,044 stars now. https://pypistats.org/api/packages/graphiti-core/recent and https://api.github.com/repos/getzep/graphiti
- `lightrag-hku` last week 34,650. First bucket 29,057. https://pypistats.org/api/packages/lightrag-hku/recent
- `cognee` last week 25,074. First bucket 12,288. https://pypistats.org/api/packages/cognee/recent
- `fastmcp` last week 13,572,504. First bucket 11,530,174. https://pypistats.org/api/packages/fastmcp/recent
- Python package `mcp` last week 64,748,152. https://pypistats.org/api/packages/mcp/recent

Spikes inside the same PyPI window that did not stay up. Cause of each spike: not found.

- `memori`: 11,828 in 2026-04-20..2026-04-26, max 136,932 in 2026-07-20..2026-07-26, last week 3,522. The peak is before the 2026-08-24 counting change. The last week is after it.
- `letta`: max 117,023 in 2026-08-10..2026-08-16, last week 2,967. Same break in the definition.
- `hindsight-all`: max 21,598 in 2026-07-27..2026-08-02, last week 2,235. Same break.
- `mcp-memory-service`: the only week at or above 10,000 is 21,303 in 2026-08-24..2026-08-30, which is the first week of the new counting rule, then 2,363 in 2026-10-12..2026-10-18. That drop is inside one definition.
- `mcp-agent`: one week at 11,613 in 2026-04-27..2026-05-03, last week 9,840.

C. What the bend was attached to

`mcp-remote`. The README says Claude Desktop, Cursor, and Windsurf are stdio clients, and it gives one config block whose command is `npx` and whose argument is `mcp-remote` plus a remote URL. It also says to remove the package once the client speaks remote MCP itself. That is a dependency of other people's config files. The homepage field on the GitHub repo is `https://glama.ai/mcp/servers`. A directory URL on the repo today does not date the May 2025 bend. No Hacker News thread, Reddit thread, benchmark post, or registry announcement was opened with a timestamp inside 2025-03-24..2025-03-30 or 2025-05-05..2025-05-11. The cause is the maintainer's own README. The weeks are from the npm API. The link between them is not a matched timestamp.

Official reference servers. `server-memory`, `server-filesystem`, and `server-everything` share a first week of 124, 124, and 202. Two of them cross 1,000 in the next week, the week that starts 2024-11-25. They live in one repo. Counting them as three growth stories would triple one distribution event. Their later cross of 10,000 is 14 weeks later for filesystem (week of 2025-02-24), 22 weeks later for memory (week of 2025-04-21), and 30 weeks later for everything (week of 2025-06-16). No single post was opened that lands on those three later weeks.

`basic-memory`. The 2025-06-30 post says 1,004 stars, six months from the first commit, 44 releases, 795 commits, 9 contributors. It gives documentation page views and no PyPI downloads. The download cross of 10,000 is the week of 2026-04-20, nine months after that post, from a base that was already 5,154 in a five-day bucket. Maintainer's story for stars. Download bend unverified as to cause.

Graphiti, maintainer's story, not a small-base case. On 2025-07-16 Daniel Chalef wrote that the project had nearly 14,000 stars in eight months and 25,000 weekly PyPI downloads, and he attributed the use to the Graphiti MCP server for Claude and Cursor. By the date of the 25,000 claim, the star count in his own sentence is already about 14,000. The same site's later card says 20,000 stars on 2025-11-10. Current week is 197,676 without mirrors, and current stars are 32,044. The July 2025 weekly number is his claim. It is outside the pypistats window pulled here. Source: https://www.getzep.com/blog/graphiti-knowledge-graphs-falkordb-support/

FastMCP, maintainer's story, stars only. Jeremiah Lowin, 2025-05-16: about six weeks from the 2.0 relaunch to 10,000 stars, six months from the first 1.0 commit, and 1.0 was adopted into the official Python SDK. By the first pypistats bucket, 2026-04-08..2026-04-12, downloads are already 11,530,174. A week under 500 downloads was not retrieved. Source: https://jlowin.dev/blog/fastmcp-2-10k-stars

Directories. A before-and-after download study of a listing on the official MCP registry, Glama, Smithery, PulseMCP, or an awesome-list was not found. What was found is the shape of the official registry. DevToolHub counted the registry API on 2026-09-10: 30,375 unique servers, 407 first published in September 2025, 6,265 first published in August 2026, no install or usage field in the schema, `com.anthropic` at 0 entries, `io.github.modelcontextprotocol` at 0 entries. The author's conclusion on that page is that discovery of the servers people actually run still goes through docs and word of mouth. Source: https://devtoolhub.com/mcp-registry-by-the-numbers/

SDK scale is a different object from a tool's installs. The 2025-12-09 MCP post says "over 97 million monthly SDK downloads" and names first-class support in ChatGPT, Claude, Cursor, Gemini, Copilot, and VS Code. That sentence is Anthropic's, dated December 2025. The Python package `mcp` last week, measured here, is 64,748,152. Source: https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/

D. Order, and weeks from the first measured install week

`mcp-remote`. npm registry `time.created` is 2025-03-17. The GitHub repo that the API returns today was created 2025-03-20. Version 0.0.1 on the registry is published by npm user `geelen`. The order visible in documents opened here is: publish the proxy, document one config block for the three stdio clients. Whether a directory listing existed in March 2025 was not checked.

From the week of 2025-03-17 (337) to the week of 2025-03-24 (1,958) is one week. From the week of 2025-03-17 to the week of 2025-05-05 (11,228) is seven weeks. Last full week pulled, 2026-10-12..2026-10-18, is 505,267. The path from 11,228 to that level was not week-by-week in this run, because 2025-07-01 through 2026-07-31 was not requested.

Official servers. Order visible in the data: the repo exists (created 2024-11-19), downloads start the week of 2024-11-18 at under 500, the next week is already in the thousands for filesystem and memory. Time from the first measured week to 1,000 per week: one week for filesystem and memory, about 17 weeks for everything (week of 2025-03-17). Time from the first measured week to 10,000 per week: about 14, 22, and 30 weeks, in the order filesystem, memory, everything. Nothing opened here shows a directory submission, a Product Hunt launch, or a benchmark post sitting on those later weeks.

`basic-memory`. Order in the maintainer's post: first commit, then 44 releases, then 1,004 stars at month six (post dated 2025-06-30). Repo `created_at` is 2024-12-02. Weeks from first install command to 1,000 downloads per week: not found. Weeks to 10,000: not found from the first install. Inside the retained window the cross from a five-day 5,154 to a full-week 10,227 takes two weeks, and the package is back under 10,000 by the week of 2026-10-12.

No package opened here shows the sequence "list in five directories, then the curve bends" with dates on both events.

E. What was measured, and what was not a download result

Bought stars. A field experiment bought stars for new Python packages so the median moved from 0 to about 20-65. Three months later the authors could not reject a null effect on PyPI downloads, forks, pull requests, or issues. A second experiment raised median download counts from about 20 to about 100 and found no detectable effect on later downloads. Source: https://arxiv.org/html/2603.07919v1

Fake-star campaigns, observational, 2019-2024. StarScout flagged about 6.0 million suspected fake stars across 18,617 repositories. In the panel model, a 1 percent increase in fake stars is associated with about a 0.07 percent increase in real stars the next month, against about 0.36 percent for a 1 percent increase in real stars. After two months the fake-star coefficient turns negative, about a 0.04 percent decrease in later real stars. The authors say this is not strong causal proof. The outcome is later real stars, not PyPI downloads. Source: https://arxiv.org/html/2412.13459v2

`llms.txt`. Ahrefs, 15 June 2026, 137,210 domains with traffic in May 2026: 97 percent of the `llms.txt` files received zero requests that month, and 96 percent of the requests that did arrive were bots. SE Ranking, 15 October 2026, about 300,000 domains, 10.13 percent adoption: no correlation with AI citations, and an XGBoost model got more accurate when the `llms.txt` variable was removed. Both studies measure fetches and citations. A PyPI or npm install effect was not in either paper. Sources: https://ahrefs.com/blog/llmstxt-study/ and https://seranking.com/blog/llms-txt/

Product Hunt. A study that measures Product Hunt against later weekly PyPI or npm downloads was not found in this run.

Drive-by benchmark posts against a competitor. A study that measures those posts against later installs was not found in this run.

Mass directory submissions. A before-and-after install study was not found. The registry count above is evidence that a listing is common and that the registry does not store an install number. It is not a measured effect on one package.

Mass pull requests. Two opened sources measure maintainer load. They do not measure the sender's installs. On 19 March 2026 the maintainer of `awesome-mcp-servers` reported that a canary line in `CONTRIBUTING.md` was followed by 21 of 40 new pull requests in 24 hours, and he estimated the bot share closer to 70 percent. That 70 percent is his estimate. On 18 June 2026 GitHub's blog said monthly merged pull requests went from about 25 million in January 2023 to more than 90 million, and that pull requests opened by Copilot or another AI agent count toward a new per-user cap. A measured drop in package downloads after a spray of pull requests was not found. Sources: https://glama.ai/blog/2026-03-19-open-source-has-a-bot-problem and https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/

Which of these backfired, in the sources opened. Bought stars: no download effect in the experiment. In the observational star study, a later negative association with real stars. `llms.txt`: no citation gain in the SE Ranking model. Mass pull requests: maintainer hostility and a platform cap, not a measured install penalty. Product Hunt, directory blasts, and attack-benchmark posts: no install result either way, so "backfired" is not found.

F. Mirrors, CI, bots

`without_mirrors` on pypistats drops downloads whose client is `bandersnatch`, `z3c.pypimirror`, Artifactory, or `devpi`. CI downloads stay in. The FAQ also says a release day brings a burst of mirror syncs, and that mirrors can inflate less common packages because the mirror does not report the later installs it serves. Source: https://pypistats.org/faqs

Share of total PyPI traffic, one day, Hugo van Kemenade's table in the 13 May 2025 thread: pip 72.76 percent, uv 23.18 percent, bandersnatch 0.23 percent, devpi 54,966 downloads, Artifactory 38,664, on a stated total of 1,854,452,670. Named public mirror clients are a fraction of a percent of that day. Source: https://discuss.python.org/t/pypi-downloads-statistics-and-continuous-integration/91810

CI share. In that same thread Kevin Fleming recalled CI as more than 80 percent of pypi.org traffic over about a week, from a PyCon 2024 talk. On 15 May 2025 another participant watched the talk he linked and reported the slide as 27 percent CI over one day. Package-level user-agent flags in the same thread: Wagtail about 10 to 30 percent CI by month, FastAPI about 20 to 50 percent. Those flags exist only when pip or uv sets them. A single "typical share" for a package at about 100 downloads a week was not found.

The cases above were not filtered to humans. npm's downloads API, as used here, has no CI switch. pypistats numbers above are `without_mirrors` only. ClickPy's mirror filter is an open request, issue 136, filed 4 April 2025, and the issue says the CI filter is a separate open request, issue 109. A shipped ClickPy "human classification" was not found. Source: https://github.com/ClickHouse/clickpy/issues/136

Unique clones. GitHub's clone endpoint returns `count` and `uniques` for 14 days, and only with write access to the repo. Source: https://docs.github.com/rest/metrics/traffic . No public unique-clone series was retrieved for any package in the table.

`sqlite-graph-memory`'s week of 113 falls after 2026-08-24, so it is artifact downloads. It still includes CI, caches, and any mirror client outside the four names pypistats drops. The human share of that 113 was not found.

G. Five cheapest moves, ranked

Hour figures are judgment of the work, not a measured duration from a source. The kill line uses `without_mirrors` weekly downloads. Current base is 113. Treat 50 to 200 as the noise band until a week leaves it. Stars are the wrong instrument: the experiment in section E found no download effect from moving a new package from 0 stars to 20-65.

1. One config block that an existing client doc already tells people to paste. Cost: about 8 to 20 hours to make the `uvx` or `pip` line true on Claude Desktop and one other client, then one issue or one invited edit in a doc that already has an install section. Evidence: `mcp-remote` went from 337 to 1,958 in one week and to 11,228 in seven weeks, and the only distribution mechanism in the README is that paste. Expected effect for a memory package: the multiple is not transferable. `mcp-remote` is the bridge every remote server needed. A memory tool is not. The evidence says this is the only measured route from under 500 to over 10,000 that was not the protocol launch itself. Kill at 30 days: weekly downloads still under 300, and no external page contains the install line.

2. The next four releases, aimed at the one external contributor, with the install line from move 1 in the release notes. Cost: about 20 to 40 hours across the month. Evidence: Basic Memory's maintainer credits 44 releases in six months for the path to 1,004 stars. That post has no download series. Their later download peak, 19,659, did not stay. Last week is 7,671. Expected effect: a star path on the order of months, documented for one project. A download path to 10,000 a week is not in that post. Kill at 30 days: still one external person, and the weekly number still inside 50 to 200.

3. One submission to the official MCP registry, with the source repo linked, and no second directory in the same month. Cost: about 2 to 4 hours. Evidence of an install lift: not found. Evidence that the registry is not where installs are counted: the 2026-09-10 census found no install field, 30,375 servers, and zero entries under `com.anthropic` and `io.github.modelcontextprotocol`. Expected effect on the weekly number: not found, so it is not estimated. Kill: if the registry row is the only new surface at day 30 and the weekly number is still inside 50 to 200, do not submit to Glama, Smithery, PulseMCP, or an awesome-list next. The census is the reason the second submission is the thing to stop.

4. Do not buy stars and do not script downloads. Cost: 0 hours. Evidence: the two experiments in section E, null at three months for stars and for a quintupled download count. Expected effect of spending the month on either: none detected. Kill: the result is already in. A proposal to buy stars inside this 30 days is the signal to drop it.

5. Do not write `llms.txt`, a Product Hunt launch, or a post whose point is to rank this package against `basic-memory`. Cost of skipping them: 0 hours. Evidence: section E. `llms.txt` had a measured null on fetches and on citations, and no install study. Product Hunt and attack-benchmark posts had no install study at all. Expected effect on weekly downloads: not found. Kill: if one of the three has been published and day 30 is still inside 50 to 200, stop that lane and do not publish the other two.

Moves that are real for other projects and unavailable here. Adoption into the MCP Python SDK is the maintainer's account of FastMCP 1.0. A pilot package does not get that by asking. Becoming the paste target for every remote server is the `mcp-remote` position, and it is already occupied.

H. Claims I could not verify

- Eight packages that started under 100 stars and under 500 downloads a week and later cleared 10,000 a week. Verified count: zero. Download-base-only: the four npm rows in the table. Star count in the base week: not retrieved for any of them. GitHub's star history was not queried.
- Weekly PyPI series before 2026-04-08. The endpoint did not return them. That covers the early bend, if any, for `basic-memory`, `graphiti-core`, `mem0ai`, `cognee`, `lightrag-hku`, `fastmcp`, `graphrag`, and `hindsight-client`.
- `graphiti-core` at 25,000 weekly downloads on 2025-07-16. One maintainer sentence. Not recomputed from pypistats.
- FastMCP at 10,000 stars in six weeks. Maintainer's post. A download week under 500 for FastMCP was not retrieved. The "70 percent of MCP servers" line on the PyPI page was not opened in this run.
- npm weekly series for `mcp-handler`, `@upstash/context7-mcp`, `@playwright/mcp`, `chrome-devtools-mcp`, `@notionhq/notion-mcp-server`, `@supabase/mcp-server-supabase`, and the npm package `fastmcp`. Not pulled. An earlier attempt in this project hit HTTP 429. Those curves are not in this report.
- `mcp-remote` downloads from 2025-07-01 through 2026-07-31. Not pulled. The 2,901,098 week is the max of the late window only.
- Peaks of the official servers after the end of each pulled range (filesystem after 2025-03-31, memory after 2025-05-31, everything after 2025-07-15).
- The primary URL of the 25 November 2024 MCP announcement. Not opened. The week of 2024-11-25 is a fact in the npm data. Calling it the launch week is an identification I did not re-check against the announcement page.
- ClickPy's chart series, pepy.tech's CI-filtered series, and any "human" share for these packages. ClickPy issue 136 is still a request. pepy's CI toggle is described in the Python discussion as a paid switch. Neither was used for a number.
- A 70 percent mirror share for the package `skore` on ClickPy. Not re-opened. Omitted from the figures above.
- Unique clones for any repo in the table.
- A download effect from Product Hunt, from a mass directory submission, from a drive-by benchmark post, or from a spray of pull requests.
- Hacker News points to install counts. A paper on stars after a Hacker News launch was not opened in this run, so it is not cited.
- Why `memori`, `letta`, `hindsight-all`, `mcp-use`, `honcho-ai`, and `mcp-memory-service` spiked and fell. The weeks were computed. The cause was not found. Part of the fall for the first three sits across the 2026-08-24 definition change.
- `sqlite-graph-memory` last month 0 beside last week 113. Observed. Unexplained here.
- The 113 includes how many humans. Not found.

Coverage. Pages and APIs opened in this run are the URLs in sections A through G. Week sums were computed from those API responses on 2026-10-19. No file was written for this report. Numbers that lived only in an earlier pass, and were not pulled again, are in section H or are absent.
