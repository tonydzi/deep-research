---
dr_id: DR26-06-23-HUB-02
title: "Claude Code Multi-Account and Multi-Machine Operations"
date: 2026-06-23
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-23-HUB-02): Claude Code Multi-Account and Multi-Machine Operations

> Investigates whether the same working folder and local toolchain survive switching Claude accounts, and what is/isn't officially supported for portability across multiple accounts and machines.

## Ключевые выводы
- Claude Code separates user/project/local machine state (settings, skills, hooks, project .mcp.json, desktop extensions) from account-scoped cloud features (OAuth, billing, remote connectors) — so the same repo/folder keeps working after switching accounts, but account-bound state does not.
- Remote connectors, usage limits/billing, and Cowork are account/org-brokered: Team/Enterprise owners enable connectors, each user connects individually; usage is shared across Claude web/Desktop/Code within one account.
- Officially supported portability paths are narrow: same account/workspace, personal→Team/Enterprise migration (one-way, includes chats/projects/files/memory/connectors/skills), Remote Control (continue a running local session from another device), and one-way VS Code resume of certain cloud sessions (changes not synced back to claude.ai). Exported data cannot be imported into another personal account.
- No official support exists for merging per-account 'recent sessions' registries across unrelated personal accounts — any such filesystem-level merge is unsupported and can fail silently after app updates.
- Anthropic doesn't explicitly ban owning multiple personal accounts, but Consumer Terms forbid credential sharing, the Usage Policy forbids coordinated multi-account activity to evade detection/guardrails, and Claude Code docs say Pro/Max limits assume 'ordinary, individual usage' — rotating several personal accounts to spread rate limits is not an endorsed model and carries enforcement risk.
- Live session state (transcripts, .jsonl logs, SQLite-backed metadata) is unsafe to sync bidirectionally across machines: Syncthing creates sync-conflict files on concurrent edits, SQLite warns about corruption/locking on networked filesystems, and Claude Code's own changelog shows recent fixes for truncation on network/cloud-synced folders and history loss from concurrent cleanup processes.
- Cowork controls appear inconsistent across docs: one support article says Team/Enterprise Cowork is an org-wide toggle with no per-user granularity, but later release notes describe Enterprise role-based access controls enabling finer gating — Team likely stays org-wide while Enterprise may now allow team-level control.
- `.mcp.json` supports ${VAR} / ${VAR:-default} environment-variable expansion for command/args/env/url/headers, enabling one shared project config to adapt across machines with different paths instead of hardcoding absolute paths.
- CLAUDE_CONFIG_DIR (Linux/Windows) can relocate .credentials.json, enabling launcher-script or OS-profile-based credential isolation per account without a first-class 'profiles' feature existing in the product.

## Рекомендации / решения
- Adopt a layered state model: portable project state (.mcp.json, CLAUDE files, skills, hooks) in version control; machine-local accountless state (desktop extensions, local MCP servers, secrets in OS keychain) kept stable; account-bound cloud state tracked via a per-account/org manifest (required connectors, org capabilities, expected login org); history/archives treated as cold backups, not live shared state.
- Use git + chezmoi (or similar) for declarative, templated config instead of raw Syncthing sync of absolute-path JSON, to handle machine-to-machine path/OS differences.
- Isolate credentials via CLAUDE_CONFIG_DIR-based launcher scripts or separate OS user profiles per account rather than sharing one home directory across accounts.
- Treat remote connectors as re-connectable entitlements per account — reconnect via manifest + scripted post-login verification, never attempt to copy hidden auth material between accounts.
- For transcripts: keep a 'local live store + synced archive mirror' design — let Claude Code write locally, then copy only quiescent (idle/closed) session directories into a separately synced archive tree; never let the same live transcript be concurrently active on multiple writer machines.
- For genuinely compliant multi-user/multi-machine operation at scale, migrate to Team/Enterprise seats or API/Console-backed Claude Code (with workspace spend/rate controls) rather than relying on unsupported consumer-account rotation.
- Use Remote Control for continuing the same local session across devices, and Claude Code on the web (with VS Code local resume where useful) for cloud execution — instead of building custom cross-account session-merging tools.
- If keeping a custom recents-merger tool, make it rollback-safe: back up before writing, validate JSON schema, never mutate while the app is running, and expect breakage after Claude Code updates.

## Сущности
- **Люди:** —
- **Компании:** Anthropic
- **Продукты/инструменты:** Claude Code, Claude Desktop, Claude (claude.ai), Cowork, Remote Control, VS Code, MCP (Model Context Protocol), Syncthing, SQLite, chezmoi, Claude Team, Claude Enterprise, Claude Pro/Max, Console (API billing)

## Открытые вопросы
- The schema and support status of the desktop per-account 'recent sessions' registry is undocumented by Anthropic — any recommendation touching it is inferential, not officially blessed.
- No official Anthropic documentation confirms a first-class 'Claude Code profile system' for running multiple consumer accounts on one machine; current primitives are just account switching, OAuth credential management, and CLAUDE_CONFIG_DIR relocation.
- Whether Team's Cowork toggle truly stays org-wide-only or has gained finer per-team gating (per newer Enterprise release notes) is not fully reconciled across Anthropic's own docs.
- Exact enforcement risk threshold for owning/using multiple personal accounts (vs. sharing/coordinating them) is not explicitly quantified by Anthropic — inferred only from Terms/Usage Policy language.

## Источник
- DR-ID `DR26-06-23-HUB-02` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-06-23-HUB-02-claude-code-multi-account-and-multi-machine-o.md`

## Связано
- [[machine-migration]]
- [[session-machine-tagging]]
- [[credential-store]]
- [[machine-bus-telegram-rail]]
- [[config-safety-backup-and-migration-check]]
- [[claude-desktop-sessions-per-account]]
- [[vault-data-architecture]]
- [[machine-governance-leader-follower]]
