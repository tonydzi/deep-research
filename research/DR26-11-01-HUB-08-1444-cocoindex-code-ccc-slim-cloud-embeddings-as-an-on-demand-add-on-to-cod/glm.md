# DR26-11-01-HUB-08-1444 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Source: https://приватный чат
Mode: GLM5.3 Effort Max; sent once, stale Thinking recovered by one exact-own reload; full final report follows. Captured 2026-11-01T19:35:59.915603+00:00. Technical export, not substantive acceptance. Raw UI export preserves preceding visible progress narrative.

# DR26-11-01-HUB-08-1444 — CocoIndex Code (slim/cloud) additive to CodeIndexer: identity, dependencies, lifecycle, pricing, trial critique, adversarial decision
Research date: 2026-11-01 · Report delivered: in-chat Markdown only (no files/PDF) · Scope: CocoIndex Code only — not a seven-product ranking.
Decision under test: on-demand, additive CocoIndex Code slim (base) + cloud embeddings alongside the existing CodeIndexer for Claude/Codex/Cursor on Windows Hub, Intel Mac Pro, Apple Silicon Mac Studio — saving total local RAM/CPU/GPU while preserving correctness/freshness. Ordinary nonsecret code may go to cloud. No new plan/card/payments; no local embedding models; no permanent per-session stdio MCP or background watchers. Existing credits first; a chat subscription does not imply embeddings API credit.

Epistemic labels used throughout: [Measured] = read from an artifact I fetched (API/registry/source at a pinned ref) · [Implementation] = release-pinned source/README/docs of the product · [Marketing] = vendor promotional claim, not verified as measurement · [Secondary] = third-party page, vendor number not re-verified at primary · [Hypothesis] = reasoned, untested · [UNKNOWN] = could not establish; do not treat as zero or as fact.

Method & limits. Web-tool budget was capped (15 calls this session: one 5-query search batch + page fetches); I compensated with ~40 direct fetches of primary artifacts via curl at pinned refs: GitHub repo/releases/raw files/issues search API, PyPI JSON, Docker Hub API, and provider docs (Voyage, Google, Cohere, Anthropic, LiteLLM). No private connected email/calendar/Drive sources were used. Two pages are JS/Cloudflare-gated from this environment (OpenAI pricing, Voyage data-use policy) — those items are marked [Secondary]/[UNVERIFIED-PRIMARY]. Prior internal context is honored: the October broad survey (18 classes / 4 accepted vendors) is treated as historical only, and the 40-answer v2 comparison is treated as INVALID (serialized context exceeded the limit; zero successful structural-grep calls) and is not reused as a rating anywhere in this report.


## A. Exact product identity and released version
[Implementation] The product is cocoindex-io/cocoindex-code — "A super light-weight embedded code search engine CLI (AST based) that just works – improves speed and efficiency for coding agent" — Apache-2.0, CLI name ccc, usable as CLI, stdio MCP server (ccc mcp), or agent Skill ([https://github.com/cocoindex-io/cocoindex-code](https://github.com/cocoindex-io/cocoindex-code)). Repo stats at fetch (2026-11-01): ~2.9k stars, 244 forks, 262 commits, 49 tags, 9 branches; latest commit on main Oct 22, 2026 (b3834e6).

[Measured] Latest release, independently verified — the operator's 0.2.42/Oct 5/Apache-2.0/Python≥3.11 is correct:
 v0.2.42, released 2026-10-05 21:21–21:23 UTC by georgeh0, tag commit 6bb17d0, GPG-verified ([https://github.com/cocoindex-io/cocoindex-code/releases](https://github.com/cocoindex-io/cocoindex-code/releases)).
 Wheel cocoindex_code-0.2.42-py3-none-any.whl — 89.7 KB, sha256 6ad19ef0…7590, upload 2026-10-05T21:23:31Z; sdist 92.4 KB (release assets list, same URL).
 PyPI metadata for 0.2.42: requires_python: >=3.11, License Apache-2.0 ([https://pypi.org/pypi/cocoindex-code/0.2.42/json](https://pypi.org/pypi/cocoindex-code/0.2.42/json)).
 v0.2.42 contents: Elixir language support (#279), OMP hooks (#281), dep bumps incl. litellm→1.88.6 and sentence-transformers→5.6.0 (release notes).

[Measured] "slim" is an official base-install mode — not a package called cocoindex-code-slim. The release-pinned README says: "cocoindex-code (slim) — LiteLLM-only; requires a cloud embedding provider and API key. Use when you don't want the local-embedding deps (~1 GB of torch + transformers)" ([https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/v0.2.42/README.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/v0.2.42/README.md)). PyPI extras are only full, embeddings-local (both = cocoindex[sentence-transformers]>=1.0.17,<1.1.0), and dev ([https://pypi.org/pypi/cocoindex-code/0.2.42/json](https://pypi.org/pypi/cocoindex-code/0.2.42/json)). Docker tags mirror the names: cocoindex/cocoindex-code:latest (slim) and :full (README + [https://hub.docker.com/r/cocoindex/cocoindex-code/tags](https://hub.docker.com/r/cocoindex/cocoindex-code/tags)). No cocoindex-code-slim distribution exists; none was invented here.

[Implementation] Release vs main (do not conflate): main is ahead of v0.2.42 with unreleased fixes/features — daemon displaced-replacement socket fix #300 (merged Oct 6, after the Oct 5 release), MCP "codebase exploration tools" #308 (Oct 13), custom embedder provider #316 (Oct 22), Windows --path fixes #322 (Oct 26). Everything below is pinned to v0.2.42 unless stated.

[Implementation] ccc vs framework vs Plus vs namesakes:
 CocoIndex (framework) = cocoindex-io/cocoindex, Rust incremental data-transformation engine, ~11.8k stars, Apache-2.0; PyPI cocoindex; latest 1.1.0 released 2026-11-01; v0 docs in maintenance mode ([https://github.com/cocoindex-io/cocoindex](https://github.com/cocoindex-io/cocoindex), [https://cocoindex.io](https://cocoindex.io)). cocoindex-code is a consumer of it, pinned cocoindex[litellm]>=1.0.17,<1.1.0.
 CocoIndex Code Plus = separate self-hosted product (ccx CLI/MCP, symbol definitions/references, AST grep, K8s/Helm, SSO), docs published from a private product repo at cocoindex-io/cocoindex-code-plus-pub ([https://github.com/cocoindex-io/cocoindex-code-plus-pub](https://github.com/cocoindex-io/cocoindex-code-plus-pub)). Commercial; no public pricing. Not required for this decision.
 Namesakes to avoid confusing with ccc: (1) Zilliz claude-context (formerly "codeindexer"; TypeScript MCP @zilliz/claude-context-mcp, requires Zilliz Cloud/Milvus + OpenAI API key for embeddings) — [https://github.com/zilliztech/claude-context](https://github.com/zilliztech/claude-context); (2) CodeIndexer at [https://codeindexer.dev](https://codeindexer.dev) (NetTechnologys s.r.o.; Rust; local-only; call graphs) — the most likely identity of the operator's existing baseline (its FAQ lists Cursor, Claude Code, Claude Desktop, VS Code Copilot, Windsurf, OpenAI Codex CLI, Gemini/Antigravity); (3) PyPI claude-context is an unrelated tiny package. Only cocoindex-code uses the ccc command.


## B. Dependency graph, downloads, and the resource numbers
[Measured] Direct runtime dependencies of base (slim) cocoindex-code 0.2.42 (PyPI requires_dist): cocoindex[litellm]>=1.0.17,<1.1.0, mcp>=2.0.0,<3, sqlite-vec>=0.1.0, pydantic>=2, numpy>=1.24.0, einops>=0.8.2, typer>=0.9.0, msgspec>=0.19.0, pathspec>=0.12.1,<1.0, pyyaml>=6, questionary>=2. The [full]/[embeddings-local] extras add cocoindex[sentence-transformers] (→ sentence-transformers 5.6.0, transformers 5.10.1, torch 2.13.0 per the v0.2.42 lock bumps). The framework's base adds watchdog>=6, psutil>=7.2.1, click, rich, python-dotenv, typing-extensions ([https://pypi.org/pypi/cocoindex/json](https://pypi.org/pypi/cocoindex/json)).

[Measured] Native engine: the Rust engine arrives via the cocoindex wheel — cp311-abi3 wheels: win_amd64 9.7 MB, macosx_10_12_x86_64 (Intel) 9.3 MB, macosx_11_0_arm64 (Apple Silicon) 9.3 MB, manylinux x86_64 9.5 MB for 1.0.26 ([https://pypi.org/project/cocoindex/](https://pypi.org/project/cocoindex/)). All three operator hosts have wheels. Tree-sitter grammars are compiled into the engine (framework advertises "native Tree-sitter support"; no grammar download step exists in ccc; custom chunkers are user-supplied Python) — [Implementation] ([https://cocoindex.io/docs-v0/examples/code_index](https://cocoindex.io/docs-v0/examples/code_index), README).

[Implementation] Storage — SQLite + sqlite-vec + LMDB, not Postgres: "Embedded: Portable and just works, no database setup required!" Index DBs are cocoindex.db and target_sqlite.db in <project>/.cocoindex_code/; the engine state is LMDB with a default 4 GiB map ceiling (COCOINDEX_LMDB_MAP_SIZE, bytes, read at daemon start; grows as written); COCOINDEX_CODE_DB_PATH_MAPPING can relocate DBs (LMDB misbehaves on Docker bind mounts). v0.2.41 "reject NULL KNN distances" confirms sqlite-vec KNN. Postgres is a framework target, unused by ccc (README v0.2.42).

[Implementation] What still goes over the network with slim + cloud embeddings: (1) chunk text to your embedding provider via LiteLLM (that is the entire point — code content leaves the machine); (2) anonymous usage telemetry through CocoIndex ("we do not collect your source code, file paths, queries, search results, embeddings, settings"; opt-out COCOINDEX_DISABLE_USAGE_TRACKING=1); (3) PyPI/package traffic at install/upgrade. No model/tokenizer/grammar downloads — no HuggingFace fetch, no torch. With [full], the default model Snowflake/snowflake-arctic-embed-xs (22M params, 384 dims) downloads from HF on first use and the daemon then "holds the embedding model in RAM" (README).

[Measured vs Marketing] The 1 GB / 450 MB / 5 GB numbers:

Claim

 | Source

 | Reality check

 | 
"~1 GB of torch + transformers" for local deps | README v0.2.42 [Implementation] | Install-time dependency size for [full]; not RSS; slim avoids it entirely | 
Docker slim "~450 MB", full "~5 GB" | README [Marketing/Implementation-declared] | [Measured] Docker Hub API, tags pushed 2026-10-05: latest/v0.2.42 compressed 204.2 MB; full/v0.2.42-full compressed 3327.4 MB ([https://hub.docker.com/v2/repositories/cocoindex/cocoindex-code/tags](https://hub.docker.com/v2/repositories/cocoindex/cocoindex-code/tags)) — the README numbers plausibly describe uncompressed images | 
"Instant token saving by 70%", "<1s freshness after a save", "2M-file rebuild costs real money" | README / product page [Marketing] | No methodology published; "sub-second" assumes a refresh trigger (hook or next search), not a watcher (see D) | 
"zero local resource(s)" | Not found verbatim on official pages (closest: "zero-copy", "day zero") | Treat as operator-side paraphrase of slim marketing; the slim daemon does consume RAM/CPU — [UNKNOWN], no public numbers | 
RSS of slim+cloud daemon | none published | [UNKNOWN] — must be measured on-host; this is the resource-saving claim's critical unknown | 

  
[Measured] Supply-chain note for H: cocoindex v1.0.26 release notes state litellm 1.107.0 was published to PyPI with a credential stealer and backdoor and is excluded (litellm!=1.107.0,>=1.81.0 in 1.0.26 metadata; locked 1.96.2) ([https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.26](https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.26), [https://pypi.org/pypi/cocoindex/1.0.26/json](https://pypi.org/pypi/cocoindex/1.0.26/json)). Pinning matters.


## C. Pipeline: scanning, languages, chunking, query types
[Implementation] Scanning/ignores/limits (v0.2.42 README + src/cocoindex_code/settings.py at the tag): include/exclude glob patterns (DEFAULT_INCLUDED_PATTERNS ≈ 28+ types incl. .py/.pyi/.js/.jsx/.ts/.tsx/.mjs/.cjs/.rs/.go/.java/.c/.h/.cpp/…/.cs/.dart/.ex/.exs/.sql/.sh/.bash/.zsh/.md/.mdx/.txt/.rst/.php/.lua/.rb/.swift/.kt/.kts/.scala/…); default excludes **/.*, __pycache__, node_modules, dist, …; .gitignore honored (inverted-traversal fix #239 in v0.2.40); max_file_size (default no limit; accepts 500KB-style binary units; also applied by ccc grep); language_overrides (ext→lang); custom Python chunkers (module:function). Symlinks: engine v1.0.25 stops at symlink cycles instead of walking forever (#2456); other symlink semantics undocumented [UNKNOWN]. Binary-file detection beyond extension/size gating: undocumented [UNKNOWN].

[Implementation] Language support — Python/JS/TS real AST; PowerShell/shell fallback only. The v0.2.42 README table lists 33 languages (c, cpp, csharp, css, dart, dtd, elixir, fortran, go, html, java, javascript, json, kotlin, lua, markdown, pascal, php, python, r, ruby, rust, scala, solidity, sql, svelte, swift, toml, tsx, typescript, vue, xml, yaml). PowerShell (.ps1/.psm1) is absent from both the table and the default include patterns; shell (.sh/.bash/.zsh) is included by default but is not in the AST table. The product-page FAQ states: "Unknown extensions fall back to line-based chunking" ([https://cocoindex.io/cocoindex-code](https://cocoindex.io/cocoindex-code)). So: Python/JS/TS/TSX = tree-sitter AST chunks; PowerShell = at best line-based fallback after you add **/*.ps1 yourself; this matters for the Windows Hub tasks. (Features bullet lists "Shell" among supported languages — a marketing/implementation discrepancy worth noting.)

[Implementation] Chunk sizing: "Language-Aware Chunking targeting ~1,000 characters (~300 tokens)" per EMBEDDINGS.md ([https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md](https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md)); local models have 512-token contexts, cloud 8k–32k. Minimum chunk size and overlap: not documented [UNKNOWN]. Chunks are syntax units; AST chunk boundaries do not imply callers/references/flow — OSS ccc has no cross-file symbol/call graph (call graphs/blast radius are Plus/enterprise and framework marketing; the OSS MCP tool in v0.2.42 exposes exactly one tool, search).

[Implementation — with a release-timing caveat] Structural grep (ccc grep): by-example syntax-tree patterns (\NAME, \(ARGS*\)), fully local, no index/daemon/embeddings, honors project patterns + gitignore. The v0.2.42 README itself warns: "relies on cocoindex's structural code_match… Until it ships in a released cocoindex, run against a local cocoindex build." [Measured] cocoindex release notes show code_match first appearing in v1.0.26 (2026-10-22) — v1.0.25's notes (printed in full) contain no code_match, and v1.0.26 carries fix(code_match) #2525 plus "Nix syntax support for splitting and structural code patterns" #2434 ([https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.26](https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.26), …/v1.0.25). Consequence: a fresh pipx install 'cocoindex-code==0.2.42' today resolves cocoindex 1.0.26 (the pin <1.1.0 allows it), so ccc grep should work from PyPI now — but this post-dates both the release and the operator's failed v2 comparison (zero successful structural-grep calls in October). End-to-end behavior is [UNKNOWN] until canary-verified.

[Implementation] Query-type coverage in released v0.2.42 (relevant to the 24-task taxonomy):
 Known-symbol: partial — ccc grep 'def \NAME(\(ARGS*\)):' can find definitions by pattern; no symbol index.
 Exact: ccc grep structural match; no plain keyword/FTS mode (full-text search is an open request #192; hybrid/RRF #157 unreleased).
 Semantic: ccc search / MCP search(query, limit 1–100, offset, languages, paths, refresh_index=True default) returning path, language, code, line numbers, similarity score; --json output (v0.2.38 #216) for the trial's byte accounting.
 Navigation (defs/refs/callers): absent in OSS v0.2.42 — Plus ccx territory; "codebase exploration tools" only as unreleased PR #308.
 No-answer: no documented score threshold or abstention mode — it returns top-k scores; the agent decides. Design the no-answer tasks accordingly.
 RU/EN: no language handling in the tool; quality is entirely the embedding model's. Default local model is English-centric (arctic-embed-xs); documented multilingual options are local ibm-granite/granite-embedding-97m-multilingual-r2 (384d, project-scored 0.80) and cloud "Cohere Multilingual v3" (EMBEDDINGS.md), plus gemini-embedding-2 (100+ languages, Google) and voyage-multilingual-2. Zero repo issues mention Russian [Measured]; RU/EN retrieval quality on any cloud code embedder is [UNKNOWN] until the trial.


## D. Lifecycle: DB, daemon, keepalive, refresh, mutations, locks, rebuilds
[Implementation, source-pinned] Local DB & identity: per-project <project>/.cocoindex_code/ holds settings.yml, cocoindex.db (engine/LMDB state), target_sqlite.db (vectors). Project identity is project_root.resolve() — a filesystem path, no logical repo_id/revision. Therefore: duplicate checkouts = duplicate indexes and duplicate embedding spend; cross-OS root maps do not exist natively (COCOINDEX_CODE_HOST_PATH_MAPPING is a Docker display mapping only) — E:\ and /Users/… indexes are independent by construction, so never equate them. Global settings: ~/.cocoindex_code/global_settings.yml, relocatable via COCOINDEX_CODE_DIR.

[Implementation] Daemon & IPC, incl. Windows: background daemon auto-starts on first use (client spawns a detached process; start_new_session=True on Unix; creationflags=CREATE_NO_WINDOW (0x08000000) on win32); on Windows IPC uses named pipes (PipeListener/ConnectNamedPipe with explicit close-semantics handling), on Unix a domain socket (path length fix #267 in v0.2.40); per-request connections (~0.1 ms); version handshake transparently restarts an upgraded daemon; client crash-restarts a vanished daemon (#214). Sources: src/cocoindex_code/daemon.py, client.py at v0.2.42; design post: [https://cocoindex.io/blogs/building-an-invisible-daemon](https://cocoindex.io/blogs/building-an-invisible-daemon). Windows is a supported, exercised platform — but with open v0.2.42 bugs (below).

[Implementation — operator's finding verified] Idle/keepalive YAML: daemon.idle_timeout_minutes default 180 (0 = never), daemon.keep_alive_with_mcp default true (a live MCP session heartbeats to keep the daemon warm) — verified in both README v0.2.42 and settings.py (DaemonSettings defaults). History: idle timeout added v0.2.38 (#215, "stop holding the embedding model in RAM forever"); idle-exit-during-MCP option v0.2.40 (#268).

[Implementation] Refresh model (freshness is pull-based, not watched): ccc index (incremental, auto-inits), ccc search --refresh, and MCP search(refresh_index=true) by default. Optional editor hooks (Grok/Oh-My-Pi SessionStart+PostToolUse → ccc index; Claude plugin can load hooks but skill-only install is documented) — no background file watcher exists (#38 is an open feature request). The "<1s freshness" marketing therefore presumes a hook or the next search's refresh. The trial config (no global hooks/autostart) must state explicitly whether refresh happens via MCP-default auto-refresh or labeled manual ccc index.

[Implementation] Mutations & concurrency: changed files are re-chunked/re-embedded and deleted chunks retired (product page; engine memoization "pay once per chunk"); renames re-key by path (embedding memo softens cost). Interrupted updates: the engine is transactional (LMDB state + target reconcile; cocoindex docs note reconcile "may run more than once per commit") — ccc-specific crash recovery is undocumented [UNKNOWN]. Locks: per-project _index_lock in the daemon; multiple projects may index concurrently; on Apple Silicon, local GPU calls serialize through a single MPS subprocess (irrelevant for slim+cloud). Open race: concurrent CLI daemon start-up #243 (fix unreleased).

[Implementation] Model/dimension change → rebuild: "Switching models requires re-indexing (ccc reset && ccc index) since the vector dimensions differ." Stale-hit defenses: refresh-on-search (MCP default), settings-mtime staleness detection; env-var changes require a daemon restart (Windows bug #166: stale daemon can preserve a missing embeddings env — open in v0.2.42).

[Measured] Open v0.2.42 issues relevant to this decision ([https://github.com/cocoindex-io/cocoindex-code/issues](https://github.com/cocoindex-io/cocoindex-code/issues)):
 Windows Hub risk: #321 ccc search --path returns no results on Windows (Click glob expansion; open, 2026-10-25) with fix PR #322 open; #270 --path search can crash the daemon (open); #166 stale daemon env + GBK console crash (open).
 Resource risk: #273 an unsplittable long line can OOM the daemon (open) — cap chunk length unreleased; mitigate via max_file_size and excluding minified/generated files.
 Compat: #122 OpenAI-compatible endpoints / encoding_format (open).
 Post-release fixes on main only: #300 displaced-daemon socket deletion (merged Oct 6).


## E. Small, mature, documented configuration (release-pinned)
The documented surface is README + EMBEDDINGS.md at the tag (plus ccc doctor diagnostics) — small and adequate; there is no separate ccc docs site (the README's "Documentation" badge points to framework docs).

Exact install/config/operate sequence (all [Implementation] from v0.2.42 docs; commands for one host, repeat per host):

bash
   
 

 
# 1. Pinned isolated BASE (slim) install — no [full], no torch:
pipx install 'cocoindex-code==0.2.42'
# pin the engine to the release that carries code_match + litellm!=1.107.0 exclusion:
pipx runpip cocoindex-code install 'cocoindex==1.0.26'
# optional conservative litellm pin (version cocoindex 1.0.26 locks/tests):
pipx runpip cocoindex-code install 'litellm==1.96.2'

# 2. Isolated settings dir + telemetry off (shell env or daemon envs):
export COCOINDEX_CODE_DIR="/path/to/isolated/.cocoindex_code"   # global_settings.yml lives here
export COCOINDEX_DISABLE_USAGE_TRACKING=1                        # anonymous telemetry OFF
export VOYAGE_API_KEY=…                                          # never echoed/printed anywhere

# 3. ~/.cocoindex_code/global_settings.yml  (or $COCOINDEX_CODE_DIR/global_settings.yml):
# embedding:
#   provider: litellm
#   model: voyage/voyage-code-4          # code-optimized; ccc init auto-fills input_type doc/query
#   min_interval_ms: 300                 # pace requests (default 5 ms for LiteLLM)
# daemon:
#   idle_timeout_minutes: 180            # default; 0 = never
#   keep_alive_with_mcp: true            # default
# envs:
#   VOYAGE_API_KEY: …                    # only if not inherited from shell

# 4. Project COPY (never the production checkout):
cd /path/to/COPY && ccc init            # writes .cocoindex_code/settings.yml, updates .gitignore
ccc index                               # build (auto-inits; streaming progress)
ccc search "session management" --json  # measured, machine-readable
ccc search --refresh "…" --json         # labeled manual refresh
ccc status ; ccc doctor                 # chunk/file/language stats; both-side model check
ccc daemon stop                         # stop the owned daemon (rollback step)








 
 
Do not: install [full]/[embeddings-local], install global Claude/Grok/OMP plugins or hooks, enable autostart, or use the Docker home-mount flow for this trial (Docker mounts $HOME by default with an explicit security warning in the README — the native pinned install above is the constrained path).

Provider choice — established, low-cost, LiteLLM-compatible, RU/EN-capable or code-capable:

Provider/model

 | Price (primary)

 | Free allowance

 | Dims / max input

 | RU/EN

 | Notes

 | 
voyage-code-4 | $0.12/1M tokens | 200M tokens/account | 256–2048 / 32k | EN-code focus | Code-optimized; input_type auto-set by ccc ([https://docs.voyageai.com/docs/pricing](https://docs.voyageai.com/docs/pricing), fetched 2026-11-01) | 
voyage-4-lite | $0.02/1M | 200M | 256–2048 / 32k | EN | cheapest Voyage text model | 
gemini-embedding-001 | $0.15/1M paid | Free tier = $0 but "Used to improve our products: Yes" | 3072 / 8k | multilingual | paid tier: No improvement use ([https://ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing)) | 
gemini-embedding-2 | $0.20/1M text (batch $0.10) | same free-tier caveat | 2048 / multimodal, 100+ langs | multilingual | newest (same pricing page) | 
cohere/embed-multilingual-v3.0 | [Secondary] ~$0.10/1M | none found | 1024 / 512 ctx | 100+ langs incl. RU | specs primary ([https://docs.cohere.com/docs/models](https://docs.cohere.com/docs/models)); price unverified at primary | 
openai/text-embedding-3-small | [UNVERIFIED-PRIMARY] widely $0.02/1M | none | 1536 / 8191 | moderate | OpenAI pricing page Cloudflare-gated from this environment; verify before use | 

  
Recommendation [Hypothesis]: voyage/voyage-code-4 as primary canary embedder (code-optimized, 200M free tokens ⇒ trial-scale $0), with cohere/embed-multilingual-v3.0 as the RU/EN-strong alternative if Cohere credits exist. Critical caveats: (1) Credit eligibility is [UNKNOWN] — an API key is not paid permission; check existing keys/credits before anything. (2) Voyage data-use [Secondary]: hosted-API data may be stored/used for training by default; opt-out reportedly requires an org admin with a payment method on file — which collides with "no new card" and means ordinary (nonsecret) code could flow to a training-on pipeline unless the account was already opted out (primary page JS-gated: [https://docs.voyageai.com/docs/data-use](https://docs.voyageai.com/docs/data-use); corroborated [https://www.reviewramble.com/models/voyage-4-large](https://www.reviewramble.com/models/voyage-4-large)). Gemini's free tier has the same product-improvement flag; its paid tier requires billing. This single policy axis may decide the provider — verify before the canary.
Rates/limits: ccc batches up to 64 chunks/request, retries ≤6 with exponential backoff, min_interval_ms pacing (EMBEDDINGS.md); Voyage tier limits [Secondary] ~3M tokens/min & 2000 req/min at tier 1; Gemini free-tier embedding RPM lives behind AI Studio login [UNKNOWN]; batch-token ceilings per tier: T1 500K / T2 5M / T3 10M ([https://ai.google.dev/gemini-api/docs/rate-limits](https://ai.google.dev/gemini-api/docs/rate-limits)). No hidden generation/rerank: the released MCP tool surface is embeddings + vector search only — no LLM calls, no reranker (rerank/hybrid exist only as unreleased PRs #157/#182).


## F. Current pricing, terms, and the cost formula
All prices below were read from the provider's own pages on 2026-11-01 unless marked otherwise; no cost is invented — anything not verifiable is labeled.

[Measured] Voyage AI ([https://docs.voyageai.com/docs/pricing](https://docs.voyageai.com/docs/pricing)): text embeddings — first 200M tokens free for every account for voyage-4-large / voyage-4 / voyage-4-lite / voyage-context-4 / voyage-code-3 (the table also shows voyage-code-4 at 200M free; note the page's prose and "older models" table are internally inconsistent about voyage-code-3's free tokens — treat code-3 as possibly $0.18/1M with no free tokens, per the older-models table). Prices/1M: voyage-4-large $0.12 · voyage-4 $0.06 · voyage-4-lite $0.02 · voyage-code-4 $0.12 · voyage-context-4 $0.12 · voyage-multilingual-2 $0.12 (50M free) · older voyage-code-3 $0.18. Batch API −33% (no free tokens apply); Files API storage $0.05/GB/mo, 30-day retention. Free allowance wording is one-time ("first 200 million tokens … for every account"), not monthly; expiry policy [UNKNOWN].

[Measured] Google Gemini API ([https://ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing)): gemini-embedding-2 text input free tier $0 / paid $0.20 per 1M (batch $0.10); gemini-embedding-001 free tier $0 / paid $0.15 per 1M (batch $0.075). Free tier rows explicitly say "Used to improve our products: Yes" (paid: No). Rate limits ([https://ai.google.dev/gemini-api/docs/rate-limits](https://ai.google.dev/gemini-api/docs/rate-limits)): spend-based limits Tier 1 $10 / Tier 2 $50 / Tier 3 $200 per rolling 10 min; embedding batch enqueued tokens T1 500K / T2 5M / T3 10M; per-model free-tier RPM is shown in AI Studio (login) [UNKNOWN].

[Secondary] Cohere: embed specs verified (embed-v5.0-pro/fast 128k ctx, dims to 2048; embed-v4.0; embed-multilingual-v3.0 1024d/512ctx, 100+ languages) at [https://docs.cohere.com/docs/models](https://docs.cohere.com/docs/models); per-token prices not verifiable at primary from here — third parties list "cheapest Cohere embedding $0.10/1M" ([https://pricepertoken.com/embedding/author/cohere](https://pricepertoken.com/embedding/author/cohere)) and Embed 5 at $0.12 (Pro) / $0.08 (Fast) per 1M ([https://www.eesel.ai/blog/cohere-embed-5-pricing](https://www.eesel.ai/blog/cohere-embed-5-pricing), 2026-10-14). [UNVERIFIED-PRIMARY].

[UNVERIFIED-PRIMARY] OpenAI: text-embedding-3-small is widely listed at $0.02/1M input (stable since 2024), but platform.openai.com is Cloudflare-gated from this environment — verify on [https://platform.openai.com/docs/pricing](https://platform.openai.com/docs/pricing) before any use. OpenAI data-retention/no-training claims likewise not re-verified here.

[Measured] Anthropic: the Anthropic pricing page ([https://www.anthropic.com/pricing](https://www.anthropic.com/pricing), fetched 2026-11-01) contains zero embedding-model offerings — Anthropic sells no embeddings API at all, so a Claude subscription can never fund embeddings (and the operator's rule "chat subscription does not imply embeddings API credit" is, for Anthropic, a fortiori true).

Cost formula (state these assumptions in the trial record):

text
   
 

 
billed tokens = Σ(chunks × ~300 tok) initial          # chunk target ~1,000 chars ≈ 300 tok
             + Σ(changed chunks × ~300 tok) per refresh  # memoized: unchanged chunks cost 0
             + Σ(query tokens)                          # tiny (~tens)
             + retry re-bills (≤6 attempts, exponential backoff)
             + FULL REBUILD on model/dimension switch or ccc reset
             + client-context tokens are NOT provider-billed (separate, agent-side)








 
 
No monthly fee; overage = pay-per-token beyond the free allowance; no provider spend cap is documented — [UNKNOWN] whether hard caps can be set; billing observability exists only in provider dashboards (ccc reports chunk counts, not tokens/cost). Provider-billed receipts (operator's protocol) must come from the provider console.

Worked scenarios (explicit assumptions; computed, not vendor numbers): trial COPY ≈1,200 chunks ⇒ ~360K tokens ⇒ $0.00 on voyage-code-4/4-lite (within 200M free), ~$0.05 on gemini-embedding-001, ~$0.04 on Cohere multilingual [Secondary price]. Real repo ≈20k chunks (~6M tokens): $0 within Voyage free allowance; $0.90 (Gemini 001) / $1.20 (Gemini 2) / ~$0.60 (Cohere [Secondary]). ~500k chunks (150M tokens): still $0 within Voyage free; $22.50–$30 on Gemini. Marketing-scale 2M files (~1.5B tokens): $156 voyage-code-4 (after 200M free) / $26 voyage-4-lite / $225–$300 Gemini. When is CodeIndexer alone cheaper? (a) whenever the eligible-credit balance is zero or the free allowance is exhausted and no payment method may be added (CodeIndexer's local path is $0/API by design — "indexing, embedding, search, and audit all run on your machine", free tier "makes no network calls at all", [https://codeindexer.dev/faq](https://codeindexer.dev/faq)); (b) for secret/non-allowlisted code, always; (c) when rebuild frequency × corpus tokens × unit price exceeds the value of the RAM/CPU/GPU released (CodeIndexer FAQ admits "peaks around 1–2 GB RAM per project" — that is the resource the additive swap aims to release); (d) when RU/EN quality of the allowed cloud model underperforms the local baseline. At trial scale with Voyage free tokens, ccc is $0 in API cost — the comparison is then purely local-resource vs correctness/freshness, which is exactly what the trial measures.


## G. Critique of the preregistered trial (BEFORE experiments)
The protocol as specified is unusually strong: immutable allowlisted real snapshot with file hashes; logical repo_id/revision; NFC relative paths with explicit per-host root maps (correctly refusing E:\ ↔ /Users equivalence); 24 tasks = 4 × {known-symbol, exact, semanticRU, semanticEN, navigation, no-answer}; hidden oracle; independent clean-context answerers; blinded independent judge; same model/harness/budgets; baseline = CodeIndexer + same source-read fallback vs baseline + ccc; seeded AB/BA with 3 paired repetitions; 10 retrieval/read calls and 24,000 UTF-8 bytes per task counted after final serialization (JSON escapes, wrappers, errors, retries, hook echoes included), over-budget messages withheld whole; preserved delivered bytes/hash/order and real ccc successes/failures; zero-success ≠ benefit; 3 cold / 3 warm with 60 s idle before/after; tree peak/idle RSS/private bytes/CPU-seconds, total host load, GPU VRAM, disk, network, provider-billed receipts, UNKNOWN≠0; resource trials serialized by physical-host lease; COPY-only create/edit/rename/delete with t_mutation / index-observed / correct-retrieval at 5 s polls to 120 s; no stale delete hit at 120 s; manual refresh labeled; never restart production to manufacture cold; independent client must actually consume the result; hard gates (no secrets, identical corpus, complete context accounting, no wrong root, ≥90 % coverage, independent client, no stale delete at 120 s); KEEP thresholds (no new confidently false answer; correctness decline ≤1/24; AND [median walltime OR total CPU improves ≥20 %] OR [correctness improves ≥3/24 with median walltime increase ≤20 %]; added idle RSS ≤256 MiB/host, peak ≤1 GiB unless pretrial amendment); thresholds not refit after outcomes; CodeIndexer retained; expansion to other OS only after measured Hub benefit + independent review/readback. That is a sound skeleton. Gaps and risks to fix before running:

Cold/warm prebuild policy is underspecified — the biggest fairness risk. "Cold" must be defined identically per arm: fresh COPY with no prebuilt index for either arm (CodeIndexer's index and ccc's index both built inside the timed cold tasks), or both prebuilt and only warm measured. If ccc's MCP refresh_index=true fires inside a cold task, initial indexing latency lands inside the task budget while a prebuilt CodeIndexer pays nothing — that asymmetry can manufacture either result. Pre-register: cold = both indexes absent, first tool call pays build cost; warm = both indexes present.
Refresh labeling must cover the MCP default. With no hooks installed, ccc freshness comes from search(refresh_index=true) (auto, per call) or explicit ccc index (manual, no-op cost when unchanged). Log which occurred per task; the 120 s deletion gate should be tested through the same channel the answerers actually use.
Intervention is tool+model jointly. Baseline uses CodeIndexer's local embedder; ccc arm uses the cloud embedder. A semanticRU/EN difference may be model quality, not tool quality. Cannot decompose without a local-model ccc arm (forbidden by constraints) — state the confound explicitly in the write-up rather than pretending attribution to the tool.
Power & statistics. 24 tasks with ±1/24 ≈ 4.2 pp granularity: the ≥3/24 correctness threshold is detectable; a ≤1/24 decline gate is within noise of a coin flip — pre-commit to per-task paired scoring (sign/McNemar as descriptive only, given n=24) and to reporting per-category breakdowns, and keep the stated rule that operational thresholds are not statistical proof. For walltime ≥20 % with 3 cold + 3 warm observations per host: pre-specify median-of-repetitions, timeout handling (a timed-out task = failure, not exclusion), and that a single host's result does not transfer across hosts (Windows Hub ≠ Mac Studio).
Windows-specific pre-registration. v0.2.42 has open bugs #321/#322 (--path filters broken on Windows) and #270 (--path daemon crash). Either (a) exclude path-filtered queries on the Windows Hub arm (and say so), or (b) accept them knowing they may fail for tool-bug reasons — do not silently mix. GBK console crash #166: run answerers under UTF-8 code pages and record it.
Resource-gate mechanics. With slim+cloud, GPU VRAM should be ~0 — measure anyway to prove no MPS/CUDA path is touched when provider: litellm. Measure idle RSS in both keepalive states (keep_alive_with_mcp: true with a session connected, and disconnected-after-idle) — the 180-min default means a connected session holds the daemon warm indefinitely; the ≤256 MiB idle gate must say which state it gates. Peak ≤1 GiB during initial index: watch #273 (unsplittable-line OOM) — include a minified/generated file in the COPY deliberately (or exclude via max_file_size) and pre-register which.
Coverage gate (≥90 %). Define numerator/denominator before the run (e.g., share of corpus files represented in at least one successful retrieval across tasks, or share of tasks whose oracle target file was in the corpus hash list). Ambiguity here is refit-bait.
No-answer & confidently-false rubric. Pin the judge model/version and publish the rubric for "confidently false" (asserting a nonexistent symbol/file as fact with certainty) vs "unhelpful but hedged". Both arms will hedge differently.
Budget accounting details. 24,000 bytes after final serialization is right; add: count ccc --json payloads including similarity-score fields; count error text and retry transcripts; do not count operator-side orchestration logs (pre-register the boundary). ccc's 5-chunk × ~1,000-char default responses fit comfortably in 10 calls, but refresh-triggered index progress output (if streamed to the agent) counts too — capture with the same byte counter as delivered.
Preflight additions: secret-scan the allowlisted snapshot (hard gate "no secrets" needs a scanner run, not an assumption); hash the COPY after the trial (drift detection); record provider dashboard token deltas per phase vs the F-formula prediction (a ≥2× divergence falsifies the memoization/batching assumptions); fix COCOINDEX_CODE_DIR per arm; telemetry off (COCOINDEX_DISABLE_USAGE_TRACKING=1) so network accounting is clean; verify the Voyage training-opt-out / credit eligibility before any code leaves the machine.
Zero-success rule already internalized (v2 lesson) — extend it: zero successful semantic calls in the ccc arm also voids the comparison; report the failure mode (auth, 429, daemon, tool bug) verbatim.

With those amendments made before outcomes exist, the protocol can support the KEEP/PARK decision it claims to.


## H. Adversarial review — KEEP / TRY / PARK
Strongest FOR adding slim/cloud ccc:
Resource decoupling is structural, not tuned: slim pulls no torch/transformers ("~1 GB" avoided [Implementation]) and the daemon holds no local model when provider: litellm; the baseline's own FAQ concedes 1–2 GB RAM peaks per project ([https://codeindexer.dev/faq](https://codeindexer.dev/faq)) — the exact budget the operator wants back.
Money can be genuinely $0 at trial scale: voyage-code-4's 200M free tokens dwarfs a trial COPY's ~0.4M tokens [Measured pricing] — no new card required if an existing key/credits exist.
The integration surface is small and reversible: CLI + one stdio MCP tool + optional skill; per-project isolated SQLite/LMDB; explicit refresh; idle-exit 180 min with MCP keepalive true (verified); ccc daemon stop exists; Apache-2.0; wheels exist for all three hosts (win_amd64, Intel mac, arm64 mac) [Measured].
Incremental memoized re-embedding bounds ongoing spend; provider receipts can validate it.

Strongest AGAINST:
Benefit is currently unproven: the only prior comparison is INVALID (zero successful structural-grep calls), and no public slim+cloud RSS/CPU measurements exist — under the operator's own rule, resource/cost UNKNOWN blocks the resource-saving claim. Additive also means CodeIndexer keeps running: total local load = baseline + ccc daemon until KEEP.
Windows Hub is the weakest platform in the exact released version: #321/#322 (--path filter broken), #270 (--path daemon crash), #166 (stale env, GBK console) are all open in v0.2.42; the Hub is presumably where PowerShell matters most — and PowerShell has no AST support (fallback chunking only, not even indexed by default).
Privacy/policy axis can kill cloud embeddings: Voyage trains on hosted-API data by default with an opt-out that reportedly requires a payment method on file [Secondary]; Gemini's free tier is flagged "used to improve our products" [Measured]; OpenAI/Cohere policies unverified here. "Ordinary nonsecret may go to cloud" does not automatically mean "may be used for vendor training".
Structural grep only just became release-available (cocoindex 1.0.26, 2026-10-22) — after v0.2.42's README caveat and after the failed v2 comparison; end-to-end PyPI behavior unverified.
RU/EN semantic risk: code-optimized embedders are EN-centric; the multilingual cloud options are not code-specialized. semanticRU/semanticEN tasks may simply underperform the local baseline — which the trial would then honestly show as PARK evidence.
Youth/supply-chain: 0.2.x, fast-moving main with post-release fixes pending; the litellm 1.107.0 backdoor incident (excluded only from cocoindex ≥1.0.26) shows why the pin cocoindex==1.0.26 is mandatory.

Evidence matrix (release-pinned):

Claim

 | Status

 | Pinned source

 | 
v0.2.42, 2026-10-05, Apache-2.0, py≥3.11, 89.7 KB wheel | Verified [Measured] | [https://github.com/cocoindex-io/cocoindex-code/releases](https://github.com/cocoindex-io/cocoindex-code/releases) · [https://pypi.org/pypi/cocoindex-code/0.2.42/json](https://pypi.org/pypi/cocoindex-code/0.2.42/json) | 
slim = base install, LiteLLM-only; full/embeddings-local extras; no cocoindex-code-slim | Verified [Implementation] | [[email hidden]](mailto:[email hidden]): [https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/v0.2.42/README.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/v0.2.42/README.md) · [[email hidden]](mailto:[email hidden]) | 
Deps: cocoindex[litellm]≥1.0.17<1.1.0, mcp≥2<3, sqlite-vec, …; torch only via [full] | Verified [Measured] | PyPI requires_dist 0.2.42 | 
Engine wheels ~9.3–9.7 MB for all 3 host platforms | Verified [Measured] | [https://pypi.org/project/cocoindex/](https://pypi.org/project/cocoindex/) (1.0.26 files) | 
Docker slim/full actually 204.2 MB / 3,327.4 MB compressed (marketed ~450 MB / ~5 GB) | Verified [Measured] vs [Marketing] | [https://hub.docker.com/v2/repositories/cocoindex/cocoindex-code/tags](https://hub.docker.com/v2/repositories/cocoindex/cocoindex-code/tags) · README | 
SQLite+sqlite-vec+LMDB(4 GiB ceiling), no Postgres; per-path project identity | Verified [Implementation] | [[email hidden]](mailto:[email hidden]) · [[email hidden]](mailto:[email hidden]) | 
Daemon: named pipes on win32, socket on Unix; auto-start; version-handshake restart; per-project index lock | Verified [Implementation] | daemon.py/[[email hidden]](mailto:[email hidden]) · [https://cocoindex.io/blogs/building-an-invisible-daemon](https://cocoindex.io/blogs/building-an-invisible-daemon) | 
idle_timeout_minutes=180, keep_alive_with_mcp=true defaults | Verified [Implementation] | [[email hidden]](mailto:[email hidden]) · [[email hidden]](mailto:[email hidden]) (operator's finding confirmed) | 
No background watcher; refresh = CLI/--refresh/MCP default true; hooks optional | Verified [Implementation] | [[email hidden]](mailto:[email hidden]) · open #38 | 
33 AST languages; no PowerShell row; .sh included but not AST; unknown→line fallback | Verified [Implementation] | README [[email hidden]](mailto:[email hidden]) · settings.py · [https://cocoindex.io/cocoindex-code](https://cocoindex.io/cocoindex-code) | 
Chunk target ~1,000 chars (~300 tok); overlap/min-size undocumented | [Implementation] / [UNKNOWN] | [[email hidden]](mailto:[email hidden]) | 
ccc grep needs cocoindex code_match, first shipped v1.0.26 (2026-10-22) | Verified [Measured] | [[email hidden]](mailto:[email hidden]) note · [https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.26](https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.26) · …/v1.0.25 | 
Windows open bugs #321/#322/#270/#166; OOM risk #273 | Verified [Measured] | [https://github.com/cocoindex-io/cocoindex-code/issues/321](https://github.com/cocoindex-io/cocoindex-code/issues/321) (etc.) | 
Model switch ⇒ ccc reset && ccc index | Verified [Implementation] | [[email hidden]](mailto:[email hidden]) | 
Voyage $0.12/0.06/0.02, 200M free; Gemini $0.15/$0.20 + free-tier training flag; Anthropic has no embeddings API | Verified [Measured] (2026-11-01) | [https://docs.voyageai.com/docs/pricing](https://docs.voyageai.com/docs/pricing) · [https://ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing) · [https://www.anthropic.com/pricing](https://www.anthropic.com/pricing) | 
OpenAI & Cohere per-token prices; Voyage/Cohere data-use details | [Secondary]/[UNVERIFIED-PRIMARY] | [https://pricepertoken.com/embedding/author/cohere](https://pricepertoken.com/embedding/author/cohere) · [https://www.eesel.ai/blog/cohere-embed-5-pricing](https://www.eesel.ai/blog/cohere-embed-5-pricing) · [https://www.reviewramble.com/models/voyage-4-large](https://www.reviewramble.com/models/voyage-4-large) | 
Slim+cloud daemon RSS/CPU; RU/EN retrieval quality; ccc grep end-to-end on PyPI; credit eligibility | [UNKNOWN] — trial inputs | — | 

  
Missing evidence (must be produced before/within the canary): slim+cloud daemon idle & peak RSS/CPU on each host; ccc grep and semantic search end-to-end on v0.2.42+cocoindex 1.0.26; RU/EN retrieval quality on the chosen embedder; Windows --path behavior; existing provider credits/keys and their scopes; Voyage (or chosen provider) training-opt-out status without adding a payment method; primary-verified OpenAI/Cohere prices; Gemini free-tier embedding RPM; whether Voyage's 200M free tokens expire; ccc interrupted-update recovery; chunk min/overlap.

Disconfirming tests (cheap, in order, all before the 24-task trial):
Policy gate: confirm the chosen provider's data-use/training status for this account and that existing credits apply. If training-on-by-default and opt-out is impossible without a new card → cloud embeddings are non-compliant → PARK (not a tool failure).
Windows canary: on the Hub COPY, 10 searches — 5 plain, 5 --path-filtered. Path-filter failures (#321/#270) are expected; pre-register that path-filtered queries are excluded on Windows or PARK the Windows arm until a fixed release.
Resource probe: 60 s idle RSS and peak RSS during ccc index of the COPY. Idle >256 MiB or peak >1 GiB ⇒ resource-saving claim fails (no amendment, per the rule).
Structural sanity: ccc grep 'def \NAME(\(ARGS*\)):' over Python files — verifies code_match from PyPI; zero successes again ⇒ structural tasks are void, do not proceed to full trial.
RU probe: 5 RU queries vs oracle; <2/5 correct ⇒ pre-declare semanticRU likely to fail the ≥3/24 gate and decide whether the trial still runs (it should — negative results are results).
Deletion gate: delete a file; MCP search at 120 s; any stale hit ⇒ hard-gate failure.
Receipt check: provider token delta ≈ formula prediction (within ~2×); wild divergence falsifies memoization assumptions and inflates every cost projection.

Minimal canary & cost ceiling: one host (Windows Hub — the riskiest), one allowlisted COPY (≤ ~1.2k chunks ⇒ ~0.4M tokens), voyage-code-4 or the existing-credit provider; ceiling = existing confirmed credits only (with Voyage free tokens this is ~$0; otherwise ≤ the smallest existing credit balance, hard-stop at the provider dashboard). No new framework/guard is built — the preregistered protocol is the guard.

Rollback (stops only owned/isolated artifacts): ccc daemon stop (owned daemon) → unregister the temporary trial MCP registration in the trial client only → delete COPY/.cocoindex_code/ and the isolated COCOINDEX_CODE_DIR → pipx uninstall cocoindex-code. CodeIndexer, production configs, hooks, and all evidence files remain untouched. Install success / ACK / rc0 / a working snippet is not a fleet PASS.

Verdict: TRY — a time-boxed, credit-capped canary + the (amended) preregistered trial on the Windows Hub first. Not KEEP: nothing measured yet supports the resource-saving claim (v2 comparison invalid; no public slim+cloud measurements; Windows bugs open in the pinned release). Not PARK: constraints are satisfiable, the trial-scale cash cost is $0 within free tokens, the integration is small, reversible, and isolated, and the protocol to decide is already written. Expansion to Intel Mac Pro and Mac Studio only after measured Hub benefit plus independent review/readback; CodeIndexer is retained throughout regardless of outcome.


### Uncertainty register (top items, all explicitly NOT zero)
Slim+cloud daemon idle/peak RSS & CPU on all three hosts — UNKNOWN, trial input.
RU/EN semantic retrieval quality on voyage-code-4 / Cohere multilingual vs local baseline — UNKNOWN, trial input.
ccc grep end-to-end from PyPI (cocoindex 1.0.26) — probable but unverified.
Existing credits eligibility; Voyage/Cohere/OpenAI account-level data-use & training opt-out status without new payment methods — UNKNOWN / [Secondary], policy gate.
OpenAI & Cohere current per-token prices at primary — UNVERIFIED-PRIMARY.
Gemini free-tier embedding RPM; Voyage free-token expiry — UNKNOWN.
ccc behavior on interrupted updates; chunk min-size/overlap; symlink/binary handling details — UNKNOWN/partial.
"Zero local resources"-style marketing phrasing — not found verbatim in official sources; do not quote as vendor claim.

Historical-context handling: DR18's 18-class/4-vendor October survey = historical input only; the 40-answer v2 comparison = INVALID (context-limit serialization failure; zero successful structural-grep calls) and is excluded from every rating above. Zero-success treatment rule applied prospectively as specified.

