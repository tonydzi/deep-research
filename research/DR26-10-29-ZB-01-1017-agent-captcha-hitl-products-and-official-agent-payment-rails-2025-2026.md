---
dr_id: DR26-10-29-ZB-01-1017
title: "Agent CAPTCHA/HITL products and official agent-payment rails, 2025-2026 delta"
date: 2026-10-29
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-29-ZB-01-1017): Agent CAPTCHA/HITL products and official agent-payment rails, 2025-2026 delta

> Which human-in-the-loop browser products and official agent-payment rails now exist for autonomous agents, what they cost, and what legal and ToS risk comes with CAPTCHA handling.

## Ключевые выводы
- All three vendor legs converge on one browser pattern: a persistent cloud browser that keeps session state, an explicit agent pause, a time-limited live view for an authorized human, then resume. Cloudflare Browser Run has the clearest first-party version. Its HITL docs (marked Beta) name MFA, CAPTCHA, sensitive input and order approval as handoff cases. Pricing is $0.09 per browser-hour after 10 free hours per month (ChatGPT leg).
- Cloudflare Agents docs (Grok leg) set operational limits. Live View URLs are bearer credentials, so anyone holding the link controls the browser. Links expire in about 5 minutes, an idle browser stays alive up to 10 minutes, and browserTool does not pause a run, so the handoff has to happen between turns.
- Browser vendors now sell CAPTCHA solving as an ordinary feature. Browserbase has it on from Developer ($20/mo, 100 browser-hours then $0.12/hr; Startup $99, $0.10/hr), on by default and switchable with solveCaptchas:false. Its own docs limit use to workflows the user and website owner authorize. Steel Cloud charges $3 per 1,000 solves on Launch and $1 on Scale, needs a $10 paid deposit, and advertises Turnstile. Anchor offers 'Automated captcha bypass' from Starter ($50/mo). A paid solver is a technical capability, not permission from the target site.
- LangGraph interrupt and Mastra suspend/resume are pause primitives only, with no browser or live view, so they must be paired with a browser provider. LangGraph resumes with the same thread_id plus Command(resume) and needs a checkpointer. It restarts the node from the top on resume, so any side effect placed before interrupt() runs twice. Mastra resumes with resumeData and can bail() on rejection.
- Anchor HITL is the most complete API-driven handoff among the browser vendors (Grok leg). With humanIntervention:true the agent pauses and an intervention.requested webhook delivers request_id, a message and a live_view_url. Pending requests can be listed and answered over the API, and intervention.resolved fires when done. Cookies and storage are preserved during the pause.
- Legal risk is five separate layers: contract with the CAPTCHA vendor, contract with the browser vendor, the target site's ToS, statute/civil suit, and platform enforcement. hCaptcha terms (updated 17 Nov 2025) forbid bots/AI passing challenges. In Meta v. Bright Data (N.D. Cal.) the court treated CAPTCHA bypass on logged-off public data as different from breaking a password gate, leaving only tortious interference. Ryanair v. Booking won JMOL on 22 Jan 2025 because CFAA loss was not shown. No opinion was found holding that CAPTCHA solving alone violates the CFAA. Cloudflare de-listed Perplexity's bot from its Verified bots list in 2025 for evading controls.
- The cleanest way to avoid CAPTCHA is to use an official API or agent identity rather than the consumer UI. Visa's 20 Oct 2026 BILL/Billtrust pilot says Trusted Agent Protocol authentication can satisfy the protections CAPTCHA historically provided. Cloudflare Web Bot Auth/Verified bots and Stripe Shared Payment Tokens are the other cited official rails.
- Payments are shifting from handing agents card credentials to cryptographically bounded intent and delegated tokens. Visa Intelligent Commerce uses TAP with passkey-approved payment instructions and is still in pilot. Mastercard Agent Pay (April 2025) uses Agentic Tokens, with Verifiable Intent co-developed with Google in 2026 and Agent Pay for Machines for machine-to-machine buying. The Santander/Mastercard European agent payment in March 2026 was explicitly not a commercial rollout. Google AP2 and OpenAI/Stripe ACP/SPT complete the set. The ChatGPT leg ranks PAN-bearing virtual cards as a legacy fallback after human approval.
- The vendor legs disagree on some details. Anchor's tiers differ (the ChatGPT leg lists a Team $500 tier and $1/credit; the Grok leg found no Team tier on the official pricing page). Browser Run pricing is found by ChatGPT but not by Grok. Agent Pay for Machines is dated June 2026 by ChatGPT and October 2026 by Gemini. Gemini's claim that reCAPTCHA, hCaptcha and Turnstile all explicitly prohibit solvers is only supported for hCaptcha.

## Рекомендации / решения
- Standardize one harness interface: a session-preserving browser, an explicit interrupt (LangGraph or Mastra), and an expiring live-view handoff to an authorized human. Use Cloudflare Browser Run as the reference design, with Browserbase, Steel or Anchor behind the same interface for economics or features.
- Keep automatic CAPTCHA solving disabled by default for third-party sites (solveCaptchas:false on Browserbase, Anchor captcha bypass off, no Steel Cloud solving). Enable it only where the site owner has explicitly authorized automation.
- Treat live-view URLs as secrets. Give them only to the human operator, never to the model, keep them short-lived, and put the pause at the start of the LangGraph node or make earlier work idempotent.
- Before driving a UI, check for an official API, Web Bot Auth/Verified bots, or Visa TAP, and prefer them over solving a challenge.
- For agent payments, prefer delegated-token and mandate rails (ACP/SPT, AP2, Visa/Mastercard network tokens) over durable card credentials. Use issuer virtual cards only as a legacy-web fallback after explicit human approval, with merchant and amount limits.
- Choose Steel self-hosted for open source and data control, Anchor for API-driven HITL webhooks, and Browserbase for observability. Treat Browser Use Cloud ($0.02/browser-hour, ChatGPT leg) as a cheap candidate with takeover only.
- Do not assume the new payment rails are generally available. Visa and Mastercard are still pilot or emerging, so re-check merchant acceptance before building on them.

## Сущности
- **Люди:** Sarah Perez (TechCrunch, reporter on Meta v. Bright Data)
- **Компании:** Cloudflare, Browserbase, Steel, Anchor Browser, Browser Use, Mastra, LangChain, Visa, Mastercard, Google, OpenAI, Stripe, Meta, Bright Data, Ryanair, Booking Holdings, hCaptcha / Intuition Machines, BILL, Billtrust, Santander, Perplexity, Adyen, Checkout.com, Fiserv, Braintree
- **Продукты/инструменты:** Cloudflare Browser Run, Cloudflare Agents Live View, Cloudflare Turnstile, Cloudflare Web Bot Auth / Verified bots, Browserbase Live View / solveCaptchas, Stagehand, Steel Cloud / Steel Browser, browser-handoff, Anchor humanIntervention, Browser Use Cloud, LangGraph interrupt, Mastra suspend/resume, Visa Intelligent Commerce, Visa Trusted Agent Protocol (TAP), Mastercard Agent Pay, Mastercard Agent Pay for Machines, Mastercard Verifiable Intent, Google AP2, Stripe Shared Payment Token, Agentic Commerce Protocol (ACP), reCAPTCHA, hCaptcha

## Открытые вопросы
- The supplied report text is truncated to about 20k of 44-54k characters per vendor leg. The AP2, OpenAI/Stripe ACP details, issuer virtual-card products and final synthesis were not visible and need checking in the full files.
- Browserbase's and Steel's own HITL/live-view documentation was not directly verified by the ChatGPT leg (marked single-source).
- Cloudflare Browser Run is Beta for HITL, and Grok found no price for it. Pricing details differ between legs.
- Anchor's Team $500 tier and per-credit pricing are unconfirmed on the official pricing page.
- Mastercard Agent Pay for Machines is dated June 2026 by one leg and October 2026 by another. Mastercard's October 2026 page and the OpenAI page were inaccessible to Grok.
- Whether hCaptcha or Turnstile terms bind a non-registered bot operator is untested, and no Turnstile or reCAPTCHA end-user contract text was found.
- Later disposition of Meta v. Bright Data and the X Corp. v. Bright Data docket were not confirmed, and no 2024-2026 opinion holding CAPTCHA solving alone is a CFAA violation was found.
- Hosted pricing for LangGraph Platform and Mastra Cloud was not found.
- Merchant acceptance and general availability of Visa and Mastercard agent rails through 2027 is an unverified assumption.

## Источник
- DR-ID `DR26-10-29-ZB-01-1017` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»

## Связано
- human-in-the-loop browser handoff
- CAPTCHA pause and resume
- agent payments
- delegated payment tokens
- agent identity / Web Bot Auth
- CFAA and ToS risk for scraping
- harness friction removal
- virtual cards for agents

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-02 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.

## Дополнение 02.11 (ноутбук, Claude Opus 5.5): что синтезатор не увидел
Синтезатор видел ~20 тыс. символов из 44-54 тыс. на ногу и отсеял ногу claude.ai. Добрано грепом по полным файлам `results/claudeai.md` и `results/chatgpt.md`:
- **Stripe Link Agent Wallet «spend requests»**: агент предлагает сумму, продавца и позиции, человек одобряет одним тапом в окне 30 минут, реквизиты одноразовые. Единственный найденный готовый паттерн «корзина → тап → карта на сумму». Источник: только нога claude.ai [single-source], перепроверить.
- **Privacy.com API**: `POST /v1/cards`, `type` ∈ `SINGLE_USE`, `MERCHANT_LOCKED`, `DIGITAL_WALLET`; `spend_limit` в центах; `spend_limit_duration` ∈ `TRANSACTION`, `MONTHLY`, `ANNUALLY`, `FOREVER`. Обе ноги: `SINGLE_USE` и `MERCHANT_LOCKED` взаимоисключающие типы; привязка к продавцу при первом списании (заранее не задать, уязвимо к инъекции). Песочница `sandbox.privacy.com/v1` подтверждена сторонними обёртками [low-authority].
- **Stripe Issuing**: программный выпуск, лимит на авторизацию, фильтры MCC/страна, тестовая среда; готовой связки «одноразовая + один продавец» нет [no source found], продавца проверяет свой вебхук авторизации.
- **Google AP2**: мандаты Intent / Cart / Payment; авторизация, не платёжная рельса.
- **ACP + Stripe Shared Payment Tokens**: токен ограничен продавцом, суммой, сроком; ресурсы `SharedPaymentIssuedToken`/`SharedPaymentGrantedToken` в changelog Stripe 22.04.2026.
- **Stripe «Order Intents»**: живого продукта под этим именем ни одна нога не нашла.
Документ-решение с тремя шорт-листами: скретчпад сессии ноутбук `decision-agent-unleash-3shortlists.md`.


## Check it yourself: prompt, sources, raw reports

- Prompt: [DR26-10-29-ZB-01-1017-agent-captcha-hitl-products-and-official-agent-payment-rails-2025-2026/prompt.md](DR26-10-29-ZB-01-1017-agent-captcha-hitl-products-and-official-agent-payment-rails-2025-2026/prompt.md)
- All sources (245 links): [DR26-10-29-ZB-01-1017-agent-captcha-hitl-products-and-official-agent-payment-rails-2025-2026/sources.md](DR26-10-29-ZB-01-1017-agent-captcha-hitl-products-and-official-agent-payment-rails-2025-2026/sources.md)

## Raw model reports

4 reports delivered, synthesis used 1 of 6. Note: 3 report(s) arrived after the synthesis was written; they are attached raw and were not reconciled. [DR26-10-29-ZB-01-1017-agent-captcha-hitl-products-and-official-agent-payment-rails-2025-2026/](DR26-10-29-ZB-01-1017-agent-captcha-hitl-products-and-official-agent-payment-rails-2025-2026/README.md)
