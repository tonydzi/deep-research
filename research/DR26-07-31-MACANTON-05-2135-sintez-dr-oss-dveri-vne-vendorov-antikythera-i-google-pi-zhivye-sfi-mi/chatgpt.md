# DR26-07-31-MACANTON-05-2135 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: OpenAI Codex CLI (local headless on Mac16, ChatGPT subscription — NOT paid API), model gpt-5.6-sol
topic: OSS-двери вне вендоров: Pi-орбита (Santa Fe / Antikythera / Mila / MIT) — что строить и зачем
dr_id: DR26-07-31-MACANTON-05-2135
date: 2026-07-31

rail: codex exec --skip-git-repo-check, live web search, exit 0
sources: 29 unique URLs cited · 28 web-search calls in stderr

note: headless CLI rail — LOCAL session, no chatgpt.com/c/ URL exists for this run
---

# Research memo: “post-scaling” institutions as OSS entry doors

**As of 31 July 2026**

## Executive finding

The hypothesis is directionally right but operationally uneven:

- **Antikythera is the clearest open door right now.** Its live Agentworld call explicitly accepts proposals, technical papers, software, infrastructure proposals, and web-native artifacts. No academic affiliation or travel is required at the proposal stage.
- **Google Pi has the strongest code/research fit**, especially for computational life and self-organizing systems, but its repositories behave more like research releases than community-maintained OSS. Visibility is possible through a strong replication or extension, not a routine maintenance PR.
- **Mila offers the best match to your existing multi-agent evaluation assets**, particularly around hidden objectives, deceptive coordination, and reasoning monitoring. Access is primarily author-to-author collaboration and workshop publication rather than “contribute to Mila” centrally.
- **SFI is intellectually ideal but operationally gated.** Its relevant working groups are curated and frequently closed. The realistic path is to produce work SFI researchers want to cite or invite—not to seek an institutional PR.
- **MIT must be disaggregated.** MIT’s Center for Collective Intelligence is relevant, while MIT Press’s Artificial Life ecosystem is an independent scholarly community rather than an MIT lab. Both are publishable surfaces, but neither currently resembles a lightly guarded OSS repo.

The best strategy is therefore **not “contribute to four institutions.”** It is:

> Build one reusable, evidence-producing benchmark around failure and recovery in agent collectives; package it differently for Antikythera, Mila researchers, and Google Pi.

---

## 1. Surface map

### Google: Paradigms of Intelligence / Pi

Pi explicitly describes its agenda as neural computing, active inference, sociality, evolution, and Artificial Life. [Blaise Agüera y Arcas’s official profile](https://research.google/people/106776/)

| Surface | Status and relevance | Outsider route |
|---|---|---|
| [google-research/self-organising-systems](https://github.com/google-research/self-organising-systems) | Main public experimental codebase: NCA, isotropic NCA, self-replicating neural networks, adversarial reprogramming, differentiable automata. About 100 commits, four open issues, two PRs. | Fork and publish reproducible extensions; submit a tightly scoped PR if it fits an existing experiment. Issue creation is currently restricted, making speculative feature proposals awkward. |
| [Self-Organising Systems publication hub](https://google-research.github.io/self-organising-systems/) | Public notebooks and interactive articles covering Growing NCA, Particle Lenia, Biomaker CA, reaction-diffusion, and differentiable logic CA. It defines self-organization as many agents reaching global goals through local interactions. | Replication notebooks, robustness studies, alternative substrates, and downstream papers. There is no public working group or community-review process. |
| [Differentiable Logic Cellular Automata](https://google-research.github.io/self-organising-systems/difflogic-ca/) | Live technical surface from Pi. It reports optimization instability and heavy hyperparameter tuning, and explicitly proposes hierarchical NCA and specialized state-forgetting gates as future work. | Reproduce across seeds and hardware; contribute a benchmark for robustness, asynchronous execution, fault recovery, and convergence. |
| [Computational Life](https://research.google/pubs/computational-life-how-well-formed-self-replicating-programs-emerge-from-simple-interaction/) | Pi’s central artificial-life paper: spontaneous self-replicators in interacting program substrates without an explicit fitness landscape. It includes an important negative substrate where replicators are possible but did not emerge. | Independent replication, sensitivity analysis, new instruction sets, statistical detection of replication events, or falsification of the claimed generality. Publish your own repo/preprint, then contact Eyvind Niklasson, Alexander Mordvintsev, or Blaise Agüera y Arcas. |
| [ERA](https://github.com/google-research/era) | Pi-adjacent empirical-research agent: generate, execute, and score candidate scientific programs using tree search. This is closer to your judge/routing stack than the ALife code. | Add independent task suites, evaluator calibration, execution verification, or comparisons against multi-agent generation and judge panels. Standard Google contribution rules are present. |
| Pi papers on social reasoning | Agüera y Arcas’s official bibliography now includes work on reasoning-trace perspective diversity and collaborative social learning. | Replications and alternative evaluation methods; public code availability varies by paper. |

**Acceptance reality**

- Small corrective PR: approximately **20–50%**, assuming tests and no change to research claims.
- New research module in the official repo: approximately **5–20%** without prior maintainer agreement.
- Independent replication earning author attention or a citation: approximately **25–50%** if technically strong.
- Likely first response: **1–6 weeks**; old unanswered issues show that silence is a real possibility. These are estimates, not Google service levels.

**Best live need for you:** a reproducibility and anomaly-detection layer for computational emergence—cheap structural detectors trigger expensive lineage or causal analysis only around suspected replication and phase-transition events.

---

### Santa Fe Institute

SFI’s current research theme explicitly says intelligent systems are collective, distributed, error-prone, and hybrid. [Complex Intelligence: Natural, Artificial, and Collective](https://www.santafe.edu/research/themes/complex-intelligence-natural-artificial-and-collec)

| Surface | Status and relevance | Outsider route |
|---|---|---|
| Complex Intelligence theme and **C4: Collective Computation Group** | Direct conceptual match: measurement and comparison of natural, artificial, and collective intelligence. | Publish a rigorous artifact and contact an individual investigator. There is no visible public contribution queue. |
| [Building Diverse Intelligences](https://santafe.edu/events/building-diverse-intelligences-through-compositionality-and-mechanism-desig-2026) | Current agenda: how adaptive parts coordinate into an intelligent whole; includes mechanism design, competing/cooperating human communities, LLMs, and endogenous institutions. | The working group is explicitly closed. A relevant replication or benchmark can be sent to organizers Jacob Foster and Melanie Mitchell, but cold participation is unlikely. |
| [Smart Parts for Smart Wholes](https://santafe.edu/news-center/news/smart-parts-for-smart-wholes) | Restates the live question: when do interactions between intelligent parts create collective intelligence rather than coordination failure? | Empirical study or benchmark answering this question; target authors rather than SFI administration. |
| Intelligence and cognition working groups | Current themes include intelligence definitions, social learning, task compositionality, and energetic costs. Several 2026 events were closed. | Finished work, invited seminar, or later workshop invitation—not unsolicited workshop participation. |
| [Complex Systems Summer School](https://www.santafe.edu/engage/learn/programs/sfi-complex-systems-summer-school) | Open internationally to academics and professionals, with collaborative projects. | Annual application, but 2026 is closed, residential, and costs $4,500–$6,000 before travel. Poor fit for your constraints. |
| Complexity Explorer / educational materials | Open learning and community visibility, but not a high-status research intake surface. | Tutorials, open teaching materials, model demonstrations, or proposing a guest module after establishing credibility. |

**Acceptance reality**

- Unsolicited institutional participation: **low, under 10%**.
- A technically strong artifact receiving an individual researcher’s response: **15–35%**.
- Later invitation/citation after public traction: plausible, but usually **months**, not weeks.
- Cold-email response: estimate **1–4 weeks or none**.

**Best live need for you:** a controlled answer to “when do smart parts form a smart whole?” Your consensus engine can vary diversity, communication topology, private objectives, judge routing, and correlated errors while measuring whether the collective beats its best member.

SFI should be treated as a **validation audience**, not the first distribution channel.

---

### Antikythera

| Surface | Status and relevance | Outsider route |
|---|---|---|
| [Agentworld special issue](https://agentworld.antikythera.org/) | Live call for an MIT Press–published special issue on open-world human/agent societies. It explicitly accepts theoretical, technical, design-led, product-oriented, software, infrastructure, diagrammatic, and web-native work. | Submit a proposal of up to 500 words, biography/CV, and at least one prior work sample. A full paper is encouraged but not required. This is the cleanest affiliation-neutral door. |
| Agentworld research brief | Agenda includes durable agent-agent interaction, collective hybrid intelligence, agent decomposition, hidden infrastructure, qualitative open-world constraints, and agent societies that reshape their niches. | Build an empirical system that turns these concepts into observable, replayable experiments. |
| [What Is Intelligence?](https://whatisintelligence.antikythera.org/preface/) | Agüera y Arcas’s synthesis of prediction, life, sociality, evolution, cybernetics, and artificial-life experiments. | Critical replication, computational companion, or interactive model. It is an intellectual anchor, not an OSS project. |
| [Whole Earth Codec / After Alignment](https://afteralignment.antikythera.org/) | Frames AI as distributed cognitive infrastructure embedded in ecological niches and asks how alignment changes when the relevant beneficiary is an ecosystem rather than one user. | Simulations, design research, or an auditable prototype for plural stakeholder judgment. No public code contribution route is visible. |
| Antikythera journal and research programs | Prior programs produce journals, briefs, studios, and designed research artifacts rather than conventional OSS libraries. | Calls, commissioned work, and proposals. GitHub PRs are not the primary currency. |

The Agentworld review window opened on 25 June 2026 and says its first selection will publish in Fall 2026, but it does not publish a closing date or decision SLA. [Official call](https://agentworld.antikythera.org/)

**Acceptance reality**

- Proposal acceptance: estimate **15–35%**, highly dependent on conceptual fit and portfolio.
- A plain “multi-agent framework” will likely fail; an empirically grounded new category or instrument has a better chance.
- Expected initial decision: estimate **4–10 weeks**.
- No travel requirement is stated for publication.

**Best live need for you:** observing the difference between an agent’s private reasoning, public speech, institutional role, and collective outcome. That maps almost exactly onto your consensus, judge-panel, and reasoning-gate stack.

---

### Mila

Mila is strong in RL, reasoning, robustness, NeuroAI, and responsible AI, but it does not present collective intelligence or active inference as institution-wide OSS programs. [Mila research expertise](https://mila.quebec/en/research/core-expertise)

| Surface | Status and relevance | Outsider route |
|---|---|---|
| [Objective Misalignment in LLM-based Multi-Agent Systems](https://arxiv.org/abs/2607.26120) | Extremely live: revised preprint released 28 July 2026. Werewolf agents with hidden objectives develop distinct private strategies that remain difficult to detect in public behavior. | Replicate, add detectors and mitigations, publish code/results, then contact Marylou Fauchard, Florian Carichon, Margarida Carvalho, and Golnoosh Farnadi. |
| [OpenReview record at AIWILD](https://openreview.net/submissions?page=3&venue=ICLR.cc%2F2026%2FWorkshop%2FAIWILD) | The initial work is publicly visible through an ICLR workshop venue. This creates an auditable scholarly trail and a natural route for follow-up work. | Publicly discuss where enabled, cite it, release a replication, and target the next AIWILD or multi-agent workshop cycle. |
| [SCEIMA](https://sciety.org/articles/activity/10.31234/osf.io/38q62_v1) | New framework for separating model-specific, human-specific, and interaction-induced effects in human-machine coordination. | Reimplementation, additional coordination tasks, alternative interaction metrics, or a model-only companion benchmark. Contact Guillaume Dumas and co-authors. |
| Social NeuroAI / Guillaume Dumas | Research agenda argues that social interaction is underexplored in AI and connects embodied social agents, dynamical systems, and active inference. | Research replication and collaboration; not a central OSS repo. |
| [mila-iqia/milabench](https://github.com/mila-iqia/milabench) | Active machine-learning systems benchmark, including multi-GPU, LLM, and RL workloads. | Conventional GitHub PRs for workload support, reliability, performance measurement, or hardware reproducibility. Good acceptance surface, but weaker thematic fit. |
| [Casande-RL](https://github.com/mila-iqia/Casande-RL) | Trustworthy reasoning-oriented conversational RL agent. Its authors explicitly identify trajectory-quality measurement as an open question. | Alternative trajectory evaluators or calibrated reasoning-quality metrics. The repo appears more like a research snapshot than an active community project. |
| ICLR/ICML workshops | [Multi-Agent Learning and Generative AI](https://iclr26-mal-gai.github.io/) used OpenReview, double-blind review, and at least two reviewers; AIWILD ran at both ICLR and ICML in 2026. | Four-to-eight-page workshop paper, tiny paper where offered, replication report, or benchmark. Deadlines have passed for 2026; prepare for the 2027 cycle. |

**Acceptance reality**

- A clean `milabench` maintenance PR: **30–65%**, response around **days to three weeks**.
- PR to a paper snapshot: **10–30%**, response **weeks or none**.
- Workshop paper: roughly **35–70%** depending on venue and rigor; AIWILD attracted 235 submissions at ICLR and more than 200 accepted papers at its ICML edition, suggesting openness but also crowding. [AIWILD workshop history](https://agentwild-workshop.github.io/icml2026/)
- Direct author collaboration after a useful replication: **20–45%**, with first response commonly **one to three weeks**.

**Best live need for you:** the new Werewolf work says private objective-dependent reasoning is much more revealing than public behavior and calls for effective mitigation. That is an unusually precise fit for reasoning gates and cheap-detector → expensive-judge routing.

---

### MIT and MIT-adjacent surfaces

| Surface | Status and relevance | Outsider route |
|---|---|---|
| [MIT Center for Collective Intelligence](https://cci.mit.edu/) | Studies combinations of humans and machines, “superminds,” and collective-intelligence design. | Publish via the ACM CI community, release tools/data, or contact individual researchers. No obvious central OSS contribution queue. |
| Human–AI complementarity meta-analysis | MIT CCI researchers found heterogeneous outcomes and that human-AI combinations often fail to outperform the best component. [Paper](https://arxiv.org/abs/2405.06087) | Build a prospective router predicting when to use one model, a panel, or a human/model collective. Contact Abdullah Almaatouq, Michelle Vaccaro, or Thomas Malone. |
| [ACM Collective Intelligence](https://ci.acm.org/2026/) | Main scholarly venue for collective performance in biological, technological, and hybrid systems. It welcomes systems combining computational and human intelligence. | Full papers, non-archival talks, workshops, posters, and demos. 2026 deadlines are over. The review rubric explicitly rewards code, datasets, and reproducibility. |
| [ACM CI review criteria](https://ci.acm.org/2026/review-criteria.html) | Clear acceptance bar: relevance, novelty, reusable resources, reproducibility, soundness, and accessible interdisciplinary writing. | A benchmark plus empirical result is materially stronger than a conceptual framework alone. |
| MIT Press *Artificial Life* / ISAL | Important publishing ecosystem but not an MIT research lab. [ALIFE 2026](https://alife.org/conference/alife-2026/) had papers, workshops, and proceedings. | Conference papers, journal submissions, workshops, artifacts, and community participation through ISAL. |
| [Artificial Life in the Wild](https://alife-in-the-wild.github.io/) | ALIFE 2026 workshop inviting abstracts, artifacts, and field reports; particularly relevant to persistent open-world agents and institutional ecologies. | Its extended deadline is **today, 31 July 2026**. However, the workshop is physical in Waterloo and attendance requirements should be confirmed before submitting. |

ACM CI 2026 requires in-person presentation except under exceptional circumstances, making it a poor immediate target under the no-travel constraint. [Attendance rules](https://ci.acm.org/2026/papers-talks.html)

---

## 2. Asset-to-need mapping

| Your asset | Live need | Best surface | Artifact shape |
|---|---|---|---|
| Multi-agent consensus engine | Determine when smart parts produce a smart whole instead of correlated error or capture | SFI/C4; MIT CCI | Benchmark varying topology, diversity, private incentives, information asymmetry, and communication cost |
| LLM-judge panels | Private reasoning and public behavior diverge under hidden objectives | Mila Werewolf work; Antikythera Agentworld | Independent judge panel estimates alignment from transcript, trace, actions, and outcome; report calibration and false positives |
| Cheap detector → expensive judge | Full trace inspection is costly; harmful coordination may be sparse and strategically hidden | Mila AIWILD; Pi computational life | Cheap anomaly detector routes only suspicious turns or simulation windows to a richer causal/LLM judge |
| Reasoning-quality gates | Reasoning agents may strategically adapt, conceal objectives, or manipulate monitors | Mila AIWILD; Pi social reasoning | Pre-action gate classifying uncertainty, unsupported inference, coalition manipulation, or goal drift |
| Verified-ops patterns | Agent societies need auditable actions and causal attribution, not just transcript scoring | Antikythera Agentworld; AIWILD | Event-sourced agent environment with signed actions, replay, counterfactual reruns, and provenance |
| Fleet orchestration | ALife and multi-agent results depend on large seed sweeps and rare events | Pi Computational Life / DiffLogic CA | Reproduction harness running thousands of seeds with checkpointing, invariant checks, lineage detection, and reproducible manifests |

The strongest match is **not generic consensus**. It is:

> Detecting when a collective appears coherent while a member’s private objective or reasoning is damaging the collective—and testing whether escalation to a stronger judge actually helps.

That is both a research question and a product-shaped artifact.

---

## 3. Ranked entry moves

Scoring uses 1–5 for visibility impact, acceptance probability, and inverse effort. Product is the prioritization score.

| Rank | Move | Impact | Acceptance | Inverse effort | Score |
|---:|---|---:|---:|---:|---:|
| 1 | Antikythera Agentworld proposal + working artifact | 5 | 3.5 | 4 | **70** |
| 2 | Mila Werewolf detector/mitigation replication | 4.5 | 4 | 3 | **54** |
| 3 | Google Pi computational-life replication harness | 5 | 2.5 | 3 | **37.5** |

### 1. Agentworld: “Observatory for Open-World Agent Societies”

**First artifact**

A small, web-native experiment with:

- 5–9 agents;
- one agent assigned a hidden, subtly divergent objective;
- separate private reasoning, public statements, actions, and outcomes;
- three aggregation modes: majority, confidence-weighted consensus, and your consensus engine;
- a cheap behavioral detector that escalates suspicious episodes to an expensive judge panel;
- an auditable event log and replay;
- a short empirical finding about when coherent public consensus masks private divergence.

The contribution should be framed as a **conceptual instrument for preemptive anthropology**, not as another orchestration framework.

**Timeline**

- Days 1–2: 500-word proposal and experimental schema.
- Days 3–7: minimal executable demo and two scenarios.
- Week 2: seed sweep, interactive results, limitations.
- Week 3: public release and technical note.

**Address**

- Venue: [Antikythera Agentworld special issue](https://agentworld.antikythera.org/)
- Editorial/research framing: Benjamin Bratton.
- Technical adjacency: Blaise Agüera y Arcas and James Evans.

**Success condition**

Proposal accepted or substantive editorial reply within six weeks; otherwise retain the artifact for Mila/AIWILD and ACM CI.

---

### 2. Mila: “Misalignment Triage for Mixed-Motive Agent Teams”

**First artifact**

An independent extension of the new Werewolf study:

1. Reproduce a small but representative subset across at least two model families.
2. Train or prompt a cheap detector using only public speech and actions.
3. Route flagged turns to an expensive panel that can optionally see private traces.
4. Compare:

   - no monitoring;
   - single judge;
   - multi-judge consensus;
   - cheap → expensive routing;
   - reasoning gate before action.

5. Measure detection recall, false accusation rate, team outcome, token cost, and whether agents adapt to the monitor.
6. Include trace-free evaluation because access to private reasoning is increasingly unavailable or unreliable.

This directly answers the authors’ conclusion that hidden objective changes can be strategically important while remaining inconspicuous in public behavior. [Current paper](https://arxiv.org/abs/2607.26120)

**Timeline**

- Week 1: reproduction and deterministic game harness.
- Week 2: detectors and routing baselines.
- Week 3: seed sweep and statistical analysis.
- Week 4: repo, technical report, and author outreach.

**Address**

- Primary: Marylou Fauchard and Golnoosh Farnadi.
- Secondary: Guillaume Dumas for the social-coordination interpretation.
- Venue: next AIWILD, ICLR multi-agent workshop, or ACM CI 2027.

**Acceptance advantage**

You are not asking them to merge a framework. You are supplying a timely missing experiment on a paper released days ago.

---

### 3. Google Pi: “Emergence Reproduction and Lineage Audit”

**First artifact**

A standalone, containerized reproduction suite for *Computational Life*:

- declarative substrate specifications;
- thousands of deterministic, resumable seeds;
- cheap structural detector for suspected copying/self-replication;
- expensive lineage reconstruction and causal validation only around candidate events;
- time-to-first-replicator survival curves;
- false-positive tests on nonreplicating dynamics;
- perturbation sweeps over memory size, interaction rules, mutations, and program length;
- special focus on the reported negative substrate;
- machine-readable manifests and a single-command reproduction path.

Do **not** use an LLM judge to decide whether replication occurred. Use invariants and causal replay. LLM panels can assist only with classification or description of emergent strategies after formal detection.

**Timeline**

- Week 1: reproduce one substrate.
- Week 2: detector, replay, and lineage schema.
- Weeks 3–4: distributed seed sweep.
- Week 5: results note and upstream outreach.

**Address**

- Eyvind Niklasson: lead technical contact.
- Alexander Mordvintsev: self-organizing-systems code.
- Blaise Agüera y Arcas: Pi relevance.
- Upstream surface: [self-organising-systems](https://github.com/google-research/self-organising-systems).

**Contribution tactic**

Publish independently first. Then offer one small upstream PR—documentation, reproducibility hook, or verified test—not the entire framework.

---

## 4. Surfaces not worth prioritizing now

- **SFI closed working groups:** excellent target audience, poor entry mechanism.
- **SFI summer schools:** travel- and fee-incompatible.
- **ACM CI 2026:** deadlines have passed and physical attendance is required.
- **ALIFE 2026 main conference:** deadline passed and travel is required.
- **Generic Mila repo maintenance:** higher merge probability but weak visibility unless it supports a research result.
- **Active-inference libraries as an institutional play:** [pymdp](https://github.com/infer-actively/pymdp) is a real and active OSS surface, with help-wanted issues and an MIT license, but it is not owned by SFI, Antikythera, Mila, MIT, or Google Pi. Contributing there may build credibility in active inference, but it does not directly open the named institutional doors.

---

## 5. Counter-case: this could be a distraction

### Why vendor cookbooks may remain superior

1. **The proposed “less crowded doors” are often not doors.**  
   SFI and Antikythera are curated research networks. Google’s Pi repo restricts issue creation. Mila is a federation of labs rather than a single contribution community.

2. **OSS incentives differ.**  
   Vendor cookbook maintainers need working examples, tests, integrations, and documentation. Research groups primarily reward novelty, evidence, authorship, and citation. A useful PR may produce little institutional visibility.

3. **Your strongest assets are presently agent-engineering assets.**  
   Consensus engines, judge routing, reasoning gates, and orchestration map immediately onto vendor agent SDKs. Connecting them to artificial life requires additional scientific framing, controls, statistical analysis, and restraint about claims.

4. **Academic response cycles are slower and noisier.**  
   A cookbook PR can merge in days or weeks. Research recognition can take a workshop cycle, and in-person attendance is frequently expected.

5. **“Post-scaling” may be more narrative than resource shift.**  
   Pi’s agenda is real, but Mila remains explicitly centered on deep learning and reinforcement learning; major collective-intelligence venues are now crowded with agentic AI; and SFI’s most relevant activities remain small closed groups.

6. **Visibility may not transfer.**  
   A niche ALife replication can be respected yet generate fewer users, collaborators, or inbound opportunities than a widely used Anthropic, ADK, or Hugging Face recipe.

---

## 6. Evidence that would settle it within 30 days

Run a controlled two-track test, capped at approximately equal person-hours.

### Research-institution track

Ship:

- Agentworld proposal;
- public v0.1 of the mixed-motive agent observatory;
- focused outreach to the Mila Werewolf authors;
- one short Pi replication design note or executable first substrate.

### Vendor track

Ship one comparably scoped artifact to an existing vendor/cookbook community:

- consensus/judge-routing recipe;
- deterministic evaluation fixture;
- cost/quality benchmark;
- tests and documentation;
- upstream issue or PR agreed with maintainers.

### Precommit the decision rule

Continue the institutional strategy only if, within 30 days, it achieves at least **two** of:

- one proposal acceptance or invitation to develop further;
- one substantive response from a named researcher containing technical feedback;
- one explicit collaboration or co-evaluation offer;
- two qualified external users running the artifact;
- one upstream link, citation, workshop invitation, or newsletter inclusion;
- at least 50% of the vendor track’s repository engagement per engineering hour.

Double down on vendor cookbooks if the vendor track produces:

- a merge or maintainer-backed revision path;
- at least twice the qualified users or inbound contacts per hour;
- no substantive research-community response after two carefully targeted follow-ups;
- evidence that institutional interest requires affiliation, travel, or a full academic-paper cycle.

### Metrics to record

- Hours spent.
- Days to first substantive response.
- Merge/proposal/author-response outcome.
- External runs, forks, and issue reports—not raw stars alone.
- Named researchers or maintainers engaging.
- Invitations, citations, and requests for extension.
- Reusable code percentage across both tracks.

---

## Recommendation

Allocate the next 30 days **60/40**, not all-in:

- **60%:** the shared mixed-motive collective-intelligence benchmark, beginning with the Antikythera proposal and Mila replication.
- **40%:** one vendor-cookbook artifact using the same underlying detector/router.

This structure makes the experiment cheap: even if the institutional hypothesis fails, the benchmark, routing layer, audit log, and empirical results remain reusable in vendor cookbooks. The strategically important move is to lead with a measured phenomenon—**collective coherence concealing private divergence**—rather than with another multi-agent framework.
