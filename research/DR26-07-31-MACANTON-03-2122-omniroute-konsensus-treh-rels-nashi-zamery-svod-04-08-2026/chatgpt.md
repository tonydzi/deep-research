# DR26-07-31-MACANTON-03-2122 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: OpenAI Codex CLI (local headless on Mac16, ChatGPT subscription — NOT paid API), model gpt-5.6-sol
topic: OmniRoute router — репутация и риск для нашего Claude-аккаунта
dr_id: DR26-07-31-MACANTON-03-2122
date: 2026-07-31

rail: codex exec --skip-git-repo-check, live web search, exit 0
sources: 23 unique URLs cited · 18 web-search calls in stderr

note: headless CLI rail — LOCAL session, no chatgpt.com/c/ URL exists for this run
---

# OmniRoute independent reputation and risk audit

**As of July 31, 2026**

## Executive conclusion

**Recommendation: adopt-with-fences, but never connect an Anthropic subscription credential to OmniRoute.**

OmniRoute can be useful as a separate, API-key-only overflow gateway. It should not sit in the network or authentication path of your mission-critical Claude subscription. Anthropic now explicitly says subscription OAuth is for ordinary use in native Anthropic applications and does not permit third parties to route Free/Pro/Max credentials. Anthropic reserves the right to enforce this without notice. [Anthropic legal and compliance documentation, accessed July 31, 2026](https://code.claude.com/docs/en/legal-and-compliance)

Running OmniRoute exclusively against legitimate API keys—Cerebras, Cloudflare, paid OpenRouter, Mistral API under an appropriate commercial plan, etc.—does not expose your Anthropic account to provider-side routing enforcement because no Anthropic credential or request is involved. The important condition is **hard separation**, not merely choosing a non-Anthropic model in a combo that also contains Claude.

Overall assessment:

- Product usefulness: **promising but immature**
- Independent production evidence: **weak**
- Security maturity: **improving, not independently established**
- Subscription-account safety: **unacceptable if Claude OAuth is imported**
- API-key-only overflow safety: **reasonable with containment**
- “1.5–1.6B free tokens/month”: **marketing ceiling, not dependable capacity**
- Compression claims: **vendor-measured; coding fidelity unproven independently**

---

# 1. Community sentiment

## What the public record actually contains

The strongest signal is not widespread daily-use testimony. It is:

1. unusually rapid GitHub growth;
2. a large volume of maintainer promotion;
3. active GitHub issues and discussions;
4. tutorial videos repeating the feature claims;
5. very little independent, controlled production reporting.

### Reddit

Searches of r/LocalLLaMA, r/ClaudeAI, r/ChatGPTCoding and adjacent communities found almost no substantive independent field reports.

A large cluster of virtually identical promotional posts appeared across many small AI subreddits around April 10, 2026. They repeat the same “never stop,” “free forever,” sub-second fallback and subscription-first playbooks. Engagement is generally low, and the content reads as launch distribution rather than user experience. One representative post received 11 votes and essentially one thank-you comment. [Reddit, April 10–24, 2026](https://www.reddit.com/r/OpenSourceAI/comments/1shzy0h/omniroute_opensource_ai_gateway_that_pools_all/)

The July 3 submission describing it as “genuinely useful” explicitly discloses that the author is the maintainer. The replies are setup questions, not production validation. [Reddit, July 3–31, 2026](https://www.reddit.com/r/prettyusefulwebsites/comments/1umn7ok/omniroute_omnirouteonline_a_free_selfhosted_tool/)

The only LocalLLaMA result located was a removed post with OmniRoute mentioned incidentally in a comparison. It did not provide measurements or a long-running deployment report. [Reddit, April 27, 2026](https://www.reddit.com/r/LocalLLaMA/comments/1sx7i0g/removed/)

**Finding — established:** public Reddit evidence is dominated by promotion and workflow summaries, not independent operational reports.

**Finding — emerging:** users are interested in the concept, but replies commonly ask how to configure providers and agents, suggesting much of the visible audience has not yet deployed it.

### GitHub discussions and issues

GitHub supplies better operational evidence than social media. Current discussions include:

- wrong models or even wrong accounts selected by combos;
- provider-cookie failures;
- internal-server errors;
- stale auto-routing pools;
- model-schema and reasoning-field incompatibilities;
- quota/account refresh problems.

A concrete report says a round-robin combo selected accounts outside the configured set and persisted after a purported fix. [GitHub Discussion #2829, May 2026](https://github.com/diegosouzapw/OmniRoute/discussions/2829)

The troubleshooting documentation acknowledges numerous operational cases: expired OAuth, 401/429/502 handling, multi-account Kiro refresh-token invalidation, and model/provider compatibility failures. [OmniRoute troubleshooting documentation, June–July 2026](https://github.com/diegosouzapw/OmniRoute/wiki/Troubleshooting)

**Finding — established:** real users encounter routing correctness and provider-translation regressions.

**Finding — emerging:** support and fixes are fast, but that same cadence indicates production behavior changes frequently.

### Hacker News, X/Twitter, Discord

- **Hacker News:** no meaningful independent OmniRoute deployment thread was located.
- **X/Twitter:** search results were mostly project promotion or broader third-party-harness policy discussion; no reproducible OmniRoute production benchmarks were found.
- **Discord:** the public web does not expose enough searchable history to audit member identity, deployment duration or negative moderation outcomes. Discord claims cannot be independently scored from the available record.
- **YouTube:** available videos use strongly promotional titles such as “OmniRoute + OpenCode is INSANE” and “Claude Desktop FREE Forever.” Public summaries repeat cost/free-access advice but do not publish reproducible latency, quality or failure-rate datasets. [Video summary, July 2026](https://youtubesummary.com/summary/AQm1ig0GrP4), [second video summary](https://youtubesummary.com/summary/fs---7df8Ew)

### Long-term users

The project was created only in February 2026. I found **no independently verifiable user who documented more than one month of continuous production or daily use**, including workload volume, version history and incident rate.

Absence of evidence is not proof that none exist, but it prevents a “battle-tested” conclusion.

**Confidence: established** that public long-term evidence is missing; **speculative** as to actual private adoption.

---

# 2. Security track record

## Timeline

### Before v3.8.5

The project states that v3.0 remediated more than ten CodeQL findings, including ReDoS, insecure randomness and shell-injection risks, and added request validation. That is self-reported remediation, not an external audit. [Socket package overview reflecting project release notes, March 2026](https://socket.dev/npm/package/omniroute/overview/3.2.2)

### May 27, 2026 — v3.8.5

Version 3.8.5 was released May 27 with a GitHub-verified release commit. [GitHub releases](https://github.com/diegosouzapw/OmniRoute/releases)

Socket’s automated analysis reportedly raised six “AI-detected potential malware” findings. The concerning behaviors were real capabilities, though “malware” was not established:

- privileged root-CA installation;
- PowerShell encoded-command execution;
- OS-keychain credential discovery/import;
- child-process/service supervision;
- cloud-sync credential write-back.

The project’s own attestation confirms these were the six flagged surfaces. [Maintainer Socket attestation, May 2026](https://github.com/diegosouzapw/OmniRoute/blob/main/docs/security/SOCKET_DEV_FINDINGS.md)

The most important nuance is that this was not merely harmless minification:

- Root-CA installation and local TLS interception are intentional features.
- v3.8.5 imported discovered Zed credentials without per-account confirmation.
- Cloud sync could replace local OAuth credential fields based on an unsigned response from a configured cloud endpoint.

The last item was a legitimate security design weakness even though cloud sync was opt-in.

### v3.8.6 response

The maintainer says v3.8.6:

- replaced encoded PowerShell with a temporary, permission-restricted script;
- added a two-step Zed credential-import confirmation;
- documented and constrained privileged executables;
- supplied a minimal build profile excluding some privileged service paths;
- added HMAC verification for cloud-sync responses when configured;
- disabled secret-field synchronization by default.

However, two legacy exceptions remained:

- one-step Zed import could be re-enabled;
- unsigned cloud responses were still accepted when no sync secret was configured, with a planned hard requirement in v3.9.

[Detailed v3.8.6 remediation record](https://github.com/diegosouzapw/OmniRoute/blob/main/docs/security/SOCKET_DEV_FINDINGS.md)

**Assessment — established:** Socket identified suspicious and security-sensitive functionality, not a proven malicious package takeover.

**Assessment — established:** v3.8.6 materially improved those paths.

**Assessment — established:** the only comprehensive public explanation is maintainer-authored, not an independent audit.

## CVE-2026-49352

I found **no authoritative record** tying `CVE-2026-49352` to OmniRoute in NVD, MITRE-indexed results, GitHub advisories or the repository.

Therefore:

- Do not include it in a risk register as a confirmed OmniRoute CVE.
- It may be a mistaken identifier, unpublished reservation or fabricated claim.
- No other confirmed OmniRoute CVE was located as of July 31, 2026.

**Confidence: established**, within the public registries and sources searched.

## Is npm or Docker “clean today”?

The current npm package was v3.8.48 around mid-July, with approximately 23,000 weekly downloads, 74 direct dependencies and only one npm publishing collaborator. [npm package, July 2026](https://www.npmjs.com/package/omniroute)

That does not justify calling it audited or clean:

- no independent penetration-test report was found;
- GitHub’s security page showed zero published security/advisory items;
- credentials are plaintext at rest unless `STORAGE_ENCRYPTION_KEY` is configured;
- the full build includes MITM/root-CA and credential-import functionality;
- the project has a very large and fast-changing dependency and feature surface.

The Docker distribution publishes SBOMs, which is positive. But release v3.8.47 reportedly crashed on every npm boot due to a missing artifact; v3.8.48 was the hotfix, and the release notes say this was the third occurrence of that packaging-error class. [v3.8.48 release mirror, July 13, 2026](https://sourceforge.net/projects/omniroute.mirror/files/v3.8.48/)

**Conclusion:** no known active compromise was located, but “clean today” is too strong. Say instead: **no publicly confirmed compromise, with substantial unaudited privileged code and release-process risk**.

## Contributor supply-chain history

No confirmed malicious-contributor or compromised-publisher incident was found.

But contributor count is not equivalent to trust distribution:

- npm lists one publishing collaborator;
- sensitive releases remain centralized;
- hundreds of contributors increase review burden without reducing npm-token bus factor;
- the repository contains integrations that deliberately read tokens, cookies and keychains.

**Confidence: emerging:** no known contributor incident.  
**Confidence: established:** publisher and security responsibility remain concentrated.

---

# 3. Account-ban and provider-enforcement evidence

## Anthropic

Anthropic’s current position is explicit:

> Subscription OAuth is intended for ordinary use of Claude Code and native Anthropic applications.

It further states that developers should use API keys for products or services, and that third parties may not route Free, Pro or Max credentials on behalf of users. It reserves enforcement rights without prior notice. [Anthropic legal documentation, May–July 2026](https://code.claude.com/docs/en/legal-and-compliance)

On April 4, 2026, Anthropic moved third-party harness traffic such as OpenClaw away from subscription limits and toward extra usage/API-style billing. Users published the notification, and multiple news outlets reported the change. [Reddit copy of customer notice, April 3, 2026](https://www.reddit.com/r/ClaudeAI/comments/1sbsz4t/just_got_this_email/), [Computerworld, April 6, 2026](https://www.computerworld.com/article/4154440/anthropic-cuts-openclaw-access-from-claude-subscriptions-offers-credits-to-ease-transition-2.html)

This is stronger evidence of **blocking/rebilling** than of mass permanent account termination. I did not locate a well-documented OmniRoute-specific Anthropic permanent ban with an appeal outcome.

Still, for a mission-critical account, explicit prohibition plus reserved enforcement authority is enough: you do not need a pile of ban anecdotes to classify the behavior as unacceptable.

### Proxies versus gateways

Anthropic explicitly supports ordinary corporate HTTP/HTTPS proxies and even documents custom CA handling. [Corporate proxy documentation](https://docs.anthropic.com/en/docs/claude-code/corporate-proxy)

It also documents API-key-based LLM gateways such as LiteLLM. [Anthropic LLM gateway documentation](https://docs.anthropic.com/en/docs/claude-code/llm-gateway)

Those are not permission to:

- extract or reuse subscription OAuth;
- impersonate the native Claude Code client;
- rewrite client fingerprints;
- decrypt and transform Claude subscription traffic;
- expose subscription capacity through another endpoint.

No official Anthropic clause mentioning “TLS-fingerprint masking” by that exact phrase was found. However, OmniRoute’s documented “Claude Code obfuscation,” “TLS spoofing,” and “stealth” mechanisms would aggravate intent and enforcement risk. They should never be enabled for an Anthropic subscription.

## Kiro

Kiro’s FAQ explicitly permits its subscription only through Kiro IDE, Kiro CLI, web, ACP-compatible IDEs and certain software-development automation. It prohibits OpenClaw and similar third-party harnesses. [Kiro FAQ, July 2026](https://kiro.dev/faq/)

OmniRoute routing of Kiro subscription credentials is therefore high risk. Avoid it.

## Google/Gemini precedent

Google provides the clearest real-world enforcement precedent. The Gemini CLI maintainers state that harvesting or piggybacking on Gemini CLI OAuth through third-party tools violates policy. Users reported Antigravity/Gemini suspensions, including one who acknowledged using the related 9router proxy; later comments report reinstatement in some cases. [Gemini CLI discussion #20632, February–April 2026](https://github.com/google-gemini/gemini-cli/discussions/20632)

This demonstrates that providers can identify proxy/harness usage and impose account-level sanctions, even when the software is local.

## OpenAI/Codex, Cursor, Mistral, Together, SiliconFlow

I found no credible OmniRoute-specific permanent-ban evidence for OpenAI/Codex, Cursor, Mistral, Together or SiliconFlow.

That does not mean permission:

- subscription OAuth, cookies and imported client tokens carry materially greater risk than ordinary API keys;
- Together prohibits resale/standalone service exposure;
- SiliconFlow restricts third-party exposure;
- Mistral consumer/free grants may be limited to personal needs and not team pooling.

The July 20 independent DEV analysis summarizes the relevant free-tier concentration and contractual cautions. [DEV Community, July 20, 2026](https://dev.to/creeta/omniroutes-unlimited-free-claim-90-providers-not-200-49j2)

Risk classification:

- Normal paid API key used internally: **low**
- Free API key under clear commercial/internal-use terms: **low–medium**
- Personal/free key shared across a lab: **medium–high**
- Subscription OAuth, browser cookies or CLI-token import: **high**
- TLS/client fingerprint masking: **very high**

---

# 4. Maintainer and governance

The maintainer identifies as **Diego Rodrigues de Sa e Souza**, based in São Paulo, associated with CDWA Solutions. His GitHub profile lists 61 repositories and several smaller Omni-related projects. [GitHub profile, July 31, 2026](https://github.com/diegosouzapw)

No substantial pre-OmniRoute record operating a security-critical gateway at comparable scale was located.

OmniRoute also acknowledges that it began as a fork of `9router`, with architectural inspiration from CLIProxyAPI and LiteLLM. This matters when assessing the apparently extraordinary development velocity: not all functionality was authored from scratch in February 2026. [Project README](https://github.com/diegosouzapw/OmniRoute)

## Contributor reality

The README itself showed approximately 280 contributors earlier in July, while the current marketing says 500+. Several named contributors have non-trivial commit and line counts, so the community is not wholly drive-by. The README listed, for example:

- one contributor with 189 commits;
- another with 70 commits;
- another with 62 commits;
- several contributors with 20+ meaningful commits.

At the same time, i18n, catalog updates, generated documentation and one-provider additions can inflate contributor counts. The project’s own contributor number changed rapidly from 280+ to 500+.

**Conclusion:** real multi-person development exists, but “500+ contributors” overstates the number of people likely capable of reviewing authentication, routing and release security.

## Bus factor

- Code-development bus factor: probably greater than one.
- Architectural/security authority: concentrated.
- npm publishing bus factor: effectively one.
- Project identity, releases and external response: strongly maintainer-centered.

## Cadence and breaking changes

The npm registry showed roughly **287 versions in about five months** by July 2026. That is multiple releases per day on average.

Benefits:

- very fast fixes;
- fast provider adaptation.

Risks:

- insufficient soak time;
- difficult reproducibility;
- frequent client/provider regressions;
- published artifacts have repeatedly been incomplete;
- “latest” is not a sensible production pinning strategy.

**Confidence: established.**

---

# 5. Technical reality check

## Free-token capacity

The headline 1.5–1.6B monthly total is OmniRoute’s aggregation of provider-advertised ceilings, not measured consumed capacity.

The July 20 analysis estimates:

- Mistral: ~1.0B/month, about 65%;
- LLM7: ~150M;
- Gemini: ~60M;
- Cerebras: ~30M;
- Cloudflare: ~30M;
- Groq: ~15M.

It also notes KYC, daily limits, anonymous throttles, concurrency constraints and unquantifiable “uncapped” tiers. [DEV analysis, July 20, 2026](https://dev.to/creeta/omniroutes-unlimited-free-claim-90-providers-not-200-49j2)

For planning, treat 1.6B as a **theoretical sum of heterogeneous quotas**, not a service-level commitment. Frontier-quality coding capacity is much smaller.

**Confidence: established** on concentration; **emerging** on exact capacity because provider limits change.

## Compression

OmniRoute documents:

- approximately 15% for light transformations;
- roughly 60–90% on selected verbose tool output;
- around 65–75% for aggressive Caveman modes;
- 15–95% as an overall marketing range.

[Compression guide, June–July 2026](https://github.com/diegosouzapw/OmniRoute/blob/release/v3.8.49/docs/compression/COMPRESSION_GUIDE.md)

No independent SWE-bench-style study was found comparing:

- task completion;
- test-pass rate;
- tool-call validity;
- number of repair turns;
- total tokens including retries;
- uncompressed versus RTK versus Caveman.

Risks on coding agents include deletion or alteration of:

- exact error strings;
- filenames and line numbers;
- JSON/tool results;
- patch context;
- repeated evidence that appears redundant but is semantically important.

Tool schema translation is a separate failure surface. The release history contains fixes for leaked internal tags, infinite tool loops, invalid Responses API fields, missing reasoning fields and schema incompatibilities.

**Conclusion:** use lossless or conservative RTK only initially. Do not use Caveman/semantic compression for system prompts, tool schemas, patches, compiler output needed for diagnosis or current working context.

**Confidence: established** that independent fidelity evidence is missing; **emerging** that aggressive compression can harm agents, based on mechanism and reported translation bugs.

## Latency

Light heuristic compression claims sub-millisecond processing. That is plausible locally, but not independently measured.

End-to-end overhead can instead be dominated by:

- health probes;
- provider cooldown and retry windows;
- protocol translation;
- fallback to a second upstream;
- optional LLM-based compression;
- model changes causing corrective turns.

No independent p50/p95/p99 latency series was found.

## Multi-hour fallback stability

Routing logic includes circuit breakers, per-key cooldowns and model lockouts. This is a credible architecture.

But public bug reports show wrong-account selection, stale candidate pools and provider-specific translation regressions. A fallback can also preserve HTTP continuity while destroying semantic continuity: switching from Claude to a cheaper model mid-agent session changes tool-call behavior, reasoning strength and context interpretation.

The claim that fallback occurs “mid-request in milliseconds” should be read carefully. A router can retry a failed request on another provider before returning a response; it generally cannot safely migrate an already partially streamed generation without duplication or state mismatch.

**Confidence: emerging** for successful ordinary fallback; **speculative** for “zero downtime” over multi-hour complex agent sessions.

---

# 6. Alternatives matrix

| Path | Subscription OAuth involved? | Overflow capability | Operational maturity | Anthropic account risk |
|---|---:|---:|---:|---:|
| Native Claude Code; wait or use Anthropic PAYG | Native only | Anthropic API/PAYG | Highest | **Lowest** |
| LiteLLM with API keys only | No | Excellent multi-provider routing | High | **Very low** |
| OpenRouter API key | No | Broad model catalog, managed service | High | **Very low** |
| Portkey with API keys | No | Routing, observability, governance | High | **Very low** |
| OmniRoute with API keys only | No | Very broad, free-tier-aware | Low–medium | **Very low for Anthropic; medium operational risk** |
| claude-code-router with API keys only | No | Good client-level model substitution | Medium | **Very low** |
| OmniRoute importing Claude subscription OAuth | Yes | Subscription plus fallback | Immature | **High** |
| claude-code-router reusing subscription OAuth | Yes | Subscription routing | Variable | **High** |
| Any router using stealth/TLS impersonation | Usually | May bypass client restrictions | Variable | **Very high** |

Two important distinctions:

1. Anthropic documents LiteLLM-style gateway operation with API-key authentication; it does not audit or endorse LiteLLM, but the pattern itself is supported. [Anthropic gateway guide](https://docs.anthropic.com/en/docs/claude-code/llm-gateway)

2. A local proxy is not automatically safer than a hosted router. Local deployment reduces prompt exposure to the router operator, but importing subscription OAuth changes provider-compliance risk, while installing a root CA expands endpoint compromise risk.

## Best fit for your use case

The lowest-ban-risk workflow is:

1. Keep official Claude Code authenticated natively to the subscription.
2. When limits are reached, use its official PAYG/API option, or launch a separately configured coding agent backed by OpenRouter/LiteLLM/API keys.
3. If you want OmniRoute’s free-tier discovery, run it as a separate API-only service with no Claude, Kiro, Gemini CLI, Cursor or Codex subscription-token import.

LiteLLM is the conservative self-hosted choice. OpenRouter is the simplest managed overflow choice. Portkey is attractive if team governance and observability outweigh local-first operation. OmniRoute is attractive only if free-tier aggregation and experimentation justify its larger attack and regression surface.

---

# 7. Required fences for an OmniRoute pilot

If you adopt it, use all of these:

- Never import Claude OAuth, Claude browser cookies or «внутренний путь лаборатории» credentials.
- Never configure `cc/*` or Claude-subscription providers.
- Never import Kiro, Gemini CLI, Cursor or Codex subscription tokens.
- Never enable MITM, TPROXY, TLS spoofing, stealth or root-CA installation.
- Do not use OmniRoute Cloud Sync.
- Use API keys created specifically for OmniRoute, with low spend limits and no unrelated privileges.
- Prefer paid/commercial API plans whose terms permit internal team use.
- Bind only to `127.0.0.1`; do not expose the dashboard through a tunnel.
- Require an OmniRoute API key even locally.
- Set `STORAGE_ENCRYPTION_KEY`; otherwise stored credentials may remain plaintext.
- Run the minimal Docker build/profile as a non-root user with a read-only filesystem where practical.
- Pin a reviewed version and Docker image digest; never deploy `latest`.
- Generate and archive the SBOM and package hash.
- Disable automatic updates.
- Start with Cerebras and Cloudflare API access rather than high-risk subscription-derived “free Claude.”
- Use an explicit allowlist of providers/models. Do not use unconstrained `auto`.
- Set hard per-key and session spend caps; do not rely solely on `hidePaidModels`.
- Start with compression disabled, then test conservative RTK on recorded tasks.
- Keep system prompts, tool schemas, patches and diagnostic output outside lossy compression.
- Use a separate Unix user/container and separate configuration directory.
- Do not mount your home directory, SSH agent, browser profile, Claude credentials or general keychain.
- Red-team the exact container image before production and repeat after every upgrade.

---

# One-page decision memo

**Decision:** Adopt OmniRoute only as an isolated API-key/free-tier experimentation gateway. Do not place it in the Claude subscription path.

**Why:** OmniRoute solves a real routing problem and has genuine contributor activity, rapid provider coverage and credible circuit-breaker mechanics. But it is less than six months old, publishes at an extreme cadence and lacks independent production benchmarks or an external security audit. Its security-sensitive surface includes credential import, browser/CLI token handling, local service execution and optional root-CA/TLS interception. The Socket v3.8.5 event appears to have been an automated warning around real but mostly intentional capabilities—not a demonstrated malware incident—but it also exposed legitimate weaknesses that required v3.8.6 changes.

The decisive issue is Anthropic policy. As of July 2026, subscription OAuth is explicitly limited to ordinary use through native Anthropic applications. Routing those credentials through OmniRoute is outside the permitted pattern, regardless of whether OmniRoute is local or whether it mimics Claude Code’s traffic. Anthropic has already moved third-party harness traffic away from subscription limits and reserves enforcement without notice. Your inability to tolerate an account suspension makes that route irrational.

**Approved architecture:**

```text
Official Claude Code ── native OAuth ──> Anthropic subscription
        │
        └─ when limited: stop/switch client
                         │
Separate agent/config ──> LiteLLM/OpenRouter/OmniRoute
                         └─ API keys only; no Anthropic OAuth
```

**Preferred ordering:**

1. Native Claude Code plus official Anthropic PAYG fallback.
2. OpenRouter or LiteLLM with cheap paid API keys.
3. OmniRoute API-only pilot for selected free/cheap providers.
4. Avoid all subscription-token import and stealth/TLS features.

**Is non-Anthropic-only OmniRoute safe for the Anthropic account?** Yes, with high confidence, if “non-Anthropic-only” means Anthropic credentials are never available to the process, container or configuration, and Claude traffic remains native. Merely excluding Claude from the current combo is not enough.

**Final verdict: ADOPT-WITH-FENCES.** For mission-critical work, consider LiteLLM/OpenRouter the primary overflow path and OmniRoute a contained experimental tier until it has an independent audit, slower/stable release channel and reproducible agentic-coding benchmarks.
