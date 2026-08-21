---
dr_id: DR26-06-29-LEG-03
title: "Multi-LLM collaboration architecture across native vendor harnesses on a shared Obsidian v"
date: 2026-06-29
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-29-LEG-03): Multi-LLM collaboration architecture across native vendor harnesses on a shared Obsidian vault + git repo

> How can Claude Code, Codex CLI, Gemini/Antigravity and Grok Build reliably collaborate on Anton's shared markdown vault/git repo without cross-vendor subscription arbitrage, and what rules/coordination/consensus/governance setup makes that stable?

## Ключевые выводы
- Cross-vendor subscription arbitrage is impossible and prohibited; each agent must run on its own harness/subscription, so coordination has to be file/git-based rather than shared execution.
- AGENTS.md is emerging as the cross-vendor canonical rules file (60k+ repos, Linux Foundation/AAIF track), readable primarily by Codex and also Claude Code, Cursor, Grok Build; vendor-specific files (CLAUDE.md, GEMINI.md) should be thin generated layers pointing back to it, preserving human overrides via .local.md or 'GENERATED — do not edit' headers.
- Context files (AGENTS.md/CLAUDE.md) are hints, not enforcement: minimal human-curated versions give ~+4% task success, but LLM-generated or bloated ones cause ~-3% success and >20% token/cost overhead (ETH Zurich/arXiv 2602.11988, Feb 2026) — keep them tiny and high-signal.
- Heterogeneous (multi-vendor) reviewer/critique pairs catch significantly more defects than same-vendor pairs (~70% vs ~53% in aligned studies; +4-7% absolute in heterogeneous MAD ablations) — vendor diversity outperforms same-vendor variants by ~5%+.
- Multi-agent debate/Mixture-of-Agents improves complex reasoning/coding tasks by +4-13% in strong setups (e.g. A-HMAD) but backfires on easy tasks via sycophancy, wrong-consensus convergence, persuasion attacks, and cost/latency blowup; advocate-skeptic or judge/verifier topologies are more robust than open debate.
- For concurrency control on Windows with native interactive harnesses, git branches/worktrees + atomic provenance-tagged commits + human/cross-agent review before merge is the reliable pattern; file locks/leases are flaky cross-harness/cross-OS, so rely on git merge discipline plus explicit 'claim' markers in blackboard tasks.md instead.
- MCP (tool/context access, Anthropic 2024→AAIF) and A2A (agent-to-agent handoff, Google 2025→AAIF) are maturing complementary protocols but are secondary/optional upgrades — file/git/blackboard markdown remains the reliable zero-new-infra baseline for native Windows harnesses.
- MAST taxonomy (2025, 1,600+ traces) identifies 14 structural failure modes (spec ambiguity, inter-agent misalignment, verification/termination gaps, cascading errors, collusion, monoculture bias, uncontrolled loops) as the dominant failure source — not individual model weaknesses; prompt-only rules are insufficient against these without institutional/governance mechanisms.
- Adding a 3rd/4th vendor only pays off with measurable net lift (>5% defect reduction or correctness gain after subtracting cost/latency); 2-4 agents/rounds is typically optimal, beyond which returns diminish or go negative.

## Рекомендации / решения
- Start with a single canonical minimal AGENTS.md (cross-vendor process, anti-loop, provenance, git discipline, blackboard conventions) and generate thin vendor-specific files (CLAUDE.md, GEMINI.md) from it via script, rather than maintaining duplicated rule files.
- Set up a git branch/worktree + blackboard markdown workflow (.agents/tasks.md, reviews/, decisions.md) as the core coordination layer instead of relying on file locks or headless piping.
- Extend the existing Claude-reviews-Codex-diff skill into a standard heterogeneous cross-critique step (advocate-skeptic or judge topology, 2-3 rounds max, different-vendor pairs) before merges.
- Require human-in-the-loop gates on merges to shared/main branches and on any rule changes; keep full git audit trail (blame/log/diff) plus blackboard provenance tags per vendor/model/timestamp.
- Onboard additional vendors (Antigravity/Gemini, then Grok) one at a time only when metrics show >5% net quality lift after cost/latency overhead; don't scale agent count for its own sake.
- Treat MCP/A2A as opportunistic upgrades (e.g., an MCP server for the vault) rather than prerequisites — adopt only if/when richer handoff needs justify the added complexity.
- Follow the phased action plan: Phase 0 (1-2wk) trim AGENTS.md + blackboard skeleton; Phase 1 (2-4wk) git review workflow + heterogeneous cross-critique + metrics logging; Phase 2 (1-2mo) onboard Gemini, pilot 3-vendor judge topology; Phase 3 (ongoing) formalize governance, consider CRDT only if conflicts become chronic.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, OpenAI, Google, xAI, IBM, Linux Foundation, ETH Zurich
- **Продукты/инструменты:** Claude Code, Codex CLI, Google Antigravity/Gemini, Grok Build, AGENTS.md, CLAUDE.md, GEMINI.md, MCP, A2A, ACP, Mysti, Syncthing, Obsidian, Automerge, Yjs, AgentGit, AAIF (Agentic AI Foundation)

## Открытые вопросы
- Exact percentage gains for coding tasks from multi-agent debate remain moderately contested across studies (strongest evidence is on protocols and failure taxonomies, weaker on precise coding-task % lift).
- Whether lightweight CRDT integration (Automerge/Yjs) is ever needed for live collaborative markdown editing, or whether git-based append-only history remains sufficient long-term.
- Optimal stopping rules and round-count thresholds for debate/consensus topologies before diminishing/negative returns kick in were not precisely quantified beyond a general '2-4 agents/rounds' heuristic.
- Whether/when a 4th vendor (Grok) should be onboarded — explicitly deferred pending data from the 3-vendor pilot in Phase 2.

## Источник
- DR-ID `DR26-06-29-LEG-03` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\2026-06-29-DR-multi-llm-collab-grok.md`

## Связано
- [[multi-agent-role-discipline]]
- [[one-system-propagate]]
- [[machine-bus-telegram-rail]]
- [[AGENTS.md standard]]
- [[MAST failure taxonomy]]
- [[heterogeneous cross-review]]
- [[machine-governance-leader-follower]]
- [[insight-DR-DR26-06-28-HUB-02-архитектура-мульти-агентной-системы-с-vault-как-ед]] — тот же вопрос архитектуры коллаборации нескольких LLM через native harness над одним vault
