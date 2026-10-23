# DR26-10-11-HUB-01-0535 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Mistral Vibe deep-research canvas, collected 2026-10-11 from https://chat.mistral.ai/work/3eccd925-afcd-4b34-a618-f8e4ba03aa82

Desktop Multi-Harness Agent Environments: Verified Research Brief

Decision supported: Should a Windows-first, multi-machine power user run a small, reversible canary of a desktop environment that preserves several vendors' native coding-agent harnesses side by side? If yes, which product?

All volatile metrics (stars, forks, releases, push dates) retrieved 2026-10-11 (~05:15 UTC) from the GitHub API. Web sources retrieved 2026-10-11.

Executive Verdict

Verdict: YES — run a canary, but not with the prior report's #1 pick unchanged. The category is real, but the Windows-first constraint reorders it. [established]

The category exists and is verifiable. Orca, Superset, Emdash, OpenChamber, bb, ccgui, Proliferate, and CC Switch all resolved to real projects with live repositories we inspected directly. "Agent Development Environment" (ADE) is the vendors' own term (Orca: "Orca is the ADE for working with a fleet of parallel agents"; Emdash: "the Open-Source Agentic Development Environment (YC W26)"). [established]

Best canary for this use case: Emdash (generalaction/emdash) — Apache-2.0, first-class Windows builds (MSI and a portable .exe in the latest release v1.2.8, 2026-10-10), local-first with optional telemetry and a documented kill switch, OS-keychain credential storage, SSH/SFTP remote projects, worktree-isolated parallel agents with diff review and session resume. Weighted score 4.35/5. [established]

Fallback: Orca (stablyai/orca) — the most popular and most feature-rich ADE (89,285★, MIT, macOS/Windows/Linux claimed in official docs, daily releases, mobile companion, remote runtime), but it pre-applies full-autonomy "yolo" permission flags to every agent by default — a real security posture difference that matters for a canary. Weighted score 4.15/5, with a Windows artifact caveat (see Red Flags). [established]

Superset is disqualified for Windows-first users today: its README states "Windows: not yet available" (macOS primary, Linux experimental). The prior report's #2 ranking does not survive this constraint. [established]

Every named first-party harness is real — including the suspicious-sounding ones: Grok Build (xai-org/grok-build, Apache-2.0, open-sourced July 2026), DeepSeek Harness (dsh, MIT, npm @deepseek-ai/dsh, launched 2026-08-13, developer preview), ZCode (zai-org/ZCode, Apache-2.0, official harness for GLM-5.3, repo created 2026-09-20), and Google Antigravity (antigravity.google, macOS/Windows/Linux standalone + CLI). Kimi Code, Qwen Code, and Mistral Vibe also verify. [established]

Brand collisions are a genuine hazard in this space: "Emdash" is also an unrelated TypeScript CMS ("spiritual successor to WordPress", emdash-cms/emdash); "Orca" collides with Microsoft's MSI editor and OrcaSlicer; "Superset" collides with Apache Superset; "ccgui" collides with a different npm package (@jun-a/ccgui); "ZCode" is reused by unrelated projects. Most third-party "review" sites covering these tools are low-authority SEO content. [established]

Confidence in the recommendation: [emerging]. The category is under 12 months old for most products, no independent security audits exist for any of them [no source found], and Windows-specific stability evidence is mostly vendor-authored.

Methodology

Search angles: (1) category mapping ("ADE", "multi-harness orchestrator", Agent Client Protocol ecosystem); (2) per-product identity verification (official site, canonical repo, LICENSE, releases, owner); (3) first-party harness audit (publisher, license, form factor, ACP/CLI invocability); (4) adversarial searches (security, telemetry, ToS, Windows issues, brand collisions).

Source types: primary sources prioritized and opened directly — GitHub repositories and READMEs, the GitHub API for stars/forks/releases, official docs and product pages (onorca.dev, superset.sh, emdash.com, proliferate.com, getbb.app, ccswitch.io, antigravity.google, docs.x.ai, deepseekdocs.com, zcode.z.ai, mistral.ai), vendor telemetry/FAQ/security docs, YC company pages; secondary sources (MarkTechPost, innFactory, Developer's Digest, community directories) used only for independent criticism or context, always tagged.

Decision-critical claims verified by opening the source: Superset "Windows: not yet available" (README, opened); Emdash Windows MSI/portable install table + Apache-2.0 + telemetry opt-out (README, opened); Orca MIT license + platform badge + worktree/remote/mobile claims (README, opened); OpenChamber platform table and OpenCode bundling (README, opened); ccgui engine list and Windows installer (README + releases, opened); CC Switch OAuth-proxy ToS warning (official FAQ doc, snippet-level, double-sourced).

Limitations: no hands-on testing was performed; Windows stability claims rest mostly on vendor changelogs and commit history; no independent security audit exists for any product; star counts are market signal, not capability proof; some pages could only be read as search snippets (tagged accordingly).

Findings
A. The category, defined

Term

	

Definition (as used by the verified sources)




Model / provider

	

The LLM and its host (e.g., GLM-5.3 at Z.ai, Grok-4.7 at xAI).




Coding-agent harness

	

The scaffolding around a model: agent loop, tools, context, permissions, skills/hooks, MCP. innFactory: "The harness – such as Claude Code, Codex or Pi – gives the model tools, context, permissions and an environment." Claude Code, Codex, Grok Build, dsh, ZCode, Kimi Code, Qwen Code, Mistral Vibe are all harnesses.




Multi-model IDE

	

An editor that swaps models/providers inside one runtime (Cursor, Zed, VS Code). Zed and JetBrains now host external harnesses via ACP, blurring the line — but the IDE, not the harness, owns the workflow.




Native-harness desktop shell / ADE

	

A desktop app that launches multiple vendors' harnesses side by side, each in its own worktree, preserving each harness's own CLI, auth, permissions, and transcripts (Orca, Emdash, Superset, OpenChamber, bb, Proliferate, ccgui).




Configuration / router

	

A tool that switches providers/models inside one harness's config, or relays its API traffic (CC Switch, cc-switch-cli, cc-switch-web).

Why preserving the native harness matters. Each harness ships vendor-specific machinery — Claude Code's skills/hooks, Codex's sandboxed execution, Grok Build's plan-first workflow and subagents — and vendors increasingly bundle subscription benefits into the harness (Orca's account switcher tracks Claude/Codex usage and rate-limit resets on your existing plans; OpenChamber's opencode-claude plugin explicitly runs "through the official Claude Agent SDK on top of the claude CLI… Your login stays with Claude Code… The plugin never reads, copies, stores or sends tokens"). A generic multi-model runtime forfeits both the machinery and the subscription login. Counterpoint (steelman): a single strong harness with good provider-agnostic model routing (e.g., OpenCode-based OpenChamber, or Qwen Code's multi-provider endpoints) covers most single-user needs with one dependency instead of five, and ACP-based IDEs give you agent choice without a new desktop app — evidence that the multi-harness ADE is a power-user niche, not a default. Independent engineering comparisons that isolate the harness variable are only beginning to appear (aimultiple.com ran every agentic CLI on a single model to isolate orchestration quality [low-authority, methodology not fully published]); no rigorous controlled study of ADE vs. raw CLI productivity was found [no source found]. The practical case today is architectural: worktree isolation + one review surface + subscription reuse.

B. Verified identities (all metrics retrieved 2026-10-11 via GitHub API)

Product

	

Canonical repo / URL

	

Owner

	

License

	

Stars / Forks

	

Created

	

Latest release

	

Activity (≤90d)




Orca

	

github.com/stablyai/orca · onorca.dev

	

Stably AI (stablyai)

	

MIT

	

89,285 / 5,708

	

2026-03-17

	

v1.4.224 (2026-10-10)

	

Pushed 2026-10-11; ~13.5k commits




Superset

	

github.com/superset-sh/superset · superset.sh

	

superset-sh

	

Elastic License 2.0 (site); GitHub API: NOASSERTION

	

15,059 / 1,344

	

2025-10-21

	

— (active releases)

	

Pushed 2026-10-11




Emdash

	

github.com/generalaction/emdash · emdash.com

	

General Action (YC W26)

	

Apache-2.0

	

5,949 / 626

	

2025-08-28

	

v1.2.8 (2026-10-10)

	

Pushed 2026-10-10




OpenChamber

	

github.com/openchamber/openchamber · openchamber.dev

	

openchamber org (Ukraine)

	

MIT

	

11,416 / 1,261

	

2025-09-11

	

v2.2.0 (2026-10-08)

	

Pushed 2026-10-11




bb

	

github.com/get-bb/bb · getbb.app

	

get-bb

	

MIT

	

4,232 / 625

	

2026-02-24

	

frequent

	

Pushed 2026-10-11




ccgui (Desktop CC GUI)

	

github.com/zhukunpenglinyutong/desktop-cc-gui

	

zhukunpenglinyutong (CN)

	

No LICENSE file detected (GitHub API: none); winget lists MIT [low-authority]

	

4,459 / 402

	

2026-02-03

	

v1.1.3 (2026-10-10)

	

Pushed 2026-10-10




Proliferate / AnyHarness

	

github.com/proliferate-ai/proliferate · proliferate.com

	

proliferate-ai

	

AGPL-3.0

	

515 / 85

	

2026-04-30

	

—

	

Pushed 2026-10-08




CC Switch

	

github.com/farion1231/cc-switch · ccswitch.io

	

farion1231

	

MIT

	

142,542 / 9,501

	

2025-08-04

	

frequent

	

Pushed 2026-10-10

Identity notes: "AnyHarness" is real but is Proliferate's internal runtime ("the AnyHarness runtime that executes coding-agent sessions across harnesses", AGENTS.md), not a separate product — the prior report's "Proliferate / AnyHarness" pairing is confirmed but the naming conflates components. "bb / Get BB" is one product (repo get-bb/bb, site getbb.app). "Desktop CC GUI / ccgui" is one product, with an ecosystem of mirrors/forks (zhzy0077, xkun6, ysm827, Yi-Page, 17318584151…) and a different npm package named ccgui (@jun-a/ccgui) — cite the zhukunpenglinyutong repo to avoid collision. [established]

C. First-party harness audit (the claims that looked most like hallucinations — all verified)

Harness

	

Publisher / repo

	

License

	

Form factor

	

OSes

	

Invocable by third-party shells?




Claude Code

	

Anthropic

	

Proprietary (CLI free with subscription)

	

Terminal + IDE + desktop + web

	

Win/mac/Linux

	

Yes — de-facto standard; every ADE above launches it [established]




OpenAI Codex

	

OpenAI (github.com/openai/codex)

	

CLI open source; desktop app reported Feb 2026 [single-source, low-authority]

	

CLI + IDE + cloud + desktop

	

Win/mac/Linux

	

Yes — CLIs; 92,334★ reported [low-authority]




Grok Build

	

xAI — github.com/xai-org/grok-build, docs.x.ai/build (repo self-describes as "SpaceXAI's coding agent harness and TUI")

	

Apache-2.0

	

Fullscreen TUI + headless + ACP; browser OAuth or XAI_API_KEY

	

Prebuilt macOS/Linux/Windows; Windows build-from-source "best-effort"

	

Yes — documented ACP support; beta 2026-05-25, open-sourced ~2026-07-14/15 [established]




DeepSeek Harness (DSH)

	

DeepSeek — github.com/deepseek-ai/deepseek-harness, npm @deepseek-ai/dsh

	

MIT

	

Node CLI + Web UI + headless + ACP profile (@deepseek-ai/dsh-acp-app); DEEPSEEK_API_KEY

	

Node-based; Windows not tested by third-party guides

	

Yes — ACP + dsh-host-rpc (ccgui uses it as a native engine). Developer preview, breaking changes expected; npm 0.2.0-rc.2; repo 247,119★ [established]




ZCode (GLM)

	

Z.ai — github.com/zai-org/ZCode, zcode.z.ai

	

Apache-2.0

	

Electron desktop + web + CLI/TUI (one TS monorepo); GLM Coding Plan or API key

	

Desktop (SSH/WSL/Docker remote modes)

	

Yes — Orca lists ZCode; repo only 3 weeks old (created 2026-09-20, 7,658★) — too young for a canary [established]




Google Antigravity

	

Google — antigravity.google

	

Proprietary

	

Antigravity 2.0: standalone command center (macOS/Win/Linux) + IDE + CLI (headless docs) + extensions

	

macOS/Windows/Linux

	

Yes — CLI headless mode (ccgui wires it in); Orca account list includes Antigravity [established]




Kimi Code

	

Moonshot AI — github.com/MoonshotAI/kimi-code

	

MIT

	

TS CLI (kimi), subagents, /login OAuth or API key; kimi acp

	

Terminal; Windows needs Git for Windows (Git Bash shell)

	

Yes — documented ACP support; June 2026; legacy kimi-cli archived [established]




Qwen Code

	

Alibaba QwenLM — github.com/QwenLM/qwen-code

	

Apache-2.0

	

Terminal CLI (Gemini CLI v0.8.2 fork, now independent); qwen serve ACP daemon

	

Cross-platform Node

	

Yes — ACP; JetBrains lists it [established]




Mistral Vibe

	

Mistral — github.com/mistralai/mistral-vibe

	

Apache-2.0

	

Python CLI; Devstral 2; Vibe 2.0 subagents/skills; ACP clients (Zed/VS Code/JetBrains)

	

"Works on Windows, but we officially support and target UNIX environments" (README)

	

Yes — ACP [established]

D. Category separation — what each ranked product actually is

True multi-harness shells (launch native harness CLIs side by side)

	

Single-harness / multi-model environments

	

Routers & config managers (no harness preservation)




Orca — "if it runs in a terminal, it runs in Orca"; ships preconfigured Claude Code, Codex, Grok, DeepSeek Harness, ZCode, Antigravity, Kimi, Mistral Vibe, Qwen Code + any CLI; account switcher on existing Claude/Codex subscriptions

	

OpenChamber — MIT ADE built on OpenCode 2.x (bundles the OpenCode CLI); other vendors enter via OpenCode plugins (e.g., opencode-claude: Claude Pro/Max login through the official Claude Agent SDK + claude CLI) and Multi-run across up to 5 models — not native multi-harness in Orca's sense

	

CC Switch — switches provider configs for Claude Code, Claude Desktop, Codex, Gemini CLI, Grok Build, OpenCode, OpenClaw, Hermes, MiniMax Code; OAuth center signs in to ChatGPT/Copilot/xAI and re-exposes subscriptions — config plane, not a shell




Emdash — auto-detects ~25 installed agent CLIs (Claude Code, Codex, Cursor, OpenCode, Amp, Devin, Qwen Code, Droid, GitHub Copilot…); installs marker-tagged lifecycle hooks in each agent's user-level config

	

ZCode — single-vendor (GLM) workspace; and Antigravity — Google's own command center, parallel agents but within Google's stack

	

(Zed / VS Code / JetBrains host harnesses via ACP but remain editors — the prior report's classification of Cursor/VS Code/Zed as "multi-model IDE alternatives" is confirmed)




Superset, bb, ccgui, Proliferate — all run third-party CLI agents (Superset: "any CLI-based coding agent… never proxies model calls"; bb: Claude Code, Codex, Cursor, Pi, OpenCode, Grok, omp, Hermes; ccgui: 11 engines via dedicated Rust protocol adapters; Proliferate: "runs each agent through its native harness, so auth, tools, models, permissions, and transcript behavior stay intact")

	




	




E. Weighted Windows-first comparison

Scores 0–5, evidence-cited; UNKNOWN is not a score — unknown cells are excluded from the weighted sum and the coverage of evidence is shown separately. Weights: native Windows support/stability 20%; preserves native harness + subscription logins 20%; security/privacy/credential isolation 20%; parallel agents/worktrees/review/resume 15%; openness/license/provenance 10%; multi-machine/remote 10%; maturity/reversibility 5%.

Criterion (weight)

	

Emdash

	

Orca

	

OpenChamber

	

ccgui

	

bb

	

Superset

	

Proliferate

	

CC Switch




Windows support & stability (20%)

	

4.5 — MSI + portable .exe shipped in v1.2.8; early v1 Windows startup crashes fixed per changelog

	

4 — official docs claim macOS/Windows/Linux; win32 fix commits; latest release assets we could list showed only macOS/Linux artifacts → verify Windows artifact first

	

4 — desktop "for macOS, Windows, and Linux" (README)

	

4 — CC.GUI_1.1.3_x64-setup.exe (NSIS), winget package

	

1.5 — "Windows x64 installer… alpha: expect problems"; WSL2 path

	

0 — "Windows: not yet available" (README)

	

UNKNOWN — macOS download only, no Windows build found

	

5 — cross-platform Tauri (Win/mac/Linux)




Native harness + subscription logins (20%)

	

4.5 — auto-detect ~25 CLIs; marker lifecycle hooks; agents' own auth

	

5 — any CLI agent; account switcher hot-swaps Claude/Codex accounts on existing plans

	

3.5 — OpenCode core; Claude via Agent-SDK plugin with Pro/Max login (login preserved, harness UX not native)

	

4 — 11 engines, dedicated Rust adapters; DSH as native engine (dsh-host-rpc)

	

4 — 8 harnesses incl. Grok and Hermes

	

4.5 — any CLI agent; "never proxies model calls"

	

4.5 — "runs each agent through its native harness"

	

1 — config switcher only




Security / privacy / credential isolation (20%)

	

4 — local-first SQLite; "does not send your code or chats to Emdash servers"; SSH creds in OS keychain; telemetry optional, TELEMETRY_ENABLED=false kill switch

	

2.5 — yolo permission flags pre-applied to every agent by default; telemetry documented + opt-out (DO_NOT_TRACK / ORCA_TELEMETRY_DISABLED); no account system

	

4 — SECURITY.md; E2E-encrypted Private Relay; explicit "does not hijack OAuth, extract or replay browser tokens"

	

2.5 — "trust boundary" claimed for plugin SDK; no LICENSE file in repo; telemetry posture UNKNOWN

	

2.5 — production runs send anonymous telemetry by default (documented, anonymized)

	

4 — repos/worktrees stay local; cloud sync covers account metadata only

	

3 — self-hostable AGPL stack, but ships hosted control plane, model gateway, and agent_auth/billing server components

	

2 — OAuth reverse proxy "may violate OpenAI's Terms of Service" (own docs); tokens stored locally, never uploaded




Parallel / worktrees / review / resume (15%)

	

4.5 — worktree per task, diff review, PR creation, resumable sessions

	

5 — fan-out one prompt across 5 agents, diff annotation, session resume/hibernation

	

4 — Multi-run ×5 models, optional worktrees, Fusion, Session Goals, schedules

	

UNKNOWN — chat-client model; no worktree/review evidence found

	

4 — threads, worktrees, bb spawn, scheduled jobs

	

4.5 — 100+ agents, per-worktree previews, diff viewer

	

4.5 — worktrees, subagents, workflows

	

0.5 — session browsing only




Openness / license / provenance (10%)

	

4.5 — Apache-2.0, YC W26 company, public repo

	

4.5 — MIT, 13.5k commits

	

4.5 — MIT

	

1.5 — no LICENSE file detected; many mirrors; homepage points to mossx.ai

	

4 — MIT

	

3 — ELv2 (source-available, not OSI)

	

3 — AGPL-3.0; cloud components

	

4 — MIT




Multi-machine / remote (10%)

	

4.5 — SSH/SFTP remote projects incl. OpenSSH certificates

	

4.5 — SSH worktrees, Remote Orca Servers, mobile companion (iOS/Android), cloud VM (experimental)

	

4.5 — server/PWA, iOS/Android, QR-paired private relay

	

UNKNOWN

	

3 — runs via npx on remote machines

	

4 — remote control of connected computer; mobile (Pro, iOS 26+)

	

4 — cloud sandboxes; supervisor/worker targets

	

1 — local config tool




Maturity / support burden / reversibility (5%)

	

3.5 — 14 months old, 5.9k★, canary release channel exists

	

4 — 89k★, daily ships, but <7 months old

	

3.5 — 11.4k★, v2.2.0

	

3 — 4.5k★, v1.1.3

	

3 — "in active development… expect problems"

	

3.5 — 15k★

	

2 — 515★, 5 months old

	

4.5 — 142k★, 14 months




Weighted total (coverage)

	

4.35 (100%)

	

4.15 (100%)

	

3.98 (100%)

	

3.20 (75% — 2 UNKNOWN cells)

	

3.05 (100%)

	

3.25 (100%, but Windows = 0 is disqualifying)

	

3.72 (80% — Windows UNKNOWN is disqualifying)

	

2.40 (100%, wrong category)

Confidence penalties (applied separately from scores): ccgui −heavy (missing license + unknowns); Proliferate −heavy for Windows-first (no Windows build found); Orca −moderate (Windows artifact visibility + default-yolo posture); Emdash −moderate (young product, vendor-authored Windows evidence); Superset/bb/CC Switch scores are well-evidenced but category- or platform-disqualified.

F. Security, license, and provenance red flags

Orca launches every supported agent with full-autonomy flags by default — Claude --dangerously-skip-permissions, Codex --dangerously-bypass-approvals-and-sandbox, --yolo for Gemini/Cursor/Kimi/Copilot "plus the equivalent flag for every other agent that exposes one" (Orca docs). Worktree isolation ≠ process sandboxing; the docs themselves note worktrees "do not sandbox processes." For a canary, flip Settings → Agent Permissions → Manual first. [established]

CC Switch's OAuth features admit ToS risk in their own docs: "This may violate OpenAI's Terms of Service, carries the risk of account restrictions or suspensions." Do not use its OAuth Auth Center or Codex reverse proxy in the canary. Tokens are stored locally and "never uploaded" — but the flow itself is reverse-engineered. [established]

ccgui ships with no LICENSE file in the repository root (GitHub API reports no license; root listing confirms no LICENSE). A winget mirror claims MIT [low-authority, single-source]. Unlicensed = all rights reserved by default. Provenance further muddied by many mirror forks and a homepage pointing to mossx.ai. Not acceptable for a canary without resolution. [established]

Superset's license is Elastic License 2.0 (per superset.sh; the GitHub API reports NOASSERTION and a LICENSE file fetch at main 404'd) — source-available but not OSI-open; also no Windows build. [established]

Proliferate's Windows status is unverifiable — README and site offer "Download for macOS" only [no Windows source found]; and its AGPL repo contains a hosted control plane, model gateway, and agent_auth enrollment/billing modules — heavier server-side surface than "local-first" competitors. [established]

Telemetry defaults differ: Emdash — optional, documented kill switch; Orca — documented, opt-out available (Settings → Privacy, DO_NOT_TRACK=1, ORCA_TELEMETRY_DISABLED=1), never collects code/prompts/URLs per docs; bb — on by default in production runs ("anonymous usage telemetry… app starts, thread creation counts, user message counts, plugin installs"). [established]

No independent security audits or incident reports found for ANY product in this report. [no source found] All security statements are vendor-authored.

ZCode is 3 weeks old (created 2026-09-20) — treat as a harness to watch, not to put in a canary. DeepSeek Harness is a declared developer preview with expected breaking changes. [established]

Orca Windows artifact caveat: Orca's docs and download page state macOS/Windows/Linux support and the repo carries win32 fixes, but the latest GitHub release asset list we retrieved showed only macOS/Linux artifacts — possibly a Windows-specific distribution channel we could not confirm. Verify a Windows artifact exists before adopting. [single-source conflict, flagged]

G. Adversarial validation — how the prior report could have reached the wrong conclusion

Popularity-as-quality substitution. Ranking by GitHub stars puts Orca (89k★) and CC Switch (142k★) at the top, but stars measure attention, not fit: the #2 product (Superset) has no Windows build at all, and CC Switch is a config manager, not a multi-harness shell. DSH's 247k★ is a model-vendor hype signal for a developer-preview harness with breaking changes.

Brand-collision laundering through SEO directories. Toolify, ToolMage, "yetanotherorchestrator", aiidelist, vibecodinghub and similar pages republish vendor copy with no testing; "Emdash" also names an unrelated CMS (covered by Matt Mullenweg and wayf.ai — completely different product); "Orca" collides with Microsoft's MSI editor and OrcaSlicer; "Superset" with Apache Superset. A prior report fed by these directories can cite "sources" for a product that are actually about a different product with the same name.

Trusting marketing superlatives. "Orchestrate 100+ coding agents" (Superset), "The AI Orchestrator for 100x builders" (Orca), "any provider" (Emdash) — none of these claims distinguish native harness preservation from model switching inside one runtime, and none are load-tested. The prior report's category label was right, but the ranking did not weight platform support or security defaults, which flip the order for this user.

Additional mechanisms worth noting: conflating routers with shells (CC Switch ranked #8 as a "companion" blurs the line) and snapshot decay — this space ships daily (Orca v1.4.224 on 2026-10-10; Emdash v1.2.8 same day; OpenChamber v2.2.0 on 2026-10-08), so any report older than a few weeks is stale on versions and Windows status.

H. Prior-report claims: confirmed, contradicted, stale, ambiguous, or not verified

Claim

	

Status

	

Note




Category is "desktop ADE / multi-harness orchestrator"

	

Confirmed [established]

	

Vendors' own term; ACP ecosystem (Zed/JetBrains, agentclientprotocol.com) supports the framing




Orca ranks #1

	

Confirmed as most popular; contradicted for Windows-first

	

89k★, MIT, very active — but Emdash wins on Windows fit + security defaults




Superset #2

	

Contradicted for this use case

	

"Windows: not yet available" (README)




Emdash

	

Confirmed, with brand-collision ambiguity

	

generalaction/emdash (YC W26), Apache-2.0 — not EmDash CMS




Desktop CC GUI / ccgui

	

Confirmed

	

Tauri multi-engine client; license gap flagged




bb / Get BB

	

Confirmed

	

get-bb/bb; Windows alpha




OpenChamber

	

Confirmed

	

MIT, OpenCode-based ADE — note it's single-runtime at heart




Proliferate / AnyHarness

	

Confirmed (one product, two names)

	

AnyHarness is Proliferate's internal runtime; macOS-only download




CC Switch as companion control plane

	

Confirmed; risk-flagged

	

Real, 142k★ — OAuth proxy carries documented ToS risk




Cursor/VS Code/Zed are multi-model IDEs, not multi-harness shells

	

Confirmed with nuance

	

Zed/VS Code/JetBrains now host harnesses via ACP, so the boundary is softening




First-party harnesses incl. Grok Build, DeepSeek Harness, ZCode, Antigravity, Kimi Code, Qwen Code, Mistral Vibe

	

All confirmed

	

Details in section C; ZCode and DSH are too young/preview-grade for production reliance




(Implied) ranking applies to a Windows-first multi-machine user

	

Not verified / contradicted

	

Platform support was not weighted; two of the top products lack Windows builds

Recommendation

Run the canary: YES — with Emdash. [emerging]

Primary canary — Emdash (generalaction/emdash). First-class Windows (MSI + portable .exe, which makes the canary inherently reversible: no installer, no admin rights, delete the folder to remove), local-first with a telemetry kill switch, Apache-2.0, worktree-isolated parallel agents with diff review and resume, and SSH remote-project support that matches the multi-machine requirement. Main assumption it rests on: the user already holds (or will log into) the native harness CLIs with existing consumer subscriptions — Emdash orchestrates; it does not provide models.

Fallback — Orca (stablyai/orca). Choose it if mobile/remote steering and the largest agent matrix matter more than install-and-forget Windows stability. Preconditions: set Agent Permissions → Manual (never accept the default yolo flags in a canary), disable telemetry, and confirm a Windows artifact downloads before starting.

Explicit no-go conditions (any one disqualifies a product for this canary):

No native Windows build: Superset (stated), Proliferate (none found), bb (alpha installer — retry when stable).

Unresolved license/provenance: ccgui until a LICENSE file ships.

ToS-violating auth flows: CC Switch OAuth Auth Center / Codex reverse proxy (fine as a config switcher only, and not a multi-harness shell anyway).

Harness-side immaturity: ZCode (3 weeks old) and DeepSeek Harness (developer preview) as subjects of the canary — orchestrate them only after they stabilize.

Any product asking for admin rights, global installs, or valuable credentials during the canary.

60-Minute Canary Acceptance Plan (Emdash, Windows, fully reversible)

Ground rules: disposable test repo only; no production repositories; no paid API keys beyond subscriptions already in use; no admin rights; every step reversible in <5 minutes.

(5 min) Prep. Create C:\canary\ (or any deletable folder). Create a throwaway repo: git init canary-test, add a README, commit. Do not point anything at real projects.

(5 min) Zero-install acquisition. From github.com/generalaction/emdash/releases/latest download emdash-x64.exe (portable) — not the MSI. Run it from C:\canary\. No registry changes, no admin. Optionally note the sha/file size.

(3 min) Harden. In Settings, disable telemetry (or relaunch with TELEMETRY_ENABLED=false). Keep the default Manual/normal permission flow of the underlying CLIs.

(7 min) Add the disposable project. "Add project" → canary-test. Confirm Emdash detects installed agent CLIs (expect auto-detection of any of Claude Code, Codex, OpenCode, etc. you already have). Record the marker-tagged hook entries Emdash adds to each agent's user-level config (e.g., «внутренний путь лаборатории») — this is the only system-level touch; you'll remove it in rollback.

(10 min) Single-agent worktree test. Create a task "Add a hello() function and a unit test" with one agent you already pay for. Verify: the task runs in its own worktree/branch; the main working directory is untouched; the diff appears in Emdash's review view.

(10 min) Resume test. Close Emdash entirely, reopen, resume the session. Acceptance: conversation + worktree state restored; no lost task.

(10 min) Parallel two-agent test. Start a second task with a second harness (e.g., Codex if Claude Code ran the first). Acceptance: both run concurrently in separate worktrees; no cross-contamination of branches; both diffs reviewable in one place; agent statuses tracked.

(5 min) Multi-machine stretch (optional). Add an SSH target (Settings → Remote Projects) pointing at a non-production host; confirm connection with key-based auth and that credentials land in the OS keychain.

(5 min) Rollback drill. Quit Emdash. Delete Emdash's marker-hook entries from the agent user-level configs (recorded in step 4). Delete C:\canary\ (removes app, state, and the SQLite DB — local-first means nothing else persists). Delete the canary-test repo. Done: zero residue, no admin, no uninstaller.

Pass criteria: steps 5–7 all pass; rollback leaves no residue. Abort triggers: agents run without expected permission prompts; telemetry/network activity continues after the kill switch; unexpected non-provider network destinations; repeated Windows crashes.

Source Notes

All web sources retrieved 2026-10-11. GitHub metrics retrieved 2026-10-11 ~05:15 UTC via the GitHub API.

Source

	

Credibility

	

Last updated




Orca repo & README (stablyai/orca)

	

5/5

	

2026-10-11




Orca Docs — install, telemetry, agents & sessions, supported agents

	

5/5

	

current (2026)




Superset repo & README (superset-sh/superset)

	

5/5

	

2026-10-11




Superset product site (ELv2, local-first claims)

	

5/5

	

current (2026)




Emdash repo & README (generalaction/emdash)

	

5/5

	

2026-10-10




Emdash changelog (Windows fixes, canary channel)

	

5/5

	

2026-10




Y Combinator — Emdash company page

	

5/5

	

2026




OpenChamber repo & README

	

5/5

	

2026-10-11




OpenChamber docs — integrations (Claude Agent SDK auth)

	

5/5

	

current (2026)




openchamber/opencode-claude plugin

	

5/5

	

2026




bb repo & README (get-bb/bb)

	

5/5

	

2026-10-11




Proliferate repo, README & AGENTS.md (AnyHarness runtime)

	

5/5

	

2026-10-08




Proliferate product site

	

4/5

	

2026




CC Switch repo & README (farion1231/cc-switch)

	

5/5

	

2026-10-10




CC Switch FAQ — OAuth proxy ToS warning

	

5/5

	

2026




ccgui repo & README (zhukunpenglinyutong/desktop-cc-gui)

	

5/5

	

2026-10-10




Agent Client Protocol — spec, clients, ecosystem

	

5/5

	

current (2026)




Zed — Agent Client Protocol

	

5/5

	

current (2026)




JetBrains — ACP

	

5/5

	

current (2026)




Grok Build repo (xai-org/grok-build)

	

5/5

	

2026-09-29




Grok Build docs (docs.x.ai/build)

	

5/5

	

current (2026)




MarkTechPost — SpaceXAI open-sources Grok Build

	

3/5

	

2026-07-15




DeepSeek Harness repo (deepseek-ai/deepseek-harness)

	

5/5

	

2026-10-09




npm — @deepseek-ai/dsh

	

5/5

	

2026-10-01




DeepSeek Harness docs — quickstart (developer preview)

	

4/5

	

2026-09-09




Verdent — DSH install guide (MIT license, Windows not tested)

	

3/5

	

2026




ZCode product site (zcode.z.ai)

	

5/5

	

2026




Z.ai GLM Coding Plan (ZCode in supported tools)

	

5/5

	

current (2026)




Munder Difflin — ZCode analysis (repo dates, stars)

	

3/5

	

2026-10-08




Google Antigravity product site

	

5/5

	

2026-08-20 (blog)




Google Codelabs — Getting started with Antigravity (2.0 standalone, CLI)

	

5/5

	

2026




Kimi Code CLI repo (MoonshotAI/kimi-code)

	

5/5

	

2026




Qwen Code repo (QwenLM/qwen-code)

	

5/5

	

2026




Mistral Vibe repo (mistralai/mistral-vibe)

	

5/5

	

2026-10-09




Mistral AI — Devstral 2 & Vibe CLI announcement

	

5/5

	

2025-12-09




Simon Willison — mistralai/mistral-vibe link post

	

4/5

	

2025-12-09




innFactory AI harness profiles (Grok CLI, Kimi Code, Qwen Code, Claude Code)

	

3/5

	

2026




AgentConn — "Orca Built an IDE for Agent Fleets" (ADE category context; Musk harness quote)

	

2/5

	

2026




awesome-agent-orchestrators list

	

3/5

	

2026




Developers Digest — CC Switch guide

	

3/5

	

2026-05-05




aimultiple — Claude Code vs Codex harness comparison

	

3/5

	

2026




GitHub API metadata for all 12 repos (stars/forks/licenses/dates)

	

5/5

	

2026-10-11

Conflicts and caveats. (1) Orca's Windows support is claimed in official docs but the latest release's visible asset list showed only macOS/Linux artifacts — unresolved conflict, flagged in Red Flags #9. (2) ccgui's license is MIT per a winget mirror [low-authority] but no LICENSE file exists in the repo — treat as unlicensed. (3) The "Codex Desktop App launched February 2026" claim and the Musk "Grok performs significantly worse without its Build harness" quote each trace to a single low-authority source — kept out of the decision logic. (4) The Tool Nerd's "Codex CLI 92,334 stars" figure is a secondary snapshot; our GitHub API pull did not re-verify Codex's star count. (5) "EmDash" (emdash-cms/emdash) is an unrelated CMS; all Emdash citations here refer to generalaction/emdash. (6) No independent security audits exist for any product [no source found].

Open Questions

Does Orca ship a stable Windows artifact today? Official docs say yes; the release assets we could enumerate said otherwise. One direct download attempt would settle it. [single-source conflict]

Windows runtime stability of Emdash beyond vendor changelogs. The changelog shows Windows startup crashes found and fixed after v1 — real-world issue-tracker evidence on current Windows builds is thin; the canary itself is the test.

Do ccgui's protocol adapters for Kimi CLI / Grok CLI / Antigravity use official protocols (ACP/headless) or reverse-engineered ones? Not documented; matters for credential handling. [no source found]

Does preserving the native harness measurably improve outcomes over a single multi-provider harness? No controlled study found [no source found]; the ADE-vs-plain-CLI productivity question remains open.

When do Superset (Windows), bb (stable Windows), and Proliferate (Windows) ship? All three would re-enter the comparison the day they do.

Will OpenChamber's OpenCode-plugin approach and Orca's CLI-passthrough approach converge via ACP? ACP's growing registry suggests the category's center of gravity may shift from "launch terminals" to "speak protocol," which would change this whole ranking within months. [speculative]




## Source links harvested from page (a[href])
- https://github.com/stablyai/orca
- https://www.onorca.dev/
- https://agentconn.com/blog/orca-ade-agent-fleet-parallel-coding-agents-2026/
- https://nimbalyst.com/blog/orchestrating-claude-code-and-codex-together/
- https://dev.to/arshtechpro/orca-explained-the-agent-development-environment-for-running-ai-coding-agents-in-parallel-440n
- https://vibecodinghub.org/tools/orca
- https://www.reddit.com/r/ClaudeCode/comments/1wgutzu/orca_for_orchestration_anyone_using_it_here_with/
- https://github.com/andyrewlee/awesome-agent-orchestrators
- https://github.com/wshobson/agents
- https://aimultiple.com/agent-harness
- https://innfactory.ai/en/ai-harness/
- https://www.mindstudio.ai/blog/what-is-omniagent-meta-harness-orchestrate-claude-codex
- https://www.thetoolnerd.com/p/10-agent-harnesses-every-ai-builder
- https://www.mindstudio.ai/blog/what-is-meta-harness-ai-agents-omniagent
- https://agentclientprotocol.com/get-started/clients
- https://www.jetbrains.com/acp/
- https://github.com/formulahendry/acp-ui
- https://github.com/agentclientprotocol/agent-client-protocol
- https://zed.dev/acp
- https://github.com/agentclientprotocol
- https://acpserver.org/
- https://ai-sdk.dev/providers/community-providers/acp
- https://github.com/farion1231/cc-switch
- https://github.com/saladday/cc-switch-cli
- https://sourceforge.net/projects/cc-switch.mirror/
- https://github.com/topics/cc-switch
- https://github.com/Laliet/cc-switch-web
- https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/en/README.md
- https://docs.qcode.cc/en/docs/ide/cc-switch
- https://www.developersdigest.tech/blog/cc-switch-claude-code-codex-guide
- https://github.com/superset-sh/superset
- https://superset.sh/
- https://note.com/masa_wunder/n/n562b695ead6a?hl=en
- https://github.com/hoodiecollin/superset
- https://github.com/guillaumegay13/superset
- https://github.com/theswerd/superset
- https://github.com/benngarcia/superset
- https://github.com/generalaction/emdash
- https://emdash.com/
- https://www.toolify.ai/tool/emdash
- https://yetanotherorchestrator.app/apps/emdash/
- https://www.toolmage.com/en/tool/emdash-1/
- https://www.morphllm.com/emdash-ai-coding-agent
- https://www.ycombinator.com/companies/emdash
- https://github.com/zhukunpenglinyutong/desktop-cc-gui
- https://github.com/zhukunpenglinyutong/jetbrains-cc-gui
- https://jun-a.github.io/claude-code-gui-page/
- https://github.com/zhukunpenglinyutong/desktop-cc-gui/releases
- https://github.com/zhzy0077/desktop-cc-gui
- https://github.com/17318584151/desktop-cc-gui
- https://github.com/xkun6/desktop-cc-gui
- https://github.com/Yi-Page/desktop-cc-gui
- https://github.com/openchamber/opencode-claude
- https://github.com/openchamber/
- https://github.com/bastian-giersch/openchamber-claude
- https://github.com/openchamber/openchamber
- https://github.com/openchamber/openchamber/blob/main/CLAUDE.md
- https://skillsllm.com/skill/opencode-mobile
- https://github.com/openchamber/openchamber/releases
- https://docs.openchamber.dev/integrations/
- https://github.com/proliferate-ai/proliferate
- https://github.com/proliferate-ai/proliferate/blob/main/AGENTS.md
- https://github.com/proliferate-ai/proliferate/blob/main/README.md
- https://github.com/adhyaay-karnwal/proliferate-new
- https://proliferate.com/
- https://news.ycombinator.com/item?id <tg-id>
- https://github.com/topics/agent-harness
- https://github.com/RyanAlberts/best-of-Agent-Harnesses
- https://github.com/get-bb/bb
- https://github.com/whatif-dev/agent-coding-ide-bb
- https://github.com/erwinkn/bb
- https://getbb.app/
- https://github.com/ASRagab/bb
- https://github.com/salemsayed/bb
- https://github.com/get-bb/bb/releases
- https://github.com/mgpai22/bb
- https://github.com/xai-org/grok-build
- https://innfactory.ai/en/ai-harness/grok-cli/
- https://docs.x.ai/build/overview
- https://www.teamday.ai/harness/grok
- https://www.marktechpost.com/2026/07/15/spacexai-open-sources-grok-build-the-rust-agent-harness-tui-and-tool-layer-behind-its-coding-cli/
- https://codersera.com/blog/xai-grok-build-skills-connectors-guide-2026/
- https://www.developersdigest.tech/blog/grok-build-developer-guide-2026
- https://ai-x.chat/guide/grok-build/
- https://www.npmjs.com/package/@deepseek-ai/dsh
- https://deepseekdocs.com/en/docs/user-guide/cli
- https://www.verdent.ai/guides/agents/install-deepseek-harness-dsh
- https://deepseekdocs.com/en/docs/getting-started/quickstart
- https://aireiter.com/blog/deepseek-harness
- https://atoms.dev/blog/deepseek-harness
- https://github.com/HenryZ838978/deepseek-harness
- https://github.com/deepseek-ai/deepseek-harness/blob/master/apps/cli/README.md
- https://docs.z.ai/devpack/tool/others
- https://z.ai/subscribe
- https://flowtivity.ai/blog/zcode-glm-coding-agent-harness/
- https://zcode.z.ai/en
- https://alphasignal.ai/news/z-ai-opens-zcode-s-full-coding-agent-stack-to-developers-worldwide
- https://www.scriptbyai.com/zcode-glm-coding-agent/
- https://munderdiffl.in/blog/what-is-zcode/
- https://zcode.z.ai/en/docs/welcome
- https://github.com/mistralai/mistral-vibe
- https://terminaltrove.com/mistral-vibe-cli/
- https://blog.meetneura.ai/mistral-vibe-cli-guide/
- https://simonwillison.net/2025/Dec/9/mistral-vibe/
- https://www.therundown.ai/tools/mistral-vibe-cli
- https://api.github.com/repos/stablyai/orca
- https://raw.githubusercontent.com/stablyai/orca/main/README.md
- https://antigravity.google/
- https://antigravity.google/product/antigravity-ide/
- https://antigravity.google/download/
- https://codelabs.developers.google.com/getting-started-google-antigravity
- https://gizmodo.com/download/google-antigravity
- https://antigravityide.org/
- https://github.com/MoonshotAI/kimi-code
- https://www.marktechpost.com/2026/06/06/moonshot-ai-releases-kimi-code-cli-a-terminal-ai-coding-agent-built-in-typescript-for-next-gen-agents/
- https://github.com/MoonshotAI/kimi-cli
- https://innfactory.ai/en/ai-harness/kimi-code/
- https://dev.to/arshtechpro/kimi-code-cli-a-beginner-friendly-guide-to-moonshot-ais-terminal-coding-agent-39db
- https://www.kimi.ai/resources/kimi-k2-7-code
- https://rits.shanghai.nyu.edu/ai/introducing-qwen-code-alibabas-open%E2%80%91source-cli-for-agentic-coding-with-qwen3%E2%80%91coder/
- https://qwen.ai/qwencode
- https://www.cometapi.com/3-methods-to-use-qwen3-coder/
- https://github.com/QwenLM/qwen-code
- https://avinashsangle.com/blog/qwen-code-getting-started
- https://innfactory.ai/en/ai-harness/qwen-code/
- https://proliferate.com/platform
- https://docs.proliferate.com/admin/support
- https://www.microsoft.com/en-us/software-download
- https://www.microsoft.com/en-us/download
- https://www.onorca.dev/docs/agents/supported
- https://www.onorca.dev/docs/telemetry
- https://www.onorca.dev/docs/model/agents-sessions
- https://www.onorca.dev/docs/settings
- https://aiidelist.com/ide/orca-ai
- https://support.withorca.com/security--privacy/aZYfzuthvijeBDSZN7kfdA/security-white-paper/gw9jsuDKapoXYZh1HHbmPU
- https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/en/5-faq/5.2-questions.md
- https://ccswitch.co/docs/faq-questions.html
- https://developers.google.com/identity/protocols/oauth2/policies
- https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/en/2-providers/2.1-add.md
- https://github.com/farion1231/cc-switch/blob/main/docs/release-notes/v3.13.0-en.md
- https://www.reddit.com/r/ClaudeCode/comments/1mpy6q1/multiple_20m_subscription_or_1_100m_account/
- https://www.mossx.ai/download
- https://raw.githubusercontent.com/zhukunpenglinyutong/desktop-cc-gui/main/LICENSE
- https://raw.githubusercontent.com/superset-sh/superset/main/LICENSE
- https://github.com/zhukunpenglinyutong/desktop-cc-gui?tab=readme-ov-file
- https://www.wingetly.io/apps/zhukunpenglinyutong/ccgui
- https://github.com/ysm827/desktop-cc-gui
- https://github.com/ShinnChow/desktop-cc-gui
- https://emdash.sh/changelog
- https://emdash.com/changelog
- https://ma.tt/2026/04/emdash-feedback/
- https://www.makeuseof.com/windows-em-dash-update-and-shutdown/
- https://github.com/emdash-cms/emdash/issues
- https://wayf.ai/blog/emdash-cms-review/
- https://www.onorca.dev/docs/install
- https://www.onorca.dev/download
- https://learn.microsoft.com/en-us/windows/win32/msi/orca-exe
- https://stackoverflow.com/questions/48315763/how-to-install-orca-which-windows-sdks-contain-the-orca-msi-editing-tool
- https://www.reddit.com/r/sysadmin/comments/13gglz6/any_way_to_just_install_orca/
- https://github.com/deepseek-ai/deepseek-harness
