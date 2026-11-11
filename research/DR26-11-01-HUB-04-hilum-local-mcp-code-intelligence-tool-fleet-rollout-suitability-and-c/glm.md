# DR26-11-01-HUB-04 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-11-01-HUB-04-1438
vendor: glm
source: chat.z.ai

conversation_id: 6a9e2627-347f-4f88-8aa8-0c225209de73
conversation_url: "https://приватный чат"
title: "ChunkHound Cloud Embeddings Deep Research"
date_recorded: 2026-11-01
date_collected: 2026-11-05
chars: 58762

---
Verified the repo pointer. Now digging into primary sources: PyPI metadata, the v6.1.0 release notes, raw README, and the docs sitemap.

Core identity verified. Now exploring docs, package structure, the native Rust component, and the changelog.

Important finding: no macOS Intel wheel for the native component. Continuing with docs, CLI reference, provider implementation, and the Windows database issue.

The web tool hit a rate limit. Let me pause briefly and continue with targeted fetches.

Web tool is rate-limited for this turn. Switching to curl via Bash to continue primary-source research.

curl 运行良好。现在让我获取 CLI 文档、更新日志、提供商源代码列表，并系统地分析关键实现文件。

Source-level research is solid. Now provider pricing and policies, then competitor pages.

Wayback is too slow; moving on to competitor research — CodeIndexer and mgrep.

All research complete. Here is the full report as requested — Markdown directly in chat, no files.

---

# DR26-11-01-HUB-04-1438 — ChunkHound (cloud embeddings) product-specific deep research

**As of:** 2026-11-01 · **Scope:** public primary sources only · **Consumer:** multi-agent Claude/Codex/Cursor code-search workflow (Windows Hub, Intel Mac Pro, Apple Silicon Mac Studio); CodeIndexer remains first; ChunkHound evaluated as challenger only. Ordinary sanitized code may use cloud; no secrets/cookies/keys/confidential third-party data. Existing prepaid/subscription credits first; no new cards/plans. Research only — nothing installed, configured, or purchased. The prior internal 40-answer V2 comparison is treated as INVALID and was not reused.

**Evidence labels used throughout:** **[D]** = documented on official docs/site/release notes (rolling) · **[S]** = source-verified in the git tree at tag `v6.1.0` (commit `c3b332b…`) · **[M]** = measured/computed by this DR from public metadata fetched 2026-11-01 · **[V]** = vendor marketing claim, unverified · **[I]** = inferred · **[U]** = UNKNOWN (no credible public source found; never guessed).

---

## 1. Identity, version, license, recent changes (Q1)

| Item | Value | Label / Source |
|---|---|---|
| Official repository | `github.com/chunkhound/chunkhound` ("Your entire engineering context, deeply understood"; MIT; 1,456 stargazers) | [D] (https://github.com/chunkhound/chunkhound) |
| Official docs | `chunkhound.ai` (Getting Started / Configuration / CLI Reference / Changelog / Contributing) | [D] (https://chunkhound.ai, https://chunkhound.ai/docs/getting-started/) |
| Official package | PyPI `chunkhound` (project URLs point to the repo and chunkhound.ai; PyPI owners `Hanan_kavitz`, `ofriw`; author field "Ofri Wolfus") | [D] (https://pypi.org/pypi/chunkhound/json) |
| Latest stable release | **v6.1.0, released 2026-10-20**, tag commit **`c3b332b16967636a39e26f2746df28d03d31f861`** ("docs(changelog): add 6.1.0 release notes (#443)", 2026-10-20 10:41 UTC); PyPI wheels uploaded 2026-10-20T11:02–11:03Z | [M] (releases page, tag clone, PyPI JSON) |
| License | **MIT** (repo license field "MIT License"; README "ChunkHound is MIT licensed"; PyPI classifier "OSI Approved :: MIT License"; native crate also MIT) | [D] (https://github.com/chunkhound/chunkhound, https://pypi.org/pypi/chunkhound/json) |
| Python requirement | `>=3.10,<3.15` (classifiers list 3.10–3.14) | [D] (https://pypi.org/pypi/chunkhound/6.1.0/json) |

**Verification of your preliminary pointer:** CONFIRMED — `chunkhound/chunkhound` **v6.1.0**, commit `c3b332b`, **MIT**. The pointer is accurate (https://github.com/chunkhound/chunkhound/releases, https://github.com/chunkhound/chunkhound/releases/tag/v6.1.0, git tag `v6.1.0` cloned and inspected).

**Recent release history** (https://github.com/chunkhound/chunkhound/releases and CHANGELOG.md at tag):

- **v6.1.0 (2026-10-20, `c3b332b`)** — opt-in usage analytics (off by default); **native (Rust) embedding providers** for OpenAI-compatible incl. Azure and VoyageAI, with retry, token-aware batching; Requesty LLM provider; Windows `.cmd`/`.bat` CLI launch fixes (addresses issue #118 — but "The database `WinError 183` in that issue is unchanged"); Linux wheels `manylinux_2_31`; orphaned-row cleanup on forced reindex; no-op re-index no longer rebuilds HNSW; HNSW maintenance off the event loop; rerank retries on the single-batch path; `..` path-escape rejection; MCP SDK constrained to `mcp>=1.16,<2`. [D]
- **v6.0.0 (2026-09-14, `a8553ae`)** — **breaking: `chunkhound-native` (Rust extension) becomes a required dependency**; Rust parse→embed→write pipeline default (`CHUNKHOUND_USE_RUST=0` to opt out; "Installs where no native wheel is available fall back to the Python pipeline automatically"); VoyageAI `voyage-4` series (voyage-code-4 etc., Matryoshka 256/512/1024/2048 dims, 32K context); OpenRouter + OrcaRouter LLM providers; **MCP Streamable HTTP transport** (`--transport http`); `fetchurl` command/tool; **index now "owns" its embedding model/dims** (model change prompts instead of silently re-embedding); remote-config fleet fetch. [D]
- **v5.2.0 (2026-07-12, `d08e843`)** — Rust-native file scanner; DB compaction; git diff search & websearch tool; **PowerShell language support (`.ps1`/`.psm1`)**; read-only DB mode. [D]
- **v5.0.0 (2026-05-05)** — **multi-client MCP daemon** (shared DuckDB connection); HTTP MCP *removed* (stdio only — later re-added in v6.0.0); config precedence reordered; Elixir; Grok; `openai_compatible` embedding provider; Azure OpenAI embeddings; Matryoshka embeddings. [D]
- **v4.0.0 (2025-11-12)** — map-reduce synthesis, compact `[1][2][3]` citation system, `chunkhound research` CLI, `--simulate`, `calibrate`. [D]

**Rolling docs vs pinned implementation:** all implementation evidence in this report is pinned to the `v6.1.0` tag (`c3b332b`); docs at chunkhound.ai and `main`-branch CHANGELOG are rolling and were cross-checked against the tag where possible.

---

## 2. Inline evidence table (sources actually opened)

| # | Source (opened 2026-11-01) | What it supports |
|---|---|---|
| 1 | https://github.com/chunkhound/chunkhound | Repo identity, MIT, README, v6.1.0 latest |
| 2 | https://github.com/chunkhound/chunkhound/releases | Release dates/versions/commits |
| 3 | https://github.com/chunkhound/chunkhound/releases/tag/v6.1.0 | v6.1.0 changes; WinError 183 "unchanged"; native embedding providers |
| 4 | https://pypi.org/pypi/chunkhound/json and https://pypi.org/pypi/chunkhound/6.1.0/json | Version, Python range, full dependency pins, owners, upload times, wheel matrix |
| 5 | https://pypi.org/pypi/chunkhound-native/json | Native wheel platforms (win_amd64, macosx_11_0_arm64, manylinux_2_31 x86_64/aarch64) — **no macOS x86_64, no sdist, any version** |
| 6 | https://chunkhound.ai | Product claims, provider lists, MCP clients, "50M+ lines" |
| 7 | https://chunkhound.ai/docs/getting-started/ | Install (macOS/Linux/PowerShell), config JSON, per-client MCP snippets (Cursor/Claude/Codex/VS Code/OpenCode/Windsurf/Roo/Zed), verify chain, `chunks.db` path |
| 8 | https://chunkhound.ai/docs/configuration/ | Embedding models/dims/batch token limits, model-switch semantics, DuckDB vs LanceDB, indexing defaults/excludes, LLM provider table, rerank options, Azure, Ollama/vLLM, websearch/fetchurl, remote-config, startup validation codes |
| 9 | https://chunkhound.ai/docs/cli-reference/ | All CLI commands/flags incl. `mcp --transport/--host/--port/--auth-token/--cors/--read-only/--no-daemon`, `--simulate`, `--check-ignores`, `calibrate` |
| 10 | https://raw.githubusercontent.com/chunkhound/chunkhound/main/README.md | Requirements, provider recommendations, git-history search, `--vector-source` |
| 11 | https://raw.githubusercontent.com/chunkhound/chunkhound/main/CHANGELOG.md | Full version history (v0.1.0 2025-06-11 → v6.1.0 2026-10-20) |
| 12 | https://raw.githubusercontent.com/chunkhound/chunkhound/v6.1.0/pyproject.toml | Dependency pins incl. `chunkhound-native>=5.2.0` (unconditional), `duckdb==1.5.4`, `pyarrow<24`, `zendriver==0.15.3`, `mcp>=1.16,<2`; entry points |
| 13 | git clone of tag `v6.1.0` (`c3b332b`) — source files cited inline as `[email hidden]` | Provider registries, batch/retry logic, daemon/client proxy, MCP tool registry, research budgets, default excludes, relative-path storage, Rust pipeline (scan/embed/retry/token) |
| 14 | https://github.com/chunkhound/chunkhound/issues/118 | Windows: `.cmd` Claude launch + `[WinError 183] Cannot create a file when that file already exists: …\.chunkhound\db` (2025-12-12, closed; DB error unchanged in v6.1.0 per release notes) |
| 15 | https://github.com/chunkhound/chunkhound/issues/330 | Open: macOS arm64 has **no packaged Watchman runtime** (linux/x86_64, windows/x86_64 only); explicit `--realtime-backend watchman` fails without polling fallback |
| 16 | https://docs.voyageai.com/docs/pricing | Voyage prices, free tokens, rerank billing formula ("Updated about 1 month ago") |
| 17 | https://docs.voyageai.com/docs/faq.md | Rerank 600K token/request cap; billing cadence; prepaid; **data-use opt-out requires payment method + org admin** |
| 18 | https://docs.voyageai.com/llms.txt | Docs index (privacy page URL does not exist; FAQ is the data-policy source) |
| 19 | https://azure.microsoft.com/en-us/pricing/details/cognitive-services/openai-service/ | Azure OpenAI `text-embedding-3-small` regional prices (us-east $0.000022/1K) |
| 20 | https://codeindexer.dev and https://codeindexer.dev/pricing | CodeIndexer v0.12.0: 100% local Rust binary, 68 languages/27 call-graph, 32 MCP tools, free tier (3 projects/100k chunks), Pro $16/mo/seat, demo index stats |
| 21 | https://github.com/mixedbread-ai/mgrep | mgrep: Apache-2.0, 4.4k stars, cloud-backed Mixedbread stores, `watch`, agent hooks, default limits 1MB/file & 1000 files/dir, 2× token-reduction claim |
| 22 | https://www.mixedbread.ai/pricing | Mixedbread plans ($5 one-time credits Starter; Scale $20/mo) and usage rates (Fast index $1.50/1M content tokens; semantic search $4/1K queries +$3.50/1K rerank; storage $0.50/1M/mo) |
| 23 | https://www.mixedbread.ai/cli | mxbai platform CLI (distinct product from mgrep; confirms store model) |
| 24 | PyPI JSON APIs for the dependency closure (e.g. `pypi.org/pypi/duckdb/1.5.4/json`, `pyarrow/23.0.0`, `lancedb/0.25.3`, and ~35 more) | Download-size footprint computation |

**Blocked / not opened (do not treat as verified):** `platform.openai.com/docs/pricing` and `openai.com/api/pricing` (Cloudflare challenge), `docs.anthropic.com` pricing (region-blocked), `web.archive.org` snapshots (timeouts). Consequently OpenAI-direct and Anthropic API list prices are **[U]** in this report.

---

## 3. Provider map (Q2) — what exists, what each capability needs

### 3.1 Embedding providers (for indexing + semantic search)

Verified against docs [D] and the v6.1.0 source registry [S]:

| Provider | Config value | Default model | Notes |
|---|---|---|---|
| VoyageAI (recommended by README) | `voyageai` | `voyage-3.5` (constants: `VOYAGE_DEFAULT_MODEL`); docs recommend `voyage-code-4` for code | SDK path; rerank via same provider (`rerank-2.5` default, `VOYAGE_DEFAULT_RERANK_MODEL`) [S `chunkhound/core/[email hidden]`] |
| OpenAI | `openai` | `text-embedding-3-small` (`OPENAI_DEFAULT_MODEL`) [S] | Native Azure support (`azure_endpoint`/`api_version`/`azure_deployment`; mutually exclusive with `base_url`); Matryoshka dims |
| Any OpenAI-compatible | `openai` + `base_url` | explicit model required | Ollama (`qwen3-embedding`), vLLM (`Qwen/Qwen3-Embedding-0.6B`), Azure-hosted Voyage (with caveats: bundled reranker unreachable; concurrency throttled to 1 by default when `base_url` set) [D config docs] |

**Voyage model table as implemented** [S `chunkhound/providers/embeddings/[email hidden]`, identical to docs]: voyage-code-4 / voyage-4 / voyage-3.5 — 32K context, **320K tokens per batch**, 1000 texts/batch, dims 256/512/1024/2048 (default 1024); voyage-4-large / voyage-4-lite-family older models at 120K or 1M tokens/batch; voyage-law-2 16K context; unknown models: runtime dimension discovery, conservative 320K batch limit. OpenAI models [S `[email hidden]`]: text-embedding-3-small 1536 dims / 8191 max tokens; text-embedding-3-large 3072 / 8191; ada-002 1536.

**Batching / retries / limits:** token-aware batch sizing; embedding `timeout` 30s, `max_retries` 3 (default) [D config]; retry classification rate-limit / upstream-timeout / network with backoff base (rate-limit 30.0s) [S voyageai_provider.py]; Rust path mirrors this: retry on 408/429/5xx/IO, jittered sleep, hard 120s ceiling, ~3-chars/token estimate for batching [S `src/embed/retry.rs`, `src/embed/token.rs`]. Rerank: `rerank_model`, `rerank_url` (auto-derived `/rerank` from `base_url`), `rerank_format` `cohere`/`tei`/`voyage`/`auto`, `rerank_batch_size` (default 1000), retries on both paths since v6.1.0 [D+S]. Azure embeddings supported natively; **LLM-side Azure is not** ("Use a proxy if you need to route LLM traffic through Azure") [D config].

### 3.2 Reranking

Reranking is a *capability of the embedding provider config* (`embedding.rerank_model`), not a separate provider: VoyageAI SDK rerank (default `rerank-2.5`), Cohere-format HTTP rerankers (e.g., vLLM `Qwen/Qwen3-Reranker-0.6B` with `rerank_format: cohere`), TEI (`rerank_format: tei` + `rerank_url`), Voyage-format endpoints (e.g., MongoDB Atlas) [D config; S]. [S]

### 3.3 Research-LLM providers (for `research`/`code_research`, websearch synthesis)

Docs table [D config] + source [S `chunkhound/providers/llm/`]: **API-key providers:** Anthropic (utility default `claude-haiku-4-5-20251001`, synthesis `claude-sonnet-4-5-20250929`), OpenAI (`gpt-5-nano`/`gpt-5`), Gemini (`gemini-3.5-flash` example; model must be set), Grok (`grok-4.3` example), DeepSeek, OpenRouter, OrcaRouter, Requesty, API Route, Opper, Obscurify.ai, Atlas Cloud. **CLI providers (use existing coding subscriptions):** `claude-code-cli` (wraps `claude --print`, tools disabled, MCP disabled, runs from temp dir — "to enable deep research using the user's existing Claude subscription instead of API credits" [S `[email hidden]`]), `codex-cli` (wraps `codex exec`, default synthesis model `gpt-5.1-codex` [S]), `opencode-cli`. **Local:** `provider: "openai"` + `base_url` for Ollama/vLLM. Per-role split `utility_model`/`synthesis_model` supported; LLM `timeout` 120s, `max_retries` 3 [D].

### 3.4 Which capability needs which provider [D README + config]

| Capability | Providers required |
|---|---|
| Regex search (CLI/MCP) | **None** (works keyless) |
| Semantic search | Embedding provider |
| Deep research (`research` / `code_research`) | LLM provider **and** embedding provider **with reranking support** |
| `websearch` | Embedding + LLM + reranker (tool not registered otherwise — capability gating) |
| `fetchurl` | LLM + reranker-capable embedding provider |

### 3.5 Coding subscriptions vs API credits — the direct answer

**Coding subscriptions (Claude Code / Codex / OpenCode CLI) are usable ONLY for the research-LLM role, not for embeddings or reranking.** [D+S: the CLI providers exist only in the LLM provider registry; the embedding registry contains only voyageai/openai(+compatible/Azure).] Embeddings require separate VoyageAI or OpenAI(-compatible) credentials. This fits your "existing subscription first" constraint for the *research* leg (Claude/Codex subscription quota), while the *embedding* leg needs an API key — VoyageAI's 200M free tokens (§7) are the natural no-new-card path **if** an account already exists or can be created under your existing policy.

---

## 4. Architecture and data flow (Q3) — what is local, what leaves the machine

### 4.1 Local components [D config/docs + S]

- **Parsing/chunking:** tree-sitter grammars for Python, JavaScript, TypeScript (+TSX), Java, Go, Rust, C/C++, C#, and ~30 more incl. **PowerShell (`.ps1`/`.psm1`, since v5.2.0)** [D CHANGELOG; S `core/types/common.py` maps `.ps1`/`.psm1` → POWERSHELL with class support]. All four languages present in your fleet code (Python/JS/TS/PowerShell) are supported. Code-aware chunking based on **cAST (Carnegie Mellon research, arXiv:2506.15655)** [V/D — claim on chunkhound.ai].
- **Storage:** **DuckDB (stable, recommended, pinned `duckdb==1.5.4`)** at `<project-root>/.chunkhound/db/chunks.db` by default; **LanceDB (experimental — "may have rough edges around index rebuilding, migration, and edge-case query correctness")** [D config]. Non-canonical tables are dropped during compaction by design [D].
- **Vector index:** HNSW (native Rust); rebuilt off the event loop since v6.1.0; untouched on no-op re-index [D v6.1.0 notes].
- **Native Rust component:** `chunkhound-native` (PyO3): file scanner (`scan_files` with gitignore handling), parse→embed→write pipeline (default since v6.0.0), native embedding HTTP clients (OpenAI-compatible/Azure/Voyage) with token-aware batching and retry, DB layer, analytics [S `src/` at v6.1.0].
- **Change detection:** xxhash hashing, file-discovery cache, Rust scanner; per-file parse timeout (default 3.0s, applies ≥128KB files); `max_file_size_mb` 10; `config_file_size_threshold_kb` 20 [D config].
- **Realtime/watch:** `RealtimeIndexingService` (event queue 1000, dedup 2s, change/delete/embed/dir events); backends **watchman (sidecar; packaged runtimes exist only for linux/x86_64 and windows/x86_64 — none for macOS arm64, open issue #330), watchdog, or polling**; `CHUNKHOUND_INDEXING__REALTIME_BACKEND` [S services/realtime; D; issue #330].
- **Git:** pygit2 for diff/commit-range search; **git history search embeds diff hunks** (`--vector-source diff|both|db`) [D README/CLI].
- **Web fetch (websearch/fetchurl):** DuckDuckGo search; pages fetched via **zendriver 0.15.3 driving system-installed Google Chrome ≥124**, urllib fallback; PDFs via PyMuPDF [D config; S].
- **MCP daemon:** each `chunkhound mcp` invocation is a lightweight stdio↔IPC proxy; a shared background daemon owns the DB and realtime indexing; multiple clients (Claude + Codex + Cursor simultaneously) share one DuckDB connection; daemon shuts down when the last client disconnects; dead-client PID polling every 60s [S `daemon/client_proxy.py`, `daemon/client_manager.py`, `daemon/process.py`; D v5.0.0 changelog]. `--no-daemon` = single-client mode; `--read-only` = DuckDB-only, no watcher/daemon [D CLI].
- **DB concurrency model:** thread-safe serial executor; fast-fail errors when compaction or the Rust pipeline owns write access; compaction rebuilds a canonical file and swaps atomically; fixed compaction boundaries during `index` (including `--no-embeddings` and no-op runs) [S `providers/database/serial_executor.py`; D config].

### 4.2 Exactly which text leaves the machine (cloud-embedding configuration)

| Operation | Data sent to cloud | Destination |
|---|---|---|
| Indexing / re-embedding | **Chunk text (code) per file chunk** — `input_type: "document"`, `truncation: true`, optional `output_dimension` [S voyageai_provider.py `_embed_single_batch_locked`] | VoyageAI or OpenAI-compatible/Azure embeddings endpoint |
| Semantic search / research hops | **Query text** (embedded as query), plus expanded queries (LLM query expansion) | Embedding provider; LLM provider |
| Reranking | **Query + candidate chunk texts** (batch ≤1000 docs) | VoyageAI rerank (`rerank-2.5` default) or Cohere/TEI endpoint |
| Deep research (`code_research`) | Query, search results, and **file contents read locally** — per-node budgets FILE_CONTENT_TOKENS 10K–50K, LLM_INPUT_TOKENS 15K–60K, 2 LLM-expanded queries + original, up to 3 follow-up questions [S `services/research/shared/models.py`] | Research LLM (API provider or Claude/Codex CLI subscription) |
| `websearch`/`fetchurl` | Search query to DuckDuckGo; fetched page content is chunked/embedded/reranked via providers and summarized by the LLM [D config] | DDG + embedding/rerank/LLM providers |
| Usage analytics (v6.1.0) | **Off by default.** When enabled: command/tool-call events, provider calls with token counts to an operator-owned S3/MinIO bucket; free-text queries/URLs nulled unless `save_sensitive_data` [D v6.1.0 notes] | Operator's own bucket |

**ChunkHound is NOT cloud-hosted:** parsing, chunking, storage, and vector search stay local; only embeddings/rerank/LLM/websearch fetches hit the cloud. Cloud embeddings do **not** eliminate local RAM/CPU — the Rust pipeline, DuckDB, and the Python stack (pandas/scipy/scikit-learn) all run locally; no GPU use by ChunkHound itself is documented (GPU only if you run local Ollama/vLLM) [I from architecture; no measured numbers exist publicly — see §6].

### 4.3 Retention / training policies of the chosen provider

- **VoyageAI (the recommended embedding path):** "For Voyage-hosted model API endpoints, customers can opt-out from Voyage storing and using their data for future model training so that there is a zero-day retention of the data. To opt out, **you must have a payment method on file and be an organization Admin**." [D FAQ, https://docs.voyageai.com/docs/faq.md]. Default (no opt-out) therefore permits storage + training use. **Under your "no new cards" constraint this matters: if your Voyage org has no payment method on file, the zero-retention opt-out cannot be toggled** — confirm before pointing it at any code, even sanitized.
- **OpenAI / Azure OpenAI:** API data-retention/training terms — **[U]** here (pages blocked from this sandbox). Verify OpenAI's current API data-usage policy before use.
- **Local zero-egress setups** are supported (Ollama/vLLM embeddings+rerank+LLM) [D config] — outside this DR's cloud-embedding scope but the natural fallback if retention terms are unacceptable.

---

## 5. Lifecycle, commands, and integrations (Q4)

All commands below are **documented** [D getting-started + cli-reference] and cross-checked against the v6.1.0 CLI source [S `api/cli/main.py` subcommands: index, mcp, search, research, _quickresearch, websearch, fetchurl, map, autodoc, calibrate, _daemon].

### 5.1 Install (Windows Hub, Apple Silicon; **Intel Mac Pro — see warning**)

```powershell
# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
uv tool install chunkhound
chunkhound --version        # expect: chunkhound 6.1.0
```
```bash
# macOS (Apple Silicon) / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
uv tool install chunkhound
```

- Python: `>=3.10,<3.15` (uv manages it). Package matrix [M PyPI]: `chunkhound` 6.1.0 ships a pure-Python `win_amd64` wheel (12.8 MB), a `manylinux_2_31_x86_64` wheel, and an sdist (macOS installs build from sdist); `chunkhound-native` 6.1.0 ships `cp310-abi3` wheels for **win_amd64 (16.5 MB), macosx_11_0_arm64 (38.7 MB), manylinux_2_31 x86_64 (26.9 MB) and aarch64 (24.7 MB)** only.
- ⛔ **Intel Mac Pro: v6.1.0 cannot be installed.** `chunkhound-native>=5.2.0` is an unconditional dependency [S [email hidden]; PyPI requires_dist], and PyPI shows **no macOS x86_64 wheel and no sdist for chunkhound-native in any published version** (checked all 8 versions, 5.0.0→6.1.0) [M https://pypi.org/pypi/chunkhound-native/json]. Dependency resolution therefore fails before the "fall back to the Python pipeline" runtime behavior can help (that fallback concerns *runtime* unavailability, not installability) [D v6.0.0 notes; S]. If the Mac Pro must run it, the only documented-compliant options would be a future release shipping an Intel wheel/sdist or building the Rust extension yourself — both outside this DR's "no installation" and your canary constraints. **The Intel Mac Pro is effectively out of scope for ChunkHound v6.1.0.**

### 5.2 Index / verify / search / update / remove

```bash
chunkhound index .                                   # index (on-demand, explicit)
chunkhound index --simulate --json                   # dry-run: what would be indexed
chunkhound index --check-ignores --vs git            # compare ignore decisions vs git
chunkhound search --regex "import"                   # regex, no API key
chunkhound search "authentication flow"              # semantic (needs embeddings)
chunkhound search "migration" --last-n 20            # git-history (diff embeddings)
chunkhound search "API changes" --commit-range v2.0..HEAD   # + --vector-source diff|both|db
chunkhound research "How does authentication work?"  # cited deep research (needs LLM+embeddings+rerank)
chunkhound websearch "Stripe webhook retry schedule" # needs embed+LLM+reranker; Chrome >=124
chunkhound fetchurl https://example.com/spec.html -q "…"  # needs LLM+reranker
chunkhound calibrate                                 # batch-size tuning
```

- **On-demand vs watchers:** there is **no `--watch` CLI flag** [S CLI source]; explicit indexing is `chunkhound index`; **realtime watching runs inside the MCP daemon** (watchman/watchdog/polling backend) so MCP-connected clients see live updates [S services/realtime; D]. Your "explicit/manual refresh distinct from watch" requirement maps to: `chunkhound index .` (manual) vs daemon-mode `chunkhound mcp` (watch).
- **Update semantics:** re-running `index` does change detection (xxhash + discovery cache; `--all-discovered`, `--show-sizes` for inspection); `force_reindex` config flag; `cleanup: true` default removes orphaned DB records after indexing; no-op runs no longer rebuild HNSW (v6.1.0); deleted-file rows cleaned on forced reindex (v6.1.0 fix) [D config + v6.1.0 notes]. **Stale-hit risk:** deleted files are only purged on a re-index/cleanup pass or a watcher `delete` event — an explicitly-refreshed arm must run `index` (or rely on daemon) after each mutation; regex search with `--last-n/--commit-hash/--commit-range` now *warns* those are ignored (v6.1.0) [D].
- **Hashes/stale hits:** file identity via xxhash content hash [S]; embeddings are per-(provider, model, dims) — an index built with one model "cannot be searched with another… a different model, or the same model at a different `output_dims`, returns nothing" [D config]. The index records its model/dims in `chunks.db.embedding.json` and resists silent re-embedding [D].
- **Remove/reset:** no dedicated `uninstall-index` command [S CLI]; documented way is "delete the database directory and re-index" [D config]. `chunkhound` itself uninstalls via `uv tool uninstall chunkhound` [I — uv standard behavior, not ChunkHound docs].
- **Dangerous defaults / gotchas (documented):** wrong `--db` subpath **silently returns 0 results**; pre-v4 flat `.chunkhound` files block dir creation; `--config` does not override a project-local config for the DB path; `detect_embedded_sql: true` by default (SQL inside string literals gets indexed → embedded to cloud); `exclude_mode` defaults to `gitignore_only` when no custom excludes (your `.gitignore` governs coverage); built-in excludes always overlay (node_modules, .git, venv, dist, build, target, .chunkhound, IDE dirs…) [D config; S `_default_excludes`]; analytics off by default; remote-config off by default (adds a synchronous 10s fetch per command when enabled) [D config]; HTTP MCP refuses non-loopback bind without `--auth-token`; CORS requires token [D CLI].

### 5.3 Cold/warm startup, shutdown, process model

- **Cold start:** first `index` parses + embeds; MCP daemon start loads DB and (in daemon mode) starts realtime indexing [S base.py]. **Startup phase timings are measurable via `chunkhound index --profile-startup` and `--perf-diagnostics --perf-output PATH`** [D CLI] — useful for your canary.
- **Warm:** subsequent searches hit the local DuckDB/HNSW; semantic queries cost one embedding call (+rerank if enabled) [S].
- **Shutdown:** daemon exits when the last client disconnects (`on_empty` callback; proxies orphan-poll every 5s; daemon PID-polls clients every 60s) [S daemon]. `--read-only` mode runs without daemon/watcher (DuckDB only) [D CLI].
- **Windows caveats:** `.cmd`/`.bat` CLI shims launch through `cmd.exe` — fixed in v6.1.0 with strict argument sanitization (no newlines/%/quotes; `&|<>^()` rejected in unspaced values); **`[WinError 183] Cannot create a file when that file already exists: …\.chunkhound\db` from issue #118 is explicitly "unchanged" in v6.1.0** [D v6.1.0 notes; issue #118]. Windows file-handle release delays are special-cased in the DB layer (`WINDOWS_FILE_HANDLE_DELAY`) [S].

### 5.4 MCP integrations and per-client process reality

Documented registration snippets [D getting-started]:

```bash
claude mcp add ChunkHound -- chunkhound mcp     # Claude Code
codex  mcp add ChunkHound -- chunkhound mcp     # Codex
```
```json
// .cursor/mcp.json (Cursor) / .vscode/mcp.json / ~/.codeium/windsurf/mcp_config.json / .roo/mcp.json
{ "mcpServers": { "ChunkHound": { "command": "chunkhound", "args": ["mcp"] } } }
```

- **Transport reality:** stdio by default; **Streamable HTTP** since v6.0.0 (`chunkhound mcp --transport http --port 5173 --auth-token … --cors`) for remote/browser clients; HTTP was absent in v5.x (removed v5.0.0, re-added v6.0.0) [D CLI + changelogs].
- **Per-client process duplication:** every client (Claude Code, Codex, Cursor…) spawns its own `chunkhound mcp` **proxy** process, but in daemon mode they all connect to **one shared daemon** that owns the DB and watcher [S client_proxy.py]. Cost: N light proxies + 1 heavy daemon per project. With `--no-daemon`, each client gets a full independent server (and independent DB connection) [D+S].
- **Exposed MCP tools:** `search` (regex/semantic, pagination, path filter), `code_research` (cited multi-hop research), `daemon_status`, `websearch` (gated), `fetchurl` (gated) [S `mcp_server/tools.py` registry]. There is **no call-graph/navigation tool** — see §8.
- **Cross-OS index portability / repo identity:** file paths are stored **relative to the project root** and converted to native separators at response time [S `indexing_coordinator._get_relative_path`, `tools._convert_paths_to_native`]; the embedding model/dims are pinned in a sidecar JSON [D]. There is **no repo/revision identity in the index** — the DB is bound to the project directory, not to a git remote/commit [I from absence in schema/docs]. Moving/copying the project (or the `.chunkhound/` dir) across OS should therefore *not* require re-embedding [I — plausible but **not documented**; must be tested in the canary; DuckDB file portability across architectures is a DuckDB property, not a ChunkHound promise].

---

## 6. Footprint (Q5) — only sourced numbers; everything else UNKNOWN

| Metric | Value | Label |
|---|---|---|
| `chunkhound` 6.1.0 wheel | 12.8 MB (win_amd64) / 12.8 MB (manylinux x86_64); sdist for macOS | [M PyPI] |
| `chunkhound-native` 6.1.0 wheel | 16.5 MB win_amd64 · 38.7 MB macosx_11_0_arm64 · 26.9/24.7 MB manylinux x86_64/aarch64 | [M PyPI] |
| Full dependency-closure **download** (chunkhound pins: duckdb 1.5.4, pyarrow 23.x, lancedb 0.25.3, + pandas/numpy/scipy/scikit-learn/pymupdf/tiktoken/mcp/voyageai/openai/anthropic/google-genai/pygit2/…, ~37 packages) | **≈251 MB (win_amd64) / ≈275 MB (macos arm64)** wheel downloads, computed 2026-11-01 from PyPI file sizes; exact set depends on resolver | [M] |
| Installed (on-disk) size | **UNKNOWN** (wheels expand; not measured — no public figure found) | [U] |
| Index size on disk | **UNKNOWN** for ChunkHound (docs show only "Indexed 412 files, 6,841 chunks" with no bytes) | [U] — for contrast, CodeIndexer's own demo shows "14 MB" for 412 files/198k lines [V their site] |
| Idle/peak RSS, CPU, GPU, network for indexing/search/update | **UNKNOWN** — no credible public measurements found anywhere in docs, changelog, or issues. Homepage claims only: "Proven on 50M+ line codebases in production… can run on a single developer laptop without requiring Kubernetes clusters or GPU servers" [V chunkhound.ai]. v6.0.0 claims "meaningfully faster indexing" with **no numbers** [D]. | [U]/[V] |

**Reproducible measurement proposal (for the canary, if you proceed):**
1. Fixed corpus: the sanitized copies themselves; record `git rev-parse HEAD`, file count, byte count, and `chunkhound index --simulate --json` output as the manifest.
2. Disk: `du -sb .chunkhound/db` before/after index and after compaction; also `uv tool dir` size for installed footprint.
3. Walltime + phase timing: `chunkhound index --profile-startup` and `--perf-diagnostics --perf-output runs/perf.json` (built-in flags [D CLI]).
4. Process-tree RAM/CPU: sample `psutil` (already a ChunkHound dependency) over the daemon+proxy tree — idle (no queries, 5 min), cold index, warm search, mutation-refresh; on Windows Hub use PowerShell `Get-Process -Id <tree> | select WorkingSet64,CPU` in a loop; GPU expected 0 unless local providers run.
5. Network: provider-side receipts (Voyage dashboard usage + token counts from `--verbose` logs) since no local packet accounting is built in.
6. Report hardware (CPU model/RAM/OS build), corpus stats, chunkhound version+commit, provider/model, and method — every number labeled with them.

---

## 7. Costs (Q6) — dated prices, formulas, and what is unknown

### 7.1 VoyageAI (recommended embedding+rerank path) — prices opened 2026-11-01, page "Updated about 1 month ago" [D https://docs.voyageai.com/docs/pricing]

| Item | Price | Free tier |
|---|---|---|
| `voyage-code-4` (code-specialized, ChunkHound-recommended) | **$0.12 / 1M tokens** | **200M free tokens/account** (pricing table; see §9 contradiction) |
| `voyage-4` / `voyage-4-large` / `voyage-4-lite` | $0.06 / $0.12 / $0.02 per 1M | 200M free (listed in prose for 4-series) |
| `voyage-3.5` (ChunkHound **default**) | $0.06 / 1M — but listed under "Older models": "**we do not offer free tokens for them**" | **none** |
| `voyage-code-3` | $0.18 / 1M (older, no free tokens) | none |
| Rerank `rerank-2.5` (ChunkHound default reranker) | **$0.05 / 1M tokens** | contradiction (§9) |
| Rerank `rerank-3` / `rerank-3-lite` (current gen) | $0.05 / $0.02 per 1M | 200M free |
| Batch API | −33% vs standard; free tokens do **not** apply | — |

**Rerank billing formula (Voyage-defined):** `(query tokens × number of documents) + sum of document tokens` — total capped at **600K tokens per request** for the rerank-2.5 family [D FAQ]. **Billing mechanics:** monthly invoice, card charge ~the 2nd of each month; prepaid credits require a payment method on file; auto-recharge optional [D FAQ]. **No trial-credit expiry date is published** for the 200M free tokens — expiry **[U]**; overage = standard per-token rates (no separate overage penalty documented); rate limits per docs.voyageai.com rate-limits page (not opened) **[U]**. ChunkHound-side safety: retries with backoff on 429, embedding `timeout` 30s / `max_retries` 3 defaults, rerank batch ≤1000 docs [D+S].

### 7.2 OpenAI embeddings

- **Direct OpenAI API price: [U]** — platform.openai.com and openai.com/api/pricing are Cloudflare-blocked from this sandbox; do **not** rely on remembered figures.
- **Azure OpenAI** (embeddings are natively supported by ChunkHound): `text-embedding-3-small` = **$0.000022 per 1K tokens in US East** (≈$0.022/1M), with small regional variation, as embedded in the Azure pricing page [M https://azure.microsoft.com/en-us/pricing/details/cognitive-services/openai-service/]. Azure terms/retention: not opened **[U]**.
- OpenAI trial credits/expiry: **[U]** (not verifiable here). Do not assume any balance exists.

### 7.3 Cost formulas for your use pattern (VoyageAI, list prices)

Let **T** = total chunk-text tokens sent at initial index (≈ corpus tokens after chunking; ChunkHound batches by ~3–4 chars/token estimates but **billing uses the provider's tokenizer** [I]):

- **Initial indexing:** `cost = T × $0.12/1M` (voyage-code-4) → e.g. T = 3M tokens ⇒ **$0.36**, far inside the 200M free tier on a fresh account. Choose `voyage-code-4` at **first** index: the default `voyage-3.5` has **no** free tokens, and switching models later triggers an interactive re-embed prompt and re-embeds everything (index "owns" its model) [D config].
- **Changed-file re-embedding:** only changed/deleted files' chunks are re-processed (xxhash change detection; realtime change/delete/embed events) ⇒ `cost ≈ ΔT × rate` per refresh cycle [D+S; exact ΔT measurable via Voyage receipts].
- **Semantic queries:** `query_tokens × rate` — negligible (tens of tokens).
- **Rerank per research/multi-hop call:** `((q_tokens × n_docs) + Σ doc_tokens) × $0.05/1M` (rerank-2.5); e.g. 20 docs × ~500 tokens ≈ 10K tokens ⇒ ~$0.0005/call.
- **Research LLM:** via `claude-code-cli`/`codex-cli` ⇒ **$0 API spend, consumes existing subscription quota** (subject to the subscription's own limits, which are not part of ChunkHound) [D+S]; via API providers ⇒ per-node LLM input 15K–60K tokens × provider rates [S budgets; rates **[U]** here]. Websearch/fetchurl add LLM+embedding+rerank usage on fetched content [D].
- **Storage:** local disk only — **no cloud storage fee** (contrast mgrep, §8) [D architecture].
- **No card, no cost path:** fresh Voyage account + voyage-code-4 within 200M tokens ⇒ $0 monetary, but **the training-data opt-out toggle requires a payment method on file** (§4.3) — the privacy cost may be nonzero even when the monetary cost is zero [D FAQ].

---

## 8. Comparison with CodeIndexer and mgrep (Q7)

### 8.1 Positioning (documented facts only; no invented quality ranking)

| Dimension | ChunkHound v6.1.0 | CodeIndexer v0.12.0 | mgrep (mixedbread) |
|---|---|---|---|
| License / model | MIT; Python+Rust hybrid; local-first, cloud providers optional | Site + GitHub `Nettechnologys/codeindexer`; **100% local single Rust binary, no cloud, no API key** [D codeindexer.dev] | **Apache-2.0**, 4.4k stars [D github.com/mixedbread-ai/mgrep]; **cloud-backed** — "Every file is pushed into a Mixedbread Store" [D README] |
| Search modes | Regex (keyless) + semantic (embeddings) + git-history diff search; multi-hop + rerank [D] | "Semantic, symbolic and exact search in one cascade" [D site] | Semantic + exact/grep-like; rerank on by default; `--agentic`, `--web`, `-a` answers [D README] |
| Navigation | No call-graph tool (MCP tools: search, code_research, daemon_status, websearch, fetchurl) [S] | **Cross-language call graph, callers/callees, blast radius, PageRank-style ranking** — 32 MCP tools [D site] | None documented |
| Cited answers | `code_research` produces "cited markdown explanations" with file:line citations; compact `[1][2][3]` citations since v4.0.0 [D+S] | Not an answer tool — returns chunks/graph for the agent to read [D site] | `--answer` summarizes search results w/ source hints [D README] |
| Languages | 33+ tree-sitter langs incl. **Python/JS/TS/PowerShell** [S] | 68 langs (27 full call graph) incl. PDF/DOCX/XLSX [D site] | code/text/PDF/images; audio/video "coming soon" [D README] |
| Data egress | Chunk texts → embedding provider; query+chunks → reranker; file contents (10K–60K tok/node) → research LLM [D+S] | **None — "your code never leaves your machine"** [D site] | **All indexed files → Mixedbread cloud store**; queries → cloud [D README] |
| Pricing | MIT tool + provider usage (Voyage 200M free tokens possible) | Free ≤3 projects/100k chunks; Pro $16/mo/seat; enterprise self-host [D codeindexer.dev/pricing] | Mixedbread: Starter **$5 one-time credits, no card**; Scale $20/mo incl. $20 credits; usage: Fast index $1.50/1M content tokens, semantic search $4/1K queries (+$3.50/1K rerank), storage $0.50/1M tok/mo [D mixedbread.ai/pricing] |
| Footprint claims | None published [U] | Demo: 412 files/198k lines → 47s, 14 MB index, "comfortable on a 16 GB laptop" [V site] | Default limits 1MB/file, 1000 files/dir [D README] |
| Benchmarks | None published | None published | "50-task benchmark… ~2x fewer tokens than grep at similar or better judged quality" (LLM-as-judge, self-reported) [V README] |

### 8.2 Unique useful scenarios for ChunkHound in YOUR stack (documented capabilities)

1. **Git-history research** — `search`/`research` over `--last-n`/`--commit-hash`/`--commit-range` with diff embeddings; "Summarize behavior changes for reviewers," changelog drafting [D README/CLI]. Neither CodeIndexer nor mgrep documents commit-range semantic search.
2. **Cited cross-file research reports** before editing — one `code_research` call "usually replaces 5–10 manual searches" [D getting-started; V phrasing theirs], runnable on your **existing Claude/Codex subscription** with no API key [S].
3. **Pinpoint web research tied into the code loop** (`websearch`, `fetchurl`) with the same provider stack [D config].
4. **Regex search with zero providers** as a keyless floor [D].

### 8.3 Exact / known-symbol / semantic RU+EN / navigation / no-answer behavior — what is actually documented

- **Exact/known-symbol:** ChunkHound: regex mode (`--regex`), symbol extraction in chunks (classes/functions per language [S]); no dedicated symbol tool — agents grep the regex tool. CodeIndexer: dedicated symbolic + exact modes in cascade with read tools (`read_chunk`, `outline`) [D]. mgrep: exact/grep mode alongside semantic [D]. **No head-to-head symbol-recall data exists publicly — [U] for all three.**
- **Semantic RU+EN:** Voyage `voyage-multilingual-2` exists (1024 dims only, in ChunkHound's registry [S]) but is **not** the default and has no free tokens; `voyage-code-4` RU-query performance is **[U]** — no published RU evaluation. CodeIndexer's local embedding model is unnamed on its site **[U]**. mgrep claims "multilingual" without language-level data [V]. **RU/EN quality for all three: UNKNOWN — this is precisely what your 24-task RU/EN canary must measure; no prior art to lean on.**
- **Navigation:** CodeIndexer is the only one with a documented call graph (find callers, blast radius) [D]; ChunkHound and mgrep rely on the agent iterating search+file reads.
- **No-answer behavior:** **No documented no-answer guarantee for any of the three.** ChunkHound failure modes that ARE documented: wrong `--db` ⇒ silent 0 results; model/dims mismatch ⇒ empty results; research erroring when it "failed to read ANY file contents" [S `pluggable_research_service.py`]; OpenRouter reasoning models can return empty content ⇒ visible error + warning note [D v6.1.0]. mgrep returns top-k regardless (rerank optional via `--no-rerank`) [D]. **False-confidence risk in cited answers is unmeasured — [U]; your no-answer task type and blinded judge are the right instrument.**
- **Failure modes summary (ChunkHound):** silent-empty searches (db path), model-switch traps, Windows WinError 183 (open), macOS arm64 Watchman startup (open #330), websearch needs Chrome ≥124 (urllib fallback weaker), LanceDB experimental, unsupported extensions skipped since v5.0.0 (coverage gate relevant), heavy dependency stack (§6) [D+S+issues].

### 8.4 Strongest case AGAINST adding ChunkHound (steelman)

1. **Your second machine cannot run it:** no `chunkhound-native` macOS x86_64 wheel or sdist exists in any release [M PyPI] — the Intel Mac Pro is locked out of v6.1.0 entirely, so any "fleet" benefit is really a two-machine (Hub + Studio) benefit at best.
2. **CodeIndexer already covers the local-first lane** (symbol/exact/cascade + call graph + memory, 100% local, zero egress, zero API cost) — ChunkHound's incremental value reduces to *cloud-semantic quality*, *git-history research*, and *cited research reports*, none of which has public quality evidence.
3. **If cloud semantics is what you want, mgrep is a lighter, purpose-built cloud-semantic challenger** (npm install, $5 free credits, no daemon/DB stack, background sync built for agents) — though with strictly more egress (full files to cloud store) and per-query pricing.
4. **Unknown resource footprint** (no published RAM/CPU numbers anywhere) against your explicit ≤256 MiB idle / ≤1 GiB peak pretrial gates — a heavy pandas/DuckDB/LanceDB daemon stack plausibly risks both gates.
5. **Privacy friction:** Voyage's zero-retention opt-out requires a payment method on file [D FAQ]; default policy permits training-use of submitted text — sanitized code makes this tolerable, but it must be a conscious acceptance, and it collides with "no new cards" if no card is already on file.
6. **Windows sharp edges persisting into the current release** (WinError 183 unchanged in v6.1.0) on your primary Hub machine.

---

## 9. Contradictory claims found (and how they were resolved)

1. **Voyage free-token coverage for rerank-2.5:** the pricing page's Rerankers intro says "The first 200 million tokens for rerank-2.5, rerank-2.5-lite, rerank-2, and rerank-2-lite are free," yet the "Older models" table (which lists rerank-2.5 at $0.05/M) says "we do not offer free tokens for them." **Unresolved on-page contradiction** — treat rerank-2.5 free-tier eligibility as unverified; prefer `rerank-3`/`rerank-3-lite` (unambiguously 200M-free) if free-tier usage matters, noting ChunkHound's default is `rerank-2.5` [S] and would need explicit `rerank_model` override. [D pricing page]
2. **Voyage free tokens for `voyage-code-4`:** the intro sentence enumerates 200M-free models without `voyage-code-4`, but the pricing table row for `voyage-code-4` shows "200 million" free tokens. The table appears authoritative but the prose omits it — verify against the dashboard before relying on it. [D pricing page]
3. **ChunkHound OpenAI default embedding model:** v5.0.0 changelog says "default OpenAI model upgraded to `text-embedding-3-large`," but v6.1.0 source (`OPENAI_DEFAULT_MODEL = "text-embedding-3-small"`) and the current config docs both say **small**. Resolution: current release behavior is `text-embedding-3-small`; the v5.0.0 change was evidently superseded without a changelog line — pin `embedding.model` explicitly to avoid drift. [D changelog vs S source + D config]
4. **"Fall back to Python pipeline when no native wheel is available" (v6.0.0 notes) vs the hard PyPI dependency:** the fallback is a *runtime* behavior; at *install* time the unconditional `chunkhound-native>=5.2.0` dependency fails resolution on platforms with no wheel and no sdist (Intel macOS). Both statements are true in their own domain — the changelog wording can mislead about installability. [D v6.0.0 notes + M PyPI + S pyproject]
5. **chunkhound.ai homepage "Proven on 50M+ line codebases in production" / "Used by companies like Applied Materials"** — vendor claim, no methodology, treat as [V], not evidence.
6. **mgrep "2× token reduction… superior performance"** — self-reported 50-task benchmark with LLM-as-judge; useful as intent, not as comparable evidence [V README].

---

## 10. Reproducible canary proposal (Q8) — isolated, additive, on-demand

**Design principle:** ChunkHound is added *on top of* the existing CodeIndexer-first baseline; nothing shared is touched; every artifact is disposable.

### 10.1 Corpus and manifest

- **Corpus:** sanitized COPIES of actual Python/JS/TS/PowerShell repos (the only languages present — all four supported by ChunkHound v6.1.0 [S]). Copy via `git clone <local-mirror> --no-hardlinks` into `canary/corpus-<arm>/`, then strip secrets with your existing sanitizer; the copy is the only writable tree.
- **Immutable manifest (shared by arms):** per-file `sha256`, **repo-relative paths**, pinned revision (`git rev-parse HEAD` of the mirror), file counts/bytes, and `chunkhound index --simulate --json` + `--check-ignores --vs git` output (built-in coverage evidence [D CLI]). Manifest hashes are common to both arms; **source stays the oracle and is never shown to responders** — responders see only the corpus copy.

### 10.2 Arms, tasks, repetition, budget (your shared protocol, ChunkHound-adapted)

- **Arms:** A = baseline (CodeIndexer only, current configs) · B = baseline + ChunkHound 6.1.0 MCP (`claude mcp add ChunkHound -- chunkhound mcp` inside the copy only [D]). ChunkHound config in the copy root: `.chunkhound.json` with `{"embedding":{"provider":"voyageai","model":"voyage-code-4","api_key":"<trial-key>"},"llm":{"provider":"claude-code-cli"}}`; **global config neutralized** (`CHUNKHOUND_GLOBAL_CONFIG_FILE` → empty file) so nothing leaks from/to shared settings [D config precedence]; analytics and remote-config left off (defaults) [D].
- **Tasks:** your **24 tasks** — 4 each known-symbol / exact / semantic RU / semantic EN / navigation / no-answer — drawn from the corpus oracle; identical questions, same model, **order counterbalanced ABBA** across pairs; **3 paired repetitions** (72 paired task-runs).
- **Context accounting:** per task, ≤**10 retrieval/read calls** and ≤**24,000 UTF-8 bytes of ALL delivered serialized tool context** (wrappers, paths, errors included). ChunkHound-specific: `code_research` returns full cited reports and will frequently blow the byte cap — **withhold over-budget messages whole** per protocol (they still count as product calls); instruct B-arm responders to prefer the `search` MCP tool with `--page-size 10` (documented default) and use `code_research` only for the semantic/research task types where its citations are the point. Count **actual successful product calls** (from `daemon_status` + session transcripts), correctness, omissions, false-confidence (blinded judge rubric), and walltime.
- **Mutation sub-protocol (only in the copy):** after each create/edit/rename/delete, **two distinct refresh treatments, run as separate sub-arms**: (i) *manual*: `chunkhound index .` (timed; this is the explicit refresh) and (ii) *watch*: daemon-mode MCP left running (watchdog backend on Hub; **do not pass `--realtime-backend watchman` on the Mac Studio** — open issue #330 [D issues]); **120-second endpoint** after mutation before the dependent query is issued; a hit on a deleted file at 120s = stale-hit failure. Hash each mutation into the manifest.
- **Resource accounting (per §6 proposal):** idle (5 min) + peak process-tree RSS/CPU for daemon+proxies, cold (first `index`) + warm (subsequent `search`) walltime, disk (`du` of `.chunkhound/db`), network via Voyage dashboard receipts (token counts cross-checked against `--verbose` logs). GPU expected and recorded as 0 (no local inference).
- **Responders/judge:** independent clean Claude Code instances per task-run (fresh `--session-dir`, no shared memory, no MCP beyond the arm's set); judge blinded to arm and to product identity.
- **Shutdown gate check:** after each B-arm session, verify **no owned process survives** (daemon exits on last disconnect [S]; confirm via process-tree snapshot 65s after close — the daemon's PID poll is 60s [S]) and that rollback = deleting the copy's `.chunkhound/` + removing the per-copy MCP registration only; **shared settings untouched by construction**.
- **Run order:** Windows Hub first; Apple Silicon Mac Studio only after the Hub shows demonstrated benefit with actual readback and independent review (per your rule); **Intel Mac Pro is excluded — v6.1.0 cannot install there** [M §5.1].

---

## 11. Pretrial gates critique (Q9) — ChunkHound-specific assessment

| Gate | Assessment vs evidence |
|---|---|
| No secret / wrong-root failures | **High-risk, testable.** `.chunkhound.json` holds the API key in the project root (docs tell you to gitignore it [D]); wrong-`--db`/wrong-root **silently returns 0 results** [D] — a hard fail condition your context-accounting will catch as zero-product-calls. Sanitizer must run before first index because chunk text goes to Voyage. |
| ≥90% coverage | **Testable with built-ins:** `index --simulate --json` and `--check-ignores --vs git` [D]. Watch `exclude_mode: gitignore_only` default (your `.gitignore` governs) and the "unsupported extensions skipped" v5.0.0 rule; PowerShell/JS/TS/Python all in-scope [S]. |
| Real independent client consumption | Verifiable via `daemon_status` + transcripts; ensure per-client proxies actually connect to the shared daemon (single DuckDB owner) rather than N `--no-daemon` servers. |
| No deleted stale hit at 120s | **Two failure sources:** manual-refresh arm fails if you forget `index` (cleanup only runs during indexing [D]); watch arm depends on backend health — use watchdog/polling on both machines (no Watchman runtime on macOS arm64, open #330; Windows Watchman runtime exists [D issues]). Windows file-handle delays special-cased [S]. |
| Idle RSS ≤256 MiB added / peak ≤1 GiB | **Genuinely at risk:** no public numbers [U]; the stack imports pandas, scipy, scikit-learn, DuckDB, LanceDB, PyArrow into a resident daemon [S pyproject]. If exceeded, either justify in advance with measured numbers from the trial's own instrumentation or PARK — do not hand-wave. |
| No persistent owned process on on-demand shutdown | Source-supported but must be **observed**: daemon `on_empty` shutdown + 5s orphan poll in proxies + 60s PID poll [S]; test abnormal client kill too. |
| KEEP/PARK thresholds | Your thresholds (no new confidently false answers; correctness decline ≤1/24 with ≥20% median walltime **or** host-CPU improvement; or correctness +≥3/24 with ≤20% time increase) are sound and match the UNKNOWN-quality reality — no public data would justify pre-trial KEEP. `code_research`'s large outputs will interact with the 24,000-byte cap (withheld-whole messages) — pre-register that withheld ≠ benefit. |
| TRY vs PARK | **TRY is the right posture if functional** (installs, indexes, searches, refreshes within gates) with evidence incomplete — which is exactly the state of public evidence today. **PARK triggers already defined** (hard-gate failure, no incremental benefit, unauthorized cost) — plus two ChunkHound-specific PARK triggers I'd add: (a) inability to obtain a Voyage key under existing-credits/no-card policy; (b) inability to set the training-data opt-out if your policy requires zero-retention and no payment method exists on file [D FAQ]. |
| Fleet rules | Agree and reinforce: installation/ACK/rc0 ≠ PASS; Hub-first; other OS only after demonstrated benefit + readback + independent review; rollback limited to the isolated process/client. Given the Intel-mac blocker, "fleet" here can only ever mean Hub + Mac Studio. |

---

## 12. Recommendation + strongest counterargument

**Recommendation: TRY — a tightly-scoped, Hub-first canary under the §10 protocol, with PARK triggers armed.**

Rationale: ChunkHound v6.1.0 is a real, actively-maintained (v6.1.0 released 2026-10-20; 40+ releases since 2025-06), MIT-licensed, local-first tool whose *unique documented capabilities* (git-history semantic research; cited `code_research` on your existing Claude/Codex subscription; keyless regex floor) are genuinely additive to a CodeIndexer-first stack, and whose embedding costs can plausibly be $0 within Voyage's 200M free tokens. But every quality/footprint claim that would justify adoption is currently **UNKNOWN**, two of your three machines have concrete blockers/sharp edges (Intel Mac Pro: no installable native wheel [M]; Windows: WinError 183 unchanged [D]), and the privacy opt-out needs a card on file [D]. That is precisely the "functional but evidence incomplete" profile your TRY tier exists for — and exactly what the 24-task × 3-repetition counterbalanced canary with blinded judge is designed to resolve. Configure `voyage-code-4` from the first index (free-tier + code-specialized [D]), `claude-code-cli` for research [S], watchdog/polling realtime backend, analytics/remote-config off, and measure everything per §6.

**Strongest counterargument (the case for PARK/no-trial):** the challenger's only defensible edge over your incumbent stack is *cloud-embedding semantic quality and cited research* — yet (a) that edge is unproven anywhere publicly (no benchmarks, no RU/EN data, no footprint numbers [U]); (b) the incremental machine coverage is 2 of 3 (Intel Mac Pro hard-blocked [M]); (c) CodeIndexer already owns the local lane you trust, and mgrep already owns a cheaper-to-trial cloud-semantic lane with a $5 no-card credit [D] if cloud semantics is the hypothesis you actually want to test; and (d) accepting Voyage's default training-use policy (or adding a card to enable opt-out) is a real policy cost for a tool whose benefit is unmeasured. If the DR's job is to protect an already-working workflow from churn, the expected value of trialing ChunkHound before mgrep — or before simply improving CodeIndexer queries — is not clearly positive. The counter-counterpoint: only your own corpus can settle quality, and the canary's cost is near-zero within free tokens — which is why TRY, not PARK, is the recommendation.

---

## 13. Unanswered questions (with the reason each is unanswered)

1. **Runtime RAM/CPU/GPU/network for index/search/update** — no public measurements exist; must come from the §6 protocol. [U]
2. **Installed on-disk size of the full dependency stack and of the index DB** — only download sizes computed [M]; install size unmeasured. [U]
3. **OpenAI-direct and Anthropic API list prices / trial credits / expiry** — pricing pages blocked (Cloudflare/region) from this sandbox. [U]
4. **Voyage free-token expiry and account-level rate limits** — not stated on the pricing/FAQ pages opened; rate-limits page not opened. [U]
5. **Whether rerank-2.5 and voyage-code-4 actually receive the 200M free tokens** — on-page contradictions (§9.1–9.2); needs dashboard verification. [U]
6. **Cross-OS portability of a built `.chunkhound` DB (Windows→macOS ARM)** — relative-path storage suggests yes [S+I], but it is undocumented and untested; canary must test before relying on it. [U]
7. **RU-query semantic quality for voyage-code-4 / CodeIndexer's local embeddings / mgrep** — no published evaluation for any. [U]
8. **`code_research` false-confidence rate on no-answer tasks** — no data; the no-answer task type + blinded judge exist in your protocol precisely for this. [U]
9. **Whether DuckDB concurrent access (CLI `index` while daemon serves) fast-fails cleanly on Windows in practice** — serial-executor fast-fail exists in-process [S]; cross-process behavior + the open WinError 183 path need empirical confirmation. [U]
10. **Voyage account creation without a payment method, under your existing-credits policy** — signup flow not verifiable without an account. [U]
11. **ChunkHound governance/ownership trajectory** (PyPI ownership changed hands relative to the original 2025 project; current PyPI owners `Hanan_kavitz`/`ofriw` [M]) — no statement opened; relevant only to supply-chain risk appetite. [U]

---

**Method note:** every URL in §2 was opened during this DR (2026-11-01, UTC); implementation claims are pinned to git tag `v6.1.0` commit `c3b332b16967636a39e26f2746df28d03d31f861`; rolling docs were distinguished from pinned source throughout; no measurements were invented — all runtime performance figures are explicitly UNKNOWN pending the proposed measurement protocol. Nothing was installed, configured, or purchased, per your constraints.
