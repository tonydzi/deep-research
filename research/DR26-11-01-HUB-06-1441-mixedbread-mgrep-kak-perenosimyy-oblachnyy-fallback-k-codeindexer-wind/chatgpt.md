# DR26-11-01-HUB-06-1441 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-11-01-HUB-06-1441 — Mixedbread mgrep as a portable private-cloud companion to CodeIndexer

## Executive decision

**Research cut:** 2026-11-01, Europe/Lisbon. I used only public first-party sources: the official Mixedbread `mgrep` repository and issues, the official npm package page, Mixedbread Search/API documentation, pricing, billing, privacy policy, and terms. I did not access private connectors, accounts, stores, files, email, or calendars.

**Decision:** **do not adopt native `mgrep` CLI as the cross-OS shared-store fallback in its current 0.1.13 form.** Its current ingestion and search semantics are tied to each machine's absolute local filesystem path: upload uses the local absolute `filePath` as both `external_id` and `metadata.path`, while search filters on `metadata.path starts_with <current local cwd/path>`. That design is directly incompatible with one canonical store being searched interchangeably from Windows and macOS clones. This is confirmed by current source, not merely inferred from the observed Mac failure. citeturn439669view1turn181920view0turn181920view1

**A bounded Mixedbread Search fallback is nevertheless technically viable without forking `mgrep`, but only by bypassing the native `mgrep` path model and using the official Mixedbread SDK/API through a very small ordinary-shell adapter.** Mixedbread's public Store API explicitly supports caller-chosen `external_id`, arbitrary file metadata, compound metadata filters, and canonical path-like IDs. A portable schema can therefore use immutable `repo_id`, `revision`, `relative_path`, and `file_hash`, and every search can require exact `repo_id == ... AND revision == ...`; the SDK/API supports this directly. Official file-management docs even recommend path-like external IDs such as `docs/api/authentication.md`. citeturn429084search1turn429084search2turn429084search3turn429084search4

That changes the object being evaluated: it becomes **“Mixedbread Search as a cloud semantic fallback, using the already-installed SDK/package ecosystem”**, not **“native mgrep CLI as portable CodeIndexer companion.”** Native `mgrep` can remain installed and untouched, but it should not be the portability layer.

There are four hard gates before even a small canary:

1. **Privacy gate.** Mixedbread's current Terms say Free Tier usage data, including queries and inputs, may be used for model training and retained indefinitely, while its current Privacy Policy says Free Tier data is *not* used for model training but is retained indefinitely. This is an explicit first-party contradiction, also reported in the official `mgrep` issue tracker. Proprietary-code upload should therefore be treated as **not privacy-cleared** unless the existing organization is demonstrably governed by Paid-Tier/contractual terms that resolve the conflict. Store privacy alone does not resolve vendor-processing terms. citeturn439669view4turn439669view6turn546954search3
2. **Freshness gate.** Native `mgrep` 0.1.13 uses mtime as a prefilter and does **not hash a file when local `mtimeMs <= stored.mtime`**. A checkout/copy/tool that changes contents without increasing mtime can therefore leave cloud content stale. This is source-confirmed. citeturn288040view0
3. **Revision gate.** Native `mgrep` has no `repo_id` or revision dimension in its file metadata abstraction; same local absolute path across Git revisions overwrites the same store object. A safe fallback must make revision part of identity and filter it at query time. citeturn181920view3turn439669view1
4. **Benefit gate.** No RAM, CPU, GPU, latency, correctness, or cost benefit should be assumed. The existing invalid 40-answer run supplies no comparative evidence, and cloud retrieval still entails local sync/scanning, network overhead, and vendor-side compute. Only the proposed controlled benchmark can establish a net fleet benefit.

**Recommended status today: `TRY only after gates`, not `KEEP`.** If the existing account's privacy terms cannot be unambiguously established without changing tiers/accounts, status should be **`PARK`**. No new account, store, install, card, tier, or paid API is needed for this research conclusion.

## Identity, release, platform support, licensing, and live issues

### Product identity and current version

The official repository is `mixedbread-ai/mgrep`:  
https://github.com/mixedbread-ai/mgrep

The official npm package is `@mixedbread/mgrep`:  
https://www.npmjs.com/package/%40mixedbread/mgrep

The package installs with:

```text
npm install -g @mixedbread/mgrep
```

and exposes an `mgrep` executable backed by `dist/index.js`. The repository/package metadata identifies Mixedbread as the author and the GitHub repository above as the package repository. citeturn546954search0

**Fresh verification of latest release:** the official GitHub Releases page still lists **v0.1.13**, released **2026-04-25**, tag/release commit **`5c1ba62`**. The npm registry page likewise reports **0.1.13** as current. Thus the user's Nov 1 observation that npm `latest` is 0.1.13 is independently confirmed. Official release URL:  
https://github.com/mixedbread-ai/mgrep/releases/tag/v0.1.13  
npm: https://www.npmjs.com/package/%40mixedbread/mgrep citeturn546954search2

The recent release lineage is relevant because several behavior changes precede 0.1.13:

| Release | Date | Commit shown by release page | Material change |
|---|---:|---|---|
| 0.1.13 | 2026-04-25 | `5c1ba62` | update-prompt feature / release |
| 0.1.12 | 2026-04-14 | `0883c7a` | auth refresh behavior |
| 0.1.11 | 2026-04-08 | `93f5647` | watcher ignored-directory handling, stale PID tolerance, deleted-org filtering |
| 0.1.10 | 2026-01-23 | `fe2c523` | agentic search |
| 0.1.9 | 2026-01-21 | `b3ce5f7` | mtime-based unchanged-file skip, config limits, activity reporting |
| 0.1.8 | 2025-12-21 | `77d89fd` | faster hashing, home-directory protection, file-count limits |
| 0.1.7 | 2025-12-09 | `1d70c4c` | path-prefix filtering, max file size, quota handling |
| 0.1.5 | 2025-11-30 | `6a57daf` | delete missing local files from store |
| 0.1.4 | 2025-11-25 | `950d4ce` | binary-file exclusions, MCP/watch work |

Source: official releases page, https://github.com/mixedbread-ai/mgrep/releases. citeturn546954search2

**Source-version caveat:** the implementation excerpts below are from official `main` as crawled on 2026-11-01. The repository's package metadata remains 0.1.13 and no newer release is published, but I did not independently hash the npm tarball against every current `main` source file. Where exact release identity matters for a benchmark, pin the npm package hash or v0.1.13 Git tag rather than assuming `main == tarball`.

### License versus hosted-service terms

The CLI repository/package is **Apache-2.0 licensed**. That governs the open-source CLI code. The hosted Mixedbread API/Search service is separately governed by Mixedbread's Terms and Privacy Policy; the Apache license does **not** grant equivalent rights or privacy guarantees for cloud processing. Official repository/package:  
https://github.com/mixedbread-ai/mgrep  
Hosted terms: https://www.mixedbread.com/pages/terms citeturn546954search0turn439669view5

That distinction matters for a private-code workflow: open-source client code is not the privacy boundary; uploaded files, searches, metadata, logs, and billing are handled by the hosted service under its service terms.

### Node/runtime and OS/architecture support

The repository's contributor guidance states **Node.js 18+** and pnpm 8+ for development. It also tells contributors to use `node:path` for cross-platform paths. Official source:  
https://github.com/mixedbread-ai/mgrep/blob/main/AGENTS.md citeturn546954search7

However, the package metadata does **not provide a formal OS/CPU support matrix**, and there is no published statement I found promising Windows x64, Intel macOS, and Apple Silicon macOS as tested/supported production targets. This is important:

- **Fact:** the CLI is TypeScript/Node-based and the repository describes cross-platform path handling as an engineering goal. citeturn546954search7
- **Fact:** there are open Windows-specific integration failures in the official issue tracker, including use of Unix-only `os.setsid` in a Claude Code hook. citeturn546954search4
- **Inference:** core CLI search may be architecture-neutral where dependencies are pure JS/WASM, but agent hooks and surrounding integration code cannot be assumed portable merely because Node itself runs on all three target architectures.
- **Unknown:** Mixedbread publishes no formal 0.1.13 test matrix establishing identical behavior on Windows x64, Intel macOS, and arm64 macOS.

That is another reason to keep the proposed integration as an ordinary shell command using the official API/SDK rather than installing per-agent hooks.

### Current issue signals

As of the crawl, the official repository shows **44 open issues**. Particularly relevant open reports include:

- **#151, 2026-07-27:** SessionEnd hook can SIGKILL a recycled PID.
- **#149, 2026-06-12:** reporter observed hundreds of orphaned `mgrep watch` processes and memory/swap exhaustion.
- **#148, 2026-06-12:** reporter says the Claude Code SessionStart hook writes the complete environment, including API keys, to a world-readable `/tmp/mgrep-watch.log`.
- **#147, 2026-04-27:** watcher reportedly continues after uninstalling the Claude Code plugin.
- **#145, 2026-04-14:** explicitly flags the Free-Tier model-training contradiction between Terms and Privacy Policy.
- **#132, 2026-02-01:** Windows hook failure from Unix-only `os.setsid`.

Official issue listing: https://github.com/mixedbread-ai/mgrep/issues. citeturn546954search3turn546954search4

These are **user-reported open issues, not independently reproduced findings**. But they are directly relevant to the requested architecture: they strengthen the case against automatic agent installers, session hooks, or permanent watchers. They do **not** establish that a manually invoked search-only SDK wrapper has those defects.

Mixedbread's README itself says that agent integration starts a background sync process automatically at session start and stops it when the session ends, and currently names Claude Code, OpenCode, Codex, and Droid integrations; Cursor is not listed as a completed native installer. Official README: https://github.com/mixedbread-ai/mgrep#using-it-with-coding-agents. citeturn546954search0

For this decision, **do not use those integrations**.

## Cross-OS portability and the exact path problem

### Native `mgrep` 0.1.13 path identity

The portability failure is structural.

During upload, current official source builds:

```ts
{
  external_id: filePath,
  overwrite: true,
  metadata: {
    path: filePath,
    hash,
    mtime: stat.mtimeMs
  }
}
```

where `filePath` is the local machine path. Official source:  
https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/utils.ts citeturn439669view1

On Windows this can be conceptually:

```text
external_id = C:\src\project\src\auth.ts
metadata.path = C:\src\project\src\auth.ts
```

while a Mac clone naturally produces something like:

```text
external_id = /Users/anton/src/project/src/auth.ts
metadata.path = /Users/anton/src/project/src/auth.ts
```

Those are different store objects.

At query time, `mgrep search` derives its root from `process.cwd()`, normalizes `join(root, exec_path)`, and constructs this Mixedbread Search filter:

```ts
{
  all: [{
    key: "path",
    operator: "starts_with",
    value: search_path
  }]
}
```

Official source:  
https://github.com/mixedbread-ai/mgrep/blob/main/src/commands/search.ts citeturn181920view1turn181920view0

The CLI formatter then removes the **current** `process.cwd()` string from returned `metadata.path` when printing a relative-looking result. That output prettification does not make storage portable; storage remains absolute-path based. citeturn288040view3

Therefore:

**Fact:** one Windows-populated native-mgrep corpus cannot be expected to match a Mac `metadata.path starts_with /Users/...` filter. citeturn181920view0turn439669view1

**Fact:** native mgrep's own `FileMetadata` interface contains only `path`, `hash`, and optional `mtime`; it does not contain `repo_id`, Git revision, canonical relative path, clone ID, or workspace ID. citeturn181920view3

**Conclusion:** **native mgrep CLI cannot natively implement the required one-store, repo-relative, revision-scoped cross-OS contract without code changes.**

This is stronger than saying “there is a Mac bug.” The current schema and query construction make host locality part of identity by design.

### Can flags or config fix it?

No documented native `mgrep` flag provides a metadata override or a repo/revision filter.

The documented search controls include maximum result count, content display, generated answer, web, agentic mode, one-shot `--sync`, dry-run, reranking disable, file-size/count controls, and store selection. The configuration file controls file size/count. Nothing documented exposes:

```text
metadata.path = canonical repo-relative path
repo_id = ...
revision = ...
filters = ...
external_id = ...
```

Official CLI documentation/README:  
https://github.com/mixedbread-ai/mgrep#commands-at-a-glance citeturn546954search0

Nor is there a hidden path metadata override in the current abstraction: `FileMetadata` is fixed to `{path, hash, mtime?}` and upload is hardcoded to `external_id: filePath`. citeturn181920view3turn439669view1

Using a fake current working directory, filesystem alias, drive mapping, symlink forest, or path-rewriting shim would violate the requested design and still not supply revision identity. **Do not do it.**

### The official SDK/API does support a portable schema

Mixedbread's underlying Store API is more general than `mgrep`.

Official docs say:

- The caller may assign an **external ID** to a file.
- External IDs may contain slashes and are explicitly suitable for path-like identifiers.
- Re-uploading with the same external ID can replace/update a file.
- Metadata is caller-controlled.
- Metadata filters support equality, starts-with, logical `all`, nested combinations, etc.
- Store search accepts those filters.
- Search hits can be mapped back to a specific store file/chunk. citeturn429084search1turn429084search2turn429084search3turn429084search4

Official TypeScript SDK repository:  
https://github.com/mixedbread-ai/mixedbread-ts  
Package: `@mixedbread/sdk`. citeturn429084search0

A correct portable identity is therefore possible **without a server fork**. The following is a **proposed schema, not a documented Mixedbread convention**:

```json
{
  "external_id": "repo/<repo_id>/rev/<revision>/src/auth/login.ts",
  "metadata": {
    "repo_id": "stable-repository-identifier",
    "revision": "full-git-commit-sha",
    "relative_path": "src/auth/login.ts",
    "file_hash": "sha256:...",
    "language": "typescript"
  }
}
```

Use POSIX `/` separators in the canonical identifier regardless of host OS.

A query must use an AND filter equivalent to:

```json
{
  "all": [
    {"key": "repo_id", "operator": "eq", "value": "stable-repository-identifier"},
    {"key": "revision", "operator": "eq", "value": "full-git-commit-sha"}
  ]
}
```

and, where a subtree restriction is wanted:

```json
{"key": "relative_path", "operator": "starts_with", "value": "src/"}
```

The **filter syntax is officially supported**; the particular field names are the proposed adapter contract. citeturn429084search3turn429084search4

This adapter does not need to provide a daemon or MCP server. It can simply expose ordinary commands such as:

```text
mb-code-sync --repo-root . --store EXISTING_STORE
mb-code-search --repo-root . --store EXISTING_STORE --query "where is refresh-token rotation decided?"
```

Those command names are **illustrative/unverified examples**, not existing Mixedbread commands.

### CLI versus SDK/API

| Capability | native `mgrep` CLI | Official Mixedbread SDK/API |
|---|---|---|
| Caller-controlled `external_id` | No exposed option; hardcoded to local file path | Yes |
| Arbitrary metadata | No; mgrep abstraction fixes path/hash/mtime | Yes |
| Compound metadata filters | Internally supported by SDK but mgrep CLI hardcodes `path starts_with` | Yes |
| `repo_id` exact filter | No native CLI option | Yes |
| `revision` exact filter | No native CLI option | Yes |
| Canonical repo-relative path | No; native upload uses local absolute path | Yes, caller-defined |
| Cross-OS one-store design | No, not safely | Yes, with adapter contract |
| One-shot sync | Yes, `--sync` | Application implements it explicitly |
| Permanent watch required | No | No |
| Native result formatting | Yes | Adapter must format |
| Exact source/chunk retrieval | Search prints result content optionally | API exposes file/chunk retrieval; current search chunks also expose file references/URLs |

The newer generic Mixedbread CLI documents `mxbai store search <store> <query>` with top-k/rerank controls, but the public CLI page I found does **not** document arbitrary compound `repo_id`/revision metadata filters. It therefore does not displace the need for the SDK/API adapter for this safety requirement. Official CLI page:  
https://www.mixedbread.com/cli/search

### Required portability tests

No implementation should be accepted until these are exercised against a disposable, immutable test corpus. These are **tests to run later, not tests performed during this research**:

| Test | Pass condition |
|---|---|
| Different absolute roots | Same canonical file found from `C:\...`, `/Users/...`, and another Mac root with no host-root metadata dependency |
| Windows vs POSIX separators | Store identity is identical because canonical `relative_path` always uses `/` |
| Case difference | Explicitly define repo-path case policy; wrong-case lookup must not silently hit another path |
| Unicode | NFC and NFD filename variants tested; no undocumented Unicode normalization assumed |
| Duplicate basenames | `a/config.ts` and `b/config.ts` remain independently addressable |
| Wrong clone | Search may return same revision corpus, but local source readback must reject if local manifest/hash differs |
| Wrong revision | Zero accepted results unless exact requested `revision` filter matches |
| Dirty working tree | Either index dirty state under a distinct revision/worktree ID or refuse fallback; never label dirty content as clean commit SHA |
| Renamed path | New canonical external ID appears and old one disappears only within the same repo/revision ownership scope |

Mixedbread documents string filters such as `starts_with`; these comparisons should be treated as string semantics, not filesystem semantics. Unicode normalization is **not documented in the public API material I found** and must be empirically tested. citeturn429084search3

## Freshness, synchronization, and lifecycle safety

### `--sync` is genuinely one-shot

Native mgrep's `search --sync` does **not** start the permanent watcher. The source constructs a filesystem view, calls `initialSync`, then polls store status until `pending + in_progress == 0` and returns to the search. citeturn439669view2

So this command pattern is operationally compatible with “no permanent watch”:

```text
mgrep --sync "query"
```

But it is **not portable enough for this design**, because the one-shot sync still uses host-absolute IDs and paths.

The permanent watcher is separate. The README states `mgrep watch` performs initial sync and then keeps the store updated, and agent integrations can launch that watcher automatically. citeturn546954search0

### Native sync algorithm

Current 0.1.13-era source does roughly:

1. List store objects whose metadata path starts with the local repo root.
2. Enumerate local files.
3. Mark stored objects under that root as deleted if absent locally.
4. For a candidate stored file, use mtime as the first change check.
5. Only when mtime is newer or absent, read/hash the file.
6. If hash differs, upload with overwrite.
7. Run work with concurrency set to 100. citeturn439669view1turn288040view0

Current hashing is **xxHash64**, prefixed `xxh64:`; compatibility code still recognizes older unprefixed SHA-256 hashes. citeturn181920view7

This hash is useful for fast change detection; **xxHash64 is not a cryptographic integrity primitive**. For the portable manifest/oracle, use a cryptographic file hash such as SHA-256 independently of mgrep's internal optimization.

### Create, edit, rename, delete

**Create.** A local path absent from the store metadata map is uploaded. citeturn439669view1turn288040view0

**Edit.** Native mgrep first checks:

```text
stat.mtimeMs > stored.mtime
```

and later explicitly skips when:

```text
stat.mtimeMs <= stored.mtime
```

before computing the content hash. Therefore a content change with equal or older mtime can be missed. The source comment calls this a “Bloom filter,” but logically it is an mtime shortcut, not a Bloom filter. citeturn288040view0

This can arise from file restoration, archive extraction, some build/deployment tools, timestamp preservation, or checkout workflows. The exact frequency is workload-dependent; the failure condition itself is source-proven.

**Rename.** Under the same local-root namespace, the new absolute path is a new external ID and the absent old path is eligible for deletion. This behaves as delete+upload, not an atomic rename. citeturn439669view1turn288040view0

**Delete.** `initialSync` lists store files filtered by the current local root and deletes only entries that are both under that root and missing locally. This is important: a Mac sync will generally **not delete Windows entries under `C:\...`**, because it cannot see them through its own root-prefix filter. That limits one destructive hazard but produces another: the store can accumulate host-specific duplicate corpora. citeturn181920view4turn288040view0

### Multiple clones and revisions

Native mgrep makes **clone root** part of file identity but **not Git revision**.

Two clones at:

```text
C:\work\repo\src\a.ts
D:\work\repo\src\a.ts
```

become two independent external IDs.

Conversely, checking out two revisions sequentially in the *same* clone reuses:

```text
C:\work\repo\src\a.ts
```

and `overwrite: true`, so the new content replaces the prior revision's store object. There is no native revision metadata to prevent cross-revision retrieval. citeturn439669view1turn181920view3

A portable adapter should invert those semantics:

```text
identity = (repo_id, revision, relative_path)
```

not:

```text
identity = host_absolute_path
```

For deletion, it should first list only:

```text
repo_id == R AND revision == V
```

and then remove missing `relative_path` entries in **that exact owned namespace**. Never list or delete all objects by `relative_path` alone. Mixedbread's API supports compound metadata filtering for such scoping. citeturn429084search3

### Upload ownership

The adapter should write an explicit ownership marker, for example:

```json
{
  "ingestion_owner": "codeindexer-mixedbread-canary-v1"
}
```

and delete only objects matching all of:

```text
repo_id
revision
ingestion_owner
```

This is an **implementation recommendation**, not a Mixedbread-required field. Its purpose is to guarantee that a lifecycle test cannot delete files uploaded by another clone/tool.

For an existing store already containing native-mgrep absolute-path records, the adapter should **not attempt to reinterpret or delete those records**. A safe migration requires either a separately identifiable namespace inside the same store or a disposable canary corpus. Because this research was instructed not to upload or change stores, no migration was attempted.

### Ignore rules and secrets

Native mgrep's README says it respects `.gitignore` and `.mgrepignore`; source confirms those files are loaded, and hidden path components beginning with `.` are skipped in the recursive non-Git traversal. The default extra exclusions are only:

```text
*.lock
*.bin
*.ipynb
*.pyc
*.safetensors
*.sqlite
*.pt
```

Official source:  
https://github.com/mixedbread-ai/mgrep/blob/main/src/lib/file.ts citeturn181920view8turn439669view3

That is **not a secrets policy**. A tracked `credentials.json`, `.ps1` with embedded credentials, test fixture containing a token, certificate, or plaintext secret can still be eligible unless excluded by repository rules.

For proprietary-code fallback, use a **positive upload allowlist** rather than relying only on ignore rules. Example policy:

```text
include:
  **/*.py
  **/*.js
  **/*.jsx
  **/*.ts
  **/*.tsx
  **/*.ps1
  **/*.psm1
  **/*.md       # only if actually required

exclude:
  **/.env*
  **/*secret*
  **/*credential*
  **/*.pem
  **/*.key
  **/*.pfx
  **/*.p12
  **/node_modules/**
  **/dist/**
  **/build/**
  **/vendor/**
  generated/**
```

Those patterns are **proposed**, not vendor defaults, and must be adapted to the actual repository.

For non-Git directory traversal, source yields only `Dirent.isFile()` entries and recursively enters directories, so direct filesystem symlink entries are not yielded in that path. For Git repositories enumeration switches to the Git abstraction; I did not find a public guarantee covering every symlink/submodule case. **Git-repo symlink behavior remains an empirical test item.** citeturn439669view3

### Query-time revision and exact source validation

Cloud semantic retrieval should **never itself be the final source oracle**. The safe flow is:

```text
local Git/repo state
    ↓
derive repo_id + exact revision + manifest
    ↓
Mixedbread semantic search
    ↓
require returned metadata repo_id/revision
    ↓
map canonical relative_path back to current local repo
    ↓
re-hash local file
    ↓
compare to indexed file_hash
    ↓
read exact local lines/content
    ↓
only then deliver context to Claude Code/Codex/Cursor
```

Mixedbread supports search metadata filters and retrieval of a specific file/chunk. Its management docs show going from a search match's `file_id`/`chunk_index` back to the precise parsed chunk, and its file API also exposes source-file content download. citeturn429084search2turn429084search5

For this workflow, however, **prefer local exact readback** after cloud discovery. That gives the agent the bytes from the active local clone, not merely a potentially stale cloud copy.

A cloud hit should be rejected if:

```text
returned.repo_id != local.repo_id
OR returned.revision != requested.revision
OR returned.relative_path escapes repo root
OR local SHA-256 != returned.file_hash
```

If any check fails, the wrapper should return a structured **STALE/REVISION_MISMATCH** outcome, not content.

## Bounded fallback, privacy, and cost controls

### Retrieval policy

The intended architecture should be:

```text
User/agent
   |
   v
CodeIndexer / exact local tools
   |
   | only explicit semantic miss or ambiguity
   v
small shell fallback
   |
   v
Mixedbread Search
   |
   v
revision/hash validation
   |
   v
exact local source readback
```

No MCP stdio server is needed. No session hook is needed. No auto watcher is needed. All three clients can invoke the same ordinary command.

A fallback should trigger only on an **explicitly classified condition**, for example:

- semantic concept query has no adequate CodeIndexer candidate;
- CodeIndexer returns multiple plausible components and cannot disambiguate;
- natural-language intent cannot be mapped to a symbol/path by local index;
- cross-file conceptual question has insufficient local semantic recall.

It should **not** trigger for:

- known symbol definition/reference;
- exact identifier, literal, filename, or regex;
- ordinary path navigation;
- direct file read;
- a negative/no-answer conclusion that can be established through exhaustive exact/local search;
- routine “find references” work already handled by CodeIndexer/language server.

For exact matching, Mixedbread now also documents a hosted `grep` capability, but using cloud grep where the local exact index already answers the question would add network/cost/privacy exposure without solving the target semantic gap. Current pricing makes grep cheap, not free. citeturn429084search7

### Proposed hard bounds

These are **precommitted engineering limits, not vendor defaults**:

| Bound | Proposed canary limit |
|---|---:|
| Fallback episodes per user task | 1 |
| Semantic queries per fallback | 1 normally; 2 absolute maximum |
| `top_k` | 5 maximum |
| First search rerank | off |
| Optional second search | rerank only if first result set remains ambiguous |
| Successful product calls counted | every successful cloud search, regardless of whether result is later used |
| Timeout | 6 s search; 10 s hard command wall cap |
| Retry | one retry only for transient network/429/5xx; none for semantic/no-result |
| Delivered cloud-derived candidates | ≤ 4 files |
| Delivered local source context | ≤ ~8 KiB or predeclared token ceiling |
| `--answer` / hosted QA | prohibited in canary |
| agentic search | prohibited |
| web search | prohibited |
| automatic watch/sync | prohibited |

The point is to make cloud invocation and serialized context auditable. `mgrep` itself enables reranking by default unless `--no-rerank`/`MGREP_RERANK=0` is used. citeturn546954search0

Given current pricing, starting without rerank is also materially more transparent.

### RU/EN behavior

The `mgrep` README describes the search as multilingual. That is a **vendor claim**, not proof that RU↔EN code retrieval meets this workflow's accuracy requirement. citeturn546954search0

The benchmark therefore needs at least:

- English question → English identifiers/comments;
- Russian question → English code;
- English question → Russian comments/docs where present;
- Russian question → Russian comments/docs;
- symbol-like transliterated/technical vocabulary.

Do not translate RU queries into English silently in the adapter during the first benchmark; that would confound product quality with translation quality. A translation arm can be a separate experiment later.

### Private stores and access control

Mixedbread's Store documentation states that stores are **private by default**; private stores are accessible within the organization, whereas public stores are broadly searchable by users with API access. Public exposure is unnecessary and should never be enabled for this use. Official store docs:  
https://www.mixedbread.com/docs/stores/stores citeturn627323search4

API keys belong to an organization and can have scoped store capabilities. Mixedbread documents distinct scopes for read, write, delete, and search, and those scopes can be limited to explicit stores; requests outside a restricted store list are refused. citeturn918708view7

That supports least privilege in principle:

```text
query client: Search store + perhaps Read store only
sync client: Write/Read on the explicit target store
delete: only if lifecycle synchronizer genuinely needs it
```

But the user requested **no account changes**, so this is a future hardening option, not an action to take now.

**Unknown:** I found no public documentation proving which individual members of the existing organization can access the specific existing private stores; organization membership/roles must be checked separately in the live account if this reaches canary.

### Privacy, retention, training, and region

There is a serious first-party inconsistency.

Mixedbread's current **Terms & Conditions**, Free Tier section, say it collects all Usage Data including queries and other input, stores it natively, and **may use it for model training, analytics, service improvement and research**. The same section says Free-Tier data is retained indefinitely and deletion is not guaranteed. https://www.mixedbread.com/pages/terms citeturn439669view4

Mixedbread's current **Privacy Policy** instead says Free-Tier usage data is used for service improvement, analytics and research but **“not ... for model training”**; it nevertheless states the data is retained indefinitely. https://www.mixedbread.com/pages/privacy citeturn439669view6turn439669view7

For Paid Tier, both sources are materially stronger: Terms say data associated with paid usage is not used for model training or provisioned to third-party services except where legally required; the Privacy Policy says only operationally necessary data is collected and no paid-tier usage data is used for model training. citeturn439669view4turn439669view6

The Terms additionally say that data collected while on Free Tier remains subject to Free-Tier handling after an upgrade unless otherwise agreed in writing. citeturn439669view5

Therefore:

- **Fact:** Free-Tier retention is indefinite in both documents. citeturn439669view4turn439669view7
- **Fact:** training language conflicts.
- **Unknown:** which text Mixedbread would treat as controlling in a dispute without contractual clarification.
- **Decision implication:** do not treat “private store” as equivalent to “not used for training” or “deletable.”
- **Decision implication:** do not upload a fresh proprietary canary merely to resolve technical questions if the existing organization is Free Tier or its legal status is unknown.

Mixedbread says it uses encryption in transit/at rest and access controls, and reports SOC 2 Type II certification for the Security trust-services criterion. Those are **vendor security representations**, not an independent audit performed here. citeturn439669view7

Region is also not an EU-residency guarantee. The Privacy Policy says service data is stored on servers in the **United States**, with certain service providers such as product analytics also in the EU. citeturn439669view7 The pricing page advertises enterprise BYOB/BYOC options, but those are not evidence that the current existing organization is region-pinned. citeturn429084search7

**Current-region guarantee for the user's existing store: unknown.**

Store-file deletion and log retention are separate matters. Mixedbread's file-management docs say deleting a store file permanently removes its original, chunks, embeddings, metadata, indexes and related processing state; the privacy documents simultaneously reserve broader historical usage/log retention. Therefore a file-delete operation should not be represented as proof that all historical query/upload telemetry disappears. citeturn429084search2turn439669view7

### Current public pricing

The **current** public price page, crawled 2026-11-01, states: https://www.mixedbread.com/pricing. citeturn429084search7

| Item | Current public price |
|---|---:|
| Fast indexing | $1.50 / 1M content tokens |
| High-quality indexing | $3.00 / 1M content tokens |
| Semantic search | $4.00 / 1,000 queries |
| Semantic + rerank | $7.50 / 1,000 queries |
| Hosted grep | $0.10 / 1,000 queries |
| Storage | $0.50 / 1M content tokens / month |
| Bandwidth/egress | included |
| Starter plan | $5 one-time credits; no card required |
| Scale plan | $20/month supplied as $20 credits, then PAYG |

The same page estimates ordinary text at roughly **1.4 content tokens per word**. That is a vendor accounting estimate, not a tokenizer guarantee. citeturn429084search7

This supersedes any historical October 27 account/billing observation for purposes of public price assumptions.

The CLI source still contains an error string claiming a “Free tier monthly limit of 2,000,000 store tokens.” That message does not match the current public credit-based pricing documentation, so it should be considered **stale CLI copy, not authoritative current billing policy**. citeturn181920view0turn429084search7

### Cost formulas

Do not invent an actual bill from repository size. Use measured content-token and call counts.

Define:

```text
I_fast = content tokens newly/again indexed in fast mode
I_hq   = content tokens newly/again indexed in high-quality mode
S      = average indexed content tokens stored during billing month
Q_sem  = semantic searches without rerank
Q_rr   = semantic searches with rerank
Q_grep = hosted grep calls
```

Then a public-list-price scenario is:

```text
index_cost =
    1.50 * I_fast / 1,000,000
  + 3.00 * I_hq   / 1,000,000

search_cost =
    4.00 * Q_sem / 1,000
  + 7.50 * Q_rr  / 1,000
  + 0.10 * Q_grep / 1,000

storage_cost =
    0.50 * S / 1,000,000 per month

total_usage_cost =
    index_cost + search_cost + storage_cost
```

Do **not** add `$4 + $7.50` for the same reranked semantic query; the pricing FAQ describes `$7.50/1k` as the reranked semantic rate. citeturn429084search7

The public docs do not expose a separate “file update fee.” Reprocessed content should conservatively be modeled as new indexing consumption until live usage data demonstrates otherwise. **Whether server-side deduplication can suppress all billing for an identical re-upload is not documented clearly enough to budget on that assumption.**

### Credits, delay, and hard spend

Mixedbread's billing docs say usage metrics may lag by **up to 10 minutes** and that the system permits **small overdrafts** to prevent interruptions during spikes. https://www.mixedbread.com/docs/platform/billing citeturn918708view6

The 2026-10-06 changelog documents monthly spending-budget controls applied after included credits, with behavior selectable between pausing API requests or continuing service. Official changelog:  
https://www.mixedbread.com/docs/changelog citeturn429084search6

Thus:

- a **pause-on-budget** control is preferable to “keep serving” for a canary;
- it is still not a mathematically instantaneous hard stop because metering is delayed and small overdrafts are explicitly possible;
- live balance/plan/payment state must be checked separately as requested;
- this report does **not** recommend adding a card, changing tier, or enabling PAYG.

If the existing configuration could incur out-of-credit PAYG and there is no already-active safe pause threshold, **do not run the canary until that is resolved under the user's no-account-change constraint.**

## Falsifiable benchmark and precommitted decision thresholds

The benchmark needs to answer four separate questions:

1. Does the fallback get invoked correctly?
2. Does Mixedbread retrieval actually improve answers?
3. Is cloud state fresh and revision-safe?
4. Does the whole workflow reduce **fleet-local** resource work enough to justify latency, privacy exposure, and cloud cost?

Those must not be collapsed into “the agent used mgrep.”

### Corpus and immutable manifest

Take one representative repository snapshot containing actual languages that exist in the repo, prioritizing:

```text
Python
JavaScript
TypeScript
PowerShell
```

when present.

Freeze a manifest before indexing:

```json
{
  "repo_id": "canonical-stable-repo-id",
  "revision": "full-40-char-git-sha-or-test-snapshot-id",
  "files": [
    {
      "relative_path": "src/foo.ts",
      "sha256": "...",
      "bytes": 12345
    }
  ]
}
```

The allowlist used for cloud upload must be exactly derived from that manifest. No live workspace mutations during a benchmark block.

For dirty worktree testing, create a **separate disposable snapshot identity**. Never attach dirty content to the clean Git commit SHA.

### Question strata

Build the task set before seeing product outputs. Each stratum should include both answerable and deliberately unanswerable cases:

| Stratum | Examples |
|---|---|
| Known symbol | “Where is `FooParser.parse` defined?” |
| Exact literal | config key, error text, CLI option |
| Navigation | caller → callee, entrypoint → implementation |
| Semantic concept | “Where is stale auth state invalidated?” |
| Cross-file semantic | “What prevents another clone's files being deleted?” |
| Ambiguous | multiple plausible implementations |
| No-answer | requested behavior deliberately absent |
| RU semantic | Russian natural-language questions against code |
| EN semantic | equivalent English questions |
| Cross-language | RU question about English symbols/comments |

Known-symbol and exact cases are expected to favor CodeIndexer/exact tools; the point is to ensure the fallback trigger **does not fire unnecessarily**.

### Independent oracle and blinding

Create answers from a source oracle that is hidden from the answer-producing agents. The oracle should record:

```text
correct answer
required supporting files/lines
allowed equivalent answers
critical omissions
known traps
"no answer" ground truth
```

Run two conditions:

```text
A: CodeIndexer-first baseline
B: identical CodeIndexer-first + bounded Mixedbread fallback
```

Counterbalance A/B order per task to reduce warm-cache/order effects.

Answerers must not see condition labels. An independent judge, also blinded to condition, scores only answer quality against the oracle. Ideally use at least one human technical judge for a subset to calibrate any LLM judge.

### Tool-context accounting

The earlier kind of serialized-context failure must be impossible to hide.

For every task, capture **exactly what was delivered to the answering model after serialization**, including:

```text
tool names
JSON wrappers
path metadata
scores
error text
retry messages
empty results
source snippets
line numbers
system wrappers attributable to the tool harness
```

Measure:

```text
delivered tool-context bytes
delivered tool-context tokens using the actual model tokenizer if available
attempted Mixedbread calls
successful Mixedbread calls
failed calls by reason
retrieved chunks
chunks surviving revision/hash validation
```

A run with zero successful product calls is **not a product treatment**, regardless of whether the agent was nominally configured to use it.

### Correctness metrics

Score at least:

```text
fully correct
partially correct
incorrect
unsupported / hallucinated
critical omission
false positive on no-answer task
correct source attribution
wrong-revision source accepted
stale-source accepted
```

For repository QA, **false confident answers and cross-revision evidence should be weighted more heavily than mere omissions**.

Also record retrieval-level metrics independently of final answers:

```text
oracle-support file in top-1
oracle-support file in top-3
oracle-support file in top-5
wrong repo/revision hit returned
wrong repo/revision hit accepted after guard
```

The last metric must be exactly zero.

### Cold/warm performance and resource measurement

Perform **three repeats minimum** per task/condition. Report median and spread; for enough samples, bootstrap paired confidence intervals.

Do not manufacture a “cold” state by stopping production services. A valid cold trial means cold with respect to the candidate process/cache under the normal machine environment while all normal production services continue running.

Measure the **entire process tree**, not just the shell parent:

```text
wall-clock latency
CPU-seconds user + system
peak RSS / working set
idle RSS after invocation
memory-time if feasible (RSS·seconds)
disk reads/writes
network bytes sent/received
GPU utilization and GPU time
VRAM peak
child-process count
orphan process count
```

On Windows, Process Explorer/ETW/PowerShell process counters or equivalent can capture tree metrics; on macOS, `time`, `ps`, Instruments/sample tooling, `powermetrics` where permission permits, and network counters can be used. The exact measurement harness must be fixed before comparing conditions.

The resource unit relevant to the decision is **total fleet-local work**, aggregated over Windows + Intel Mac + Apple Silicon Mac:

```text
fleet CPU-seconds
fleet peak/idle RAM burden
fleet memory-time
fleet GPU/VRAM work
fleet disk I/O
fleet network transfer
```

Moving semantic indexing from one local box to another local box is therefore **not a reduction**. Cloud compute is not counted as local CPU/RAM saving, but its monetary cost and privacy exposure are separate required outputs.

### Attributable cloud cost

Record the billing dashboard before and after a bounded benchmark window, while also calculating expected cost from API-call and content-token logs. Because public docs state metering can lag ten minutes, the benchmark must not use an immediate dashboard delta as proof of zero cost. citeturn918708view6

For each treatment report:

```text
indexed content tokens attributable to canary
storage content-token-days
semantic calls without rerank
semantic calls with rerank
grep calls, if any
documented list-price estimate
eventual observed billing delta
difference / unexplained residual
```

### Proposed KEEP / TRY / PARK thresholds

These are deliberately precommitted **decision rules**, not vendor benchmarks.

**KEEP** only if all of the following hold:

- zero secret/denylisted uploads;
- zero wrong-repo or wrong-revision results accepted;
- zero stale hash-mismatch source passed to an answerer;
- all Windows/Intel-Mac/Apple-Silicon portability cases pass;
- no statistically credible overall correctness harm; paired overall correctness difference must be no worse than **−2 percentage points**;
- on the predeclared semantic-failure/ambiguity subset, fallback improves full-correct rate by at least **+5 percentage points** or reduces critical false answers by a practically meaningful predeclared amount;
- no-answer false-positive rate does not worsen by more than **1 absolute percentage point**;
- at least **10% reduction** in the selected primary fleet-local work metric on the actual target workload mix, with the metric chosen before results are seen;
- warm fallback p50 incremental wall time ≤ **2 s** and p95 ≤ **6 s**, unless answer-quality gain justifies a separately predeclared exception;
- measured cloud cost remains inside the fixed canary allowance and matches logged calls/tokens within an explainable billing-lag tolerance;
- no persistent watcher, orphan process, or meaningful post-command idle-RAM residue.

**TRY** if safety/privacy/revision tests all pass but quality or resource benefit is promising yet uncertain—for example 0–10% fleet-local work reduction, confidence interval spans zero, or semantic quality gain is positive but under +5 pp.

**PARK** if any of these occurs:

```text
privacy terms unresolved for proprietary upload
secret/denied file uploaded
wrong revision accepted
stale file accepted
cross-OS path mismatch persists
overall correctness materially worse
no measurable semantic benefit
fleet-local work unchanged/increased
unbounded retries/context
cost attribution cannot be reconciled
```

These thresholds are intentionally stricter about correctness and data governance than about latency.

Mixedbread's README markets a roughly “2x” token reduction with similar/better judged quality. Treat that only as a **vendor benchmark claim**; it is not evidence for this workflow and should not enter the KEEP threshold. citeturn546954search0

## Strongest case against adoption, reversible canary, and unresolved questions

### Strongest argument against adoption

The strongest argument is not cost. It is **architectural mismatch plus privacy uncertainty**.

Native `mgrep` promises a pleasant “semantic grep” experience by binding cloud records to the machine's local paths and automatically keeping them synchronized. That convenience is almost the opposite of what this workflow requires: a **single canonical, revision-scoped, host-independent store invoked only on demand**.

To fix that safely, the design must bypass the native mgrep ingestion/search contract and implement canonical identity with the underlying API. At that point, `mgrep` contributes little beyond demonstrating Mixedbread's own usage pattern. The candidate is really Mixedbread Search, with a custom safety envelope.

That adds:

- another retrieval system and failure domain;
- proprietary source transfer to a hosted service;
- a current Terms/Privacy contradiction for Free Tier;
- network dependence;
- store freshness and billing state to monitor;
- revision/manifest machinery absent from native mgrep;
- new benchmark and operational code;
- cloud latency and recurring storage/query cost.

Meanwhile, CodeIndexer already exists and remains first-line. If the benchmark finds that CodeIndexer handles nearly all real tasks and the semantic-failure subset is rare, the cloud fallback may increase complexity without reducing meaningful fleet work. This is entirely plausible and must be allowed as a benchmark outcome.

There is also a specific resource-risk point: native one-shot sync scans the repo and can schedule file processing with concurrency 100. It therefore cannot be assumed to reduce local CPU/disk work compared with a CodeIndexer workflow simply because embeddings are computed remotely. citeturn288040view0

### Minimal reversible canary

Subject to the privacy gate and existing-credit guard, the smallest valid canary is:

1. **Do not change the existing native `mgrep` installations, accounts, authentication, agent configs, or private stores.**
2. Use a **disposable local copy** of a small allowlisted repository snapshot for lifecycle tests. This is a future execution step; no copy was uploaded during this research.
3. Before any upload, generate the manifest containing `repo_id`, immutable revision, canonical `relative_path`, SHA-256, and allowlist result.
4. Use only an **explicitly bounded namespace owned by the canary**. If a disposable store cannot be created under the no-new-store rule, do not test destructive sync against the existing shared store; limit the first test to read-only retrieval from pre-existing safe records or postpone it.
5. Ingestion, when permitted, uses canonical external IDs and metadata through the official SDK, never native `mgrep watch`.
6. Query wrapper executes one semantic search with exact `repo_id` + `revision` filter and `top_k <= 5`.
7. Validate hit metadata and local SHA-256 before reading local lines.
8. Deliver only validated local source text to the agent.
9. Log attempted/successful API calls, bytes, context, time, process-tree resources and expected price.
10. Run create/edit/rename/delete tests **only on the disposable namespace/copy**.
11. At end, remove only objects whose `ingestion_owner` and repo/revision identify the canary; never sweep the store by path alone.
12. Disable/remove the shell wrapper to roll back. No daemon, hook, MCP registration, or watcher exists to clean up.

If existing-store policy makes step 4 impossible without risking unrelated records, the correct canary is **not to improvise deletion logic**; it is to PARK lifecycle testing until a disposable isolated store is allowed.

### Concrete rollback

Because the proposed integration is on-demand, rollback should be trivial:

```text
1. Remove the fallback command from the three clients' allowed-tool config.
2. Stop invoking the adapter.
3. Delete only canary-owned cloud records if and when deletion is authorized.
4. Leave existing mgrep installs/auth/stores untouched.
5. Return workflow to CodeIndexer-only.
```

There should be no session process to kill, no watcher to disable, no MCP daemon, and no background state beyond cloud objects explicitly owned by the canary.

### Unresolved questions requiring empirical or account-specific verification

The following could **not** be resolved from public primary sources alone:

| Question | Status |
|---|---|
| Exact current legal tier/contract governing the existing org | **Unknown; account-specific** |
| Current account credit balance/payment setup | **Intentionally not accessed; user will check separately** |
| Whether proprietary uploads in the existing org are covered by Paid-Tier no-training terms | **Unknown until tier/contract confirmed** |
| Exact data residency of the existing store | **Unknown; public default policy points to US storage, not store-specific region** |
| Exact organization member access to the existing store | **Unknown; account-specific** |
| Unicode NFC/NFD normalization of arbitrary metadata/filter strings | **Undocumented; must test** |
| Case behavior beyond documented string-filter semantics on all path examples | **Must test** |
| Git-tracked symlink/submodule behavior in mgrep's enumerator | **Not sufficiently documented for a guarantee** |
| Billing treatment of identical-content overwrite/re-upload | **No adequate public guarantee found; budget conservatively as indexing** |
| Whether an update that produces identical server content tokens incurs zero ingestion cost | **Unknown** |
| Exact SDK version installed alongside the existing 0.1.13 installs | **Not inspected** |
| Whether latest API responses always include enough metadata for local hash validation in every content type | **Code corpus must test** |
| RU↔EN semantic quality for this repository | **Vendor claims multilingual; benchmark required** |
| Apple Silicon behavior of all mgrep hook integrations | **No formal support matrix found** |
| Native mgrep ability to consume a canonical repo-relative store generated by SDK without modification | **Effectively no under current source, because its search filter expects current local path; direct test can confirm** |
| Actual RAM/CPU/GPU savings versus CodeIndexer | **Unknown; benchmark required** |

### Bottom line

**Native `@mixedbread/mgrep` 0.1.13 fails the required portability contract as implemented today.** Its absolute-path `external_id`, absolute `metadata.path`, cwd-derived `starts_with` search filter, absence of revision metadata, and mtime-before-hash freshness shortcut are implementation-level blockers for using one private store safely across Windows and macOS clones. citeturn439669view1turn181920view0turn181920view3turn288040view0

**Mixedbread Search itself can satisfy the portability contract with a minimal SDK/API adapter** because the official API supports caller-defined external IDs, arbitrary metadata, exact compound metadata filters, and source/chunk retrieval. The adapter should canonicalize `(repo_id, revision, relative_path)`, cryptographically hash files, enforce a positive upload allowlist, search only the exact repo/revision, and read exact validated source back from the local clone before supplying agent context. citeturn429084search1turn429084search3turn429084search4turn429084search2

That adapter should be **on-demand shell-only**, shared by Claude Code, Codex and Cursor, with no MCP stdio daemon, no agent installer, no automatic session watcher, no hosted answer generation, and no unbounded search loop.

The remaining adoption blocker is not technical feasibility but **evidence**: privacy terms must be unambiguous for the existing organization, and the controlled benchmark must show real semantic-quality gain and a measurable reduction in total fleet-local work. Until both are demonstrated, the correct position is **TRY-gated / otherwise PARK**, not KEEP.