# DR26-08-16-MACANTON-01-0731 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-16-MACANTON-01-0731
rail: claudeai
door: browser-chrome-mcp
class: vendor-research (кнопка "Start research", отчёт отдан артефактом)

org: 7f07033b-5895-4090-9d7e-dc34dcc5ffb0
chat_url: https://приватный чат
started: 2026-08-18T23:2xZ
collected: 2026-08-19T00:20Z

provenance: снято download-трюком со страницы claude.ai (text + tool_use.input.content), 0 токенов на чтение
---

Your DELTA investigation report on dual-agent Claude Code + Codex CLI operation (June–August 2026) is ready. It resolves the open contradictions in your DELTA-CONTEXT block: Codex's instruction loader was rewritten (`agents_md.rs`), `project_root_markers` is now honored (fixing the non-git vault issue), and `project_doc_max_bytes` is definitively a single cumulative 32 KiB budget where the deepest files get cut first. The report covers parity features (hooks, skills, /import, memories, multi_agent v2) with sharp edges flagged for each, the industry-wide convergence on git-worktree isolation (which doesn't help your Syncthing vault), the Gloaguen vs. Lulla debate on whether AGENTS.md files actually help, cross-vendor session continuity patterns, and Codex 0.147 changes (--approve-for-me, --full-auto removal, MCP 2026-07-28). It closes with a phased action plan recommending "serialize vault / parallelize code via worktrees + one-instruction-source + shared idempotent hooks," plus explicit rejections of full multi_agent parallelism and symlinked shared skill dirs.

---

# DELTA Investigation: Dual-Agent Operation of Claude Code + Codex CLI Over One Shared Vault & Undocumented Python Monorepo (June–August 2026)

## A. Executive Summary

The June–August 2026 evidence resolves most of the open contradictions in your DELTA-CONTEXT block, and the resolution is largely good news: the Codex CLI instruction-loader was rewritten (the file is now `codex-rs/core/src/agents_md.rs`, not `project_doc.rs`), and that rewrite fixes the two things that most threatened your NOT-under-git vault. First, `project_root_markers` is now honored for AGENTS.md discovery — the loader derives markers from merged config and only falls back to `.git` when unset — so you can point Codex at your vault root with a non-`.git` marker (issue #12128, filed Feb 2026 against v0.104, is addressed in the current loader). Second, silent truncation is no longer fully silent at the code level: the loader emits `tracing::warn!("project doc exceeds remaining budget; truncating")` — though this lands in logs, not the TUI, so issue #13386's UX complaint still stands for interactive users.

The truncation contradiction is definitively resolved from source: `project_doc_max_bytes` (default 32768 = 32 KiB) is a **single cumulative budget across all AGENTS.md files in one discovery pass**, decremented file-by-file as files load root→cwd. It is NOT per-file. The global `~/.codex/AGENTS.md` is loaded as host "user_instructions" and is concatenated first; project files are appended until the budget is exhausted, so **the deepest (most specific) files are cut first** when you overflow. This makes a bloated global file actively dangerous: every KB there is a KB your nested rules lose.

On parity features, the picture is: hooks, skills, /import, memories, and multi-agent v2 all shipped and are broadly stable by 0.147, but each has a sharp edge for a two-harness user. The single most important operational finding for concurrency: **no CLI vendor shipped a cross-vendor file lock** in this window; the entire industry (Claude Code `--worktree`, Copilot CLI `/worktree` Aug 3, Meta Muse Code Aug 5) converged instead on git-worktree isolation — which does not help your non-git vault at all. For the vault, your existing advisory lease board remains state-of-the-art; there is no native primitive to replace it.

## B. Key Findings (per open question)

### Q1 — Codex instruction-layer mechanics (0.144→0.147+) [established]

**Precedence and merge order (from source + official guide, developers.openai.com/codex/guides/agents-md, live Aug 2026):** Codex builds the instruction chain once per run (once per TUI session). Order:
1. **Global scope** — in `$CODEX_HOME` (default `~/.codex`): `AGENTS.override.md` if present, else `AGENTS.md`; only the first non-empty file at this level.
2. **Project scope** — from project root walking down to cwd; in each directory, `AGENTS.override.md` → `AGENTS.md` → fallback names in `project_doc_fallback_filenames`; at most one file per directory.
3. **Merge** — concatenated root→cwd joined with blank lines (source inserts a `\n\n--- project-doc ---\n\n` separator between user/global and the first project doc). "Files closer to your current directory override earlier guidance because they appear later in the combined prompt." This is **positional/soft override** (later text wins by convention/recency), NOT a hard replace. On direct conflict between global and a deep project file, the deep file wins because it appears last — but both remain in-context.

**`AGENTS.override.md` semantics:** At any single directory level where it exists, it replaces `AGENTS.md` **at that level only** (the loader's per-directory candidate list is `[AGENTS.override.md, AGENTS.md, ...fallbacks]` and takes the first match). It is NOT a whole-tree replace and does NOT suppress the global layer or parent-directory files. Third-party claims that it "replaces the whole file" are correct only in the narrow sense of "the AGENTS.md in the same directory." (Confirmed against `candidate_filenames()` in agents_md.rs.)

**`project_root_markers` — the fix that matters for your non-git vault [established]:** The current loader docstring reads: *"Determine the project root by walking upwards … until a configured `project_root_markers` entry is found. When `project_root_markers` is unset, the default marker list is used (`.git`). If no marker is found, only the current working directory is considered. An empty marker list disables parent traversal."* So issue #12128 (project_root_markers ignored, hard-coded `.git` walk, v0.104, Feb 2026) is resolved in the rewritten loader. Default is `[".git"]` (config-sample "Default:" label; config-advanced shows an illustrative `[".git",".hg",".sl"]` — flagged discrepancy, see §C). For your vault, set `project_root_markers = ["AGENTS.md"]` or a sentinel file so Codex finds a root without `.git`.

**`project_doc_fallback_filenames` — CLAUDE.md as fallback [established]:** Yes. Default `[]`. Add `project_doc_fallback_filenames = ["CLAUDE.md"]` and Codex reads CLAUDE.md wherever no AGENTS.md/AGENTS.override.md exists in that directory. This is per-directory fallback, not a global alias.

**Truncation — definitively per-cumulative-budget [established, from source]:** `load_project_instructions` sets `remaining = config.project_doc_max_bytes` once, then loops files doing `remaining = remaining.saturating_sub(entry.contents.len())`; inside `read_agents_md`, each file `if size > remaining { data.truncate(remaining) }` and warns. So: (a) the cap is **combined, not per-file**; (b) the **global file counts toward the budget** (it is the first entry); (c) the **last/deepest files are dropped first**. The config-advanced page's phrasing "how much to read from EACH AGENTS.md file" is misleading; source shows a shared budget. Issues #7138/#13386 (no TUI warning) — code now warns to the tracing log, so it is *not* truly silent, but interactive users still get no visible notice.

**`project_doc_max_bytes` placement [established, subagent-confirmed]:** It is a **root-level key only** (config-reference: "Maximum bytes read from AGENTS.md when building project instructions"; config-sample: `project_doc_max_bytes = 32768` at root, "# Default: 32768"). It is **not** documented as a key inside `[projects."/abs/path"]` tables — those accept **only `trust_level`** ("trusted"|"untrusted"): *"Mark a project or worktree as trusted or untrusted … Untrusted projects skip project-scoped .codex/ layers, including project-local config, hooks, and rules."* To scope byte budget per-repo, use a project-local `.codex/config.toml` (in a trusted project) or a profile — not the `[projects."..."]` table. The blog showing an `[agent]` table is wrong; `project_root_markers` and `project_doc_fallback_filenames` are likewise root-level, not per-project.

**approval_policy / on-failure [established]:** Current accepted values are `untrusted`, `on-request`, `never`, plus a granular object (categories: sandbox escalation, rules, MCP elicitations, permission requests, skill approvals). **`on-failure` is deprecated** — it still appears in `codex --help` for compatibility ("DEPRECATED: … Prefer `on-request` for interactive runs or `never` for non-interactive runs") but current docs mark it deprecated; do not use it in new saved config. Note the live bug #11885: `sandbox_mode=workspace-write` startup can silently coerce approval to on-failure via some startup paths — verify with `/status`. **`codex exec --full-auto` was removed in 0.147** (PR #36054): use `--sandbox workspace-write`. **`--approve-for-me` (0.147, PR #36373)** enables *automatically reviewed* approvals — not a blanket bypass; actions still pass an automated reviewer layer. **0.146.1 (Aug 5)** backported "safer cyber-model auto-review defaults" (require explicit trust for unfamiliar local projects); pre-trust a directory with `[projects."/abs/path"] trust_level = "trusted"` in `~/.codex/config.toml`.

**Recommended combo for a trusted personal monorepo run non-interactively:** `codex exec --sandbox workspace-write --ask-for-approval never` (equivalently `-s workspace-write -a never`), with `[projects."/abs/monorepo"] trust_level = "trusted"`. This keeps the workspace-write sandbox boundary (network off by default) while never blocking on a prompt — the documented pattern for headless exec.

### Q2 — Codex-native parity features [established/emerging]

**Hooks [established]:** Event list (turn scope): PreToolUse, PermissionRequest, PostToolUse, PreCompact, PostCompact, UserPromptSubmit, SubagentStop, Stop; (thread/subagent-start scope): SessionStart, SubagentStart. Config via `hooks.json` OR inline `[hooks]` tables in `config.toml`, discovered next to active config layers. Four practical locations: `~/.codex/hooks.json`, `~/.codex/config.toml`, `<repo>/.codex/hooks.json`, `<repo>/.codex/config.toml`. Project-local hooks load only when the project `.codex/` layer is trusted. Only `type:"command"` runs. Non-managed hooks require hash-based trust review before running; changing a hook re-triggers trust. `--dangerously-bypass-hook-trust` exists for pre-vetted automation. Multiple matching hooks all run concurrently — **one hook cannot block another from starting**. Hooks are gated behind a feature flag in some builds (`[features].codex_hooks = true`); admins can force off via `requirements.toml [features].hooks = false`.

**Critical hook-parity gaps vs Claude Code (decides whether one script serves both):**
- **PreToolUse fires for the Bash/shell tool only** in Codex — apply_patch/Edit/Write/Read/web-fetch/MCP calls do NOT fire it. Claude Code PreToolUse matches all tools (Edit|Write|Read|Bash|WebFetch|mcp__*). A shared file-write guard therefore cannot rely on PreToolUse in Codex; route file edits through shell or use PostToolUse.
- **PreToolUse in Codex can only deny, not modify** (`updatedInput` is accepted but fails open / no effect; likewise `permissionDecision:"allow"/"ask"`, `suppressOutput`). Claude Code supports `updatedInput` and allow/ask/deny.
- Codex sets **no dedicated env vars** during hook execution (no `CLAUDE_PROJECT_DIR` equivalent) — all context arrives via stdin JSON; the `cwd` field is the project-dir equivalent. BUT for **plugin** hooks Codex sets `PLUGIN_ROOT`/`PLUGIN_DATA` AND `CLAUDE_PLUGIN_ROOT`/`CLAUDE_PLUGIN_DATA` for compatibility with existing Claude plugin hooks (official Hooks page, developers.openai.com/codex/hooks).
- Codex stdin JSON base fields: `session_id, transcript_path, cwd, hook_event_name, model`; turn-scoped events add `turn_id`. Claude Code base fields are similar minus `model` (only SessionStart may include `model`). Exit-code semantics match: exit 0 proceed, exit 2 block (on blockable events), exit 1 blocks nothing (footgun in both).
- Codex has **no ConfigChange event** (Claude Code has it, plus WorktreeCreate/Remove, InstructionsLoaded, Elicitation in its expanded 21-event set). Async/HTTP/prompt/agent hook types are Claude-only; Codex is command-only. Codex's Stop event requires JSON output (plain text is invalid).
- **Practical rule:** a single hook *script* can serve both harnesses if it (a) reads all context from stdin JSON (never relies on env vars), (b) uses exit 0/2 only, (c) is registered on Bash/shell events in Codex and the broader tool matchers in Claude Code, and (d) does file-write governance via PostToolUse or shell interception in Codex. Double-firing: because both harnesses watch the same tree, a PostToolUse formatter registered in both will run twice per edit — make hooks idempotent.

**Skills [established/emerging]:** SKILL.md (YAML frontmatter: name, description; may include scripts/, references/, assets/, agents/openai.yaml) is portable via the Agent Skills standard; discovery under `.agents/skills`, `~/.codex/skills`. **Symlink caveat resolved but nuanced:** 0.147 "skips symlinks when installing plugins" (PR #36967) as security hardening — this is the *plugin* install copy path. For **skills**, the long-standing bug (#17344, #8943) is that user skills under `~/.codex/skills` are skipped when `SKILL.md` (or the dir) is a symlink. PR #24770 discussion proposes proper symlink handling (dereference within a marketplace root) but the canonical "one shared skills dir via symlink serves Claude + Codex + Cursor" is fragile on Codex today. **Recommendation: use real `.agents/skills` directories or the /import migrator rather than betting on symlinked skill dirs.**

**/import migrator [established]:** TUI slash command (not a CLI subcommand); user-initiated (was auto-on-startup earlier). Migrates (v0.145 expansion, PRs #31672/#33411/#33426/#33444): settings.json→config.toml, skills, plugins, MCP servers, subagents→agents, hooks, slash-commands→skills, instructions (CLAUDE.md→AGENTS.md), project-scoped memories, and up to the most recent 50 session items updated within 30 days. 0.147 PR #35623 parses Claude and Cursor session records separately (dedup fix). **Documented fidelity gaps:** conditional hook groups, async/HTTP hooks don't transfer (Codex has no equivalent); `$ARGUMENTS/$1/@file`/shell interpolation in commands can't become static SKILL.md; source model names dropped; disabled MCP servers excluded; no remote-session import; web-only (claude.ai/Cursor web) chats excluded; ConfigChange automation has no target. Practitioners' summary: "Convert behavior, not files. Keep rollback."

**Memories [emerging]:** Two-phase pipeline in `~/.codex/memories/` (memory_summary.md injected whole at session start, truncated to a ~5,000-token read-path budget per mem0's source reading; MEMORY.md grepped for the rest; raw_memories.md, rollout_summaries/). Off by default (`[features] memories`; PR #31804 stabilized the flag in 0.145). Config defaults (config-reference, subagent-confirmed): `generate_memories=true`, `use_memories=true`, `max_rollout_age_days=30` (clamp 0–90), `max_rollouts_per_startup=16` (cap 128), `min_rollout_idle_hours=6` (clamp 1–48), `disable_on_external_context=false`, `max_unused_days=30`. **Memories are CODEX_HOME-global, not project-scoped** (no per-directory memory store; the only "project-scoped memories" reference is the /import surface, which imports others' — not a native per-project store). Idle-gate fragility: a rollout must sit idle ~6h before consolidation, so back-to-back sprints may never trigger it. Blocked in EEA/UK/CH at launch per mem0. **For your stack, keep mandatory rules in AGENTS.md; treat memories as ambient only.**

**Goals / per-thread budgets [emerging]:** `/goal`, configurable rollout token budgets that track usage across threads, remind on remaining budget, and abort turns when exhausted (0.142 delegation modes + token budgets). Issue #24016 (exec resume requires a prompt for active goals) — status not re-confirmed from a primary source in this pass [single-source/unverified].

**multi_agent (v2) [established, but shares one workdir]:** `features.multi_agent` is stable and on by default; tools `spawn_agent, send_input, resume_agent, wait_agent, close_agent` (0.138/0.139 renamed close→interrupt in some builds; payloads encrypted PR #26210). `agent_max_threads` default 6, `agent_max_depth` default 1. **The practitioner report is correct: in-session subagents share ONE working directory** — "In-session subagents share a workspace, which is fine for review but risky for parallel feature work." Issue #23095 is an open enhancement request to give `spawn_agent` a `cwd`/`workspace_path`; today there is **no worktree-isolation option for Codex CLI subagents** (the Desktop app does worktree isolation internally; the CLI subagents do not). For parallel writes you must create worktrees yourself and launch separate `codex exec` processes.

**codex mcp-server / mcp add [established]:** Codex runs as an MCP server (`codex mcp-server`, docs at learn.chatgpt.com/codex/mcp-server) and as an MCP client (`[mcp_servers.<name>]` tables; `codex mcp add`). 0.147 adds opt-in MCP 2026-07-28 protocol (paginated discovery, multi-round requests, non-blocking server startup; PRs #35724/#35725/#35590/#35742) and upgraded the MCP SDK to 3.0.0.

**Safe-to-enable-NOW verdict:** hooks (command-only, idempotent, stdin-driven) — YES, high value; /import — YES, one-time; skills via real dirs — YES; memories — OPTIONAL/low-priority (ambient only); multi_agent — only with your own worktrees for writes; goals — optional. **Conflict risk:** any PostToolUse hook registered in BOTH harnesses double-fires on the shared tree; SessionStart context-loaders double-load. Scope each harness to distinct events or make every hook idempotent and side-effect-guarded.

### Q3 — Concurrency delta since July 2026 [established]

The dominant June–Aug 2026 development is industry-wide convergence on **git-worktree isolation** as the answer to "two agents editing one tree." Claude Code documents a `--worktree` flag + dedicated docs page and fixed worktree-isolation security bugs in **v2.1.212 (Jul 16, 2026)**: worktree-isolated sessions could still run destructive git against the real checkout — closed; also fixed PreToolUse auto-allow hooks bypassing tool restrictions, and symlink-following during worktree creation; **v2.1.203** delivered worktree/background-session fixes and MCP roots for additional working dirs. GitHub **Copilot CLI shipped experimental `/worktree` in the Aug 3 release train** ("Create an isolated worktree and begin a separate conversation with the new experimental /worktree command"). Meta Muse Code (Aug 5) described parallel subagents. The framing across all three: the real hazard is **silent last-writer-wins overwrite** — two agents writing one file in one tree produce NO merge conflict; the later write silently erases the earlier — and worktrees convert that invisible data-loss risk into a visible merge step (Git shares everything except per-worktree HEAD/index).

**Two hard facts for YOUR stack:**
1. **No cross-vendor lock primitive shipped**, and **Codex CLI shipped no native file-lock/lease/cross-session coordination primitive June–Aug 2026** (subagent-confirmed negative across 0.137–0.147 changelogs and GitHub releases). Codex has an in-session "steer vs queue" input feature (Enter steers a message into the running turn, Tab queues it) — but that is single-session keystroke routing, not coordination, and its "stable/default" status is not tied to a specific version/PR in a primary source [flagged].
2. **Worktrees require git; your vault is not under git.** So for the Python monorepo you CAN adopt worktree-per-writer (Codex `exec` per worktree, Claude `--worktree`); for the Syncthing vault you CANNOT. Your existing advisory-lease board + TTL declarations + serialize-via-task-queue remains the correct and only mechanism there. **Recommendation: serialize vault writes through the SQLite task board (single-writer lease); parallelize code work via git worktrees.**

### Q4 — Bootstrapping an undocumented 400k+ LOC Python monorepo [emerging, with a strong caution]

The pivotal, well-replicated result is **Gloaguen et al., "Evaluating AGENTS.md," arXiv:2602.11988 (ETH Zurich SRI Lab, submitted Feb 12 2026)**. Verbatim abstract: *"we find that providing context files does not generally improve task success rates, while increasing inference cost by over 20% on average … This observation holds across different LLMs, coding agents, and for both LLM-generated and developer-committed context files."* The evaluation combined SWE-bench Lite with a novel benchmark, **AGENTbench — 138 real GitHub issues from 12 niche Python repos (438 tasks total)**. LLM-generated context files *reduced* resolve rates in 5/8 settings (avg −0.5% SWE-bench, −2% AGENTbench; p=0.87/0.37 — no significant effect); developer-written files gave +2.4% avg (p=0.21, not significant) but still +19% cost and +3.3 steps. Mechanism: agents follow instructions too literally — a tool named in the file was used 160× more often. The paper's own prescription: *"human-written context files should describe only minimal requirements."*

This does **not** mean "don't write AGENTS.md" — it means keep them minimal, human-written, task-relevant, and be very wary of auto-`/init` bloat. The countervailing result: **Lulla et al., arXiv:2601.20404 (2026)** found curated AGENTS.md improved *efficiency* on focused PRs — **median runtime dropped 28.64%, output tokens fell 16.58%, task completion rates stayed the same**. The reconciliation: "Gloaguen counted total inference cost (including reading the file); Lulla counted how efficiently the agent worked after reading it." **Probe-and-Refine (arXiv:2606.20512)** proposes tuning repo guidance by probing which instructions actually help and pruning the rest. Broader context-engineering literature agrees "less context > more context": the **JetBrains study "The Complexity Trap: Simple Observation Masking Is as Efficient as LLM Summarization" (Lindenbauer et al., NeurIPS 2025 DL4Code workshop, arXiv:2508.21433)** found simple observation masking is "52% cheaper than the Raw Agent baseline but also improves on the solve rate by 2.6%" on 500 SWE-bench Verified instances, while LLM summarization made agents run 13–15% longer; agents over-retrieve (ContextBench, arXiv:2602.05892); and long context degrades — Chroma's 2025 study of 18 frontier models found degradation "at every increment of context growth, with a 'lost-in-the-middle' effect causing 30%+ accuracy drops for information buried in the middle."

**Tooling for repo maps / per-dir AGENTS.md (2026):** repo-map tools that emit a compact map (one advertises "~250 tokens replaces 50,000+ tokens of exploration"; a compiler-grade TS/JS blast-radius map claims "100% precision on blast radius vs grep's 60%"); AGENTS.md generators (markoblogo/AGENTS.md_generator, github/awesome-copilot `create-agentsmd`, Nx `nx-ai-agents-config` generating one source-of-truth for Claude/Cursor/Copilot/Gemini/Codex). Monorepo project-graph exposure (Nx-style) gives agents architecture without reading files. **Measured outcomes for docstring-backfill or test-scaffolding campaigns specifically:** I found no rigorous 2026 benchmark isolating those two interventions [no source found] — treat as unproven and measure locally.

**Concrete guidance for your legacy monorepo (69% docstrings, 5% tests, no CI):**
- Write **minimal, human-authored** root AGENTS.md: exact build/test/lint commands, the 6 invariants, "never touch X" boundaries, and where things live — nothing aspirational. Keep it small so it never eats the nested budget.
- Add per-directory AGENTS.md ONLY where a subtree has genuinely different conventions (payments-style override), following the "closest wins" model — but remember the cumulative-budget cut, so keep global+root under ~2–3 KB.
- Use a repo-map/project-graph tool to give blast-radius context on demand rather than dumping architecture into AGENTS.md.
- Prioritize **test scaffolding on agent-touched code paths first** (not blanket coverage): the highest-value target is a fast, deterministic test the agent can run to self-verify — this is what actually converts "proposed" into "verified" cheaply. Docstring backfill is lower-priority; do it opportunistically on files the agents edit.
- Because there's no CI, lean on **hooks** (PostToolUse formatter/linter; Stop-event "run tests before finishing") as your deterministic gate — the role CI plays elsewhere.

### Q5 — Cross-vendor session/task continuity [established/emerging]

**Codex continuity primitives:** sessions are JSONL rollout files under `~/.codex/sessions/YYYY/MM/DD/*.jsonl(.zst)`; a SQLite `state_*.sqlite` indexes ThreadMetadata; `session_index.jsonl` maps IDs→names. `codex exec resume --last` / `resume <SESSION_ID>` continues non-interactively (accepts `--output-schema` since 0.132; `-i image`); `codex fork` branches. Exec sessions are **hidden from the picker unless `--include-non-interactive`**; picker defaults to cwd-only (`--all` widens). **0.147 changes:** PR #35621 "skip restored token-usage replay for exec resumes" (resumed exec runs don't re-count prior tokens against the budget — cleaner cost accounting on reuse); PR #35644 "preserve thread metadata when rollout files are missing" (a resume/list won't hard-fail if the JSONL is gone — metadata survives so the thread still lists/attaches). **You cannot set a custom session-id/rollout filename** (server-generated; discussion #3827) and there is **no headless fork** confirmed fixed (issue #11750 status not re-verified this pass [unverified]). **Rollout-file reuse across vendors is not possible** — the JSONL format is Codex-internal; Claude Code cannot read it and vice-versa.

**What leading dual-CLI setups actually do (the pattern to copy):** they do NOT try to share session state; they share a **plain-file/DB task board + handoff documents** that both harnesses read as context, and let each agent reconstruct state from the transcript + the board. This matches your existing SQLite task board with file leases. The cross-vendor "same tasks, either agent continues" pattern that works today:
- **One human-readable task board** (your SQLite board) as the single source of truth for what's claimed/in-progress/proposed/verified; each task row carries a lease (owner + TTL) so either agent avoids duplicate pickup.
- **Handoff docs** ("long agent sessions rot; handoff files keep the work inspectable") written at end-of-turn, referenced by both AGENTS.md and CLAUDE.md so both harnesses load them at SessionStart.
- **Instruction bridge:** Claude Code still does NOT read AGENTS.md natively (issue #6235, opened by DylanLIiii 2025-08-21, **5,270+ reactions**, still open as of mid-2026; **Anthropic "confirmed in May 2026 that AGENTS.md support is 'not planned for now'"**; related #31005 notes "3,020 upvotes, 224 comments, still open, zero team response"). Keep AGENTS.md as the real file and bridge via a one-line `@AGENTS.md` import in CLAUDE.md (works cross-OS) or `ln -s AGENTS.md CLAUDE.md` (Unix; needs admin on Windows). This gives both harnesses one instruction source.
- Prevent duplicate work with the lease + hetero-verification you already run: implementer proposes → different-vendor reviewer verifies.

## C. Contradictions & Debates

1. **Per-file vs combined budget** — RESOLVED from source: combined cumulative budget; deepest files cut first; global counts. The config-advanced "each file" wording is misleading; the guide's "combined size" wording is right.
2. **Silent truncation** — PARTIALLY RESOLVED: code now emits `tracing::warn!` (not truly silent in logs), but there is no TUI surface, so #13386's user-facing complaint stands.
3. **project_root_markers honored?** — RESOLVED: now honored in the rewritten `agents_md.rs`; #12128 (v0.104) addressed. (Related #23826 reported a `:project_roots` breakage at 0.132 — config-key churn is real; verify on your installed version.)
4. **project_root_markers default** — UNRESOLVED doc discrepancy: config-sample says `[".git"]` (explicit "Default:" label); config-advanced example shows `[".git",".hg",".sl"]`. Trust the source docstring / config-sample "Default:" → `[".git"]`.
5. **AGENTS.override.md = full replace or per-level?** — RESOLVED: per-directory-level replace of that dir's AGENTS.md only; not whole-tree, not global-suppressing.
6. **Do context files help?** — GENUINE DEBATE: Gloaguen (arXiv:2602.11988 — no correctness gain, +20% cost) vs Lulla (arXiv:2601.20404 — 28.6% less runtime, 16.6% fewer tokens). Steelman for context files: both agree agents *follow* them; the disagreement is whether the exploration they induce is worth it, which is task-dependent, and Lulla measures post-read efficiency while Gloaguen measures total cost including the read. Consensus: keep them minimal and human-written.

## D. Best Practices (synthesized)

- Keep global `~/.codex/AGENTS.md` tiny (communication/style prefs only), because it eats the shared budget first. Put codebase rules in the repo root; put divergent rules in nested overrides.
- One instruction source: AGENTS.md real file + `@AGENTS.md` import in CLAUDE.md.
- For the vault (non-git): serialize writes via the SQLite lease board; never rely on a native lock (none exists).
- For the monorepo: worktree-per-writer for parallel work; `codex exec -s workspace-write -a never` in each; Claude `--worktree`.
- Hooks: command-type, stdin-JSON-only, idempotent, exit 0/2; register file-governance on shell events in Codex; expect double-fire and guard for it.
- Trust directories explicitly (`[projects."/abs"] trust_level="trusted"`) so project `.codex/` hooks/rules load.
- Treat memories as ambient; keep mandatory knowledge in AGENTS.md.

## E. Failure Modes (documented)

- **Silent last-writer-wins** on a shared tree/vault — the #1 concurrency hazard; no merge conflict is raised.
- **Instruction truncation** — a bloated global/root file silently drops nested rules (deepest-first); only a log warning.
- **Hook double-fire** — the same PostToolUse/SessionStart hook in both harnesses runs twice per event on the shared tree.
- **Codex PreToolUse blind spots** — fires on Bash only; file-edit/MCP tools bypass it; can only deny, not modify.
- **Symlinked skills skipped** on Codex (#17344/#8943); 0.147 also skips symlinks on plugin install (#36967).
- **Approval coercion** — workspace-write may silently use on-failure via some startup paths (#11885).
- **Subagents share one workdir** — parallel writes collide (#23095 open).
- **Cross-vendor session reuse impossible** — rollout JSONL is Codex-only.

## F. Strategic Recommendations for OUR stack

1. **Vault:** Keep the advisory lease board as the authoritative write-serializer; there is no native lock and worktrees don't apply to non-git Syncthing folders. Add a Codex `project_root_markers` sentinel (e.g., `["AGENTS.md"]`) so Codex resolves the vault root and loads your protected root AGENTS.md + nested files correctly.
2. **Monorepo:** Adopt worktree-per-writer. Run Codex as `codex exec -s workspace-write -a never` per worktree with `trust_level="trusted"`; run Claude with `--worktree`. Serialize merges through your board (proposed→hetero-review→verified).
3. **Instruction layers:** Shrink the global 54 KB `~/.codex/AGENTS.md` mirror — under the cumulative-budget rule it can silently starve your protected root + nested files. Split the canon: tiny global (prefs), lean root AGENTS.md (invariants + write protocol + commands), nested overrides only where conventions differ. Bridge Claude via `@AGENTS.md` import. Consider raising `project_doc_max_bytes` to 65536 if the canon legitimately needs it — but prefer trimming (Gloaguen: more context ≠ better).
4. **Gates without CI:** Implement one shared hook script (stdin-JSON, exit 0/2, idempotent) that runs your standalone gate scripts on PostToolUse (edits) and Stop (run tests before "done"); register on Bash events in Codex and broad matchers in Claude Code. This gives deterministic enforcement despite no CI.
5. **Bootstrapping:** Human-write minimal AGENTS.md; use a repo-map tool for on-demand architecture; scaffold fast deterministic tests on agent-touched paths first (this powers cheap self-verification); backfill docstrings opportunistically.
6. **Continuity:** Keep the SQLite board + handoff docs as the cross-vendor continuity layer; do not attempt rollout-file reuse. Use `codex exec resume --last` only within Codex's own chains.

**Decision — named options:**
- **Option A (recommended): "Serialize vault / parallelize code + one-instruction-source + shared idempotent hooks."** Trade-off: some vault throughput lost to single-writer leasing; benefit: eliminates the silent-overwrite class of bugs, which is unrecoverable, for the store you cannot worktree. Risk: relies on your existing lease discipline (mechanical, already in production). **Confidence: [established].** Main assumption: no native cross-vendor or Codex file lock arrives soon (confirmed absent through 0.147).
- **Option B: Full parallelism everywhere via multi_agent v2.** Rejected: subagents share one workdir (#23095), so parallel writes collide; only safe for read/review fan-out.
- **Option C: Bet on symlinked shared skills/instruction dirs.** Rejected as primary: Codex skips symlinked skills (#17344) and plugin symlinks (0.147); use real dirs + `@import` instead.

## G. Action Plan (phased, cheapest-first)

**Phase 0 (config, ~1 hour, zero risk):**
- Add to `~/.codex/config.toml`: `project_root_markers` sentinel for the vault; `project_doc_fallback_filenames = ["CLAUDE.md"]`; `[projects."/abs/monorepo"] trust_level="trusted"` and same for the vault.
- Verify instruction loading: `codex --cd <dir> -a never "List the instruction sources you loaded."` and check the tracing log for truncation warnings.

**Phase 1 (instruction diet, ~half day):**
- Trim global AGENTS.md; move canon to lean root files; add `@AGENTS.md` to CLAUDE.md. Re-run the load check to confirm nested files survive the budget.

**Phase 2 (deterministic gates, ~1 day):**
- Write the shared command hook; register PostToolUse + Stop in both harnesses; make it idempotent; test double-fire behavior on the shared tree.

**Phase 3 (concurrency, ~1–2 days):**
- Adopt worktree-per-writer for the monorepo; keep vault writes serialized on the board. Document "serialize vault / parallelize code" in AGENTS.md.

**Phase 4 (bootstrap, ongoing):**
- Add repo-map generation; scaffold tests on hot paths; opportunistic docstrings; measure token/runtime deltas locally (don't trust generic claims).

**Benchmarks that would change the plan:** if a future Codex release ships a native file-lease/lock or subagent `cwd` (watch #23095), revisit vault serialization and in-session parallel writes. If Claude Code adds native AGENTS.md (#6235), drop the CLAUDE.md bridge. If your local measurement shows nested AGENTS.md raising cost >20% without accuracy gain (per Gloaguen), cut them further.

## H. Sources

**Primary — source code & official docs (version-pinned):**
- openai/codex `codex-rs/core/src/agents_md.rs` (main) — cumulative budget, truncation warn, project_root_markers docstring, override/fallback candidate order. github.com/openai/codex/blob/main/codex-rs/core/src/agents_md.rs
- openai/codex `codex-rs/core/src/config/mod.rs` — PROJECT_DOC_MAX_BYTES = 32*1024, field defs. github.com/openai/codex/blob/main/codex-rs/core/src/config/mod.rs
- developers.openai.com/codex/guides/agents-md (Custom instructions with AGENTS.md, live Aug 2026)
- developers.openai.com/codex/config-reference, /config-advanced, /config-basic, /config-sample (project_doc_max_bytes, project_root_markers, trust_level, memories defaults)
- developers.openai.com/codex/hooks (event list, plugin env vars incl. CLAUDE_PLUGIN_ROOT); developers.openai.com/codex/plugins
- developers.openai.com/codex/changelog; github.com/openai/codex/releases rust-v0.144.0–0.147.0 (0.147.0 Aug 7 2026: --approve-for-me #36373, remove --full-auto #36054, skip symlinks on plugin install #36967, MCP 2026-07-28 #35724/#35725, exec resume token-replay #35621, thread metadata #35644, session parsing #35623); 0.146.1 Aug 5; 0.145.0 Jul 21
- code.claude.com/docs/en/hooks and /changelog (v2.1.203, v2.1.212 Jul 16 2026 worktree security fixes)

**Primary — GitHub issues/PRs/discussions:**
- openai/codex #7138, #13386 (silent truncation), #12128 & #12539 (project_root_markers), #23826 (:project_roots break at 0.132), #11885 (approval coercion), #17344 & #8943 (symlinked skills), #24770 (#36967 symlink policy), #23095 (subagent cwd), #24016 (goal resume), #11750 (headless fork), discussion #3827 (custom rollout filename)
- anthropics/claude-code #6235 (AGENTS.md support, opened 2025-08-21, 5,270+ reactions; Anthropic "not planned" May 2026), #31005

**Research papers:**
- Gloaguen et al., "Evaluating AGENTS.md," arXiv:2602.11988 (ETH Zurich SRI Lab, Feb 12 2026; AGENTbench 138 issues/12 repos; no success gain, +20% cost)
- Lulla et al., arXiv:2601.20404 (2026; curated AGENTS.md: −28.64% runtime, −16.58% output tokens)
- Probe-and-Refine, arXiv:2606.20512
- Lindenbauer et al., "The Complexity Trap: Simple Observation Masking…," arXiv:2508.21433 (NeurIPS 2025 DL4Code; masking −52% cost, +2.6% solve)
- ContextBench arXiv:2602.05892; Chroma 2025 long-context degradation study

**Secondary/practitioner (corroborating, lower authority):** codex.danielvaughan.com (Codex KB series on import/memories/multi-agent/worktrees), mem0.ai & exsesx.dev (memories internals), symposium.dev & agenticcontrolplane.com & knightli.com (Codex hooks), morphllm.com & code.claude.com (Claude hooks schema), digitalapplied.com & github.blog (worktree convergence, Copilot /worktree Aug 3), firecrawl.dev (Codex multi-agent), inventivehq.com/verdent.ai (resume), yurukusa/shawnmayzes/agyn.io (CLAUDE.md↔AGENTS.md bridge), iwoszapar.com (context-engineering meta-review). All practitioner claims cross-checked against a primary source where used for a load-bearing fact; single-source/low-authority items are tagged inline.