---
dr_id: DR26-11-17-MAYAK-01
title: "Attributing Codex/ChatGPT subscription usage per session and per machine"
date: 2026-11-17
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-17-MAYAK-01): Attributing Codex/ChatGPT subscription usage per session and per machine

> How to track and attribute Codex (ChatGPT subscription) token usage per session/device, given that official tools only show account-wide quota, and which tools can merge multiple machines.

## Ключевые выводы
- No official per-session or per-device usage ledger exists: OpenAI staff confirmed (Sep 6, 2026) there is no per-task breakdown in the web/app UI and 'no timeline'; /usage, /status, Settings→Usage, the wham/usage endpoint and the app-server RPC are all account/workspace-wide, so global remaining quota is not attribution.
- Attribution evidence lives locally in ~/.codex (%USERPROFILE%\.codex) rollout JSONL: per-event cumulative token counters (input incl. cached; output incl. reasoning), per-turn model and cwd, fork/subagent lineage, and 5h/weekly limit snapshots, verified from the live openai/codex protocol source.
- Counting rule: never sum last_token_usage. Rate-limit-only events re-emit stale deltas (openai/codex issue #14489, closed as 'working as intended'), and tools that did this over-counted by roughly 2x. Derive deltas from total_token_usage high-water marks instead.
- Only two tools can merge multiple machines today: ccusage v20 (the @ccusage/codex package is deprecated since May 2026; CODEX_HOME accepts comma-separated directories) and tokscale (machineLabels plus group-by machine,model).
- Other tools are limited: CodexBar (steipete) and nervouna's codex-usage-monitor show account-wide quota via chatgpt.com/backend-api/wham/usage or app-server JSON-RPC; harveyxiacn's codex-usage-monitor is the best local single-machine cross-check; tokentop treats Codex only as an OAuth-polled provider among 11, not per-session JSONL.
- Coverage gaps: cloud tasks and pre-Sept-2025 sessions are invisible in local logs, so local counters need weekly reconciliation against /usage.
- Official docs moved to learn.chatgpt.com, which blocks curl (Vercel bot protection); other sources (GitHub raw, npm registry, ccusage.com, community forum) were reachable. Latest openai/codex release at research time was 0.169.0 (Nov 13, 2026).
- The report is tagged VERIFIED/INFERENCE/UNKNOWN throughout and includes a 10-task validation protocol to run before building any recurring workflow.

## Рекомендации / решения
- Build a per-host counter-only collector that reads only token counters from rollout JSONL, never auth.json, prompts or SQLite, polls deterministically, and does no LLM wake-ups.
- Compute usage deltas from total_token_usage high-water marks and never sum last_token_usage.
- Use ccusage v20 with comma-separated CODEX_HOME, or tokscale with machineLabels, for multi-machine aggregation; use harveyxiacn's monitor as a local cross-check.
- Reconcile local totals weekly against account-wide /usage to estimate the share not visible locally (cloud tasks, old sessions).
- Run the Section 10 validation experiment on the machines right after a quota window reset, before adopting any recurring workflow.

## Сущности
- **Люди:** steipete, junhoyeo, harveyxiacn, nervouna
- **Компании:** OpenAI, Vercel, Z.ai
- **Продукты/инструменты:** Codex CLI, ChatGPT subscription, ccusage v20, @ccusage/codex, CodexBar, tokscale, tokentop, codex-usage-monitor (harveyxiacn), codex-usage-monitor (nervouna), wham/usage endpoint, Codex app-server JSON-RPC, openai/codex issue #14489, learn.chatgpt.com, CODEX_HOME

## Открытые вопросы
- Rate-card weights for Codex usage remain unverified.
- Exact wording of the official /usage documentation could not be fetched because learn.chatgpt.com blocked curl and web tools were limited.
- How large the share of cloud-task and pre-Sept-2025 usage is that stays invisible locally has not been measured.
- Whether the proposed collector's totals reconcile with /usage within an acceptable tolerance is untested until the 10-task protocol is run.
- The report is labelled DR26-11-15-HUB-01-0805 while the registry ID is 0905, a mismatch needing verification.

## Источник
- DR-ID `DR26-11-17-MAYAK-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- LLM subscription usage attribution
- Fleet token cost accounting
- Codex rollout JSONL
- Rate-limit window tracking
- Multi-machine usage aggregation
- Counter-only collectors (no secrets, no LLM)
- Measure token cost of every improvement

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-17 через gh: 1 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 1 · 🤷 0.
