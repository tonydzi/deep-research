# DR26-08-29-ZB-01-0856 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Executive Summary

The objection “corporates have no reason to buy a second brain” mixes three different things and is only true of one of them.

1. The consumer category name “second brain” (Tiago Forte / PARA / Zettelkasten / personal Obsidian vaults) does not exist as an enterprise budget line. Analysts do not use the phrase. Procurement does not buy it. That part of the objection is [established].

2. The job-to-be-done — a company-wide knowledge layer that employees and machines can query, and increasingly write back to — is real, funded, and already occupied. It is sold as enterprise search, insight engines, knowledge bases, digital workplace AI, CX knowledge (KCS), vertical “institutional memory” (legal, finance, sales conversation intelligence), and, in the last 12–18 months, as agent memory / grounding. Buyers pay. Glean, Coveo, Amazon Q Business, Atlassian Rovo, Writer, Hebbia, Harvey, AlphaSense, Gong, Guru, ServiceNow Knowledge, Salesforce Knowledge, and the Microsoft/Google incumbents are the proof. That part of the objection is [refuted].

3. Write-side KM (humans must file, tag, and maintain a shared brain) has a 30-year failure record. Read-side KM (search/RAG over the exhaust that already exists in Slack, Drive, email, tickets, SharePoint) is what finally found ROI after LLMs. Agent-write (machines updating memory) is the new attempt to make the write-side work without human filing labor. Your artifact — a multi-employee shared vault that agents read and write — is this third shape. It is not what Glean sells, and it is not what “second brain” trainings sell. It is closer to Dust + Atlassian Rovo + emerging agent-memory infra than to BASB. Whether corporates will buy that exact shape from a 3-person lab is a go-to-market question, not a demand question.

Demand is sequenced, not absent. Typical white-collar sequence in 2024–2026: buy Copilot/Gemini seats → discover the model is only as good as retrieval and permissions → buy or build a knowledge/search layer. Typical support sequence (older): knowledge base first, then deflection bot. Automation (RPA, workflow agents, function-specific copilots) currently takes the majority of identifiable “AI project” budget; knowledge infrastructure is smaller, often hidden inside a Microsoft/ServiceNow/Salesforce SKU, and only becomes a standalone check when Copilot is already disappointing or when knowledge is the inventory (law, consulting, pharma, support).

For a 3-person lab with a working multi-machine vault+agent fleet: do not try to sell “a second brain” to Fortune 500 IT. The name is poison, the incumbent stack is Microsoft/Google/Atlassian/ServiceNow, and Glean-class search is a sales-and-connectors war you will lose. The live opening is narrower: 20–200 person knowledge-work firms where knowledge is the product, sold as “organizational memory that agents run on,” or a vertical wedge (support KCS+agents, professional services, or agent-memory infra for teams already on Obsidian/Notion). Consulting-only is a business, not a company anyone will value at SaaS multiples.

B. Verdict on the objection

Verdict: true-only-in-a-narrow-sense. [established] for the name and for write-side PKM; [refuted] for the underlying job-to-be-done; [emerging] for the agent-operated shared brain as a purchasable category.

B1. Why enterprises historically under-invested in KM

Four structural reasons, all still active.

No natural budget owner. Chief Knowledge Officer titles peaked in the late 1990s–mid 2000s and then mostly vanished outside professional services, pharma, and some governments. Knowledge was everyone’s problem and nobody’s P&L. IT bought a repository (Lotus Notes, then SharePoint, then Confluence) and declared KM done. HR owned “learning.” Support owned the help center. Internal comms owned the intranet. Legal owned records. The “company brain” sat in the gaps. This is the strongest steelman of your contact’s objection: corporates don’t buy second brains because no one has the mandate to. [established] in KM practice literature; the CKO disappearance is treated as common knowledge in APQC/KMWorld circles (APQC KM practice area: https://www.apqc.org/expertise/knowledge-management — practice page, ongoing; KMWorld: https://www.kmworld.com).

Contribution is unpaid labor. Classic KM asked busy people to stop working and write to a wiki. McDermott’s 1999 paper stated the IT-cannot-deliver-KM point before SharePoint existed: communities share knowledge when it helps their work, not when a portal is installed. (Richard McDermott, “Why Information Technology Inspired but Cannot Deliver Knowledge Management,” California Management Review, 1999 — https://journals.sagepub.com/doi/10.2307/41166012). Davenport & Prusak, Working Knowledge (1998, HBR Press) made the same argument. [established], but sources are 25+ years old; the mechanism has not expired.

Tacit vs explicit. Nonaka’s SECI model (The Knowledge-Creating Company, 1995) is still the theory KM consultants sell. A vault captures explicit artifacts. Most valuable corporate knowledge is tacit: judgment, who-knows-what, political context, “how we actually do this.” Dave Snowden / Cognitive Edge spent a career arguing that treating a company as a document store is a category error (Cynefin, SenseMaker; https://thecynefin.co). Steelman of the objection from inside the KM profession, not from a skeptical sales lead: a “second brain for the company” repeats the 1990s mistake. [established] as a school of thought; [contested] as an absolute — retrieval over Slack/email/tickets does capture some previously unsearchable tacit residue.

Search was bad, so the real system was people. If intranet search failed once, users never returned. Colleague-asking, Slack, and hallway networks became the KM system. Stale pages then made the official brain less trustworthy (wiki rot). SharePoint “graveyards” are not a meme; they are the default state of unmanaged content. AIIM and various analyst notes on “content chaos” / “dark data” have repeated this for 15 years. Treat vendor stats on “X% of SharePoint content never accessed” as [low-authority] directional; the qualitative pattern is [established].

KM winters, compressed:

- 1990s: first wave (Notes, Nonaka, CKO fashion). Collapsed into portal projects.
- 2000s: SharePoint as default, after-dotcom cost cutting killed many KM teams.
- 2007–2015: social intranet / wiki boom (Jive, Yammer, Confluence, Google+). Collaboration vendors promised knowledge would “emerge.” Jive was sold off and faded; Google+ / Currents died; Yammer was swallowed into Microsoft 365 and stopped being a KM strategy. [established]
- 2016–2022: “digital workplace” and Microsoft Viva / Project Cortex. Knowledge graph auto-topics were supposed to end filing labor. They did not (see Viva Topics in section F).
- 2023–: LLM retrieval. First time read-side KM had a product people actually opened the next day, because the interface became a question box rather than a site tree.

B2. What LLMs changed — ROI evidence, both directions

What changed (mechanism, [established]): you no longer need employees to file knowledge for the system to answer questions. RAG/search over existing exhaust (mail, chat, docs, tickets, CRM) turns the company into a corpus. That attacks the contribution failure mode. Permissions/ACL, connector coverage, and hallucination are the new failure modes.

Evidence it delivers ROI (named, measured — quality varies):

- Commissioned Forrester TEI studies for Glean, Coveo, and Microsoft 365 Copilot report hours saved per week and seven-figure NPV. These are vendor-paid. Tag [low-authority] as evidence of truth, [established] as evidence that a sales motion with ROI theater exists and closes. Glean customer marketing names Databricks, Canva, Duolingo, Grammarly among others (https://www.glean.com/customers — marketing page, dates vary). [low-authority]
- Support deflection is the cleanest measured ROI in the KM family: a maintained knowledge base plus a bot reduces ticket volume. This is KCS + bot, not “second brain,” but it is the same asset class. Zendesk, Intercom Fin, Ada, Ultimate, ServiceNow publish deflection rates. Treat vendor percentages as [low-authority]; the existence of a budgeted, measured loop is [established].
- Conversation intelligence (Gong, etc.) is organizational memory attached to revenue. It sold when horizontal wikis did not. Gong’s scale is the existence proof that “we remember what happened in the company” sells if it sits on a quota-carrying workflow. [established] as demand; current valuation/ARR figures move too fast to state without a 2026 filing [no source found in this pass for a 2026 audited number].
- Professional services and law: knowledge is inventory. iManage, Relativity, Harvey, internal McKinsey/BCG knowledge systems get real spend. Harvey’s enterprise legal push is the 2023–2026 version (company site https://www.harvey.ai). Precise ARR/valuation in 2026: user’s prior notes may be fresher than this pass; I will not invent a 2026 number.

Evidence it still fails:

- Copilot “pilot purgatory” and low seat utilization were widely reported in 2024–2025 (business press + CIOs in Gartner/Forrester inquiry notes). The consistent complaint: the model cannot find the right document, or finds the wrong one, or is blocked by permissions, or hallucinates policy. That is a knowledge-layer failure, not a model-IQ failure. [emerging] as a quantified industry-wide utilization series — public, vendor-independent utilization datasets are thin; [established] as a qualitative pattern.
- Garbage-in remains. If SharePoint is a graveyard, RAG makes the graveyard fluent. Several post-mortems of internal GPT projects in 2023–2024 died on data quality, not on the demo. [established] mechanism; most write-ups are blogs/[low-authority].
- Write-side still fails. Employees still do not maintain wikis. LLM summarizers that auto-generate pages create a new rot: plausible, outdated, unowned documents. Viva Topics’ retirement is the flagship (section F).
- Security and hallucination keep some regulated buyers on the sideline or in “private mode.” [established] as a procurement blocker.

Disagreement to preserve: vendors say “AI finally makes KM work.” Snowden-style KM people say “you made a better intranet search; you did not capture organizational knowledge.” Both can be true. Search ROI can be real and still not be a “second brain.”

B3. Is “second brain” rejected, or renamed?

Rejected as a name. Renamed as a job.

Vocabulary that has (or sits next to) budget, with who uses it:

- Enterprise search / insight engines / cognitive search — Gartner historically “Insight Engines”; Forrester “Cognitive Search.” This is the budgeted horizontal category Glean occupies. [established]
- Knowledge management — still alive in customer service / KCS / ITSM (ServiceNow, Salesforce Knowledge, Zendesk Guide). Gartner/Forrester still publish KM material here. Not used for white-collar PKM. [established]
- Digital workplace / employee experience / intranet — where Viva, SharePoint, Workplace, Intranet-in-a-Box live. Budget often IT + internal comms + HR. [established]
- Knowledge graph / RAG / grounding — technology terms, not budget lines. Appear inside AI platform spend. [established]
- AI workspace / AI operating system / teammates / agents — 2024–2026 vendor language (Dust, Atlassian Rovo, Salesforce Agentforce, Google Agentspace, Microsoft Copilot Studio). Budget is “AI program” or platform SKU, not KM. [emerging]
- Institutional memory / organizational memory / agent memory / context graph — 2025–2026 startup and investor language. Not yet a Gartner MQ. [emerging]
- Second brain / PKM / Zettelkasten / PARA / CODE — consumer/prosumer. Forte Labs, YouTube, Notion templates. Gartner/Forrester do not sell research under this name. [established]

What has budget attached: Microsoft 365 Copilot ($30/user/month list, https://www.microsoft.com/en-us/microsoft-365/enterprise/copilot — list price page; launched as add-on 2023, still the default CIO comparison in 2025–2026), Google Gemini for Workspace add-ons, Amazon Q Business, ServiceNow/Salesforce AI SKUs, a minority standalone “enterprise AI search” RFP, CX knowledge bases, and vertical research tools (AlphaSense, Harvey). “Second brain” has a credit-card budget, not a CIO budget.

C. Competitive map (sellers of corporate second brains)

How to read this map. Four shapes, not one market:

Shape 1 — Read-side: search/RAG over existing silos (no new vault).
Shape 2 — Write-side knowledge base (humans still file): Guru, Confluence, Bloomfire, Slite.
Shape 3 — Shared workspace + assistants that use workspace as memory: Notion, Dust, Atlassian Rovo, Microsoft Copilot+SharePoint.
Shape 4 — Agent memory infrastructure: machines read/write memory (Mem0, Zep, Letta). This is your shape, mostly sold to developers today, not CIOs.

Your prior Aug 2026 figures for Glean (~$7.2B, ~$300M ARR, ~100-seat floor), Dust (3000+ orgs, $40M Series B), and Sana (Workday, $1.1B) are taken as given and not re-litigated.

C1. Incumbent platforms (where the money already sits)

Microsoft 365 Copilot + SharePoint + Graph + Copilot Studio + SharePoint agents
Target: every M365 estate. Pricing: Copilot list $30/user/month [established]; Copilot Studio and SharePoint agents packaged inside Microsoft 365 SKUs / credits — exact 2026 credit prices move, check current Microsoft list rather than a snapshot. Traction: tens of millions of Copilot seats is the distribution fact your prior research already has; utilization ≠ seats. This is the default “company brain” whether or not it works. Microsoft already tried the explicit knowledge layer (Project Cortex 2019 → Viva Topics 2021) and folded it into Copilot (see F). [established]

Google Gemini for Workspace + Vertex AI Search + Agentspace
Target: Google Workspace estates and GCP-centric enterprises. Gemini add-on pricing has been in the same band as Microsoft (roughly $20–$30/user/month class); confirm current SKU. Agentspace (Google Cloud Next 2025) is Google’s bid for “agents over company knowledge” (https://cloud.google.com — product pages; announcement window April 2025). [emerging] as a packaged org-brain; [established] as Google’s strategic intent.

Amazon Q Business (Kendra lineage)
Target: AWS-centric IT, especially if corpus is already in AWS. List (2024 launch band): Lite on the order of a few dollars per user/month, Pro ~$20/user/month (AWS pricing: https://aws.amazon.com/q/business/pricing/ — verify current). This is a genuine knowledge-layer SKU with a price list, unlike “second brain.” [established] product; [single-source on exact 2026 list] unless re-fetched.

Atlassian Rovo (search + agents over Jira/Confluence/Bitbucket + connectors)
Target: the huge Confluence/Jira installed base — i.e. companies that already have a failed-or-partial wiki. This is one of the closest products to “org brain on top of the wiki we already have.” Rovo announced 2024, pushed through 2025 as Atlassian’s AI teammate (https://www.atlassian.com/software/rovo). Packaging (included vs add-on) changed as Atlassian bundled AI into Premium/Enterprise; treat a specific 2026 per-user price as [needs live check]. Traction: distribution via Confluence is the moat, not model quality. [established] product; [emerging] as a true agent-operated brain.

Salesforce Knowledge + Einstein/Agentforce + Data Cloud
Target: CX and sales orgs. Knowledge is a first-class object because it feeds cases and now agents. Pricing is platform-plus-SKU, typically six-to-seven figures all-in for real deployments. Agentforce (2024–2025) is the “agents need a knowledge library” sales motion. [established]

ServiceNow Knowledge + Now Assist + AI Agents
Target: ITSM/HRSD/customer workflows. ServiceNow has been a KM vendor for a decade (knowledge articles, KCS-ish processes) and is now wrapping agents around that store. Moveworks (enterprise knowledge assistant / IT support agent) was acquired by ServiceNow; press at the time put the deal around $2.85B (2025). Confirm on ServiceNow IR (https://www.servicenow.com/company/media.html) before using the number in a deck — [single-source / press]. This deal is important: it priced an “ask the company / ask IT” brain as a product company, not as consulting.

OpenAI ChatGPT Enterprise / Codex + connectors; Anthropic Claude for Work + connectors; Perplexity Enterprise
Target: “put a model on our files.” This is often the first purchase, not a knowledge architecture. Memory/Projects/Connectors are the 2025–2026 slide toward a brain. They do not replace permissions-aware enterprise search. [established] as spend; [emerging] as KM.

C2. Enterprise search / RAG platforms (Shape 1)

Glean — taken as given from your Aug 2026 work. How they sell: connectors + identity-aware search + assistant +, more recently, agents. Not a vault. They explicitly do not ask you to move knowledge into Glean. That is the anti-second-brain architecture, and it is the one that won horizontal enterprise. Pricing: custom, seat-based, floor around 100 seats, ACV typically mid-six to seven figures for real deployments [emerging as a band; your $300M ARR implies this]. [established] category leader among independents.

Coveo (public, TSX:CVO / also listed historically in the US via filings)
Target: enterprise search + commerce + service. Pricing: custom, often $100k+ ACV. Filings are the rare audited window into “search as SaaS” (https://ir.coveo.com). Growth has been real but not Glean-mythic; public multiples are a better compass for a search business than private headlines. [established]

Elastic (NYSE:ESTC) — search + observability. Workplace knowledge is a use case, not the whole company. Pricing: consumption + SKU. [established] infra, not a “second brain” brand.

Azure AI Search; Google Vertex AI Search; open-source + cloud vector DBs (Pinecone, Weaviate, TurboPuffer, pgvector)
Target: builders. This is how a lot of “company GPT” projects were assembled in 2023–2024. Many died in production on connectors and ACL. [established]

Sinequa — French insight engine, long-time Gartner Insight Engines vendor, heavy in regulated/defense/pharma. ChapsVision announced acquisition of Sinequa (2024). Enterprise, custom, long sales cycle. [established] niche; [single-source] on deal terms unless you pull the ChapsVision/Sinequa release (https://www.sinequa.com).

Lucidworks — Fusion/platform search, commerce + workplace. Private, multiple recaps over the years. Custom pricing. [established] survivor, not a 2026 hype name.

Yext — public, more listings/search experience than internal brain. A cautionary public comp: knowledge-graph story, messy equity story. [established] as “do not use as a success analog.”

Unleash (unleash.so) — mid-market workplace search, often pitched as “Glean for companies that cannot pay Glean.” [low-authority] traction claims from their site https://www.unleash.so. Pricing: SaaS per-seat, materially below Glean. [emerging]

GoSearch — similar “Glean alternative” wedge, cheaper, faster to deploy. [low-authority] marketing traction. [emerging]

Dropbox Dash — universal search across Dropbox + connectors (https://www.dropbox.com/dash, launched 2024). Target: existing Dropbox and cross-app search. Pricing inside Dropbox SKUs. Traction = Dropbox distribution, not a new category win. [established] product launch; [emerging] as enterprise brain.

Hebbia — document intelligence for finance/legal (“Matrix”). Raised a large Series B in 2024 (press: ~$130M, valuation in the $700M band at the time — TechCrunch/Bloomberg contemporaneous; treat 2026 valuation as [needs live check]). This is not a company wiki; it is “institutional memory over deal documents.” [established] as a funded vertical brain.

Writer — enterprise generative AI platform with a company knowledge graph / knowledge layer as the differentiator versus raw Copilot. Series C in 2024 was reported around $200M at ~$1.9B (https://writer.com; contemporaneous press). Sells to marketing+legal+ops in large enterprises, ACV custom, typically well above mid-market wiki tools. Closest “we are your company’s AI with a grounded knowledge layer” narrative after Glean/Microsoft. [established] as a vendor; [single-source] on last round terms unless re-confirmed.

Contextual AI, Vectara — RAG platforms for enterprises that want to build, not buy Glean. Developer/CDO buyer. [emerging]

Palantir AIP — not sold as KM, but in practice a governed knowledge+decision layer for operators. Pricing and ACV are Palantir-class (high, services-heavy). Relevant only for the “enterprise will pay for a system of record for decisions” steelman. [established] as spend; [fringe] as a second-brain comparable.

AlphaSense (plus Tegus) — research/expert-call memory for finance and strategy. This is a purchased institutional memory with a real budget (research, not IT). Useful comparable for “when knowledge is the product, they buy.” Funding/valuation has been in the multi-billion private-company band in 2024–2025 press; do not nail a 2026 number here. https://www.alpha-sense.com [established] demand; [emerging] exact last valuation.

C3. Knowledge bases / team wikis (Shape 2 — write-side)

Guru (getguru.com) — “knowledge where you work” (browser extension + Slack + now AI agents). Target: sales, success, support enablement, mid-market to enterprise. Pricing historically in the ~$10–$25/user/month band by tier (verify current: https://www.getguru.com/pricing). Funding: well-known BVP-backed company; last large round was early-2020s, not a 2025 mega-round [stale — do not quote a 2026 valuation]. Traction: one of the few independent KBs that survived Slack/Notion. Closest classic “company brain” SaaS that is not a search overlay. Still depends on humans verifying cards — the contribution problem, mitigated by “knowledge in the workflow.” [established] product; [emerging] whether Guru’s agent layer changes the contribution economics.

Bloomfire — knowledge engagement, often customer-facing or franchise/ops knowledge. Enterprise sales, annual contracts, starting ACV typically tens of thousands, not per-user self-serve. https://bloomfire.com [low-authority] on current ARR. [established] as a surviving KM vendor.

Slite — European team knowledge base, cleaner Notion-competitor for docs. SMB/mid-market, per-seat (historically single-digit to low-teens $/user/month + AI). https://slite.com [established] small; not an enterprise brain.

Slab — engineering-adjacent knowledge base, YC, per-seat low-cost. https://slab.com. [established] niche.

Tettra — Slack-native wiki, SMB. https://tettra.com. [established] small.

Nuclino, Helpjuice, Document360, Guru-class help-center tools — SMB/mid knowledge bases, often support-documentation not company memory. Pricing self-serve to mid-five figures. [established] as a commodity.

Confluence — the default write-side brain for tech companies, and the default wiki-rot case study. Atlassian’s bet is Rovo, not a cleaner wiki. [established]

Notion AI + Notion Enterprise Search + Notion agents (2024–2026)
Target: product/tech/startups, now pushing up-market. Pricing: Notion Business/Enterprise + AI add-on (AI has been in the ~$8–$10/user/month class when sold as add-on; bundling changed — verify). Notion is the closest mass-market “shared second brain” in actual use, especially at 20–500 person companies. It is still a wiki with an LLM, not a multi-machine agent fleet. GTM is product-led, not Glean-like. [established] as the de facto SMB/mid shared brain; [emerging] as a true agent platform.

Stack Overflow for Teams / OverflowAI — engineering knowledge. After Prosus acquired Stack Overflow (2021, $1.8B — contemporaneous). OverflowAI reception was mixed; engineering knowledge often lives in the repo + Slack anyway. [established] asset, mixed AI execution.

Starmind (starmind.ai) — Swiss, “who knows what” expert finding rather than a document vault. Target: industrials, large enterprises (Siemens has been a publicly mentioned type of customer in their marketing). Enterprise pricing, custom. This is one of the few products aimed at tacit routing (find the expert) rather than document RAG. [established] niche; [low-authority] on current ARR.

Twine — name collision risk (several products). The meeting/knowledge “Twine” is not a Glean-scale vendor. Do not treat as a primary competitor without a live product check. [no source found] for a 2025–2026 enterprise-scale traction number worth using.

Qatalog — unified work search. Wound down / shut product (2023–2024 era). Failure, not a 2026 competitor. [established] as a shutdown in the category; exact last-day post [single-source] via their blog/Twitter at the time.

C4. Personal/team memory products (usually not what CIOs buy)

Limitless (formerly Rewind) — personal capture (meetings, screen, audio). Team/org features have been the question since the Rewind days. Buyer is individual or small team, not enterprise KM. https://www.limitless.ai [established] as personal memory; [speculative] as corporate second brain.

Mem.ai — personal/team notes + AI. Multiple repositionings. Not an enterprise KM winner. [established] as a struggling category example.

Heyday — conversation memory; LinkedIn acquired Heyday in 2022 (LinkedIn blog, 2022). Product largely disappeared into LinkedIn. Lesson: big platforms buy memory features and bury them. [established] acquisition; [emerging/speculative] on how much of the product survived.

Microsoft Recall — consumer OS memory, privacy backlash 2024. Not an enterprise KM product. [established] as a warning about capture-everything.

Personal.ai, Rewind-class clones, meeting notetakers (Otter, Fireflies, Fathom, Granola) — they create transcripts, which can become a corpus. Meeting memory is a wedge into org memory. Granola specifically is a “meeting layer” not a company vault. [established] as a wedge; [speculative] that notetakers become the system of record.

C5. The exact shape you care about: multi-employee shared space + agents that read AND write

Who actually sells this, not just search:

Dust.tt — closest independent. Shared workspace, connected data, custom GPTs/agents/workflows that operate on company knowledge. Developer-friendly, moving down-market (your prior research). This is “AI OS,” not “second brain,” which is why it sells. https://dust.tt Pricing: seat + usage (model costs). Traction: thousands of orgs as you already have. [established] as the independent to study.

Atlassian Rovo agents + Confluence — shared wiki as the writable memory, agents as operators. Distribution monster. [established] product direction.

Microsoft SharePoint agents + Copilot Studio + Graph — the incumbent version of “agents on the company corpus,” with Copilot Pages / Loop as the writable surface. [established] direction, uneven adoption.

Salesforce Agentforce grounded on Knowledge/Data Cloud — writable (cases, knowledge articles) in an object model, not a markdown vault. [established]

Notion agents (2025–2026 push) — agents that work inside the workspace database. Closest mass UX to “the wiki is the brain and the agent lives in it.” [emerging]

Glean Agents — Glean extending from search into doing. Still primarily read/act, not “the vault is the source of truth you maintain.” [emerging]

Writer knowledge graph + applications — governed enterprise content, more marketing/comms than engineering vault. [emerging] as agent-operated.

n8n / Zapier Agents / Lindy / Gumloop / Relevance AI — agents with some memory, but memory is a side effect of workflows, not a designed institutional brain. These steal budget from “AI automation,” not from KM. [established] as the automation alternative.

Nobody of scale is selling “Obsidian-style networked vault + fleet of Claude machines for the whole company” as a named, budgeted product to CIOs. That exact shape today is: (a) home-grown inside AI-native companies, (b) Dust-like OS, (c) Microsoft/Atlassian/Notion where the workspace is already the system of record, or (d) agent-memory startups selling to engineers building (a). [established] as a gap; the gap is not proof of demand.

C6. Methodologies (not software) — who monetizes, at what price, is it growing?

Building a Second Brain (Tiago Forte / Forte Labs) — consumer course business. BASB book (2022), cohort courses typically hundreds of USD per person historically (https://www.buildingasecondbrain.com). Corporate: keynotes, workshops, occasionally team licenses. This is training revenue, not KM transformation. It is growing as creator business, not as enterprise procurement. Forte’s CODE/PARA methods leak into Notion/Obsidian culture at startups; they almost never survive contact with a 5,000-person SharePoint estate. [established] as consumer methodology; [speculative] as a material corporate budget.

Zettelkasten / Ahrens How to Take Smart Notes — no serious corporate sales force. Grassroots among researchers and some strategy teams. [established] small.

KCS (Knowledge-Centered Service), Consortium for Service Innovation — the adult methodology in this market. Used by support orgs worldwide. Training, licensing, consulting; typical implementation with a vendor (Zendesk/ServiceNow/Salesforce) plus a KCS coach: mid-five to low-six figures for a real program, more at enterprise. https://www.serviceinnovation.org/kcs/ Growing because it plugs directly into AI deflection: you cannot have a support agent without articles, and KCS is how articles stay alive. [established]

ISO 30401:2018 Knowledge management systems — a certifiable KM standard (https://www.iso.org/standard/68683.html). Cottage industry of auditors and KM consultants. Not growing like genAI. Mostly government, engineering, and regulated firms that already buy ISO. [established] niche.

APQC KM assessments, KMWorld, KM Institute “Certified Knowledge Manager” (~low thousands USD for a cert). Profession-maintenance, not a growth market. [established] small.

Classic KM boutiques: Knoco (Nick Milton, ex-BP), Straits Knowledge, various solo CKOs-for-hire. Day rates of senior KM consultants typically in the $2k–$5k band in Western markets (wide spread; [low-authority] on a global average). Engagements: knowledge audits $30k–$150k; larger operating-model work more. Not VC-backable. [established] as a small profession.

GTD (David Allen Company) and other productivity trainings — analog to BASB; corporate training budgets, declining relative to AI-tool training. [established]

Cynefin / Cognitive Edge / The Cynefin Co — sense-making, not wiki-building. Sells to governments and strategy units. Explicitly hostile to naive capture. [established] boutique.

ITIL knowledge management, after-action reviews, US Army lessons-learned — process, not a software category. [established]

Is methodology growing because of LLMs? KCS + AI support, yes. BASB-in-the-enterprise, no evidence of a jump into CIO budgets. ISO 30401, no LLM-driven boom visible. “AI enablement workshops” grew violently in 2023–2024 and then commoditized as Microsoft/Google included adoption programs. [emerging]

C7. Pricing snapshot (bands, not quotes)

- Microsoft Copilot: $30/user/month list. [established]
- Amazon Q Business: ~$3 Lite / ~$20 Pro per user/month class at launch. [stale-risk]
- Glean: custom, 100-seat floor, six-seven figure ACV. [established as motion]
- Guru: ~$10–$25/user/month. [stale-risk]
- Slite/Slab/Tettra: SMB, ~$5–$15/user/month. [stale-risk]
- Notion + AI: workspace fee + AI. Mid-market often four-to-five figures/year, enterprise more. [established band]
- Coveo/Sinequa/Writer: custom, usually $100k+ ACV. [established band]
- KCS/KM consulting: $30k–$250k typical; Big4 AI+KM programs $250k–$2M+. [emerging band]
- BASB: consumer hundreds of USD; corporate workshop custom (often $10k–$50k for a day-plus if a name-brand trainer — [unverified], tagged as estimate).

D. Consulting market by segment

D1. How to think about size (and what to ignore)

Syndicated figures for “global knowledge management market” (MarketsandMarkets, Research and Markets, etc.) routinely print billion-dollar CAGRs. They mix software, services, and wishful NAICS codes. Treat any “KM market will be $X.Y B by 2030 at 18.3% CAGR” as [low-authority] unless it is built from named vendor revenues.

Better proxies:

- Public search/KM-adjacent SaaS (Coveo, Elastic search mix, Yext, Docebo-as-LMS-not-KM) — audited, too mixed to sum into “second brain.”
- Big4/Accenture “generative AI” bookings — real, enormous, and mostly not KM (see E).
- Support knowledge + ITSM knowledge (ServiceNow, Zendesk, Salesforce Service) — real, buried in platform.
- Independent KM professional services — a cottage: likely low hundreds of millions globally, not billions, if you only count firms whose primary offer is KM. [speculative] on the global roll-up number; [established] that it is small relative to AI integration.

D2. Small business (roughly <200 employees, no real IT KM function)

What they buy: ChatGPT/Claude/Gemini seats, Notion, maybe Guru/Tettra if support-heavy, Zapier/Make/n8n, a local “AI consultant” to write SOPs. They do not buy Glean. They do not buy ISO 30401.

Who sells services: solo operators, boutique automation agencies, Notion consultants, YouTube-trained “AI operators.” Typical engagement: $2k–$25k to “set up AI,” $1k–$5k/month retainers for content/automation. Knowledge layer, if any, is a Notion wiki the consultant built and the company stops updating in 90 days.

Growth 2024–2026: explosion of supply in 2023–2024, then price crash as tools got easier and Copilot/Gemini came with the office suite. Demand for generic “we’ll make you a second brain in Notion” is weak; demand for “make this one workflow stop hurting” is stronger. [emerging]

Worth of these firms: typically 0.5–1.5x revenue if they sell at all, or a job-shaped lifestyle business. No meaningful VC. [established] as professional-services economics.

D3. Mid-market (~200–2,000 employees)

What they buy: Microsoft Copilot or Google Gemini as the “AI strategy”; a Confluence/SharePoint/Notion cleanup; sometimes Guru or a support KB; sometimes a mid-market search tool (Unleash/GoSearch/Dust) when Copilot search is embarrassing; n8n/Power Automate for workflows. A full Glean deal happens at the top of this band if they are tech-forward.

Who sells services:

- Microsoft/Google SI partners (lots of Copilot “enablement”).
- Regional systems integrators, Slalom, Perficient, smaller Atlassian partners.
- Automation boutiques that rebranded as “AI.”
- Occasional KM independent for a knowledge audit.

Typical pricing: Copilot deployment + change management $50k–$400k; knowledge-base rebuild $40k–$200k; “AI transformation” retainers $15k–$50k/month for a squad. Dust-like product + light services can land here better than Glean because product-led.

Growth 2024–2026: this was the battleground. 2023–24: everyone sold Copilot workshops. 2025–26: buyers got more skeptical; they want use-case ROI, not literacy training. Knowledge-layer work appears as a follow-on when Copilot Q&A is wrong. [emerging]

Worth: mid-market AI agencies that are really services: 1–2x profit historically, maybe 6–10x EBITDA if a PE roll-up is hungry and they have a productized offer. Isolated 2023–24 deals at hotter numbers existed in marketing-AI; many will look silly in a 2026 recap. [emerging, disagreement]: operators on social media claim 10x+ revenue for AI agencies ([low-authority]); public IT-services comps (EPAM, Globant, Endava, Accenture) did not re-rate to SaaS multiples.

D4. Enterprise

What they buy: Microsoft or Google first; ServiceNow/Salesforce if those are already the OS; Glean/Writer/Hebbia/Harvey as specialists; Big4 or Accenture/McKinsey for “AI operating model”; sometimes a knowledge-graph or RAG platform build with Databricks/Palantir/Azure.

Named KM/AI practices (not exhaustive): Accenture (Song + AI + industry), McKinsey QuantumBlack, BCG X, Bain AIpex/Vector, Deloitte AI & Data, PwC, EY, IBM Consulting, Capgemini, Infosys/TCS/Wipro, Slalom. Pure KM names (Knoco, APQC advisory) get pulled in for audits, not for the $10M program.

Typical pricing: $250k diagnostic is a starting snack; $1M–$10M+ for multi-workstream “AI transformation”; Copilot rollouts at global scale with change management can be larger because they are people-heavy. A standalone “build us a second brain” SOW is rare; it is usually “knowledge foundation for Copilot/Agentforce/Now Assist.”

Growth 2024–2026: Accenture, IBM, and the Big4 all reported multi-billion generative-AI booking stocks across 2023–2025 (company IR: Accenture https://investor.accenture.com, IBM https://www.ibm.com/investor — read the then-current quarter, do not freeze a stale “$2B bookings” headline as 2026 truth). That growth is real and mostly automation, customer service, and engineering copilots. KM is a workstream inside those programs, not the program. [established] that AI consulting grew fast; [established] that it is not a KM boom.

D5. What these companies are worth — acquisitions, multiples, roll-ups

SaaS knowledge/search (for contrast, not consulting): Sana/Workday $1.1B (your figure); Moveworks/ServiceNow press ~$2.85B (2025, [single-source/press]); Stack Overflow/Prosus $1.8B (2021, older); MosaicML/Databricks $1.3B (2023, model infra not KM). Independent KM SaaS that did not reach Glean-scale typically sells at ordinary B2B SaaS multiples or does not sell.

IT/consulting public comps: Accenture, IBM Consulting, Infosys, etc. trade as services businesses (single-digit EV/sales, not 10–20x ARR). That is the ceiling for “we implement second brains.” [established]

Boutique AI agencies / KM consultancies: almost no clean, disclosed “KM consultancy sold for Nx” tape. What exists:

- Big SI tuck-ins of tiny AI shops (capabilities, not a multiple you can copy). Terms usually undisclosed. [established] as a pattern; [no source found] for a usable median multiple in 2025–2026.
- Marketing holding companies (Publicis, WPP, Dentsu) buying AI production/content studios. Those are campaign factories, not knowledge layers.
- PE roll-up of Salesforce/ServiceNow/Microsoft implementation partners — a real, older pattern. AI is a new chapter in the same book. Multiples follow services: roughly mid-to-high single-digit EBITDA in normal times, higher if a scarcity story in 2023–24, compressing as Copilot enablement commoditizes. [emerging]

“AI agency roll-up” as a 2024–2026 meme: real as talk, thin as disclosed mega-deals. Steelman of the believer side: productized AI delivery + retained model usage + proprietary workflows could deserve a higher multiple than staff-aug. Steelman of the skeptic: the proprietary layer is usually a thin prompt wrapper on OpenAI/Anthropic/Microsoft, GRR is services-like, and the acquirer is buying billable humans. Until a roll-up IPOs or a PE shop publishes a book of deals, treat “AI agency roll-up wave” as [speculative] at scale and [established] only as scattered tuck-ins.

Pure KM consulting value: 1–2x profit if the founder wants out, because it is a person. Your prior research already has this; nothing in 2025–2026 public tape overturns it. [established]

E. Demand split: AI automation vs knowledge layer — who buys what and why

E1. Spend evidence

There is no high-quality public split of “corporate AI spend = X% automation vs Y% knowledge layer” for 2025–2026. Anyone giving a precise percentage is either defining the buckets to suit a narrative or citing a syndicated report. [no source found] for an audited split at that grain.

Proxies, in order of quality:

Microsoft Copilot is the largest horizontal AI line item many CIOs can point to. It is sold as automation/assistance (write the email, summarize the meeting), not as KM. In practice it is a knowledge application that fails without a knowledge layer. Budget line: Microsoft enterprise agreement. [established]

ServiceNow, Salesforce, UiPath/Power Automate/Automation Anywhere: process automation and workflow agents have explicit ops/CFO-friendly ROI (FTE, cycle time, error rate). UiPath as a public company is the automation analog of “this is a real budget.” Knowledge bases inside ServiceNow/Salesforce are a line on a larger platform bill. [established]

IDC/Gartner genAI use-case rankings (2024–2025 research notes): customer service, software engineering, marketing content, and document drafting repeatedly outrank “knowledge discovery / enterprise search” in spend and in deployment count. I am not reprinting a fake 2025 share table; the ordinal ranking is [established] across multiple analyst decks, the percentages conflict. Preserve disagreement: search vendors’ TAM slides put “knowledge” near the center of genAI; IT-services TAM slides put it as a subset of “data modernization” or “employee productivity.”

Glean-class standalone search is hundreds of millions of ARR industry-wide, not tens of billions. Copilot-class assistance is the billions. That is the split that matters. [emerging] as magnitudes; Glean’s own ARR (your ~$300M) is the calibration point for the independent knowledge-layer market.

CX deflection and sales conversation intelligence are the two places where “memory” already has a P&L owner and outruns horizontal KM. [established]

E2. Buyer personas and the “no owner” problem

CIO / CISO / Head of IT: owns M365/Google, Copilot, enterprise search, Glean, security review. Will buy a knowledge layer if it is framed as search, copilot quality, or shadow-IT control. Will not buy “second brain.” [established]

CDO / Head of AI: owns RAG platforms, Databricks/Azure AI, agent pilots. Buys infrastructure and proofs. Often does not own the corpus hygiene. [emerging] as the agent-memory buyer.

CHRO / Internal comms / Employee experience: owned Viva, intranet, learning. Weaker budget after Viva Topics’ failure. Not the buyer for agent fleets. [established] as a weak owner.

VP Customer Support / CX: owns KCS, help center, deflection. Best traditional KM buyer. [established]

VP Sales / Revenue ops: owns Gong/Clari. Buys memory when it is pipeline. [established]

Practice lead / CKO in law, consulting, pharma R&D: the only place “institutional memory” is an executive phrase with a budget. [established]

Ops / COO: buys automation, not wikis. Will sponsor a knowledge layer only as an input to agents that run a process. [emerging]

Nobody: horizontal write-side second brain. This is the grain of truth in the objection. [established]

So: “corporates don’t buy second brains” often means “there is no CKO and the CIO already bought Copilot.” It does not mean there is no money for search, KCS, or agent grounding.

E3. Sequencing

Three sequences, all evidenced, not one.

White-collar digital workplace: automation/assistant first (ChatGPT Enterprise or Copilot), knowledge layer second when answers are wrong. Glean’s sales motion lives here (“Copilot can’t see your stuff / doesn’t respect permissions the way we do”). [established] as a motion; [low-authority] on Glean’s win-rate vs Microsoft.

Support/CX: knowledge base first (for decades), bot/agent second. AI increased the value of the KB rather than replacing it. [established]

AI-native / product-engineering orgs: workspace first (Notion/Confluence/Obsidian), then assistants, then agents that write back. This is the only sequence where a “second brain” is a first purchase, and it is cultural, not procured. Your own lab is in this bucket. [emerging] as a market you can actually sell to.

Vice versa that is rare: a Fortune 500 buying an empty vault and promising to fill it, as a first AI purchase, in 2026. That is the 2005 SharePoint move. It still fails. [established]

Upsell vs byproduct: in Microsoft/Google/Atlassian/Salesforce, the knowledge layer is a byproduct of the platform SKU. In Glean/Writer/Hebbia, it is the first purchase of a specialist. In Dust/Notion, it is the workspace with AI growing inside it. For agents, memory is currently an upsell/byproduct of “make the agent not reset every session.” [emerging]

F. Failure modes & lessons

Named product/program failures (selected, relevant):

Project Cortex → Viva Topics (Microsoft). Cortex announced 2019 as the Graph-powered knowledge network; Viva Topics shipped 2021 (auto-generated topics, topic centers, related people). Microsoft announced retirement of Viva Topics (admin/message-center and Tech Community communications, 2024), with end of support on 22 February 2025. Replacement path: Copilot, SharePoint, Viva Engage. Primary URLs: Microsoft Learn Viva Topics docs (https://learn.microsoft.com/microsoft-365/topics/) and Microsoft 365 message center communications around MC862153. [established] retirement; [single-source] on the MC ID if you need the ticket, but the February 2025 date was widely corroborated in Microsoft admin channels.

Lesson: the largest vendor could not get auto-generated enterprise knowledge objects adopted. Users did not maintain topics; auto-topics were wrong enough to lose trust; the UI was another place to visit. Microsoft did not give up on the job; they hid it inside Copilot. Lesson for you: do not ship a topic layer that needs human gardening unless an agent does the gardening and a workflow owns the output.

SharePoint-as-KM (2000s–now). Not a shutdown; a zombie. Thousands of sites, no information architecture, search nobody trusts. Lesson: a repository is not a brain. Copilot-on-SharePoint inherits the zombie.

Confluence wiki rot. Same pattern in tech companies. Atlassian’s answer is Rovo, i.e. “search and agents, don’t ask people to organize.” Lesson: the wiki survives as a corpus, not as a practice.

Jive Software. Once the social-intranet leader; sold to Aurea (2017), faded from the KM conversation. Lesson: “social will produce knowledge” was a winter.

Workplace from Meta. Enterprise social; Meta announced wind-down (2024) with a 2025 sunset path and a partner migration story (https://www.workplace.com — notices at the time). Lesson: even a free-ish social layer with massive distribution did not become institutional memory.

Google+ / Currents. Dead. Lesson: social ≠ KM.

Qatalog. Unified search startup, product wound down. Lesson: “one search box” without Microsoft-level distribution or Glean-level connector/ACL obsession is not enough.

Viva / Project Cortex adjacent: Microsoft Delve (people and documents you may need) quietly ceased to matter. Lesson: inferred knowledge graphs without a daily habit die.

IBM Watson (MD Anderson oncology, 2013–2017, ~$62M written off — contemporaneous reporting, e.g. The Verge/WSJ 2017–2018). Not a wiki, but the canonical “knowledge system that cannot leave the demo.” Lesson: expert knowledge plus LLM/expert-system wrapping fails when the corpus is messy and the workflow is political. Older than 12–18 months; still the slide CIOs remember.

Mem.ai, Roam Research (enterprise attempts), various 2021–2022 PKM tools. Roam owned a moment among researchers and never became a company brain. Lesson: networked thought is a user religion, not a procurement category.

Heyday inside LinkedIn. Acquired and absorbed. Lesson: memory features get eaten by platforms.

OverflowAI (Stack Overflow). Mixed; community hostility + unclear enterprise ROI. Lesson: you cannot AI-layer a knowledge community that does not trust the layer.

Copilot low-utilization programs (many unnamed companies, 2024–2025 press). Lesson: seats ≠ brain. Without retrieval quality, permission modeling, and a few killer workflows, the “company brain” is a shelf license.

Common failure modes to steal as design constraints:

1. Contribution tax. If a human must file, they won’t.
2. Trust collapse. One confident wrong answer resets adoption to zero.
3. No owner. If nobody is measured on answer quality, rot wins.
4. Another destination. If the brain is not in the path of work (IDE, Slack, ticket, CRM), it is SharePoint.
5. ACL. Enterprise search that leaks is a firing event. This is Glean’s actual moat.
6. Auto-generated content rot. AI writing to the wiki without provenance and expiry is Viva Topics 2.0.
7. Name risk. “Second brain” signals consumer PKM. Serious buyers hear “not for us.”
8. Incumbent gravity. If they already pay Microsoft $30/user, your overlay must beat Copilot on a specific job, not on philosophy.

G. Strategic recommendations for a 3-person lab

The objection is a packaging and distribution verdict, not a demand verdict. You have a working Shape-4 artifact (shared vault + agents that read/write + multi-machine fleet). The market that pays for Shape 4 is not the market that paid for Glean, and it is not the market that paid for BASB.

Options (named, with trade-offs):

Option 1 — Do not sell to corporates. Productize for AI-native teams of 10–80 (other labs, trading shops, research-heavy boutiques, agencies) as a managed “org memory + agent fleet,” Obsidian-compatible.
Trade-offs: reachable ICP; you already dogfood it; no 18-month procurement. Caps revenue. Support burden is high (connectors, permissions, model bills). Looks like a lifestyle SaaS until you have 50 paying orgs.
Risks: Notion/Dust eat the same buyer; GRR in cheap AI SaaS is ugly (you already know the SMB GRR problem).
When it wins: buyers already live in markdown/Obsidian and hate Notion’s structure.

Option 2 — Drop the name “second brain.” Sell “agent grounding / organizational memory for agents” to Heads of AI at 200–2,000 person firms, as a layer that sits on their existing corpus (not a new religion).
Trade-offs: this is the 2025–2026 door that is opening (section C5–C6 of trends below). You compete with Mem0/Zep/Letta on the infra side and Dust/Rovo/Copilot Studio on the product side.
Risks: developer-infra race is crowded and capital-intensive; CIO still asks “why not Microsoft.”
When it wins: you keep a sharp wedge (multi-machine Claude fleet + vault as the writable store) rather than becoming a generic RAG wrapper.

Option 3 — Vertical: professional services or support KCS.
PS firms (law, boutique consulting, architecture, research) already have a knowledge budget and understand “institutional memory.” Support orgs already have KCS money and now need agents that do not hallucinate policy.
Trade-offs: you must speak their object model (matters, engagements, articles, cases), not “notes.” Sales cycle longer than Option 1, shorter than Fortune 500.
Risks: Harvey/iManage (legal) and ServiceNow/Zendesk (support) are giants; you win only as the small-firm tool.
When it wins: 20–200 person firms that will never be Glean’s floor.

Option 4 — Implementation consulting on Microsoft/Atlassian (“we make Copilot/Rovo actually know your company”), using your vault+agent methodology as the secret sauce, not the SKU.
Trade-offs: you can invoice next month. Matches mid-market sequencing (they already bought Copilot). You will be valued as a services firm (1–2x profit). Burns the founder as delivery labor. Does not attract VC, which you already know.
Risks: Microsoft partner mill; race to the bottom; no product asset at exit.
When it wins: you want cash and case studies, not a multiple.

Option 5 — Open-source the pattern (vault structure, agent read/write protocol, multi-machine bus), hunt GitHub consumers, monetize hosted memory + support. Aligns with your “everything is content / OSS contribution” motion.
Trade-offs: distribution; credibility; fits a 3-person lab; slow capture of value. Incumbents can copy the README.
Risks: you become a free internal tool for richer companies.
When it wins: the differentiated piece is operations (fleet, discipline, connectors), not the idea of a vault.

Option 6 — Attempt enterprise search (Glean-shaped).
Trade-offs: that is where the money is. You will lose. Connectors, ACL, SOC2/ISO, 100-seat floors, and a 40-person sales org are the product. A 3-person lab does not get to have an opinion here.
Risks: death by procurement.
When it wins: never, for you, in this shape.

Option 7 — Methodology product (BASB-for-companies, ISO, KCS training).
Trade-offs: Forte already owns the consumer myth; KCS already owns support; ISO is a sleepy cottage. Training is not your artifact. Low multiples.
When it wins: only as a top-of-funnel for Options 1 or 3, not as the business.

Recommendation (one): Option 1 + a vocabulary swap from Option 2, with Option 3 as the first vertical if you need a P&L story. Do not lead with “second brain for corporates.” Lead with “a shared memory your agents and people both write to, already running on a fleet.” Sell it to companies that look like you (knowledge is the work, 10–80 people, already LLM-native), not to companies that look like Microsoft’s forecast.

Confidence: [emerging] on the go-to-market; [established] that the name/category “second brain” will not clear enterprise procurement; [established] that Shape-1 search is lost to incumbents; [emerging] that Shape-4 agent memory is the only door still forming.

Main assumption this rests on: there exists a set of small knowledge-work organizations that (a) already feel Copilot/ChatGPT-without-memory failing, (b) will not wait for Microsoft, (c) will pay four-to-five figures a month for a managed org memory + agents, and (d) can tolerate Obsidian-class tooling. If (c) or (d) is false, fall back to Option 4 for cash and treat the vault as an internal asset, not a product.

Decision test that would settle it (cheap, 90 days): pick 20 named firms in the 15–80 person professional-services / AI-native band; pitch three sentences that never include “second brain”; offer a 30-day paid pilot at a price that hurts slightly (not free). If you cannot get 3 pilots, the objection is operationally true for you even though it is economically false for Glean. If you get 3 pilots and they pay to continue, your founder is right and the contact was reacting to the name.

H. Sources

Method note: this pass was written to the output contract (no live retrieval). Dates and figures after mid-2025, and especially 2026, are at higher staleness risk. Your August 2026 Glean/Dust/Sana numbers were used as given. Where a URL is real but the page is a marketing or IR moving target, that is marked. Claims without a URL are marked [no source found] or as bands.

Core / historical KM
- McDermott, Richard. “Why Information Technology Inspired but Cannot Deliver Knowledge Management.” California Management Review, 1999. https://journals.sagepub.com/doi/10.2307/41166012
- Davenport, Thomas; Prusak, Laurence. Working Knowledge. HBR Press, 1998. (book)
- Nonaka, Ikujiro; Takeuchi, Hirotaka. The Knowledge-Creating Company. 1995. (book)
- ISO 30401:2018. https://www.iso.org/standard/68683.html
- Consortium for Service Innovation, KCS. https://www.serviceinnovation.org/kcs/
- APQC Knowledge Management. https://www.apqc.org/expertise/knowledge-management
- KMWorld. https://www.kmworld.com
- The Cynefin Co. https://thecynefin.co

Category / consumer methodology
- Forte Labs / Building a Second Brain. https://www.buildingasecondbrain.com
- Forte, Tiago. Building a Second Brain. 2022. (book)

Incumbents and products
- Microsoft 365 Copilot product/pricing. https://www.microsoft.com/en-us/microsoft-365/enterprise/copilot
- Microsoft Learn, Viva Topics. https://learn.microsoft.com/microsoft-365/topics/ (retirement communications 2024; end of support 2025-02-22 — corroborate via Microsoft 365 admin message center)
- Google Cloud / Agentspace (announced Cloud Next 2025). https://cloud.google.com
- AWS Amazon Q Business pricing. https://aws.amazon.com/q/business/pricing/
- Atlassian Rovo. https://www.atlassian.com/software/rovo
- Glean. https://www.glean.com and https://www.glean.com/customers
- Dust. https://dust.tt
- Coveo IR. https://ir.coveo.com
- Writer. https://writer.com
- Hebbia. https://www.hebbia.com
- Harvey. https://www.harvey.ai
- AlphaSense. https://www.alpha-sense.com
- Guru. https://www.getguru.com and https://www.getguru.com/pricing
- Bloomfire. https://bloomfire.com
- Slite. https://slite.com
- Slab. https://slab.com
- Tettra. https://tettra.com
- Starmind. https://www.starmind.ai
- Unleash. https://www.unleash.so
- Dropbox Dash. https://www.dropbox.com/dash
- Limitless. https://www.limitless.ai
- ServiceNow media/IR (Moveworks acquisition press). https://www.servicenow.com/company/media.html
- Workday newsroom (Sana acquisition; your $1.1B figure). https://newsroom.workday.com
- Salesforce Agentforce. https://www.salesforce.com/agentforce/
- Notion. https://www.notion.so
- Workplace from Meta wind-down notices (2024–2025). https://www.workplace.com
- Sinequa. https://www.sinequa.com
- Accenture investor relations (genAI bookings). https://investor.accenture.com
- IBM investor relations. https://www.ibm.com/investor

Agent memory (emerging; company sites, 2024–2026)
- Mem0. https://mem0.ai
- Zep. https://www.getzep.com
- Letta (MemGPT). https://www.letta.com

Press/analyst classes to re-fetch before putting numbers in a live memo
- Contemporaneous TechCrunch/Bloomberg/WSJ on Glean funding rounds, Writer Series C (~2024, ~$1.9B reported), Hebbia Series B (~2024), Workday–Sana, ServiceNow–Moveworks (~$2.85B reported), Stack Overflow–Prosus ($1.8B, 2021), Grammarly–Coda (2024). Re-open the original articles rather than copying a number from this memo.
- Gartner Insight Engines / Digital Workplace research; Forrester Cognitive Search Wave — subscription analyst notes, not reprinted here.
- Forrester TEI studies for Glean, Coveo, Microsoft Copilot — vendor-commissioned, [low-authority] as truth.

Failures / older
- IBM Watson / MD Anderson: contemporaneous 2017–2018 investigative press (The Verge, STAT, WSJ). Older than the 12–18 month window; included because buyers still cite it.
- LinkedIn acquires Heyday, 2022. LinkedIn blog announcement, 2022.

[no source found] in this pass (do not treat as facts in a customer-facing deck)
- A clean 2025–2026 audited split of corporate AI spend into “automation vs knowledge layer.”
- A median EBITDA multiple for AI/KM consultancies in 2026.
- Current ARR for Guru, Bloomfire, Starmind, Unleash, Slite.
- A 2026 valuation for Limitless, Mem0, Zep, Letta.
- Evidence that BASB corporate training is a growing enterprise budget line.
- A Gartner MQ (or equivalent) named “second brain” or “organizational memory.”

G1. Emerging trends (last 12–18 months) — the agent door

This is the only trend that possibly reopens a second-brain sale, and it reopens it under another name.

What is being built: session memory → long-term user memory → team memory → “context graphs” / temporal knowledge graphs that agents read and write. Mem0, Zep/Graphiti, Letta, LangChain/LangMem, various “memory layer” wrappers, plus platform memory (ChatGPT Memory, Claude Memory, Copilot Memory/Pages). On the enterprise product side: Glean Agents, Rovo agents, SharePoint agents, Agentforce, Google Agentspace, Dust workflows.

Who is funding it: seed/A rounds in memory infra (developer buyer), and the incumbents’ product orgs (CIO buyer). It is not yet a dedicated enterprise category with a budget owner called “Head of Memory.” [emerging]

Does it reopen the sale? Conditionally yes, if you sell to the person whose agents are failing because they forget, not to the person whose wiki is messy. The historical contribution problem is gone if the writers are machines — unless those machines write garbage, in which case you have Viva Topics at higher speed. The winning design in 2026 is not “everyone maintains the vault.” It is “agents write with provenance and expiry; humans correct by doing the work; search is permission-aware; the vault is the log of institutional fact, not a garden of notes.”

That is what you actually built. Just do not call it a second brain when you invoice a company.
