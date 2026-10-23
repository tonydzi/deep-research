# DR26-07-05-ZB-02 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "AI-agent session isolation"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4add7f-e050-83eb-8ad6-b4acadfb92db
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-05
date_added: 2026-07-07
valid_as_of: 2026-07-05
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 3
related_concepts: []
revisit_if: ""
---

# AI-agent session isolation

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

# DR26-07-05-ZB-02 — Single-host isolation of multiple concurrent AI-agent sessions editing shared sensitive files



CONTEXT (what we already know — do NOT re-research this, dig deeper/around it):

- Setup: one person's private "fleet" = an always-on hub desktop + a laptop + 2 Macs + 1 VPS, all kept in one synchronized environment by Syncthing (shared Obsidian vault + shared «внутренний путь лаборатории» config). Multiple Claude Code (agentic CLI) sessions run concurrently — often SEVERAL on the SAME machine at once — plus scheduled/background agent jobs.

- The CROSS-MACHINE coordination problem is already solved and NOT the focus: we use per-machine single-writer append-only JSONL shards (git 3-way-merges them, zero sync conflicts) + a lightweight consensus engine with a fixed leader tie-break. We deliberately REJECTED Raft/Paxos/CometBFT/CRDT-engines/OrbitDB as overkill for a ~4-node personal fleet. Do not propose those.

- The UNDER-RESEARCHED gap = the SAME-MACHINE case: two or more agent/editor sessions on ONE host, sharing one filesystem and one config directory, editing the SAME sensitive files. Here there is NO sync-conflict safety net — it's silent last-writer-wins clobber. This is what we need best practices for.

- Sensitive shared targets: always-loaded agent instruction files (a global CLAUDE.md + a MEMORY.md index loaded into EVERY session), a rules/"Bible" corpus, the «внутренний путь лаборатории» config tree (skills/hooks/scheduled-tasks/settings.json), shared SQLite databases (CRM leads.db, names.db, a system.db, a turnstate.db), and a plaintext secrets store.

- Hard constraint "AK-47": prefer the SIMPLEST repairable solution a NON-technical owner can understand and fix himself. Flag any added complexity explicitly with the pain it removes. Windows is the primary OS (also macOS, Linux VPS).



TOPIC:

How do practitioners safely isolate, segregate, and coordinate MULTIPLE concurrent AI-agent / coding-agent / IDE sessions running on a SINGLE machine that share one filesystem and one config environment, when several sessions may edit the same sensitive files at the same time? Cover the full spectrum from zero-infra conventions to real isolation, and say which fits a small single-user multi-agent fleet.



Investigate specifically:

1. OS-level concurrency primitives on a shared host: advisory vs mandatory file locking (fcntl/flock, Windows LockFileEx, O_CREAT|O_EXCL lockfiles), atomic write patterns (write-temp + atomic rename), fencing tokens, lock TTL / stale-lock recovery, and the TOCTOU race between "check free" and "acquire". Which are reliable on Windows specifically?

2. Per-session workspace isolation: git worktree-per-session/agent, per-agent branches + merge, copy-on-write / overlay filesystems, ephemeral sandboxes, containers/namespaces, chroot/jails. When is isolation-then-merge better than a shared lock? Costs and failure modes.

3. How existing multi-agent & agentic-coding tools actually handle concurrent sessions on one machine: Claude Code, Cursor, Aider, Windsurf, Devin, OpenHands/OpenDevin, LangGraph, AutoGPT/AutoGen, CrewDD — do any lock, sandbox, worktree, or serialize file writes? What are their documented patterns and known collision bugs?

4. How mainstream IDEs/editors prevent two instances from clobbering the same file (VS Code, JetBrains, vim swapfiles, Emacs lockfiles) — the "file is already open/locked elsewhere" mechanisms and what we can copy.

5. Shared-database concurrency on one host: SQLite WAL + busy_timeout + transaction discipline, when SQLite is enough vs when to move to a server DB, advisory locks (Postgres), and how agents should serialize writes.

6. Config & secrets segregation for concurrent sessions: keeping per-session/per-agent state out of shared always-loaded files, single-writer ownership, "read is free, write needs a lease" designs, and preventing a background job from clobbering an interactive session's edit to the same config.

7. The human/coordination layer: session presence/marking, lightweight lease registries, editor-style "someone else is editing this" signaling for autonomous agents, and auto-handoff when a session dies holding a lock.



OBJECTIVE:

Find the best current practices, state-of-the-art approaches, real failures, trade-offs, and an implementation framework. Rank solutions by simplicity/repairability for a single-user multi-agent fleet, and be explicit about what is overkill.



RESEARCH REQUIREMENTS:

1. Academic research — papers, meta-analyses, benchmarks (concurrency control, optimistic vs pessimistic locking, single-writer designs).

2. Industry practice — how agent-framework and coding-agent projects (open source + companies) solve concurrent single-host file access.

3. Competitive landscape — direct (multi-agent orchestration), indirect (IDE multi-instance, build systems, VCS), substitutes (full isolation vs locking vs serialization).

4. Failure analysis — real silent-clobber / lost-update / deadlock / stale-lock incidents and lessons learned.

5. Emerging trends — last 12 months in agentic-coding concurrency and sandboxing.

6. Implementation recommendations — a concrete, AK-47-ranked framework: what to do with zero infra, what to add if pain appears, what to avoid.



OUTPUT FORMAT:

A. Executive Summary

B. Key Findings

C. Contradictions & Debates (e.g. lock vs isolate-and-merge; advisory vs mandatory)

D. Best Practices (ranked by simplicity/repairability; same-host vs cross-host called out separately)

E. Failure Modes (silent clobber, lost update, deadlock, stale lock, split-brain)

F. Strategic Recommendations (what fits a small single-user multi-agent fleet; what is overkill)

G. Action Plan (staged: zero-infra now → add-if-pain → avoid)

H. Sources@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"prompt":"# DR26-07-05-ZB-02 \u2014 Single-host isolation of multiple concurrent AI-agent sessions editing shared sensitive files\n\n(Use the user's full prompt from this conversation verbatim as the research specification, including output format A-H and research requirements.)","attachments":["/mnt/data/Recall \u043f\u043b\u044e\u0441 Deep Research.txt"]}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"# DR26-07-05-ZB-02 \u2014 Single-host isolation of multiple concurrent AI-agent sessions editing shared sensitive files\n\nUse the full user request in this conversation as the research specification. Preserve the requested scope, investigation points, research requirements, and output format A-H."}}
```
