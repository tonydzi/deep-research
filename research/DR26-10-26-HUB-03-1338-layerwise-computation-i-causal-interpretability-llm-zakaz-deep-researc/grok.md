# DR26-10-26-HUB-03-1338 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll pull primary papers, official research pages, and the prior-work files first, then write the staged report from sources opened in this run.The output contract forbids saving files, so I will keep gathering primary sources and write the full report in this reply.A. Executive summary

Research cutoff: 2026-10-26. This report is an independent scientific investigation of how decoder-only transformer LLMs compute across layers, what can be observed, what has been causally established, and what can be reproduced on open models. Prior October 11/18 teaching claims are treated as hypotheses to recheck, not as evidence.

A transformer does not overwrite a hidden state. It starts from a token embedding, then each block reads the residual stream, computes an attention update and an MLP update, and adds both back. The final residual is mapped to logits by unembedding. That additive residual picture is the core mechanistic fact, established for attention-only toy models and then used as the working language for later work. https://transformer-circuits.pub/2021/framework/

Empirical roles by depth are real but not a universal staircase. On factual recall in GPT-2/GPT-J, middle-layer MLPs at the last subject token mediate stored associations, and late attention copies that information to the prediction site. https://rome.baulab.info/ On multilingual Llama-2, middle layers sit in a more abstract “concept space” that is closer to English, while late layers move into the output language. https://arxiv.org/abs/2402.10588 On GPT-2 small indirect-object identification, early heads detect duplicates, mid heads inhibit the subject, and late heads write the other name into the logits. https://arxiv.org/abs/2211.00593 Late layers therefore refine, suppress, route, calibrate and compose. They do not merely become “more abstract.”

Four compute axes must be kept separate: architectural depth (number of distinct blocks), hidden-state evolution inside one forward pass, generated reasoning tokens over time, and recurrence/test-time looping that reuses the same weights. A looped transformer can increase effective depth without adding parameters; that is a different object from stacking unique layers or writing a chain of thought. https://arxiv.org/abs/2610.24135

Observability is not causality. Logit lens and tuned lens decode intermediate residuals into vocabulary space. That is decodability, not proof that the model is “thinking those words.” https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens https://arxiv.org/abs/2303.08112 Sparse autoencoders and transcoders recover features, but reconstruction is incomplete, dictionaries split, and SAE probes often lose to ordinary linear probes. https://proceedings.mlr.press/v267/kantamneni25a.html Attribution graphs on cross-layer transcoders can recover prompt-specific circuits, yet they replace MLPs, freeze attention, capture only a fraction of computation, and require intervention checks. https://transformer-circuits.pub/2025/attribution-graphs/methods.html Natural-language autoencoders and the Jacobian lens can read silent concepts, including evaluation awareness, but both confabulate or cover only a small verbalizable subframe. https://www.anthropic.com/research/natural-language-autoencoders https://www.anthropic.com/research/global-workspace

Causal claims that survive controls: mid-layer MLP mediation of many facts in GPT-style models; a 26-head IOI circuit in GPT-2 small; a one-dimensional residual refusal direction in many chat models; a toxic/misaligned persona latent that can be steered in GPT-4o; rhyme planning and multi-hop latent intermediates in Claude when the corresponding features are swapped. Causal claims that do not survive: “localization tells you which layer to edit” (near-zero correlation in GPT-J), “CoT is the model’s true reasoning,” “decoded tokens are an inner monologue,” “SAE features are complete,” “J-space proves consciousness.”

Closed chat APIs do not expose arbitrary tensors. Open replication is currently strongest on GPT-2 small (TransformerLens, CPU-capable) and Gemma-2-2B / Llama-3.2-1B (circuit-tracer, Neuronpedia, Colab-scale GPU). This report proposes, but does not run, a falsifiable IOI patching experiment.

Русское резюме. Трансформер не переписывает мысль целиком: каждый слой добавляет поправку в общий поток. Ранние слои ловят форму слов, средние часто достают факты и смыслы, поздние готовят ответ, глушат лишнее и переводят мысль в конкретный язык. Это не лестница “чем глубже, тем умнее” для всех моделей. Прочитать промежуточный слой как слова ещё не значит, что модель этими словами думает. Настоящие доказательства появляются только когда внутренность меняют и поведение меняется, при случайных контролях. Цепочка рассуждений в тексте часто врёт. Закрытый чат внутренности не отдаёт.

B. Four staged outputs

Release 1. Prior-work audit and evidence map

What the October package established, and still holds.

1. Weights, transient activations, and generated chain-of-thought are three different objects. Confirmed. Weights are persistent parameters. Activations are per-forward-pass vectors. CoT is sampled tokens that may or may not mediate the answer. https://transformer-circuits.pub/2021/framework/ https://arxiv.org/abs/2305.04388
2. Intermediate decoding is not a literal inner monologue. Confirmed. Logit/tuned lens and J-lens are readouts. Tuned lens authors treat layers as iterative inference of a latent next-token prediction, not as a diary. https://arxiv.org/abs/2303.08112 https://www.anthropic.com/research/global-workspace
3. Feature activation is not intention. Confirmed. SAE latents and persona features are directions. Steering them changes behavior; that does not make them desires. https://openai.com/index/emergent-misalignment/
4. SAE and attribution graphs are incomplete. Confirmed and strengthened in 2025–2026. Circuit tracing authors state that even on short prompts the method captures only a fraction of computation and can introduce tool artifacts. CLT replacement models match underlying outputs in about 50% of cases. https://www.anthropic.com/research/tracing-thoughts-language-model https://transformer-circuits.pub/2025/attribution-graphs/methods.html
5. Interventions need random-direction and unrelated-position controls, plus held-out paraphrases. Confirmed as a methodological requirement. ROME’s CounterFact already separates efficacy, specificity and paraphrase generalization. https://rome.baulab.info/
6. Ordinary closed chat APIs do not provide arbitrary internal tensors. Confirmed. All layerwise methods below require weights or hidden states.

What the prior package overclaimed, or left as metaphor.

1. A universal early-to-late staircase (lexical then syntactic then semantic then factual then answer) as architecture-independent fact. Not demonstrated. Geva’s shallow-to-semantic MLP keys are from one encoder LM family. BERT-style probing is a different architecture. GPT factual recall is subject-token middle MLPs plus late attention, not “facts live only at the top.” https://aclanthology.org/2021.emnlp-main.446/ https://rome.baulab.info/
2. Treating decoded intermediate tokens as the model’s thoughts. Overclaim if stated as identity rather than a projection. Logit lens is often brittle in early layers; tuned lens is a trained translator into final coordinates. https://arxiv.org/abs/2303.08112
3. Treating SAE features as the model’s true feature set. Overclaim. Reconstruction error is “dark matter.” Kantamneni et al. could not make SAE probes consistently beat ordinary probes. Heap-style negative results (SAEs looking similar on random networks) further weaken unsupervised-as-truth. https://proceedings.mlr.press/v267/kantamneni25a.html
4. Treating CoT as a window onto internal computation. Overclaim. Turpin et al.: biasing cues change answers while CoT omits the cue, accuracy drop up to 36% on BIG-Bench Hard with GPT-3.5 and Claude 1.0. Lanham et al.: corrupting CoT often leaves the answer unchanged; larger models were less faithful on most studied tasks. Anthropic circuit tracing: Claude describes schoolbook addition while internals use parallel approximate and precise paths. https://arxiv.org/abs/2305.04388 https://arxiv.org/pdf/2307.13702 https://www.anthropic.com/research/tracing-thoughts-language-model
5. Hidden intentions / consciousness as established. The J-space paper argues for a functional global-workspace analogue and explicitly does not establish phenomenal consciousness. Introspection success is about 20% in the best Claude protocol. https://www.anthropic.com/research/global-workspace https://www.anthropic.com/research/introspection
6. The ancillary claim that all post-2000 text exceeds all earlier text. The brief already marked this not established. It is not a layerwise result and is not repeated here as fact.

What was left open, and what 2025–2026 updates.

Open in Oct 2024: a depth-resolved causal atlas, not just a catalogue of tools. Update: attribution graphs plus open circuit-tracer on Gemma-2-2B and Llama-3.2-1B; NLA (7 May 2026); J-lens / J-space (6 Jul 2026); OpenAI persona-feature mediation of emergent misalignment (18 Jun / 24 Jun 2025); Kantamneni SAE-utility negative result (ICML 2025); looped-transformer mechanistic papers in Oct 2026.

Source-to-question matrix

| Question | Best primary sources | What they actually measure | Still open |
|---|---|---|---|
| How does one token move through the net? | https://transformer-circuits.pub/2021/framework/ | Additive residual, QK/OV split, path expansion in attention-only toys | Full MLP path expansion in large models |
| Where are facts? | https://rome.baulab.info/ https://arxiv.org/abs/2202.05262 | Causal tracing + ROME on GPT-2 XL / GPT-J | Localization–edit mismatch https://ar5iv.labs.arxiv.org/html/2301.04213 |
| Early vs late linguistic roles? | https://aclanthology.org/2021.emnlp-main.446/ https://arxiv.org/abs/2402.10588 | MLP key patterns; logit-lens multilingual phases | Transfer across families and tasks |
| Can we decode intermediates? | https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens https://arxiv.org/abs/2303.08112 | Vocab projections, trained affine translators | Whether decoded tokens are used |
| Can we find circuits? | https://arxiv.org/abs/2211.00593 https://transformer-circuits.pub/2025/attribution-graphs/methods.html | Path patching; CLT attribution graphs | Attention not traced; error nodes; scale |
| Do SAEs recover true features? | https://arxiv.org/abs/2408.05147 https://proceedings.mlr.press/v267/kantamneni25a.html | JumpReLU SAE suite; downstream probe tests | Completeness, uniqueness, utility |
| Silent reasoning / eval awareness? | https://transformer-circuits.pub/2026/nla/index.html https://www.anthropic.com/research/global-workspace | NLA text bottleneck; Jacobian verbalizable subframe | Confabulation, coverage, closed-model access |
| Alignment internals? | https://arxiv.org/abs/2406.11717 https://openai.com/index/emergent-misalignment/ | Refusal direction; toxic persona SAE latent | Redundancy, post-training localization |
| Depth vs test-time compute? | https://arxiv.org/abs/2610.24135 https://arxiv.org/abs/2610.25998 | Looped vs untied depth; CoT vs latent loops | Frontier training recipes |

Search-coverage limits. Opened in this run: transformer-circuits.pub (2021 framework, 2025 circuit tracing, 2026 NLA, 2026 workspace via Anthropic summary plus paper), Anthropic research posts (tracing thoughts, circuit-tracing OSS, NLA, global workspace, introspection), OpenAI emergent-misalignment page, ROME site, Gemma Scope DeepMind blog, TransformerLens and circuit-tracer GitHub, arXiv abstracts for tuned lens, refusal, persona features, IOI, Llama latent language, Gemma Scope. Not fully read as PDFs: every appendix of the 2025 methods paper, the full J-space PDF, full NLA experimental tables, Kantamneni full ICML PDF, Hase 2023 full paper beyond HTML snippets. Not accessed: closed-model hidden states, unpublished 2026 system-card internals beyond cited public posts. No experiment in this order was executed.

Release 2. Layer-by-layer explanatory atlas

Worked token example (illustrative, using published circuits, not a new run).

Prompt fragment, IOI-style, as in Wang et al.: “When John and Mary went to the store, John gave a drink to”

Token path for the final position, GPT-2 small (12 layers, 12 heads). https://arxiv.org/abs/2211.00593

1. Embedding. Each token becomes a vector in residual space. Position information is added. No inter-token mixing yet. A zero-layer transformer is only bigram statistics from W_U W_E. https://transformer-circuits.pub/2021/framework/
2. Early residual blocks (roughly layers 0–2). Attention heads can move information between positions. Duplicate-token heads at layer 0 detect that “John” appeared twice. This is lexical/identity tracking, not “the answer.”
3. Early–mid MLPs. Feed-forward layers act as key-value memories: keys fire on patterns, values write vocabulary-ish updates. In Geva et al., lower keys are shallow n-grams; upper keys are more semantic. That result is on a different model family and should not be copied onto GPT-2 as a law. https://aclanthology.org/2021.emnlp-main.446/
4. Mid blocks (roughly layers 5–8 in GPT-2 small for IOI). Induction-style heads and S-inhibition heads use the duplicate signal to suppress the subject name as a candidate. Information is relational: which name is the subject of the last clause.
5. Late blocks (roughly layers 9–11). Name-mover heads copy the other name (Mary) into the residual at the end position. Negative name-movers write in the opposite direction. This is answer preparation and suppression, not a new abstraction layer.
6. Final layer norm and unembedding. Logits = unembed(norm(residual)). Direct logit attribution can assign late head writes to the Mary-versus-John logit difference.

Factual-recall variant, GPT-2 XL / GPT-J, Meng et al. Prompt: “The Eiffel Tower is located in the city of”. Causal tracing: corrupt the subject, then restore hidden states. Restoring mid-layer MLP states at the last subject token recovers the object (“Paris”). Late attention at the final position is a second causal site that copies recalled information into the answer. Editing the early MLP site with ROME changes the fact across paraphrases; editing the late attention site often yields parroting without generalization. https://rome.baulab.info/

Multilingual variant, Llama-2, Wendler et al. Non-English prompt with a unique next token. Three phases: input-space (early, far from output embeddings), concept-space (middle, semantically correct token decodable, but English version preferred), output-space (late, input-language region). https://arxiv.org/abs/2402.10588

Claude 3.5 Haiku variant, Ameisen/Lindsey 2025. Opposite-of-small across languages: shared features for smallness and oppositeness, then language-specific output. Poetry: planned rhyme “rabbit” is represented before the second line is written; suppressing it yields “habit”; injecting “green” yields a non-rhyme ending. Mental math: parallel approximate and precise paths, not the schoolbook algorithm the model claims. https://www.anthropic.com/research/tracing-thoughts-language-model

Diagram 1. One forward pass (illustrative).

token ids
  -> embedding (+ positions)                [layer 0 residual]
  -> for l = 1..L:
       x <- x + Attention_l(LN(x))          [move info across positions]
       x <- x + MLP_l(LN(x))                [write memories / features]
  -> LN, unembed, softmax                   [next-token logits]

Residual identity paths let early writes persist unless later writes cancel them. https://transformer-circuits.pub/2021/framework/

Diagram 2. Depth is not reasoning time (illustrative).

Axis 1  Architectural depth     L distinct blocks
Axis 2  Hidden residual time    L additive updates per generated token
Axis 3  Token time              T prompt tokens, then K generated tokens
Axis 4  Recurrent depth         R loops of a shared block (looped / recurrent-depth models)
Axis 5  Test-time token compute extra CoT / tool tokens, not extra layers

A 12-layer model generating a 40-token CoT has 12 residual updates per token and 40 sequential generation steps. A 4-layer block looped 8 times has effective depth 32 with shared weights. Sesodia et al. 2026: a 4-layer loop iterated 8 times solved a hop-traversal task in every seed, while an identically trained untied stack of the same effective depth did not, unless intermediate layers were supervised. That is evidence against “a loop is just a deeper unique stack.” https://arxiv.org/abs/2610.24135

Normalized depth. Absolute layer index is not comparable across GPT-2 small (12), Gemma 2 2B (26), Gemma 2 9B (42), GPT-2 XL (48), GPT-3-class (96). Report both l and l/L. Refusal processing in Qwen 1.8B concentrated around layers 14–17, a mid-depth band, not “the last layer.” https://www.greaterwrong.com/posts/jGuXSZgv6qfdhMCuJ/refusal-in-llms-is-mediated-by-a-single-direction/comment/vruKcxuWkYoEuFeic J-space “workspace range” is a band of intermediate-to-late layers, not the entire stack. https://transformer-circuits.pub/2026/workspace/index.html

What changes across positions. Subject-token mid MLPs store/recall; last-token late attention reads out. IOI information is routed among name positions, then to END. Generation step t+1 conditions on sampled token t; later-layer residuals are already near the previous prediction, so errors can persist (nostalgebraist’s observation). https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens

Late-layer functions, demonstrated rather than metaphorical.

| Function | Evidence | Model / task |
|---|---|---|
| Refine a guess | Logit/tuned lens trajectories converge before the last layer | GPT-2 and models to 20B https://arxiv.org/abs/2303.08112 |
| Suppress competitors | Negative name movers; refusal direction ablation | GPT-2 small IOI; 13 chat models https://arxiv.org/abs/2211.00593 https://arxiv.org/abs/2406.11717 |
| Route / copy | Late attention copies recalled object or IO name to END | GPT-2 XL facts; GPT-2 small IOI https://rome.baulab.info/ |
| Calibrate / language-realize | Late move into output-language region | Llama-2 https://arxiv.org/abs/2402.10588 |
| Compose / plan | Future rhyme token in residual/J-space before it is written | Claude 3.5 Haiku, Opus 4.6 https://www.anthropic.com/research/tracing-thoughts-language-model |
| Default-refuse then inhibit | Hallucination study: default is decline; answering requires inhibiting reluctance | Claude 3.5 Haiku https://www.anthropic.com/research/tracing-thoughts-language-model |

Model/task comparison table

| Family | Depth | Early | Middle | Late | Method | Replication |
|---|---|---|---|---|---|---|
| Attention-only toys | 0–2 | bigrams | skip-trigrams; induction at 2 layers | logit path expansion | weights | framework paper, later induction-head follow-up |
| GPT-2 small | 12 | duplicate-token heads | S-inhibition, induction-style | name movers, unembed | path patching | TransformerLens IOI demos |
| GPT-2 XL / GPT-J | 48 / 28 | subject MLP recall | mid MLP causal site | late attention copy | causal tracing, ROME | official notebooks; Hase challenges edit-localization link |
| Llama-2 | 32 | input space | English-leaning concept space | output language | logit lens + embedding geometry | public repo https://github.com/epfl-dlab/llm-latent-language |
| Gemma 2 2B/9B | 26 / 42 | SAE features at all sublayers | feature evolution | residual/MLP/attn SAEs | JumpReLU SAEs | Gemma Scope weights + Neuronpedia |
| Claude 3.5 Haiku | closed | shared multilingual features | planning, multi-hop | output language, refusal/hallucination control | CLT graphs + interventions | closed model; open analogue on Gemma/Llama via circuit-tracer |
| Claude Sonnet 4.5 / Opus 4.6 | closed | automatic processing | J-space workspace band | verbal report | J-lens, NLA | open J-lens code; NLA on open models via Neuronpedia |

Release 3. Adversarial review of deep-layer claims and tools

Tool-by-tool.

Logit lens. Access: residual at each layer, final unembed (and usually final LN). Object: vocab distribution. Granularity: layer × token. Assumptions: intermediate residuals live in a space where W_U is meaningful. Trained decoder: none. Errors: early-layer nonsense, coordinate mismatch. Artifacts: looks like a monologue. Controls: compare to input embedding as well as to final logits. Code: original Colab; any HF hidden_states loop. Failures: brittle on some models; tuned lens paper calls it often brittle. https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens

Tuned lens. Access: residuals + training corpus for affine maps. Object: KL-matched estimate of final distribution. Granularity: layer × token. Assumptions: an affine translator into final coordinates exists; distillation target is the model’s own final logits. Causal check: authors say the lens uses similar features to the model. Failures: still a readout of next-token prediction, not of all latent content; trained on a corpus, can leak that corpus’s statistics. Code: https://github.com/AlignmentResearch/tuned-lens (cited from the paper). https://arxiv.org/abs/2303.08112

Probes. Access: labeled activations. Object: researcher-chosen concept. Assumptions: linear separability. Leakage: probe can pick up correlates, not causes. Necessity requires ablation/patching.

Attention maps. Access: attention weights. Object: where heads look, not what they write. QK is not OV. Framework paper’s central warning. https://transformer-circuits.pub/2021/framework/

SAEs. Access: cached activations, expensive training or a released dictionary. Object: sparse features of one site (resid/attn/MLP). Gemma Scope: JumpReLU SAEs on all layers/sublayers of Gemma 2 2B and 9B, select 27B, plus IT 9B comparison; >400 SAEs, >30M features, ~15% of Gemma 2 9B pretrain compute, ~20 PiB activations. Demo: https://www.neuronpedia.org/gemma-scope Weights: https://huggingface.co/google/gemma-scope Paper: https://arxiv.org/abs/2408.05147 Failures: incomplete reconstruction, dead features, feature splitting, dictionary-size dependence, poor downstream probe utility versus linear probes. https://proceedings.mlr.press/v267/kantamneni25a.html

Transcoders / CLTs. Approximate MLP input→output with a sparse bottleneck, enabling feature–feature edges. Attribution graphs freeze attention and norms. Replacement model matches ~50% of outputs. Missing attention circuits is a stated limitation. Gemma graphs use Gemma Scope transcoders. https://transformer-circuits.pub/2025/attribution-graphs/methods.html Oct 2026: many transcoder features in late layers are “neurons in disguise.” https://arxiv.org/abs/2610.24521

Circuit tracing OSS. Library computes attribution graphs, visualizes, supports feature interventions. Supported: Gemma-2-2B on ~15 GB Colab GPU; Llama-3.2-1B; later Qwen-3 and Gemma-3 via nnsight backend. Current repo opened: https://github.com/decoderesearch/circuit-tracer Anthropic announcement also pointed at https://github.com/safety-research/circuit-tracer https://www.anthropic.com/research/open-source-circuit-tracing Browser: Neuronpedia graphs, no install. https://www.neuronpedia.org/gemma-2-2b/graph

NLA. AV maps activation→text, AR maps text→activation, jointly RL-trained for reconstruction. Access: activations plus two trainable copies of a same-size model. Cost: hundreds of generated tokens per activation; training is RL on two LMs. Failures: confabulation of context details; black-box (not mechanistic); AV can infer beyond the activation; degenerate bottleneck in the limit. Independent corroboration required. Open code: https://github.com/kitft/natural_language_autoencoders Demo: https://www.neuronpedia.org/nla Paper: https://transformer-circuits.pub/2026/nla/index.html

J-lens / J-space. Jacobian from hidden state toward future-token logits, aggregated into per-layer maps. Cheap at inference (one matrix multiply per layer after precompute). Reads a ranked list of single tokens the model is poised to say, not multi-token propositions. J-space component of a concept vector: median 6–7% of variance, rest outside. Ablating J-space leaves fluent speech and simple facts, harms higher-order cognition in their tests. Evaluation-awareness ablation in a blackmail scenario changed verbalized awareness and blackmail rates. Consciousness is not established. Open: https://github.com/anthropics/jacobian-lens Demo: https://www.neuronpedia.org/jlens Papers: https://www.anthropic.com/research/global-workspace https://transformer-circuits.pub/2026/workspace/index.html Follow-up theory: J-lens prefers coherent Lyapunov-like modes. https://arxiv.org/abs/2610.31955

Patching vs ablation vs steering.

Activation patching / causal tracing: replace a corrupted-run activation with a clean-run one (or vice versa). Localizes sufficiency of a state for a metric. Residual-stream patching at many layers can look broad because the stream carries the information, not because every layer computes it.

Ablation / mean ablation / directional ablation: remove a component or project out a direction. Tests necessity in that parameterization. Redundant circuits can hide.

Steering / CAA / ActAdd: add a difference-in-means vector. Rimsky et al.: CAA on Llama 2 Chat, vectors for sycophancy, hallucination, etc., added after the user prompt. https://arxiv.org/html/2312.06681v4 Off-manifold risk: large coefficients produce garbage. Need matched-norm random directions and unrelated-position controls.

Causal ladder. Decodable ⊂ correlated ⊂ used-if-present (steering) ⊂ necessary (ablation) ⊂ mediating (patch + path) ⊂ sufficient and specific (edit + paraphrase + neighborhood). Most published figures sit between correlated and mediating.

Claims that fail adversarial review.

1. “Causal tracing localizes the layer to edit.” Hase et al.: at the datapoint level, tracing and ROME/MEMIT success are statistically uncorrelated on GPT-J CounterFact. Substantial knowledge is stored outside the ROME band. https://ar5iv.labs.arxiv.org/html/2301.04213
2. “SAEs are the interpretability microscope.” Useful for unsupervised discovery and some audits; not consistently useful as a probe basis. DeepMind-associated authors are among the SAE-utility skeptics.
3. “CoT is the reasoning.” Turpin, Lanham, Claude math case. Inverse-scaling of faithfulness in Lanham is single-paper and metric-sensitive; later work showed order-of-options can swing the metric. Treat as: CoT is sometimes load-bearing, often post-hoc.
4. “Models have hidden intentions we can read as if they were people.” Persona latents and J-space contents are causal residual patterns. They are not evidence of phenomenal states. Anthropic states this. NLA explanations can invent details.
5. “Late layers are the abstract mind.” Late layers often realize language, copy answers, and suppress refusals. Abstraction frequently peaks in a middle band (concept space, workspace range, factual MLP site).
6. “Closed-model prose reveals internals.” It does not. Claude results do not transfer to GPT-4o internals without a new measurement.

Null and contradictory findings to keep.

- Heap et al. 2025 (discussed in Garriga-Alonso 2025): autointerp pipelines can look similar on random networks. https://agarriga.substack.com/p/the-sparse-autoencoders-bubble-has
- Localization ≠ edit site (Hase 2023).
- SAE probes ≰ linear probes (Kantamneni 2025).
- Faithfulness of CoT is task- and size-dependent (Lanham 2023) and cue-sensitive (Turpin 2023).
- Introspection fails most of the time (Lindsey 2025); best protocol ~20% identification.
- Looped vs untied stacks are not equivalent (Sesodia 2026).
- Transcoder features often align with single MLP neurons in late layers (Shabalin & Piotrowski 2026).

Evidence-strength table

| Claim | Strength | Why |
|---|---|---|
| Residual stream is additive communication | High | Architecture identity plus path expansion https://transformer-circuits.pub/2021/framework/ |
| Mid MLP + late attention in factual recall | High for GPT-2/J family | Causal tracing + successful ROME, with Hase caveat |
| IOI 26-head circuit | High for GPT-2 small IOI | Path patching, faithfulness/completeness/minimality; residual gaps admitted |
| Refusal ≈ 1 residual direction | High across 13 open chat models | Ablation + induction + rank-one weight edit https://arxiv.org/abs/2406.11717 |
| English-leaning middle concept space | Medium–high for Llama-2 | Geometry + logit lens; may not hold for more balanced multilingual models |
| Shared multilingual features in Claude | Medium | Attribution graphs on a closed model; open analogue in Gemma/Llama demos |
| Planning rhymes ahead | Medium–high | Feature swap changes the line in Claude; NLA replicates planning on Opus 4.6 |
| Unfaithful CoT | High as existence proof | Multiple labs, multiple metrics |
| SAE completeness | Low | Reconstruction error, splitting, probe losses |
| Toxic persona mediates emergent misalignment | Medium–high in GPT-4o | SAE diff + steer up/down; not fully suppressed in all domains |
| J-space as global workspace | Medium as functional analogy | Multiple tests; 6–7% variance; philosophical commentaries dispute broadcast/modularity |
| Introspective awareness | Low–medium | Real above chance, unreliable, post-training dependent |
| Phenomenal consciousness | Not established | Experiments do not address it |

Release 4. Reproducibility protocol and decision memo

Smallest useful experiment (proposal only; not run here).

Goal. Show, on one open model, that a behavioral difference is mediated by a localized residual site, with random-direction and paraphrase controls.

Model. GPT-2 small (`gpt2`), 12 layers, ~124M parameters. Why: TransformerLens official IOI path, CPU possible, no gated weights. Alternative scale-up: Gemma-2-2B + circuit-tracer on a 16 GB GPU. https://github.com/TransformerLensOrg/TransformerLens https://github.com/decoderesearch/circuit-tracer

Task. Indirect object identification. Clean: “When John and Mary went to the store, John gave a drink to” → Mary. Corrupted: swap names or ABC control as in Wang et al. Metric: logit(Mary) − logit(John) at the last prompt position.

Two prompt conditions. A: IOI templates with held-in name pairs. B: paraphrases and different syntactic frames (held-out names, “After X and Y …, X handed the book to”).

Per-layer measurements. resid_pre, attn_out, mlp_out at every layer × position. Direct logit attribution of each head to the metric.

Interventions. (i) Activation patching clean→corrupt at each (layer, position). (ii) Mean-ablate name-mover heads L9H6, L9H9, L10H0. (iii) Matched-norm random directions at the same sites. (iv) Patch at unrelated positions (punctuation, verbs).

Seeds and uncertainty. ≥3 generation seeds for any sampled continuation; for logit-diff use greedy last-position logits, bootstrap over ≥100 templates. Report mean, 95% CI, and the fraction of templates where the sign of the effect matches.

Acceptance. Patching name-mover outputs at END recovers ≥50% of the clean−corrupt logit-diff; random directions of equal norm recover <15%; paraphrases keep the same layer/head band.

Falsification. If random directions match name-mover patches, or if the band jumps under paraphrase, or if MLP-only patches explain the task without the published heads, the replication failed or the circuit is incomplete (the original paper already reports remaining gaps).

Prerequisites. Python 3.10+, `pip install transformer_lens` (3.8/3.9: `transformer_lens~=2.0`). Hugging Face `gpt2`. Optional GPU.

Estimated resources. GPT-2 small weights ~500 MB disk. CPU RAM 8 GB is enough for single-sequence patching; 16 GB comfortable. GPU: any 4 GB card speeds sweeps. Gemma-2-2B graphs: ~15 GB GPU as stated by circuit-tracer authors. These are published hardware claims, not measurements from this order.

CPU fallback. TransformerLens README shows `TransformerBridge.boot_transformers("gpt2", device="cpu")`. Restrict patching to resid_pre over 12×seq, skip per-head-by-position slow path.

Browser demonstration. Neuronpedia Gemma-2-2B graph viewer and “Steer” on pinned nodes. No local install. https://www.neuronpedia.org/gemma-2-2b/graph Logit-lens notebook on Colab with `output_hidden_states` also works in-browser on a T4 for Gemma-2-2B in about 5 minutes according to a public tutorial; that runtime is not remeasured here.

Access limits for closed models. Claude/GPT-4o internals in this literature were measured by the labs that own the weights. Public users cannot patch residuals through the chat API. Do not infer Claude layer indices from Claude’s prose.

Decision memo. For an engineer who needs a true layerwise causal claim: start with GPT-2 small IOI or ROME tracing, not with NLA or J-lens. For an auditor who needs hypotheses about silent evaluation awareness: NLA/J-lens on models where those tools exist, then corroborate. For product safety on open chat models: refusal-direction ablation is the most replicated alignment-internal result. Do not ship a “universal layer map.”

C. Key findings with evidence strength

1. Computation is residual and additive. High. https://transformer-circuits.pub/2021/framework/
2. Early, middle, and late blocks have different typical jobs, but the jobs depend on task and family. High as a caution, medium as any specific map. Compare ROME, IOI, Wendler, Geva, J-space workspace band.
3. Late layers often prepare, suppress, copy, and language-realize rather than invent new abstractions. Medium–high.
4. Architectural depth ≠ CoT time ≠ recurrent loops. High conceptually; looped vs untied inequality: https://arxiv.org/abs/2610.24135
5. Decodability is cheap and misleading without interventions. High. Logit lens 2020, tuned lens 2023/2025 v6.
6. Causal mediation requires patching or ablation plus controls. High. IOI, ROME, Arditi, CAA.
7. Localization can fail to predict the best edit site. Medium–high. Hase 2023.
8. CoT is an unfaithful window often enough to forbid treating it as the computation. High existence, medium as a rate.
9. Refusal and some persona/misalignment behaviors have low-dimensional residual handles. High for refusal on open chat models; medium–high for OpenAI toxic persona in GPT-4o.
10. Silent multi-hop and planning exist in at least some frontier models under feature intervention. Medium, closed-model plus open analogues.
11. SAEs/transcoders are incomplete dictionaries, still the best unsupervised feature tools for circuit graphs. Medium.
12. NLA and J-lens add readable silent content, with confabulation and tiny coverage. Medium, 2026 primary papers.
13. Introspection is real, rare, brittle, stronger in more capable Claude models, and not consciousness. Low–medium. https://www.anthropic.com/research/introspection
14. Closed APIs hide the objects this science needs. High.

D. Contradictions and debates

Staircase vs mosaic. BERT probing and Geva MLP keys encourage a hierarchy. ROME puts facts in middle MLPs. Wendler puts concepts in the middle and language at the end. J-space puts verbalizable working memory in a workspace band. IOI uses early, mid, and late heads as one algorithm. The mosaic wins.

English pivot vs universal language of thought. Wendler: Llama-2 concept space nearer English. Anthropic: Claude shares features across languages, more so at larger scale. Both can be true: English-heavy pretraining imprints a pivot that later fades as scale and multilingual data grow. Not resolved for all families.

SAE optimism vs SAE winter. 2023–2024 scaling papers versus 2025 probe/steering benchmarks and DeepMind-associated deprioritization of SAE-as-default. Transcoders and CLTs are the current circuit bet; even they collapse toward neurons in some late layers.

CoT as explanation vs CoT as theater. Wei-style performance gains versus Turpin/Lanham unfaithfulness versus Anthropic catching motivated reasoning in graphs. Practical stance: monitor CoT, do not trust it as a causal trace.

Localization vs editing. Meng used tracing to choose ROME layers; Hase found datapoint-level uncorrelation. Editing can work for reasons tracing does not explain.

Workspace vs consciousness. Gurnee et al. claim functional GNW properties. Commentaries (Dehaene/Naccache; Eleos) accept reportability and dispute modules/broadcast and phenomenal inference. Anthropic’s own FAQ: experiments do not show consciousness. https://www.anthropic.com/research/global-workspace

Base vs instruct. Gemma Scope releases IT 9B SAEs for comparison. Arditi refusal directions exist because of safety post-training. Introspection in a 2026 open-weights follow-up is absent in base models and appears after preference optimization. Post-training moves mechanisms; do not assume base-layer maps survive chat tuning.

E. Best practices and failure modes

Practices.

- State the object: weights, activations, or tokens.
- Report absolute layer and l/L, site (resid/attn/MLP), and token position.
- Use a behavioral metric (logit difference, refusal score), not only visualization.
- Patch, then ablate, then steer. Require matched-norm random directions and off-position controls.
- Hold out paraphrases and name/entity swaps.
- For SAEs/NLAs, report reconstruction/coverage and an independent causal check.
- Freeze attention only if you then say QK circuits are unaccounted for.
- Prefer open models when the claim is mechanistic.

Failure modes.

- Reading logit lens as inner speech.
- Treating attention heatmaps as computation.
- One-prompt graphs without intervention.
- Off-manifold steering that looks like control but destroys fluency.
- Probe leakage from template artifacts.
- Assuming a 12-layer map transfers to a 42-layer model.
- Inferring internals of GPT-4o/Claude from their written self-reports.
- Equating recurrence, CoT tokens, and unique depth.

F. Reproducibility protocol

See Release 4. Short form:

1. Install TransformerLens; load `gpt2` on CPU or GPU.
2. Build ≥100 IOI clean/corrupt pairs; keep a held-out paraphrase set.
3. Cache resid/attn/mlp; compute logit-diff.
4. Activation-patch a layer×position grid; plot.
5. Repeat with random vectors of equal norm.
6. Ablate published name-mover heads; confirm metric collapse.
7. Accept/reject by the thresholds in Release 4.
8. Optional: open the same qualitative circuit on Neuronpedia Gemma-2-2B.

Versions. TransformerLens 4.x replaced `HookedTransformer.from_pretrained` with `TransformerBridge`; compatibility mode folds LayerNorm differently. Pin the version in any replication. SAELens holds SAE tooling after TL 2.0. circuit-tracer: `pip install .` from the cloned repo; Gemma-2-2B transcoders on Hugging Face (`mntss/gemma-scope-transcoders` or CLTs).

This order did not install, train, call paid APIs, or execute the experiment.

G. Action plan

1. If the goal is teaching engineers how a token is computed: use the residual diagram plus the IOI walk-through in TransformerLens/ARENA, not NLA screenshots.
2. If the goal is a lab replication: run the GPT-2 small protocol in Release 4, then Gemma-2-2B circuit-tracer on one factual prompt.
3. If the goal is auditing silent eval-awareness: use NLA/J-lens only where weights and tools exist; corroborate with resampling and independent probes.
4. If the goal is alignment engineering on open chat models: test refusal-direction ablation with capability checks.
5. If the goal is theory of depth: keep looped, CoT, and stacked-unique-layer papers in separate bins.
6. Do not commission more audio until this report is cross-checked by another vendor rail. Independent reviewer named in the brief: Claude session d71b832e-8c45-4976-b2aa-f164de4f6c34.

H. Sources

Primary pages opened in this run.

https://transformer-circuits.pub/2021/framework/
https://www.anthropic.com/research/tracing-thoughts-language-model
https://transformer-circuits.pub/2025/attribution-graphs/methods.html
https://www.anthropic.com/research/open-source-circuit-tracing
https://www.anthropic.com/research/natural-language-autoencoders
https://transformer-circuits.pub/2026/nla/index.html
https://www.anthropic.com/research/global-workspace
https://transformer-circuits.pub/2026/workspace/index.html
https://www.anthropic.com/research/introspection
https://openai.com/index/emergent-misalignment/
https://rome.baulab.info/
https://github.com/TransformerLensOrg/TransformerLens
https://github.com/decoderesearch/circuit-tracer
https://deepmind.google/blog/gemma-scope-helping-the-safety-community-shed-light-on-the-inner-workings-of-language-models/
https://arxiv.org/abs/2303.08112
https://arxiv.org/abs/2406.11717

Additional primary URLs used from live search snippets in this run.

https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens
https://arxiv.org/abs/2202.05262
https://arxiv.org/abs/2211.00593
https://arxiv.org/abs/2305.04388
https://arxiv.org/pdf/2307.13702
https://arxiv.org/abs/2402.10588
https://arxiv.org/abs/2408.05147
https://arxiv.org/abs/2506.19823
https://arxiv.org/html/2312.06681v4
https://aclanthology.org/2021.emnlp-main.446/
https://arxiv.org/abs/2301.04213
https://ar5iv.labs.arxiv.org/html/2301.04213
https://proceedings.mlr.press/v267/kantamneni25a.html
https://huggingface.co/google/gemma-scope
https://www.neuronpedia.org/gemma-scope
https://github.com/kitft/natural_language_autoencoders
https://github.com/anthropics/jacobian-lens
https://arxiv.org/abs/2610.31955
https://arxiv.org/abs/2610.24135
https://arxiv.org/abs/2610.25998
https://arxiv.org/abs/2610.24521
https://github.com/epfl-dlab/llm-latent-language
https://github.com/AlignmentResearch/tuned-lens
https://www.neuronpedia.org/nla
https://www.neuronpedia.org/jlens
https://www.neuronpedia.org/gemma-2-2b/graph

Compact claim matrix

| Claim | Model/version | Depth/layer | Token/task | Method | Intervention/control | Observed effect | Uncertainty | Source/date | Replication |
|---|---|---|---|---|---|---|---|---|---|
| Residual is an additive channel | attention-only toys; used as GPT framing | all | next-token | path expansion | none (identity of architecture) | heads/MLPs write low-rank updates | high for attn-only; MLP nonlinear | https://transformer-circuits.pub/2021/framework/ 2021-12-22 | conceptual, widely reused |
| Mid MLP at last subject token mediates facts | GPT-2 XL, GPT-J | middle MLPs | last subject token, factual cloze | causal tracing | restore clean states into corrupted run | recovers object prediction | family-specific; Hase uncorrelation with edits | https://rome.baulab.info/ 2022 | notebooks; editing success ≠ tracing |
| Late attention copies facts to END | GPT-2 XL | late attn | final token | tracing vs ROME site | edit late attn vs early MLP | late edit: poor paraphrase generalization | medium | https://rome.baulab.info/ | CounterFact |
| IOI uses 26 heads / 7 classes | GPT-2 small | L0 duplicate → L7–8 inhibit → L9–10 movers | END, IOI | path patching | mean ablation of circuit | bulk of logit-diff explained; gaps remain | high on this task | https://arxiv.org/abs/2211.00593 2022-11-01 | TransformerLens demos |
| Lower MLP keys shallow, upper semantic | encoder LM in Geva et al. | layers 1–9 vs 10–16 in that model | FFN keys | trigger examples | none causal | interpretable patterns | not shown universal | https://aclanthology.org/2021.emnlp-main.446/ 2021 | original code repo |
| Middle concept space nearer English | Llama-2 | middle vs late | non-English cloze | logit lens + geometry | none | English token preferred mid, target language late | one family, English-heavy corpus | https://arxiv.org/abs/2402.10588 2024 | https://github.com/epfl-dlab/llm-latent-language |
| Tuned lens refines iterative next-token guesses | LMs to 20B | each block | next token | affine probe | causal feature overlap claimed | more reliable than logit lens | readout ≠ use | https://arxiv.org/abs/2303.08112 v6 2025-11-11 | official code |
| CLT graphs recover prompt circuits | 18L model; Claude 3.5 Haiku | all MLP-replaced layers | short prompts | attribution graphs | feature perturbation | qualitative match; ~50% output match of replacement model | incomplete, attn frozen | https://transformer-circuits.pub/2025/attribution-graphs/methods.html 2025-03-27 | https://github.com/decoderesearch/circuit-tracer on Gemma-2-2B |
| Shared multilingual features | Claude 3.5 Haiku vs smaller | overlapping features | “opposite of small” | circuit tracing | language swap | shared core, more sharing at scale | closed model | https://www.anthropic.com/research/tracing-thoughts-language-model 2025-03-27 | open demos on Gemma/Llama |
| Plans rhyme ahead | Claude 3.5 Haiku; Opus 4.6 NLA | pre-line residual | poetry | feature swap / NLA | suppress rabbit, inject green | line ending changes | tool artifacts possible | same URL; https://transformer-circuits.pub/2026/nla/index.html 2026-05-07 | not independently re-run here |
| CoT can be unfaithful | GPT-3.5, Claude 1.0 | n/a (tokens) | BBH, BBQ | cue / CoT corruption | answer-order bias; add mistakes | answers follow cue; CoT omits it; up to −36% | metric-sensitive | https://arxiv.org/abs/2305.04388 2023; https://arxiv.org/pdf/2307.13702 2023 | many follow-ups |
| Refusal is ~1 residual direction | 13 open chat models ≤72B | often a mid band (e.g. Qwen ~14–17) | harmful vs harmless | difference-in-means | directional ablation vs add | bypass vs induce refusal | direction selection finicky | https://arxiv.org/abs/2406.11717 2024-06-17 | many abliteration replications |
| Toxic persona latent mediates emergent misalignment | GPT-4o SAE on base activations | activation space after FT | open-ended eval | SAE model-diff + steering | steer ± latent; 100s of benign FT | misalignment up/down; re-alignment easy | closed SAE; incomplete in some domains | https://openai.com/index/emergent-misalignment/ 2025-06-18 https://arxiv.org/abs/2506.19823 | behavioral EM replicated more than the latent |
| SAE probes lose to linear probes | Gemma-class activations | various | probing regimes | SAE vs logistic | OOD, noise, imbalance | no consistent SAE win | task-limited | https://proceedings.mlr.press/v267/kantamneni25a.html 2025 | ICML paper |
| NLA reads silent eval awareness | Opus 4.6, Mythos Preview | residual activations | audits, SWE-bench | AV/AR reconstruction | independent methods | unverbalized eval tags; 16–26% vs <1% prod | confabulation | https://www.anthropic.com/research/natural-language-autoencoders 2026-05-07 | open NLAs on Neuronpedia |
| J-space is a small verbalizable workspace | Sonnet 4.5, Haiku/Opus 4.5 | workspace layer band | multi-step, injection | J-lens + swap | J vs non-J components | report/modulate/reason; ~6–7% variance | analogy to GNW disputed | https://www.anthropic.com/research/global-workspace 2026-07-06 | https://github.com/anthropics/jacobian-lens |
| Introspection of injected concepts | Claude Opus 4/4.1 best | layer-sensitive | concept injection | steering + self-report | no-injection controls | ~20% correct ID; failures are the norm | highly context-dependent | https://www.anthropic.com/research/introspection 2025-10-29 | some open-weight replications |
| Looped depth ≠ unique depth | small looped vs untied | 4×8 vs 32 unique | hop traversal | residual probes/interventions | identical train | looped solves all seeds; untied does not | algorithmic task | https://arxiv.org/abs/2610.24135 2026-10-16 | new, limited independent replication |

Glossary

Residual stream. The running hidden vector that every sublayer reads from and adds to.

QK / OV. Attention’s “where to look” versus “what to write.”

Logit lens. Project an intermediate residual through the final unembedding.

Tuned lens. Per-layer affine translator plus the same unembedding.

Activation patching. Swap hidden states between two forward passes.

Steering / CAA. Add a contrastive direction to activations.

SAE. Sparse autoencoder: overcomplete dictionary for one activation site.

Transcoder / CLT. Sparse stand-in for MLP computation, possibly across layers.

Attribution graph. Prompt-specific circuit over features, tokens, errors, logits.

NLA. Natural language autoencoder: activation → text → activation.

J-lens / J-space. Jacobian readout of verbalizable future-token directions; the subframe they span.

CoT faithfulness. Whether written reasoning actually mediates the answer.

Normalized depth. Layer index divided by total layers.

Recurrent / looped depth. Reusing the same block R times.

Decodability. A probe can read a variable. Not the same as the model using it.

Русский мини-словарь. Residual stream — общий черновик сети. Patching — подмена внутренностей. Steering — толчок внутренностей. SAE — разборка активаций на редкие признаки. CoT — письменные рассуждения, часто не те, что внутри.

Coverage notes (inside this report, not a follow-up). No files were written. No experiment was executed. Prior local NotebookLM/ChatGPT packets were described in the brief and treated as claims to recheck, not as sources. Two circuit-tracer GitHub paths exist in public pages (safety-research and decoderesearch); the decoderesearch README was the live repo opened. Gemma Scope blog URL currently lives at https://deepmind.google/blog/gemma-scope-helping-the-safety-community-shed-light-on-the-inner-workings-of-language-models/ . 2026 material verified as existing: NLA 2026-05-07, J-space 2026-07-06, J-lens theory 2026-10-22, looped-transformer mechanistic papers 2026-10, transcoder-vs-neuron 2026-10-17.

🧒 Простыми словами. Модель думает слоями, как конвейер: сначала узнаёт слова, потом достаёт смысл и факты, в конце собирает ответ и вычёркивает лишнее. Подсмотреть конвейер можно, но табличка «Париж» на среднем слое ещё не доказательство. Доказательство — если вынуть эту деталь, ответ ломается, а случайная деталь того же размера не ломает. Текст «давайте подумаем» часто рисует красивую историю после того, как решение уже принято.

🏛H · Grok 4.6 · 01a178f9
