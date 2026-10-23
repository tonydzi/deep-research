# DR26-09-15-MACANTON-01-0806 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Executive summary

1. [established] For this five-machine Claude Code fleet, Nango Cloud is the best long-term control plane: one connection record per account, encrypted credential storage, automatic refresh, raw proxy/tool/sync support, exportable credentials, auditability, MCP support, and a relatively portable code-first model. Its current free tier covers 10 connections; pay-as-you-go starts at $50/month with $0.29/connection, $0.72/compute-hour, and $0.50/GB. ([Nango pricing, accessed 2026-09-15](https://nango.dev/pricing); [Nango security, accessed 2026-09-15](https://nango.dev/docs/guides/platform/security))

2. [established] Composio is the fastest one-day proof of concept because it already exposes a single persistent MCP endpoint, manages authentication during tool use, has the broadest agent-oriented catalog in this comparison, and provides 100,000 tool calls/month free. It is more operationally convenient but more proprietary and execution-oriented than Nango. ([Composio Connect docs, accessed 2026-09-15](https://docs.composio.dev/docs/composio-connect); [Composio pricing, accessed 2026-09-15](https://composio.dev/pricing))

3. [established] Pipedream Connect is the strongest alternative when catalog breadth and mature actions matter more than self-hosting. It advertises managed auth for roughly 3,000 apps and 10,000+ tools, supports vendor-owned or bring-your-own OAuth clients, MCP, SDKs, custom requests, workflows, and triggers. Production Connect pricing is based on external users plus execution credits and is less transparent than Nango or Composio. ([Pipedream Connect, accessed 2026-09-15](https://pipedream.com/connect); [Pipedream pricing docs, accessed 2026-09-15](https://pipedream.com/docs/pricing))

4. [established] Paragon/ActionKit and Merge.dev primarily target customer-facing B2B product integrations. They are credible infrastructure, but their commercial packaging, embedded-user assumptions, and pricing make them poor fits for five machines sharing a handful of operator-owned accounts. Merge currently charges $650/month for up to 10 production linked accounts after three free accounts. ([Paragon pricing, accessed 2026-09-15](https://www.useparagon.com/pricing); [Merge pricing, accessed 2026-09-15](https://www.merge.dev/pricing/unified))

5. [established] Arcade is an authorization-and-tool runtime designed explicitly for agents. It has a sound two-identity model, where the project identifies the calling application and a user identity identifies the principal on whose behalf the action runs. Its $0.01/tool-call Team pricing can become expensive for a busy autonomous fleet. ([Arcade pricing, accessed 2026-09-15](https://www.arcade.dev/pricing/); [Arcade architecture paper, accessed 2026-09-15](https://www.arcade.dev/documents/mcp-runtimes-replace-gateways.pdf)) [single-source for the two-identity design]

6. [established] Telegram MTProto user sessions and an existing browser-extension MCP cannot be normalized safely into an ordinary SaaS OAuth catalog. Keep Telethon as a separately governed connector and register the Chrome extension MCP behind the same local gateway or client configuration. Treat them as exceptions visible in the credential inventory, not as fake Nango “providers.”

7. [established] Centralization solves silent refresh failure and credential drift only if every machine receives a non-provider fleet credential and never stores Google/Trello/Calendly refresh tokens locally. The hub should retain the upstream tokens; the nodes should receive narrowly scoped Nango/Composio environment keys or an internal gateway token. Nango logs credential refresh and API use and supports OpenTelemetry export. ([Nango security, accessed 2026-09-15](https://nango.dev/docs/guides/platform/security))

8. [established] Nango’s “self-hosted” offer is not a cheap community Docker escape hatch in the current product: the documented supported deployment requires an Enterprise subscription, fixed annual license/maintenance, and usage charges. It involves five Node services plus PostgreSQL, Redis, object storage, and Elasticsearch, so it is excessive for this lab. ([Nango self-hosting, accessed 2026-09-15](https://nango.dev/docs/guides/platform/self-hosting))

9. [emerging] The integration/auth category is converging with IAM and MCP infrastructure. Auth0 Token Vault, Okta Cross-App Access, Stytch Connected Apps, Cloudflare’s OAuth Provider Library, and the MCP authorization specification increasingly standardize token exchange, resource discovery, delegation, and agent identity. This will commoditize basic OAuth storage and refresh, but not maintained provider behavior, action schemas, retries, syncs, or observability. ([MCP authorization specification, 2025-06-18](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization); [Auth0 Token Vault, 2025-10-09](https://auth0.com/blog/auth0-token-vault-secure-token-exchange-for-ai-agents/); [Okta XAA, 2026-06-23](https://www.okta.com/newsroom/press-releases/okta-announces-cross-app-access-partners/))

10. Recommendation: run a one-day Composio Cloud bake-off for immediate fleet relief, while designing the stable abstraction around Nango-compatible connection IDs and raw HTTP/tool execution. If the four target integrations work reliably, migrate production credential custody to Nango Cloud and retain Composio or Pipedream as a catalog fallback. Confidence: [established]. Main assumption: the lab needs fewer than 10–20 active account connections and values credential control more than thousands of pre-built actions.

B. Proof of work

Search queries actually run

- `Nango docs self-hosting architecture Elastic License 2.0 pricing funding 2025`
- `Composio pricing self host license OAuth MCP funding 2025`
- `Paragon ActionKit pricing funding MCP connectors 2025`
- `Merge.dev pricing integrations funding 2025 unified API`
- `site:nango.dev/docs OAuth PKCE device flow client credentials token encryption scopes audit logs`
- `site:github.com/NangoHQ/nango license Elastic-2.0 providers.yaml Mistral DeepSeek Groq Together Fireworks Cohere Hugging Face`
- `Nango GitHub AGPL Elastic License change`
- `Nango founders Bastien Beurier Robin Guldener funding YC headcount jobs developer relations`
- `Pipedream Connect pricing MCP OAuth token security integrations funding`
- `Arcade.dev pricing MCP auth token vault funding connectors`
- `Klavis AI pricing self host license MCP connectors funding`
- `Unified.to pricing integrations OAuth MCP funding license`
- `Auth0 Token Vault AI agents 2025 OAuth RFC 9728 MCP`
- `Descope Agentic Identity Stytch Connected Apps Okta Cross App Access 2025`
- `MCP authorization specification 2025-06-18 2025-11-25 RFC 9728 dynamic client registration`
- `Zapier MCP OpenAI Apps SDK Cloudflare Agents OAuth MCP registry 2025 2026`
- `site:developers.google.com identity OAuth testing refresh token expires 7 days sensitive scopes verification Gmail`
- `site:docs.nango.dev google mail calendar provider setup redirect URI nango MCP Claude Code`
- `site:docs.composio.dev Claude Code MCP Gmail Calendar Trello Calendly setup command`
- `site:nango.dev/integrations Mistral DeepSeek Groq Together Fireworks Cohere Hugging Face`

URLs actually opened

- https://nango.dev/docs/guides/platform/self-hosting
- https://nango.dev/pricing
- https://nango.dev/docs/guides/platform/security
- https://github.com/NangoHQ/nango
- https://raw.githubusercontent.com/NangoHQ/nango/master/LICENSE
- https://raw.githubusercontent.com/NangoHQ/nango/master/providers.yaml — retrieval failed; no claims below rely on its contents.
- https://nango.dev/blog/nango-raises-7-5m-led-by-gradient
- https://www.ycombinator.com/companies/nango/jobs/oiAdChY-developer-relations-founding
- https://composio.dev/pricing
- https://docs.composio.dev/docs/composio-connect
- https://pipedream.com/docs/connect
- https://www.arcade.dev/pricing/
- https://www.merge.dev/pricing/unified
- https://www.useparagon.com/pricing
- https://unified.to/ — retrieval failed; search result used only where explicitly marked [single-source].
- https://aws.amazon.com/marketplace/pp/prodview-vekzgxp36lscq
- https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization
- https://auth0.com/features/token-vault
- https://auth0.com/blog/auth0-token-vault-secure-token-exchange-for-ai-agents/
- https://developers.cloudflare.com/agents/model-context-protocol/protocol/authorization/
- https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/
- https://stytch.com/blog/stytch-connected-apps/
- https://www.okta.com/newsroom/press-releases/okta-announces-cross-app-access-partners/

C. Product profiles

Catalog figures are not directly comparable. “Provider” usually means an authentication definition; “app” means a branded service; “action” or “tool” means an operation; and “unified API” vendors may count many upstream products behind a small number of normalized categories.

1. Nango

Category and architecture: [established] Auth, proxy, tools/MCP, triggers, webhooks, and one- or two-way sync infrastructure. Its current self-hosted architecture contains Server, Orchestrator, Jobs, Runner, Persist, PostgreSQL, object storage, Elasticsearch, and Redis. Cloud isolates Runner workloads by customer and separates PostgreSQL by workload. ([Self-hosting docs](https://nango.dev/docs/guides/platform/self-hosting), accessed 2026-09-15)

Authorization: [established] Per-user or per-organization connections; OAuth tokens, API keys, Basic/custom credentials, client credentials, configurable scopes and provider parameters, vendor-managed or BYO OAuth applications, refresh and expired-credential detection. PKCE is provider-dependent rather than a universal Nango-side promise. Device-flow coverage was not verified. ([API authorization overview](https://docs.nango.dev/guides), accessed 2026-09-15; [Salesforce setup](https://nango.dev/docs/api-integrations/salesforce/salesforce-api-oauth-app-setup), accessed 2026-09-15)

Storage: [established] AES-256-GCM application-level encryption for credentials, envelope encryption using AWS KMS in cloud, TLS 1.2+, environment isolation, audit trail, refresh/API-access logs, and OpenTelemetry export. Self-hosters supply their own 256-bit key. ([Security docs](https://nango.dev/docs/guides/platform/security), accessed 2026-09-15)

Catalog: [established] Nango claimed 1,000+ APIs in its 2026 seed announcement; the pricing page currently says 900+ APIs/MCPs. This discrepancy probably reflects different counting or page-update timing, but Nango does not publish a clear counting methodology on those pages. ([Nango funding announcement](https://nango.dev/blog/nango-raises-7-5m-led-by-gradient), 2026; [pricing](https://nango.dev/pricing), accessed 2026-09-15)

License/self-hosting: The repository exposes a license file, but its full text was not returned by the web reader. The exact current SPDX identifier and the alleged AGPL→Elastic-2.0 change therefore remain [no source found]. The documented supported self-hosted edition is Enterprise-only, not an inexpensive community deployment. All paid cloud features are said to be included. ([Repository license](https://raw.githubusercontent.com/NangoHQ/nango/master/LICENSE), accessed 2026-09-15; [self-hosting](https://nango.dev/docs/guides/platform/self-hosting), accessed 2026-09-15)

Pricing: [established] Free: 10 connections, 10 compute hours and 10 GB/month. PAYG: $50/month credit, $0.29/connection/month, $0.72/hour, $0.50/GB. Enterprise/self-hosting: quote. ([Pricing](https://nango.dev/pricing), accessed 2026-09-15)

Funding/team: [established] $7.5 million seed led by Gradient, with Horizon, YC and angels; Nango said it was cash-flow positive and at several million ARR when raising. The live YC DevRel posting says 14 people, 500+ customers, and mid-seven-figure ARR, but an older portion of the same listing says six people, $2 million funding and 250+ integrations. ([Funding announcement](https://nango.dev/blog/nango-raises-7-5m-led-by-gradient), 2026; [YC job](https://www.ycombinator.com/companies/nango/jobs/oiAdChY-developer-relations-founding), accessed 2026-09-15)

Launch/momentum: Project launched in 2023; YC W23; founders Bastien Beurier and Robin Guldener. The repository displayed 36 open issues and 88 open pull requests when opened. GitHub stars were not rendered, so the job listing’s “4,000 stars” should be treated as stale marketing data. ([GitHub](https://github.com/NangoHQ/nango), accessed 2026-09-15; [YC job](https://www.ycombinator.com/companies/nango/jobs/oiAdChY-developer-relations-founding), accessed 2026-09-15)

2. Composio

Category: agent tool router, managed authorization, hosted MCP, triggers, custom tools, and proxy execution.

Authorization/storage: [established] Persistent connected accounts, shared vendor OAuth clients or BYO apps, and self-managed credentials passed at execution time. The latter are neither stored nor refreshed by Composio. Enterprise advertises KMS; public evidence for its standard-plan encryption algorithm, BYOK details, revocation workflow, device flow, and audit retention was not found. ([Pricing](https://composio.dev/pricing), accessed 2026-09-15)

Catalog/MCP: [established] The live page claims 1,500+ toolkits; Composio Connect offers one hosted MCP endpoint and can authenticate during a session. “Toolkit” is not equivalent to application or provider. ([Pricing](https://composio.dev/pricing); [Connect docs](https://docs.composio.dev/docs/composio-connect), accessed 2026-09-15)

License/self-hosting: Cloud platform proprietary. Some SDK/server components are public, but a complete self-hosted replacement for managed auth/runtime was not verified: [no source found].

Pricing: Free 100,000 tool calls, 50,000 triggers, three team members; Pro $29/month; base overage $0.0003/tool call; proxy and execution add-ons; vendor-managed connections up to 1,000 free and then $0.10/connection on paid plans. ([Pricing](https://composio.dev/pricing), accessed 2026-09-15)

Funding/team/launch: [no source found] in the opened sources.

3. Pipedream Connect

Category: managed auth, proxy, actions/triggers, workflow engine, SDK and MCP gateway.

Authorization/storage: [established] Approved Pipedream OAuth clients or BYO clients; API-key acceptance; encrypted credentials and tokens; refresh management. Connect states that it does not store proxy/MCP API request or response bodies. Detailed cipher/KMS/BYOK and device-flow support were not verified. ([Connect docs](https://pipedream.com/docs/connect), accessed 2026-09-15)

Catalog: [established] Current product page advertises managed authentication for approximately 3,000 apps and 10,000+ tools. The differing 2,700/3,000 figures visible on the page show live marketing-transition inconsistency. ([Pipedream Connect](https://pipedream.com/connect), accessed 2026-09-15)

License/self-hosting: Managed control plane is proprietary. Individual components/actions are publicly visible; no complete self-hosted Connect control plane was verified.

Pricing: Development mode is free; production bills external users plus credits, with one credit per up-to-30-second operation. Current public documentation does not expose an easily comparable production dollar amount. ([Pricing docs](https://pipedream.com/docs/pricing), accessed 2026-09-15)

Funding/team: [no source found].

4. Paragon and ActionKit

Category: embedded iPaaS with managed authentication, workflows, managed data sync, and ActionKit’s agent-facing API/MCP actions.

Authorization: Embedded per-connected-user model, managed auth, headless or hosted connect UI. Exact cipher, OAuth grant matrix, KMS/BYOK, device flow and refresh implementation are [no source found].

Catalog: “Dozens” of integrations and “hundreds” of ActionKit operations are suggested by product language, but no stable current total was verified.

License/self-host: Proprietary; self-host/forward-deploy appears only under Enterprise. ([Pricing](https://www.useparagon.com/pricing), accessed 2026-09-15)

Pricing: No public dollar schedule; every plan includes unlimited integrations and the product suite, with usage related to connected users. [single-source] ([Pricing](https://www.useparagon.com/pricing), accessed 2026-09-15)

Funding/team: [no source found].

5. Merge.dev

Category: normalized unified APIs and synchronized common data models, plus authenticated passthrough and MCP.

Authorization/storage: Per “linked account,” embedded link UI, provider auth managed by Merge, scopes and field-level controls. Encryption algorithm, BYOK and device-flow support were not found.

Catalog: “Hundreds” of integrations across category-specific unified APIs, rather than thousands of arbitrary agent tools. Exact live count was not verified.

License/self-host: Proprietary managed service. Single-tenancy can be purchased; full self-hosting was not verified. ([Pricing](https://www.merge.dev/pricing/unified), accessed 2026-09-15)

Pricing: First three production linked accounts free; $650/month for up to 10; $65/account thereafter; audit trail and advanced enterprise controls move up-plan. ([Pricing](https://www.merge.dev/pricing/unified), accessed 2026-09-15)

Funding/team: [no source found].

6. Arcade.dev

Category: agent-native authorization and action runtime, MCP framework and tool registry.

Authorization: Project/application identity plus end-user identity; authorization is evaluated in the context of the user and action. Arcade argues that token refresh, retry and authorization must be coupled to tool execution. ([Architecture paper](https://www.arcade.dev/documents/mcp-runtimes-replace-gateways.pdf), accessed 2026-09-15) [single-source]

Deployment/license: Fully managed cloud on Free/Team; Enterprise offers VPC or air-gapped deployment. Exact licenses of its open components and which cloud services remain required are [no source found].

Pricing: Free 2,000 auth events and 2,000 tool calls/month. Team $25/month plus $0.10/auth event and $0.01/tool call. SSO, RBAC, audit logs, and private registry are Enterprise. ([Pricing](https://www.arcade.dev/pricing/), accessed 2026-09-15)

Catalog/funding/team: [no source found].

7. Unified.to

Category: real-time unified API plus MCP, embedded authorization, normalized objects/scopes/errors, raw passthrough and virtual webhooks.

Authorization: OAuth authorization produces a connection ID; it supports connection-bound remote MCP URLs over Streamable HTTP and SSE. Encryption/BYOK/device-flow details were not verified.

Catalog: 34 API categories and 60+ identity providers were advertised in the search result, but the product page failed to open; mark [single-source]. ([Unified.to](https://unified.to/), accessed unsuccessfully 2026-09-15)

License/self-host/pricing/funding: Proprietary hosted product; concrete current figures and complete self-hosting were [no source found].

8. Klavis AI

Category: hosted or self-hosted MCP servers, managed OAuth, REST and Python/TypeScript SDKs, plus the Strata progressive-discovery gateway.

Authorization: Built-in OAuth 2.0, multi-tenant and user-level controls; detailed token encryption and KMS evidence was not found.

Catalog: The AWS listing names GitHub, Slack, Salesforce, Notion, HubSpot, Gmail and others but provides no defensible total. It claims 5,300 GitHub stars and +13–15% tool-performance improvements; both are vendor claims and [single-source]. ([AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-vekzgxp36lscq), accessed 2026-09-15)

License/self-host: The listing calls it open source and says Docker self-hosting is available; exact SPDX and cloud-only feature boundary were not verified.

Funding/team: YC X25 and founders from Google DeepMind/Lyft are claimed in the vendor-authored AWS listing. Round amount was not found. [single-source]

9. Auth0 Token Vault

Category: auth-only/token-exchange layer, not a broad action catalog.

Authorization: [established] Standard OAuth consent stores provider refresh tokens; an Auth0 token is exchanged using RFC 8693 for a short-lived provider token. Agents need not see refresh tokens. Revocation invalidates the vault connection. More than 30 providers are supported. ([Token Vault feature page](https://auth0.com/features/token-vault), accessed 2026-09-15; [technical article](https://auth0.com/blog/auth0-token-vault-secure-token-exchange-for-ai-agents/), 2025-10-09)

Storage/license/pricing: Hosted Auth0 service; encryption implementation, BYOK and price were not publicly established in the opened material. It does not itself provide Gmail/Trello action schemas.

10. Stytch Connected Apps

Category: makes your own application an OAuth/OIDC provider for agents and integrations; it is the inverse of Nango’s “connect my agent to third-party SaaS” problem.

Authorization: Issues scoped access tokens, supports consent and cross-device/session scenarios. It does not create a generic catalog of third-party API actions. ([Stytch announcement](https://stytch.com/blog/stytch-connected-apps/), 2025-02-20)

Pricing/license/funding: [no source found].

11. Okta Cross-App Access

Category: enterprise governance and delegated identity protocol, not an integration action catalog.

Authorization: [established] XAA extends OAuth so enterprise policy and audit can follow agent-to-app connections; Okta’s 2026 ecosystem announcement lists 25+ adopters and infrastructure partners. ([Okta announcement](https://www.okta.com/newsroom/press-releases/okta-announces-cross-app-access-partners/), 2026-06-23)

Fit: Excellent future governance layer for a large Okta estate; disproportionate for this lab and not a replacement for connector implementations.

12. Cloudflare Agents/OAuth

Category: infrastructure for deploying and authorizing remote MCP servers, plus Cloudflare’s own MCP servers.

Authorization: [established] Cloudflare’s OAuth Provider Library implements the provider side of MCP OAuth; it can delegate login to Auth0, WorkOS, Descope or custom handlers. ([Authorization docs](https://developers.cloudflare.com/agents/model-context-protocol/protocol/authorization/), updated 2026-07-27)

Catalog/architecture: Cloudflare’s own API MCP compresses 2,594 endpoints into `search` and `execute` tools through isolated-worker code execution, reducing advertised schema context from hundreds of thousands of tokens to approximately 1,000. Those measurements are Cloudflare’s own and [single-source]. ([Cloudflare MCP](https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/), updated 2026-07-28)

Fit: Strong substrate for a custom fleet gateway; it does not provide the required Gmail/Trello/Calendly catalog by itself.

13. Zapier MCP/Connectors

Category: agent-facing access to Zapier’s automation catalog.

Catalog, pricing, auth, license and production architecture were not verified from an opened authoritative source in this run. Treat it as a credible bake-off candidate, not a decision-grade recommendation: [no source found].

14. Apideck, Truto, Integration.app, Membrane, Panora, Vessel, Hotglue, Finch, Knit, Nylas, Anon, Keet, Tray Embedded, Workato Embedded, Pica and Toolhouse

These are credible adjacent names, but their current authorization, encryption, price, funding and licensing claims were not verified from opened sources in this run. It would be misleading to manufacture profiles from memory. They remain [no source found] and should enter phase two only if the shortlist fails.

D. Comparative decision table

| Product | Category | Auth model and storage | Catalog | MCP | Self-host/license | Pricing entry | Funding/stage | Fleet fit |
|---|---|---|---:|---|---|---|---|---|
| Nango | Auth + proxy + tools + sync | User/org connections; OAuth/API key/custom; AES-256-GCM + KMS; refresh and audit | 900+/1,000+ APIs, counting unclear | Yes | Enterprise-supported self-host; exact current SPDX unverified | Free 10 connections; PAYG $50 | $7.5M seed in 2026; YC W23 | 5/5: best credential control and portability |
| Composio | Agent tool router | Managed or BYO OAuth apps; persistent connected accounts; KMS Enterprise | 1,500+ toolkits | Native single endpoint | Managed platform proprietary; full self-host unverified | Free 100K calls; Pro $29 | [no source found] | 5/5 POC, 4/5 durable: easiest Claude Code path |
| Pipedream Connect | Embedded integrations/workflows | Vendor or BYO OAuth; encrypted token store and refresh | ~3,000 apps; 10K+ tools | Native | Managed control plane | Dev free; production user + credit billing | [no source found] | 4/5: superb breadth, less predictable economics |
| Paragon/ActionKit | Embedded iPaaS + agent actions | Managed per-connected-user auth | Dozens of apps/hundreds of actions, unverified | ActionKit MCP | Enterprise self/forward deployment; proprietary | Quote | [no source found] | 2/5: customer-facing product bias |
| Merge.dev | Unified APIs/sync | Per-linked-account; managed OAuth and scopes | Hundreds, category-normalized | Yes | Proprietary; purchasable single tenant | 3 free, then $650/10 | [no source found] | 2/5: expensive and wrong abstraction for arbitrary tools |
| Arcade | Agent auth/action runtime | App + user identity; scoped action execution | [no source found] | Native/framework | Cloud; VPC/air-gap Enterprise | Free 2K; Team $25 + usage | [no source found] | 3/5: strongest authorization semantics, call cost risk |
| Unified.to | Unified API + MCP | Per-connection IDs; embedded OAuth | 34 categories [single-source] | Native per connection | Proprietary; self-host unverified | [no source found] | [no source found] | 3/5: attractive normalization, evidence incomplete |
| Klavis AI | MCP gateway/runtime | Managed OAuth, user/multi-tenant access | No verified total | Core product | Docker self-host claimed; SPDX unverified | Free Hobby claimed | YC X25; amount unverified | 3/5: promising and agent-native, young |
| Auth0 Token Vault | Token vault/auth exchange | RFC 8693 exchange; OAuth refresh token retained in vault | 30+ providers | Indirect | Hosted/proprietary | [no source found] | Auth0/Okta product | 3/5: excellent custody, tools still must be built |
| Cloudflare Agents | MCP hosting/OAuth substrate | OAuth 2.1 provider library; external or custom IdP | Cloudflare APIs; not generic SaaS | Native | Workers-based; library available | Cloudflare usage pricing [not verified] | Public-company platform | 3/5: best custom gateway substrate |
| Stytch Connected Apps | Outbound OAuth provider | Your app becomes IdP; scoped tokens | Not a connector catalog | Supports agent use | Hosted/proprietary | [no source found] | [no source found] | 1/5: solves the reverse problem |
| Okta XAA | Enterprise identity governance | Policy-governed agent/app delegation | 25+ ecosystem participants | MCP auth extension | Enterprise proprietary | [no source found] | Public-company platform | 1/5 now; 4/5 in a future enterprise estate |

E. Fit for this fleet

What can replace the current connectors

- Gmail and Google Calendar: Nango, Composio, Pipedream and probably Paragon can centralize the OAuth connection. Nango’s strongest advantage is that a connection scopes credential access to integration code and can proxy arbitrary provider calls without returning the token to the agent. ([Nango Functions](https://nango.dev/docs/guides/functions/functions-guide), accessed 2026-09-15)

- Trello: Nango, Composio and Pipedream can represent API-key/OAuth credentials and execute catalog or raw API operations. The exact current completeness of each Trello action catalog was not verified.

- Calendly: The same three can centralize its OAuth/API credential. Exact trigger/action parity was not verified.

- Telegram: Telethon uses an MTProto user session, not a standard SaaS OAuth/API-key connection. A generic custom-secret provider might store the session string, but sending it through third-party cloud infrastructure increases account-takeover exposure and does not give the platform MTProto refresh/session semantics. Keep it in an internal encrypted store and expose a narrowly permissioned Telegram MCP.

- Chrome extension MCP: already a tool server, not an upstream credentialed SaaS API. Register it beside the unified integration MCP. Do not tunnel browser session cookies through Nango or Composio.

Recommended topology

```text
Claude Code on five nodes
        |
        | fleet-specific MCP/API credential
        v
small internal MCP policy gateway
        |
        +-- Nango: Gmail, Calendar, Trello, Calendly
        +-- Telethon MCP: Telegram exception
        +-- existing extension MCP: browser exception
```

The internal gateway should map `principal + machine + service + account alias` to an upstream connection ID. It should expose semantic aliases such as `lab_google_primary`, not vendor connection IDs, and log actor, machine, tool, connection alias, result and timestamp. This is a design recommendation, not a product claim.

Secrets model

- Provider refresh tokens remain only in Nango/Composio/Pipedream.
- Each node receives a distinct gateway credential so access can be revoked per machine.
- The gateway enforces tool allowlists and read/write separation.
- OAuth application client secrets stay at the hub/control plane.
- Agents receive short-lived provider access only if unavoidable; proxy execution is preferable.
- Back up connection metadata and account mappings, not plaintext refresh tokens.
- Quarterly export/reconnect drills test portability.

F. One-day adoption plan

The safest one-day plan uses Composio Cloud for the bake-off because the documented single MCP endpoint avoids writing an MCP adapter. It should be followed by a Nango production evaluation.

1. Hour 0–1: inventory and freeze

   1. Enumerate the four current integrations, account aliases, current scopes, credential location, last successful refresh, and read/write requirements.
   2. Mark Telegram and Chrome MCP as explicit exceptions.
   3. Do not delete existing tokens or disable existing connectors.
   4. Define acceptance tests: Gmail search and draft, Calendar list/create test event, Trello list/create test card, Calendly read event types.
   5. Define rollback: remove the new MCP endpoint and restore old connector routing.

2. Hour 1–2: create the integration workspace

   1. Create one Composio project/environment for the lab.
   2. Use one stable entity/user identifier for the human account; do not create one identity per machine.
   3. Prefer BYO Google OAuth credentials for durability; Composio itself says its shared applications are easy to start with but not recommended for scaling. ([Composio pricing](https://composio.dev/pricing), accessed 2026-09-15)
   4. Obtain the persistent Composio Connect MCP URL from the project UI. The documented generic endpoint is `https://connect.composio.dev/mcp`. ([Composio Connect docs](https://docs.composio.dev/docs/composio-connect), accessed 2026-09-15)

3. Hour 2–4: Google OAuth

   1. In Google Cloud, enable Gmail API and Google Calendar API.
   2. Configure the OAuth consent screen and add only the lab Google accounts as test users.
   3. Create a Web application OAuth client.
   4. Copy the exact callback/redirect URI displayed by Composio or Nango into Google Cloud; do not guess it.
   5. Start with minimum scopes:
      - Gmail metadata/read as required.
      - Gmail compose/send only if agents genuinely need it.
      - Calendar read-only or events scope depending on the acceptance test.
      - `openid`, `email`, and `profile` for account identity.
   6. Request offline access so the server receives a refresh token. Google’s server-side authorization flow returns a refresh token on the initial offline exchange. ([Gmail server authorization](https://developers.google.com/workspace/gmail/api/auth/web-server), accessed through search 2026-09-15) [single-source because this URL appeared in search but was not separately opened; treat command details as authoritative Google documentation]
   7. Pitfall: an External app in Testing normally receives refresh tokens that expire after seven days unless only basic identity scopes are requested. Gmail/Calendar access therefore will not make a durable deployment while the app remains in that state. ([Google OAuth 2.0](https://developers.google.com/identity/protocols/oauth2), accessed through search 2026-09-15)
   8. Pitfall: sensitive/restricted scopes trigger warnings, user caps and verification requirements. Broad restricted Gmail scopes may require an independent security assessment for a public external application. ([Google production readiness](https://developers.google.com/identity/protocols/oauth2/production-readiness/overview), accessed through search 2026-09-15; [restricted-scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification), accessed through search 2026-09-15)
   9. For a Workspace domain, an Internal app or an administrator-trusted app can avoid some external-testing restrictions. Confirm the actual account type before relying on this.

4. Hour 4–5: connect Trello and Calendly

   1. Enable their Composio toolkits.
   2. Use OAuth when available; otherwise enter the smallest-scope API token.
   3. Store a single connection for each human/account, not five copies.
   4. Verify that reconnection and revocation are visible centrally.
   5. If the required Trello or Calendly operation is absent, test proxy/custom-tool execution before rejecting the platform.

5. Hour 5–6: expose to Claude Code

   Where the Claude Code build supports a remote HTTP MCP endpoint, configure one named server:

```json
{
  "mcpServers": {
    "lab-integrations": {
      "url": "https://connect.composio.dev/mcp"
    }
  }
}
```

   The exact Claude Code configuration filename varies by installation and scope; this research did not open an authoritative Claude Code configuration page, so filename and CLI commands are [no source found]. Do not blindly place credentials in a repository-level file. Configure the server using the installed client’s documented MCP command or user-level settings.

   On each machine:

   1. Add the same MCP endpoint.
   2. Authenticate as the same Composio entity/account.
   3. Keep any endpoint token in the OS secret store or user-level protected configuration.
   4. Give each machine a separate project/gateway credential if Composio’s access model permits; otherwise put a small internal gateway in front before production.

6. Hour 6–7: positive tests

   1. Gmail: search for a known message; create a draft but do not send.
   2. Calendar: list the next event; create and then delete a clearly named test event.
   3. Trello: read one board; create and archive a test card.
   4. Calendly: list event types and one scheduled event.
   5. Record tool name, machine, account alias, result, latency and whether the agent ever saw a raw token.

7. Hour 7–8: failure and security tests

   1. Revoke one low-risk test connection and confirm a clear error.
   2. Reconnect and confirm the new credential is used on every machine without copying a provider token.
   3. Attempt an unapproved write action and verify it is blocked.
   4. Verify the MCP URL/key is not present in Git, shell history, logs or agent transcripts.
   5. Confirm no Gmail content is retained in platform logs beyond the selected plan’s documented behavior.
   6. Keep the old connectors as rollback for at least one week.

Acceptance gate

Adopt only if all four ordinary SaaS integrations pass, refresh/revocation state is centrally visible, no provider token reaches a node, and every machine can be individually disabled. If Composio fails the observability or machine-isolation requirements, run the same tests on Nango Cloud.

G. Risks and pricing cliffs

Vendor lock-in

- Nango’s provider definitions and TypeScript functions are relatively portable, and its docs say credentials can be exported. Its connection identifiers, hosted execution, record cache and sync scheduling remain proprietary control-plane dependencies. ([Nango authorization overview](https://docs.nango.dev/guides), accessed 2026-09-15)
- Composio locks applications into toolkit/action names, session semantics, connected-account IDs and its router. Self-managed credentials improve custody but surrender automatic refresh.
- Pipedream’s strongest lock-in is its huge component catalog and hosted workflow runtime.
- Merge’s normalized common models create the deepest schema lock-in: the normalization is the product.
- Paragon adds workflow and embedded-UI lock-in.
- MCP reduces client lock-in but does not make authentication records, action semantics or event delivery portable.

Self-host viability

- Nango’s supported self-host requires Enterprise and at least five services plus four state/storage systems. For this lab, operational cost likely exceeds cloud risk. ([Self-hosting](https://nango.dev/docs/guides/platform/self-hosting), accessed 2026-09-15)
- Paragon reserves self/forward deployment for Enterprise. ([Paragon pricing](https://www.useparagon.com/pricing), accessed 2026-09-15)
- Arcade reserves VPC/air-gap deployment for Enterprise. ([Arcade pricing](https://www.arcade.dev/pricing/), accessed 2026-09-15)
- Klavis claims Docker self-hosting, but feature parity and exact licensing remain unverified.
- Composio and Pipedream do not have verified full self-hosted equivalents in the material reviewed.

Pricing cliffs

At 5–10 connections, Nango Free or PAYG is reasonable; at 50 persistent connections, the nominal connection component would be only $14.50/month, but the $50 base credit and compute/data dominate. ([Nango pricing](https://nango.dev/pricing), accessed 2026-09-15)

Composio’s free tier is unusually generous, but proxy, direct execution, sandboxing, shared connections, compliance and hosted-provider actions layer incremental per-call fees. ([Composio pricing](https://composio.dev/pricing), accessed 2026-09-15)

Arcade’s 100,000 monthly calls would cost roughly $1,000 in call fees before auth events and platform fee at the published Team rate. That is arithmetic based on $0.01/call, not a vendor quote. ([Arcade pricing](https://www.arcade.dev/pricing/), accessed 2026-09-15)

Merge’s $650/month for ten linked accounts is plainly uneconomic for this fleet. ([Merge pricing](https://www.merge.dev/pricing/unified), accessed 2026-09-15)

Security

A centralized vault reduces token sprawl but creates a high-value compromise point. Required controls are per-node identities, connection-level authorization, short-lived downstream credentials, outbound provider allowlists, audit export, refresh/revocation alerts, and a tested break-glass path.

No verified Nango, Composio, Pipedream, Paragon, Merge or Arcade security incident or product-specific CVE was identified in the opened material. That is not evidence that none exists: [no source found].

Company-survival risk

- Nango is a small seed-stage company despite reported revenue traction. Its supported self-hosting being Enterprise-licensed means survival risk is not eliminated by source availability.
- Klavis appears younger and therefore carries higher execution and support risk.
- Pipedream, Merge and Paragon appear more mature, but current funding/runway figures were not verified.
- Auth0/Okta and Cloudflare have far lower company-survival risk, but they do not replace the connector/tool catalog.

Consolidation or acquisitions during 2025–2026 were not verified from opened sources: [no source found].

H. Nango deep dive and critique

Architecture

Nango now spans four layers:

1. Authorization and connection custody.
2. Authenticated proxy calls.
3. TypeScript integration functions for tools, triggers, webhooks and syncs.
4. Persisted sync records and logs.

The self-hosted deployment includes Server, Orchestrator, Jobs, Runner, Persist, PostgreSQL, Redis, object storage and Elasticsearch. The current documentation does not mention Temporal as an active component. Therefore the prompt’s “Temporal history” cannot be confirmed as current architecture; it may describe an earlier implementation: [no source found]. ([Self-hosting](https://nango.dev/docs/guides/platform/self-hosting), accessed 2026-09-15)

Authorization strengths

- Credentials are scoped to a connection and function invocation.
- Tokens need not be returned to the application or agent.
- Vendor-managed and BYO OAuth clients coexist.
- OAuth scope/parameter overrides are supported.
- AES-256-GCM plus KMS envelope encryption is clearly documented.
- Refresh operations and credential access are logged.
- Credential export reduces hard lock-in. ([Security](https://nango.dev/docs/guides/platform/security); [Functions](https://nango.dev/docs/guides/functions/functions-guide); [authorization overview](https://docs.nango.dev/guides), accessed 2026-09-15)

License-history finding

The requested assertion “AGPL → Elastic 2.0” could not be verified. The live raw license URL opened but its body was not returned by the research tool. No authoritative Nango post explaining a change date or reason was found. Exact SPDX, transition date and motive must therefore be treated as [no source found], not repeated as fact.

Self-hosting critique

[established] “Open-source” positioning and practical supported self-hosting are not synonymous. Current docs say Enterprise subscription, fixed annual license/maintenance, partial cloud-style usage charges, and a Kubernetes/Helm-scale stack. This is not a credible low-cost self-host route for a small lab. ([Self-hosting](https://nango.dev/docs/guides/platform/self-hosting), accessed 2026-09-15)

Catalog and missing AI providers

The live catalog search and `providers.yaml` could not be read successfully. Therefore the absence of Mistral, DeepSeek, Groq, Together AI, Fireworks, Cohere and Hugging Face was not verified. Each claim is [no source found].

A plausible explanation is that Nango historically optimized for end-user/tenant authorization to SaaS systems, whereas many inference vendors use a server-side API key owned by the developer. That makes them less valuable as reusable OAuth connection definitions. This is an inference, not a documented Nango policy: [speculative].

Adding providers

The repository exposes contributions and provider definitions, but the exact current `providers.yaml` contribution workflow, documentation checklist, response SLA and review latency were not verified. The visible repository state was 36 issues and 88 pull requests, but those totals include all types of work and do not prove slow review. ([GitHub](https://github.com/NangoHQ/nango), accessed 2026-09-15)

Community and DevRel footprint

- The repository had 36 visible issues and 88 PRs at access time.
- Nango has product docs, a blog, integration examples and public GitHub.
- The live YC posting seeks a Founding Developer Relations person at $150,000–$250,000 plus 0.10%–1.00% equity in San Francisco. ([YC job](https://www.ycombinator.com/companies/nango/jobs/oiAdChY-developer-relations-founding), accessed 2026-09-15)
- That role is evidence that DevRel is viewed as strategically missing or underbuilt, although that interpretation is [emerging], not something Nango states.
- Slack/Discord activity, YouTube cadence, conference talks, template-marketplace activity and median issue/PR response times were not measured: [no source found].
- No defensible current G2, HN or Reddit complaint set was verified: [no source found].

Funding, founders and headcount

Nango announced a $7.5 million seed led by Gradient with Horizon, YC and named angels. It said it was already cash-flow positive and producing several million ARR. Founders are Bastien Beurier and Robin Guldener. The current job copy says 14 staff, 500+ customers and mid-seven-figure ARR. These are company claims and thus [single-source], though the funding round and YC listing corroborate the company’s existence and stage. ([Funding](https://nango.dev/blog/nango-raises-7-5m-led-by-gradient), 2026; [YC job](https://www.ycombinator.com/companies/nango/jobs/oiAdChY-developer-relations-founding), accessed 2026-09-15)

Ten defensible weaknesses

1. Enterprise-only supported self-hosting conflicts with the intuitive promise of open-source deployability. ([Self-hosting](https://nango.dev/docs/guides/platform/self-hosting))
2. The self-host stack is operationally heavy for small teams.
3. Catalog counts conflict: 900+ on pricing versus 1,000+ in the funding announcement. ([Pricing](https://nango.dev/pricing); [funding announcement](https://nango.dev/blog/nango-raises-7-5m-led-by-gradient))
4. “API,” “provider,” “integration,” “MCP” and “template” counts are not clearly separated on headline pages.
5. The free tier’s 10-connection cap is small compared with Composio’s connection allowance. ([Nango pricing](https://nango.dev/pricing); [Composio pricing](https://composio.dev/pricing))
6. The supported self-host price is not public.
7. The architecture requires multiple data stores and services, increasing backup and upgrade complexity.
8. Compared with Composio and Pipedream, Nango’s agent-facing turnkey action breadth is less obviously discoverable.
9. Public proof of device authorization, universal PKCE behavior and fine-grained machine-to-connection authorization is incomplete.
10. The live Founding DevRel vacancy suggests the ecosystem story, educational surface and community feedback loop are not yet staffed at the maturity implied by Nango’s platform ambitions. This is an inference: [emerging].

What Nango does better

- Better credential-custody documentation than Composio in the material reviewed: exact AES mode, key sizes, IV, tag and KMS envelope design are public.
- More transparent low-volume connection/compute/data pricing than Paragon and Pipedream.
- More portable than Merge’s normalized common-model architecture.
- Supports raw proxy calls, arbitrary TypeScript integration code and syncs in one connection model.
- Offers credential export.
- Preserves provider-native semantics instead of forcing all services into one normalized schema.
- Compared with Composio, Nango more clearly separates authentication infrastructure from the tool catalog and lets developers own deeper integration logic.
- Compared with Paragon, it is more code-first and less tied to embedded workflow-builder UX.
- Compared with Merge, it fits cross-category integrations better.
- The stated cloud/self-host feature parity is stronger than products that reserve functional capabilities for cloud, although the enterprise cost limits practical access. ([Self-hosting](https://nango.dev/docs/guides/platform/self-hosting))

I. Emerging trends

Agent authorization is splitting into three distinct problems:

1. Agent authenticates to MCP server.
2. MCP server/agent obtains delegated authority to a downstream SaaS account.
3. Enterprise governs the delegation chain and records which human, agent and application caused an action.

[established] The MCP 2025-06-18 authorization specification requires OAuth 2.1 authorization servers, RFC 9728 Protected Resource Metadata, RFC 8414 Authorization Server Metadata and `WWW-Authenticate` discovery; it recommends RFC 7591 Dynamic Client Registration. ([MCP specification](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization), 2025-06-18)

The requested 2025-11 revision and any later replacement of DCR with Client ID Metadata Documents were not verified from an opened primary specification page: [no source found].

[established] Auth0 Token Vault covers downstream token custody and RFC 8693 exchange; the agent need not possess the provider refresh token. ([Auth0 article](https://auth0.com/blog/auth0-token-vault-secure-token-exchange-for-ai-agents/), 2025-10-09)

[established] Stytch Connected Apps enables a SaaS vendor to turn its own application into an OAuth provider for third-party agents. This solves the resource-provider side, not the catalog side. ([Stytch](https://stytch.com/blog/stytch-connected-apps/), 2025-02-20)

[established] Okta XAA adds enterprise policy and audit to cross-app agent delegation and has an ecosystem of gateways and frameworks. ([Okta](https://www.okta.com/newsroom/press-releases/okta-announces-cross-app-access-partners/), 2026-06-23)

[established] Cloudflare supplies deployable MCP OAuth infrastructure and demonstrates “search then execute” as an answer to tool-schema overload. ([Cloudflare authorization](https://developers.cloudflare.com/agents/model-context-protocol/protocol/authorization/); [Cloudflare MCP servers](https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/), updated July 2026)

[emerging] These standards commoditize login, discovery and token exchange, but a standards-compliant vault does not automatically provide working Gmail draft semantics, Calendly pagination, Trello rate-limit handling, webhook repair, schema evolution or operator-visible recovery. The durable commercial moat shifts toward maintained connector behavior, action quality, policy enforcement, observability and evaluation.

Anthropic, OpenAI and Google first-party connector-directory details were not verified from opened primary sources in this run: [no source found].

J. Talking points for Nango’s CTO

Five hard questions

1. “What exactly is licensed under which SPDX identifier today, what changed from the original license, and why is supported self-hosting now Enterprise-only?”
2. “Why do pricing and fundraising pages say 900+ and 1,000+ APIs respectively? Can the catalog expose separate counts for auth providers, tested tool templates, syncs and MCP servers?”
3. “What is the authorization boundary between an agent, a machine, a human account and a Nango connection? Can policy deny one machine’s write access while preserving another machine’s read access?”
4. “How quickly does a community provider PR reach production, and what automated conformance suite proves refresh, revocation, pagination, retries and scope minimization?”
5. “As Auth0, Okta, Cloudflare and MCP standardize agent authorization, what remains Nango’s defensible layer: provider maintenance, tool quality, sync runtime, evaluation data, or the connection graph?”

Five things to bring as DevRel

1. A public catalog-quality dashboard separating authentication coverage from production-tested actions, triggers and syncs.
2. A “five machines, one human, four SaaS accounts” reference architecture with per-machine identity, revocation, auditing and secret-free agents.
3. Automated recipes and failure labs for Google’s seven-day testing-token expiry, incremental scopes, revoked refresh tokens and OAuth-client migration.
4. A provider contribution kit: generator, test harness, mock OAuth server, review SLA, ownership metadata and maintenance-health badge.
5. A monthly evidence-based connector report: additions, breakages, median repair time, stale integrations, PR response time, security changes and real compatibility tests across Claude Code, Codex and other MCP clients.

K. Disagreements and unresolved evidence

- Nango catalog: pricing says 900+; its 2026 fundraising article says 1,000+. Both are first-party. No counting reconciliation was found.
- Nango staff/funding: older YC copy says six people and $2 million; current role copy says 14 people while the company separately announced a $7.5 million seed. The old block is evidently stale but remains live.
- Pipedream catalog: its page contained both 2,700 and 3,000 managed-auth app figures. Use “approximately 3,000,” not an exact total.
- Klavis performance and star counts come from its own AWS listing and were not independently validated.
- Nango’s alleged AGPL→Elastic-2.0 transition was not verified.
- Absence of the seven named AI providers from Nango’s current catalog was not verified because the live catalog/provider file could not be read.
- Funding totals, valuations and headcounts for most competitors were not established from opened sources.
- No verified security-incident/CVE inventory was obtained.
- Exact Claude Code MCP configuration filename/CLI was not established from an opened official source.
- Current details for Apideck, Truto, Integration.app, Panora, Hotglue, Finch, Knit, Nylas, Tray, Workato, Pica, Toolhouse and related players remain unverified.

L. Full source list

All pages below were accessed on 2026-09-15 unless a publication/update date is stated.

- Nango, “Self-host Nango”: https://nango.dev/docs/guides/platform/self-hosting
- Nango pricing: https://nango.dev/pricing
- Nango security documentation: https://nango.dev/docs/guides/platform/security
- Nango authorization overview: https://docs.nango.dev/guides
- Nango Functions guide: https://nango.dev/docs/guides/functions/functions-guide
- Nango Salesforce OAuth setup: https://nango.dev/docs/api-integrations/salesforce/salesforce-api-oauth-app-setup
- Nango GitHub repository: https://github.com/NangoHQ/nango
- Nango raw license: https://raw.githubusercontent.com/NangoHQ/nango/master/LICENSE
- Nango `providers.yaml`: https://raw.githubusercontent.com/NangoHQ/nango/master/providers.yaml — retrieval failed.
- Nango, “Nango raises $7.5M led by Gradient,” 2026: https://nango.dev/blog/nango-raises-7-5m-led-by-gradient
- YC, “Developer Relations (Founding) at Nango”: https://www.ycombinator.com/companies/nango/jobs/oiAdChY-developer-relations-founding
- Composio pricing: https://composio.dev/pricing
- Composio Connect documentation: https://docs.composio.dev/docs/composio-connect
- Pipedream Connect documentation: https://pipedream.com/docs/connect
- Pipedream Connect product page: https://pipedream.com/connect
- Pipedream pricing documentation: https://pipedream.com/docs/pricing
- Paragon pricing: https://www.useparagon.com/pricing
- Merge Unified pricing: https://www.merge.dev/pricing/unified
- Arcade pricing: https://www.arcade.dev/pricing/
- Arcade, “Why MCP Runtimes Replace API Gateways for Agentic AI”: https://www.arcade.dev/documents/mcp-runtimes-replace-gateways.pdf
- Unified.to: https://unified.to/ — page retrieval failed.
- Klavis AI AWS Marketplace listing: https://aws.amazon.com/marketplace/pp/prodview-vekzgxp36lscq
- MCP Authorization Specification, 2025-06-18: https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization
- Auth0 Token Vault feature page: https://auth0.com/features/token-vault
- Auth0, “Token Vault: Secure Token Exchange for AI Agents,” 2025-10-09: https://auth0.com/blog/auth0-token-vault-secure-token-exchange-for-ai-agents/
- Cloudflare MCP authorization, updated 2026-07-27: https://developers.cloudflare.com/agents/model-context-protocol/protocol/authorization/
- Cloudflare MCP servers, updated 2026-07-28: https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/
- Stytch Connected Apps, 2025-02-20: https://stytch.com/blog/stytch-connected-apps/
- Okta Cross-App Access ecosystem, 2026-06-23: https://www.okta.com/newsroom/press-releases/okta-announces-cross-app-access-partners/
- Google OAuth overview: https://developers.google.com/identity/protocols/oauth2
- Google OAuth production readiness: https://developers.google.com/identity/protocols/oauth2/production-readiness/overview
- Google restricted-scope verification: https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification
- Gmail server-side authorization: https://developers.google.com/workspace/gmail/api/auth/web-server

M. Decision

Option 1: Composio-only

Fastest deployment, broadest turnkey agent catalog, generous free quota, weakest verified self-host and portability story.

Option 2: Nango-only

Best centralized credential control, auditability, proxy flexibility and portable integration logic; fewer turnkey action assurances and impractical supported self-hosting for this lab.

Option 3: Pipedream-only

Largest mature action/workflow surface and managed OAuth; proprietary runtime and less predictable production pricing.

Option 4: Nango control plane plus narrow internal MCP gateway

Best security and long-term architecture. Requires a small amount of integration work and disciplined policy/logging.

Option 5: Auth0/Cloudflare custom stack

Strongest standards-based custody and deployment control, but recreates the connector-maintenance burden the project is trying to eliminate.

Single recommendation: [established] use Composio for the one-day bake-off, then adopt Nango Cloud behind a small fleet-owned MCP/policy gateway for production if all four provider tests pass. Keep Composio or Pipedream as a catalog fallback, Telethon separate, and the Chrome-extension MCP separate. The main assumption is that provider-token custody, auditability and connection portability matter more than having the maximum possible number of pre-built actions.
