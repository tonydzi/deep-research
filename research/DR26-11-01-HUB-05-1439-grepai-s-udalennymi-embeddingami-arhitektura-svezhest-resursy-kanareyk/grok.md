# DR26-11-01-HUB-05-1439 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Grok
Worked for 41s

# grepai (yoanbernabeu/grepai v1.2.0) — Identity-Pinned Primary-Source Report

Research date: 2026-11-01

Identity pin verified: yes

Latest release source (live): https://api.github.com/repos/yoanbernabeu/grepai/releases/latest returns tag_name: v1.2.0, published_at: 2026-10-30T09:51:53Z, html_url: https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0.

Pinned tag source: https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0 shows commit SHA d86397d (d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef). No discrepancy between live latest and the frozen pin.

License: MIT, Copyright (c) 2026 Yoan Bernabeu (https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/LICENSE).

Go: 1.25.5 (https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/go.mod). Dependencies include pgvector-go, qdrant/go-client, go-tree-sitter.

This is a standalone replacement evaluation against the original A–H brief and operating constraints. Prior report is ignored. No trial has been run; no measured benefit is claimed. Ordinary owned code only after owner privacy/corpus acceptance; existing explicitly verified subscription/API allowances first; no new accounts, cards, paid plans, top-ups, cap changes or inferred credits. No local neural startup, permanent watcher/MCP/global config. Shared contract owner freezes the 24-task comparison, budgets, independent actors and exact full-input accounting.

A. Identity and architecture

Identity. Owning implementation is the Go CLI at https://github.com/yoanbernabeu/grepai. Stable tag v1.2.0 (published 2026-10-30, SHA d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef) is confirmed by both the GitHub Releases page and the live /releases/latest API. MIT license. [established]

Architecture. Local chunking + embedding (local or remote provider) + store. Original code stays on disk. Chunks, vectors and metadata live in the configured store. Default store is GOB (.grepai/index.gob). Optional stores: PostgreSQL + pgvector or Qdrant. Symbol/call-graph data lives in local .grepai/symbols.gob regardless of vector backend. Queries are embedded and searched against the store by the local process. There is no official vendor-hosted indexing or search service. Remote store (user-operated Postgres or Qdrant) does not imply remote search for GOB; Qdrant executes search server-side. [established; https://yoanbernabeu.github.io/grepai/backends/stores/, indexer.go, config.go]

GOB performs local full scan (recommended ≤ ~50 k chunks; ~1.2 GB RAM peak cited in docs). Postgres ranks all chunks by cosine. Qdrant supports multi-user/workspace via namespace. [https://yoanbernabeu.github.io/grepai/backends/stores/]

B. Cloud path

Supported providers (v1.2.0 source). ollama, lmstudio, openai, synthetic, openrouter, requesty (config.go, embedders docs). Defaults: openai / openrouter / requesty → text-embedding-3-small (1536 dims, official endpoints); synthetic → hf:nomic-ai/nomic-embed-text-v1.5 (768). [https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/config/config.go, https://yoanbernabeu.github.io/grepai/backends/embedders/]

Implementation. OpenAI: POST {endpoint}/embeddings with JSON {model, input: []string, dimensions?}; batching, adaptive rate limiter, retries on 429/5xx, optional TPM pacing (openai.go). OpenRouter: POST https://openrouter.ai/api/v1/embeddings, falls back to OPENROUTER_API_KEY then <key-var> only on official endpoint, adds HTTP-Referer/X-Title (openrouter.go). Environment keys are sent only to official HTTPS endpoints (env_key.go); custom endpoints require explicit api_key in config. Config file permissions 0600. [established]

Data transmitted. Code chunks (and queries) are sent to the configured endpoint. Vectors/metadata remain in the chosen store. No retention or training policy is implemented in grepai source. [openai.go, openrouter.go]

OpenRouter ZDR. Zero Data Retention applies to inference requests for participating providers (no retention of prompts/responses after processing except CSAM screening or user choice). No explicit statement that embeddings are exempt; treated as inference. Planned CSAM screening update noted 2026-10-27. [https://openrouter.ai/docs/guides/features/zdr]

Pricing (OpenAI primary). text-embedding-3-small: $0.02 / 1M tokens (standard), $0.01 / 1M (Batch). text-embedding-3-large: $0.13 / $0.065. [https://developers.openai.com/api/docs/models/text-embedding-3-small, https://platform.openai.com/docs/pricing]

Estimates (not measured usage; label as such).

100 k code tokens ≈ $0.002 (small standard)
1 M code tokens ≈ $0.02
1 k queries (assume short) negligible relative to initial index
Incremental churn + retries: multiply by re-embed fraction × retry factor

Zero-new-spend pilot route. Possible only if an existing, explicitly verified OpenAI/OpenRouter/Requesty allowance already covers the expected token volume. Chat subscriptions are not assumed to include embedding API credits. No new accounts, cards, top-ups or inferred free credits are authorized under the operating constraints. Budget gate: confirm balance and data-sharing eligibility before any cloud embedder use. [constraint-driven; pricing from OpenAI]

C. Search and languages

Search. Semantic vector search is core. Hybrid (vector + keyword substring, RRF with k=60) is optional and disabled by default; loads all chunks for text search (not recommended for large indexes). Path filters, limit, JSON/compact/toon, file-level dedup exist. No explicit no-answer threshold; empty results if nothing ranks. Workspace searches ignore hybrid. [https://yoanbernabeu.github.io/grepai/hybrid-search/]

Languages. Indexing is text-chunk based on supported extensions (scanner.go: size ≤1 MB, non-binary, UTF-8). Trace/symbol extraction (regex) supports Go, JS/TS/JSX/TSX, Vue, Python, PHP, Lua, Java, C/C++, Rust, Zig, C#, F#, Pascal, Ruby, GDScript. PowerShell is not listed. Default models are English-centric; multilingual quality is model-dependent and unmeasured here. [https://yoanbernabeu.github.io/grepai/trace/, scanner.go]

Agent integration via CLI or MCP. One-shot (grepai index) and short-lived invocation are supported. Index sizes/RAM loading depend on backend (GOB RAM-bound). Claims of token savings require local measurement. [watch-guide]

D. Call graph vs LSP

Modes. Fast (regex) is the only mode used by grepai watch and grepai index. Precise (tree-sitter) exists in source behind treesitter build tag + CGO for a subset of languages but “was never wired into indexing.” Release binaries omit it. --mode is deprecated and always reports "mode": "fast". [https://yoanbernabeu.github.io/grepai/trace/]

Limitations. Regex cannot reliably resolve ambiguous same-name symbols, imports/aliases, overloads, class methods, dispatch/inheritance, callbacks, decorators or dynamic Python/JS/TS. Ruby bare calls without parentheses are explicitly undetected. Precise mode does not participate in indexing. Callers/callees display is syntactic approximation, not LSP-grade semantic resolution. PowerShell unsupported. [established; trace docs]

Fair fixture protocol (not executed). Sealed source-truth fixtures with known ambiguous symbols and dynamic dispatch; compare regex-trace output against language-server references and manual source truth; count false callers/callees. Do not equate syntax precision with semantic correctness.

E. Freshness

grepai index performs one-shot scan + synchronization then exits (no watcher). grepai watch adds continuous monitoring. Unchanged chunks reuse embeddings via content-hash cache (scoped; recent fix stops cross-path reuse). Rename/move re-embeds; delete removes chunks and document. Windows paths normalized to forward slash. Ignore via .gitignore, .grepaiignore (with negation), config ignore, external gitignore; no automatic secret detection (.env is a supported extension if not ignored). Persistence: periodic saves, writer lock; GOB on unclean shutdown has had corruption reports in issues (not re-verified here). Worktrees supported with per-worktree indexes. Index is not freely portable across machines/OS without reindex in many cases (path normalization, model metadata in .grepai/index-meta.json). [https://yoanbernabeu.github.io/grepai/watch-guide/, indexer.go, gitignore.go, scanner.go]

F. Resource and cost evidence

No independent peak/idle process RAM, CPU seconds, GPU/VRAM, disk I/O, network bytes or cloud token measurements are present in the retrieved primary sources. Docs note GOB ~1.2 GB peak at ~50 k chunks and search latency degradation. Remote embeddings still leave local (or user-operated remote) storage and search RAM/CPU. Author token-saving claims exist outside the listed primary sources and are not used here. Every unmeasured field is UNKNOWN. [stores docs; constraint: no trial has run]

Simplest on-demand architecture matching constraints: binary + one-shot grepai index (cloud embedder using verified existing allowance) + CLI --json or short-lived MCP only when needed. No permanent watcher, no Ollama/LM Studio, no shared config mutation.

G. Additive isolated canary (protocol, not executed)
Immutable snapshots of representative owned Python/JS/TS (and any PowerShell present) with hash manifest + secret scan. Exclude secrets/cookies/keys/confidential third-party data via .grepaiignore + config. Ordinary owned code only after owner privacy/corpus acceptance.
CodeIndexer baseline remains untouched; owner supplies common protocol. Do not mutate baseline.
Tasks: RU/EN known-symbol, exact, semantic, navigation, no-answer. Source truth sealed from independent answerers.
Counterbalanced order, independent answerers + judge. Count all serialized delivered context (wrappers/echoes included). Record actual invocations, accuracy/misses/false answers, wall time, resources, cloud cost, 1-shot/cold/warm.
Create/edit/rename/delete only on copies.
Preregister thresholds before measurements (accuracy delta, cost ceiling, abort on secret leak or unexpected spend). Shared contract owner freezes the 24-task comparison, budgets, independent actors and exact full-input accounting.
Abort/rollback: delete .grepai/ and any generated index; stop only own processes; preserve evidence. Second-OS readback of unchanged snapshots.
Separate functional “does it return results” from comparative benefit vs CodeIndexer. No claim of measured benefit because no trial has run.
H. Skeptical recommendation

TRY (narrow, on-demand, remote-embeddings canary) — Medium confidence.

Strongest argument against: Residual local (or user-operated) index RAM/disk; cloud egress of ordinary owned code (even if secrets excluded); regex-only call graphs that do not match LSP semantic resolution; no PowerShell support; multi-OS path/index portability friction; and the requirement that embedding spend must be covered by already-verified allowances with no new cards or inferred credits.

Unresolved gaps: Independent quality and resource measurements; operational status of tree-sitter precise mode (absent from releases); RU-query quality with English-centric models; exact credit eligibility under the “existing verified allowances only” rule; long-term index portability across Windows/macOS.

Named consumer: On-demand grepai index + grepai search --json --compact (or short-lived MCP) invoked by agents only for semantic “intent” queries where CodeIndexer is known to be weak, on secret-scanned owned code only, using an already-verified embedding allowance.

Exact next experiment: Single-repo, single-OS, one-shot index with an already-verified OpenAI/OpenRouter key, 24 sealed RU/EN tasks under the shared contract, full context accounting, abort if any unexpected API charge or secret appears in logs. No fleet rollout from install/ACK/exit 0.

Source Catalog (all retrieved 2026-11-01)
https://api.github.com/repos/yoanbernabeu/grepai/releases/latest (v1.2.0, 2026-10-30)
https://github.com/yoanbernabeu/grepai/releases/tag/v1.2.0 (SHA d86397d5f0c94ea31b96d1bb8b4da6966e26f5ef)
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/LICENSE (MIT, 2026)
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/go.mod (Go 1.25.5)
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/config/config.go
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/indexer/indexer.go
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/indexer/gitignore.go
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/indexer/scanner.go
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/embedder/openai.go
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/embedder/openrouter.go
https://raw.githubusercontent.com/yoanbernabeu/grepai/v1.2.0/embedder/env_key.go
https://yoanbernabeu.github.io/grepai/backends/stores/
https://yoanbernabeu.github.io/grepai/backends/embedders/
https://yoanbernabeu.github.io/grepai/hybrid-search/
https://yoanbernabeu.github.io/grepai/trace/
https://yoanbernabeu.github.io/grepai/watch-guide/
https://openrouter.ai/docs/guides/features/zdr
https://developers.openai.com/api/docs/models/text-embedding-3-small (pricing $0.02 / 1M)
https://platform.openai.com/docs/pricing (embeddings table)

Decision-oriented ending

Options:

TRY (recommended) — One-shot remote-embeddings canary on secret-scanned owned code using only already-verified allowances. Trade-off: residual local index resources and cloud egress of ordinary code; gain: measured semantic retrieval delta vs CodeIndexer under the shared contract.
PARK — Defer until independent quality/resource data or confirmed zero-new-spend allowance. Trade-off: no incremental capability.

Single recommendation: TRY (Medium confidence). Main assumption: an existing, explicitly verified embedding allowance covers the pilot without new cards, top-ups or inferred credits, and CodeIndexer remains the unmutated baseline.

10 sources