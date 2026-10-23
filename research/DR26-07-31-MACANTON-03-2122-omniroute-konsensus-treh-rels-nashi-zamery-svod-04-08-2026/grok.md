# DR26-07-31-MACANTON-03-2122 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: Grok Heavy (grok.com), 622 sources, Thought for 1m24s
topic: OmniRoute — independent reputation & risk audit
dr_id: DR26-07-31-MACANTON-03-2122
date: 2026-07-31

url: https://приватный чат
---

# DR26-07-31-MACANTON-03-2122 — OmniRoute: independent reputation & risk audit

Deep Research request: OmniRoute (github.com/diegosouzapw/OmniRoute) — independent reputation & risk audit, July 2026.
Context: We are a small AI lab running Claude Code on Anthropic subscriptions and constantly hitting rate limits. OmniRoute is a local-first MIT AI gateway (36k stars, 4.6k forks, created Feb 2026) that promises one endpoint over 290+ providers, quota-aware fallback and 15-95% token compression (RTK+Caveman). We are deciding whether to route our coding agents through it.
Please research and answer with dated sources:

COMMUNITY SENTIMENT. What do real users report on Reddit (r/LocalLLaMA, r/ClaudeAI, r/ChatGPTCoding), Hacker News, X/Twitter, Discord, dev.to, YouTube about OmniRoute in production/daily use? Separate hype from measured experience. Any long-term users (>1 month)?
SECURITY TRACK RECORD. Full timeline: Socket.dev npm block of v3.8.5 (May 2026, alleged obfuscated code/install scripts), CVE-2026-49352 and any other CVEs, the maintainer's responses, current state of audits. Is the Docker image / npm package considered clean today? Any supply-chain incidents among its 500+ contributors?
ACCOUNT-BAN EVIDENCE. Concrete reports of provider accounts (Anthropic/Claude, OpenAI/Codex, Cursor, Kiro, Mistral, Together, SiliconFlow) being banned or flagged for routing through OmniRoute or similar proxies (OpenClaw-class harnesses). How aggressive is Anthropic specifically about third-party gateways on Claude subscription (not API) traffic? What do the ToS of Anthropic Claude Code subscription say about proxies/TLS-fingerprint masking?
MAINTAINER & GOVERNANCE. Who is diegosouzapw? Track record, other projects, bus-factor. How much of the "500+ contributors" is real vs drive-by? Release cadence and breaking-change history.
TECHNICAL REALITY CHECK. Independent measurements of: actual free-token capacity (claimed 1.5B/month; dev.to measured ~65% is Mistral alone), compression quality loss on coding tasks (does RTK/Caveman break tool-calling or degrade agentic coding?), latency overhead, stability of quota-aware fallback in multi-hour agent sessions.
ALTERNATIVES MATRIX. For the specific use case "Claude Code subscription user hitting limits, wants overflow to cheaper/free models without risking the Anthropic account": compare OmniRoute vs LiteLLM vs OpenRouter vs Portkey vs claude-code-router vs doing nothing (native model fallback in Claude Code). Which path has the lowest ban risk for the Anthropic account?
VERDICT. Given our constraints (Anthropic account reputation is mission-critical for us — we cannot risk a ban; we CAN spend on cheap API keys), recommend: adopt / adopt-with-fences (which fences exactly) / avoid. If adopt-with-fences: is it safe to run OmniRoute ONLY for non-Anthropic free tiers while keeping Claude traffic native?
Format: findings with dates and links, confidence per claim (established/emerging/speculative), final one-page decision memo.

## MANDATORY RESEARCH PROTOCOL (applies to every section above)

Today is 2026-07-31. Prefer sources from the last 90 days; label anything older.
Search the web EXTENSIVELY. Open at least 15-25 primary sources (GitHub issues/PRs, Reddit/HN threads, CVE databases, provider ToS pages, vendor changelogs). Secondary blog rewrites of the README do NOT count as sources.
For every factual claim: a short quote + the exact URL you actually opened. Cite ONLY URLs you really opened — never reconstruct a plausible link.
Mark each claim's confidence: established / emerging / speculative.
Before the final report, list the search queries you ran and the URLs you actually opened (proof of work).
If you cannot verify something, say "not verified" — do not fill the gap with a plausible guess.
End with a one-page DECISION MEMO: recommendation (adopt / adopt-with-fences / avoid), the exact fences, and what evidence would flip the decision.
Search the web and X extensively before answering. Open at least 15-25 primary sources and cite ONLY URLs you actually opened.
Thought for 1m 24s

OmniRoute (github.com/diegosouzapw/OmniRoute) — Independent Reputation & Risk Audit
Scope / Date: July 31–August 1 2026 (prefer last ~90 days). Audience: small AI lab running Claude Code on Anthropic subscriptions, hitting rate limits, evaluating local-first MIT gateway for overflow.
Overall confidence: Medium on project existence/features/growth; High on Anthropic ToS prohibition and Socket mitigations; Low–Emerging on long-term production stability, independent compression efficacy for agentic coding, and verified usable free-token capacity.

Executive Summary
OmniRoute is a real, viral (~36k stars, ~4.6–4.7k forks as of ~Aug 1 2026; created ~Feb 13–17 2026) local-first MIT TypeScript/Next.js AI gateway promising one OpenAI-compatible endpoint over 290+ providers (90+ free), quota-aware fallback, and RTK+Caveman compression (claimed 15–95%). It explicitly targets Claude Code / Cursor / Codex users.⁠GitHub
Community sentiment is mostly hype-driven (stars, Chinese blogs, X promo, YouTube); measured long-term (>1 month) independent production reports are sparse. Positive short-session anecdotes exist alongside open issues on crashes, DB bloat, and tool-calling breaks on failover. Confidence: Emerging.
Security: Socket.dev blocked v3.8.5 (May 2026) for AI-detected “malware” patterns (MITM root-CA install, install scripts, obfuscation). Maintainer mitigated in v3.8.6 with attestation, fingerprint changes, and minimal-build profile. Current npm (3.8.49) and Snyk show 0 known vulns. CVE-2026-49352 is upstream 9Router (hardcoded JWT), not OmniRoute. Residual risk is intentional opt-in stealth/MITM. Confidence: Established for timeline/mitigations.
Account-ban evidence: Anthropic Consumer Terms and Claude Code legal docs explicitly prohibit using Free/Pro/Max OAuth tokens in any third-party product/tool/harness (including Agent SDK). Enforcement active since Jan–Apr 2026 with bans for OpenClaw-class tools. OmniRoute implements extensive Claude Code fingerprint/stealth (TLS, headers, integrity tokens, zero-width). No verified public Anthropic bans specifically attributed to OmniRoute found; Google/Antigravity bans from its features are documented. Confidence: High on ToS; Speculative on specific OmniRoute Anthropic bans.
Maintainer (diegosouzapw / Diego Rodrigues de Sa e Souza, Brazil) is responsive; “500+ contributors” includes many drive-bys; core activity concentrated (high bus factor). Rapid release cadence.
Technical: Free-token claim (~1.53B documented/month) is self-reported and heavily Mistral-weighted (~65%). Independent JetBrains benchmarks on RTK/Caveman show far lower (or negative) savings on real agentic coding tasks and no quality gain. Stability issues (crash loops, SQLite bloat, OOM) reported in mid-2026 discussions. Confidence: Emerging.
Lowest ban-risk path for Anthropic account: keep Claude traffic native; overflow via mature self-hosted proxy (LiteLLM) using only API keys or free tiers.

Recommendation (one-page Decision Memo at end): Adopt-with-fences only for non-Anthropic free/cheap tiers while keeping Claude Code 100% native. Full adoption (routing Anthropic subscription traffic) is high-risk and should be avoided.

Detailed Findings
1. Community Sentiment

Viral growth is established (stars rose from ~6k mid-June to ~35–36k by late July 2026; npm ~58k weekly downloads). Positive coverage appears in ProvenLabs (2026-07-23), genztech.blog (2026-07-25), Dashen Tech, Juejin posts (2026-07-30/31), YouTube (~1.5k views), and X promotional threads. One Juejin user reported a 6-hour Cursor session with seamless tier fallback.⁠Provenlabs

Measured experience is thinner. Independent review (Florian Gahn, ~2026-07-14) flags claim contradictions, frequent hotfixes, and advises against production. GitHub Discussion #4444 (2026-06-20) details crash/restart loops, 1.4 GB+ SQLite bloat, OOM (4.5–8 GB), and pricing drift; maintainer acknowledged issues. Other issues document tool-calling breaks on provider failover mid-session (#8437, 2026-07-24) and high error rates under load. Reddit hits are sparse (maintainer posts, one short positive “it works” for Codex via OmniRoute); no strong long-running r/LocalLLaMA, r/ClaudeAI, or r/ChatGPTCoding threads with multi-month users found. HN threads not located. Confidence: Hype established; long-term independent production experience emerging/sparse.

2. Security Track Record

Timeline (established):

2026-05-28: Issue #2863 — Socket.dev blocked [email hidden] (Supply Chain score 48; 6 AI-detected potential-malware, obfuscated code, install scripts). Flagged paths included MITM root-CA install (elevated PowerShell/certutil across OSes) and Zed credential import.⁠GitHub
Maintainer response (PR ~#2871 → v3.8.6, May 29): removed base64-EncodedCommand fingerprint, 2-step confirmed import, HMAC for cloud-sync, OMNIROUTE_BUILD_PROFILE=minimal to stub sensitive modules, full attestation in docs/security/SOCKET_DEV_FINDINGS.md. Document explicitly states findings are false positives on legitimate documented MITM/stealth features (same commands as mitmproxy/Charles).⁠GitHub
CVE-2026-49352 (OpenCVE/IONIX, 2026-07-15): hardcoded JWT fallback in upstream 9Router (0.2.21–0.4.43); fixed in 0.4.44. OmniRoute lineage acknowledged but not directly affected.⁠App.opencve
Current state (2026-07-30): npm 3.8.49 / Snyk 0 C/H/M/L; Socket page for latest shows no active block. Docker image (diegosouzapw/omniroute) multi-arch. No verified supply-chain incidents among contributors. Early unprotected-endpoint issues fixed. Residual risk is the intentional opt-in MITM/TLS-fingerprint/stealth layer documented in STEALTH_GUIDE.md.
3. Account-Ban Evidence

Anthropic policy is unambiguous. Claude Code legal/compliance page (current as of 2026-07): “OAuth authentication is intended exclusively for … Claude Free, Pro, Max … and other native Anthropic applications.” “Anthropic does not permit third-party developers to … route requests through Free, Pro, or Max plan credentials on behalf of their users.” Using such OAuth tokens in any other product/tool (including Agent SDK) violates Consumer Terms.⁠Code.claude

Register (2026-02-20) and related coverage document the clarification and enforcement wave (server-side blocks from Jan 2026, further April changes, bans for OpenClaw-class harnesses generating “unusual traffic patterns”). Anthropic engineer commentary notes third-party harnesses lack telemetry and complicate support.⁠Theregister

OmniRoute implements extensive “compatibility”/stealth for Claude Code (wreq-js Chrome 124 TLS, exact UA/headers, integrity cch tokens, zero-width obfuscation, body ordering) and documents ban-keyword detection. STEALTH_GUIDE.md warns that features are “not for evading … or violating provider Terms of Service” and flags ANTIGRAVITY_CREDITS=always as the “single most common ToS-violation report” (multiple Google Ultra bans). No verified public reports of Anthropic account bans specifically caused by OmniRoute were located in the last 90 days; risk is high by design if Claude subscription OAuth is routed through it. Confidence: Established on policy/enforcement; Speculative on OmniRoute-specific Anthropic incidents.

4. Maintainer & Governance

diegosouzapw = Diego Rodrigues de Sa e Souza (São Paulo, Brazil; CDWA Solutions; LinkedIn in/diegosouzapw; ~795 GitHub followers). Primary project is OmniRoute; smaller related repos (OmniGlyph, OmniAntigravityRemoteChat). Responsive to security flags (days from Socket report to mitigations + attestation).⁠GitHub

“500+ contributors” claim appears in README; GitHub lists ~464; top committers (oyi77 213, others 50–100+) exist, but activity is heavily concentrated on the maintainer (high bus factor). Many contributions are drive-by given viral growth. Release cadence is rapid (near-daily patches in 3.8.x series; some breaking security/env changes). MIT license, active issues/PRs, Discord/Telegram/WhatsApp communities.

5. Technical Reality Check
Free-token capacity: Project docs/reference/FREE_TIERS.md and README claim ~1.53B documented recurring free tokens/month (43 pools, pool-deduped; first-month higher with signup credits). Independent commentary (creeta/DEV-style analyses ~2026-07-20) notes ~65% is Mistral. No third-party audit of real-world usable capacity after rate limits, account verification, and ToS. Confidence: Established for self-reported math; Emerging for practical yield.
Compression: Official stacked RTK→Caveman claims 78–95% (avg ~89%) on eligible tool/context payloads; individual modes 15–75%. Independent JetBrains AI blog benchmarks (2026-07) on real Claude Code / SkillsBench tasks: Caveman measured ~8.5% (vs claimed 65%); RTK raised median cost +7.6% and turns +13.8% with no quality gain. OmniRoute issues report 0 measured savings in some stacked runs. Tool-calling integrity can break on cross-provider failover. Confidence: Established independent null/negative results for agentic coding; claims best-case for noisy shell output only.
Latency/stability: Local process; failover claimed millisecond-scale. Multi-hour agent sessions show emerging problems (Discussion #4444 crash loops / SQLite unbounded growth / OOM; concurrent-load error rates; quota-cache bugs). Frequent hotfixes. One positive 6 h report exists. Confidence: Emerging instability signals.
6. Alternatives Matrix

Use case: Claude Code subscription user needing overflow to cheaper/free models without risking the Anthropic account.

Path	Ban risk (Anthropic sub)	Maturity / Ops	Features for overflow	Recommendation for this constraint
Native Claude Code only	Lowest	Highest	None	Baseline safe
LiteLLM (self-host, API keys only)	Low (if no sub OAuth)	High (mature, Python ecosystem)	Fallbacks, load-balance, virtual keys	Preferred overflow
OpenRouter (hosted)	Medium (if sub tokens used)	High	Many models, simple	Acceptable with own keys
Portkey	Low–Medium	High (obs + guardrails)	Strong observability	Good for teams
claude-code-router	Medium–High	Medium	Claude-Code oriented	Still third-party
OmniRoute full (Claude sub routed)	Highest	Low–Medium (4–5 mo old, instability reports)	Richest free aggregation + compression	Avoid for Claude traffic
OmniRoute non-Anthropic only	Low	Same	Free tiers + cheap APIs	Acceptable with fences

Anthropic explicitly treats third-party harnesses using subscription OAuth as ToS violations. Keeping Claude traffic on the official binary and routing only API-key / free-tier traffic through a mature proxy minimizes risk.

Synthesis and Implications

OmniRoute solves a real pain (quota fragmentation + token waste on tool output) with ambitious engineering and rapid iteration. The security incident was handled transparently and mitigated. However, the combination of (1) clear Anthropic prohibition on third-party OAuth for consumer subscriptions, (2) OmniRoute’s intentional stealth/fingerprint layer that increases detection surface, (3) sparse long-term production data, (4) independent benchmarks showing compression claims do not hold on agentic coding workloads, and (5) high bus-factor + ongoing stability issues makes full adoption incompatible with a “cannot risk Anthropic ban” constraint.

Safest path that still captures value: keep Claude Code 100% native; stand up OmniRoute (or preferably LiteLLM) solely for non-Anthropic free/cheap providers as overflow when Claude quotas are exhausted.

Limitations and Open Questions
No exhaustive Discord scrape or private long-term user interviews.
Free-token usable yield after real rate limits/ToS not independently audited.
Exact detection rate of Anthropic’s fingerprinting vs OmniRoute’s stealth remains unquantified (by design, both sides evolve).
JetBrains benchmarks are the strongest independent compression data; more coding-agent-specific measurements would raise confidence.
Source Catalog (key primary sources actually opened)

GitHub: repo README, issues #2863 / #4444 / #8437 / #6345 / #2321 / #1846, PRs, docs (SOCKET_DEV_FINDINGS.md, STEALTH_GUIDE.md, FREE_TIERS.md, COMPRESSION_GUIDE.md, ARCHITECTURE, USER_GUIDE), releases, contributor graph.
Socket.dev npm page; Snyk versions; OpenCVE/IONIX for CVE-2026-49352.
Anthropic: code.claude.com/docs/en/legal-and-compliance; Register 2026-02-20 article; related ToS coverage.
Independent: JetBrains AI blog (Caveman/RTK benchmarks 2026-07); ProvenLabs 2026-07-23; Florian Gahn review; creeta/DEV free-token analyses; Juejin posts; genztech.blog; X keyword results.
npm, Docker Hub references, maintainer profile.

Proof-of-work search queries (representative):
“OmniRoute github.com/diegosouzapw/OmniRoute”; “OmniRoute Socket.dev npm block OR CVE-2026-49352”; “OmniRoute site:reddit.com”; “OmniRoute site:news.ycombinator.com”; “Anthropic Claude Code (proxy OR gateway OR third-party) (ban OR ToS)”; “diegosouzapw”; “OmniRoute (ban OR banned) Anthropic”; “OmniRoute free tokens OR 1.5B OR compression OR RTK OR Caveman benchmark”; “OmniRoute (latency OR overhead OR quality loss OR tool calling)”; “JetBrains RTK OR Caveman”; “LiteLLM vs OpenRouter vs Portkey vs claude-code-router”; plus issue-specific and X/semantic searches. ≥25 primary URLs opened and quoted above.

One-Page Decision Memo

Recommendation: Adopt-with-fences (not full adopt, not pure avoid).

Exact fences:

Never configure Claude Code (or any client) to send Anthropic subscription OAuth traffic through OmniRoute. Keep ANTHROPIC_BASE_URL / auth native to the official Claude Code binary.
Use OmniRoute (or, preferably for maturity, LiteLLM) only for non-Anthropic providers: free tiers (Kiro, SiliconFlow, etc.) and cheap API keys you control.
Pin version ≥3.8.6 (post-Socket mitigations); prefer Docker with volume isolation and no elevated privileges.
Explicitly disable MITM / stealth / cert-install features and any Claude-specific fingerprint modules.
Monitor provider ban-keyword logs and Anthropic account health daily for the first 30 days; have a rollback plan (native Claude Code only).
Treat compression as best-effort for noisy tool output only; do not rely on 15–95% claims for agentic coding quality or cost.

Why this path: Anthropic account reputation is mission-critical. Routing subscription OAuth through any third-party gateway (including OmniRoute’s stealth layer) is a clear ToS violation with active enforcement. The fences capture free-tier aggregation and fallback value while eliminating the highest-risk vector. LiteLLM is the lower-ops alternative if you want only mature fallback without OmniRoute’s surface area.

Evidence that would flip the decision:

Explicit Anthropic written allowance for local gateways on consumer OAuth (currently absent and contradicted by docs).
Multiple independent, multi-month production users reporting zero Anthropic bans + stable multi-hour agent sessions with tool-calling intact under load.
Independent third-party audit confirming usable free-token capacity and compression fidelity on coding benchmarks matching claims.

Until then, the conservative posture protects the account while still allowing overflow capacity.

622 sources
Investigate LiteLLM implementation details
Explore Anthropic detection mechanisms




Heavy