# DR26-08-26-HUB-03-0935 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. STATE OF THE ART IN CONTEXT COMPRESSION FOR AGENTS

Scope. This is application-level survival of *session state* under a finite context window: what is kept verbatim, what is summarized, what is offloaded, what triggers a shrink, and what numbers exist. It is not another tour of Cline Memory Bank / cursor-memory-bank / AGENTS.md / claude-handoff / magic-compact / AgentOps / self-learning-skills (already mapped). Three layers keep getting mixed in public talk; they are not substitutes.

  L1. Prompt-text compaction (Claude Code /compact, paste-blocks, rolling summaries).
  L2. External memory + retrieval (MemGPT/Letta blocks, LangMem, vector/graph stores).
  L3. Inference-time KV-cache eviction/compression (StreamingLLM, H2O, SnapKV, quantization). Claude Code users cannot control L3. Your two repos are L1, with a bit of L2 discipline (one-home rules, door audit).

A.1 Product / framework mechanisms

MemGPT / Letta (established mechanism, emerging numbers)
  Source: Packer et al., “MemGPT: Towards LLMs as Operating Systems,” arXiv:2310.08560; Letta docs and “sleep-time compute” writeups (2024–2025).
  Model: OS paging. Main context is a working set; overflow goes to recall (conversation DB) and archival (fact store). The agent has tools: core_memory_append/replace, conversation_search, archival_memory_search/insert. Letta later made this “memory blocks”: named, character-capped, always-in-prompt segments (persona, human, custom).
  Verbatim vs summarized: core/blocks = verbatim, bounded. Recalled messages = retrieved snippets, not a full log. Archival = extracted facts, usually rewritten. Sleep-time compute (emerging, 2025): idle-time jobs rewrite blocks from the session, so the *next* working set is curated rather than naively truncated.
  Trigger: context utilization threshold (fill-then-page), plus explicit memory-edit tools, plus (Letta) async consolidation.
  Published quality: MemGPT paper is the rare one with task numbers, not “feels better.” Nested key-value retrieval beyond the window; document QA on corpora larger than context; multi-session chat (MSC) consistency vs truncation. Exact percentages belong in the PDF, not in a remembered summary — treat vendor-adjacent blog numbers as emerging. Independent replication of *compaction fact survival* (your 15/15 probe) is still thin. Confidence: mechanism established; sleep-time emerging; “MemGPT beats GPT-4 long context” as a slogan is oversold.

LangGraph checkpointing + LangMem (established persistence, emerging memory quality)
  Source: LangGraph checkpointer docs (Sqlite/Postgres/MemorySaver); LangChain “trim_messages” / summarization middleware; LangMem SDK (2025).
  Checkpointing is *not* compression. It serializes graph state (messages, channels, pending writes) so a thread can resume. Unlimited thread length is a disk problem, not a context problem. The live prompt still has to shrink.
  Typical L1 shrink in LangGraph apps: (a) keep last K messages verbatim, (b) replace the rest with one summary message, (c) optionally extract memories via LangMem into a store and inject top-k on the next turn.
  Verbatim vs summarized: recent turns + tool results the node author pins = verbatim; older dialogue = summary; “facts about the user/task” = extracted memories (lossy by design). Checkpoints keep the raw history *outside* the prompt, which is the important split your /compact currently does *not* have — Claude Code’s compact discards the pre-compact transcript from the working context and does not give you a first-class off-prompt store.
  Trigger: node-level (after N messages, after token estimate, after a subgraph ends). Not standardized.
  Published quality: no first-party fact-retention benchmark comparable to LongMemEval. Confidence: architecture established; quality numbers mostly absent (speculative if someone quotes a %).

OpenAI Assistants / Responses / AgentKit (established product behavior, opaque compression)
  Source: Assistants API thread-run model (now legacy-leaning); Responses API + tool calling; ChatGPT Memory; AgentKit (DevDay 2025) marketing/docs.
  Assistants threads: the platform reserved the right to drop/summarize old turns. What was preserved was never a public contract. File Search / vector stores = L2 RAG, not session compaction. ChatGPT Memory = user-level durable facts, curated, small, not a session image. AgentKit adds evals, guardrails, and a builder; it does not publish a compaction grammar.
  Verbatim vs summarized: unpublished. Empirical reports (emerging): tool outputs and the last few turns survive; long reasoning traces and earlier file citations do not. Cookbook pattern: “summarize conversation periodically, keep a running state object.”
  Trigger: platform-internal, not user-defined. That is the same failure mode you measured: 354/354 bare /compact runs ignored CLAUDE.md. A vendor summarizer that does not take a schema will not honor a schema.
  Published quality: none that qualify as fact-survival. SWE-bench / OSWorld numbers for Agents are downstream-task, not compaction probes. Confidence: product behavior established; internals speculative.

AutoGen / AG2 teachability (established pattern, narrow eval)
  Source: AutoGen “TeachableAgent” / teachability extra; AG2 (autogen-license fork) docs; related: Magentic-One, AgentChat UpdateContext.
  Teachability is L2 lesson storage, not L1 compact. User (or self) statements of the form “remember that X” are embedded and retrieved when similar tasks appear. Conversation summarization exists as a separate UpdateContext/condense pattern between agent turns.
  Verbatim vs summarized: teachings stored as paraphrased notes + embeddings, not verbatim transcripts. Recent messages kept; old messages dropped or summarized.
  Trigger: teachability on explicit “teaching” moments; context update when token budget exceeded.
  Published quality: original teachability demos (code-gen with user corrections persisting across sessions). No public “after 5 compacts, N facts remain” suite. Confidence: pattern established; production-grade quality data weak.

CrewAI (established marketing taxonomy, weak evidence)
  Source: CrewAI docs — short-term (Chroma), long-term (SQLite of task results), entity memory, contextual memory.
  This is retrieve-and-stuff, not a compaction contract. Short-term = embeddings of recent interactions; long-term = “what worked on past tasks”; entity = names/attributes.
  Verbatim vs summarized: almost everything is summarized/extracted. No pinned working-set schema.
  Trigger: after tasks; retrieval on kickoff.
  Published quality: none of independent weight. Confidence: feature list established; quality speculative.

Devin (Cognition) / Factory (emerging product, speculative internals)
  Source: Cognition product blog (Devin Knowledge / wiki / “Devin Search” over past work, 2025); Factory Droids + org knowledge marketing. No systems paper.
  What is visible: a durable markdown/wiki layer the agent reads/writes; session notes; user-editable knowledge; search over previous jobs. That is L2 organizational memory plus a weak L1 (the current session still truncates).
  Verbatim vs summarized: Knowledge articles are rewritten prose, not a 7-header state image. User-pinned knowledge is the closest thing to verbatim. Session transcripts are not the unit of reuse.
  Trigger: end of task / user save / automatic “learn from this correction” (product copy; internals unpublished).
  Published quality: job-completion anecdotes, not fact-survival. Confidence: existence of a knowledge feature established; compression policy speculative. Do not cargo-cult “Devin remembers” as a design.

Aider (established, measured on coding, not on session facts)
  Source: aider.chat blog (repo-map, token budgets, weak-model summarization); tree-sitter repo map; `/tokens`, `/drop`, `/clear`.
  Aider compresses the *codebase view*, not the *conversation state*. Repo-map is a ranked, token-budgeted skeleton of the repo (verbatim identifiers, summarized structure). Chat history: when the window fills, a weaker model summarizes older turns; recent turns + the files in chat stay verbatim. Conventions files (`CONVENTING.md` etc.) are pinned, analogous to your DO-NOT-TOUCH idea.
  Trigger: token budget vs model limit; user `/compact`-like commands; files dropped from chat.
  Published quality: SWE-bench / aider polyglot leaderboard are *task* numbers. They do not isolate “did summarization drop the constraint the user stated 40 turns ago.” Repo-map token counts are published in blog posts (order of 1–2k tokens typical; verify current). Confidence: repo-map mechanism established and production-hardened; session-fact survival unmeasured.

A.2 Published research that actually bears on /compact

Agentic Context Engineering / ACE (emerging, 2025 — highest relevance to your two repos)
  Source: Zhang et al., “Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models” (2025, Stanford / SambaNova and collaborators). Search title on arXiv rather than trusting a remembered ID.
  Claim: treating context as an evolving *playbook* with incremental deltas beats iterative summarization. Two named failure modes of the thing Claude Code /compact does:
    1. Brevity bias — summarizers are trained to be short, so they drop operational detail (paths, counters, tool contracts).
    2. Context collapse — each rewrite is a lossy codec; after a few generations the playbook becomes generic.
  Architecture: Generator (run the task), Reflector (extract lessons from traces), Curator (merge *deltas* into the playbook; no full rewrite). This is almost a formalization of claw-retro’s classify → one-home → door-audit, except ACE refuses to recopy the whole memory image every cycle.
  Quality: paper reports gains on an agent benchmark (AppWorld) and a domain (finance) vs strong prompt-optimization baselines (including GEPA-class methods). Quote numbers from the PDF; do not circulate remembered “+10.6%.” Confidence: emerging (new, vendor-adjacent authors, but the *failure mode* matches your 354/354 measurement).

Hierarchical / recursive summarization (established)
  Sources: Wu et al., “Recursively Summarizing Books with Human Feedback” (OpenAI, 2021); MEMWALKER (Chen et al., “Walking Down the Memory Maze,” 2023); AutoCompressors (Chevalier et al., 2023); classic hierarchical IR.
  Pattern: leaf chunks summarized → summaries of summaries → a tree you can walk. Verbatim lives only at the leaves (if you kept them). Recursive summaries are the *enemy* of PATHS & VALUES and COUNTERS: numbers and absolute paths die at generation 2.
  Quality: book-length summary evals are human preference / QA on the book, not session-state probes. MEMWALKER reports long-doc QA improvements vs stuffing. AutoCompressors report task scores under summary-vector compression with a quality drop vs full context. Confidence: established as a method; established that quality degrades with depth — which is exactly why your 7-header block must not be re-summarized as prose.

KV-cache-aware compression (established at systems layer, mostly unavailable to you)
  Sources: StreamingLLM / attention sinks (Xiao et al., 2023); H2O Heavy-Hitter Oracle (Zhang et al., 2023); SnapKV (Li et al., 2024); PyramidKV; Quest; KIVI / KV quantization; FastGen.
  These drop *tokens in the cache*, not sections of a paste-block. They preserve attention sinks + recent tokens + heavy hitters. They do not know that a line is a COUNTER. Relevance: if a future Claude Code exposed “pin these spans in the KV cache,” your DO-NOT-TOUCH section would map onto it. Today it does not. Confidence: established in research; speculative as a Claude Code lever.

“Lost in the middle,” long-context benches, agent memory benches (established)
  Sources: Liu et al., “Lost in the Middle” (2023); RULER (Hsieh et al. / NVIDIA, 2024); LongBench; ∞Bench; HELMET (2025); LoCoMo (Maharana et al., 2024); LongMemEval (Wu et al., 2024/25).
  These measure *whether the model can use a long prompt*, not *whether a summarizer preserved the facts you needed*. Saturated needle-in-a-haystack made this distinction painfully obvious by 2024. Confidence: established.

Other 2024–2025 memory systems (emerging)
  A-MEM / “Agentic Memory” (Xu et al., 2025): notes as an evolving graph, Zettelkasten-like, agent decides what to link. MemoryOS, G-Memory (multi-agent), Zep/Graphiti (temporal KG), Mem0 (memory layer; vendor LoCoMo numbers — treat as emerging/vendor-reported). Generative Agents (Park et al., 2023) still the cleanest *reflection loop* paper: memory stream (verbatim observations) → periodic reflections (summaries) → retrieval by recency/salience/relevance. Voyager (Wang et al., 2023): skill library, not context compact. Reflexion (Shinn et al., 2023): verbal self-critique into an episodic buffer, usually within a trial, not into a durable config with a door.

A.3 What is actually preserved — synthesis

  Always-verbatim in systems that work: identity/persona, hard constraints, absolute paths, credentials-by-reference, counters, tool schemas, “do not do X” rules, the current uncommitted goal. Letta blocks, Aider conventions, ACE playbook items, your 7 headers when inlined.
  Always-lossy: dialogue flavor, exploration dead-ends, raw tool dumps, chain-of-thought, the fifth restatement of the same plan.
  The industry default compressor (LLM summary of the transcript) is optimized for the first list to die. Your paste-block exists because you measured that. ACE independently named the same bug in 2025. Confidence: established as an empirical pattern (your 354/354 + ACE’s collapse argument + every Assistants-thread complaint).

B. EVALUATION — HOW SERIOUS TEAMS MEASURE COMPACTION

B.1 What exists, and what it is actually for

  Needle-in-a-haystack (Kamradt) and variants. Insert a unique sentence at varying depths; ask it back. Saturated for frontier models on *raw long context*. Almost useless for compaction quality: a summarizer that drops the needle scores 0, which you already know, and a summarizer that keeps one planted sentence can still destroy PATHS & VALUES. Confidence: established as a long-context test; established as the wrong test for /compact.

  RULER / HELMET / LongBench / ∞Bench. Multi-hop, variable tracking, aggregation, hard negatives. Still “can the model read a long prompt,” not “did the codec keep the state.” Useful as a *negative*: if even full-context models fail multi-hop variable tracking, a 20:1 summary (your 54,358 → 2,522) will fail it unless those variables are lifted into a structured section. Confidence: established.

  LoCoMo (Maharana et al., 2024). Very long multi-session conversations; QA, event summarization, multimodal. Closest *conversational* memory bench. Weak for coding agents: almost no PATHS, COUNTERS, tool contracts. Confidence: established for chat memory; poor fit for Claude Code sessions.

  LongMemEval (Wu et al.). ~500 questions over constructed user histories, typed: single-session user/assistant/preference, multi-session reasoning, knowledge update, temporal, abstention. This is the current serious chat-assistant memory bench. Commercial assistants in the original study leaked or forgot across sessions; RAG helped some types and hurt others (classic: retrieval misses the updated fact). Still not a *compaction codec* bench: the history is available to the system under test; the question is whether the *memory pipeline* surfaces it. Confidence: established as the chat-memory standard; emerging as something agent-harness people cite; not a drop-in for compact-canon.

  FactScore / atomic-fact precision (Min et al.) and SummEval. Decompose a summary into atomic facts, check support against a source. This is the right *unit* for compaction: your 15 seeded facts are homemade FactScore without the NLP pipeline. Downstream-task success (SWE-bench after compact, AppWorld after playbook update) is the right *outcome* metric and the one ACE uses. LLM-as-judge on “is this summary faithful” is cheap and systematically kind to fluent lies. Hamel Husain / Eugene Yan eval writing: always prefer a pinned probe set over a judge. Confidence: established in eval culture; rarely applied to session compact.

  Production teams (Anthropic, OpenAI, Cursor — inferred from public eval talk, not from a compaction whitepaper). They measure: (1) downstream task pass, (2) user thumbs, (3) sometimes “memory regression” suites of planted user facts. They do not publish a session-compaction fact-survival number. Your 7/7 headers + 15/15 facts + 354/354 ignore-rate is, as of this writing, more specific than anything those labs have put in public. Confidence: emerging (we see eval *culture*, not a published compact protocol).

B.2 Does a “session compaction fact survival” benchmark already exist?

  No public benchmark matches the object you actually ship: a structured session image, run through a vendor /compact (or a summarizer), then probed with the original transcript gone. Closest cousins:
    - LongMemEval / LoCoMo (chat, not coding-session state).
    - ACE’s ablation of rewrite vs delta (playbook-level, not Claude Code /compact).
    - Recursive book summarization evals (wrong domain).
    - Your own compact-canon measurements (the current gold, but private-N=1).
  Confidence: established that the exact bench is missing; that is a contribution hole, not a reading failure.

B.3 What a credible public bench would look like

  Name (straw): SCFS — Session Compaction Fact Survival.
  Design, stealing the bits that made LongMemEval / FactScore / RULER respectable:

    1. Fixtures, not live chats. 30–50 synthetic-but-realistic Claude Code transcripts (auth, multi-file refactor, failed-then-fixed test, policy decision, counter that incremented, absolute Windows path, tool schema, a revoked decision). Each fixture ships a *manifest* of planted items tagged by section (DECISIONS / TODO / NOW / PATHS & VALUES / COUNTERS / OPEN / TOOLS & CONTRACTS) and by class (constraint, path, number, proper name, schema, temporal, negated/revoked).
    2. Codec under test. (a) bare /compact, (b) inline 7-header paste-block then /compact, (c) naive LLM summary, (d) last-K trim, (e) ACE-style delta playbook, (f) MemGPT-style block replace. Same model family for fairness; also a “vendor black box” lane for Claude Code.
    3. Probe without the source. After compaction, a frozen probe list: exact-string asks (“what is the vault path”), numeric asks (“what was the token count before compact”), negated asks (“which approach did we reject”), multi-hop (“which counter must be bumped before the next deploy”), abstention (“what is the user’s middle name” — must say unknown).
    4. Metrics. Header survival (k/7). Fact survival overall and per class. False memory rate (facts that appear in the compact but contradict the fixture — this is the dangerous one). Generation-N survival: compact the compact 3× and 7× (the ACE collapse test). Token ratio. Downstream: a 5-step continuation script that fails if a planted path/counter is wrong.
    5. Anti-cheat. Probes not visible to the compressor. Planted facts look like real session noise (not “REMEMBER THIS NEEDLE”). A small hidden-canary set rotated so people cannot overfit the public split.
    6. Reporting. Not a single score. A slice table: class × codec × generation. Your headline 15/15 is generation-1, structured, in-distribution. The number that would change minds is generation-5 PATHS & COUNTERS survival.

  Could you adopt instead of inventing? Adopt LongMemEval’s *question typology* and FactScore’s *atomic facts*; do not adopt their corpora. Contribute SCFS as a tiny eval harness next to compact-canon, with 10 public fixtures and a private holdout. That is the “serious team” shape: published protocol, small N, slice metrics, no LLM-judge as the primary score. Confidence: speculative as a community standard (it does not exist yet); established as eval methodology (this is just LongMemEval+FactScore aimed at /compact).

C. RETRO / SELF-IMPROVEMENT LOOPS IN PRODUCTION

Beyond Reflexion. Reflexion writes a verbal lesson into an ephemeral buffer for the next trial of the *same* task. Production loops write into a durable artifact that the *next* session loads. Different object, different failure mode (bloat and staleness, not just “did this trial improve”).

C.1 Documented industrial practice

  Anthropic (established guidance, emerging productization).
  Sources: “Building Effective Agents”; Claude 4 / Claude Code docs (CLAUDE.md, skills, hooks); 2025 engineering posts on *context engineering* (compaction, structured notes, sub-agent isolation, just-in-time retrieval); “effective harnesses for long-running agents” style writeups where the agent is told to keep a feature list + git as the source of truth and a progress file across context resets. Claude.ai project memory / user memory (2025): small, user-visible, editable. The official shape is: put durable rules in files the next prompt will load; do not trust the transcript; compact is a last-resort shrink of the *working* set. They do *not* publish a door-audit or a usage counter on CLAUDE.md bullets. Your claw-retro is stricter than their public guidance.

  OpenAI cookbook / ChatGPT Memory / AgentKit (emerging).
  Pattern: running state object + periodic summary; user-level memories with a UI to delete; evals as a first-class AgentKit object. Memories are curated and few. No public “agent rewrites its own system prompt every night” loop except research demos (OPRO, Promptbreeder), which are prompt optimization, not postmortem rule capture.

  Cursor / Windsurf (emerging, user-visible memories).
  Cursor Memories (2025): short natural-language items, extracted from sessions, shown in settings, user-deletable. Rules files (`.cursorrules` → AGENTS.md) remain the *manual* durable channel. Windsurf Cascade Memories: similar auto-extract + UI. Guardrail is social/UI, not expiry: the user sees the list, the list is small, there is no skill-door. Bloat control = “keep it tiny so it always fits.” That is the opposite of an unbounded Bible.

  Devin Knowledge / Factory (emerging, thin docs).
  Wiki the agent writes; humans edit; later jobs retrieve. Guardrail = human-in-the-loop wiki, not counters. Stale pages are a known complaint in user forums (emerging, anecdotal).

  Letta (established in product).
  The agent *literally* edits its own blocks with tools. Guardrail = character cap per block (hard). That is the most production-real “self-modifying prompt” system. Stale facts get replaced because the block cannot grow forever. No usage counter in the open docs; the cap *is* the counter.

  Generative Agents / Voyager / teachability (established research practice).
  Reflection summaries on a cadence; skill library grows monotonically (Voyager); teachings accumulate in a vector DB (AutoGen). Guardrails in the papers are weak: retrieval relevance, not expiry. Voyager’s skill library can accumulate dead skills; they mitigate by execution success, which is a *verification* gate — the important bit.

  Your claw-retro vs this landscape. You already have the pieces serious papers keep rediscovering: recall/reconcile, deterministic inventory (git — rare; most loops inventory *tokens* not *files*), keep/one-off/promote, one home, door audit, archive note, emit compact block. The ACE Curator is your “one home”; the door audit is stricter than ACE. What they have that you do not, publicly: (1) incremental delta instead of full rewrite, (2) hard caps, (3) execution-verification before a rule is promoted (Voyager), (4) user-visible memory lists (Cursor). Confidence: comparison established at the pattern level.

C.2 Guardrails against bloat and staleness (what is actually used)

  Hard caps (Letta blocks, MEMORY.md line budgets, Cursor’s “keep memories short”). Established, works, brutal.
  User visibility + delete (ChatGPT, Cursor, Windsurf, Devin wiki). Established, depends on a human bottleneck.
  Retrieval instead of always-load (MemGPT archival, LangMem, RAG). Established; stale rules still poison retrieval if they match.
  Verification before promote (Voyager execute-success; your /tt spirit; ACE Reflector looking at traces). Emerging as a requirement, still rare in product.
  Usage counters / 30-day unused (your canon). Almost nobody in public agent products does this. It is closer to SRE feature flags than to LLM papers. Emerging as *your* differentiator.
  Expiry. Almost never automatic in production memories. Research systems sometimes decay by recency (Generative Agents’ retrieval score). Automatic expiry of *rules* is dangerous (a 6-month-old “never push secrets” is not stale). Speculative as a default; established that *facts* and *rules* need different clocks.
  One-home / no duplicates. Preached everywhere, enforced almost nowhere. Your door audit is the enforcement mechanism papers lack.

  Failure mode to treat as established: unbounded self-write into always-loaded files. You already named it (always-loaded bloat). Cursor’s answer is “don’t put it in always-loaded.” Anthropic’s public answer is “CLAUDE.md, but be careful.” ACE’s answer is “append deltas, don’t rewrite.” All three are compatible.

D. OPTIMIZATION IDEAS FOR compact-canon + claw-retro

Rank is effort × payoff for a small team that already has 7 headers, a live 15/15 probe, and a door audit. Payoff here means: (i) generation-N fact survival, (ii) less rule bloat, (iii) something you can publish as a number.

  1. Generation-tagged, delta-only compact (ACE curator). Effort: M. Payoff: S.
     Do not rewrite the 7-header block from the transcript every /compact. Diff it. New facts = appended lines with `gen:N`. Unchanged PATHS/COUNTERS/TOOLS = byte-identical. Summarize only the narrative residue (NOW/OPEN) that is allowed to collapse. This directly attacks context collapse, which is the bug that kills you on session 4, not session 1. Your 54,358 → 2,522 is gen-1 success; the product risk is gen-3. Confidence: emerging (ACE), maps 1:1.

  2. DO-NOT-TOUCH / pin spans. Effort: S. Payoff: S.
     A header or fenced region the paste-block instructions mark as “copy verbatim, do not paraphrase.” Put PATHS & VALUES, COUNTERS, TOOLS & CONTRACTS, and irrevocable DECISIONS there. Analogues: Letta block caps, Aider conventions, KV “pin these tokens” if it ever exists. Your 354/354 finding says the instruction must live *inside* the compact payload, not in CLAUDE.md. Confidence: established need (your measurement).

  3. Traveling probe / post-compact self-test. Effort: S. Payoff: S.
     After every compact, run 10–15 asks against the new block *before* continuing work: one fact per section + 2 negated + 1 counter. Fail closed: if PATHS or COUNTERS drop, abort and restore the pre-compact block from the retro archive. This is SCFS-mini, in-process. You already know how to seed facts. Publish the harness. Confidence: established eval practice; not yet wired to /compact.

  4. Per-section token floors, not just headers. Effort: S. Payoff: M–S.
     Bare 7 headers still lose if TODO prose eats the budget. Assign floors (e.g. COUNTERS ≥ 8 lines, PATHS ≥ 10, TOOLS ≥ schema-complete) and a hard ceiling on NOW. Aider already budgets the repo-map this way. Confidence: established in Aider; untested in compact-canon.

  5. Outcome-mode retro (“did last week’s rules fire?”). Effort: M. Payoff: S for claw-retro, M for compact.
     Retro currently classifies new residue. Add a pass over rules promoted in the last N retros: door still exists? usage counter > 0? contradicted by a later DECISION? This is Voyager’s execution-success gate plus your unused-30d rule, made into a ritual instead of a night job. Confidence: emerging; this is the actual self-improvement loop, not “write more CLAUDE.md.”

  6. Split clocks: rules vs facts vs counters. Effort: S. Payoff: M.
     COUNTERS and PATHS: freshness timestamp, warn at 1 session, expire at 3 unless refreshed (they go stale by *drift*, not by time). DECISIONS of type constraint (“never X”): no auto-expiry; require explicit supersede. OPEN: expire at end of session. Mixing these under one “automatic rule expiry” will delete the constitution. Confidence: established as a need; speculative as exact thresholds.

  7. Confidence / freshness tags per line. Effort: S. Payoff: M if the tags have a consumer; L if they are decoration.
     `path: E:\...  conf:high  seen:gen12  src:git`. Only worth it if compact instructions say “drop conf:low before dropping conf:high” and retro can challenge low-conf. Without a consumer this is bloat. Confidence: emerging.

  8. Cumulative multi-cycle preservation rule, explicit. Effort: S. Payoff: M–S.
     One sentence in the paste-block: “If a line has survived ≥2 compacts, copy it byte-identical; do not paraphrase survivors.” Cheap approximation of (1) if you do not want a full diff curator yet. Confidence: speculative as a prompt rule (vendor summarizer may ignore it, per 354/354); test it with the traveling probe, do not assume.

  9. Deterministic inventory already exists — lift COUNTERS from git/tests, not from the model. Effort: M. Payoff: M.
     Anything a script can count (files changed, tests, token estimates) should be written into COUNTERS by claw-retro *before* compact, so the LLM is not the source. This is your git inventory generalized. Confidence: established (you already do git).

  10. Do *not* auto-expire rules in v1; do expire unused *always-loaded* lines into archive. Effort: S. Payoff: M.
      Matches your MEMORY.md hygiene. Cursor-style small always-loaded + retrieval for the rest. Automatic tombstones without a door-check will violate “every rule needs a door” in reverse (dead doors). Confidence: established in your own canon.

  Deprioritize: LLM-as-judge “was this a good compact”; extra headers beyond 7 (diminishing, and the vendor ignores instructions anyway); graph memory (Zep etc.) — wrong layer for a paste-block; KV-cache papers — no API.

  Rank by leverage for *next week* (small team): 2 pin, 3 traveling probe, 8 survivor-copy rule, 4 section floors, 6 split clocks, 1 delta curator, 5 outcome-mode retro, 9 script-written counters, 7 tags only with a consumer, 10 archive unused always-loaded (already spiritually done).

E. WHERE THE CONVERSATION LIVES (2025–2026)

No tools on this pass, so URLs are canonical homes + search keys, not scraped issue numbers. Do not invent `#12345`. Confidence on “this venue is live” = emerging unless noted; confidence on exact issue IDs = speculative (omitted).

  GitHub
    anthropics/claude-code — issues/discussions matching `compact`, `context window`, `CLAUDE.md ignored`, `memory`, `pin`. Why: this is the actual codec you wrap. Your 354/354 + inline-block result is the comment that belongs here, with the probe harness attached. Useful contribution: a failing fixture (“bare /compact drops these 15 facts; inline 7-header keeps them”) plus a feature request for vendor-side pin spans / schema-aware compact. That is better than another “please remember more” issue.
    langchain-ai/langgraph, langchain-ai/langmem — summarization middleware, memory tools. Contribute: SCFS fixture results vs their trim+summarize node.
    letta-ai/letta — memory block semantics, sleep-time. Contribute: comparison of blocks vs 7-header paste; character-cap vs section floors.
    Aider-AI/aider — repo-map + chat summary. Contribute: “session fact survival” as a sibling to repo-map token blogs.
    dexhoris / HumanLayer `12-factor-agents` (search that name) — Factor: “own your context window.” Why: this is where practitioners who reject hidden vendor memory hang out. Useful: a factor-shaped writeup of compact-canon (own the state image, don’t outsource it).
    Avoid re-posting into the hobbyist memory-bank cluster you already mapped.

  HN / blogs (high traffic, 2025 context-engineering wave)
    Search HN: “context engineering”, “Claude Code compact”, “MemGPT”, “12-factor agents”, “long-running agents.” Why: 2025 is when “context engineering” displaced “prompt engineering” in this crowd (Anthropic engineering blog + LangChain blog + independent practitioners). Useful contribution: a Show HN of compact-canon with the 354/354 and 15/15 numbers in the first paragraph — those are the only two numbers in this niche that look like measurements.
    Simon Willison’s weblog — long-running agent notes, Claude Code coverage. A concise “compaction is a codec, here is a fixture” post will get syndicated.
    Latent Space (swyx) newsletter/podcast — memory/context episodes. Useful: offer the 7-header + probe as a segment, not a pitch deck.
    Hamel Husain evals writing / Eugene Yan — for SCFS design review before you freeze a public bench.

  Discord / forums
    Letta Discord — people who already think in blocks.
    LangChain Discord — checkpointer vs summarization confusion; teach the L1/L2/L3 split.
    Aider Discord — token-budget culture, closest to section floors.
    Anthropic Discord / Claude Code GitHub discussions — vendor ear.
    r/ClaudeAI, r/LocalLLaMA — high traffic, low signal; use only to drop the measurement, not to design.
    Cursor forum / Windsurf Discord — memories UI complaints (bloat, wrong memories). Useful as *negative* evidence for auto-extracted rules without doors.

  arXiv clusters (cs.CL / cs.AI)
    Query packs: `agentic context engineering`; `long-term memory LLM agent` (LongMemEval, LoCoMo, A-MEM, MemoryOS); `KV cache compression` (SnapKV, H2O, StreamingLLM) — cite as L3, don’t confuse reviewers; `recursive summarization` / MEMWALKER; Reflexion / Voyager / Generative Agents as the loop ancestors. Why: ACE is the paper your repos should be in conversation with. Useful contribution: an “empirical note” (even a 4-page workshop) with SCFS generation-1..5 numbers on Claude Code /compact. That is how a GitHub hobby object becomes a cited codec.

  Newsletters / talks
    Anthropic engineering blog (context engineering, long-running agents).
    LangChain blog (LangMem).
    Letta blog (sleep-time compute).
    Interconnects (Nathan Lambert), Ahead of AI (Raschka), Import AI (Jack Clark) — they will cover ACE-class papers; they will not cover your GitHub unless there is a number.
    AI Engineer / AIE World’s Fair 2025–2026 talks on harnesses, evals, context windows (Dex Horthy, Hamel, swyx). Useful: a 10-minute “compaction as codec” talk with the 7 headers on one slide and the gen-5 collapse chart on the next.

  What a useful contribution looks like, everywhere
    Not: another memory taxonomy.
    Yes: (fixture, codec, generation, slice table). Your two repos already produce that if you instrument gen-N and ship 10 public transcripts. Lead with 354/354 and 15/15; those are the door-openers. Then ask the vendor for pin spans. Then cite ACE as the independent discovery of the same collapse.

F. WHAT WE WOULD CHANGE IN THE TWO REPOS NEXT WEEK

Not a rewrite. Six patches, in order, each with a number attached. Confidence on “this is the right next week” = high for 1–4 (they follow from your own measurements + ACE’s collapse claim); medium for 5–6.

  compact-canon
    1. Pin spans. Mark PATHS & VALUES, COUNTERS, TOOLS & CONTRACTS, and constraint-type DECISIONS as copy-verbatim in the paste-block itself. Re-run the live /compact probe. Pass = 7/7 headers AND 100% of pinned-class facts. Fail = you have just reproduced 354/354 on a subset; publish the fail.
    2. Survivor rule. Lines that already lived in the previous compact block get a `gen:` tag and the instruction “byte-identical copy.” Probe generation-3 on the same 15 facts (this is the ACE test you do not yet have). Report gen-1 vs gen-3 vs gen-5. If gen-5 PATHS drop, delta-curator becomes next week+1, not a blog idea.
    3. Traveling probe. A 15-item JSON manifest ships with the block; a 20-line script asks the model (or regex-checks the block) immediately after compact. Exit non-zero on pin-class loss. This is the public SCFS-mini. Put 5 anonymized fixtures in the repo so other people can run codecs against them.
    4. Section floors. Minimum lines/tokens per header in the compact instructions. Re-measure token ratio; you are allowed to go above 2,522 if COUNTERS start surviving gen-3. Do not worship the 20:1 ratio.

  claw-retro
    5. Outcome pass. Before classifying new residue, scan last N promoted rules: door still executable? usage counter? contradicted? Emit KEEP / STALE-ARCHIVE / SUPERSEDE. No automatic delete of constraint-class rules.
    6. Write COUNTERS and PATHS from git/env/scripts in the retro, *then* emit the compact block, so the compressor is not the source of numbers. Split clocks in the archive note: facts expire, constraints supersede.

  Explicitly not next week
    Graph memory, KV-cache papers, extra headers, LLM-judge quality scores, auto-expiry of rules, re-survey of the memory-bank neighborhood, claiming LongMemEval as “our bench.”

  Public posture (if Mission-2 / loud-by-default applies to these two repos)
    One artifact: “Claude Code /compact is a lossy codec. Bare compact ignored project instructions 354/354. An inline 7-header block kept 7/7 headers and 15/15 seeded facts at 54,358 → 2,522 tokens. We still do not know gen-5. Fixtures and a probe are in the repo. Closest paper: ACE (do not rewrite the playbook).” Post that in anthropics/claude-code (issue with fixture), a Show HN, and an ACE-citing discussion. That is the contribution, not another taxonomy.

  Sources for the two numbers you already own: your compact-canon measurements (established *for your harness*). Sources for the change list: ACE 2025 (emerging, independent collapse claim); Letta block caps / Aider token budgets (established analogues); LongMemEval+FactScore (established eval shape, new target); your 354/354 (established that instructions outside the payload do not bind the codec).

  Confidence recap
    Established: L1/L2/L3 split; vendor summarizers ignore out-of-band instructions; recursive rewrite collapses operational detail; LongMemEval/LoCoMo/FactScore as eval ancestors; Letta blocks, Aider repo-map, Anthropic “put it in a file”; missing SCFS bench.
    Emerging: ACE as the right paper; Cursor/Windsurf/Devin memories; LangMem; sleep-time compute; AgentKit; gen-N as the metric that matters.
    Speculative: internals of Devin/Factory/Assistants truncation; automatic rule expiry; exact 2026 GitHub issue IDs; any remembered percent from a paper not opened in this pass (treat as a pointer, not a citation).
