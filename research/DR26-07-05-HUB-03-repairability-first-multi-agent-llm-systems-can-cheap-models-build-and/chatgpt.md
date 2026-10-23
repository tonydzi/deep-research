# DR26-07-05-HUB-03 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Repairability-first multi-agent systems"
type: ai-conversation
stage: raw
source: chatgpt


authored_by: hybrid
conversation_id: 6a4a932e-50fc-83ed-87f4-fc172d02ef40
model: "gpt-5-5-pro"
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
msg_count: 29
related_concepts: []
revisit_if: ""
---

# Repairability-first multi-agent systems

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 29 сообщений

## Диалог

**Anton:**

@Deep research # DR26-07-05-HUB-03 — Repairability-first multi-agent systems: cheap models as builders and repairers

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-05-HUB-03 \u2014 Repairability-first multi-agent systems: cheap models as builders and repairers"}}
```

---

**Anton:**

Additional context and full research requirements for this deep research run (the first message was only the title; use ALL of the below to steer the research):

CONTEXT (what we already know — do not re-derive, dig DEEPER):
- We build multi-agent AI systems (orchestrator + workers) for a small business on Claude models (Haiku/Sonnet/Opus tiers).
- Our working hypothesis (founder's): don't chase top-tier models; a system built by cheap models is simpler and can be REPAIRED by the cheapest/local model (even a free local 8GB-VRAM model). Analogy: schoolkids build axe-simple things repairable with a hammer; PhDs build spaceships only PhDs can fix.
- Our counter-arguments (internal review): (a) cheap builders often produce MESSY, not simple, artifacts (spaghetti, silent bugs); (b) simplicity is driven by the SPEC/constraints, not the builder's IQ — a top model with a strict "stdlib-only, one file, no abstractions" spec yields simpler artifacts than a cheap model without constraints; (c) design errors are costlier than grunt errors, so "expensive model designs a SIMPLE blueprint, cheap models build and repair" may dominate.
- Our current practice already: deterministic code first (SQL/grep/scripts, zero LLM), cheap model for grunt work, expensive model for thinking/synthesis; "dumb detector first, LLM as judge".
- Proposed mechanism to validate: a "repair drill" quality gate — deliberately break a component and check whether a CHEAP/local model can fix it; if not, the system is too complex and must be simplified.

RESEARCH REQUIREMENTS:

1. Academic research
- papers, meta analyses, benchmarks on: code complexity/maintainability vs model capability; multi-agent systems with heterogeneous model tiers; "LLM repairing/debugging code written by another LLM" success rates by model size
- any evidence for or against "smaller model output is simpler to maintain"

2. Industry practice
- startups, unicorns, leading companies, open source projects using cheap/local models as workers or maintainers (orchestrator-worker splits, model cascades, router patterns)
- documented cost-of-ownership data: build cost vs lifetime maintenance cost of agent systems by model tier
- anyone using a "can a weak model fix it?" or similar repairability gate, chaos-engineering-style drills for agent systems

3. Competitive landscape
- frameworks/platforms that formalize model-tier routing and graceful degradation to local models (direct competitors, indirect, substitutes)

4. Failure analysis
- failed attempts: products killed by betting on cheap models (quality collapse), and products killed by frontier-model unit economics
- abandoned projects and lessons learned about maintainability of LLM-generated codebases

5. Emerging trends
- last 12 months: small-model progress (local 7-14B class), distillation, cascades, "frontier designs / small executes" patterns, self-healing agent systems

6. Implementation recommendations
- actionable framework: how to decide per-component which tier builds, which tier repairs
- how to specify constraints so ANY tier produces simple, repairable artifacts
- a concrete repair-drill protocol (how to break, what to measure, pass/fail criteria)
- roadmap and priorities for a 2-person company running always-on agent fleets on subscription budgets

OUTPUT FORMAT:

A. Executive Summary
B. Key Findings
C. Contradictions & Debates
D. Best Practices
E. Failure Modes
F. Strategic Recommendations
G. Action Plan
H. Sources

---

**Anton:**

Please output the FULL final deep research report as a regular chat message, verbatim, in markdown (all sections A-H including Sources). Do not summarize or shorten. If it is too long for one message, output part 1 now and I will say "continue" for the rest.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LLM code generation maintainability complexity model size benchmark paper"},{"q":"LLM generated code maintainability complexity empirical study"},{"q":"large language models code repair debugging success rates model size SWE-bench small models"},{"q":"LLM code repair generated by LLM empirical study"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"SWE-bench Verified leaderboard model size Claude Haiku Sonnet 2026"},{"q":"SWE-bench Lite small language models code repair benchmark 7B 14B"},{"q":"SWE-agent paper SWE-bench code repair LLM agent"},{"q":"Agentless SWE-bench paper code repair LLM"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:anthropic.com/news Claude Haiku 4.5 multi-agent coding workers one third cost Sonnet"},{"q":"site:anthropic.com/news Claude Sonnet 4.5 SWE-bench Verified agents computer use"},{"q":"site:anthropic.com/news Claude Opus 4.5 pricing coding agents 2025"},{"q":"site:anthropic.com/pricing Claude Haiku Sonnet Opus pricing API 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

I’ll reconstruct the full report directly in chat from fresh, cited evidence, covering academic results, routing/cascade practice, failure cases, and an implementable repair-drill protocol.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Anthropic Claude Haiku 4.5 coding agents one third cost Sonnet 4.5"},{"q":"Anthropic Claude Sonnet 4.5 SWE-bench Verified 82.0 OSWorld agents"},{"q":"Anthropic Claude Opus 4.5 price SWE-bench 2025"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Anthropic docs Claude pricing input output tokens Haiku Sonnet Opus official"},{"q":"Anthropic model pricing docs Claude 4 Haiku 4.5 Sonnet 5 Opus 4.8"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"https://www.anthropic.com/news/claude-3-5-sonnet"},{"ref_id":"https://www.anthropic.com/news/claude-3-5-haiku"},{"ref_id":"https://www.anthropic.com/news/claude-3-7-sonnet"},{"ref_id":"https://www.anthropic.com/news/claude-4"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"https://docs.anthropic.com/en/docs/about-claude/models/overview"},{"ref_id":"https://docs.anthropic.com/en/docs/about-claude/pricing"},{"ref_id":"https://www.anthropic.com/news/claude-sonnet-5"},{"ref_id":"https://www.anthropic.com/news/claude-haiku-4-5"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn691743view1","lineno":148},{"ref_id":"turn691743view2","lineno":123},{"ref_id":"turn691743view3","lineno":123},{"ref_id":"turn691743view3","lineno":139}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn691743view3","lineno":20},{"ref_id":"turn691743view3","lineno":50},{"ref_id":"turn691743view2","lineno":35},{"ref_id":"turn691743view2","lineno":80}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"https://www.swebench.com/"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"FrugalGPT LLM cascade model routing paper cost accuracy"},{"q":"RouteLLM learning to route LLMs open source paper 2024"},{"q":"LLM router model cascade cost performance commercial model selection framework paper"},{"q":"small language model cascade large language model cost efficient routing enterprise"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"RouteLLM learning to route LLMs GitHub UC Berkeley LMSYS 2024"},{"q":"Martian Model Router LLM routing benchmarks cost performance"},{"q":"OpenRouter model routing automatic fallback LLM providers docs"},{"q":"LangChain model routing fallback cheap model tool calling docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"OpenAI model router cost model cascade routing agent platform docs fallback local model"},{"q":"LangChain fallbacks model routing docs LLM router"},{"q":"LiteLLM model routing fallback budget docs"},{"q":"Haystack LLM router model fallback pipeline docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:docs.litellm.ai router fallbacks budget model routing LiteLLM"},{"q":"site:python.langchain.com fallback chat model router runnable fallbacks docs"},{"q":"site:docs.llamaindex.ai router query engine model selector LLM routing docs"},{"q":"site:portkey.ai/docs gateway model fallback load balancing routing"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LiteLLM router fallbacks budget model routing docs"},{"q":"LangChain fallbacks chat models docs"},{"q":"LlamaIndex router query engine selector docs"},{"q":"Portkey AI gateway model fallback load balancing routing docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"https://docs.litellm.ai/docs/routing"},{"ref_id":"https://docs.litellm.ai/docs/proxy/reliability"},{"ref_id":"https://python.langchain.com/docs/how_to/fallbacks/"},{"ref_id":"https://docs.llamaindex.ai/en/stable/examples/query_engine/RouterQueryEngine/"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn832281view1","lineno":142},{"ref_id":"turn832281view1","lineno":488},{"ref_id":"turn832281view3","lineno":250},{"ref_id":"turn832281view2","lineno":120}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"multi agent LLM systems heterogeneous model tiers small large models paper"},{"q":"heterogeneous LLM agents multi-agent different model sizes cost efficiency"},{"q":"multi-agent LLM collaboration different models cheap expensive planner executor paper"},{"q":"frontier model planner small model executor LLM agents paper"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"empirical study LLM generated code maintainability readability complexity developers 2025"},{"q":"AI generated code maintainability technical debt empirical study LLM"},{"q":"LLM code generation software quality maintainability security performance bugs empirical 2025"},{"q":"study AI generated code review maintainability bugs human code GitHub 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Godot AI generated code contributions ban official blog July 2026 maintainability"},{"q":"Godot Foundation AI authored code contributions banned maintainers official blog"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"AI coding startup shut down due to high model costs frontier model unit economics"},{"q":"AI agent startup killed by inference costs LLM unit economics case study"},{"q":"generative AI startup failed because OpenAI API costs unit economics"},{"q":"products shut down because of LLM costs AI agents 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"AI companies losing money on API inference costs generative AI unit economics agents 2026"},{"q":"Cursor Claude Code OpenAI Anthropic subscription costs power users agent tools compute costs 2026"},{"q":"Lovable Replit AI coding agents inference costs business model 2026"},{"q":"SaaS AI copilots high inference costs unit economics failure case"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"METR randomized controlled trial AI coding tools experienced open-source developers 19% slower 2025"},{"q":"GitClear AI generated code technical debt 211 million lines 2024 2025"},{"q":"GitHub Copilot study developers 55% faster controlled experiment 2022 paper"},{"q":"DORA AI adoption software delivery performance study 2024 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

# DR26-07-05-HUB-03 — Repairability-first multi-agent systems: cheap models as builders and repairers

**Report date:** 2026-07-05  
**Scope:** multi-agent AI systems for a small business using tiered Claude-style models, cheap/local workers, deterministic tools, repairability gates, and always-on agent fleets.

---

## A. Executive Summary

### A1. Bottom line

The founder’s hypothesis is **partly right but wrong in its naive form**.

The defensible version is:

> Do not optimize for “cheap models built it.” Optimize for “the cheapest acceptable repairer can understand, test, and safely patch it under bounded context, bounded tools, and bounded attempts.”

The research does **not** support a clean claim that smaller or cheaper models naturally produce simpler, more maintainable artifacts. The stronger evidence says maintainability is driven by **specification, scaffolding, tests, component boundaries, dependency discipline, observability, and repair workflow**. Smaller models can be excellent workers or repairers when the task is narrow, the interface is explicit, and the component is deliberately boring. But cheap builders without constraints often produce fragile glue, hidden bugs, hallucinated dependencies, and noisy PRs. Academic and industry evidence points in the same direction: **system design dominates model tier**. citeturn365200academia2turn915020academia5turn280167academia7turn471708academia2

The best architecture for a two-person company is therefore not “cheap models build everything.” It is:

1. **Deterministic first:** SQL, grep, scripts, validators, tests, static checks, schema checks.
2. **Expensive model designs simple blueprints:** use a strong model for architecture, invariants, threat modeling, and simplification.
3. **Cheap/local models execute leaf work:** constrained patches, data cleanup, extraction, boilerplate, test generation, documentation sync, triage.
4. **Weak model repair gate:** every always-on component must periodically survive a drill where a cheap/local model repairs seeded faults under realistic constraints.
5. **Escalation creates simplification debt:** if only a frontier model can fix a routine break, the component is not “maintainable”; it gets simplified or isolated.

This converts the hypothesis from a vague model-choice philosophy into an operational quality gate.

---

### A2. Key conclusion by evidence category

| Question | Evidence-based conclusion | Confidence |
|---|---:|---:|
| Do smaller models produce simpler code? | **No strong direct evidence.** Available evidence is mixed; complexity is more affected by prompt/spec/scaffold and task shape than by model size alone. | Medium |
| Can cheap/local models be useful builders? | Yes, for narrow bounded tasks with tests and explicit interfaces. Small specialized agents can reduce main-agent token use without reducing performance in some studies. | High |
| Can cheap/local models be useful repairers? | Yes for simple, localized faults with good tests/logs/specs. Repair success drops sharply for broad, semantic, security-sensitive, or human-reported bugs. | High |
| Should frontier models be avoided? | No. Frontier models are valuable as architects, judges, synthesizers, and difficult repairers. But uncontrolled frontier-agent loops create token and maintenance risk. | High |
| Are heterogeneous multi-agent systems promising? | Yes. Research on routing, cascades, and heterogeneous agent teams shows cost/performance gains, but routers can fail or collapse without evaluation and budgets. | High |
| Is a “weak model can fix it” gate publicly established? | Not as a named standard practice. Closest public patterns are mutation testing, chaos/fault injection, SWE-bench-style repair evaluation, CI gates, fallbacks, and model-routing evals. | Medium |
| What should a small company do now? | Build deterministic/tool-first systems, use strong models for simple specs, cheap/local for execution, instrument everything, and make repair drills mandatory for always-on components. | High |

---

### A3. The most important strategic correction

The real question is not:

> Which model should build the system?

It is:

> What is the weakest repairer that can safely restore this component when it breaks at 03:00 with only logs, tests, schemas, and the component contract?

That question forces good engineering:

- smaller components;
- fewer dependencies;
- explicit contracts;
- executable tests;
- reproducible local environments;
- readable logs;
- no hidden prompts or hidden global state;
- bounded tool permissions;
- measurable repair time;
- escalation paths.

The repair drill is a strong idea because it turns “simplicity” from an aesthetic preference into an empirical test.

---

### A4. Most relevant findings

1. **Complexity-aware feedback improves LLM code generation**, especially for weaker models. A 2025 study reports that adding static complexity metrics as feedback improved Pass@1 on HumanEval, with a particularly large improvement for GPT-3.5 Turbo versus its baseline. This supports “measure and constrain complexity,” not “use weak models and hope.” citeturn365200academia1

2. **Simple scaffolds can beat complex agent frameworks.** Agentless, a deliberately simpler repair method using localization, repair, and validation phases, reported 32% solved on SWE-bench Lite at about $0.70 per issue and argued that interpretable, non-agentic scaffolding can outperform heavier agent designs. citeturn471708academia2

3. **Cheap/small models can be excellent in narrow roles.** BudgetMLAgent combined no/low-cost models with GPT-4 as an ask-expert/planner mechanism and reported a 94.2% cost reduction versus GPT-4 single-agent, while improving average success in its benchmark. Terminus-4B, a 4B terminal-execution subagent, reduced main-agent token use by up to about 30% without performance loss in reported coding-agent settings. citeturn315675academia2turn315675academia1

4. **Heterogeneity beats cloning cheap workers.** Multi-agent scaling research reports diminishing returns from homogeneous agents, while diverse agents with different models, prompts, or tools can match or exceed much larger homogeneous ensembles. X-MAS similarly reports gains from heterogeneous model teams across reasoning domains. citeturn315675academia3turn315675academia0

5. **AI-generated code has real maintenance debt.** A large 2026 preprint analyzing 304,362 verified AI-authored commits across 6,275 GitHub repositories reports that code smells dominated detected issues, more than 15% of commits from every studied assistant introduced at least one issue, and 24.2% of tracked AI-introduced issues survived to the latest revision. citeturn915020academia5

6. **Agentic repair is still brittle.** Google’s internal program-repair evaluation reported plausible patches for 73% of machine-reported bugs but only 25.6% of human-reported bugs; semantically equivalent patches were lower, at 43% and 17.9% respectively. This strongly suggests that repairability depends on fault type and observability. citeturn471708academia0

7. **Security repair remains hard.** SecureAgentBench reports that the best evaluated agent/model combination achieved only 15.2% correct-and-secure repairs, and explicit security instructions did not significantly improve security outcomes. citeturn876613academia11

8. **Cost is not just model price.** A 2026 SWE-bench Verified cost study reports that agentic software-engineering tasks can consume about 1,000× more tokens than ordinary code chat/reasoning tasks, with up to 30× cost variation on the same task and no monotonic relationship between higher token use and higher accuracy. citeturn584861academia21

9. **Routing/cascades are becoming a core pattern.** RouteLLM, Routoo, C3PO, R2-Router, and uncertainty-based production routing all show that dynamic model selection can reduce cost while preserving quality, but router collapse is a real failure mode when routers over-select expensive models. citeturn990047academia0turn102240academia1turn102240academia3turn639965academia1

10. **Industry is converging on “modelmaxxing,” not single-model maximalism.** Recent industry reporting describes companies optimizing which model is used for which task, motivated by cost, latency, and scarcity of frontier-model budgets. This matches the research direction: route by task difficulty, do not hard-code a single premium model everywhere. citeturn268333news2

---

## B. Key Findings

### B1. Repairability is a property of the artifact and workflow, not the builder’s IQ

The strongest evidence cuts against the simplistic “cheap builder → simple artifact” claim. Code generated by LLMs can be syntactically plausible but semantically wrong, prompt-biased, missing corner cases, incomplete, or dependent on hallucinated objects. One 2024 bug taxonomy study across CodeGen, PanGu-Coder, and Codex categorized 333 bugs, including misinterpretations, wrong input types, missing corner cases, hallucinated objects, and incomplete generations. citeturn365200academia2

The same theme appears in newer maintainability work. A 2026 study of AI-authored commits at scale reports persistent code smells and issues, while another study of AI-agent readability commits reports that maintainability index often decreased and cyclomatic complexity often increased even when agents were explicitly modifying readability-related code. This is direct counter-evidence against “LLM output is naturally simple enough if the model is weaker.” citeturn915020academia5turn280167academia7

What improves repairability is structure. Agentless shows that a non-agentic, transparent pipeline—localization, repair, validation—can solve real GitHub issues without a complex autonomous loop. AutoCodeRover similarly uses program-structure analysis, AST search, tests, and fault localization rather than relying on a model’s raw intelligence. The consistent lesson is that **scaffolded repair beats free-form repair**. citeturn471708academia2turn471708academia1

Anthropic’s own discussion of Claude Code and SWE-bench scaffolding is directionally consistent: the model is embedded in a tool scaffold where it can search, read, edit, run tests, and use bash, while prior strong systems such as Agentless used retrieval, localization, candidate generation, and validation. The performance comes from the model plus the workflow, not the model alone. citeturn775830view2

**Implication:** choose the model after defining the artifact contract. A cheap model can maintain a simple component; a cheap model cannot rescue an under-specified, over-abstracted, dependency-heavy component just because it originally wrote it.

---

### B2. Smaller/cheaper models are viable as workers when the task is narrow

The evidence is strongest for cheap/local models as **bounded executors**, not autonomous architects.

BudgetMLAgent is a useful reference pattern: no/low-cost models underperformed as standalone agents, but when combined with a stronger model as an ask-expert/planning mechanism, the system reported both lower cost and higher success. The reported cost reduction was 94.2%, from $0.931 average per run with GPT-4 single-agent to $0.054, while average success rose from 22.72% to 32.95%. citeturn315675academia2

Terminus-4B is even closer to the proposed “small executes” pattern. It fine-tunes a Qwen3-4B model as a terminal-execution subagent. The authors report that small terminal subagents can reduce main-agent token usage by up to about 30% with no performance hit on SWE-bench Pro and an internal C# benchmark. citeturn315675academia1

Anthropic’s Haiku positioning also supports cheap-model workers. Claude Haiku 4.5 is described as a small model with low latency, lower cost, and coding/computer-use capability; Anthropic explicitly describes a pattern where Sonnet breaks down a complex problem and orchestrates multiple Haiku agents in parallel. Vendor claims require caution, but the pattern matches independent cascade and heterogeneous-agent research. citeturn691743view3turn436730view0turn315675academia0

**Implication:** cheap workers are economically rational for:

- deterministic transforms with small variation;
- schema-constrained extraction;
- boilerplate generation;
- writing tests from a precise spec;
- triaging logs;
- summarizing known context;
- localized patches with failing tests;
- mechanical refactors with strong test coverage.

They are risky for:

- architecture;
- ambiguous product judgment;
- security-sensitive changes;
- broad multi-file repairs;
- schema migrations without a deterministic migration plan;
- open-ended debugging without repros.

---

### B3. Heterogeneous agent teams outperform “many copies of the same cheap model”

Research on multi-agent LLM systems increasingly suggests that **diversity matters more than count**. A 2026 study on agent scaling reports that homogeneous scaling has diminishing returns, while heterogeneous agents using different models, prompts, tools, or reasoning strategies can match or exceed much larger homogeneous agent groups. citeturn315675academia3

X-MAS studied heterogeneous LLM multi-agent systems across 27 LLMs, five domains, five agentic functions, and more than 1.7 million evaluations. It reports that heterogeneous model assignment can improve performance without redesigning the whole multi-agent structure, with substantial gains on some math benchmarks. citeturn315675academia0

This matters for a small business. Running five identical cheap agents is usually worse than running:

- one deterministic validator;
- one cheap executor;
- one cheap critic;
- one strong architect/judge;
- one local repairer for drill compliance.

**Implication:** do not scale by adding more cheap agents. Scale by diversifying roles and making each role measurable.

---

### B4. Model routing and cascades are now a serious design space

Model routing has become a formal research area, not just an engineering hack. RouteLLM routes queries between stronger and weaker LLMs using preference data and reports more than 2× cost reduction without quality loss in some settings. Routoo uses performance prediction and cost-aware selection, reporting Mixtral-level performance at about one-third lower cost and near-GPT-4 performance at about half the cost when GPT-4 is available in the pool. citeturn990047academia0turn102240academia1

More recent work extends this further. UCCI uses calibrated uncertainty to route between 4B and 12B models in production named-entity recognition, reporting a 31% inference-cost reduction while maintaining micro-F1 around 0.91 and improving calibration. C3PO builds cascades with conformal prediction and cost bounds without labeled data. R2-Router jointly routes both model choice and output length budget, arguing that a strong model with a constrained output can outperform a weak model at the same cost. citeturn102240academia0turn102240academia3turn639965academia0

There is also a warning: “When Routing Collapses” reports that routers can degenerate toward selecting the most expensive model as budgets increase, even when cheaper models are sufficient. This is exactly the failure mode a subscription-budget company must avoid. citeturn639965academia1

**Implication:** model routing must be evaluated as a production subsystem. It needs budgets, labels, outcome metrics, latency metrics, fallback metrics, and audits. A router that always escalates to Opus/Fable is not a router; it is expensive theater.

---

### B5. Repair/debugging success is strongly fault-type dependent

Program repair benchmarks show progress but also hard limits.

Google’s internal agent-based program repair evaluation is especially relevant because it used real enterprise bugs rather than only public benchmark tasks. The agent produced plausible patches for 73% of machine-reported bugs but only 25.6% of human-reported bugs; semantically equivalent patches were 43% and 17.9% respectively. This gap matters because always-on business agents will produce both machine-detectable failures and vague human complaints. citeturn471708academia0

SWE-bench-style results show that agents can solve some real GitHub issues, but success depends heavily on localization, tests, validation, and scaffolding. Agentless reported 32% solved on SWE-bench Lite at about $0.70 per issue, while AutoCodeRover reported 19% on SWE-bench Lite at about $0.43 average cost. These are not “LLM magically fixes code” results; they are “LLM inside a repair pipeline” results. citeturn471708academia2turn471708academia1

SWE-bench itself has evolved into multiple tracks, including Verified, Lite, Multilingual, Full, and Multimodal, with the core metric being percentage of resolved issues. The benchmark’s continued segmentation is a signal that “repair ability” is not a single scalar; issue source, language, modality, and validation quality all matter. citeturn305309view0

**Implication:** the repair drill should classify faults. A model that fixes a broken config constant is not proven capable of fixing race conditions, security flaws, prompt injection, schema drift, or ambiguous product logic.

---

### B6. Security repair and secure code generation remain weak spots

Security-sensitive repair is far less mature than ordinary bug fixing. SecureAgentBench evaluates agents on tasks derived from real CVEs in large repositories and reports that the best evaluated agent/model combination achieved only 15.2% correct-and-secure repairs. It also reports that explicit security instructions did not significantly improve security outcomes. citeturn876613academia11

A 2025 study on AI-generated patches reports that Llama 3.3 70B and agentic repair workflows can introduce vulnerabilities, especially as autonomy increases. The risk is not only that agents fail to fix bugs; they may make code more exploitable. citeturn471708academia3

Package hallucination is another practical security and reliability problem. A 2026 replication study across modern models reports Python/JavaScript package hallucination rates in the 4.62%–6.10% range for the evaluated models, and notes that some hallucinated package names were invented by all five models. Even if rates have improved versus older open-source models, the attack surface remains real. citeturn630121academia11

**Implication:** cheap/local repairers should not be trusted as final authority on security-sensitive code. They can propose patches, but security checks must be deterministic where possible and reviewed by a stronger model or human for high-risk components.

---

### B7. Frontier-model unit economics can break the business case

A small company running always-on agents must treat token usage as an operating expense, not a rounding error. The 2026 SWE-bench cost study reports that agentic tasks can use about 1,000× more tokens than regular code chat/reasoning tasks, and that the same task can vary by up to 30× in token consumption depending on the model. Higher token use did not reliably imply higher accuracy; accuracy peaked at intermediate cost levels in the reported analysis. citeturn584861academia21

Anthropic’s current model pricing also makes tiering economically meaningful. Official pricing lists Claude Haiku 4.5 at $1 per million input tokens and $5 per million output tokens, Sonnet 5 at introductory $2/$10 through August 31, 2026 and then $3/$15, and Opus 4.8 at $5/$25. The input/output price ratios matter because agentic loops often produce long outputs, traces, tool calls, and repeated retries. citeturn975689view0turn691743view0

Industry reporting around agent tools also shows subscription-budget tension. Anthropic reportedly blocked some third-party agent tools from using Claude subscriptions because agentic usage can be too costly for flat-rate plans, while OpenAI reportedly adjusted Codex limits after users burned through credits due to automatic review/subagent behavior. These reports are not controlled studies, but they match the academic token-cost findings. citeturn268333news4turn584861news3

**Implication:** always-on agents need a cost governor. No component should be allowed to recursively call frontier models without explicit budget, task class, and stop condition.

---

### B8. “Build cost” is the wrong cost model; lifetime maintenance cost dominates

For agent systems, total cost includes:

- initial build tokens;
- recurring inference;
- retries and tool calls;
- failed automation cleanup;
- human review;
- debugging time;
- repair latency;
- outages;
- security review;
- dependency management;
- prompt/version drift;
- evaluation maintenance;
- opportunity cost of brittle automation.

This matches the broader shift toward lifecycle AI costing. Recent research proposes lifecycle cost metrics for AI output that include capital expenditure, operating expense, and vendor API versus self-hosted deployment economics. citeturn268333academia17turn584861academia13

Industry surveys also indicate that governance and cost visibility are becoming bottlenecks. Recent reporting on a GitLab study says organizations using AI coding tools increasingly cite review, validation, governance, and long-term maintainability concerns. Reporting on a KPMG study similarly says many business leaders lack clear visibility into AI usage costs, token costs, and ROI. These are survey/news data rather than controlled engineering studies, but they reflect the operational pain. citeturn174001news0turn584861news4

**Implication:** if a cheap model builds something in 10 minutes but it creates 20 hours of debugging later, it was expensive. The cost ledger must include repair time and review burden.

---

### B9. The strongest evidence against naive AI-code optimism comes from brownfield work

AI coding tools often perform well in toy or greenfield tasks, but evidence in real codebases is mixed. A 2023 randomized controlled trial found that GitHub Copilot users completed an HTTP server task 55.8% faster. That is a valid productivity signal, but the task was bounded. citeturn174001academia2

A 2025 METR study reported the opposite in a brownfield setting: experienced developers working in familiar repositories with Cursor were 19% slower, despite expecting to be faster. The key operational point is not “AI is bad”; it is that real codebase context, review overhead, and correction cost can erase expected gains. citeturn174001news1

Other studies show intermediate results. A small undergraduate brownfield study reported faster progress with Copilot but also concern about developers understanding suggestions. A longitudinal enterprise case study at NAV found no significant changes in commit-based activity after Copilot adoption, with users already more active before adoption. A proprietary-projects study reported time savings for documentation, autocomplete, repetitive coding, unit tests, debugging, and pair programming, but struggles on complex tasks, large functions, multi-file changes, and proprietary context. citeturn174001academia3turn174001academia5turn174001academia6

**Implication:** for your agent fleet, benchmark on your own brownfield components. Do not extrapolate from leaderboard coding tasks.

---

### B10. Open-source maintainers are reacting against low-quality AI contributions

The Godot project is a useful failure signal. Recent reporting says Godot maintainers were overwhelmed by AI-generated PRs and planned guideline changes forbidding AI-authored code and AI-generated communication in contributions, citing quality, review burden, and accountability concerns. citeturn280167news0turn280167news2

This does not mean AI-generated code is always bad. It means unbounded AI output can shift work from the author to the reviewer. For a two-person company, that reviewer is you. If cheap agents produce many plausible but untrusted patches, they become negative leverage.

**Implication:** your internal agent system should treat every AI patch as guilty until tests, static checks, and repair drill metrics prove otherwise.

---

## C. Contradictions & Debates

### C1. “Cheap models build simple systems” vs “cheap models build messy systems”

**Argument for the founder’s view:** smaller models have less capacity for elaborate abstractions, may default to simpler patterns, and are cheaper to run repeatedly. Narrow-worker studies and vendor positioning support the idea that small models can execute bounded tasks efficiently. citeturn315675academia1turn691743view3

**Counterargument:** smaller does not mean cleaner. AI-generated code studies show persistent code smells, bug categories, maintainability issues, package hallucinations, and security risks. Weak models may avoid advanced abstractions, but they can still produce spaghetti, duplicated logic, missing error handling, silent data corruption, and brittle prompts. citeturn365200academia2turn915020academia5turn630121academia11

**Resolution:** simplicity must be specified and tested. Use weak models only inside constraints that make messy output hard to accept.

---

### C2. “Use the strongest model everywhere” vs “route aggressively”

**Argument for strongest model:** strong models can reason through ambiguous design, maintain more context, detect contradictions, and synthesize architecture. Official Anthropic docs position top models for complex agentic coding and enterprise-grade tasks. citeturn691743view0

**Counterargument:** frontier-agent loops can be economically irrational. Token costs vary massively; higher token usage does not guarantee higher accuracy; cheaper models can outperform larger ones on some specific tasks. The code-review preprint comparing Claude Sonnet 4.6 and Haiku 4.5 reports Haiku outperforming Sonnet on that dataset with lower cost, though real PR performance remained poor for all models. citeturn584861academia21turn876613academia4

**Resolution:** use strong models for high-leverage thinking, not repetitive execution. Frontier time should be spent designing simple contracts, finding hidden assumptions, reviewing high-risk changes, and diagnosing failures that cheap repairers cannot fix.

---

### C3. “Agents are the future” vs “deterministic scaffolds beat agents”

**Argument for agents:** modern coding agents can use tools, run tests, inspect repositories, and perform multi-step repairs. SWE-bench and Claude Code-style workflows show practical capabilities. citeturn305309view0turn775830view2

**Counterargument:** Agentless and related approaches show that simple, interpretable pipelines can match or beat complex agent frameworks in some repair settings. More autonomy also increases token spend and security risk. citeturn471708academia2turn471708academia3turn584861academia21

**Resolution:** make workflows deterministic wherever possible. Let LLMs fill in uncertain steps inside a controlled scaffold rather than giving them open-ended autonomy.

---

### C4. “More test-time compute improves quality” vs “more tokens may just burn money”

Some frontier-model release notes and coding workflows emphasize extended thinking, tool use, and test-time compute. This can help on hard tasks. Anthropic positions Sonnet 5 and higher-tier models across different cost/performance ranges, with extended reasoning options. citeturn436730view2turn691743view0

But the SWE-bench cost study reports that higher token usage is not monotonically associated with better accuracy and that accuracy can peak at intermediate cost. This contradicts blind “let it think longer” policies. citeturn584861academia21

**Resolution:** treat reasoning budget as a tunable parameter with measured ROI. For routine repairs, cap attempts and escalate only after deterministic signals.

---

### C5. “Local models are free” vs “local models have hidden cost”

Local models avoid API bills and improve privacy/control, but they are not free. Local agents consume GPU power, memory, engineering time, and operational attention. AgentStop, a 2026 study of local device agents, reports that agentic execution increases GPU power, temperature, and battery use, and that early termination can reduce wasted energy by 15–20% with less than 5% utility drop. citeturn268333academia16

**Resolution:** local models are best for high-volume, low-risk, repetitive tasks and repair drills. They are not a substitute for good architecture or deterministic tests.

---

### C6. “Security instructions fix security” vs “security needs structural controls”

SecureAgentBench reports that explicit security instructions did not significantly improve secure repair outcomes. This is a direct warning against prompt-only safety. citeturn876613academia11

**Resolution:** use deterministic security controls: static analysis, dependency allowlists, sandboxed execution, least-privilege credentials, no unreviewed new packages, no network by default, and explicit taint boundaries.

---

### C7. “Routers save money” vs “routers collapse”

Routers can reduce cost. RouteLLM, Routoo, UCCI, C3PO, and R2-Router all report cost/performance gains under certain conditions. citeturn990047academia0turn102240academia1turn102240academia0turn102240academia3turn639965academia0

But routers can collapse toward expensive models when budgets loosen or evaluation is poorly calibrated. citeturn639965academia1

**Resolution:** route decisions must be logged and audited. The router needs explicit per-task target quality, maximum spend, and periodic counterfactual evaluation against cheaper alternatives.

---

## D. Best Practices

### D1. Tiering doctrine

Use this hierarchy:

| Tier | Role | Use for | Avoid for |
|---|---|---|---|
| **Tier 0: deterministic tools** | Source of truth | SQL, grep, schema validation, static analysis, tests, linters, diff checks, metric thresholds, allowlists | Anything requiring judgment or ambiguity |
| **Tier 1: local small model** | Weak repairer / cheap worker | repair drills, simple log triage, boilerplate, test stubs, low-risk transforms, repetitive classification | architecture, security final review, broad debugging |
| **Tier 2: cheap hosted model / Haiku-class** | Fast worker | bounded code edits, extraction, summarization, test generation, small refactors, parallel worker tasks | ambiguous design, high-risk patch approval |
| **Tier 3: Sonnet-class** | architect / senior implementer / judge | decomposition, brownfield reasoning, multi-file plans, repair after cheap failure, review of patches | bulk low-value repetitive work |
| **Tier 4: Opus/Fable/frontier** | principal architect / final escalation | architecture, root-cause analysis, hard incidents, threat modeling, complex trade-offs | routine fleet operation |

This matches the direction of heterogeneous multi-agent and cascade research: use cheap models where they are sufficient, strong models where they are necessary, and validate routing empirically. citeturn315675academia2turn315675academia0turn990047academia0turn639965academia1

---

### D2. Design every component so a weak repairer can understand it

A component is “repairable-first” only if a weak/local model can answer these questions from local context:

1. What does this component do?
2. What are its inputs and outputs?
3. What invariants must never break?
4. What logs prove it is working?
5. What tests fail when it is broken?
6. What dependencies are allowed?
7. What is the smallest safe patch?
8. How do I roll back?

A repairable component should usually have:

| Property | Target |
|---|---:|
| Files touched by normal repair | 1–2 |
| Runtime dependencies | 0 new deps by default |
| Main file size | preferably <300–500 LOC |
| Function cyclomatic complexity | ideally <10 |
| External APIs | wrapped behind one adapter |
| Config | externalized, typed, documented |
| Tests | smoke + unit + one regression test per incident |
| Logs | structured, grep-friendly, with correlation IDs |
| State mutation | explicit and idempotent |
| Prompt text | versioned and testable, not hidden in code |
| Secrets | never accessible to repairer by default |
| Tool permissions | allowlisted and least-privilege |

This is consistent with evidence that complexity and scaffolding affect model performance. Complexity-aware feedback improved code-generation performance, and newer work argues that LLM difficulty is tied to semantic/compositional complexity rather than only traditional metrics. citeturn365200academia1turn365200academia4

---

### D3. Spec constraints that force simple output from any tier

Use a strict implementation contract. Weak models and frontier models should both receive the same simplicity constraints.

```markdown
# Component Implementation Contract

## Goal
Implement exactly: <one-sentence business function>.

## Non-goals
- Do not add new product behavior.
- Do not add a framework.
- Do not add runtime dependencies.
- Do not create abstractions unless listed below.
- Do not modify files outside the allowed list.

## Allowed files
- <path/file.py>
- <path/test_file.py>

## Allowed dependencies
- Python stdlib only.
- Existing project utilities: <list>.
- New packages: forbidden unless explicitly approved.

## Input contract
- Input schema:
  ```json
  <schema>
  ```

## Output contract
- Output schema:
  ```json
  <schema>
  ```

## Invariants
- <invariant 1>
- <invariant 2>
- <invariant 3>

## Failure behavior
- On invalid input: return <explicit behavior>.
- On timeout: return <explicit behavior>.
- On upstream failure: log and <fallback behavior>.

## Observability
- Must log:
  - component_name
  - request_id
  - input_hash, not raw PII
  - decision
  - error_code
  - latency_ms

## Tests required
- Unit test for happy path.
- Unit test for invalid input.
- Regression test for the bug/fault.
- Smoke command:
  ```bash
  <command>
  ```

## Complexity budget
- No function >50 lines.
- No nested conditionals deeper than 2.
- No new class unless required by existing interface.
- Prefer pure functions.
- Diff should be <120 changed lines unless approved.

## Patch output format
- Explain root cause in 3 bullets.
- List changed files.
- Provide patch.
- Provide test command and expected result.
```

This contract is deliberately boring. The goal is not elegance. The goal is that a cheap/local model can repair it.

---

### D4. Builder-tier decision framework

Choose the builder model based on **ambiguity, blast radius, and reversibility**, not on habit.

| Component/task type | Builder | Repairer | Judge | Notes |
|---|---|---|---|---|
| SQL report, deterministic extraction | SQL/script first; local/Haiku for wrapper | local | deterministic tests | LLM should not invent data logic |
| Data normalization script | Haiku/local | local | tests + Sonnet spot review | Stdlib-only, schema tests |
| Prompt template for low-risk summarization | Haiku | Haiku/local | eval set + Sonnet | Version prompts, run regression evals |
| Orchestrator routing policy | Sonnet | Haiku for simple config; Sonnet for logic | deterministic cost tests + Opus/Sonnet | High leverage, avoid cheap design |
| Tool adapter to external API | Sonnet or Haiku with strict spec | Haiku/local for small changes | tests + Sonnet | External API drift needs logs |
| Security/credential handling | Sonnet/Opus/Fable + human | Sonnet only | deterministic security checks + human | No weak final approval |
| Multi-agent architecture | Sonnet/Opus/Fable | not weak | human + evals | Strong model designs simple blueprint |
| Boilerplate tests | Haiku/local | local | test runner | Good cheap-worker task |
| Incident root-cause analysis | Sonnet first if high impact | Sonnet/Opus | human | Token cost acceptable for outages |
| Cosmetic docs/summaries | local/Haiku | local | lightweight | Low risk |

---

### D5. Repairer-tier decision framework

A repairer should be selected by fault class.

| Fault class | First repairer | Escalation | Required evidence |
|---|---|---|---|
| Config constant wrong | local | Haiku | failing + passing test |
| Schema field rename | local/Haiku | Sonnet | schema diff + regression test |
| Data-format drift | Haiku | Sonnet | parser tests |
| API timeout/retry issue | Haiku | Sonnet | simulated timeout test |
| Single-function logic bug | Haiku/local | Sonnet | unit/regression test |
| Multi-file logic bug | Sonnet | Opus/Fable | integration tests |
| Prompt-injection vulnerability | Sonnet | Opus/Fable/human | adversarial eval |
| Credential/security bug | Sonnet + human | Opus/Fable + human | security scan and manual approval |
| Performance regression | Sonnet | Opus/Fable | benchmark before/after |
| Race/idempotency bug | Sonnet | Opus/Fable | concurrency test |

---

### D6. Use deterministic detectors before LLM judges

Your current practice—“dumb detector first, LLM as judge”—is correct and should become policy.

Good deterministic detectors:

- unit tests;
- integration smoke tests;
- schema validators;
- JSON schema checks;
- SQL constraints;
- type checks;
- linting;
- dependency diff checks;
- no-new-package checks;
- static security scans;
- timeout tests;
- golden-output comparisons;
- mutation tests;
- log pattern checks;
- cost/token budget checks.

LLM judges should only adjudicate what deterministic tools cannot: intent match, semantic plausibility, product judgment, root-cause explanation, and maintainability review. This is aligned with repair literature showing the importance of validation, localization, and tests. citeturn471708academia2turn471708academia1

---

### D7. Use routers, but make them auditable

A production router should log:

```json
{
  "task_id": "uuid",
  "component": "lead_enrichment.agent",
  "task_class": "schema_repair",
  "risk_tier": "medium",
  "selected_model": "haiku",
  "candidate_models": ["local_8gb", "haiku", "sonnet"],
  "router_reason": "local failed schema confidence; haiku allowed under budget",
  "input_tokens": 2142,
  "output_tokens": 638,
  "tool_calls": 3,
  "latency_ms": 9120,
  "estimated_cost_usd": 0.0062,
  "actual_cost_usd": 0.0065,
  "tests_before": "failed",
  "tests_after": "passed",
  "escalated": false,
  "human_review_required": false
}
```

LiteLLM is a practical gateway option because its router supports load balancing, cooldowns, fallbacks, timeouts, retries, and routing strategies such as weighted, rate-limit-aware, latency-based, least-busy, custom, and lowest-cost routing. It also documents fallback chains, including local Hugging Face endpoint examples. citeturn832281view0turn976185view0turn976185view1

LangChain/LangGraph/LangSmith are relevant for orchestration and observability because their docs emphasize deterministic plus agentic workflows, standardized model interfaces, tracing, debugging, evaluation, monitoring, retries, routing, and guardrails. LlamaIndex RouterQueryEngine is relevant for routing across query tools/indexes, especially when RAG source choice is part of the workflow. citeturn976185view3turn832281view3

---

### D8. Competitive/platform landscape

| Category | Examples | What they formalize | Strategic relevance |
|---|---|---|---|
| **Model gateway/router** | LiteLLM, OpenRouter-style gateways, Portkey-style gateways | fallback, routing, spend control, model abstraction | Direct substitute for custom routing layer |
| **Agent orchestration** | LangGraph/LangChain, AutoGen-style systems, Crew-style systems | agents, tools, state machines, retries | Useful but can over-abstract |
| **RAG/query routing** | LlamaIndex RouterQueryEngine | route to indexes/tools | Useful for knowledge-heavy agents |
| **Coding agents** | Claude Code, GitHub Copilot coding agent, Codex-style agents, Devin-style agents | repo inspection, edit, tests, PRs | Direct competitor/substitute for internal repair workers |
| **Academic repair systems** | Agentless, AutoCodeRover, SWE-agent/mini-SWE-agent | localization, repair, validation | Design references for internal repair drill |
| **Small-model worker pattern** | Terminus-4B, distilled/specialized local models | small executor under strong planner | Strong match for your thesis |
| **Cost/eval tooling** | RouteJudge/ORBIT-style router eval, LangSmith-style eval | routing quality, cost, latency, preference | Needed to prevent router collapse |

GitHub Copilot’s coding agent is especially relevant as a mainstream commercial substitute: reporting describes it as able to work on assigned issues, make code changes, run in a VM, log decisions, and return PRs for user review. This moves the competitive bar from “AI autocomplete” to “AI maintenance worker.” citeturn915020news7

---

## E. Failure Modes

### E1. Cheap model spaghetti

**Pattern:** cheap model builds a component that works on one demo but has duplicated logic, implicit state, no tests, unclear data contracts, and hidden failure modes.

**Evidence:** LLM bug taxonomies and AI-generated commit studies show real patterns of missing corner cases, wrong assumptions, hallucinated objects, incomplete generation, code smells, and persistent issues. citeturn365200academia2turn915020academia5

**Mitigation:**

- no component accepted without tests;
- no new dependency by default;
- diff size caps;
- complexity caps;
- no hidden global state;
- repair drill required before always-on deployment.

---

### E2. Frontier overengineering

**Pattern:** strong model creates a sophisticated architecture with plugin layers, abstractions, async queues, decorators, generic managers, and implicit conventions. It is impressive but only a strong model can understand or repair it.

**Evidence:** The best evidence is indirect: scaffolding and complexity control matter more than raw model capability, and Agentless-style simpler repair pipelines can compete with heavier agent frameworks. citeturn471708academia2turn365200academia1

**Mitigation:**

- use frontier models to simplify, not to elaborate;
- require “one-file boring implementation” as default;
- ask strong model for failure modes and simplification alternatives;
- make weak-model repair pass a release gate.

---

### E3. Silent semantic bugs

**Pattern:** tests pass because they are shallow; the agent changes business logic incorrectly; no exception is thrown.

**Evidence:** Google’s internal APR results show much lower semantically equivalent patch rates than plausible patch rates, especially for human-reported bugs. Plausibility is not correctness. citeturn471708academia0

**Mitigation:**

- golden datasets;
- adversarial examples;
- invariant checks;
- business-metric smoke tests;
- regression tests per incident;
- LLM judge cannot override failing deterministic tests.

---

### E4. Dependency hallucination and supply-chain risk

**Pattern:** model adds or references packages that do not exist, are wrong, abandoned, malicious, or unnecessary.

**Evidence:** A 2026 package hallucination study reports modern models still hallucinate package names in Python/JavaScript prompts, with some invented names shared across multiple models. citeturn630121academia11

**Mitigation:**

- new dependencies forbidden by default;
- dependency diff check in CI;
- package allowlist;
- lockfiles;
- SBOM;
- no model can install packages during repair drill unless task explicitly allows it.

---

### E5. Agent cost runaway

**Pattern:** agent loops through tool calls, retries, subagents, long reasoning, and repeated context stuffing until the monthly budget is damaged.

**Evidence:** The SWE-bench cost study reports ~1,000× token consumption for agentic tasks versus ordinary code chat/reasoning tasks, up to 30× variation on the same task, and no monotonic link between more tokens and higher accuracy. Recent reporting on Codex credits and Anthropic subscription restrictions also shows practical sensitivity to agentic usage costs. citeturn584861academia21turn584861news3turn268333news4

**Mitigation:**

- hard per-task token budgets;
- maximum attempts;
- maximum tool calls;
- stop conditions;
- cheap-first routing;
- escalation only with failure evidence;
- daily spend dashboard.

---

### E6. Router collapse

**Pattern:** the model router gradually sends most tasks to the expensive model because that path has fewer immediate failures or because the routing threshold is too conservative.

**Evidence:** “When Routing Collapses” explicitly studies this failure mode and proposes methods to reduce cost while maintaining high-tier performance. citeturn639965academia1

**Mitigation:**

- weekly router audit;
- counterfactual replay on cheaper models;
- per-task target success rate;
- minimum cheap-route quota for low-risk tasks;
- escalation must attach deterministic failure reason.

---

### E7. Maintainer burden from AI PRs

**Pattern:** agents generate many plausible patches that require human review, creating more work than they save.

**Evidence:** Godot maintainers’ reported backlash against AI-generated contributions is a concrete open-source signal. Research on AI-agent contributions also reports rising agent activity and more churn over time than human-authored code. citeturn280167news0turn280167academia4

**Mitigation:**

- patch volume caps;
- require failing test before patch;
- require regression test with patch;
- reject patches touching too many files;
- review only patches that pass deterministic gates.

---

### E8. Security regression from autonomous patches

**Pattern:** agent fixes a visible bug but introduces injection, privilege escalation, data leakage, unsafe parsing, or insecure dependency behavior.

**Evidence:** SecureAgentBench reports low correct-and-secure repair rates, and a 2025 patch-safety study reports vulnerabilities introduced by AI-generated patches and agentic workflows. citeturn876613academia11turn471708academia3

**Mitigation:**

- security-critical code excluded from weak autonomous repair;
- sandbox execution;
- static security analysis;
- secret scanning;
- threat-model checklist;
- human review for credential/auth/payment/customer-data paths.

---

### E9. Non-reproducible generated projects

**Pattern:** generated code works in the model’s imagined environment but fails in the real runtime because dependencies, versions, env vars, or setup steps are missing.

**Evidence:** A 2025/2026 preprint on AI-generated project reproducibility reports that only 68.3% of generated projects executed out-of-the-box, with large language differences and major expansion from declared to runtime dependencies. citeturn365200academia3

**Mitigation:**

- every component has `make smoke` or equivalent;
- generated code must run in clean container;
- no hidden environment assumptions;
- dependency lockfile check;
- install/run instructions tested by CI.

---

### E10. Local model false economy

**Pattern:** local model seems free but consumes setup time, GPU resources, cooling/power, and produces lower-quality patches that require review.

**Evidence:** AgentStop reports local agent execution increases device resource consumption and that early termination can reduce wasted energy while preserving most utility. citeturn268333academia16

**Mitigation:**

- local only for high-volume/repetitive/low-risk tasks;
- measure watt-hours or at least wall-clock and GPU utilization;
- compare local total cost against Haiku-class hosted calls;
- keep local repair scope small.

---

## F. Strategic Recommendations

### F1. Adopt the corrected doctrine

Reject:

> Cheap models should build the system because cheap-built systems are simpler.

Adopt:

> Strong models design simple, testable, boring components; cheap/local models build and repair bounded leaf tasks; a component is accepted only if the weakest designated repairer can fix seeded faults under budget.

This preserves the useful intuition—repairability matters more than prestige—without making an unsupported claim about model size.

---

### F2. Use “frontier designs, cheap executes, weak repairs”

Recommended operating model:

```text
Opus/Fable/Sonnet:
  - system architecture
  - simplification review
  - threat modeling
  - root cause of hard incidents
  - final review of high-risk patches

Haiku:
  - bounded coding tasks
  - test generation
  - structured extraction
  - localizable repairs
  - parallel worker agents

Local 7–14B / small coding model:
  - repair drills
  - smoke-test diagnosis
  - simple patches
  - repetitive text/code transforms
  - cheap critics/detectors

Deterministic code:
  - validation
  - routing predicates
  - data movement
  - schema checks
  - tests
  - deployment gates
```

This is directly consistent with BudgetMLAgent, Terminus-4B, routing research, and Anthropic’s own Haiku/Sonnet orchestration positioning. citeturn315675academia2turn315675academia1turn990047academia0turn691743view3

---

### F3. Define a repairability budget for every component

Every component should have an explicit budget:

```yaml
component: lead_enrichment_normalizer
risk_tier: medium
builder_tier_allowed:
  - haiku
  - sonnet_if_design_change
repairer_tier_required:
  - local_8gb
  - haiku
judge_tier:
  - deterministic_tests
  - sonnet_for_medium_risk
max_files_for_normal_repair: 2
max_diff_lines_for_normal_repair: 120
new_runtime_dependencies_allowed: false
max_function_lines: 50
max_cyclomatic_complexity: 10
required_tests:
  - unit
  - smoke
  - regression_for_each_fault
required_logs:
  - request_id
  - component
  - error_code
  - latency_ms
  - decision
repair_drill_pass_rate_required: 0.75
max_repair_attempts: 2
max_repair_tokens:
  local: 8000
  haiku: 12000
max_repair_wall_clock_minutes: 20
escalation_creates_simplification_ticket: true
```

The budget should live next to the component, not in a Notion page. The repairer should read it during the drill.

---

### F4. Make repair drills a release gate

A component should not be deployed into an always-on agent fleet unless it passes seeded repair drills. This is the practical version of the founder’s hypothesis.

The repair drill should answer:

1. Can the weak repairer reproduce the failure?
2. Can it localize the fault?
3. Can it patch without broad rewrites?
4. Can it add a regression test?
5. Can it pass all tests?
6. Can it avoid new dependencies?
7. Can it explain the fix?
8. Can it do this under budget?

If the answer is no, the component is too complex for its repair tier.

---

### F5. Treat failed weak repair as architecture feedback, not model failure

When the local/cheap repairer fails, do not immediately conclude “use a better model.” First classify why it failed:

| Failure reason | Architecture response |
|---|---|
| Could not find relevant file | improve component manifest and logs |
| Could not understand inputs | add schema and examples |
| Could not reproduce bug | improve smoke test and fixtures |
| Patch touched too many files | split component |
| Added dependency | tighten dependency policy and provide stdlib pattern |
| Fixed symptom not root cause | improve invariants and regression tests |
| Needed broad context | reduce coupling or add local README |
| Failed security reasoning | mark component high-risk and remove weak final approval |

Escalation to Sonnet/Opus is allowed, but every escalation from a routine fault should create simplification debt.

---

### F6. Build a cost ledger before buying more model power

Track these per component and per agent:

- input tokens;
- output tokens;
- cache hits;
- tool calls;
- model used;
- router decision;
- retries;
- wall-clock latency;
- estimated cost;
- actual invoice allocation;
- failure rate;
- human review minutes;
- repair drill success;
- production incident MTTR;
- patch churn;
- escaped defects.

This is necessary because token spend is not linearly tied to quality and agentic tasks can consume orders of magnitude more tokens than ordinary chat. citeturn584861academia21

---

### F7. Use local models as a maintainability constraint, not a religion

A local 8GB-VRAM model is useful as a **floor test**:

> If this component cannot be diagnosed or patched by the local model for simple seeded faults, it is too opaque.

But do not force local repair for:

- security-critical code;
- payment/credential systems;
- broad architectural changes;
- high-value customer-impacting incidents;
- legal/compliance wording;
- ambiguous business logic.

The local model is a measuring instrument. It is not always the right doctor.

---

### F8. Prefer “expensive simplification pass” over “expensive implementation pass”

Use a frontier model to attack the design:

```markdown
Review this component for repairability by a weak local model.

Goals:
- reduce files touched in normal repair;
- remove abstractions;
- remove dependencies;
- add invariants;
- add smoke tests;
- add logs;
- make failure modes explicit;
- preserve behavior.

Output:
1. top 10 complexity sources;
2. simplification plan;
3. minimal refactor patch;
4. repair drill faults to seed;
5. acceptance criteria.
```

This is likely a better use of frontier tokens than asking the frontier model to write large chunks of production code.

---

### F9. Classify components by blast radius

| Risk tier | Examples | Builder | Repairer | Deployment gate |
|---|---|---|---|---|
| **R0: disposable** | scratch scripts, one-off summaries | local/Haiku | local | smoke test |
| **R1: low** | docs sync, non-critical reports | local/Haiku | local | tests + cheap repair drill |
| **R2: medium** | lead enrichment, CRM update draft, inbox triage | Haiku/Sonnet | local/Haiku | repair drill + Sonnet review |
| **R3: high** | customer messaging, billing-adjacent workflows, database writes | Sonnet | Haiku for diagnosis, Sonnet for patch | integration tests + human approval |
| **R4: critical** | auth, secrets, payments, legal/compliance, irreversible actions | Sonnet/Opus/Fable + human | Sonnet/Opus + human | security review + staged deploy |

This prevents cheap/local ideology from leaking into high-risk paths.

---

## G. Action Plan

### G1. First 7 days — inventory and baseline

#### 1. Build an agent/component inventory

For every agent/component, record:

```yaml
component:
owner:
purpose:
risk_tier:
runtime_frequency:
external_actions:
data_touched:
files:
dependencies:
model_calls:
current_builder_model:
current_repair_model:
tests:
logs:
known_failure_modes:
monthly_token_cost_estimate:
human_review_minutes_per_week:
```

#### 2. Add a cost ledger

Minimum columns:

| Field | Meaning |
|---|---|
| `component` | agent/component name |
| `task_class` | extraction, coding, repair, review, routing, etc. |
| `model` | actual model used |
| `tokens_in` | input tokens |
| `tokens_out` | output tokens |
| `cost_usd` | estimated/actual |
| `latency_ms` | wall-clock |
| `tool_calls` | count |
| `success` | deterministic success |
| `escalated` | yes/no |
| `human_minutes` | review/cleanup |
| `incident_link` | if relevant |

#### 3. Define default routing policy

Start conservative:

```yaml
routing_policy:
  deterministic_first: true
  local_allowed:
    - log_triage
    - simple_patch_with_failing_test
    - boilerplate_test_generation
    - docs_sync
  haiku_allowed:
    - bounded_code_edit
    - extraction
    - summarization
    - schema_repair
    - test_generation
  sonnet_required:
    - multi_file_change
    - architecture
    - ambiguous_debugging
    - external_api_adapter
    - customer_visible_behavior
  opus_or_fable_required:
    - hard_incident_root_cause
    - security_architecture
    - irreversible_action_design
    - major_orchestrator_redesign
```

#### 4. Freeze dependency policy

Default rule:

> No AI-generated patch may add a runtime dependency unless the component manifest explicitly allows it.

This directly addresses hallucinated package and reproducibility failure modes. citeturn630121academia11turn365200academia3

---

### G2. Days 8–21 — implement repair drill harness

#### Repair drill protocol

**Goal:** empirically test whether the designated weak repairer can fix realistic faults.

##### Step 1 — Select component

Pick one always-on or soon-to-be always-on component.

Required inputs:

- component manifest;
- README;
- spec;
- tests;
- smoke command;
- logs or sample traces;
- allowed files;
- dependency policy;
- risk tier;
- designated repairer model.

##### Step 2 — Confirm baseline is green

Run:

```bash
make test COMPONENT=<component>
make smoke COMPONENT=<component>
```

Store baseline:

```json
{
  "component": "lead_enrichment_normalizer",
  "baseline_commit": "abc123",
  "tests": "passed",
  "smoke": "passed",
  "timestamp": "2026-07-05T00:00:00Z"
}
```

##### Step 3 — Inject one seeded fault

Use one fault per drill run. Avoid huge compound failures at first.

Fault library:

| Fault ID | Fault type | Example |
|---|---|---|
| `F01` | config mutation | timeout changed from 10s to 0.1s |
| `F02` | schema drift | `company_name` renamed to `organization_name` |
| `F03` | API contract drift | upstream returns list instead of object |
| `F04` | edge case missing | empty list, null field, unicode, duplicate ID |
| `F05` | dependency break | remove accidental dependency or version mismatch |
| `F06` | prompt injection | input says “ignore previous instructions” |
| `F07` | retry/idempotency | duplicate webhook event |
| `F08` | permissions | missing API key / read-only mode |
| `F09` | data corruption guard | malformed CSV/JSON |
| `F10` | concurrency | two workers process same job |

##### Step 4 — Provide weak repairer only realistic context

The repairer gets:

```markdown
You are repairing component: <component>.

Allowed files:
- <list>

You may use:
- read files
- edit allowed files
- run tests
- run smoke command
- inspect provided logs

You may not:
- install packages
- access secrets
- modify unrelated files
- change public contract unless instructed
- disable tests
- remove logging
- increase model tier

Failure:
<test output/log>

Task:
1. Reproduce or explain the failure.
2. Identify root cause.
3. Patch minimally.
4. Add or update one regression test.
5. Run tests.
6. Return root cause, changed files, test result, and residual risk.
```

##### Step 5 — Enforce budget

Recommended default budgets:

| Risk tier | Repairer | Attempts | Token budget | Wall-clock | Max files | Max diff |
|---|---|---:|---:|---:|---:|---:|
| R1 low | local | 2 | 8k | 20 min | 2 | 120 lines |
| R2 medium | local then Haiku | 2 each | 8k/12k | 30 min | 2 | 150 lines |
| R3 high | Haiku diagnosis, Sonnet patch | 1/2 | 12k/30k | 45 min | 3 | 200 lines |
| R4 critical | Sonnet/Opus + human | n/a | case-specific | case-specific | case-specific | case-specific |

##### Step 6 — Judge deterministically first

Pass requires:

- baseline fault reproduced;
- patch applies cleanly;
- failing test now passes;
- full component tests pass;
- smoke command passes;
- no new dependency unless approved;
- no unrelated file edits;
- no test deletion or weakening;
- no secrets exposed;
- logs remain structured.

##### Step 7 — Strong model or human review only after deterministic pass

For R2+ components, run a Sonnet-class review:

```markdown
Review this repair for:
- root-cause correctness;
- minimality;
- hidden regressions;
- security risk;
- maintainability;
- whether a weaker model could likely repair similar faults.

Do not rewrite unless necessary.
Return approve/reject with reasons.
```

##### Step 8 — Record metrics

```json
{
  "component": "lead_enrichment_normalizer",
  "fault_id": "F02",
  "repairer": "local_8gb_model",
  "attempts": 2,
  "success": true,
  "time_minutes": 14,
  "tokens_in": 6320,
  "tokens_out": 1480,
  "files_touched": 2,
  "diff_lines": 86,
  "new_dependencies": 0,
  "regression_test_added": true,
  "deterministic_tests_passed": true,
  "sonnet_review": "approved",
  "notes": "Schema aliases should be centralized."
}
```

---

### G3. Repair drill pass/fail criteria

#### Component-level pass

A component passes the repairability gate if:

| Risk tier | Required seeded-fault pass rate | Required repairer |
|---|---:|---|
| R1 low | ≥70% over 10 seeded faults | local |
| R2 medium | ≥75% over 10 seeded faults | local or Haiku |
| R3 high | ≥80% diagnosis by Haiku/local, patch by Sonnet | Haiku/Sonnet |
| R4 critical | no autonomous weak repair required; weak model may diagnose only | Sonnet/Opus/human |

#### Hard fail conditions

Any of these fail the drill:

- repairer disables or deletes tests;
- repairer adds unapproved dependency;
- repairer touches unrelated files;
- repairer changes public contract without explicit migration;
- repairer exposes or requests secrets;
- repairer claims success without running tests;
- patch passes narrow test but breaks smoke test;
- patch increases blast radius;
- patch requires frontier model for a trivial seeded fault.

#### Escalation rule

If local fails and Haiku succeeds, keep the component but add simplification debt.

If Haiku fails and Sonnet succeeds, the component is not cheap-repairable.

If only Opus/Fable/human succeeds, the component is high-complexity or high-risk; isolate it, simplify it, or accept that it is premium-maintained.

---

### G4. Days 22–45 — run drills and simplify

Run repair drills on the top 10 components by:

1. production frequency;
2. business impact;
3. token cost;
4. incident history;
5. human review burden.

For each failed drill, create exactly one simplification ticket:

```markdown
# Simplification ticket

Component:
Failed fault:
Repairer:
Failure mode:
Why weak repair failed:
Simplification action:
Expected effect:
Acceptance test:
Next drill date:
```

Common simplification actions:

- split component;
- add local README;
- add schema examples;
- centralize config;
- remove dependency;
- add smoke command;
- replace LLM judgment with deterministic rule;
- add adapter around external API;
- add regression test;
- reduce prompt length;
- remove abstraction;
- add structured logs.

---

### G5. Days 46–90 — productionize routing and repair

#### 1. Install or build a model gateway

Use a gateway/router layer rather than scattering model calls.

Minimum features:

- model aliases;
- fallback chains;
- per-task budgets;
- per-component budgets;
- logs;
- retries;
- cooldowns;
- local endpoint support;
- cost estimates;
- kill switch.

LiteLLM is a practical candidate because its documented router covers fallbacks, load balancing, cooldowns, timeouts, retries, and cost/latency/rate-limit routing strategies. citeturn832281view0turn976185view0

#### 2. Add weekly router replay

Take a sample of production tasks and replay them through cheaper models.

Measure:

- would cheap model have succeeded?
- would it have failed safely?
- cost delta;
- latency delta;
- output quality;
- deterministic pass/fail.

This is how you prevent router collapse.

#### 3. Add “chaos repair day”

Once per week:

- seed 3–5 faults in staging;
- require weak repairer attempts first;
- log repair performance;
- simplify one failed component;
- update the fault library.

#### 4. Create an incident-to-drill loop

Every production incident must produce:

- root cause;
- regression test;
- seeded fault version;
- repair drill run;
- manifest update.

This makes the agent fleet antifragile instead of merely patched.

---

### G6. Roadmap for a two-person company on subscription budgets

#### Month 1 — control

- Inventory all agents.
- Add model/cost logging.
- Enforce deterministic-first policy.
- Freeze dependencies.
- Build repair drill harness.
- Run drills on 3 highest-value components.

#### Month 2 — simplification

- Simplify components that fail local/Haiku repair.
- Add component manifests.
- Add smoke commands.
- Add schema tests.
- Add structured logs.
- Start weekly router replay.

#### Month 3 — routing

- Put all calls behind a gateway.
- Define tier policy by task class.
- Add fallbacks and kill switches.
- Track cost per successful output.
- Introduce local model for low-risk drills/workers.

#### Month 4–6 — specialization

- Identify repetitive high-volume tasks.
- Build small eval sets.
- Consider fine-tuning/distillation only if volume justifies it.
- Add adversarial/security drills for R3/R4 components.
- Add monthly frontier-model architecture review focused on simplification.

#### Do not do yet

- Do not build a complex multi-agent framework before the repair drill exists.
- Do not fine-tune local models before you have task logs and evals.
- Do not use frontier models as always-on workers without hard budgets.
- Do not trust AI patches without deterministic tests.
- Do not accept generated code that a weak repairer cannot understand.

---

## H. Sources

### H1. Academic and benchmark sources

1. **Enhancing LLM-Based Code Generation with Complexity Metrics: A Feedback-Driven Approach** — supports complexity-aware prompting/feedback and the claim that complexity constraints improve code generation, especially for weaker models. citeturn365200academia1

2. **Bugs in Large Language Models Generated Code** — taxonomy of LLM-generated code bugs including misinterpretation, missing corner cases, hallucinated objects, and incomplete generation. citeturn365200academia2

3. **AI-Generated Code Is Not Reproducible (Yet): Dependency Gaps in Executable Code Generation** — evidence that generated projects often fail out-of-the-box and expand runtime dependencies beyond declared dependencies. citeturn365200academia3

4. **Rethinking Code Complexity Through the Lens of LLMs** — argues traditional complexity metrics are insufficient and proposes LLM-oriented complexity based on semantic/compositional difficulty. citeturn365200academia4

5. **Code Readability in the Age of LLMs: An Industrial Case Study from Atlassian** — industrial evidence on AI-generated code readability compared with human code. citeturn915020academia3

6. **Debt Behind the AI Boom: Quantifying Code Quality Issues in GitHub Copilot and Other LLM-generated Code** — large-scale AI-authored commit analysis with code smells and persistent issues. citeturn915020academia5

7. **Investigating Autonomous Agent Contributions in the Wild** — analysis of open-source PRs from coding agents and increased churn. citeturn280167academia4

8. **To What Extent Does Agent-generated Code Require Maintenance?** — empirical dataset on maintenance patterns of AI-generated files. citeturn280167academia6

9. **Do AI Agents Really Improve Code Readability?** — reports mixed/negative maintainability-index and cyclomatic-complexity outcomes for agent readability commits. citeturn280167academia7

10. **Bigger Isn’t Always Better: Cost–Performance Trade-offs in LLM Code Review** — evidence that a smaller/cheaper model can outperform a larger model on a specific code-review benchmark, while all models degrade heavily on real PRs. citeturn876613academia4

11. **Evaluating Agent-based Program Repair at Google** — internal enterprise APR evaluation with plausible and semantically equivalent patch rates for machine- and human-reported bugs. citeturn471708academia0

12. **AutoCodeRover: Autonomous Program Improvement** — AST/search/test/fault-localization repair system with reported SWE-bench Lite performance and cost. citeturn471708academia1

13. **Agentless: Demystifying LLM-based Software Engineering Agents** — simple localization/repair/validation pipeline reporting strong SWE-bench Lite results at low cost. citeturn471708academia2

14. **SWE-bench official leaderboard** — benchmark taxonomy and resolved-issue metric for software-engineering agents. citeturn305309view0

15. **How Safe Are AI-Generated Patches?** — evidence that AI-generated and agentic patches can introduce vulnerabilities. citeturn471708academia3

16. **SecureAgentBench** — security repair benchmark showing low correct-and-secure repair rates and weak effect from explicit security instructions. citeturn876613academia11

17. **Range Shrinks, Threat Remains: Package Hallucinations in Code Generation by Large Language Models in 2026** — modern package hallucination rates and shared hallucinated package names. citeturn630121academia11

18. **X-MAS: Towards Building Multi-Agent Systems with Heterogeneous LLMs** — large-scale study of heterogeneous multi-agent systems. citeturn315675academia0

19. **BudgetMLAgent: A Cost-Effective LLM Multi-Agent System for Automating Machine Learning Tasks** — no/low-cost models plus GPT-4 cascade/ask-expert pattern with large reported cost reduction. citeturn315675academia2

20. **Understanding Agent Scaling Law Through the Lens of Agent Diversity** — evidence that heterogeneous agent teams can outperform or match larger homogeneous teams. citeturn315675academia3

21. **Terminus-4B: Training Time Scaling for Agentic Terminal-Use Agents** — small 4B terminal subagent reducing main-agent token usage without reported performance loss. citeturn315675academia1

22. **RCR-Router: A Multi-Agent LLM Context Routing Framework for Knowledge-Intensive Tasks** — context routing with structured memory and token reduction. citeturn993662academia2

23. **RouteLLM: Learning to Route LLMs with Preference Data** — dynamic routing between weaker and stronger models for cost reduction. citeturn990047academia0

24. **Routoo: Learning to Route to Large Language Models Effectively** — performance-prediction and cost-aware model selection. citeturn102240academia1

25. **Uncertainty-Calibrated Cost Control for LLM-Based Information Extraction** — production routing between 4B and 12B models with reported cost reduction and calibration gains. citeturn102240academia0

26. **C3PO: Cost-Effective LLM Cascades with Conformal Prediction** — self-supervised cascades with cost bounds and no labeled data. citeturn102240academia3

27. **R2-Router: Learning to Route and Reason for Cost-Effective and Adaptive LLM Inference** — joint routing of model and output-length budget. citeturn639965academia0

28. **When Routing Collapses: Router Design in LLM Cost-Performance Optimization** — router-collapse failure mode and mitigation. citeturn639965academia1

29. **RouteJudge: Serving as a Cost-Aware Reinforcement Learning Judge for Model Routing** — online evaluation and logging concepts for model routers. citeturn990047academia1

30. **How Do AI Agents Spend Your Money? A Study of Cost in Agentic Software Engineering** — token-cost analysis on SWE-bench Verified, including massive token usage and non-monotonic cost/accuracy relationship. citeturn584861academia21

31. **AgentStop: Dynamic Utility Monitoring for Wasted Energy Reduction in LLM Agents** — local-agent energy/resource cost and early stopping. citeturn268333academia16

32. **Lifecycle Cost of AI / LCOAI-related work** — framework direction for lifecycle cost per productive AI output. citeturn268333academia17turn584861academia13

33. **GitHub Copilot RCT / productivity study** — bounded-task evidence of faster completion with Copilot. citeturn174001academia2

34. **Brownfield Copilot / longitudinal and real-world studies** — mixed evidence on AI coding productivity in real codebases. citeturn174001academia3turn174001academia5turn174001academia6

---

### H2. Vendor and platform documentation

35. **Anthropic Claude model overview** — current model tiers, positioning, context windows, output limits, and official pricing table context. citeturn691743view0

36. **Anthropic pricing page** — official Claude pricing for Haiku, Sonnet, Opus, and related pricing notes. citeturn975689view0

37. **Claude Haiku 4.5 announcement/docs** — vendor positioning of Haiku as fast/cheap coding-capable worker and Sonnet-orchestrates-Haiku pattern. citeturn691743view3turn436730view0

38. **Claude Haiku 4.5 SWE-bench note** — reported SWE-bench Verified score and evaluation setup. citeturn436730view1

39. **Claude Sonnet 5 announcement/docs** — vendor positioning on cost/performance, coding, tool use, and partner examples. citeturn436730view2turn436730view3

40. **Claude 3.7 Sonnet and Claude Code announcement** — early Claude Code tool-use/scaffold details and hybrid reasoning context. citeturn775830view2

41. **Claude 3.5 Sonnet announcement** — historical context on coding-agent performance and cost/speed positioning. citeturn775830view0

42. **LiteLLM Router docs** — production routing, load balancing, fallbacks, cooldowns, timeouts, retries, and routing strategies. citeturn832281view0

43. **LiteLLM fallback docs** — fallback order, error-specific fallbacks, and local endpoint fallback examples. citeturn976185view0turn976185view1

44. **LangChain/LangGraph/LangSmith docs** — deterministic plus agentic workflows, standardized model interface, tracing, debugging, evaluation, retries, routing, and guardrails. citeturn976185view3

45. **LlamaIndex RouterQueryEngine docs** — routing across tools/indexes and aggregating selected tool outputs. citeturn832281view3

---

### H3. Industry reporting and market signals

46. **Business Insider on “modelmaxxing”** — industry trend toward optimizing model choice by task complexity and cost rather than defaulting to the largest model. citeturn268333news2

47. **Reuters on Cohere/customized models** — enterprise trend toward smaller/customized models over only scaling foundation models. citeturn102240news5

48. **Financial Times on distillation** — distillation trend: large teacher models transferring behavior to smaller/faster/cheaper student models, with trade-offs. citeturn102240news7

49. **Reuters on GLM/Z.ai low-cost coding/agentic model pressure** — competitive pressure from inexpensive coding/agentic models. citeturn268333news1

50. **Business Insider on Z.ai/ZCode pricing** — market pressure from low-cost coding tools and model integration. citeturn584861news1

51. **OpenAI Codex credit reporting** — practical signal that agent subagents/auto-review can unexpectedly burn credits. citeturn584861news3

52. **Axios on Anthropic blocking third-party agent tools from subscriptions** — practical signal of flat-rate subscription tension with agentic workloads. citeturn268333news4

53. **Reuters/Gartner on agentic AI project cancellations** — warning that many agentic AI projects may be scrapped due to cost, unclear value, or risk. citeturn268333news3

54. **GitLab study reporting** — industry signal that review, validation, governance, and maintainability are becoming bottlenecks for AI coding adoption. citeturn174001news0

55. **KPMG/ITPro cost visibility reporting** — industry signal that token/cost visibility and ROI measurement remain weak in many AI deployments. citeturn584861news4

56. **The Verge on GitHub Copilot coding agent** — mainstream coding-agent feature set: assigned issues, VM execution, code changes, logs, and PR review. citeturn915020news7

57. **Godot AI contribution backlash reporting** — maintainer-burden and quality-control signal for AI-generated PRs. citeturn280167news0turn280167news2

58. **Veracode/TechRadar AI code security reporting** — industry security signal that AI-generated code can contain substantial flaws and larger models are not automatically secure. citeturn915020news6

59. **IDESaster AI IDE vulnerability reporting** — prompt-injection and agentic-tool vulnerability signal for AI coding environments. citeturn915020news8

---

## Final strategic position

The right system is not “cheap models as builders.” It is **repairability-first orchestration**:

```text
Strong model:
  designs the simplest possible blueprint.

Cheap model:
  builds bounded parts from strict specs.

Local/cheap repairer:
  must fix seeded faults.

Deterministic tools:
  decide whether repair actually worked.

Strong model/human:
  reviews high-risk changes and simplifies components that fail drills.
```

Your proposed repair drill is the highest-leverage idea in the whole thesis. Make it the gate that determines whether an agent component is allowed to live in the fleet.
