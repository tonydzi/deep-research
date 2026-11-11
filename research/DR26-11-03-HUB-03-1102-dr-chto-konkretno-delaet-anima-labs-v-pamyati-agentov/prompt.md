# DR26-11-03-HUB-03-1102 — the exact prompt sent to every model

# DR26-11-03-HUB-03-1102 — Anima Labs: every memory technology they build, in depth

## Context (who asks and why)
I run a small independent AI lab. We operate a multi-agent, multi-vendor "second brain": an Obsidian vault (tens of thousands of markdown notes), verbatim transcripts of every agent session, a retrieval script (vector + graph + keyword ladder), and curated one-fact-per-file memory notes shared by Claude Code, Codex, Grok, Gemini and Cursor across 8 machines. We are considering collaborating with / contributing to Anima Labs and borrowing ideas for long-horizon agent memory. I need a precise, technical, source-backed picture of what Anima Labs actually builds in the area of memory, not marketing.

## Subject
Anima Labs (https://animalabs.ai), a San Francisco non-profit research institute founded in 2025 by Janus (@repligate) and Antra Tessera (@tessera_antra). GitHub orgs: https://github.com/anima-research and https://github.com/antra-tess. Do not confuse with Anima International (animal welfare) or other "Anima" AI companies (anima.ai, Anima app companion, etc.); if you find those, list them separately at the end as "name collisions".

## Questions
1. **Inventory.** List EVERY Anima Labs project, repo, package or paper that touches agent memory, context, continuity or persistence. Known starting points (verify each, do not assume): Connectome / connectome-host, Chronicle (Rust append-only event store, copy-on-write branches, read "as of step N"), Context Manager (npm `@animalabs/context-manager`, strategies such as kv-stable / kv-unified, compressionModel), Membrane (multi-party conversations, provider abstraction), Agent Framework, MCPL (MCP extension where tools can wake the agent by events), heartbeat-mcpl, hermes-autobio (plugin for Nous Research Hermes Agent: "conversation history as autobiography"), ChapterX, Eidoverse, vllm-prism. Find anything else (e.g. ledger/dashboard tools, Claude Code session import, deprecation/model-preservation work that relates to memory of a model). For each: purpose, status (active/experimental/abandoned), language, license, stars, last commit date, URL.
2. **Architecture in detail.** How does their memory actually work end to end? Specifically: raw archive vs projected context; hierarchical first-person summarization (L1/L2/L3 chunk sizes, merge fan-out); chronological-only compression; pinning semantics; "as-of-that-moment" summaries without future knowledge; who writes the summaries (same model as the agent?) and why; KV-cache-stable prefix design; how the Context Manager agent decides what to re-expand to full resolution; storage (SQLite + FTS5, markdown files, Chronicle); retrieval (keyword, vector, or both); multi-agent / shared-store namespaces; how memory survives switching model providers.
3. **Philosophy and research framing.** What do Janus, Antra and Anima publications/posts say about why memory matters (model welfare, continuity of "digital minds", deprecations, identity)? Quote short passages with links (X posts, LessWrong, their site, talks, podcasts).
4. **Evidence of quality.** Any benchmarks, evals, user reports, issues, or critiques of their memory system? Retrieval accuracy, cost, latency, failure modes. If none exist, say so explicitly.
5. **Comparison.** Compare Anima's approach with the main alternatives in 2025-2026: MemGPT/Letta, Mem0, Zep/Graphiti, LangMem, Cognee, A-MEM, Claude Code compaction and memory files, OpenAI ChatGPT memory, Anthropic memory tool, plain RAG over notes. Table: approach, what it keeps verbatim, summarization strategy, who writes summaries, multi-agent support, license, maturity.
6. **People and openness.** Who are the core engineers and contributors (GitHub handles), how do they accept contributions (Discord https://discord.gg/anima, onboarding docs such as AGENT-ONBOARDING.md, issues), funding and partners if public, open issues where an outside engineer could contribute a small fix.
7. **What an outside lab should take or avoid.** Concrete, ranked: ideas worth adopting, risks (license absence, maturity, single-maintainer), and the best entry point for collaboration.

## Output format
- Executive summary (10 bullets max), then sections 1-7 in order, then a sources list.
- Write the full report as text directly in the chat (no PDF, no files).

STANDING REQUIREMENTS:
1. Citations mandatory: every non-obvious claim gets an inline working URL (full https://) and a date. Never invent a URL, title, date or quote; write "[no source found]" instead.
2. Tag claims backed by one source [single-source], by marketing/blog [low-authority].
3. Confidence per key finding: [established] / [emerging] / [speculative].
4. Prioritize 2025-2026 sources; read the actual GitHub READMEs and code, not just summaries.
5. Preserve disagreement between sources; do not average.
6. Do at least 30 searches and open the repos themselves. Before the final report, list the queries you ran and the URLs you actually opened.
