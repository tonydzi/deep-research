---
dr_id: DR26-11-01-HUB-04
title: "Hilum (local MCP code-intelligence tool): fleet-rollout suitability and canary plan"
date: 2026-11-01
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-01-HUB-04): Hilum (local MCP code-intelligence tool): fleet-rollout suitability and canary plan

> Is Hilum (hilum-tools, v0.1.0-rc.4, SAKIVA-related) worth deploying across a Windows/Mac fleet already using CodeIndexer + rg + LSP/tree-sitter, or should it be parked or trialed in a bounded canary?

## Ключевые выводы
- Decision: PARK fleet rollout; TRY only as a bounded, isolated canary for named use-cases. Confidence is high on release/source/licensing-status facts, medium on architecture interpretation, low on untested runtime guarantees.
- The public GitHub repo hilum-tools/hilum is a 'Binaries and installer' distribution repo with only README.md, install.sh, install.ps1 and minisign.pub. The engine source is not public, so binaries or auto-generated 'Source code' archives do not make Hilum open source.
- The latest release is v0.1.0-rc.4, a pre-release dated 10 Sep 2026 (rc.3 was 30 Aug 2026). There is no stable 0.1.0. Both installers use GitHub's /releases/latest, which excludes prereleases, so an unpinned install fails to find a stable build and rc.4 must be pinned explicitly.
- Licensing is unresolved. The author's page claims private sources under BSL 1.1 converting to Apache-2.0 on 1 Aug 2030, but the repo has no LICENSE file. The Additional Use Grant is not inspectable, so commercial or production use is UNKNOWN. The old pricing page's 'free local stack' is marketing, not a license.
- Supply chain: rc.4 publishes per-archive SHA-256 files, an aggregate SHA256SUMS and a .minisig signature. install.sh fails closed on checksum problems but skips signature verification when minisign is absent. install.ps1 has no minisign path. There is no OS signing (no Authenticode or notarization), and Windows runtime tests were not run.
- The mailbox/work queue has a verification lifecycle (acceptance criteria bound at post time, review states, recorded verdicts). Claim atomicity holds only within one filesystem. Syncing .hilum between machines is unsupported and two replicas can each win the same claim. Multi-host coordination is promised only via a future 'server contour' (SAKIVA), and public material shows no multi-host protocol, SLA or auth. Presence is in-memory only.
- Lease TTL/renewal, fencing tokens, idempotency keys, requester ACK, per-agent authorization and audit immutability have no public contract and remain UNKNOWN. Windows `daemon stop` terminates the daemon and abandons in-flight work.
- Capability claims are unproven against the baseline. Navigation works better with rust-analyzer, and syntax fallback is less accurate across files. Python/JS/PowerShell semantic coverage is not established (TypeScript and Rust are the vendor claims). Semantic search needs a first-use download of an embedding model and runtime. `memory recall` is keyword-only in rc.4. There are no public RU/EN benchmarks or resource numbers.
- rc.4 fixed a bug where an `explore` call without a workspace returned an empty answer labelled fresh and exhaustive. That is good epistemic engineering, but also counterevidence against trusting Hilum's 'no result' without version-specific testing. The release notes also contradict themselves: 'What changed' says the 4 GiB sidecar ceiling is gone, while 'Known limitations' still says a 4 GiB cap with about 2x memory on write. The likely cause is stale text, but this is unverified.
- Telemetry, retention, encryption at rest, local HTTP auth and SAKIVA tenancy/privacy are all UNKNOWN. The vendor says source bytes and local indexes do not enter the sync path, but this is not independently verified.

## Рекомендации / решения
- Do not roll Hilum out to the fleet. Run only an isolated, copy-only canary on one host, with a pinned rc.4 archive verified by SHA-256 and the minisign key (ID EA6FCF55310D8901), semantic embeddings disabled at first, and no changes to existing Claude Code/Codex/Cursor MCP configs.
- Keep .hilum out of any synced folder and never copy it between hosts. Log all outbound network traffic and treat unexplained egress as a hard failure. Keep secrets, cookies and keys out of the trial tree.
- Test two scenarios. (1) Mailbox: a requester posts a task, a second independent LLM client claims it, and the requester reads back and ACKs the result. Include negative cases: double claim, duplicate post, worker crash, daemon restart and stale worker. Use an external ACK journal only for experiment integrity and do not count it as a Hilum feature. (2) Navigation: source-verified cross-file cases, plus create/edit/rename/delete freshness polled every 5 s for up to 120 s.
- Run a preregistered 24-task benchmark: 4 each of known-symbol, exact, semantic-RU, semantic-EN, navigation and no-answer. Compare A (CodeIndexer + rg + LSP/tree-sitter) with B (A + Hilum), counterbalanced AB/BA, 3 repetitions (144 runs), blind judge, max 10 calls and 24,000 UTF-8 bytes of serialized tool output per task. Record native tokens separately, and make no benefit claim from runs with zero successful Hilum calls.
- Require provenance on every result (repo id, full git SHA, relative path, file SHA-256, line range, freshness). If Hilum does not supply it, add it from the external harness and mark it UNKNOWN for the product.
- Decision rule: KEEP a named use-case only if all hard gates pass, there are no new confident false answers, correctness drops by at most 1/24, and either median wall time or host CPU improves by at least 20%, or correctness improves by at least 3/24 with at most 20% extra time. Otherwise TRY if results are mixed or underpowered, and PARK on a hard failure. Unsupported languages count against the 90% coverage gate.
- Resource gates are policy, not expectations: idle incremental RAM at most 256 MiB per host and peak at most 1 GiB. Unmeasurable values are UNKNOWN, not zero. Installation success, `--version`, exit code 0 or an internal ACK must never count as a fleet PASS.
- Define rollback before the trial: stop only experiment-scoped processes, preserve evidence, remove only temporary config and storage, and verify the existing tools and channels match the pre-trial snapshot.
- Before any production or commercial use, get the exact BSL parameters and Additional Use Grant from the vendor, and treat SAKIVA as a separate future product to evaluate on its own terms and privacy documentation.

## Сущности
- **Люди:** Aleksandr Ivannikov
- **Компании:** SAKIVA, hilum-tools, MariaDB (BSL 1.1 text)
- **Продукты/инструменты:** Hilum, hilum-tools/hilum, SAKIVA, CodeIndexer, ripgrep (rg), Tree-sitter, Language Server Protocol, rust-analyzer, minisign, MCP (stdio / Streamable HTTP), Claude Code, Codex, Cursor, NATS KV, mgrep, Business Source License 1.1

## Открытые вопросы
- What are the exact BSL 1.1 parameters and Additional Use Grant, and are commercial or production use and fleet use permitted?
- What are SAKIVA's terms, privacy, retention, authentication and tenancy, and does a multi-host endpoint exist?
- Is there a complete public CLI/MCP schema? docs.hilum.tools was not retrievable, and the exact mailbox/task/claim/ACK commands are unknown.
- Does Hilum offer semantic coverage for Python, JavaScript and PowerShell, and how good are RU/EN semantic results?
- What are the lease/TTL, fencing, idempotency, requester ACK, per-agent authorization and audit semantics?
- Is the 4 GiB sidecar cap actually removed (stale 'Known limitations' text), and what do large indexes do to memory?
- Does Hilum emit telemetry or crash reports, what are its local retention and at-rest encryption, and what are the HTTP bind/auth settings?
- What are the real cold/warm RAM, CPU, GPU/VRAM, disk and network costs, and what are the embedding model and runtime identities and licenses?
- Does the engine behave correctly on Windows given that no Windows test suite has run, and when will signing and a stable release arrive?
- Is there a result contract that gives the same repo, revision and file hash across Windows and Mac, including case sensitivity and path portability?

## Источник
- DR-ID `DR26-11-01-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- CodeIndexer-first code navigation
- MCP servers on demand (no always-on stdio)
- Multi-host agent mailbox / work queue (NATS KV history)
- Shadow-first canary and paired A/B benchmark
- BSL 1.1 licensing and commercial-use risk
- Supply-chain verification (SHA-256, minisign, OS signing)
- Source provenance for retrieval results (repo, revision, file hash)
- Resource budgets per host (RAM/CPU/GPU)

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-02 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
