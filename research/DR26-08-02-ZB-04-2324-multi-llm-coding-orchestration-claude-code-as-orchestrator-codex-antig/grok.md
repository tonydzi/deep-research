# DR26-08-02-ZB-04-2324 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-02-ZB-04-2324
vendor: grok
mode: Heavy (Deep Research)
chat_url: https://приватный чат

collected_at: 2026-08-03T00:45Z
proof_of_work: "462 sources · Heavy" (счётчик Grok в футере ответа); флаг леджера «Thought for 1m19s подозрительно быстро» СНЯТ — рой реально искал
capture_method: get_page_text (claude-in-chrome) -> ПЕРЕНАБРАНО в markdown (заголовки/списки восстановлены). НЕ байт-в-байт innerText; содержание и цифры не менялись
note: тело = только ОТВЕТ Grok; промпт лежит в леджере _drafts/DR-FANOUT-DR26-08-02-ZB-04-2324-multi-llm-coding-orchestration.md
---

# Best Practices for an "Orchestrator + External Implementers" Multi-LLM Coding Pipeline (2025–2026 State of the Art)

*Grok Heavy, «Thought for 58s», 462 sources.*

**Scope / Date:** Practitioner reports, GitHub projects, official CLI docs, arXiv/preprints, and X discourse primarily from Feb–Aug 2026 (current time 2026-08-03). Solo-founder Windows 11 lab with subscription CLIs, Claude Code as non-coding orchestrator, local headless implementers (Codex exec, gemini-cli, Grok Build), Obsidian vault RO-exposed.

**Overall confidence:** [emerging]. Strong, convergent practitioner patterns and official tooling docs; limited large-N controlled experiments on exact first-pass rates, token deltas for alien-diff review, or headless-implementer success by task class. No fabricated sources.

## Executive Summary

- Real cross-vendor orchestration (Claude Code planning/verifying + Codex/Gemini/Grok implementing via headless CLIs) is actively practiced in 2026 via plugins (DevSquad, Codex Orchestrator), MCP gateways, tmux, and git worktrees. Composition ("Claude brain, Codex hands, Gemini eyes") is the dominant framing. [emerging]
- Structured task contracts (GOAL / SCOPE / CONTEXT file-list / DONE-WHEN / verification) materially raise first-pass success for headless runs; overhead is worthwhile above ~1–2 human-hours of code. [emerging]
- Review is token-expensive (often majority of agentic spend); cap at 1–2 return cycles then orchestrator takes over. Diff-review preferred to full regenerate for cost. [emerging]
- Isolation standard is git worktree + dedicated branch + sandbox `workspace-write`; full FS isolation needs extra containers or OS sandbox. Vault prompt-injection risk is real. [established for worktrees; emerging for residual risks]
- Route scripts, tests, localized refactors/parsers/chore to implementers; keep architecture, cross-cutting infra, safety-critical, and heavy-domain context with Claude. [emerging]
- Measurement is sparse; a simple per-task ledger of orchestrator tokens, cycles, first-pass, wall-time supports a 1-week pilot with a clear ≥30% orchestrator-token flip criterion. [speculative on exact savings]
- Tooling (2026): Codex exec mature; Gemini auth fragile for personal accounts; Grok Build headless ready on SuperGrok/Premium+; Cursor has usable headless CLI on Pro.

## 1. Orchestration Patterns

Documented architectures treat one LLM (usually Claude Code) as non-coding supervisor that decomposes, writes contracts, dispatches headless implementers in isolated environments, reviews diffs/tests, and integrates.

**Working patterns (practitioner reports):**

- **Claude as primary + plugin interception (DevSquad):** hooks block expensive tool calls and redirect file-reads/tests/docs to cheaper CLIs after thresholds. "The difference between 'asking nicely' and 'blocking the call' is the difference between a fun toy and a working orchestrator." — codewithseb.com
- **Explicit MCP delegation** (Roundtable AI) or bash `codex exec` / `gemini -p` / `grok -p` hand-offs.
- **Parallel worktrees** (johannesjo/parallel-code, Claude native isolation: worktree, Grok `-w`): one agent per branch/worktree; review diffs then merge.
- **Journal + claim-vs-evidence** (XDA / Codex Orchestrator plugin, Jul 2026): Claude plans/verifies/journals; Codex implements via `codex exec`; Claude re-runs tests because "a Codex agent reporting 'tests pass' wasn't proof." Orchestrated setup 9/10 vs solo 8/10 on a bug-fix set.
- **AWS Labs CLI Agent Orchestrator (CAO):** local server + isolated tmux sessions for Claude/Codex/Cursor/etc.
- **Swarm examples** (EloPhanto DEV.to Feb 2026; stellarlinkco/myclaude; multicli MCP): parallel independent features, multi-model review.

**What failed / friction:** Context-handoff loss (mitigated by structured files/journals); auth sprawl and rate limits; debugging across three CLI histories; early worktree isolation bugs in Claude Code (fixed by Jul 2026); selection effects (easy tasks outsourced). Composition now preferred over monolithic CLIs; "Composition won."

**X discourse (last ~30 days, Aug 2026):** Practitioners describe Claude as orchestrator dispatching to Codex/Gemini/Grok via folders or sub-agents, parallel PRs, and token-limit management. Representative: "you can use Claude for organise and delegate-quality check to other subscriptions … one folder per task" (Javi Guembe); routing Codex for parallel async work; some prefer Claude for complex reasoning and Codex for terminal/CI. Verified accounts mixed with anonymous; treat as sentiment, not controlled data. [emerging / low-authority for individual posts]

No large academic multi-vendor "Claude-orchestrates-Codex" controlled trials found; evidence is practitioner blogs + GitHub + one XDA hands-on. [emerging]

## 2. Spec Contract Format

Highest first-pass success for headless (`codex exec`, `gemini -p`, `grok -p`) comes from explicit, verifiable contracts rather than conversational prompts.

**Effective field list (convergent across sources):**

1. Goal (specific, measurable, observable done condition)
2. Scope (allowed paths + explicit out-of-scope)
3. Context (reading list of 10+ files/docs; prefer references + key excerpts over full inline dump)
4. Constraints / anti-patterns / architecture fences
5. Permissions (sandbox, network, edit rights)
6. Done-when / acceptance criteria (tests green, specific files/behavior)
7. Verification loop + output contract

Examples: tectontide.com (May 2026) XML-style `/goal` prompts (600+ words) after Claude interview + Codex challenge pass; agent-spec / project-spec 7-section templates (Mission/Architecture/Components/Constraints/Build-Order/Done-When); Codex official best-practices (Goal + Context + Constraints + Done-when).

**Context packing:** Inline only critical excerpts; reference paths; keep standing `AGENTS.md` / `CLAUDE.md` short (~32 KiB limit for Codex, layered discovery). Full vault only if RO + allow-listed.

**Overhead vs task size:** Spec writing (10–15 min via structured interview) is cheaper than debugging loops for multi-file/refactors; for tiny scripts the overhead can exceed writing the code yourself. [emerging / single-source on exact times]

## 3. Review Economics

Agentic coding is input-token dominated. Cache reads and context loading can be 80–96% of tokens; generation itself is a minority. Code-review / verification stages frequently consume the largest share (one analysis ~59% of tokens in a multi-agent pipeline). — Jellyfish + others

- Reviewing an alien diff still costs (orchestrator must load context + diff + run tests), but is usually cheaper than full high-tier generation + its own internal loops.
- **Optimal round-trip cap: 1–2 returns** to implementer; beyond that the reviewer (Claude) should take over. Practitioner consensus from multi-CLI write-ups.
- Diff-review preferred for routine work; regenerate-and-compare for high-stakes or when the implementer model is known to be weak on the task class.

No precise head-to-head "Claude reviewing Codex diff vs Claude writing from scratch" token table found for identical tasks. High variance (up to 30× on same task) is documented. [emerging]

## 4. Isolation & Safety

**Best practice:**

1. Orchestrator stays on main/worktree.
2. `git worktree add ../agent-task-N -b agent/task-N` (or Claude/Grok native flags).
3. Implementer runs inside the worktree with `--sandbox workspace-write` (Codex) or equivalent.
4. Review diff / tests / adversarial breaks; merge or discard; remove worktree.

This is the dominant pattern (parallel-code, Boris Cherny tip, multiple Jun–Jul 2026 blogs, Grok/Claude native support).

**Residual risks:** Worktrees isolate working tree + branch + index but **not** full filesystem, home directory, network, or environment variables. Agent can still `cd ..` or read secrets. Mitigate with Codex/OS sandbox, containers (h5i-style), or deny-lists. Never let implementer push to main or touch secrets.

**Vault:** Expose RO + allow-list relevant notes only. Full untrusted markdown carries prompt-injection risk (Anthropic containment guidance). Output-to-review-folder or PR-only integration preferred. Windows 11: Codex elevated sandbox available; worktrees native. [established for worktree convention; emerging for residual attack surface]

## 5. Routing

**Higher success with headless implementers** (practitioner + stratified PR data): scripts, unit/integration tests, chore/docs, localized refactors/relocation, simple parsers/fixers. Codex often strong on fix/refactor; Claude stronger on docs/features.

**Lower success / keep with orchestrator:** cross-cutting infrastructure, architecture-layer changes, safety-critical, heavy domain-context multi-file work, acceptance-level tests, performance. Architecture gaps can make even "easy" patches fail.

No large published success-rate table specifically for "headless external implementer under Claude orchestration." Consensus is "grunt work out, reasoning in." [emerging]

## 6. Measurement

Hard multi-vendor pilot data is sparse. Useful metrics: orchestrator tokens per task (vs solo baseline), implementer tokens, rework rate (cycles or human takeover %), first-pass acceptance, wall-clock time, total tokens.

**Pitfalls:** survivorship (only easy tasks outsourced), ignoring review/integration cost (code is cheap; review is expensive), shrinking denominators.

**Minimal 1-week shadow pilot ledger** (CSV or simple log):

```
task_id | type | orch_tokens | impl_tokens | cycles | first_pass (Y/N) | wall_s | notes
```

Instrument via CLI `--json` / `-o` outputs + git stats. Flip criterion "≥30% orchestrator-token savings and rework not worse than baseline" is operationally sensible and matches the spirit of practitioner cost reports (~$25–40/week claimed savings in one stack). [speculative on exact 30% threshold; emerging on metric set]

## 7. Tooling Specifics (2026)

- **Codex exec:** `codex exec "prompt" --sandbox workspace-write --json --ephemeral -o file [--ask-for-approval never]`. AGENTS.md layered (global → project, ~32 KiB). Sandbox modes: `read-only` (default safe), `workspace-write`, `danger-full-access`. Official docs emphasize least privilege for CI. Quotas via subscription/API. [established]
- **gemini-cli headless:** `gemini -p "..."` or stdin; `--output-format json`. Personal Google accounts frequently hit "ineligible or project ID" / 403 when `GOOGLE_CLOUD_PROJECT` is set; **unset it** for OAuth free/pro tiers, or use Gemini API key / Vertex for reliable headless. Multiple GitHub issues Feb–May 2026. Trust/sandbox via config. [established for the error pattern]
- **Grok Build / CLI (0.2.x / current):** `grok -p "prompt" [--output-format json|streaming-json] [--always-approve] [-w worktree] [--device-auth]`. Supports AGENTS.md, worktrees, headless. Requires SuperGrok or X Premium+; device-auth or `XAI_API_KEY`. Quotas subscription-tied. [established for flags]
- **Cursor:** Headless CLI exists (`agent -p --force / --yolo` for edits, `--output-format`). Usable on Pro (with usage limits / `CURSOR_API_KEY` for scripts). Not a pure unlimited subscription CLI like the others; Background Agents also available. Does not unlock standalone Grok Build. [established]

## Synthesis and Implications

The pipeline described in the query is already the emerging best practice among practitioners who run multiple paid subscriptions: Claude Code stays expensive but high-reasoning (plan, contract, verify, integrate); idle capacity on Codex/Gemini/Grok is burned on implementation inside worktrees under tight contracts and sandboxes. Savings are real but come with review overhead and isolation discipline. The main failure modes are underspecified contracts, unbounded round-trips, and insufficient isolation of secrets/vault.

## Limitations and Open Questions

- Almost all quantitative claims are single-stack case studies or secondary analyses; no multi-lab randomized comparison of "Claude-orchestrates-X" vs solo.
- Exact first-pass rates by task class for pure headless external implementers remain thin.
- Windows 11-specific sandbox behavior under concurrent worktrees needs local verification.
- Long-term prompt-injection surface of a large Obsidian vault under repeated RO exposure is under-documented.

New evidence that would raise confidence: a 50–100 task controlled pilot logging tokens/cycles by task type across the three implementers.

## Source Catalog (selected primary sources actually retrieved; 20+ across searches)

- codewithseb.com Multi-CLI Stack, 2026-05-11
- xda-developers.com Claude orchestrating Codex, 2026-07-29
- github.com/awslabs/cli-agent-orchestrator
- github.com/johannesjo/parallel-code
- developers.openai.com/codex/noninteractive + sandbox + AGENTS.md guides (2026)
- gemini-cli GitHub issues/docs on GOOGLE_CLOUD_PROJECT / auth (Jan–May 2026)
- x.ai/docs/build (headless, CLI reference, Jun–Jul 2026)
- cursor.com/docs/cli/headless + overview
- tectontide.com SPEC.md → /goal, 2026-05
- arXiv / Jellyfish / Coding Agent Economy token analyses 2026
- ResearchGate task-stratified PR acceptance 2026
- Multiple DEV.to, Medium, and X posts Feb–Aug 2026 (EloPhanto, practitioners)
- Official Codex best-practices, Grok Build announcement May 2026

## Decision-Oriented Ending — Concrete Options for the Lab

**Option A – Minimal viable (recommended start):** Claude Code + structured contracts + `codex exec --sandbox workspace-write` (and/or `grok -p`) inside auto-created worktrees; 1–2 review cycles max; selective vault allow-list. Trade-off: lowest setup cost, still captures most savings. Risk: residual FS leakage if sandbox misconfigured.

**Option B – Full multi-CLI swarm:** Add gemini-cli + parallel-code or CAO/tmux + DevSquad/Roundtable plugin + multi-model review. Trade-off: higher parallelism and specialization. Risk: auth/ops complexity and attention fragmentation.

**Option C – Keep Claude coding for everything:** Zero orchestration overhead. Trade-off: continues burning the expensive subscription. Risk: no idle-capacity capture.

**Single recommendation [emerging confidence]: Implement Option A immediately for a 1-week instrumented shadow pilot** using the ledger above. Route only scripts/tests/localized refactors first. Flip to production use of the pipeline if ≥30% orchestrator-token savings and rework rate ≤ baseline. Main assumption: the task mix contains a substantial fraction of "grunt" work that headless implementers can complete under tight contracts, and that worktree + `workspace-write` isolation is sufficient for the repo's secret surface. If the pilot shows review cost dominating or high rework on the chosen task classes, fall back to Option C or tighten routing further.

*This matches the documented 2026 practitioner consensus while remaining conservative on unproven quantitative claims.*

**462 sources · Heavy**
