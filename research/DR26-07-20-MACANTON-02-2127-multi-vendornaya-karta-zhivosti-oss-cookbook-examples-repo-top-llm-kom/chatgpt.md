# DR26-07-20-MACANTON-02-2127 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "LLM Vendor OSS Contributions"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a5f151d-9ad4-83ed-b5b3-d6d586cd0be8
model: "gpt-5-6-pro"
url: "https://приватный чат"
date_recorded: 2026-07-21
date_added: 2026-07-21
valid_as_of: 2026-07-21
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 40
related_concepts: []
revisit_if: ""
---

# LLM Vendor OSS Contributions

> ChatGPT · 2026-07-21 · `gpt-5-6-pro` · 40 сообщений

## Диалог

**Anton:**

DR26-07-20-MACANTON-02-2127

DEEP RESEARCH REQUEST — "Which official LLM-vendor OSS repos actually merge external contributions right now (July 2026), and what are they asking for?"

CONTEXT. I contribute polished, runnable example/cookbook PRs to official LLM-vendor OSS repos (with live runs, honest AI-authorship disclosure, and credit to issue authors). Six PRs are already open in Anthropic repos (claude-cookbooks #784/#787/#788/#789, claude-agent-sdk-demos #70, skills #1460). My portable assets: (a) multi-agent consensus + adversarial verification (default-refute skeptics), (b) authority routing ADVISE/EXECUTE/DEFER/STOP for tool-using agents, (c) pipeline-vs-barrier concurrency for sub-agent orchestration with measured speedups, (d) citation-faithfulness evals (verbatim gate + LLM judge), (e) reasoning-quality gates, (f) persistent memory / "second brain" patterns for agents, (g) multi-machine agent fleet operations, (h) MCP tooling and tool-call signing.

MY CURRENT LIVENESS MAP (from raw GitHub API scans, 2026-07-20/21) — please VERIFY, CORRECT, and EXTEND it:
- HIGH (external merges proven): google/adk-python-community (a governance plugin by an external author was merged), huggingface/cookbook (repeat external authors, issue #303 "search agent cookbook" open >1 year), microsoft/semantic-kernel (~7 duplicate issues demanding function-invocation approval + memory-poisoning guards), groq/groq-api-cookbook (hours-to-days merges, explicit scoring rubric).
- MED-HIGH: openai/openai-agents-python and -js (minutes-fast maintainer, issue #1097 in -js = approval-with-modification), xai-org/xai-cookbook (fast merges but 2-5-month stale PR backlog), google-gemini/cookbook (issue-first culture, rejects third-party pip deps), google-gemini/genai-processors (contrib/ folder), togethercomputer/together-cookbook, mistralai/cookbook (42 stale external PRs — worrying).
- SKIP verdicts I want double-checked: QwenLM/Qwen-Agent (external PRs incl. security fixes closed unmerged), DeepSeek (no cookbook repo at all, only awesome-lists), Cohere (cookbook archived; active repo merges staff only), meta-llama/llama-cookbook (no real commits since ~Nov 2025), microsoft/autogen (official maintenance mode).

QUESTIONS.
1. VERIFY MERGE-FRIENDLINESS: for each repo above, does the community consensus (blog posts, maintainer statements, contributor complaints/praise on X/Reddit/HN/Discord) match my verdicts? Any repo where the apparent PR graveyard is misleading (e.g., Google's Copybara pattern of closing PRs that actually landed internally — which other orgs do this)?
2. MAINTAINER INTEL: who are the actual gatekeepers (GitHub handles) for external example/cookbook PRs in: huggingface/cookbook, google/adk-python-community, semantic-kernel (Python side), openai-agents-js, xai-cookbook, groq-api-cookbook, google-gemini/cookbook? What do their recent reviews reward and reject?
3. WHAT'S BEING ASKED FOR NOW (July 2026): trending unmet content demands in each of these ecosystems — agent security/guardrails? memory? evals? MCP? multi-agent orchestration? Cite concrete open issues, forum threads, or maintainer roadmap statements.
4. MISSED DOORS: per vendor, are there 1-2 official repos I overlooked that DO welcome external example contributions? Specifically check: Meta (PurpleLlama, llama-models, synthetic-data-kit — anything alive?), OpenAI (evals repo status?), Microsoft (agent-framework community process?), Google (firebase/genkit? other DeepMind repos?), plus Perplexity, Alibaba Cloud (model-studio-cookbook?), NVIDIA (GenerativeAIExamples), AWS (bedrock samples) as secondary ecosystems.
5. NORMS THAT KILL PRs: for Google CLA, Microsoft CLA, and issue-first cultures — what practical mistakes cause external example PRs to die (scope too broad? third-party deps? missing tests/notebook CI? wrong folder?). Give per-repo checklists if documented.
6. TIMING/OPTICS: is there evidence (merged-PR histories) for the best day/format to submit — e.g., small single-purpose notebooks vs multi-file demos — per repo?

OUTPUT FORMAT. (1) Per-repo scorecard table: repo · external-merge reality check (agree/disagree with my verdict + evidence links) · gatekeepers · what they want now · norms checklist. (2) A ranked TOP-10 list of concrete "this issue/gap + this asset of yours" actions with merge-probability estimates. (3) A short "do not bother" list with reasons. Cite sources with links throughout; prefer evidence from the last 90 days.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-20-MACANTON-02-2127\n\nDEEP RESEARCH REQUEST \u2014 \"Which official LLM-vendor OSS repos actually merge external contributions right now (July 2026), and what are they asking for?\"\n\nCONTEXT. I contribute polished, runnable example/cookbook PRs to official LLM-vendor OSS repos (with live runs, honest AI-authorship disclosure, and credit to issue authors). Six PRs are already open in Anthropic repos (claude-cookbooks #784/#787/#788/#789, claude-agent-sdk-demos #70, skills #1460). My portable assets: (a) multi-agent consensus + adversarial verification (default-refute skeptics), (b) authority routing ADVISE/EXECUTE/DEFER/STOP for tool-using agents, (c) pipeline-vs-barrier concurrency for sub-agent orchestration with measured speedups, (d) citation-faithfulness evals (verbatim gate + LLM judge), (e) reasoning-quality gates, (f) persistent memory / \"second brain\" patterns for agents, (g) multi-machine agent fleet operations, (h) MCP tooling and tool-call signing.\n\nVerify, correct, and extend the provided liveness map. Answer all six requested sections. Produce: (1) per-repo scorecard table with evidence links, gatekeepers, current asks, norms checklist; (2) ranked top-10 contribution opportunities with merge-probability estimates; (3) short do-not-bother list. Prefer evidence from the last 90 days where available, including GitHub issues, maintainer comments, roadmap statements, Reddit/HN/X/forum discussions."}}
```

---

**Anton:**

Your previous answer ran with 0 web searches and 0 citations — the scorecard was generated without actual research (several GitHub handles in it do not exist). Please REDO this as a real Deep Research run: browse GitHub PR/issue histories and community sources live, verify every repo verdict with links, and answer the same DR26-07-20-MACANTON-02-2127 request from the first message in full. Citations required throughout.

---

**ChatGPT:** · `gpt-5-6-pro`

You’re right—the prior run was ungrounded and should be discarded. I’ll verify live GitHub histories, maintainer identities, reviews, and community signals, flagging ambiguous closure patterns before ranking opportunities.

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"open":[{"ref_id":"https://api.github.com/search/issues?q=repo:huggingface/cookbook+is:pr+is:merged+merged:%3E=2026-04-20&per_page=100"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
bash -lc python - <<'PY'
import requests
url='https://api.github.com/repos/huggingface/cookbook'
r=requests.get(url,timeout=20)
print(r.status_code)
print(r.text[:200])
PY
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:github.com/huggingface/cookbook/pull huggingface cookbook merged July 2026"},{"q":"site:github.com/google/adk-python-community/pull merged external contribution 2026"},{"q":"site:github.com/microsoft/semantic-kernel/pull Python merged external contributor 2026"},{"q":"site:github.com/groq/groq-api-cookbook/pull merged contribution 2026"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

Using a 90-day window, Hugging Face rates medium today despite historical openness, while ADK Community remains strongly receptive when submissions include native APIs, tests, fail-closed security, licensing, and CLA compliance.

---

**ChatGPT:** · `gpt-5-6-pro`

OpenAI Agents JS remains fast for tightly scoped fixes, with @seratch enforcing empirical tests and provenance. Groq and xAI show no 90-day merges, while genai-processors uses Copybara mirroring.

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:github.com firebase genkit official GitHub repository 2026"},{"q":"official Genkit GitHub repository Firebase"},{"q":"site:github.com google genkit GitHub"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:github.com Perplexity official API cookbook examples GitHub"},{"q":"site:github.com Alibaba Cloud Model Studio cookbook samples official GitHub"},{"q":"site:github.com/NVIDIA GenerativeAIExamples official GitHub"},{"q":"site:github.com/aws-samples amazon bedrock samples GitHub"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"\"jliounis\" Perplexity"},{"q":"site:linkedin.com/in jliounis Perplexity"},{"q":"site:github.com/jliounis Perplexity API cookbook"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"open":[{"ref_id":"https://api.github.com/repos/aws-samples/amazon-bedrock-samples/pulls/715"},{"ref_id":"https://api.github.com/repos/perplexityai/api-cookbook/pulls/67"},{"ref_id":"https://api.github.com/repos/perplexityai/search_evals/pulls/16"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:reddit.com/r/LocalLLaMA GitHub cookbook pull request OpenAI Anthropic Google ADK contributor"},{"q":"site:news.ycombinator.com \"openai-agents\" GitHub contributions"},{"q":"site:reddit.com \"xai-cookbook\" OR \"mistralai/cookbook\" pull request"},{"q":"site:reddit.com \"google/adk-python-community\" OR \"google-gemini/cookbook\" contribution"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:opensource.microsoft.com CLA Microsoft GitHub pull request contributor license agreement official"},{"q":"site:github.com/microsoft \"Microsoft CLA\" pull request bot contributor license agreement"},{"q":"site:opensource.google/documentation/reference/cla Google CLA pull request official"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:opensource.microsoft.com Microsoft CLA GitHub contributors CLA bot official"},{"q":"site:microsoft.github.io contributor license agreement Microsoft GitHub CLA official"},{"q":"site:github.com/microsoft \"Microsoft CLA\" \"cla.opensource.microsoft.com\" contributing"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:reddit.com GitHub \"huggingface/cookbook\" pull request contribution maintainer review"},{"q":"site:reddit.com OR site:news.ycombinator.com \"google-gemini/cookbook\" contribution PR"},{"q":"site:reddit.com OR site:news.ycombinator.com \"openai-agents-js\" pull request maintainer"},{"q":"site:reddit.com OR site:news.ycombinator.com \"xai-cookbook\" GitHub pull request"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

Microsoft Agent Framework now outranks Semantic Kernel; unclaimed issues #7211 and #7230 fit memory and fleet assets. AgentScope, AWS AgentCore Samples, and DeepSeek’s integration repo are additional live doors.

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"skills://plugins/github/github/skill.md","start_line":1,"num_lines":240}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"paths":["GitHub"],"query":"pull"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"paths":["GitHub"],"query":"issue"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn2","start_line":1,"num_lines":120}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"paths":["GitHub"],"query":"repository"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn4","query":"merged_at"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn3","query":"total_count"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn3","query":"https://github.com/google/adk-python-community/pull/"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn8","query":"merged_at"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn15","query":"merged_at"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn15","start_line":120,"num_lines":60}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn23","query":"merged_at"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn32","query":"merged_by"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn40","query":"merged_at"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"paths":["GitHub"],"query":"search_issues"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn78","query":"merged_by"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn143","query":"'user': {'login':"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn125","query":"merged_by"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"paths":["GitHub"],"query":"user"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"paths":["GitHub"],"query":"fetch_issue"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"uri":"/response/turn170","query":"rajesh"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

# DR26-07-20-MACANTON-02-2127 — verified July 2026 map

## Bottom line

Your map substantially **overrates Groq, xAI, Hugging Face’s current velocity, Together, and new-feature work in Semantic Kernel**. It underrates or misses the best current doors:

1. **microsoft/agent-framework**
2. **genkit-ai/genkit**
3. **agentscope-ai/agentscope**
4. **awslabs/agentcore-samples**
5. **google/adk-python-community**
6. **openai/openai-agents-python / openai-agents-js**
7. **google-gemini/genai-processors**
8. **openai/openai-cookbook**, selectively

The strongest demand is not “another multi-agent demo.” It is:

- preserving tool-call/result and approval state across persistence, compaction, reconnects, and resume;
- bounded concurrency, cancellation, and backpressure;
- MCP schema validation, authorization, and trust boundaries;
- executable governance and human-approval semantics;
- narrow evals tied to a real failure mode;
- fixing stale notebooks and migrations with live verification.

I used **April 22–July 21, 2026** as the primary window, then inspected older history where the 90-day sample was sparse. I counted a repository as externally merge-friendly only when an externally submitted PR actually landed; bot mirrors and obviously vendor-internal PRs were not treated as proof. Where author employment was ambiguous, I did not use the PR as strong external evidence.

A limitation: there is no reliable, broad “community consensus” corpus for most of these exact repos. Recent Reddit/HN/X/Discord discussion is sparse and highly selective. The strongest evidence is therefore merged-PR history, review threads, contribution docs, and explicit maintainer statements. Groq is the clearest example where public community messaging remains welcoming while current GitHub merge activity has gone cold. citeturn322091search5turn516371search7

---

# 1. Per-repo scorecard

## Original liveness map

| Repo | External-merge reality check | Gatekeepers and review behavior | What they want now / PR-killers |
|---|---|---|---|
| [`google/adk-python-community`](https://github.com/google/adk-python-community) — **HIGH; agree** | Clear external proof. [`#141`](https://github.com/google/adk-python-community/pull/141), an externally authored governance plugin, was merged after the contributor replaced an earlier PR and satisfied substantive architecture and safety review. Google explicitly positions this as the contribution-oriented ADK community repo. fileciteturn4file0L1-L1 citeturn991391search0 | Primary visible gatekeeper: **[@DeanChensj](https://github.com/DeanChensj)**. The review rewarded native `BasePlugin` use, asynchronous callbacks, fail-closed defaults, correct folder placement, tests, and a usable sample. It rejected incorrect Google licensing, non-native abstractions, unsafe defaults, and incomplete test coverage. fileciteturn5file0L1-L1 | Best live fits: memory backends and governance/audit extensions. [`#142`](https://github.com/google/adk-python-community/issues/142) asks for a post-execution, independently verifiable audit trail complementing the governance plugin. Killers: no coordination with the issue author, custom framework instead of ADK hooks, weak failure semantics, missing CLA/header/tests. fileciteturn174file0L1-L3 |
| [`huggingface/cookbook`](https://github.com/huggingface/cookbook) — **MEDIUM now; HIGH historically** | Downgrade. Only one merge appeared in the 90-day scan, although external notebook PRs such as [`#341`](https://github.com/huggingface/cookbook/pull/341) and [`#333`](https://github.com/huggingface/cookbook/pull/333) prove historical openness. This is not presently a high-throughput target. fileciteturn8file0L1-L1 fileciteturn9file0L1-L1 | Most visible reviewer: **[@sergiopaniego](https://github.com/sergiopaniego)**; recurring relevant handles include **[@merveenoyan](https://github.com/merveenoyan)** and **[@stevhliu](https://github.com/stevhliu)**. Reviews reward runnable Colab paths, model-choice rationale, clear interpretation of results, and neutral educational framing. Promotional/vendor language and unexplained complexity get pushed back. | Better current work is maintenance: [`#316`](https://github.com/huggingface/cookbook/issues/316) reports a broken multi-agent recipe due to removed `smolagents.ManagedAgent`; [`#326`](https://github.com/huggingface/cookbook/issues/326) asks for a more realistic graph-plus-Agent-RAG treatment. [`#303`](https://github.com/huggingface/cookbook/issues/303) is not clean white space because a related search-agent/vector-search cookbook has already landed. fileciteturn175file1L9-L18 fileciteturn175file2L19-L25 fileciteturn175file3L27-L46 |
| [`microsoft/semantic-kernel`](https://github.com/microsoft/semantic-kernel) — **MED-HIGH for fixes; LOW for new features** | Your merge-friendliness call is directionally right for targeted Python bugs and security hardening, but wrong strategically for new components. Microsoft now identifies **Agent Framework as the production successor**. Semantic Kernel still merges fixes such as MCP loopback/Origin hardening, but substantial new work can be redirected. citeturn991391search2 fileciteturn143file5L51-L62 | Python-side visible owners/reviewers include **[@eavanvalkenburg](https://github.com/eavanvalkenburg)** and **[@TaoChenOSU](https://github.com/TaoChenOSU)**. Recent work rewards exact security boundaries, cross-language consistency where relevant, regression tests, and full formatting/test compliance. | Open demand includes function/tool governance, indirect prompt-injection controls, memory-poisoning resistance, MCP trust verification, and persistent memory. But many similarly worded “governance” issues are vendor pitches or duplicates rather than roadmap commitments. For net-new APIs, target Agent Framework instead. fileciteturn59file2L68-L79 fileciteturn61file6L182-L193 |
| [`groq/groq-api-cookbook`](https://github.com/groq/groq-api-cookbook) — **LOW now; historically friendly** | Downgrade from HIGH. I found **zero merges in the last 90 days**. The prior external wave was largely late-2025 MCP integrations, including [`#103`](https://github.com/groq/groq-api-cookbook/pull/103). The explicit scoring rubric remains real, but current liveness does not. fileciteturn23file0L1-L1 | Visible reviewers from the active period: **[@janzheng](https://github.com/janzheng)**, **[@shaunjoshi](https://github.com/shaunjoshi)**, and **[@ozenhati](https://github.com/ozenhati)**. Reviews rewarded complete OAuth setup, `.env.example`, pinned requirements, multiple runnable use cases, dependency hygiene, and consistent model names. | Public Groq messaging still emphasizes community cookbook contributions and MCP connectors, but GitHub behavior is presently cold. The rubric scores usefulness, originality, clarity, accuracy, depth, and writing quality; below roughly 3/5 on any major dimension is a rejection risk. Do not submit until a maintainer confirms active review bandwidth. fileciteturn25file0 citeturn516371search5turn322091search5 |
| [`openai/openai-agents-python`](https://github.com/openai/openai-agents-python) — **HIGH for narrow, exact work** | Agree. Recent small external-looking fixes have merged extremely quickly; representative [`#3898`](https://github.com/openai/openai-agents-python/pull/3898) was a two-file regression fix merged in roughly 74 minutes. Large behavior changes face a much higher bar. fileciteturn63file0L1-L3 | **[@seratch](https://github.com/seratch)** is a recurring cross-SDK gatekeeper; other area owners appear by subsystem. Reviews reward reproducible failures, minimal compatibility-preserving patches, explicit lifecycle semantics, and complete verification. | Current demand clusters around sessions, approval/history integrity, persistence, MCP lifecycle, and tracing. [`#3738`](https://github.com/openai/openai-agents-python/issues/3738) asks for turn/run-aware session retrieval because item-based slicing can orphan tool calls and results. It is a high-impact API discussion, so a fixture/test-only first PR is safer than a new public API. fileciteturn164file0L1-L3 fileciteturn71file0L59-L76 |
| [`openai/openai-agents-js`](https://github.com/openai/openai-agents-js) — **HIGH; agree** | Clear external merge path. In [`#1474`](https://github.com/openai/openai-agents-js/pull/1474), the maintainer reproduced the reported provider behavior against a concrete LiteLLM version, rejected an artificial-only case and provider-specific wording, then approved after comments and tests were generalized. fileciteturn16file0 | Primary visible gatekeeper: **[@seratch](https://github.com/seratch)**. Rewards: empirical reproducer, provider-neutral contract, exact tests and comments, backward compatibility. Rejects: constructed-only failures, stale version claims, broad provider-specific special cases. | Strong current gap: [`#1097`](https://github.com/openai/openai-agents-js/issues/1097), approval with modified arguments, including the requirement that execution, persisted history, `previous_response_id`, conversation-backed runs, and resumed sessions all agree on what was actually approved. There is already a prototype, so offer conformance tests/docs rather than a competing implementation. |
| [`xai-org/xai-cookbook`](https://github.com/xai-org/xai-cookbook) — **LOW; disagree with MED-HIGH** | No merges in the last 90 days and a large stale queue. Open PRs already cover human sign-off, verifiable reasoning, MLflow tracing, MCP, research/citations, and PDFs. Recent merged mobile work was xAI-associated, so it is not evidence of an open external funnel. The repo’s last visible push was April 16, 2026. citeturn322091search12 | **No stable recurring public external-PR gatekeeper could be established.** A representative older merge, [`#7`](https://github.com/xai-org/xai-cookbook/pull/7), had no public human review trail. fileciteturn154file0L1-L1 | Almost every one of your portable assets collides with an already-open PR: human approval, verifiable reasoning, research/citations, tracing, and MCP. Norms require end-to-end notebook execution, visible outputs, minimal dependencies, no committed keys, and `registry.yaml`; the real blocker is review capacity, not packaging. |
| [`google-gemini/cookbook`](https://github.com/google-gemini/cookbook) — **MED-HIGH for maintenance; MED/LOW for new notebooks** | Your assessment is mostly right but needs bifurcation. Small compatibility, auth, quickstart, and Interactions API updates merge. A substantial external notebook such as [`#790`](https://github.com/google-gemini/cookbook/pull/790) took roughly a year; a docs cleanup such as [`#1280`](https://github.com/google-gemini/cookbook/pull/1280) took around two weeks. fileciteturn40file0L1-L1 citeturn991391search3 | Visible gatekeepers/reviewers: **[@Giom-V](https://github.com/Giom-V)**, **[@markmcd](https://github.com/markmcd)**, **[@kkorpal](https://github.com/kkorpal)**, and notebook reviewer **[@andycandy](https://github.com/andycandy)**. They reward pedagogy, first-use imports, explanation of complex cells, concise presentation, template compliance, and current Gemini APIs. | New content is strictly issue-first. Killers: missing Google CLA, missing Colab head-branch link, `nbfmt`/`nblint` failures, unlicensed media, hard-coded stale metadata, unexplained model choice, gratuitous third-party dependencies, and broad integration marketing. Current merge history suggests maintainers want migration and API-correctness work more than another general agent architecture. fileciteturn39file0 |
| [`google-gemini/genai-processors`](https://github.com/google-gemini/genai-processors) — **MED-HIGH; agree with Copybara caveat** | A real external path exists through `genai_processors/contrib`, but some accepted changes reappear as `copybara-service[bot]` mirror PRs. Counting only the bot PR would falsely make the repo look internal-only. External [`#15`](https://github.com/google-gemini/genai-processors/pull/15) demonstrates the contribution route. fileciteturn54file0L1-L1 | Primary visible gatekeeper: **[@kibergus](https://github.com/kibergus)**. Review rewards native processor/data abstractions, generic provider support, streaming consistency, fail-loud unsupported media behavior, and tests under `contrib/tests`. It rejects provider formatting logic and oversized framework integrations. | Excellent live target: [`#164`](https://github.com/google-gemini/genai-processors/issues/164), unbounded queues, missing cancellation, and task/resource leaks in `streams.concat`. The contribution contract expects implementation, Markdown docs, and tests; the maintainers explicitly warn that review bandwidth is limited. fileciteturn157file0L1-L3 fileciteturn48file0L3-L48 |
| [`togethercomputer/together-cookbook`](https://github.com/togethercomputer/together-cookbook) — **MED-LOW externally** | Active repository, but recent merges are dominated by authors whose external status is unclear. The README welcomes PRs and Discord outreach, but I did not find strong recent proof of a repeat external cookbook pipeline. fileciteturn149file0L13-L20 | No strong public reviewer pattern comparable to ADK/OpenAI/HF was established. Contact through the repo’s Discord before doing substantive work. | High duplication risk: the cookbook already contains serial chains, conditional routing, parallel agents, orchestrator/subtask agents, a cross-provider advisor, deep research, and several LLM-judge/eval notebooks. Your consensus, orchestration, advisor-routing, and generic eval assets need a sharply different measured failure mode to avoid “already covered.” fileciteturn149file0L31-L53 |
| [`mistralai/cookbook`](https://github.com/mistralai/cookbook) — **MED for maintenance; LOW-MED for new concepts** | Maintenance has restarted: SDK migrations, compatibility fixes, automated review, and stale-notebook detection are merging. However, the open PR queue is now about **49**, not 42, and contains numerous external agent/eval/reasoning proposals. citeturn324017view0 | No stable external-content gatekeeper could be established from the sampled history. Extremely fast recent merges appear associated with trusted/known contributors and should not be generalized to a random new notebook. | Best current work is generated maintenance. [`#300`](https://github.com/mistralai/cookbook/issues/300) flags the stale evaluation notebook’s deprecated `open-mistral-7b`. Killers: secrets, unpinned dependencies, non-Colab code, warnings/errors in outputs, unreadable cells, missing README registry update, or turning a maintenance PR into a new framework. fileciteturn161file0L1-L3 fileciteturn83file0 |
| [`QwenLM/Qwen-Agent`](https://github.com/QwenLM/Qwen-Agent) — **SKIP; agree** | Confirmed. No merges in the 90-day scan and only two small merges in the preceding year, while security and correctness PRs accumulate. [`#871`](https://github.com/QwenLM/Qwen-Agent/pull/871), an SSRF fix with tests, was closed by its author after two weeks without response. fileciteturn89file0L1-L1 | No functioning external gatekeeper path was evident. | Open proposals include execution approval gates, remote MCP review gates, timeout fixes, and tool-call ID integrity—but the issue/PR graveyard makes these poor investments. The fact that a change is security-related does not rescue its merge probability here. |
| DeepSeek — **no cookbook, but not a total SKIP** | You are right that DeepSeek has no conventional cookbook. The overlooked official door is [`deepseek-ai/awesome-deepseek-agent`](https://github.com/deepseek-ai/awesome-deepseek-agent), which has recently merged external bilingual integration guides. [`#93`](https://github.com/deepseek-ai/awesome-deepseek-agent/pull/93) credited Qwen Code generation and still merged. fileciteturn92file0L1-L1 citeturn854610search0 | Visible gatekeeper: **[@tianyicui](https://github.com/tianyicui)**. Review behavior is lightweight: resolve conflicts, match the bilingual integration format, and document a real external tool/client. | Good for a concise integration guide, not for a new orchestration/eval framework. AI-authorship disclosure is evidently not disqualifying. |
| Cohere — **LOW, but “staff only” is too strong** | The old notebooks/cookbook surfaces are archived, but [`cohere-ai/cohere-developer-experience`](https://github.com/cohere-ai/cohere-developer-experience) explicitly welcomes contributions. External typo PR [`#140`](https://github.com/cohere-ai/cohere-developer-experience/pull/140) eventually merged, although after a very long delay. fileciteturn94file0L1-L1 | Visible reviewer on that merge: **[@billytrend-cohere](https://github.com/billytrend-cohere)**. | The active repo has a **one-way internal sync** for OpenAPI/snippet content; external changes may be ported internally and later overwritten or reintroduced. That makes apparent close/overwrite events ambiguous, but it also makes large external example PRs unattractive. fileciteturn95file0L31-L39 |
| [`meta-llama/llama-cookbook`](https://github.com/meta-llama/llama-cookbook) — **SKIP; agree** | No merges in the 90-day window and roughly 68–69 open PRs. A representative external documentation fix, [`#981`](https://github.com/meta-llama/llama-cookbook/pull/981), took months in 2025; there is no 2026 signal supporting new example work. fileciteturn99file0L1-L1 citeturn768558search7 | Historical merger visible on #981: **[@IgorKasianenko](https://github.com/IgorKasianenko)**. That is not evidence of a currently staffed external funnel. | Do not invest in a new cookbook. Small critical corrections only. |
| [`microsoft/autogen`](https://github.com/microsoft/autogen) — **SKIP for features; agree** | Microsoft explicitly describes AutoGen as being in maintenance mode and directs new development toward Agent Framework. citeturn174367search0 | Maintenance owners only. | Bugs, security, and docs may still be valid. New memory, orchestration, authority-routing, or MCP architecture belongs in Agent Framework. |

---

## Missed doors

| Repo | Current external reality | Best fit for your assets |
|---|---|---|
| [`microsoft/agent-framework`](https://github.com/microsoft/agent-framework) — **HIGH** | This is now the best Microsoft door. Externally authored Python fixes land after focused review; representative [`#7124`](https://github.com/microsoft/agent-framework/pull/7124) was a narrow issue-linked regression fix approved by `@eavanvalkenburg`. The repo requires issue-first alignment for nontrivial changes and warns against surprise APIs. fileciteturn101file0L1-L1 citeturn174367search3 | Session/history integrity, approval persistence, compaction correctness, disconnect-safe execution, structured HITL, and memory. [`#7212`](https://github.com/microsoft/agent-framework/issues/7212) is an unusually strong live target: reproducible, unassigned, automatically confirmed, and directly aligned with tool-call/result integrity. fileciteturn156file0L1-L3 fileciteturn163file0L1-L3 |
| [`genkit-ai/genkit`](https://github.com/genkit-ai/genkit) — **HIGH** | Major Google/Firebase-adjacent missed door. Externally authored RFC-aligned features and fixes merge after serious review. [`#5475`](https://github.com/genkit-ai/genkit/pull/5475) added public model-mocking utilities and was reviewed for serialization, rendering, tests, and API shape. fileciteturn119file0L1-L1 citeturn550750search0 | MCP input-schema conversion and runtime validation are literally open TODO issues: [`#4366`](https://github.com/genkit-ai/genkit/issues/4366) and [`#4365`](https://github.com/genkit-ai/genkit/issues/4365). Also relevant: approval/resume behavior, interrupts, testing utilities, and MCP trace identity. fileciteturn159file0L1-L3 fileciteturn160file0L1-L3 |
| [`agentscope-ai/agentscope`](https://github.com/agentscope-ai/agentscope) — **HIGH** | Best Alibaba-associated door. It is actively merging external security, permission, persistence, and context-integrity work. In [`#2004`](https://github.com/agentscope-ai/agentscope/pull/2004), an external contributor’s Bash permission fix was accepted only after `@DavdGao` required use of the project’s existing tree-sitter AST rather than a second, fragile parsing paradigm. fileciteturn152file0L1-L1 citeturn844092search0 | Authority routing, concurrent approval correctness, multi-machine event delivery, persistent memory, context compression, and MCP security. [`#2123`](https://github.com/agentscope-ai/agentscope/issues/2123) is a deterministic replay/live race that silently loses team HITL events—an excellent concurrency/fleet-operations fit. fileciteturn171file0L1-L3 |
| [`awslabs/agentcore-samples`](https://github.com/awslabs/agentcore-samples) — **HIGH** | Very active and demonstrably external-friendly when the strict process is followed. External Dash0 integration [`#1539`](https://github.com/awslabs/agentcore-samples/pull/1539) merged after scope/folder review. The repo wants issue-first proposals, focused changes, local tests, CI follow-through, and complete documentation. fileciteturn140file0L1-L1 fileciteturn145file0L25-L46 | Best AWS target for governance, Cedar policies, MCP, evaluation, memory, HITL, and tool authorization. Several apparent gaps already have “implementation ready” authors, so collaborate rather than duplicate. |
| [`openai/openai-cookbook`](https://github.com/openai/openai-cookbook) — **MED-HIGH, selective** | Active and still merges external vendor-neutral examples. [`#2772`](https://github.com/openai/openai-cookbook/pull/2772), an Oracle-backed Agents SDK memory example, received concrete review on registry metadata, dependency consistency, and why the third-party backend added value beyond built-in sessions/SQLite/Redis/SQLAlchemy/Conversations/compaction. fileciteturn114file0L1-L1 fileciteturn115file0L1-L1 | Citation-faithfulness or reasoning-gate content could fit if it is a small, runnable Agents SDK cookbook with a clear failure mode and no framework marketing. Review remains best-effort with no merge guarantee. fileciteturn116file0 |
| [`perplexityai/api-cookbook`](https://github.com/perplexityai/api-cookbook) — **MEDIUM; external proof not fully established** | Active repo, explicitly inviting examples, showcases, and advanced articles. Recent work focuses on Agent API, `finance_search`, sandbox execution, live sources, and research workflows. Some recent author affiliations were ambiguous, so I would not yet rate it HIGH externally. citeturn257635search0 | Strong semantic fit for citation-faithfulness evals. The repo wants MDX; full apps stay in separate public repos. Required content includes real-world value, setup, code explanation, limitations, tested code, environment variables, and error handling. fileciteturn148file0L3-L31 fileciteturn148file0L81-L107 |
| [`aws-samples/amazon-bedrock-samples`](https://github.com/aws-samples/amazon-bedrock-samples) — **MEDIUM** | Active, but current author affiliation is often AWS-adjacent. A good recent example, [`#715`](https://github.com/aws-samples/amazon-bedrock-samples/pull/715), documented a live prompt-optimization run, recorded score improvement from 0.42 to 1.0, and also included a replay mode in which all notebook cells passed. fileciteturn137file1L9-L27 citeturn982882search3 | Good for measured eval and optimization notebooks, but AgentCore Samples is the stronger fit for your governance and agent-runtime assets. |
| [`NVIDIA/GenerativeAIExamples`](https://github.com/NVIDIA/GenerativeAIExamples) — **LOW-MED externally** | Alive and has a `community/` area, but only one merge appeared in the 90-day scan and the representative author was NVIDIA-associated. There is not enough recent proof of a reliable external funnel. citeturn119442search0 | Thematic fit exists for guardrails, parallel rails, RAG, HITL, and citation evaluation. Get explicit folder/maintainer approval before building. |
| [`meta-llama/synthetic-data-kit`](https://github.com/meta-llama/synthetic-data-kit) — **LOW-MED** | Historical external merges exist for chain-of-thought curation, parsers, configuration, and rolling summaries, but no 90-day merge activity. Approximately 17 PRs remain open. citeturn381215view0 | Reasoning-quality gates could fit, but current liveness is too weak to rank above the targets above. |
| [`meta-llama/PurpleLlama`](https://github.com/meta-llama/PurpleLlama) — **SKIP** | Internal re-sync commits exist, but there is no evidence that the stale external security/eval PR queue is quietly landing through those syncs. Do not interpret “re-sync with internal repository” as Copybara proof for community contributions. |
| [`meta-llama/llama-models`](https://github.com/meta-llama/llama-models) — **SKIP for examples** | Virtually no recent external merge signal. It is not a hidden cookbook door. |
| [`openai/evals`](https://github.com/openai/evals) — **SKIP** | No merges in the 90-day scan; the last visible wave was internal-ish maintenance. The README now points broader evaluation work toward the managed Evals API, while old external eval PRs remain open for years. | Put eval assets into OpenAI Cookbook, Agents SDK examples, or the Evals API—not a new OSS Evals registry PR. |
| [`dashscope/dash-cookbook`](https://github.com/dashscope/dash-cookbook) — **SKIP** | I found no merged PRs in either the 90-day or one-year scan. citeturn317320search7 | Use AgentScope core instead for Alibaba-associated agent work. |

---

## Requested gatekeeper map

These are **visible reviewers/approvers from sampled recent PR histories**, not a claim that they are the only CODEOWNERS.

| Repo | Visible gatekeepers | What gets rewarded | What gets rejected or delayed |
|---|---|---|---|
| Hugging Face Cookbook | `@sergiopaniego`; recurring `@merveenoyan`, `@stevhliu` | Free/runnable path, neutral education, model rationale, interpreted results, clean notebook/TOC metadata | Promotional framing, unnecessary paid/vendor dependency, unexplained model choice, duplicate content |
| Google ADK Community | `@DeanChensj` | Native ADK plugin/service design, fail-closed behavior, async hooks, focused module, tests and sample | Wrong license/header, custom abstraction, fail-open behavior, missing tests, wrong folder |
| Semantic Kernel Python | `@eavanvalkenburg`, `@TaoChenOSU` | Exact security boundary, regression tests, compatibility, current Python style/tooling | New feature without issue alignment, cross-cutting public API, work better suited to Agent Framework |
| OpenAI Agents JS | `@seratch` | Concrete reproducer, provider-neutral contract, exact comments/tests, all supported history/session modes | Artificial-only reproduction, stale provider claim, special case that does not generalize |
| xAI Cookbook | No recurring public external gatekeeper identified | Repository docs say end-to-end outputs, minimal deps, `registry.yaml` | The practical failure mode is silence/backlog, not a clearly documented technical rejection |
| Groq Cookbook | `@janzheng`, `@shaunjoshi`, `@ozenhati` in the last active wave | Full setup, multiple real use cases, OAuth/env hygiene, neutral writing, high rubric score | Thin hello-world integration, marketing, missing dependency/setup detail |
| Gemini Cookbook | `@Giom-V`, `@markmcd`, `@kkorpal`, `@andycandy` | Pedagogy, current Gemini APIs, concise cells, first-use imports, licensed inputs, template/lint compliance | Unsolicited broad content, stale API/model, missing CLA/Colab link, excessive dependencies, unlicensed data |

---

## Copybara and internal-sync exceptions

### Confirmed misleading graveyards

**`google-gemini/genai-processors`: yes.** Contributor PRs may be reviewed under the contributor’s branch and then represented by `copybara-service[bot]`. Always follow replacement links, merge commits, and attribution rather than counting only the final bot author.

**`cohere-ai/cohere-developer-experience`: partly.** The repo documents a one-way sync from an internal source for OpenAPI/snippet content. An external change can be accepted conceptually but later appear as an internal resync. fileciteturn95file0L31-L39

### Not demonstrated

I found no comparable evidence for OpenAI Agents, Microsoft Agent Framework/Semantic Kernel, xAI Cookbook, Groq Cookbook, Mistral Cookbook, Qwen-Agent, or Together Cookbook.

PurpleLlama’s internal re-sync commits are not enough to infer that external PRs were imported. Treat the external queue as stale unless a maintainer explicitly references the internal landing.

---

# 2. Ranked TOP-10 contribution actions

The probabilities below are subjective estimates of:

> **P(merge | you first obtain required issue/maintainer alignment, preserve issue-author credit, submit a focused PR, and pass all repo checks).**

Without prior alignment, subtract roughly **10–25 percentage points**, especially for Google cookbook content, Microsoft APIs, AWS samples, and OpenAI session changes.

## 1. Microsoft Agent Framework — repair compaction pairing in `#7212`

**Gap:** [`microsoft/agent-framework#7212`](https://github.com/microsoft/agent-framework/issues/7212) is open, unassigned, reproducible, and automatically marked reproduced. Compaction groups function calls/results by adjacency, allowing strict providers to receive orphaned results after HITL/resume traffic. fileciteturn156file0L1-L3 fileciteturn163file0L1-L3

**Your asset:** persistent-memory correctness, reasoning/history gates, adversarial verification.

**Best PR shape:**

- one failing regression covering non-adjacent `call_id` pairs;
- pair by `call_id` or add a post-compaction replay-validity invariant;
- test every affected compaction strategy;
- distinguish completed pairs from legitimately pending calls;
- no new public abstraction unless maintainers request it.

**Estimated probability:** **60–75%**

This is the best combination of active repo, externally proven funnel, concrete production bug, narrow scope, and strong existing reproducer.

---

## 2. GenAI Processors — bounded backpressure and cancellation in `#164`

**Gap:** [`google-gemini/genai-processors#164`](https://github.com/google-gemini/genai-processors/issues/164) documents unbounded `asyncio.Queue` growth, full buffering of later multimodal streams, leaked tasks after early close, and cancellation that can hang while attempting to enqueue a sentinel. fileciteturn157file0L1-L3

**Your asset:** pipeline-versus-barrier concurrency with measured speedups.

**Best PR shape:**

- bounded queue with a conservative default;
- `await queue.put()` backpressure rather than `put_nowait`;
- deterministic early-close and cancellation tests;
- memory-bound test using a slow consumer and large synthetic frames;
- benchmark showing throughput/memory trade-off;
- one implementation PR, one focused docs update.

**Estimated probability:** **55–70%**

Avoid turning this into a generic orchestration package. Fix `streams.concat` and document the performance envelope.

---

## 3. AgentScope — close the replay/live HITL race in `#2123`

**Gap:** [`agentscope-ai/agentscope#2123`](https://github.com/agentscope-ai/agentscope/issues/2123) demonstrates that replay and projection occur before live subscription, so a concurrent worker HITL event can be accepted by Redis yet never reach the leader UI. The issue includes a deterministic real-Redis reproducer and a scoped subscribe-first/buffer/deduplicate fix. fileciteturn171file0L1-L3

**Your asset:** multi-machine fleet operations, concurrency, durable HITL.

**Best PR shape:**

- credit and coordinate with issue author `@yuminshen`;
- subscribe and await readiness before replay;
- buffer live events while replaying;
- deduplicate overlap by Redis Stream entry ID;
- deterministic real-Redis or repository-native integration test;
- cancellation-before-ready test;
- leave cursor/retention redesign for a separate RFC.

**Estimated probability:** **55–70%**

AgentScope recently merged similarly exact permission/security fixes after substantive human review, making this a better bet than most vendor cookbooks. fileciteturn152file0L1-L1

---

## 4. Genkit Python MCP — split `#4366` and `#4365`

**Gaps:**

- [`#4366`](https://github.com/genkit-ai/genkit/issues/4366): convert MCP `inputSchema` to Genkit JSON schema.
- [`#4365`](https://github.com/genkit-ai/genkit/issues/4365): validate tool arguments against that schema and return useful validation errors. fileciteturn159file0L1-L3 fileciteturn160file0L1-L3

**Your asset:** MCP tooling, signed/validated tool calls, authority routing.

**Best sequence:**

1. PR 1: schema conversion and fixtures, no behavior change beyond correct schema exposure.
2. PR 2: argument validation, negative tests, nested objects/arrays/enums/additional properties.
3. Optional later RFC: cryptographic call envelope or authorization policy—not in either first PR.

**Estimated probability:** **50–65%**

Combining conversion, validation, signing, and policy in one PR would sharply reduce the odds.

---

## 5. Mistral Cookbook — repair the stale evaluation notebook in `#300`

**Gap:** [`mistralai/cookbook#300`](https://github.com/mistralai/cookbook/issues/300) is an automatically generated July 20 issue identifying the deprecated `open-mistral-7b` model in `mistral/evaluation/evaluation.ipynb`. fileciteturn161file0L1-L3

**Your asset:** citation-faithfulness and reasoning-quality gates.

**Best PR shape:**

- first make the existing notebook current and fully runnable;
- replace deprecated SDK/model usage;
- pin dependencies and preserve Colab compatibility;
- include fresh outputs;
- only add a compact verbatim-evidence gate plus judge comparison if the maintainer agrees in the issue;
- do not introduce an “eval framework.”

**Estimated probability:**  
**55–70%** for the maintenance-only repair; **35–50%** if you add a new evaluation section.

This is lower prestige than a new agent demo but materially more mergeable.

---

## 6. ADK Community — collaborate on tamper-evident execution audit in `#142`

**Gap:** [`google/adk-python-community#142`](https://github.com/google/adk-python-community/issues/142) asks for outcome records—tool name, argument hash, output hash, timestamp—to complement pre-execution allow/review/block decisions. fileciteturn174file0L1-L3

**Your asset:** tool-call signing and authority chains.

**Best PR shape:**

- first contact and credit issue author `@giskard09`;
- define a small `AuditBackend` protocol rather than embedding a ledger;
- ship a deterministic local JSONL/hash-chain backend;
- use canonical serialization and explicit versioning;
- independently verify tampering, reordering, deletion, and mismatched arguments;
- keep on-chain or third-party backends outside the core PR.

**Estimated probability:** **45–60%**

The adjacent governance plugin has already passed the same maintainer’s review, which is strong architectural precedent. Do not compete with the issue author.

---

## 7. OpenAI Agents JS — conformance support for modified approvals in `#1097`

**Gap:** [`openai/openai-agents-js#1097`](https://github.com/openai/openai-agents-js/issues/1097) proposes approving a tool call with modified arguments. The maintainer explicitly requires consistency across manual history, `previous_response_id`, conversation-backed runs, and resumed sessions.

**Your asset:** ADVISE/EXECUTE/DEFER/STOP authority semantics and approval auditability.

**Best contribution:**

- do not replace the existing prototype;
- offer a table-driven conformance suite;
- assert that the executed arguments equal the approved-modified arguments;
- assert persisted history and tracing do not retain the rejected original as if executed;
- test rejection, modification, repeated resume, session backend, and conversation backend;
- add docs describing the authority boundary.

**Estimated probability:** **40–55%**, conditional on `@seratch` inviting the test/docs contribution.

---

## 8. AWS AgentCore Samples — strengthen the claimed Cedar coding-agent sample

**Gap:** [`awslabs/agentcore-samples#1587`](https://github.com/awslabs/agentcore-samples/issues/1587) proposes Cedar controls for workspace paths, shell commands, and tool access, with 18 ALLOW/DENY scenarios. The issue author says implementation is ready. fileciteturn158file0L1-L3

**Your asset:** authority routing plus adversarial verification.

**Action:** collaborate with `@psv901`; do **not** open a parallel PR.

Offer a bounded addition:

- map read-only, reversible, irreversible, and forbidden operations to authority classes;
- add adversarial path traversal, shell indirection, encoding, and multi-command test cases;
- verify policy decision versus actual execution outcome;
- add one audit table, not a second policy engine.

**Estimated probability:** **40–55%** as a collaboration; near zero as a competing implementation.

---

## 9. OpenAI Agents Python — fixture-only history boundary matrix for `#3738`

**Gap:** [`#3738`](https://github.com/openai/openai-agents-python/issues/3738) observes that exact item-count slicing can start with an orphaned tool result or assistant response. Discussion correctly identifies that silently expanding or shrinking `limit` would break the existing contract. fileciteturn164file0L1-L3 fileciteturn165file0L1-L1

**Your asset:** persistent memory and replay-validity gates.

**Best first PR:**

- no public API;
- a reusable three-turn fixture with multiple tool calls;
- cut at every item offset;
- record exact count, replay validity, and missing dependency;
- make it usable to compare later turn-aware API proposals;
- wait for maintainer approval before implementing `get_turns` or metadata changes.

**Estimated probability:** **30–45%**

The technical case is strong; the lower estimate reflects absent maintainer direction and public-API sensitivity.

---

## 10. Hugging Face Cookbook — fix the broken multi-agent recipe before adding benchmarks

**Gap:** [`huggingface/cookbook#316`](https://github.com/huggingface/cookbook/issues/316) reports that the multi-agent hierarchy notebook no longer runs because `ManagedAgent` disappeared from current `smolagents`. fileciteturn175file3L27-L46

**Your asset:** pipeline-versus-barrier orchestration.

**Best PR shape:**

- update only to the current supported `smolagents` pattern;
- execute fully in Colab;
- preserve the existing educational narrative;
- add a small timing comparison only if it naturally replaces the old orchestration, not as a new framework;
- show median wall time and call counts over a small repeated run;
- update TOC/index metadata if required.

**Estimated probability:** **35–50%**

A pure repair is more mergeable than a new “multi-agent consensus” cookbook, because the latter overlaps existing content.

---

# 3. Norms that kill PRs

## Google CLA

For Google repositories, the most avoidable blocker is identity mismatch:

- every commit author and co-author must be covered;
- the email in the commit must correspond to the identity used for the CLA;
- an individual CLA is usually recognized quickly; corporate CLA routing can take longer;
- after fixing commit metadata, retrigger the bot rather than merely commenting “signed.”

The CLA is generally signed once for Google OSS contributions, but each PR still has to pass the bot’s author check. citeturn270843search0turn270843search1

## Microsoft CLA

Microsoft’s CLA bot generally recognizes a one-time signature across Microsoft repos. The more important operational blockers are lack of issue assignment, surprise public APIs, incomplete pre-commit/test execution, and failure to involve the relevant area owner. citeturn610543search0turn610543search2

## Per-repo checklist

| Repo | Minimum green checklist | Common fatal mistakes |
|---|---|---|
| ADK Community | Issue alignment; CLA; correct Google header; native ADK base class/hooks; fail-closed behavior; unit tests; runnable sample; focused module | Copying in a standalone framework; sync/blocking callback; broad policy engine; third-party backend as mandatory core dependency |
| Gemini Cookbook | Issue approved before new content; CLA; official template; `nbfmt` and `nblint`; live outputs; current API; licensed data/media; direct Colab link to PR branch | Starting with the PR; stale model/API; unexplained cells; unlicensed assets; excessive dependencies; hard-coded outputs or credentials |
| GenAI Processors | `.py` implementation, `.md` docs, tests in `contrib/tests`; generic processor contract; cancellation and streaming tests | Provider-specific message formatting; silent unsupported-media behavior; complex framework-sized processor; misreading Copybara replacement as rejection |
| Microsoft Agent Framework | Issue-first for nontrivial work; get assignment/ack; failing test; focused patch; `uv run poe check`/pre-commit; linked issue; owner sign-off | New API without design agreement; mixing Python/.NET changes unnecessarily; broad refactor; changing compatibility contract incidentally |
| Semantic Kernel | Same Microsoft discipline, but prefer fixes/security/docs | New feature that belongs in Agent Framework; ignoring maintenance/successor status |
| OpenAI Agents Python | Exact reproducer; preserve lifecycle/session/history contracts; full verification sequence; focused tests; no secret/provider coupling | Speculative concurrency architecture; breaking session API; manufacturing missing history; changing tracing/persistence as a side effect |
| OpenAI Agents JS | Node/pnpm versions; build and build-check; `CI=1 pnpm test`; lint; Changeset when required; Conventional Commit; issue link | Missing Changeset; provider-specific workaround with no current repro; constructed-only test; comments claiming unsupported versions |
| Hugging Face Cookbook | Runnable notebook; neutral tone; free-access path; author attribution; TOC/index; clean outputs; explain results | Promotional content; unexplained paid service; duplicated topic; model chosen only because sponsor/vendor requested it |
| Groq Cookbook | Complete environment/setup; several useful scenarios; neutral writing; strong score across usefulness/originality/clarity/accuracy/depth/grammar | Thin integration, marketing, missing auth setup, or submitting while review activity is dormant |
| xAI Cookbook | End-to-end outputs; minimal deps; `%pip`; no keys; `registry.yaml`; issue-first if uncertain | Duplicating one of the many stale open PRs; multi-file app with no notebook; assuming fast historical staff merges imply external review |
| Mistral Cookbook | Colab-compatible; pinned dependencies; no warnings/errors; current SDK/model; README table; self-review | Mixing stale-content repair with a new framework; leaving old outputs; committed key; large external service dependency |
| AWS AgentCore Samples | Issue first; use `review ready`; correct current folder; latest main; one scope; architecture, prerequisites, usage, sample prompts, cleanup; deploy/invoke/cleanup; tests; add contributor | Wrong/legacy workshop folder; mixing workshop and main sample; broad reformat; no cleanup path; opening a PR for an already-claimed proposal |
| Perplexity Cookbook | MDX structure and frontmatter; tested snippets; real use case; limitations; env vars; full app in separate repo | Dropping a multi-file application into the cookbook; generic hello world; missing limitations/cost/source behavior |

---

# 4. Timing and optics

## What the histories actually support

| Example | PR shape | Observed behavior |
|---|---|---|
| OpenAI Agents Python [`#3898`](https://github.com/openai/openai-agents-python/pull/3898) | Two-file regression fix | Merged in about 74 minutes. fileciteturn63file0L1-L3 |
| ADK Community [`#141`](https://github.com/google/adk-python-community/pull/141) | Focused governance plugin after replacement | Roughly one day once it matched native architecture and tests. fileciteturn4file0L1-L1 |
| Microsoft Agent Framework [`#7124`](https://github.com/microsoft/agent-framework/pull/7124) | Issue-linked Python fix and regression test | Approximately days, not months. fileciteturn101file0L1-L1 |
| HF Cookbook [`#341`](https://github.com/huggingface/cookbook/pull/341) | New runnable notebook | Roughly ten days with content review. fileciteturn8file0L1-L1 |
| Gemini Cookbook [`#1280`](https://github.com/google-gemini/cookbook/pull/1280) | Small docs/notebook cleanup | Around two weeks. |
| Genkit [`#5475`](https://github.com/genkit-ai/genkit/pull/5475) | Large RFC-aligned public testing feature | Roughly 40 days and substantive review. fileciteturn119file0L1-L1 |
| Gemini Cookbook [`#790`](https://github.com/google-gemini/cookbook/pull/790) | New visual notebook | Approximately a year. fileciteturn40file0L1-L1 |
| Cohere DXP [`#140`](https://github.com/cohere-ai/cohere-developer-experience/pull/140) | Tiny external typo fix | Approximately 21 months. fileciteturn94file0L1-L1 |

## No defensible best weekday

I found no evidence strong enough to claim that Tuesday beats Thursday, or that a particular UTC submission time materially changes merge odds. The sample is confounded by release schedules, maintainer time zones, trusted contributors, and issue pre-coordination.

The evidence supports **format effects**, not weekday effects:

- **Fastest:** one bug, one reproducer, one regression test, no new abstraction.
- **Next:** stale-content/API migration with fresh outputs.
- **Slower:** one new notebook plus TOC/registry metadata.
- **Slowest:** new public API, framework, multi-service demo, or unsolicited architectural feature.

### Format by repository type

**SDK/framework repos:** issue link, failing test, smallest possible patch, compatibility analysis, full CI.

**Cookbooks:** one self-contained notebook or MDX article; live outputs; current API/model; explicit cost/limitations; no sprawling application.

**AWS AgentCore/NVIDIA-style sample repos:** multi-file demos are acceptable or expected, but must include architecture, deployment, invocation, cleanup, security boundaries, and costs.

**Community plugin repos:** one native module, one optional dependency group if necessary, unit tests, docs, and one sample.

---

# 5. Do not bother

1. **QwenLM/Qwen-Agent:** security and approval PRs already demonstrate that correctness alone does not produce review.

2. **xAI Cookbook:** the existing backlog already contains nearly every asset you would submit; no 90-day merge signal and no reliable public gatekeeper.

3. **Groq API Cookbook, for now:** excellent rubric, outdated liveness. Reconsider after one genuinely external 2026 merge or a maintainer explicitly accepts your issue.

4. **OpenAI Evals OSS:** use OpenAI Cookbook, Agents SDK, or the managed Evals API.

5. **Meta PurpleLlama, llama-models, and llama-cookbook:** no credible current external example funnel. Internal resyncs are not proof otherwise.

6. **Microsoft AutoGen for new features:** maintenance mode; target Agent Framework.

7. **DashScope Cookbook:** no recent merge history. Use AgentScope core.

8. **Cohere’s archived notebooks/toolkit:** the active DXP repo is only worth small docs work, with internal-sync caveats.

9. **Together Cookbook for generic orchestration/eval content:** its existing catalog already covers parallel, routing, orchestrator, deep-research, advisor, and judge patterns. A new contribution needs a concrete correctness or performance failure, not another pattern taxonomy.

10. **NVIDIA GenerativeAIExamples without prior approval:** thematic fit is not the same as external merge proof.

## Recommended submission order

The rational sequence is:

**MAF #7212 → GenAI Processors #164 → AgentScope #2123 → Genkit #4366/#4365 → Mistral #300 → ADK #142 collaboration.**

That ordering maximizes the combination of current review activity, externally proven merge behavior, unambiguous technical need, limited overlap, and direct reuse of your existing assets.
