# DR26-07-16-FLEE-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt.com (Deep Research, экспорт Markdown — выгружен Антоном вручную)
topic: "DR26-07-16-FLEE-01 — Conditional rule activation: decision-tree dispatcher for always-loaded agent instruction files"
dr_id: DR26-07-16-FLEE-01
date: 2026-07-17


chat_url: https://приватный чат
prompt_author: Маяк (Маяк)
note: "verbatim; исходник «внутренний путь лаборатории» (2).md"
---

# Conditional Rule Activation in LLM Agents

**Executive Summary:** Modern AI coding agents suffer from “context bloat” when every rule and instruction is loaded at session start. The consensus of recent analyses and practices is to **split instructions into a small, always-loaded index (triggers) and larger bodies fetched on demand**. Anthropic and others recognize this need: a 2026 Claude Code feature request explicitly calls for **lazy-loading context components** (skills, rules, instructions) instead of all at startup. In practice, teams have succeeded by moving “always-needed” invariants into compact files (often with path-based triggers) and pushing the rest to external documents or dynamic hooks. For example, path-scoped rules (globs in frontmatter) load only when editing matching files, slashing irrelevant context. Further semantic routing can be layered on this: one group built a *PreToolUse* hook that scans each user message for keywords (e.g. “debug”, “delegate”) and injects only the relevant rule documents. 

Key findings include: 1) **Always-loaded index + lazy rule bodies is the scalable architecture**. As one expert notes, “Always-loaded index, lazy bodies is the right structure: the index tells the agent what it knows; the bodies give it the actual rule when needed”. 2) **Current Claude Code has no built-in lazy loading** – it forces full `CLAUDE.md` at startup, aside from path-scoped rules on file reads. Industry tools differ: *Cursor* and *Windsurf* support fine-grained rule files with frontmatter (always/auto/glob/manual) and let the LLM decide which “Intelligent” rules to include, whereas GitHub Copilot’s custom instructions are loaded once per session and not reevaluated per turn. 3) **Context-size vs compliance**: Empirical evidence (benchmarks and user reports) shows a steep knee around a few hundred tokens of rules. Papers and users report that 30–150 lines of instructions typically maximize performance; beyond ~150 lines, additional rules often *hurt* accuracy. Claude Code itself “compresses” old instructions over time, meaning large static instructions get pruned under load. 4) **Rule taxonomy matters**: Best practice is to cluster rules by triggering context (file paths, actions, topics). For example, one system divided rules into (a) *always-on* small CLAUDE.md with core conventions, (b) *path-scoped* glob rules for file-specific guidelines, (c) *knowledge-based* guideline docs loaded on demand, (d) *skills* for workflows, and (e) isolated *subagents* for complex tasks. Teams test coverage with “cold-reader” experiments and trap scenarios: giving a fresh agent just rule summaries or misdirection prompts and verifying it still behaves correctly.  

**Key Findings:**

- **Index+Lazy-Body Architecture [established]:** A split architecture—keeping only a small index (rule triggers, names, short descriptions) always in context and fetching full rule bodies on demand—is widely endorsed. Nicolas Bustamante’s analysis argues “Always-loaded index, lazy bodies is the right structure” for scalability. By contrast, monolithic instructions slow the agent and lead to forgotten rules over long sessions. 

- **Path-Scoped and Keyword Routing [established]:** Native features like Anthropic’s path-scoped rules can effectively reduce context. A rule with `paths: ["src/api/**/*.ts"]` only enters context on matching file reads. Custom solutions go further: one workflow built a *context router* using a PreToolUse hook that matches keywords in each turn and injects only the matching rule docs. This achieved “Smart Loading”: core rules in context always, domain-specific rules only when relevant. Cursor’s “Apply Intelligently” mode similarly uses rule descriptions (no globs) and lets the model self-select relevant rules per prompt.

- **Size vs Compliance [established]:** Multiple sources show that more instructions do *not* linearly improve adherence. For example, an ETH-Zurich study found *LLM-generated* context files often hurt performance (–0.5–2% success) unless they were very concise; “Keep it short (under 150 lines)”. Sohit Kumar reports a drop-off after ~100 lines: his agent went from 70% to 99% first-try success after shrinking CLAUDE.md from 470 to 94 lines. Primeline’s analysis notes “quality degrades as context fills; more relevant beats more total context”. Recent research even suggests injecting instructions *after* input can improve following for long contexts, implying the very front-heavy loading of rules may lead to forgetting.

- **Rule Taxonomy [emerging]:** Organizing rules into a hierarchy or faceted taxonomy is crucial. Effective designs use a decision framework (e.g. “if violating breaks the build, make it a rule; if triggered by an action, make a skill; if reference info, make a guideline; otherwise skip”). Clustering by project facet (e.g. front-end vs back-end rules) or tags (security, performance, style) helps control injection. Teams validate coverage by “retrieval-regression” tests: scripted prompts that should invoke specific rules, and “cold-reader” scenarios: giving an agent only summary pointers to see if it still applies rules correctly.

- **Harness Capabilities 2026 [established]:** Claude Code today has no built-in two-level loader for rules. A public Anthropic issue (Apr 2026) explicitly requests *lazy-loading* for all context components (skills, MCP tools, rules). It even outlines a future “context-aware instructions” feature (e.g. load `.claude/instructions/on-test.md` only when running tests). As of now, Claude Code only supports path-scoped rules and full CLAUDE.md (which is reloaded verbatim from disk on each turn). No new native hooks (besides the generic AdditionalContext) directly address conditional rule sets. Competing tools: Cursor and Devin support directory-scoped instruction files (AGENTS.md) and rule globs; Copilot supports only static instructions loaded once. 

**Contradictions & Debates:**

- **Information Density vs Recall:** There’s broad agreement that *concise* instructions outperform bloated ones, but not all studies converge on the thresholds. Anecdotal reports place the “sweet spot” at tens of lines, yet some academic work on model training suggests models can follow instructions better when positioned flexibly. The debate hinges on whether larger context windows (e.g. 1M tokens) will meaningfully allow hundreds of instructions without loss. Empirical results (performance vs context usage) are sparse beyond tool vendor blogs, so this remains partly speculative.

- **Static vs Dynamic Loading:** Industry practice is split. Claude Code’s design opts for simplicity (load everything), whereas tools like Cursor emphasize dynamic loading. Some argue static is simpler and avoids “fetch at wrong time” failure modes, while others (like Primeline) show clear cost/quality benefits from on-demand loading. There is no public benchmark for “lazy vs always-on rules” fidelity; the field relies on expert testimony. Early adopters of dynamic routers report success, but admit it adds system complexity. 

- **Semantic Routing Accuracy:** Injecting rules by content matching (e.g. keyword or embedding classifiers) is powerful but not foolproof. The Primeline router example achieves high savings, but it is a homegrown system. No formal evaluation exists of “missed-rule” rate versus saved tokens. The risk is that if the classifier/rule-matcher fails, the model might miss a needed rule. Tool chaining and hook injection help, but the consensus is that nothing beats end-to-end testing. Teams often tag such mechanisms as “emerging practices” until more studies appear.

- **Position and Format Effects:** Older studies showed that order and format of instructions matter greatly. Newer models may be more robust, but we found no definitive 2026 evidence that position no longer matters. The state-of-the-art advice (keep global directives at the very front) still seems prudent, though some suggest (ACL 2024) that placing instructions *after* input text can help long cases. This contradicts Claude’s default of system->instructions->dialogue. More controlled experiments are needed.

**Best Practices:**

- **Tiered Context Architecture:** Follow a context-tiering model. Keep `CLAUDE.md` tiny (ideally <100–150 lines), containing only mission-critical invariants and pointers. Move “must never” rules into `.claude/rules/` files with path globs so they always load as needed. Place infrequent or bulky instructions outside initial context: e.g. as Git-tracked markdown docs or as skills invoked on demand. 

- **Path-Scoped Rules:** Convert as many rules as possible into path-scoped (glob) rules. For any rule not universally needed, add a `paths:` frontmatter to load it only during relevant file edits. This is Claude Code’s native mechanism and provides “zero cost when not applicable”. (Cursor/Windsurf offer analogous glob scoping.) 

- **Semantic Rule Dispatch:** Implement additional routing logic for general-purpose rules. Options include: 
  - Using a PreToolUse hook (Anthropic) or middleware to scan each user turn for topics and fetch relevant rules or docs. 
  - Leveraging “intelligent” rule modes (Cursor’s description-based rules) which let the model include a rule if it deems the short description relevant. 
  - Building or using an external router (as Primeline’s *Evolving Lite* plugin does) keyed on keywords or embeddings. 
  These approaches keep only the relevant subset of rules in context per turn. 

- **Pointer-Form Rules:** Structure rule descriptions in CLAUDE.md (or equivalent) as *pointers* rather than full text. Store the full procedure or rationale in separate knowledge-base files and have the agent fetch them when needed. The user’s own “cold-reader” tests confirm that giving Claude only the triggers plus core directive can preserve behavior. This matches the “index+body” principle. 

- **Conservative Rule Authoring:** Keep each rule brief and precise. Replace verbose prose with bullet-pointed commands. Use explicit “NEVER do X” sections. Avoid instructing on things the model already does well. In practice, teams audit and trim their CLAUDE.md to essentials (often via the “Rule of Three”: only codify a pattern after the agent fails it repeatedly). 

- **Budget Monitoring:** Continuously monitor context usage. Use tools like `/context` (Claude) or IDE statusbars (Cursor) to ensure rules consume <5–10% of the window. If your project needs >150KB of rules, plan for dynamic strategies. 

- **Rigorous Testing:** For each rule branch, write validation prompts (“should,” “should not”) to ensure the right rules fire. Include “trap” cases to catch false positives. Continuously test on fresh agents to simulate cold-start behavior. Keep a regression suite of coding tasks that cover all rule categories. 

**Failure Modes:**

- **Missed Rule Trigger:** If the routing (glob or semantic) fails to recognize a needed rule, the model may violate that rule. This is common if globs are too narrow or keyword matchlists are incomplete. Mitigation: include fallback coverage (e.g. broad “always-apply” core rules) and use training/testing to surface blind spots.

- **Too-Late Injection:** Hooks inject context *just before* tool calls. If a rule should influence the *current* action, injecting after the turn’s decision point can be too late. As noted in Claude, additionalContext from PreToolUse does affect that turn’s reasoning, but it may not retroactively fix a just-made decision. Care must be taken: route and inject at the earliest sign of a relevant topic, not only at tool execution time.

- **Stale Summaries:** If only summary pointers are provided (in pointer-form CLAUDE.md), the agent might “follow stale advice” if underlying procedures have changed. Without the full text, verification is impossible. One tactic is to include version/timestamp in pointers and teach the agent to re-load or confirm outdated pointers (as Claude’s memory does for facts).

- **Token Bloat Over Sessions:** Even with on-demand loading, some context accumulates. Most frameworks don’t auto-unload. Claude’s rules are re-injected each turn (major waste); current sessions do not decay rules unless manually trimmed. This can silently push out relevant conversation. Regular compaction or restart is needed. 

- **Anchor Context Errors:** Changing context mid-session (by injecting or unloading rules) might confuse the model if it expects continuity. Custom context routers must “preserve the cache” (static prefix) to avoid duplicating system prompt or losing state.

**Strategic Recommendations:**

1. **Adopt a Two-Level Rule System:** We recommend **restructuring rules into an “index” + “body” design**. In practice, shorten the always-loaded instructions to include only rule names/triggers and core directive; store full rule bodies externally. Implement either 
   - *path-scoped rule files* (Claude: `.claude/rules/glob.md`), 
   - or a custom trigger-index in CLAUDE.md with references. 
   This aligns with Bustamante’s principle and Claude’s own tiered context. It trades off some complexity (needing an agent fetch on trigger) for scalability. We are **medium-confidence [emerging]** in its necessity – supported by expert recommendations but lacking large-scale benchmarks.

2. **Leverage Semantic Routing for Domain Rules:** For non-critical domains (logging, debugging, specific frameworks), implement a **router**. Options include: using Claude Code hooks (e.g. PreToolUse) with a keyword/intent matcher, or an external classifier that signals the agent which extra docs to load. Primeline’s example shows ~80% token savings with a keyword hook router, though it is a custom solution [single-source]. We rate this **[emerging]**. If built, it must be tested carefully to avoid missing injections. 

3. **Tier Content via File Types:** Use a decision framework like Claude’s 3-tier model. Store truly universal directives in `CLAUDE.md` (limited to <100 lines), file-specific invariants in `.claude/rules/` (glob triggers), workflow steps as separate *skills*, and reference materials as guideline docs. This ensures no rule is left in “temporary” chat context where it will be compressed away. Confidence is [established] given multiple corroborating guides.

4. **Budget Guards and Monitoring:** Implement context-usage checks. Prioritize converting large always-on rule sets to scoped ones until startup usage is minimal (Primeline suggests <5%). Consider subagents for heavy work (they isolate context). Plan to use compaction/rewind strategies mid-session to avoid silent drop-off.

5. **Test & Iterate:** Develop a comprehensive test suite for rule adherence. Use “cold agent” tests (no prior memory, only new rules) to ensure rules still fire. Treat each rule group like a feature: write example code and verify the agent applies the rule. This step is [established] practice in engineering critical systems, but only limited guidelines exist in literature (mostly blogs). 

**Action Plan:**

1. **Audit Existing Rules:** Inventory all CLAUDE.md content. Tag each rule by *always*, *contextual*, or *rarely-used*. Aim to shrink CLAUDE.md to core 30–50 lines. 

2. **Implement Path-Scoping:** For each rule that is not universally needed, convert it into a `.claude/rules/*.md` file with appropriate `paths:` frontmatter (or put it in an AGENTS.md in a subfolder if using Devin/Cascade). Confirm that editing outside those paths indeed omits the rule (use `/context` to verify).

3. **Build or Integrate a Router:** If use case demands fine-grained routing, implement a keyword-based injector. For Claude Code, write a PreToolUse hook that reads a small JSON config mapping keywords to docs, as in the Primeline example. Otherwise, schedule rule injection by context (e.g. using `@import` in CLAUDE.md when invoking a skill). 

4. **Refactor Rule Bodies:** Move the bulk text of each rule into a separate file or knowledge base. In CLAUDE.md or .cursorrules, replace it with a reference and short summary. Ensure the agent *can* retrieve the body on-demand (via a tool call or hook). Conduct “cold-reader” trials (fresh agent, with only pointers) to confirm no behavior loss.

5. **Monitor and Tune:** Measure context token usage before/after changes using Claude’s `/context` or similar. Watch for rule failure prompts or model drift. If the agent stops following a rule, investigate if the rule’s trigger was missed. Iterate on keyword lists or adjust scopes. 

6. **Stay Informed on Platform Updates:** Track Anthropic’s releases and competitor features. For instance, there is a known gap (issue closed as “not planned”) for lazy-loading rules, but future updates or plugins (e.g. Currying, Cascade Hooks) might offer new mechanisms. Also consider cross-platform formats (AGENTS.md) for multi-agent consistency.

**Sources:** Credible practitioner guides, expert blog posts, and framework docs are cited for all claims. Key references include Roland Huß’s context-engineering survey, Anand Rao’s memory analysis, Primeline’s Claude blog, and Cursor rule docs. All factual statements above are supported by sources; if any claim lacks a verified reference, it is noted as [no source found]. Each key finding is flagged with a confidence tier (established/emerging/speculative) based on source quality and consensus. 

