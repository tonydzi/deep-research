# DR26-07-10-MACB-01 — raw report from rakes

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: in-house Claude subagent (DR-C), session 48e95fc8, Mac -->
<!-- topic: DR26-07-10-MACB-01 — грабли коллег: как другие синкают AI-флоты и большие волты -->
<!-- date: 2026-07-10 · origin: claude-inhouse -->

# Top-10 Rakes Peers Hit Syncing AI-Agent Config + Big Vaults Across Machines — and How They Closed Them

*Research date: 2026-07-10. Scope: GitHub, HN, Syncthing/Obsidian forums, Reddit, blogs, arXiv, 2022–2026. Tags: **[widely-reported]** / **[single-anecdote]**.*

**BLUF:** Your pain is the textbook failure pattern. Peer consensus: **Syncthing is the wrong tool for (a) config/text you want history on, (b) coordination files open-and-writing, (c) live databases.** Peers split: **git for config, real DB/bus for live state, Syncthing only for bulk content with aggressive ignores.**

## The Top-10 Rakes

### 1. Syncing an actively-written database/coordination file → conflicts + corruption [widely-reported]
- Maintainer **Audrius Butkevicius**: "don't expect to sync databases… you will corrupt them and lose all your data." Root cause: "sqlite uses memory mapped files, which do not update mtime as they are written." — [forum, 2023-09](https://forum.syncthing.net/t/sync-conflict-with-1-specific-sqlite-file-constantly/20836)
- Founder **Jakob Borg**: "SQLite doesn't support someone else (Syncthing) just replacing the database file while it's open."
- [sqlite.org/howtocorrupt](https://www.sqlite.org/howtocorrupt.html): mid-transaction copy "might contain some old and new content, and thus be corrupt."
- **Fix:** never file-sync live DBs — client-server DB, single-writer node, replication (LiteSync), or one-small-file-per-key.

### 2. receive-only stuck "out of sync" / demands "Revert local changes" [widely-reported]
- [forum 20534](https://forum.syncthing.net/t/receive-only-folder-is-stuck-out-of-sync/20534); confirmed bug: "incorrectly detect 'Local Additions'… due to self-induced mtime changes", worse on "slow systems and/or very large folders" — [#10574](https://github.com/syncthing/syncthing/issues/10574)
- **Fix:** RO fragile at scale → one authoritative writer with send/receive, or git-pull for config.

### 3. `.obsidian/workspace.json` conflict storm [widely-reported]
- "I get a lot of '.obsidian/workspace-conflict' files… contributes to slow app load" — [Obsidian forum 44853](https://forum.obsidian.md/t/slow-app-load-times-and-workspace-sync-conflicts-when-using-syncthing/44853)
- **Fix (verbatim):** `.stignore`: `(?d).obsidian`, `(?d).obsidian.desktop`, `(?d).obsidian.mobile` — load time "from 8–12 s to less than 3" (same thread, 2024-08). Minimum: ignore `workspace.json` + `.obsidian/cache`.

### 4. Two devices edit before sync completes → duplicated/vanishing notes [widely-reported]
- Most common trigger: "open a vault on one device before the other has finished uploading" — [synch.run](https://synch.run/blog/obsidian-sync-conflicts/)
- **Fix:** wait for "Up to Date"; versioned backups **outside** the sync system; merge conflict files, never bulk-delete.

### 5. Multiple sync systems on one vault → dueling authorities [widely-reported]
- "Do not sync one vault with multiple sync systems at the same time" — [synch.run](https://synch.run/blog/obsidian-sync-conflicts/)
- **Directly us:** vault under Google Drive *and* Syncthing = named rake. One authority per path.

### 6. Syncthing over git repos / many tiny files → conflict factory [widely-reported]
- "I really wanted to like Syncthing, but it can't handle git repos inside it" — [HN 27153129](https://news.ycombinator.com/item?id <tg-id>); [forum 23160](https://forum.syncthing.net/t/mixing-git-and-syncthing/23160)
- **Fix:** git for text/version control; Syncthing for real-time files; don't overlap on the same paths.

### 7. mtime/permission mismatches across OSes → phantom conflicts [widely-reported]
- Conflicts with no simultaneous edits; "file permission and timestamp changes between operating systems" + a real v1.22 bug — [forum 19101](https://forum.syncthing.net/t/obsidian-conflicts/19101)
- **Fix:** "Ignore Permissions" on; stay current; heterogeneous fleets (our case) inherently more conflict-prone.

### 8. Committing «внутренний путь лаборатории» state/secrets to the config repo [widely-reported]
- Track configuration (settings, hooks, skills); ignore state (`projects/`, `history.jsonl`, `sessions/`, `transcripts/`, `shell-snapshots/`, `plugins/`, `*.key`, `credentials.json`) — [elizabethfuentes12/claude-code-dotfiles](https://github.com/elizabethfuentes12/claude-code-dotfiles)
- `.gitignore` insufficient for pasted secrets → gitleaks pre-commit: "it's caught me… more than once" — [Michael LaPlante, 2026-04-04](https://michaellaplante.com/blog/2026-04-04-version-controlling-your-claude-directory-across-machines/)

### 9. Losing agent project/context state on machine switch [single-anecdote, representative]
- "I opened my Windows machine… empty projects panel." Author rejected full-sync ("machine-specific credentials and active task states would be problematic"), used a scoped git repo — [Medium, 2026-03-28](https://medium.com/@creativeaininja/i-lost-a-full-day-of-claude-cowork-projects-overnight-a-git-repo-fixed-it-in-10-minutes-7742ee53046e)
- **Lesson:** durable artifacts (git) ≠ machine-local live state (don't sync).

### 10. Two machines push config simultaneously → manual merge [widely-reported]
- Git dotfiles "requires manual conflict resolution if two machines push simultaneously (`git pull --rebase`)" — [claude-code-dotfiles README](https://github.com/elizabethfuentes12/claude-code-dotfiles)
- **Structural fix — three-way merge with baseline:** `claude-sync` keeps `last-sync/` snapshot, compares local vs remote vs baseline: "never blindly overwrites. Conflicts resolved interactively" — [baptisterajaut/claude-sync](https://github.com/baptisterajaut/claude-sync). This is the fix for CLAUDE.md/MEMORY.md split-brain.

## How Others Sync Claude Code / Agent Config Across Machines

Community converged on **git for config, not Syncthing** — six independent 2026 projects/guides:

| Approach | Syncs | Rake handling | Source |
|---|---|---|---|
| Git dotfiles + shell wrapper | CLAUDE.md, settings, commands/, hooks/, agents/, skills/ | pull-on-open/push-on-close; allowlist .gitignore | [claude-code-dotfiles](https://github.com/elizabethfuentes12/claude-code-dotfiles); [Nick Ang](https://nickang.com/how-to-sync-claude-code-global-files-across-machines-if-you-work-on-multiple-computers/) |
| Git + gitleaks pre-commit | config only | secret scanner blocks commit | [LaPlante 2026](https://michaellaplante.com/blog/2026-04-04-version-controlling-your-claude-directory-across-machines/) |
| claude-sync (rsync/git, three-way) | CLAUDE.md, settings, skills/, agents/ | `last-sync/` baseline, interactive conflicts | [claude-sync](https://github.com/baptisterajaut/claude-sync) |
| chezmoi | templated dotfiles incl. «внутренний путь лаборатории» | per-machine templating | [frxiaobei guide](https://www.frxiaobei.com/en/posts/2026/04/chezmoi-claude-code/) |
| NAS + symlinks + Syncthing hybrid | settings via NAS; projects via Syncthing; sessions local | warns: NAS down → defaults; plaintext keys | [steeman.be 2026-05](https://www.steeman.be/posts/syncing-claude-code-across-multiple-machines/) |
| Sessions-only, E2E cloud (R2) | session history only | separates state from config | [tawanorg/claude-sync](https://github.com/tawanorg/claude-sync) |

**Universal rule:** *track configuration, ignore state.*

## Multi-Agent Shared State: What Small Fleets Do

- **No mainstream framework gives write-ownership over shared mutable state**: LangGraph = workflow graphs, AutoGen = message channels, CrewAI = agent-local memory; "conflicts are detected post-hoc… if at all" — [S-Bus, arXiv 2605.17076](https://arxiv.org/pdf/2605.17076) [widely-reported in literature]
- "Consistency errors account for 23–31% of LangGraph failures" — [DataCamp](https://www.datacamp.com/tutorial/crewai-vs-langgraph-vs-autogen)
- Working pattern: **typed state + reducers + DB-backed checkpointing** (LangGraph), event-driven core (AG2 v0.4), **shared read / scoped write** (Cognee) — [tryxlr8.ai 2026](https://tryxlr8.ai/blogs/best-open-source-ai-memory-frameworks-2026)

### OSS for cross-machine agent memory (2025–2026)
- **Mem0** (Cassandra v1.0.1 Nov-2025, Valkey v0.1.118 Sep-2025) — [mem0.ai](https://mem0.ai/blog/state-of-ai-agent-memory-2026); **Zep/Graphiti**, **Letta** (MemGPT), **EverMind EverOS**, **Cognee** — [evermind.ai](https://evermind.ai/blogs/best-open-source-agent-memory-frameworks-2026), [atlan.com](https://atlan.com/know/best-ai-agent-memory-frameworks-2026/)
- **Takeaway:** the field's answer = shared memory service with a real backend + scoped writes, **not file replication**.

## Synthesis for our fleet

1. **«внутренний путь лаборатории» config** → git, allowlist .gitignore, gitleaks, three-way merge (claude-sync baseline) — kills CLAUDE.md/skills split-brain (rakes #6/#8/#10).
2. **Message channel + consensus/TurnState** → NOT files on Syncthing (rake #1). Single-writer node / DB / bus. Telegram-03 rail = right instinct.
3. **180k vault** → ignore churn (`(?d).obsidian`, workspace.json), one authority per path (GDrive+Syncthing overlap = rake #5), Ignore Permissions (#7), backups outside sync (#4). The 959 conflicts are the predicted outcome of large-vault + receive-only + heterogeneous-OS (#2/#3/#7).

**Confidence:** the three-substrate split is **widely-reported** (Syncthing maintainers + six Claude-config projects + Obsidian forums + multi-agent literature). Note: two HN threads (Dotclaude #47278197, Claude-Config #46653896) were rate-limited (429) during fetch; substance corroborated by the repos above.
