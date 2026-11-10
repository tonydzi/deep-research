# DR26-10-29-ZB-01-1017 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
id: DR26-10-29-ZB-01-1017
rail: chatgpt
url: https://приватный чат
mode: ChatGPT Deep research (Extra High)
collected: 2026-11-02T12:42:26.032Z via Chrome (Claude-in-Chrome) ноутбук
unique_urls: 44
---

# Autonomous-Agent CAPTCHA Handling and Agent-Payment Rails: 2025–2026 Deep Research

**Research cut-off:** 2 November 2026  
**Scope:** Q2 CAPTCHA/HITL and Q3 agent payments only. Models, general agent architecture, external policy engines, and unrestricted payment-card autonomy are intentionally excluded.

The principal 2025–2026 change is that the market is bifurcating into two much cleaner architectures. For browser friction, the strongest pattern is no longer “make the agent look human”; it is **persistent cloud browser + explicit pause + live human takeover + resume**, with Cloudflare Browser Run providing the clearest first-party implementation and Browserbase/Steel/Anchor supplying broader browser infrastructure. For payments, the strategic shift is away from handing agents durable card credentials and toward **cryptographically bounded intent, delegated payment tokens, network tokens and merchant/amount constraints** through Visa Intelligent Commerce, Mastercard Agent Pay, AP2 and OpenAI/Stripe’s commerce stack.

The important legal distinction is equally clear: **CAPTCHA-solving capability is not authorization**. A vendor can technically solve a challenge while the target site can still terminate accounts, block traffic, allege breach of contract, or pursue civil claims. Recent U.S. cases continue to distinguish public/logged-off access from authenticated or otherwise access-controlled areas, but none creates a general legal right to defeat anti-bot measures.

## Executive assessment

| Area | Best current pattern | Confidence | Critical assumption |
|---|---|---:|---|
| CAPTCHA / browser challenges | Use a browser service that preserves the exact session, pauses the agent, exposes a time-limited live view to an authorized human, then resumes. Prefer official APIs instead of UI automation whenever available. | **[established]** | The human is authorized to perform the underlying action and the site's terms do not prohibit the surrounding automation. |
| Agent payments | Prefer merchant/network-native delegated-payment rails — ACP/SPT, AP2-style mandates, Visa/Mastercard network tokens — over minting ordinary PAN-bearing virtual cards. Use issuer virtual cards only as a legacy-web fallback after explicit human approval. | **[emerging]** | Merchant acceptance and production availability of agent-native rails continue expanding through 2027. |

A useful risk hierarchy is therefore:

**official API / agent-commerce endpoint → authorized browser + human takeover → ordinary browser automation without bypass → vendor-advertised automated CAPTCHA solving → deliberate evasion after revocation or authenticated-access controls.**

The steps to circumvent a particular site's defenses are intentionally omitted.

## CAPTCHA and human-in-the-loop browser products

### Product comparison

| Product | 2026 capability and integration surface | Price observed | CAPTCHA / pause-resume behavior | Maturity and risk | Recommendation |
|---|---|---|---|---|---|
| **Cloudflare Browser Run** | Browser sessions accessible through browser automation/CDP; Live View exposes the running browser to an operator. Its structured handoff API lets the application generate a live-view handoff and determine when the operator has completed the intervention. Cloudflare explicitly documents MFA, CAPTCHA, sensitive-data entry and order approval as handoff cases. Quote: **“hand off control to a human operator.”** https://developers.cloudflare.com/browser-run/features/human-in-the-loop/ citeturn892405view1 | Free allowance: **10 browser minutes/day**. Paid Browser Run includes **10 browser-hours/month**, then **$0.09/hour**; included session concurrency is separately metered beyond allowance. Quote: **“$0.09 per browser hour.”** https://developers.cloudflare.com/browser-run/pricing/ citeturn463851view0 | Native HITL, not CAPTCHA bypass. Operator works in the same browser state and the automation resumes afterward. | **[established]** implementation; HITL feature was marked Beta in the reviewed documentation. Low inherent legal risk when used as human handoff; target-site ToS still governs the automation. | **Best default for challenge-sensitive workflows** where preserving session state and making the human intervention explicit matters more than anti-detection features. |
| **Browserbase** | Managed browsers supporting Playwright, Puppeteer, Selenium and Stagehand; HTTP/API-based orchestration and enterprise private-cloud/VPC options are advertised. Quote: **“Playwright, Puppeteer, Selenium”**. https://www.browserbase.com/pricing citeturn784146view2 | Free: 3 browsers/1 browser-hour. Developer: **$20/mo**, 100 browser-hours then **$0.12/hour**. Startup: **$99/mo**, 500 hours then **$0.10/hour**. Enterprise custom. https://www.browserbase.com/pricing citeturn784146view2 | Paid tiers explicitly advertise **“automatic captcha solving.”** Browserbase also has a Live View/human-handoff architecture in its product ecosystem, but I did not complete a direct retrieval of the HITL documentation page during this research session. **[single-source]** | Browser platform itself is mature. Automated CAPTCHA solving materially increases ToS/enforcement risk on third-party sites. | Good cloud browser infrastructure; **disable automated challenge solving for sites where authorization is unclear and use takeover instead.** |
| **Steel** | Open-source browser infrastructure with hosted service, concurrent sessions, proxies and browser automation surface. Quote: **“Built in the open on GitHub.”** https://steel.dev/pricing citeturn858184view1 | Launch: **$0 base + usage**, $0.10 browser-hour, proxies $10/GB, CAPTCHA solving $3/1,000. Scale: **$250/mo + usage**, $0.08/hour, proxy $6/GB, CAPTCHA $1/1,000. https://steel.dev/pricing citeturn858184view1 | The commercial pricing explicitly meters CAPTCHA solving. Steel also documents human-in-loop/live-session functionality, but the machine-readable HITL page did not render successfully in the final source pass. **[single-source]** | Strong open-source/self-hosting appeal. Same legal caveat: paid CAPTCHA capability is a technical feature, not permission from the target. | **Best fit if self-hosting/open source is important**; configure the product around operator takeover rather than automatic solving. |
| **Anchor Browser** | Cloud browser/embedded-browser-agent service; SDK and MCP integration plus browser sessions and authenticated-browser functionality. Quote: **“SDK”**, **“MCP.”** https://anchorbrowser.io/ citeturn892405view0 | Free: 5 credits/month. Starter: **$50/mo + $1/credit**. Team: **$500/mo**. Growth: **$2,000/mo**. Enterprise custom. https://anchorbrowser.io/ citeturn858184view0 | Starter explicitly advertises **“Automated Captcha bypass”**; higher tiers add larger concurrency and, at Team, Cloudflare Verified Browser Agents. https://anchorbrowser.io/ citeturn892405view0 | **[emerging]**. The product positioning is unusually aggressive around CAPTCHA bypass; that makes legal/contract review more important, not less. | Attractive for controlled first-party sites and sanctioned automation; **not my default for arbitrary third-party challenge bypass.** |
| **Mastra** | Agent framework rather than browser provider. Its HITL primitives suspend an execution and later resume it, including explicit tool approval/decline. Quote: **“suspend()”**, **“resumeGenerate(runId)”**. https://mastra.ai/docs/agents/human-in-the-loop citeturn858184view5 | OSS framework: **$0 / Apache-2.0**. Hosted Starter $0; Teams **$250/mo**; enterprise custom. https://mastra.ai/pricing citeturn858184view4 | Excellent orchestration primitive, but it does **not itself provide the browser live view**. Pair it with Cloudflare, Browserbase, Steel or another browser session provider. | **[established]** as an orchestration pattern. Low direct CAPTCHA risk because challenge handling is delegated to the attached browser/human channel. | Strong choice if your runtime already uses Mastra; keep the live browser outside the framework. |
| **LangGraph interrupt** | LangGraph's HITL model persists graph state and interrupts execution until an external input resumes it. The reviewed canonical documentation URL redirected during research, so exact current hosted-product pricing and API semantics were not independently reconstructed. https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/ citeturn858184view6 | OSS framework; hosted LangGraph/LangSmith commercial pricing not re-verified here. **[no source found]** | Same architectural class as Mastra: excellent pause/resume primitive, but browser takeover/live-view must come from another service. | **[established]** architectural pattern; exact 2026 hosted-service details are an evidence gap in this report. | Use it as the **workflow checkpoint**, not as the CAPTCHA solution. |
| **Browser Use Cloud** | Managed Chromium exposed through API/CDP, with a `liveUrl` that lets a person observe or take over a browser. Quote: **“watch or take over.”** https://browser-use.com/cloud-browser citeturn463851view3 | Browser infrastructure advertised at **$0.02/browser-hour** plus **$5/GB residential proxy**; agent execution adds model cost/markup. https://browser-use.com/pricing citeturn463851view4 | Vendor advertises built-in CAPTCHA and stealth alongside takeover. **[low-authority]** because these are vendor marketing claims rather than independent technical measurements. | **[emerging]** and unusually inexpensive on published browser-hour price. | Worth evaluating as a low-cost session provider, but use human takeover for third-party challenges. |

### What changed in 2025–2026

The meaningful delta is that **HITL is becoming a first-class browser primitive rather than an application-level workaround**. Cloudflare's 2026 Browser Run documentation is the strongest example: the session itself can expose a controlled live view, hand authority to an operator, and then return control to automation. citeturn892405view1turn463851view0 Mastra and LangGraph make the corresponding control-flow checkpoint explicit at the agent-runtime layer, while Browserbase, Steel, Anchor and Browser Use provide persistent remote browser state.

Conversely, several browser vendors now commercialize CAPTCHA solving as an ordinary line item. Browserbase includes automatic solving on paid tiers; Steel charges per thousand challenges; Anchor explicitly advertises “Automated Captcha bypass”; Browser Use advertises built-in CAPTCHA handling. citeturn784146view2turn858184view1turn892405view0turn463851view3 **That is a product capability claim, not a legal safe harbor. [established]**

### Legal, contractual and enforcement risk

The legal landscape needs five separate buckets rather than one “legal/illegal” label.

**CFAA exposure.** Recent federal decisions continue to make the **public versus authenticated/access-controlled boundary** important. In *Meta Platforms v. Bright Data*, the Northern District of California found no disputed evidence that Bright Data had scraped non-public data while logged in and framed the remaining contract question around logged-off scraping of publicly available information. The opinion calls the distinction between public access and scraping “behind a log-in screen” **“pivotal.”** https://www.govinfo.gov/content/pkg/USCOURTS-cand-3_23-cv-00077/pdf/USCOURTS-cand-3_23-cv-00077-7.pdf citeturn346915view2turn879813view0 The decision was a contract case and **does not establish a general entitlement to evade anti-bot controls**.

*Ryanair DAC v. Booking Holdings/Booking.com* is even more cautionary. The 2024 litigation dealt directly with automated booking/scraping and Ryanair's “Shield” anti-bot system; after a jury verdict, the district court granted judgment as a matter of law on 22 January 2025 because Ryanair had not established the CFAA's statutory loss requirement. Quote: **“Motion for Judgment as a Matter of Law … is GRANTED.”** https://www.govinfo.gov/app/details/USCOURTS-ded-1_20-cv-01191/USCOURTS-ded-1_20-cv-01191-5 citeturn346915view0 That outcome is **not** a holding that circumventing Shield was categorically lawful. The safer reading is that CFAA liability is fact- and element-specific, particularly around access authorization and qualifying technological loss. **[established]**

**Contract / ToS exposure.** This can be easier for a platform to assert than CFAA liability because it turns on assent, the text of the terms and the activity. *Bright Data* itself illustrates this: the court separately analyzed whether Meta's terms contractually covered logged-off collection rather than assuming anti-scraping terms automatically followed the defendant everywhere. citeturn346915view1turn346915view2 Some CAPTCHA providers are much more explicit: the hCaptcha terms reviewed during this research prohibit using bots/scripts/AI to perform challenges. **[single-source]** The corresponding current URL was https://www.hcaptcha.com/terms. Google's general service terms also prohibit bypassing protective measures; however, I did **not** locate a comparably explicit reCAPTCHA end-user provision establishing that every third-party solver user has a direct contractual relationship with Google. **[no source found]**

For **Cloudflare Turnstile**, I likewise did not find an authoritative provision establishing that a visitor to an unrelated site necessarily enters a direct Turnstile contract with Cloudflare. **[no source found]** The more immediate contractual risk is normally the **target site's own terms and account agreement**.

**Civil liability beyond contract.** Depending on facts, plaintiffs may plead interference, trespass-type theories, misuse of credentials or other claims in addition to CFAA/contract. *Meta v. Bright Data* itself included tortious-interference allegations. citeturn346915view2 Whether such claims survive is jurisdiction- and fact-specific; no general “CAPTCHA case” rule resolves them. **[established]**

**Platform enforcement.** Account closure, session invalidation, IP/rate-limit enforcement, fraud review and blocking generally require far less than a court judgment. A CAPTCHA-solving vendor cannot immunize you from those measures. **[established as operational inference]**

**Reputation/commercial risk.** Deliberately automating a site after its operator has unambiguously denied permission is qualitatively different from automating your own account through a normal browser flow and handing a challenge to the authorized human. The former creates materially worse vendor, partner and litigation optics even where the CFAA claim itself is uncertain. **[established as risk assessment]**

### Official no-CAPTCHA path

The clean architectural principle is: **if the service has an authenticated API for the transaction, use it instead of driving the consumer UI.** The API may have OAuth, rate limits, fraud scoring or explicit agent policies, but the webpage CAPTCHA normally disappears from the execution path.

Concrete examples include Shopify's Storefront/API ecosystem for catalog/cart operations, GitHub's REST/GraphQL APIs for repository/account operations, Stripe's API for payment/business operations, and Google/Slack APIs for calendar/workspace actions. **The exact 2026 API scopes for those examples were not individually re-opened in the final source pass, so treat this example set as [single-source]/[no source found] rather than part of the core evidentiary set.** The important design rule is stronger than any specific example: **a CAPTCHA is a signal that your browser is using an interface designed for interactive humans; before solving it, check whether the provider has a supported machine interface or agent program.**

**Overall Q2 recommendation — [established], high confidence:** standardize on **session-preserving browser + explicit interrupt + expiring live-view handoff**, with Cloudflare Browser Run currently the cleanest integrated reference design. Put Browserbase/Steel/Browser Use behind the same interface if you need different economics or browser features. Treat every automatic CAPTCHA solver as **disabled-by-default for third-party sites** unless the site has affirmatively authorized automation. The critical assumption is that your operator is legitimately entitled to perform the underlying transaction.

## Agent-payment rails and the 2025–2026 delta

The central 2025–2026 change is that payment networks, Google, OpenAI and Stripe are moving the **authorization object** outside the language model. Instead of an agent merely deciding “buy this,” the ecosystem is defining signed or tokenized artifacts expressing **who may buy, from whom, for how much, and for how long**.

### Visa Intelligent Commerce

Visa Intelligent Commerce exposes infrastructure for registering trusted agents, tokenizing cards and binding a consumer's payment instruction to the eventual authorization. Visa's developer material describes an agent-specific token flow and passkey-authenticated payment instructions; the eventual payment authorization is checked against the consumer's approved merchant/amount intent. Quote: **“authenticate the Payment Instruction with a passkey.”**  
https://developer.visa.com/capabilities/visa-intelligent-commerce **[single-source]**

As of the research date, this is **[emerging] rather than universally GA**. Visa described the capability during 2025 as being developed/deployed, and on 20 October 2026 announced a live pilot involving BILL and Billtrust in which trusted agents use tokenized credentials and approved payment instructions. Quote: **“first live pilot.”**  
https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-bill-billtrust-launch-pilot.html **[low-authority]** because this status evidence is a Visa corporate announcement.

The human-approval model is strongest when the consumer explicitly approves an instruction with a passkey; the token then prevents the agent from freely substituting a different merchant/amount. Visa's Trusted Agent Protocol work also addresses merchant-side recognition of authorized agents, which is strategically important because it turns “bot or human?” into “is this a network-recognized authorized agent?” **[emerging]**

**Recommendation:** very strong long-term rail for card-network commerce; do not assume general merchant availability yet.

### Mastercard Agent Pay and Agent Pay for Machines

Mastercard announced Agent Pay in April 2025 around **Agentic Tokens**, network tokenization, registered/verified agents and consumer authentication. Quote: **“Agentic Tokens.”**  
https://www.mastercard.com/news/press/2025/april/mastercard-unveils-agent-pay-pioneering-agentic-payments-technology-to-power-commerce-in-the-age-of-ai **[single-source]**

In 2026 Mastercard expanded the model with **Verifiable Intent**, co-developed with Google, creating a cryptographically verifiable record of the user's intended transaction and aligning with broader agent-commerce protocols. Mastercard described it as a **“tamper-resistant record”** of user intent.  
https://www.mastercard.com/content/mccom/eu/en/news-and-trends/stories/2026/verifiable-intent.html **[low-authority]**

Mastercard's June 2026 **Agent Pay for Machines** targets autonomous machine-to-machine purchasing rather than ordinary consumer checkout. The network describes registered agent identities, organizational authorization rules/spending limits and multi-rail settlement.  
https://www.mastercard.com/au/en/news-and-trends/press/2026/june/mastercard-launches-agent-pay-for-machines.html **[low-authority]**

The distinction matters: Agent Pay for Machines is not evidence that a consumer browser agent can today obtain unrestricted Mastercard purchasing authority. It is an enterprise/M2M architecture with administratively bounded authority. **[emerging]**

**Recommendation:** track closely for enterprise procurement/M2M; for consumer cart checkout, ordinary Agent Pay/Verifiable Intent is the more relevant branch.

### Google Agent Payments Protocol

AP2 is the most conceptually clean protocol in the set because it makes **mandates** explicit rather than treating the agent conversation itself as authorization. The current specification distinguishes human-present and human-not-present flows: a user can sign a closed mandate for a specific purchase, while unattended flows start from constraints the user has authorized and result in a narrower transaction-specific artifact. Current work also uses cryptographically verifiable credentials such as SD-JWT/FIDO-associated mechanisms.  
https://ap2-protocol.org/  
https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md **[single-source]**

The important 2026 delta is that AP2 is no longer merely “a human clicks yes for every transaction.” Its model explicitly accommodates **standing constraints** — for example merchant/category/amount bounds — followed by a cryptographically attributable transaction mandate. **[emerging]**

AP2 is a protocol, not a universal acquiring network. Supporting AP2 does not itself make every merchant or issuer accept an agent-originated transaction. That distinction is often blurred in launch coverage. **[established]**

**Recommendation:** best protocol model for expressing bounded authority, but pair it with an actual payment network/PSP acceptance path.

### OpenAI / Stripe Agentic Commerce and delegated payments

OpenAI's Agentic Commerce Protocol architecture separates **order orchestration** from **payment authority**. The current key-concepts material describes a delegated-payment object with a **maximum chargeable amount and expiration**, with the payment service provider returning a narrowly scoped token rather than handing the agent reusable card credentials. Stripe Shared Payment Tokens are the first compatible implementation identified in the reviewed material. Quote: **“max chargeable amount.”**  
https://developers.openai.com/commerce/guides/key-concepts **[single-source]**

OpenAI's September 2025 launch material emphasized that the user explicitly confirms the transaction and that encrypted payment tokens are scoped to the particular merchant/amount; the merchant remains the merchant of record. Quote: **“users explicitly confirm each step.”**  
https://openai.com/index/buy-it-in-chatgpt/?faq=5 **[low-authority]**

Stripe's subsequent Agentic Commerce Suite expanded the same design direction around merchant-scoped/time-scoped/amount-scoped Shared Payment Tokens and Stripe Radar risk signals.  
https://stripe.com/newsroom/news/agentic-commerce-suite **[low-authority]**

**Order Intents require an important qualification.** Stripe's 2025 product keynote showed an Order Intents API concept where an agent could identify a merchant page, construct an order and confirm the transaction.  
https://stripe.com/mx/sessions/2025/product-keynote **[low-authority]**

I did **not** find a current 2 November 2026 public production API reference establishing that “Order Intents” remains a separately GA developer product under that exact name. **[no source found]** The public architecture that is clearly documented now is ACP/delegated payments/SPTs. It would therefore be unsafe to design a new system around the 2025 Order Intents demo without confirming private/partner availability with Stripe.

### Rail comparison

| Rail | Authority object | Human approval | Cloud integration | Launch status at research cut-off | Price |
|---|---|---|---|---|---|
| Visa Intelligent Commerce | Network token + authenticated payment instruction | Passkey / explicit payment instruction; bounded merchant/amount | Visa developer APIs; agent registration/tokenization/payment-intent services | **[emerging]** live pilots in 2026 | Commercial/network pricing not publicly established in reviewed material **[no source found]** |
| Mastercard Agent Pay | Agentic Token + registered agent identity; Verifiable Intent adds cryptographic intent | Consumer-authenticated intent; enterprise limits for machine variant | Mastercard/network integrations and partner APIs | **[emerging]** announced 2025, expanded 2026 | **[no source found]** |
| Google AP2 | Signed mandate / verifiable credential | Transaction-specific mandate or previously approved constraint mandate | Open protocol/spec; actual processor/network integration required separately | **[emerging]**, open protocol | Protocol itself open; payment-provider fees separate |
| OpenAI ACP + Stripe SPT | Delegated token scoped by merchant, amount and expiry | User confirmation before delegation | ACP merchant/API integration + compatible PSP | **[emerging]**, production restricted to supported/approved commerce integrations | ACP protocol no separate public protocol fee; PSP/Stripe economics apply |
| Stripe Order Intents | 2025 demo/API concept | Human confirmation demonstrated | Stripe API concept | **[speculative as a current standalone product]** | **[no source found]** |

**Overall network-rail recommendation — [emerging], medium-high confidence:** make your internal purchase authorization object look more like **AP2/ACP/Visa payment instructions** than like “LLM has card.” Prefer amount-, merchant- and time-scoped delegated credentials whenever the merchant supports them. The most important assumption is that merchant acceptance will broaden enough to cover a meaningful share of your spend.

## Virtual-card APIs and the exact-amount purchase pattern

### Privacy.com

Privacy has unusually explicit public API semantics. `POST /cards` supports a card `type` including **`SINGLE_USE`** or **`MERCHANT_LOCKED`**, plus `spend_limit` and `spend_limit_duration`; the latter includes **`TRANSACTION`**, monthly, annual and forever-style limits.  
https://developers.privacy.com/docs/cards citeturn726012view0

A subtle but important limitation is visible directly in that schema: **SINGLE_USE and MERCHANT_LOCKED are alternative card types**, not two independent flags. A merchant-locked card binds to the first merchant that successfully authorizes it; it is therefore closer to trust-on-first-use than to “pre-bind this PAN to Merchant ID X.” **[established inference from API schema]**

Consumer pricing reviewed on 2 November 2026 was Free $0/12 cards monthly, Plus **$5/mo**/24 cards, Pro **$10/mo**/36 and Premium **$25/mo**/60. API access is offered on paid plans.  
https://www.privacy.com/pricing citeturn726012view1

Privacy also exposes a Sandbox; its support documentation warns that API keys have broad account authority and that the customer remains responsible for financial activity.  
https://support.privacy.com/hc/en-us/articles/12036411849495-What-is-the-Privacy-API citeturn726012view2

**Native fit for your pattern:** good for **single-use + exact transaction cap**; good separately for **merchant-locked + cap**; **not a native “pre-specified merchant + single-use + exact amount” primitive.**

### Stripe Issuing

Stripe Issuing supports programmatic creation of virtual cards and issuer-side spending controls, including per-authorization limits and merchant-category/country restrictions. The API surface is mature and has a genuine test environment.  
https://docs.stripe.com/api/issuing/cards citeturn726012view3  
https://docs.stripe.com/issuing/testing citeturn726012view4

A key gap for the exact requested primitive: I found **no documented first-class Stripe Issuing control that pre-locks a card to one specific merchant identifier while simultaneously making it single-use. [no source found]** Real-time authorization controls can implement merchant-specific logic, but that is an external policy decision rather than a static card primitive.

Stripe publishes Issuing pricing, but I did not extract a sufficiently reliable current 2026 per-card/per-authorization figure from the opened pricing page, so I will not reproduce a potentially stale number.  
https://stripe.com/pricing citeturn726012view5

**Native fit:** strong for **programmatic virtual-card issuance + hard per-authorization amount**, true sandbox and real-time authorization; weaker if your requirement literally means an immutable card-level single-merchant lock.

### Lithic

Lithic has one of the strongest developer environments: its Sandbox is a separate environment with no real-money movement. Quote: **“Sandbox … doesn't transact real money.”**  
https://docs.lithic.com/docs/api-basics citeturn382103view0

Its spending limits support transaction-based constraints, but Lithic explicitly warns that spend limits should not be treated as a perfect ledger balance: authorizations/clearing and force-post cases can exceed simplistic expectations, and limit state may be eventually consistent.  
https://docs.lithic.com/docs/spend-limits citeturn382103view1

The 2026 architectural delta is significant: Lithic is steering more control logic toward **Authorization Rules v2** rather than relying on legacy account-level limits. Rules can use merchant descriptors, amount, MCC and risk signals. The documentation also exposes an important merchant-lock nuance: the `MERCHANT_LOCK` rule is described as Lithic-managed rather than a normal rule that a customer can freely create/update through the public API.  
https://docs.lithic.com/docs/authorization-rules-v2 citeturn382103view2

Public transaction/card pricing was not established from the reviewed materials. **[no source found]**

**Native fit:** excellent issuer processor for sophisticated authorization policy; less compelling if you need a trivially exposed, public-API “mint one exact merchant-locked card” operation.

### Ramp

Ramp's core card/expense product is advertised as free, with paid Ramp Plus functionality layered on top.  
https://support.ramp.com/hc/en-us/articles/360043075754-Ramp-pricing-overview citeturn382103view5

Ramp's 2026 agent-payment direction is particularly relevant because it is not merely “give an agent a company card.” Its Agent Cards material describes connecting agent identity/user permissions to funds, spend limits, merchant rules and approval policies; separate Vault/virtual-card APIs can issue card credentials programmatically. **[single-source]** The corresponding developer documentation URLs identified during research were:

`https://docs.ramp.com/llms-guides/agent-payments.txt`  
`https://docs.ramp.com/llms-guides/virtual-cards-embedded.txt`

The ordinary rendered docs page was a JavaScript shell in the final retrieval pass, so I am treating the detailed API semantics here as **[single-source]**, not equivalently verified to the Privacy/Lithic material.

Ramp is strategically closer than a generic issuer to the requested **agent + approval-policy** abstraction, but I did not find definitive public documentation proving a native atomic operation that means: **human taps once → immutable card is minted for exactly amount X and pre-bound to merchant Y → card dies after that purchase. [no source found]**

### Which product actually supports the desired flow?

The exact requested primitive is stricter than most marketing descriptions:

> agent constructs cart → human sees merchant + exact total → human approves → credential becomes usable only for that merchant and amount → credential cannot be reused elsewhere.

The strongest solutions are **not conventional virtual cards**.

| Option | Exact amount | Merchant-specific | One-use / expiry | Human authorization native | Verdict |
|---|---:|---:|---:|---:|---|
| **ACP + Stripe Shared Payment Token** | Yes | Yes | Narrowly scoped / expires | Yes | **Closest native fit where merchant supports it. [emerging]** |
| **Visa Intelligent Commerce** | Yes, payment-instruction model | Yes/intended merchant | Network-token/instruction scoped | Passkey | **Architecturally excellent; rollout still emerging.** |
| **AP2 mandate + compatible payment rail** | Yes | Can encode merchant/constraints | Mandate-scoped | Cryptographic user mandate | **Best protocol abstraction, but not itself a card issuer.** |
| **Privacy SINGLE_USE + TRANSACTION limit** | Yes | No pre-bound merchant | Yes | No; your app supplies approval | **Good legacy-web fallback.** |
| **Privacy MERCHANT_LOCKED** | Yes | Locks after first successful merchant | Not simultaneously SINGLE_USE in the public schema | No | **Useful, but TOFU merchant lock.** |
| **Stripe Issuing virtual card** | Yes, via issuer controls | No static merchant-ID lock found | Can be closed after purchase externally | Your app supplies approval | **Good fallback, not exact native primitive.** |
| **Lithic** | Strong amount/auth rules | Rich merchant logic; public `MERCHANT_LOCK` creation constrained | Programmable lifecycle | Your app supplies approval | **Best if you operate issuer-grade infrastructure.** |
| **Ramp Agent Cards** | Limits supported | Merchant rules advertised | Card lifecycle controllable | Approval policies integrated | **Very promising; exact atomic primitive not proven in reviewed docs.** |

The security advantage of ACP/AP2/Visa-style credentials is structural. A prompt injection saying “buy a €4,000 gift card instead” cannot by itself expand a token whose cryptographic/processor scope says **merchant A, ≤€132.47, expires in five minutes**. With a conventional virtual card, comparable safety requires the issuer and your external policy engine to enforce those restrictions, and merchant identity is usually the hardest part to make immutable.

Prompt injection still matters before approval: an agent can misrepresent the cart, substitute a merchant, hide shipping/recurring charges, or manipulate the approval UI. The approval surface should therefore be generated from **trusted merchant/payment data**, not merely the LLM's prose summary. This is an architectural inference rather than a vendor-specific guarantee. **[established]**

## Recommendations, evidence gaps and proof of work

### Shortlists

For **Q2 CAPTCHA/HITL**, my shortlist is **Cloudflare Browser Run first, Browserbase second, Steel third**. Cloudflare wins because its documented primitive directly matches the desired security posture: hand off the same live session for CAPTCHA/MFA/sensitive approval, then resume. Browserbase is a stronger general-purpose browser cloud, while Steel is the best of the three if open source/self-hosting matters. Mastra or LangGraph should sit above any of them as the orchestration interrupt. **Do not treat Anchor/Browserbase/Steel/Browser Use CAPTCHA-solving features as permission to cross a site's anti-bot boundary.**

For **Q3 payments**, the preferred hierarchy is **ACP/SPT or Visa/AP2-native delegated authorization where supported → Ramp Agent Cards/issuer controls for managed business spending → Stripe Issuing/Lithic/Privacy virtual cards as compatibility fallback**. A virtual PAN should be an implementation detail of last resort, not the object the LLM is entrusted to control.

### Important gaps

Several limitations materially affect confidence. No current public Stripe documentation was found establishing that **Order Intents** remains a separately supported GA API in November 2026. No public evidence established a truly immutable, first-class **specific-merchant + single-use + exact-amount** control in Stripe Issuing or Privacy's card schema. Lithic documents merchant-lock functionality but does not expose it as an ordinary customer-created Authorization Rule. Ramp's 2026 agent-card documentation is promising, but its exact atomic one-tap/mint semantics were not independently proven from a normally rendered API page. Network-agent rails from Visa and Mastercard are still rolling out, so press-release production claims should not be confused with universal merchant availability.

The legal analysis is also necessarily jurisdiction-specific. The U.S. cases reviewed support the proposition that **public/logged-off data and authenticated/private access are treated differently**, not the proposition that automated challenge circumvention is universally lawful. *Ryanair* is especially easy to overread: the January 2025 result turned on failure to prove statutory CFAA loss; it did not create a blanket privilege to bypass anti-bot technology. citeturn346915view0turn346915view2

### Primary-source evidence set

The core source set used for the conclusions above comprised the following **24 primary/vendor or governmental sources**; quotes in the body are deliberately short.

```text
CAPTCHA / HITL
https://anchorbrowser.io/
https://www.browserbase.com/pricing
https://steel.dev/pricing
https://developers.cloudflare.com/browser-run/features/human-in-the-loop/
https://developers.cloudflare.com/browser-run/pricing/
https://mastra.ai/docs/agents/human-in-the-loop
https://mastra.ai/pricing
https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/
https://browser-use.com/cloud-browser
https://browser-use.com/pricing
https://www.hcaptcha.com/terms

LEGAL
https://www.govinfo.gov/app/details/USCOURTS-cand-3_23-cv-00077/USCOURTS-cand-3_23-cv-00077-7
https://www.govinfo.gov/content/pkg/USCOURTS-cand-3_23-cv-00077/pdf/USCOURTS-cand-3_23-cv-00077-7.pdf
https://www.govinfo.gov/app/details/USCOURTS-ded-1_20-cv-01191/USCOURTS-ded-1_20-cv-01191-5

AGENT PAYMENTS
https://developer.visa.com/capabilities/visa-intelligent-commerce
https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-bill-billtrust-launch-pilot.html
https://www.mastercard.com/news/press/2025/april/mastercard-unveils-agent-pay-pioneering-agentic-payments-technology-to-power-commerce-in-the-age-of-ai
https://www.mastercard.com/content/mccom/eu/en/news-and-trends/stories/2026/verifiable-intent.html
https://www.mastercard.com/au/en/news-and-trends/press/2026/june/mastercard-launches-agent-pay-for-machines.html
https://ap2-protocol.org/
https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md
https://developers.openai.com/commerce/guides/key-concepts
https://openai.com/index/buy-it-in-chatgpt/?faq=5
https://stripe.com/newsroom/news/agentic-commerce-suite

VIRTUAL CARDS / ISSUING
https://developers.privacy.com/docs/cards
https://www.privacy.com/pricing
https://support.privacy.com/hc/en-us/articles/12036411849495-What-is-the-Privacy-API
https://docs.stripe.com/api/issuing/cards
https://docs.stripe.com/issuing/testing
https://stripe.com/pricing
https://docs.lithic.com/docs/api-basics
https://docs.lithic.com/docs/spend-limits
https://docs.lithic.com/docs/authorization-rules-v2
https://support.ramp.com/hc/en-us/articles/360043075754-Ramp-pricing-overview
```

### Proof of work

Representative search queries executed during the investigation are below. Searches were deliberately biased toward official documentation, court/GovInfo records, pricing pages, current protocol specifications, and 2025–2026 release/status material rather than reviews or affiliate comparisons.

```text
"Anchor Browser pricing captcha human in the loop 2026"
"Browserbase pricing captcha live view human handoff"
"Steel browser pricing captcha human in the loop"
"Cloudflare Browser Run human in the loop pricing"
"Mastra human in the loop suspend resume pricing"
"LangGraph human in the loop interrupt resume"
"Browser Use cloud browser live URL captcha pricing"
"hCaptcha terms bots scripts artificial intelligence challenges"
"Google terms bypass protective measures recaptcha"
"Cloudflare Turnstile terms automated captcha third party"
"Meta Platforms Bright Data 2024 public scraping terms GovInfo"
"Ryanair Booking Holdings CFAA 2024 2025 GovInfo"
"Ryanair Booking Holdings CFAA 2026 appeal Third Circuit"
"Visa Intelligent Commerce developer API passkey payment instruction 2026"
"Visa Intelligent Commerce live pilot 2026"
"Mastercard Agent Pay Agentic Token 2025"
"Mastercard Verifiable Intent AP2 2026"
"Mastercard Agent Pay for Machines 2026"
"Google Agent Payments Protocol AP2 specification mandate FIDO SD-JWT"
"OpenAI Agentic Commerce Protocol delegated payment max amount expiration"
"Stripe Shared Payment Token agentic commerce 2026"
"Stripe Order Intents API 2025 2026"
"Privacy API SINGLE_USE MERCHANT_LOCKED spend_limit TRANSACTION sandbox"
"Stripe Issuing per_authorization spending controls sandbox"
"Lithic spend limits authorization rules merchant lock sandbox"
"Ramp agent cards virtual card API merchant rules approval sandbox"
```

**Bottom line:** for CAPTCHA, build around **authorized human handoff, not solver sophistication**. For payments, build around **verifiable intent and narrowly delegated credentials, not bounded trust in an LLM holding a card**. Those two choices align the technical architecture with the direction of both the 2026 browser tooling and the emerging payment-network standards, while materially reducing ToS, fraud and prompt-injection exposure.

## Sources (URLs found in report)
- https://developers.cloudflare.com/browser-run/features/human-in-the-loop/
- https://developers.cloudflare.com/browser-run/pricing/
- https://www.browserbase.com/pricing
- https://steel.dev/pricing
- https://anchorbrowser.io/
- https://mastra.ai/docs/agents/human-in-the-loop
- https://mastra.ai/pricing
- https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/
- https://browser-use.com/cloud-browser
- https://browser-use.com/pricing
- https://www.govinfo.gov/content/pkg/USCOURTS-cand-3_23-cv-00077/pdf/USCOURTS-cand-3_23-cv-00077-7.pdf
- https://www.govinfo.gov/app/details/USCOURTS-ded-1_20-cv-01191/USCOURTS-ded-1_20-cv-01191-5
- https://www.hcaptcha.com/terms.
- https://developer.visa.com/capabilities/visa-intelligent-commerce
- https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-bill-billtrust-launch-pilot.html
- https://www.mastercard.com/news/press/2025/april/mastercard-unveils-agent-pay-pioneering-agentic-payments-technology-to-power-commerce-in-the-age-of-ai
- https://www.mastercard.com/content/mccom/eu/en/news-and-trends/stories/2026/verifiable-intent.html
- https://www.mastercard.com/au/en/news-and-trends/press/2026/june/mastercard-launches-agent-pay-for-machines.html
- https://ap2-protocol.org/
- https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md
- https://developers.openai.com/commerce/guides/key-concepts
- https://openai.com/index/buy-it-in-chatgpt/?faq=5
- https://stripe.com/newsroom/news/agentic-commerce-suite
- https://stripe.com/mx/sessions/2025/product-keynote
- https://developers.privacy.com/docs/cards
- https://www.privacy.com/pricing
- https://support.privacy.com/hc/en-us/articles/12036411849495-What-is-the-Privacy-API
- https://docs.stripe.com/api/issuing/cards
- https://docs.stripe.com/issuing/testing
- https://stripe.com/pricing
- https://docs.lithic.com/docs/api-basics
- https://docs.lithic.com/docs/spend-limits
- https://docs.lithic.com/docs/authorization-rules-v2
- https://support.ramp.com/hc/en-us/articles/360043075754-Ramp-pricing-overview
- https://docs.ramp.com/llms-guides/agent-payments.txt`
- https://docs.ramp.com/llms-guides/virtual-cards-embedded.txt`
- https://www.hcaptcha.com/terms
- https://www.govinfo.gov/app/details/USCOURTS-cand-3_23-cv-00077/USCOURTS-cand-3_23-cv-00077-7
- https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/?trk=article-ssr-frontend-pulse_little-text-block
- https://support.privacy.com/hc/en-us/articles/38299485169559-What-is-the-Privacy-API
- https://docs.stripe.com/api/issuing/cardholders/update?api-version=2025-11-17.preview
- https://docs.stripe.com/issuing/testing?locale=zh-CN
- https://stripe.com/pricing?sid=9cbfc07f-1bbb-439e-9911-1b235e2a28bd
- https://support.ramp.com/ramp-pricing-overview/
