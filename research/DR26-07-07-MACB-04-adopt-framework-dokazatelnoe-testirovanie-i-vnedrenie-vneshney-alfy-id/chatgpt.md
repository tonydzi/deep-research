# DR26-07-07-MACB-04 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Adopting External Ideas Framework"
type: ai-conversation
stage: raw
source: chatgpt


authored_by: hybrid
conversation_id: 6a4d409b-9db4-83ed-9874-49ab3711d568
model: "gpt-5-5-pro"
url: "https://приватный чат"
date_recorded: 2026-07-07
date_added: 2026-07-09
valid_as_of: 2026-07-07
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 27
related_concepts: []
revisit_if: ""
---

# Adopting External Ideas Framework

> ChatGPT · 2026-07-07 · `gpt-5-5-pro` · 27 сообщений

## Диалог

**Anton:**

@Web search # DR26-07-07-MACB-04 — ADOPT framework: testing & implementing external "alpha" into a personal knowledge system

Conduct a deep research investigation on the following topic.

TOPIC:
How to rigorously TEST and IMPLEMENT external "alpha" — someone else's ideas, methods, mental models, or practices — into a PERSONAL knowledge system and daily working processes (a solo operator augmented by LLM agents + an Obsidian vault + RAG). Not org-scale change management — personal/small-team adoption. The goal is a repeatable framework that takes an idea from "captured & judged valuable" to "tested on my real data → probation → implemented in my process → verified it actually improved the system (or dropped)."

CONTEXT (what we already have — don't re-derive this; dig where we're blind):
- We already have a HARVEST engine: deterministic detector (0-token) → LLM-judge → review screen → write idea into a home note. It MINES alpha from text corpora and stores it as knowledge. It does NOT test or implement anything.
- The MISSING layer (the subject of this research) is ADOPT: map → test-on-real-data → probation(KEEP/DROP) → implement(rule/skill/hook/field) → verify-with-a-consumer.
- Draft 6 stages we invented and want validated/improved against evidence: CAPTURE → MAP → TEST(before→after on real data) → TRIAL(KEEP/DROP, time-boxed) → PROMOTE(implement + regression test + assign a consumer) → DROP(log to declined-registry).
- Design constraints: simplicity/repairability first (a non-engineer must understand & fix it); prefer deterministic checks over LLM judgment where possible; every implemented change must have a named consumer or it's a "black hole" (built != used).

OPEN QUESTIONS we most need answered:
1. What VALIDATED frameworks exist for adopting external practices at the individual level — PDCA/Deming cycle, Lean Build-Measure-Learn, evidence-based practice, Kaizen, personal experiments / Quantified-Self, habit-formation & behavior-change models, spaced trials? Which map cleanly onto a 6-stage TEST→TRIAL→IMPLEMENT pipeline?
2. How to MEASURE "stickiness" — that a process change actually took hold vs silently reverted (adherence, retention, decay curves, leading vs lagging indicators)?
3. What are the ANTIPATTERNS that make adopted practices fail (cargo-culting, too-many-changes-at-once, no owner, no metric, novelty bias)? How to guard against each?
4. How to set KEEP/DROP thresholds and probation windows objectively for a single user?
5. How to distinguish "validate that my existing system already implements this principle" (no-op adoption) from "genuinely new implementation"?
6. Any prior art for adopting ideas specifically into a "human + LLM agents + PKM/RAG" loop?

RESEARCH REQUIREMENTS:
1. Academic research — papers, meta-analyses, benchmarks (behavior change, EBP, improvement science, diffusion of innovations).
2. Industry practice — startups, unicorns, leading companies, open-source PKM/agent projects.
3. Competitive landscape — direct, indirect, substitutes (existing "idea-to-implementation" methodologies & tools).
4. Failure analysis — failed adoption attempts, abandoned projects, lessons learned.
5. Emerging trends — developments from the last 12 months, frontier approaches (esp. LLM-assisted adoption/verification).
6. Implementation recommendations — an actionable framework, roadmap, priorities that refine or replace our 6-stage draft.

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

**ChatGPT:** · `gpt-5-5-pro`

I’ll treat your 6-stage ADOPT draft as the baseline to validate, not re-derive HARVEST. I’ll map academic and industry adoption models into a solo-operator pipeline with measurable gates, failure guards, and LLM/RAG-specific verification.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"PDSA cycle improvement science systematic review small tests of change adoption evidence"},{"q":"Normalisation Process Theory implementation individual practices adoption evidence May et al 2009"},{"q":"Proctor implementation outcomes acceptability adoption appropriateness feasibility fidelity penetration sustainability 2011"},{"q":"RE-AIM framework adoption implementation maintenance individual level evidence"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"habit formation study automaticity asymptote days Lally 2010 health psychology"},{"q":"behavior change technique taxonomy v1 Michie 2013 paper"},{"q":"COM-B behaviour change wheel intervention functions Michie 2011 paper"},{"q":"maintenance of behavior change systematic review habits self-monitoring action planning"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"N-of-1 trials design single patient interventions systematic review reporting CONSORT extension"},{"q":"Quantified Self self tracking personal science self-experimentation review"},{"q":"personal science n-of-1 self-experimentation framework health behavior review 2024"},{"q":"single-case experimental design behavior change adoption ABAB multiple baseline systematic review"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"evidence based practice five steps ask acquire appraise apply assess Sackett evidence based medicine"},{"q":"evidence based practice process ask acquire appraise apply assess individual practitioner"},{"q":"knowledge to action framework implementation science action cycle monitor knowledge use evaluate outcomes sustain knowledge use"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Lean Startup build measure learn validated learning minimum viable product Eric Ries"},{"q":"A/B testing experimentation culture trustworthy online controlled experiments guide Microsoft Ron Kohavi"},{"q":"continuous discovery product habits opportunity solution tree Teresa Torres assumptions testing"},{"q":"Toyota kata improvement kata scientific thinking daily practice"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"RAG evaluation framework RAGAS faithfulness answer relevancy context precision 2024 2025"},{"q":"LangSmith evaluations regression testing LLM applications documentation"},{"q":"OpenAI evals framework LLM evaluation regression testing prompts"},{"q":"promptfoo LLM evals regression tests prompts RAG CI"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LLM agents evaluation traces tool use benchmark regression testing 2025 paper"},{"q":"AI agent evaluation framework trajectory tool use error analysis 2025"},{"q":"Anthropic agent evaluation prompt engineering evals official docs agents"},{"q":"OpenAI Agents SDK evals tracing official docs 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"implementation science barriers facilitators adoption fidelity sustainability systematic review"},{"q":"failed implementation evidence based practices no fidelity sustainability implementation failure review"},{"q":"cargo cult agile adoption failure antipatterns process improvement no metrics"},{"q":"Diffusion of Innovations trialability observability relative advantage compatibility complexity adoption Rogers"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"measuring adherence to behavior change interventions retention decay curve habit automaticity self report SRBAI"},{"q":"Self-Report Behavioural Automaticity Index SRBAI habit strength measure paper Gardner 2012"},{"q":"intervention adherence measurement implementation fidelity dose delivered dose received enactment framework"},{"q":"long-term retention behavior change relapse decay curve adherence interventions maintenance metrics"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"architecture decision records ADR template status accepted deprecated superseded consequences implementation verification"},{"q":"Wardley mapping doctrine no-op adoption existing capability map practices"},{"q":"capability maturity model personal process improvement individual personal software process"},{"q":"Personal Software Process measurement improvement Humphrey evidence"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Obsidian spaced repetition plugin incremental writing spaced repetition knowledge management"},{"q":"readwise reader ghostreader AI export to Obsidian highlights notes workflow"},{"q":"Tana Supertags AI knowledge management agents workflows 2025"},{"q":"Mem AI knowledge management AI notes workflows 2025 personal knowledge management"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Obsidian Copilot plugin RAG vault AI chat documentation 2025"},{"q":"Khoj personal AI second brain Obsidian notes open source RAG"},{"q":"AnythingLLM desktop documents RAG personal knowledge base"},{"q":"Google NotebookLM sources notebook AI research assistant 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"2026 LLM agent skills reusable procedures markdown skills Anthropic Claude skills"},{"q":"Claude skills agents reusable capabilities folder instructions 2025 official"},{"q":"AI agents memory personal knowledge management RAG Obsidian 2026 emerging trends"},{"q":"agentic knowledge management AI personal workflow automation 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn888662search0"},{"ref_id":"turn888662search6"},{"ref_id":"turn888662search3"},{"ref_id":"turn723276search6"},{"ref_id":"turn723276search5"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn888662search12"},{"ref_id":"turn387878search2"},{"ref_id":"turn995690search2"},{"ref_id":"turn995690search12"},{"ref_id":"turn735408search6"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn723276search4"},{"ref_id":"turn723276search3"},{"ref_id":"turn995690search5"},{"ref_id":"turn995690search3"},{"ref_id":"turn995690search19"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn351584search1"},{"ref_id":"turn351584search0"},{"ref_id":"turn351584search7"},{"ref_id":"turn946441search2"},{"ref_id":"turn946441search11"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn233101search0"},{"ref_id":"turn233101search3"},{"ref_id":"turn414208search0"},{"ref_id":"turn414208search1"},{"ref_id":"turn414208search18"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn414208search24"},{"ref_id":"turn414208search8"},{"ref_id":"turn414208search22"},{"ref_id":"turn524945search29"},{"ref_id":"turn524945search10"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"minimal clinically important difference MCID decision threshold individual n-of-1 trial pre-specified effect size"},{"q":"N-of-1 trial minimal important difference decision criteria treatment response individual"},{"q":"single-case experimental design standards minimum data points phase stability visual analysis"},{"q":"A/B testing minimum detectable effect practical significance guardrail metrics"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn712848search1"},{"ref_id":"turn712848search2"},{"ref_id":"turn712848search3"},{"ref_id":"turn712848search18"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

Early signal: the draft needs stronger counterfactual testing and maintenance tracking. DROP should be possible at every gate, and VERIFY should be explicit before promotion.

---

**ChatGPT:** · `gpt-5-5-pro`

# A. Executive Summary

Your 6-stage draft is directionally correct, but I would **not** keep it as a simple linear pipeline. The stronger design is a **gated state machine**:

> **INTAKE → MAP → TEST → TRIAL → PROMOTE → VERIFY**  
> with terminal side states: **DROP**, **CONFIRMED_EXISTING**, **SUPERSEDED / RETIRED**

The critical change: **DROP should not be a final “stage” after all work is done**. It should be possible after MAP, TEST, TRIAL, or VERIFY. Conversely, **VERIFY should be explicit**, not buried inside PROMOTE. Otherwise “implemented” changes become silent dead code.

The evidence base converges on five invariants:

1. **Small local tests before implementation** — this is straight PDSA / improvement-science logic: test in the real environment, learn, adapt, then implement only after evidence accumulates. IHI explicitly frames PDSA as learning how a change works “in the local environment” and recommends many small cycles before broad implementation. citeturn391488view0

2. **Implementation success is not the same as outcome success** — Proctor’s implementation-outcomes taxonomy is useful here: acceptability, adoption, appropriateness, feasibility, fidelity, cost, penetration, and sustainability. This distinction prevents the common error: “the idea failed” when actually the implementation failed. citeturn506458view1

3. **Behavior-change mechanics must be specified, not implied** — COM-B says behavior depends on capability, opportunity, and motivation; the BCT taxonomy exists because vague behavior-change interventions are hard to replicate, test, or implement with fidelity. For ADOPT, every alpha must become a specified behavior, rule, hook, field, skill, or evaluation artifact. citeturn506458view3turn506458view4

4. **Solo testing should use N-of-1 / single-case logic** — for one operator, the best rigorous pattern is repeated measurement: baseline, intervention, phase comparison, and preferably reversal or multiple-baseline designs when practical. Single-case designs are explicitly built for testing effects within an individual, while acknowledging threats like maturation, confounding events, and reactivity. citeturn794661view4turn731471view3

5. **For LLM/RAG/agent workflows, promotion requires evals and traces** — modern LLM eval practice is already aligned with ADOPT: offline datasets before shipping, regression tests, online monitoring, trace inspection, task-level graders, and outcome-state evaluation rather than vibes. citeturn261709view0turn261709view2turn261709view3turn261709view4

The strongest recommendation: **make ADOPT a registry + test harness + promotion gate**, not a note-taking ritual. HARVEST finds alpha. ADOPT decides whether alpha becomes **living process** or **archived idea**.

---

# B. Key Findings

## 1. Validated frameworks that map cleanly onto ADOPT

| Framework | What it contributes | ADOPT mapping | Caveat |
|---|---:|---:|---|
| **PDSA / Model for Improvement** | Small, fast, local tests; prediction → data → learning → adaptation | MAP → TEST → TRIAL → PROMOTE | PDSA is often poorly executed unless predictions and data are explicit. IHI emphasizes planning objectives, predictions, data collection, studying results, and refining before implementation. citeturn391488view0 |
| **Evidence-Based Practice / KTA** | Ask, acquire, appraise, apply, assess; adapt knowledge to context; monitor use | CAPTURE/HARVEST → MAP → TEST → TRIAL → VERIFY | Designed mostly for clinical/org contexts, but the apply/assess cycle translates well to personal systems. citeturn387878search1turn387878search5 |
| **Proctor implementation outcomes** | Separates “was the thing used?” from “did it work?” | TRIAL and VERIFY metrics | Excellent vocabulary for your registry: adoption, fidelity, feasibility, cost, sustainability. citeturn506458view1 |
| **RE-AIM** | Reach, effectiveness, adoption, implementation, maintenance | Especially useful for “stickiness” and maintenance | Needs translation from org/population scale to solo workflow scale. citeturn888662search3turn888662search7 |
| **Normalization Process Theory** | Explains how practices become normalized/routinized | VERIFY / MAINTAIN | More useful as a diagnostic lens than as a lightweight solo workflow. citeturn888662search9turn888662search5 |
| **COM-B / Behavior Change Wheel** | Behavior = capability × opportunity × motivation | MAP: diagnose why adoption might fail | Good for human behavior; less direct for agentic/RAG changes unless translated into triggers, affordances, and feedback. citeturn506458view3 |
| **BCT Taxonomy** | Forces specific active ingredients: prompts, feedback, self-monitoring, action planning, etc. | MAP → PROMOTE implementation recipe | Useful to avoid vague “I should do X” adoption notes. citeturn506458view4 |
| **N-of-1 / single-case experimental design** | Baseline → intervention → repeated measures → phase comparison | TEST and TRIAL | Best rigor for one user, but can be too heavy for low-risk workflow tweaks. citeturn794661view4turn731471view3 |
| **Lean Build–Measure–Learn** | Minimum viable test; validated learning; actionable metrics | TEST design and thresholding | Startup language is useful, but can encourage speed over measurement quality. citeturn794618search0turn794618search3 |
| **Toyota Kata / Kaizen** | Current condition → target condition → obstacles → experiment | MAP and TEST | Strong fit for daily process improvement; weaker for knowledge artifacts unless converted into observable workflow changes. citeturn794618search8turn794618search26 |
| **LLM eval practice** | Offline evals, online traces, regression tests, quality gates | TEST → PROMOTE → VERIFY | Very relevant for agent/RAG changes; immature for human habit adoption. citeturn261709view0turn261709view1turn261709view2 |

**Verdict:** the cleanest evidence-backed ADOPT spine is:

> **PDSA for the loop**  
> **Proctor / RE-AIM for implementation metrics**  
> **COM-B / BCT for human behavior design**  
> **N-of-1 / SCED for single-user testing**  
> **LLM evals for agent/RAG regression and verification**

---

## 2. The right unit of adoption is not an “idea”; it is a changed mechanism

An external alpha should not be promoted as a quote, note, or vague principle. It needs to be converted into one or more of these implementation units:

| Implementation unit | Example | Best consumer |
|---|---|---|
| **Rule** | “Every promoted note field must have a query or agent consumer.” | Human checklist, linter, review agent |
| **Skill** | Agent skill for literature triage or RAG answer verification | LLM agent |
| **Hook** | Pre-commit check, daily-review trigger, template validator | Automation |
| **Field** | `consumer`, `success_metric`, `drop_rule`, `last_verified` | Obsidian/RAG dashboard |
| **Rubric** | Evaluation criteria for generated synthesis quality | LLM judge + human spot check |
| **Eval case** | Historical query/task that failed before | Regression harness |
| **Dashboard query** | “Show promoted items unused in 30 days” | Human operator |
| **Checklist step** | “Before adopting, run no-op check.” | Human workflow |

If an alpha cannot be expressed as a changed mechanism, it stays as **knowledge**, not **process**.

---

## 3. Stickiness is measurable

“Stickiness” means the change survives after novelty, friction, and memory decay. For ADOPT, measure it at three layers.

### Layer 1 — opportunity-level adherence

Define a **trigger opportunity**:

> “A situation occurred where the adopted practice should have fired.”

Then log:

```text
adherence = actual_uses / trigger_opportunities
```

Example:

```text
Alpha: Add contradiction scan before publishing synthesis.
Trigger opportunity: Any final research answer >1,500 words.
Actual use: Contradiction scan was run.
Adherence: 8 / 10 = 80%.
```

This prevents fake adoption where the rule exists but is never invoked.

### Layer 2 — fidelity

Fidelity asks whether the adopted practice was used **as intended**.

```text
fidelity = completed_core_steps / required_core_steps
```

Example:

```text
Contradiction scan core steps:
1. Extract major claims.
2. Identify conflicting source claims.
3. Mark uncertainty.
4. Add caveat or decision.

If only 2 of 4 happen, fidelity = 50%.
```

This matters because implementation science distinguishes failed implementation from failed intervention. citeturn506458view1

### Layer 3 — maintenance / automaticity

For human habits, the habit literature suggests that automaticity rises gradually and varies widely. Lally et al. found a large range in time to automaticity, often much longer than popular “21-day” claims; the Self-Report Behavioral Automaticity Index was developed as a short way to track whether a behavior feels automatic. citeturn723276search4turn391488view4

For a personal knowledge system, use both telemetry and subjective automaticity:

| Measure | Human process | Agent/RAG process |
|---|---|---|
| **D+7 retention** | Did I still use it after one week? | Did the agent still invoke it after one week? |
| **D+14 retention** | Does it appear in real work without special prompting? | Does it appear in traces without manual reminder? |
| **D+30 retention** | Has it become default? | Is it part of the default tool/skill/eval path? |
| **Decay** | Are missed opportunities increasing? | Are failed invocations increasing? |
| **Automaticity** | Weekly 4-item SRBAI-style self-rating | Invocation rate without explicit user prompt |

---

## 4. Single-user thresholds should be practical, not pseudo-statistical

For a solo operator, p-values will often be overkill or misleading unless the workflow produces many repeated observations. The better approach is to pre-register:

1. **Minimum valuable effect**
2. **Guardrail limits**
3. **Minimum opportunity count**
4. **Adherence threshold**
5. **Drop rule**

This resembles product-experiment practice: define the smallest effect worth detecting and guardrail tolerances in advance. Spotify’s experimentation guidance frames minimum detectable effect as the smallest effect stakeholders care about and guardrails as tolerated degradation limits. citeturn731471view2

A practical ADOPT threshold template:

```yaml
keep_if:
  success_delta: ">= 20% improvement OR >= 1 point on 5-point quality rubric"
  adherence_last_window: ">= 70%"
  fidelity_core_steps: ">= 80%"
  guardrails: "no severe breach"
  consumer_uses: ">= 3 real uses or 100% of rare opportunities"

drop_if:
  severe_guardrail_breach: true
  adherence_last_window: "< 50%"
  no_named_consumer: true
  no_measurable_benefit_after_min_opportunities: true
  maintenance_cost_exceeds_benefit: true

adapt_if:
  outcome_signal: positive
  adherence_or_fidelity: weak
  likely_failure_type: "implementation failure, not idea failure"
```

Recommended probation windows:

| Change frequency | Minimum test/probation window | Better criterion |
|---|---:|---|
| Daily workflow | 7–14 days | ≥10 trigger opportunities |
| Multiple times/week | 2–4 weeks | ≥8 trigger opportunities |
| Weekly workflow | 4–6 weeks | ≥6 trigger opportunities |
| Monthly/rare workflow | 2–3 months | ≥3 real uses |
| Agent/RAG eval change | Immediate offline test + 1–4 weeks online monitoring | Pass regression set + no production trace degradation |
| High-risk reversible change | ABAB or multiple-baseline if practical | ≥5 observations per phase, repeated effect |

Single-case design standards often recommend repeated measurements across phases, with stronger designs using multiple demonstrations of effect and at least several data points per phase. citeturn794661view4turn731471view3

---

## 5. “Already implemented” should be a first-class outcome

A common mistake is treating every good idea as something to add. ADOPT needs a **no-op adoption** state:

> **CONFIRMED_EXISTING** = the system already implements the active mechanism, has a trigger, has a consumer, and has verification evidence.

Use this decision table:

| Finding | Status | Action |
|---|---|---|
| Principle already exists, has trigger, has consumer, has test | **CONFIRMED_EXISTING** | Link evidence; optionally add regression case |
| Principle exists as passive note only | **PARTIAL** | Add trigger and consumer |
| Principle exists in human memory only | **PARTIAL** | Convert to checklist/rule/skill |
| Similar principle exists but lacks active ingredient | **DELTA_ADOPT** | Implement only the missing mechanism |
| Existing implementation has no consumer | **BLACK_HOLE** | Either assign consumer or retire |
| Existing implementation has consumer but no eval | **NEEDS_VERIFICATION** | Add regression / telemetry |

This is where your “named consumer” constraint is very strong. It converts “I built a thing” into “something actually consumes this thing.”

---

## 6. Prior art for human + LLM agents + PKM/RAG exists, but it is fragmented

I did not find a mature, direct “idea → test → probation → implementation → verified consumer” system for personal PKM + agents. What exists are partial substitutes:

| Category | Examples | What they cover | What they miss |
|---|---|---|---|
| **Capture / read-it-later / highlight ingestion** | Readwise → Obsidian | Alpha intake and export | No adoption testing or process verification. Readwise emphasizes syncing highlights and metadata into Obsidian. citeturn112469view3 |
| **Obsidian RAG/chat** | Obsidian Copilot, RAG Chat | Chat with vault, source-grounded retrieval | Mostly retrieval/QA, not implementation governance. citeturn352040view2turn112469view2 |
| **Personal AI knowledge assistants** | Khoj, AnythingLLM, NotebookLM | RAG over docs, web, notes, agents/research | Useful substrate, but not ADOPT gates. citeturn352040view3turn112469view1turn112469view0 |
| **Typed PKM objects** | Tana supertags | Fields/templates for structured notes | Good for alpha/adoption cards, but not testing by itself. citeturn112469view4 |
| **Agent skills** | Claude Agent Skills | Package reusable workflows as filesystem skills with instructions/code/resources | Strong PROMOTE substrate, but needs external eval and adoption governance. citeturn352040view0turn352040view1 |
| **LLM eval platforms** | LangSmith, Ragas, Promptfoo, OpenAI/Anthropic eval patterns | Regression, traces, graders, quality gates | Strong for agents/RAG; weak for human habit/process adoption. citeturn261709view0turn261709view1turn261709view2turn261709view3turn261709view4 |

The emerging pattern is clear: **PKM tools capture and retrieve; agent platforms execute; eval tools verify; but ADOPT must orchestrate them.**

---

# C. Contradictions & Debates

## 1. Fidelity vs adaptation

Implementation science has a long-running tension: should you preserve the original practice exactly, or adapt it to local context? Strict fidelity protects against cargo-cult distortion, but local adaptation is often necessary for sustainability. The practical compromise is:

```text
Preserve the core mechanism.
Adapt the interface, trigger, tooling, and cadence.
```

For ADOPT:

```yaml
core_mechanism:
  must_preserve:
    - "active ingredient"
    - "target behavior"
    - "success condition"

adaptable_surface:
  can_change:
    - "template wording"
    - "agent prompt"
    - "dashboard location"
    - "frequency"
    - "automation level"
```

## 2. Rigor vs speed

PDSA favors quick, small-cycle learning. Single-case experimental design favors stronger repeated-measure evidence. Both are valid, but for different stakes.

| Risk / cost | Recommended rigor |
|---|---|
| Low-risk note-template tweak | One PDSA cycle + quick rollback |
| Medium workflow change | Baseline + intervention + adherence tracking |
| Expensive automation | Historical replay + shadow mode + live trial |
| High-risk reasoning change | Regression suite + human review + online monitoring |
| Health/finance/legal impact | Do not rely on casual ADOPT; use domain-grade evidence and expert review |

## 3. Deterministic checks vs LLM judgment

Your constraint is correct: use deterministic checks wherever possible. But semantic workflows still need some judgment.

Recommended hierarchy:

1. **Static checks** — required fields, links, schema validity.
2. **Unit tests** — known input → expected output.
3. **Regression evals** — historical cases.
4. **Metric checks** — latency, cost, retrieval precision, adherence.
5. **LLM-as-judge** — only where semantic quality cannot be captured deterministically.
6. **Human spot check** — sampled, especially after model or prompt changes.

Promptfoo’s CI/CD framing is useful here: fail builds on quality thresholds, security scans, and regressions. LangSmith’s offline/online eval model is also directly applicable. citeturn261709view2turn261709view0

## 4. Statistical significance vs practical significance

For a solo operator, “statistically significant” is often the wrong default. Better question:

> “Would I pay the complexity cost to keep this change?”

Use **minimum valuable effect** and **guardrail degradation limits** instead of pretending every personal experiment is a powered RCT.

## 5. Habit formation is not a fixed 21-day window

Habit automaticity varies widely. A short probation can detect obvious failures, but it does not prove long-term normalization. Use staged verification:

```text
TRIAL pass: it works under attention.
D+14 pass: it survives early novelty decay.
D+30 pass: it is probably integrated.
D+90 pass: it is part of the system or should be retired.
```

---

# D. Best Practices

## 1. Use an ADOPT card with hard gates

Every candidate alpha should become a structured adoption record.

```yaml
id: ADOPT-2026-07-07-001
title: "Contradiction scan before publishing research synthesis"
status: mapped # intake|mapped|testing|trial|promoted|verified|dropped|confirmed_existing|retired

source_alpha:
  origin: "external article / person / paper / transcript"
  claim: "High-quality research synthesis should explicitly surface contradictions."
  evidence_grade: "medium"

consumer:
  name: "final-answer composer"
  call_site: "research_synthesis_workflow.md"
  required: true

mechanism:
  principle: "Detect conflicting claims before final recommendation."
  target_behavior: "Run contradiction scan on major claims."
  trigger: "Any final research output >1500 words or >10 sources."
  output_artifact: "Contradictions & Debates section."

existing_coverage:
  status: none # none|partial|confirmed_existing
  evidence_links: []

test:
  design: "historical replay + live PDSA"
  baseline_cases:
    - "DR26-07-01"
    - "DR26-07-03"
  success_metric: "missed_major_contradictions"
  minimal_valuable_effect: ">=50% fewer missed contradictions"
  guardrails:
    max_extra_time: "10 minutes"
    max_extra_tokens: "3000"
  deterministic_checks:
    - "final answer contains Contradictions & Debates section"
    - "each contradiction has cited evidence"
  llm_judge_rubric: "rates whether major opposing evidence was surfaced"

trial:
  window: "14 days or 10 trigger opportunities"
  adherence_threshold: ">=70%"
  fidelity_threshold: ">=80%"
  friction_threshold: "<=2/5"

promotion:
  implementation_type: "workflow rule + agent skill + answer template"
  files:
    - "workflows/research_synthesis.md"
    - "agents/researcher/SKILL.md"
  regression_tests:
    - "evals/research_contradiction_scan.yaml"

verification:
  d7_check: null
  d30_check: null
  consumer_use_count: 0
  last_verified: null

rollback:
  drop_rule: "Drop if adherence <50% after 10 opportunities or no quality gain."
  declined_reason: null
```

## 2. Separate idea quality from implementation quality

During TEST/TRIAL, classify failure:

| Failure type | Meaning | Correct response |
|---|---|---|
| **Bad alpha** | Mechanism does not improve anything | DROP |
| **Bad mapping** | Idea was translated into wrong behavior/tool | Remap |
| **Bad test** | Metric did not capture intended value | Redesign test |
| **Bad implementation** | Good idea, poor trigger/interface | Adapt |
| **Bad consumer** | Implemented artifact has no user | Assign consumer or retire |
| **Context mismatch** | Works elsewhere, not in your system | DROP or store as contextual |
| **Overhead failure** | Benefit real but maintenance cost too high | Simplify or drop |

This is the personal-system version of distinguishing intervention failure from implementation failure. citeturn506458view1

## 3. Make “consumer-required” enforceable

Promotion should fail automatically if no consumer exists.

```python
def can_promote(adopt_card):
    required = [
        adopt_card["consumer"]["name"],
        adopt_card["consumer"]["call_site"],
        adopt_card["test"]["success_metric"],
        adopt_card["test"]["minimal_valuable_effect"],
        adopt_card["rollback"]["drop_rule"],
        adopt_card["promotion"]["implementation_type"],
        adopt_card["promotion"]["regression_tests"],
    ]
    return all(bool(x) for x in required)
```

Examples of valid consumers:

| Artifact | Valid consumer |
|---|---|
| Note field | Dataview query, dashboard, RAG retriever, agent prompt |
| Agent skill | Named agent and eval suite |
| Checklist step | Daily/weekly review workflow |
| Rule | Linter or review template |
| Prompt | Specific task route |
| RAG metadata | Retrieval filter or ranking logic |
| Declined registry entry | Future duplicate detector |

If the consumer cannot be named, the alpha is not ready for promotion.

## 4. Use historical replay for LLM/RAG changes

For agent/RAG changes, never rely only on subjective “this feels better.” Use:

1. **Frozen historical tasks**
2. **Baseline output**
3. **Candidate output**
4. **Deterministic checks**
5. **LLM/human quality rubric**
6. **Regression threshold**
7. **Trace inspection**

LangSmith describes offline evals using curated datasets, historical traces, synthetic cases, and human/code/LLM evaluators; Anthropic and OpenAI both emphasize traces, task-level grading, tool calls, and final outcome state for agent evaluation. citeturn261709view0turn261709view3turn261709view4

## 5. Promote as code, skill, rule, or schema — not prose

Promotion should create at least one durable artifact:

```text
/rules/
/skills/
/hooks/
/templates/
/evals/
/dashboards/
/declined-registry/
/adoption-cards/
```

Recommended promotion mapping:

| Alpha type | Promotion target |
|---|---|
| Mental model | Decision checklist or rubric |
| Writing/research practice | Answer template + eval rubric |
| Agent behavior | Skill file + regression evals |
| Retrieval improvement | Metadata field + retrieval test |
| Review habit | Daily/weekly checklist hook |
| Error-prevention idea | Linter or preflight check |
| Strategic principle | Decision record + recurring review |

Agent Skills are especially relevant prior art: Anthropic describes skills as reusable filesystem-based resources with instructions, code, and references that agents load when relevant. citeturn352040view0turn352040view1

---

# E. Failure Modes

| Failure mode | Symptom | Root cause | Guardrail |
|---|---|---|---|
| **Cargo-cult adoption** | You copy the surface ritual but not the mechanism | Mechanism not extracted | Require `core_mechanism` and `target_outcome` fields |
| **Alpha hoarding** | Huge vault of “valuable” ideas, little process change | Capture mistaken for implementation | HARVEST and ADOPT must be separate |
| **Too many changes at once** | Cannot tell what helped or hurt | Confounded trials | WIP limit: max 1–3 active trials |
| **No baseline** | “Seems better” but no comparison | No counterfactual | Require baseline or historical replay |
| **No named consumer** | Field/rule/skill exists but is unused | Black-hole implementation | Promotion gate fails without consumer |
| **No trigger** | You forget to use it | Behavior not connected to context | Require trigger opportunity definition |
| **No drop rule** | Bad ideas linger forever | Sunk-cost bias | Pre-register drop criteria |
| **Novelty bias** | Strong first week, then silent decay | Motivation spike, no system support | D+7/D+30/D+90 verification |
| **Metric gaming** | Metric improves but work quality does not | Bad proxy | Pair success metric with guardrails |
| **LLM judge drift** | Eval results change without real change | Model instability | Deterministic checks first; frozen eval set; human spot checks |
| **Regression blindness** | New skill improves one case, breaks others | No test suite | Add historical replay before promotion |
| **Schema bloat** | More fields, slower work | Every idea becomes metadata | Field must have consumer/query |
| **Over-automation** | System becomes hard to repair | Too many fragile hooks | Prefer simple deterministic checks |
| **Underspecified practice** | Inconsistent use | Vague behavior | Use BCT-style specification: who, when, where, how often, output |
| **Implementation failure misread as idea failure** | Good principle dropped after bad interface | No fidelity/adherence tracking | Track adoption, fidelity, feasibility separately |
| **Stale promoted rules** | Old practices remain after context changes | No maintenance review | Quarterly retire/supersede sweep |

---

# F. Strategic Recommendations

## 1. Adopt this refined ADOPT v1

Use this state model:

```text
0. INTAKE
   From HARVEST. Candidate alpha exists.

1. MAP
   Translate alpha into mechanism, target behavior, consumer, trigger, and expected outcome.
   Run no-op / existing-coverage check.

2. TEST
   Test on real historical data or a small live slice.
   For LLM/RAG: offline eval + regression.
   For human workflow: baseline/intervention or PDSA.

3. TRIAL
   Time-boxed probation in real daily work.
   Track adherence, fidelity, friction, outcome, guardrails.

4. PROMOTE
   Implement as rule, skill, hook, field, template, eval, or checklist.
   Add regression test and rollback rule.
   Assign named consumer.

5. VERIFY
   Confirm the consumer actually uses it.
   Check D+7, D+30, D+90 retention.
   Keep, adapt, retire, or supersede.
```

Terminal statuses:

```text
DROP
CONFIRMED_EXISTING
SUPERSEDED
RETIRED
```

## 2. Treat CAPTURE as HARVEST, not ADOPT

Your HARVEST engine already captures and judges alpha. ADOPT should begin only when an idea is:

```text
captured + judged valuable + assigned to adoption review
```

So the combined system is:

```text
HARVEST:
  detect → judge → review → home note

ADOPT:
  map → test → trial → promote → verify / drop
```

This prevents the two systems from blending into one vague “knowledge management” flow.

## 3. Make no-op checks mandatory

Before testing anything, ask:

```text
Does my current system already implement the active mechanism?
```

Use this checklist:

```yaml
no_op_check:
  same_mechanism_exists: true/false
  trigger_exists: true/false
  consumer_exists: true/false
  regression_or_usage_evidence_exists: true/false
  current_artifact_links: []
  decision: confirmed_existing | partial | new_adoption_needed
```

Decision rule:

```text
If mechanism + trigger + consumer + evidence exist:
  CONFIRMED_EXISTING

If mechanism exists but trigger/consumer/evidence missing:
  PARTIAL → implement missing piece only

If mechanism does not exist:
  NEW_ADOPTION
```

## 4. Use three evaluation modes

| Mode | Use when | Method |
|---|---|---|
| **Replay test** | Agent, prompt, RAG, retrieval, writing workflow | Run old vs new on historical cases |
| **Shadow test** | New process could affect output quality | Run new process in parallel without relying on it yet |
| **Live PDSA / N-of-1** | Human workflow or daily process | Baseline → intervention → repeated observations |

For RAG specifically, add retrieval metrics where possible:

```text
context_precision
context_recall
answer_correctness
citation_support
unsupported_claim_count
latency
token_cost
```

Ragas explicitly focuses on systematic evaluation loops for LLM/RAG systems, and LangSmith/Promptfoo provide similar offline/CI-style evaluation patterns. citeturn261709view1turn261709view0turn261709view2

## 5. Build a declined registry as an anti-reinfection system

DROP is not trash. It is a learning asset.

```yaml
declined_registry_entry:
  alpha_id:
  title:
  source:
  reason:
    - no_consumer
    - failed_test
    - too_much_overhead
    - already_implemented
    - weak_evidence
    - context_mismatch
  evidence:
  reconsider_after:
  duplicate_detection_keywords:
```

This prevents repeated re-adoption of seductive but previously failed ideas.

## 6. Add quarterly pruning

Every 90 days, run:

```text
Show promoted items with:
- no consumer use in 30 days
- no verification in 90 days
- failing regression tests
- fields with no query
- skills with no successful trace
- rules never triggered
```

Decision:

```text
KEEP / SIMPLIFY / MERGE / SUPERSEDE / RETIRE
```

This is the maintenance layer most personal PKM systems lack.

---

# G. Action Plan

## Phase 1 — Build the registry

Create an Obsidian folder:

```text
/ADOPT/
  /00-inbox/
  /10-mapped/
  /20-testing/
  /30-trial/
  /40-promoted/
  /50-verified/
  /90-declined/
  /99-retired/
```

Create one required template:

```yaml
---
type: adopt-card
status: intake
source_alpha:
consumer:
mechanism:
existing_coverage:
test:
trial:
promotion:
verification:
rollback:
created:
last_updated:
---
```

Add a dashboard:

```dataview
TABLE status, consumer.name, test.success_metric, trial.window, verification.last_verified
FROM "ADOPT"
WHERE type = "adopt-card"
SORT status ASC, last_updated DESC
```

## Phase 2 — Add promotion gates

Implement deterministic lint checks:

```text
Cannot PROMOTE unless:
- consumer.name exists
- consumer.call_site exists
- mechanism.trigger exists
- test.success_metric exists
- test.guardrails exist
- rollback.drop_rule exists
- promotion.implementation_type exists
- promotion.regression_tests exist where relevant
- verification.review_date exists
```

For a non-engineer-repairable version, this can be an Obsidian checklist first; later it can become a script.

## Phase 3 — Create your first eval dataset

Start with 10 real historical cases:

```text
/evals/adopt/
  research_synthesis_001.yaml
  rag_answer_001.yaml
  daily_review_001.yaml
  writing_quality_001.yaml
```

Each case should contain:

```yaml
input:
expected_behavior:
baseline_output:
candidate_output:
deterministic_checks:
rubric:
guardrails:
```

This gives you the “test on my real data” layer.

## Phase 4 — Run three alpha candidates through ADOPT

Pick three different types:

1. One **human workflow** alpha  
2. One **LLM agent skill** alpha  
3. One **RAG / note-schema** alpha  

Run each through:

```text
MAP → TEST → TRIAL → PROMOTE or DROP
```

Do not run more than three simultaneous trials.

## Phase 5 — Add stickiness telemetry

Minimum viable telemetry:

```yaml
usage_log:
  date:
  trigger_opportunities:
  actual_uses:
  skipped_reason:
  fidelity_score:
  friction_score:
  outcome_score:
```

Weekly summary:

```text
adherence = uses / opportunities
fidelity = completed core steps / required steps
friction = subjective 1–5
benefit = subjective 1–5 or objective metric
```

D+7 / D+30 / D+90 checks:

```text
Did the named consumer use it?
Did it improve the target metric?
Did it create maintenance drag?
Should it be kept, simplified, merged, or retired?
```

## Phase 6 — Promote agentic changes as skills

For LLM-agent changes, use a filesystem-style skill structure:

```text
/skills/contradiction-scan/
  SKILL.md
  examples.md
  failure-modes.md
  tests.yaml
```

Promotion requirements:

```text
- skill has description and trigger
- examples exist
- failure modes exist
- eval cases pass
- traces show correct invocation
- consumer agent is named
```

This mirrors the emerging agent-skill pattern: reusable instructions, code, and references loaded by an agent when relevant. citeturn352040view0turn352040view1

---

# H. Sources

## Academic / scientific foundations

- **PDSA / Model for Improvement** — best source for small-cycle local testing before implementation. citeturn391488view0  
- **PDSA quality review** — systematic review work on whether PDSA is applied consistently and rigorously. citeturn888662search0  
- **Proctor implementation outcomes** — adoption, fidelity, feasibility, cost, sustainability, and related metrics. citeturn506458view1  
- **RE-AIM** — reach, effectiveness, adoption, implementation, maintenance. citeturn888662search3turn888662search7  
- **Normalization Process Theory** — how practices become embedded or normalized. citeturn888662search9turn888662search5  
- **COM-B / Behavior Change Wheel** — capability, opportunity, motivation as behavior-change drivers. citeturn506458view3  
- **Behavior Change Technique Taxonomy v1** — 93 specified behavior-change techniques for reproducible intervention design. citeturn506458view4  
- **Habit automaticity / SRBAI** — habit formation, automaticity, and short automaticity measurement. citeturn723276search4turn391488view4  
- **Single-case experimental design** — repeated within-person measurement, baseline/intervention phases, and design threats. citeturn794661view4turn731471view3  
- **Quantified Self / personal science** — self-experimentation and personal empirical methods. citeturn995690search1turn995690search2  

## Industry and product-practice foundations

- **Lean Startup / Build–Measure–Learn** — minimum viable testing and validated learning. citeturn794618search0turn794618search3  
- **Toyota Kata** — target condition, current condition, obstacles, experiments. citeturn794618search8turn794618search26  
- **Spotify experimentation guidance** — minimum detectable effect and guardrail degradation limits. citeturn731471view2  
- **Architecture Decision Records** — useful model for accepted/superseded/retired decision states. citeturn444575search0turn444575search4  

## LLM / RAG / agent evaluation

- **LangSmith evaluation docs** — offline evals, online evals, datasets, traces, human/code/LLM evaluators. citeturn261709view0  
- **Ragas** — systematic RAG evaluation loops and metrics. citeturn261709view1  
- **Promptfoo** — CI/CD-style LLM quality gates, regression testing, and threshold failures. citeturn261709view2  
- **Anthropic agent evaluation guidance** — task, trial, grader, transcript, tool use, outcome state. citeturn261709view3  
- **OpenAI agent evaluation guidance** — traces, graders, datasets, eval runs, regression/failure-mode analysis. citeturn261709view4  
- **Anthropic Agent Skills** — reusable filesystem-based agent capabilities with instructions/code/resources. citeturn352040view0turn352040view1  

## PKM / RAG / knowledge-tool prior art

- **Obsidian Copilot** — LLM chat and vault QA over Obsidian notes. citeturn352040view2  
- **RAG Chat Obsidian plugin** — local vector search/RAG, source-grounded note answers, BM25 indexing. citeturn112469view2  
- **Khoj** — personal AI over documents, web, Obsidian, semantic search, agents. citeturn352040view3  
- **AnythingLLM** — local/cloud RAG over documents with chunking, vector database, reranking. citeturn112469view1  
- **Readwise Official Obsidian plugin** — capture/export highlights into Obsidian with metadata customization. citeturn112469view3  
- **Tana supertags** — typed note objects with fields/templates, relevant for adoption-card schemas. citeturn112469view4  
- **NotebookLM Deep Research** — source-grounded research reports and notebook-based synthesis. citeturn112469view0

---

**Anton:**

@Deep research do deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"Web search # DR26-07-07-MACB-04 \u2014 ADOPT framework: testing & implementing external \"alpha\" into a personal knowledge system\n\nConduct a deep research investigation on the following topic.\n\nTOPIC:\nHow to rigorously TEST and IMPLEMENT external \"alpha\" \u2014 someone else's ideas, methods, mental models, or practices \u2014 into a PERSONAL knowledge system and daily working processes (a solo operator augmented by LLM agents + an Obsidian vault + RAG). Not org-scale change management \u2014 personal/small-team adoption. The goal is a repeatable framework that takes an idea from \"captured & judged valuable\" to \"tested on my real data \u2192 probation \u2192 implemented in my process \u2192 verified it actually improved the system (or dropped).\"\n\nCONTEXT:\nWe already have a HARVEST engine that mines alpha. The missing layer is ADOPT: map \u2192 test-on-real-data \u2192 probation \u2192 implement \u2192 verify. Validate and improve the proposed six-stage pipeline, answer the listed research questions, and produce the requested report with executive summary, findings, debates, best practices, failure modes, recommendations, action plan, and sources. Emphasize academic evidence, implementation science, LLM agent evaluation, PKM/RAG workflows, and practical implementation for a solo operator."}}
```
