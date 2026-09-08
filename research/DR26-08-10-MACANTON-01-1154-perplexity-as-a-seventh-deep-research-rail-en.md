---
dr_id: DR26-08-10-MACANTON-01-1154
title: "Perplexity as a seventh deep-research rail: only the direct API works unattended, and it is billed separately"
date: 2026-08-12
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Perplexity: which interface actually works without a human

> **Evidence base:** four of six research rails completed: ChatGPT (66.7 KB), Grok (34.6 KB), Claude.ai (25.5 KB), and GLM (19.2 KB). The four-rail quorum was reached, so “consensus” is justified. Gemini and Mistral did not complete.

## Main finding

The only supported Perplexity interface suitable for a fully unattended scheduled deep-research run is the direct Sonar HTTP API.

For a full report, use `sonar-deep-research` through the asynchronous endpoint: submit with `POST /v1/async/sonar`, then poll for completion. A long research job should not depend on one synchronous request surviving from start to finish. All four completed research rails agreed on this conclusion.

## Important correction: “MCP does not work” is false

An early summary collapsed three different verdicts into “the other interfaces do not work.” The synthesis shows a more precise picture:

| Interface | Does it work? | Suitable as an unattended cron deep-research rail? | Why |
|---|---:|---:|---|
| Direct HTTP API (`sonar-deep-research`) | Yes | **Yes — the only suitable option** | Async submit-and-poll, pinned model ID, reproducible behavior |
| Official MCP (`perplexityai/modelcontextprotocol`) | Yes | No | Uses the same separately billed API key, adds host timeout and process-lifecycle failure modes, and does not provide the same pinned native Sonar DR path |
| Official `pplx` CLI | Yes | No | Provides Search API access and content extraction, not a finished deep-research report; a DR-shaped CLI would still have to be built over the HTTP API |
| Headless browser / Comet automation | No supported path | No | No supported automation API; CAPTCHA, session decay, DOM churn, account risk, and Terms-of-Service concerns |

The practical recommendation is: keep MCP for interactive sessions, use the official CLI for raw search, and do not automate the Perplexity website or Comet. For unattended deep research, use the direct asynchronous API or do not add the rail.

## The subscription does not pay for the API

Perplexity Pro, Max, and Enterprise subscriptions are not a Sonar API budget. API use is metered separately on a pay-as-you-go basis.

The price components recorded in the research were:

- input: $2 per million tokens;
- output: $8 per million tokens;
- citations: $2 per million tokens;
- reasoning: $3 per million tokens;
- search: $5 per 1,000 requests.

Perplexity's official example came to approximately **$0.816 for one run**.

The research rails produced widely different estimates for five reports per day: $48/month from GLM, $75–230 from Claude.ai, $75–300 from Grok, and $105–338 from ChatGPT. Each estimate was single-source, so these figures should be treated only as orders of magnitude, not as a budget.

## Citation quality is not synthesis quality

Perplexity's strong source attachment does not automatically mean it produces the best synthesis. Two comparative quality claims appeared in the research, but each was supported by only one rail and should not be treated as established:

- ResearcherBench coverage of 0.480 versus 0.703 for OpenAI and 0.693 for Gemini;
- Humanity's Last Exam score of 21.1% versus 26.6% for OpenAI.

## What remains unverified

- The claim that the MCP `perplexity_research` tool uses an `openai/gpt-5.6-sol` agent preset came from ChatGPT alone and remains unconfirmed.
- GLM alone claimed that the async API did not exist; this was rejected by the other rails and the official example.
- GLM also claimed a hard 10,000-token output cap; another documented example exceeded that figure.

## Decision for an autonomous research stack

1. Do not build MCP, CLI, or headless-browser paths for this unattended role.
2. If new API spending is acceptable, run a small `sonar-deep-research` async pilot with a hard spending cap.
3. If the rule is to consume only already-paid subscriptions, do not add Perplexity as a seventh rail.

**Perplexity was not added to the autonomous stack. The remaining decision is financial, not technical.**

