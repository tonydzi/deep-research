# DR26-07-05-HUB-03 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!--
source: Grok (SuperGrok, Expert mode), grok.com
DR: DR26-07-05-HUB-03 — Repairability-first multi-agent systems: cheap models as builders and repairers
date: 2026-07-05


url: https://приватный чат
note: Grok UI no longer has a separate DeepSearch toggle; run in Expert mode with explicit deep-research instruction. "Thought for 1m 19s", 80 sources. Verbatim below.
-->

# DR26-07-05-HUB-03 — Grok report (verbatim)

A. Executive Summary

Repairability-first multi-agent LLM systems prioritize artifacts (code, prompts, workflows) that the weakest viable model—even cheap/local 7-14B-class models on modest hardware—can diagnose, debug, and repair. This aligns with the founder's hypothesis (schoolkids build hammer-repairable axes; PhDs build PhD-only spaceships) but faces strong counter-evidence: cheap builders frequently produce messy/spaghetti artifacts or "invisible technical debt" when unconstrained; design errors dominate cost; and real-world repair success (e.g., SWE-bench class) hovers at 24-35% even for frontier models, with rapid decay in iterative debugging. [arXiv]

State-of-the-art (2025-2026) centers on heterogeneous multi-agent systems (MAS) with dynamic routing/cascades (MasRouter, RouteLLM/FrugalGPT, Iterative Critique-and-Routing Controller) and emerging self-healing loops (multi-agent failure detection + replanning + corrective prompting). These deliver 30-52% inference cost reductions while improving reliability via verification/re-routing "repair" mechanisms. Small/coder-specialized models (Qwen2.5/3-Coder series, DeepSeek coder variants) have advanced dramatically, enabling capable local workers/repairers. [Aclanthology]

Core tension: Simplicity and repairability are driven far more by strict specifications and constraints ("stdlib-only, one file or clear modules, full tests/docstrings, explicit error handling, no unnecessary abstractions") than by builder model size. A top-tier model with weak specs often yields worse maintainability than a cheap model with ironclad constraints. "Repair drills" (deliberate fault injection + cheap-model recovery measurement) emerge as a practical quality gate, echoing chaos engineering for LLM-MAS. [arXiv]

For a 2-person company running always-on agent fleets on subscription budgets, the winning pattern is frontier/expensive for design, architecture review, and complex synthesis + cheap/local + router for build/execution/repair + self-healing observability layer. This minimizes both build cost and lifetime maintenance while validating the "weakest-model-can-repair-it" constraint.

B. Key Findings

Academic Research

- Heterogeneous MAS routing & cascades: MasRouter (ACL 2025) formalizes Multi-Agent System Routing (MASR): dynamically selects collaboration mode (chain/tree/graph/debate), agent roles, and specific LLM per query/role. Cascaded controller network yields +18.82% on MBPP and up to 52% inference overhead reduction vs. homogeneous SOTA; inductive to unseen models/LLMs; modular/plug-and-play. [Aclanthology]
- Multi-turn critique + re-routing as repair: 2026 arXiv controller (heterogeneous pool e.g. Qwen 1.5B/7B/30B) uses GRPO-trained policy for verification, feedback, and re-routing to stronger agent on failure. +11-16 pp accuracy over single-shot routers; strong model used <25% of time under constraints; explicit recovery mechanism via re-routing. Case studies show weak agent draft → error detection → strong repair. [arXiv]
- Single-LLM routing foundations: RouteLLM (preference/matrix factorization) and FrugalGPT (cascades) achieve ~50%+ cost reduction at near-parity quality by routing simple queries cheap-first and escalating on confidence/threshold. [Augmentcode]
- Self-healing frameworks: Multiple 2026 works propose multi-agent closed loops (failure detection, reliability assessment, replanning, corrective prompting, trace analysis for prompt optimization). Reliability-aware recovery; IaC drift remediation examples show high detection rates and low MTTR. [arXiv]
- Code complexity/maintainability vs. model capability: No strong causal evidence that smaller builders produce inherently simpler/more maintainable output. Real-world code generation/repair (SWE-bench, class-level tasks) shows massive gaps vs. synthetic (24-35% vs. 84-89%). Debugging effectiveness decays exponentially (60-80% loss after 2-3 attempts for most models). AI-generated code introduces distinct "machine signature" technical debt, comprehension/cognitive debt, and "invisible debt" (works but black-box to humans/LLMs). Iterative LLM code quality can degrade over time. Strict prompts/specs matter more than raw model size. [arXiv]
- Repair success by model size: Limited direct head-to-heads; frontier models dominate SWE-bench leaders. Smaller models excel at narrow/simple tasks or as first-pass repairers when guided; decay and real-world robustness remain challenges across sizes.

Industry Practice

- Hierarchical splits are common and recommended: cheap/fast models (e.g., Claude Haiku-class) for routing/orchestration; capable models for specialists/workers. Routing errors cascade badly in agent graphs, so end-to-end workflow optimization > per-call routing. [Builder.aws]
- Local/small model adoption: Strong 2025-2026 progress in Qwen2.5/3-Coder (especially MoE variants optimized for agents, long context, tool use, recovery), DeepSeek coder models. Viable on 8-16 GB VRAM for coding agents; popular in open-source communities for cost/privacy/control. Production use often hybrid (local for routine + cloud frontier for hard cases).
- Cost-of-ownership data: Inference savings from routing/cascades well-documented (30-52%). Lifetime maintenance costs dominated by technical/comprehension debt from unconstrained generation. No comprehensive public TCO studies by builder tier, but anecdotes warn of debt explosion when cheap models used without constraints or verification.
- Repairability/chaos-style gates: Emerging. Academic chaos engineering frameworks for LLM-MAS (fault injection: crashes, delays, state corruption). NTT LLM-automated chaos; practitioner tools adapting resilience testing to agents. "Repair drill" concept is novel but directly supported by self-healing and critique-re-route literature. [arXiv]

Competitive Landscape

- Open-source: RouteLLM (lm-sys), LiteLLM (proxy + routing/fallbacks/policies), MasRouter (MAS-specific cascaded routing).
- Commercial/SaaS: Not Diamond (meta-router + automatic prompt adaptation), Martian (interpretability-informed gateway), AWS Bedrock Intelligent Prompt Routing (built-in for Claude/Llama families, ~30% savings).
- Focus shifting to agent workflows (cascading errors) and self-healing/observability layers. Substitutes include pure self-hosted small-model stacks (Ollama + LangGraph/CrewAI) or hybrid.

Failure Modes

- Cheap-model quality collapse: Unconstrained generation produces spaghetti, silent bugs, missing error handling/observability (the "80% problem"). Debt compounds; later repair harder for both humans and LLMs.
- Frontier-only economics: Unsustainable token costs for high-volume or simple tasks; overkill for many components.
- Maintenance hell in agent systems: Prompt drift, eval gaps, silent failures, comprehension debt. Many early multi-agent projects abandoned or require heavy ongoing human intervention.
- Router/controller failures: Single bad routing decision cascades; poor calibration leads to over-use of expensive models or quality drops.
- Iterative degradation: LLM-generated code quality can worsen over generations without strong verification/repair loops.

Emerging Trends (≈ last 12 months to mid-2026)

- Learned dynamic routers for full MAS (collab mode + roles + LLM tiers).
- Multi-turn critique/re-route controllers enabling built-in recovery.
- Self-healing multi-agent systems (trace analysis → prompt/system optimization; closed-loop remediation).
- Strong small/coder MoE models (Qwen3-Coder variants explicitly agent/recovery-focused) making local repairers practical.
- Chaos engineering adapted to LLM-MAS for proactive resilience.
- Hybrid cascades + routing; graceful degradation to local models.

C. Contradictions & Debates

- Hypothesis vs. evidence: "Cheap builders → simpler artifacts" lacks direct support. Counter-evidence stronger: complexity driven by spec/prompt constraints and task nature; cheap models can produce more debt if unconstrained; design errors (frontier strength) are costlier long-term than implementation grunt.
- Repair by weakest vs. escalation: Multi-turn controllers show value in starting cheap/weak then escalating for repair. Pure "weakest fixes everything" is aspirational; practical systems use tiered recovery.
- Speed vs. maintainability: AI agents ship fast (80% functional) but the missing 20% (robustness, docs, observability) creates disproportionate debt. Heavy AI coding shifts toil from writing to debt management.
- Homogeneous vs. heterogeneous: Homogeneous simpler to manage but misses specialization gains (8-47% in some studies). Heterogeneous wins on perf/cost but adds routing complexity.
- Self-healing maturity: Promising research/prototypes; production adoption still early, reliant on good observability.

D. Best Practices

- Constraints first, model tier second: Enforce simplicity for every builder (stdlib/minimal deps, single-responsibility modules or one-file where possible, comprehensive tests + docstrings, explicit error handling, no magic, measurable outputs). This makes repair feasible regardless of builder IQ.
- Tiered architecture with routing:
  - Expensive/frontier: High-level design, architecture, complex synthesis, blueprint review, final quality gate.
  - Cheap/local: Grunt implementation, routine tasks, first-pass repair/debug, high-volume execution.
  - Router/controller (cheap or small model): Orchestration, verification, initial routing, simple critique.
- Verification & repair loops: "Dumb detector first, LLM as judge." Multi-turn critique + re-route or dedicated self-heal multi-agent layer.
- Observability as foundation: Replay logs, traces, checkpoints for self-healing and drills.
- Repair drill as quality gate: Systematically break components (code mutation, bad inputs, simulated failures, dependency removal) → task target cheap/local model to diagnose/fix/restore passing tests. Measure autonomous success rate, iterations, human touch-ups. Pass threshold (e.g., ≥60-70% fully autonomous) before promoting component.
- Metrics beyond pass@k: Maintainability index, cyclomatic complexity, debt audits, repair success over iterations, TCO (tokens + engineer hours).

E. Failure Modes (detailed above)

F. Strategic Recommendations

- Adopt "frontier designs simple blueprint; cheap models build & repair under strict constraints + router + self-heal layer" as the dominant pattern. This directly addresses internal counter-arguments while validating the repairability hypothesis via drills.
- Treat repairability as a first-class non-functional requirement equivalent to performance/cost.
- Invest in constraints/prompt libraries and router/controller infrastructure before scaling fleets.
- Use open-source routing (RouteLLM/LiteLLM/MasRouter) or commercial (Not Diamond, Bedrock) as accelerators; evolve toward learned MAS routers.
- Pilot self-healing multi-agent monitor on critical paths.
- For always-on fleets: Hybrid API (Claude for design/thinking) + local/small for volume/repair to control subscription burn.

G. Action Plan (2-person company, subscription budgets)

Phase 1 (0-4 weeks – Foundation)
- Codify strict simplicity constraints + reusable prompt templates (one-file where feasible, tests mandatory, etc.).
- Implement basic router (LiteLLM or custom cheap classifier) + tier split (Claude Opus/Sonnet for design → Haiku/local for build).
- Instrument observability (logs, traces, replay).

Phase 2 (4-8 weeks – Validation Gate)
- Build automated repair-drill harness: fault injection scripts + cheap-model repair task + metrics (success %, iterations, diff size, tests restored).
- Run drills on existing components; simplify any that fail threshold.
- Add lightweight critique/re-route or self-heal monitor (cheap model analyzing traces → suggest/patch fixes).

Phase 3 (8-12 weeks – Scale & Optimize)
- Evolve to MasRouter-style or equivalent learned routing for collab/roles/tiers.
- Integrate chaos-engineering-style experiments into CI or staging.
- Full TCO dashboard (tokens by tier + engineer hours on debt/repair).
- Pilot fully local repairer tier (Qwen3-Coder or equivalent on available hardware).

Ongoing Priorities
- Weekly repair-drill reviews as quality gate.
- Monthly debt audit + simplification sprints.
- Quarterly router/self-heal model upgrades (small models improving fast).
- Budget guardrails: cap frontier usage via router constraints; measure ROI of drills (reduced future maintenance).

This roadmap keeps the system simple enough for cheap/local repair while leveraging frontier intelligence where it matters most—design.

H. Sources (selected; full traces via tool results)

Key Papers
- MasRouter: Learning to Route LLMs for Multi-Agent System (ACL 2025) — https://aclanthology.org/2025.acl-long.757.pdf
- Iterative Critique-and-Routing Controller for Multi-Agent LLM Systems with Heterogeneous LLMs (arXiv 2026) — https://arxiv.org/html/2605.08686v1
- RouteLLM and FrugalGPT foundational routing papers.
- Self-Healing Framework for Reliable LLM-Based Autonomous Agents (arXiv 2026) and related.
- AI-Generated Smells: Technical debt audit in LLM-generated code (arXiv 2026).
- Chaos engineering for LLM-MAS robustness (arXiv 2025).

Frameworks & Tools
- RouteLLM (lm-sys), LiteLLM, MasRouter (GitHub).
- Not Diamond, Martian, AWS Bedrock Intelligent Prompt Routing.
- Self-healing examples: Adaptive multi-agent optimization frameworks; Union.ai replay/checkpointing patterns.

Industry & Practitioner
- AWS blogs on hierarchical agent architectures and multi-LLM routing.
- SWE-bench analyses and real-world code gen gaps.
- Community discussions (Reddit r/LocalLLaMA, HN) on Qwen/DeepSeek coder progress and local agent workflows 2025-2026.
- Articles on comprehension/cognitive/invisible technical debt from AI-generated code (Addy Osmani, etc.).

All claims grounded in 2025-2026 sources retrieved via extensive search. The field is moving extremely fast—small model capabilities and routing/self-healing techniques are the highest-leverage areas to watch and adopt.

80 sources
