---
dr_id: DR26-07-04-ZB-01
title: "Zero-trust security wave in agent platforms: product/positioning for a small agent-farm op"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-ZB-01): Zero-trust security wave in agent platforms: product/positioning for a small agent-farm operator

> Assesses whether the 2026 zero-trust/agent-security wave is monetizable for a 2-3 person agent-farm operator, and if so, what product and positioning to use.

## Ключевые выводы
- Full 'AI security platform' is a bad category to enter — Lakera, Protect AI, Prompt Security, Noma Security, HiddenLayer, Snyk+Invariant already crowd it with runtime defense, red teaming, inventory and governance suites.
- Better opportunity for a small team: narrow, implementation-heavy 'agent deployment hardening, delegated-action control, and auditability' layer, not a broad security brand.
- Adoption/control gap is wide: Gravitee reports enterprise agent estates roughly doubled in 4 months with 48% of production AI agents running unsecured; Darktrace finds 92% of security pros concerned about AI agents; EY finds 66% of enterprises investing/planning to invest in agentic AI and security is now the top supplier-selection attribute.
- Industry converges on a minimal viable control plane: per-agent identity, short-lived/JIT credentials, authorization enforced independently of the model, policy-gated tool/skill registry (supply-chain treatment), egress allowlists, and immutable/structured logging (CSA OpenClaw guide, AWS Agentic AI Lens, Microsoft, Anthropic all align).
- No defense is close to a silver bullet: AgentDojo (97 tasks/629 test cases) and ASB (10 scenarios, 400+ tools, 27 attack/defense methods) show attack success rates above 84% in some settings; more realistic LivePI shows 10.7-29.6% indirect prompt-injection success on live OpenClaw; AgentDyn shows none of 9 benchmark-winning defenses are acceptable for real deployment (either fail on open-ended attacks or destroy utility).
- Aikido found orgs using MORE security tools were MORE likely to suffer incidents — buyers need fewer, better-integrated controls, not another console.
- Standardization (MCP, A2A) helps interoperability and policy enforcement (Microsoft recommends standardizing on both; A2A has 150+ orgs via Linux Foundation, signed Agent Cards) but also enlarges common attack surface — protocol support alone isn't security, only useful paired with signing/registry/identity/enforcement.
- Regulatory environment (FTC focus on false AI claims in the US; EU Cyber Resilience Act + Product Liability Directive) argues against blanket 'we secure agents' claims — narrow, evidence-based claims (what's implemented, what's covered, what remains human) are safer.
- Security-themed education converts only when paired with a concrete operational artifact/next step (Semgrep academy, Snyk open-source program, Okta AI-readiness assessments, Lakera red-team games) — buyers self-educate ~two-thirds of the journey (6sense) but 58% engage sellers earlier specifically to clarify AI claims; EY: 59% say vendors lack case studies, 43% plan to consolidate vendors.
- A2A support matters mainly as a credibility/interoperability signal, not a near-term revenue driver; AP2 (delegated payments) only matters if entering agent-mediated commerce.

## Рекомендации / решения
- Position as 'operator-grade zero-trust hardening for autonomous workflows' / 'agent control plane for small production teams' — never 'AI security vendor' (raises liability/expectation).
- Build a three-layer offer ladder: (1) Assessment SKU — 'Agent Zero-Trust Readiness Audit' scored against 8 domains (identity, secrets, tool registry, approvals, containment, egress, observability, incident response); (2) Implementation SKU — fixed-scope hardening package (per-agent identity, short-lived secrets, policy-gated tool execution, allowlisted/pinned tool-skill registry, outbound allowlists + logging, delegation ledger); (3) bounded managed retainer for policy/registry/telemetry maintenance — not open-ended 'SOC for AI agents'.
- Use off-the-shelf primitives (SPIFFE/SPIRE or cloud workload identity, Infisical/Vault-class secrets, OPA-style policy points, GitHub/CI scanning, OpenTelemetry logs) so the differentiator is orchestration, not reinvention.
- Only make provable claims (implemented controls, blast-radius reduction, approval/provenance coverage, time-to-disable) — avoid 'breach prevention', 'no prompt injection', 'safe autonomy' language.
- Turn existing open-source education funnel into pipeline by pairing each security-themed publication (e.g. 'why your agent is not a user account') with a downloadable artifact (checklist/policy pack/audit worksheet) and a CTA of 'run your architecture through our readiness scorecard', not 'book a demo'.
- Add lightweight A2A credibility (publish/sign a public-facing Agent Card) without heavy protocol productization; skip AP2 unless packaging delegated purchasing/payments.
- Sell in order: audit first (low friction, triangulates pain) → hardening package second (visible artifacts) → retained governance third, only to accounts already dependent on the workflow.
- Keep sanitized publications lagging production and stripped of environment topology/approval-channel specifics — publish patterns, not live control-surface internals.

## Сущности
- **Люди:** —
- **Компании:** Gravitee, Darktrace, EY, 6sense, Lakera, Protect AI, Prompt Security, Noma Security, HiddenLayer, Snyk, Invariant Labs, Semgrep, Okta, Infisical, GitHub, Microsoft, AWS, Google, Anthropic, NIST, OWASP, Linux Foundation, OpenSSF, SPIFFE, CSA (Cloud Security Alliance), Aikido, FTC, Reuters Legal
- **Продукты/инструменты:** A2A (Agent2Agent protocol), AP2, MCP (Model Context Protocol), OpenClaw, AgentDojo, ASB, AgentDyn, LivePI, MalSkillBench, SkillVetBench, SkillFortify, MCP-TDP, Tool-Guard, SPIFFE/SPIRE, OPA, OpenTelemetry, Claude Code

## Открытые вопросы
- No solid public data on conversion rates from security-themed open-source education into paid agent-hardening services — treated as inference from buyer-behavior research, not a settled benchmark.
- Agent-security benchmark/defense literature is moving faster than peer review; most recent findings (AgentDyn, LivePI, MalSkillBench) are not yet battle-tested outside research settings.
- Exact scope/cost of implementing the minimum-viable control stack for Anton's specific agent farm not modeled — needs a concrete internal audit against the 8-domain framework.
- How much A2A/Agent Card signing costs to implement for Anton's public-facing agents is unquantified ('if implementation cost is low').

## Источник
- DR-ID `DR26-07-04-ZB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- agent-identity-zero-trust
- mcp-security
- prompt-injection-benchmarks
- agent-supply-chain-registry
- ftc-ai-claims-liability
- second-brain-northstar
- everything-becomes-content
