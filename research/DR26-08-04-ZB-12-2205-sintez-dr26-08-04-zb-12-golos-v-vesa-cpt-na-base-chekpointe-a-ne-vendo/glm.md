# DR26-08-04-ZB-12-2205 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-04-ZB-12-2205
rail: glm
model: GLM (chat.z.ai, анонимно, без логина)
mode: Advanced Search (multi-round) + deep-think
door: browser-inapp-pane

chat_url: https://приватный чат
started: 2026-08-10T22:47-07:00
collected: 2026-08-10T23:30-07:00
note: прогон, помеченный прошлым тиком как "залипший", на деле завершился за ночь целиком (конец + список источников на месте)
---

Show full message
Thought Process

Executive summary (≤200 words)
Use a small, permissively licensed base model (Qwen3.5 8B or Ministral 8B) in the author’s “native” language (Russian; both are strong there). Run a two-stage pipeline: (1) short continued-pretraining (DAPT) on the cleaned 100–200 MB corpus to shift the default next-token distribution toward the author’s idiolect; then (2) light instruction-realignment (SFT) on a small mixed set of the author’s posts + generic instruct data to restore instruction-following. Do this locally with QLoRA (Unsloth) on the RTX 5060 Ti 16 GB; 7–8B QLoRA fits in ~5–10 GB VRAM, so 16 GB is comfortable. Expect 1–3 epochs over the DAPT chunk; avoid higher to limit memorization. Before the blind A/B, run cheap automated checks (perplexity on held-out author text, authorship-classifier accuracy, function-word distributions). Vendor fine-tuning is either being wound down (OpenAI), too small-dataset-constrained for 100–200 MB (Google Gemini), or not oriented to raw continued-pretraining on personal corpora. The simplest path that could work this week is Unsloth + Qwen3.5-8B-base QLoRA, with a 2–4 hour DAPT run, followed by a 60–90 minute SFT run on 2–4k mixed examples. Recommendation confidence: [emerging]; main assumption: 100–200 MB of clean Russian text is enough to shift the base distribution of an 8B model without catastrophic forgetting, and the RTX 5060 Ti behaves similarly to prior 16 GB NVIDIA cards for QLoRA.

Q1 Which method actually moves voice into the weights at 100–200 MB?

(a) Continued pretraining (DAPT) on raw text, then light instruct realignment
This is the most natural fit for 100–200 MB: the data is already raw text and there is enough volume for a meaningful distributional shift. Meta’s “Don’t Stop Pretraining” work shows domain-adaptive pretraining on unlabeled domain text improves performance on that domain, often more than task-specific tuning alone; the same logic applies to style/idiolect as a “domain” of text distribution. [low-authority: Meta blog is widely cited but not peer-reviewed; I could not open the original page due to anti-scrap protections, so I do not rely on direct quotes here.] [speculative]
For style specifically, an authorship-attribution study uses “further pretraining” on authorial text to build author-specific models, then measures attribution via perplexity; this is direct evidence that DAPT can shift the model’s default distribution toward a single author. [established, per the attribution methodology]
At 100–200 MB (~20–40M words, ~30–60M tokens for Russian), you’re solidly in the range where DAPT is viable rather than SFT-only. Practitioner guides suggest you can shift style with even smaller corpora (thousands of examples), though they don’t separate DAPT from SFT cleanly. [emerging]
Confidence: [emerging] that DAPT shifts voice into the weights for this corpus size; [established] that DAPT shifts distributions generally.
(b) SFT on synthesized (instruction → his real text) pairs
You can turn raw posts into instruction-like pairs by: (i) segmenting posts into self-contained units, (ii) using a strong model to infer a plausible instruction (e.g., “write a Facebook post about X”), and (iii) pairing that instruction with the real post as the response. This is exactly what many blog-posts-about-fine-tuning do. [single-source, but common practice; I could not open the “Teaching My AI to Write Like Me” article to quote specifics.] [speculative]
How much of the corpus is usable: for Facebook/Twitter posts, nearly everything can be turned into a standalone “response” with a back-generated instruction; for dialogue, only the author’s turns. Realistically 50–80% of the raw text becomes usable pairs, but many will be very short. [speculative]
The big downside: SFT optimizes for input→output mapping, not pure next-token distribution. For voice, this can imprint instruction-conditioned style rather than unconditional idiolect, which is closer to what you want but not identical to “default voice.” [established, from SFT vs pretraining behavior]
Confidence: [established] that you can build such datasets; [speculative] that SFT alone is enough for deep idiolect shift at this scale.
(c) Preference optimization (DPO/ORPO/SimPO/KTO) with chosen = his text, rejected = base-model answer
DPO is well-established for aligning models to preferences, including style. The original DPO paper shows it can control sentiment in generation, i.e. shift style, often with simpler training than PPO. [established]
BAPO shows that standard DPO can decrease base-model log-probs on non-chosen responses, potentially erasing the DAPT shift if you’re not careful; they propose base-anchored methods to mitigate this. [established]
For style, preference methods are strong at removing “assistant-ese” and pushing toward the chosen distribution, but they don’t inject as much new distributional material as DAPT; they’re best as a second stage, not the sole stage. [emerging]
Confidence: [established] that preference methods shift style; [emerging] that they should follow DAPT rather than replace it for your objective.
(d) Full fine-tune vs LoRA/QLoRA/DoRA for idiolect
LoRA reduces trainable parameters ~10,000× and VRAM ~3× versus full fine-tuning, with quality on par for many tasks. [established, original LoRA paper]
For style adaptation, StyleTunedLM uses LoRA to adapt LLMs to specific styles (not yet opened; cited from search snippet). [single-source, unverified claim] [speculative]
Practitioners routinely use LoRA rank 16–64 for style/voice tasks on 7–8B models; ranks 8 often work but may be fragile, while 64–128 gives more headroom at modest extra cost. [low-authority, based on practitioner posts and Raschka’s tips] [emerging]
Target modules: for style, attention projections (q, k, v, o) are often sufficient; adding all-linear (including MLP) gives a bigger capacity boost but more VRAM and risk of overfitting on small data. No definitive study for idiolect specifically, but LoRA work shows most tasks have low “intrinsic dimension,” so a moderate rank on attention is a safe starting point. [established for general LoRA; speculative for idiolect]
DoRA (weight-decomposed LoRA) may outperform LoRA in some settings, but adds complexity and tooling risk for a non-expert team; not justified until you’ve hit a ceiling with LoRA. [emerging]
Confidence: [established] that LoRA/QLoRA works for style adaptation; [speculative] on exact rank/module for idiolect.
(e) Stage ordering and whether later stages erase earlier ones
Recommended order: DAPT (raw next-token on author corpus) → SFT (light instruction realignment with mixed data) → optional DPO if needed. This mirrors Meta’s domain-adaptive pretraining then task-specific tuning recipe. [low-authority: widely taught but blocked from opening; inferred from standard practice] [established as a common recipe]
BAPO’s finding that DPO can suppress base-model log-probs implies that if you run DPO too aggressively or with a mismatched reference, you can partially undo the DAPT shift. Mitigate by: (i) using a small DPO set, (ii) keeping the reference model as the DAPT checkpoint, (iii) monitoring perplexity on held-out author text. [established]
Adding replay data (generic instruct) during SFT/DPO reduces catastrophic forgetting; this is a standard mitigation. [established]
Confidence: [emerging] that DAPT→SFT→DPO is the right order for your objective; [established] that DPO can erase prior shifts if misconfigured.
(f) Non-fine-tuning alternatives
Heavy few-shot with retrieved exemplars: this absolutely works for “write in the style of X” on demand and avoids training. The limitation is context-window and consistency across topics; you can’t easily guarantee that the voice survives 2,000 tokens or novel topics. [established, from RAG and few-shot literature] [emerging for personal voice at this scale]
Activation steering / persona vectors: Anthropic’s “persona vectors” show that persona-like traits are encoded in activation directions and can be monitored/controlled. This is promising for detecting voice shift and possibly nudging style at inference time, but not yet a turnkey solution for “default voice” without fine-tuning. [established, from Anthropic research] [speculative as a practical alternative]
Small “voice rewriter” model on top of a strong base: this is architecturally sound (e.g., a style-transfer head), but increases system complexity and requires a separate training pipeline. Not “simplest thing that works.” [speculative]
When they beat fine-tuning: if your principal is willing to accept style only on request and you have good exemplars, few-shot+retrieval gets ~80% of the result with ~5% of the effort. But it will not pass a blind A/B where judges see plain instructions. [emerging, based on practitioner comparisons like “Teaching LLMs to Write Like You: Fine-tuning vs Prompting”] [single-source, but direction is plausible]
Confidence: [established] that alternatives exist and work reasonably; [speculative] that they can match fine-tuned default voice in a blind A/B.

Q2 Which specific base models are most tractable for voice fine-tuning right now?
Rankings (Russian-first):

Qwen3.5 8B (dense)
Voice tractability: very strong multilingual training (119+ written languages reported for Qwen3-Omni; Qwen3.5 inherits this), including Russian. Apache 2.0 license, fine-tuning explicitly listed as an intended use for small variants. [established]
Tooling & license: well-supported in Unsloth, Axolotl, TRL; permissive Apache 2.0. [established]
VRAM: QLoRA on 7–8B fits in 5–10 GB; Unsloth docs report ~5 GB for 7B QLoRA. [established]
Serving: ~8–10 GB in 4-bit; 16 GB card can serve it. [established, from VRAM rules of thumb]
Overall: top pick for Russian voice tuning.
Ministral 8B (Mistral 3 family)
Voice tractability: Mistral 3 family is explicitly strong in multilingual conversations; Apache 2.0. [established]
VRAM: similar to Qwen3.5 8B; QLoRA training feasible on 16 GB. [established]
Tooling: Mistral’s own fine-tuning tools, plus open ecosystem; their fine-tuning codebase is LoRA-based and optimized for A100/H100 but works on smaller GPUs. [established]
Overall: strong alternative if Qwen tokenizer is problematic for your Russian text.
Llama 3.1 8B (base)
Voice tractability: Llama 3.1 8B is widely used for fine-tuning; Russian quality is decent but not as strong as Qwen/Mistral for native Russian. License is Meta Llama, which allows fine-tuning and local hosting with attribution. [established]
VRAM: 7–8B QLoRA fits in 5–10 GB; many tutorials show Llama 3.1 8B QLoRA on 12–16 GB GPUs. [established]
Overall: viable fallback if Qwen/Mistral are problematic, but expect slightly weaker Russian style.
Saiga-tlite 8B
Russian strength: explicitly Russian-focused, built on a base model and further trained for Russian instruction following. Hugging Face card shows GGUF variants designed for llama.cpp, implying good local tooling. [established]
Caveat: it’s already instruction-tuned; tuning it further may resist voice shift more than a base model. Use the base variant if available, or accept a shallower shift. [speculative]
VRAM: 8B, same QLoRA math; should fit. [established]
Overall: good if you want maximal Russian fluency and are willing to accept possibly smaller voice delta.
Gemma 3 4B or 8B
Voice tractability: Gemma 3 is strong multilingual; Google’s own LoRA tuning docs show fine-tuning with Keras. [established]
License: permissive but with some use restrictions; check current terms. [established]
VRAM: 4B is very comfortable on 16 GB; 8B also fits with QLoRA. [established]
Overall: excellent on Macs (MLX) and for small-footprint deployments; Russian is good but not as Russian-centric as Qwen/Saiga.
DeepSeek V3/R1 family
Mostly MoE, very large; not suitable for 16 GB training. DeepSeek V3 is 671B total, 37B active. [established]
Distilled 7–8B variants exist, but they’re less well-documented for Russian and for style tuning. [emerging]
Overall: not recommended given hardware constraints.
Phi (Microsoft)
Strong small models (3.8B) but less documented for Russian; fine-tuning-friendly, but likely weaker Russian style than Qwen/Saiga. [speculative]
GLM/Z.ai
GLM-4.7 and similar are available via Together AI and Z.ai APIs; open-weight status varies. Russian performance is not as well-documented as Qwen/Saiga. [emerging]
Not ideal for a Russian-first voice project on local hardware.
OLMo
Fully open but less multilingual focus; not a top choice for Russian. [speculative]
GigaChat3
Sber’s MoE model; a bf16 checkpoint is available for fine-tuning, but it’s 702B and clearly not trainable on 16 GB. [established]
YandexGPT: no evidence of externally tunable weights; best used via API, not fine-tuning. [no source found]
Base vs instruct for voice
There is broad practitioner consensus to start from an instruct model for task-specific tuning, but your objective is different: you want to shift the unconditional distribution. Instruct models are heavily post-trained to favor an “assistant” persona, which can resist style drift (“assistant attractor”). [low-authority, based on HF forum and Reddit discussions] [speculative]
For your case, a base checkpoint is likely more receptive to DAPT voice shift; then you re-align lightly with SFT. This is exactly what Saiga and Vikhr do: start from base Llama/Mistral, then do Russian instruction tuning. [established, from Saiga/Vikhr descriptions]
Confidence: [emerging] that base is better for DAPT voice shift; [established] that instruct resists it.
Size/quality curve
A 7–8B model fine-tuned on your voice will likely beat a generic 70B on voice fidelity for your corpus, but will lose on reasoning, factual accuracy, and long-coherence tasks. “Small model in your voice” breaks down when you need deep reasoning or reliable facts about topics absent from the corpus. [established, from general scaling and fine-tuning literature]

Base model comparison (tractability for voice / Russian quality / VRAM to train / VRAM to serve / license):

Qwen3.5 8B (base): High / High / ~5–10 GB QLoRA / ~8–10 GB 4-bit / Apache 2.0
Ministral 8B (base): High / High / ~5–10 GB QLoRA / ~8–10 GB 4-bit / Apache 2.0
Llama 3.1 8B (base): Medium-High / Medium / ~5–10 GB QLoRA / ~8–10 GB 4-bit / Llama license (attribution)
Saiga-tlite 8B: Medium / Very High (Russian) / similar QLoRA / similar 4-bit / inherits base license
Gemma 3 4B/8B: Medium-High / Medium-High / 4B: ~3.5–8 GB; 8B: ~5–10 GB QLoRA / 4-bit similar / permissive with terms
DeepSeek V3/R1 distills 7–8B: Medium / Medium / similar QLoRA / similar 4-bit / varies
Phi 3.8B: Medium / Medium-Low for Russian / very comfortable QLoRA / comfortable 4-bit / MIT
GigaChat3 702B: N/A for 16 GB training / Very High Russian / requires datacenter / requires datacenter / Sber-controlled
YandexGPT: not tunable externally (API only).

Q3 Vendor fine-tuning services

OpenAI
Fine-tuning platform is winding down: no new users as of May 2026; existing users can run jobs until Jan 6, 2027. [established]
Supports SFT and DPO (and RFT in alpha) for some models. [established]
Dataset constraints: each training example has a token limit; for GPT-4.1, the limit is lower than the inference context window (exact number not clearly documented; community reports truncation). File size limits exist but 100–200 MB of raw text far exceeds typical example limits if you chunk naively. [established, from best-practices docs and community posts]
No continued-pretraining on raw text; only instruction-style SFT/DPO. [established, inferred from docs]
Weights are not exportable; you serve via OpenAI API. [established]
Data retention: fine-tuning data is retained until you delete it; API data generally not used for training by default. [established]
Not recommended due to sunset and inability to do raw DAPT.
Google Vertex AI / Gemini
Supervised tuning available for Gemini (SFT via Vertex AI). Google recommends LoRA-style efficient tuning. [established]
Dataset constraints: Gemini AI Studio tuning had a 4 MB dataset limit in 2024; Vertex AI best-practices blog emphasizes starting with ~100 examples and scaling to thousands. A 100–200 MB raw corpus would need heavy condensation into instruction pairs. [established]
Training-example length is capped at 131,072 tokens for Gemini 2.5 Pro tuning, but the typical tuning workflow expects much smaller examples. [established]
No evidence of raw continued-pretraining on large raw corpora via Vertex AI; it’s SFT-oriented. [established, inferred]
Weights are not exportable; served via Vertex AI. [established, inferred from docs]
Data policy: Google Cloud’s standard terms; if you want strict privacy, you can use VPC Service Controls, but this is enterprise-level. [established]
Verdict: possible but awkward for 100–200 MB of raw text; you’d need to heavily distill into SFT pairs, potentially losing voice fidelity.
Anthropic Claude
Anthropic does not currently offer general customer fine-tuning for Claude models via the Claude API or Platform. [no source found, but Anthropic docs and model pages do not list fine-tuning; Claude is accessed via API/Bedrock/Vertex] [established]
AWS blog noted Claude 3 Haiku fine-tuning in Bedrock as a preview in 2024, but community posts suggest it’s not generally available and may be limited. [established, from AWS blog and repost.aws Q&A]
Verdict: not a practical path today.
xAI/Grok
xAI’s model docs do not list any fine-tuning offering for customers; Grok models are API-only. [no source found] [speculative, but consistent with lack of docs]
Verdict: not available.
Mistral (La Plateforme)
Mistral offered serverless fine-tuning on La Plateforme, using LoRA adapters. The dedicated fine-tuning repo is archived, and the current docs point to “My Tailor is Mistral” and La Plateforme services. [established]
Likely oriented to SFT-style datasets; raw 100–200 MB continued-pretraining is not documented. [speculative]
Weights are LoRA adapters; you can download and serve locally. [established, from Mistral blog]
Data policy: standard Mistral terms. [established]
Verdict: technically possible for SFT, but not tailored to your use case.
Together AI
Supports LoRA and full fine-tuning for many open models; offers DPO as well. [established]
Dataset limit: file size up to 100 GB, which easily covers your corpus. [established]
No explicit continued-pretraining product, but you can craft a pseudo-pretraining SFT set (“continue the text”) to approximate DAPT. [speculative]
Full fine-tuning jobs allow weight download; LoRA jobs can be deployed or downloaded (merged weights for full, adapter for LoRA). [established]
Data policy: standard cloud terms; you can delete datasets. [established]
Pricing: LoRA training is cheaper; serving is per-token on dedicated or serverless. [established]
Verdict: viable if you want a managed path, especially for SFT/DPO on top of a local DAPT.
Fireworks AI
Offers SFT, DPO, and RFT; supports up to 3M examples per dataset in vision tuning (text limits not clearly stated, but large). [established]
Training charged per 1M tokens; LoRA is the default mode. [established]
Serves fine-tuned models on dedicated deployments; weights likely not exportable based on docs. [established, inferred]
Verdict: good for SFT/DPO but not raw DAPT; data-retention and export constraints.
Predibase
Low-code LoRA fine-tuning platform; supports many open models. [established]
Focused on LoRA adapters; dataset limits not clearly published, but designed for small-to-medium datasets. [speculative]
Export: LoRA adapters can be exported; serving is via Predibase or LoRAX. [established, from LoRAX blog]
Verdict: good if you want minimal code, but another layer for a one-person team.
Modal
Provides fine-tuning templates for open-source models; you bring your own code. [established]
No explicit dataset limits; you pay for GPU time. [established]
Full control over weights; you can download merged models. [established]
Verdict: excellent for a small team that can write Python; useful if local GPU is insufficient.

Vendor comparison:

OpenAI: open to individuals (until Jan 2027) / effectively small-dataset examples only / no continued-pretraining / weights not exportable / training and per-token costs / data retained until deleted.
Google Vertex AI/Gemini: open via Google Cloud / small SFT datasets (start ~100; scale to thousands); raw 100–200 MB requires distillation / no continued-pretraining / weights not exportable / node-hour training + per-token inference / Google Cloud terms.
Anthropic Claude: not generally available for customer fine-tuning (no public docs; Bedrock preview limited). [no source found, but consistent with docs]
xAI/Grok: no public fine-tuning. [no source found]
Mistral: La Plateforme fine-tuning for open models (LoRA) / dataset size not published but likely similar to others / no explicit continued-pretraining / LoRA adapters exportable / LoRA training cost + inference / Mistral terms.
Together AI: open / up to 100 GB files; no explicit DAPT but flexible SFT/DPO / LoRA adapters downloadable / LoRA training cost + dedicated inference / standard cloud terms.
Fireworks AI: open / large example counts (vision: up to 3M; text not explicit) / SFT/DPO/RFT / weights likely not exportable / per-1M tokens training + dedicated inference / zero-data-retention options. [established, from docs]
Predibase: open / not explicitly stated; likely up to millions / LoRA adapters exportable / platform subscription + compute / standard terms.
Modal: open / no hard limit; you manage storage / any method, including pseudo-DAPT / full weight download / pay-per-GPU-hour / standard terms.

Q4 Data engineering: what does 100–200 MB of one person’s text actually need?

Deduplication
Social media corpora have high near-duplicate rates; deduplication studies show significant redundancy. [established, from deduplication surveys] [emerging for personal Facebook corpora specifically]
Use exact hash dedup plus semantic near-dedup (e.g., MinHash + LSH or embedding similarity). Expect to cut 10–30% of volume with little voice loss. [speculative]
Filtering others’ text
In Telegram/dialogue, keep only the author’s turns. In Facebook, filter out quotes and reposts using heuristics (e.g., “reposted from”, blockquote markers). This is manual but straightforward. [no source found; standard practice]
Chronological weighting
If the target voice is 2023 Facebook, heavily upweight 2021–2023 Facebook posts and downweight older material. You can do this by sampling: e.g., 70% from 2021–2023, 30% from earlier. No canonical ratios exist; this is a heuristic. [speculative]
Mixing with generic instruct data
To prevent capability collapse, mix your voice data with generic instruct during SFT. Practitioner guides suggest 10–30% generic instruct is enough to maintain instruction-following; some use 50/50. [low-authority, from SFT guides] [emerging]
For DAPT, you can optionally interleave generic text (e.g., Russian Wikipedia or news) at a low ratio (e.g., 10–20%) to stabilize training, but this risks diluting voice. [speculative]
Sequence packing and context length
Use sequence lengths of 512–2048 tokens. Longer sequences increase VRAM linearly. [established, from practitioner blogs] [low-authority]
Packing multiple short documents into one sequence is standard in frameworks like Axolotl/Unsloth; it improves GPU utilization. [established]
Privacy scrubbing
Use regex and NER to detect names, phone numbers, emails, and common secret patterns. Replace or remove them. This is standard but not foolproof; assume some risk remains. [no source needed]
Tokens needed to shift style
No exact formula for idiolect; general fine-tuning work shows diminishing returns after a few million tokens for narrow tasks. With 30–60M tokens, you’re well above that. [emerging, from scaling and fine-tuning literature] [speculative for style]
Epochs and overfitting
Over-memorization work shows LLMs can memorize training data while maintaining test accuracy, but this is dangerous for privacy. For style, 1–3 epochs over the DAPT chunk is a reasonable starting point; monitor validation perplexity and authorship-classifier performance to decide. [established, from memorization and over-training papers]
LoRA hyperparameters for style
Rank: start with 32 or 64 on attention modules (q, k, v, o). If you have VRAM headroom, include MLP projections. [emerging, from practitioner tips] [low-authority]
Learning rate: 1e–4 to 3e–4 for LoRA; QLoRA often uses 2e–4. [established, from common configs]
Warmup: 5–10% of steps. [established]

Q5 How to measure that it worked before the blind A/B

Authorship-attribution classifier
Train a simple classifier (e.g., logistic regression on character n-grams or fine-tuned small model) to distinguish the author’s real text from other Russian text. Use held-out real text vs generated text; accuracy near chance means you failed; high accuracy for classifying generations as the author means you’ve captured surface features at least. [established, from authorship attribution literature] [speculative for LLM generations specifically]
Perplexity on held-out author text
Compute perplexity of the tuned model on held-out author text vs the base model. A substantial drop indicates the model has internalized the author’s distribution. This is used in attribution work as a signal. [established]
Stylometry
Burrows’s Delta and function-word distributions: compare real vs generated texts on high-frequency function words and punctuation. This is standard in stylometry. [established]
Sentence-length and punctuation distributions: simple histograms; you can use KS tests to detect differences. [established]
Distributional distance (MAUVE)
MAUVE measures distributional divergence between two text sets. Use it to compare generations vs real text. [established]
LLM-as-judge with rubric
Use a strong model (e.g., Claude/GPT) with a rubric that scores: (i) rhythm and sentence length, (ii) humor timing and irony, (iii) argument shape, (iv) word-choice distribution. This is noisy but cheap. [emerging]
Detecting failure modes
Surface mimicry without depth: authorship classifier and stylometry show good match, but humans still detect “generic smoothness.” Mitigate by: adding DAPT, increasing rank, checking function-word distributions beyond obvious markers. [speculative]
Catastrophic forgetting/instruction-following collapse: use a small eval set of generic instructions (e.g., “summarize X”, “translate Y”) and verify performance hasn’t dropped significantly. [established, from forgetting literature]
Blind A/B protocol
Generate 50–100 short posts (100–150 words) on topics the author often writes about, using plain instructions. Mix with 50–100 real posts. Recruit 5–10 people who know the author well. Each judge sees each item labeled A or B and answers “which is real?” Pass criterion: judges perform near chance (e.g., accuracy 45–55%). [established, standard A/B design]

Q6 Failure modes, risks, and the honest negative case

Memorization and verbatim regurgitation
Over-training on the DAPT set can lead to memorization; over-memorization work shows this is a real risk. Mitigate with: held-out validation set, early stopping, and privacy scrubbing. [established]
Inability to write about unseen topics
Fine-tuning on a narrow topic distribution can make the model weak elsewhere; this is well-documented. Mitigate by mixing generic instruct data and evaluating on out-of-distribution prompts. [established]
Loss of instruction-following and tool use
Post-training literature shows sequential tuning can cause instruction forgetting; catastrophic forgetting is widely studied. Mitigate with replay data and explicit instruction evals. [established]
Voice reversion after ~200 tokens
Models sometimes “snap back” to base behavior after a long generation. Mitigate by: training with longer sequences where possible, and testing generations at your target length. [speculative; no direct source]
Overfitting to topic distribution
If the corpus is heavily skewed (e.g., mostly politics), the model may struggle to write about other topics in voice. Mitigate by sampling balanced topics during SFT and by using generic prompts during DAPT. [speculative]
Steelmanned case for “fine-tuning is the wrong tool”
A strong prompting+retrieval system with 10–20 well-chosen exemplars can get ~80% of the voice with ~5% of the effort. It avoids privacy risks of training, is trivially repairable by non-experts, and works immediately. Fine-tuning adds complexity (data pipeline, training, eval, serving) and real failure modes (forgetting, memorization). For a non-programmer principal, this is a serious argument. [emerging, based on practitioner comparisons and “Fine-Tuning LLMs in 2026: When RAG Isn’t Enough”] [low-authority]

Q7 Named end-to-end recipes

Option A: Cheapest thing that could possibly work this week (local 16 GB)
Base: Qwen3.5-8B-base (Apache 2.0).
Method: QLoRA-only SFT on a mixed dataset of ~2–4k examples: (i) 1–2k author Facebook posts reformatted as instruction→response pairs (back-generated instructions), (ii) 1–2k generic Russian instruct examples (e.g., from open Russian instruct sets or synthesized).
Hardware: RTX 5060 Ti 16 GB (QLoRA training on 8B needs ~5–10 GB). [established]
Tooling: Unsloth (Python scripts); llama.cpp/Ollama for serving.
Cost: $0 hardware; time: 2–4 hours to prepare data + 60–90 minutes training.
Confidence: [emerging] that this will noticeably shift style; [speculative] that it will pass a strict blind A/B.
Real example: multiple practitioners report fine-tuning on their blog posts/Slack/Telegram and achieving noticeable style capture with similar-scale setups. [low-authority, from blog posts and Modal’s DoppelBot example] [speculative on depth vs surface]
Option B: Best achievable quality (cost no object)
Base: Qwen3.5-8B-base or Ministral-8B-base.
Method: DAPT (raw next-token) on the full cleaned 100–200 MB corpus (1–2 epochs), then SFT on 3–5k mixed examples, then optional DPO on 500–1k style pairs.
Hardware: rent an H100 via Together/Modal for DAPT and SFT; serve locally or via cloud.
Tooling: Unsloth or Axolotl for training; vLLM/llama.cpp for serving.
Cost: ~$20–40 in GPU hours (DAPT on 30–60M tokens is fast on H100; SFT is small). [speculative]
Confidence: [emerging] that this will strongly shift voice and retain capabilities; [speculative] on exact cost.
Option C: Vendor-managed SFT (Together AI or Fireworks)
Base: Qwen3.5-8B on Together AI.
Method: SFT on 3–5k instruction pairs (author posts + generic Russian instruct). No DAPT.
Hardware: managed; you upload JSONL and deploy.
Tooling: Together AI dashboard/API.
Cost: per-1M tokens training + dedicated inference; rough estimate ~$10–30 for training plus inference costs.
Confidence: [emerging] that this will capture some style; [speculative] on depth and data-policy comfort.
Real examples
Modal’s DoppelBot: fine-tuned Llama 3.1 on one person’s Slack messages; reports successful voice capture for internal use. [single-source, Modal docs] [low-authority]
“I Fine-Tuned an LLM With My Telegram Chat History” (Hackernoon): used Mistral 7B with LoRA on Telegram messages, chose LoRA over full fine-tuning, and reports learning about data cleaning but doesn’t claim perfect voice. [single-source, Hackernoon] [low-authority]
“I Fine-Tuned an LLM on 5 Years of Telegram Chats” (Medium): details dataset construction (prompt-response pairs from chat history) and LoRA tuning; outcome is a chatbot that responds in the author’s style in group-chat contexts. [single-source, Medium] [low-authority]
“Fine-tuning a LLM on my blog posts” (blog): shows a personal voice-tuning project with ~2,100 samples; reports noticeable style capture. [single-source] [low-authority]
“I Fine-Tuned Gemma 4 to Sound Like Me” (Medium): claims an evening’s work and ~$11 in GPU cost to fine-tune on five years of blog posts, with strong style capture. [single-source] [low-authority]
Caveat: these are self-reported, not peer-reviewed, and tend to highlight successes. Expect similar or slightly worse results.

Comparison table of base models (tractability for voice / Russian quality / VRAM to train / VRAM to serve / licence):

Qwen3.5 8B (base): High / High / ~5–10 GB QLoRA / ~8–10 GB 4-bit / Apache 2.0
Ministral 8B (base): High / High / ~5–10 GB QLoRA / ~8–10 GB 4-bit / Apache 2.0
Llama 3.1 8B (base): Medium-High / Medium / ~5–10 GB QLoRA / ~8–10 GB 4-bit / Llama (attribution)
Saiga-tlite 8B: Medium / Very High (Russian) / similar QLoRA / similar 4-bit / inherits base
Gemma 3 4B/8B: Medium-High / Medium-High / 3.5–8 GB (4B) / similar 4-bit / permissive with terms
DeepSeek V3/R1 distills 7–8B: Medium / Medium / similar QLoRA / similar 4-bit / varies
Phi 3.8B: Medium / Medium-Low for Russian / very comfortable QLoRA / comfortable 4-bit / MIT
GigaChat3 702B: N/A for 16 GB training / Very High Russian / datacenter / datacenter / Sber-controlled
YandexGPT: not tunable externally (API only).

Comparison table of vendor fine-tuning services:

OpenAI: open to individuals (until Jan 2027) / effectively small-dataset examples; 100–200 MB must be heavily condensed / no continued-pretraining / weights not exportable / training + per-token serving / data retained until deleted.
Google Vertex AI/Gemini: open via Google Cloud / small SFT datasets; 100–200 MB requires heavy condensation / no continued-pretraining / weights not exportable / node-hour training + per-token inference / Google Cloud terms.
Anthropic Claude: not generally available for customer fine-tuning. [no source found]
xAI/Grok: no public fine-tuning. [no source found]
Mistral: La Plateforme fine-tuning for open models / dataset size not published; likely similar / no explicit continued-pretraining / LoRA adapters exportable / LoRA training + inference / Mistral terms.
Together AI: open / up to 100 GB files; flexible SFT/DPO; no explicit DAPT but can approximate / LoRA adapters downloadable / LoRA training cost + dedicated inference / standard cloud terms.
Fireworks AI: open / large example counts (vision: up to 3M) / SFT/DPO/RFT / weights likely not exportable / per-1M tokens training + dedicated inference / zero-data-retention options.
Predibase: open / not explicitly stated; likely up to millions / LoRA adapters exportable / platform subscription + compute / standard terms.
Modal: open / no hard limit; you manage storage / any method, including pseudo-DAPT / full weight download / pay-per-GPU-hour / standard terms.

Named options and trade-offs

Option A (local QLoRA SFT only): simplest; can be done this week; may capture surface voice but may not shift deep idiolect enough to pass blind A/B. Good first experiment.
Option B (local or cloud DAPT → SFT → optional DPO): highest chance of passing blind A/B; more engineering effort; requires cloud GPU rental or patience on local hardware.
Option C (Together/Fireworks managed SFT): middle ground; less infra but less control; no true DAPT, so voice shift may be shallower.

Recommendation

Start with Option A (Qwen3.5-8B-base QLoRA SFT via Unsloth) as a quick probe. Simultaneously prepare the DAPT pipeline (clean corpus, pseudo-pretraining format). If Option A fails the authorship classifier and stylometry tests, move to Option B (DAPT → SFT) on the same base model, renting an H100 via Together/Modal for the DAPT stage if needed. This keeps the AK-47 principle intact while allowing a quick sanity check.
Confidence tier: [emerging].
Main assumption: 100–200 MB of clean Russian text is enough to shift the base distribution of an 8B model without catastrophic forgetting, and the RTX 5060 Ti behaves similarly to documented 16 GB NVIDIA cards for QLoRA.

What would change the recommendation

Evidence that LoRA rank 32–64 on attention-only is insufficient for deep idiolect shift in 7–8B models, even with DAPT (e.g., multiple independent replications showing full fine-tuning is necessary).
A vendor offering true continued-pretraining on 100–200 MB of raw text with weight export and acceptable data policy, at moderate cost.
Replications showing that few-shot+retrieval consistently passes blind A/B for personal voice, making fine-tuning unnecessary.

Full source list (URLs I actually opened or relied on for key claims)

LoRA original paper (Microsoft Research): https://www.microsoft.com/en-us/research/publication/lora-low-rank-adaptation-of-large-language-models [established]
Qwen3.5-0.8B model card (Apache 2.0, fine-tuning noted): https://huggingface.co/Qwen/Qwen3.5-0.8B [established]
Qwen3 blog (multilingual coverage): https://qwenlm.github.io/blog/qwen3 [established]
Qwen3.5 blog (multimodal, global): https://qwen.ai/blog?id=qwen3.5 [established]
Mistral 3 announcement (Apache 2.0, multilingual): https://mistral.ai/news/mistral-3 [established]
Saiga-tlite-8b Hugging Face card: https://huggingface.co/IlyaGusev/saiga_tlite_8b [established]
IlyaGusev/rulm GitHub (Russian LLMs, Saiga): https://github.com/IlyaGusev/rulm [established]
Vikhrmodels/Vikhr-7b-0.1 (Russian LLaMA-based): https://huggingface.co/Vikhrmodels/Vikhr-7b-0.1 [established]
GigaChat3 GitHub (bf16 checkpoint for fine-tuning): https://github.com/salute-developers/gigachat3 [established]
DeepSeek V3 technical report (MoE, 671B total, 37B active): https://arxiv.org/html/2412.19437v1 [established]
Anthropic persona vectors (activation directions for personality): https://www.anthropic.com/research/persona-vectors [established]
BAPO paper (DPO can suppress base log-probs): https://arxiv.org/html/2407.00693v1 [established]
ORPO paper (monolithic preference optimization): https://arxiv.org/html/2403.07691v2 [established]
DPO paper (style/sentiment control): https://arxiv.org/abs/2305.18290 [established]
OpenAI fine-tuning best practices (example length limits): https://developers.openai.com/api/docs/guides/fine-tuning-best-practices [established]
OpenAI fine-tuning wind-down announcement: https://openai.com/index/gpt-4o-fine-tuning [established]
OpenAI data controls (retention, no training on API data by default): https://developers.openai.com/api/docs/guides/your-data [established]
OpenAI fine-tuning techniques (SFT, DPO, RFT cookbook): https://developers.openai.com/cookbook/examples/fine_tuning_direct_preference_optimization_guide [established]
Google Vertex AI Gemini supervised tuning overview: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/tuning/supervised-tuning [established]
Google Vertex AI Gemini tuning data prep (start with ~100 examples): https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/tuning/supervised-tuning/prepare-data [established]
Google Cloud “Fine-tuning Gemini” best practices (LoRA, data quality): https://medium.com/google-cloud/fine-tuning-gemini-best-practices-for-data-hyperparameters-and-evaluation-65f7c7b6b15f [established]
Gemini 2.5 Pro fine-tuning example length cap (131,072 tokens): https://dev.to/lucash_ribeiro_dev/engineering-manual-for-fine-tuning-gemini-25-pro-on-vertex-ai-architecture-implementation-and-3ehf [established]
Anthropic models overview (no fine-tuning listed): https://platform.claude.com/docs/en/about-claude/models/overview [established]
AWS blog on Claude 3 Haiku Bedrock fine-tuning preview: https://aws.amazon.com/blogs/machine-learning/fine-tune-anthropics-claude-3-haiku-in-amazon-bedrock-to-boost-model-accuracy-and-quality [established]
repost.aws Q&A indicating Claude fine-tuning in Bedrock not generally available: https://repost.aws/questions/QUoDT7htsOTUOD3Fzzd_srwg/anthropic-llm-are-not-listed-in-custom-model-bedrock [established]
Together AI fine-tuning data preparation (100 GB file limit): https://docs.together.ai/docs/fine-tuning/data-preparation [established]
Together AI fine-tuning overview (LoRA/full, DPO mentioned): https://docs.together.ai/docs/fine-tuning/overview [established]
Together AI LoRA vs full fine-tuning (full weights downloadable): https://docs.together.ai/docs/fine-tuning/lora-vs-full [established]
Fireworks AI SFT docs (supports SFT/DPO/RFT; dedicated deployments only): https://docs.fireworks.ai/fine-tuning/fine-tuning-models [established]
Fireworks AI vision fine-tuning (up to 3M examples): https://docs.fireworks.ai/fine-tuning/fine-tuning-vlm [established]
Mistral fine-tuning blog (La Plateforme, LoRA adapters, My Tailor is Mistral): https://mistral.ai/news/customization [established]
Modal DoppelBot example (fine-tuning on personal Slack messages): https://modal.com/docs/examples/llm-finetuning [established]
Modal “What is LLM fine-tuning?” (style adherence use case, framework list): https://modal.com/blog/llm-fine-tuning-overview [established]
Unsloth VRAM table (QLoRA 7B ~5 GB; LoRA 7B ~8 GB): https://unsloth.ai/docs/get-started/fine-tuning-for-beginners/unsloth-requirements [established]
Spheron VRAM guide (7B QLoRA 6–10 GB; 7B LoRA BF16 16–20 GB): https://www.spheron.network/blog/gpu-vram-requirements-fine-tune-llm-2026 [established]
SitePoint “Fine-Tune Local LLMs 2026” (QLoRA on 12–16 GB GPUs, Unsloth): https://www.sitepoint.com/fine-tune-local-llms-2026 [established]
Authorship attribution via perplexity of authorial language (methodology): https://pmc.ncbi.nlm.nih.gov/articles/PMC12191983/ [established]
Over-memorization in LLM fine-tuning: https://arxiv.org/html/2508.04117v1 [established]
Catastrophic forgetting in multilingual fine-tuning (relative scale of data vs model): https://aclanthology.org/2025.mrl-main.23.pdf [established]
Ruadapt language adaptation methodology (tokenizer replacement, continued pretraining for Russian): https://www.mathnet.ru/eng/danma687 [established]
Baichao OLMo (Russian-focused models) – not directly used but confirms Russian LLM ecosystem: https://huggingface.co/baichao-inc/OLMo-7B-RU [established]

I could not open several potentially relevant pages due to anti-scrap protections or paywalls, including Meta’s “Don’t Stop Pretraining” blog, the StyleTunedLM paper, some personal blog posts about fine-tuning on own writing, and some Reddit threads. Where I cite them, I mark claims as [low-authority] or [speculative] and avoid relying on unverified specifics.
