# DR26-08-06-HUB-01-1055 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-06-HUB-01-1055
vendor: xAI Grok
door: cli-headless
rail: grok
cmd: «внутренний путь лаборатории»

collected: 2026-08-06T14:54:43Z
took_sec: 248
exit: 0
sources_unique_urls: 37
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

Начинаю веб-разведку по TRIZ: сначала зафиксирую ядро метода, доказательства и применения к software/ops, затем столкновение с радикальной простотой.# DR26-08-06-HUB-01-1055  
## TRIZ as a working toolkit for debugging agentic AI systems — and where it collides with radical simplicity

**Mode:** external web reconnaissance only. No local files, no project canon.  
**Date of research:** 2026-08-06.  
**Verdict up front:** TRIZ is a real, historically grounded inventive-problem method with useful *fragments* for ops/agent failures (contradiction framing, IFR, trimming). The full apparatus (matrix, Su-Field, ARIZ, certification stack) has a **weak controlled-evidence base**, high learning cost, and a structural bias toward *clever resolution of trade-offs* that often **adds** mechanism — which fights an “AK-47 / non-engineer can repair it” doctrine. For a two-person lab, partial adoption is coherent **if** you treat TRIZ as a **question checklist**, not a religion; full classical TRIZ is the wrong product.

---

# A. What TRIZ actually is (stripped of mythology)

## A1. Core instruments — problem shape, INPUT, OUTPUT

### Origins (baseline fact) **[established]**

TRIZ (*teoriya resheniya izobretatelskikh zadach*, “theory of inventive problem solving”) was developed from ~1946 by Genrich Altshuller and colleagues in the USSR, by mining patent literature for repeated problem/solution patterns. Three classical claims: (1) problems and solutions recur across domains; (2) technical systems evolve along repeatable patterns; (3) solutions often import effects from other fields.1(https://libquotes.com/john-gall/quote/lbk2l5f)

Classic claim size: tens of thousands of patents initially; modern marketing often says “millions” — treat large-N patent counts as tradition, not as a modern meta-analysis with published sampling protocol.

---

### 1) Technical contradiction + Contradiction Matrix (39 × 39 → 40 principles)

| | |
|---|---|
| **Problem shape** | “If I improve A, B gets worse” — a **trade-off** between two parameters (e.g. reliability vs complexity). |
| **INPUT** | Named improving parameter + worsening parameter (classically one of **39** engineering parameters: weight, speed, reliability, ease of manufacture, etc.). |
| **OUTPUT** | A short list of recommended **Inventive Principles** (typically up to ~4 cell entries) from the **40 principles**, plus human interpretation into a concrete design move. |

Altshuller’s matrix maps “improve X / worsen Y” cells to principle numbers statistically associated with patents that resolved that conflict without mere compromise.2(https://en.wikipedia.org/wiki/John_Gall_(author))

**Transfer note:** the 39 parameters are **mechanical/physical-era** (mass, force, temperature…). Mapping “cookie session dead” or “Syncthing lag” onto them is **metaphorical**, not mechanical science. Software rephrasings of the 40 principles exist (e.g. Mishra 2014 SSRN “Revised 40 Principles for Software Inventions”) but are reinterpretations, not re-derived patent matrices. **[single-source / low-authority for software matrix validity]**

---

### 2) Physical contradiction + Separation principles

| | |
|---|---|
| **Problem shape** | The **same** parameter must be **both** high and low (or present and absent): “must be hot and cold,” “must be present and absent.” |
| **INPUT** | One parameter + two mutually exclusive required states + the reason each state is needed. |
| **OUTPUT** | A **separation strategy**, then a design that implements separation. |

Canonical separations (wording varies slightly by school):

1. **In time** — property A at T1, opposite at T2.  
2. **In space** — property A in region R1, opposite in R2.  
3. **On condition** — property depends on a trigger/condition.  
4. **Between system and parts** (or scale) — whole has property X, parts have not-X (or vice versa).3(https://fs.blog/chestertons-fence/)

This is often more useful for ops than the matrix, because many reliability problems *are* physical contradictions (“must be offline for safety / must be online for liveness”).

---

### 3) IFR / Ideal Final Result and ideality ratio

| | |
|---|---|
| **Problem shape** | “What is the ideal end-state that removes the problem without adding cost/harm?” |
| **INPUT** | Function the system must deliver + current costs/harms/resources. |
| **OUTPUT** | A directional ideal (“function performed **by itself**, with **zero** system”) + stepwise moves toward higher ideality. |

Common formalization in TRIZ literature:

**Ideality ≈ Σ Benefits / (Σ Costs + Σ Harm)**  

IFR is “the problem is fully eliminated with minimal changes to the system / resources already present do the work.”

**Critical distinction for your lab:** IFR optimizes **function vs cost+harm**, not **repairability by a novice**. Those often align (less stuff = less to break) but diverge when “harm” is defined as energy/parts/cost, not as “cognitive load for a non-engineer.”

---

### 4) The 40 Inventive Principles — which transfer to software/ops

Full list is public (Segmentation, Taking out, Local quality, Asymmetry, Merging, Universality, Nested doll, Anti-weight, Preliminary anti-action, Preliminary action, Beforehand cushioning, Equipotentiality, ‘The other way round’, Spheroidality, Dynamics, Partial or excessive actions, Another dimension, Mechanical vibration, Periodic action, Continuity of useful action, Skipping, ‘Blessing in disguise’, Feedback, Intermediary, Self-service, Copying, Cheap short-living objects, Mechanics substitution, Pneumatics/hydraulics, Flexible shells, Porous materials, Colour changes, Homogeneity, Discarding and recovering, Parameter changes, Phase transitions, Thermal expansion, Strong oxidants, Inert atmosphere, Composite materials).

**Plausibly high transfer to agentic ops (software metaphors work):**

| # | Principle | Ops/agent reading |
|---|-----------|-------------------|
| 1 | Segmentation | Split monolith agents/pipelines into small jobs |
| 2 | Taking out | Extract toxic/fragile piece (auth) out of main loop |
| 3 | Local quality | Different quality/SLA per machine/role |
| 5 | Merging | Batch; dual-rail send as one operation |
| 6 | Universality | One deploy script for all nodes |
| 10 | Preliminary action | Pre-warm sessions; pre-fetch OTP paths |
| 11 | Beforehand cushioning | Circuit breakers, retries with budget |
| 13 | The other way round | Invert control: pull vs push deploy |
| 15 | Dynamics | Adaptive rate limits; mode switches |
| 16 | Partial/excessive action | Over-scrape then filter; under-do then iterate |
| 22 | Blessing in disguise | Use failure as signal (fail closed) |
| 23 | Feedback | Heartbeats that prove *output*, not process |
| 24 | Intermediary | Message bus, queue, proxy |
| 25 | Self-service | Auto-heal; self-apply on receipt |
| 26 | Copying | Shadow runs; canaries; dry-run twin |
| 27 | Cheap short-living objects | Ephemeral containers/sessions; burn tokens |
| 34 | Discarding and recovering | Dispose cookies/profile; rebuild from pure state |
| 35 | Parameter changes | Change cadence, batch size, model tier |

**Weak / forced transfer (physics residue):** 8 Anti-weight, 14 Spheroidality, 18 Vibration, 29 Pneumatics, 30 Flexible shells, 31 Porous materials, 36–40 phase/thermal/oxidants/composites — only via strained metaphor. **[established for classical list; speculative for ops transfer ranking]**

---

### 5) Laws / trends of engineering system evolution

Commonly cited trends (names vary by school; often ~8–11 “laws”):

- **S-curve** of system maturity (birth → growth → maturity → decline/replacement).  
- **Increasing ideality** (more function, less cost/harm).  
- **Transition to super-system** (component absorbed into larger system).  
- **Trimming** (remove parts while keeping function).  
- **Dynamization** (rigid → adjustable → adaptive).  
- **Increasing controllability / coordination**.  
- Uneven development of parts, etc.1(https://libquotes.com/john-gall/quote/lbk2l5f)

**INPUT:** current system stage + missing functions.  
**OUTPUT:** forecasted next form + candidate improvements aligned with the trend.

For ops: “transition to super-system” often means **move function into the platform** (OS scheduler, browser profile manager, Syncthing itself) rather than custom robots — which can *increase* coupling to opaque platforms (ban risk, vendor APIs).

---

### 6) Substance-Field (Su-Field / vepol) analysis + 76 Standard Solutions

| | |
|---|---|
| **Problem shape** | Incomplete interaction model: substance S1 acts on S2 via a “field” (mechanical, thermal, information…). |
| **INPUT** | Su-Field model of the incomplete/harmful/insufficient interaction. |
| **OUTPUT** | One of **76 Standard Inventive Solutions** (5 classes: build/complete models; improve; transition to supersystem/micro-level; measurement/detection; helpers). |

In software, “field” is usually **information/control flow**. Useful as a diagram habit; the 76 standards are dense and rarely completed outside specialist training. **[established instrument; speculative value for software without training]**

---

### 7) ARIZ (Algorithm of Inventive Problem Solving)

ARIZ (esp. **ARIZ-85C**) is a long sequential program (~parts for problem model → resources → IFR + physical contradiction → mobilize resources → apply knowledge base → analyze solution). Altshuller framed it as the tool for **non-typical** problems; practitioners repeatedly note it is heavy and “don’t skip steps.”4(https://www.augmentcode.com/guides/ai-sre-ai-powered-site-reliability-engineering)

**Who completes it outside classic mechanical engineering?** Documented uses exist in industrial/mobile design case studies, but literature and community practice treat full ARIZ as **expert-only**. Even TRIZ Journal commentary notes ~95 steps and instruction not to skip.5(https://neurips.cc/virtual/2025/poster/116834)  
**Honest status:** almost nobody in a two-person software lab will complete ARIZ-85C end-to-end on a cookie-drift incident. **[emerging consensus among practitioners; not a controlled survey]**

---

### 8) Function Analysis / trimming, CECA, Nine Screens, Size-Time-Cost, Smart Little People

| Tool | INPUT | OUTPUT | Problem shape |
|------|-------|--------|---------------|
| **Function Analysis + Trimming** | Component–function model (useful / harmful / insufficient) | Components removed or functions reassigned; simpler structure | Cost/complexity bloat |
| **CECA (Cause-Effect Chain Analysis)** | Observed undesirable effect | Chain of causes to root contradictions | RCA with structure |
| **Nine Screens / System Operator** | System × past-present-future × sub-system / system / super-system | Wider solution space | Tunnel vision on present component |
| **Size-Time-Cost (STC) operator** | Extreme “make size/time/cost → 0 or ∞” thought experiment | Resource ideas | Stuck local optimum |
| **Smart Little People** | Zone of conflict modeled as tiny agents | Visualized interactions → separation ideas | Hard-to-visualize conflict zones |

Function analysis/trimming is one of the strongest bridges to simplicity doctrine (remove parts, keep function).  
CECA appears in modern multi-agent TRIZ case work as a first analysis step.6(https://arxiv.org/html/2506.18783v1)

---

## A2. Classical TRIZ vs modern derivatives — who owns what / what is marketing

| Lineage | Who / what | What’s real | What’s marketing |
|---------|------------|-------------|------------------|
| **Classical TRIZ** | Altshuller et al., USSR; public core: 40 principles, matrix, IFR, trends, Su-Field, ARIZ | Historical method + open literature | “Scientific theory” as in physics — contested |
| **I-TRIZ** | **Ideation International** (founded ~1992 by emigrating TRIZ specialists); software + Directed Evolution, AFD, etc. | Commercial modernization | Product suite branding as “the” modern TRIZ |
| **GEN-TRIZ** | **GEN TRIZ, LLC** (Simon Litvin et al.) — consulting “modern TRIZ” product innovation | Function analysis lineage (Gerasimov/Litvin school) heavily used in industry consulting | Corporate method packaging |
| **SIT / ASIT** | Israel mid-1990s (Goldenberg, Horowitz, Levav…): **Closed World** + few thinking tools; deliberately **simpler** than TRIZ | Academic + HBR presence; fewer rules | Still a branded method |
| **USIT** | Ed Sickafus at **Ford** from Israeli SIT + TRIZ, simplified industrial process | Documented Ford training lineage | Not classical TRIZ |
| **Oxford TRIZ™** | Oxford Creativity commercial simplification | Practitioner product | Trademarked simplification |
| **TOP-TRIZ** | Zinovy Royzen; Wikipedia notes **not** accepted by MATRIZ as definitive modern TRIZ | One master’s system | Competing “next gen” claim |
| **MATRIZ / MATRIZ Official** | International TRIZ Association certification Levels 1–5 | Real certification ecosystem | Lifetime certificates; industry, not ISO science |
| **ISO 69580:2025** | User query referenced this | **[no source found]** for an ISO standard numbered 69580 defining TRIZ certification in public indexes searched 2026-08. Do not treat as established. | Possible confusion with other ISO/TRIZ biomimetics docs or future drafts |

SIT explicitly *differs* from TRIZ: fewer tools, Closed World as primary constraint, less engineering-specific apparatus.  
USIT history: SIT into Ford → Sickafus USIT textbook 1997.7(https://en.wikipedia.org/wiki/Unified_structured_inventive_thinking)  
I-TRIZ: Ideation International post-1992 emigration.7(https://en.wikipedia.org/wiki/Unified_structured_inventive_thinking)  
MATRIZ levels 1–5 practitioner→master.8(https://triz-japan.org/PRESENTATION/sympo2011/Pres-Japan/JI09eS-Nakagwa(Osaka%20Gakuin%20University)-110815.pdf)

**Liberating Structures “TRIZ”** is a *facilitation* exercise (“make space by imagining how to destroy the work”) — **not** classical TRIZ; inspired by “one small element.”1(https://libquotes.com/john-gall/quote/lbk2l5f) Do not confuse.

---

## A3. Evidence base — studies, critiques, honesty

### What exists

1. **Industrial self-reports (dominant form)**  
   - Samsung (Forbes / widely repeated): TRIZ → 50 patents (2003); one DVD pick-up project **~$100M savings** (2004); TRIZ mandatory for advancement. **[single-source chain for the $100M figure; corporate PR risk]**  
   - Intel case study cited on Wikipedia claiming **>$212.5M ROI** over 21 months — Wikipedia tags **non-primary source needed**.1(https://libquotes.com/john-gall/quote/lbk2l5f) Treat as **unverified secondary**.  
   - Long lists of company names (Boeing, GE, P&G…) appear in secondary sources without open methods. **[low-authority for causal effect]**

2. **Software case with numbers (rare)**  
   Bhushan, TRIZ Journal / TRIZCON2008: identity-masking product “Akiva”; structural ideality + System Complexity Estimator; **LOC 7,964 → 3,866 (−50%+)**; complexity score ~88.7 → 81.2. Tools used: ideality + structural redesign (router pattern), not full ARIZ.9(https://www.econlib.org/misusing-chestertons-fence/) **Self-reported case study; no control group.**

3. **Controlled-ish experiment (medical device design)**  
   Dathe DBA thesis (University of Gloucestershire, 2017): 2×2 experiment, experienced practitioners, TRIZ vs brainstorming on sample medical device improvements; quality of solutions and group work analyzed; impact **depends on problem type**; claims systematic approach can beat pure trial-and-error — **not** a large RCT, domain-specific. **[emerging; small N design study]**

4. **Academic review of practice challenges**  
   Ilevbare, Probert & Phaal, *Technovation* 2013 (highly cited): TRIZ **complex**, needs substantial effort, **lacks accepted standard** for application; benefits in structured ideation coexist with organizational/cultural barriers.10(https://www.facebook.com/victoria.haliburton.1/posts/quote-of-the-daygalls-law-is-popular-in-technology-circles-a-complex-system-that/10157445594603455/) **[established as the main critical review]**

5. **LLM+TRIZ evals (2024–2025)**  
   AutoTRIZ (Jiang et al.): contradiction detection on 10 textbook cases × 100 runs; **7/10** top-3 match or half-match textbook analysis; solutions plausible but not production-validated.  
   TRIZ Agents multi-agent (Szczepanik & Chudziak, ICAART 2025): multi-agent TRIZ workflow on gantry-crane case — demo, not ops reliability RCT.

### What does **not** exist (important)

- No large **randomized controlled trials** showing TRIZ > TOC / DoE / expert consultation for **software reliability / MTTR / incident rate**.  
- No credible **SRE/agent-fleet** field experiment with TRIZ as intervention.  
- Patent-mining → 40 principles: selection bias / post-hoc rationalization critiques are **structurally plausible** (survivorship of patented solutions; principles can re-describe almost any design after the fact) — **[speculative as formal refutation; established as methodological concern]** Ilevbare et al. document complexity and standardization problems rather than a formal statistical audit of the 40 principles.10(https://www.facebook.com/victoria.haliburton.1/posts/quote-of-the-daygalls-law-is-popular-in-technology-circles-a-complex-system-that/10157445594603455/)

### Main published critiques (steelman)

| Critique | Content | Strength |
|----------|---------|----------|
| **Complexity / no standard** | Hard to learn; application not standardized | **[established]** Ilevbare 2013 |
| **Consultancy case study literature** | Success stories without controls | **[established]** pattern in secondary sources |
| **Post-hoc principle fitting** | Any clever design can be labeled with a principle after invention | **[speculative mechanism; widely argued by method skeptics]** |
| **Patent sample selection** | Corpus biased to what was patentable/published in Soviet/industrial contexts | **[speculative / emerging]** |
| **Not causal science** | Heuristic taxonomy ≠ predictive theory | Philosophical; consistent with weak RCT record |

**Consensus map:** TRIZ is a **useful structured creativity system** with industrial adoption anecdotes; it is **not** a validated clinical-grade method for reliability engineering. **[established]**

---

# B. TRIZ applied to software, agents, infrastructure — with numbers

## B1. Real documented applications (not “could be”)

| Case | Org / year | Problem | Instruments | Measured result | Source quality |
|------|------------|---------|-------------|-----------------|----------------|
| Akiva data-masking redesign | Software product (Bhushan) / published 2008 | Structural complexity, maintainability | Ideality as structural simplicity; SCE; redesign toward router | LOC **7964→3866**; complexity **~89→81** | TRIZ Journal case; self-reported; **[single-source]** |
| Samsung electronics | Samsung / ~2000–2004 | Product innovation program | Enterprise TRIZ program | **50 patents** (2003); **~$100M** one DVD project (2004) | Forbes 2013; **[low-authority for causality]** |
| Intel manufacturing ecosystem | Intel / claimed 21-month deploy | Semiconductor productivity | TRIZ deployment (details opaque) | **>$212.5M** ROI claimed | Wikipedia cites ResearchGate; **non-primary; treat as unverified** |
| Medical devices | Dathe thesis 2017 | Design improvement vs brainstorming | TRIZ vs BS 2×2 | TRIZ advantage depends on problem type | Academic thesis; limited N |
| Mechanical / manufacturing | Many TRIZfest / CIRP survey papers | Various | Mixed tools | Critical survey notes case-study quality issues | Spreafico & Russo 2016 cited in AutoTRIZ refs |

**Not found:** peer-reviewed TRIZ case with **MTTR, incident count, SLO burn** as primary endpoints in SRE. **[no source found]**

## B2. Debugging, reliability, LLM agents, prompts (esp. 2024–2026)

| Topic | What exists | Verdict |
|-------|-------------|---------|
| **(a) Debugging / RCA** | CECA + contradiction is used inside TRIZ workflows; not competitive with STPA/CAST literature in software safety | Weak direct TRIZ-for-RCA corpus |
| **(b) Reliability / fault-tolerance design** | Classical TRIZ used in hardware reliability trade-offs; software mostly metaphor | Sparse |
| **(c) LLM multi-agent systems** | **TRIZ Agents** (2025): multi-agent LLM *implements TRIZ*, does not use TRIZ to *debug agents* | Research prototype |
| **(d) Prompt/architecture for AI** | AutoTRIZ / TRIZ-GPT style: LLMs *apply* TRIZ steps; reverse use (TRIZ to design prompts) is thin | Emerging research, not production ops playbooks |

Adjacent 2025–2026 work on **agentic SRE** (STRATUS, Google AI Operator, multi-agent incident agents) uses **LLM multi-agents**, not TRIZ.5(https://neurips.cc/virtual/2025/poster/116834) Do not conflate.

## B3. Your four failure classes — full TRIZ formulation + honesty check

### Failure 1 — Watchdog independence vs internal visibility

**Technical contradiction (matrix-style):**  
- Improve: **Reliability of detection** (must not share fate with the watched process).  
- Worsen: **Accuracy / completeness of state** (must see internal truth).  
- Classical-ish parameters: Reliability vs Amount of information / Measurement accuracy (forced mapping).

**Physical contradiction:**  
The monitor **must be coupled** (to see internal state) **and must be uncoupled** (to survive watched-system death).

**Separation:**  
- **In space / system–parts:** external process/machine watches **artifacts at the consumer** (output freshness), not the producer’s internals.  
- **On condition:** deep probes only when **external heartbeat fails** (escalate visibility).  
- **In time:** continuous cheap external liveness; periodic expensive deep inspection.

**IFR:** *There is no watchdog process; the system’s own useful outputs continuously prove health, and absence of fresh useful output is itself the alarm — without a co-located monitor that can die with the job.*

**Non-obvious?** **Mostly relabel.** Competent SRE already says “watch the **output**, not the process; independent plane.” TRIZ names the contradiction cleanly; the solution is **standard**. **Judgment: useful as discipline, not invention.**

---

### Failure 2 — Browser automation: human-like logged-in vs unattended

**Technical contradiction:**  
Improve **access completeness** (subscription features, anti-bot acceptance) ↔ worsen **unattended operability / ban risk / secret exposure**.

**Physical contradiction:**  
Session **must look like a real continuous human presence** and **must run with zero human**.

**Separation:**  
- **In time:** human presence only at **auth/bootstrap**; robots use **stable session artifacts** afterward.  
- **In space:** dedicated “identity machine/profile” vs task workers.  
- **On condition:** interactive path only when auth state invalid.  
- **System–parts:** browser **profile** holds humanness; **script** holds automation (don’t merge into one fragile super-agent).

**IFR:** *No robot “logs in”; a durable authorized environment already exists and tasks merely use it; bans are impossible because nothing synthetic is exposed* — practically unreachable; nearest: **one warm human session plane + headless consumers of that plane’s outputs**, not full browser mimicry everywhere.

**Non-obvious?** **Partial.** TRIZ pushes toward **separating identity plane from task plane** (good). “Self-service + preliminary action” can also push **over-clever session farms** (bad for AK-47). **Judgment: contradiction framing helps; principles can overshoot into ban-risky cleverness.**

---

### Failure 3 — Fleet-wide fix vs staggered offline / receive-only shares

**Technical contradiction:**  
Improve **completeness of rollout** ↔ worsen **availability / autonomy of offline or receive-only nodes**.

**Physical contradiction:**  
Fix **must be applied everywhere at once** and **cannot be applied everywhere at once**.

**Separation:**  
- **In time:** deliver now, **apply when alive** (store-and-forward).  
- **In space:** hub holds canonical package; peers hold local apply.  
- **On condition:** apply only if **verify predicate** fails.  
- **System–parts:** delivery channel ≠ apply executor ≠ verify auditor (three planes).

**IFR:** *There is no “deploy.” Each node, when it wakes, already has the correct state because the desired configuration is the only reachable equilibrium (pull + self-heal + proof).*

**Non-obvious?** **Mostly relabel of desired-state / GitOps / outbox patterns.** TRIZ helps refuse “I sent the file” as “done.” **Judgment: IFR + separation in time are high value; matrix low value.**

---

### Failure 4 — Quiet when healthy vs silence ≠ death of indicator

**Technical contradiction:**  
Improve **signal-to-noise (quiet when OK)** ↔ worsen **detectability of monitor failure**.

**Physical contradiction:**  
Indicator **must produce no noise** when healthy and **must produce evidence of life** always.

**Separation:**  
- **In time:** rare **explicit heartbeat** (e.g. daily “I am alive and last success was T”) separate from **incident alerts**.  
- **On condition:** silence OK only if **fresh external artifact** exists; else alarm.  
- **System–parts:** **liveness channel** ≠ **fault channel**.

**IFR:** *No indicator process. Fresh useful work product is the only status; staleness is the alarm.*

**Non-obvious?** **Relabel of “dead man’s switch / output freshness.”** Extremely well-known in reliability; TRIZ restates it. **Judgment: IFR wording is excellent pedagogy; not a novel design.**

**Cross-cutting honesty:** For your four classes, TRIZ’s **win is problem formulation quality**, not rare inventive principles. Most “solutions” are what a good SRE handbook already contains.

---

## B4. Head-to-head vs alternatives (same problem class)

| Method | Beats TRIZ when… | TRIZ beats it when… | Learn cost (order of magnitude) | Sources / notes |
|--------|------------------|----------------------|----------------------------------|-----------------|
| **5 Whys** | Linear causal chain, single root, time-critical incident | Trade-off is structural, not linear cause | Hours | Classic lean; shallow on contradictions |
| **Kepner-Tregoe** | Situation appraisal, decision analysis, IS/IS-NOT | Need generative redesign, not choice among options | Days–weeks | Industrial decision method |
| **FMEA** | Enumerate failure modes pre-build; RPN ranking | Contradictory requirements, not ranked modes | Days | Complements TRIZ (risk vs inventiveness) |
| **Fault Tree Analysis** | Known failure top event, Boolean structure | Solution generation under conflict | Days | Analytical, not inventive |
| **STAMP/STPA/CAST** | Socio-technical safety, control loops, loss scenarios (Google SRE STPA writeups show proactive hazard finds) | Pure product invention under trade-off | Weeks–months | Stronger for **complex control systems** than TRIZ for software safety |
| **TOC** | Throughput bottleneck is clear; exploit/subordinate | Many simultaneous inventiveness trade-offs | Days (core), months (thinking processes) | TOC = bottleneck management; TRIZ = contradiction invention |
| **Cynefin** | Domain is chaos/complex; need probe-sense | Ordered domain with clear tech contradiction | Hours–days | Sense-making, not solution library |
| **Systems thinking / CLD** | Feedback delays, policy resistance | Need portable “principle catalog” | Weeks | Diagnosis of dynamics |
| **DoE** | Quantify factor effects; optimize known levers | Factors unknown; need conceptual leap | Weeks | Empirical, not inventive |
| **Wardley** | Strategy, evolution of components, build/buy | Concrete mechanical contradiction on floor | Days–weeks | Strategy map |
| **Hypothesis-driven debugging** | Live system, logs, experiments | Stuck in trade-off thinking, no hypothesis space | Hours (if skilled) | **Usually best default for your fleet** |

Google’s public STPA experience (engineer-weeks, hundreds of scenarios pre-outage) is a different evidence class than TRIZ software anecdotes.11(https://www.reddit.com/r/changemyview/comments/v2zci3/cmv_i_disagree_with_chestertons_fence/)

## B5. LLMs applying TRIZ — usable today?

| System | Year | What worked | What failed / limits | Usable by 2-person lab? |
|--------|------|-------------|----------------------|-------------------------|
| **AutoTRIZ** | 2024–2025 | End-to-end report; 7/10 contradiction top-3 overlap textbook; web tool | Hallucinations; subjective eval; mechanical case bias; N=10 cases | **Yes as ideation assistant** (prompts + optional autotriz.ai), not as truth oracle |
| **TRIZ Agents** | 2025 | Multi-agent role split for TRIZ steps; crane case | Research; not ops MTTR | Prototype only |
| **TRIZ-GPT / similar** | 2024 | Prompted contradiction→principles | Quality depends on model; domain mech | DIY prompts today |

**Practical today:** a checklist prompt (“state technical + physical contradiction; IFR; three separations; only principles 1,2,10,13,23,25,27,34”) is **more usable** than running a multi-agent TRIZ research stack. AutoTRIZ authors themselves cite Ilevbare: classical TRIZ is hard to master.

---

# C. Collision with radical simplicity (the section that matters)

## C1. Ideality ≠ AK-47 repairability — exact divergences

| Dimension | TRIZ ideality | AK-47 doctrine | Collision example |
|-----------|---------------|----------------|-------------------|
| Objective | Max useful function / min cost+harm; system may **disappear** | Max **repairability by non-engineer** | “Self-healing multi-agent that rewrites its own prompts” is ideal-ish, unrepairable |
| Elegance | Elegant resolution of contradiction is a **feature** | Elegance is a **smell** if opaque | Clever dual-state browser profile with hot-swap cookies |
| Coupling | Absorb into super-system often “more ideal” | Super-system opacity **hurts** hammer repair | Depend on Chrome sync magic instead of visible files |
| Hidden state | Allowed if function increases | Hidden state is enemy | Adaptive dynamization of cron schedules via learned policy |
| Mechanism count | Can rise if net ideality rises | Mechanism count is primary cost | Feedback + intermediary + prior action stack |

**Concrete clever-but-worse-for-novice TRIZ-ish moves:**

1. **Intermediary + feedback** micro-service mesh for agent bus — higher ideality of decoupling, lower repairability.  
2. **Phase transition** of auth (token states, multi-hop OAuth refresh automata).  
3. **Composite** “smart” watchdog that is also healer, deployer, and reporter — one process, many functions (anti-cohesion; inverse of Bhushan’s structural ideality lesson).  
4. **Self-service** agents that mutate fleet config without human-readable desired-state files.

Bhushan’s own software TRIZ case *aligned* ideality with **simpler structure and half the code** — that is the **good** collision zone.9(https://www.econlib.org/misusing-chestertons-fence/) The bad zone is when ideality is measured as “function magic” not “LOC + cognitive surface.”

## C2. Documented failure mode: over-clever TRIZ?

- **Direct TRIZ literature:** Ilevbare et al. emphasize **complexity of the method** and adoption barriers more than “TRIZ produces over-engineered products.”10(https://www.facebook.com/victoria.haliburton.1/posts/quote-of-the-daygalls-law-is-popular-in-technology-circles-a-complex-system-that/10157445594603455/) **[single-source for method complexity, not product bloat]**  
- **Adjacent literature (stronger):**  
  - **Gall’s Law** (John Gall, *Systemantics*, 1975): *A complex system that works evolved from a simple system that worked; a complex system designed from scratch never works.*2(https://en.wikipedia.org/wiki/John_Gall_(author))  
  - **Worse is Better** (Richard P. Gabriel, ~1989–91): implementation simplicity outcompetes “the Right Thing” elegance.  
  - **Chesterton’s Fence** (G.K. Chesterton, *The Thing*, 1929): do not remove a structure until you know why it exists — temper reckless **trimming**.3(https://fs.blog/chestertons-fence/)  

**Steelmans for “TRIZ over-clever”:** principles like nesting, dynamization, phase transitions, intermediaries, self-service, feedback reward **non-obvious mechanism**. If success metric is patent-like novelty, you select for cleverness. Your metric is hammer-repair → different selection pressure.

## C3. Explicit split of the 40 principles (software/ops, simplicity-first)

### REMOVE complexity (adopt aggressively)

**1 Segmentation · 2 Taking out · 5 Merging (when reduces interfaces) · 6 Universality · 13 The other way round · 16 Partial/excessive · 22 Blessing in disguise · 25 Self-service (only if deletes human toil *and* remains legible) · 26 Copying (shadow/canary) · 27 Cheap short-living · 34 Discarding & recovering · Trimming (method, not numbered principle)**

### ADD complexity / refuse by default (allow only with COMPLICATION flag)

**7 Nested doll · 10 Preliminary action (if multi-stage pipeline) · 11 Beforehand cushioning (if many layers) · 15 Dynamics (adaptive control) · 17 Another dimension · 23 Feedback (if multi-loop) · 24 Intermediary · 28 Mechanics substitution (abstract layers) · 35 Parameter changes (tunable knobs explosion) · 36 Phase transitions · 40 Composite materials (composite architectures)**

### NEUTRAL / context-dependent

**3 Local quality · 4 Asymmetry · 8 Anti-weight (metaphor) · 9 Preliminary anti-action · 12 Equipotentiality · 14 Spheroidality · 18–21 physical · 29–33 materials · 37–39 chemistry**

**Simplicity-first subset (recommended ceiling):**  
**Contradiction statement + IFR + Separation (time/space/condition/system) + Trimming + principles {1,2,6,13,27,34} + optional {23 as output-freshness only, 25 as self-apply only}.**  
**Refuse:** full matrix ritual, Su-Field 76, ARIZ-85C, dynamization/phase/composite as first resort.

## C4. Trimming / increasing ideality ≈ AK-47? Correct reading?

**Mostly correct, with two traps.**

- **Correct:** trimming and ideality push “function without parts,” which often *is* simpler. Function analysis asks “can another component already do this?” — AK-47-compatible.  
- **Misreading risk 1:** IFR “system does not exist but function is performed” can mean **outsourcing** to opaque super-systems (cloud, browser, multi-agent) — simple on paper, unrepairable in practice.  
- **Misreading risk 2:** aggressive trimming without Chesterton’s Fence removes **redundancy that was the reliability** (second rail, explicit ACK, dual-send). TRIZ literature celebrates cost reduction; it does not automatically protect **novice operability** or **fail-soft redundancy**.

**When trimming increases fragility:** when removed parts were **independent verification**, **isolation boundaries**, or **human-visible state**. That is a control-structure loss, not a free ideality gain. (Aligns with STPA’s focus on control actions — different method, relevant warning.)

## C5. Adoption cost — is partial TRIZ coherent?

| Path | Hours to usable | Risk |
|------|-----------------|------|
| Full classical + Level 1–2 cert | 40–100+ hours + practice | High; method becomes the job |
| Matrix + 40 memorization | 20–40 hours | Metaphor abuse on software |
| **Minimal: contradiction + IFR + separation + trim** | **4–12 hours** | Dilution — critics call this “TRIZ-lite” |
| SIT/USIT instead | 1–3 days workshop-scale | Different brand, similar goal: fewer tools |

Ilevbare et al.: TRIZ is complex and non-standardized in practice — partial tool use is **how most companies actually use it**, not a heresy.10(https://www.facebook.com/victoria.haliburton.1/posts/quote-of-the-daygalls-law-is-popular-in-technology-circles-a-complex-system-that/10157445594603455/) SIT exists *because* full TRIZ was too heavy.

**Verdict:** Partial TRIZ is **coherent** and is what industry mostly does. “TRIZ-lite does nothing” is true only if you use principles as **after-the-fact labels**. If you use contradiction+IFR **before** designing the fix, it changes behavior.

---

# D. What you should actually do

## D1. Named options

### Option 0 — **Do not adopt TRIZ**  
**Use instead:** hypothesis-driven debugging + **output-freshness watchdogs** + **desired-state deploy** + STPA-lite (control loops) for safety-critical rails + TOC for “what is the bottleneck this week.”  
- **Learn cost:** 0 TRIZ hours; deepen existing practices 5–10 h.  
- **Daily change:** none from TRIZ.  
- **30-day measure:** recurring incident classes (auth loop, silent no-op, unapplied deploy) count vs baseline.  
- **Abandon:** N/A.  
- **When right:** if failures are measurement/visibility, not true inventiveness.

### Option 1 — **“Contradiction Card” only (recommended)**  
Minimal ritual (below). No matrix, no ARIZ, no cert.  
- **Learn cost:** **6–10 hours** (one worked example per failure class).  
- **Daily change:** before any permanent fix, fill ½-page card; refuse fix that doesn’t state what got worse.  
- **30-day measure:** (a) % of “permanent” fixes that still have a named contradiction; (b) re-open rate of same class within 14 days; (c) count of mechanisms **removed** (trim wins).  
- **Abandon if:** after 10 cards, no fix differed from what you’d do anyway *and* reopen rate flat.

### Option 2 — **SIT/ASIT lite instead of TRIZ**  
Closed World + Qualitative Change + few tools.  
- **Learn cost:** 1–2 days.  
- **Better fit for non-engineer** than classical TRIZ.  
- **Measure:** same as Option 1.  
- **Risk:** another vocabulary.

### Option 3 — **Full TRIZ / MATRIZ path**  
- **Learn cost:** 40–100+ h + tooling.  
- **Reject for two-person non-engineer lab** unless you productize invention for hardware clients.  
- **Abandon early if:** first month produces more documents than deleted code.

## D2. Minimal ritual (≤1 page) + demo on Failure 4

### Ritual — “Contradiction Card” (non-engineer language)

1. **What broke?** (one sentence, observable)  
2. **What are we trying to improve?**  
3. **What gets worse when we improve that?** (if you cannot name it, you are guessing)  
4. **Same thing must be both X and not-X?** Write the pair.  
5. **Ideal:** “How would the problem disappear if we added **no new permanent robot**?”  
6. **Separate:** time / place / condition / whole-vs-part — pick one.  
7. **Trim:** which existing part can die if another already does the job?  
8. **Only then design.** Max **one** new mechanism. Flag COMPLICATION if more.  
9. **Proof:** what fresh **output** shows health? What proves the fix on **all** machines?

### Demo — Indicator quiet vs indicator dead

1. Broke: “Green dashboard, dead pipeline.”  
2. Improve: quiet when OK.  
3. Worsens: cannot tell if watcher died.  
4. Physical: must be silent **and** must prove life.  
5. IFR: no watcher; **fresh work product** is status.  
6. Separate **in time**: daily “last success at T” heartbeat ≠ fault alert.  
7. Trim: delete process-level “running?” checks that shared fate with job.  
8. Design: external `output_freshness` on consumer artifact; alarm on staleness.  
9. Proof: force kill producer → alarm; force kill freshness job → separate alarm.

**This is textbook reliability; TRIZ only forced the IFR sentence.** That is enough value if it stops symptom patches.

## D3. When TRIZ is the **wrong** choice + strongest argument against this recommendation

**Wrong if:**

- Your bottleneck is **observation** (no logs, no output age, no deploy verify) — buy visibility, not inventiveness.  
- Owner cannot spend even 6 hours on a new vocabulary.  
- Team starts **certifying** instead of **deleting** mechanisms.  
- Problems are chaos-domain (Cynefin) requiring probes, not contradiction matrices.

**Strongest argument against recommending even Contradiction Cards:**  
For your four examples, TRIZ **did not produce non-obvious designs** — it **relabeled** SRE orthodoxy (independent monitor, desired state, dead-man switch, session plane). Paying any method tax risks **false sophistication** while the real fix is **instrumentation + simpler architecture**. That argument is **strong**. The counter: the card’s only job is to stop **silent trade-off denial** (“we’ll just add another retry”) — a cheap cognitive seatbelt, not a magic method.

---

# Decision-oriented ending

## Recommendation

**Adopt Option 1 — Contradiction Card (IFR + physical contradiction + separation + trim). Refuse matrix, Su-Field, ARIZ, MATRIZ, and “40 principles as idea lottery.”**  

- **Confidence:** **[emerging]** for partial TRIZ value; **[established]** that full TRIZ is costly and weakly evidenced for software reliability.  
- **Main assumption:** your recurring failures are partly **unacknowledged trade-offs** (not only missing metrics). If they are only missing metrics, Option 0 wins.  
- **30-day kill criteria:** zero reduction in reopened failure classes **and** zero net mechanism removal → drop TRIZ vocabulary entirely; keep only output-freshness + desired-state deploy as hard engineering rules.

## One-line steelman of the opposite view

“TRIZ is a 1960s patent-mining heuristic with a consultancy mythology, a certification industry, and almost no RCTs in software ops; a non-engineer lab should learn STPA-lite and good SRE, not Russian inventive principles.” — **This is a fully defensible position.** Partial TRIZ is recommended only as **cheap structured honesty about trade-offs**, not as a belief system.

---

# Source list (working URLs)

1. https://en.wikipedia.org/wiki/TRIZ  
2. https://www.ee.iitb.ac.in/~apte/CV_PRA_TRIZ_INTRO.htm  
3. https://www.triz.co.uk/what-is-triz  
4. https://the-trizjournal.com/case-study-use-of-triz-in-software-design/ (Bhushan 2008)  
5. https://www.forbes.com/sites/haydnshaughnessy/2013/03/07/why-is-samsung-such-an-innovative-company/  
6. https://www.sciencedirect.com/science/article/abs/pii/S0166497212001356 (Ilevbare et al. 2013 abstract)  
7. https://www.rndtoday.co.uk/paper/a-review-of-triz-its-benefits-and-challenges-in-practice/  
8. https://leanmethods.com/articles/split-your-innovation-problem-four-ways-separation-principles/  
9. https://wiki.matriz.org/docs/triz/problem-solving-tools-5890/substance-field-modeling/standard-inventive-solutions/  
10. https://matriz.org/certification/  
11. https://www.matriz-official.net/index.php/certification  
12. https://en.wikipedia.org/wiki/Systematic_inventive_thinking  
13. https://en.wikipedia.org/wiki/Unified_structured_inventive_thinking  
14. https://www.triz40.com/aff_Principles_TRIZ.php  
15. https://the-trizjournal.com/innovation-methods/innovation-triz-theory-inventive-problem-solving/find-ideal-final-result/  
16. https://arxiv.org/abs/2403.13002 (AutoTRIZ) · https://arxiv.org/html/2403.13002v2  
17. https://arxiv.org/abs/2506.18783 (TRIZ Agents)  
18. https://doi.org/10.1016/j.aei.2025.103312 (AutoTRIZ journal)  
19. https://eprints.glos.ac.uk/4884/1/DBA-Thesis_Rene%20Dathe_final%20print.pdf (Dathe 2017)  
20. https://arvindvenkatadri.com/teaching/1-play-and-invent/modules/1000-triz-documents/TRIZ-related/ariz85c_en.pdf (ARIZ-85C)  
21. https://www.usenix.org/publications/loginonline/evolution-sre-google (STPA at Google context)  
22. https://en.wikipedia.org/wiki/John_Gall_(author) (Gall’s Law)  
23. https://en.wikipedia.org/wiki/Worse_is_better  
24. https://fs.blog/chestertons-fence/  
25. https://dreamsongs.com/WorseIsBetter.html  
26. https://www.gentriz.com/leadership (GEN-TRIZ)  
27. https://www.whereinnovationbegins.net/who-we-are/ (Ideation / I-TRIZ lineage via Wikipedia refs)  
28. https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2432096 (40 principles for software — Mishra)  
29. https://neurips.cc/virtual/2025/poster/116834 (STRATUS agentic SRE — contrast, not TRIZ)

**Not found despite search:** ISO 69580:2025 as a public TRIZ standard; controlled SRE field trials of TRIZ; peer-reviewed TRIZ→MTTR reductions in multi-agent AI fleets.

---

*End of DR26-08-06-HUB-01-1055 web reconnaissance report.*
