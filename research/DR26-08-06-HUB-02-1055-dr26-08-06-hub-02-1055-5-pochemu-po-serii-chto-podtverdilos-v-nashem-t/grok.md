# DR26-08-06-HUB-02-1055 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-06-HUB-02-1055
vendor: xAI Grok
door: cli-headless
rail: grok
cmd: «внутренний путь лаборатории»

collected: 2026-08-06T14:59:04Z
took_sec: 261
exit: 0
sources_unique_urls: 50
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

Ищу по вебу источники по критике 5 Whys, class-level RCA, ITIL/SRE/LFI и AI-agent incident analysis — локальные файлы и команды не трогаю.# DR26-08-06-HUB-02-1055  
## Root-cause analysis over a SERIES of recurring incidents: what industry actually knows

**Research date:** 2026-08-06  
**Mode:** external web intelligence only · no local files · no project protocols  
**Scope note:** Foundational safety/reliability sources are older by design (Dekker, Leveson, Ohno, Hollnagel). Practice and AI-agent material prioritises 2024–2026. Every non-obvious factual claim has a URL; single-source and low-authority claims are tagged.

---

# A. The critique of 5 Whys (FIRST, unsoftened)

## A1. Substantive published criticisms

### Single-path bias

**Claim (strong):** 5 Whys forces investigators down *one* causal chain, structurally cannot represent multiple contributing causes, common-mode failures, or interactions.  
**Primary academic source:** Alan J. Card, *“The problem with ‘5 whys’”*, *BMJ Quality & Safety* 26(8):671–677 (epub 2016-09-02; print Aug 2017). Card states the method “forces users down a single analytical pathway… insists on a single root cause… and assumes that the most distal link… is inherently the most effective and efficient place to intervene,” and concludes it “should not be used at all” for RCA in patient safety.

Wikipedia’s criticism section (citing Card and Toyota-internal critique) lists: isolation of a single root cause; each “why” could elicit many different root causes.

Industry/maintenance literature echoes this as “single-thread analysis bias”: most equipment failures are multi-factor; linear chains miss interactions.

**How badly it distorts:** Card’s argument is conceptual + practice-based, not a controlled trial measuring distortion magnitude. No high-quality RCT of “5 Whys vs multi-cause method → wrong fix rate” was found. Distortion is argued as *structural* (the method cannot encode multi-cause systems), not as a measured error rate. **[established as critique of form; speculative as quantitative distortion magnitude]**

### Stopping-point arbitrariness

**Why five?** Ohno/TPS used “five” as a *pedagogical average*, not a scientific stopping rule. Wikipedia (citing Ohno 1988): number of whys “may be higher or lower depending on complexity.”

Card: arbitrary depth of the fifth why is “unlikely to correlate with the root cause.”

**Where people actually stop:** documented failure modes include stopping at symptoms (1–2 whys), stopping at “human error,” stopping at answers that are organizationally comfortable (“not my team,” training, “be more careful”). These are repeatedly described in practice literature and Card; controlled measurement of political stopping points is thin. **[established as known failure mode of practice; emerging as quantified political bias]**

### Investigator-dependence / non-reproducibility

Wikipedia (citing criticism tradition including Minoura/Toyota purchasing director): “Results are not repeatable – different people using five whys come up with different causes for the same problem.” Also: inability to go beyond the investigator’s current knowledge; no support for generating the “right” answer to why.

This is **widely asserted** in quality/safety literature but **not backed by large multi-rater reliability studies specifically on 5 Whys** in the sources found. Related evidence: accident-analysis methods (AcciMap, STAMP-CAST, etc.) *have* been tested for inter-rater reliability and show substantial variation (e.g. Hulme et al. 2024 comparison; Goncalves Filho et al. 2019 on Accimap/STAMP). That does not automatically transfer to 5 Whys, but it supports the general claim that causal attribution in complex systems is investigator-dependent. **[established as criticism; emerging as measured non-reproducibility for 5 Whys specifically]**

### “Human error” as terminal answer — safety science

Sidney Dekker’s *Field Guide to Understanding ‘Human Error’* (editions from ~2002; 3rd ed. Routledge) is the canonical source for the “new view”: finding “human error” is where investigation **starts**, not stops; “human error” is an organizational/system product; root cause is “simply the place where you stop looking”; there is **no single cause**—multiple factors are each necessary and only jointly sufficient.

Dekker’s formulation (from the Field Guide PDF material): “What you call ‘root cause’ is simply the place where you stop looking any further… there are no such things as root or primary causes… If you find a root or primary cause, it was your decision.”

David Woods, Erik Hollnagel, Richard Cook and the resilience-engineering lineage (Hollnagel, Woods & Leveson, *Resilience Engineering: Concepts and Precepts*, 2006) reject linear “bad apple” models and treat adverse outcomes as emergent from normal performance under conflicting goals.

**[established]** in human-factors / safety science; **[contested]** by classical RCA vendors who still treat “human error” as a legitimate terminal category when accompanied by training/discipline actions.

### Safety-II / resilience engineering and STAMP (Leveson): no single root cause

**STAMP (Systems-Theoretic Accident Model and Processes), Nancy Leveson (MIT):**  
Accidents are **not** chains of component failures; they result from **inadequate control** of safety constraints; safety is a control problem; causes include unsafe interactions among components that may *not* have failed.

Leveson explicitly rejects single root-cause thinking: “Instead of viewing accidents as the result of an initiating (root cause) event in a series of events leading to a loss, accidents are viewed as resulting from interaction among components that violate the system safety constraints.”

**CAST** (Causal Analysis based on STAMP) is the retrospective method: examines the entire safety control structure, not a single cause.

**Safety-II (Hollnagel):** Safety is ensuring as many things as possible go *right*, not only preventing things going wrong; investigations should understand everyday work variability, not hunt a root. Hollnagel has said he would be “delighted if Root Cause Analysis would disappear” because its simplicity is “too attractive to be overcome by sound arguments.” [single-source / secondary quote via Quentic summary of Hollnagel] Primary white paper: *From Safety-I to Safety-II* (Hollnagel, Wears, Braithwaite; NHS Sign up to Safety / related versions ~2015).

**This is a live disagreement, not a resolved scientific consensus.**  
- **Classic RCA / FRACAS / 8D / ITIL Problem Management** still assume identifiable underlying causes worth eliminating to prevent recurrence.  
- **Safety-II / STAMP / LFI (Allspaw lineage)** argue linear root-cause is the *wrong model* for complex socio-technical systems.  
Presenting either as “debunked” would be false. Presenting either as universally true would be false.

### Toyota / Ohno — folklore vs primary source

**Primary:** Taiichi Ohno, *Toyota Production System: Beyond Large-Scale Production* (1988 English): “The basis of Toyota’s scientific approach is to ask why five times whenever we find a problem… By repeating why five times, the nature of the problem as well as its solution becomes clear.” Classic fuse/bearing/lubrication pump/shaft/strainer example (Ohno p.17 cited via Lean Enterprise Institute).

**Folklore corrections:**
1. **5 is not sacred.** Number is average/pedagogy; practitioners and Toyota-adjacent writers say 3–8 is fine.  
2. **Originally not pure “RCA for complex socio-technical systems.”** Wikipedia notes five whys were initially developed in a product/feature/manufacturing context and that others at Toyota (e.g. Minoura) criticized it as too basic for deep root-cause work.  
3. **Toyota’s real practice is broader:** A3 problem-solving, go-and-see (*genchi genbutsu*), multi-cause tools, kaizen culture — not internet-meme “ask why five times and you’re done.” Credible secondary reporting (Liker *Toyota Way* tradition; Lean Enterprise Institute) places 5 Whys *inside* a richer system.  
4. **Card’s historical claim (stronger):** 5 Whys as popularized may be a “misguided reuse” of a feature-understanding tool as RCA. [single-source as Card’s historical reading]

**Net on folklore:** The internet sells 5 Whys as *the* Toyota root-cause method. Primary Ohno text *does* endorse it as foundational TPS “scientific approach.” Internal and external critics *also* treat it as too shallow for complex systems. Both statements are true simultaneously. **[established]**

## A2. Counter-arguments (steelmanned)

| Defence | Steelman | Source quality |
|--------|----------|----------------|
| **Communication / teaching tool, not investigation method** | Card and AHRQ PSNet note value as *teaching* tool while rejecting it as RCA for complex care. It trains people to dig past the first symptom. | Strong (Card + PSNet) |
| **Cheap and beats nothing** | For a shop-floor jam or a one-machine fuse blow (Ohno’s example), five minutes of why-chain beats “just replace the fuse.” Opportunity cost of CAST/fishbone for a $20 part is absurd. | Practice consensus; weak RCTs |
| **Works for simple linear mechanical failures** | When causality *is* approximately linear, single-path, and local, 5 Whys is approximately correct. Failures of multi-factor software/fleet systems are exactly the domain where the method’s assumptions break. | Established in lean literature |
| **Focus forces action (vs analysis paralysis)** | FlowFuse-style defence: multi-cause maps produce 17 causes and zero fixes; 5 Whys forces one path to completion. | [low-authority] vendor/blog |

## A3. Net verdict for a small technical team debugging recurring distributed software failures

**Verdict: (c) actively misleading as primary method; (b) usable only with named modifications as a *probe*, not a verdict engine.**  
**Confidence: [established] for complex socio-technical/software; [speculative] that small teams will actually sustain heavier methods.**

**Named modifications if kept as a probe:**
1. Never stop at “human error / agent mistake / be more careful.”  
2. Force *branching* (cause tree / multi-why), not a single chain.  
3. Every “because” = **proven-with-evidence** or **hypothesis** (your rule already).  
4. Cap at “class of problem,” not “this session.”  
5. Prefer fishbone / causal map / problem record over pure linear 5 Whys when recurrence is the trigger.

**Replacement candidates (by tier of effort):**
- **Light:** ITIL-style Problem Record + Known Error + workaround + permanent fix gate (see B).  
- **Medium:** SRE class-level error-budget trigger + themed postmortem aggregation.  
- **Heavy (when stakes warrant):** CAST / multi-factor causal map; Safety-II learning reviews (not “root”).

For *your* fleet (browser drift, auth loops, multi-machine Syncthing/bus agents), pure 5 Whys will systematically invent tidy single stories. That is not a skill issue; it is a model mismatch.

---

# B. Root-cause work over a SERIES / CLASS (core)

## B1. Established industry names and disciplines

### ITIL Problem Management vs Incident Management

| Concept | Definition | Source |
|---------|------------|--------|
| **Incident** | Unplanned interruption / degradation — restore service fast | IBM Think; ITIL 4 practice material |
| **Problem** | Underlying *cause* of one or more incidents | IT Process Wiki / Freshworks / IBM |
| **Known Error** | Problem with documented root cause + workaround | KEDB |
| **KEDB** | Known Error Database — shared by Incident + Problem | IT Process Wiki |
| **Reactive PM** | After known incidents, find cause, reduce recurrence | Emory IT; Xurrent |
| **Proactive PM** | Trend analysis / monitoring to find problems before major incidents | Emory IT; IBM |

**Is the distinction real in practice or paperwork?**  
- **As design:** Real and useful — separates “restore now” from “eliminate cause.”  
- **As practice:** Frequently paperwork. Many orgs create Problem tickets that are never diagnosed; KEDB becomes a graveyard. Success metrics *should* be reduction in *repeat* incidents, permanent fixes, MTTR-RC — not number of Problem records.  
- **Evidence of effectiveness:** Industry claims (reduced recurrence, ~50% unplanned work cut in case anecdotes) are **vendor-heavy and rarely controlled**. Freshworks cites ~13% of incidents as repeats and long-lived problems — [single-source / marketing-adjacent numbers].  
**Verdict:** Distinction is conceptually correct and matches your “series not case” instinct. Implementation quality varies wildly. **[established as framework; emerging as proven ROI]**

### Google SRE: postmortems, aggregation, error budgets

**Blameless postmortem** (SRE Book Ch.15, Lunney & Lueder): document incident, impact, contributing causes, preventive actions; blameless culture from healthcare/avionics. Triggers: user-visible downtime, data loss, on-call intervention, long resolution, monitoring failure.

**Aggregation / trend analysis:** Google explicitly builds tools “to aggregate postmortems… identify common themes and areas for improvement across product boundaries,” enhanced templates with metadata for automated analysis, ML for predicting weaknesses / reducing duplicate incidents (aspirational). Etsy Morgue as public cousin.

**Repeating incidents (Workbook Ch.10):** If failures *mirror previous* incidents — dig deeper: action items too slow? feature velocity trumping reliability? wrong AIs? Bring postmortem collaborators from *each similar incident* together.

**Error budget as class-level signal (Workbook Appendix B, example policy):**  
- Single incident >20% of 4-week error budget → mandatory postmortem with ≥1 P0 action on root cause.  
- **Single class of outage >20% of error budget over a quarter → P0 item on quarterly plan.**  
This is the closest high-status industry analogue to your “class-level dedicated work.”

**[established]** as Google’s published practice; external replication varies.

### Learning From Incidents (LFI) — Allspaw / Adaptive Capacity Labs / Jeli / Verica lineage

John Allspaw (ex-Etsy CTO; Adaptive Capacity Labs):  
- Learning ≠ fixing.  
- Most post-incident docs written to be *filed*, not *read*.  
- **Frequency of incidents has nothing to do with how well an organization learns.**  
- Explicit goal to “reduce incident count” can *reduce reporting*, not actual failures.  
- Paradoxes of LFI: messy, social, not a root-cause factory.

2025 Adaptive Capacity Labs piece: “An absence of incidents is not evidence that learning is happening.”

**Scepticism about counting:** Central. If your trigger is purely “N recurrences,” LFI will say: beware that you will over-value countable surface events and under-value near-misses, adaptive success, and deep one-offs. **Disagreement with pure FRACAS/ITIL counting is real and unresolved.**

### Reliability engineering classics

| Method | Role for *series* | Notes |
|--------|-------------------|-------|
| **Pareto analysis** | Rank failure modes by frequency/impact; fix vital few | Standard FRACAS practice |
| **Weibull / failure trending** | Characterize time-to-failure distributions for recurring fault modes | Needs volume of data |
| **FRACAS** | Closed-loop Failure Reporting, Analysis, Corrective Action | Military/aerospace heritage; closed loop is the point |
| **8D** (Ford, 1987) | 8 Disciplines: team, problem, interim containment, root cause, permanent corrective action, prevent recurrence, recognize team | Explicitly recurrence-focused |
| **FMEA / FTA** | Forward-looking class risk; complement to retrospective RCA | Not substitutes for investigation |

### Aviation / medicine aggregation

**ASRS (NASA Aviation Safety Reporting System):** Confidential voluntary reports → de-identified database → trend/hazard identification across the NAS; goal is connecting dots between *seemingly unrelated* reports before accidents. ASRS itself cautions against naive distributional use (reporting bias).

**NTSB:** Accident investigation → safety recommendations that often address *systemic* issues spanning multiple accidents (not one flight).  

**Medicine:** RCA mandated for serious events, but Card and Safety-II literature heavily criticize shallow RCA/5 Whys in hospitals; ASRS-like voluntary reporting systems exist with similar aggregation goals and similar biases.

## B2. Concrete formats that work

| Format | Structure | Who / cadence | Evidence it works |
|--------|-----------|---------------|-------------------|
| **Blameless postmortem (single)** | Timeline, impact, contributing causes, lessons, AIs with owner/priority/bug | After significant incident; review within days | Google claims fewer outages / better UX with continuous investment — observational, not RCT |
| **Periodic incident REVIEW (window)** | Theme extraction across N postmortems; metadata tags; “greatest hits” | Monthly/quarterly; SRE working groups; Postmortem Reading Clubs | Google aggregation tooling; industry practice |
| **Problem / Known Error records** | Problem → diagnosis → workaround → KE → permanent fix | Continuous; PM owns | ITIL doctrine; weak independent outcome studies |
| **CAST on a class** | Safety control structure map; inadequate control actions across components for *pattern* of losses | Expert facilitator; high effort | Case applications (e.g. mine disasters, Walkerton water); reliability studies of CAST vs other methods exist but CAST is heavy |
| **FTA / FMEA class-level** | Top event tree / failure mode scoring | Design or reliability eng | Forward prevention; not recurrence analysis per se |
| **Ishikawa / fishbone** | Categories of causes (people/process/tools/env…) | Workshop | Multi-cause; still can be theatre without evidence |
| **Current Reality Trees (TOC)** | Logical cause-effect map with “entities” and necessary conditions | TOC practitioners | Good multi-cause; niche skill |
| **Theme extraction / taxonomy tagging** | Code postmortems; tags for component, failure mode, detection lag | Analyst + tooling | Enables Pareto + Google-style aggregation; validity depends on taxonomy (B3) |
| **Retro-of-retros / quarterly reliability review** | Meta-review of action-item closure, repeat rates, systemic themes | Quarterly planning | Google error-budget *class* rule forces this into planning |

## B3. CRITICAL: How do you know the series is ONE class?

### What literature says about taxonomy validity

- Incident taxonomies (ENISA CSIRT taxonomy, HFACS, custom SRE tags) exist; **inter-rater reliability is often moderate unless heavily trained.**  
- Goncalves Filho et al. / Hulme et al.: different accident analysis methods and raters produce different causal structures.  
- ASRS: good for hazard *identification*, not naive rate trends (reporting bias).  
- AI incident classification work (2024+) stresses validation samples against human analysis.

### Failure mode you fear (spurious clustering by surface)

“All browser problems” is exactly the **surface-symptom cluster**. Literature equivalent: grouping all “human error” or all “communication failures” — labels that *feel* unified but hide different mechanisms.

### Operational test (not a warning): **common-cause test battery**

Use **all five**; fail any → split the class or demote to “theme, not cause.”

1. **Shared necessary condition (mechanism test)**  
   Can you name a *single mechanism* such that if removed, *all three cases would have been prevented*?  
   - Pass example: “shared cookie store corrupted by profile migration script X.”  
   - Fail example: “browsers break” (Chrome OAuth vs Firefox profile vs CAPTCHA vs vendor ban).

2. **Counterfactual / repair test (AgentFail-style)**  
   Apply the *same* permanent fix to a representative subset. If only 1 of 3 would be fixed, not one class.  
   Academic parallel: AgentFail used counterfactual repair to validate annotations (diagonal dominance of repair rates).

3. **Inter-rater dual coding**  
   Two people (or human + independent model with different prompt) tag: system, detection, proximate failure, hypothesized mechanism. Cohen’s κ on mechanism code.  
   - κ ≥ ~0.6: usable class  
   - κ < 0.4: re-slice  
   MAST multi-agent failure taxonomy reported κ=0.88 with expert annotation — high bar, but shows the standard.

4. **Discriminating feature table**  
   Build a case×feature matrix (auth state, machine, browser, vendor, time-of-day, recent deploy, profile path, Syncthing lag…).  
   Real common cause → shared cells in mechanism-relevant columns.  
   Spurious class → only shared *surface* columns (e.g. “browser involved = yes”).

5. **Near-miss / success contrast**  
   Find cases with *same surface* that *succeeded*. What differed?  
   If “succeeded” and “failed” share your supposed root, your root is wrong (or incomplete).

**Hard rule:** Class membership is defined by **shared preventable mechanism**, not shared symptom vocabulary.

## B4. Minimum series size — is three defensible?

| Tradition | Guidance | Implication for “3” |
|-----------|----------|---------------------|
| **ITIL** | Problem can be raised from *one* major incident or multiple related | Three is more than required |
| **Google error budget** | *One* large incident can force postmortem; *class* uses budget share over quarter, not event count | Count of 3 is arbitrary vs impact |
| **Pareto / FRACAS** | Prioritize by frequency×impact; small N is fine for high-impact modes | Three identical high-cost failures is enough to act |
| **SPC** | Control charts need ~20–30 samples to stabilize limits; n=3–5 common *subgroups* for means | Three events is **too few for statistical process claims**; fine for special-cause investigation |
| **Safety science** | One serious event can justify deep analysis (aviation, medicine) | Severity can trump N |
| **LFI (Allspaw)** | Count is a bad primary metric | Three as trigger is crude |

**Verdict on your “minimum three”:**  
- **Defensible as a *heuristic gate against over-triggering* on one-offs.**  
- **Arbitrary as science.**  
- **Wrong if it blocks analysis of one high-severity or high-budget-burn event.**  
- **Wrong if it *forces* analysis of three weakly related surface matches.**

**Recommended hybrid trigger (evidence-aligned):**  
Open class-level analysis when **any** of:
- ≥3 cases with **mechanism-plausible** similarity (pass B3 tests), **or**  
- 1 case burns disproportionate “budget” (time, money, auth lockouts, multi-machine cascade), **or**  
- 2 cases after a permanent-fix supposedly landed (recurrence after fix = class signal).

**[speculative] as exact numbers; [established] that pure count without mechanism is weak.**

## B5. Does class-level analysis reduce recurrence more than per-incident?

**Honest answer: evidence is thin and mostly observational / vendor / case-study.**

What exists:
- Google: continuous postmortem culture investment associated with fewer outages / better UX — no public controlled experiment.  
- Google satellite/diskerase case: action items from first postmortem *dramatically reduced blast radius and rate* of a similar incident three years later (Workbook case study). That is recurrence *mitigation* from deep single + systemic AIs, not pure multi-case mining.  
- ITIL PM literature: claims reduced repeat incidents as *the* KPI — few independent before/after with controls.  
- FRACAS/8D: industry standard in automotive/aerospace because closed-loop corrective action is contractually required; efficacy assumed more than re-proven in open literature.  
- Safety-II critiques: fixing “roots” from RCA often fails to reduce harm because roots were mis-specified or work-as-done differs from work-as-imagined.

**If the evidence is thin — it is thin.** Do not harden your rule on a false claim that “industry proved class-level analysis works.” Industry *believes* it works when the class is real and the fix is systemic and verified. That belief is rational but under-measured.

---

# C. Adaptation to AI-agent / LLM operations (2025–2026)

## C1. Is anyone doing incident analysis for AI-agent systems?

**Field is no longer empty (2025–2026), but still early.** **[emerging]**

### Real taxonomies / papers

| Work | What it covers | URL / id |
|------|----------------|----------|
| **MAST** — *Why Do Multi-Agent LLM Systems Fail?* (Cemri et al.; NeurIPS 2025 poster lineage; arXiv 2503.13657) | 14 failure modes in 3 categories: (i) system design issues, (ii) inter-agent misalignment, (iii) task verification; 150 traces; κ=0.88; LLM-as-Judge annotation | https://github.com/multi-agent-systems-failure-taxonomy/MAST · https://arxiv.org/abs/2503.13657 |
| **AgentFail** — Ma et al. 2025 (platform-orchestrated agents, Dify/Coze) | 307 failure logs; taxonomy: **agent-level** (tool planning, format, content deviation, knowledge/reasoning, prompt, encoding, tool/KB), **workflow-level** (input validation, node deps, loops/deadlock, conditional logic, task decomposition, context conflict, interface mismatch), **platform-level** (network/resource, service unavailability); auto-RCA max accuracy **33.6%** even *with* taxonomy | https://arxiv.org/html/2509.23735v1 |
| **Who&When / automated failure attribution** (Zhang et al.) | Who failed and at which step | arXiv 2505.00212 |
| **AgenTracer** | Which agent induces failure | arXiv 2509.03312 |
| **Partnership on AI** — real-time failure detection | Live monitoring of planning/tool/execution hazards; surveys Microsoft/OWASP agent threat models | https://partnershiponai.org/wp-content/uploads/2025/09/agents-real-time-failure-detection.pdf |
| **Taxonomy of failures in tool-augmented LLMs** | Structured TaLLM failures | IEEE AST 2025 |

### Mapping to your failure language

| Your phrase | Closest published buckets |
|-------------|---------------------------|
| Silent no-op | Verification failures (MAST); suboptimal quality without termination (AgentFail) |
| Tool-call failure | F1.1 / F1.7 AgentFail; tool-augmented LLM taxonomies |
| Context loss | Context conflict F2.6; inter-agent misalignment |
| Hallucinated success | Task verification failures; response content deviation F1.3 |
| Stale credentials | Platform/auth (under-specified in pure MAS papers — more ops/SRE) |
| Green indicator, dead pipeline | Monitoring failure / false health (classic SRE; not fully agent-taxonomy-native) |

**Gap:** Long-running *coding-agent fleets*, *browser automation against LLM vendors*, *multi-machine Syncthing + bus* ops are **under-covered** relative to Dify/Coze-style orchestrated workflows and academic multi-agent benchmarks. Treat published taxonomies as **starter codes**, not complete for your lab.

## C2. Can an LLM agent credibly run this analysis on itself?

### What has been tried

| Approach | Measured results | Source |
|----------|------------------|--------|
| **OpenRCA** (Xu et al., ICLR 2025) | Even Claude 3.5 + RCA-agent: **~11.34%** full solve rate on realistic software failure cases | https://openreview.net/forum?id=M4qNIzQYpd · PDF summaries |
| **AgentFail auto-diagnosis** | Peak **33.6%** root-cause ID *with* taxonomy guidance; ~8–13% without | arXiv html 2509.23735 |
| **LLM RCA reasoning failures** (Riddell et al. / Waterloo 2026) | 48k scenarios; taxonomy of **16 reasoning failure types**; accuracy remains low; agentic complexity can *compound* errors | https://arxiv.org/abs/2601.22208 |
| **RCAgent, Flow-of-Action, etc.** | Tool-augmented agents improve vs ReAct on industrial RCA metrics — still not “solved” | various arXiv 2024–2025 |

### Specific failure modes of LLM self-RCA

From OpenRCA / reasoning-failure work / general LLM critique:
1. **Confabulating tidy causal stories** from incomplete logs.  
2. **Stopping at convenient causes** (especially ones that blame tools/users rather than the model’s own prior actions).  
3. **Agreeableness / sycophancy** toward the narrative the prompt suggests.  
4. **Multi-hop reasoning collapse** as number of required root elements increases.  
5. **Late-stage error fixation** — flags proximate failure, misses earliest decisive error (AgentFail notes this).  
6. **Self-exculpation** when analyzing own traces (not strongly quantified in papers; **[speculative]** but consistent with sycophancy literature).

**Net:** LLM can be a **draft analyst**, not a **trusted adjudicator**. Measured accuracy is far below “ship the fix based on LLM alone.”

## C3. Safeguards for trustworthy self-analysis

Your existing rules (because = proven or hypothesis; stricter scrutiny on convenient conclusions) are **aligned** with:
- Blameless-but-evidence-based SRE postmortems  
- Counterfactual validation (AgentFail)  
- Safety science’s rejection of untested stories  

**Add:**

1. **External eyes gate** — second model *or* human for any class fix that changes fleet behavior. Heterogeneous review beats same-model self-grade (general reliability practice; Google multi-reviewer culture).  
2. **Evidence appendix mandatory** — logs, timestamps, machine IDs, before/after commands; no evidence → max label = hypothesis.  
3. **Counterfactual / canary fix** — apply class fix on one machine first; verify non-recurrence *and* no new failure.  
4. **Adversarial prompt / red team** — force generation of *alternative* mechanisms; require why they were rejected.  
5. **Do not score self on eloquence** — score on: prediction of next recurrence window, and whether the gate catches re-run.  
6. **Separate roles** — Analyst agent ≠ Fixer agent ≠ Verifier agent (MAST/AgentFail multi-role insight).  
7. **Recurrence metric with reporting bias awareness** — LFI warning: “zero incidents” after a blameful process may mean silence.

---

# D. What you should actually do

## D1. Critique of YOUR rule (point by point, not polite)

| # | Your rule | Verdict | Why |
|---|-----------|---------|-----|
| **1** | Unit of analysis = series, not incident | **Supported** by ITIL Problem Management, Google class-of-outage budget policy, FRACAS trending, ASRS-style aggregation | Best idea in your pack. Matches industry’s actual “stop fighting fires, open a problem.” |
| **2** | Minimum three cases | **Arbitrary; partially contradicted** | Three is a fine *anti-noise* heuristic, not evidence-based. Severity/budget can require N=1. Mechanism-weak “3” is worse than strong “1.” SPC says 3 is not a sample for process claims. |
| **3** | Trigger = recurrence, not severity | **Half-wrong** | Recurrence is a valid trigger. **Excluding severity is not industry practice** (SRE, aviation, ITIL major incident). Hybrid trigger is better. |
| **4** | Fix in separate dedicated session, not inline | **Supported for class work; over-strict if absolute** | Matches Problem Management vs Incident Management separation. Google: restore first, postmortem after. Absolute ban on inline *containment* would be harmful — use interim containment (8D) *plus* separate permanent fix session. |
| **5** | Each “because” = proven or hypothesis | **Strongly supported** | Aligns with scientific method, SRE data-driven postmortems, AgentFail counterfactual ethos. Best epistemic control you have. Keep. |
| **6** | Fix must close the CLASS via gate/test/rule | **Supported** | Matches 8D permanent corrective action + prevent recurrence; Google “at least one P0”; Ben Treynor: postmortem without action ≈ no postmortem. |
| **7** | Propagate fix to all machines | **Supported for true common cause; dangerous if class is fake** | Fleet parity is correct *after* B3 tests pass. Propagating a wrong root multiplies damage (classic common-mode failure). Canary → verify → fleet. |

**Overall character of your rule:**  
Better than pure 5 Whys. Closer to **ITIL Problem Management + SRE class budget + claim hygiene** than to Safety-II (you still seek a “root”). Main naiveties: **N=3 fetish**, **severity blindness**, **no formal class-validity test**, **implicit single-root frame** in multi-cause systems, **under-specified verification after fleet roll**.

## D2. Recommended written procedure (one page)

### Name: **Class Problem Review (CPR)**  
*For non-engineers and LLM agents. Executable checklist.*

#### When to open a Class Problem Record (trigger — any one)
- [ ] **R3:** ≥3 incidents with *mechanism-plausible* similarity in 30 days, **or**  
- [ ] **S1:** 1 incident with multi-machine cascade / auth lockout / >4h human time / data loss, **or**  
- [ ] **F1:** Same failure after a “permanent” fix already shipped.

#### Stop condition (when NOT to open)
- Only surface similarity (“browser”) and B3 tests fail → tag as **Theme**, not Problem. Continue per-incident containment only.

#### Steps (order fixed)

**0. Contain (incident lane — may be inline)**  
Restore service. Log workaround. Do **not** claim class root yet.

**1. Open Problem Record** (one document)  
Record: title · date opened · owner (one human or one named agent) · list of case IDs with dates/machines/logs links · impact (time, machines, money).

**2. Build case×feature matrix**  
Rows = cases. Columns = machine · browser/vendor · auth state · last change · profile path · sync lag · error string · detection method · recovery method.  
Fill only from evidence.

**3. Class validity tests (must pass ≥4 of 5)**  
1. Shared mechanism hypothesis stated in one sentence.  
2. Counterfactual: would *this* fix have prevented *each* case? (yes/no/unknown per case)  
3. Dual coding of mechanism by second reviewer (human or different model).  
4. Discriminating features: ≥1 non-surface feature shared.  
5. Success contrast: found a near-success that differs on the hypothesized mechanism.  
**Fail → split or Theme. Do not invent root.**

**4. Causal map (not linear 5 Whys)**  
List **contributing factors** in three layers:  
- Proximate (what broke)  
- Enabling (why detection/recovery failed)  
- Structural (why the system allowed this class)  
Each factor tagged **Proven** (evidence link) or **Hypothesis**.  
Ban terminal answers: “human error,” “agent dumb,” “be careful.”

**5. Known Error + Workaround**  
If permanent fix not immediate: document Known Error + operator workaround. Share to all machines’ runbooks.

**6. Permanent corrective action (dedicated session)**  
One primary fix that attacks shared mechanism. Must include:  
- **Gate** (code/test/watchdog/rule that fails closed)  
- **Canary** on 1 machine, 24–72h observation  
- **Verify** predicted non-recurrence metric  
- **Fleet propagate** only after canary green  
- **Regression test** that re-runs the original failing scenario class

**7. Close criteria**  
Close Problem only when:  
- Canary + fleet verify green, **and**  
- 14 days (or N=1 full recurrence window, whichever longer) with no same-mechanism case, **and**  
- Action items closed or explicitly accepted risk with owner.

**8. Meta**  
Quarterly: list open Problems, repeat-incident rate, action-item age. If same class reopens → escalate method (deeper CAST-style control map).

## D3. Failure modes of THIS procedure + cheapest detection

| Failure mode | How it fakes roots | Cheapest detection |
|--------------|--------------------|--------------------|
| Spurious class | Groups by symptom | B3 matrix empty on non-surface features |
| Confirmation bias | Only collects supporting logs | Require alternative mechanisms written down |
| Hypothesis laundering | Labels “proven” without evidence | Spot-check: every Proven must have URL/log path |
| Canary theatre | Canary too short / wrong machine | Require canary on *worst* machine type |
| Fix the detector, not the fault | Alerts stop; failure continues | Measure user/outcome metric, not only alert count (LFI) |
| LLM confabulation | Beautiful narrative | Dual-review + OpenRCA-level distrust (expect <40% accuracy) |
| Fleet amplify | Wrong root everywhere | Canary mandatory; rollback plan written *before* fleet |
| Paperwork without learning | Problem records pile up | KPI: **repeat rate of same mechanism**, not # of records |

## D4. Options and recommendation

| Option | What | Trade-offs |
|--------|------|------------|
| **O1 Keep-ours** | Your 7 points as written | Simple; wrong on N=3 and severity; weak class validity |
| **O2 Refine-ours (CPR)** | Keep series + claim hygiene + class gate + fleet; add hybrid trigger, B3 tests, canary, multi-factor map, interim containment | Slightly more steps; much better alignment with ITIL/SRE/8D |
| **O3 Replace-with-X** | Full Safety-II / CAST learning reviews; abandon “root” language | Best model for complex socio-tech; high skill cost; non-engineer + LLM agents will struggle without facilitator |
| **O4 Dual tier** | Tier-A: CPR for recurring/ops classes; Tier-B: deep CAST/LFI learning review when budget burn / multi-team / auth security / “we fixed it thrice” | Best coverage; needs discipline to not default everything to Tier-A shallow |

### Recommendation

**O2 refined into CPR (optionally grow into O4 later).**  

**Confidence: [emerging]** — industry practice converges here; hard outcome RCTs are scarce.  

**Main assumption it rests on:**  
Your failures are often **real shared mechanisms** (stale credentials, profile drift, non-idempotent automation, green-but-dead health checks) that can be closed by gates — not pure irreducible complexity where only adaptive capacity helps. If most “recurrences” are *different* mechanisms with the same surface, CPR’s B3 tests will (correctly) refuse to invent a root, and you will need Tier-B learning reviews more often.

**Explicit non-recommendation:**  
Do **not** harden pure 5 Whys. Do **not** harden “exactly three then always class root.” Do **not** let an LLM agent close a fleet-wide class fix without canary + second-reviewer.

---

# Decision-oriented ending (short)

| Path | Risk | When to choose |
|------|------|----------------|
| Keep current rule | Fake roots from surface clusters; miss severity | Never as permanent |
| **CPR (refine)** | Slight process load | **Default now** |
| Full STAMP/CAST | Skill & time | After 2+ failed CPR fixes on same class |
| Dual tier | Complexity | When lab grows / security stakes rise |

**Single recommendation:** Adopt **Class Problem Review (CPR)** — series unit + hybrid trigger + mechanism validity tests + multi-factor claim-tagged map + dedicated permanent fix session + canary-before-fleet + recurrence closeout. Keep 5 Whys only as an optional *probe* inside step 4, never as the method. Treat LLM self-analysis as draft only (expect ~10–35% accuracy on hard RCA). Revisit in 90 days with one metric: **repeat rate of same *mechanism***, not incident count.

---

# Full source list

### 5 Whys critique & Toyota
1. Card AJ. *The problem with ‘5 whys’*. BMJ Qual Saf. 2017;26(8):671–677. https://qualitysafety.bmj.com/content/26/8/671 · PubMed https://pubmed.ncbi.nlm.nih.gov/27590189/  
2. AHRQ PSNet commentary. https://psnet.ahrq.gov/issue/problem-5-whys  
3. Wikipedia *Five whys* (Criticism, Ohno, Minoura). https://en.wikipedia.org/wiki/Five_whys  
4. Lean Enterprise Institute lexicon (Ohno machine example). https://www.lean.org/lexicon-terms/5-whys/  
5. Ohno T. *Toyota Production System* (1988) — primary quote via secondary citations above.  
6. Reliable Plant / maintenance single-thread bias. https://reliamag.com/articles/5-whys-root-cause-analysis-maintenance/  
7. IMD blog pitfalls. https://www.imd.org/blog/strategy/the-5-whys-technique/  

### Safety science / STAMP / Safety-II
8. Dekker S. *Field Guide to Understanding ‘Human Error’*. https://sidneydekker.com/the-field-guide-to-understanding-human-error · PDF excerpts https://www.leonardo-in-flight.nl/PDF/FieldGuide%20to%20Human%20Error.PDF  
9. Leveson et al. *Applying STAMP in Accident Analysis*. https://shemesh.larc.nasa.gov/iria03/p13-leveson.pdf  
10. Leveson STPA Handbook. https://www.flighttestsafety.org/images/STPA_Handbook.pdf  
11. CAST Handbook reference (MIT). http://sunnyday.mit.edu/CAST-Handbook.pdf  
12. UL SIS intro STAMP/STPA/CAST. https://www.ul.com/sis/blog/introduction-to-stamp-stpa-and-cast  
13. Hollnagel et al. *From Safety-I to Safety-II* white paper. https://www.england.nhs.uk/signuptosafety/wp-content/uploads/sites/16/2015/10/safety-1-safety-2-whte-papr.pdf  
14. AHRQ primer Safety-I/II. https://psnet.ahrq.gov/primer/safety-i-safety-ii-and-new-views-safety  
15. Hulme et al. Testing reliability of accident analysis methods (2024). https://www.tandfonline.com/doi/full/10.1080/00140139.2023.2240048  

### ITIL / Problem Management
16. IT Process Wiki Problem Management / KEDB. https://wiki.en.it-processmaps.com/index.php/Problem_Management  
17. IBM: Incident vs Problem Management. https://www.ibm.com/think/topics/incident-management-vs-problem-management  
18. Emory IT Problem Process Guidelines. https://it.emory.edu/smcc/itsm_process/problem/process-guidelines.html  
19. ITSM.tools PM article (2026). https://itsm.tools/problem-management-itsm-firefighting-to-prevention/  
20. Freshworks PM best practices. https://www.freshworks.com/freshservice/itsm/problem-management-best-practices/  
21. Splunk ITIL PM. https://www.splunk.com/en_us/blog/learn/problem-management.html  

### Google SRE
22. SRE Book Ch.15 Postmortem Culture. https://sre.google/sre-book/postmortem-culture/  
23. SRE Workbook Ch.10 Postmortem Culture. https://sre.google/workbook/postmortem-culture/  
24. SRE Workbook Appendix B Error Budget Policy. https://sre.google/workbook/error-budget-policy/  
25. Etsy Morgue. https://github.com/etsy/morgue  

### LFI / Allspaw
26. Allspaw slides: Learning ≠ fixing; frequency ≠ learning. https://speakerdeck.com/jallspaw/incident-analysis-how-star-learning-star-is-different-than-star-fixing-star  
27. Adaptive Capacity Labs (2025). https://www.adaptivecapacitylabs.com/2025/02/28/what-progress-in-learning-from-incidents-actually-looks-like/  
28. IT Revolution article. https://itrevolution.com/articles/learning-effectively-from-incidents-the-messy-details/  

### FRACAS / 8D / reliability
29. Relyence FRACAS methodologies (8D, DMAIC, PDCA). https://relyence.com/2023/01/16/an-overview-of-fracas-methodologies/  
30. AccendoReliability FRACAS. https://accendoreliability.com/failure-reporting-analysis-corrective-action-system-fracas/  

### Aviation aggregation
31. NASA ASRS. https://asrs.arc.nasa.gov/  
32. FAA ASIAS note on ASRS use. https://www.asias.faa.gov/  
33. SKYbrary ASRS. https://skybrary.aero/articles/aviation-safety-reporting-system-asrs  

### SPC / sample size
34. SPC for Excel: how much data for control limits. https://www.spcforexcel.com/knowledge/control-chart-basics/how-much-data-do-i-need-calculate-control-limits/  
35. Advantive SPC sampling. https://www.advantive.com/solutions/spc-software/spc-101/populations-and-sampling/  

### AI-agent failures & LLM RCA (2025–2026)
36. Cemri et al. MAST / multi-agent failures. https://arxiv.org/abs/2503.13657 · https://github.com/multi-agent-systems-failure-taxonomy/MAST · NeurIPS poster https://neurips.cc/virtual/2025/poster/121528  
37. Ma et al. AgentFail dataset/taxonomy/benchmark. https://arxiv.org/html/2509.23735v1  
38. OpenRCA (Xu et al.). https://openreview.net/forum?id=M4qNIzQYpd · PDF https://netman.aiops.org/wp-content/uploads/2025/05/13411_OpenRCA_Can_Large_Langua.pdf  
39. Riddell et al. LLM reasoning failures in cloud RCA. https://arxiv.org/abs/2601.22208 · Waterloo summary https://uwaterloo.ca/waterloo-intelligent-systems-engineering-lab/projects/llm-reasoning-failures-cloud-root-cause-analysis  
40. Partnership on AI — real-time agent failure detection (2025). https://partnershiponai.org/wp-content/uploads/2025/09/agents-real-time-failure-detection.pdf  
41. Zhang et al. Who&When failure attribution. https://arxiv.org/abs/2505.00212  
42. AgenTracer. https://arxiv.org/abs/2509.03312  
43. Awesome LLM AIOps list (pointers to RCAgent etc.). https://github.com/Jun-jie-Huang/awesome-LLM-AIOps  

### Incident classification / taxonomy validity
44. ENISA Reference Incident Classification Taxonomy (2018). https://www.enisa.europa.eu/publications/reference-incident-classification-taxonomy  
45. Simon Mylius — scalable AI incident classification (2024). https://simonmylius.com/blog/incident-classification  

---

**Epistemic summary:**  
- 5 Whys as primary method for complex multi-machine agent systems: **[established] harmful**.  
- Class-level Problem Management / SRE class budget / closed-loop corrective action: **[established] as practice**, **[emerging] as measured superiority**.  
- Agent failure taxonomies & LLM auto-RCA: **[emerging]**, accuracy still low.  
- Exact “N=3” rule: **[speculative]** heuristic only.  
- “There is never a root cause” (strong Safety-II/STAMP) vs “find and kill class causes” (ITIL/FRACAS/SRE): **live disagreement** — your environment needs both *mechanism-seeking CPR* and *humility that some failures are multi-control, multi-agent emergent*.
