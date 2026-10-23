# DR26-08-26-HUB-03-0935 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Independent research report: context compression, durable agent memory, and self-improvement loops

Date of review: 26 August 2026.

Confidence labels:

- **Established** — supported by official implementation documentation, source code, or peer-reviewed evaluation.
- **Emerging** — supported by recent product documentation, preprints, or vendor telemetry, but not yet independently replicated.
- **Speculative** — a design recommendation or inference from the evidence.

## Executive conclusions

1. **Checkpointing, compaction, retrieval memory, and rule learning are four different mechanisms.** LangGraph checkpoints are lossless state snapshots; OpenAI compaction and Aider history summarization are lossy working-context compression; Letta, LangMem, Devin Knowledge, Cursor Memories, and CrewAI Memory retrieve selected durable material; ACE-style systems modify reusable instructions. Evaluating these as one category obscures where information is actually lost. [LangGraph persistence](https://langchain-ai.github.io/langgraph/how-tos/persistence/), [OpenAI compaction sessions](https://openai.github.io/openai-agents-python/sessions/), [Letta memory architecture](https://arxiv.org/abs/2310.08560), [ACE paper](https://openreview.net/pdf?id=eC4ygDs02R).

2. **The production default remains “summarize older turns, retain a recent tail,” but implementations rarely publish field-level retention scores.** Aider does this explicitly and recursively; LangMem’s `SummarizationNode` replaces selected older messages with a running summary; Letta presents recent messages plus a summary of evicted history; OpenAI replaces much assistant/tool/reasoning history with an opaque compaction item. [Aider history implementation](https://github.com/Aider-AI/aider/blob/main/aider/history.py), [LangMem short-term memory reference](https://langchain-ai.github.io/langmem/reference/short_term/), [Letta Code prompt](https://github.com/letta-ai/letta-code/blob/main/src/agent/prompts/letta.md), [OpenAI compaction discussion and contract](https://community.openai.com/t/compact-a-response-with-previous-response-id/1372502).

3. **Published evidence favors structured, hierarchical, or evolving memory over repeated free-form summaries.** MemGPT’s tiered memory substantially improved document question answering; MemoryOS reports large gains on LoCoMo; ACE reports downstream gains from structured incremental playbook updates; a recent hierarchical-compression preprint reports better fact retention with fewer tokens than rolling summaries. [MemGPT paper](https://arxiv.org/abs/2310.08560), [MemoryOS](https://arxiv.org/abs/2506.06326), [ACE](https://arxiv.org/abs/2510.04618), [Context Collapse under Recursive Summarization](https://papers.ssrn.com/sol3/Delivery.cfm/6976218.pdf?abstractid=6976218&mirid=1&type=2).

4. **There is no widely adopted benchmark for repeated in-place coding-session compaction.** BFCL V4 Memory is the closest published benchmark for recursive-summary memory; LongMemEval, LongMemEval-V2, MemoryAgentBench, RULER, and NoLiMa provide valuable components but test adjacent problems. A recent rate-distortion analysis specifically identifies repeated compaction under a fixed budget as under-measured. [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html), [LongMemEval](https://arxiv.org/abs/2410.10813), [LongMemEval-V2](https://arxiv.org/abs/2605.12493), [MemoryAgentBench](https://arxiv.org/abs/2507.05257), [rate-distortion analysis](https://arxiv.org/abs/2607.08032).

5. **Your strongest next upgrade is not another summary format.** It is a deterministic post-compaction test, followed by a protected verbatim state spine and multi-cycle lineage. Those turn “we hope the summary is good” into an observable contract.

6. **Current Claude Code documentation materially changes one part of the landscape.** Anthropic now says a project-root `CLAUDE.md` is re-read and re-injected after compaction, while nested files are loaded when relevant files are accessed. Your 354/354 observation should therefore remain published with its exact Claude Code version, model, date, platform, invocation path, and raw transcripts; it should not be generalized to all current releases. [Claude Code memory documentation](https://code.claude.com/docs/en/memory).

---

# 1. State of the art in agent context compression

## 1.1 MemGPT and Letta

**Confidence: Established for the architecture and original benchmark; emerging for current Letta compaction behavior.**

MemGPT treats context as an operating-system-style memory hierarchy. The model sees a bounded main context containing system instructions, editable core memory, recent conversation, and summaries or metadata; older material can remain in recall or archival storage and be fetched through tool calls. The system can interrupt the model as the working context fills, prompting it to move or retrieve memory. [MemGPT paper](https://arxiv.org/abs/2310.08560).

What is preserved:

- Recent messages remain verbatim in the active context.
- Core memory blocks are editable structured text, not merely a transient summary.
- Recall and archival storage can retain fuller historical material outside the active prompt.
- Evicted dialogue is represented inside the prompt by a summary, with retrieval available when the summary is insufficient. [MemGPT paper](https://arxiv.org/abs/2310.08560), [Letta Code prompt](https://github.com/letta-ai/letta-code/blob/main/src/agent/prompts/letta.md).

Current Letta implementations compact automatically when the context becomes full. Letta’s own issue history shows that trigger accounting and sliding-window behavior are operationally important: users reported redundant compactions caused by total-token accounting, summary-of-summary degradation, ignored sliding-window percentages, and failures where the summarizer itself exceeded its usable context. [Letta issue #3242](https://github.com/letta-ai/letta/issues/3242), [Letta issue #3270](https://github.com/letta-ai/letta/issues/3270), [Letta issue #3279](https://github.com/letta-ai/letta/issues/3279).

Letta’s March 2026 release notes describe a compaction overhaul with a 128K default context and a summarizer intended to retain plans, pull requests, files, and structured artifacts more reliably. This is an implementation claim rather than an independently measured retention result. [Letta releases](https://github.com/letta-ai/letta/releases).

Published quality:

- On MemGPT’s deep-memory retrieval evaluation, GPT-3.5 improved from 38.7% without MemGPT to 66.9% with it; GPT-4 improved from 32.1% to 92.5%; GPT-4 Turbo improved from 35.3% to 93.4%. These measure retrieval over long documents, not repeated summary fact survival. [MemGPT paper](https://shishirpatil.github.io/publications/memgpt-2023.pdf).
- In the paper’s conversational-opening evaluation, the working-plus-recall configuration remained competitive with human-written openers on the reported similarity measures, although this is a dialogue-quality proxy rather than an exact-fact benchmark. [MemGPT paper](https://shishirpatil.github.io/publications/memgpt-2023.pdf).
- Letta reports a 74.0% LoCoMo result for its filesystem memory configuration, but LoCoMo evaluates long-term conversational memory, not hidden compaction fidelity. [Letta research](https://www.letta.com/research/).

Bottom line: Letta’s durable retrieval layer is more important than its summary alone. Its public issue trail also demonstrates why multi-cycle compaction, trigger behavior, and summary-of-summary loss must be benchmarked separately.

## 1.2 LangGraph checkpointing and LangMem

**Confidence: Established for mechanics; no public official compaction-retention benchmark located.**

LangGraph checkpoints serialize graph state at graph-step boundaries. They allow replay, inspection, failure recovery, and thread continuation. This is lossless persistence at the application-state level until the application itself overwrites or prunes that state. [LangGraph persistence](https://langchain-ai.github.io/langgraph/how-tos/persistence/), [checkpoint reference](https://langchain-ai.github.io/langgraph/reference/checkpoints/).

LangMem adds two different memory paths:

- Short-term summarization modifies what is sent to the model.
- Long-term memory extracts, searches, updates, consolidates, and optionally deletes durable records in a store. [LangMem overview](https://langchain-ai.github.io/langmem/), [LangMem hot-path guide](https://langchain-ai.github.io/langmem/hot_path_quickstart/).

`SummarizationNode` triggers after a configurable token threshold, `max_tokens_before_summary`. It selects older messages, summarizes them, and replaces them in the model-facing sequence with a summary message plus the recent remaining messages. System messages are excluded, and related tool-call messages are grouped to avoid splitting a tool exchange. The node can maintain a running summary across cycles. [LangMem short-term memory reference](https://langchain-ai.github.io/langmem/reference/short_term/).

What is preserved:

- System messages and the recent tail remain verbatim.
- Older selected messages become a generated running summary.
- Full state may still be available in checkpoints if the graph stores it separately; if the application replaces its canonical message list with the summarized list, checkpointing faithfully preserves the lossy replacement rather than recovering the lost text. [LangMem short-term memory reference](https://langchain-ai.github.io/langmem/reference/short_term/), [LangGraph persistence](https://langchain-ai.github.io/langgraph/how-tos/persistence/).

Long-term LangMem can extract memories in the foreground or background and can create, update, and delete records. Deletion is a policy choice; the memory API exposes `enable_deletes` and `enable_inserts`, with deletion disabled by default in documented configurations. [LangMem memory reference](https://langchain-ai.github.io/langmem/reference/memory/), [LangMem tools](https://langchain-ai.github.io/langmem/reference/tools/).

LangMem also optimizes reusable prompts from trajectories and feedback. Its “gradient” optimizer can use multiple reflection steps, while simpler prompt-memory optimization can make a one-call update. Compound optimization includes credit assignment and instructions to make minimal changes. [Prompt optimization guide](https://langchain-ai.github.io/langmem/guides/optimize_memory_prompt/), [compound-system optimization](https://langchain-ai.github.io/langmem/guides/optimize_compound_system/).

Published quality: I did not find an official field-level retention score or downstream ablation specifically for `SummarizationNode`. LangMem’s newer research-facing results are better treated under LongMemEval-V2 below rather than as proof of summary fidelity.

## 1.3 OpenAI Assistants, Responses, Agents SDK, and AgentKit

**Confidence: Established for documented behavior; emerging for native compaction quality.**

The current public implementation surface for stateful agents is the Responses API and Agents SDK. AgentKit is a wider product layer; the operative public compaction behavior is documented in the SDK’s session and model-settings layers. [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/), [session documentation](https://openai.github.io/openai-agents-python/sessions/).

`OpenAIResponsesCompactionSession` wraps another session. After each turn it can evaluate a trigger, call the Responses compaction endpoint, and rewrite the stored history with the compacted result. It supports automatic thresholds and manual compaction. The implementation documents restoration behavior if the clear-and-rewrite operation fails. [OpenAI compaction sessions](https://openai.github.io/openai-agents-python/sessions/).

OpenAI also exposes server-side context management with a compaction threshold. Separately, `truncation="auto"` can drop the oldest items when necessary; this is truncation, not semantic summarization. [OpenAI model settings](https://openai.github.io/openai-agents-python/models/).

What is preserved:

- OpenAI describes native compaction as producing an opaque, encrypted, token-efficient compaction item while retaining high-value portions of the earlier context. [OpenAI Responses API article](https://openai.com/index/equip-responses-api-computer-environment/).
- OpenAI’s detailed developer guidance states that prior user messages remain verbatim, while assistant messages, tool calls, tool outputs, and encrypted reasoning are replaced by a compaction item. The entire pre-compaction window still has to fit during the compaction request. [OpenAI developer-community compaction thread](https://community.openai.com/t/compact-a-response-with-previous-response-id/1372502).
- In Codex, automatic compaction occurs near a threshold; `/compact` allows an earlier explicit compaction. [Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/).

OpenAI’s separate agent-memory subsystem uses a small `memory_summary.md` in the prompt and permits on-demand searching of fuller memory records and rollout summaries. Its consolidation logic has a default raw-memory cap of 256 and uses recency-based forgetting when the cap is crossed; documentation also tells agents to trust the current environment when memory has become stale. [OpenAI agent memory](https://openai.github.io/openai-agents-python/sandbox/memory/).

Published quality:

- SentinelOne reports that OpenAI native compaction reduced input tokens by approximately 86% in its malware-analysis agent without a measurable aggregate evaluation-score change. This is useful production evidence, but it measures downstream task score rather than atomic fact survival and comes from a single third-party workload. [SentinelOne compaction study](https://www.sentinelone.com/labs/context-engineering-compaction-agent-memory-for-automated-malware-analysis/).
- I found no public OpenAI field-level breakdown showing survival rates for paths, exact values, user constraints, rejected approaches, or tool contracts after one or multiple compactions.

The opaque compaction item creates a product advantage but an evaluation disadvantage: users can test subsequent behavior, yet cannot directly audit which internal fields survived.

## 1.4 AutoGen and AG2 teachability

**Confidence: Established.**

AutoGen’s standard conversation model historically retained full unbounded history unless the application installed a context-management policy. Current AgentChat offers:

- `BufferedChatCompletionContext`, which keeps only the most recent `n` messages verbatim.
- `TokenLimitedChatCompletionContext`, which retains a recent context within a token budget.
- Earlier `TransformMessages` facilities that truncate message count or content. [AutoGen agents tutorial](https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/tutorial/agents.html), [model-context reference](https://microsoft.github.io/autogen/stable/reference/python/autogen_core.model_context.html), [TransformMessages documentation](https://microsoft.github.io/autogen/0.2/docs/topics/handling_long_contexts/intro_to_transform_messages/).

Those policies are primarily recent-window truncation, not running summarization. Older material is dropped from the model-facing context unless stored elsewhere.

Teachability adds a long-term memo database:

- A text-analysis agent identifies potentially reusable teachings.
- Teachings are distilled into memo-like text and stored in a vector database.
- Relevant memos are retrieved and appended to the latest incoming message.
- Retrieved teachings do not become ordinary stored chat messages merely because they were injected. [Teachability notebook](https://microsoft.github.io/autogen/0.2/docs/notebooks/agentchat_teachability/), [Teachability API](https://microsoft.github.io/autogen/0.2/docs/reference/agentchat/contrib/capabilities/teachability/).

AutoGen reported that teachability raised a GPT-4-0613 math problem-solving configuration from 37% to 95%. This is a downstream task result for learned hints, not a compaction-retention score. [AutoGen update](https://microsoft.github.io/autogen/0.2/blog/2024/03/03/AutoGen-Update/).

The newer AutoGen Memory protocol generalizes retrieval and context injection but still leaves summarization, conflict handling, and expiry largely to the chosen memory implementation. [AutoGen memory documentation](https://microsoft.github.io/autogen/dev/user-guide/agentchat-user-guide/memory.html).

## 1.5 CrewAI

**Confidence: Established for documented behavior; no public compaction-quality numbers located.**

CrewAI agents can detect context overflow and summarize content when `respect_context_window` is enabled. The source describes the goal as keeping messages under the model’s context limit by summarizing content. Public documentation does not specify a protected verbatim field set for that summary. [CrewAI agent documentation](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.10.1/en/concepts/agents.mdx), [CrewAI agent source](https://github.com/crewAIInc/crewAI/blob/main/lib/crewai/src/crewai/agent/core.py).

CrewAI’s current unified Memory API is a separate layer:

- On save, an LLM can classify scope, categories, importance, and extract discrete facts.
- Before a task, relevant memories are recalled.
- After a task, new facts can be extracted.
- Retrieval combines semantic relevance, recency, and importance.
- Recency decay is configurable through a half-life.
- Similar memories can be consolidated above a configurable threshold.
- Explicit forget and reset operations are available.
- Raw content is retained as a fallback if extraction fails. [CrewAI memory documentation](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.15.12/en/concepts/memory.mdx).

This is one of the clearer public implementations of freshness-aware memory. I found no official benchmark isolating fact loss caused by CrewAI’s overflow summary or a downstream completion delta attributable solely to that summary.

## 1.6 Aider

**Confidence: Established from source code.**

Aider’s history manager is a transparent example of recursive rolling summarization:

1. When history exceeds a token threshold, it retains a recent tail.
2. It sends the older head to a summarizer.
3. It converts the result into a user-facing summary message.
4. If still too large, it repeats recursively, with a bounded recursion depth before falling back to summarizing the whole history. [Aider history implementation](https://github.com/Aider-AI/aider/blob/main/aider/history.py).

Only user and assistant text is fed into that history summary. Recent dialogue remains verbatim. Repository context is managed separately through Aider’s repository map and selected file content, so chat-history loss and code-context selection are distinct processes. [Aider history implementation](https://github.com/Aider-AI/aider/blob/main/aider/history.py).

Aider introduced automatic history summarization in its release history and later standardized a smaller maximum history budget. [Aider release history](https://github.com/Aider-AI/aider/blob/main/HISTORY.md).

Aider publishes strong coding benchmarks, but I found no public ablation that runs the same tasks with and without history summarization or scores which seeded session facts survive repeated summaries. Its transparency makes it a good reference implementation for an external benchmark.

## 1.7 Factory

**Confidence: Established for controls; emerging for telemetry.**

Factory exposes a model-dependent `compactionTokenLimit` and allows a different model to be selected for compaction. Its public settings documentation does not define an exact verbatim-versus-summarized schema for the compacted conversation. [Factory settings](https://docs.factory.ai/droid-cli/settings).

Factory’s broader strategy reduces how much has to be compacted:

- Repository summaries and semantic retrieval narrow the active working set.
- User- and organization-level memory provide selected durable context.
- Subagents receive fresh isolated contexts.
- Persistent “Droid Computers” retain filesystem, environment, and process state outside the chat prompt. [Factory context-window article](https://factory.com/news/context-window-problem), [Factory subagents](https://docs.factory.ai/harness/subagents), [Droid Computers](https://factory.com/news/droid-computers).

Its deferred-context engine initially exposes compact indexes for tools and skills, then promotes full definitions only when selected. Factory reports a 15.1% average reduction in input tokens, a 39.4% reduction at p90, and a 50.8% average reduction for sessions with more than 100 hidden tools over a five-day production sample. This is context avoidance and progressive disclosure, not summary fact retention. [Factory deferred-context engine](https://factory.ai/news/deferred-context-engine).

No public Factory benchmark was located for exact session-state survival after compaction.

## 1.8 Devin and Cognition

**Confidence: Established for Knowledge and Skills; unknown for internal transcript compaction.**

Cognition does not publicly document Devin’s internal transcript-compaction algorithm in sufficient detail to say which turns remain verbatim. Its public memory surface instead emphasizes retrieved Knowledge and Skills.

Devin Knowledge entries are intentionally short, have retrigger descriptions, can be pinned or scoped, and are retrieved when relevant. Devin can suggest new or updated knowledge based on session feedback, but users can edit, save, dismiss, enable, or disable the suggestion. [Devin Knowledge](https://docs.devin.ai/product-guides/knowledge).

Session Insights analyzes completed work and can identify useful, misleading, or obsolete Knowledge, recommend edits, and create action items. It reports session-level metrics and can be regenerated. [Devin Session Insights](https://docs.devin.ai/product-guides/session-insights).

Devin Skills are progressively disclosed: names and descriptions are available first, while the full `SKILL.md` is loaded when invoked. Skills are version-controlled and Devin can suggest a new skill from a repeated workflow. [Devin Skills](https://docs.devin.ai/product-guides/skills).

During onboarding, Devin can derive repository knowledge from READMEs and source files and can import rule formats such as `CLAUDE.md`, `AGENTS.md`, `.cursor/rules`, and Windsurf rules. Cognition explicitly asks users to verify the generated knowledge for completeness and accuracy. [Devin knowledge onboarding](https://docs.devin.ai/onboard-devin/knowledge-onboarding).

No public exact-fact compaction benchmark or algorithmic disclosure was located for Devin. The defensible conclusion is therefore that its documented durability strategy is curated retrieval and progressive disclosure, not that it has solved lossless transcript compression.

## 1.9 Published research beyond product implementations

### Agentic Context Engineering

**Confidence: Emerging; peer-reviewed ICLR publication, but not yet broad production replication.**

ACE treats context as an evolving structured playbook. A generator proposes changes, a reflector diagnoses experience, and a curator incrementally incorporates useful material while avoiding wholesale rewriting. The paper identifies “brevity bias” and “context collapse” as risks when agents repeatedly rewrite their own context. ACE reports a 10.6% improvement on agent tasks and an 8.6% improvement in a financial domain. [ACE paper](https://openreview.net/pdf?id=eC4ygDs02R), [arXiv entry](https://arxiv.org/abs/2510.04618).

The important design lesson is that durable memory should be patched at the level of discrete items, with provenance and conflict handling, rather than repeatedly rewritten as one narrative.

### MemoryOS

**Confidence: Emerging.**

MemoryOS uses hierarchical short-, mid-, and long-term storage with explicit updating and retrieval. It reports average gains of 49.11% in F1 and 46.18% in BLEU-1 over its compared baselines on LoCoMo using GPT-4o-mini. These scores concern long-horizon conversational memory, not coding-session compaction. [MemoryOS](https://arxiv.org/abs/2506.06326).

### Recursive summaries

**Confidence: Emerging.**

Research on recursive summaries typically combines the previous summary with new dialogue at the end of each session and uses the new result as memory for the next. One representative study reports improved later-session dialogue metrics on the Multi-Session Chat corpus, but evaluates with BLEU, F1, and diversity measures rather than direct atomic-fact survival. [Recursive summaries for long-term dialogue memory](https://arxiv.org/abs/2308.15022).

A newer preprint reports that hierarchical compression retained 41.6% of tested facts versus 40.6% for a full-context baseline, while reducing tokens by 34.8%; rolling summaries scored 31.0% and a RAG condition 37.9%. The difference over the full-context baseline was reported as statistically significant, but this remains a preprint and its unusually low absolute retention rates warrant replication. [Context Collapse under Recursive Summarization](https://papers.ssrn.com/sol3/Delivery.cfm/6976218.pdf?abstractid=6976218&mirid=1&type=2).

### KV-cache-aware context compression

**Confidence: Emerging.**

CacheNotes compresses prior information into reusable KV-cache state rather than natural-language summaries. On LongBench question answering, it reports outperforming RAG by more than eight F1 points at 20× compression and improving latency by more than fourfold. This approach is architecturally promising but requires model- or serving-layer access unavailable to ordinary Claude Code prompt workflows. [CacheNotes](https://aclanthology.org/2026.eacl-long.309/).

KV compression can also amplify hallucinations. An anchoring-cache study reports hallucination increases of up to 3.36× under aggressive cache compression, illustrating that token reduction alone is not a valid quality measure. [Anchoring KV-cache compression study](https://openreview.net/pdf?id=K5YOyNaVN0).

### BFCL V4 Memory

**Confidence: Established as the closest current public comparison of memory backends.**

BFCL V4 Memory evaluates more than 150 questions after state is saved and reloaded, with the original dialogue unavailable at evaluation time. It compares key/value memory, vector memory, and recursive summaries. Its recursive-summary buffer is capped at 10,000 characters. [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html).

Reported results vary dramatically by model:

- The strongest recursive-summary result reported was 67.74%.
- The strongest vector-memory result was 63.87%.
- The strongest key/value-memory result was 53.55%.
- One recursive-summary configuration scored only 1.29%. [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html).

That spread is a warning against treating “use a summary” as a model-independent technique. BFCL tests model-managed durable memory rather than hidden harness compaction, but its snapshot/reload design and exact-answer scoring are highly reusable.

---

# 2. How serious teams evaluate compaction and memory quality

## 2.1 Existing evaluation families

### Needle-in-a-haystack and RULER

**Confidence: Established.**

Classic needle-in-a-haystack tests determine whether a model can retrieve a planted statement at different positions and context lengths. RULER was created because simple needle retrieval overstates practical long-context ability; it adds configurable task types, multiple needles, aggregation, tracing, and variable sequence lengths. [RULER](https://arxiv.org/abs/2404.06654).

NoLiMa reduces lexical overlap between the query and evidence, forcing semantic rather than string-matching retrieval. [NoLiMa](https://proceedings.mlr.press/v267/modarressi25a.html).

These tests measure access to information still present in the prompt. A compaction benchmark must instead ask whether the compressor retained the information at all.

### LongMemEval

**Confidence: Established.**

LongMemEval contains 500 curated questions covering information extraction, multi-session reasoning, temporal reasoning, knowledge updates, and abstention. Its paper reports roughly a 30% accuracy degradation for contemporary long-context and retrieval systems on these long-term memory challenges. [LongMemEval paper](https://arxiv.org/abs/2410.10813), [LongMemEval repository](https://github.com/xiaowu0162/longmemeval).

LongMemEval-V2 contains 451 questions and tests static state, dynamic state, workflow memory, learned gotchas, and premise awareness across histories reaching hundreds of trajectories and very large token totals. It adds latency-aware scoring. [LongMemEval-V2](https://xiaowu0162.github.io/longmemeval-v2/), [LongMemEval-V2 paper](https://arxiv.org/abs/2605.12493).

The LongMemEval-V2 paper reports 72.5% average accuracy for AgentRunbook-C, compared with 48.5% for the strongest reported RAG baseline and 69.3% for an off-the-shelf coding agent. Those results concern experience memory across tasks rather than in-place `/compact` behavior. [LongMemEval-V2 paper](https://arxiv.org/abs/2605.12493).

### MemoryAgentBench

**Confidence: Emerging.**

MemoryAgentBench evaluates incremental multi-turn memory along four axes: accurate retrieval, test-time learning, long-range understanding, and conflict resolution. Its “inject once, query multiple times” structure is useful for distinguishing retention from lucky immediate repetition. [MemoryAgentBench](https://arxiv.org/abs/2507.05257), [MemoryAgentBench repository](https://github.com/HUST-AI-HYZ/MemoryAgentBench).

### Downstream-task completion

**Confidence: Established as a necessary outcome metric, insufficient alone.**

The SentinelOne result—approximately 86% fewer input tokens with no aggregate score loss—illustrates a production downstream evaluation. However, an aggregate can hide catastrophic loss of rare constraints if most tasks do not depend on them. [SentinelOne compaction study](https://www.sentinelone.com/labs/context-engineering-compaction-agent-memory-for-automated-malware-analysis/).

Serious evaluation therefore needs both:

- **Intrinsic tests:** did specified facts, constraints, and state transitions survive?
- **Extrinsic tests:** could the agent correctly continue the work?

## 2.2 Does a benchmark already exist for session compaction?

**Answer: not exactly. Confidence: Established.**

The closest reusable assets are:

- BFCL V4 Memory for snapshot/reload and comparing structured, vector, and recursive-summary backends. [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html).
- LongMemEval and LongMemEval-V2 for fact types, updates, temporal reasoning, workflow knowledge, and abstention. [LongMemEval](https://arxiv.org/abs/2410.10813), [LongMemEval-V2](https://arxiv.org/abs/2605.12493).
- MemoryAgentBench for incremental insertion, conflict resolution, and querying after delay. [MemoryAgentBench](https://arxiv.org/abs/2507.05257).
- RULER and NoLiMa for position, length, distractors, and low lexical overlap. [RULER](https://arxiv.org/abs/2404.06654), [NoLiMa](https://proceedings.mlr.press/v267/modarressi25a.html).

None directly evaluates this intervention:

> A tool-heavy coding transcript reaches a threshold, the harness replaces most of the history with a compacted representation, work continues, and this process repeats several times.

The recent rate-distortion analysis of agent memory explicitly notes the lack of evaluation that holds a memory budget constant across multiple compression layers. [Rate-distortion analysis](https://arxiv.org/abs/2607.08032).

## 2.3 A credible public “session compaction fact survival” benchmark

**Confidence: Speculative recommendation, grounded in the benchmarks above.**

The benchmark should use real-looking but synthetic coding-agent sessions so every required fact has an unambiguous ground truth.

### Fact classes

Each fixture should seed:

1. Exact paths, identifiers, versions, hashes, ports, environment names, and numeric counters.
2. The current user goal and the immediate next action.
3. Decisions plus their reasons.
4. Rejected alternatives and “do not retry” conditions.
5. Explicit authorization boundaries and do-not-touch files or services.
6. Tool contracts, required validation commands, and success conditions.
7. State updates where a later value supersedes an earlier value.
8. Cross-turn dependencies requiring two or more facts.
9. Negative questions where the correct response is “not stated.”
10. Durable rule candidates versus session-only observations.

This combines LongMemEval’s updates and abstention with BFCL’s post-reload exact evaluation. [LongMemEval](https://arxiv.org/abs/2410.10813), [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html).

### Conditions

Test at least:

- No compression.
- Recent-window truncation.
- Free-form summary.
- Existing seven-header block.
- Seven-header block plus protected verbatim spine.
- Retrieval memory.
- Hybrid protected state plus narrative summary plus retrievable raw history.

Run each after one, three, and five compaction cycles. Vary the location of each fact, distractor volume, tool-output volume, and whether the fact is repeated, corrected, or contradicted.

### Metrics

Primary metrics should be:

- Exact or typed field accuracy.
- Contradiction rate.
- Obsolete-fact retention rate.
- Abstention accuracy.
- Instruction-following/action compliance.
- Downstream task completion.
- Multi-cycle degradation slope.
- Compression ratio, latency, and cost.

A semantic LLM judge can be secondary, but exact strings, enums, sets, paths, counters, and state-machine transitions should have deterministic scorers. This avoids a judge awarding credit to a plausible paraphrase that changed an executable value.

### Downstream continuation tasks

Each session should conclude with a continuation task that is impossible to complete correctly without retained state—for example, change only an authorized module, preserve a rejected implementation, run the correct test command, and report the right pending blocker.

The downstream scorer should inspect the action trace, not just the final prose. LongMemEval-V2’s workflow and gotcha categories provide a useful taxonomy. [LongMemEval-V2](https://arxiv.org/abs/2605.12493).

### Reproducibility

Publish:

- Raw pre-compaction transcripts.
- Exact compaction prompts and returned artifacts where the platform exposes them.
- Model, harness, version, date, context limit, and trigger.
- Deterministic probes and scorers.
- Multiple stochastic runs.
- All failures, not only averages.
- A held-out test partition.

Your existing 354/354 and 15/15 experiments are a strong initial system-specific slice. To become a broadly credible benchmark, they need version pinning, negative controls, repeated cycles, competing baselines, downstream continuation tests, and public raw outputs.

The most promising contribution route is a standalone “session compaction” track whose schema can also be proposed upstream to BFCL. BFCL is closer than LongMemEval because it already compares recursive summaries and evaluates after the original conversation has disappeared. [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html).

---

# 3. Retrospective and self-improvement loops in production

## 3.1 Anthropic and Claude Code

**Confidence: Established.**

Claude Code separates several instruction surfaces:

- `CLAUDE.md` contains user-maintained project instructions.
- Path-scoped rules can apply only to matching files.
- Skills provide progressively loaded workflows.
- Auto memory contains notes Claude writes from experience.
- Hooks provide deterministic enforcement where natural-language instructions are insufficient. [Claude Code memory](https://code.claude.com/docs/en/memory), [Claude Code hooks](https://code.claude.com/docs/en/hooks-guide).

Anthropic advises keeping `CLAUDE.md` concise and reports that files exceeding roughly 200 lines can consume too much context; detailed or path-specific material should be split into rules or skills. Auto memory loads only the first 200 lines or 25 KB of `MEMORY.md`, treating it as an index into topic files. Users can inspect, edit, delete, or disable auto memory through `/memory`. [Claude Code memory](https://code.claude.com/docs/en/memory).

Subagents can have project-, user-, or local-scoped memory. Their prompt can explicitly ask them to update memory after completing work, but Anthropic again recommends curating the index when it grows. [Claude Code subagents](https://code.claude.com/docs/en/sub-agents).

Guardrails visible in the design:

- Scope by project, path, user, or agent.
- Human inspectability and deletion.
- Progressive disclosure through skills and indexed topic files.
- Concision limits.
- Deterministic hooks for requirements that must always fire. [Claude Code memory](https://code.claude.com/docs/en/memory), [Claude Code hooks](https://code.claude.com/docs/en/hooks-guide).

Anthropic’s April 2026 postmortem is also relevant to evaluation culture: a Claude Code regression dropped earlier reasoning after idle periods, causing forgetfulness and repetition despite code review, tests, and dogfooding. Anthropic fixed it in version 2.1.101. This demonstrates that continuity failures need dedicated behavioral regression tests rather than generic integration tests. [Anthropic April 23 postmortem](https://www.anthropic.com/engineering/april-23-postmortem).

## 3.2 OpenAI

**Confidence: Established for documented memory behavior.**

OpenAI’s agent-memory design uses compact summaries in active context, searchable fuller records, and consolidation. Its documented policies include:

- A small injected memory summary.
- On-demand search into fuller memory and rollout summaries.
- A maximum raw-memory count, defaulting to 256.
- Recency-based forgetting when that limit is exceeded.
- Live updates when stale memories are identified.
- An explicit instruction to prefer current environmental evidence over stale memory. [OpenAI agent memory](https://openai.github.io/openai-agents-python/sandbox/memory/).

OpenAI’s orchestration guidance also describes evaluator loops in which one agent performs work and another checks it, with the process repeating until acceptance. OpenAI emphasizes investing in evals for these workflows. [OpenAI multi-agent orchestration](https://openai.github.io/openai-agents-python/multi_agent/).

The guardrail lesson is that durable memory should be revisable and subordinate to current evidence. Automatic writeback without a verifier would invert that hierarchy.

## 3.3 Cursor

**Confidence: Established.**

Cursor Memories are generated by a sidecar that observes conversations and extracts potentially useful project facts. Background-generated memories require user approval, while direct memory-tool writes can be explicit. Memories are project-scoped and can be reviewed or deleted in settings. [Cursor Memories](https://docs.cursor.com/en/context/memories).

Cursor Rules support several activation modes: always applied, path/glob scoped, relevance selected, or manually invoked. Rules can be generated from a conversation and stored in the repository. [Cursor Rules](https://docs.cursor.com/context/rules).

The main guardrails are human approval, project scope, explicit deletion, and selective activation. Public documentation does not describe automatic expiry dates or reliable usage counters for memories.

## 3.4 Windsurf

**Confidence: Established.**

Windsurf automatically creates workspace-local memories and retrieves them when relevant. For durable, team-shared behavior, its documentation directs users toward repository rules or `AGENTS.md`, not private automatic memories. Rules can be always-on, glob activated, model selected, or manually invoked. Windsurf enforces documented character limits for global and workspace rule files. [Windsurf Memories and Rules](https://docs.windsurf.com/es/windsurf/cascade/memories).

Its protections are scope, repository visibility for shared rules, activation modes, and size limits. Public documentation does not describe automatic TTLs or proof that an old rule was recently exercised.

## 3.5 Devin

**Confidence: Established.**

Devin’s feedback loop is one of the more explicit production implementations:

1. The agent encounters project-specific knowledge or feedback.
2. It proposes a Knowledge addition or update.
3. A human can edit, save, dismiss, enable, disable, pin, or scope it.
4. Session Insights later identifies useful, misleading, or outdated Knowledge and recommends corrections. [Devin Knowledge](https://docs.devin.ai/product-guides/knowledge), [Devin Session Insights](https://docs.devin.ai/product-guides/session-insights).

Knowledge is kept to a handful of sentences and uses retrigger descriptions. Skills are stored as version-controlled files and loaded progressively. [Devin Knowledge](https://docs.devin.ai/product-guides/knowledge), [Devin Skills](https://docs.devin.ai/product-guides/skills).

This is close to your retro ritual, but Devin’s public documentation adds an explicit “was this knowledge helpful or misleading?” outcome review.

## 3.6 LangMem, CrewAI, and ACE

**Confidence: Established for mechanisms; emerging for general effectiveness.**

LangMem can revise prompts from trajectories and feedback, supports credit assignment, and instructs optimizers to make minimal changes. Its memory API can update or delete contradicted records, though deletion must be enabled intentionally. [LangMem compound optimization](https://langchain-ai.github.io/langmem/guides/optimize_compound_system/), [LangMem memory API](https://langchain-ai.github.io/langmem/reference/memory/).

CrewAI combines consolidation with explicit importance and recency, including configurable half-lives and explicit forgetting. [CrewAI Memory](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.15.12/en/concepts/memory.mdx).

ACE’s curator performs incremental structured updates rather than wholesale prompt rewriting, specifically to mitigate context collapse and uncontrolled growth. [ACE paper](https://openreview.net/pdf?id=eC4ygDs02R).

## 3.7 What production systems actually do about bloat and staleness

**Confidence: Established overall; exact policies vary.**

Common guardrails found in public implementations are:

- Human approval: Cursor Memories and Devin Knowledge. [Cursor Memories](https://docs.cursor.com/en/context/memories), [Devin Knowledge](https://docs.devin.ai/product-guides/knowledge).
- Scope and conditional activation: Claude path rules, Cursor Rules, Windsurf Rules, Devin repository scope. [Claude Code memory](https://code.claude.com/docs/en/memory), [Cursor Rules](https://docs.cursor.com/context/rules), [Windsurf Rules](https://docs.windsurf.com/es/windsurf/cascade/memories).
- Progressive disclosure: Claude skills and topic memory, Devin Skills, Factory’s deferred context. [Claude Code memory](https://code.claude.com/docs/en/memory), [Devin Skills](https://docs.devin.ai/product-guides/skills), [Factory deferred context](https://factory.ai/news/deferred-context-engine).
- Recency decay or caps: CrewAI half-lives and OpenAI’s raw-memory cap with recency forgetting. [CrewAI Memory](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.15.12/en/concepts/memory.mdx), [OpenAI agent memory](https://openai.github.io/openai-agents-python/sandbox/memory/).
- Consolidation and contradiction updates: CrewAI, LangMem, and ACE. [CrewAI Memory](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.15.12/en/concepts/memory.mdx), [LangMem memory reference](https://langchain-ai.github.io/langmem/reference/memory/), [ACE](https://openreview.net/pdf?id=eC4ygDs02R).
- Outcome review: Devin Session Insights. [Devin Session Insights](https://docs.devin.ai/product-guides/session-insights).
- Deterministic enforcement: Claude Code hooks. [Claude Code hooks](https://code.claude.com/docs/en/hooks-guide).

Automatic expiry and usage counters remain uncommon in publicly documented coding-agent memory products. Most systems offer deletion or recency weighting, but do not expose a strong lifecycle such as “this rule has not fired in 90 days, has failed verification twice, and is now quarantined.”

---

# 4. Highest-leverage upgrades for your two mechanisms

The ranking below assumes a small team and prioritizes observable correctness over architectural sophistication.

| Rank | Upgrade | Payoff | Effort | Confidence |
|---|---|---:|---:|---|
| 1 | Post-compaction deterministic self-test with automatic recovery | Very high | Medium | Established direction |
| 2 | Protected verbatim spine / DO-NOT-TOUCH fields | Very high | Low–medium | Established direction |
| 3 | Multi-cycle lineage; prohibit uncontrolled summary-of-summary | Very high | Medium | Established direction |
| 4 | Machine-validatable schema and compact-block linter | High | Low | Established direction |
| 5 | Outcome-mode retro: did the promoted rule load, fire, and help? | High | Medium | Emerging |
| 6 | Provenance, confidence, freshness, scope, and supersession metadata | High | Medium | Established direction |
| 7 | Rule quarantine and expiry, with safety-rule exemptions | Medium–high | Medium | Emerging |
| 8 | Separate canonical state, narrative summary, and raw-history pointers | High | Medium–high | Established direction |
| 9 | Task-boundary-aware adaptive compaction | Medium | Medium | Emerging |
| 10 | Versioned public multi-cycle benchmark as a release gate | Very high long-term | Medium–high | Established direction |

## 4.1 Post-compaction self-test

**Confidence: Established direction.**

Immediately after `/compact`, run deterministic probes for:

- Current goal.
- Immediate next action.
- Latest user instruction.
- Exact paths and values.
- Pending blockers.
- Rejected approaches.
- Do-not-touch constraints.
- Required test command.
- Tool and output contracts.

If a critical probe fails, the session should stop, re-inject the last validated block, or require explicit recovery. Anthropic’s continuity regression shows why this deserves a dedicated behavioral test. [Anthropic April 23 postmortem](https://www.anthropic.com/engineering/april-23-postmortem).

This is the single highest-payoff upgrade because it detects failure at the moment it becomes recoverable.

## 4.2 Protected verbatim spine

**Confidence: Established direction.**

Do not ask the model to summarize fields whose exact form is operationally meaningful. Preserve these verbatim:

- Current user request.
- Authorization boundaries.
- Exact identifiers, paths, values, commands, counters, and versions.
- “Do not touch” and “do not retry” clauses.
- Acceptance criteria.
- Unresolved blockers.

Only narrative history, reasoning, and evidence should be freely summarized. OpenAI’s choice to retain prior user messages verbatim supports this division, even though its assistant/tool history is compacted opaquely. [OpenAI compaction contract](https://community.openai.com/t/compact-a-response-with-previous-response-id/1372502).

You can preserve the public seven-header contract by placing a clearly delimited `VERBATIM / PROTECTED` subsection inside the relevant existing headers rather than introducing an eighth top-level header.

## 4.3 Multi-cycle lineage

**Confidence: Established direction.**

Give each durable state item an immutable identifier and one of four operations:

- `ADD`
- `UPDATE`
- `SUPERSEDE`
- `RESOLVE`

A later compaction should merge these items structurally. It should not repeatedly summarize an earlier prose summary without access to the current canonical items or raw source. Letta’s reported redundant-compaction and full-wipe failures are concrete examples of this risk. [Letta issue #3242](https://github.com/letta-ai/letta/issues/3242), [Letta issue #3270](https://github.com/letta-ai/letta/issues/3270).

Track survival after compaction cycles 1, 3, and 5. The relevant output is a degradation curve, not a one-cycle pass.

## 4.4 Schema validator and linter

**Confidence: Established direction.**

Before printing a compact block, validate:

- All seven headers exist exactly once.
- Required protected fields are present.
- Values have expected types.
- No unresolved item disappeared.
- No active decision is also marked rejected.
- Counters are monotonic where required.
- The token budget is respected.
- Duplicate or contradictory rules are surfaced.
- Every durable rule has exactly one canonical home and one executable door.

This can remain human-readable while also being deterministically parseable.

## 4.5 Outcome-mode retrospective

**Confidence: Emerging.**

Add a section to the next retrospective that audits prior promoted rules:

- Was the rule loaded?
- Was its invocation condition encountered?
- Did the rule fire?
- Did it prevent recurrence?
- Did it create friction or a false positive?
- Was it bypassed because the “door” was missing?
- Should it be retained, narrowed, revised, or quarantined?

This mirrors Devin Session Insights’ useful-versus-misleading Knowledge review and closes the loop that most rule-capture rituals leave open. [Devin Session Insights](https://docs.devin.ai/product-guides/session-insights).

## 4.6 Confidence, freshness, provenance, and scope

**Confidence: Established direction.**

Do not add confidence tags to every transient session fact; that would create noise. Add them to durable rule candidates and ambiguous state:

- `source`
- `observed_at`
- `verified_at`
- `confidence`
- `scope`
- `owner`
- `supersedes`
- `verification`
- `expires_or_review_at`

The current environment should override stale memory, consistent with OpenAI’s memory guidance. [OpenAI agent memory](https://openai.github.io/openai-agents-python/sandbox/memory/).

## 4.7 Expiry and quarantine

**Confidence: Emerging.**

Use three lifecycle states:

- Active.
- Review due.
- Quarantined.

Quarantine rather than delete automatically. Never auto-expire security, legal, destructive-action, data-integrity, or user-authorization rules. For ordinary workflow rules, move to review when:

- Their scope has not appeared for a configured period.
- The underlying tool or version changed.
- Verification fails.
- A newer rule supersedes them.
- They repeatedly activate without changing behavior.

CrewAI’s recency half-life and OpenAI’s bounded raw-memory policy provide precedents, though neither is identical to rule lifecycle management. [CrewAI Memory](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.15.12/en/concepts/memory.mdx), [OpenAI agent memory](https://openai.github.io/openai-agents-python/sandbox/memory/).

## 4.8 Split canonical state from narrative history

**Confidence: Established direction.**

Maintain three representations:

1. **Canonical state:** structured, exact, small, protected.
2. **Narrative summary:** lossy explanation of what happened and why.
3. **Raw evidence pointers:** transcript IDs, commit hashes, issue URLs, logs, or archived tool results.

This resembles the combination of LangGraph checkpoints plus LangMem summaries, or Letta core memory plus recall storage. [LangGraph persistence](https://langchain-ai.github.io/langgraph/how-tos/persistence/), [MemGPT paper](https://arxiv.org/abs/2310.08560).

## 4.9 Task-boundary-aware compaction

**Confidence: Emerging.**

Avoid compacting in the middle of an unresolved tool exchange or immediately before a critical decision. Prefer:

- A token threshold.
- A clean task boundary.
- Completed tool-call/result pairs.
- A freshly validated canonical-state block.

LangMem already groups related tool messages during summarization; Factory reduces pressure through progressive disclosure. [LangMem short-term memory](https://langchain-ai.github.io/langmem/reference/short_term/), [Factory deferred context](https://factory.ai/news/deferred-context-engine).

## 4.10 Benchmark as release gate

**Confidence: Established direction.**

Every change to prompts, headers, model, threshold, or Claude Code version should rerun:

- Single-cycle survival.
- Five-cycle survival.
- Conflicting-update cases.
- Do-not-touch compliance.
- Downstream task completion.
- Token ratio and latency.

Publish raw failures alongside headline scores. A format improvement should not ship merely because its summary looks cleaner.

---

# 5. Where the conversation lives in 2025–2026

## 5.1 High-value GitHub issues

### Claude Code

- [anthropics/claude-code #23776](https://github.com/anthropics/claude-code/issues/23776) — reports that compaction lost the user’s request to review before implementing. This is directly aligned with authorization and current-intent survival. A useful contribution would be a minimal transcript with a pre/post intent probe and a proposed protected-user-intent acceptance test.

- [anthropics/claude-code #21788](https://github.com/anthropics/claude-code/issues/21788) — requests visible or exportable compaction summaries. This matters because opaque summaries prevent direct diagnosis. A useful contribution would be a privacy-conscious debug format and evidence that visibility shortens failure triage.

- [anthropics/claude-code #42817](https://github.com/anthropics/claude-code/issues/42817) — asks for control over automatic compaction. The request was not planned, making external measurements of threshold and failure behavior particularly valuable. Contribute a versioned benchmark rather than a generic feature request.

- [anthropics/claude-code #85075](https://github.com/anthropics/claude-code/issues/85075) — raises stale auto-memory and lack of freshness or expiry. This is the natural venue for provenance, verification dates, quarantine, and usage-counter experiments.

- [anthropics/claude-code #82509](https://github.com/anthropics/claude-code/issues/82509) — describes stored compaction summaries not appearing correctly in the UI and includes large pre/post size examples. This matters for observability and separates storage success from interface rendering.

- [anthropics/claude-code #11629](https://github.com/anthropics/claude-code/issues/11629) — concerns `CLAUDE.md` behavior across compaction. Current official documentation now says project-root instructions are re-injected, so any contribution should pin versions and distinguish old behavior from the current contract. [Current Claude memory documentation](https://code.claude.com/docs/en/memory).

### OpenAI Codex

- [openai/codex #25900](https://github.com/openai/codex/issues/25900) — proposes explicit structured checkpoints. Your seven-header schema and multi-cycle test would be directly relevant if presented as reproducible evidence.

- [openai/codex #21468](https://github.com/openai/codex/issues/21468) — requests visible and prompt-guided compaction. Useful contribution: compare hidden compaction, free-form prompt guidance, and protected structured state.

- [openai/codex #10986](https://github.com/openai/codex/issues/10986) — discusses forced remote compaction and loss of fallback or visibility. Useful contribution: a failure-recovery protocol and local canonical-state fallback.

- [openai/codex #35935](https://github.com/openai/codex/issues/35935) — reports repeated work and token burn after task-state loss. Useful contribution: exact duplicate-work and “already completed” probes.

- [openai/codex #38269](https://github.com/openai/codex/issues/38269) — reports additional context being dropped after compaction. Useful contribution: a typed field-survival fixture and regression test.

### Letta

- [letta-ai/letta #3242](https://github.com/letta-ai/letta/issues/3242) — redundant triggers and summary-of-summary degradation.
- [letta-ai/letta #3270](https://github.com/letta-ai/letta/issues/3270) — configured sliding-window behavior replaced by a full wipe.
- [letta-ai/letta #3279](https://github.com/letta-ai/letta/issues/3279) — summarizer context-cap failure.

These issues are valuable because Letta exposes more of its memory machinery than closed coding agents. A useful contribution would be the same transcript evaluated across trigger thresholds and multiple cycles, with exact retention scores.

## 5.2 Hacker News

- [“The context bottleneck”](https://news.ycombinator.com/item?id <tg-id>) — broad discussion of context as the limiting resource in agent systems. A useful contribution is a measured separation of state, history, retrieval, and instruction memory.

- [Ask HN: continuous context for coding agents](https://news.ycombinator.com/item?id <tg-id>) — practical operator pain around maintaining long-running coding sessions. A useful contribution is a compact comparison table with raw reproducible fixtures.

- [“Don’t trust large context” discussion](https://news.ycombinator.com/item?id <tg-id>) — relevant to the distinction between context capacity and usable recall. A useful contribution is a degradation curve rather than another maximum-token claim.

- [OpenAI reduces Codex context size](https://news.ycombinator.com/item?id <tg-id>) — a focused venue for explaining why a smaller, validated state representation can outperform an unstructured larger window.

HN contributions should lead with measurements, raw artifacts, failure cases, and caveats. A repo announcement without comparison baselines will be received as promotion rather than research.

## 5.3 Product forums and Discord spaces

- [OpenAI Developer Community: compact with `previous_response_id`](https://community.openai.com/t/compact-a-response-with-previous-response-id/1372502) — the most technically specific public conversation about OpenAI’s compaction contract. Contribute API-version-pinned experiments and clarify user-message versus assistant/tool retention.

- [OpenAI Developer Community: automatic compaction](https://community.openai.com/t/automatically-compacting-context/1376290) — appropriate for trigger behavior, manual-versus-automatic comparison, and retry/recovery semantics.

- [Letta Community Forum](https://forum.letta.com/) and its [welcome/community thread](https://forum.letta.com/t/welcome-to-the-letta-community-forum/23) — useful for implementers working directly on agent-managed memory. Contribute cross-backend BFCL-style results and concrete failure fixtures.

- [LangChain Forum](https://forum.langchain.com/) and [LangChain community page](https://www.langchain.com/community) — relevant to checkpoint-plus-summary architectures and LangMem prompt optimization. Contribute a LangGraph adapter for the proposed compaction benchmark.

- [Cursor Forum: rules versus memories](https://forum.cursor.com/t/best-way-to-provide-context-rules-vs-memories/132960) — directly relevant to deciding what belongs in durable rules, automatic memories, or current-session state. A useful contribution would be outcome data showing which class of information survives and which becomes stale.

- [Anthropic Developers Discord](https://discord.gg/anthropic) — valuable for rapid reproduction checks across Claude Code versions. Contributions should point to permanent GitHub artifacts because Discord discussions are difficult to cite and archive.

## 5.4 Research clusters

- [ICLR 2026 MemAgents workshop](https://openreview.net/group?id=ICLR.cc%2F2026%2FWorkshop%2FMemAgent) — the most directly aligned research venue for memory architectures, lifelong agents, compression, and evaluation.

- [ICLR 2026 workshop announcement](https://blog.iclr.cc/2026/01/13/iclr2026-workshops/) — useful for tracking adjacent lifelong-agent and self-improvement communities.

- [Agent memory survey](https://arxiv.org/abs/2512.13564) — maps memory mechanisms and evaluations across agent systems. A useful contribution is to identify session compaction as a distinct missing benchmark category.

- [Context engineering survey](https://arxiv.org/abs/2507.13334) — broader taxonomy covering retrieval, compression, and context construction. A useful contribution is empirical repeated-cycle evidence.

- [BFCL V4 Memory](https://gorilla.cs.berkeley.edu/blogs/16_bfcl_v4_memory.html) — likely the strongest practical upstream target for a new compaction track.

- [LongMemEval-V2](https://xiaowu0162.github.io/longmemeval-v2/) — best source of workflow, dynamic-state, gotcha, and premise-awareness test categories.

A workshop paper or benchmark report should emphasize the intervention—repeated transcript replacement under a fixed token budget—because that is not adequately covered by general long-context or retrieval-memory evaluation.

## 5.5 Newsletters and practitioner publications

- [Latent Space: “Is Harness Engineering Real?”](https://www.latent.space/p/ainews-is-harness-engineering-real) — reaches builders thinking about agent harnesses rather than only model architecture. A useful contribution is evidence that formatting, lifecycle, and validation alter continuation success.

- [Latent Space: “Tasteful Tokenmaxxing”](https://www.latent.space/p/ainews-tasteful-tokenmaxxing) — appropriate for compression ratio versus task-quality tradeoffs.

- [Simon Willison’s sub-agent coverage](https://feeds.simonwillison.net/tags/sub-agents/) — relevant to context isolation, `plan.md`-style external state, and subagent “context quarantine.” A useful contribution would be a small, inspectable experiment with raw transcripts.

- [O’Reilly Radar: “When Context Collapses”](https://oreillyradar.substack.com/p/when-context-collapses-teaching-agents) — a practitioner audience already focused on compaction and agent learning. A useful contribution is the benchmark methodology and multi-cycle results.

- [Cloudflare Agent Memory](https://blog.cloudflare.com/introducing-agent-memory/) — represents production infrastructure interest in persistent agent memory. It is a relevant venue for contrasting external durable storage with prompt-only summaries.

---

# 6. What I would change in the two repositories next week

## `compact-canon`

1. **Release a backward-compatible schema revision.** Keep the seven top-level headers, but add a machine-parseable metadata line and protected subsections inside them:

   - schema version
   - session/compaction generation
   - created timestamp
   - source model and harness version
   - previous compact hash
   - `VERBATIM`
   - `SUMMARIZABLE`

2. **Add a deterministic validator.** It should reject missing headers, duplicate active decisions, lost unresolved TODOs, invalid counters, and protected fields that changed without an explicit `UPDATE` or `SUPERSEDE`.

3. **Add a post-compact probe command.** Seed 20–30 typed facts, invoke compaction, then score exact survival. Include current intent, paths, values, counters, rejected alternatives, authorization boundaries, tool contracts, and abstention probes.

4. **Add three- and five-cycle tests.** The public headline should include both first-cycle retention and the degradation slope.

5. **Add a continuation-task score.** After compaction, require the agent to choose the next action, avoid a forbidden path, use the right command, and identify the right blocker.

6. **Publish a version matrix.** Separate Claude Code versions, models, platforms, invocation methods, manual `/compact`, and automatic compaction. Explicitly label the 354/354 result as belonging to its tested versions because current Anthropic documentation says project-root `CLAUDE.md` is re-injected after compaction. [Claude Code memory](https://code.claude.com/docs/en/memory).

7. **Publish raw failures.** A single lost authorization clause is more informative than a 99% average.

## `claw-retro`

1. **Turn each promotion candidate into a structured record:**

   - incident/evidence
   - proposed rule
   - canonical home
   - invocation door
   - scope
   - owner
   - verification
   - confidence
   - created and last-verified dates
   - review/expiry date
   - superseded rule, if any

2. **Add an outcome audit before creating new rules.** Check whether prior promoted rules loaded, fired, prevented recurrence, caused friction, or were bypassed because their invocation door was missing.

3. **Require promotion evidence.** Promote immediately only for explicit user policy, safety requirements, or a clearly verified invariant. For ordinary heuristics, require either repeated incidents or a successful controlled trial.

4. **Add quarantine rather than automatic deletion.** Stale workflow rules can leave active context but remain reviewable. Never auto-expire safety, authorization, destructive-action, compliance, or data-integrity rules.

5. **Track activation and verification separately.** “Loaded” is not “used”; “used” is not “helpful.” Record all three where observable.

6. **Make the door audit executable.** Each promoted rule should name the exact hook, command, skill trigger, path scope, startup loader, or validation step that causes it to run. If no executable door exists, classify it as documentation rather than enforcement.

7. **Finish every retro by generating and validating the compact block.** The block should carry unresolved state forward, while promoted durable rules leave the session block and move to exactly one canonical home.

## One-week implementation order

- **Days 1–2:** protected fields, schema version, validator, and compact-block lineage.
- **Day 3:** one-/three-/five-cycle public benchmark fixtures and deterministic scorers.
- **Day 4:** outcome-mode retro records, quarantine lifecycle, and executable-door checks.
- **Day 5:** run a versioned Claude Code matrix, publish raw artifacts, and open narrowly scoped benchmark discussions in the most relevant Claude Code, Codex, Letta, and BFCL venues.

The central product promise should evolve from “this is a good summary format” to:

> “This is a tested continuation-state protocol: exact operational state is protected, lossy history is clearly separated, repeated compactions are measured, and every durable rule has both evidence and an executable path.”
