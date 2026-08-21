---
dr_id: DR26-07-14-HUB-04
title: "Best practices for maintaining a very large personal knowledge graph grounding an AI digit"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-04): Best practices for maintaining a very large personal knowledge graph grounding an AI digital twin

> How should a 175k-note, 37k-contact personal vault be cleaned and maintained (entity resolution, link hygiene, dedup, sync-conflict handling, graph quality metrics) so it reliably grounds an AI digital twin, without destructive or cosmetic-only fixes?

## Ключевые выводы
- In the 2026 OpenSanctions Pairs benchmark a rule-based matcher scored 91.33% F1 while LLM matchers scored higher, including 98.23% F1 with a locally-deployable DeepSeek-R1-Distill-Qwen-14B — but pairwise matching is nearing a practical ceiling, so effort should shift to blocking, clustering, and uncertainty-aware review, not better matchers.
- Public CRMs split merge behavior by identifier strength: Attio auto-merges by email/domain and deletes the duplicate; HubSpot auto-dedupes by email/domain but states merged records can never be unmerged; Folk only auto-merges on exact combos (name+email, name+phone, name+social URL); Clay's 'auto-dedupe' is just key-based uniqueness — implying hard auto-merge is safe only for intrinsically strong identifiers.
- PKM practitioner consensus (Matuschak, Milo, Zettelkasten/Ahrens) is NOT zero-orphans everywhere — it's tiered: curated evergreen notes (insights/decisions/concept hubs) should be dense and reachable; archive/fleeting/raw-import material can stay orphaned by design as long as it retains metadata and a path back to canonical entities.
- This vault's measured pathology — ~50% of insight notes with zero incoming links — is a genuine problem because insights are exactly the note class mature PKM methods expect to be linked and revisited; large orphan counts among 100k raw chat imports are a classification issue, not a methodological violation.
- Across memory-system tooling (Mem0, Graphiti/Zep, Letta, Basic Memory, Khoj) the dominant architecture is asynchronous, incremental, provenance-preserving: safe extraction/indexing runs automatically, consolidation runs on a slower cadence, and destructive/structural mutations require backups, scopes, or human review; Letta stores memory in a git-backed filesystem with version history for exactly this reason.
- GraphRAG's benefit is conditional, not universal: it tends to win on multi-hop/reasoning/synthesis tasks while plain RAG stays better for single-hop, detail-oriented, or fast-changing facts; one benchmark found only ~65.8%/65.5% of answer entities present in constructed KGs, showing that entity/edge coverage matters more than raw graph density.
- Syncthing is a replicated file system, not a collaboration protocol: concurrent edits to the same file produce renamed sync-conflict copies; it offers send-only/receive-only modes and per-device versioning but does not merge content — CRDT/local-first systems (Ink & Switch) solve fine-grained concurrent merging, which plain file sync does not.
- A lease alone does not guarantee mutual exclusion for concurrent writers (etcd docs, Kleppmann) — correctness requires conditional writes or fencing tokens, which is the standard fix if multiple agents must write the same shard.
- Local CPU-only dedup at 175k-note scale is comfortably handled by standard tools: exact hashing, SimHash (Google's 8-billion-page study used 64-bit SimHash with Hamming distance k=3), MinHash/LSH (datasketch: sub-millisecond query time, num_perm=128 default), and Faiss embedding ANN (HNSW) for semantic candidate generation followed by exact verification.
- The vault's current measured state includes 900 sync-conflict copies, 1,200 Telegram-handle collisions, 1,900 suffixed person duplicates, 1,000 identical-body import stubs, and 70k broken wiki-links — the report treats the sync-conflict volume as evidence the system is generating corruption faster than it's being cleaned.

## Рекомендации / решения
- Stop the bleeding first: make canonical directories (people/, concepts/, decisions/) single-writer — one node send-only, others receive-only — enable Syncthing file versioning on receivers, and quarantine *.sync-conflict-* files into an explicit review queue.
- Never destructively merge/delete fuzzy matches; use a reversible canonical-note model instead — one canonical person note, source records kept as aliases, with superseded_by/same_as/merged_from provenance fields — and reserve hard deletion for exact-hash junk only.
- Run a 3-stage entity-resolution pipeline: (A) deterministic blocking + auto-resolve on exact normalized Telegram handle/ID, email, E.164 phone, or social URL; (B) cheap fuzzy blocking passes (surname+initial, name+domain, name+city) for the unresolved remainder; (C) a structured scorer plus LLM judge (adversarial two-role review a la OpenSanctions) only for the ambiguous band.
- Prioritize the 1,200 Telegram-handle collisions and 1,900 suffixed person duplicates as the highest-ROI ER pass, and collapse the 1,000 identical-body import stubs immediately via exact hash dedupe.
- Repair the 70k broken wiki-links via a deterministic rename/supersede map before attempting any creative LLM-assisted relinking.
- Concentrate relinking effort on the curated layer only — the ~2k insight notes and 270 concept hubs — targeting <15% zero-inlink insights and >80-90% reachability from a hub within two hops; do not spend the same effort sculpting links across the 100k archived chat imports.
- Adopt a tiered janitor cadence: on-ingest (normalize + exact dedupe + incremental index), nightly (exact-ID ER + broken-link repair from rename maps + conflict triage), weekly (fuzzy candidate queues + relinking suggestions + dead-stub cleanup), monthly (canonicalization review + threshold retuning + rollback of bad merges).
- Track end-to-end 'digital twin' answer quality (answer correctness, faithfulness/groundedness, citation accuracy, context precision/recall, context-entity recall) via a fixed eval set before/after every cleanup pass — use graph-structure metrics only as supporting diagnostics, not the primary success signal.
- For dedup, use lexical methods (SimHash Hamming <=3 auto, 4-6 review; MinHash/Jaccard >=0.90 auto-cluster, 0.80-0.90 review) for the auto-mutation tier, keep embedding similarity confined to recall expansion within an already-blocked candidate set, and always exact-verify before any destructive mutation.
- If true concurrent multi-writer access to a shard becomes unavoidable, add a lightweight coordinator (etcd or ZooKeeper) issuing a lease plus fencing token per note/shard rather than trusting Syncthing's 'first writer wins' semantics.

## Сущности
- **Люди:** Andy Matuschak, Nick Milo, Sönke Ahrens, Martin Kleppmann
- **Компании:** OpenSanctions, Attio, HubSpot, Folk, Clay, Mem0, Zep, Letta, Ink & Switch, Microsoft, Google
- **Продукты/инструменты:** Splink, dedupe, Graphiti, Basic Memory, Khoj, Obsidian Copilot, Syncthing, Faiss, HNSW, datasketch, DeepSeek-R1-Distill-Qwen-14B, etcd, ZooKeeper, KG²RAG, Peritext

## Открытые вопросы
- No canonical numeric thresholds exist in PKM literature for link density, MOC coverage, or orphan ratio — the report's thresholds (e.g. <15% zero-inlink insights, >90% hub reachability) are engineering defaults for this vault, not established doctrine.
- No direct published benchmark links '% broken Obsidian wiki-links' to measurable RAG answer-quality gain — this is an informed systems inference from GraphRAG coverage/completeness results, not a directly published finding.
- Internal matching heuristics of Attio, Folk, Clay, and several OSS memory tools are undocumented publicly — conclusions rely on observed external behavior, not verified internal algorithms.
- Whether LLM-judge adjudication (validated on OpenSanctions' sanctions-list domain) generalizes with the same accuracy to a personal contacts/CRM vault is untested — rated 'emerging' confidence.
- The exact vault-level KPI bundle and the exact human-gated review workflow for fuzzy merges/deletions/hub rewrites are proposed as reasonable but not yet validated in practice for this specific vault.

## Источник
- DR-ID `DR26-07-14-HUB-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- entity-resolution
- vault-data-architecture
- second-brain-northstar
- GraphRAG
- sync-via-telegram-03-mandatory
- dedup
- one-system-propagate
- always-on-memory-pilot
