# DR26-08-16-MACANTON-01-0731 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-16-MACANTON-01-0731
rail: grok
door: cli-headless

collected: 2026-08-18T14:25Z

class: умная модель + веб-поиск (НЕ вендорский Deep Research)
stripped_agent_narration_bytes: 303
note: третья попытка рельсы; первые две (17.08) отдали нарратив агента вместо отчёта. Лечил не размер промпта, а OUTPUT CONTRACT в шапке ('пиши отчёт текстом ответа, файлов не создавай').
---

A. Executive Summary

The June–August 2026 delta is not a new dual-agent architecture. It is a set of Codex CLI mechanics that now collide with a stack you already run: a 54 KB receive-only `~/.codex/AGENTS.md`, a ~150-line vault-root `AGENTS.md`, a Syncthing-synced vault that is not a git repo, and `codex exec` as a second writer on the same tree as Claude Code.

Three facts dominate.

First, official Codex instruction discovery is now explicit and it is not “nearest file wins.” Codex concatenates global → project-root → cwd. `AGENTS.override.md` replaces only the file at that same directory. It does not erase parents and it does not touch the other harness. Default `project_doc_max_bytes` is 32,768. Official pages disagree on whether that cap is per file or combined. Either reading is bad for a 54 KB global mirror: the global file is truncated, or later (higher-precedence) project files are never added. `CLAUDE.md` is not a default fallback. Default `project_root_markers` is `[".git"]`. On a non-git vault, Codex does not walk to vault root; it only checks the current directory. https://developers.openai.com/codex/guides/agents-md (official, retrieved 2026-08-18)1(https://developers.openai.com/codex/agent-configuration/agents-md)

Second, Codex now has real parity surface: Agent Skills, hooks, plugins, `/import`, local memories, `/goal`, multi-agent, `codex mcp-server`, `codex mcp add`. The SKILL.md format is shared. The discovery paths are not. Claude still loads «внутренний путь лаборатории» and `.claude/skills`. Codex loads `.agents/skills` and `~/.agents/skills`. One directory does not serve both unless you symlink. Hooks are per-harness, not per-filesystem: they do not double-fire the same tool event, but they will double-mutate the same files if both agents format, lint, or write. `/import` is a one-shot migrator, not a live sync of your canon. https://developers.openai.com/codex/build-skills (official, retrieved 2026-08-18); https://code.claude.com/docs/en/memory (official, retrieved 2026-08-18)2(https://developers.openai.com/codex/build-skills)

Third, neither CLI shipped a cross-vendor lock for a shared non-git tree. Claude Code did ship native worktree isolation (`--worktree`, `isolation: worktree`, `WorktreeCreate`/`WorktreeRemove` hooks, sparse worktrees) through July–August 2026. That helps the Python monorepo if it is git. It does nothing for the Obsidian vault. On the vault, last-writer-wins and Syncthing `.sync-conflict-*` remain the failure mode. Serialize vault writes. Parallelize git code via worktrees. Keep the existing lease/task board as source of truth. https://code.claude.com/docs/en/worktrees (official, retrieved 2026-08-18); https://code.claude.com/docs/en/changelog (2.1.x, July–August 2026)3(https://code.claude.com/docs/en/changelog)

Recommendation (single): do not turn Codex into a second Claude. Keep Claude as orchestrator/reviewer. Run Codex as a non-interactive implementer with an explicit write-enabled exec profile, a much smaller global instruction file, a vault-root marker so nested `AGENTS.md` actually loads, no Codex hooks that mutate files Claude already gates, and the existing SQLite board plus a one-page handoff as the only shared session. Confidence: [established] on instruction/sandbox facts; [emerging] on the operational split. Main assumption: you will raise or split the 54 KB global file, because leaving it as-is wastes the Codex subscription you already measured at 1.1% utilization.

B. Key Findings

1. Codex CLI instruction-layer mechanics (0.144 → 0.147+)

1.1 Precedence and merge order — [established]

Official discovery, built once per run / TUI session: https://developers.openai.com/codex/guides/agents-md (retrieved 2026-08-18) and the mirror at https://learn.chatgpt.com/docs/agent-configuration/agents-md.1(https://developers.openai.com/codex/agent-configuration/agents-md)

1. Global scope (`$CODEX_HOME`, default `~/.codex`): read `AGENTS.override.md` if it exists, else `AGENTS.md`. Only the first non-empty file at this level is used.
2. Project scope: start at the project root (typically the git root) and walk down to cwd. If Codex cannot find a project root, it only checks the current directory.
3. In each directory: `AGENTS.override.md`, then `AGENTS.md`, then names in `project_doc_fallback_filenames`. At most one file per directory.
4. Merge: concatenate root-down, joined by blank lines. Files closer to cwd appear later and therefore override earlier guidance in the combined prompt.

This is not the agents.md.org “nearest file wins, others ignored” rule. That rule is the public AGENTS.md convention for many tools. Codex is a concatenating layered reader. https://agents.md/ (retrieved 2026-08-18) says nearest wins; Codex official docs say concatenate. Treat that as a real inter-tool disagreement, not a wording quibble.4(https://agents.md/)

Direct conflict between global and project: both are loaded. The project file does not delete the global file. The model sees global first, then project. On a true contradiction, later text usually wins because it is last in the prompt. That is prompt-order override, not a structured merge. There is no documented field-level conflict resolver.

1.2 AGENTS.override.md semantics — [established] official; [low-authority] third-party overclaim

Official: override is a same-directory replacement.

- At global: `~/.codex/AGENTS.override.md` replaces `~/.codex/AGENTS.md`. Remove the override to restore the base file. The project chain is unchanged.
- At a project directory: if `services/payments/AGENTS.override.md` exists, sibling `AGENTS.md` is ignored. Parent `AGENTS.md` files are still concatenated.
- Official example: cwd `services/payments` loads global, then repo-root `AGENTS.md`, then the payments override. https://developers.openai.com/codex/guides/agents-md (retrieved 2026-08-18)1(https://developers.openai.com/codex/agent-configuration/agents-md)

A May 2026 guide claims “AGENTS.override.md at any level replaces rather than extends parent instructions.” https://codersera.com/blog/agents-md-complete-guide-2026/ (2026-05-23) [low-authority]. That is not what the official page describes. Do not use override as a “forget the 54 KB global file” switch. It will not do that. To isolate a tree from global guidance you must either shrink global, use a separate `CODEX_HOME`, or accept that global always prepends.5(https://codersera.com/blog/agents-md-complete-guide-2026/)

1.3 project_root_markers — [established], and this is the vault landmine

Default is `[".git"]`. https://developers.openai.com/codex/config-sample (retrieved 2026-08-18): “Project root marker filenames … Default: [`.git`]”. https://developers.openai.com/codex/config-advanced (retrieved 2026-08-18): Codex walks up until it hits a marker. `project_root_markers = []` skips the walk and treats cwd as the root. You can add `.hg`, `.sl`, or any filename you choose.6(https://developers.openai.com/codex/config-advanced)

Your vault is Syncthing-synced and not under git. Unless you set a non-git marker at vault root, Codex started from `06-Concepts/foo/` will not see the protected 150-line root `AGENTS.md`. Official text: “If Codex cannot find a project root, it only checks the current directory.” https://developers.openai.com/codex/guides/agents-md (retrieved 2026-08-18) [established]

The Python engines/scripts tree is a different question. If that tree has `.git`, default markers work. If fleet scripts live in «внутренний путь лаборатории» which is also not the vault git root, treat it as a second project and give it its own marker or its own `.codex/config.toml`.

1.4 project_doc_fallback_filenames and CLAUDE.md — [established]

Default is `[]`. Codex does not read `CLAUDE.md` unless you list it. Official check order after you add fallbacks: `AGENTS.override.md`, `AGENTS.md`, then each fallback name. Filenames not on the list are ignored. https://developers.openai.com/codex/guides/agents-md (retrieved 2026-08-18)1(https://developers.openai.com/codex/agent-configuration/agents-md)

Independent write-up, 2026-06-09: “OpenAI Codex CLI reads AGENTS.md (and AGENTS.override.md or configured fallback names) — not CLAUDE.md.” https://inventivehq.com/blog/claude-md-vs-agents-md-vs-gemini-md [single-source, industry blog]. Claude’s official memory page is the reverse: Claude reads `CLAUDE.md`, not `AGENTS.md`; the supported bridge is a one-line `@AGENTS.md` import or a symlink. https://code.claude.com/docs/en/memory (retrieved 2026-08-18) [established]7(https://inventivehq.com/blog/claude-md-vs-agents-md-vs-gemini-md)

Do not add `CLAUDE.md` as a Codex fallback on this machine. Your Claude files are large, always-loaded, and not written for a 32 KB Codex budget. Point Claude at `AGENTS.md`. Keep Codex on `AGENTS.md` only.

1.5 project_doc_max_bytes: cap, truncation, and where the key lives — [established] that the key exists; [emerging] on exact cut order because official pages disagree

Facts that are documented:

- Default is 32,768 bytes. https://developers.openai.com/codex/config-sample (retrieved 2026-08-18)8(https://developers.openai.com/codex/config-sample)
- Official AGENTS.md page: “Codex skips empty files and stops adding files once the combined size reaches the limit defined by `project_doc_max_bytes` (32 KiB by default).” https://developers.openai.com/codex/guides/agents-md (retrieved 2026-08-18)1(https://developers.openai.com/codex/agent-configuration/agents-md)
- Official advanced config page, same product: “`project_doc_max_bytes`: how much to read from each AGENTS.md file.” https://developers.openai.com/codex/config-advanced (retrieved 2026-08-18)9(https://learn.chatgpt.com/docs/config-file/config-advanced)
- Troubleshooting: “Instructions truncated: Raise `project_doc_max_bytes` or split large files across nested directories.” https://developers.openai.com/codex/guides/agents-md (retrieved 2026-08-18)

Where the key is read:

- It is a root-level `config.toml` key, not a magical `[projects."..."]` only-key in current docs. User-level `~/.codex/config.toml` holds it. Trusted project `.codex/config.toml` files can override most keys; Codex walks root → cwd and closest wins. https://developers.openai.com/codex/config-reference and https://developers.openai.com/codex/config-advanced (retrieved 2026-08-18) [established]
- I did not find a current official page that still documents `project_doc_max_bytes` exclusively inside `[projects."/abs/path"]`. If an older local config still uses that table, treat it as stale until you verify against `codex --ask-for-approval never "List the instruction sources you loaded."` [unverified on this machine; no live CLI probe in this investigation]

Truncation implication if the combined-size reading is correct and files are added root-down: global is added first; when the budget fills, later (cwd-closer, higher-precedence) files are the ones not added. That inverts the override model. If the per-file reading is correct, a 54 KB global file is cut to 32 KB and project files still load, each up to 32 KB. You cannot tell which world you are in from docs alone. Measure it.

Your 54 KB global mirror is already over the default. That is the highest-leverage unused fact in this delta.

1.6 approval_policy, on-failure, and exec combinations — [established]

Current official values: `untrusted | on-request | never | { granular = { sandbox_approval, rules, mcp_elicitations, request_permissions, skill_approval } }`. Official note: “`on-failure` is deprecated; use `on-request` for interactive runs or `never` for non-interactive runs.” https://developers.openai.com/codex/config-reference (retrieved 2026-08-18)10(https://developers.openai.com/codex/config-reference)

Historical meaning of on-failure, still useful so you do not revive it: run sandboxed; if a command fails, ask to retry outside the sandbox. GitHub issue #3129 (2025-09-03) is the old CLI-switch bug around that mode. Do not put `approval_policy = "on-failure"` in a 0.147 config. https://github.com/openai/codex/issues/3129

Sandbox modes remain `read-only | workspace-write | danger-full-access`. `codex exec` still defaults to a read-only sandbox. You already paid for the silent no-write / exit 0 failure. 0.147.0 (2026-08-07) removed the deprecated `codex exec --full-auto` flag; the replacement is `--sandbox workspace-write`. https://learn.chatgpt.com/docs/changelog (2026-08-07, CLI 0.147.0)11(https://learn.chatgpt.com/docs/changelog)

0.147 also added `--approve-for-me`: automatically reviewed approvals, not a wider filesystem grant. Use it for interactive TUI, not as a substitute for `never` in unattended exec.

Recommended combinations for this trusted personal monorepo, non-interactive:

- Implementer `codex exec` on a git worktree of the Python tree, cwd = that worktree: `approval_policy = "never"` + `sandbox_mode = "workspace-write"`. Add `[sandbox_workspace_write] network_access = true` only if tests need network.
- Implementer that must write the vault, «внутренний путь лаборатории», or other roots outside cwd: `approval_policy = "never"` + `sandbox_mode = "danger-full-access"`, or `workspace-write` plus an explicit `writable_roots` list. Official sandbox docs pair these keys; community still uses the same pair. https://developers.openai.com/codex/config-reference (retrieved 2026-08-18); https://community.openai.com/t/how-to-turn-off-the-annoying-prompts-for-approval-in-codex-cli-vs-code/1358815 (2025-09-18, still the working pair) [low-authority for the blog echo, official for the keys]
- Interactive TUI on the same machine: `on-request` + `workspace-write`. Do not use `never` + `danger-full-access` in a TUI you leave unattended around network/MCP.
- Put the exec pair in a profile file (`~/.codex/exec.config.toml`) and call `codex exec --profile exec`. Profiles are overlay files as of 0.134.0, not `[profiles.*]` tables. https://developers.openai.com/codex/config-advanced (retrieved 2026-08-18)

Version pin for the window you named:

- 0.144.0: 2026-07-09. Compaction recovery on retired models; `/usage` reset-credit picker. https://www.reddit.com/r/CodexAutomation/comments/1urx4mi/ (2026-07-10) [low-authority]; corroborated by later official compare `rust-v0.144.0...rust-v0.145.0` on the changelog page.
- 0.144.6: 2026-07-18. Context-window correction for GPT-5.6 Sol/Terra/Luna to 272,000 tokens. [single-source aggregator: https://blakecrosley.com/guides/codex , page updated through 0.147]
- 0.145.0: 2026-07-21. Paginated thread history, expanded `/import`, multi-agent v2 controls, memories in history, Bedrock, audio/realtime V3. https://learn.chatgpt.com/docs/changelog
- 0.146.0: 2026-07-29. Session naming/pinning, plugin manifests. 0.146.1: 2026-08-05. Safer auto-review defaults for cyber-capable models.
- 0.147.0: 2026-08-07. Portable plugins, conversation sections, `--approve-for-me`, Cursor-skill import + de-duped Claude/Cursor chat sync, MCP 2026-07-28 opt-in, `--full-auto` removed.
- GitHub already shows `0.148.0-alpha.20` on 2026-08-18. Do not run alpha on the vault. https://github.com/openai/codex/releases (retrieved 2026-08-18)

2. Codex-native parity features vs Claude Code on one machine

2.1 Skills — [established] format; [emerging] path split

Format: a directory with `SKILL.md` (YAML `name` + `description` required) plus optional `scripts/`, `references/`, `assets/`. This is the Agent Skills standard. https://agentskills.io/specification (retrieved 2026-08-18); https://developers.openai.com/codex/build-skills (retrieved 2026-08-18)12(https://agentskills.io/specification)

Codex discovery (official):

- Repo: `.agents/skills` in every directory from cwd up to repo root
- User: `$HOME/.agents/skills`
- Admin: `/etc/codex/skills`
- System: bundled (`skill-creator`, `plan`, …)
- Symlinked skill folders are followed
- Same `name` in two places: not merged; both can appear
- Initial catalog is capped at 2% of context or 8,000 characters; descriptions get shortened first; some skills may be omitted with a warning
- Disable without delete: `skills.config path = ".../SKILL.md" enabled = false`

Claude discovery (official): «внутренний путь лаборатории» personal, `.claude/skills/` project. https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview (retrieved 2026-08-18)13(https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

Community complaint, 2026-01-14, still accurate in August: “The spec unified us. The paths divided us.” `.github/skills`, `.claude/skills`, `.codex/skills`, `.copilot/skills`. https://www.reddit.com/r/ClaudeCode/comments/1qcigma/ [low-authority, but it names the real interoperability gap]14(https://www.reddit.com/r/ClaudeCode/comments/1qcigma/agent_skills_the_spec_unified_us_the_paths/)

Can one directory serve both? Not natively. Practical pattern: keep the portable subset (no Claude-only hooks, no `$VARS` that only Claude expands, no `allowed-tools` that only Claude honors) under `~/.agents/skills` and/or `<repo>/.agents/skills`, then symlink into «внутренний путь лаборатории» / `.claude/skills` or the reverse. Do not symlink the whole Claude skill forest into Codex. Your Claude skills encode Anton-specific fleet, Telegram, and vault rituals. Codex will implicitly trigger them from `description` and then fail or wander.

Secondary docs sometimes mention `~/.codex/skills`. Official August 2026 build-skills page does not list that path. Prefer `~/.agents/skills`. [unverified that `~/.codex/skills` still works]

2.2 Plugins — [emerging], useful later

0.147.0 (2026-08-07): portable Agent Plugins; search local, personal, workspace, remote catalogs. Plugins bundle skills + connectors/MCP. Official: skills are the authoring format; plugins are the distribution unit. https://developers.openai.com/codex/plugins (retrieved 2026-08-18); changelog 2026-08-07. Do not install a marketplace plugin that writes the vault until you have read its hooks. Plugin hooks are not trusted on install; `/hooks` must trust them. Hardened isolation and deny-network-on-policy-update shipped in the same 0.147 release.

2.3 Hooks — [established]

Official event list (https://developers.openai.com/codex/hooks, retrieved 2026-08-18):

During a turn: `PreToolUse`, `PermissionRequest`, `PostToolUse`, `PreCompact`, `PostCompact`, `UserPromptSubmit`, `SubagentStop`, `Stop`

Session/subagent start: `SessionStart`, `SubagentStart`

Main thread end: `SessionEnd` (not for subagents)

Config locations: `~/.codex/hooks.json`, `~/.codex/config.toml`, `<repo>/.codex/hooks.json`, `<repo>/.codex/config.toml`. Installed plugins can add `hooks/hooks.json`. All matching sources run. Higher-precedence config does not replace lower-precedence hooks. Same layer with both `hooks.json` and `[hooks]` → merge + startup warning. Multiple command hooks for the same event launch concurrently. Default timeout 600s; `SessionEnd` default 1s, max 3s. Only `type: "command"` runs; `prompt` and `agent` handlers are parsed and skipped. Feature flag: `[features] hooks = true` is the default; `codex_hooks` is a deprecated alias. Project hooks load only if the project is trusted. Non-managed hooks require `/hooks` trust by hash. `--dangerously-bypass-hook-trust` exists for already-vetted automation. Plugin hook env includes `PLUGIN_ROOT` and, for compatibility, `CLAUDE_PLUGIN_ROOT`.15(https://learn.chatgpt.com/docs/hooks)

Claude Code hook surface is larger (on the order of thirty events, including `WorktreeCreate`, `WorktreeRemove`, `InstructionsLoaded`). https://code.claude.com/docs/en/hooks (retrieved 2026-08-18) [established]

Double-fire, precisely:

- Same Codex event registered in two Codex sources → fires twice. Vendor docs say so. https://docs.endorlabs.com/agent-governance/codex/index [low-authority vendor, matches official “all matching hooks run”]
- Claude hook + Codex hook on the same working tree → they do not see each other’s tool events. They can both format/lint/write the same file if both agents run, or both SessionStart hooks rewrite a shared status file.
- Safe now: Codex hooks that are read-only (log, redact, inject context) or that call the same standalone gate scripts you already run, made idempotent.
- Not safe now: Codex `PostToolUse` formatters, test runners, or “append to vault note” hooks that Claude already owns.

2.4 TUI `/import` from Claude Code — [established] inventory; [emerging] fidelity

Official import page (announced for desktop + CLI on 2026-08-11; CLI `/import` existed earlier and was expanded in 0.145.0 on 2026-07-21): https://developers.openai.com/codex/import (retrieved 2026-08-18)16(https://developers.openai.com/codex/import)

CLI can import from Claude Code or Cursor. Desktop also imports Claude Cowork.

What it maps:

- Instruction files → `AGENTS.md`
- `settings.json` → `config.toml`
- Skills → Skills
- Plugins → Plugins
- Existing project folders → Projects on the same folders
- Claude project memories → Codex Memories
- Chats from the last 30 days → chats (CLI cap: 50 chats)
- MCP server config → Codex MCP config
- Hooks → Codex hooks
- Slash commands → Skills
- Subagents → Codex agents

It does not change or delete existing Codex setup. `/import` is blocked during a running task, in a remote session, or while attached to a local app-server daemon. 0.147 added Cursor-managed skill import and sync of already-imported Claude/Cursor conversations without creating duplicates.

Official “review after import” list: tool restrictions in imported skills/agents; MCP auth/headers/env; hooks whose behavior may differ; plugins that need finish-setup; prompt templates that depend on arguments or path placeholders.

Known breakages / fidelity limits (official + implication):

- Slash commands become skills. That is a type change, not a copy.
- `settings.json` permissions do not equal `config.toml` sandbox/approval. Expect a translation, not a 1:1.
- Hooks “may differ after import.” Do not import Claude hooks onto this tree.
- Claude memories become Codex memories. Two stores. They will drift.
- 50 chats / 30 days is not your session archive.

Do not `/import` the 54 KB canon. You already rebuild that from the hub. Use `/import` once, if at all, as an inventory of Claude skills/MCP you might want on the Codex side, then delete anything that would double-fire.

Claude’s reverse door: `/import` in Claude Code (v2.1.213+) copies `AGENTS.md` into `CLAUDE.md` plus MCP, commands, subagents, skills. Official preferred daily bridge is still `@AGENTS.md` inside `CLAUDE.md`, not repeated imports. https://code.claude.com/docs/en/memory (retrieved 2026-08-18)

2.5 Memories (two-stage) — [established] as a local optional store; [emerging] as “safe to enable”

Official: local memories off by default; `[features] memories = true`. After enable, Codex turns eligible prior chats into files under `~/.codex/memories/` (summaries, durable entries, recent inputs, evidence). It skips active or short-lived sessions, redacts secrets, and waits until a chat has been idle. Two models: `memories.extract_model` (per-chat extraction) and `memories.consolidation_model` (global consolidation). That is the two-stage pipeline. Also: `generate_memories`, `use_memories`, `disable_on_external_context` (alias `no_memories_if_mcp_or_web_search`), `min_rate_limit_remaining_percent`. Official warning: keep required team guidance in `AGENTS.md`; memories are recall, not rules. https://developers.openai.com/codex/memories (retrieved 2026-08-18)17(https://developers.openai.com/codex/memories)

0.145.0 wired memories into paginated thread history. Computer History (2026-08-13) is a separate macOS desktop opt-in that feeds memories from apps/websites; Pro/Business/Enterprise; excluded in EEA/CH/UK at launch. Do not enable Computer History on a machine that sees the vault and Telegram.

Value now: low, unless you want Codex-only recall of Codex sessions. Conflict: your durable memory is the vault + `MEMORY.md`. A second unsupervised store will invent a second Anton.

2.6 Goals (thread-scoped, budget-aware) — [established] as a long-run loop; [emerging] on user-visible token budgets because sources disagree

Official use-case page: `/goal <objective>`, `/goal` to inspect, `/goal pause|resume|clear`. Enable with `[features] goals = true` or `codex features enable goals`. Intended for migrations, large refactors, eval loops. “When you use `/goal`, Codex can work independently for multiple hours.” https://developers.openai.com/codex/use-cases/follow-goals/ (retrieved 2026-08-18)18(https://developers.openai.com/codex/use-cases/follow-goals/)

Official cookbook, 2026-05-09: a Goal is thread-scoped state with lifecycle and budget accounting; stopping conditions include success, pause, clear, interruption, budget limit, or a blocker. https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex [established]19(https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex)

Implementation write-ups of PRs #18073/#18076/#18077 (May 2026): persisted `thread_goals` table; states `active|paused|budget_limited|done`; token budget exhaustion is a soft stop that injects wrap-up steering. https://kingy.ai/news/openai-codex-goal-the-new-long-horizon-mode-for-agentic-coding/ (2026-05-03) [low-authority but cites PR numbers]; https://pub.towardsai.net/i-walked-away-from-openais-new-codex-goal-for-18-hours-it-shipped-14-of-18-features-solo-a280f8407707 (2026-05-03) [low-authority]

Opposite pole: https://www.jdhodges.com/blog/codex-goal-feature-review/ (2026-05-08) — “Not a token-budget command… there is no `/goal budget` in the docs or in `codex --help`.” [single-source] And GitHub issue #24629 (2026-05-26): Codex created a self-imposed token budget the user never requested and stopped the goal. https://github.com/openai/codex/issues/24629

Steelman the less convenient side (Hodges / #24629): the budget exists in the runtime and can fire, but it is not a clean user-facing knob you can set per thread in `config.toml`. Official follow-goals page still does not document a `/goal budget` command. Treat budgets as an internal/soft stop, not as a planning tool. Do not start `/goal` on the vault overnight.

Feature was enabled by default in CLI 0.133.0 (2026-05-21) per unofficial release narrative. [single-source: blakecrosley guide]

2.7 multi_agent — [emerging], stabilize-not-enable on this tree

`[agents]` in `config.toml`: `enabled` default true, `max_concurrent_threads_per_session`, per-role `description`/`config_file`, default subagent model and reasoning. https://developers.openai.com/codex/config-reference (retrieved 2026-08-18). 0.137 shipped multi-agent v2; 0.145 called the multi-agent experience “more stable.” Subagents inherit parent `sandbox_mode`, MCP, skills unless overridden. https://developers.openai.com/codex/subagents (retrieved 2026-08-18)

On one working tree this is how you get silent last-writer-wins inside a single vendor. Leave `agents.enabled = false` for vault work. Allow it only inside a git worktree with non-overlapping paths.

2.8 `codex mcp-server` and `codex mcp add` — [established]

Server: `codex mcp-server` exposes two tools, `codex` (start: prompt, approval-policy, sandbox, model, cwd, base-instructions, config) and `codex-reply` (threadId + prompt). Official cookbook pattern is Claude-or-SDK-as-orchestrator, Codex-as-writer, with the server told `"approval-policy": "never"` and `"sandbox": "workspace-write"`. https://developers.openai.com/codex/mcp-server (retrieved 2026-08-18)20(https://developers.openai.com/codex/mcp-server)

Client: `codex mcp add <name> --env K=V -- <command>`. Also `codex mcp list`. Config in `~/.codex/config.toml` or trusted `.codex/config.toml`. https://developers.openai.com/codex/mcp (retrieved 2026-08-18)21(https://developers.openai.com/codex/mcp)

0.147: opt-in MCP 2026-07-28 (paginated discovery, multi-round requests, non-blocking startup). This is the cleanest official way to keep Claude as the only interactive driver and still burn the Codex subscription.

2.9 Enable-now vs do-not-enable

Enable now (valuable, low conflict):

- `codex exec --profile exec` with `never` + write-capable sandbox
- `project_doc_max_bytes` raised and measured
- `project_root_markers` that include a vault sentinel
- A small portable skill subset via `.agents/skills` + optional symlink
- `codex mcp-server` called by Claude for implementer turns
- `codex mcp add` only for servers Claude does not already own, or that Codex must see when running alone

Enable later / isolated:

- `/goal` on a git worktree with a test command as the stop condition
- plugins from a catalog you have read
- memories, after you decide they are Codex-private scratch, not a second bible
- multi_agent, only in worktrees

Do not enable on the shared tree:

- Codex hooks that format, test, or write vault notes
- `/import` of Claude hooks, Claude memories, or the 54 KB canon
- Computer History
- `agents.enabled` against the vault
- `CLAUDE.md` as a Codex fallback

3. Concurrency delta since July 2026

3.1 What is new — [established] Claude worktrees; [emerging] Codex session/worktree locks; [established] no cross-vendor lock

Claude Code, July–August 2026 (official changelog https://code.claude.com/docs/en/changelog):

- 2.1.206 (2026-07-09): `/doctor` proposes trimming checked-in `CLAUDE.md`; `EnterWorktree` confirmation outside `.claude/worktrees/`
- 2.1.207-era fix: `extensions.worktreeConfig` leftover broke go-git tools
- 2.1.222 (2026-08-04): worktree-isolated sessions can no longer run destructive git against the main checkout
- WorktreeCreate / WorktreeRemove hook events
- `--worktree`, `isolation: worktree` for subagents, `worktree.sparsePaths` for large monorepos
https://code.claude.com/docs/en/large-codebases (retrieved 2026-08-18); https://code.claude.com/docs/en/hooks22(https://code.claude.com/docs/en/large-codebases)

Codex: per-worktree session entries and lock files exist; GitHub issue #23515 (2026-05-19, still the cited failure): a Codex worktree session is interrupted when another Codex session is active in the base worktree. [single-source issue; not contradicted in later official docs I found] https://github.com/openai/codex/issues/23515

Neither vendor documents a file lease that the other vendor honors. Your SQLite leases and zone board are still the only cross-harness lock.

3.2 Newest failure evidence — [established] as a class; [emerging] as 2026 write-ups

The failure has not changed shape. Two writers, one inode, last write wins. Worktree guides in 2026 just document it more loudly:

- Two agents, one checkout: reads see partial edits, line numbers shift, reports cite ghosts. https://claudedirectory.org/blog/claude-code-worktrees-guide (2026-04-30) [low-authority]
- Shared lockfiles (`package-lock.json` and the Python equivalent) diverge across worktrees and produce unreviewable merges. https://www.termdock.com/en/blog/git-worktree-conflicts-ai-agents (2026-03-20) [low-authority]
- Reddit consensus (worktrees as the fix): “One agent reverts what another just wrote.” https://www.reddit.com/r/ClaudeAI/comments/1qzduim/ [low-authority]
- Codex #23515: even separate worktrees are not isolated if the base session holds a lock.

Syncthing-specific: there is no new 2026 native agent lock. Syncthing still resolves concurrent writes as last-writer-wins plus `.sync-conflict-<date>-<time>-<modifiedBy>` copies. On a vault that is not git, those conflict files are the entire audit trail. [no new official Codex/Claude source found for Syncthing; this is Syncthing’s long-standing behavior, older than the 12-month window]

3.3 When to serialize vs worktree — [established] practice

Serialize (task queue, one writer):

- Any vault path
- Any Syncthing-shared non-git tree
- Shared generated artifacts (indexes, lockfiles, `MEMORY.md`, instruction files)
- Two implementers on the same module
- Codex `exec` + Claude edit of the same file

Parallelize via git worktrees:

- Independent features in the Python monorepo, if it is a git repo
- Hetero-review: Claude reviews in the main tree or a read-only sandbox while Codex implements in a worktree
- Subagents of one vendor, using that vendor’s isolation

Do not invent a worktree analog for Obsidian. Copying the vault is how you get a second diverging second brain.

3.4 New native coordination — [emerging]

Useful new knobs, all single-vendor:

- Claude `isolation: worktree` + sparsePaths
- Claude WorktreeCreate hook (you could point it at your lease table)
- Codex `--approve-for-me` and conversation sections (coordination UX, not a lock)
- Codex paginated history / resume (session continuity, not a lock)

Nothing in 0.144–0.147 is a shared steering bus. Keep the dual-rail you already have.

4. Bootstrapping an undocumented 400k+ LOC personal Python tree

4.1 What 2026 actually uses — [established] techniques; [speculative] measured outcomes on a private 650k-LOC Python mix

There is no published, measured campaign on a 400k-engine + 250k-script undocumented personal Python monorepo with 69% docstring coverage and 5% tests. Anything that claims a percentage improvement on “your” shape is unverified.

What is used in production-shaped write-ups:

Repo maps

- Aider RepoMap: tree-sitter symbols + PageRank, sent every turn. This is the oldest measured design (2023–2024, still the reference). https://aider.chat/docs/repomap.html (original 2023-10-22, still current docs). It is not new in this window, and it still beats “dump the tree.” [established, older than 12 months; say so]
- Repo Atlas (skill for Claude and Codex): writes `docs/atlas/` with repo-map, entry points, test matrix. https://www.littlemight.com/repo-atlas/ and https://github.com/cathrynlavery/repo-atlas [low-authority, no independent outcome study]
- Claude official large-codebase guide: nested `CLAUDE.md` / rules, per-package skills, sparse worktrees, `/init`, `/doctor` trim. https://code.claude.com/docs/en/large-codebases (retrieved 2026-08-18) [established]

Per-directory AGENTS.md

- Official AGENTS.md site and Morph’s 2026-06-09 guide: OpenAI’s own Codex repo ships 88 nested `AGENTS.md` files; keep the root short (they say ~20–30 lines); subdirectory files carry local commands. https://agents.md/ ; https://www.morphllm.com/agents-md-guide (2026-06-09) [the 88-file claim is repeated often; treat as [emerging] industry lore unless you count the public repo yourself]
- Codex will concatenate these until `project_doc_max_bytes` fills. A campaign that writes a novel in every directory will truncate the ones you care about.

Docstring backfill

- No 2026 paper I found measured “backfill all docstrings, agent quality +X%.” Consensus practice is the opposite of a campaign: document the modules agents actually touch, in the file, with purpose / in / out / callers. Your own 69% coverage already exceeds most legacy trees. Spend the tokens on the 31% that sit on hot paths, and on the 95% without tests.
- Claude `/doctor` (2.1.206, 2026-07-09) now proposes cutting instruction files that restate what the code already shows. That is a hint: more prose is not more orientation.

Test scaffolding

- 2026 agent-ready checklists still say: tests are the only durable acceptance surface. https://domizajac.medium.com/is-your-repo-ready-for-the-ai-agents-revolution-926e548da528 [low-authority]
- For 5% coverage, do not generate a mirror test suite. Write characterization tests (input → observed output) on the exact functions an agent is about to change. Make the test the `/goal` stop condition or the Claude review checklist.
- No CI means the test is only real if the implementer profile is required to run it and the reviewer reruns it. That is process, not a tool.

llms.txt-style for code

- llms.txt is a website TOC standard (2024, still current): short intro + curated markdown links. https://llmstxt.org/ (v2; original 2024-09-03). In 2026 it is widely used for product docs (Anthropic, Stripe, Vercel examples: https://www.mintlify.com/blog/real-llms-txt-examples , 2026-02-07).
- For a codebase, the isomorphic artifact is a 1–2 page `llms.txt` / `REPO_MAP.md` at the root: what this tree is, how to test, where the engines live, “if you need X start at Y,” links to per-directory `AGENTS.md`. It is a router, not an encyclopedia. Agents follow links. That matches Codex’s byte cap.

What to run, cheap first:

1. A deterministic tree + entrypoint script you already trust (grep/ast, not an LLM) → `REPO_MAP.md` under the 32–64 KB budget
2. Nested `AGENTS.md` only in directories that have a different test command or a different invariant
3. Characterization tests on the next N agent-touched functions
4. Optional: Aider RepoMap or Repo Atlas as a generated sibling, gitignored or written to a cache dir, not