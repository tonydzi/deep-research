# DR26-07-03-11 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Agent-Platform Optimization Validation"
type: ai-conversation
stage: raw
source: chatgpt

authored_by: hybrid
conversation_id: 6a482e55-d244-83ed-ae80-ae2264827439
model: "gpt-5-5-pro"
url: "https://приватный чат"
date_recorded: 2026-07-03
date_added: 2026-07-04
valid_as_of: 2026-07-03
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 2
related_concepts: []
revisit_if: ""
---

# Agent-Platform Optimization Validation

> ChatGPT · 2026-07-03 · `gpt-5-5-pro` · 2 сообщений

## Диалог

**Anton:**

Conduct a deep research investigation on the following topic.

# DR26-07-03-11 — Validation of 20 agent-platform optimization techniques + self-evolve loop

TOPIC:
Validation and portability assessment of 20 claimed agent-platform optimization techniques (routing, prompts, graph memory) plus a "self-evolve" loop (agent auto-research improving its own harness), for a SINGLE-USER, local-first, file-based agent stack (Claude Code + markdown vault + SQLite + local embeddings), operated by a non-technical owner.

CONTEXT (what already exists — do not re-recommend it, build beyond it):
- Our stack: Claude Code on Windows machines (always-on hub with 2 GPU + laptop + 2 Macs, Syncthing-synced), one ~128k-note Obsidian vault, e5+reranker RAG, SQLite fact DBs, deterministic-first token ladder (SQL/grep/RAG before LLM), static 2-tier model routing (cheap model for grunt work, top model for reasoning), multi-agent advocate/skeptic panels, cross-machine consensus engine, nightly scheduled routines, session ledger (TurnState) + graph-recall pilot over the vault.
- A practitioner (Monosnap General AI Lab) claims 19/20 implemented optimizations with self-reported A/B results, e.g.: Prompt Cache Preheating (-56.4% API cost), Hierarchical Graph Memory (+18.5% RAG recall), Structured JSON Constraints (100% syntax-error elimination), Dynamic Thompson Routing (-84.1% cost vs flat top-model), Input Context Pruning (-9.4%), CoT Token Suppression (-11.2%), Semantic Graph-Vector RAG (+10.8%), Self-Correction Loops (+11.5% compile), Few-Shot Dynamic Ingestion (+7.5%), Temporal Edge Tracking (+5.4% historical recall), Graph Entity Aging & Pruning (-4.8% context cost), Negative Prompting, Action-aware Actor Contours (tenant isolation), Fallback Chain Compression (-35% switch time), Pre-action Task Batching (-4.2%), Dynamic Response Length Caps (-2.9%), Conflict Resolution in Merging (+3.6% graph integrity), Role-based Specialization (+3.1%), Provenancing & Citations (100% node-session linkage). He also formalizes backlog selection as a Quadratic Knapsack Problem over a synergy/conflict hypothesis graph (N=200, RICE weights, effort budget), and runs multi-agent debate with Fleiss Kappa agreement + Borda count aggregation. His 4-tier cascade: cheap models (Gemini Flash/DeepSeek) for classification, mid (Claude Sonnet) for code synthesis, high (Claude Opus/Qwen Max) for verification/critique, top (Claude Fable) for hard architecture; a Thompson Sampler routes by live latency/failure metrics with cross-vendor failover on 429s.
- We already have separate deep-research briefs on: unified agent memory, codegraph memory, harness safety gates, negative-knowledge memory, model-fallback stacks. Do NOT re-cover those wholesale.

OBJECTIVE:
For each of the 20 techniques and the meta-practices (QKP backlog selection; Fleiss-Kappa/Borda debate; self-evolve auto-research loops; agent-to-agent (A2A) setup-sharing protocols; zero-trust agent infrastructure avoiding supply-chain risk in ecosystems like openclaw/Hermes): (a) find independent evidence — papers, benchmarks, reputable engineering posts — that the technique yields real gains and of what magnitude; (b) judge whether the claimed numbers are plausible or inflated baselines; (c) assess portability to a subscription-based Claude Code + files + SQLite stack (no custom inference server), incl. prompt-cache mechanics of the Anthropic API/subscription; (d) rank a shortlist worth implementing for a solo operator, with effort estimates and the simplest repairable implementation for each.

RESEARCH REQUIREMENTS:
1. Academic research — papers/benchmarks on: LLM router bandits (Thompson sampling, RouteLLM-class), prompt caching economics, KV-cache reuse, graph memory with temporal edges and entity aging (GraphRAG, Zep/Graphiti, HippoRAG arXiv 2405.14831, MemGPT/Letta), multi-agent debate aggregation (Borda, majority, kappa-weighted), self-improving agent loops (self-evolve, ADAS, AlphaEvolve-class, Ralph loops), portfolio/knapsack selection for R&D backlogs.
2. Industry practice — how leading agent products (Claude Code, Cursor, Devin, Trae, SWE-agent, Cline, AG2/AutoGen, LangGraph) implement routing, caching, context pruning, length caps, self-correction; published cost-reduction case studies with real numbers; tools from his own stack list: mem0, getzep/graphiti, LLMLingua, dspy, jsonformer, outlines, temporal.io, agno.
3. Competitive landscape — off-the-shelf components (routers, memory servers, token compressors like headroom, codebase-memory-mcp, OpenSage) vs building thin custom scripts.
4. Failure analysis — where dynamic routing degraded quality, where graph memories rotted or poisoned, where self-evolve loops diverged or overfit their own metrics (metric gaming), where A2A/shared-setup channels created supply-chain or prompt-injection risk.
5. Emerging trends — last 12 months: inference-time scaling for agents, agent-to-agent protocols (A2A, MCP-based sharing), zero-trust agent sandboxes, measured self-improvement pipelines.
6. Implementation recommendations — a ranked shortlist (top 5-7 of the 20+meta) SPECIFIC to the CONTEXT stack, each with: expected realistic gain for a solo subscription user, effort in days, simplest repairable implementation sketch, and what NOT to build (explicitly list which of the 20 are not worth it for us and why).

OUTPUT FORMAT:
A. Executive Summary
B. Technique-by-technique verdict table (evidence strength: established / emerging / speculative / marketing; realistic gain range; portable yes/no)
C. Contradictions & Debates
D. Best Practices
E. Failure Modes
F. Strategic Recommendations (ranked shortlist for our stack)
G. Action Plan (phased, solo-operator effort)
H. Sources

Do not ask clarifying questions — this brief is complete; start the research now.

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"Conduct a deep research investigation on the following topic.\n\n# DR26-07-03-11 \u2014 Validation of 20 agent-platform optimization techniques + self-evolve loop\n\nTOPIC:\nValidation and portability assessment of 20 claimed agent-platform optimization techniques (routing, prompts, graph memory) plus a \"self-evolve\" loop (agent auto-research improving its own harness), for a SINGLE-USER, local-first, file-based agent stack (Claude Code + markdown vault + SQLite + local embeddings), operated by a non-technical owner.\n\nCONTEXT (what already exists \u2014 do not re-recommend it, build beyond it):\n- Our stack: Claude Code on Windows machines (always-on hub with 2 GPU + laptop + 2 Macs, Syncthing-synced), one ~128k-note Obsidian vault, e5+reranker RAG, SQLite fact DBs, deterministic-first token ladder (SQL/grep/RAG before LLM), static 2-tier model routing (cheap model for grunt work, top model for reasoning), multi-agent advocate/skeptic panels, cross-machine consensus engine, nightly scheduled routines, session ledger (TurnState) + graph-recall pilot over the vault.\n- A practitioner (Monosnap General AI Lab) claims 19/20 implemented optimizations with self-reported A/B results, e.g.: Prompt Cache Preheating (-56.4% API cost), Hierarchical Graph Memory (+18.5% RAG recall), Structured JSON Constraints (100% syntax-error elimination), Dynamic Thompson Routing (-84.1% cost vs flat top-model), Input Context Pruning (-9.4%), CoT Token Suppression (-11.2%), Semantic Graph-Vector RAG (+10.8%), Self-Correction Loops (+11.5% compile), Few-Shot Dynamic Ingestion (+7.5%), Temporal Edge Tracking (+5.4% historical recall), Graph Entity Aging & Pruning (-4.8% context cost), Negative Prompting, Action-aware Actor Contours (tenant isolation), Fallback Chain Compression (-35% switch time), Pre-action Task Batching (-4.2%), Dynamic Response Length Caps (-2.9%), Conflict Resolution in Merging (+3.6% graph integrity), Role-based Specialization (+3.1%), Provenancing & Citations (100% node-session linkage). He also formalizes backlog selection as a Quadratic Knapsack Problem over a synergy/conflict hypothesis graph (N=200, RICE weights, effort budget), and runs multi-agent debate with Fleiss Kappa agreement + Borda count aggregation. His 4-tier cascade: cheap models (Gemini Flash/DeepSeek) for classification, mid (Claude Sonnet) for code synthesis, high (Claude Opus/Qwen Max) for verification/critique, top (Claude Fable) for hard architecture; a Thompson Sampler routes by live latency/failure metrics with cross-vendor failover on 429s.\n- We already have separate deep-research briefs on: unified agent memory, codegraph memory, harness safety gates, negative-knowledge memory, model-fallback stacks. Do NOT re-cover those wholesale.\n\nOBJECTIVE:\nFor each of the 20 techniques and the meta-practices (QKP backlog selection; Fleiss-Kappa/Borda debate; self-evolve auto-research loops; agent-to-agent (A2A) setup-sharing protocols; zero-trust agent infrastructure avoiding supply-chain risk in ecosystems like openclaw/Hermes): (a) find independent evidence \u2014 papers, benchmarks, reputable engineering posts \u2014 that the technique yields real gains and of what magnitude; (b) judge whether the claimed numbers are plausible or inflated baselines; (c) assess portability to a subscription-based Claude Code + files + SQLite stack (no custom inference server), incl. prompt-cache mechanics of the Anthropic API/subscription; (d) rank a shortlist worth implementing for a solo operator, with effort estimates and the simplest repairable implementation for each.\n\nRESEARCH REQUIREMENTS:\n1. Academic research \u2014 papers/benchmarks on: LLM router bandits (Thompson sampling, RouteLLM-class), prompt caching economics, KV-cache reuse, graph memory with temporal edges and entity aging (GraphRAG, Zep/Graphiti, HippoRAG arXiv 2405.14831, MemGPT/Letta), multi-agent debate aggregation (Borda, majority, kappa-weighted), self-improving agent loops (self-evolve, ADAS, AlphaEvolve-class, Ralph loops), portfolio/knapsack selection for R&D backlogs.\n2. Industry practice \u2014 how leading agent products (Claude Code, Cursor, Devin, Trae, SWE-agent, Cline, AG2/AutoGen, LangGraph) implement routing, caching, context pruning, length caps, self-correction; published cost-reduction case studies with real numbers; tools from his own stack list: mem0, getzep/graphiti, LLMLingua, dspy, jsonformer, outlines, temporal.io, agno.\n3. Competitive landscape \u2014 off-the-shelf components (routers, memory servers, token compressors like headroom, codebase-memory-mcp, OpenSage) vs building thin custom scripts.\n4. Failure analysis \u2014 where dynamic routing degraded quality, where graph memories rotted or poisoned, where self-evolve loops diverged or overfit their own metrics (metric gaming), where A2A/shared-setup channels created supply-chain or prompt-injection risk.\n5. Emerging trends \u2014 last 12 months: inference-time scaling for agents, agent-to-agent protocols (A2A, MCP-based sharing), zero-trust agent sandboxes, measured self-improvement pipelines.\n6. Implementation recommendations \u2014 a ranked shortlist (top 5-7 of the 20+meta) SPECIFIC to the CONTEXT stack, each with: expected realistic gain for a solo subscription user, effort in days, simplest repairable implementation sketch, and what NOT to build (explicitly list which of the 20 are not worth it for us and why).\n\nOUTPUT FORMAT:\nA. Executive Summary\nB. Technique-by-technique verdict table (evidence strength: established / emerging / speculative / marketing; realistic gain range; portable yes/no)\nC. Contradictions & Debates\nD. Best Practices\nE. Failure Modes\nF. Strategic Recommendations (ranked shortlist for our stack)\nG. Action Plan (phased, solo-operator effort)\nH. Sources\n\nDo not ask clarifying questions \u2014 this brief is complete; start the research now."}}
```
