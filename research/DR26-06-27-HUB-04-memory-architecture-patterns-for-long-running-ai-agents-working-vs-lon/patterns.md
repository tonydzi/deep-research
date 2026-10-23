# DR26-06-27-HUB-04 — raw report from patterns

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Memory Architecture Patterns

- **Working vs Long-Term Memory:** Modern AI agents distinguish **working memory** (the active context) from **long-term store**. Working memory holds the immediate conversation, current plan and recent observations; it is session-scoped and aggressively pruned. Long-term memory (episodic/semantic) stores facts, logs and knowledge across sessions. The cognitive taxonomy suggests handling each separately: keep only what’s needed in-context, and persist the rest externally.
- **Context Summarization & Pruning:** A standard pattern is to **summarize** or **compress** old context before hitting limits, and **prune** easily re-fetchable content. For example, systems prompt the model to “condense the conversation so far, preserving decisions and constraints”, and drop old tool outputs or drafts. This keeps the active window focused on essentials.
- **Scratchpad / Notes:** Many designs include a small “scratchpad” or state file that the agent updates each turn (current goal, decisions, next steps). This file is always reloaded even when history is summarized, serving as a bridge between sessions. It’s a lightweight memory that makes the agent’s plan explicit and recoverable.
- **Retrieval-Augmented Generation (RAG):** Instead of loading everything into context, agents use **retrieval**: encode each user query to an embedding, search a vector store or index for relevant past interactions or documents, and inject those into the prompt. For example, one architecture maps queries to embeddings and retrieves the most similar memory entries by cosine similarity. Unlike static RAG over fixed corpora, such agents *dynamically* update and query their memory store on each turn.
- **MemGPT/Letta (Self-Editing Memory):** MemGPT-like systems give the LLM tools to write and recall memories. Letta (the successor) decouples this by using a “sleep” agent that handles memory management offline. The sleep-time agent continuously restructures and cleans up memory in downtime, allowing the live agent to focus on conversation. This two-agent pattern offloads heavy memory updates (e.g. rewriting messy notes into concise entries) and avoids lag in the user loop.
- **Sleep-Time Compute / Consolidation:** Inspired by human sleep, agents can run background “dreaming” cycles to reorganize knowledge. Letta’s **Sleep-Time Compute** lets a stronger model run in the background over idle periods to consolidate information. Similarly, the SAGE framework uses a forgetting-curve mechanism (based on Ebbinghaus) to **consolidate important memories** and prune the rest. More elaborate systems (e.g. SCM) even simulate NREM/REM phases: they replay recent episodes, strengthen related concepts, downscale unimportant links, and “dream” to create new associations. These biologically-inspired cycles greatly improve coherence but are complex.
- **Reflexion & Generative Agents:** Some approaches add reflection loops. For example, Reflexion agents keep an **episodic memory buffer** of past trials and use verbal feedback to refine strategies. While not directly a memory management scheme, Reflexion explicitly mentions equipping agents with *dynamic memory* so they can learn from past performance. More broadly, “generative agents” combine memory, planning and reflection: they store experiences as natural-language memories and use retrieval to simulate future actions.

# Detecting Current Focus

- **Recency and Access Patterns:** A simple signal of focus is *recent activity*. Track a timestamp or counter for each memory item. Items frequently accessed or updated remain “hot”. For example, an LRU (least-recently-used) counter provides a cheap heuristic: if a memory hasn’t been used in X turns or days, it cools down and may be evicted. Conversely, if an older item suddenly resurfaces (e.g. due to search hits), it indicates a pivot.
- **Embedding Similarity (Topic Drift):** Measure semantic drift by embeddings. Compute the similarity between the current user query (or recent context) and each memory cluster or category. A sharp drop in similarity (below a chosen threshold) suggests a new topic. In practice, agents map queries to vector space and compare to stored memory vectors; if no memory scores above a threshold, the agent may treat this as a context switch. (Note: pure embedding drift detection is heuristic; one can calibrate thresholds or use clustering to improve robustness.)
- **User-Explicit Signals:** Where possible, rely on explicit cues. If the user mentions a new project or changes tasks (e.g. “let’s switch to X now”), use that as a strong signal. Design the interface or prompts so the user can tag or label topics. These explicit markers eliminate guesswork and avoid over-reliance on model inferences.
- **Task/Metadata Tracking:** If the agent maintains structured state (e.g. active task ID, objectives), watch for changes. A new task assignment implicitly pivots focus. Embedding metadata (tags, headings, commands) into memory entries can make matching easier.
- **Confidence and Anchors:** Some systems track “attention” weights or entropy of the model’s internal distribution: a sudden drop in confidence on the current plan (low token predictability) can hint at a shift. Similarly, tracking pivot words or “headings” (like chapter titles of tasks) and noting when a heading appears/disappears can help. (There’s no single bulletproof method in literature; it’s often application-specific. Hybrid rules combining recency, user cues, and embedding checks tend to work best.)

# Active–Archive Interaction

- **Promotion Triggers (Archive→Active):** When should archived memories return? Common strategies include: (a) *Relevance search*: run a vector search of the archive using the current context or query; if an archived item has high similarity, “promote” it into active memory. (b) *Keyword/heading matching*: if current task mentions a keyword or heading tied to an old memory, bring it back. (c) *Scheduled re-heating*: periodically scan the archive for items related to the agent’s current mission or goals (e.g. nightly preprocessing). 
- **Demotion Triggers (Active→Archive):** Items in active memory cool off when unused. Use LRU or time-to-live (TTL): if an item hasn’t been referenced for *N* turns or days, move it to archive. Also prune when hitting context limits: drop the least-recent or least-relevant active entries.  
- **Hysteresis and Validation:** To avoid “flapping” (an item ping-ponging between active/archive), require stability. For example, only promote archived memory if it crosses a higher threshold (e.g. similarity > 0.8) *and* remains relevant for multiple turns. One can implement a two-tier system: items first go into a **candidate** pool (or mark as *tentatively active*), and only become fully active after validation (multiple hits or user confirmation). This mirrors safe “promotion pipelines” in memory design: e.g. candidate procedures are held separate from validated rules.  
- **Persistent ‘Fixed’ Memory:** Some facts or plans must never drop. Designate these as **procedural memory** that is always-loaded (like “cards standing on the table”). This could include system prompts, user preferences, or key task rules. These items have no TTL and only change via explicit update. For example, treat global settings as version-controlled rules that are never auto-evicted.
- **Implementation Sketch:** In code/JSON one might express triggers and guards as configuration. For example:

```yaml
memory:
  active_limit: 50               # max active items
  promote_threshold: 0.75        # similarity threshold to archive-promote
  demote_idle_turns: 10         # demote if unused for 10 turns
  demote_ttl_days: 14           # max age of active item in days
  flapping_cooldown: 3          # require 3-turn stability before remotion
  critical_ids: [user_profile, goals]  # always keep these active
```

Or as pseudo-code:

```
on_new_user_input(query):
    embed = embed_model(query)
    candidates = archive.search(embed)  # vector search
    if candidates.top.similarity > promote_threshold:
        if not was_recently_promoted(candidates.top):
            promote_to_active(candidates.top)
    generate_response_with(active_memory)
    update_active_memory(query, response)
    if active_memory.size > active_limit:
        evict = select_for_eviction(active_memory)
        demote_to_archive(evict)
```

# Revision Cadence and Cost

- **Per-Session (Daily) Updates:** After each user session or at end-of-day, do a quick maintenance pass. This can include summarizing the day’s notes, compressing lengthy chains of thought, and pruning any items beyond a TTL. This is cheap (few model calls) and prevents runaway growth.
- **Periodic Deep Maintenance (Weekly/On Pivot):** Less frequently (e.g. weekly or when shifting tasks), run a more intensive “Opus revision”: re-summarize compound memories, check for conflicts, and perform thorough vector-indexing. Letta’s sleep-time agent is a form of continuous revision—if enabled, it can run hourly or nightly on fresh data. Letta notes that higher **sleep frequency** uses more tokens but yields more up-to-date context, so this should be tuned to the application’s tolerance for lag vs cost.  
- **Event-Driven Triggers:** Key events (new project, major user command, detected pivot) can trigger on-demand updates. For example, before switching topics, the agent could “snapshot” current plan and refresh relevant memories.  
- **Compute Trade-offs:** Each revision (embedding many memories, calling the LLM for summaries, etc.) has a cost. A practical approach is incremental: do small, fast steps every turn (e.g. check retrieval hits; maybe auto-summarize parts of the scratchpad) and budget larger operations only on schedule. The goal is a **Pareto improvement**: delegate heavy lifting to idle times so interactive latency stays low.  

# Anti-Patterns and Failure Modes

- **Silent Memory Loss:** If forgetting is too aggressive, the agent may “lose” rules or facts without warning. This can happen if items are pruned without adequate backup or validation. To detect this, monitor retrieval failures or user confusion (e.g. agent saying “I don’t recall that” unexpectedly). Use logging to catch when an expected memory isn’t found.  
- **Context Overflow:** Without pruning, the context window fills and causes slowdowns or hallucinations. As noted, even very large windows degrade attention quality and latency. This wastes compute and can mislead the model with stale info. Use strict limits (e.g. max tokens or items) and automatic culling.  
- **Memory Drift/Staleness:** Stored facts can become outdated (“the world changed”). This leads to contradictions or wrong actions. For example, user preferences or external data may change. To guard against this, attach **freshness metadata** (timestamps, sources) to records and check them against external signals. Hidekazu’s guide emphasizes that “staleness” is a known open problem. Practical fixes include TTLs on episodic logs, periodic verification queries, or invalidation hooks from external events (e.g. if a config repo changes, invalidate relevant memories).  
- **Oscillation (Thrashing):** Without hysteresis, an item might shuttle back and forth between active/archive every few turns. We avoid this with cooldown rules and validation gates. For example, only demote an item if it has been idle *and* we have exceeded capacity, and only promote back after repeated relevance. The “candidate” approach (promote softly then confirm) prevents flip-flopping.  
- **Overgeneralization:** If retrieval is too fuzzy (low threshold), the agent may pull irrelevant archive pieces, confusing context. Conversely, too strict thresholds miss useful info. One must tune similarity thresholds and possibly use multiple retrieval modes (e.g. keyword + embedding) to balance precision and recall.  
- **Dependency Loops:** Be careful that memories don’t depend on each other in cycles, causing infinite loops. Version memories where appropriate and use DAG structures for introspective references.  
- **Detection Gates:** Implement sanity checks: e.g. if a “remembered” fact produces an error, flag it. Use unit tests on memory (like replaying candidate procedures) as in [14], adapted for facts. Hidekazu suggests treating memory with managed pipelines, requiring TTLs and staleness checks by design to catch these issues.

# Generalizing to All Always-Loaded Files

The same principles can apply to any static knowledge base (e.g. system docs, indexes). For example:

- **Treat as Procedural Memory:** Files like `CLAUDE.md` or a “Bible-index” are akin to procedural/system knowledge. They are best kept in version-controlled form and only referenced when needed. Instead of loading them verbatim, the agent should use a retrieval tool (keyword search or semantic search) to fetch relevant parts on demand.  
- **Indexing and Search:** Build a vector or text index of these files. When the agent needs information (e.g. the user asks about Claude commands or biblical reference), query this index. This avoids carrying the full text in context while making it queryable.  
- **Synchronization:** If these always-loaded files update over time, the memory system should detect changes (e.g. via file watchers) and refresh the index. This is similar to staleness management.  
- **Unified Memory Interface:** Wherever possible, present all knowledge (dynamic memories and static docs) through a common retrieval API. For instance, a “memory tool” the agent calls with a query, which searches both long-term memory and static indexes transparently. This keeps the architecture consistent.

# Architectures Comparison and Recommendation

We compare three conceptual architectures, from simplest to most complex:

1. **Baseline (Manual/Static)** – *AK-47 approach*: No automated retrieval or dynamic memory. The agent relies on hard-coded contexts and manual cues.  
   - *Design:* Only the fixed memory index is loaded; the agent uses its in-context window plus the static index file (MEMORY.md) without any smart pruning or recall. Archived memories are only accessed by explicit user action (e.g. “please recall X”).  
   - *Pros:* Very simple; minimal implementation effort. Costs nearly nothing in compute.  
   - *Cons:* **Context bloat** – the active context grows without bounds if not pruned (leading to degraded performance). No systematic way to recall past info, so relevant memories often stay inaccessible unless the user remembers to ask. Rigid: any pivot must be handled manually.  
   - *Risks:* Memory quickly overflows; agent forgets older context; high latency or hallucinations if context is overloaded. Hard to maintain consistency.  
   - *Use-case:* Only if the owner is non-technical and can tolerate the agent “just using brute force”. Infeasible for a truly long-running, complex agent.

2. **Moderate Dynamic Memory** – *Semi-automated retrieval*: Adds vector search and simple policies.  
   - *Design:* Keep the always-loaded index as before, **plus** a vector database of archived memories. On each user turn, compute an embedding of the query and retrieve top-1 or top-k archived entries (cosine similarity). If similarity exceeds a threshold, promote that entry to active memory. Conversely, evict least-recent active items when limits are reached (LRU). Run a daily or per-session cleanup that prunes very old entries (TTL) and optionally compresses long summaries.  
   - *Pros:* Recovers relevant past information automatically; context window stays manageable by LRU/pruning. No need for full LLM inference to guess focus – pure vector search handles most recall cheaply. Low to moderate implementation complexity (many tools/libraries exist).  
   - *Cons:* Still somewhat heuristic. Thresholds must be tuned to avoid false positives. More moving parts to manage (database, embedding model). Latency overhead for embeddings and searches, but this can be small (embedding models are fast).  
   - *Risks:* Mistuned thresholds can cause oscillation (flashy retrieval) or misses. But overall relatively robust and transparent. Failures are limited in scope (some irrelevant memory might slip into context or some useful memory might stay archived).  
   - *Example Evidence:* In [39], such a system (query→embedding→retrieve by cosine) significantly improved coherence in long dialogues, using relevance-based pruning to cap memory size. This approach avoids context explosion while preserving key info.  

3. **Advanced Adaptive Memory** – *Full dynamic hierarchy with consolidation*: Inspired by frameworks like SCM or hierarchical memory.  
   - *Design:* Maintain multiple memory layers (e.g. recent working buffer, intermediate episodic store, long-term semantic graph). Limit the immediate working set to a small fixed capacity (like 7–20 items). Use complex triggers: e.g. enter a “sleep cycle” when entropy/conflict is high to consolidate or prune memories. Use multi-pass retrieval: first use topic/heading to select a sub-memory index (as in H-MEM), then search within it. Possibly run background “dreaming” that combines or generates new memory entries. Include full validation pipelines for promotions (like a human-reviewed step). Use sophisticated importance scores (novelty, recency, valence) to rank what to forget.  
   - *Pros:* The most thorough: highly structured memory yields very coherent, human-like behavior. Dynamically adapts to shifting topics and continually refines knowledge. Could achieve state-of-the-art performance on very long interactions.  
   - *Cons:* Very high complexity. Requires considerable engineering: a graph database or multiple vector stores, custom logic for consolidation, etc. Computational cost is large (embedding searches at multiple layers, offline LLM processing during sleep, continuous re-indexing). Not easily maintained by a non-technical user “with a hammer.”  
   - *Risks:* Many: if any part breaks (e.g. the consolidation scheduler, or the forgetting rate is mis-set), the system may catastrophically degrade. Hard to debug emergent behavior. Overkill for most tasks. Examples like SCM illustrate these ideas but are research prototypes, not drop-in modules.  
   - *Example Evidence:* Academic systems have demonstrated gains (e.g. hierarchical memory led to better long-term dialogue reasoning), but they rely on complex offline passes and multi-stage retrieval. Unless your use-case truly needs it, the implementation burden and fragility outweigh the marginal gains.

**Recommendation:** For a practical, non-technical deployment, we advise the **Moderate Dynamic Memory** architecture (#2). It achieves most of the benefit at reasonable cost. Key points: use a vector search over archived memories on each turn (retrieval-augmented memory) and simple prune-by-recency/TTL policies. This satisfies the “bottom-up” rule: *“prune what can be re-fetched, summarize what cannot”*. In contrast, the naive (AK-47) approach is likely to blow up the context and lose coherence, while the full adaptive system is too heavyweight. The moderate design is straightforward to implement with existing tools (e.g. a vector DB and a daily cleanup script) and it safeguards against the main failure modes (context overflow, stale memory) with clear, deterministic policies.  

**Trade-offs:** The middle-tier design does require some LLM and vector operations each turn, incurring CPU/GPU cost, but far less than constantly querying the LLM over a huge window. It offers automated recovery of past relevant knowledge (unlike manual memory) while avoiding the complexity of full RL-style self-reflection. Crucially, it can be monitored and tuned (thresholds, TTLs) without deep ML expertise. 

**Sources:** We base this on best-practice surveys and research. For example, memory-augmented dialogue systems use query embeddings + retrieval with pruning; design guides emphasize active summarization + just-in-time retrieval over piling up context; and industry leaders warn that forgetting must be built-in (via TTL/decay) to avoid contradictions. These sources inform the above recommendations for a balanced, robust memory architecture.