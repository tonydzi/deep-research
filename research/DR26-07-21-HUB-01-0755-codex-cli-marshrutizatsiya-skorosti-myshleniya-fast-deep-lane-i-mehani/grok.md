# DR26-07-21-HUB-01-0755 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: Grok (grok.com, SuperGrok Heavy), chat https://приватный чат
topic: "DR26-07-21-HUB-01-0755 — Codex CLI thinking-speed routing + Max subscription quota mechanics"
date_collected: 2026-07-21


note: verbatim extract via get_page_text; прогон запущен ранее 21.07 не этой сессией (промпт = чат-блок версия без GROK-SPECIFIC add-on)
---

# Grok report — DR26-07-21-HUB-01-0755 (verbatim)

Structured Report: OpenAI Codex CLI Thinking-Speed Routing & ChatGPT Max/Pro Quota Mechanics (as of 21 July 2026)

Primary sources prioritized: official OpenAI docs (learn.chatgpt.com / developers.openai.com), pricing/help pages, API reasoning guide, non-interactive mode docs, config reference, and ChatGPT Terms of Use. Community/GitHub/forum reports used only for real-world ratios, failure modes, and un-published multipliers (explicitly marked). "ChatGPT Max" is treated as the highest individual tier (officially "Pro" with 5× or 20× multipliers relative to Plus; community often calls the top tier Max / Pro 20×). Model context: gpt-5.6-sol (and family) with reasoning effort levels.

## 1. Quota Mechanics

How it meters (established, high confidence)
Subscription (ChatGPT-managed auth) usage is hybrid but primarily message/task-count + complexity-based within rolling windows, not pure open-ended tokens like the paid API.

- Hard limits appear as adaptive ranges of "Local Messages / 5 h" (and Cloud Tasks / Code Reviews where available). Example for Plus: GPT-5.6 Sol 15–90 local messages per 5-hour window (ranges are load-adaptive; lower bound is the reliable floor). [Chatgpt]
- Pro (the Max-equivalent tier) is officially "5× or 20× more usage than Plus" + "Maximum Codex tasks". [Chatgpt]
- Additional weekly limits exist on top of the 5-hour windows (widely reported and visible in usage dashboards; exact numbers vary and are not published as fixed public constants).
- Usage is shared across Codex (CLI/IDE/App), ChatGPT Work, Excel agents, etc.
- After plan limits, users can buy credits. Credits are token-based (input / cached-input / output). Reasoning tokens count as output tokens. [Developers.openai +1]

Impact of reasoning effort (established + reported)
Higher model_reasoning_effort (especially xhigh / max) generates substantially more internal reasoning tokens → higher output-token / credit burn and faster consumption of the 5-hour and weekly budgets. Official statement: "Lower reasoning effort favors speed and lower token usage, while higher effort lets the model think more completely."
Community proxies (reasoning-time analysis on older GPT-5.x): Plus ≈ ~40 min of heavy reasoning per 5 h window; Pro 5×/10×-promo scales that roughly linearly. xhigh/plan-mode runs are repeatedly reported to drain the window much faster than medium. Exact multiplier is task-dependent (can be several-fold for hard agentic work); no official fixed "xhigh = N× medium" table exists. Fast/service-tier modes further multiply credit use (~2–2.5× reported). [Developers.openai]

Dashboard: chatgpt.com/codex/settings/usage; inside CLI use /status. Confidence: high on structure and effort → token relationship; medium on precise numerical multipliers for current Max/Pro 20×.

## 2. Per-Call Effort Control

Supported mechanisms (established)

- User/project config: ~/.codex/config.toml or project .codex/config.toml
```toml
model = "gpt-5.6-sol"
model_reasoning_effort = "xhigh" # minimal | low | medium | high | xhigh | max (model-dependent)
```
- Profiles (cleanest for side-by-side lanes): separate files ~/.codex/fast.config.toml, ~/.codex/deep.config.toml (or any name). Selected with --profile name / -p name. Profiles load after project config but before user defaults. [Learn.chatgpt +1]
- One-off CLI overrides (highest precedence):
  codex -c 'model_reasoning_effort="medium"'
  or multiple -c flags. Also works with codex exec.
- Interactive: /model slash command.
- Hierarchy (highest → lowest): CLI flags/-c > project .codex/ (trusted only) > profile > user config.toml > system/defaults. [Learn.chatgpt]

Two-lane cleanliness: Yes. One installation cleanly supports fast (medium/low) and deep (xhigh) lanes via profiles or per-invocation -c. Gotchas: project-level overrides can surprise; older CLI versions rejected "xhigh" (0.144.x supports it); Windows path / quoting for -c TOML strings; some settings (auth, provider) cannot live only in profiles.

Sources: official config-reference, config-basic, sample-config, non-interactive docs.

## 3. Latency vs Quality

Published / credible data (established official guidance + reported community/eval)
Official guidance (API reasoning & latest-model docs):

- medium = balanced default / good starting point for most agentic coding.
- high = complex debugging, multi-step planning, high-value work.
- xhigh = deep research, long agentic rollouts, security/code review, challenging coding; "use only when evals show a clear benefit that justifies the extra latency and cost."
- max = even higher for the hardest quality-first workloads; compare vs xhigh. [Developers.openai +1]

Reasoning tokens are invisible but billed as output and consume context; higher effort → longer wall-clock and higher quota cost. Adaptive behavior exists inside a given effort level.

Community / independent evals (GPT-5 era and early 5.x):

- High effort produced large coding-score lifts vs medium on difficult tasks (e.g., one suite: 7.71 → 8.86 average; some hard tasks went from failure to strong solutions). [Eval.16x]
- Later models / research tasks sometimes show diminishing or even negative returns from higher effort (over-thinking).
- For architecture / code-review style work: medium or high is frequently sufficient (humans edit anyway); xhigh is materially better for novel/high-stakes/deep architectural analysis or security but overkill for routine PR sanity checks.

Diminishing returns are real beyond high for many review tasks; xhigh's value is clearest when the cost of missing a subtle issue is high and the run is asynchronous. Confidence: high on official recommendations and directional effects; medium on exact numerical deltas for gpt-5.6-sol architecture reviews (no large public head-to-head published by OpenAI).

## 4. Adaptive Routing Patterns

Practitioner / research patterns 2025–2026 (reported / established in agent literature)
Common triage heuristics:

- Task type / complexity signals (simple edit / one-move sanity vs multi-file architecture / security / long-horizon).
- Reversibility / blast radius / error cost (high effort if hard to undo or production/security impact).
- Human waiting (interactive 90 s budget → low/medium; offline retrospective → high/xhigh).
- History / ambiguity / tool-use intensity / confidence signals.

Patterns: static profiles (fast/deep lanes), lightweight classifier/router that picks min sufficient effort, cascade (try medium → escalate on low confidence), multi-agent graphs (planner at high effort, executors at lower). Research example (Ares, Mar 2026): adaptive per-step effort selection can save ~50 %+ tokens with minimal success degradation.

Failure modes: over-thinking simple tasks (quota/latency waste, sometimes worse answers); under-thinking critical ones (missed bugs, incomplete reviews); oscillation or always-max policies that burn budget. Confidence: high that these heuristics are widely used; medium on any single "best" router.

## 5. Unattended Use / ToS

Current position (established docs + ToU reading)
Official non-interactive docs explicitly endorse codex exec for pipelines, CI, pre-merge checks, and scheduled jobs. Auth-refresh guidance exists for trusted CI runners using ChatGPT-managed (subscription) auth. [Learn.chatgpt +1]

ChatGPT Terms of Use prohibit automated/programmatic extraction of data/output and circumvention of rate limits or protective measures.

Practical reading: Legitimate headless / scheduled use of the intended CLI interface is promoted and tolerated. Heavy unattended cron that looks like mass scraping, limit circumvention, or abuse is restricted and carries account risk. No widely documented ban waves specifically for normal background Codex CLI automation under a paid subscription were found; risk is higher if usage patterns trigger abuse systems. Confidence: high that intended headless is supported; medium that pure high-volume unattended is "safe forever."

## 6. Timeout Practice for Long Headless Runs

Recommended practices & known failure modes (reported, high confidence on issues)

- No public long-thinking timeout config inside Codex itself for arbitrary agent runs. Use external supervisors (Windows timeout, process monitors, systemd, custom watchdogs).
- Streaming: progress on stderr, final answer on stdout; --json (JSONL events: thread.started, turn., item., error) is the reliable way to monitor progress and detect stalls.
- Resume: codex exec resume --last or codex exec resume <SESSION_ID> (also interactive codex resume). Sessions live under ~/.codex/. --ephemeral avoids persistence for one-shots.
- Known failure modes: silent hangs on dead connections (especially Windows), stdin-pipe hangs in headless, occasional resume hangs, fixed internal timeouts on some review sub-tasks (~120 s warning / 300 s stop reported), partial output on disconnect.

Practices: wrap long runs with external timeout + retry-with-resume logic; always prefer --json + watchdog for 10–30 min thinking; monitor quota via /status or dashboard; test resume path before relying on it for retrospectives. Confidence: high on documented resume and reported hangs; medium on exact best external supervisor recipe.

## Recommended Two-Lane Configuration (for your Windows 11 / 0.144.x / Max-or-Pro / gpt-5.6-sol setup)

Create two profiles:

```toml
# ~/.codex/fast.config.toml
model = "gpt-5.6-sol"
model_reasoning_effort = "medium" # or "low" for absolute speed
# optional: model_verbosity = "low"
```
```toml
# ~/.codex/deep.config.toml
model = "gpt-5.6-sol"
model_reasoning_effort = "xhigh"
# optional: model_reasoning_summary = "detailed"
```

Usage

- Fast lane (90 s sanity / one-move checks, human waiting):
  codex exec --profile fast "…prompt…"
  or codex exec -c 'model_reasoning_effort="medium"' …
- Deep lane (architecture / retrospective, 10–30+ min):
  codex exec --profile deep --json "…prompt…"
  Wrap with external timeout (e.g. 40 min) + resume-on-failure logic. Monitor with /status or the usage dashboard.

Keep default user config at medium. Escalate only when task signals (architecture keywords, high blast radius, offline) justify it. Prefer medium for the majority of live work to stay inside Max/Pro limits; reserve xhigh for high-value offline reviews. Always check remaining quota before long deep runs.

## Uncertainties / caveats

- Exact current Max/Pro 20× numerical limits and weekly numbers are adaptive and not published as fixed constants.
- Precise xhigh vs high token multiplier is task- and model-version-dependent.
- "Max" naming is community/colloquial; official is Pro with multipliers.
- ToS risk for pure unattended is low for intended use but non-zero if patterns look abusive.

Sources for the above are the official pages and docs cited throughout (pricing, config reference, non-interactive mode, reasoning guide, latest-model guide, help articles, Terms of Use) plus community reports explicitly marked. All claims are current as of the July 2026 crawl dates of the cited pages.

376 sources · Heavy
