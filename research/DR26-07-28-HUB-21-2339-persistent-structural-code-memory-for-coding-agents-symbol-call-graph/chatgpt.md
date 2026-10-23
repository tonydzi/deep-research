# DR26-07-28-HUB-21-2339 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Coding Agent Memory Research"
type: deep-research-original
source: chatgpt

dr_id: DR26-07-28-HUB-21-2339
conversation_id: 6a499669-8c18-83ed-b94a-09faec21e206
url: "https://приватный чат"
research_started_at: 2026-07-04T23:27:06.338122Z
research_stopped_at: 2026-07-04T23:37:23.039816Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-07-28T23:39:11
vault_chat_note: "2026-07-05-coding-agent-memory-research-6a499669.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
Conduct a deep research investigation on the following topic.



TOPIC: Best way to give a coding agent (Claude Code / Codex) a persistent, auto-updated STRUCTURAL memory of a codebase — symbol graph, call graph, change-impact ("blast radius") — to cut tokens and prevent breaking shared code, for a solo operator's repos (Python automation, a CRM app, agent skills, n8n workflows).



CONTEXT: Today the agent reads whole files + greps ("who calls X", "what breaks if I change shared script Y") — token-heavy, sometimes misses callers. Standing rule: "read the code before fixing safety-critical infra". Reference: codegraph (github.com/colbymchenry/codegraph, MIT) — tree-sitter 20+ langs → symbols/call-graph/imports → SQLite+FTS5+WAL; OS file-watcher (2s debounce); MCP tools codegraph_explore/search/callers/callees/impact/node/files/status; one-line install, auto-configures CC/Cursor/Codex/opencode/Gemini/Hermes; self-reported (unaudited) ~58% fewer tool calls, 23-64% fewer tokens. Orthogonal to concept-RAG (RAG=knowledge, codegraph=code structure).



OBJECTIVE: SOTA + trade-offs for code-structure memory / semantic code indexing for agents: adopt existing (codegraph, Sourcegraph/Cody, Serena, Aider repo-map, LSP/tree-sitter homemade) vs build minimal; token/accuracy economics; how impact-analysis prevents regressions; cost of a persistent watcher.



RESEARCH REQUIREMENTS:

1. Academic — code representation for LLM agents, repo-level context, graph-vs-embedding code retrieval, impact analysis.

2. Industry — Aider repo-map, Sourcegraph/Cody, Serena, Continue, Cursor index, codegraph, LSP/tree-sitter; measured savings.

3. Competitive — code-graph tools vs grep/RAG-over-code vs bigger context windows.

4. Failure analysis — stale index, wrong-symbol match, watcher/perf, multi-repo pain.

5. Emerging trends — last 12 months.

6. Implementation — adopt-vs-build + which repos first + where to host the watcher (heavy/persistent → desktop hub) + wiring impact-check into "read-before-fix".



OUTPUT: exec summary; adopt-vs-build matrix (tool × {token-saving evidence quality, setup, maintenance, repairability, license, Windows}); recommendation + rollout order + hosting placement; gotchas (stale index, false confidence); sources.@Deep research
```

## Отчёт
# Persistent Structural Memory for Coding Agents

## Executive summary

For a solo operator working across Python automation, a CRM app, agent skills, and some n8n workflows, the best current answer is **not** to build a full bespoke code-memory system first. The best answer is to adopt a **local, persistent structural layer** that gives the agent symbol- and caller-level access to the repo, then keep semantic search as a second layer. In practice, that means **CodeGraph first** for the repos where “who calls this?” and “what breaks if I change this shared thing?” matter most; **Serena second** if you want IDE-grade symbol refactors and stronger language-server semantics; **Cursor’s native index** or **Sourcegraph/Cody** as complementary semantic/context engines rather than substitutes for blast-radius analysis. Aider’s repo-map remains useful as a lightweight, cheap baseline, but it is not a persistent call-graph/impact-analysis memory. citeturn12view0turn20view0turn22search2turn10view6turn9view2

The underlying research points in the same direction. Repository-scale coding is a retrieval and navigation problem before it is a generation problem. Benchmarks such as RepoBench, ContextBench, SWE-ContextBench, and SWE-Explore show that repo-level tasks hinge on finding the right code regions efficiently, and that better context selection improves accuracy while reducing runtime and token cost; unfiltered or poorly selected context can actively hurt. Graph-based retrieval papers such as GraphCoder, RepoGraph, CodexGraph, SGKR, and Prometheus all argue, in different forms, that **structure beats flat similarity search** when the task depends on relations like caller/callee, dependency paths, inheritance, or multi-hop repository navigation. citeturn5search2turn30view2turn30view6turn30view3turn30view1turn29view1turn31view0turn30view4turn31view1

The main business case is not “AI knows more code.” It is narrower and more valuable: **the agent stops paying discovery tax every session** and stops making changes in local isolation. CodeGraph’s own disclosed benchmark, while self-reported and not independently audited, is directionally useful: across seven open-source repos it reports a median **58% fewer tool calls**, **22% faster completion**, and **23% to 64% fewer total tokens**, with file reads near zero when the graph is available. Serena’s official evidence is weaker and qualitative, but its docs and evaluation materials consistently show the same pattern: symbol-aware navigation and atomic refactors collapse fragile multi-step text operations into one or a few semantic calls. citeturn12view0turn21search0turn21search2

My recommendation is therefore straightforward. **Adopt, do not build, for the first phase.** Start with **CodeGraph** on the Python automation repos and the CRM app. Add a **small custom extractor for n8n workflow JSON** only after the imperative-code repos are paying off, because n8n’s workflow structure is node-and-connection JSON rather than ordinary function-call topology. Host the watcher/indexer on the **persistent local desktop or dev workstation**, not inside ephemeral agent sessions, not on a network share, and not under WSL-mounted paths where SQLite WAL can degrade. Then wire a mandatory **impact preflight** into your “read-before-fix” rule: before editing any shared/public/exported symbol, ask the structure layer for callers, callees, impacted files, and likely related tests; only then read the few live files needed for the exact edit. citeturn12view0turn13view0turn11view2turn33search0turn33search5

## What the research says about repo-level context

Repository-level coding is fundamentally different from single-file code generation. RepoBench was introduced specifically because single-file benchmarks miss the real problem: multi-file retrieval, completion, and end-to-end repository pipelines. That is the core reason whole-file read/grep loops become expensive and brittle in live agent workflows. citeturn5search2turn5search6

The recent literature is converging on a simple point: **context retrieval quality is now a first-order bottleneck**. ContextBench shows that even sophisticated agent scaffolding yields only marginal gains in retrieval quality, that LLMs tend to over-favor recall over precision, and that there is a real gap between what agents explore and what they actually use. SWE-ContextBench reaches a complementary conclusion: when prior context is accurately summarized and correctly retrieved, agents improve resolution accuracy and reduce runtime and token cost, but bad or unfiltered context can produce limited or negative returns. SWE-Explore pushes the argument further by isolating repository exploration itself and showing that file-level localization is no longer the main differentiation; **line-level coverage and efficient ranking** are. citeturn30view2turn30view6turn30view3

That is exactly where structural memory helps. GraphCoder shows that a **code context graph** with control-flow and dependence information outperforms sequence-based retrieval, improving exact-match metrics while using less time and space. RepoGraph argues that file-level or semantically similar retrieval often misses genuinely related code and reports strong gains on SWE-bench when its repository graph is plugged into existing frameworks. CodexGraph makes the critique explicit: similarity-based retrieval often has **low recall** in complex tasks, while graph databases enable more precise structure-aware retrieval and navigation. SGKR generalizes the same pattern outside standard coding benchmarks, showing that dependency-path retrieval improves correctness over similarity-only baselines for both vanilla LLMs and coding agents. citeturn30view0turn29view1turn31view0turn30view4

There is also a newer and more practical result that matters for your use case: the point of static structure is often not to make the model “smarter,” but to make it **less stochastic**. The recent deterministic anchoring study found that lightweight call/inheritance topology improved function-level localization, shortened trajectories, roughly halved run-to-run variance, and improved Pass@1 on medium-scale repos, at the cost of roughly 10% more input tokens. That trade-off is favorable when the alternative is an agent free-climbing through grep, reads, and guesswork around shared code. citeturn30view5

The literature on change impact analysis is older than the current coding-agent wave, but its lessons still transfer cleanly. Change impact analysis is classically defined as estimating the consequences of a change or what else must be modified to accomplish it. Static impact analysis typically uses call graphs and related representations and is conservative; dynamic approaches use execution traces and tend to be more precise for observed behavior but incomplete outside traced executions; and computing all effects exactly is not tractable in the general case, so all practical systems are approximations. In other words, blast-radius tools are **decision support**, not proof. That is still enough to markedly reduce regressions, because even an approximate caller graph is better than hoping grep found every relevant use site. citeturn6search20turn6search18turn6search4turn6search14

## What the current tools actually provide

The important distinction is between three different memory layers. **Structural memory** answers questions like “find references,” “show callers,” “what implementations sit behind this interface,” and “what is the impact radius of changing this symbol.” **Semantic memory** answers “where is the retry logic?” or “which part of the repo handles OAuth?” even when names do not match. **Long context** just stuffs more raw material into the model and hopes attention will do the rest. The strongest stacks increasingly combine all three, but structural memory is the layer that most directly attacks your specific failure mode: breaking shared code because the agent missed non-obvious dependents. citeturn10view4turn8search4turn18view2turn30view7

### Adopt-vs-build matrix

The setup, maintenance, and repairability columns below are my operational judgments based on the cited install model, architecture, source availability, and deployment style.

| Tool | What it gives the agent | Token-saving evidence quality | Setup | Maintenance | Repairability | License | Windows | Bottom line |
|---|---|---|---|---|---|---|---|---|
| **CodeGraph** | Local symbol graph, call edges, imports, impact radius, FTS5 search, OS file watcher, MCP integration; default primary tool is `codegraph_explore`, with narrower tools available but unlisted by default. citeturn12view0turn11view1turn11view3 | **Medium-low.** Best public numbers in this set, but still self-reported by the project: 7 repos, 4 runs/arm, disclosed methodology, 23–64% fewer tokens and 58% fewer tool calls. citeturn12view0 | **Easy.** One-line install, auto-configures Claude Code/Codex/Cursor/Gemini/etc., then `codegraph init` per repo. citeturn12view0turn11view4turn11view5 | **Low** if repos are local and ignores are sane; watcher debounces at 2s and does catch-up on reconnect. Watch for local-disk/WAL caveats. citeturn11view2turn9view0 | **High.** MIT, 100% local, SQLite-based, source available. citeturn13view0 | **MIT.** citeturn13view0 | **Yes.** Native PowerShell installer and ReadDirectoryChangesW watcher. citeturn11view4turn11view2 | Best fit for your immediate problem: persistent structural memory with low operational drag. |
| **Serena** | Symbol-level retrieval, references, implementations, safe symbol edits/refactors; LSP backend by default, optional JetBrains-backed semantic engine; built-in memory system for long-lived workflows. citeturn20view0turn15view1 | **Low.** Official evaluation is transparent but self-evaluative and mostly qualitative; useful evidence, not a benchmark you should treat as hard ROI. citeturn21search0turn21search2 | **Moderate.** `uv` install plus language-server dependencies and MCP client config. citeturn15view4turn21search7turn16search0 | **Medium.** Excellent when the language server is mature; more tuning for languages like C/C++ that require `compile_commands.json`. citeturn21search6turn20view0 | **High** for the OSS LSP path, **lower** for the paid JetBrains path. citeturn20view0turn15view1 | **MIT** for the OSS toolkit; JetBrains plugin is paid. citeturn15view1turn20view0 | **Mostly yes.** `uv` supports Windows; some language backends explicitly support Windows, but behavior depends on the underlying LSP. citeturn16search0turn21search5 | Strong second choice if you want safer symbol refactors, renames, moves, and richer IDE semantics. |
| **Aider repo-map** | A compressed tree-sitter map of files, symbols, and signatures; graph-ranking chooses the most relevant subset under a token budget, default 1k tokens. citeturn9view2turn25view0turn32view0 | **Low.** Strong mechanism, but no recent public head-to-head benchmark isolating repo-map savings in the way CodeGraph does. citeturn9view2turn25view0 | **Easy.** Simple installer; Windows one-liner exists. citeturn24view1turn23search4 | **Low-medium.** You will tune `--map-tokens`, refresh policy, and `.aiderignore`; still lighter than managing semantic servers. citeturn32view0 | **High.** Apache-2.0 and source available. citeturn14search0 | **Apache-2.0.** citeturn14search0 | **Yes.** Installer and docs cover Windows. citeturn24view1turn23search4 | Good cheap baseline, but not a persistent call graph and not true blast-radius memory. |
| **Cursor native index** | Background semantic index from syntactic chunks + embeddings; semantic search, instant grep, explore subagent; Merkle-tree sync and chunk caching. citeturn22search1turn9view3turn22search2 | **Medium** for indexing latency and codebase-Q&A quality, **low** for explicit structural-impact savings because that is not its public focus. citeturn22search2turn18view2 | **Easy** if you already use Cursor. citeturn26search3turn26search1 | **Low.** Indexing is async and reused across similar repos; ignores are supported. citeturn9view3turn22search0turn22search2 | **Low.** Commercial black box. citeturn26search5turn26search16 | **Commercial.** citeturn26search5turn26search16 | **Yes.** Desktop and CLI are available on Windows. citeturn26search0turn26search1 | Keep it as a semantic/concept layer, but it does not replace explicit blast-radius analysis. |
| **Sourcegraph/Cody** | Hybrid context engines, code search, precise code intelligence, cross-repo symbol IDs via SCIP, multi-repo context, enterprise-scale navigation. citeturn10view4turn10view6turn10view7turn8search6 | **Medium.** Solid public writing on retrieval and evaluation, plus long-context experiments; less direct token-savings data for solo local use. citeturn17search7turn18view2 | **Heavy** relative to local tools. Best when multi-repo/cross-repo pain is real. citeturn10view6turn8search9 | **Medium-high.** Powerful once running, but operationally larger than a local watcher. citeturn10view6turn27search16 | **Low.** Commercial product. citeturn27search16turn27search15 | **Commercial.** citeturn27search16turn27search15 | **Yes** via VS Code and JetBrains extensions. citeturn27search10turn27search1 | Best if you outgrow single-repo local memory and need cross-repo references as a first-class primitive. |
| **Continue** | Embeddings-based codebase awareness and custom code RAG path; local index historically mixed embeddings with keyword search, using LanceDB in current docs. citeturn10view3turn9view7turn10view2 | **Low.** Good docs, weak public proof for your exact blast-radius use case. citeturn10view3turn9view7 | **Moderate.** Flexible, but more “build-your-own retrieval stack” than turnkey structural memory. citeturn10view0turn9view7 | **High** relative to the value, because the original repo is now read-only and no longer actively maintained. citeturn9view6 | **Medium-high.** Apache-2.0 and source/docs remain usable. citeturn13view3 | **Apache-2.0.** citeturn13view3 | **Usable, but not the selling point.** Current docs emphasize VS Code, CLI, and JetBrains rather than OS-specific setup. citeturn9view6turn10view1 | Fine as a foundation if you intentionally want custom semantic RAG, not as your first structural-memory choice. |
| **Minimal custom build** | Exactly the memory you want: tree-sitter or SCIP/LSP extractors, SQLite/Postgres/graph store, watcher, MCP server, custom n8n extractor. Tree-sitter is incremental; LSP/SCIP gives semantic refs/nav. citeturn2search3turn10view7turn16search1 | **None** until you benchmark it yourself. Academic results say structure helps, but your implementation will determine the payoff. citeturn30view5turn30view4 | **Medium** for a narrow tree-sitter+SQLite version; **high** for a real multi-language semantic graph. citeturn2search3turn16search1turn10view7 | **Medium-high.** Language edge cases, stale-index handling, and graph quality are where the real work lives. citeturn6search18turn11view2 | **Highest** if you keep it small. | **Your choice.** | **Your choice.** | Worth it only for a narrow missing piece, especially n8n/workflow extractors, not as the first move. |

The short version of the matrix is blunt. **CodeGraph is the best immediate fit** if your pain is “persistent structural memory for local repos with low setup.” **Serena is the best complement** if you want semantically safe refactors and IDE-grade symbol operations. **Cursor and Sourcegraph are not wrong choices, but they solve a broader and slightly different problem**: semantic/code-search context, and in Sourcegraph’s case enterprise multi-repo intelligence, more than explicit local blast-radius memory. **Aider is useful but thinner.** **Continue is more blueprint than recommendation now.** citeturn12view0turn20view0turn22search2turn10view6turn9view2turn9view6

## Competitive position against grep, RAG-over-code, and bigger context windows

Plain grep plus whole-file reads is still a surprisingly strong baseline on easy tasks, which is why exaggerated token-savings marketing is dangerous. But the baseline fails exactly where your concern lies: **shared code, indirect callers, interface dispatch, and multi-file consequences**. CodeGraph’s README is unusually explicit about this distinction: the universal win is not magical model improvement; it is collapsing multi-step discovery into direct structural queries, with cost savings becoming material only as repos grow and the discovery tax compounds. That framing is credible, and it tracks what the literature says about context selection. citeturn12view0turn30view2turn30view6

Pure semantic RAG over code is excellent for “find me the thing that semantically resembles retry/auth/cache logic,” and modern products lean hard on it. Cursor chunks files syntactically, embeds them, updates the index asynchronously, and caches embeddings by chunk content; Continue’s codebase awareness similarly centers embeddings and vector search; Sourcegraph itself has moved from a simple embeddings story toward multiple retrieval engines, including search and code intelligence. This makes semantic RAG indispensable as a **concept layer**, but it is not the best answer to “who calls this symbol?” or “what implementations hang off this contract?” because those are graph queries, not nearest-neighbor queries. CodexGraph and SGKR say this explicitly in research form. citeturn9view3turn10view1turn10view3turn10view4turn31view0turn30view4

Bigger context windows help, but they do not remove the need for retrieval or structure. Sourcegraph’s 1M-token experiments with Gemini 1.5 Flash improved essential recall, concision, helpfulness, and reduced hallucination in technical Q&A. That matters. But the same company framed the question correctly: “will long context replace RAG?”—and recent research says the answer is still no. Lost in the Middle showed that long-context models do not robustly use information placed in the middle of long inputs. Prometheus then restated the same limitation in the code-agent setting: even with million-token windows, large codebases still produce a needle-in-a-haystack problem where relevant evidence is drowned by irrelevant code and docs. citeturn18view2turn18view1turn30view7turn31view1

The economically rational stack for you is therefore **layered retrieval**:

1. **Structural query first** for symbols, callers, implementations, imports, route paths, and blast radius.  
2. **Semantic search second** for intent- and concept-level retrieval when names are unknown.  
3. **Literal grep/search third** for strings, feature flags, configs, env vars, and constants.  
4. **Read live code last**, but only the few files or symbols that the first three steps identify.  

That ordering is not just elegant. It directly addresses token burn. Structural memory shrinks the number of exploratory turns; semantic search reduces misses when terminology drifts; grep remains cheap and exact for literals; and raw reads become precision follow-ups rather than the main navigation strategy. citeturn12view0turn22search1turn8search4turn30view3turn30view5

## Watcher cost, stale indexes, and the real failure modes

A persistent watcher is usually cheaper than people think and more dangerous than they think. The **compute cost** is often manageable because the best implementations avoid full re-indexes: Tree-sitter is an incremental parser; Cursor updates only changed branches of its Merkle tree and only re-embeds changed syntactic chunks; CodeGraph debounces bursts of edits and then syncs. In a solo setup, that means a persistent watcher on a few active repos is normally fine on a desktop-class machine. citeturn2search3turn9view3turn22search2turn11view2

The **operational failure modes** matter more.

The first is **staleness**. CodeGraph handles this better than most local tools because it explicitly surfaces pending files, warns the agent to read a file directly during the debounce window, and performs a connect-time reconciliation before first query. That is a real design advantage, because the most dangerous version of a code-memory system is one that is stale but silent. Still, staleness never goes to zero. If a file is currently changing, or if the indexer lacks semantic coverage for a language construct, the agent can acquire false confidence from a crisp-looking but incomplete graph. citeturn11view2turn12view0

The second is **wrong symbol resolution or partial semantics**. Static structure helps, but call graphs and references are approximations in many languages and toolchains. The software maintenance literature is clear that static analysis tends to be conservative and false-positive-prone, while exact change effects are not computable in the general case. Serena’s own docs acknowledge that some capabilities, such as implementations or declarations in external dependencies, depend on the language server and may be limited. If you build your own stack on tree-sitter alone, the risk rises: tree-sitter is excellent for syntax and incremental updates, but semantic resolution, inheritance, and external-library references are where LSP or SCIP become much more valuable. citeturn6search18turn6search14turn15view1turn10view7turn2search3

The third is **watcher/performance hygiene**. Indexers become miserable when they are fed generated code, vendored trees, or giant dependency folders. CodeGraph’s troubleshooting explicitly calls out excluding large directories such as `node_modules`; Cursor provides `.cursorignore` and `.cursorindexingignore` to control what gets indexed. This is boring, but it is where many “the watcher is too heavy” complaints actually come from. citeturn9view0turn22search0

The fourth is **multi-repo pain**. Local per-repo memory works very well until shared libraries, adjacent microservices, or infra repos start forming one logical system. Cursor’s own support team notes that product questions often span multiple repositories, which is why they use multi-root workspaces; Sourcegraph’s explicit advantage is cross-repository symbol IDs and references via SCIP, which is what local single-repo graph tools generally do not give you. For a solo operator, this usually means: start per repo, then promote only the genuinely shared libraries and infra repos into a cross-repo layer once the pain is frequent enough. citeturn22search5turn10view6turn10view7

The fifth is **domain mismatch**, especially for n8n. n8n workflows are node-based automation graphs saved as JSON, and n8n documents them as workflows made of nodes connected together that can be exported as JSON. That means ordinary code-graph tooling will help mostly around surrounding Python/JS helper code, custom nodes, or integration libraries—not around the workflow graph itself—unless you add a schema-aware extractor that turns `nodes` and `connections` into a workflow graph the agent can query. That is why I would not make n8n your first rollout target. citeturn33search5turn33search0

One more practical note for safety-critical infra: recent work on agentic workflows has shown that automation platforms such as GitHub workflows and n8n templates can be hijacked through crafted contextual inputs in some setups. That is not an argument against code memory. It is an argument for your standing rule to remain in force: **read the live code before fixing safety-critical infra**, and treat the memory layer as topology and triage, not as authority. citeturn33academia18

## Recommendation and rollout

My recommendation is to deploy this in **three layers**, with a strict rollout order.

### What to adopt

Adopt **CodeGraph as the default structural memory layer** for the repos where shared-code regressions are the expensive failure mode. Add **Serena** if you want a second semantic/symbolic lane for safer refactors, especially renames, moves, and implementation/reference navigation in richer IDE-backed environments. Keep your existing IDE/agent native indexing—especially Cursor if you already use it—as the **semantic concept layer**, not as the main blast-radius engine. Do **not** start by building a full LSP/tree-sitter/graph stack from scratch. citeturn12view0turn20view0turn22search2

### Which repos first

Use this rollout order:

1. **Python automation repos with shared scripts, cron jobs, infrastructure logic, or reused helpers.** This is the highest leverage because the failure mode is exactly “change shared script Y, miss callers, break something real.” Structural memory directly addresses that. citeturn12view0turn6search20  
2. **CRM app.** This is where the call graph, imports, route handlers, interface implementations, and test impact matter most after the automation layer. CodeGraph’s route and symbol graph support, or Serena’s symbol-aware tooling, are a fit here. citeturn12view0turn20view0  
3. **Agent skills repos.** Structural memory is still useful, but these often have more documentation, prompt, and script-value than deep call-graph value, so your semantic layer pulls more weight here. citeturn22search7turn10view0  
4. **n8n workflows.** Do this last, and only after you decide whether you want a custom `workflow-node -> downstream-node` extractor. The raw JSON is available, which makes custom graph extraction feasible, but ordinary code-memory tools will not give you the same quality here out of the box. citeturn33search0turn33search5

### Where to host the watcher

Host the persistent watcher/indexer on your **always-on desktop or primary dev workstation**—the machine that already has the repos locally and that your coding agents use most often. The reasons are architectural, not ideological: CodeGraph stores a local `.codegraph` database per project, uses SQLite WAL, and depends on OS-level file events; Cursor similarly performs background indexing and sync. This kind of state wants a persistent filesystem, low-latency local disk, and a process that outlives any individual agent session. Avoid network shares, and avoid WSL-mounted paths where WAL may not stay in the best mode. citeturn12view0turn9view0turn22search2

### What not to build yet

Do **not** build your own multi-language semantic graph first. If you build anything in the first 60 days, build only a **narrow add-on** where the existing tools are obviously weak. The best example is n8n: a tiny extractor that parses workflow JSON and emits a simple graph of workflow IDs, node IDs, node types, expressions, and entry/exit connections. Tree-sitter, LSP, and SCIP are excellent building blocks, but a real self-maintained semantic platform becomes a product, not a utility, very quickly. citeturn2search3turn10view7turn16search1turn33search0turn33search5

## Implementation blueprint for your read-before-fix rule

The key process change is to make the structural layer mandatory **before editing shared or externally consumed code**.

A good policy for Claude Code or Codex is:

```text
If the requested change touches any of the following:
- shared utility modules
- public/exported functions or classes
- integration adapters
- workflow entry points
- auth, billing, scheduling, messaging, or infrastructure code

then before editing:
1. query the structure layer for the target symbol/file;
2. fetch callers, callees, implementations, imports, and impact radius;
3. read the live source only for the impacted symbols/files that the graph surfaces;
4. name the likely blast radius in the plan;
5. run targeted tests for the impacted area after the edit.
```

That policy is supported by the tool capabilities themselves. CodeGraph can already return callers, callees, call paths, impacted symbols, files, and line-numbered source in a single exploration flow, with staleness warnings when necessary. Serena provides find-symbol, referencing-symbols, implementations, hierarchy, and safer symbol edits; Sourcegraph/SCIP brings compiler-like reference navigation across repositories when you need that later. citeturn11view3turn12view0turn15view1turn10view7

For your setup, I would implement the rollout like this:

1. **Install CodeGraph and initialize the first two repos**: the most failure-sensitive Python automation repo and the CRM repo. Keep `.codegraph` local to each repo. Validate that the agent actually uses the graph instead of falling back to file-reading subagents. citeturn12view0  
2. **Add explicit ignore rules** for vendored/generated directories before you judge watcher cost. If the repo includes huge build artifacts or synced data, exclude them early. citeturn9view0turn22search0  
3. **Add a hard rule in your agent instructions**: “For shared/public symbols, run impact analysis before editing.” In CodeGraph terms, that means using `codegraph_explore` first; in Serena terms, use symbol/reference tooling first. citeturn11view3turn20view0  
4. **Measure one month of real tasks**, not toy benchmarks. Track: tool calls, file reads, total tokens, missed callers found post hoc, and regression rate on shared modules. CodeGraph’s benchmark methodology is useful as a template, but your repos are the only numbers that matter operationally. citeturn12view0  
5. **Only then decide on Serena.** If your month-one pain is mostly navigation plus light edits, CodeGraph alone may be enough. If the pain is “rename/move/implementation/refactor across files without text surgery,” add Serena on the same repos. citeturn20view0turn21search0  
6. **Build the n8n extractor last.** Export workflows as JSON and generate a tiny sidecar graph that includes node IDs, node names, node types, expressions, credentials references, and outgoing connections. That gives you a structural memory for workflows that ordinary code-memory tools do not model directly. citeturn33search0turn33search5

The biggest gotcha is false confidence. A crisp graph answer feels authoritative even when it is partial. Static structure is an approximation; semantic search is an approximation; long context is lossy; and all of them fail silently sometimes. The practical antidote is simple and worth keeping as doctrine: **use structural memory to localize and scope the change, then read the live code for the exact touched area before editing, and verify with targeted tests afterward.** That rule is fully consistent with both the software-maintenance literature and the current coding-agent research. citeturn6search18turn6search14turn30view2turn30view3turn30view5
