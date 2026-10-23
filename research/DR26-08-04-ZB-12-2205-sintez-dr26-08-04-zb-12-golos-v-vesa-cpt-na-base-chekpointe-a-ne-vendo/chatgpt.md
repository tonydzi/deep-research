# DR26-08-04-ZB-12-2205 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

## Research log

Research cutoff: **August 4, 2026**. “Late 2026” products not yet released are excluded.

### Searches run

1. `continued pretraining style adaptation language model persona writing style`
2. `authorship style language model fine tuning DPO style transfer`
3. `LoRA rank style adaptation LLM all linear DoRA`
4. `personal language model fine tune own messages writing style`
5. `continued pretraining catastrophic forgetting domain adaptive pretraining instruction tuning mix generic data`
6. `DAPT tokens learning rate epochs LLM continued pretraining`
7. `style preference optimization DPO author style`
8. `persona vectors activation steering language models`
9. Official model-card searches for Qwen, Llama, Mistral, Gemma, DeepSeek, GLM, Phi, OLMo
10. Russian-model searches for T-lite/T-pro, Vikhr, RuAdapt, YandexGPT and GigaChat
11. Official fine-tuning searches for OpenAI, Vertex/Gemini, Anthropic and xAI
12. Official fine-tuning searches for Mistral, Together, Fireworks, Predibase and Modal
13. Authorship attribution, Burrows’s Delta, MAUVE, memorisation and human-evaluation searches

### Primary or first-party pages actually opened

Twenty-seven substantive pages were opened, including: TinyStyler; CoNLL 2026 authorial-style GRPO; SCAR; Persona Vectors; Mix-CPT; catastrophic-forgetting rehearsal; MAUVE; fine-tuning memorisation; TSTBench; Modal DoppelBot; model cards for Mistral 3, Gemma 3, Llama 4, GLM-4.5, DeepSeek V3, OLMo 3, Vikhr and T-pro; and official fine-tuning documentation from OpenAI, Google, Together, Fireworks, Mistral and Modal. Failed or bot-blocked pages were not used as factual support.

# Executive summary

[emerging] The best-supported practical answer is **continued causal-LM pretraining on a base checkpoint, followed by a small author-conditioned SFT and optional style-preference stage**. Raw DAPT is the only simple method that can expose the model to essentially the entire 20–40-million-word corpus and directly alter its unconditional next-token distribution. However, no controlled study yet proves that this recipe makes a modern bilingual LLM indistinguishable from one living author; that conclusion remains an engineering extrapolation.

Start with **Qwen3/3.5 Base around 8–14B**, or **Ministral 3 8B Base** as the second candidate. Use QLoRA/DoRA, all-linear modules, rank 64, one conservative pass over the clean, era-weighted corpus. Then restore instruction following with 5–20k short instruction→real-author examples plus generic bilingual instructions. Do not begin with DPO.

A 16GB 5060 Ti can run the cheapest 4B–8B QLoRA experiment, but full-corpus 8–14B work is simpler and faster on a rented 80GB H100. Vendor SFT services are mostly the wrong fit: they accept instruction pairs, generally do not expose raw continued pretraining or exportable weights, and create privacy and recurring-serving costs.

The decisive experiment is a held-out, cross-topic blind test—not training loss or an LLM style score.

# Q1. What actually moves voice into the weights?

## 1.1 Continued pretraining on raw author text

[emerging] **This is the leading method for the stated objective**, because the objective is explicitly a change in the default conditional token distribution, and causal-language-model training optimises exactly next-token likelihood. It can consume nearly all clean prose without inventing prompts.

The direct evidence is adjacent rather than exact:

- Continued pretraining reliably shifts models toward a target data distribution, but can degrade general capabilities. Mix-CPT found that adding instruction/alignment data during continued pretraining reduced general-capability loss relative to target-domain text alone ([Mix-CPT, ICLR 2025, published April 2025](https://proceedings.iclr.cc/paper_files/paper/2025/file/cc721384c26c0bdff3ec31a7de31d8d5-Paper-Conference.pdf)).
- Fine-tuning-induced behavioural/persona shifts correlate with measurable activation shifts, showing that relatively small post-training datasets can change broad model behaviour, not merely individual phrases ([Persona Vectors, July 29, 2025](https://arxiv.org/abs/2507.21509)).
- Catastrophic forgetting from successive domain fine-tuning is established; rehearsal data mitigates it ([Bai et al., NAACL Findings, April 2025](https://aclanthology.org/2025.findings-naacl.138/)).

What is **not established**: that one epoch of DAPT on 25–50M Russian-heavy tokens will reproduce a specific living author’s humour, argument structure and long-range cadence by default. `[no source found]`

Recommendation:

1. Train a **base/pretrained** checkpoint on author text with causal loss.
2. Preserve 5–15% general/bilingual pretraining or instruction material as rehearsal.
3. Stop on held-out author perplexity and capability evals—not after an arbitrary epoch count.
4. Perform a small author-aware instruction re-alignment afterward.

## 1.2 Synthesised instruction → real-text SFT

[established] SFT is effective when the desired behaviour can be represented as prompt-response pairs. It is less natural for using a raw lifetime corpus because every target text needs a plausible conditioning prompt.

Construction:

- Keep the real author text unchanged as the assistant target.
- Reverse-generate a topic, intent, audience, genre and length instruction from it.
- Strip stylistic descriptions from the instruction: use “Write a 120-word post arguing X,” not “write sarcastically like Anton.”
- For replies, retain preceding messages as user context but put loss only on the author’s turn.
- Generate two or three alternative instructions for important posts, but do not duplicate the target excessively.
- Audit reverse prompts for leakage of distinctive wording from the answer.

How much is usable:

- Standalone posts, essays and outbound correspondence: often directly convertible.
- Messenger replies: usable only with authentic preceding context and strict masking of the other speaker.
- Voice-note transcripts: usable after removing transcription artifacts unless those artifacts are intentionally part of the target voice.
- Fragments, quotes and reposts: generally unsuitable.

A realistic high-quality SFT set is **5k–30k examples**, probably only a minority of the raw bytes. That numerical range is practitioner judgement, not a literature-established optimum. `[no source found]`

[emerging] Data consistency may matter more than volume: SCAR reported that a style-consistent 0.7% subset could match or beat full-data instruction tuning in some tasks, though that paper concerns response consistency and task performance, not personal idiolect ([SCAR, ACL, July 2025](https://aclanthology.org/2025.acl-long.625/)). Do not interpret 0.7% as a personal-style recipe.

## 1.3 DPO/ORPO/SimPO/KTO using real versus generic answers

[emerging] Preference optimisation can sharpen a learned stylistic decision boundary:

- Prompt: content/genre request.
- Chosen: authentic author text.
- Rejected: base model’s response to the same reconstructed prompt.
- Harder rejected examples: the tuned model’s superficially styled but “LLM-smooth” answers.

This directly teaches “prefer the author distribution over the assistant distribution.” Together currently exposes DPO plus RPO/SimPO modifiers and supports downloadable weights ([Together fine-tuning docs, undated; accessed August 4, 2026](https://docs.together.ai/reference/cli/finetune)).

However:

- DPO does not naturally use the entire raw corpus.
- Reconstructed prompts may not have actually caused the historical texts.
- Chosen and rejected answer lengths must be controlled; DPO variants exhibit length-related biases.
- DPO is best used after DAPT/SFT, not as the only voice-learning stage.

The strongest recent direct style result used an authorship-verification reward and GRPO on an 8B generator, reaching a reported mean style score of 0.893 across four classic authors. It was style-conditioned, not default-persona training, and used public literary authors rather than private bilingual social writing ([Liu et al., CoNLL, July 2026](https://aclanthology.org/2026.conll-main.31/)). [single-source]

[speculative] ORPO, SimPO or KTO may be operationally easier, but there is no compelling author-idiolect evidence establishing one as superior. `[no source found]`

## 1.4 Full fine-tuning versus LoRA, QLoRA and DoRA

[established] LoRA can match full fine-tuning on many adaptation tasks while training far fewer parameters; the original paper found major trainable-parameter and memory reductions ([Hu et al., June 17, 2021—older evidence](https://arxiv.org/abs/2106.09685)). That does **not** establish the optimal rank for idiolect.

Decision:

- **First experiment:** QLoRA or DoRA.
- **Full fine-tune:** reserve for the final quality run after proving that adapters plateau.
- **Targets:** all-linear projections, not attention-only.
- **Rank:** start at 64; compare 16 and 128 as ablations.
- **Alpha:** 64–128, depending on implementation’s scaling convention.
- **Dropout:** 0–0.05.

Evidence tier for those exact choices: [speculative]. No controlled study establishes rank 8, 64 or 128 for personal idiolect. `[no source found]`

Practical expectation:

- Rank 8–16: often enough for format, catchphrases and broad tone.
- Rank 32–64 all-linear: sensible default for lexical and syntactic distribution changes.
- Rank 128 or full tuning: worth testing if rank 64 improves surface markers but fails argument structure.

These rank claims are practitioner heuristics, not established facts. `[no source found]`

## 1.5 Stage order and erasure

[emerging] Recommended order:

```text
base checkpoint
  → conservative author DAPT/CPT
  → small mixed author-SFT + generic-instruction SFT
  → optional short DPO/SimPO or style-reward stage
```

Later stages can overwrite earlier behaviour. Mix-CPT explicitly found the simple CPT→SFT→DPO pipeline struggled to preserve both domain and general capability, while mixing knowledge/instruction data during CPT improved the balance ([Mix-CPT, April 2025](https://proceedings.iclr.cc/paper_files/paper/2025/file/cc721384c26c0bdff3ec31a7de31d8d5-Paper-Conference.pdf)).

Safeguards:

- Use lower LR and fewer tokens in every later stage.
- Include author examples in the re-alignment stage.
- Re-run author perplexity and style tests after each stage.
- Retain stage checkpoints; do not assume the last checkpoint is best.
- Consider adapter separation: DAPT adapter, instruction adapter and optional preference adapter, tested both separately and merged.

## 1.6 Alternatives

### Retrieval-heavy few-shot prompting

[established] Retrieval is the cheapest strong baseline. Retrieve 3–8 examples matched by genre, topic, length and era, then ask a frontier model to draft without an explicit style lecture.

TinyStyler shows that authorship embeddings plus a small 800M model can outperform GPT-4 on its few-shot style-transfer benchmark, demonstrating that specialised rewriting/retrieval approaches can beat general prompting on constrained style tasks ([TinyStyler, EMNLP Findings, November 2024—older than the requested window](https://aclanthology.org/2024.findings-emnlp.781/)).

It beats fine-tuning when:

- most requests resemble existing topics or genres;
- privacy permits local retrieval;
- exact default voice without exemplars is not mandatory;
- iteration speed matters more than long-horizon consistency.

### Activation/style vectors

[emerging] Persona vectors can steer traits and monitor fine-tuning-induced drift, but published demonstrations focus on traits such as sycophancy, humour and hallucination—not a full bilingual idiolect ([Chen et al., July 2025](https://arxiv.org/abs/2507.21509)). They are promising as a diagnostic or inexpensive ablation, not the primary recommendation.

### Voice rewriter

[emerging] A 1–4B rewriter trained on `generic draft → authentic author text` pairs may provide better quality per unit effort when a strong frontier model supplies reasoning and factual structure. TinyStyler is relevant evidence for this decomposition, but it does not establish success on Russian/English long-form prose.

Its weakness is structural: if the first model chooses generic argument architecture, a surface rewriter may preserve that generic skeleton—the exact failure identified by the requester.

# Q2. Base models

## 2.1 The “assistant attractor”

[emerging] Strongly post-trained instruct models are harder to move away from their assistant defaults than base checkpoints. Direct controlled evidence specifically comparing author-style absorption in base versus RLHF checkpoints is scarce. `[no source found]`

The broader evidence is consistent with the phenomenon:

- Base and instruct models have intentionally different purposes; Meta says Llama pretrained models are for adaptation, while instruct models target assistant-like chat ([Llama 4 model card, April 5, 2025](https://github.com/meta-llama/llama-models/blob/main/models/llama4/MODEL_CARD.md)).
- OLMo publishes distinct base, SFT, DPO and final RLVR checkpoints, making this hierarchy experimentally testable ([OLMo 3 model card, late 2025](https://huggingface.co/allenai/Olmo-3-7B-Instruct)).
- Persona Vectors shows fine-tuning datasets can cause broad persona shifts, but not that base checkpoints always shift more readily.

Practical implication: **start from base if one exists, then re-align lightly**. Run an instruct-checkpoint LoRA as a cheap control; do not accept the attractor story on faith.

## 2.2 Model comparison

VRAM figures below are engineering estimates for text-only operation, not vendor guarantees. Training assumes 4-bit QLoRA, gradient checkpointing, batch 1 and moderate sequence length. Serving assumes roughly Q4 weights plus overhead.

| Family/checkpoint | Voice tractability | Russian | QLoRA train | Q4 serve | Licence/remarks |
|---|---:|---:|---:|---:|---|
| **Qwen3/3.5 Base 8–14B** | High | High | ~12–24GB | ~7–13GB | Usually Apache 2.0; current Qwen3.5 documentation explicitly includes Russian among supported languages ([Qwen docs, undated; accessed Aug. 4, 2026](https://docs.qwencloud.com/resources/faq-text-generation)). Exact small base checkpoint availability must be checked before purchase. |
| **Ministral 3 8B Base** | High | Medium-high | ~12–16GB | ~6–8GB | Base/instruct/reasoning variants, Apache 2.0, multilingual ([Mistral, December 2025](https://mistral.ai/news/mistral-3/)). Strong second candidate. |
| **Ministral 3 14B Base** | High | Medium-high | ~20–28GB | ~10–13GB | Apache 2.0; likely needs cloud or two GPUs. |
| **Gemma 3 4B PT** | High | Medium | ~8–12GB | ~3–5GB | Gemma terms; 140+ languages claimed ([Google DeepMind, March 2025](https://deepmind.google/models/gemma/gemma-3/)). Best cheap ablation, but weaker long-form reasoning. |
| **Gemma 3 12B PT** | High | Medium | ~18–25GB | ~9–11GB | Better capacity; custom terms rather than Apache/MIT. |
| **Llama 3.1 8B Base** | High | Medium-low | ~12–16GB | ~6–8GB | Mature tooling; custom Llama licence. Russian is not a first-class supported language in later Llama cards. |
| **Llama 4 Scout Base** | Medium | Low/uncertain | Multi-GPU | >55GB total Q4 | 109B total MoE; Russian absent from the 12 supported languages, though broader pretraining covered more languages ([Meta, April 5, 2025](https://github.com/meta-llama/llama-models/blob/main/models/llama4/MODEL_CARD.md)). Poor AK-47 fit. |
| **OLMo 3 7B Base** | High/research-friendly | Low-medium | ~12–16GB | ~6–8GB | Apache 2.0; unusually open training artifacts ([AI2, late 2025](https://huggingface.co/allenai/Olmo-3-7B-Instruct)). Excellent experimental control, not first Russian choice. |
| **Phi-4 14B** | Medium | Low | ~20–28GB | ~10–12GB | MIT, but model card says non-English performance is worse and multilingual use is not intended ([Microsoft Phi-4 report, Dec. 2024—older](https://arxiv.org/abs/2412.08905)). |
| **DeepSeek V3/V3.2 Base** | High theoretically | High | Impractical locally | Hundreds of GB | 671B total/37B active for V3; excellent capacity but operationally wrong for one engineer ([DeepSeek model card, Dec. 2024](https://huggingface.co/deepseek-ai/DeepSeek-V3)). |
| **GLM-4.5-Air Base** | High theoretically | Medium-high | 4×H100 LoRA | 2×H100 FP8 | 106B total/12B active, MIT; official recipe specifies four H100s for LoRA ([Z.ai, Aug. 2025](https://huggingface.co/zai-org/GLM-4.5)). Not simple. |
| **T-lite 7B** | Medium-high | High | ~12–16GB | ~6–8GB | Qwen2.5-derived, Russian-adapted, Apache 2.0 according to T-Bank ([T-Bank, Dec. 2024—older](https://habr.com/en/companies/tbank/articles/865582/)). Older base; worthwhile Russian control. |
| **T-pro 2.0** | Medium | Very high | checkpoint-dependent | checkpoint-dependent | Open-weight Russian hybrid-reasoning system with Cyrillic-dense tokenizer ([T-pro 2.0, Dec. 2025](https://arxiv.org/abs/2512.10430)). Tooling/support less mature than Qwen/Mistral. |
| **Vikhr Llama 3.1 8B Instruct** | Medium-low | High | ~12–16GB | ~6–8GB | Russian/English SFT+SMPO model ([Vikhr, Sept. 2024—older](https://huggingface.co/Vikhrmodels/Vikhr-Llama3.1-8B-Instruct-R-21-09-24)). Already strongly post-trained; better benchmark/control than starting point. |
| **RuAdapt Qwen3** | Medium-high | High | size-dependent | size-dependent | Russian-tokenizer adaptation can improve Cyrillic token efficiency, but evidence is largely creator model cards. [low-authority] ([RuAdapt Qwen3 card, 2025](https://huggingface.co/RefalMachine/RuadaptQwen3-4B-Instruct-GGUF)). |
| **GigaChat open weights** | Unclear | Very high | Generally impractical | Generally impractical | Large MoE releases exist, but small clean base-checkpoint/tooling story is weaker. [single-source] |
| **YandexGPT Lite tuning** | Medium | Very high | Vendor hosted | API only | Yandex AI Studio documents LoRA tuning of YandexGPT Lite; weights are not exportable ([Yandex AI Studio docs, undated; accessed Aug. 4, 2026](https://aistudio.yandex.ru/docs/en/ai-studio/operations/tuning/create.html)). |

### Ranking

1. **Qwen3/3.5 Base 8–14B**
2. **Ministral 3 8B/14B Base**
3. **T-lite 7B or a current RuAdapt-Qwen base-compatible checkpoint**
4. **Gemma 3 4B/12B pretrained**
5. **Llama 3.1 8B Base**
6. OLMo 3 7B Base
7. Phi, DeepSeek and GLM for this particular hardware/workflow

## 2.3 Size-quality curve

[emerging] A well-tuned 7–8B may beat a generic 70B **on identity and voice preference**, but not necessarily on truthfulness, novel-domain reasoning, planning or sustained essay architecture.

The “small model in your voice” argument breaks when:

- the subject requires knowledge not in the author corpus;
- the model must reason reliably before writing;
- the requested piece is substantially longer or structurally more complex than training examples;
- tool calling, citation accuracy or multilingual nuance matters;
- DAPT causes topic/style entanglement.

The best practical architecture may therefore be:

```text
strong planner/researcher → author-voice generator or rewriter
```

But this should be tested against direct 8–14B generation because a rewriter may preserve generic argument structure.

# Q3. Vendor services

Vendor documentation changes frequently. “Undated” means the page exposes no reliable publication date; it was checked August 4, 2026.

| Provider | Individual access | Maximum/data format | Raw CPT? | Export weights? | Cost | Personal-data position |
|---|---|---|---|---|---|---|
| **OpenAI** | API customers, model-dependent | JSONL; individual file max 512MB ([Files API, undated](https://platform.openai.com/docs/api-reference/files)) | No public raw-CPT product | No | SFT/DPO token pricing is model-specific; RFT on `o4-mini-2025-04-16` is $100 training-hour plus model-grader tokens ([OpenAI, updated July 2026](https://help.openai.com/en/articles/11323177-billing-guide-for-the-reinforcement-fine-tuning-api)) | API inputs not used for model training by default; fine-tuning application state retained until deleted and abuse logs normally 30 days ([OpenAI data controls, undated](https://developers.openai.com/api/docs/guides/your-data#default-usage-policies-by-endpoint)). |
| **Google Vertex Gemini** | Yes with Cloud billing/project | JSONL; model-specific example/token/file limits | No public Gemini raw CPT | No | Training tokens × epochs; tuned endpoint uses base prediction price ([Google pricing, undated](https://cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing)) | Governed by Google Cloud terms/region controls; verify project’s data-residency terms. |
| **Anthropic Claude** | No general self-service Claude fine-tuning found | N/A | No | No | N/A | Bedrock/Vertex access does not imply customer fine-tuning. [established as of cutoff; no affirmative product source found] |
| **xAI/Grok** | API available | No public general fine-tuning product found | No | No | N/A | `[no source found]` for customer fine-tuning. |
| **Mistral API** | Yes | JSONL, 512MB per file ([Mistral Files, undated](https://docs.mistral.ai/api/endpoint/files)) | Public API is fine-tuning, not documented raw CPT | No downloadable proprietary base weights; open-model portability varies | Model/job-specific; current pricing must be read from account/pricing page | Check workspace retention and deletion terms before upload. |
| **Together AI** | Yes | Up to 25GB training and validation files; packing enabled by default | SFT/DPO; raw-text CPT not clearly offered as a separate managed mode | **Yes**, merged or adapter checkpoints ([Together docs, undated](https://docs.together.ai/reference/cli/finetune)) | CLI returns estimate before confirmation | Most attractive managed option if privacy terms are accepted. |
| **Fireworks** | Yes | JSONL, up to 3M examples; CLI recommended above 500MB | LoRA SFT publicly documented; full training API can create/export full checkpoints | **Yes** ([Fireworks docs, undated](https://docs.fireworks.ai/getting-started/introduction)) | Training tokens plus deployment/GPU cost; exact rate is model-specific | Verify retention/security contract; documentation supports deleting/exporting custom models. |
| **Predibase** | Availability/current product status unclear at cutoff | `[no source found]` | Historically LoRA-focused | Historically adapter portability, current status unverified | `[no source found]` | Do not select without a sales-confirmed current data-processing agreement. |
| **Modal** | Yes | Your own files/code | Yes—you run it yourself | **Yes** | Per-second GPU compute; current H100 examples are roughly $4/hour in recent docs, older 2024 post quoted $7.65/hour ([Modal inference example, early 2026](https://modal.com/docs/examples/trtllm_throughput); [older pricing post, Feb. 6, 2024](https://modal.com/blog/introducing-h100)) | You control training implementation and stored artifacts; still a cloud processor. |
| **Yandex AI Studio** | Region/account-dependent | Instruction tuning for supported YandexGPT models | No documented raw CPT | No | Region-specific | Russian strength is attractive, but lock-in and corpus upload make it a secondary experiment. |
| **GigaChat** | API access exists | No general self-service personal-corpus fine-tuning product verified | No | No | Region-specific | `[no source found]` for exportable customer tuning. |

### Corpus-size implications

The **raw 100–200MB byte size is not itself the main obstacle**: OpenAI and Mistral accept files up to 512MB, and Together accepts much larger files. The obstacle is **format and objective**. Hosted SFT services require JSONL prompt/answer structures, so most raw causal text cannot be used as-is.

Cutting 25–50M tokens to 2–5M high-quality SFT target tokens might preserve visible tone but plausibly lose rare vocabulary, cross-genre rhythm and long-tail syntactic choices. [speculative] No study quantifies the voice-fidelity penalty for this author. `[no source found]`

# Q4. Data engineering

## 4.1 Splits before cleaning

Create immutable document IDs and split by **whole document/thread and time**, before near-duplicate filtering:

- 70% training
- 10% development
- 20% final test
- reserve a separate “novel topic” evaluation set
- prevent the same thread, repost or revised essay from crossing splits

## 4.2 Deduplication

Use:

1. normalized exact hashes;
2. MinHash/LSH on word 5-grams;
3. embedding similarity only for manual-review candidates;
4. retain one canonical version plus metadata.

No defensible published estimate was found for the typical duplicate proportion of a decade of one person’s Facebook/Telegram corpus. `[no source found]` Measure it rather than assuming 10%, 30% or 50%.

## 4.3 Provenance and dialogue

- Remove quoted blocks, forwarded posts, signatures, legal boilerplate and copied news.
- In chats, other speakers’ text may appear only as conditioning context.
- Set their token loss weight to zero.
- Exclude uncertain speaker attribution.
- Keep the author’s typographic quirks only if they are genuinely intentional, not exporter artifacts.

## 4.4 Era weighting

If the target is “2023 Facebook voice”:

- assign explicit source, date, genre and quality metadata;
- make 2018–2023 Facebook the dominant sampling pool;
- downweight 2013–2017 rather than deleting it;
- run an ablation with and without older material.

Starting heuristic:

- 60% target-era native Facebook
- 15% long-form essays
- 10% author chat replies
- 5% cleaned voice-note transcripts
- 5% English author text
- 5% generic rehearsal

This is a proposed experiment, not a literature-established ratio. `[no source found]`

Upsample by resampling **documents**, not copying them into multiple splits.

## 4.5 Generic data and collapse

[established] Generic rehearsal reduces forgetting in continual adaptation, but the ideal ratio is task-specific. Mix-CPT supports mixing instruction/alignment data with raw domain text, while catastrophic-forgetting research supports rehearsal generally ([Mix-CPT, April 2025](https://proceedings.iclr.cc/paper_files/paper/2025/file/cc721384c26c0bdff3ec31a7de31d8d5-Paper-Conference.pdf); [Bai et al., April 2025](https://aclanthology.org/2025.findings-naacl.138/)).

Start at **90:10 author:generic tokens**, then compare 95:5 and 80:20. Those exact ratios are [speculative].

## 4.6 Packing and sequence length

- Pack independent short posts into 2,048–4,096-token sequences with document separators.
- Keep long essays/thread structures intact where possible.
- Do not concatenate unrelated posts without boundaries.
- Use 4k first; 8k only if long-form voice materially improves.
- Evaluate at 120, 500, 1,500 and 3,000 generated tokens.

Together enables packing by default, illustrating that this is mature managed-training functionality ([Together docs, accessed Aug. 4, 2026](https://docs.together.ai/reference/cli/finetune)).

## 4.7 Hyperparameter starting points

These are proposed search ranges, not established personal-style optima:

| Stage | LR | Passes | Adapter | Notes |
|---|---:|---:|---|---|
| QLoRA DAPT | 5e-6–2e-5 | 0.5–1.5 | r64, all-linear | cosine decay, 3–5% warmup |
| SFT | 1e-5–5e-5 | 1–3 | same/second adapter | mask instruction tokens |
| DPO/SimPO | 2e-6–1e-5 | 0.25–1 | r32–64 | 2k–10k carefully matched pairs |
| Full DAPT | 1e-6–5e-6 | ≤1 initially | all weights | H100; higher risk |

Modal’s real Slack-persona example found overfitting after only a few hundred steps with batch 128 on a small dataset; it also found that filtering out terse messages fixed a short-answer bias. This is useful practitioner evidence, not a controlled personal-voice study ([Modal DoppelBot, undated; accessed Aug. 4, 2026](https://modal.com/docs/examples/llm-finetuning)). [low-authority]

## 4.8 Privacy

Before training:

- detect phones, emails, account IDs, addresses, API keys and passwords;
- distinguish the author’s own names from third-party private identities;
- pseudonymise consistent entities if relational context matters;
- create canary strings that must never be emitted;
- maintain a local mapping outside the training set;
- run nearest-neighbour and long-substring regurgitation tests.

Fine-tuned models can memorise sensitive data; memorisation grows with model/data/training conditions and is a distinct fine-tuning risk ([“Exploring Memorization in Fine-tuned Language Models,” ACL, August 2024—older](https://aclanthology.org/2024.acl-long.216/); [Sakarvadia et al., October 2024—older](https://arxiv.org/abs/2410.02159)).

# Q5. Evaluation harness

## 5.1 Cheap automated suite

Build one frozen evaluation set of at least:

- 100 Russian prompts;
- 25 English prompts;
- equal representation of native topics, adjacent topics and novel topics;
- lengths of 120, 500 and 1,500 words/tokens as appropriate;
- identical decoding grids for base, retrieval baseline and every checkpoint.

### A. Authorship classifier

Train a Russian-capable classifier on held-out authentic text from:

- target author;
- 10–30 demographically/genre-matched authors;
- base-model generations;
- retrieval-prompted generations.

Use grouped cross-validation by document and year. Report:

- author-vs-other AUROC;
- calibration;
- genre-conditioned scores;
- performance with content words removed or masked.

If it only works with nouns/topic terms, it is measuring subject matter, not voice.

### B. Stylometry

Measure:

- function-word frequencies;
- character and POS n-grams;
- sentence and paragraph lengths;
- punctuation and dash/parenthesis/ellipsis patterns;
- lexical diversity;
- dependency-depth and clause-length distributions;
- Burrows’s Delta and Cosine Delta.

Recent Russian research found trigram–Cosine Delta useful as an interpretable human/LLM discriminator, though detection is not identical to author attribution ([Salnikov & Bonch-Osmolovskaya, 2025](https://jle.hse.ru/article/view/22211)). [single-source]

### C. Held-out perplexity

Compute author-test NLL/perplexity for base and every tuned checkpoint. Also compute general Russian, English and instruction-eval perplexity.

Interpretation:

- author perplexity improves, human preference improves: good;
- author perplexity collapses while novelty/regurgitation worsens: overfit;
- author perplexity improves but classifier relies on topic nouns: topic adaptation;
- SFT restores instruction score but worsens author perplexity: re-alignment erased voice.

### D. Distributional measures

Use MAUVE between generations and held-out author documents, within matching genre and length. MAUVE compares human and model text distributions using divergence frontiers and correlates with human judgments in its original evaluations ([Pillutla et al., NeurIPS, Nov. 2021—older evidence](https://arxiv.org/abs/2102.01454); [JMLR follow-up, 2023—older](https://www.jmlr.org/papers/v24/23-0023.html)).

Do not use MAUVE on a mixed pile of Facebook posts, emails and essays; it will confound genre.

### E. LLM judge

Use two unrelated strong judges, blind to model identity, scoring:

1. lexical choice;
2. sentence rhythm;
3. humour setup and timing;
4. argument architecture;
5. consistency over the full piece;
6. LLM-smoothness;
7. topical correctness;
8. copying suspicion.

LLM judges are secondary evidence because they may favour their own families’ polished prose.

## 5.2 Detect surface mimicry

Create “style-adversarial” prompts:

- serious topic where catchphrases are inappropriate;
- novel subject absent from training;
- request explicitly forbidding common surface markers;
- long argumentative essay;
- follow-up that reverses the position;
- English continuation after Russian dialogue.

If authorship scores disappear when punctuation/catchphrases are normalised, the model learned surface style.

## 5.3 Detect capability collapse

Before and after every stage, run:

- Russian and English instruction-following;
- summarisation;
- factual QA;
- reasoning;
- tool-schema adherence;
- refusal/safety sanity checks;
- long-context coherence.

A checkpoint fails operationally if voice improves but material capabilities decline beyond the principal’s predeclared tolerance—suggested threshold: more than 5% relative loss on core instruction/correctness composites. This threshold is a project decision, not a published standard.

## 5.4 Human blind A/B

Recommended protocol:

- 100 authentic and 100 generated samples;
- generated samples matched to authentic ones by era, genre, topic and length;
- exclude passages searchable verbatim or recognisable from famous events;
- 8–15 judges who know the author;
- each judge sees a balanced random subset;
- one sample per screen, forced human/generated choice plus confidence;
- mixed-effects logistic regression with judge and prompt/topic random effects;
- preregister the pass criterion.

Pass:

1. pooled accuracy’s 95% confidence interval includes 50%;
2. its upper bound is below 60%;
3. no major genre or length stratum exceeds 65%;
4. judges do not consistently identify private-text regurgitation.

With 10 judges × 100 judgments each, the study has 1,000 judgments, but non-independence means confidence must be estimated by judge and item, not a simple binomial calculation.

# Q6. Failure modes and the negative case

## Common failures

1. **Verbatim regurgitation.** Repetition, rare sequences and excessive epochs increase privacy risk.
2. **Topic-style entanglement.** The model learns “author discusses startups and politics,” not how the author reasons.
3. **Assistant reversion.** Short outputs sound right; long outputs drift toward base-model exposition.
4. **Instruction collapse.** Raw DAPT makes the model an effective autocomplete engine but a worse assistant.
5. **Reasoning degradation.** Small models may adopt confident rhetoric without correct reasoning.
6. **Genre averaging.** Business correspondence smooths out Facebook voice.
7. **Temporal averaging.** 2016 and 2023 voices blend into a voice the author never had.
8. **Synthetic prompt contamination.** Reverse-generated prompts teach another model’s categorisation and prose.
9. **Preference shortcutting.** DPO chooses authentic texts because of length or punctuation rather than deep style.
10. **English regression.** Russian-heavy adaptation damages English and tool syntax.

## Steelman: fine-tuning may be the wrong tool

[established] The strongest contrary case is:

- Voice depends heavily on topic, audience, social context and retrieved autobiographical particulars.
- Twenty million words are large enough to create cleaning, privacy and memorisation problems, yet tiny relative to foundation pretraining.
- A modern frontier model with 5–8 retrieved exemplars can supply much stronger reasoning and factual reliability.
- Retrieval is inspectable: the principal can see which texts conditioned the answer.
- Fine-tuning introduces opaque failures, GPU/software maintenance, benchmark work and version lock-in.
- A rewriter or retrieved-exemplar prompt can deliver perhaps 80% of the perceived result with a fraction of the engineering.

The “80% for 5% effort” numerical claim itself is unverified. `[no source found]`

TinyStyler materially strengthens this case by showing that a specialised few-shot authorship-transfer model can outperform GPT-4 on its benchmark ([Horvitz et al., November 2024](https://aclanthology.org/2024.findings-emnlp.781/)). Modal’s DoppelBot also shows that straightforward message-pair LoRA can pick up persona-like behaviour cheaply, while simultaneously illustrating dataset biases and quick overfitting ([Modal, accessed Aug. 4, 2026](https://modal.com/docs/examples/llm-finetuning)).

Why fine-tuning is still justified here: **the pass condition explicitly demands default voice without style prompts and persistence across contexts**. Retrieval should therefore be the mandatory baseline, not the final presumed solution.

# Q7. Concrete end-to-end recipes

Dollar and wall-clock figures are engineering estimates. Actual time varies strongly with tokenisation, sequence length, packing, FlashAttention/Blackwell support and whether the second consumer GPU can participate efficiently.

## Option A — Cheapest thing that can work this week

[emerging]

**Model:** Gemma 3 4B pretrained or the smallest current Qwen3 Base that fits cleanly  
**Hardware:** RTX 5060 Ti 16GB  
**Tooling:** Unsloth or LLaMA-Factory; llama.cpp/Ollama for serving  
**Method:**

1. Clean and split only the best 2020–2023 Facebook subset.
2. Create 3k–8k reverse-generated instruction→real-post pairs.
3. QLoRA SFT, r32/r64, all-linear, 4-bit, 2k context.
4. Mix 15% generic Russian/English instructions.
5. Train 1–2 epochs with checkpoint evaluations.
6. Compare against a retrieval-heavy Qwen/Claude/Gemini baseline.
7. Export GGUF and serve in Ollama/llama.cpp.

**Time:** 1–3 days data preparation; roughly 3–10 GPU-hours training.  
**Direct cost:** near zero excluding electricity.  
**Risk:** likely catches surface and short-post voice but may fail long-form reasoning and deep rhythm.

Modal documents a single-GPU Unsloth Qwen3-14B example and reports large memory/speed savings, but those are vendor claims and not specific to Blackwell consumer GPUs ([Modal, accessed Aug. 4, 2026](https://modal.com/docs/examples/unsloth_finetune)). [low-authority]

## Option B — Recommended AK-47 pipeline

[emerging]

**Model:** Qwen3/3.5 Base 8–14B; parallel control with Ministral 3 8B Base  
**Hardware:** rented H100 80GB for training; 5060 Ti for quantised serving  
**Tooling:** Axolotl or LLaMA-Factory; TRL for DPO; vLLM during evaluation; llama.cpp/Ollama for local serving  
**Stages:**

1. Deduplicate, provenance-filter, PII-scrub and era-weight all text.
2. Tokenise; expect approximately 25–60M subword tokens depending on Cyrillic tokenizer efficiency. This is an estimate.
3. QLoRA/DoRA DAPT, r64 all-linear, author:generic = 90:10, 4k packed sequences, LR around 1e-5.
4. Evaluate at 0.25, 0.5 and 1.0 corpus pass.
5. SFT on 8k–20k author instruction pairs plus bilingual generic instructions.
6. Stop here unless blind/stylometric results show consistent residual assistant smoothness.
7. Optional 2k–5k hard DPO pairs: authentic chosen versus tuned-model generic rejected.
8. Merge/quantise the winning adapter; serve Q4/Q5 locally.

**Time:** 1–2 weeks data/eval engineering; approximately 4–15 H100-hours across experiments.  
**Cloud cost:** roughly $20–$120 at current commodity H100 rates, potentially $150–$400 including failed runs and ablations.  
**Risk:** DAPT may learn topic distribution; SFT may partially erase voice; 14B serving may be tight on 16GB at long context.

## Option C — Quality-first

[emerging]

**Model:** Qwen3/3.5 dense 27–32B Base if available and well-supported; otherwise Ministral 3 14B Base with broader ablations  
**Hardware:** 2–4×H100/H200  
**Tooling:** Axolotl/torchtune or native Transformers+FSDP; TRL; vLLM/SGLang  
**Stages:**

1. Full data pipeline with human-reviewed provenance.
2. Full-parameter or high-rank DoRA DAPT on the full clean corpus with 10–20% rehearsal.
3. Multi-checkpoint author-perplexity/capability selection.
4. Mixed bilingual SFT.
5. Train an authorship-verification reward model.
6. Short GRPO or DPO stage optimising deep-style rubric.
7. Distil or LoRA-transfer into an 8–14B local serving model if necessary.
8. Run preregistered blind study.

**Time:** 3–6 weeks.  
**Compute:** tens to low hundreds of H100-hours.  
**Cost:** approximately $500–$5,000 depending on full tuning, reward training and ablations.  
**Risk:** complexity violates the principal’s repairability constraint; reward hacking may optimise classifier artifacts.

## Option D — Retrieval/rewrite baseline

[established as a baseline, not as guaranteed final quality]

**Model:** strongest already-subscribed model plus local embedding retrieval; optional 1–4B author-voice rewriter  
**Hardware:** existing Macs/GPU  
**Tooling:** FAISS/Qdrant, local embeddings, llama.cpp/Ollama; TinyStyler-inspired authorship embeddings if engineering permits  
**Stages:**

1. Index clean author texts by genre, era, topic and length.
2. Retrieve 3–8 exemplars.
3. Generate a content draft.
4. Either prompt with exemplars or pass through a locally trained rewriter.
5. Evaluate identically to Options A–C.

**Time:** 2–5 days.  
**Cost:** near zero if included subscription quotas suffice.  
**Risk:** violates the “no exemplars/style prompt” ideal and may preserve generic argument structure, but sets the quality floor every fine-tune must beat.

# Decision

## Named choices

- **A: Gemma/Qwen 4B SFT-only local pilot** — cheapest, fastest, highest surface-mimicry risk.
- **B: Qwen Base 8–14B DAPT → mixed SFT → optional DPO** — best balance of voice depth, Russian quality, repairability and local serving.
- **C: 27–32B full/high-rank quality run with learned style reward** — highest potential, disproportionate complexity and cost.
- **D: retrieved-exemplar frontier model plus optional local rewriter** — best quality/effort baseline, but not true default-weight voice.

## One recommendation

[emerging] **Choose Option B, but gate it behind a three-day Option D baseline and a small Option A pipeline rehearsal.**

Main assumption: **the cleaned 2018–2023 Facebook subset contains a stable, topic-independent stylistic signal strong enough that causal-LM adaptation generalises beyond its historical topics.**

Use **Qwen3/3.5 Base 8–14B**, QLoRA/DoRA r64 all-linear, conservative DAPT over the era-weighted corpus, then light bilingual SFT. Do not add DPO unless evaluation shows a persistent assistant attractor after SFT.

# What would change the recommendation?

The recommendation flips to retrieval/rewrite if:

1. authorship classification on real held-out texts fails across topics—meaning the corpus itself does not expose a stable measurable idiolect;
2. retrieved-exemplar prompting already passes the human blind test;
3. DAPT improves held-out author perplexity but not human attribution;
4. style gains vanish when catchphrases and punctuation are normalised;
5. novel-topic accuracy or instruction following falls more than the accepted threshold;
6. regurgitation testing finds unacceptable private-text extraction;
7. an instruct-checkpoint SFT control matches DAPT, making the raw-CPT stage unnecessary.

It flips toward Option C if an 8–14B DAPT model materially improves voice but systematically fails long-form coherence, while a 27–32B untuned/base model demonstrates the missing reasoning capacity.

# Full source list

1. Horvitz et al., “TinyStyler,” November 2024: [ACL Anthology](https://aclanthology.org/2024.findings-emnlp.781/)
2. Liu et al., “Capturing Classic Authorial Style…,” July 2026: [ACL Anthology](https://aclanthology.org/2026.conll-main.31/)
3. Li et al., “SCAR,” July 2025: [ACL Anthology](https://aclanthology.org/2025.acl-long.625/)
4. Chen et al., “Persona Vectors,” July 29, 2025: [arXiv](https://arxiv.org/abs/2507.21509)
5. Bai et al., catastrophic-forgetting rehearsal, April 2025: [ACL Anthology](https://aclanthology.org/2025.findings-naacl.138/)
6. “Mix-CPT,” ICLR, April 2025: [ICLR PDF](https://proceedings.iclr.cc/paper_files/paper/2025/file/cc721384c26c0bdff3ec31a7de31d8d5-Paper-Conference.pdf)
7. Hu et al., “LoRA,” June 17, 2021: [arXiv](https://arxiv.org/abs/2106.09685)
8. Pillutla et al., “MAUVE,” November 2021: [arXiv](https://arxiv.org/abs/2102.01454)
9. Pillutla et al., “MAUVE Scores,” 2023: [JMLR](https://www.jmlr.org/papers/v24/23-0023.html)
10. “Exploring Memorization in Fine-tuned Language Models,” August 2024: [ACL PDF](https://aclanthology.org/2024.acl-long.216.pdf)
11. Sakarvadia et al., memorisation mitigation, October 2024: [arXiv](https://arxiv.org/abs/2410.02159)
12. Salnikov and Bonch-Osmolovskaya, trigram–Cosine Delta, 2025: [Journal of Language and Education](https://jle.hse.ru/article/view/22211)
13. TSTBench, 2025: [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12191983/)
14. Meta, Llama 4 model card, April 5, 2025: [GitHub](https://github.com/meta-llama/llama-models/blob/main/models/llama4/MODEL_CARD.md)
15. Google DeepMind, Gemma 3, March 2025: [Model page](https://deepmind.google/models/gemma/gemma-3/)
16. Mistral, Mistral 3 announcement, December 2025: [Mistral](https://mistral.ai/news/mistral-3/)
17. Z.ai, GLM-4.5 model card, August 2025: [Hugging Face](https://huggingface.co/zai-org/GLM-4.5)
18. DeepSeek V3 model card, December 2024: [Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V3)
19. AI2, OLMo 3 model card, 2025: [Hugging Face](https://huggingface.co/allenai/Olmo-3-7B-Instruct)
20. Microsoft, Phi-4 technical report, December 2024: [arXiv](https://arxiv.org/abs/2412.08905)
21. T-Bank, T-lite/T-pro, December 2024: [Habr](https://habr.com/en/companies/tbank/articles/865582/)
22. T-pro 2.0, December 2025: [arXiv](https://arxiv.org/abs/2512.10430)
23. Vikhr model card, September 2024: [Hugging Face](https://huggingface.co/Vikhrmodels/Vikhr-Llama3.1-8B-Instruct-R-21-09-24)
24. RuAdapt Qwen3 model card, 2025: [Hugging Face](https://huggingface.co/RefalMachine/RuadaptQwen3-4B-Instruct-GGUF)
25. Qwen multilingual FAQ, undated; accessed August 4, 2026: [Qwen](https://docs.qwencloud.com/resources/faq-text-generation)
26. OpenAI fine-tuning reference, undated; accessed August 4, 2026: [OpenAI](https://developers.openai.com/api/reference/resources/fine_tuning)
27. OpenAI RFT billing, updated July 2026: [OpenAI Help](https://help.openai.com/en/articles/11323177-billing-guide-for-the-reinforcement-fine-tuning-api)
28. OpenAI data controls, undated; accessed August 4, 2026: [OpenAI](https://developers.openai.com/api/docs/guides/your-data#default-usage-policies-by-endpoint)
29. OpenAI Files API, undated; accessed August 4, 2026: [OpenAI](https://platform.openai.com/docs/api-reference/files)
30. Google Gemini tuning-data documentation, undated; accessed August 4, 2026: [Google Cloud](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/tuning/supervised-tuning/prepare-data)
31. Google generative-AI pricing, undated; accessed August 4, 2026: [Google Cloud](https://cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing)
32. Together fine-tuning documentation, undated; accessed August 4, 2026: [Together](https://docs.together.ai/reference/cli/finetune)
33. Fireworks SFT documentation, undated; accessed August 4, 2026: [Fireworks](https://docs.fireworks.ai/fine-tuning/fine-tuning-models)
34. Fireworks platform/export documentation, undated; accessed August 4, 2026: [Fireworks](https://docs.fireworks.ai/getting-started/introduction)
35. Mistral Files/fine-tuning limits, undated; accessed August 4, 2026: [Mistral](https://docs.mistral.ai/api/endpoint/files)
36. Modal DoppelBot, undated; accessed August 4, 2026: [Modal](https://modal.com/docs/examples/llm-finetuning)
37. Modal Unsloth example, undated; accessed August 4, 2026: [Modal](https://modal.com/docs/examples/unsloth_finetune)
38. Modal GPU documentation, undated; accessed August 4, 2026: [Modal](https://modal.com/docs/guide/gpu)
39. Modal H100 pricing post, February 6, 2024: [Modal](https://modal.com/blog/introducing-h100)
40. YandexGPT tuning example, undated; accessed August 4, 2026: [Yandex AI Studio](https://aistudio.yandex.ru/docs/en/ai-studio/operations/tuning/create.html)
