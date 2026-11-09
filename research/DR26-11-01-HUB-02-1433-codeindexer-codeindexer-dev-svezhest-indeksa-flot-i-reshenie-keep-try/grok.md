# DR26-11-01-HUB-02-1433 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

CodeIndexer (codeindexer.dev) — Deep Research Report

Research questions: Точная идентичность продукта Code Indexer (https://codeindexer.dev, community-репозиторий https://github.com/Nettechnologys/codeindexer), лицензирование/EULA vs LICENSE репозитория, инкрементальная свежесть индекса, ресурсная архитектура (HTTP vs stdio, GPU/CPU), retrieval/navigation, portable identity across hosts, security/privacy, минимальный canary-тест и KEEP/TRY/PARK decision tree для fleet (Claude/Codex/Cursor на Windows Hub, Intel Mac Pro, ARM Mac Studio).

Scope / Date: Внешнее веб-исследование по состоянию на 2026-11-01. Только этот продукт (не путать с другими code-index/cidx/code-indexer). Различение verified release history / marketing / deployed binary (live 0.11.0).

Overall confidence: Medium. Официальные docs/pricing/FAQ/security/advisories дают высокую уверенность по identity, tiers, local-only claims и high-level CLI. Низкая–средняя по внутренностям freshness (watcher, auto_reindex defaults, schema data-dir, exact path handling, PowerShell, no-op refresh) — proprietary binary, публичная документация не раскрывает implementation details.

Executive Summary
Identity и ownership (High): Продукт — proprietary signed binary (Rust) от NetTechnologys s.r.o. Репозиторий GitHub — только community/disclosure space (README, SECURITY.md, LICENSE на файлы репо, advisories). Engine source не опубликован. Free-tier — отдельный compile-time build с лимитами. Website рекламирует v0.12.0; live Hub — 0.11.0 (commit 328915f, build_date 2026-07-24). Verified public release history отсутствует (нет changelog/releases); advisories фиксируют fixes в 0.11.0 и 0.12.0.⁠GitHub
Licensing и hardware (High for policy, Medium for binding details): Free (0$, ≤3 projects / 100k chunks total, symbol/semantic/exact); Pro ($16/mo annual, unlimited + cascade + call graph + quality); Security ($32/mo, +audit); Enterprise (custom, multi-user/shared). Paid: один HTTPS call/день (license ID + anonymous machine fingerprint), 14-day grace при offline, затем fallback на Free limits. Free — zero network. Offline activation/grace documented; exact hardware binding schema и relocate/export details — частично (есть relocate path update).⁠Codeindexer
Freshness (Medium): Marketing/FAQ: после initial только changed files, “usually under a second on save”. Docs: CLI reindex incremental by default (--force full). Нет документированного server-side file watcher или auto_reindex default в публичных страницах. Config: ~/.config/codeindexer/config.toml + per-project .indexer.toml. User observation (stale last_indexed 2026-10-27 при изменениях) consistent с отсутствием guaranteed auto-watcher. Freshness unknown для non-git roots — official warning. Demonstrate fresh bytes: только через reindex + status/doctor + independent readback.⁠Codeindexer
Resources (Medium): Preferred shared HTTP server (default 127.0.0.1:8978) — один процесс, multiple clients. Альтернатива stdio (agent launches binary). GPU backends выбираются при download: CPU / CUDA / DirectML / Metal / TensorRT. Peak RAM indexing ~1–2 GB/project; index size typically 5–15% source. LanceDB. Knobs: documented CLI (compact, rebuild-fts, quantize); precise idle/lazy/contention — UNKNOWN, измерять локально. Различать VRAM / RSS / private / disk weights — нет public breakdown.⁠Codeindexer
Retrieval (High for modes, Medium for semantics): Symbol + semantic + exact (cascade = Pro). Callers/callees/chains/cycles = Pro. 27 languages full call-graph (Python/JS/TS/Bash etc.; PowerShell не в списках). RU/EN via embeddings (general-purpose model). False no-answer может быть stale/unsupported parser. Exact source read tools (read_chunk, read_file_range). Freshness warnings clients должны учитывать. Website ~200ms benchmarks — marketing, не measured here.⁠Codeindexer
Portable identity (Medium): relocate <project> <new-path> updates path without reindex. Нет публичного полного export/import schema для repo_id+revision+relative path+hash across Windows drive/UNC / Mac / case / Unicode NFC. Existing relocate — частичное решение; для fleet нужен explicit mapping (не equating E: и /Users). Dirty roots / scratch copies — risk contamination.⁠Codeindexer
Security/privacy (High for local claims): Все processing local; bind 127.0.0.1 default. No code/queries to vendor. Advisories (published 2026-10-21): path traversal (fixed 0.11.0), Host-header/DNS rebinding, unvalidated project IDs, per-key MCP auth (fixed 0.12.0), archive escape (0.7.0). MCP auth/bind/host confinement improved in later versions. Local/offline/code-retention separated from optional LLM triage (BYO LLM, Security). Backup: plain files, inspectable.⁠Codeindexer
Decision: KEEP CodeIndexer as primary (installed, shared HTTP, on-demand). TRY measured manual/incremental reindex cadence + doctor/status verification of fresh bytes. PARK full upgrade to 0.12.0 until compatibility/rollback evidence; optional cloud challengers only for ordinary non-secret code. Bounded rollback: leave working 0.11.0 intact, test side-by-side. Strongest case against: proprietary internals + observed staleness make “automatic freshness” unproven for fleet. Cheapest next: frozen snapshot paired test (primary vs tuned primary) on real Python/JS/TS/PowerShell.
Detailed Findings
1. Identity, ownership, engine license/EULA vs repository license; Free/Pro/Security/Enterprise limits; hardware binding, offline, platforms, compatibility/relocate/data-directory

Documented: NetTechnologys s.r.o. owns the proprietary commercial product. GitHub repo explicitly: “The Code Indexer software itself is a proprietary commercial product distributed as signed binaries… It is not open source. The source code is not published here.” LICENSE in repo applies only to the files in that repository. Terms of Service (last updated 2026-05-26): non-exclusive, non-transferable right to use on purchased plan; customer data (source, configs, processed data) remains customer property. Website advertises v0.12.0. Platforms: statically-linked native binary for macOS, Linux, Windows; GPU builds selected at download (CPU/CUDA/DirectML/Metal/TensorRT).

Limits (exact from pricing/FAQ):

Free: ≤3 projects, 100 000 chunks total, symbol/semantic/exact, read tools, memory/roadmap within 8 phases / 42 cards / 64 reports, community support.
Pro: unlimited + cascade + full call graph + quality scanners + cross-file + history. $16/mo annual ($192/year).
Security: + deep audit (CWE, reachability CVE, supply-chain, optional LLM triage BYO, CI/CD, compliance). $32/mo annual.
Enterprise: multi-user, shared indexes, custom parsers, air-gapped options, SLA.

Hardware/offline: Paid tiers — one HTTPS call/day with license ID + anonymous machine fingerprint (~200 bytes). 14-day grace if unreachable, then Free limits. Free — zero network calls. License files 0600 on Unix. codeindexer activate/deactivate/license/upgrade. relocate updates path without reindex. Index format versioned; auto-migrate; breaking changes ≤1/year with 30-day notice. Data in user data directory (plain files: index, embeddings, memory cards, license, optional audit log). Exact schema of data-directory and full export/import — not published.⁠GitHub

Observed-by-us: Live Hub /health = 0.11.0, commit 328915f, build_date 2026-07-24. last_indexed=2026-10-27T17:27:11Z, total_files=3738, chunks=28265, symbols=33773. Scratch/backup copies present. Independent readback shows stale signature on some files.

Inferred: Website 0.12.0 is current marketing; 0.11.0 is stable deployed. Advisories show 0.11.0 already fixed path traversal; 0.12.0 fixed remaining (Host, project ID, MCP auth).

UNKNOWN / conflicting: Exact public changelog or GitHub releases for binary versions. Precise hardware binding algorithm (beyond “anonymous fingerprint of stable hardware IDs”). Full data-directory schema and cross-host export compatibility. Windows drive letter / UNC / case / Unicode NFC handling in relocate. Dates of docs pages not always explicit.

2. Incremental freshness: watcher vs CLI/MCP reindex; config locations; auto_reindex; ignores; dirty tree; renamed/deleted; unsupported languages; large files; symlinks; locks; demonstrate fresh bytes

Documented: CLI codeindexer reindex <project> — incremental by default, --force for full. status, doctor (metadata, integrity, duplicates), jobs (when server running). Config locations: ~/.config/codeindexer/config.toml (global), .indexer.toml (per-project). codeindexer init generates default with options documented inline; project-config shows effective. Marketing/FAQ: “only changed files get re-indexed — usually under a second on save”; 5–30 s / 10k LOC initial, 1–3 min for 200k LOC. No mention of built-in server file watcher or auto_reindex key in public docs pages. Languages: PowerShell not listed in any category (full graph / symbols / searchable).

Observed-by-us: Index stale relative to disk changes (signature mismatch on x_outbox.enqueue etc.). Search explicitly warns freshness unknown for non-git roots. Scratch/backup copies appear in report.

Inferred: Freshness relies on explicit reindex or (undocumented) background mechanism that did not fire for the observed changes. No-op refresh possible if hash/mtime matches, but not verified.

UNKNOWN: Exact auto_reindex default, ignore precedence (.gitignore vs .indexer.toml vs defaults), handling of dirty working tree, renames/deletes (purge?), large files cutoff, symlink following/locks, PowerShell chunking behavior (likely line-based or skipped). How clients surface freshness warnings. No proprietary implementation invented.

To demonstrate fresh bytes (not just job acceptance / indexing_complete): run reindex, then status/doctor, then independent readback of specific file content/hash vs indexed chunk. Shared host resource runs must serialize.

3. Resource architecture: HTTP shared vs stdio; embedding/rerank backends; RAM scaling; disk/cache; lazy/idle; contention; knobs; what to measure

Documented: One HTTP server (codeindexer serve, default 127.0.0.1:8978, dashboard on same port). Clients connect via MCP HTTP/SSE or Streamable HTTP. Alternative: serve --stdio (agent launches binary, no long-lived server). Embedding/rerank models ship inside binary (Enterprise air-gapped ready). GPU selection at download. Peak RAM ~1–2 GB during indexing; index 5–15% of source tree; “tunes itself to your RAM — comfortable on a 16 GB laptop”. Storage: LanceDB files (compact merges fragments; rebuild-fts, rebuild-index, quantize/TurboQuant).

Inferred: Shared HTTP preferred for fleet (one process, multiple Claude/Codex/Cursor). Stdio per-session increases process count and RAM/CPU. Simultaneous clients possible but indexing/search contention not quantified.

UNKNOWN / must measure locally: Exact embedding model name/dim, rerank backend, VRAM vs process RSS vs private memory, idle lifetime / lazy loading, no-op refresh cost, bounded incremental update wall time/CPU/GPU/disk, simultaneous client scaling. Distinguish these metrics; do not turn architecture claims into measured savings without local data.

4. Retrieval and navigation

Documented: Modes: symbol, semantic, exact; cascade (smart picker) = Pro. Call graph (callers, callees, chains, cycles, shortest path, blast radius) = Pro. Cross-file analysis, quality scanners = Pro. Read tools always available (read_chunk, read_file_range, outline). Languages: 27 full call-graph+references (Python, JS, TS, Bash, etc.; no PowerShell); additional symbols/searchable. Semantic works across languages via general-purpose embedding model. Compact mode reduces tool output.

False no-answer vs unsupported vs stale: Possible causes — parser unsupported (PowerShell), index stale (observed), or true absence. Clients should prefer cascade (Pro) then fall back to exact + source read; treat freshness warnings as signal to reindex or verify. Website 200 ms / 80% compact claims — marketing, not proven on this fleet.

5. Portable identity

Documented: relocate <project> <new-path> updates project path without reindexing. Memory cards/reports export to Markdown. Index is derived data; repo is source of truth.

Inferred / UNKNOWN: No public schema guaranteeing repo_id + revision/snapshot + repo-relative slash path + file hash survives Windows drive/UNC prefix, case sensitivity, Unicode NFC, duplicate basenames, dirty roots, or moving clones between Hub/Macs. Existing relocate helps path update but does not automatically provide full portable identity. Explicit simple mapping (or per-host reindex of non-secret trees) safer than blind equate of E: and /Users. Checkout isolation recommended.

6. Security/privacy/reliability

Documented advisories (published 2026-10-21, all fixed):

CI-SA-2026-001: Missing Host-header validation (DNS rebinding) — affected ≤0.11.0, fixed 0.12.0.
CI-SA-2026-002: Path traversal in source-lines endpoint — fixed 0.11.0.
CI-SA-2026-003: Unvalidated project identifiers — fixed 0.12.0.
CI-SA-2026-005: Per-key project authorization not enforced across MCP tools — fixed 0.12.0.
CI-SA-2026-009: Archive member paths escape — fixed 0.7.0.

Local execution, 127.0.0.1 default, no code/queries/embeddings to vendor. Free zero network. Paid fingerprint only. Optional audit logs. Binaries signed (Apple Developer ID, Authenticode, minisign). Database/index — plain files, backup by copying data directory. MCP exposure: default localhost; set password if exposing. Only defensive operational relevance noted.⁠GitHub

7. Minimal additive canary and falsifiable paired test

Frozen real snapshot of Python/JS/TS/PowerShell files (exclude archives/config JSON to avoid corpus contamination).

Retained primary (current 0.11.0 shared HTTP) vs tuned primary (explicit reindex cadence + doctor) or primary + optional cloud challenger (ordinary code only, never secrets/cookies/keys/confidential).
Queries: RU/EN symbol/exact/semantic/navigation/no-answer cases.
Independent answerers + blind source judge.
Metrics: exact delivered context after serialization, real tool usage, correctness/false answers, cold/warm wall time, peak/idle RAM, CPU/GPU/disk/network/cost.
Isolated create/edit/rename/delete tests for freshness.
Shared host runs serialize.
Success criteria: fresher bytes demonstrated, no regression in correctness, resource within budget.
8. Concrete KEEP / TRY / PARK decision tree

KEEP: CodeIndexer installed as primary. Shared HTTP server (not per-session stdio). On-demand tasks. Existing subscriptions/credits only. Ordinary code may go to approved cloud challenger; never secrets.

TRY (after accepted research, production changes not authorized by this prompt):

Measured manual or scheduled incremental reindex + status/doctor verification of fresh bytes.
Confirm config locations and effective ignores on each host (Windows vs Mac).
Side-by-side resource measurement (shared HTTP vs occasional stdio).

PARK:

Upgrade to advertised 0.12.0 until compatibility, migration, and rollback evidence collected (leave working 0.11.0 intact).
Per-session stdio processes, extra embeddings/watchers.
Treating architecture claims as measured savings.
Blind path equate across hosts.

Where it helps/fails for three-client three-host: Helps when shared HTTP + verified reindex gives consistent graph/search across Claude/Codex/Cursor with low operator effort and minimal local RAM/CPU/GPU. Fails if observed staleness persists (non-git roots, dirty trees, unsupported languages like PowerShell) or if resource contention appears under simultaneous clients.

Bounded rollback: Keep 0.11.0 binary and data directory intact; test any change in isolated copy or side-by-side.

Explicit unknowns: Auto-watcher presence and reliability, exact data-dir schema, full portable identity guarantees, precise resource numbers, PowerShell handling.

Strongest case against recommendation: Proprietary nature + live observation of staleness mean “it just stays fresh” is unproven; without local measurement any claim of zero operator effort is marketing.

Cheapest useful next experiment: Frozen snapshot + paired reindex test (before/after) on one host, measure fresh bytes, wall time, peak RAM, correctness on known symbols. Serialize if shared.

Synthesis and Implications

CodeIndexer is a legitimate local-first proprietary MCP code intelligence tool with clear tier differentiation, strong local/privacy claims, and useful CLI for shared HTTP use. Marketing freshness (“under a second on save”) is not matched by public docs (explicit reindex) or by the live observation of staleness. For the fleet (Windows Hub + Intel Mac Pro + ARM Mac Studio, Claude/Codex/Cursor, minimize local resources and operator effort), the practical path is KEEP the installed 0.11.0 as primary shared HTTP, TRY explicit verification/reindex cadence, and PARK upgrade or challengers until measurement. Existing subscriptions only; secrets stay local.

Limitations and Open Questions

Public documentation is high-level; proprietary binary hides watcher implementation, exact config schema, embedding model, path normalization, and resource internals. No independent third-party benchmarks or full changelog. User observation is single-point and does not prove watcher failure or fleet outage. New evidence that would change conclusions: public config reference showing auto_reindex, verified 0.12.0 migration notes, local measurements of freshness and resources, or vendor response on PowerShell and non-git roots.

Source Catalog
https://codeindexer.dev/ — product, v0.12.0 claim, local claims (accessed 2026-11-01)
https://codeindexer.dev/docs — CLI, config locations, transports, platforms (2026-11-01)
https://codeindexer.dev/pricing — exact tiers and limits (2026-11-01)
https://codeindexer.dev/faq — languages, indexing claims, open-source status, offline/grace (2026-11-01)
https://codeindexer.dev/terms — ToS, ownership, customer data (last updated 2026-05-26)
https://codeindexer.dev/security — local processing, fingerprint, advisories summary (2026-11-01)
https://github.com/Nettechnologys/codeindexer — README (proprietary engine), advisories directory (2026-11-01)
https://github.com/Nettechnologys/codeindexer/tree/main/docs/security/advisories — specific CI-SA-2026-* with version ranges (2026-11-01)
Appendix: Evidence Table
Claim	Documented	Observed-by-us	Inferred	UNKNOWN
Proprietary binary, not OSS; repo LICENSE ≠ engine	Yes (README, FAQ, ToS)	—	—	—
Website v0.12.0; live 0.11.0	Yes (site)	Yes (/health)	Marketing vs deployed	Full changelog
Free/Pro/Security limits & pricing	Yes	—	—	Exact seat binding
14-day grace, fingerprint only for paid	Yes	—	—	Exact fingerprint algorithm
Incremental reindex CLI; marketing “on save”	Yes (CLI + FAQ)	Stale index observed	Possible missing watcher	Auto_reindex default, watcher existence
Config ~/.config/... + .indexer.toml	Yes	—	—	Full option list / Windows path variant
PowerShell unsupported in lists	Yes	—	Likely no full graph	Exact chunking behavior
Shared HTTP preferred; stdio alternative	Yes	—	Lower resource for shared	Contention numbers
Peak RAM 1–2 GB; index 5–15%	Yes (FAQ)	—	—	Idle, VRAM vs RSS, per-client
relocate path update	Yes	—	Partial portability	Full identity schema across OS
Advisories fixed by 0.11/0.12	Yes	—	0.11.0 already safer on path	Residual risk in 0.11.0
Local-only, no code to vendor	Yes (multiple pages)	—	—	Optional LLM triage details

Missing evidence candidly: Public full config schema, embedding model identity, exact auto-freshness mechanism, data-directory layout, cross-host path normalization guarantees, independent performance numbers, verified upgrade path from 0.11.0. All claims above are grounded in the cited primary pages; no implementation invented.
