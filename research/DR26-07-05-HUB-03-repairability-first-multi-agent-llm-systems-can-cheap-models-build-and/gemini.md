# DR26-07-05-HUB-03 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!--
source: Google Gemini Deep Research (Pro), gemini.google.com, account почтовый ящик (Punk)
DR: DR26-07-05-HUB-03 — Repairability-first multi-agent systems: cheap models as builders and repairers
date: 2026-07-05, report completed ~18:50 machine time


url: https://приватный чат
note: report body verbatim below; Gemini's interactive research-log ("Thoughts") and the "Sources read but not used" tail live in the chat URL above. Tables converted from DOM text flow.
-->

# Repairability-First Design of Multi-Agent LLM Systems: Model Allocation, Entropy, and Chaos Engineering

## A. Executive Summary

The rapid commoditization of Large Language Models (LLMs) has fractured the software engineering landscape into a dichotomy of capabilities: frontier models that exhibit advanced reasoning but incur prohibitive operational costs, and local or "workhorse" models that offer unparalleled unit economics but suffer from constrained contextual awareness. For organizations deploying always-on, multi-agent AI systems, balancing these tiers is a matter of existential viability. A prominent architectural hypothesis posits that utilizing cheaper models for the initial construction of an agent system organically results in simpler, more robust artifacts. This theory operates on the assumption that restricted capabilities naturally yield restricted complexity, analogizing that an artifact built by a fundamental tool can be effortlessly repaired by that same tool. By extension, systems built by cheap models should theoretically be fully maintainable and repairable by those same low-cost or local models.

However, empirical analysis and production telemetry suggest a profound paradox: limited-capacity models do not inherently produce simplicity. Instead, they frequently produce unmaintainable technical debt due to an inability to execute repository-scale, multi-objective optimization. Simplicity in autonomous software artifacts is not an organic byproduct of low intelligence; rather, it is the result of high-level reasoning applied to extreme, deterministic constraints. A frontier model forced into a highly restrictive architectural specification will generate an artifact that is vastly more maintainable than a weak model operating without boundaries.

Despite this inversion of the builder paradigm, the secondary component of the hypothesis—that a system is only truly maintainable if its weakest components can repair it—represents a state-of-the-art methodology for continuous integration in agentic workflows. When weaponized as a "Repair Drill" or Quality Gate, the weakest-model protocol serves as an adversarial user-acceptance test. If a local, heavily quantized model with zero historical context cannot successfully diagnose and repair a broken component, the component's architecture is objectively too complex and its specifications too opaque.

This report provides an exhaustive investigation into the repairability-first design of multi-agent LLM systems. It evaluates code complexity metrics tailored to LLM perception, maps the degradation of autonomous agents through the Entropy Principle, dissects the catastrophic unit economics of runaway agentic loops, and outlines a comprehensive architectural framework for allocating model tiers across the build, operate, and maintain lifecycle. The analysis synthesizes academic benchmarks, industry telemetry, and emerging chaos engineering practices to provide actionable methodologies for organizations operating multi-agent fleets under strict resource constraints.

## B. Key Findings

### LLM-Perceived Code Complexity and Semantic Nonlinearity

The traditional software engineering paradigm relies on metrics such as Cyclomatic Complexity, Halstead Complexity, and the Maintainability Index to gauge the readability and sustainability of a codebase. However, extensive benchmarking demonstrates that these classical execution-centric metrics exhibit no statistically reliable correlation with LLM performance once the confounding factor of code length is isolated. Code snippets possessing identical cyclomatic complexity scores frequently elicit drastically different performance success rates from LLMs, indicating a fundamental mismatch between human-perceived structural complexity and model-perceived difficulty.

The difficulty an LLM faces when generating, modifying, or repairing code stems directly from semantic nonlinearity. Because LLMs operate on linear token sequences via distributed representations, structural discontinuities—such as deep hierarchical nesting, asynchronous callbacks, and broad branching fan-outs—degrade probabilistic inference. To accurately quantify this phenomenon, researchers have developed the Large Model Code Complexity (LM-CC) metric, which measures the entropy-guided compositional hierarchy of a program. Data indicates a profound negative correlation (Spearman's r ranging from −0.92 to −0.97) between LM-CC and LLM comprehension. When architectures are rigidly constrained to lower their LM-CC—preserving semantics while flattening hierarchies—downstream repair success rates (pass@1) by LLMs improve by up to 20.9%. Consequently, artifacts generated for multi-agent systems must be explicitly designed to minimize semantic nonlinearity to ensure they remain comprehensible and repairable by lower-tier models.

### The Mechanics and Efficacy of Iterative Self-Repair

The capability of smaller, open-weight models to execute code repair has advanced significantly, invalidating earlier industry assumptions that autonomous self-repair required massive scale or highly specialized fine-tuning paradigms. In evaluated benchmarks across both open-weight and proprietary ecosystems, modern instruction-tuned models at the 8-billion parameter scale demonstrate a base capacity to iteratively repair broken code using prompt-based instructions alone. The success of small-model repair is, however, heavily dependent on the nature of the fault and the prompting strategy utilized.

A Chain-of-Thought (CoT) repair strategy, wherein the model explicitly reasons through the stack trace prior to generating a patch, yields substantial improvements over minimal prompting, adding up to 5.5 percentage points of repair gain for highly capable models. Yet, error resolution rates vary dramatically by exception type. Syntax errors and name errors (e.g., uninitialized variables) are resolved by smaller models at rates exceeding 77%, whereas assertion and logic errors—where the code executes perfectly but produces an invalid state—are repaired at rates hovering around 45%. This disparity arises because logic failures require deep semantic reasoning that is rarely surfaced in a standard execution trace. Crucially, the vast majority of achievable repair gains (76% to 95%) are captured within the first two iterative attempts. Allowing an agent to loop beyond three retry attempts yields negligible marginal benefit and rapidly inflates computational waste.

Table 1: Comparative success rates of iterative self-repair across model scales.
| Model | Tier/Params | Repair Gain (HumanEval Δ) | Repair Gain (MBPP Δ) | Tokens per Percentage Point |
|---|---|---|---|---|
| Llama 3.1 8B | 8B | +9.8 pp | +16.0 pp | 19.9K |
| Llama 4 Scout | 17B (16E) | +14.0 pp | +17.9 pp | 11.1K |
| Qwen 3 | 32B | +4.9 pp | +17.5 pp | N/A |
| Llama 3.3 70B | 70B | +10.4 pp | +23.0 pp | 10.8K |
| Gemini 2.5 Flash | N/A | +9.8 pp | +30.0 pp | N/A |
| Gemini 2.5 Pro | N/A | +17.1 pp | +25.7 pp | N/A |

### Advanced Debugging Frameworks and Dynamic Trace Inspection

To overcome the limitations of raw outcome-level failure symptoms, state-of-the-art repair frameworks have transitioned from static prompt reflection to dynamic trace inspection. Frameworks such as DebugRepair and InspectCoder introduce simulated debugging environments directly into the agentic loop. Rather than feeding a raw, noisy stack trace to the model, these systems perform static program slicing to extract the minimal semantic subset related to the failure. The LLM is then directed to autonomously insert instrumentation (e.g., print statements or breakpoints) along key execution paths.

By gathering intermediate runtime evidence, models operating within these frameworks significantly outperform vanilla repair attempts. For example, integrating simulated debugging allows baseline models to correctly fix up to 51.3% more bugs across established benchmarks like Defects4J and HumanEval. This highlights a critical operational insight: weak models fail at repair not solely due to a lack of reasoning capacity, but due to a lack of state visibility. When provided with dynamic, intermediate execution states, the diagnostic capabilities of cheap models are drastically amplified.

### Total Cost of Ownership and Unit Economics

The financial sustainability of multi-agent systems is dictated by a rigid Total Cost of Ownership (TCO) equation that extends far beyond the initial development phase. Industry telemetry reveals that integration and orchestration account for 45% to 65% of the total build cost, overshadowing the direct costs of model API consumption during development. However, once deployed into production, the unit economics invert. Annual maintenance costs consistently represent 20% to 30% of the initial build cost, while the true year-one TCO often reaches 1.5 times the headline development price due to continuous API consumption, prompt drift, and required integration upkeep.

The economic burden is exacerbated by the architectural nature of agents. Unlike traditional software where compute costs per user approach zero at scale, generative AI products incur linear or exponentially scaling inference costs. A single multi-agent workflow typically requires 5 to 20 discrete LLM calls per task. If the system is not aggressively optimized through semantic caching, intent routing, and bounded retry budgets, the cost to resolve a user query can easily exceed the lifetime value of the user, rendering the deployment financially unviable.

Table 2: Estimated development and operational costs for AI agent architectures.
| Agent Architecture | Typical Build Cost | Monthly Run Cost | Primary Cost Drivers |
|---|---|---|---|
| Single-Purpose Custom Agent | $1,500 – $5,000 | $300 – $800 | API consumption, prompt maintenance |
| Multi-Agent Workflow | $5,000 – $25,000 | $1,000 – $3,000 | Orchestration overhead, cross-agent calls |
| Enterprise Custom Build | $75,000 – $300,000 | $1,500 – $8,000 | Compliance, vector DB ops, complex routing |

## C. Contradictions & Debates

### The Spaghetti Code Paradox: Builder Capacity vs. Artifact Simplicity

A central architectural debate revolves around the origin of simplicity in code generated by LLMs. The prevailing hypothesis among many agile teams posits that cheap, constrained models act akin to novices, building simple, localized tools that can easily be repaired by similar models. Conversely, frontier models are viewed as experts that build opaque, overly abstracted architectures that only they can decipher.

Extensive empirical evaluations categorically contradict this analogy. When deployed in repository-scale, multi-objective optimization tasks, smaller open-weight models (in the 20B to 100B parameter range) struggle profoundly with holistic context management. Rather than producing clean, simple code, they frequently output brittle "spaghetti" code characterized by logic redundancies, failure to utilize existing architectural patterns, and an inability to foresee downstream side effects. The production of simplicity in software is not the organic result of limited cognitive capacity; rather, it is the result of rigorous intelligence applying compression and abstraction to solve complex problems elegantly.

A frontier model forced to operate under extreme, deterministic constraints—such as strict directives to avoid external libraries, utilize single-file architectures, and reject nested inheritance—will reliably produce an artifact with a significantly lower LM-CC score than a cheap model operating without constraints. Consequently, the optimal allocation of model tiers dictates that expensive frontier models must design the blueprint and synthesize the constrained code, while cheap models are relegated to executing the sub-tasks, verification, and routine maintenance within those rigid boundaries.

### Multi-Turn Refinement Degradation

While small models demonstrate high competence in single-turn bug resolution, they suffer from catastrophic performance degradation during multi-turn refinements. Structured evaluations reveal that the functional correctness of code generated by models such as Llama 3.1 8B decreases by up to 69.2% when subjected to follow-up refinement instructions.

Additive change requests and logic-level refinements rapidly overwhelm the limited context processing capabilities of smaller models. As the model attempts to resolve new errors or add features, it inevitably overwrites or corrupts previously functioning logic, leading to a cascading failure of the module. This phenomenon establishes a hard limit on the autonomy of cheap models. They function exceptionally well as single-shot diagnostic tools or verifiers within a bounded repair drill, but they cannot be trusted to execute complex, multi-step evolutionary refactoring of an agentic codebase without human intervention or frontier-model supervision.

### The Illusion of Automated LLM Judges

Relying on one language model to evaluate the output of another creates a dangerous epistemological echo chamber. Automated LLM judges frequently share the exact cognitive biases, blind spots, and hallucination vectors of the agents they are tasked with evaluating. A final output may appear syntactically correct and receive high confidence scores from an LLM judge, yet completely mask broken intermediate reasoning or fabricated tool calls.

Ground-truth evaluation must rely on deterministic state changes rather than semantic approval. Frameworks operating in "coverage-exhaustion mode" address this by abandoning LLM judges in favor of unbeatable tests and regression oracles. An evaluation is only valid if it measures concrete state deltas—such as database modifications, successful compilation, or verified API responses—ensuring that the agent cannot optimize for a positive semantic review by hallucinating mock data.

### The Memory Curse and Context Rot

The assumption that expanding context windows solves agent memory issues is fundamentally flawed. In decentralized multi-agent topologies, "Context Rot" occurs when sequential chains compress earlier messages, eroding information fidelity with each handoff. If shared memory infrastructure is not utilized, early critical context vanishes, leading to conformity bias. In this failure mode, an agent aligns with a hallucinated fact introduced upstream, reinforcing the error at every subsequent hop until false consensus effectively locks in the deviation.

## D. Best Practices

### Dynamic Routing and Model Cascades

Model routing frameworks dynamically dispatch traffic by classifying the implicit difficulty of an incoming query, ensuring that premium compute is strictly reserved for high-stakes, ambiguous, or multi-step reasoning tasks. Implementing a semantic or machine-learning classifier routing layer adds a negligible 50 to 100 milliseconds of latency overhead, yet allows 60% to 80% of traffic to be safely directed to cost-efficient workhorse models (e.g., Mistral 7B, Llama 3.1 8B, Qwen 2.5 7B). This traffic split can reduce total operational inference costs by 40% to 85% without a visible drop in answer quality.

The cascading pattern sequentializes this failover mechanism. A query is first routed to a cheap model. If the model's output confidence falls below an empirically calibrated threshold, or if it explicitly triggers an "early abstention" mechanism to signal uncertainty, the request gracefully escalates to the next tier. Dedicated platforms like LiteLLM, Portkey, and Langfuse formalize this process by offering programmable fallback chains, circuit breakers, and load-balancing algorithms.

Table 3: Latency overhead and cost reduction across routing strategies.
| Routing Strategy | Latency Overhead | Primary Use Case | Cost Reduction Potential |
|---|---|---|---|
| Rule-Based (Regex) | < 1 ms | Keyword matching, strict intents | Low to Medium |
| Embedding-Based | ~ 5 ms | Semantic similarity, cached responses | High (via cache hits) |
| ML Classifier | 50 - 100 ms | Ambiguity detection, complexity scoring | Maximum (up to 85%) |
| LLM-as-Router | 500 - 2000 ms | Complex orchestration | Negative (adds inference cost) |

### Physical Integrity Gates vs. Memory Gates

Memory Gates consist of rules, constraints, and operational procedures encoded within the LLM's system prompt. These are highly susceptible to context window erosion and cognitive decay, wherein the agent forgets operational rules after multiple conversational turns.

Physical Gates, conversely, are deterministic, external code structures—such as Python try/except blocks, strict JSON schema validators, and continuous integration pipeline triggers—that operate outside the LLM's probabilistic execution path. The Irreversible Protection Principle dictates that any operation causing an irreversible state change (e.g., committing code, dropping a database table, executing a financial transaction) must be secured by a Physical Gate, never solely by a Memory Gate.

### Self-Healing Orchestration and Infrastructure as Context

When an agent encounters an infrastructure failure, pure semantic reasoning is insufficient because the LLM lacks visibility into the underlying hardware or network state. The best practice is to provide "infrastructure as context". By executing the core agent loop in a general-purpose language like Python, infrastructure failures (e.g., Out of Memory errors, missing dependencies, API timeouts) are surfaced as structured, inspectable exceptions. When these exceptions are passed directly back into the agent's inner loop, the model can explicitly reason about the failure and provision more memory, update a dependency, or swap a tool, rather than blindly retrying the same flawed action.

### Agentic Chaos Engineering and Fault Injection

To definitively validate maintainability and repairability, systems must undergo Agentic Chaos Engineering. Tools such as tumult-agentic operationalize this by inserting a local reverse proxy between the AI agent and the upstream model provider. This infrastructure intercepts traffic and injects behavioral faults directly into the execution pathway. Test scenarios include truncating JSON mid-stream to simulate parser failures, contaminating the retrieval context to test if the agent blindly accepts false premises, and simulating third-party API degradation through artificial latency. These proxies utilize behavioral contracts (e.g., retry_budget, graceful_error, no_secret_leakage) to objectively score the agent's resilience.

## E. Failure Modes

### The Entropy Principle and Silent Failures

The most profound threat to the maintainability of multi-agent systems is not acute software crashing, but the gradual, undetectable accumulation of disorder, codified as the Entropy Principle. LLM agent systems operating without external deterministic constraints inevitably experience "silent failures"—unexpected deviations from intended behavior that produce zero error signals. The accumulation of system entropy S(t)=S0·e^(αt) grows exponentially; without external intervention, entropy eventually exceeds the reliability threshold regardless of initial system quality.

Key silent failure classifications:
- Channel Fracture: progressive decay of information fidelity across agent-to-agent boundaries. Information loss in cross-agent memory injection routinely exceeds 30% within five communication hops, despite each agent reporting a successful transaction.
- Cognitive Framework Lag (CFL): operational rules and naming conventions established early in a session are inconsistently applied later. Rule adherence degrades sharply (from 94% to 61%) as session length increases.
- Data Consistency Decay: insidious divergence between recorded state and actual state in agent-managed pipelines; consistency rates can drop to 23.5% in pipelines exceeding ten hops.

Table 4: Taxonomy of silent failures.
| Failure Mode | Lifecycle Layer | Severity | Key Observable Characteristic |
|---|---|---|---|
| Channel Fracture | L1 (Transmission) | High | >30% info loss across 5 agent handoffs |
| Cognitive Framework Lag | L2 (Memory) | High | Rule adherence drops from 94% to 61% over time |
| Data Consistency Decay | L3 (Execution) | Medium | Aggregate pipeline output systematically corrupted |
| Knowledge Fragmentation | L2 (Memory) | Medium | Agents operate with locally correct, globally invalid views |
| Behavior Routing Deficiency | L3 (Execution) | Low | Suboptimal task allocation compounding over iterations |

### Token Tsunamis and the Inversion of Unit Economics

In an autonomous agent system, an unhandled edge case often triggers a "Token Tsunami". When an agent encounters an unrecognizable data format or a downed third-party API, a poorly governed orchestrator will attempt to resolve the issue by feeding the error back into its context window and retrying the action. Without strict circuit breakers and physical integrity gates, a standard query that should consume 800 tokens ($0.008) can easily explode into 50,000 tokens ($0.50) of wasted retries and context reloads. At scale, these 3x to 7x cost multipliers destroy gross margins — a primary catalyst for the projected failure of up to 80% of application-layer AI startups.

### Model Deprecation and API Churn

Organizations that tightly couple their agent architectures to specific capabilities or prompt quirks of a single frontier model face severe existential risk due to accelerated API deprecation schedules ("Killed by AI" phenomenon; providers frequently retire model snapshots quarterly). Defensive data moats and the ability to seamlessly swap model endpoints via abstraction layers are required to survive this rapid churn.

## F. Strategic Recommendations

1. Establish the "Repair Drill" as a Continuous Quality Gate. Integrate it into CI/CD as an adversarial user-acceptance test: the chaos engine intentionally injects a localized fault (deleting a critical configuration parameter, altering a variable name, modifying a JSON payload structure) into the newly generated artifact. A localized, cheap model (e.g., Llama 3.1 8B running locally) with access to the stack trace and the specific file is dispatched to resolve the error. If the local model cannot generate a passing patch within a strict limit of two iterations, the underlying codebase fails the maintainability check.

2. Invert the Builder Paradigm: Constraint-Driven Spec Generation. Frontier models design the system blueprints and synthesize the core code under extreme, deterministic constraints (standard libraries only, single-file architectures, no nested inheritance, explicit type hinting). Once the frontier model generates this low LM-CC artifact, workhorse models take over routine execution, verification, and maintenance.

3. Implement Agentic Site Reliability Engineering (SRE):
   - Explicit Error Budgets: maximum permissible token spends and retry limits per task; on ceiling → graceful degradation to a human operator or structured failure.
   - Decouple Memory from Reasoning: deterministic shared state machine (LangGraph state persistence, Redis) instead of the context window as state manager; failed agents resume from checkpoints.
   - Continuous Chaos Testing: fault-injecting proxies continuously validate behavioral contracts against simulated latency, malformed context, retrieval poisoning.

## G. Action Plan (2-person company, subscription budgets)

Phase 1: Establish Deterministic Guardrails and Routing (Weeks 1-2)
- Extract Deterministic Logic: decouple the LLM from all operations that can be handled deterministically (regex, stdlib, SQL). Reserve LLM inference exclusively for probabilistic reasoning.
- Deploy a Gateway Router (LiteLLM, Portkey): strict fallback chains, classifier-based routing of routine traffic to local/cheap workhorse models.
- Define the Specification Surface: per-agent strict spec file with absolute architectural rules (no nested abstractions, pure functions only).

Phase 2: Deploy the "Repair Drill" Quality Gate (Weeks 3-4)
- Construct the Adversarial Verifier: local 8B-class model (Llama 3.1 8B / Qwen 2.5 Coder 7B) dedicated to the repair drill.
- Integrate the Chaos Harness (e.g., tumult-agentic or lightweight Python wrapper) mutating variables / truncating JSON in a subset of integration tests.
- Enforce Pass/Fail Criteria: CI injects a deterministic error → local model prompted with stack trace + isolated file → must output a valid patch passing the state-delta test within two rounds → fail = PR rejected, frontier model simplifies the module until it passes.

Phase 3: Financial Operations and Entropy Control (Weeks 5-6)
- Circuit Breakers: hard token limits per task, strict retry budgets (loop >3 → terminate + log).
- Semantic Caching: vector-backed intent cache.
- Monitor the Entropy Constant: track Cost per Successful Outcome; P95/median cost ratio >2-3x = agents trapped in exploratory loops → tighten specs.

## H. Sources used in the report (selected)

- Rethinking Code Complexity Through the Lens of Large Language Models (arXiv) — LM-CC metric
- How Many Tries Does It Take? Iterative Self-Repair in LLM Code Generation Across Model Scales and Benchmarks (arXiv)
- DebugRepair: Enhancing LLM-Based Automated Program Repair via Self-Directed Debugging (arXiv)
- InspectCoder: Dynamic Analysis-Enabled Self Repair through Interactive LLM-Debugger Collaboration (arXiv)
- Silent Failure in LLM Agent Systems: The Entropy Principle and the Inevitable Disorder of Autonomous Agents (arXiv 2606.08162)
- The Kitchen Loop: User-Spec-Driven Development for a Self-Evolving Codebase (arXiv + GitHub 0xagentkitchen/kitchenloop)
- CodeChat-Eval: Evaluating LLMs in Multi-Turn Code Refinement Dialogues (arXiv) — 69.2% degradation
- FormulaCode: Evaluating Agentic Optimization on Large Codebases (arXiv)
- Code for Machines, Not Just Humans: Quantifying AI-Friendliness with Code Health Metrics (arXiv)
- tumult.rs — Chaos-Test Your AI Agent: Fault Injection for Claude Code, Codex, Copilot & OpenCode
- Applying Site Reliability Engineering to Autonomous AI Agents (Microsoft Community Hub)
- How to Build Self-Healing Agents (Union.ai); A Self-Healing Framework for Reliable LLM-Based Autonomous Agents (arXiv 2605.06737)
- LiteLLM / Portkey / Langfuse docs (fallbacks, routing, load balancing)
- LLM routing: n8n blog, Digital Applied, tianpan.co, FrugalGPT (nexos.ai), RouteLLM (Medium)
- Cost data: Codeloop, Services Ground, Neontri, Gaper.io, V7 Labs, GetVocal (LangChain TCO)
- Unit economics: Amit Kothari, TechAhead (Inference Cost Explosion), Loot Drop meta-study, IdeaProof (AI startup failures), Killed by AI graveyard
- Galileo AI / Redis — Why Multi-Agent LLM Systems Fail

(Full source list including "sources read but not used" and Gemini's step-by-step research log: see the chat URL in the header.)
