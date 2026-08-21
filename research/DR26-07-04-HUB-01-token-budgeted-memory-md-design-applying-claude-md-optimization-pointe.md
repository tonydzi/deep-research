---
dr_id: DR26-07-04-HUB-01
title: "Token-budgeted MEMORY.md design: applying CLAUDE.md optimization (pointer-form, dedup, tie"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-01): Token-budgeted MEMORY.md design: applying CLAUDE.md optimization (pointer-form, dedup, tiering, cold-reader test) to the always-loaded memory index

> The report asks whether the CLAUDE.md optimization playbook (dedup, clustering, cold-reader testing) should also govern MEMORY.md, and concludes yes — but refined into an explicit hot/warm/cold tiered architecture rather than just tighter compression.

## Ключевые выводы
- Claude Code loads only the first 200 lines or 25KB of MEMORY.md at session start; everything beyond that is effectively not loaded, and topic files are read on demand rather than at startup — so oversize is a correctness bug, not just an efficiency loss.
- H1 confirmed with an exception list: if a standing rule already has a behavior-preserving trace in CLAUDE.md, repeating it in MEMORY.md wastes always-loaded tokens; keep a MEMORY.md line only if it adds retrieval aliases, routes to a cluster, or captures a highly recurrent trigger not derivable from CLAUDE.md wording.
- H2 confirmed as hub-and-spoke, not summary replacement: grouping related lines into one hub line is a net win only if the hub still links to a sub-index/topic file preserving fine-grained triggers — RAPTOR, A-MEM, and HORMA all show hierarchical retrieval beats flat lists specifically because raw/lower-level detail stays dereferenceable.
- H3 strongly confirmed and refined: the fix is not a larger cap but a smaller live index plus aggressive tiering — CLAUDE.md as canonical procedural core, MEMORY.md as a hot dispatcher of global invariants and domain hubs, warm sub-index files for clusters, cold topic/archive files for rationale and superseded variants.
- Recommended (engineering-inference, not a published constant) operating target: MEMORY.md around 60-100 lines / ~8-12KB, soft redline before 15KB, with the documented 25KB limit treated as an emergency boundary, not a design target.
- Anthropic guidance (shorter CLAUDE.md = better adherence, avoid edge-case laundry lists) plus 'Lost in the Middle' long-context degradation research both argue against blanket duplication; prompt-repetition studies that show gains are from non-reasoning models/general prompts, not procedural coding-agent memory, so they don't license default duplication.
- Safe forgetting should be modeled as accessibility/salience demotion, not deletion: demote hot→warm→cold, archive with provenance (who/what changed, what replaced it, where detail now lives), delete only on supersession with passing regression tests — mirrors FadeMem's decay and Oblivion's separation of consult vs. reinforce.
- Documented real-world failure modes for memory indexes: startup truncation (silent loss below the cutoff), memory drift (index vs. actual state diverging across sessions), stored-but-not-applied rules (memory updated but behavior unchanged), summary distortion (over-compaction losing subtle critical context), and path-based orphaning (repo move stranding memory).
- Competitive/production landscape converges on the same 3-tier pattern under different names: Anthropic's memory tool ('just-in-time context retrieval'), Letta/MemGPT (core memory + archival), Mem0 (layered conversation/session/user/org memory with reranked search), Zep (temporal knowledge graph with timestamped facts), LangMem (hot-path tools + background consolidation), OpenAI ChatGPT memory (top-of-mind vs. background, versioned).

## Рекомендации / решения
- Build a canonical-rule inventory: for every MEMORY.md line, mark whether it's already behaviorally covered in CLAUDE.md, missing there, or purely a retrieval alias/hub cue — archive the first category unless a retrieval test proves the extra line is needed.
- Restructure MEMORY.md's top section to contain only two things: non-negotiable hot rules and hub pointers; ban incident history, rationale, and examples from the startup-loaded file.
- Any domain with 3+ near-duplicate standing lines should collapse into a single hub line plus a warm sub-index file that preserves exact triggers, aliases, anti-patterns, and recovery steps.
- Define explicit promotion/demotion signals: promote to hot on cross-session behavior impact or repeated regression root-causing; demote when usage falls or a stronger canonical rule duplicates it.
- Attach provenance to every archived/demoted rule (who/what changed it, replacement, new location of raw detail).
- Build a retrieval regression suite testing natural-language, alias, exact-keyword, and grep-style recovery for every hot hub and archived rule — fail the change if recall gets materially harder after compaction.
- Extend the existing cold-reader test into a full behavior-trap suite (wrong-directory execution, forgotten rebuild steps, deployment sequencing, superseded rules, stale-incident bait), measuring both compliance and false-trigger rate.
- Set both a hard budget (respect the documented 200-line/25KB startup cutoff) and a softer operating budget well below it; treat exceeding the soft budget as a mandatory clustering/dedup pass, not a cue to compress wording harder.
- Run scheduled drift audits comparing CLAUDE.md, hot hubs, topic files, and archive to catch stale pointers, dead references, and duplicate canonical rules.
- Prefer archive-plus-demotion over hard deletion for standing rules; only hard-delete once there's an explicit replacement, passing tests without it, and a retained archive trail.

## Сущности
- **Люди:** Liu et al. (Lost in the Middle), Maharana et al. (LoCoMo), Hu et al. (MemoryAgentBench), Wu et al. (LongMemEval-V2, LIFBench), Robinette et al. (VerIFY), Sarthi et al. (RAPTOR), Xu et al. (A-MEM), Hsu et al. (HORMA / Organize then Retrieve), Wang et al. (Memex(RL)), Wei et al. (FadeMem), Rana et al. (Oblivion)
- **Компании:** Anthropic, Letta, Mem0, Zep, LangChain (LangMem), OpenAI
- **Продукты/инструменты:** CLAUDE.md, MEMORY.md, Claude Code memory tool, MemGPT/Letta core+archival memory, Mem0, Zep (temporal knowledge graph), LangMem, OpenAI ChatGPT memory ('Dreaming'), RAPTOR, A-MEM, HORMA, Memex(RL), FadeMem, Oblivion, LoCoMo benchmark, MemoryAgentBench, LongMemEval-V2, LIFBench, VerIFY

## Открытые вопросы
- No universal published percentage/threshold exists for how much of always-loaded context standing rules should occupy — the 60-100 line / 8-12KB target is an engineering inference, not a validated constant, and needs empirical confirmation in this specific harness.
- Whether exact repetition ever helps in a long-running coding-agent memory index (vs. general prompt repetition in non-reasoning models) is untested — the strongest repetition-benefit evidence doesn't come from this domain.
- The tradeoff between learned/decay-driven forgetting (MemAct, Oblivion, FadeMem) and explicit rule-based lifecycle management is unresolved; decay-driven approaches are promising but less operationally auditable for a rule-bearing index.
- Several cited failure modes (memory drift, rules stored-but-unapplied, path-based orphaning) come from public Claude Code GitHub issues, not official Anthropic postmortems — severity/frequency in general use is unverified.
- No regression suite or cold-reader harness yet exists that has been validated against MEMORY.md specifically (as opposed to CLAUDE.md) — needs to be built and tested per the action plan.

## Источник
- DR-ID `DR26-07-04-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-04-HUB-01-memory-md-index-optimization-REPORT.md`

## Связано
- [[claude-md-optimization-mechanics]]
- [[memory-index-hygiene]]
- [[write-service-files-tight-no-recompress]]
- [[ak47-simplicity]]
- [[vault-data-architecture]]
- [[test-after-build-skill]]
- [[fix-root-cause-not-symptoms]]
- [[capture-rules-into-bible]]
