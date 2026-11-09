# DR26-11-01-HUB-06-1441 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

**# Mixedbread mgrep / Mixedbread Search — Deep Research Report**

**Research questions:** Exact identity and portability of official `@mixedbread/mgrep` 0.1.13 (and Mixedbread Search) as a bounded on-demand semantic cloud fallback for an existing CodeIndexer-first multi-client (Claude Code, Codex, Cursor) workflow on Windows, Intel Mac, and Apple Silicon Mac; cross-OS path/sync/freshness semantics; privacy/ACL/retention; current public pricing and controls; falsifiable benchmark design; strongest arguments against adoption and reversible canary.

**Scope / Date:** Public primary sources only (GitHub mixedbread-ai/mgrep, npm, mixedbread.com docs/pricing/security/ToS/Privacy, changelog). Snapshot ~1 Nov 2026. No installs, uploads, account changes, or private data. Prior 40-answer test and Oct 27 billing treated as non-evidence / historical per request. Moving compute between local hosts is not a saving.

**Overall confidence:** Medium-High on identity, CLI path filter mechanics, pricing structure, and privacy contradictions (primary sources). Medium on exact stored path format and full sync edge cases (source + user-observed mismatch; full upload path construction not fully extracted in one pass). Low on untested edge cases (Unicode, case, separators, multi-clone ownership).

## Executive Summary

- Official package is `@mixedbread/mgrep` v0.1.13 (Apache-2.0 CLI; last release 25 Apr 2026, commit 5c1ba62). It is a thin Node/TypeScript client over Mixedbread Search cloud stores. Latest npm matches installed 0.1.13. High confidence.
- Native CLI stores and filters on absolute local paths (`metadata.path` + `starts_with` against `process.cwd()`-resolved path). This is the root cause of the observed Windows-populated store returning empty from Mac CLI while SDK can succeed. One private store with canonical repo-relative paths + `repo_id` + revision is **not** supported by unmodified native CLI; it is supported via official SDK/API custom metadata and filters. Medium-High.
- Privacy: stores default private (`is_public=false`). Security docs claim “never used to train models, for any user on any plan.” ToS and Privacy Policy contradict each other on free-tier model training (ToS allows; Privacy Policy denies). US region by default; free-tier retention indefinite. Documented issue #145. High on contradiction, Medium on actual practice.
- Pricing (public as of Nov 2026): Starter free + $5 one-time credits; Scale $20/mo including $20 credits then pay-as-you-go. Index Fast $1.50 / High Quality $3 per 1M content tokens; semantic search $4/1K (+$3.50 rerank); storage $0.50/1M tokens/mo; Toast 1 at discounted rates. Usage metrics lag up to ~10 min; small overdrafts noted. Monthly budgets / spending limits exist. High.
- Freshness is one-shot or watch-based with hash/dedup and delete detection, but multi-clone/revision ownership and absolute-path coupling make safe shared use non-trivial without adapter. No permanent watch required for bounded fallback.
- Strongest argument against: non-portable absolute-path coupling in native CLI + free/paid training-policy contradiction + US residency + storage/index costs on large or frequently changing trees. Minimal reversible canary is possible with disposable store, SDK adapter for relative paths, hard call/token/spend limits, and CodeIndexer-first trigger.
- Decision posture: PARK native CLI multi-OS use; TRY only with explicit SDK adapter + disposable store + measured canary that counts actual delivered context, successful calls, and attributable charges. No unsupported resource savings claimed.

## Detailed Findings

### 1. Exact identity, repositories, release vs 0.1.13, license, runtime, history, issues

**Fact:** Official CLI repository is https://github.com/mixedbread-ai/mgrep (Apache-2.0). npm package `@mixedbread/mgrep` version 0.1.13 (published ~25 Apr 2026). Bin points to `dist/index.js`. Dependencies include `@mixedbread/sdk` ^0.43.0 and `xxhash-wasm`. Related monorepo openbread contains `@mixedbread/cli`. Hosted service is Mixedbread Search / Stores at mixedbread.com / api.mixedbread.com.

**Fact:** v0.1.13 is latest (release notes: prompt-to-install new versions). Prior recent tags include 0.1.12 (auth refresh), 0.1.11 (org filter, watch ignore, hooks). Open issues include Windows hook compatibility (`os.setsid`, `/tmp`, mgrep.cmd), process leaks / stale PIDs, privacy ToS/Privacy contradiction (#145), and quota display mismatches. No major open path-portability issue found; behavior is by design of absolute filter.

**Fact:** CLI license Apache-2.0. Hosted terms are separate (Terms & Conditions + Privacy Policy). Node/ESM runtime; no hard `engines` field observed in package.json snippet. Works on Windows (user smoke succeeded) and macOS; hook scripts have known POSIX assumptions. Inference: modern Node LTS sufficient. Unknown exact minimum Node.

**Vendor claim:** “Calm, CLI-native way to semantically grep everything”; reduces agent tokens ~2x in internal tests. Distinguish from independent measurement.

### 2. Native portability: search/sync/upload source, flags, filters, one store across OS

**Fact (CLI search path filter):** In `src/commands/search.ts` the search path is resolved as absolute:
```ts
const root = process.cwd();
const search_path = exec_path?.startsWith("/")
  ? exec_path
  : normalize(join(root, exec_path ?? ""));
```
Then filter:
```ts
const filters = {
  all: [{ key: "path", operator: "starts_with", value: search_path }]
};
```
Result formatting strips `pwd` for display. This matches the observed failure: Windows absolute `metadata.path` does not `starts_with` a Mac absolute path.

**Fact (API support):** Mixedbread metadata filters support `starts_with` (and eq, in, contains, regex, etc.). Official docs recommend `external_id` as relative repository path. Upload accepts arbitrary metadata (strings, numbers, booleans, arrays) that is inherited by chunks and filterable. Overwrite default true when external_id collides.

**Fact (CLI vs SDK):** Native CLI hardcodes the absolute-path filter and does not expose a simple flag to override metadata key or use relative canonical paths. Official SDK/API allows full control: upload with metadata `{ path: "src/lib/foo.py", repo_id: "...", revision: "...", file_hash: "..." }` and search with matching filters (e.g. `starts_with` on relative path + eq on repo_id/revision). DeepWiki and file helpers show relative conversion + separator normalization (`\` → `/`) for ignore checks, but the search filter uses absolute.

**Inference / unverified:** Exact stored `metadata.path` in current CLI uploads is absolute local path of the uploading host (consistent with user report and filter design). Full upload construction lives in helpers not fully extracted here; hash likely xxhash (dependency present). No official flag or config for “portable relative mode” found.

**Answer to the concrete question:** ONE private store can hold canonical repo-relative paths + repo_id + revision and be consumed from Windows/Mac **via official SDK/API filters**, without a fork. Native CLI cannot do this without post-processing results or a thin adapter that bypasses the hardcoded absolute filter. Minimal adapter (SDK wrapper that injects relative metadata and filters) is sufficient; no fake cwd required. Absolute roots, Unicode, case, separators, duplicate basenames, wrong clone/revision remain **unverified** in public sources—must be tested on disposable copies.

### 3. Freshness: one-shot sync, hashes, create/edit/rename/delete, multi-clone

**Fact:** `mgrep watch` does initial sync then file watching (per-directory non-recursive to avoid inotify limits). Respects `.gitignore` / `.mgrepignore`. Search has `--sync` / `-s` for one-shot sync before query and `--dry-run`. Deletes are detected (stat failure → deleteFile). Renames treated as delete+create. Ignore includes hidden files and common build artifacts.

**Fact:** Dedup via content hash (xxhash-wasm dependency; DeepWiki mentions hash in FileMetadata). Upload ownership is by external_id; overwrite replaces. Query-time revision validation is possible only if you store revision in metadata and filter on it (SDK). Exact source readback is available via file retrieve / content.

**Inference / risk:** Absolute-path coupling means two clones of the same repo on different machines/OS appear as different files. Relative + repo_id scoping avoids accidental cross-clone deletes if the adapter never deletes on the basis of local absence alone, or uses revision-scoped external_ids. Multi-revision or multi-clone without scoping can overwrite or delete the other’s files. Lifecycle tests must be on disposable copies. Staleness is a function of explicit sync cadence; no permanent watcher required for bounded fallback.

### 4. Bounded fallback design (CodeIndexer first)

Ordinary shell is preferred. Trigger only on explicit CodeIndexer semantic failure or ambiguity (not per-session stdio MCP, not auto-install watchers). Hard limits: topK (e.g. 5–10), snippet/context size, timeout, retry count, total calls per task/session. Exact-symbol / navigation / no-answer cases should short-circuit to CodeIndexer or local tools. RU/EN both supported by vendor claim (multilingual). Adapter can be a small script usable by Claude Code, Codex, and Cursor. No automatic watchers.

### 5. Privacy: private defaults, ACL, retention, training, region

**Fact:** Stores default `is_public=false` (organization-only). Public stores exist but searches billed to caller. API keys support scoped permissions (search-only, per-store, tags). Encryption TLS 1.2+ in transit, AES-256 at rest. SOC 2 Type 2, ISO 27001, GDPR claims. US processing/storage by default; EU residency “coming soon”. BYOB (Bring Your Own Bucket) available for Enterprise with zero data retention attestation.

**Fact (contradiction):** Security docs: “Your content and usage are never used to train models, for any user on any plan.” Terms & Conditions §3.1.1 (free tier): Usage Data “may be used for … model training”. Privacy Policy (free tier): “We do not use any data from Free Tier users for model training.” Paid tier: only operational data, no training. Free-tier retention indefinite; deletion not guaranteed. GitHub issue #145 documents the contradiction (ToS/Privacy last updated ~Apr 2026). Separate guarantees (security page) from unknown (actual free-tier practice until clarified).

Never publicize private stores. Upload allowlists / exclusions via ignore files + config. For proprietary code prefer paid tier + explicit confirmation, or disposable stores only.

### 6. Current public pricing and included-credit accounting

**Fact (plans):** Starter free + $5 one-time credits (3 users, 3 stores, 100 queries/min). Scale $20/mo including $20 credits, then pay-as-you-go (unlimited users, 10k stores, higher rate limits, backups). Enterprise custom (BYOB/BYOC, etc.). Startup credits up to $250 possible.

**Fact (usage rates, approximate public):**
- Index: Fast $1.50 / High Quality $3 per 1M content tokens.
- Search: Grep $0.10/1K; Semantic $4/1K (+$3.50 with rerank); Toast 1 related rates discounted (launch pricing noted).
- Storage: $0.50 per 1M content tokens / month.
- Content token estimates published (text ~1.4/word, pages/images/video/audio higher).

**Fact:** Usage metrics may lag up to 10 minutes. Small overdrafts allowed to avoid interruption. Monthly budgets / spending limits (pause or continue + alert) available. No new cards/tiers required for evaluation within credits. Scenario formulas only: e.g. 10M content tokens High Quality index ≈ $30 once + $5/mo storage; 10k semantic queries ≈ $40 (+rerank). Actual cost depends on file mix, reprocessing, and credits. Live balance checked separately.

### 7. Falsifiable benchmark design (not executed)

Representative Python/JS/TS/PowerShell (when present), RU/EN, known-symbol / exact / semantic / navigation / no-answer. Identical immutable allowlisted snapshot with `repo_id` / revision / relative_path / file_hash. Source oracle hidden from independent answerers. Blinded independent judge. Counterbalanced baseline (CodeIndexer only) vs baseline+product. Count **all** actually delivered tool context **after** serialization (errors/wrappers included). Count actual successful product calls. Measure correctness, omissions, false answers. Cold/warm wall time, process-tree peak/idle RAM, CPU-seconds, GPU/VRAM if any, disk/network, attributable cloud charges. Three repeats + uncertainty. Host trials serialized; do not stop production services to fake cold. Separate invocation/use from demonstrated benefit.

**Precommit thresholds (example, adjust):** KEEP if accuracy non-inferior, total delivered context reduced ≥20 % on semantic failures, attributable cost < threshold, no privacy regression, and path/freshness tests pass. TRY if partial. PARK if path non-portability unfixed, training ambiguity unresolved, or net cost/context increase.

### 8. Strongest argument AGAINST, failure modes, minimal reversible canary, unresolved

**Strongest against:** Absolute-path coupling in native CLI makes a single private store non-portable across Windows/Mac without an adapter. Combined with free-tier training-policy contradiction (or at least public ambiguity), US residency, and ongoing storage/index costs, the “reduce total fleet RAM/CPU/GPU while preserving privacy and cost transparency” goal is not met by drop-in CLI use. Moving compute to cloud is not free.

**Likely failure modes:** Empty results on cross-OS (already observed); accidental delete/overwrite across clones if relative paths collide without repo_id/revision scoping; stale index if sync skipped; free-tier data retention/training risk; accounting lag / small overdraft; rate limits on free tier; Unicode/case/separator mismatches unverified.

**Minimal reversible canary:** Disposable store (or tagged). SDK-only adapter that uploads with relative path + repo_id + revision + hash, filters on those, never uses native absolute filter. CodeIndexer first; product only on explicit semantic miss; hard topK/timeout/call/spend caps. One-shot sync only on disposable copies. Rollback: stop calling adapter, delete disposable store, revoke scoped key. No permanent watchers, no new accounts/stores/installs beyond existing isolated 0.1.13.

**Unresolved / unverified:** Exact bytes stored as `metadata.path` by current CLI (absolute confirmed by behavior); full rename/delete ownership under concurrent clones; Unicode, case sensitivity, separator normalization on all three OSes; exact min Node; whether paid-tier practice matches security-page claim; real attributable charges on the user’s corpus; any GPU use (none expected).

## Synthesis and Implications

Native mgrep CLI is a convenient local-same-machine semantic front-end but is not a portable multi-OS companion for a shared CodeIndexer workflow without adaptation. The official SDK/API already supports the required schema (relative path + repo_id + revision + hash + filters). A thin, explicit, bounded adapter is the only path that preserves the decision criteria. Privacy requires treating free tier as higher-risk until the ToS/Privacy contradiction is resolved in writing. Cost is transparent at the rate-card level but must be measured on real snapshots. No resource-saving claim is supported without the canary measurements described.

## Limitations and Open Questions

Public sources only; no live account or code execution. Upload path construction and exact hash format not fully quoted from source. Edge-case tests (Unicode, case, multi-clone) remain to be run on disposable copies. Training-policy contradiction is public and unresolved. Pricing is rate-card; actuals require measurement.

## Source Catalog

- https://github.com/mixedbread-ai/mgrep (README, package.json 0.1.13, search.ts, watch.ts, issues) — primary
- https://www.mixedbread.com/pricing and /docs/platform/billing — primary
- https://www.mixedbread.com/docs/production/security, privacy, terms — primary
- https://www.mixedbread.com/docs/stores/search, store-files, metadata-filtering — primary
- GitHub issue #145 (ToS/Privacy contradiction)
- DeepWiki summaries of path/metadata (secondary, used for orientation)

All major claims above are tied to the cited primary sources or explicitly labeled inference/unknown.