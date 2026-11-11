# DR26-11-01-HUB-04 — raw report from novem

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Hilum-specific external deep research — 1 November 2026

## Executive decision

**Decision: PARK for fleet deployment; TRY only as a bounded, isolated canary for named use-cases. Confidence: high on release/source/licensing-status facts, medium on architecture interpretation, low on untested runtime guarantees.**

The strongest reason to **PARK fleet rollout** is not that Hilum is obviously bad; it is that the publicly verifiable evidence is still too thin for the claims that matter in your environment. The public GitHub repository is explicitly a **“Binaries and installer”** distribution repository and currently contains only `README.md`, `install.sh`, `install.ps1`, and `minisign.pub`; it does not expose the engine source tree. That means public availability of binaries, installers, release notes, or auto-generated GitHub “Source code” archives does **not** establish an open-source Hilum engine. ([GitHub repository](https://github.com/hilum-tools/hilum)) citeturn210646view0turn151678view2

The latest published release visible on the public release page is **`v0.1.0-rc.4`**, marked **Pre-release**, released by Aleksandr Ivannikov on **10 September 2026 at 05:52**; the preceding `rc.3` was released on 30 August 2026. The release page still shows no stable `0.1.0` release. ([releases](https://github.com/hilum-tools/hilum/releases), [rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn151678view0turn157140view0turn157140view1

The strongest counterargument is that Hilum appears to be a genuine, nontrivial local product rather than vaporware. Its release notes disclose concrete failure modes, fixes, daemon behavior, embedding persistence issues, Windows limitations, claim locality, and sidecar internals at a level that creates testable hypotheses. Its author also states that the local engine is used daily for multi-agent engineering and that the next version is intended to extend to multiple hosts. Those are **vendor claims**, not independent performance evidence, but they are sufficiently specific to justify a controlled TRY rather than outright rejection. ([author project page](https://ivannikov.pro/ru/projects/hilum-tools/), [LinkedIn](https://ae.linkedin.com/in/an-ivannikov)) citeturn967735search2turn967735search0

My confidence labels throughout this report are:

- **VERIFIED** — directly supported by a current public artifact or primary specification.
- **VENDOR CLAIM** — stated by Hilum/SAKIVA or its author, not independently demonstrated.
- **INFERENCE** — technical interpretation from verified facts.
- **UNKNOWN** — not established by the public material I could verify on 1 November 2026.

For your CodeIndexer-first Windows Hub + Intel Mac Pro + Apple Silicon Mac Studio workflow, the correct question is not “does Hilum work?” but **“does adding Hilum improve correctness/navigation/continuity or reduce total delivered context/time/host resources beyond CodeIndexer + genuine `rg` + native LSP/tree-sitter, under identical budgets?”** Nothing public currently answers that. Resource savings, correctness gains, and total-cost reduction therefore remain **UNKNOWN**, not zero.

## Identity, version, license, platform and supply-chain status

### Product identity and SAKIVA separation

**VERIFIED:** the public GitHub organization is `hilum-tools`, and its only visible public repository is `hilum`, described as “MCP-native code intelligence & memory for AI agents” and, at repository level, as “Binaries and installer.” ([organization](https://github.com/hilum-tools), [repository](https://github.com/hilum-tools/hilum)) citeturn151678view2turn210646view0

**VERIFIED:** the current `sakiva.dev` page describes **SAKIVA** as “an agent-native workspace where AI agents work together across a company's operations,” with software engineering as the first proving ground, and identifies Aleksandr Ivannikov as founder. It does not publish a public technical specification for a shipping multi-host SaaS on that page. ([SAKIVA](https://sakiva.dev/)) citeturn224398view6

**VENDOR CLAIM:** Ivannikov states that the current local version works on one host and that the “second version” enabling multi-host collaboration is in development. He also describes the local engine as written in Rust and providing shared project knowledge, persistent memory, a work queue, and messaging. ([LinkedIn profile/posts](https://ae.linkedin.com/in/an-ivannikov)) citeturn967735search0

**UNKNOWN:** I could not independently establish from current public SAKIVA material a production multi-host protocol, service SLA, authentication model, tenancy model, retention policy, pricing contract, or customer-available multi-host endpoint. Therefore SAKIVA should be evaluated as a **separate future product**, not as proof that local Hilum already offers distributed coordination.

### Release and platform matrix

The current release assets for `rc.4` are unusually easy to verify because GitHub exposes names, sizes, timestamps and SHA-256 digests directly. ([expanded assets](https://github.com/hilum-tools/hilum/releases/expanded_assets/v0.1.0-rc.4)) citeturn996144view3

| Component / platform | Current verified state | Evidence / implication |
|---|---|---|
| Release | **VERIFIED:** `v0.1.0-rc.4`, prerelease, 10 Sep 2026 05:52 | [rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4) citeturn157140view0 |
| Stable release | **Not found on public release page** | [releases](https://github.com/hilum-tools/hilum/releases) lists rc.4 then rc.3 citeturn151678view0 |
| Windows x86-64 | **VERIFIED asset:** 27.4 MB ZIP, SHA-256 `73bd…9851` | [assets](https://github.com/hilum-tools/hilum/releases/expanded_assets/v0.1.0-rc.4) citeturn996144view3 |
| Windows ARM64 | **VERIFIED asset:** 25.8 MB ZIP, SHA-256 `40ca…3c21` | same source citeturn996144view3 |
| macOS Intel | **VERIFIED asset:** 26.9 MB tarball, SHA-256 `10e5…2029` | same source citeturn996144view3 |
| macOS Apple Silicon | **VERIFIED asset:** 25.7 MB tarball, SHA-256 `db49…0b25` | same source citeturn996144view3 |
| Linux x86-64 / ARM64 | **VERIFIED assets** | same source; Linux runtime requires glibc ≥2.35 per release notes citeturn996144view3turn210646view4 |
| Windows runtime testing | **VENDOR DISCLOSURE:** built and checked, but no Windows test suite exercised | [rc.4 limitations](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4) citeturn210646view7 |
| OS signing | **VERIFIED vendor disclosure:** none yet | same source citeturn210646view7 |

The current installers explicitly use GitHub’s `/releases/latest` API when no version is pinned, and both scripts note that this endpoint excludes prereleases. Therefore, with only prereleases published, a default unpinned install path is expected to fail to obtain a stable “latest”; an operator must explicitly request `rc.4`. ([Unix installer](https://github.com/hilum-tools/hilum/blob/main/install.sh), [Windows installer](https://github.com/hilum-tools/hilum/blob/main/install.ps1)) citeturn315840view0turn315840view3

That is a useful maintenance-risk signal: the release process exists, but the nominal “latest stable” installer path and the actually available release state are currently misaligned.

### Checksums, signatures and provenance

**VERIFIED:** `rc.4` publishes each archive, per-archive checksum files, an aggregate `hilum-0.1.0-rc.4-SHA256SUMS`, and `hilum-0.1.0-rc.4-SHA256SUMS.minisig`. ([assets](https://github.com/hilum-tools/hilum/releases/expanded_assets/v0.1.0-rc.4)) citeturn996144view3

**VERIFIED:** the distribution repository publishes a minisign public key whose key ID comment is `EA6FCF55310D8901`. ([minisign.pub](https://github.com/hilum-tools/hilum/blob/main/minisign.pub)) citeturn157140view4

**VERIFIED:** the Unix installer fails closed on a missing checksum manifest or checksum mismatch, but signature verification is conditional on `minisign` being installed. If the signature is present but `minisign` is absent, it explicitly continues after reporting that signature verification was skipped. ([install.sh](https://github.com/hilum-tools/hilum/blob/main/install.sh)) citeturn315840view1turn315840view2

**VERIFIED:** the Windows PowerShell installer validates the archive SHA-256 against the downloaded checksum manifest but contains no `minisign` verification path. ([install.ps1](https://github.com/hilum-tools/hilum/blob/main/install.ps1)) citeturn315840view4turn315840view5turn315840view6

**VERIFIED:** the release commit is GitHub-verified, but the release itself also explicitly states that binaries have **no operating-system signing yet**. A verified Git commit does not replace Apple Developer ID/notarization or Windows Authenticode. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn157140view0turn210646view7

**UNKNOWN:** there is no publicly inspectable build source for the engine in this repository, so reproducible-build verification from source to binary cannot be performed from the public distribution repository.

### Licensing and commercial use

This requires careful separation.

**VERIFIED:** the public repository shows no `LICENSE` file and exposes only four files: README, two installers and the minisign key. GitHub’s repository page also shows Shell as its public language, consistent with it being a distribution repo rather than the claimed Rust engine source tree. ([repository](https://github.com/hilum-tools/hilum), [organization](https://github.com/hilum-tools)) citeturn210646view0turn151678view2

**VENDOR CLAIM:** Ivannikov’s own project page says the Hilum sources are private under **BSL 1.1**, with conversion to **Apache-2.0 on 1 August 2030**. ([author project page](https://ivannikov.pro/ru/projects/hilum-tools/)) citeturn967735search2

That claim does **not** make the public distribution repository open source, and it does not let us determine exact present-day production/commercial rights because the actual Hilum BSL parameter block, Additional Use Grant, and version-specific Licensed Work text are not publicly inspectable in the repository. The standard BSL 1.1 text grants non-production use and permits the licensor to define an Additional Use Grant for production use; exact commercial rights therefore depend on the project-specific parameters. ([BSL 1.1 official text](https://mariadb.com/bsl11/)) citeturn488995search0

**Conclusion:** “commercial use permitted” is **UNKNOWN** from the public distribution artifacts. The old Hilum pricing page says the local stack was free and no-account, but pricing copy is a commercial claim, not the operative software license. ([historical pricing](https://hilum.tools/pricing/)) citeturn937456search0

For a one-host internal evaluation, this is probably a manageable legal uncertainty; for production fleet deployment, resale, embedded distribution, or enterprise use, it is a hard unanswered licensing question.

## Capability evidence versus `rg`, LSP, tree-sitter and CodeIndexer baseline

Hilum’s public marketing and release notes describe several layers under a single surface, but they should not be collapsed into one capability.

`ripgrep` is a genuine line-oriented recursive regex/text search engine. Its own documentation says it recursively searches files for regex patterns, respects `.gitignore` by default, and supports Windows, macOS and Linux. It does **not** claim type resolution, import resolution, or semantic symbol ownership. ([ripgrep](https://github.com/BurntSushi/ripgrep/)) citeturn809898search0

Tree-sitter is a parser generator and incremental parsing library that builds concrete syntax trees and updates them efficiently when source changes. That gives syntax-aware structure, but not by itself whole-program type/import resolution. ([Tree-sitter](https://tree-sitter.github.io/tree-sitter/)) citeturn809898search8

LSP defines a protocol between an editor/client and a language server providing features including go-to-definition and references; the protocol does not guarantee equal semantic quality across languages or servers. ([LSP](https://microsoft.github.io/language-server-protocol/)) citeturn809898search11

Hilum’s older product page describes its Code layer as a hybrid of language servers, tree-sitter and stack-graphs, with TypeScript and Rust shipping “today” and Python/Go/Java/Kotlin targeted later. ([Hilum products](https://hilum.tools/products/)) citeturn937456search1

The critical evidence table is therefore:

| Capability | Current evidence | Status for your benchmark |
|---|---|---|
| Exact text / regex | Baseline `rg` is independently defined and mature | **Baseline capability; Hilum must show additive value** |
| Syntax structure | Tree-sitter can provide fast syntax trees | **Baseline capability if already available** |
| Definition/reference/type navigation | LSP can expose semantic navigation; quality server-specific | **Baseline capability; Hilum must beat or unify it measurably** |
| Hilum symbol navigation | Vendor says definitions/references/call hierarchy through hybrid engine | **VENDOR CLAIM** |
| Hilum semantic search | Release confirms a semantic-query path and model/runtime download | **Existence VERIFIED; quality UNKNOWN** |
| Hilum memory recall | Release says keyword scan only in rc.4 | **VERIFIED limited scope** |
| Cross-file navigation | Release says better with `rust-analyzer`; syntax fallback is less accurate across files | **VERIFIED limitation, Rust-specific evidence only** |
| Python support | Older product page described Python as future through v1.0.0 | **UNKNOWN / not established** |
| JavaScript support | Not clearly enumerated as shipping on current product page | **UNKNOWN** |
| TypeScript support | Claimed shipping | **VENDOR CLAIM; test required** |
| PowerShell support | No public support statement found | **UNKNOWN / likely coverage risk** |
| RU semantic search | No independent/public benchmark found | **UNKNOWN** |
| EN semantic search | No independent/public benchmark found | **UNKNOWN** |
| No-answer calibration | rc.4 fixed one false “fresh/exhaustive empty” composite failure | **Positive fix plus counterevidence that this class existed** |
| Create/edit/rename/delete freshness | No public end-to-end timing guarantee found | **UNKNOWN** |
| Repo/revision/hash identity | No public result contract proving all three found | **UNKNOWN** |
| Cross-OS path portability | No verified contract found | **UNKNOWN** |

A particularly important counterexample is in `rc.4`: the release states that an `explore` call over the daemon without a workspace previously could return an **empty answer labelled fresh and exhaustive** even though its underlying tools had refused the operation. `rc.4` changes that to a typed error and marks failed composites stale/unmeasured. ([rc.4 release notes](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view6turn151678view0

That is encouraging engineering behavior—surfacing epistemic state—but it is also direct counterevidence against assuming a “no result” from Hilum is inherently trustworthy without version-specific testing.

### Semantic model, offline behavior and the 4 GiB contradiction

**VERIFIED:** `rc.4` says the first semantic query downloads two items: an embedding model and the inference runtime; both are cached locally and fetched once. Before that first fetch, semantic search does not work offline; “everything else” is claimed to. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view4

**VERIFIED:** the same release says `hilum memory recall` is **keyword-only**, with no embedding model wired to memory; it ranks by fraction of query words and cannot match synonyms. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view6

So “semantic search” must not be conflated with “semantic memory.” They are separate code paths in this candidate.

The sidecar documentation is internally contradictory:

- In “What changed,” `rc.4` says the multi-vector sidecar’s **4 GiB ceiling is gone**, because offsets moved from 32-bit to 64-bit, and that the writer now streams rows rather than building the file multiple times in memory. ([rc.4 changes](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view5
- In “Known limitations” a few lines later, the same release says a single sidecar **is capped at 4 GiB** and expects roughly twice the sidecar size in memory for the write. ([same release](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view4turn151678view0

**INFERENCE:** the most likely explanation is stale Known Limitations text carried forward after the format-v2/streaming fix. But that is not safe to treat as resolved because the public engine source is unavailable and no independent large-index test is published.

**Status: UNKNOWN.** For your resource constraints, this contradiction is material. No savings claim should be made until actual sidecar size, persistence behavior, model/runtime cache, peak RSS, CPU-seconds and GPU/VRAM are measured on the canary corpus.

### Repository identity, paths and cross-OS behavior

A valid cross-host navigation result should be anchored to **repository identity + revision + relative path + file-content hash**, not just an absolute path. This is a proposed acceptance contract, not a documented Hilum schema.

A Windows result like `D:\repo\src\foo.ts` cannot be called portable merely because a harness string-replaces the prefix with `/Users/anton/repo/`. The Mac must independently resolve the same repository/revision and validate the same file bytes. Case sensitivity also matters: NTFS is normally case-preserving/case-insensitive in typical Windows configurations, while macOS volumes may be case-insensitive or case-sensitive. Therefore `Foo.ts` and `foo.ts`, Unicode normalization, symlinks, aliases and relative imports must be checked against actual filesystem behavior rather than normalized away in scoring.

For every returned source location, the benchmark should require a record equivalent to:

```json
{
  "repo_id": "stable-id",
  "revision": "full-git-sha",
  "dirty_snapshot_id": "hash-or-clean",
  "relative_path": "src/example.ts",
  "file_sha256": "…",
  "line_start": 42,
  "line_end": 58,
  "resolution_kind": "exact|syntax|lsp|semantic",
  "freshness": "fresh|stale|unknown"
}
```

If Hilum does not expose one or more of those fields, that is not an automatic failure, but the missing provenance must remain **UNKNOWN** and be supplied by the external harness without pretending it came from the product.

## Mailbox, task lifecycle and same-host versus multi-host guarantees

The release evidence is strongest for local work queue concepts and weakest for the exact distributed-systems guarantees you care about.

**VERIFIED:** `rc.3` says the agent work queue gained a verification lifecycle in which work may carry acceptance criteria bound when posted, pass through review states, and receive recorded verdicts; it distinguishes “done” from “verified done.” It also says completion of a dispatched worker wakes the orchestrator that dispatched it. ([rc.3](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.3)) citeturn151541view0turn151541view5

**VERIFIED:** `rc.3` and `rc.4` both state that work-claim atomicity holds only **within one filesystem**, that synchronizing `.hilum` between machines is unsupported, and that two synced replicas can each win the same claim. Multi-host coordination is said to arrive with a “server contour,” not filesystem sync. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view8turn151541view1

This means the following distinction is not optional:

| Scenario | Evidence-based status |
|---|---|
| Two independent clients using one host-local Hilum store | **Plausibly supported; must test races** |
| Claude Code + Codex + Cursor sharing one local daemon | **VENDOR-intended; exact process semantics UNKNOWN** |
| Two hosts synchronizing `.hilum` | **Explicitly unsupported** |
| Two hosts competing for one work claim | **Not established by local Hilum** |
| SAKIVA acting as authoritative multi-host coordinator | **Future VENDOR CLAIM / private implementation** |

The public release material does **not** establish a complete state machine for task, lease, ACK and audit. What is actually evidenced versus still unknown is:

| Property | Public evidence | Status |
|---|---|---|
| Task/post | Work queue exists | **VENDOR/RELEASE documented** |
| Acceptance criteria bound at post time | Explicit in rc.3 | **VERIFIED statement** |
| Review states | Explicit in rc.3 | **VERIFIED statement** |
| Recorded verdict | Explicit in rc.3 | **VERIFIED statement** |
| Atomic same-filesystem claim | Explicit limitation | **VERIFIED statement** |
| Lease TTL | No precise contract found | **UNKNOWN** |
| Lease renewal | No precise contract found | **UNKNOWN** |
| Fencing token / generation | No precise contract found | **UNKNOWN** |
| Duplicate submission dedupe | No precise contract found | **UNKNOWN** |
| Idempotency keys | No precise contract found | **UNKNOWN** |
| Durable requester ACK | No precise contract found | **UNKNOWN** |
| No-consumer detection | Presence is only live/in-memory, not durable | **UNKNOWN as mailbox guarantee** |
| Authorization per client | No public contract found | **UNKNOWN** |
| Audit immutability | No public contract found | **UNKNOWN** |
| Crash recovery | Windows stop abandons in-flight work | **Known risk** |
| Presence persistence | Explicitly none | **VERIFIED limitation** |

Presence is particularly easy to overinterpret. `rc.4` says the roster is deliberately live and in-memory and persists nothing. That is not a durable consumer heartbeat or proof that a requester has read a result. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view4turn151541view6

Windows also deserves separate treatment: `hilum daemon stop` is documented as terminating the daemon rather than gracefully asking it to exit, with work in flight abandoned; the daemon handles Ctrl-C, Ctrl-Break, closed console and host shutdown differently. ([rc.4 limitations](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view7

That means your canary must explicitly distinguish:

`posted → claimed → running → result persisted → requester readback → requester ACK`

from merely:

`worker process exited 0`.

A successful CLI exit, an `ACK` printed by the worker, or an internal `done` state is **not** evidence that another client consumed the result.

### Commands that are documented versus experiments that remain necessary

Documented in public release text:

`hilum mcp`, `hilum doctor`, `hilum memory recall`, `hilum memory decision record`, `hilum memory pitfall record`, `hilum embed backfill`, `hilum init`, and `hilum daemon stop` are explicitly named. ([rc.3/rc.4](https://github.com/hilum-tools/hilum/releases)) citeturn151678view0

What I did **not** find in current accessible primary material is a complete, canonical public CLI reference enumerating exact mailbox/task/claim/ACK commands and schemas. The old Hilum docs page says a full CLI/tool reference existed at `docs.hilum.tools`, but that documentation host was not retrievable through the research interface. ([historical docs page](https://hilum.tools/docs/)) citeturn156331search2

Therefore the canary must discover exact command names from the pinned binary’s own `--help`/MCP tool list before use. Do not invent commands from marketing terminology.

The prior NATS KV mailbox you described remains **history**, not evidence about Hilum. The five adopted practices—acceptance-before-code, structured freshness/coverage, real-input red-proof, paired isolated measurements, and additive mailbox—are process disciplines. Likewise the expired 23–31 October NATS trial and the invalid 27 October comparison with zero successful `mgrep` calls do not provide a ranking signal for Hilum.

## Architecture, privacy, network and incremental-use analysis

The public Hilum material describes one binary with both CLI and MCP surfaces, and the author additionally claims stdio and Streamable HTTP support using the Rust MCP SDK. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4), [author post](https://www.linkedin.com/posts/an-ivannikov_hilum-tools-mcp-platform-giving-ai-agents-activity-7491671519480225793-Cj8M)) citeturn157140view0turn967735search1

MCP itself supports stdio for local subprocess-style integrations and Streamable HTTP for HTTP-based integrations; those transports say nothing about Hilum’s own process sharing, authentication or state durability. ([MCP transport material](https://modelcontextprotocol.io/)) citeturn809898search21turn809898search31

For your stated preference—on-demand, no permanent per-session stdio MCP, no new watcher, no heavy embeddings by default—the likely architectural options are:

| Mode | Likely benefit | Risk / unknown | Recommendation |
|---|---|---|---|
| CLI on demand | No permanent MCP process | Cold starts; may still contact/start daemon | **First test** |
| One temporary stdio MCP client | Native agent tool use | Could spawn daemon or duplicate runtime | **Canary only** |
| One host-local daemon shared by clients | Shared index/cache possible | Persistent RAM/CPU/watchers/auth surface | **Only if required** |
| One stdio server per LLM session | Simple client config | Process/cache duplication | **Avoid by default** |
| Local Streamable HTTP | Could share one host service | Bind/auth/origin/session semantics UNKNOWN | **Second-stage test** |
| SAKIVA multi-host | Could centralize coordination later | Private/future; cost/privacy/terms UNKNOWN | **Out of present scope** |

No public benchmark justifies any idle or peak number. Your proposed **≤256 MiB idle incremental RAM per host** and **≤1 GiB peak incremental RAM** are therefore useful experimental gates, not estimates of expected Hilum behavior.

### Telemetry, retention, authentication and offline behavior

The old Hilum pricing page claimed that the free local stack was offline-capable with no account and that source bytes and local indexes did not enter the sync path. It also described paid hosted sync, team memory and fleet coordination. ([historical pricing](https://hilum.tools/pricing/)) citeturn937456search0turn156331search1

Those are **VENDOR CLAIMS**, and the domain’s product positioning has since shifted toward SAKIVA. The current SAKIVA landing page does not provide a privacy policy, data retention policy, technical auth scheme or tenant security model in the content I could verify. ([SAKIVA](https://sakiva.dev/)) citeturn224398view6

Accordingly:

| Question | Current state |
|---|---|
| Local source upload to cloud | Vendor says no; **not independently network-verified** |
| Telemetry | **UNKNOWN** |
| Analytics/crash reporting | **UNKNOWN** |
| Local retention duration | **UNKNOWN** |
| Secure deletion of caches | **UNKNOWN** |
| At-rest encryption | **UNKNOWN** |
| Local HTTP authentication | **UNKNOWN** |
| Per-agent authorization | **UNKNOWN** |
| Hosted SAKIVA retention | **UNKNOWN** |
| Hosted SAKIVA tenancy/RBAC | **UNKNOWN** |
| Semantic cold-start network | **VERIFIED required for model + runtime download** |
| Non-semantic offline behavior | **VENDOR claim: everything else works offline** |

For your data policy, the canary should remain copy-only and allowlisted. Ordinary code may be cloud-eligible, but secrets, cookies, keys and confidential third-party data should never be inside the trial tree. Outbound network should be logged; unexplained egress is a hard failure, not an observation to rationalize afterward.

### Where Hilum may add real value versus noise

The most plausible incremental use-cases are **not** “replace grep.”

First, single-host **multi-client task handoff** is genuinely additive if a Claude Code requester can post a real task, a Codex or Cursor client can independently claim it, produce a source-grounded result, and the requester can later read and ACK that exact result without human prompt shuttling.

Second, **cross-file navigation** may be additive on cases where CodeIndexer + native LSP either misses a relation or requires substantial delivered context. But the current release’s explicit emphasis on `rust-analyzer` means no assumption should be made that Python, JS/TS or PowerShell receive equivalent semantic resolution. ([rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4)) citeturn210646view9

Third, semantic retrieval might help RU/EN intent queries that exact search cannot express, but it is also the feature most likely to violate your “no heavy local embeddings by default” constraint. It should be a later phase, not the prerequisite for proving mailbox or symbol navigation.

Likely noise scenarios include wrapping `rg` without improving correctness, duplicating language-server results under another tool surface, adding persistent daemons only to avoid negligible cold-start cost, or increasing serialized MCP tool output while reducing nominal model-native tokens. Usage counts alone are not benefit; a hundred Hilum calls with unchanged accuracy and higher CPU are negative evidence.

## Bounded isolated canary and preregistered benchmark

### Freeze first

Before any operator-run installation or execution, freeze the trial manifest:

- exact `v0.1.0-rc.4` asset name, archive SHA-256, aggregate checksum file and minisign key fingerprint;
- physical host identity and OS build;
- a **copy** of the real allowlisted repository;
- repository canonical identity, full Git commit, dirty/clean state, and per-file SHA-256;
- two independent LLM clients for the mailbox trial;
- existing CodeIndexer, `rg`, LSP/tree-sitter configuration snapshots;
- network policy and forbidden paths;
- model/client versions, prompts, budgets and included subscription/credit constraints;
- hard gates and KEEP/TRY/PARK criteria below.

Do not mutate existing Claude Code/Codex/Cursor global MCP configuration. Do not place `.hilum` in any synced folder. Do not use the default installer’s moving “latest” path; the release itself is prerelease and the scripts intentionally exclude prereleases unless explicitly pinned. ([installers](https://github.com/hilum-tools/hilum/blob/main/install.sh)) citeturn315840view0turn315840view3

### Canary path

The initial trial should contain exactly two substantive scenarios.

**Mailbox scenario:** one requester submits an actual engineering task with frozen acceptance criteria; a second independent LLM client claims it; it produces a real code-grounded result; the requester independently reads the result; the requester ACKs that result. Preserve task ID, claim evidence, worker identity, result identity, timestamps and readback evidence externally.

Mandatory negative cases: simultaneous double claim, repeated claim, duplicate post, requester absent, worker crash/restart, daemon restart, late stale worker after any lease/TTL behavior, and attempted unauthorized read/claim if the product exposes identity controls. If Hilum has no native ACK, record that as **missing product capability** and use external ACK only for experiment integrity; do not count the external journal as a Hilum feature.

**Navigation scenario:** on the same frozen repo, select real cross-file cases with hidden oracle. Require source verification against file hashes and revision. Test known symbol, exact symbol collision, imported definition, reference, no-answer, wrong-root behavior, and one type-resolution case per language where feasible.

Then perform copy-only freshness mutations:

`create → edit → rename → delete`

Poll every **5 seconds up to 120 seconds**. Record automatic watcher freshness separately from any manual refresh. A deleted file still returned as current at 120 seconds is a hard gate failure. A manual `init` or refresh may demonstrate recoverability, but must not be scored as watcher freshness.

### Shared 24-task benchmark

Freeze a real corpus containing Python, JavaScript, TypeScript and PowerShell where those languages actually exist; any missing language must be explicitly marked missing rather than synthesized and passed off as representative.

Use **24 tasks**: four each of known-symbol, exact, semantic-RU, semantic-EN, navigation and no-answer. Hide the source oracle from answerers; use a blind judge.

Compare:

**A:** CodeIndexer + genuine `rg` + native LSP/tree-sitter + existing allowed tools  
**B:** identical baseline + Hilum

Everything else must match: snapshot, prompt, model, tool budget, system prompt and answer budget. Counterbalance AB/BA across tasks and run **three clean repetitions**, producing 144 primary answer runs before mutation/mailbox extras.

The context budget should remain exactly as you specified: maximum **10 retrieval/read calls** and **24,000 UTF-8 bytes TOTAL serialized delivered tool messages per task**. Count after final serialization and include wrappers, JSON escaping, tool errors, hooks, metadata and retries. If the next message would exceed the remaining byte allowance, withhold the entire message. Record native model tokens separately.

A run in B with **zero successful Hilum calls cannot establish Hilum benefit**, even if the final answer is correct.

Record at minimum:

| Outcome | Required measurement |
|---|---|
| Answer quality | correct / partial / miss / confident false / correct abstention |
| Source integrity | root, revision, file hash, range |
| Tool use | attempted and successful calls by actual product |
| Context | exact serialized UTF-8 delivered bytes |
| Time | wall time, cold and warm separately |
| Memory | idle and peak full process-tree RSS |
| CPU | full process-tree CPU-seconds |
| GPU | utilization and VRAM where measurable |
| Disk | binary, indexes, models, caches, logs |
| Network | destination, bytes, reason |
| Cloud | actual included-credit consumption / receipts |
| Contamination | unrelated host activity during resource run |

Resource trials should be serialized per physical host. If GPU attribution, network attribution or process-tree accounting cannot be measured reliably, mark the value **UNKNOWN**, not zero.

### Critique of the preregistered thresholds

Your gates are directionally strong, but two details deserve explicit treatment.

The **≥90% task coverage** gate on 24 tasks means at least **22 evaluable tasks**. Unsupported languages should count against coverage if they were part of the preregistered corpus; otherwise one can artificially improve coverage by dropping hard cases.

The **correctness decline ≤1/24** criterion is permissive enough to tolerate one regression, but because the sample is small, every transition should be shown individually. A net `+3` correctness improvement could hide three gains plus one new dangerous false positive; your separate “no new confident false answer” condition correctly prevents that.

The proposed decision rule is sound:

**KEEP named use-case only** if all hard gates pass, there are no new confident false answers, correctness decline is at most 1/24, and either median wall time **or total host CPU** improves by at least 20%, **or** correctness improves by at least 3/24 with no more than 20% time increase.

**TRY** if the function works but evidence is mixed, incomplete, underpowered or resource accounting remains uncertain.

**PARK** for hard failure, no incremental benefit, unauthorized cost/egress, insufficient source identity, stale deleted hits, or failure to demonstrate actual independent-client consumption.

Your added resource thresholds—**idle incremental RAM ≤256 MiB/host** and **peak incremental RAM ≤1 GiB**—are reasonable as policy gates, but not evidence-based expectations. Any amendment must be written before the relevant run, with a technical justification. Resource **UNKNOWN** precludes a “saves RAM/CPU/GPU” conclusion.

Installation success, `hilum --version`, an internal `ACK`, process exit code 0, or an `rc.4` release label must never be treated as fleet PASS.

### Rollback

Rollback is intentionally narrow:

stop only experiment-scoped Hilum/MCP/daemon processes and descendants; preserve external logs, hashes, serialized tool messages and judge records; remove only temporary client config and trial storage after evidence capture; verify no watcher/service/daemon remains; verify existing CodeIndexer, Claude Code, Codex, Cursor and the two pre-existing communication channels are byte-for-byte/configuration-equivalent to the pretrial snapshot.

No `.hilum` directory is copied to another host. Cross-OS testing uses the same frozen code snapshot independently on each machine, with matching repository/revision/file hashes; it does not copy or synchronize Hilum’s internal DB.

## Remaining uncertainties and dated primary-source ledger

The unresolved items that materially block a fleet recommendation are: exact engine license grant and commercial-use terms; current SAKIVA terms/privacy/retention/auth; complete Hilum CLI/MCP schema; Python/JS/PowerShell semantic coverage; lease/TTL/fencing/idempotency semantics; requester ACK semantics; per-agent authorization; actual telemetry; HTTP bind/auth behavior; model/runtime identities and licenses; cold/warm resource consumption; and the contradictory 4 GiB sidecar documentation.

The most important live tests are therefore not broad benchmarking but narrowly falsifiable checks: two-client claim race, crash/restart/late-result handling, requester readback, deleted-file freshness, wrong-root no-answer behavior, cross-OS source identity, and process/network accounting with semantic search disabled first.

Primary sources used in this research, all checked against the 1 November 2026 research session where accessible:

| Source | Date/status | What it establishes |
|---|---|---|
| [hilum-tools/hilum](https://github.com/hilum-tools/hilum) | checked 1 Nov 2026 | Distribution repo contents; not engine source citeturn210646view0 |
| [Hilum releases](https://github.com/hilum-tools/hilum/releases) | checked 1 Nov 2026 | rc.4 current visible release; rc.3 history citeturn151678view0 |
| [v0.1.0-rc.4](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.4) | released 10 Sep 2026 | release status, fixes, known limitations, Windows/signing/navigation/offline claims citeturn157140view0turn210646view4 |
| [v0.1.0-rc.3](https://github.com/hilum-tools/hilum/releases/tag/v0.1.0-rc.3) | released 30 Aug 2026 | first installable candidate; verification lifecycle; earlier embedding persistence failure citeturn157140view1turn151541view0 |
| [rc.4 assets](https://github.com/hilum-tools/hilum/releases/expanded_assets/v0.1.0-rc.4) | 10 Sep 2026 | Windows/macOS/Linux assets, sizes, hashes, signed checksum manifest citeturn996144view3 |
| [install.sh](https://github.com/hilum-tools/hilum/blob/main/install.sh) | checked 1 Nov 2026 | stable-latest behavior, SHA-256, conditional minisign verification citeturn315840view0turn315840view1 |
| [install.ps1](https://github.com/hilum-tools/hilum/blob/main/install.ps1) | checked 1 Nov 2026 | Windows asset selection, checksum verification, no minisign path found citeturn315840view3turn315840view6 |
| [minisign.pub](https://github.com/hilum-tools/hilum/blob/main/minisign.pub) | checked 1 Nov 2026 | public signing key material citeturn157140view4 |
| [Hilum GitHub organization](https://github.com/hilum-tools) | checked 1 Nov 2026 | one public repo; Shell public language; domain verification citeturn151678view2 |
| [SAKIVA](https://sakiva.dev/) | checked 1 Nov 2026 | current public SAKIVA positioning and founder identity citeturn224398view6 |
| [Ivannikov Hilum project page](https://ivannikov.pro/ru/projects/hilum-tools/) | indexed 2026 | private-source / BSL 1.1 / Apache-2.0 2030 vendor claim; architecture claims citeturn967735search2 |
| [Ivannikov LinkedIn](https://ae.linkedin.com/in/an-ivannikov) | current posts indexed 2026 | local current version versus multi-host future-version claims citeturn967735search0 |
| [Business Source License 1.1](https://mariadb.com/bsl11/) | official license text | why exact Additional Use Grant matters to production rights citeturn488995search0 |
| [ripgrep](https://github.com/BurntSushi/ripgrep/) | current primary project | genuine `rg` scope and platform support citeturn809898search0 |
| [Tree-sitter](https://tree-sitter.github.io/tree-sitter/) | current official docs | incremental syntax-tree parsing scope citeturn809898search8 |
| [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) | current official docs | go-to-definition/reference protocol scope citeturn809898search11 |
| [MCP official Rust SDK](https://rust.sdk.modelcontextprotocol.io/) | current official docs | stdio and Streamable HTTP transport availability at protocol/SDK level citeturn809898search31 |

The final evidence-weighted position is therefore: **Hilum is a real prerelease local tool with enough concrete engineering detail to justify an isolated TRY, but current public evidence does not establish an open-source engine, stable licensing for your intended commercial use, multi-host correctness, privacy/auth guarantees, broad language-semantic coverage, or fleet resource savings. The most defensible first test is same-host independent-client mailbox handoff plus source-verified navigation, with semantic embeddings disabled until the lighter path proves incremental value.**