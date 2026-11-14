---
dr_id: DR26-11-01-HUB-08-1444
title: "CocoIndex Code (ccc) slim/cloud embeddings as an on-demand add-on to CodeIndexer"
date: 2026-11-01
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-01-HUB-08-1444): CocoIndex Code (ccc) slim/cloud embeddings as an on-demand add-on to CodeIndexer

> Can CocoIndex Code 0.2.42 in slim (base) mode with cloud embeddings be added on demand next to the existing CodeIndexer on Windows Hub, Intel Mac Pro and Apple Silicon Mac Studio to cut total local RAM/CPU/GPU without losing correctness or freshness, and under what trial and cost conditions.

## Ключевые выводы
- Identity verified by all vendors: cocoindex-io/cocoindex-code, CLI `ccc`, latest release v0.2.42 (2026-10-05, tag commit 6bb17d0, Apache-2.0, Python >=3.11, PyPI status Alpha). The wheel is pure Python (py3-none-any, ~90-92 kB). It is distinct from the CocoIndex framework (Rust engine) and from the proprietary self-hosted CocoIndex Code Plus.
- "Slim" is the README's name for the plain base install (LiteLLM-only, requires a cloud embedding provider and API key), also the Docker `:latest` tag. There is no `cocoindex-code-slim` package. The extras `[full]` and `[embeddings-local]` are identical today and add cocoindex[sentence-transformers] (torch + transformers, ~1 GB of dependencies).
- The base package's direct dependencies are mcp, cocoindex[litellm]>=1.0.17,<1.1.0, sqlite-vec, pydantic, numpy, einops, typer, msgspec, pathspec, pyyaml and questionary. The Rust engine arrives via cocoindex abi3 wheels (~9.3-10.1 MB) for Windows x86-64, macOS x86-64 and macOS ARM64, so all three target hosts have wheels. Package availability does not prove operational compatibility or performance.
- Storage is local: SQLite + sqlite-vec for vectors plus LMDB engine state (default map ceiling 4 GiB), under <project>/.cocoindex_code/. Postgres is not needed. Cloud embeddings remove only local embedding inference. Scanning, chunking (Tree-sitter, defaults 1000/250/150), the daemon, SQLite and vector search stay local, so CocoIndex is an additional consumer next to CodeIndexer.
- "Zero local resource usage" and the Docker sizes (~450 MB slim, ~5 GB full) are marketing numbers, not measured RSS. Docker Hub shows compressed ~204 MB for slim and ~3.3 GB for full. No published measurement of the slim cloud mode exists, so the net RAM/CPU saving is UNKNOWN and cannot support a resource-saving claim.
- The only public resource data for 0.2.42 is third party (agent-takkub issues #901 and #904, Windows 11): the daemon used ~62% CPU and ~1.8 GB RAM, even with no index for the active project. That setup used local embeddings, not slim, but it is a risk signal for the Windows Hub, so idle RSS must be a hard trial gate.
- Lifecycle: the daemon auto-starts (default idle timeout 180 min, keep_alive_with_mcp=true), indexing is incremental via CocoIndex memoization, and on-demand CLI use (ccc index / ccc search) works without a permanent MCP. A watcher auto-refresh (#276) and full-text search (#192) are not released. Known fragilities: daemon/client protocol (0.2.38 yanked), Windows path and socket bugs, and displaced-daemon fixes that sit on main but not in 0.2.42.
- Functional limits: semantic search only (no call graph, no symbol navigation, no built-in no-answer signal). PowerShell falls back to text splitting rather than AST. `ccc grep` released status is contradicted between the README and the framework docs (one vendor says it needs an unreleased cocoindex build, another says it is released). The 70% token-saving claim is unproven: one independent benchmark found no tool in the category saved 60% (31.6% on Codex, 15.9% on Claude Code).
- Cost: the tool is free. Cost is only provider embedding tokens. OpenAI text-embedding-3-small is $0.02/1M tokens. Voyage gives 200M free tokens but defaults to training use of content, and opting out needs a payment method. Gemini's free tier uses content for product improvement. Under 'no new plan/card/payments', a chat subscription or an API key alone is not proof of embeddings credit. Without confirmed existing credit the decision is PARK.
- Vendors split on the verdict, but all agree it is not KEEP. ChatGPT, Mistral and the merged report say TRY: Windows-Hub-only canary, base cocoindex-code==0.2.42 with remote embeddings, isolated environment and project COPY, CLI only. Claude.ai and Grok say PARK by default, moving to a canary only with confirmed existing credit. Nobody supports expanding to the Mac Pro or Mac Studio before the Hub trial passes.
- The earlier 40-answer v2 comparison is invalid as evidence (context overflow, zero successful mgrep calls). The October DR18 work is historical context only. No valid CodeIndexer-vs-ccc measurement exists.

## Рекомендации / решения
- Do not adopt (no KEEP). Conditional TRY: a single canary on the Windows Hub only, if existing API credit with an acceptable embeddings provider is confirmed; otherwise PARK.
- Install only base `cocoindex-code==0.2.42`, never `[full]` or `[embeddings-local]`. Use an isolated environment, an isolated COCOINDEX_CODE_DIR and a COPY of the corpus. Pin the complete environment, including the resolved cocoindex version (1.0.25 or 1.1.0 may be resolved), and capture the resolver output.
- Avoid plugin hooks, permanent MCP registration, watchers, home-directory Docker mounts, the bare `cocoindex-code` command (it silently writes settings) and mixing ccc versions that share one COCOINDEX_CODE_DIR. Use only the on-demand CLI (ccc index / search) and run `ccc daemon stop` afterwards, with keep_alive_with_mcp=false.
- Minimal config: provider litellm, model openai/text-embedding-3-small (the most terms-compatible paid option), min_interval_ms 300, API key kept out of the repo. Set COCOINDEX_DISABLE_USAGE_TRACKING=1 to opt out of telemetry.
- Preregistered trial: measure incremental RSS/CPU/disk/network and provider token cost against the running CodeIndexer, with idle RSS as a hard gate. Add correctness and freshness checks (stale hits, PowerShell, exact and symbol lookups, RU/EN). Only after a pass plus an independent review or readback, extend to the Mac Pro and Mac Studio.
- If the budget rule matters, check embeddings credit in each provider dashboard (a chat subscription does not imply API credit). Avoid Voyage free tokens unless training use on code is acceptable.
- Keep CodeIndexer as the baseline. CodeIndexer alone is cheaper unless a measured benefit is shown.

## Сущности
- **Люди:** georgeh0
- **Компании:** CocoIndex, OpenAI, Voyage AI, Google, Zilliz, MongoDB, Anthropic, Mistral, NetTechnologys s.r.o.
- **Продукты/инструменты:** CocoIndex Code (ccc), cocoindex-code 0.2.42, CocoIndex framework, CocoIndex Code Plus, CodeIndexer, LiteLLM, sqlite-vec, LMDB, sentence-transformers, Tree-sitter, OpenAI text-embedding-3-small, Voyage voyage-code-3 / voyage-code-4, Gemini embedding, Docker cocoindex/cocoindex-code, claude-context (Zilliz), Claude Code, Codex, Cursor, agent-takkub

## Открытые вопросы
- Actual incremental RSS/CPU/disk/network and provider cost of slim cloud mode next to CodeIndexer on the Hub, Mac Pro and Mac Studio (UNKNOWN until measured).
- Whether the operator has confirmed, usable embeddings API credit with an acceptable data policy (the report covers only the OpenAI and Voyage paths, Gemini only as a free-tier note).
- Whether `ccc grep` structural search is released or needs an unreleased cocoindex build (README and framework docs contradict each other).
- Which cocoindex framework version a fresh install resolves (1.0.25 vs 1.1.0 vs the <1.1.0 range) and whether it behaves as the release was tested.
- Whether the displaced-daemon and stale-daemon socket bugs (#243, #288) affect the released 0.2.42 on Windows, and how the Windows path-filter and encoding bugs behave in the trial.
- Correctness and freshness versus CodeIndexer (no valid comparison exists), plus RU/EN embedding quality, PowerShell coverage and no-answer behaviour.
- Report text was truncated per vendor (about 10k of 14-51k characters each), so the cost model, trial protocol details and adversarial sections were not seen in full.

## Источник
- DR-ID `DR26-11-01-HUB-08-1444` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- CodeIndexer
- code search / semantic code index
- LiteLLM embedding providers
- MCP server lifecycle (on-demand, no stdio in always-on config)
- preregistered trial and UNKNOWN != 0
- resource-saving claim requires measurement
- Shadow-first / canary rollout
- Cloud embeddings data terms and privacy

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-02 через gh: 6 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 6 · 🤷 0.


## Check it yourself: prompt, sources, raw reports

- Prompt: [DR26-11-01-HUB-08-1444-cocoindex-code-ccc-slim-cloud-embeddings-as-an-on-demand-add-on-to-cod/prompt.md](DR26-11-01-HUB-08-1444-cocoindex-code-ccc-slim-cloud-embeddings-as-an-on-demand-add-on-to-cod/prompt.md)
- All sources (112 links): [DR26-11-01-HUB-08-1444-cocoindex-code-ccc-slim-cloud-embeddings-as-an-on-demand-add-on-to-cod/sources.md](DR26-11-01-HUB-08-1444-cocoindex-code-ccc-slim-cloud-embeddings-as-an-on-demand-add-on-to-cod/sources.md)

## Raw model reports

5 reports delivered, synthesis used 5 of 6. [DR26-11-01-HUB-08-1444-cocoindex-code-ccc-slim-cloud-embeddings-as-an-on-demand-add-on-to-cod/](DR26-11-01-HUB-08-1444-cocoindex-code-ccc-slim-cloud-embeddings-as-an-on-demand-add-on-to-cod/README.md)
