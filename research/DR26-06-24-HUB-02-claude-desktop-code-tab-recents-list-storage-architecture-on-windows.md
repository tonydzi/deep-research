---
dr_id: DR26-06-24-HUB-02
title: "Claude Desktop Code tab Recents list storage/architecture on Windows"
date: 2026-06-24
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-24-HUB-02): Claude Desktop Code tab Recents list storage/architecture on Windows

> Investigates where Claude Desktop's Code tab 'Recents' session list actually comes from on Windows and how to safely clean up unwanted/stale entries.

## Ключевые выводы
- Recents is a hybrid, index-driven rendered catalog, not a direct listing of ~/.claude/projects/*.jsonl transcripts and not explained by live claude processes
- Desktop app embeds a claude.ai Code web surface (https://claude.ai/epitaxy in newer builds, older builds used claude.ai/claude-code-desktop/...) on top of locally persisted state
- Local session registry/metadata lives at %APPDATA%\Claude\claude-code-sessions\<account>\<org>\local_*.json; log evidence shows app loading 'persisted sessions' from this path
- Transcript content is stored separately as JSONL under ~/.claude/projects/<project>/<session-id>.jsonl (official Anthropic docs), but the UI does not reliably rebuild Recents from these files alone
- Chromium storage (IndexedDB key conversations_v2:<timestamp>, Local Storage) acts as a UI cache/renderer layer for claude.ai surfaces, with rehydration from server on deletion — not the sole durable source
- Cloud/remote/web Code sessions are server-side, source of truth is the account backend; only claude.ai/code offers documented archive AND permanent delete
- Desktop sidebar only documents filter/group/rename/archive/auto-archive-after-PR — no documented per-session permanent delete, no bulk delete, no hide-untitled/sub-session toggle, no internal/headless session flag
- Session sprawl comes from features that spawn independent top-level sessions: claude agents, --bg, /background, Dispatch, scheduled tasks, Remote Control — as opposed to subagents/dynamic workflows inside a session which stay in the Tasks pane
- Prevention is via settings.json/env flags (disableAgentView, disableWorkflows, disableRemoteControl, CLAUDE_CODE_DISABLE_BACKGROUND_TASKS/WORKFLOWS/CRON, CLAUDE_CODE_FORK_SUBAGENT=0) rather than any documented 'hide from Recents' flag
- No publicly documented bulk-delete API for consumer Claude Code sessions exists; reverse-engineered /v1/sessions/{id} archive/delete routes exist but are not documented as supported for Desktop Recents cleanup

## Рекомендации / решения
- Back up (copy, not move) claude-code-sessions, IndexedDB, Local Storage, logs, and ~/.claude/projects/history/backups before any cleanup attempt
- Do in-app non-destructive cleanup first: filter/group/archive sessions in Desktop sidebar, enable auto-archive after PR merge/close
- For cloud/web sessions, archive then permanently delete via claude.ai/code (irreversible, official flow)
- For background sessions from agent view/--bg/--background, use documented `claude rm <id>` (removes from list, keeps transcript on disk)
- For local-only session artifacts, use `claude project purge --dry-run` then `claude project purge` (or --all) rather than hand-editing files
- Only as a last resort, rename (not delete) %APPDATA%\Claude\IndexedDB / Local Storage / Session Storage with Claude fully closed to force a cache rebuild
- Avoid: logout/login as a cleanup tactic, hand-editing local_*.json to hide entries, directly mutating LevelDB/IndexedDB files, and using undocumented /v1/sessions delete/archive endpoints for account cleanup
- To prevent recurrence, disable session-spawning features via settings.json/env vars (disableAgentView, disableWorkflows, disableRemoteControl, CLAUDE_CODE_DISABLE_* flags) rather than looking for a hide-from-Recents toggle

## Сущности
- **Люди:** —
- **Компании:** Anthropic
- **Продукты/инструменты:** Claude Desktop, Claude Code, claude.ai/code, claude.ai/epitaxy, IndexedDB, Local Storage, LevelDB, PowerShell, Remote Control, claude project purge, claude rm, claude agents

## Открытые вопросы
- Which specific unwanted entries in the user's Recents are local desktop sessions vs server-backed cloud sessions cannot be determined from public sources alone
- Whether every junk entry is server-side is unresolved; evidence shows both https_claude.ai_* and app_localhost_* IndexedDB patterns depending on build generation
- No publicly documented, supported bulk-delete API/GraphQL mutation for consumer Claude Code sessions was found
- Whether Anthropic will ever expose a documented per-session permanent delete in the Desktop sidebar itself (currently absent) remains unknown

## Источник
- DR-ID `DR26-06-24-HUB-02` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-24-HUB-02-claude-desktop-code-recents-on-windows.md`

## Связано
- [[claude-desktop-sessions-per-account]]
- [[crash-recovery-command]]
- [[turnstate-ledger]]
- [[machine-bus-telegram-rail]]
- [[deterministic-script-gotchas]]
