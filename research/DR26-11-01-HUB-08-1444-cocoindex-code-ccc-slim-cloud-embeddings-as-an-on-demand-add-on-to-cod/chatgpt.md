# DR26-11-01-HUB-08-1444 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Source: https://приватный чат
Native Markdown export after existing-widget recovery; Deep Research GPT6 Extra High. Captured at about 2026-11-01T19:00Z. UI shows completed8m,154searches,34sources; those are vendor UI counts, not verified coverage.

# DR26-11-01-HUB-08-1444 — CocoIndex Code slim/cloud embeddings decision

**Research date:** 2026-11-01  
**Scope:** CocoIndex Code only. The October multi-product DR18 work is treated as historical context only; the prior 40-answer v2 comparison is **invalid for decision evidence** because its serialized context exceeded the limit and it produced zero successful `mgrep` calls. It is not used below as a rating or benchmark.

**Decision:** **TRY, not KEEP, and only on the Windows Hub first.** Use the released **base `cocoindex-code==0.2.42` package with a remote embedding provider**, in an isolated environment and an isolated COPY of the corpus. Do **not** install `[full]` or `[embeddings-local]`; do not add plugin hooks, permanent MCP registration, watchers, home-directory Docker mounts, or a local embedding server. Do not extend to the Intel Mac Pro or Apple Silicon Mac Studio until the preregistered Hub trial passes and an independent review/readback agrees. This is not yet a resource-saving deployment recommendation because actual incremental RSS/CPU/disk/network/provider cost relative to the existing CodeIndexer is **UNKNOWN**, and your preregistered rules correctly make UNKNOWN insufficient for a resource-saving claim.

If there is **no currently confirmed, usable embedding-API credit balance or other already-authorized API spend**, the decision becomes **PARK**, because the constraint is “no new plan/card/payments.” An API key alone is not evidence of paid permission or available credit. OpenAI now explicitly documents that ChatGPT and API billing are separate; Plus does not receive the current linked-API-credit benefit, while some Pro and Business subscriptions can receive credits only after eligibility and activation are confirmed. https://help.openai.com/en/articles/9039756-managing-billing-for-chatgpt-and-the-api-platform and https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions citeturn770219search0turn123983view2

The strongest case **for** the trial is narrow: CocoIndex Code has an official LiteLLM-only “slim” installation that omits Sentence Transformers/Torch; its semantic index is local SQLite/sqlite-vec while embedding computation can be remote; the framework ships native wheels for the three relevant processor/OS classes; and the released CLI allows explicit, on-demand indexing and JSON search without requiring permanent MCP. The strongest case **against** KEEP is stronger than the project’s “zero local resource usage” wording suggests: cloud embeddings eliminate **local embedding inference**, not the CocoIndex process tree, Rust engine, NumPy/LiteLLM stack, LMDB state, SQLite vector database, file scanning, chunking, serialization, or vector search. Relative to an already-running CodeIndexer, CocoIndex Code is an **additional** local consumer, so only measurement can establish a net host-resource win. The project's cloud-mode phrase “zero local resource usage” is therefore marketing shorthand for the embedding backend, not a literal system-level measurement. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md citeturn620494view5

## Product identity, release pin, packaging and dependency surface

### Exact product and release

The operator's identification is correct. As of the research date, the latest released OSS CocoIndex Code is **v0.2.42**, released **2026-10-05 21:21**, Git tag `v0.2.42`, short commit `6bb17d0`; PyPI's provenance attestation binds the release artifacts to full commit **`6bb17d001649fc0948a7300c2ce847b6218d174c`**. The package is **Apache-2.0**, requires **Python >=3.11**, and PyPI classifies it as Development Status “Alpha.” Its release wheel is pure Python, `cocoindex_code-0.2.42-py3-none-any.whl`, **91.8 kB**; the sdist is **94.6 kB**. https://github.com/cocoindex-io/cocoindex-code/releases and https://pypi.org/project/cocoindex-code/0.2.42/ citeturn489529view2turn617584search1turn489529view0

That 91.8-kB figure is only the **CocoIndex Code wheel**, not the installed environment. The base package pulls CocoIndex, MCP, LiteLLM and other Python dependencies, and CocoIndex itself includes a Rust native extension. No trustworthy official measurement was found for the complete unpacked base environment or its runtime RSS on the three target hosts; those remain **UNKNOWN until the trial**. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/pyproject.toml citeturn461400view0

The official package contains two command entry points, `cocoindex-code` and `ccc`; these are not separate products. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/pyproject.toml citeturn461400view2

**“Slim” is an official installation description, not a separate PyPI package.** The project's README calls plain `cocoindex-code` the LiteLLM-only **slim** install. There is **no official `cocoindex-code-slim` package or extra** in v0.2.42. `[embeddings-local]` adds Sentence Transformers, while `[full]` is currently an umbrella alias for the same local-embedding extra and may gain more optional dependencies later. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/pyproject.toml and https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/README.md citeturn461400view2turn617584search3

CocoIndex Code must also be distinguished from the **CocoIndex framework** (`cocoindex-io/cocoindex`), which supplies the Rust/Python indexing engine, and from **CocoIndex Code Plus**, which is a distinct proprietary/self-hosted product with a query service and richer navigation functionality. Code Plus is explicitly not the subject of this decision. The public Plus package is proprietary (`LicenseRef-CocoIndex-Proprietary`), whereas CocoIndex Code OSS is Apache-2.0. https://github.com/cocoindex-io/cocoindex-code-plus-pub and https://pypi.org/project/cocoindex-code-plus/ citeturn937100search0turn937100search4

### Base versus local/full dependency graph

The **direct dependencies** of released v0.2.42 base are:

```text
cocoindex-code 0.2.42
├── mcp >=2,<3
├── cocoindex[litellm] >=1.0.17,<1.1.0
├── sqlite-vec >=0.1.0
├── pydantic >=2
├── numpy >=1.24
├── einops >=0.8.2
├── typer >=0.9
├── msgspec >=0.19
├── pathspec >=0.12.1,<1
├── pyyaml >=6
└── questionary >=2
```

`[embeddings-local]` and `[full]` both additionally request `cocoindex[sentence-transformers]>=1.0.17,<1.1.0`. That is the branch that pulls the heavyweight local ML runtime/model stack and is deliberately excluded from this proposal. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/pyproject.toml citeturn461400view0turn461400view2

The CocoIndex framework is built with **Maturin/PyO3** and contains a Rust engine. Its codebase includes Rust support for local filesystem processing, text/code operations and Tree-sitter-backed language handling. The v0.2.42 requirement is a range, not a hard framework pin. On 2026-11-01, the latest permitted framework release is **CocoIndex 1.0.25**, but a reproducible trial should capture the actual resolver output and pin the complete environment rather than assuming future installs will continue to resolve identically. https://github.com/cocoindex-io/cocoindex/blob/main/pyproject.toml and https://github.com/cocoindex-io/cocoindex/releases citeturn952204search8turn300665search3

CocoIndex 1.0.25 publishes ABI3 native wheels suitable for all three intended host classes: Windows x86-64 (**10.1 MB**), macOS x86-64 (**9.7 MB**), and macOS ARM64 (**9.7 MB**), all uploaded 2026-10-06. That establishes package availability, **not operational compatibility or performance**. https://pypi.org/project/cocoindex/1.0.25/ citeturn617584search0

`sqlite-vec` is the vector-store extension used by CocoIndex Code OSS. Postgres is **not** required for this base deployment. The indexing code declares a SQLite vector target; the project keeps incremental engine state separately and stores the searchable vector table locally. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/indexer.py and https://github.com/cocoindex-io/cocoindex/blob/main/pyproject.toml citeturn599323view3turn952204search8

### What “cloud” does and does not download

With plain `cocoindex-code` and a remote LiteLLM embedding provider, **Sentence Transformers, Torch and a Hugging Face embedding model are not intentionally part of the requested installation**. Consequently there should be no local embedding-model download/cache or GPU inference attributable to that path. However, there are still ordinary Python/package downloads at installation time, native wheels for CocoIndex/sqlite-vec, and whatever transitive HTTP/tokenizer support LiteLLM installs. The released CocoIndex Code indexer itself does **not** show a local Hugging Face tokenizer step before sending chunks; its chunk sizes are character-structured, and embedding text is handed to the configured embedder. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/indexer.py and https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md citeturn461400view4turn620494view0

Tree-sitter grammar machinery remains relevant for local chunking/structural matching, so “cloud embedding” does **not** mean “all parsing happens remotely.” The semantic payload sent to the provider consists of chunk/query text for embedding; local scanning, language detection, splitting, database writes and vector search remain local implementation work. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/indexer.py citeturn461400view4

The project's current published Docker documentation describes `cocoindex/cocoindex-code:latest` slim as approximately **450 MB** and `:full` as approximately **5 GB**. The README also says the slim Python install avoids roughly **1 GB of Torch + Transformers**. These are **publisher-provided approximate image/dependency-size claims**, not measurements of compressed image transfer, unpacked layers, virtualenv size, RSS, private bytes, CPU, GPU VRAM, or the incremental footprint against your existing CodeIndexer. They must not be converted into a resource-savings conclusion. https://pypi.org/project/cocoindex-code/ and https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/README.md citeturn617584search4turn617584search3

For this decision, native installation is preferable to Docker: Docker adds another service/container boundary and image footprint, while the project's own Docker example can bind `$HOME` into the container—a configuration explicitly outside the brief. No Docker image is necessary to obtain the “slim” dependency behavior. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/README.md citeturn300665search0

## Indexing pipeline, retrieval semantics and capability boundaries

### Scanning, ignores, files and chunking

Released v0.2.42's project settings contain a broad source-extension allowlist including `.py`, `.js`, `.jsx`, `.ts`, `.tsx`, Rust, Go, Java, C/C++, C#, Swift, Kotlin, SQL, shell, markup/configuration formats and others. **`.ps1` is not in the default include list.** Therefore PowerShell must be explicitly added to the trial's project `include_patterns` if present in the corpus. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py citeturn757074view2

The framework's `RecursiveSplitter` has syntax-aware support for major languages including Python/JavaScript/TypeScript, while many additional languages, including PowerShell, use separator-based fallback rather than Tree-sitter structural splitting. Thus a PowerShell trial can test semantic retrieval, but it should **not** be described as getting the same AST-aware chunking as Python/JS/TS. https://cocoindex.io/docs/ops/text/ citeturn119810search0

Default exclusions include hidden directories, `__pycache__`, `node_modules`, build/distribution paths, vendor patterns, and `.cocoindex_code`. `max_file_size` defaults to **no limit**; the frequently shown `500KB` is an example configuration, not the default. After changes to file include/exclude/max-size/language overrides, the project directs users to `ccc doctor` and then `ccc index` or `ccc search --refresh`. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py and https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/README.md citeturn757074view2turn952204search0

The indexer reads text and drops files that raise `UnicodeDecodeError`; empty/whitespace-only files are also skipped. This is not equivalent to a strong magic-number binary detector, so corpus coverage must be measured rather than inferred from extensions alone. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/indexer.py citeturn461400view5

Released chunking constants are **target 1000 characters, minimum 250, overlap 150**. The splitter is language-aware where supported; the resulting row stores the chunk text, language, line range and POSIX-form path. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/indexer.py citeturn461400view4turn461400view6

That is an important capability boundary: **AST-aware chunk boundaries do not imply a symbol graph, call graph, data-flow graph, reference index or caller/callee analysis.** CocoIndex Code OSS stores semantic chunks and vector embeddings. Its released semantic query embeds a query and performs nearest-neighbour distance retrieval from `code_chunks_vec`. It does not call a generation model or a reranker in this path. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/query.py citeturn757074view3turn546139view1

### Structural grep is released—but separate

`ccc grep` is **released functionality**, not a main-branch-only claim: structural grep shipped before v0.2.42, and the v0.2.42 source uses CocoIndex code-pattern matching locally. It does not require the semantic index or cloud embeddings. https://github.com/cocoindex-io/cocoindex-code/releases and https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/grep.py citeturn952204search1turn752656view7

For this canary, I would **not expose `ccc grep` as a new treatment tool**. The research question is whether additive slim/cloud semantic retrieval improves the existing CodeIndexer setup. Mixing a second new retrieval primitive into treatment would make causal attribution worse. Structural grep can be a later preregistered experiment.

### Search behavior by task class

**Known symbol / exact string.** Semantic search accepts natural-language or code-snippet queries, but it is not an exact-text index. Exact known symbols may therefore be cheaper and more deterministic through the existing CodeIndexer/source-read fallback. The CocoIndex skill itself advises conceptual queries rather than exact syntax. https://github.com/cocoindex-io/cocoindex-code/blob/main/skills/ccc/SKILL.md citeturn982134search2

**Semantic EN / RU.** This is the most plausible additive win, especially when the answerer does not know identifier names. OpenAI's `text-embedding-3-small` has documented multilingual retrieval improvements and an 8,192-token input limit, but its published generic MTEB/MIRACL figures are **not evidence of performance on this codebase, Russian code-search questions, or your exact chunking regime**. The RU and EN task strata must therefore remain empirical. https://developers.openai.com/api/docs/guides/embeddings citeturn770219search4

**Navigation.** CocoIndex Code OSS cannot be credited with a native “find callers/references/flow” capability merely because chunks respect syntax boundaries. Navigation tasks remain valid treatment tasks because additive semantic search might help an LLM find relevant files, but any answer should still be verified by source reads/CodeIndexer. The richer definitions/references interface belongs to Code Plus, not this OSS product. https://github.com/cocoindex-io/cocoindex-code-plus-pub citeturn937100search0

**No-answer.** This is a particularly important adversarial class. Released semantic search orders neighbours by L2 distance and returns the requested number of nearest chunks; the v0.2.42 query path contains **no semantic acceptance threshold**. Therefore an unrelated query can still receive “best available” chunks. The model/harness must retain the option to say “not found,” and the judge must penalize confidence induced by merely plausible neighbours. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/query.py citeturn546139view0turn546139view1turn757074view3

A small released-performance wrinkle is also relevant: path-filtered searches in v0.2.42 perform explicit full-scan distance computation rather than the normal vec0 KNN plan. A September 2026 issue proposes bounded-KNN optimization and reports synthetic latency differences, but that change is not evidence about v0.2.42 end-to-end speed and must not be treated as released. https://github.com/cocoindex-io/cocoindex-code/issues/286 citeturn982134search1

## Lifecycle, freshness, state identity and OS risks

CocoIndex Code's CLI uses a **background daemon that starts automatically on first use**. The released default daemon idle timeout is **180 minutes**, and `keep_alive_with_mcp` defaults to **true**; `0` means never exit. Clients can restart the daemon on the next request after idle exit. For this brief, those defaults are wrong: use a short timeout and `keep_alive_with_mcp: false`, and do not install an MCP server at all. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py citeturn757074view0turn546139view8

The manual CLI is compatible with the desired on-demand model. `ccc search` defaults to **no refresh**; `--refresh` explicitly runs indexing first. `--json` serializes the response for deterministic harness capture. In contrast, the MCP search tool defaults `refresh_index=true`, another reason not to add permanent MCP for this trial. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/cli.py and https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/README.md citeturn546139view3turn546139view4turn982134search0

Without hooks/watchers, freshness is **operator/harness-driven**. The appropriate lifecycle is therefore: refresh at a known boundary, search repeatedly without refreshing while the snapshot is immutable, and after each controlled mutation issue a deliberately labelled manual refresh. This is different from marketing language such as “keeps it fresh”; the project's own skill instructs `ccc index` or `ccc search --refresh` when an index may be stale. https://github.com/cocoindex-io/cocoindex-code/blob/main/skills/ccc/SKILL.md citeturn952204search3

The local project state is path-oriented rather than Git-identity-oriented. An open upstream issue explicitly says multiple worktrees/checkouts of the same repository currently pay for separate full indexes and have daemon state tied to the `.cocoindex_code` inside each checkout. There is no released canonical `(repo_id, revision)` identity layer to solve duplicate checkout or Windows/macOS root equivalence. https://github.com/cocoindex-io/cocoindex-code/issues/170 citeturn952204search19

Consequently the trial's **logical `repo_id`, revision and NFC-normalized relative path** must live in the experiment manifest, not be inferred from CocoIndex paths. `E:\foo\repo` and `/Users/x/foo/repo` are never to be declared equivalent merely from root spelling. The tool has host/DB path-mapping environment variables for container/path presentation use, but those are not a cross-OS repository identity system. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py citeturn757074view1

There is significant disconfirming lifecycle evidence. Open issue **#210** documents v0.2.37-era/current project-state failure in which deleting or corrupting `target_sqlite.db` while CocoIndex's incremental state remains can leave `ccc index` reporting files unchanged yet no usable vector table; the reported workaround is `ccc reset -f` followed by a rebuild. The issue notes that `cocoindex.db` may be a directory, not a SQLite file. This means “incremental state says unchanged” is not sufficient evidence that the target index is healthy after interrupted/damaged updates. https://github.com/cocoindex-io/cocoindex-code/issues/210 citeturn952204search4

Windows also deserves the first, isolated canary rather than presumptive fleet enablement. Open issue **#166** reports a stale daemon retaining an environment without embeddings and, separately, a GBK-console Unicode-output crash on v0.2.33. The Unicode-output problem was addressed in v0.2.40's release notes, but the issue remains useful evidence that daemon/environment lifecycle on Windows has produced real failures and must be explicitly exercised. https://github.com/cocoindex-io/cocoindex-code/issues/166 and https://github.com/cocoindex-io/cocoindex-code/releases citeturn952204search7turn982134search3

Symlink behavior also depends on the resolved CocoIndex framework version. Framework **1.0.25** includes a fix making recursive local-filesystem walks stop at symlink cycles. This is another reason to record/pin the actual framework version rather than saying “0.2.42” alone fully determines runtime behavior. https://github.com/cocoindex-io/cocoindex/releases citeturn982134search4

For create/edit/rename/delete freshness, interrupted update, locks, concurrent projects and model/dimension changes, the released documentation is not strong enough to waive an empirical test. The indexer is memoized and intended to update incrementally, but the state/target inconsistency above disproves any blanket “incremental means always self-healing” assumption. **Correctness after rename/delete and after an interrupted operation is therefore UNKNOWN until the mutation canary.** citeturn461400view4turn952204search4

Changing the embedding model or output dimension must be treated as an **index-schema change requiring a fresh isolated index/rebuild unless a release-pinned source proves safe migration**. Index and query vectors must have the same dimension for the SQLite vector table. Do not dynamically tune dimensions halfway through the preregistered experiment.

## Minimal isolated configuration and cloud economics

### Recommended canary shape

The intervention should be deliberately smaller than the project's “recommended plugin” setup:

```text
Existing CodeIndexer                         retained unchanged
        │
        ├── existing source-read fallback    retained unchanged
        │
        └── optional treatment call
             └── ccc 0.2.42 CLI
                   ├── short-lived daemon
                   ├── local LMDB/CocoIndex state
                   ├── local SQLite/sqlite-vec vectors
                   └── remote embeddings only
```

Do not install `[full]`, `[embeddings-local]`, Claude/Grok/OMP plugins, session/post-edit hooks, a permanent stdio MCP registration, filesystem watchers, or Docker. The official README itself supports “skill-only/no MCP/no auto-index” and direct CLI use, so this is not an unsupported mode. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/README.md citeturn300665search0

Use an isolated Python environment and pin the top-level release:

```bash
python -m venv <trial-venv>
# activate with the host-native mechanism
python -m pip install "cocoindex-code==0.2.42"
ccc version
python -m pip freeze > <evidence-dir>/resolved-requirements.txt
```

For stronger reproducibility, preserve the installer logs, wheel hashes and `pip freeze`; specifically record the resolved `cocoindex`, `litellm`, `sqlite-vec`, `numpy` and `mcp` versions. Do not silently upgrade during the trial. The base `cocoindex-code` wheel itself has SHA-256 `6ad19ef0065a73cd0ddb2c7d7abe270751df7c920f6584cd23c3cb5676a27590` according to PyPI provenance. https://pypi.org/project/cocoindex-code/0.2.42/ citeturn489529view0

Set **`COCOINDEX_CODE_DIR` to a trial-owned isolated settings/state location** before initialization, and operate only inside the COPY corpus. The API credential, if one is already authorized, should come from the process environment/credential mechanism; do **not** place it in YAML, logs, evidence JSON, command history or this report.

A small global configuration for the cloud path is:

```yaml
embedding:
  provider: litellm
  model: openai/text-embedding-3-small
  indexing_params: {}
  query_params: {}

daemon:
  idle_timeout_minutes: 1
  keep_alive_with_mcp: false
```

The `embedding`, optional `indexing_params`/`query_params`, and sibling `daemon` hierarchy are released v0.2.42 settings. The daemon's stock defaults are 180 minutes/true, so the explicit override is material. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py citeturn757074view0turn546139view8

CocoIndex's LiteLLM documentation examples use provider prefixes such as `voyage/...` and `openai/...`; OpenAI `text-embedding-3-small` is an explicitly recommended remote option in v0.2.42. Because LiteLLM model-string conventions can change independently, the preflight must confirm that the frozen resolved LiteLLM actually accepts `openai/text-embedding-3-small`; a successful configuration parse alone is not enough. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md citeturn620494view0turn620494view1

For a corpus containing PowerShell, add it **before preregistration is frozen**, for example:

```yaml
include_patterns:
  - "**/*.py"
  - "**/*.js"
  - "**/*.jsx"
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.ps1"
```

Do not replace the real repository's required non-code/default extensions blindly; derive the immutable trial allowlist from the snapshot manifest. The important fact is that `.ps1` is not a stock default. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py citeturn757074view2

The operational sequence is intentionally explicit:

```bash
# In COPY only
ccc init
ccc doctor
ccc index

# Frozen snapshot searches: no hidden refresh
ccc search --json --limit <N> "<query>"

# Mutation/freshness test only, explicitly counted as manual refresh
ccc search --refresh --json --limit <N> "<query>"

# End of owned trial
ccc daemon stop
```

`ccc index`, `ccc search --refresh`, plain `ccc search`, and `--json` are released CLI behavior. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/cli.py citeturn546139view4turn546139view5

### Why `text-embedding-3-small` is the least-complex provider candidate

This is a **candidate contingent on already-existing usable credits**, not a requirement to buy OpenAI API service. The model is an established remote embedding endpoint supported by CocoIndex's LiteLLM path, has a documented maximum input of **8,192 tokens**, defaults to **1,536 dimensions**, allows the `dimensions` parameter, and currently costs **$0.02 per 1 million input tokens**. CocoIndex chunks target about 1,000 characters, well below that provider input limit in ordinary cases. https://developers.openai.com/api/docs/guides/embeddings and https://developers.openai.com/api/docs/models/text-embedding-3-small citeturn770219search4turn324801view3

Do **not** reduce dimensions in the first primary comparison. A 512-dimensional vector might reduce local vector storage/search work, but it introduces a second accuracy-changing intervention. First measure the canonical 1,536-dimensional model; a reduced-dimension arm can be a separate preregistered follow-up if the main arm misses the resource gate. OpenAI documents that dimensions can be shortened, but does not establish that 512 dimensions preserves this code/RU retrieval workload. https://developers.openai.com/api/reference/resources/embeddings/methods/create citeturn770219search5

CocoIndex Code batches up to **64 chunks** per cloud embedding request and documents automatic 429 retries with exponential backoff, up to six times. These retries and all provider calls must be preserved in the experiment log rather than hidden as “internal.” https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md citeturn620494view5

There is **no generation or rerank charge intrinsic to released OSS semantic search**: indexing/query text is embedded; the local SQLite vector query returns chunks by distance. Any later Claude/Codex/Cursor reasoning over those returned snippets belongs to the client/model's context cost, not the embedding endpoint's charge. https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/query.py citeturn757074view3

### Pricing, credits and cost accounting

As of 2026-11-01, OpenAI's primary model page states **$0.02 / 1M input tokens** for `text-embedding-3-small`. https://developers.openai.com/api/docs/models/text-embedding-3-small citeturn324801view3

The correct cost expression is therefore:

```text
T_billable =
    T_initial_index
  + T_changed_chunks
  + T_queries
  + T_billable_retries
  + T_rebuilds
  + T_maintenance

Embedding_API_cost =
    T_billable / 1,000,000 × $0.02
```

`T_changed_chunks` means text actually re-embedded, not merely files scanned. `T_queries` includes every query embedding, including unsuccessful/repeated attempts. `T_rebuilds` matters because model/dimension changes, corruption recovery or resets may re-embed the corpus. Retry accounting must follow provider receipts: a failed retry should not be assumed free unless billing evidence says it was free.

An **illustrative, explicitly non-measured** example: if a corpus produced 20 million billable embedding input tokens on first build, the listed rate would imply `$0.40`; another 2 million changed/rebuild/query tokens would imply `$0.04`. Those figures say nothing about your actual corpus because neither its chunk-token count nor retry/rebuild rate has been measured.

The experiment's **24,000 UTF-8-byte client context budget is not the same thing as embedding billing**. Returned JSON/code consumed by Claude/Codex/Cursor can create separate model-context cost; wrappers, escaping, retries and errors must nevertheless be counted in your task budget exactly as preregistered.

There is no general “free monthly embeddings allowance” documented on the model price page. API promotional/grant credits are account-specific. OpenAI now documents an API-credit program for certain **Pro and Business** subscriptions, while **Plus is explicitly not eligible for that offer**; even an eligible subscription does not prove a grant exists until it is activated and visible in the linked API organization. https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions citeturn123983view2

Therefore the precondition is:

```text
existing API organization
AND explicit embedding-model access
AND confirmed positive usable credit/grant balance
AND no new payment method
AND no auto-recharge/new purchase
= eligible for canary

anything else
= UNKNOWN / PARK
```

An API key alone fails this test.

OpenAI's Usage Dashboard can report organization/project activity, while the Billing page exposes credit grants; those are distinct records and should both be captured for the trial. Dashboard timestamps are UTC. https://help.openai.com/en/articles/10478918-reviewing-api-usage-and-costs citeturn123983view3

For prepaid API billing generally, free credits are consumed before purchased credits; purchased credits expire after one year. The brief forbids creating new paid balance, so this is background policy rather than permission to purchase. https://help.openai.com/en/articles/8264644-setting-up-and-managing-prepaid-api-billing citeturn770219search6

For ordinary nonsecret source code sent to OpenAI embeddings, current API data controls say API inputs are **not used to train OpenAI models unless the organization explicitly opts in**. `/v1/embeddings` has **no application-state retention**, default abuse-monitoring retention of **30 days**, and is eligible for Zero Data Retention subject to organizational eligibility/configuration. That is materially different from “nothing is retained,” so the trial must treat code chunks as transmitted data. https://developers.openai.com/api/docs/guides/your-data citeturn324801view0turn324801view2

**When is CodeIndexer alone cheaper?** Always when its existing marginal monetary/API cost is zero or lower and CocoIndex contributes no compensating reduction elsewhere. More operationally, CodeIndexer alone wins if CCC is rarely invoked, duplicates retrieval already available, causes additional client context without accuracy gains, requires frequent rebuilds, or its extra daemon/DB CPU-RSS-disk burden outweighs any source-read/LLM-call savings. Because none of those quantities has been measured on the target Hub, a monetary or host-resource “saving” is currently **UNKNOWN**.

## Preregistered trial: critique and complete execution specification

The proposed protocol is fundamentally sound because it evaluates the **additive system actually being considered**, not CocoIndex Code in isolation. The main modifications below close capability and accounting loopholes without changing thresholds after results.

### Freeze before the first experimental answer

Create one immutable allowlisted **COPY**, never the production checkout. Record a manifest containing: logical `repo_id`; exact revision/commit or snapshot identifier; NFC-normalized relative path for every included file; SHA-256 of raw file bytes; byte length; detected test language; and an explicit per-host root map. If real Python, JavaScript, TypeScript or PowerShell exists, include it according to the frozen allowlist; do not synthesize a language merely to satisfy balance. If PowerShell exists, `.ps1` must be explicitly configured before freeze because v0.2.42 excludes it by default. citeturn757074view2

Also freeze: `cocoindex-code==0.2.42`; full resolved Python dependency set; provider/model and default 1,536 dimensions; project/global YAML hashes; CodeIndexer version/config; model/harness version; prompt template; randomization seed; task set; scoring rubric; judge model/config; byte/call limits; mutation files; and all KEEP/hard-gate thresholds.

The trial has exactly **24 tasks: four each** of known-symbol, exact, semantic-RU, semantic-EN, navigation, and no-answer. The oracle is hidden from answerers. Independent clean-context answerers receive only their condition's serialized evidence. A separate blinded judge scores correctness/confidence against the oracle.

The two arms are:

```text
A: existing CodeIndexer + existing identical source-read fallback
B: existing CodeIndexer + identical source-read fallback + ccc semantic search
```

Do not remove CodeIndexer in treatment. Do not give the CCC arm extra source-read privileges. Do not expose `ccc grep`, MCP, hooks or watchers.

Use seeded **AB/BA order** and **three paired repetitions**. The same model/harness and budgets apply to both arms. A CCC treatment in which the answerer never invokes CCC is still a treatment observation; record invocation separately. This prevents conflating “tool installed” with “tool caused benefit.”

### Context and call accounting

The cap is **10 retrieval/read calls and 24,000 UTF-8 bytes per task**, calculated **after final transport serialization**, not from pre-serialization content. Count JSON keys, quotes, escape expansion, wrappers, filenames, line metadata, scores, tool errors, retries, hook echoes and other delivered tool material. An over-budget tool message is **withheld whole**, not truncated into a different evidence object.

For every delivered retrieval/read result preserve:

```text
task_id
condition
repetition
call_ordinal
tool
argv_or_structured_invocation_without_secrets
started_at / ended_at
success / failure class
raw_serialized_bytes
sha256(raw_serialized_bytes)
byte_count
delivery_order
whether_answerer_actually_consumed_it
```

This specifically prevents a recurrence of the invalid historical comparison: **zero successful CCC retrievals cannot support a benefit claim**, and failed/oversize calls remain part of the accounting.

### Correctness-specific adversarial controls

For **known-symbol/exact** tasks, do not force semantic search; an additive system is allowed to decide that exact CodeIndexer/source reads are superior.

For **semantic RU/EN**, preregister equivalent intent difficulty rather than translating after observing results. RU and EN results are separate strata; do not average away a major language failure.

For **navigation**, judge the final answer, but additionally label whether CCC actually supplied evidence sufficient for the relationship. A semantically adjacent chunk is not proof of a caller/reference edge.

For **no-answer**, the judge must flag a newly confident false answer as a hard failure candidate. This is especially important because CCC nearest-neighbour search has no released relevance cutoff. citeturn546139view0turn757074view3

### Resource protocol

Run **3 cold and 3 warm observations per arm** with at least **60 seconds idle before and after**. A “cold” run is only naturally cold according to the frozen protocol; never restart production software or kill shared processes to manufacture cold state.

Serialize resource trials using a **physical-host lease** so another trial cannot overlap. Measure the complete owned process tree plus total-host deltas:

```text
peak RSS
idle RSS
private bytes / private working set
CPU seconds
host CPU/load
GPU VRAM / Metal allocation where measurable
disk bytes and final disk footprint
network bytes
wall time
provider request/token receipts
provider billed/credited amount
```

If the OS cannot expose a metric reliably, record **UNKNOWN**, not zero. On the cloud/slim path, expected CCC GPU VRAM is plausibly zero because no local embedding model is requested, but that remains a measurement field rather than a fact to fill with `0`.

The project's ~450-MB Docker, ~1-GB avoided dependencies and “zero local resource usage” claims do not substitute for these observations. citeturn617584search4turn620494view5

### Freshness mutation protocol

Use four COPY-only mutations: **create, edit, rename, delete**. For each, record `t_mutation`, then run the frozen refresh strategy and poll at **5-second intervals to 120 seconds**, recording both “index observed change” and “correct retrieval observed.”

A manual `ccc index` or `ccc search --refresh` is labelled **manual refresh** and its CPU/network/API/wall cost is counted; it cannot be described as automatic freshness. A no-op refresh is still an operation and must be counted. Never restart a production daemon to make freshness pass. Released CLI semantics make this distinction explicit because ordinary `ccc search` has refresh off. citeturn546139view3turn546139view4

For delete, the hard requirement is **no stale deleted-content hit at 120 seconds**. Preserve the actual returned JSON bytes to distinguish “deleted path absent” from “answerer happened not to use a stale result.”

Also inject one **trial-owned interrupted-update failure** if it can be done without touching production. Issue #210 makes this a worthwhile disconfirming test: a vector target/state mismatch must fail visibly or recover safely; silent “unchanged” with unusable target state is unacceptable. Do not repair the failure mid-test with `ccc reset` unless the protocol explicitly enters rollback/recovery. https://github.com/cocoindex-io/cocoindex-code/issues/210 citeturn952204search4

### Independent client requirement and hard gates

At least one **different LLM client** from the primary harness must actually consume a real CCC result. “CLI installed,” “MCP handshake succeeded,” “rc=0,” or “snippet appeared on stdout” establishes invocation only, **not benefit**.

The hard gates remain:

- no secrets leave the approved boundary;
- identical frozen corpus across arms;
- complete post-serialization context accounting;
- no wrong-root/cross-checkout contamination;
- at least **90% intended corpus coverage**;
- a genuinely independent client consumes a result;
- no stale delete hit at 120 seconds.

Failure of any hard gate means **not KEEP** regardless of aggregate score.

The KEEP rule also remains exactly preregistered: **no new confidently false answer; correctness decline ≤1/24; and either median wall time or total CPU improves ≥20%, or correctness improves ≥3/24 while median wall time rises ≤20%. Added idle RSS must be ≤256 MiB/host and peak ≤1 GiB unless an amendment was written before observing outcomes.** These are operational acceptance thresholds, not universal statistical significance claims. With 24 tasks and three repeats, report paired distributions and uncertainty rather than claiming general population efficacy.

Most importantly, **resource or cost UNKNOWN blocks the resource-saving claim**. Do not retroactively lower thresholds, exclude bad repetitions, change task classes, reduce dimensions, add structural grep, or alter refresh behavior after seeing outcomes.

## Evidence matrix, adversarial verdict and rollback

| Question | Release-pinned / primary evidence | What it supports | Disconfirming evidence / missing evidence | Decision implication |
|---|---|---|---|---|
| Is v0.2.42 the exact released product? | GitHub release + PyPI provenance: https://github.com/cocoindex-io/cocoindex-code/releases ; https://pypi.org/project/cocoindex-code/0.2.42/ citeturn489529view2turn489529view0 | Yes: Oct 5, Apache-2.0, Python >=3.11, attested commit | Alpha classification | Pin 0.2.42; no `latest` |
| Is “slim” real? | v0.2.42 package/README: https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/pyproject.toml citeturn461400view2 | Plain base package is officially called slim; LiteLLM-only | No package named `cocoindex-code-slim` | Install base only |
| Does slim eliminate local embedding inference? | https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/EMBEDDINGS.md citeturn620494view0 | Yes, with a genuine remote provider | Local scanning/chunking/DB/vector search remain | Possible resource win, not proven |
| Are target platform wheels available? | https://pypi.org/project/cocoindex/1.0.25/ citeturn617584search0 | Windows x86-64, Intel Mac, ARM Mac wheels exist | Wheel availability ≠ Windows lifecycle reliability | Hub preflight required |
| Is PowerShell first-class by default? | https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/settings.py citeturn757074view2 | No `.ps1` default | Framework separator fallback can handle added PowerShell, but not same AST mode | Explicitly add/test or coverage gate fails |
| Does AST chunking provide refs/callers? | Index schema/query source: https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/indexer.py citeturn461400view4 | No; it stores chunks + vectors | Plus is the product advertising richer defs/refs | Navigation must rely on combined evidence |
| Is structural grep released? | https://github.com/cocoindex-io/cocoindex-code/releases citeturn952204search1 | Yes | It is a different local retrieval intervention | Exclude from first canary |
| Does search reject irrelevant results? | https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/query.py citeturn546139view0turn757074view3 | No released relevance threshold found; nearest neighbours returned | No-answer false-confidence risk | Strong adversarial gate |
| Is freshness automatic without hooks/watchers? | CLI source + skill docs: https://github.com/cocoindex-io/cocoindex-code/blob/v0.2.42/src/cocoindex_code/cli.py citeturn546139view4 | Plain CLI search does not refresh by default | Marketing “fresh” depends on refresh/hook usage | Count manual refresh explicitly |
| Is incremental state robust to target damage? | https://github.com/cocoindex-io/cocoindex-code/issues/210 citeturn952204search4 | No; documented mismatch can require reset/rebuild | Open issue | Interrupted-update canary required |
| Is checkout identity Git-aware? | https://github.com/cocoindex-io/cocoindex-code/issues/170 citeturn952204search19 | No; worktrees indexed separately | Cross-OS logical identity absent | External manifest/root map required |
| Is Windows operationally proven by packaging? | Windows issue: https://github.com/cocoindex-io/cocoindex-code/issues/166 citeturn952204search7 | No | Stale-daemon/env and historical console failure | Hub first; no fleet PASS from install |
| Is provider cost low? | https://developers.openai.com/api/docs/models/text-embedding-3-small citeturn324801view3 | $0.02/M input tokens | Corpus/rebuild/query tokens unknown | Cheap-looking, but cost claim still UNKNOWN |
| Does a ChatGPT subscription guarantee API credit? | https://help.openai.com/en/articles/9039756-managing-billing-for-chatgpt-and-the-api-platform ; https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions citeturn770219search0turn123983view2 | No; billing separate, Plus excluded from current benefit | Some Pro/Business accounts may have claimed grants | Confirm balance; key alone insufficient |
| What happens to code sent to OpenAI embeddings? | https://developers.openai.com/api/docs/guides/your-data citeturn324801view0turn324801view2 | Not trained by default; 30-day abuse logs; no application state; ZDR eligible | Organization-specific controls/eligibility may differ | Ordinary nonsecret code only |

### Strongest FOR

The strongest affirmative hypothesis is **not** “CocoIndex is lighter than CodeIndexer.” It is: *an on-demand remote-semantic index may let Claude/Codex/Cursor locate conceptual code with fewer expensive source reads or less total host CPU than the current CodeIndexer-only workflow, while adding little idle burden because the daemon can exit quickly.* The base package really does avoid the local Sentence-Transformers/Torch path, semantic search is released and simple, and the remote provider's token price is small enough that an existing credit grant could make the monetary canary negligible. citeturn461400view2turn757074view0turn324801view3

### Strongest AGAINST

The strongest counter-hypothesis is that the treatment simply adds **another index, another daemon, another local database, another scan/chunk pipeline, another network dependency and extra context**, while the existing CodeIndexer already covers known-symbol/exact/navigation tasks adequately. In that case cloud embeddings save only the local ML inference that you were not running in the first place, so total host resource consumption rises. No official 450-MB/1-GB/“zero local resource” statement answers that comparison. Open issues on damaged target state, worktree duplication and Windows daemon lifecycle increase operational uncertainty. citeturn617584search4turn952204search4turn952204search19turn952204search7

### Disconfirming tests that matter most

The fastest ways to falsify the optimistic hypothesis are: a no-answer task where CCC creates a new confident hallucination; PowerShell coverage below the 90% gate; a rename/delete mutation leaving stale retrieval at 120 seconds; Hub idle RSS above 256 MiB; a natural workload showing total CPU or wall time not improving by the preregistered threshold; semantic RU failures; an interrupted index leaving state/target disagreement; or provider/account evidence showing no authorized existing credit. Any one of the hard-gate failures prevents KEEP.

The fastest ways to falsify the pessimistic hypothesis are three paired runs showing CCC is actually invoked on semantic tasks, materially raises correct answers without new false confidence, lowers total source-read/context work or CPU/wall time by the preregistered threshold, and stays below the added RSS limits with verified provider receipts.

### Minimal canary and cost ceiling

The only justified canary is **one Windows Hub, one immutable COPY, one isolated venv, one isolated `COCOINDEX_CODE_DIR`, base v0.2.42, remote embeddings, no MCP/hooks/watchers, existing CodeIndexer retained**.

The monetary ceiling is **not a newly invented dollar amount**. It is:

```text
min(
    confirmed currently usable existing embedding-API credit,
    preregistered trial-specific allowance explicitly approved from that existing credit
)
```

If the first term is UNKNOWN or zero, **do not send the first billable embedding request**. There is no authorization here to add a card, buy prepaid balance, activate a new paid plan, or enable auto-recharge.

### KEEP / TRY / PARK

**KEEP — not supported today.** There are no measured Hub correctness/resource/cost results satisfying the preregistered gates.

**TRY — supported, conditionally.** Proceed only if the existing API-credit/permission check is affirmative, the exact frozen install resolves correctly on the Hub, `.ps1` coverage is deliberately configured if needed, and the trial remains COPY-only and isolated.

**PARK — mandatory** if credits/permission are unknown or absent, if the Hub preflight cannot establish the correct project root, if corpus coverage is <90%, if the provider requires a new payment mechanism, or if resource/cost instrumentation cannot distinguish UNKNOWN from zero.

Even a clean installation, `ccc version`, successful initialization, ACK, rc=0, or one good snippet is **not fleet PASS**. A KEEP outcome requires the entire preregistered paired trial plus independent-client consumption and freshness/resource gates.

### Rollback

Rollback is intentionally narrow: stop **only the trial-owned CocoIndex Code daemon** with `ccc daemon stop`; remove/disconnect any temporary trial client invocation if one was created; preserve the isolated environment, COPY, YAML, logs, hashes, serialized retrieval bytes, provider receipts and databases as evidence; and leave the production CodeIndexer, production repository, user/global agent configuration and all other processes untouched. Do **not** introduce a new framework, guard service or watcher merely to make rollback work.

The Intel Mac Pro and Apple Silicon Mac Studio remain **out of scope for activation until a measured Hub benefit exists**. If the Hub passes, the next action is not automatic rollout but a release/readback review confirming that the exact dependency lock and protocol are transferable; only then should each other OS receive its own resource/freshness canary. The native-wheel evidence says installation is feasible there, not that the Hub result generalizes. https://pypi.org/project/cocoindex/1.0.25/ citeturn617584search0