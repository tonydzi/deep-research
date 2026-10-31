# DR26-10-19-MACANTON-01-0501 — the exact prompt sent to every model

# DR26-10-19-MACANTON-01-0501 — sqlite-graph-memory: путь от 100 к 10 000 установок в неделю

# Deep Research: from 100 to 10,000 weekly installs for a small OSS agent-memory package

Use web search heavily. Cite every number with a URL. Say "not found" instead of guessing. Date range: January 2025 to October 2026.

## Context (so you do not re-derive it)
`sqlite-graph-memory` is a Python package on PyPI, MIT licensed, one SQLite file, no graph database and no server. It gives an agent associative recall over the user's own markdown notes: dense retrieval for entry points, the user's hand-written wikilinks as the graph, a cross-encoder reranker on top. It also ships an MCP server (`sgm-mcp`). Status: pilot. Today: version 0.3.0 on PyPI, 113 downloads in its first week (78 on release day, 4 per day by day four), 9 GitHub stars, 6 forks (all with zero commits ahead), one external contributor who found us through a sibling repo of the same topic. It is listed in no MCP directory yet. Closest comparable, `basic-memory`, has about 7,700 PyPI downloads per week and about 4,300 stars. Bigger neighbours: mem0, graphiti-core, cognee, LightRAG.

## Questions
1. Which small open-source packages for agent memory, RAG, or MCP servers reached 10,000+ weekly PyPI/npm downloads during 2025-2026, starting from a base similar to ours (under 100 stars, under 500 downloads a week)? Give at least 8 cases with package name, the week the curve bent, and the numbers before and after, each from a source (pypistats, ClickPy, npm-stat, a maintainer post).
2. For each case, what caused the bend? Separate: a listing in a directory or registry (official MCP Registry, Glama, Smithery, PulseMCP, awesome lists), an integration inside a bigger product (Claude Desktop, Cursor, Codex, VS Code, a framework default or tutorial), a benchmark or comparison post, a Hacker News or Reddit thread, a single influencer mention, a dependency of another popular package. Say which ones you could verify by timestamp and which are the maintainer's own story.
3. In what order did the winners do things, and how many weeks from first install command to 1,000, then to 10,000 per week?
4. What did NOT work, with measured evidence: bought stars, mass directory submissions, Product Hunt, llms.txt, drive-by benchmark posts against a competitor, mass PRs into other repos. Which of those backfired?
5. How much of the download count of tiny packages is mirrors, CI and bots? Give the typical share and how the cases above filtered it (without_mirrors, ClickPy human classification, unique clones).
6. Give a ranked list of the five cheapest moves for a package at our base, each with a cost in hours, an expected effect with its evidence, and a kill criterion measured after 30 days.

## Output
A report with one table of cases (columns: package, base, peak, week of bend, cause, evidence URL, confidence), then the ordered plan, then a section "claims I could not verify". No marketing language.
