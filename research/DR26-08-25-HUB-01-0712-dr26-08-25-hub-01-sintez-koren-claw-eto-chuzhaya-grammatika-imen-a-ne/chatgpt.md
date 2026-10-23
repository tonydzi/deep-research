# DR26-08-25-HUB-01-0712 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Deep-research report: the “CLAW” name cluster

**Snapshot date: 2026-08-25.**

**ESTIMATE — Scope.** “Notable” here means the principal `claw`-named agent projects with roughly 250+ GitHub stars, official OpenClaw infrastructure, institutionally significant projects, registry/product collisions, and every item explicitly requested. Typographical forks and hundreds of abandoned single-purpose copies are excluded.

**UNKNOWN — Exhaustiveness.** No changing GitHub/npm/PyPI census can prove that every low-visibility project has been found. The table is a decision-grade map, not a mathematically complete trademark clearance.

**FACT — Metric limitation.** GitHub stars and forks below are snapshots taken on 2026-08-25; they measure repository attention, not active installations, users, or commercial revenue.

## Executive conclusion

**FACT — OpenClaw is overwhelmingly the largest relevant project:** `openclaw/openclaw` had **387,499 stars and 81,352 forks** on 2026-08-25 ([GitHub API, accessed 2026-08-25](https://api.github.com/repos/openclaw/openclaw)).

**FACT — OpenClaw’s name itself ultimately descends from Claude.** Peter Steinberger says the November 2025 name “Clawd” was a playful Claude/claw pun; Anthropic asked for reconsideration; the project briefly became Moltbot and then OpenClaw, where “Claw” officially denotes its lobster heritage ([OpenClaw announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)).

**ESTIMATE — Current semantics.** In agent discourse, bare **“claw” now primarily means an OpenClaw-like persistent personal-agent system**, not a generic animal metaphor. “Claude with a claw” remains the historical substrate and a strong secondary reading, especially in names such as ClaudeClaw and Claw Code.

**ESTIMATE — Branding verdict.** Bare `CLAW`, `claw-consensus`, or `clawrush` will usually be interpreted first as an OpenClaw-family project, an OpenClaw alternative, or Claude-adjacent tooling. A small independent lab can ride that attention only when the product genuinely belongs to that category. For a multi-machine consensus protocol, the association creates more classification noise than useful borrowed demand.

**ESTIMATE — Recommendation.** If the rebranding cost is still low, leave the root for the primary public brand. If preserving existing equity is important, use a distinctive compound such as `claw1024` as the mandatory parent brand—not bare CLAW—and explicitly state that the lab is independent of OpenClaw and Anthropic.

---

# 1. Census

| Name | What it does | Steward | Stars / traction | Name origin |
|---|---|---|---|---|
| **[OpenClaw](https://github.com/openclaw/openclaw)** | **FACT —** Self-hosted personal-agent platform running on the user’s machine and reachable through WhatsApp, Telegram, Discord, Slack, Teams and other channels ([official announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)). The official npm package is [`openclaw`](https://www.npmjs.com/package/openclaw). | **FACT —** OpenClaw Foundation is the present project steward; its site says no single company controls it ([Foundation, accessed 2026-08-25](https://www.openclaw.org/)). Peter Steinberger joined OpenAI, which sponsors the project ([Steinberger, 2026-02-14](https://steipete.me/posts/2026/openclaw)). | **FACT —** 387,499 stars; 81,352 forks ([GitHub API, 2026-08-25](https://api.github.com/repos/openclaw/openclaw)). | **FACT —** Claude pun → lobster/community identity. “Clawd” was explicitly a Claude pun; “OpenClaw” preserves the lobster heritage ([official naming history, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)). |
| **[Claw Code](https://github.com/ultraworkers/claw-code)** | **FACT —** Rust agent/code CLI described as an agent-managed “museum exhibit,” with active work directed toward Gajae-Code/LazyCodex ([repository, accessed 2026-08-25](https://github.com/ultraworkers/claw-code)). | **FACT —** `ultraworkers`; repository disclaims Anthropic affiliation ([repository, accessed 2026-08-25](https://github.com/ultraworkers/claw-code)). | **FACT —** 195,112 stars; 108,951 forks ([GitHub API, 2026-08-25](https://api.github.com/repos/ultraworkers/claw-code)). **UNKNOWN —** The unusually high fork/star ratio does not establish actual usage. | **ESTIMATE —** Primarily a Claude/Claude Code wordplay branch, not an OpenClaw fork. Its scale proves that `claw` is not exclusively controlled semantically by OpenClaw. |
| **[CC Switch](https://github.com/farion1231/cc-switch)** | **FACT —** Desktop manager for Claude Code, Codex, OpenCode, OpenClaw, Grok Build and Hermes configurations ([repository, accessed 2026-08-25](https://github.com/farion1231/cc-switch)). | **FACT —** `farion1231` and contributors ([repository, accessed 2026-08-25](https://github.com/farion1231/cc-switch)). | **FACT —** Approximately 129.2k stars and 8.9k forks on 2026-08-25 ([repository](https://github.com/farion1231/cc-switch)). | **FACT —** No `claw` in the name; included as the requested control case showing that major cross-agent infrastructure can remain discoverable without entering the CLAW namespace. |
| **[ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)** | **FACT —** Rust personal-agent runtime distributed as a single binary, supporting many providers and messaging channels ([repository, accessed 2026-08-25](https://github.com/zeroclaw-labs/zeroclaw)). | **FACT —** ZeroClaw Labs/community project ([repository, accessed 2026-08-25](https://github.com/zeroclaw-labs/zeroclaw)). | **FACT —** Approximately 32.6k stars and 4.9k forks ([repository, 2026-08-25](https://github.com/zeroclaw-labs/zeroclaw)). | **ESTIMATE —** Direct OpenClaw-category naming: “smaller/lighter/different Claw.” |
| **[NanoClaw](https://github.com/nanocoai/nanoclaw)** | **FACT —** Lightweight OpenClaw alternative using containers and the Claude Agent SDK, with messaging integrations ([repository, accessed 2026-08-25](https://github.com/nanocoai/nanoclaw)). | **FACT —** NanoCo AI/community ([repository, accessed 2026-08-25](https://github.com/nanocoai/nanoclaw)). | **FACT —** Approximately 30.6k stars and 12.8k forks ([repository, 2026-08-25](https://github.com/nanocoai/nanoclaw)). | **FACT —** Its own account says “Nano” means minimal and “Claw” was intended generically ([NanoClaw blog, 2026-01-31](https://nanoclaws.io/blog/clawdbot-to-openclaw-trademark-drama)). **ESTIMATE —** Market perception nevertheless places it squarely in the OpenClaw wake. |
| **[PicoClaw](https://github.com/sipeed/picoclaw)** | **FACT —** Lightweight Go personal assistant intended for constrained hardware; the README says it is written independently rather than forked from OpenClaw ([repository, accessed 2026-08-25](https://github.com/sipeed/picoclaw)). | **FACT —** Sipeed/community ([repository, accessed 2026-08-25](https://github.com/sipeed/picoclaw)). | **FACT —** Approximately 29.9k stars and 4.4k forks ([repository, 2026-08-25](https://github.com/sipeed/picoclaw)). | **ESTIMATE —** Independent implementation but explicit OpenClaw-category/wake naming. |
| **[NemoClaw](https://github.com/NVIDIA/NemoClaw)** | **FACT —** NVIDIA reference stack for running OpenClaw and other agents inside the OpenShell security environment ([repository, accessed 2026-08-25](https://github.com/NVIDIA/NemoClaw)). | **FACT —** NVIDIA ([repository, accessed 2026-08-25](https://github.com/NVIDIA/NemoClaw)). | **FACT —** Approximately 22.3k stars and 3.0k forks ([repository, 2026-08-25](https://github.com/NVIDIA/NemoClaw)). | **FACT —** Direct OpenClaw integration and category naming. |
| **[IronClaw](https://github.com/nearai/ironclaw)** | **FACT —** Security-focused Rust agent OS; its README describes it as inspired by or reimplementing OpenClaw concepts ([repository, accessed 2026-08-25](https://github.com/nearai/ironclaw)). | **FACT —** NEAR AI/community ([repository, accessed 2026-08-25](https://github.com/nearai/ironclaw)). | **FACT —** Approximately 12.6k stars and 1.5k forks ([repository, 2026-08-25](https://github.com/nearai/ironclaw)). | **FACT —** Direct OpenClaw-category derivative. |
| **[ClawHub](https://github.com/openclaw/clawhub)** | **FACT —** Official registry for OpenClaw skills and plugins ([repository, accessed 2026-08-25](https://github.com/openclaw/clawhub)). | **FACT —** OpenClaw organization/Foundation ecosystem ([repository, accessed 2026-08-25](https://github.com/openclaw/clawhub)). | **FACT —** Approximately 9.3k stars and 1.4k forks ([repository, 2026-08-25](https://github.com/openclaw/clawhub)). | **FACT —** Official OpenClaw brand extension. |
| **[NullClaw](https://github.com/nullclaw/nullclaw)** | **FACT —** Small Zig autonomous-agent runtime focused on speed and low resource use ([repository, accessed 2026-08-25](https://github.com/nullclaw/nullclaw)). | **FACT —** `nullclaw` organization/community ([repository, accessed 2026-08-25](https://github.com/nullclaw/nullclaw)). | **FACT —** Approximately 8.0k stars and 944 forks ([repository, 2026-08-25](https://github.com/nullclaw/nullclaw)). | **UNKNOWN —** No authoritative etymology found. **ESTIMATE —** Strongly reads as another OpenClaw-category variant. |
| **[MimiClaw](https://github.com/memovai/mimiclaw)** | **FACT —** OpenClaw-style personal assistant implemented in C for inexpensive ESP32-S3 hardware; the project credits OpenClaw and Nanobot as inspirations ([repository, accessed 2026-08-25](https://github.com/memovai/mimiclaw)). | **FACT —** MemovAI/community ([repository, accessed 2026-08-25](https://github.com/memovai/mimiclaw)). | **FACT —** Approximately 5.7k stars and 841 forks ([repository, 2026-08-25](https://github.com/memovai/mimiclaw)). | **FACT —** Explicit OpenClaw-category derivative. |
| **[TinyAGI, formerly TinyClaw](https://github.com/TinyAGI/tinyagi)** | **FACT —** Multi-agent/team/channel orchestration around Claude and coding agents; the README identifies it as formerly TinyClaw and credits OpenClaw inspiration ([repository, accessed 2026-08-25](https://github.com/TinyAGI/tinyagi)). | **FACT —** TinyAGI/community ([repository, accessed 2026-08-25](https://github.com/TinyAGI/tinyagi)). | **FACT —** Approximately 3.6k stars and 508 forks ([repository, 2026-08-25](https://github.com/TinyAGI/tinyagi)). | **FACT —** Originally OpenClaw-category naming. **UNKNOWN —** No authoritative evidence was found that trademark pressure caused the TinyClaw→TinyAGI rename. |
| **ClaudeClaw variants** | **FACT —** The largest located variant, [`moazbuilds/claudeclaw`](https://github.com/moazbuilds/claudeclaw), runs an OpenClaw-like daemon, heartbeats, cron and chat bridges inside Claude Code. Other independent variants include [`aerolalit/claudeclaw`](https://github.com/aerolalit/claudeclaw), [`sbusso/claudeclaw`](https://github.com/sbusso/claudeclaw), [`earlyaidopters/claudeclaw`](https://github.com/earlyaidopters/claudeclaw) and [`leadsblender/claudeclaw`](https://github.com/leadsblender/claudeclaw) (all accessed 2026-08-25). | **FACT —** Several unrelated maintainers; no single canonical ClaudeClaw organization was found. | **FACT —** The `moazbuilds` repository had approximately 1.3k stars and 239 forks on 2026-08-25 ([repository](https://github.com/moazbuilds/claudeclaw)); the other variants were materially smaller. | **FACT —** Hybrid: Claude product name plus the OpenClaw persistent-agent pattern. |
| **[ClawAgents on PyPI](https://pypi.org/project/clawagents/)** | **FACT —** Compact Python agent framework combining OpenClaw and DeepAgents-style patterns, supporting OpenAI, Gemini, Claude and Bedrock ([PyPI, accessed 2026-08-25](https://pypi.org/project/clawagents/)). | **FACT —** PyPI maintainer shown as `xqjiang` ([PyPI, accessed 2026-08-25](https://pypi.org/project/clawagents/)). | **FACT —** Version 6.20.67 was released 2026-08-19 ([PyPI](https://pypi.org/project/clawagents/)). **UNKNOWN —** Reliable download/user traction was not established. | **FACT —** Its project description explicitly references OpenClaw patterns. |
| **[MicroClaw](https://github.com/microclaw/microclaw)** | **FACT —** Rust chat-agent runtime inspired by NanoClaw ([repository, accessed 2026-08-25](https://github.com/microclaw/microclaw)). | **FACT —** `microclaw` community. A separate [`MicrosoftHackathons/MicroClaw`](https://github.com/microsofthackathons/MicroClaw) wraps an installed OpenClaw in a Windows-oriented environment; the two are distinct projects (accessed 2026-08-25). | **FACT —** Main Rust project approximately 732 stars and 134 forks; MicrosoftHackathons project approximately 31 stars ([repositories, 2026-08-25](https://github.com/microclaw/microclaw)). | **FACT —** OpenClaw/NanoClaw-category derivative. |
| **[ZeptoClaw](https://github.com/qhkm/zeptoclaw)** | **FACT —** Local-first Rust assistant with OpenClaw migration support ([repository, accessed 2026-08-25](https://github.com/qhkm/zeptoclaw)). | **FACT —** Aisar Labs / `qhkm` ([repository, accessed 2026-08-25](https://github.com/qhkm/zeptoclaw)). | **FACT —** Approximately 649 stars and 97 forks ([repository, 2026-08-25](https://github.com/qhkm/zeptoclaw)). | **ESTIMATE —** OpenClaw-category prefix escalation: nano → pico → zepto. |
| **[ClawWork](https://github.com/clawwork-ai/clawwork)** | **FACT —** Desktop workspace/client for OpenClaw sessions and artifacts ([repository, accessed 2026-08-25](https://github.com/clawwork-ai/clawwork)). | **FACT —** ClawWork AI ([repository, accessed 2026-08-25](https://github.com/clawwork-ai/clawwork)). | **FACT —** Approximately 529 stars and 78 forks ([repository, 2026-08-25](https://github.com/clawwork-ai/clawwork)). | **FACT —** Direct OpenClaw product extension. |
| **[X-OmniClaw](https://github.com/OPPO-Mente-Lab/X-OmniClaw)** | **FACT —** Edge-native multimodal Android agent with vision/audio/device-control capabilities; its README aligns it with OpenClaw runtime capabilities ([repository, accessed 2026-08-25](https://github.com/OPPO-Mente-Lab/X-OmniClaw)). | **FACT —** OPPO Mente Lab ([repository, accessed 2026-08-25](https://github.com/OPPO-Mente-Lab/X-OmniClaw)). | **FACT —** Approximately 255 stars and 41 forks ([repository, 2026-08-25](https://github.com/OPPO-Mente-Lab/X-OmniClaw)). | **FACT —** OpenClaw-category inspiration with “Omni” denoting multimodality. |
| **[OmniClaw](https://github.com/omniaura/omniclaw)** | **FACT —** Multi-agent/group-chat coding orchestrator that began from a NanoClaw/Claude-in-WhatsApp direction and now supports several runtimes ([repository, accessed 2026-08-25](https://github.com/omniaura/omniclaw)). | **FACT —** OmniAura/community ([repository, accessed 2026-08-25](https://github.com/omniaura/omniclaw)). | **FACT —** Approximately 17 stars and 3 forks ([repository, 2026-08-25](https://github.com/omniaura/omniclaw)). | **FACT —** Indirect OpenClaw derivative via NanoClaw. |
| **[win4r/claude-code-clawdbot-skill](https://github.com/win4r/claude-code-clawdbot-skill)** | **FACT —** Skill/wrapper for running Claude Code through a PTY from the legacy Clawdbot environment ([repository, accessed 2026-08-25](https://github.com/win4r/claude-code-clawdbot-skill)). | **FACT —** `win4r` ([repository, accessed 2026-08-25](https://github.com/win4r/claude-code-clawdbot-skill)). | **FACT —** Approximately 123 stars and 36 forks ([repository, 2026-08-25](https://github.com/win4r/claude-code-clawdbot-skill)). | **FACT —** Direct historical OpenClaw/Clawdbot plus Claude Code hybrid. |
| **[JamesZhang0402/Claude-Code-x-OpenClaw-Guide-Zh](https://github.com/JamesZhang0402/Claude-Code-x-OpenClaw-Guide-Zh)** | **FACT —** Chinese-language tutorial corpus for using Claude Code with OpenClaw; the README credits KimYx0207 for the canonical content ([repository, accessed 2026-08-25](https://github.com/JamesZhang0402/Claude-Code-x-OpenClaw-Guide-Zh)). | **FACT —** Mirror/fork maintained under JamesZhang0402, with authorship attribution in its README ([repository, accessed 2026-08-25](https://github.com/JamesZhang0402/Claude-Code-x-OpenClaw-Guide-Zh)). | **FACT —** Approximately 131 stars and 113 forks ([repository, 2026-08-25](https://github.com/JamesZhang0402/Claude-Code-x-OpenClaw-Guide-Zh)). | **FACT —** Explicit dual-brand guide, not an independent CLAW product. |
| **[claw-army](https://github.com/claw-army)** | **FACT —** Small GitHub organization containing Claude-oriented utilities; [`claw-army/claude-node`](https://github.com/claw-army/claude-node) is a Python bridge to an installed Claude Code CLI using stream JSON (accessed 2026-08-25). | **FACT —** `claw-army` organization; no public member roster was visible ([organization, accessed 2026-08-25](https://github.com/claw-army)). | **FACT —** `claude-node` had approximately 6 stars and 2 forks ([repository, 2026-08-25](https://github.com/claw-army/claude-node)). | **UNKNOWN —** No authoritative etymology found. **ESTIMATE —** More likely Claude/claw slang than an OpenClaw satellite. |
| **[ClawFans](https://www.claws.fans/docs)** | **FACT —** Product/API where AI agents can register, post, subscribe and offer paid creator subscriptions through Stripe ([product documentation, accessed 2026-08-25](https://www.claws.fans/docs)). | **UNKNOWN —** A clearly identified legal steward was not established from the public documentation. | **UNKNOWN —** No reliable public user, revenue or repository traction was found. | **UNKNOWN —** The site does not give an authoritative name history. **ESTIMATE —** The agent-social-product context strongly suggests OpenClaw wake-riding. |
| **`claw-init` / [`Clawdapus`](https://github.com/mostlydev/clawdapus)** | **UNKNOWN —** No canonical notable repository or package whose actual product name is exactly `claw-init` was located. **FACT —** The nearest relevant match is Clawdapus, an infrastructure layer for several “Claws,” whose CLI exposes `claw init` ([repository, accessed 2026-08-25](https://github.com/mostlydev/clawdapus)). The independent npm package [`@opsyhq/claw`](https://www.npmjs.com/package/@opsyhq/claw) also exposes a `claw init` command. | **FACT —** Clawdapus is maintained under `mostlydev`; `@opsyhq/claw` is published by Opsy ([GitHub](https://github.com/mostlydev/clawdapus), [npm](https://www.npmjs.com/package/@opsyhq/claw), accessed 2026-08-25). | **FACT —** Clawdapus had approximately 3 stars and 3 forks ([repository, 2026-08-25](https://github.com/mostlydev/clawdapus)). **UNKNOWN —** npm usage was not established. | **FACT —** Clawdapus explicitly treats “Claws” as the OpenClaw/NanoClaw/PicoClaw category. **UNKNOWN —** `@opsyhq/claw` does not establish an OpenClaw derivation. |
| **[`@opsyhq/claw`](https://www.npmjs.com/package/@opsyhq/claw)** | **FACT —** MCP server/CLI giving agents SSH-based operations across machines and integrations with Claude Code, Cursor and Claude Desktop ([npm page, accessed 2026-08-25](https://www.npmjs.com/package/@opsyhq/claw)). | **FACT —** Opsy ([npm page, accessed 2026-08-25](https://www.npmjs.com/package/@opsyhq/claw)). | **UNKNOWN —** Reliable current downloads were not established. | **UNKNOWN —** No explicit OpenClaw origin was found. **ESTIMATE —** “Giving an agent claws” is an independent tool-metaphor usage. |
| **GoClaw variants** | **FACT —** [`sausheong/goclaw`](https://github.com/sausheong/goclaw) is a self-hosted Go gateway inspired by OpenClaw; [`smallnest/goclaw`](https://github.com/smallnest/goclaw) is a distinct same-name Go agent project (repositories accessed 2026-08-25). | **FACT —** Separate maintainers; there is no single canonical GoClaw. | **FACT —** Approximately 92 stars for `sausheong/goclaw` and 602 for `smallnest/goclaw` on 2026-08-25 ([repositories](https://github.com/smallnest/goclaw)). | **FACT —** At least one explicitly derives from OpenClaw; the shared spelling itself creates repository-level ambiguity. |

## What the census shows

**FACT —** The largest claw-named projects after OpenClaw include Claw Code at 195,112 stars, ZeroClaw at about 32.6k, NanoClaw at 30.6k, PicoClaw at 29.9k and NVIDIA NemoClaw at 22.3k ([repositories, accessed 2026-08-25](https://github.com/openclaw/openclaw), [Claw Code](https://github.com/ultraworkers/claw-code), [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw), [NanoClaw](https://github.com/nanocoai/nanoclaw), [PicoClaw](https://github.com/sipeed/picoclaw), [NemoClaw](https://github.com/NVIDIA/NemoClaw)).

**ESTIMATE —** This is no longer one dominant project surrounded by a few accidental collisions. It is a recognizable naming grammar: `[size/material/function prefix] + Claw`.

**ESTIMATE —** That grammar tells readers “an OpenClaw-like personal-agent runtime or supporting product” before they read the README.

**FACT —** A second, smaller semantic branch uses claw directly as a Claude pun—Claw Code, ClaudeClaw and the claw-army Claude utilities ([Claw Code](https://github.com/ultraworkers/claw-code), [ClaudeClaw](https://github.com/moazbuilds/claudeclaw), [claw-army](https://github.com/claw-army), accessed 2026-08-25).

---

# 2. Etymology and present-day semantics

## Origin

**FACT —** The chain is:

`Claude` → `Clawd` → lobster/molt imagery → `Moltbot` → `OpenClaw`.

Peter Steinberger documented that sequence on 2026-01-29 and explicitly called “Clawd” a playful Claude pun ([official announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)).

**FACT —** OpenClaw’s official explanation now assigns “Open” to open source/community governance and “Claw” to lobster heritage ([official announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)).

## Community usage

**FACT —** On 2026-02-21, Simon Willison reproduced Andrej Karpathy’s usage of plural “Claws” as a layer above LLM agents providing orchestration, scheduling, context, tool calls and persistence; Willison described “Claw” as an emerging term of art for OpenClaw-like systems running on personal hardware and reachable through messaging ([Willison, 2026-02-21](https://simonwillison.net/2026/Feb/21/claws/)).

**FACT —** The corresponding Hacker News discussion accumulated 412 points and 941 comments in the observed snapshot, and commenters used “claws” as a category rather than merely as the OpenClaw proper name ([HN thread originating 2026-02, accessed 2026-08-25](https://news.ycombinator.com/item?id <tg-id>)).

**FACT —** OpenClaw later formalized the word inside its own product: its CLI documentation defines a “Claw” as a versioned setup for a new OpenClaw agent, including identity, workspace, skills, plugins, MCP and scheduled tasks ([OpenClaw CLI documentation, accessed 2026-08-25](https://docs.openclaw.ai/cli/claws)).

**FACT —** A March 2026 Reddit user repeatedly used “my Claw” to mean their running OpenClaw assistant instance, rather than the codebase abstractly ([r/openclaw post, 2026-03-18](https://www.reddit.com/r/openclaw/comments/1rx31x2/3_weeks_of_claw_my_basic_assistant_set_up/)).

**FACT —** OpenClaw’s founder himself uses community phrases such as “Claw Crew,” “clawtributor” and “the claw is the law” ([official announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md); [Steinberger, 2026-02-14](https://steipete.me/posts/2026/openclaw)).

**UNKNOWN —** Publicly searchable Discord material is insufficient for a quantitative frequency analysis. The official history confirms that Moltbot was chosen in a Discord brainstorming session, but that does not constitute a representative Discord corpus ([official announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)).

## Semantic verdict

**ESTIMATE — Primary reading in AI-agent context:** OpenClaw ecosystem or the broader OpenClaw-like persistent-agent category.

**ESTIMATE — Secondary reading:** Claude/Claude Code wordplay.

**ESTIMATE — Tertiary reading:** Generic “agent with tools/power.” This persists in products such as `@opsyhq/claw`, but it is now the weakest agent-industry interpretation.

**ESTIMATE — Outside AI:** “Claw” remains an ordinary English word. The collision is therefore context-specific rather than universal.

---

# 3. Trademark and legal position

## What is registered or being pursued

**FACT —** Amantus Machina GmbH owns Austrian word mark **OpenClaw**, registration **333757**, filed 2026-01-29, registered 2026-02-03 and published 2026-02-20 for Nice classes 9 and 42, including AI software, virtual-assistant software, software engineering and AI-as-a-service; the Austrian record showed no opposition as of 2026-08-21 ([Austrian Patent Office](https://seeip.patentamt.at/markesuche/results/details/EDBC4595-7AAD-45AF-9E71-1DB83A44A3E0?type=OEPA)).

**FACT —** Amantus Machina GmbH also owns Austrian word mark **ClawHub**, registration **333756**, in the same relevant software/service classes ([Austrian Patent Office, status 2026-08-18](https://seeip.patentamt.at/markesuche/results/details/775DAF5E-A50F-44DF-8C99-347751BC9427?type=OEPA); [official trademark gazette, February 2026](https://www.patentamt.at/fileadmin/root_oepa/Dateien/Marken/MA_Publikationen/MA_Anzeiger2026_02.pdf)).

**FACT —** A UK application for **OPENCLAW/OpenClaw**, number **UK00004340581**, was filed on 2026-02-13 by Amantus Machina GmbH in classes 9 and 42, claiming priority from Austrian registration 333757 ([UK IPO journal, 2026-03-06](https://www.ipo.gov.uk/t-tmj/tm-journals/2026-010/UK00004340581.html)).

**FACT —** The public corporate trademark owner is therefore **Amantus Machina GmbH**, not OpenAI, in the cited Austrian and UK records ([Austrian Patent Office, 2026-08-21](https://seeip.patentamt.at/markesuche/results/details/EDBC4595-7AAD-45AF-9E71-1DB83A44A3E0?type=OEPA); [UK IPO, 2026-03-06](https://www.ipo.gov.uk/t-tmj/tm-journals/2026-010/UK00004340581.html)).

**FACT —** Separately, the operational project says it is governed by an independent non-profit foundation, and Peter Steinberger says OpenAI sponsors the project after he joined OpenAI ([Foundation, July 2026](https://www.openclaw.org/); [Steinberger, 2026-02-14](https://steipete.me/posts/2026/openclaw)).

**UNKNOWN —** The public sources reviewed do not establish the complete licensing or assignment relationship between Amantus Machina GmbH and the Foundation.

**UNKNOWN —** No worldwide conclusion about ownership of bare **CLAW** can be made from the Austrian and UK records alone. A proper clearance must cover exact marks, phonetic equivalents and common-law use in every intended jurisdiction.

## Enforcement history

**FACT —** The only documented naming demand in the project’s own history came from **Anthropic**, whose legal team asked that “Clawd” be reconsidered because it played on Claude ([OpenClaw announcement, 2026-01-29](https://github.com/openclaw/openclaw.ai/blob/main/src/content/blog/introducing-openclaw.md)).

**UNKNOWN —** No documented OpenClaw Foundation or Amantus demand against NanoClaw, PicoClaw, ZeroClaw, IronClaw, ClaudeClaw or another external claw-named project was located.

**UNKNOWN —** Absence of a public report is not proof that no private contact, coexistence agreement or cease-and-desist communication exists.

**FACT —** Within ClawHub, OpenClaw reserves the ability to transfer, rename, hide or quarantine namespaces in response to real-world brand or trademark disputes ([ClawHub namespace policy, accessed 2026-08-25](https://docs.openclaw.ai/clawhub/namespace-claims)).

## Risk assessment for your names

**ESTIMATE — Bare `CLAW`: medium legal uncertainty, high semantic collision.** An `OPENCLAW` registration is not automatically a monopoly over every compound containing “claw,” especially in a crowded naming field. However, using bare CLAW for adjacent AI/agent software creates a more credible likelihood-of-confusion argument than using it for an unrelated industry.

**ESTIMATE — `claw-consensus`: medium-low present legal risk, high perception risk.** A multi-machine consensus protocol differs from a personal assistant, but both occupy agent software, orchestration and infrastructure. Risk rises if branding, documentation or distribution suggests OpenClaw compatibility or affiliation.

**ESTIMATE — `claw1024`: lower confusion than bare CLAW, but not a legal clean room.** A distinctive compound improves source identification, yet it retains the contested semantic root and could still look like another `[prefix]+Claw` ecosystem project.

**UNKNOWN — Registrability.** Availability of `claw1024` as a mark, company name, package, domain and social handle has not been legally cleared merely because no prominent project appeared in this census.

**ESTIMATE — Required action.** Before commercial launch, commission an attorney-led knockout and full search covering UK, EU, US and every actual sales jurisdiction. This report is not legal advice.

---

# 4. Collision cost-benefit

## Discoverability

**FACT —** A small project competes against a 387,499-star OpenClaw repository, a 195,112-star Claw Code repository and multiple 20k–33k-star `[prefix]+Claw` alternatives ([OpenClaw API](https://api.github.com/repos/openclaw/openclaw), [Claw Code API](https://api.github.com/repos/ultraworkers/claw-code), [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw), [NanoClaw](https://github.com/nanocoai/nanoclaw), [PicoClaw](https://github.com/sipeed/picoclaw), accessed 2026-08-25).

**ESTIMATE — Bare-query outcome.** Searches or LLM questions such as “claw agents,” “claw protocol” or “CLAW AI” will usually allocate their first semantic prior to OpenClaw or the OpenClaw-like category. A new small lab is likely to drown on generic queries.

**ESTIMATE — Exact-query outcome.** A unique composite such as `claw1024`, reinforced by a matching domain, organization and social handles, can become retrievable through exact-name searches even though the word `claw` remains crowded.

**ESTIMATE — Wake-riding upside.** Borrowed discovery helps when users already searching for OpenClaw alternatives will find a relevant alternative. It does not help a consensus protocol whose value proposition is orthogonal; those clicks are poorly qualified.

## Perception among frontier-lab engineers and recruiters

**FACT —** NanoClaw, PicoClaw, ZeroClaw, IronClaw, NullClaw, NemoClaw and MimiClaw all occupy the personal-agent/runtime/security category and visually establish the `[modifier]+Claw` convention ([project repositories, accessed 2026-08-25](https://github.com/nanocoai/nanoclaw), [PicoClaw](https://github.com/sipeed/picoclaw), [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw), [IronClaw](https://github.com/nearai/ironclaw), [NullClaw](https://github.com/nullclaw/nullclaw), [NemoClaw](https://github.com/NVIDIA/NemoClaw), [MimiClaw](https://github.com/memovai/mimiclaw)).

**ESTIMATE —** A knowledgeable engineer encountering `claw-consensus` without context will be likely to infer one of three things: OpenClaw consensus infrastructure, an OpenClaw fork/satellite, or Claude-themed tooling.

**ESTIMATE —** The name does not automatically make the project look unserious; NVIDIA and NEAR AI have normalized the suffix through NemoClaw and IronClaw. It does, however, make independent conceptual authorship harder to communicate.

**ESTIMATE —** Recruiter impact is therefore mixed: `claw` may signal participation in a current agent trend, but it makes it harder to distinguish original protocol research from ecosystem packaging.

## Association turning toxic

**FACT —** OpenClaw’s own security documentation states that the assistant may execute shell commands, read and write files, use network services and send messages; it treats prompt manipulation and blast-radius containment as core threats ([OpenClaw security documentation, accessed 2026-08-25](https://github.com/openclaw/openclaw/blob/main/docs/gateway/security/index.md)).

**FACT —** OpenClaw published a high-severity advisory for CVE-2026-27001 concerning unsanitized working-directory text injected into model prompts; it was published 2026-02-18 and fixed in versions from 2026.2.15 ([official GitHub advisory, 2026-02-18](https://github.com/openclaw/openclaw/security/advisories/GHSA-2qj5-gwg2-xwc4)).

**FACT —** The project is simultaneously institutionalizing: the Foundation launched in July 2026, says it is independent and community-controlled, and announced a long-lived stable-release program later that month ([Foundation, accessed 2026-08-25](https://www.openclaw.org/)).

**ESTIMATE —** Association risk is medium and two-sided. OpenAI sponsorship and foundation governance may normalize the category; conversely, a major agent-caused security, privacy or autonomy incident could contaminate every “claw” brand in public discussion.

**ESTIMATE —** An unrelated consensus project receives little upside from OpenClaw’s adoption but remains exposed to that reputational spillover.

---

# 5. Wake-riding precedents

## 1. yt-dlp / youtube-dl / YouTube

**FACT —** `yt-dlp` explicitly identifies itself as a fork of `youtube-dl`, based on the intermediate `youtube-dlc`, and had about 186.7k GitHub stars in the 2026-08-25 snapshot ([official repository](https://github.com/yt-dlp/yt-dlp)).

**ESTIMATE — Outcome: positive because lineage was real.** The derivative name communicated compatibility, migration path and function. Users looking for the old project were exactly the intended audience.

**ESTIMATE — Lesson for CLAW:** wake-riding works when the product is genuinely a successor, fork or drop-in alternative.

## 2. VSCodium / VS Code

**FACT —** VSCodium distributes binaries built from Microsoft’s open-source VS Code repository without Microsoft branding, telemetry and licensing; its official repository had about 33k stars on 2026-08-25 ([VSCodium repository](https://github.com/VSCodium/vscodium)).

**ESTIMATE — Outcome: strong adoption but permanent downstream framing.** The close name makes the value proposition instantly legible, while almost guaranteeing that VSCodium is explained in relation to VS Code.

**ESTIMATE — Lesson for CLAW:** close naming can succeed commercially or socially, but independence is traded for permanent comparison.

## 3. OpenTF → OpenTofu

**FACT —** The OpenTF project publicly discussed a new name because trademark clearance around the Terraform-adjacent identity was difficult; the maintainers stated that legal review was part of the selection process ([OpenTF naming issue, 2023-09-06](https://github.com/opentofu/opentofu/issues/296)).

**FACT —** The Linux Foundation launched the resulting project as **OpenTofu**, describing it as an open-source alternative to Terraform and reporting support from more than 140 organizations and over 600 individuals at launch ([Linux Foundation, 2023-09-20](https://www.linuxfoundation.org/press/announcing-opentofu)).

**ESTIMATE — Outcome: beneficial separation.** The project retained category association through its positioning while choosing an ownable name instead of betting its identity on the upstream root.

**ESTIMATE — Lesson for CLAW:** a distinct name does not prevent you from explaining compatibility or ancestry.

## 4. React Query → TanStack Query

**FACT —** React Query was renamed TanStack Query as the library expanded to Vue, Svelte and Solid rather than remaining React-specific ([official TanStack announcement, 2022-07-14](https://tanstack.com/blog/announcing-tanstack-query-v4)).

**ESTIMATE — Outcome: the original name helped initial comprehension but became a scope ceiling.** The new umbrella brand better supported a multi-framework future.

**ESTIMATE — Lesson for CLAW:** a borrowed category name may be useful early, then expensive when the project’s ambition broadens.

## 5. Minetest → Luanti

**FACT —** Minetest’s maintainers said the name caused people to perceive the project as a Minecraft clone or “test,” even though it had evolved into a general game platform; they renamed it Luanti in October 2024 ([official Luanti announcement, 2024-10-13](https://blog.luanti.org/2024/10/13/Introducing-Our-New-Name/)).

**ESTIMATE — Outcome: delayed but necessary differentiation.** The giant’s namespace made the small project easy to understand but froze an outdated, subordinate perception.

**ESTIMATE — Lesson for CLAW:** if independent-platform status is the goal, a comparison-dependent name usually becomes technical and reputational debt.

## Precedent synthesis

**ESTIMATE —** Wake-riding tends to help when at least one of these is true:

- **ESTIMATE —** The project is a fork, successor or compatible replacement.
- **ESTIMATE —** Users of the giant are precisely the intended users.
- **ESTIMATE —** The derivative relationship is an asset the project is willing to retain permanently.

**ESTIMATE —** It tends to hurt when:

- **ESTIMATE —** The product has a different category or theory of value.
- **ESTIMATE —** The project seeks recognition for original research.
- **ESTIMATE —** The borrowed name limits future scope.
- **ESTIMATE —** The giant’s legal or reputational behavior is outside the smaller project’s control.

---

# 6. Recommendation matrix

| Option | Discoverability | Perception | Legal/reputation exposure | Strategic assessment |
|---|---|---|---|---|
| **Keep bare CLAW** | **ESTIMATE — Poor for generic search.** OpenClaw and the `[prefix]+Claw` cluster dominate the prior. | **ESTIMATE — High probability of being read as an OpenClaw satellite, alternative or Claude pun.** | **ESTIMATE — Highest of the three options.** Bare use provides the least source distinction. | **ESTIMATE — Reject, unless the lab deliberately intends to become part of the OpenClaw category.** |
| **Keep but qualify: `claw1024`** | **ESTIMATE — Materially better for exact-name search, handles and citations.** | **ESTIMATE — Distinctive enough to develop its own identity, but still initially reads as another `[prefix]+Claw` brand.** | **ESTIMATE — Lower than bare CLAW but not eliminated; formal clearance remains necessary.** | **ESTIMATE — Best pragmatic fallback if existing CLAW equity must be preserved.** |
| **Leave the root** | **ESTIMATE — Initially requires building recognition from zero, but gives the strongest long-term SEO and LLM-answer ownership.** | **ESTIMATE — Lowest likelihood of fork/satellite assumptions and strongest signal of original research.** | **ESTIMATE — Lowest OpenClaw-specific exposure, although the new name still requires clearance.** | **ESTIMATE — Preferred long-term choice for a flagship independent lab or protocol.** |

# Final recommendation

**ESTIMATE — Preferred decision:** leave `CLAW` as the primary public root while the migration cost is still low. The fact pattern is unusually unfavorable: OpenClaw is enormous, “Claw” has become an agent-category term, several serious organizations already use `[modifier]+Claw`, and your consensus work is not naturally an OpenClaw personal-assistant derivative.

**ESTIMATE — Pragmatic fallback:** if existing repos and channels make a full departure too costly, consolidate everything under **one exact composite**, such as `claw1024`, and stop presenting bare `CLAW` as the source identifier.

**ESTIMATE — If `claw1024` is retained, use all four safeguards:**

- **ESTIMATE —** Make `Claw1024` the indivisible wordmark; do not alternate between `CLAW`, `Claw` and `Claw1024`.
- **ESTIMATE —** Use names such as `claw1024-consensus`, not `claw-consensus`.
- **ESTIMATE —** Put a short disclaimer in every repository and product page: “Independent project; not affiliated with OpenClaw, the OpenClaw Foundation, Anthropic or OpenAI.”
- **ESTIMATE —** Lead every description with the category—“multi-machine agent consensus protocol”—before mentioning the brand.

**UNKNOWN —** `claw1024` cannot yet be called legally clear or collision-free without formal trademark, package, domain and common-law searches.

**ESTIMATE — Bottom line:** bare CLAW now carries more borrowed meaning than borrowed distribution. For original protocol research, ownable distinctiveness is worth more than riding the OpenClaw wave.
