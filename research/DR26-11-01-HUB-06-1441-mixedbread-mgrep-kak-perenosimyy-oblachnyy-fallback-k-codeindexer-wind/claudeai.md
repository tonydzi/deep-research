# DR26-11-01-HUB-06-1441 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Mixedbread mgrep as a Bounded Cloud Fallback for CodeIndexer: Cross-OS Path, Freshness, Privacy, Cost and Benchmark Assessment (DR26-11-01-HUB-06-1441)

**Bottom line: PARK the native mgrep 0.1.13 CLI as the shared cross-OS fallback, because nothing in its public flags or docs lets one private store be read correctly from both Windows and Mac. The only path that avoids a fork is a small TRY canary: a thin adapter on the official Mixedbread SDK that writes repo-relative paths plus repo_id and revision as metadata and filters on them at query time.** The CLI has no documented flag for custom metadata or query filters. Its search is scoped by default to the local working directory,\[1\] and your own Mac/Windows failure shows that this scoping breaks when a store is shared across operating systems.\[2\] The Mixedbread Stores API does natively support everything a portable schema needs: arbitrary file metadata, `all/any/none` filters with `eq`/`starts_with`, private-by-default stores and per-store expiry.\[3\]\[4\] So a minimal adapter is justified. It needs no fake cwd and drops neither repo nor revision scope. No resource saving has been demonstrated yet; the prior 40-answer run had zero successful mgrep calls and is not evidence.

## TL;DR

- **Verdict: PARK native mgrep CLI; TRY only an SDK-adapter canary under precommitted gates.** npm latest is still 0.1.13 (Apache-2.0, 22 versions). From the public README there is no supported way to make the CLI read a Windows-populated store from macOS, and the CLI's own background-watch hooks have open bugs: orphaned watchers, a log that dumps the environment including API keys, and Windows `os.setsid`.
- **The portable design has to sit on the Stores API, not the CLI.** Upload with metadata `{repo_id, revision, rel_path, file_hash, ...}` and query with `{"all":[repo_id eq, revision eq, (rel_path starts_with)]}`. Then read the source back from the local git object at `revision:rel_path`. Whether mgrep itself writes absolute or normalized paths could not be verified from source in this pass (see Unknowns).
- **Cost is small but needs active control.** Semantic search with rerank (mgrep's default) costs $7.50 per 1,000 queries; without rerank it is $4. Fast indexing is $1.50 per 1M content tokens and storage $0.50 per 1M tokens per month. Usage metrics "can lag by up to ten minutes" and "small overdrafts are allowed", so put hard per-day call counters in the adapter rather than relying on the balance.\[5\]

## Key Findings (labelled)

| # | Finding | Label | Source (accessed 2026-11-01) |
|---|---|---|---|
| 1 | Official repo is `mixedbread-ai/mgrep`, TypeScript, Apache-2.0, 185 commits on `main`; the indexed repo page shows "Star 4.4k", "Fork 177", "Issues 44" and "Pull requests 14" (14 open PRs) | FACT | https://github.com/mixedbread-ai/mgrep |
| 2 | npm `@mixedbread/mgrep` latest = 0.1.13, Apache-2.0, 22 versions, "Published 2 months ago" in the indexed snapshot; maintainers `jdierkes`, `mixedbreadai`\[6\] | FACT (search-snippet of npm; direct fetch blocked by bot protection); jsDelivr independently shows 0.1.13\[7\] | https://www.npmjs.com/package/@mixedbread/mgrep ; https://www.jsdelivr.com/package/npm/@mixedbread/mgrep \[6\] |
| 3 | Your installed 0.1.13 = current latest; no newer release to upgrade to | INFERENCE from 2 | — |
| 4 | Contributor prerequisites: Node.js 18+, pnpm 8+; `package.json` `engines` field not read | FACT / UNVERIFIED | https://github.com/mixedbread-ai/mgrep/blob/main/CONTRIBUTING.md \[8\] |
| 5 | README: "Searches default to the current working directory unless you pass a path";\[1\] "Results include relative paths"\[9\] | FACT (doc) | README\[2\] |
| 6 | README lists agent installers for Claude Code, OpenCode, Codex, Factory Droid; "More agents (Cursor, Windsurf, etc.) are on the way" | FACT | README\[2\] |
| 7 | CLI flags: `-m`, `-c/--content`, `-a/--answer`, `-w/--web`, `--agentic`, `-s/--sync`, `-d/--dry-run`, `--no-rerank`, `--max-file-size`, `--max-file-count`, `--store <name>`; env `MXBAI_API_KEY`, `MXBAI_STORE` (default store `mgrep`), `MGREP_*`. No documented flag for custom metadata or metadata filter\[2\] | FACT (doc); absence = INFERENCE from docs | README\[2\] |
| 8 | Stores API supports `starts_with`, `eq`, `in`, `like`, `regex`, `contains`, `all/any/none`; string ops case-sensitive; system fields `file_id`, `chunk_index`\[4\] | FACT | https://www.mixedbread.com/docs/stores/metadata-filtering \[4\] |
| 9 | Stores are **private by default** ("Only your organization can access"); `is_public=True` lets "anyone with an API key" search\[10\] | FACT | https://www.mixedbread.com/docs/stores/stores |
| 10 | Paid tier: data "shall not be used for model training"; Free tier: Terms say data "may be used for … model training" and "retained indefinitely", while Privacy Policy says "We do not use any data from Free Tier users for model training"\[11\]\[12\] — contradiction tracked in open issue #145 | FACT (conflict) | https://www.mixedbread.com/pages/terms ; https://www.mixedbread.com/pages/privacy ; https://github.com/mixedbread-ai/mgrep/issues \[13\] |
| 11 | Pricing: Grep $0.10/1K, Semantic $4/1K (+$3.50 rerank), Fast index $1.50/1M content tokens, HQ $3, storage $0.50/1M tokens/month; Starter $5 one-time, Scale $20/mo with $20 credits\[5\] | FACT | https://www.mixedbread.com/pricing \[5\] |
| 12 | "Usage metrics can lag by up to ten minutes, and small overdrafts are allowed"\[5\] | FACT | pricing FAQ\[5\] |
| 13 | Changelog announces "Spending Limits … automatically pause requests when you approach the limit"\[14\] | VENDOR CLAIM (behaviour not tested) | https://www.mixedbread.com/docs/changelog |
| 14 | "~2x fewer tokens … at similar or better judged quality" (50 tasks, LLM judge)\[15\] | VENDOR CLAIM | README\[2\] |
| 15 | Exact `metadata.path` construction, cwd `starts_with` filter, hash and delete logic in `src/lib/store.ts` / `sync-helpers.ts` | **UNVERIFIED** (source files could not be fetched under this task's access rules) | — |

## 1. Identity, release, license, runtime, issues

**FACTS.**
- **Product identity.** The official CLI is `@mixedbread/mgrep` (https://www.npmjs.com/package/@mixedbread/mgrep), built from https://github.com/mixedbread-ai/mgrep. \[16\] The backend is Mixedbread Search / Stores (https://www.mixedbread.com/docs/concepts). \[6\]\[17\] Unrelated forks exist (F0rce/mgrep, vinuxito/mgrep, gmh5225/mgrep, jackyrx/X_mgrep), as do third-party wrappers such as `pi-mgrep`, which auto-runs `npm install -g` and should be avoided.\[18\] Do not use any of them.
- **Version.** 0.1.13 is latest per the npm snippet and jsDelivr.\[7\] The GitHub Releases page (github.com/mixedbread-ai/mgrep/releases) lists tagged releases including v0.1.11 (commit 93f5647), v0.1.10, v0.1.6 (14691ae) and v0.1.4 (950d4ce), but no v0.1.13 release appeared in the indexed snapshot, so treat npm as the version of record. I could not resolve the exact git commit that corresponds to the 0.1.13 tag; DeepWiki's analysis cites commit `5c1ba628` for `main` (secondary).\[15\]
- **License split.** The CLI code is Apache-2.0 (README, npm).\[2\]\[6\] The hosted service is governed separately by Mixedbread Terms (https://www.mixedbread.com/pages/terms, Delaware law, binding arbitration), the Privacy Policy and the DPA (https://www.mixedbread.com/pages/dpa). \[11\] Apache-2.0 covers only the client. The service terms let Mixedbread "modify, suspend, or discontinue any aspect of the Services at any time, with or without notice" (Terms §8).\[11\]
- **Runtime.** Node.js (ESM), TypeScript, executable `dist/index.js` (README).\[19\] The README's Development section says "Tests are not wired up yet" in one place and lists `bats` tests in another, so test coverage is thin.\[2\]\[20\]
- **Live issues relevant to this decision** (https://github.com/mixedbread-ai/mgrep/issues, open):

| Issue | Opened | Relevance to the decision |
|---|---|---|
| #151 | 2026-07-27 | SessionEnd hook SIGKILLs recycled PIDs\[13\] |
| #149 | 2026-06-12 | "mgrep watch processes leak on unclean session end — 600+ orphans accumulated, exhausted memory/swap". Directly contradicts any RAM-saving claim for the agent-hook mode\[13\] |
| #148 | 2026-06-12 | "Claude Code SessionStart hook dumps full os.environ (incl. API keys) to world-readable /tmp/mgrep-watch.log"\[13\] |
| #147 | 2026-04-27 (andrewsanchez) | Watch keeps running after uninstalling the Claude Code plugin\[13\] |
| #145 | 2026-04-14 (dxvidparham) | ToS vs Privacy contradiction (see section 5) |
| #138 | — | Proprietary-code concerns\[13\] |
| #136 | — | Dependency audit: 4 high vulnerabilities\[13\] |
| #135 | — | SKILL.md/hook says replace grep, contradicting the README's "complement"\[13\] |
| #134 | — | Stale PID file breaks resume\[13\] |
| #132 | 2026-02-01 | "Windows compatibility: os.setsid not available on Windows"\[13\] |

- **Windows PRs** (reported by the research subagent from list views; not opened individually):
  - #27 "Fix mgrep watch for Windows and add CI hooks tests" (2025-11-24, open)\[21\]
  - #110 "fix: add Windows compatibility to hook scripts" (2025-12-28, open)\[21\]
  - #137 "fix: pass project cwd to mgrep watch subprocess" (2026-03-12)\[21\]
  - #154 "one active watcher per directory tree across sessions" (2026-10-06)\[21\]
  - #83 "prevent multiple watchers on the same dir"\[21\]
- **No issue found on separators.** I found no issue specifically about Windows path separators or `metadata.path` on page 1 of open issues. Closed issues and PRs were not checked.

**INFERENCE.** Windows support is a second-class path in the agent-hook layer. Every Windows item above is still open, some for nearly a year. Your plan of not using agent installers or watchers is therefore the right one; the hook layer is where most of the open defects sit.

## 2. Native portability: can one private store serve Windows and Mac?

**What is verified.**
- **CLI scoping and output.** The CLI scopes searches to cwd or a path argument and prints relative paths (README).\[1\] It exposes `--store` but no metadata or filter flag (README).\[2\]
- **Upload metadata.** The Stores API accepts arbitrary file metadata at upload, e.g. `metadata={"category": ..., "tags": [...]}` via `stores.files.upload_and_poll` (https://www.mixedbread.com/docs/quickstart). \[3\] That metadata "propagates to every chunk produced from the file" (https://www.mixedbread.com/docs/concepts). \[17\]
- **Search filters.** `stores.search(query=..., store_identifiers=[...], top_k=..., filters={...})` accepts metadata filters (https://www.mixedbread.com/docs/stores/search). \[3\]\[22\]
- **Filter operators.** Operators include case-sensitive `eq` and `starts_with`. A negated prefix is expressed as `{"none":[{"key":"path","operator":"starts_with","value":"/tmp/"}]}` (metadata-filtering doc). Mixedbread's own example filters a `path` key with POSIX prefixes such as `/Users` and `/tmp/`.\[4\]
- **Your observation (user-reported fact).** The Mac CLI returned empty results on a store populated from Windows because "metadata.path starts_with normalized local cwd" did not match Windows absolute paths. The official SDK on Mac returned the correct source for one query.

**UNVERIFIED (could not read source).**
- Whether mgrep 0.1.x writes `metadata.path` as an absolute OS-native path or a POSIX-normalized one. A Windows path would look like «внутренний путь лаборатории», with or without separator conversion.
- Whether it lower-cases drive letters.
- Whether it sets `external_id`.

The research subagent's unverified recollection is: absolute path in `metadata.path`, a `starts_with` filter on the resolved cwd, a SHA-256 hash in metadata, and an `external_id` equal to the path. That fits your symptom but is **not** a source-verified finding. The files to read are `src/lib/store.ts` ("Core search and sync logic" per AGENTS.md), `src/lib/sync-helpers.ts`, `src/lib/utils.ts` and `src/commands/search.ts`, at the commit behind npm 0.1.13.\[23\]

**INFERENCE: why the native CLI cannot be made portable without a fork.**
- An absolute-path prefix filter cannot match across `C:\…` and `/Users/…` roots.
- No CLI flag injects `repo_id`/`revision` or replaces the filter.
- Getting a match would mean faking the cwd, which the brief forbids. Even if it were allowed, a Windows-style prefix cannot be produced on macOS.
- Result: **native mgrep CLI = insufficient. A minimal adapter on the official SDK is required.**

**Proposed portable schema (natively supported API mechanics only).** Field names are this report's proposal; the operators and metadata propagation are documented.

```json
// per-file metadata at upload (SDK stores.files.upload / upload_and_poll)
{
  "schema": "ci-fallback/1",
  "repo_id": "github.com/org/repo",          // stable, lowercase
  "revision": "<40-hex git commit SHA>",      // immutable snapshot id
  "rel_path": "src/pkg/module.py",            // POSIX '/', repo-root-relative, NFC, original case
  "rel_path_lc": "src/pkg/module.py",         // lower-cased copy for case-insensitive lookup
  "file_hash": "<git blob SHA-1 or SHA-256 of bytes>",
  "lang": "python",
  "uploader": "<host-id>"                      // ownership, never used for filtering answers
}
// external_id (if used): "<repo_id>@<revision>:<rel_path>"  — per Mixedbread Store Files docs, an existing external_id is overwritten by default; pass overwrite: false to get an error instead
```

```json
// query-time filter (SDK stores.search filters=...)
{"all":[
  {"key":"repo_id","operator":"eq","value":"github.com/org/repo"},
  {"key":"revision","operator":"eq","value":"<HEAD SHA of the asking clone>"},
  {"key":"rel_path","operator":"starts_with","value":"src/pkg/"}   // optional subtree
]}
```

**Edge-case handling under this schema:**

| Case | How the schema handles it | Status |
|---|---|---|
| Absolute roots | Never stored; each host computes `rel_path` from `git rev-parse --show-toplevel`. | INFERENCE: removes the root mismatch entirely |
| Separators | Normalize `\` to `/` before upload. Filters are plain string ops. | FACT for case-sensitivity of filters\[4\] |
| Case | `starts_with`/`eq` are case-sensitive (doc).\[4\] Keep the original case for readback and add `rel_path_lc` for case-insensitive filters; macOS APFS and NTFS are usually case-insensitive while git is case-sensitive. | FACT / INFERENCE |
| Unicode | macOS may hand back NFD file names. Normalize to NFC before upload and before filtering. | INFERENCE; untested against Mixedbread |
| Duplicate basenames | Full `rel_path` makes them unique; never key on basename. | — |
| Wrong clone or revision | `revision eq HEAD` returns nothing for an unindexed revision, which gives an explicit "not indexed" signal rather than stale hits. Wrong repo: `repo_id eq` excludes it. | — |

The adapter must treat an empty result plus a missing revision as **INDEX_MISS** and fall back to CodeIndexer. It must not report "no answer".

**CLI vs SDK/API comparison:**

| Capability | mgrep CLI 0.1.13 | Official SDK / Stores API |
|---|---|---|
| Custom metadata on upload | Not documented | Yes (`metadata=`)\[3\] |
| Custom query filter | Not documented (implicit cwd scope) | Yes (`filters=`) |
| Store selection | `--store`, `MXBAI_STORE` | `store_identifiers=[...]` |
| Rerank control | Default on; `--no-rerank` | `search_options={"rerank":...}`\[2\]\[24\] |
| Top-k | `-m` (default 10)\[25\] | `top_k`\[2\] |
| Shell-usable by all 3 clients | Yes | Via a small script you own |
| Cross-OS shared store | **No** (observed failure) | Yes, by schema design (to be canaried) |

## 3. Freshness and update semantics

**Verified.**
- **README sync behaviour.** `watch` "performs an initial sync, respects .gitignore, then keeps the Mixedbread store updated as files change".\[1\] `.mgrepignore` at the repo root uses gitignore syntax.\[26\] `-s/--sync` syncs once before a search without a permanent watch. `--max-file-count` "limit[s] sync to 5000 changed files or fewer", with a default of 1,000 per operation. Max file size defaults to 1 MB.\[2\]
- **Store expiry.** Stores can auto-expire via `expires_after` anchored on `last_active_at` (https://www.mixedbread.com/docs/stores/stores). \[10\]
- **The separate official `mxbai` CLI** (https://mixedbread.com/cli/sync) offers `mxbai store sync <store> <patterns> --metadata '<json>' --from-git <ref>` with "Git-based" or "Hash-based (accurate): Compares file hashes with stored metadata" change detection.\[27\] This is a second native route for attaching `repo_id`/`revision` metadata. However, it is a **new install** (`@mixedbread/cli`), which the no-duplicate-installs constraint forbids, and its path and delete semantics were not read.

**UNVERIFIED (mgrep source not read).**
- Whether `mgrep search -s` deletes remote files that are missing locally under the cwd prefix. If it does, two clones of one repo at different absolute roots would not delete each other's files under absolute-path keys, but under a shared relative key they would.
- How renames are represented: delete plus add, or overwrite.
- Symlink following.
- Whether hash comparison skips re-upload.

Mixedbread's Store Files docs (mixedbread.com/docs/stores/store-files) confirm that "By default, uploading a file with an existing external_id will overwrite the previous file", and that `overwrite: false` "Prevents overwriting and returns an error if the external_id already exists". Their example uses a relative ID, `external_id="path/to/my-file.pdf"`. Whether mgrep 0.1.13 itself sets `external_id` is still unverified.

**INFERENCE: adapter update semantics that avoid cross-clone damage.**
1. **Revision-immutable namespace.** Each upload set is keyed by `(repo_id, revision)` and never mutated afterwards. "Edit/rename/delete" means indexing a new revision. Old revisions are garbage-collected only by `files.list(metadata_filter={"all":[repo_id eq, revision eq OLD]})` followed by deletion, run by the single designated uploader host. This removes the risk of one clone deleting another clone's files.
2. **Upload ownership.** Exactly one host, preferably the always-on Windows host where the store was populated, uploads. The other hosts are query-only. Record this in the `uploader` field.
3. **Dedup.** Before uploading revision N+1, copy forward unchanged files by `file_hash`. Whether the API supports server-side copy is UNKNOWN; if it does not, re-upload and pay the index cost.
   - Cheaper alternative: index only `main`/release SHAs, and on non-indexed revisions answer through CodeIndexer.
4. **Allowlist, not denylist.** Enumerate via `git ls-files -z` filtered by an extension allowlist (`.py .js .ts .tsx .ps1 .psm1 .md`). This excludes untracked secrets (`.env`), build output and symlinks. Optionally also skip `git ls-files -s` mode `120000` entries, which are symlinks.
5. **Query-time validation.** Each hit must carry `revision == local HEAD`. Read back via `git show <revision>:<rel_path>` and check `git hash-object` against `file_hash`. On mismatch, discard the hit and log STALE.
6. **Lifecycle tests run only in disposable copies.** Use a throwaway store with `expires_after` of 1 day and a scratch clone.

## 4. Bounded fallback design

- **Order.** CodeIndexer is always called first.
- **Trigger (all must hold).**
  - (a) The query is classified as semantic or conceptual (no exact identifier, regex or path).
  - (b) CodeIndexer returns 0 hits, or its top hit score is below a precommitted threshold, or the top-3 hits span ≥3 unrelated files.
  - (c) The per-session call budget is not exhausted.
- **Never route to the fallback:** exact-symbol lookups, go-to-definition/navigation, or literal grep. Use ripgrep or CodeIndexer for those. The README itself says "use grep for exact matches".\[2\]
- **Limits (precommitted).**
  - `top_k=5`
  - rerank on only if the A/B shows a benefit; otherwise `--no-rerank` / `rerank: False` saves $3.50 per 1K\[5\]
  - snippet text truncated to ≤1,200 characters per hit and ≤4 hits returned
  - serialized output ≤6 KB including JSON wrapper and errors
  - timeout 8 s
  - 1 retry, only on 429/5xx and honouring `Retry-After` (https://www.mixedbread.com/api-reference/rate-limits)
  - ≤3 calls per agent session; ≤N calls per day per host (see section 6)
- **No-answer behaviour.** Return a structured `{"status":"NO_MATCH"|"INDEX_MISS"|"STALE"|"ERROR"}` with no text body, so the agent cannot treat an empty result as proof of absence.
- **RU/EN.** README says "Semantic, multilingual". The Mixedbread docs Overview (mixedbread.com/docs) claims "Multilingual by Default — Search across 100+ languages seamlessly. Query in English, find relevant content in German, Japanese, or any other language without translation layers", but does not name Russian (VENDOR CLAIM). Russian queries over English code identifiers are UNTESTED and must be a benchmark stratum.
- **Integration.** Expose a single shell command (e.g. `cifallback query --repo . "<q>"`) invokable from Claude Code, Codex and Cursor terminals. Do not run `mgrep install-*` (these start per-session watchers/MCP; issues #147/#149).\[13\] DeepWiki describes `mgrep mcp` as a stdio server that runs background sync, and its tools array as empty in one version (secondary).\[25\]\[28\] Exclude it.

## 5. Privacy

| Topic | Guarantee (FACT) | Unknown / risk |
|---|---|---|
| Store visibility | Private by default; `is_public` opt-in (stores doc)\[10\] | Ensure the adapter never passes `is_public`. Audit existing stores in the dashboard (not done here) |
| Org ACL | Organizations with roles (Admin manages resources/members; billing and ownership reserved for Owner) — https://www.mixedbread.com/docs/platform/organization \[29\] | Per-store ACL inside an org: not documented in what I read |
| Training | Paid tier: no training (Terms §3.2.1, §7.2)\[11\] | **Free/Starter tier: Terms allow training plus indefinite retention; Privacy Policy says no training. Contradiction (issue #145).** Which tier your org is on decides which clause applies; check separately. Terms §3.2.3: data collected under Free tier stays under Free-tier terms after upgrading\[11\]\[13\] |
| Retention / deletion | DPA: data returned or deleted at customer's choice after services; "self-service functionality … to delete"\[30\] | Backup purge timing (Scale has "automatic backups") UNKNOWN |
| Region / residency | Enterprise BYOB/BYOC exist (pricing) | Default region of processing: UNKNOWN\[5\] |
| Certifications | Site shows SOC and ISO/IEC 27001:2022 badges\[11\] | VENDOR CLAIM; reports not reviewed\[5\] |
| Client-side leak | — | Issue #148: hook writes the env (API keys) to `/tmp/mgrep-watch.log`. Mitigated by not using hooks\[13\] |

**Recommendation.** Treat any code uploaded while on the Starter/free tier as potentially trainable and indefinitely retained until Mixedbread resolves #145 in writing. Use the allowlist upload only.

## 6. Pricing and credit accounting (no live balance read here)

Current public rates (https://www.mixedbread.com/pricing, same rates on all plans):

| Item | Rate |
|---|---|
| Semantic search | $4.00 / 1K queries\[5\] |
| Semantic + rerank (mgrep default) | $7.50 / 1K\[5\] |
| Grep (exact/regex) | $0.10 / 1K\[5\] |
| Index, Fast | $1.50 / 1M content tokens (text ≈1.4 tokens/word)\[5\] |
| Index, High Quality | $3.00 / 1M\[5\] |
| Storage | $0.50 / 1M content tokens / month\[5\] |
| Bandwidth / egress | Included\[5\] |

**Scenario formulas (no invented actuals).**
- Let Q be fallback queries per month, T the content tokens of the allowlisted snapshot, R the number of revisions retained and U the tokens re-indexed per revision.
- Monthly cost ≈ Q×(0.004 or 0.0075) + (T + Σ U)×1.5e-6 + R_eff×T×0.5e-6, where R_eff is the average number of revisions stored.
- Example shape: Q=300 with rerank gives $2.25. T=2M with a one-off Fast index gives $3.00, and storage is $1.00/month per stored copy.
- Index churn and stored revisions, not queries, dominate cost. That is the main argument for indexing only main/release SHAs.

**Controls and failure modes.**
- Metrics lag up to 10 minutes and small overdrafts are allowed (FACT).\[5\] A runaway loop can therefore overshoot before the balance shows it.
- The changelog's "Spending Limits" auto-pause is a VENDOR CLAIM.\[14\] Its exact threshold behaviour and the error code returned when exhausted are UNKNOWN.
- Starter: "When the credits are used up, upgrade to Scale to keep going".\[5\] The failure mode at exhaustion was not documented in what I read.
- Mitigation: client-side counters (calls/day, uploaded tokens/day) that fail closed. Read `X-`/billing data from the Billing API (exists per third-party API profile; secondary).\[31\]
- A secondhand pointer says `watch.ts` references a "2,000,000 store tokens" monthly quota (third-party issue anomalyco/opencode #6265; may be stale).\[32\]
- Rate limits are stated at two levels. The pricing page (mixedbread.com/pricing) gives a plan query limit of 100 req/min on Starter and 2,400 req/min on Scale. The API Rate Limiting page (mixedbread.com/api-reference/rate-limits) gives per-operation limits of Read 1,200, List 600, Write 360, Update 480 and Delete 240 per minute, and says a 429 response "will include a Retry-After header". Budget against the lower plan-level figure.

## 7. Falsifiable benchmark protocol

- **Snapshot.**
  - Freeze one immutable allowlisted snapshot per repo and record `repo_id, revision, rel_path, file_hash` in a manifest.
  - Include actual Python, JS, TS and PowerShell files where present.
  - Upload once with the adapter; both arms use the same frozen CodeIndexer index at the same SHA.
- **Question set.** ≥60 questions, balanced across 5 strata × 2 languages (RU/EN): known-symbol, exact-string, semantic/conceptual, navigation (callers/defs), no-answer (absent feature).
  - Write each question's source oracle (file:line ranges) before the run and hide it from answerers.
  - The 40-answer run is excluded.
- **Arms.** A = CodeIndexer-first baseline; B = A + bounded fallback.
  - Counterbalanced order (ABBA per question block).
  - Same model, temperature and client per pairing.
  - Rotate across Claude Code, Codex and Cursor.
- **Independence.** Answerers see no oracle. A blinded judge, a separate model/session given only the question, the oracle and the anonymized answers, scores correct / partial / omission / false-claim.
- **Counting.**
  - Total tool context actually delivered, measured on the serialized bytes/tokens the client injected, with wrappers, errors and retries included.
  - Successful product calls, counted from adapter logs with HTTP 200 and a non-empty valid payload. A run with 0 successful calls in arm B is **void**, not a negative result.
  - Invocation rate and demonstrated benefit are reported separately.
- **Resources (per host, serialized trials, no stopping production services).**
  - Cold = first call after reboot or idle ≥30 min; warm = subsequent calls.
  - Wall time; process-tree peak/idle RSS (Windows: `Get-Process` tree/ETW; macOS: `ps`/`powermetrics`).
  - CPU-seconds; GPU/VRAM (`powermetrics` / `nvidia-smi` where present); disk I/O; network bytes.
  - Attributable cloud charge = per-call counter × list rate, reconciled with the dashboard after a ≥10-minute lag.
- **Repeats.** 3 repeats. Report the median, a bootstrap 95% CI, and a paired sign test on correctness.
- **Precommitted gates.**
  - **KEEP:** B correctness ≥ A + 5 pp on the semantic stratum (CI excludes 0); false-claim rate not higher on any stratum; no-answer accuracy ≥ A; delivered context per answered question ≤ A; fleet process-tree CPU-s + RAM-time ≤ A (no local watcher); cost ≤ $X/month as set by the owner; 0 cross-OS path or stale-revision errors.
  - **TRY (extend canary):** semantic gain between +1 and +5 pp, or a CI that includes 0 with a positive point estimate, and all safety gates met.
  - **PARK:** any false-claim increase, any STALE/INDEX_MISS reported as "no answer", any cross-clone deletion, net resource increase, or semantic gain ≤0.

## 8. Strongest case against, failure modes, canary and rollback

**Strongest argument against.** In this architecture, a cloud fallback cannot reduce the expensive local work. CodeIndexer still runs first on every query, so local RAM/CPU are unchanged or rise slightly from the adapter. The only possible saving is LLM context tokens, and the vendor's "2x fewer tokens" figure was measured against grep-only workflows,\[15\] not against an existing semantic local indexer.\[2\] Meanwhile you take on:
- uploading proprietary code under contradictory free-tier terms;
- a second index that can go stale;
- a client whose hook layer has open memory-leak and secret-logging bugs;
- an adapter you own because the CLI cannot do cross-OS stores.

The likely outcome is "invoked but not beneficial".

**Likely failure modes.**
- Empty result misread as absence.
- Revision drift (local uncommitted edits not indexed).
- NFD/case mismatches.
- Rerank cost doubling unnoticed.
- Overdraft during the 10-minute metric lag.
- Accidental `mgrep watch`/`install-*` spawning orphan watchers.
- Searching the default `mgrep` store instead of the scoped one.

**Minimal reversible canary.**
1. Use the existing org and the existing private store, or a new store named `cifallback-canary` with `expires_after` of 14 days, but only if creating a store is permitted. It does not create an account.
2. Upload one repo at one release SHA from the designated uploader host with the adapter schema.
3. Enable the shell command on one Mac and one Windows host for one week, with the call caps from section 4 and fail-closed counters.
4. Log every call (query hash, status, bytes delivered, latency, cost estimate).

**Rollback.** In order:
1. Remove the command from the clients' allowed tools.
2. Delete canary-revision files via a `files.list` metadata filter, or delete the canary store.
3. Revoke the adapter API key (https://www.mixedbread.com/docs/platform/api-keys).
4. Confirm no `mgrep watch`/`mcp` processes remain on any host.

Nothing on the CodeIndexer path changes, so rollback cannot degrade the baseline.

## Facts vs Vendor Claims vs Inference vs Unknown: consolidated ledger

- **FACTS:**
  - repo, license and version 0.1.13 (npm/jsDelivr);
  - CLI flags/env and the cwd-default scope;
  - no Cursor integration;
  - API metadata upload and filter operators, case-sensitive;
  - private-default stores, `expires_after`;
  - prices, the 10-minute lag and small overdrafts;
  - paid-tier no-training clause and the Terms vs Privacy free-tier conflict;
  - the open issues #132/#134/#135/#136/#138/#145/#147/#148/#149/#151;
  - your observed Mac CLI empty result vs SDK success;
  - API-level `external_id` overwrite-by-default with an `overwrite: false` opt-out (Store Files docs).
- **VENDOR CLAIMS:**
  - "2x fewer tokens";
  - "Supports text, images, PDFs, video, and audio across 100+ languages" (Mixedbread homepage);
  - "<80ms P50 search time in prod" (homepage); the changelog separately claims "sub 90ms latency without reranking and sub 120ms with reranking";
  - spending-limit auto-pause;
  - SOC/ISO badges;
  - "agents and teammates can query the same corpus without re-uploading",\[9\] which is contradicted cross-OS by your observation.\[2\]
- **INFERENCE:**
  - native CLI insufficient for cross-OS;
  - SDK adapter with a relative-path/revision schema is sufficient;
  - index churn dominates cost;
  - no fleet compute saving is expected while CodeIndexer stays first.
- **UNKNOWN/UNVERIFIED:**
  - mgrep's exact `metadata.path`, filter, hash, delete, rename and symlink code at the 0.1.13 commit;
  - whether mgrep itself sets `external_id`;
  - server-side copy/dedup;
  - default data region;
  - backup purge timing;
  - the exhausted-credit error code;
  - Russian-query retrieval quality on code;
  - `package.json` `engines`.

## Unresolved questions

1. Exact code in `src/lib/store.ts`, `sync-helpers.ts`, `utils.ts` and `commands/search.ts` at the 0.1.13 commit: path construction, separator handling, the `starts_with` filter, hash skip, and delete-missing behaviour.
2. Does `mgrep search -s` delete remote files under the cwd prefix that are absent locally?
3. Contents of open PRs #27/#110/#137, and whether any would change path semantics.
4. Which tier the org is on, and Mixedbread's written resolution of #145.
5. Spending-limit configuration, the error code when it is hit, and how overdraft is settled.
6. Region of storage and processing; backup retention after deletion.
7. Whether `external_id` upserts replace chunks atomically, which determines whether a query can observe stale and new versions together.

## Caveats

- I could not open the TypeScript source or the raw npm tarball under this task's fetch rules. All code-level semantics are therefore flagged UNVERIFIED rather than inferred as fact.
- DeepWiki and third-party blogs are secondary.
- npm metadata comes from a search snippet because npmjs.com blocked direct fetch.
- All URLs above were accessed 2026-11-01. No installs, purchases, uploads or account changes were made.

## Sources

1. [github.com](https://github.com/gmh5225/mgrep)
2. [GitHub - mixedbread-ai/mgrep: A calm, CLI-native way to semantically grep everything, like code, images, pdfs and more.](https://github.com/mixedbread-ai/mgrep)
3. [Quickstart - Mixedbread](https://www.mixedbread.com/docs/quickstart)
4. [Metadata Filtering](https://www.mixedbread.com/docs/stores/metadata-filtering)
5. [Pricing: plans and usage rates - Mixedbread](https://www.mixedbread.com/pricing)
6. [@mixedbread/mgrep - npm](https://www.npmjs.com/package/@mixedbread/mgrep)
7. [@mixedbread/mgrep CDN by jsDelivr - A CDN for npm and GitHub](https://www.jsdelivr.com/package/npm/@mixedbread/mgrep)
8. [mgrep/CONTRIBUTING.md at main · mixedbread-ai/mgrep · GitHub](https://github.com/mixedbread-ai/mgrep/blob/main/CONTRIBUTING.md)
9. [Mixedbread AI Launches mgrep for Natural Language Code Searc](https://www.toolmesh.ai/news/mixedbread-ai-mgrep-natural-language-code-search)
10. [Create Stores - Mixedbread](https://www.mixedbread.com/docs/stores/stores)
11. [Terms & Conditions - Mixedbread](https://www.mixedbread.com/pages/terms)
12. [Privacy Policy - Mixedbread](https://www.mixedbread.com/pages/privacy)
13. [Issues · mixedbread-ai/mgrep](https://github.com/mixedbread-ai/mgrep/issues)
14. [Changelog - Mixedbread](https://www.mixedbread.com/docs/changelog)
15. [FAQ](https://deepwiki.com/mixedbread-ai/mgrep/11-faq)
16. [mixedbread-ai/mgrep](https://deepwiki.com/mixedbread-ai/mgrep)
17. [Concepts - Mixedbread](https://www.mixedbread.com/docs/concepts)
18. [New:Introducing Socket Scanning for VS Code Marketplace Extensions.Learn more →](https://socket.dev/npm/package/pi-mgrep/overview/0.1.2)
19. [Installation](https://deepwiki.com/mixedbread-ai/mgrep/2.1-installation)
20. [mgrep Documentation](https://docsearch.algolia.com/mcp/docs/repo/mixedbread-ai/mgrep)
21. [Pull requests · mixedbread-ai/mgrep](https://github.com/mixedbread-ai/mgrep/pulls)
22. [Search - Mixedbread](https://www.mixedbread.com/docs/stores/search)
23. [mgrep/AGENTS.md at main · mixedbread-ai/mgrep · GitHub](https://github.com/mixedbread-ai/mgrep/blob/main/AGENTS.md)
24. [Reranking - Mixedbread](https://www.mixedbread.com/docs/stores/search/rerank)
25. [OpenCode Integration](https://deepwiki.com/mixedbread-ai/mgrep/6.3-opencode-integration)
26. [stars/stars/mixedbread-ai/mgrep.md at master · DavidWells/stars · GitHub](https://github.com/DavidWells/stars/blob/master/stars/mixedbread-ai/mgrep.md)
27. [mixedbread.com](https://mixedbread.com/cli/sync)
28. [mcp Command (MCP Server)](<https://deepwiki.com/mixedbread-ai/mgrep/4.6-mcp-command-(mcp-server)>)
29. [Organization - Mixedbread](https://www.mixedbread.com/docs/platform/organization)
30. [Data Processing Addendum - Mixedbread](https://www.mixedbread.com/pages/dpa)
31. [GitHub - api-evangelist/mixedbread-ai: API Evangelist profile of Mixedbread - embeddings and reranking API · GitHub](https://github.com/api-evangelist/mixedbread-ai)
32. [Feature Request: Integrate mgrep for Semantic Code Search · Issue #6265 · anomalyco/opencode · GitHub](https://github.com/anomalyco/opencode/issues/6265)
