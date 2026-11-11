# DR26-10-24-HUB-04-0512 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll verify the Nautilus piece against the primary paper and the earlier LSD MEG literature, then write the investigation from sources opened in this run.A. The paper, the dataset, and what Nautilus changed

The study exists. Venkatesh Subramani and colleagues published “LSD reconfigures cortical dynamics through faster brain rhythms and increased fractal dimension” in Communications Biology on 7 October 2026 (volume 9, article 1244; DOI 10.1038/s42003-026-10629-7). The journal record lists receipt on 9 February 2026 and acceptance on 29 June 2026. A preprint of the same title was posted on bioRxiv (DOI 10.64898/2026.01.28.702361); the full text read for the numbers below is version 2, dated 29 May 2026. The published abstract on the Nature page matches that preprint. Where a figure or a sentence lives only in the preprint body, that is stated.

Authors and homes, from the preprint and the published author list reported by PsyPost: Venkatesh Subramani, Timothy Nest, Jérémy Brunel, Yorguin José Mantilla Ramos, Yann Harel and Karim Jerbi (Cognitive and Computational Neuroscience Laboratory, Université de Montréal; Jerbi also Mila and the UNIQUE center); Subramani, Giulia Lioi and Nicolas Farrugia (IMT Atlantique, Lab-STICC, Brest); Annalisa Pascarella (Institute for Applied Mathematics, CNR, Italy); Suresh Muthukumaraswamy (University of Auckland); Robin Carhart-Harris (University of California, San Francisco). Subramani led the analysis. Jerbi is the senior author and the funding holder for the reanalysis.

The recordings are not new. The paper says it reanalyzed magnetoencephalography (MEG) from the Imperial College London LSD study led by Carhart-Harris. That study is “Neural correlates of the LSD experience revealed by multimodal neuroimaging,” PNAS, 26 April 2016 (DOI 10.1073/pnas.1518377113). Twenty healthy volunteers each attended two days, LSD and placebo, at least two weeks apart, in balanced order. Each day had functional MRI and then MEG. The PNAS MEG analyses that were usable covered 14 volunteers. The 2026 paper uses 17. The acknowledgement thanks Kenneth Shinozuka for finding missing data for two subjects. The text read here does not explain, subject by subject, how 14 became 17, or which of the original 20 were dropped. That gap is real.

Design, as restated in the 2026 methods. Within-subject, single-blind, order counterbalanced. Intravenous LSD, 75 micrograms, versus saline. Sessions 14 days apart. MEG on a CTF-275 system at 1200 Hz, bandpass 0–300 Hz, about four hours after injection. The conditions used here are eyes closed in silence, and eyes closed while listening to music, each about seven minutes. The music was two seven-minute excerpts from Yearning, the electro-acoustic ambient album by Robert Rich and Lisa Moskow. After each run, people rated mood, imagery and related items in the scanner. Ethics for the original collection: National Research Ethics Service Committee London–West London, Helsinki 2000, ICH-GCP, NHS research governance, Imperial College as sponsor, Home Office licence for Schedule 1 drugs.

What they actually computed. Continuous data were filtered 1–120 Hz, notched at 50 and 100 Hz, cut into 2-second epochs, cleaned with two passes of Autoreject plus ICA (Picard) aimed at heart, eye and muscle components, then source-localized with dynamic statistical parametric mapping onto individual FreeSurfer surfaces. Empty-room noise recordings were not available, so the noise covariance was an identity matrix. Activity was parcelled into 360 Human Connectome Project regions. Spectra were split into a rhythmic part and a 1/f-like background with spectral parameterization (specparam, formerly FOOOF): fixed aperiodic mode, 1–120 Hz, at most five peaks. They then measured alpha and beta peak frequency, power at those peaks, the aperiodic exponent and offset, Lempel-Ziv complexity (LZC), and Higuchi fractal dimension (HFD). Statistics: cluster permutation along frequency for whole-brain spectra; for regional maps, MNE’s permutation t-test, 50,000 permutations, threshold p < 0.05. Phenomenology regressions used false-discovery-rate control. The music interaction did not.

The five claims, paper versus Nautilus (Kristen French, Nautilus, 22 October 2026, https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030).

Claim 1, rhythms speed up, and older work missed it because of fixed bands. The paper’s result is real and narrower than the headline. In regions that survived permutation correction, alpha peaks sat about 2 Hz higher under LSD and beta peaks about 5 Hz higher (p < 0.05, permutation-corrected). The same analysis says the power drop is not only an artefact of the shift. After the 1/f background is removed, oscillatory power from theta through beta (up to 30 Hz) is still lower, and mid-gamma power (60–90 Hz) is higher, both cluster-corrected at p < 0.05 on the region-averaged spectrum. Fixed canonical bands made the power loss look more widespread. The authors’ phrase is that ignoring peak shifts “inflates estimates of drug effects.” Alpha power loss was strongest in temporal cortex. Beta power loss was broad. In the posterior cingulate, a standard analysis showed a large power drop. Once the aperiodic part was removed, the rhythmic difference there disappeared. So the “controller hub” change, in this recording, was mostly background slope, not a missing alpha bump.

The “older studies missed it” line in Nautilus does not survive the 2016 paper. PNAS Figure 5C, n = 14, the sensor cluster tied to simple hallucinations: placebo alpha peak at 10 Hz, LSD at 12 Hz, t = 4.21, P = 0.0009, alongside a broadband power decrease. The 2026 introduction cites that result as a “subtle” posterior shift and says, correctly, that nobody had mapped the shift across cortex or recomputed power at the new peaks. Nautilus turns a partial, unmapped finding into a miss. That is the clearest distortion in the piece.

Nautilus also calls alpha and beta “the two main rhythms,” alpha the rest rhythm and beta the rhythm of focus and decision-making. Those are textbook glosses. They are not results.

Claim 2, the signal becomes more fractal, with a possible tie to nested visual geometry. LZC and HFD both rose across cortex, strongest posteriorly (highlighted top decile, p < 10^−5 in the figure note). The aperiodic exponent flattened and the offset fell, strongest in posterior cingulate and visual cortex (top decile marked at p < 10^−3). Functional decoding against NiMARE topic maps placed the largest complexity and slope changes in sensory, language, emotion and imagery systems, with motor cortex relatively spared. Spearman correlations between the size of the power drop and the size of the frequency shift were about −0.04 (alpha) and −0.12 (beta), p > 0.05 uncorrected. The authors read that as two effects, not one effect seen twice. Nautilus’s snowflake sentence is metaphor. The paper never mentions snowflakes, Klüver’s form constants, or Romanesco broccoli. Jerbi’s caution, quoted by Nautilus and by the university release, is in the release, not as a result in the paper: they measured fractal structure in time series, not in the images people saw. Nautilus prints that caveat. It still leads with the visual echo.

Claim 3, a classifier separated LSD from placebo at 68–82%. This matches the preprint’s feature-wise numbers, not one model. Random forests on regional oscillatory power exceeded 68% accuracy. Forests on peak frequency exceeded 82%. Forests on LZC and HFD were about 76%. Chance is 50%. Training was nested StratifiedGroupKFold (outer 4, inner 3) with a 1,000-label permutation test for accuracy and feature importance. The grouping variable is not named in the methods text read here. If the group is the person, subject leakage is limited. If it is not, the accuracies are softer. The university release, carried by News-Medical on 22 October 2026, says that combining all measures yields 88%. That 88% figure does not appear in the preprint text. Nautilus stayed with 68–82% and is closer to the paper than the press release is. Nautilus does not say these are separate models, so a reader can think one classifier scored in that band.

Claim 4, music did not boost the LSD effects and may have dampened them. The paper’s hypothesis H3 was that music would potentiate the neural effects. It did not. No region survived whole-brain FDR. Uncorrected maps (p < 0.05) went the other way: steeper slope, higher offset, lower LZC and HFD when music was paired with LSD, relative to LSD in silence. The discussion says this “should be viewed as tentative” and “exploratory.” Nautilus says the result “didn’t hold up under strict statistical analysis, so it’s more of a hint than a fact.” On this claim the article is careful, and more careful than a headline that music “dampened” LSD would have been.

Claim 5, the mainstream view is that psychedelics quiet control centres. Nautilus frames the new work as a replacement: “it’s less that LSD turns off the brain’s controller than that it reshuffles how the whole system talks to itself.” The paper does not say that. Its discussion says the pattern “provides electrophysiological support for hierarchical and entropic models of psychedelic action, including REBUS,” by relaxing structured activity and expanding the repertoire in perceptual and cognitive circuits. The new result sits beside the quieting account. It does not retire it. Nautilus’s “rather than” is the second real overreach. The opening nod to “since at least 2012” and the default mode network is fair as history. The pivot away from that history is not what the authors concluded.

Other Nautilus points that check out: 17 volunteers, two days, saline, MEG, eyes closed, the Rich and Moskow excerpts, the small sample, the reused dataset, and the gamma/muscle caveat in general terms. One sentence does not check out against the paper: “the authors say the patterns they found are likely too organized” to be muscle. The limitations section says there was no concurrent EMG, so muscle activity could not be compared, and that high-gamma peaks are less stable than alpha and beta. It does not clear gamma by appealing to spatial organization.

B. Where this sits in the quieting literature

The “turn the control centres down” story is a stack of findings, not one slogan.

In 2012, Carhart-Harris and colleagues reported that intravenous psilocybin reduced cerebral blood flow and BOLD signal in midline hubs, including anterior and posterior cingulate and thalamus (PNAS, 23 January 2012, DOI 10.1073/pnas.1119598109). That paper is the one Nautilus links. It surprised people who expected a stimulant-like increase in activity.

In February 2014, Carhart-Harris, Leech, Hellyer, Shanahan, Feilding, Tagliazucchi, Chialvo and Nutt published the entropic brain hypothesis (Frontiers in Human Neuroscience, DOI 10.3389/fnhum.2014.00020). The claim: the psychedelic state is a high-entropy “primary” state. Normal waking consciousness suppresses entropy. Entry into that state was tied to a collapse of organized default-mode activity and to a decoupling from medial temporal lobe. Criticality, a regime poised between order and disorder, was offered as the physical picture. Alpha power in posterior cingulate was treated as one correlate of “ego integrity.”

In 2013, Muthukumaraswamy, Carhart-Harris and colleagues recorded MEG after intravenous psilocybin (Journal of Neuroscience, 18 September 2013, DOI 10.1523/JNEUROSCI.2063-13.2013). Spontaneous power fell from about 1–50 Hz in posterior association cortex and from about 8–100 Hz frontally, with large drops in default-mode regions. Dynamic causal modelling of posterior cingulate pointed to greater excitability of deep pyramidal cells, which carry dense 5-HT2A receptors. Visually induced and movement-induced gamma were spared. The interpretation was desynchronization from excitation, not a global shutdown.

The 2016 LSD MEG study repeated the broadband power loss and tied decreased visual-cortex alpha to simple hallucinations, and decreased delta and alpha to ego-dissolution. It also, as noted, saw the alpha peak move from 10 to 12 Hz in one cluster.

In April 2017, Schartner, Carhart-Harris, Barrett, Seth and Muthukumaraswamy reported higher spontaneous MEG signal diversity under psychoactive doses of ketamine, LSD and psilocybin, including after controls for the spectrum (Scientific Reports, DOI 10.1038/srep46421). Single-channel Lempel-Ziv rose most. For normalized Lempel-Ziv, the LSD increase held in 93% of participants. That is the direct ancestor of the 2026 LZC result, on overlapping data. The 2026 paper is not the first report that this LSD MEG is more complex.

In November 2019, Timmermann and colleagues recorded EEG during intravenous DMT (Scientific Reports, DOI 10.1038/s41598-019-51974-4; 13 volunteers, analyses often n = 12). Alpha and beta power fell. Lempel-Ziv rose (max t(11) = 8.09 for the diversity contrast). They separated fractal and oscillatory spectra with IRASA. Delta and theta tracked the visual peak of the trip. They did not report an alpha peak acceleration of the LSD kind. The 2026 paper uses that absence, plus inconsistent psilocybin peak findings, to suggest LSD may be different. That suggestion is comparative across studies with different drugs, doses, modalities and analyses. It is not a head-to-head test.

In June 2019, Carhart-Harris and Karl Friston published REBUS, “relaxed beliefs under psychedelics,” in Pharmacological Reviews (DOI 10.1124/pr.118.017160). Psychedelics, via 5-HT2A agonism and an entropic effect on cortex, are argued to loosen the precision of high-level priors, especially those linked to the default mode network, and to free bottom-up signalling. Reduced alpha and beta are part of the proposed mechanism, because those rhythms are treated as carriers of top-down predictions. The 2026 discussion explicitly hooks weaker rhythms to looser priors and faster rhythms to stronger ascending flow, and then says the data fit REBUS and do not prove it. Daily Neuron’s 20 October 2026 write-up is closer to the paper on this point than Nautilus is.

A 2020 fMRI reanalysis by Varley, Carhart-Harris, Roseman, Menon and Stamatakis (NeuroImage, DOI 10.1016/j.neuroimage.2020.117049) found higher fractal dimension of cortical connectivity networks under both LSD and psilocybin, and higher Higuchi fractal dimension of BOLD time series under LSD (Wilcoxon, p = 0.001), with a non-significant psilocybin trend. BOLD fractal changes localized toward the dorsal attention network. That is fractal structure in slow hemodynamics, not in MEG. It makes the 2026 HFD result an extension to a faster signal, not a first sighting of “fractal LSD.”

In December 2017, Atasoy, Roseman, Kaelen and colleagues decomposed the same LSD fMRI era’s data into connectome harmonics (Scientific Reports, DOI 10.1038/s41598-017-17546-0). LSD expanded the repertoire of active harmonics, pushed energy upward, fit power laws more closely, and moved dynamics toward criticality. Music increased that energy shift further, and the music effect reversed faster under LSD than under placebo. That music result points the opposite way from the 2026 MEG trend. Different measure, different modality, same drug, same broad programme.

Read against this stack, the 2026 paper does three things that are new, and several that are not. New: a cortex-wide map of alpha and beta peak shifts. New: a demonstration that alpha and beta power are still lower when power is taken at the shifted peak and the 1/f floor is removed. New: the two maps do not track each other, so “desynchronization” and “acceleration” should not be treated as one number. Not new: lower alpha/beta power, flatter spectra, higher Lempel-Ziv, a motor cortex that is relatively spared, and a gesture at criticality or entropy. The posterior-cingulate nuance is the sharpest revision of the old MEG story. The hub did not lose its rhythm so much as lose the slow tilt of its background.

C. Methods, power, and the classifier

Spectral parameterization is the right tool for the question the authors asked. Donoghue, Voytek and colleagues showed in Nature Neuroscience (23 November 2020, DOI 10.1038/s41593-020-00744-x) that a fixed band mixes peak frequency, peak power, bandwidth, aperiodic offset and aperiodic exponent, and that those can move independently. An upward peak on a downward 1/f slope looks like a power loss if you only integrate a preset window. The 2026 design is a direct answer to that confound. It is not immune to it. Specparam in fixed mode assumes one straight exponent. Peaks can be missed, split, or fit as part of the slope, especially when several bumps sit in alpha, which the authors say they saw often. They defined “the” peak as the tallest bump in the 1/f-removed spectrum. Goodness-of-fit is deferred to a supplement. An alternative, IRASA, was used in the DMT EEG paper and was not the primary split here. Agreement between the two algorithms on this dataset is unreported in the text read here.

Muscle and movement. Muthukumaraswamy’s 2013 review (Frontiers in Human Neuroscience, DOI 10.3389/fnhum.2013.00138) states that neural gamma (about 30–80 Hz and above) overlaps muscle activity from about 20–300 Hz, and that non-invasive high-frequency reports are often contaminated. The 2026 pipeline removed ICA components labelled muscular, cardiac or ocular. There was no EMG electrode, so residual cranial muscle cannot be measured. LSD can increase jaw tension, fidgeting and eye movement under closed lids. Alpha and beta are less vulnerable than gamma, but beta is not clean. The mid-gamma increase (60–90 Hz) is the result most exposed to this artefact. The authors treat high-gamma as less reliable than alpha and beta. A reader should too. Nautilus’s reassurance that the pattern is “too organized” to be muscle is stronger than the paper.

Eyes and the late window. Scans were about four hours after injection. Subjective ratings referred back to the peak, which the limitations section places about 150 minutes before the MEG. The authors say this timing likely adds noise and helps explain weak brain–experience correlations. Global, uncorrected associations are reported in the supplement: lower alpha and beta peak power with simple imagery; flatter slope, lower offset, lower beta power and lower LZC with complex imagery; ego-dissolution with beta peak shift and LZC. Intensity, emotional arousal and positive mood were not predicted by the main drug effect. Only emotional arousal tracked the music interaction, and that test was uncorrected. These are hints. They are not a validated neural readout of the trip.

N = 17 is the constraint the authors name. A crossover helps, because each person is their own control, but interaction tests need more people than main effects. The music result is the one they flag as underpowered. No Cohen’s d or partial-eta-squared for the peak shifts appears in the sections read here. “About 2 Hz” and “about 5 Hz” are the effect sizes on offer, averaged over significant regions, not over the whole cortex. A 2 Hz alpha shift is large relative to ordinary peak-frequency wobble, and it matches the 2016 cluster (10 to 12 Hz). It still comes from one cohort.

Researcher degrees of freedom. This is a reanalysis, received in 2026, of a 2016 dataset that has already yielded PNAS, signal-diversity, music-fMRI, connectome-harmonic and entropy papers. The 2026 paper states three questions and three hypotheses before the results, which is better than a silent fish. It does not mention a preregistration or a registered analysis plan. None was found in the paper. Choices that can move the maps include the parcel atlas, the specparam settings, the peak rule inside alpha, the 1–120 Hz fit range, the identity noise covariance, the ICA rejection rule, and which of several complexity metrics to trust. The supplement compares a 1–45 Hz fit. That is a sensitivity check, not a lock. The honest status is: prespecified in the manuscript, not locked before the data were reopened.

The classifier. Random forests on regional features, nested cross-validation, label shuffles. That is a serious setup, and 68–82% on n = 17 sessions-per-class is not a clinical test. It is a compactness check: the same maps that the t-tests like are also what the forest uses. Feature-importance maps “closely resembled” the statistical maps, which is reassuring and partly circular, because both are driven by the same mean difference. Unknowns that would change a reader’s confidence: the grouping key inside StratifiedGroupKFold; whether both sessions of one person could inform each other; class balance after dropped epochs; and whether accuracy was computed on held-out people or held-out epochs. The preprint reports AUC-ROC only as a supplement figure, not as a number in the text read here. The 88% “all measures combined” line exists in the university release and in Medical Xpress. It is not in the paper text reviewed here. Until the published supplement shows it with the same cross-validation, it should not be quoted as a result.

Replication status. There is no second cohort. The peak-frequency map has not been repeated on a new LSD sample, or on psilocybin or DMT with the same pipeline. The authors say they are extending the approach to connectivity and to other drugs. That work was not published as of the sources read on 23 October 2026. A parallel preprint using a different method on LSD MEG (Shinozuka and colleagues, bioRxiv 2025, “LSD reconfigures the frequency-specific network landscape”) was visible in search and was not read in full in this run, so its numbers are not used here. It is relevant only as a sign that other groups are already on this same recording.

D. Fractal time series and geometric hallucinations

Two different “fractals” are being stacked.

Higuchi fractal dimension asks whether a waveform looks statistically similar when you zoom in and out in time. A higher value means a rougher, more scale-free trace. Lempel-Ziv counts how many new patterns appear. A 1/f exponent near zero means the spectrum is less dominated by slow fluctuations. None of these is a picture of a spiral on the retina.

The geometric hallucinations people report on LSD, mescaline and psilocybin were grouped by Heinrich Klüver into form constants: tunnels and funnels, spirals, lattices and honeycombs, cobwebs. Paul Bressloff, Jack Cowan and colleagues showed that if primary visual cortex is modelled as a sheet of orientation columns with symmetric lateral connections, the patterns that appear when a uniform resting state goes unstable, once passed through the retino-cortical map, match those four families (Philosophical Transactions of the Royal Society B, 2001, DOI 10.1098/rstb.2000.0769; Neural Computation, March 2002, DOI 10.1162/089976602317250861). The mechanism is spatial pattern formation in V1, of the same family as stripes in physical media. It does not require the MEG time series to have a high fractal dimension. It requires a change in cortical gain or inhibition so that a spatially uniform state loses stability.

The 2026 maps do put the largest HFD, LZC and slope changes toward visual and imagery cortex, and the 2016 study tied visual-cortex blood flow, connectivity and alpha power to hallucination ratings. That is a geographical coincidence worth testing. It is not a bridge. A bridge would show that the spatial scale of V1 activity, or the wavelength of travelling waves, shifts into the hypercolumn range the Bressloff model uses, and that this shift tracks form-constant ratings in the same minute. The 2026 study averages seven minutes, has no eye-tracking of closed-eye imagery, and correlates fractal metrics with questionnaire imagery only in uncorrected global tests. Jerbi’s public line is the right one: the hypothesis is open, and the measurement does not confirm it.

Scale-free temporal statistics are also easy to over-read as “the brain became a snowflake.” Many non-fractal processes raise HFD, including a simple drop in rhythmicity. The authors checked one version of that worry: band-limited LZC rose in beta and low gamma and did not rise in alpha, so the complexity increase is not just “less alpha to compress.” That is a useful control. It still does not identify self-similar geometry in vision.

E. Music

The behavioural and fMRI literature says music and LSD amplify each other at the level of feeling and of some circuits.

Kaelen, Barrett, Roseman, Carhart-Harris and colleagues (Psychopharmacology, October 2015, DOI 10.1007/s00213-015-4014-y) had ten volunteers listen to instrumental tracks on a placebo day and an LSD day. LSD increased music-evoked emotion, especially wonder, transcendence, power and tenderness. The order was fixed, placebo then LSD, so expectancy is not fully controlled.

Kaelen and colleagues (European Neuropsychopharmacology, July 2016, DOI 10.1016/j.euroneuro.2016.03.018) scanned twelve volunteers after 75 µg intravenous LSD or placebo, eyes closed, with and without music. The LSD-by-music interaction increased parahippocampal connectivity and information flow into visual cortex, and that flow correlated with eyes-closed imagery, including autobiographical imagery. Frederick Barrett is a co-author. This is the study Nautilus’s “music should boost LSD” intuition comes from, and it is about connectivity and reported imagery, not about peak frequency or Higuchi dimension.

Atasoy’s 2017 harmonic analysis, above, found music pushed LSD brain dynamics toward higher-energy states more than placebo.

Mediano, Rosas, Timmermann, Kaelen, Seth, Muthukumaraswamy, Carhart-Harris and colleagues (ACS Chemical Neuroscience, 7 February 2024, DOI 10.1021/acschemneuro.3c00289) returned to the Imperial LSD MEG and varied context: eyes closed or open, silence, music, or video. Lempel-Ziv rose under LSD in every condition and rose most with eyes closed. Stronger external input reduced the LSD-versus-placebo gap. A linear mixed model gave a negative drug-by-condition interaction at p < 0.01. Subjective ratings tracked entropy best when eyes were closed, and the link loosened during video, which they describe as competition between outside stimulation and endogenous imagery.

The 2026 music trend is the same direction as Mediano 2024, on the same recordings, with a stricter multiple-comparison bar that the interaction fails. It is the opposite direction from Kaelen’s fMRI imagery effect and from Atasoy’s energy effect. Those are not contradictions until someone shows they measure the same thing. Emotion and imagery can rise while a scalar “complexity” score falls, if music organizes a signal that silence leaves roaming. The 2026 authors offer that reading and label it a hypothesis. A further limit they state: genre preference was checked in a different group, not in these 17. One seven-minute average also flattens a piece of music that changes every bar.

F. Who has spoken, who would push back, who paid

Independent commentary on this specific paper, in the sources opened for this investigation, is thin. The quotes in Nautilus, PsyPost (Eric W. Dolan, 23 October 2026), the University of Montreal release, EurekAlert, Medical Xpress and News-Medical are from Karim Jerbi and Venkatesh Subramani. Jerbi posted on X on 24 October 2026 that the Nautilus piece was “great coverage,” which is an author endorsing the write-up, not an outside review. No methods critique from a lab that did not touch the data was found in the pages opened. That absence is a finding about the coverage, not evidence that the paper is uncontested.

People whose published work makes them the natural critics, and who are not quoted on this paper in the sources read here: Suresh Muthukumaraswamy (Auckland), on muscle artefact and on whether gamma and beta survive EMG, except that he is a co-author, so he is inside the paper. Pedro Mediano (and Fernando Rosas) on whether a music-related drop in complexity is the same phenomenon they already reported. Christopher Timmermann (Imperial, now broader DMT EEG work) on whether an alpha/beta acceleration shows up when DMT or psilocybin is run through specparam. Thomas Donoghue and Bradley Voytek (UC San Diego) on whether fixed-mode specparam is stable when alpha splits into two peaks. Paul Bressloff (Utah) and the Cowan line on whether any of this constrains V1 pattern formation. Anil Seth (Sussex), a co-author of the 2017 diversity paper, on whether higher Lempel-Ziv should be called a higher “level” of consciousness. None of those critiques was found as a published reaction to the October 2026 paper.

Conflicts and money, from the paper. Carhart-Harris reports scientific advice to TRYP Therapeutics, Osmind, Otsuka and Red Light Holland. The other authors declare none. Jerbi is supported by a Canada Research Chair (950-232368) and an NSERC Discovery Grant (2021-03426). The original recordings were paid for by the Safra Foundation, the Beckley Foundation through the Beckley–Imperial programme, and the Walacea crowdfunding campaign. Amanda Feilding’s Beckley Foundation funded and, on earlier papers from this programme, co-authored the acquisition. She is not an author of the 2026 paper. The reanalysis is computationally new. The data are not independent of the lab that built REBUS.

Data access. The paper says queries go to Muthukumaraswamy and Carhart-Harris. Code is at https://github.com/venkateshness/specparam_LSD. The MRI portion of the 2016 study is on OpenNeuro as ds003059 (version 1.0.0, 7 August 2020, MRI only, 15 participants, CC0). The MEG used here is not in that deposit.

G. Verdict table

Alpha and beta speed up by about 2 Hz and about 5 Hz across significant cortex. Supported, on this dataset. Confidence: moderate. The direction and the alpha magnitude agree with the 2016 cluster (10 to 12 Hz). What would move it: a preregistered replication in a new sample, with effect sizes and a pre-set peak rule. A failure under IRASA or under a different atlas would downgrade it.

The power drop is genuine after the peak shift and the 1/f floor are removed. Supported for alpha and beta in this reanalysis. Confidence: moderate. What would move it: EMG-controlled beta, and a demonstration that the specparam fit, not the drug, is not creating the residual peak. The posterior-cingulate “rhythm loss” being mostly aperiodic is supported here and is a real revision of how that hub was described.

Older studies missed the acceleration because they used fixed bands. Unsupported as stated. Partly supported if restated: fixed bands inflate the spatial extent of the power loss, and the cortex-wide peak map is new. The 2016 PNAS already reported a 2 Hz alpha shift in the hallucination cluster. Confidence in this correction: high.

The acceleration is unique to LSD. Speculative. Confidence: low. The paper says “may differentiate” and treats the dopamine-affinity and receptor-kinetics story as one plausible reading. No within-study psilocybin or DMT arm.

Brain activity becomes more fractal and more complex. Supported for HFD and LZC on this MEG, and consistent with Schartner 2017 and with Varley’s fMRI fractal result. Confidence: moderate for the direction, lower for any claim that the signal is scale-free in a strict physical sense. What would move it: surrogate tests that preserve the spectrum, published in full, and a second cohort.

That fractal change explains snowflake or Klüver geometry. Speculative. Confidence: low. The authors and Jerbi say so. A V1 spatial-pattern study with concurrent imagery ratings would be the test. A time-series fractal dimension cannot confirm it.

Classifier accuracy 68–82%. Partly supported. Those figures match separate feature families in the preprint, not one advertised model. The 88% combined figure is a press-release number not found in the paper text read here. Confidence: moderate that the states are linearly and non-linearly separable above chance in this sample; low that the percentage will travel. What would move it: subject-wise locked cross-validation on a new cohort, published confusion matrices.

Music dampened LSD’s neural effects. Unsupported as a confirmed result. The uncorrected trend is real and points to attenuation, and it agrees with Mediano 2024 on entropy. It did not survive correction. Confidence that there is no robust interaction in this sample: moderate. Confidence that music truly dampens the pharmacology: low.

LSD works by reshuffling the whole system rather than quieting control hubs. Unsupported as a replacement. Partly supported as an addition: power loss, peak shift, slope and complexity are spatially organized, and the posterior cingulate’s apparent rhythm loss was largely aperiodic. The paper still aligns itself with REBUS and the entropic account. Confidence: high that Nautilus over-rotated the conclusion; moderate that the mechanism is broader than “DMN off.”

H. Why it matters, and what to watch

The useful piece is methodological, and it is easy to miss under the fractal headline. If a rhythm moves up a sloping spectrum, a fixed-band analysis will report a loss of power that is partly a loss of aim. Psychedelic EEG and MEG papers since 2013 have leaned on “alpha and beta desynchronization” as the electrophysiological signature, and REBUS has used that signature as evidence that top-down predictions loosen. The 2026 analysis says the loosening is still there when you aim at the new peak, and that some of the headline map, including a flagship hub, was background slope. That is a reason to re-fit old spectra, not a reason to announce a new theory of hallucinations.

It also matters because therapy protocols are built on music. The Kaelen studies are why playlists sit at the centre of psychedelic sessions. A MEG complexity score that trends down under music does not say playlists fail. It says “more signal diversity” and “more emotional imagery” are not the same outcome. Trials that use neural complexity as a target, or as a biomarker of a “deeper” session, need to decide which one they mean before they randomize soundtrack against silence.

What would change the story next, in order:

A public MEG deposit. The code is public. The traces are not. The MRI is already on OpenNeuro. Without the MEG, the peak maps cannot be refit by a lab that does not need permission from the original investigators.

A preregistered replication with EMG, EOG and a prespecified specparam or IRASA pipeline, at the time of peak drug effect rather than 150 minutes later. N greater than 17, powered for the interaction, not only the main effect.

The same pipeline on psilocybin and DMT, which the Montreal group says it is doing. If the peak shift appears only with LSD, the dopamine and kinetics story becomes a hypothesis worth a receptor study. If it appears in all three, the “LSD signature” line should be retired.

A real test of the visual hypothesis: spatially resolved visual-cortex activity, form-constant ratings in time, and a model comparison against Bressloff–Cowan planforms. Fractal dimension of the MEG trace is not that test.

Watch the press-release number. If 88% keeps circulating without a supplement table, it will outlive the 68, 76 and 82 that the paper actually ties to named features.

Coverage note, inside this report so it is not lost. The published HTML on Nature was opened and matched the abstract. Figure-level statistics were taken from the 29 May 2026 preprint full text, then checked against PsyPost’s 23 October account of the published paper and against the university release. If the version of record changed a p-value in the supplement, that page was not in the text extracted here. No preregistration was found. No independent expert was found on the record in the sources opened. Unknowns are marked above rather than filled.

Sources

https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030

https://www.nature.com/articles/s42003-026-10629-7

https://www.biorxiv.org/content/10.64898/2026.01.28.702361v2.full

https://doi.org/10.1073/pnas.1518377113

https://www.pnas.org/doi/suppl/10.1073/pnas.1518377113/suppl_file/pnas.1518377113.sapp.pdf

https://www.psypost.org/lsd-pushes-brain-activity-toward-a-more-fractal-complex-state-study-finds/

https://dailyneuron.com/lsd-alpha-beta-brain-waves/

https://www.news-medical.net/news/20261022/LSD-speeds-up-brain-waves-and-increases-neural-complexity-study-finds.aspx

https://medicalxpress.com/news/2026-10-lsd-desynchronize-brain-rhythms-complexity.html

https://www.eurekalert.org/news-releases/1148738

https://x.com/karimjerbineuro/status/2113806440569954515

https://www.frontiersin.org/journals/human-neuroscience/articles/10.3389/fnhum.2014.00020/full

https://pharmrev.aspetjournals.org/article/S0031-6997%2824%2901296-1/fulltext

https://doi.org/10.1124/pr.118.017160

https://pmc.ncbi.nlm.nih.gov/articles/PMC6618409/

https://pubmed.ncbi.nlm.nih.gov/24048847/

https://www.nature.com/articles/s41598-019-51974-4

https://www.nature.com/articles/srep46421

https://pmc.ncbi.nlm.nih.gov/articles/PMC5396066/

https://www.sciencedirect.com/science/article/pii/S1053811920305358

https://pubmed.ncbi.nlm.nih.gov/32619708/

https://www.nature.com/articles/s41598-017-17546-0

https://www.nature.com/articles/s41593-020-00744-x

https://pmc.ncbi.nlm.nih.gov/articles/PMC8106550/

https://pubmed.ncbi.nlm.nih.gov/23596409/

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3625857/

https://pubmed.ncbi.nlm.nih.gov/38214686/

https://www.sciencedirect.com/org/science/article/pii/S1948719324001403

https://link.springer.com/article/10.1007/s00213-015-4014-y

https://pubmed.ncbi.nlm.nih.gov/26257162/

https://pubmed.ncbi.nlm.nih.gov/27084302/

https://direct.mit.edu/neco/article/14/3/473/6598/What-Geometric-Visual-Hallucinations-Tell-Us-about

https://pmc.ncbi.nlm.nih.gov/articles/PMC1088430/

https://openneuro.org/datasets/ds003059/versions/1.0.0/metadata

https://github.com/venkateshness/specparam_LSD

https://doi.org/10.1073/pnas.1119598109

🧒 Простыми словами

Учёные заново посчитали старые записи мозга 17 человек под ЛСД. Ритмы правда стали чуть быстрее, и сигнал стал более «рваным». Журнал Nautilus в целом пересказал это честно, но в двух местах приукрасил. Он написал, будто раньше ускорение никто не замечал. Замечали, ещё в 2016 году, просто не разложили по всей коре. И он написал, будто препарат не «выключает дирижёра» в мозге, а делает что-то совсем другое. Сами авторы так не говорят. Они говорят, что старая теория всё ещё подходит, а новая деталь её уточняет. Связь с узорами перед глазами, как у снежинки, пока догадка. Музыка эффект не усилила, но это слабый намёк, а не доказанный факт.

🏛M · Grok 4.7 · scripts
