# DR26-11-01-HUB-06-1441 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll research this thoroughly using live web sources. Let me start by mapping the topic with broad searches.Let me extract the remaining search results from the saved file.Primary sources identified. Now let me open the key pages — the GitHub repo README, releases, npm package page, and pricing.Rich primary data. Now let me verify npm publish dates independently via the registry API, and inspect the repo structure.Let me retry with regex extraction for the npm time data and inspect the GitHub API response.npm registry verified: `latest = 0.1.13`, published 2026-04-25 (from the registry's internal timestamp; gitHead `5c1ba62` matches the GitHub release). Now let me map the repo structure.GitHub API is rate-limited. Let me extract the repo structure from the HTML page and try jsDelivr's API.Full repo structure mapped. Now let me read the core source files that determine path handling and sync semantics.This is a crucial finding — `search.ts` hardcodes a `path starts_with` filter on the absolute local cwd. Let me read `utils.ts` (upload/sync/hashing) and `store.ts` (Store SDK wrapper).The core cross-OS finding is confirmed by source. Let me now read `file.ts` (ignore patterns/symlinks), the rest of `watch.ts`, `watch_mcp.ts`, and `context.ts`.Now let me examine `git.ts`, `index.ts` (global options), the agent skill file, and the session hook script.Critical findings on the agent integrations. Now let me check `config.ts`, `install/codex.ts`, and `switch-org.ts`.Now let me get the current pricing page and the Mixedbread Search docs (filters, store model).Pricing confirmed. Now let me find the metadata filtering docs (operator list), upload/file docs, and check GitHub issues for path/sync problems.The metadata filtering API is fully documented. Let me get the complete issue list from the GitHub search API.Let me pull the details of the most decision-relevant issues.Core API is rate-limited. Let me open the HTML pages for these issues instead.Two major documented issues captured. Let me pull a few more key ones: #47 (deleted files persist), #117 (quota accounting mismatch), #132 (Windows hooks), #67 (uploads 0).#47 (deleted-files persistence) is closed; the Windows hook crash (#132) and several others remain open. Now let me check store privacy defaults and security/data-handling docs.Privacy policy and store docs captured. Let me extract the docs index for upload/files/billing pages, and verify the ToS training clause.ToS contradiction verified from primary sources. Now let me open the Security, Billing, and Limits docs pages.Now let me check the file upload docs (external_id semantics), Node support issue #92, and the mgrep.dev marketing page.Let me check the repo's guides directory and TESTING.md for any official multi-machine guidance.Research is nearly complete. One last check: the API keys docs (scopes, org context) for the shared-service account design.All research is complete. Here is the full report.

---

# DR26-11-01-HUB-06-1441: Mixedbread mgrep / Mixedbread Search — Portable Private-Cloud Companion for CodeIndexer

**Research date:** 2026-11-01 (all pages fetched live on this date). **Scope:** public primary sources only — npm registry, GitHub repo source/issues/releases, mixedbread.com docs/terms/privacy/pricing, mgrep.dev. No private systems, accounts, or installs were accessed.

---

## Question

Can `@mixedbread/mgrep` (v0.1.13) serve as a **bounded, on-demand semantic cloud fallback** to an existing CodeIndexer-first workflow shared by Claude Code, Codex and Cursor on Windows + Intel Mac + Apple Silicon Mac — reducing **total fleet RAM/CPU/GPU work** while preserving accuracy, freshness, privacy and cost transparency — given one existing org, existing private stores, and no new accounts/stores/installs?

---

## Executive Summary (ranked)

1. **The cross-OS failure you observed is by design, not a bug.** mgrep hardcodes a server-side filter `metadata.path starts_with <absolute local cwd>` on every search ([`src/commands/search.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/commands/search.ts), v0.1.13-era code on `main`). Uploads key on absolute local paths (`external_id` = absolute path; metadata = `{path, hash, mtime}`) ([`src/lib/utils.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/utils.ts)). **The unmodified CLI cannot consume a store populated on a different OS — or even a different local path — ever.** Your Mac-empty-result is fully explained by source code.
2. **A one-store cross-OS schema is natively supported by the Mixedbread API but not by the mgrep CLI.** Official docs explicitly recommend `external_id` = *relative* path "to mirror your repository's structure" and support arbitrary metadata (≤256 keys/file, filterable via `eq/in/starts_with/regex/…`) ([Create Files](https://www.mixedbread.com/docs/stores/store-files), [Metadata Filtering](https://www.mixedbread.com/docs/stores/metadata-filtering), [Limits](https://www.mixedbread.com/docs/platform/limits)). So ONE private store CAN hold `repo_id + revision + relative_path + file_hash` and be consumed on Windows/Mac — but only via a **minimal SDK adapter**, not via the CLI flags. The CLI has no metadata-override or filter-override flags, no repo/revision concept.
3. **The product's default agent deployment actively fights your fleet-RAM goal.** Shipped integrations spawn a detached per-session background watcher (`mgrep watch`) and inject a "MANDATORY: replaces ALL built-in search tools" skill ([`mgrep_watch.py`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/plugins/mgrep/hooks/mgrep_watch.py), [`SKILL.md`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/plugins/mgrep/skills/mgrep/SKILL.md)). A documented leak produced **606 orphaned watchers, ~8 GB RAM, swap exhaustion, load >900** (issue [#149](https://github.com/mixedbread-ai/mgrep/issues/149), open). Do **not** run `mgrep install-claude-code` / `install-codex`; use the plain shell CLI only.
4. **Version state verified:** installed 0.1.13 = npm `latest` = last GitHub release (v0.1.13, 2026-04-25, commit `5c1ba62`), independently confirmed via the [npm registry](https://registry.npmjs.org/@mixedbread/mgrep) (`dist-tags.latest: 0.1.13`; publish timestamp 2026-04-25; gitHead `5c1ba62…`). No release since. CLI is Apache-2.0; the service it talks to is proprietary cloud.
5. **Privacy is contradictory at the legal-document level for the free tier.** ToS §3.1.1 says free-tier Usage Data (queries, inputs) **may be used for model training**; Privacy Policy §2.3 and the security page say free-tier data is **never** used for training ([Terms](https://www.mixedbread.com/pages/terms), [Privacy Policy](https://www.mixedbread.com/pages/privacy), [Security](https://www.mixedbread.com/docs/production/security)). Open issue [#145](https://github.com/mixedbread-ai/mgrep/issues/145) documents the contradiction; it was unresolved as of its last activity (2026-06-12 update list). Paid-tier terms are unambiguous (no training).
6. **Pricing (current, fetched 2026-11-01):** semantic search $4/1K queries (+$3.50 with rerank → $7.50; rerank is mgrep's default); indexing $1.50/1M content tokens (fast mode, mgrep default); storage $0.50/1M tokens/month; per-unit rates identical across plans ([Pricing](https://www.mixedbread.com/pricing)). Accounting lag ≤10 min and "small overdrafts" are documented behavior ([Billing](https://www.mixedbread.com/docs/platform/billing)); no hard spend cap is documented anywhere.
7. **Recommendation: TRY (not KEEP, not PARK)** — bounded, adapter-mediated, canary-first, with the CLI used directly by all three clients via ordinary shell, search-scoped API key, one existing private store, precommitted KEEP/TRY/PARK thresholds (§7). The strongest argument against adoption (§8) is that the bounded, multi-OS, no-watcher deployment you need is one the vendor neither ships nor tests — you would be maintaining an unsupported consumption path against a fast-moving 0.x service.

---

## Methodology

- **Search angles:** package identity & release history; CLI source code reading (search/sync/upload/hook paths); official hosted-API docs (stores, files, metadata filtering, QA, API keys, billing, limits, security); legal documents (ToS, Privacy Policy); GitHub issue tracker (all 61 issues enumerated, 60 titles reviewed, ~20 opened); vendor marketing claims (mgrep.dev, README).
- **Primary sources preferred:** npm registry JSON, raw source files at `main` (as of 2026-11-01; last tag v0.1.13 = 2026-04-25, so `main` ≈ 0.1.13), GitHub releases/issues, mixedbread.com official docs fetched live.
- **Limitations:** no runtime testing performed (research-only, per instruction); GitHub core REST API was rate-limited (HTML mirrors used instead); some GitHub release dates lack an explicit year (inferred from ordering — labeled); docs pages are undated (fetch date given); internals of the hosted service (retrieval model identity, QA pricing) are not publicly documented.
- **Facts vs vendor claims vs inference vs unknown** are labeled inline throughout.

---

## Findings

### 1. Identity, releases, licenses, runtime/OS support

**[Fact] Identity.** `@mixedbread/mgrep` is an npm CLI by Mixedbread (`mixedbread-ai/mgrep`), "a calm, CLI-native way to semantically grep everything," powered by the hosted **Mixedbread Search** service ([GitHub](https://github.com/mixedbread-ai/mgrep), [npm](https://www.npmjs.com/package/@mixedbread/mgrep)). The CLI talks to `https://api.mixedbread.com` hardcoded, with `NODE_ENV=development` switching to `localhost:8000` for internal development only ([`src/lib/context.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/context.ts)). It is **online-only**; offline use is an open, unimplemented feature request ([#31](https://github.com/mixedbread-ai/mgrep/issues/31), [#97](https://github.com/mixedbread-ai/mgrep/issues/97)).

**[Fact] Version/release state (independently verified 2026-11-01).** [npm registry](https://registry.npmjs.org/@mixedbread/mgrep): `dist-tags.latest = 0.1.13`, published 2026-04-25 (registry internal timestamp 1777138298668 ms), gitHead `5c1ba628…`, published via GitHub Actions OIDC trusted publisher, built with Node 24.14.1. [GitHub releases](https://github.com/mixedbread-ai/mgrep/releases): v0.1.13 = 2026-04-25 (commit `5c1ba62`), v0.1.12 = 2026-04-14 (expired-token auth fix), v0.1.11 = 2026-04-08 (skip ignored dirs when watching; tolerate stale PID files), v0.1.10 = 2026-01-23 (agentic search added), v0.1.9 = 2026-01-21 (mtime skip optimization; quota error changes), v0.1.8 = 2025-12-21 **[inferred year]** (web search added). Your installed 0.1.13 = latest; your Nov 1 npm read is confirmed.

**[Fact] Licenses.** CLI: **Apache-2.0** (README, `package.json`, npm). Hosted service: Mixedbread's commercial [Terms](https://www.mixedbread.com/pages/terms) and [Privacy Policy](https://www.mixedbread.com/pages/privacy). Open-source CLI + proprietary cloud backend: the Apache license covers ~5k lines of TypeScript, not the retrieval stack.

**[Fact] Runtime.** Pure-JS/TS Node CLI (`"type": "module"`); dependencies are pure JS (xxhash via WASM); **no native binaries, no platform-specific builds** — [package.json](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/package.json). It depends on the official SDK `@mixedbread/sdk ^0.43.0` and the MCP SDK.

**[Fact] Node support is unguarded:** no `engines` field in package.json; issue [#92](https://github.com/mixedbread-ai/mgrep/issues/92) (open) reports `ERR_REQUIRE_ESM` crash on Node 22.2.0 because the compiled output requires the ESM-only `chalk`; the maintainers publish with Node 24.14.1. **[Inference]** Use Node ≥ 24 (or a late 22.x with `require(esm)` support); nothing enforces this for you.

**[Fact] OS support.** Core CLI paths (search/watch/upload) use only cross-platform Node APIs — consistent with your successful Windows smoke test. The **agent plugin hooks are Python and POSIX-only**: `mgrep_watch.py` uses `os.setsid`, crashing on Windows ([#132](https://github.com/mixedbread-ai/mgrep/issues/132), open; also [#107](https://github.com/mixedbread-ai/mgrep/issues/107), [#111](https://github.com/mixedbread-ai/mgrep/issues/111), [#23](https://github.com/mixedbread-ai/mgrep/issues/23)). **[Fact]** No `install-cursor` command exists; Cursor support is "coming soon" (README). **[Inference]** Intel/Apple Silicon: no native modules → arch-independent; one early M1 install issue exists ([#58](https://github.com/mixedbread-ai/mgrep/issues/58), open, from an early version).

**[Fact] Live issues relevant to paths/sync/staleness** (61 total; 60 reviewed): watcher orphans ([#149](https://github.com/mixedbread-ai/mgrep/issues/149), open), env-var/API-key dump to world-readable `/tmp/mgrep-watch.log` by the session hook ([#148](https://github.com/mixedbread-ai/mgrep/issues/148), open), watch indexing the home folder ([#94](https://github.com/mixedbread-ai/mgrep/issues/94), open), deleted files persisting in results ([#47](https://github.com/mixedbread-ai/mgrep/issues/47), **closed**), uploads silently 0 on a free account ([#67](https://github.com/mixedbread-ai/mgrep/issues/67)), CLI quota error vs dashboard balance mismatch ([#117](https://github.com/mixedbread-ai/mgrep/issues/117), open), deleted-store re-index failure ([#76](https://github.com/mixedbread-ai/mgrep/issues/76), open), symlink indexing ([#80](https://github.com/mixedbread-ai/mgrep/issues/80), open), no `--json` output ([#46](https://github.com/mixedbread-ai/mgrep/issues/46), open), staleness guard requests ([#40](https://github.com/mixedbread-ai/mgrep/issues/40), [#34](https://github.com/mixedbread-ai/mgrep/issues/34), open). **[Fact]** No issue or PR in the tracker addresses cross-machine store sharing or relative-path mode (reviewed 2026-11-01).

### 2. Native portability — the core question

**Answer: No, the unmodified CLI cannot share one store across Windows and Mac. Yes, one store with canonical repo-relative paths + repo_id + revision is achievable without a fork — via a minimal adapter against the same official SDK/API mgrep itself uses.**

**[Fact] What mgrep writes on upload** ([`src/lib/utils.ts`, `uploadFile`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/utils.ts)):
```typescript
const options = {
  external_id: filePath,        // ABSOLUTE local path
  overwrite: true,
  metadata: { path: filePath,   // ABSOLUTE local path again
              hash: "xxh64:<hex>", mtime: stat.mtimeMs },
};
```
**[Fact] What mgrep filters on search** ([`src/commands/search.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/commands/search.ts)):
```typescript
const search_path = normalize(join(process.cwd(), exec_path ?? ""));
const filters = { all: [{ key: "path", operator: "starts_with",
                           value: search_path }] };
```
The filter is hardcoded; there is no flag, env var, or config key to change the filter key/operator/value (config schema is only `maxFileSize`, `maxFileCount`, `autoUpdate` — [`src/lib/config.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/config.ts)). Output display merely strips the local `pwd` prefix client-side (`chunk.metadata.path.replace(pwd, "")`).

Consequences **[Fact, from code]**:
- A store populated at «внутренний путь лаборатории» (Windows) returns **zero chunks** for a query run from `/Users/me/repo` (Mac): every stored `metadata.path` fails `starts_with` against the Mac cwd. Your observed behavior is exactly this mechanism.
- Even two clones on the *same* machine at different roots are invisible to each other, and **two Windows machines with different usernames cannot share a view** either. Only byte-identical absolute path prefixes match.
- `starts_with` is **case-sensitive** ([Metadata Filtering](https://www.mixedbread.com/docs/stores/metadata-filtering)) → a Windows cwd launched with different drive-letter/path casing can silently return zero results. **[Inference]** flag for testing.

**[Fact] The official API supports exactly the schema you want** ([Create Files](https://www.mixedbread.com/docs/stores/store-files)):
- "`external_id` … makes it easy to replace or update the same file by using the same external ID. The ID can be any string value, **though we recommend using a relative file path to mirror your repository's structure**."
- `overwrite: true` (default) replaces the file with the same `external_id`; `false` errors instead.
- Metadata is structured (string/number/boolean/date/array), inherited by every chunk, filterable at search time.
- Limits ([Limits](https://www.mixedbread.com/docs/platform/limits)): external ID ≤ 256 chars (repo-relative paths fit easily), ≤ 256 metadata keys/file, value ≤ 8 KiB, total ≤ 128 KiB, nesting ≤ 5 — `repo_id`, `revision`, `relative_path`, `file_hash` fit trivially.
- Search filters support `eq, not_eq, gt, gte, lt, lte, in, not_in, contains, like, not_like, regex, starts_with` with `all/any/none` groups and nesting, plus `file_ids` and system fields `file_id`/`chunk_index` ([Metadata Filtering](https://www.mixedbread.com/docs/stores/metadata-filtering), [Search](https://www.mixedbread.com/docs/stores/search)).

**[Fact] CLI vs SDK/API comparison:**

| Capability | mgrep CLI 0.1.13 | Official SDK/API |
|---|---|---|
| Path scoping | Hardcoded `metadata.path starts_with <abs cwd>` | Arbitrary metadata filters incl. `eq`/`starts_with` on your keys |
| external_id | Absolute local path | Any string; **docs recommend relative path** |
| Metadata written | `{path, hash, mtime}` only | Any schema within limits |
| repo_id/revision | None | First-class via your metadata + filters |
| Rerank | On by default (`--no-rerank`, `MGREP_RERANK=0`) | `search_options.rerank` |
| top_k | `-m` (default 10) | `top_k` |
| Answer mode | `-a` (QA endpoint, `<cite i="n">` citations) | QA with `instructions`, `qa_options` |
| Agentic | `--agentic` (Toast 1, extra cost) | `agentic`, `strict_top_k`, `instructions` |
| Machine-readable output | None (plain text; `--json` is open request [#46](https://github.com/mixedbread-ai/mgrep/issues/46)) | Structured JSON |
| Auth | OAuth device login or `MXBAI_API_KEY` | API key with granular scopes |

**Minimal adapter design [Inference, grounded in the documented API — every element individually supported, the composition unverified]:**
- Upload: `external_id = "<repo_id>:<revision>:<relative_path>"` (or relative path + metadata), `metadata = { repo_id, revision, relative_path, file_hash }`, `overwrite: true`.
- Search: `filters = { all: [ {key:"repo_id",operator:"eq",value:…}, {key:"revision",operator:"eq",value:…} ] }`, `top_k ≤ 10`, `rerank` configurable.
- Delete: list files with `metadata_filter` on `repo_id` + prefix of `relative_path`, delete by `external_id` — mirroring mgrep's own scoped-deletion logic (below).
- mgrep CLI files (absolute-path `external_id`s) can coexist in the same store under a disjoint namespace, but this **doubles ingest/storage cost**; prefer that the adapter own **all** uploads for cross-OS repos.
- **No fake cwd, no dropped repo/revision scope** — both are explicit filter conditions, enforced server-side.
- **[Unknown]**: exact SDK method names in the current `@mixedbread/sdk` for scoped file listing (mgrep's own `MixedbreadStore.listFiles` uses `stores.files.list` with `metadata_filter` — [`src/lib/store.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/store.ts) — so the pattern exists in the SDK).

**Duplicate basenames [Fact]:** non-issue — identity is the full relative path/external_id, not the basename. **Unicode [Unknown]:** strings pass through unmodified in code; no documented constraints; untested publicly — include in lifecycle tests. **Separators/case [Fact/Inference]:** mgrep stores backslash paths on Windows and `/` on POSIX; the adapter avoids this by using repo-relative POSIX-style paths and never matching on absolute paths.

### 3. Freshness semantics

**[Fact] One-shot sync exists — as a search flag, not a watch flag.** `mgrep "query" -s` (`--sync`) runs a full incremental sync (upload changed + delete removed, scoped to the search path), polls the store until `pending`+`in_progress` = 0, then searches ([`search.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/commands/search.ts)). `mgrep watch` has **no `--once` flag** — the `watch --once` shown in [TESTING.md](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/TESTING.md) references a nonexistent flag (doc error). For your bounded design, `-s` is the one-shot mechanism; `watch` is the permanent-watcher mechanism you must avoid.

**[Fact] Hashes/dedup** ([`utils.ts`, `initialSync`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/utils.ts)):
- mtime "bloom filter": `stat.mtimeMs <= stored.mtime` → skip without reading.
- If mtime changed: read file, compare **xxhash64** (`xxh64:` prefix; legacy SHA-256 hashes still honored) → re-upload only if content hash differs. Rename/mtime-touch without content change = no re-upload, no re-ingest cost.
- Sync deletes stored files under the current root that no longer exist locally.

**[Fact] Create/edit/rename/delete:**
- Create/edit → upload with `overwrite: true` on same `external_id` (server replaces file; status `pending → in_progress → completed` — [Create Files](https://www.mixedbread.com/docs/stores/store-files)).
- Delete → `store.deleteFile(external_id)`; deletion is **scoped by absolute-path subtree**: `filesToDelete = stored paths where isSubpath(repoRoot, path) && not present locally`. **A sync from clone A never deletes clone B's files** (B's paths are not under A's root). Your "avoid deleting another clone's files" requirement is natively satisfied — per-absolute-root — and the adapter must replicate it with `repo_id`-scoped prefix deletion instead.
- Rename = delete old `external_id` + upload new (content re-ingested once at the new ID).
- Historical deletion-persistence bug ([#47](https://github.com/mixedbread-ai/mgrep/issues/47)) is **closed**; re-sync now removes deleted files.

**[Fact] Ignored files / secrets / symlinks** ([`file.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/file.ts), [`git.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/git.ts)):
- Git repos: enumeration = `git ls-files` (tracked + untracked-not-ignored) → `.gitignore` respected; `.mgrepignore` (gitignore syntax) honored; hidden dotfiles (`.env`, `.git`) **always** skipped; default ignore globs: `*.lock *.bin *.ipynb *.pyc *.safetensors *.sqlite *.pt`.
- **[Fact]** A non-hidden, non-gitignored secret file (e.g., `secrets.yaml`) **WILL be uploaded**. There is **no allowlist mechanism** — only deny-lists. Your "upload allowlist" requirement therefore needs an adapter/wrapper-level allowlist; mgrep natively cannot express one.
- Symlinks **[Fact+Inference]**: in git repos, tracked symlinks are listed and uploaded **following the link** (content of target); in non-git trees, symlinks are skipped (Dirent `isFile()` is false). Inconsistent semantics; symlink support is an open issue ([#80](https://github.com/mixedbread-ai/mgrep/issues/80)).
- **[Fact]** Home-directory guard: sync/watch refuse to run at/above `$HOME` (`isAtOrAboveHomeDirectory`) — though a user report of home-folder indexing exists ([#94](https://github.com/mixedbread-ai/mgrep/issues/94), open).

**[Fact] Multiple clones/revisions and upload ownership:** stores belong to the **organization**; every authenticated member of the org shares the same store namespace; there is no per-user partition, and mgrep has **no revision awareness**: the store reflects whatever the last sync from a given absolute path wrote. Checking out a different revision at the same root and re-running sync updates the store in place — the store represents "current working tree," not history.

**[Fact] Query-time revision validation: none.** Without `-s`, search **never checks freshness** — it will silently return stale chunks (staleness-guard feature requests [#40](https://github.com/mixedbread-ai/mgrep/issues/40), [#34](https://github.com/mixedbread-ai/mgrep/issues/34) are open). With `-s`, freshness is enforced up to sync completion, then searched. **[Fact]** Exact readback: results carry relative path + line ranges (text) or page numbers (PDF) and scores; verification means opening the local file at those lines. **[Inference]** Your benchmark's "wrong clone/revision" test is the right guard: query for a symbol added in revision Y while the working tree is on X — mgrep cannot detect this; the adapter's `revision` filter is the only protection.

**[Fact] Bounded-sync safety valve:** if the number of files to change exceeds `maxFileCount` (default 1000), sync **throws and syncs nothing** (`MaxFileCountExceededError`) — a useful anti-runaway control; tune via `--max-file-count` / `MGREP_MAX_FILE_COUNT`.

### 4. Bounded fallback design (CodeIndexer-first)

**[Fact] What the shipped integrations do — avoid them:**
- `mgrep install-codex` runs `codex mcp add mgrep mgrep mcp` (registers a **per-session stdio MCP server**) and appends a skill to `~/.codex/AGENTS.md` telling the agent to "not use grep" ([`src/install/codex.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/install/codex.ts)). The MCP server itself exposes **zero tools** and merely starts a background `mgrep watch` ([`src/commands/watch_mcp.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/commands/watch_mcp.ts)) — it is a watcher bootstrap, not an MCP tool surface.
- `mgrep install-claude-code` installs a SessionStart hook that **spawns a detached `mgrep watch` per session** and injects "You MUST use the mgrep skill for ALL searches… NEVER use built-in WebSearch, Grep" context ([`mgrep_watch.py`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/plugins/mgrep/hooks/mgrep_watch.py), [`SKILL.md`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/plugins/mgrep/skills/mgrep/SKILL.md)). Documented consequences: orphaned watchers exhausting RAM/swap ([#149](https://github.com/mixedbread-ai/mgrep/issues/149)), full `os.environ` (incl. API keys) logged to world-readable `/tmp/mgrep-watch.log` ([#148](https://github.com/mixedbread-ai/mgrep/issues/148)), Windows hook crashes ([#132](https://github.com/mixedbread-ai/mgrep/issues/132)), stale-PID kill bugs ([#151](https://github.com/mixedbread-ai/mgrep/issues/151), [#134](https://github.com/mixedbread-ai/mgrep/issues/134)). Open issue [#135](https://github.com/mixedbread-ai/mgrep/issues/135) notes the shipped skill contradicts the README's "complement, not replace" positioning.
- No Cursor installer exists.

**[Fact] The ordinary-shell path satisfies your constraints:** `mgrep` is a plain CLI any of the three agents can invoke via its existing shell capability — identical command, no per-session MCP, no auto-install watchers, no client config changes. This is also the only mode that works on Windows (hooks are broken there anyway).

**Recommended bounded contract [Inference — design]:**
- **Trigger:** CodeIndexer first; invoke the cloud fallback only on explicit semantic failure/ambiguity (CodeIndexer returned no/low-confidence hits, or the question is intent-based rather than symbol-based). Cap: ≤2 fallback calls per task/session.
- **Command shape:** `timeout 20 mgrep -m 5 --no-rerank "query"` (or rerank on — see §6 cost table), **no `-a`** (QA = extra generation cost, latency, and unpublished pricing), **no `--agentic`** (multiplies searches + Toast 1 LLM tokens), **no `--web`**, **no `-s` in the query hot path** (sync only pre-benchmark or on an explicit refresh command, because `-s` blocks on indexing completion).
- **Timeout/retry:** mgrep has **no timeout or retry flags** — enforce in the wrapper (e.g., GNU `timeout`, PowerShell job timeout); count every failed/timed-out call against the call budget.
- **Exact-symbol/navigation/no-answer:** route to grep/CodeIndexer — mgrep's CLI has no regex/grep mode (the store grep endpoint is API-only, priced separately) and "politely ignores" unsupported grep-style switches (README); semantic-only retrieval is expected to be *worse* than grep for exact symbols. No-answer behavior: mgrep always returns top-k chunks with scores (never "no answer"); a low-score/empty result must be interpreted by the caller.
- **RU/EN:** [Vendor claim] multilingual semantic retrieval (README; marketing cites 100+ languages; multilingual evals like MIRACL-Vision exist at the platform level — [evals index](https://www.mixedbread.com/llms.txt)). [Unknown] Russian-language code-query quality specifically is unpublished — include RU queries in the canary benchmark.
- **Output:** plain text `./relative/path:12-34 (95.2% match)` lines — no JSON yet ([#46](https://github.com/mixedbread-ai/mgrep/issues/46)); agents parse it easily, but your "count ALL delivered context after serialization" accounting must capture exactly these bytes plus wrapper overhead.

### 5. Privacy

**[Fact] Store visibility.** Stores are **private by default** ("Only your organization can access. You pay all costs."); `is_public=True` lets **anyone with any API key** search the store, paying for their own searches ([Create Stores](https://www.mixedbread.com/docs/stores/stores)). mgrep's `store.create` passes only `name`+`description` → **mgrep stores are private unless deliberately changed** ([`watch.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/commands/watch.ts)). Store names are lowercase/hyphen/period, unique per org.

**[Fact] Org/ACL.** API keys belong to the organization and inherit its stores/billing; keys can be scoped per-store and per-tag with read/write/delete/search scopes; "search store" covers search, agentic, grep, QA, facets ([API Keys](https://www.mixedbread.com/docs/platform/api-keys)). Caution: an **empty scope list = full access**. For the canary, create a **search-only key scoped to the one canary store**.

**[Fact] Training — three documents, two positions:**
- ToS §3.1.1 (Free Tier): Usage Data "including… queries and any other input" is "stored in its native form and **may be used for… model training**, analytics, service improvement, and research" ([Terms](https://www.mixedbread.com/pages/terms)).
- Privacy Policy §2.3: "We do not use any data from Free Tier users for model training." Data retained indefinitely ([Privacy Policy](https://www.mixedbread.com/pages/privacy)).
- Security page: "Your content and usage are **never** used to train models, for any user on any plan" ([Security](https://www.mixedbread.com/docs/production/security)).
- Open issue [#145](https://github.com/mixedbread-ai/mgrep/issues/145) documents the ToS/PP contradiction; unresolved as of its last activity. **Paid tier (§3.2.1): data "shall not be used for model training"** — unambiguous; upgrading does **not** retroactively change free-tier data rules (§3.2.3).

**[Fact] Retention/deletion.** Content: "Delete a store or your account and the associated content is removed" ([Security](https://www.mixedbread.com/docs/production/security)). Usage data: free-tier retained indefinitely, "we do not guarantee the deletion of historical data upon request" (PP §6). Store-level `expires_after` policies exist (default: never) ([Create Stores](https://www.mixedbread.com/docs/stores/stores)).

**[Fact] Region/compliance.** "All processing and Mixedbread-managed storage currently happen in the **United States**. EU data residency is coming soon" ([Security](https://www.mixedbread.com/docs/production/security)); the Privacy Policy (§8–9) confirms US storage, some EU analytics providers, and US-governed policy. SOC 2 Type II (Security criterion), ISO 27001, GDPR DPA with EU/UK/Swiss SCCs available on request. Encryption: TLS 1.2+ in transit, AES-256 at rest. **[Unknown]** whether a legally signed DPA is in place for your org — separate from public claims.

**[Fact] Local-side leakage risk:** the session hook logs **all environment variables** to world-readable `/tmp/mgrep-watch.log` ([#148](https://github.com/mixedbread-ai/mgrep/issues/148), open) — eliminated by not installing the hooks. Your "never publicize private stores" requirement: keep `is_public` unset, don't share the store name publicly, use scoped keys.

### 6. Current public pricing & credit accounting (fetched 2026-11-01)

**[Fact] Plans** ([Pricing](https://www.mixedbread.com/pricing)): Starter (free, **$5 one-time credits, no card**, 3 users, **3 stores**, 100 queries/min, 6,000 uploads/min); Scale ($20/mo, **$20 credits included monthly**, then pay-as-you-go; unlimited users, 10,000 stores, 2,400 queries/min, 12,000 uploads/min); Enterprise (custom, BYO bucket/cloud). **Same per-unit rates on every plan.**

**[Fact] Usage rates:**

| Item | Rate |
|---|---|
| Index — Fast (mgrep default) | $1.50 / 1M content tokens ingested |
| Index — High Quality (OCR/summaries) | $3.00 / 1M |
| Semantic search | $4.00 / 1K queries |
| … with rerank (mgrep default) | +$3.50 → **$7.50 / 1K** |
| Grep (API only; not in CLI) | $0.10 / 1K |
| Storage | $0.50 / 1M content tokens / month |
| Toast 1 agent LLM (`--agentic`) | $0.50 in / $0.06 cached-in / $1.20 out per 1M LLM tokens (launch: $0.30/$0.036/$0.72); its searches $1/1K (+$1.50/1K rerank) |
| Content tokens | text ≈ 1.4 tokens/word; bandwidth/egress included |

**[Unknown]** Question-answering (`-a`) is not separately priced on the pricing page (it is recorded as a `qa` event — [Question Answering](https://www.mixedbread.com/docs/stores/search/question-answering)); generation cost is presumably LLM-token-based — unverified. Another reason to leave `-a` off in the bounded fallback.

**Scenario formulas [Fact — arithmetic from published rates]:** for a repo of `W` words → `T ≈ 1.4·W` content tokens:
- Initial index: `1.5 · T/1M $`; per-resync of changed content `ΔT`: `1.5 · ΔT/1M $` (unchanged files cost nothing).
- Storage: `0.5 · T/1M $` per month.
- Search: rerank on `7.5 · Q/1000 $`, off `4.0 · Q/1000 $`.
- Example: 1M-word repo ≈ 1.4M tokens → $2.10 one-time index + $0.70/mo storage; 1,000 fallback queries/mo → **$7.50 with rerank / $4.00 without**. A cross-OS dual-namespace (CLI + adapter both uploading) would roughly double ingest+storage — avoid it.

**[Fact] Accounting delay & overdraft:** "usage metrics may be delayed by up to 10 minutes… the system allows **small overdrafts** to prevent service interruptions" ([Billing](https://www.mixedbread.com/docs/platform/billing), [Pricing FAQ](https://www.mixedbread.com/pricing)). **[Fact]** No hard spend cap is documented on any public page. **[Fact]** mgrep contains a hardcoded legacy quota message ("monthly limit of 2,000,000 store tokens" — [`utils.ts`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/utils.ts)) whose relationship to the current credit model is unclear; a CLI-quota vs dashboard-balance mismatch is documented in [#117](https://github.com/mixedbread-ai/mgrep/issues/117) (open). **[Inference]** Effective spend control under your constraints: Starter with **no payment method** hard-stops at credit exhaustion; on Scale, control procedurally (monitor Billing Settings balance + store `events` endpoint for query counts; the events API lists ingestion/search/grep/qa events per store — [API reference](https://www.mixedbread.com/docs)). Your Oct-27 billing data is indeed historical; current public rates are as above.

### 7. Falsifiable benchmark protocol

Your specification is methodologically sound; the public evidence base is not — the vendor's own numbers are **[Vendor claim]**: mgrep.dev's "76% vs 24% win rate, $0.23 vs $0.49 cost/query, 82s vs 158s response time" come from **50 queries on their own playground repos with an LLM judge, methodology unpublished** ([mgrep.dev](https://www.mgrep.dev/), README). Treat as marketing, not evidence.

Protocol (counterbalanced, precommitted):
- **Corpus:** identical immutable allowlisted snapshot of your actual Python/JS/TS/PowerShell sources, tagged `repo_id`/`revision`/`relative_path`/`file_hash`; source oracle hidden from answerers. RU/EN; question classes: known-symbol, exact-string, semantic/intent, navigation ("where is X configured"), no-answer (planted unanswerable).
- **Arms:** baseline (CodeIndexer-first alone) vs baseline+fallback, counterbalanced across agents (Claude Code, Codex, Cursor) and question order; **3 repeats**; host trials serialized (no concurrent benchmarks); cold state via fresh sessions/VMs, never by stopping production services.
- **Counting:** all delivered tool context **after serialization** (errors, wrappers, prompt echoes included); actual successful product calls; correctness/omissions/false answers by a blinded independent judge; cold/warm wall time; process-tree peak & idle RAM; CPU-seconds; GPU/VRAM; disk/network; attributable cloud charges from the store events endpoint + billing page (accepting the ≤10-min metric lag).
- **Separate invocation cost from demonstrated benefit:** report fallback-call count, hit-rate at each trigger tier, and per-successful-extra-answer marginal cost.
- **[Unknown → measure]**: retrieval model identity is not publicly pinned per store; two runs weeks apart may use different models — pin the benchmark window short and record the store's `updated_at` ([`getInfo`](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/store.ts)).
- **Precommit thresholds [Inference — proposed]:**
  - **KEEP**: judged correctness ≥ baseline +5pp, serialized context ≤ 0.9× baseline, zero false-answer regressions, no silent-staleness incident in any wrong-revision test, attributable cloud cost ≤ $5/mo at observed volume.
  - **TRY**: correctness within ±5pp but ≥1 of context-size/time/cost improves ≥20%, no correctness regression; revisit with tuned triggers.
  - **PARK**: correctness −5pp or more, or context grows, or any wrong-revision/wrong-clone answer served, or cost unpredictable at canary scale.

### 8. Strongest argument against adoption; failure modes; canary & rollback

**Strongest argument AGAINST [Inference from facts above]:** the deployment you need — bounded, watcher-free, cross-OS, revision-scoped, single-store — is one the vendor does not ship, does not test, and has no tracker activity toward (no relative-path issue/PR as of 2026-11-01). The CLI's absolute-path coupling is its core scoping design, the project is 0.x with rapid change (7+ releases in 5 months, 61 issues, many core-quality ones open), and the free-tier legal documents contradict each other on training with indefinite retention. You would be maintaining an unsupported adapter against an undocumented-at-the-edges 0.x service, for a fallback whose value-add over CodeIndexer is limited to semantic/ambiguity cases. The 40-answer prior test being invalid means you currently have **zero** comparative evidence; the vendor's only public benchmark is 50 self-judged queries.

**Likely failure modes [Fact-sourced]:** silent staleness without `-s` (§3); cross-OS empty results if anyone uses the raw CLI against the shared store (§2); quota/credit mismatches and 10-min-lag overdrafts (§6); Windows hook crashes and env-var leaks if anyone runs the installers (§4); watcher orphans if the plugins are ever installed (§4); secret leakage via non-hidden files (no allowlist, §3); case-sensitivity zero-result traps on Windows (§2); unpinned retrieval model changing results between runs (§7).

**Minimal reversible canary [Inference — design]:**
1. One disposable copy of one representative repo (never production working copies); no production service touched.
2. Existing org; **no new accounts**; use one **existing private store** (or its [store copy](https://www.mixedbread.com/docs) feature to clone an index without re-ingestion cost if you prefer isolation — copy-store is listed in the official API reference).
3. Adapter (or, for a single-machine smoke, the raw CLI) with `repo_id`/`revision`/`relative_path`/`file_hash` metadata; allowlist enforced adapter-side; secrets scan pre-upload.
4. **Search-only API key scoped to that store**; no card added; watch the balance and the store events endpoint.
5. ≤2 calls/task, `-m 5`, no `-a`/`--agentic`/`--web`; wrapper timeout 20s; RU/EN question set from §7; three repeats.
6. Decision against the KEEP/TRY/PARK thresholds.

**Rollback [Fact-grounded]:** delete the canary store (dashboard or API — "delete a store… and the associated content is removed", [Security](https://www.mixedbread.com/docs/production/security)); revoke the scoped key; remove the adapter from PATH; since no agent hooks/MCP registrations were installed and no client configs were changed, there is nothing to unwind on any of the three clients. Note **[Fact]**: free-tier *usage logs* (queries) are retained indefinitely under current terms — the rollback removes content, not necessarily historical usage data; this is a known, documented limitation (ToS §3.1.2, PP §6).

---

## Source Notes

| Source | Credibility | Last updated |
|---|---|---|
| [npm registry: @mixedbread/mgrep](https://registry.npmjs.org/@mixedbread/mgrep) (dist-tags, publish timestamps, package metadata) | 5/5 | 2026-04-25 (v0.1.13 publish; fetched 2026-11-01) |
| [mixedbread-ai/mgrep releases](https://github.com/mixedbread-ai/mgrep/releases) | 5/5 | 2026-04-25 |
| [mgrep source: search.ts](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/commands/search.ts) | 5/5 | main @ 2026-11-01 (≈ v0.1.13) |
| [mgrep source: utils.ts (upload/sync/hash)](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/utils.ts) | 5/5 | main @ 2026-11-01 |
| [mgrep source: store.ts (SDK wrapper)](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/store.ts) | 5/5 | main @ 2026-11-01 |
| [mgrep source: file.ts / git.ts (ignore & enumeration)](https://raw.githubusercontent.com/mixedbread-ai/mgrep/main/src/lib/file.ts) | 5/5 | main @ 2026-11-01 |
| [mgrep source: watch.ts / watch_mcp.ts / config.ts / index.ts / codex.ts / mgrep_watch.py / SKILL.md](https://github.com/mixedbread-ai/mgrep) | 5/5 | main @ 2026-11-01 |
| [mgrep README](https://github.com/mixedbread-ai/mgrep/blob/main/README.md) | 4/5 (self-description; benchmark claims vendor-only) | 2026-11-01 |
| [mgrep.dev](https://www.mgrep.dev/) (marketing benchmark) | 2/5 (vendor, n=50, method unpublished) | 2026-11-01 |
| [Mixedbread Pricing](https://www.mixedbread.com/pricing) | 4/5 (vendor, primary for rates) | fetched 2026-11-01 |
| [Mixedbread Billing docs](https://www.mixedbread.com/docs/platform/billing) | 4/5 | fetched 2026-11-01 (undated) |
| [Create Files (external_id, overwrite, statuses)](https://www.mixedbread.com/docs/stores/store-files) | 5/5 (official API docs) | fetched 2026-11-01 (undated) |
| [Metadata Filtering (operators)](https://www.mixedbread.com/docs/stores/metadata-filtering) | 5/5 | fetched 2026-11-01 (undated) |
| [Search docs](https://www.mixedbread.com/docs/stores/search) / [QA docs](https://www.mixedbread.com/docs/stores/search/question-answering) | 5/5 | fetched 2026-11-01 (undated) |
| [Create Stores (private default, is_public, expires_after)](https://www.mixedbread.com/docs/stores/stores) | 5/5 | fetched 2026-11-01 (undated) |
| [Limits](https://www.mixedbread.com/docs/platform/limits) / [API Keys](https://www.mixedbread.com/docs/platform/api-keys) | 5/5 | fetched 2026-11-01 (undated) |
| [Security & Compliance](https://www.mixedbread.com/docs/production/security) | 4/5 (vendor claims incl. certifications) | fetched 2026-11-01 (undated) |
| [Privacy Policy](https://www.mixedbread.com/pages/privacy) | 4/5 (legally binding, self-asserted) | last updated 2026-04-13 per [#145](https://github.com/mixedbread-ai/mgrep/issues/145); fetched 2026-11-01 |
| [Terms & Conditions](https://www.mixedbread.com/pages/terms) | 4/5 (legally binding, self-asserted) | last updated 2026-04-13 per [#145]; fetched 2026-11-01 |
| [Issue #145 (ToS/PP training contradiction)](https://github.com/mixedbread-ai/mgrep/issues/145) | 4/5 (verified against both documents myself) | opened 2026-04-14; activity to 2026-06 |
| [Issue #149 (watcher orphans, RAM/swap)](https://github.com/mixedbread-ai/mgrep/issues/149) | 3/5 (user report, detailed, plausible against source) | 2026-06-12; open |
| [Issues #148, #132, #117, #47, #92, #67, #94, #76, #80, #58, #46, #40, #34, #31, #135, #151](https://github.com/mixedbread-ai/mgrep/issues) | 3/5 (user reports; #47 closed) | 2025-12 → 2026-07; fetched 2026-11-01 |

**Conflicts & caveats:** ToS §3.1.1 vs Privacy Policy §2.3 vs Security page on free-tier training — unresolved, three-way inconsistency (#145 open). mgrep's hardcoded "2,000,000 store tokens/month free quota" message vs the current credit-based pricing — legacy wording, relationship unclear (#117 open). TESTING.md references a nonexistent `watch --once`. npmjs.com's "last published: 2 months ago" snippet is a stale crawl; the registry timestamp (2026-04-25) is authoritative. GitHub release years before 2026 are inferred from ordering. Vendor benchmark figures are unreplicated marketing.

---

## Open Questions

1. **QA (`-a`) pricing** is not published anywhere — measure from the events endpoint/billing during the canary.
2. **Retrieval model identity/version per store** is undocumented; no reproducibility guarantee across time. (Unknown.)
3. **Vendor appetite for native relative-path mode**: no issue/PR exists as of 2026-11-01 — unknown whether a feature request would be accepted; worth filing before investing in the adapter.
4. **Free-tier training contradiction** — unresolved; only a written statement (or paid-tier contract) resolves it.
5. **Windows case-sensitivity edge cases** (drive-letter casing vs `starts_with`) and **Unicode paths** — plausible failure modes from code/docs, publicly untested.
6. **Exact current SDK method signatures** in `@mixedbread/sdk` ≥0.43 for scoped listing/filtering — confirm against the SDK docs before writing the adapter.
7. Whether a Homebrew formula exists (reported by one user in [#67](https://github.com/mixedbread-ai/mgrep/issues/67); npm remains the only documented install path).
8. Which plan your org is currently on — determines which privacy/retention regime actually applies (public docs cover both; your account state is out of scope here).

---

## Recommendations / Next Steps

1. **Do not run `install-claude-code` / `install-codex` anywhere** — they add per-session stdio MCP + background watchers (documented RAM-leak and secret-leak issues) and inject a "replace all search tools" skill that contradicts your bounded-fallback contract.
2. **Do not attempt cross-OS use of the raw CLI against a shared store** — it is architecturally impossible (absolute-path `starts_with` filter) and fails silently with empty results.
3. **File a feature request first** (repo-relative `external_id`/metadata mode + filter override flag) — if accepted, the adapter shrinks to a wrapper; if ignored, that is itself a signal about maintenance risk.
4. **If you proceed: build the minimal adapter** (upload with `repo_id`/`revision`/`relative_path`/`file_hash`, allowlist, revision-scoped search filters, scoped deletion), using a **search-only, single-store-scoped API key**, in your existing org and an existing private store.
5. **Run the §7 canary with precommitted KEEP/TRY/PARK thresholds**; treat any wrong-revision answer or uncontrolled watcher/process as an automatic PARK.
6. **Pre-benchmark status: TRY.** Your prior 40-answer test is void (zero successful mgrep calls, serialized-context overflow), so no adoption decision should be made before the counterbalanced canary produces evidence.