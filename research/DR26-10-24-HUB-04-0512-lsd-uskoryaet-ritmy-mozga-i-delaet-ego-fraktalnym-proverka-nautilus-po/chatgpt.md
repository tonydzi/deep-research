# DR26-10-24-HUB-04-0512 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# LSD, Faster Rhythms, and the “Fractal Brain”: What the 2026 Re-analysis Actually Shows

## Executive verdict

The October 7, 2026 *Communications Biology* paper behind the *Nautilus* story is real, peer-reviewed, technically serious, and more interesting than a simple “LSD speeds up the brain” headline. Venkatesh Subramani, Karim Jerbi and colleagues re-analysed magnetoencephalography recordings from 17 participants drawn from the Imperial College London LSD experiment first reported by Robin Carhart-Harris and colleagues in *PNAS* in 2016. The new analysis separates oscillatory peaks from the broadband aperiodic spectrum, localizes effects across 360 cortical parcels, measures Lempel–Ziv complexity and Higuchi fractal dimension, and applies nested-cross-validated Random Forest classifiers. citeturn955851view5turn604882view4

Its central electrophysiological result is credible within this dataset: LSD both **weakens alpha- and beta-band oscillatory power and shifts their peak frequencies upward**, by roughly 2 Hz and 5 Hz respectively in regions that passed the authors' permutation-corrected significance test. At the same time, the broadband 1/f spectrum flattens and two measures of temporal complexity, Lempel–Ziv complexity and Higuchi fractal dimension, increase. citeturn955851view1turn470430view1

But the strongest journalistic correction is this: **the alpha-frequency acceleration was not “missed” by the original study**. Carhart-Harris et al. explicitly reported in 2016 that the relevant posterior alpha peak shifted from 10 Hz under placebo to 12 Hz under LSD, with *t*=4.21 and *P*=0.0009. The 2026 paper itself acknowledges that faster alpha and beta peaks had been visible in earlier spectra; its real advance is systematic, source-resolved quantification after separating periodic from aperiodic activity—not first discovery of the phenomenon. citeturn460259view1turn955851view3

The “fractal hallucination” story is much weaker. Increased Higuchi fractal dimension is a result about the **temporal geometry of MEG time series**, whereas Klüver-type lattices, spirals and tunnels are generally modeled as **spatial pattern formation in visual cortex**. There is a plausible theoretical bridge, but this experiment does not demonstrate that one causes the other, and neither the paper nor Jerbi claims that it does. citeturn470430view1turn697770search0turn856612view1

The music result is weaker still. No LSD×music interaction survived whole-brain multiple-comparison correction. What remains is an uncorrected trend suggesting that music may partly attenuate the LSD-related flattening and complexity increase. The paper explicitly calls this exploratory, and *Nautilus* accurately tells readers that it is “more of a hint than a fact.” citeturn955851view3turn856612view1

| Claim | Verdict | Confidence | What would change the verdict |
|---|---|---:|---|
| LSD shifts alpha peaks upward by ~2 Hz and beta by ~5 Hz | **Supported in this dataset** | High for within-sample effect; moderate for generalization | Independent, preregistered MEG/EEG replication with individual peak tracking and larger N. citeturn955851view1 |
| Older studies “missed” the acceleration | **Misleading / partly false** | High | None for alpha: the 2016 paper explicitly reported 10→12 Hz. Novelty is systematic spatial quantification and beta characterization. citeturn460259view1turn955851view3 |
| LSD makes brain activity “more fractal” | **Supported operationally** | Moderate-high | Replication with multiple fractal estimators, frequency-resolved controls and explicit sensitivity analyses. citeturn470430view1 |
| Increased fractality explains snowflake-like hallucinations | **Speculative** | High confidence that evidence is insufficient | Trial linking time-resolved visual phenomenology to spatial cortical pattern dynamics and fractal metrics within subjects. citeturn856612view1turn697770search0 |
| ML separates LSD from placebo at 68–82% | **Supported, with caveats** | Moderate | External cohort validation; current estimates come from only 17 people. citeturn604882view0turn955851view4 |
| Music dampens LSD's neural effects | **Unsupported as a corrected statistical finding; suggestive as exploration** | High | A powered, preregistered interaction study with individualized music and time-resolved analyses. citeturn955851view3 |
| Psychedelic neuroscience's mainstream model is simply “quieting control centres” | **Oversimplified** | High | The modern literature already incorporates entropy, network desegregation and relaxed hierarchical priors rather than global suppression. citeturn856612view4turn604882view1 |

## The paper under the microscope

The paper, **“LSD reconfigures cortical dynamics through faster brain rhythms and increased fractal dimension,”** by Subramani, Nest, Pascarella, Brunel, Mantilla Ramos, Harel, Muthukumaraswamy, Carhart-Harris, Lioi, Farrugia and Jerbi, was received February 9, accepted June 29 and published October 7, 2026 in *Communications Biology*. Jerbi's group is based principally at Université de Montréal, with Mila among the affiliations; Muthukumaraswamy is at the University of Auckland and Carhart-Harris at UCSF. citeturn604882view4turn470430view1

The underlying experiment was not new. It came from the landmark Imperial College LSD study published April 11, 2016 in *PNAS*. Participants attended two sessions 14 days apart in a counterbalanced, within-subject, single-blind design, receiving 75 μg intravenous LSD on one day and saline placebo on the other. The 2026 re-analysis used MEG from 17 participants, specifically seven-minute eyes-closed recordings in silence and seven-minute eyes-closed recordings with music, obtained roughly four hours after injection with a 275-channel CTF MEG system sampled at 1,200 Hz. citeturn955851view5turn856612view2

The difference between “17 participants” now and “14 suitable for MEG” in the original *PNAS* paper reflects the fact that later analyses have used additional conditions or usable recordings from the same parent experiment; the original published resting-state MEG power analysis had 14 participants. The important point is that this is not an independent replication cohort—it is a deeper re-analysis of a decade-old dataset. citeturn460259view1turn126466search2

Preprocessing was considerably more elaborate than in 2016. The 2026 pipeline filtered 1–120 Hz, notched 50 and 100 Hz, split data into two-second epochs, used visual inspection plus two passes of Autoreject, and performed Picard ICA to identify cardiac, ocular and muscular components. About 200 epochs per run remained on average, with no statistically significant difference in epoch retention between LSD and placebo. Source reconstruction used individual MRI anatomy, FreeSurfer, a boundary-element forward model and cortical parcellation; because no empty-room noise recordings existed, the authors used an identity noise-covariance matrix. citeturn470430view4

The decisive analytical change was **spectral parameterization**. Instead of assuming that all power between, say, 8 and 13 Hz represents “alpha,” the authors separated narrowband oscillatory peaks from the aperiodic 1/f-like background. That distinction matters because a peak that moves from 10 to 12 Hz can create an apparent loss of power at one fixed frequency even if its amplitude has not fallen proportionally. Modern methods such as FOOOF/specparam and IRASA were designed precisely to separate oscillatory structure from aperiodic background activity, although neither method is assumption-free. citeturn774156search0turn774156search7turn774156search4

After parameterization, the study still found genuine peak-amplitude reductions in alpha and beta, so the older “desynchronization” result was not simply an analytical illusion. What changed was its spatial extent: conventional band-power analyses produced broader suppression, whereas peak-based analyses—accounting for moving peaks and the 1/f background—left a more selective pattern. In particular, posterior-cingulate effects apparent in total spectral power disappeared after periodic and aperiodic components were separated, implying that part of the classic PCC signal reflected broadband rather than oscillatory change. citeturn604882view0

For the headline frequency result, the authors compared paired LSD–placebo peak frequencies across cortical regions with 50,000 permutation iterations and report corrected *P*<0.05. Their plotted ~2 Hz alpha and ~5 Hz beta shifts are averages over **regions already passing the significance mask**, a detail worth preserving because “across large swaths of cortex” can sound like an unbiased whole-cortex mean. citeturn955851view1turn470430view2

There is also a reporting oddity: the methods say the null distribution was obtained by “permuting the regions” using MNE's `permutation_t_test`. Standard paired permutation logic would normally sign-flip observations while correcting across tests; the wording is ambiguous enough that the exact implementation deserves inspection in the released code rather than inference from the prose alone. This is a transparency issue, not evidence that the test was invalid. citeturn470430view2turn470430view5

The fractality analysis used Higuchi fractal dimension, computed epoch by epoch with an empirically chosen \(K_{\max}=18\), alongside Lempel–Ziv complexity. HFD increased under LSD, with the largest effects in posterior and sensory-association territories; LZC also rose. The two measures are related but not interchangeable: LZC indexes sequence diversity/compressibility, while HFD estimates scale-dependent irregularity of a time series. citeturn470430view1

The machine-learning section is better controlled than a superficial reading might suggest. Random Forest models used nested `StratifiedGroupKFold` cross-validation—four outer folds, three inner folds for hyperparameter selection—and repeated the full procedure under 1,000 label permutations. Single-feature families produced >68% accuracy for spectral-power measures, >82% for peak-frequency measures and >76% for complexity measures; a combined model was reported separately by the university as reaching about 88%. citeturn955851view4turn604882view0turn431330search0

That protects against the most elementary train/test leakage, but it does not solve the harder problem: **17 people are 17 biological units**. Cross-validation repeatedly resamples the same small cohort; it does not create external generalizability. An early peer-review version was explicitly criticized for lacking an independent test set and for only moderate classification accuracy, after which the authors strengthened the validation procedure. The revised nested design is defensible for an exploratory biomarker analysis, but an external holdout cohort is still needed before 68–88% should be described as a robust state detector. citeturn205175view0turn955851view4

I found no preregistration identified in the article, reporting summary or code-availability statement. The paper does state three explicit hypotheses, but those hypotheses appear in a retrospective re-analysis of pre-existing data; absence of a registration link does not prove that no prior plan existed, so the appropriate label is **not demonstrably preregistered**, rather than “unregistered.” The code is public; the raw dataset is not openly deposited, and data requests are directed to Muthukumaraswamy and Carhart-Harris. citeturn955851view6turn470430view5

## What *Nautilus* gets right—and where it overreaches

Kristen French's October 22 *Nautilus* article is, overall, unusually careful for a popular account. It correctly identifies the 17-person re-analysis, correctly gives the approximate +2 Hz alpha and +5 Hz beta shifts, correctly explains that separating rhythmic peaks from broadband background changes the interpretation of fixed-band power, and explicitly warns that the music interaction failed strict statistical correction. It also quotes Jerbi cautioning that temporal fractality in MEG does not demonstrate fractal visual perception. citeturn856612view1

The largest overstatement is the sentence that **“Older studies had not picked up this change in rhythm.”** The 2016 *PNAS* paper could hardly be clearer: the posterior alpha peak was “10 Hz under placebo, 12 Hz under LSD,” with *P*=0.0009. The 2026 authors themselves write that evidence for increases in alpha and beta frequency “has been noted in the PSDs across few studies” but had not been systematically quantified or spatially characterized. The fair formulation is therefore: **previous work noticed at least the alpha acceleration; the new paper makes peak acceleration a principal, cortex-wide analytical target and adds a much stronger beta result.** citeturn460259view1turn955851view3

A second problem is conceptual framing. *Nautilus* says that since roughly 2012 the field has viewed psychedelics as “turning the dial down” on control centres such as the default-mode network. That captures a historically important result but compresses more than a decade of theory into a misleading cartoon. The 2012 psilocybin work did report reduced blood flow/BOLD and reduced connectivity in high-level hubs, including medial prefrontal and posterior cingulate regions; later LSD work also reported DMN disintegration associated with ego dissolution. citeturn676950search0turn460259view1

But the **entropic-brain** hypothesis, published in 2014, was already about increased repertoire, entropy and movement toward less constrained dynamical states—not simple neural shutdown. REBUS (“Relaxed Beliefs Under Psychedelics”), developed later, reframes psychedelic action as reduced precision or weighting of high-level priors, allowing more bottom-up information to influence perception and cognition. Contemporary fMRI work likewise emphasizes network desegregation, altered hierarchy and increased cross-network coupling. citeturn856612view4turn676950search2turn604882view1

So the 2026 paper does **not overthrow a decade-long “quiet brain” consensus**. It helps reconcile two observations that have coexisted for years: organized low-frequency oscillations lose amplitude, while the overall dynamical repertoire becomes richer and less constrained. The new contribution is that alpha and beta can simultaneously become **weaker and faster**. That is a refinement of the entropic/hierarchical picture, not its negation. citeturn604882view1turn604882view0

The broader electrophysiology supports that interpretation. Psychedelic MEG and EEG studies have repeatedly reported reductions in alpha and beta power under psilocybin, LSD and DMT; a 2017 analysis found increased MEG signal diversity under LSD, psilocybin and ketamine; DMT EEG has shown both reduced alpha/beta power and increased Lempel–Ziv complexity. citeturn676950search3turn676950search4

Nor is the flatter 1/f component new. Muthukumaraswamy and Liley used IRASA in 2018 to show that LSD and ketamine reduce the spectral exponent at higher frequencies, demonstrating altered scale-free background activity independently of narrowband oscillations. The 2026 paper extends that line with source-resolved aperiodic estimates plus peak-frequency and HFD analyses rather than discovering an entirely new class of psychedelic electrophysiology. citeturn346606search2turn955851view1

Likewise, increased “fractal dimension” under psychedelics predates this paper. Varley and colleagues reported in *NeuroImage* in 2020 that LSD and psilocybin increased fractal dimension in spatial and temporal brain activity. The 2026 work adds a different MEG implementation and stronger regional characterization, but popular descriptions should not imply that fractality itself was first detected here. citeturn604882view3turn470430view1

The criticality story remains unsettled. Some LSD work has interpreted power-law or harmonic changes as movement toward a critical regime, whereas newer psychedelic studies show that “entropy,” “criticality,” “scale-free behavior” and oscillatory state can point in different directions depending on metric and frequency range. Those terms should not be treated as synonyms. citeturn346606search6turn346606search4

## Methodological stress test

The strongest methodological case for the paper is that **fixed frequency bands genuinely can confound oscillatory and aperiodic changes**. Donoghue and colleagues' specparam/FOOOF framework formalized the problem: a spectrum contains both periodic peaks and an aperiodic component, and changing the latter can masquerade as changing “band power.” IRASA provides an alternative decomposition based on resampling. Comparative evaluations show that both approaches have strengths and failure modes depending on peak shape, knees, noise and parameter choices. citeturn774156search0turn774156search7turn774156search4

That means the new analysis is directionally well motivated, but it does not make spectral decomposition ground truth. Model range, peak-width assumptions, the permitted number of peaks and aperiodic functional form can affect recovered parameters. The authors partially address this with analyses over both broad and restricted frequency ranges, but an ideal replication would preregister several decompositions—specparam, IRASA and perhaps model-free individual-frequency estimates—and require convergence. citeturn470430view1turn774156search4

High-frequency MEG is a separate vulnerability. Facial, scalp and neck muscle activity extends broadly above ~20 Hz and can dominate apparent beta/gamma activity; even ICA cleaning cannot guarantee complete removal. The original 2016 LSD paper explicitly suspected residual muscle artifact in its gamma result, and the 2026 paper concedes that no simultaneous EMG was recorded and that high-gamma peak estimates require caution. citeturn460259view1turn955851view3turn774156search2

That caveat matters particularly under LSD, because altered jaw tension, facial expression, posture, eye movements or head movement can differ systematically between drug and placebo sessions. The 2026 pipeline's visual inspection, Autoreject and ICA are appropriate countermeasures, and retained-epoch counts did not differ significantly by condition, but equal rejection rates do not prove equal residual artifact. The clean solution in a new experiment is concurrent EMG/EOG plus continuous head-position monitoring and artifact metrics entered directly into the statistical model. citeturn470430view4turn955851view3

The sample size is the most fundamental limitation. A crossover design gives more power than 17 independent parallel-group subjects because every participant serves as his or her own control, and the large alpha-frequency effect is therefore quite believable. But N=17 is small for stable estimates of regional heterogeneity, phenomenology correlations, nonlinear complexity, drug×music interactions and high-dimensional classifiers. The authors themselves state that interaction effects require substantially greater power and call their music analysis exploratory. citeturn955851view5turn955851view3

Researcher degrees of freedom are also nontrivial: artifact pipeline, source model, 360-region atlas, spectral fit range, peak-selection rule, HFD hyperparameter, complexity metrics, phenomenology regressions, functional decoding and several machine-learning feature families. None of those choices is inherently improper; the concern is that a re-analysis of familiar data without a documented prospective registration can accumulate analytic flexibility. Open code improves auditability, but independent replication is what distinguishes a robust biological phenomenon from an analysis-contingent one. citeturn470430view1turn470430view5

One subtle statistical point deserves attention. The paper's ~2 Hz and ~5 Hz numbers are displayed after masking to statistically significant regions. That is legitimate for showing where an effect occurs, but it can make the descriptive mean look more universal than it is. Future papers should report subject-level peak-frequency differences with confidence intervals and standardized effect sizes independent of the spatial significance mask, ideally with the full distribution of individual changes. citeturn955851view1

For ML, the nested grouped cross-validation and label permutation are substantial strengths, especially because the inner tuning loop does not see the outer test fold. Yet there is no truly independent cohort, and the feature space is large relative to 17 participants. Classification here should therefore be read as a compact statement that the drug state leaves a multivariate electrophysiological signature in these recordings—not as an 82% clinically transportable LSD detector. citeturn955851view4turn604882view0

## Fractals, hallucinations, and the music puzzle

The “snowflake” hypothesis combines two scientifically distinct traditions. The first is **temporal scale-free neural dynamics**: power-law spectra, fractal dimension and complexity metrics applied to time series. The second is the mathematical theory of **geometric hallucinations**, in which recurrent cortical circuits in visual cortex can undergo pattern-forming instabilities that generate lattices, cobwebs, tunnels and spirals resembling Klüver's classic “form constants.” citeturn470430view1turn697770search0turn697770search3

The influential Ermentrout–Cowan and Bressloff–Cowan line of models derives geometric forms from the retinotopic map and symmetries of lateral connectivity in V1. These models predict spatial waves, rolls, hexagonal structures and related visual motifs when cortical excitation–inhibition dynamics cross an instability. Later work has linked these mathematical patterns to the architecture of visual cortex and to elementary hallucinations. citeturn697770search0turn697770search3turn697770search4

That literature makes a psychedelic link biologically plausible: reduced occipital alpha power, increased visual-cortex connectivity and altered excitation/inhibition could loosen constraints on spontaneous visual pattern formation. Indeed, the original LSD experiment linked reduced occipital alpha and expanded V1 connectivity to visual hallucination ratings. citeturn460259view1

But **Higuchi fractal dimension is not a picture of a fractal in cortex**. It estimates how irregular a one-dimensional neural time series appears across sampling scales. A higher HFD can therefore coexist with a participant seeing no explicitly fractal geometry, while a vivid spiral hallucination need not produce a specific increase in HFD. Jerbi's statement to *Nautilus* is scientifically precise on this point: the study measures fractal structure “of brain signals over time,” not the fractal structure of what participants see. citeturn470430view1turn856612view1

The paper's phenomenology analyses do not close that causal gap. They report exploratory associations among several neural variables and imagery or ego-dissolution measures, but the headline result is not a demonstrated HFD→geometric-hallucination relationship, and several correlations were uncorrected. A convincing test would require continuous or event-marked reports of form-constant hallucinations, simultaneous retinotopically resolved imaging, and a preregistered prediction linking spatial cortical wave properties—not merely global temporal complexity—to the reported geometry. citeturn955851view3

The music finding should be read in a similarly restrained way. Under the LSD×music interaction, **no cortical region survived whole-brain FDR correction**. At nominal *P*<0.05, the maps tended toward steeper rather than flatter aperiodic slopes, higher broadband offset, and lower LZC and HFD during music—opposite the dominant LSD signature. The authors propose that structured external stimulation may act as a “grounding” or attentional anchor, but they explicitly say the experiment was not powered or optimized to establish the interaction. citeturn955851view3

That does not mean music has no psychedelic effect. Kaelen and colleagues found in 2015 that LSD enhanced music-evoked emotion in a small placebo-controlled study; in 2016, an fMRI experiment found LSD altered parahippocampal–visual connectivity during music in association with enhanced imagery. A 2018 review by Barrett, Preller and Kaelen concluded that music plausibly modulates emotion, imagery and meaning under psychedelics while emphasizing the limited experimental base. citeturn126466search7turn126466search0turn690374search0

A 2023 analysis of this broader LSD-MEG dataset is particularly relevant: Mediano and colleagues reported that LSD increased neural signal diversity across conditions but that the drug–placebo complexity difference became smaller as external stimulation increased, including during music. That offers a coherent precedent for the 2026 attenuation trend: external sensory structure can itself raise or organize neural variability, reducing the additional difference produced by the drug. citeturn126466search2

There is, however, an unresolved documentation problem worth flagging. The 2026 paper says participants heard excerpts from Robert Rich and Lisa Moskow's *Yearning*. A 2023 analysis described the music in what it identifies as the same MEG experiment as material from Alio Die's “Eleusian Lullaby,” whereas *Yearning* is well documented in the related Imperial LSD-fMRI music work. Without the raw dataset publicly archived, I cannot resolve whether this reflects different runs, different subsets, or a methods-description error. It should be clarified by the authors because stimulus identity matters in any claim about music×drug interactions. citeturn955851view3turn126466search2turn126466search12

The wider music literature also argues against overgeneralization. A small randomized psilocybin study found no unique therapeutic advantage for Western classical music over an alternative playlist, and recent reviews describe the field as heterogeneous and underpowered. “Music” is not a single standardized intervention: familiarity, preference, genre, expectancy, timing and emotional salience all matter. citeturn824436search5turn690374search11

## Independent scrutiny, conflicts, and what to watch next

The journal's transparent peer review is unusually informative. *Communications Biology* credits Santiago Castro-Zaballa and Yasir Çatal as reviewers, although the public review comments are not safely attributable comment-by-comment to either individual. One reviewer specifically challenged the interpretation of high-gamma activity because of possible muscular contamination; another characterized the music interaction as “very weak” and questioned the original classifier validation because no independent test set was available. The authors subsequently implemented nested grouped cross-validation and retained explicit caveats about EMG and music in the published version. citeturn470430view6turn205175view0turn955851view4

Natural independent methodological critics—not people I found publicly commenting on this specific paper—include Thomas Donoghue and Bradley Voytek, whose spectral-parameterization work defines the assumptions behind separating periodic from aperiodic components; Haiguang Wen and Zhongming Liu, developers of IRASA; and Paul Bressloff and collaborators, whose visual-cortex models make clear the distinction between spatial form-constant generation and generic temporal fractality. Their work supplies the right stress tests for the paper's strongest interpretive leaps. citeturn774156search0turn774156search7turn697770search0

The authors disclose one relevant commercial connection: Robin Carhart-Harris reports scientific-advisory relationships with TRYP Therapeutics, Osmind, Otsuka and Red Light Holland; the other authors declare no competing interests. Jerbi's work was supported by the Canada Research Chairs program and an NSERC Discovery Grant. The original Imperial data collection was supported by the Safra Foundation, the Beckley Foundation/Beckley–Imperial research programme and crowdfunding through Walacea. None of those disclosures establishes bias, but they belong in the evidentiary record. citeturn470430view6turn604882view3

The most important next experiment is straightforward: a preregistered, adequately powered, double-blind or active-placebo-controlled replication that measures **individual peak frequencies, aperiodic spectra, LZC/HFD, EMG/EOG and head motion simultaneously**, ideally with both eyes-open and eyes-closed conditions. It should report subject-level changes and confidence intervals before region selection, compare specparam with IRASA, and reserve an independent cohort for ML validation. Those design requirements follow directly from the present paper's analytical strengths and its acknowledged limitations. citeturn470430view4turn955851view3turn774156search4

For the hallucination question, the decisive study would couple high-temporal-resolution electrophysiology to high-spatial-resolution visual-cortex measurements and time-locked reports of lattices, spirals, tunnels or nested geometry. Without that bridge, “fractal brain” and “fractal vision” remain an attractive analogy rather than a demonstrated mechanism. citeturn697770search0turn856612view1

For music, replication should be explicitly powered for the interaction and should personalize or systematically manipulate musical preference, arousal and familiarity rather than treating one soundtrack as “music.” Time-resolved analysis is also essential because a seven-minute average can erase brief periods when music either amplifies or constrains the psychedelic state. The authors themselves identify this temporal-collapsing problem as a limitation. citeturn955851view3

There is also a larger reason the mechanistic work now matters. As of November 3, 2026, pharmaceutical lysergide is already in late-stage clinical development. The Phase III PANORAMA trial in generalized anxiety disorder is ongoing but closed to recruitment in Europe, testing a single 100-μg dose against placebo with longer follow-up, while another Phase III GAD trial, VOYAGE, is also active but no longer recruiting. One late-stage GAD readout reported in August 2026 found a statistically significant benefit for DT120 over placebo; these clinical programmes are separate from the Jerbi electrophysiology study and should not be taken as validation of its proposed neural mechanism. citeturn303068search3turn303068search1turn303068news9

The bottom line is therefore narrower—and scientifically more useful—than the headline. **The best-supported new claim is not that LSD “activates” rather than “quiets” the brain. It is that conventional band-power summaries hide part of the drug's electrophysiology: alpha and beta oscillations can lose amplitude while their resonant peaks shift faster, against a flatter and more complex broadband background.** That result fits surprisingly well with modern hierarchical and entropic models. The leap from those temporal dynamics to snowflake-like hallucinations remains theoretical; the music attenuation result remains uncorrected; and the whole package still rests on a 17-person re-analysis awaiting independent replication. citeturn955851view1turn604882view1turn955851view3

## Sources

- Subramani V. et al., **“LSD reconfigures cortical dynamics through faster brain rhythms and increased fractal dimension,”** *Communications Biology*, published October 7, 2026. Primary paper, methods, results, funding, competing interests and code/data statements. citeturn856612view0turn604882view4turn470430view5
- French K., **“LSD May Speed Up Brain Activity and Make it Fractal,”** *Nautilus*, October 22, 2026. citeturn856612view1
- Carhart-Harris R.L. et al., **“Neural correlates of the LSD experience revealed by multimodal neuroimaging,”** *PNAS*, April 11, 2016. Original Imperial LSD dataset and original MEG finding of a 10→12 Hz alpha shift. citeturn856612view2turn460259view1
- Carhart-Harris R.L. et al., **“The entropic brain: a theory of conscious states informed by neuroimaging research with psychedelic drugs,”** *Frontiers in Human Neuroscience*, February 3, 2014. citeturn856612view4
- Carhart-Harris R.L. & Friston K.J., **REBUS / relaxed-beliefs framework**, *Pharmacological Reviews*, 2019. citeturn676950search2
- Carhart-Harris R.L. et al., **psilocybin fMRI study reporting decreased CBF/BOLD in high-level hubs**, *PNAS*, 2012. citeturn676950search0
- Schartner M.M. et al., **“Increased spontaneous MEG signal diversity for psychoactive doses of ketamine, LSD and psilocybin,”** *Scientific Reports*, 2017. citeturn676950search3
- Muthukumaraswamy S.D. & Liley D.T.J., **IRASA analysis of LSD/ketamine aperiodic spectral changes**, *NeuroImage*, 2018. citeturn346606search2
- Timmermann C. et al., **DMT EEG: reduced alpha/beta power and increased signal diversity**, 2019. citeturn676950search4
- Varley T.F. et al., **“Serotonergic psychedelics LSD & psilocybin increase the fractal dimension of cortical brain activity in spatial and temporal domains,”** *NeuroImage*, 2020. citeturn604882view3
- Donoghue T. et al., **spectral parameterization / FOOOF framework**, *Nature Neuroscience*, 2020. citeturn774156search0turn774156search1
- Wen H. & Liu Z., **IRASA method for separating fractal and oscillatory spectral components**, *Brain Topography*, 2016. citeturn774156search7
- Gerster M. et al., **comparison and methodological scrutiny of oscillatory/aperiodic separation methods**, 2022. citeturn774156search4
- Muthukumaraswamy S.D., **review of high-frequency EEG/MEG muscle artefact**, 2013. citeturn774156search2turn774156search6
- Bressloff P.C. et al., **geometric visual hallucinations and cortical pattern formation**, *Philosophical Transactions of the Royal Society B*, 2001. citeturn697770search0turn697770search2
- Ermentrout G.B. & Cowan J.D., **mathematical theory of visual hallucination form constants**, 1979. citeturn697770search3
- Kaelen M. et al., **“LSD enhances the emotional response to music,”** *Psychopharmacology*, 2015. citeturn126466search7
- Kaelen M. et al., **“LSD modulates music-induced imagery via changes in parahippocampal connectivity,”** *European Neuropsychopharmacology*, 2016. citeturn126466search0
- Barrett F.S., Preller K.H. & Kaelen M., **“Psychedelics and music: neuroscience and therapeutic implications,”** *International Review of Psychiatry*, 2018. citeturn690374search0
- Mediano P.A.M. et al., **LSD, context and MEG signal diversity analysis from the Imperial dataset**, *ACS Chemical Neuroscience*, 2023. citeturn126466search2
- Strickland J.C. et al., **randomized comparison of music genres during psilocybin-assisted treatment**, 2021. citeturn824436search5
- *Communications Biology* **transparent peer-review file and reporting summary** for the Subramani et al. paper, 2026. citeturn205175view0turn205175view2turn205175view3
- EU Clinical Trials Information System, **PANORAMA Phase III lysergide trial**, record current November 2, 2026. citeturn303068search3
- ClinicalTrials.gov-derived record, **VOYAGE Phase III lysergide trial for generalized anxiety disorder**, updated September 2026. citeturn303068search1
- Reuters, **late-stage DT120 generalized-anxiety readout**, August 12, 2026. citeturn303068news9

## Sources (harvested)
- https://www.nature.com/articles/s42003-026-10629-7
- https://doi.org/10.1073/PNAS.1518377113
- https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030
- https://pubmed.ncbi.nlm.nih.gov/11316482/
- https://www.frontiersin.org/journals/human-neuroscience/articles/10.3389/fnhum.2014.00020/full
- https://doi.org/10.1021/acschemneuro.3c00289
- https://pmc.ncbi.nlm.nih.gov/articles/PMC9588478/
- https://pubmed.ncbi.nlm.nih.gov/26318848/
- https://pubmed.ncbi.nlm.nih.gov/33230329/
- https://nouvelles.umontreal.ca/en/article/2026/10/21/lsd-does-more-than-desynchronize-the-brain
- https://static-content.springer.com/esm/art%3A10.1038%2Fs42003-026-10629-7/MediaObjects/42003_2026_10629_MOESM3_ESM.pdf
- https://pmc.ncbi.nlm.nih.gov/articles/PMC3277566/
- https://pmc.ncbi.nlm.nih.gov/articles/PMC6588209/
- https://www.nature.com/articles/s41598-019-51974-4
- https://www.nature.com/articles/srep46421
- https://www.sciencedirect.com/science/article/pii/S1053811918305755
- https://pmc.ncbi.nlm.nih.gov/articles/PMC12809620/
- https://www.nature.com/articles/s41598-017-17546-0
- https://pubmed.ncbi.nlm.nih.gov/23596409/
- https://pubmed.ncbi.nlm.nih.gov/486593/
- https://pubmed.ncbi.nlm.nih.gov/22448914/
- https://pubmed.ncbi.nlm.nih.gov/26257162/
- https://pubmed.ncbi.nlm.nih.gov/27084302/
- https://pubmed.ncbi.nlm.nih.gov/30240282/
- https://link.springer.com/content/pdf/10.1007/s00213-023-06394-8.pdf
- https://pmc.ncbi.nlm.nih.gov/articles/PMC13297023/
- https://pubmed.ncbi.nlm.nih.gov/33860177/
- https://ctis.eu/trial/2024-513572-17-00
- https://www.guidelinecentral.com/clinical-trial/?id=NCT06741228
- https://www.reuters.com/business/healthcare-pharmaceuticals/definium-says-lsd-based-pill-reduces-anxiety-late-stage-study-2026-08-12/
- https://www.nature.com/articles/s41593-020-00744-x
- https://www.frontiersin.org/journals/human-neuroscience/articles/10.3389/fnhum.2013.00138/full
- https://pmc.ncbi.nlm.nih.gov/articles/PMC1088430/
- https://static-content.springer.com/esm/art%3A10.1038%2Fs42003-026-10629-7/MediaObjects/42003_2026_10629_MOESM2_ESM.pdf
- https://doi.org/10.3389/fnhum.2014.00020
- https://doi.org/10.3389/fnhum.2013.00138
- https://doi.org/10.10...
- https://doi.org/10.113...
- https://osf.io/xgh4w
- http://www.w3.org/ns/dcat#>
- http://purl.org/dc/terms/>
- http://xmlns.com/foaf/0.1/>
- https://share.osf.io/vocab/2023/trove/>
- https://share.osf.io/trove/index-card/598b87e8-cd3e-41b1-a71f-1b7cba252ce4
- https://doi.org/10.31234/os...
- https://github.com/venkateshness/specparam_LSD.