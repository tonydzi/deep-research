---
dr_id: DR26-08-02-ZB-04-2324
title: "Multi-LLM coding orchestration: Claude Code as orchestrator, Codex/Antigravity/Grok/Cursor"
date: 2026-08-02
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-02-ZB-04-2324): Multi-LLM coding orchestration: Claude Code as orchestrator, Codex/Antigravity/Grok/Cursor as headless implementers

> Deep research on 2025-2026 state of the art for architectures where Claude Code orchestrates external CLI coding agents (Codex, Antigravity/Gemini, Grok, Cursor) as implementers, covering spec formats, review economics, isolation/safety, task routing and measurement.

## Ключевые выводы
- Architecture is [emerging] not [established]: practitioner reports exist (madewithlove.com June 2026, amanhimself.dev Feb 2026) but no controlled production-scale study compares Claude-orchestrates-Codex against direct Claude implementation
- Crossover problem is real and measured in one practitioner case: on a multi-file Sentry-integration task Codex used ~20x the tokens of Claude's orchestration layer, but on a trivial README edit Codex used 6,359 tokens vs Claude's 7,225 output tokens — a roughly fixed 5-7K token wrapper overhead makes delegation lose money on tiny tasks [single-source]
- Recommended implementer stack ranked: 1) Codex CLI (best documented headless contract, JSONL, Windows-native elevated sandbox, mature AGENTS.md), 2) Antigravity CLI (Google's current consumer lane, strong telemetry), 3) Grok Build CLI (device-auth, headless JSON, opaque quotas), 4) Cursor CLI (headless exists but quota accounting under Pro plan poorly documented)
- AGENTS.md evidence conflicts: a larger cross-agent study (Feb 2026, revised June 2026) found NO general task-success improvement and >20% higher inference cost from repo context files; a smaller study (10 repos, 124 PRs) found AGENTS.md associated with 28.64% lower median runtime and 16.58% fewer output tokens — synthesis: keep AGENTS.md short/operational (commands, invariants, constraints), not a generic repo tour
- No published universal review/write token-ratio or spec-cost-crossover formula exists [no source found]; routing thresholds by task duration (Codex should own the sub-10-min tier as pure spec is described as speculative operating heuristics only)
- Review can erase productivity gains: Meta's AI-fix-to-review-comments system initially made reviewers spend >5% longer reviewing when patches were shown directly (July 2025); METR's 2025 RCT found experienced devs 19% SLOWER with AI tools; a Feb 2026 METR update showed weak/uncertain reversal (18% speedup for returning devs, 4% for new devs, both CIs crossing zero)
- Self-repair round-trip cap is disputed: an April 2026 study found self-repair effective with most gains in first 2 rounds and lower token cost than blind resampling; a July 2026 placebo-controlled study on 1.5B-7B models found blind resampling stronger/tied and 2.5-5.5x cheaper because failed code anchors retries — recommended policy: 1 targeted repair + 1 fresh retry (different vendor) + Claude takeover after 2 failed attempts, itself only [emerging]
- Prompt injection via repository/vault content is empirically demonstrated: a July 2026 study on Claude Code/Codex memory files showed planted malicious instructions affecting current+later sessions; GitInject reproduced 11 injection attacks across GitHub workflow families; AIShellJack got up to 84% malicious command-execution success against Cursor/Copilot in tested configs — git worktrees isolate diffs but NOT authority (no protection against secret access, network exfiltration, or prompt injection)
- Antigravity's CLI can 'soft-deny' a headless action requiring unavailable approval while still exiting 0 (notice only in stderr) — exit code alone is not a valid success signal for headless pipelines [established, vendor docs]
- Codex loads at most one AGENTS.md per directory walking root→cwd, closer files take precedence, hard 32 KiB combined limit by default [established, vendor docs]

## Рекомендации / решения
- Adopt authority separation, not full delegation: Claude owns intent/spec/acceptance-oracle/final review; a deterministic host wrapper owns worktree/process/secrets-boundary/git integration; external CLI owns only the isolated diff and cannot commit/push
- Use a canonical, reusable task-contract template (YAML frontmatter + goal/repro/acceptance-criteria/invariants/examples/scope/context-manifest/verification-commands/stop-escalate-rules/required-result-JSON) passed byte-identical to whichever vendor is used
- Delegate only medium, bounded, testable tasks (rough sweet spot 30-120 min human-equivalent); do tiny tasks directly in Claude; keep security/architecture/migrations/hidden-invariant work in Claude
- Cap repair loop at: 1 targeted repair with same implementer + exact failure evidence, then 1 fresh retry (preferably different vendor, without the failed diff), then Claude takes over after 2 external attempts total
- Never treat a git worktree as a security boundary — add OS sandbox/low-privilege identity/network block/environment variable scrubbing (strip API keys, GITHUB_TOKEN, AWS/Azure creds, DB URLs) before invoking any implementer
- Expose the vault to implementers only via a curated agent-context/TASK-ID/ directory with explicit trust/authority frontmatter (treat as untrusted data, never as instructions) — never mount the full vault or raw-ingest/private folders
- Independent validation plane must re-run tests/build/lint/adversarial+smoke checks itself; never trust the implementer's prose claim that tests pass
- Run the local 1-week shadow pilot (as originally scoped) with an explicit ledger and flip criterion, since all economics numbers above are practitioner/single-source and must be re-measured on Anton's own repo/workload before adoption

## Сущности
- **Люди:** —
- **Компании:** OpenAI, Google, xAI, Cursor, Anthropic, Meta, METR
- **Продукты/инструменты:** Claude Code, Codex CLI (codex exec), Antigravity CLI (formerly Gemini CLI), Grok Build CLI, Cursor CLI, AGENTS.md, CLAUDE.md, git worktree, GitInject, AIShellJack

## Открытые вопросы
- Gemini/Antigravity vendor section was not retrieved on this collection tick (Mac16 dr-runner) — headless-panel mount failure across three surfaces (Chrome extension, computer-use, Firefox no AutoFF profile); pending fix = deploy AutoFF profile on Mac16
- No published universal review-token-to-write-token ratio or spec-cost/task-size crossover formula exists — thresholds in this report are explicitly [speculative] operating heuristics
- Cursor CLI's headless-mode quota accounting under a Pro subscription is not clearly documented — unresolved whether it should be a 4th implementer lane
- Grok Build CLI quota/rate-limit behavior remains opaque in vendor docs
- Self-repair vs blind-resampling disagreement is based on small (1.5B-7B) code models, not directly tested on frontier repository agents — needs local validation
- No controlled study directly compares the full Claude-orchestrates-external-CLI architecture against Claude implementing directly at production scale

## Источник
- DR-ID `DR26-08-02-ZB-04-2324` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- multi-agent-role-discipline
- test-after-build-skill
- model-routing-fable-smart
- prompt-injection
- AGENTS.md
- secondop-second-opinion
- shadow-first-mvp-pattern
