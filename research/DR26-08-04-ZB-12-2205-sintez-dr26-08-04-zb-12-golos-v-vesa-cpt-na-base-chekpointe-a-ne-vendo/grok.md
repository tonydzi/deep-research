# DR26-08-04-ZB-12-2205 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll research this thoroughly — papers, vendor docs, tooling, and practitioner reports — then deliver a decision-grade report with citations and confidence tiers.# Decision report: can a personal authorial voice be moved into LLM weights?

**Searches run (sample):** continued pretraining / DAPT personal style; DPO/ORPO style adaptation; LoRA rank idiolect; OpenAI/Google/Anthropic/Mistral/Together/xAI fine-tuning limits; base vs instruct style resistance; T-lite/T-pro/Vikhr Russian models; MAUVE; representation engineering style vectors; personal-corpus fine-tune write-ups; LIMA; OpenAI FT deprecation.

**Primary pages opened (not just SERP snippets):**  
OpenAI model-optimization / FT docs · Google Vertex Gemini supervised tuning · Mistral fine-tuning docs · Together AI FT overview · Unsloth continued pretraining · Chris McCormick CPT walkthrough · Vikhr paper (arXiv HTML) · Style Vectors paper (arXiv HTML) · Anthropic persona vectors · MAUVE project page · T-lite-it-2.1 / T-pro HF cards · Hugging Face LLM4Russian collection · Google supervised-tuning about-limits (SERP + docs).

---

## 1. Executive summary (≤200 words)

**Yes — voice can be moved into weights, but not via “style SFT on a few thousand posts.”** At **100–200 MB / ~20–40M words**, the evidence-backed primary method is **continued pretraining (next-token CPT/DAPT) on raw author text**, preferably on a **base (not heavily RLHF’d instruct) checkpoint**, then a **light instruct re-align**, optionally **DPO** where chosen = his real text and rejected = base-model imitation.

**Vendor closed APIs are a poor fit for this goal:** they expose **instruction SFT / DPO**, not true CPT; **weights are usually non-exportable**; OpenAI is **winding down FT for new users**. Google allows large JSONL but still SFT/LoRA only. **Local open weights (Qwen / T-lite / Vikhr-class Russian bases) + Unsloth/Axolotl on the 16 GB card (pilot) then H100 for full pass** is the tractable path.

**Blind A/B “indistinguishable from him” is achievable for short FB-like posts if data is cleaned and target-era upweighted; it is not guaranteed for long-form reasoning or topics he never wrote about.**

**Recommendation (see §5):** Option B — **Qwen3-8B-base or T-lite base → CPT LoRA r≥64 all-linear → light SFT → optional DPO**, eval harness first. Confidence: **[emerging]**. Main assumption: **surface markers are not enough; default next-token must shift**, which SFT-only and prompt-only systematically fail to do.

---

## 2. Q1 — Which method actually moves voice into the weights?

### (a) Continued pretraining / DAPT on raw text → light instruct re-align  
**[established] for domain/format/style shift via next-token; [emerging] for personal idiolect at this scale**

- DAPT (Gururangan et al., 2020) established that **continued pretraining on domain text** improves domain performance more reliably than task-only fine-tuning when unlabeled domain data is available. (Paper: https://arxiv.org/abs/2004.10964 — classic; older than 18 months but still the reference.)  
- Practitioner stacks treat **raw next-token CPT** as the way to teach **OOD format and writing style**, not just jargon: Unsloth CPT docs; McCormick’s 2025 walkthrough explicitly maps “new style/format → CPT”.  
- Vikhr (Russian adaptation): **full-weight continued pretraining** on large Russian corpora + instruction tuning — not LoRA-only — and reports big gains vs LoRA-on-English base; they also warn CPT can **erase reasoning** without regularization (KL / replay).  

**Why this matches your corpus size:** 100–200 MB ≈ **~25–50M tokens** (Russian often more tokens/word than English). That is **tiny vs pretraining trillions**, but **large vs typical SFT sets (1k–50k examples)**. Every token is a training label under next-token loss — CPT **uses the whole pile**, whereas SFT wastes most of it unless you reverse-engineer instructions for every post.  

**Order of operations that is standard in practice:**  
1. CPT on author raw text (optionally mixed with some generic text for anti-forgetting)  
2. Short SFT on chat/instruction format  
3. Optional preference step  

**Risk of stage (3) erasing (1):** preference/SFT on **generic** assistant data **can dilute voice** (capability–style tradeoff). Preference on **his-vs-not-him** pairs is less dilutive. **[emerging / partially folklore]** — widely reported by practitioners; controlled ablations for *personal* idiolect are scarce.

### (b) SFT on (instruction → his real text) pairs  
**[established] for format/task; [speculative→emerging] as sole method for deep idiolect**

How to build pairs from posts:

| Technique | What it is | Usable fraction of corpus |
|---|---|---|
| Reverse instruction / “back-translation” | Strong model invents “Write a FB post about X…” for real post as target | High if auto; quality noisy |
| Topic extraction | Keywords/summary → prompt | Medium |
| Dialogue turns | Only **his** turns as completion; partner as user | Dialogue-only slice |
| Era filter | Only 2018–2023 FB as targets | Subset, higher purity |

**Limits:**  
- LIMA showed **~1k carefully curated** SFT examples can align style/behavior of a strong base — **quality ≫ quantity** for *instruction following / surface style*. (https://arxiv.org/abs/2305.11206, 2023; still cited.)  
- But LIMA does **not** prove default next-token becomes a specific human’s decade-long idiolect.  
- Vendor SFT (OpenAI, Gemini) is built around **example pairs**, often recommended in the **100–500 to few-thousand** range for style/format — **orders of magnitude smaller than your corpus**. Using 100–200 MB as “one giant SFT file” without structure is wrong format for those APIs.  

**Honest yield:** maybe **5–20% of posts** become clean single-turn SFT rows without heavy engineering; the rest is better used as **CPT raw**.

### (c) Preference optimization (DPO / ORPO / SimPO / KTO)  
**[emerging] for tone/style when pairs are well-defined**

- OpenAI’s own FT guide lists **DPO** as “best for… generating chat messages with the **right tone and style**.” (https://developers.openai.com/api/docs/guides/model-optimization)  
- Construction for *your* goal:  
  - **chosen** = his real post / real reply  
  - **rejected** = base (or strong generic) model’s answer to the same prompt  
- Reported pattern (literature + practice): preference methods **sharpen style discrimination** more than they inject knowledge. **[emerging]**  
- **Weak for knowledge, strong for “prefer this distribution over that”** — matches voice objective better than pure SFT of assistant-y paraphrases.

ORPO/SimPO/KTO: same preference idea with different losses; **no strong head-to-head on personal author idiolect** found in primary sources opened for this report → treat choice as implementation detail **[speculative]** beyond “use a preference objective on him-vs-not-him.”

### (d) Full FT vs LoRA / QLoRA / DoRA  
**[established] LoRA works for style/format on consumer GPUs; [emerging] rank/module choices for idiolect**

| Knob | Practitioner default for style CPT | Notes |
|---|---|---|
| Method | **QLoRA 4-bit + LoRA** on 16 GB | Unsloth path; full FT for 7–8B wants multi-GPU or rent |
| Rank `r` | **16–32** light; **64–128** for serious style shift | McCormick/Unsloth examples use **r=128** for CPT style; Raschka notes higher r + alpha scaling on small data |
| Targets | **all-linear** (q,k,v,o,gate,up,down) | Attention-only is outdated default |
| CPT extras | often include **`embed_tokens` + `lm_head`** with **2–10× lower LR** | Unsloth CPT docs |
| DoRA | marginal vs well-tuned LoRA | not required for first pass |

**Full fine-tune:** better if you want **maximum distributional shift** and can afford H100s (Vikhr: full-weight CPT). **LoRA can under-capacity** pure knowledge injection; for **style**, LoRA at high rank is often enough **[emerging]**.

### (e) Combinations and stage order  
**[emerging] best practice stack**

```
clean corpus → CPT (raw) → light SFT (chat format, mix his + generic) → DPO (him vs base) → eval → optional replay CPT
```

- **Does last stage erase first?** SFT/DPO on *generic* assistant data **can** pull toward “assistant attractor.” Mitigation: **keep preferred targets in his voice**, mix **small** generic instruct (e.g. 5–20% of SFT tokens, not 50%+), early-stop on voice metrics not just loss. **[emerging]**

### (f) Non-FT alternatives — when they beat fine-tuning  
**[established] that prompting is weaker for default distribution; [emerging] for steering**

| Alternative | Effort | Can it become **default** voice? | When it wins |
|---|---|---|---|
| Heavy few-shot + retrieved own posts | Low–med | **No** — still prompt-conditioned; long context drifts | MVP in days; topics outside corpus |
| Style / persona **activation vectors** | Med | **Partial** — inference-time add to residual stream | Controllable λ; no weight risk; papers show sentiment/writing-style steer (Shakespeare, modern). Does **not** replace training for decade-idiolect depth. |
| Small **rewriter** on top of strong base | Med | **Yes as pipeline default** | If you only need posts/replies; keep big model for reasoning |
| Full CPT+SFT+DPO | High | **Yes, closest to “in the weights”** | Blind A/B on his native genres |

Style-vector paper (Konen et al., 2024): activation-based style vectors steer sentiment/emotion/writing style without weight updates; too-large λ → collapse; factual prompts resist style.  
Anthropic **persona vectors** (2025): extractable directions for character traits; steering at inference.  
**Neither** demonstrates decade-long personal Russian FB idiolect as **unprompted default**.

**Steelman non-FT:** For **80% of “sounds more like me”** with **5% effort**, RAG of 8–20 real posts + strict decoding (low temp, stop sequences, rewrite pass) often beats a bad first fine-tune. Your stated failure mode (“LLM-smooth with surface markers”) is **exactly** what weak SFT and light prompting produce — so **either** invest in CPT+eval **or** accept rewriter architecture; don’t do mid-quality SFT and call it done.

---

## 3. Q2 — Base models (late 2025–2026 tractability)

### Base vs instruct (“assistant attractor”)  
**[emerging] for practice; theory is [established]**

- Instruct/RLHF models are trained to be **helpful, structured, safe assistants**. That is a **strong prior** competing with personal style.  
- McCormick (2025, [low-authority] blog but careful): CPT **should start from base**, because instruct is already a “chatbot style CPT”; further CPT **erases** it.  
- Practical implication: **CPT on base → light SFT for “answer instructions”** rather than CPT on a max-RLHF chat model.  
- Conflicting practitioner view: “just LoRA the instruct model, easier chat template” — common in forums **[low-authority]**; works for light style, fights you for deep idiolect.

### Russian quality  
**[established] that Cyrillic-dense / RU-adapted models beat raw English bases on efficiency and RU quality**

- **Vikhr** pipeline: tokenizer rebuild + CPT + SFT; full weights not LoRA-only.  
- **T-tech T-lite / T-pro**: Russian-oriented; T-lite-it-2.1 on Qwen3 architecture; T-pro 2.0 hybrid reasoning, Cyrillic-dense tokenizer (HF / paper arxiv:2512.10430). T-lite-it-1.0 card literally says designed for **further fine-tuning**, not as ready assistant.  
- **Qwen 2.5/3.x**: strong multilingual/RU among global open models (community consensus + RU collections).  
- **YandexGPT / GigaChat**: strong closed RU; **not** generally customer-exportable for local voice FT.  
- **Llama / Mistral / Gemma / Phi / OLMo**: trainable; RU weaker or less token-efficient than Qwen/T-tech/Vikhr-class unless CPT spends tokens “teaching Russian.”

### Size curve: 7–8B-in-voice vs 70B-generic  
**[emerging]**

| Use case | Likely winner |
|---|---|
| 80–200 word FB post, native register | **7–14B in his voice** |
| Long essay coherence, multi-hop reasoning, tool use | **Larger base + lighter style layer** or **big model + rewriter** |
| Facts he never wrote | **Neither** — need RAG; FT won’t invent accurate biography |

**Breakdown point of “small model in your voice”:** complex planning, math, code, multi-document synthesis, long-context consistency >~1–2k tokens of *new* reasoning.

### Comparison table — base models (voice tractability)

| Model family | Voice absorb (base) | Russian quality | Train VRAM (QLoRA 7–9B) | Serve (Q4–Q5) | Licence (typical) |
|---|---|---|---|---|---|
| **Qwen3 / Qwen2.5 7–14B base** | High | Strong multilingual | ~10–14 GB | ~6–10 GB | Qwen license (check commercial) |
| **T-lite (Qwen-based RU)** | High (made for further FT) | **Very strong RU** | ~10–14 GB | ~6–10 GB | Check HF card |
| **T-pro ~32B** | High if base available | Very strong | Multi-GPU / rent | ~20+ GB | Check HF card |
| **Vikhr / Vikhr-Nemo ~12B** | High (RU CPT pedigree) | Strong RU | ~14–16 GB tight | ~8–12 GB | Open (check) |
| **Llama 3.x 8B base** | High | OK→weak RU without CPT | ~10–14 GB | ~6–10 GB | Llama community |
| **Mistral / Ministral** | High | Medium | Fits 16 GB | Fits | Apache/Mistral |
| **Gemma 2/3** | Medium–high | Medium | Fits | Fits | Gemma ToU |
| **DeepSeek (open distill)** | Medium–high | Medium–good | Varies | Varies | Check |
| **GLM / Z.ai open** | Medium | Medium–good CN/EN; RU varies | Varies | Varies | Check |
| **Phi** | Medium (small) | Weak–medium | Easy | Easy | MS license |
| **OLMo** | High (research-friendly) | Weak RU | Fits | Fits | Apache-ish |
| **YandexGPT / GigaChat** | N/A externally | Top-tier RU | N/A | API only | Closed |

*VRAM numbers are approximate for QLoRA CPT at seq 2k–4k; full FT 7B needs roughly 4–8× more or multi-GPU.*

---

## 4. Q3 — Vendor fine-tuning services

| Vendor | Open to individuals? | Max dataset (reported) | CPT offered? | Weights exportable? | Cost shape | Data policy notes | Fit for 100–200 MB voice |
|---|---|---|---|---|---|---|---|
| **OpenAI** | **New users: platform winding down**; existing FT users still for a period | Token/example limits; examples often truncated (e.g. 65k ctx on 4.1) | **No** — SFT / DPO / RFT | **No** (hosted only) | Train $/M tokens × epochs; higher inference | Data used per policy; personal corpus risk | **Poor**: no CPT; FT deprecating; assistant attractor |
| **Google Vertex Gemini** | Yes (Cloud project) | **1 GB JSONL**; up to 10M text examples; rec. **100–500** examples for style | **No** — supervised LoRA SFT | **No** (endpoint) | Cloud billable tokens/job | GCP data processing terms | **Partial**: can ingest large *file* but method is SFT pairs, not raw CPT; voice fidelity loss if you must subsample into “examples” |
| **Anthropic Claude** | **No public self-serve FT** found in current docs for individuals | — | — | — | — | — | **Not available** for this plan [no public FT product found in opened sources] |
| **xAI / Grok** | **No public FT API** found | — | — | — | — | — | **No** |
| **Mistral** | Yes (API / La Plateforme) | Docs discuss large jobs; FAQ around **~500 MB** upload issues | SFT-style FT; not classic open CPT | Hosted; export depends on product | Token-based | Mistral terms | **Possible** for SFT/DPO on instruct models; not ideal for raw 200 MB CPT |
| **Together AI** | Yes | JSONL/Parquet; SFT + **DPO**; LoRA **or full** | Not marketed as DAPT; full FT approximates heavy adaptation | **Yes — download** | Train tokens + dedicated endpoint | Standard cloud | **Best cloud open path** if you don’t want to own GPUs |
| **Fireworks / Predibase / Modal** | Yes (varying) | Provider-specific | Usually LoRA/SFT | Often download adapters | Variable | Variable | Good engineering wrappers; same method limits as open FT |
| **Z.ai / GLM** | Subscription chat; enterprise FT unclear for individuals | — | — | — | — | — | Treat as **chat quota**, not voice-weight platform unless enterprise contract |

**Hard limit that kills fidelity:** forcing **100–200 MB → 500–5k chat examples** for vendor SFT. You throw away the **distributional mass** that CPT would use. Expect **surface style, not default idiolect**.

OpenAI explicitly: fine-tuning platform **no longer accessible to new users**; existing models remain until base deprecation.  
Google: supervised tuning, adapter sizes, 100–500 examples guidance; 1GB JSONL ceiling.  
Together: LoRA default, full FT option, SFT + preference (DPO), deploy or download.

---

## 5. Q4 — Data engineering for 100–200 MB one-author text

**[emerging] numbers; [established] hygiene principles**

### Pipeline (AK-47 order)

1. **Normalize** (encoding, newlines, strip UI chrome).  
2. **Exact dedup** (hash). Social decade corpora often **10–40% near-dup** (repost, edit, cross-post) — **[speculative range, no single paper on “all FB authors”]**; measure on *your* data.  
3. **Near-dup** (MinHash / simhash / embedding cluster). Keep newest or “most native voice” exemplar.  
4. **Authorship filter:** drop quotes >N chars, reposts, “shared via”, news paste.  
5. **Dialogue:** keep only **his** turns as LM targets; partner turns only as context (masked loss on partner tokens).  
6. **Chronology / target voice:** weight **2018–2023 FB** higher (e.g. sample weight 2–5×); downweight 2016 corporate email if register differs.  
7. **Upsample strongest-voice subset** (native FB) rather than diluting with business boilerplate.  
8. **Privacy scrub:** phones, emails, addresses, third-party secrets, financial IDs — **before** any cloud vendor.  
9. **Anti-collapse mix:** add **generic instruct** only at **SFT** stage (start ~**10%** of SFT tokens; adjust by eval). CPT can include **small English/RU general replay** if catastrophic forgetting appears (Vikhr-style motivation).  
10. **Packing:** pack posts into sequences of **2k–8k** with EOS between docs; don’t concatenate unrelated posts without separators.  
11. **Hold-out:** **≥5–10%** by **time** (e.g. last 3 months of 2023 never seen) for PPL + blind A/B.

### Training knobs practitioners report for style CPT  
**[emerging / mixed authority]**

| Knob | Starting point |
|---|---|
| Epochs | **1–2** over full CPT data; more → memorization |
| LR (LoRA) | ~**1e-4 to 5e-5**; embeddings **5–10× smaller** |
| Rank | **64–128** for serious style |
| Seq len | **2048–4096** first; longer later |
| Early stop | held-out author PPL + voice classifier, not train loss alone |

**Tokens needed to shift style measurably:** style CPT demos show **visible** shift with **very small** data (e.g. thousands of short stories in TinyStories-style notebooks) **[low-authority demos]**. Deep personal idiolect almost certainly needs **orders of magnitude more** — your **tens of millions of tokens** is a **feature**, not a bug, **if** used as CPT not as 2k SFT rows.

---

## 6. Q5 — Measurement before blind A/B

Build a **small harness** (one folder, one `eval.py`) — 0 drama.

| Metric | What it detects | Pass signal (illustrative) |
|---|---|---|
| **Held-out author PPL** (base vs tuned) | Distributional fit to him | Tuned PPL **clearly lower** on held-out *his* text |
| **Authorship classifier** (train on real him vs others/base gens) | Deep vs surface | Generations labeled **him** at rates close to real holdout |
| **Stylometry** (function words, punct, sentence length, Burrows’s Delta) | Surface–mid features | Distance to him ≪ distance to base gens |
| **MAUVE** (Pillutla et al., 2021) | Distributional gap human vs model gens | Higher MAUVE vs his holdout samples (https://arxiv.org/abs/2102.01454) |
| **LLM-as-judge rubric** | Fluency + “LLM-smooth” + humor timing | Blinded scores; use as **screening only** |
| **Capability suite** | Forgetting | Same RU instruct/reasoning suite **pre/post** |
| **Voice decay curve** | Reverts after N tokens | Score voice every 100 tokens in long gen |

### Failure modes early detection

1. **Surface mimicry:** stylometry up, authorship classifier **not** fooled, humans still say “AI”. → need more CPT / better data purity / DPO him-vs-base.  
2. **Capability collapse:** instruct benchmarks drop hard, tools break. → mix SFT, lower LR, fewer epochs, freeze more modules.

### Human blind A/B protocol  
**[established psychometrics practice applied here]**

- **Materials:** 30–50 items: half real held-out posts, half model gens on **matched prompts/topics**, length-matched (e.g. 80–150 words).  
- **Judges:** ≥5 people who know his writing **well** (not strangers).  
- **Task:** “real or generated?” forced choice + confidence.  
- **Pass bar (suggested):** accuracy **≤55–60%** (near chance 50%) **or** d-prime near 0; report CI.  
- **Controls:** include obvious LLM-smooth baselines so judges aren’t guessing randomly from fatigue.  
- **Separate** short-post A/B from long-form A/B — don’t average.

---

## 7. Q6 — Failure modes and the honest negative case

| Failure | Mechanism | Mitigation |
|---|---|---|
| **Verbatim private regurgitation** | Overfit / memorization | Dedup, ≤1–2 epochs, privacy scrub, canary strings in train, membership tests |
| **Can’t write outside corpus topics** | Topic–style entanglement | RAG for facts; don’t expect omniscient twin |
| **Instruction / tool collapse** | CPT without re-align | Light SFT + tool demos after CPT |
| **Reasoning drop** | Overwrite base circuits | KL/replay (Vikhr-style), smaller LR, shorter CPT |
| **Voice dies after ~200 tokens** | Weak adapter / instruct prior returns | Longer CPT, higher rank, DPO, decode with presence penalty tuned on eval |
| **Topic costume, not idiolect** | Model learns *what* he posts, not *how* | Hold-out **new** topics for A/B; upweight register-pure FB |

### Steelman: “fine-tuning is the wrong tool”

**Claim:** For personal writing, **retrieval of his own text + a strong general model + a disciplined rewrite prompt** captures most of what humans notice (lexicon, cadence, jokes), at **~5% of the cost** of a real CPT stack. Fine-tuning risks **privacy leaks**, **capability loss**, and a **false sense** of “digital twin.” Many “I trained on my chatlogs” blogs show **parlor-trick demos**, not double-blind longevity.

**When steelman is true:**  
- Need is **assisted drafting** with human in the loop.  
- Corpus is **dirty** (quotes, multi-author).  
- Principal can’t maintain eval/ops.  
- Goal is **knowledge** of his life facts (RAG wins).

**When steelman is false (your case):**  
- Goal is **default next-token = him** without style prompt.  
- Prompted transfer **already failed** in the specific way (surface markers, LLM-smooth core).  
- Corpus is **large, single-author, cleanable**.  
- Success metric is **blind A/B by people who know him**.

Then weights-level adaptation is not vanity — it is the **only** method class that matches the metric.

---

## 8. Q7 — Named end-to-end recipes

### Recipe A — “Cheapest that could work this week” (16 GB)  
**[emerging]**

| | |
|---|---|
| **Base** | `Qwen2.5-7B` or `Qwen3-8B` **base**, or `T-lite` base if available |
| **Data** | 2–5 GB cleaned subset: strongest FB voice only, privacy scrub, hold-out |
| **Method** | Unsloth **CPT** QLoRA r=64–128, all-linear + embed/lm_head lower LR, **1 epoch** |
| **Then** | Light SFT 1–3k (instruction → his post) with Unsloth/TRL |
| **Hardware** | RTX 5060 Ti 16 GB local |
| **Wall clock** | 1–3 days data + 0.5–2 days train (subset) |
| **$** | ~0–50 electricity; 0 cloud |
| **Serve** | Ollama / llama.cpp Q5 |
| **Tooling** | Unsloth, TRL, HuggingFace datasets |

**Expect:** measurable PPL drop + better short posts; **maybe not** full blind-pass yet.

### Recipe B — **Recommended production path** (local pilot → H100 finish)  

| | |
|---|---|
| **Base** | **T-lite / Qwen3-8B-base** (RU-first) |
| **Stages** | CPT full clean corpus (1–2 ep) → SFT (his-pairs + ~10% generic) → DPO (him vs base gens) |
| **Hardware** | Pilot on 16 GB; full CPT rent **1× H100 80GB** ~10–40 h |
| **$** | Rough **$50–400** train depending on hours/provider; serving local free |
| **Tooling** | Axolotl **or** LLaMA-Factory **or** Unsloth; eval harness custom; serve vLLM/Ollama |
| **Export** | Own weights — critical for privacy and AK-47 repair |

### Recipe C — Best quality, cost secondary  

| | |
|---|---|
| **Base** | **14–32B RU-strong base** (T-pro class / Qwen14–32B base) |
| **Method** | Full FT or high-rank LoRA CPT on multi-GPU; KL/replay vs base; SFT; multi-round DPO; optional rewriter for long-form |
| **Hardware** | 4–8× A100/H100 or Together **full FT** |
| **$** | **$1k–10k+** depending on size/epochs |
| **Tooling** | Axolotl, torchtune, Megatron-style if full; vLLM serve |

### Recipe D — Vendor-only (honest “not recommended for goal”)  

Gemini or OpenAI SFT on **1–20k** reverse-instruct pairs of best posts + DPO if available.  
**Use when:** need hosted API, accept **surface** style, cannot run GPUs.  
**Do not expect** default next-token twin.

### Public “I fine-tuned on my writing” reality check  
**[low-authority]**

- TinyStories/CPT demos (Unsloth/McCormick): **clear style shift** on simple domains after small CPT.  
- Blog “train on my messages” posts: often **anecdotal**, chatty output, **no blind protocol**.  
- LIMA-class: small curated SFT changes **alignment style**, not a specific human’s decade corpus.  
- Vikhr: serious **language** CPT, not personal author — but proves **CPT + SFT pipeline** works for RU distribution shift.

**Claim vs achieve:** public demos usually achieve **“recognizably more X-like”**, rarely **“friends fail forced-choice A/B.”** Your bar is higher → plan for **eval-driven iteration**, not one training run.

---

## 9. Named options A/B/C + single recommendation

| Option | What | Pros | Cons | Risk |
|---|---|---|---|---|
| **A. Prompt+RAG+rewriter** | Strong base + retrieved posts + small rewriter model | Fast, cheap, reversible | Not default-in-weights; same LLM-smooth risk | Low technical, high metric miss |
| **B. Local CPT→SFT→DPO on 7–9B RU base** | Unsloth/Axolotl, own weights | Matches goal; privacy; fits hardware ladder | Eng effort; capability tradeoffs | Medium |
| **C. Vendor SFT/DPO only** | OpenAI/Gemini/Mistral hosted | Easy UI | No CPT; no export; OpenAI FT dying; assistant prior | High goal-miss |
| **D. Max quality 32B+ full stack** | H100 farm | Best chance long-form | Cost, complexity vs AK-47 | Medium–high ops |

### **ONE recommendation: Option B**  
**Confidence: [emerging]**  
**Main assumption:** *Default distribution shift requires next-token training on the bulk corpus (CPT), not instruction SFT alone; a Russian-capable 7–9B base on consumer/H100 hybrid is enough for short-form blind A/B if data hygiene and eval are real.*

**Week-1 actions (engineer):**  
1. Build clean **author-only** dataset + time hold-out + privacy scrub.  
2. Freeze **eval harness** (PPL, stylometry, classifier, 20-item mini A/B).  
3. Run **Recipe A** on strongest-voice subset.  
4. If metrics move → Recipe B full corpus on rented H100.  
5. Only then invest in DPO.

---

## 10. What would flip the recommendation

| Evidence | Flip toward |
|---|---|
| Mini A/B: friends **already** fail to distinguish **RAG+rewriter** outputs | Stay on **A**, skip CPT |
| CPT 1 epoch: **no** PPL/classifier gain on hold-out | Data purity problem or need higher rank / full FT (**C/D**) |
| After CPT, instruct collapse unfixable at 8B | **Larger base + lighter style adapter**, or dual-model (reasoner + voice rewriter) |
| Open-weight RU 32B becomes trivial on 2×16 GB / cheap H100 | Prefer **D**-lite |
| Vendor launches **true CPT + weight export** with privacy terms you accept | Re-evaluate cloud |
| Blind A/B fails even after full B because judges key on **life facts** not style | Hybrid: voice model + RAG of biography |

---

## 11. Full source list (URLs actually used)

1. https://developers.openai.com/api/docs/guides/model-optimization — OpenAI evals/FT methods; platform winding down for new users; SFT/DPO/RFT table  
2. https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini-use-supervised-tuning — Gemini supervised tuning workflow  
3. https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/tuning/supervised-tuning — limits, adapter size, 100–500 examples guidance (Google docs, updated 2026-08)  
4. https://docs.mistral.ai/capabilities/finetuning/ — Mistral fine-tuning product  
5. https://docs.together.ai/docs/fine-tuning/overview — Together LoRA/full, SFT, preference  
6. https://docs.together.ai/docs/fine-tuning/quickstart — Together job lifecycle  
7. https://unsloth.ai/docs/basics/continued-pretraining — CPT definition, embed LR, LoRA targets  
8. https://mccormickml.com/2025/01/18/continuing-pre-training-on-raw-text/ — CPT vs SFT, base vs instruct, LoRA r=128 style example [low-authority / careful practitioner]  
9. https://arxiv.org/abs/2004.10964 — DAPT (Gururangan et al., 2020)  
10. https://arxiv.org/html/2405.13929v2 — Vikhr: RU CPT + SFT, tokenizer, forgetting  
11. https://arxiv.org/html/2402.01618v1 — Style vectors / activation steering for writing style  
12. https://www.anthropic.com/research/persona-vectors — Persona vectors (2025-08)  
13. https://arxiv.org/abs/2102.01454 — MAUVE  
14. https://krishnap25.github.io/mauve/ — MAUVE library notes  
15. https://arxiv.org/abs/2305.11206 — LIMA: Less Is More for Alignment  
16. https://huggingface.co/t-tech/T-lite-it-2.1 — T-lite Russian model card  
17. https://huggingface.co/t-tech/T-pro-it-1.0 — T-pro for further FT note  
18. https://huggingface.co/papers/2512.10430 — T-pro 2.0 paper page  
19. https://huggingface.co/collections/grib0ed0v/llm4russian — RU model collection  
20. https://magazine.sebastianraschka.com/p/practical-tips-for-finetuning-llms — LoRA rank/dataset tips (2023, still cited)  
21. https://sebastianraschka.com/faq/docs/base-vs-instruct-vs-reasoning-model.html — base vs instruct definitions  

**Claims without solid primary source in this pass** (marked in body): exact % of decade social media near-dup; quantitative “rank 8 never enough for idiolect”; Anthropic/xAI private enterprise FT existence; precise H100 hour costs for your token count.

---

### Confidence map (rollup)

| Finding | Tier |
|---|---|
| CPT/DAPT is the right primary tool for large unlabeled style/domain text | **[established]** |
| Personal decade-idiolect will pass strict blind A/B after CPT+SFT+DPO | **[emerging]** |
| Base ≫ heavily RLHF instruct for absorbing voice | **[emerging]** |
| Vendor SFT alone reaches “default him” | **[fringe]** for this metric |
| Activation steering alone equals weight-level twin | **[speculative]** |
| 7–9B RU model enough for short FB-native posts | **[emerging]** |
| 7–9B enough for long-form reasoning twin | **[speculative / unlikely]** |

---

**🎙 Mike:** You’re not shopping for “fine-tuning” as a product checkbox — you’re shopping for **who owns the next-token distribution**. Closed SFT vendors sell format glue; your metric needs **CPT on the bulk corpus + eval that can catch LLM-smooth fakes**. Do the cheap 16 GB pilot only if the harness is already built; otherwise you’ll train noise and declare defeat.

**🧒 Простыми словами:**  
Представь, что модель — это человек, который **привык** говорить «как умный ассистент». Если ты ему **только подсказываешь** «говори как Антон» — он **притворяется**. Чтобы он **сам по себе** говорил как ты, его надо **долго кормить твоими текстами** как обычной книгой (это CPT), а не только сотней примеров «вопрос–ответ». Готовые сервисы OpenAI/Google для этого **плохо подходят**: они не дают «дочитать всю твою жизнь как книгу» и **не отдают** модель домой. Лучший путь: **русская открытая модель 7–9B → дообучение на своих текстах у себя/на аренде GPU → проверка «друзья не отличают»**.
