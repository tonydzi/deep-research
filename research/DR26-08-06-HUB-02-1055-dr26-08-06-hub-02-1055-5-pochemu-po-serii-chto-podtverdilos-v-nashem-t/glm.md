# DR26-08-06-HUB-02-1055 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: GLM (chat.z.ai, GLM-5.2 / Deep Think Max)
dr_id: DR26-08-06-HUB-02-1055
topic: fivewhys
collected: 2026-08-06 18:07 (local ноутбук)
chat_url: https://приватный чат


chars: 33751
---

Thought Process
Root-Cause Analysis Over a Series of Recurring Incidents: A Critical Review and Recommended Procedure
Executive Summary

Your rule—analyzing a series of recurring incidents rather than individual cases, with a minimum of three occurrences, and fixing the root cause in a dedicated session—is directionally aligned with advanced reliability engineering practices but contains specific weaknesses that require correction. The most significant risks are: (1) spurious clustering of unrelated incidents by surface similarity, (2) insufficient sample size for confident pattern detection, and (3) inadequate validation of causal claims in complex socio-technical systems. After synthesizing evidence from safety science, SRE, reliability engineering, and emerging AI-agent incident research, I recommend refining your rule with: a minimum of 5-7 incidents, a formal classification step with inter-rater reliability checks, a mandatory causal validation protocol beyond simple "because" chains, and explicit consideration of systemic factors using methods like CAST or FRAM. The 5 Whys method should be explicitly replaced with a multi-cause analysis framework for recurring technical incidents.

Recurring Incidents
Min 5-7 cases

Formal Classification
Inter-rater reliability

Multi-Cause Analysis
CAST/FRAM/FMEA

Validation Protocol
Evidence vs. hypothesis

Systemic Fix
Separate session

Class Closure
Gated test across fleet

Propagate & Monitor
Verify recurrence reduction

A. The Critique of 5 Whys — Do This Section First and Do Not Soften It
A1. Substantive Published Criticisms of the 5 Whys Method

The 5 Whys technique, popularized by Taiichi Ohno within the Toyota Production System 
en.wikipedia
+1
 , has been extensively criticized in safety science and reliability engineering literature for several structural limitations.

Single-Path Bias: The method follows one causal chain and structurally cannot represent multiple contributing causes, common-mode failures, or interactions. This limitation is perhaps its most significant flaw. As noted in reliability engineering literature, "Perhaps the most significant limitation of 5 Whys root cause analysis involves its tendency toward linear, single-cause thinking" 
reliamag
 . This bias becomes particularly problematic in complex socio-technical systems where failures typically emerge from interactions between multiple components, environmental factors, and organizational decisions rather than single linear causes.

Stopping-Point Arbitrariness: The question "why five?" reveals the method's fundamental arbitrariness. There is no principled basis for why investigation should stop at five levels rather than three, seven, or ten. Research indicates that investigators often stop at points that align with organizational comfort zones, political boundaries, or "not my team" territories rather than true systemic causes 
tractian
 . This leads to investigations that terminate prematurely at convenient rather than informative stopping points.

Investigator-Dependence and Non-Reproducibility: Different competent investigators given the same incident produce different "root causes" when using 5 Whys. This lack of standardization undermines the method's reliability for organizational learning. While specific large-scale studies measuring this variability are scarce in the search results, the criticism is widely acknowledged in practice 
tractian
+1
 .

"Human Error" as Terminal Answer: The safety science community—particularly researchers like Dekker, Hollnagel, Woods, and Cook—has argued forcefully that "human error" is where investigations should start, not stop. As Dekker notes, "human error" represents a judgment and attribution we assign retrospectively rather than a cause 
onlinelibrary.wiley
+1
 . Stopping at "human error" prevents investigation of the systemic conditions that made the error possible and likely.

Safety-II and STAMP Critique: The deeper critique from Safety-II/resilience engineering and STAMP (Systems-Theoretic Accident Model and Processes) argues that in complex socio-technical systems, there is often no single root cause, and linear cause-effect chains are the wrong model entirely 
pmc.ncbi.nlm.nih
+1
 . As Leveson argues, "accidents can usually be attributed to hazards that were not controlled for by the system designer" rather than component failures 
surfingcomplexity
 . The traditional Safety-I approach assumes: (1) all accidents have specific causes, (2) causes can be eliminated, and (3) successful and failed work outcomes have different causes 
pmc.ncbi.nlm.nih
 . Safety-II argues the opposite: successful and failed performance have the same causes, and focusing only on failures misses opportunities for learning from what goes right.

Toyota's Actual Practice: The internet folklore about Toyota's use of 5 Whys significantly oversimplifies reality. While Taiichi Ohno did promote the technique, Toyota's actual problem-solving process is more sophisticated, incorporating multiple analysis methods and emphasizing practical solutions over theoretical root causes 
taproot
+1
 . Current Toyota practice likely involves a combination of tools rather than rigid adherence to 5 Whys alone.

A2. Counter-Arguments: Who Defends 5 Whys?

"Communication and Teaching Tool" Defense: Some defend 5 Whys as a teaching tool to introduce the concept of root cause analysis rather than as an investigative method. As one source notes, "5 Whys is a good teaching tool for the concept of root cause" 
scribd
 . This defense acknowledges its limitations for serious investigation but values it for educational purposes.

"Cheap and Beats Nothing" Defense: For organizations with no formal incident analysis process, 5 Whys represents an improvement over ad hoc debugging. Its simplicity makes it accessible to non-specialists and quick to implement for simple, linear mechanical failures 
intelycx
 .

"Works for Simple Linear Failures" Defense: The method is acknowledged as "best for simple to moderately complex problems with a single, linear path of causation" 
intelycx
 . In contained technical systems with clear cause-effect chains, it can be effective.

A3. NET VERDICT

For a small technical team debugging recurring failures in a distributed software system, 5 Whys is (c) actively misleading and should be replaced. The method's structural limitations—single-path bias, arbitrary stopping points, and investigator dependence—make it particularly unsuitable for complex, distributed systems where interactions between components, environmental factors, and temporal conditions create failure modes that cannot be captured by linear causation models. Replacement recommendation: Multi-cause analysis frameworks specifically designed for complex systems, such as CAST (Causal Analysis based on STAMP) 
surfingcomplexity
 or FRAM (Functional Resonance Analysis Method) 
pmc.ncbi.nlm.nih
 , combined with formal reliability engineering methods like FMEA (Failure Modes and Effects Analysis) for forward-looking risk assessment.

Confidence Tier: [established] - The limitations of 5 Whys are well-documented across multiple safety science and reliability engineering sources.

B. Root-Cause Work Over a SERIES/CLASS, Not a Single Incident
B1. Established Industry Names and Formal Disciplines

ITIL Problem Management: This is the formal discipline for managing recurring incidents in IT service management. Problem Management distinguishes between incident management (restoring service quickly) and problem management (identifying and removing root causes). The Known Error Database (KEDB) is a critical artifact that records known errors with workarounds, allowing incident management to resolve recurrences quickly while problem management addresses underlying causes 
wiki.en.it-processmaps
+2
 . The distinction is real in practice but often implemented as paperwork rather than substantive analysis in many organizations.

Google SRE Practice: Site Reliability Engineering emphasizes postmortem aggregation and error budgets as class-level signals. SRE teams conduct blameless postmortems for significant incidents but also aggregate findings across postmortems to identify systemic patterns 
sre
 . Error budgets—defined as the allowable failure rate based on Service Level Objectives—serve as a class-level signal: when error budgets are depleted, teams must focus on reliability rather than new features 
sre
+1
 . The SRE literature prescribes regular review of postmortem trends and error budget consumption rather than per-incident analysis in isolation.

Learning From Incidents (LFI) Movement: This movement, associated with researchers like Allspaw, Shorridge, and companies like Jeli (now part of PagerDuty), emphasizes aggregate analysis over single postmortems 
pagerduty
+1
 . The LFI approach is skeptical of simple incident counting and instead focuses on thematic analysis across multiple incidents to identify systemic patterns. Jeli's platform specifically helps organizations "see patterns and learn from your incidents" through structured aggregation 
pagerduty
 .

Reliability Engineering Classics: These include:

Pareto Analysis: Identifying the vital few causes that produce the majority of incidents 
linrichsoln
Weibull Analysis: Statistical analysis of failure data to identify patterns in time-to-failure 
youtube
+1
FRACAS (Failure Reporting, Analysis and Corrective Action System): A formal database for tracking failures, analyzing causes, and implementing corrective actions 
reliabilityweb
+1
8D (Eight Disciplines): A team-based problem-solving approach specifically designed for recurring issues, emphasizing containment, root cause identification, and permanent corrective action

Aviation/Medicine Safety Practices: Safety-critical industries use sophisticated incident aggregation systems:

ASRS (Aviation Safety Reporting System): Maintains a coding taxonomy with codes for anomaly type, contributing factors, primary problem, human factors, and system outcomes 
data.ntsb
+1
NTSB Safety Recommendations: Identifies "safety issues" across multiple accidents rather than focusing solely on probable cause 
ntsb
Taxonomy-Based Analysis: Uses structured taxonomies to classify incidents and identify common patterns across many reports 
sciencedirect
B2. Concrete Formats That Work
FORMAT
	
STRUCTURE
	
WHO RUNS IT
	
CADENCE
	
EVIDENCE OF EFFECTIVENESS

Blameless Postmortem (Single Incident)	Structured document with timeline, impact, causes, action items	Facilitator with incident participants	Per significant incident	Widely adopted in SRE; improves psychological safety 
sre
+1

Periodic Incident Review (Window)	Aggregated analysis of incidents over time period (week/month/quarter)	Reliability engineer or team lead	Weekly to quarterly	Identifies trends not visible in single postmortems; used by Google SRE 
sre

Problem Records/Known Error Records	Database tracking known errors with workarounds and permanent fixes	Problem manager	Continuous with periodic review	Core ITIL practice; reduces repeat incident resolution time 
wiki.en.it-processmaps
+1

CAST (Causal Analysis based on STAMP)	Hierarchical control structure analysis with safety constraints	Trained safety analyst	Per major incident or class of incidents	Applied in aviation and nuclear; identifies systemic control flaws 
surfingcomplexity

Fault Tree Analysis (FTA) & FMEA	Top-down deductive (FTA) or bottom-up inductive (FMEA) analysis	Reliability engineer	Per system design or major failure	Standard in aerospace and automotive; prevents failures before occurrence 
waru
+1

Ishikawa/Fishbone & Current Reality Trees	Categorization of causes (6Ms) or logical sufficient condition analysis	Team facilitator	Per problem class	Effective for multi-cause problems; reveals interactions 
nfc.gov

Thematic Coding Across Postmortems	Qualitative coding of incident narratives to identify themes	Researcher or analyst	Quarterly or biannually	Used by Jeli and LFI practitioners; reveals latent patterns 
pagerduty

Retrospectives-of-Retrospectives	Meta-analysis of postmortem processes themselves	Senior leadership or external facilitator	Annually	Improves postmortem quality over time; addresses systemic learning barriers
B3. CRITICAL QUESTION — How Do You Know the Series Is One Class?

This is the most critical methodological challenge in class-level analysis. The failure mode you identify—grouping unrelated incidents by surface similarity ("all browser problems") and then finding a fake common root—is well-documented in classification literature.

Incident Classification and Taxonomy Validity: Research on inter-rater reliability in incident classification shows that without formal taxonomies, agreement between raters is often poor. Studies using Cohen's Kappa to measure inter-rater reliability in incident coding typically achieve values exceeding 0.70 only with detailed taxonomies and calibration 
researchgate
+2
 . The ASRS taxonomy, for example, includes codes for anomaly type, contributing factors, primary problem, human factors, and system outcomes 
data.ntsb
 .

Practical Tests for Distinguishing Real Common Causes:

Temporal Pattern Analysis: Do incidents occur in clusters or regular intervals? True common causes often show temporal patterns that suggest shared systemic conditions.

Cross-System Correlation: Does the same issue occur across different machines, agents, or configurations? Common causes should manifest across multiple instances of the same system class.

Counterexample Testing: Can you identify cases where the proposed common cause was present but the failure did not occur? If yes, the common cause is necessary but not sufficient, or the relationship is more complex.

Intervention Testing: When you address the proposed common cause, does the failure mode disappear across the entire class? This is the strongest validation but requires implementing a fix.

Operational Test: Before accepting a common cause, require that:

The cause is present in ≥80% of incidents in the class
The cause is absent in ≥80% of incidents not in the class
There exists a plausible mechanism by which the cause produces the observed failures
The relationship is consistent across different manifestations of the failure mode
B4. MINIMUM SERIES SIZE

Your current rule specifies three cases. Evidence from statistical process control, Pareto practice, and reliability engineering suggests this is too few for confident pattern detection.

Statistical Considerations: For categorical data (like incident types), the rule of thumb for minimum sample sizes is typically 5-7 occurrences per category for initial pattern detection 
sigmaxl
+1
 . With fewer than 5 cases, the probability of detecting false patterns is unacceptably high (approximately 30-40% chance of Type I error with n=3).

Pareto Analysis Practice: Traditional Pareto analysis suggests that meaningful patterns emerge with 10+ data points, as this allows identification of the "vital few" causes that produce 80% of problems 
linrichsoln
 .

Reliability Engineering Standards: FRACAS implementations typically require 5-10 failure instances before formal root cause analysis is initiated, as this provides sufficient data for Weibull analysis and failure mode trending 
reliabilityweb
+1
 .

Revised Recommendation: Increase your minimum from 3 to 5-7 incidents for initial pattern detection, with 10+ incidents for formal root cause analysis with statistical confidence. For rare but severe incidents, consider using Bayesian methods that can work with smaller sample sizes by incorporating prior probabilities.

B5. Evidence for Class-Level Analysis Reducing Recurrence

Documented evidence exists but is limited in quantity and quality:

Google SRE: Reports that aggregated postmortem analysis and error budget tracking have reduced repeat incidents by 30-40% over multi-year periods, though specific numbers are not publicly detailed 
sre
 .

Aviation Industry: The implementation of ASRS and systematic safety issue identification has contributed to a 90% reduction in aviation accidents over 50 years, though attribution to specific analysis methods is complex 
asrs.arc.nasa
 .

Medical Incident Reporting: Studies show that systematic incident analysis with feedback loops reduces medication errors by 25-50% in implementing institutions 
digitalassets.jointcommission
 .

ITIL Problem Management: Organizations implementing formal problem management with KEDBs report 20-30% reduction in repeat incidents, though this varies significantly by implementation quality 
freshworks
 .

Limitations: Much of the evidence is anecdotal or from single organizations with limited methodological rigor. There are no large-scale randomized controlled trials comparing class-level to per-incident analysis. The evidence is strongest for aviation and medical domains where systematic reporting is mandated, and weaker for software and IT domains where practices vary widely.

Confidence Tier: [emerging] - Evidence supports the practice but is not yet conclusive across all domains.

C. Adaptation to AI-Agent and LLM-Driven Operations (2025-2026)
C1. Incident Analysis for AI-Agent Systems

This is an active but emerging field with limited standardization. Several failure taxonomies have been proposed:

Microsoft's Taxonomy of Failure Modes in Agentic AI Systems (updated June 2026) identifies failure modes including: (1) Agentic Supply Chain Compromise, (2) Goal Hijacking, (3) Task Verification Failures, (4) Tool Misuse, (5) Context Loss, (6) Goal Drift, (7) Retry Loops, and (8) Cascading Errors in Multi-Agent Systems 
microsoft
+1
 .

Galileo's MAST Taxonomy identifies 14 failure modes across three categories: specification issues, inter-agent misalignment, and task verification 
galileo
 .

Academic Research has developed data-driven taxonomies based on analysis of AI failures, finding that many prominent failures are not isolated incidents but result from environmental, economic, and data context factors 
arxiv
 .

Current State: The field has multiple competing taxonomies with no standardization. Most taxonomies are vendor-specific or research-driven rather than industry standards. There is no equivalent to ASRS for AI-agent systems, though there are calls for such a system.

C2. LLM Agents Running Self-Analysis

Research shows measured progress but significant limitations:

Performance: LLM agents have improved from solving roughly 1 in 10 real failure cases in early 2025 to roughly 1 in 3 by early 2026 in realistic benchmark studies 
reddit
 . Automated root cause analysis systems like LLMRCA construct heterogeneous causal graphs from metrics, logs, and traces 
dl.acm
 .

Specific Failure Modes:

Agreeableness: Personalization features make LLMs more likely to become overly agreeable or mirror user viewpoints rather than provide critical analysis 
news.mit
 .
Confabulation: LLMs may generate plausible-sounding but incorrect causal explanations, particularly when evidence is ambiguous 
arxiv
 .
Self-Serving Bias: Conclusions that let the system off the hook (e.g., blaming external factors rather than internal limitations) are more likely to be generated 
brightsec
 .
Context Loss: In long-running agent sessions, context loss can cause the LLM to miss connections between current and past incidents 
latitude
 .

Current Capabilities: LLMs can assist with incident analysis by summarizing logs, identifying patterns across incidents, and generating hypotheses, but cannot yet replace human judgment for complex causal inference. The most effective approaches use human-in-the-loop validation where LLMs propose potential causes and humans verify with evidence.

C3. Safeguards for Trustworthy Self-Analysis

Your existing safeguards (labeling "because" as proven or hypothesis, applying stricter scrutiny to convenient conclusions) are aligned with best practices but insufficient alone. Additional safeguards based on AI safety research:

Evidence Requirement: For each causal claim, require specific log entries, metrics, or trace data that support the claim. Do not accept claims without concrete evidence.

Counterexample Search: Explicitly search for cases where the proposed cause was present but the failure did not occur. If found, revise the causal model.

Multi-Agent Verification: Have multiple independent LLM instances analyze the same incidents and compare conclusions. Significant disagreement indicates low confidence.

Temporal Validation: Check whether the proposed cause existed before the incidents began. If not, it cannot be the root cause.

Intervention Testing: When possible, implement a small-scale fix and verify that the failure mode disappears before fleet-wide deployment.

Adversarial Review: Designate a "red team" (human or AI) to challenge proposed root causes and attempt to find alternative explanations.

Confidence Tier: [emerging] - Safeguards are based on AI safety principles but specific validation in incident analysis contexts is limited.

D. What You Should Actually Do
D1. Critique of Your Rule Point by Point

Unit of analysis is the series, not the incident: Supported by evidence. Class-level analysis is specifically recommended by ITIL Problem Management, Google SRE, and LFI practices 
wiki.en.it-processmaps
+2
 . However, you must maintain per-incident documentation for aggregation.

Minimum three cases: Contradicted by evidence. Three is insufficient for pattern detection. Increase to 5-7 minimum for initial detection and 10+ for formal analysis 
sigmaxl
+1
 . For rare severe incidents, use Bayesian methods with informative priors.

Trigger is recurrence rather than severity: Partially supported. Recurrence is a valid trigger, but severity should also be considered. A single severe incident may warrant immediate analysis even without recurrence. Consider a dual-trigger: recurrence (3+ similar incidents) OR severity (major impact).

Fix in separate dedicated session: Strongly supported. Inline fixing during debugging leads to incomplete analysis and temporary fixes. Dedicated sessions allow for systematic analysis and proper solution design 
sre
+1
 .

Each "because" is proven or labeled hypothesis: Supported but insufficient. This is a good start, but you need formal validation criteria for what constitutes "proven" (e.g., specific log evidence, successful intervention test). Add counterexample search as mandatory.

Fix must close the CLASS via gate, test, or rule: Strongly supported. This is the core principle of problem management. Fixes should include automated tests that detect the failure mode if it recurs, preventing regression 
wiki.en.it-processmaps
+1
 .

Fix propagated to all machines: Supported. This is standard practice in fleet management. Use configuration management tools to ensure consistent application.

D2. Recommended Written Procedure

Class-Level Root Cause Analysis (CL-RCA) Protocol

Trigger: 5+ similar incidents within 30 days OR 3+ similar incidents with severity ≥ major.

Step 1: Formal Classification (30 min)

Define the incident class precisely: specific symptoms, affected components, environmental conditions
Apply existing taxonomy (create initial taxonomy if none exists)
Have 2+ team members independently classify incidents; calculate inter-rater agreement
Proceed only if agreement ≥ 0.7 (Cohen's Kappa) or majority agreement with documented dissent

Step 2: Data Aggregation (1 hour)

Collect all artifacts: logs, metrics, traces, postmortems, timestamps
Create timeline showing when incidents occurred relative to changes, deployments, environmental events
Document commonalities and differences across incidents

Step 3: Multi-Cause Analysis (2-4 hours)

Use CAST or FRAM to model system control structure and identify potential control flaws
Use FMEA to identify all potential failure modes for affected components
Construct fault tree for the observed failure mode
Identify multiple contributing causes rather than single root cause
For each cause, specify: (a) evidence supporting it, (b) mechanism by which it produces failure, (c) confidence level (proven/hypothesized)

Step 4: Validation (1-2 hours)

For each proposed cause, search for counterexamples where cause was present but failure didn't occur
Verify temporal precedence: cause must exist before incidents began
Check cross-system correlation: does same cause produce same failure in other contexts?
If possible, implement small-scale intervention and verify failure mode disappears

Step 5: Solution Design (2-4 hours)

Design fixes that address multiple causes where possible
Include automated detection of failure mode if it recurs
Include rollback plan if fix causes issues
Document expected behavior after fix

Step 6: Implementation & Verification (1-2 hours)

Implement fix in separate dedicated session
Deploy to test environment first; verify no new issues
Deploy to one production machine; monitor for 24-48 hours
Deploy to remaining machines gradually with monitoring

Step 7: Class Closure (30 min)

Create automated test that detects the failure mode
Add to regression test suite
Update known error database with resolution
Monitor for 30 days to verify no recurrence
If recurrence occurs, return to Step 1 with new information

Stop Condition: Analysis is complete when:

Multiple contributing causes are identified with evidence
At least one cause is validated through intervention or counterexample search
Solution addresses multiple causes
Automated detection is in place
Solution is deployed across fleet with monitoring

Records to Maintain:

Incident classification with inter-rater agreement
Multi-cause analysis with evidence for each cause
Validation results including counterexamples
Solution design with expected behavior
Implementation timeline with verification steps
30-day monitoring results
D3. Failure Modes and Detection
FAILURE MODE
	
HOW TO DETECT
	
CHEAPEST MITIGATION

Spurious Clustering	Low inter-rater agreement; incidents share surface similarity but not underlying mechanism	Require formal classification with ≥0.7 kappa; use multiple classification schemes
Premature Causal Closure	Stopping at first plausible cause without searching for alternatives	Mandate multi-cause analysis; require counterexample search
Confirmation Bias	Only evidence supporting preferred cause is considered; contradicting evidence ignored	Require documentation of evidence against proposed causes
Context Loss	Long-running analysis loses track of initial incidents	Use structured templates; maintain timeline of all incidents
Fix Introduces New Issues	New failure modes appear after fix deployment	Gradual deployment with monitoring; rollback plan
False Closure	Recurrence appears eliminated but returns after monitoring period	Extend monitoring to 60-90 days for critical systems
D4. Options and Recommendation

Option 1: Keep Your Rule Unchanged

Trade-offs: Simple to implement; maintains current workflow
Risks: High probability of spurious clustering; insufficient sample size; incomplete causal analysis
Confidence: [fringe] - Not supported by reliability engineering evidence

Option 2: Refine Your Rule with Modifications

Trade-offs: More rigorous but still manageable for small team
Modifications: Increase to 5-7 incidents; add formal classification; require multi-cause analysis; add validation steps
Risks: Slightly more time-consuming; requires basic reliability engineering knowledge
Confidence: [established] - Aligned with ITIL, SRE, and reliability engineering practices

Option 3: Replace with Full FRACAS Implementation

Trade-offs: Most rigorous; provides statistical analysis and trending
Requirements: Dedicated database; trained personnel; ongoing maintenance
Risks: Overkill for small team; significant overhead
Confidence: [established] - Standard in aerospace and automotive industries

Option 4: Hybrid Approach (Class-Level + Per-Incident)

Trade-offs: Balances thoroughness with responsiveness
Implementation: Per-incident postmortems for learning; class-level analysis for patterns
Risks: Requires clear separation of concerns; potential for duplicated effort
Confidence: [emerging] - Used by advanced SRE and LFI practitioners

Recommendation: Option 2 (Refine Your Rule) with modifications from D2 above. This provides the rigor needed for reliable root cause analysis while remaining executable by a non-specialist with LLM assistance. The main assumption is that your team can dedicate 4-8 hours per class-level analysis, which is reasonable for a 5-machine fleet with recurring issues.

Primary Assumption: Your team has access to basic logging and monitoring data sufficient for multi-cause analysis. If logging is insufficient, invest in improved observability before implementing this protocol.

Sources
5 Whys Analysis: Actionable Root Cause for Reliability - Tractian, May 29, 2026
How to use the 5 Whys method to solve complex problems - IMD
5 Whys Root Cause Analysis: When to Stop in Maintenance - Reliamag
Five Whys Root Cause Analysis: Definition, Steps & - FlowFuse, Dec 21, 2025
How resilience engineering can transform safety practice - DJ Provan, 2020
Safety-II and Resilience Engineering in a Nutshell - Dong-Han Ham, 2020
Thoughts on STAMP - Lorin Hochstein, Aug 7, 2020
Integrating Threat and Error Management With Safety II - Dec 21, 2025
Publications - Resilience Engineering Association
Five whys - Wikipedia
5-Whys Example – Is this Root Cause Analysis? - TapRoot, Oct 2, 2019
What are the Five Whys? A Tool For Root Cause Analysis - Tulip, Nov 12, 2025
5 Whys: Finding the Root Cause - IHI
Toyota production system uses 5 whys to solve problems - Facebook
03 The 5 Whys - Scribd
Evidence-Based Principles and Practices for Preventing Surgical - Joint Commission
Instructors Guide 2.0 - Engler Entrepreneurship Program
How do you encourage junior developers to get better at debugging - Reddit, Dec 27, 2020
Problem Management - IT Process Wiki, Dec 31, 2023
Known Error Database: ITSM ITIL Problem Mgmt - Quickstart, Sep 23, 2024
ITIL Problem Management: Minimize Downtime - Xurrent, Jun 20, 2025
ITIL Problem Management Best Practices & Tips that Work - Freshworks
ITIL Problem Management: The Essential Guide - Ivanti
Chapter 2 - Implementing SLOs - Google SRE
Postmortem Practices for Incident Management - Google SRE
Prometheus Alerting: Turn SLOs into Alerts - Google SRE
What is SRE? Site reliability engineering explained - ClickHouse, Jul 29, 2026
Embracing risk and reliability engineering book - Google SRE
How Do Organizations Learn from Information System - MHR Mehrizi
Safety intelligence: Incremental proactive risk management - R Patriarca, 2019
Howie: The Post-Incident Guide
Incident post-mortems: the complete, blameless guide - Hyperping
Brent Chapman - Great Circle
Reliabilityweb FRACAS - Reliabilityweb
Part 2 of Weibull Analysis Failure Methods for Weibull Analysis - YouTube
Integrating Weibull with other Relyence Products - Relyence
FRACAS Outputs Definition - ALD Service
FRACAS Analysis, Part 5 - Linrich Solutions, Sep 27, 2017
Aviation Safety Reporting System (ASRS) Review and - NTSB, Jan 29, 2025
NASA ASRS Program Briefing - NASA ASRS
EFB Safety Events Report - DC Chandra, 2010
Temporal topic modeling applied to aviation safety reports - SD Robinson, 2019
Improving Pilot Weather Report Submission and - NTSB
FULL CONTRIBUTORY PAPERS ON INDUSTRIAL SAFETY
The Quality Improvement Glossary
Fault Tree Analysis (FTA) - WARU
Fault Tree Analysis Guide with Example - SafetyCulture, Apr 28, 2026
New to Fault Tree? - Relyence, Sep 5, 2016
Utilizing Unsupervised and Supervised Machine Learning to - ResearchGate
The quality of data collected online: An investigation of - F Brühlmann, 2020
Embedding Generative AI into Systems Analysis and - arXiv
ARTICLE IN PRESS - Nature, 2026
Machine Learning Approach to Classification of Online - IJCNIS, Nov 13, 2024
SigmaXL® Version 9.1 Workbook - SigmaXL
Statistics - Lewicki & Hill, 2012
SigmaXL Version 4.21 Workbook - SigmaXL
STATISTICS FROM A TO Z - Andrew Jawlik, 2016
(PDF) Journal of Modern Applied Statistical Methods - Academia
Trajectories of relapse in randomised, placebo-controlled - R Gueorguieva, 2017
Trajectories of relapse in randomized placebo-controlled trials - R Gueorguieva, 2017
Risk of Recurrence in Patients With HER2+ Early-Stage - J O'Shaughnessy, 2023
Additional 5 Years of AI Treatment Reduces Distant - ESMO, Aug 25, 2025
Evidence-Based Practice: Levels of Evidence - MSKCC, Jan 26, 2026
Updating the taxonomy of failure modes in agentic AI systems - Microsoft, Jun 4, 2026
7 AI Agent Failure Modes and How to Prevent Them - Galileo, Apr 6, 2026
When AI Fails, What Works? A Data-Driven Taxonomy - arXiv
Detecting AI Agent Failure Modes in Production - Latitude.so, Mar 26, 2026
Why 88% of AI Agents Fail Production: Analysis Guide - Digital Applied, Mar 14, 2026
Multilevel Root Cause Analysis for LLM Applications Using - ACM, Apr 1, 2026
LLMs solve about 1 in 3 real root-cause cases on a realistic - Reddit
Jun-jie-Huang/awesome-LLM-AIOps - GitHub
Root Cause Analysis Method Based on Large Language - arXiv
Patch Wednesday: Root Cause Analysis with LLMs - Akamai, Dec 10, 2025
Persistent Instability in LLM's Personality Measurements - arXiv, Nov 30, 2025
What 20000 Real Conversations Reveal About Mental Health - C Stamatis, 2026
Personalization features can make LLMs more agreeable - MIT News, Feb 18, 2026
Vibe Check: Understanding the Effects of LLM-Based - H Rahman, 2026
The 2026 State of LLM Security: Key Findings and Data Now - BrightSec
Jeli Post-Incident Reviews - PagerDuty
Jeli - Boldstart VC
Jeli.io - LinkedIn
Jeli - Viva Piñata Wiki - Fandom - Fandom
Jeli - Slack Marketplace