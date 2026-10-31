# DR26-07-31-MACANTON-03-2122 — the exact prompt sent to every model

Deep Research request: OmniRoute (github.com/diegosouzapw/OmniRoute) — independent reputation & risk audit, July 2026.

Context: We are a small AI lab running Claude Code on Anthropic subscriptions and constantly hitting rate limits. OmniRoute is a local-first MIT AI gateway (36k stars, 4.6k forks, created Feb 2026) that promises one endpoint over 290+ providers, quota-aware fallback and 15-95% token compression (RTK+Caveman). We are deciding whether to route our coding agents through it.

Please research and answer with dated sources:

1. COMMUNITY SENTIMENT. What do real users report on Reddit (r/LocalLLaMA, r/ClaudeAI, r/ChatGPTCoding), Hacker News, X/Twitter, Discord, dev.to, YouTube about OmniRoute in production/daily use? Separate hype from measured experience. Any long-term users (>1 month)?

2. SECURITY TRACK RECORD. Full timeline: Socket.dev npm block of v3.8.5 (May 2026, alleged obfuscated code/install scripts), CVE-2026-49352 and any other CVEs, the maintainer's responses, current state of audits. Is the Docker image / npm package considered clean today? Any supply-chain incidents among its 500+ contributors?

3. ACCOUNT-BAN EVIDENCE. Concrete reports of provider accounts (Anthropic/Claude, OpenAI/Codex, Cursor, Kiro, Mistral, Together, SiliconFlow) being banned or flagged for routing through OmniRoute or similar proxies (OpenClaw-class harnesses). How aggressive is Anthropic specifically about third-party gateways on Claude subscription (not API) traffic? What do the ToS of Anthropic Claude Code subscription say about proxies/TLS-fingerprint masking?

4. MAINTAINER & GOVERNANCE. Who is diegosouzapw? Track record, other projects, bus-factor. How much of the "500+ contributors" is real vs drive-by? Release cadence and breaking-change history.

5. TECHNICAL REALITY CHECK. Independent measurements of: actual free-token capacity (claimed 1.5B/month; dev.to measured ~65% is Mistral alone), compression quality loss on coding tasks (does RTK/Caveman break tool-calling or degrade agentic coding?), latency overhead, stability of quota-aware fallback in multi-hour agent sessions.

6. ALTERNATIVES MATRIX. For the specific use case "Claude Code subscription user hitting limits, wants overflow to cheaper/free models without risking the Anthropic account": compare OmniRoute vs LiteLLM vs OpenRouter vs Portkey vs claude-code-router vs doing nothing (native model fallback in Claude Code). Which path has the lowest ban risk for the Anthropic account?

7. VERDICT. Given our constraints (Anthropic account reputation is mission-critical for us — we cannot risk a ban; we CAN spend on cheap API keys), recommend: adopt / adopt-with-fences (which fences exactly) / avoid. If adopt-with-fences: is it safe to run OmniRoute ONLY for non-Anthropic free tiers while keeping Claude traffic native?

Format: findings with dates and links, confidence per claim (established/emerging/speculative), final one-page decision memo.
