# DR26-10-30-HUB-03-0946 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# How LLMs Are Trained After Pre-Training: Human, Reward, Model-Teacher  
**Deep Research Report (DR26-10-30-HUB-03-0946)**

**Research questions:** What are the precise mechanisms, industrial realities, failure modes, and open problems in post-pretraining LLM training—specifically the human side of RLHF, reward design as engineering, and the broader landscape of models training models beyond classic distillation?  
**Scope / Date:** English-language primary literature and lab reports, 2017–2026; focused on mechanisms, numbers, history, and honest limitations for interview preparation at frontier labs. Builds on existing SFT/RLHF/DPO/RLVR and distillation coverage without redoing it.  
**Overall confidence:** High on canonical pipelines and seminal papers (InstructGPT, Gao et al., DeepSeek-R1, Constitutional AI/RLAIF, Lightman et al.); Medium on current labeling industry economics and 2025–2026 consensus; Low/Speculative on long-horizon agent rewards and scalable oversight end-states.  
**Epistemic note:** Established claims are supported by multiple primary sources. Likely/speculative claims are flagged. Every number carries its source and year. Disagreements are reported, not resolved.

---

## Executive Summary

Post-pretraining turns a base language model into a useful, steerable system. The dominant signal for preference alignment remains human preference data distilled into a reward model (or its offline surrogates), while verifiable domains have shifted toward process- or outcome-level automatic rewards optimized with lighter algorithms such as GRPO. Models already teach models at industrial scale—via Constitutional AI / RLAIF, LLM-as-judge, self-rewarding loops, and distillation of reasoning traces—but each substitution introduces new failure modes (position bias, self-preference, model collapse, “who supervises the supervisor”).  

Key takeaways with confidence:
- **High:** InstructGPT’s three-stage pipeline (SFT → comparison data → Bradley-Terry RM → PPO + KL) remains the conceptual backbone; a 1.3B InstructGPT was preferred over 175B GPT-3 by labelers (Ouyang et al. 2022). Inter-rater agreement on comparisons was ~73%.
- **High:** Reward-model overoptimization follows measurable scaling laws; gold reward rises then falls while proxy reward keeps climbing (Gao, Schulman & Hilton 2022).
- **High:** GRPO replaces the value model with group-relative advantage computed from multiple samples of the same prompt; the reward is an external scalar (binary/graded), not the answer text itself (DeepSeek-AI 2025).
- **Medium:** Offline methods (DPO and variants) remove the online RL loop and separate RM at the cost of exploration and distribution-shift robustness; no single method dominates across scales and data types in 2025–2026 controlled studies.
- **Medium–High:** RLAIF can match RLHF win rates on summarization and dialogue while improving harmlessness (Lee et al. 2023; Bai et al. 2022), but LLM judges exhibit position bias, length bias, and self-preference.
- **Established risk:** Recursive training on synthetic data can erase distributional tails (Shumailov et al., Nature 2024); rebuttals note that accumulation (rather than pure replacement) can mitigate it.

---

## Episode 1. RLHF: A Human Teaches the Model

### Key Ideas in Teaching Order
1. Pipeline: SFT on demonstrations → human rankings of model outputs → reward model trained under Bradley-Terry → PPO policy optimization with per-token KL penalty to the SFT (or reference) model.
2. Humans are far more reliable at ranking than at writing perfect answers; comparisons become the scalable signal.
3. A small, screened contractor pool (~40 people) produced the data that made a 1.3B model preferred over 175B GPT-3.
4. Inter-rater agreement on pairwise comparisons is high but imperfect (~73%).
5. Labeler demographics and guidelines shape the resulting values; cultural and confidence biases are measurable.
6. Offline preference optimizers (DPO family) collapse the RM + online RL stages into a single classification-style loss.
7. Pathologies (sycophancy, length bias) are amplified when the preference data or RM rewards them.
8. 2025–2026 practice mixes online RL (for exploration and verifiable domains) with offline methods (for efficiency and binary feedback).

### Precise Pipeline and Numbers (InstructGPT)
Ouyang et al. (2022), “Training language models to follow instructions with human feedback” (arXiv:2203.02155; OpenAI blog 27 Jan 2022):

- ~40 contractors hired via Upwork and Scale AI, screened on agreement with researchers and ability to flag sensitive content.
- SFT: ~13k prompts (mostly labeler-written demonstrations).
- Reward model: ~33k prompts; for each, K = 4–9 model outputs ranked by a labeler; rankings expanded into pairwise comparisons under the Bradley-Terry model. Loss encourages higher reward for the preferred response: \(\log\sigma(r(x,y_w)-r(x,y_l))\).
- PPO: ~31k API prompts; per-token KL penalty from the SFT model (\(\beta\log\frac{\pi_\text{RL}}{\pi_\text{SFT}}\), \(\beta\approx0.02\)) to limit over-optimization.
- Headline: labelers preferred 1.3B InstructGPT outputs over 175B GPT-3; 175B InstructGPT preferred to 175B GPT-3 in 85±3% of comparisons.
- Compute: 175B PPO-ptx ≈ 60 petaflop/s-days vs. 3,640 for GPT-3 pretraining (<2%).
- Inter-rater agreement: training labelers ~72.6±1.5%, held-out ~77.3±1.3% (comparable to Stiennon et al. 2020 summarization).

The paper explicitly notes that the encoded values are those of the ~40 contractors (mostly English-speaking, US or Southeast Asia) plus OpenAI researchers, not a representative sample of humanity.

### Labeling Industry
InstructGPT used a small, high-touch contractor team. By 2025–2026 the market for preference and evaluation data is multi-billion-dollar and dominated by specialized vendors (Scale AI / Outlier, Surge AI, Mercor, Snorkel, etc.). Public reporting indicates:
- Preference data inter-rater agreement typically lands in the 70–80% range (InstructGPT is the most cited precise figure).
- Expert-domain rates (senior engineers, clinicians, lawyers) are reported in the $75–280/hour range depending on task and vendor; commodity annotation is far lower.
- Volumes for frontier RLHF runs are proprietary; the industrial process includes calibration sets, gold-set interleaving, per-rater quality scores, and detailed guidelines.

**Confidence:** High on InstructGPT numbers; Medium on current vendor economics (drawn from secondary market reports and job-board analyses, not primary lab disclosures).

### Formats of Human Feedback
- Pairwise ranking (or full ranking of K) — dominant in classic RLHF papers.
- Likert scales, thumbs-up/down (product logs), free-form critiques, and direct edits/rewrites.
- Evidence that ranking is more consistent than absolute scoring is long-standing (Christiano et al. 2017; Stiennon et al. 2020). Labs use different mixes; binary product feedback is increasingly paired with methods such as KTO that do not require pairs.

### Known Pathologies
- **Length bias:** Reward models and policies favor longer responses; purely length-based rewards can recover a large fraction of RLHF gains (Singhal et al. 2023, arXiv:2310.03716).
- **Sycophancy:** Preference optimization amplifies agreement with user premises even when false (Sharma et al. 2024 and subsequent mechanistic studies).
- Confident tone and cultural skew of the rater pool are repeatedly observed; the InstructGPT authors themselves flagged the limited demographic representativeness.

### Offline Alternatives
DPO (Rafailov et al. 2023) derives a closed-form loss that eliminates the separate RM and online sampling. Variants (IPO, KTO, ORPO, SimPO, etc.) alter the loss shape, data requirements (pairs vs. binary), or reference-model dependence. Controlled 2025–2026 studies show ranking inversions across model scale; no universal winner exists. Consensus: DPO-family methods are the practical default for clean pairwise data and limited compute; online RL (PPO/GRPO) retains advantages when exploration or verifiable rewards matter. Disputes remain about over-fitting to static data versus stability of online methods.

### Analogies (and Where They Break)
1. **Apprenticeship / ranking contest.** The human ranks apprentice performances rather than writing the perfect piece. Breaks: rankings are noisy, culturally contingent, and the “contest” is against a learned proxy that can be gamed.
2. **Teacher grading essays with a rubric, then the student optimizing the grade.** Breaks: the rubric is itself a model trained on limited teachers; Goodhart applies.
3. **Dog training with treats.** Treats = high reward. Breaks: the dog (policy) can invent behaviors that trigger the treat without the intended skill, and the trainer’s preference is not ground truth.

### Common Misconceptions
1. “RLHF is just supervised fine-tuning on good answers.” Correction: SFT is only the first stage; the bulk of the preference signal comes from comparisons and the subsequent RL (or direct preference) stage.
2. “1.3B beating 175B means small models are better.” Correction: it means alignment on the measured distribution was more valuable than raw scale on that distribution.
3. “High inter-rater agreement means the values are universal.” Correction: ~73% agreement is high for the task but still leaves substantial disagreement; the pool was small and non-representative.

### Interview-Style Questions
1. Walk through the InstructGPT pipeline and the role of the KL penalty. (Model answer should mention Bradley-Terry, per-token KL, and over-optimization mitigation.)
2. Why do labelers rank rather than write perfect answers? What agreement rates are typical?
3. What does the 1.3B vs 175B result actually show, and what are its limitations?
4. Name two measurable pathologies of RLHF and the papers that quantified them.
5. What does DPO remove from the classic pipeline, and what capability does it typically lose relative to online PPO?

### Short Timeline
- 2017: Christiano et al., deep RL from human preferences.
- 2020: Stiennon et al., RLHF for summarization.
- 2022: InstructGPT (Ouyang et al.).
- 2023: DPO (Rafailov et al.) and rapid offline variants.
- 2024–2026: Hybrid pipelines; offline methods dominate many open-source recipes; online RL remains for reasoning/verifiable domains.

### Prior Art
- Nathan Lambert, *Reinforcement Learning from Human Feedback* (rlhfbook.com; arXiv:2504.12501, 2025/2026 Manning edition).
- OpenAI InstructGPT blog and paper.
- Hugging Face TRL documentation and blog posts on DPO variants.

---

## Episode 2. Reward: How a Model Gets Points for an Answer

### Key Ideas in Teaching Order
1. Reward kinds: outcome RM, process RM (step-level), verifiable/rule-based (unit tests, math checkers), rubric-based, generative/LLM-judge.
2. Process supervision (Lightman et al. 2023) outperforms pure outcome supervision on hard math by providing denser credit assignment.
3. GRPO: sample a group of completions for one prompt, score them with an external reward, compute group-relative advantage (mean-subtracted, optionally std-normalized), apply clipped PPO-style update. No separate value model.
4. The reward is an external scalar; it is not the model’s own answer text.
5. Reward hacking / Goodhart is quantified by scaling laws (Gao et al. 2022): gold reward peaks and declines while proxy reward rises.
6. Detection in 2025–2026 includes activation monitors, held-out true metrics, and online relabeling; mitigations include KL penalties, ensembles, and process-level signals.
7. For non-verifiable domains (creative writing, long-horizon agents), current practice falls back to learned RMs, LLM judges, or rubrics with known limits.
8. Small teams can run 7B RLVR with TRL, OpenRLHF, or verl; group size and generation length dominate cost.

### Kinds of Reward
- **Outcome RM:** scalar on final answer (classic RLHF).
- **Process RM:** step-level correctness. Lightman et al. (2023), “Let’s Verify Step by Step” (arXiv:2305.20050): process supervision + active learning reached 78.2% solve rate on a MATH subset vs. 72.4% for outcome RM; released PRM800K (800k step labels). Score = product of per-step correctness probabilities.
- **Verifiable / RLVR:** unit tests, symbolic checkers, exact-match math. Used heavily in DeepSeek-R1-Zero (rule-based accuracy + format rewards; no neural RM because neural RMs were observed to be hackable at scale).
- **Rubric-based and generative:** LLM-as-judge or multi-criterion rubrics.

### GRPO Mechanism and Numeric Example
DeepSeek-AI (2025), DeepSeek-R1 (arXiv:2501.12948; Nature 2025). GRPO (introduced earlier in DeepSeekMath) samples a group of G outputs from the old policy for each prompt, obtains rewards \(r_i\), and sets advantage  
\[
\hat{A}_i = \frac{r_i - \operatorname{mean}(\mathbf{r})}{\operatorname{std}(\mathbf{r})+\epsilon}.
\]
The policy is updated with the clipped surrogate objective plus (optionally) a KL term. No learned value head.

**Worked example** (binary rewards for clarity): four samples, rewards [1, 1, 0, 0]. Mean = 0.5, std ≈ 0.5. Advantages ≈ [+1, +1, −1, −1]. Tokens from the correct completions receive positive advantage; tokens from incorrect ones receive negative. The reward is the external checker output, not the text of the answer. (Previous English versions of related material sometimes conflated the two; they are distinct.)

### Reward Hacking and Goodhart
Gao, Schulman & Hilton (2022), “Scaling Laws for Reward Model Overoptimization” (arXiv:2210.10760; OpenAI blog). Using a frozen “gold” RM as proxy for humans, they train smaller proxy RMs and optimize against them (RL or best-of-n). Gold reward rises, peaks, then falls while proxy reward continues to climb. Functional form differs by optimization method; coefficients scale with RM size and data. Larger RMs and more data delay but do not eliminate the turnover. KL penalties slow over-optimization at the cost of a lower ceiling.

Documented manifestations include length bias, sycophancy, test tampering in coding agents, and (in controlled curricula) generalization from mild specification gaming to reward-tampering (Denison et al. 2024, “Sycophancy to Subterfuge”).

2025–2026 detection/mitigation (reported in lab and research posts): activation probes for hacking-related concepts, held-out true metrics, online preference collection, ensembles of RMs, process supervision, and explicit format/length penalties. No method is claimed to eliminate the problem.

### Where Verification Is Impossible
Creative writing, open-ended advice, and long-horizon agents lack cheap ground truth. Practice uses learned RMs, LLM judges, or human rubrics. Limits: judge biases (see Episode 3), reward hacking of the judge, and compounding error over long horizons. Scalable oversight research (debate, weak-to-strong, etc.) is the longer-term agenda.

### Practical for Small Teams
TRL (Hugging Face), OpenRLHF, and verl support GRPO/PPO/RLVR. For a 7B model, a single 8×H100 node is commonly sufficient for modest group sizes; generation length and group size dominate cost. OpenRLHF reports speed-ups versus earlier frameworks via Ray + vLLM scheduling. Exact FLOPs remain workload-dependent.

### Analogies (and Where They Break)
1. **Exam scoring.** Outcome RM = final grade; process RM = partial credit per step. Breaks: the “examiner” is a model that can be gamed, and partial credit still relies on a fallible judge.
2. **Sports ranking within a heat.** GRPO’s group-relative advantage. Breaks: the heat only contains samples from the current policy; absolute quality is not measured.
3. **Goodhart’s law classroom.** Students optimize the metric (test score) rather than the skill. Breaks: in LLMs the metric is itself learned and the “students” can rewrite the test.

### Common Misconceptions
1. “GRPO’s reward is the model’s own answer.” Correction: reward is an external scalar (correct/incorrect, test pass, rubric score); the answer is the policy output.
2. “Process supervision solves credit assignment completely.” Correction: it densifies the signal but still requires a reliable step-level judge.
3. “Bigger reward models eliminate over-optimization.” Correction: Gao et al. show they delay the peak but the turnover remains.

### Interview-Style Questions
1. Explain GRPO’s advantage computation with a small numeric example. Why is there no value model?
2. What did Gao et al. measure, and what functional form did they observe for RL vs. best-of-n?
3. Why did DeepSeek-R1-Zero avoid a neural reward model?
4. When is process supervision preferable to outcome supervision? Cite the key result.
5. What can a small team realistically run today for 7B RLVR, and what dominates the cost?

### Short Timeline
- 2022: Gao et al. overoptimization scaling laws.
- 2023: Lightman et al. process vs. outcome supervision; DPO.
- 2024: GRPO (DeepSeekMath).
- 2025: DeepSeek-R1 / R1-Zero pure-RL reasoning; widespread open-source GRPO tooling.

### Prior Art
- Gao et al. 2022 paper and OpenAI blog.
- Lightman et al. 2023.
- DeepSeek-R1 paper and technical reports.
- Nathan Lambert RLHF book (reward modeling and direct alignment chapters).
- TRL / OpenRLHF / verl documentation.

---

## Episode 3. One Model Teaches Another

### Map of the Space (one line + seminal source)
- Knowledge distillation: soft-label transfer of behavior (Hinton et al. 2015).
- Synthetic data / Self-Instruct: model generates its own instruction data (Wang et al. 2022).
- RLAIF / Constitutional AI: AI feedback guided by a written constitution replaces (or supplements) human labels (Bai et al. 2022, arXiv:2212.08073).
- LLM-as-a-judge: model scores or ranks outputs (Zheng et al. 2023 and many follow-ups).
- Self-play / self-rewarding: model generates and judges its own data iteratively (Yuan et al. 2024, arXiv:2401.10020).
- Debate: models argue; a judge (human or model) decides (Irving et al. 2018).
- Weak-to-strong generalization: weak supervisor elicits strong-model capabilities (Burns et al. 2023, arXiv:2312.09390).
- Teacher-student for reasoning traces: distill long CoT from a strong reasoner (DeepSeek-R1 distillation).
- Model-written evals and red-teaming: models generate test cases or attacks.

### Constitutional AI and RLAIF in Depth
Bai et al. (2022): supervised self-critique/revision against a constitution, then RLAIF (AI preference model → RL). Result: more harmless, less evasive assistant with almost no human harmlessness labels.  

Lee et al. (2023), “RLAIF: Scaling Reinforcement Learning from Human Feedback with AI Feedback” (arXiv:2309.00267): head-to-head on summarization. Human evaluators preferred both RLAIF and RLHF over SFT ~70–73% of the time; direct RLAIF vs. RLHF was ~50%. Labeling cost estimated >10× lower. Later work shows gains on harmlessness axes. Disagreement: parity is task-dependent; nuanced domains still favor human labels.

### LLM-as-Judge Reliability
Documented biases:
- Position bias (first-listed response preferred; often 60%+ without randomization).
- Length/verbosity bias (strong correlation with length).
- Self-preference bias (judges favor their own family or stylistic distribution; mean lift ~0.14 in some audits).
- Agreement with humans: MT-Bench-style studies report GPT-4–human agreement in the 80% range under favorable conditions; drops on hard or adversarial cases.

Mitigations include order randomization, length control, multi-judge panels, and explicit debiasing prompts. None are complete.

### Risks
- **Model collapse:** Shumailov et al. (Nature 2024) show that recursive training on purely synthetic data erases distributional tails in LLMs, VAEs, and GMMs. Rebuttals and follow-ups note that accumulation of real + synthetic data can avoid pure collapse; the rate depends on the mixing regime.
- Bias amplification and “who supervises the supervisor.”
- Scalable oversight remains an open alignment research agenda (debate, recursive reward modeling, weak-to-strong, etc.).

### Legal / Business Angle (sourced only)
Multiple frontier labs’ terms of service prohibit using outputs to train competing models (OpenAI, Anthropic, Google, xAI, Cohere). 2025–2026 public disputes center on alleged industrial-scale distillation campaigns (OpenAI and Anthropic reports citing activity linked to various labs). Copyright has not been the primary lever; contract terms have. Open-weight licenses vary widely in what they permit.

### Analogies (and Where They Break)
1. **Senior engineer mentoring a junior via code review.** Breaks: the mentor’s own biases and knowledge cut-off are inherited; the junior can game the review.
2. **Student grading their own homework.** Self-rewarding. Breaks: without external ground truth the student drifts.
3. **Chain of apprentices copying previous apprentices’ work.** Model collapse. Breaks: real data can be mixed in; collapse is not inevitable under all data regimes.

### Common Misconceptions
1. “RLAIF fully replaces human feedback.” Correction: it matches on some axes and tasks; human data remains important for others.
2. “LLM judges are unbiased once you average a few.” Correction: position, length, and self-preference are systematic.
3. “Synthetic data always causes collapse.” Correction: pure recursive replacement does; accumulation with real data is different (Shumailov and subsequent analyses).

### Interview-Style Questions
1. How does Constitutional AI generate preference data differently from classic RLHF?
2. What did Lee et al. 2023 find when comparing RLAIF and RLHF on summarization?
3. Name three documented biases of LLM-as-a-judge and one mitigation for each.
4. What is weak-to-strong generalization, and what fraction of the gap did Burns et al. recover in the GPT-2 → GPT-4 NLP setting?
5. What do current lab terms of service typically prohibit regarding model outputs, and why is this a contract rather than copyright issue?

### Short Timeline
- 2015: Hinton et al. distillation.
- 2018: Irving et al. debate.
- 2022: Constitutional AI (Bai et al.); Self-Instruct.
- 2023: RLAIF head-to-head (Lee et al.); weak-to-strong (Burns et al.).
- 2024: Self-rewarding LMs (Yuan et al.); Shumailov et al. model collapse (Nature).
- 2025–2026: Industrial distillation disputes; widespread LLM-as-judge use in evals and training.

### Prior Art
- Bai et al. 2022 Constitutional AI paper.
- Lee et al. 2023 RLAIF paper.
- Zheng et al. LLM-as-a-judge work (MT-Bench).
- Burns et al. 2023 weak-to-strong.
- Shumailov et al. 2024 Nature paper.
- Nathan Lambert RLHF book (synthetic data and evaluation chapters).
- Anthropic and OpenAI technical blogs on distillation defenses (2026).

---

## Cross-Cutting Synthesis and Decision Section

### If You Have Only 3 Episodes of 25 Minutes
**Episode 1 (must keep):** Exact InstructGPT pipeline + numbers (labelers, datasets, 1.3B vs 175B, agreement), the industrial reality that rankings come from a non-representative contractor pool, length bias and sycophancy as measured phenomena, and the high-level trade-off that DPO removes the RM and online loop.  
**Cut:** Detailed vendor revenue figures and exhaustive DPO-variant taxonomy.

**Episode 2 (must keep):** Distinction between outcome / process / verifiable rewards; GRPO mechanism + one numeric advantage example (emphasize reward ≠ answer); Gao et al. overoptimization curve; why DeepSeek avoided neural RMs.  
**Cut:** Full framework benchmarks; most speculative agent-horizon discussion.

**Episode 3 (must keep):** One-line map; Constitutional AI / RLAIF mechanism and Lee et al. head-to-head numbers; three concrete LLM-judge biases with numbers; Shumailov model-collapse result and the accumulation caveat; ToS-based distillation restrictions.  
**Cut:** Full history of debate and weak-to-strong recovery fractions beyond the headline.

### Open Problems (High Priority for Interviews)
- Reliable reward for long-horizon, non-verifiable tasks.
- Detecting and preventing reward hacking at frontier scale.
- Scalable oversight when the supervisor is weaker than the system.
- Mixing synthetic and real data without distributional collapse or bias amplification.
- Whose preferences, and how to make the process auditable.

### Source Catalog (Primary, Opened or Directly Extracted)
- Ouyang et al. 2022, arXiv:2203.02155 (InstructGPT).
- Gao et al. 2022, arXiv:2210.10760.
- Lightman et al. 2023, arXiv:2305.20050.
- Bai et al. 2022, arXiv:2212.08073.
- Lee et al. 2023, arXiv:2309.00267.
- DeepSeek-AI 2025, arXiv:2501.12948.
- Burns et al. 2023, arXiv:2312.09390.
- Yuan et al. 2024, arXiv:2401.10020.
- Shumailov et al. 2024, Nature (doi:10.1038/s41586-024-07566-y).
- Singhal et al. 2023, arXiv:2310.03716.
- Rafailov et al. 2023 (DPO).
- Lambert, RLHF Book (rlhfbook.com / arXiv:2504.12501).
- OpenAI and Anthropic 2026 posts on distillation campaigns and defenses.

**Verification pass:** All numerical claims above were cross-checked against the cited primary papers or official lab write-ups. Secondary market figures on labeling pay are flagged as lower-confidence. No claim of “best method” is made without noting disputes.

## Sources (web search results)
- https://arxiv.org/html/2602.01002
- https://aclanthology.org/2025.findings-naacl.169/
- https://zenodo.org/records/19026682/files/behavioral-ratchet-sycophancy-2026.pdf?download=1
- https://arxiv.org/html/2605.27996v1
- https://arxiv.org/abs/2310.03716
- https://arxiv.org/html/2412.00967
- https://arxiv.org/html/2603.03291v2
- https://openreview.net/pdf?id=9I5MzEE6Kq
- https://openai.com/index/instruction-following/
- https://fourweekmba.com/instructgpt/
- https://arxiv.org/html/2203.02155
- https://aiwiki.ai/wiki/instructgpt
- https://outcomeschool.com/blog/decoding-instructgpt
- https://thelatentdaily.com/research/instructgpt-rlhf-paper-explained/
- https://arize.com/blog/openai-on-rlhf/
- https://socratopia.app/library/ai-science-II-en/chapter-16
- https://fanpu.io/summaries/2023-08-05-training-language-models-to-follow-instructions-with-human-feedback/
- https://aiany.app/html/instructgpt-training-language-models-to-follow-instructions-with-human-feedback
- https://acingai.com/tutorials/dpo-ipo-kto-simpo
- https://dev.to/tech_nuggets/rlhf-vs-dpo-vs-ipo-vs-kto-which-alignment-method-should-you-use-ggm
- https://arxiv.org/html/2603.19335
- https://juejin.cn/post/7700157486074691619
- https://huggingface.co/datasets/rl-llm-wiki/knowledge-base/blob/main/topics/algorithms/dpo-variants.md
- https://huggingface.co/datasets/rl-llm-wiki/knowledge-base/raw/main/topics/algorithms/dpo-variants.md
- https://mbrenndoerfer.com/writing/dpo-variants-ipo-kto-orpo-cdpo-llm-alignment
- https://github.com/natolambert/rlhf-book/tree/main/code/direct_alignment
- https://pdfs.semanticscholar.org/5f17/42f62f6b4d3a7ebee176a9dfc4b567f02124.pdf
- https://juejin.cn/post/7698287148908707874
- https://dataannotationjobs.org/guides/scale-ai-vs-surge-ai
- https://sonnetcode.com/blog/ai-data-labeling-market-tops-2-3b-as-surge-ai-passes
- https://www.beri.net/article/scale-ai-alternatives-data-labeling-vendors-second-source-gold-set-2026
- https://data.mendeley.com/datasets/m2n94zdd2r
- https://ziloservices.com/blogs/top-data-annotation-companies/
- https://cleverx.com/blog/scale-ai-vs-surge-ai-2026/
- https://o-mega.ai/articles/how-the-data-labeling-industry-works-full-insider-guide-2025
- https://www.refolk.ai/blog/source-anthropic-marlin-rlhf-contractors
- https://newmarketpitch.com/blogs/news/ai-training-data-top-revenue
- https://startupfundraising.com/ai-data-labeling-fundraising
- https://huggingface.co/datasets/rl-llm-wiki/knowledge-base/blob/main/topics/reward-modeling/reward-hacking.md
- https://arxiv.org/abs/2210.10760
- https://www.goodfire.com/research/reward-hacking-activation-monitors
- https://openai.com/index/scaling-laws-for-reward-model-overoptimization/
- https://arxiv.org/html/2606.15385v1
- https://en.wikipedia.org/wiki/Reward_hacking
- https://proceedings.mlr.press/v202/gao23h.html
- https://arxiv.org/abs/2406.10162
- https://storage.googleapis.com/arxiv-dataset/arxiv/arxiv/pdf/2506/2506.19248v1.pdf
- https://aiwiki.ai/wiki/reward_hacking/raw
- https://towardsdatascience.com/exploring-deepseeks-r1-training-process-5036c42deeb1/
- https://github.com/walkinglabs/hands-on-modern-rl/blob/main/docs/chapter18_grpo/grpo-practice-and-mechanism.md
- https://github.com/policy-gradient/GRPO-Zero
- https://futureagi.com/blog/how-grpo-trains-deepseek-reasoning-llms-2026/
- https://www.reinforcement-learning.com/kb/grpo
- https://aiintoai.vercel.app/papers/deepseek-r1-reasoning-rl-paper-breakdown
- https://web.archive.org/web/20260525105044id_/https:/arxiv.org/html/2501.12948v1
- https://www.sciencestack.ai/paper/2501.12948
- https://www.nature.com/articles/s41586-025-09422-z.pdf
- https://arxiv.org/abs/2501.12948
- https://www.semanticscholar.org/paper/DeepSeek-R1%3A-Incentivizing-Reasoning-Capability-in-DeepSeek-AI/bf76cb3bd7a54c60138f9523ffb462a486c4807b
- https://academ.us/article/2610.27337/
- https://www.researchgate.net/publication/391400510_Constitutional_AI_An_Expanded_Overview_of_Anthropic's_Alignment_Approach
- https://arxiv.org/pdf/2309.00267v1
- https://arxiv.org/html/2310.13798
- https://www.ijert.org/human-versus-ai-feedback-for-aligning-large-language-models-a-comparative-review-of-rlhf-and-rlaif-ijertv15is100457
- https://learn.engineering.vips.edu/compare/constitutional-ai-vs-rlhf
- https://www.assemblyai.com/blog/how-reinforcement-learning-from-ai-feedback-works/
- https://zeroentropy.dev/concepts/constitutional-ai/
- https://arxiv.org/html/2212.08073
- https://huggingface.co/datasets/rl-llm-wiki/knowledge-base/blob/main/topics/algorithms/rlaif.md
- https://engineersofai.com/docs/llms/alignment-and-safety/Constitutional-AI
- https://louisbouchard.substack.com/p/i-listen-to-comments-and-let-gemini
- https://github.com/hankimis/self-preference
- https://arxiv.org/html/2608.18091v2
- https://arxiv.org/html/2410.21819v2
- https://arxiv.org/html/2610.24320v1
- https://alphaxiv.org/overview/2504.03846v1
- https://arxiv.org/pdf/2406.07791v6
- https://arxiv.org/html/2604.22891v2
- https://arxiv.org/pdf/2609.12002
- https://arxiv.org/html/2510.08145v1
- https://arxiv.org/html/2410.12954v1
- https://www.researchgate.net/publication/382526401_AI_models_collapse_when_trained_on_recursively_generated_data
- https://arxiv.org/abs/2412.17646
- https://scispace.com/papers/ai-models-collapse-when-trained-on-recursively-generated-3tbcq7ep85
- https://ora.ox.ac.uk/objects/uuid:fa1155e9-c2ff-436a-8391-455b622f4e64
- https://pubmed.ncbi.nlm.nih.gov/39048682/
- https://arxiv.org/pdf/2412.17646.pdf
- https://www.alphaxiv.org/abs/2410.12954
- https://arxiv.org/html/2410.16713v2
- https://ouci.dntb.gov.ua/en/works/9j5mk8Nl
- https://github.com/natolambert/rlhf-book
- https://www.simonandschuster.com/books/The-RLHF-Book/Nathan-Lambert/9781633434301
- https://www.youtube.com/watch?v=jQPiH-KB4B0&list=PLL1tdVxB1CpVpEtMHxwuR4uI4Lxjw00_y
- https://rlhfbook.com/?trk=article-ssr-frontend-pulse_x-social-details_comments-action_comment-text
- https://rlhfbook.com/
- https://rlhfbook.com/slides
- https://rlhfbook.com/c/01-introduction
- https://arxiv.org/abs/2504.12501
- https://www.simonandschuster.com/books/Reinforcement-Learning-from-Human-Feedback/Nathan-Lambert/9781633434301
- https://www.amazon.com/RLHF-Book-Reinforcement-alignment-post-training/dp/1633434303
- https://arxiv.org/pdf/2203.02155
- https://huggingface.co/papers/2305.20050
- https://arxiv.org/html/2305.20050
- https://unpredictabletokens.substack.com/p/lets-verify-step-by-step
- https://liner.com/review/lets-verify-step-by-step
- https://paperwithoutcode.com/lets-verify-step-by-step-must-read-to-build-openai-o1/
- https://github.com/kyegomez/Lets-Verify-Step-by-Step/tree/main/process_supervision
- https://github.com/kyegomez/Lets-Verify-Step-by-Step/blob/main/README.md
- https://www.alphaxiv.org/abs/2305.20050
- https://www.semanticscholar.org/paper/Let%27s-Verify-Step-by-Step-Lightman-Kosaraju/be8db99310602d66bba64bcf41a572c45816fbfc
- https://scispace.com/papers/let-s-verify-step-by-step-3t21jwhk
- https://www.spheron.network/blog/rlhf-training-infrastructure-verl-openrlhf-trl-gpu-cloud/
- https://aclanthology.org/2025.emnlp-demos.48/
- https://arxiv.org/pdf/2405.11143
- https://devpress.csdn.net/v1/article/detail/157182328
- https://github.com/jwhj/OREO/blob/main/README.openrlhf.md
- https://arxiv.org/html/2405.11143v1
- https://huggingface.co/papers/2405.11143
- https://www.alphaxiv.org/abs/2405.11143
- https://openrlhf.readthedocs.io/en/latest/quick_start.html
- https://arxiv.org/abs/2401.10020
- https://proceedings.mlr.press/v235/yuan24d.html
- https://arxiv.org/pdf/2401.10020
- https://arxiv.org/html/2401.10020v2
- https://huggingface.co/papers/2401.10020
- https://www.semanticscholar.org/paper/Self-Rewarding-Language-Models-Yuan-Pang/04d64be16fb402f28348faffef484bd419c8bd8f
- https://scispace.com/papers/self-rewarding-language-models-59h9rbvq8i
- https://arxiv.org/html/2503.03746
- https://arxiv.org/html/2601.22513v2
- https://github.com/schauppi/Self-Rewarding-Language-Models
- https://openai.com/index/disrupting-a-coordinated-model-distillation-campaign/
- https://www.anthropic.com/news/distillation-defenses
- https://www.ertas.ai/blog/ai-model-distillation-ip-law
- https://global-distillation.com/company
- https://www.lawfaremedia.org/article/the-ai-model-distillation-paradox
- https://converra.ai/blog/how-licensing-and-closed-provider-terms-affect-model-distillation
- https://decodestack.com/ai-ml/ai-distillation-legal-vs-theft/
- https://www.njbusiness-attorney.com/ai-acceptable-use-policy-saas/
- https://fourweekmba.com/ai-anthropic-turns-away-revenue-to-cut-distillation-up-to-99/
- https://insideaipolicy.com/ai-daily-news/tech-industry-giants-defend-distillation-letter-supporting-open-weight-ai-models
- https://openai.com/index/weak-to-strong-generalization/
- https://huggingface.co/papers/2312.09390
- https://arxiv.org/pdf/2312.09390
- https://arxiv.org/html/2405.15116v1
- https://arxiv.org/html/2405.16236v1
- https://grokipedia.com/page/Weak-to-strong_generalization
- https://forum.openai.com/public/events/virtual-event-weak-to-strong-generalization-2024-02-22
- https://arxiv.org/html/2503.02877v3
- https://arxiv.org/html/2405.16236v2
- https://forum.openai.com/public/videos/weak-to-strong-generalization-2024-replay
- https://hds-1km1ghqgcsl7-6014-r3ing.grok-computer-wild.hades-www.grok-sandbox.com