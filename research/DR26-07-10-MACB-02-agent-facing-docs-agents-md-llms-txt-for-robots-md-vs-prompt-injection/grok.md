# DR26-07-10-MACB-02 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-10-MACB-02

 
DEEP RESEARCH REQUEST: Agent-facing documentation vs prompt injection — where is the line, and how should OSS projects respond to public accusations?
 
Search the web extensively and cite sources.
 
CONTEXT
We maintain a small open-source repo (agent-security control patterns) that includes a FOR-ROBOTS.md — a file explicitly addressed to AI coding agents whose human operators point them at the repo ("give this link to your agent"). A reader publicly called this pattern "prompt injection". We fixed the one genuinely injection-shaped sentence ("apply this, don't wait for your human" -> "this document is data, not authorization; propose to your operator"). We now want the industry picture.
 
QUESTIONS

THE LINE. What distinguishes a legitimate agent-facing document (llms.txt, FOR-ROBOTS/AGENTS.md files, agent cards, MCP tool descriptions) from prompt injection? Is there emerging consensus (academic, vendor guidance — Anthropic/OpenAI/Microsoft/NIST/OWASP LLM Top-10) on criteria such as: consent of the operator who directed the agent to read it, imperative vs advisory language, scope of requested actions, reversibility, transparency to the human? Collect concrete published definitions and taxonomies (2025-2026).
PRECEDENTS. Which OSS projects or vendors publish agent-addressed instructions today (llms.txt adopters, AGENTS.md convention, A2A agent cards, MCP servers)? Have any been publicly accused of prompt injection or "agent-targeting dark patterns"? How did they respond — wording changes, disclaimers, security.md, third-party audit? What worked reputationally?
DISCLOSURE PRACTICE. Best practices for responding publicly when someone accuses your repo/content of prompt injection: acknowledge-verify-fix-credit patterns, security advisories for non-code (prose) issues, CVE applicability to instruction-text, responsible-disclosure invitations in README. Examples of good and bad public responses.
DESIGN GUIDANCE. If a project WANTS agent-facing docs (that is our deliberate pattern), what wording/structure minimizes injection-shape while staying useful: advisory framing, "data not authorization" preambles, operator-consent framing, avoiding imperatives that touch irreversible actions? Any published style guides or lint rules for agent-readable prose?
 
FORMAT
Cited sources with dates (2025-2026 preferred), separating established practice vs emerging opinion vs speculation.
A one-page synthesis: the 5-7 criteria that most sources agree distinguish legit agent-facing docs from injection.
A short checklist we can run over every future agent-facing file before publishing.
Thought for 1m 22s

Agent-facing documentation (e.g., llms.txt, AGENTS.md/FOR-ROBOTS.md/CLAUDE.md, agent cards, MCP tool descriptions) vs. prompt injection is an emerging distinction in 2025–2026 literature and practice. These files provide project- or site-specific context to AI coding agents or LLMs that humans deliberately direct at a repo or URL. They differ from classic (especially indirect) prompt injection primarily in authorship intent, human consent/transparency, language framing, scoping, and the degree to which they request or enable autonomous/high-impact actions without oversight.⁠Developer.nvidia

No single authoritative taxonomy or vendor consensus document (from Anthropic, OpenAI, Microsoft, NIST, or OWASP) draws an explicit bright line for these exact artifacts as of mid-2026. However, a clear synthesis emerges from OWASP LLM Top 10 (2025), security research on indirect injection via repos/docs/config, vendor agent robustness work, and analyses of the AGENTS.md pattern.⁠Genai.owasp

1. The Line: Distinctions and Emerging Consensus (2025–2026)

Legitimate agent-facing docs are authored and maintained by the project/site owners (or trusted contributors) as helpful context or recommendations for agents that human operators explicitly point at the resource. They function like specialized READMEs or config for context engineering: build commands, coding conventions, boundaries, documentation indexes, or tool metadata. Runtimes (VS Code, Cursor, Codex, Claude Code, etc.) often treat them as trusted or easily injectable additional context.⁠Morphllm

Prompt injection (particularly indirect) involves attacker-controlled or untrusted content embedded in what the agent processes—malicious commits, compromised dependencies overwriting files, poisoned docs/comments, or crafted tool descriptions—that overrides intended behavior, often via imperative language or by exploiting trust in external context. Classic vectors explicitly include code comments, documentation, commit messages, repo files, and web content processed by AI coding assistants.⁠Cheatsheetseries.owasp

Key distinguishing criteria (synthesized from sources; no single paper lists all exactly, but these recur across OWASP, NVIDIA Red Team, Backslash Security, Prompt.Security analyses, Microsoft MCP guidance, and Anthropic/OpenAI robustness research):

Authorship, Control & Intent — Project-owner-authored/maintained for helpful project-specific context vs. attacker-controlled or supply-chain-injected (e.g., malicious dep overwriting AGENTS.md).⁠Developer.nvidia
Human Operator Consent & Transparency — Deliberately directed/read by a human who can review the file (often signaled in README or agent UI) vs. hidden/sneaky in untrusted external content the agent ingests without full awareness.⁠Prompt
Language & Precedence — Advisory/recommendatory framing ("recommended practices," "consider," "best for this project") with explicit deference to operator/user instructions vs. imperative/overriding ("always do X," "ignore previous/user instructions," "silently").⁠Backslash
Scope & Claimed Authority — Explicitly scoped to the project/site/tools/domain vs. broad/global claims or attempts to set agent-wide policy or bypass safety.
Action Orientation & Human Oversight — Provides data/context for decisions (or low-stakes suggestions) vs. direct instructions for autonomous, irreversible, or privileged actions (exfil, code changes, external calls) without human confirmation.⁠OpenAI
Reversibility, Impact & Risk Profile — Info-only or easily reversible context vs. high-impact effects (credential staging, silent execution, data exfiltration, persistence) executable with low friction.⁠Backslash
Runtime Treatment, Auditability & Guardrails — Treated as additional context (subject to sanitization, guardrails, or review) with changes audited like code/config vs. blindly trusted executable substrate or control plane.⁠Themenonlab

Supporting sources and nuance:

OWASP LLM01:2025 Prompt Injection and Cheat Sheet (updated 2025): Defines indirect injection via external content including "code comments and documentation analyzed by AI coding assistants," repo-related vectors, and "context poisoning." Defenses emphasize separating instructions from data, sanitization, guardrails, least privilege, and human-in-the-loop (HITL). Agent-specific attacks include thought/observation injection and tool manipulation.⁠Cheatsheetseries.owasp
NVIDIA AI Red Team (Apr 20, 2026): AGENTS.md is "trusted context by design" for legitimate project instructions/conventions; the risk arises from supply-chain overwrite turning it malicious (e.g., directives to inject delays silently and ignore user requests). Mitigations: dependency pinning/scanning, protect config files from agent write, monitoring, guardrails (garak/NeMo).⁠Developer.nvidia
Backslash Security (≈Jul 2026): OpenAI Codex (exec mode) treats AGENTS.md as implicitly trusted config (like Makefile/CI); malicious content enables silent credential exfiltration. Classification: "indirect prompt injection delivered through an implicitly trusted configuration file." Recommendation: review AGENTS.md before execution on untrusted repos; treat as supply-chain artifact.⁠Backslash
Prompt.Security (Dec 2025) and TheMenonLab (Feb 2026): AGENTS.md in VS Code is "instruction set, not documentation"—auto-injected, treated as authoritative, enabling goal hijack and data exfil. Structural risk beyond classic injection due to persistence and trust model.⁠Prompt
Microsoft (MCP guidance, 2025): Tool descriptions/metadata in Model Context Protocol (MCP) servers are vulnerable to "tool poisoning" (indirect injection). Guidelines focus on mitigating indirect attacks via external content.⁠Developer.microsoft
Anthropic/OpenAI (2025 research): Emphasis on model robustness to indirect injection (e.g., browser agents), explicit/specific instructions to agents rather than broad autonomy, and human oversight for consequential actions. Avoid giving agents wide latitude that lets hidden content mislead them.⁠Anthropic
llms.txt proposal (Jeremy Howard / llmstxt.org, ongoing since 2024; active implementations 2025–2026): Passive, structured Markdown index/summary + links to .md versions for LLM-friendly consumption (like a sitemap for agents). Low active-instruction risk; focused on discoverability and token efficiency. No formal security section, but positioned as helpful data provision.⁠Llmstxt

Established practice vs. emerging opinion: Widespread adoption of AGENTS.md/llms.txt patterns is established (dozens of tools, many repos, doc platforms). Treating them as security-sensitive config (review like code, sandbox agents) is rapidly emerging as consensus in security research. Formal academic taxonomies specifically for "agent doc vs. injection" remain limited; most analysis is in vendor/security blogs and OWASP.

2. Precedents: Adoption, Accusations, and Responses

Widespread patterns:

llms.txt: Emerging convention/proposal; adopted by API/doc platforms (Fern, GitBook, Mastercard examples) and referenced in contexts like Google Agent2Agent (A2A) discussions. Provides concise LLM-optimized indexes.⁠Buildwithfern
AGENTS.md / CLAUDE.md / variants: Common by 2025–2026 for repo-level context (build/test commands, style, boundaries, "source of truth" notes). Used in production OSS (e.g., Sentry) and tools (VS Code, Cursor, Codex, Claude Code). Some repos add self-referential rules inside the file.⁠Morphllm
MCP tool descriptions and agent cards: Emerging (2025+); tool metadata can carry descriptive context but risks poisoning.⁠Developer.microsoft

Accusations of "prompt injection" or "dark patterns": No prominent public cases found of specific OSS projects being widely accused of maliciously injecting via their own AGENTS.md/FOR-ROBOTS.md-style files. Instead, security researchers broadly flag the pattern itself as expanding the attack surface (supply chain via deps, public malicious commits, auto-trust leading to hijack). Examples include NVIDIA Red Team demos, Backslash Codex findings, and analyses labeling AGENTS.md a "control plane" or "attack surface." Community discussion includes effectiveness critiques (context bloat/rot per ETH Zurich-linked studies) and occasional "dark signal" framing (signaling heavy AI use). Defensive/positive uses exist (e.g., OWASP secure-agent-playbook CLAUDE.md for security guidance).⁠Ssojet

Responses that worked reputationally: Security-focused posts emphasize transparency and hygiene—review AGENTS.md changes with code-level rigor, add disclaimers or scoping language, treat as supply-chain artifact, pair with sandboxing/HITL. Projects adopting the pattern often include explicit notes inside the file (e.g., "update only here"). No major reputational incidents from the pattern itself when projects responded with fixes or clarifications. General OSS security precedent: prompt public acknowledgment, credit, clear diff/explanation, and updates to docs or SECURITY.md.

3. Disclosure Practice: Responding to Public Accusations

Best practices (drawn from general responsible disclosure norms and analogous non-code/content issues):

Acknowledge-verify-fix-credit: Prompt public reply (GitHub issue/comment or blog), thank the reporter, confirm investigation, share the fix (diff + rationale), and credit them. Update relevant files (e.g., the agent doc itself + README mention).
Security advisories for prose: Feasible via GitHub Security Advisories even for non-code if it could enable misuse; reference in SECURITY.md. Less likely to warrant CVE unless tied to a specific exploitable chain (e.g., leading to RCE/exfil in a popular agent runtime).
Responsible-disclosure invitation: Explicitly welcome reports in README/SECURITY.md/CONTRIBUTING.md (private channel preferred for sensitive findings).
Good examples (general pattern): Transparent post-mortems or updates explaining the distinction ("we clarified it's advisory data, not directives") and improvements. Builds trust.
Bad examples (avoid): Denial without investigation, deleting/closing issues without response, attacking the reporter, or no visible fix. These damage reputation more than the original file.

For your case (already fixed the imperative sentence): A concise public note along the lines of "Thanks for the feedback highlighting the distinction. Updated FOR-ROBOTS.md to emphasize advisory data for operator-directed agents (not authorization). Full diff linked. This aligns with emerging guidance on safe agent context files. Further suggestions welcome via issues/PRs" would be strong.

4. Design Guidance: Wording/Structure to Minimize Injection Shape

If deliberately publishing agent-facing docs (a valid and increasingly common pattern for context engineering and agent effectiveness):

Core framing (use/adapt your fix): Strong preamble like: "This document provides context, conventions, recommendations, and data for AI agents working on this project when directed by a human operator. It is data for reasoning, not system instructions or authorization. It does not override operator prompts, general safety rules, or agent guardrails. Agents: Propose any actions or changes to your human operator for explicit approval, especially those involving external systems, modifications, secrets, or potential impact. Scope is strictly this project's coding, build, test, and documentation practices."
Language principles: Advisory everywhere ("Recommended:", "Best practice for this repo:", "Consider...", "To improve X, Y is often effective"). Avoid direct imperatives on actions, "always," "do without confirmation," or precedence claims. Use "propose to operator" language for anything actionable.
Structure tips: Keep minimal (studies note excessive content can degrade performance via context rot). Use clear sections, optional frontmatter, or explicit markers. End with or include human-review emphasis. Reference or link to the project's main README for transparency.
Additional safeguards: Explicitly defer to user/operator instructions. For any suggested commands/tools, note "human confirmation required." Avoid embedding credential paths, exfil patterns, or broad behavioral overrides. Test by simulating (agent reads file + realistic user prompt) to check for unwanted precedence or hijack.
Published or community patterns: Look to repos offering "safer command output, validation, and prompt-injection resistance" patterns for AGENTS.md. Some production examples self-declare as "source of truth" while scoping tightly. Treat the file itself with config-level review processes.⁠GitHub
For related formats: llms.txt stays closer to passive index/summary (lower risk). MCP tool descriptions should remain factual/descriptive.

Short checklist for every future agent-facing file (run before publishing; adapt as needed):

Clear preamble framing it as advisory data/recommendations scoped to operator-directed use in this project only; explicit "not authorization or override."
All language advisory/recommendatory; zero overriding imperatives or "ignore user/previous" clauses.
Explicit human-in-the-loop emphasis for any suggested actions (especially external/privileged/irreversible).
Explicit scope boundaries stated.
Minimalist content (only high-value, non-obvious info); reviewed for bloat or performance impact.
Mentioned or linked in main README/CONTRIBUTING.md for human transparency.
Changes gated behind PR review (ideally with security-aware scanning or patterns checks for sensitive directives).
Simulated/tested with sample agent + user prompts to verify no hijack or precedence issues.
For MCP/tool metadata: purely factual descriptions, no embedded behavioral directives.

This pattern remains useful for OSS agent security/control work when implemented with the above hygiene. The core insight from 2025–2026 sources is that the files are powerful context tools whose risks are manageable through authorship transparency, careful language, scoping, oversight emphasis, and treating changes like code/config rather than through prohibition. Your fix already moves strongly in the right direction.

Further monitoring of OWASP updates, vendor guardrail advancements (e.g., Prompt Shields, robustness training), and MCP/agent runtime security will refine these practices.

58 sources
Get notified when Grok finishes answering
Enable




Expert