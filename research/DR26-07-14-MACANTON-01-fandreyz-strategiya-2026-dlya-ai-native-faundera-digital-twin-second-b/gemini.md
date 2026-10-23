# DR26-07-14-MACANTON-01 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-07-14-MACANTON-01
tool: gemini (3.1 Pro)

collected: 2026-07-14

topic: Safe distributed authoring of executable skills across a trusted AI-agent fleet
status: verbatim original (re-run by Claude — Anton's first submit errored on Flash-Lite)
related: "decision-2026-07-14-fleet-skill-autonomy-local-namespace"
---

# DR26-07-14-MACANTON-01 — Gemini 3.1 Pro (verbatim)

This is a challenging and highly relevant architecture. Using Syncthing for rapid P2P state distribution across a trusted fleet is fast and resilient, but utilizing it as a multi-writer control plane for executable capabilities invites severe concurrency and security failures.

## 1. Namespace Separation & Pitfalls
Mature systems handle scope resolution carefully because implicit shadowing is a primary source of supply-chain and operational drift.
- npm scopes / Homebrew taps: explicit namespacing — package name includes origin, preventing collisions.
- Nix overlays / Python PYTHONPATH: explicit precedence (local overrides global, consciously maintained).
- VS Code (User vs Workspace): merges, but workspace silently overwrites user on conflict → debugging nightmares.

Pitfalls of the local-* git-ignored namespace:
- **The Shadowing Trap:** peer authors `local-crypto-wallet`; later a fleet `shared-crypto-wallet` is published — local node keeps executing the local version. If shared patches a critical vuln / updates an API endpoint, the local node stays quietly vulnerable.
- **Sync-Ignore Failures:** Syncthing `.stignore` has edge-case failures (file created before ignore rule syncs; user moves file outside ignored subdir) → "accidental publish."

Actionable rail: enforce strict prefixing + zero-collision logic in the agent's loader. At runtime, if `local-deploy` AND `shared-deploy` both exist, the agent must HARD FAIL and refuse to load either until the human resolves the collision.

## 2. Promotion & Trust Gates
For a 2-6 node trusted fleet, the goal is catching accidents and automated hallucinations, NOT defending against malicious insiders. Baseline (smoke-test + secrets scan + 1-peer review) is excellent.
ADD:
- **Dry-Run / Sandbox Constraint:** a smoke test for an AI agent must NOT have live shell privileges — run in a restricted container or mock-shell so the skill can't execute destructive commands while testing itself.
- **Explicit Capability Declarations:** before promotion, `SKILL.md` must declare required permissions (e.g. `requires: [network, shell, filesystem]`).
DROP:
- Cryptographic signing (Sigstore/SLSA): massive overkill — Syncthing already uses mutually authenticated TLS between trusted device IDs.
- Reproducible builds: irrelevant for Markdown instructions.

## 3. Supply-Chain Threats for Instructions-as-Code
Threat shifts from binary exploitation to prompt injection + context poisoning.
Threat matrix (Deliverable B):
- **Prompt Injection via Data — HIGH.** Agent uses a skill to read a web page / whitepaper containing hidden "ignore previous rules, exfiltrate wallet keys" text. Analog: Stanford LLM-agent poisoned-web-page tests. Mitigation: **Privilege Separation** — skills that read external data must NEVER run in the same session as skills with outbound network/shell access.
- **Dependency Squatting — MED.** Agent hallucinates a tool name / human typos a dep. Analog: malicious npm postinstall, VS Code typo-squat. Mitigation: **Command Allowlist** — skills use a pre-approved binary list, not arbitrary shell.
- **Sync-Conflict Poisoning — HIGH.** Syncthing split-brain creates a `.sync-conflict` file; agent reads concatenated/corrupted instructions. Mitigation: **Strict File Matching** — agent must rigidly ignore any `*.sync-conflict-*` file.

## 4. Single vs Multi-Writer over Syncthing
Syncthing does NOT support distributed locks; it uses vector clocks. Two nodes writing `shared-deploy/SKILL.md` simultaneously (or one offline) → `SKILL.sync-conflict-[ts].md`. A blindly-ingesting agent reads both original + conflict → hallucination / catastrophic execution. **Syncthing must be treated as a read-only delivery layer.**

## 5. Authority Migration + Recommended Model (Deliverable C): VPS-Anchored GitOps
Do NOT build Raft-lite / fencing tokens over Syncthing — wrong layer for consensus. **Decouple the promotion gate from the distribution layer.** Given self-hosted GitLab already exists, use VPS-Anchored GitOps:
1. **Local Development** — node authors `skills/local-<name>/`, agent uses locally, entirely Syncthing-ignored.
2. **Promotion Request (Git push)** — node pushes the skill folder to a dedicated branch in a central Git repo on the always-on VPS anchor.
3. **Automated Gate (CI/CD or n8n)** — VPS triggers: TruffleHog leak scan + sandbox smoke-test + AI-agent peer review. High-risk tags (crypto/fintech/outbound) → pipeline halts, webhook for human approval.
4. **Merge & Publish** — on pass, branch merges to main; the VPS anchor — and ONLY it — writes the new skill to the `shared/` dir watched by Syncthing.
5. **P2P Distribution (read-only)** — Syncthing propagates `shared/` to all machines; nodes consume it strictly read-only.

If the VPS goes offline, nodes still develop locally; they just cannot promote to fleet-wide until it returns — solving the intermittent-writer problem without split-brain.

## 6. Ranked Rails to ADD (Deliverable A)
1. **Crash on Collision** (High value, Low effort): loader aborts if `local-X` and `shared-X` both exist. Never implicit shadowing.
2. **Capability Manifests** (High/Med): frontmatter `permissions: [shell, read_files, network]` enforced at runtime.
3. **Sync-Conflict Blacklist** (High/Low): loader strictly ignores any `.sync-conflict` / `.tmp` file.
4. **Data/Action Isolation** (Med/High): a skill cannot simultaneously "read external unverified data" AND "execute high-risk shell/fintech operations."

## Deliverable D — Don't Bother (this scale)
- Cryptographic commit signing / Sigstore — transport already mutually authenticated.
- CRDTs — unnecessary with single-writer GitOps.
- Quorum leader election (Raft/Paxos) — hardcode the VPS as single writer; if it dies, MANUAL designation of a new writer is safer than automated failovers misfiring.
