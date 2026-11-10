# DR26-10-29-ZB-01-1017 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
id: DR26-10-29-ZB-01-1017
rail: claudeai
url: https://приватный чат
mode: claude.ai Research, Opus 5.5 Medium (Fable 5.1 usage limit hit)
collected: 2026-11-02T12:43:34.634Z via Chrome (Claude-in-Chrome) ноутбук
unique_urls: 134
---

# Agent Fleet Report: Handling CAPTCHAs Legitimately (Q2) and Official Agent-Payment Rails (Q3), the 2025–2026 Delta

**Use APIs first. Where you have to drive a browser, get your agent recognised by identity, not by solving the challenge. When a CAPTCHA does appear, pause and hand the live session to a human.** Agent payments now have an official one-tap pattern ready to use: Stripe's Link Agent Wallet "spend requests" (agent proposes an amount and merchant → human approves in Link → a one-time card or token is minted for that purchase). The card-network protocols (Visa Intelligent Commerce/TAP, Mastercard Agent Pay, AP2, ACP) are real, but they are mostly reached *through* a PSP or issuer, not wired up directly by a small fleet.

## TL;DR
- **CAPTCHA (Q2):** For pause-and-handoff, use a cloud browser with live view and takeover: Cloudflare Browser Run "Human in the Loop" (beta, structured `Cloudflare.handoff` CDP command), or Browserbase or Steel live sessions. Drive the pause/resume from LangGraph `interrupt()` or Mastra `suspend()`. Turn off vendor auto-solving on sites you don't control. hCaptcha's terms explicitly ban bot/AI solving. The Ninth Circuit's August 4, 2026 *Amazon v. Perplexity* ruling lowered CFAA risk for user-directed agents, but it did not touch ToS, contract or platform-enforcement risk.
- **Recognised, not challenged:** Cloudflare folded "signed agents" into Verified bots on July 1, 2026 (Web Bot Auth, RFC 9421). Visa's Trusted Agent Protocol (Oct 2025, built with Cloudflare) uses the same signature approach for commerce. These are the legitimate ways around challenges. Most fleets will get them through a vendor that is already verified, not by registering their own bot.
- **Payments (Q3):** Stripe Link Agent Wallet spend requests are the only product found that does "agent builds cart → human one-tap → credential for that amount" end-to-end. Per the docs: amount, merchant and line items are shown, a 30-minute approval window applies, and credentials are one-time-use. For business-funded cards, Privacy.com (`SINGLE_USE` + `spend_limit_duration: TRANSACTION`), Lithic or Ramp Agent Cards (early access since March 11, 2026) give hard caps, but you have to build the approval tap yourself.

## Assumptions
- You (the operator) are both the account holder and the agent operator, acting on your own accounts at third-party sites, mostly in the US.
- "Opened" below means the page was fetched by me or by my research subagent. Where I relied only on a search-result snippet of a page, I tag it **[snippet]**. Vendor blogs and third-party explainers are tagged **[low-authority]**.
- Out of scope, as you instructed: models, policy engines, and bounded-budget design via Stripe Issuing/Ramp/Deel. Issuing and Ramp appear below only for the specific per-transaction and merchant-lock details you asked about.

---

# Q2 — CAPTCHA: legitimate handling and human-in-the-loop

## Key findings

**1. The 2025–2026 delta: HITL handoff moved from DIY live-view hacks to first-class APIs.** [established]
- **Cloudflare Browser Run** (the renamed Browser Rendering) now ships a "Human in the Loop" feature, marked **Beta**, last updated Jul 28, 2026. The page lists structured CDP commands `Cloudflare.handoff`, a `Cloudflare.handoffComplete` event, `Cloudflare.getHandoffState` and `Cloudflare.getLiveView`. It names CAPTCHA as a use case: "Login pages with MFA, SSO, or CAPTCHA that cannot be bypassed programmatically." Its stated purpose: "Human in the Loop lets a human step into a live browser session through Live View to handle what automation cannot, then hand control back to the script." (developers.cloudflare.com/browser-run/features/human-in-the-loop/, opened)
  - The same nav also lists "Guardrails (Beta)", "WebMCP (Beta)" and "Session recording (Beta)". That makes Cloudflare the most complete *infrastructure-level* handoff primitive found, though it is still beta.
- **LangGraph** `interrupt()`: "LangGraph saves the graph state using its persistence layer and waits indefinitely until you resume execution… you resume execution by re-invoking the graph using Command." The docs also note interrupt response schemas "require langgraph>=1.2.12". (docs.langchain.com/oss/python/langgraph/interrupts **[snippet]**) [established]
- **Mastra** (1.0 released): `requireToolApproval: true` pauses on every tool call, with `approveToolCall`/`declineToolCall` by `runId`, and workflows use `suspend()` / `run.resume({resumeData})` / `bail()`. Per the docs, "you can suspend a tool call before it executes so a human can approve or decline it." (mastra.ai/docs/agents/human-in-the-loop; mastra.ai/docs/workflows/human-in-the-loop **[snippet]**) [established]
- These orchestration frameworks don't render a browser. They hold agent state while a human acts in a live-view URL from a browser vendor. The working pattern is **framework interrupt + browser live-view URL sent to Slack/email + resume on human "done"**.

**2. Browser vendors: everyone has live view; most also sell auto-solving, which is where the ToS risk lives.** [established]
- **Browserbase:**
  - Live View with take-control and session replay.
  - Auto CAPTCHA solving on paid plans.
  - Pricing per a third-party tracker (checked Sep 26, 2026): Free $0; Developer $20/mo (100 browser-hours, then $0.12/hr, "Basic Stealth Mode + Auto captcha solving", 6-hour sessions); Startup $99/mo (500 hrs, then $0.10/hr); Scale custom. (toolradar.com/tools/browserbase **[snippet] [low-authority]**)
  - Browserbase states that it "does not use human-in-the-loop to solve captchas" (browserbase.com/blog/what-is-a-captcha-solver **[snippet] [low-authority]**).
  - **Disagreement:** sources differ on Free-plan concurrency. Toolradar says "1 concurrent browser"; humanbrowser.cloud and skyvern.com say 3. Toolradar also lists "Priority waitlist for Browserbase Agent Identity" on Scale, which suggests Browserbase is moving from "stealth" to "identity" [single-source].
- **Steel** (open-source, self-hostable):
  - Pricing per its docs: Launch/Scale/Enterprise tiers; CAPTCHA solves "$3 per 1,000 solves on Launch and $1 per 1,000 solves on Scale"; sessions up to 15 min (Launch), 1 hr (Scale), 24 hr (Enterprise); 10/100/1,000+ concurrency. (docs.steel.dev/overview/pricinglimits **[snippet]**)
  - Steel ships live-session embedding over WebRTC and an "Implement Human-in-the-Loop Controls" guide (docs.steel.dev/overview/sessions-api/embed-sessions **[snippet]**).
  - Its own CAPTCHA doc warns: "Even with successful CAPTCHA solving, respect the target site's rate limits and terms of service."
  - **Disagreement:** Steel's older `llms-full.txt` still shows a different tier ladder (Hobby $0 / Starter $29 / Developer $99 / Pro $499, captcha $4/1k–$3.5/1k). Treat the pricinglimits page as current.
- **Anchor Browser:**
  - Agent-level HITL: when enabled, "the agent sends its intervention requests to a session-specific queue" (docs.anchorbrowser.io/agentic-browser-control/human-in-the-loop **[snippet]**).
  - Also the most aggressive solving pitch: "The vision-based approach imitates human behavior to solve any CAPTCHA (including Cloudflare) without multiple challenges", requiring a proxy or profile (docs.anchorbrowser.io/advanced/captcha-solving **[snippet]**).
  - Its homepage lists a $50/mo tier with "Automated Captcha bypass" and also "Cloudflare Verified Browser Agents" (anchorbrowser.io **[snippet]**).
- **Newer entrants:**
  - **Human Browser** sells finished tasks with "$0.005 per CAPTCHA solve" and human takeover where "time while a human has taken over… is not billed" (humanbrowser.cloud **[snippet] [low-authority]**, competitor-written comparison pages).
  - **Notte** is "registered as a verified bot in Cloudflare's registry" and signs every request when `web_bot_auth=True` (docs.notte.cc **[snippet]**).
  - **Kernel** markets itself as a Browserbase alternative (kernel.sh **[low-authority]**).
- **Cloudflare Agents SDK** (Workers-based agent framework) approval flows: [no source found] in this research. I could only confirm Browser Run's handoff, not the Agents SDK's own HITL API surface.

**3. Legal / ToS risk of auto-solving on third-party sites: low criminal risk, real contractual and enforcement risk.** Each layer is a separate question:
- **Legality (CFAA):**
  - The district court enjoined Perplexity's Comet agent in March 2026, finding it accessed Amazon accounts "with the Amazon user's permission, but without authorization by Amazon" (geekwire.com **[snippet]**).
  - The Ninth Circuit stayed that order on March 16 and **vacated it on August 4, 2026**. It held "Amazon was unlikely to succeed on its Computer Fraud and Abuse Act (CFAA) and California Comprehensive Computer Data Access and Fraud Act (CDAFA) claims" (cooley.com **[snippet]**).
  - The reasoning: the Assistant "is a tool, not a person", and "the user, not Perplexity, was the one who 'accessed'" Amazon (ropesgray.com **[snippet]**). [emerging]
  - **Disagreement on the district-court date:** Cooley/Mogin say March 9, 2026; nohacks.co and CNBC (via nohacks) say March 10. This is probably order date vs. reporting date.
  - **What it means for you:** the ruling shifts CFAA "access" onto the human principal. In your setup, you are both principal and operator, so the ruling does *not* shield you. It just moves the question to whether *you* were authorised. The decision is a preliminary-injunction ruling in one circuit, on remand, and covers only CFAA/CDAFA. [emerging]
- **ToS breach (contract):**
  - **hCaptcha** is explicit. Its Master Terms (updated Nov 17, 2025) §7 forbid users to "perform any task with the use of Internet bots, web robots, bots, scripts, or any other form of artificial intelligence or otherwise attempt to pass challenges… without completing tasks as they are described", and to use "any proxy web server or other device designed to obfuscate your true location or identity" (hcaptcha.com/terms, opened by subagent). [established]
  - **Google APIs Terms** (governing reCAPTCHA integrators): "You will not misrepresent or mask either your identity or your API Client's identity" and no attempt to "circumvent" limitations (developers.google.com/terms, opened by subagent). These bind *integrators*, not end users who face the challenge. [established]
  - **Cloudflare:** no Turnstile-specific anti-solving clause found. The Website/Online Services Terms (effective Aug 1, 2025) prohibit trying to "exceed or circumvent… limitations" and obtaining information "through any means not intentionally made available" (cloudflare.com/website-terms/, opened by subagent). Whether these bind Turnstile solvers is unclear [single-source].
  - The bigger contractual exposure is usually the **target site's own ToS** (e.g., Amazon's Conditions of Use), which typically ban automated access. That stays live whatever the Ninth Circuit said about CFAA. Amazon also raised trademark/unfair-competition claims (nohacks.co **[snippet]**).
- **Civil liability:** the district court accepted that Amazon spent over $5,000 building tools to block Comet, which satisfied the CFAA loss threshold (courtdocket.org **[snippet] [low-authority]**). Disguising an agent as Chrome was central to Amazon's narrative (geekwire.com **[snippet]**). Masking is aggravating; declaring identity is mitigating. [emerging]
- **Platform enforcement:** account suspension, shadow-blocking and loss of verified status. Cloudflare's own Aug 4, 2025 blog post on Perplexity's "stealth, undeclared crawlers" stated: "we have de-listed them as a verified bot and added heuristics to our managed rules that block this stealth crawling" (quoted by Silicon Republic). On your *own* business accounts, a ban is the most likely and most costly outcome. [established]
- **Reputation:** being named as an evasive bot (the Perplexity precedent) hurts partner and payments relationships. Card-network agent programs require registration and verification (Mastercard: "requires agent registration and verification before any transactions can take place", mastercard.com LatAm press release **[snippet]**). [emerging]

**4. Official no-CAPTCHA paths.** [established]
- **General principle:** a CAPTCHA protects a UI meant for humans. If the service offers an authenticated API (OAuth/API key), using it removes the challenge entirely and is explicitly authorised. Prefer, in order: (a) official API or MCP server; (b) export/webhook/email-in channels; (c) signed-agent identity; (d) HITL browser handoff.
- **Concrete examples found in this research:**
  - Privacy.com ships an API, an MCP server and a CLI (developers.privacy.com nav, opened).
  - Stripe Link exposes `api.link.com` plus a CLI/SDKs. Its PR notes that with a Link Pay Token "Payment confirms without CVC or CAPTCHA" (github.com/stripe/link-cli/pull/188 **[snippet]**).
  - Mastercard and Visa supplier-portal pilots replace human portal logins: "many payment portals relied on human authentication requirements… AI agents can now be securely authenticated to credentialed portals" (Visa/BILL/Billtrust, Oct 20, 2026, usa.visa.com **[snippet]**).
  - Standard examples beyond these (Google Workspace/Gmail, Microsoft Graph, GitHub, Shopify Admin, AWS/GCP consoles → their APIs): [no source found in this research], but these are well-known official APIs.
- **Agent-identity schemes:**
  - **Cloudflare Web Bot Auth / Verified bots:** "As of July 1, 2026, the distinction between a Verified bot and a signed agent is expressed by a new metadata field tracked in BotBase: Direct versus Intermediary access" (developers.cloudflare.com/bots/concepts/bot/verified-bots/ **[snippet]**). You apply through the dashboard. Verification is "using one of… Web Bot Auth [or] IP validation" (github.com/cloudflare/cloudflare-docs **[snippet]**).
    - Cloudflare's own Verified bots policy doc has stated that "The minimum traffic should have more than 1,000 requests per day across multiple domains… A bot crawling one site is not valid". That policy URL now redirects to the main Verified bots page, which describes only "Honest self-identification" and "Non-abusive behavior", so the threshold may no longer be current. A small private fleet may not qualify on its own, so the practical route is a vendor that is already verified (Notte, Anchor's "Cloudflare Verified Browser Agents", Browserbase "Agent Identity"). [emerging]
  - **Visa Trusted Agent Protocol** (Oct 14, 2025), "developed in partnership with Cloudflare", is "built on the HTTP Message Signatures (RFC 9421) standard" and carries "Agent Intent". It is available "in the Visa Developer Center and GitHub" (corporate.visa.com **[snippet]**; eco.com **[low-authority]**). It is a merchant-side recognition scheme. Your agent benefits only if it is a registered agent at participating merchants. [emerging]

## Q2 shortlist

| Product | Price (as found) | Cloud-integration surface | Pause/resume + live view | Legal/ToS risk | One-line recommendation |
|---|---|---|---|---|---|
| Cloudflare Browser Run HITL | Workers Paid: 10 browser-hours/mo included, then $0.09/hr + $2.00 per extra concurrent browser; Free: 10 min/day (Cloudflare changelog, Jul 28, 2025) | Workers binding, CDP, Playwright/Puppeteer, Stagehand, MCP | `Cloudflare.handoff` → human in Live View → `handoffComplete` event | Low (no auto-solve advertised; Cloudflare-native identity) | **Primary pick** for structured handoff; beta, so pin versions |
| Browserbase | $0 / $20 / $99 / custom; $0.10–0.12/hr overage [low-authority] | Sessions API, CDP, Stagehand, MCP | Live View iframe with take-control; recordings | Medium if auto-solve left on (on by default on paid plans per reviewers) | Strong alternative; **disable auto-solve** for third-party sites |
| Steel | Launch/Scale; $3→$1 per 1k solves; 15 min–24 h sessions | Sessions API, CDP, SDKs; **self-hostable** | WebRTC live embed + HITL guide | Medium (solving opt-in via `solve_captcha`) | Best if you want open-source/self-host and long sessions |
| Anchor Browser | $50/mo tier [snippet] | Sessions API, agentTask, profiles | Agent HITL queue | **High** with "bypass any CAPTCHA (incl. Cloudflare)" enabled | Use its HITL and verified-agent features only; do not enable solver |
| LangGraph `interrupt` | OSS (platform pricing not researched) | Python/JS; LangSmith server API | Checkpointed pause, `Command(resume=…)` | None (orchestration only) | Orchestration layer of choice if Python |
| Mastra | OSS | TypeScript; runs on Workers/Vercel | `suspend()`/`resume()`, `requireToolApproval` | None | Orchestration layer of choice if TS/Cloudflare |
| Notte (Web Bot Auth) | [no source found] | Sessions API, `web_bot_auth=True` | — | Low (declared identity) | Use when target is Cloudflare-fronted and allows verified agents |
| Human Browser | $0.005/solve, $0.10/hr [low-authority] | A2A + MCP tasks | viewer_url takeover, unbilled | Medium–High (auto-solve) | Watch, don't adopt yet (single-source claims) |

**Q2 overall recommendation:**
1. Make API-first the default.
2. For unavoidable UI work, run Cloudflare Browser Run (or Browserbase/Steel) with **auto-solving off**, orchestrated by LangGraph `interrupt()` or Mastra `suspend()`. On any challenge, emit a live-view URL to a human and resume on completion.
3. Prefer vendors with Cloudflare-verified/signed identity, and never mask the agent as a human browser.

**Confidence: medium-high.** **Key assumption:** the target sites' ToS don't prohibit automated access to your own account outright. Where they do, even HITL-assisted automation is a ToS breach, and the only clean path is the official API or no automation.

---

# Q3 — Agent payments: official rails, the 2025–2026 delta

## Key findings

**1. Network and protocol layer: launch status as of Nov 2, 2026.**

- **Visa Intelligent Commerce (VIC) + Trusted Agent Protocol (TAP).**
  - TAP was launched Oct 14, 2025 (corporate.visa.com **[snippet]**). As of December 2025 Visa reported "100+ ecosystem partners, 30+ projects in the Visa Intelligent Commerce sandbox" (eco.com **[low-authority]**; corroborated by techinformed.com **[snippet]**).
  - **Intelligent Commerce Connect** (Apr 8, 2026) is "a network, protocol, and token vault-agnostic 'on ramp'". It enables "secure payment initiation, tokenization, spend controls, and authentication" and accepts TAP, MPP, ACP and UCP. It is "Currently in pilot with select partners including Aldar, AWS, Diddo, Highnote, Mesh, Payabli, Sumvin" (usa.visa.com press release 22276, opened). [emerging]
  - The first live B2B pilot (Visa/BILL/Billtrust, Oct 20, 2026) uses an agent "registered using Visa's Trusted Agent Protocol and Agentic Directory" with "tokenized payment credentials and controls aligned with the buyer's approved payment instructions" (usa.visa.com press release 22856 **[snippet]**).
  - **Integration:** via the Visa Acceptance Platform/Developer Center or an enabler (Stripe, Ramp). Not a self-serve developer API for a small fleet. [emerging]
- **Mastercard Agent Pay / Agent Pay for Machines.**
  - Agent Pay was announced Apr 29, 2025. It uses Agentic Tokens (MDES network tokens bound to an agent and consent policy) (eco.com, finance-loop.net **[low-authority]**).
  - The Acceptance Framework (Oct 2025) "requires agent registration and verification before any transactions can take place" (mastercard.com LatAm release **[snippet]**).
  - **Agent Pay for Machines** launched June 10, 2026 with 30+ partners (Adyen, Cloudflare, Coinbase, Stripe, Tempo…). It "supports credentialing, controls and guaranteed settlement across multiple payment types, from cards to stablecoins" (mastercard.com press release **[snippet]**).
  - In October 2026 Mastercard said: "Developers can access the first version of our buyer- and seller-side APIs through Agent Pay for Machines Early Access, along with the toolkit, sandbox and developer documentation", and agents transact "within approved budgets and permissions" (mastercard.com Oct 2026 **[snippet]**).
  - Verifiable Intent (Mar 5, 2026, with Google) was donated to the FIDO Alliance on Apr 28, 2026 (agentspay.ai **[low-authority] [single-source]**).
  - **Disagreement on firsts:** "first live agentic payment" is claimed for Singapore (Mar 4, 2026; aiautomationglobal **[low-authority]**), Santander Europe (March 2026), and ING/Worldline (June 2, 2026; finance-loop.net). These are regional firsts, not contradictions. [emerging]
- **Google AP2.**
  - AP2 was announced Sep 16, 2025 on the Google Cloud blog as "an open protocol developed with leading payments and technology companies" that "can be used as an extension of the Agent2Agent (A2A) protocol and Model Context Protocol (MCP)". Named partners include Adyen, American Express, Coinbase, Mastercard, PayPal, Revolut, Salesforce and Worldpay (Technology Magazine).
  - It defines three signed Verifiable Credentials: Intent Mandate ("human-not-present"), Cart Mandate ("the user's final, explicit authorization for a specific cart, including the exact items and price"), and Payment Mandate (signals agent involvement to the network/issuer) (ap2-protocol.net **[snippet]**).
  - In human-present flows, "their device cryptographically signs both the Cart Mandate for the merchant and the Payment Mandate for the network" (vellum.ai **[low-authority]**).
  - It is an authorization layer, not a rail: "It proves an agent is allowed to spend, but doesn't move money" (github.com/goodmeta/ap2-example **[snippet]**).
  - **Status:** open spec plus reference implementations; production settlement depends on PSP/network adoption. [emerging]
- **OpenAI/Stripe Agentic Commerce Protocol (ACP) and Stripe Shared Payment Tokens.**
  - ACP was released Sep 29, 2025 under Apache 2.0 (eco.com **[low-authority]**).
  - **March 2026 delta:** OpenAI wrote, "the initial version of Instant Checkout did not offer the level of flexibility that we aspire to provide, so we're allowing merchants to use their own checkout experiences while we focus our efforts on product discovery", and is "expanding the Agentic Commerce Protocol (ACP) to support product discovery" (openai.com/index/powering-product-discovery-in-chatgpt/, opened by subagent).
  - **Disagreement:** TechInformed (April 2026 **[snippet]**) still describes Instant Checkout as "currently limited to approved partners". Treat Instant Checkout as de-emphasised, not as a dependable rail. [established for the scale-back]
  - Stripe's **Shared Payment Tokens** (public preview) "grant sellers scoped access to customers' payment methods for agent-initiated purchases… each with usage and expiration limits". Stripe "might use tokens issued through network programs such as Mastercard's Agent Pay, Visa's Intelligent Commerce, and American Express ACE Kit" (docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens, opened index + **[snippet]**).
  - API resources `SharedPaymentIssuedToken` (create/retrieve/revoke) and `SharedPaymentGrantedToken` with `usage_limits` shipped in the 2026-04-22 "dahlia" changelog (docs.stripe.com/changelog **[snippet]**). **Stripe is the practical on-ramp to all three networks' agent tokens.** [established]
- **"Order Intents":** [no source found] for a product by that exact name in this research.

**2. Virtual-card APIs: hard per-transaction limits and merchant lock.**

- **Privacy.com.** Exact API (developers.privacy.com/docs/cards, opened, modified 2026-04-03):
  - `POST https://api.privacy.com/v1/cards` with `type` ∈ `SINGLE_USE`, `MERCHANT_LOCKED`, `DIGITAL_WALLET`.
  - `spend_limit` is in cents: "Transaction requests above the spend limit will be declined. The Spend-Limit value has to be a whole dollars (in cents)."
  - `spend_limit_duration` ∈ `TRANSACTION`, `MONTHLY`, `ANNUALLY`, `FOREVER`.
  - `state` ∈ `OPEN`, `PAUSED`; the response includes `hostname` ("Hostname of card's locked merchant").
  - Per the CLI docs, `SINGLE_USE` "Closes automatically after the first transaction" and `MERCHANT_LOCKED` is "Locked to the first merchant it's used with". Wrong-merchant attempts decline as `UNAUTHORIZED_MERCHANT` (developers.privacy.com/docs/privacy-cli and /transactions **[snippet]**).
  - **Important nuance:** the lock binds on *first use*. You cannot pre-designate the merchant. A prompt-injected agent could "lock" a fresh card to the wrong merchant. `SINGLE_USE` + `TRANSACTION` limit = exact-amount, one-shot card.
  - **Gotcha:** with `TRANSACTION` duration on a non-single-use card, "Card will authorize multiple transactions if each individual transaction is under the spend limit" (developers.privacy.com/docs/cards **[snippet]**).
  - **Sandbox:** `https://sandbox.privacy.com/v1` with simulate endpoints, per third-party skills/wrappers (github.com/dylanfeltus/skills; github.com/nickcheek/privacy-wrapper **[low-authority]**). Not confirmed on an opened official page.
  - **Pricing:** the personal plan allows "up to 12 new cards per month" (privacy.com/pricing **[snippet]**). Higher tiers not captured.
  - **Access:** API access reportedly requires emailing support (remoteopenclaw.com **[low-authority]**).
  - Privacy also ships an official **MCP server** and **CLI**. [established for API fields]
- **Lithic:**
  - Card-level `spend_limit` + `spend_limit_duration` (same enum as Privacy), plus Auth Rules for "merchant category, transaction amount, velocity, time of day" (lithic.com/card-issuing **[snippet]**).
  - Auth Stream Access lets "customers… build unique rules for each card", i.e., real-time per-authorization decisions (lithic.com/blog/agentic-payments **[snippet]**).
  - **True sandbox:** `sandbox.lithic.com` with simulation endpoints and documented sandbox rate limits ("In Sandbox… read operations is 15 RPS and for write operations is 1 RPS") (docs.lithic.com/docs/rate-limits **[snippet]**).
  - Write access needs an Issuing API key ("Write access requires an Issuing API key. Reach out at lithic.com/contact", docs.lithic.com/reference/postcards **[snippet]**).
  - Per a third-party crosswalk, `MERCHANT_LOCK` auth rules "cannot be created or updated through this API" (huggingface.co **[low-authority] [single-source]**).
  - **Pricing:** [no source found].
- **Stripe Issuing:**
  - "$0.10 per virtual card" and "$3.50 per standard, physical card" (stripe.com/pricing **[snippet]**).
  - Sandbox available. Reviewers report a "default of $500 per day" limit if none is set (lilachbullock.com **[low-authority]**) and that "Issuing for agents" is in private preview (nevermined.ai **[low-authority]**).
  - Stripe documents converting an Issuing card into an SPT for MPP/UCP sellers (docs.stripe.com/issuing/agents/programmatic-checkout **[snippet]**).
  - Per-authorization limits, MCC allow/block lists and the real-time `issuing_authorization.request` webhook: [no source found in opened pages], although they are standard Issuing features. Approval/eligibility for US businesses has reportedly varied over time (merchantmaverick says contact sales; Stripe's 2020 blog said open self-serve). **Disagreement, unresolved.**
- **Ramp Agent Cards:**
  - Launched Mar 11, 2026 ("Ramp Agent Cards give agents the ability to spend, governed with real spend limits, merchant controls", x.com/RampLabs **[snippet]**). Currently "in early access" as "tokenized credentials issued by Visa Intelligent Commerce", with "Instant card issuance… via API" and "Merchant-level controls: Lock each card to a specific vendor" (ramp.com/blog/virtual-cards-for-ai-agents **[snippet]**).
  - Ramp is the only one of the card issuers here that advertises *pre-designated* vendor lock natively.
  - **Pricing:** [no source found]. Ramp's platform is typically free to businesses.
  - **Sandbox:** [no source found].
- **Newer:** Mercury Agent Cards (Aug 2026): "A human must create the Agent Card, and the agent can't bypass or raise its own limit" (pymnts.com **[snippet]**).

**3. The "agent builds cart → human one-tap → card minted for exactly that amount" pattern.**
- **Native: Stripe Link Agent Wallet spend requests** (docs.stripe.com/agentic-commerce/link-cli/use-link-wallet-pay-online, opened). [established, preview-stage product]
  - **Flow:** "Your agent creates a spend request with the amount, the seller, and a description of the purchase. The customer approves it on the Link website or in the mobile app… Your agent retrieves the credential and pays the seller."
  - **Approval screen:** `--line-item` and `--total` populate it, and `--context` "must be at least 100 characters, and it's what your customer reads when deciding whether to approve."
  - **Credential types:** `card` (default), `shared_payment_token`, or `link_pay_token`.
  - **Timing:** "Customers have 30 minutes from the approval request to respond, after which the status becomes `expired`."
  - **Anti-leak safeguard:** "write card details directly to a file rather than returning them to stdout, where they can persist in model transcripts and application logs". The file is created with `0600` permissions.
  - **Retries:** idempotency keys per purchase intent.
  - **Testing:** a `--test` mode.
  - **Scope:** "Agent payments are available to US and Canadian customers."
  - **Limits per a fork of the repo [low-authority]:** max $500 per spend request, $500 daily, $20,000/30 days, credentials valid 12 hours (github.com/NeotaskInc/stripe-link-cli **[snippet]**). Verify against the official repo before relying on them.
  - **Fit:** this is consumer-wallet-funded (Link). It fits a founder's own card, not a corporate program.
- **Protocol-native:** AP2's human-present **Cart Mandate** is the same pattern at the spec level: the user's device signs the exact cart and price. Production availability depends on the PSP. [emerging]
- **Mastercard Agent Pay for Machines:** a plan or budget approval up front (the Cloudflare Connect demo: the agent "returns with a two-step plan" the business approves, then runs "nine payments, two independent merchants and one budget"). Budget-level, not per-transaction (mastercard.com Oct 2026 **[snippet]**; pymnts.com **[snippet]**). [emerging]
- **Build-it-yourself:** with Privacy.com, Lithic or Stripe Issuing, the agent proposes a cart. Your approval UI (Slack button/passkey) mints `SINGLE_USE` with `spend_limit = cart total` (Privacy), or a card with a per-authorization limit + MCC restriction (Lithic/Issuing). The card is then closed. Privacy's whole-dollar `spend_limit` rule means you must round up, so the cap is never exactly the cart amount. [established components; integration is yours]
- **Fraud and prompt-injection safeguards by product:**
  - **Link:** the human sees merchant, line items and context. Credentials are one-time and short-lived, the real PAN is never exposed, and there is a file-only PAN option.
  - **SPT:** seller-scoped, with usage/expiry limits and revocable.
  - **Privacy:** single-use auto-close and cap-decline. Its lock-on-first-use is weak against injection.
  - **Lithic/Issuing:** real-time auth decisioning (ASA / webhook), so you can check merchant descriptor = approved merchant at authorization time. This is the strongest defence against a hijacked agent paying the wrong merchant.
  - **Ramp:** pre-set vendor lock + tokenized credential, no PCI exposure.
  - **Networks (TAP/Agent Pay/AP2):** registered agent identity + signed intent shift dispute liability. AP2 explicitly frames liability around whether the cart matched the signed mandate (allium.so **[low-authority]**).

## Q3 shortlist

| Product | Price | Cloud-integration surface | Human approval model | Hard limit / merchant lock | Sandbox | Legal/ToS risk | One-line recommendation |
|---|---|---|---|---|---|---|---|
| Stripe Link Agent Wallet (spend requests) | [no source found] (agent side needs no Stripe account) | CLI, Node/Go SDK, HTTP (`api.link.com`), OAuth scope `payment_methods.agentic`, MCP via skills | **Per-transaction tap** in Link app/web, 30-min window | One-time credential for approved amount + named merchant | `--test` mode | Low (official, user-approved); US/CA only | **Default for the one-tap pattern** |
| Stripe Shared Payment Tokens / ACP | Stripe processing fees; SPT in public preview | Stripe API (`/v1/shared_payment/issued_tokens`), ACP endpoints | Agent/platform-defined; token carries usage limits | Seller-scoped, max amount, expiry | Test helpers | Low; preview ToS applies | Use where sellers accept SPT/MPP; on-ramp to network tokens |
| Privacy.com API | Personal plan 12 cards/mo; paid tiers not captured | REST, CLI, MCP server | None native, build your own | `SINGLE_USE` + `TRANSACTION`; lock-on-first-use; whole-dollar caps | Yes (sandbox.privacy.com) [low-authority] | Low; check consumer-account ToS for business use | Cheapest exact-cap one-shot cards for a small fleet |
| Lithic | [no source found] | REST, Auth Rules v2, Auth Stream Access webhooks | None native, build your own | Spend limits, MCC/velocity rules, real-time auth decisions | **Yes, true sandbox** with simulation | Low; program approval required | Best for engineering-led, injection-resistant control |
| Stripe Issuing | $0.10/virtual card | REST, webhooks, Agent toolkit, SPT conversion | None native, build your own | Spending controls + real-time auth webhook | Yes | Low; eligibility review | Use if already on Stripe (bounded-budget design already settled) |
| Ramp Agent Cards | [no source found] | API, MCP, CLI (`ramp agent-card`) | Policy-based, not per-tap | Pre-designated vendor lock, per-agent limit, Visa VIC tokens | [no source found] | Low; early access | Best corporate option once GA |
| Visa Intelligent Commerce / ICC | Not public | Visa Acceptance Platform; via enablers | Spend controls + authentication (passkey-style) | Token-level controls | VIC sandbox (partners) | Low; partner-gated | Consume indirectly (Stripe/Ramp); don't integrate directly |
| Mastercard Agent Pay (for Machines) | Not public | Early Access buyer/seller APIs, toolkit, sandbox | Budget/plan approval; consumer-set limits | Agentic Token bound to agent + scope + limit | Yes (Early Access) | Low; registration required | Pilot only if you sell machine services; else consume via Stripe |
| Google AP2 | Free spec | Open-source reference impl. (A2A/MCP) | Human-present signed Cart Mandate; delegated Intent Mandate | Mandate constraints (cap, merchant, TTL) | Reference code | Low | Adopt the mandate *shape* for your audit log; no direct rail |

**Q3 overall recommendation:**
- Use **Stripe Link Agent Wallet spend requests** for purchases made with a founder's personal or own card. It delivers the exact one-tap, amount-bound, one-time-credential pattern with the safeguards already built.
- For business-funded spend, mint per-purchase cards from **Lithic** (best real-time control and sandbox) or **Privacy.com** (cheapest, simplest), behind your own approval tap. Verify merchant descriptor at authorization time where possible.
- Reach Visa, Mastercard and AP2 tokens **through Stripe SPTs or Ramp** rather than integrating directly.

**Confidence: medium.** Link Agent Wallet and SPTs are preview-stage, and the network programs are pilot or early-access. **Key assumption:** your purchases are US/Canada and card-acceptable, and you're comfortable with a consumer-wallet (Link) approval surface. If not, the build-your-own approval path with Lithic/Privacy is the fallback.

---

## Caveats and gaps
- **Sourcing limits:** many vendor pricing figures came from search snippets or third-party trackers, not opened official pricing pages. Re-verify Browserbase, Steel, Anchor and Privacy tiers before purchase.
- **Unresolved items:** Cloudflare Agents SDK approval APIs, Lithic and Ramp pricing, and an "Order Intents" product are [no source found]. Cloudflare Browser Run pricing now comes from Cloudflare's Jul 28, 2025 changelog: Workers Paid includes 10 browser-hours/month, then $0.09 per extra browser-hour and $2.00 per extra concurrent browser.
- ***Amazon v. Perplexity*** is a preliminary-injunction appeal, remanded, Ninth Circuit only. It is not a merits ruling, and it does not address ToS/contract claims.
- **Protocol churn is high:** TAP, Agent Pay, AP2 and ACP each changed scope in 2025–2026 (e.g., Instant Checkout's March 2026 retreat; signed agents merged into Verified in July 2026). Re-check quarterly.

## Proof of work
**Searches run (18):**
1. Browserbase live view human in the loop captcha pricing
2. Anchor Browser human in the loop captcha
3. Steel.dev captcha solving sessions pricing
4. Browserbase docs live view human takeover session pricing plans
5. LangGraph interrupt human-in-the-loop Command resume docs
6. Visa Intelligent Commerce Trusted Agent Protocol developer launch 2026
7. Mastercard Agent Pay agentic tokens launch status 2026
8. Agent Payments Protocol AP2 mandates intent cart mandate GitHub
9. Mastercard "Agent Pay for Machines" press release
10. Stripe Agentic Commerce Protocol shared payment token docs
11. Stripe link-cli spend request customer approves agent payment credentials
12. Privacy.com API spend_limit_duration TRANSACTION merchant locked card sandbox
13. Amazon v. Perplexity Comet preliminary injunction computer fraud 2026
14. Cloudflare Web Bot Auth signed agents verified bots docs
15. Lithic pricing sandbox virtual cards API spend limits auth rules
16. Stripe Issuing pricing virtual card fee spending_controls sandbox
17. Ramp agent cards AI agents virtual card launch 2026
18. Mastra suspend resume / Cloudflare Agents SDK human-in-the-loop

Three further searches (OpenAI Instant Checkout status, hCaptcha/Turnstile terms, Cloudflare Agents SDK) failed after the search budget ran out. The first two were covered by the subagent.

**Pages fully opened (fetched):**
- developers.cloudflare.com/browser-run/features/human-in-the-loop/
- usa.visa.com/about-visa/newsroom/press-releases.releaseId.22276.html
- docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens (index)
- docs.stripe.com/agentic-commerce/link-cli/use-link-wallet-pay-online
- developers.privacy.com/docs/cards

**Opened by research subagent:**
- hcaptcha.com/terms
- developers.google.com/terms
- cloudflare.com/sv-se/terms
- cloudflare.com/website-terms/
- openai.com/index/powering-product-discovery-in-chatgpt/

**All other citations are search-result snippets, tagged [snippet].**

## Sources (web results returned during research)
- https://api.privacy.com/v1/cards`
- https://sandbox.privacy.com/v1`
- https://www.google.com/s2/favicons?sz=64&domain=cloudflare.com
- https://developers.cloudflare.com/browser-run/features/human-in-the-loop/
- https://www.google.com/s2/favicons?sz=64&domain=langchain.com
- https://docs.langchain.com/oss/python/langgraph/interrupts
- https://docs.langchain.com/oss/python/langgraph/interrupts%20%20
- https://www.google.com/s2/favicons?sz=64&domain=mastra.ai
- https://mastra.ai/docs/v1/agents/human-in-the-loop-with-tools
- https://mastra.ai/docs/workflows/human-in-the-loop
- https://mastra.ai/docs/agents/human-in-the-loop
- https://www.google.com/s2/favicons?sz=64&domain=toolradar.com
- https://toolradar.com/tools/browserbase
- https://www.google.com/s2/favicons?sz=64&domain=browserbase.com
- https://www.browserbase.com/blog/what-is-a-captcha-solver
- https://www.google.com/s2/favicons?sz=64&domain=skyvern.com
- https://www.skyvern.com/blog/browser-use-vs-browserbase-comparison-reviews-and-alternatives/
- https://www.google.com/s2/favicons?sz=64&domain=steel.dev
- https://docs.steel.dev/overview/pricinglimits
- https://docs.steel.dev/llms.txt
- https://docs.steel.dev/overview/pricinglimits.md
- https://docs.steel.dev/overview/sessions-api/embed-sessions
- https://docs.steel.dev/overview/stealth/captcha-solving
- https://docs.steel.dev/llms-full.txt
- https://www.google.com/s2/favicons?sz=64&domain=anchorbrowser.io
- https://docs.anchorbrowser.io/agentic-browser-control/human-in-the-loop
- https://docs.anchorbrowser.io/advanced/captcha-solving
- https://anchorbrowser.io/
- https://www.google.com/s2/favicons?sz=64&domain=humanbrowser.cloud
- https://humanbrowser.cloud/vs/browserbase
- https://www.google.com/s2/favicons?sz=64&domain=notte.cc
- https://docs.notte.cc/features/sessions/cloudflare-web-bot-auth
- https://www.google.com/s2/favicons?sz=64&domain=kernel.sh
- https://www.kernel.sh/ai-library/browserbase-alternatives-2026
- https://www.google.com/s2/favicons?sz=64&domain=geekwire.com
- https://www.geekwire.com/2026/judge-blocks-perplexitys-ai-bot-from-shopping-on-amazon-in-early-test-of-agentic-commerce/
- https://www.google.com/s2/favicons?sz=64&domain=cooley.com
- https://www.cooley.com/news/insight/2026/2026-08-06-ninth-circuit-rules-on-ai-agent-access-to-third-party-websites-under-cfaa
- https://www.google.com/s2/favicons?sz=64&domain=courtdocket.org
- https://courtdocket.org/amazon-vs-perplexity-ai-lawsuit-comet-injunction-and-appeal/
- https://www.google.com/s2/favicons?sz=64&domain=ropesgray.com
- https://www.ropesgray.com/en/insights/alerts/2026/08/tool-or-intruder-what-amazon-v-perplexity-means-for-agentic-ai-and-the-cfaa
- https://www.google.com/s2/favicons?sz=64&domain=moginlawllp.com
- https://moginlawllp.com/courts-weigh-computer-access-in-age-of-agentic-ai-in-amazon-perplexity-case/
- https://www.google.com/s2/favicons?sz=64&domain=nohacks.co
- https://nohacks.co/blog/amazon-perplexity-cfaa-agent-visitor-rights
- https://www.google.com/s2/favicons?sz=64&domain=hcaptcha.com
- https://www.hcaptcha.com/terms
- https://www.google.com/s2/favicons?sz=64&domain=google.com
- https://developers.google.com/terms
- https://www.cloudflare.com/website-terms/
- https://www.google.com/s2/favicons?sz=64&domain=mastercard.com
- https://www.mastercard.com/news/latin-america/en/newsroom/press-releases/pr-en/2025/december/mastercard-unveils-agent-pay-in-latin-america-and-the-caribbean/
- https://www.google.com/s2/favicons?sz=64&domain=privacy.com
- https://developers.privacy.com/docs/cards
- https://www.google.com/s2/favicons?sz=64&domain=github.com
- https://github.com/stripe/link-cli/pull/188/files
- https://www.google.com/s2/favicons?sz=64&domain=stripe.com
- https://docs.stripe.com/agentic-commerce/link-agent-wallet
- https://www.google.com/s2/favicons?sz=64&domain=visa.com
- https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.22856.html
- https://github.com/cloudflare/cloudflare-docs/blob/production/src/content/docs/bots/concepts/bot/verified-bots/index.mdx
- https://developers.cloudflare.com/bots/concepts/bot/verified-bots/
- https://www.google.com/s2/favicons?sz=64&domain=eco.com
- https://eco.com/support/en/articles/14845484-visa-intelligent-commerce-explained
- https://www.google.com/s2/favicons?sz=64&domain=crowdfundinsider.com
- https://www.crowdfundinsider.com/2025/10/254629-visa-agentic-commerce-solution-to-enable-secure-communication-between-ai-agents-and-merchants/
- https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-unveils-trusted-agent-protocol-for-ai-commerce.html
- https://www.google.com/s2/favicons?sz=64&domain=techinformed.com
- https://techinformed.com/visa-opens-one-integration-for-ai-agent-payments/
- https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.22276.html
- https://eco.com/support/en/articles/15192001-what-is-mastercard-agent-pay-ai-agent-commerce-protocol-in-2026
- https://www.google.com/s2/favicons?sz=64&domain=finance-loop.net
- https://www.finance-loop.net/mastercard-agent-pay/
- https://www.google.com/s2/favicons?sz=64&domain=genfinity.io
- https://genfinity.io/2026/06/10/mastercard-agent-pay-for-machines-launch/
- https://www.mastercard.com/global/en/news-and-trends/press.html
- https://www.mastercard.com/us/en/news-and-trends/press/2026/june/mastercard-launches-agent-pay-for-machines.html
- https://www.google.com/s2/favicons?sz=64&domain=marketchameleon.com
- https://marketchameleon.com/PressReleases/i/2320039/MA/mastercard-launches-agent-pay-for-machines-to
- https://www.mastercard.com/global/en/news-and-trends/press/2026/october/Mastercard-Agent-Pay-for-Machines-in-action.html
- https://www.google.com/s2/favicons?sz=64&domain=agentspay.ai
- https://agentspay.ai/vs/mastercard-agent-pay
- https://www.google.com/s2/favicons?sz=64&domain=aiautomationglobal.com
- https://aiautomationglobal.com/blog/mastercard-agent-pay-agentic-payments-live
- https://www.google.com/s2/favicons?sz=64&domain=vellum.ai
- https://www.vellum.ai/blog/googles-ap2-a-new-protocol-for-ai-agent-payments
- https://github.com/goodmeta/ap2-example
- https://eco.com/support/en/articles/14845478-acp-agentic-commerce-protocol-explained
- https://www.google.com/s2/favicons?sz=64&domain=openai.com
- https://openai.com/index/powering-product-discovery-in-chatgpt/
- https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens?agent-seller=seller&locale=en-US
- https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens
- https://docs.stripe.com/changelog/dahlia/2026-04-22/shared-payment-granted-token
- https://docs.stripe.com/changelog/dahlia/2026-04-22/shared-payment-issued-token
- https://developers.privacy.com/docs/privacy-cli
- https://developers.privacy.com/docs/transactions
- https://github.com/dylanfeltus/skills/blob/main/privacy-cards/SKILL.md
- https://github.com/nickcheek/privacy-wrapper
- https://www.privacy.com/pricing
- https://www.google.com/s2/favicons?sz=64&domain=remoteopenclaw.com
- https://www.remoteopenclaw.com/skills/johnielee/privacy-cards
- https://www.google.com/s2/favicons?sz=64&domain=lithic.com
- https://www.lithic.com/card-issuing
- https://www.google.com/s2/favicons?sz=64&domain=huggingface.co
- https://huggingface.co/datasets/Agentic-Payment/agent-spending-controls-crosswalk
- https://www.lithic.com/blog/agentic-payments
- https://docs.lithic.com/docs/rate-limits
- https://docs.lithic.com/reference/postcards
- https://stripe.com/pricing
- https://www.google.com/s2/favicons?sz=64&domain=nevermined.ai
- https://nevermined.ai/blog/stripe-issuing-review
- https://www.google.com/s2/favicons?sz=64&domain=lilachbullock.com
- https://www.lilachbullock.com/stripe-issuing/
- https://docs.stripe.com/issuing/agents/programmatic-checkout
- https://www.google.com/s2/favicons?sz=64&domain=merchantmaverick.com
- https://www.merchantmaverick.com/reviews/stripe-corporate-card-review/
- https://stripe.com/blog/issuing-self-serve
- https://www.google.com/s2/favicons?sz=64&domain=ramp.com
- https://ramp.com/blog/virtual-cards-for-ai-agents
- https://www.google.com/s2/favicons?sz=64&domain=x.com
- https://t.co/MZePxVJTrK
- https://x.com/RampLabs/status/2031792565066891555
- https://www.google.com/s2/favicons?sz=64&domain=pymnts.com
- https://www.pymnts.com/news/artificial-intelligence/2026/ai-agents-just-got-their-own-company-credit-cards/
- https://docs.stripe.com/agentic-commerce/link-cli/use-link-wallet-pay-online
- https://github.com/NeotaskInc/stripe-link-cli
- https://www.google.com/s2/favicons?sz=64&domain=ap2-protocol.net
- https://ap2-protocol.net/en/
- https://www.pymnts.com/news/artificial-intelligence/2026/mastercard-debuts-tool-to-help-ai-agents-purchase-digital-services/
- https://www.google.com/s2/favicons?sz=64&domain=stabledash.com
- https://stabledash.com/news/2026-03-11-ramp-launches-agent-cards-to-enable-secure-autonomous-ai-spending
- https://www.google.com/s2/favicons?sz=64&domain=allium.so
- https://www.allium.so/blog/ap2-googles-agent-payments-protocol/
