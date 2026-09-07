---
dr_id: DR26-08-04-ZB-01-1437
title: "DR26-08-04-ZB-01-1437 - synthesis: Chinese LLMs as a fourth pair of eyes alongside Grok and Gemini"
date: 2026-08-04
lang: en
source: Palo Alto AI Research Lab - deep research programme
translation_of: DR26-08-04-ZB-01-1437-dr26-08-04-zb-01-1437-sintez-kitayskie-llm-kak-4-ya-para-glaz-ryadom-s.md
---

# Synthesis DR26-08-04-ZB-01-1437 - a Chinese LLM as a fourth pair of eyes

> Evidence base: **three independent vendors**, two machines, and three separate research rails.
> `chatgpt` = Codex CLI headless (Mac16); `grok` = Grok CLI headless (Mac16); `gemini` = browser-based Deep Research (HP17, 122 sources).
> Original reports: `_originals/deep-research/DR26-08-04-ZB-01-1437-chinese-llm-landscape-2026-08-{chatgpt,grok,gemini}.md`
>
> This expands an earlier two-vendor synthesis with Gemini's third opinion and an analysis of disagreements. It does not replace the earlier snapshot.

## 1. Where all three vendors agreed

| # | Claim | Confidence |
|---|---|---|
| 1 | **DeepSeek V4-Pro is the first choice** for our use case. All three vendors independently ranked it first. | [established] |
| 2 | V4-Pro pricing: **$0.435 / 1M input tokens, $0.0036 / 1M cache-hit tokens, and $0.87 / 1M output tokens**, with a 1M-token context. Their shared primary source was DeepSeek's official pricing dated April 24, 2026. | [established] |
| 3 | DeepSeek provides a **native Anthropic-compatible endpoint**, `https://api.deepseek.com/anthropic`, making it a drop-in option for Claude Code through `ANTHROPIC_BASE_URL` and `ANTHROPIC_MODEL=deepseek-v4-pro[1m]`. It was the only Chinese provider for which the research confirmed a native Anthropic endpoint. Alibaba offers `/apps/anthropic` for Qwen; Z.ai uses an OpenAI-style API plus a helper. | [established] |
| 4 | **$25 is more than enough for PAYG usage** at our volume: approximately 28.7M output tokens or 57.5M input tokens. A subscription is less economical for this workload. | [established] |
| 5 | **The value is uncorrelated errors, not maximum Elo.** All three reports argued that DeepSeek and GLM offer a perspective less correlated with Grok and Gemini because of different pretraining data, a stronger mathematical and algorithmic bias, and different refusal behaviour. | [emerging] |
| 6 | **Do not choose:** Kimi K3 for this budget ($3/$15 would consume it in one or two sessions, and Moonshot had temporarily paused new subscriptions because of capacity constraints); Qwen3.7-Max as a daily model ($2.5/$7.5); 01.AI or Baichuan; or a direct mainland API for NDA-protected or client code. | [established] |
| 7 | **The nearest expiring fact:** DeepSeek announced a 2x peak-time multiplier. Its effective date must be checked weekly. | [emerging] |

## 2. Where the vendors disagreed

These are decision branches, not noise.

### A. Direct API vs OpenRouter

- **Gemini:** use **OpenRouter only**. It cited European Visa/Mastercard support without KYC, one key across several labs, and a clearer path around prompt-storage concerns.
- **Grok:** the first choice is the **direct** `api.deepseek.com/anthropic` endpoint because it is native and cheaper. It ranked OpenRouter third and noted the intermediary's own terms and an estimated 5.5% fee.
- **ChatGPT/Codex:** confirmed the direct endpoint, but found **no official guarantee** that ordinary EU cards would be accepted in every country.

This is a real disagreement. Grok optimises for price and native compatibility. Gemini optimises for payment and privacy certainty. ChatGPT confirms the exact uncertainty that Gemini's recommendation avoids.

### B. Coding-plan subscription vs pay as you go

- **Grok:** Z.ai GLM Coding Plan Lite, approximately $12.60-$18 per month, is the second-best option.
- **ChatGPT/Codex:** the same $18 plan does not publicly disclose its base allowance, so the buyer cannot price the real capacity in advance.
- **Gemini:** do **not** buy a coding plan for this use case. A headless agent can produce dozens of hidden micro-calls per minute; limits of 80-400 prompts per five hours can be exhausted in a single debugging session and stop the lab for five hours.

Gemini's operational objection is strongest for our workload, and it agrees with ChatGPT's PAYG arithmetic: $25 buys roughly 28.7M output tokens.

### C. Are DeepSeek V4 weights open?

- Gemini: yes, MIT.
- Grok: no.
- ChatGPT: not confirmed for V4.

Two of three did not confirm the claim. We therefore treat V4's open-weight status as **unconfirmed** and do not plan around self-hosting it.

### D. GLM-5.2 PAYG pricing: false corroboration

- Grok and Gemini both reported $1.40/$4.40.
- ChatGPT found that those numbers belonged to **GLM-5.1** on the captured pricing page; it could not find an official PAYG line for **5.2**.

This is a classic false-corroboration failure: two vendors agreed because both read the same page and transferred an older model's price. We treat GLM-5.2 pricing as unverified.

### E. MiniMax M3 has the weakest evidence

Gemini ranked it second at $0.30/$1.20 with a 50% OpenRouter promotion. Grok ranked it third through a $20 Token Plan Plus. ChatGPT said current M2.5/M3 prices were **not confirmed** and advised against using it as the primary review API.

It belongs in a third evaluation wave, not the first.

## 3. Recommendation

**Use DeepSeek V4-Pro on a pay-as-you-go basis as a fourth pair of eyes. Do not buy another subscription yet.**

One-evening test plan, capped at $15:

1. Start with **OpenRouter** using a $10-$15 top-up on a European card. This tests the payment rail that ChatGPT found was not guaranteed through DeepSeek directly.
2. In parallel, try a small top-up on the **direct** `api.deepseek.com/anthropic` endpoint. If the card works, prefer the direct route because it is cheaper and natively Anthropic-compatible.
3. Use a separate `ds-review.sh` environment. Do not change the primary Claude Code configuration.
4. Give it an adversarial review role: find bugs, missing tests, and places where the design can be simplified.
5. Enforce a $25 hard cap and log spending.

**Second wave, not now:** test GLM-5.2 only if DeepSeek agrees with Grok too often. The useful metric is not Elo; it is the rate of verified disagreements and unique findings.

**Shared safety boundary:** send only code diffs with secrets removed. Do not send the vault, CRM data, or client/NDA code to the Chinese-model rail. Direct-API prompts may be used for training by default.

No service was purchased or paid for as part of this research. The spending decision remains with the account owner.

## 4. Facts that need rechecking

| Fact | Recheck cadence |
|---|---|
| Effective date of DeepSeek's 2x peak-time multiplier | Weekly until announced |
| GLM Coding Plan allowance and GLM-5.2 PAYG price | In one to two weeks, or when the plan page changes |
| Whether DeepSeek accepts an EU card directly | At the moment of a small top-up attempt |
| Whether V4 weights are open | Monthly |
| DeepSeek restrictions in the EU and EU AI Act enforcement | Quarterly |

## Related concepts

`gemini-third-reviewer-rail` · `hub-review-rails` · `model-routing-fable-smart` · `prefer-included-limits-before-paid-api` · `false-corroboration-two-sources-agreeing` · `coding-outsource-external-implementers`
