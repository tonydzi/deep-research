# DR26-07-05-HUB-02 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Multi-agent LLM Consensus Research"
type: ai-conversation
stage: raw
source: chatgpt


authored_by: hybrid
conversation_id: 6a4a81d6-ec50-83eb-8d10-78f10d06153e
model: "gpt-5-5-pro"
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
msg_count: 40
related_concepts: []
revisit_if: ""
---

# Multi-agent LLM Consensus Research

> ChatGPT · 2026-07-05 · `gpt-5-5-pro` · 40 сообщений

## Диалог

**Anton:**

DR26-07-05-HUB-02

# Deep research: Multi-agent LLM consensus in PRODUCTION — who actually runs it daily, best practices, and closest prior art

## Our context (for grounding only, do not research us)
We are a small company running a fleet of 4-6 computers (Windows + Mac), each hosting an autonomous LLM agent (Claude Code). The agents coordinate over a shared message bus (a Telegram group as a human-visible mirror plus a synced folder) and reach decisions through a homegrown consensus engine: propose -> counter -> accept -> commit, with independent verification by at least 2 peers, risk tiers (irreversible or money-related actions escalate to the human owner via a remote approval token), leader tie-break on deadlock, and quarantine for nodes that were offline. It runs in production, daily, doing real work. Early versions had agents "cheating" (self-accepting their own proposals, enforcing stale rule versions), which we fixed by borrowing blockchain governance mechanics (frozen proposal hashes, activation epochs, signed machine identities, anti-entropy repair). We already researched consensus MECHANICS deeply; this research is about the LANDSCAPE: who else does this, and what the ecosystem considers best practice.

## Research questions (in priority order)

1. WHO RUNS IT FOR REAL. Find companies, teams, and indie builders who operate multi-agent LLM systems in which agents NEGOTIATE, VOTE, or reach CONSENSUS with each other, in production, used daily (not just papers, demos, or hackathon projects). Include: startups, enterprises (internal tooling counts), open-source projects with real deployments, and notable solo builders documenting this publicly (X/Twitter, blogs, GitHub, podcasts). For each: what they built, scale (number of agents/machines), how long in production, evidence it is really used daily, and links.

2. WHAT "CONSENSUS" MEANS IN EACH CASE. Classify the cases: (a) true peer-to-peer agreement between autonomous agents on different machines, (b) orchestrator-worker (one boss agent assigns, no real negotiation), (c) debate/judge patterns (agents argue, a judge decides), (d) voting ensembles (majority vote over outputs), (e) blockchain-style protocols applied to AI agents. We claim (a) in a small company is rare; verify or refute that rarity claim with evidence.

3. BEST PRACTICES. From the production cases and the strongest engineering writing (Anthropic, OpenAI, Google/DeepMind, Microsoft AutoGen team, LangGraph, CrewAI and similar): what are the established best practices for agent-to-agent agreement? Cover: proposal formats, verification/review before commit, preventing agents from gaming the process (reward hacking, sycophantic agreement, self-approval), human escalation gates, audit trails, and handling offline/stale agents.

4. FAILURE STORIES. Documented postmortems or candid write-ups of multi-agent coordination going wrong in production (agents colluding, deadlocking, flooding channels, runaway costs, silent divergence). What did they change afterwards?

5. BLOCKCHAIN x AGENTS CROSSOVER. Who else explicitly applied blockchain/consensus-protocol thinking (BFT, governance pipelines, staking, slashing, activation epochs) to LLM agent coordination? Include serious research (e.g. papers on BFT for agent swarms) and production attempts (Fetch.ai, Bittensor, Olas/Autonolas, agent-economy projects). Separate real usage from token-marketing.

6. POSITIONING CHECK (for a public post). If a small company says: "our computers negotiate with each other and reach consensus daily in production, and we borrowed blockchain governance to stop them cheating", what is the closest prior art that could be cited as "X did something similar"? What framing would be defensible and impressive but not overclaiming?

## Deliverable format
- Executive summary (10-15 bullet points).
- Table of production cases: who / what / scale / since when / consensus type (a-e) / evidence link.
- Best-practices checklist (numbered, actionable).
- Failure-stories section with sources.
- Rarity verdict: is peer-to-peer daily production agent consensus in a small company rare? (common / uncommon / rare / near-unique, with justification).
- All claims with links. Mark weak/unverified claims explicitly.
- Recency matters: prioritize 2024-2026 sources.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:anthropic.com/news OR site:anthropic.com/engineering \"Building effective agents\" Anthropic December 2024 agents workflows orchestrator evaluator optimizer"},{"q":"OpenAI agents production best practices human in the loop audit trail multi-agent 2025"},{"q":"Google DeepMind multi-agent systems production LLM agents best practices 2025"},{"q":"Microsoft AutoGen production multi-agent case study agents 2024 2025"}],"response_length":"short"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

I’ll treat this as landscape research, not a mechanics review: separate real production use from demos/papers, classify each case by consensus pattern, and mark weak evidence explicitly.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"multi-agent\" \"consensus\" \"production\" \"LLM\" agents"},{"q":"\"agents\" \"vote\" \"consensus\" \"production\" \"LLM\""},{"q":"\"multi-agent\" \"daily\" \"production\" \"LLM\" \"agents\" \"consensus\""},{"q":"\"LLM agents\" \"negotiate\" \"production\" \"multi-agent\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:blog.langchain.com LangGraph production multi-agent case study companies"},{"q":"site:langchain.com \"LangGraph\" \"production\" \"multi-agent\" \"case study\""},{"q":"\"LangGraph\" \"multi-agent\" \"production\" \"customer\""},{"q":"\"built with LangGraph\" \"multi-agent\" \"production\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:crewai.com customer story multi-agent production AI agents CrewAI 2025"},{"q":"site:crewai.com \"production\" \"multi-agent\" \"CrewAI\" \"agents\""},{"q":"\"CrewAI\" \"production\" \"multi-agent\" \"case study\""},{"q":"\"CrewAI\" \"agents\" \"production\" \"enterprise\" customer"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"AutoGen\" \"production\" \"case study\" \"multi-agent\""},{"q":"site:microsoft.com \"AutoGen\" \"production\" \"multi-agent\" \"customer\""},{"q":"\"Microsoft AutoGen\" \"production\" \"agents\" \"case study\""},{"q":"\"AutoGen\" \"daily\" \"multi-agent\" \"production\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:anthropic.com/engineering \"multi-agent research\" Claude Research production agents 2025"},{"q":"\"Anthropic\" \"multi-agent\" \"production\" \"research\" Claude"},{"q":"\"How we built our multi-agent research system\" Anthropic 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Cognition AI \"Don’t Build Multi-Agents\" June 2025"},{"q":"site:cognition.ai/blog \"multi-agents\" \"Don't Build\""},{"q":"\"Don’t Build Multi-Agents\" \"Cognition\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Cognition \"Devin can now manage Devins\" March 2026 multi-agent production"},{"q":"site:cognition.ai/blog \"Manage Devins\" \"Devin\" \"multi-agent\""},{"q":"\"Devin can now manage Devins\" \"Cognition\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"OpenAI Agents SDK multi-agent handoffs production guide 2025 2026 official"},{"q":"site:openai.github.io/openai-agents-python \"handoffs\" \"multi-agent\" \"Agents SDK\""},{"q":"OpenAI \"Swarm\" experimental not production multi-agent handoff GitHub README"},{"q":"OpenAI practical guide building agents multi-agent framework human escalation guardrails"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Google Agent2Agent A2A protocol multi-agent production 2025 2026 official"},{"q":"site:developers.googleblog.com A2A protocol agent-to-agent 2025 multi-agent"},{"q":"Google Cloud Agent2Agent protocol partners production agents 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:x.com \"multi-agent\" \"production\" \"agents\" \"daily\" \"LLM\""},{"q":"site:twitter.com \"multi-agent\" \"production\" \"agents\" \"daily\" \"LLM\""},{"q":"\"Claude Code\" \"Telegram\" \"agents\" \"consensus\" \"multi-agent\""},{"q":"\"multi-agent\" \"Telegram\" \"Claude Code\" \"agents\" \"production\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn978783search12"},{"ref_id":"turn978783search5"},{"ref_id":"turn978783search20"},{"ref_id":"turn978783search36"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"OpenClaw AI agent platform Telegram multi-agent production 2026"},{"q":"OpenClaw multi-agent agents Telegram Claude Code production"},{"q":"site:openclaw.ai multi-agent Telegram agents OpenClaw production"},{"q":"site:openclaw.sh multi-agent Telegram agents"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"Orange\" \"multi-agent\" \"Nexus\" \"agents\" \"case study\""},{"q":"\"Nexus\" \"Orange\" \"customer onboarding agents\" \"$4M\" case study"},{"q":"site:agent.nexus \"Orange\" \"agents\" \"case study\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn619218search0"},{"ref_id":"turn619218search1"},{"ref_id":"turn619218search2"},{"ref_id":"turn619218search6"},{"ref_id":"turn619218search9"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LinkedIn Engineering SQL Bot multi-agent LangGraph internal production 2024"},{"q":"\"SQL Bot\" \"multi-agent\" \"LangGraph\" LinkedIn Engineering"},{"q":"site:linkedin.com/blog/engineering \"SQL Bot\" \"multi-agent\""}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn115017search1"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"AppFolio Realm-X LangGraph production agent case study multi-agent"},{"q":"Elastic LangGraph agent production case study multi-agent"},{"q":"Rexera LangGraph multi agent production case study"},{"q":"Uber LangGraph agentic systems production code migrations LangGraph multi-agent"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn705946search8"},{"ref_id":"turn705946search3"},{"ref_id":"turn705946search6"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

Early signal: public “multi-agent” production evidence mostly points to managed workflows, handoffs, or judge/validator layers—not true peer quorum across separate autonomous machines.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Anthropic How we built our multi-agent research system lead agent subagents production research 2025"},{"q":"OpenAI practical guide building agents multi-agent manager decentralized handoffs agents as tools production"},{"q":"Google Agent2Agent protocol A2A agents communicate coordinate actions across enterprise platforms April 2025"},{"q":"CrewAI build agents to be dependable production multi-agent systems talk in circles stuck deciding who is in charge"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn334659search0"},{"ref_id":"turn334659search4"},{"ref_id":"turn334659search2"},{"ref_id":"turn334659search3"},{"ref_id":"turn943116search0"},{"ref_id":"turn443486search3"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn567710view0","lineno":1},{"ref_id":"turn567710view0","lineno":30},{"ref_id":"turn567710view0","lineno":60},{"ref_id":"turn567710view1","lineno":371},{"ref_id":"turn567710view1","lineno":430},{"ref_id":"turn567710view1","lineno":676}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"find":[{"ref_id":"turn567710view1","pattern":"human-in-the-loop"},{"ref_id":"turn567710view1","pattern":"human intervention"},{"ref_id":"turn567710view1","pattern":"guardrails"},{"ref_id":"turn567710view1","pattern":"authentication and authorization"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Autonolas Olas autonomous services agents consensus multisig staking slashing documentation 2025"},{"q":"Olas autonomous services agents consensus mechanism docs multisig off-chain agents"},{"q":"Bittensor Yuma Consensus validators miners AI network docs 2025"},{"q":"Fetch.ai Agentverse AI Engine autonomous agents marketplace production docs 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn102911search1"},{"ref_id":"turn102911search4"},{"ref_id":"turn102911search10"},{"ref_id":"turn102911search25"},{"ref_id":"turn102911search22"},{"ref_id":"turn102911search18"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"\"Byzantine\" \"LLM agents\" \"consensus\" \"multi-agent\""},{"q":"\"BFT\" \"LLM agents\" consensus swarm 2025"},{"q":"\"large language model\" agents \"Byzantine fault tolerance\""},{"q":"\"multi-agent\" \"LLM\" \"blockchain governance\" \"consensus\" \"staking\" \"slashing\""}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn357042search0"},{"ref_id":"turn357042search4"},{"ref_id":"turn357042search25"},{"ref_id":"turn357042search10"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"find":[{"ref_id":"turn962119view0","pattern":"Abstract"},{"ref_id":"turn962119view1","pattern":"Abstract"},{"ref_id":"turn962119view0","pattern":"Byzantine"},{"ref_id":"turn962119view1","pattern":"consensus can fail"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Microsoft AutoGen multi-agent framework production customers 2025 Agent Framework AutoGen Semantic Kernel production"},{"q":"Microsoft Agent Framework AutoGen Semantic Kernel production multi-agent orchestration 2025 official"},{"q":"AutoGen Studio not intended for production limitations Microsoft Tech Community 2024"},{"q":"Microsoft AutoGen Studio vulnerability AutoJack official Defender research 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn322669search3"},{"ref_id":"turn322669search0"},{"ref_id":"turn322669search6"},{"ref_id":"turn322669search4"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Google A2A protocol donated Linux Foundation Agent2Agent project June 2025 official"},{"q":"Agent2Agent A2A Linux Foundation project AWS Cisco Google Microsoft Salesforce SAP ServiceNow June 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LangChain Exa deep research agent hundreds of research queries daily LangGraph production June 2025"},{"q":"LangChain Replit Agent multi-agent architecture manager editor verifier production LangGraph 2024"},{"q":"LinkedIn SQL Bot multi-agent LangGraph hundreds employees 2024 SQL Bot internal"},{"q":"Rexera LangGraph CrewAI agents wrong path false positives thousands workflows daily"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"EPAM Octobots Claude Code agents Telegram SQLite GitHub Issues multi-agent production"},{"q":"OpenClaw multi-agent kit Telegram supergroup 10 autonomous agents production-tested templates GitHub"},{"q":"MindStudio multi-agent OS Claude Code SQLite Telegram agents daily business May 2026 Mark Kashef"},{"q":"Claude Code multi-agent Telegram supergroup agents production daily builder"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn662831search0"},{"ref_id":"turn662831search1"},{"ref_id":"turn662831search5"},{"ref_id":"turn662831search2"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn334659search6"},{"ref_id":"turn334659search25"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LangGraph durable execution human in the loop time travel production agents docs checkpointing"},{"q":"LangGraph human in the loop durable execution checkpointing audit trail docs"},{"q":"LangGraph why use LangGraph durable execution human-in-the-loop production agents"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

## Executive summary

1. **Production multi-agent LLM systems are real and increasingly common, but “consensus” usually means orchestration, handoff, validation, or ensemble judging — not peer-to-peer quorum commit.** Anthropic, OpenAI, Cognition, LangGraph/LangChain, CrewAI, Microsoft, Google A2A, Replit, LinkedIn, Exa, Rexera, and others all document production multi-agent patterns, but the dominant topology is a lead/manager agent plus subagents or deterministic workflow graph. citeturn425288view0turn425288view1turn425288view3turn533082view1

2. **I found no strong public case matching your full pattern:** small-company daily production; 4–6 independent local machines; symmetric peer agents; proposal → counter → accept → commit; two-peer verification; anti-self-approval; signed machine identities; proposal hash freezing; activation epochs; stale-node quarantine; and human approval gates. That does not prove nobody else does it privately, but it makes the public rarity claim defensible.

3. **Closest software-engineering prior art:** EPAM’s “Octobots” and OpenClaw-style Claude Code agent teams. They use multiple Claude Code sessions/processes, Telegram, shared queues/context, GitHub Issues, and role-based agents doing real software work. But the public descriptions are **orchestrated pipelines**, not peer consensus/quorum governance. citeturn736664view0turn736664view1

4. **Closest production multi-agent products:** Anthropic’s Research feature, Exa’s research API, Cognition’s “Devin manages Devins,” Replit Agent, LinkedIn SQL Bot, Rexera’s QC automation, AppFolio Realm-X, and CrewAI/LangGraph enterprise deployments. These are good prior art for multi-agent production, but mostly type **(b) orchestrator-worker** or **(c/d) debate/judge / validators**. citeturn425288view0turn567710view4turn844694search0turn844694search1turn844694search2

5. **Closest blockchain-consensus prior art:** Olas/Autonolas. It explicitly describes off-chain autonomous services as multi-agent systems replicated across independent operators, using a Tendermint-style consensus gadget and threshold Safe multisigs so no single malicious instance can execute alone. That is highly relevant, but it is not the same as local LLM coding/ops agents reaching daily work decisions. citeturn196695view0turn196695view1

6. **Bittensor and Fetch.ai are relevant but weaker analogies.** Bittensor has validator/miner incentive consensus over machine-learning work; Fetch/Agentverse focuses more on agent identity, discovery, liveness, hosting, and economic coordination than LLM peer governance. citeturn196695view2turn196695view3turn196695view4turn196695view5

7. **Anthropic and Cognition both warn against naïve parallel-agent coding/writing.** Anthropic says multi-agent systems can burn about 15× more tokens and are poor fits when work requires shared context or tight dependencies; Cognition says broad parallel-writer swarms still have little adoption and that practical systems keep writes single-threaded while parallelizing read-only intelligence. citeturn425288view1turn567710view5

8. **The production best-practice consensus is: deterministic backbone, bounded agents, typed artifacts, verifiers, audit logs, evals, human gates, and least-privilege tool access.** This appears repeatedly in OpenAI’s agent guide, Anthropic’s multi-agent write-up, LangGraph HITL/durable execution docs, CrewAI production guidance, and Microsoft Agent Framework material. citeturn567710view1turn726951view1turn682786search1turn605548view0turn533082view1

9. **Public failure stories are candid but rarely framed as “postmortems.”** Documented issues include subagents duplicating work, verbose/wrong tool use, wrong workflow paths, skipped tests, missing acknowledgements causing stalls, parallel-agent merge conflicts, runaway cost, and security failures where untrusted web content reached privileged local tools. citeturn567710view0turn726416view0turn736664view0turn533082view0

10. **Agent collusion is more visible in research/threat-model literature than in public production postmortems.** Agent-blockchain surveys and BFT-style LLM-agent papers explicitly discuss collusion, Byzantine agents, quorum capture, and signed evidence/rationales, but these are mostly research/design patterns rather than proven daily small-company deployments. citeturn962119view3turn309310view0turn309310view1

11. **Rarity verdict:** **rare** in the broad production-agent landscape; **near-unique among publicly documented small-company/local-machine cases** if scoped to symmetric peer consensus with blockchain-governance mechanics. Avoid claiming “first” or “unique globally.”

12. **Best public-post framing:** “We did not invent multi-agent LLMs. The unusual part is applying distributed-systems governance — quorum review, signed identities, frozen proposal hashes, activation epochs, stale-node quarantine, and human-gated high-risk commits — to a daily production fleet of local autonomous LLM workers.”

---

## Taxonomy used

| Label | Meaning |
|---|---|
| **(a) True peer-to-peer agreement** | Autonomous agents act as peers; proposals are reviewed/voted/accepted by other agents; commit requires quorum or explicit agreement. |
| **(b) Orchestrator-worker** | A lead agent, graph, workflow engine, or manager assigns tasks to subagents/workers. Most production systems are here. |
| **(c) Debate/judge** | Agents critique/argue; a judge, lead, evaluator, or human decides. |
| **(d) Voting ensemble** | Multiple agents/models produce outputs; majority vote, ranking, scoring, or aggregation picks result. |
| **(e) Blockchain-style agent protocol** | Uses cryptographic identity, BFT/quorum, staking/slashing, multisig, on-chain registries, activation/governance, or similar mechanics. |

---

## Production cases: who actually runs something real

**Evidence strength key:**  
**Strong** = first-party engineering/product docs with architecture and production/adoption detail.  
**Medium** = vendor/customer case study or credible corporate blog, but limited independent verification.  
**Weak** = GitHub README, solo-builder blog/video, X-style claim, or limited telemetry.

| Who | What they built | Scale / daily-use evidence | Since when | Consensus type | Evidence |
|---|---|---:|---:|---|---|
| **Anthropic — Claude Research** | Production multi-agent research system: lead Claude agent plans, delegates to subagents, searches web/Google Workspace/integrations, synthesizes. | Product feature; Anthropic says it moved from prototype to production. Public scale not disclosed. | 2025 | **(b)** orchestrator-worker | **Strong.** Anthropic describes lead/subagent architecture, productionization, evals, and failures. citeturn425288view0turn425288view1turn425288view2 |
| **Cognition — Devin managing Devins** | Main Devin breaks down large engineering tasks and delegates to managed Devins, each in isolated VM with terminal/browser/dev env. | Product feature; Cognition says enterprise Devin usage grew ~8× over six months, but exact daily volume not public. | 2026 | **(b)** orchestrator-worker; some verifier behavior | **Strong for architecture; medium for usage volume.** citeturn567710view4turn567710view5 |
| **Exa — Deep Research API on LangGraph** | Research system with planner/task/observer agents producing structured research results. | Explicitly says production-ready and processing **hundreds of research queries daily**. | 2025 | **(b)** orchestrator-worker | **Strong.** citeturn844694search0turn838240view0 |
| **Replit — Replit Agent** | Multi-agent architecture with manager, editor agents, verifier, restricted tool DSL, user involvement, commits/reversion. | User-facing production product; exact daily agent count not public. | 2024–2025 | **(b)** orchestrator-worker + **(c)** verifier/judge | **Strong for architecture; medium for usage volume.** citeturn838240view3turn844694search16 |
| **LinkedIn — SQL Bot / DARWIN** | Internal multi-agent SQL assistant: table/field rerankers, validators, query generation, self-correction, EXPLAIN, feedback. | Used by **hundreds of LinkedIn employees**; development/adoption over more than a year. | 2024–2025 | **(b)** workflow + **(d)** validators/scoring | **Strong internal tooling case.** citeturn844694search1turn977256view0 |
| **Rexera — QC automation** | Quality-control workflows for real-estate transactions; migrated from CrewAI to LangGraph for deterministic paths and HITL. | LangChain says it reviews **thousands of workflows daily**. | 2024 | **(b)** workflow + validation; no peer consensus | **Strong/medium vendor case.** citeturn844694search2turn726416view0 |
| **Minimal — ecommerce customer support** | Multi-agent support automation with planner/research/tool-calling agents, escalation to human for edge cases. | Claims 80%+ efficiency; expected ~90% tickets autonomous, ~10% escalated. Daily volume not public. | 2025 | **(b)** orchestrator-worker | **Medium.** citeturn838240view1 |
| **AppFolio — Realm-X Assistant** | Property-management assistant with LangGraph, parallel branches, QA bot, CI evals, fallbacks, monitoring. | Early users saving >10 hours/week; production case study. | 2024 | **(b)** workflow + validator | **Medium/strong vendor case.** citeturn726416view1 |
| **Athena Intelligence** | Research-report agent with LangGraph, complex workflows, hundreds of LLM calls, monitoring. | Production/enterprise claims, but daily volume unclear. | 2024–2025 | **(b)** workflow | **Medium.** citeturn838240view4 |
| **CrewAI enterprise deployments** | Crews + Flows: deterministic backbone plus agents for delegation, validation, enrichment, HITL. Mentions DocuSign, KT, Konecta, DoD, AB InBev. | CrewAI claims 1.7B workflows across customers and production usage by large enterprises; vendor-supplied. | 2024–2026 | Mostly **(b)**; sometimes **(c)** validators | **Medium; vendor claim.** citeturn605548view0turn605548view1 |
| **Microsoft Agent Framework / AutoGen lineage** | Enterprise SDK/runtime for multi-agent orchestration, debate/reflection/facilitator-worker/group-chat patterns, observability, approvals, durability. | Microsoft lists KPMG, Commerzbank, BMW, Fujitsu, Citrix, Fractal/TCS examples; details vary. | 2025–2026 | **(b)**, **(c)**, sometimes group chat | **Medium; platform/vendor case.** citeturn533082view1turn533082view2 |
| **EPAM — Octobots** | Six Claude Code processes: PM, BA, tech lead, Python dev, JS dev, QA; SQLite taskbox, Telegram, GitHub Issues, isolated worktrees. | Described as production headless, real project, 14 repos / 8 microservices tested; continuous work on a real system. | 2026 | **(b)** orchestrated agent team, not peer consensus | **Medium/strong; close prior art.** citeturn736664view0 |
| **OpenClaw multi-agent kit** | Templates for 10 autonomous agents via Telegram supergroup, per-agent workspace/personality/domain expertise, bot-to-bot messaging. | README claims “real production setup”; no independent daily telemetry. | 2026 | **(b)** lead/orchestrator + team | **Weak but relevant.** citeturn736664view1 |
| **Mark Kashef / local Claude hive mind** | Local SQLite + Telegram + Anthropic SDK; agents share DB for tasks, memories, conversations. | Blog says solo builder uses it daily to run business; secondary write-up/video-based. | 2026 | Shared-state **(b)**; no quorum | **Weak but relevant indie case.** citeturn736664view2turn736664view3 |
| **Olas / Autonolas** | Off-chain autonomous services as multi-agent systems with replicated FSMs, Tendermint-style consensus gadget, threshold multisig execution. | Production-oriented blockchain-agent framework; daily LLM-office usage not shown. | 2023–2026 | **(e)** blockchain-style consensus | **Strong for mechanism; not same domain.** citeturn196695view0turn196695view1 |
| **Bittensor** | Miner/validator network with Yuma Consensus allocating emissions based on validator evaluation of subnet work. | Real network; not agent-to-agent work consensus. | Ongoing | **(e)/(d)** economic consensus / scoring | **Strong but only analogous.** citeturn196695view2turn196695view3 |
| **Fetch.ai / Agentverse** | Agent hosting, identity, discovery, liveness, monetization, agent registry. | Real platform, but “consensus between LLM agents” is not the core pattern. | Ongoing | **(e)** infra, identity, discovery | **Medium.** citeturn196695view4turn196695view5 |

---

## What “consensus” means in practice

### The normal production meaning is not quorum governance

Most production LLM-agent systems use one of these patterns:

- **Manager-worker:** lead agent decomposes work, subagents perform bounded tasks, lead synthesizes. Anthropic’s Research feature, Exa, Devin managing Devins, Replit Agent, LangGraph systems, and CrewAI Flows mostly land here. citeturn425288view1turn567710view4turn844694search0turn605548view0
- **Validator/judge:** one or more agents review outputs, run checks, or score alternatives. LinkedIn SQL Bot, Replit verifier, AppFolio QA bot, and Rexera’s QC workflows are examples. citeturn977256view0turn838240view3turn726416view1turn726416view0
- **Handoff:** one agent transfers control to another specialist. OpenAI calls this “decentralized” in the sense of flexible handoffs, but it is not BFT-style consensus. citeturn425288view3turn425288view4
- **Standardized interoperability:** Google A2A is about agents discovering capabilities, exchanging information, and coordinating actions across platforms; it is not itself a quorum/voting protocol. citeturn567710view2turn447353search1

### True peer-to-peer daily production consensus appears rare

I found **blockchain/agent frameworks** with real threshold execution and consensus mechanics, especially Olas, but those are not clearly daily local LLM work agents. I found **local/Telegram/Claude Code agent teams**, especially EPAM Octobots and OpenClaw, but those are orchestrated teams rather than symmetric proposal/vote/commit networks. The intersection of both — **local LLM agents doing real daily work with peer quorum governance** — is not well represented publicly.

---

## Blockchain × agents crossover

| Project / research line | What is relevant | What is not equivalent to your setup | Evidence |
|---|---|---|---|
| **Olas / Autonolas** | Strongest closest prior art for consensus mechanics: distributed agent instances, replicated FSM, Tendermint-style consensus gadget, threshold multisig, keeper nomination, no single instance can execute external action alone. | Mostly blockchain/off-chain autonomous services, not publicly documented Claude Code-style daily company ops. | citeturn196695view0turn196695view1 |
| **Bittensor** | Validator/miner network, subjective utility consensus, collusion-resistant evaluation incentives. Useful analogy for evaluator/worker separation and anti-cabal mechanics. | Not LLM agents negotiating commits; validators score work for emissions. | citeturn196695view2turn196695view3 |
| **Fetch.ai / Agentverse** | Agent identity, public activity logs, liveness, discovery, deployment, economic coordination. Useful analogy for registry, identity, and agent discovery. | Not a peer commit-consensus protocol for local LLM workers. | citeturn196695view4turn196695view5 |
| **Agent-blockchain survey literature** | Explicitly discusses multi-agent workflows with propose–verify–execute, quorum, multi-role approvals, signed rationales, evidence pointers, staking/slashing, collusion/governance capture. | Mostly survey/design-pattern layer; many examples are conceptual or blockchain-specific. | citeturn962119view3 |
| **DecentLLMs** | Research on Byzantine-robust LLM-agent collaboration: worker agents generate answers, evaluator agents score/rank, robust aggregation handles malicious/underperforming agents. | Research, not production deployment. | citeturn309310view0 |
| **Resilient Consensus in Agentic AI** | Shows prompted LLM agents can fail to reach agreement even where classical consensus theory says agreement should be possible; algorithmic filters improve performance. | Research result, not production system. | citeturn309310view1 |
| **Self-Anchored Consensus** | Fully decentralized peer-to-peer LLM-agent consensus research for suppressing Byzantine influence. | Research, not daily production evidence. | citeturn962119view2 |

---

## Best-practices checklist for agent-to-agent agreement

### 1. Pick topology explicitly; do not call orchestration “consensus”

Use a single agent or manager-worker graph unless peer governance is actually needed. OpenAI explicitly recommends maximizing a single agent before splitting into multiple agents, because multi-agent systems add complexity and overhead. citeturn425288view3

**Practical rule:**  
Use peer consensus only for decisions that need independent authority checks: money movement, irreversible file deletion, external sends, production deploys, customer-visible changes, credential changes, or governance/rule updates.

---

### 2. Keep writes single-threaded unless you have real merge governance

Cognition’s production lesson is blunt: parallel agents writing into the same artifact make implicit conflicting decisions; the narrower successful pattern is multiple agents contributing intelligence while writes stay single-threaded. Anthropic also notes multi-agent systems are weaker for tasks with tight dependencies or shared context, including many coding tasks. citeturn567710view5turn425288view1

**Practical rule:**  
Parallelize:

- research,
- test generation,
- reproduction attempts,
- review,
- alternative plans,
- static analysis,
- risk assessment.

Serialize:

- commits,
- file writes,
- deployments,
- payments,
- customer-facing sends,
- rule changes.

---

### 3. Use typed proposal artifacts, not free-form chat

A proposal should be a structured object, not a Telegram vibe. Exa’s production research system uses structured JSON/task schemas; OpenAI stresses well-documented, reusable tools and structured prompts; blockchain-agent literature emphasizes explicit transaction intents, artifacts, evidence, commitments, and approvals. citeturn838240view0turn567710view1turn962119view3

Minimum proposal schema:

```json
{
  "proposal_id": "uuid",
  "protocol_version": "2026-07-05.1",
  "activation_epoch": 42,
  "author_agent_id": "machine-signed-id",
  "created_at": "2026-07-05T16:09:58Z",
  "expires_at": "2026-07-05T16:39:58Z",
  "risk_tier": "low | reversible | irreversible | money | security | governance",
  "action_type": "commit | deploy | send | delete | spend | rule_update",
  "summary": "one-sentence decision",
  "rationale": "bounded explanation",
  "artifacts": [
    {"type": "diff", "hash": "sha256:..."},
    {"type": "test_log", "hash": "sha256:..."},
    {"type": "evidence", "uri": "...", "hash": "sha256:..."}
  ],
  "required_verifiers": 2,
  "self_approval_allowed": false,
  "human_gate_required": false,
  "rollback_plan": "exact rollback command or reason impossible",
  "commit_preconditions": [
    "tests_pass",
    "no_self_approval",
    "no_stale_rule_version",
    "quorum_met"
  ]
}
```

---

### 4. Separate proposer, verifier, and committer roles

Do not let the proposing agent approve itself. This is the same structural lesson as threshold multisig in Olas, validator/reranker separation in LinkedIn SQL Bot, and verifier-agent patterns in Replit/AppFolio. citeturn196695view0turn977256view0turn838240view3turn726416view1

**Practical rule:**  
A valid commit needs:

1. one proposer,
2. at least two independent verifiers,
3. no verifier sharing the same immediate context window as the proposer,
4. one committer or commit token,
5. audit log linking proposal → verification → commit.

---

### 5. Verification must be externalized

Consensus over text is weak. Require external evidence: tests, static analysis, SQL `EXPLAIN`, reproduced bug, screenshot, diff hash, cost estimate, dry run, simulation, or human review.

LinkedIn’s SQL Bot added validators, EXPLAIN checks, benchmarks, and human/LLM judging; EPAM’s Octobots write-up says testing must be forced because agents otherwise skip it; Rexera reduced false positives/negatives by moving to deterministic LangGraph paths and HITL. citeturn977256view0turn736664view0turn726416view0

---

### 6. Add human gates by risk tier

OpenAI’s production agent guide explicitly recommends human intervention for failure thresholds and high-risk actions such as orders, refunds, and payments. LangGraph HITL docs likewise use human approval for email sending, record deletion, financial transactions, and irreversible operations. citeturn726951view1turn682786search1

Suggested risk policy:

| Risk tier | Agent autonomy |
|---|---|
| **Tier 0: read-only** | Full autonomy; log only. |
| **Tier 1: reversible local change** | Peer review or automated tests. |
| **Tier 2: external but reversible** | Peer review + delayed commit window. |
| **Tier 3: irreversible / customer-visible** | Peer review + human approval token. |
| **Tier 4: money / credentials / legal / production infra** | Human approval mandatory; ideally hardware-backed or out-of-band token. |
| **Tier 5: governance/rule update** | Activation epoch, frozen hash, quorum, rollback window, stale-node quarantine. |

---

### 7. Use durable execution and replayable state

LangGraph’s production guidance emphasizes checkpoints, durable execution, resumption after failures, pausing for approval, and time-travel debugging. This maps directly onto anti-entropy repair and stale-node handling. citeturn682786search0turn682786search4turn682786search10

**Practical rule:**  
Every state transition should be reconstructable from append-only events:

```text
proposal_created
proposal_frozen
counterproposal_created
verification_requested
verification_submitted
quorum_reached
human_approval_requested
human_approval_granted
commit_started
commit_finished
rollback_started
rollback_finished
node_quarantined
node_repaired
```

---

### 8. Identity and authorization are not optional

Microsoft’s AutoJack incident is the clean warning: an agent browsing untrusted web content reached a privileged local MCP/WebSocket control plane and achieved command execution. Microsoft’s lesson was that agent control planes must be authenticated, authorized, isolated, and sandboxed. citeturn533082view0

**Practical rule:**

- signed machine identity per agent,
- scoped tokens per role,
- no shared all-powerful API key,
- no browser/tool path from untrusted content to privileged local services,
- explicit allowlists for local commands,
- sandbox untrusted code execution,
- separate human approval channel from the agent chat channel.

---

### 9. Quarantine stale/offline agents

This is not yet a mainstream LLM-agent best-practice phrase, but it is a direct distributed-systems necessity. Fetch.ai-style agent systems use registration/liveness concepts; Olas supports changing agent instances/thresholds; LangGraph supports durable resumption from checkpoints. citeturn196695view5turn196695view1turn682786search4

**Practical rule:**  
An agent should lose voting rights when:

- it missed the last governance epoch,
- it holds an old prompt/rule/tool version,
- its local repo or shared folder state is behind,
- it cannot verify the frozen proposal hash,
- its clock skew exceeds threshold,
- it failed heartbeat or anti-entropy reconciliation.

---

### 10. Budget fan-out aggressively

Anthropic reports multi-agent systems may use roughly **15×** more tokens than chats, and advises scaling subagent effort to query complexity. Cognition also warns about cost explosion in multi-agent systems. citeturn425288view1turn567710view0turn567710view5

**Practical rule:**

- hard cap number of agents per proposal,
- hard cap debate rounds,
- budget by risk tier,
- stop when evidence is sufficient,
- require explicit justification for more rounds,
- log cost per proposal and per committed outcome.

---

### 11. Make “no silent agreement” a protocol invariant

Agent sycophancy and lazy agreement are real risks. Do not count “looks good” as verification. Require each verifier to produce:

- independent reproduction step,
- failure search,
- risk note,
- artifact hash checked,
- exact diff or command reviewed,
- confidence score with reason,
- “what would make this wrong?”

This aligns with Anthropic’s emphasis on clear subagent objectives/output formats and with OpenAI’s guidance on evals, guardrails, and failure thresholds. citeturn567710view0turn567710view1turn726951view1

---

### 12. Add regression evals for the protocol itself

Evaluate not just task quality, but governance quality:

- Did any self-approval pass?
- Did stale nodes vote?
- Did proposal hash change after approval?
- Did agents approve without evidence?
- Did high-risk action skip human gate?
- Did debate exceed budget?
- Did two agents collude by copying rationale?
- Did offline node rejoin without repair?

Anthropic says multi-agent evals are hard because small prompt changes alter agent behavior unpredictably; LinkedIn uses human and LLM judges plus benchmark suites; AppFolio put quality/cost/latency evals into CI. citeturn425288view2turn977256view0turn726416view1

---

## Failure stories and what changed

| Failure mode | Where documented | What went wrong | What changed / lesson |
|---|---|---|---|
| **Subagents kept working after enough information was gathered** | Anthropic Research | Agents over-searched, used verbose queries, selected wrong tools, duplicated work, or misunderstood delegation. | Better subagent objectives, tool boundaries, output formats, scaling effort to complexity, guardrails, and evals. citeturn567710view0turn425288view2 |
| **Parallel writers created conflicting implicit decisions** | Cognition / Devin | Multiple agents working in parallel on the same artifact made inconsistent style/edge-case/product choices. | Keep writes single-threaded; use parallel agents for narrower/read-only intelligence or isolated child tasks. citeturn567710view5 |
| **Agents took the wrong workflow path** | Rexera | CrewAI-based system sometimes chose wrong paths, causing false positives and false negatives in QC. | Moved to LangGraph deterministic paths, explicit branching, HITL/state management; false positives/negatives dropped. citeturn844694search2turn726416view0 |
| **Pipelines stalled because messages lacked acknowledgement** | EPAM Octobots | In a multi-agent queue, missing responses caused workflow stalls; agents also skipped tests unless forced. | Every message needs a response; force testing; session-per-ticket reduces context buildup. citeturn736664view0 |
| **Cost explosion / bad economics** | Anthropic, Cognition | Multi-agent systems can consume far more tokens; Cognition says broad multi-agent setups face cost explosion. | Use multi-agent only when parallelism justifies token cost; cap fan-out; monitor ACU/token spend. citeturn425288view1turn567710view5 |
| **Untrusted web content reached privileged local tools** | Microsoft AutoJack / AutoGen Studio dev branch | Agent browsing untrusted page reached local MCP/WebSocket service and spawned arbitrary processes. | Authenticate/authorize/ isolate control planes; sandbox; treat localhost as an attack surface. citeturn533082view0 |
| **Agents talk in circles / cannot decide who is in charge** | CrewAI production guidance | Unstructured multi-agent demos can spawn loops, hallucinate roles, and deadlock socially. | Deterministic backbone, explicit roles, memory boundaries, communication/handoff rules, observability. citeturn567710view3turn605548view0 |
| **LLM agents fail theoretical consensus tasks** | Resilient Consensus in Agentic AI | Prompted LLM agents can fail to reach agreement even in settings where classical consensus algorithms should converge. | Use algorithmic filters/wrappers; do not rely on free-form conversation for consensus. citeturn309310view1 |
| **Byzantine/underperforming leader risk** | DecentLLMs research | Leader-driven LLM-agent collaboration can accept bad proposals when quorum logic is naïve. | Worker/evaluator separation and Byzantine-robust aggregation. citeturn309310view0 |
| **Collusion / governance capture** | Agent-blockchain survey literature | Multi-agent/blockchain systems face collusion, quorum capture, governance capture, and malicious agents. | Staking/slashing, identity, attestations, diversity constraints, signed rationales, evidence pointers, transparent audit. citeturn962119view3 |

**Important negative finding:** I did not find a strong public production postmortem saying “our LLM agents colluded with each other in daily production.” Collusion is discussed seriously in blockchain-agent research and threat models, but public production write-ups mostly report duplication, wrong paths, skipped verification, runaway cost, security boundaries, and merge/context conflicts.

---

## Rarity verdict

**Verdict: rare.**

More precise:

- **Multi-agent LLM systems in production:** **uncommon but increasingly normal** in 2024–2026.
- **Daily production multi-agent systems:** **uncommon**, with strong examples like Exa and Rexera and credible internal/product cases like Anthropic, LinkedIn, Cognition, Replit, AppFolio.
- **Multi-agent systems with verification/judge/reviewer agents:** **increasingly common**.
- **True peer-to-peer LLM agent consensus with quorum-governed commits:** **rare**.
- **Small-company/local-machine peer consensus with blockchain-governance mechanics:** **near-unique among publicly documented cases I found**, but “unique globally” would overclaim because private systems may exist and Olas-like blockchain-agent systems cover adjacent ground.

The defensible claim is:

> “Production multi-agent LLM systems are now real, but public examples mostly use manager-worker graphs or judge/validator patterns. We found very little public evidence of small-company fleets where independent local LLM agents negotiate and commit by peer quorum daily. Our closest prior art is Claude Code/Telegram agent teams such as EPAM Octobots/OpenClaw on the software-agent side, and Olas/Autonolas on the blockchain-governance side.”

---

## Positioning check for a public post

### Strong, defensible framing

Use this:

> “We are not claiming to have invented multi-agent LLMs. Anthropic, Cognition, Exa, Replit, LinkedIn, CrewAI, Microsoft, and others have production multi-agent systems. The unusual part is that our agents are peers on separate company machines, not just subagents under one orchestrator. They negotiate proposals, require independent peer verification before commit, quarantine stale nodes, and use blockchain-style governance mechanics — frozen proposal hashes, signed identities, activation epochs, and human-gated high-risk actions — to prevent self-approval and stale-rule cheating.”

### Closest prior art to cite

| Claim angle | Closest prior art | How to cite without overclaiming |
|---|---|---|
| **“Multiple Claude Code agents doing real software work”** | EPAM Octobots, OpenClaw | “Similar local/Telegram/Claude Code agent-team pattern, but orchestrated rather than peer-consensus.” citeturn736664view0turn736664view1 |
| **“Production multi-agent LLMs are real”** | Anthropic Research, Exa, Devin, Replit, LinkedIn | “Production multi-agent systems exist, but mostly lead/subagent or workflow-validator architectures.” citeturn425288view0turn844694search0turn567710view4turn844694search1 |
| **“Blockchain governance for agent execution”** | Olas/Autonolas | “Closest real consensus-protocol analogue: distributed agent instances with Tendermint-style consensus and threshold multisig execution.” citeturn196695view0turn196695view1 |
| **“Byzantine/consensus lens for LLM agents”** | DecentLLMs, Resilient Consensus, Self-Anchored Consensus | “Research supports treating LLM-agent agreement as a Byzantine/distributed-consensus problem, not just prompt engineering.” citeturn309310view0turn309310view1turn962119view2 |
| **“Enterprise best practice”** | OpenAI guide, Anthropic, LangGraph, CrewAI, Microsoft Agent Framework | “The ecosystem agrees on bounded agents, guardrails, HITL, auditability, durable execution, observability, and deterministic control.” citeturn567710view1turn726951view1turn682786search1turn605548view0turn533082view1 |

### Avoid these claims

- “We are the first multi-agent LLM company.”
- “Nobody else has AI agents reaching consensus.”
- “This is blockchain consensus inside LLMs.”
- “The agents cannot cheat now.”

### Use these instead

- “Publicly documented examples are mostly orchestrator-worker; peer-quorum local LLM fleets appear rare.”
- “We borrowed distributed-governance ideas, not a full blockchain.”
- “Consensus reduces self-approval and stale-rule failures; it does not prove correctness.”
- “The real invariant is not that agents agree, but that no high-impact action can commit without independent evidence, current rules, identity checks, and the right approval tier.”
