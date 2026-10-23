# DR26-07-20-MACANTON-02-2127 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-20-MACANTON-02-2127 — Gemini (Pro Extended, Deep Research) отчёт

- source: https://приватный чат

- collected: 2026-07-23 (сессия musing-proskuriakova-2, забрано Claude через Chrome MCP — canvas innerText `div.markdown.markdown-main-panel` → render `<pre>` → get_page_text)
- run proof / ГЕЙТ: ✅ Deep Research режим, «I've completed your research», канвас-отчёт «LLM Vendor Repo Contribution Analysis» (Jul 21, 12:36 AM), с секцией Sources + инлайн-эвиденс (номера issue, GitHub-хендлы гейткиперов) → настоящий Deep Research, гейт searches>0 ПРОЙДЕН.
- ⚠️ ВАЖНО про доступ: гео-стена «not supported in your country» была у ДРУГОГО Chrome-профиля (deviceId 21580995); в реальном профиле Антона (d6394213) Gemini под a@ работает штатно. Блок был профиль/сессия, НЕ IP-машины.
- verbatim ниже (чистый канвас-текст, таблицы линеаризованы)

---

Optimizing Open-Source Contributions in the LLM Vendor Ecosystem: A July 2026 Analysis

The open-source landscape surrounding official large language model (LLM) vendor repositories has undergone a massive architectural shift by July 2026. The initial wave of rudimentary application programming interface (API) wrappers and trivial chat examples has been entirely superseded by enterprise-grade frameworks emphasizing multi-agent orchestration, robust Model Context Protocol (MCP) integrations, and stringent, zero-trust security guardrails. For external contributors possessing advanced, portable assets—such as multi-agent consensus algorithms, persistent memory patterns, and human-in-the-loop (HITL) authority routing—navigating this ecosystem requires more than just high-quality code. It demands a nuanced understanding of maintainer psychology, rigid continuous integration (CI) constraints, and the hidden mechanics of corporate monorepo synchronization.

This comprehensive report validates and extends current repository liveness metrics, deeply analyzes gatekeeper behavior and pull request (PR) rejection norms, identifies high-probability unmet demands across primary and secondary vendor ecosystems, and delivers a ranked integration strategy tailored for advanced agentic assets.

## Ecosystem Liveness and External Merge Verification

An analysis of commit histories, pull request resolutions, maintainer interactions, and underlying corporate synchronization protocols over the past ninety days reveals a stark divergence between apparent repository stagnation on GitHub and actual internal engineering velocity. The raw GitHub API scans often fail to capture the reality of how enterprise AI teams manage community contributions, necessitating a verified adjustment of liveness verdicts.

The phenomenon of the "PR graveyard" is frequently an optical illusion caused by internal monorepo synchronization tools. Google and its subsidiaries heavily utilize a tool called Copybara to synchronize internal Piper repositories with public GitHub mirrors. In repositories like the Google Gemini Cookbook and GenAI Processors, external pull requests may appear to sit idle or be closed without a standard GitHub merge event. In reality, internal engineers squash the community's logic and push it directly to the main branch from the internal codebase, carrying over the functionality while breaking the public contribution graph. OpenAI exhibits similar synchronization behavior for core model definitions, although their agent SDKs are managed more transparently directly on GitHub. Conversely, Microsoft explicitly avoids this opacity in the Semantic Kernel ecosystem, operating highly visible sprint planning and merge queues directly on the public platform.

### Per-repo scorecard (corrected reality check — линеаризовано)

**openai/openai-agents-python & -js — Very High.** Exceptionally active, transparent external integration; maintainers merge community patches within hours. Gatekeeper @seratch. Demand: pre-approval tool guardrails, sandbox boundary enforcement, external tracing processors. Killers: violating backwards-compat parameter contract; bypassing mandatory $pr-draft-summary.

**google/adk-python-community — High.** Explicitly engineered to house external integrations; frequent merges of community persistent-memory + governance plugins. Gatekeepers @DeanChensj, @secprog (bot). Demand: HITL approval gateways, model fallback logic, session persistence. Killers: unforgiving Google CLA; must match ADK event adapter patterns + trigger release-please.

**microsoft/semantic-kernel — High.** Core engine transitioned to Microsoft Agent Framework (MAF) v1.0, heavy enterprise security focus; merges external PRs via structured sprints. Gatekeepers @moonbox3, @markwallace-microsoft. Demand: OWASP ASI06 memory-poisoning defense, mid-execution HITL, time-decay memory weighting. Killers: C#/Python API parity; missing test fixtures; altering trace shapes.

**huggingface/cookbook — High.** Consistent merging of community recipes, high bar for educational value/neutrality/reproducibility. Gatekeepers @rtrompier, @hf-security-analysis. Demand: smolagents fleet ops, OpenEnv training recipes, resilient multi-model generation. Killers: uppercase filenames, empty cells, missing _toctree.yml, promotional tone.

**groq/groq-api-cookbook — High.** Highly active/community-driven, ultra-low-latency focus. Gatekeeper: Groq DevRel Team. Demand: MCP bridging, structured JSON outputs, LLM-as-a-judge evals. Killers: must score ≥3 across all categories on explicit 4-point public rubric. ⚠️ (Расходится с Grok=MED и ChatGPT=LOW — Gemini держит HIGH.)

**xai-org/xai-cookbook — Medium-High.** Merges external PRs but significant chronological backlog; high bar for end-to-end execution w/o proprietary deps. Gatekeeper: xAI DevRel Team. Demand: verifiable reasoning via xProof, MCP, real-time audio analysis. Killers: strict Gitleaks + notebook CI hooks; hardcoded keys; large data files without Git LFS.

**meta-llama/llama-cookbook — High (Correction).** ⚠️ Perceived stagnation was artifact of massive repo refactor moving legacy code to archive-main. Active branch highly receptive to Llama 4. Gatekeepers @mattcarrollcode, @Mukller. Demand: multi-agent orchestration, Llama 4 Scout/Maverick demos, deep MCP tool bridging. Killers: strict API-key handling via os.getenv(); strict Python version floors + dependency dedup. ⚠️ (Расходится с Grok=SKIP и ChatGPT=SKIP — Gemini переворачивает в HIGH.)

**google-gemini/genai-processors — High (New).** Extremely active async-first framework; contrib/ exclusively for community plugins. Gatekeeper @kibergus. Demand: semantic caching processors, language-detection layers, parallel agent execution. Killers: must use novel ContentStream + ProcessorPart data models; blocking synchronous code rejected.

**google-gemini/cookbook — Medium.** Suffers Copybara illusion (external PRs merged via internal squashing → delayed public resolution, confusing feedback). Gatekeepers @kkorpal, @markmcd, @Giom-V. Demand: Omni Flash Interactions API, Live Translation, Batch API audio. Killers: issue-first culture; rejects 3rd-party pip when native Vertex/AI Studio exists.

**SKIP double-checks:** QwenLM/Qwen-Agent + qwen3-vl = corporate drop points (external PRs incl. security closed unmerged for internal monolithic updates). DeepSeek: no official cookbook repo (only awesome-lists). Cohere: cookbook archived, active repo staff-only. microsoft/autogen: CONFIRMED maintenance mode → all new dev → Microsoft Agent Framework.

## Gatekeeper Intelligence (recent review dynamics)
- **OpenAI (@seratch):** merges compliant code within hours; rewards tracing logic, strictly-typed MCP, session mgmt (MongoDB/Redis serialization). Rejects "parameter order contract" violations (new optional fields must append to end); untrusted sandbox manifests escaping host FS; bypassing $pr-draft-summary hook.
- **Semantic Kernel (@moonbox3, @markwallace-microsoft):** favor middleware, robust unit testing, .NET→Python/Java parity. Reject breaking observability/trace structures; altering KernelProcessStepContext shape; forcing heavy external DB (e.g. Weaviate) for basic memory.
- **Google ADK (@DeanChensj):** rewards Pydantic-typed A2A contracts, fallback mechanisms, HITL gateways. Rejects stateful logic ignoring ADK event adapter pattern; CLA identity mismatch; not triggering release-please.
- **Hugging Face (@rtrompier):** prioritizes end-to-end reproducible open-weight workflows (smolagents, GRPO, RAG eval). Rejects proprietary promo, uncited OSS origins, empty cells, uppercase filenames.
- **Groq (DevRel team):** rewards advanced MCP servers (Browserbase, Firecrawl), structured JSON, LLM-as-a-judge. Explicit 6-category rubric (Usefulness/Originality/Clarity/Accuracy/Depth/Grammar), <3 any = reject.

## Trending Unmet Demands
- **Agent Security & HITL Authorization:** SK — declarative controls vs memory poisoning (OWASP ASI06) + prompt injection during Auto Function Invocation; community drafting GovernancePolicyFilter (default-deny high-risk native skills, zero-trust HITL). ADK — standalone FastAPI service queue managers + typed Pydantic ApprovalRequest. OpenAI merged pre-approval tool input guardrails.
- **Persistent Memory / "Second Brain":** SK — lightweight decay-weighted cross-session persistence (proposed DakeraMemoryStore) + cross-session memory-poisoning defenses. Groq — sophisticated chat history mgmt + RAG for high-param reasoning models.
- **MCP standardization race:** OpenAI/Groq/Meta Llama all prioritizing MCP PRs. Groq wants Exa+Browserbase workflows. Meta Llama soliciting 3rd-party MCP for Llama 4. Google GenAI Processors v2.0 added background non-blocking MCP session support → need async MCP examples in contrib/.
- **Advanced Concurrency & Fleet Orchestration:** GenAI Processors built on asyncio, parallel_concat operator — measured-speedup pipeline-concurrency contributions aligned. Together AI + HF want orchestrator-subtask patterns, parallel agent workflows, smolagents/LangGraph fleet integrations.

## Missed Doors (secondary ecosystems)
- **Meta:** llama-cookbook active (post-refactor, Llama 4 focus). **meta-llama/PurpleLlama** = central trust&safety hub (Llama Guard 3, Code Shield) — highly receptive to adversarial verification + I/O safeguards. meta-llama/synthetic-data-kit — active, fits LLM-as-a-judge curation. ⚠️ (PurpleLlama: Gemini + Grok = alive door; ChatGPT = SKIP.)
- **Google:** genai-processors contrib/ (pipeline concurrency, semantic caching). **firebase/genkit** — full-stack TS/Go, new Agents API (message history, tool loops, HTTP serving), relies on community plugins — target for authority routing + session persistence.
- **AWS:** aws-samples/amazon-bedrock-flows-samples (JSON flow templates, multi-agent collab, guardrails). aws-samples/sample-bedrock-migration-and-modernization-tools → agent-eval module (LLM-as-a-Jury over offline traces) — fits citation-faithfulness/reasoning-quality.
- **OpenAI:** evals restricted to YAML model-graded, BUT openai/monitorability-evals (outcome-property + process-based evals) receptive to citation-faithfulness. Also AgentThreatBench in UK AISI inspect_evals.
- **Alibaba:** aliyun/pai-examples + cinience/alicloud-skills (Model Studio deploys, standalone skills) — core Qwen tightly controlled but these secondary repos open.

## Structural Norms / PR-killing optics
- **Legal/identity:** Google CLA bot (Git author email MUST match signed CLA) on ADK + GenAI Processors; Microsoft CLA on SK. Issue-first cultures (Gemini Cookbook) reject large unsolicited PRs; avoid 3rd-party pip when native SDK suffices.
- **Architectural/testing regressions:** SK — backward compat paramount; PR dies if it alters telemetry trace JSON shape, changes core pipeline filter signatures without regression fixtures, or breaks schema builder; Python must mirror C#. OpenAI — "Parameter Order Contract" sacred (append new optional fields to end); docs must match impl (runnable snippets = API compat checks).
- **Formatting:** educational repos (HF, groq, xai) prefer single-purpose self-contained notebooks executing top-to-bottom; HF strict lint (lowercase filenames, _toctree.yml, no empty cells). Framework repos (SK, openai-agents, genai-processors) want modular packages/plugin classes + rigorous unit tests; notebooks only in docs/examples/.
- **Timing:** ⚠️ mid-week (Tue–Thu) fastest triage; Friday-late PRs sink over weekend. (Расходится с ChatGPT «no defensible weekday».)

## Ranked Strategic Action Plan (by merge probability)
1. **MS Semantic Kernel (Python) — OWASP ASI06 Memory Poisoning Defense.** Asset: adversarial verify + persistent memory. Target: semantic-kernel (issues #14062/#14011/#14025). Action: IMemoryFilter middleware (prompt-injection scan + time-decay weighting). **Extremely High** (@moonbox3 explicitly acknowledged attack surface, requested validate-before-persist + session isolation).
2. **OpenAI Agents Py/JS — Pre-Approval Tool-Call Governance.** Asset: authority routing ADVISE/EXECUTE/DEFER/STOP. Target: openai-agents-python/-js. Action: typed examples/ leveraging newly-merged pre-approval tool input guardrails (PRs #3487/#1358), ADVISE/EXECUTE routing intercepting high-risk MCP calls. **Very High** (@seratch focus on pre-approval safety).
3. **Google ADK Python Community — Production HITL Gateway.** Asset: authority routing + fleet ops. Target: adk-python-community (issues #111/#141). Action: Pydantic ApprovalRequest + standalone FastAPI service queue manager plugin in plugins/. **Very High** (repo exists for these; HITL/governance = top request).
4. **Google GenAI Processors — Pipeline Concurrency.** Asset: pipeline-vs-barrier concurrency. Target: genai-processors contrib/. Action: PartProcessor with measured speedups via parallel_concat (//) orchestrating sub-agents non-blocking. **High** (v2.0 emphasizes async + ContentStream).
5. **Meta Llama Cookbook — Verifiable Reasoning & Llama 4 Agents.** Asset: reasoning-quality gates + multi-agent consensus. Target: llama-cookbook end-to-end-use-cases/. Action: multi-agent consensus w/ Llama 4 Scout/Maverick + reasoning-quality gates filtering hallucinations. **High** (refactor prioritized Llama 4 + multi-agent).
6. **Groq API Cookbook — High-Speed MCP Sandboxing.** Asset: MCP tooling + tool-call signing + concurrency. Target: groq-api-cookbook. Action: single polished notebook — ultra-low-latency agent orchestrating E2B MCP gateway executing cryptographically signed tool calls. **High** (Groq requests MCP/E2B; must hit 6-point rubric).
7. **Hugging Face Cookbook — SmolAgents Fleet Operations.** Asset: multi-machine fleet ops. Target: huggingface/cookbook. Action: neutral-toned notebook on deploying/orchestrating/state-syncing a smolagents fleet across instances. **Medium-High** (merging swarm recipes #358/#349; must conform to formatting).
8. **AWS Bedrock Flows — Agentic Task Delegation Templates.** Asset: authority routing + consensus. Target: aws-samples/amazon-bedrock-flows-samples. Action: JSON flow template w/ multi-agent collab, dynamic delegation, guardrail prompt nodes. **Medium-High**.
9. **xAI Cookbook — Citation-Faithfulness Evals.** Asset: citation-faithfulness (verbatim gate + LLM judge). Target: xai-cookbook. Action: notebook — Grok self-correction, LLM-as-judge verifying citation faithfulness in RAG. **Medium** (values verifiable reasoning #26/#27, but big backlog).
10. **Firebase Genkit — Multi-Turn Authority Routing w/ Suspended State.** Asset: authority routing. Target: firebase/genkit. Action: remote HTTP agent (TS/Go) via Agents API — pause, serialize state via aix.WithSessionStore, await OOB human approval, resume. **Medium** (Genkit pushing Agents API, needs state-suspension examples).

## Do Not Bother
- **microsoft/autogen** — maintenance mode → MAF.
- **google-gemini/cookbook (core framework changes)** — Copybara clash; route complex agentic logic to genai-processors.
- **mistralai/cookbook** — >40 stale external PRs + unresolved breaking-API issues; lacks DevRel triage.
- **openai/evals (custom code executions)** — only YAML model-graded accepted; shift exec assets to monitorability-evals / inspect_evals.
- **QwenLM/Qwen-Agent & DeepSeek** — corporate drop points; PRs (incl. security) closed unmerged; DeepSeek no cookbook.

_(Источник: канвас-отчёт Gemini Deep Research, секция Sources присутствует в чате; извлечён verbatim 23.07.)_
