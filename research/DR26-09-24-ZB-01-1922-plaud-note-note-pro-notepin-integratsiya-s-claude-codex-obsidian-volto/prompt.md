# DR26-09-24-ZB-01-1922 — the exact prompt sent to every model

# DR26-09-24-ZB-01-1922 — Plaud: юзкейсы подключения к Claude/Codex/Obsidian-волту/CRM (CLI, MCP, API, обвязки, грабли)

ROLE: You are a senior research analyst. Search the web extensively (official docs, GitHub, Reddit, X, Hacker News, YouTube descriptions, blogs, Plaud community forums, npm). Cite ONLY URLs you actually opened; give a short quote + exact URL for every factual claim. 15-25 primary sources minimum. Period: 2025-2026, prioritize the most recent.

TOPIC: Plaud AI voice recorders (Plaud Note, Note Pro, NotePin, NotePin S) — how real users connect them to (1) Claude (Claude Code, Claude Desktop, MCP), (2) OpenAI Codex / ChatGPT, (3) an Obsidian vault (auto-export of audio + transcripts + summaries into markdown), and to a CRM.

CONTEXT: The user runs a fleet of 6 machines (Windows + macOS + a Linux VPS) with a shared Obsidian vault synced via Syncthing, Claude Code and Codex as agents, a CRM of leads, and an automated "meeting recap -> CRM card -> follow-up draft" pipeline. Official Plaud CLI (npm @plaud-ai/cli: login/files/audio/transcript/summary/search/recent/today) and official MCP (npm @plaud-ai/mcp) exist; OAuth tokens live in ~/.plaud/tokens.json.

QUESTIONS:
1. Official surfaces: what exactly do @plaud-ai/cli, @plaud-ai/mcp and the Plaud developer API (platform.plaud.ai) expose? List MCP tools, rate limits, plan requirements (free vs Pro vs Unlimited), regions (EU vs US data centers), audio formats, 24h download-link behavior. Is there a webhook / push event for "new recording", or only polling?
2. Third-party wrappers: sergivalverde/plaud-toolkit, danielgwilson/plaud, plaudit-cli, any others (Python libs, reverse-engineered web API clients, Zapier/Make/n8n integrations, Plaud's own "Integrations" e.g. Notion/Zapier). What do they add, how maintained, what breaks?
3. Real user pipelines: concrete write-ups of "Plaud -> Obsidian" (plugins, scripts, folder layouts, frontmatter), "Plaud -> Claude/ChatGPT/Codex" (MCP in Claude Desktop/Code, custom GPTs, uploading transcripts), "Plaud -> CRM/Notion/HubSpot". Include setup steps, what worked, what failed.
4. Automation mechanics: how do people detect new recordings and pull them unattended (cron polling via CLI, Sync-to-cloud-while-charging over Wi-Fi, desktop app watch folders)? Latency from end of recording to availability in API.
5. Multi-machine concerns: can one OAuth token be used on several machines, token refresh/expiry, headless servers, running the MCP on a VPS.
6. Pitfalls & risks: transcript quality vs self-hosted Whisper large-v3, speaker diarization quality, recording-consent law (one-party vs two-party states like California), GDPR (audio stored on Plaud servers, region), account bans/ToS for automated API use, outages.
7. Recommendation matrix: for (a) a live chat assistant session and (b) an unattended robot pipeline into a vault + CRM, which layer is best (MCP vs CLI poller vs raw API vs third-party), and why.

OUTPUT: Russian language. Structure: Executive summary (10 bullets) -> sections per question -> table of tools/wrappers (name, URL, type, last update, what it gives) -> recommended architecture for a 6-machine fleet (who polls, where tokens live, idempotency, file naming) -> open questions / what is NOT documented (say "not found" with where you looked). Finish with a list of all URLs actually opened.
