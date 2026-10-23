# DR26-07-01-ZB-04 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Negative Knowledge for Agents"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4adc44-582c-83ed-a53a-f0e1853c85b0
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-05
date_added: 2026-07-07
valid_as_of: 2026-07-05
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 3
related_concepts: []
revisit_if: ""
---

# Negative Knowledge for Agents

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

DR26-07-01-ZB-04

Conduct a deep research investigation on the following topic.



TOPIC:

Best cheap architecture for giving personal coding agents a durable "negative knowledge" / FAILURE MEMORY — a record of approaches that were tried and FAILED — so agents stop rediscovering the same dead-ends across sessions, using structured (closed-vocabulary) fields rather than fuzzy text similarity.



CONTEXT (what exists / what was found):

- Today failures live nowhere durable; a rejected-PROPOSAL journal exists but not a tried-and-FAILED technical record; dead-ends get rediscovered every session or buried in transcripts.

- Reference: paper "Negative Knowledge as Failure-aware Shared Memory for AutoResearch" (arxiv 2606.21024, repo github.com/hch-wang/Negative_Knowledge) — Depth-1 records (task_id, attempted_route, observation, failure{layer,scope,degree,recommended_action,risk}, rationale, recommended_alternative) with CLOSED vocabularies; Depth-N pattern records; a curator writes records, the agent MUST read+adopt/reject before its next move ("forced grounding"); warning that naive similarity retrieval of "similar past fix" actively misleads → retrieve by structured tags, not embeddings.

- Anton's stack already uses SQLite tables (leads.db, system.db) + markdown cards (concept-*, reglament-*) + "SQL/grep before LLM".



OBJECTIVE:

Find SOTA on failure/anti-pattern memory for agents and the cheapest AK-47 way to add it: schema, when to write, how to retrieve safely (structured tags), how to force the agent to consult it before retrying, and how to avoid it becoming landfill.



RESEARCH REQUIREMENTS:

1. Academic — negative knowledge, failure-aware memory, reflexion/self-refine, case-based reasoning, experience replay for LLM agents.

2. Industry practice — how real agent frameworks (AutoGPT-lineage, LangGraph, CrewAI, Reflexion impls) store failed attempts; anti-pattern memories.

3. Competitive landscape — structured failure DB vs vector memory vs rules files.

4. Failure analysis — where failure-memory backfired (misretrieval, stale "don't do X" that later became valid, over-suppression of exploration).

5. Emerging trends — last 12 months.

6. Implementation recommendations — a minimal SQLite-table + markdown-card + grep-before-retry design that fits "SQL/grep before LLM"; retrieval strictly by structured domain/layer tags; the discipline that makes agents actually write records.



OUTPUT FORMAT:

- Executive summary.

- Concrete minimal schema (table columns + card template) tuned to the CONTEXT.

- Read/write workflow + "forced grounding" mechanism.

- Anti-patterns (esp. similarity-retrieval trap, stale-negative decay).

- Sources.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"prompt":"DR26-07-01-ZB-04\nConduct a deep research investigation on the following topic.\n\nTOPIC:\nBest cheap architecture for giving personal coding agents a durable \"negative knowledge\" / FAILURE MEMORY \u2014 a record of approaches that were tried and FAILED \u2014 so agents stop rediscovering the same dead-ends across sessions, using structured (closed-vocabulary) fields rather than fuzzy text similarity.\n\nCONTEXT:\n- Today failures live nowhere durable; a rejected-PROPOSAL journal exists but not a tried-and-FAILED technical record.\n- Reference: Negative Knowledge as Failure-aware Shared Memory for AutoResearch (arXiv 2606.21024).\n- Stack: SQLite + markdown cards + SQL/grep before LLM.\n\nOBJECTIVE:\nFind SOTA on failure memory for agents and the cheapest implementation.\n\nResearch requirements:\n1. Academic.\n2. Industry practice.\n3. Competitive landscape.\n4. Failure analysis.\n5. Emerging trends (last 12 months).\n6. Concrete implementation.\n\nOutput:\nExecutive summary, schema, workflows, forced grounding, anti-patterns, sources."}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"DR26-07-01-ZB-04 Conduct a deep research investigation on: Best cheap architecture for giving personal coding agents a durable negative knowledge/failure memory using structured closed-vocabulary fields instead of fuzzy similarity. Include academic SOTA, industry practice, competitive landscape, failure analysis, emerging trends (last 12 months), and concrete implementation for SQLite + markdown + SQL/grep before LLM. Output executive summary, schema, workflows, forced grounding, anti-patterns, sources."}}
```
