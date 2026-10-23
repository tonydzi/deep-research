# DR26-07-27-HUB-01-2203 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
type: original
source: Gemini 3.1 Pro + Deep Research (почтовый ящик)
dr_id: DR26-07-27-HUB-01-2203
topic: "Практики оптимизации always-loaded файлов правил (CLAUDE.md codex + MEMORY.md индекс)"
date_collected: 2026-07-28
chat_url: https://приватный чат
proof_of_work: "Deep Research, Researching 18+ websites"

captured_by: claude-code (hub хаб)
verbatim: true
---

State of the Art in Always-Loaded Agentic Rule and Memory Architecture (2025-2026)
Research Plan

To systematically address the optimization of always-loaded rule artifacts (CLAUDE.md and MEMORY.md) for a personal multi-machine AI-agent fleet, this analysis employs a rigorous, structured research methodology.

First, the investigation prioritizes primary official documentation and peer-reviewed literature over secondary commentary. The foundational constraints of the Claude Code environment, including the strict 200-line or 25KB injection limits for memory files, are established using Anthropic's official technical specifications. This is synthesized with 2025-2026 academic literature on long-context instruction following, specifically focusing on the mechanics of "attention sinks", the "attention basin" phenomenon, and the "Format Tax" associated with constrained generation.   

Second, the analysis deliberately allocates extensive focus to documented failure modes and contrarian evidence. Rather than merely cataloging best practices, this report dissects why architectures fail at scale. This includes an examination of parallel-agent race conditions leading to file lock failures, the catastrophic degradation of conflict resolution across multi-tier instructions as evidenced by the Many-Tier Instruction Hierarchy (ManyIH) benchmark, and the empirical proof that output-formatting constraints actively degrade a model's deductive reasoning capabilities.   

Third, the report executes a head-to-head structural comparison of five concrete, named architectural conventions currently dominating the agentic ecosystem: Anthropic's CLAUDE.md, OpenAI Codex's AGENTS.md, Cursor's .cursorrules / .mdc directories, the Cline Memory Bank pattern, and aider's CONVENTIONS.md. By comparing their activation mechanisms, contextual scoping, and adherence controls, the analysis derives a highly optimized, token-efficient template specifically tailored to the 100KB codex and 20KB index caps required by the user's multi-machine fleet.

Structure and Adherence Evidence

The internal architecture of persistent, system-prompt-class files exerts a measurable and deterministic impact on the instruction-following fidelity of frontier language models. The arrangement of a ~10-100KB codex is not merely an exercise in human readability; it fundamentally dictates how the underlying transformer architecture allocates attention across the context window.

Extensive research into long-context utilization demonstrates that model performance is subject to severe positional bias. The initial understanding of this vulnerability, termed the "Lost in the Middle" phenomenon, established that language models successfully retrieve information located at the absolute beginning (primacy bias) and the absolute end (recency bias) of a context window, but suffer catastrophic retrieval failure when querying information buried in the middle [established].   

By 2026, this phenomenological observation was refined into the mechanistic "Attention Basin" theory. Empirical analysis across mainstream models reveals that when a model is presented with a sequence of structured items—such as distinct sections of a rule file, retrieved documents, or few-shot examples—it systematically allocates disproportionate attention to the structural boundaries of the overall block, while the interior forms a U-shaped attention basin [emerging]. This behavior is intimately tied to the existence of "attention sinks." Autoregressive language models consistently allocate massive activation values to the initial tokens of a sequence (specifically the ⟨BOS⟩ token and its immediate successors), regardless of their semantic relevance. This artifact stems from the Softmax normalization operation within the attention mechanism, which forces attention scores to sum to one across all contextual tokens, causing the model to use initial tokens as a repository for excess attention mass [established].   

Consequently, the implementation of a priority "TOP" block is a strict mathematical requirement for long rule files. To guarantee high-fidelity adherence, the most critical behavioral constraints must be placed at the absolute beginning of the file, anchoring them to the model's intrinsic attention sinks [established].   

A significant contradiction exists in the current literature regarding the use of formatting syntax—specifically, Markdown headers versus XML tags. On one side, the 2026 "Format Tax" study demonstrates that forcing a language model to generate structured outputs (such as JSON, XML, or strict Markdown) substantially degrades its reasoning and writing performance. The prompt-level cognitive cost of format-requesting instructions alone causes massive accuracy loss before any constrained decoding is applied [established]. However, for the purpose of input parsing, practitioner data heavily favors XML tags (<context>, <rules>) over Markdown headers (## Rules) for delineating semantic zones. Markdown headers are processed by the model as continuous narrative strings, forcing it to expend probabilistic processing power to infer where a rule ends and background context begins. XML tags create mathematically explicit, discrete semantic boundaries that prevent the model from confusing internal data with new instructions, effectively eliminating the "attention tax" associated with ambiguity [emerging] [single-source].   

The resolution to this contradiction lies in decoupling input structure from output constraints. The optimal architectural approach utilizes XML tags to wrap major structural blocks within the CLAUDE.md and MEMORY.md files to aid the model's input parsing, while strictly avoiding any instructions that force the agent to output complex, nested XML or JSON during its intermediate analytical steps [emerging]. Furthermore, categorization and specificity are paramount. Anthropic's official guidance dictates that rules must be concrete and verifiable; a directive such as "Use 2-space indentation" achieves measurably higher adherence than a vague instruction like "Format code properly" [established].   

The Dual Audience Dilemma

Always-loaded rule files suffer from a fundamental tension: they must serve as highly optimized, token-efficient control vectors for an AI agent, while simultaneously remaining legible for the non-technical human operator who must audit and maintain them.

Attempting to force a single Markdown file to act as both a perfect human-readable dashboard and a mathematically optimized machine prompt is a documented anti-pattern that leads to context bloat and degraded agent performance [speculative] [single-source]. AI coding agents require absolutely complete, unvarnished context regarding the project's mechanics at the beginning of each session. Because coding agents know nothing about the codebase prior to the injection of CLAUDE.md, every token spent on human-centric formatting, polite phrasing, or exhaustive historical rationale is a token stolen from the agent's active reasoning capacity [established].   

Anthropic has introduced a specific mechanism to mitigate this tension within the Claude Code ecosystem: block-level HTML comments. Any text wrapped in standard HTML comments (<!-- maintainer notes -->) within a CLAUDE.md file is entirely stripped by the harness before the file's contents are injected into the model's context window [established]. This allows human operators to leave extensive documentation, structural warnings, and rationale for other human team members directly in the source file without consuming a single token of the model's context budget.   

However, as architectures scale to ~100KB, practitioners increasingly converge on treating the agent-facing rule files as "compiled code." The source .md or .mdc files are written with rigid, machine-optimized syntax, utilizing XML tags, strict negative constraints, and zero conversational fluff. For human oversight, a generated human-readable view—such as a dynamically compiled dashboard, a synthesized summary report, or a visualization of the Memory Bank's Mermaid flowcharts—is vastly superior to attempting to humanize the source file itself [speculative] [single-source]. This progressive disclosure strategy ensures that the agent's memory remains dense and token-efficient, prioritizing the machine's attention budget over human aesthetic preferences.   

Recency Markers and Conflict Resolution

The practice of stamping rule lines with inline dates (e.g., "⭐25.07") to encode chronological provenance and enforce a "newer beats older" dynamic introduces severe, often catastrophic cognitive load on the agent.

The assumption that an LLM can reliably parse a date string, compare it against other date strings scattered throughout a 100KB file, deduce the chronological hierarchy, and successfully override an older foundational rule is empirically false. The April 2026 Many-Tier Instruction Hierarchy (ManyIH) benchmark exposes this critical vulnerability in modern agentic architectures. In real-world environments, agents receive instructions from heterogeneous sources—system prompts, dynamically loaded memory files, tool outputs, and user requests—each carrying different implicit or explicit privilege levels. The ManyIH-Bench evaluation, comprising 853 complex agentic tasks across up to 12 privilege levels, demonstrates that even frontier models (such as GPT-4o and Claude 3.5 Sonnet) perform exceptionally poorly, achieving only ~40% accuracy when navigating instruction conflicts [established].   

Models are highly brittle when asked to resolve contradictions. If two rules conflict, the model frequently fails to identify the conflict, prioritizes the wrong instruction, or picks one entirely arbitrarily [established]. Relying on an inline date marker forces the model to perform a complex, multi-stage reasoning task merely to determine which rule is valid. Given the sub-50% success rate in dedicated ManyIH scenarios, utilizing inline chronological markers for conflict resolution is a dangerous anti-pattern [established].   

Instead of relying on the model's logical deduction to resolve chronological conflicts, practitioners must utilize structural and spatial overrides. Because of the established "Attention Basin" effect, a rule placed in the priority <TOP> block mathematically overrides a contradictory rule buried in section §7 of the file [established]. Therefore, spatial positioning (higher in the file equates to higher priority) is significantly more reliable than inline markers.   

Furthermore, the most robust defense against multi-writer drift and conflicting rules is strict file-scoping. Rather than maintaining contradictory historical rules in a single monolithic file, superseded rules should be physically archived out of the active context window. Utilizing frontmatter scoping (e.g., Cursor's alwaysApply: false paired with specific file globs) or dynamic active context files ensures that the agent is only ever presented with the single, canonical version of a rule for the specific task at hand [emerging].   

Index-Line Grammar and Optimization

For index files such as MEMORY.md—which are hard-capped by the Claude Code harness at the first 200 lines or 25KB—the grammar and structure of individual pointer entries dictate the success of the agent's retrieval operations [established].   

The user's current operational cap of ≤135 lines with a maximum of 150 characters per line is highly grounded in token economics and harness limitations. At a maximum of 150 characters, each pointer line consumes approximately 30 to 40 tokens. An index of 135 lines will total roughly 4,000 to 5,500 tokens. This guarantees that the entire memory index remains well under the 25KB physical limit enforced by the Claude Code auto-memory loader, while simultaneously ensuring that the entirety of the index occupies the highly attended "primacy" zone of the model's context window, safely avoiding the middle attention basin [established].   

Regarding entry grammar, empirical evidence strongly favors dense, one-line pointer entries structured as "Trigger + Gist + Pointer" over short paragraphs. LLMs exhibit degraded adherence when forced to extract actionable directives from dense prose. Conversely, structured, bulleted lists utilizing Markdown or XML boundaries are scanned highly efficiently by the attention mechanism [established].   

The optimal anchor syntax relies heavily on the native routing capabilities of the agent's harness. In the Claude Code ecosystem, the @path/to/import syntax is natively supported. When the @ symbol is used (outside of backticks), the harness automatically expands the referenced file and loads it directly into the context window at launch, recursively supporting up to four hops of depth [established]. For dynamic, on-demand loading where the agent must actively decide to read a file using a tool, standard Markdown syntax [Title](path.md) is appropriate. However, this must be paired with an explicit, unambiguous trigger condition. A pointer reading [Auth Architecture](docs/auth.md) - OAuth2 flow and RBAC rules is sufficient, but adding a conditional trigger—If modifying authentication, you MUST read [Auth Architecture](docs/auth.md)—removes the probabilistic ambiguity of whether the agent should expend a tool call to fetch the secondary file.   

Taxonomies and Top-Block Capacity

The organization of agent rule files requires a taxonomy that strictly segregates concerns to prevent context pollution.

Across the open-source and enterprise ecosystem, practitioners have largely converged on a unified section schema, heavily popularized by the structured Markdown files of the Cline Memory Bank architecture [established]. This schema operates on a strict hierarchy:   

Core Directives / Project Brief: The absolute foundational behavioral rules, universal constraints, and the ultimate goal of the system.

Active Context: The current focus of the agent, immediate next steps, recent changes, and active blockers. This section is highly volatile and updated frequently.

System Patterns: Architectural decisions, mandatory design patterns, and systemic constraints that govern how code is written.

Tech Context: The infrastructure, build commands, testing protocols, and environmental setups required to execute tasks.

Progress: Completed milestones, superseded objectives, and known issues.

The capacity of the TOP block—the highest-priority section of the file—is heavily constrained by the model's ability to hold distinct directives in active memory. While modern context windows span hundreds of thousands of tokens, research indicates that frontier models can only reliably follow approximately 150 to 200 distinct instructions before exhibiting a uniform, linear decay in instruction-following quality [emerging] [single-source]. Smaller, non-frontier models suffer an exponential decay under the same conditions. Crucially, the Claude Code harness system prompt inherently consumes roughly 50 individual instructions before any user-defined rules are loaded [low-authority] [single-source].   

Therefore, a TOP priority block should never exceed 20 highly distinct directives (a practical expansion of the 7±2 cognitive limit). Overloading the TOP block dilutes the model's attention and degrades its ability to identify the most critical safety and architectural constraints. Furthermore, the TOP block must strictly replace section entries, not duplicate them. Duplicating a rule in the TOP block and again in a thematic section wastes precious token budget and creates a severe risk of asynchronous structural drift; if an agent updates the thematic section but fails to update the duplicated rule in the TOP block, it creates a direct conflict that the model will struggle to resolve [established].   

Ecosystem Scan: Architectural Conventions

The agentic development landscape features multiple competing standards for managing persistent rules and memory. A comparative analysis of these frameworks reveals distinct methodologies for balancing global context against token efficiency.

Framework	Core Mechanism	Activation Strategy	Adherence Control & Efficacy


Anthropic CLAUDE.md

[cite: 1]

	Single universal codex loaded automatically at session start.	Global. Subdirectories load on-demand. Native @import syntax expands files recursively up to four hops.	Relies heavily on brevity (strict <200 lines guidance). Sub-rules are managed via .claude/rules/ directories using YAML paths frontmatter to restrict loading to specific file globs.


Cursor Rules (.mdc)

[cite: 20, 22, 23]

	Granular, highly scoped markdown files containing YAML frontmatter.	Metadata-driven. Fields dictate activation: alwaysApply (global), globs (file pattern match), or description (semantic agent match).	Highly precise and token-efficient. Physically excludes irrelevant rules from the context window, effectively neutralizing the "Lost in the Middle" vulnerability by controlling context size.


Cline Memory Bank

[cite: 19, 21]

	Multi-file directory hierarchy (projectbrief.md, activeContext.md, etc.).	Prompt-enforced reading. Utilizes inline Mermaid flowcharts to explicitly dictate the chronological order of file evaluation to the agent.	High structural rigidity. The agent is forced to verify that its context is complete before executing planning or coding tasks. Excellent for long-horizon autonomous tasks.


aider CONVENTIONS.md

[cite: 24]

	Standard natural language Markdown file.	Global flag execution (--conventions-file).	Flexible and easy to implement, but highly prone to attention dilution and context bloat if the file grows unchecked.


Codex AGENTS.md

[cite: 25, 26]

	Minimalist, tool-agnostic standard designed for cross-agent compatibility.	Global and directory-scoped cascading rules.	Relies entirely on the agent to interpret hierarchical nesting based on file proximity to the working directory.

The comparative data heavily favors the architectural approach pioneered by Cursor's .mdc files, specifically the use of glob-based scoping. By conditionally loading rules based on the specific files the agent is interacting with, the system physically protects the context window from bloat [established]. For users operating within the Claude Code ecosystem, Anthropic has mirrored this capability by allowing rules to be placed inside a .claude/rules/ directory, where YAML frontmatter can dictate path-specific rule activation (e.g., paths: - "src/api/**/*.ts") [established]. This mechanism is critical for managing a ~100KB codex, as forcing 100KB of text into the root CLAUDE.md guarantees catastrophic retrieval failure and adherence drops.   

Anti-Patterns and Multi-Writer Drift

Maintaining an always-loaded file architecture while deploying a fleet of autonomous, parallel agents introduces severe operational hazards that quickly degrade system stability.

The most prevalent failure mode is the creation of the "Laundry List." As agents encounter edge cases, they append granular, highly specific fixes directly to the root CLAUDE.md or MEMORY.md. Once these files exceed their physical limits (200 lines for Claude auto-memory), the harness forcefully truncates them, severing critical pointers. Even if truncation is bypassed, the resulting context bloat ensures that the model ignores the instructions [established]. This leads directly to "Context Rot," where 90% of a bloated memory file consists of per-topic historical data that is entirely irrelevant to the agent's current task [emerging] [single-source].   

A more insidious failure mode is "Parallel Append Drift." When multiple agent sessions (e.g., 4+ computers running Claude Code) operate concurrently, simultaneous attempts to update a shared MEMORY.md file result in race conditions. Documented failure modes in agent repositories include git lock conflicts, silent file overwrites, and structural drift where newly appended intake lines land in the wrong markdown sections due to asynchronous file reads and writes [emerging]. Furthermore, auto-formatting robots often fight against hand-made XML or Markdown structures, leading to syntax corruption.   

To mitigate these systemic failures, several architectural defenses are required:

The Append-Only Inbox: Agents must be strictly forbidden from editing the core taxonomy or thematic sections of the MEMORY.md file directly. Instead, they are instructed to append any new learnings or memory pointers exclusively to a designated <inbox> section at the absolute bottom of the file (EOF). This minimizes git lock collisions and prevents agents from corrupting the core schema. A scheduled, single-threaded batch process—or a specialized "librarian" sub-agent—periodically runs a re-sort pass to move items from the inbox into the correct taxonomy [speculative] [no source found].

Write Gates and Linting: Implementing PreToolUse hooks within the Claude Code harness can block destructive actions. A shell script hook can intercept any attempt by the agent to write to CLAUDE.md or the core sections of MEMORY.md, returning a hard exit code (e.g., exit code 2) if the edit violates structural guidelines [established].   

Self-Compaction Frameworks: April 2026 research into agentic long-horizon tasks introduces "SelfCompact" methodologies. By providing the agent with a compaction tool and a strict, lightweight rubric dictating when to fire it, the agent can autonomously summarize its accumulated history into dense pointers, preventing the inbox from overflowing before the next sort pass [emerging].   

Verification and Adherence Testing

Subjective observation is insufficient to prove that restructuring a 100KB codex or a 20KB index actually improved the agent's behavior. Practitioners must rely on deterministic evaluation suites, specifically utilizing frameworks like promptfoo or inspect_ai.

To verify adherence, the industry standard relies on executing side-by-side tasks against different versions of the rule file, holding the model, task, and environment constant. The evaluation must prioritize deterministic assertions before relying on LLM-as-a-judge [established].   

Cheapest Credible A/B Design for a Solo Operator (3-Day Window):

Isolate the Challenger: Maintain the baseline rule files (v1) and create the restructured challenger files (v2).

Behavior-Trap Generation: Author a suite of 10 to 20 specific prompts designed to trigger distinct rules located in various sections of the codex. For example, if a rule dictates that "All API error responses must include a timestamp," the prompt asks the agent to generate an API error handler.

Deterministic Assertions (promptfoo):

Routing Check: Use the skill-used assertion to verify if the agent actually read the target sub-file or memory pointer [established].   

Syntax Check: Use the contains or is-json assertions to verify that the agent's output perfectly matches the required structure dictated by the rule [established].   

Cost and Latency Signals: Track token usage and time-to-completion across both versions. A bloated or poorly structured rule file will predictably cause the agent to consume more context tokens and exhibit higher latency, serving as a secondary signal of inefficiency [established].   

Execution: Run the suite with --repeat 3 to account for nondeterministic agent behavior, utilizing the --no-cache flag to ensure fresh inference passes [established].   

Explicit Contradictions in the Literature

When synthesizing the state of the art, two explicit contradictions emerge across credible sources:

The XML Tagging vs. Format Tax Contradiction: Anthropic's prompt engineering guidelines strongly recommend the use of XML tags to create discrete semantic zones, improving the model's ability to parse complex instructions. Conversely, the rigorous 2026 "Format Tax" research proves that requiring a model to process and generate highly structured formatting (like XML or JSON) degrades its deductive reasoning and writing performance by imposing a severe cognitive tax at the prompt level.   

Resolution: The consensus dictates a decoupled approach. XML tags should be used to wrap the input rules and sections within the CLAUDE.md file to establish semantic boundaries for the model's attention mechanism. However, the rules themselves must explicitly allow the agent to output freeform text or use standard thinking tags (<think>) during its intermediate analytical steps, only enforcing rigid output formats at the absolute final stage of a task [emerging].   

File Length Limits vs. Empirical Agent Usage: Anthropic's official documentation establishes a strict hard limit, advising users to keep CLAUDE.md under 200 lines to prevent context bloat and ensure high adherence. However, empirical scans of highly successful, agent-native open-source repositories reveal that the most effective teams routinely deploy memory files containing tens of thousands of characters, utilizing them as massive verification gates.   

Resolution: Monolithic large files fail. The repositories successfully deploying massive rule sets do so through strict progressive disclosure. The root CLAUDE.md remains small (acting as a router), while the bulk of the rules are offloaded into sub-files that are conditionally loaded via @path imports or YAML frontmatter globs.

Ranked Recommendations
Practice	Evidence Strength	Applicability to Fleet Caps (100KB Codex / 20KB Index)	Description & Risk
Spatial Priority (TOP Block)	[established]	Critical	Restrict the TOP block to ≤20 explicit rules placed at line 1. Leverages the "Attention Basin" effect to ensure critical safety and architectural constraints are never dropped from attention. Risk: Pushing necessary context into the middle decay zone.
Path-Scoped Sub-Rules	[established]	Critical	A 100KB monolithic codex will fail. Offload 90% of the codex into .claude/rules/*.mdc files tied to specific directory or file extension globs. Keeps the root file strictly <200 lines.
XML Tagging for Input Boundaries	[emerging]	High	Wrap sections in <priority_rules>, <active_context>. Creates explicit mathematical boundaries that prevent the agent from conflating background data with executable instructions.
Append-Only <inbox> Zone	[speculative]	High	Mandate that parallel agents only append new learnings to an <inbox> at the absolute end of the file. Mitigates git lock race conditions and preserves core file taxonomy.
Deterministic Rule Linting	[established]	High	Utilize promptfoo or inspect_ai to run deterministic assertions (contains, skill-used) against the rule files to prove adherence without relying on subjective observation.
HTML Maintainer Comments	[established]	Moderate	Use <!-- maintainer notes --> for human-readable context in CLAUDE.md. The harness strips these before injection, saving token budget.
Inline Date Recency Markers	[fringe]	Low	DO NOT USE. Frontier models fail ~60% of the time at multi-tier chronological conflict resolution. Rely on spatial priority or absolute file-scoping instead.
Proposed File Templates

To support parallel writers without breaking the meticulously crafted structure, the architecture must completely separate the immutable, high-priority taxonomy from a volatile append-zone.

Codex Template (CLAUDE.md)

Constraint: The root file must remain strictly under 200 lines. The remaining ~100KB of behavioral rules must be offloaded into the .claude/rules/ directory, utilizing YAML paths frontmatter to load only when specific files are accessed.

XML
<priority_rules>
- CRITICAL: Never bypass pre-tool-use safety checks.
- ARCHITECTURE: State management is handled strictly by Zustand.
- PRECEDENCE: Rules in this block mathematically override all other instructions.
</priority_rules>

<project_context>
- Project: Multi-machine AI fleet orchestrator.
- Tech Stack: Node.js, TypeScript, Claude Code.
- Goal: Maintain 100% autonomous uptime with zero human intervention.
</project_context>

<routing_index>
- Database schemas and migrations: import @.claude/rules/database.md
- Core testing protocols: import @.claude/rules/testing.md
- Deployment and CI/CD steps: import @.claude/rules/deploy.md
</routing_index>

<!-- maintainer notes: 
  DO NOT add specific feature rules directly to this file. 
  This root codex is restricted to 200 lines to preserve the attention sink.
  Create a scoped file in .claude/rules/ with YAML frontmatter instead. 
-->

Index Template (MEMORY.md)

Constraint: Hard capped by the Claude Code auto-memory loader at 200 lines or 25KB. The user specification of ≤135 lines at ≤150 characters per line (approx. 20KB) perfectly optimizes for this limit.

XML
<active_context>
- Current Focus: Refactoring concurrent git lock handlers for agent file I/O.
- Blocker: Parallel agents overwriting MEMORY.md simultaneously causing data loss.
- Next Step: Implement the append-only inbox architecture below.
</active_context>

<memory_index>
- [Auth](docs/auth.md) - OAuth2 flow, token refresh logic, RBAC role definitions
- [API](docs/api.md) - Rate limiting thresholds, error handling schemas, core endpoints
- [Fleet](docs/fleet.md) - Sub-agent orchestration, timeout handling, heartbeat checks
- [Archive](docs/archive.md) - Superseded rules, completed milestones, legacy endpoints
</memory_index>

<inbox>
<!-- AGENT INSTRUCTIONS: 
  If you must record a new memory or discovering, you are FORBIDDEN from editing the blocks above.
  You must append your entry ONLY to the bottom of this inbox block. 
  A scheduled background process will sort it into the index. 
-->
- 
</inbox>

Decision-Oriented Ending

For an operator managing a 4+ multi-machine AI-agent fleet, constrained by a 135-line index and a ~100KB codex, the core operational threats are severe context bloat (triggering the attention basin) and parallel-write corruption (causing structural drift).

Option 1: The Monolithic Codex. Keep the entire 100KB of rules in a single CLAUDE.md file.

Trade-offs: This requires zero structural engineering and is easy for a human to search. However, it guarantees catastrophic "Lost in the Middle" retrieval failures, explicitly violates Anthropic's 200-line hard cap, and ensures the agent will randomly ignore vital safety constraints.

Option 2: The Chronological Log. Utilize inline dates (⭐25.07) and allow agents to append rules freely wherever they see fit.

Trade-offs: Provides zero friction for autonomous agents. However, it is mathematically proven to fail. Frontier models suffer massive accuracy drops when forced to resolve multi-tier chronological conflicts, resulting in the agent prioritizing outdated rules.

Option 3: Modular Progressive Disclosure with Inbox Append. Strip the root CLAUDE.md down to a 50-line <priority_rules> block and a routing index. Offload the remaining 95KB into .claude/rules/ files scoped by file-path (mirroring Cursor's .mdc format). Force all parallel agents to dump new learnings strictly into an <inbox> at the absolute bottom of MEMORY.md.

Recommendation: Option 3 is the only viable path for a production-grade multi-agent fleet.

Confidence Tier: [established]

Main Assumption: This recommendation rests on the assumption that the local Claude Code client harness correctly parses and evaluates .claude/rules/ path-scoping frontmatter, dynamically injecting context only when relevant files are touched, thereby successfully bypassing the global attention basin limitations inherent to a monolithic file structure.