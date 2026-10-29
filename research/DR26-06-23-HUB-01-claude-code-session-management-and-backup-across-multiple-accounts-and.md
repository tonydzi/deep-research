---
dr_id: DR26-06-23-HUB-01
title: "Claude Code session management and backup across multiple accounts and machines"
date: 2026-06-23
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-23-HUB-01): Claude Code session management and backup across multiple accounts and machines

> How should Claude Code local sessions, config and account state be backed up and kept consistent across multiple machines and accounts without relying on unsupported live sync?

## Ключевые выводы
- Claude Code stores local plaintext session transcripts under «внутренний путь лаборатории» with a default 30-day retention (cleanupPeriodDays); the SDK's list_sessions()/get_session_messages() treat these transcripts as real operational state, not just a UI cache.
- Anthropic does NOT offer a general feature to sync all local Claude Code sessions/settings/history across machines and accounts — only narrower features exist: Remote Control (mirrors a still-running LOCAL session to web/mobile, nothing moves to cloud), VS Code's one-way download of certain cloud sessions from claude.ai (not synced back), and Claude app chat memory/search becoming portable.
- Account portability is officially narrow: personal→Team/Enterprise migration is supported (chats, projects, files, memory, connectors, skills move), but Anthropic explicitly does NOT support importing exported data into another personal account or migrating data between two personal accounts.
- Naive file-sync (Syncthing) is fundamentally file-replication, not transactional state management: it creates sync-conflict copies on simultaneous edits and is explicitly documented as not a backup system; SQLite explicitly warns of corruption under unreliable/networked locking and WAL requires single-host.
- Direct evidence of breakage exists: an April 2026 Windows bug report showed .claude symlinked into a OneDrive-synced folder causing EEXIST failures in session-env/<UUID>, breaking Bash/slash-commands/agents — proof that putting Claude's volatile runtime tree inside live cloud-sync/symlinked directories is brittle.
- GitHub issues confirm cross-machine session sync, per-account settings sync, portable project memory, and synced VS Code Remote session history are current unmet feature requests, not solved workflows as of 2026.
- Anthropic documents a SessionEnd hook (fires on session exit, /clear, session switch) as the clean mechanism to mirror only stabilized/closed transcripts rather than live-appending files.
- Mackup's own docs now recommend copy mode over symlink 'link mode' for backup/restore, reinforcing that symlinking volatile state is a poor default pattern.

## Рекомендации / решения
- Adopt a layered/two-tier architecture: (1) working tier — each machine keeps live auth/settings/active sessions strictly local; use Remote Control to attach another device to the SAME running session instead of opening it from a second machine; (2) archive tier — a SessionEnd hook or scheduled job mirrors only closed/stabilized transcripts to a backup location.
- Increase retention beyond the 30-day default via cleanupPeriodDays if longer local recovery windows are needed.
- Keep portable, deterministic text config (CLAUDE.md, skills, prompts, MCP definitions) in Git/chezmoi with hostname-aware templating and env-var path indirection — do not hardcode user paths.
- Never live-sync the whole «внутренний путь лаборатории» tree; never make .claude a consumer-cloud-synced symlink root (proven to break via the OneDrive bug).
- Enforce single-writer-per-session semantics: don't resume the same session on a second machine until the first has closed and the closed transcript has propagated.
- If continuing to use Syncthing for transcript archival, use send-only/single-writer staging + receive-only mirrors + versioning enabled, rather than syncing the active writer directory directly.
- Build and maintain your own neutral session catalog (session_id, summary, cwd, created_at, last_modified, machine, account/org label) via the SDK's list_sessions(), rather than depending on any single Desktop 'recent sessions' UI registry.
- Do not attempt arbitrary account-to-account history merges by editing registry files directly — treat that as unsupported and likely to produce stale pointers/ghost entries/inaccessible sessions; use the officially supported personal→Team/Enterprise migration path if account consolidation is needed.
- One-sentence policy: sync configs like code, sync transcripts like archived logs (post-close only), keep runtime/account state local.

## Сущности
- **Люди:** —
- **Компании:** Anthropic
- **Продукты/инструменты:** Claude Code, Claude Desktop, Claude Agent SDK, VS Code, claude.ai, Remote Control, Syncthing, SQLite, chezmoi, Git, Mackup, OneDrive, GitHub

## Открытые вопросы
- The exact format of the Desktop app's per-account 'recent sessions' registry is not publicly documented by Anthropic — treated as a likely cache/index only by inference.
- No official announcement exists of a planned general cross-machine full sync feature for local Claude Code sessions/settings; multiple GitHub feature requests show demand but not a committed roadmap item.
- Community/GitHub issue reports illustrate real-world failure patterns (e.g. Windows file-locking, OneDrive symlink bug) but are not vendor-guaranteed limitations — behavior could change without notice.
- Exact scope/limits of Remote Control's 'stays in sync across connected devices' mechanism beyond the documented cases isn't fully specified.

## Источник
- DR-ID `DR26-06-23-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- claude-desktop-sessions-per-account
- machine-bus-telegram-rail
- one-system-propagate
- session-machine-tagging
- config-safety-backup-and-migration-check
- deterministic-script-gotchas
- vault-backup-rule
- machine-migration
