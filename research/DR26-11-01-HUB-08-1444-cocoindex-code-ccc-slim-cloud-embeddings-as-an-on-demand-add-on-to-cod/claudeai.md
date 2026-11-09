# DR26-11-01-HUB-08-1444 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Source: https://приватный чат
Mode: Research;Fable5.1Extra;Ultra profile; observed research activity before completed report. UI export with visible anchor URLs; original raw export and five code blocks retained separately. Not independently accepted.

# DR26-11-01-HUB-08-1444 — CocoIndex Code (ccc) as an on-demand, cloud-embedding add-on to CodeIndexer

Verdict: PARK by default. Move to TRY, as a single Hub-only, CLI-only canary, only if you can confirm existing API credit with an embeddings provider whose data terms you accept. No published measurement shows that ccc’s slim (cloud-embedding) mode saves RAM/CPU/GPU next to CodeIndexer. The only public resource measurements of the released 0.2.42 daemon come from a third party running local embeddings on Windows, and they are bad. The free cloud-embedding options available without a card come with model-training data terms.

## TL;DR

- Identity verified. The released product is `cocoindex-code` 0.2.42: tag `v0.2.42`, commit `6bb17d0`, published Oct 5, 2026, Apache-2.0, Python >= 3.11, extras `dev`/`embeddings-local`/`full`, shipped as a pure `py3-none-any` wheel. “Slim” is the documented name for the base install with no extras (and for the `:latest` Docker tag). It is not a separate package. The native Rust engine comes in through the `cocoindex` framework wheels.

- The cost and resource case is unproven, and some evidence points against it. The only RSS/CPU numbers for 0.2.42 come from takkub/agent-takkub issue #901, measured on Windows 11 (16 logical cores, 39.6 GB RAM). The `ccc.exe run-daemon` process used about 62% CPU in a 5-second sample and 1.8 GB RAM with a local model, “even when the active project has no index”. Slim mode has never been measured. The released stack still makes network calls even with cloud embeddings: anonymous telemetry, and LiteLLM’s import-time price-map fetch. It offers semantic search only: no exact/full-text search, no call graph, and no PowerShell grammar.

- Recommendation. CodeIndexer stays as the baseline. Run the preregistered Hub trial (amended as described below) only with confirmed existing API credit, through the CLI, against a project COPY, in an isolated `COCOINDEX_CODE_DIR`. OpenAI `text-embedding-3-small` is the most terms-compatible paid option at $0.02/1M tokens. Voyage’s 200M free tokens require accepting training use, and opting out requires a payment method. If you lack confirmed credit, PARK.

## Key Findings

#FindingEvidence classSource

10.2.42 was released Oct 5, 2026 from commit `6bb17d001649fc0948a7300c2ce847b6218d174c`; Apache-2.0; Requires Python >=3.11; classifiers 3.11/3.12/3.13; extras `dev`, `embeddings-local`, `full`Implementation fact (registry metadata)[https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/) [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

2Wheel `cocoindex_code-0.2.42-py3-none-any.whl` is 91.8 kB (SHA-256 `6ad19ef0065a73cd0ddb2c7d7abe270751df7c920f6584cd23c3cb5676a27590`); sdist is 94.6 kB (SHA-256 `9c9bc290a490735ee9c6b32c2ca85ca6553b150295cd07791774148ef19f2902`); both carry GitHub Actions provenanceImplementation fact[https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/) [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

3GitHub marks the release “Latest”; main had “9 commits to main since this release” when fetchedImplementation fact[https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42) [github](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42)

4Slim = “LiteLLM-only; requires a cloud embedding provider and API key”; full “Pulls in `sentence-transformers`”Vendor documentation[https://github.com/cocoindex-io/cocoindex-code](https://github.com/cocoindex-io/cocoindex-code) [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

5The released `ccc grep` is not usable on released cocoindex: “Until it ships in a released cocoindex, run against a local cocoindex build”Vendor documentation (disconfirms the feature)[https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/) [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

6Third-party measurement on Windows 11, 16 logical cores: `ccc.exe run-daemon (cocoindex-code 0.2.42)` at ~62% of the whole machine (~10 cores), 1.8 GB, 107 threads, local `snowflake-arctic-embed-xs` on CPUMeasured by a third party (not slim)[https://github.com/takkub/agent-takkub/issues/901](https://github.com/takkub/agent-takkub/issues/901) [github](https://github.com/takkub/agent-takkub/issues/901)

7Auto-refresh by watcher is not released: open issue “support index auto refresh based on watchdog” (#276)Implementation fact[https://github.com/cocoindex-io/cocoindex-code/issues](https://github.com/cocoindex-io/cocoindex-code/issues) [github](https://github.com/cocoindex-io/cocoindex-code/issues)

8Full-text search is not released: open issue “support full text search in cocoindex-code” (#192)Implementation fact[https://github.com/cocoindex-io/cocoindex-code/issues](https://github.com/cocoindex-io/cocoindex-code/issues) [github](https://github.com/cocoindex-io/cocoindex-code/issues)

9Voyage `voyage-code-3` (the model shown in ccc’s README example) is now an “older” model at $0.18/1M with 0 free tokens. The Voyage FAQ recommends `voyage-code-4` “for code-related tasks such as coding agent use cases”; it costs $0.12/1M with 200M freeOfficial pricing[https://www.mongodb.com/docs/voyageai/management/billing/](https://www.mongodb.com/docs/voyageai/management/billing/) , [https://docs.voyageai.com/docs/faq](https://docs.voyageai.com/docs/faq)

10Voyage’s default terms license customer content for training; opting out requires “a payment method on file,” and opting out may void free tokensOfficial terms[https://www.voyageai.com/tos](https://www.voyageai.com/tos) , [https://docs.voyageai.com/docs/faq](https://docs.voyageai.com/docs/faq) [voyageai](https://www.voyageai.com/tos)[voyageai](https://docs.voyageai.com/docs/faq)

## Details

### Section A — Product identity, version, and namesakes

Verified (implementation facts):

- Package and repo. `cocoindex-code` on PyPI is published from `github.com/cocoindex-io/cocoindex-code`. The publishing commit is `6bb17d001649fc0948a7300c2ce847b6218d174c` and the workflow is `.github/workflows/release.yml` ([https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/)). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) The GitHub release `v0.2.42` was made by georgeh0 on “05 Oct 21:21” at commit `6bb17d0` with a verified signature ([https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42)). [github](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42) This confirms the operator’s 0.2.42 / v0.2.42 / Oct 5 / Apache-2.0 / Python >= 3.11.

- Release contents. v0.2.42 adds an Oh My Pi marketplace, Elixir support, OMP session-start/post-edit index hooks, and dependency bumps, including “litellm from 1.84.0 to 1.88.6” and “sentence-transformers from 5.2.2 to 5.6.0” (same release page). [github](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42) Those bumps most likely touch the repo’s `uv.lock` and dev environment. A `uv tool install` does not use the repo lockfile, so your resolved versions can differ.

- Release history and yanks. 0.2.38 was yanked, with the reason pointing to issue #237. [pypi](https://pypi.org/project/cocoindex-code/) 0.2.39 followed with “fix(client): survive handshake from a protocol-incompatible daemon” ([https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/), [https://github.com/cocoindex-io/cocoindex-code/releases](https://github.com/cocoindex-io/cocoindex-code/releases)). [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)[github](https://github.com/cocoindex-io/cocoindex-code/releases) Treat daemon/client protocol compatibility as a known fragility, and never mix ccc versions that share one `COCOINDEX_CODE_DIR`.

- CLI names. `ccc` is the CLI. The bare `cocoindex-code` command still works as an MCP server for backward compatibility and “auto-creates settings from environment variables on first run” (README on PyPI). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Avoid the bare command, because it can write settings silently.

- Wheels and OS coverage. `cocoindex-code` itself is pure Python (`py3-none-any`). The native Rust engine is the `cocoindex` framework (a separate package, currently 1.0.25). Its wheels are `cp311-abi3` for Windows x86-64, macOS 11+ ARM64, macOS 10.12+ x86-64, and Linux glibc 2.28+ x86-64/ARM64, about 9.6–10.1 MB each, uploaded Oct 6, 2026 ([https://pypi.org/project/cocoindex/](https://pypi.org/project/cocoindex/)). [pypi](https://pypi.org/project/cocoindex/) That covers all three of your hosts (Windows Hub, Intel Mac Pro, Apple Silicon Mac Studio) at the engine level.

- Caveat: cocoindex 1.0.25 came out one day after ccc 0.2.42, so a fresh install today may resolve to a framework build the ccc release never shipped with. Pin both.

- “Slim” status. “Slim” is the README’s name for installing `cocoindex-code` with no extra, and also for the Docker tag `cocoindex/cocoindex-code:latest (slim, default)`. [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) There is no package called `cocoindex-code-slim`, and none should be invented. The PyPI extras list (`dev`, `embeddings-local`, `full`) has no “slim” entry. [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

- Released vs. main. About 9 commits had landed on main after v0.2.42 when fetched. The GitHub README on main already differs from the 0.2.42 PyPI README. For example, main says hooks fire “when `.cocoindex_code/settings.yml` exists (never in the directory holding ccc’s user settings, `~` by default)” and adds a note that `ccc index` never auto-initializes the settings-holding directory. The 0.2.42 PyPI text says only “when `.cocoindex_code/` exists” (compare [https://github.com/cocoindex-io/cocoindex-code](https://github.com/cocoindex-io/cocoindex-code) with [https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/)). [pypi](https://pypi.org/project/cocoindex-code/)[github](https://github.com/cocoindex-io/cocoindex-code) Treat the home-directory guard as UNRELEASED unless you verify it in 0.2.42 code.

Namesakes (do not confuse):

- `cocoindex`: the Rust/Python incremental framework ([https://pypi.org/project/cocoindex/](https://pypi.org/project/cocoindex/)). [pypi](https://pypi.org/user/cocoindex/)

- `cocoindex-code-plus`: “The ccx client CLI for CocoIndex Code Plus,” published from `cocoindex-io/cocoindex-code-plus`. Versions 0.1.69/0.1.71/0.1.72 appeared Oct 21–23, 2026 ([https://pypi.org/project/cocoindex-code-plus/0.1.72/](https://pypi.org/project/cocoindex-code-plus/0.1.72/)). [pypi + 2](https://pypi.org/project/cocoindex-code-plus/0.1.69/) This is a remote/hosted client and is out of scope. Its current latest version is UNKNOWN.

- Third-party look-alikes: `aanno/cocoindex-code-mcp-server` (needs PostgreSQL + pgvector; [https://github.com/aanno/cocoindex-code-mcp-server](https://github.com/aanno/cocoindex-code-mcp-server)), and many forks (e.g., faysou, kapoorsunny, 2233admin). [github + 3](https://github.com/aanno/cocoindex-code-mcp-server) None of them is the product under evaluation.

### Section B — Dependency graph, sizes, and hidden network or downloads

Verified:

- Engine dependencies. The framework’s own pyproject (main branch, not pinned to 1.0.25) declares `typing-extensions`, `click`, `rich`, `python-dotenv`, `watchfiles`, `numpy`, and `psutil`, builds with maturin, and has `requires-python >=3.11` ([https://github.com/cocoindex-io/cocoindex/blob/main/pyproject.toml](https://github.com/cocoindex-io/cocoindex/blob/main/pyproject.toml)). [github](https://github.com/cocoindex-io/cocoindex/blob/main/pyproject.toml) So even the slim install pulls in numpy and watchfiles, a file-watcher library. ccc’s released CLI does not expose a watcher (see D), but the library is installed.

- Storage. Index databases are `cocoindex.db` and `target_sqlite.db` in `<project>/.cocoindex_code/`. The index uses “an LMDB database whose maximum size is fixed when the daemon starts,” with a 4 GiB default ceiling (README on PyPI). [pypi](https://pypi.org/project/cocoindex-code/) Troubleshooting mentions `sqlite3.Connection ... enable_load_extension`, which implies a loadable SQLite vector extension. [pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Postgres is not used by ccc; Postgres/pgvector belongs to framework examples and third-party servers.

- AST layer. Chunking uses the framework’s tree-sitter `RecursiveSplitter`, which “fall[s] back to separator-based splitting for unsupported languages” ([https://cocoindex.io/docs/ops/](https://cocoindex.io/docs/ops/)). [cocoindex](https://cocoindex.io/docs/ops/)

- Network activity even with cloud embeddings:

- Telemetry. “`cocoindex-code` sends anonymous usage telemetry through CocoIndex.” To opt out, set `COCOINDEX_DISABLE_USAGE_TRACKING=1` (README on PyPI). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

- LiteLLM price map. On import, LiteLLM fetches `model_prices_and_context_window.json` from GitHub unless `LITELLM_LOCAL_MODEL_COST_MAP=True` is set ([https://docs.litellm.ai/docs/completion/token_usage](https://docs.litellm.ai/docs/completion/token_usage), [https://docs.litellm.ai/docs/proxy/custom_model_cost_map](https://docs.litellm.ai/docs/proxy/custom_model_cost_map)). [litellm](https://docs.litellm.ai/docs/proxy/custom_model_cost_map)[litellm](https://docs.litellm.ai/docs/completion/token_usage) Whether ccc’s daemon import path triggers this fetch is UNKNOWN. Set the variable anyway.

- Embedding API calls. Every changed chunk and every query is sent to the provider.

- Tokenizer and model downloads. In slim mode, no local embedding model should be downloaded. Docker docs say the slim image is “unaffected” because “LiteLLM runs the model on the provider’s side.” [pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Whether LiteLLM or ccc downloads a tokenizer (e.g., a tiktoken BPE file) for token counting or truncation is UNKNOWN; measure it with egress logging. Tree-sitter grammars ship compiled inside the cocoindex wheel (inferred from the 10 MB native wheel; not verified).

- Docker default mount. The default compose file mounts your whole home directory into the container (README). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) That is excluded by your constraints, and it is a security footgun.

Inferred from secondary sources, not verified at 0.2.42: DeepWiki (generated from an older commit `2a3ba53b`) lists ccc core dependencies as `mcp`, `cocoindex[litellm]`, `sqlite-vec`, `pydantic`, `numpy`, `einops`, and `typer`, with `full` → `cocoindex[sentence-transformers]` ([https://deepwiki.com/cocoindex-io/cocoindex-code/2-getting-started](https://deepwiki.com/cocoindex-io/cocoindex-code/2-getting-started)). [deepwiki](https://deepwiki.com/cocoindex-io/cocoindex-code/2.1-installation) The exact `requires_dist` pins for 0.2.42 and the contents of `embeddings-local` are UNKNOWN. Our subagent could not fetch the PyPI JSON or the tag’s raw pyproject. Get them with `uv pip show`, or by unpacking the wheel’s METADATA, before installing.

Size claims, separated by type:

ClaimWhat it actually isStatus

“~1 GB of torch + transformers” avoided by slimThe 0.2.42 PyPI README says slim is for when “you don’t want the local-embedding deps (~1 GB of torch + transformers)”Vendor estimate, not measured. Installed-environment size for slim: UNKNOWN

“~450 MB” `:latest`Docker image size (compressed or unpacked not stated)Vendor claim; irrelevant to a native install [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

“~5 GB” `:full`Docker image sizeVendor claim [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

91.8 kBccc wheel sizeRegistry fact (does not include dependencies) [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

~10 MBcocoindex native wheel, per platformRegistry fact

“zero local resource”Not stated by ccc docs; would be false anyway (daemon, LMDB, SQLite, Rust engine, numpy)Reject

“Instant token saving by 70%” (verbatim in the 0.2.42 PyPI README, next to “1 min setup”) / “70% fewer tokens per turn, 80-90% cache hits on re-index, sub-second freshness”README and framework PyPI marketingUnsubstantiated marketing; no method published

Disconfirming evidence (marketing vs. code): The framework’s PyPI page says CocoIndex-code “keeps live call graphs, symbols, vectors, and chunks fresh on every commit” and offers “call graphs and blast-radius analysis” ([https://pypi.org/project/cocoindex/](https://pypi.org/project/cocoindex/)). [pypi](https://pypi.org/project/cocoindex/)[pypi](https://pypi.org/project/cocoindex/) The released ccc 0.2.42 MCP surface exposes only `search`, with no call-graph tool ([https://pypi.org/project/cocoindex-code/0.2.42/](https://pypi.org/project/cocoindex-code/0.2.42/)). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Do not credit ccc with call graphs, references, or flow analysis.

### Section C — Pipeline and retrieval behavior

Verified:

- Scanning. Default `include_patterns` cover “28+ file types.” Default excludes are `**/.*` (hidden dirs), `**/__pycache__`, `**/node_modules`, and `**/dist`. `max_file_size` is “default: no limit” (inclusive limit, units B/KB/MB/GB). `ccc init` adds `.cocoindex_code/` to `.gitignore`. `ccc grep` “honors the project’s include/exclude patterns and `.gitignore`” (README). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Whether the indexer honors `.gitignore` is not stated for 0.2.42; DeepWiki claims “.gitignore integration” ([https://deepwiki.com/cocoindex-io/cocoindex-code/4-core-components](https://deepwiki.com/cocoindex-io/cocoindex-code/4-core-components)). [deepwiki](https://deepwiki.com/cocoindex-io/cocoindex-code/4-core-components) Symlink handling, binary-file detection, and encoding errors: UNKNOWN.

- Languages. The supported-languages table includes `python` (.py), `javascript` (.js), `typescript` (.ts), and `tsx` (.tsx). PowerShell is absent: no `.ps1`, `.psm1`, or `.psd1` (README on PyPI). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) PowerShell files are not indexed by default. If you force them in with `include_patterns` plus `language_overrides`, they get the framework’s separator fallback, not AST chunking. Whether `language_overrides` accepts an unknown language name is UNKNOWN. `.mjs`, `.cjs`, and `.jsx` are not listed either.

- Chunk sizes. ccc’s actual `chunk_size`, `min_chunk_size`, and `chunk_overlap` are UNKNOWN for 0.2.42. The framework’s documented example is `chunk_size=1000, min_chunk_size=300, chunk_overlap=300` ([https://cocoindex.io/docs/ops/](https://cocoindex.io/docs/ops/)), but it is not confirmed as ccc’s setting. [cocoindex](https://cocoindex.io/docs/ops/) Custom per-extension chunkers are supported through `settings.yml` `chunkers:`, with the signature `my_chunker(path, content) -> (language_override, list[Chunk])`. [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

- Structural grep. Documented, but it needs an unreleased cocoindex `code_match`; treat it as not released. (There is a conflict: framework docs show `CodePattern` in `cocoindex.ops.code`, [https://cocoindex.io/docs/ops/text/](https://cocoindex.io/docs/ops/text/). [cocoindex](https://cocoindex.io/docs/ops/text/) Whether the released 1.0.25 satisfies ccc grep is UNKNOWN.)

- Search surface. `search(query, limit=5 [1–100], offset=0, refresh_index=True, languages, paths)` returns chunks with path, language, code, line numbers, and similarity score. By default the CLI search is scoped to the current working directory (README). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

- Known bug: “ccc search --path crashes the daemon” (#270, open) ([https://github.com/cocoindex-io/cocoindex-code/issues](https://github.com/cocoindex-io/cocoindex-code/issues)). [github](https://github.com/cocoindex-io/cocoindex-code/issues)

Task-class implications (hypotheses to test, not measured):

ClassExpected ccc behaviorWhy

Known-symbol (e.g., “where is `Get-HubConfig` defined”)Weak to unreliable; worse for PowerShell (not indexed)Embedding similarity is not symbol resolution; no symbol table is exposed

Exact string / regexNot supported nativelyFull-text search is an open feature request (#192); `ccc grep` is unreleased [github](https://github.com/cocoindex-io/cocoindex-code/issues)

Semantic-EN (“where are retries with backoff handled”)Most plausible winThis is the design target

Semantic-RU ("где обрабатываются повторные попытки")UNKNOWN; depends on the embedder’s multilingual qualityOpenAI and Gemini embedders are multilingual (Gemini “supports over 100 languages,” [https://developers.googleblog.com/gemini-embedding-available-gemini-api/](https://developers.googleblog.com/gemini-embedding-available-gemini-api/)); code-specialized models may be English-biased [googleblog](https://developers.googleblog.com/gemini-embedding-available-gemini-api/)

Navigation (callers/callees/imports)Not supportedAST chunk boundaries ≠ references, callers, or data flow

No-answerHigh false-positive risk`search` returns top-k with scores; no documented abstention threshold

### Section D — Lifecycle, daemon, freshness

Verified:

- Storage layout. Per-project databases live in `<project>/.cocoindex_code/` and can be remapped with `COCOINDEX_CODE_DB_PATH_MAPPING=/src=/dst` (absolute prefixes, first match wins). Global settings live in `~/.cocoindex_code/global_settings.yml`, or under `COCOINDEX_CODE_DIR` (README). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

- Daemon start and idle behavior. The daemon “starts automatically on first use.” [deepwiki](https://deepwiki.com/cocoindex-io/cocoindex-code/2-getting-started) The setting `daemon.idle_timeout_minutes` defaults to 180 (0 = never), and `daemon.keep_alive_with_mcp` defaults to true, meaning “a live MCP session sends periodic heartbeats so the daemon remains warm” (README on PyPI). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) This confirms the operator’s 180 min and the true MCP keepalive. It also means any live MCP client pins the daemon indefinitely, which conflicts with your “no permanent background” rule. Use the CLI, or set `keep_alive_with_mcp: false`.

- IPC. Per DeepWiki (secondary), the client and daemon use “Unix domain sockets or Windows named pipes” with msgpack ([https://deepwiki.com/cocoindex-io/cocoindex-code/3-architecture](https://deepwiki.com/cocoindex-io/cocoindex-code/3-architecture)). [deepwiki](https://deepwiki.com/cocoindex-io/cocoindex-code/3-architecture)[deepwiki](https://deepwiki.com/cocoindex-io/cocoindex-code/4-core-components) On macOS, the files are `~/.cocoindex_code/daemon.sock` and `daemon.pid` ([https://github.com/cocoindex-io/cocoindex-code/issues/288](https://github.com/cocoindex-io/cocoindex-code/issues/288)). [github](https://github.com/cocoindex-io/cocoindex-code/issues/288) Whether these move with `COCOINDEX_CODE_DIR` is UNKNOWN; verify before the trial.

- Windows history (plus one cross-platform daemon issue):

- v0.2.4 fixed “daemon showing visible console window on Windows” ([https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.4](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.4)). [github](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.4)

- PR #243 serialized daemon startup across processes, including “avoid reading locked startup byte on Windows”. Its validation reports “six launches before, one after.” On the same PR, GitHub user noVaSon still reproduced duplicate daemons on macOS (not Windows): “Hit this on 0.2.32 (macOS, arm64) — eleven ccc run-daemon processes, oldest nearly 10 days, all idle”, because “single-instance isn’t enforced daemon-side” ([https://github.com/cocoindex-io/cocoindex-code/pull/243](https://github.com/cocoindex-io/cocoindex-code/pull/243)).

- Issue #166, “Windows: stale daemon can preserve missing embeddings env; search output can also crash on GBK consoles,” was listed as open ([https://github.com/cocoindex-io/cocoindex-code/issues](https://github.com/cocoindex-io/cocoindex-code/issues)). [github](https://github.com/cocoindex-io/cocoindex-code/issues)

- Issue #288: killing a stale daemon “deletes the live daemon’s socket” ([https://github.com/cocoindex-io/cocoindex-code/issues/288](https://github.com/cocoindex-io/cocoindex-code/issues/288)). [github](https://github.com/cocoindex-io/cocoindex-code/issues/288)

- Refresh paths. Available: `ccc index`, `ccc search --refresh`, and the MCP `refresh_index=True` default (an incremental index before every MCP search), plus plugin hooks (SessionStart/PostToolUse), which you exclude. No released watcher or polling (#276 open). [pypi](https://pypi.org/project/cocoindex-code/0.2.42/)[github](https://github.com/cocoindex-io/cocoindex-code/issues) Creates, edits, renames, and deletes become visible only after a refresh.

- Model or dimension change. “Switching models requires re-indexing your codebase (`ccc reset && ccc index`).” Chunker changes require `ccc daemon restart` and then `ccc index` (README). [pypi](https://pypi.org/project/cocoindex-code/)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/)

- Open robustness bugs: “Search/index do not recover when target_sqlite.db is missing but state DB remains” (#210), and “Creation of multiple ccc projects for same codebase” (#204) ([https://github.com/cocoindex-io/cocoindex-code/issues](https://github.com/cocoindex-io/cocoindex-code/issues)). The third-party gh-plan PR notes that “The one ccc daemon holds a project’s databases open and keeps writing to them once deleted, so a reopened issue’s worktree at the same path would search a stale index” ([https://github.com/pjfitzgibbons/gh-plan/pull/128](https://github.com/pjfitzgibbons/gh-plan/pull/128)). [github](https://github.com/pjfitzgibbons/gh-plan/pull/128)[github](https://github.com/cocoindex-io/cocoindex-code/issues) That is direct evidence of a stale-hit risk after delete/recreate at the same path.

- Concurrency. “Multiple projects may prepare indexes concurrently” (README). [github](https://github.com/cocoindex-io/cocoindex-code)[pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Interrupted-update atomicity is UNKNOWN.

- Identity and paths. Repo/revision identity is not modeled. The project is identified by its filesystem root, and there is no git-revision field. Whether stored paths are relative or absolute is UNKNOWN. Never share an index across OSes or checkouts. Index each host’s COPY separately, and map E:\… ↔ /Users/… only in your evidence layer, never in ccc.

### Section E — Minimal documented configuration (Hub first)

These commands follow documented interfaces. Values shown as `<...>` are yours. No credentials are shown. Put the API key in the process environment only, never in YAML, logs, or evidence.

1. Pinned, isolated, native base install (no `full`, no `embeddings-local`):

```
uv tool install "cocoindex-code==0.2.42" --python 3.12
# then record the exact resolved versions (incl. cocoindex, litellm) as evidence:
uv tool list --show-paths
```

Before production use, pin `cocoindex` too, with a constraints file containing the version the tool resolved. Python 3.12 sits inside the classifier range (3.11–3.13); 3.14 is unlisted for ccc. Do not run `npx skills add`, `/plugin install`, Grok/OMP plugins, hooks, or Docker compose.

2. Isolated environment (PowerShell on the Hub; use `export` on macOS):

```
$env:COCOINDEX_CODE_DIR = "<D:\ccc-trial\state>"      # NOT named .cocoindex_code, NOT your home
$env:COCOINDEX_DISABLE_USAGE_TRACKING = "1"
$env:LITELLM_LOCAL_MODEL_COST_MAP = "True"
$env:<key-var> = "<from your secret store; never echoed>"
```

3. `<COCOINDEX_CODE_DIR>\global_settings.yml`:

yaml

```
embedding:
  provider: litellm
  model: text-embedding-3-small
  min_interval_ms: 300
daemon:
  idle_timeout_minutes: 5
  keep_alive_with_mcp: false
```

4. `<COPY>\.cocoindex_code\settings.yml` (the copy is an immutable, allowlisted snapshot):

yaml

```
include_patterns: ["**/*.py", "**/*.js", "**/*.ts", "**/*.tsx"]
exclude_patterns: ["**/.*", "**/node_modules", "**/dist", "**/__pycache__"]
max_file_size: 500KB
```

5. Lifecycle:

```
cd <COPY>
ccc init            # one-time; non-interactive runs require it first
ccc doctor          # checks settings, daemon, model (both indexing & query sides), file matching
ccc index
ccc status
ccc search --limit 5 "where are HTTP retries handled"
ccc search --refresh --limit 5 "<query>"   # manual refresh, label as MANUAL
ccc daemon stop
```

Expose ccc to Claude/Codex/Cursor through their shell tool calling `ccc search`, not a registered MCP server. If a trial arm needs MCP, register it per-session and remove it afterwards.

Provider choice (official pricing and terms, as found Nov 1, 2026):

Provider/modelPriceFree allowanceTraining on your dataLimitsFit

OpenAI `text-embedding-3-small`$0.02/1M; Batch $0.01/1M ([https://developers.openai.com/api/docs/models/text-embedding-3-small](https://developers.openai.com/api/docs/models/text-embedding-3-small))None documented; Free-tier rate limits 100 RPM / 2,000 RPD / 40,000 TPM (same page)“data sent to the OpenAI API is not used to train or improve OpenAI models (unless you explicitly opt in)”; abuse logs “up to 30 days” ([https://developers.openai.com/api/docs/guides/your-data](https://developers.openai.com/api/docs/guides/your-data))8,191 input tokens, 1,536 dims (third-party listing: [https://zilliz.com/ai-models/text-embedding-3-small](https://zilliz.com/ai-models/text-embedding-3-small))Best fit only if existing API credit is confirmed. A ChatGPT subscription is not API credit [zilliz + 2](https://zilliz.com/ai-models/text-embedding-3-small.md)

Voyage `voyage-code-4`$0.12/1M200M tokens per accountDefault license to train unless opted out; opt-out needs a payment method and “may” void free tokens ([https://www.voyageai.com/tos](https://www.voyageai.com/tos), [https://docs.voyageai.com/docs/faq](https://docs.voyageai.com/docs/faq))LiteLLM support for `voyage-code-4`: UNKNOWNConflicts with “no new card” if you want no training [mongodb + 2](https://www.mongodb.com/docs/voyageai/management/billing/)

Voyage `voyage-code-3` (README example)$0.18/1M0Same as above—Avoid; billed from token 1 [mongodb](https://www.mongodb.com/docs/voyageai/management/billing/)

Gemini `gemini-embedding-001`$0.15/1MFree tier existsFree tier: “Used to improve our products: Yes”; paid: “No” ([https://aistudio.google.com/docs/pricing](https://aistudio.google.com/docs/pricing))2,048 max input tokens; 3,072 default dims ([https://developers.googleblog.com/gemini-embedding-available-gemini-api/](https://developers.googleblog.com/gemini-embedding-available-gemini-api/))Free tier = training; short input limit may truncate chunks [google + 2](https://aistudio.google.com/docs/pricing)

None of these is a “hidden generation/rerank” path. ccc calls only the embedding endpoint (inferred from docs: `indexing_params`/`query_params` accept only `input_type` for LiteLLM). [pypi](https://pypi.org/project/cocoindex-code/0.2.42/) Credit eligibility for any of your accounts: UNKNOWN. An API key alone does not prove paid permission or remaining balance.

### Section F — Cost model (formula; no invented actual cost)

Formula (embedding-provider dollars only):
`Cost = P × (T_init × k_overlap + Σ T_changed_chunks + N_queries × T_query + T_retries + T_rebuilds)`

- `P` = $/token (e.g., 2×10⁻⁸ for 3-small).

- `k_overlap` = token inflation from chunk overlap (UNKNOWN for ccc; ≥1).

- `T_changed_chunks`: changed files are re-chunked, so more than the edited lines gets re-embedded (granularity UNKNOWN).

- `N_queries`: each MCP search with `refresh_index=True` also pays for any pending changed chunks. A no-op refresh should cost $0 in embeddings, but local CPU is still spent (unmeasured).

- `T_rebuilds`: a full `T_init` on any model or dimension change, or on reset after corruption (#210).

- Client context tokens (search results pasted into Claude/Codex/Cursor) are billed or quota’d by the LLM client, not the embedder. They count toward the trial’s 24,000-byte budget, not this formula.

Illustrative arithmetic (hypothetical inputs, not a measurement): a 5M-token snapshot × k_overlap 1.3 = 6.5M tokens. On 3-small that is ≈ $0.13 initial. Add 50 queries/day × 30 tokens and 200k changed-chunk tokens/day, about 6.5M tokens/month, ≈ $0.13/month. On voyage-code-4 the same volume uses about 13M of the 200M free tokens, at the price of the training-terms issue.

When CodeIndexer alone is cheaper:

- In dollars: always (ccc’s cost is purely additive).

- In local resources: whenever the ccc daemon’s added RSS/CPU exceeds what it saves the LLM client in reads and turns. Slim-mode overhead is unmeasured, so this is UNKNOWN until the trial.

- In correctness per byte: for known-symbol, exact, navigation, and no-answer tasks, where ccc has no native capability.

### Section G — Critique of the preregistered trial (before any experiment)

The design is sound in principle (blinded, paired, AB/BA, hard gates, preregistered thresholds). The source-level findings above call for these pre-trial amendments:

- Scope the corpus to what ccc can index. PowerShell is unsupported. Either (a) pre-register that PowerShell tasks test the shared source-read fallback only, or (b) exclude `.ps1` from ccc’s include list and report coverage per language. The “>= 90% coverage” gate must be defined as files ccc claims to index, computed from `ccc status`, and reported separately from total corpus coverage. Otherwise the gate silently fails or passes for the wrong reason.

- The class mix is lopsided by design. ccc has no native exact, navigation, or no-answer capability (Section C). Expected benefit concentrates in 8 of 24 tasks (semantic-RU/EN). The rule “correctness improves >= 3/24” therefore requires winning 3 of about 8 eligible tasks. That is fine, but state it up front so nobody later fits thresholds after outcomes.

- Statistical power. With 4 tasks per class and 3 paired repetitions, a 1/24 decline is within run-to-run noise for LLM answerers. Keep the operational thresholds, but pre-register the aggregation (e.g., per-task majority across 3 reps) and treat results as operational, not statistical proof. That framing is already in the brief, and it is correct.

- Daemon attribution. The daemon is detached and auto-started; it is not a child of the client. “Tree peak RSS” measured from the client will miss it. Measure by process identity (`ccc run-daemon` / `ccc.exe`, plus any GPU or subprocess children), and use private bytes on Windows and phys_footprint on macOS. Record GPU VRAM as UNKNOWN or N/A, never 0.

- Cold vs. warm. “Cold” means no ccc daemon running in the isolated `COCOINDEX_CODE_DIR`, with OS file cache state recorded. Do not restart production to manufacture it. With `idle_timeout_minutes: 5`, a 60-second idle window keeps the daemon warm. Explicitly `ccc daemon stop` the owned daemon for cold runs.

- Freshness protocol. No watcher exists, so “index-observed” will never happen without a trigger. Pre-register two arms: (a) MCP `refresh_index=True`, which is implicit refresh but costs embeddings and CPU per poll, and (b) CLI manual `ccc index`, labeled MANUAL. 5-second polls up to 120 seconds with `refresh_index=True` mean up to 24 refreshes per mutation; count their cost. For deletes, test delete-then-recreate at the same path specifically (gh-plan PR #128 evidence) and enforce “no stale delete hit at 120 s.”

- Network accounting. Log egress per host: provider API, telemetry (should be zero with opt-out), GitHub raw (LiteLLM map), and any tokenizer fetch. Unexpected egress fails the “no secrets” gate unless explained.

- Context accounting. The default `limit=5` with full chunk code can produce large JSON. Count bytes after final serialization, as specified. Note that the MCP and CLI output formats differ, so pre-register which one each arm uses.

- Billing receipts. Provider usage dashboards lag (latency UNKNOWN). Reconcile receipts after the window, and keep cost UNKNOWN until a receipt exists.

- Root maps. Index paths are relative to the project root in the CLI’s `--path` semantics, but storage representation is UNKNOWN. Compare answers using NFC-normalized, repo-relative paths, and never equate `E:\` with `/Users/`.

- Version freeze. Freeze `cocoindex-code==0.2.42`, the resolved `cocoindex`, and the resolved `litellm`. Record their hashes. A dependency drift mid-trial invalidates the paired comparison.

- Zero-success rule. If ccc produces zero successful retrievals (the October mgrep failure mode), the result is “no evidence,” not “no benefit.”

### Section H — Adversarial assessment and decision

Strongest case FOR:

- Semantic, natural-language retrieval (EN, plus possibly RU with a multilingual embedder) is a capability CodeIndexer may lack.

- The slim install avoids torch. [deepwiki](https://deepwiki.com/cocoindex-io/cocoindex-code/2.1-installation)

- Incremental re-embedding should keep the ongoing cost tiny at $0.02/1M.

- The native engine has wheels for all three hosts.

- The release is Apache-2.0, with signed provenance and an active maintainer cadence.

Strongest case AGAINST:

- Zero measured benefit exists; the October comparison is invalid.

- The only 0.2.42 resource data shows a heavy daemon (local mode, Windows).

- Daemon lifecycle bugs: duplicates, socket deletion, stale env on Windows, and a crash with `--path`.

- No watcher, no exact search, no navigation, no PowerShell.

- Marketing overclaims (call graphs, 70%) contradict the released surface.

- Cost-free providers impose training terms, and Voyage’s opt-out needs a card.

- Any live MCP session keeps the daemon alive by default.

Evidence matrix (release-pinned where possible):

QuestionEvidencePinned to 0.2.42?Status

Version/license/PythonPyPI 0.2.42 pageYesVerified

Slim = base, no extraREADME on PyPI 0.2.42YesVerified

Exact dependency pinsDeepWiki (older commit)NoUNKNOWN

Chunk sizesFramework docs exampleNoUNKNOWN

Slim RSS/CPUNone—UNKNOWN

Local-mode daemon costtakkub #901 (third party)Yes (0.2.42)Measured, not slim

Windows IPCDeepWikiNoSecondary

WatcherIssue #276 openYesNot released

Training termsVoyage ToS/FAQ, OpenAI data controls, Gemini pricingn/aVerified (Nov 1, 2026)

Missing evidence: slim idle and peak RSS per host; installed environment size; egress list; stored path format; behavior on symlinks and binaries; Windows named-pipe robustness under concurrent clients; RU retrieval quality; confirmed credit balance.

Disconfirming tests (any one of these → PARK):

- Slim idle RSS above 256 MiB or peak above 1 GiB on the Hub.

- Any stale delete hit at 120 s.

- Unexplained egress.

- More than one `ccc run-daemon` after a client storm (5 concurrent `ccc search`).

- Semantic-EN shows no improvement over CodeIndexer at equal byte budget.

- Any new confidently false answer.

Minimal canary (Hub only):

- Pinned native base install.

- Isolated `COCOINDEX_CODE_DIR`.

- Project COPY (Python/JS/TS only).

- OpenAI 3-small only if existing API credit is confirmed.

- CLI invocation only.

- `idle_timeout_minutes: 5`, `keep_alive_with_mcp: false`.

- Telemetry off, `LITELLM_LOCAL_MODEL_COST_MAP=True`.

Cost ceiling: stop at 20M embedded tokens (≈ $0.40 at list price), or at the confirmed remaining credit, whichever is lower. Set a provider-side project budget or limit if your account offers one (availability UNKNOWN). Expand to the Intel Mac Pro and Mac Studio only after a measured Hub benefit plus independent review and readback.

Rollback (owned scope only):

- With the trial env vars set, run `ccc daemon stop`. Do not `kill`/`pkill`, because of issue #288 socket deletion. [github](https://github.com/cocoindex-io/cocoindex-code/issues/288) Confirm that no `ccc run-daemon` process remains with your `COCOINDEX_CODE_DIR`.

- Remove any per-session MCP registration from the temporary client.

- Leave production CodeIndexer, its config, the COPY, the `.cocoindex_code/` DBs, and all evidence intact.

No new framework or guard is needed. An install that completes, an ACK, rc=0, or a returned snippet is not a fleet PASS.

## Recommendations

- Today: PARK. Confirm whether any existing account has API credit (not a chat subscription), and check its data terms.

- If confirmed: run the Hub canary with the Section G amendments. Pre-register them before any run.

- Do not use Voyage’s free tier, Gemini’s free tier, or the README’s `voyage-code-3` example unless you explicitly accept training use, or (for `voyage-code-3`) zero free tokens.

- Do not use the Docker compose default ($HOME mount), plugins, hooks, the skills installer, or long-lived MCP.

- KEEP only under the preregistered rule. Without measured resource data, make no resource-saving claim.

## Caveats

- The exact 0.2.42 dependency pins, chunk parameters, symlink/binary handling, the location of daemon files under `COCOINDEX_CODE_DIR`, and the Windows IPC implementation were not verified from 0.2.42 source. The DeepWiki material is secondary and comes from an older commit.

- The third-party CPU/RAM report uses local embeddings via a wrapper (`takkub ccc serve`), so it is not representative of slim mode. [github](https://github.com/takkub/agent-takkub/issues/901)

- Pricing and terms can change. All figures are as fetched around Nov 1, 2026. The OpenAI token limit and dimensions come from a third-party listing.

- Main has unreleased changes, such as the home-directory guard. Behavior may differ from the README on GitHub.
