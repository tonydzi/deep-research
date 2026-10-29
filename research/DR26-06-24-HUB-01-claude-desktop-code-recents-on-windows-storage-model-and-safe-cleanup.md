---
dr_id: DR26-06-24-HUB-01
title: "Claude Desktop Code Recents on Windows: storage model and safe cleanup"
date: 2026-06-24
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-24-HUB-01): Claude Desktop Code Recents on Windows: storage model and safe cleanup

> Investigates where Claude Desktop's Code tab Recents list actually gets its data on Windows and what the safe, documented way is to clean up unwanted/junk sessions.

## Ключевые выводы
- Recents is a rendered catalog, not a direct listing of «внутренний путь лаборатории»*.jsonl transcripts — it's a hybrid model: embedded claude.ai Code surface (claude.ai/epitaxy) + local session metadata in %APPDATA%\Claude\claude-code-sessions\<account>\<org>\local_*.json + Chromium IndexedDB/Local Storage as a UI cache layer.
- Desktop log evidence showed the app 'loaded 57 persisted sessions from claude-code-sessions' while rendering Code through claude.ai/epitaxy, confirming the host/UI split.
- IndexedDB stores a React-Query-persisted conversation cache under key 'conversations_v2:<timestamp>'; deleting the IndexedDB origin causes rehydration from server, but sessions with valid .jsonl files can still fail to appear because the sidebar reads from IndexedDB-backed index, not the files.
- Cloud/remote Code sessions are server-side, account-backed, sourced of truth at claude.ai/code (can be archived and permanently deleted there); Remote sessions keep running in Anthropic's cloud even after closing the app.
- Supported controls are asymmetric: Desktop sidebar only offers filter/group/rename/archive/auto-archive-after-PR — no documented per-session permanent delete, no bulk delete, no hide-untitled/sub-session toggle; web (claude.ai/code) supports archive + permanent delete.
- Session sprawl in Recents comes from features that spawn independent top-level sessions: claude agents, --bg/background sessions, Dispatch-spawned Code sessions, scheduled tasks (fresh session per run), Remote Control — as opposed to in-session subagents/dynamic workflows which stay in the Tasks pane, not Recents.
- No public/documented 'headless/internal session' flag exists to suppress a session from Recents retroactively; only prevention settings exist (disableAgentView, disableWorkflows, disableRemoteControl, workflowKeywordTriggerEnabled, and env vars like CLAUDE_CODE_DISABLE_BACKGROUND_TASKS/WORKFLOWS/CRON, CLAUDE_CODE_FORK_SUBAGENT=0).
- Reverse-engineered but UNDOCUMENTED/unsupported API routes exist (GET /v1/sessions/{id}, POST /v1/sessions/{id}/archive, POST /v1/sessions/{id}/events, DELETE /v1/sessions/{id} in a separate managed-agents API context) — not recommended for consumer cleanup since Anthropic doesn't document them for that purpose.
- claude project purge (dry-run first) removes local project state under projects/, per-session task/debug/file-history data, and the «внутренний путь лаборатории» project entry — but is NOT a proven fix for stale Desktop Recents since the sidebar doesn't reliably rebuild from transcript files alone.
- Editing local_*.json manually often has no effect (app may overwrite fields like cliSessionId or mark sessions transcriptUnavailable on next launch) — confirmed by multiple bug reports.

## Рекомендации / решения
- Backup first (copy, not move) claude-code-sessions, IndexedDB, Local Storage, logs, «внутренний путь лаборатории», history.jsonl, file-history, backups before touching anything.
- Clean up in this safe order: (1) in-app filter/group/archive in Desktop sidebar + enable auto-archive after PR merge/close; (2) for cloud/web sessions, archive then permanently delete via claude.ai/code; (3) for background sessions use `claude rm <id>` (removes from list, keeps transcript on disk); (4) for local-only artifacts use `claude project purge --dry-run` then purge; (5) only as last resort, rename (don't delete) IndexedDB/Local Storage/Session Storage folders with Claude closed, to force a cache rebuild.
- Do NOT: use logout/login as a cleanup tactic, hand-edit local_*.json, or directly mutate LevelDB/IndexedDB files — all are fragile/unsupported and reported to worsen state.
- To prevent recurrence, disable the features that create independent sessions via settings.json (disableAgentView, disableWorkflows, disableRemoteControl, workflowKeywordTriggerEnabled: false) and/or environment variables (CLAUDE_CODE_DISABLE_BACKGROUND_TASKS, CLAUDE_CODE_DISABLE_WORKFLOWS, CLAUDE_CODE_DISABLE_CRON, CLAUDE_CODE_FORK_SUBAGENT=0) rather than trying to hide sessions after the fact.
- Determine first whether junk entries are local desktop sessions or server-backed cloud sessions before choosing a cleanup path, since the two require different (and differently risky) actions.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI (ChatGPT, Deep Research)
- **Продукты/инструменты:** Claude Desktop, Claude Code, claude.ai/code, claude.ai/epitaxy, IndexedDB, Local Storage, Session Storage, LevelDB, React Query, claude-code-sessions, claude project purge, claude rm, claude agents, Remote Control, Dispatch, PowerShell

## Открытые вопросы
- Which specific unwanted Recents entries on Anton's machine are local desktop sessions versus server-backed cloud sessions remains unresolved from public sources alone.
- No publicly documented, supported bulk-delete API or GraphQL mutation for consumer/account Claude Code sessions was found — only the web UI delete flow at claude.ai/code is officially safe for cloud sessions.
- Whether `claude project purge` or a cache rebuild (renaming IndexedDB/Local Storage) actually resolves the stale Desktop Recents problem is unproven — reports show it may not fix the UI sync issue.
- Version-dependent IndexedDB origin (https_claude.ai_* vs app_localhost_*) behavior across desktop build generations is not fully mapped.

## Источник
- DR-ID `DR26-06-24-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- claude-desktop-sessions-per-account
- crash-recovery-command
- system-architect
- config-safety-backup-and-migration-check
- deterministic-script-gotchas
