# DR26-10-11-HUB-01-0535 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# claude.ai Research report (Fable 5.1 Max, Research mode; служебный аккаунт account), collected 2026-10-11 from https://приватный чат (artifact: Native-Harness Desktop Shells for Windows Power Users: Verification Audit and Orca Canary Recommendation)

# Native-Harness Desktop Shells for a Windows-First, Multi-Machine Power User: Verification Audit and Canary Recommendation (retrieved October 11, 2026)

**Verdict: Yes, run a small reversible canary. Use Orca (stablyai/orca) as the canary and Emdash (generalaction/emdash) as the fallback. The category is real but immature [emerging], and the prior report's ranking is only partly right.** Retrieval date for all volatile metrics: October 11, 2026. Overall confidence in the recommendation: moderate. It depends on Orca's native Windows build behaving on your machines, and the canary is built to test exactly that.

## TL;DR

- **The category exists and most named products are real.** Orca, Superset, Emdash, ccgui, bb, OpenChamber, Proliferate and CC Switch all have identifiable canonical repositories [established]. They are not all the same kind of product, though: OpenChamber is a single-harness GUI for OpenCode, CC Switch is a config manager and router, and Superset ships no Windows build. Those three cannot be the answer for a Windows-first user who wants to preserve native harnesses.
- **For Windows-first use, Orca beats the prior #2 and #3.** Orca has a signed native Windows installer, dedicated Windows/WSL/SSH engineering, SSH remote worktrees, an MIT license, and wraps any CLI agent in a real terminal. Emdash is the most credible fallback: Apache-2.0, local-first, Windows x64 installer. Orca carries real credential-handling red flags, documented in its own issue tracker, so the canary must isolate credentials [emerging].
- **Most claimed first-party harnesses are verified, with caveats.** Grok Build (xAI), DeepSeek Harness (`@deepseek-ai/dsh`), Google Antigravity CLI (`agy`), Kimi CLI, Qwen Code and Mistral Vibe exist. Several speak ACP. ZCode is real, but it is Z.ai's own proprietary desktop ADE, not an embeddable harness, and I found no documented protocol for third-party shells to invoke it [established for existence; emerging for integration claims].

## Key Findings

### A. Category definitions and why the native harness matters

- **Model**: the LLM weights (e.g., GLM-5.2, grok-build-0.1).
- **Provider**: who serves the model and bills for it (Anthropic, OpenAI, xAI, Z.ai, a reseller, or a local server).
- **Coding-agent harness/runtime**: the agent loop that owns tools, context, permissions, session state and auth. Examples: Claude Code, Codex CLI, Grok Build, dsh, agy, Mistral Vibe.
- **Multi-model IDE**: one vendor's harness that lets you switch models inside it (Cursor, VS Code with Copilot). Zed is a partial exception, since it hosts external agents over ACP.
- **Native-harness desktop shell / ADE**: a desktop app that launches each vendor's own harness, either as a terminal process or through a structured protocol (ACP, Codex app-server, vendor JSON-RPC). It adds worktrees, review and orchestration around the harnesses without replacing them.
- **Configuration manager / router**: rewrites harness config files or proxies traffic to swap providers (CC Switch).

**Does preserving the native harness matter?** Yes for cost and behavior. The evidence on accuracy is mixed.

- The arXiv paper "The Scaffold Effect in Coding Agents" (Vats and Golev, submitted June 8, 2026) ran 300 trials: 3 harnesses × 2 models × 50 Terminal-Bench Pro tasks. Harness choice produced up to a 40× gap in tokens per solved task. Paired pass-rate differences stayed within 0–8 percentage points, and most confidence intervals included zero [emerging; preliminary workshop paper; tested open-source harnesses, not Claude Code or Codex].
- An ICML 2026 position paper on coding benchmarks by Gorinova et al. (Tessl; arXiv 2606.17799, June 16, 2026) argues the harness is "part of the measured object". It states that "practitioner reports document 4–10 point swings for Claude Opus 4.5 between standardised and custom scaffolds on SWE-Bench Verified", a figure it credits to Morph Labs. Its Table 1 shows Opus 4.6 scoring 79.8% under ForgeCode versus 58.0% under Claude Code on TerminalBench.
- A harness-native RL study (Du et al., LEGO-RL, arXiv 2608.17393, August 18, 2026) found model gains were harness-specific: "KAT-Coder-V2.5-Dev gains 3.4 points under Claude Code, the harness its authors report, but −0.4 under OpenHands SDK, where the untuned Qwen3.6-35B-A3B outscores it" [emerging].
- **Steelman of the opposite view:** if pass rates barely move across harnesses, a single good generic runtime with model switching (OpenCode-based OpenChamber, or a multi-model IDE) could be "good enough" and simpler. For a subscription-heavy user, though, the decisive factor is not accuracy. It is that consumer subscription logins (Claude Max, ChatGPT/Codex, SuperGrok) are bound to the vendors' own harnesses, and wrapping them through third-party providers creates terms-of-service and credential risk.

### B. Verified identities and canonical URLs

| Prior name | Verdict | Canonical site / repo | Owner / provenance | License | Stars / forks (Oct 11, 2026) | Latest activity | Native Windows |
|---|---|---|---|---|---|---|---|
| Orca | VERIFIED | onorca.dev; github.com/stablyai/orca | Stably AI | MIT (README badge and license section) | ~73.1k / 4.8k (rounded GitHub display; one third-party listing claims 86.5k, conflict unresolved) | Releases on Oct 7, Oct 8 (v1.4.223) and Oct 10, 2026 | Yes: `orca-windows-setup.exe`, code signing via SignPath |
| Superset | VERIFIED (collides with Apache Superset BI tool; superset.sh disambiguates itself) | superset.sh; github.com/superset-sh/superset | Superset Inc. | Elastic License 2.0 (source-available, not OSI open source) | ~15.1k shown on superset.sh; ~14.9k / 1.3k on GitHub | desktop-v1.37.0, Oct 8, 2026 | **No.** "Windows is not yet available" (superset.sh) |
| Emdash | VERIFIED (lookalike domains emdash.com and emdash.ai also claim the product; the repo's own link is emdash.sh) | emdash.sh / docs.emdash.sh; github.com/generalaction/emdash | General Action (YC W26) | Apache-2.0 (repo sidebar; changed from MIT in v0.4.48) | 5.6k / 578 | Stable v1.2.7 (Sep 27, 2026); canary v1.2.8-canary.100 (Oct 9, 2026) | Yes: x64 installer and MSI per docs; no ARM64 |
| Desktop CC GUI / ccgui | VERIFIED (many forks; upstream is zhukunpenglinyutong) | github.com/zhukunpenglinyutong/desktop-cc-gui | Individual maintainer (zhukunpenglinyutong) | NOT RETRIEVED | ~4.4k / 395 | v1.1.3, Oct 10, 2026 | UNKNOWN (Windows fixes appear in release notes; installer not confirmed) |
| bb / Get BB | VERIFIED | getbb.app; github.com/get-bb/bb | Sawyer Hood / Michael Yong (attribution conflicts across sources) | MIT | ~4.2k / 621 | v0.45.0 announced Oct 2, 2026 | Alpha only; desktop app is macOS Apple Silicon; Windows needs Git for Windows or WSL2 |
| OpenChamber | VERIFIED, but **single-harness** | openchamber.dev; github.com/openchamber/openchamber | Bohdan Triapitsyn (fan-made, unaffiliated with OpenCode) | MIT | ~11.4k / 1.3k | Org repos updated Oct 6, 2026 | Desktop for macOS/Windows/Linux claimed |
| Proliferate / AnyHarness | VERIFIED (older docs.proliferate.com describes a pre-pivot "background agents" product) | proliferate.com; github.com/proliferate-ai/proliferate | Proliferate (YC S25) | AGPL-3.0 (per founder's Show HN post) | ~505 / 84 | NOT RETRIEVED | "Windows (beta)" docs page [single-source] |
| CC Switch | VERIFIED (forks such as cc-switch-web exist; only official site is ccswitch.io) | ccswitch.io; github.com/farion1231/cc-switch | Jason Young (farion1231) | MIT per secondary guides; LICENSE not retrieved | ~139k / 9.4k | v3.19.1 (Jul 31, 2026) newest confirmed; main branch is ahead | Yes: MSI and portable zip, including ARM64 |
| Cursor / VS Code / Zed | Correctly classed as multi-model IDEs | n/a | Anysphere / Microsoft / Zed Industries | Mixed | n/a | n/a | Yes |

### C. Classification: true multi-harness shells vs. single-harness tools vs. routers

| Class | Products | Integration mechanism (primary evidence) |
|---|---|---|
| **True multi-harness shells (terminal-wrapping)** | Orca, Superset, Emdash | Orca: "if it runs in a terminal, it runs in Orca", with state from OSC titles and agent hooks. Superset: "works with any CLI-based coding agent". Emdash: auto-detects provider CLIs and installs marker-tagged hooks in each agent's user-level config. |
| **True multi-harness shells (structured protocol adapters)** | bb, ccgui, Proliferate | bb: adapters for Codex (app-server protocol), Claude Code, Pi and any ACP agent. ccgui: "dedicated protocol adapter in the Rust backend… not scraped off a terminal screen". Proliferate: Rust AnyHarness runtime "runs each agent through its native harness". |
| **Single-harness, multi-provider GUI** | OpenChamber | Runs on the OpenCode SDK. Claude access is through an "Anthropic provider plugin for OpenCode", not Claude Code. |
| **Single-harness multi-model IDEs** | Cursor, VS Code + Copilot; Zed partly (hosts external ACP agents) | Vendor's own agent loop |
| **Router / config manager** | CC Switch | Rewrites «внутренний путь лаборатории»/`~/.codex` config. Optional "Routing" and "Aggregation" proxies. A "Codex OAuth reverse proxy" reuses a ChatGPT account inside Claude Code. |

Interpretation: terminal-wrapping preserves harness behavior most faithfully, because it runs the real TUI. Structured adapters give richer UI but can lag harness changes. The bb author concedes that "Normalizing events does not make Codex, Claude Code, Pi, and ACP agents identical."

### D. Claimed first-party harnesses: audit

| Harness | Exists? | Publisher | Form factor / OS | License | Third-party invocation |
|---|---|---|---|---|---|
| Claude Code | Yes | Anthropic | CLI, desktop | Proprietary | Stable CLI; wrapped by every shell above |
| OpenAI Codex | Yes | OpenAI | CLI, app | Open-source CLI | CLI plus app-server protocol (used by bb) |
| Grok Build | Yes. Launched May 14, 2026 (early beta); expanded to SuperGrok/X Premium+ on May 25, 2026 | xAI | Terminal CLI; Windows PowerShell installer | Apache 2.0: MarkTechPost (July 15, 2026) reports the Rust CLI/TUI/agent runtime was published as xai-org/grok-build. Appwrite's blog cites a July 16 announcement that data retention is now "off by default" [secondary sources; MarkTechPost dates the early beta to May 25, which conflicts with May 14] | `grok agent stdio` runs it as an ACP agent [single-source, secondary] |
| DeepSeek Harness | Yes. `@deepseek-ai/dsh`, released Aug 13, 2026 alongside V4-Pro GA | DeepSeek (deepseek-ai/deepseek-harness) | Node runtime with local web UI (`npx @deepseek-ai/dsh web`, port 3080); developer preview | MIT | JSON-RPC agent composition (used in arXiv 2608.01347); ACP not confirmed. **Collision:** an unrelated PyPI `deepseek-harness` by Henry Zhang also exposes a `dsh` command |
| ZCode (GLM) | Yes. Launched Wednesday, July 2, 2026 (VentureBeat). Z.ai's X post: "Introducing ZCode, the official development environment for GLM-5.2" | Z.ai (formerly Zhipu AI) | Desktop ADE for macOS/Windows; Linux beta; zcode.z.ai; bring-your-own-key for third-party models | Proprietary app (GLM-5.2 weights are MIT) | **No documented protocol found.** It is a competing shell, not an embeddable harness |
| Google Antigravity | Yes. IDE plus `agy` CLI | Google | Native Windows/macOS/Linux; per-user Windows install under AppData | Proprietary (free preview) | Listed in the ACP Registry (`agy_acp_server.par`) |
| Kimi Code / Kimi CLI | Yes | Moonshot AI | CLI | UNKNOWN | ACP (JetBrains ACP page) |
| Qwen Code | Yes | Qwen (Alibaba) | CLI | Open source | ACP (JetBrains ACP page) |
| Mistral Vibe | Yes. v2.26.1 on PyPI, Oct 9, 2026 (Windows wheel) | Mistral AI | CLI, VS Code, web | Apache 2.0 | ACP (`vibe-acp`; WinGet `MistralAI.MistralVibe.ACP`). "Works on Windows, but we officially support and target UNIX environments" |

## Details

### E. Weighted Windows-first comparison (0–5 per criterion; evidence-only; UNKNOWN is excluded from the sum and flagged)

| Criterion (weight) | Orca | Emdash | bb | Proliferate | Superset | ccgui |
|---|---|---|---|---|---|---|
| Native Windows (20%) | 4: signed .exe, a dedicated Windows contributor, frequent Windows crash-loop fixes | 3.5: x64 installer/MSI; repos inside WSL not supported [low-authority competitor source] | 1: alpha; desktop is Apple Silicon only | 1.5: beta [single-source] | 0: not available | UNKNOWN |
| Native harness + subscription logins (20%) | 4: real CLIs, "own subscription"; history of Claude OAuth clobbering bugs | 4: detects installed CLIs; writes hooks into agent configs | 4.5: Codex app-server/ACP adapters; existing CLI credentials | 4: native-harness claim; LLM traffic "flows through the gateway" in its architecture | 4: any CLI agent | 3: adapters, but imports CC Switch third-party channels |
| Security / privacy / credential isolation (20%) | 2.5: opt-out anonymous telemetry; open pairing-token security issues; Copilot config permission fix | 3: "does not send your code or chats to Emdash servers"; SSH credentials in OS keychain; telemetry UNKNOWN | UNKNOWN (an "Account Pooler" across accounts is a concern) | UNKNOWN (hosted control plane, model gateway) | UNKNOWN | UNKNOWN |
| Parallel / worktrees / review / resume (15%) | 5 | 4.5 (worktrees, diffs, resumable sessions via hooks) | 4 | 4.5 | 5 | UNKNOWN |
| Openness / license / provenance (10%) | 4: MIT, signed; reproducible builds UNKNOWN | 4: Apache-2.0 | 4: MIT | 4: AGPL-3.0 | 2.5: ELv2 | UNKNOWN license |
| Multi-machine / remote (10%) | 5: SSH worktrees, Windows SSH hosts, WSL, mobile | 3.5: SSH remote | 4: remote machines, tunnel | 4: cloud sandboxes | 5: remote hosts, iPhone | UNKNOWN |
| Maturity / support / reversibility (5%) | 3: ships near-daily, so high churn | 3.5: stable plus canary channels | 2: "in active development" | 2: small, mobile still beta | 3 | 2 |
| **Weighted score (known cells only)** | **3.90 / 5** | **3.70 / 5** (security partly UNKNOWN) | 2.75 / 4.0 known weight | 2.85 / 4.0 known weight | 2.45 / 4.0 known weight; **fails Windows** | not scorable |
| Confidence | moderate | moderate-low | low | low | moderate (in the no-go) | very low |

How to read this: Orca and Emdash are the only candidates with enough cited evidence to score fully. bb and Proliferate may be architecturally superior (structured adapters), but on Windows they are alpha/beta. Superset is excluded by its own homepage statement, not by an unknown.

### F. Security, license and provenance red flags

- **Orca credential races.** Issue #9582 documents Orca rewriting «внутренний путь лаборатории» with an expired snapshot. That triggered Anthropic token-family revocation and repeated 401s/"Login expired". Issue #13165 (Aug 2026) reports the same loop persisting on 1.4.176 with multiple accounts. Both are macOS reports, but the mechanism is the account-switcher, not the OS [established from primary issue tracker].
- **Orca remote pairing.** Open security issues #17397, #17398 and #17399 describe several weaknesses: bearer credentials inside `orca://` custom-scheme links, pairing tokens that become permanent device credentials, and a browser client that keeps a runtime-scoped bearer token in localStorage [established; status of fixes not verified].
- **Orca fixed bug.** Orca's release notes disclose that trusting a folder for Copilot could leave `~/.copilot/config.json`, which can hold login tokens, readable by other users. v1.4.219/v1.4.220 did the same on SSH hosts. It is now fixed.
- **Orca on Windows.** Antivirus kept deleting the old Windows `orca` CLI launcher, so it was replaced with a native one. That is a positive response, but it shows EDR friction is real on managed Windows machines.
- **Orca telemetry.** Anonymous usage telemetry is on in packaged builds unless you opt out. The docs say it never sends prompts, file contents, paths or repo names. Opt-out is via Settings → Privacy, `DO_NOT_TRACK=1` or `ORCA_TELEMETRY_DISABLED=1`.
- **Emdash and Orca both modify agent configs.** Both write hooks into user-level agent configuration. Emdash says its hooks are marker-tagged and inert outside Emdash, but rollback must include removing them.
- **Superset license.** ELv2 is source-available, not open source.
- **CC Switch.** Its Codex OAuth reverse proxy and aggregation routing change the authentication path. That defeats the goal of preserving native subscription behavior and may conflict with vendor terms [speculative on ToS; established on mechanism].
- **bb.** The "Account Pooler" routes Claude/Codex across pooled accounts. That is a ToS and credential-sharing concern [speculative].
- **SEO and lookalike pages.** grokbuild.homes, deepseek-code.com, dsh-plugin.org (self-described unofficial), and emdash.com/emdash.ai versus the repo's emdash.sh are all examples. Use only vendor domains and canonical repos for installers.

### G. Audit of the prior report's claims

| Claim | Status |
|---|---|
| Category = "desktop ADE / multi-harness orchestrator" | **Confirmed.** Orca, Emdash, ZCode and OpenChamber all self-describe as ADEs. |
| Orca #1 | **Confirmed for Windows-first** [emerging] |
| Superset #2 | **Contradicted for Windows-first**: no Windows build |
| Emdash #3 | **Confirmed as a strong fallback** |
| ccgui #4 | **Ambiguous**: real, active, protocol adapters; license and Windows not verified |
| bb #5 | **Confirmed real; stale for Windows**: alpha only |
| OpenChamber #6 | **Miscategorized**: single-harness (OpenCode) GUI |
| Proliferate / AnyHarness #7 | **Confirmed real**; Windows beta [single-source]; small and early |
| CC Switch as companion control plane | **Partly contradicted**: it is a router that can override native auth paths; use read-only, if at all |
| Cursor/VS Code/Zed as multi-model IDEs | **Confirmed**, with the nuance that Zed hosts ACP agents |
| Grok Build, DeepSeek Harness, Antigravity, Kimi, Qwen, Mistral Vibe are first-party harnesses | **Confirmed** |
| ZCode as a first-party harness for shells | **Contradicted in form**: it is a standalone proprietary ADE |

**Three-plus ways the prior report could have gone wrong:**
1. It ranked by star counts, which measure popularity, not Windows capability (Superset has more stars than Emdash and no Windows build).
2. It treated "supports Claude Code" as one thing, when terminal-wrapping, protocol adapters and provider plugins (OpenChamber) preserve native behavior to very different degrees.
3. It folded a router (CC Switch) into a "preserve native harness" stack even though its proxies replace the native auth path.
4. It listed ZCode as an embeddable harness when it is a competing shell.
5. It risked trusting lookalike domains for Emdash, Grok Build and DeepSeek Harness.

**Decision-critical claims opened and confirmed against the source:**
- Orca README states MIT, macOS/Windows/Linux, a Windows `.exe`, and SignPath Windows code signing.
- Superset's homepage states Windows is not yet available.
- The arXiv Scaffold Effect abstract states the 40× token gap and 0–8 pp pass-rate range.
- The Orca issue #9582 text documents credential clobbering.
- The Emdash repository page states local-first operation and hook installation.

## Recommendations

**Canary: Orca.** **Fallback: Emdash.** **Main assumption:** Orca's terminal-wrapping model, with the account switcher left unused, preserves each vendor's native login and behavior on your Windows machines without credential side effects.

Trade-offs:
- **Orca** has the strongest Windows, remote and mobile story, but the largest attack surface (relay, pairing, account switching) and very high release churn.
- **Emdash** is simpler, local-first and Apache-2.0, but Windows is less tested than macOS, it has no ARM64 build, and it does not support repos inside WSL.
- **Watch, don't adopt yet:** bb for structured-adapter design, Proliferate for cloud handoff, ccgui for breadth of Chinese-ecosystem harnesses (dsh, Kimi, Qoder).

**No-go conditions (abort and roll back if any occurs):**
- Any write to «внутренний путь лаборатории», `~/.codex/auth.json` or `~/.copilot/config.json` that you did not trigger, or any forced re-login of a harness during the canary.
- SmartScreen or EDR blocking a binary that is not signed by the expected publisher.
- Outbound connections other than telemetry (opted out), GitHub and the vendors' own APIs.
- Agent hooks that remain after uninstall and cannot be removed cleanly.
- Any harness running noticeably differently inside the shell than in a plain terminal (different model, permissions or session format).

### 60-minute canary acceptance plan

1. **Prep (10 min).**
   - Create a fresh Windows local user, or a Windows Sandbox/VM, with no valuable credentials.
   - Back up and hash `%USERPROFILE%\.claude`, `.codex` and `.copilot` if they exist.
   - Create a disposable repo (`git init canary-repo`, one commit) outside any production tree.
   - Set `ORCA_TELEMETRY_DISABLED=1` and `DO_NOT_TRACK=1` for that user.
2. **Install (5 min).**
   - Download `orca-windows-setup.exe` only from the stablyai/orca GitHub releases.
   - Check the Authenticode signature (Properties → Digital Signatures).
   - Record where it installs and which per-user startup entries it creates.
3. **Harness test (20 min).**
   - Log in to two harnesses with throwaway or included-plan accounts, e.g. Claude Code and Codex, or the free tiers of `agy` and Mistral Vibe.
   - Log in from a plain terminal first, then launch them inside Orca, in two worktrees of the canary repo.
   - Confirm the same version, model, slash commands, permission prompts and session resume (`claude --resume`, `agy -c`) as outside Orca.
   - Do not use Orca's account switcher, mobile pairing or remote server.
4. **Isolation checks (10 min).**
   - Re-hash the credential files, then diff the agent config files to list the hooks Orca added.
   - Run `netstat -b` or Resource Monitor to check outbound hosts.
   - Kill and restart Orca to confirm terminal scrollback and session survival.
5. **Review and merge test (5 min).** Annotate a diff, merge one worktree, and confirm `git worktree list` cleanup.
6. **Rollback (10 min).**
   - Uninstall via Apps & Features and delete Orca's AppData folders.
   - Remove the Orca hook entries from the agent configs, or restore the backups.
   - Delete the canary repo and worktrees, then re-verify the hashes.
   - If anything fails, repeat steps 2–6 with the Emdash x64 installer from docs.emdash.sh.

**Acceptance:**
- Both harnesses behave the same inside and outside Orca.
- No unexpected credential writes.
- Clean uninstall.
- Restart survival works.

## Caveats

- All star and fork counts are rounded GitHub display values retrieved October 11, 2026. Exact API counts were not retrievable, and Orca's count conflicts between sources.
- Orca's latest release tag also conflicts between sources (v1.4.223 on Oct 8 plus an untagged Oct 10 release, versus an older v1.4.211 reported elsewhere). I treat the release page as authoritative.
- Several harness claims are secondary only: Grok Build's Apache 2.0 open-sourcing (reported by MarkTechPost on July 15, 2026) and its ACP mode, and the claim that Gemini CLI was retired in June 2026.
- The harness-effect research is preliminary. It tested open-source harnesses rather than Claude Code or Codex directly.
- Emdash's Windows limitations come from a competitor's comparison page [low-authority].
- Licenses for ccgui and CC Switch, and Proliferate's Windows beta status, are unverified against primary files.

## Sources (artifact md_citations, url | title)
- https://github.com/FlorianBruniaux/claude-code-ultimate-guide/blob/main/guide/core/agent-harness.md | github
- https://arxiv.org/abs/2607.22585 | arxiv
- https://arxiv.org/html/2607.22585v1 | arxiv
- https://github.com/stablyai/orca | github
- https://github.com/stablyai/orca/releases | github
- https://superset.sh/ | superset
- https://github.com/superset-sh/superset/releases/tag/desktop-v1.37.0 | github
- https://github.com/generalaction/emdash/releases/tag/v0.4.48 | github
- https://github.com/generalaction/emdash | github
- https://github.com/generalaction/emdash/releases | github
- https://github.com/generalaction/emdash/releases/tag/v1.2.8-canary.100 | github
- https://docs.emdash.sh/installation | emdash
- https://runpane.com/compare/emdash | runpane
- https://github.com/zhukunpenglinyutong/desktop-cc-gui | github
- https://github.com/zhukunpenglinyutong/desktop-cc-gui/releases/tag/v1.1.3 | github
- https://github.com/zhukunpenglinyutong/desktop-cc-gui/releases/tag/v1.1.2 | github
- https://github.com/get-bb/bb | github
- https://x.com/sawyerhood/status/2085039905529597982 | x
- https://daily.dev/posts/bb-an-ide-where-your-coding-agents-work-together-h6km1494m | daily
- https://x.com/get_bb_app | x
- https://getbb.app/ | getbb
- https://github.com/openchamber/openchamber | github
- https://github.com/awesome-opencode/awesome-opencode/discussions/55 | github
- https://openchamber.dev/ | openchamber
- https://docs.proliferate.com | proliferate
- https://news.ycombinator.com/item?id <tg-id> | ycombinator
- https://proliferate.com/docs | proliferate
- https://github.com/farion1231/cc-switch | github +2
- https://github.com/Laliet/cc-switch-web | GitHub - Laliet/cc-switch-web: ⭐️ A cross-platform web-based All-in-One assistant tool for Claude Code, Codex & Gemini CLI, based on CC Switch.
- https://docs.aihubmix.com/en/api/cc-switch | CC Switch Setup Guide - AIHubMix
- https://www.developersdigest.tech/blog/cc-switch-claude-code-codex-guide | developersdigest
- https://github.com/farion1231/cc-switch/releases/tag/v3.19.1 | github
- https://knightli.com/en/2026/05/06/cc-switch-ai-cli-manager/ | knightli
- https://www.onorca.dev/docs/model/agents-sessions | onorca
- https://github.com/guillaumegay13/superset | GitHub - guillaumegay13/superset: Superset is an agentic IDE to orchestrate 100+ coding agents in parallel. Run any agent with your own subscription. · GitHub
- https://flaviocopes.com/bb-agentic-ide/ | flaviocopes
- https://github.com/zhzy0077/desktop-cc-gui | github
- https://github.com/proliferate-ai/proliferate | github
- https://deepwiki.com/proliferate-ai/proliferate | deepwiki
- https://github.com/openchamber/ | github
- https://github.com/farion1231/cc-switch/releases/tag/v3.12.3 | github
- https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/en/2-providers/2.1-add.md | github
- https://blog.buildfastwithai.com/grok-build-xai-cli-ai-agents-2026 | buildfastwithai
- https://www.buildfastwithai.com/blogs/grok-build-xai-cli-ai-agents-2026 | buildfastwithai
- https://www.developersdigest.tech/blog/grok-build-developer-guide-2026 | developersdigest
- https://codersera.com/blog/xai-grok-build-skills-connectors-guide-2026/ | codersera
- https://aireiter.com/blog/deepseek-harness | aireiter
- https://atoms.dev/blog/deepseek-harness | atoms
- https://deepseek-code.com/ | deepseek-code
- https://www.developersdigest.tech/blog/deepseek-harness-dsh-first-look | developersdigest
- https://arxiv.org/pdf/2608.01347 | arxiv
- https://www.llmreference.com/agents/zcode | llmreference
- https://www.tonyreviewsthings.com/zcode-free-ai-coding-ide-glm-5-2/ | tonyreviewsthings
- https://en.wikipedia.org/wiki/Google_Antigravity | Wikipedia
- https://antigravity.google/docs/cli/install/ | antigravity
- https://zed.dev/acp/agent/antigravity-acp | zed
- https://www.jetbrains.com/acp/ | jetbrains
- https://pypi.org/project/mistral-vibe/ | pypi
- https://docs.mistral.ai/vibe/code/choose-cli-vscode-web-sessions | mistral
- https://docs.mistral.ai/mistral-vibe/introduction | mistral
- https://github.com/mistralai/mistral-vibe | github +2
- https://docs.mistral.ai/getting-started/quickstarts/vibe-code/install-cli | Install the Vibe CLI and send your first prompt
- https://winstall.app/apps/MistralAI.MistralVibe.ACP | Install Mistral Vibe (ACP mode) with WinGet
- https://github.com/stablyai/orca/releases/tag/v1.4.220 | github
- https://github.com/stablyai/orca/issues/9582 | github
- https://github.com/proliferate-ai/proliferate/blob/main/AGENTS.md | github
- https://github.com/zhukunpenglinyutong/desktop-cc-gui/releases | github
- https://www.onorca.dev/docs/telemetry | onorca +2
- https://github.com/stablyai/orca/issues/17398 | Security: exchange pairing tickets for per-device credentials · Issue #17398 · stablyai/orca
- https://getbb.app/changelog | getbb
- https://github.com/saurabh-statisfy/superset | github
- https://github.com/stablyai/orca/issues/13165 | github
- https://github.com/stablyai/orca/issues/17399 | Security: replace browser client localStorage bearer token with proof-of-possession · Issue #17399 · stablyai/orca
- https://github.com/stablyai/orca/issues/17397 | Security: replace credential-bearing custom pairing URLs with verified app links · Issue #17397 · stablyai/orca
- https://github.com/stablyai/orca/releases/tag/v1.4.219 | github
- https://grokbuild.homes/ | grokbuild +2
- https://dsh-plugin.org/ | DeepSeek Harness Plugins - DSH Plugin Market Install
- https://sourceforge.net/projects/emdash.mirror/ | Emdash download
- https://www.eesel.ai/blog/zcode | eesel
- https://alphasignal.ai/news/z-ai-s-zcode-ships-a-desktop-agent-built-around-glm-5-2-s-1m-token-window | alphasignal
- https://github.com/zhukunpenglinyutong/desktop-cc-gui/releases/tag/v1.0.2 | github
- https://www.codeagentswarm.com/en/guides/how-to-use-antigravity-cli | codeagentswarm
