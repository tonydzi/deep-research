# DR26-11-11-ZB-01-1753 — the exact prompt sent to every model

# DR26-11-11-ZB-01-1753 — Выбор MCP-коннекторов под наши задачи: какие есть, какие популярны, что брать (кластеры спроса 9125 сессий)

# {DR-ID} — Which MCP connectors should a one-person AI lab actually connect (Nov 2026)

## CONTEXT (what we already know — do not redo)
We run a small "AI lab" fleet: ~8 machines (Windows + macOS + one Linux VPS), agents = Claude Code (desktop app + CLI), OpenAI Codex CLI, Cursor; plus web subscriptions to ChatGPT, Gemini, Grok, GLM (z.ai), Mistral. One operator (Anton, COO/CTO-type engineer, job-hunting into frontier LLM labs, publishes papers and posts, does OSS contributions).

Hard rule of our fleet: an always-on MCP config may contain ONLY remote/HTTP(S)/SSE servers (one process per machine or a hosted endpoint). stdio MCP servers spawn one process PER agent session; we measured 176 processes / 7 GB RAM on one hub for a single stdio server, so stdio servers are allowed only ad hoc ("add, use, remove"), never in the permanent config.

Already connected (Nov 2026): Gmail, Google Calendar, Google Drive (claude.ai first-party connectors), Claude-in-Chrome (browser control), our own Telegram MCP (HTTP daemon on Telethon), our own WhatsApp MCP (HTTP), a local code index MCP (HTTP), n8n, scheduled tasks. Recently added but never used yet (waiting OAuth): Buffer, Typefully, X (official) remote MCPs.

Measured demand, 9,125 agent sessions, 27 May – 11 Nov 2026 (sessions that touched it):
- MCP calls: Chrome control 15.3k calls/311 sessions; Telegram 2.7k/357; Gmail 2.6k/358; scheduled tasks 1.9k/326; Calendar 818/291; Drive 88/13; WhatsApp 93/26; n8n 66/12. Buffer/Typefully/X official = 0.
- GitHub: gh CLI 2,349 calls / 305 sessions; github.com via shell 227 sessions.
- LLM web UIs driven by browser automation (for deep research fan-out): chatgpt.com 75 sessions, claude.ai 56, grok.com 49, z.ai 34, mistral 8; openrouter.ai via shell 10.
- Social posting/reading: x.com 26+19, facebook.com 15, dev.to 27, hackernoon 6, bsky 5, mastodon 5, reddit 6, linkedin 6 (our LinkedIn is banned), Telegram channels.
- Scholarly: orcid.org 58 sessions, arxiv 25, zenodo 21, doi.org 22, crossref 7, datacite 8, openalex 6, semanticscholar 6, academia.edu 5, hotcrp (paper submission).
- Job hunting: ashbyhq 16, greenhouse 10, lever 6, wellfound 5, workatastartup, ycombinator 9.
- Infra: docker CLI 19, hetzner 4+3, cloudflare 3, tailscale 6, huggingface 18.
- Events/meetings: calendly 13, luma 5, partiful 3. Misc: airtable 3, jina.ai 5, algolia 6.

## QUESTION
For each demand cluster above, is there a mature MCP connector worth connecting, and which ones should we NOT connect?

For EVERY cluster (GitHub · LLM vendors/OpenRouter · X/Twitter · Facebook/Meta · Bluesky/Mastodon/Reddit/dev.to/Hashnode/HackerNoon · LinkedIn · scholarly (arXiv, ORCID, Zenodo, Crossref, DataCite, OpenAlex, Semantic Scholar) · job boards/ATS (Ashby, Greenhouse, Lever, Wellfound, YC) · Hugging Face · cloud/infra (Hetzner, Cloudflare, Tailscale, Docker) · scheduling/events (Calendly, Luma, Partiful) · Airtable/Notion-type · web fetch/search (Jina, Firecrawl, Exa, Brave, Tavily) · social schedulers (Buffer, Typefully, Hootsuite, Publer, Ayrshare) · browser automation (Playwright MCP, Browserbase, Claude in Chrome) · messaging (Slack, Discord, Signal):
1. Name the best existing MCP server(s): official vendor-run vs popular community; exact repo/URL; stars or install count if known; last release date.
2. Transport: hosted remote HTTP/SSE with OAuth? self-hostable HTTP? stdio only? (stdio-only = we can't keep it always-on.)
3. What it gives versus our current path (CLI like gh, direct REST script, or browser automation). Be concrete: which operations become possible/cheaper/safer.
4. Cost (free tier limits, paid plan), and API keys/OAuth required.
5. Ban / ToS risk (especially X, Facebook, LinkedIn, Reddit, LLM web UIs automation).
6. Multi-vendor: does it work with Codex CLI, Cursor, Gemini CLI and ChatGPT connectors too, or only Claude?
7. Security: write permissions, prompt-injection exposure, token scope.

Separately:
A. Evaluate Buffer, Typefully and the official X MCP specifically for "post to X from any of our machines": which one actually supports posting threads/media, pricing, rate limits, and which is the least risky for account bans.
B. Describe the current catalogs: claude.ai Connectors directory, the official MCP Registry (registry.modelcontextprotocol.io), OpenAI/ChatGPT connectors, Cursor MCP directory, Smithery, mcp.so, Glama. How to tell a maintained server from abandonware.
C. Is the GitHub official remote MCP (api.githubcopilot.com/mcp) worth it over the gh CLI for agents? Token cost of tool definitions vs CLI.

## OUTPUT
1. Table: connector · cluster · our frequency (from the context) · TAKE / DON'T TAKE / LATER · why (one line) · transport · cost · source URL.
2. Top-10 to connect now, ranked by expected value for us.
3. Explicit DON'T list with reasons (risk, stdio-only, duplicates a working CLI, abandoned).
4. For each recommendation name whether prior art (someone already measured/compared it) exists, or write "prior art not found" and where you looked.

Rules: search the web extensively (at least 30 searches), cite every non-obvious claim with a full https:// URL you actually opened; mark confidence (high/medium/low) per row; say "unknown" rather than guess; dates matter, prefer sources from 2026. Deliver the complete report as Markdown text directly in this chat (no PDF, no files).
