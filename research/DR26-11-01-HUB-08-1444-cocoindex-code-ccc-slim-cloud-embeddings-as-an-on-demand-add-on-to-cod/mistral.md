# DR26-11-01-HUB-08-1444 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Source: https://chat.mistral.ai/work/8340c5d6-102e-46fe-9f39-a5eb864033ca
Mode: Think + deep-research; own browser report; captured 2026-11-01T19:01Z. Technical export, not substantive acceptance.


# DR26-11-01-HUB-08-1444 — CocoIndex Code (ccc) Additive Trial Brief: Slim/Cloud Embeddings Alongside CodeIndexer
Research date: 2026-11-01 · Scope: on-demand additive CocoIndex Code (not a multi-product ranking) for Claude/Codex/Cursor on Windows Hub, Intel Mac Pro, Apple Silicon Mac Studio · Decision: add or not, with total local RAM/CPU/GPU savings while preserving correctness/freshness · Constraints honored: existing credits first, no new plan/card/payments, no local embedding models, no permanent per-session stdio MCP, no background watchers, ordinary nonsecret code may go to cloud · DR18 (Oct, 18 classes/4 accepted vendors) treated as historical; the "its40" v2 comparison is INVALID (serialized context exceeded limit, zero successful mgrep calls) and is not reused as a rating anywhere below.

## Question
Can cocoindex-code (the ccc CLI) in its slim + cloud-embeddings configuration be added on-demand alongside the existing CodeIndexer such that total local RAM/CPU/GPU use falls and answer correctness/freshness is preserved — and what exact, mature, documented configuration, cost model, preregistered trial, and adversarial evidence gate the decision?

## Executive Summary (ranked)
Product identity verified, operator's findings confirmed. cocoindex-io/cocoindex-code is the official product; ccc is its CLI; latest release is v0.2.42, Oct 5, 2026 (commit 6bb17d0, signed), Apache-2.0, Python ≥3.11 (classifiers 3.11–3.13, Development Status Alpha) ([ release v0.2.42](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42), [ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml), [ PyPI](https://pypi.org/project/cocoindex-code/)).
Slim is official and is exactly what this decision needs: the plain base package (no invented cocoindex-code-slim package exists — do not use that name) is LiteLLM-only with no torch/transformers; [full]/[embeddings-local] extras add sentence-transformers (~1 GB of torch+transformers per the README) ([ README](https://github.com/cocoindex-io/cocoindex-code), [ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)).
The "zero local resource" claim is marketing, and Windows is the disconfirmed risk zone. Independent third-party reports (Windows 11, v0.2.42, measured 2026-10-23) show the daemon burning ~62% CPU sustained and ~1.8–1.9 GB RAM, in one case even with no index for the active project ([ agent-takkub #901](https://github.com/takkub/agent-takkub/issues/901), [ agent-takkub #904](https://github.com/takkub/agent-takkub/issues/904)). These were full/local-embedding setups wired with auto-indexing, not slim+cloud — but they directly attack the resource-saving premise on the Windows Hub and make idle-RSS a hard trial gate.
The 70% token-saving claim does not survive independent scrutiny: a public independent benchmark found no tool in the category saved 60% (full-session figures: 31.6% on Codex, 15.9% on Claude Code), and one cocoindex instance returned no answer at all (n=41) ([ r/LLMDevs benchmark](https://www.reddit.com/r/LLMDevs/comments/1wp1szf/i_benchmarked_repowise_codegraph_serena_graphify/)). The vendor's own evidence is a single anecdote (1.8K→~650 tokens, 54s→34s, one security-audit task) ([ Towards AI vendor article](https://pub.towardsai.net/i-built-an-ast-powered-code-mcp-that-saves-70-tokens-heres-how-it-works-3dbe58746729?gi=5cd1236a2789)).
Cost is near-zero for a trial if existing credits/allowances apply: Voyage's own pricing page grants 200M free tokens per account (prose lists voyage-code-3; the table lists voyage-code-4 at $0.12/1M after free) ([ Voyage pricing](https://docs.voyageai.com/docs/pricing)); OpenAI text-embedding-3-small is $0.02/1M tokens ([ OpenRouter](https://openrouter.ai/openai/text-embedding-3-small), [ CostGoat Oct 2026](https://costgoat.com/pricing/openai-embeddings)); Gemini embedding-2 is free on the free tier but free-tier content is used to improve Google products ([ EmbeddingCost, checked 16 Oct 2026](https://embeddingcost.com/google)). Chat subscription credits ≠ embeddings API credit — eligibility remains UNKNOWN until checked in each provider dashboard.
Correctness/freshness machinery is real but incomplete for this use case: PowerShell is only separator-based (not AST) chunking ([ Text ops docs](https://cocoindex.io/docs/ops/text/), [ supported-languages snippet](https://pub.towardsai.net/i-built-an-ast-powered-code-mcp-that-saves-70-tokens-heres-how-it-works-3dbe58746729?gi=5cd1236a2789)); structural grep (ccc grep) depends on a framework feature whose released status is contradicted between the README and the framework docs; there is no navigation/call-graph capability in the OSS CLI, and no built-in no-answer signal.
Verdict: TRY (conditional minimal canary), not KEEP. No valid CodeIndexer-vs-ccc comparison exists (v2 invalid), the resource-saving claim is unmeasured and partially disconfirmed, and two daemon bugs (socket deletion on stale-daemon shutdown [#288], displaced daemons [#243]) are not both fixed in the released v0.2.42. KEEP only if the preregistered gates in §G pass.

## Methodology
Sources: official repo/README (main, head b3834e6, Oct 22, 2026), release tag v0.2.42, pyproject.toml, EMBEDDINGS.md, PyPI pages (cocoindex-code, cocoindex 1.0.x wheels, namesake cocoindex-code-mcp-server), framework docs (text ops, telemetry), framework release v1.0.25, issue/PR tracker (#288, #291, #243, cocoindex #2456), the product marketing page, and provider pricing/policy pages (Voyage, OpenAI, Google). ~33 searches/inspections total.
Third-party evidence: independent Reddit benchmark with public raw data (competitor-authored — weighted accordingly), two Windows resource-burn issue reports, vendor blog posts labeled as marketing.
Limitations: no local execution or measurement was performed; all resource figures other than the cited third-party reports are UNKNOWN, not 0. Vendor RSS/CPU measurements are not published. Internal v2 trial data is invalid and excluded. DR18 vendor classes are historical context only. No private connected sources were used.

## Findings

### A. Exact product identity and current release
Measured/implementation facts (from primary sources):
Official repo: cocoindex-io/cocoindex-code — "AST-based semantic code search", CLI (ccc) + MCP server + Claude Code/Grok/Oh My Pi plugin marketplace ([ README](https://github.com/cocoindex-io/cocoindex-code)). Apache-2.0 (license = "Apache-2.0"; LICENSE created Feb 17, 2026) ([ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)).
Latest release v0.2.42, released Oct 5, 2026, 21:21 UTC by georgeh0, commit 6bb17d0, GPG-signed, 9 commits to main since; main head Oct 22, 2026; 262 commits, 49 tags ([ release v0.2.42](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42), [ repo](https://github.com/cocoindex-io/cocoindex-code)). PyPI shows 0.2.42 latest; package history spans 0.1.x (Feb 2026) → 0.2.42 ([ PyPI 0.1.5 page](https://pypi.org/project/cocoindex-code/0.1.5/), [ piwheels: 46 versions](https://www.piwheels.org/project/cocoindex-code/)). Operator's 0.2.42 / Oct 5 / Apache-2.0 / Python ≥3.11 finding independently verified.
Entry points: ccc and cocoindex-code (backward-compatible MCP alias) ([ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)).
Slim is an official install style, not a separate package: "cocoindex-code (slim) — LiteLLM-only; requires a cloud embedding provider and API key. Use when you don't want the local-embedding deps (~1 GB of torch + transformers)" ([ README](https://github.com/cocoindex-io/cocoindex-code)). Extras: embeddings-local and umbrella full — both identical today (cocoindex[sentence-transformers]) ([ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)). Docker variants :latest (slim, ~450 MB) and :full (~5 GB) mirror these ([ README](https://github.com/cocoindex-io/cocoindex-code)).
ccc vs framework: the product depends on the separate framework cocoindex (repo cocoindex-io/cocoindex, PyPI cocoindex, current 1.0.25 / 1.1.0) — ccc pins cocoindex[litellm]>=1.0.17,<1.1.0 ([ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml), [ PyPI cocoindex](https://pypi.org/project/cocoindex/), [ framework releases](https://github.com/cocoindex-io/cocoindex/releases)). CocoIndex Code is the packaging of the earlier realtime-codebase-indexing example pipeline ([ realtime-codebase-indexing](https://github.com/cocoindex-io/realtime-codebase-indexing)).
Namesakes to avoid: many same-named GitHub forks (harmless mirrors), and a different PyPI package cocoindex-code-mcp-server (v0.2.0) that is not the official tool ([ PyPI namesake](https://pypi.org/project/cocoindex-code-mcp-server/)). Some forks carry an outdated "saves 70% token" tagline.
Release vs main: on main but not in v0.2.42 — the displaced-daemon socket-deletion fix (Oct 6, 2026) ([ repo file list](https://github.com/cocoindex-io/cocoindex-code)). The marketing page's shared-daemon, VPC, branch-overlay enterprise story is enterprise/marketing, not in the OSS CLI release ([ marketing page](https://cocoindex.io/cocoindex-code/)).

### B. Dependency graph — base (slim) vs full/embeddings-local
Base (slim) direct dependencies: mcp>=2.0.0,<3, cocoindex[litellm]>=1.0.17,<1.1.0 (pulls LiteLLM), sqlite-vec>=0.1.0, pydantic>=2, numpy>=1.24, einops, typer, msgspec, pathspec, pyyaml, questionary. No torch, no transformers ([ pyproject.toml](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)).
Rust engine: the cocoindex framework ships a native Rust extension as platform wheels — manylinux_2_28 x86_64/aarch64, win_amd64 (including cp314 free-threaded), abi3, plus sdist ([ PyPI cocoindex 1.0.21 files](https://pypi.org/project/cocoindex/1.0.21/)). Building from source on Windows is known to be painful ("this Windows checkout lacks the built Rust extension") ([ cocoindex PR #2456](https://github.com/cocoindex-io/cocoindex/pull/2456)) — use wheels.
Storage — SQLite/sqlite-vec, not Postgres: sqlite-vec is a base dependency; the index DBs are cocoindex.db and target_sqlite.db under <project>/.cocoindex_code/; engine state lives in LMDB with a default 4 GiB map ceiling fixed at daemon start, raisable via COCOINDEX_LMDB_MAP_SIZE (manual step "until cocoindex#2108 lands"; LMDB works poorly on mounted volumes) ([ README snippets](https://github.com/cocoindex-io/cocoindex-code), [ maintainer: "the new one no longer use postgres! it is embedded"](https://www.reddit.com/r/opencodeCLI/comments/1rajn6x/cocoindexcode_super_light_weight_mcp_that/)). Framework required deps include numpy, msgspec, watchdog, psutil, rich, click ([ pypistats](https://pypistats.org/packages/cocoindex)).
Full extra at v0.2.42 pins: sentence-transformers 5.6.0, transformers 5.10.1, litellm 1.88.6 ([ release v0.2.42](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42)).
What still downloads/uses network with cloud (slim): (1) pip/uv wheel download at install (native cocoindex wheel + sqlite-vec wheel); (2) embeddings API calls to the provider — batches up to 64 chunks per request, min_interval_ms default 5 ms for LiteLLM, automatic retries with exponential backoff up to 6 times on 429 ([ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)); (3) anonymous usage telemetry via CocoIndex to the Scarf gateway at four lifecycle points, tagged cocoindex-code — opt out with COCOINDEX_DISABLE_USAGE_TRACKING=1 ([ telemetry docs](https://cocoindex.io/docs/about/telemetry/), [ README snippet](https://github.com/cocoindex-io/cocoindex-code)). No model/tokenizer download occurs in slim — inference runs provider-side; tree-sitter grammars are compiled into the native extension (inference from wheel-shipped grammar support, not separately downloaded).
Marketing figures vs measurement — keep them separate:"~1 GB of torch + transformers" = install-size claim for the full extra ([ README](https://github.com/cocoindex-io/cocoindex-code)).
"~450 MB / ~5 GB" = Docker image sizes for slim/full ([ README](https://github.com/cocoindex-io/cocoindex-code)) — compressed vs uncompressed is not stated.
"Cloud LiteLLM Remote … zero local resource usage" = marketing ([ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)) — slim still runs a Python daemon, the Rust engine, tree-sitter parsing, LMDB and sqlite-vec writes; RSS/CPU for slim+cloud are UNKNOWN, not 0, and must be measured in the trial. No official RSS/inference measurements are published anywhere I could find.


### C. Pipeline — scanning, languages, chunking, query semantics
File matching: per-project settings.yml with include_patterns (defaults for "28+ file types"), exclude_patterns (hidden dirs, __pycache__, node_modules, dist, …), max_file_size (default: no limit; example 500KB), language_overrides, custom Python chunkers; honors .gitignore; .cocoindex_code/ auto-added to .gitignore at init ([ README](https://github.com/cocoindex-io/cocoindex-code)). Binary-file detection is not documented — UNKNOWN; rely on ccc doctor file-match preview and excludes. Symlinks: the framework's walker historically follows symlinked directories with a cycle-hang risk; a fix tracking (st_dev, st_ino) ancestors is in PR, released status unverified ([ cocoindex PR #2456](https://github.com/cocoindex-io/cocoindex/pull/2456)).
Languages: Python/JS/TS (and .pyi/.pyw/.pyx, .mjs/.cjs/.jsx, .tsx, etc.) are tree-sitter AST-aware, along with ~35 syntax-aware languages (C/C++/C#/Go/Rust/Java/SQL/bash/Markdown/TOML/JSON…) ([ Text ops docs](https://cocoindex.io/docs/ops/text/)). PowerShell is NOT AST-aware: it is among "110 additional languages [that] use separator-based splitting (e.g. … perl, powershell, proto, zig…)" ([ Text ops docs snippet](https://pub.towardsai.net/i-built-an-ast-powered-code-mcp-that-saves-70-tokens-heres-how-it-works-3dbe58746729?gi=5cd1236a2789)); unknown extensions fall back to plain-text/line chunking ([ marketing FAQ](https://cocoindex.io/cocoindex-code/)). Whether .ps1 is in ccc's default include list is UNKNOWN — add "**/*.ps1" explicitly and verify with ccc doctor.
Chunk sizes: syntax-aware RecursiveSplitter with chunk_size=1000, min_chunk_size=300, chunk_overlap=300 (framework documented example); ccc "targets a chunk size of ~1,000 characters (~300 tokens)" at logical boundaries ([ Text ops docs](https://cocoindex.io/docs/ops/text/), [ index-codebase example](https://cocoindex.io/docs/examples/index-codebase/), [ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)).
Structural grep (ccc grep): released in ccc (#200) ([ releases listing](https://github.com/cocoindex-io/cocoindex-code/releases)), runs locally with no index/daemon/embeddings; but the README — both at v0.2.42 and on main as of Oct 6, 2026 — says it "relies on cocoindex's structural code_match feature. Until it ships in a released cocoindex, run against a local cocoindex build" ([ README](https://github.com/cocoindex-io/cocoindex-code)), while framework docs (v1.0.14, reviewed Jul 4, 2026) already document CodePattern/CodeSource ([ Text ops docs](https://cocoindex.io/docs/ops/text/)). CONFLICT — verify with a smoke test before counting ccc grep as an available tool.
Search semantics: one MCP tool search(query, limit 1–100 default 5, offset, refresh_index=True by default, languages, paths) returning chunks with path, language, content, line numbers, similarity score ([ README](https://github.com/cocoindex-io/cocoindex-code)). Implications:Known-symbol/exact: no dedicated symbol index; structural grep is the intended tool (see released-status conflict above); literal search stays with existing ripgrep/CodeIndexer.
Semantic: the core capability; RU/EN quality is model-bound. Default local models are English/code-centric (arctic-embed-xs 22M/384-d, MTEB code score 0.67); the documented multilingual local option is granite-embedding-97m-multilingual-r2 (0.80), and cloud multilingual options are Cohere multilingual v3 / Gemini ([ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)). Inference (labeled as such): RU natural-language queries against English-trained models will likely degrade; the trial's semantic-RU tasks will measure this.
Navigation: no callers/references/control-flow capability — AST chunk boundaries make chunks coherent retrieval units, nothing more; blast-radius/call-graph claims on the marketing page belong to the enterprise story, not this CLI ([ marketing page](https://cocoindex.io/cocoindex-code/)).
No-answer: none built in — top-k chunks with scores are always returned; the consuming agent must judge. This makes the trial's no-answer category a genuine test of "no new confidently false answer."


### D. Lifecycle — DB, identity, daemon, freshness, mutation
Identity & paths: project = directory containing .cocoindex_code/settings.yml; index DBs default to the same folder; COCOINDEX_CODE_DIR relocates global settings; COCOINDEX_CODE_DB_PATH_MAPPING=src=dst relocates DBs, both sides absolute, per-host ([ README](https://github.com/cocoindex-io/cocoindex-code), [ PyPI snippet](https://pypi.org/project/cocoindex-code/)). No git repo+revision identity in the OSS CLI — it indexes the working tree as of the last refresh; commit/branch lineage is enterprise marketing ([ marketing page](https://cocoindex.io/cocoindex-code/)). Duplicate checkouts are separate projects (duplicate embeddings and cost). Cross-OS root maps: the mapping is per-host absolute paths — never equate E:\ and /Users/...; exact internal path representation is UNKNOWN and must be verified empirically before any cross-OS DB sharing (recommend: don't share DBs across OS; re-index per host).
Daemon: starts automatically on first ccc use; holds the embedding model in RAM (full) and serves all clients; ccc daemon status/restart/stop; idle_timeout_minutes default 180, keep_alive_with_mcp default true (a live MCP session sends periodic heartbeats keeping the daemon warm) — operator's findings verified ([ README](https://github.com/cocoindex-io/cocoindex-code)). IPC via socket file (daemon.sock) + pid file under ~/.cocoindex_code/; it runs on Windows (ccc.exe run-daemon observed on Windows 11) ([ agent-takkub #901](https://github.com/takkub/agent-takkub/issues/901)).
Daemon defects relevant to the trial: a stale/displaced daemon unlinks the live daemon's socket on shutdown, leaving the live daemon unreachable — present in 0.2.32, fix merged on main Oct 6, not in v0.2.42 ([ Issue #288](https://github.com/cocoindex-io/cocoindex-code/issues/288), [ PR #291](https://github.com/cocoindex-io/cocoindex-code/pull/291)); earlier, eleven idle daemon processes accumulated (0.2.32, macOS) ([ PR #243](https://github.com/cocoindex-io/cocoindex-code/pull/243)). Count daemon processes as a trial health metric.
Freshness: no filesystem watcher in the core CLI. Refresh happens via ccc index, ccc search --refresh, MCP search(refresh_index=True) (default), or plugin hooks (SessionStart/PostToolUse → incremental ccc index) — the hooks are exactly the "background watcher" behavior this decision excludes; a skill-only or plain-CLI setup avoids them, and Grok/OMP plugin docs show how to disable hooks/MCP ([ README](https://github.com/cocoindex-io/cocoindex-code)). Live DB sync risk (external process writing while daemon holds LMDB) is not documented — treat as UNKNOWN; do not point two daemons at one project dir.
Mutations & rebuilds: create/edit/rename/delete are picked up at the next refresh incrementally; deleted chunks retire ([ marketing page](https://cocoindex.io/cocoindex-code/)). Interrupted-update and lock behavior: not documented — UNKNOWN; test with a killed mid-index run. Concurrency: multiple projects may prepare indexes concurrently (GPU serialized through one MPS subprocess on full) ([ README](https://github.com/cocoindex-io/cocoindex-code)); slim concurrency behavior UNKNOWN.
Model/dimension changes: switching embedding models requires ccc reset && ccc index (dimensions differ); include/exclude/max_file_size changes need only re-index, no reset; chunker changes need a daemon restart and re-chunk (cached embeddings reused for unchanged chunk text) ([ README](https://github.com/cocoindex-io/cocoindex-code)). ccc doctor checks settings, daemon, model (indexing and query sides separately), file matching, index health; ccc reset deletes index DBs, --all also deletes settings ([ README](https://github.com/cocoindex-io/cocoindex-code)).

### E. Small mature documented config (slim + remote embedder, isolated)
Install (pinned, isolated, native — no Docker, no home mount, no plugin, no hooks):
bash



```
# isolated pinned BASE install (slim; NO [full], no torch)
pipx install 'cocoindex-code==0.2.42'
# isolated settings dir (never ~/.cocoindex_code of daily accounts)
export COCOINDEX_CODE_DIR=/vol/ccc-trial/.cocoindex_code
# telemetry opt-out
export COCOINDEX_DISABLE_USAGE_TRACKING=1
```





$COCOINDEX_CODE_DIR/global_settings.yml:
yaml



```
embedding:
  provider: litellm
  model: voyage/voyage-code-3      # or text-embedding-3-small / gemini/gemini-embedding-001
  min_interval_ms: 300            # pace requests; LiteLLM default is 5
  indexing_params:
    input_type: document           # auto-populated by ccc init for known models
  query_params:
    input_type: query
envs:
  VOYAGE_API_KEY: <from existing account env — never echo, never commit>
daemon:
  idle_timeout_minutes: 180        # default; 0 = never
  keep_alive_with_mcp: true        # irrelevant for CLI-only use; set false if MCP ever used
```





Project (a COPY, never production): ccc init (or go straight to ccc index, which auto-inits) → edit <copy>/.cocoindex_code/settings.yml to pin include_patterns (**/*.py, **/*.js, **/*.ts, **/*.tsx, **/*.ps1), exclude_patterns, max_file_size: 500KB → ccc doctor (preview matches + both model checks) → ccc index → ccc search --refresh "<query>" → ccc daemon stop after use; rollback = ccc daemon stop && ccc reset --all in the copy + pipx uninstall cocoindex-code ([ README](https://github.com/cocoindex-io/cocoindex-code), [ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)).
Provider shortlist (established, LiteLLM-compatible, current):





OpenAI text-embedding-3-small

$0.02 (Batch $0.01)

$5 credits for new API users (3rd-party)

8,192-token input cap; no training on API data by default; abuse-monitoring logs ≤30 days ([ OpenRouter](https://openrouter.ai/openai/text-embedding-3-small), [ CostGoat Oct 2026](https://costgoat.com/pricing/openai-embeddings), [ OpenAI data controls](https://developers.openai.com/api/docs/guides/your-data))


Voyage voyage-code-3 (32k ctx)

$0.18 per third-party trackers

200M tokens free per account (prose includes voyage-code-3; table lists voyage-code-4 at $0.12/1M)

Default policy stores data and may train unless opted out; zero-day retention after opt-out requires an org admin with a payment method — conflicts with the no-new-card constraint ([ Voyage pricing](https://docs.voyageai.com/docs/pricing), [ Voyage FAQ](https://docs.voyageai.com/docs/faq), [ usagepricing 2026-09-28](https://www.usagepricing.com/blueprint/voyage-ai), [ FutureAGI verified Aug 6, 2026](https://futureagi.com/llm-cost-calculator/voyage-ai/voyage-code-3/))


Google gemini-embedding-2 / -001

$0.20 ($0.10 Batch) / $0.15

Free tier includes it

Free-tier content is used to improve Google products; paid tier is not ([ Google blog GA](https://pirouter.ai/blog/does-your-llm-provider-train-on-api-data), [ EmbeddingCost checked 16 Oct 2026](https://embeddingcost.com/google), [ Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing))


Cohere embed-v4.0 (multilingual)

not verified this pass (−)

−

ccc auto-populates its input_type params ([ README](https://github.com/cocoindex-io/cocoindex-code))







No generation or rerank calls are involved — ccc issues embeddings only; anything else would show in provider usage dashboards, which are the billing observability (ccc emits no cost receipts itself). Credit eligibility of any existing chat subscription for embeddings APIs is UNKNOWN; an API key is not paid permission — check each dashboard before the trial. Never display credentials; keys live in the daemon environment only.

### F. Cost model, terms, and when CodeIndexer alone is cheaper
Formula (assumptions explicit; not a measured cost):
cost ≈ (T_index + Σ T_changed-per-refresh + N_queries × T_query + T_retries) × price_per_1M
T_index ≈ all chunk text at ~1,000 chars ≈ 250–300 tokens/chunk (illustrative: a ~5M-token corpus → $0.10 at OpenAI 3-small; $0.90 at voyage-code-3 list price, or $0 within the 200M free allowance) ([ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md), [ Voyage pricing](https://docs.voyageai.com/docs/pricing)).
Incremental refresh re-embeds only changed chunks; embeddings are memoized per chunk content ("pay once per chunk") — but a model/dimension switch forces a full re-embed (ccc reset && ccc index) ([ README](https://github.com/cocoindex-io/cocoindex-code), [ marketing page](https://cocoindex.io/cocoindex-code/)).
Queries: one short query embedding per search call (negligible); retries ≤6 on 429; batching 64 chunks/request ([ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)).
Client-context cost: returned chunks ~300 tokens × limit (default 5) per search — this is where the (unproven) 70% saving claim would come from.
Free-vs-trial distinctions: Voyage's 200M tokens are a per-account allowance (not expiring trial credits) ([ Voyage pricing](https://docs.voyageai.com/docs/pricing)); OpenAI's $5 is one-time new-user credit ([ CostGoat](https://costgoat.com/pricing/openai-embeddings)); Gemini's free tier is ongoing but rate-limited and its data-use clause applies ([ EmbeddingCost](https://embeddingcost.com/google)). Overages bill per token to the linked account; no invented actual cost is stated here — the trial records provider-billed receipts instead.
Data transmission/retention/training: OpenAI — no training by default, ≤30-day abuse-monitoring retention, ZDR by approval ([ data controls](https://developers.openai.com/api/docs/guides/your-data)); Voyage — opt-out required, zero-day retention after opt-out ([ FAQ](https://docs.voyageai.com/docs/faq)); Google — free tier improves products, paid tier does not ([ EmbeddingCost](https://embeddingcost.com/google)). Given the "ordinary nonsecret code may go to cloud" policy, all three are policy-compatible; prefer OpenAI (no-training-by-default) if any corpus chunk could drift sensitive, and set COCOINDEX_DISABLE_USAGE_TRACKING=1 regardless ([ telemetry docs](https://cocoindex.io/docs/about/telemetry/)).
When CodeIndexer alone is cheaper: (a) tasks dominated by known-symbol/exact lookups that CodeIndexer already serves (ccc adds no symbol index; ccc grep released status uncertain); (b) small corpora where direct file reads fit the byte budget without retrieval tooling; (c) any constraint requiring zero network spend (then only the local-model path exists, which this decision excludes); (d) sessions where the full-session token saving nets out small — the independent benchmark measured full-session savings of 31.6% (Codex) / 15.9% (Claude Code) across the category, with nobody reaching 60% ([ r/LLMDevs](https://www.reddit.com/r/LLMDevs/comments/1wp1szf/i_benchmarked_repowise_codegraph_serena_graphify/)). The actual crossover for your fleet is UNKNOWN — it is precisely what the trial measures.

### G. Preregistered full-scenario trial (critiqued BEFORE experiments; do not fit thresholds after outcomes)
Corpus: immutable allowlisted real Python/JS/TS/PowerShell snapshot (if present), file hashes recorded, logical repo_id/revision, NFC-normalized relative paths, explicit per-host root maps (Windows Hub E:\…, Mac /Users/… mapped separately — never equated). Because PowerShell is only separator-chunked ([ Text ops docs](https://cocoindex.io/docs/ops/text/)), include .ps1 in include_patterns so the trial actually tests the weakest path.
24 tasks = 4 each: known-symbol, exact, semantic-RU, semantic-EN, navigation, no-answer. Pre-registered note: ccc has no navigation capability — navigation tasks test that the baseline+ccc arm introduces no new confidently false answer, and no-answer tasks test score-blind top-k handling (§C).
Arms: baseline (CodeIndexer + same source-read fallback) vs baseline + ccc; oracle hidden from independent clean-context answerers; blinded independent judge; same model/harness/budgets; seeded AB/BA with 3 paired repetitions.
Budgets: 10 retrieval/read calls and 24,000 UTF-8 bytes per task, counted after final serialization including JSON escapes/wrappers/errors/retries/hook echoes; over-budget messages withheld whole; actual delivered bytes/hash/order and real ccc successes/failures preserved. Zero-success treatment cannot prove benefit (explicitly: the invalidated v2 with zero successful calls proves nothing).
Resources: 3 cold / 3 warm observations, 60 s idle before/after; tree peak, idle RSS, private bytes, CPU-seconds, total host load, GPU VRAM, disk, network, provider-billed receipts; UNKNOWN is recorded as UNKNOWN, never as 0. Resource trials serialized by physical-host lease. Given §B/§D evidence, add: daemon process count (historical duplicate-daemon defect) and slim-cloud daemon idle RSS as first-class metrics, with particular attention on the Windows Hub ([ agent-takkub #901](https://github.com/takkub/agent-takkub/issues/901)).
Mutations: COPY-only create/edit/rename/delete; t_mutation / index-observed / correct-retrieval at 5-second polls to 120 s; no stale delete hit at 120 s; manual refresh labeled manual with its no-op cost measured; never restart production to manufacture cold.
Consumption: an independent other LLM client must actually consume the result; invocation and benefit are separately recorded.
Hard gates: no secrets; identical corpus; complete context accounting; no wrong-root; ≥90% coverage; independent client; no stale delete at 120 s.
KEEP only if: no new confidently false answer; correctness decline ≤1/24; and median wall-time OR total CPU improves ≥20% — OR correctness improves ≥3/24 with median wall-time increase ≤20%; added idle RSS ≤256 MiB/host and peak ≤1 GiB (unless pretrial amendment). Resource/cost UNKNOWN blocks any resource-saving claim. Operational thresholds are not statistical universal proof. CodeIndexer retained; expansion to other OS only after measured Hub benefit and independent review/readback.
Additions from this research (pretrial amendments, declared now): pin and record cocoindex-code==0.2.42 + resolved cocoindex version + lockfile hash; COCOINDEX_DISABLE_USAGE_TRACKING=1 in both arms' env; COCOINDEX_LMDB_MAP_SIZE headroom for the corpus; smoke-test ccc grep before counting it as an available tool (code_match released-status conflict, §C); CLI-only operation (no MCP, no hooks) so idle_timeout_minutes applies and keepalive heartbeats are absent; cost ceiling = existing confirmed credits only.

### H. Adversarial verdict — KEEP / TRY / PARK
Verdict: TRY (minimal canary under §G), then KEEP only on gate pass. Do not KEEP now; do not PARK the question — the slim variant specifically removes the largest known objection (torch).





1

Slim avoids ~1 GB local ML deps

pyproject base deps, README install styles ([ pyproject](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml), [ README](https://github.com/cocoindex-io/cocoindex-code))

Verified (implementation)


2

Saves local RAM/CPU overall

No vendor measurements; Windows 11 reports of 62% CPU / 1.8–1.9 GB RSS with daemon, v0.2.42, Oct 23, 2026 ([ #901](https://github.com/takkub/agent-takkub/issues/901), [ #904](https://github.com/takkub/agent-takkub/issues/904))

UNKNOWN/disconfirmed risk — must measure; slim+cloud RSS unknown


3

70% token saving

Vendor anecdote ([ Towards AI](https://pub.towardsai.net/i-built-an-ast-powered-code-mcp-that-saves-70-tokens-heres-how-it-works-3dbe58746729?gi=5cd1236a2789)); independent benchmark: nobody hit 60%, one ccc instance returned no answer ([ r/LLMDevs](https://www.reddit.com/r/LLMDevs/comments/1wp1szf/i_benchmarked_repowise_codegraph_serena_graphify/))

Marketing; treat as unproven


4

Freshness (no stale hits)

Incremental refresh; MCP refresh default; delete-retire semantics ([ README](https://github.com/cocoindex-io/cocoindex-code), [ marketing](https://cocoindex.io/cocoindex-code/))

Plausible; delete-at-120 s is a trial gate


5

Structural grep available now

README (Oct 6): needs unreleased cocoindex; docs (Jul 4) document CodePattern ([ README](https://github.com/cocoindex-io/cocoindex-code), [ Text ops](https://cocoindex.io/docs/ops/text/))

Conflict — smoke test


6

Windows support

Windows wheels exist ([ PyPI 1.0.21](https://pypi.org/project/cocoindex/1.0.21/)); daemon runs on Win11 ([ #901](https://github.com/takkub/agent-takkub/issues/901))

Supported, with resource/daemon-bug caveats (v0.2.42 lacks the #288 fix)


7

Trial affordable with existing credits

Voyage 200M free/account; OpenAI $0.02/1M; Gemini free tier ([ Voyage](https://docs.voyageai.com/docs/pricing), [ OpenRouter](https://openrouter.ai/openai/text-embedding-3-small), [ EmbeddingCost](https://embeddingcost.com/google))

Plausible; eligibility UNKNOWN until dashboards checked


8

Alpha-quality release

Development Status :: 3 - Alpha ([ pyproject](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml))

Fact — informs canary scope and rollback readiness







Strongest FOR: official slim variant with zero ML deps + cloud inference; genuinely incremental re-embedding (changed chunks only, memoized); freshness built into search (--refresh, MCP default); embedded SQLite/LMDB storage with no Postgres; signed, active releases through Oct 2026; Apache-2.0; 200M free Voyage tokens can fund the entire trial ([ pyproject](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml), [ EMBEDDINGS.md](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md), [ README](https://github.com/cocoindex-io/cocoindex-code), [ Voyage pricing](https://docs.voyageai.com/docs/pricing)).
Strongest AGAINST: the decision's own goal (save local RAM/CPU on the Windows Hub) is the exact axis with adverse third-party measurements and zero favorable measurements; the token-saving headline is unproven; the released v0.2.42 still contains the socket-deletion defect; PowerShell (a required language) is second-class in chunking; navigation is absent so 4/24 tasks can only test "does no harm"; Voyage's privacy opt-out needs a payment method, conflicting with constraints ([ #901](https://github.com/takkub/agent-takkub/issues/901), [ r/LLMDevs](https://www.reddit.com/r/LLMDevs/comments/1wp1szf/i_benchmarked_repowise_codegraph_serena_graphify/), [ #288](https://github.com/cocoindex-io/cocoindex-code/issues/288), [ Text ops](https://cocoindex.io/docs/ops/text/), [ Voyage FAQ](https://docs.voyageai.com/docs/faq)).
Minimal canary & cost ceiling: one Windows Hub COPY, pinned cocoindex-code==0.2.42 slim, isolated COCOINDEX_CODE_DIR, CLI-only (no hooks/MCP/plugin), one provider chosen from existing confirmed credits (check OpenAI/Voyage/Gemini dashboards first), §G trial, ceiling = those credits only. Rollback stops ONLY the owned isolated daemon (ccc daemon stop) and disconnects the temporary client; ccc reset --all inside the COPY; production, CodeIndexer, config, and evidence remain untouched. Install success, ACK, rc=0, or a snippet working is not fleet PASS.

## Source Notes




[ cocoindex-code repo / README (main)](https://github.com/cocoindex-io/cocoindex-code)

5/5

2026-10-06 (README); main head 2026-10-22


[ Release v0.2.42](https://github.com/cocoindex-io/cocoindex-code/releases/tag/v0.2.42)

5/5

2026-10-05


[ pyproject.toml (raw, main)](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)

5/5

2026-10-22


[ EMBEDDINGS.md (raw, main)](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)

5/5

2026-08-02


[ PyPI cocoindex-code](https://pypi.org/project/cocoindex-code/)

5/5

0.2.42 (Oct 2026)


[ PyPI cocoindex (framework)](https://pypi.org/project/cocoindex/) and [ 1.0.21 wheel files](https://pypi.org/project/cocoindex/1.0.21/)

5/5

2026-11-01 (1.1.0 current)


[ Framework release v1.0.25](https://github.com/cocoindex-io/cocoindex/releases/tag/v1.0.25) and [ releases listing](https://github.com/cocoindex-io/cocoindex/releases)

5/5

2026-10-06


[ CocoIndex text-ops docs](https://cocoindex.io/docs/ops/text/) and [ languages snippet](https://pub.towardsai.net/i-built-an-ast-powered-code-mcp-that-saves-70-tokens-heres-how-it-works-3dbe58746729?gi=5cd1236a2789)

5/5

reviewed 2026-07-04 (v1.0.14)


[ CocoIndex telemetry docs](https://cocoindex.io/docs/about/telemetry/)

5/5

current


[ Voyage AI pricing](https://docs.voyageai.com/docs/pricing) and [ FAQ](https://docs.voyageai.com/docs/faq)

5/5

2026-11-01


[ OpenAI data controls](https://developers.openai.com/api/docs/guides/your-data)

5/5

current (2026)


[ OpenRouter text-embedding-3-small](https://openrouter.ai/openai/text-embedding-3-small), [ CostGoat Oct 2026](https://costgoat.com/pricing/openai-embeddings)

3/5 (aggregators)

2026-10


[ Google embedding GA blog](https://pirouter.ai/blog/does-your-llm-provider-train-on-api-data), [ Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing), [ EmbeddingCost checked 2026-10-16](https://embeddingcost.com/google)

5/5, 5/5, 3/5

2026


[ Issue #288 socket deletion](https://github.com/cocoindex-io/cocoindex-code/issues/288) + [ PR #291](https://github.com/cocoindex-io/cocoindex-code/pull/291) + [ PR #243](https://github.com/cocoindex-io/cocoindex-code/pull/243)

4/5

2026 (v0.2.32-era; fix post-v0.2.42)


[ Windows resource reports (agent-takkub #901, #904)](https://github.com/takkub/agent-takkub/issues/901)

2/5 (third-party, single reporter, but measured)

2026-10-23


[ r/LLMDevs independent benchmark](https://www.reddit.com/r/LLMDevs/comments/1wp1szf/i_benchmarked_repowise_codegraph_serena_graphify/)

2/5 (competitor-authored; public raw data)

2026


[ Vendor 70% articles (Towards AI / HackerNoon)](https://pub.towardsai.net/i-built-an-ast-powered-code-mcp-that-saves-70-tokens-heres-how-it-works-3dbe58746729?gi=5cd1236a2789)

2/5 (vendor marketing)

2026


[ PyPI namesake cocoindex-code-mcp-server](https://pypi.org/project/cocoindex-code-mcp-server/)

5/5 (for existence)

0.2.0







Conflicts and caveats: (1) ccc grep/code_match released status — README says unreleased, framework docs document it (§C); (2) Voyage's own pricing prose lists voyage-code-3 in the 200M-free group while its table lists voyage-code-4 at $0.12/1M, and third parties price voyage-code-3 at $0.18/1M — verify in the dashboard before budgeting; (3) all resource/memory claims other than the cited Windows reports are unmeasured marketing or install sizes, not RSS; (4) the independent benchmark author works on a competing tool — direction of skepticism noted, raw data public.

## Open Questions
Actual slim+cloud daemon idle/peak RSS and CPU on the Windows Hub — UNKNOWN, gating.
Is code_match in the installed (released) cocoindex wheel → does ccc grep work out of the box? — verify by smoke test.
Is .ps1 in ccc's default include patterns, and how are binary files detected? — UNKNOWN; ccc doctor + explicit patterns.
Exact DB path representation across OS and duplicate-checkout behavior — UNKNOWN; keep DBs per-host.
Which existing accounts hold usable embeddings credits/allowances (chat subscriptions do not imply them) — UNKNOWN until checked.
Cohere embed-v4.0 pricing/retention (only noted, not relied upon).
Interrupted-update resume and concurrent-project lock semantics — not documented; test.

## Recommendations / Next Steps
Do not KEEP yet. Approve the TRY: one Windows Hub COPY canary exactly per §E/§G, existing credits only, with the slim build's daemon RSS and process-count as first-class metrics.
Before any trial run: check OpenAI/Voyage/Gemini dashboards for existing credit eligibility; run the ccc grep smoke test; pin versions and lockfile hash; declare the LMDB map-size and telemetry opt-out amendments.
Run the blinded 24-task A/B/BA trial with the preregistered gates; a resource/cost UNKNOWN blocks any resource-saving claim even if correctness holds.
If gates pass on the Hub, expand to Intel Mac Pro and Mac Studio copies only after independent review/readback, per the preregistered rule.
If gates fail or resources stay UNKNOWN, roll back (ccc daemon stop, ccc reset --all in the COPY, uninstall) and retain CodeIndexer unchanged; the evidence matrix above remains reusable for the next DR.

