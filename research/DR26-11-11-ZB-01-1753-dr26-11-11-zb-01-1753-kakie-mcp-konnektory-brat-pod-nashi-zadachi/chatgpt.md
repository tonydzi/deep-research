# DR26-11-11-ZB-01-1753 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt.com Deep research
chat: https://приватный чат
dr_id: DR26-11-11-ZB-01-1753

---

# Which MCP Connectors Should a One-Person AI Lab Actually Connect? — 11 November 2026

## Executive decision

The dominant result is that **“there is an MCP for it” is a poor criterion for putting a server in an always-on fleet configuration**. For this fleet, the correct filter is narrower: a permanent connector should be remote Streamable HTTP/SSE or a single deliberately hosted HTTP daemon, solve a repeatedly observed task better than an existing CLI/API, have a tolerable tool-schema footprint, and not create a broad new write-capability or account-ban surface.

That changes several seemingly obvious choices. **GitHub’s official remote MCP is mature, but `gh` should remain the primary path** because the CLI already handles your highest-volume developer workflow and published measurements show that broad MCP tool schemas can cost far more context than a shell-oriented workflow. GitHub’s server is useful selectively—especially read-only/scoped toolsets or clients without good shell access—but is a poor default “always on everywhere” choice. GitHub itself provides toolset- and read-only-specific remote URLs, which is exactly how it should be deployed if used. citeturn567158search1turn490519search2  
Primary sources: https://github.com/github/github-mcp-server and https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md

Conversely, **remote MCP is genuinely better where it removes brittle browser automation or hand-written authentication glue**. The strongest examples for this fleet are Typefully for cross-machine publishing, ORCID/Zenodo plus one consolidated scholarly HTTP service, Hugging Face’s official remote server, OpenRouter’s official server, Calendly, and Cloudflare. For web research, I would add **one** of Jina/Exa/Firecrawl, not all three; for your mix of papers, PDFs and web extraction, Jina is the best first fit. Jina’s remote server exposes URL/PDF reading, web and scholarly search, screenshots, reranking and extraction from one endpoint. citeturn657492search0  
Source: https://github.com/jina-ai/MCP

The other major result is that **several apparently attractive “official MCPs” are irrelevant to the demand you actually measured**. Ashby, Greenhouse and Wellfound MCPs are recruiter/employer-side systems, not improved interfaces for a candidate traversing public job boards. Ashby’s MCP authenticates into an Ashby organization, Greenhouse’s is tied to employer-side Greenhouse permissions, and Wellfound’s MCP documentation describes eligible recruiter accounts. They do not replace the candidate-facing browser path you are using. citeturn942234search0turn942234search2turn942234search3  
Sources: https://docs.ashbyhq.com/ashby-mcp-server-beta , https://support.greenhouse.io/hc/en-us/articles/52096319906971-Set-up-Greenhouse-MCP-with-supported-AI-tools , https://help.wellfound.com/article/1219-connect-wellfound-to-your-ai-assistant

A useful operating rule for the fleet is therefore:

> **Permanent remote MCP:** only when it removes authentication/browser friction or provides high-value structured operations.  
> **Ad-hoc MCP:** broad developer/cloud servers, especially where a CLI is already excellent.  
> **No MCP:** low-frequency sites, unofficial browser-session wrappers, stdio-only servers, and anything whose automation surface materially raises suspension risk.

One cross-client caveat matters. Streamable HTTP itself is no longer the main portability problem: Claude Code, Cursor and modern Codex/Gemini clients increasingly accept remote MCP. **ChatGPT is more restrictive**: OpenAI supports remote MCP servers, but custom-MCP capabilities differ by plan and mode; Deep Research uses read/fetch-oriented access, while broader custom write behavior is available in business-oriented configurations. Do not infer “works with MCP” to mean “identical tool/write capability in every ChatGPT surface.” citeturn854831search0turn854831search2  
Sources: https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt and https://developers.openai.com/api/docs/guides/tools-connectors-mcp

## Connector decision matrix

“Frequency” below is your measured 27 May–11 Nov 2026 demand, not an external estimate. “Maturity” uses repo/release data where I could verify it; **unknown means I did not find a defensible current value rather than extrapolating one**.

| Connector | Cluster | Your frequency | Decision | Why | Transport / auth | Cost | Maturity / latest verified | Primary source | Confidence |
|---|---|---:|---|---|---|---|---|---|---|
| **GitHub official MCP** | GitHub | 2,349 `gh` calls / 305 sessions; github.com 227 sessions | **LATER / ad hoc** | Excellent server, but `gh` already wins on context efficiency and universality; enable only narrow/read-only toolsets when structured MCP is specifically useful | Hosted Streamable HTTP; OAuth/token; scoped/read-only toolset URLs | Available to GitHub users; feature-specific GitHub products may still require paid plans | **34.4k stars** observed; **v2.1.0, 3 Nov 2026** | https://github.com/github/github-mcp-server citeturn567158search0turn490519search0turn490519search1 | **High** |
| **OpenRouter MCP** | LLM vendors / OpenRouter | OpenRouter shell 10; LLM web UIs 222 combined | **TAKE** | Structured live model catalog, pricing, docs, benchmarks and test inference can reduce browser fan-out and stale model metadata | Hosted remote HTTP; OAuth; dedicated API credential | OAuth creates dedicated key; docs state 7-day expiry and configurable $10 spend cap | Official launch; current hosted service | https://openrouter.ai/blog/announcements/openrouter-mcp-server/ citeturn716433search4turn716433search5 | **High** |
| **Browser-driving ChatGPT/Grok/Claude/z.ai/Mistral as “LLM connector”** | LLM vendors | ChatGPT 75; Claude 56; Grok 49; z.ai 34; Mistral 8 | **DON’T formalize further** | Browser automation is brittle and, for at least OpenAI/Mistral, programmatic extraction can conflict with service terms; use official APIs/OpenRouter where economics allow | Browser session automation | Subscription-dependent | N/A | https://openai.com/en-GB/policies/eu-terms-of-use/ ; https://legal.mistral.ai/terms/eu-consumers-terms-of-service/ citeturn591534search1turn591534search6 | **High** for OpenAI/Mistral; **Low/unknown** for exact Grok/z.ai rules |
| **Typefully MCP** | X + multi-social scheduler | X 45; dev.to 27; Bluesky 5; Mastodon 5; LinkedIn 6 | **TAKE** | Best fit if X/thread publishing is the primary target: documented threads, images/media, drafts, scheduling and multi-network posting | Hosted **Streamable HTTP**, OAuth; bearer key supported headlessly | Free tier exists and MCP/API advertised; current pricing page was internally inconsistent on free posting quota; paid numeric price not reliably rendered → **unknown** | Official hosted; help article current in 2026; repo exposes ~26 tools | https://support.typefully.com/en/articles/13128440-typefully-mcp-server citeturn768058search3turn160160search6 | **High** |
| **Buffer MCP** | Multi-social scheduler | same social cluster | **LATER — choose instead of Typefully, not alongside it** | Broader network coverage and queue/analytics; explicitly supports X/Bluesky/Mastodon/Threads threads plus per-thread media. Main downside is broad account-wide OAuth/API reach | Hosted HTTP; OAuth or bearer API key | Free: up to 3 channels, 10 scheduled posts/channel; API allowances shown on pricing page; paid exact current price omitted here because dynamic page did not expose it reliably | Official hosted | https://developers.buffer.com/guides/integrations/mcp citeturn768058search0turn160160search4 | **High** |
| **Official X MCP / xMCP** | X/Twitter | 45 X sessions | **DON’T as permanent write connector; TAKE read-only or ad hoc** | Direct remote route with app-only bearer is read-only. User-context writes require the local `xurl mcp` stdio bridge, violating your permanent-config rule | Hosted `https://api.x.com/mcp`; direct bearer for read-only; **stdio xurl bridge for OAuth user writes** | X API is pay-per-use; verified price examples include Post Read $0.005, User Read $0.010, Post Create $0.015; endpoint rate ceiling not verified | Official; xurl **v1.2.1, 29 Jun 2026** | https://github.com/xdevplatform/docs/blob/main/tools/mcp.mdx citeturn768058search4turn158197search4turn642391search0 | **High** |
| **Meta/Facebook Devtools / Ads MCP** | Facebook/Meta | Facebook 15 | **LATER** | Official Meta MCP work is currently strongest around developer/ads tooling, not a clean general Facebook Page-posting replacement; low measured frequency | Hosted remote reported under `mcp.facebook.com`; OAuth | Unknown | Official project appears active; public metadata sparse | https://github.com/facebook/agentic-tools ; https://apis.io/servers/facebook-business-manager/facebook-business-manager-mcp-yml/ citeturn832933search6turn832933search7 | **Medium** |
| **Community Facebook Pages MCPs** | Facebook/Meta | 15 | **DON’T** | Unofficial write-capable social connector adds a large credential/account surface for a task already covered better by Buffer/Typefully | Community/local; varies | Free OSS; API costs/account limits external | Fragmented | https://github.com/IvanBBaev/facebook-mcp citeturn832933search8 | **Medium** |
| **cyanheads Bluesky MCP** | Bluesky | 5 | **LATER** | Mature enough for structured Bluesky reads, with hosted/self-host HTTP, but Typefully already covers publishing and direct ATProto is simple | Hosted remote + self-host HTTP; app-password needed for some authenticated ops | OSS; hosting free/unknown | Community, actively maintained in 2026 | https://github.com/cyanheads/bluesky-mcp-server citeturn342633search0 | **Medium-high** |
| **Vitex Mastodon MCP** | Mastodon | 5 | **LATER** | Read-only default and HTTP transport are good, but demand is tiny and Typefully can handle posting | Streamable HTTP + stdio; Mastodon access token | OSS | Community | https://github.com/VitexSoftware/mcp-server-mastodon citeturn342633search5 | **Medium** |
| **Community Reddit MCP** | Reddit | 6 | **DON’T permanent** | Popular implementation located is stdio-oriented and unofficial; six sessions do not justify permanent auth/schema exposure | Primarily stdio | OSS | ~184 stars reported; pushed Jun 2026 | https://devhunt.org/mcp/for/reddit citeturn342633search6 | **Medium** |
| **dev.to remote read MCP** | dev.to | 27 | **LATER** | Public read access can help research, but direct DEV API/browser is already simple; publishing value is weak in the remote read implementation found | Remote HTTP; public/read-only | Free | Community | https://dev.to/nickytonline/introducing-the-devto-mcp-server-42jg citeturn342633search10 | **Medium** |
| **content-distribution-mcp** | dev.to / Hashnode / Reddit / Bluesky | 27 / low | **DON’T permanent** | Attractive aggregation, but the implementation found is stdio-only—direct violation of your hard rule | **stdio only** | OSS | ~5 stars; last push Jun 2026 reported | https://github.com/AutomateLab-tech/content-distribution-mcp citeturn342633search7turn342633search8 | **High** |
| **HackerNoon / Hashnode MCP** | publishing | HackerNoon 6; Hashnode unmeasured | **DON’T / no mature remote found** | No sufficiently mature official remote connector surfaced; direct API/browser is proportionate to usage | Unknown | Unknown | **Prior art not found** in official registry, GitHub, Smithery/Glama/web search | https://registry.modelcontextprotocol.io citeturn567158search5 | **Medium** |
| **LinkedIn public/general automation MCP** | LinkedIn | 6; account already banned | **DON’T** | LinkedIn explicitly restricts unauthorized bots/scraping/automation; no public general-purpose first-party posting MCP was verified. Do not add more account-risk | No recommended connector | N/A | A limited “My Profile” MCP has been reported for select partners, but not a public publishing solution | https://www.linkedin.com/help/linkedin/answer/a1340567/automated-activity-on-linkedin?lang=en ; https://developer.linkedin.com/legal citeturn832933search0turn832933search1 | **High** on prohibition; **Medium** on current limited MCP availability |
| **scholarly-mcp** | arXiv/Crossref/DataCite/OpenAlex/S2 | arXiv25 + Crossref7 + DataCite8 + OpenAlex6 + S2 6 + DOI22 | **TAKE** | One self-hosted HTTP process consolidates multiple scholarly APIs and obeys your “one daemon” rule; dramatically better than a half-dozen stdio servers | Self-host HTTP | OSS; upstream APIs largely free/public | Community; 41/41 API verification tests reported Sep 2026 | https://github.com/M-Romdhani/scholarly-mcp citeturn255102search6 | **Medium-high** |
| **cyanheads ORCID MCP** | ORCID | **58 sessions** | **TAKE** | Highest scholarly site demand; public-author record retrieval without browser interaction; hosted remote and self-host HTTP | Hosted remote + self-host HTTP; public reads keyless | OSS | Active community | https://github.com/cyanheads/orcid-mcp-server citeturn255102search2 | **High** |
| **cyanheads Zenodo MCP** | Zenodo | **21** | **TAKE** | Structured records/depositions reduce browser friction; remote HTTP and no per-session process | Hosted remote Streamable HTTP | OSS | Community; current 2026 implementation | https://github.com/cyanheads/zenodo-mcp-server citeturn255102search4 | **High** |
| **cyanheads Crossref MCP** | Crossref / DOI | Crossref7 + DOI22 | **LATER if scholarly-mcp deployed** | Good hosted remote, but redundant if consolidated scholarly daemon is always on | Hosted `https://crossref.caseyjhand.com/mcp` | Public API / OSS | **v0.5.1** verified | https://crossref.caseyjhand.com/ citeturn255102search9 | **High** |
| **cyanheads DataCite MCP** | DataCite | 8 | **LATER** | Good structured DOI metadata, but duplicated by scholarly-mcp | Self-host HTTP; public API; optional contact identity | OSS | Active community | https://github.com/cyanheads/datacite-mcp-server citeturn255102search5 | **Medium-high** |
| **Semantic Scholar MCP** | Semantic Scholar | 6 | **LATER** | Useful paper/citation graph, but scholarly-mcp already aggregates it; hosted “official” endpoint status was not confirmed from a primary S2 source | Community HTTP; optional S2 key | OSS / API plan dependent | Community | https://github.com/smaniches/semantic-scholar-mcp citeturn255102search7 | **Medium** |
| **blazickjp arXiv MCP** | arXiv | 25 | **DON’T permanent** | Popular and mature, but stdio-only in the implementation found; consolidated HTTP alternative exists | **stdio** | OSS | ~**3.2k stars**, pushed Oct 2026 | https://devhunt.org/mcp/server/arxiv citeturn255102search1 | **High** |
| **Ashby official MCP** | Jobs/ATS | Ashby 16 | **DON’T for job hunting** | Employer/recruiter-side Ashby organization tooling, not a candidate-facing job-search connector | Hosted remote, OAuth | Available across Ashby plans during beta | Official beta announced 29 Jun 2026 | https://docs.ashbyhq.com/ashby-mcp-server-beta citeturn942234search0turn942234search1 | **High** |
| **Greenhouse official MCP** | Jobs/ATS | Greenhouse 10 | **DON’T for job hunting** | Authenticates into employer Greenhouse Recruiting; does not replace public candidate browsing | Remote HTTP, OAuth PKCE/DCR; permission-scoped | Greenhouse Core/Plus/Pro | Official 2026 | https://support.greenhouse.io/hc/en-us/articles/52096319906971-Set-up-Greenhouse-MCP-with-supported-AI-tools citeturn942234search2turn942234search8 | **High** |
| **Lever MCP** | Jobs/ATS | Lever 6 | **DON’T** | No official candidate-oriented MCP found; available community wrappers add little over public postings/browser | Community/varies | Unknown | No verified official MCP | https://beatview.ai/guides/lever-mcp citeturn942234search5 | **Medium** |
| **Wellfound official MCP** | Jobs | Wellfound 5 | **DON’T for your use** | MCP is documented for **eligible recruiter accounts**, not candidate job discovery | Hosted `https://wellfound.com/api/mcp`, OAuth | Recruiter-account dependent | Official | https://help.wellfound.com/article/1219-connect-wellfound-to-your-ai-assistant citeturn942234search3 | **High** |
| **YC / Work at a Startup** | Jobs | YC 9 | **DON’T / none verified** | No mature official candidate-facing MCP found; browser remains proportionate | Unknown | Unknown | Prior art not found | https://registry.modelcontextprotocol.io citeturn567158search5 | **Medium** |
| **Hugging Face official MCP** | Hugging Face | **18** | **TAKE** | Native Hub search, docs, Jobs/Sandboxes/community tools; explicitly documented across Codex/Cursor/Gemini/Claude and avoids custom HF shell glue | Hosted `https://huggingface.co/mcp`, OAuth or Bearer token | MCP surcharge not separately stated; compute/actions follow HF services | Official, active | https://github.com/huggingface/hf-mcp-server citeturn304315search0turn304315search1 | **High** |
| **HF Inference Endpoints MCP** | Hugging Face infra | within HF18 | **LATER** | Valuable only if you actually manage HF inference endpoints; includes destructive create/update/delete operations | Official MCP | Underlying endpoint compute billed normally; MCP fee unknown | Official | https://huggingface.co/docs/inference-endpoints/guides/mcp_server citeturn304315search2 | **High** |
| **Cloudflare official MCP** | Cloud/infra | 3 | **TAKE** | Low frequency but unusually clean official remote/OAuth path; Cloudflare’s code-mode design reduces schema bloat and can replace manual dashboard/API work safely | Hosted `https://mcp.cloudflare.com/mcp`, Streamable HTTP + OAuth | No separate MCP fee verified | Official; current | https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/ citeturn304315search4turn304315search5 | **High** |
| **mcp-hetzner** | Hetzner | 7 combined | **DON’T permanent / ad hoc only** | ~137 cloud/DNS/storage tools and broad credentials are too much persistent attack surface for seven sessions; server itself lacks auth unless you add it | Self-host Streamable HTTP + stdio; must wrap HTTP with TLS/auth | OSS; Hetzner usage billed normally | Community | https://github.com/ginkelsoft-development/mcp-hetzner citeturn656722search0 | **High** |
| **tailscale-mcp** | Tailscale | 6 | **DON’T permanent** | ~186 tools and stdio-oriented deployment; read-only default helps but shell/API is cheaper for six sessions | Primarily stdio; control-plane API | OSS | npm **2.0.5**, published ~16 days before crawl | https://github.com/tailscale-mcp/tailscale-mcp citeturn304315search7turn304315search8 | **High** |
| **Tailscale Aperture** | Tailscale/MCP security | 6 | **LATER as gateway, not management MCP** | Interesting for policy/proxying MCP servers, but it is not a Tailscale fleet-management connector | Remote proxy/gateway | Tailscale-plan dependent | Official | https://tailscale.com/docs/aperture/mcp-server citeturn304315search9 | **High** |
| **Docker MCP Gateway / Toolkit** | Docker | 19 | **DON’T as Docker-management replacement** | Docker CLI already gives compact, composable control. MCP Gateway is more useful as MCP infrastructure than as a reason to replace `docker` CLI | Gateway/container orchestration | Docker Desktop / governance plan dependent; governance service currently limited availability | Official | https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/ citeturn304315search6 | **High** |
| **Calendly official MCP** | Scheduling | **13** | **TAKE** | Adds availability, scheduling links, event creation/cancellation—semantics Google Calendar alone does not provide | Hosted `https://mcp.calendly.com`, OAuth 2.1 PKCE/DCR | Exact MCP surcharge not stated; Calendly account required | Official 2026 | https://developer.calendly.com/docs/mcp/calendly-mcp-server citeturn656722search3turn656722search4 | **High** |
| **Luma official MCP** | Events | 5 | **TAKE, lower priority** | Clean official remote server for discovering/managing events; low security burden relative to browser automation | Hosted `https://mcp.luma.com`, Streamable HTTP, OAuth/CIMD | Docs say available with a Luma account; no Plus required | Official 2026 | https://help.luma.com/p/mcp citeturn656722search5 | **High** |
| **Partiful community MCP** | Events | 3 | **DON’T** | No official public API/MCP was found; community solution depends on a browser session/cookie, which is exactly the fragile credential pattern to avoid | Community HTTP/stdio/browser session | OSS/commercial variants | Community only | https://github.com/cotletdev/partiful-api-mcp-server citeturn656722search6 | **High** |
| **Airtable official MCP** | Airtable | 3 | **LATER** | Excellent official remote connector, but three sessions do not justify permanent tool exposure yet | Hosted `https://mcp.airtable.com/mcp`, OAuth | **No additional MCP cost; all Airtable plans** | Help updated **9 Nov 2026** | https://support.airtable.com/articles/9897799762-using-the-airtable-mcp-server citeturn353466search0turn353466search1 | **High** |
| **Notion remote MCP** | Notion-type | no measured Notion demand | **DON’T now / LATER if demand appears** | Official remote is the correct implementation, but you have no observed demand; old self-host repo is explicitly no longer actively maintained | Official remote OAuth | Plan-dependent; MCP surcharge unknown | Old self-host repo deprecated | https://www.notion.com/help/notion-mcp ; https://github.com/makenotion/notion-mcp-server citeturn353466search3turn353466search4 | **High** |
| **Jina MCP** | Web fetch/search | Jina 5; broader research use high | **TAKE** | Best single fit for your workload: URL/PDF reading, web + arXiv/SSRN search, screenshots, extraction, reranking and dedupe | Hosted `https://mcp.jina.ai/v1`; API key optional/required by tool | Free/paid API quotas apply; exact 11 Nov 2026 quota not verified → unknown | Official, active | https://github.com/jina-ai/MCP citeturn657492search0 | **High** |
| **Exa MCP** | Web search | research cluster | **LATER** | Excellent search/research server, but duplicates Jina if both are permanently exposed | Hosted `https://mcp.exa.ai/mcp` | Exa API pricing/credits apply; current exact free allowance not verified here | **v3.4.1, 23 Sep 2026** reported | https://github.com/exa-labs/exa-mcp-server citeturn353466search5turn353466search7turn353466search8 | **High** |
| **Firecrawl MCP** | Web scrape/crawl | research cluster | **LATER** | Prefer if crawling/site-map extraction becomes more frequent than scholarly/PDF work; keyless hosted mode is intentionally limited | Hosted `https://mcp.firecrawl.dev/v2/mcp`, OAuth/API key | Limited keyless tools; full crawl/map/agent uses Firecrawl plan | Official | https://github.com/firecrawl/firecrawl-mcp-server citeturn353466search6 | **High** |
| **Tavily MCP** | Web search/crawl | research cluster | **LATER** | Solid hosted search/extract/map/crawl, but another permanent search MCP adds little once Jina is present | Hosted `https://mcp.tavily.com/mcp`; OAuth/key | Current exact quota not verified | Official | https://github.com/tavily-ai/tavily-mcp citeturn657492search2 | **High** |
| **Brave Search MCP** | Web search | research cluster | **DON’T permanent** | Official implementation can self-host HTTP, but documented HTTP mode is unauthenticated by default and hosted vendor endpoint was not verified; Jina/Exa/Tavily are cleaner | stdio or self-host HTTP | Brave API plan | Official OSS | https://github.com/brave/brave-search-mcp-server citeturn657492search3turn657492search4 | **High** |
| **Hootsuite MCP** | Social scheduler | social cluster | **DON’T** | Duplicates Typefully/Buffer, requires Hootsuite entitlements, and its connector chat has network-specific data restrictions | Official hosted remote/OAuth; Perch/Nest/Lumen/Parliament servers | Paid Hootsuite entitlement; exact current price not verified | Official | https://www.hootsuite.com/integrations/mcp citeturn359035search0turn359035search1 | **High** |
| **Publer MCP** | Social scheduler | social cluster | **DON’T** | Competent, scoped API-key option, but adds a fourth scheduler with no demonstrated advantage for your existing accounts | Hosted `https://mcp.publer.com`; OAuth/API key | Business/Enterprise access documented | Official | https://publer.com/help/en/article/14orlq0/ citeturn359035search2 | **High** |
| **Ayrshare MCP** | Social scheduler/API | social cluster | **LATER** | More API-centric and useful if social automation becomes a product/workflow, but redundant for one operator today | Remote HTTP | Current price not verified | Official | https://www.ayrshare.com/integrations/ ; https://www.ayrshare.com/docs/mcp citeturn359035search5turn359035search6 | **Medium-high** |
| **Microsoft Playwright MCP** | Browser automation | Chrome control 15.3k calls / 311 sessions | **DON’T permanent** | You already have a heavily used browser-control path. Microsoft itself now recommends CLI + skills for many coding-agent scenarios because of token efficiency; Azure remote MCP remains preview | Local MCP package; Azure Playwright Workspaces offers remote Streamable HTTP preview | Azure usage; preview/no production SLA | Official | https://github.com/microsoft/playwright-mcp/blob/main/README.md ; https://learn.microsoft.com/en-us/azure/app-testing/playwright-cloud-browsers/how-to-playwright-workspaces-remote-mcp citeturn657492search6turn657492search7 | **High** |
| **Browserbase MCP** | Browser automation | same 311-session browser cluster | **LATER** | Useful for unattended cloud browsers or disposable sessions, but does not replace the value of your already-authenticated local browser profile | Hosted remote + package supports HTTP/stdio | Browserbase + Stagehand/model costs | Official | https://www.browserbase.com/mcp citeturn359035search7turn359035search8 | **High** |
| **Slack official MCP** | Messaging | no Slack demand supplied | **LATER** | Technically strong official server, but no measured demand. If Slack enters your job/network workflow, it becomes a good candidate immediately | Hosted `https://mcp.slack.com/mcp`, Streamable HTTP/OAuth; registered Slack app required | Workspace/plan dependent | Official | https://docs.slack.dev/ai/slack-mcp-server/ citeturn716433search0turn716433search1 | **High** |
| **Discord community MCP** | Messaging | no measured demand | **DON’T** | Community server found exposes ~222 tools and no compelling hosted first-party path; enormous schema/permission surface for zero observed usage | Local/community, deployment varies | OSS | Pre-1.0/community | https://github.com/cappyeo/discord-mcp citeturn716433search8 | **Medium** |
| **Signal community MCP** | Messaging | no measured demand | **DON’T** | Requires linked Signal device / signal-cli and gives an agent access to highly sensitive messaging; no demand justifies the risk | Self-host HTTP possible; community | OSS | Community | https://github.com/retog/signal-mcp citeturn716433search7turn716433search9 | **High** |

A pattern in the table is worth making explicit: **maturity is necessary but not sufficient**. GitHub’s MCP is probably the most mature server in the entire set and still does not clear the “replace an excellent CLI” threshold for your default fleet. Conversely, ORCID is a relatively modest community project but solves a real 58-session browser-access pattern with little credential risk because much ORCID data is public.

## Top ten to connect now

The ranking below uses observed frequency, ability to eliminate browser/manual glue, transport compliance, write-risk, schema duplication and cross-machine utility. “Connect now” means **worth entering the permanent remote/HTTP configuration**; it does not mean every server should receive unconditional write approval.

| Rank | Connector | Expected value for this fleet | Recommended permissions | Prior art |
|---:|---|---|---|---|
| **1** | **Typefully MCP** | Converts X/thread/media publishing from browser activity into one hosted, OAuth-backed remote surface and also covers Bluesky/Mastodon/Threads. Its documented MCP explicitly supports create/edit/schedule, threads and media. citeturn768058search3 | OAuth; human approval for publish/delete; do **not** expose unattended “publish now” to content fetched from the web | **Prior art not found**: I found vendor docs/plugin repo and directory entries, but no independent MCP-vs-browser reliability or ban-rate benchmark in GitHub/official Registry/Smithery/Glama/web search |
| **2** | **scholarly-mcp** | One HTTP daemon replaces separate arXiv/Crossref/DataCite/OpenAlex/Semantic Scholar integrations and directly matches your research/publishing pattern. citeturn255102search6 | Public/read-only API calls where possible; no unnecessary API keys | **Partial prior art**: project reports 41/41 API verification; no independent latency/token benchmark found |
| **3** | **ORCID MCP** | ORCID alone has 58 measured sessions, the highest individual scholarly site. Hosted/self-host HTTP removes a conspicuous manual browsing pattern. citeturn255102search2 | Public read-only initially; do not give profile-write scope unless an actual write workflow appears | **Prior art not found** after GitHub, Registry and directory search |
| **4** | **Zenodo MCP** | 21 Zenodo sessions plus DOI/paper work make structured record/deposition access valuable, and hosted HTTP fits your process rule. citeturn255102search4 | Read by default; require approval for deposit/update/publish operations | **Partial prior art exists**: a separate Zenodo MCP technical report is archived on Zenodo, but I found no head-to-head benchmark of this implementation: https://zenodo.org/records/20023539 citeturn255102search3 |
| **5** | **Hugging Face MCP** | Official remote server, 18 observed sessions, and unusually good cross-client support. Removes bespoke Hub search/metadata/job/sandbox shell calls. citeturn304315search0turn304315search1 | Start with Hub/search/docs; add compute/destructive endpoint tools only when needed | **Prior art not found** for MCP-vs-`huggingface-cli` efficiency; official documentation and client examples exist |
| **6** | **Jina MCP** | A single remote endpoint covers the research primitives you repeatedly need: page/PDF extraction, scholarly search, screenshots, ranking and deduplication. This is more coherent than permanently installing Exa+Firecrawl+Tavily simultaneously. citeturn657492search0 | Treat returned web text as untrusted; never permit downstream writes merely because fetched content instructs the agent to do so | **Prior art not found** for a controlled Jina-vs-Exa/Firecrawl MCP token/cost study; searched vendor docs, GitHub and directories |
| **7** | **OpenRouter MCP** | Particularly useful for your multi-model research fan-out: current model catalog/pricing/benchmark metadata is exactly the kind of volatile information that should not live in prompts. citeturn716433search4 | Keep the MCP-created key spend cap low; inference call approval above a cost threshold | **Prior art not found** for the OpenRouter MCP itself; general MCP-vs-CLI measurements exist but are not OpenRouter-specific |
| **8** | **Calendly MCP** | Calendar alone does not model Calendly’s availability rules, event types, scheduling links and cancellations. Thirteen measured sessions clear the threshold. citeturn656722search3 | Read availability freely; require confirmation for event creation/cancellation | **Prior art not found**; searched official Registry, vendor docs and GitHub |
| **9** | **Cloudflare MCP** | Only three sessions, but this is a rare infra MCP whose official “code mode” is deliberately optimized to avoid exposing a giant native tool list. Cloudflare reports about **1,100 tokens** for its three code-mode tools, making permanent exposure unusually cheap. citeturn304315search5 | OAuth minimum scopes; confirmation for DNS/routing/security writes | **Vendor prior art exists**: Cloudflare explicitly compares its compact code-mode surface against large native tool schemas in its repo/docs |
| **10** | **Luma MCP** | Five sessions is marginal, but it is official, remote, OAuth-backed and low operational burden; event workflows benefit from structured lookup/management more than another browser wrapper. citeturn656722search5 | Read/search by default; explicit approval for event mutations | **Prior art not found** after vendor/GitHub/directory search |

**Buffer is the first alternate, not an eleventh simultaneous social connector.** If your social workflow is primarily queue/analytics and broad network coverage rather than X-centric thread composition, substitute Buffer for Typefully. Running both permanently gives an agent two different ways to create the same public side effect and increases the chance of duplicate posting or stale queue edits. Buffer documents immediate publishing, editing and irreversible deletion, and explicitly advises retaining approval prompts. citeturn768058search0turn768058search1

Likewise, the academic recommendation intentionally combines **one aggregator plus two exceptions**. `scholarly-mcp` covers arXiv, Crossref, DataCite, OpenAlex and Semantic Scholar; ORCID and Zenodo are sufficiently high-demand and not equivalently covered that their dedicated remote endpoints remain justified. citeturn255102search2turn255102search4turn255102search6

## Cluster findings, compatibility and security

### Developer and LLM connectors

The official GitHub MCP is a model of how a mature server should look: remote hosting, targeted toolsets, read-only modes, active releases, official docs and broad client compatibility. Its weakness is not quality but **economics relative to `gh`**. GitHub exposes default toolsets including context, repositories, issues, pull requests and users, but lets you narrow the remote URL to particular toolsets and `/readonly` forms. That makes a small, read-only MCP useful for a research agent that needs structured issue/PR retrieval while preserving `gh` for everything else. citeturn490519search2turn567158search1  
Sources: https://github.com/github/github-mcp-server/blob/main/README.md and https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md

OpenRouter is almost the opposite situation. Your alternative is currently shell/browser fan-out into a rapidly changing model marketplace. The remote MCP can supply current model IDs, pricing and capabilities without embedding them in prompts or scraping a web UI. Its OAuth flow creates a dedicated API key with an expiry and spend cap, which is materially safer than distributing your normal OpenRouter API key to eight machines. citeturn716433search4  
Source: https://openrouter.ai/blog/announcements/openrouter-mcp-server/

I would **not** add separate MCP wrappers for ChatGPT.com, Claude.ai, Grok.com, z.ai or Mistral’s consumer UI. The principal value in those sites today is subscription economics and model diversity, not a clean automation contract. OpenAI’s current consumer terms prohibit automatically/programmatically extracting data or output except where permitted, and Mistral’s EEA consumer terms similarly restrict extraction methods outside permitted interfaces. citeturn591534search1turn591534search6  
Sources: https://openai.com/en-GB/policies/eu-terms-of-use/ and https://legal.mistral.ai/terms/eu-consumers-terms-of-service/  
I did **not** verify equivalent November-2026 language for every Claude, Grok and z.ai consumer surface, so their exact enforcement risk is **unknown**, not assumed equivalent.

### Social networks

For Bluesky and Mastodon there are respectable community MCPs, but protocol-native APIs are already relatively agent-friendly. A permanent MCP therefore has to beat the incremental cost of simply letting Typefully handle outbound content while using browser/direct API for the occasional read. At five sessions each, neither standalone connector clears that bar today. The Bluesky implementation I found is particularly good architecturally—hosted remote plus self-host HTTP and read-focused tools—but low demand is still low demand. citeturn342633search0  
Source: https://github.com/cyanheads/bluesky-mcp-server

Reddit is less attractive. I did not find a first-party Reddit remote MCP comparable to GitHub/Cloudflare/Calendly; the community option surfaced by current directories is stdio-oriented. Six sessions plus a history of API/policy changes is not enough to make it permanent. citeturn342633search6  
Source: https://devhunt.org/mcp/for/reddit

LinkedIn is a much stronger **no**. LinkedIn states that third-party software or browser extensions that scrape or automate activity can cause restrictions, and its API terms constrain automated use. Given that the account is already banned, there is no rational expected-value argument for unofficial LinkedIn MCP automation. citeturn832933search0turn832933search1turn832933search3  
Sources: https://www.linkedin.com/help/linkedin/answer/a1340567/automated-activity-on-linkedin?lang=en , https://developer.linkedin.com/legal , https://ac.linkedin.com/legal/user-agreement

There are November-2026 reports of a limited LinkedIn “My Profile MCP” at `https://mcp.linkedin.com/mcp-my-profile` for selected integrations, but I did not find a first-party LinkedIn page documenting a generally available feed/posting MCP. I therefore treat that as **medium-confidence, limited-availability information**, not as a recommendation. citeturn832933search10

For Facebook, the official Meta MCP direction appears focused on developer/ads workflows rather than a clean “manage my personal/Page social feed” connector. At fifteen sessions, Buffer or Typefully is the lower-risk abstraction for supported outbound posting, while direct Graph API use remains preferable if you later build a serious Page workflow. The community Page MCPs add credential and maintenance risk without sufficient measured benefit. citeturn832933search6turn832933search8

### Scholarly, jobs and infrastructure

The scholarly cluster is where MCP is especially well matched to the work. Most source APIs are structured and relatively low-risk, so the agent is not ingesting arbitrary private inboxes or obtaining irreversible cloud-admin powers. The consolidated `scholarly-mcp` supports OpenAlex, Crossref, Semantic Scholar, arXiv, Unpaywall, DataCite, Europe PMC and OpenCitations over HTTP; it reported 41/41 upstream verification tests in September 2026. citeturn255102search6  
Source: https://github.com/M-Romdhani/scholarly-mcp

That also makes several popular single-service servers unnecessary. The arXiv server at `blazickjp/arxiv-mcp-server` is widely adopted—roughly 3.2k stars in the sources I found—but its permanent deployment model is stdio, so it loses to the consolidated HTTP daemon under your fleet rule. citeturn255102search0turn255102search1

The job-board result is counterintuitive but decisive: **the official ATS MCP boom is mostly useless to a job seeker**. Ashby’s server lets authenticated Ashby customers work with recruiting data and enforces Ashby permissions and rate limits; Greenhouse similarly connects into the customer’s Greenhouse Recruiting tenant; Wellfound’s docs frame its MCP around recruiter accounts and candidate/application management. citeturn942234search0turn942234search2turn942234search3  
For your use case, browser automation plus direct access to public job pages remains the correct path. Building an “ATS MCP collection” would increase tool count while solving the wrong side of the marketplace.

Infrastructure should be held to a much higher security threshold. A community Hetzner server exposing roughly 137 cloud/DNS/storage operations with an HCloud credential is a dangerous permanent capability, particularly because the implementation itself does not provide the authentication boundary you would want on a public HTTP endpoint. citeturn656722search0  
Source: https://github.com/ginkelsoft-development/mcp-hetzner

Tailscale is similar: the community server exposes roughly 186 tools and uses an API key plus node control capabilities. It defaults toward read-only behavior, which is good design, but six sessions do not justify adding that schema and token scope to every agent. citeturn304315search7  
Source: https://github.com/tailscale-mcp/tailscale-mcp

Cloudflare is the exception because the vendor has actively designed around MCP schema bloat. Its current server is hosted, OAuth-based and uses a small code-mode tool surface rather than hundreds of endpoint-shaped tools. The official repository reports roughly 1,100 tool-definition tokens for its three code-mode tools. citeturn304315search4turn304315search5  
Sources: https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/ and https://github.com/cloudflare/mcp/blob/main/README.md

Docker should remain CLI-first. Docker’s MCP Gateway is interesting **as an MCP orchestration architecture**, not as evidence that `docker` commands should become tool calls. The Gateway can centralize/containerize MCP services and is worth revisiting if you eventually want a policy-enforced broker in front of many servers, but it does not improve a 19-call Docker-management workload enough to justify replacing the CLI. citeturn304315search6  
Source: https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/

### Browser, messaging and prompt injection

Your browser data strongly argues against another permanent browser server. Microsoft’s Playwright MCP is official and capable, but the project itself now recommends Playwright CLI plus agent skills for many coding-agent workflows because they are more token-efficient than exposing the browser tool surface continuously. Microsoft’s managed Playwright Workspaces remote MCP exists, but is still preview and has no production SLA. citeturn657492search6turn657492search7  
Sources: https://github.com/microsoft/playwright-mcp/blob/main/README.md and https://learn.microsoft.com/en-us/azure/app-testing/playwright-cloud-browsers/how-to-playwright-workspaces-remote-mcp

Browserbase is the reasonable second browser if you need **unattended cloud execution**, disposable profiles, IP isolation or server-side browser sessions. It is not a substitute for Claude-in-Chrome when the value is “act inside the already-authenticated human browser with all my local sessions.” citeturn359035search7turn359035search8  
Sources: https://www.browserbase.com/mcp and https://www.npmjs.com/package/@browserbasehq/mcp

Slack’s server is technically one of the strongest official messaging MCPs. It uses Streamable HTTP, respects Slack auth/scopes, and documents Claude, Cursor, Codex and Gemini usage. But Slack requires the MCP client to be backed by an approved/registered Slack app; it does not simply offer unconstrained dynamic client registration. citeturn716433search0turn716433search1  
Sources: https://docs.slack.dev/ and https://docs.slack.dev/ai/slack-mcp-server/

The important security distinction across all of these connectors is **prompt-injection reach**, not merely credential secrecy. A search/PDF server can ingest arbitrary attacker-authored text; a Slack/Gmail/social connector can ingest attacker text **and** also possess write tools. The dangerous composition is therefore:

`untrusted content → agent reasoning → broad write-capable connector`

For permanent servers, I would enforce four operational constraints: keep read and write credentials separate where a service permits it; use tool-specific/read-only endpoints such as GitHub’s `/readonly` variants; retain human confirmation for public posts, deletions, DNS/cloud mutations and calendar cancellations; and never let instructions extracted from web pages/messages authorize a different connector’s side effect. GitHub explicitly exposes read-only toolsets, Buffer warns that its account credential reaches every organization/channel and that delete/publish are real side effects, and Greenhouse documents permission-scoped OAuth. citeturn567158search1turn768058search1turn942234search8

## X publishing, connector catalogs, and GitHub versus `gh`

### Buffer vs Typefully vs official X for posting from any machine

For your exact requirement—**“post to X from any one of ~8 machines without a local per-session MCP process”**—the practical answer is **Typefully or Buffer, not the official X MCP**.

| Capability | Typefully MCP | Buffer MCP | Official X MCP / xurl |
|---|---|---|---|
| Permanent remote endpoint | **Yes** — `https://mcp.typefully.com/mcp` | **Yes** — `https://mcp.buffer.com/mcp` | **Read-only yes** direct; **user-context writes require local `xurl mcp` stdio bridge** |
| OAuth | Yes; bearer key also supported headlessly | OAuth; bearer/API-key route documented | OAuth2 user context handled by `xurl` bridge |
| Ordinary X posting | **Explicitly documented** | **Explicitly documented** | X REST API and `xurl` explicitly support create-post; the high-level X MCP documentation I opened does **not** clearly enumerate ordinary post creation among its headline MCP tools |
| Threads | **Yes** | **Yes**, `metadata.twitter.thread` | Can be built through X API reply chains; not documented as a convenient MCP thread primitive in the page reviewed |
| Images/media | **Yes** | **Yes**, media assets per post/thread item | X API/xurl supports media upload; xMCP tool allowlist includes media operations |
| Other networks | LinkedIn, Threads, Bluesky, Mastodon | Facebook, Instagram, LinkedIn, Pinterest, YouTube, TikTok, Mastodon, Threads, Bluesky, etc. | X only |
| Free use | Free tier advertised; current page gave conflicting free-post figures, so exact quota **unknown** | Free: up to 3 channels and 10 scheduled posts/channel | API pay-per-use |
| Verified X API unit price | N/A—bundled through service | N/A—bundled through service | Post Create **$0.015/request**, Post Read **$0.005/resource**, User Read **$0.010/resource** in current X pricing material |
| Exact MCP rate limit | Unknown | MCP propagates API rate-limit/429 behavior; exact current ceiling not verified | Endpoint-specific exact current write ceiling **unknown** |
| Fleet fit | **Best for X/thread-first publishing** | **Best for multi-network queue + analytics** | **Fails permanent-write rule** because write auth is bridged over stdio |
| Ban/ToS risk | Uses Typefully’s supported publishing integration; Typefully explicitly reminds users to obey X automation rules | Uses Buffer’s supported publishing integration | First-party API is contractually the cleanest X path, provided automation policies are obeyed |
| Recommended | **TAKE** | **Alternative to Typefully** | **Read-only permanent; writes ad hoc via xurl/REST** |

Typefully evidence: https://support.typefully.com/en/articles/13128440-typefully-mcp-server and https://support.typefully.com/en/articles/8718287-typefully-api citeturn768058search3turn642391search4  
Buffer evidence: https://developers.buffer.com/guides/integrations/mcp and https://developers.buffer.com/examples/create-threaded-post.html citeturn768058search0turn768058search2  
X evidence: https://github.com/xdevplatform/docs/blob/main/tools/mcp.mdx and https://github.com/xdevplatform/docs/blob/main/tools/xurl.mdx citeturn768058search4turn768058search5

The most important X detail is authentication. X’s hosted MCP server is genuinely remote at `https://api.x.com/mcp`, but the **simple direct bearer mode is app-only and read-only**. X’s own documentation says that writes and user-context tools require `xurl mcp`, which runs a local bridge, handles OAuth/refresh and proxies stdio to the remote MCP. citeturn768058search4 That makes it a good ad-hoc tool but fails your permanent configuration constraint by design.

For ban risk, I would not claim that Buffer has a demonstrably lower suspension rate than Typefully, because I found **no comparative suspension-rate data**. The hierarchy I can defend is narrower: first-party X API is the cleanest contractual interface; Buffer and Typefully use intended social-publishing integrations rather than browser scraping; unofficial browser automation is the least defensible. Typefully itself explicitly warns that automation still has to comply with X rules. citeturn642391search5  
Source: https://typefully.com/docs/api

For your actual workflow, I would pick **Typefully** if thread composition/X is the center of gravity, or **Buffer** if one queue across Facebook/X/Bluesky/Mastodon and analytics matters more. Do not OAuth both until you have a specific reason to maintain two independent public-posting state machines.

### Current MCP catalogs and how to separate maintained servers from abandonware

The catalog ecosystem in November 2026 has stratified into several layers rather than one authoritative “app store.”

| Catalog | What it is now | What to trust |
|---|---|---|
| **Claude Connectors Directory** — https://claude.ai/directory | Anthropic-curated connector discovery for Claude products; includes remote connectors and desktop-extension style integrations | Strong signal for Claude compatibility and curation, **not** proof that the same auth flow works identically in Codex/ChatGPT |
| **Official MCP Registry** — https://registry.modelcontextprotocol.io | Canonical metadata registry, still described as preview; standardized `server.json`; backed by MCP ecosystem organizations | Best namespace/discovery baseline, but **publisher namespace verification does not prove control of the remote endpoint URL** |
| **OpenAI / ChatGPT connectors** | OpenAI supports remote MCP/custom connectors with plan/mode restrictions; Deep Research is more read-oriented | Good for ChatGPT compatibility, but check plan and whether writes are exposed |
| **Cursor MCP / Plugins Marketplace** | Cursor supports stdio, SSE and Streamable HTTP plus OAuth; plugins/marketplace provide discovery/install flows | Strong client support signal; verify underlying upstream repo/vendor |
| **Smithery** — https://smithery.ai | As of **31 Oct 2026**, Smithery shut down its proxy/managed identity/server execution products and remains essentially a registry/discovery layer powered with Arcade integrations | Do not rely on old `server.smithery.ai` proxy instructions found in stale blog posts |
| **mcp.so** — https://mcp.so | Large discovery/index site with hosted filters and setup guides | Discovery only; verify source repo, vendor ownership and transport yourself |
| **Glama** — https://glama.ai | Very large indexed MCP/catalog ecosystem with API and hosting-related functionality | Useful for breadth/security metadata; raw listing count is not a quality signal |

Anthropic describes its directory as the place to browse and connect supported services across Claude products. citeturn854831search3turn854831search4  
Sources: https://support.anthropic.com/en/articles/11596036-anthropic-connectors-directory-faq and https://support.anthropic.com/en/articles/11817150-connect-your-tools-to-unlock-a-smarter-more-capable-ai-companion

The official Registry exposes its API at `https://registry.modelcontextprotocol.io` and explicitly warns that registry ownership validation does **not** imply verification that the publisher controls the declared remote server URL. citeturn567158search4turn567158search5turn567158search6  
Sources: https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx , https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/api/official-registry-api.md , https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/remote-servers.mdx

Cursor currently supports all three relevant transports—stdio, SSE and Streamable HTTP—and OAuth. Its newer Plugins/Marketplace layer is distinct from merely pasting MCP configuration. citeturn567158search7turn231843search3  
Sources: https://prod.cursor.com/docs/mcp and https://cursor.com/docs/plugins

Smithery is a special source of stale instructions right now. Its own notice says its proxy, managed identity, `server.smithery.ai`, `run.tools`, API/CLI/SDK execution products were shut down on **31 October 2026**; it remains a public non-proxied registry/discovery surface. citeturn854831search6  
Source: https://www.smithery.ai/future

Glama, mcp.so and similar indexes are useful for recall but their enormous counts should not be confused with maintained production endpoints. One independent observatory counted roughly 120k Glama entries, ~61.6k official-registry entries, ~20k mcp.so entries and ~14.9k Smithery entries on 11 November 2026—numbers that primarily illustrate catalog duplication and indexing breadth. citeturn854831search5  
Source: https://mcptoplist.com/observatory

For maintenance triage, the strongest signals are: vendor-owned domain **and** vendor repo/docs; remote Streamable HTTP rather than a stale SSE-only quickstart; OAuth2/OIDC discovery or a documented token model; a release or substantive commit in the last few months; protocol-version updates; security policy and explicit scopes; tests against live upstream APIs; documented read-only/destructive modes; and a tool count small enough to make context cost plausible. Red flags are one-click directory installs pointing to unrelated domains, query-string secrets, an HTTP server with no authentication, no release after 2025 protocol changes, an abandoned `npx` package, hundreds of broad write tools, or docs that still rely on a proxy product that has shut down.

### GitHub remote MCP versus `gh` CLI

For your fleet, **keep `gh` as default and make GitHub MCP opt-in/scoped**.

The best concrete measurement I found ran the same GitHub task through CLI, a skill and MCP variants on Claude Sonnet 5.5. In that benchmark, the shell/CLI path consumed about **1,421 tokens** total, while a default 46-tool GitHub MCP configuration consumed about **59,417 tokens**; enabling all 91 tools rose to about **101,351 tokens**. Restricting the MCP to two tools still used ~23,228 tokens, while aggressively limiting returned MCP result content reduced it to ~5,357. All variants solved the task, so the difference was predominantly interface/context cost rather than correctness. citeturn490519search4turn490519search6  
Sources: https://relay.openox.tech/blog/insights/mcp-vs-cli-vs-skills/ and https://dllg.cc/blog/insights/mcp-vs-cli-vs-skills/

That benchmark should **not** be generalized as a universal 40× tax. A separate schema analysis estimated GitHub’s 46 default tool definitions at about **11,207 tokens**, and found that an individual issue-write MCP tool definition (~807 tokens) was actually comparable to the relevant `gh issue create` help text (~869 tokens). The structural advantage of the CLI is that the agent normally carries only the shell tool schema and asks for command help only when necessary; it does not inject the entire GitHub vocabulary into every interaction. citeturn490519search5  
Source: https://importstatic.com/ai/mcp-tool-definition-token-overhead

The broader 2026 MCP-cost dataset reinforces the point without singling out GitHub. MCP Cost’s measurements put the median server around **2,562 tool-definition tokens**, 90th percentile around **19,087**, and 99th percentile around **55,031**. It also notes a crucial mitigation: Claude Code can defer MCP definitions through tool search, meaning the full schema size is a ceiling rather than necessarily a per-turn cost. citeturn490519search8turn490519search9  
Sources: https://mcpcost.ulehla.dev/tokens and https://mcpcost.ulehla.dev/

There is also more formal prior art: an open Zenodo benchmark compares the official GitHub MCP with shell-based interaction across multiple agent scaffolds, including Claude Code and Codex. citeturn490519search7  
Source: https://zenodo.org/records/21851992

So the deployment I would use is:

`gh` CLI remains globally available → GitHub MCP is **not** in the full default tool surface → when MCP is required, attach only `repos`, `issues`, or `pull_requests`, ideally `/readonly` → enable write toolsets only for the session that needs them.

That buys you structured JSON-like semantics where useful without paying the full persistent schema tax or giving every agent another route to merge, close, delete or mutate repository state.

## Explicit DON'T list and operating policy

| Do not permanently connect | Primary reason |
|---|---|
| **Official X MCP for user-context writes** | Requires local `xurl mcp` stdio bridge; direct hosted bearer mode is read-only. |
| **GitHub MCP with all/default broad toolsets on every agent** | Duplicates a highly successful `gh` workflow and can impose large tool-schema/result context cost. Use scoped/read-only/ad hoc instead. |
| **arXiv’s popular stdio server** | Good project, wrong transport for your measured process-explosion environment. |
| **Reddit community stdio server** | Low frequency, unofficial, wrong transport. |
| **content-distribution-mcp** | stdio-only and overlaps a scheduler/API workflow. |
| **LinkedIn automation MCPs/browser bots** | High account-policy risk; your account is already banned; no public general-purpose first-party posting MCP verified. |
| **Partiful browser-session MCP** | Unofficial, no public API found, depends on browser-session credentials. |
| **Ashby / Greenhouse / Wellfound ATS MCPs for job search** | They solve employer/recruiter workflows, not candidate-side browsing. |
| **Hetzner broad MCP always-on** | Huge cloud write surface and low demand; `hcloud`/REST are safer and cheaper. |
| **Tailscale community MCP always-on** | ~186 tools for six sessions; API/CLI is proportionate. |
| **Docker-management MCP** | Docker CLI is already a concise, mature agent interface; Gateway is infrastructure, not a better management primitive. |
| **Brave MCP in permanent config** | Self-host HTTP path is unauthenticated by default and cleaner hosted search alternatives exist. |
| **Exa + Firecrawl + Tavily + Jina simultaneously** | Four overlapping web-retrieval tool vocabularies increase schema cost and tool-selection ambiguity. Start with Jina; add one specialized service only when data proves a gap. |
| **Hootsuite/Publer alongside Typefully/Buffer** | Redundant public-write surfaces, extra OAuth credentials and duplicate queues. |
| **Playwright MCP always-on** | Duplicates a browser-control workflow with 15.3k measured calls; upstream itself recommends CLI/skills for many coding agents. |
| **Discord MCP** | Zero measured demand, community maturity, very large tool surface. |
| **Signal MCP** | Zero measured demand plus exceptionally sensitive message/identity access. |
| **Old self-hosted Notion MCP repo** | Notion explicitly directs users to its current remote service; old repo is no longer actively maintained. |
| **Old Smithery proxy URLs** | Smithery shut down its proxy/managed execution services on 31 Oct 2026. |

The permanent fleet I would actually run after this review is therefore small: **Typefully; scholarly-mcp; ORCID; Zenodo; Hugging Face; Jina; OpenRouter; Calendly; Cloudflare; and Luma**, with Buffer as the social alternative rather than a duplicate. GitHub stays CLI-first, X writes stay through Typefully/Buffer or ad-hoc official `xurl`, and cloud/admin MCPs remain scoped to sessions where they are actually needed.

The security model should be **least-capability by connector, not merely least-privilege by token**. A token can be narrowly scoped while a tool still accepts arbitrary untrusted text; conversely, a broad OAuth token can be tolerable if the exposed toolset is strictly read-only. For this fleet, the most defensible permanent pattern is public/read-only research servers freely callable, private-read servers permitted but isolated from automatic writes, and all externally visible or destructive actions—posts, deletes, DNS changes, repo mutation, calendar cancellation, cloud-resource changes—behind explicit approval. GitHub’s read-only endpoints, Buffer’s warning about whole-account reach, Typefully’s headless bearer option, and Greenhouse’s permission-aware OAuth all illustrate why the **tool surface, OAuth scope and approval boundary have to be considered together**, rather than treating “OAuth” as sufficient security. citeturn567158search1turn768058search1turn768058search3turn942234search8

## safe_urls
https://ac.linkedin.com/legal/user-agreement
https://ac.linkedin.com/legal/user-agreement?utm_source=chatgpt.com
https://ai.gotry.io/mcp/exa
https://ai.gotry.io/mcp/exa?utm_source=chatgpt.com
https://apis.io/servers/facebook-business-manager/facebook-business-manager-mcp-yml/
https://apis.io/servers/facebook-business-manager/facebook-business-manager-mcp-yml/?utm_source=chatgpt.com
https://beatview.ai/guides/lever-mcp
https://beatview.ai/guides/lever-mcp?utm_source=chatgpt.com
https://buffer.com/pricing
https://buffer.com/pricing?utm_source=chatgpt.com
https://calendly.com/help/connect-calendly-to-your-ai-tools
https://calendly.com/help/connect-calendly-to-your-ai-tools?utm_source=chatgpt.com
https://crossref.caseyjhand.com/
https://crossref.caseyjhand.com/?utm_source=chatgpt.com
https://cursor.com/docs/plugins
https://cursor.com/docs/plugins?utm_source=chatgpt.com
https://dev.to/nickytonline/introducing-the-devto-mcp-server-42jg
https://dev.to/nickytonline/introducing-the-devto-mcp-server-42jg?utm_source=chatgpt.com
https://developer.calendly.com/docs/mcp/calendly-mcp-server
https://developer.calendly.com/docs/mcp/calendly-mcp-server?utm_source=chatgpt.com
https://developer.linkedin.com/legal
https://developer.linkedin.com/legal?utm_source=chatgpt.com
https://developer.x.com/
https://developer.x.com/?utm_source=chatgpt.com
https://developers.buffer.com/examples/create-threaded-post.html
https://developers.buffer.com/examples/create-threaded-post.html?utm_source=chatgpt.com
https://developers.buffer.com/guides/integrations/mcp
https://developers.buffer.com/guides/integrations/mcp.html/
https://developers.buffer.com/guides/integrations/mcp.html/?utm_source=chatgpt.com
https://developers.buffer.com/guides/integrations/mcp?utm_source=chatgpt.com
https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/?utm_campaign=best-mcp-servers&utm_content=Cloudflare&utm_medium=blog&utm_term=blog-visit-website
https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/?utm_campaign=best-mcp-servers&utm_content=Cloudflare&utm_medium=blog&utm_term=blog-visit-website&utm_source=chatgpt.com
https://developers.openai.com/api/docs/guides/tools-connectors-mcp
https://developers.openai.com/api/docs/guides/tools-connectors-mcp?utm_source=chatgpt.com
https://devhunt.org/mcp/for/reddit
https://devhunt.org/mcp/for/reddit?utm_source=chatgpt.com
https://devhunt.org/mcp/server/arxiv
https://devhunt.org/mcp/server/arxiv?utm_source=chatgpt.com
https://devhunt.org/mcp/server/meta-social-technologies
https://devhunt.org/mcp/server/meta-social-technologies?utm_source=chatgpt.com
https://dllg.cc/blog/insights/mcp-vs-cli-vs-skills/
https://dllg.cc/blog/insights/mcp-vs-cli-vs-skills/?utm_source=chatgpt.com
https://docs.ashbyhq.com/ashby-mcp-server-beta
https://docs.ashbyhq.com/ashby-mcp-server-beta?utm_source=chatgpt.com
https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/
https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/?utm_source=chatgpt.com
https://docs.slack.dev/
https://docs.slack.dev/?utm_source=chatgpt.com
https://docs.slack.dev/ai/slack-mcp-server/
https://docs.slack.dev/ai/slack-mcp-server/?utm_source=chatgpt.com
https://exa.ai/mcp
https://exa.ai/mcp?utm_source=chatgpt.com
https://github.com/AutomateLab-tech/content-distribution-mcp
https://github.com/AutomateLab-tech/content-distribution-mcp?utm_source=chatgpt.com
https://github.com/IvanBBaev/facebook-mcp
https://github.com/IvanBBaev/facebook-mcp?utm_source=chatgpt.com
https://github.com/M-Romdhani/scholarly-mcp
https://github.com/M-Romdhani/scholarly-mcp?utm_source=chatgpt.com
https://github.com/VitexSoftware/mcp-server-mastodon
https://github.com/VitexSoftware/mcp-server-mastodon?utm_source=chatgpt.com
https://github.com/brave/brave-search-mcp-server
https://github.com/brave/brave-search-mcp-server?pubDate=20260209
https://github.com/brave/brave-search-mcp-server?pubDate=20260209&utm_source=chatgpt.com
https://github.com/brave/brave-search-mcp-server?utm_source=chatgpt.com
https://github.com/cappyeo/discord-mcp
https://github.com/cappyeo/discord-mcp?utm_source=chatgpt.com
https://github.com/cloudflare/mcp/blob/main/README.md
https://github.com/cloudflare/mcp/blob/main/README.md?utm_source=chatgpt.com
https://github.com/cotletdev/partiful-api-mcp-server
https://github.com/cotletdev/partiful-api-mcp-server?utm_source=chatgpt.com
https://github.com/cyanheads/bluesky-mcp-server
https://github.com/cyanheads/bluesky-mcp-server?utm_source=chatgpt.com
https://github.com/cyanheads/datacite-mcp-server
https://github.com/cyanheads/datacite-mcp-server?utm_source=chatgpt.com
https://github.com/cyanheads/orcid-mcp-server
https://github.com/cyanheads/orcid-mcp-server?utm_source=chatgpt.com
https://github.com/cyanheads/zenodo-mcp-server
https://github.com/cyanheads/zenodo-mcp-server?utm_source=chatgpt.com
https://github.com/exa-labs/exa-mcp-server
https://github.com/exa-labs/exa-mcp-server?utm_source=chatgpt.com
https://github.com/firecrawl/firecrawl-mcp-server
https://github.com/firecrawl/firecrawl-mcp-server?utm_source=chatgpt.com
https://github.com/ginkelsoft-development/mcp-hetzner
https://github.com/ginkelsoft-development/mcp-hetzner?utm_source=chatgpt.com
https://github.com/github/github-mcp-server
https://github.com/github/github-mcp-server/blob/main/README.md?plain=1
https://github.com/github/github-mcp-server/blob/main/README.md?plain=1&utm_source=chatgpt.com
https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md
https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md?utm_source=chatgpt.com
https://github.com/github/github-mcp-server/pkgs/container/github-mcp-server/
https://github.com/github/github-mcp-server/pkgs/container/github-mcp-server/?utm_source=chatgpt.com
https://github.com/github/github-mcp-server/releases
https://github.com/github/github-mcp-server/releases?utm_source=chatgpt.com
https://github.com/github/github-mcp-server?utm_source=chatgpt.com
https://github.com/huggingface/hf-mcp-server
https://github.com/huggingface/hf-mcp-server?utm_source=chatgpt.com
https://github.com/jina-ai/MCP
https://github.com/jina-ai/MCP?utm_source=chatgpt.com
https://github.com/makenotion/notion-mcp-server
https://github.com/makenotion/notion-mcp-server?utm_source=chatgpt.com
https://github.com/microsoft/playwright-mcp/blob/main/README.md
https://github.com/microsoft/playwright-mcp/blob/main/README.md?utm_source=chatgpt.com
https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx
https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx?utm_source=chatgpt.com
https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/remote-servers.mdx
https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/remote-servers.mdx?utm_source=chatgpt.com
https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/api/official-registry-api.md
https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/api/official-registry-api.md?utm_source=chatgpt.com
https://github.com/retog/signal-mcp
https://github.com/retog/signal-mcp?utm_source=chatgpt.com
https://github.com/smaniches/semantic-scholar-mcp
https://github.com/smaniches/semantic-scholar-mcp?utm_source=chatgpt.com
https://github.com/tailscale-mcp/tailscale-mcp
https://github.com/tailscale-mcp/tailscale-mcp?utm_source=chatgpt.com
https://github.com/tavily-ai/tavily-mcp
https://github.com/tavily-ai/tavily-mcp?utm_source=chatgpt.com
https://github.com/typefully/typefully-plugin
https://github.com/typefully/typefully-plugin?utm_source=chatgpt.com
https://github.com/xdevplatform/docs/blob/main/tools/mcp.mdx
https://github.com/xdevplatform/docs/blob/main/tools/mcp.mdx?utm_source=chatgpt.com
https://github.com/xdevplatform/docs/blob/main/tools/xurl.mdx
https://github.com/xdevplatform/docs/blob/main/tools/xurl.mdx?utm_source=chatgpt.com
https://github.com/xdevplatform/xurl/blob/main/CHANGELOG.md
https://github.com/xdevplatform/xurl/blob/main/CHANGELOG.md?utm_source=chatgpt.com
https://help.luma.com/p/mcp
https://help.luma.com/p/mcp?utm_source=chatgpt.com
https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt
https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt?utm_source=chatgpt.com
https://help.wellfound.com/article/1219-connect-wellfound-to-your-ai-assistant
https://help.wellfound.com/article/1219-connect-wellfound-to-your-ai-assistant?utm_source=chatgpt.com
https://hootsuite.my.site.com/hootsuite/s/article/about-mcps
https://hootsuite.my.site.com/hootsuite/s/article/about-mcps?utm_source=chatgpt.com
https://huggingface.co/docs/hub/agents-mcp
https://huggingface.co/docs/hub/agents-mcp?utm_source=chatgpt.com
https://huggingface.co/docs/inference-endpoints/guides/mcp_server
https://huggingface.co/docs/inference-endpoints/guides/mcp_server?utm_source=chatgpt.com
https://importstatic.com/ai/mcp-tool-definition-token-overhead
https://importstatic.com/ai/mcp-tool-definition-token-overhead?utm_source=chatgpt.com
https://index.zbs.gg/en/mcp/io-github-automatelab-tech-content-distribution/
https://index.zbs.gg/en/mcp/io-github-automatelab-tech-content-distribution/?utm_source=chatgpt.com
https://joestump.github.io/signal-mcp/docs/configuration/
https://joestump.github.io/signal-mcp/docs/configuration/?utm_source=chatgpt.com
https://learn.microsoft.com/en-us/azure/app-testing/playwright-cloud-browsers/how-to-playwright-workspaces-remote-mcp
https://learn.microsoft.com/en-us/azure/app-testing/playwright-cloud-browsers/how-to-playwright-workspaces-remote-mcp?utm_source=chatgpt.com
https://legal.mistral.ai/terms/eu-consumers-terms-of-service/
https://legal.mistral.ai/terms/eu-consumers-terms-of-service/?utm_source=chatgpt.com
https://mcpcost.ulehla.dev/
https://mcpcost.ulehla.dev/?utm_source=chatgpt.com
https://mcpcost.ulehla.dev/tokens
https://mcpcost.ulehla.dev/tokens?utm_source=chatgpt.com
https://mcptoplist.com/observatory
https://mcptoplist.com/observatory?utm_source=chatgpt.com
https://mcpyet.com/mcp/arxiv/
https://mcpyet.com/mcp/arxiv/?utm_source=chatgpt.com
https://openai.com/en-GB/policies/eu-terms-of-use/
https://openai.com/en-GB/policies/eu-terms-of-use/?utm_source=chatgpt.com
https://openrouter.ai/blog/announcements/openrouter-mcp-server/
https://openrouter.ai/blog/announcements/openrouter-mcp-server/?utm_source=chatgpt.com
https://openrouter.ai/mcp
https://openrouter.ai/mcp?utm_source=chatgpt.com
https://prod.cursor.com/docs/mcp
https://prod.cursor.com/docs/mcp?utm_source=chatgpt.com
https://publer.com/help/en/article/14orlq0/
https://publer.com/help/en/article/14orlq0/?utm_source=chatgpt.com
https://relay.openox.tech/blog/insights/mcp-vs-cli-vs-skills/
https://relay.openox.tech/blog/insights/mcp-vs-cli-vs-skills/?utm_source=chatgpt.com
https://support.airtable.com/articles/9897799762-using-the-airtable-mcp-server
https://support.airtable.com/articles/9897799762-using-the-airtable-mcp-server?utm_source=chatgpt.com
https://support.anthropic.com/en/articles/11596036-anthropic-connectors-directory-faq
https://support.anthropic.com/en/articles/11596036-anthropic-connectors-directory-faq?utm_source=chatgpt.com
https://support.anthropic.com/en/articles/11817150-connect-your-tools-to-unlock-a-smarter-more-capable-ai-companion
https://support.anthropic.com/en/articles/11817150-connect-your-tools-to-unlock-a-smarter-more-capable-ai-companion?utm_source=chatgpt.com
https://support.greenhouse.io/hc/en-us/articles/52096319906971-Set-up-Greenhouse-MCP-with-supported-AI-tools
https://support.greenhouse.io/hc/en-us/articles/52096319906971-Set-up-Greenhouse-MCP-with-supported-AI-tools?utm_source=chatgpt.com
https://support.greenhouse.io/hc/en-us/articles/57182931313819-Greenhouse-MCP-security-FAQ
https://support.greenhouse.io/hc/en-us/articles/57182931313819-Greenhouse-MCP-security-FAQ?utm_source=chatgpt.com
https://support.typefully.com/en/articles/13128440-typefully-mcp-server
https://support.typefully.com/en/articles/13128440-typefully-mcp-server?utm_source=chatgpt.com
https://support.typefully.com/en/articles/8718287-typefully-api
https://support.typefully.com/en/articles/8718287-typefully-api?utm_source=chatgpt.com
https://tailscale.com/docs/aperture/mcp-server
https://tailscale.com/docs/aperture/mcp-server?utm_source=chatgpt.com
https://typefully.com/docs/api
https://typefully.com/docs/api?utm_source=chatgpt.com
https://www.airtable.com/platform/mcp
https://www.airtable.com/platform/mcp?utm_source=chatgpt.com
https://www.ashbyhq.com/product-updates/mcp
https://www.ashbyhq.com/product-updates/mcp?utm_source=chatgpt.com
https://www.ayrshare.com/install/
https://www.ayrshare.com/install/?utm_source=chatgpt.com
https://www.ayrshare.com/integrations/
https://www.ayrshare.com/integrations/?utm_source=chatgpt.com
https://www.browserbase.com/mcp
https://www.browserbase.com/mcp?utm_source=chatgpt.com
https://www.hootsuite.com/integrations/mcp
https://www.hootsuite.com/integrations/mcp?utm_source=chatgpt.com
https://www.linkedin.com/help/linkedin/answer/a1340567/automated-activity-on-linkedin?lang=en
https://www.linkedin.com/help/linkedin/answer/a1340567/automated-activity-on-linkedin?lang=en&utm_source=chatgpt.com
https://www.notion.com/help/notion-mcp
https://www.notion.com/help/notion-mcp?utm_source=chatgpt.com
https://www.npmjs.com/package/%40browserbasehq/mcp
https://www.npmjs.com/package/%40browserbasehq/mcp?utm_source=chatgpt.com
https://www.npmjs.com/package/%40tailscale-mcp/tailscale-mcp
https://www.npmjs.com/package/%40tailscale-mcp/tailscale-mcp?utm_source=chatgpt.com
https://www.postreach.ai/blogs/linkedin-instagram-mcp-servers-compared
https://www.postreach.ai/blogs/linkedin-instagram-mcp-servers-compared?utm_source=chatgpt.com
https://www.smithery.ai/future
https://www.smithery.ai/future?utm_source=chatgpt.com
https://zenodo.org/records/20023539
https://zenodo.org/records/20023539?utm_source=chatgpt.com
https://zenodo.org/records/21851992
https://zenodo.org/records/21851992?utm_source=chatgpt.com