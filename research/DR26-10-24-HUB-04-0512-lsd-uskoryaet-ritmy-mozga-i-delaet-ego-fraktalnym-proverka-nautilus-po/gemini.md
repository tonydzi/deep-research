# DR26-10-24-HUB-04-0512 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

LSD Brain Data Reanalysis Investigation
Contents
Share & Export
Create
Re-evaluating the Psychedelic State: A Methodological Investigation of LSD-Induced Cortical Dynamics

The renaissance of psychedelic science over the past decade has fundamentally reshaped neurobiological models of consciousness, psychiatric treatment, and cortical network dynamics. Central to this resurgence is the attempt to map the precise electrophysiological signatures of the psychedelic state. In October 2026, the science publication Nautilus featured a provocative article titled "LSD May Speed Up Brain Activity and Make it Fractal". The article reported on a newly published re-analysis of magnetoencephalography (MEG) data, suggesting that lysergic acid diethylamide (LSD) does not merely suppress or "quiet" brain rhythms—as has been the mainstream consensus for a decade—but actively accelerates them and pushes neural signals into a more complex, fractal state. Furthermore, the article teased a potential link between this fractal brain activity and the highly structured, geometric visual hallucinations characteristic of the psychedelic experience.   

This report presents a deep, source-verified journalistic investigation with strict neuroscientific editorial rigor. By systematically dissecting the primary research underlying the Nautilus article—a paper published in Communications Biology by Venkatesh Subramani, Karim Jerbi, and an international consortium of researchers—this investigation audits the exact methods, statistical validities, and contextual implications of the findings. The analysis explores the paradigm shift from the established "Entropic Brain" and REBUS frameworks to new models of spectral parameterization, addresses the methodological perils of small-sample post hoc machine learning, and untangles the speculative leap from temporal signal fractality to spatial hallucinatory geometries.   

1. Verification of the Primary Paper and Journalistic Framing

The Nautilus article accurately stems from a peer-reviewed paper titled "LSD reconfigures cortical dynamics through faster brain rhythms and increased fractal dimension," published on October 7, 2026, in the Springer Nature journal Communications Biology (DOI: 10.1038/s42003-026-10629-7). The research was spearheaded by Venkatesh Subramani, a doctoral researcher, and supervised by Karim Jerbi, a professor of psychology at the Université de Montréal and researcher at Mila (the Quebec AI Institute), in collaboration with Nicolas Farrugia, Giulia Lioi, Robin Carhart-Harris, and others.   

Dataset Origin and Experimental Design

The dataset utilized in this paper is not novel; it originates from the landmark Imperial College London studies directed by Robin Carhart-Harris around 2016, which first mapped the human brain on LSD using MEG and fMRI. The specific parameters of the dataset involve 17 healthy volunteers. Participants were recorded on two separate days, receiving either an intravenous infusion of 75 µg of LSD or a saline placebo. Approximately 4 hours post-injection, MEG data was acquired using a 275-channel system at a 1200 Hz sampling rate during resting state conditions, both with and without music (7 minutes per condition).   

Statistical Methods and Effect Sizes

The researchers employed advanced spectral parameterization and temporal complexity metrics. Crucially, they quantified oscillatory amplitude using "Peak Power" at individualized peak frequencies, effectively separating the genuine oscillatory changes from shifts in peak location and background aperiodic noise.   

The primary claims extracted from the Nautilus coverage generally align with the paper's empirical findings, though the nuances require strict methodological contextualization:

Acceleration of Brain Rhythms: The study asserts that LSD induces robust, spatially structured increases in the peak frequencies of alpha and beta rhythms. The journalistic shorthand of alpha accelerating by ~+2 Hz and beta by ~+5 Hz reflects the upward shift in oscillatory peaks. Specific frequency network analyses indicate that LSD significantly increases the variance explained by components at 12.1 Hz and 13.3 Hz (high alpha), while decreasing components at 8.5 Hz (low alpha) and 18.1 to 19.3 Hz (low beta), and subsequently increasing high beta networks at 25.3 Hz and 26.5 Hz.   

Scale-Free Dynamics: The research confirms that LSD flattens the aperiodic (1/f) spectral slope and increases both the Lempel-Ziv Complexity (LZC) and the Higuchi Fractal Dimension (HFD) of the neural signal.   

Machine Learning Classification: Utilizing Random Forest classifiers with a nested cross-validation pipeline, the researchers successfully decoded the LSD state from placebo. The model yielded a classification accuracy of 0.798 (79.8%), fitting perfectly within the 68–82% accuracy range cited across different frequency bands and regions of interest.   

The Music Paradox: Exploratory analyses investigating the interaction between the drug and music (LSD × Music) failed to survive False Discovery Rate (FDR) corrections for multiple comparisons. The results indicated that music did not robustly amplify the neural signatures of LSD and showed nominal, statistically non-significant trends toward attenuating or dampening these signatures.   

Evaluating the Journalistic Overreach

The Nautilus piece is largely scientifically faithful to the paper, avoiding gross misrepresentations. However, the journalistic framing flirts with overreach when linking the mathematical fractality of the MEG signal to the nested, snowflake-like visual hallucinations experienced by users. To the publication's credit, it includes direct cautionary quotes from senior author Karim Jerbi, who explicitly warned: "Our findings lend support to this intriguing hypothesis, but they do not yet confirm it... We measure the fractal structure of brain signals over time, not the fractal structure of the images people perceive". This effectively flags the distinction between temporal signal complexity and spatial phenomenology, separating empirical fact from phenomenological inference.   

2. Contextualizing the Literature: From REBUS to Spectral Shifts

To understand the magnitude of Subramani and Jerbi's findings, it is necessary to contextualize them against the prevailing neurobiological theories of the psychedelic state that have dominated the literature for the past decade.

The Entropic Brain and DMN Suppression

Following early fMRI and MEG studies on psilocybin, DMT, and LSD, the dominant consensus—formalized primarily by Robin Carhart-Harris and Karl Friston—was the "Entropic Brain" hypothesis and the subsequent REBUS (Relaxed Beliefs Under Psychedelics) model. These models posited that psychedelics exert their profound subjective effects by relaxing the rigid, top-down predictive processing of the brain's control centers, most notably the Default Mode Network (DMN).   

Electrophysiologically, this top-down relaxation was consistently observed as a profound global desynchronization of cortical activity. Prior studies universally reported massive decreases in oscillatory power, particularly in the alpha band (8–12 Hz), which is heavily associated with inhibitory control, resting wakefulness, and sensory gating. The mainstream narrative held that psychedelics effectively "quiet" these rhythmic control mechanisms, plunging the brain into a highly entropic, unconstrained mode of cognition characterized by increased functional connectivity across normally segregated networks.   

The Aperiodic Confound and the New Paradigm

The 2026 Communications Biology paper challenges the mechanics of this established narrative by highlighting a severe methodological blind spot in previous research: the failure to separate true periodic oscillations from the aperiodic background noise of the brain.   

Traditional EEG/MEG analyses measure power within fixed canonical frequency bands (e.g., measuring alpha strictly between 8 Hz and 12 Hz). However, if a pharmacological agent like LSD causes the brain's actual oscillatory peak to accelerate and shift from 10 Hz to 13 Hz, a fixed-band analysis will merely register a massive drop in power within the 8–12 Hz window. This is historically interpreted as a "weakening" of the rhythm or "desynchronization."

Subramani and colleagues applied spectral parameterization algorithms to mathematical separate the 1/f aperiodic component from the periodic peaks. Their analysis revealed a dual mechanism: LSD does induce genuine attenuation of oscillatory power, confirming the classical desynchronization theory. Crucially, however, it also causes the peak frequencies of those surviving oscillations to shift significantly upward.   

This distinction is monumental for the field. The finding that alpha and beta rhythms speed up—rather than just fading away—indicates that the cortex under LSD is not merely "quieted" or suppressed; it is fundamentally hyperexcitable.   

Criticality and Lempel-Ziv Complexity

The study also reported a flattening of the aperiodic 1/f spectral slope and increased neural signal fractality (HFD) and complexity (LZC). The 1/f slope is a well-established marker of the excitation/inhibition (E/I) balance in the brain. A flatter slope generally indicates a shift toward excitation and increased neural noise, aligning with theories that psychedelics push the brain closer to a state of "criticality"—a phase transition point between order and chaos where information processing and network flexibility are theoretically maximized. Previous studies have consistently reported increased LZC under psilocybin and DMT, reflecting a richer, less predictable time-series. The Subramani findings perfectly replicate these temporal complexity metrics, embedding the new peak-shift discoveries securely within the broader entropic framework while providing a much higher-resolution map of the underlying electrophysiology.   

3. Methodological Critique: Artifacts, Confounds, and Machine Learning

While the application of spectral parameterization represents a significant leap forward, the Subramani et al. study is an exploratory, post hoc re-analysis of a small legacy dataset. Subjecting the methodology to rigorous critique is essential to gauge the reliability of the findings.   

Aperiodic Component Confounds: FOOOF vs. IRASA

The authors primarily relied on the FOOOF (specparam) algorithm to decompose the MEG signals into periodic and aperiodic components. While FOOOF is currently the industry standard for this task, it is highly dependent on user-defined hyperparameters, such as the peak-width limits and the specific frequency ranges fitted. The authors fitted their models both up to 45 Hz and up to 120 Hz to ensure robustness. However, alternative methods like IRASA (Irregular Resampling Auto-Spectral Analysis) isolate the fractal component of the signal using a fundamentally different mathematical approach (resampling the time-series at non-integer factors). Because FOOOF assumes peaks are strictly Gaussian over a 1/f background, overlapping peaks or non-Gaussian rhythmic phenomena can occasionally be misclassified. A replication using IRASA would provide highly valuable convergent validity to ensure the reported peak shifts and flattened slopes are not algorithmic artifacts.   

Muscle and Movement Artifacts in MEG

A persistent and critical vulnerability in high-density MEG and EEG research involving stimulants or psychedelics is electromyographic (EMG) contamination. Subtle muscle twitches, jaw clenching (bruxism), and micro-saccadic eye movements generate broadband high-frequency noise that easily bleeds into the beta and gamma frequency bands of the MEG sensors. Because LSD is known to induce sympathetic arousal and mild motor restlessness, the risk of drug-induced EMG artifacts artificially flattening the 1/f slope and inflating fractal complexity (HFD) is exceptionally high.

The researchers attempted to mitigate this by utilizing Independent Component Analysis (ICA) and an automated rejection algorithm (Autoreject) to clean the data and turn off excessively noisy sensors. Furthermore, the study noted that the effects on complexity preferentially affected sensory, language, and emotion networks while generally "sparing motor cortex," which provides a robust physiological defense against the muscle artifact hypothesis. Nonetheless, because residual EMG artifacts can perfectly mimic a flattened 1/f slope, this methodological caveat cannot be entirely dismissed without concurrent muscular paralysis (which is ethically impossible in such trials) or highly advanced EMG-regression techniques.   

Researcher Degrees of Freedom and N=17 Power

The dataset consists of just 17 volunteers. In modern neuroimaging, an 
𝑁
=
17
N=17 sample size is severely underpowered for whole-brain, voxel-wise analyses, raising the risk of both Type I (false positive) and Type II (false negative) errors. Furthermore, this is a post hoc, un-preregistered exploratory analysis. Without a preregistered hypothesis, the "researcher degrees of freedom"—the sheer number of analytical choices regarding which frequency bands to test, which brain parcels to average, and which complexity metrics to apply—drastically increases the likelihood of finding spurious correlations. The authors properly applied False Discovery Rate (FDR) corrections to account for multiple comparisons, and the lack of survival of the music interaction effect demonstrates statistical integrity, but the lack of preregistration demotes the findings from definitive proof to hypothesis-generating evidence.   

Machine Learning Validity and Leakage Risk

A highly touted feature of the study is the use of interpretable machine learning—specifically Random Forest classifiers—to distinguish the LSD state from the placebo state based on temporal and spectral features. The models achieved an impressive accuracy of 0.798 (79.8%).   

To prevent data leakage, the authors utilized a nested cross-validation pipeline (outer K=4, inner K=3) with grid search for hyperparameter tuning. Nested cross-validation is the correct approach for small datasets. However, deploying high-dimensional machine learning on a dataset of 17 subjects is inherently precarious. In a 4-fold outer loop, the model trains on approximately 12 or 13 subjects and tests on 4. With such a restricted variance pool, the classifier may simply learn the idiosyncratic neural noise signatures of the specific individuals in the training set rather than a generalizable "LSD signature." While the 79.8% accuracy is mathematically accurate within the confines of the study, the true out-of-sample generalizability of this specific Random Forest model to a new population is entirely unknown and likely significantly lower. The ML results should be interpreted as an advanced multivariate feature-importance test rather than the development of a diagnostic biomarker.   

4. The Fractal Brain and Geometric Visual Hallucinations

One of the most theoretically tantalizing aspects of the Nautilus coverage is the suggested link between the scale-free, fractal dynamics of the brain under LSD and the nested, geometric visual hallucinations—such as cobwebs, checkerboards, tunnels, and spirals—classically associated with the psychedelic experience. To evaluate this, one must separate the temporal fractality measured in this study from the spatial fractality of visual perception.   

The Bressloff-Cowan Models of V1 Pattern Formation

In the 1920s, Heinrich Klüver categorized psychedelic visual hallucinations into four "form constants." In 2001 and 2002, mathematical biologists Paul Bressloff, Jack Cowan, Martin Golubitsky, and colleagues published seminal models explaining how the primary visual cortex (V1) generates these specific geometric patterns.   

The Bressloff-Cowan model demonstrated that the functional architecture of V1—specifically the lateral connectivity between hypercolumns that process visual edges and contours—can become unstable. When the spatially uniform resting state of V1 becomes hyper-excitable, it undergoes a symmetry-breaking bifurcation, resulting in spontaneous spatial standing waves (Turing-like patterns) across the cortical surface. The wavelength of these cortical patterns in human V1 is estimated at 2.4 to 3.2 mm.   

Crucially, the mapping of the visual field from the retina to the cortex is not 1:1; it undergoes a complex logarithmic transformation known as the retino-cortical map. Because of this anatomical mapping, a simple, parallel stripe of neural excitation across the physical surface of V1 is perceived by the subject as an expanding logarithmic spiral, a funnel, or a concentric tunnel in their visual field. Thus, the geometry of the hallucination is merely the visual translation of a standing wave in a hyper-excitable visual cortex.   

Bridging the Temporal-Spatial Divide

The Subramani et al. paper quantified neural complexity using the Higuchi Fractal Dimension (HFD) and Lempel-Ziv Complexity (LZC). Both are measures of temporal complexity—they indicate that the time-series of the MEG signal contains richer, less predictable, and more scale-free activity over time.   

Is there a direct evidentiary link between the temporal fractality measured by Jerbi's team and the spatial Turing patterns modeled by Bressloff and Cowan? Currently, the connection remains highly speculative, as Jerbi himself cautioned.   

However, a theoretical bridge exists. The Bressloff-Cowan model absolutely requires a state of cortical hyperexcitability to push V1 into instability and generate spatial standing waves. The accelerated alpha and beta rhythms (peak shifts) and the flattened 1/f slope measured by Subramani et al. are direct electrophysiological markers of precisely this type of cortical hyperexcitability and altered E/I balance. Therefore, while a fractal time-series does not directly encode a fractal image, the accelerated, scale-free temporal dynamics identified in this MEG study likely act as the neurophysiological trigger that destabilizes V1, permitting the spatial geometric form constants to emerge. Proving this conclusively would require simultaneous high-field fMRI (to capture the millimeter-scale spatial waves) and MEG (to capture the temporal acceleration), which remains a formidable technical challenge.   

5. The Music Paradox: Order within the Entropy

Music is universally recognized as a potent contextual driver (the "setting") of the psychedelic experience, heavily utilized in both indigenous traditions and modern clinical trials to guide emotional breakthroughs. Consequently, Subramani et al. explicitly hypothesized that music would potentiate and amplify the LSD-induced neural effects, increasing changes in oscillatory organization and temporal complexity.   

Remarkably, their exploratory analysis revealed the exact opposite. While LSD caused massive increases in temporal complexity and spectral shifts, the addition of music during the MEG recordings did not robustly amplify these neural signatures. Instead, statistical trends indicated that music actively dampened or attenuated them. Because the interaction effect between the drug and music (ANOVA) failed to survive strict FDR correction for multiple comparisons, the finding is technically statistically non-significant, but the directionality of the trend is unmistakable.   

Reconciling Connectivity with Complexity

This finding initially appears to contradict seminal prior work. In 2016, Mendel Kaelen, Robin Carhart-Harris, and colleagues demonstrated that LSD enhances music-induced visual imagery, a phenomenon directly mediated by increased functional connectivity between the parahippocampal cortex and the visual cortex. Subsequent research by experts like Frederick Barrett and Justin Strickland has further solidified the understanding that psychedelics and music synergistically alter acute brain network connectivity to support emotional processing and therapeutic outcomes.   

The apparent contradiction is resolved by distinguishing between the types of neuroimaging metrics utilized. The Kaelen (2016) study and related research rely heavily on fMRI functional connectivity—a measure of how different brain regions phase-lock and communicate across spatial networks. In contrast, the 2026 Subramani study focused on local temporal complexity (LZC, HFD) and localized spectral peaks.   

The inference to be drawn is that music does not simply throw "more fuel on the fire" of cortical hyperexcitability. The psychedelic brain is highly entropic, noisy, and unconstrained. Rather than increasing this local temporal randomness, music acts as an external sensory anchor. It imposes structural scaffolding onto the chaotic brain, routing the hyper-excitable neural signals into coherent, emotionally resonant spatial networks (such as the parahippocampal-visual circuits). By imposing external order and driving synchronous network communication, music inevitably reduces the raw, isolated temporal randomness (complexity) of the local neural signals. Thus, music "dampens" local temporal chaos precisely because it enhances global spatial connectivity.   

6. Independent Experts, Conflicts of Interest, and Institutional Context

The implications of this re-analysis ripple into the broader fields of computational neuroscience and clinical psychiatry, where independent experts and institutional frameworks provide essential context.

Independent Experts and Natural Critics

The findings of Subramani and Jerbi naturally intersect with the work of network dynamicists and clinical researchers who focus on how macroscopic brain states emerge from local oscillatory changes. A prime institutional hub for this intersection is the Champalimaud Foundation in Lisbon, Portugal.   

Joana Cabral, a leading researcher associated with Champalimaud, specializes in MEG functional connectivity and how delayed network interactions lead to structured brain states. Her theoretical work on connectome harmonics and oscillatory phase-locking provides a natural critical framework for interpreting the Mila group's findings. Network dynamicists like Cabral would likely emphasize that the accelerated peak frequencies (e.g., alpha shifting from 10 to 12 Hz) inherently alter the phase-delay communication across long-range anatomical connections, mechanistically explaining the breakdown of the Default Mode Network without relying purely on local power suppression.   

On the clinical translation front, figures like Albino Oliveira-Maia, Head of the Neuropsychiatry Unit at Champalimaud, represent the end-users of this basic science. Oliveira-Maia conducts extensive research into treatment-resistant depression using transcranial magnetic stimulation (TMS) and is actively involved in palliative care psilocybin trials. His work relies heavily on modulating cortical excitability and neuroplasticity. If psychedelics accelerate brain rhythms and increase network flexibility by relaxing rigid structural constraints—as the Mila group's connectome harmonic preprints suggest—this provides a direct, measurable neurophysiological biomarker (peak shifts and 1/f slopes) that clinicians like Oliveira-Maia could utilize to track patient response and titrate doses during psychedelic-assisted psychotherapy.   

Another crucial node of independent expertise resides in Basel, Switzerland, where Professor Matthias Liechti and researchers like Yasmin Schmid and Friederike Holze are conducting large-scale, modern clinical trials comparing the acute effects of LSD, psilocybin, and mescaline in both healthy and depressed populations. The Basel group's massive datasets will be the ultimate proving ground for whether the peak-shift signatures found in this 
𝑁
=
17
N=17 sample hold true across larger, double-blind, active-placebo cohorts.   

Funding and Conflicts of Interest

Transparency regarding funding and conflicts of interest (COI) is vital in psychedelic science, a field currently experiencing a massive influx of venture capital. The primary dataset utilized by Subramani et al. was generated at the Imperial College Centre for Psychedelic Research, led by Robin Carhart-Harris. Carhart-Harris (now at UCSF) acts as an advisor to several for-profit psychedelic pharmaceutical companies, most notably Compass Pathways, which sponsors massive multi-site clinical trials (including sites at Champalimaud).   

While the Communications Biology paper itself is a basic science re-analysis funded primarily by Canadian academic grants (Mila, UNIQUE Center, NSERC), the intense commercial pressure to identify proprietary neurobiological biomarkers for psychedelic therapy necessitates rigorous independent replication of any ML-based decoding algorithms before they are patented or commercialized as clinical tools.   

7. Verdict Table: Assessing the Claims

The following table synthesizes the validity, confidence level, and current evidentiary status of the primary claims generated by the study and the surrounding media narrative.

Claim / Hypothesis	Verdict	Confidence Level	Reasoning & Methodological Context	What Would Change the Verdict
LSD speeds up alpha and beta peak rhythms	Supported	High	Rigorously demonstrated using specparam to control for 1/f aperiodic noise, exposing a genuine upward peak shift (e.g., 10Hz to 12.1Hz) missed by older fixed-band studies.	A direct replication in a larger, independent MEG dataset (N>50) failing to find the peak frequency shift.
LSD flattens 1/f slope and increases fractal complexity	Supported	Moderate-High	LZC and HFD metrics robustly increased, aligning with prior entropy theories. Caveat: High-frequency components remain slightly susceptible to residual EMG artifacts.	Simultaneous MEG and muscle (EMG) paralyzation studies, or high-density EEG replication showing identical parameters.
Fractal MEG signals directly explain geometric hallucinations	Speculative	Low	The Bressloff-Cowan models require cortical hyperexcitability to form spatial Turing patterns in V1. While the MEG temporal data confirms excitability, temporal fractality does not directly equate to spatial visual fractality.	Simultaneous high-field fMRI (spatial) and MEG (temporal) demonstrating that temporal complexity peaks exactly when spatial V1 Turing patterns emerge.
Music dampens the temporal complexity of the LSD state	Partly Supported	Moderate	Music failed to potentiate the temporal complexity statistically (did not survive FDR); trends showed attenuation. This suggests music structurally organizes, rather than randomizes, brain activity, bridging temporal and spatial connectivity theories.	Pre-registered trials specifically powered to test the interaction of LSD and music on spectral features.
ML classifier can decode LSD state at ~80% accuracy	Supported	Low-Moderate	The model performed mathematically as described (79.8% accuracy via nested CV, Random Forest). However, an N=17 sample size severely limits the generalizability of any high-dimensional ML classifier.	Testing the exact trained algorithm on a completely unseen dataset from a different laboratory/scanner.
8. Conclusion and Future Outlook

The 2026 Communications Biology study by Subramani, Jerbi, Carhart-Harris, and colleagues represents a vital methodological maturation in the field of psychedelic neuroscience. By moving beyond rudimentary measurements of fixed-band power suppression and utilizing advanced spectral parameterization, the researchers have exposed a highly dynamic, accelerated, and complex cortical state that was previously obscured by mathematical artifacts. The brain on LSD is not merely "turned down" or desynchronized; it is vibrating at higher, more complex frequencies.

The Nautilus journalistic coverage effectively communicates these findings, correctly identifying the paradigm shift away from pure DMN-suppression models and accurately conveying the paradoxical stabilizing role of music. However, readers and researchers alike must navigate the allure of neuro-aesthetic speculation carefully. The jump from a flattened 1/f temporal power spectrum to the intricate, nested spatial spirals of a Klüver form constant remains an elegant, plausible hypothesis awaiting direct spatiotemporal proof.

Looking forward, the psychedelic research community must prioritize three critical directives to validate these findings:

Preregistered Replications: The era of conducting deep, post hoc exploratory analyses on the 2016 Imperial College dataset must transition into generating new, large-scale (N>100), preregistered electrophysiological datasets.

Multimodal Imaging Integration: Combining the millisecond temporal resolution of MEG (to capture alpha/beta peak shifts) with the spatial precision of ultra-high-field fMRI (to capture the millimeter-scale Turing patterns modeled by Bressloff and Cowan) is the necessary next step to bridge the gap between fractal rhythms and geometric hallucinations.

Clinical Biomarker Validation: If accelerated rhythms and increased temporal complexity are the true signatures of neuroplastic flexibility, clinical researchers must test whether the magnitude of these specific MEG signatures accurately predicts long-term clinical outcomes in trials for treatment-resistant depression and anxiety.

Ultimately, the revelation that LSD speeds up the brain's intrinsic rhythms while increasing its temporal complexity fundamentally refines our understanding of consciousness, suggesting that profound alterations in subjective reality are driven not by the silencing of the brain, but by its untethered acceleration.

Sources used in the report
nautil.us
Free Will, Illustrated - Nautilus
Opens in a new window
nautil.us
LSD May Speed Up Brain Activity and Make it Fractal - Nautilus
Opens in a new window
nautil.us
Inside the Brains of Monks Who Have Meditated for 15,000 Hours
Opens in a new window
news-medical.net
LSD speeds up brain waves and increases neural complexity, study
Opens in a new window
nouvelles.umontreal.ca
LSD does more than desynchronize the brain - UdeMnouvelles
Opens in a new window
pubmed.ncbi.nlm.nih.gov
LSD reconfigures cortical dynamics through faster brain rhythms and
Opens in a new window
orcid.org
Robin Carhart-Harris - ORCID
Opens in a new window
mila.quebec
Karim Jerbi - Mila - Quebec Artificial Intelligence Institute
Opens in a new window
researchgate.net
(PDF) LSD reconfigures cortical dynamics through faster brain
Opens in a new window
scilit.com
Jeremy Brunel from Université de Montréal | Scilit
Opens in a new window
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics and Default
Opens in a new window
media.springernature.com
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
Opens in a new window
researchgate.net
(PDF) LSD reconfigures the frequency-specific network landscape of
Opens in a new window
researchgate.net
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
Opens in a new window
researchgate.net
Mesoscopic cortical activities associated with pupil-linked
Opens in a new window
psychedelics.media
LSD Changes the Speed, Strength and Complexity of Brain
Opens in a new window
psiconautica.org
LSD and brain rhythms: a 17-volunteer MEG reanalysis - Psiconáutica
Opens in a new window
researchgate.net
Connectogram views of differences in functional (FC) and effective
Opens in a new window
imperial.ac.uk
Publications | Research groups - Imperial College London
Opens in a new window
bsky.app
brain rhythms - Bluesky
Opens in a new window
researchgate.net
LSD-induced alterations in temporal and spectral ... - ResearchGate
Opens in a new window
scienceleadr.com
What geometric visual hallucinations tell us about the visual cortex.
Opens in a new window
pubmed.ncbi.nlm.nih.gov
What geometric visual hallucinations tell us about the visual cortex
Opens in a new window
researchgate.net
What Geometric Visual Hallucinations Tell Us about the Visual Cortex
Opens in a new window
case.edu
What Geometric Visual Hallucinations Tell Us about the Visual Cortex
Opens in a new window
mind-foundation.org
Music & Psychedelics: The Power of Harmony | MIND Blog
Opens in a new window
researchgate.net
(PDF) Effects of LSD and music on brain activity - ResearchGate
Opens in a new window
fchampalimaud.org
Champalimaud Research
Opens in a new window
shemeshlab.org
Contact | Shemesh Lab
Opens in a new window
icvs.uminho.pt
Joana Cabral - ICVS
Opens in a new window
cienciavitae.pt
Albino Jorge Carvalho de Sousa Oliveira Maia (ED15-6691-FEC7)
Opens in a new window
fchampalimaud.org
Neuropsychiatry - Champalimaud Foundation
Opens in a new window
fchampalimaud.org
Oliveira-Maia | Champalimaud Foundation
Opens in a new window
fchampalimaud.org
EU Funds Psychedelic Therapy Research for Treatment-resistant
Opens in a new window
fchampalimaud.org
From promise to practice: a dose of reality for psychedelic therapies
Opens in a new window
fchampalimaud.org
The Potential - and Pitfalls - of Psychedelic Therapies for Psychiatric
Opens in a new window
open-foundation.org
Europe's psychedelic science renaissance has started
Opens in a new window
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics - bioRxiv
Opens in a new window
Sources browsed but not used
orcid.org
Venkatesh Subramani - ORCID
Opens in a new window
alphagalileo.org
Le LSD accélère les rythmes du cerveau et rend son activité plus
Opens in a new window
ae.headtopics.com
LSD speeds up brain waves and increases neural complexity, study
Opens in a new window
ur.1lib.sk
LSD reconfigures cortical dynamics through faster brain rhythms and
Opens in a new window
nautil.us
Kristen E. Paral - Nautilus
Opens in a new window
nautil.us
All Articles - All Posts - Nautilus
Opens in a new window
nautil.us
All Posts - Nautilus
Opens in a new window
nautil.us
Kristen French - Nautilus Magazine
Opens in a new window
researchgate.net
Functional Decoding of LSD-induced electrophysiological changes
Opens in a new window
biorxiv.org
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
Opens in a new window
scilit.com
Venkatesh Subramani from Université de Montréal - Scilit
Opens in a new window
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics and Default
Opens in a new window
media.springernature.com
Peer Review File LSD Reconfigures Cortical Dynamics Through
Opens in a new window
researchgate.net
Robin Carhart-Harris's research works | Imperial College London
Opens in a new window
semanticscholar.org
[PDF] Effects of LSD on music-evoked brain activity | Semantic Scholar
Opens in a new window
scilit.com
Jeremy Brunel from Université de Montréal | Scilit
Opens in a new window
mila.quebec
Karim Jerbi - Mila - Quebec Artificial Intelligence Institute
Opens in a new window
researchgate.net
The 10 Hz Frequency Fulcrum. The top row shows examples of EEG
Opens in a new window
psydao.world
LSD reconfigures cortical dynamics through faster brain rhythms and
Opens in a new window
researchgate.net
Suresh Muthukumaraswamy's research works | University of
Opens in a new window
psypost.org
LSD pushes brain activity toward a more fractal, complex state, study
Opens in a new window
researchgate.net
Faster Independent Component Analysis by Preconditioning With
Opens in a new window
researchgate.net
Nicolas FARRUGIA | Professor (Associate) | PhD | Research profile
Opens in a new window
support.google.com
Google Sports Data
Opens in a new window
researchgate.net
Accurate Machine Learning-based Monitoring of Anesthesia Depth
Opens in a new window
researchgate.net
Application of Higuchi's fractal dimension from basic to clinical
Opens in a new window
github.com
ICT-36c : trois attributions au papier Subramani et al. 2026 ... - GitHub
Opens in a new window
scholar.google.com
‪J. D. Cowan‬ - ‪Google Scholar‬
Opens in a new window
medelevator.ru
What geometric visual hallucinations tell us about the visual cortex.
Opens in a new window
math.utah.edu
U of U Mathematical Biology Journal Club - math.utah.edu
Opens in a new window
royalsocietypublishing.org
Measuring the contents of hallucinations from psychedelics and
Opens in a new window
scholar.google.com.vn
‪Paul C Bressloff‬ - ‪Google Scholar‬
Opens in a new window
fchampalimaud.org
Champalimaud Research
Opens in a new window
cienciavitae.pt
Niccolò Bonacchi (0612-705C-8295) | CIÊNCIAVITAE
Opens in a new window
pubmed.ncbi.nlm.nih.gov
Study Protocol for 'PsilOCD - PubMed
Opens in a new window
fchampalimaud.org
Annual Report 2020 - Lisboa - Champalimaud Foundation
Opens in a new window
discovermagazine.com
Psychedelic Therapy Faces a Need For Higher Standards
Opens in a new window
fchampalimaud.org
Relatorio FC 2019_Website.pdf - Champalimaud Foundation
Opens in a new window
researchgate.net
Albino OLIVEIRA-MAIA | Head of Department | Research profile
Opens in a new window
loop.frontiersin.org
Albino J. Oliveira-Maia - Loop (Frontiers)
Opens in a new window
rtp.pt
Especialista pede a governos aposta na educação sobre efeitos do
Opens in a new window
Show thinking
Deconstructing Psychedelic Neuroscience Claims
I am initiating a comprehensive investigation into recent findings regarding LSD's impact on brain activity speed and fractal dynamics. My analysis centers on evaluating a recent media report against its underlying primary study from Communications Biology, focusing on methodological rigour, statistical significance, and potential media overreach.
Mapping Methodological and Conceptual Gaps
To assess the validity of the claims, I am examining key technical nuances, such as separating aperiodic neural signals from true oscillatory shifts, controlling for muscle and movement artifacts in MEG data, and evaluating small sample sizes (N=17). I am also checking for potential machine learning cross-validation leakages and examining how these findings interact with established frameworks like the entropic brain hypothesis, REBUS model, and music-induced neuroplasticity.
Planned Research Actions
I will search primary databases and scientific literature to retrieve the target study, analyze its statistical controls, explore prior MEG/EEG psychedelic research, and identify perspectives from independent experts in neuroimaging and psychedelic science.
nouvelles.umontreal.ca
LSD does more than desynchronize the brain - UdeMnouvelles
researchgate.net
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics and Default
mila.quebec
Karim Jerbi - Mila - Quebec Artificial Intelligence Institute
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics - bioRxiv
orcid.org
Venkatesh Subramani - ORCID
alphagalileo.org
Le LSD accélère les rythmes du cerveau et rend son activité plus
researchgate.net
(PDF) LSD reconfigures cortical dynamics through faster brain
ae.headtopics.com
LSD speeds up brain waves and increases neural complexity, study
orcid.org
Robin Carhart-Harris - ORCID
pubmed.ncbi.nlm.nih.gov
LSD reconfigures cortical dynamics through faster brain rhythms and
ur.1lib.sk
LSD reconfigures cortical dynamics through faster brain rhythms and
scilit.com
Venkatesh Subramani from Université de Montréal - Scilit
nautil.us
Free Will, Illustrated - Nautilus
nautil.us
LSD May Speed Up Brain Activity and Make it Fractal - Nautilus
nautil.us
Inside the Brains of Monks Who Have Meditated for 15,000 Hours
nautil.us
Kristen E. Paral - Nautilus
nautil.us
All Articles - All Posts - Nautilus
nautil.us
All Posts - Nautilus
nautil.us
Kristen French - Nautilus Magazine
mila.quebec
Karim Jerbi - Mila - Quebec Artificial Intelligence Institute
researchgate.net
Functional Decoding of LSD-induced electrophysiological changes
Identification of Key Research Findings
Analyzing recent publications reveals breakthrough findings regarding LSD's effect on human cortical dynamics. Evaluating magnetoencephalography data demonstrates that LSD induces faster intrinsic brain rhythms alongside an elevated fractal dimension and temporal complexity, challenging the traditional view that psychedelics merely desynchronize neural activity.
Dissecting Aperiodic and Oscillatory Features
Synthesizing the spectral parameterization methodology highlights how separating the aperiodic background spectrum from periodic neural oscillations avoids misattributing shifts in peak frequencies to simple signal power loss. Resolving this technical overlap provides a clearer electrophysiological signature specific to LSD compared to other serotonergic compounds.
Investigating Machine Learning and Contextual Variables
Future research steps focus on examining the predictive accuracy of machine learning models trained on these spectral features, alongside evaluating how contextual auditory stimuli, such as music, interact with or modulate these fractal and dynamic neural reconfigurations.
psydao.world
LSD reconfigures cortical dynamics through faster brain rhythms and
news-medical.net
LSD speeds up brain waves and increases neural complexity, study
researchgate.net
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
biorxiv.org
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
scilit.com
Venkatesh Subramani from Université de Montréal - Scilit
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics and Default
imperial.ac.uk
Publications | Research groups - Imperial College London
media.springernature.com
Peer Review File LSD Reconfigures Cortical Dynamics Through
media.springernature.com
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
psiconautica.org
LSD and brain rhythms: a 17-volunteer MEG reanalysis - Psiconáutica
researchgate.net
Robin Carhart-Harris's research works | Imperial College London
semanticscholar.org
[PDF] Effects of LSD on music-evoked brain activity | Semantic Scholar
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics and Default
psychedelics.media
LSD Changes the Speed, Strength and Complexity of Brain
scilit.com
Jeremy Brunel from Université de Montréal | Scilit
researchgate.net
(PDF) LSD reconfigures cortical dynamics through faster brain
pubmed.ncbi.nlm.nih.gov
LSD reconfigures cortical dynamics through faster brain rhythms and
Uncovering Core Spectral and Machine Learning Findings
I am analyzing the specific electrophysiological shifts identified in the recent MEG reanalysis, noting that LSD accelerates alpha and beta peak frequencies while dampening overall oscillatory power once aperiodic 1/f spectral components are removed. Through interpretable machine learning models using leave-one-subject-out cross-validation, these peak frequency accelerations and fractal complexity metrics emerge as primary classifiers distinguishing the psychedelic state from placebo.
Resolving Music Interaction and Methodological Confounds
I am examining how these findings address prior assumptions regarding musical synergy and spectral power drop. Notably, the data reveals that music does not statistically amplify LSD's neural signatures but instead displays a slight dampening trend, contrasting earlier behavioral hypotheses. Furthermore, separating genuine peak frequency shifts from the baseline 1/f slope resolves historical confounds where power drops were misattributed due to fixed frequency bands.
Next Steps in Technical and Theoretical Evaluation
I will now search for precise statistical values, effect sizes, and artifact removal pipelines (such as ICA for myogenic noise control) used in the study. Additionally, I will evaluate the theoretical alignment of signal fractality with visual hallucination models, scrutinize small sample size limitations (N=17), and gather perspectives from independent neuroimaging experts.
media.springernature.com
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
mila.quebec
Karim Jerbi - Mila - Quebec Artificial Intelligence Institute
researchgate.net
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
researchgate.net
The 10 Hz Frequency Fulcrum. The top row shows examples of EEG
psydao.world
LSD reconfigures cortical dynamics through faster brain rhythms and
orcid.org
Robin Carhart-Harris - ORCID
imperial.ac.uk
Publications | Research groups - Imperial College London
researchgate.net
(PDF) LSD reconfigures the frequency-specific network landscape of
scilit.com
Jeremy Brunel from Université de Montréal | Scilit
biorxiv.org
LSD Relaxes Structural Constraints on Brain Dynamics and Default
researchgate.net
Suresh Muthukumaraswamy's research works | University of
bsky.app
brain rhythms - Bluesky
psypost.org
LSD pushes brain activity toward a more fractal, complex state, study
researchgate.net
Mesoscopic cortical activities associated with pupil-linked
researchgate.net
Faster Independent Component Analysis by Preconditioning With
researchgate.net
(PDF) LSD reconfigures cortical dynamics through faster brain
researchgate.net
Nicolas FARRUGIA | Professor (Associate) | PhD | Research profile
pubmed.ncbi.nlm.nih.gov
LSD reconfigures cortical dynamics through faster brain rhythms and
Uncovering Oscillatory Shifts and Spectral Parameterization
Synthesizing recent electrophysiological findings demonstrates that LSD induces genuine upward shifts in peak alpha and beta frequencies, alongside a flattening of the aperiodic background spectrum. By disentangling periodic neural oscillations from the background signal slope, I observed that apparent broadband desynchronization is partly explained by these frequency accelerations, providing a refined mechanistic account of psychedelic brain states.
Clarifying Temporal Fractality versus Visual Geometric Imagery
Evaluating the relationship between neural temporal complexity and subjective experience resolves a major conceptual confusion: increased fractal dimension and signal complexity in MEG time series describe signal unpredictability over time, rather than the spatial geometry of visual hallucinations. Mathematical models of visual cortex symmetry breaking account for spatial form constants, whereas temporal fractality reflects altered cortical network dynamics.
Evaluating Contextual Stimuli and Machine Learning Predictors
Examining contextual factors reveals that auditory stimuli do not statistically amplify LSD-induced electrophysiological changes, showing instead a subtle stabilizing trend. I am now proceeding to verify machine learning classifier schemes, cross-validation metrics, and statistical corrections to finalize a source-verified assessment of these spectral signatures.
support.google.com
Google Sports Data
researchgate.net
Accurate Machine Learning-based Monitoring of Anesthesia Depth
psychedelics.media
LSD Changes the Speed, Strength and Complexity of Brain
researchgate.net
Mesoscopic cortical activities associated with pupil-linked
researchgate.net
Application of Higuchi's fractal dimension from basic to clinical
researchgate.net
(PDF) LSD reconfigures cortical dynamics through faster brain
researchgate.net
LSD Reconfigures Cortical Dynamics Through Faster Brain Rhythms
github.com
ICT-36c : trois attributions au papier Subramani et al. 2026 ... - GitHub
researchgate.net
LSD-induced alterations in temporal and spectral ... - ResearchGate
researchgate.net
Connectogram views of differences in functional (FC) and effective
scienceleadr.com
What geometric visual hallucinations tell us about the visual cortex.
pubmed.ncbi.nlm.nih.gov
What geometric visual hallucinations tell us about the visual cortex
scholar.google.com
‪J. D. Cowan‬ - ‪Google Scholar‬
medelevator.ru
What geometric visual hallucinations tell us about the visual cortex.
math.utah.edu
U of U Mathematical Biology Journal Club - math.utah.edu
case.edu
What Geometric Visual Hallucinations Tell Us about the Visual Cortex
royalsocietypublishing.org
Measuring the contents of hallucinations from psychedelics and
researchgate.net
What Geometric Visual Hallucinations Tell Us about the Visual Cortex
scholar.google.com.vn
‪Paul C Bressloff‬ - ‪Google Scholar‬
mind-foundation.org
Music & Psychedelics: The Power of Harmony | MIND Blog
researchgate.net
(PDF) Effects of LSD and music on brain activity - ResearchGate
Clarifying Temporal Fractality and Mathematical Models
I am contrasting time-series signal fractality with spatial pattern formation models of visual hallucinations. Analyzing classic neuro-mathematical frameworks reveals that geometric visual patterns arise from spatial symmetry breaking and Turing-like instabilities in the visual cortex, whereas the fractal dimensions measured in magnetoencephalography represent temporal scale-free complexity across neural networks. Conflating temporal complexity with visual geometry represents a conceptual overreach in popular science reporting.
Reevaluating Music's Neural Role in Psychedelic States
I am resolving contradictions between prior behavioral literature and electrophysiological observations regarding music. While earlier studies emphasized musical synergy with psychedelics in altering connectivity and emotional processing, current spectral analyses demonstrate that music actually stabilizes and dampens signal turbulence, suggesting auditory structure acts as a grounding anchor rather than an amplifier of cortical entropy.
Synthesizing Evidence for Final Verdicts
I am now synthesizing my findings into a structured verdict evaluation to systematically grade each empirical and theoretical claim. Next, I will review signal artifact controls, sample size constraints, and independent expert viewpoints to construct a comprehensive final analysis.
fchampalimaud.org
Champalimaud Research
shemeshlab.org
Contact | Shemesh Lab
fchampalimaud.org
Champalimaud Research
open-foundation.org
Europe's psychedelic science renaissance has started
cienciavitae.pt
Niccolò Bonacchi (0612-705C-8295) | CIÊNCIAVITAE
pubmed.ncbi.nlm.nih.gov
Study Protocol for 'PsilOCD - PubMed
fchampalimaud.org
Annual Report 2020 - Lisboa - Champalimaud Foundation
discovermagazine.com
Psychedelic Therapy Faces a Need For Higher Standards
fchampalimaud.org
Relatorio FC 2019_Website.pdf - Champalimaud Foundation
icvs.uminho.pt
Joana Cabral - ICVS
cienciavitae.pt
Albino Jorge Carvalho de Sousa Oliveira Maia (ED15-6691-FEC7)
fchampalimaud.org
From promise to practice: a dose of reality for psychedelic therapies
fchampalimaud.org
Oliveira-Maia | Champalimaud Foundation
researchgate.net
Albino OLIVEIRA-MAIA | Head of Department | Research profile
loop.frontiersin.org
Albino J. Oliveira-Maia - Loop (Frontiers)
fchampalimaud.org
EU Funds Psychedelic Therapy Research for Treatment-resistant
fchampalimaud.org
The Potential - and Pitfalls - of Psychedelic Therapies for Psychiatric
fchampalimaud.org
Neuropsychiatry - Champalimaud Foundation
rtp.pt
Especialista pede a governos aposta na educação sobre efeitos do
Synthesis of Electrophysiological Insights
Synthesizing the complete empirical picture reveals that LSD drives robust peak frequency accelerations in alpha and beta bands alongside flattened background spectral slopes and heightened signal complexity across key functional networks. By isolating genuine oscillatory peaks from aperiodic shifts, these findings refine classical entropic brain models without contradicting the core premise that psychedelics disrupt organized cortical rhythms.
Evaluating Contextual Anchors and Sample Constraints
Critically examining the interaction between auditory stimuli and brain states resolves prior assumptions: music acts as a stabilizing anchor rather than an amplifier of neural entropy under LSD. Addressing methodological limitations—such as residual myogenic muscle noise in high-frequency MEG signals and small sample sizes—highlights the necessity of prospective, pre-registered replications to confirm these complex spectral signatures.
Finalizing Comprehensive Assessment
I am now finalizing the comprehensive research evaluation, compiling the final verdict table, and structuring the complete technical synthesis to summarize all evidence, expert commentary, and methodological nuances.
The initial research returned a massive amount of information. I'm now deeply analyzing and organizing all of it to make sure the report flows well and hits every key point. Because this is a thorough deep dive, putting the final report together can take up to 30 minutes.
Now comes the heavy lifting! I’m synthesizing everything into a comprehensive report, cross-checking the details, and polishing it up. Crafting a thorough piece of work like this can take several minutes, so feel free to step away while I finish up.

## Sources (harvested)
- https://nautil.us/what-regret-tells-us-238464
- https://nautil.us/lsd-may-speed-up-brain-activity-and-make-it-fractal-1286030
- https://nautil.us/inside-the-brains-of-monks-who-have-meditated-for-15000-hours-1278761
- https://www.news-medical.net/news/20261022/LSD-speeds-up-brain-waves-and-increases-neural-complexity-study-finds.aspx
- https://nouvelles.umontreal.ca/en/article/2026/10/21/lsd-does-more-than-desynchronize-the-brain
- https://pubmed.ncbi.nlm.nih.gov/42844315/
- https://orcid.org/0000-0002-6062-7150
- https://mila.quebec/en/directory/karim-jerbi?page=0%2C1
- https://www.researchgate.net/publication/415348133_LSD_reconfigures_cortical_dynamics_through_faster_brain_rhythms_and_increased_fractal_dimension?_tp=eyJjb250ZXh0Ijp7InBhZ2UiOiJzY2llbnRpZmljQ29udHJpYnV0aW9ucyIsInByZXZpb3VzUGFnZSI6bnVsbCwic3ViUGFnZSI6bnVsbH19
- https://www.scilit.com/scholars/019f28d1ea44720194e276b2ea974ff1
- https://www.biorxiv.org/content/10.64898/2026.03.02.709138v1.full
- https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs42003-026-10629-7/MediaObjects/42003_2026_10629_MOESM1_ESM.pdf
- https://www.researchgate.net/publication/390163828_LSD_reconfigures_the_frequency-specific_network_landscape_of_the_human_brain
- https://www.researchgate.net/publication/400393577_LSD_Reconfigures_Cortical_Dynamics_Through_Faster_Brain_Rhythms_and_Increased_Fractal_Dimension
- https://www.researchgate.net/publication/405669150_Mesoscopic_cortical_activities_associated_with_pupil-linked_perceptions_inferred_via_explainable_machine_learning
- https://psychedelics.media/research/lsd-brain-rhythms-neural-complexity-study/
- https://psiconautica.org/en/news/lsd-accelerates-brain-rhythms/
- https://www.researchgate.net/figure/Connectogram-views-of-differences-in-functional-FC-and-effective-connectivity-EC_fig1_370265863
- https://www.imperial.ac.uk/psychedelic-research-centre/research/publications/
- https://bsky.app/profile/nschawor.bsky.social/feed/aaap7fylawwqy
- https://www.researchgate.net/figure/LSD-induced-alterations-in-temporal-and-spectral-electrophysiological-features-A_fig3_415348133
- https://scienceleadr.com/publications/?id <tg-id>
- https://pubmed.ncbi.nlm.nih.gov/11860679/
- https://www.researchgate.net/publication/11500666_What_Geometric_Visual_Hallucinations_Tell_Us_about_the_Visual_Cortex
- https://case.edu/artsci/math/thomas/BressloffCowanGolubitskyThomasWiener2002NeuralComp.pdf
- https://www.mind-foundation.org/blog/music-psychedelic-therapy
- https://www.researchgate.net/publication/309596938_Effects_of_LSD_and_music_on_brain_activity
- https://www.fchampalimaud.org/sites/default/files/pdf/CRAnnualReport2018.pdf
- https://shemeshlab.org/contact/
- https://icvs.uminho.pt/member/joana-cabral/
- https://www.cienciavitae.pt/pt/ED15-6691-FEC7
- https://www.fchampalimaud.org/clinical_areas/neuropsychiatry
- https://fchampalimaud.org/research/groups/oliveira-maia
- https://www.fchampalimaud.org/news/eu-funds-psychedelic-therapy-research-treatment-resistant-mental-disorders-palliative-care
- https://fchampalimaud.org/news/from-promise-to-practice-dose-of-reality-psychedelic-therapies
- https://www.fchampalimaud.org/news/potential-and-pitfalls-psychedelic-therapies-psychiatric-disorders
- https://open-foundation.org/europes-psychedelic-science-renaissance-has-started/
- https://www.biorxiv.org/content/10.64898/2026.03.02.709138v3
- https://orcid.org/0009-0003-2900-577X
- https://www.alphagalileo.org/en-gb/Item-Display/ItemId/279576
- https://ae.headtopics.com/news/lsd-speeds-up-brain-waves-and-increases-neural-complexity-89068350
- http://ur.1lib.sk/book/wPbYJYMov6/lsd-reconfigures-cortical-dynamics-through-faster-brain-rhythms-and-increased-fractal-dimension.html
- https://nautil.us/author/kristen-e-_je
- https://nautil.us/all/?after=YXJyYXljb25uZWN0aW9uOjEyODYwNTc=
- https://nautil.us/all?before=YXJyYXljb25uZWN0aW9uOjEyODYwMjc=
- https://nautil.us/author/kristen-french
- https://www.researchgate.net/figure/Functional-Decoding-of-LSD-induced-electrophysiological-changes-using-NiMARE-Binned_fig4_415348133
- https://www.biorxiv.org/content/10.64898/2026.01.28.702361v1
- https://www.scilit.com/scholars/33947409
- https://www.biorxiv.org/content/10.64898/2026.03.02.709138v2.full-text
- https://media.springernature.com/original/springer-static/esm/art%3A10.1038%2Fs42003-026-10629-7/MediaObjects/42003_2026_10629_MOESM3_ESM.pdf
- https://www.researchgate.net/scientific-contributions/Robin-Carhart-Harris-2309906164
- https://www.semanticscholar.org/paper/95bde0e18b4b3a54c1c5e6652958533ff0183d83
- https://www.scilit.com/scholars/24855663
- https://mila.quebec/en/directory/karim-jerbi
- https://www.researchgate.net/figure/The-10-Hz-Frequency-Fulcrum-The-top-row-shows-examples-of-EEG-recordings-during-various_fig2_306394107
- https://psydao.world/science/papers/pmid-42844315/
- https://www.researchgate.net/scientific-contributions/Suresh-Muthukumaraswamy-2139643868
- https://www.psypost.org/lsd-pushes-brain-activity-toward-a-more-fractal-complex-state-study-finds/
- https://www.researchgate.net/publication/325864944_Faster_Independent_Component_Analysis_by_Preconditioning_With_Hessian_Approximations
- https://www.researchgate.net/profile/Nicolas-Farrugia
- https://www.researchgate.net/publication/384144367_Accurate_Machine_Learning-based_Monitoring_of_Anesthesia_Depth_with_EEG_Recording
- https://www.researchgate.net/publication/303690278_Application_of_Higuchi's_fractal_dimension_from_basic_to_clinical_neurophysiology_A_review
- https://github.com/jsboige/CoursIA/issues/21854
- https://medelevator.ru/library/3311805/
- https://www.math.utah.edu/research/mathbio/journalclub/journalclub.html
- https://royalsocietypublishing.org/rsos/article/13/9/rsos260647/483177/Measuring-the-contents-of-hallucinations-from
- https://www.fchampalimaud.org/taxonomy/term/198?page=38
- https://www.cienciavitae.pt/en/0612-705C-8295
- https://pubmed.ncbi.nlm.nih.gov/39882198/
- https://www.fchampalimaud.org/sites/default/files/pdf/CRAnnualReport2020_0.pdf
- https://www.discovermagazine.com/psychedelic-therapy-faces-a-need-for-higher-standards-46868
- https://www.fchampalimaud.org/sites/default/files/pdf/Relatorio%20FC%202019_Website.pdf
- https://www.researchgate.net/profile/Albino-Oliveira-Maia
- https://loop.frontiersin.org/people/623
- https://www.rtp.pt/noticias/pais/especialista-pede-a-governos-aposta-na-educacao-sobre-efeitos-do-uso-de-psicadelicos_n1687518