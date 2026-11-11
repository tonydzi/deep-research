# DR26-10-24-HUB-04-0512 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Faster, Weaker, "More Fractal": What the LSD-MEG Study Shows and Where Nautilus Overreaches

**The core findings hold up but are less new than the coverage suggests.** In a 17-person MEG reanalysis, LSD sped up and weakened alpha and beta rhythms, flattened the 1/f background and raised signal complexity. But the "speed-up" was already visible in the original 2016 Imperial study. The link to fractal hallucinations was never tested. The music "dampening" did not survive correction for multiple comparisons.

## TL;DR

- **The primary paper exists and the headline numbers are accurate.** Subramani, Jerbi and nine co-authors, *Communications Biology* 9:1244, published 7 October 2026. Alpha peaks rose by about 2 Hz and beta peaks by about 5 Hz under LSD, the aperiodic slope flattened, and Lempel-Ziv complexity and Higuchi fractal dimension both rose. Classifiers beat chance at more than 68% to more than 82% depending on the feature. Nautilus's claim that "older studies missed" the speed-up is overstated: the original 2016 PNAS paper on this same dataset already reported the alpha peak moving from 10 Hz under placebo to 12 Hz under LSD (t = 4.21, P = 0.0009).
- **The "snowflake" hallucination link is speculation, from the press release and Nautilus, not a result.** The paper never correlates fractal dimension with visual ratings. In its exploratory, uncorrected analyses, stronger complex imagery tracked *lower*, not higher, Lempel-Ziv complexity.
- **The music finding is a non-significant trend on the same dataset.** No interaction survived correction, and the authors call it exploratory. It echoes a 2024 reanalysis of the same recordings, so it is not independent confirmation. Independent replication, and independent expert commentary, do not yet exist.

## Key Findings

### 1. The primary paper: verified, with caveats

**The paper.** The DOI 10.1038/s42003-026-10629-7 resolves to "LSD reconfigures cortical dynamics through faster brain rhythms and increased fractal dimension" in *Communications Biology* (volume 9, article 1244). The Université de Montréal press release of 21 October 2026 gives the publication date as 7 October 2026. A bioRxiv preprint, DOI 10.64898/2026.01.28.702361, preceded it in early 2026.

**Authors and affiliations.** The authors are Venkatesh Subramani, Timothy Nest, Annalisa Pascarella, Jérémy Brunel, Yorguin José Mantilla Ramos, Yann Harel, Suresh Muthukumaraswamy, Robin Carhart-Harris, Giulia Lioi, Nicolas Farrugia and Karim Jerbi. The affiliations listed include:
- the CoCo Lab in UdeM's psychology department;
- IMT Atlantique in Brest;
- CNR in Italy;
- the University of Auckland;
- UC San Francisco;
- Mila;
- the UNIQUE neuro-AI centre.

**Dataset.** The paper analysed "MEG data from 17 participants collected by Carhart-Harris and colleagues", the 2016 PNAS study "Neural correlates of the LSD experience revealed by multimodal neuroimaging". That study enrolled 20 healthy participants for two scanning days, LSD and placebo, at least two weeks apart. LSD was given as 75 µg intravenously. The design was within-subject, single-blind and counterbalanced. MEG was recorded about four hours after injection on a CTF-275 system, and the reanalysis used the eyes-closed and eyes-closed-with-music runs.

**Why 17 rather than 20 is unknown.** The published paper gives no reason. Its acknowledgements thank a colleague for "identifying missing data for two subjects", which hints at incomplete data but does not explain the number. The 2016 PNAS paper itself reported usable ASL/BOLD data from 15 volunteers. A separate 2025 bioRxiv reanalysis of the same LSD MEG data used 14 participants. Sample sizes therefore differ across reanalyses of one dataset, which is a classic researcher-degree-of-freedom.

**Pipeline.** Preprocessing combined:
- 1–120 Hz filtering;
- 2-second epochs;
- two passes of the Autoreject package around an ICA cleaning step;
- source reconstruction with dSPM into the 360-region HCP-MMP atlas.

No empty-room recordings existed, so an identity noise covariance was assumed.

Spectra were parameterized with the FOOOF/specparam toolbox in "fixed" aperiodic mode (no knee) over 1–120 Hz, with up to five peaks, and a 1–45 Hz robustness check was placed in the supplement. Peak frequency was defined as the maximum of the 1/f-removed spectrum within each band. "Peak Power" was the oscillatory power at that maximum. Complexity was measured with:
- broadband Lempel-Ziv complexity (LZC) on concatenated epochs;
- Higuchi fractal dimension (HFD) with K_max = 18, chosen by a knee-detection algorithm.

There was no DFA, Hurst or multifractal analysis.

**Statistics.** Global spectra used cluster-based permutation tests. Regional maps used MNE's permutation t-test with 50,000 permutations. The bioRxiv methods describe the null as built by "permuting the regions", an unusual wording that readers should check against the code. Brain–experience correlations used Spearman's ρ with FDR correction, but the authors themselves call the region-resolved results exploratory. The paper reports no preregistration; whether one exists is unknown.

**Openness.** Code is public on GitHub (venkateshness/specparam_LSD). Data are available only by request to Muthukumaraswamy and Carhart-Harris.

**What the paper reports.** The confirmed results are:
- **Peak shifts.** Alpha peaks moved up by about 2 Hz and beta peaks by about 5 Hz on average in significant regions (permutation-corrected p < 0.05).
- **Genuine oscillatory weakening.** Alpha and beta Peak Power fell even after accounting for the peak shifts. Fixed-band analyses inflated the apparent spatial extent of the weakening.
- **The PCC result.** A posterior cingulate (PCC) power drop seen in the raw spectra vanished once the aperiodic component was removed. This matters for DMN-centric readings.
- **Aperiodic and complexity changes.** The slope flattened and the offset fell. LZC and HFD both rose, most strongly in posterior regions.
- **Unreported effect sizes.** Effect sizes are given as t-maps and p-thresholds. Standardized effect sizes (Cohen's d, confidence intervals on the Hz shifts) do not appear in the text I could access.

**Music.** The abstract says music "does not robustly amplify these neural signatures" and shows "a trend toward attenuation". The results section states that no regions survived whole-brain FDR correction, and that interaction effects were only nominally significant at uncorrected p < 0.05. "Dampened" therefore refers to spectral and complexity measures, specifically a steeper slope, higher offset, and lower LZC and HFD. It does not refer to classifier accuracy or to subjective effects.

**Classifier.** The classifier was a Random Forest with nested StratifiedGroupKFold cross-validation (outer 4 folds, inner 3) and 1,000 label permutations. The text reports these accuracies:
- above 68% for spectral power;
- above 82% for peak frequency;
- about 76% (published: above 76%) for LZC and HFD.

The UdeM press release says combining all measures gives 88%. That figure does not appear in the published text and may live only in a figure or the supplement. Treat it as unverified.

**Conflicts and funding.** Carhart-Harris declares scientific advice to TRYP Therapeutics, Osmind, Otsuka and Red Light Holland; the other authors declare none. The work was funded by Jerbi's Canada Research Chair (950-232368) and an NSERC Discovery Grant (2021-03426). The original data collection was funded by the Safra Foundation, the Beckley Foundation and Walacea crowdfunding. A peer review file is published. The named reviewers are Santiago Castro-Zaballa and Yasir Çatal; I did not read the file.

A point Nautilus omits: two co-authors, Muthukumaraswamy and Carhart-Harris, built the original dataset and the 2016 paper. This is a methods reanalysis by an extended original team, not an independent re-examination.

### 2. Nautilus versus the paper

The Nautilus piece ran on 22 October 2026, written by senior editor Kristen French and updated 23 October. Its only quotes are Jerbi's, taken from the UdeM "statement", not from an interview. Its accuracy, point by point:

- **Accurate:**
  - the 17 participants;
  - the ~2 Hz and ~5 Hz shifts;
  - the flatter "background hum";
  - the motor cortex being spared;
  - the muscle-artefact caveat for gamma;
  - the 68–82% range (though the paper's figures are lower bounds, not a range).
- **Overreach: "Older studies had not picked up this change."** The 2016 PNAS paper reported a peak alpha rhythm "reduced in amplitude and of higher frequency", 10 Hz under placebo versus 12 Hz under LSD (t = 4.21, P = 0.0009). The new paper itself says LSD peak increases "have been noted" before but were never systematically quantified or mapped across the cortex. The accurate claim is that older studies saw the shift but did not separate it from power changes or map it spatially.
- **Overreach: framing against the "quieting" model.** Nautilus casts the result as "less that LSD turns off the brain's controller" and more a reshuffle. The paper does not position itself against DMN or REBUS accounts. It explicitly interprets the alpha/beta weakening as consistent with REBUS and the slope and complexity changes as consistent with the entropic brain hypothesis.
- **Journalistic embellishment: the snowflake.** The image of nested patterns "like the geometries of a snowflake" is Nautilus's. The press release raises a "tempting" link to fractal hallucinations. Jerbi immediately cautions: "We measure the fractal structure of brain signals over time." The paper does not make this claim.
- **Mischaracterization: "more dynamic" background under music.** Nautilus says the background "became more dynamic" with music. The paper reports a steeper slope and higher offset, which means more low-frequency-dominated, not "more dynamic". Nautilus does correctly flag that the music result failed strict statistics, calling it "more of a hint than a fact".
- **Ambiguity: "either got LSD or a placebo".** The volunteers did not get one or the other; every participant received both, on separate days.
- **Omissions:**
  - the co-authorship of the original data team;
  - Carhart-Harris's industry ties;
  - the missing EMG;
  - the timing problem: MEG was recorded about 150 minutes after peak effects, and the 2016 paper notes subjective intensity was "attenuated somewhat" during MEG, which was acquired about 50 minutes after the MRI protocol ended;
  - the fact that the brain-to-function mapping came from Neurosynth meta-analytic "functional decoding", not from measured perception.

### 3. Context: the "quieting" story and prior electrophysiology

**The "quieting" story.** Nautilus links the 2012 PNAS psilocybin fMRI paper as the origin of the "control centre" view. That paper reported decreased activity and connectivity in DMN hubs; its details were not re-verified here beyond the citation. The theoretical lineage runs through:
- the 2014 *Frontiers in Human Neuroscience* "entropic brain" paper;
- the 2019 REBUS model of Carhart-Harris and Friston in *Pharmacological Reviews*.

Both are cited as references in the new paper; I did not independently fetch them.

**Critiques of the DMN-centric account.** Doss et al. (*Brain*, February 2022) set REBUS alongside two other models:
- a cortico-striato-thalamo-cortical (CSTC) gating model;
- a claustro-cortical (CCC) model in which psychedelics disrupt claustrum-entrained network states.

Their point is that subcortical circuits may matter as much as cortical hierarchy. Siegel et al. (*Nature*, 17 July 2024) found psilocybin desynchronizes functional connectivity, most strongly in the DMN, with a perceptual task reducing the effect. An international fMRI mega-analysis in *Nature Medicine* (2026) found a cross-drug signature of *enhanced* connectivity between transmodal and sensorimotor networks. Inference: "quieting" was never the whole story. The new MEG paper adds an electrophysiological layer rather than overturning the field.

**Prior electrophysiology.** Broadband desynchronization under psilocybin was established by Muthukumaraswamy et al. (*J Neurosci* 2013). Similar effects are reported for DMT (Timmermann et al., *Scientific Reports* 2019, N=12, which separated oscillatory and fractal components) and for LSD in the 2016 PNAS study. On this same Imperial dataset:
- **Schartner et al.** (*Scientific Reports*, 19 April 2017) found higher Lempel-Ziv signal diversity under LSD, psilocybin and ketamine, even after controlling for spectral changes.
- **Pallavicini et al.** (*NeuroImage*, October 2019) showed broadband power reductions and machine-learning separation of psychedelics from ketamine.
- **Varley et al.** (*NeuroImage*, October 2020) found higher fractal dimension under LSD and psilocybin. That was in fMRI BOLD, not MEG, and localized to the dorsal attention network.

The complexity and fractality findings are therefore replications within one data lineage, not discoveries.

**The aperiodic precedent and a tension.** Muthukumaraswamy & Liley (*NeuroImage* 2018) modelled 1/f spectra across four pharmaco-MEG datasets including this LSD one. A 2025 bioRxiv preprint (FREQ-NESS) summarizes that work as suggesting LSD appears to *increase* alpha power once 1/f noise is corrected. That sits uneasily with the new paper's "genuine attenuation". The discrepancy is unresolved and likely reflects different decompositions. On peak frequency, the paper itself says comparable shifts have only "inconsistently" been seen for psilocybin and not for DMT. I could not verify Kometer et al. 2013, Toker et al. 2022 or Atasoy et al. 2017 within this investigation's search budget.

### 4. Methodological critique

**Aperiodic-fit confounds.** Fitting specparam in "fixed" mode across 1–120 Hz is risky. Gerster et al. (*Neuroinformatics*, 2022) show several failure modes:
- the estimated 1/f exponent depends on fitting range;
- high-frequency plateaus break the power law;
- overlapping peaks corrupt aperiodic estimates.

Donoghue et al. (*Nature Neuroscience*, 2020) introduced knee-mode fitting precisely because fixed-mode fits misbehave when spectra bend. Inference: a drug-induced change in slope can move the residual peak's apparent location, especially for broad, multi-peaked beta. A +5 Hz beta "shift" defined as an argmax inside a wide band is more fragile than the +2 Hz alpha shift. The authors partly address this with the 1–45 Hz refit and by noting that peaks were present in nearly all spectra, but no simulation-based recovery test is reported.

**Muscle artefacts.** Muthukumaraswamy's own *Frontiers* review (15 April 2013) notes that high-frequency neural activity overlaps entirely with muscle activity's ~20–300 Hz bandwidth, and that spectral contamination starts around 20 Hz, so it reaches into upper beta as well as gamma. The study had no EMG. Its argument that spatially coherent decoding maps argue against muscle contamination is plausible but indirect: temporal-muscle artefacts are themselves spatially structured.

**Power and degrees of freedom.** With N=17, a new peak definition, a new pipeline and many feature families, the forking-paths risk is real. The main drug effects are large and consistent with prior work. The interaction and brain–experience correlations are underpowered by the authors' own admission.

**The classifier.** Group-wise cross-validation keeps each subject's sessions in the same fold, which addresses the most common leakage. But:
- with about 34 recordings per condition set, each outer test fold holds only about 8–9 samples, so accuracies are coarse;
- the inputs are 360 regional features per model;
- for a within-subject drug contrast that already shows large t-values, accuracies of 68–82% are respectable but unsurprising, and they confirm the group statistics rather than adding independent evidence.

**Replication status.** None independent.

### 5. The fractal–hallucination hypothesis

**Established theory.** Klüver's form constants (tunnels, spirals, lattices, cobwebs) can be derived from V1 architecture. Ermentrout and Cowan (1979) did so first. Bressloff, Cowan, Golubitsky, Thomas & Wiener (*Phil Trans R Soc B*, 2001) extended it using the Euclidean symmetry of V1 and the retino-cortical log-polar map. Butler et al. (*PNAS*, January 2012), not *Neuron* as sometimes cited, argued that V1 is configured to *avoid* Turing-type pattern formation under normal conditions. Billock & Tsou's 2012 *Psychological Bulletin* review ties flicker, migraine and drug hallucinations to shared pattern-forming mechanisms. Their flicker experiments found that fractal (1/f) noise can drive hallucinatory pattern formation.

**Why this does not support the snowflake story.** These models explain *spatial* standing-wave patterns in cortex. The new study measured *temporal* self-similarity of regional MEG time series. Temporal HFD and spatial Turing patterns are different objects.

**The paper's own data cut against it.** Its exploratory, uncorrected correlations show:
- complex imagery was predicted by a flatter slope, lower offset, reduced beta Peak Power and *lower* LZC;
- HFD is not listed among the predictors of imagery;
- reduced alpha and beta Peak Power tracked simple imagery.

Verdict: suggestive analogy, untested here.

### 6. Music and LSD

**Prior work.** Kaelen et al. (*Psychopharmacology*, August 2015; N=10) found LSD enhanced music-evoked emotions such as "wonder" and "transcendence". The new paper also cites:
- Kaelen et al. 2016 on parahippocampal connectivity and imagery, using this dataset;
- Preller et al. 2017 (*Current Biology*) on meaning;
- Barrett, Preller & Kaelen 2018.

Strickland, Garcia-Romeu & Johnson (2021; N=10 psilocybin smoking-cessation patients) found no unique benefit of Western classical over overtone-based music.

**The precedent on this dataset.** Mediano, Rosas et al. (*ACS Chemical Neuroscience*, January 2024) found that external stimulation, music or video, *reduced* LSD's effect on Lempel-Ziv entropy. The new "dampening" trend is a partial re-observation on overlapping data, not independent support.

**A discrepancy.** Mediano et al. describe the music as from the album "Eleusian Lullaby". The new paper says excerpts from "Yearning" by Robert Rich and Lisa Moskow. I could not resolve this.

**How to read it.** Subjective emotional enhancement and spectral "attenuation" are not contradictory. Music may raise emotional salience while anchoring neural dynamics. Subramani told PsyPost the music result "is exploratory and should not be over-interpreted."

### 7. Independent experts

I found no on-record comment from any scientist outside the author team. That includes Voytek, Donoghue, Timmermann, Tagliazucchi, Mediano, Rosas, Barrett and Preller. All coverage I found quotes only Jerbi or Subramani: Nautilus, PsyPost (Eric W. Dolan, 23 October 2026), Medical Xpress, News-Medical, Newswise and Scientific Frontline. Natural critics would be the specparam developers (Voytek, Donoghue), on fixed-mode fitting, and Gerster, Nikulin and colleagues, on fitting-range sensitivity. Altmetric counts dozens of X and Bluesky posts, but their content was not verifiable.

## Verdict Table

| Claim | Verdict | Confidence | What would change it |
|---|---|---|---|
| (1) Alpha ~+2 Hz, beta ~+5 Hz on LSD | Supported (alpha); partly supported (beta) | Moderate–high alpha; moderate beta | Knee-mode or IRASA refits, simulation recovery, EMG-controlled replication |
| "Older studies missed it because of measurement" | Partly supported / overstated | High | Already contradicted: the 2016 PNAS paper reported a 10→12 Hz alpha peak |
| (2) More fractal/scale-free activity | Supported (temporal HFD/LZC) | Moderate–high | Independent dataset; alternative estimators (DFA) |
| (2b) Link to snowflake-like hallucinations | Speculative | High (that it is untested) | Pre-registered correlation of fractal metrics with 5D-ASC visual subscales at peak effect |
| (3) Classifier 68–82% | Supported (as lower bounds); 88% unverified | Moderate | Out-of-dataset validation |
| (4) Music did not boost / may dampen | Unsupported as a finding; trend only | Moderate | Adequately powered factorial study |
| (5) Decade-long "quieting" consensus | Partly supported; already contested | Moderate | — |

## Why It Matters and What to Watch

**Why it matters.** The durable contribution is methodological. Fixed-band power under LSD conflates speed with strength, and an apparent PCC "desynchronization" may be an aperiodic artefact. This should push psychedelic M/EEG studies toward routine spectral parameterization, with fitting choices reported. Inference: if the beta and alpha speed-up is LSD-specific, it could help distinguish LSD from psilocybin and DMT. That matters because an LSD formulation is in late-stage trials: MindMed's MM120 ODT (lysergide D-tartrate, 100 µg) is in three Phase 3 trials, Voyage (generalized anxiety disorder, ~200 US participants, first patient dosed December 2024), Panorama (generalized anxiety disorder, ~250 participants in the US and Europe) and Emerge (major depressive disorder, ~140 US participants), as reported by PharmaTimes (17 December 2024), Psychiatric Times and Medscape; the FDA has granted MM120 Breakthrough Therapy Designation for generalized anxiety disorder.

**What to watch:**
- the group's announced LSD-psilocybin-DMT comparison and its functional-connectivity preprint;
- independent reanalyses of the pharmaco-MEG LSD, psilocybin and ketamine recordings, which an arXiv paper describes as obtained from an open data repository;
- the published peer review file;
- any new study recording MEG with EMG at peak effect, with visual-phenomenology ratings taken concurrently.

## Caveats

- I could not open the Nature HTML page directly. Methods details come from the bioRxiv full text and a subagent's reading of the published PDF.
- Effect sizes, the 88% figure and the aperiodic-feature accuracies are in figures I could not read.
- Several classic citations (Klüver, Kometer, Toker, Atasoy, Rule, Pearson, Schwartzman, Kaelen 2018, Barrett 2020) were not verified within this investigation.

## Sources (harvested)
- https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030,
- https://doi.org/10.1038/s42003-026-10629-7
- https://www.nature.com/articles/s42003-026-10629-7
- https://www.google.com/s2/favicons?sz=64&domain=nature.com
- https://medicalxpress.com/news/2026-10-lsd-desynchronize-brain-rhythms-complexity.html
- https://www.google.com/s2/favicons?sz=64&domain=medicalxpress.com
- https://nouvelles.umontreal.ca/en/article/2026/10/21/lsd-does-more-than-desynchronize-the-brain
- https://www.google.com/s2/favicons?sz=64&domain=umontreal.ca
- https://www.altmetric.com/details/188535542
- https://www.google.com/s2/favicons?sz=64&domain=altmetric.com
- https://nature.com/articles/s42003-026-10629-7.pdf
- https://www.biorxiv.org/content/10.64898/2026.01.28.702361.full.pdf
- https://www.google.com/s2/favicons?sz=64&domain=biorxiv.org
- https://pmc.ncbi.nlm.nih.gov/articles/PMC4855588
- https://www.google.com/s2/favicons?sz=64&domain=nih.gov
- https://www.biorxiv.org/content/10.1101/2025.03.21.644645.full.pdf
- https://www.biorxiv.org/content/10.64898/2026.01.28.702361v1.full
- https://pubmed.ncbi.nlm.nih.gov/42844315/
- https://news.google.com/read/CBMiX0FVX3lxTE03Q3I0aWVWV1duQXMxU2FvLVU3aS11bUhzRWowV1ozOUl0WEdlM1VHWDNoT0FUaGhaYTJ0Zk5fdkZIVzF1Q2ozZE5qOWh6RGk2amJzY0lMa2dSTElsYmsw?hl=en-US&gl=US&ceid=US%3Aen
- https://www.google.com/s2/favicons?sz=64&domain=google.com
- https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030
- https://www.google.com/s2/favicons?sz=64&domain=nautil.us
- https://pubmed.ncbi.nlm.nih.gov/34897383/?dopt=Abstract
- https://consciousnesslibrary.org/authors/58677-manoj-k-doss
- https://www.google.com/s2/favicons?sz=64&domain=consciousnesslibrary.org
- https://pubmed.ncbi.nlm.nih.gov/39020167/
- https://www.researchgate.net/publication/382332140_Psilocybin_desynchronizes_the_human_brain
- https://www.google.com/s2/favicons?sz=64&domain=researchgate.net
- https://www.nature.com/articles/s41591-026-04287-9
- https://www.imperial.ac.uk/psychedelic-research-centre/research/publications/?respub-t4-action=citation.html&id=966044&noscript=noscript
- https://www.google.com/s2/favicons?sz=64&domain=imperial.ac.uk
- https://www.researchgate.net/publication/333978531_Spectral_signatures_of_serotonergic_psychedelics_and_glutamatergic_dissociatives
- https://www.sciencedirect.com/science/article/pii/S1053811920305358
- https://www.google.com/s2/favicons?sz=64&domain=sciencedirect.com
- https://www.researchgate.net/publication/342583907_Serotonergic_psychedelics_LSD_psilocybin_increase_the_fractal_dimension_of_cortical_brain_activity_in_spatial_and_temporal_domains
- https://www.sciencedirect.com/science/article/abs/pii/S1053811918305755
- https://www.biorxiv.org/content/10.1101/2025.03.21.644645v1.full
- https://www.sciencedirect.com/science/article/pii/S187892932400063X
- https://github.com/moritz-gerster/separating_periodic_from_aperiodic_PSDs
- https://www.google.com/s2/favicons?sz=64&domain=github.com
- https://www.nature.com/articles/s41593-020-00744-x
- https://dl.acm.org/doi/abs/10.1162/089976602317250861
- https://www.google.com/s2/favicons?sz=64&domain=acm.org
- https://www.researchgate.net/publication/12018383_Geometric_visual_hallucinations_Euclidean_symmetry_and_the_functional_architecture_of_striate_cortex
- https://www.pnas.org/doi/10.1073/pnas.1118672109
- https://www.google.com/s2/favicons?sz=64&domain=pnas.org
- https://pubmed.ncbi.nlm.nih.gov/22448914/
- https://www.semanticscholar.org/paper/Elementary-visual-hallucinations-and-their-to-Billock-Tsou/aab793c9d737b562b0b8902343c33fc18675be90
- https://www.google.com/s2/favicons?sz=64&domain=semanticscholar.org
- https://pubmed.ncbi.nlm.nih.gov/17470794/
- https://www.semanticscholar.org/paper/33f032dbfd3649b4c6dd7341422b6e112a38772a
- https://link.springer.com/article/10.1007/s00213-015-4014-y
- https://www.google.com/s2/favicons?sz=64&domain=springer.com
- https://www.semanticscholar.org/paper/Set-and-Setting:-A-Randomized-Study-of-Different-in-Strickland-Garcia-Romeu/a8aedc0f5a2a12305ce4c7d95273a24ef083dcbe
- https://www.researchgate.net/publication/348047004_Set_and_Setting_A_Randomized_Study_of_Different_Musical_Genres_in_Supporting_Psychedelic_Therapy
- https://pubs.acs.org/doi/10.1021/acschemneuro.3c00289
- https://www.google.com/s2/favicons?sz=64&domain=acs.org
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10853937/
- https://www.psypost.org/lsd-pushes-brain-activity-toward-a-more-fractal-complex-state-study-finds/
- https://www.google.com/s2/favicons?sz=64&domain=psypost.org
- https://nature.altmetric.com/details/205950063
- https://arxiv.org/pdf/2410.11583
- https://www.google.com/s2/favicons?sz=64&domain=arxiv.org