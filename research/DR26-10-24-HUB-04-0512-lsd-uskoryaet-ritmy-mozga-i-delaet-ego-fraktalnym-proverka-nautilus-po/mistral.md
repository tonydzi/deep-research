# DR26-10-24-HUB-04-0512 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Faster, Flatter, Fractal — Investigating the LSD Brain-Rhythm Study Behind the Nautilus Headline
Share
Faster, Flatter, Fractal: An Investigative Deep Dive into the LSD MEG Re-Analysis Behind the Nautilus Story

Investigation prepared November 3, 2026. Role: investigative science writing with a neuroscience editor's rigor. Every factual claim is linked to an inspected source; inferences and unknowns are labeled.

The Question

On October 22, 2026, Nautilus published "LSD May Speed Up Brain Activity and Make it Fractal" by senior editor Kristen French, covering a new paper in Communications Biology. This investigation verifies the primary paper and its dataset, tests each of the article's five headline claims against the sources, situates the findings in the psychedelics-electrophysiology literature, critiques the methods, and identifies who benefits from the story.

Executive Summary

The study is real, peer-reviewed, and honestly reported in its key numbers — but it is a re-analysis of a 2016 Imperial College London MEG dataset, not new data (Nature paper, bioRxiv preprint). The Nautilus piece is broadly accurate on the science, including the music result's statistical weakness — its framing is better calibrated than the university press release.

The biggest journalistic distortion is the claim that older studies "missed" the rhythm speed-up. The original 2016 PNAS paper on this very dataset reported that the alpha peak shifted from ~10 Hz to ~12 Hz under LSD (t = 4.21, P = 0.0009), and cited 1950s–60s EEG studies reporting the same thing (PNAS 2016 PDF). What is new is the systematic, region-resolved peak tracking and the demonstration that power loss survives after accounting for the shift.

"Fractal" is real but not new. Increased fractal dimension and Lempel-Ziv complexity under LSD had been shown in 2017 (MEG signal diversity) and 2020 (fMRI fractal dimension) (Schartner 2017, Varley 2020). The new paper replicates this in source space — a confirmation, not a discovery.

The fractal-hallucination link remains speculation, and the authors say so on the record (UdeM press release).

The music-dampening finding is explicitly exploratory: nothing survived correction; the authors themselves call it "hypothesis-generating" and warn against over-interpretation (PsyPost interview).

No independent expert commentary exists in any coverage I found — the entire news cycle traces to one press release. That is the weakest link in the journalism, not in the paper.

Methodology

I opened and read: the Nautilus article (Oct 22, 2026); the published paper's abstract, figure captions, methods and conflict/funding records via PubMed, the Nature article page, and the open bioRxiv full text (posted Jan 29, 2026); the original 2016 PNAS paper whose data were re-analyzed; the Université de Montréal press release (Oct 21, 2026); an author interview at PsyPost (Oct 23, 2026); and the authors' public analysis code. Limitations: Nature's full-text page is bot-blocked, so some Results-level detail (exact per-feature decoding accuracies, supplementary statistics) rests on the preprint, figure captions, the press materials, and search-indexed excerpts of the published version rather than a complete read of the final typeset text; where that matters, it is flagged.

Part 1 — Verifying the Primary Paper
The paper and its provenance

Citation: Subramani, V., Nest, T., Pascarella, A., Brunel, J., Mantilla Ramos, Y.J., Harel, Y., Muthukumaraswamy, S., Carhart-Harris, R., Lioi, G., Farrugia, N., Jerbi, K. "LSD reconfigures cortical dynamics through faster brain rhythms and increased fractal dimension." Communications Biology 9:1244, published online October 7, 2026 (Nature, PubMed). A preprint was posted to bioRxiv on January 29, 2026 (bioRxiv).

Team: first author Venkatesh Subramani (doctoral student, CoCo Lab, Université de Montréal) with co-supervisors Karim Jerbi (UdeM/Mila/UNIQUE) and Nicolas Farrugia and Giulia Lioi (IMT Atlantique, Brest); co-authors include Suresh Muthukumaraswamy (University of Auckland) and Robin Carhart-Harris (UCSF) (bioRxiv author list).

Dataset origin — confirmed: the MEG data were collected at Imperial College London by Carhart-Harris's team for the first modern neuroimaging study of LSD, published in PNAS on April 26, 2016 (PNAS, Imperial College release). The re-analysis states: "we analyzed MEG data from 17 participants collected by Carhart-Harris and colleagues" (bioRxiv Methods).

What was done to the data

Original design (2016): 20 healthy volunteers, two scanning days at least two weeks apart, single-blind (not double-blind), balanced order; 75 µg LSD intravenously in saline vs. saline placebo. Peak subjective effects occurred ~120–150 min post-infusion; the 14 minutes of eyes-closed MEG rest were recorded 225 minutes (~3.75 h) after injection, on the declining phase of the drug effect, which the original authors acknowledged ("the intensity of LSD's subjective effects was… attenuated somewhat for the MEG") (PNAS PDF). PsyPost's interview adds that this is roughly 2.5 h after the subjective peak (PsyPost).

Sample accounting (inference flagged): the original paper used 14 volunteers in its MEG analyses; the re-analysis uses 17; the authors' code repository lists 18 subjects' raw files (IDs 001–020 with gaps) (GitHub). The most plausible reading — that better automated artifact rejection recovered three more participants — is my inference; the paper does not (in the text I could access) explain the discrepancy. Unknown: the exact reason.

Music condition: two seven-minute eyes-closed runs, silence vs. ambient music (excerpts from Yearning by Robert Rich and Lisa Moskow) (bioRxiv Methods, Nautilus).

New analysis pipeline: 2-s epochs; Autoreject (mask-only) → ICA (Picard) → second Autoreject pass, with similar epoch retention across drug conditions; source localization with FreeSurfer/BEM/dSPM onto the 360-region HCP-MMP1 atlas; spectral parameterization with FOOOF over 1–120 Hz (robustness check at 1–45 Hz); peak-frequency and peak-power extraction; Lempel-Ziv complexity (LZC); Higuchi fractal dimension (HFD, kmax = 18); Neurosynth/NiMARE functional decoding against >14,000 studies; Random Forest decoding with permutation testing (bioRxiv Methods, PubMed figure captions, GitHub).

Key results as reported

Alpha peak frequency rose by ~2 Hz and beta by ~5 Hz across large cortical areas, alongside a genuine attenuation of alpha and beta power that survives correction for the peak shifts (Nature abstract).

The aperiodic 1/f slope flattened (strongest effects highlighted in posterior cingulate and visual cortex, p < 10⁻³ to 10⁻⁵ per the figure legend), and LZC and HFD rose, strongest posteriorly, sparing motor cortex (PubMed Fig. 3 caption).

A subtle but consequential detail: in the posterior cingulate cortex — a default-mode hub — the large "power drops" seen in standard analyses vanished once the aperiodic component was removed; LSD's effect there was on the background signal, not the rhythm (PsyPost).

Machine learning: per-feature decoding accuracies ran ~68–82% (Nautilus's range; PsyPost reports >82% for peak-frequency features and >76% for complexity features), and the press release reports 88% when all features are combined, vs. 50% chance (Nautilus, UdeM, PsyPost).

Music: "does not robustly amplify these neural signatures and instead shows a trend toward attenuation" — no interaction survived correction; uncorrected trends pointed toward music dampening the drug signature (Nature abstract, bioRxiv statistical framework).

Statistics, corrections, preregistration

Main spectral contrasts used non-parametric cluster tests; region-level contrasts used paired tests with permutation correction (region-permutation, 50,000 iterations); phenomenology regressions used FDR correction and were explicitly labeled "exploratory"; the drug × music interaction results are presented as "nominally significant (i.e., p < 0.05 uncorrected)" (bioRxiv statistical framework). Preregistration: none is mentioned anywhere I could access — neither paper page, preprint, press materials, nor the code repository. Marked unknown whether one exists; if it does, it is not linked. This matters: all three hypotheses (H1 peak shifts, H2 genuine power loss, H3 music potentiation) were formulated after the data existed, which makes H3's "failure" a soft falsification, not a preregistered one.

How the Nautilus piece compares with the paper

Nautilus wording

	

What the sources say

	

Verdict




"LSD sped up… alpha… by about 2 beats per second, and beta by about 5"

	

Paper: alpha +~2 Hz, beta +~5 Hz; Subramani: "up to 2 Hz… up to 5 Hz" (PsyPost)

	

Accurate




"Older studies had not picked up this change in rhythm because of the way they measured"

	

The 2016 PNAS paper on this dataset reported the alpha peak shifting 10→12 Hz (t=4.21, P=0.0009) and cited 1950s–60s EEG studies reporting increased alpha frequency (PNAS PDF)

	

Misleading — the shift was observed, just not systematically modeled




"brain activity becomes more fractal… snowflake"-like nested patterns echoing visual hallucinations

	

Paper shows HFD/LZC increases; hallucination link explicitly unconfirmed by Jerbi (UdeM)

	

Accurate with the authors' own hedge carried through




ML "68 percent and 82 percent accuracy, depending on the specific feature"

	

Matches per-feature numbers; the 88% combined figure appears in press materials, and Nautilus omits it — a conservative choice (Nautilus, UdeM)

	

Accurate




Music "dampened" effects but "didn't hold up under strict statistical analysis… more of a hint than a fact"

	

Matches the paper and the authors' "hypothesis-generating" framing (PsyPost)

	

Accurate — better calibrated than the press release's suggestion of surprise




"17 volunteers who either got LSD or a placebo… on two separate days"

	

Within-subject crossover: each volunteer got both, two weeks apart (bioRxiv)

	

Sloppy phrasing, could imply between-subject design




"Since at least 2012, scientists have believed… turning the dial down on… control centers"

	

DMN-disintegration findings and entropic-brain/REBUS theory do dominate the literature (PNAS, REBUS)

	

Reasonable simplification

Net assessment: the Nautilus piece is a careful piece of science journalism that accurately transmits the study, flags the small sample, the re-analysis status, and the muscle-artifact caveat — and its caveat on music is more honest than the average press release. Its one real distortion is the "older studies missed it" frame, which manufactures novelty. Its structural weakness: no independent voices; the Jerbi "radio station" quote is from the university release, and everything else is press-release-derived (UdeM) — a pattern shared by nearly every other outlet that covered it (Medical Xpress, News-Medical, Scientific Frontline).

Part 2 — Context: The Quieting Paradigm and the Prior Electrophysiology

The "decade of quieting" Nautilus describes is real and traceable:

Entropic brain (2014): Carhart-Harris et al. proposed that psychedelics push the brain toward criticality and higher entropy (Frontiers in Human Neuroscience). Note: the entropic brain framework predicts more complex, less predictable dynamics — the new paper's complexity findings are confirmations of that pillar, not a refutation of the paradigm.

REBUS (2019): Carhart-Harris & Friston's "Relaxed Beliefs Under Psychedelics" unified model — the strongest articulation of the quieting idea: psychedelics relax the precision of high-level priors, liberating bottom-up signaling (Pharmacological Reviews).

Psilocybin MEG (2013): Muthukumaraswamy et al. found broadband cortical desynchronization (1–50 Hz posterior, 8–100 Hz frontal), strongest in default-mode regions, with dynamic causal modeling attributing posterior cingulate desynchronization to increased deep-layer pyramidal excitability (Journal of Neuroscience) — the paper that anchored "psychedelics desynchronize."

LSD MEG (2016): the dataset paper itself: decreased delta-to-beta power; decreased visual-cortex alpha correlating with hallucinations and ego dissolution (PNAS, New Scientist).

Complexity/diversity (2017–2023): increased MEG signal diversity (LZC) under psilocybin, ketamine, LSD — interpreted as an "elevated level of consciousness" (Schartner 2017, Scientific Reports); DMT: alpha suppression with increased LZ complexity and delta/gamma power (Timmermann et al. preprint; PNAS 2023 EEG-fMRI DMT study).

Fractals (2020): Varley et al. showed LSD and psilocybin increase the fractal dimension of cortical activity in both spatial (connectivity) and temporal (BOLD) domains, explicitly framed as validating criticality-based theories (NeuroImage).

Desynchronization at scale (2024): Siegel et al.'s precision-mapping study, "Psilocybin desynchronizes the human brain," showed massive acute FC disruption centered on the DMN, persisting weeks for the hippocampus-DMN link (Nature).

Where the new result fits: it does not overturn the desynchronization finding — it refines it, showing the power loss is genuine and co-occurs with a frequency shift, and that part of the "DMN suppression" signal (in PCC) is aperiodic rather than rhythmic. The speed-up itself fits an older empirical thread: the 1950s–60s EEG literature and the 2016 PNAS report of the 10→12 Hz alpha shift (PNAS PDF). A 2025 preprint from an Oxford/Aarhus/Imperial group analyzing the same Imperial dataset with a different method (FREQ-NESS, n=14) independently found LSD enhances high-alpha (~12–13 Hz) and high-beta (~25 Hz) prominence (bioRxiv) — convergent evidence that LSD pushes rhythms faster, from a different lab. The 1/f flattening is the converse of the criticality story: flatter spectra and higher fractal dimension are both markers of dynamics closer to criticality, consistent with the entropic-brain prediction (entropic brain, Varley 2020).

Part 3 — Methodological Critique

Aperiodic-separation validity. The whole edifice rests on FOOOF-style parameterization (Donoghue et al. 2020). These fits make choices (fit range 1–120 Hz, fixed aperiodic mode, max 5 peaks) that can move peak-frequency and slope estimates; the authors ran a 1–45 Hz robustness check in the supplement and reported goodness-of-fit metrics (bioRxiv). An alternative decomposition (IRASA, which separates fractal from oscillatory components by irregular resampling) exists and was not used (Wen & Liu 2016). Unknown: how sensitive the +2/+5 Hz estimates are to the parameterization choices beyond the one robustness check.

Muscle artifacts. MEG's dirty secret: cranial muscle activity spans ~20–300 Hz, overlapping "high-gamma" (Muthukumaraswamy 2013 review) — a critique written by the same Muthukumaraswamy who is now a co-author of the re-analysis, and a confound the original PNAS authors flagged in their own gamma results (PNAS PDF). Nautilus relays the authors' counter-argument (the patterns are "likely too organized" for muscle) (Nautilus); PsyPost notes the authors' own call for EMG sensors in future studies (PsyPost). The alpha/beta findings are largely below the classic muscle band and more robust; the high-gamma claims should be treated cautiously.

Drug-induced movement/eye artifacts. The pipeline (visual inspection, Autoreject ×2, ICA) is reasonable, and the authors report epoch retention did not differ between LSD and placebo (t = −1.69, p < 0.12) (bioRxiv). Still, subtle between-condition artifacts (blinks, micro-movements) that survive ICA cannot be fully excluded; the identity covariance (no empty-room recordings) is a further approximation.

N = 17, single-blind, declining phase. The original study was explicitly single-blind — the authors' 2016 justification was that blinding is ineffective with a conspicuous drug (PNAS PDF). Expectation effects cannot be excluded. Seventeen participants is small; the drug × music interaction is exactly the kind of subtle effect that is underpowered at n=17 — the authors say as much (PsyPost). And MEG was ~2.5 h past the subjective peak, which the authors offer as a reason few strong brain–experience correlations emerged.

Researcher degrees of freedom. A re-analysis of public-ish legacy data, multiple feature families (peaks, power, slope, offset, LZC, HFD), 360 regions, several decoding models, and no preregistration I could find. The main findings are permutation- or cluster-corrected, which helps; but the "weakening and speeding are dissociable" claim rests on the absence of a statistical link between two spatial maps — weak evidence for mechanistic separation.

ML classifier validity. Random Forest decoders, cross-validated, with 1000-label-permutation significance testing on feature importances (PubMed Fig. 3). Accuracies of 68–88% vs. 50% chance are meaningful but modest. Unknown (from accessible text): whether cross-validation splits were subject-wise. With two sessions per subject and highly correlated epochs, epoch-wise splitting would risk leakage and inflated accuracy; the phrase "nested CV" appears in a third-party distillation of the paper, not in a source I can verify directly. Marked unknown — this is the single most important check a peer or replicator should do first (the code is public: GitHub).

Replication status. No independent replication of the peak-shift analysis exists yet. Convergent support: the FREQ-NESS preprint's high-alpha/beta findings in the same dataset (bioRxiv); the paper's own note that DMT does not show the shift and psilocybin shows it only inconsistently (bioRxiv intro) — meaning the "LSD signature" claim awaits the team's in-progress LSD/psilocybin/DMT comparison (UdeM).

Part 4 — The Fractal–Hallucination Hypothesis: Evidence vs Speculation

Evidence: LSD reliably increases temporal complexity metrics (LZC: Schartner 2017; HFD: the new paper) and spatial/temporal fractal dimension under fMRI (Varley 2020). Geometric hallucinations have a serious mathematical account: Bressloff, Cowan, Golubitsky, Thomas & Wiener showed that Klüver's form constants (gratings, cobwebs, tunnels/funnels, spirals) correspond to planforms produced when V1's resting state destabilizes — patterns whose wavelengths are integer multiples of the hypercolumn width (Phil. Trans. R. Soc. B 2001). The original PNAS paper already leaned on this model ("instability within the primary visual cortex may facilitate the emergence of geometric hallucinations via self-organized patterns of neural excitation") and tied reduced alpha to a disinhibition of visual cortex (PNAS PDF).

Speculation: the leap from temporal fractal structure in whole-cortex MEG signals to spatial self-similarity in perceived images. These are different constructs measured in different domains. Jerbi says exactly this: "We measure the fractal structure of brain signals over time, not the fractal structure of the images people perceive" (UdeM). No data in the paper connect HFD values to hallucination reports; phenomenology regressions were exploratory and few strong links survived (the MEG timing gap is one likely reason) (PsyPost). The "snowflake" imagery is journalism, not results. Verdict: an elegant, currently unfalsified hypothesis.

Part 5 — Music and LSD: A Field Says "Synergy," a Trend Says "Anchor"

The prior literature strongly supports music–psychedelic interaction at the subjective and connectivity levels: LSD enhances the emotional response to music ("wonder," "transcendence," "power," "tenderness"; n = 10) (Kaelen et al. 2015, Psychopharmacology); the LSD × music interaction increased parahippocampal–visual-cortex connectivity and information flow, correlating with imagery (n = 12) (Kaelen et al. 2016, Eur. Neuropsychopharmacol.); LSD broadly altered music-evoked brain activity, with effects tracking timbral complexity and "wonder" (Kaelen et al. 2017 preprint); and Kaelen's "hidden therapist" synthesis argued for a central role of music in psychedelic therapy (Kaelen 2018).

The new trend sits oddly against this — but the tension may be more apparent than real:

What was measured differs. Kaelen's work measured music-evoked emotion and imagery systems; the new paper measured whether music amplifies drug-wide spectral signatures (slope, complexity). Music can be emotionally powerful while simultaneously anchoring/stabilizing large-scale dynamics. (This reconciliation is my inference.)

The new result is not significant. Nothing survived correction; the authors' own follow-up preprint on the same dataset (structure-function coupling) likewise found no significant drug × music interaction with a consistent attenuation trend (bioRxiv follow-up).

The "anchor" idea has independent support. A 2024 analysis of this dataset found psychedelic-induced complexity was highest eyes-closed and dropped with outward attention (Mediano et al. 2024, ACS Chem. Neurosci.); Siegel 2024 found performing a perceptual task reduced psilocybin's FC disruption (Nature); and a 2026 Nature study found psilocybin LZC highest in eyes-closed conditions (Nature 2026).

Design limits: one 7-minute ambient track averaged to a static measure, ~4 h post-injection, n = 17 — as the authors themselves concede (PsyPost).

Bottom line: "music did not boost LSD effects" is an accurate description of an exploratory null; "music dampens LSD" is a hypothesis. Nautilus got this distinction right.

Part 6 — Independent Experts, Conflicts of Interest, Funding

Independent commentary: none found. Across all coverage I inspected (Nautilus, Medical Xpress/Science X, News-Medical, Scientific Frontline, Yahoo, Newswise, NewsBeep, PsyPost), every quote comes from the authors or the press release; PsyPost's interview adds author elaboration but no outside scientist ([all links above]). Unknown: whether any outlet solicited and failed to obtain outside comment. This is a single-source news cycle.

Natural critics to consult next (no fabricated quotes; roles and institutions from their cited work):

Bradley Voytek (UC San Diego) — senior author of the spectral parameterization method the paper stands on (Donoghue et al. 2020); the natural referee of FOOOF-application choices.

Enzo Tagliazucchi (Universidad de Buenos Aires / Frankfurt) — long-time complexity-and-psychedelics analyst on the original dataset (PNAS co-author).

Joshua Siegel (Washington University in St. Louis) — desynchronization mapping, context effects (Nature 2024).

Frederick Barrett (psychedelic-music neuroscientist, co-author of the Kaelen music papers) — the obvious referee for the music claim (Kaelen 2015).

Chris Timmermann (DMT EEG; now psychedelic neuroscientist) — cross-psychedelic comparisons (Timmermann preprint).

Note the internalized critic: Suresh Muthukumaraswamy, author of the field's canonical muscle-artifact critique, is a co-author of both the original dataset analyses and this re-analysis — a good sign for the artifact handling, but it also means the tight-knit Imperial-era network reviews itself.

Conflicts of interest: the paper discloses that Robin Carhart-Harris "reports providing scientific advice for TRYP therapeutics, Osmind, Otsuka, and Red Light Holland. Others declare no competing interests" (PubMed). Funding: Jerbi's Canada Research Chair and an NSERC Discovery Grant; original data collection was supported by the Safra Foundation and the Beckley Foundation (Beckley-Imperial program) plus a Walacea crowdfunding campaign (UdeM, PubMed funding). No funder had a psychedelic-therapy commercial stake in this particular analysis (inference from the disclosure list); the broader field is heavily commercialized — MindMed is running the first-ever Phase 3 trials of an LSD formulation (MM120) for generalized anxiety disorder, with topline 12-week Voyage data expected in 2026 and a second Phase 3 (Panorama) underway (Definium/MindMed release, ClinicalTrials.gov NCT06809595, Psychiatric Times). Unknown: Voyage topline results as of this writing.

Part 7 — Verdict Table

#

	

Claim

	

Verdict

	

Confidence

	

What would change the verdict




1

	

Alpha/beta rhythms speed up (~+2 / ~+5 Hz) under LSD

	

Supported (in this dataset)

	

High

	

Failure to replicate in a new, larger MEG/EEG dataset at peak drug effect




2

	

Older studies "missed" the speed-up because of fixed-band measurement

	

Partly misleading

	

High

	

Nothing — the 2016 paper and 1950s–60s EEG already reported it; what older studies lacked was systematic peak-resolved analysis




3

	

Rhythms genuinely weaken after accounting for the shift

	

Supported (this dataset)

	

Medium-high

	

A peak-resolved analysis of an independent dataset contradicting it




4

	

Background (1/f) flattens; activity more fractal/complex

	

Supported, and replicates prior work

	

High

	

A confound demonstration (e.g., muscle-driven flattening) — unlikely given low-frequency focus




5

	

Fractal brain dynamics explain fractal/nested hallucinations

	

Speculative

	

High (that it's speculative)

	

Trial-level correlation of HFD with sampled hallucination reports at peak effect




6

	

ML separates LSD from placebo at 68–82% (88% combined)

	

Supported as reported

	

Medium

	

Subject-wise cross-validation check on the public code; independent replication




7

	

Music dampened LSD's neural effects

	

Unsupported (exploratory trend only)

	

Medium-high

	

A powered, preregistered music × drug MEG study; significant interaction in either direction




8

	

LSD "doesn't quiet control centers; it reshuffles the background hum"

	

Partly supported / overreach as stated

	

Medium

	

The PCC aperiodic finding supports a reinterpretation of DMN results, but genuine rhythmic weakening and DMN effects remain; direct DMN-vs-slope adjudication hasn't been done




9

	

Speed-up is a signature unique to LSD

	

Speculative / pending

	

Low

	

The team's ongoing LSD/psilocybin/DMT comparison; independent peak-frequency studies of psilocybin

Part 8 — Why It Matters and What to Watch Next

Why it matters. If a psychedelic's signature includes faster oscillations — the opposite of what sedatives and many anesthetics do to alpha — then band-power meta-analyses, biomarkers, and EEG endpoints in psychedelic trials may be measuring through a moving window, and the entropic/REBUS vocabulary needs an update that accommodates excitation-with-disorganization rather than pure quieting. Methodologically, it is a clean demonstration of why aperiodic-aware analysis is now the minimum standard in psychedelic electrophysiology (Donoghue et al. 2020). The motor-sparing, sensory-heavy topography also gives the first source-resolved geometry of where complexity rises.

Watch next:

Replication at the peak. The current data were recorded on the drug's declining phase; a peak-effect MEG study with EMG is the obvious confirmatory test — and the authors' call for exactly this (PsyPost).

Cross-psychedelic comparison. Subramani et al. are already applying these methods to psilocybin and DMT (UdeM); this will adjudicate the "unique to LSD" claim (its dopamine receptor affinity is the mechanistic candidate — inference).

The functional-connectivity preprint. The team says a preprint on how peak shifts confound connectivity measures is imminent (PsyPost); their structure-function preprint is already posted (bioRxiv).

Open science posture. Code is public under MIT (GitHub); raw MEG data were not in the repo (stored externally), though the Imperial dataset has demonstrably circulated to at least two outside groups (FREQ-NESS preprint, Mediano 2024). Watch for a formal data deposit and for whether the ML CV scheme is subject-wise.

Field-level rigor headwinds. The same week this coverage ran, a five-lab consortium (UC Berkeley, UCSF, Stanford; ~200 mice per experiment) reported that many persistent behavioral effects of psilocybin failed to replicate (Nature Neuroscience 2026, Berkeley News) — a reminder that single small-sample psychedelic findings, however elegant, await convergence.

Clinical pipeline. MindMed's MM120 (LSD) Phase 3 program in generalized anxiety disorder (ClinicalTrials.gov) — if an electrophysiological "speed-up signature" becomes a trial biomarker, this paper will be its origin story.

Source Notes

Source

	

Credibility

	

Last updated




Subramani et al. 2026, Communications Biology 9:1244 (published paper page)

	

5/5 (peer-reviewed primary source; page bot-blocked, accessed via excerpts)

	

Oct 7, 2026




Subramani et al. 2026, PubMed record (abstract, captions, COI, funding)

	

5/5 (primary)

	

Oct 7, 2026




Subramani et al. 2026, bioRxiv preprint full text

	

4/5 (preprint of the published paper; methods may differ slightly from final version)

	

Jan 29, 2026




Specparam_LSD analysis code repository

	

4/5 (authors' own code; direct evidence of pipeline)

	

May 29, 2026 (last commit)




Nautilus: "LSD May Speed Up Brain Activity and Make it Fractal" (Kristen French)

	

3/5 (the article under review; secondary)

	

Oct 22, 2026




Université de Montréal press release

	

3/5 (institutional PR; primary for quotes, biased toward novelty)

	

Oct 21, 2026




PsyPost author interview (Eric W. Dolan)

	

3/5 (secondary; author-sourced but adds caveats and design detail)

	

Oct 23, 2026




Carhart-Harris et al. 2016, PNAS 113(17):4853–4858 (dataset paper, full PDF)

	

5/5 (primary; the origin of the data)

	

Apr 26, 2016




Imperial College 2016 press release

	

3/5 (institutional PR, historical)

	

Apr 2016




New Scientist 2016 coverage

	

3/5 (secondary, historical)

	

Apr 11, 2016




Muthukumaraswamy et al. 2013, J. Neurosci. (psilocybin MEG desynchronization)

	

5/5 (primary)

	

Sep 18, 2013




Muthukumaraswamy 2013, muscle-artifact review, Front. Hum. Neurosci.

	

5/5 (primary methods review)

	

Apr 15, 2013




Schartner et al. 2017, Scientific Reports (MEG signal diversity)

	

5/5 (primary)

	

Apr 19, 2017




Varley et al. 2020, NeuroImage (fractal dimension under LSD/psilocybin)

	

5/5 (primary)

	

Oct 15, 2020




Timmermann et al., DMT multivariate EEG (preprint)

	

4/5 (preprint; later work published in PNAS 2023)

	

Jul 2019




Timmermann et al. 2023, PNAS (DMT EEG-fMRI)

	

4/5 (primary, accessed via ResearchGate record)

	

Mar 2023




Siegel et al. 2024, Nature (psilocybin desynchronizes the human brain)

	

5/5 (primary)

	

Jul 17, 2024




Carhart-Harris et al. 2014 (entropic brain), Front. Hum. Neurosci.

	

4/5 (peer-reviewed theory paper)

	

Feb 3, 2014




Carhart-Harris & Friston 2019 (REBUS), Pharmacol. Rev.

	

5/5 (primary theory)

	

Jul 2019




Donoghue et al. 2020 (specparam/FOOOF), Nat. Neurosci.

	

5/5 (primary methods)

	

Nov 23, 2020




Wen & Liu 2016 (IRASA), J. Neurosci. Methods

	

4/5 (primary methods, accessed via record)

	

2016




Bressloff et al. 2001 (geometric visual hallucinations), Phil. Trans. R. Soc. B

	

5/5 (primary theory)

	

Mar 29, 2001




Kaelen et al. 2015 (LSD enhances emotional response to music), Psychopharmacology

	

5/5 (primary; institutional record)

	

Oct 2015




Kaelen et al. 2016 (LSD modulates music-induced imagery), Eur. Neuropsychopharmacol.

	

4/5 (primary, accessed via ResearchGate record)

	

Jul 2016




Kaelen et al. 2017 (Effects of LSD on music-evoked brain activity), bioRxiv preprint

	

3/5 (preprint; appears unpublished in a journal as far as I could verify)

	

Jun 2017




Kaelen 2018 ("The hidden therapist"), Psychopharmacology (PMC)

	

4/5 (peer-reviewed review/synthesis)

	

Feb 2018




Mediano et al. 2024 (external stimulation & psychedelic neurodynamics), ACS Chem. Neurosci.

	

4/5 (primary; institutional record)

	

Jan 12, 2024




Shinozuka et al. 2025 (FREQ-NESS LSD MEG), bioRxiv preprint

	

3/5 (preprint; independent-of-UdeM but Imperial-linked team, same dataset)

	

Mar 24, 2025




Subramani et al. 2026 (structure-function decoupling under LSD), bioRxiv preprint

	

3/5 (preprint, same group)

	

Mar 2, 2026




Multi-lab psilocybin mouse study, Nat. Neurosci. and Berkeley News release

	

5/5 (primary) / 3/5 (PR)

	

Nov 2, 2026




MindMed MM120 Phase 3 (Voyage) press release and Panorama, ClinicalTrials.gov NCT06809595

	

3/5 (sponsor PR; registry 4/5)

	

Dec 2024 / Sep 4, 2026




Medical Xpress / News-Medical / Scientific Frontline coverage

	

2/5 (press-release republication; used only to document the news cycle)

	

Oct 21–22, 2026




Pallavicini et al. 2019 (spectral signatures), NeuroImage

	

4/5 (primary; record-based citation)

	

Oct 15, 2019

Caveats and conflicts: the Nature-published version could not be read in full due to bot-blocking; Results-level details rest on the preprint (whose final published wording may differ), PubMed records, indexed excerpts, and press materials. The original dataset's investigators (Carhart-Harris, Muthukumaraswamy) are co-authors of the re-analysis, and the Beckley Foundation — an advocacy-oriented funder — supported the original data collection; both facts are disclosed, but they mean this critique of the "quieting" paradigm is an inside critique.

Open Questions

Were the machine-learning cross-validation splits subject-wise or epoch-wise? (Decisive for the 68–88% accuracies; checkable in the public code.)

Why 17 participants here vs. 14 in the original MEG analysis vs. 18 in the code repo's raw folders?

Was any analysis plan preregistered? No evidence found either way.

Do the alpha/beta peak shifts replicate at the peak of the drug effect, with EMG monitoring, in a fresh dataset?

Does psilocybin also speed up alpha/beta under peak-resolved analysis, or is the LSD claim of uniqueness robust? (The team's in-progress comparison should answer this.)

What are the Voyage Phase 3 topline results (expected H1 2026), and will any psychedelic EEG biomarker standard adopt aperiodic/peak-resolved measurement?

Recommendation for the Reader

The Nautilus piece is trustworthy on what the study found and appropriately hedged on what it might mean, with one correction needed: the rhythm speed-up was observed before — in the very dataset being re-analyzed — and the new paper's genuine contribution is showing that the desynchronization survives after you account for it, mapping it across the cortex, and connecting it to the aperiodic "hum." Treat the fractal-hallucination link as a hypothesis and the music-dampening as a lead, not a finding. And when the next psychedelic-electrophysiology headline lands, ask first whether the analysis separated peaks from the background hum — because this paper has made that the question that separates careful work from confounded work.

## Sources (harvested)
- https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030
- https://www.nature.com/articles/s42003-026-10629-7
- https://www.biorxiv.org/content/10.64898/2026.01.28.702361v1
- https://orca.cardiff.ac.uk/id/eprint/95413/1/4853.full.pdf
- https://www.nature.com/articles/srep46421
- https://www.sciencedirect.com/science/article/pii/S1053811920305358
- https://nouvelles.umontreal.ca/en/article/2026/10/21/lsd-does-more-than-desynchronize-the-brain
- https://www.psypost.org/lsd-pushes-brain-activity-toward-a-more-fractal-complex-state-study-finds/
- https://pubmed.ncbi.nlm.nih.gov/42844315/
- https://www.biorxiv.org/content/10.64898/2026.01.28.702361v1.full
- https://github.com/venkateshness/specparam_LSD
- https://www.imperial.ac.uk/news/171699/the-brain-lsd-revealed-first-scans/
- https://pubmed.ncbi.nlm.nih.gov/31221820/
- https://medicalxpress.com/news/2026-10-lsd-desynchronize-brain-rhythms-complexity.html
- https://www.news-medical.net/news/20261022/LSD-speeds-up-brain-waves-and-increases-neural-complexity-study-finds.aspx
- https://www.sflorg.com/2026/10/ns10212601.html
- https://www.frontiersin.org/journals/human-neuroscience/articles/10.3389/fnhum.2014.00020/full
- https://www.jneurosci.org/content/33/38/15171.short
- https://www.newscientist.com/article/2083851-first-lsd-brain-imaging-study-offers-insights-into-consciousness/
- https://www.biorxiv.org/content/10.1101/706283v1.full
- https://www.researchgate.net/publication/369381928_Human_brain_effects_of_DMT_assessed_via_EEG-fMRI
- https://www.nature.com/articles/s41586-024-07624-5
- https://www.biorxiv.org/content/10.1101/2025.03.21.644645v1
- https://www.nature.com/articles/s41593-020-00744-x
- https://www.semanticscholar.org/paper/Separating-Fractal-and-Oscillatory-Components-in-of-Wen-Liu/b3e253bd4a981a91e64e40bda9b7b4a5556672d9
- https://www.frontiersin.org/journals/human-neuroscience/articles/10.3389/fnhum.2013.00138/pdf
- https://pubmed.ncbi.nlm.nih.gov/11316482/
- https://pure.johnshopkins.edu/en/publications/lsd-enhances-the-emotional-response-to-music/
- https://www.researchgate.net/publication/301279545_LSD_modulates_music-induced_imagery_via_changes_in_parahippocampal_connectivity
- https://www.researchgate.net/publication/346926189_Effects_of_LSD_on_music-evoked_brain_activity
- https://pmc.ncbi.nlm.nih.gov/articles/PMC5893695/
- https://www.biorxiv.org/content/10.64898/2026.03.02.709138v1.full
- https://pure.au.dk/portal/en/publications/effects-of-external-stimulation-on-psychedelic-state-neurodynamic/
- https://www.nature.com/articles/s41586-026-10910-z
- https://ir.definiumtx.com/news-events/press-releases/detail/169/mindmed-announces-first-patient-dosed-in-phase-3-voyage-study-of-mm120-in-generalized-anxiety-disorder-gad
- https://clinicaltrials.gov/study/NCT06809595
- https://www.psychiatrictimes.com/view/mindmeds-phase-3-trials-of-mm120-for-generalized-anxiety-disorder-underway
- https://www.nature.com/articles/s41593-026-02500-z
- https://news.berkeley.edu/2026/11/02/in-parallel-mouse-studies-researchers-fail-to-replicate-many-reported-effects-of-psilocybin/
- https://pubmed.ncbi.nlm.nih.gov/31247301/