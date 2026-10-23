# DR26-07-04-ZB-02 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Graph RAG on SQLite"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a4853db-70cc-83ed-8a65-1b384a440986
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-04
date_added: 2026-07-07
valid_as_of: 2026-07-04
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 26
related_concepts: []
revisit_if: ""
---

# Graph RAG on SQLite

> ChatGPT · 2026-07-04 · `gpt-5-5-pro` · 26 сообщений

## Диалог

**Anton:**

DR26-07-04-ZB-02



Conduct a deep research investigation on the following topic.



CONTEXT (what we already know — don't re-derive, dig DEEPER):

- We run a working pilot: Graph RAG over a personal ~100k-note Obsidian vault where the knowledge graph is NOT extracted by an LLM but already exists as hand-curated wikilinks; retrieval = e5-base dense top-60 → 1-hop wikilink expansion (bounded 15 hits / 40 neighbours) → cross-encoder rerank top-12; SQLite is the only persistence (per-turn agent ledger + retrieval A/B telemetry). Public code: https://github.com/Palo-Alto-AI-Research-Lab/sqlite-graph-memory

- Our own A/B telemetry shows graph expansion helps THEME queries but hurts ENTITY/name queries (we gate it off for name-shaped queries).

- Our hypothesis: the academic/industry niche "Graph RAG on SQLite for agents, with hand-curated edges instead of entity extraction" is nearly EMPTY — most Graph RAG work assumes Neo4j/graph-DB + LLM entity extraction (GraphRAG by Microsoft, LightRAG, nano-graphrag etc.). We plan an article claiming the lightweight lane.



TOPIC:

Graph RAG on SQLite / lightweight Graph RAG for single-user LLM agents over markdown knowledge bases (Obsidian/Logseq wikilink graphs) — state of the art, competitors, gap validation, and the strongest angles for a technical article claiming this niche.



OBJECTIVE:

Validate (or refute) that the niche is empty; find the best current practices, state-of-the-art approaches, failures, trade-offs, emerging trends, and implementation frameworks; identify what would make our article the definitive reference.



RESEARCH REQUIREMENTS:



1. Academic research

- papers, meta analyses, benchmarks on graph-augmented retrieval WITHOUT entity extraction (link-structure / citation-graph / hyperlink expansion)

- evaluations of 1-hop neighbourhood expansion + reranker pipelines



2. Industry practice

- startups, leading companies, open source projects doing Graph RAG on embedded/file databases (SQLite, DuckDB) or over PKM wikilink graphs

- how Microsoft GraphRAG / LightRAG / nano-graphrag / txtai / khoj / rememberizer handle graphs, and their infra cost



3. Competitive landscape

- direct competitors (anyone publishing "Graph RAG on SQLite" or "wikilink Graph RAG"), indirect (vector-only RAG over Obsidian), substitutes (agent memory products: mem0, Zep, Letta/MemGPT, LangMem)



4. Failure analysis

- failed/abandoned Graph RAG projects; reported cases where graph expansion HURT retrieval quality (esp. entity queries); over-engineering complaints



5. Emerging trends (last 12 months)

- agent memory ledgers / episodic memory on SQLite; "measure your RAG" A/B telemetry practices; local-first / privacy-first RAG



6. Implementation recommendations

- actionable framework: what benchmarks/metrics the article must show to be credible; where to publish for reach (venues that indexed similar pieces)



OUTPUT FORMAT:



A. Executive Summary

B. Key Findings

C. Contradictions & Debates

D. Best Practices

E. Failure Modes

F. Strategic Recommendations (incl. verdict: is the niche empty? strongest article angles)

G. Action Plan

H. Sources

---

**ChatGPT:** · `gpt-5-5-pro`

I’ll treat the “empty niche” claim as falsifiable: map papers, repos, products, and failure reports, focusing on whether anyone already owns SQLite plus hand-curated wikilink Graph RAG.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"Graph RAG\" SQLite Obsidian wikilink markdown RAG"},{"q":"\"wikilink\" \"RAG\" Obsidian graph retrieval"},{"q":"\"GraphRAG\" \"SQLite\" \"RAG\""},{"q":"\"sqlite-graph-memory\" Palo Alto AI Research Lab"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"survey GraphRAG retrieval augmented generation graph without entity extraction hyperlinks citation graph link structure 2025 paper"},{"q":"RAG hyperlink graph expansion reranker 1-hop neighborhood retrieval paper"},{"q":"citation graph augmented retrieval reranking 1-hop neighbors dense retrieval paper"},{"q":"link structure retrieval augmented generation knowledge graph without entity extraction"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"graph expansion\" hurts retrieval quality RAG entity queries"},{"q":"\"knowledge graph\" expansion \"hurts\" retrieval RAG"},{"q":"\"GraphRAG\" \"worse\" \"retrieval\" \"entity\" queries"},{"q":"\"graph-based\" \"reranking\" RAG \"noise\" \"irrelevant\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"Graph-Based RAG for Manuscript Collections\" \"graph expansion helps catalog and complex queries\""},{"q":"\"graph expansion helps catalog\" \"hurts entity\" \"semantic\" RAG"},{"q":"\"Graph-Based RAG for Manuscript Collections\" LangGraph ReAct agent hybrid RRF graph expansion"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Microsoft GraphRAG local search global search entity extraction graph database cost docs 2026"},{"q":"LightRAG simple fast retrieval augmented generation knowledge graph paper github storage requirements"},{"q":"nano-graphrag github GraphRAG lightweight implementation"},{"q":"txtai graph rag knowledge graph SQLite"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Khoj Obsidian RAG graph search markdown notes architecture SQLite vector database"},{"q":"Khoj AI personal knowledge base Obsidian local embeddings search docs"},{"q":"Rememberizer AI personal knowledge base Obsidian RAG graph"},{"q":"mem0 Zep Letta LangMem agent memory SQLite local first 2025 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn779678search3"},{"ref_id":"turn779678search6"},{"ref_id":"turn779678search7"},{"ref_id":"turn243239search4"},{"ref_id":"turn243239search1"},{"ref_id":"turn398666search1"},{"ref_id":"turn398666search25"},{"ref_id":"turn779678search18"},{"ref_id":"turn779678search19"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"https://github.com/Palo-Alto-AI-Research-Lab/sqlite-graph-memory"},{"ref_id":"https://github.com/Palo-Alto-AI-Research-Lab/sqlite-graph-memory/blob/main/README.md"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"GraphRAG-Bench benchmark graph retrieval augmented generation 2025"},{"q":"RAGBench benchmark retrieval augmented generation evaluation metrics 2024 2025"},{"q":"HippoRAG neurobiologically inspired long-term memory LLMs graph retrieval 2024"},{"q":"G-RAG Don't Forget to Connect graph-based reranking RAG 2024"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn692925search0"},{"ref_id":"turn692925search1"},{"ref_id":"turn692925search2"},{"ref_id":"turn692925search3"},{"ref_id":"turn336145search0"},{"ref_id":"turn732324search18"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn248482view0","lineno":20},{"ref_id":"turn248482view2","lineno":20},{"ref_id":"turn248482view3","lineno":20}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"Document Graph RAG\" intrinsic structure without entity extraction markdown links"},{"q":"\"document graph\" \"GraphRAG\" \"intrinsic structure\" RAG 2025"},{"q":"\"Graph of Records\" RAG long-context summarization graph records 2024"},{"q":"\"GraphRAG\" \"citation graph\" retrieval augmented generation"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn353547search0"},{"ref_id":"turn353547search12"},{"ref_id":"turn353547search1"},{"ref_id":"turn353547search5"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"mem0 memory layer agents graph memory open source docs 2026"},{"q":"Zep temporal knowledge graph memory layer agents docs 2026"},{"q":"Letta MemGPT agent memory architecture docs 2026"},{"q":"LangMem long-term memory LangGraph agents docs"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn161343search6"},{"ref_id":"turn161343search26"},{"ref_id":"turn161343search1"},{"ref_id":"turn161343search8"},{"ref_id":"turn161343search9"},{"ref_id":"turn161343search17"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn621016view1","lineno":20}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Kuzu embedded graph database active 2026 GitHub"}, {"q":"GraphQLite embedded graph database SQLite GraphRAG GitHub"}, {"q":"\"sqlite-graphrag\" GitHub Rust local graph rag"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"per-turn ledger\" AI agent memory SQLite"},{"q":"\"agent memory\" \"SQLite\" \"ledger\" \"Claude Code\""},{"q":"\"RAG telemetry\" A/B testing retrieval evaluation production"},{"q":"\"measure your RAG\" telemetry A/B testing retrieval evaluation"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"RAGAS evaluation metrics context precision context recall faithfulness answer relevance docs"},{"q":"Arize Phoenix RAG evaluation retrieval tracing documentation context precision 2026"},{"q":"LangSmith RAG evaluation retrieval metrics trace docs 2026"},{"q":"TruLens RAG triad context relevance groundedness answer relevance"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:github.com Obsidian GraphRAG wikilinks SQLite RAG"},{"q":"site:github.com \"wikilinks\" \"GraphRAG\" \"Obsidian\""},{"q":"site:github.com \"wikilinks\" \"RAG\" \"SQLite\""},{"q":"site:github.com \"Obsidian\" \"sqlite-vec\" \"RAG\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

A. executive summary

verdict: the niche is not empty if phrased broadly as “graph rag over markdown/wikilink notes” or “graph rag on sqlite.” there are already small but real projects in that lane. the niche is still under-claimed if phrased narrowly as:

sqlite-native, single-user agent memory over an existing human-curated obsidian/logseq graph, where wikilinks are treated as first-class retrieval edges, graph expansion is bounded candidate generation, and routing is validated by a/b telemetry showing when graph helps and when it hurts.

that narrower claim is the one to take. “we invented graph rag” is cope. “we measured the boring local-first wikilink graph rag lane and found the routing rule” is defensible.

your own repo already states the clean core: working pilot, not framework; sqlite only; obsidian wikilinks as graph; ~100k-note vault; dense top-60 → 1-hop neighbours → rerank top-12; and telemetry showing graph expansion should be gated for entity-shaped queries. that is exactly the article’s moat, assuming the numbers are made public and reproducible. citeturn750069view0

the academic literature is mostly not about this exact niche. it mostly assumes knowledge-graph construction, entity linking, community summaries, and graph traversal over extracted entities. microsoft graphrag explicitly extracts a knowledge graph from raw text and builds community summaries, while lightrag still relies on kg/vector dual retrieval and entity/relationship extraction patterns. citeturn622281view3turn622281view4

the closest academic support for your lane comes from document graphs, citation graphs, hyperlink graphs, and graph-based reranking. those validate the idea that pre-existing structure can improve retrieval, especially for multi-hop, thematic, and relationship queries. but the same literature also warns about information overload from expanding neighbourhoods, especially when k-hop traversal drags in irrelevant content. apparently graphs are not magic. tragic. citeturn248482view4turn696867view0turn696867view1turn225770view2

the strongest negative result aligns with your internal telemetry: graph expansion helps catalog/complex/relational queries but can reduce effectiveness for semantic/entity queries by diluting the top list. a manuscript-collection graphrag benchmark reports exactly that shape: graph expansion improved some catalog/complex queries but hurt entity/semantic ones, and the graph+hybrid variants underperformed hybrid+rerank overall in that setup. citeturn128499search1turn128499search3

so the definitive article should not be “sqlite graphrag is better.” it should be “when sqlite wikilink graphrag helps, when it fails, and how to route queries before it ruins retrieval.” less glamorous, more useful. terrible for hype, good for builders.

B. key findings

1. the broad field is crowded, but your exact shape is still sparse

mainstream graphrag means entity extraction, relation extraction, graph construction, community detection, and graph-db-ish infra. microsoft graphrag’s default mental model is extracting a kg from text, then using graph summaries and global/local/drift search modes. lightgrag positions itself as lighter than microsoft’s graphrag, but still operates in the knowledge-graph plus vector embedding paradigm. nano-graphrag is mostly a smaller implementation of the same entity/kg idea. citeturn622281view3turn622281view4turn243239search2turn243239search6

the lightweight sqlite/local-first world is now waking up. there are projects like sqlite-memory, which treats markdown as source of truth and uses sqlite, fts5, embeddings, and local/private search; GraphQLite, which adds graph database capabilities and cypher-like graph querying to sqlite; and sqlite-graphrag, a 2026 crate that stores memories, entities, and relationships in a small sqlite-backed graph memory system. citeturn622281view2turn622281view8turn819760search7turn819760search5

but most of those either do vector-only markdown search, use llm/entity extraction, or introduce embedded graph-db abstractions. your sharper lane is: no llm graph extraction, no neo4j, no cypher-first graph layer, just a human-authored wikilink graph used as a retrieval prior with telemetry.

2. direct competitors exist, so don’t say “empty” too casually

the closest direct competitor i found is Vasallo94/ObsidianRAG: a native obsidian plugin that advertises local/private operation, hybrid vector + bm25 search, cross-encoder reranking, and “graphrag” by following `wikilinks` to expand context from linked notes. that is very close on paper. citeturn375581search0

the fsck.com “knowledge graph tools” project is also very close conceptually. it turns an obsidian vault into a queryable graph, parses every markdown file as a node and every wikilink as an edge, indexes into sqlite plus sqlite-vec and fts5, and deliberately keeps llms out of the tool layer. its own “gotchas” are also useful: hub nodes overwhelm output, pagerank can fail on disconnected graphs, yaml is messy, and content has to be truncated. in other words, someone else already discovered that pkms are not clean enterprise ontologies. shocking development. citeturn622281view6

other adjacent/direct projects include obsidian-notes-rag, obsidian-graph-mcp, Mnemo, MindGraph-style markdown graph retrieval, and MotherDuck’s duckdb-based obsidian rag. these are not all identical, but they prove the local-first pkm retrieval lane is alive. citeturn375581search2turn375581search12turn375581search18turn398666search0

therefore the article should avoid “nobody is doing this.” safer and stronger: “almost nobody has published a measured, sqlite-only, hand-curated-wikilink retrieval evaluation with negative results and routing rules.”

3. academic evidence supports the structure-first idea, but not the exact product pattern

the graphrag survey literature recognizes document graphs, hyperlinks, citations, entities, and sentences as graph structures that can support retrieval. it also says graph traversal methods include entity linking, relational matching, bfs/dfs, graph kernels, and graph embeddings. importantly, it warns that seed expansion can cause information overload because neighbourhoods grow and include irrelevant content. citeturn248482view4

citation-graph rag is a good academic analogue. cg-rag argues that dense/sparse retrieval alone misses inter-document relationships, while citation and hierarchy links capture structure useful for research question answering. this maps cleanly to obsidian/logseq: wikilinks are basically personal citations with higher intent density. citeturn696867view1

document graphrag is another analogue: it uses the intrinsic structure of documents to build a graph for domain-specific qa. not the same as obsidian, but philosophically aligned: use existing structure before asking an llm to hallucinate a new ontology for $0.002 per “relationship.” citeturn696867view0

graph-based reranking is also relevant. “don’t forget to connect” puts a graph-based reranker between the retriever and reader, combining document connections with semantic graph structure, and reports gains versus strong reranking baselines. your pipeline is simpler, but the architectural pattern is similar: retrieval first, graph signal second, rerank before answer generation. citeturn225770view2

4. evaluation is the missing layer in most projects

GraphRAG-Bench exists because standard qa benchmarks are too weak for testing graph-based reasoning improvements; it evaluates graph construction, retrieval, generation, and logical coherence over domain-specific multi-hop questions. RAGBench similarly exists because rag evaluation lacks unified criteria and broad annotated data across realistic tasks. citeturn225770view0turn248482view1

that is a gift for your article. the “definitive” piece is not another architecture diagram. it is a query-stratified benchmark showing where graph expansion wins, loses, and should be disabled.

use ragas-style context precision/recall, plus mrr, ndcg@12, recall@60/100, answer faithfulness, and latency. context precision specifically checks whether relevant chunks are ranked above irrelevant chunks; context recall checks whether the retrieved context covers the needed evidence. those map directly to your graph-expansion problem. citeturn978256search0turn978256search8turn978256search16

5. agent memory substitutes are moving fast, but they are not the same niche

mem0, zep/graphiti, letta/memgpt, and langmem are substitutes, not direct competitors. they sell or implement memory layers for agents. zep’s paper positions graphiti as a temporal knowledge-graph memory layer and reports higher answer accuracy and lower latency versus a baseline. letta is explicitly memory-first, with sleep-time/dream memory updates and git-backed markdown memory files. langmem frames long-term memory as semantic, episodic, and procedural, integrated with langgraph. citeturn621016view0turn282625view0turn621016view2turn621016view3turn621016view4turn621016view5

these systems are relevant because they define the “agent memory” buyer category. but they do not usually start from a 100k-note hand-curated obsidian graph. they either build memories from conversations, extract entities, or manage new memory stores. your article can position sqlite wikilink graphrag as the “local-first, existing-memory substrate” for single-user agents.

C. contradictions & debates

graph rag vs vector rag

the real debate is not “graph or vector.” it is “what query type deserves graph expansion.” vanilla dense retrieval is usually better for exact entity/name/tool lookups because a graph hop can pull in neighbours that are related but not relevant. graph expansion becomes useful when the user wants themes, synthesis, context, chronology, project history, or “what connects x and y.”

this matches both your telemetry and external evidence. a recent manuscript graphrag evaluation reported that graph expansion improved catalog/complex queries but hurt semantic/entity queries by diluting the top list. citeturn128499search1turn128499search3

entity extraction vs hand-curated links

mainstream graphrag assumes text is raw ore and an llm must mine entities/relations from it. for personal markdown, that is often backwards. obsidian/logseq users have already made semantic decisions by linking notes. a human-created `wikilink` often has more signal than an llm-extracted “alice — works_at — company” edge from a vague paragraph. your repo already frames this correctly: wikilinks are hand-curated, edge quality is higher than model extraction, and sqlite is enough for this scale. citeturn750069view0

but the counterpoint matters: hand-curated links are biased, incomplete, stale, inconsistent, and full of hub pages like “ai”, “ideas”, “people”, “todo”, “moc”, “daily notes”. so the article should not romanticize them. human edges are better priors, not ground truth.

sqlite vs graph database

enterprise graphrag likes neo4j/graph-db infra because it expects large heterogeneous graphs, graph algorithms, query languages, team workflows, and explainability dashboards. for a single-user vault, that can be architectural theater. sqlite plus fts5 plus sqlite-vec plus a simple edge table is enough for most retrieval-time graph expansion.

the counterpoint: once you need typed edges, multi-hop path queries, graph analytics, multi-user permissions, high update volume, or graph-native query planning, embedded graph databases like GraphQLite/Kuzu/GraphLite become attractive. GraphQLite explicitly pitches graph database capabilities inside a single sqlite file. citeturn819760search7

long context vs retrieval

long-context models reduce the need for retrieval gymnastics in small corpora. but a 100k-note vault is not “just put it in context” territory unless the goal is to donate money to gpu vendors as performance art. retrieval still matters because the hard problem is selecting the right evidence, not merely stuffing more text.

that said, long-context models make reranking and context compression even more important: the retrieval system should produce a compact, well-proven evidence bundle, not 40 graph neighbours because “connected knowledge” sounds nice in a slide deck.

D. best practices

use graph expansion only as candidate generation

the graph should add candidates, not override relevance. your repo’s design choice is right: dense retrieval gets the seeds, graph adds bounded 1-hop neighbours, and the cross-encoder decides final top-k. that keeps the graph from becoming a relevance oracle. it is not one. citeturn750069view0

route by query type

implement a query router with at least four classes:

entity/name/tool lookup: graph off by default, or only alias/redirect expansion.

theme/synthesis query: graph on.

relationship/multi-hop query: graph on with path provenance.

temporal/latest/current query: graph not enough; use recency filters or external freshness.

this routing rule is likely the article’s strongest practical contribution.

bound graph expansion hard

keep 1-hop as default. 2-hop is where retrieval quality goes to die unless there is a very specific path query. use caps like your current 15 seeds / 40 neighbours, but publish ablations: seed count 5/15/30 and neighbour cap 10/20/40/80. the graphrag survey warning about exponential neighbourhood noise is not theoretical. it is exactly what happens in messy pkm graphs. citeturn248482view4

penalize hubs

degree-normalize graph candidates. exclude or downweight daily notes, index pages, maps of content, tags, templates, and generic hub notes. fsck’s practical notes say hub nodes overwhelm output until brief mode and truncation are added. same disease, different vault. citeturn622281view6

keep provenance for every candidate

for each chunk/note, store whether it came from dense, bm25, graph expansion, alias expansion, or manual pin. for graph candidates, store the seed note and edge path. without provenance, you cannot debug why the agent dragged in some random 2021 note about “strategy” during a query about a person named strategy. yes, this happens.

use hybrid search as the boring baseline

compare dense-only against bm25+vector rrf, not just vector-only. khoj’s public docs describe a standard pattern: bi-encoder embeddings for initial retrieval and a slower cross-encoder for reranking. many pkm systems are already at that level. beating a weak vector-only baseline is not enough. citeturn622281view5

store telemetry in sqlite

your sqlite ledger/a-b setup is not a side detail. it is the moat. log query text, query type, retrieval mode, candidate provenance, final chunks, answer id, latency, token count, user feedback, and whether graph-added candidates made top-12. your repo already logs retrieval a/b and per-turn ledgers; the article should expose the schema and core sql reports. citeturn750069view0

evaluate with human labels first, llm judges second

llm-as-judge is fine for scale, but the article needs a hand-labeled set. build 150-300 real queries from the vault, stratified by query type. label top-12 relevance and answer support. report confidence intervals. otherwise it becomes another “we ran vibes@k and got 17% more synergy” post.

E. failure modes

entity query dilution

graph expansion can bury the exact note/person/tool under adjacent but irrelevant neighbours. this is the most important failure mode because it is counterintuitive to graph enthusiasts. if the user asks “what did nikita say about x,” expanding into every note linked to nikita may be worse than just retrieving the exact nikita note.

theme query overreach

theme queries benefit from expansion until the graph starts pulling generic hub notes. “ai agents” links to everything, therefore it explains nothing. hub penalty is not optional.

stale edges

wikilinks preserve author intent, but old intent. a link made in 2021 may encode a worldview that is wrong in 2026. graph retrieval can resurrect stale context with high confidence. use modified time, recency weighting, or explicit “historical” labels.

llm-extracted graph drift

for competitors using extraction: entity extraction creates brittle edges, alias duplication, false relations, and weird ontologies. this is why your “no entity extraction” angle is strong. but do not overstate it: human wikilinks have their own mess.

yaml/frontmatter chaos

real obsidian vaults contain broken yaml, aliases, embeds, renamed notes, unresolved links, duplicate titles, and plugin-specific syntax. fsck’s writeup explicitly calls out messy yaml and sqlite-vec/pagerank gotchas. this is useful because it shows the implementation pain is real. citeturn622281view6

graph algorithms that look smart and do little

pagerank, community detection, centrality, and paths can be useful diagnostics. as ranking features, they can also promote popular notes rather than relevant notes. do not lead with graph algorithms. lead with retrieval quality.

evaluation leakage

a personal vault benchmark can overfit to the author’s memory and writing style. publish the harness, synthetic/public mini-vault, and anonymized query taxonomy even if the real 100k vault cannot be released.

privacy leakage

local-first is a feature, but agent logs can leak sensitive retrieved text. sqlite ledger should have redaction modes, retention policy, and local encryption option. otherwise “private rag” becomes “private until someone opens `memory.db`.”

F. strategic recommendations

verdict: is the niche empty?

broad niche: no. there are now direct and adjacent projects doing obsidian graph rag, sqlite-backed markdown memory, sqlite graph extensions, and local-first pkm retrieval. the “empty niche” claim would be too easy to dunk on. and someone will dunk. probably with a github search screenshot. deserved.

narrow niche: yes, mostly. i did not find a definitive technical reference that combines all of these:

100k-note personal markdown vault.

existing hand-curated wikilink graph, not llm entity extraction.

sqlite-only persistence and telemetry.

dense retrieval → bounded 1-hop graph expansion → cross-encoder rerank.

query-type gating where graph is disabled for entity/name queries.

published a/b retrieval telemetry showing graph helps theme queries and hurts entity queries.

that combination is still a clean lane.

strongest article angles

“your obsidian vault is already a graph. stop paying an llm to rediscover your links.”

this is the cleanest hook. it contrasts human-authored wikilinks with extracted kg theater.

“graph rag is a query router, not a religion.”

this is the most useful thesis. graph helps some queries and harms others. publish the rule.

“sqlite is enough until proven otherwise.”

do not frame this as anti-graph-db ideology. frame it as an empirical claim for single-user agents over markdown. your repo already says the cost is the reranker, not graph infra. citeturn750069view0

“negative results are the moat.”

most graphrag posts only report wins. your entity-query regression is the credible part. it makes the article feel like engineering, not launchpad copy.

“agent memory should be measured as retrieval behavior, not architecture diagrams.”

tie the ledger to agent memory. zep, letta, mem0, and langmem define the market category; your contribution is a local-first, inspectable, sqlite-backed version over pre-existing personal knowledge. citeturn282625view0turn621016view2turn621016view4

moat

the moat is not sqlite. anyone can use sqlite. the moat is:

the 100k-note real vault.

the hand-curated wikilink graph.

the telemetry showing conditional wins/losses.

the routing rule.

the reproducible benchmark harness.

the humility to show where graph rag fails.

sadly, “we use sqlite” alone is not a moat. it is a file format with good taste.

G. action plan

1. freeze the claim

use this exact claim:

“we evaluate a sqlite-native, wikilink-based graph rag pattern for single-user agents over a 100k-note obsidian vault. unlike typical graphrag, we do not extract entities or build a separate graph database. we use existing human-authored wikilinks as retrieval edges, and show that graph expansion improves thematic/multi-hop queries but hurts entity/name lookups unless routed.”

that is hard to attack and easy to understand.

2. build the benchmark

create 150-300 queries:

entity/name lookup.

tool/project lookup.

theme/synthesis.

relationship/multi-hop.

temporal/history.

known-note exact recall.

ambiguous natural-language queries.

label top-12 passages manually. use llm labels only as secondary checks.

3. run retrieval ablations

compare:

dense-only.

bm25-only.

bm25+dense rrf.

dense → graph expansion → rerank.

bm25+dense → graph expansion → rerank.

graph-only, mostly as a cautionary tale.

router: entity gate off / theme graph on.

test neighbour caps, seed counts, 1-hop vs 2-hop, reranker on/off.

4. publish the telemetry schema

show sqlite tables:

queries.

retrieval_runs.

candidates.

candidate_sources.

rerank_scores.

answers.

feedback.

ab_test_assignments.

include 5-10 sql queries that produce the article charts. boring, inspectable, good.

5. publish the failure gallery

include examples where graph expansion hurt:

name query where linked neighbours bury exact match.

hub note explosion.

daily note noise.

alias collision.

stale link.

2-hop expansion disaster.

this is where the article becomes definitive.

6. publish code and mini-vault

release a small synthetic obsidian vault with 200-500 notes and the same benchmark harness. the real vault can stay private. the harness must be public.

7. distribution

best venues:

github repo + technical blog first.

hacker news, lobste.rs, r/localllama, r/obsidianmd, r/rag.

a short arxiv technical report if benchmark labels and ablations are solid.

then pitch to sqlite-ai / langchain / llamaindex / motherduck / obsidian community channels.

do not lead with “ai agent memory protocol.” lead with “measured wikilink graphrag on sqlite.” less vapor, more adoption.

H. sources

core implementation / your repo

your public repo: sqlite-only graph rag pilot over ~100k obsidian notes, wikilink graph, dense retrieval, 1-hop expansion, cross-encoder reranking, entity gate, and a/b telemetry. citeturn750069view0

mainstream graphrag systems

microsoft graphrag: graph extraction from raw text, community hierarchy/summaries, global/local/drift search. citeturn622281view3

lightrag: lighter kg/vector graphrag architecture, still in the knowledge-graph extraction family. citeturn622281view4

nano-graphrag: smaller implementation of the microsoft-style graphrag pipeline, still based on chunking/entity extraction/kg construction. citeturn243239search2turn243239search6

academic / benchmark sources

GraphRAG survey: retrieval with graphs, graph traversal, document/hyperlink/citation graphs, and warnings about noisy neighbourhood expansion. citeturn248482view4

GraphRAG-Bench: benchmark arguing existing qa evaluations are insufficient for graph reasoning. citeturn225770view0

RAGBench: broad annotated benchmark for rag task/evaluation consistency. citeturn248482view1

Document GraphRAG: uses intrinsic document structure as graph for domain qa. citeturn696867view0

CG-RAG: citation-graph retrieval for research question answering. citeturn696867view1

G-RAG graph-based reranking: graph connections used between retrieval and reader. citeturn225770view2

failure / negative evidence

manuscript-collection graphrag benchmark: graph expansion helps catalog/complex queries but hurts semantic/entity queries, matching your telemetry pattern. citeturn128499search1turn128499search3

direct / adjacent competitors

Vasallo94/ObsidianRAG: local obsidian plugin with hybrid search, cross-encoder reranking, and wikilink graphrag expansion. citeturn375581search0

fsck knowledge graph tools: obsidian vault to sqlite/sqlite-vec/fts5 wikilink graph, no llm inside the tool, plus real implementation gotchas. citeturn622281view6

obsidian-notes-rag: local semantic search over obsidian with sqlite-vec. citeturn375581search2

obsidian-graph-mcp: parses wikilinks/frontmatter/tags and exposes graph neighbourhood retrieval flow. citeturn375581search12

Mnemo: personal obsidian knowledge graph with wikilinks/yaml/tags and hybrid graph/vector search. citeturn375581search18

MotherDuck obsidian rag: duckdb-based local-first obsidian rag with connected-note exploration. citeturn398666search0

sqlite / embedded graph infra

sqlite-memory: markdown source of truth, sqlite, fts5, embeddings, local/private memory. citeturn622281view2turn622281view8

GraphQLite: graph database capabilities inside sqlite. citeturn819760search7

sqlite-graphrag: 2026 sqlite-backed graph memory crate for agents. citeturn819760search5

agent memory substitutes

mem0: general memory layer for ai assistants/agents with hybrid search/entity features. citeturn621016view0

zep/graphiti: temporal knowledge graph memory layer with reported benchmark improvements. citeturn282625view0

letta: memory-first stateful agents with memory tools and git-backed memory filesystem. citeturn621016view2turn621016view3

langmem/langgraph memory: semantic/episodic/procedural long-term memory and memory management tooling. citeturn621016view4turn621016view5

evaluation tooling

ragas context precision/recall and related rag metrics. citeturn978256search0turn978256search8turn978256search16

langsmith rag evaluation workflow. citeturn978256search2turn978256search10

trulens rag triad: context relevance, groundedness, answer relevance. citeturn978256search3

explain like a five-year-old

you have a huge notebook castle. every room is a note. the blue strings between rooms are your obsidian wikilinks.

normal rag says: “search for the room with similar words.”

graph rag says: “also check the rooms connected by blue strings.”

that helps when you ask, “what is this whole area about?” because nearby rooms explain the theme.

but it hurts when you ask, “where is bob?” because checking all bob’s nearby rooms may distract from the one room that actually says where bob is.

so the best system is not “always follow the strings.” the best system first asks: “is this a theme question or a name question?” then it decides whether the strings help or just make a mess.

---

**Anton:**

do deep research!! @Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"topic":"Graph RAG on SQLite / lightweight Graph RAG for single-user LLM agents over markdown knowledge bases (Obsidian/Logseq wikilink graphs): state of the art, competitors, gap validation, benchmarks, failure modes, implementation guidance","goal":"Produce a comprehensive research report with academic literature, industry landscape, competitor analysis, failure analysis, emerging trends, implementation recommendations, and publication strategy. Validate or refute whether the niche 'SQLite-native Graph RAG using hand-curated wikilinks instead of LLM entity extraction' is genuinely underexplored.","deliverable":"Long-form research report with executive summary, evidence tables, references, and actionable recommendations."}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"Conduct deep research on Graph RAG on SQLite / lightweight Graph RAG for single-user LLM agents over markdown knowledge bases (Obsidian/Logseq wikilink graphs). Validate whether the niche of SQLite-native Graph RAG using hand-curated wikilinks instead of LLM entity extraction is underexplored. Cover academic literature, benchmarks, industry practice, GraphRAG/LightRAG/nano-graphrag/txtai/khoj/rememberizer, competitors, failure modes, last-12-month trends, implementation recommendations, publication strategy, and produce a comprehensive report with sources."}}
```
