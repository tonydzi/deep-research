# DR26-11-01-HUB-06-1441 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll research this thoroughly using live web sources. Let me start with the core identity and official sources.
Found the primary sources. Let me fetch the official GitHub repo, docs, and site.
README captured. Now the critical part: source code for path handling, GitHub issues, pricing, and SDK docs.
Now let me look at the source code structure — this is critical for the path-portability question.
Source confirms the cwd-based `starts_with` filter. Now the upload/sync internals — this determines whether repo-relative paths + repo_id/revision can work.
The upload semantics are now clear. Let me check the store SDK layer, npm registry version verification, and pricing.
Version verified via registry API. Now pricing, the hosted CLI docs, and the SDK filter documentation.
I have the core mechanism documented. Continuing with pricing, store visibility defaults, and the sync/search CLI docs.
The web tool limit is hit; I'll use `curl` directly for the remaining pages (Mintlify docs sites usually expose markdown directly).
The full docs index is available. Let me download all relevant pages as markdown.
Pricing captured — note the $5 one-time credits model and the documented 10-minute metering lag. Now the store and filter semantics.
The API filter surface is now clear. Now store creation defaults, upload semantics, and search options.
Now the live issues, privacy docs, and remaining repo details via the GitHub API.
The privacy contradiction is documented. Let me verify the actual ToS/Privacy text and the security page.
Privacy contradiction verified against live primary sources. Now the final details: SDK dependency version, guides, mgrep.dev FAQ, and the @mixedbread/cli package.
One last source check — symlink and hidden-file handling in the file scanner.
All research complete. Here is the full report.

---

# DR26-11-01-HUB-06-1441 — Deep Research Report: `@mixedbread/mgrep` / Mixedbread Search as a Portable Private-Cloud Companion for CodeIndexer

**Research date:** 2026-11-01/02 (all live sources fetched directly; no private connectors, accounts, documents or calendar accessed; no installs, purchases, uploads or account changes)
**Labels used:** **[F]** = fact from primary source (linked) · **[V]** = vendor claim · **[I]** = inference from source code/docs · **[U]** = unknown/unverified

---

## 0. Executive Summary

1. **Your Mac empty-result diagnosis is confirmed at source level.** `mgrep search` hardcodes a cloud metadata filter `path starts_with <normalized local cwd>` and uploads use the **absolute local path** as both `external_id` and `metadata.path`. A store populated on Windows (`C:\...`, backslashes, drive letters) cannot be queried by the native CLI on macOS (`/Users/...`). This is not a bug you hit; it is the only search path the CLI has. There is no flag to disable or override the filter. (§2)
2. **A single private store with canonical repo-relative paths + `repo_id` + `revision` IS achievable without a fork — but only through the official SDK/API (`@mixedbread/sdk` / `mixedbread` Python), not through the `mgrep` CLI.** The docs explicitly recommend repo-relative `external_id`s, support arbitrary metadata (`eq`/`starts_with`/`in`… filters), delete-by-external-id, and full metadata update. A thin adapter script is required. (§2.3–2.5)
3. **The mgrep repo is effectively in maintenance mode**: latest release 0.1.13 published 2026-04-25, no commit on `main` since 2026-04-25, while 44 issues remain open including Windows breakage, watcher memory leaks (600+ orphan processes), and a hook that logs API keys to a world-readable file. Meanwhile the platform side (pricing, CLI, Toast 1) has moved on — the mgrep binary's free-tier quota message ("2,000,000 store tokens/month") no longer matches current pricing ($5 one-time credits). (§1, §3, §6)
4. **Privacy is genuinely unresolved for free-tier users**: the live Terms of Service say free-tier "Usage Data … including queries and any other input provided by the User" **may be used for model training** and is **retained indefinitely with no guaranteed deletion**, while the live Privacy Policy and Security page say free-tier data is **never used for training**. Issue [#145](https://github.com/mixedbread-ai/mgrep/issues/145) (open since 2026-04-14) documents this contradiction; it remains unreconciled as of 2026-11-02. (§5)
5. **The strongest argument against adoption** is the combination of (a) that unresolved training-data contradiction on exactly the tier your $5 credits live on, (b) a stalled 0.x CLI you'd have to bypass with a custom adapter anyway, and (c) unproven local-resource savings — every `mgrep` call still spawns a local Node process, and the vendor's own "2× token reduction" number is a 50-task, LLM-judged, self-run benchmark. (§8)

---

## 1. Exact Identity, Releases, Licensing

**[F] Identity.** `@mixedbread/mgrep` is the npm distribution of **[github.com/mixedbread-ai/mgrep](https://github.com/mixedbread-ai/mgrep)** ("A calm, CLI-native way to semantically grep everything, like code, images, pdfs and more"), by Mixedbread AI. Site: [mgrep.dev](https://www.mgrep.dev); demo: demo.mgrep.mixedbread.com. It is a thin CLI over **Mixedbread Search** vector stores via the official SDK (`@mixedbread/sdk` `^0.43.0` dependency — **[F]**, [package.json](https://github.com/mixedbread-ai/mgrep/blob/main/package.json)). The separate **`mxbai` CLI** is `@mixedbread/cli` (latest **2.4.0**, published 2026-07-08, Apache-2.0 — **[F]**, npm registry) with richer store/sync commands ([docs](https://www.mixedbread.com/cli)).

**[F] Version verification (independent, 2026-11-01).** npm registry API (`registry.npmjs.org/@mixedbread/mgrep`): `dist-tags.latest = 0.1.13`, published **2026-04-25T17:31:38Z**, license **Apache-2.0**. Your installed 0.1.13 is current. Release history: 0.1.4 (2025-11-25) → 0.1.5 (11-30) → 0.1.6 (12-02) → 0.1.7 (12-09) → 0.1.8 (12-21) → 0.1.9 (2026-01-21) → 0.1.10 (01-23) → 0.1.11 (04-08) → 0.1.12 (04-14) → **0.1.13 (04-25)**. Nothing since.

**[F] Repo state.** 4.4k stars, 176 forks, 185 commits, 31 tags, 49 branches; last `main` commit `5c1ba62` "chore: new release v0.1.13" (2026-04-25). **Issues continue to arrive (latest #154, 2026-10-06) with no code release for ~6 months** — 44 open / 17 closed.

**[F] License vs hosted terms.** CLI code: Apache-2.0 (free to fork/modify). Use of the **hosted service** is governed by Mixedbread's Terms ([mixedbread.com/pages/terms](https://www.mixedbread.com/pages/terms)) and Privacy Policy ([mixedbread.com/pages/privacy](https://www.mixedbread.com/pages/privacy)) — materially different regimes for free vs paid tiers (§5). There is **no self-hosted server**: issue [#150 "How can I run the server locally?"](https://github.com/mixedbread-ai/mgrep/issues/150) is open and unanswered (2026-06-17); [#138](https://github.com/mixedbread-ai/mgrep/issues/138) asks for local-only/custom-cloud and got no affirmative answer. Enterprise BYOB/BYOC exists on the platform ([pricing](https://www.mixedbread.com/pricing)) but not for mgrep.

**[F] Runtime/OS.** `package.json` has **no `engines` field and no `os`/`cpu` restrictions**; single bin `mgrep → dist/index.js`; built with Node 24.14.1 (registry metadata). **[I]** Runs anywhere modern Node runs, but watch/hooks have documented Windows defects: [#27 "Fix mgrep watch for Windows"](https://github.com/mixedbread-ai/mgrep/issues/27) (open since 2025-11-24), [#107](https://github.com/mixedbread-ai/mgrep/issues/107)/[#109](https://github.com/mixedbread-ai/mgrep/issues/109)/[#111](https://github.com/mixedbread-ai/mgrep/issues/111) (`os.setsid` POSIX-only crashes, hook-script breakage), [#132](https://github.com/mixedbread-ai/mgrep/issues/132). Your Windows smoke success is consistent — plain search/sync paths are less affected than the agent hooks/watcher.

**[F] Live issues most relevant to paths/sync/staleness** (all open unless noted): #76 (deleted store, re-index fails to upload), #80 (symlink support during indexing — i.e., symlinks not properly handled), #83/#154 (multiple watchers on same dir; one-watcher-per-tree requested), #131 (background sync with Claude Code not working), #147 (watch runs after uninstalling plugin), #149 (watch processes leak — "600+ orphans accumulated, exhausted memory/swap", 2026-06-12), #151 (SessionEnd hook SIGKILLs recycled PIDs), #148 (SessionStart hook dumps full `os.environ` incl. API keys to world-readable `/tmp/mgrep-watch.log`), #117 (quota exceeded in CLI while platform shows available tokens — accounting mismatch), #135 (SKILL.md contradicts README's "complement, not replace" positioning), #136 (dependency audit: 4 high vulnerabilities). Closed and fixed: #140 (tolerate stale PID files), #142 (skip ignored directories when watching) — both Apr 2026, the last active month.

---

## 2. Native Portability — the Core Question

### 2.1 What the mgrep CLI actually does (verbatim source evidence, `main` @ v0.1.13)

**Search always filters by absolute local cwd.** [src/commands/search.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/commands/search.ts):
```ts
const root = process.cwd();
const search_path = exec_path?.startsWith("/")
    ? exec_path
    : normalize(join(root, exec_path ?? ""));
...
const filters = {
  all: [{ key: "path", operator: "starts_with" as const, value: search_path }],
};
```
There is **no flag, env var, or config key that changes or removes this filter**. The complete documented surface is: `-m/--max-count` (top_k, default 10 / `MGREP_MAX_COUNT`), `-c/--content`, `-a/--answer`, `-w/--web`, `--agentic`, `-s/--sync`, `-d/--dry-run`, `--no-rerank` / `MGREP_RERANK`, `--max-file-size`, `--max-file-count`, `--store` (default `"mgrep"`, env `MXBAI_STORE`), auth via `mgrep login` or `MXBAI_API_KEY` (**[F]**, [README](https://github.com/mixedbread-ai/mgrep#readme), [index.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/index.ts)).

**Uploads key files by absolute local path.** [src/lib/utils.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/utils.ts), `uploadFile()`:
```ts
const options = {
  external_id: filePath,          // absolute local path
  overwrite: true,
  metadata: { path: filePath, hash, mtime: stat.mtimeMs },  // hash = xxh64:… (sha-256 legacy)
};
```

**Store listing and deletion are also prefix-scoped to the local absolute root.** `initialSync()` calls `listStoreFileMetadata(store, storeId, repoRoot)` → SDK `files.list` with `metadata_filter: {key:"path", operator:"starts_with", value: repoRoot}` ([store.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/store.ts)), then deletes store entries under `repoRoot` that are not present locally.

**Result display** does `chunk.metadata.path?.replace(process.cwd(), "")` — display-only; if the stored path doesn't contain the local cwd, the raw stored (foreign) path is printed.

### 2.2 Root cause of your observations — confirmed

- **[F]** Mac native CLI empty on a Windows-populated store: filter value is `/Users/…/repo` (POSIX, from `normalize(join(cwd))`); stored `metadata.path` values are `C:\…` (Windows). `starts_with` is **case-sensitive** and separator/drive-format sensitive per the [filter docs](https://www.mixedbread.com/docs/stores/metadata-filtering). Zero matches — exactly what you observed.
- **[F]** Mac official SDK returning correct source for one query: consistent — the SDK (`stores.search`) accepts **arbitrary filters**, including no filter or a `repo_id`-style filter you build yourself; nothing forces the cwd filter at the API level.
- **[I]** Even Mac↔Mac or Windows↔Windows sharing breaks when clone locations differ (`~/work/api` vs `~/src/api`). It only works if the absolute prefix matches.
- **[I] Unicode hazard:** the CLI does no NFC/NFD normalization; macOS filesystems commonly store path components in NFD, Windows in NFC. Directory names containing Cyrillic/accented characters can fail `starts_with` even between two Macs. No test evidence found in the repo — treat as code-derived risk, unverified.
- **[I] Case hazard:** `starts_with` is case-sensitive; two Windows machines differing in drive-letter or user-name case will not see each other's uploads.

### 2.3 Can ONE private store hold canonical repo-relative paths + `repo_id` + `revision`, consumed on Windows/Mac without a fork?

**Via the native mgrep CLI: NO [F].** The CLI has no concept of repo_id/revision, no path-metadata override, and an unavoidable cwd-prefix filter (§2.1).

**Via the official SDK/API: YES [F].** Every required primitive is documented in the official platform docs:

- **Repo-relative `external_id` is the *recommended* pattern**: "The ID can be any string value, though **we recommend using a relative file path to mirror your repository's structure**" ([Create Files](https://www.mixedbread.com/docs/stores/store-files)). `external_id` ≤ 256 chars ([Limits](https://www.mixedbread.com/docs/platform/limits)).
- **Arbitrary metadata on upload**, inherited by all chunks: strings, numbers, booleans, dates, arrays ([Metadata Types](https://www.mixedbread.com/docs/stores/store-files/metadata-types)); ≤ 256 keys, 8 KiB per value, 128 KiB total, depth 5.
- **`overwrite` semantics**: uploading with an existing `external_id` replaces the file (default `true`; `false` errors instead) — this is the natural update primitive.
- **Filter operators for query-time scoping**: `eq`, `not_eq`, `gt/gte/lt/lte`, `in`, `not_in`, `contains`, `like`/`not_like` (case-insensitive, `%`/`_`), `regex`, **`starts_with`** (case-sensitive), plus `all`/`any`/`none` logical groups and `generated_metadata.*` fields ([Metadata Filtering](https://www.mixedbread.com/docs/stores/metadata-filtering)). `filters` is a first-class parameter of `stores.search` and `stores.questionAnswering` — this is exactly how mgrep itself passes its cwd filter.
- **Listing for sync**: `files.list` accepts the same `metadata_filter` (mgrep uses it for its prefix scan; the same endpoint accepts any filter, e.g. `repo_id eq X AND revision eq Y`).
- **Deletion**: `files.delete` by `file_identifier` — documented examples use UUIDs, but mgrep passes the **`external_id`** in that slot and its delete flow works in production code paths (**[F]** mgrep source; **[U]** the API reference doesn't explicitly document external-id resolution on the delete endpoint — verify with one disposable call before relying on it).
- **Metadata update without re-upload**: `files.update` replaces the whole metadata object (full-replace semantics documented).

**[I] Supported schema (constructed from documented semantics — NOT an official example; label as unverified until your disposable-store test passes):**
```
external_id: "<repo_id>@<revision>/<repo-relative-path>"   (≤256 chars; unique per revision)
metadata:    { repo_id, revision, relative_path, file_hash, mtime }
query:       filters = { all: [ {key:"repo_id", operator:"eq", value:…},
                               {key:"revision", operator:"eq", value:…} ] }
```
Design note **[I]**: because `external_id` is the overwrite key, holding **multiple revisions of the same file simultaneously** in one store requires encoding revision in `external_id` (or one store per revision — the starter plan allows only 3 stores, Scale 10,000). Per-revision filtering then keeps wrong-revision hits out of results. Duplicate basenames are a non-issue because the full relative path is the key.

### 2.4 CLI vs official SDK/API vs `mxbai` CLI

| Capability | `mgrep` 0.1.13 | SDK (`@mixedbread/sdk` / Python `mixedbread`) | `mxbai` CLI 2.4.0 |
|---|---|---|---|
| Search filter | hardcoded `path starts_with cwd` **[F]** | arbitrary `filters` **[F]** | `--top-k`, `--threshold`, `--rerank`, `--return-metadata`, `--file-search`; **no documented metadata-filter flag** **[F]** ([docs](https://www.mixedbread.com/cli/search)) |
| Path metadata at upload | absolute local path, hardcoded **[F]** | any `external_id` + any metadata **[F]** | `--metadata <json>` uniform per run; per-pattern via `--manifest` **[F]** ([docs](https://www.mixedbread.com/cli/files)); **[U]** how it derives `external_id` is not documented publicly |
| repo_id/revision scoping | impossible | full | upload-side yes, query-side no |
| One-shot sync | only `search -s` (sync-then-search) or `watch` | build from `files.list`+hash compare (mgrep's algorithm is a usable reference) | `mxbai store sync` with git-based (`--from-git`) or hash-based change detection, `--dry-run`, `--force` **[F]** ([docs](https://www.mixedbread.com/cli/sync)) |
| top_k | `-m` (default 10) | `top_k` param (search CLI range 1–100) | `--top-k` |
| Rerank | on by default, `--no-rerank` | param | off by default, `--rerank` |
| Timeout/retry/call-limit controls | **none in CLI source [F]** | **[U]** SDK defaults not documented on the pages reviewed | **[U]** |

### 2.5 Minimal adapter (only because native means are insufficient)

**[I] Recommended shape** (~150 lines, no fork, ordinary shell usable by Claude Code / Codex / Cursor):
- `mxbc-sync <repo-root>`: compute `repo_id` (origin URL hash) + `revision` (`git rev-parse HEAD`); walk git-tracked+untracked-non-ignored files (reuse mgrep's rules: `.gitignore`, `.mgrepignore`, skip hidden, `*.lock/*.bin/*.ipynb/*.pyc/*.safetensors/*.sqlite/*.pt`, 1 MB cap default); `files.list` filtered by `repo_id+revision`; upload changed/missing with repo-relative `external_id` and `{repo_id, revision, relative_path, file_hash (xxh64/sha-256), mtime}`; delete entries whose relative path vanished locally **filtered to the same `repo_id+revision`** — this can never touch another clone's or another revision's files because the deletion candidate set is metadata-filtered, not path-prefix-filtered.
- `mxbc-search <query> [--top-k N]`: `stores.search` with `filters {all:[repo_id eq, revision eq]}`, rerank on, print `relative_path:start_line-end_line` + snippet. Map back to the local absolute path client-side.
- **No fake cwd, no symlink-following, no repo/revision dropped** — both are mandatory filter terms on every query.

### 2.6 Edge-case matrix (what to test in disposable copies)

| Case | Native CLI behavior | SDK adapter behavior |
|---|---|---|
| Absolute roots | always uploads/searches absolute paths **[F]** | repo-relative everywhere **[F]** |
| Windows ↔ Mac separators/case | mismatch → empty results **[F]** | irrelevant (relative paths) **[F]** |
| Unicode (NFC/NFD) paths | **[I]** no normalization in CLI; risk of prefix mismatch | normalize to NFC client-side before upload/filter (adapter responsibility) |
| Duplicate basenames | fine (full path key) | fine (relative path key) |
| Wrong clone / wrong revision | silently mixed: any store entry under the cwd prefix matches; **no revision validation [F]** | excluded by mandatory `repo_id`+`revision` filters; validate local `file_hash` == stored `file_hash` before trusting a snippet for readback **[I]** |
| Source readback | prints stored path with line range; no local-hash verification **[F]** | adapter prints relative path + lines; verify hash before citing |

---

## 3. Freshness Semantics (one-shot sync, no permanent watch)

**[F] Detection algorithm (mgrep `initialSync`, utils.ts):** list store files under the sync root → local file set from `git ls-files` + `git ls-files --others --exclude-standard` (so `.gitignore` honored; untracked-but-not-ignored included) → delete store entries under the root missing locally → for remaining: mtime "bloom filter" (skip if `stat.mtimeMs <= stored.mtime`), else read file and compare content hash (xxhash64, `xxh64:` prefix; legacy sha-256 accepted) → upload if hash differs. Empty files skipped. Concurrency 100. **No git-diff logic in mgrep's git.ts** — the git-based diff mode is an `mxbai store sync --from-git` feature **[F]** ([docs](https://www.mixedbread.com/cli/sync)).

**[F] Create/edit/rename/delete:** plain search never uploads. `search -s` runs one `initialSync` then searches. `watch` does initialSync + per-directory non-recursive `fs.watch`, uploading on change events and deleting on ENOENT/ENOTDIR (rename = delete old + upload new). **[I]** Because the key is the path, a rename re-uploads content (no rename detection); content-identical files at two paths are stored twice (no cross-path dedup — hash dedup is same-path only).

**[F] Multiple clones/revisions:** each clone syncs only against store entries under **its own absolute root** (deletion candidate set is `path starts_with <own root>`), so clone A's sync cannot delete clone B's files when roots differ — your "don't delete another clone's files" requirement is satisfied **by accident of the prefix design, not by intent**. Caveats **[I]**: (a) syncing from a **parent** directory of several clones (allowed anywhere below `$HOME`) will delete store entries of any subtree not present locally; (b) **checking out a different revision in the same clone deletes the previous revision's files** from the store (they're under the same root and no longer present locally) — revisions cannot coexist in one clone's root with the native CLI; the SDK adapter with revision-encoded `external_id`s handles coexistence.

**[F] Ignored secrets / symlinks:** ignore sources = `.gitignore` + `.mgrepignore` (gitignore syntax, repo root) + 7 hardcoded patterns (`*.lock`, `*.bin`, `*.ipynb`, `*.pyc`, `*.safetensors`, `*.sqlite`, `*.pt`) + all dot-file components. **There is no secret scanning** — a tracked or non-ignored `.env`, key file or token is uploaded verbatim. Symlinks: open issue [#80](https://github.com/mixedbread-ai/mgrep/issues/80) (2025-12-06) requests symlink support; **[I]** from file.ts, the non-git walker skips symlinks (`entry.isFile()` false), while in git repos `git ls-files` lists tracked symlinks and `readFile` would follow them — inconsistent and unverified; test in a disposable copy or exclude symlinks explicitly in the adapter.

**[F] Upload ownership:** all uploads are attributed to the authenticated principal (device-login JWT, refreshed every 5 min under `watch`, or `MXBAI_API_KEY`) into a store owned by your **one organization**; store names are unique per org, stores are **org-private by default** ([docs](https://www.mixedbread.com/docs/stores/stores)). No per-user ACL granularity within an org (open issue #120 requests multi-user support).

**[F] Query-time revision validation: none in the CLI.** The SDK adapter must do it (filter + local hash check). Exact source readback = `relative_path` + `generated_metadata.start_line`/`num_lines` (chunk metadata inherited from file — verified in mgrep's `formatChunk` and the [Generated Metadata](https://www.mixedbread.com/docs/stores/store-files/generated-metadata) docs).

---

## 4. Bounded Fallback Design (CodeIndexer-first)

Grounded in the verified surface:

- **Trigger:** call the cloud store only on explicit semantic failure/ambiguity of CodeIndexer (e.g., ≥2 failed/empty keyword attempts, or a natural-language intent query). Do **not** install agent plugins: `install-claude-code` / `install-codex` / `install-opencode` / `install-droid` inject SessionStart/SessionEnd hooks that auto-start a background watcher **[F]** (README caution block) — precisely what you exclude. Issue [#135](https://github.com/mixedbread-ai/mgrep/issues/135) documents that the plugin's SKILL.md pushes mgrep as the *first* search tool, contradicting the bounded-fallback role.
- **No per-session stdio MCP, no watchers:** plain shell (`mxbc-search` / `mgrep "query"` without `-s`, or `mxbai store search`). mgrep's binary includes an MCP server capability (`watch_mcp`, `@modelcontextprotocol/sdk` dependency) — don't use it.
- **Bounds available natively [F]:** top_k (`-m` / `MGREP_MAX_COUNT`, default 10; CLI range 1–100), snippets (`-c`), rerank toggle, `--max-file-size/count`. **Timeout/retry/call limits do not exist in the mgrep CLI** — wrap with shell `timeout` (e.g., 15 s), one retry, and a per-session cap (e.g., ≤5 cloud calls) in the adapter. **[U]** SDK-level default timeout/retry behavior is not documented on the reviewed pages.
- **Exact-symbol/navigation/no-answer:** routing rule from the vendor's own positioning table **[V]** ("use grep for exact matches, mgrep for intent") — exact symbols → CodeIndexer/grep always; navigation ("go to definition") → never cloud; no-answer → return the negative result to the agent, don't escalate to QA mode (`-a`), which costs LLM tokens and can hallucinate beyond snippets.
- **RU/EN:** mgrep advertises "semantic, multilingual & multimodal" **[V]**; Mixedbread's retrieval stack is multilingual (their own multilingual benchmarks, e.g. MIRACL-Vision across 18 languages **[V]**). No published RU-specific code-search quality evidence — must be a benchmark axis (§7).

---

## 5. Privacy

**[F] Defaults:** stores are **private by default** ("Private (default): Only your organization can access" — [Create Stores](https://www.mixedbread.com/docs/stores/stores)); `is_public=True` is opt-in. Public stores are addressed `org/store` and **searches by outsiders are billed to the caller's key** ([Public Stores](https://www.mixedbread.com/docs/stores/stores/public-stores)). mgrep's `watch` creates the store via SDK `stores.create({name, description})` with no `is_public` → private **[F]**. Deletion: deleting a store file removes "the original file … all generated chunks and embeddings, associated metadata and search indexes, processing history and logs" **[F]** ([Manage Files](https://www.mixedbread.com/docs/stores/store-files/manage-store-files)); store deletion available via dashboard/API; API keys can be scoped to Stores vs completions ([API Keys](https://www.mixedbread.com/docs/platform/api-keys)) — **[U]** whether store-scope keys can be read-only (query-only) is not stated on the reviewed pages.

**[F] Guarantees (vendor):** SOC 2 Type 2, ISO 27001, GDPR with DPA + EU/UK/Swiss SCCs; TLS ≥1.2 in transit, AES-256 at rest; "No training on your data. Your content and usage are never used to train models, for any user on any plan"; **data residency: all processing and Mixedbread-managed storage currently in the United States**, EU residency "coming soon" ([Security & Compliance](https://www.mixedbread.com/docs/production/security), fetched 2026-11-02). Enterprise BYOB/BYOC options exist.

**[F] The unresolved contradiction (live pages, fetched 2026-11-02):**
- **Terms of Service** ([mixedbread.com/pages/terms](https://www.mixedbread.com/pages/terms)): under the **Free Tier**, the company "collects all Usage Data, including but not limited to **queries and any other input provided by the User**. This data is stored in its native form and may be used for … **model training**, analytics, service improvement, and research activities." Free-tier data "is **retained indefinitely**. Although Users may contact the Company to request deletion of such data, **no guarantee can be provided** that any data will be removed." Paid tier: usage data "shall not be used for model training."
- **Privacy Policy** ([mixedbread.com/pages/privacy](https://www.mixedbread.com/pages/privacy)): "We **do not use any data from Free Tier users for model training**. Data collected under the Free Tier is retained indefinitely."
- Issue [#145](https://github.com/mixedbread-ai/mgrep/issues/145) (open since 2026-04-14) documents exactly this and is unresolved.

**[I]** Since uploaded code is literally "input provided by the User," the ToS reading is that **free-tier uploaded source could be used for training**. The Security page agrees with the Privacy Policy, but the ToS has not been amended. For your decision this means: **either accept the ambiguity on the $5-credit tier, or run the canary only on a paid ($20 Scale) organization where "no training" is stated in all three documents.**

**[F] Additional concrete incident:** [#148](https://github.com/mixedbread-ai/mgrep/issues/148) — the Claude Code SessionStart hook writes full `os.environ` (including API keys) to a world-readable `/tmp/mgrep-watch.log`. One more reason to avoid agent-hook installation entirely.

---

## 6. Current Public Pricing and Accounting (verified 2026-11-01/02)

**[F]** [Pricing page](https://www.mixedbread.com/pricing) + [billing docs](https://www.mixedbread.com/docs/platform/billing):

| Item | Rate |
|---|---|
| Starter (free) | **$5 one-time credits**, no card; 3 stores; 100 queries/min; 6,000 uploads/min; 3 users |
| Scale | $20/mo **returned as $20 credits**, then PAYG; 10,000 stores; 2,400 queries/min |
| Indexing — Fast (content-aware) | **$1.50 / 1M content tokens** |
| Indexing — High Quality (OCR/summaries) | $3.00 / 1M |
| Search — grep (exact/regex) | $0.10 / 1K queries |
| Search — semantic | **$4.00 / 1K** |
| Search — semantic + rerank | **$7.50 / 1K** ($0.0075/query) |
| Toast 1 search rate | $1 / 1K, +$1.50 with rerank |
| Toast 1 LLM (launch, 40% off) | $0.30 input / $0.036 cached / $0.72 output per 1M LLM tokens |
| Storage | **$0.50 / 1M content tokens / month** |
| `mixedbread/web` store | billed as search-with-rerank per query |
| Bandwidth/egress | included |
| Token conversions | text ≈ 1.4 tokens/word; 1 doc page = 2,000; 1 image = 2,000; 1 min video = 24,000; 1 min audio = 12,000 |

**[F] Accounting behavior (documented):** "The usage metrics may be delayed by **up to 10 minutes**"; "the system allows for **small overdrafts** to prevent service interruptions" ([billing](https://www.mixedbread.com/docs/platform/billing)). Balance/credits visible in Billing Settings; no documented **hard spend cap** for self-serve plans on the reviewed pages — **[U]** whether a configurable hard limit exists in the dashboard (check when you inspect the live balance; October 27 billing is historical as you note). Corroborating live issue: [#117](https://github.com/mixedbread-ai/mgrep/issues/117) — CLI reported quota exceeded while the platform showed available tokens (Jan 2026). Note the CLI's built-in message ("monthly limit of 2,000,000 store tokens") is **stale** relative to the current $5-credit model — do not use the CLI message as ground truth for the balance.

**[I] Scenario formulas (no invented actuals; plug in your own numbers):**
```
One-time index   = content_tokens × $1.50/1M                 (fast; text tokens ≈ 1.4 × words)
Monthly storage  = content_tokens × $0.50/1M
Incremental sync = changed_files_tokens × $1.50/1M           (unchanged files: $0, mtime+hash gated)
Search session   = calls × $0.0075  (semantic+rerank)  or  × $0.004 (no rerank)
QA/answer call   = search + Toast 1 LLM tokens (input/cached/output rates above)
Example: 500k-word repo ≈ 700k content tokens → $1.05 initial index, $0.35/mo storage;
a 20-query reranked session ≈ $0.15. $5 free credits ≈ ~666 reranked queries or ~3.3M indexed tokens.
```
**[U]** Whether `files.list`/management calls are billed is not stated on the reviewed pages (they are not in the priced list; assume free but verify in the usage breakdown).

---

## 7. Falsifiable Benchmark Protocol (per your precommit spec)

Your prior 40-answer test is treated as invalid, per your own note (zero successful mgrep calls; serialized context over budget). Redesign:

- **Corpus:** immutable, allowlisted snapshot of your actual repos (Python/JS/TS/PowerShell present; skip absent languages), RU+EN queries; categories: known-symbol, exact-match, semantic intent, navigation, no-answer. Tag every item with `repo_id`, `revision`, `relative_path`, `file_hash`. Source oracle built from the snapshot, hidden from answerers.
- **Arms (counterbalanced, within-subject per question):** baseline (CodeIndexer/grep only) vs baseline + cloud fallback (adapter or mgrep CLI). Order counterbalanced; answerers blinded to arm.
- **Metering — the lesson from the invalid run:** count **all delivered tool context after serialization**, including error bodies and wrappers, in bytes/tokens; log **actual successful product calls** (a call that returns zero results still counts as a successful API call and is billed); record correctness, omissions, and false answers against the oracle. For mgrep specifically: results include match % and path+line-range only unless `-c`, so snippet size is controllable — pin it identically across arms.
- **Resource metrics:** cold/warm wall time (hyperfine, 3+ repeats, report mean±spread); process-tree peak and idle RSS (mgrep = Node process per invocation; watch = persistent process — measure both modes); CPU-seconds; GPU/VRAM (should be ~0 locally — if not, that's a finding); disk/network bytes; attributable cloud charges from the usage dashboard **read ≥10 minutes after the run** to clear the metering lag (§6).
- **Serialization discipline:** host trials serialized; do not stop production services to fake cold state (per your constraint); cold = fresh clone + fresh store (disposable), warm = second run same store.
- **Precommit thresholds (proposal, to be ratified before running):**
  - **KEEP:** ≥25% reduction in total serialized context across arms with correctness not worse than baseline (≤1 additional miss per 20 questions), ≥1 breached: 10–25% context reduction or correctness within noise → investigate one axis; **PARK:** <10% reduction, any correctness regression beyond noise, or any store content leakage/overwrite incident during trials.
  - Also KEEP-gate: total fleet resource delta (peak RAM·minutes + CPU-seconds across all three clients) must **decrease**, since moving compute to another local host is not a saving (your constraint) — and note the cloud side replaces local embedding compute with per-call Node process overhead; this must be measured, not assumed. **Invocation ≠ demonstrated benefit**: report both separately.

---

## 8. Strongest Argument AGAINST Adoption, Failure Modes, Canary

**Strongest argument (synthesis of [F] items):** *You would be routing proprietary source code to a cloud service whose own legal documents currently disagree about whether free-tier data trains models, through a 0.x CLI whose repository has had no code release in ~6 months, and which — for your actual cross-OS requirement — you must bypass entirely with a custom SDK adapter. The only quantified benefit ("2× token reduction", 50 tasks, LLM-judged) is a vendor-run benchmark, and the claimed local resource savings are unmeasured.* Every one of those four clauses is individually resolvable; together they justify a bounded canary, not adoption.

**Likely failure modes (with evidence):**
1. Cross-OS/multi-clone path mismatch (native CLI) — §2, confirmed by source.
2. Revision confusion: no revision concept in CLI; same-clone branch switches delete other-revision files — §3.
3. Watcher leakage/orphans and Windows hook breakage — #149, #147, #151, #148, #27, #107/#111/#132.
4. Secret upload when not gitignored (no secret scanning) — §3.
5. Quota/accounting surprises: 10-min metering lag, small overdrafts, stale CLI quota messages vs current pricing — #117, §6.
6. Cost creep via `-a`/QA/agentic modes (each = search + Toast 1 LLM tokens) and `mixedbread/web` queries (billed as search+rerank) — §6.
7. Privacy-law ambiguity on the free tier — §5.

**Minimal reversible canary [I] (research-derived; nothing here installed or executed):**
- One **disposable clone** of a non-sensitive allowlisted repo; one **dedicated store** (e.g., `mgrep-canary`), private by default, in an org chosen per §5 (paid org if the training ambiguity matters).
- Adapter only (SDK, repo-relative schema §2.3) or plain `mgrep watch` inside the disposable clone; **never** `install-*` hooks; a **separate API key scoped to stores**, stored outside world-readable paths (contrast #148).
- Success criteria = §7 protocol on the canary store only.
- **Rollback (concrete):** delete the canary store via dashboard/API (removes files, chunks, embeddings, metadata, processing history — documented), delete the API key, `npm uninstall -g @mixedbread/mgrep`, remove `~/.config/mgrep/`, and delete any `~/.config/mixedbread/` CLI config. Nothing else persists locally; production CodeIndexer workflow untouched throughout.

**Unresolved questions (exact state):**
1. ToS vs Privacy Policy vs Security page on free-tier training — three sources, two positions, issue #145 open (**[F]**; unverifiable externally which reflects practice).
2. Whether the `files.delete` endpoint officially resolves `external_id` in `file_identifier` (works in mgrep's code path; doc examples show UUIDs only).
3. SDK default timeout/retry behavior; whether any self-serve hard spend cap exists in the dashboard.
4. Whether management calls (`files.list`) are billed.
5. RU-language code-search quality (vendor multilingual claims only).
6. Symlink behavior in git repos (mixed evidence; #80 open).
7. `mxbai store upload` external-id assignment rules (undocumented publicly).
8. Native `mgrep watch` stability on Windows (open issues #27/#107/#111/#132; no release since).

---

## Primary Sources (all fetched live 2026-11-01/02)

- Repo/source: [github.com/mixedbread-ai/mgrep](https://github.com/mixedbread-ai/mgrep) — [search.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/commands/search.ts), [watch.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/commands/watch.ts), [utils.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/utils.ts), [store.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/store.ts), [file.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/file.ts), [git.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/git.ts), [index.ts](https://github.com/mixedbread-ai/mgrep/blob/main/src/index.ts), [package.json](https://github.com/mixedbread-ai/mgrep/blob/main/package.json) (all at `main`, v0.1.13, commit `5c1ba62`, 2026-04-25)
- npm registry: `registry.npmjs.org/@mixedbread/mgrep` (0.1.13, 2026-04-25), `@mixedbread/cli` (2.4.0, 2026-07-08)
- Docs: [pricing](https://www.mixedbread.com/pricing) · [billing](https://www.mixedbread.com/docs/platform/billing) · [stores](https://www.mixedbread.com/docs/stores/stores) · [public stores](https://www.mixedbread.com/docs/stores/stores/public-stores) · [store files](https://www.mixedbread.com/docs/stores/store-files) · [manage files](https://www.mixedbread.com/docs/stores/store-files/manage-store-files) · [metadata filtering](https://www.mixedbread.com/docs/stores/metadata-filtering) · [metadata types](https://www.mixedbread.com/docs/stores/store-files/metadata-types) · [limits](https://www.mixedbread.com/docs/platform/limits) · [security](https://www.mixedbread.com/docs/production/security) · [CLI intro](https://www.mixedbread.com/cli) · [CLI files](https://www.mixedbread.com/cli/files) · [CLI sync](https://www.mixedbread.com/cli/sync) · [CLI search](https://www.mixedbread.com/cli/search)
- Legal: [Terms](https://www.mixedbread.com/pages/terms) · [Privacy Policy](https://www.mixedbread.com/pages/privacy) (both live 2026-11-02; issue #145 states both "last updated April 13, 2026")
- Issues: [#145](https://github.com/mixedbread-ai/mgrep/issues/145), [#138](https://github.com/mixedbread-ai/mgrep/issues/138), [#149](https://github.com/mixedbread-ai/mgrep/issues/149), [#148](https://github.com/mixedbread-ai/mgrep/issues/148), [#147](https://github.com/mixedbread-ai/mgrep/issues/147), [#117](https://github.com/mixedbread-ai/mgrep/issues/117), [#76](https://github.com/mixedbread-ai/mgrep/issues/76), [#80](https://github.com/mixedbread-ai/mgrep/issues/80), [#27](https://github.com/mixedbread-ai/mgrep/issues/27), [#107](https://github.com/mixedbread-ai/mgrep/issues/107), [#135](https://github.com/mixedbread-ai/mgrep/issues/135), [#150](https://github.com/mixedbread-ai/mgrep/issues/150), plus the [full issue list](https://github.com/mixedbread-ai/mgrep/issues)
- Vendor site: [mgrep.dev](https://www.mgrep.dev) (FAQ: cloud stores, internet connection required, `.mgrepignore`)

---

**Bottom line:** the portable one-store/`repo_id`/`revision` design is real and natively supported **at the API level** (with repo-relative `external_id` even being the vendor's recommended pattern), but the `mgrep` CLI you already have cannot consume it across Windows/Mac — a thin SDK adapter is unavoidable. Before any fleet rollout, resolve the free-tier training-data ambiguity (use a paid org or get written clarification) and run the §7 canary; the resource-saving case is currently unproven in either direction.