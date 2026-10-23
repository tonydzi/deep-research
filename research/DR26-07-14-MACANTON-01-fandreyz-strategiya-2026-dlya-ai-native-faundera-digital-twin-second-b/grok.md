# DR26-07-14-MACANTON-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-07-14-MACANTON-01
tool: grok (SuperGrok, Expert mode)

collected: 2026-07-14

topic: Safe distributed authoring of executable skills across a trusted AI-agent fleet — local namespace, promotion gates, supply-chain trust
status: verbatim original (regenerated once — first pass returned empty)
related: "decision-2026-07-14-fleet-skill-autonomy-local-namespace"
---

# DR26-07-14-MACANTON-01 — Grok (Expert, 28 sources, thought 4m22s)

> Verbatim capture of the Grok response. Prompt = DR26-07-14-MACANTON-01 (see decision note / registry).

Red-Teaming Skill Namespace, Promotion Gates, and Single-Writer Authority for a Small Trusted AI Agent Fleet (2–6 nodes)

The design moves from a single intermittent hub (read-only on peers) to a hybrid model: per-machine skills/local-<name>/ (git-ignored, sync-ignored, freely authored with only a self smoke-test gate) + fleet-wide shared skills under exactly one writer node (the always-on VPS "anchor"). Promotion requires smoke-test PASS + secrets/leak scan + one peer review (human owner consulted only for high-risk: money, irreversible actions, outbound, secrets, or core always-loaded behavior). Reversibility relies on hourly git backup + supersede-not-delete on the anchor.

This is a classic "instructions-as-code" supply-chain problem in a P2P file-sync substrate (Syncthing). Skills are natural-language prompt injections + arbitrary shell execution that auto-propagate. The small trusted fleet reduces some external attacker surface but amplifies insider mistake, compromised single node, and composition risks.

## 1. NAMESPACE — Local/Private vs Shared/Published Separation
Mature systems separate "local override/experiment" from "published/shared" via explicit namespaces, loader precedence rules, and friction on accidental promotion:
- npm: Scopes (@org/pkg) + npm link / local file: deps; published go through registry. Pitfall: accidental npm publish (mitigated by .npmignore + private: true).
- Python: site-packages vs venv / PYTHONPATH / editable -e. Same import name in local path shadows site-packages ("works on my machine"). Prevention: virtualenvs + namespace packages.
- Nix: Overlays + channels vs local overlay.nix/flake inputs; drift explicit.
- git: main + feature branches / worktrees; .gitignore for local/. Accidental commit dominant failure (pre-commit hooks + branch protection).
- VS Code: user/global settings vs workspace .vscode/settings.json; user can shadow workspace; drift common.
- Homebrew taps / Ansible Galaxy / Terraform: local path modules explicit; accidental publish rare (needs separate push).

Common named failure modes: Shadowing · Drift ("works on my machine") · Accidental publish · Name collision on promote.

For a git-ignored local-<name>/ namespace here — prevention rails (value/effort ranked):
1. Loader-level collision detection + explicit precedence (high/med): error/warn if local-foo collides with shared skill; strict rule shared-wins OR fully-qualified names; manifest declaring provenance.
2. Promotion script enforces separation (high/low): copies only local-<name>/ → shared/<name>/ stripping prefix, runs scans, refuses if name already in shared.
3. Pre-commit / promotion hook + Syncthing .stignore (high/low): block staged files under local-*/ from shared git; keep .stignore and .gitignore identical & comprehensive (**/local-*/**, **/.env*).
4. Dependency/env declaration (med/med): optional skill.yaml frontmatter listing required CLIs/env/paths; smoke test warns if missing on target.
5. Regular audit diff (med/low): fleet health check diffs shared vs any local override of same name.

## 2. PROMOTION / TRUST GATES
Proposed gate (smoke + leak-scan + 1-peer, human on high-risk) is mostly sufficient but under-specified on automation + capability scoping. Overkill at this scale: full SLSA L3+, mandatory cosign per artifact, complex canary infra.
- KEEP & strengthen: smoke test (isolated) + secret scan (gitleaks/trufflehog/detect-secrets or LLM) + 1 peer review.
- ADD (high value): automated dangerous-pattern / risk-classification (regex + LLM judge on SKILL.md + embedded scripts): flag `rm -rf`, `curl | bash`, secret access, outbound-without-gate, "processes untrusted input" → block/escalate. Capability declaration frontmatter: requires_shell, shell_allowlist, processes_untrusted_data, risk_level, exfil_risk → gate enforces (shell OFF by default; high-risk → owner sign-off). Explicit supersede metadata + git commit with review note. Optional staged rollout for high-risk (1–2 canary nodes, monitor N hours, then fleet).
- DROP/deprioritize: full Sigstore signing + SLSA provenance (trust already high); reproducible builds (only if skills grow compiled parts); heavy CODEOWNERS / 2-person-per-change.

## 3. SUPPLY-CHAIN THREATS ("instructions-as-code")
Skill = NL prompt injection + arbitrary shell + secret read + auto-sync → strictly more dangerous than a library (execution implicit on load; blast radius fleet-wide on promotion).
Attack classes (table in deliverable b):
- Malicious/buggy shell in promoted skill — analogue: npm postinstall (event-stream 2018, ua-parser-js, colors.js; 2026 AsyncAPI CI compromise), VS Code extension exfil/typosquat. Relevance HIGH. Mitigations: mandatory peer+owner review for shell-containing; command allowlist or sandbox (firejail/bubblewrap/docker); pre-promo static/LLM scan; requires_shell:false default; secret scan blocks.
- Prompt/indirect injection via skill processing untrusted data — analogue: agentic coding tool attacks via honeypot files/MCP output. Relevance HIGH. Mitigations: processes_untrusted_data:true flag; sandboxed/constrained LLM calls + output filter or human confirm; never feed raw external content into shared prompt without isolation.
- Name collision/shadowing — Relevance MED-HIGH if no loader check. Strict naming + collision error + shared precedence + promotion refuses reuse.
- Accidental/malicious secret leak in skill file — Relevance HIGH, scan cheap. gitleaks hard gate; never store secrets in skill files (runtime inject only); pre-commit hook.
- Split-brain during failover — Relevance MED. FS read-only on peers + Receive Only + lease/epoch + human failover.
- Typosquat/attractive-name social eng — Relevance LOW-MED. Content-focused review (not just name); diff review mandatory; tests pass.

Highest-ROI overall (prioritized): (1) shell OFF by default + allowlist/sandbox; (2) capability scoping + declaration enforced by loader; (3) pre-promo secret scan + dangerous-pattern/LLM classifier; (4) mandatory peer+owner for shell/high-risk + static analysis; (5) FS read-only on shared/ for non-writers; (6) audit logging of loads/shell/promotions. Local namespace limits blast radius but local skills still get basic smoke+scan (defense in depth).

## 4. SINGLE-WRITER vs MULTI-WRITER over Syncthing
Single-writer + lease / designated primary strongly preferred (humans must reason "what is current fleet-wide behavior?"). Multi-writer/CRDT = constant conflict + eventual-consistency surprises hard to debug when data is executable behavior.
Syncthing specifics: Receive Only on peers + Send Only on writer = local peer changes don't propagate but device shows out-of-sync + red "Revert Local Changes"; if cluster later changes a locally-modified file, a sync-conflict file is created (cluster wins). No automatic RO enforcement. Split-brain if two Send Only or misconfig during failover.
Recommended: Send Only on anchor + Receive Only on all peers + FS read-only perms (chmod / mount --bind ro) on shared/ tree. Changes only on anchor after gate. Git on anchor = history/supersede/rollback. CRDT/multi-writer adds complexity with little benefit at 2–6 nodes.

## 5. AVAILABILITY / AUTHORITY MIGRATION (writer handoff)
Moving writer intermittent-hub → always-on anchor is right. Hard part = safe failover when anchor unreachable without dual-writer split-brain / stale-leader.
Patterns (practicality ranked): (1) Designated primary + human-coordinated failover w/ explicit lease/epoch file — anchor Send Only if reachable; unreachable > threshold (30–60min) → owner approves promoting a designated backup: verify no other Send Only, update folder types via Syncthing API + restart, write `.leader` {node_id, ts, epoch+1, human_note, fencing_token}, new writer → Send Only; on anchor return reconcile via git, authoritative wins, demote temp, promote anchor. (2) Lease + heartbeat + fencing token (`.leader` refreshed periodically; claim increments epoch; short random backoff + human confirm). (3) Raft-lite — overkill unless already running. (4) Designated tie-breaker — anchor preferred when reachable; home machines secondary w/ increasing timeouts.
Classic mistakes: dual-writer split-brain (both Send Only / racy lease); stale-leader (old anchor overwrites, no fencing/epoch); "arm before green" (new writer writes before old fenced / before propagation); misconfigured folder types persisting.
For this size: human-in-the-loop + documented checklist + lease file + git-as-truth beats auto-Raft. Test failover end-to-end periodically.

## 6. RED-TEAM — top failure modes (by blast radius)
1. Insufficient promotion gate → dangerous/malicious skill fleet-wide (highest). 2. Namespace shadowing/collision → inconsistent/malicious behavior. 3. Writer failover split-brain/stale (misconfig, no fencing, racy). 4. Receive Only + no FS perms → local tampering/dirty state/conflict files. 5. Prompt injection via untrusted-data skill or composition. 6. Secret leak despite scan (encoded/env/dynamic). 7. Git backup/restore incomplete (hourly misses recent promo; restore → vulnerable old version). 8. Syncthing config drift across nodes.

### (a) Ranked rails to ADD (impact × feasibility)
1. Capability declaration + enforcement in SKILL.md frontmatter (requires_shell, shell_allowlist, processes_untrusted_data, risk_level) + loader gating/sandboxing. HIGHEST leverage — kills whole attack classes by default.
2. Automated dangerous-pattern + LLM risk classifier as mandatory pre-promotion.
3. Strict loader collision detection + explicit precedence + promotion refuses name reuse.
4. FS read-only enforcement on shared/ for non-writer nodes (perms or ro bind mount) + Syncthing Receive Only.
5. Pre-promotion secret scan (gitleaks) as HARD blocking gate + git pre-commit hook on anchor.
6. Mandatory git commit on anchor with review metadata on EVERY promotion (not just hourly); supersede old explicitly.
7. Staged canary for high-risk (1–2 nodes first, monitor, then fleet).
8. Fleet health/config audit skill (verify folder types, connected writer, `.leader` presence, no sync-conflict; alert on drift).
9. Audit logging of promotions, skill loads, shell commands (timestamp, node, skill, cmd).

### (c) Recommended single-writer + failover model
Hybrid: Syncthing Send Only anchor + Receive Only peers + FS read-only on shared/ + git source-of-truth on anchor + human-coordinated lease/epoch failover. Normal: all shared changes on anchor after gate; git commit per change + hourly remote backup. Failover (anchor down > threshold): owner approves; designated backup → Send Only; `.leader` epoch+fencing; on return git-reconcile, anchor authoritative, demote backup. Survives offline because existing skills keep working from last sync; new promotions paused/temp-writer-with-approval; git = reconciliation layer; no auto dual-writer.

### (d) DON'T BOTHER at this scale
- Full SLSA provenance / in-toto / mandatory cosign per skill (high effort, low gain in trusted fleet w/ human review).
- CRDT / unrestricted git-merge on every node (conflict/debug burden).
- Fully automatic Raft leader election without human gate (misconfig/partition → split-brain).
- Heavy staged-rollout / progressive-delivery infra (manual promote-to-1-peer + monitor suffices at 2–6 nodes).
- Reproducible builds / SBOM for skills (unnecessary unless compiled deps).
- CODEOWNERS intricate per-path / mandatory 2-person per change (1 peer + owner-for-high-risk is right bar).

Summary: capability-scoped promotion gate + automated scanning + collision-proof namespace + Syncthing Send/Receive Only + FS read-only + git-backed anchor + documented human-coordinated failover w/ lease/epoch. Implement top 4–5 rails first — most risk reduction at modest effort. (28 sources.)
