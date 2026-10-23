# DR26-08-02-ZB-05-2357 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-02-ZB-05-2357
vendor: grok
mode: Heavy (Deep Research)
chat_url: https://приватный чат

collected_at: 2026-08-03T00:40Z
proof_of_work: "433 sources · Heavy" (счётчик Grok в футере ответа); гейт пройден
capture_method: get_page_text (claude-in-chrome) -> ПЕРЕНАБРАНО в markdown (заголовки/таблица восстановлены). НЕ байт-в-байт innerText: содержание и цифры не менялись, форматирование - моя реконструкция. Байт-верный дубль недоступен: blob-download с grok.com заблокирован (скрытая вкладка), fetch на localhost режет CSP
note: тело = только ОТВЕТ Grok; промпт сохранён в реестре/леджере
---

# Cursor (Anysphere) for external implementer role in multi-LLM coding pipeline (2025–2026), plus corporate relationship with X/xAI/Grok

*Grok Heavy, «Thought for 44s», 433 sources.*

Research questions addressed: Ownership status and xAI/X ties; Grok model availability/integration and SuperGrok benefits; headless/CLI/API viability for external orchestrator (Claude Code); power-user agentic practices; comparison vs Codex CLI and Grok CLI as headless implementers; verdict for Windows 11 subscription fleet.

Scope/Date: Prioritized last 12–18 months, with emphasis on 2025–mid-2026 developments through ~3 August 2026. Primary sources opened/retrieved: ≥20 (Reuters, AP, TechCrunch, Guardian, Forbes, official Cursor docs/blogs, Wikipedia, xAI/Cursor X posts, practitioner blogs/forums). X discourse (last ~30 days) for practitioner sentiment.

Overall confidence: High on ownership facts and official product features; medium on precise first-pass success rates/cost-per-task (emerging practitioner data); medium-high on recommendation under stated constraints (subscription preference, Windows, already-wired Codex + Grok CLI).

## 1. Ownership fact-check

[confirmed-deal, pending close] SpaceX signed a binding all-stock agreement on 16 June 2026 to acquire Anysphere (Cursor parent) for $60 billion. Expected close in Q3 2026 (subject to regulatory approval). Cursor becomes a wholly owned subsidiary (via X67 Inc. merger structure). — Reuters

This followed an April 2026 option: SpaceX could buy for $60B later in 2026 or pay $10B for joint work. xAI had already been absorbed/merged into SpaceX in February 2026. Multiple independent high-authority outlets corroborate (Reuters, AP News, TechCrunch, The Guardian, Forbes, CNBC summaries of filings). — AP News

Pre-deal: Anysphere raised to $29.3B valuation (Series D, Nov 2025, Accel/Coatue); was discussing ~$50–60B rounds. ARR reported in the multi-billion range by mid-2026. Wikipedia (updated) and secondary analyses align.

No closed acquisition yet as of early August 2026. Products remain separate operationally for now.

Implications for Grok access/pricing/data: Joint training of Grok 4.5 already occurred (trillions of Cursor interaction tokens + broader data; released jointly 8 July 2026). Post-close: expected tighter model integration, Cursor data feeding xAI/SpaceX models (explicitly noted as a rationale), potential compute access via Colossus, and possible future bundling. Current data policy/pricing unchanged beyond the joint model. Rumors of earlier full merger were overstated; the June agreement is the confirmed deal. No evidence of immediate pricing collapse or forced model exclusivity.

Labeling: Agreement = [confirmed-deal]; full legal close/operational merger = [reported/pending]; earlier pure rumors = [rumor].

## 2. Grok integration

[established] Cursor natively offers Grok models as first-party:

- **Grok Code** (e.g., grok-code-fast-1): cheap ($0.20/M input), 256k context, agent tools, optimized for fast coding.
- **Grok 4.5** (and Fast variant): jointly trained with SpaceX/xAI; released 8 July 2026; available across desktop, web, iOS, CLI, SDK. 256k context (some reports higher effective); priced ~$2/M input / $6/M output (Fast higher); effort levels; strong on long-running tool-use tasks. Part of Cursor Models pool.
- **Plans** (Pro $20/mo, Pro+ $60, Ultra $200, Teams): include "generous" usage of first-party Cursor Models pool (Grok 4.5 + Composer 2.5; permanently doubled around mid-July 2026 for self-serve). Separate "Other Models" pool ($20/$70/$400). Cloud Agents and extended agent limits on paid plans.
- **SuperGrok / X Premium+**: No shared quota, SSO, or automatic access inside Cursor. Separate products/subscriptions. Grok 4.5 in Cursor draws from Cursor's own pools; SuperGrok covers Grok apps + Grok Build CLI. Confirmed repeatedly by @grok (July–Aug 2026). Some future account linking mentioned as work-in-progress, not live. API key BYOK possible but bills separately.

## 3. Headless / CLI lane

[established for existence and basic usability; emerging for production orchestrator robustness]

- Official Cursor CLI (`agent` / `cursor-agent`): install via `curl ... | bash` (macOS/Linux/WSL) or PowerShell `irm 'https://cursor.com/install?win32=true' | iex` (Windows explicit support).
- Headless: `agent -p/--print` (non-interactive); `--force/--yolo` to apply edits; `--output-format text|json|stream-json`; streaming options. Auth: browser login (recommended, local credentials) or `CURSOR_API_KEY` (dashboard-generated, preferred for scripts/CI). Draws from plan usage pools.
- Programmatic: TypeScript SDK (`@cursor/sdk`, `Agent.create/send`, local/cloud/self-hosted workers; Apr 2026). Cloud Agents API for remote VM runs. Background/Cloud Agents: async, isolated Ubuntu VMs, GitHub clone, work on separate branch, push PR + artifacts (videos/screenshots); privacy mode must be off for some; Pro+ required. Can be kicked from CLI (& handoff), web, Slack, etc.
- Isolation: cloud VMs / separate branches / git worktrees. Windows: supported but some community reports of PowerShell subprocess/CPU/EDR friction and process leaks (fixable via WSL or care).
- Quotas/rate limits: plan pools (not public RPM/TPM numbers); heavy agent use can exhaust $20 Other Models or burn first-party quickly; on-demand overage available. No public high-RPM claims for headless batch.
- Usable for external orchestrator (Claude Code CLI) on normal Pro subscription: dispatch prompt → get text/JSON/diff back or PR. Fragility: binary name collision risk with Grok's `agent`; some Windows quirks; less "pure CLI" than Codex/Grok Build; usage metering less predictable than fixed CLI subs.

GUI-only patterns (deeplinks, MCP handoff, workspace) exist but are more fragile for fully automated pipelines.

## 4. Power-user practice (delegated/agentic, not autocomplete)

[established features; emerging consensus on high-leverage workflows]
Strong engineers treat Cursor as an agent runtime, not Tab completer:

- **Rules**: `.cursor/rules/*.mdc` (frontmatter: `alwaysApply`, `globs`, `description` for auto/intelligent/manual) + root `AGENTS.md` (portable, tool-agnostic conventions). Encode style, architecture, test commands, protected paths. Skills for procedural playbooks.
- **Modes/flows**: Plan mode (clarify + written plan) → Agent mode (execute multi-file, tools, terminal, iterate). Ask/Debug for exploration. YOLO/auto-approve for flow. Parallel agents + git worktrees for isolation.
- **Background/Cloud Agents**: Spawn async for refactors, test gen, deps, overnight PRs with self-test + demo artifacts. Cursor internal: cloud agents now produce >30–50%+ of merged PRs.
- **TDD/multi-file loops**: Rules enforce "run tests after changes"; agents explore, edit, fix failures, PR.
- Credible sources (Cursor blogs 2026, practitioner guides on cowork.ink, blink.new, aicoderscope, forum; X threads from known accounts): plan-then-execute, rules-first onboarding, cloud for long tasks, MCP for external tools. Dates 2025–Aug 2026.

Contrarian/dissenting [emerging, low-to-medium authority]: Some engineers drop Cursor for pure terminal agents (Claude Code, etc.) citing usage burn, CLI friction, or preference for non-IDE surfaces. Anecdotes of rogue agents (e.g., destructive actions) exist on X (Aug 2026, mostly anonymous/low-authority). Category error common: treating Cursor as pure autocomplete vs full agent system. X sentiment mixed — praise for multi-file/retrieval + cloud, frustration with limits or Windows quirks.

## 5. Comparison for our case (external orchestrator-driven headless implementer)

| Dimension | Cursor (agent/CLI/cloud) | Codex CLI | Grok CLI / Grok Build |
|---|---|---|---|
| First-pass success | Strong on large multi-file + retrieval/rules [emerging] | High on scoped tasks; cloud sandbox option | Good parallelism; local-first |
| Spec format | Natural language + rules/AGENTS.md preferred | Clear task prompts | Plan/goal modes + config |
| Isolation | Cloud VMs + branches/worktrees (excellent) | Cloud/local options | Local (machine responsibility) |
| Cost (sub) | Pro $20 + pools (Grok first-party generous; can overage) | Subscription/API | SuperGrok ~$30+ (device-auth) |
| Rate limits / headless | Plan pools; usable but less transparent | More predictable for pure CLI | Subscription-oriented |
| Windows | Official support; some friction | Strong | Strong |
| Wins | Custom retrieval, multi-file refactors, cloud PR isolation, rules system | Mature headless, OpenAI ecosystem | Native Grok, parallelism, open harness elements |
| Loses | Headless still editor-oriented; usage predictability; binary collisions | Less integrated retrieval than Cursor | Less cloud isolation; model lock-in |

Cursor clearly wins large multi-file edits with its indexing/retrieval + rules + cloud isolation. Loses pure headless simplicity/maturity and cost predictability vs dedicated CLIs (especially when you already have Grok CLI subscription). Practitioner evidence favors hybrid: use Cursor for complex delegated tasks needing context, pure CLIs for simple/fast loops.

## 6. Verdict / Recommendation

Concrete options:

1. **Implementer #3** (in addition to Grok CLI + Codex CLI): Dispatch complex multi-file/spec-heavy tasks to Cursor CLI/SDK/cloud agents. Trade-off: extra subscription cost + learning, risk of usage overages; gain superior retrieval/isolation/PR handoff.
2. **Replacement for Grok CLI**: Unlikely net win — Grok CLI already subscription-aligned, native, and lighter; Cursor adds overhead without fully replacing Grok strengths.
3. **GUI-only helper for human**: Use desktop for review/oversight of Claude-orchestrated diffs; minimal automation. Low risk, under-utilizes agent features.
4. **Skip**: If pure headless + fixed costs dominate and multi-file retrieval not critical.

**Single recommendation [medium-high confidence]: Add Cursor as implementer #3 (Pro subscription)** for the subset of tasks that benefit from its custom retrieval, rules system, and cloud isolation/PR output. Keep Grok CLI primary for Grok-model work (already subscribed, device-auth) and Codex for its strengths. Use Cursor desktop opportunistically for human review.

Main assumption: Your multi-file/complex tasks occur frequently enough to justify the extra sub and orchestration glue; Windows CLI friction is manageable (or WSL used); deal close does not immediately alter pricing/quotas adversely. Evidence would change this if headless success rates prove lower than pure CLIs on your workload or if SuperGrok/Cursor bundling lands soon.

Limitations/Open questions: Exact first-pass rates and cost-per-task are practitioner-reported rather than controlled benchmarks [emerging]. Deal close timing/regulatory outcomes unknown. Usage metering opacity remains. No single source claims universal superiority.

## Source catalog (key primary, with dates; all retrieved)

- Reuters (16 Jun 2026): SpaceX $60B deal.
- AP, TechCrunch, Guardian, Forbes (16 Jun 2026): corroborating coverage.
- Wikipedia Anysphere/Cursor (updated ~2026): ownership/funding timeline.
- cursor.com/blog/grok-4-5 (8 Jul 2026); docs/models/grok-code, models-and-pricing, cli/headless, authentication, rules, cloud-agent posts (2026).
- cursor.com/blog/typescript-sdk (29 Apr 2026); agent-computer-use (24 Feb 2026).
- @grok X posts (Jul–Aug 2026): SuperGrok/Cursor separation, integration status.
- Practitioner: cowork.ink (Aug 2026), blink.new, aicoderscope, Cursor forum, etc. (2026).
- X keyword/semantic results (Jul–Aug 2026) for sentiment/workflows.

Vendor marketing (Cursor/xAI blogs) distinguished from independent reporting and practitioner X/forums. No fabricated sources. Remaining gaps (precise internal benchmarks, final deal close) noted explicitly.

**433 sources · Heavy**
