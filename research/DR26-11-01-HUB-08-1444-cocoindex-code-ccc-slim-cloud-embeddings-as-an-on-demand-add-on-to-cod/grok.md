# DR26-11-01-HUB-08-1444 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Source: https://приватный чат?rid=8249f602-870b-4d99-b96d-525884154074
Mode: Expert + deep-research; own browser report; captured 2026-11-01T18:56Z. Technical export, not substantive acceptance.


## Grok




DR-ID: DR26-11-01-HUB-08-1444
Research date: 2026-11-01
Core question: Can on-demand additive CocoIndex Code (slim/base package + cloud embeddings via LiteLLM) be used alongside existing CodeIndexer on Windows Hub / Intel Mac Pro / Apple Silicon Mac Studio to reduce total local RAM/CPU/GPU while preserving correctness and freshness, under the constraints of no new local embedding models, no permanent per-session stdio MCP or background watchers, ordinary nonsecret code only to cloud, and existing credits first?
Scope: Official primary sources only (GitHub cocoindex-io/cocoindex-code, PyPI, Docker Hub, cocoindex.io docs/README/EMBEDDINGS.md, issues). No private connected sources. October broad DR and invalid 40-answer v2 comparison are treated as historical and not reused.
Overall confidence: Medium on identity/dependencies/pipeline/lifecycle (primary source verified); Low on resource-saving and correctness benefits (no measured data meeting the preregistered gates; marketing claims unverified).

## Executive Summary

Exact product is cocoindex-io/cocoindex-code (CLI ccc), latest released v0.2.42 (2026-10-05, commit 6bb17d0, Apache-2.0, Python ≥3.11). Slim is the official base package (no extra); [full] is the optional local-embeddings extra. Distinct from the cocoindex framework and from Code Plus.[⁠GitHub](https://github.com/cocoindex-io/cocoindex-code/releases)

Base/slim depends on cocoindex[litellm] + sqlite-vec + numpy etc.; full adds sentence-transformers. Docker slim compressed ~204 MB; full ~3.25 GB. Marketing “~450 MB / ~5 GB / zero-local-resource” is approximate marketing, not measured RSS/inference.[⁠Hub.docker](https://hub.docker.com/r/cocoindex/cocoindex-code/tags)

Pipeline uses Tree-sitter AST chunking (defaults chunk_size=1000, min=250, overlap=150) for supported languages (Python/JS/TS yes; PowerShell not listed, falls back to text/line). Semantic search via embeddings + sqlite-vec; ccc grep structural search is released and index-free. AST chunks do not imply callers/references/flow.[⁠Raw.githubusercontent](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/src/cocoindex_code/indexer.py)

Lifecycle is per-project local SQLite under .cocoindex_code/, daemon auto-starts with default 180 min idle + true MCP keepalive. Incremental via CocoIndex memoization. Windows path/encoding issues exist.[⁠GitHub](https://github.com/cocoindex-io/cocoindex-code)

Tool itself is free Apache-2.0. Embedding cost is pure provider API (tokens for initial + deltas + queries + retries). Unknown credit eligibility is UNKNOWN; chat subscription does not imply embeddings credit. No invented costs.[⁠Cocoindex](https://cocoindex.io/cocoindex-code/)

Preregistered trial gates are not met by any public evidence. Resource/cost UNKNOWN blocks any resource-saving claim. Decision: PARK for fleet; optional isolated canary only with confirmed existing credits and full trial protocol. CodeIndexer alone is cheaper unless measured benefit is proven.


## A. Exact Product Identity
Finding: Official identity is the GitHub repository cocoindex-io/cocoindex-code (Python package cocoindex-code, CLI entry point ccc). Latest released version is v0.2.42 (released 2026-10-05 by georgeh0, commit 6bb17d0, Apache-2.0 license, requires-python ≥3.11). It is a thin application/CLI/MCP/skill wrapper on top of the cocoindex framework (Rust engine, separate repo cocoindex-io/cocoindex). It is not Code Plus (self-hosted indexer+query server with ccx CLI/MCP, documented in cocoindex-code-plus-pub). Namesakes (e.g. third-party setup packs) are unrelated.[⁠GitHub](https://github.com/cocoindex-io/cocoindex-code/releases)
Slim: Official base package is the slim variant. pip install cocoindex-code (or Docker :latest / v0.2.42) is LiteLLM-only and requires a cloud embedding provider + API key. The [full] extra (or Docker :full) pulls cocoindex[sentence-transformers] for local embeddings. Never invent “cocoindex-code-slim”; the README explicitly calls the no-extra package “slim”. Distinguish released tags from main/unreleased features.[⁠Raw.githubusercontent](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/README.md)
Disconfirming / uncertainty: Operator observation of 0.2.42 / v0.2.42 Oct 5 Apache 2.0 Python ≥3.11 is independently verified. No evidence of a separate slim package name.

## B. Dependency Graph
Base (slim) direct deps (from pyproject.toml): mcp, cocoindex[litellm] (≥1.0.17,<1.1), sqlite-vec, pydantic, numpy, einops, typer, msgspec, pathspec, pyyaml, questionary. Transitive includes the CocoIndex Rust engine (via the Python package), LiteLLM for cloud embeddings, and SQLite/sqlite-vec for the local vector store. No local embedding model by default.[⁠Raw.githubusercontent](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/pyproject.toml)
Full / embeddings-local: Adds cocoindex[sentence-transformers]. This pulls torch + transformers (the source of the ~1 GB local dependency claim).
Cloud path still incurs: Tokenizer/model download on the provider side (not local), network for embedding API calls, local cache of the SQLite index and any LiteLLM metadata. No local model weights or GPU inference for embeddings. Wheel/installed size of the Python package itself is small (tens of MB); Docker slim compressed size is ~204 MB (amd64/arm64); full is ~3.25–3.38 GB. Marketing “~450 MB / ~5 GB / zero-local-resource” is not a measured RSS, private bytes, or inference figure—do not treat as measurement. Local CPU/RAM still used for scanning, chunking, SQLite, and the daemon process.[⁠Hub.docker](https://hub.docker.com/r/cocoindex/cocoindex-code/tags)
Disconfirming: Full extra is the only path that loads local models. Slim avoids torch/transformers but still has a resident daemon and SQLite.

## C. Pipeline
Scanning respects include/exclude patterns, .gitignore-style rules (pathspec), max_file_size, and skips binaries/empty/undecodable files. Symlinks and size limits are configurable via project settings. Languages with explicit Tree-sitter support include Python (.py), JavaScript (.js etc.), TypeScript (.ts/.tsx). PowerShell (.ps1) is not in the published language table; it falls back to text/line-based chunking via RecursiveSplitter. Unknown extensions also fall back.[⁠Raw.githubusercontent](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/README.md)
Chunk defaults (hard-coded in indexer.py): chunk_size=1000, min_chunk_size=250, chunk_overlap=150. Structural ccc grep is released and does not require an index or embeddings. AST chunk boundaries produce coherent units (functions/classes) but do not imply callers, references, or control-flow analysis. Semantic search is embedding similarity (sqlite-vec KNN, optional language partition, path filter may full-scan). Known-symbol / exact / navigation / no-answer behavior is limited to what the embedding model + chunking provide; no built-in symbol index or call graph. RU/EN quality depends entirely on the chosen embedding model (multilingual options exist but are not default).[⁠Raw.githubusercontent](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/src/cocoindex_code/indexer.py)
Disconfirming: Marketing “AST-based” and “70% token saving” are not measured against CodeIndexer or a controlled baseline. Windows path-filter bugs can return no results.

## D. Lifecycle
Index is local SQLite (target_sqlite.db + cocoindex.db) stored by default under <project>/.cocoindex_code/. Path representation is relative; COCOINDEX_CODE_DB_PATH_MAPPING and COCOINDEX_CODE_DIR allow isolation and remapping (useful for Docker or cross-OS). Duplicate checkouts and cross-OS root maps require explicit mapping; never equate Windows and Unix roots blindly. Live DB sync risk is low if isolated; concurrent projects are supported but GPU (when used) is serialized.[⁠GitHub](https://github.com/cocoindex-io/cocoindex-code/blob/main/README.md)
Daemon auto-starts on first use, uses IPC (Unix socket style; Windows compatibility has known issues). Default idle_timeout_minutes=180, keep_alive_with_mcp=true (MCP heartbeats prevent idle exit). Refresh is via ccc index (incremental) or search with refresh flag; no permanent background watcher is required if MCP is not long-lived. Create/edit/rename/delete are handled incrementally via CocoIndex memoization and target-state reconciliation; interrupted updates can leave partial state (restart + reindex recovers). Model/dimension changes require ccc reset && ccc index. Stale hits are possible until refresh completes.[⁠GitHub](https://github.com/cocoindex-io/cocoindex-code)
Disconfirming / issues: Open issues include Windows path-glob returning no results, daemon crashes on certain --path searches, and stale daemon embedding environment. macOS SQLite extension loading requires Homebrew Python.

## E. Small Mature Documented Config
Pinned isolated native base install (pipx install cocoindex-code or equivalent, no [full]), remote embedder via LiteLLM, isolated COCOINDEX_CODE_DIR and project COPY (no home Docker mount, no global plugin/hooks/autostart). Exact minimal global_settings.yml:
YAML


```
embedding:
  provider: litellm
  model: openai/text-embedding-3-small   # or other LiteLLM-compatible
  min_interval_ms: 300
daemon:
  idle_timeout_minutes: 180
  keep_alive_with_mcp: false             # prefer false for on-demand
envs:
  <key-var>: ...                    # never commit
```




Then ccc init (project settings), ccc index, ccc search ..., ccc daemon stop when done. Established low-cost multilingual/code providers compatible with LiteLLM exist (OpenAI text-embedding-3-small, Gemini, Voyage code models, Cohere multilingual). Dimensions/token limits/rates/credit bounds/overage/billing observability are provider-specific (UNKNOWN for any particular existing credit). No hidden generation/rerank in the base path. API key is not paid permission; unknown credit eligibility is UNKNOWN.[⁠Raw.githubusercontent](https://raw.githubusercontent.com/cocoindex-io/cocoindex-code/main/EMBEDDINGS.md)

## F. Current Official Pricing and Terms
CocoIndex Code itself is Apache-2.0, free forever for individuals and teams. No product-level free monthly, trial, or expiry for the tool. All monetary cost is the embedding provider’s API (initial index tokens + changed chunks + queries + retries/rebuilds + any client-side context). Data transmission: code chunks are sent to the provider for embedding; retention/training depends on the provider’s terms (not CocoIndex’s). CocoIndex telemetry is anonymous and does not include source code. No invented actual cost. CodeIndexer alone is cheaper whenever embedding credits are insufficient or zero, or when measured benefit does not exceed the cost.[⁠Cocoindex](https://cocoindex.io/cocoindex-code/)
Inline primary: [https://github.com/cocoindex-io/cocoindex-code](https://github.com/cocoindex-io/cocoindex-code) (license), [https://cocoindex.io/cocoindex-code/](https://cocoindex.io/cocoindex-code/) (FAQ: Apache 2.0 free), [https://cocoindex.io/privacy-policy/](https://cocoindex.io/privacy-policy/) (telemetry and no pipeline data).

## G. Critique of Preregistered Full-Scenario Trial
The proposed protocol (immutable allowlisted snapshot with hashes, logical repo_id/revision, NFC relative paths + explicit Windows/Mac root maps, 24 tasks = 4 each of known-symbol/exact/semantic-RU/semantic-EN/navigation/no-answer, hidden oracle, blinded independent judge, same model/harness/budgets, baseline CodeIndexer + source-read fallback vs baseline+ccc, seeded AB/BA, 3 paired repetitions, strict byte accounting after final serialization, 3 cold/3 warm, 60 s idle, full resource/network/provider-billed measurement, COPY-only mutations, independent client consumption, hard gates including ≥90 % coverage and no stale delete at 120 s, KEEP only on the stated correctness/time thresholds, resource UNKNOWN blocks resource-saving claim) is rigorous and correctly pre-registered. No public evidence satisfies these gates. Zero-success or incomplete-context trials cannot prove benefit. Operational thresholds are not statistical proof. Expand to other OS only after measured Hub benefit + independent review.
Missing evidence: Any controlled measurement of wall-time, CPU, RSS, or correctness delta versus CodeIndexer under the exact gates.

## H. Adversarial KEEP / TRY / PARK
Strongest FOR: Official slim + LiteLLM path exists, incremental AST chunking is real, daemon can be short-lived, tool is free, and ordinary nonsecret code can be embedded in cloud. Potential to reduce local embedding load if credits exist and benefit is proven.
Strongest AGAINST: No measured resource or correctness data meeting the gates. Marketing claims (1 GB / 450 MB / 5 GB / zero-local / 70 % tokens) are unverified. Windows path and daemon issues exist. PowerShell support is fallback-only. Resource/cost is UNKNOWN. Adding any daemon/MCP increases surface area. Chat subscription does not cover embeddings.
Evidence matrix (release-pinned):

Identity/version/license/slim: High (GitHub releases + README + pyproject.toml).

Deps/Docker sizes: High (pyproject + Docker Hub).

Chunk sizes / languages / fallback: High (indexer.py + README).

Daemon/idle/keepalive: High (settings.py + README).

Pricing of tool: High (Apache-2.0 + FAQ).

Resource-saving vs CodeIndexer: None (UNKNOWN).

Correctness delta: None.

Missing evidence: Controlled trial results, provider-billed receipts under the scenario, RSS/CPU under idle/warm on the three hosts, PowerShell-specific quality.
Disconfirming tests: Windows path-filter returns empty; daemon crash on certain searches; any trial with zero successful ccc retrievals; any resource measurement that treats marketing numbers as data.
Minimal canary and cost ceiling: Isolated COPY of one nonsecret repo, base package only, confirmed existing embedding credits only, manual ccc index / ccc search (no permanent MCP/hooks/autostart), stop owned daemon on completion, leave production CodeIndexer untouched. Ceiling = existing confirmed credits; unknown eligibility remains UNKNOWN.
Rollback: Stop only the owned isolated daemon / disconnect temporary client; leave production config and evidence intact. No new framework/guard. Install/ACK/rc0/snippet is not fleet PASS.
Final decision under the brief: PARK for production/fleet use. Optional isolated canary only after confirming existing credits and executing the full preregistered trial. CodeIndexer remains the default. Resource-saving claim is blocked until measured.






