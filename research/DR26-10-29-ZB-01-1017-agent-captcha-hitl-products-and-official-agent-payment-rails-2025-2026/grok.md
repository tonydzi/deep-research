# DR26-10-29-ZB-01-1017 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll research live sources on HITL captcha handoff products and 2025–2026 official agent-payment rails, then write the full report from pages opened in this run.Search hits are in. Next I’ll open the official docs, pricing pages, and payment-rail announcements so each claim has a page I actually read.I need the exact quotes from pages already opened before the report can be written. Checking the saved fetches now.The earlier fetches are in the session log. I am pulling the exact quotes from those pages so the report does not invent numbers.A. Platform, scope, and what was actually opened

Report ID DR26-10-29-ZB-01-1017. Research date requested: 29 October 2026. Model and mode: Grok 4.7, live web search plus full-page fetch. Live search worked. Several targets did not: Mastercard’s October 2026 press URL returned Akamai “Access Denied”; The Block’s 20 October 2026 Mastercard article returned only a Cloudflare “Just a moment…” interstitial; https://openai.com/index/buy-it-in-chatgpt/ returned an empty body. Those three are not sources.

Scope kept to Q2 (CAPTCHA handling, human handoff, legal and terms risk, official paths that avoid a challenge) and Q3 (2025–2026 official agent-payment rails and virtual-card APIs). Model choice, and a general essay on a policy engine outside the LLM, are out of scope. This report does not describe how to defeat any site’s bot protection.

Assumption used throughout: the operator is automating its own accounts or a workflow the site owner has allowed. If the only path is another site’s challenge and that site has not allowed the automation, none of the products below make that allowed.

Vendor pages print their own dates. Those dates are quoted as printed. A page “last updated” on 29 October 2026 was fetched in this research session; that stamp is the vendor’s, not an independent clock check.

B. Q2. Human-in-the-loop products that pause or hand a browser to a person

These are two different mechanisms. A workflow interrupt pauses the agent program and waits. A live view lets a person click in the same browser. Only some products do both. Prices below are what the opened pricing pages print. [established] for those figures.

B1. Anchor Browser. Cloud browsers over CDP or REST, plus natural-language tasks. Official plan table: Free, $5 credits per month, up to 5 concurrent browsers; Starter, $50 per month, up to 25 concurrent; Growth, $2,000 per month, up to 200 concurrent; Enterprise, custom, 500+. “Automated captcha bypass” is marked on from Starter, not on Free. “Cloudflare verified browser agents” is marked on from Growth. Task runs are “0.1 credits ($0.10)” or the infrastructure cost if that is higher. Infrastructure: “$0.01 per browser”, “$0.05 per browser hour”, Anchor proxy “$8 per GB”, bring-your-own or proxyless egress “$0.2 per GB”. AI-step prices “take effect on August 21, 2026”: Light $0.01, Standard $0.05, High Performance $0.10. https://docs.anchorbrowser.io/pricing

Pause and resume: set `humanIntervention: true` (Python `human_intervention`). The docs say the agent “pause[s] execution and request[s] human intervention”. Webhook `intervention.requested` includes `request_id`, the agent’s message, and a `live_view_url` “for one-click live takeover”. `intervention.resolved` fires when it is answered. There is a GET for pending requests and a POST to respond. https://docs.anchorbrowser.io/agentic-browser-control/human-in-the-loop

Cloud surface: API key, “Agent Access” without a dashboard account, webhooks, CDP/REST. Maturity: current Mintlify docs with a dated AI-price change in 2026. The opened pricing page lists Free, Starter, Growth, and Enterprise. A “Team $500” tier was not on that page. [no source found] on the official page for a Team plan. Third-party price roundups were not opened, so they are not treated as a second source.

B2. Browserbase. Pricing page: Free $0, “3 concurrent browsers”, “1 browser hour”, “15 minutes /session”, captcha solving “No”, data retention 7 days, stealth “No”. Developer $20, 25 concurrent, 100 browser hours then “$0.12/browser hr”, “Auto captcha solving”, 1 GB proxy then $12/GB, retention 30 days, stealth “Basic”. Startup $99, 100 concurrent, 500 hours then “$0.10/browser hr”, auto captcha, 5 GB then $10/GB, stealth Basic. Scale, custom, “250+ concurrent”, “Verified Agents + Captcha solving”, stealth “Advanced”, HIPAA BAA and SSO. https://www.browserbase.com/pricing

A search snippet said the docs bill Free at 1 concurrent browser. That docs page was not opened. The opened pricing page says 3. Prefer the pricing page. [search-only] for the “1”.

CAPTCHA product: “Browserbase enables CAPTCHA solving by default for all sessions.” “Expect CAPTCHA solving to take up to 30 seconds.” Disable with `solveCaptchas: false`. Console events `browserbase-solving-started` and `browserbase-solving-finished`. The same page says: “Use CAPTCHA solving only for workflows that users and website owners authorize. Follow the site's terms, respect access controls, and use supported integrations when available.” https://docs.browserbase.com/platform/identity/captcha-solving

Live view is not a durable graph pause. `bb.sessions.debug(session.id)` returns `debuggerFullscreenUrl`. “On any running browser session - watch, click, type, and scroll in real-time.” Uses include “Human in the loop - instantly take control” and “delegate credentials - give control to the end user”. https://docs.browserbase.com/platform/browser/observability/session-live-view

Cloud surface: session API, Playwright/Selenium, debug URLs, iframe embed. Maturity: public self-serve pricing and current docs. [established]

B3. Steel. Docs last edit “October 23rd, 2026”. Launch “$0 + usage” with “$30 free usage credits, one time”, valid 90 days. Scale “$250 + usage” with “$100 free usage credits / month”. Enterprise custom. Browser hours $0.10 Launch / $0.08 Scale. Proxy $10/GB / $6/GB. “CAPTCHA solves” $3 per 1,000 / $1 per 1,000. Concurrency 10 / 100 / 1,000+. Max session 15 minutes / 1 hour / up to 24 hours. “To use CAPTCHA solving or Steel-provided proxies on Launch, verify your account by adding $10 in paid balance. Free credits do not count.” Stealth Browser is Enterprise only. https://docs.steel.dev/overview/pricinglimits

FAQ: “Can Steel bypass CAPTCHAs? Yes, on Steel Cloud.” It names “Cloudflare Turnstile, PerimeterX press-and-hold and image CAPTCHAs”. “Steel Browser is the open-source server” you host; “Steel Cloud is the managed version, adding scale, stealth, proxies and CAPTCHA solving.” HITL: “A live session viewer lets people watch or take over running sessions.” https://steel.dev/faq

The FAQ is vendor marketing. [established] that Steel advertises solving. [low-authority] as a claim that those challenges are actually solved. Cloud surface: CDP WebSocket, Node and Python SDKs. Open-source browser versus paid cloud is a real split: solving and stealth are Cloud features, not the self-hosted server.

B4. LangGraph interrupts. This is a graph runtime, not a browser and not a CAPTCHA product. “When an interrupt is triggered, LangGraph saves the graph state using its persistence layer and waits indefinitely until you resume execution.” Resume with the same `thread_id` and `Command({ resume })`. “The node restarts from the beginning of the node where the interrupt was called when resumed, so any code before the interrupt runs again.” A Zod `responseSchema` needs `@langchain/langgraph` version 1.4.16 or later. A raw JSON Schema is exposed but “does not validate resume values”. https://docs.langchain.com/oss/javascript/langgraph/interrupts

LangGraph Platform price: [no source found]. Maturity: current OSS docs. The restart-from-the-top behavior matters: a side effect placed before `interrupt()` runs again on resume. Put the handoff at the start of the node, or make the earlier work idempotent.

B5. Mastra workflows. Also a framework, not a browser. “Some workflows need to pause for human input before continuing.” `suspend()` can return a payload. `resume()` with `resumeData` “continues a workflow after receiving input from a human. The workflow resumes from the step where it was paused.” The documented example uses `resumeData: { approved: true }`. `bail()` stops the step “without triggering an error” when a person rejects. https://mastra.ai/docs/workflows/human-in-the-loop

Mastra Cloud price: [no source found].

B6. Cloudflare Agents browser. Docs header: “Last updated Oct 29, 2026”. Browser Run keeps “tabs, cookies, and login session intact while the browser session remains alive”. “By default, an idle browser stays alive for up to 10 minutes.” “Live View lets a person watch or control a running browser in real time. Use it when a step needs a human, such as a login, multi-factor authentication (MFA), a CAPTCHA, or sensitive input.” `browser.liveView({ mode: "tab" })` or `"devtools"`. “expiresInMs” is “about five minutes”. “browserTool does not pause a run while a person acts. Hand off between turns instead.” “Live View URLs are bearer credentials. Anyone with a link can control the browser.” https://developers.cloudflare.com/agents/tools/browser/

Cloud surface: Cloudflare Worker, Durable Object, Agents SDK, CDP, `wrangler`. Browser Run list price: [no source found]. This product does not advertise a CAPTCHA solver. It hands the challenge to a person. [established]

C. Q2. Who advertises “solves CAPTCHA”, and the risk layers

Products that advertise solving, from pages opened here: Browserbase from the Developer plan up, on by default in the solver docs, with an authorization warning. Steel Cloud, including a named list of challenge types, after a $10 Launch deposit. Anchor from Starter up, labeled “Automated captcha bypass”. Cloudflare Agents, LangGraph, and Mastra do not advertise a solver. Cloudflare’s separate Verified-bots program is an identity system, described in D.

Risk is five different things. They are not interchangeable.

C1. Contract with the CAPTCHA vendor. hCaptcha Master Terms, “Last Updated: November 17, 2025. Please note that our English language terms are authoritative.” Section 2: a user “will not abuse our website, misrepresent your identity to us, or use artificial intelligence to complete any portion of any task on IMI which requires human intelligence.” Section 7: “you WILL NOT: perform any task with the use of Internet bots, web robots, bots, scripts, or any other form of artificial intelligence or otherwise attempt to pass challenges from IMI or any Requester or Integrator without completing tasks as they are described.” Section 5 forbids a Requester, without a separate written agreement, from posting “(b) tasks that violate a third party website's terms and conditions” and “(d) tasks that ask Contributors to take action to manipulate a website's behavior or results.” Section 5 is about tasks posted on Intuition Machines. Sections 2 and 7 are the anti-bot clauses for people who use the Services. https://www.hcaptcha.com/terms

Whether an outside bot operator, who never registered, has formed a contract just by touching an embedded widget is not decided by any case opened here. The terms say people who do not agree “may not use or access the Site or the Service.” That is the vendor’s position. [established] as the text of the terms. [emerging] as enforceability against a non-customer.

C2. Contract with the browser vendor. Browserbase’s own solver page limits solving to “workflows that users and website owners authorize.” A customer who turns the solver on against a site that has not allowed it is outside the use Browserbase states. [established] as Browserbase’s instruction. Steel’s FAQ states the opposite posture in marketing language (“Yes, on Steel Cloud”) and does not, on the opened FAQ, repeat an authorization limit.

C3. Contract with the target site. A site’s own terms often forbid automated access. That breach is a contract or terms issue with that site. It was not re-documented site by site in this run. [no source found] for a 2025–2026 survey of individual site terms.

C4. Statute and civil lawsuit. The opened case report is TechCrunch, Sarah Perez, 24 January 2024, on Meta v. Bright Data, N.D. Cal. No. 3:23-cv-00077-EMC. The court denied Meta partial summary judgment on breach of contract for scraping that Meta had not shown was anything but public, logged-out data. Meta argued that tools to circumvent CAPTCHAs proved collection “behind authentication barriers.” “The court disagreed with this, too, saying that using an automated program to bypass a CAPTCHA ‘is different from accessing a password-protected website’ and that ‘Meta surely understands the difference.’” Bright Data was treated as a logged-out “visitor”, not a “user”, because Meta had not shown Bright Data used its own Facebook or Instagram accounts while scraping. “The only remaining claim against Bright Data in this case is for tortious interference with contract.” https://techcrunch.com/2024/01/24/court-rules-in-favor-of-a-web-scraper-bright-data-which-meta-had-used-and-then-sued/

That is one news report of a partial summary judgment, not the opinion PDF. [single-source] [emerging]. It does not hold that solving a CAPTCHA is lawful in general. It holds, as reported, that CAPTCHA bypass was not treated as the same thing as breaking a password gate, in a logged-out public-data contract dispute. Later disposition of that case, and the separate X Corp. v. Bright Data docket, were search hits only. They are not used as findings. No 2024–2026 opinion was opened that says automated CAPTCHA solving is, by itself, a Computer Fraud and Abuse Act violation. [no source found] for that holding.

C5. Platform enforcement and reputation. Cloudflare’s 16 October 2026 post says identity is not a permanent pass. “we de-listed Perplexity’s bot from our Verified bot list in 2025 when it sought to access resources on websites that opted out of scraping and subsequently attempted to circumvent controls when those requests were blocked.” Trust is “built over time” against robots.txt (RFC 9309) and HTTP 429. “a robots.txt does not need to say that a bot must stop” if the site signals that access is unwelcome another way. https://blog.cloudflare.com/verified-bots-accountability/

So: a solver subscription can be a terms breach with hCaptcha (if those terms bind the operator), with Browserbase (if the site owner did not authorize it), and with the target site. It can draw blocks, de-listing, and account loss. Civil exposure depends on facts a 2024 public-data ruling did not treat as password circumvention. Criminal CFAA exposure for solving alone was not established by any page opened here. Reputation cost is separate and already demonstrated by Cloudflare de-listing a named bot for evasion.

Google’s reCAPTCHA FAQ, “Last updated 2026-04-02 UTC”, is marked “This page is deprecated” toward Google Cloud Fraud Defense. It does not contain a sentence of the form “you may not solve other sites’ CAPTCHAs.” What it does say, under automated tests: for reCAPTCHA v2 test keys, “You will always get No CAPTCHA and all verification requests will pass,” with site key `6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI`. That is for the site owner’s own tests. The “automated queries” answer is the visitor-facing message “We're sorry, but your computer or network may be sending automated queries,” and it points to Google’s unusual-traffic help page. https://developers.google.com/recaptcha/docs/faq

Cloudflare Turnstile’s own terms page was searched and not opened. [no source found] for the Turnstile contract text. Steel’s claim that it solves Turnstile is Steel’s claim only.

D. Q2. Official paths where a CAPTCHA need not appear

Principle, from the pages that were opened: if the counterparty offers an official credential or an official API, use that, and do not drive the website. A challenge exists to stop an untrusted browser. It is the wrong layer once the other side will accept a signed agent, a payment token, or a direct API.

Concrete opened examples:

Visa’s 20 October 2026 pilot: “This agent authentication can satisfy the site protections historically provided by CAPTCHA and other bot blocking mechanisms, without requiring an additional agent login.” That is for supplier portals that participate, using Trusted Agent Protocol. It is not a solver. https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.22856.html

Cloudflare Verified bots: “Bots use Web Bot Auth to sign requests that establish their identity, which BotBase then catalogues.” A site can choose to allow that identity. Continuing trust still depends on robots.txt, 429s, and not evading blocks. https://blog.cloudflare.com/verified-bots-accountability/

Stripe agent checkout, so the agent sends a Shared Payment Token to a seller instead of clicking through the seller’s card form. Details are in Q3. https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens.md?agent-seller=agent

reCAPTCHA test keys remove the challenge only on the owner’s own v2 test integration. They are not a way to skip another site’s widget. https://developers.google.com/recaptcha/docs/faq

A catalog of ordinary SaaS products (mail, chat, ads, and so on) that already have official APIs was not re-opened. [no source found] for a general list. The working rule is the same: if an official API exists for the action, the browser, and therefore the challenge, is the wrong tool.

E. Q2 shortlist and recommendation

| Product | Price opened | Cloud surface | Legal / terms risk | One line |
| --- | --- | --- | --- | --- |
| LangGraph interrupt | OSS price of the library is the docs; hosted platform price [no source found] | Checkpointer, `thread_id`, `Command({resume})` | No CAPTCHA product. Risk is whatever the resumed tool then does | Use this when the pause must survive and wait |
| Mastra suspend / resume / bail | Cloud price [no source found] | Workflow run, `resumeData.approved` | Same: framework only | Use this for an approve / reject step |
| Cloudflare Agents live view | Browser Run price [no source found]. Idle browser up to 10 minutes | Worker + Durable Object + `browser.liveView()` | Low if the person solves a challenge on a session they are allowed to use. URLs are bearer credentials | Hand the human the browser. Do not give the URL to the model |
| Anchor HITL | Free $5 credits; Starter $50; Growth $2,000. Solver from Starter | Webhook `intervention.requested` + `live_view_url` + POST response | Solver flag is a terms risk if used on a site that did not allow it. HITL itself is a handoff | Buy the handoff. Leave captcha bypass off unless the site owner allowed it |
| Browserbase live view | Free $0 without solver; solver from Developer $20 | `sessions.debug` → `debuggerFullscreenUrl` | Solver is on by default in the solver docs and is limited by Browserbase to authorized workflows | Use live view on Free, or paid with `solveCaptchas: false`, unless the site owner authorized solving |
| Steel live viewer | Launch $0 + usage after $30 credits; solver needs $10 paid deposit; Scale $250 | CDP cloud session, viewer | FAQ advertises bypass of named challenges. Highest marketing risk of this set | Self-host the open-source browser for your own apps. Do not turn on Cloud solving against third-party challenges |

Overall Q2 recommendation. Pause in LangGraph or Mastra, and, when a page really needs a person, hand them a live view on Cloudflare, Anchor, Browserbase, or Steel. Do not buy a “solves CAPTCHA” plan to pass another site’s reCAPTCHA, hCaptcha, or Turnstile. Prefer an official API, Cloudflare Web Bot Auth where the site allows that bot, or Visa Trusted Agent Protocol where the portal is in the pilot. Confidence: [established] on the product facts that were opened; [emerging] on the case-law boundary, because the only opened court account is one 2024 news report of a partial summary judgment. Most important assumption: the session is one the human and the site owner are allowed to automate. If that is false, a live view used by a person who is allowed to use the site is a different act from an unattended solver, and the unattended solver stays outside Browserbase’s own rule and inside hCaptcha’s anti-bot clauses if those terms bind the operator.

F. Q3. Network and protocol rails, 2025–2026 delta

F1. Visa Intelligent Commerce, as of the 20 October 2026 press release. “Visa, BILL and Billtrust today announced they have launched the first live pilot to enable trusted AI agents to initiate and complete approved buyer invoice payments end-to-end through supplier portals.” The BILL Payment Agent is “registered using Visa’s Trusted Agent Protocol and Agentic Directory” and “uses tokenized payment credentials and controls aligned with the buyer’s approved payment instructions.” Buyers do not share portal logins: “No. The pilot is designed to eliminate the need to share supplier-portal logins.” Human approval: “Is the AI agent making payment decisions? No. The agent only executes eligible payments that have already been approved by the buyer.” Controls listed: authenticated agent for an identified buyer, tokenized credentials, controls tied to the approved instruction, a path tied to the specific invoice, visibility of authorization and completion. https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.22856.html

Status: a live pilot with named partners, not a self-serve developer API on the page that was opened. Passkeys are not described there. [no source found] on that page for a public signup or API reference. An 8 April 2026 “Intelligent Commerce Connect” announcement was a search hit only and is not used. [emerging] as a production rail beyond this pilot.

F2. Mastercard Agent Pay / Agent Pay for Machines. The Mastercard press URL was blocked. The Block article did not load. The only opened primary that names the product is a Stripe footnote: “Stripe might use tokens issued through network programs such as Mastercard’s Agent Pay and Visa’s Intelligent Commerce programs on your behalf.” https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens.md?agent-seller=agent

Launch status, buyer and seller APIs, sandbox, and “approved budgets” for Agent Pay for Machines: [no source found] on a page that actually opened. Do not treat search snippets as a launch date. [speculative] if someone needs those details before Mastercard’s own page can be read. Google’s 16 September 2025 AP2 post does include a Mastercard executive quote supporting AP2 and FIDO verifiable credentials. That is a 2025 partnership quote, not a 2026 product specification. https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol

F3. Google Agent Payments Protocol. Two opened documents disagree on names. They should not be averaged.

Blog, 16 September 2025, Stavan Parikh and Rao Surapaneni: “an open protocol” that “can be used as an extension of the Agent2Agent (A2A) protocol and Model Context Protocol (MCP)”, with “more than 60 organizations”. Mandates are “tamper-proof, cryptographically-signed digital contracts” signed by “verifiable credentials”. Human present: an Intent Mandate, then the user signs a Cart Mandate, “a secure, unchangeable record of the exact items and price”. Human not present: the user signs an Intent Mandate with “price limits, timing, and other conditions”, and that “can allow the agent to automatically generate a Cart Mandate”. https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol

Spec on GitHub, title “Agentic Payment Protocol (v0.2)”: the mandate types in this document are Checkout Mandate and Payment Mandate, not Intent and Cart. “AP2 specifies the use of SD-JWTs for securing the Payment and Checkout Mandates.” `vct` examples: `mandate.payment.1`, `mandate.checkout.open.1`. “The following role MUST be non-agentic: Trusted Surface.” “The following role is expected to be agentic: Shopping Agent.” “When this document refers to validation or processing for a particular role, it MUST happen in deterministic code regardless of whether the role is agentic or not.” Direct mode: “The User directly sees the closed Checkout and approves it and its payment explicitly” on the Trusted Surface. Autonomous mode: the user approves constraints; open mandates “MUST include the agent's public key as a `cnf` claim”; “It is RECOMMENDED to set the `exp` claim … to the smallest value that will allow the Shopping Agent to complete the assigned task.” “Shopping Agents MUST NOT present any subsequent open Payment or Checkout Mandates without receiving a rejection receipt from the previous one.” Agent-to-agent delegation is “outside the scope of the current specification.” Dispute evidence is “a non-repudiable picture”; “retention, and retrieval requirements are outside the scope.” The spec says it is compatible with the Universal Commerce Protocol. https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md

The spec page does not state a production network go-live date. A secondary “released 28 April 2026” date was not on either opened page. [no source found] for a GA date. Status: open spec plus a 2025 announcement. [emerging] as something a developer can implement from GitHub. [speculative] as a live card-network rail.

Prompt-injection control that is actually in the spec: the human signs on a non-agentic Trusted Surface, and verifiers check mandates in deterministic code. An LLM is not the verifier.

F4. Stripe agentic commerce, agent side, Shared Payment Tokens. Seller-side protocol and Order Intents pages were not opened. The OpenAI “buy it in ChatGPT” URL returned empty. Those are not described here.

What was opened: “Shared payment tokens (SPTs) grant sellers scoped access to customers’ payment methods for agent-initiated purchases.” Available in the US, Canada, and a listed European set: AT, BE, BG, CH, CY, CZ, DE, DK, EE, ES, FI, FR, GB, GI, GR, HR, HU, IE, IT, LI, LT, LU, LV, MT, NL, NO, PL, PT, RO, SE, SI, SK, plus CA and US. “By using SPTs, you agree to the terms of service” at Stripe’s agentic-commerce agent-services preview section. Flow: customer submits a PaymentMethod via the Payment Element; agent `POST`s `https://api.stripe.com/v1/shared_payment/issued_tokens` with header `Stripe-Version: 2026-09-30.preview`, `payment_method`, `seller_details[network_business_profile]`, `usage_limits[currency]`, `usage_limits[expires_at]`, `usage_limits[max_amount]`, and `return_url`. Sandbox seller profile printed on the page: `profile_test_61TU90nIeGjU7NNVXA6TU90m7ISQWsBxpcx9lASWWXTk`. Methods: cards, Link, Apple Pay, Google Pay, Klarna, and Affirm “(limited)”. Affirm rule: “An agent may not programmatically interact with the Affirm loan application UI inside the webview; the buyer must be the one navigating and confirming.” States include `active`, `requires_action` (3-D Secure or other next action), and terminal `deactivated`. The agent handles `shared_payment.issued_token.requires_action` by showing `next_action` to the customer. https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens.md?agent-seller=agent

Human approval on this page is the Payment Element plus any 3-D Secure step, and the caps are `max_amount` and `expires_at` on the create call. The page shows the agent setting those caps. A model that can call that endpoint can choose the number. The safeguard is to set the cap in deterministic code, which is the same pattern AP2 requires and which this Stripe page does not, by itself, enforce. Status: public preview (`2026-09-30.preview`) in the listed countries. [emerging]

G. Q3. Virtual-card APIs with hard limits

G1. Stripe Issuing. Three opened pages, and they do not use one limit story.

“Issuing for your business lets you programmatically create virtual cards for your business, employees, contractors, or AI agents to make purchases on your behalf.” Standard program: “Get started immediately with no sales conversation.” Custom: contact sales. Standard default limits on this page: “Create 25 cards per week”, “Spend 5000 USD per week”, “Spend 500 USD per card per day”. “Reseller and on-demand and fulfillment programs can only create single use cards.” “You can activate Issuing in a sandbox in the Stripe Dashboard without contacting sales. Sandbox access is instant.” “In a sandbox, all card transactions are simulated. No real funds move.” Single-use cards: “Cards are automatically invalidated after use.” Real-time authorizations: “Approve or decline every transaction using webhooks. … You own the authorization integration.” https://docs.stripe.com/issuing/direct

Virtual cards: “Virtual cards are 0.10 USD each in the US, 0.10 GBP in the UK and 0,10 EUR in the EU.” Number and CVC are retrieved with `expand[]=number` and `expand[]=cvc`. “If you’re generating virtual cards for your own use, you’re not required to attain PCI-DSS compliance for Issuing activity. If you’re generating virtual cards for use by your users, you might be considered a Service Provider.” https://docs.stripe.com/issuing/cards/virtual

Spending controls: categories, countries, card presence, and `spending_limits` with `amount` and `interval` (the example uses `per_authorization`, `weekly`, and `monthly`). “Merchant ID spending controls is in private preview.” “If you don’t set spending_limits, a default spending limit of 500 USD per day applies.” “an unconfigurable default spending limit of 10000 USD also applies to each authorization.” “Spending aggregation is done on a best-effort basis. You might notice a delay of up to 30 seconds.” “Additional tips and fees can be posted at a later time, causing a spending limit to be exceeded.” https://docs.stripe.com/issuing/controls/spending-controls

Disagreement inside Stripe: the spending-controls page’s default is $500 per day and a $10,000 per-authorization ceiling; the “for your business” page’s standard program is 25 cards per week, $5,000 per week, and $500 per card per day. Both pages were opened. They are different layers (card control versus program limit), and this report does not collapse them into one number. Merchant-ID lock is private preview, not a generally available “this card, this merchant” switch. The public list price page at stripe.com/pricing#issuing was linked and not opened. The $0.10 figure is from the virtual-card doc.

G2. Privacy.com. Cards API, page `updatedAt: 2026-04-03`. `POST https://api.privacy.com/v1/cards`. `type`: `SINGLE_USE`, `MERCHANT_LOCKED`, or `DIGITAL_WALLET`. `spend_limit` is integer cents and whole dollars. `spend_limit_duration`: `ANNUALLY`, `FOREVER`, `MONTHLY`, `TRANSACTION`. “SINGLE_USE: Card will close shortly after the first transaction.” “MERCHANT_LOCKED: Card is locked to first merchant that successfully authorizes the card.” “TRANSACTION: Card will authorize multiple transactions if each individual transaction is under the spend limit.” https://developers.privacy.com/docs/cards

So `TRANSACTION` is a repeating per-authorization cap, not a one-charge close. `SINGLE_USE` is the close-after-first type. `MERCHANT_LOCKED` binds to whichever merchant first succeeds. It is not a merchant id chosen at create time on this page. A support article saying the lock happens on the first successful transaction was not opened. The API table already says that.

Getting Started, `updatedAt: 2026-06-22`: programmatic issue, spend limits, and “the identity of all end user’s must successfully pass the Privacy Customer Identification Program (CIP)” under BSA, OFAC, and the PATRIOT Act before they can transact. Callout: “You are responsible for all financial activity on your Privacy.com account, inclusive of API, CLI or MCP activity.” https://developers.privacy.com/docs/getting-started

The opened Getting Started page does not mention a sandbox. An older readme claim of `sandbox.privacy.com` was not confirmed on the destination page. [no source found] for a current Privacy sandbox. Consumer plan prices: [no source found].

G3. Lithic spend limits. Page `updatedAt: 2026-06-03`. “Spend Limits have been superseded by Authorization Rules.” “New programs should implement Velocity Limit Authorization Rules.” The authorization-rules page itself was not opened. On this page, cards have one `spend_limit` and `spend_limit_duration` of `ANNUALLY`, `FOREVER`, `MONTHLY`, or `TRANSACTION`. “a spend limit of 0 means that no spend limit is enforced.” Limits are rolling, not calendar. “For customers onboarded after 2/1/2026, no default account limits are set.” Before that, non-ASA defaults were “$1,250 per day and $5,000 per month.” “they are not designed for precise balance tracking.” “Spend velocity calculation may not include transactions from the past several seconds, potentially allowing approvals that exceed configured limits.” “Spend limits cannot block force posted charges.” A merchant may authorize $90 and clear $110 against a $100 remaining limit, and “Spend limits are not able to prevent this discrepancy.” https://docs.lithic.com/docs/spend-limits

Lithic list price, merchant-id lock, and the sandbox host: [no source found] on an opened page. [established] that a Lithic spend limit is not an exact-amount lock.

G4. Ramp sandbox. The HTML docs page is a JavaScript shell. The machine-readable guide that opened is https://docs.ramp.com/llms-guides/sandbox.txt and names source `/developer-api/v1/sandbox`. “The Ramp sandbox is a testing environment where developers and partners can safely build, test, and experiment with Ramp's API.” “No real money movement: Demo accounts cannot process actual financial transactions.” UI `https://demo.ramp.com`. API base `https://demo-api.ramp.com`. Demo actions via Command-J: mark a bill paid, mark a reimbursement paid, add transactions, each with a role permission. The opened text addresses “developers and partners”. It does not say there is an instant self-serve toggle. Card-issuance field names and public pricing: [no source found]. Do not treat Ramp as documented here for “mint a card for this amount.”

H. Q3. “Agent builds the cart, human confirms once, credential is capped at that amount”

No opened product mints a network card that is guaranteed to capture exactly one amount and no more. Force-post, over-capture, tips, FX conversion, and a few seconds of aggregation lag are documented on Stripe and Lithic.

What is native:

Stripe SPT. Human enters the payment method in the Payment Element. The create call sets `usage_limits[max_amount]` and `expires_at` for one seller profile. 3-D Secure can still demand the human. Affirm’s loan UI must be the human, not the agent. The cap is only as strong as the code that sets `max_amount`. Preview header `2026-09-30.preview`.

Stripe Issuing single-use card plus `per_authorization` spending limit, optionally a real-time authorization webhook the business owns. Programmatic issue is documented, including “for AI agents”, with an instant no-funds sandbox. Merchant-id lock is private preview. Single-use “invalidated after use” is documented on the business-issuing page; the lifecycle subpage was not opened. Over-capture and tips can still exceed the limit after approval. The $10,000 per-authorization ceiling is unconfigurable unless support disables it, and a missing limit becomes $500 per day.

Privacy `SINGLE_USE` plus `spend_limit`. Closest “close after the first transaction” on a page that was opened. `MERCHANT_LOCKED` is first-successful-merchant, not a predeclared merchant. `TRANSACTION` allows many charges under the cap. CIP before spend. The human is liable for API and MCP activity. No one-tap mandate UI was on the opened pages. Sandbox not confirmed.

AP2 v0.2. Human signs on a non-agentic Trusted Surface. Direct mode is one approval of the closed checkout. Autonomous mode is an earlier signed constraint bound to the agent’s public key, with a short `exp`. Verification is deterministic code. This is a spec, not a card issuer.

Visa pilot. Human approval happens before the agent runs: “already been approved by the buyer.” The agent does not decide. Tokenized credentials, invoice-tied path. Not a self-serve mint API on the opened release.

Lithic spend limits. Useful velocity control. The issuer’s own page says they are not precise and do not stop force-post or over-clear. Wrong tool for “exactly that amount.”

Ramp. Real no-money sandbox for partners. Card-mint pricing and limit fields were not opened.

Prompt-injection safeguards that the pages actually state: AP2’s non-agentic Trusted Surface and deterministic verification; Stripe’s Affirm “buyer must be the one navigating”; Stripe Issuing’s webhook that “You own”, so the approve/decline can sit outside the model; Visa’s “agent only executes eligible payments that have already been approved.” Privacy’s liability callout is the reverse: the account owner pays for what the API, CLI, or MCP does.

I. Q3 shortlist and recommendation

| Rail | Price / access opened | Cloud surface | Legal / terms risk | One line |
| --- | --- | --- | --- | --- |
| Stripe SPT | Preview. No per-token fee on the opened page. Countries listed above | `POST /v1/shared_payment/issued_tokens`, version `2026-09-30.preview`, Payment Element, webhooks | Preview terms. Agent must not drive Affirm’s loan UI. Seller must be the Stripe profile on the token | Best opened scoped credential when the seller is on Stripe |
| Stripe Issuing | $0.10 per virtual card in the virtual-card doc. Standard program, no sales call. Instant sandbox, simulated funds | Cards API, `spending_controls`, real-time auth webhook | PCI may apply if cards are for users. Merchant lock is private preview. Limits are not exact against later clearing | Use for a card a normal merchant already accepts, single-use, per-authorization cap, webhook outside the model |
| Privacy.com API | Consumer price [no source found]. CIP required | `POST /v1/cards`, `SINGLE_USE`, `spend_limit`, `spend_limit_duration` | Account owner is liable for API, CLI, and MCP spend. Sandbox not on the opened getting-started page | `SINGLE_USE` plus a cent limit is the close-after-first pattern. Do not expect a pre-chosen merchant id |
| Lithic | List price [no source found]. Spend-limit API opened | Card `spend_limit`. New programs pointed at Authorization Rules v2, which was not opened | Documented over-capture and force-post | Do not use the legacy spend limit as an exact-amount guarantee |
| Ramp sandbox | Price [no source found] | `https://demo-api.ramp.com`, no real money | Partner/developer sandbox. Card-mint API not opened | Safe place to test Ramp’s API. Not yet a sourced “mint this amount” rail |
| Visa Intelligent Commerce pilot | No public price. Live BILL + Billtrust pilot, 20 Oct 2026 | Trusted Agent Protocol, Agentic Directory, tokenized credentials. No self-serve API on the release | Pilot with participating portals. Agent executes an approval that already exists. Can replace a portal CAPTCHA only where the supplier participates | Right official rail for invoice portals in the pilot. Not a general checkout SDK |
| AP2 v0.2 | Open spec, no network fee printed | SD-JWT Checkout and Payment Mandates. Human on a non-agentic Trusted Surface | Spec. September 2025 blog still says Intent and Cart. No GA date on the spec page | Implement the human signature outside the agent. Do not treat the blog names and the spec names as the same object |
| Mastercard Agent Pay | Not opened | Only named inside Stripe’s SPT footnote | Unknown from primary pages | Wait until Mastercard’s own page loads. Do not build on snippets |

Overall Q3 recommendation. For a cloud developer on 29 October 2026, issue a Stripe Shared Payment Token with `usage_limits[max_amount]` and `expires_at` set in deterministic code, after the human has passed the Payment Element and any 3-D Secure step, and only to that seller’s Stripe profile. Where the merchant only accepts a normal card, mint a Stripe Issuing single-use virtual card in the instant sandbox first, with a `per_authorization` limit and a real-time authorization webhook the business owns, and treat merchant-id lock as private preview. Keep Visa Trusted Agent Protocol for portals that are actually in the pilot. Treat AP2 v0.2 as the open shape of the human mandate, with the signature on a non-agentic surface. Do not give the agent an open card. Confidence: [emerging]. Most important assumption: the seller is already on the same rail, so the agent never has to drive a checkout page and never holds a reusable card.

J. Disagreements, gaps, and coverage

Opened disagreements, left as disagreements:

Anchor’s official plan table is Free, Starter, Growth, Enterprise. A Team plan at $500 was not on that page.

Browserbase’s opened pricing page says Free has 3 concurrent browsers. A docs snippet saying 1 was not opened.

AP2’s 16 September 2025 blog uses Intent Mandate and Cart Mandate. The v0.2 spec uses Checkout Mandate and Payment Mandate, SD-JWT, and a Trusted Surface that MUST be non-agentic.

Stripe Issuing spending-controls defaults ($500 per day if unset, $10,000 per authorization, unconfigurable) and the standard business-program limits (25 cards per week, $5,000 per week, $500 per card per day) are both printed. They are different controls.

Privacy `TRANSACTION` allows many authorizations under the cap. `SINGLE_USE` closes after the first transaction. `MERCHANT_LOCKED` follows the first merchant that succeeds.

Lithic says spend limits are not precise. Stripe says aggregation can lag 30 seconds and later tips can exceed the limit. “Exactly that amount” is not what those issuers promise.

Not opened, so not used as findings: Mastercard Agent Pay for Machines press page; The Block article; OpenAI buy-in-ChatGPT page; Stripe seller SPT variant, ACP protocol, and Order Intents; Visa Intelligent Commerce Connect April 2026 release; Turnstile terms; X Corp. v. Bright Data docket; Privacy sandbox and consumer prices; Lithic price, merchant lock, and Authorization Rules v2; Ramp card-funds API and price; LangGraph Platform price; Mastra Cloud price; Cloudflare Browser Run price; stripe.com/pricing#issuing; later outcome of Meta v. Bright Data.

K. Proof of work

Searches run in this research session:

Anchor Browser human in the loop CAPTCHA pause live view pricing 2025 2026

Browserbase captcha solving human in the loop live view pricing API 2025 2026

Steel.dev browser agent CAPTCHA session pricing human handoff 2026

LangGraph interrupt human in the loop pause resume documentation 2025 2026

Cloudflare Agents human in the loop browser rendering CAPTCHA 2026

Mastra AI human in the loop suspend resume agent framework 2026

Visa Intelligent Commerce agent payments launch status 2025 2026 developer integration

Mastercard Agent Pay Agent Pay for Machines 2025 2026 launch API

Google Agent Payments Protocol AP2 mandates 2025 2026 documentation

OpenAI Stripe Agentic Commerce Protocol Order Intents Shared Payment Token 2025 2026

reCAPTCHA terms of service automated solving prohibited 2025 site:developers.google.com OR site:policies.google.com

hCaptcha terms of service automated access bots prohibited

Cloudflare Turnstile terms of service bot scraping prohibited 2025 2026

Meta Bright Data CFAA scraping ruling 2024 2025 2026 anti-bot case law

Stripe Issuing virtual cards spend controls merchant category sandbox pricing 2026 site:docs.stripe.com OR site:stripe.com

Privacy.com API single use card spend_limit merchant lock sandbox documentation

Lithic API virtual card spend limit merchant lock sandbox pricing 2026

Ramp developer API virtual cards programmatic issuance sandbox 2026

Stripe Shared Payment Token documentation maximum amount expiry agentic commerce 2026

Browserbase live view session debugger human takeover documentation 2026

Anchor Browser documentation live view human in the loop captcha bypass official docs

Google Agent Payments Protocol AP2 announcement September 2025 site:cloud.google.com OR site:blog.google OR site:ap2-protocol.org

Stripe Order Intents agentic commerce documentation 2026

Pages opened and used:

https://docs.anchorbrowser.io/pricing

https://docs.anchorbrowser.io/agentic-browser-control/human-in-the-loop

https://www.browserbase.com/pricing

https://docs.browserbase.com/platform/identity/captcha-solving

https://docs.browserbase.com/platform/browser/observability/session-live-view

https://docs.steel.dev/overview/pricinglimits

https://steel.dev/faq

https://docs.langchain.com/oss/javascript/langgraph/interrupts

https://mastra.ai/docs/workflows/human-in-the-loop

https://developers.cloudflare.com/agents/tools/browser/

https://blog.cloudflare.com/verified-bots-accountability/

https://www.hcaptcha.com/terms

https://developers.google.com/recaptcha/docs/faq

https://techcrunch.com/2024/01/24/court-rules-in-favor-of-a-web-scraper-bright-data-which-meta-had-used-and-then-sued/

https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.22856.html

https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol

https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md

https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens.md?agent-seller=agent

https://docs.stripe.com/issuing/controls/spending-controls

https://docs.stripe.com/issuing/cards/virtual

https://docs.stripe.com/issuing/direct

https://developers.privacy.com/docs/cards

https://developers.privacy.com/docs/getting-started

https://docs.lithic.com/docs/spend-limits

https://docs.ramp.com/llms-guides/sandbox.txt

Attempted and not used as sources: https://www.mastercard.com/us/en/news-and-trends/press/2026/october/Mastercard-Agent-Pay-for-Machines-in-action.html (Access Denied), https://www.theblock.co/news/business/2026-10-20-mastercard-opens-early-access-to-ai-agent-payments-following-pilots-with-coinbase-base-and-cloudflare-421406 (interstitial only), https://openai.com/index/buy-it-in-chatgpt/ (empty), https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens (index only; the agent variant above is the page that was read), https://docs.ramp.com/developer-api/v1/sandbox (JavaScript shell; the llms-guides text above is the page that was read), https://privacy-com.readme.io/docs/getting-started (redirect only; destination was then opened).

hCaptcha terms were read through section 7. The arbitration sections later in that long page were truncated and are not cited. The AP2 spec body that was read is the v0.2 page quoted above; linked subpages for checkout-mandate field tables were not separately opened.
