# DR26-07-27-HUB-01-2203 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
type: original
source: Grok Heavy (SuperGrok, [email hidden])
dr_id: DR26-07-27-HUB-01-2203
topic: "Практики оптимизации always-loaded файлов правил (CLAUDE.md codex + MEMORY.md индекс)"
date_collected: 2026-07-28
chat_url: https://приватный чат
proof_of_work: "Thought for 1m 7s, 504 sources"

captured_by: claude-code (hub хаб)
verbatim: true
prompt_echo_trimmed_at: "Thought for" (offset 7161)
---

Thought for 1m 7s

Report: State of the Art on Always-Loaded Rule/Memory Files (CLAUDE.md-class codex ~100KB; MEMORY.md-class index ≤135 lines / ≤20KB)
Deep research synthesis, prioritizing 2025–2026 sources. All load-bearing claims cite URLs actually retrieved. Confidence tiers: [established] / [emerging] / [speculative] / [fringe]. Single-source or low-authority flagged.

1. STRUCTURE → ADHERENCE EVIDENCE

Lost-in-the-middle / primacy-recency remains the dominant positional effect. Liu et al. (TACL 2024, originally 2023 arXiv:2307.03172) established the U-shaped curve on multi-document QA and key-value retrieval: accuracy highest when relevant information is at the beginning or end of context, significantly degraded in the middle (often below closed-book baseline). Quote: “performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts.” This is still the reference in 2025–2026 practitioner and survey writing (e.g., agentpatterns.ai 2026-07, understandingdata.com 2026-02). Later work (arXiv 2508.07479, 2025) finds the classic LiM strongest when input ≤50% of context window; beyond that primacy weakens while recency holds, shifting to a distance-from-end bias. [established]⁠Direct.mit

Practical implication for 10–100KB always-loaded files: Place highest-priority rules (safety, never-violate, core identity) at the absolute top. Optionally restate 1–3 critical constraints near the end (recency). Middle sections are for stable reference material or lower-priority thematic content. Adding middle material pushes existing rules deeper into the low-attention zone. [established application of Liu et al.]

Markdown headers vs XML tags vs plain lines. Anthropic official docs for CLAUDE.md recommend “use markdown headers and bullets to group related instructions. Claude scans structure the same way readers do: organized sections are easier to follow than dense paragraphs.” (code.claude.com/docs/en/memory.md, ~2026-07). XML tags remain preferred for complex system prompts (boundaries, untrusted content isolation) per Anthropic prompt-engineering guidance. Ablations on format for long instruction lists are sparse and mixed; one 2026 study (referenced in practitioner summaries) finds instruction count dominates, with format effects small and model-specific. No large controlled ablation specifically measuring headers vs plain on 100KB agent rule files was located. Practitioners converge on markdown for dual human/model readability. [emerging for markdown preference; established for XML in system prompts]

Ordering by priority vs theme; TOP blocks. No high-powered ablation of “TOP-20 priority block + thematic §§0–N” vs pure thematic or pure priority list was found. Primacy bias supplies the rationale for a short TOP. Anthropic’s July 2026 context-engineering post treats “put it all upfront” as a prior-generation myth now replaced by progressive disclosure. [emerging practice]

Contrarian / cargo-cult view (preserved): Anthropic itself (2026-07-24) reports removing >80% of Claude Code’s system prompt “with no measurable loss on our coding evaluations” and reframes earlier practices (enumerate many rules, front-load everything, memory in CLAUDE.md) as myths for the Claude 5 generation; prefer model judgment + progressive disclosure + auto-memory. Independent commentary (headgym.com 2026-05) labels elaborate sectioning/personas without external verifiers “cargo-cult engineering.” Some X practitioners report largest gains from aggressive deletion rather than better headers (“Most of my CLAUDE.md wins came from deleting lines”). Short flat imperative lists of hard constraints can outperform long structured files when instruction count stays low. [emerging dissent; steelman: structure without mechanism is decoration]⁠Claude

Instruction-count ceiling. HumanLayer (2025-11) and related analyses: frontier thinking models follow ~150–200 instructions with reasonable consistency; smaller models decay exponentially. Claude Code’s own system prompt already consumes ~50. Beyond this, adherence degrades uniformly. Perfect simultaneous adherence collapses at higher counts. Your ~100KB codex is far outside consensus safe density. [emerging, corroborated by multiple practitioner reports + related multi-constraint papers]

2. DUAL AUDIENCE

Official Anthropic guidance: keep CLAUDE.md “short and human-readable.” Cline’s Memory Bank is explicitly dual-audience hierarchical markdown (projectbrief.md → productContext.md → activeContext.md → systemPatterns.md → techContext.md → progress.md) that both humans and the agent read; the agent is instructed to treat it as its sole persistent memory after reset.⁠Docs.cline

Practices for one source:

Keep the always-loaded source model-optimized (imperative, short lines, concrete, scannable).
Use block HTML comments (<!-- human notes -->) which Anthropic strips before injection; humans see them, model does not.
Prefer progressive disclosure / @import or topic files so the human-readable detail lives outside the always-loaded payload.
Generated human views (dashboard, summary, rendered HTML) are better when the source must stay ultra-dense (your 150-char index lines) or when multiple tools consume the same rules. One canonical source + view generators avoids drift; dual-maintained files drift within hours under parallel writers. [emerging consensus]

When the source itself must be human-friendly (onboarding new owners), Cline-style hierarchical readable files win; for pure model density, generate the human view.

3. RECENCY MARKERS

Anthropic auto-memory records an ISO modified timestamp in YAML frontmatter of memory files; “the timestamp shows how current the fact is, both to you and to Claude.” A community tool injects aggressive current-date blocks into CLAUDE.md to combat temporal hallucination. [single-source / emerging for auto-memory timestamps]

Inline dates (“⭐25.07”) vs alternatives:

Pros of inline: co-located with the rule (recency bias may help conflict resolution); simple provenance.
Cons: token tax every session, visual noise, multi-writer inconsistency, no automatic enforcement.
Prefer: valid_as_of or modified in frontmatter of the pointed-to note + periodic prune; or explicit “supersedes: older-rule-id” chains; or changelog + archive section for DONE. No measured evidence was found that inline dates demonstrably improve conflict resolution over frontmatter + prune. [speculative for inline benefit; emerging for frontmatter]
4. INDEX-LINE GRAMMAR

Anthropic MEMORY.md hard-caps the loaded index at the first 200 lines or 25KB (whichever first); excess is dropped on next load. Claude is instructed to keep one line per entry and move detail to topic files. Community practice (and reverse-engineering of the byte budget) converges on ~150 characters per line as a practical ceiling so that ~60–130 entries fit comfortably. Your ≤135 lines / ≤20KB / ≤150 chars is tightly aligned. [established for the hard cap; emerging for the 150-char heuristic]⁠Code.claude

One-line pointer vs short paragraph: Index files use one-line “- Title — hook/gist”. Short paragraphs belong in the pointed-to notes. Markdown links or Claude @path imports are the standard anchor syntax for on-demand load. Relative paths preferred. [emerging, widely observed]

5. TAXONOMIES

Convergent schemas across CLAUDE.md / AGENTS.md / .cursorrules / CONVENTIONS.md / Cline Memory Bank:

Project identity / purpose / “why” (1 short paragraph)
Tech stack + key commands (build/test/lint that cannot be inferred)
Architecture / directory map / boundaries
Conventions (only non-defaults)
Hard rules / constraints / NEVER-ALWAYS (safety, non-negotiables)
Gotchas / locked decisions / current focus (more volatile)
Pointers to skills / detailed rules / on-demand files

TOP block size: Practitioner TOP/“Rules Never Violate” sections are typically 5–15 atomic hard rules. No measured LLM equivalent of Miller’s 7±2 was found; the relevant limit is total instruction count (~150–200 frontier ceiling) and primacy. A TOP-20 of one-liners is borderline feasible under primacy but risks uniform decay. TOP should replace (via progressive pointers) rather than duplicate body entries. [emerging]

6. ECOSYSTEM SCAN
Anthropic CLAUDE.md (2026-07-24 blog + docs): Keep lightweight; spend tokens on codebase gotchas, not the obvious; progressive disclosure + tree of files/skills loaded on demand; target under 200 lines (longer reduces adherence); “Bloated CLAUDE.md files cause Claude to ignore your actual instructions!”; /doctor for rightsizing. Removed >80% of own system prompt with no measurable loss.⁠Claude +1
MEMORY.md: Hard 200 lines / 25KB index loaded every session; rest on-demand topic files.
OpenAI AGENTS.md: Layered discovery (global → repo root → nested); keep small; Codex has a 32 KiB soft/hard truncation risk on auto-discovered files (practitioner reports of silent truncation).
Cursor: Legacy single .cursorrules deprecated; prefer .cursor/rules/*.mdc with frontmatter (alwaysApply, globs, description). Keep individual files under 500 lines; scope aggressively to avoid token tax.
Windsurf / Cascade: Rules in .windsurf/rules or global; 12k char limit per file; prefer concise bullets/markdown; XML tags optional for grouping.
Aider: Simple CONVENTIONS.md loaded read-only via --read or config; keep short imperative.
Cline Memory Bank: Hierarchical dual-readable markdown files; agent must read all at task start.
Token budgets: community consensus for always-loaded is well under a few thousand tokens; 100KB is outlier territory requiring progressive disclosure. [established for Anthropic/Cursor/OpenAI docs; emerging for measured truncation reports]
7. ANTI-PATTERNS + MULTI-WRITER DRIFT

Documented failure modes:

Laundry-list / “central repository of every practice” (Anthropic explicitly calls this a myth).
Duplication across CLAUDE.md / AGENTS.md / .cursor/rules leading to contradictions (model picks arbitrarily).
Structure drift under parallel agent sessions (intake lines land in wrong sections within hours).
Auto-tidy robots fighting hand structure.
Over-constraint / conflicting messages across system prompt + rules + user request (Anthropic observed this internally).

Mitigations [emerging practice]:

Write gates / linters (claudelint validates size, imports, structure).
Canonical section registry (machine-readable list of allowed headers).
Append-only “inbox” / “intake” section at file end for parallel writers; scheduled re-sort pass (human or agent) into thematic sections.
Path-scoped / subdirectory CLAUDE.md or .claude/rules/ so writers touch narrow files.
Private namespaces + append-only event logs for multi-agent coordination (file-based inbox patterns).
Progressive disclosure so parallel sessions write topic notes, not the always-loaded index.
One canonical source (often AGENTS.md) with tool-specific thin adapters that import it.
8. VERIFICATION

Cheapest credible designs for a solo operator with a 3-day window [emerging]:

Behavior-trap suite: 10–20 fixed prompts that should trigger specific rules (e.g., “should I force-push?”, expect hard refuse; “refactor unrelated?”, expect refuse). Measure pass rate before/after.
Cold-reader test: Fresh session, paste the file, ask “list the 5 highest-priority rules and any contradictions.” Compare to gold.
Real-use log: Track violation rate / “I had to re-instruct” events over 3 days of normal fleet work.
Pass-rate proxy on a small fixed task set (build/test/lint compliance, style adherence).
Tools: Anthropic /doctor, community analyzers (ccmd-style token/bloat scorers), simple A/B with LLM-as-judge on the trap suite.
More rigorous: statistical A/B frameworks exist (heyneo, enterprise prompt-testing blogs) but overkill for 3 days.

No large public A/B of TOP-block restructures was located; the above is the practical minimum that produces a decision signal.

Ranked Recommendations Table
Practice	Evidence strength	Applicability to your caps (135L/20KB index; ~100KB codex)
Hard-cap index at ≤200L/25KB; one-line pointers ≤150 chars	[established] (Anthropic MEMORY.md)	Direct match; keep ≤135/20KB
Progressive disclosure / tree of skills + on-demand files for codex	[established] (Anthropic 2026-07)	Mandatory; 100KB monolith is outside safe density
Critical rules at very top (primacy) + optional restatement near end	[established] (Liu et al. + replications)	High; use for TOP block
Markdown headers + bullets; keep always-loaded <200 lines ideally <60–100	[emerging] (Anthropic docs + HumanLayer + practitioners)	Codex needs aggressive split; index already compliant
TOP 5–15 atomic hard rules that replace body duplicates	[emerging]	TOP-20 borderline; prefer fewer + pointers
Inbox/append zone at end + scheduled re-sort for multi-writer	[emerging]	High value for parallel fleet
Frontmatter timestamps / supersedes over inline date stamps	[speculative] for superiority	Prefer; less noise
Generated human dashboard vs making source human-friendly	[emerging]	Prefer for dense index
Flat short imperative list over elaborate taxonomy	[emerging dissent]	Viable only if total instructions stay very low
External verifiers / behavior traps over pure structure	[emerging]	Always pair structure with traps
Explicit Contradictions
Anthropic 2026-07: “put it all upfront / give many rules” is now a myth; progressive + judgment wins. vs. classic lost-in-middle advice that still privileges edges.
Format (markdown vs XML vs plain) often claimed important; recent multi-instruction studies find count dominates and format effects are small/model-specific.
“Structure helps adherence” (most practitioners) vs. “structure without external mechanism is cargo-cult; deletion often wins” (Anthropic self-report + cargo-cult critiques + some X voices).
TOP duplication vs pure progressive pointers: duplication wastes attention; pure progressive risks missing critical rules if the model fails to load.
Proposed File Templates

Codex (CLAUDE.md-class, target resident set <<100KB via progressive):

Markdown
Copy
# CLAUDE.md — [Project]

## TOP (priority, never-violate; ≤15 one-liners)
- NEVER force-push / destructive git without explicit confirmation
- ALWAYS run the project test command before claiming done
- ... (atomic, concrete, primacy position)

## §0 Identity & Purpose
[1 short paragraph]

## §1 Commands & Stack (non-obvious only)
- Build: `...`
- Test: `...`

## §2 Architecture / Boundaries
[pointers or 5–10 lines; details in skills]

## §3 Conventions (deltas only)
...

## §4 Gotchas / Locked Decisions
...

## §5 On-demand Skills & Rules
- Verification: @.claude/skills/verify.md
- ...

## INBOX (append-only for parallel writers; re-sort weekly)
- [YYYY-MM-DD agent] raw intake line...

Use @import or path-scoped files for the bulk. HTML comments for human-only notes. /doctor + behavior traps after any change.

Index (MEMORY.md-class):

Markdown
Copy
# MEMORY.md — index (≤135 lines / ≤20KB)

## TOP / Warm (recent or high-frequency)
- [Title](topic.md) — ≤150-char hook

## §0 Architecture
- [Decision X](arch-x.md) — ...

## §1–§7 thematic
...

## Archive / DONE
- [Old](archive/old.md) — superseded YYYY-MM-DD

## INBOX (parallel append zone)
- [raw]( ) — intake; re-sort into sections

Claude (or a tidy agent) moves detail to topic files and keeps the index one-line. Frontmatter modified on topic files for recency.

Decision-Oriented Ending

Concrete options:

Aggressive progressive (recommended): Keep TOP ≤10–15 hard rules + thin thematic pointers; move everything else to skills/topic files. Resident set collapses; adherence rises. Risk: model occasionally fails to load a needed skill. Confidence: [emerging–established] on Anthropic data + instruction-count limits. Main assumption: your fleet’s harness supports reliable on-demand load (Claude Code does).
Dense flat TOP-only: Delete structure, keep ≤40–60 one-line hard constraints. Lowest token tax, maximal primacy. Risk: loses thematic navigation and dual-audience value. Better if total instructions already near ceiling.
Status-quo + linter + inbox: Keep current TOP-20 + §§, add append-zone + claudelint + weekly re-sort. Lowest immediate change cost; drift continues. Risk: continued middle-rule decay and multi-writer misplacement.
Full dual files + generated view: Canonical dense index + separate human dashboard. Highest maintenance.

Single recommendation: Option 1 (aggressive progressive disclosure with short TOP + on-demand tree). Confidence [emerging] leaning [established] given Anthropic’s own 80% cut result and the MEMORY.md hard caps. Main assumption: progressive loading is reliable in your multi-machine harness and you will maintain a small behavior-trap suite to verify the restructure over a 3-day window. Pair with an inbox section and a simple linter/registry to contain multi-writer drift.

Sources (every URL retrieved)
https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models (2026-07-24)
https://code.claude.com/docs/en/memory.md (2026-07)
https://code.claude.com/docs/en/best-practices.md (2026-07)
https://www.humanlayer.dev/blog/writing-a-good-claude-md (2025-11)
https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00638/119630 (Liu et al. TACL 2024)
https://arxiv.org/html/2508.07479 (positional biases 2025)
https://docs.cline.bot/best-practices/memory-bank
https://cursor.com/docs/rules.md / help pages (2026)
https://developers.openai.com/codex/guides/agents-md.md (2026)
https://docs.windsurf.com/plugins/cascade/memories.md
Aider conventions GitHub docs
Multiple 2026 practitioner blogs (GetuWork, AgentLint, Blink, understandingdata, headgym, etc.)
X posts (2026-06/07) from @PawelHuryn, @theo, practitioners on bloat/trim/ignore
Related arXiv on multi-constraint adherence and rules polarity (2025–2026)
Community MEMORY.md analyses (zenn, HarrisonSec, DEV.to)

Where evidence was thin it is marked; no fabricated URLs or quotes.

504 sources
Design behavior trap suite
Explore LLM-as-judge metrics
Add X practitioner quotes




Heavy