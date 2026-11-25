# DR26-11-17-MACANTON-01-1941 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Deep Research: Economics and Mechanics of AI-Agent Skill & Tool Catalogs as of November 17, 2026

## Executive summary

- **The highest-leverage MCP distribution point is now the official MCP Registry.** It is the canonical metadata layer rather than a hosting service, uses `server.json` plus namespace verification, and is explicitly designed so downstream registries can ingest it. Glama confirms that it republishes the entire official registry; mcp.directory confirms that it auto-discovers official-registry entries. For a free maintainer, publishing once to the official registry can therefore create multiple downstream placements with near-zero incremental submission effort. **CONFIRMED.** citeturn800024view0turn800024view1turn866978view0turn800024view9  
  Sources: https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx ; https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/authentication.mdx ; https://glama.ai/mcp/methodology ; https://mcp.directory/submit

- **For Agent Skills, there is no single canonical marketplace analogous to the official MCP Registry.** `agentskills.io` is the specification/ecosystem site and explicitly says it does **not** accept community skill submissions; skills.sh is currently the broadest GitHub-native discovery/install surface researched here, while Anthropic, OpenAI and Cursor operate product-specific plugin/skill distribution systems. **CONFIRMED.** citeturn566918view0turn882906view2turn800024view15turn796574view0turn796574view3  
  Sources: https://github.com/agentskills/agentskills/blob/main/CONTRIBUTING.md ; https://www.skills.sh/docs ; https://github.com/anthropics/claude-plugins-official ; https://developers.openai.com/plugins/deploy/submission ; https://prod.cursor.com/docs/plugins

- **Do not equate catalog size, traffic, page views, stars, pulls, installs or active usage.** The best independent 2026 comparison found shows radically different metrics: Cline exposes installs, Docker exposes image pulls, mcp.so exposes listing views, GitHub's registry emphasizes repository stars, while PulseMCP publishes modeled visitor estimates. These cannot be normalized without first-party activation telemetry. **CONFIRMED for metric types; INFERRED for comparability conclusion.** citeturn866978view1turn882906view1  
  Sources: https://www.hedwigai.com/blog/mcp-registries ; https://www.pulsemcp.com/servers

- **mcp.so is one of the clearest pay-for-visibility directories.** Its current submit page offers a one-time **$39** tier promising immediate publication without review, a verified badge, priority/featured placement and a dofollow link; its advertising page sells Silver/Gold/Platinum placements at **$399/$699/$1,299 per month**. That makes it useful as an optional discovery channel but unsuitable as a neutral signal of organic quality. **CONFIRMED.** citeturn800024view6turn800024view7  
  Sources: https://mcp.so/submit ; https://mcp.so/advertise

- **Smithery underwent a material platform retreat in 2026, but the directory did not die.** Arcade acquired Smithery on **August 5, 2026**; Smithery then shut down its proxy, managed connections/identity, APIs, accounts, API keys, CLI/SDK infrastructure and `server.smithery.ai`/`run.tools` proxy URLs on **October 31, 2026**. The public directory remains, now oriented around direct vendor MCP endpoints and powered with Arcade. **CONFIRMED.** citeturn866978view4turn800024view4  
  Sources: https://www.arcade.dev/blog/smithery-joins-arcade/ ; https://www.smithery.ai/future

- **PulseMCP is alive as a discovery site but closed to normal listing changes at the research date.** Its homepage says new server submissions and listing changes are paused while the team reworks ingestion and listing management. No primary source reviewed gives a reopening date or a more specific reason. **CONFIRMED; reason beyond that is UNKNOWN.** citeturn800024view5turn882906view1  
  Sources: https://www.pulsemcp.com/ ; https://www.pulsemcp.com/servers

- **Client-integrated catalogs are more valuable than SEO directories when the metric is actual use.** Cline can expose servers to one-click installation and publishes install counts; Docker distributes approved servers as packaged, signed container images and exposes pulls; Claude/OpenAI/Cursor put extensions inside the agent product itself. **INFERRED ranking; underlying mechanics CONFIRMED.** citeturn800024view10turn800024view11turn800024view15turn796574view0turn796574view3  
  Sources: https://github.com/cline/mcp-marketplace ; https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog ; https://github.com/anthropics/claude-plugins-official ; https://developers.openai.com/plugins/deploy/submission ; https://prod.cursor.com/docs/plugins

- **For `tonydzi/join-the-fleet`, keep the 232 skills in one canonical monorepo but do not market them only as one 232-skill blob.** Vercel's Skills CLI supports repositories containing multiple named skills; Anthropic's plugin format can expose selected skill directories. The best packaging architecture is therefore one source monorepo plus several thematic, curated plugin/skill packs and an optional "all" bundle. **Mechanics CONFIRMED; packaging recommendation INFERRED.** citeturn566918view2turn800024view15  
  Sources: https://vercel.com/docs/agent-resources/skills ; https://github.com/anthropics/claude-plugins-official

- **Awesome-lists are becoming hostile to bulk/low-signal submissions.** `awesome-claude-skills` currently requires social proof, automatically closes skill submissions below 10 GitHub stars in most cases, explicitly prohibits AI-generated/AI-submitted PRs, and had **891 open PRs** when checked. `awesome-claude-code` requires age/activity or ≥100 stars, allows only one recommended resource at a time and automatically closes failures. **CONFIRMED.** citeturn566918view1turn161972view3  
  Sources: https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md ; https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md

- **For this lab, the core distribution stack should be:** official MCP Registry → automatic mirrors/claims → Cline/Docker for mature MCPs; skills.sh → Claude/OpenAI plugin directories for strong skill bundles; mcp.so and selected awesome-lists only as secondary channels. Measure **first successful use**, not listing count or stars, as the primary KPI. **INFERRED recommendation based on the evidence below.**

This research used more than **30 distinct web searches** and opened more than **40 distinct URLs**, with primary sources preferred for entry requirements, monetization and status. Traffic estimates are explicitly separated from first-party metrics because the methodologies differ materially.

## Taxonomy and acceptance

The market in late 2026 is better understood as six different layers rather than one homogeneous "directory" market: canonical registries; independent aggregators/directories; client-integrated catalogs; skill/plugin marketplaces; human-curated awesome-lists; and package/application indexes. Their economics and value to a maintainer differ substantially. The official MCP Registry is a metadata and namespace layer; Glama and mcp.directory are downstream discovery systems; Cline/Docker/GitHub connect catalog entries to an execution environment; Claude/OpenAI/Cursor package capabilities for their own agent products; awesome-lists sell no runtime access at all; npm/PyPI/GitHub Marketplace are generic software distribution channels rather than MCP/Skill directories. citeturn800024view0turn866978view0turn800024view10turn800024view11turn800024view15turn796574view0turn796574view8turn796574view9

**Legend:** **C** = CONFIRMED from a primary source opened in this research; **I** = INFERRED from platform mechanics; **U** = UNKNOWN/not publicly established in the reviewed material.

| Catalog | Class | Accepts | Business model | Entry door | Requirements | Evidence of traffic / use | Status, Nov. 17 2026 | Source URLs |
|---|---|---|---|---|---|---|---|---|
| Official MCP Registry | Canonical MCP registry | MCP server metadata/packages/remote endpoints | **C:** public ecosystem infrastructure; no paid listing found. **I:** strategic interoperability layer | `mcp-publisher` CLI/API | `server.json`; authenticated publisher; GitHub namespace or DNS/domain ownership | **C:** registry itself does not publish a meaningful per-listing use metric. Secondary census found tens of thousands of entries, but catalog size ≠ users | **Alive / preview-active** | https://github.com/modelcontextprotocol/registry ; https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx ; https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/authentication.mdx citeturn800024view2turn800024view0turn800024view1 |
| Glama | MCP aggregator + quality/security layer + hosting/gateway | Open-source MCP servers; remote HTTP MCP | **C:** paid hosting/workspace/gateway services; quality scanning and request/connection observability. **U:** no evidence reviewed that telemetry itself is sold | Official-registry import; GitHub listing/claim | Buildable/runnable server; license/security/health checks; maintainer can claim | **C:** says it performed >1M scans in preceding 12 months; no trustworthy public per-listing acquisition figure found | **Alive** | https://glama.ai/mcp/faq ; https://glama.ai/mcp/methodology citeturn800024view3turn866978view0 |
| Smithery | MCP directory; formerly proxy/managed connection platform | MCP servers | **C:** former hosted proxy/managed identity infrastructure ended; directory retained under Arcade. **U:** current directory listing monetization not public | Current directory is curated/direct-endpoint oriented | Current verified direct server endpoint; former Smithery proxy no longer usable | Secondary DataForSEO estimate: ~2.4k Google-organic visits/month to registry in Nov. 2026; not equivalent to total site traffic or listing conversions | **Directory alive; proxy/runtime shut Oct. 31, 2026** | https://www.smithery.ai/future ; https://www.arcade.dev/blog/smithery-joins-arcade/ ; https://www.hedwigai.com/blog/mcp-registries citeturn800024view4turn866978view4turn866978view1 |
| PulseMCP | MCP directory/news ecosystem | MCP servers | **U:** no catalog-specific monetization found | Normally submission/listing management; currently unavailable | Submission requirements temporarily moot | Site exposes Recommended, Recent and popularity views plus modeled weekly visitor estimates; these are model estimates, not audited installs | **Browsing alive; new submissions/listing changes paused** | https://www.pulsemcp.com/ ; https://www.pulsemcp.com/servers citeturn800024view5turn882906view1 |
| mcp.so | Multi-category AI directory | MCP servers, remote MCP, clients, Agent Skills, CLI tools, AI products, loops | **C:** $39 fast-track/priority listing; $399/$699/$1,299 monthly ad tiers | Web form | URL/repository/endpoint and metadata; paid tier bypasses normal review delay | **Self-reported:** 2.2M unique visitors/12mo, 6M page views/12mo, 266k monthly active users. Third party estimated ~327k visits/mo Sept. 2026; DataForSEO organic estimate much smaller | **Alive** | https://mcp.so/submit ; https://mcp.so/advertise ; https://www.agentlaun.ch/directories/mcp-so/ ; https://www.hedwigai.com/blog/mcp-registries citeturn800024view6turn800024view7turn796574view12turn866978view1 |
| mcpservers.org | MCP directory | MCP servers | Indexed submit page reported free and $39 premium fast-track tiers; **live page timed out twice during verification**, so current details are **PARTIALLY VERIFIED** | Web form | Indexed page requested repository/site/registry metadata | Third-party directory estimator reported substantial site traffic; no audited per-listing conversion data found | **Apparently alive; submission page could not be live-verified in this session** | https://mcpservers.org/submit ; https://www.agentlaun.ch/directories/mcp-registries/ citeturn796574view13 |
| mcp.directory | MCP aggregator/directory | MCP servers; site also exposes Skills browsing | **U:** no monetization substantiated from reviewed primary page | GitHub URL submission; official-registry auto-discovery; claim existing listing | Repository metadata; optional npm/PyPI; automatic tool detection | Third-party site-traffic estimate exists; no first-party install metric found | **Alive** | https://mcp.directory/submit ; https://www.agentlaun.ch/directories/mcp-registries/ citeturn800024view9turn796574view13 |
| Cline MCP Marketplace | Client-integrated catalog | MCP servers | **I:** strategic ecosystem/client retention; no listing charge found | GitHub issue in marketplace repo | GitHub repo; square ≥400×400 PNG icon; working README/`llms-install.md`; adoption, maintainer credibility, maturity/security considered; no hard minimum stars | **Strongest type of evidence:** public install counts. Secondary snapshot found Filesystem ~162,680 and Context7 ~135,084 Cline installs | **Alive** | https://github.com/cline/mcp-marketplace ; https://www.hedwigai.com/blog/mcp-registries citeturn800024view10turn866978view1 |
| Docker MCP Catalog | Client/runtime catalog | Containerized MCP servers | **C:** strategic Docker Desktop/Hub distribution; paid enterprise governance is part of broader Docker AI offering; no listing fee found | PR to `docker/mcp-registry` | Docker-build or compatible image metadata; curated review; Docker-built path gives signed image, SBOM/provenance and managed updates | Public image pulls provide a use-adjacent metric; secondary snapshot found some servers in millions of pulls, but pulls ≠ unique users | **Alive** | https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog ; https://github.com/docker/mcp-registry ; https://www.hedwigai.com/blog/mcp-registries citeturn800024view11turn800024view12turn866978view1 |
| GitHub MCP Registry / VS Code surface | Client-integrated MCP catalog + enterprise registry controls | MCP servers | **C:** strategic Copilot discovery; private/enterprise MCP registry controls are part of organization governance. No public listing fee found | Public catalog originally curated; current independent direct-public submission path not clearly documented in sources reviewed | Public inclusion path **UNKNOWN**; private registries support compatible registry endpoints | Public UI emphasizes repository/star signals rather than verified activation. Secondary snapshot counted ~770 entries | **Alive / public preview ecosystem** | https://github.blog/changelog/2025-09-16-github-mcp-registry-the-fastest-way-to-discover-ai-tools/ ; https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-registry ; https://www.hedwigai.com/blog/mcp-registries citeturn800024view13turn800024view14turn866978view1 |
| Claude plugin directory / official marketplace | Product-integrated plugin directory | Claude plugins containing skills and/or other plugin components; Claude Code marketplace | **I:** product ecosystem/retention; no listing fee or commission found | `claude.ai/directory/manage`; not PR to mirror repo | Review/security pipeline; plugin source can expose one or multiple `SKILL.md` folders; accepted plugins become available across Claude products | No public per-plugin install/referrer dataset identified | **Alive** | https://github.com/anthropics/claude-plugins-official ; https://github.com/anthropics/claude-plugins-community/blob/main/README.md citeturn800024view15turn800024view16 |
| Anthropic community plugin GitHub repo | Mirror, not primary marketplace | Approved Claude community plugins | **C:** read-only mirror of internal review pipeline; no direct monetization | Do **not** PR; entries sync from Anthropic pipeline | Must first submit through Claude directory | No independent traffic metric needed; GitHub repo is not canonical submission path | **Alive mirror** | https://github.com/anthropics/claude-plugins-community/blob/main/README.md citeturn800024view16 |
| skills.sh | GitHub-native Agent Skill directory/install index | Agent Skills in repositories, including multi-skill repos | **C:** anonymous install telemetry drives leaderboard; no paid placement found. **I:** strategic ecosystem value to Vercel | `npx skills add owner/repo`; installation can make community skills discoverable | Agent Skills-compatible folder/SKILL.md; GitHub repository; individual skill selectable from multi-skill repo | First-party CLI install telemetry; installation badge/leaderboards | **Alive** | https://www.skills.sh/docs ; https://vercel.com/docs/agent-resources/skills citeturn882906view2turn566918view2 |
| agentskills.io | Specification/ecosystem site, **not** skill marketplace | Client/platform ecosystem listings; **not community skills** | Open-standard/specification function | PR only for compatible client/platform ecosystem listing | Publicly available product that can actually discover/execute skills; logo/docs evidence | Anthropic's Nov. 2026 announcement says the wider Agent Skills ecosystem had reached roughly 1M published skills and nearly 280M installs; this is ecosystem-wide, not agentskills.io traffic | **Alive; community skill submissions explicitly not accepted** | https://github.com/agentskills/agentskills/blob/main/CONTRIBUTING.md ; https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation citeturn566918view0turn800024view18 |
| OpenAI ChatGPT/Codex plugin directory | Product-integrated plugin directory | Plugins with skills, MCP connections, or both | **I:** platform ecosystem/distribution; no listing fee/commission identified in docs reviewed | Plugin submission portal; upload ZIP | Root `plugin.json`; skills paths; `mcp.json` for MCP; domain verification for MCP; test cases, credentials where needed, review artifacts | No public per-plugin install count found. Guidelines state utility/user satisfaction may affect enhanced distribution | **Alive** | https://developers.openai.com/plugins ; https://developers.openai.com/plugins/deploy/submission ; https://developers.openai.com/plugins/plugin-guidelines?site_locale=en citeturn796574view2turn796574view0turn796574view1 |
| Cursor Marketplace | Product-integrated plugin catalog | Plugins, including skills/MCP integrations | **C:** team/enterprise marketplaces are product governance features; no public community listing fee identified | Submit Git repository to Cursor team; manual review | Every plugin manually reviewed; team marketplace rules differ from public/community directory | No public per-plugin install statistics identified in official docs reviewed | **Alive** | https://prod.cursor.com/docs/plugins citeturn796574view3 |
| cursor.directory | Community directory linked by Cursor | Community plugins/MCP servers | **U:** economics and detailed current submission mechanics not established from primary sources reviewed | Community directory | Not sufficiently documented in primary Cursor docs reviewed | No reliable per-listing usage data found | **Alive, but lower-confidence characterization** | https://prod.cursor.com/docs/plugins citeturn796574view3 |
| `awesome-mcp-servers` by wong2 | Human-curated awesome-list/front end | MCP servers | No direct monetization identified | Current README says **no PRs**; submit via mcpservers.org | Routed through mcpservers.org | GitHub stars/traffic are awareness proxies; no install attribution | **Alive; submission outsourced** | https://github.com/wong2/awesome-mcp-servers/blob/main/README.md citeturn796574view7 |
| `awesome-claude-skills` | Human-curated awesome-list | Claude/Agent Skills and resources | None observed | Pull request | Functional/tested; documentation; typically ≥10 stars; social proof; no AI-generated/submitted PR; no SaaS funnels | ~891 open PRs at Nov. 17 check is evidence of severe review congestion, not end-user traffic | **Alive but congested** | https://github.com/travisvn/awesome-claude-skills ; https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md citeturn882906view5turn566918view1 |
| `awesome-claude-code` / similar lists | Human-curated awesome-lists | Claude Code resources/plugins/skills depending list | None observed | Maintainer-specific PR/issue workflow | Example: `hesreallyhim` requires ≥14 days + active development **or** ≥100 stars, one resource at a time; only collaborators can currently create PRs | Large GitHub-star audiences, but stars are not installs | **Alive, tightly curated** | https://github.com/hesreallyhim/awesome-claude-code ; https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md ; https://github.com/subinium/awesome-claude-code/blob/main/README.md citeturn882906view4turn161972view3turn796574view6 |
| PyPI | General package index | Python packages, including MCP implementation packages | General package infrastructure; not MCP-specific paid discovery | `pyproject.toml` + upload/Trusted Publishing | Valid package metadata; PyPI recommends Trusted Publishing/OIDC | Package downloads measurable, but downloads ≠ MCP activation | **Alive** | https://docs.pypi.org/publishing/publishing-to-pypi/ citeturn796574view8 |
| npm | General package index | npm packages, including MCP servers/CLIs | General package infrastructure; not MCP-specific paid discovery | `npm publish` / trusted publishing | npm package metadata/account or trusted publishing | Package downloads measurable, but dependency installs can inflate them | **Alive** | https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages/ citeturn796574view9 |
| GitHub Marketplace | App/Action marketplace | GitHub Apps, OAuth apps, Actions — **not raw MCP/SKILL.md repos** | **C:** GitHub Marketplace paid transactions pay developers 95%, GitHub 5% under current agreement | Marketplace app listing process | Verified publisher and app-specific requirements; paid GitHub Apps need minimum adoption thresholds | App installations/users are meaningful for GitHub-native apps, irrelevant to plain skills/MCP repos | **Alive, usually wrong category for these artifacts** | https://docs.github.com/en/apps/github-marketplace/creating-apps-for-github-marketplace/requirements-for-listing-an-app ; https://docs.github.com/en/site-policy/github-terms/github-marketplace-developer-agreement citeturn796574view10turn796574view11 |

### Mirrors and automatic importers

The strongest confirmed import graph is:

`Official MCP Registry → Glama` — Glama explicitly states that it is a **superset** of the official registry and ingests/re-publishes everything from it. Its methodology then builds, runs, introspects, audits and scores servers rather than merely copying metadata. **CONFIRMED.** citeturn866978view0  
https://glama.ai/mcp/methodology

`Official MCP Registry → mcp.directory` — mcp.directory's submission UI says already-listed projects may have been automatically discovered from the official registry and can be claimed by their maintainer. **CONFIRMED.** citeturn800024view9  
https://mcp.directory/submit

`Anthropic internal plugin review pipeline → anthropics/claude-plugins-community` — the GitHub repository is explicitly a **read-only, nightly-synchronized mirror**, and direct PRs are not the submission mechanism. **CONFIRMED.** citeturn800024view16  
https://github.com/anthropics/claude-plugins-community/blob/main/README.md

`GitHub repository → skills.sh` is not a registry mirror in the same sense. The CLI installs directly from GitHub-style `owner/repo` coordinates, and skills.sh uses anonymous installation telemetry for ranking/discovery; Vercel documentation explicitly supports selecting a named skill from a multi-skill repo. **CONFIRMED.** citeturn882906view2turn566918view2  
https://www.skills.sh/docs ; https://vercel.com/docs/agent-resources/skills

For GitHub's MCP Registry, the September 16, 2025 launch announcement described an initially curated catalog and said GitHub was working with the official open-source MCP Registry. In the primary sources reviewed here, a current statement that GitHub simply mirrors every official-registry entry was not found. Therefore, **automatic full official-registry → GitHub import is UNKNOWN**, and it would be unsafe to assume it. citeturn800024view13  
https://github.blog/changelog/2025-09-16-github-mcp-registry-the-fastest-way-to-discover-ai-tools/

## Business models and strategic incentives

The catalogs fall into four economically distinct groups.

**Paid attention marketplaces.** mcp.so is the clearest confirmed example. Its submission page monetizes reduced friction and ranking: the $39 one-time option advertises immediate publication, no review delay, a verified badge, priority search/featured placement and a dofollow backlink. Its separate advertising inventory is priced at $399/month for Silver, $699 for Gold and $1,299 for Platinum. This means ranking is not purely a quality or popularity signal. **CONFIRMED.** citeturn800024view6turn800024view7  
https://mcp.so/submit ; https://mcp.so/advertise

The indexed mcpservers.org submission page also advertised a free path and a $39 premium path, but that live page timed out on repeated direct-open attempts during this research. The claim should therefore be treated as **PARTIALLY VERIFIED rather than primary-source confirmed**, and no expenditure should be made without re-checking the live page.  
https://mcpservers.org/submit

**Hosting/runtime businesses wrapped around discovery.** Glama's directory is coupled to hosted MCP infrastructure, workspace services and request/connection observability. Its methodology makes the directory itself a quality/security funnel into the broader platform: servers are built, run and introspected, failed builds can be withheld from discovery, and suspected malicious behavior can trigger review/delisting. The economic logic — free discovery feeding hosting/workspace/gateway use — is **INFERRED**, while the existence of those services and quality controls is **CONFIRMED**. citeturn800024view3turn866978view0  
https://glama.ai/mcp/faq ; https://glama.ai/mcp/methodology

Smithery was formerly the strongest example of the same model: catalog → connection proxy → identity/runtime. That model changed materially in 2026. Arcade announced its acquisition of Smithery on **August 5, 2026**, emphasizing the combination of Smithery's server discovery/publishing ecosystem with Arcade's enterprise security, reliability and governance. Smithery subsequently announced that its proxy and managed connection infrastructure would end on **October 31, 2026**; `run.tools`, `server.smithery.ai`, Connect, Toolbox, accounts/API keys and Smithery CLI/SDK infrastructure were among the affected components, while the public directory remained. **CONFIRMED.** citeturn866978view4turn800024view4  
https://www.arcade.dev/blog/smithery-joins-arcade/ ; https://www.smithery.ai/future

The primary sources do **not** disclose the Smithery acquisition price or a reliable standalone Smithery funding total. Secondary venture databases found during research disagree or lack primary substantiation. The defensible answer is therefore: **Smithery financing amount: UNKNOWN/not public in the primary sources reviewed; acquisition terms: not publicly disclosed in the sources reviewed.** Arcade's strategic rationale is public, but it should not be reverse-engineered into a Smithery valuation. citeturn866978view4  
https://www.arcade.dev/blog/smithery-joins-arcade/

**Client/platform ecosystem catalogs.** Cline, Docker, GitHub, Claude, OpenAI and Cursor have a different incentive: the catalog improves the value of the host agent/runtime. Cline's marketplace offers direct installation into Cline; Docker turns MCP servers into curated images available through Docker Desktop and Docker Hub; GitHub ties MCP discovery and private registries to Copilot governance; Claude distributes reviewed plugins across Claude products; OpenAI's plugin directory spans ChatGPT and Codex; Cursor offers manual plugin review plus team/enterprise marketplace controls. Direct listing charges were not found in the reviewed official documentation for these catalogs. **Platform-distribution mechanics CONFIRMED; "retention/ecosystem moat" motive INFERRED.** citeturn800024view10turn800024view11turn800024view14turn800024view15turn796574view2turn796574view3  
https://github.com/cline/mcp-marketplace ; https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog ; https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-registry ; https://github.com/anthropics/claude-plugins-official ; https://developers.openai.com/plugins ; https://prod.cursor.com/docs/plugins

Docker and GitHub provide particularly clear enterprise-governance plays. Docker's catalog/toolkit is coupled to Docker's runtime, image provenance and governance surface; GitHub supports organization-controlled private MCP registries compatible with registry APIs. Cursor likewise documents private/team marketplaces for Teams and Enterprise. These are economically more important than charging open-source maintainers for a listing because the control plane is sold to organizations. **CONFIRMED for product capabilities; exact catalog-attributable revenue is UNKNOWN.** citeturn800024view11turn800024view12turn800024view14turn796574view3  
https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog ; https://github.com/docker/mcp-registry ; https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-registry ; https://prod.cursor.com/docs/plugins

**Telemetry-backed ranking.** skills.sh explicitly says its leaderboard uses anonymous installation telemetry from the Skills CLI and says it tracks which skills were installed rather than personal data or usage patterns. This is **confirmed telemetry collection for ranking**; there is **no evidence in the reviewed source that Vercel sells that telemetry or monetizes it as data**, so "data monetization" would be unsupported. citeturn882906view2  
https://www.skills.sh/docs

Glama similarly gathers operational information to test, score and monitor connectors. Again, the reviewed material establishes use of telemetry for functionality, quality and diagnostics, not sale of the data. **Telemetry collection CONFIRMED; telemetry monetization UNKNOWN.** citeturn866978view0  
https://glama.ai/mcp/methodology

**Awesome-lists and package indexes.** The reviewed awesome-lists do not charge listing fees; their scarce resource is maintainer attention. As the lists became flooded, maintainers introduced social-proof and submission-quality gates. PyPI/npm are package distribution infrastructure rather than MCP discovery businesses, while GitHub Marketplace is a genuine transaction marketplace for GitHub Apps/OAuth apps/Actions and currently allocates 95% of paid Marketplace revenue to the developer and 5% to GitHub under its developer agreement. **CONFIRMED.** citeturn566918view1turn161972view3turn796574view8turn796574view9turn796574view11  
https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md ; https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md ; https://docs.pypi.org/publishing/publishing-to-pypi/ ; https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages/ ; https://docs.github.com/en/site-policy/github-terms/github-marketplace-developer-agreement

**Funding claims require restraint.** For independent catalogs such as Glama, PulseMCP, mcp.so and mcp.directory, no sufficiently authoritative primary disclosures of catalog-specific venture financing were found in the opened sources. They should be recorded as **UNKNOWN/not public**, not filled with numbers from startup databases. Smithery's acquisition by Arcade is confirmed, but deal terms are not. For GitHub/Microsoft, Docker, Anthropic, OpenAI, Vercel and Cursor, company-level financing is not evidence of the economics of their specific registries, so treating total corporate fundraising as "registry funding" would be misleading. citeturn866978view4

## Real traffic, ranking and measurable impact

The central measurement problem is that the catalogs expose **different points in the funnel**.

A useful hierarchy is:

`directory impression → listing view → click to repository → install/pull → successful initialize → first successful tool/skill use → repeat use → star`

A metric farther to the right is usually more valuable. A GitHub star or page view measures awareness; a Docker pull may represent CI/re-pulls rather than a new human user; a Cline installation is closer to intent; an MCP `initialize` followed by a successful tool invocation is closer to actual activation.

The best cross-catalog comparative study found in this research is Hedwig's **November 12, 2026** registry analysis. It deliberately distinguishes the native metrics exposed by different catalogs rather than pretending they are interchangeable. In its snapshot, Cline exposed installs, Docker exposed pulls, Smithery exposed connections on the subset that remained measurable, mcp.so exposed views and GitHub's catalog exposed GitHub-star signals. Example values included roughly **162,680 Cline installs for Filesystem**, **135,084 for Context7**, and about **2.77 million Docker pulls for Playwright**; by contrast, mcp.so showed tens of thousands of listing views and GitHub's Playwright entry inherited tens of thousands of repository stars. These are not estimates of the same thing. **Secondary source; methodology transparent enough to be useful, but values should not be treated as audited first-party business metrics.** citeturn866978view1  
https://www.hedwigai.com/blog/mcp-registries

The same analysis estimated Google-organic search traffic using DataForSEO and produced much smaller figures than whole-site traffic estimators: approximately **4,935 monthly organic visits for mcp.so**, **2,401 for Smithery**, **1,816 for PulseMCP** and **1,024 for the official registry** at the time of its November 2026 snapshot. That methodology intentionally excludes direct traffic, referrals, in-client discovery and most agent-driven installation paths. It is therefore useful for measuring **SEO reach**, not total users. citeturn866978view1  
https://www.hedwigai.com/blog/mcp-registries

AgentLaunch's September 2026 directory comparison reports much larger site-level traffic estimates — roughly **327k visits/month for mcp.so**, **341k for Smithery**, **214k for Glama**, **127k for mcp.directory**, and higher figures for some other MCP-directory sites. The discrepancy is not evidence that one source is necessarily false: AgentLaunch is estimating broader website visits, whereas Hedwig specifically reports DataForSEO-estimated Google-organic traffic. Both are third-party estimates with substantial uncertainty and neither tells you how many visitors install *your* MCP server. citeturn796574view12turn796574view13  
https://www.agentlaun.ch/directories/mcp-so/ ; https://www.agentlaun.ch/directories/mcp-registries/

mcp.so's own advertiser page claims **2.2 million unique visitors over 12 months, 6 million page views over 12 months and 266,000 monthly active users**. These numbers are first-party marketing claims rather than independently audited metrics in the source, so they should be labeled **SELF-REPORTED**, not mixed with Similarweb/DataForSEO-type estimates. citeturn800024view7  
https://mcp.so/advertise

PulseMCP labels its server-level metric as **"Est Visitors (Week)"** and supports Recommended, Recently Released, Most Popular All Time, Month and Week sorting. "Estimated" is important: it is not evidence that PulseMCP observed an install or successful MCP call. citeturn882906view1  
https://www.pulsemcp.com/servers

skills.sh is unusually useful for skill distribution because the ranking signal is tied to the actual installation CLI. Its documentation says the leaderboard derives from **anonymous install telemetry** and the site exposes install-count badges and trending/hot views. That is materially more actionable than repository stars, although it still stops before successful skill execution. **CONFIRMED.** citeturn882906view2  
https://www.skills.sh/docs

Glama's scoring is much more technical than a simple popularity leaderboard. Its methodology says it automatically builds, launches, introspects, audits and scores servers; failed reproducible builds can be hidden from discovery and malicious findings can lead to review/delisting. Its quality model includes several dimensions rather than simply GitHub stars. It also says it carried out more than **one million scans in the preceding 12 months**. This makes Glama potentially valuable for trustworthy projects even when raw traffic is modest. citeturn866978view0  
https://glama.ai/mcp/methodology

Cline's marketplace review considers community adoption, developer credibility, project maturity and security, but the repository explicitly says there is **no minimum star count** and gives an indicative review goal of a couple of days rather than a rigid SLA. The marketplace's value comes from being inside the client and supporting one-click install, not from web SEO. citeturn800024view10  
https://github.com/cline/mcp-marketplace

OpenAI exposes another ranking model: its plugin guidelines state that real-world usefulness and user satisfaction can influence enhanced distribution, including directory placement or proactive suggestions. That makes activation and satisfaction more relevant than raw submission volume. **CONFIRMED.** citeturn796574view1  
https://developers.openai.com/plugins/plugin-guidelines?site_locale=en

### Dead, paused, spammed and overcrowded

**Smithery should not be labeled "dead."** Its managed proxy/runtime layer is dead as of October 31, 2026, but the directory remains. Any automation that still expects `server.smithery.ai`, `run.tools`, old API keys, the old CLI/SDK or Smithery-managed identities is obsolete. citeturn800024view4  
https://www.smithery.ai/future

**PulseMCP is not dead either.** Browsing is live, but new submissions and listing changes are paused while ingestion/listing management is reworked. For a new distributor, that means **zero current submission ROI**, even if existing pages still receive visitors. citeturn800024view5turn882906view1  
https://www.pulsemcp.com/ ; https://www.pulsemcp.com/servers

**There is hard evidence of review congestion in at least one skills awesome-list.** `travisvn/awesome-claude-skills` showed **891 open PRs** when checked on November 17, 2026. Its maintainers explicitly say that increasing volumes of newly generated, untested or narrow skills led to a social-proof requirement; submissions under 10 stars are normally closed automatically, and AI-generated/AI-submitted PRs are explicitly disallowed. Calling that list "spam-flooded" would go beyond the maintainer's wording, but calling the submission queue **heavily congested** is directly supported. citeturn566918view1  
https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md

The `hesreallyhim/awesome-claude-code` list similarly added strong filters: a resource must either be at least 14 days old and show subsequent active development, or have at least 100 stars; only one resource may be recommended at a time, and failures are automatically closed. Only collaborators can currently create PRs. These policies are strong evidence that maintainer-review scarcity is now a meaningful constraint. citeturn161972view3  
https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md

By contrast, **there is not enough evidence to call the official MCP Registry, Glama, mcp.so or mcp.directory "spam directories."** Their large entry counts create an **INFERRED visibility problem**, but volume alone is not evidence of spam. Glama specifically invests in quality/security filtering, and mcp.so mixes organic and paid ranking. citeturn866978view0turn800024view6

Two Reddit maintainer threads provide useful but anecdotal qualitative evidence. One maintainer reported that several directories automatically crawled/indexed a server but only Smithery sent noticeable signups; another reported Glama profile views without corresponding npm downloads and characterized several other directory listings as mostly SEO exposure. These posts are **ANECDOTAL, non-representative evidence** and should not be used to estimate conversion rates, but they reinforce the need to instrument your own funnel. citeturn866978view2turn866978view3  
https://www.reddit.com/r/mcp/comments/1w43q35/has_anyone_actually_wired_an_mcp_directory_into/ ; https://www.reddit.com/r/mcp/comments/1u22jwo/lessons_from_shipping_a_hosted_mcp_server_to_all/

## How to get listed, review constraints and automation rules

### MCP servers

**Official MCP Registry.** Publish through the official publisher rather than submitting a README PR. The registry authenticates publishers and associates them with controlled namespaces. GitHub identity can authorize `io.github.<user-or-org>/*`; domain-style namespaces use reverse-DNS ownership verification. A server entry uses `server.json`, while the referenced implementation can live in package registries or behind a remote MCP endpoint. For a two-person open-source lab, GitHub namespace verification is the lowest-friction route. **CONFIRMED.** citeturn800024view0turn800024view1turn800024view2  
https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx ; https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/authentication.mdx ; https://github.com/modelcontextprotocol/registry

The official publisher is automation-friendly: legitimate CLI/CI publishing is part of the design. There is no evidence in the reviewed official materials of a "human-only" submission rule analogous to the Claude skills awesome-list. **CONFIRMED for CLI support; no-ban statement limited to sources reviewed.** citeturn800024view1turn800024view2

**Glama.** The efficient path is *not* to resubmit every official-registry server manually: Glama states that it ingests and republishes the official registry. Publish canonically, then verify the Glama listing and claim it if maintainer controls are useful. Servers benefit from clean licensing, reproducible installation, successful startup/introspection and safe behavior because Glama's quality pipeline actually executes and audits them. Broken builds can reduce or eliminate discoverability. citeturn866978view0turn800024view3  
https://glama.ai/mcp/methodology ; https://glama.ai/mcp/faq

**mcp.directory.** Submit a GitHub repository URL if not already indexed. The site can pull metadata, inspect tools and accept optional npm/PyPI information. If it discovered the server from the official registry, claim rather than duplicate it. The page says submissions are generally reviewed on the order of 24 hours; that is a site claim, not a guaranteed SLA. citeturn800024view9  
https://mcp.directory/submit

**Cline.** Open a submission issue in `cline/mcp-marketplace`, provide the GitHub repository, a square 400×400-or-larger PNG logo and a rationale, and make sure Cline can install it from the repository's README or `llms-install.md`. Review considers adoption, developer credibility, maturity and security; there is no hard minimum-stars rule. The repository says the team aims for review within a couple of days. Bulk bot-generated issues would be strategically unwise, but **no explicit general anti-automation ban was found** in the reviewed primary submission page. citeturn800024view10  
https://github.com/cline/mcp-marketplace

**Docker.** Submit via PR to `docker/mcp-registry`. Docker's recommended path allows Docker to build, sign and publish the server image and attach provenance/SBOM data; approved entries are normally made available through Docker Desktop MCP Toolkit, Docker MCP Catalog and the Docker Hub MCP namespace. This is higher packaging effort than a pure URL directory but also produces a much stronger trust/install artifact. citeturn800024view11turn800024view12  
https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog ; https://github.com/docker/mcp-registry

**GitHub MCP Registry.** GitHub's September 2025 launch post described a curated initial collection and collaboration with the official registry. GitHub's current enterprise documentation focuses heavily on organization/private registry configuration. In the official material reviewed here, a simple current "submit your public MCP here" form for arbitrary independent maintainers was not located. Treat **direct public submission mechanics as UNKNOWN** and use the official MCP Registry as the primary canonical publication route rather than inventing a GitHub-specific path. citeturn800024view13turn800024view14  
https://github.blog/changelog/2025-09-16-github-mcp-registry-the-fastest-way-to-discover-ai-tools/ ; https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-registry

**mcp.so.** Submit MCP servers, remote MCP endpoints and several adjacent artifact classes through its form. The paid $39 route explicitly trades money for immediate publication, verification and priority placement. For a free-open-source lab, use the free route unless an attribution experiment demonstrates that paying produces incremental *activated* users exceeding the opportunity cost. citeturn800024view6  
https://mcp.so/submit

**PulseMCP.** Do not build a submission automation around it today: new-server and listing-change submissions are paused. Monitor the status page/homepage and re-enable the route only after the primary site says ingestion is open again. citeturn800024view5  
https://www.pulsemcp.com/

**Smithery.** Do not build new deployments around the old Smithery proxy, identities, API keys, CLI or SDK. If Smithery remains useful for discovery, the maintained model is the public directory with direct vendor endpoints. citeturn800024view4  
https://www.smithery.ai/future

### Agent Skills and plugins

The base Agent Skills format requires the standardized skill package rather than a proprietary store manifest. `agentskills.io` itself currently refuses community-skill submissions; it accepts ecosystem-listing PRs only for products/platforms that already discover and execute skills. It also has an explicit AI-contribution policy for contributions to the specification repository: AI assistance must be disclosed, and submissions that fail the disclosure policy may be closed. That rule applies to the spec repo, not to every third-party skill marketplace. citeturn566918view0  
https://github.com/agentskills/agentskills/blob/main/CONTRIBUTING.md

**skills.sh.** A major advantage is that a repository can contain many skills. Vercel's documentation explicitly supports:

```text
npx skills add <owner/repo>
npx skills add <owner/repo> --skill <skill-name>
```

It says those skills work across 18+ agents, and the skills.sh leaderboard is based on anonymous install telemetry. This maps unusually well to `tonydzi/join-the-fleet`: you do not need 232 repositories to give 232 skills distinct install identities. citeturn566918view2turn882906view2  
https://vercel.com/docs/agent-resources/skills ; https://www.skills.sh/docs

**Claude official plugin directory.** The correct submission path is `claude.ai/directory/manage`, not a PR to `anthropics/claude-plugins-community`. Approved third-party plugins are processed through Anthropic's review/security pipeline and then mirrored to the public community repository. A plugin can point to a source repository and expose selected skills, which makes thematic bundles practical. citeturn800024view15turn800024view16  
https://github.com/anthropics/claude-plugins-official ; https://github.com/anthropics/claude-plugins-community/blob/main/README.md

**OpenAI ChatGPT/Codex directory.** A plugin can contain skills, MCP connections or both. Submission is a ZIP through the plugin portal with a root `plugin.json`. MCP-containing submissions require substantially more review material: domain verification, reviewer authentication credentials when appropriate, test cases including positive and negative cases, and a walkthrough/review flow. Skills-only plugins avoid some MCP-specific review burden. OpenAI also requires accurate tool-operation annotations and rejects designs that conceal many unrelated operations behind a generic dynamic executor because that prevents the product from understanding the action surface. citeturn796574view0turn796574view1turn796574view2  
https://developers.openai.com/plugins/deploy/submission ; https://developers.openai.com/plugins/plugin-guidelines?site_locale=en ; https://developers.openai.com/plugins

**Cursor.** Official plugins are submitted as Git repositories and every plugin is manually reviewed. Cursor also supports team/enterprise marketplaces, and its documentation points users seeking community plugins/MCP servers to `cursor.directory`. Manual review makes bulk generation of hundreds of near-identical submissions an especially poor strategy. citeturn796574view3  
https://prod.cursor.com/docs/plugins

### Awesome-lists

The current awesome-list environment is explicitly anti-bulk.

`travisvn/awesome-claude-skills` asks for a conventional PR, but its contribution policy says submissions must be valuable, tested and documented, generally require community social proof, and are automatically closed below a basic threshold of **10 GitHub stars** in most cases. Most importantly for automation, it now says the PR must **not be explicitly generated/submitted with AI assistance** and that non-compliant submissions may be closed without comment. It also rejects skills whose primary purpose is funneling users into the submitter's paid SaaS. **CONFIRMED anti-AI/anti-low-effort rule.** citeturn566918view1  
https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md

`hesreallyhim/awesome-claude-code` requires either ≥14 days of project age with evidence of subsequent development or ≥100 stars, permits one resource recommendation at a time, and automatically closes resources that fail the criteria. Only collaborators can currently create PRs. This is another place where a "submit all 55 repos" bot would be structurally incompatible with the maintainer's rules. citeturn161972view3  
https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md

`subinium/awesome-claude-code` uses an even stronger social-proof model in the version reviewed, requiring roughly **1,000 GitHub stars** for listed repositories. That makes it irrelevant for new projects until genuine adoption exists. citeturn796574view6  
https://github.com/subinium/awesome-claude-code/blob/main/README.md

The prominent `wong2/awesome-mcp-servers` README no longer accepts normal listing PRs and routes submissions to mcpservers.org instead. Therefore, submitting to that web directory is effectively the gate for that list, rather than maintaining a second independent PR workflow. citeturn796574view7  
https://github.com/wong2/awesome-mcp-servers/blob/main/README.md

## Practical implications for a free open-source lab

### Skills versus MCP servers

For **MCP servers/tools**, publication should start with the official MCP Registry because it is the canonical layer and provides free downstream propagation into confirmed aggregators such as Glama and mcp.directory. From there, spend manual review effort only on catalogs that place the server inside a client or trusted execution surface: Cline and Docker first, GitHub when a documented inclusion route is available, then secondary SEO directories. citeturn800024view0turn866978view0turn800024view9turn800024view10turn800024view11

For **Agent Skills**, the equivalent canonical distribution primitive is the Git repository plus the open Agent Skills format, not agentskills.io itself. skills.sh gives you GitHub-native installability and measurable anonymous installs; Claude and OpenAI give product-level distribution; Cursor is worthwhile for a smaller number of strong plugins rather than hundreds of tiny entries. citeturn566918view0turn882906view2turn800024view15turn796574view0turn796574view3

### Should `join-the-fleet` become a Claude Code plugin marketplace?

**Yes — but as a set of curated bundles backed by the existing monorepo, not as 232 separately maintained repositories and not only as one undifferentiated 232-skill package.**

The technical reason is that both the Agent Skills tooling and Claude plugin architecture can address multiple skills from one source repository. Vercel's CLI explicitly selects an individual skill from a multi-skill repository; Anthropic's plugin structure can expose selected skill directories. citeturn566918view2turn800024view15

A rational packaging structure is therefore:

`tonydzi/join-the-fleet` remains the **single canonical source of truth**; each skill keeps its own stable folder/name and `SKILL.md`; generated metadata indexes make each skill addressable individually; 5–12 thematic bundles expose curated subsets such as coding/repository work, research, data, operations, writing/document production, web automation, security, MCP management, etc.; and an optional `join-the-fleet-all` bundle exists for advanced users who explicitly want everything.

That recommendation is **INFERRED** from installation/discovery mechanics rather than a published conversion experiment. It minimizes repository maintenance and spam footprint while giving marketplaces semantically coherent entries.

Creating **232 one-skill repositories** would multiply issue triage, Dependabot/security configuration, release management, licensing drift and submission footprint. No reviewed marketplace provides evidence that "one repository per skill" materially improves ranking enough to compensate for those costs. By contrast, skills.sh specifically supports multiple named skills per repository. citeturn566918view2

### What to measure

Do not optimize the distribution routine for "number of catalogs listed." That metric can be made arbitrarily large while delivering no users.

The recommended funnel is:

**listing impression → listing click → install → successful initialize/load → first successful operation → second-session use → GitHub star**

The primary KPI should be **activated unique installations**, defined differently by artifact class:

For an MCP server, activation means a distinct client successfully performs MCP initialization and at least one useful `tools/list`/tool invocation during a defined period. For a skill, activation means installation plus at least one invocation/load event where that signal can be measured without collecting prompts or user content. **This KPI definition is a recommendation, not a marketplace-standard metric.**

Use platform-native metrics where they are close to the funnel: skills.sh installs, Cline installs, Docker pulls, npm/PyPI downloads, mcp.so views and GitHub stars/referrers. Do **not** combine them into one "users" number. skills.sh's installation telemetry and the distinction between Cline installs/Docker pulls/mcp.so views are documented. citeturn882906view2turn866978view1  
https://www.skills.sh/docs ; https://www.hedwigai.com/blog/mcp-registries

For remote MCP servers, first-party logs can measure successful initialization and first tool use. Keep this privacy-preserving: store a coarse source identifier and pseudonymous installation/session ID, not prompts, arguments or user documents unless genuinely required and explicitly disclosed. This is an instrumentation recommendation rather than a catalog rule.

For local MCP packages, attribution is harder. Practical options are source-specific documentation landing URLs, source-tagged installer links where permitted, package-download data and an **explicitly disclosed opt-in** anonymous activation ping. Avoid covert telemetry: it creates trust risk disproportionate to its value for an open-source lab.

### Delisting and spam risks

The concrete failure modes evidenced by current catalog rules are more important than cosmetic SEO:

A broken or non-reproducible MCP server can lose Glama discovery because Glama actually builds and exercises servers. citeturn866978view0

A Claude community plugin submitted by direct GitHub PR is using the wrong door; that repository is a read-only mirror of Anthropic's review pipeline and PRs are not the supported submission mechanism. citeturn800024view16

An OpenAI MCP plugin with inaccurate action annotations, opaque dynamic tool dispatch, unverifiable domains or incomplete review cases can fail review because the directory requires the product to understand and verify the action surface. citeturn796574view0turn796574view1

An awesome-list submission can be automatically closed for insufficient age/social proof, exceeding one-at-a-time rules or AI-generated submission behavior. citeturn566918view1turn161972view3

A Cline submission whose README does not allow reliable installation or whose project looks immature/insecure can fail manual marketplace review. citeturn800024view10

An official MCP Registry publication using a namespace you cannot authenticate is structurally invalid; use the GitHub-controlled namespace or a domain you can prove you control. citeturn800024view1

The broader anti-spam principle is therefore: **automate metadata generation and validation; do not automate human-review channels as if they were APIs.** The official registry/packaging pipelines are appropriate for CI automation; curated awesome-list PRs and marketplace reviews should remain human-approved.

## Recommendations for a free open-source distributor

The ranking below weights: direct proximity to an agent/client, measurable installation/use, downstream propagation, submission friction, no mandatory payment, trust/security value and expected maintenance burden. The rank is therefore **INFERRED**, not a published market share ranking.

| Rank | Venue | Artifact | Reach per hour | Why it deserves effort | Recommended action |
|---|---|---|---|---|---|
| **1** | Official MCP Registry | MCP | **Very high** | Canonical publication plus confirmed downstream ingestion by Glama and mcp.directory; automatable | Publish every production-ready MCP with verified `io.github.tonydzi` namespace and clean `server.json` |
| **2** | skills.sh | Skills | **Very high** | GitHub-native; multi-skill repos supported; install telemetry gives an actual adoption signal | Make all high-quality skills installable by stable name from `join-the-fleet`; expose thematic packs |
| **3** | Glama | MCP | **Very high after registry publication** | Official-registry mirror means marginal listing cost is near zero; quality/security scans create trust signal | Let import happen, claim listings, fix scan/build issues; do not duplicate-submit unnecessarily |
| **4** | Claude plugin directory | Skills / plugins | **High** | Direct distribution into Claude products; supports skill bundles; official review adds trust | Publish 5–12 coherent thematic `join-the-fleet` plugins rather than 232 one-skill plugins |
| **5** | Cline MCP Marketplace | MCP | **High for good servers** | Inside-client one-click installation and visible install counts | Manually submit only mature MCP servers with excellent install docs/icon/security posture |
| **6** | OpenAI ChatGPT/Codex plugin directory | Skills + MCP | **High, higher review cost** | Direct product discovery; supports skill-only and MCP+skill plugins | Submit the strongest thematic skill bundles first; submit MCP integrations only when review/test burden is justified |
| **7** | Docker MCP Catalog | MCP | **Medium-high, higher packaging cost** | Curated runtime, signed images/SBOM/provenance, Docker pull signal | Containerize mature local servers such as `sqlite-graph-memory`; submit only those that benefit from container delivery |
| **8** | mcp.directory | MCP | **High per unit effort** | Official-registry auto-discovery; little incremental work | Claim and correct imported entries; no repetitive manual publishing if already imported |
| **9** | mcp.so | MCP + Skills | **Medium** | Broad multi-category reach and meaningful site traffic claims, but commercial placement affects ranking | Use free listings; run a controlled experiment before ever spending $39 or buying ads |
| **10** | Selective awesome-lists | Skills/MCP | **Low-to-medium initially; potentially high after social proof** | Large GitHub audiences, but queues and anti-spam rules are real | Submit only proven top artifacts, one at a time, manually, after thresholds are met |

Evidence for this ranking comes from the official-registry propagation mechanics, skills.sh install telemetry, client-integrated Cline/Docker mechanics, Claude/OpenAI review/distribution, and the documented submission congestion/gating of awesome-lists. citeturn866978view0turn800024view9turn882906view2turn800024view10turn800024view11turn800024view15turn796574view0turn566918view1  
Sources: https://glama.ai/mcp/methodology ; https://mcp.directory/submit ; https://www.skills.sh/docs ; https://github.com/cline/mcp-marketplace ; https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog ; https://github.com/anthropics/claude-plugins-official ; https://developers.openai.com/plugins/deploy/submission ; https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md

**PulseMCP should currently sit outside the active submission queue** because submissions are paused. **Smithery should be treated as discovery-only unless its post-Arcade architecture changes again**, because the former proxy/runtime distribution layer ended October 31, 2026. **GitHub MCP Registry should be monitored but not given a bespoke submission workflow until GitHub documents a clear public self-submission route.** citeturn800024view5turn800024view4turn800024view13  
Sources: https://www.pulsemcp.com/ ; https://www.smithery.ai/future ; https://github.blog/changelog/2025-09-16-github-mcp-registry-the-fastest-way-to-discover-ai-tools/

### Recurring routine: discover venues

Maintain a machine-readable ledger with one row per catalog and fields for artifact class, canonical URL, submission door, auto-import source, fee, paid-ranking flag, last successful submission, last status check, review SLA if publicly stated, traffic metric type, attribution support and owner.

Run discovery on a regular cadence against terms such as `MCP registry`, `MCP marketplace`, `MCP directory`, `Agent Skills marketplace`, `SKILL.md directory`, `Claude plugins`, `Codex plugin directory`, `awesome MCP`, and `awesome agent skills`, but qualify a venue before adding it to the active queue.

A simple internal score is useful:

```text
venue_score =
  4 * direct_client_integration
+ 3 * measurable_installs_or_use
+ 3 * automatic_downstream_distribution
+ 2 * trust_or_security_signal
+ 2 * relevant_real_traffic
- 2 * manual_submission_hours
- 2 * paid_rank_dependency
- 3 * paused_or_stale
- 3 * spam_or_review_congestion
```

The weights are an **internal decision heuristic**, not empirical market coefficients.

Re-check status before each submission cycle. This matters because the 2026 examples show that venue mechanics can change quickly: Smithery retired its runtime layer, PulseMCP paused submissions and awesome-list maintainers tightened contribution rules. citeturn800024view4turn800024view5turn566918view1

### Recurring routine: package artifacts

Use **one canonical metadata source in each repository** and generate venue-specific adapters from it rather than hand-maintaining 12 inconsistent descriptions.

For an MCP server, the release gate should include: valid official `server.json`; authenticated namespace; reproducible install; SPDX-compatible license declaration; clear remote/local transport; deterministic `initialize` and `tools/list`; explicit tool descriptions and side-effect semantics; no embedded secrets; package metadata for npm/PyPI if applicable; Dockerfile only when container distribution is useful; square marketplace icon; concise README; and an agent-oriented installation section such as `llms-install.md` where a client expects it. Official registry authentication, Cline's installation requirements and Docker's image/provenance model support this approach. citeturn800024view1turn800024view10turn800024view12

For a skill, the release gate should include: valid `SKILL.md`; stable unique name; precise description telling the agent **when** to invoke it; license; dependencies; bounded instructions; examples/tests where meaningful; scripts/resources separated cleanly; no hidden network calls; and stable folder paths so external plugin manifests do not break. The Agent Skills repository is the standards reference, while skills.sh and Vercel documentation demonstrate repository-level and individual-skill installation. citeturn800024view19turn566918view2  
Sources: https://github.com/agentskills/agentskills ; https://vercel.com/docs/agent-resources/skills

For `join-the-fleet`, generate a catalog manifest containing at minimum:

```yaml
skills:
  - id: sqlite-investigator
    path: skills/sqlite-investigator
    categories: [data, sqlite]
    maturity: stable
    tested_agents: [claude-code, codex, cursor]
    license: MIT
    bundle: data-tools
```

Then derive `skills.sh` install examples, Claude plugin skill arrays, OpenAI plugin bundles, README tables and website/catalog metadata from that single inventory.

### Recurring routine: distribute and measure

The optimal order is **canonical first, mirrors second, client catalogs third, SEO/community directories fourth**.

For MCP: release GitHub/package → publish official registry → verify Glama/mcp.directory imports → fix quality scans → submit mature servers to Cline/Docker → optionally list on mcp.so and other free directories → submit to an awesome-list only after real adoption/social proof.

For skills: release canonical skill folders in `join-the-fleet` → verify `npx skills add` installation → make high-value skills visible through skills.sh → package thematic Claude/OpenAI/Cursor plugins → use awesome-lists only for the strongest individual skills after their required social proof exists. The current awesome-claude-skills rule alone makes a 232-PR blast both inefficient and non-compliant. citeturn566918view1

After each distribution cycle, record:

```text
venue
artifact
published_at
listing_url
listing_views
repo_clicks
installs_or_pulls
successful_activations
7d_repeat_users
stars_delta
issues_from_new_users
hours_spent
cash_spent
```

Then optimize for:

```text
activated_users_per_hour =
    successful_activations_attributed_to_venue
    / maintainer_hours_spent_on_venue
```

and, for paid experiments:

```text
cost_per_activated_user =
    venue_cash_spend
    / incremental_successful_activations
```

A paid mcp.so listing should therefore be judged by **incremental activated users versus the free baseline**, not by its verified badge or page-view uplift. Its commercial priority placement is confirmed; whether that placement produces useful users for `tonydzi` is **UNKNOWN until measured experimentally.** citeturn800024view6  
https://mcp.so/submit

The most important operational conclusion is that the catalog market is splitting in two. **Generic web directories are becoming SEO/advertising businesses or giant mirrors, while the higher-value layer is moving inside agent clients, runtime environments and governed registries.** For a two-person lab giving everything away, the rational strategy is not "submit everywhere." It is to publish canonically where one action propagates, invest manual effort where users can install directly, and reserve human-curated lists for artifacts that have already earned social proof. This conclusion is **INFERRED from the distribution, ranking and monetization mechanics documented above**, rather than a claimed universal conversion law.

## Sources (safe_urls + citations)
- https://developers.openai.com/plugins
- https://developers.openai.com/plugins/deploy/submission
- https://developers.openai.com/plugins/plugin-guidelines?site_locale=en
- https://docs.docker.com/ai/mcp-catalog-and-toolkit/catalog
- https://docs.github.com/en/apps/github-marketplace/creating-apps-for-github-marketplace/requirements-for-listing-an-app
- https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-registry
- https://docs.github.com/en/site-policy/github-terms/github-marketplace-developer-agreement
- https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages/
- https://docs.pypi.org/publishing/publishing-to-pypi/
- https://github.blog/changelog/2025-09-16-github-mcp-registry-the-fastest-way-to-discover-ai-tools/
- https://github.com/agentskills/agentskills
- https://github.com/agentskills/agentskills/blob/main/CONTRIBUTING.md
- https://github.com/anthropics/claude-plugins-community/blob/main/README.md
- https://github.com/anthropics/claude-plugins-official
- https://github.com/cline/mcp-marketplace
- https://github.com/docker/mcp-registry
- https://github.com/hesreallyhim/awesome-claude-code
- https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md
- https://github.com/modelcontextprotocol/registry
- https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/about.mdx
- https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/authentication.mdx
- https://github.com/subinium/awesome-claude-code/blob/main/README.md
- https://github.com/travisvn/awesome-claude-skills
- https://github.com/travisvn/awesome-claude-skills/blob/main/CONTRIBUTING.md
- https://github.com/wong2/awesome-mcp-servers/blob/main/README.md
- https://glama.ai/mcp/faq
- https://glama.ai/mcp/methodology
- https://mcp.directory/submit
- https://mcp.so/advertise
- https://mcp.so/submit
- https://prod.cursor.com/docs/plugins
- https://vercel.com/docs/agent-resources/skills
- https://www.agentlaun.ch/directories/mcp-registries/
- https://www.agentlaun.ch/directories/mcp-so/
- https://www.anthropic.com/news/donating-agent-skills-to-the-agentic-ai-foundation
- https://www.arcade.dev/blog/smithery-joins-arcade/
- https://www.hedwigai.com/blog/mcp-registries
- https://www.pulsemcp.com/
- https://www.pulsemcp.com/servers
- https://www.reddit.com/r/mcp/comments/1u22jwo/lessons_from_shipping_a_hosted_mcp_server_to_all/
- https://www.reddit.com/r/mcp/comments/1w43q35/has_anyone_actually_wired_an_mcp_directory_into/
- https://www.skills.sh/docs
- https://www.smithery.ai/future