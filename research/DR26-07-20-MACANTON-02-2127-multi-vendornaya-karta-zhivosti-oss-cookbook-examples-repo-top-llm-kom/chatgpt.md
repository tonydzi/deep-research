# DR26-07-20-MACANTON-02-2127 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-20-MACANTON-02-2127 — ChatGPT (Pro, Deep Research) отчёт

- source: https://приватный чат

- collected: 2026-07-23 (сессия musing-proskuriakova-2, забрано Claude через Chrome MCP get_page_text)
- run proof / ГЕЙТ: ⚠️ первый прогон 21.07 был ПУСТОЙ (0 web searches, 0 citations — видно по follow-up в треде). Пользователь пнул REDO → **второй прогон «Worked for 70m 48s» + секция Sources с живыми GitHub-ссылками** = настоящий Deep Research, гейт searches>0 ПРОЙДЕН.
- title чата: "LLM Vendor OSS Contributions" (21 Jul)
- verbatim ниже (текст страницы get_page_text; таблицы линеаризованы)

---

## Промпт (первое сообщение треда)

DR26-07-20-MACANTON-02-2127

DEEP RESEARCH REQUEST — "Which official LLM-vendor OSS repos actually merge external contributions right now (July 2026), and what are they asking for?"

CONTEXT. I contribute polished, runnable example/cookbook PRs to official LLM-vendor OSS repos (with live runs, honest AI-authorship disclosure, and credit to issue authors). Six PRs are already open in Anthropic repos (claude-cookbooks #784/#787/#788/#789, claude-agent-sdk-demos #70, skills #1460). My portable assets: (a) multi-agent consensus + adversarial verification (default-refute skeptics), (b) authority routing ADVISE/EXECUTE/DEFER/STOP for tool-using agents, (c) pipeline-vs-barrier concurrency for sub-agent orchestration with measured speedups, (d) citation-faithfulness evals (verbatim gate + LLM judge), (e) reasoning-quality gates, (f) persistent memory / "second brain" patterns for agents, (g) multi-machine agent fleet operations, (h) MCP tooling and tool-call signing.

[MY CURRENT LIVENESS MAP + QUESTIONS 1-6 + OUTPUT FORMAT — как в исходном промпте DR-2127, идентичны телу, разосланному всем вендорам.]

**Follow-up пользователя (после пустого первого прогона):** "Your previous answer ran with 0 web searches and 0 citations — the scorecard was generated without actual research (several GitHub handles in it do not exist). Please REDO this as a real Deep Research run: browse GitHub PR/issue histories and community sources live, verify every repo verdict with links, and answer the same DR26-07-20-MACANTON-02-2127 request from the first message in full. Citations required throughout."

---

## Отчёт (второй прогон — Worked for 70m 48s)

### DR26-07-20-MACANTON-02-2127 — verified July 2026 map

**Bottom line**

Your map substantially overrates Groq, xAI, Hugging Face's current velocity, Together, and new-feature work in Semantic Kernel. It underrates or misses the best current doors:

- microsoft/agent-framework
- genkit-ai/genkit
- agentscope-ai/agentscope
- awslabs/agentcore-samples
- google/adk-python-community
- openai/openai-agents-python / openai-agents-js
- google-gemini/genai-processors
- openai/openai-cookbook, selectively

The strongest demand is not "another multi-agent demo." It is:
- preserving tool-call/result and approval state across persistence, compaction, reconnects, and resume;
- bounded concurrency, cancellation, and backpressure;
- MCP schema validation, authorization, and trust boundaries;
- executable governance and human-approval semantics;
- narrow evals tied to a real failure mode;
- fixing stale notebooks and migrations with live verification.

Window used: April 22–July 21, 2026 primary, older history where 90-day sample sparse. Counted a repo as externally merge-friendly only when an externally submitted PR actually landed; bot mirrors and vendor-internal PRs not treated as proof. Limitation: no reliable broad "community consensus" corpus for most repos; strongest evidence = merged-PR history, review threads, contribution docs, maintainer statements. Groq = clearest case where public messaging stays welcoming while GitHub merge activity has gone cold.

### 1. Per-repo scorecard — Original liveness map

**google/adk-python-community — HIGH; agree.** External proof: #141 (externally authored governance plugin) merged after replacing earlier PR + architecture/safety review. Gatekeeper: @DeanChensj. Rewards native BasePlugin, async callbacks, fail-closed defaults, correct folder, tests, usable sample. Rejects wrong Google licensing, non-native abstractions, unsafe defaults, incomplete tests. Best fits: memory backends, governance/audit extensions. #142 asks post-execution independently verifiable audit trail. Killers: no coordination with issue author, custom framework instead of ADK hooks, weak failure semantics, missing CLA/header/tests.

**huggingface/cookbook — MEDIUM now; HIGH historically. DOWNGRADE.** Only one merge in 90-day scan (external notebook PRs #341/#333 prove historical openness). Not presently high-throughput. Reviewer @sergiopaniego; recurring @merveenoyan, @stevhliu. Rewards runnable Colab, model-choice rationale, clear results, neutral framing. Better current work = maintenance: #316 broken multi-agent recipe (removed smolagents.ManagedAgent); #326 realistic graph+Agent-RAG. #303 NOT clean whitespace — related search-agent/vector-search cookbook already landed.

**microsoft/semantic-kernel — MED-HIGH for fixes; LOW for new features.** Merge-friendly for targeted Python bugs/security hardening, wrong strategically for new components — MS now identifies Agent Framework as production successor. Owners @eavanvalkenburg, @TaoChenOSU. Rewards exact security boundaries, cross-language consistency, regression tests, full compliance. Open demand: function/tool governance, indirect prompt-injection controls, memory-poisoning resistance, MCP trust, persistent memory. But many "governance" issues = vendor pitches/duplicates. Net-new APIs → target Agent Framework.

**groq/groq-api-cookbook — LOW now; historically friendly. DOWNGRADE from HIGH.** Zero merges in last 90 days. Prior external wave = late-2025 MCP integrations (#103). Rubric real but current liveness not. Reviewers from active period: @janzheng, @shaunjoshi, @ozenhati. Rubric: usefulness/originality/clarity/accuracy/depth/writing, <3/5 any dim = reject risk. Do not submit until maintainer confirms active review bandwidth.

**openai/openai-agents-python — HIGH for narrow exact work; agree.** Small external-looking fixes merge extremely fast; #3898 = two-file regression fix merged ~74 min. Large changes higher bar. @seratch cross-SDK gatekeeper. Rewards reproducible failures, minimal compat-preserving patches, explicit lifecycle semantics. Demand: sessions, approval/history integrity, persistence, MCP lifecycle, tracing. #3738 asks turn/run-aware session retrieval (item-based slicing orphans tool calls) — fixture/test-only first PR safer than new public API.

**openai/openai-agents-js — HIGH; agree.** #1474: maintainer reproduced provider behavior vs concrete LiteLLM version, approved after generalization. Gatekeeper @seratch. Rewards empirical reproducer, provider-neutral contract, exact tests, backward compat. Gap: #1097 approval-with-modified-arguments (execution, persisted history, previous_response_id, conversation-backed runs, resumed sessions must all agree on what was approved). Prototype exists → offer conformance tests/docs, not competing impl.

**xai-org/xai-cookbook — LOW; disagree with MED-HIGH.** No merges last 90 days, large stale queue. Open PRs already cover human sign-off, verifiable reasoning, MLflow tracing, MCP, research/citations, PDFs. Recent merged mobile work = xAI-associated. Last visible push April 16 2026. No stable public external gatekeeper. Almost every portable asset collides with an already-open PR. Real blocker = review capacity, not packaging.

**google-gemini/cookbook — MED-HIGH maintenance; MED/LOW new notebooks.** Small compat/auth/quickstart/Interactions-API updates merge. Substantial notebook #790 took ~a year; docs cleanup #1280 ~two weeks. Gatekeepers @Giom-V, @markmcd, @kkorpal, reviewer @andycandy. Rewards pedagogy, first-use imports, concise cells, template compliance, current Gemini APIs. New content strictly issue-first. Killers: missing Google CLA, missing Colab head-branch link, nbfmt/nblint failures, unlicensed media, hard-coded stale metadata, unexplained model choice, gratuitous 3rd-party deps. Maintainers want migration/API-correctness > another agent architecture.

**google-gemini/genai-processors — MED-HIGH; agree with Copybara caveat.** Real external path via genai_processors/contrib, but some accepted changes reappear as copybara-service[bot] mirror PRs (counting only bot PR falsely looks internal-only). External #15 demonstrates route. Gatekeeper @kibergus. Rewards native processor/data abstractions, generic provider support, streaming consistency, fail-loud unsupported media, tests under contrib/tests. Rejects provider formatting logic, oversized framework integrations. Live target: #164 (unbounded queues, missing cancellation, task/resource leaks in streams.concat).

**togethercomputer/together-cookbook — MED-LOW externally.** Active but recent merges dominated by authors of unclear external status. README welcomes PRs+Discord, no strong recent repeat-external proof. Contact via Discord first. High duplication risk (already has serial chains, routing, parallel agents, orchestrator, cross-provider advisor, deep research, LLM-judge/eval notebooks). Needs sharply different measured failure mode.

**mistralai/cookbook — MED maintenance; LOW-MED new concepts.** Maintenance restarted (SDK migrations, compat fixes, auto review, stale-notebook detection merging). Open PR queue ~49 (not 42), many external agent/eval/reasoning proposals. No stable external-content gatekeeper. Fast recent merges = trusted/known contributors. Best work = generated maintenance. #300 flags stale evaluation notebook's deprecated open-mistral-7b. Killers: secrets, unpinned deps, non-Colab, warnings/errors in outputs, missing README registry update, turning maintenance PR into new framework.

**QwenLM/Qwen-Agent — SKIP; agree.** No merges in 90-day scan, only two small merges preceding year while security/correctness PRs accumulate. #871 (SSRF fix w/ tests) closed by author after two weeks no response. No functioning external gatekeeper. Security-related ≠ rescued merge probability.

**DeepSeek — no cookbook, but not total SKIP.** No conventional cookbook, BUT overlooked official door = deepseek-ai/awesome-deepseek-agent (recently merged external bilingual integration guides; #93 credited Qwen Code generation and still merged). Gatekeeper @tianyicui. Lightweight review: resolve conflicts, match bilingual format, document real external tool/client. Good for concise integration guide, not new framework. AI-authorship disclosure NOT disqualifying.

**Cohere — LOW, but "staff only" too strong.** Old cookbook archived, BUT cohere-ai/cohere-developer-experience explicitly welcomes contributions. External typo PR #140 eventually merged (after very long delay). Reviewer @billytrend-cohere. One-way internal sync for OpenAPI/snippet content → external changes may be ported internally then overwritten (ambiguous close/overwrite). Large external example PRs unattractive.

**meta-llama/llama-cookbook — SKIP; agree.** No merges 90-day, ~68-69 open PRs. #981 (external doc fix) took months in 2025; no 2026 signal. Historical merger @IgorKasianenko ≠ currently staffed funnel. Small critical corrections only.

**microsoft/autogen — SKIP for features; agree.** MS explicitly = maintenance mode, directs new dev to Agent Framework. Bugs/security/docs may be valid. New memory/orchestration/authority-routing/MCP → Agent Framework.

### 1b. Missed doors

**microsoft/agent-framework — HIGH.** Best Microsoft door now. External Python fixes land after focused review; #7124 = narrow issue-linked regression fix approved by @eavanvalkenburg. Requires issue-first for nontrivial, warns against surprise APIs. Fits: session/history integrity, approval persistence, compaction correctness, disconnect-safe execution, structured HITL, memory. #7212 = unusually strong live target (reproducible, unassigned, auto-confirmed, aligned with tool-call/result integrity).

**genkit-ai/genkit — HIGH.** Major Google/Firebase-adjacent missed door. External RFC-aligned features/fixes merge after serious review. #5475 (public model-mocking utilities). MCP input-schema conversion + runtime validation = open TODO issues #4366 and #4365. Also: approval/resume, interrupts, testing utilities, MCP trace identity.

**agentscope-ai/agentscope — HIGH.** Best Alibaba-associated door. Actively merging external security/permission/persistence/context-integrity work. #2004: external Bash permission fix accepted only after @DavdGao required project's existing tree-sitter AST. Fits: authority routing, concurrent approval correctness, multi-machine event delivery, persistent memory, context compression, MCP security. #2123 = deterministic replay/live race silently losing team HITL events (concurrency/fleet-ops fit).

**awslabs/agentcore-samples — HIGH.** Very active, demonstrably external-friendly with strict process. External Dash0 integration #1539 merged after scope/folder review. Wants issue-first, focused changes, local tests, CI follow-through, complete docs. Best AWS target: governance, Cedar policies, MCP, evaluation, memory, HITL, tool authorization. Several gaps already have "implementation ready" authors → collaborate not duplicate.

**openai/openai-cookbook — MED-HIGH, selective.** Active, merges external vendor-neutral examples. #2772 (Oracle-backed Agents SDK memory example) reviewed on registry metadata, dependency consistency, why 3rd-party backend added value beyond built-in sessions. Citation-faithfulness/reasoning-gate content fits if small, runnable Agents SDK cookbook w/ clear failure mode, no framework marketing. Best-effort, no merge guarantee.

**perplexityai/api-cookbook — MEDIUM; external proof not fully established.** Active, invites examples/showcases/advanced articles. Focus: Agent API, finance_search, sandbox execution, live sources, research workflows. Some author affiliations ambiguous → not yet HIGH externally. Strong fit for citation-faithfulness evals. Wants MDX; full apps in separate repos.

**aws-samples/amazon-bedrock-samples — MEDIUM.** Active but author affiliation often AWS-adjacent. #715 documented live prompt-optimization (score 0.42→1.0 + replay mode). Good for measured eval/optimization notebooks, but AgentCore Samples stronger for governance/agent-runtime assets.

**NVIDIA/GenerativeAIExamples — LOW-MED externally.** Alive, has community/ area, but only one merge in 90-day scan (NVIDIA-associated author). Thematic fit for guardrails/parallel rails/RAG/HITL/citation eval. Get explicit folder/maintainer approval before building.

**meta-llama/synthetic-data-kit — LOW-MED.** Historical external merges (CoT curation, parsers, config, rolling summaries) but no 90-day activity, ~17 PRs open. Reasoning-quality gates could fit, liveness too weak to rank higher.

**meta-llama/PurpleLlama — SKIP.** Internal re-sync commits exist, BUT no evidence stale external security/eval PR queue is quietly landing through syncs. **Do not interpret "re-sync with internal repository" as Copybara proof for community contributions.**

**meta-llama/llama-models — SKIP for examples.** Virtually no recent external merge signal. Not a hidden cookbook door.

**openai/evals — SKIP.** No merges 90-day; README points broader eval work to managed Evals API; old external eval PRs open for years. Put eval assets in OpenAI Cookbook, Agents SDK examples, or Evals API.

**dashscope/dash-cookbook — SKIP.** No merged PRs in 90-day or 1-year scan. Use AgentScope core.

### 1c. Copybara / internal-sync exceptions
**Confirmed misleading graveyards:** google-gemini/genai-processors (YES — contributor PRs reviewed under contributor's branch then represented by copybara-service[bot]; follow replacement links/merge commits/attribution). cohere-ai/cohere-developer-experience (PARTLY — one-way sync from internal source; external change accepted conceptually then appears as internal resync).
**Not demonstrated:** OpenAI Agents, MS Agent Framework/SK, xAI Cookbook, Groq, Mistral, Qwen-Agent, Together. PurpleLlama internal re-sync ≠ imported external PRs; treat external queue as stale unless maintainer references internal landing.

### 2. Ranked TOP-10 contribution actions
P(merge | prior issue/maintainer alignment + issue-author credit + focused PR + all checks pass). Without alignment subtract ~10-25pp.

1. **Microsoft Agent Framework — #7212** compaction pairing repair (orphaned results after HITL/resume). Asset: persistent-memory correctness, reasoning/history gates. **60-75%** — best combo of active repo + proven funnel + concrete bug + narrow scope + strong reproducer.
2. **GenAI Processors — #164** bounded backpressure + cancellation (unbounded asyncio.Queue, leaked tasks). Asset: pipeline-vs-barrier concurrency. **55-70%**. Fix streams.concat, don't build generic orchestration package.
3. **AgentScope — #2123** close replay/live HITL race (subscribe-first/buffer/dedup). Credit @yuminshen. Asset: fleet ops, concurrency, durable HITL. **55-70%**.
4. **Genkit Python MCP — split #4366 + #4365** (schema conversion PR1, arg validation PR2). Asset: MCP tooling, signed/validated calls. **50-65%**. Don't combine conversion+validation+signing+policy in one PR.
5. **Mistral Cookbook — #300** repair stale evaluation notebook (deprecated open-mistral-7b). Asset: citation-faithfulness/reasoning gates. **55-70% maintenance-only; 35-50% if adding new eval section.**
6. **ADK Community — #142** collaborate on tamper-evident execution audit. Credit @giskard09. Asset: tool-call signing, authority chains. **45-60%** (governance plugin precedent by same maintainer).
7. **OpenAI Agents JS — #1097** conformance suite for modified approvals. Asset: ADVISE/EXECUTE/DEFER/STOP authority semantics. **40-55%**, conditional on @seratch inviting test/docs.
8. **AWS AgentCore Samples — #1587** strengthen Cedar coding-agent sample. Collaborate @psv901. Asset: authority routing + adversarial verify. **40-55% as collaboration; ~0 as competing impl.**
9. **OpenAI Agents Python — #3738** fixture-only history boundary matrix (no public API). Asset: persistent memory, replay-validity gates. **30-45%**.
10. **Hugging Face Cookbook — #316** fix broken multi-agent recipe (removed ManagedAgent) before adding benchmarks. Asset: pipeline-vs-barrier orchestration. **35-50%** (pure repair > new consensus cookbook).

### 3. Norms that kill PRs
**Google CLA:** every commit author/co-author covered; commit email = CLA identity; individual CLA fast, corporate slower; retrigger bot after fixing metadata (don't just comment "signed"). Signed once for Google OSS, but each PR passes bot author check.
**Microsoft CLA:** one-time signature across MS repos. Bigger blockers: no issue assignment, surprise public APIs, incomplete pre-commit/test, no area owner involvement.
[Per-repo minimum green checklists + fatal mistakes — ADK, Gemini, GenAI Processors, MAF, SK, OpenAI Agents Py/JS, HF, Groq, xAI, Mistral, AWS AgentCore, Perplexity — see full table in source.]

### 4. Timing and optics
Observed merge times: OpenAI Agents Py #3898 (2-file fix) ~74 min; ADK #141 ~1 day; MAF #7124 ~days; HF #341 (new notebook) ~10 days; Gemini #1280 (docs) ~2 weeks; Genkit #5475 (RFC feature) ~40 days; Gemini #790 (new notebook) ~a year; Cohere DXP #140 (typo) ~21 months.
**No defensible best weekday** — format effects, not weekday. Fastest: one bug/one reproducer/one regression test/no new abstraction. Next: stale-content/API migration w/ fresh outputs. Slower: new notebook + TOC/registry. Slowest: new public API/framework/multi-service demo/unsolicited architecture.

### 5. Do not bother
QwenLM/Qwen-Agent · xAI Cookbook (backlog already has your assets, no 90-day signal) · Groq API Cookbook for now (great rubric, dead liveness) · OpenAI Evals OSS · Meta PurpleLlama/llama-models/llama-cookbook (no external funnel, resyncs ≠ proof) · MS AutoGen for features · DashScope Cookbook · Cohere archived notebooks (only small docs, internal-sync caveats) · Together Cookbook for generic orchestration/eval (catalog covers it) · NVIDIA GenerativeAIExamples without prior approval.

### Recommended submission order
**MAF #7212 → GenAI Processors #164 → AgentScope #2123 → Genkit #4366/#4365 → Mistral #300 → ADK #142 collaboration.**
Maximizes current review activity + externally proven merge behavior + unambiguous technical need + limited overlap + direct reuse of existing assets.

_(Sources section present in chat — inline GitHub PR/issue links throughout; run = "Worked for 70m 48s".)_
