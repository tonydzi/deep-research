# DR26-10-29-ZB-01-1017 — the exact prompt sent to every model

# DR26-10-29-ZB-01-1017 — Агенты: капча HITL-продукты и официальные платёжные рельсы (дельта 2025-2026, Q2+Q3 добор к ZB-01-1315)

Conduct a deep research investigation. Write the final report in English. Research date: 29 October 2026. Use the strongest available research/reasoning mode with live web search. Do not ask clarification questions; state reasonable assumptions. Do not use private connected workspace data.

CONTEXT. We run a fleet of autonomous AI agents for our own accounts and our own business. We want to remove self-imposed harness friction and move fast, but WITHOUT defeating other sites' protections and WITHOUT giving a bot unattended free rein over a payment card. Survey publicly-sold or open-source PRODUCTS and official rails. Describe capabilities, pricing, cloud-integration surface, and legal/ToS risk. Do NOT write step-by-step instructions for defeating any specific website's bot-protection. 

SCOPE NOTE (avoid re-researching what we already know): bounded-budget agent spending via Stripe Issuing / Ramp / Deel and "policy engine outside the LLM" are already settled for us. The MODELS question (which low-over-refusal models to use) is ALSO already answered in a separate report, do NOT cover it. Research ONLY the two areas below, and give the 2025-2026 DELTA plus concrete product specifics.

Search the web extensively and cite ONLY URLs you actually opened. Give 15-25 primary sources, each claim with a short quote and exact URL. Prefer official docs, pricing pages, changelogs, GitHub repos dated 2025-2026. Tag unsupported statements [no source found], single-source [single-source], marketing/blog [low-authority]. Use confidence labels [established]/[emerging]/[speculative]. Show disagreements between sources rather than averaging them. End with a "proof of work" list of queries run and pages read.

## Q2 — CAPTCHA: legitimate handling and human-in-the-loop products (2025-2026)
- Human-in-the-loop (HITL) agent frameworks that PAUSE on a challenge and hand off to a human: Anchor Browser, Browserbase, Steel, LangGraph interrupt, Cloudflare Agents, Mastra, and any newer ones. For each: capabilities, pricing, how the pause/resume + live-view works, cloud API surface, and maturity/maintenance.
- Which agent-browser products advertise "solves CAPTCHA" and what is the LEGAL / ToS status of using them on third-party sites (reCAPTCHA/hCaptcha/Turnstile terms, CFAA/anti-bot case law 2024-2026)? Describe the risk landscape, not how-to. Distinguish legality, ToS breach, civil liability, platform enforcement, reputation.
- Official no-CAPTCHA paths: which common services expose an official API that removes the need to drive a UI at all (so no CAPTCHA arises)? General principle + concrete examples.

## Q3 — AGENT PAYMENTS: official rails, the 2025-2026 delta
- Visa Intelligent Commerce, Mastercard Agent Pay / Agent Pay for Machines, Google Agent Payments Protocol (AP2), OpenAI/Stripe Agentic Commerce / Order Intents: what each does, launch status as of research date, how a developer integrates from the cloud, what tokens/mandates/passkeys they use, and how the HUMAN approves (per-transaction, spend caps).
- Virtual-card APIs with hard per-transaction limits and single-merchant lock: Stripe Issuing, Privacy.com (exact API: single-use card, spend_limit, spend_limit_duration, merchant lock, sandbox availability), Lithic, Ramp - pricing, approval requirements, which allow programmatic issuance, which offer a true sandbox.
- Pattern for "agent builds the cart -> human confirms with one tap -> card minted for exactly that amount": which products support it natively, and the fraud/prompt-injection safeguards each provides.

DELIVERABLE: for EACH of the two areas, a shortlist with price, cloud-integration surface, legal/ToS risk, and a one-line recommendation. One overall recommendation per area with its confidence and the most important assumption.

PLATFORM MODE: ChatGPT Deep Research / Claude Research / Gemini Deep Research / Grok strongest documented browser research mode / GLM strongest with search / Mistral deep-research. State the actual model and mode and report whether live search worked. A short answer without sources is not a completed research report.
