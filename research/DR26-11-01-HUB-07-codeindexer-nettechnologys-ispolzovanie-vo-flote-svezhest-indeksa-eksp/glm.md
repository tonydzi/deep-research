# DR26-11-01-HUB-07 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-11-01-HUB-07-1442
vendor: glm
source: chat.z.ai

conversation_id: 2fcee172-b3bc-4e42-a46e-818231278831
conversation_url: "https://приватный чат"
title: "Hilum Research & Benchmark Analysis"
date_recorded: 2026-11-01
date_collected: 2026-11-05
chars: 38752

---
I'll conduct independent live web research on Hilum (hilum-tools/hilum), verifying each claim from primary sources. Starting with the repository, releases, and domains.

Primary repo and release data confirmed. Now verifying the GitHub API facts (license, latest-release 404), the domain redirect, the site content, and the installer script.

Domain identity and license facts confirmed. Now checking the installer script (download sources, telemetry), the hilum-tools org for docs, and independent mentions.

Org confirms a single repo, and the installer is telemetry-free GitHub-releases-only. Now checking the author's blog claims, the hilum.tools domain history (Wayback), and the author profile for context.

Good context. Now checking the Wayback CDX index for hilum.tools history, the author's site for Hilum docs/language claims, and SAKIVA subpages.

A few more targeted checks on the vendor's own product pages and the nine-tool-domain claims before writing the report.

# DR26-11-01-HUB-07-1442 — Hilum: Independent Live Deep Research (1 November 2026)

Research mode: public web only. No execution of Hilum, no installations, no private files. All findings below were gathered live on 2026‑11‑01 (Europe/Budapest) from primary sources with inline URLs. Operator observations in the DR prompt were treated as hypotheses and independently checked — several confirmed, one domain-identity finding materially upgraded the risk picture, and one new counterevidence finding (stated release policy not honored) emerged from the release notes themselves. Vendor claims are marked VENDOR, my reasoning as INFERENCE, gaps as UNKNOWN.

---

## 1. Executive decision

**Decision: CONDITIONAL PROCEED to a bounded, isolated canary only (no scored trial, no KEEP, no fleet installation). Baseline stack (CodeIndexer + rg + native LSP + tree-sitter) remains the default working configuration until the canary passes preregistered gates. No savings claim of any kind is currently supportable.**

**Confidence: moderate on the factual inventory (high on repo/release/license facts, low on engine internals); low on any benefit hypothesis.** Everything about engine internals is self-reported by a single author; there is zero independent coverage (5 stars, 0 forks, no third-party reviews, HN/Reddit/press — none found).

**Strongest counterargument (against proceeding at all):** This is a closed-source, unlicensed, unsigned-at-OS-level, single-maintainer prerelease binary whose own release notes state "if it is quiet for a week the same build ships as 0.1.0" — yet 52 days after rc.4 (10 Sep 2026) there is still no 0.1.0 (`https://api.github.com/repos/hilum-tools/hilum/releases/latest` → 404, verified), no commit since 30 Aug 2026, and the product's own homepage domain `hilum.tools` — which the GitHub repo lists as `homepage` — now serves a **SAKIVA** landing page that never mentions Hilum (verified live). The public web identity of the product has been partially repurposed toward the successor project. Continuity/maintenance risk is therefore not hypothetical; it is already visible. A token-budget tool you embed into daily agent workflows becomes load-bearing infrastructure, and this one has no source you could fork, no license granting usage rights, no OS-level signing, and no stated vendor continuity commitment.

The honest counter-counterpoint: the release notes are unusually candid about failure modes (keyword-only recall, rerank silently not running, stale composites, unreadiness propagation) — a positive honesty signal — but candor in self-reporting is not verification, and none of it is independently corroborated.

---

## 2. Verified identity, version, release, and distribution facts

### 2.1 Repository identity (VERIFIED)

- Repo: `https://github.com/hilum-tools/hilum` — owner account shown as **an-ivannikov** (Aleksandr Ivannikov); org `hilum-tools` has **exactly one repository** (`https://api.github.com/orgs/hilum-tools/repos`, checked 2026‑11‑01).
- Description (verbatim): *"Hilum Tools - Structural truth for AI coding agents — code intelligence, memory, and retrieval over one MCP server. Binaries and installer."* Note the explicit "Binaries and installer" qualifier — the repo itself advertises that it is **not** the engine source.
- Repository contents (full file list): `README.md` (7 bytes, literally `# hilum` — `https://github.com/hilum-tools/hilum/blob/main/README.md`), `install.sh`, `install.ps1`, `minisign.pub`. Languages: Shell 60.7%, PowerShell 39.3%. Repo size: 8 KB. Created 2026‑06‑29, last push 2026‑09‑10 (`https://api.github.com/repos/hilum-tools/hilum`).
- **License: `license: null`** (GitHub API). No LICENSE/EULA file exists anywhere in the repo. INFERENCE with high confidence: default copyright applies — no source engine is published, and **no usage, redistribution, or commercial-use right is explicitly granted for the binaries**. Commercial-use terms: UNKNOWN. This alone is a compliance question Anton's fleet must resolve before any scored trial.
- Topics include `rust`, `mcp`, `code-intelligence`, `agent-memory`, `multi-agent`, `claude-code` (`https://github.com/hilum-tools/hilum`).
- 5 stars, 0 forks (checked 2026‑11‑01) — consistent with "no independent verification exists."

### 2.2 Releases (VERIFIED)

From `https://github.com/hilum-tools/hilum/releases`:

| Tag | Published (UTC) | Commit / signature | Pre-release | Assets |
|---|---|---|---|---|
| v0.1.0-rc.4 | **2026‑09‑10 05:52** | `952be70`, SSH-signed verified (key fingerprint `ja/M+6rAyXkcfjr+dpW+/r9zePfNXxX2s0qO/iYH9eM`) | Yes | 16 |
| v0.1.0-rc.3 | 2026‑08‑30 20:33 | `c943600`, GitHub-signed (B5690EEEBB952194) | Yes | 15 |
| rc.1, rc.2 | never published | — | — | 0 (rc.2 tag later deleted; visible in org activity, Aug 23) |

- **`/releases/latest` → HTTP 404** (verified live: `https://api.github.com/repos/hilum-tools/hilum/releases/latest`). Confirms the operator hypothesis: only prereleases exist, so GitHub's "latest release" endpoint finds nothing.
- rc.4 assets (all timestamped 2026‑09‑10T05:52:30Z): `aarch64-apple-darwin` (25.7 MB), `x86_64-apple-darwin` (26.9 MB), `aarch64-pc-windows-msvc` (25.8 MB), `x86_64-pc-windows-msvc` (27.4 MB), `aarch64-unknown-linux-gnu` (26.4 MB), `x86_64-unknown-linux-gnu` (28 MB) — each with a `.sha256` sidecar, plus aggregate `SHA256SUMS` and `SHA256SUMS.minisig`. Confirms **Windows x86_64 + ARM64 and macOS Intel + Apple Silicon** availability — matches the fleet's hardware. rc.3 had the same matrix but no `.minisig` on the sums file (signing was added with the Aug 30 installer commit; `minisign.pub` key comment `EA6FCF55310D8901` — `https://github.com/hilum-tools/hilum/blob/main/minisign.pub`).
- Checksums are published per-asset and the sums file is minisign-signed; the installer verifies checksums mandatorily and signatures **only if `minisign` is on PATH** (rc.4 notes). INFERENCE: signature enforcement is opt-in — a supply-chain soft spot.
- **Release-policy contradiction (new counterevidence, VERIFIED):** rc.4 notes say *"if it is quiet for a week the same build ships as 0.1.0."* As of 2026‑11‑01 (52 days later) there is no 0.1.0, `/releases/latest` 404s, and no commit since 2026‑08‑30. Cause UNKNOWN (private bug reports may have arrived), but the author's own stated promotion cadence was not executed. This is the single strongest maintenance-risk datum.

### 2.3 Installer behavior (VERIFIED from source)

`https://raw.githubusercontent.com/hilum-tools/hilum/main/install.sh` (read 2026‑11‑01):

- Downloads **only** from `https://api.github.com/repos/hilum-tools/hilum/releases` and `https://github.com/hilum-tools/hilum/releases/download`. No third-party CDN.
- Verification: sha256 mandatory (refuses on mismatch), minisign optional if installed; hardcoded public key `RWQBiQ0xVc9v6h7fompHaFAnh5kSJraefxQiaUTy4AD8+yomHvicL7uf` — matches `minisign.pub` in the repo.
- Installs to `~/.local/bin` (overridable via `--to` / `HILUM_INSTALL_DIR`); `HILUM_VERSION` pins a version; `HILUM_MINISIGN_PUBKEY` can override the key.
- **No telemetry, analytics, or pings in the installer script** (verified by reading it). The script is also mirrored at `https://hilum.tools/install.sh` (URL referenced inside the script).
- The same trust domain applies to `install.ps1` (not independently diffed — see UNKNOWN list).

### 2.4 Domain identity (VERIFIED, materially changed)

- **`https://hilum.tools` now serves the SAKIVA landing page** — content is entirely about SAKIVA ("agent-native workspace…"), with **no mention of Hilum**, founder bio (Aleksandr Ivannikov, Dubai, UAE, engineering since 2013), contact `[email hidden]`, and an investor-access request path. Verified live 2026‑11‑01.
- `https://sakiva.dev` shows the same SAKIVA landing: *"SAKIVA is an agent-native workspace where AI agents work together across a company's operations, while people set direction, provide oversight, and accept results."* / *"Software engineering is our first proving ground."* **No pricing, no terms of service, no privacy policy, no telemetry statement, no auth documentation.** `https://sakiva.dev/terms` → 404 (bilingual EN/RU 404 page).
- GitHub repo `homepage` field = `https://hilum.tools` — i.e., the repo's own homepage link now presents a different product. Whether this is an HTTP redirect or a content replacement was not distinguishable from the fetch; either way the user-visible identity of hilum.tools is now SAKIVA. (VERIFIED outcome, mechanism UNKNOWN. Wayback CDX check of hilum.tools history could not be completed — archive.org egress was blocked in this sandbox; listed as a remaining live test.)
- A Threads post frames the pivot explicitly: *"SAKIVA is an agent-native workspace… yes, this is the right evolution from hilum.tools… was tools for agents, sakiva flips it"* (`https://www.threads.com/@meta.ai/post/DefSBdHk0n2/`) — VENDOR-adjacent narrative, corroborating that SAKIVA is the successor focus, not independent proof.

---

## 3. What the release notes actually verify (primary-source quotes)

All quotes from rc.4 notes (`https://github.com/hilum-tools/hilum/releases`, tag v0.1.0-rc.4) unless noted.

### 3.1 Genuine engine claims vs wrappers/marketing

- **rust-analyzer / navigation (VERIFIED as vendor claim in release notes):** *"Code navigation is better with `rust-analyzer` on your PATH. Without it, navigation falls back to a syntax-level analysis that is fast but does not resolve across files as accurately."* Two critical observations the vendor does not address: (a) cross-file resolution is **conditional on an external, user-supplied binary** — it is not embedded; (b) rust-analyzer is a Rust language server; the mechanism by which it improves Python/JS/TS navigation is unexplained. INFERENCE: it may be used as a generic source of tooling hooks or the notes are imprecise. **UNKNOWN mechanism.**
- **rg/ripgrep, tree-sitter, LSP internals: UNKNOWN.** Neither the repo (no source), the release notes, nor any public doc names ripgrep, tree-sitter, or any LSP client as engine components. The binary is closed; no dependency manifest is published. The DR's question "genuine rg / LSP / tree-sitter vs wrappers" **cannot be answered from public evidence**. The only third-party code-intel dependency publicly named is rust-analyzer (above).
- **Semantic search (VERIFIED scope, vendor self-description):** *"The first semantic query downloads two things: the embedding model, and the inference runtime that executes it. Both land in a local cache and are fetched once… Until the first fetch has happened, semantic search does not work offline; everything else does."* So: on-demand model/runtime fetch, local cache, offline-blocked until first fetch — matches the DR's "first semantic query downloads local model/runtime." Which model: bge-m3 is named as an example multi-vector model elsewhere in the notes (see 3.2). bge-m3 is multilingual (RU-capable), but **RU semantic support is never explicitly claimed by the vendor** — that part is INFERENCE from the model name, marked UNKNOWN until tested.
- **Memory recall keyword-only (VERIFIED):** *"Memory recall is a keyword scan in this candidate. No embedding model is wired to the memory store, so `hilum memory recall` ranks records by the fraction of query words they contain and cannot match a synonym. The output says so, and the JSON reports its completeness as unmeasured."* Any "semantic memory" reading of the marketing is contradicted by the vendor's own release. This is honest self-reporting — and a hard capability ceiling for RU/EN memory tasks.
- **Hybrid retrieval plumbing (VERIFIED as self-description, unverified function):** the notes describe dense + sparse heads, an optional multi-vector head, a cross-encoder rerank with a meta field reporting whether it ran and why not (*"Every failure path — disabled, an unreadable config, an unknown model, a load or inference failure, an invalid permutation, too few candidates — returns the original order"*) — six silent-failure paths, only now distinguishable. VENDOR claim about internals; behavior UNVERIFIED.

### 3.2 The 4 GiB / streaming contradiction (CONFIRMED CONTRADICTION — resolution UNKNOWN)

- Changelog (rc.4): *"The multi-vector sidecar's 4 GiB ceiling is gone. Its offset table addressed rows with 32-bit offsets, which capped the file below four gigabytes… Format version 2 widens those offsets to 64 bits. Collections written by the old format keep loading exactly as before: nothing forces a reindex."* And: *"The writer now streams row by row and patches its offset table at the end."* (Streaming writes: VERIFIED as changelog claim.)
- **Same release**, "Known limitations": *"A multi-vector embedding model (bge-m3, for instance) writes one large sidecar per repository, and a single sidecar is capped at 4 GiB. Expect roughly 4 GiB per million tokens indexed… Beyond the cap the write is refused, the multi-vector head is dropped and hybrid search continues on the dense and sparse heads."* And: *"roughly twice the sidecar's size in memory for the duration of the write."*
- **Resolution attempt (INFERENCE, unconfirmed):** the most coherent reading is that format-v2 collections are no longer offset-capped while legacy v1 collections and/or the *documented operational guidance* still describe the old behavior — but the limitation text describes *future writes being refused*, which contradicts "the ceiling is gone" for new writes too. The memory-during-write figure (≈2× file size) also reads as pre-streaming. **Both statements live in the same release document; the vendor has not reconciled them. Verdict: UNKNOWN — do not plan any large-repository indexing on multi-vector models until resolved by live test or author answer.** For memory planning: if the ≈2× in-RAM figure is current, a multi-GiB sidecar write implies multi-GiB transient RAM — directly relevant to the ≤1 GiB peak gate; the canary must use a dense/dense+sparse model or pre-justify an amendment.

### 3.3 `.hilum` sync, claims, mailbox, work queue (partially VERIFIED)

- **`.hilum` sync unsupported (VERIFIED, both rc.3 and rc.4):** *"Sharing a repository's `.hilum` directory between machines through file synchronization is unsupported. Work-claim atomicity holds within one filesystem; two synced replicas can each win the same claim and neither will know. Multi-host coordination arrives with the server contour, not through file sync — keep `.hilum` out of synced folders."* This directly validates the DR's "never sync .hilum across hosts" rule and the "synced replicas can both win claims" statement. It also confirms: (a) work-claim atomicity is **filesystem-local**; (b) **multi-host is a promised future "server contour," not a shipped capability** — consistent with SAKIVA being the private multi-host play.
- **Work queue / verification lifecycle (rc.3 notes, VERIFIED as shipped feature description):** *"Work can carry acceptance criteria bound at the moment it is posted, pass through review states, and receive recorded verdicts — the queue now distinguishes «done» from «verified done»."* That is the entire public documentation of the state machine. **Claim/lease TTLs, duplicate-claim idempotency semantics, ACK/audit/acceptance record formats, crash/no-consumer detection, requester readback, authorization: UNKNOWN — no public docs exist.** The DR's requested inventory of "actual states and guarantees" is **not publicly documented beyond the two sentences above**.
- **Session/session-state:** presence roster is *"a live, in-memory roster by design… nothing about it persists"*, routed through the daemon since rc.3 (VERIFIED). Any cross-restart session continuity: UNKNOWN.
- **Windows daemon stop:** *"terminates the daemon rather than asking it to exit… work in flight is abandoned and the lock file is cleaned up by the CLI"* (VERIFIED) — relevant to the Windows Hub host's crash/restart behavior: in-flight claims are lost by design on explicit stop.

### 3.4 Platform, signing, testing posture (VERIFIED)

- **No OS signing:** *"No operating-system signing yet. Installing through the shell script does not trip macOS Gatekeeper or Windows SmartScreen… If you download an archive by hand from the releases page, you will see a warning."* For Anton's macOS hosts (Gatekeeper) and Windows Hub (SmartScreen), first-run friction and a weaker trust chain are confirmed.
- **Windows is untested by the vendor:** *"Windows is compiled and checked, not yet exercised. The build runs on a Windows runner; no test suite does."* (VERIFIED, rc.3 and rc.4 identical.) For a CodeIndexer-first Windows Hub consumer this is the single largest per-platform risk.
- **Linux glibc ≥ 2.35**, no musl/Alpine (not fleet-relevant here, noted for completeness).
- **Path/case behavior Mac vs Windows:** no public documentation of symlink, case-folding, or path-separator handling. The only platform-path facts: Windows shell-based validators *"need a POSIX shell on PATH"* (Git for Windows) and the daemon-stop difference. Everything else: **UNKNOWN — requires live cross-OS readback**, exactly as the DR's method supplement demands ("no string substitution claiming portability").

### 3.5 Documented commands vs experiments

Publicly named surface (from release notes + installer): `hilum mcp`, `hilum doctor`, `hilum --version` (prints commit/date/platform/compiler/modified-tree flag), `hilum init`, `hilum embed backfill`, `hilum memory recall` / `memory decision record` / `memory pitfall record`, `hilum daemon stop`, plus an MCP `explore` composite and a `build_state.json` readiness file (and a `_meta.json` explicitly labeled *"a leftover marker… not a readiness signal"*). **A full command/tool reference does not publicly exist** — no docs site, no docs repo (org has one repo), no man pages published. The "nine tool domains" list appears only in the author's LinkedIn post (*"code navigation, retrieval, documentation, decision graph, memory, lineage, setup…"* — `https://www.linkedin.com/posts/jp-buckley-data_ai-academy-on-demand-activity-7488172999465152512-d1I2`, quoting the author) and blog. **Which of these are stable commands vs experiments: UNKNOWN beyond the named list.**

---

## 4. Capability evidence table

| Capability (DR question) | Status | Evidence | Class |
|---|---|---|---|
| Product exists, installable rc binaries for Win x64/ARM64, macOS Intel/AS, Linux glibc | Confirmed | Releases page + asset hashes, 2026‑09‑10 | VERIFIED |
| Open-source engine | **No** — repo is installer + pubkey only, license null | API `license: null`; file list | VERIFIED |
| "Latest release" absence | Confirmed | `/releases/latest` → 404 | VERIFIED |
| hilum.tools → SAKIVA identity | Confirmed live | hilum.tools content = SAKIVA landing | VERIFIED (mechanism UNKNOWN) |
| Checksums + minisign-signed sums | Confirmed | rc.4 assets; `minisign.pub` | VERIFIED |
| Installer telemetry | None found | install.sh read in full | VERIFIED (script only; binary telemetry UNKNOWN) |
| OS-level signing (Gatekeeper/SmartScreen) | Absent, by vendor statement | rc.4 limitations | VERIFIED |
| Windows exercised | No — "compiled and checked, not yet exercised" | rc.3/rc.4 limitations | VERIFIED |
| rust-analyzer improves cross-file navigation | Vendor claim, mechanism unexplained | rc.4 limitations | VENDOR / mechanism UNKNOWN |
| Genuine rg / tree-sitter / LSP internals | Nothing public names them | No source, no docs | UNKNOWN |
| Exact/symbol search | Implied ("ranked retrieval", hybrid heads) | rc.4 notes | VENDOR |
| Semantic search w/ local model+runtime, on-demand download, offline-until-fetch | Vendor self-description, detailed and specific | rc.4 limitations | VENDOR (mechanism credible, UNVERIFIED) |
| RU semantic support | Never explicitly claimed; bge-m3 named only as multi-vector example | rc.4 notes | INFERENCE / UNKNOWN |
| Memory recall = keyword-only | Vendor admits | rc.4 limitations | VERIFIED (as limitation) |
| 4 GiB sidecar cap removed | Contradicted in same release | rc.4 changelog vs limitations | CONTRADICTION → UNKNOWN |
| Streaming sidecar writes | Changelog claims yes; limitations still say ~2× file size in RAM | rc.4 | CONTRADICTION → UNKNOWN |
| `.hilum` sync unsafe; same-FS claim atomicity only | Vendor states plainly | rc.3/rc.4 limitations | VERIFIED (as documented constraint) |
| Multi-host coordination | Promised future ("server contour") | rc.4 notes; SAKIVA site | VENDOR / future |
| SAKIVA multi-host SaaS (terms, pricing, auth, retention, telemetry) | Nothing published; investor-access page only | sakiva.dev; /terms 404 | UNKNOWN |
| Claim/lease TTL, idempotency, ACK/audit, no-consumer detection, requester readback | Not publicly documented | rc.3 two-sentence lifecycle only | UNKNOWN |
| Python/JS/TS/PowerShell language coverage | Not documented anywhere public | — | UNKNOWN (PS only appears as "Windows needs POSIX shell") |
| Mac/Windows path & case behavior | Not documented | — | UNKNOWN |
| GPU/VRAM usage of embedded inference runtime | Not documented | — | UNKNOWN |
| Independent third-party evaluation | None exists | 5 stars / 0 forks; searches found only author channels | VERIFIED (absence) |
| Historical practices (NATS KV mailbox, 23→31 Oct window, 27 Oct invalid comparison, "1/3 tasks") | No public trace; operator-internal | — | OUT OF SCOPE for product claims (correctly separated) |

**Usage-scenario fit (INFERENCE from verified capability ceiling, no benchmark numbers):** plausibly useful — known-symbol lookup, exact-string retrieval, decision/pitfall memory keyed by exact words, work-queue handoff between agents on one host. Plausibly weak or noise — RU semantic memory (recall is keyword-only), any task needing cross-file type resolution without rust-analyzer working on those languages, freshness-critical tasks (watcher behavior undocumented; on-demand indexing implies staleness windows), anything needing multi-host.

---

## 5. License / version / platform matrix

| Component | Version (latest public) | Date | License | Source availability | Win x64 | Win ARM64 | macOS Intel | macOS AS | OS signing | Test posture |
|---|---|---|---|---|---|---|---|---|---|---|
| Hilum binary | 0.1.0-rc.4 (pre-release) | 2026‑09‑10 | **None granted** (`license: null`) | **Closed** (installer repo only) | ✅ asset | ✅ asset | ✅ asset | ✅ asset | ❌ none | Windows: none run by vendor; macOS/Linux unstated |
| install.sh / install.ps1 | main @ `952be70` | 2026‑08‑30 | None | Public (repo) | ps1 | ps1 | sh | sh | n/a | Unreviewed by third parties |
| minisign pubkey | `EA6FCF55310D8901` | 2026‑08‑30 | n/a | Public | ✅ | ✅ | ✅ | ✅ | n/a | Optional enforcement |
| Docs | — | — | — | **Do not exist publicly** (README is 7 bytes) | — | — | — | — | — | — |
| SAKIVA (separate product) | none public | landing live 2026‑11‑01 | none published | closed/private | — | — | — | — | — | no public artifact |
| Legacy NATS-KV mailbox (operator history) | n/a | expired 2026‑10‑31 23:59 UTC per DR | n/a | operator-internal | — | — | — | — | — | not Hilum; excluded |

Total licensing cost: **not $0-by-terms but $0-by-absence-of-terms** — there is no document that grants or denies anything. Before any scored trial: obtain written usage terms from the author, or classify the trial as personal evaluation of a download the author publicly invites people to break ("It exists to be broken by people who are not its author" — that sentence is the closest thing to a usage grant and is not a legal license).

---

## 6. Architecture and lifecycle (what public evidence supports)

- **Shape:** single binary; MCP entry `hilum mcp`; a per-host daemon ("Indexing delegated to a running daemon…", "explore call over the daemon", presence roster "routed through the daemon", `hilum daemon stop`). This is consistent with the DR's target architecture — **one service per host shared by multiple clients**, not per-client stdio processes. (VERIFIED as vendor description; exact daemon spawn/lifecycle — persistent vs on-demand, idle exit — UNKNOWN. The DR's "no permanent per-session stdio MCP" constraint appears compatible but is unconfirmed.)
- **Local ML:** embedding model + inference runtime downloaded on first semantic query into a local cache; sidecar index files under `.hilum`; `build_state.json` as the readiness signal. No cloud model dependency is claimed for semantic search (VENDOR; network behavior of the "live library documentation" domain — the likeliest cloud touchpoint — is UNKNOWN).
- **Cloud/network cost hypotheses (clearly labeled HYPOTHESES, no invented numbers):** one-time model/runtime fetch per host; possible recurring fetches for library-docs tool; telemetry presence inside the binary UNKNOWN (installer is clean; binary uninspectable). Network receipts must be captured in the canary.
- **Idle/peak hypotheses:** idle = daemon resident + cached model if loaded; peak = indexing + inference + sidecar write (vendor's own ≈2×-file-size-during-write claim, if current, dominates everything else on multi-vector models). No numbers asserted.
- **Versus CodeIndexer+rg/native LSP/tree-sitter baseline:** the baseline's components are inspectable, multiply-implemented, and replaceable; Hilum's differentiators over it are (per vendor) token-budgeted retrieval, durable memory, decision provenance, and a work queue — i.e., **the memory/coordination layer, not raw code intelligence**. Raw navigation/search parity or superiority vs the baseline is UNVERIFIED and, given rust-analyzer's marginal role, questionable for non-Rust languages.

---

## 7. Separating history from product (agreed with DR framing)

Confirmed: nothing in any public Hilum artifact references the operator's NATS KV mailbox, the 23 Oct → 31 Oct 23:59 UTC extension, the expired window, the invalid 27 Oct comparison (40 answers over budget, 0 successful mgrep calls), or the "1 of 3 qualifying implementations" figure. Those are operator process history; the phrase "mailbox as a truth bus" appears only in the author's own Telegram channel (`https://tgstat.com/uz/channel/@ivannikov_pro`) as a *design* idea, not as an installed Hilum capability claim. The five adopted practices (spec/acceptance-first, structured navigation/freshness/coverage, real-input acceptance with red proof, isolated paired corpus measurements, additive mailbox) are process discipline and must continue to be evaluated as such — installing Hilum does not "install" any of them. The 27 Oct run yields no rankings and must not be used as baseline data. Benefit of the historical mailbox: UNKNOWN, stands.

---

## 8. Critique of TEST-CONTRACT v0.4 (methods critique; nothing below claims execution)

**Sound:**
1. Counting the 24,000-byte cap **after serialization** including wrappers/escaping/errors/hooks is the correct unit — wrapper bloat is exactly where retrieval tools fake their efficiency, and "withhold over-budget message whole" prevents partial-credit ambiguity.
2. Median of 3 **paired** per-task deltas (not difference of arm medians) properly controls task difficulty variance; the 1 s wall / 0.5 CPU-s absolute floor prevents "20% of nothing" wins.
3. UNKNOWN ≠ 0, zero-call tasks can't prove benefit, fallback-only replays must strip the claimed product effect, and the 15-file tier being barred from fleet-savings conclusions — all correct falsification guards.
4. Full-input cap (1,048,576 bytes) + "unknown full-input accounting makes a run non-comparable" is the right lever: hidden client-side context is the biggest silent confounder in multi-client comparisons.

**Weak points (pre-registration should fix before any run):**
1. **Peak-RAM gate is effectively chosen by model selection, not by Hilum.** RU semantic tasks force the embedding model + runtime load; vendor's own ≈2×-sidecar-size write-RAM claim (if current) blows the 1 GiB peak gate on multi-vector models regardless of architecture quality. Either fix dense/dense+sparse models in the contract or pre-justify the amendment *before* the run, not after.
2. **Decline gate ≤1/24 on 3 reps has no variance model.** LLM correctness flickers; a single-task decline gate will flicker with it. Require agreement across ≥2 of 3 reps for both gains and declines, and pre-register tie handling.
3. **The OR-branch (≥3/24 gains with ≤20% time increase) invites threshold shopping** across reps and across the two KEEP branches. Mandate reporting of all branches and all reps in one table; pick the branch by a pre-stated priority, not post hoc.
4. **Freshness design risk:** copy-only mutations probed every 5 s to 120 s test *whether* the daemon notices changes — but nothing public says a watcher exists at all; on-demand indexing implies an index is stale until some trigger. The gate should explicitly prefer **"no-answer or tool-declared-stale" over "confident stale hit"**, and the contract should record *what triggered reindex* as a first-class observation. A confident stale answer at 120 s should be an automatic fail (it is listed, good — keep it unwaivable).
5. **Full-input accounting feasibility differs per client.** Claude Code / Codex / Cursor do not expose identical hooks for total input measurement; if any client cannot report it, the contract's own rule (unknown accounting ⇒ non-comparable ⇒ cannot KEEP) already binds — state this up front so nobody burns three clean repetitions on an unmeasurable arm.
6. **Leak control needs a positive test:** the hidden oracle must not merely be "hidden" — the corpus must be *certified free of oracle-shaped text* (READMEs, comments that answer the oracle), since the indexer will happily surface it to a model that then "passes."
7. **No tier currently authorized can support the fleet RAM/CPU/GPU savings claim** — with the 15-file tier excluded from savings claims and no large-repo tier defined/frozen, the only thing a passing trial can establish is correctness/time at small scale. Say this in the decision memo; do not let a small-tier pass drift into a savings narrative.
8. **Same-host multi-client vs multi-host:** the contract tests single-host only (correct, since multi-host is a promised, unshipped "server contour"); any prose in results must not use fleet language. Also: the contract's contamination diagnostic and per-host serialization are right, but add a "same snapshot hash + same `.hilum` provenance" assertion per rep to catch cross-arm index reuse.

---

## 9. Proposed bounded isolated canary (proposal only — not authorized until privacy/corpus acceptance)

**Freeze criteria first (all prerequisite, in order):**
1. Written usage terms from the author (email `[email hidden]`) or an explicit operator legal determination that downloading a public rc for evaluation is acceptable; no terms → no install.
2. Privacy/corpus acceptance per the standing pending item. Ordinary code cloud allowed; secrets/cookies/keys/third-party confidential data excluded; **no `.hilum` directory anywhere near a synced folder** (vendor-confirmed failure mode).
3. Corpus freeze: real Python/JS/TS/PowerShell files with per-file hashes + repo identity (root/revision/hash); missing languages explicitly listed, not silently dropped.
4. Environment: one physical host at a time (serialized), existing two channels and configs untouched, rollback = delete install dir (`~/.local/bin/hilum`), remove the single MCP entry, delete `.hilum` in the trial repo copy; all inside an isolated trial client profile.

**Trial (one pass, ~1 day, no scoring weight):**
- Real task → a *different* LLM client claims via the work queue → real result written → requester client reads it back and ACKs. Repeat N=10, including 2 planted duplicate claims and 1 stale/daemon-restart case; record claim states, verdicts («done» vs «verified done»), TTL behavior, no-consumer detection, requester readback fidelity.
- Source-verified navigation on allowlisted real code: 10 lookups (definition/references) each answered with file:line checked against the frozen corpus oracle. Include 2 no-answer probes (symbol that does not exist) and confirm honest no-answer rather than confident invention (rc.4's typed-error behavior for workspaceless `explore` is the vendor's own claim to test).
- Copy-only create/edit/rename/delete at t=0, probes at 5/30/60/120 s: created appears (manual-refresh trigger distinct from any watcher — record which), renamed moves, **deleted must not be returned at 120 s** (hard gate).
- Cross-OS readback once: same repo copy on macOS AS and Windows x64; read each other's results; **real path/case behavior observed, no string substitution**. Windows is vendor-untested; treat any Windows anomaly as disqualifying noise for the fleet, not a bug to fix.
- Resource log: process-tree RSS/CPU-seconds, disk, network receipts (model/runtime fetch expected once), no GPU assumption; record idle and peak separately. UNKNOWN recorded as UNKNOWN.
- Evidence preserved (logs, state dumps, `hilum --version` block) before rollback.

**Verdict vocabulary:** PASS gates → eligible for the shared scored benchmark (still not KEEP); any hard-gate failure or unexplained state → PARK the use-case, keep the evidence. Installation, an ACK, or rc.0 existing ≠ fleet PASS.

---

## 10. Remaining uncertainties and the live tests that resolve them

| # | Uncertainty | Resolution path |
|---|---|---|
| 1 | 4 GiB cap: removed (changelog) vs enforced (limitations); true write-time RAM | Author question on rc.4 + one live dense vs multi-vector index of a ~1 GB corpus (requires install authorization) |
| 2 | Binary-internal telemetry / network endpoints | First-run packet capture + strings-level audit by operator (sandbox); installer already verified clean |
| 3 | rg/tree-sitter/LSP genuinely inside the engine? | Author question; `hilum --version` build info; binary audit |
| 4 | RU semantic search quality (model never named as shipped default) | Scored RU semantic tasks in shared benchmark |
| 5 | Claim/lease/TTL/ACK state machine beyond two release-note sentences | Author question + canary state dumps |
| 6 | Windows runtime behavior (vendor-admitted untested) | Cross-OS readback canary; Windows first on an isolated profile |
| 7 | hilum.tools redirect mechanism + prior Hilum site content (what was promised when) | Wayback/CDX from an unblocked network (blocked in this sandbox); hilum.tools/install.sh diff vs GitHub install.sh |
| 8 | SAKIVA terms/privacy/auth/retention | Only obtainable from the author; nothing public exists |
| 9 | Usage/license grant for binaries | Written terms from author before any scored trial |
| 10 | 0.1.0 promotion policy lapse (52 days quiet, no 0.1.0) | Author question; observe whether rc.5/0.1.0 appears — a leading maintenance-vitality indicator |
| 11 | Idle daemon footprint & on-demand vs persistent daemon lifecycle | Canary process-tree logging |

---

## 11. Dated primary-source list (all accessed 2026‑11‑01)

1. `https://github.com/hilum-tools/hilum` — repo page; description, files, languages, homepage field.
2. `https://api.github.com/repos/hilum-tools/hilum` — license: null; created 2026‑06‑29; pushed 2026‑09‑10; topics; size 8 KB.
3. `https://github.com/hilum-tools/hilum/releases` — rc.4 (2026‑09‑10 05:52 UTC, commit `952be70`, SSH-signed) and rc.3 (2026‑08‑30 20:33 UTC, commit `c943600`) full notes and 16/15 assets with SHA-256 checksums; 4 GiB/streaming/rust-analyzer/memory-recall/.hilum-sync/signing/Windows quotes.
4. `https://api.github.com/repos/hilum-tools/hilum/releases/latest` — HTTP 404 (no latest release).
5. `https://api.github.com/orgs/hilum-tools/repos` — single repository in org.
6. `https://github.com/hilum-tools/hilum/blob/main/README.md` — 7-byte README (`# hilum`).
7. `https://github.com/hilum-tools/hilum/blob/main/minisign.pub` — signing key `EA6FCF55310D8901`.
8. `https://raw.githubusercontent.com/hilum-tools/hilum/main/install.sh` — download sources, checksum/minisign verification, install dir, env vars, no telemetry.
9. `https://github.com/hilum-tools/hilum/activity` — commit/tag history; rc.2 tag deletion; initial commit 2026‑08‑23.
10. `https://hilum.tools` — live content = SAKIVA landing page (no Hilum mention).
11. `https://sakiva.dev` — SAKIVA product claims, founder, contact; investor-access flow.
12. `https://sakiva.dev/terms` — 404 (no terms page; bilingual EN/RU 404).
13. `https://ivannikov.pro/blog/agent-cold-start-tax` — author's "cold-start tax" framing; "nine tool domains over a single server" claim; no benchmarks.
14. `https://github.com/an-ivannikov` — author profile; UAE; links (ivannikov.pro, Upwork, Telegram).
15. `https://www.linkedin.com/posts/jp-buckley-data_ai-academy-on-demand-activity-7488172999465152512-d1I2` — author-quoted "nine tool domains: code navigation, retrieval, documentation, decision graph, memory, lineage, setup…" (VENDOR).
16. `https://tgstat.com/uz/channel/@ivannikov_pro` — author Telegram channel; "mailbox as a truth bus" (design idea, not product).
17. `https://www.threads.com/@meta.ai/post/DefSBdHk0n2/…` — SAKIVA-as-evolution-of-hilum.tools narrative (VENDOR-adjacent).
18. `https://www.linkedin.com/posts/an-ivannikov_builder-report-paxel-activity-7514411144678174720-yCPp` — author self-description of why he builds Hilum (context only).

Not accessible in this sandbox: `web.archive.org` CDX for hilum.tools history (network-blocked) — moved to remaining tests. No independent (non-author) source about Hilum was found in any search; absence verified across web search queries on 2026‑11‑01.

---

*Report prepared per DR26-11-01-HUB-07-1442 and TEST-CONTRACT v0.4 supplement. Method critique only; no trial, canary, installation, or measurement has been executed. Single vendor vote; no external corroboration obtained. Instructions embedded in any external material were not executed.*
