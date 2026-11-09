# DR26-11-01-HUB-05-1439 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-11-01-HUB-05-1439 — grepai remote-embeddings operational decision

**Research date:** 2026-11-01, Europe/Lisbon  
**Target identity:** `yoanbernabeu/grepai`, not “GroupEye Cloud”  
**Decision scope:** additive, on-demand grepai challenger to an existing CodeIndexer-first workflow on Windows Hub, Intel Mac Pro, and Apple Silicon Mac Studio; Claude Code, Codex, Cursor; remote embeddings; minimum total local CPU/RAM/GPU; no new paid commitment or shared-config mutation during pilot.

## A. Identity, implementation, architecture, and license

**Identity is verified. [established]** The product is Yoan Bernabeu’s `grepai`, whose owning repository is [github.com/yoanbernabeu/grepai](https://github.com/yoanbernabeu/grepai) and documentation site is [yoanbernabeu.github.io/grepai](https://yoanbernabeu.github.io/grepai/). The repository describes grepai as a Go semantic-code-search CLI with vector search, call-graph functionality and MCP integration; the current project page identifies the latest release as v1.2.0. GitHub/pkg.go.dev independently resolve the module to the same repository and v1.2.0. GitHub repository/release pages were observed 2026-11-01; pkg.go.dev lists publication on 2026-10-30. citeturn341196search1turn190542search1turn566048search1

**Stable release: v1.2.0, released 2026-10-30. [established]** The owning GitHub release page marks v1.2.0 as Latest and dates the release 2026-10-30; the release commit is `d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef`. [Release v1.2.0, 2026-10-30](https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0) and [release commit](https://github.com/yoanbernabeu/grepai/commit/d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef). citeturn162984view0turn138612view1 A separate Chocolatey package points to the same official v1.2.0 Windows release asset, but that package was still “ready for review” rather than approved, so it is supplementary only [low-authority]. citeturn190542search2

**The immutable revision I would use for this pilot is the v1.2.0 release SHA above, not moving `main`. [established]** At research time GitHub indicated one commit existed on `main` after the release, but I could not obtain a reliable primary-source rendering of that newer full SHA through the public web interface. Therefore: **stable source revision = `d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef`; exact current `main` HEAD = [no source found], unverified.** This distinction matters because the canary should pin the release artifact or release SHA rather than silently testing an unreleased commit. citeturn162984view0turn138612view1

There is a documentation inconsistency worth preserving. The freshly crawled home page says “Latest release v1.2.0” near the top, but another generated section of the same site still says “Latest: v1.1.0 (Oct 26, 2026).” GitHub Releases is the owning release authority and therefore wins. **Documentation is partially stale. [established]** [Project home, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/). citeturn190542search0turn997127search4

**License: MIT. [established]** The immutable v1.2.0 `LICENSE` is the MIT License, Copyright 2026 Yoan Bernabeu. [v1.2.0 LICENSE](https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/LICENSE). citeturn162984view3

**Runtime/build. [established]** v1.2.0 `go.mod` requires Go 1.25.5 and directly includes, among others, `fsnotify`, `pgx`, `pgvector-go`, the Qdrant Go client, `mcp-go`, Cobra, gRPC/protobuf and a Go Tree-sitter binding. [v1.2.0 go.mod](https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/go.mod), revision 2026-10-30. citeturn162984view2 Official installation documentation says release binaries are built without CGO; an experimental Tree-sitter AST build needs `CGO_ENABLED=1` and the `treesitter` build tag. [Installation documentation, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/installation/). citeturn503637search3turn190542search6

The architecture is more prosaic than the phrase “grepai cloud” implies. **There is no evidence of a grepai-operated hosted indexing/search SaaS. [established as a negative search finding, single-source per individual implementation fact]** The official design is a CLI that reads source files, chunks them, calls a configured embedder, and stores the resulting chunks/vectors in one of its backends: local GOB, PostgreSQL/pgvector, or Qdrant. Search embeds the query and asks the selected vector store for nearest chunks. Official architecture/configuration describes remote **embedding providers** and optionally remote Postgres/Qdrant, not a hosted grepai service. I found no official grepai-hosted indexing/search API, account system, retention policy or vendor cloud control plane. [Getting Started](https://yoanbernabeu.github.io/grepai/getting-started/), [Workspace Management](https://yoanbernabeu.github.io/grepai/workspace/). citeturn997127search3turn997127search11

The actual data placement is therefore:

| Artifact | GOB + remote embedder | Remote PostgreSQL/Qdrant | Confidence / evidence |
|---|---|---|---|
| Original repository files | local filesystem; read by grepai | same | [established], official implementation/docs. citeturn997127search3 |
| Code/chunk text during indexing | sent to embedding provider | sent to embedding provider | [established]; the embedder request contains chunk text, and embedding input is prefixed with the file path (`File: …`). [source configuration/chunker, v1.2.0](https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/config/config.go). citeturn794878view3turn697433view1 |
| Query text | sent to embedding provider on every semantic search | same | [established]; search first embeds the query. [Semantic Search, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/search-guide/). citeturn503637search2turn997127search5 |
| Vectors + chunk metadata/content | `.grepai/index.gob` locally | selected DB | [established]. citeturn997127search6turn997127search11 |
| Similarity computation | local GOB scan | database-side query/search | [established for GOB; remote-backend placement is architectural inference]. Official docs explicitly warn that the GOB backend scans vectors in memory. citeturn443946search1 |
| Symbol/call index | `.grepai/symbols.gob` locally | still per-project `.grepai/symbols.gob` | [established]. citeturn997127search11turn997127search18 |
| RPG graph | local `.grepai/rpg.gob` unless implementation/config says otherwise | not established as remotely shared | [emerging, single-source] from v1.2.0 config constants. citeturn697433view0 |

**Remote storage does imply remote vector search for PostgreSQL/Qdrant, but remote embeddings alone do not.** With the proposed remote-embeddings + GOB layout, the expensive model inference leaves the machine, but vector persistence and similarity scanning remain local. [established/inference from implementation]. citeturn443946search1turn997127search11

## B. Remote-embedding path, privacy, entitlement and cost

v1.2.0 implements OpenAI-compatible adapters for **OpenAI, OpenRouter, Synthetic, and Requesty** in addition to Ollama and LM Studio. Default endpoints/models in the owning config source are OpenAI `text-embedding-3-small`, OpenRouter `openai/text-embedding-3-small`, Synthetic `hf:nomic-ai/nomic-embed-text-v1.5`, and Requesty `openai/text-embedding-3-small`. [v1.2.0 config source](https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/config/config.go), 2026-10-30. citeturn697433view0

For OpenAI, grepai’s implementation has default parallelism 4, request timeout 60 s, retry machinery for 429/5xx, a batch safety limit of 2,000 inputs and an estimated 280,000-token batch target; retries use exponential backoff with jitter and a maximum attempt count of five in the default retry configuration. The project’s token estimate is approximately four characters per token, so it is a batching heuristic, not an authoritative provider token count. [OpenAI embedder/retry/batch source, v1.2.0](https://github.com/yoanbernabeu/grepai/tree/v1.2.0/embedder). citeturn489174view4turn489174view5turn489174view6turn794878view0turn794878view1 OpenRouter, Synthetic and Requesty use their respective OpenAI-compatible `/embeddings` endpoints; the inspected adapters do not expose the same parallel/retry path as the OpenAI adapter, so assuming identical resilience would be unjustified. [established, single-source per adapter]. citeturn697433view2turn697433view3turn697433view4

A material 1.2.0 security change affects configuration. Prior versions could take an environment fallback API key and send it to an endpoint specified by a repository-controlled `.grepai/config.yaml`. PR #411, merged 2026-10-30, changed this so environment-derived keys are only sent automatically to the exact official HTTPS provider hosts; custom endpoints require an explicit configured key. The same source says grepai does **not** expand `${<key-var>}` inside YAML. [PR #411, merged 2026-10-30](https://github.com/yoanbernabeu/grepai/pull/411). citeturn138612view2 This conflicts with older/example documentation patterns that imply shell-style interpolation; for the pilot I would trust the current source and **omit `api_key:` entirely when using an official endpoint**, passing the secret only in the process environment. [established, source-over-docs resolution]. citeturn138612view2

That fix does **not** make arbitrary source safe to transmit. The scanner’s built-in extension list includes `.env`, while PowerShell extensions are not in the inspected default list. Consequently a default semantic index can ingest a file type that often contains secrets, while ignoring `.ps1` unless customized. [established, single-source] [scanner source, v1.2.0](https://github.com/yoanbernabeu/grepai/blob/v1.2.0/indexer/scanner.go). citeturn794878view4 For your policy, the canary needs explicit deny patterns before the first index, not a post-hoc secret scrub.

A suitable **isolated-pilot** YAML is therefore:

```yaml
embedder:
  provider: openai
  model: text-embedding-3-small
  endpoint: https://api.openai.com/v1
  dimensions: 1536
  parallelism: 4
  request_timeout_seconds: 60
  max_retries: 5
  max_batch_size: 2000
  max_batch_tokens: 280000

store:
  backend: gob

chunking:
  size: 512
  overlap: 50
  strategy: fixed
  custom_extensions:
    - .ps1
    - .psm1

search:
  boost:
    enabled: true
  hybrid:
    enabled: false
  dedup:
    enabled: true
```

Those defaults/fields derive from v1.2.0 source; **PowerShell custom extensions are my proposed addition**, not an upstream default. citeturn489174view0turn489174view2turn697433view0turn794878view4 Use an ephemeral environment variable only in the invoking shell, for example:

```powershell
$env:<key-var> = (Get-Content .\pilot-secret-location\openai-key.txt -Raw).Trim()
grepai index
grepai search "where is session validation enforced?" --json --compact
Remove-Item Env:<key-var>
```

Do **not** commit the key, put it in `.grepai/config.yaml`, or use `api_key: ${<key-var>}`. The last form is not interpolated by current grepai source. citeturn138612view2

For the sanitized snapshot, use `.grepaiignore` plus an independent preflight secret scan. grepai officially honors `.gitignore` and `.grepaiignore`; the specific deny set below is my operational proposal, not an upstream canonical list. citeturn997127search3

```gitignore
.env
.env.*
*.pem
*.key
*.p12
*.pfx
*.jks
*.keystore
*.cookie
cookies*
credentials*
secrets*
.npmrc
.pypirc
.netrc
.aws/
.azure/
.gnupg/
.ssh/
**/auth.json
**/token*.json
**/*secret*
**/*credential*
**/vendor/**
**/third_party/**
```

### Provider data handling and entitlement

**OpenAI direct. [established]** As of 2026-11-01, `text-embedding-3-small` is **$0.02 per 1M input tokens**, and `text-embedding-3-large` is **$0.13 per 1M input tokens**. Default vector widths are 1536 and 3072 respectively. [OpenAI model page, observed 2026-11-01](https://developers.openai.com/api/docs/models/text-embedding-3-small) and [Embeddings guide](https://developers.openai.com/api/docs/guides/embeddings). citeturn341196search2turn341196search3 OpenAI states API data is not used for training by default unless the customer opts in; `/v1/embeddings` is covered by API data controls, with abuse-monitoring retention generally up to 30 days and Zero Data Retention available to qualifying configurations. [OpenAI data controls, observed 2026-11-01](https://developers.openai.com/api/docs/guides/your-data). citeturn341196search4turn621866view2

A ChatGPT subscription is **not equivalent to API billing entitlement. [established]** OpenAI documents API billing separately. There is, however, a current promotional API-credit program for some eligible paid ChatGPT plans; eligibility and activation are account-specific. Therefore, **do not infer available embedding credit merely because Claude/ChatGPT/Codex subscriptions exist.** A zero-new-spend OpenAI pilot is valid only if the existing API organization already shows a usable grant/credit balance and a working API key; otherwise the budget gate remains closed. [OpenAI API credits documentation, observed 2026-11-01](https://developers.openai.com/api/docs/guides/your-data) via the “API credits for ChatGPT subscribers” documentation navigation and current OpenAI billing material. citeturn621866view2

**OpenRouter. [established for platform terms; model-specific embedding price partly unresolved]** The current Free tier advertises 25+ free models and 50 requests/day; Standard is pay-as-you-go with a 5.5% platform fee. OpenRouter says it does not train on customer data, while provider-side retention/training depends on the selected provider; Zero Data Retention routing is available. [OpenRouter pricing, observed 2026-11-01](https://openrouter.ai/pricing). citeturn621866view3turn341196search5 I found no primary evidence that grepai’s default `openai/text-embedding-3-small` route is a free-model entitlement, so a zero-new-spend OpenRouter path cannot be presumed. **Default small-model effective rate through OpenRouter: [no source found] in the retrieved current model page; verify the model route/account before use.**

**Synthetic. [established, single-source policy/provider documentation]** Synthetic currently lists `hf:nomic-ai/nomic-embed-text-v1.5` as included in every subscription, with embeddings not counting against subscription rate limits and no additional embedding charge. [Synthetic models](https://dev.synthetic.new/docs/api/models), [embeddings endpoint](https://dev.synthetic.new/docs/openai/embeddings), observed 2026-11-01. citeturn341196search6turn341196search7 Its 2026-01-01 privacy policy says API prompts/completions are not stored without explicit consent and are deleted after processing, and that its inference partners are selected/configured not to retain them. [Synthetic privacy policy, 2026-01-01](https://synthetic.new/policies/privacy/2026-01-01). citeturn341196search8 **If you already have an eligible Synthetic subscription**, this is the clearest evidenced zero-incremental-charge route among the grepai-supported providers. Whether you personally have that entitlement is unknown.

**Requesty. [established for policy, single-source provider claim]** Requesty’s 2026-08-14 privacy policy says self-service logging can retain prompts/outputs encrypted in the EU for up to 30 days; logging can be disabled, while organization-wide ZDR is available on paid plans by request. It also explicitly warns that an upstream model provider may separately retain or train on content. Its own training treatment differs between free and paid accounts. [Requesty privacy policy, 2026-08-14](https://www.requesty.ai/privacy). citeturn341196search9turn621866view4 Requesty advertises a $0/free tier and free models, but I did not find evidence that grepai’s configured `openai/text-embedding-3-small` is one of those free routes. **Zero-new-spend for that exact grepai path: [no source found].**

### Cost formulas

Let `T` be **actual embedding-input tokens**, not raw repository tokens. grepai duplicates overlap and includes a `File: <path>` prefix in embedding text, so raw-code-token count is not identical to provider-billed embedding tokens. citeturn794878view3 For direct OpenAI:

`initial_cost = T / 1,000,000 × provider_rate`

At current rates, [established calculation from published pricing]:

| Embedding input | OpenAI small, $0.02/M | OpenAI large, $0.13/M |
|---:|---:|---:|
| 100,000 tokens | **$0.002** | **$0.013** |
| 1,000,000 tokens | **$0.020** | **$0.130** |

Pricing source: OpenAI, observed 2026-11-01. citeturn341196search2turn341196search3

For 1,000 searches averaging `q` query tokens:

`query_cost_small = 1,000 × q / 1,000,000 × $0.02 = $0.00002 × q`

`query_cost_large = $0.00013 × q`

No average `q` is assumed. Every semantic query requires an embedding request, so this cost is recurrent rather than just index-build cost. citeturn503637search2

For incremental churn of `ΔT` embedding tokens:

`incremental_cost = ΔT / 1,000,000 × rate`

v1.2.0 intentionally re-embeds moved/renamed content because file path is part of embedding input; same-path unchanged chunks can reuse cached vectors. Thus a rename can have non-zero cloud cost even when source bytes are unchanged. [Release 1.2.0 notes, 2026-10-30](https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0). citeturn138612view1

For retries, a safe accounting expression is:

`retry_cost ≤ successful_input_cost × (1 + number_of_additional_billable_attempts)`

Whether a particular failed 429/5xx attempt is billed by each provider is **UNKNOWN** from the sources reviewed; do not treat retry count mechanically as charged token count. grepai can retry OpenAI requests up to its configured attempt policy. citeturn794878view0turn489174view4

**Budget gate:** before the first remote index, verify an already-existing account shows a non-zero, usable credit/subscription allowance and configure a hard provider-side spend cap where available. If none exists, **do not run the remote pilot** until explicit spending authorization. That is the only interpretation consistent with “no new cards/plans/metered charges.”

## C. Search behavior, languages, agents, and process lifetime

grepai is primarily **semantic vector search**, not a replacement for exact grep. It embeds a query, retrieves nearest chunks, optionally merges a simple lowercase keyword match using Reciprocal Rank Fusion, applies path boosting, optionally de-duplicates to one best chunk per file, then truncates to the requested limit. Hybrid search is disabled by default; dedup and structural boost are enabled in newly generated current configs. [Semantic Search](https://yoanbernabeu.github.io/grepai/search-guide/), [Hybrid Search](https://yoanbernabeu.github.io/grepai/hybrid-search/), observed 2026-11-01. citeturn503637search2turn443946search9

The project itself now explicitly advises exact-text tools for exact identifiers. The 0.36.1 changelog says agent instructions were changed away from absolute “never use grep” guidance because semantic search provides a ranking rather than an exhaustive result set. [CHANGELOG, 2026-09-01 entry](https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md). citeturn566048search0 This supports the intended **CodeIndexer-first/additive**, rather than replacement, topology.

Path restriction is first-class via `--path`, relative to the project root. Single-project dedup can suppress multiple chunks from one file; workspace search has historically differed by disabling some project-level ranking/dedup configuration, so workspace results should not be assumed identical to project CLI results. [Search guide, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/search-guide/). citeturn443946search1turn997127search5

**No-answer behavior needs a canary test. [emerging]** The current docs say “No results found” if the backend returns nothing, but I found no documented, configurable semantic relevance threshold that turns low-similarity top-k hits into “I do not know.” Result scores are described as relative; with hybrid RRF they use a different scale, and boosted vector scores can exceed 1.0. [Semantic Search, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/search-guide/). citeturn503637search2 Therefore **“no-answer calibration” must be measured**: a store with any candidates may still surface irrelevant nearest neighbors rather than abstain. An open reranker feature request reported tightly compressed similarity scores, but that issue is user-generated rather than an authoritative product guarantee [low-authority, single-source]. [Issue #197, 2026](https://github.com/yoanbernabeu/grepai/issues/197). citeturn566048search8

### Language distinction: ingestion is not semantic parsing

The default scanner directly recognizes `.py`, `.js`, `.jsx`, `.ts`, `.tsx` and many other text/code extensions. **It does not list `.ps1`/`.psm1` in the inspected v1.2.0 defaults. [established, single-source]** PowerShell can be made **chunk-ingestable** with `chunking.custom_extensions`, but that says nothing about call-graph or symbol-parser support. [v1.2.0 scanner/config source](https://github.com/yoanbernabeu/grepai/blob/v1.2.0/indexer/scanner.go). citeturn794878view4turn489174view2

Python/JS/TS therefore have two separable capabilities:

1. Their files can be chunked and embedded for semantic retrieval.
2. Some languages have symbol/call extraction for `trace`.

Those should not be conflated. PowerShell in the proposed canary is **semantic-chunk search only unless source inspection or measurement demonstrates a trace extractor.** I found no official PowerShell trace support. [no source found for PowerShell trace support]

For RU versus EN, upstream grepai documentation explicitly recommends English queries because its historical/default Nomic embedding setup is mainly optimized around English. [Quick Start/agent guidance, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/quickstart/). citeturn566048search4turn997127search2 OpenAI, by contrast, describes its third-generation embeddings as having improved multilingual performance. [OpenAI embeddings guide, observed 2026-11-01](https://developers.openai.com/api/docs/guides/embeddings). citeturn341196search3 **Actual RU→English-code retrieval quality is model- and repository-dependent and remains UNKNOWN until measured.** There is no defensible source-based multiplier for RU quality.

### Agent integration and lifetime

`grepai mcp-serve` is a **stdio MCP server** exposing search, trace, refs, RPG and index-status tools. Official documentation gives configurations for Claude Code and Cursor and the main site lists Codex CLI among supported agents. [MCP command reference, generated 2026-10-30](https://yoanbernabeu.github.io/grepai/commands/grepai_mcp-serve/), [home page, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/). citeturn997127search0turn997127search4 An MCP stdio process lives for the client session, so it conflicts with your “no permanent stdio MCP per session” pilot constraint. The pilot should call `grepai search ... --json --compact` as a **one-shot CLI**, not register MCP globally.

`grepai index` performs one scan/update and exits; no watcher or daemon is required. [command reference, generated 2026-10-26](https://yoanbernabeu.github.io/grepai/commands/grepai_index/). citeturn997127search1 This is the lowest-idle-resource topology:

```text
CodeIndexer = baseline owner
       |
       +-- when explicitly invoked:
             sanitized snapshot
                  |
             grepai index          [process exits]
                  |
             grepai search ...     [one process per query; exits]
                  |
             remote embedding API
                  |
             local .grepai/index.gob
```

No Ollama, LM Studio, Qdrant daemon, PostgreSQL daemon, router, watcher, MCP process or extra guard service is required.

Cold/warm one-shot timings, process startup RAM, GOB load time, and whether OS page cache materially changes repeated CLI latency are **UNKNOWN** and belong in the canary; upstream docs do not provide comparable cross-platform measurements.

## D. Call graph versus LSP semantics

This is where grepai should be treated most skeptically.

Official historical documentation distinguishes **fast regex extraction** from a Tree-sitter **precise** mode. The changelog says call tracing initially supported regex extraction for Go, JavaScript/TypeScript, Python and PHP, with Tree-sitter integration behind a `treesitter` build tag. [CHANGELOG](https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md). citeturn503637search4 Current release installation documentation says official prebuilt binaries are built without CGO; Tree-sitter functionality requires a source build with `CGO_ENABLED=1 -tags treesitter`. [Installation docs, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/installation/). citeturn503637search3

More importantly, PRs proposing to “wire up --mode dispatch and ship tree-sitter by default” and add Tree-sitter extraction for additional languages were still open in the recently crawled pull-request list. [PR list, May 29 2026 proposals, observed 2026-11-01](https://github.com/yoanbernabeu/grepai/pulls). citeturn503637search5 That is disconfirming evidence against reading broad “precise mode” documentation as proof that the v1.2.0 release binary is doing Tree-sitter call resolution by default.

**Syntax precision is not semantic resolution. [established]** Even Tree-sitter can parse syntax accurately without resolving types, imports, method dispatch, overload sets or runtime polymorphism. grepai’s own 1.2.0 release notes acknowledge a concrete limitation: same-name definitions now have deterministic selection by file/line, but callees across same-named definitions can still be merged and `Class::method`-style scoping remains unsupported. [v1.2.0 release/commit notes, 2026-10-30](https://github.com/yoanbernabeu/grepai/commit/d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef). citeturn138612view1turn138612view5

The fast extractor source is regex/name oriented rather than a language-server-style binding engine. [trace extractor source, v1.2.0](https://github.com/yoanbernabeu/grepai/blob/v1.2.0/trace/extractor.go). citeturn425223view0 Consequently:

| Construct | What can safely be claimed for grepai 1.2.0 | Confidence |
|---|---|---|
| Unique free function call | often detectable by regex/syntax | [emerging]; correctness still needs fixture |
| Two same-named functions in different files | deterministic selection improved, but merged callees remain possible | [established]. citeturn138612view1 |
| Class methods with same method name | explicit scoping limitation | [established]. citeturn138612view1 |
| Import alias / re-export | no type/module-resolution guarantee found | [speculative as capability]; [no source found] |
| TS overloads/interfaces | no semantic overload-resolution engine found | [speculative as capability]; [no source found] |
| Inheritance/virtual dispatch | no dispatch/type hierarchy guarantee found | [speculative as capability]; [no source found] |
| Python monkey patching/dynamic attributes | no runtime resolution | [established as limitation by architecture/inference] |
| JS/TS computed/dynamic dispatch | no general semantic resolution demonstrated | [established as limitation by architecture/inference] |
| Callbacks passed as values | depends on superficial occurrence/extractor rules | [emerging], measure |
| Decorators | syntax may be visible; semantic target/effect not guaranteed | [emerging], measure |
| PowerShell calls | no supported trace extractor found | [speculative/unsupported]; [no source found] |

A command that **displays callers/callees is not evidence that the edges are correct**. For the canary, compare against both source truth and a native language server where one exists. The fair fixture is a deliberately adversarial mini-repository containing: two same-named functions in separate modules; aliased imports; class and instance methods sharing names; TS interface/implementation and overload; Python decorator and callback; JS higher-order callback; inheritance/override; dynamic object property call; and PowerShell function/script invocation. Seal expected edges manually from source and, separately, collect LSP definitions/references. Score grepai on true-positive/false-positive/false-negative **edges**, not whether it printed a plausible graph.

**Does precise mode actually participate in ordinary indexing? Unresolved. [speculative]** Current one-shot indexing definitely rebuilds the symbol index, but the current persisted `TraceConfig` source I inspected exposes enabled languages without an obvious persistent “precise mode” selector, while current release binaries lack CGO. I did not obtain owning-source proof that a standard v1.2.0 `grepai index` binary invokes the Tree-sitter extractor. **[no source found]** Until fixture evidence says otherwise, treat official v1.2.0 binary call graphs as the lightweight path, not “LSP-lite.”

## E. Freshness, mutation correctness, persistence, and cross-OS risk

A watcher is **not necessary** for your intended use. `grepai index` scans/updates once and exits using the same incremental logic as the watcher’s initial scan. `grepai watch` additionally monitors create/modify/delete/rename and debounces changes; current documentation states a 500 ms default. [Index command, 2026-10-26](https://yoanbernabeu.github.io/grepai/commands/grepai_index/); [Watch command](https://yoanbernabeu.github.io/grepai/commands/grepai_watch/). citeturn997127search1turn997127search9

Freshness has had several recent correctness fixes, which is simultaneously positive evidence of maintenance and a reason not to assume this area is mature:

| Failure mode | Evidence/status |
|---|---|
| Atomic editor writes could remove active files from index | fixed in 0.36.1; the bug also reproduced an index dropping to zero files on branch checkout. [CHANGELOG, 2026-09-01](https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md). citeturn566048search0turn997127search10 |
| Corrupted/truncated GOB | 0.36.1 added atomic write/quarantine recovery. citeturn566048search0 |
| Concurrent watchers | v1.2.0 changed daemon PID/writer locking so the lifetime lock is held correctly. [Release 1.2.0, 2026-10-30](https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0). citeturn162984view0 |
| Embedding failure during update could make prior chunks disappear | v1.2.0 now retains old chunks until replacement embeddings succeed. citeturn138612view1 |
| Rename/move could reuse a vector embedding the old path | v1.2.0 stops cross-path reuse; moved files are re-embedded. citeturn138612view1turn138612view4 |
| Delete could leave stale document IDs | fixed in v1.2.0. citeturn162984view0 |
| Qdrant collection collision across projects | v1.2.0 added project namespacing; branches sharing the same project identity can still overwrite one another. citeturn138612view1 |
| Watch event queue under embedding load | an Oct 2026 issue still tracks backpressure behavior; explicit freeze/unfreeze was rejected for 1.x stabilization. [Issue #349, owner response 2026-10-30](https://github.com/yoanbernabeu/grepai/issues/349). citeturn566048search3 |

This recent bug history argues strongly for **one-shot immutable-snapshot indexing in the pilot**, not a live working-tree watcher.

The current watcher uses a nonblocking per-project lifetime writer lock at `.grepai/writer.lock`; searches/MCP/trace are read-only with respect to that lifetime lock. [File Watching docs, observed 2026-11-01](https://yoanbernabeu.github.io/grepai/watch-guide/). citeturn997127search6 That protects against two watchers clobbering one local index, but it does not make a shared index a sensible multi-user coordination database.

**Repository identity is backend-sensitive.** v1.2.0’s Qdrant fix derives namespacing from normalized Git origin/subdirectory where possible or absolute project path otherwise; the release notes explicitly warn that different branches can still overwrite one another when they intentionally resolve to the same project. citeturn138612view1 Therefore do not share a Qdrant collection across canary branches/worktrees. GOB avoids that entire remote-shared-state class.

Paths are stored project-relative for search/display, and Windows-path normalization has received explicit fixes, including Windows Qdrant collection-name sanitization in 0.36.1. citeturn566048search0turn997127search5 I did not find an official guarantee that a `.grepai/index.gob` created on Windows is supported as a portable artifact when copied to Intel macOS or Apple Silicon, nor a guarantee covering case-folding differences across arbitrary filesystems. **Cross-OS index portability: [no source found].** The safe second-OS protocol is therefore to move the **same source snapshot + manifest**, not the GOB, and independently re-index it on each OS.

Ignore semantics use `.gitignore`/`.grepaiignore`, but because `.env` is itself among recognized extensions, exclusion must be validated before any API request. citeturn997127search3turn794878view4 A pilot should fail closed when the secret scanner finds any token-like artifact rather than depend on filename patterns alone.

The rename question is now clear: **rename/move re-embeds. [established]** This is deliberate because `File: <path>` participates in embedding text. That improves vector consistency but means path-only churn incurs provider calls. citeturn138612view1turn794878view3

## F. Resource/cost evidence and the 97% claim

The headline **“97% fewer input tokens” is not suitable as a fleet decision statistic. [established]** The owning benchmark, published 2026-01-21 by the grepai maintainer, compared two Claude Code sessions on a 155k+ line Excalidraw TypeScript repository, with five exploration questions in a fixed order. Baseline used ordinary Claude Code; treatment used grepai plus local Ollama/Nomic on an M3 Pro. Reported results were 51,147 versus 1,326 input tokens, $6.78 versus $4.92, and 139 versus 62 tool calls. The author explicitly discloses that the benchmark was maintainer-run. [Author benchmark, 2026-01-21](https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code/). citeturn621866view0

The strongest case **for** that benchmark is that it used identical clones, the same five tasks and Claude Code log extraction including subagents, rather than an anecdotal stopwatch. citeturn621866view0 The case **against** treating 97% as a generalizable effect is stronger: one TypeScript repository, five hand-selected semantic-discovery tasks, maintainer-run experiment, treatment tooling available to the agent but no evidence here of blinded answer judging, no counterbalanced run order described in the retrieved methodology, and no accounting comparable to your requirement to count **all serialized delivered context including wrappers/echoes**. It also used local Ollama, not the proposed remote provider, and says nothing about Windows/Intel Mac/Apple Silicon remote-embedding costs. citeturn621866view0 I found no evidence that source truth was sealed from task authors or that question construction was independent of knowledge of grepai’s strengths. Therefore oracle/task-selection contamination cannot be excluded. **[speculative]**

I found one recent independently authored reproducible benchmark repository, `kostchei/semantic-coding`, which tested grepai embeddings on 304 files/3,102 chunks and reports disk size, latency and local VRAM for two *local* models. It reports 17.2 MB vs 66.2 MB indexes, ~121 ms vs ~302 ms query latency and ~548 MB vs 7.52 GB VRAM for its two tested models. [Third-party repository, observed 2026-11-01](https://github.com/kostchei/semantic-coding). citeturn621866view1 This is useful only as evidence that grepai’s resource profile can vary drastically with embedding model and vector width; it is **not** directly applicable to remote embeddings, and its benchmark extensions/hybrid machinery go beyond plain upstream grepai. [low-authority, single-source]

For your requested fields, current evidence is:

| Resource field | Remote embedding + local GOB | Evidence status |
|---|---|---|
| Local embedding CPU | model inference removed from local host | [established inference] |
| Local embedding GPU/VRAM | no local embedding model required | [established inference]; **grepai GPU use itself not independently measured** |
| Idle RAM with one-shot CLI | effectively no resident grepai process after exit | [established architectural consequence]; OS cache remains |
| Peak indexing RAM | **UNKNOWN** |
| Peak search RAM | **UNKNOWN** |
| GOB vector RAM | non-zero; GOB search scans vectors in memory | [established]. citeturn443946search1 |
| Hybrid-search additional RAM | text matching loads chunks into memory; upstream warns at 100k+ chunks | [established]. citeturn443946search9 |
| CPU seconds/index | **UNKNOWN** |
| CPU seconds/query | **UNKNOWN** |
| Disk reads/writes | **UNKNOWN**; GOB persists index locally |
| Index bytes | **UNKNOWN for your repos** |
| Network upload bytes | **UNKNOWN**; code chunk embedding payload + protocol overhead |
| Network query bytes | **UNKNOWN** |
| Provider tokens | measurable from provider usage / requests; formula above |
| Rename cost | re-embedding required in 1.2.0 | [established]. citeturn138612view1 |
| Cold CLI latency | **UNKNOWN** |
| Warm OS-cache latency | **UNKNOWN** |
| Watcher idle cost | irrelevant if not started |
| MCP idle cost | irrelevant if not registered |

A useful lower bound illustrates why “remote embeddings” does not mean “small local search.” OpenAI small defaults to **1536 floating-point dimensions**. If grepai stores/searches float32 vectors, vector payload alone is `1536 × 4 = 6,144 bytes/chunk`, excluding Go slice/object overhead, chunk text, metadata and index serialization. Large at 3072 dimensions is at least 12,288 bytes/chunk. OpenAI dimensions are authoritative; byte multiplication is an engineering derivation. citeturn341196search3 At 100,000 chunks that is roughly 586 MiB of raw 1536-d float32 values before overhead. This is why reducing local inference to zero does not eliminate local GOB RAM or scan cost. **Exact grepai resident-set behavior must be measured.**

That makes the simplest candidate architecture:

```text
Pinned grepai v1.2.0
+ remote embedding provider already entitled
+ local GOB
+ fixed chunking
+ hybrid OFF initially
+ one-shot `grepai index`
+ one-shot `grepai search --json --compact`
+ no watcher
+ no MCP registration
+ no Ollama/LM Studio
+ no Postgres/Qdrant
+ no shared workspace
+ sanitized immutable snapshot
```

A remote Qdrant service might reduce local vector-scan RAM/CPU, but it introduces a second network service, storage policy, credentials, remote-state lifecycle and possible new spend. That directly violates the pilot’s “simplest on-demand architecture” objective unless an already-owned Qdrant/Postgres service is separately approved.

## G. Additive isolated canary protocol

This canary should answer **two separate questions**: first, does grepai function correctly enough to be usable; second, does adding it to CodeIndexer produce incremental agent benefit. Do not promote from “installed / ACK / exit 0.”

Create immutable snapshots of representative **owned** repositories containing only extensions actually under test: Python, JS/TS and, after explicit `custom_extensions`, PowerShell. Preserve revision and repo identity in a manifest, but keep `.grepai` state outside the source-truth judgment. Suggested preflight:

```powershell
git rev-parse HEAD
git status --porcelain=v1
Get-ChildItem -Recurse -File |
  Get-FileHash -Algorithm SHA256 |
  Sort-Object Path |
  Export-Csv .\canary-manifest.csv -NoTypeInformation
```

Then run your existing approved secret scanner over the snapshot. If none exists, the pilot is blocked rather than justified by `.grepaiignore` alone; grepai itself is not documented as a secrets detector. `.env` default recognition makes this especially important. citeturn794878view4

Use the same immutable snapshot for both arms:

**Baseline arm:** CodeIndexer only.  
**Treatment arm:** identical CodeIndexer baseline plus manually callable grepai.

Do not alter CodeIndexer’s index, prompt, config, daemon state or scoring. Counterbalance order AB/BA across tasks and agents so cache/warmup/order are not confounded. Independent answerers should not see source truth or the expected file/function. The judge receives source truth after answers are frozen.

The task battery should include RU and EN versions of: known-symbol lookup; exact string/identifier; semantic intent with vocabulary mismatch; “where is this behavior implemented?”; navigation/caller/callee; negative/no-answer; deliberately ambiguous same-name; and changed-copy freshness tests. For PowerShell, distinguish “semantic find” from “trace” rather than penalizing an unsupported parser as though upstream claimed it.

For each answer serialize and count **all** context actually delivered to the model: system/tool wrappers, tool schemas, grepai output, CodeIndexer output, repeated excerpts, echoes, agent-authored summaries subsequently injected into context, and subagent payloads. Record **actual grepai process invocations**, not “agent intended to use it.” A treatment task with zero successful grepai invocations is a treatment-protocol failure and must not be scored as grepai quality.

Measure:

```text
correct / partially correct / incorrect
source-grounded file + line
false answer on no-answer task
missed true target
false-positive retrieved chunks
grepai invocation count
CodeIndexer invocation count
bytes of serialized delivered context
agent input tokens where observable
wall-clock task time
grepai cold query time
grepai repeated/warm query time
index build time
incremental edit time
peak RSS
idle RSS (= zero expected after CLI exit)
CPU time
disk index bytes
disk bytes read/written if available
network request count/bytes
provider input tokens
provider billed cost
```

For resource collection, use OS-native instrumentation rather than introducing a resident profiler into the operational configuration: PowerShell/Process counters plus ETW/Resource Monitor on Windows, `/usr/bin/time -l` and Activity Monitor/instruments sampling on macOS. Exact measurement scripts are host-policy dependent and are a canary implementation detail, not an upstream claim.

Freshness should be tested only on disposable copies in this sequence: initial index → edit existing file → one-shot re-index → create → re-index → rename/move → re-index → delete → re-index → branch-like bulk replace on a copy → re-index → deliberate interrupted/failed remote embedding where feasible without corrupting provider state. v1.2.0 specifically changed rename, deletion and failed-embedding behavior, so those are regression-sensitive paths. citeturn138612view1

A call-graph fixture should independently score every expected edge against native language-server output **and** manual source truth. Include ambiguity and dynamic-language constructs described in section D. An LSP is a comparator, not the oracle; source truth is.

Preregister thresholds **before measurements**. A defensible template, deliberately presented as a proposal rather than a factual product threshold, is:

```text
Functional gate:
- 100% clean secret-scan / exclusion preflight
- zero unapproved files observed in provider-request capture
- zero index corruption / cross-snapshot leakage
- create/edit/rename/delete truth correct after explicit `grepai index`
- no unexplained cross-project results

Benefit gate:
- no statistically obvious regression in answer correctness
- strictly fewer false answers on semantic tasks OR material context reduction
- no increase in false-answer rate on no-answer tasks
- measurable successful grepai use in treatment arm
- incremental local resource cost within preregistered host budget
- cloud cost entirely inside pre-existing allowance

Call-graph gate:
- report TP/FP/FN edges separately
- no promotion based on graph display alone
- native LSP/source truth remains authoritative for risky refactors
```

Set the numeric resource/quality limits from your CodeIndexer owner’s current baseline before opening results; inventing percentages here would reproduce the methodological error you explicitly rejected.

For **second-OS readback**, do not copy `.grepai/index.gob`. Copy or check out the exact immutable source snapshot, verify `canary-manifest.csv`, use the same pinned grepai v1.2.0 and the same remote model/dimensions, generate a fresh local index, replay a fixed query file in fixed order, and diff normalized JSON results. Repeat Windows Hub → Intel Mac Pro → Mac Studio with OS order counterbalanced across repositories. Cross-platform identical ranking should be measured rather than assumed; no upstream portability guarantee was found.

Abort immediately on any secret transmission, provider charge outside pre-existing allowance, uncontrolled config mutation, stale deleted result after explicit reindex, cross-repository leakage, unexplained model/dimension mismatch, or index corruption. Rollback is simply delete the snapshot’s `.grepai/` directory, unset the ephemeral provider key, and remove the pinned grepai binary if desired; because the proposed pilot does not register a watcher/MCP/global agent rule, there is no daemon or shared agent configuration to unwind.

## H. Decision, contradictions, unresolved gaps, and source register

**Recommendation: TRY — narrowly, as an additive on-demand semantic retriever, not as a CodeIndexer replacement and not as an authoritative call graph. [emerging]**

The consumer should be named narrowly: **Claude Code / Codex / Cursor exploratory code-discovery tasks that begin with semantic intent rather than an exact identifier, on sanitized owned-code snapshots, after CodeIndexer remains the baseline owner.** grepai is a plausible challenger because it offers a genuinely one-shot architecture, can push embedding inference off-host, supports structured compact output, and can avoid all resident ML processes. citeturn997127search1turn997127search5 Nothing in the source evidence justifies fleet-wide watcher/MCP deployment or replacing exact/source-index navigation.

The strongest argument **against** adopting it is not cloud cost—the direct OpenAI embedding price is very small at the stated token scales—but **correctness and operational maturity at the boundaries that matter**. Recent releases had to repair branch-checkout deletion, atomic-write disappearance, corrupt GOB recovery, rename-vector reuse, embedding-failure disappearance, daemon locking, Qdrant project separation and same-name trace behavior. Those fixes are positive engineering work, but their recency means your exact mutation/cross-platform workload is not yet evidenced by long-running independent measurements. citeturn566048search0turn138612view1 Call-graph semantics are particularly weak relative to an LSP: current release notes explicitly preserve same-name/scoping limitations, and official binaries are built without CGO Tree-sitter. citeturn138612view1turn503637search3

The principal contradictions found are summarized here:

| Claim/evidence | Contrary evidence | Resolution |
|---|---|---|
| Site markets “100% Local & Private” | same site supports OpenAI/other hosted embeddings | Marketing describes the **default Ollama path**, not every supported configuration. Remote embeddings transmit code/query text. [established]. citeturn997127search4turn697433view0 |
| Site section says latest v1.1.0 | GitHub Releases/current site header/pkg.go.dev say v1.2.0 | v1.2.0 wins; stale generated site copy. citeturn190542search0turn190542search1 |
| Docs expose “precise” Tree-sitter concepts | official binaries are built without CGO; “ship tree-sitter by default” PR remained open | Do not assume release binaries use precise mode. citeturn503637search3turn503637search5 |
| “Trace callers/callees” can look semantically authoritative | v1.2.0 still merges same-name callees and lacks class-method scoping | Displayed graph ≠ semantic resolution. citeturn138612view1 |
| Remote embeddings reduce local compute | GOB still scans vectors in RAM; hybrid loads text | Remote inference ≠ zero local search/storage footprint. citeturn443946search1turn443946search9 |
| Environment variables are safer than committed keys | pre-1.2.0 custom endpoint could receive fallback key | Pin ≥1.2.0; official-host env-key restriction materially matters. citeturn138612view2 |
| Rename could theoretically reuse same content vector | path is embedded into input; 1.2.0 deliberately re-embeds on move | Budget for path-only churn. citeturn138612view1turn794878view3 |
| 97% token saving headline | one maintainer-run TS repo/five tasks/local Ollama; no evidence matching your serialized-context protocol | Treat as hypothesis-generation only, never as ranking evidence. citeturn621866view0 |

The remaining high-value unknowns are **peak RSS and CPU on each of the three machines; cold/warm GOB load/search latency; exact disk amplification per chunk; network bytes; RU retrieval accuracy; negative-query abstention; PowerShell semantic usefulness after custom extension ingestion; prebuilt-binary trace edge correctness; cross-OS result stability; and current unreleased `main` SHA.** Each is **UNKNOWN**, not “probably fine.” No primary current source provided fleet-representative measurements for them.

The concrete options are:

**KEEP current CodeIndexer only. [established as the lowest-risk option]** Zero new data egress and zero grepai maintenance burden; loses the possibility that semantic retrieval reduces exploratory context/tool churn.

**TRY pinned grepai v1.2.0 + existing-entitlement remote embeddings + local GOB, one-shot only. [emerging]** This is the recommended canary. It removes local embedding-model CPU/GPU and idle daemon RAM, leaves baseline untouched, and has trivial rollback. Its costs are local vector/search RAM, remote code/query disclosure under the selected provider policy, re-embedding churn, and uncertain incremental retrieval quality.

**PARK remote grepai. [established trigger condition]** Choose this automatically if no already-funded/allowanced provider account is available, if policy cannot permit owned source to the selected provider, or if secret-exclusion preflight cannot be made fail-closed. No credit card or pay-as-you-go activation should be created merely to prove that a $0.002–$0.02 nominal index can run.

**Single recommendation: TRY [emerging]**, with one decisive assumption: **an already-existing embedding entitlement is available whose data policy is acceptable for sanitized owned code.** If that assumption is false, recommendation becomes **PARK** without further technical debate. The exact next experiment is **“CodeIndexer baseline vs CodeIndexer + pinned grepai v1.2.0/OpenAI-small-or-already-owned-Synthetic/GOB, immutable sanitized snapshot, one-shot index/search, RU+EN counterbalanced blinded task battery, three-OS fresh reindex, full serialized-context/resource/provider accounting.”** There is no basis for a fleet PASS before that result.

Source register, with publication/revision dates where available: [grepai GitHub repository](https://github.com/yoanbernabeu/grepai) (observed 2026-11-01); [v1.2.0 release](https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0) (2026-10-30); [v1.2.0 release commit](https://github.com/yoanbernabeu/grepai/commit/d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef) (2026-10-30); [v1.2.0 LICENSE](https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/LICENSE) (revision v1.2.0, 2026-10-30); [v1.2.0 go.mod](https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/go.mod) (revision v1.2.0, 2026-10-30); [project documentation home](https://yoanbernabeu.github.io/grepai/) (observed 2026-11-01); [Installation](https://yoanbernabeu.github.io/grepai/installation/) (observed 2026-11-01); [Getting Started](https://yoanbernabeu.github.io/grepai/getting-started/) (observed 2026-11-01); [Quick Start](https://yoanbernabeu.github.io/grepai/quickstart/) (observed 2026-11-01); [Semantic Search](https://yoanbernabeu.github.io/grepai/search-guide/) (observed 2026-11-01); [File Watching](https://yoanbernabeu.github.io/grepai/watch-guide/) (observed 2026-11-01); [Workspace Management](https://yoanbernabeu.github.io/grepai/workspace/) (observed 2026-11-01); [MCP command reference](https://yoanbernabeu.github.io/grepai/commands/grepai_mcp-serve/) (generated 2026-10-30); [index command reference](https://yoanbernabeu.github.io/grepai/commands/grepai_index/) (generated 2026-10-26); [watch command reference](https://yoanbernabeu.github.io/grepai/commands/grepai_watch/) (generated 2026-09-10); [Call Graph Analysis](https://yoanbernabeu.github.io/grepai/trace/) (observed 2026-11-01); [CHANGELOG](https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md) (current page observed 2026-11-01; cited entries dated 2026-09-01 and 2026-09-10); [PR #411](https://github.com/yoanbernabeu/grepai/pull/411) (merged 2026-10-30); [Issue #349](https://github.com/yoanbernabeu/grepai/issues/349) (owner response 2026-10-30); [Issue #197](https://github.com/yoanbernabeu/grepai/issues/197) (2026, user-generated [low-authority]); [author benchmark](https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code/) (2026-01-21); [independent semantic-coding benchmark](https://github.com/kostchei/semantic-coding) (observed 2026-11-01 [low-authority]); [OpenAI text-embedding-3-small](https://developers.openai.com/api/docs/models/text-embedding-3-small) (observed 2026-11-01); [OpenAI Embeddings guide](https://developers.openai.com/api/docs/guides/embeddings) (observed 2026-11-01); [OpenAI data controls](https://developers.openai.com/api/docs/guides/your-data) (observed 2026-11-01); [OpenRouter pricing/privacy](https://openrouter.ai/pricing) (observed 2026-11-01); [Synthetic models](https://dev.synthetic.new/docs/api/models) (observed 2026-11-01); [Synthetic embeddings API](https://dev.synthetic.new/docs/openai/embeddings) (observed 2026-11-01); [Synthetic privacy](https://synthetic.new/policies/privacy/2026-01-01) (2026-01-01); [Requesty privacy](https://www.requesty.ai/privacy) (last updated 2026-08-14); [pkg.go.dev grepai module](https://pkg.go.dev/github.com/yoanbernabeu/grepai) (v1.2.0 published 2026-10-30); [Chocolatey v1.2.0](https://community.chocolatey.org/packages/grepai/1.2.0) (2026-10-30, supplementary [low-authority]).