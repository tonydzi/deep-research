# DR26-10-24-HUB-07-0721 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll verify the current public evidence for a semantic-search companion and a navigation baseline, starting from the official project identities and post-September 2026 changes.A. Executive Summary

Keep CodeIndexer. Do not add a second always-on search service, and do not treat the 11 September 2026 four-leg note as four verified sources. Two bounded, reversible canaries are justified, each in a different category.

Semantic canary: `cocoindex-io/cocoindex-code` **0.2.42** (released 5 October 2026, Apache-2.0), extra `[full]`, CLI only, telemetry forced off, embedding model pinned to the incumbent `intfloat/multilingual-e5-small` with the model card's `query:` / `passage:` prefixes. The thing being tested is AST-bounded chunking plus an explicit incremental index, not a new embedding model and not the English default `Snowflake/snowflake-arctic-embed-xs`. Confidence: **[emerging]**. Assumption: CodeIndexer does not already chunk on AST boundaries with that same model and a provable delete/rename path. If it does, stop.

Navigation canary: `oraios/serena` package `serena-agent` **1.7.0** (9 August 2026), the last MIT release, not current **2.1.0** (19 October 2026, GPL-3.0-or-later). One local HTTP process, dashboard off, usage reporting off, Python read-only symbol/reference tools only, no rename, no shell, no JetBrains plugin. Score it against the existing Pyright run and against Tree-sitter/Python AST. A tie with Pyright means park Serena. Confidence: **[emerging]**.

Park `zilliztech/claude-context`, CocoIndex Code Plus, a second Code Indexer install, SCIP as the first baseline, Semgrep, and Vulture. No performance number below is a measurement from this fleet.

B. Key Findings

Categories stay separate. (a) Embedding retrieval answers natural-language and paraphrase queries and can return the wrong symbol of the same name. (b) LSP/SCIP answers definition, references, and rename only for names the analyzer bound. (c) Tree-sitter, Python AST, and `ccc grep` answer structure and spelling, including comments and strings. (d) Vulture and Semgrep answer unused code and rule matches. Serena's README phrase "semantic retrieval" is (b), not (a). `ccc grep` is (c) and must not be scored as (a). Vulture 2.16 and Semgrep 1.182.1 are (d).

| Category | Candidate | Identity and pin | What it actually is | Verdict |
|---|---|---|---|---|
| (a) semantic | cocoindex-code | `cocoindex-io/cocoindex-code` 0.2.42, commit `6bb17d0`, 5 Oct 2026, Apache-2.0. Engine dependency `cocoindex[sentence-transformers]>=1.0.17,<1.1.0`. Framework release beside it is cocoindex v1.0.25, 6 Oct 2026, also Apache-2.0 | Packaged CLI `ccc` plus a background daemon. Tree-sitter chunks, then embeds. Not the CocoIndex framework and not CocoIndex Code Plus | CANARY |
| (a) semantic | claude-context | `zilliztech/claude-context`, MIT. MCP quickstart is `npx @zilliz/claude-context-mcp@latest` with `<key-var>` and a Milvus/Zilliz address | Hybrid dense plus sparse search, AST split, Merkle incremental sync. Default path is a cloud embedding API and a cloud or separate Milvus | PARK |
| (a) semantic | public "Code Indexer" | `https://codeindexer.dev` and `Nettechnologys/codeindexer`. Proprietary signed binaries. Source is not in that repo | Marketing describes local semantic plus symbolic search and Windows DirectML. Exact model string `intfloat/multilingual-e5-small` was not on the pages opened | Do not install a second copy. Identity with the incumbent is UNKNOWN |
| (b) navigation | Serena | `oraios/serena`. Canary pin `serena-agent==1.7.0` (9 Aug 2026, MIT). Current tag v2.1.0 (19 Oct 2026). Application GPL-3.0-or-later from v2.0.0 (13 Oct 2026). `src/solidlsp` stays MIT | LSP client and MCP/CLI. Default backend is language servers. JetBrains plugin is a paid alternative. Python backend on current main is Pyright, pin `1.1.403` in current source; that pin was not verified inside the 1.7.0 tag | CANARY at 1.7.0 only |
| (b) navigation | SCIP | Protocol repo `scip-code/scip`, Apache-2.0. `sourcegraph/scip-python` is a Pyright fork, MIT text in `LICENSE.txt`, still updated 23 Oct 2026, open defects in 2026 | A snapshot of compiler-accurate occurrences. Useful later as a hashable index. First Python slice duplicates Pyright | PARK as the first baseline |
| (c) structural | already in use | Tree-sitter and Python AST, 24 Oct 2026 local canary | Lexical locations matched; one ambiguous name returned unrelated definitions | KEEP as the control. Not a semantic result |
| (c) structural | `ccc grep` | Same package as cocoindex-code. No daemon and no embeddings | By-example syntax match | Control only, if the semantic canary is installed |
| (d) static | Vulture 2.16 | `jendrikseipp/vulture`, 25 Mar 2026, MIT | Python AST unused-code finder. Author: dynamic calls are missed | PARK |
| (d) static | Semgrep 1.182.1 | `semgrep/semgrep`, PyPI 22 Oct 2026, LGPL-2.1. Wheels for Linux, musl, and Windows amd64 | Pattern/taint analysis. Not retrieval | PARK |

What changed since 11 September 2026. **[established]** where two primary pages agree.

cocoindex-code moved from 0.2.41 (7 Aug 2026, NULL KNN distances rejected) to 0.2.42 (5 Oct 2026: Elixir, Oh My Pi hooks, dependency bumps). 0.2.38 was yanked on 24 Jul 2026 for issue 237. The README now distinguishes slim and `[full]`. Slim is LiteLLM and needs a cloud key. `[full]` pulls SentenceTransformers. That is source, not only marketing: `pyproject.toml` at commit `6bb17d0` sets `full = ["cocoindex[sentence-transformers]>=1.0.17,<1.1.0"]` and the base dependency is `cocoindex[litellm]`.

Serena's license changed after the September research. Discussion #1986 (3 Sep 2026) announced GPL for the application. v2.0.0 shipped 13 Oct 2026. v2.1.0 shipped 19 Oct 2026. Last MIT release is v1.7.0. The change is not retroactive. v2.1.0 also fixes a real trust bug: `typescript-language-server` could load `node_modules/typescript/lib/tsserver.js` from an untrusted repo (changelog, issue #2129).

Open cocoindex-code issues that still showed as open in the GitHub issue listing this run: #276 (19 Aug 2026) watchdog auto-refresh not shipped; #270 (6 Aug 2026) `ccc search --path` can crash the daemon; #288 (18 Sep 2026) killing a stale daemon deletes the live socket; #309 (13 Oct 2026) asks for freshness tools that are not there yet.

Platform, footprint, network. **[established]** from README, `EMBEDDINGS.md`, and `client.py` unless marked.

cocoindex-code runs as a native Python 3.11+ tool on Windows, macOS, and Linux. `client.py` has an explicit `win32` stop path, so Windows is not "Docker or nothing." Docker is also documented, including Windows Docker Desktop, and the README says LMDB on mounted volumes is a poor fit. Device values documented are `cpu`, `cuda`, and `mps`. DirectML is absent. On this Windows hub the honest device is CPU unless CUDA is present. `[full]` is described as about 1 GB of torch plus transformers; the `:full` image is described as about 5 GB. Both figures are vendor size claims **[single-source]**. The daemon starts on first use, holds the model in RAM, and idle-exits after 180 minutes by default. That is one shared local process, not one stdio server per agent, but only if MCP and editor hooks are not installed.

Default local model in the interactive init is `Snowflake/snowflake-arctic-embed-xs` (22M, 384-d). The model card (26 Jul 2024) describes it as an English MiniLM-family retrieval model. Snowflake's 4 Dec 2024 post says Arctic Embed 1 suffers on multilingual retrieval and that v2, not xs, is the multilingual line. cocoindex's own `EMBEDDINGS.md` (scores "as of 15 Jun 2026", vendor table **[single-source]**) lists `ibm-granite/granite-embedding-97m-multilingual-r2` as the multilingual option and gives arctic-embed-xs a code score of 0.67 against 0.80 for Granite. Those scores are not an independent benchmark. The incumbent model card (`intfloat/multilingual-e5-small`, page content dated from 2024; accessed 24 Oct 2026) says every input should start with `query:` or `passage:`, including non-English text, or quality drops.

Telemetry for cocoindex-code is on unless `COCOINDEX_DISABLE_USAGE_TRACKING=1`. The README says events are anonymous, application name `cocoindex-code`, and that source, paths, queries, results, embeddings, and settings are not collected. That exclusion is a vendor claim **[single-source]** until the canary watches the first-run network.

Serena installs with `uv` on Windows, macOS, and Linux (`uv tool install -p 3.13 serena-agent` in the current README). Some language servers are not native everywhere: current docs say Roslyn auto-download covers Windows x64/ARM64, macOS, and Linux, and Windows C# wants PowerShell 7. Older third-party README mirrors said Elixir/NextLS had no Windows binaries; that mirror is stale relative to current docs, so Elixir-on-Windows is UNKNOWN. Serena is not vector search. Its README evaluation quotes are agent self-reports from a vendor prompt. The methodology page was not opened in full, so those quotes are not accuracy evidence **[low-authority]**.

Serena does phone home unless disabled. Changelog v1.1.0 (11 Apr 2026) added startup usage collection: Serena version, language backend, OS, dashboard enabled, "no personally identifiable information." v1.1.1 turned on TLS verification for `oraios-software.de`. v1.1.2 adds `SERENA_USAGE_REPORTING=false`. A later section still in the changelog, immediately above the v1.1.2 heading, says reporting also sends the context name. On 15 May 2026, maintainer MischaPanch wrote on issue #1491: "We don't have any telemetry btw." Both statements stand. The changelog entries were still present on main on 24 Oct 2026. No removal note was found. Dashboard construction also queries GitHub news and PyPI (comment on PR #1526, 29 May 2026). Windows WebView2 dashboard processes have leaked gigabytes across orphaned sessions (issues #1387, 21 Apr 2026, and #1389, still reproduced 17 Sep 2026 on `1.7.1.dev0`).

Freshness. **[established]** for the mechanism that is written down; **[speculative]** for branch and symlink behavior.

`ccc search` does not refresh by default. `ccc search --refresh` and the MCP tool's `refresh_index=True` do. CLI and MCP therefore disagree. There is no shipped watchdog (#276 still open). The v0.2.42 protocol struct `IndexingProgress` has `num_adds`, `num_deletes`, `num_reprocesses`, `num_unchanged`, and `num_errors`. It has no file hash field. `ccc status` reports chunk counts, file counts, and languages, not source hashes. A consumer cannot prove indexed bytes from the documented status output. Search results do include chunk text and line ranges, so a unique string can be used as a behavioral check. That is weaker than a hash manifest.

Race that remains: a file can change after the refresh snapshot and before the query. `IndexWaitingNotice` only means another index is in progress. No snapshot isolation is documented. Missing index, partial index, and `num_errors > 0` are not defined as "search must fail closed" in the pages opened. `ccc doctor` checks settings, daemon, model, file matching, and index health, which is a diagnostic, not an acceptance proof. Deleting the index is `ccc reset`. Renames are not given a separate operation. Inferred behavior, **[speculative]** until the canary: a rename is a delete of the old path plus an add of the new path only after an explicit refresh. A branch switch without refresh would keep the previous tree's chunks. Symlinks and generated copies are UNKNOWN beyond include/exclude, `max_file_size`, and gitignore.

Serena/Pyright do not keep an embedding index. They re-analyze through the language server. Serena changelog entries show real stale-cache bugs in other languages (Lean 4 empty symbols persisted, #1356, in the post-1.1.2 section). Python stale behavior on this fleet is UNKNOWN. LSP document state can lag a save. A green process exit is not "the bytes you just wrote were analyzed."

What each layer can prove. **[established]** as a distinction; the fleet's dynamic case stays UNKNOWN.

Lexical callers (ripgrep, Tree-sitter, Python AST) prove a spelling or a syntactic call shape. The 24 Oct canary already showed an ambiguous name pulling unrelated definitions. They do not prove which binding was used.

Pyright, Serena-on-Pyright, and scip-python can prove definitions and references the type checker bound: imports, aliases it understands, methods, and inheritance it can see. Sourcegraph's precise-navigation doc says SCIP is compile-time information and search-based navigation is the fallback when no index exists. The same doc says precise mode avoids same-name false positives when the index is present. It does not claim `getattr`, string-built names, or decorators that rebind at runtime are resolved. scip-python's own 2026 issues contradict "precise means complete": #221 (27 Jul 2026) cross-package references silently dropped; #223 (29 Jul 2026) missing symbol information; #228 (1 Oct 2026) indexing abort on a recursion error. The 21 Oct local Pyright canary already left one dynamic case unresolved and did not show incremental value on four cases. That does not mean LSP is useless. It means Serena must beat that Pyright result, not replace the memory of it.

Unicode identifiers and multilingual comments: no candidate has a published result on this corpus. CoIR (arXiv:2407.02883, revised 6 Jun 2025, ACL 2025) shows code retrieval is hard even for strong embedding models, on public datasets, not on this fleet. Do not import CoIR scores as local recall.

C. Contradictions and Debates

Embeddings versus grep. Cursor's own undated research post `https://cursor.com/blog/semsearch` says semantic search plus grep raised answer accuracy about 12.5 percent on their internal bench (range 6.5 to 23.5 by model) and that grep alone is worse on large repos. A 19 Oct 2026 secondary article claims Cursor later removed server-side embeddings in favor of local "Instant Grep" (`https://www.toolbit.ai/blog/cursor-vs-windsurf-which-agentic-ide-solves-m`). That removal was not confirmed on a Cursor primary page in this run **[low-authority]**. George He's 7 Oct 2026 talk argues grep works for code because code is local, textual, and full of names, and that embeddings matter more for messy documents. The 19 Oct 2026 johal.in measurement on CPython compared grep with a symbol index, not with embeddings, and says the winner depends on name frequency. Steelman for grep: exact names, imports, and refs should stay on (b) and (c). Steelman for embeddings: the query that does not know the identifier. Both can be true. This canary tests only the second.

"Semantic" in product names. cocoindex-code's title says "AST-based semantic" while the pipeline is chunk-then-embed. Serena says "semantic retrieval" for LSP symbols. Comparing those marketing sentences as if they were one feature is the mistake the September note is at risk of repeating.

Serena telemetry. Changelog says a startup ping exists and can be disabled. A maintainer told a user there is no telemetry. Treat the changelog as the engineering evidence and disable the flag anyway. Do not treat the comment as proof the ping is gone.

Vendor token-saving claims. cocoindex.io says roughly 70 percent fewer tokens and sub-second re-embed of changed files (product page, reviewed text dated 16 Jul 2026 in search metadata). Claude-context writeups repeat about 40 percent token savings. None of these are an independent paired benchmark on this fleet **[low-authority]** when they come from the vendor or a recap of the vendor.

Default model versus stated multilingual need. cocoindex recommends arctic-embed-xs as the compatible default and separately admits a multilingual model for mixed-language repos. Shipping the default would confound "worse chunking" with "English-only vectors" against CodeIndexer's multilingual-e5-small. The canary forbids that confound.

GPL versus "just use latest Serena." v2.1.0 is the maintained line and contains the TypeScript trust fix. v1.7.0 is the last MIT build and is missing that fix. Internal use of GPL is not the same as shipping a combined work. This report is not legal advice. For a read-only Python canary on a trusted tree, the trust fix is out of scope, and the GPL change is a new obligation the September research did not have. That is why the pin is 1.7.0. If the canary later includes untrusted JavaScript, 1.7.0 is the wrong pin and the run should stop rather than silently upgrade.

Evidence against the recommended semantic option. It adds a daemon and a torch stack the hub's ORT DirectML path does not share. CPU embeddings will not be a latency win, and latency is not the acceptance test. The default model is the wrong language coverage. Telemetry is on by default. Status output does not prove bytes. Path search has an open crash. Auto-refresh is an open feature request. A second embedding index can create false confidence if its corpus is a different exclude set than CodeIndexer's. If those gaps are not closed in the canary log, park it.

Evidence against the recommended navigation option. The Python engine is Pyright, which was already tried on four cases. Serena adds process, dashboard, and editing surface around that engine. Windows dashboard leaks are documented. Usage reporting exists. v1.7.0 is two months behind v2.1.0. Multi-language value is unmeasured here. If the Python slice ties Pyright, keeping Serena would be a new daemon with no new facts.

D. Best Practices

Use one tool per question. Natural language goes to CodeIndexer and, only inside the canary, to `ccc search`. Definitions and references go to Pyright or Serena's symbol tools. Call shape goes to Tree-sitter or Python AST. Dead code and security rules stay off this decision.

Pin versions and models in the command line. Record `ccc version`, `serena --version`, the Pyright version the language server actually started, and the embedding model string from `ccc doctor`. Do not accept "latest."

Force the same bytes. Before any query, hash the frozen tree (path, size, sha256), including a list of excluded, generated, and symlink paths. Run the same manifest through CodeIndexer, Tree-sitter, Pyright, and the candidate. A tool that indexes a different set is a different experiment.

Refresh explicitly, then prove it. For cocoindex-code, search once without `--refresh` and once with `--refresh` after a scripted edit, and require the stale result and the fresh result to differ in the expected direction. Count `num_deletes` from the index progress. Then delete the unique string's file and require that string to be absent after refresh.

Disable network. `COCOINDEX_DISABLE_USAGE_TRACKING=1` before the first `ccc` process. `SERENA_USAGE_REPORTING=false`, dashboard off, no JetBrains. No LiteLLM, no OpenAI, no Zilliz, no Semgrep registry fetch.

Do not put index files on a synced disk. cocoindex-code already gitignores `.cocoindex_code/`. Keep that directory machine-local. The README's suggestion to point `COCOINDEX_CODE_DIR` at a synced folder is for settings, not a license to sync LMDB across Windows, macOS, and Linux.

Prefer the CLI the fleet can start and stop. `ccc` is that CLI. `serena start-mcp-server --transport=streamable-http` on 127.0.0.1 was demonstrated for 1.7.0 in issue #1889. Do not add stdio MCP entries to every agent session. Stop `ccc daemon stop` and the Serena process at the end, and confirm no child remains.

Randomize query order. Separate cold (process just started, model just loaded) from warm (second identical query). Isolate side effects: the semantic canary may write `.cocoindex_code/` inside the fixture only; the navigation canary must not write source. Neither may edit the other tool's index.

Do not use one candidate as the ground truth of the other. Expected answers are written from the frozen source by a human before either tool runs.

E. Failure Modes

False confidence from a green exit. `ccc index` can report `num_errors` and still return success shapes that a caller might ignore. Serena's MCP `serverInfo.version` on 1.7.0 reported the MCP SDK version (`1.28.1`) rather than Serena `1.7.0` (issue #1889, 18 Aug 2026). Log the tool's own `serena --version`.

Stale or partial index. CLI search without `--refresh` will answer from old chunks. A crash during index (#270, #288) can leave a socket or a half-written LMDB. `ccc doctor` failing is a stop, not a retry loop.

Wrong corpus. `.gitignore` is honored inside an initialized project for `ccc grep`. Generated copies and tests will be retrieved if they are not excluded. The 24 Oct ambiguous-name failure is the same class of bug: extra definitions in the corpus.

Wrong layer. A high embedding score on a comment, a test double, or a deprecated function is a false positive for "where is this implemented." A lexical caller list that includes a string in a docstring is a false positive for "who calls this."

Dynamic code scored as a miss. `getattr`, alias rebinding, and decorator wrapping should be labeled UNKNOWN, not a failed reference. Forcing a tool to pick one target manufactures a false positive.

Model confound. arctic-embed-xs on Russian or Chinese comments against multilingual-e5-small measures language coverage, not the index. Missing `query:` / `passage:` prefixes measures prefix misuse, not chunking.

Privacy. Slim cocoindex-code and claude-context's quickstart send source to a third party. Serena's startup ping and dashboard version check are smaller but still off-box. An editor hook that runs `ccc index` on every write can index a home directory; the README warns that `~/.cocoindex_code/settings.yml` as a project would cover every repo under home, and tells you to `ccc reset --all` there.

Windows resource leak. Default Serena dashboard plus one process per agent session has already exhausted RAM on Windows. That configuration is a failed canary even if the symbols are right.

License drift. Installing `serena-agent` without a pin now resolves to GPL v2.1.x. A later "upgrade the canary" changes the license.

Deleted and renamed files. If refresh is skipped, search still returns the old path. If two branches share one index directory, results mix trees. Syncing that directory between operating systems copies a native LMDB the README already says not to put on a mount.

F. Strategic Recommendations

KEEP. CodeIndexer, with `intfloat/multilingual-e5-small` and the Windows ORT DirectML path. Tree-sitter and Python AST as the structural control. The existing Pyright install as the Python reference control. Do not remove any of them to make room.

CANARY, semantic. `cocoindex-code==0.2.42` with the `[full]` extra, `cocoindex==1.0.25` if the solver allows that exact pin inside `>=1.0.17,<1.1.0`, model `intfloat/multilingual-e5-small`, prefixes set, `COCOINDEX_DISABLE_USAGE_TRACKING=1`, device `cpu` unless CUDA is already on that machine, no DirectML claim, CLI only, no MCP, no hooks, no Code Plus, no Docker unless native `ccc doctor` fails. One fixture directory. `ccc daemon stop` and `ccc reset -f` is the rollback.

CANARY, navigation. `serena-agent==1.7.0`, MIT. `SERENA_USAGE_REPORTING=false`. Dashboard off. Language-server backend, not JetBrains. One loopback HTTP server. Tools limited to symbol overview, find symbol, and find referencing symbols. No rename, no shell, no memory writes, no REPL. Python only, same fixture as the 21 Oct Pyright cases plus the new frozen items. Rollback is uninstall the tool and delete `~\.serena` project data for that fixture.

PARK. CocoIndex the framework, except as the library inside the pinned package. The `examples/code_embedding` Postgres pipeline is an example, not a product. CocoIndex Code Plus (`cocoindex-code-plus` 0.1.69 on 21 Oct 2026) is a separate client and is out. claude-context, because the documented quickstart exports source. The proprietary codeindexer.dev binary, because it may be the incumbent and its source cannot be checked. scip-python, until Serena-versus-Pyright is decided; if Serena ties, SCIP is still the same type checker plus a snapshot format, and its 2026 issues are open. Semgrep 1.182.1 and Vulture 2.16, different questions; Semgrep's 7 Oct 2026 notes show a telemetry endpoint can affect scans. ast-grep, prior art for (c), not needed while Tree-sitter is already in the loop.

Do not adopt either canary onto the 21 registered projects. Registration was never proof of a fresh index. One frozen fixture first.

G. Action Plan and reproducible benchmark contract

This contract is a plan. Every recall, latency, and memory cell starts empty. Filling one with a guessed number is a failed run.

Fixture. One small git tree copied out of the real projects, then frozen. Commit recorded. Working tree dirty flag must stay false except during the scripted edit steps. Include Python plus, only if the Python navigation slice ties and a follow-up is authorized, one non-Python language. Include at least one non-English comment, one Unicode identifier, one generated or vendored copy that must be excluded, one symlink, and one file the ignore rules should drop. Independent expected answers are written from the source before tools run. At least: 8 natural-language items whose target is a function the query does not name; 8 exact-definition items; 8 caller/reference items split into lexical-only, import-alias, method, and inheritance; 2 rename and 2 delete freshness items; 4 dynamic items (`getattr`, alias rebind, decorator wrap, string-built name) whose correct label is UNKNOWN for (b) unless the tool shows the binding. No item is graded by another tool's output.

Order. Shuffle items with a recorded seed. For each tool, run cold once (fresh process) and warm once (same process, same query). Tools do not run in the same process. Side effects stay inside the fixture copy.

Semantic arm. CodeIndexer as it is configured today, then `ccc` with the pins in section F. Same exclude list. Record indexed file count against the manifest. If `ccc doctor` cannot load multilingual-e5-small with explicit passage prefixes on index and query prefixes on search, stop. Do not fall back to arctic-embed-xs. Query with `ccc search` and with `ccc search --refresh` as two modes. Do not call `ccc grep` in this arm.

Navigation arm. Tree-sitter and Python AST lexical callers; Pyright; Serena 1.7.0 read-only. Same questions. Serena's regex tool stays disabled so it cannot imitate the lexical arm.

Freshness arm. After the main scores: edit a unique string, rename a file, delete a file, switch branch, and touch an ignored path. Query before refresh and after explicit refresh. Record whether the old path's chunk text is still returned. Repeat once while a writer appends during the query, and mark that trial race, not accuracy.

Metrics, computed by the harness, not by the tools. recall@5, recall@10, MRR, false-positive rate, unknown rate, corpus coverage (manifest files the tool actually indexed, or UNKNOWN if it cannot say), source freshness (sha256 of the tree versus sha256 at query return), cold and warm latency, index and update wall time, peak RSS, and bytes of returned context. Dynamic items contribute only to the unknown rate. A confident hit on a dynamic item is a false positive.

Acceptance. Semantic: adopt only if natural-language recall@5 is strictly higher than CodeIndexer on the pre-registered NL set, the ambiguous-name false positive does not get worse, and both the delete and the rename are invisible after `--refresh` and visible before it. A tie parks cocoindex-code. Navigation: adopt Serena only if it reports at least one resolved reference that the lexical arm misses and Pyright also misses, or a multi-language follow-up was separately authorized and it resolves a non-Python reference the lexical arm misses, and it labels every dynamic item unknown or refuses it. A Python-only tie with Pyright parks Serena. Either arm fails if source left the machine, if a stdio server was added to agent config, if the index directory was placed on a synced disk, or if the process was not gone after rollback.

Maintenance if a canary later passes. cocoindex-code: pin the package, pin the model, keep telemetry off, run `ccc` on demand per machine, do not sync `.cocoindex_code`. Expect torch CPU cost on the Windows hub. Serena: stay on 1.7.0 until a human accepts GPL for v2, keep one HTTP process, dashboard off. Re-run this contract after any version bump. Rollback is uninstall plus delete the fixture's index directory. Neither tool replaces CodeIndexer.

Prior art for this exact contract: not found. Searches performed: "CoIR code information retrieval benchmark", "Cursor semantic search blog", "grep versus embeddings code agent 2026", "johal code index versus grep token cost", "SCIP precise code navigation", "cocoindex-code freshness watchdog". CoIR is a public retrieval bench (2024, revised 2025), not a local paired freshness bench. Cursor's post is an internal vendor bench without a public query set. johal.in (19 Oct 2026) measured symbol index versus grep and explicitly excluded embeddings.

H. Sources and proof of work

Access date for every page: 24 October 2026. Queries run included: cocoindex-code releases and license; oraios serena releases and license; local code semantic search MCP; Sourcegraph SCIP versus Zoekt; CodeIndexer multilingual-e5-small DirectML; claude-context Zilliz; Cursor grep versus semantic search; Serena changelog telemetry Windows; cocoindex-code deletion and incremental; Snowflake arctic-embed-xs; intfloat multilingual-e5-small prefixes; CoIR arXiv; Semgrep and Vulture current versions.

Primary pages opened:

https://github.com/cocoindex-io/cocoindex-code
https://github.com/cocoindex-io/cocoindex-code/releases
https://pypi.org/project/cocoindex-code/
https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/README.md
https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md
https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/6bb17d001649fc0948a7300c2ce847b6218d174c/pyproject.toml
https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/6bb17d001649fc0948a7300c2ce847b6218d174c/src/cocoindex_code/protocol.py
https://github.com/cocoindex-io/cocoindex-code/issues
https://github.com/cocoindex-io/cocoindex/releases
https://cocoindex.io/cocoindex-code/
https://cocoindex.io/docs/examples/index-codebase/
https://github.com/cocoindex-io/cocoindex/blob/main/examples/code_embedding/README.md
https://github.com/oraios/serena
https://github.com/oraios/serena/releases
https://github.com/oraios/serena/tags
https://raw.githubusercontent.com/oraios/serena/main/CHANGELOG.md
https://github.com/oraios/serena/blob/main/LICENSE
https://github.com/oraios/serena/discussions/1986
https://oraios.github.io/serena/01-about/020_programming-languages.html
https://oraios.github.io/serena/02-usage/050_configuration.html
https://github.com/oraios/serena/issues/1491
https://github.com/oraios/serena/issues/1889
https://github.com/oraios/serena/issues/1387
https://github.com/oraios/serena/issues/1389
https://github.com/oraios/serena/blob/main/pyproject.toml
https://github.com/oraios/serena/blob/main/src/solidlsp/language_servers/pyright_server.py
https://huggingface.co/Snowflake/snowflake-arctic-embed-xs
https://www.snowflake.com/en/engineering-blog/snowflake-arctic-embed-2-multilingual/
https://huggingface.co/intfloat/multilingual-e5-small
https://cursor.com/blog/semsearch
https://arxiv.org/abs/2407.02883
https://github.com/CoIR-team/coir
https://sourcegraph.com/docs/code_navigation/explanations/precise_code_navigation
https://sourcegraph.com/blog/the-future-of-scip
https://github.com/scip-code/scip
https://github.com/sourcegraph/scip-python
https://github.com/sourcegraph/scip-python/issues
https://github.com/zilliztech/claude-context
https://pypi.org/project/semgrep/
https://github.com/semgrep/semgrep/tags
https://github.com/jendrikseipp/vulture
https://github.com/jendrikseipp/vulture/releases
https://github.com/Nettechnologys/codeindexer
https://codeindexer.dev/faq

Lower-authority pages used only as disagreement or market color, not as acceptance evidence: https://www.toolbit.ai/blog/cursor-vs-windsurf-which-agentic-ide-solves-m , https://johal.in/code-index-vs-grep-token-cost , https://libraries.io/pypi/cocoindex-code-plus , https://rywalker.com/research/claude-context .

Not verified in this run: whether the fleet's installed CodeIndexer is the proprietary codeindexer.dev binary; cocoindex-code Windows wheel install actually succeeding; DirectML under SentenceTransformers; per-file content hashes inside LMDB; symlink and branch-switch behavior; Serena 1.7.0's exact Pyright pin; a packet capture proving the telemetry opt-out. No software was installed and no private code was sent.

Single recommendation. Run the two canaries above, in that pin, on one frozen fixture, and park both if the acceptance lines in section G are not met. Confidence **[emerging]**. The assumption is that AST chunking and a real LSP reference list are still unmeasured against this fleet's CodeIndexer, Tree-sitter, and Pyright, and that a tie is a reason to stop rather than a reason to add a daemon.

🏛H · Grok 4.7 · session 01a16cbe
