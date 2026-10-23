# DR26-09-15-MACANTON-01-0806 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll run live searches and open primary sources first, then write the full report from those pages.Opening primary docs, catalogs, licenses, and the remaining vendor pages next.Opening official docs and catalogs to verify catalog counts, licenses, token storage, and LLM-provider coverage.Checking Nango’s live catalog for LLM providers and the connectors we actually need.A. PROOF OF WORK

Search queries actually run in this session:
1. Nango unified API integrations platform architecture self-host license pricing
2. Nango funding round investors Bastien Beurier Robin Guldener
3. Composio AI agents integrations MCP pricing funding 2025 2026
4. Paragon ActionKit AI agents integrations pricing funding
5. Merge.dev unified API pricing funding self-host 2025 2026
6. Pipedream Connect OAuth MCP pricing funding
7. Arcade.dev AI tools MCP auth funding pricing
8. Klavis AI MCP server integrations pricing GitHub
9. Unified.to pricing catalog MCP self-host 2026
10. Apideck unified API pricing funding MCP 2026
11. Auth0 Token Vault agentic identity MCP OAuth
12. MCP OAuth specification 2025-06 2025-11 RFC 9728
13. Nango Elastic License 2.0 AGPL change providers.yaml GitHub
14. Nango jobs DevRel developer advocate 2026
15. Pica Toolhouse Membrane integration.app Truto Panora Hotglue Finch Knit Nylas Anon Keet pricing MCP
16. Zapier MCP Cloudflare Agents OAuth Descope Stytch Connected Apps Okta Cross-App Access
17. site:github.com/NangoHQ/nango mistral groq deepseek fireworks together huggingface cohere
18. Nango token encryption KMS credential storage OAuth BYOK security
19. Nango MCP server Claude Code docs connection
20. Composio funding Series A Lightspeed GitHub stars license self-host
21. Nango AGPL license history Elastic 2.0 when changed
22. Nango GitHub issues complaints "open source" pricing MCP self-host enterprise
23. Arcade.dev funding seed amount investors 2025 2026
24. Pipedream Workday acquisition 2025 2026
25. Paragon useparagon funding Series A headcount 2025 2026
26. Google OAuth testing mode refresh token expires 7 days unverified app 2025 2026
27. Klavis AI official pricing page YC X25 funding
28. Toolhouse AI MCP integrations pricing funding 2026

Primary URLs actually opened (not just search snippets):
- https://nango.dev/pricing
- https://nango.dev/docs/guides/platform/self-hosting
- https://nango.dev/docs/guides/platform/security
- https://nango.dev/docs/getting-started/coding-agent-setup
- https://nango.dev/docs/implementation-guides/platform/auth/contribute-new-api
- https://nango.dev/api-integrations
- https://nango.dev/blog/nango-raises-7-5m-led-by-gradient
- https://raw.githubusercontent.com/NangoHQ/nango/master/packages/providers/providers.yaml
- https://github.com/NangoHQ/nango/blob/master/LICENSE
- https://github.com/NangoHQ/nango/issues/2802
- https://composio.dev/pricing
- https://raw.githubusercontent.com/Klavis-AI/klavis/main/LICENSE
- plus the search-result pages cited below (GitHub READMEs, vendor docs, funding posts, MCP spec, Google OAuth docs)

---

B. EXECUTIVE SUMMARY

- The class is splitting into three layers that are often sold as one: (1) token vault / OAuth broker, (2) unified SaaS API, (3) agent tool/MCP gateway. Mixing them in a purchase is how teams overpay. [established]
- For this fleet (first-party Claude Code agents on ~5 machines, not a multi-tenant SaaS product), the cheapest working layer is a hosted MCP + managed OAuth vault, not a unified HRIS/CRM API. Merge/Unified.to/Apideck are the wrong shape. [established]
- Live Nango Cloud pricing (opened 2026-09-15): Free = 10 connections / 10 compute-hours / 10 GB; then $0.29 per connection-month, $0.72 per function-hour, $0.50 per GB. Growth add-on $450/mo. Self-host full stack is Enterprise. [established]
- Live Composio pricing (opened 2026-09-15): Hobby $0 with 100,000 tool calls and unlimited connected accounts; Pro $29/mo. MCP and custom tools are included on Hobby. [established]
- Nango’s live `providers.yaml` has Gmail, Google Calendar, Trello (OAuth 1.0a), Calendly (OAuth 2), OpenAI, Anthropic, xAI, Perplexity, Replicate. It does **not** have Mistral, Groq, DeepSeek, Together AI, Fireworks, Cohere, or Hugging Face. Telegram is Bot API, not MTProto. [established]
- Nango was never AGPL. The GitHub LICENSE is Elastic License 2.0 (ELv2) and the first LICENSE commit is dated 2022-11-30. Calling it “open source” is a documented community fight (issues #900, #2802). [established]
- Free self-hosted Nango is Auth + Proxy only. Syncs, tools, webhooks, and the **MCP server are Enterprise/Cloud**. SAML SSO is Cloud-only even on Enterprise self-host. [established]
- Telegram user sessions and the Chrome-extension MCP cannot be represented by any vendor in this class. Keep Telethon and the Chrome MCP. [established]
- 1-day recommendation: Composio Hobby for Claude Code tools; Nango Cloud Free in parallel if the CTO conversation is the point. Do not self-host Nango on day one. Confidence: [established] on facts, [emerging] on “Composio over Nango for this fleet,” resting on the assumption that credentials may live in a US SaaS vault.
- Adjacent “agent identity” vendors (Auth0 Token Vault, Stytch Connected Apps, Descope, Okta Cross-App Access, Cloudflare Agents OAuth) solve MCP server *front-door* auth, not Gmail/Trello connectors. They commoditize Nango’s MCP-auth story, not its 1,000-API proxy. [emerging]

---

C. REQUIREMENT 1 — PRODUCT PROFILES

Confidence labels apply to each profile’s money/catalog/license numbers.

1) Nango (nango.dev) — unified integration *infrastructure* (auth + proxy + optional functions/syncs/MCP), not a rigid unified data model.

What it is: Code-first integration platform. You store per-end-user Connections; Nango runs OAuth, refreshes tokens, proxies HTTP, and (on Cloud/Enterprise) runs TypeScript functions, syncs, webhooks, and a hosted MCP. GitHub: NangoHQ/nango, ~12k stars (11.8k–12.1k across pages opened 2026-09-11 to 2026-09-15). YC W23. Launch as OAuth helper 2023; YC launch post 2023-03-14 described 55+ APIs.

Architecture (official self-hosting docs, opened this run): Server (dashboard/API/proxy/webhooks), Orchestrator (task scheduling), Jobs (dispatch), Runner (executes integration code), Persist (records/logs), PostgreSQL, Redis (cache, token-refresh locks, rate limits), Elasticsearch (execution logs), object storage for compiled functions. Recommended size: 5 Node services at 1 CPU/2 GB each, Postgres 2 CPU/8 GB/128 GB, Redis 128 MB, ES 2 vCPU/1 GB/30 GB. Default config claimed for 1M+ executions/day. **No Temporal.** The “Temporal history” item in the brief is not in Nango’s published stack. [established]

Authorization: OAuth 2.0 auth-code (PKCE on by default; some providers set `disable_pkce: true`), OAuth 2 client-credentials (`OAUTH2_CC`), OAuth 1.0a (Trello), API key, Basic, custom/TWO_STEP, MCP_OAUTH2 with dynamic client registration. Per-end-user Connections. BYO OAuth app or Nango-managed apps (Nango warns managed apps can be revoked and block token export). Automatic refresh; concurrency-safe locks in Redis. Revocation = delete connection (soft delete then hard delete at 31 days). Scopes stored on the connection. Audit trail exists; on self-host it is off until `FLAG_AUDIT_TRAIL_ENABLED=true` plus a Postgres URL. [established]

Token storage: Cloud uses envelope encryption: AES-256-GCM DEK wrapped by AWS KMS. TLS 1.2+. Self-host: you set `NANGO_ENCRYPTION_KEY` (base64 256-bit); **without it credentials are stored unencrypted**. Key rotation is not supported. No customer-held HSM/BYOK on Free/PAYG. SOC 2 Type II, GDPR DPA on all cloud accounts, HIPAA BAA on Growth add-on. Logs: 15 days on Cloud; audit trail 1 year. Proxy logs URL/method/status/headers, not bodies; Authorization headers redacted. [established]

Catalog: Marketing says “1,000+ APIs” and “7k pre-built tools, triggers & syncs” (pricing page, 2026-09-15). GitHub README says “900+ APIs.” Live `providers.yaml` (fetched this run, 954,878 bytes) contains at least: `google-mail`, `google-calendar`, `google-calendar-mcp`, `trello` (OAUTH1), `calendly` (OAUTH2), `telegram` (API_KEY bot token), `openai`, `anthropic`, `xai`, `perplexity`, `replicate`. **Absent as top-level keys: mistral, groq, deepseek, together, fireworks, cohere, hugging/huggingface.** [established]

License: SPDX `Elastic-2.0`. You may use/modify internally; you may **not** offer Nango as a hosted/managed service to third parties; you may not circumvent license-key functionality. Not OSI “Open Source.” GitHub issues #900 (2023-08-09) and #2802 (2024-10-02, closed) called this out. [established]

Self-host actually includes:
- Free/hobby docker-compose: Auth + Proxy only. No functions, syncs, webhooks, MCP, RBAC, MFA, OTel. Observability = auth+proxy only. SAML = Cloud only.
- Enterprise self-host (Helm, AWS/GCP/Azure): full Cloud feature parity except SAML SSO “not yet available (planned).” Fixed annual license + discounted usage. Managed images on a two-month cadence. [established]

Pricing (live nango.dev/pricing, this run): Free 10 connections / 10 h compute / 10 GB. PAYG $0.29/connection-month (pro-rated while stored), $0.72/hour function compute (rounded up to 1 second), $0.50/GB egress. Volume example on-page: connections “as low as $0.01” on Enterprise. Growth add-on $450/mo (Slack, 5-day integration SLA). Integration delivery SLA: 20 days Free, 5 days Growth, 2 days Enterprise. No EU cloud; BYOC any region on Enterprise. [established]

Funding/team: Founded 2022, YC W23. Founders Robin Guldener (CEO; ex-Avrios) and Bastien Beurier (CTO; YC twice, Uber/Trip.com/Triplebyte). Own blog 2026-03-23: **$7.5M seed led by Gradient**, Horizon + YC + angels Akshay Kothari (Notion), John Kim (Sendbird). Claims “several million in ARR” and cash-flow positive at raise — [single-source, vendor]. PitchBook: latest $7.5M, 18 employees, “raised $8M.” Job posts (2026-09): team of 14, “more than 500 paying customers” (Replit, Ramp, Monday, ServiceNow named), mid-seven-figure ARR — [single-source, employer]. Caplight’s $24.5M total and Employbl’s $10.5M disagree; treat as bad aggregators. HQ advertised as San Francisco; Delaware/Wilmington entity also listed. DevRel: “Developer Relations (Founding)” on-site, $150k–$250k + 0.1–1% equity, posted 2026-09-11; older remote “Founding Devrel” $120k–$180k. [emerging on total raised; established on the $7.5M announcement]

2) Composio (composio.dev) — agent tool-calling / MCP gateway with managed OAuth.

What it is: 1,000–1,500 “toolkits,” sessions that bind user + permissions + connected accounts, hosted MCP URL per session, sandboxed workbench. GitHub ComposioHQ/composio: **30.2k stars**, MIT license on the SDK monorepo (opened this run). YC W24. Founders Soham Ganatra (CEO) and Karan Vaidya. Legal entity Sampark Inc., SF. [established]

Architecture: Hosted control plane. Python/TS SDKs, CLI (`curl -fsSL https://composio.dev/install | sh`), provider adapters (OpenAI, Anthropic, Claude Agent SDK, LangChain, Vercel AI SDK). Helm charts repo exists; **VPC/self-hosting is Enterprise**. Sessions expose `session.mcp.url`. [established]

Auth: Managed OAuth, API keys, Composio-managed shared OAuth apps (lower free allowances) or BYO app. Per-user connected accounts. Self-managed credentials (pass tokens at execution, Composio does not store them) is listed as Included on the live Hobby table — this **disagrees** with Scalekit’s 2026-08-14 teardown, which said self-managed credentials start at Business $599. Live page wins for new signups. CMK/KMS = Enterprise only. [established on live page; disagreement flagged below]

Catalog: Live pricing table says **1500+ toolkits**. Older reviews said 250+ then 1000+. Count is toolkits, not REST providers. [emerging]

Pricing (live composio.dev/pricing, this run): Hobby $0, 100k tool calls, 50k triggers, unlimited connected accounts, 3 members, hard-capped. Pro $29/mo + $29 usage credit, then $0.0003/call (own app) / $0.003/trigger. Composio-managed apps: only 20k of the 100k free, then $0.0005/call and $0.10/connection after 1k. Premium tools (Groq, Exa, Maps, etc.) = provider cost + 5%. New pricing for signups on/after 2026-08-15; grandfather through 2026-12-31. [established]

Funding: $25M Series A led by Lightspeed 2025-07-22; vendor says ~$29M total (prior ~$4M seed). PitchBook: 40 employees. [established on Series A]

Fit note: Best MCP on-ramp for Claude Code. Cloud-custody of tokens unless you use self-managed credentials or Enterprise CMK.

3) Paragon / ActionKit (useparagon.com) — embedded iPaaS + agent action API/MCP.

What it is: Connect Portal for per-tenant OAuth, workflows, ActionKit = 1,000+ tools over 130+ connectors, hosted ActionKit MCP (SSE, on Anthropic MCP registry), MIT `useparagon/paragon-mcp` adapter. Platform itself is closed. [established]

Auth: Fully managed, Connect Portal, BYO client on some apps, multi-account. Enterprise self-host / forward-deploy. [single-source, vendor]

Pricing: Quote-only. Older Sacra note ~$30–40k ACV for ~5 integrations. No public free production plan. [low-authority on ACV]

Funding: Inspired Capital $13M Series A 2022-07-27 (company said $16.5M total then). CB Insights total $21.15M, stage Series A-II. PitchBook lists a $21.1M Series A 2024-10-29 and 32 employees; Tracxn says 157 employees — **disagreement**. Founders Brandon Foo, Ishmael Samuel. LA office. [emerging]

Fit: Built for B2B SaaS embedding Salesforce-for-tenants. Wrong buyer for a 5-machine lab.

4) Merge.dev — classic unified API, now pivoting to AI (Unified + Agent Handler + Gateway).

What it is: Normalized models across HRIS/ATS/CRM/accounting/ticketing/KB. Agent Handler = tool calls. Gateway = LLM proxy. SDKs MIT; product closed. Self-host “available for purchase” on Enterprise. Customers named: Ramp, OpenAI, Dropbox (vendor). 1.1B daily API requests claimed in 2025 review. [single-source, vendor]

Pricing Unified (live merge.dev/pricing/unified): first 3 production Linked Accounts free; **$650/mo for up to 10**; $65 per extra. Professional/Enterprise contract. Agent Handler: reviews cite 2,000 free credits then $1,000/mo Pro / 25k credits — [single-source]. [established on Unified $650]

Funding: $4.5M (2021) + $15M A (2021) + $55M B Accel Oct 2022 = **$75M**. Founders Shensi Ding, Gil Feig. [established]

Fit: Score 1. Per-account tax destroys a fleet; unified HRIS models do not replace Gmail/Trello.

5) Pipedream Connect — workflow iPaaS + managed-auth tool API + MCP. Now Workday-owned.

What it is: 3,000+ apps, 10,000+ pre-built tools, Connect proxy, deployable MCP. Personal MCP at mcp.pipedream.com free. Production embedding = Connect plan. Credentials encrypted at rest; requests go through Pipedream servers. [established]

Auth: End-user OAuth or API keys; Pipedream-approved OAuth clients or BYO; Connect tokens scoped to external users; developer auth is client-credentials. [established]

Pricing: Connect **$99/mo annual** ($150 monthly) with 10k credits and 100 external users, **$2/extra user**. Credits = 30s compute @ 256 MB. Dev mode free. [established]

Company: Workday signed to acquire 2025-11-19; closed fiscal Q4 2026 (Workday FY2026 Q4, reported 2026-02-24). Terms undisclosed. 5,000+ customers claimed. [established]

Fit: Huge catalog, real MCP. Workday ownership is a roadmap/survival risk for a non-Workday lab.

6) Arcade.dev — agent authorization runtime + MCP tool catalog.

What it is: Per-tool OAuth, IdP enforcement, 7,500–8,000 “agent-optimized tools” across ~44–81 MCP servers (sources disagree). Claims to have authored MCP authorization bits Anthropic adopted — [single-source, vendor]. Docs: server-level (front-door OAuth) vs tool-level (Gmail/GitHub tokens). Gmail.SendEmail example requires `gmail.send`. [established on the two-layer auth model]

Pricing (arcade.dev/pricing): Free $0, 2,000 auth events + 2,000 tool calls. Enterprise custom; VPC or air-gapped. Independent review (Jul 2026) listed Team $25/mo + $0.10/auth event + $0.01/tool call — **Team tier not visible on the pricing page fetched via search in this run**. [emerging]

Funding: $12M seed 2025 + **$60M Series A 2026-06-12** led by SYN Ventures, Morgan Stanley + Wipro; total **$72M**. CEO Alex Salazar. PitchBook 34 employees. [established]

Fit: Best security story. Overkill and likely expensive for a 5-node lab unless you already live in Okta.

7) Unified.to — real-time unified API + MCP.

Catalog: Grow plan lists 386+ integrations, 24+ unified APIs. MCP changelog 2026-07-22: **43,456 tools** including 8,242 “unified” tools. [established]

Pricing: Grow **$750+/mo**, 750k API calls, $1/1k overage, unlimited connections. Pro $1,250+, Scale $3,000+. 30-day free start. On-prem/private cloud = Enterprise. [established]

Fit: Too expensive. MCP tool explosion (43k tools) is the opposite of what Claude Code needs.

8) Apideck — unified API (Unify) + Vault auth + MCP.

200+ connectors, 7 unified APIs on MCP (Accounting, CRM, HRIS, ATS, Ecommerce, …). Vault manages OAuth. Proxy passthrough to native endpoints. MCP URL `https://mcp.apideck.dev/mcp` with `x-apideck-api-key`, `x-apideck-app-id`, `x-apideck-consumer-id`. [established]

Pricing: Launch **$269/mo billed annually**, 120k API calls/year, 1 Unified API, unlimited consumers. Growth $539/mo, 2 Unified APIs. Alternative consumer-based pricing mentioned in FAQ. HQ SF + Antwerp. [established]

Fit: Accounting/CRM-shaped. Not Gmail/Trello-first.

9) Klavis AI (klavis.ai) — open-source MCP servers + hosted OAuth.

GitHub Klavis-AI/klavis: **5.8k stars**, **Apache-2.0** (LICENSE fetched this run). YC X25. Founders Xiangkai Zeng (ex-Gemini/DeepMind function calling) and Zihao Lin. Docker images `ghcr.io/klavis-ai/<service>-mcp-server`. Hosted Strata MCP aggregates servers. Gmail and Google Calendar images exist. [established]

Auth: Hosted OAuth via Klavis API key, or pass `AUTH_DATA='{"access_token":"..."}'` for no-OAuth Docker. [established]

Pricing disagreement: AWS Marketplace lists Hobby $0 (3 users, 500 calls, $0.05 overage), Pro $99 (10k calls), Team $499 (100k), Enterprise $4,999. SaaSworthy scraped $79.20 / $399.20 (likely annual). Tracxn: **$500k YC**. [emerging]

Fit: Best true-OSS MCP self-host. OAuth-in-Docker still wants a Klavis key unless you inject tokens yourself. Telegram not MTProto.

10) Pica (picaos.com) — passthrough integration layer + MCP + ToolKit for agents.

~200+ integrations, 25k–26k actions, AuthKit for OAuth, Rust core, MCP `npx @picahq/mcp` with `PICA_SECRET`. Starter **$50/mo** (250 connections, 25k requests, $0.01 extra). Pro $250. MIT on the MCP package. [established]

Fit: Reasonable cheap cloud. Smaller than Composio/Nango; less Claude Code-native than Composio.

11) Auth0 Token Vault / Auth0 for AI Agents — auth-only token broker.

Stores third-party access/refresh tokens; agent exchanges an Auth0 token (RFC 8693) for a Google/GitHub/Slack access token. ~30+ social/enterprise connections, not 1,000 SaaS tools. MCP: “Auth for MCP” (OAuth 2.1, DCR, resource-scoped tokens, OBO). No Gmail tool catalog. Pricing is Auth0 platform (not a $0 hobby integrations tier). [established]

Fit: Right if you already pay Auth0 and only need a vault. Wrong as the fleet’s only integration layer.

12) Zapier MCP — consumer automation catalog as MCP.

9,000+ apps, 40,000+ actions. Connect URL `https://mcp.zapier.com/api/v1/connect`, OAuth or connection token, Streamable HTTP only (no SSE). Tied to a Zapier account/task quota. [established]

Fit: Fastest “make Claude do Slack” demo. Bad multi-machine credential control, Zapier task pricing, not an audit vault.

Others (shorter, still sourced):

- **Truto**: zero-code unified API + auto MCP from config; cloud-only; blog contrasts Nango’s code-first model. [single-source, competitor blog]
- **Membrane / Integration.app**: embedded iPaaS, customer-facing integrations. Not opened as a full pricing page this run — [not fully verified].
- **Finch**: payroll/HRIS unified API. Wrong category.
- **Knit**: unified API, India-origin; not verified live this run — [not verified].
- **Nylas**: email/calendar/contacts API. Could replace Gmail+Calendar only; not Trello/Calendly; commercial.
- **Anon / Keet**: browser-automation “connect any site” — closer to Chrome MCP than OAuth vaults; high ToS/ban risk. Not verified in depth this run.
- **Hotglue / Vessel / Panora / Supaglue-successors**: ETL/unified CRM remnants; Panora was OSS Merge alternative. Not verified as 2026-alive MCP gateways this run.
- **Tray Embedded / Workato Embedded**: enterprise embedded iPaaS, $50k–$130k/yr Workato mid-market (Vendr via secondary). Wrong scale.
- **Stytch Connected Apps / Descope Inbound Apps / Okta Cross-App Access / Cloudflare Agents OAuth**: make *your* app an OAuth AS for MCP clients. Complementary to, not a replacement for, Gmail connectors. [established]
- **Toolhouse**: agent-worker platform, Business $500/mo, MCP as an add-on, ~10 employees, seed 2024. Not a token vault. [established]
- **Cloudflare Agents**: Workers OAuth Provider library; can sit in front of an MCP server. No SaaS catalog.

---

D. REQUIREMENT 2 — COMPARATIVE TABLE

Catalog numbers are what vendors print; they are not comparable units (APIs vs actions vs tools vs toolkits).

| Product | Category | Auth model | Token storage / encryption | Catalog (as claimed) | MCP | Self-host & license | Pricing entry (live/public) | Funding & stage | Fleet fit (1–5) |
|---|---|---|---|---|---|---|---|---|---|
| **Nango** | Auth + proxy + optional functions | OAuth2/1, CC, API key, Basic, custom, MCP OAuth; per-connection; BYO or Nango app | AES-256-GCM + AWS KMS (cloud); `NANGO_ENCRYPTION_KEY` or **plaintext** (self-host) | 900–1,000+ APIs; 7k tools | Hosted `api.nango.dev/mcp`; **not in free self-host** | ELv2; free = Auth+Proxy; full = Enterprise Helm | Free: 10 conn / 10h / 10GB; then $0.29/conn | ~$7.5M 2026 seed + earlier YC; ~14–18 people | **4** — has our four SaaS APIs + proxy; MCP gated on Cloud |
| **Composio** | Agent tool/MCP gateway | Managed OAuth, BYO app, API keys; per-user accounts | Cloud vault; CMK = Enterprise | 1,000–1,500 toolkits | First-class session MCP URL | MIT SDK; VPC = Enterprise | Hobby $0 / 100k calls, unlimited accounts | $25M A (Lightspeed 2025), ~$29M tot., ~40 ppl | **5** — cheapest MCP to Claude Code |
| **Paragon ActionKit** | Embedded iPaaS + agent actions | Connect Portal, per-tenant OAuth | Vendor-managed | 130+ connectors, 1,000+ actions | Hosted ActionKit MCP + MIT adapter | Closed; Enterprise self-host | Quote-only | ~$16.5–21M; Series A | **2** — sales-led, wrong buyer |
| **Merge** | Unified API (+ Agent Handler) | Merge Link, per Linked Account | Cloud; self-host purchasable | Category unified APIs, 200+ | Agent Handler, not a Gmail MCP | Closed | 3 free then **$650/10** | $75M, Series B | **1** — per-account cliff |
| **Pipedream Connect** | iPaaS + tool API | Managed OAuth, BYO, Connect tokens | Encrypted at rest on Pipedream | 3,000 apps / 10k tools | mcp.pipedream.com + Connect MCP | Closed; now Workday | Connect $99/mo + $2/user | Acquired by Workday | **4** — catalog/MCP yes; owner risk |
| **Arcade** | Agent auth runtime | Tool-level OAuth + IdP front door | Arcade vault; air-gap Enterprise | 7.5k–8k tools / 44–81 servers | Native MCP | Partial OSS; full = Enterprise | Free 2k auth + 2k calls | $72M ($60M A Jun 2026) | **3** — security-excellent, enterprise-priced |
| **Unified.to** | Unified API + MCP | Unified connections | Cloud; on-prem Enterprise | 386+ integrations; 43k MCP tools | Hosted MCP | Closed | **$750+/mo** | Private; not verified | **2** — price + 43k-tool dump |
| **Apideck** | Unified API + Vault | Vault OAuth | Cloud | 200+ connectors; ~360 MCP tools | `mcp.apideck.dev` | Closed | $269/mo annual Launch | Private; not fully verified | **2** — unified-domain, not Gmail-first |
| **Klavis** | OSS MCP servers | Hosted OAuth or injected token | Cloud or local Docker | 50–100+ MCP servers (marketing 100+) | Native, Docker | **Apache-2.0** | Hobby $0 (500 calls) / Pro $99 | YC X25 ~$500k | **4** — real self-host MCP |
| **Pica** | Passthrough + agent ToolKit | AuthKit OAuth | Pica vault | 200+ / 25k+ actions | `@picahq/mcp` | MCP MIT; platform hosted | $50/mo Starter | Not verified | **3** — cheap, smaller ecosystem |
| **Auth0 Token Vault** | Auth-only vault | OAuth/OIDC + RFC 8693 exchange | Auth0 | ~30 IdP connections, not SaaS tools | Auth for MCP servers | Closed | Auth0 contract | Public co. (Okta) | **3** — vault only |
| **Zapier MCP** | Consumer iPaaS as MCP | Zapier account OAuth | Zapier | 9k apps / 40k actions | `mcp.zapier.com` | Closed | Zapier task plans | Public co. | **3** — demo-grade, weak fleet control |

---

E. REQUIREMENT 3 — FIT FOR OUR FLEET

What can replace the hand-rolled connectors

| Our connector today | Nango | Composio | Klavis | Pipedream | Notes |
|---|---|---|---|---|---|
| Gmail (OAuth refresh, manual scopes) | Yes, `google-mail` OAuth2 | Yes, Gmail toolkit | Yes, gmail-mcp-server | Yes | Restricted Gmail scopes → Google CASA. None of these waive verification. |
| Google Calendar | Yes, `google-calendar` + `google-calendar-mcp` | Yes | Yes | Yes | Sensitive scopes; App verification usually required, CASA usually not. |
| Trello (API key today) | Yes, **OAuth 1.0a** (`trello`) | Yes | Check catalog; not verified here | Yes | Nango is OAuth1, not API key. |
| Calendly (API key today) | Yes, **OAuth 2** | Yes | Not verified | Yes | Nango is OAuth2, not API key. |
| Telegram MTProto (Telethon user session) | **No** — `telegram` is BotFather **bot token** → `api.telegram.org/bot` | Bot/API tools only, not MTProto | No | No | Keep Telethon. |
| Chrome extension MCP | **No** | **No** | **No** | **No** | Keep the extension. |

How tools show up in Claude Code

- **Nango Cloud:** Streamable HTTP MCP at `https://api.nango.dev/mcp` with headers `Authorization: Bearer <ENV-SECRET>`, `connection-id`, `provider-config-key`. Desktop/stdio clients need `mcp-remote` and exact header spacing. Separate **Management MCP** (Beta) at `https://mcp.nango.dev/mcp` for account/docs, not Gmail tools. Claude Code: `claude mcp add --transport http nango-management --scope user https://mcp.nango.dev/mcp --header "Authorization: Bearer <KEY>"`. [established]
- **Composio:** `composio.create(..., mcp: true)` → `session.mcp.url`; or `claude mcp add --transport http ...`. CLI install for coding agents.
- **Klavis:** hosted Strata URL or Docker on localhost:5000.
- **HTTP proxy path (Nango):** `GET https://api.nango.dev/proxy/...` with `Provider-Config-Key` + `Connection-Id`. Useful if MCP is gated.

Multi-machine sharing

One cloud project, one set of Connections. Every Mac/Win/VPS Claude Code points at the same MCP URL + the same env secret. Do **not** copy refresh tokens onto nodes. That is the whole point.

Secrets

Today: refresh tokens and Telethon sessions scattered per node. Target: one vault. Nango Cloud = AES-256-GCM + KMS in AWS. Composio = their cloud (CMK only on Enterprise). Nango free self-host = you must set `NANGO_ENCRYPTION_KEY` or tokens sit in Postgres as plaintext. [established]

---

1-DAY PLAN (recommended path: Composio Hobby for tools + optional Nango Cloud eval)

Assumption: first-party use of Anton’s own Google/Trello/Calendly. Not shipping a product to 1,000 customers. Google will still treat Gmail read scopes as restricted.

Hour 0–0.5 — Decide the vault (0 min if you take the recommendation)
1. Primary: Composio Hobby (MCP in one morning).
2. If the Nango CTO meeting is this week: also create a Nango Cloud free account. Do not Docker-compose Nango today; MCP is not in the free self-host.

Hour 0.5–1.5 — Google Cloud OAuth app (the long pole)
3. Google Cloud Console → new project `anton-fleet-oauth`.
4. Enable Gmail API and Google Calendar API.
5. OAuth consent screen: **External**. Publishing status **In production** as soon as you can click it, even if unverified. **Testing + External = refresh tokens die in 7 days.** Google’s own OAuth 2.0 docs: a project with External + Testing is “issued a refresh token expiring in 7 days” unless scopes are only profile/email/openid. Also: 100 test users; 100 refresh tokens per user per client ID; unused refresh tokens die at 6 months; Gmail-scoped tokens can die on password change. [established]
6. Scopes: start with Calendar (`calendar.readonly` or `calendar`) and Gmail send-only (`gmail.send`) if that is enough. **Most Gmail read/modify scopes are Restricted → App Verification + CASA security assessment.** Nango’s Google review guide says this explicitly. You can run in test mode internally; you cannot pretend Restricted Gmail is a 2-hour task. [established]
7. Create OAuth client type **Web application**.
8. Redirect URIs (add all you might use):
   - Composio: whatever Composio dashboard shows (copy from the toolkit auth config; do not guess).
   - Nango Cloud: `https://api.nango.dev/oauth/callback` (Nango Calendar setup guide).
   - Local Nango later: `http://localhost:3003/oauth/callback` (UI is localhost:3003 per contribute docs).
9. Authorized JavaScript origins: Nango docs say site URL, `https://app.nango.dev` if testing from Nango UI.
10. Download client ID/secret into the OS secret store, not into git, not into `CLAUDE.md`.

Hour 1.5–3 — Composio
11. Sign up at https://composio.dev (Hobby, no card).
12. Create a session / auth config for Gmail, Google Calendar, Trello, Calendly. Prefer **your** Google OAuth client, not Composio-managed apps (managed apps only get 20k of the 100k free calls and then cost extra).
13. Complete each Connect flow once in the dashboard. Confirm four connected accounts.
14. Create an MCP session and copy the URL.
15. On **each** of the 5 machines:

```bash
claude mcp add --transport http composio-fleet "<SESSION_MCP_URL>" --headers "X-API-Key:<COMPOSIO_API_KEY>"
claude mcp list
```

16. In Claude Code: `/mcp`, authenticate if asked, then “list my next 3 calendar events” and “list Trello boards.”
17. Put the API key in the machine secret store; user-scope MCP config (`--scope user`), never a repo file.

Hour 3–4.5 — Nango Cloud eval (skip if you only wanted Composio)
18. https://app.nango.dev/signup
19. Integrations → Configure: Gmail, Google Calendar, Trello, Calendly. Paste Google client ID/secret. Trello is OAuth1; Calendly OAuth2 — follow each setup guide.
20. Connections → Add Test Connection for each. Save the `connection-id`s.
21. Proxy smoke test (Calendar):

```bash
curl "https://api.nango.dev/proxy/calendar/v3/users/me/calendarList?maxResults=10" \
  -H "Authorization: Bearer <NANGO-ENV-API-KEY>" \
  -H "Provider-Config-Key: google-calendar" \
  -H "Connection-Id: <CONNECTION-ID>"
```

22. Claude Code Management MCP (docs/account, not Gmail tools):

```bash
claude mcp add --transport http nango-management --scope user https://mcp.nango.dev/mcp \
  --header "Authorization: Bearer <NANGO-API-KEY>"
```

23. Gmail/Calendar **tools** MCP (Cloud only):

```bash
claude mcp add --transport http nango-gmail \
  https://api.nango.dev/mcp \
  --header "Authorization: Bearer <NANGO-ENV-API-KEY>" \
  --header "provider-config-key: google-mail" \
  --header "connection-id: <GMAIL-CONNECTION-ID>"
```

If the Claude Code HTTP transport drops custom headers, use `npx -y mcp-remote` as in Nango’s Claude Desktop snippet; **do not change header spacing**. [established]

Hour 4.5–6 — Pitfalls to hit on purpose
24. Revoke a Google grant in https://myaccount.google.com/permissions, then call the tool: you want a clean re-auth, not a silent 401 loop.
25. Confirm Testing-mode 7-day expiry is **not** your fate: consent screen must not stay on Testing.
26. Do **not** enable Gmail Restricted scopes “just in case.” Every extra Restricted scope is a CASA invoice.
27. Telegram: leave Telethon. Nango’s Telegram provider is a bot token to `api.telegram.org/bot`. User-session MTProto is a different protocol.
28. Chrome MCP: leave it.
29. Write a 10-line runbook: where the Google client lives, which connection IDs exist, which MCP URL each machine uses, who rotates the env API key.

Hour 6–7 — Optional true self-host (only Auth+Proxy)
30. Only if you must have tokens on the VPS today:

```bash
git clone https://github.com/NangoHQ/nango.git
cd nango
# set NANGO_ENCRYPTION_KEY=$(openssl rand -base64 32) in .env *before* first boot
docker compose up
# UI: http://localhost:3003
# OAuth callback: $NANGO_SERVER_URL/oauth/callback
```

31. Know that this box will **not** speak MCP. You would wrap `https://<host>/proxy/...` in a 40-line local MCP. That is day 2.

Do not: enable Nango syncs, buy Merge, or point Zapier MCP at production Gmail.

---

F. REQUIREMENT 4 — RISKS

Vendor lock-in
- Nango Connection IDs and function code are portable-ish; **tokens are not exportable if you used Nango-managed OAuth apps**. BYO Google client is the escape hatch (Nango auth guide says this). [established]
- Composio toolkit names and session IDs are proprietary; MIT SDK does not include their OAuth vault.
- Merge common models are the classic lock-in; you do not want them.
- Pipedream is now a Workday product. Multi-year dependency on a non-Workday lab is a concentration risk. [established]

Self-host viability
- Nango free self-host is a token proxy, not the product you saw in the demo. MCP/syncs/tools = Enterprise or Cloud.
- ELv2 forbids offering Nango as a managed service to others (irrelevant internally, relevant if you ever host for peers).
- Klavis Apache-2.0 is the only fully self-hostable MCP catalog here; hosted OAuth still likes their API key.
- Arcade air-gap and Composio VPC are enterprise SKUs.
- Nango encryption key cannot be rotated. Lose it, lose the vault. [established]

Pricing cliffs
- Nango: 10 free connections, then $0.29 each. At 50 connections ≈ $11.60/mo + compute. Mild. Growth $450 is the real cliff (HIPAA, Slack, 5-day SLA).
- Composio: Hobby is hard-capped (good). Pro overage $0.0003/call is cheap; **Composio-managed apps and premium tools** are the surprise. Grandfather ends 2026-12-31. Scalekit documented a 2026-08 hike (50k calls at $29 vs old 200k; $229 tier replaced by $599). Live page we opened is already a third revision (100k Hobby). Expect another change. [established on live numbers; emerging on “they will hike again”]
- Merge: $650 + $65/account is a massacre at 50 connections (~$3,250/mo).
- Unified.to: $750 floor.
- Arcade Team usage ($0.01/call if that review is still current) is 30× Composio’s $0.0003.

Security / CVEs
- No Nango CVE set found in this run. Absence ≠ audit. [not verified]
- Self-host Nango without `NANGO_ENCRYPTION_KEY` stores tokens in Postgres cleartext. That is the incident waiting to happen.
- Shared Nango/Composio-managed OAuth apps: one vendor revocation burns every customer. Use BYO Google client.

Survival / consolidation 2025–2026
- Pipedream → Workday (signed 2025-11-19, closed FY Q4 2026). [established]
- Arcade $72M: well-funded, enterprise-shaped.
- Composio $29M: best-funded independent MCP gateway.
- Nango: claims cash-flow positive + $7.5M dry powder; small team (14–18). Seed-stage concentration on two founders.
- Klavis: YC ~$500k, 1–10 people, homepage recently tilted from “hosted MCP” toward “agent training data.” Pivot risk. [emerging]
- Merge $75M, restarting as “AWS of AI” in 2025. Alive, expensive.

MCP gateway commoditization
- Anthropic connector directory, OpenAI Apps SDK, Google managed MCP, Cloudflare Workers OAuth, Auth0 Token Vault, Stytch CIMD, MCP spec 2025-06 (OAuth 2.1 + RFC 9728 PRM + RFC 8707 resource indicators) and 2025-11 (CIMD, OIDC discovery). [established]
- This eats **generic MCP hosting** and **front-door OAuth**. It does **not** eat “refresh Trello OAuth1 + Gmail restricted scopes + proxy + audit across 1,000 APIs.” Nango/Composio still have a job. The job is narrower than 2024 pitch decks.

---

G. REQUIREMENT 5 — NANGO DEEP-DIVE AND CRITIQUE

Architecture, again, in one paragraph
Five Node services + Postgres + Redis + Elasticsearch + S3. Orchestrator is their own scheduler (Postgres LISTEN/NOTIFY in third-party writeups), not Temporal. Cloud runners are isolated per customer; self-host shares a runner. Persist is the records/log writer. Jobs mint a short-lived HMAC “task JWT” (iss `nango-internal`, default 25 h) so a stolen runner token forges at most that task. [established, self-hosting docs]

License history — correct the brief
Nango did **not** go AGPL → ELv2. The public LICENSE file is Elastic License 2.0; GitHub dates the LICENSE file to 2022-11-30. Community members have been saying “this isn’t open source” since 2023 (#900, wontfix) and 2024 (#2802, closed). Marketing still says “open source.” ELv2 is source-available fair-code: internal use yes, SaaS-resale no, license-key circumvention no. [established]

What “self-hosted” excludes (free tier)
API Auth yes, Proxy yes. No: functions, pre-built syncs/tools/triggers, webhooks, MCP server, OTel, branding, RBAC, MFA. SAML is Cloud-only even for Enterprise self-host (docs: “Not yet available (planned)”). Audit trail requires extra flags/DB. Encryption is opt-in via env and unrotatable. [established]

Catalog philosophy — LLM providers
Verified against live `packages/providers/providers.yaml` this run:

Present: OpenAI, OpenAI Administration, Anthropic, Anthropic Administrator, xAI, Perplexity, Replicate, plus the usual SaaS long tail (Gmail, Calendar, Trello, Calendly, Slack, HubSpot, …).

**Not present as keys:** `mistral`, `groq`, `deepseek`, `together`, `fireworks`, `cohere`, `hugging` / `huggingface`.

So the claim “by 2026-09-15 Nango’s 1000+ catalog had no Mistral, DeepSeek, Groq, Together, Fireworks, Cohere, Hugging Face” is **true** against the live yaml. OpenAI/Anthropic/xAI/Perplexity/Replicate **are** there — the catalog is not “no AI providers,” it is “no GPU-inference long tail.”

Why: Nango’s add-API guide requires a public HTTP/SOAP API and a test account. LLM inference providers are API-key-only, already trivial (`Authorization: Bearer`), and compete with Merge Gateway / OpenRouter, not with Nango’s OAuth proxy. Community PRs are accepted (`feat(integrations)`); SLA to *request* an API is 20/5/2 days by plan. That process optimizes for the long tail of **SaaS OAuth**, which is the actual hard problem. [established on process; speculative on “we skip Groq on purpose,” but it is the steelman]

How to add a provider
Fork → edit `providers.yaml` → `npx tsx scripts/validation/providers/validate.ts` → `docker compose up` → UI localhost:3003 → add integration → connect → write `docs/integrations/all/<api>.mdx` + `docs/docs.json` → PR. Past merged PRs labeled `api` / `feat(integrations)`. Slack `#request-a-new-api` is the non-PR path. [established]

Community health
ecosyste.ms snapshot: 471 issues, 5,709 PRs, average issue close ~1 month (10 days in the past year), PR close 3 days, 4,772 merged. Top issue authors are insiders (bastienbeurier 194, khaliqgant 99, rguldener 39). Past-year issue authors: 21. That is a company-run repo with drive-by API PRs, not a noisy town square. Slack community exists (`nango.dev/slack`). Issue #5536 (2026-02-27, open/Stale) asked “what does free self-host actually include?” — the question itself is evidence the README oversells. [established]

DevRel footprint
Docs are strong (Mintlify, llms.txt, api-catalog.txt, OpenAPI spec.yaml, coding-agent-setup for Claude Code/Cursor/Codex). Blog is high-volume comparison content (often anti-Merge). Skills repo `NangoHQ/skills`. YouTube/conference presence not independently audited this run. **They are hiring a Founding Developer Relations Lead** (on-site, $150–250k, 0.1–1% equity, posted 2026-09-11): demos around agents/MCP, content, ecosystem. Team of 14, no dedicated DevRel yet. That is the job-shaped hole. [established]

Known complaints
- “Open source” wording vs ELv2 (#900, #2802). [established]
- Self-host feature matrix was unclear (#5536). [established]
- Free self-host is a teaser; MCP/functions paid (docs now admit this; blogs of Truto/Oden repeat it). [established]
- Catalog count oscillates in public (400 / 600 / 700 / 900 / 1,000+) depending on the page date. [established]
- MakerStack review (2026-03-19) listed $19 Starter — **that is stale/wrong vs live $0.29/connection PAYG**. [low-authority, contradicted]
- No EU region; BYOC only on Enterprise. [established]
- Google verification still lands on you; Nango does not eat CASA. [established]

Pricing changes
Old third-party tables: Free 1,000 connections / Starter $50 with 20 connections / Growth $500. Live page (this run) is Free 10 connections then $0.29, plus a $450 Growth add-on. That is a **material tightening of the free connection cap** (1,000 → 10) if the older tables were ever accurate. Treat historical $50 Starter as superseded. [emerging]

5–10 defensible weaknesses (with evidence)
1. **ELv2 while marketing “open source.”** Issues #900/#2802; LICENSE text. Trust tax with the exact engineers they want.
2. **Free self-host does not include MCP**, the feature AI-agent buyers demo. Official matrix. [established]
3. **SAML missing on Enterprise self-host.** Security page. Enterprise buyers notice. [established]
4. **Encryption key unrotatable; empty key = plaintext tokens.** Self-host docs. [established]
5. **LLM inference long tail missing** (Mistral/Groq/DeepSeek/Together/Fireworks/Cohere/HF) despite “1,000+” and AI-agent homepage. Live yaml. [established]
6. **Catalog counting is mushy** (APIs vs tools vs pre-built functions; 900 vs 1,000+). Makes DevRel claims easy to puncture.
7. **Nango-managed OAuth apps are a footgun** (revocation, no token export). Their own auth guide. [established]
8. **No EU cloud.** Pricing FAQ. [established]
9. **Tiny team vs Composio/Arcade/Merge.** 14–18 people vs Composio ~40, Arcade ~34 + $72M, Merge post-Series B. Execution risk on DevRel, support SLAs, and Google-scope guidance.
10. **Two MCP endpoints confuse buyers:** `mcp.nango.dev` (management) vs `api.nango.dev/mcp` (tools, needs connection-id). Easy to wire Claude Code to the wrong one. Docs are clear if you read them; the homepage is not.

What Nango does **better** than Composio / Paragon / Merge (fair)
- **You own the data model.** Merge/Apideck/Unified.to force common models; Nango lets you proxy raw or write your own functions. That is the right call for a lab that already has opinions.
- **Auth coverage is actually wide:** OAuth1 (Trello), CC, custom two-step, MCP OAuth with DCR — not just OAuth2 code+PKCE.
- **Source-available proxy you can read.** Composio’s vault is a black box; Paragon’s platform is closed.
- **Honest-ish usage pricing** once you ignore old $50 tables: $0.29/connection is not Merge’s $65.
- **BYOC / Helm** exists as a real path (paid). Merge self-host is “call sales.”
- **Coding-agent setup is first-class** (Claude Code, Cursor, Codex, skills). They are building for the buyer who is also an agent.
- **Concurrency-safe refresh** as a documented product feature, which is exactly this fleet’s current outage mode.

Funding / people recap for the meeting
Robin Guldener CEO, Bastien Beurier CTO. $7.5M Gradient seed announced 2026-03-23. YC W23. ~14 people, 500+ paying (employer claim), customers named Replit, Ramp, Mercor, Monday, ServiceNow. Hiring Founding DevRel and Founder’s Associate (SF, on-site). [established on names/round; single-source on ARR/customer count]

---

H. REQUIREMENT 6 — EMERGING TRENDS (last 12–18 months)

Agent auth as a category [emerging → established]
Auth0 Token Vault (announced with Auth0 for AI Agents 2025-04-08; RFC 8693 exchange). Stytch Connected Apps + Cloudflare Remote MCP (2025-04-07). Descope Inbound Apps. Okta Cross-App Access MCP PoC (Identity Assertion Authorization Grant). WorkOS Connect / AuthKit RFC 9728 posts (2025-05). Arcade as the “action layer” with $60M (2026-06). These products treat the agent as a principal (`sub_profile: ai_agent` in Auth0). They do not replace Nango’s 1,000 SaaS connectors; they replace “roll your own OAuth for *your* MCP server.”

MCP OAuth spec [established]
- 2025-03-26: Streamable HTTP replaces HTTP+SSE.
- **2025-06-18:** OAuth 2.1 draft 13, PKCE mandatory, RFC 9728 Protected Resource Metadata **MUST**, RFC 8414 AS metadata, RFC 8707 resource indicators, tokens audience-bound, **MCP servers MUST NOT pass through tokens to downstream APIs**. Fallback `/authorize` `/token` `/register` paths removed.
- **2025-11-25:** CIMD (Client ID Metadata Documents), OIDC discovery as an alternative to RFC 8414, DCR as optional fallback. Stytch/MCPJam checklist 2025-11-05.

First-party connector directories [emerging]
Anthropic MCP registry / Claude connectors; OpenAI Apps SDK; Google managed MCP. These are distribution, not a 1,000-API proxy. They will skim the “connect GitHub to Claude” cream.

Unified-API vendors pivoting to tools [established]
Merge: Agent Handler + Gateway (2025 “restart”). Paragon: ActionKit. Nango: MCP + functions. Apideck/Unified.to: MCP skins over the same Vault. The old “one Employee object” pitch is dying; the new pitch is “tools for agents.” Nango’s code-first stance is the least fake version of that pivot.

---

I. TALKING POINTS FOR NANGO CTO

Hard questions
1. Why does the homepage still say “open source” when the LICENSE has been ELv2 since 2022 and your own GitHub issues called this out in 2023 and 2024? What words will you let DevRel use?
2. Why is the MCP server — the feature every AI-agent demo needs — absent from free self-host? Is that a license-key feature, a capacity issue, or a funnel?
3. We fetched `providers.yaml` on 2026-09-15: no Mistral, Groq, DeepSeek, Together, Fireworks, Cohere, Hugging Face, while OpenAI/Anthropic/xAI exist. Is the rule “API-key LLM hosts are out of scope”? If yes, say so on the catalog page.
4. Enterprise self-host still has no SAML, and `NANGO_ENCRYPTION_KEY` cannot be rotated. Which quarter do those close? We will not put Gmail refresh tokens in a vault we cannot rotate.
5. Free connections went to **10** on the live pricing page. Older public tables said 1,000 or a $50 Starter with 20. What is the actual 12-month price path for a 50-connection AI lab that only needs Auth+Proxy+MCP?

What we would bring as DevRel
1. A public, brutal matrix: Cloud vs free self-host vs Enterprise self-host, including MCP, SAML, encryption, EU, audit. Issue #5536 should never have existed.
2. A Claude Code “fleet recipe”: one Google OAuth app, four connections, `claude mcp add` with the **tools** endpoint, plus a warning card for the management MCP mix-up.
3. A catalog honesty pass: count APIs, auth modes, and pre-built tools as three numbers; publish the yaml badge; add the missing LLM API-key providers in a weekend if you want AI builders to stop bouncing.
4. A Google Restricted-scopes runbook that does not lie: CASA cost, 7-day Testing tokens, `gmail.send` vs `gmail.readonly`, Nango-managed app revocation. This is the #1 silent failure in the wild.
5. Agent-native content that is not another “vs Merge” blog: concurrency-safe refresh, RFC 9728, “why MCP servers must not passthrough tokens” (the 2025-06 spec) — Nango already does the downstream-token isolation that the spec now requires. That is a real story. Tell it.

---

J. DISAGREEMENTS BETWEEN SOURCES (do not average)

- **Nango total funding:** company $7.5M seed 2026; PitchBook/Tracxn ~$8M total; Caplight $24.5M; Employbl $10.5M; CB Insights $500K stale. Use **$7.5M announced + earlier YC/seed ≈ $8M**, ignore Caplight.
- **Nango catalog size:** pricing/homepage 1,000+; GitHub README 900+; older blogs 400/700. Units differ (APIs vs tools).
- **Nango list prices:** live page (this run) ≠ MakerStack $19 Starter ≠ older $50/20-conn Starter ≠ PulseSignal $50 credits table. **Live nango.dev/pricing wins.**
- **Nango “open source”:** company yes; OSI/community no (ELv2).
- **Nango ARR/customers:** only employer/job posts and the raise blog. Unaudited.
- **Composio pricing:** Scalekit 2026-08-14 described Free 20k / Pro $29 with 50k / Business $599. `composio.dev/updated-pricing` search hit still showed that. **Live `composio.dev/pricing` this run is Hobby 100k / Pro $29 / no $599 tier.** Two pages, two eras. New signups = live page; grandfather through 2026-12-31.
- **Composio self-managed credentials:** Scalekit said Business+; live Hobby table says Included.
- **Arcade catalog:** 7,500 vs 8,000 tools; 44 vs 81 MCP servers. Pricing Team $25 from a review, not from the pricing page snippet we got (Free vs Enterprise only).
- **Paragon funding/headcount:** $16.5M (2022 company) vs $21.15M (CB Insights) vs $21.1M 2024 round (PitchBook); 32 vs 157 employees.
- **Klavis price:** AWS MP $0 / $99 / $499 vs SaaSworthy $79.20 / $399.20 (annual?).
- **Pipedream close date:** PR Newswire expected close FY Q4 2026 ending 2026-01-31; Workday FY2026 Q4 results (2026-02-24) say closed; LinkedIn 2025-12-04 already said closed. Treat as **closed by early December 2025 / FYQ4 2026**, terms still undisclosed.
- **Nango architecture “Temporal”:** not in official docs. Third-party “openapps.pro” described Postgres LISTEN/NOTIFY. Do not tell the CTO they run Temporal.

---

K. DECISION — NAMED OPTIONS AND ONE RECOMMENDATION

Option A — **Composio Hobby (MCP-first)**
Trade-off: fastest Claude Code path, $0, unlimited accounts, MIT SDK. Tokens in Composio’s cloud; CMK/VPC are Enterprise; they already hiked once in Aug 2026. Telegram/Chrome still local.

Option B — **Nango Cloud Free/PAYG (auth-first)**
Trade-off: BYO Google app, AES-256-GCM+KMS, proxy escape hatch, $0.29/conn after 10, MCP on Cloud. Free self-host later is Auth+Proxy only. ELv2. Catalog will not grow Groq for you.

Option C — **Klavis Docker MCP (OSS-first)**
Trade-off: Apache-2.0, Gmail/Calendar images, real self-host. OAuth story is weaker; catalog smaller; company is YC-seed and pivoting toward training data.

Option D — **Nango free Docker + homemade MCP wrapper**
Trade-off: tokens on the VPS. You rebuild the MCP they gated. Only rational if US SaaS custody is a hard no.

Option E — **Do nothing / keep hand-rolled**
Trade-off: you already know the failure mode (silent refresh, scope drift, no inventory).

**Recommendation: Option A this week, Option B in parallel if the Nango conversation is real. Do not self-host Nango on day one.**

Confidence: **[emerging]** (product choice) on top of **[established]** facts.

Main assumption: it is acceptable for Anton’s Gmail/Calendar/Trello/Calendly OAuth tokens to live in a US vendor vault (Composio and/or Nango Cloud) with BYO Google client IDs. If that assumption is false, switch to Option D and keep MCP as a 40-line local wrapper over Nango Proxy.

Do not buy Merge, Unified.to, Paragon, or Arcade for this fleet. Keep Telethon and Chrome MCP. Do not ask Nango or Composio to become an MTProto client.

---

L. FULL SOURCE LIST (URLs opened or used from live search/fetch this run)

Nango
- https://nango.dev/pricing (opened 2026-09-15)
- https://nango.dev/docs/guides/platform/self-hosting (2026-09-07 docs stamp)
- https://nango.dev/docs/guides/platform/security (2026-09-08)
- https://nango.dev/docs/getting-started/coding-agent-setup (2026-09-03)
- https://nango.dev/docs/implementation-guides/platform/auth/contribute-new-api
- https://nango.dev/docs/guides/auth/auth-guide
- https://nango.dev/docs/implementation-guides/use-cases/tool-calling/implement-mcp-server
- https://nango.dev/api-integrations
- https://nango.dev/blog/nango-raises-7-5m-led-by-gradient (2026-03-23)
- https://raw.githubusercontent.com/NangoHQ/nango/master/packages/providers/providers.yaml (fetched this run)
- https://github.com/NangoHQ/nango (README, ~2026-09-11)
- https://github.com/NangoHQ/nango/blob/master/LICENSE
- https://github.com/NangoHQ/nango/issues/2802 (2024-10-02)
- https://github.com/NangoHQ/nango/issues/900 (2023-08-09)
- https://github.com/NangoHQ/nango/issues/5536 (2026-02-27)
- https://www.ycombinator.com/companies/nango
- https://www.ycombinator.com/launches/IHK-nango-get-oauth-tokens-for-apis-fast-secure (2023-03-14)
- https://startup.jobs/developer-relations-founding-nango-7884360 (2026-09-11)
- https://nango.dev/docs/api-integrations/google-mail
- https://nango.dev/docs/api-integrations/google-calendar
- https://nango.dev/docs/api-integrations/google-shared/google-security-review
- https://issues.ecosyste.ms/hosts/GitHub/repositories/NangoHQ%2Fnango

Composio / Paragon / Merge / Pipedream / Arcade
- https://composio.dev/pricing (opened this run)
- https://github.com/composiohq/composio
- https://lsvp.com/stories/investing-in-composio-building-the-backbone-of-ai-agent-intelligence/ (2025-07-22)
- https://siliconangle.com/2025/07/22/composio-raises-25m-funding-ease-ai-agent-development/
- https://www.scalekit.com/blog/composio-pricing-change (2026-08-14)
- https://docs.useparagon.com/actionkit/overview
- https://github.com/useparagon/paragon-mcp
- https://www.merge.dev/pricing/unified
- https://www.merge.dev/blog/the-new-merge (2026-05-19)
- https://pipedream.com/pricing
- https://pipedream.com/docs/connect/mcp
- https://www.prnewswire.com/news-releases/workday-signs-definitive-agreement-to-acquire-pipedream-302619298.html (2025-11-19)
- https://www.arcade.dev/pricing/
- https://www.arcade.dev/blog/arcade-series-a/ (2026-06-12)
- https://docs.arcade.dev/en/learn/server-level-vs-tool-level-auth

Others
- https://www.unified.to/pricing
- https://docs.unified.to/mcp/changelog
- https://apideck.com/pricing
- https://developers.apideck.com/mcp
- https://github.com/Klavis-AI/klavis
- https://raw.githubusercontent.com/Klavis-AI/klavis/main/LICENSE
- https://aws.amazon.com/marketplace/pp/prodview-vekzgxp36lscq
- https://www.picaos.com/pricing
- https://docs.picaos.com/mcp-server
- https://auth0.com/ai/docs/intro/token-vault
- https://auth0.com/blog/introducing-auth0-for-ai-agents/ (2025-04-08)
- https://docs.zapier.com/mcp/get-started/connect
- https://stytch.com/blog/remote-mcp-stytch-cloudflare/ (2025-04-07)
- https://developers.cloudflare.com/agents/model-context-protocol/authorization
- https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization
- https://www.mcpjam.com/blog/mcp-oauth-guide (2025-11-05)
- https://developers.google.com/identity/protocols/oauth2 (refresh token expiration)
- https://toolhouse.ai/pricing

Claims not verified in this run (do not treat as fact)
- Nango penetration-test reports / CVE list — [no source found beyond “we run pentests”]
- Exact Nango ARR and customer count — [single-source, vendor/jobs]
- Membrane/Integration.app, Knit, Anon, Keet, Panora 2026 live pricing — [not verified]
- Arcade Team $25/mo rate card — [single-source review; not on the pricing page we retrieved]
- Whether Composio helm charts yield a working self-host without an Enterprise key — [not verified]
- Nango GitHub issue #2802 maintainer reply text — page failed to load comments

---

M. 🧒 Простыми словами

Сейчас у нас пароли от почты лежат по разным компьютерам, как ключи от дома в разных карманах — и иногда ключ протухает, а мы не замечаем.

Нужен один сейф: положил ключ от Gmail/Календаря/Trello/Calendly, все роботы Claude берут оттуда, сейф сам обновляет ключ.

Telegram (как мы заходим «как человек») и Chrome в этот сейф не лезут — их не трогаем.

На завтра: бесплатный Composio, подключить четыре сервиса, дать Claude Code одну ссылку. Если разговор с Нанго важен — завести ещё и их облако. Свой сервер Нанго в первый день не поднимать: без дорогой лицензии он не умеет MCP.

🎙 Майк: идём верно, если цель — «роботы перестанут молча терять токены». Неверно, если цель — «один вендор заменит Telethon и Chrome». Вопрос к тебе: токены Gmail могут жить в американском облаке (наш клиент Google, не их) — да или нет? От этого A или D.
