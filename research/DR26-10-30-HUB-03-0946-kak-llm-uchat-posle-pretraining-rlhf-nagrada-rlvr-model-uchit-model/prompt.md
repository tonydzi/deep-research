# DR26-10-30-HUB-03-0946 — the exact prompt sent to every model

# DR26-10-30-HUB-03-0946 — Как учат LLM: RLHF человек, награда и RLVR, модель учит модель — основа серии из 3 подкастов

# DR-ID: (будет присвоен dr_start) — Как учат языковые модели: человек, награда, модель-учитель (основа серии из 3 подкастов)


ROLE: You are a senior research analyst preparing the factual backbone for a 3-episode educational audio series (NotebookLM) about how large language models are trained after pre-training. Audience: a technical generalist (programmer, ex-COO/CTO) preparing for interviews at frontier AI labs. He needs mechanisms, history, numbers and honest open problems, not marketing.

CONTEXT (what we already have, do NOT redo it, build on it):
- We already produced an episode "Training signal: example, preference or verification" (SFT vs RLHF vs DPO vs RLVR, 11 primary sources incl. InstructGPT, DPO, DeepSeek-R1/GRPO). Two Russian versions accepted. Known failure of our previous English versions: the generator confused GRPO's binary verifiable reward with raw task output, and reduced training to "loss/style". Please be precise exactly there.
- We already have a 2-episode series on distillation (Hinton 2015 soft labels, Alpaca to R1-distill, "distillation attacks" and model extraction; distillation transfers behavior, not weights).
- GAP: what we lack is depth on (a) the human side of RLHF as an industrial process, (b) reward design as an engineering discipline and its failure modes, (c) the full landscape of "a model training a model" beyond classic distillation.

EPISODE 1. RLHF: a human teaches the model
1. Precise pipeline: SFT -> comparison data -> reward model (Bradley-Terry) -> PPO with KL penalty. What each step contributes, with numbers from InstructGPT (labeler count, comparisons, 1.3B beating 175B).
2. The labeling industry: who the raters are (Scale AI, Surge, contractors, in-house experts), pay, volumes, instructions/guidelines, inter-rater agreement rates actually reported, cost per comparison where public.
3. Formats of human feedback: pairwise, ranking of K, Likert, thumbs up/down in products, edits/rewrites, critiques. Which labs use which, and evidence of what works better.
4. Known pathologies: sycophancy, length bias, rater preference for confident tone, cultural bias of the rater pool. Cite the measured studies.
5. Offline alternatives (DPO, IPO, KTO, ORPO, SimPO): what they remove from the pipeline and what they lose. State the 2025-2026 consensus, if any.

EPISODE 2. Reward: how a model gets points for an answer
1. Kinds of reward: learned reward model (outcome RM), process reward model (step-level, "Let's Verify Step by Step"), verifiable reward (unit tests, math checkers, RLVR), rubric-based rewards, generative / LLM-judge rewards.
2. GRPO and successors: exact mechanism (group sampling, group-relative advantage, binary or graded reward, no value model). Show one worked numeric example of advantage computation. Distinguish reward from the answer itself.
3. Reward hacking and Goodhart: documented cases in LLMs (test tampering in coding agents, specification gaming, reward-model overoptimization scaling laws by Gao et al.). How labs detect and mitigate it in 2025-2026.
4. Where reward is impossible to verify (creative writing, advice, agents with long horizons): current approaches and their limits.
5. Practical: what a small team can actually run today (TRL, OpenRLHF, verl, compute ballpark for a 7B RLVR run).

EPISODE 3. One model teaches another
1. Map of the space with one line each and a seminal source: knowledge distillation; synthetic data / self-instruct; RLAIF and Constitutional AI; LLM-as-a-judge; self-play and self-rewarding models; debate; weak-to-strong generalization; teacher-student for reasoning traces (R1-distill); model-written evals and red-teaming.
2. Constitutional AI and RLAIF in depth: how AI feedback replaces human labels, measured comparison RLAIF vs RLHF (Lee et al. 2023 and later).
3. LLM-as-judge reliability: position bias, self-preference bias, agreement with humans, known numbers.
4. Risks: model collapse from recursive synthetic data (Shumailov et al. 2024 and rebuttals), bias amplification, the "who supervises the supervisor" problem, scalable oversight as an alignment research agenda.
5. Legal/business angle: terms-of-service bans on training on outputs, the 2025-2026 distillation disputes between labs (only mention what is sourced).

CROSS-CUTTING REQUIREMENTS
- For each episode give: 5-8 key ideas in teaching order; 3 vivid analogies AND where each analogy breaks; 3 common misconceptions with corrections; 5 interview-style questions with model answers; a short timeline of milestones with years.
- Separate clearly: established (multiple sources), likely, speculative. Mark every number with its source and year.
- For any claim of the form "the best method is X", name who disputes it.
- Prior art: list existing high-quality explainers/podcasts/lectures on each topic (Nathan Lambert RLHF book, HF blog, Chip Huyen, Karpathy, etc.) with URLs, so we know what is already well covered.
- Mandatory citations with working URLs (papers on arXiv, official lab blogs). No citation = do not state it.
- Epistemic neutrality: report disagreements between labs and researchers rather than picking a side silently.
- End with a decision-oriented section: "If you have only 3 episodes of 25 minutes, what must be in each, and what to cut."
- Language of the report: English. Length: as long as needed, structured with headings and tables.
