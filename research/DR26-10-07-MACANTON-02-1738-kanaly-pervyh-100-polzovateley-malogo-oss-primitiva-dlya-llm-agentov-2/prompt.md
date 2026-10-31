# DR26-10-07-MACANTON-02-1738 — the exact prompt sent to every model

# DR26-10-07-MACANTON-02-1738 — Первые 100 реальных пользователей маленького OSS-примитива для LLM-агентов: какие каналы дали измеримый приток, в каком порядке, и что НЕ работало

How did solo engineers and small labs actually get the FIRST 100 real users for a small
open-source primitive aimed at LLM agents, in 2025–2026?

I am not asking for growth-marketing theory. I am asking what measurably worked, for whom,
in what order, with names and numbers. Treat "it got popular" as a claim that needs a
mechanism behind it.

## The situation this has to answer for

A single-maintainer Python primitive: Graph RAG memory for LLM agents on SQLite (vector
search for entry points, hand-written `wikilinks` as the graph, cross-encoder rerank).
MIT, 88 tests, CI, a DOI, an MCP server, and real daily use on one ~100k-note corpus.
8 GitHub stars, 6 forks from strangers, 6 pull requests from 3 strangers, and — until this
week — no `pip install`. The author's problem is not code quality. It is that nobody who has
not already met the author has any path to arriving at it.

## What I need, specifically

1. **Channels with measured inflow.** For each channel, name projects that used it and what
   it produced in numbers (downloads, installs, inbound issues — not stars where you can
   avoid it). Candidate channels to confirm or kill: PyPI/npm presence itself · awesome-list
   inclusion · MCP server registries and directories (this is new in 2025–2026 — I want to
   know whether listings there actually move installs or are a dead mall) · answering in
   other people's GitHub issues and Discussions · Show HN · Lobsters · r/LocalLLaMA and
   similar subreddits · Hacker News comment sections rather than submissions · X/Twitter
   threads · dev.to / Medium write-ups · YouTube walkthroughs · conference lightning talks ·
   being vendored into a larger framework · arXiv/JOSS papers with code · Discord/Slack
   communities of adjacent tools · appearing in LLM training data or being cited by
   assistants when users ask "how do I give my agent memory".

2. **The ORDER.** What came first, second, third, for projects that went from ~0 to real
   adoption. I specifically want to know whether packaging (`pip install`) before or after
   distribution attempts changed the outcome, and whether there is a documented threshold
   below which outreach is simply wasted.

3. **What did NOT work.** Separate this explicitly and give it equal weight. I am especially
   interested in: cold PRs into large repositories (we got 8 of 9 closed in one repo by
   sending near-identical PRs — I want to know whether that pattern is a known failure mode
   and what replaced it), mass submissions to directories, cold DMs to maintainers,
   Product Hunt for dev tools, paid promotion, and posting into one's own audience channels.

4. **The metrics those maintainers actually watched.** What number told them a channel was
   working, how soon after the push did it move, and what lag should be expected before
   declaring a channel dead. How do they separate "nobody came" from "not indexed yet"?

5. **The under-10-stars case.** Most advice assumes existing traction. What do you do when
   the honest state is single-digit stars and zero downloads — which doors are open at that
   stage, which require traction you do not have, and is there evidence that a few deeply
   engaged users (forks, bug reports) convert into adoption better than a spike of shallow
   attention?

6. **Agent-era specifics (2025–2026).** Has the arrival of MCP, agent frameworks and
   coding agents changed discovery for small primitives? Is there evidence that being
   installable-and-documented-for-machines (clear README, MCP manifest, llms.txt,
   machine-readable docs) produces adoption because *agents* recommend or install the tool,
   not humans? This is the part I am least sure about and most interested in.

## Rules for the answer

- Name projects and link them. A claim without a project behind it is worth less than
  "I don't know".
- Give numbers where they exist, and say explicitly where they do not. Do not estimate a
  number and present it as reported.
- Separate "worked / did not work" with a visible line, not a blended narrative.
- Where sources disagree, say so and say which is better evidenced.
- Prefer 2025–2026 evidence; flag anything older than 2024 as possibly stale, since
  discovery channels for dev tools have changed.
- End with a concrete ordered sequence of the next 5 moves for the situation in section one,
  each with the metric that would tell us within 30 days whether it worked.
