# DR26-11-01-HUB-05-1439 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

DR26-11-01-HUB-05-1439 — grepai with REMOTE embeddings: product-specific gap research and operational decision input

Product under test: https://github.com/yoanbernabeu/grepai and https://yoanbernabeu.github.io/grepai/
Scope: fresh external research with primary-source citations. NOT a re-ranking of the 18-product overview of 2026-10-24/27; the earlier 40-answer comparison remains INVALID and none of its numbers are reused.

0. Executive summary

grepai is real, active, MIT-licensed, Go-based, and currently at v1.2.0 (2026-10-30). It is not a hosted cloud product: it is a local CLI/MCP tool that chunks code locally and can send chunk text to a remote embedding provider (OpenAI, OpenRouter, Synthetic, Requesty) while keeping vectors, chunk text, metadata, symbol graph and search execution on your machines (or on your own PostgreSQL/Qdrant). There is no official hosted grepai service. The dictation label "GroupEye Cloud" corresponds to no findable product; this report substitutes nothing and studies grepai only.

Key decision-relevant findings, all traceable to sections below:

Remote embeddings are well supported and cheap at our scale; a zero-new-spend route exists only conditionally (existing ChatGPT Pro API-credit benefit, existing Synthetic subscription, or OpenRouter's free embedding route) — otherwise a payment method is required, which is a hard budget gate.
Remote embeddings do NOT remove local RAM/storage costs: with the default GOB store the whole index (vectors + chunk text) is loaded into RAM on every search (~1.2 GB peak measured by the author at 55k chunks; [single-source, author docs]).
PowerShell is effectively unsupported: .ps1 is not indexed by default and has no trace (call-graph) patterns. Python/JS/TS are first-class for both search and trace.
The "precise/tree-sitter" mode is dead weight for us: it is behind a build tag, not wired into indexing, and release binaries are built without it. Trace is regex-based ("fast mode") in every official binary.
The famous 97% token claim is maintainer-run, single-codebase, no answer-quality metric, with +30% cache-read tokens offsetting it; net was −27.5% cost. Usable as hypothesis, not as evidence for our fleet.
Windows-specific risk is currently elevated: open issues report multi-GB RAM spikes during indexing (#361) and a watcher-stall deadlock under embed backpressure (#355), both reported on Windows with GOB+Ollama, both unfixed as of v1.2.0.
Secrets handling is our responsibility: .env files are indexed by default; there is no secret scanner; only gitignore/grepaiignore/ignore lists and a 1 MB size cap protect us. With a cloud embedder, ignored-nothing means transmitted-secrets.

Recommendation (section H): TRY — narrowly. On-demand grepai with remote embeddings on the two macOS machines, Python/JS/TS repos only, one-shot grepai index (no watcher, no MCP daemon), explicit .grepaiignore secret exclusion, gated on a named zero-spend key path. PARK for the Windows Hub until the two open Windows issues are fixed and PowerShell support is requested/decided. Confidence: [emerging]; main assumption: an existing allowance (ChatGPT-Pro-linked API credits or a Synthetic subscription) can actually be used, or the OpenRouter free embedding route is acceptable.

Research protocol and disclosure
Web searches actually performed: 35 substantive queries via the research service's web search (7 batches of 5, executed 2026-11-02), plus direct fetches of primary pages and a full clone of the repository for source inspection. No search counts were invented; this is the actual count.
Primary source inspection: repository cloned read-only at HEAD 9cf58bf41dcd45813ec697b2fcda43cbabb08c84 (2026-10-30 10:59 +0100, "docs: fix default trace languages and keyless server api_key note (#417)", consistent with v1.2.0 released 2026-10-30). Source files are cited as GitHub blob URLs on main. No grepai code was executed anywhere; nothing was purchased; no outreach was sent; only public web was accessed.
Failed/blocked accesses (disclosed): the Reddit thread https://www.reddit.com/r/ClaudeAI/comments/1qiv0d3/open_source_i_reduced_claude_code_input_tokens_by/ was found but not readable from this environment (Reddit block page); its existence and quoted comment are taken from the search result and the project README. The OpenAI pricing page fetch did not render embedding-model prices (page structure issue), so embedding prices are cited from third-party calculators cross-checked against OpenAI's 2024 announcement. The Synthetic /docs/api/embeddings page could not be fetched (tool budget exhausted mid-run); Synthetic claims rely on the official models/API-overview pages that did load, plus flagged third-party reviews.
Method: claims are labeled by origin — official statement (project docs/author), source behavior (read from cloned code), third-party measurement, or my inference. Confidence tiers: [established] / [emerging] / [speculative] / [fringe]. Single-source claims are tagged. Nothing from the invalid October 40-answer comparison is reused.
A. Identity and architecture
A.1 Identity verification [established]
Official repository: yoanbernabeu/grepai, tagline "Semantic Search & Call Graphs for AI Agents (100% Local)" / "grep for the AI era". https://github.com/yoanbernabeu/grepai (accessed 2026-11-02).
Official docs: https://yoanbernabeu.github.io/grepai/ ("Semantic code search and call graph tracing for your terminal and your AI agents. Runs locally.").
Author: Yoan Bernabeu (YoanDev), MIT License, "Copyright (c) 2026 Yoan Bernabeu" — https://github.com/yoanbernabeu/grepai/blob/main/LICENSE (read from clone).
Identity check vs dictation: the dictation string "GroupEye Cloud" matches no product found in 35 searches. grepai is the only nominated product matching the URL given. There is no product, page, or vendor operating under "GroupEye Cloud" [no source found for any "GroupEye Cloud" product; label: dictation error, not silently substituted].
Maturity: 1.9k stars, ~160 forks, 283 commits, 56–58 tags/releases, 46 contributors "since January 2026" (docs site, fetched 2026-11-02). ~25,584 downloads per the landing page crawl of ~2026-10-25 [single-source snapshot, count moves daily].
Is there an official hosted service? No. The docs, README and source describe a local CLI (grepai), a local MCP stdio server (grepai mcp-serve), and self-managed stores. No grepai-operated indexing/search endpoint exists in the code or docs. The only network calls in the embedder go to the embedding provider you configure (factory: https://github.com/yoanbernabeu/grepai/blob/main/embedder/factory.go). All "cloud" aspects are (a) third-party embedding APIs and (b) optionally your own PostgreSQL/Qdrant, including Qdrant Cloud. [established, source behavior]
A.2 Version, revision, runtime, license [established]
ITEM
	
VALUE
	
SOURCE

Latest stable	v1.2.0, released 2026-10-30	https://github.com/yoanbernabeu/grepai/releases (fetched 2026-11-02)
Prior	v1.1.0 (2026-10-26), v1.0.0 (2026-10-25)	same
Prereleases	none visible on the releases page	same
main HEAD	9cf58bf41dcd45813ec697b2fcda43cbabb08c84 (2026-10-30)	clone, 2026-11-02; https://github.com/yoanbernabeu/grepai/commits/main
Go version	go 1.25.5	https://github.com/yoanbernabeu/grepai/blob/main/go.mod
License	MIT	repo LICENSE
Language stat	"C 92.6%, Go 7.4%" on GitHub — the C is vendored tree-sitter grammar sources (github.com/smacker/go-tree-sitter), not a C product	https://github.com/yoanbernabeu/grepai
Key deps	mark3labs/mcp-go v1.3.0 (MCP), qdrant/go-client v1.20.0 (gRPC), jackc/pgx/v5 v5.11.0 + pgvector/pgvector-go v0.4.1 (Postgres), smacker/go-tree-sitter (parse, build-tagged), spf13/cobra, fsnotify/fsnotify v1.10.1	go.mod
Release binaries	built without CGO; the experimental ast chunking needs a self-built binary with -tags treesitter + CGO	https://yoanbernabeu.github.io/grepai/installation
Release-channel hygiene	v1.2.0 includes fix(embedder): only send environment API keys to official endpoints (#411) and docs(security): warn about indexing untrusted repositories (#415)	releases page

Note on divergence: the docs landing crawl of ~Oct 25 showed "Latest release v1.0.0" while a fetch on 2026-11-02 showed "v1.2.0" — a crawl-freshness artifact, resolved in favor of the releases page (v1.2.0). Release count likewise differs (56 vs 58) for the same reason. [established]

A.3 Where everything actually lives [established, source behavior]

grepai's dataflow, read from the code (config constants: https://github.com/yoanbernabeu/grepai/blob/main/config/config.go):

ARTIFACT
	
LOCATION
	
SENT TO EMBEDDING PROVIDER?
	
NOTES

Original code files	your disk only	Chunk text is sent to the remote embedder during indexing (POST {endpoint}/embeddings, e.g. https://github.com/yoanbernabeu/grepai/blob/main/embedder/openai.go)	nothing else transmits code
Chunks	.grepai/index.gob (GOB) or chunks/documents tables (Postgres) or Qdrant points	no	chunk text starts with a File: <path> header (search docs)
Vectors	same store as chunks	no	768/1536/3072/4096 dims per model
Metadata (path, hash, mtime)	same store; .grepai/index-meta.json records provider/model/dims	no	https://github.com/yoanbernabeu/grepai/blob/main/config/index_meta.go
Call graph / symbols	.grepai/symbols.gob — regex-extracted locally, never embedded	no — zero network	trace docs: "queries never call your embedding provider" (https://yoanbernabeu.github.io/grepai/trace)
RPG graph (opt-in, default off)	.grepai/rpg.gob; optional local LLM summarization via configured endpoint (default http://localhost:11434/v1)	no (unless you point RPG's LLM at a cloud endpoint)	https://yoanbernabeu.github.io/grepai/rpg/
Queries	query text → embedder on every search	yes, query text is transmitted on each search	search flow: https://yoanbernabeu.github.io/grepai/search-guide
Rerank	none exists; ranking = cosine similarity × local path-boost factors, or RRF fusion of vector + local keyword match	—	https://yoanbernabeu.github.io/grepai/hybrid-search/

Local index with remote embeddings vs vendor-hosted indexing/search: grepai is strictly the former. Vendors (OpenAI/Synthetic/…) only ever see chunk text or query text; they store nothing for you (except their abuse-log retention windows) and return vectors. Search execution is local cosine distance (GOB: in-memory full scan) or runs in your own Postgres/Qdrant. Qdrant Cloud is a third party hosting your vectors + chunk text if you choose it — that is a decision, not a default. [established, source behavior]

Backends actually implemented (store factory + docs https://yoanbernabeu.github.io/grepai/backends/stores/):

gob (default): single in-memory index serialized to .grepai/index.gob, atomic temp-file replace, cross-process file lock, self-heal to .corrupt.
postgres + pgvector: shared index, per-project project_id = absolute path; no ANN index — full cosine scan per search.
qdrant (gRPC, default port 6334; TLS + API key for Qdrant Cloud): collection per project path or shared collection with a project_namespace derived from the git origin URL.
A.4 v1.0.0 vs prerelease divergence

No prerelease channel exists. The only "divergence" that matters operationally: release binaries (no CGO, no treesitter tag) vs source builds — see D. Version velocity is high (three releases in six days at the end of October 2026); any pilot must pin the binary version and record grepai version output. [established]

B. Cloud path: remote embeddings
B.1 Providers as currently implemented [established, source behavior]

Six providers are compiled in (factory: https://github.com/yoanbernabeu/grepai/blob/main/embedder/factory.go; docs: https://yoanbernabeu.github.io/grepai/backends/embedders/):

PROVIDER
	
RUNS
	
DEFAULT MODEL
	
DEFAULT DIMS
	
KEY ENV VAR
	
ENDPOINT (DEFAULT)

ollama (default)	local	nomic-embed-text	768	—	http://localhost:11434
lmstudio	local	text-embedding-nomic-embed-text-v1.5	768	—	http://127.0.0.1:1234
openai	cloud	text-embedding-3-small	1536 (3072 for 3-large)	<key-var>	https://api.openai.com/v1
synthetic	cloud	hf:nomic-ai/nomic-embed-text-v1.5	768	SYNTHETIC_API_KEY (falls back to <key-var>)	https://api.synthetic.new/openai/v1
openrouter	cloud	openai/text-embedding-3-small	1536 (3072 / 4096 for 3-large / qwen3-embedding-8b)	OPENROUTER_API_KEY (falls back to <key-var>)	https://openrouter.ai/api/v1
requesty	cloud	openai/text-embedding-3-small	1536 / 3072	REQUESTY_API_KEY (falls back to <key-var>)	https://router.requesty.ai/v1

Any OpenAI-compatible endpoint also works via provider: openai + custom endpoint (Azure, LiteLLM, vLLM, TEI — docs, same page).

Secret handling (verified in code + hardening shipped in v1.2.0): environment API keys (<key-var> etc.) are only sent to the provider's official host over HTTPS; if endpoint in .grepai/config.yaml points anywhere else, grepai refuses to run rather than leak the env key to a config that may have arrived with a cloned repo. A custom endpoint requires an explicit api_key in config. config.yaml does not expand ${VARS} — a literal api_key: ${<key-var>} would be sent as text (documented footgun). Sources: https://github.com/yoanbernabeu/grepai/blob/main/embedder/env_key.go and embedders doc. [established]

Batch/retry/cache config (openai provider only for batch+retry): parallelism (default 4), max_retries on HTTP 429/5xx (default 5; exponential backoff 1s→32s cap + jitter; Retry-After honored ≤60s; halve workers after 3 consecutive 429s, regrow after 10 successes; oversized batches split), max_batch_size (default 2000 inputs), max_batch_tokens (default 280,000 estimated tokens; ~4 chars/token estimate). Non-openai providers embed file-by-file. Embedding cache: unchanged chunks reuse stored vectors in all three stores; a CacheNamespace (provider+model+dims+endpoint, no secrets) keys reuse; v1.2.0 fixed vectors being reused across different file paths and keeps old chunks until a batch succeeds (release notes; indexer code: https://github.com/yoanbernabeu/grepai/blob/main/indexer/indexer.go). [established]

What is transmitted: chunk text (with File: <path> header) during indexing; query text during search; nothing during trace/refs/RPG (local). Vectors flow from the provider, never to a search vendor. [established, source behavior]

B.2 Provider policies: training / retention / deletion
PROVIDER
	
TRAINING ON YOUR API DATA?
	
RETENTION
	
DELETION / ZDR
	
SOURCE & DATE

OpenAI API (embeddings)	No (API data not used for training since 2023-03-01 unless opted in)	up to 30 days abuse-monitoring logs	Zero Data Retention available for approved orgs; /v1/embeddings is listed among ZDR-supported endpoints	Official: https://developers.openai.com/api/docs/guides/your-data (fetched via summaries, checked 2026-09-26 per below); secondary summaries dated 2026-07-28 https://humla.team/blog/openai-data-retention-policy and https://meetily.ai/llm-privacy/openai (checked 2026-09-26) [established core, via official page + two concordant secondary sources]
OpenRouter	Depends on the underlying provider and your OpenRouter logging settings — I did not verify OpenRouter's per-route logging/retention for embeddings	UNKNOWN	UNKNOWN	https://openrouter.ai/docs/api_reference/embeddings (fetched 2026-11-02; documents endpoint, models, no-streaming; silent on retention) [gap flagged]
Synthetic	"not training on user data and not storing prompts" per third-party review [single-source, low-authority]; official pages fetched do not state retention	UNKNOWN (official)	UNKNOWN	https://dev.synthetic.new/docs/api/models and https://dev.synthetic.new (fetched 2026-11-02); claim: https://ai-deck.app/blog/synthetic (Aug 2026)
Requesty	UNKNOWN	UNKNOWN (platform markets request observability/logging)	UNKNOWN	https://docs.requesty.ai/api-reference/endpoint/embeddings-create (2026-05-26) — documents endpoint + models only

Chat subscription vs API entitlement (critical budget fact):

ChatGPT Plus/Go/Free include no platform API credits. ChatGPT Pro ($100/$200/$500 tiers) and eligible Business seats can claim a monthly API-credit benefit ($100/$200/$500 per month; Business $20/Standard seat, $100/Premium seat, $500 workspace cap) — official help page, updated ~2026-11: https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions. These credits fund <key-var> usage including embeddings. [established]
Anthropic Max/Ultra/Team monthly API credits (announced ~2026-10-08; official FAQ: https://support.claude.com/en/articles/10366389-how-can-i-get-higher-rate-limits-on-the-claude-api) pay Anthropic API only — Anthropic has no embedding endpoint, so they cannot pay for grepai embeddings. [established]
Therefore: "existing subscription allowances first" works only if the operator holds ChatGPT Pro/Business (OpenAI credits), an existing Synthetic subscription, or existing OpenRouter/Requesty credits. A bare Claude/ChatGPT-Plus subscription covers nothing of the embedding path. This is the precise budget gate. [established]
B.3 Dated pricing and scenario formulas

Prices as published, with units:

PRICE POINT
	
VALUE
	
SOURCE & DATE

OpenAI text-embedding-3-small	$0.02 / 1M tokens (standard); $0.01/1M via OpenAI Batch API (which grepai does not use)	https://costgoat.com/pricing/openai-embeddings (Oct 2026) [low-authority]; concordant: https://www.cloudzero.com/blog/openai-pricing (2026), https://zilliz.com/ai-models/text-embedding-3-small; origin: OpenAI's 2024-01-25 announcement set $0.02/1M (5× cut vs ada-002) https://openai.com/index/new-embedding-models-and-api-updates
OpenAI text-embedding-3-large	$0.13 / 1M tokens	same sources
OpenRouter openai/text-embedding-3-small	$0.02 / 1M tokens	https://openrouter.ai/collections/embedding-models (rankings updated Oct 2026; fetched 2026-11-02)
OpenRouter qwen/qwen3-embedding-8b	$0.01 / 1M tokens (multilingual, 4096 dims, ~100+ languages)	same + https://openrouter.ai/blog/insights/best-embedding-models-2026 (2026-09-23)
OpenRouter nvidia/nemotron-3-embed-1b:free	$0 (free route, 32,768-token context)	https://openrouter.ai/blog/insights/best-embedding-models-2026 [single-source for the route's persistence; free-tier rate limits unpublished]
Synthetic embeddings	included in every subscription; third-party reviews say embedding requests don't count against the subscription rate limits; plan price quoted as $30/month in two 2026 reviews, but an older undated source says $20 Standard / $60 Pro — current price unconfirmed	https://dev.synthetic.new/docs/api/models ("Always-On and Embeddings models are included in every subscription", fetched 2026-11-02); $30: https://ai-deck.app/blog/synthetic (Aug 2026) [single-source], https://aimarketing.ghost.io/synthetic-new-review-claude-code-drop-in [low-authority]; $20/$60 conflict: https://tomrochette.com/agents/model-access/synthetic [single-source, undated]
Requesty	per-model price + platform fee; one source says ~5% on model spend [single-source]	model page: https://docs.requesty.ai/api-reference/endpoint/embeddings-create (2026-05-26); fee: https://www.getmaxim.ai/articles/requesty-alternatives (~2026-10-30)
Qdrant Cloud (if ever used as shared store)	free tier: 1 GB RAM / 4 GB disk, no credit card; paid from ~$25/mo	https://qdrant.tech/documentation/cloud/create-cluster; https://qdrant.tech/pricing

Scenario formulas (all ESTIMATES; grepai's own cost note uses ~4 chars ≈ 1 token: https://yoanbernabeu.github.io/grepai/backends/embedders/). Overhead factor: default chunking is 512 tokens with 50 overlap (config source: https://github.com/yoanbernabeu/grepai/blob/main/config/config.go), so tiled re-embedding of overlaps ≈ ×1.11 embedded tokens vs raw file tokens.

Let P = price per 1M tokens, T = code tokens.

Initial full index: cost ≈ T × 1.11 × P / 10^6
100k code tokens @ 3-small ($0.02): 100,000 × 1.11 × 0.02/10⁶ ≈ $0.0022 (~one-fifth of a cent)
1M code tokens @ 3-small: ≈ $0.022; @ 3-large: ≈ $0.145; @ OpenRouter qwen3-8b: ≈ $0.011
1k queries @ ~20 tokens each: 20k tokens ≈ $0.0004 @ 3-small (queries are embedded one-at-a-time per search call; HTTP overhead dominates, not cost)
Incremental churn: cost/day ≈ (changed file tokens) × 1.11 × P / 10^6. At 5% daily rewrite of a 1M-token repo: 50k tokens → ≈ $0.0011/day ≈ $0.033/month @ 3-small.
Retries: grepai re-sends a batch after 429/5xx (≤5 attempts). A failed request presumably bills nothing, so retry cost ≈ $0 unless partial successes are billed; bill effects of partial batch failure: UNKNOWN [no source found — labeled assumption].
Embedded-caching: unchanged chunks cost $0 on re-index (all stores reuse identical-content vectors — https://yoanbernabeu.github.io/grepai/backends/stores/).

Zero-new-spend pilot routes (evidence-checked):

ChatGPT Pro credit benefit → provider: openai. If the operator's org has ChatGPT Pro ($100+) or eligible Business seats, claim the monthly API credits and link them to an API org; embeddings spend draws on that. Official: https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions. No new card needed. [established] — budget gate: requires an eligible plan actually held; Plus/Go do not qualify.
Existing Synthetic subscription → provider: synthetic. If a Synthetic subscription already exists, embeddings are included; zero marginal spend. [established for inclusion; single-source for "doesn't count against rate limits"]
OpenRouter free embedding route → provider: openrouter, custom model. nvidia/nemotron-3-embed-1b:free exists per OpenRouter's 2026-09 blog; grepai's openrouter provider accepts arbitrary models with explicit dimensions. Zero spend, unknown quality/limits. [emerging, single-source]
Otherwise: no evidenced zero-spend route. OpenAI/OpenRouter/Requesty are metered prepaid APIs; creating an account implies adding a payment method = new spend authorization = budget gate not passable without explicit authorization. [established]
C. Search and languages
C.1 Search modes and behavior [established]
Semantic (default): query → vector → cosine similarity over the store; fetch 2× candidates (4× with dedup) → local path-boost factors → optional per-file dedup → top-N. No minimum-score threshold exists in code (https://github.com/yoanbernabeu/grepai/blob/main/search/search.go): grepai always returns the top-k closest chunks, however dissimilar; "No results found." appears only when zero candidates exist. Implication for the canary: "no-answer" discipline must come from the calling agent, not the tool. [established, source behavior]
Hybrid (opt-in): local keyword match fused with vector ranking via Reciprocal Rank Fusion, k=60 default (https://yoanbernabeu.github.io/grepai/hybrid-search/). Docs explicitly say: exact identifiers → enable hybrid "or use a text search tool such as grep". So lexical/exact search inside grepai is deliberately minimal.
Path filter: --path prefix filter on search (https://github.com/yoanbernabeu/grepai/blob/main/cli/search.go).
Duplicates: with search.dedup.enabled, only the best chunk per file is kept; otherwise multiple chunks of one file can appear.
Output: text, --json, --toon (token-frugal tabular format), --compact (drop content) — https://yoanbernabeu.github.io/grepai/search-guide.
Scores: cosine×boost without hybrid (can exceed 1.0 with boost); small RRF values (~≤0.03) with hybrid.
C.2 Language coverage — the honest matrix [established, source behavior]

Indexed-by-default extensions live in SupportedExtensions (https://github.com/yoanbernabeu/grepai/blob/main/indexer/scanner.go): ~60 extensions incl. .py, .js, .ts, .jsx, .tsx, .go, .rs, .java, .c/.cpp/.h, .cs, .php, .rb, .vue, .svelte, .sh/.bash/.zsh, .sql, .yaml/.yml/.json/.xml/.toml/.ini/.cfg/.conf, .env, .tf/.hcl, .md/.txt, plus .r, .lua, .dart, .ex, .clj, .hs, .zig, .nim, .pas, etc. .ps1/.psm1 (PowerShell) are absent — PowerShell files are indexed only if you add them via chunking.custom_extensions (docs: https://yoanbernabeu.github.io/grepai/watch-guide).

Trace (call graph) languages (regex patterns: https://github.com/yoanbernabeu/grepai/blob/main/trace/patterns.go; default trace.enabled_languages in config): Go, JS/JSX, TS/TSX, Vue, Python, PHP, Lua, GDScript, Ruby, C/C++ headers, Rust, Zig, C#, Java, F#, Pascal/Delphi. No PowerShell. No shell. No Kotlin/Swift for trace. refs (property read/write) is JS/TS/Vue-only (trace docs).

Chunk ingestion vs symbol parsing are separate pipelines: chunking = fixed-size text windows (512 tokens, 50 overlap) for embedding; optional experimental AST chunking behind the treesitter build tag (not in release binaries); symbols = regex extraction over trace.enabled_languages. So semantic search quality does not depend on the symbol parser, and vice versa. [established]

C.3 RU vs EN queries and model-dependent multilingual quality
Official guidance: "Use English: most embedding models work best with English queries" (https://yoanbernabeu.github.io/grepai/search-guide). [established]
Default local model nomic-embed-text is English-centric; the multilingual option in the same family is nomic-embed-text-v2-moe (~100 languages, 768 dims, 512-token context — https://ollama.com/library/nomic-embed-text-v2-moe; paper: https://static.nomic.ai/nomic_embed_multilingual_preprint.pdf, 2025). [established]
OpenAI 3-small improved multilingual retrieval vs ada-002 (31.4%→44% on the then-benchmark, 2024-01: https://sdtimes.com/ai/openai-announces-new-models-and-api-updates); for strong RU support OpenRouter's own 2026 guide recommends qwen/qwen3-embedding-8b (100+ languages) (https://openrouter.ai/blog/insights/best-embedding-models-2026). [established at model level]
RU query quality inside grepai specifically: no measurements exist [no source found]. Requirement for our canary: RU/EN task pair per section G.
Code-identifier search across RU comments/EN identifiers is model-dependent and unmeasured for every candidate model — must be measured locally.
C.4 Agent integration, transports, lifetimes [established]
CLI: plain stdout; --json/--toon for structured framing. One-shot friendly: grepai search, grepai trace … are stateless single processes.
MCP server: grepai mcp-serve speaks stdio (https://github.com/yoanbernabeu/grepai/blob/main/cli/mcp_serve.go) via mark3labs/mcp-go. Tools: grepai_search, grepai_trace_callers/callees/graph, grepai_refs_readers/writers/graph, grepai_index_status, grepai_list_workspaces, grepai_list_projects, grepai_rpg_search/fetch/explore, grepai_stats (https://github.com/yoanbernabeu/grepai/blob/main/mcp/server.go). It runs per client session, for the lifetime of the client that spawned it — it is not a system daemon; no permanent background MCP is required. [established, source behavior]
Skills/subagent: grepai agent-setup writes usage instructions into CLAUDE.md / AGENTS.md / GEMINI.md / .cursor/rules / .windsurfrules and creates a Claude Code subagent .claude/agents/deep-explore.md (https://github.com/yoanbernabeu/grepai/blob/main/cli/agent_setup.go). A separate repo ships 27 agent skills (MIT): https://github.com/yoanbernabeu/grepai-skills. Note: agent-setup mutates repo-visible files — must be applied only in the pilot sandbox copy during the pilot (no shared configuration changes).
Cold/warm: every CLI search and every MCP tool call loads the whole index (GOB) then scans it; a running watcher does not keep a warm in-memory index for clients — the file lock just serializes access. Warm vs cold is therefore mostly OS file-cache effect, not process residency. [established, source behavior + stores docs]
Index sizes/RAM: see F.
C.5 Claims requiring local measurement

RU query quality; score-threshold behavior on our repos; GOB search latency at our chunk counts; PowerShell-free coverage of our Hub scripts; TOON vs JSON token cost; actual cold/warm wall time on macOS/Windows. All unmeasured for our environment. [no source found]

D. Call graph (trace) vs LSP: what precise mode really is
D.1 The build-tag truth [established, source behavior]
A tree-sitter extractor and AST chunker exist in the source tree behind //go:build treesitter (CGO required): https://github.com/yoanbernabeu/grepai/blob/main/trace/extractor_ts.go, https://github.com/yoanbernabeu/grepai/blob/main/indexer/chunker_ast.go, covering Go, JS, TS, Python, PHP, C#, F#.
The official docs state plainly: "precise mode was never wired into indexing. grepai watch and grepai index always use the regex extractor. The --mode flag of trace callers|callees|graph is deprecated … the output always reports "mode": "fast"." And: "Release binaries are built without it." (https://yoanbernabeu.github.io/grepai/trace; installation docs confirm no-CGO release builds: https://yoanbernabeu.github.io/grepai/installation). The Makefile release builds have no -tags treesitter (https://github.com/yoanbernabeu/grepai/blob/main/Makefile). [established]
So for us: precise mode does not participate in indexing and cannot participate in any official binary. Any experiment with it requires self-compiling a nonstandard binary — out of scope for an isolated pilot.
D.2 What the regex "fast mode" actually resolves [established, source behavior]

Symbols = regex matches for function/method/class/interface definitions per language; references = call-site regex attributed to the enclosing function; caller→callee edges derived in-memory; persisted in symbols.gob; a manual regexExtractorVersion ("regex-v3-python-gdscript") invalidates caches when patterns change (https://github.com/yoanbernabeu/grepai/blob/main/trace/extractor.go).

Known-correct behaviors recently fixed by the author (release notes, https://github.com/yoanbernabeu/grepai/releases):

v1.2.0: "fix(trace): report real callees and pick same-name definitions deterministically" — same-name symbols are ordered by file, then line (https://github.com/yoanbernabeu/grepai/blob/main/trace/resolve.go: "symbolLess orders same-name definitions by file, then line… so the choice never depends on the [map iteration]"). So ambiguous same-name resolution exists but is lexicographic, not semantic. [established]
v1.0.0: GDScript same-line annotations, comment masking.

What it cannot do (by construction, no dynamic resolution): imports/aliases (import x as y call forms are matched textually at best), overloads (all definitions share one name node), class-method dispatch and inheritance (a call obj.method() is attributed to any same-name definition, not the receiver's class), callbacks/lambdas as named symbols, decorators that rename, dynamic Python (getattr, monkey-patching), dynamic JS/TS (obj[var], proxies), PowerShell (unsupported entirely). This is my inference from the regex architecture, consistent with the docs' decision to keep the deprecated mode and with the author's own caveat. Label: [established architecture, inferred per-case behavior]. It is exactly the "syntax precision ≠ semantic LSP resolution" distinction: grepai's trace is a textual candidate list, useful for "who might call this", not a compiler-grade reference graph. Callers/callees display exists; correctness for dynamic dispatch is not claimed by the project and must not be assumed by us.

Source-linked counterexamples to expect in fixtures (predicted, to be confirmed in the canary): Login defined in admin/auth.py and portal/auth.py → trace callers "Login" picks one deterministically by path/line, not by import context; TS Handler.handle overridden in subclasses → callers of the base name conflate all; Python from auth import login as authenticate → call sites of authenticate may not match.

D.3 Fair fixture protocol against native language servers and source truth
Build a frozen fixture repo per language (Python 3.12, TS 5.x, PowerShell 7) containing, each: unique-name function; same-name functions in two modules; import-alias usage; class-method override chain (base + 2 subclasses); decorator-wrapped function; dynamic dispatch (getattr) case; callback passing.
Source truth: generate the expected caller/callee edge list mechanically from the fixture construction script (we know ground truth because we wrote the calls), sealed from answerers.
Runs: (a) grepai trace on the fixture; (b) native LSP (Pyright / tsserver / PowerShell Editor Services) queried via a scripted LSP client; (c) ripgrep baseline.
Score each on: precision/recall of caller sets, callee sets, and cross-file dispatch resolution; record wrong-symbol attributions separately from misses.
Decide trace's role: if trace ≈ rg for our code, its only added value is convenience, not correctness.
E. Freshness, indexes, portability
E.1 Initial indexing vs watcher [established]
One-shot exists and is first-class: grepai index "runs the same initial synchronization as grepai watch … then exits. It does not start a watcher or daemon. Unchanged files are scanned but not re-embedded." (https://yoanbernabeu.github.io/grepai/watch-guide). This is the user's "no seven background watchers" requirement: satisfied — run grepai index on demand, no watcher.
Watcher (optional): fsnotify events (create/modify/delete/rename) debounced 500 ms; per-file re-index; hash-skip; watch.last_index_time enables fast restarts (mtime-gated re-hash). Background daemon mode exists (--background/--status/--stop, per-worktree PID files with flock; v1.2.0 holds PID locks while running).
Fail-safe design: watcher refuses to serve a stale index — if it cannot keep full coverage (directory unwatchable, fsnotify overflow, root removed), it stops rather than continue (watch-guide). Editor atomic-save pattern (write temp + rename over original) is treated as modify, not delete, when the file still exists.
Concurrency: single-writer via .grepai/writer.lock; grepai index fails while a watcher runs on the same project; readers use index.gob.lock. Crash-released locks; self-healing corrupt-index quarantine; v1.2.0 keeps previous chunks until a re-embed batch succeeds (no half-updated files on failure). [established]
E.2 Rename / move / delete / re-embedding
Delete → chunks removed (DeleteByFile); rename/move → file re-indexed under the new path. Re-embedding on rename is normally avoided because all stores reuse the stored vector of an identical-content chunk, and the GOB reuse path is content-hash based (https://github.com/yoanbernabeu/grepai/blob/main/store/gob.go; issue #360 describes it). Caveats shipped in v1.2.0: "stop reusing vectors embedded under another file path" and "keep previous chunks until batch embeddings succeed" — i.e., rename reuse had real bugs in ≤v1.1; treat v1.2.0 as the minimum acceptable version. Also: chunk-ID deletion fix (v1.2.0 #406) means earlier versions could leave orphan chunk IDs. [established]
Branch/worktree changes: git worktrees are supported explicitly — linked worktrees auto-init by copying config.yaml + index.gob + symbols.gob from the main worktree, then keep independent indexes; Postgres/Qdrant worktrees start empty under their own path key (https://yoanbernabeu.github.io/grepai/git-worktrees/). Plain branch switches inside one worktree are handled as ordinary file modifications (branch-switch tests exist: https://github.com/yoanbernabeu/grepai/blob/main/indexer/indexer_branchswitch_test.go); Qdrant docs warn that two checkouts of the same repo sharing a collection overwrite each other's file versions.
Ignore rules: root+subdir .gitignore, .grepaiignore (gitignore syntax, supports ! re-include), ignore list in config (matches names, not globs), external_gitignore, minified-file skip, >1 MB skip, binary (non-UTF-8/null-byte) skip. No secret scanning anywhere in the indexer (grep of indexer package: no secret/credential logic). .env is a supported extension (indexed). SECURITY.md: "When using OpenAI: code content is sent to OpenAI's API" (https://github.com/yoanbernabeu/grepai/blob/main/SECURITY.md). → Secret exclusion is 100% our configuration's job (see G). [established]
E.3 Paths, portability, multi-user
Storage keys are repository-relative paths; Postgres keys rows by the project's absolute path; Qdrant collection names/project_namespace derive from absolute path or normalized origin URL (host-lowercased, credentials dropped) (https://yoanbernabeu.github.io/grepai/backends/stores/). Windows path normalization to forward slashes shipped in v1.0.0 after a watch-orphan bug; case/drive-letter handling for Windows is only partially documented — case-insensitive filesystems and drive moves are untested territory [gap flagged; no source found].
GOB index is explicitly not portable across machines ("Local only: the file cannot be shared across machines") and each worktree has its own. Portable/shared indexing requires Postgres/Qdrant. Reindex (not copy) is the only safe transfer for GOB. [established]
Multi-user/multi-client shared index risks (documented by the author): shared Qdrant collection with an older-version client "can overwrite or delete the other projects' points"; same-repo checkouts on different branches sharing a store overwrite each other; namespace changes (origin URL change) force full reindex and leave orphan points to clean by REST filter (stores doc, "Sharing a Collection"). For a solo pilot these are manageable; for fleet sharing they are real operational hazards. [established]
F. Resource and cost evidence
F.1 The 97% claim, inspected [established as a claim; unproven for us]

The claim originates from the maintainer's benchmark post (2026-01-21): https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code — Excalidraw (155k LOC TypeScript), Claude Code, MacBook Pro M3 Pro, grepai with local Ollama nomic-embed-text, 5 identical questions, 5 runs "with consistent results", metrics from Claude Code JSON logs:

METRIC
	
BASELINE
	
GREPAI
	
Δ

input_tokens	51,147	1,326	−97%
tool calls	139	62	−55%
cache_creation_tokens	563,883	162,289	−71%
cache_read_tokens	5,973,161	7,775,888	+30%
billed cost	$6.78	$4.92	−27.5%
subagents	5	0	−100%

Assessment against contamination criteria:

Budget/context: real billed-token accounting, good; the headline −97% is the smallest cost component (fresh input ≈ 3.8% of baseline cost); the honest number is −27.5%.
Oracle contamination: questions written by the author of the tool against a codebase with well-named TS symbols; no evidence questions were authored blind to grepai's strengths; no answer-quality metric at all ("both approaches found correct solutions" — asserted, not scored). Baseline's 5 subagent launches vs 0 is a single high-variance behavioral fork the author himself flags.
Independence: maintainer-run, single repo, single agent (Claude Code), self-published, with an explicit disclaimer inviting reproduction. No independent reproduction found in 35 searches [no source found]. Reception exists (r/ClaudeAI thread quoted in README, "280K+ views" [low-authority]; third-party directory write-ups e.g. https://ai-tldr.dev/tools/grepai [low-authority]) but none contains a reproduction.
Verdict: usable as a hypothesis generator, not as a ranking input or a fleet forecast. This matches the standing rule: no ranking based solely on it.
F.2 Measured resource numbers that DO exist [single-source = author docs, unless noted]

From the author's stores page (https://yoanbernabeu.github.io/grepai/backends/stores/, "Measured with 768-dimension vectors on a recent laptop"):

CHUNKS
	
INDEX.GOB SIZE
	
LOAD + SEARCH TIME

10,000	~60 MB	~0.1 s
50,000	~310 MB	~0.5 s
100,000	~625 MB	~0.8 s
"Memory follows the same curve: on the Go source tree (about 55,000 chunks, a 494 MB index.gob), each search peaks at about 1.2 GB of RAM." 1536/3072-dim models "multiply the size by 2 or 4". Warning printed above ~50k chunks recommending Qdrant. [single-source]
Idle: no resident process unless watch --background runs; one-shot CLI exits after the command. GPU/VRAM: zero with remote embeddings (this is the point of the remote path). [established, source behavior]
F.3 Independently reproducible measurements from issues (third-party, on-record)
#361 "IndexAll decide-phase HashedFiles retains full file bodies (RAM spike)" — grepai 1.0.0, Windows, GOB: full file contents retained for all hashed files during the decide/index window → "Multi-GB of retained strings on large repos", RSS "peaks far above final index size", competing with Ollama on the same machine. Open, no maintainer response visible (https://github.com/yoanbernabeu/grepai/issues/361).
#355 "Watch flush blocks forever when event queue is full under embed backpressure" — 1.0.0, fixed-size event channel (buffer 100), no timeout/drop; agent save storms + slow embeds → watcher stall, unbounded pending, possible missed events on Windows. Open (https://github.com/yoanbernabeu/grepai/issues/355).
#360 "GOB LookupByContentHash is O(n)" — per-chunk content-hash lookup linearly scans all in-memory chunks → O(chunks_in_file × store_size) CPU/RAM burn exactly in the caching feature meant to save work; Postgres/Qdrant are indexed, GOB is the hot path. Open (https://github.com/yoanbernabeu/grepai/issues/360).
All three filed 2026-10-25 by the same contributor (wwylon) against 1.0.0/Windows/GOB/Ollama; none closed as of v1.2.0 (2026-10-30). [established as open issues; severity on our hardware UNKNOWN]
Older datapoint: #192 (2026-03-19) status showed "Files indexed: 0" after successful scan — since apparently fixed, illustrates early-stage churn (https://github.com/yoanbernabeu/grepai/issues/192).
Repo currently shows 14 open issues total (issues page, 2026-11-02) — small backlog, fast-moving project.
F.4 Remote embeddings vs local footprint — the honest ledger

Remote embeddings remove: Ollama/LM Studio model RAM and the embedding CPU/GPU burn (the dominant compute cost of local operation, and the #355/#361 aggravator — both reporters ran slow local Ollama embeds). Remote embeddings do not remove: index.gob on disk (vectors + chunk text), the full-index RAM load per search (~1.2 GB at 55k chunks × dims multiplier), symbols.gob, and the per-search CPU of a full cosine scan (GOB/Postgres). With 1536-dim models the vector bytes double vs the 768-dim measurements above. Simplest on-demand architecture with no new daemon/router/guard: grepai index (one-shot) + grepai search/trace CLI (or session-scoped MCP only if the agent needs it), GOB store, no watcher, no Qdrant/Postgres, no skills installation into shared config. [established + inference, labeled]

Unmeasured fields (UNKNOWN by mandate): peak/idle RSS on our three machines; CPU-seconds per 100k tokens indexed with openai provider; network bytes per index (≈ chunk text + JSON overhead, never measured); disk write amplification during gob saves; cold vs warm wall time; Windows 11 behavior under v1.2.0; cloud cost under our real churn. Any pilot must fill these with local measurement.

G. Additive isolated canary protocol (preregistration draft)

Hard constraints honored: immutable snapshots; CodeIndexer baseline untouched (its owner supplies the common protocol; this document only proposes the grepai-additive layer); no shared config mutation; secrets excluded; all invocations counted; abort/rollback defined.

G.1 Snapshot. Pick 2 owned repos: one Python/TS-heavy, one JS/TS + PowerShell-heavy (to expose the PowerShell gap). Freeze at a tagged commit; record SHA-256 manifest of every file. Only extensions actually present in each repo are exercised. Secret scan before anything: run a standard scanner (e.g. trufflehog/gitleaks) over the snapshot; then author .grepaiignore + config ignore to exclude any secret-bearing paths by construction; re-scan the would-be-transmitted set (all files not ignored) and require zero findings before the first grepai init. Re-verify per run. Abort if scan non-zero.

G.2 Task set (per repo, RU and EN mirrored). ≥5 tasks × {known-symbol lookup, exact-identifier search, semantic/intent query ("where do we refresh expired tokens"), navigation ("who calls X"), no-answer trap (query about code that does not exist)}. Source truth (expected files/symbols/no-answer) sealed by the protocol owner before answerers see tasks.

G.3 Arms and order. Arm A: baseline (CodeIndexer workflow) only. Arm B: baseline + grepai (remote embeddings). Counterbalance across two independent answerers (A-first/B-first swapped); same snapshot; answerers sealed from source truth and from each other; a separate judge scores against sealed truth (accuracy, misses, false answers).

G.4 Accounting (all serialized delivered context counted, including wrappers/echoes). Capture full stdout/stderr of every grepai invocation and every agent-visible echo; count tokens of everything delivered into the agent context, per arm. Record: number and type of actual product invocations (frozen-run successful calls must be >0 or the run is INVALID — the failure mode of the October comparison), wall time, RSS peak via /usr/bin/time -v (macOS equivalent), disk bytes, network requests/bytes (proxy log), cloud token charges from provider dashboards. Runs at 1-shot/cold and warm; churn sub-run performs create/edit/rename/delete only on a copy of the snapshot.

G.5 Preregistered thresholds (set BEFORE measurement; they are proposals to be frozen by the baseline owner):

Proceed only if: successful grepai invocations ≥ 1 in every frozen run; no answer uses a hallucinated path (false-answer rate ≤ 1 task/arm); RU semantic task accuracy not worse than EN by >1 task; context-token delta (B vs A) ≤ 0 for ≥50% of tasks OR wall-time delta ≥ 20% faster on ≥50% of tasks (benefit must appear in at least one measured axis); peak additional RSS ≤ 2 GB; cloud cost per full pilot ≤ the existing allowance with margin ≥10×.
Abort/rollback: grepai watch --stop (if ever started), delete .grepai/ directory (removes index.gob/symbols.gob/config — complete rollback, no system state), remove any agent-setup-written blocks from sandbox copies only. No registry/service/PATH residue beyond the binary itself (%LOCALAPPDATA%\Programs\grepai on Windows — delete to roll back).

G.6 Second-OS readback. After the macOS pilot arm completes: copy the snapshot + .grepai/ config (not the GOB index — it is non-portable by design) to the second OS; run grepai index fresh from the same config and embedder identity (.grepai/index-meta.json pins provider/model/dims); confirm identical file coverage (grepai status) and that top-3 results for the sealed EN tasks match the first OS. This tests model-determinism and path handling cross-platform before any Windows involvement.

G.7 Functional vs comparative separation. Report (i) functional: did grepai work at all on our stack (install, init, index, search, trace, RU/EN) — pass/fail per task; (ii) comparative: deltas vs baseline on the same protocol. A functional failure (e.g., PowerShell repo) terminates the arm early and is recorded as such, not as a low comparative score.

H. Skeptical recommendation

Decision: TRY, narrowly — with a hard Windows PARK. Confidence tier: [emerging]. Main assumption it rests on: a zero-marginal-cost embedding key exists for us (ChatGPT-Pro-linked API credits or an existing Synthetic subscription or acceptance of a free OpenRouter route), because without it the budget gate cannot be passed.

Named options:

TRY-narrow (recommended): grepai v1.2.0 pinned, provider: openai (credits route) or provider: synthetic (if subscribed), GOB store, one-shot grepai index only (no watcher, no background daemon, no persistent MCP), CLI/skills access from Claude Code and Codex on the Apple Silicon Mac Studio and Intel Mac Pro, Python/JS/TS repos only, .grepaiignore authored from a secret scan, RU+EN tasks per G. Trade-offs: adds a second index to maintain manually (re-run grepai index after work sessions — no watcher means freshness is our discipline); zero new daemon surface; minimal local CPU/RAM beyond search-time index load; risk: benefit may not reproduce outside the author's benchmark.
PARK-Windows (concurrent, explicit): exclude the Windows Hub from the pilot. Basis: #361 multi-GB RAM spike and #355 watcher stall are open, Windows-reported, and unfixed in v1.2.0; PowerShell — a Hub-native language — is unsupported for both indexing-by-default and trace; Windows path handling has had one shipped normalization bug already (v1.0.0). Revisit when: #361 and #355 closed AND a .ps1 support decision exists (feature request to be filed only with operator authorization).
KEEP (status quo): stay CodeIndexer-only. Trade-offs: zero integration cost; forgoes the only tool in the challenger set that demonstrably ships local trace + remote-embedding search with permissive licensing; loses the cheap option value while the project iterates at 3-releases/week pace. Strongest argument for keeping: our own invalid October comparison shows how easily this category produces theater — without the canary, installing grepai is compliance, not knowledge.

Strongest single argument against TRY: the value proposition is unproven where it would matter to us — the 97% number is maintainer-run on one TS repo with no quality scoring, no independent reproduction exists, our Hub's primary shell language is unsupported, and trace is regex-mode in every official binary; therefore the expected benefit over a tuned ripgrep/CodeIndexer baseline on our codebases is genuinely unknown, while the costs (an index to keep fresh, a secret-exclusion configuration surface, open Windows bugs) are concrete. Steelman of the KEEP side: the October overview nominated grepai on external quorum, not validation; quorum is not evidence. Steelman of the TRY side (equally strong): the marginal cost of the G-protocol is one afternoon, the rollback is deleting one directory, and remote embeddings at $0.02/1M make the cloud-exposure experiment affordable — the only way to replace quorum with evidence is to run G.

Unresolved gaps (each blocks fleet claims):

Independent reproduction of the token/cost benefit on a non-author codebase — none exists [no source found].
RU-language search quality for every candidate embedding model inside grepai — unmeasured.
OpenRouter/Synthetic/Requesty retention and logging specifics for embeddings — undocumented/UNVERIFIED (OpenRouter docs are silent; Synthetic's no-store claim is third-party).
Windows v1.2.0 RAM/backpressure behavior — open issues, unfixed.
PowerShell indexing + trace — absent by design.
Embedder-side billing of partial batch failures after retry — UNKNOWN.
Multi-client shared-store safety — documented hazards, fine for solo pilot, unresolved for fleet sharing.

No fleet PASS may be inferred from install/ACK/exit-0. The exact next experiment is G as preregistered above, executed on one macOS machine, one Python/TS repo, credits-funded OpenAI embedder, with the PowerShell repo added as a functional-failure probe only.

Contradictions register (preserved, not smoothed)
"100% Local" (README headline) vs cloud reality. README: "Your code never leaves your machine"; same README: providers Ollama/LM Studio/or OpenAI; SECURITY.md: "When using OpenAI: code content is sent to OpenAI's API." Landing page softens to "By default … Nothing is sent anywhere." Both statements are true only in their scopes; the product's privacy claim is default-scoped, not absolute. Sources: https://github.com/yoanbernabeu/grepai, https://yoanbernabeu.github.io/grepai/, SECURITY.md.
Synthetic pricing: $30/mo (two 2026 reviews) vs $20/Standard + $60/Pro (undated third source). Unresolved; current price must be confirmed before any budget claim. Sources listed in B.3.
OpenAI embedding price page vs calculators: the official pricing page as fetched did not render embedding rows; $0.02/$0.13 come from three concordant third parties + OpenAI's 2024 announcement. Treated as established-with-caveat, dated.
97% headline vs its own table: −97% fresh input tokens coexists with +30% cache-read tokens; net −27.5%. The marketing number is technically true and economically misleading; the author's own limitations section concedes non-determinism.
"Precise mode" in docs navigation vs deprecation note: docs describe Fast/Precise extraction modes, then state precise was never wired into indexing and the flag is deprecated. The docs page carries both; the deprecation note governs.
Resource claims: author docs (~1.2 GB search peak) vs issue #361 (multi-GB spikes during indexing) — not contradictory (different phases), but neither number is validated on our hardware.
OpenRouter latency for embeddings: one HN commenter calls it significant/"not suitable" for agent loops (https://news.ycombinator.com/item?id <tg-id>) [single-source, low-authority]; OpenRouter docs claim smaller embedding models are "faster and cheaper" without numbers. Unmeasured either way for our query pattern.
Dictation identity "GroupEye Cloud": no such product exists in any searched source; the actual product is grepai. Not substituted silently — stated explicitly here.
Evidence table (key claims at a glance)
#
	
CLAIM
	
TIER
	
ORIGIN
	
SOURCE(S)

1	grepai v1.2.0, 2026-10-30, MIT, Go 1.25.5, no prereleases	[established]	official	releases page; clone go.mod; LICENSE
2	No official hosted service; local index + optional remote embedder	[established]	source behavior	factory.go; stores doc; landing
3	6 embedder providers incl. 4 cloud; env-key-to-official-host-only guard	[established]	source behavior	factory.go; env_key.go; embedders doc
4	OpenAI API: no training, ≤30-day abuse logs, ZDR incl. /v1/embeddings	[established]	official + 2 concordant secondary	your-data guide; humla (2026-07-28); meetily (2026-09-26)
5	ChatGPT Pro/Business include claimable API credits; Plus does not	[established]	official	help.openai.com article 20001626
6	3-small $0.02/1M; 3-large $0.13/1M; OpenRouter qwen3-8b $0.01/1M	[established]	third-party pricing pages, concordant	costgoat (Oct 2026); cloudzero; OpenRouter collections (Oct 2026)
7	GOB: full-index RAM load per search; ~1.2 GB @55k chunks; 60/310/625 MB @10/50/100k chunks (768-dim)	[established claim, single-source]	author measurement	stores doc
8	Open issues #361/#355/#360: Windows RAM spike, watcher stall, O(n) hash lookup	[established]	third-party issue reports, open	GitHub issues
9	Precise/tree-sitter mode: never wired into indexing; absent from release binaries	[established]	official statement + source/build check	trace doc; installation doc; Makefile; build tags
10	PowerShell: not indexed by default, no trace patterns	[established]	source behavior	scanner.go; patterns.go; watch-guide
11	.env indexed by default; no secret scanning	[established]	source behavior	scanner.go; indexer grep; watch-guide skip list
12	97% / −27.5% benchmark: maintainer-run, 5 runs, Excalidraw, no quality metric, no independent reproduction	[emerging]	single-origin measurement	benchmark blog (2026-01-21)
13	RU query quality inside grepai	[no source found]	—	gap → G protocol
14	One-shot grepai index equivalent to watcher's initial sync, then exits	[established]	official	watch-guide
15	GOB index not portable across machines; worktrees get independent copies	[established]	official	stores doc; worktrees doc
16	Rename/embedding-reuse bugs fixed in v1.2.0 (cross-path reuse; premature chunk deletion)	[established]	official changelog	releases page
17	Requesty supports embeddings (3-small/3-large), fee ~5%	[emerging]	official endpoint doc + single-source fee	docs.requesty.ai (2026-05-26); getmaxim (~2026-10-30)
18	Synthetic includes embeddings in subscriptions; rate-limit exemption	[emerging]	official inclusion + single-source exemption	dev.synthetic.new models page; ai-deck (Aug 2026)
19	OpenRouter free embedding route nemotron-3-embed-1b:free	[emerging, single-source]	vendor blog	best-embedding-models-2026 (2026-09-23)
20	nomic-embed-text-v2-moe ≈100 languages, 768 dims	[established]	vendor	ollama library page; HF card (2025-02-11)
Appendix: concrete commands and configuration (pilot-shaped, v1.2.0)

Install (pin; verify grepai version prints 1.2.0):

bash
# macOS (Homebrew)                    # Windows (later, only if unparked)
brew install yoanbernabeu/tap/grepai  # irm https://raw.githubusercontent.com/yoanbernabeu/grepai/main/install.ps1 | iex
grepai version

Remote-embedding init for the zero-credit route (ChatGPT-Pro-linked API credits), OpenAI 3-small:

bash
export <key-var>=sk-...          # funded via the ChatGPT Pro credit benefit; env key only goes to api.openai.com
cd /path/to/owned-repo
grepai init --provider openai         # then edit .grepai/config.yaml:
yaml
embedder:
  provider: openai
  model: text-embedding-3-small       # 1536 dims default; add dimensions: 768 only with a matching model change (forces reindex)
  parallelism: 4
  max_retries: 5
store:
  backend: gob                        # stays on-machine; NOT portable across machines
chunking:
  custom_extensions: [".ps1"]         # ONLY if PowerShell files must be text-indexed (no trace support)
search:
  hybrid:
    enabled: true                     # needed when queries carry exact identifiers
ignore:                               # name-matching only (not globs) — extend for secret-bearing dirs
  - .env
  - secrets

Secret exclusion (mandatory before first index with a cloud embedder): author .grepaiignore (gitignore syntax, globs supported, ! re-include) for *.env, *.pem, *.key, *credentials*, token caches; scan the would-be-transmitted set; re-scan every run.

On-demand usage (no watcher, no daemon, satisfies "no seven background watchers"):

bash
grepai index                          # one-shot: syncs vector+symbol indexes, exits
grepai search "where do we refresh expired tokens" --json --compact
grepai search "обновление токенов" --json --compact     # RU probe (quality unmeasured — see C.3)
grepai trace callers "handleUserSession"                # local, zero network, regex 'fast' mode
grepai status                         # chunk count / index size before deciding on Qdrant

Session-scoped MCP (only if the agent needs tool access; dies with the client session):

json
{ "mcpServers": { "grepai": { "command": "grepai", "args": ["mcp-serve"] } } }

Abort/rollback (complete; no system state beyond the binary):

bash
grepai watch --stop                   # only if a watcher was ever started
rm -rf /path/to/repo/.grepai          # removes index.gob, symbols.gob, config.yaml — full rollback
# remove any agent-setup-written blocks from sandbox copies; delete the binary to uninstall

Version pin and re-verification after any grepai update: record grepai version and the .grepai/index-meta.json contents (provider/model/dims) in the pilot log; changing provider/model/dimensions forces a full re-embed by design.

Complete source list (all URLs actually accessed/fetched during this research, with dates)

Primary — project (fetched/cloned 2026-11-02 unless noted):

https://github.com/yoanbernabeu/grepai — repository, README, license, topics
https://github.com/yoanbernabeu/grepai/releases — release notes v1.2.0/v1.1.0/v1.0.0
https://yoanbernabeu.github.io/grepai/ — official docs landing
https://yoanbernabeu.github.io/grepai/installation
https://yoanbernabeu.github.io/grepai/backends/embedders/
https://yoanbernabeu.github.io/grepai/backends/stores/
https://yoanbernabeu.github.io/grepai/trace
https://yoanbernabeu.github.io/grepai/watch-guide
https://yoanbernabeu.github.io/grepai/git-worktrees/
https://yoanbernabeu.github.io/grepai/search-guide
https://yoanbernabeu.github.io/grepai/hybrid-search/
https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code (pubDate 2026-01-21)
https://github.com/yoanbernabeu/grepai/blob/main/go.mod (HEAD 9cf58bf, 2026-10-30)
https://github.com/yoanbernabeu/grepai/blob/main/embedder/factory.go
https://github.com/yoanbernabeu/grepai/blob/main/embedder/env_key.go
https://github.com/yoanbernabeu/grepai/blob/main/embedder/openai.go
https://github.com/yoanbernabeu/grepai/blob/main/config/config.go
https://github.com/yoanbernabeu/grepai/blob/main/config/index_meta.go
https://github.com/yoanbernabeu/grepai/blob/main/indexer/scanner.go
https://github.com/yoanbernabeu/grepai/blob/main/indexer/indexer.go
https://github.com/yoanbernabeu/grepai/blob/main/indexer/chunker_ast.go (build tag)
https://github.com/yoanbernabeu/grepai/blob/main/trace/extractor.go
https://github.com/yoanbernabeu/grepai/blob/main/trace/patterns.go
https://github.com/yoanbernabeu/grepai/blob/main/trace/resolve.go
https://github.com/yoanbernabeu/grepai/blob/main/trace/extractor_ts.go (build tag)
https://github.com/yoanbernabeu/grepai/blob/main/watcher/events.go
https://github.com/yoanbernabeu/grepai/blob/main/daemon/daemon.go
https://github.com/yoanbernabeu/grepai/blob/main/mcp/server.go
https://github.com/yoanbernabeu/grepai/blob/main/cli/mcp_serve.go
https://github.com/yoanbernabeu/grepai/blob/main/cli/agent_setup.go
https://github.com/yoanbernabeu/grepai/blob/main/cli/search.go
https://github.com/yoanbernabeu/grepai/blob/main/Makefile
https://github.com/yoanbernabeu/grepai/blob/main/SECURITY.md
https://github.com/yoanbernabeu/grepai/issues/361 (RAM spike, open)
https://github.com/yoanbernabeu/grepai/issues/355 (watch flush stall, open, 2026-10-25)
https://github.com/yoanbernabeu/grepai/issues/360 (GOB O(n) reuse, open, 2026-10-25)
https://github.com/yoanbernabeu/grepai/issues/349 (freeze/unfreeze feature request)
https://github.com/yoanbernabeu/grepai/issues/192 (status bug, 2026-03-19)
https://github.com/yoanbernabeu/grepai-skills (27 agent skills, MIT)

Provider / platform (fetched 2026-11-02 unless noted):
40. https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions (updated ~2026-11)
41. https://developers.openai.com/api/docs/guides/your-data (fetched via concordant summaries; core claims dated 2026-07-28/2026-09-26 below)
42. https://humla.team/blog/openai-data-retention-policy (2026-07-28) [secondary]
43. https://meetily.ai/llm-privacy/openai (checked 2026-09-26 per page) [secondary]
44. https://openai.com/index/new-embedding-models-and-api-updates (2024-01-25) — price origin for 3-small
45. https://costgoat.com/pricing/openai-embeddings (Oct 2026) [low-authority]
46. https://www.cloudzero.com/blog/openai-pricing (2026) [secondary]
47. https://zilliz.com/ai-models/text-embedding-3-small [secondary]
48. https://openrouter.ai/docs/api_reference/embeddings
49. https://openrouter.ai/collections/embedding-models (rankings updated Oct 2026)
50. https://openrouter.ai/blog/insights/best-embedding-models-2026 (2026-09-23/24)
51. https://dev.synthetic.new/docs/api/models
52. https://dev.synthetic.new (API overview)
53. https://ai-deck.app/blog/synthetic (Aug 2026) [single-source]
54. https://aimarketing.ghost.io/synthetic-new-review-claude-code-drop-in [low-authority]
55. https://tomrochette.com/agents/model-access/synthetic [single-source, undated]
56. https://docs.requesty.ai/api-reference/endpoint/embeddings-create (2026-05-26)
57. https://docs.requesty.ai/quickstart (2026-10-12)
58. https://www.getmaxim.ai/articles/requesty-alternatives (~2026-10-30) [single-source]
59. https://qdrant.tech/documentation/cloud/create-cluster
60. https://qdrant.tech/pricing
61. https://ollama.com/library/nomic-embed-text-v2-moe
62. https://huggingface.co/nomic-ai/nomic-embed-text-v2-moe (2025-02-11)
63. https://static.nomic.ai/nomic_embed_multilingual_preprint.pdf (2025)
64. https://sdtimes.com/ai/openai-announces-new-models-and-api-updates (2024-01-25)
65. https://support.claude.com/en/articles/10366389-how-can-i-get-higher-rate-limits-on-the-claude-api (official; credits are Anthropic-API-only)
66. https://www.zeniteq.com/anthropic-adds-monthly-api-credits-to-max-and-team-rzc3x2 (2026-10-08) [secondary]

Third-party coverage / reception (low-authority, used only as reception evidence):
67. https://www.reddit.com/r/ClaudeAI/comments/1qiv0d3/open_source_i_reduced_claude_code_input_tokens_by/ (found; page itself not readable from this environment — disclosed)
68. https://ai-tldr.dev/tools/grepai
69. https://daily.dev/posts/grepai-is-a-privacy-first-semantic-code-search-tool-that-indexes-the-meaning-of-your-code-using-vect-lm92wcsxl (2026-01-14)
70. https://news.ycombinator.com/item?id <tg-id> (OpenRouter embedding-latency comment) [single-source]
71. https://github.com/anomalyco/opencode/issues/13866 (2026-02-16, third-party grepai usage in OpenCode)

Explicitly not found: any independent reproduction of the 97%/−27.5% benchmark; any RU-language grepai measurement; any "GroupEye Cloud" product; any official hosted grepai service. [no source found — recorded as gaps, not guesses]