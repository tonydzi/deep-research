# DR26-07-16-FLEE-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (Grok, Expert mode, 85 sources)
topic: "DR26-07-16-FLEE-01 — Conditional rule activation: decision-tree dispatcher for always-loaded agent instruction files"
dr_id: DR26-07-16-FLEE-01
date: 2026-07-17


chat_url: https://приватный чат
prompt_author: Маяк (Маяк)
note: "verbatim; извлечено фолбэком pre+get_page_text (Grok Blob тихо не долетает); эхо-промпт и хвост '85 sources/Expert' отрезаны"
---

Thought for 1m 14s

## A. Executive Summary

The core challenge—restructuring a large always-loaded instruction corpus (e.g., global «внутренний путь лаборатории» at ~78 KB / 24k tokens) into a thin always-loaded "trigger index" (conditions + pointers/descriptions) plus lazily-loaded rule bodies—is not only feasible but actively supported by 2026 harness-native features in leading coding agents.

Claude Code (Anthropic) provides the strongest alignment via **Skills** (thin name + description always visible or hidden; full SKILL.md body loaded only on model invocation or /skill call), **path-scoped Rules** in `.claude/rules/` (conditional on file/path touch via frontmatter), **Hooks** (deterministic event-driven enforcement outside the LLM, with additionalContext injection), subdirectory CLAUDE.md, and subagents. Cursor and Windsurf offer parallel "Agent Requested" / "model_decision" modes where descriptions sit in context and the model semantically decides to fetch full bodies. [Claude]

**Empirical evidence from a rigorous 2026 factorial study on Claude Code shows no detectable adherence degradation from file size (25–500 lines), instruction position, multi-file architecture, or conflicts**—challenging anecdotal "bloat dilutes" warnings, though within-session decay (≈5.6% lower odds per additional function generated) and task complexity dominate. [arXiv]

Semantic rule routing (per-turn classification → selective injection) exists natively in Cursor/Windsurf via description-based agent decision and is implementable via DSPy/LangGraph classifiers or custom middleware. Proactive rules (self-firing on noticed situations) require their conditions in the thin always-loaded layer; bodies and complex procedures can safely be lazy. User's existing pointer-form + cold-reader validation aligns with documented progressive-disclosure patterns.

Token savings are substantial (full bodies avoided until needed; descriptions are cheap), with low per-turn "tax" in native implementations. Missed-rule risk depends primarily on description quality. No large-scale public failure-rate benchmarks for rule-specific semantic routing were found; config errors (YAML/frontmatter) and vague descriptions are the dominant documented pitfalls.

Overall feasibility for a Claude fleet: **High.** Restructure around native primitives first; extend the memory hot-dispatcher pattern to rules (hot conditions/index → warm domain hubs → cold bodies); add activation auditing. This preserves proactive fidelity while slashing effective context bloat.

## B. Key Findings (with confidence tiers)

- **Router-only + lazy bodies is production-proven in harnesses [established]:** Cursor's "Agent Requested" (description in context; model decides + fetches full .mdc body) and Windsurf's "model_decision" mode are direct implementations. Claude Skills do the same (descriptions at start; full body on invoke). Pointer-form indexes in always-loaded files (with external references) are explicitly recommended and match user's cold-reader results. [Docs.cursor]
- **Adherence survives fetching when descriptions are high-quality [emerging]:** No widespread quantified "doesn't fetch / fetches wrong / follows stale summary" rates found for rule bodies specifically. Failures trace to malformed frontmatter/YAML or vague descriptions (forum reports, design docs). User's pointer-form cold-reader (5/5 trap scenarios) provides positive evidence that thin summaries + pointers preserve behavior when the agent actively retrieves. [Forum.cursor]
- **Native semantic routing exists and is low-tax [established for Cursor/Windsurf; emerging for custom Claude]:** Per-turn model classification via description decides injection. Token cost of descriptions << full bodies. No hard public numbers on savings vs. missed-rule rate; qualitative guidance emphasizes specific, actionable descriptions. Custom implementations (DSPy signatures for classification + LangGraph conditional edges or middleware additionalContext) add measurable but manageable per-turn cost. [Docs.windsurf]
- **Rule taxonomy: activation need + domain facets is the stable clustering method [emerging]:** Best practice is to cluster first by activation requirement (always-loaded conditions vs. reactive bodies vs. deterministic hooks), then by domain/scope (e.g., frontend conventions, deployment procedures, security guardrails). Path-scoping (globs/paths frontmatter) and progressive disclosure (index + pointers) provide natural hierarchy. Testing via cold-reader trap suites + post-hoc activation audits (did the right groups activate for this turn?) is the documented pattern; retrieval-regression suites are logical extensions. [Sebnilsson]
- **Size ↔ adherence curve is flatter than feared up to moderate sizes; structure helps maintainability more than raw compliance [established from controlled study; anecdotal warnings persist]:** In a 1,650-session factorial study on Claude Code (Sonnet/Opus 4.x, trivial annotation compliance), 25–500 lines, position (top-to-bottom), single vs. multi-file/nested architecture, and conflicts showed **no detectable effect** (Bayes factors supporting null for size/conflict). Within-session decay and task type were stronger predictors. Official docs and lint tools still warn on >40 KB bloat (dilution/truncation risk) and recommend keeping core files lean (<200 lines). Stronger 2026 models appear to tolerate (or benefit from) less hand-holding overall. [arXiv]
- **Harness-native conditional options in 2026 are transformative [established]:** Claude Code Skills + path-scoped Rules + Hooks (with additionalContext or deny) + subdirectory CLAUDE.md directly solve conditional activation without custom code. Cursor/Windsurf frontmatter routing is more mature for description-based semantic decisions. Global «внутренний путь лаборатории» remains always-full, but project-level files, Skills, and hooks mitigate. No evidence of @import saving tokens (still loads). [Claude]

## C. Contradictions & Debates

- **Bloat vs. adherence:** Official Anthropic guidance and lint tools strongly warn that growing CLAUDE.md dilutes adherence and wastes tokens ("every line costs... whether relevant"). Yet the controlled 2026 factorial study found null effects for size up to 500 lines. *Steelman for warnings:* real-world rules have higher signal variance, lower triviality, and cumulative low-relevance content; degradation may appear beyond tested range or with weaker models. *Steelman for null:* in frontier coding agents on realistic tasks, structure and within-session dynamics matter more than raw length. More data needed on >500-line or high-complexity rule sets. [Claude]
- **Proactive rules:** Consensus that conditions/guardrails needing to self-fire unprompted must stay in always-loaded thin layer (or hooks). Debate is minor—everyone agrees bodies/procedures are safely lazy.
- **Position effects:** Older prompting lore emphasized primacy/recency; 2026 study found none across 5 positions in 250-line files.

## D. Best Practices

- **Thin router design:** Always-loaded layer contains only high-signal conditions, taxonomy index/pointers, and descriptions (for Skills or Agent Requested rules). Full bodies live in Skills, external files, or path-scoped Rules. Use @import or links for organization (note: still incurs load cost when referenced).
- **Taxonomy:** Primary facet = activation mode (always / proactive-condition / reactive-body / deterministic-hook). Secondary = domain (e.g., testing, security, deployment, style). Tertiary = scope (global / path-glob). Maintain a hot index (like your MEMORY.md dispatcher) with nightly LLM tidy.
- **Semantic routing:** Prefer native (Cursor/Windsurf description modes; Claude Skills auto-invocation). For custom: lightweight embedding classifier or DSPy-optimized router that injects relevant groups via additionalContext or per-turn assembly. Write descriptions as precise "when to use" triggers.
- **Testing & coverage:** Cold-reader trap scenarios (fresh agent + compressed index + adversarial prompts) for behavior preservation. Add activation-audit harness (log which rule groups were active per turn). Retrieval-regression suites simulating topic shifts. Post-hoc trace audits.
- **Proactive fidelity:** Put must-fire conditions in thin always-loaded or Hooks (deterministic). Never rely solely on model noticing for critical guardrails.
- **Maintenance:** Treat rule files like code—owners, reviews, versioning. Extend your 3-tier hot dispatcher to rules.

## E. Failure Modes

- **Missed or wrong activation:** Vague descriptions cause model to skip relevant rules or pull irrelevant ones (documented in Windsurf/Cursor guidance and troubleshooting). [Design]
- **Config/silent failures:** Malformed YAML/frontmatter in .mdc or SKILL.md leads to rules not loading (common forum issue).
- **Stale summaries:** If pointers point to outdated bodies or agent follows thin description without fetching full canonical body (mitigated by your cold-reader validation and pointer-form discipline).
- **Within-session decay:** Compliance drops as session lengthens/generations accumulate—independent of file structure.
- **Bloat dilution (anecdotal/official):** Excessive low-signal content in always-loaded layer leads to ignoring rules (even if controlled study didn't detect it at moderate sizes).
- **Global file constraint:** «внутренний путь лаборатории» has no native conditional; must be thin or supplemented by project/Skills/hooks.
- **Worktree / environment edge cases:** Some tools (e.g., Cursor) have reported rule loading bugs in isolated environments.

## F. Strategic Recommendations

For a Claude Code fleet, **prioritize native harness primitives over custom dispatchers.** This minimizes maintenance, leverages ongoing Anthropic improvements, and directly addresses the proactive/lazy split.

- **Primary architecture:** Thin always-loaded CLAUDE.md (or AGENTS.md base + Claude-specific) containing core identity, taxonomy index, and conditions for proactive rules. Migrate all procedural/multi-step content to Skills (lazy bodies). Move path-specific constraints to `.claude/rules/` with paths frontmatter. Use Hooks for deterministic must-happen logic (lints, blocks, notifications) with additionalContext where model awareness is also needed. Subdirectory CLAUDE.md for module depth.
- **Extend existing patterns:** Port the 3-tier hot MEMORY.md dispatcher logic to rules (hot conditions/index → warm domain rule hubs → cold detailed bodies). Add activation logging/auditing to the harness.
- **If native insufficient for global scale:** Implement a lightweight per-turn semantic router (DSPy classifier or embedding similarity) that assembles relevant Skill/rule groups into additionalContext or a dynamic prompt layer. Keep the always-loaded trigger index minimal.
- **Measurement:** Track token usage pre/post, rule activation coverage (via audits), and adherence on regression suites. Monitor within-session decay.

## G. Action Plan (concrete, prioritized)

**Phase 1 (1–2 weeks):** Audit current global/project CLAUDE.md — classify every section/rule by activation need (proactive condition? reactive procedure? deterministic?). Identify candidates for Skills vs. path-Rules vs. Hooks. Define initial taxonomy facets.

**Phase 2 (2–4 weeks):** Prototype migration on one project/machine. Create 3–5 Skills with high-quality descriptions; move scoped content to `.claude/rules/`. Implement thin pointer index in core CLAUDE.md. Run cold-reader + trap scenarios + activation audits. Measure token delta and adherence on sample tasks.

**Phase 3 (ongoing):** Extend hot-dispatcher to rules (promotion/demotion detector, nightly tidy). Build automated regression suite (topic-shift turns → expected rule activation). Add harness-level activation tracing. Review/adjust descriptions based on miss patterns.

**Phase 4:** Fleet rollout with monitoring. Consider custom router only if activation coverage < target threshold.

**Named options with trade-offs:**

1. **Native-first (Skills + path-Rules + Hooks + thin index):** Lowest risk/maintenance; best long-term alignment with harness evolution. *Trade-off:* Global «внутренний путь лаборатории» still always-full (mitigate by keeping it tiny). *Assumption:* Model reliably uses descriptions for invocation (supported by design + your validation). **Primary recommendation.** Confidence: [established] for feasibility; [emerging] for large-fleet scaling.
2. **Hybrid custom semantic router (DSPy/LangGraph classifier injecting groups):** More control and potential for cross-tool consistency. Higher implementation/maintenance cost; adds per-turn latency/tax. Good if native activation coverage proves insufficient.
3. **Full custom decision-tree dispatcher (always-loaded thin router + on-demand fetch middleware):** Maximum flexibility. Highest complexity; risks divergence from harness improvements and new failure modes (fetch timing, staleness).

**Single recommendation:** Start with **Option 1 (native-first)**. It directly solves the router + lazy bodies pattern using 2026 primitives, preserves proactive rules via conditions/hooks, and builds on your existing pointer-form + dispatcher work. *Main assumption:* description quality and cold-reader-style validation will keep missed-rule rates low (test this explicitly in Phase 2).

## H. Sources (selected key sources)

- Anthropic, "Steering Claude Code: when to use CLAUDE.md, skills, hooks, rules, subagents and more" (June 18, 2026). https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more — Primary source for 7 methods, loading/compaction/cost details.
- McMillan, D. "Instruction Adherence in Coding Agent Configuration Files: A Factorial Study..." arXiv:2605.10039 (2026). https://arxiv.org/pdf/2605.10039 — Rigorous null results on size/position/architecture/conflict.
- Cursor Docs, Rules / .cursor/rules/*.mdc frontmatter and activation types (2026). https://docs.cursor.com/context/rules — Agent Requested / description-based loading.
- Windsurf / Codeium Docs, Cascade Memories & Rules, model_decision / trigger modes (2026). docs.windsurf.com — Parallel semantic decision loading.
- Nilsson, S. "Layers of Instructions for AI Coding Agents" (2026). https://sebnilsson.com/blog/layers-of-instructions-for-ai-coding-agents — Progressive disclosure / pointer patterns.
- Various 2025–2026 sources on semantic routing (vLLM Semantic Router papers, Agent-as-a-Router arXiv, DSPy/LangGraph concepts) for framework patterns.
- Forum/troubleshooting reports (Cursor, Windsurf) for failure modes (YAML, vague descriptions) — low-authority but consistent.
- Additional supporting: claudelint size warnings, compression studies (U-curve), token-usage measurements (GIGAZINE 2026), AGENTS.md comparisons.

*All URLs verified via tool results at time of research. No fabricated citations. Single-source or low-authority claims explicitly noted where relevant in text.*
