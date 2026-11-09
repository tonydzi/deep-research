# DR26-11-01-HUB-05-1439 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

CopyPublishgrepai with Remote Embeddings: Operational Decision for a CodeIndexer-First Fleet (DR26-11-01-HUB-05-1439)

Recommendation: TRY only as a narrow, budget-gated canary. Run grepai v1.2.0 as an on-demand CLI with no watcher and no MCP, using text-embedding-3-small remote embeddings. Index one sealed Python/JS/TS snapshot on the Windows Hub for Claude Code. If no existing prepaid API credit or existing Synthetic subscription is evidenced, the decision becomes PARK automatically, because no zero-new-spend route exists [emerging]. The product is real, MIT-licensed and actively fixed. go But “cloud” in grepai means only third-party embedding APIs: there is no vendor-hosted grepai service. github Search and the call graph still run locally against a GOB file that is fully loaded on every query. github Precise (tree-sitter) mode is not in any released binary, and PowerShell is neither chunked by default nor traced. github + 2

TL;DR


Identity and cloud scope. The intended product is grepai by Yoan Bernabeu (https://github.com/yoanbernabeu/grepai, MIT). Its current stable release is v1.2.0, dated 2026-10-30; the release PR #416 was merged as commit d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef. github No product called “GroupEye Cloud” was found. grepai has no official hosted indexing/search service: “cloud” means sending chunks and queries to OpenAI, OpenRouter, Synthetic or Requesty embedding APIs, while vectors, symbols and search stay local unless you point it at your own Qdrant or Postgres [established]. github
Local cost of “remote” embeddings. Remote embeddings remove the Ollama/LM Studio CPU/GPU load but not the local RAM and disk. The author measured ~0.5 s and ~1.2 GB peak RAM per search on a 54,610-chunk, 494 MB GOB index, because every search and every MCP call loads and scans the whole file. For small owned repos (≈1.7k chunks), cost is trivial: github $0.02 per 1M embedded tokens on text-embedding-3-small, per OpenAI’s own model page. Even so, it is metered spend that chat subscriptions do not cover [emerging].
Decision. TRY = one preregistered canary (Claude Code on the Windows Hub, CLI-only, grepai index one-shot, Python/JS/TS snapshot, Mac Studio readback), and only after explicit authorization of a capped API balance or proof of existing credit. Otherwise PARK. Do not count the author’s 97% token claim as evidence: it is a maintainer-run, single-project benchmark of fresh input tokens only, and cost fell just 27.5% [emerging]. githubgithub


Key Findings

#FindingConfidence1grepai = github.com/yoanbernabeu/grepai, MIT, v1.2.0 (2026-10-30) is the latest stable; go 1.x is in a “stabilization phase” accepting only fixes, stability, security and new providers github[established]2No official hosted grepai service exists; Qdrant Cloud/Postgres are customer-operated storage options githubgithub[established]3Code chunks (with a File: <path> header) and every search query are sent to the embedding provider; github symbols/call graph never touch the provider githubgithub[established]4Precise/tree-sitter mode “was never wired into indexing”; release binaries are built without CGO/tree-sitter; go request to ship it closed “not planned” on 2026-10-30 githubgithub[established]5PowerShell (.ps1/.psm1/.psd1) is not in SupportedExtensions and not in trace languages; .env is indexed by default github + 2[emerging] (single primary listing via pkg.go.dev + issue #367)6Every GOB search loads and scans the full index; ~1.2 GB peak RAM at ~55k chunks (author measurement) githubgithub[emerging] [single-source]7One-shot grepai index exists since v0.39.0 (2026-10-25); no watcher required github[established]8Renames re-embed since v1.2.0; unchanged chunks of edited files reuse vectors since v1.0.0 githubgithub[established]9ChatGPT Plus does not include API usage/credits; no evidence any chat subscription includes embedding credits openai[established] for ChatGPT; [no source found] for Cursor10Synthetic’s docs say embeddings carry “no additional charge” inside a Synthetic subscription, the only potential zero-marginal-spend route, valid only if one is already owned synthetic[emerging] [single-source]11The 97% claim is maintainer-run, single project, and measures fresh input_tokens only githubgithub[established]

Details

A. Identity and architecture

Name discrepancy. A web search for “GroupEye Cloud” returned only unrelated results (galaxy clusters, Microsoft “Cloud Group Analyzer”, camera apps); no software product of that name was found [no source found]. Wikipedia + 2 The brief’s URLs identify grepai, and this report evaluates only grepai, with no substitution. “GroupEye Cloud” is best explained as a dictation artifact for “grepai” plus “cloud (remote) embeddings” [speculative, inference].

Stable version, date, revision.


The CHANGELOG on main (fetched 2026-11-01) lists [1.2.0] - 2026-10-30 with an empty [Unreleased] section. It describes 1.2.0 as fixes only, with two behavior changes github (https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md, fetched 2026-11-01). github
Release PR #416, “chore(release): bump version to 1.2.0”, was merged on Oct 30, 2026 as commit d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef. The PR states “go.mod and go.sum did not change since v1.1.0” github (https://github.com/yoanbernabeu/grepai/pull/416, 2026-10-30). github
Caveat: the tag object’s SHA for v1.2.0 could not be read directly (release/tag pages were not fetchable). d86397d is the merge commit of the release PR and the likely tag target [emerging, inference]. Verify with git ls-remote https://github.com/yoanbernabeu/grepai refs/tags/v1.2.0 before pinning.
pkg.go.dev shows the indexer package at “Version: v1.2.0, Published: Oct 30, 2026, License: MIT” (https://pkg.go.dev/github.com/yoanbernabeu/grepai/indexer, via subagent, accessed 2026-11-01). go


Stable/prerelease divergence. No prerelease tags were found. Public sources nonetheless disagree about what is “latest” (see Contradictions):


The GitHub releases list we retrieved still flagged v0.35.0 (16 Mar) as “Latest” (https://github.com/yoanbernabeu/grepai/releases, fetched 2026-11-01). github This is likely a stale or cached render.
The docs homepage says both “Latest release v1.2.0” and “Latest: v1.1.0 (Oct 26, 2026)” (https://yoanbernabeu.github.io/grepai/, fetched 2026-11-01). github
An aggregator dated mid-October reported v0.37.0 (2026-09-10) as latest (https://hysenlabs.com/projects/yoanbernabeu-grepai, ~2026-10) [low-authority]. hysenlabs
The cadence was very fast: v0.38.0 on 2026-10-24, v0.39.0 and v1.0.0 on 2026-10-25, v1.1.0 on 2026-10-26, v1.2.0 on 2026-10-30. github Four releases in six days before our research date is itself a stability signal.


Go/runtime/dependencies/licenses.


Pure Go CLI. “Building from source now requires Go 1.25.5 or later” (v0.36.1, 2026-09-01). hysenlabs Released binaries need no toolchain (CHANGELOG). github
Direct dependencies named in recent CHANGELOG bumps: github.com/mark3labs/mcp-go 1.3.0, github.com/qdrant/go-client 1.20.0 (both v1.1.0), github.com/jackc/pgx/v5 5.11.0 and google.golang.org/grpc 1.84.0 (v0.38.0), github.com/pgvector/pgvector-go 0.4.1 and github.com/fsnotify/fsnotify 1.10.1 (v0.36.0), and golang.org/x/sync (CHANGELOG). githubgithub
go.mod itself and third-party dependency licenses were not inspected [no source found]. The project license is MIT (https://pkg.go.dev/github.com/yoanbernabeu/grepai/indexer; docs footer “© 2026 grepai. MIT license.”). github + 2
pkg.go.dev shows the v1.2.0 search package (published Oct 30, 2026) as “Affected by GO-2026-6125 and 1 other vulnerabilities”, the other being GO-2026-6128. Per OSV (https://osv.dev/vulnerability/GO-2026-6125), GO-2026-6125 (aliases CVE-2026-11479, GHSA-6h35-9p2w-3j3r) was published 2026-08-18 with review status “UNREVIEWED”. It marks “All previous versions are affected”, with no known fixed version, and references grepai PR #248 and issue #247, the shared-Qdrant-collection overwrite bug that the v1.2.0 CHANGELOG says it fixed. Because the advisory is unreviewed and lists no fix, check whether it still applies to v1.2.0 before any install, and read GO-2026-6128, whose details remain unread [emerging].


Where data lives (source/doc-verified).

ArtifactLocation (default GOB backend)Leaves machine?Original source filesYour working treeNo (only chunk text is sent)Chunk text.grepai/index.gob (stored with vectors)Yes: sent to embedder; embedded text begins with File: <path> (CHANGELOG 1.2.0, #366) githubgithubVectors + metadata (file hash, chunk IDs).grepai/index.gob; .grepai/index-meta.json records provider/model/dimensions githubgithubNo (unless Postgres/Qdrant remote)Symbols / call graph.grepai/symbols.gob (always per-project, “whatever vector store the workspace uses”) githubgithubNo; “queries never call your embedding provider” (https://yoanbernabeu.github.io/grepai/trace/) githubRPG graph.grepai/rpg.gob (config constants, https://pkg.go.dev/github.com/yoanbernabeu/grepai/config) goNoSearch queriesEmbedded by the provider on every search githubYesRerankNone. Only path-based boost, optional RRF hybrid, optional file dedup githubn/a

Backends and whether remote storage implies remote search.


Backends are GOB (default, local file), PostgreSQL+pgvector, and Qdrant (local Docker or Qdrant Cloud) (CHANGELOG 0.2.0, 0.18.0). githubgithub
With Qdrant, vector search runs server-side: “Qdrant keeps an HNSW index server-side”. Postgres creates “no vector index there, so it also scans every row” github (https://github.com/yoanbernabeu/grepai/pull/402, 2026-10-26).
Remote storage does not make all search remote. Hybrid search “loads all chunks into memory for text matching” github (https://yoanbernabeu.github.io/grepai/hybrid-search/), and the trace and symbol indexes stay local GOB.
Workspaces require Postgres or Qdrant, not GOB (CHANGELOG 0.22.0). github
There is no official hosted grepai service. The homepage describes local operation, with “OpenAI and other providers … one line of config away” (https://yoanbernabeu.github.io/grepai/) [established]. github


B. Cloud path

Providers as implemented in v1.2.0 (https://yoanbernabeu.github.io/grepai/backends/embedders/, fetched 2026-11-01):

ProviderDefault endpointDefault modelDimsEnv keyBatching/retryopenaihttps://api.openai.com/v1text-embedding-3-small1536<key-var>Parallel batches (≤2000 inputs, ≤280,000 est. tokens), parallelism 4, max_retries 5 on 429/5xx, github backoff 1s→32s + jitter, honors Retry-After ≤60s, halves workers after 3×429, splits oversized batches githubopenrouterhttps://openrouter.ai/api/v1openai/text-embedding-3-small1536 (3072 large; 4096 qwen3-8b)OPENROUTER_API_KEY (fallback <key-var>, official host only) githubgithubDocs: only openai “sends batches in parallel and retries on rate limits” githubsynthetichttps://api.synthetic.new/openai/v1hf:nomic-ai/nomic-embed-text-v1.5768 (always sent)SYNTHETIC_API_KEY90s timeout githubrequestyhttps://router.requesty.ai/v1openai/text-embedding-3-small1536REQUESTY_API_KEYFixed 60s timeout; request_timeout_seconds ignored githubgithubollama/lmstudiolocalhostnomic768noneExcluded by brief


An Opper provider PR exists (https://github.com/yoanbernabeu/grepai/pull/330), but Opper is absent from the v1.2.0 embedder docs. githubgithub Treat it as not shipped [emerging].
No embedding cache by query. Content-addressed dedup reuses vectors for unchanged chunks (v0.28.0). Since v1.2.0, renamed or moved files are re-embedded (CHANGELOG #366). github + 2


Secret handling.


grepai “does not expand environment variables in config.yaml” (${<key-var>} would be sent literally). Leave api_key unset and export the variable (embedders doc). githubgithub
v1.2.0 security fix: before v1.2.0, a committed third-party .grepai/config.yaml could set embedder.endpoint and receive your environment key. Now “grepai uses an environment key only when the endpoint is the provider’s official host over HTTPS” githubgithub (CHANGELOG; https://github.com/yoanbernabeu/grepai/pull/411). github Never run pre-1.2.0 binaries on repositories you did not author.
Also fixed in v1.2.0: framework_processing.node_path could execute a repo file during indexing of .vue. githubgithub Disable the Vue processor on untrusted repos (CHANGELOG 1.2.0). github


Data transmitted. Indexing sends chunk text, which includes the project-relative path header. Search sends the raw query string. Trace and refs send nothing [established, from CHANGELOG/trace docs].

Training, retention, deletion by provider.


OpenAI API: “data sent to the OpenAI API is not used to train or improve OpenAI models (unless you explicitly opt in)”. Abuse monitoring logs are “retained for up to 30 days, unless … legally required to retain the logs for longer”. Zero Data Retention and Modified Abuse Monitoring require approval openai (https://developers.openai.com/docs/guides/your-data, accessed 2026-11-01) [established].
OpenRouter: “OpenRouter itself has a ZDR policy; your prompts are not retained unless you specifically opt in to prompt logging”. Upstream provider policies vary, and zdr: true restricts routing openrouter (https://openrouter.ai/docs/guides/privacy/zdr, accessed 2026-11-01). grepai’s openrouter embedder does not document sending a zdr parameter [no source found], so set ZDR account-wide in OpenRouter privacy settings.
Synthetic and Requesty: training/retention policies [no source found]. Treat them as unapproved for owned code until read.


Chat subscriptions vs API entitlement.


“The ChatGPT Plus subscription does not include access to the ChatGPT API and are billed separately… covers usage on chatgpt.com only” openai (OpenAI community, quoting OpenAI support, https://community.openai.com/t/paid-bill-but-api-keys-not-working/896367, 2024) [established; older source, consistent with 2026 secondary reports].
Anthropic’s Help Center (support.claude.com, article 9876003) states: “A paid Claude subscription enhances your chat experience, and the Claude API is billed separately. Claude Max and Team plans include monthly credits for the Claude API.” Claude Pro includes no API credit. Max/Team credits apply only to the Claude API, and Anthropic offers no embedding model, so no Claude plan can fund grepai embeddings [established].
Cursor: [no source found] for any embedding-API entitlement. Assume none.


Dated pricing (USD per 1M input tokens).


OpenAI’s own model page (https://developers.openai.com/api/docs/models/text-embedding-3-small, accessed 2026-11-01) lists “Price $0.02” per 1M tokens for text-embedding-3-small and “$0.13” for text-embedding-3-large. Secondary trackers agree and give the Batch API at half price (https://costgoat.com/pricing/openai-embeddings, Oct 2026; https://www.silicondata.com/use-cases/openai-api-pricing-per-1m-tokens, Mar 2026) [established]. grepai does not use the Batch API.
OpenRouter lists openai/text-embedding-3-small at “$0.02/M tokens” openrouter (https://openrouter.ai/openai/text-embedding-3-small, accessed 2026-11-01).
Synthetic: “There’s no additional charge for using embeddings, and embeddings requests don’t count against your subscription rate limit” synthetic (https://dev.synthetic.new/docs/api/models, accessed 2026-11-01) [single-source]. The subscription is cited at $30/month ghost (https://aimarketing.ghost.io/synthetic-new-review-claude-code-drop-in/, May 2026) [low-authority].
Requesty pricing: [no source found].


Scenario formulas (ESTIMATES; not our usage).


Variables: T = code tokens indexed; chunk size S = 512, overlap O = 50 (defaults, https://yoanbernabeu.github.io/grepai/configuration/); github chunks N ≈ T/(S−O); header h ≈ path tokens + ~3; P = $/1M tokens. github
Initial index: E ≈ T·(S/(S−O)) + N·h ≈ 1.11·T + N·h; cost = E·P/10⁶.

100k tokens: N≈217, h≈15 → E≈114k → $0.0023 (3-small) / $0.015 (3-large).
1M tokens: E≈1.14M → $0.023 / $0.15.



1k queries at q tokens each: 1k·q·P/10⁶. At q=30, that is $0.0006 (3-small).
Churn: only changed chunks re-embed (“a one-line edit to a 27-chunk file embeds 1 chunk instead of 27”, v1.0.0). Cost ≈ C_changed·~560 tokens·P. Renames and moves re-embed the whole file (v1.2.0). github A provider/model/dimension change forces a full re-embed (v1.0.0). githubgithub
Retries: upper bound multiplier (1+max_retries) = ×6 only if failed requests are billed. Whether 429/5xx attempts are billed is UNKNOWN. A grepai crash before v1.2.0 could also cause a re-embed of changed files (fixed by #353). githubgithub
Interpretation: for owned repos the dollar amounts are negligible. The binding constraint is the authorization gate: any OpenAI, OpenRouter or Requesty call requires a funded API balance that chat plans do not provide.


Zero-new-spend route. Only two routes exist. (a) An already-paid Synthetic subscription, if one exists in the org (embeddings included per Synthetic docs). synthetic (b) An already-funded OpenAI/OpenRouter API balance. If neither is evidenced, request explicit authorization for a hard-capped prepaid balance with auto-recharge disabled. Until then: PARK.

C. Search and languages


Modes.

Default is pure vector similarity.
Optional hybrid search fuses vector results with “simple keyword matching” (query tokenized, lowercase, min 2 chars, score = matches/total_words) via RRF, k=60, disabled by default github (https://yoanbernabeu.github.io/grepai/hybrid-search/). github
There is no exact/regex mode. The maintainers replaced “never use grep” agent instructions because “semantic search returns a ranking rather than an exhaustive result set” (CHANGELOG 0.36.0, #272). github
Consequence: keep CodeIndexer/ripgrep for exact and known-symbol queries.



Thresholds and no-answer. --limit (default 10) and scores are shown. No documented minimum-score threshold or “no answer” response was found [no source found]. Expect top-k results for nonsense queries; this is a false-answer risk to measure.
Filters and duplicates.

--path prefix filter with backend pushdown (v0.33.0). github The Postgres wildcard bug was fixed in v1.1.0. github
search.dedup collapses chunks per file (v0.36.0). github + 2
Ties break deterministically on chunk ID (v0.37.0). github
Before v1.2.0, Qdrant hybrid search returned a single merged result (#414). github



Languages, as two separate questions:

Chunk ingestion (SupportedExtensions, v1.2.0) includes .py .js .ts .jsx .tsx and .env. github It excludes .ps1/.psm1/.psd1/.mjs/.cjs (https://pkg.go.dev/github.com/yoanbernabeu/grepai/indexer, via subagent) [emerging]. go PowerShell or .mjs need chunking.custom_extensions (v0.36.0), and the watcher honors them only since v1.1.0. github + 2
Symbol/call parsing (regex) covers Go, JS, TS, Vue, Python, PHP, Lua, Java, C/C++, Rust, Zig, C#, F#, Pascal, Ruby and GDScript. No PowerShell github (https://yoanbernabeu.github.io/grepai/trace/). github
AST chunking (chunking.strategy: ast) exists only in a tree-sitter build, which no release ships github (https://github.com/yoanbernabeu/grepai/issues/392). go



RU vs EN. grepai’s docs say nomic-embed-text “is trained mostly on English” and recommend a multilingual model for other languages (embedders doc). githubgithub Synthetic’s default is nomic v1.5. RU query quality on text-embedding-3-small is UNKNOWN here [no source found]. Measure it in the canary.
Agent integration.

CLI: grepai search "<q>" --json --compact (or --toon); compact drops content for “~80% token savings” (CHANGELOG 0.10.0). github
MCP: grepai mcp-serve [project-path] over stdio, 14 tools, with a per-request root parameter since v0.37.0 githubgithub (https://yoanbernabeu.github.io/grepai/commands/grepai_mcp-serve/; CHANGELOG 1.0.0 “14 MCP tools”). github
A stdio framing fix for Windsurf (titleFixWriter, v0.31.0) shows stdout pollution has occurred before. githubgithub
Codex MCP configuration is not documented by grepai [no source found]. github
grepai agent-setup appends to existing .cursor/rules, .cursorrules, .windsurfrules, CLAUDE.md, GEMINI.md and AGENTS.md (https://yoanbernabeu.github.io/grepai/commands/grepai_agent-setup/, via subagent). github It mutates shared files: forbidden during the pilot.



Cold/warm one-shot behavior. Each CLI search is a fresh process that loads the GOB and calls the embedder for the query. There is no warm cache across CLI calls [inference from PR #402]. MCP is warm for config but still “loads and scans the whole index on every … MCP call” (PR #402). githubgithub
Requires local measurement: query latency including the network round-trip, peak RSS per search at our index size, RU recall, no-answer false positives, and serialized output bytes per call.


D. Call graph vs LSP


Precise mode does not participate in indexing. “precise mode was never wired into indexing. grepai watch and grepai index always use the regex extractor… output always reports \"mode\": \"fast\"” github (https://yoanbernabeu.github.io/grepai/trace/).

The tree-sitter extractor (Go, JS, TS, Python, PHP, C#, F#) exists behind the treesitter build tag with CGO. “Release binaries are built without it.” github
The maintainer declined to ship it: “would change default chunking and symbol extraction, forcing a reindex… out of scope for now” github (https://github.com/yoanbernabeu/grepai/issues/392, closed 2026-10-30).
Note the docs/skills conflict below: older skills still advertise --mode precise. playbooks



Documented resolution limits (counterexamples):

Same-name symbols: resolution picks a definition “deterministically (by file, then line)”, and “Callees of same-name definitions are still merged; Class::method scoping is not supported” (CHANGELOG 1.2.0, #372). githubgithub Two save() methods in different classes therefore share callers and callees. That is name-matching, not resolution.
Before v1.2.0: “MCP picks symbols[0] while CLI disambiguates” github (https://github.com/yoanbernabeu/grepai/pull/416, issue #372).
Python lexing: an apostrophe in a # comment hid every later call in the file until v1.0.0 (#280). github This is evidence of regex fragility.
Ruby: bare calls without parentheses are not found (trace docs). github
Elixir: excluded from the call graph because the tree-sitter pass would “attribute every real call to a caller named “def”” github (https://github.com/yoanbernabeu/grepai/pull/261).
Imports/aliases, overloads, inheritance/dispatch, callbacks, decorators, dynamic Python/JS/TS: no documentation of handling [no source found]. JS/TS refs handles “simple alias” property reads and writes only. github
PowerShell: not supported.



Interpretation: grepai’s trace is a name-keyed regex index. It displays callers and callees but carries no type, import or scope resolution. Do not equate it with LSP results, and do not rely on even tree-sitter syntax precision, which is unavailable.
Fair fixture protocol. Per language (Python, JS, TS, plus a PowerShell control expected to return nothing), build a ≤30-file fixture containing:

two classes with the same method name;
aliased imports (from a import f as g; import {f as g});
a decorated function;
a callback passed as an argument;
an overridden method called via the base type;
a dynamic getattr/obj[name]() call;
a re-export barrel (TS).


Seal source truth as a hand-annotated edge list. Run (a) grepai trace callers/callees --json; (b) native LSP find-references and call-hierarchy (Pyright, tsserver, PowerShell Editor Services); (c) CodeIndexer. Score edge precision and recall against the truth, separately for “displayed” vs “correct”.


E. Freshness


No watcher needed. grepai index “updates a project’s vector, symbol and optional RPG indexes once and exits, without starting a watcher or daemon” (v0.39.0, 2026-10-25). github It is incremental by mtime/hash. There is no --force flag (open request: https://github.com/yoanbernabeu/grepai/issues/397). github A full rebuild means deleting .grepai/index.gob/symbols.gob, or changing the model. github
Create/edit/rename/move/delete.

Edits reuse unchanged chunks (v1.0.0). Renames and moves re-embed github (v1.2.0, measured by the maintainer: previously “11 of 20 queries return a different top-10”, githubgithub https://github.com/yoanbernabeu/grepai/pull/410).
Watcher-side folder move/delete was fixed in v1.0.0 (#310), and atomic-write false deletions in v0.36.1 (#295). github Atomic writes had “emptied the index wholesale on git checkout”. github
Open or unverified: stale symbols for files deleted while no watcher ran (https://github.com/yoanbernabeu/grepai/issues/313, status unknown) and empty-chunk stale documents (https://github.com/yoanbernabeu/grepai/issues/352, status unknown). githubgithub Test deletion pruning explicitly in the one-shot mode.



Branches and worktrees. Worktrees are auto-detected and seeded under a writer lock (v0.29.0, v0.37.0). github With an explicitly named shared Qdrant collection, “checkouts on different branches still overwrite each other’s files” (CHANGELOG 1.2.0). githubgithub Use per-snapshot GOB.
Ignore rules and secrets.

Sources: .gitignore (nested), .grepaiignore with negation (v0.35.0), external_gitignore, and the ignore list. github Setting ignore “replaces the default list” (configuration docs, via subagent). github
The default list contains .git, .grepai, node_modules, vendor, bin, dist, __pycache__, .venv, venv, .idea, .vscode, target, .zig-cache, zig-out, qdrant_storage, with no secret patterns. .env is a supported extension (issue #367, https://github.com/yoanbernabeu/grepai/issues/367). github + 2
Secret exclusion must be added by us and verified by a dry scan.



Persistence and concurrency.

Atomic index.gob writes with corrupt-file quarantine (v0.36.0); failed replacement preserves the previous index (v0.37.0). github
“One lifetime writer per canonical project root, while read-only search, MCP, and trace processes remain concurrent” (v0.37.0); daemon PID lock held for the daemon’s whole life (v1.2.0, #357). github
Crash between delete and save fixed (#356); stamp written only after save (#311); previous chunks kept until batch embeddings succeed (#353). githubgithub



Paths and portability.

Paths are project-relative and slash-separated; Windows backslash indexes are rebuilt on v1.0.0 start (#329, https://github.com/yoanbernabeu/grepai/pull/329); Windows watcher deletes used backslashes until v1.0.0 (https://github.com/yoanbernabeu/grepai/pull/346). github + 2
Qdrant identity uses the normalized origin URL (v1.2.0). githubgithub GOB has no repo-identity or revision field [no source found].
Case-folding behavior across macOS/Windows: UNKNOWN.
Portability of a GOB index across OS: plausible because paths are relative and index-meta.json pins the embedder [speculative]. It must be tested by second-OS readback, not assumed.



Shared-index risk. Multiple clients may read concurrently, but there is one writer per root. Shared Qdrant collections pre-1.2.0 overwrote other projects (#247). githubgithub Never share an index across users or clients in the pilot.


F. Resource and cost evidence

MetricValueSource / statusGOB load+search, 10k/25k/50k/100k chunks (768-dim synthetic)69/208/458/800 ms; 62/156/312/625 MB file githubAuthor, PR #402 (2026-10-26) [single-source]Real repo (Go src, 54,610 chunks)494 MB index.gob; ~0.5 s and ~1.2 GB peak RAM per search githubgithubAuthor, PR #402 [single-source]grepai repo itself1,714 chunks githubAuthor, PR #402Independent RAM/CPU measurementsnone foundUNKNOWN“100–500 MB index for ~10,000 files”claimed “independent reviewer” rywalkerhttps://rywalker.com/research/grepai (~Jun 2026) [low-authority]Idle RAM (no watcher, CLI-only)0 between calls (no resident process)Inference from one-shot designCPU seconds per index/search, disk R/W bytes, network bytes, GPU/VRAM—UNKNOWN (GPU expected 0 with remote embeddings: inference)1536-dim vectors (OpenAI) vs 768-dimroughly 2× vector bytes vs author’s synthetic tableInference; UNKNOWN until measured


Why remote embeddings still leave local load. Vectors are stored and scanned locally in GOB. At 1536 dims, each chunk carries ~6 KB of float32 vectors plus chunk text [inference]. Every search deserializes the whole file. Remote embedding moves only the embedding compute off the machine.
The 97% claim.

The benchmark carries the disclaimer “conducted by the grepai maintainer”. The 97% figure is fresh input_tokens (51,147 → 1,326), while “cache_read_input_tokens dominated total token consumption”, so billed cost fell only 27.5% ($6.78 → $4.92) github (https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code/, 2026-01-21). githubgithub
A third-party guide notes “Benchmark par le mainteneur… Single-project validation (TypeScript only)… Pas de validation indépendante” github (https://github.com/FlorianBruniaux/claude-code-ultimate-guide/blob/main/guide/workflows/search-tools-mastery.md, 2026).
Contamination risks: wrapper and echo context is not counted; there is no accuracy oracle; the run predates v1.0 bug fixes; and the metric excludes cache reads.
Not usable for ranking.



Simplest on-demand architecture: a release binary, a per-snapshot .grepai/ (GOB), grepai index run manually before a session, and agent calls to grepai search … --json --compact via the existing shell tool. No daemon, no MCP server, no router, no guard.


G. Additive isolated canary (protocol)


Preregister (written and frozen before any run) the thresholds below.
Snapshot. Copy representative owned repos to C:\canary\snap-<date> (Hub). Include only extensions actually present among .py .js .ts .tsx .jsx; include .ps1 only if present, and then as a known-unsupported control.

Hash manifest: Get-ChildItem -Recurse -File | Get-FileHash -Algorithm SHA256 | Export-Csv manifest.csv.
Secret scan with an existing scanner (for example gitleaks if already installed). Delete hits from the snapshot. Rescan to zero.
Make it read-only after .grepai/ is created.



Baseline. The CodeIndexer owner supplies the common task protocol and runs the baseline unmodified on the same snapshot.
Tasks (≥40, sealed truth held by a non-answerer), balanced RU/EN: known-symbol lookup, exact string, semantic “where do we…”, navigation (callers/callees), and no-answer questions whose correct reply is “not present”.
Arms: baseline vs baseline+grepai (CLI only), with AB/BA order counterbalancing, independent answerer sessions and a blind judge.
Log per task: all serialized delivered context bytes and tokens, including tool wrappers and echoes; count of actual grepai invocations from process logs (not ACKs); correct/miss/false answer; wall time; peak RSS of each grepai process; provider-side token usage; cold (first call after index) vs warm (repeat) vs one-shot.
Freshness on copies only: create, edit, rename, move and delete files, then run grepai index and verify search and trace reflect the changes, including deleted-symbol pruning.
Preregistered thresholds (proposed):

functional PASS requires ≥1 successful search per task class and no secret file found in the index;
benefit PASS requires accuracy ≥ baseline −0, false answers not higher than baseline, a median serialized context reduction ≥20%, peak RSS per call ≤500 MB, provider spend ≤ the authorized cap, and zero stale results after the churn test.



Abort/rollback. Stop on any secret found in an embedded chunk, spend above the cap, a crash that leaves the index unusable, or any write outside the snapshot. Rollback: delete .grepai/, unset the env var, revoke the API key, remove the binary. Nothing else was touched.
Second-OS readback. Copy the snapshot plus .grepai/ to the Apple Silicon Mac Studio. Run grepai status, then 10 frozen queries with --json, and compare byte-for-byte with the Hub. Then run grepai index and expect “0 reindexed”; any re-embed means the index is not portable.


Concrete commands and configuration (pilot only; on snapshot copies)

powershell# Windows Hub — download release binary manually from GitHub Releases (v1.2.0), verify SHA256 from checksums file
# (avoid `irm https://grepai.dev/install.ps1 | iex` — pipes remote script)
$env:<key-var> = "<key from authorized, capped project>"   # session-scoped only
cd C:\canary\snap-2026-11
grepai init --provider openai --backend gob --yes               # flags per CHANGELOG 1.0.0 (#345); verify with --help
grepai index                                                    # one-shot, no watcher
grepai status
grepai search "where are expired tokens refreshed" --json --compact --limit 5
grepai search "где обновляются просроченные токены" --json --compact --limit 5
grepai trace callers "refresh_token" --json --compact

yaml# .grepai/config.yaml (snapshot only). Setting `ignore` REPLACES defaults, so re-list them.
version: 1
embedder:
  provider: openai
  model: text-embedding-3-small
  parallelism: 2
  max_retries: 2
store:
  backend: gob
chunking:
  size: 512
  overlap: 50
  # custom_extensions: [".ps1"]   # only if PowerShell present; exact schema unverified
search:
  hybrid: { enabled: true, k: 60 }
trace:
  enabled_languages: [".py", ".js", ".ts", ".jsx", ".tsx"]
ignore:
  - .git
  - .grepai
  - node_modules
  - vendor
  - bin
  - dist
  - __pycache__
  - .venv
  - venv
  - .idea
  - .vscode
  - target
  - .env
  - "*.env"
  - "*.pem"
  - "*.key"
  - "*.pfx"
  - "*cookie*"
  - secrets
framework_processing:
  frameworks:
    vue: { enabled: false }

bash# macOS (Mac Studio readback / Intel Mac Pro later)
brew install yoanbernabeu/tap/grepai     # documented install path
grepai version

MCP registration (a separate, opt-in arm only; it violates “no permanent stdio MCP per session” if left installed):

bashclaude mcp add grepai -s local -- grepai mcp-serve /abs/path/to/snapshot     # documented

json// .cursor/mcp.json in the snapshot only (documented format)
{ "mcpServers": { "grepai": { "command": "grepai", "args": ["mcp-serve", "/abs/path/to/snapshot"] } } }

toml# ~/.codex/config.toml — NOT documented by grepai; Codex-convention inference [speculative]
[mcp_servers.grepai]
command = "grepai"
args = ["mcp-serve", "/abs/path/to/snapshot"]

Do not run grepai agent-setup, grepai watch, or --background during the pilot.

H. Skeptical recommendation


Verdict: TRY (conditional), otherwise PARK [emerging].
Strongest argument against. The things grepai adds over a CodeIndexer-first workflow are fuzzy semantic recall and a regex call graph. The recall carries no exactness, no-answer or multilingual guarantee. The call graph merges same-name methods and cannot resolve imports, inheritance or PowerShell. In exchange you get a metered external dependency, a code-egress path (chunks plus paths), a whole-index load on every query, and a tool that shipped five versions in seven days. Ripgrep plus native LSPs plus CodeIndexer may already dominate.
Narrowly named consumer: Claude Code on the Windows Hub, CLI-only, on one sealed Python/TS snapshot.
Exact next experiment: the Section G canary at v1.2.0, text-embedding-3-small, GOB, ≤$5 authorized cap or proven existing credit, with Mac Studio readback.
No fleet PASS may follow from install, ACK or exit 0.


Contradictions

TopicSide ASide BAssessmentLatest versionReleases page render: v0.35.0 “Latest” (fetched 2026-11-01)CHANGELOG/PR #416/pkg.go.dev: v1.2.0, 2026-10-30; homepage shows both v1.2.0 and “Latest: v1.1.0” github + 2v1.2.0 is current; the releases render is stale. Homepage inconsistency is a docs lag.97% benchmark independencerywalker.com calls it “independent” and “rare… outside verification” [low-authority] rywalkerThe blog itself: “conducted by the grepai maintainer”; Bruniaux guide: no independent validation githubgithubMaintainer-run. The steelman for A: the numbers come from Claude Code logs and are reproducible in principle; but no one has published a reproduction.Precise modeOlder skills/docs: --mode precise “More accurate” playbooks (https://playbooks.com/skills/yoanbernabeu/grepai-skills/grepai-trace-callers)Current trace docs: never wired; flag deprecated githubCurrent docs win; skills were synced on 2026-10-30 github (https://github.com/yoanbernabeu/grepai-skills/pull/1). github“Your code stays on your machine”Homepage headline githubCloud providers send chunks and queries off-machineTrue only for the Ollama/LM Studio default. githubGOB RAM guidanceSkill table: 500 MB index → ~1 GB RAM agent-skillsPR #402: 494 MB → ~1.2 GB peak githubgithubRoughly consistent; both from the maintainer.Contribution modelEmbedders doc: “does not accept pull requests” (issue-first) githubgithubMany merged external PRs (#329, #330 open)Policy appears to have tightened recently; it affects fix latency.

Recommendations

Named options:


OPTION 1 — PARK now.

Trade-off: zero spend, zero egress, zero new surface; you forgo semantic recall.
Risk: missing a real benefit for vague “where is X done” queries.



OPTION 2 — TRY: budget-gated CLI canary (recommended). The Section G protocol on one snapshot, one OS and one client.

Trade-off: about 1–2 operator-days, cents of API spend, a controlled egress of owned code.
Risk: secret leakage if the ignore setup is wrong (mitigated by scan plus abort); stale-index bugs (#313/#352).



OPTION 3 — TRY via an existing Synthetic subscription. Viable only if one is already owned.

Trade-off: zero marginal spend.
Risk: retention/training policy unknown [no source found]; nomic v1.5 is mostly English, which is weak for RU.



OPTION 4 — Fleet MCP rollout. Rejected: it violates the no-permanent-stdio and no-shared-config constraints, and is unsupported by evidence.


Single recommendation: OPTION 2, converting to OPTION 1 if no authorization or existing credit is confirmed. Confidence: [emerging].

Main assumption: semantic recall on our owned Python/TS code adds correct answers that CodeIndexer misses, at lower total delivered context. This is untested and is exactly what the canary measures.

Caveats


Unresolved gaps: the v1.2.0 tag SHA verified directly; full go.mod and transitive licenses; the details of GO-2026-6128 and whether the unreviewed GO-2026-6125 still applies to v1.2.0; status of issues #313/#352; Synthetic and Requesty retention terms; Requesty pricing; Cursor API entitlements; RU retrieval quality; case-insensitive path behavior; GOB cross-OS portability; every resource metric marked UNKNOWN.
Source freshness: most product evidence is dated 2026-10-24 to 2026-10-30, so the codebase was changing daily up to our research date. The ChatGPT entitlement source is from 2024 and corroborated only by 2026 secondary sites.
Search budget: web search ran out before independent RAM tests could be checked. GO-2026-6125 has since been identified via OSV (unreviewed, references issue #247); only GO-2026-6128’s details are still unread.
Method: no code was executed, nothing was purchased, and no outreach was made. Statements labelled “inference” are the author’s reasoning, not source claims.


Evidence Table

ClaimSource URLDateTypeTierv1.2.0 = 2026-10-30, fixes + 2 behavior changeshttps://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md2026-10-30Owner changelog[established]Release PR merged as d86397d; go.mod unchanged since 1.1.0https://github.com/yoanbernabeu/grepai/pull/4162026-10-30Owner PR[established]MIT, v1.2.0 publishedhttps://pkg.go.dev/github.com/yoanbernabeu/grepai/indexer2026-10-30Registry[established]Precise mode never wired; release w/o tree-sitterhttps://yoanbernabeu.github.io/grepai/trace/fetched 2026-11-01Owner docs[established]Tree-sitter build declined, stabilization phasehttps://github.com/yoanbernabeu/grepai/issues/3922026-10-30Owner issue[established]Providers, keys, batch/retry, no env expansionhttps://yoanbernabeu.github.io/grepai/backends/embedders/fetched 2026-11-01Owner docs[established]Env key only to official HTTPS hosthttps://github.com/yoanbernabeu/grepai/pull/4112026-10Owner PR[established]GOB full-load per search; 1.2 GB peak at 54,610 chunkshttps://github.com/yoanbernabeu/grepai/pull/4022026-10-26Author measurement[emerging][single-source]Rename re-embed fix; 11/20 queries differedhttps://github.com/yoanbernabeu/grepai/pull/4102026-10Owner PR[established]Watcher dropped dotfiles; .env in SupportedExtensionshttps://github.com/yoanbernabeu/grepai/issues/3672026-10-25Issue[established]Stale symbols after offline deletehttps://github.com/yoanbernabeu/grepai/issues/3132026Issue[emerging]Empty-chunk stale documenthttps://github.com/yoanbernabeu/grepai/issues/3522026-10Issue[emerging]No force reindexhttps://github.com/yoanbernabeu/grepai/issues/3972026-10Issue[established]Hybrid = keyword + RRF, loads all chunkshttps://yoanbernabeu.github.io/grepai/hybrid-search/fetched 2026-11-01Owner docs[established]MCP stdio, mcp-serve argshttps://yoanbernabeu.github.io/grepai/commands/grepai_mcp-serve/2026Owner docs[established]agent-setup edits AGENTS.md/CLAUDE.md/.cursor/ruleshttps://yoanbernabeu.github.io/grepai/commands/grepai_agent-setup/2026-10-30Owner docs[emerging][single-source]97% = maintainer, fresh input tokens; −27.5% costhttps://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code/2026-01-21Author benchmark[established]Benchmark lacks independent validationhttps://github.com/FlorianBruniaux/claude-code-ultimate-guide/blob/main/guide/workflows/search-tools-mastery.md2026Third-party guide[emerging][low-authority]OpenAI API: no training; ≤30-day abuse logshttps://developers.openai.com/docs/guides/your-dataaccessed 2026-11-01Vendor policy[established]ChatGPT Plus excludes APIhttps://community.openai.com/t/paid-bill-but-api-keys-not-working/8963672024Vendor forum[established] (older)Claude API billed separately; Max/Team credits are Claude-API-onlysupport.claude.com, article 9876003accessed 2026-11-01Vendor help center[established]3-small $0.02/M, 3-large $0.13/Mhttps://developers.openai.com/api/docs/models/text-embedding-3-smallaccessed 2026-11-01Vendor model page[established]GO-2026-6125: unreviewed, all versions, refs #247/#248https://osv.dev/vulnerability/GO-2026-61252026-08-18Vulnerability DB[emerging]OpenRouter 3-small $0.02/Mhttps://openrouter.ai/openai/text-embedding-3-smallaccessed 2026-11-01Vendor page[established]OpenRouter itself retains nothing by defaulthttps://openrouter.ai/docs/guides/privacy/zdraccessed 2026-11-01Vendor policy[established]Synthetic embeddings no extra chargehttps://dev.synthetic.new/docs/api/modelsaccessed 2026-11-01Vendor docs[emerging][single-source]GroupEye Cloud product———[no source found]

Sources


https://github.com/yoanbernabeu/grepai (brief-provided; repository) — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/ — homepage, fetched 2026-11-01
https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md — latest entry 2026-10-30
https://github.com/yoanbernabeu/grepai/releases — fetched 2026-11-01 (stale render)
https://github.com/yoanbernabeu/grepai/pull/416 — 2026-10-30
https://github.com/yoanbernabeu/grepai/pull/411 — 2026-10
https://github.com/yoanbernabeu/grepai/pull/410 — 2026-10
https://github.com/yoanbernabeu/grepai/pull/402 — 2026-10-26
https://github.com/yoanbernabeu/grepai/pull/346 — 2026-10
https://github.com/yoanbernabeu/grepai/pull/329 — 2026-10
https://github.com/yoanbernabeu/grepai/pull/330 — 2026 (status unverified)
https://github.com/yoanbernabeu/grepai/pull/261 — 2026
https://github.com/yoanbernabeu/grepai/issues/392 — 2026-10-26 / closed 2026-10-30
https://github.com/yoanbernabeu/grepai/issues/367 — 2026-10-25 / closed 2026-10-26
https://github.com/yoanbernabeu/grepai/issues/313 — 2026 (date unverified)
https://github.com/yoanbernabeu/grepai/issues/352 — 2026-10
https://github.com/yoanbernabeu/grepai/issues/397 — 2026-10
https://github.com/yoanbernabeu/grepai/issues/135 — 2026-02
https://github.com/yoanbernabeu/grepai-skills/pull/1 — 2026-10-30
https://yoanbernabeu.github.io/grepai/trace/ — fetched 2026-11-01
https://yoanbernabeu.github.io/grepai/backends/embedders/ — fetched 2026-11-01
https://yoanbernabeu.github.io/grepai/hybrid-search/ — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/search-guide/ — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/configuration/ — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/mcp/ — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/commands/grepai_mcp-serve/ — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/commands/grepai_agent-setup/ — generated 2026-10-30
https://yoanbernabeu.github.io/grepai/installation/ — accessed 2026-11-01
https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code/ — 2026-01-21
https://pkg.go.dev/github.com/yoanbernabeu/grepai/indexer — v1.2.0, 2026-10-30
https://pkg.go.dev/github.com/yoanbernabeu/grepai/config — accessed 2026-11-01
https://osv.dev/vulnerability/GO-2026-6125 — published 2026-08-18
https://github.com/M0Rf30/PKGBUILD/pull/816 — 2026-10-26
https://github.com/M0Rf30/PKGBUILD/pull/842 — 2026-10-30/31
https://playbooks.com/skills/yoanbernabeu/grepai-skills/grepai-trace-callers — undated (pre-sync)
https://github.com/FlorianBruniaux/claude-code-ultimate-guide/blob/main/guide/workflows/search-tools-mastery.md — 2026 (date unverified)
https://rywalker.com/research/grepai — ~June 2026 [low-authority]
https://hysenlabs.com/projects/yoanbernabeu-grepai — ~Oct 2026 [low-authority]
https://developers.openai.com/docs/guides/your-data — accessed 2026-11-01
https://developers.openai.com/api/docs/models/text-embedding-3-small — accessed 2026-11-01
https://community.openai.com/t/paid-bill-but-api-keys-not-working/896367 — 2024
support.claude.com, article 9876003 (Anthropic Help Center) — accessed 2026-11-01
https://aionx.co/ai-comparisons/is-api-access-included-in-subscription/ — 2026 [low-authority]
https://costgoat.com/pricing/openai-embeddings — Oct 2026
https://www.silicondata.com/use-cases/openai-api-pricing-per-1m-tokens — Mar 2026
https://openrouter.ai/openai/text-embedding-3-small — accessed 2026-11-01
https://openrouter.ai/docs/guides/privacy/zdr — accessed 2026-11-01
https://dev.synthetic.new/docs/api/models — accessed 2026-11-01
https://aimarketing.ghost.io/synthetic-new-review-claude-code-drop-in/ — May 2026 [low-authority]

