# DR26-06-23-HUB-01 — raw report from multipl

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Claude Code session management across multiple accounts and machines

## Bottom line

The highest-confidence conclusion is this: treat **local session transcripts** as the durable asset, treat any **Desktop “recent sessions” registry** as an index/cache, and do **not** rely on live multi-master syncing of Claude Code runtime state across machines. Anthropic officially documents that Claude Code stores local session transcripts in plaintext under «внутренний путь лаборатории», retains them for 30 days by default, and exposes SDK functions that rediscover sessions by scanning project directories. Anthropic also documents several other state layers under «внутренний путь лаборатории», including auth and settings, and explicitly treats local and cloud execution differently. That combination strongly points to a design where transcript files are the closest thing to a canonical local record, while UI/session registries are secondary metadata. citeturn11view1turn10view5turn11view2

For your use case, the best current pattern is a **layered model**:

* **Source-controlled, portable text config** in Git/chezmoi.
* **Machine-local secrets and auth** kept off sync.
* **Transcript backup/snapshot** from «внутренний путь лаборатории», ideally only after the session is quiescent or closed.
* **No direct live sync of the whole «внутренний путь лаборатории» tree**, and especially no attempt to make two machines co-own the same active session file in real time. That is where corruption/conflicts and account-bound weirdness begin. citeturn12search1turn12search17turn11view2turn13search3turn23search2turn14search0turn14search5

Anthropic has added partial continuity features over the last year, but **not** a general “all Claude Code local sessions/history/settings sync automatically across all devices and accounts” feature. What exists today is narrower: Remote Control mirrors a **running local** session to web/mobile; VS Code can download certain **cloud** sessions from claude.ai; Claude chat memory/search is becoming portable in the Claude app; and personal-to-Team/Enterprise migration is supported. None of that equals general cross-machine, cross-account portability for local Claude Code history. citeturn10view0turn10view4turn29view0turn30view0turn10view3turn22search0turn35view0turn35view1

## What Anthropic officially supports today

Anthropic’s official docs say Claude Code clients store **local plaintext session transcripts** under «внутренний путь лаборатории» to enable session resumption, with a default retention window of 30 days controlled by `cleanupPeriodDays`. Anthropic also documents that transcript writes can be disabled, and that `cleanupPeriodDays` is enforced at startup. If you care about long-term history, leaving the default in place without backup is a bad setup: old transcripts will eventually be swept. citeturn11view1turn11view0

The SDK further confirms that session discovery is based on filesystem-backed session data: `list_sessions()` enumerates past sessions either for a project directory or across all projects, and `get_session_messages()` reads messages from past session transcripts. In practice, that means local transcript files are not just a cache for display; they are real operational state that Claude Code itself can enumerate and read back. citeturn10view5

Anthropic also clearly separates **local** vs **cloud** Claude Code. Remote Control lets claude.ai/code or the mobile app attach to a Claude Code session that is still running on **your machine**; Anthropic’s docs explicitly say “nothing moves to the cloud,” the conversation stays in sync across connected devices, and the web/mobile surfaces are just a window into that local session. This is device continuity, not historical cloud sync of local session archives. citeturn10view0

Anthropic does support a limited form of local/cloud portability in VS Code: the Session History dialog has **Local** and **Remote** tabs, and cloud sessions from claude.ai can be resumed locally in VS Code. But that path is constrained: only web sessions started with a GitHub repository appear in the Remote tab, and once resumed, the history is **downloaded and continued locally** with changes **not synced back** to claude.ai. That is explicit one-way import, not bi-directional sync. citeturn10view4

On account portability, the official support boundary is also clear. Anthropic supports **personal account → Team/Enterprise organization** migration on supported plans, and if you keep both accounts you can switch between them from the account menu. But Anthropic also states that exported Claude data **cannot be imported into another personal Claude account**, and that they do **not** support migrating data between personal accounts. So for arbitrary account A → account B session portability, there is no official general-purpose import path today. citeturn10view3turn22search1turn10view2turn22search0

Anthropic’s broader app roadmap does show movement toward continuity features. In August 2025 Claude gained past-chat search; in September 2025 memory rolled out to Team/Enterprise and then broader plans; in March 2026 memory reached free users and memory import/export was highlighted in release notes; and in February–March 2026 Anthropic introduced cross-device control concepts like Remote Control and persistent Cowork threads. Those are real signs of a platform trend toward continuity, but they are mostly **Claude app / cloud memory / remote control** features, not full local Claude Code history sync across machines and accounts. citeturn30view0turn29view0turn10view0

## Why naive sync breaks

The failure mode is not mysterious. Syncthing is fundamentally a **file replication** system, not a transactional application-state manager. Its own docs say it is **not** an ideal backup application because changes, deletions, and mistakes propagate to all devices; it detects conflicts by creating `sync-conflict` copies when files differ after simultaneous modification; and it provides ignore files and file versioning as mitigation, not true multi-writer coordination. citeturn23search2turn13search3turn13search1turn23search1

That is already risky for live app state. It becomes worse for formats that assume local locking or multi-file consistency. SQLite’s official docs explicitly warn that corruption can occur when file locking is unreliable, especially on networked or nonstandard filesystems, and that WAL mode requires all processes to be on the same host and does not work over a network filesystem. SQLite also documents that corruption can result from malformed writes into the database or journal. The practical implication is simple: if a state format depends on “local host + correct locking + correct journal semantics,” file-sync tools are the wrong abstraction for active live state. citeturn14search0turn14search1turn14search2turn14search5turn14search20

Your Claude Code session transcripts are JSONL, not SQLite, so the exact corruption model is different. JSONL is more tolerant in one narrow sense: a closed file that is append-only text is easier to copy than a live SQLite database. But the multi-machine problem remains. If two devices resume or mutate the same logical session history concurrently, Syncthing will still see diverging file contents and treat that as a conflict. Even if corruption is not binary-level corruption, you get semantic corruption: split histories, stale recents, duplicate or missing session metadata, and “which branch of history is real?” problems. Syncthing’s own conflict model predicts that result. citeturn13search3turn23search9

Windows adds another failure mode: locked or in-use files. Syncthing community support discussions repeatedly note that on Windows, files in use may be inaccessible until the owning program closes them, and Syncthing has no VSS-like workaround built in. That does not prove every Claude transcript will hard-fail, but it does mean “live-write and sync anyway” is inherently racey on Windows. citeturn24search4turn24search7turn24search11

There is also direct Claude Code evidence that syncing or symlinking the whole «внутренний путь лаборатории» runtime tree into a consumer sync product can go sideways. In April 2026, a reproducible Windows bug report showed `.claude` symlinked into a OneDrive-synced folder causing `EEXIST` failures in `session-env/<UUID>` and breaking Bash/slash commands/agents. That is not proof that every sync setup fails, but it is strong evidence that **putting Claude’s volatile runtime area inside a live cloud-sync/symlinked home directory is brittle enough to trip real bugs**. citeturn35view2

Anthropic’s own issue tracker also shows the product gap directly. Users filed feature requests for cross-machine session synchronization, per-account settings sync, portable project memory across machines, and synchronized session history across multiple VS Code Remote clients. Those issues are not official docs, but they are current evidence that as of 2026 this remains an unmet need rather than a solved supported workflow. citeturn35view0turn35view1turn35view3turn35view4

## Best-practice architecture for backup and multi-machine use

The most robust pattern is to separate Claude state into **three classes** and handle each class differently.

First, keep **portable, deterministic text configuration** in source control. Anthropic’s own docs show machine-specific flexibility via environment-variable expansion in `.mcp.json`, and chezmoi’s docs explicitly support hostname-aware templates and templating for machine-to-machine differences. This is exactly the right layer for `CLAUDE.md`, reusable skills, prompts, MCP definitions, and any config that should be reviewed and merged like code. citeturn2search4turn12search1turn12search17

Second, keep **credentials and machine-bound runtime state local**. Anthropic’s docs say «внутренний путь лаборатории» stores authentication token, user settings, and session history, and Desktop/cloud sessions use OAuth. Anthropic also provides managed settings such as `forceLoginMethod` and `forceLoginOrgUUID` to constrain which account/org is valid in managed environments. That combination is a strong signal that auth and account-bound runtime context should stay per machine or per managed environment, not be indiscriminately shared. citeturn11view2turn10view1turn28view0

Third, treat **session transcripts** as backup/archive material, but not as a live multi-master sync target. Anthropic documents a `SessionEnd` hook that runs on session exit, `/clear`, and session switches. The cleanest architecture is therefore: let Claude write live JSONL locally, and only after the session is closed or switched, mirror the relevant transcript tree into a backup/sync staging area. That staging area can then be versioned and replicated safely. This is materially safer than making Syncthing watch the active writer directory directly. citeturn8search21turn11view1

For multi-machine continuity, the safest operational rule is **one writer per session**. If Machine A is currently hosting the live Claude Code process for Session X, do not resume Session X on Machine B until A has finished, flushed, and your backup/sync layer has propagated the closed transcript. Remote Control exists precisely to give you another surface onto the **same local session** without turning two machines into co-writers. Anthropic’s docs say the conversation stays in sync across connected devices because they are all attached to one running local session, not because Anthropic is merging two local histories in the background. citeturn10view0

For the backup channel itself, use **snapshot-style replication** rather than live co-edit replication whenever possible. Syncthing’s docs explicitly say it is not your backup system of record, but it does support ignore rules, receive-only folders, and file versioning. So if you continue using Syncthing, the least-bad setup is typically:

* a **send-only** or single-writer staging folder on the machine that closed the session,
* **receive-only** mirrors elsewhere,
* and **versioning enabled** on at least one destination. citeturn23search2turn13search1turn23search1turn23search19

A practical inference from the official SDK is that you should also maintain your **own portable session catalog**. Because `list_sessions()` can enumerate sessions across all projects, you can periodically scan local transcripts and export a neutral index with fields like `session_id`, `summary`, `cwd`, `created_at`, `last_modified`, `machine`, and `account/org label`. That catalog is portable across machines and accounts even if the Desktop-specific recents UI is not. The SDK supports this cleanly; the trade-off is that this becomes *your* source of truth for discovery, not Anthropic’s UI cache. citeturn10view5

## How to handle different paths and OS layouts

Absolute path drift is the real enemy of portable session continuity. Anthropic’s SDK and current local session model are directory-oriented, and users have explicitly reported that path-derived local memory/session storage becomes nonportable when usernames, mount points, or OS path formats differ. Anthropic has not yet shipped a path-agnostic local history abstraction for this case. citeturn35view3turn35view0turn35view4

The best current mitigation is boring but effective: **standardize project roots where you can, template the rest**. On same-OS machines, keep repos under the same canonical root when possible. For cross-OS fleets, do not hardcode user paths into portable config; use host-specific templates and environment variables instead. Chezmoi explicitly supports host-conditional templates, and Anthropic’s MCP docs support environment-variable expansion so one shared config can resolve differently per machine. citeturn12search1turn12search17turn2search4

Do **not** use symlinking of the whole Claude home as the primary path-normalization strategy. Mackup’s own architecture docs now recommend **copy mode** for backup/restore and describe symlink “link mode” as legacy and not recommended on modern macOS. Combined with the OneDrive-symlink Claude bug above, the direction is clear: symlinks are fine for carefully chosen stable config files, but they are a poor default for volatile Claude runtime state. citeturn17search0turn18search2turn35view2

The clean implementation framework is therefore:

* use Git/chezmoi for text config,
* keep machine-private auth/runtime local,
* and build a path-neutral **archive layer** on top of transcripts rather than trying to force path-neutral semantics into the live runtime tree. citeturn12search1turn11view2turn10view5

## Moving or merging history between accounts and orgs

Officially supported account/history portability is narrow. Anthropic supports keeping both a personal account and a Team/Enterprise account and switching between them from the account menu, or migrating the personal account into the organization. In that supported migration path, Anthropic says chats, projects, files, memory, connectors, and skills can move into the organization workspace. citeturn10view3turn22search1turn22search6

Outside that case, the official position is restrictive. Anthropic’s export article says data exports cannot be imported into another personal Claude account, and Anthropic does not support migrating data between personal accounts. If your scenario is “take local/desktop/session history from Account A and make Account B own it,” Anthropic does not currently document a supported general mechanism for that. citeturn10view2turn22search0

Filesystem-level transplant of an account-bound Desktop recents/history registry is therefore an **unsupported workaround**, not a supported migration path. Given your observed per-account registry layout, the safest assumption is that copying or merging those registry files across accounts/orgs may at best produce a cosmetic list, and at worst create stale pointers, ghost entries, inaccessible sessions, or account-bound Remote Control glitches. Anthropic’s own release notes show account identity matters enough that they had to fix Remote Control sessions not disconnecting when signing into a different account, and recent issue reports show client-side archive state and Desktop visibility can diverge from the underlying local CLI transcript state. citeturn36view0turn35view5

The sane alternative is to separate **session discovery** from **account ownership**. In other words:

* keep the original transcript archive untouched,
* index it yourself with the SDK,
* and if you need cross-account continuity, continue from the transcript material locally rather than trying to convince another Anthropic account that it “owns” the old recents list. That is not as seamless as native account migration, but it is much less likely to corrupt state or create misleading UI artifacts. This is an inference from Anthropic’s SDK/session model and current migration boundaries, but it is the lowest-risk option available today. citeturn10view5turn22search0turn10view3

## Recommended implementation framework

The strongest current implementation is a **two-tier system**.

At the **working tier**, each machine keeps its own live Claude runtime state local. That includes auth, machine-specific settings overrides, and active sessions. If you need the same active session elsewhere, use Remote Control to attach another surface to the current host instead of opening the same session from another machine. Anthropic explicitly designed Remote Control for that exact “same session, other device” use case. citeturn10view0turn10view1

At the **archive tier**, a hook or scheduled job mirrors only stabilized transcript/history material to a neutral backup location after session end. Anthropic documents SessionEnd hooks, and Syncthing provides versioning/receive-only semantics that fit archival replication better than live co-authoring. This is where you replicate transcripts between hub, laptop, and Macs. citeturn8search21turn23search1turn23search19

Concretely, the operating policy should be:

1. **Increase retention** beyond the 30-day default if you want local recovery windows longer than a month. `cleanupPeriodDays` is the first setting to change. citeturn11view0turn11view1  
2. **Keep portable config in Git/chezmoi**, with hostname/OS templating and env-var-based path indirection. citeturn12search1turn12search17turn2search4  
3. **Do not sync the whole «внутренний путь лаборатории» tree live.** In particular, do not make `.claude` a consumer-cloud-synced symlink root. citeturn11view2turn35view2  
4. **Mirror transcripts after close**, not while actively appending. A SessionEnd hook is the cleanest official mechanism. citeturn8search21  
5. **Use single-writer semantics per session.** One host owns the live session; other devices either wait or attach via Remote Control. citeturn10view0  
6. **Maintain your own neutral session catalog** from transcript scanning via the SDK so you are not dependent on any one Desktop recents registry. citeturn10view5  
7. **Do not attempt arbitrary account-to-account history merges** by editing registry files unless you accept unsupported behavior and possible UI/account inconsistencies. Official support exists for personal→Team/Enterprise migration, not general personal→personal history import. citeturn10view3turn22search0turn22search1turn36view0

If you want a single sentence version: **sync configs like code, sync transcripts like archived logs, and keep runtime/account state local.** That matches both Anthropic’s current support boundary and the way other tooling ecosystems handle multi-machine configuration safely. citeturn11view2turn12search1turn17search0

## Open questions and limitations

Anthropic’s public docs do **not** appear to document the Desktop app’s per-account recent-sessions registry format or any official import/export path for that registry. So the advice above treats that registry as a likely cache/index based on your observed layout plus Anthropic’s documented transcript/SDK model, but Anthropic has not publicly specified it. citeturn11view1turn10view5

Anthropic also has not publicly announced, in the sources reviewed here, a general feature for **full cloud sync of all local Claude Code sessions/settings across machines**. The existence of multiple recent feature requests for exactly that capability is evidence of demand, not evidence of planned delivery. citeturn35view0turn35view1turn35view4

Finally, user-reported GitHub issues and community support threads are useful for identifying failure patterns, but they are not the same thing as a vendor support statement or a guaranteed product limitation. I used them here only where they illuminate real-world risks that Anthropic or the sync tool vendors have not fully documented elsewhere. citeturn35view2turn35view5turn24search7