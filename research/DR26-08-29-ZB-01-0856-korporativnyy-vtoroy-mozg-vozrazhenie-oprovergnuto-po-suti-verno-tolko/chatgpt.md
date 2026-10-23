# DR26-08-29-ZB-01-0856 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Executive Summary

Date: 30 August 2026, Europe/London. Exact time not checked.

Evidence note: because the instruction prohibited tools and live browsing, I have not live-verified whether every linked page is still available on 30 August 2026. I therefore avoid claiming 2026 vendor status where I cannot support it, identify undated vendor pages, and mark unsupported market ranges “[no source found]”.

1. [established] The objection “corporates have no reason to buy a second brain” is refuted literally. Corporations already pay for the underlying capability under other names: enterprise search, knowledge management, digital workplace, employee-service platforms, knowledge graphs, AI assistants, and data/AI platforms. ServiceNow’s $2.85 billion agreement to acquire Moveworks is unusually strong revealed-preference evidence that governed enterprise knowledge retrieval plus automation has substantial strategic value. [single-source] (ServiceNow, 10 March 2025, https://www.servicenow.com/company/media/press-room/servicenow-to-acquire-moveworks.html)

2. [established] The objection is true in a narrower commercial sense: “a second brain for the company” is not normally an enterprise budget line. A generic shared repository creates an extra migration, governance, security, and adoption problem. Buyers fund a measurable workflow owned by somebody: support resolution, employee service, sales enablement, research, onboarding, engineering productivity, or compliance.

3. [established] Historical KM failures did not prove that institutional memory lacks value. They showed that publishing software alone does not create a maintained knowledge system. The recurring problems were incentives, tacit knowledge, context loss, ownership, permissions, stale material, and separation from daily work. These problems were documented well before modern LLMs. (McDermott, Summer 1999, https://doi.org/10.2307/41166012; Alavi and Leidner, June 2001, https://doi.org/10.2307/3250961)

4. [established] LLMs materially reduce the retrieval and interpretation cost of corporate knowledge, but do not remove the need for governance. The strongest independent evidence is the large contact-centre study later published in the Quarterly Journal of Economics: generative assistance raised productivity by about 14% on average, with much larger gains for less-experienced workers, consistent with the AI transferring practices associated with stronger workers. (Brynjolfsson, Li and Raymond, 2025, https://academic.oup.com/qje/article/140/2/889/7990658)

5. [emerging] “Organizational memory for agents” is a credible new entry door. Letta, AWS AgentCore Memory, Google Vertex AI Memory Bank, and the stateful tooling around agent platforms show that memory is becoming an explicit infrastructure component. However, basic conversation memory is likely to commoditize. The defensible layer is governed organizational context: provenance, permissions, temporal validity, entity resolution, evaluation, and safe write-back. [single-source per product] (Letta, September 2024, https://www.letta.com/blog/letta-seed-round; AWS, 16 July 2025, https://aws.amazon.com/blogs/aws/introducing-amazon-bedrock-agentcore-securely-deploy-and-operate-ai-agents-at-any-scale/; Google Cloud, undated, https://cloud.google.com/vertex-ai/generative-ai/docs/agent-engine/memory-bank/overview)

6. [established] No credible public dataset cleanly divides corporate AI spending into “task automation” and “knowledge layer.” Analyst taxonomies overlap: search embedded in copilots looks like application spending; vector retrieval can be infrastructure; data preparation can be consulting; and suite vendors bundle knowledge access into existing licenses. Gartner’s headline $644 billion 2025 generative-AI forecast was dominated by devices and servers, not corporate AI-workflow software. (Gartner, 31 March 2025, https://www.gartner.com/en/newsroom/press-releases/2025-03-31-gartner-forecasts-worldwide-generative-ai-spending-to-total-644-billion-in-2025)

7. [emerging] For a three-person lab, the best opening is not a horizontal SaaS product or a large-enterprise “replace all knowledge systems” pitch. It is a paid, narrow “agent-ready institutional memory” engagement for a mid-market organization, attached to one expensive workflow and implemented over its existing systems of record.

B. Verdict on “corporates have no reason to buy a second brain”

Verdict: refuted as a statement about need; substantially correct as a warning about packaging and purchasing sequence.

B1. Why enterprises historically under-invested in KM

[established] KM is a collective-action problem. The employee doing the documentation pays the cost immediately, while the future benefit is diffuse, uncertain, and often captured by another team. Technology cannot resolve that incentive mismatch by itself. McDermott’s pre-LLM analysis explicitly distinguished human knowledge communities from information storage and warned that treating knowledge as an IT object produces disappointing systems. [older source] (McDermott, Summer 1999, https://doi.org/10.2307/41166012)

[established] Knowledge is not equivalent to documents. It includes judgment, exceptions, relationships, provenance, and local context. Alavi and Leidner’s review described KM as processes of creating, storing, transferring, and applying knowledge, not merely constructing a repository. [older source] (Alavi and Leidner, June 2001, https://doi.org/10.2307/3250961)

[established] The “SharePoint graveyard” and “wiki rot” labels summarize several mechanisms, not a rigorously established universal failure rate:

- Material is copied rather than linked to an authoritative source.
- Nobody owns freshness.
- Search indexes text but not business meaning.
- Publishing is outside the workflow where knowledge is created.
- Permissions become either too broad or too restrictive.
- Taxonomies demand continuous manual upkeep.
- Leaders fund launch and migration, but not stewardship.
- Employees cannot see a personal return from contributing.

No defensible cross-industry percentage for “failed SharePoint/KM programs” was identified here; popular failure-rate numbers should be treated as [speculative] [no source found].

[established] Information retrieval itself was economically meaningful even before LLMs. McKinsey estimated that interaction workers spent about 19% of their working time searching for and gathering information. This is old evidence and should not be treated as a current benchmark, but it establishes the size of the historical friction. [single-source] [older source] (McKinsey Global Institute, July 2012, https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-social-economy)

B2. What changed with LLMs

[established] LLMs changed the interface and unit economics of retrieval:

- Users can ask in ordinary language rather than knowing the taxonomy.
- The system can synthesize several documents rather than return ten links.
- It can translate an expert’s material into a novice’s working context.
- Meetings, tickets, code changes, and conversations can be converted into draft knowledge automatically.
- The same grounding layer can serve employees and software agents.

The contact-centre field study is important because it was not merely a vendor search benchmark. It found approximately 14% average productivity improvement and much larger gains for less-skilled or less-experienced workers; the authors interpreted part of the effect as diffusion of knowledge embodied in the model. (Brynjolfsson, Li and Raymond, 2025, https://academic.oup.com/qje/article/140/2/889/7990658)

[emerging] Named enterprise deployments report additional benefits, but their evidentiary quality is weaker:

- Morgan Stanley said 98% of financial-adviser teams adopted its grounded assistant and that access broadened from roughly 20% of available documents to more than 80%. This is a vendor/customer case study, not an independently audited trial. [single-source] [low-authority] (OpenAI, 26 March 2024, https://openai.com/index/morgan-stanley/)
- Booking.com reported that Glean saved approximately 5,000 employee hours per month. This is vendor-published and should be treated as directional until independently audited. [single-source] [low-authority] (Glean, undated, https://www.glean.com/customers/booking-com)

[established] LLMs have not fixed bad source material, permission errors, ambiguous ownership, or absent evaluation. Gartner predicted that at least 30% of generative-AI projects would be abandoned after proof of concept by the end of 2025 because of data quality, risk controls, escalating costs, or unclear business value. This was a forecast, not an observed final failure rate. [single-source] (Gartner, 29 July 2024, https://www.gartner.com/en/newsroom/press-releases/2024-07-29-gartner-predicts-30-percent-of-generative-ai-projects-will-be-abandoned-after-proof-of-concept-by-end-of-2025)

The evidence therefore supports “LLMs make a good knowledge system more valuable,” not “LLMs make knowledge management unnecessary.”

B3. Is the category rejected, or renamed?

[established] The phrase “second brain” is predominantly associated with personal knowledge management. Enterprise buyers use language connected to an owner, architecture, or measurable outcome:

- Enterprise search or cognitive search: CIO, digital workplace, IT.
- Knowledge management system: KM, operations, support, professional services.
- Knowledge base: customer service, IT service management, sales enablement.
- Employee experience or employee-service platform: CHRO and CIO.
- Knowledge graph, ontology, data fabric: CDO, CIO, platform engineering.
- AI workspace or enterprise assistant: CIO/CAIO and business-function owners.
- Institutional memory: commonly understandable language, but not a standardized procurement category.
- Agent platform or agentic workflow: automation budget, with knowledge presented as required grounding.

Forrester’s older analyst vocabulary included “cognitive search,” while Gartner used “insight engines.” These are older categories and the exact 2025-2026 category boundaries were not live-verified here. [single-source per analyst] [older source] (Forrester, Q4 2021, https://www.forrester.com/report/the-forrester-wave-cognitive-search-q4-2021/RES176208; current Gartner taxonomy [no source found])

[established] The important commercial distinction is therefore:

“Second brain” describes the founder’s architecture.

“Enterprise search,” “employee support,” “service knowledge,” or “agent grounding” describes the buyer’s budget.

B4. Steelman of the objection

[established] The strongest version of the objection is not “knowledge has no value.” It is:

- Microsoft, Google, ServiceNow, Salesforce, Atlassian, and other incumbents already possess the documents, identities, permissions, and distribution.
- Another shared vault introduces another source of truth.
- A broad brain has no single accountable budget owner.
- Migration and employee behaviour change can cost more than retrieval saves.
- A generic assistant can hallucinate across a larger surface area.
- Broad write access by agents can contaminate authoritative records.
- Enterprises prefer a solution to a specific operational problem over an abstract intelligence layer.

That position is persuasive against a standalone, repository-first sale. It is not persuasive against a governed layer attached to an expensive workflow.

C. Competitive map

The Glean, Dust, Sana/Workday, and Microsoft Copilot figures supplied in the brief are treated as established context and are not repeated.

C1. Full-stack company knowledge plus agents

1. ServiceNow plus Moveworks

Shape: enterprise search, employee assistant, IT/HR/service automation, permissions-aware access, and actions.

Pricing: custom enterprise contract; no reliable public unit price.

Target: large enterprise.

Traction/value: ServiceNow agreed to acquire Moveworks for $2.85 billion in cash and stock. This is one of the clearest market-value signals for the combined knowledge-plus-automation category. [single-source] (ServiceNow, 10 March 2025, https://www.servicenow.com/company/media/press-room/servicenow-to-acquire-moveworks.html)

2. Writer

Shape: enterprise generative-AI platform, Knowledge Graph, agent/application builder, governance, and business-facing AI applications.

Pricing: custom enterprise subscription/usage; public pricing insufficient for an enterprise TCO comparison.

Target: mid-market and enterprise, particularly regulated or brand-sensitive organizations.

Traction/value: Writer raised $200 million at a reported $1.9 billion valuation in November 2024. [single-source] (Writer, 12 November 2024, https://writer.com/blog/series-c/)

Exact-shape assessment: close. Its Knowledge Graph supplies governed context to applications and agents, although it is not marketed as a transparent Obsidian-like shared vault. [single-source] [low-authority] (Writer, undated, https://writer.com/product/knowledge-graph/)

3. Palantir AIP and Ontology

Shape: a shared operational ontology over enterprise objects, permissions, logic, workflows, and AI agents capable of taking governed actions.

Pricing: custom platform/usage contracts.

Target: large enterprise and government; complex operational environments.

Valuation: Palantir is publicly traded, so a current valuation requires a live market quote and is not asserted here.

Exact-shape assessment: architecturally one of the closest systems. Its differentiator is not note-taking but a governed representation of the company on which humans and agents operate. [single-source] [low-authority] (Palantir, undated, https://www.palantir.com/platforms/aip/)

4. Atlassian Rovo and Teamwork Graph

Shape: cross-product search, chat, and agents grounded in Atlassian and connected SaaS data.

Pricing: suite/per-seat packaging that has changed since launch; current 2026 rates were not verified.

Target: Atlassian’s installed base, from mid-market through enterprise.

Valuation: part of publicly traded Atlassian rather than a separately valued business.

Exact-shape assessment: close for engineering and project organizations. The Teamwork Graph performs the shared-context role; agents are the operational interface. [single-source] [low-authority] (Atlassian, 30 April 2024, https://www.atlassian.com/blog/announcements/introducing-atlassian-rovo-ai)

5. Microsoft 365, SharePoint, Graph and Copilot Studio

Shape: identity, documents, collaboration history, enterprise graph, search, copilots, and custom agents/actions.

Pricing: a mixture of per-seat suite licenses and consumption for agent execution; current rates require verification.

Target: all segments, with the strongest distribution in existing Microsoft estates.

Valuation: not separable from Microsoft.

Exact-shape assessment: technically yes, commercially bundled. Microsoft is powerful precisely because the company does not have to move all its knowledge into a new repository. Copilot Studio is positioned as a place to build and govern agents connected to enterprise data. [single-source] [low-authority] (Microsoft, undated, https://www.microsoft.com/en-us/microsoft-copilot/microsoft-copilot-studio)

6. Salesforce Agentforce plus Data Cloud

Shape: customer and operational data unified in Data Cloud, grounded agent reasoning, and actions in Salesforce workflows.

Pricing: consumption plus Salesforce platform licenses; packaging has changed and current 2026 rates were not verified.

Target: sales, service, commerce, and Salesforce-centric enterprises.

Valuation: not separable from Salesforce.

Exact-shape assessment: yes within customer-facing workflows, but not usually a universal company memory.

7. Glean Agents and Dust

Shape: federated enterprise knowledge plus agents/actions.

Exact-shape assessment: among the closest independent products. Their evolution from search/assistants into agents supports the sequencing thesis: retrieval establishes context and distribution; agents turn that context into workflow value. Glean’s agent announcement is vendor evidence rather than an independent evaluation. [single-source] [low-authority] (Glean, 25 February 2025, https://www.glean.com/blog/introducing-glean-agents)

C2. Enterprise-search and RAG specialists

8. Coveo

Pricing model: custom enterprise and usage-oriented pricing.

Target: large digital businesses, commerce, customer service, and workplace search.

Traction/valuation: publicly traded; a current market capitalization was not verified.

Assessment: strong search/relevance layer, but an organization must still supply workflow, governance, and write-back.

9. Sinequa

Pricing model: custom enterprise platform.

Target: large, complex, often regulated organizations.

Traction/value: acquired by ChapsVision; terms were not reliably verified here. [no source found]

Assessment: search and retrieval are core; agent operation is an extension rather than the original product identity.

10. Lucidworks

Pricing model: custom subscription/platform pricing.

Target: enterprise search and digital experience.

Traction/value: private; no current valuation found. Its last widely reported large round was $100 million in 2019, now too old to indicate current value. [older source] [single-source] (Lucidworks, 12 August 2019, https://lucidworks.com/post/lucidworks-raises-100m/)

11. Elastic

Pricing model: cloud consumption or self-managed subscription.

Target: technical teams and enterprises that can assemble search, security, observability, and retrieval infrastructure.

Valuation: publicly traded; current value not asserted.

Assessment: a component for building a brain, not a finished corporate operating memory. It places more implementation and product responsibility on the buyer.

12. Yext and Algolia

Pricing model: subscription/usage, with enterprise contracts.

Target: Yext emphasizes structured enterprise and customer-facing search; Algolia emphasizes developer-led search and retrieval experiences.

Assessment: adjacent infrastructure. They solve findability, not the human operating model of institutional memory.

C3. AI knowledge bases and team-memory products

13. Guru

Shape: knowledge base, intranet, enterprise search, verification workflows, and AI answers/knowledge agents.

Public pricing: the pricing page has advertised an all-in-one per-user plan, historically around $15 per user per month; current contractual pricing and enterprise discounts require verification. [single-source] [low-authority] [no publication date] (Guru, undated, https://www.getguru.com/pricing)

Target: support, sales enablement, operations, and mid-market/enterprise teams.

Traction/value: private; no defensible current valuation found. [no source found]

Assessment: one of the stronger products for maintained, verified team knowledge. It is closer to a governed knowledge base than an autonomous shared workspace.

14. Slite

Shape: team documentation, wiki, search, and AI answers.

Public pricing: per-user SaaS; the pricing page has historically shown approximately $8-$12.50 per user per month depending on tier and billing period. Recheck before commercial use. [single-source] [low-authority] [no publication date] (Slite, undated, https://slite.com/pricing)

Target: small and mid-market teams.

Traction/value: no defensible current valuation or audited traction figure found. [no source found]

Assessment: easier adoption than a full enterprise platform, but limited differentiation once Notion, Microsoft, and Atlassian bundle similar capabilities.

15. Slab

Shape: team wiki and unified search.

Public pricing: historically around $6.67 per user per month for the lower paid tier and $12.50 for Business when billed annually; verify current pricing. [single-source] [low-authority] [no publication date] (Slab, undated, https://slab.com/pricing/)

Target: technical SMB and mid-market teams.

Traction/value: private; no reliable current valuation found. [no source found]

16. Tettra

Shape: internal knowledge base with AI answers, historically oriented toward Slack-centric teams.

Public pricing: historically around $4 and $8 per user per month across paid tiers; current rates require verification. [single-source] [low-authority] [no publication date] (Tettra, undated, https://tettra.com/pricing)

Target: SMB and mid-market support/operations teams.

Traction/value: Capacity acquired Tettra; terms were not disclosed. [single-source] (Capacity, 1 February 2024, https://capacity.com/blog/capacity-acquires-tettra/)

Lesson: a standalone team knowledge base can become a feature in a broader support-automation platform.

17. Bloomfire

Shape: enterprise KM, communities, search, and knowledge sharing.

Pricing: custom annual enterprise pricing.

Target: mid-market and enterprise knowledge, research, support, and customer-insight teams.

Traction/value: private; reliable current valuation not found. [no source found]

Assessment: established KM packaging, but less obviously an agent-operated workspace.

18. Starmind

Shape: expert discovery and access to tacit organizational knowledge.

Pricing: custom enterprise pricing.

Target: large organizations where finding the person who knows is as important as finding a document.

Traction/value: no defensible current valuation found. [no source found]

Assessment: an important counterexample to document-centric architecture. A corporate brain must model people and expertise, not only files.

19. Twine

Shape: intranet and employee communications/knowledge access.

Pricing: custom.

Traction/value: reliable current independent evidence was not identified. [no source found]

Assessment: digital workplace rather than agent memory.

20. Qatalog

Shape: originally a work hub/unified workspace, later AI-oriented knowledge work.

Pricing, current traction, and operating status: not reliably verified. [no source found]

Assessment: do not cite Qatalog as proof of present market traction without current diligence. Its shifting positioning is itself a warning about selling an abstract “work hub.”

C4. Personal-memory products adjacent to enterprise

21. Rewind/Limitless

Shape: personal meeting and activity memory rather than a permissioned company knowledge plane.

Value: Rewind reportedly raised funding at a $350 million valuation in 2023. This is old, single-source evidence and not evidence of current enterprise traction. [single-source] [older source] (Forbes, 4 May 2023, https://www.forbes.com/sites/alexkonrad/2023/05/04/rewind-ai-raises-12-million-at-350-million-valuation/)

Assessment: proves investor interest in memory, but not corporate willingness to establish a shared brain.

22. Mem

Shape: AI-first personal/team notes and recall.

Value: Mem raised $23.5 million at a reported $110 million valuation in 2022. This is old and does not establish current enterprise adoption. [single-source] [older source] (TechCrunch, 1 November 2022, https://techcrunch.com/2022/11/01/mem-a-work-focused-app-with-ai-smarts-raises-23-5m/)

23. Heyday

Shape: personal browsing/research memory.

Current traction, pricing, and enterprise offer: no reliable current source found. [no source found]

Conclusion: personal-memory startups demonstrate a user need, but should not be counted as corporate-second-brain competitors unless they have enterprise identity, permissions, governance, retention, administration, and shared ownership.

C5. Methodologies sold without owning the software

1. KCS, or Knowledge-Centered Service

[established] KCS is the most commercially legible methodology in this field because it attaches knowledge creation and reuse to support work. It treats solving a case and improving the knowledge base as one workflow rather than a separate documentation project. The Consortium for Service Innovation maintains the methodology; training and certification are sold by authorized providers. [single-source] [older source] (Consortium for Service Innovation, 2016, https://library.serviceinnovation.org/KCS_Practices_Guide_v6)

Buyer: support/service leadership.

Budget: service transformation, customer experience, ITSM, or support enablement.

Pricing: course and implementation prices vary by provider; no reliable cross-provider median found. [no source found]

2. ISO 30401 Knowledge Management Systems

[established] ISO 30401 gives KM a governance and management-system vocabulary suitable for enterprises, audits, and certification. It is not a software architecture. [single-source] [older source] (ISO, November 2018, https://www.iso.org/standard/68683.html)

Buyer: quality, risk, KM leadership, regulated operations, or executive transformation.

Pricing: standards, training, readiness consulting, and certification are separate purchases; no global engagement median found. [no source found]

3. APQC

[established] APQC monetizes KM through benchmarking, research, membership, process frameworks, and advisory services. Its vocabulary is more enterprise-compatible than “Building a Second Brain.” [single-source] [low-authority] [no publication date] (APQC, undated, https://www.apqc.org/expertise/knowledge-management)

4. Building a Second Brain and PKM training

[speculative] Tiago Forte’s methodology has clear individual and cohort-course recognition, but robust public evidence for a growing, separately measurable corporate BASB-training market was not identified. Corporate workshops may exist, but current prices, enterprise clients, and repeatable procurement demand require direct verification. [no source found]

5. Zettelkasten

[speculative] No material corporate-consulting category organized around the term “Zettelkasten” was established. Companies may use linked-note principles internally, but enterprise buyers are unlikely to purchase the German methodology name. [no source found]

6. Knowledge audits

[established] Knowledge audits, taxonomy work, content inventories, knowledge-risk analysis, and governance design are sold by boutiques and larger consultancies, normally under custom statements of work. Public price transparency is extremely weak. [no source found for a defensible market median]

C6. Who sells the exact “shared space plus read/write agents” shape?

[established] The exact shape exists, but usually without the shared editable vault being the product headline.

Closest architectures:

- Palantir: ontology plus governed human/agent operations.
- Writer: Knowledge Graph plus agents and applications.
- Microsoft: SharePoint/Graph plus Copilot Studio.
- Atlassian: Teamwork Graph plus Rovo agents.
- ServiceNow/Moveworks: enterprise knowledge, employee assistant, and workflow actions.
- Salesforce: Data Cloud plus Agentforce.
- Glean and Dust: federated company context plus agents.
- Guru: verified shared knowledge plus knowledge agents, with more limited operational write-back.

[established] The mainstream architecture is federation, not “move everything into one vault.” Enterprises want existing sources to remain authoritative while a knowledge layer resolves identity, permissions, meaning, provenance, and retrieval across them.

[emerging] A transparent shared vault can still differentiate where auditability and human editability matter. But it should appear as the controlled memory substrate behind a workflow, not as a demand that the enterprise abandon Microsoft, Google, Salesforce, Jira, Slack, or its document-management system.

D. Consulting market by segment

D1. Market-size integrity

[established] There is no clean, authoritative public market size for “knowledge-management consulting by company size.” Commercial market reports frequently mix KM software, enterprise search, content management, business-process outsourcing, training, and consulting. Figures from different reports are therefore not safely additive.

[emerging] One commercial estimate placed the AI-consulting market at roughly $8.75 billion in 2024 and projected a 26.4% CAGR through 2030. This is a single commercial-research estimate, not an official statistic, and its category boundaries should be inspected before use in an investment model. [single-source] [low-authority] (Grand View Research, undated, https://www.grandviewresearch.com/industry-analysis/artificial-intelligence-ai-consulting-market-report)

[established] The addressable services pool surrounding the category is much larger. Gartner forecast approximately $1.73 trillion in worldwide IT-services spending for 2025, but only an unidentified fraction concerns AI enablement or KM. [single-source] (Gartner, 21 January 2025, https://www.gartner.com/en/newsroom/press-releases/2025-01-21-gartner-forecasts-worldwide-it-spending-to-grow-9-point-8-percent-in-2025)

[established] Demand for AI services is demonstrated more concretely by supplier bookings. Accenture announced a $3 billion, three-year AI investment in 2023 and reported more than $3 billion in generative-AI bookings for FY2024. These bookings are not equivalent to recognized revenue or KM work, but they demonstrate enterprise willingness to fund AI transformation. [single-source per claim] (Accenture, 13 June 2023, https://newsroom.accenture.com/news/2023/accenture-to-invest-3-billion-in-ai-to-accelerate-clients-reinvention; Accenture, 26 September 2024, https://newsroom.accenture.com/news/2024/accenture-reports-fourth-quarter-and-full-year-fiscal-2024-results)

D2. Small business: approximately 5-99 employees

Typical sellers:

- Solo AI consultants.
- Notion, Microsoft 365, or Google Workspace specialists.
- Managed-service providers.
- Automation agencies using Zapier, Make, n8n, or CRM tools.
- Independent KM trainers and documentation specialists.

What buyers purchase:

- A single automation.
- CRM cleanup.
- SOP generation.
- Meeting capture.
- Search across a small document set.
- Basic chatbot or internal assistant.
- Tool setup and employee training.

Commercial structure: fixed packages, day rates, and small retainers.

Indicative engagement range: €2,000-€20,000 for a tightly scoped setup; €500-€5,000 monthly support. [speculative] [no source found] These are planning bands, not audited market medians.

Main problem: low willingness to fund a separate knowledge layer. The solution normally has to pay for itself through immediate owner time, sales, support, or administrative savings.

D3. Mid-market: approximately 100-999 employees

Typical sellers:

- KM and digital-workplace boutiques.
- Microsoft/Atlassian/ServiceNow partners.
- Data and AI engineering consultancies.
- Fractional CIO/CDO/CAIO advisers.
- Larger regional systems integrators.
- Specialist automation agencies moving upstream.

What buyers purchase:

- Enterprise search over several systems.
- Service or sales knowledge.
- AI-readiness and knowledge audits.
- Copilot/agent pilots.
- Permission and data-governance work.
- Workflow automation with human review.
- Managed retrieval and evaluation layers.

Indicative engagement range: €25,000-€250,000 for an implementation; €3,000-€25,000 monthly for managed operation and iteration. [speculative] [no source found]

Why this is the attractive segment: the organization has enough fragmentation and repeated labour to create ROI, but may not have the internal platform team or procurement complexity of a global enterprise. This is an inference, not a published market statistic. [emerging]

D4. Enterprise: 1,000-plus employees

Typical sellers:

- Accenture, Deloitte, PwC, EY, KPMG.
- IBM Consulting, Capgemini, Cognizant, TCS, Infosys, Wipro.
- McKinsey/QuantumBlack, BCG X, Bain.
- Large cloud and software implementation partners.
- Specialized KM firms embedded within larger transformation programs.

What buyers purchase:

- AI/data strategy and operating models.
- Security and governance.
- Copilot or agent-platform deployment.
- Search/knowledge integration.
- Data modernization.
- Change management and training.
- Function-specific transformation.
- Managed services.

Indicative engagements: €250,000 to multiple millions. [speculative] [no source found] Global transformations can exceed this substantially, but no defensible KM-only median was found.

Enterprise difficulty: the need is larger, but procurement, security review, stakeholder count, integration burden, and incumbent competition are also larger.

D5. What consulting companies are worth

The best disclosed comparisons are broader digital/data/AI services firms, not pure KM boutiques.

1. Perficient

EQT agreed to acquire Perficient for approximately $3 billion in 2024. [single-source] (EQT, 5 May 2024, https://eqtgroup.com/news/eqt-to-acquire-perficient-in-approximately-usd-3-billion-all-cash-transaction-2024-05-05)

Approximate sales multiple: about 3.3 times FY2023 revenue, based on roughly $907 million of reported revenue. The exact enterprise-value bridge and revenue basis should be rechecked before investment use. [single-source] [no independently verified calculation]

2. Thoughtworks

Apax agreed to acquire Thoughtworks for an enterprise value of approximately $1.75 billion in 2024. [single-source] (Thoughtworks, 5 August 2024, https://www.thoughtworks.com/en-us/about-us/news/2024/thoughtworks-to-be-acquired-by-apax-funds-for-usd1-75-billion)

Approximate sales multiple: roughly 1.5-1.7 times recent annual revenue, depending on the period selected. [single-source] [no independently verified calculation]

3. WNS

Capgemini agreed to acquire WNS for approximately $3.3 billion in 2025. WNS combines operations, analytics, and technology services, making it relevant to AI-enabled service delivery but not pure KM. [single-source] (Capgemini, 7 July 2025, https://www.capgemini.com/news/press-releases/capgemini-to-acquire-wns-to-create-a-global-leader-in-intelligent-operations/)

Approximate sales multiple: around 2.5-2.7 times recent annual revenue. [single-source] [no independently verified calculation]

4. Hakkoda

IBM announced the acquisition of data-and-AI consultancy Hakkoda in April 2025; terms were not disclosed. [single-source] (IBM, 7 April 2025, https://newsroom.ibm.com/2025-04-07-ibm-to-acquire-hakkoda-inc-to-expand-data-and-ai-consulting-capabilities)

Lesson: strategic buyers are acquiring scarce data/AI implementation capability, but the absence of disclosed prices prevents a clean boutique multiple.

D6. Multiple interpretation

[established] The disclosed services comparisons cluster around roughly 1.5-3.3 times revenue, not SaaS-like recurring-revenue multiples. They also involve scale, customer relationships, and strategic capabilities unavailable to a small agency.

[emerging] A consultancy can obtain a higher strategic value when it owns:

- Reusable software or proprietary accelerators.
- Recurring managed-service revenue.
- Deep access to a regulated vertical.
- A scarce implementation partnership.
- A repeatable data model or evaluation system.
- A customer base valuable to a platform acquirer.

[emerging] The “AI agency roll-up” trend is real in the broad sense that consultancies, technology firms, and private-equity owners are buying data, cloud, and AI specialists. Evidence for a standardized roll-up model with consistently disclosed AI-agency multiples is thin because most boutique transactions do not disclose consideration. Calling it a mature valuation category would be [speculative].

E. Demand split: automation versus knowledge layer

E1. What the spend data can and cannot tell us

[established] Gartner forecast $644 billion in worldwide generative-AI spending in 2025, but approximately $398 billion was attributed to devices and $181 billion to servers. Software and services together were a much smaller part of the headline total. The number therefore should not be cited as corporate spending on AI automation or knowledge systems. [single-source] (Gartner, 31 March 2025, https://www.gartner.com/en/newsroom/press-releases/2025-03-31-gartner-forecasts-worldwide-generative-ai-spending-to-total-644-billion-in-2025)

[established] Menlo Ventures estimated enterprise generative-AI spending of $13.8 billion in 2024, but its model/infrastructure/application taxonomy does not provide a clean automation-versus-knowledge split. It is a venture-firm estimate rather than audited national statistics. [single-source] (Menlo Ventures, 20 November 2024, https://menlovc.com/perspective/2024-the-state-of-generative-ai-in-the-enterprise/)

[established] McKinsey’s 2025 survey found generative-AI use concentrated in functions including marketing and sales, product/service development, service operations, software engineering, and IT. These are predominantly task or function-oriented adoption doors, even when knowledge retrieval is an enabling component. [single-source] (McKinsey, 12 March 2025, https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)

Verdict on share: no defensible percentage was found for “automation versus knowledge infrastructure.” [no source found] Any precise split would create false precision.

E2. Practical demand segmentation

Small business:

- Buys: immediate automation, lead handling, content, bookkeeping, customer support, scheduling.
- Rarely buys: a separately governed company knowledge layer.
- Knowledge becomes acceptable when bundled into the automation.

Mid-market:

- Buys: automation plus integration because fragmented Slack, email, CRM, drives, and ticketing now cause visible rework.
- Can buy: search, onboarding, support knowledge, proposal memory, or an agent-grounding layer.
- Best buyer: the executive owning the workflow and its cost, not a generic “Head of Knowledge.”

Enterprise:

- Buys: function-specific copilots, platform licenses, data foundations, governance, search, and transformation programs.
- Frequently owns knowledge infrastructure indirectly through Microsoft, ServiceNow, Salesforce, Atlassian, or a data platform.
- Will fund an independent layer when federation, relevance, governance, or cross-suite access is materially better.

E3. Buyer-persona map

CIO / CTO:

Owns architecture, identity, integration, security, digital workplace, and often enterprise search.

Likely message: reduce duplicated systems and make existing investments usable by humans and agents.

Resistance: another repository and another vendor.

CAIO / CDO:

Owns AI platform, data readiness, model governance, and sometimes enterprise agents.

Likely message: agents cannot operate reliably without governed context, provenance, and evaluation.

Resistance: “memory” looks like a feature of the chosen cloud/model platform.

CHRO / Head of Employee Experience / L&D:

Owns onboarding, learning, employee self-service, and expertise transfer.

Likely message: shorten time-to-competence and reduce dependence on a few experts.

Resistance: productivity benefits may accrue to business units while HR pays.

COO / Operations:

Owns process consistency, rework, handoffs, and service performance.

Likely message: operational memory that survives employee turnover and drives repeatable execution.

Resistance: abstract knowledge programs without a process metric.

VP Support / Customer Service:

Owns resolution time, deflection, escalation, quality, and KCS.

Likely message: capture knowledge while solving cases and ground agents in verified answers.

This is one of the clearest budget owners.

Revenue Operations / Sales Enablement:

Owns playbooks, proposal reuse, call intelligence, and CRM quality.

Likely message: turn account and delivery history into next actions and reusable evidence.

Legal / Risk / Compliance:

Owns records, retention, policy interpretation, and auditability.

Likely message: cited, permissioned answers and controlled write-back.

Resistance: hallucination, residency, discovery, and uncontrolled retention.

[established] “No clear owner” is a real problem for a generic organizational brain. It is not a problem once the offer is attached to a function with a metric and budget.

E4. Purchase sequencing

[emerging] The dominant sequence appears to be:

1. Buy or pilot a specific automation/assistant.
2. Discover that answers and actions fail because company context is fragmented, stale, or inaccessible.
3. Add retrieval, identity, permissions, taxonomy/entity resolution, and evaluation.
4. Expand the knowledge layer across additional workflows.

Moveworks’ path from IT/employee support toward broader enterprise search and automation, Glean’s move from search to assistants and agents, and Microsoft’s absorption of Topics-style knowledge discovery into Copilot all fit this sequence. The conclusion is an inference across product histories, not a measured universal law. (ServiceNow, 10 March 2025, https://www.servicenow.com/company/media/press-room/servicenow-to-acquire-moveworks.html; Glean, 25 February 2025, https://www.glean.com/blog/introducing-glean-agents; Microsoft, 22 February 2024, https://learn.microsoft.com/en-us/viva/topics/changes-coming-to-topics)

Alternative sequence:

1. Establish a governed ontology/data platform.
2. Deploy one valuable workflow on it.
3. Reuse the platform for additional agents.

Palantir represents this foundation-plus-use-case-together motion. It is more appropriate where operational entities and decisions need stronger structure than document retrieval. [single-source] [low-authority] (Palantir, undated, https://www.palantir.com/platforms/aip/)

Conclusion: a corporate second brain is usually an enabling layer, upsell, or byproduct of a funded workflow—not the first abstract purchase.

F. Failure modes and lessons

F1. Microsoft Viva Topics

[established] Microsoft retired Viva Topics on 22 February 2025 after announcing the change a year earlier. Microsoft directed customers toward newer AI-powered experiences and other Microsoft 365 knowledge capabilities. [single-source] (Microsoft, updated 22 February 2024, https://learn.microsoft.com/en-us/viva/topics/changes-coming-to-topics)

Lesson:

- Automatic topic extraction alone was not a durable standalone product.
- A separately licensed knowledge-discovery layer was vulnerable to absorption into Copilot and SharePoint.
- Retirement does not show that knowledge is valueless; it shows that the capability may become bundled infrastructure.

F2. Meta Workplace

[established] Meta announced that Workplace would be phased out, with normal service ending in 2025 and a read/export period following. Reuters reported that Meta was refocusing on AI and metaverse work. [single-source] (Reuters, 14 May 2024, https://www.reuters.com/technology/meta-shut-workplace-app-focus-building-ai-metaverse-2024-05-14/)

Lesson:

- An enterprise social workspace is not automatically a durable knowledge system.
- Broad collaboration products face strong suite competition.
- Customers bear significant switching risk when company memory lives inside a non-core vendor.

F3. IBM Watson Health / Watson for Oncology

[established] IBM sold Watson Health’s data and analytics assets to Francisco Partners in 2022. [single-source] [older source] (IBM, 21 January 2022, https://newsroom.ibm.com/2022-01-21-Francisco-Partners-to-Acquire-Healthcare-Data-and-Analytics-Assets-from-IBM)

Lesson:

- A large corpus and prestigious AI branding do not guarantee reliable domain judgment.
- Knowledge systems in high-stakes environments need evaluation against real decisions, not demonstration questions.
- This was healthcare decision support, not a general corporate KM product, so it is an analogy rather than direct market evidence.

F4. Air Canada chatbot

[established] British Columbia’s Civil Resolution Tribunal held Air Canada responsible after its chatbot supplied incorrect information about bereavement fares. (British Columbia Civil Resolution Tribunal, 14 February 2024, https://www.canlii.org/en/bc/bccrt/doc/2024/2024bccrt149/2024bccrt149.html)

Lesson:

- The organization remains responsible for answers produced by its interface.
- Retrieval must preserve policy dates, exceptions, and authority.
- “The bot said it” is not a governance defence.
- Citation and escalation are product requirements, not optional UX.

F5. Builder.ai

[established] Builder.ai entered insolvency proceedings in May 2025 after previously reaching a reported valuation above $1 billion. It was an AI software-development company rather than KM, but its collapse is relevant to AI-services valuation: a services-heavy operation presented as scalable AI software can receive a software valuation and then unwind violently when economics or representations fail. [single-source] (Reuters, 20 May 2025, https://www.reuters.com/technology/artificial-intelligence/microsoft-backed-builderai-files-insolvency-2025-05-20/)

Lesson:

- Measure how much work is actually performed by repeatable software.
- Do not value consulting revenue as SaaS merely because AI is in the delivery loop.
- Expose human operations, gross margin, model cost, and retention honestly.

F6. General failure modes

1. Repository-first architecture

Failure: creates another place employees must maintain.

Countermeasure: federate existing sources; create new canonical records only where ownership is explicit.

2. Search without an operational outcome

Failure: usage looks positive, but nobody can connect it to revenue, cost, risk, or cycle time.

Countermeasure: launch with a workflow metric.

3. Generated knowledge without stewardship

Failure: agents multiply plausible but conflicting summaries.

Countermeasure: distinguish source records, derived summaries, hypotheses, and approved policies.

4. Permissions reconstructed after ingestion

Failure: retrieval leaks material or becomes unusably restrictive.

Countermeasure: inherit source permissions and test access as part of every evaluation set.

5. Timeless embeddings

Failure: an obsolete policy and its replacement remain equally retrievable.

Countermeasure: model effective dates, supersession, source authority, and deletion.

6. Human contribution as unpaid extra work

Failure: the system becomes stale.

Countermeasure: capture knowledge in the workflow and return immediate benefit to the contributor.

7. Autonomous write-back without transaction controls

Failure: duplicate actions, overwritten records, and self-reinforcing false memories.

Countermeasure: approvals by action risk, idempotency keys, append-only audit, versioning, rollback, and conflict detection.

8. Broad rollout before value proof

Failure: high integration and change-management cost precedes evidence.

Countermeasure: one workflow, one owner, one corpus, one success metric, then expansion.

F7. Emerging organizational memory for agents

[emerging] Letta’s $10 million seed round around the MemGPT approach demonstrated investor interest in explicit long-lived agent memory. The amount is meaningful for infrastructure experimentation but small compared with the capital behind full enterprise AI platforms. [single-source] (Letta, September 2024, https://www.letta.com/blog/letta-seed-round)

[emerging] Amazon Bedrock AgentCore introduced managed memory as part of a broader agent deployment and operations platform. [single-source] [low-authority] (AWS, 16 July 2025, https://aws.amazon.com/blogs/aws/introducing-amazon-bedrock-agentcore-securely-deploy-and-operate-ai-agents-at-any-scale/)

[emerging] Google Vertex AI Agent Engine exposes a Memory Bank for generating and retrieving long-term memories. [single-source] [low-authority] [no publication date] (Google Cloud, undated, https://cloud.google.com/vertex-ai/generative-ai/docs/agent-engine/memory-bank/overview)

[emerging] OpenAI’s Responses API and Agents SDK made stateful, tool-using agent applications easier to build, although they do not by themselves provide a governed company-wide institutional memory. [single-source] [low-authority] (OpenAI, 11 March 2025, https://openai.com/index/new-tools-for-building-agents/)

Companies also associated with this layer include Zep, Mem0, Cognee, and Supermemory. Their latest funding, enterprise traction, and August 2026 status were not reliably verified here. [no source found]

Verdict on the trend:

- Agent memory is becoming a recognized technical primitive.
- Hyperscalers will probably commoditize basic session and user memory.
- The reopened commercial opportunity is organizational memory with permissions, provenance, time, evaluation, and safe operations.
- The agent door makes the need more urgent: a human can notice that a wiki page is stale; an autonomous agent may execute it.

G. Strategic recommendations

G1. Option 1: “Agent-Readiness and Institutional Memory Audit”

Offer:

- Two-week fixed-scope audit.
- Map one workflow, its sources, people, permissions, repeated questions, contradictions, and knowledge-loss points.
- Produce a live demonstration using the client’s data.
- Deliver an evaluation set and financial case for one pilot.

Proposed price: €7,500-€15,000. This is a recommended price, not a claimed market median.

Best buyer: COO, CIO, support leader, professional-services leader, or RevOps.

Advantages:

- Easy to understand.
- Paid discovery.
- Exposes whether the client has a real problem.
- Creates the data required to price a pilot.
- Avoids promising company-wide transformation immediately.

Risks:

- Can become generic consulting.
- Client may take the blueprint to a larger implementer.
- Needs a clear expansion path.

G2. Option 2: “One Workflow, One Memory” implementation

Named wedges:

- Support Memory: verified answers plus case-resolution agent.
- Delivery Memory: client history, decisions, reusable work, risks, and proposal agent.
- Engineering Memory: decisions, incidents, code/docs, and onboarding agent.
- Compliance Memory: cited policies, effective dates, evidence, and controlled escalation.
- Revenue Memory: CRM, calls, proposals, commitments, and follow-up preparation.

Proposed price: €25,000-€60,000 for a six-to-eight-week pilot; €4,000-€12,000 per month for managed operation. These are recommended commercial bands, not market statistics.

Advantages:

- Has an owner and metric.
- Demonstrates the existing system on real data.
- Produces reusable connectors, evaluations, and governance.
- Can expand into a broader institutional layer.

Risks:

- Integration work can consume the margin.
- Each customer may demand unique sources.
- Security and permissions can dominate the schedule.

G3. Option 3: Microsoft/Atlassian/ServiceNow implementation partner

Offer:

- Build the knowledge and governance layer around the client’s incumbent suite.
- Use the lab’s own system as the methodology and evaluation harness, not necessarily the deployed substrate.

Advantages:

- Lower platform resistance.
- Existing identities, permissions, and procurement.
- Easier explanation to CIOs.

Risks:

- Lower product multiple.
- Dependency on vendor roadmap and partner rules.
- The lab may become an implementation agency.
- Microsoft or Atlassian can absorb differentiating features.

G4. Option 4: Managed “Company Memory Plane”

Offer:

- Federation over existing systems.
- Transparent canonical records for decisions, entities, policies, and commitments.
- Human and agent read/write access.
- Provenance, temporal validity, permissions, evaluation, audit, and rollback.
- Vendor-neutral model and agent support.

Advantages:

- Closest to the lab’s real differentiated asset.
- Recurring managed-service potential.
- Builds reusable intellectual property.
- Addresses a growing agent-governance problem.

Risks:

- Difficult category creation.
- High trust and security burden.
- Incumbents can bundle basic retrieval and memory.
- Dangerous if sold before proving a workflow.

G5. Option 5: Methodology and training only

Offer:

- Knowledge audit.
- KCS-inspired workflow redesign.
- AI-era institutional-memory training.
- Governance and contribution routines.

Advantages:

- Fastest delivery.
- Low engineering burden.
- Useful lead generator.

Risks:

- Weak recurring revenue.
- Lower valuation.
- Results depend heavily on client execution.
- BASB/Zettelkasten language may sound individual and tactical to enterprise buyers.

G6. What not to do

- Do not sell “Obsidian for the whole company.”
- Do not ask a company to migrate everything before proving value.
- Do not compete with Microsoft or Glean on generic search.
- Do not lead with number of notes, embeddings, models, or machines.
- Do not let agents silently rewrite shared memory.
- Do not promise an autonomous workforce before demonstrating authoritative grounding.
- Do not call services revenue SaaS revenue.

G7. The experiment that would settle the objection

Run three matched paid pilots in organizations with 100-1,000 knowledge workers.

For one workflow in each client, compare:

A. Automation-only agent using the client’s existing search and documents.

B. The same agent with the governed knowledge layer: source authority, entity links, effective dates, verified summaries, permission inheritance, and captured corrections.

Measure for 8-12 weeks:

- Median task-completion time.
- Answer correctness against a human-reviewed set.
- Escalation rate.
- Rework and duplicate-work rate.
- Percentage of answers with valid source support.
- Permission violations.
- Stale-source incidents.
- Human maintenance minutes per successful task.
- Repeat usage after 30 and 90 days.
- Financial value per active user or completed workflow.

Decision rule:

- If B does not produce a material improvement after including maintenance cost, the objection wins for that workflow.
- If B materially improves accuracy, task time, or rework and the benefit survives 90 days, the knowledge layer has earned expansion.
- If both fail, the workflow or automation is the problem.
- If A and B perform equally, suite search is sufficient and a separate layer should not be sold.

G8. Final recommendation

[emerging] Recommendation: sell “agent-ready institutional memory” to mid-market professional-services, support, and operational organizations through a paid audit followed by one workflow pilot. Keep the company-wide shared memory as the architecture, but sell the business outcome at the door.

Recommended first offer:

“Delivery Memory Pilot: in eight weeks, make every past decision, commitment, reusable artifact, and account fact available—with sources—to the humans and agents delivering the next client engagement.”

Why this option:

- It uses the lab’s working multi-machine system rather than inventing a new product.
- Mid-market clients have real fragmentation but fewer procurement barriers than global enterprises.
- Professional-service work contains expensive reusable knowledge and visible rework.
- A read/write memory is more valuable there than a read-only chatbot.
- The audit and pilot create consulting revenue while identifying the genuinely reusable product.

Confidence: [emerging]

Main assumption: the lab can demonstrate a material improvement in one workflow after counting integration and human-maintenance costs, while preserving source permissions and provenance. If that assumption fails in three well-chosen pilots, do not build a horizontal company-brain product; become an implementation/evaluation partner for incumbent platforms instead.

H. Sources

1. Alavi, Maryam; Leidner, Dorothy. “Review: Knowledge Management and Knowledge Management Systems.” MIS Quarterly, June 2001.
https://doi.org/10.2307/3250961

2. McDermott, Richard. “Why Information Technology Inspired but Cannot Deliver Knowledge Management.” California Management Review, Summer 1999.
https://doi.org/10.2307/41166012

3. McKinsey Global Institute. “The social economy.” July 2012.
https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-social-economy

4. Brynjolfsson, Erik; Li, Danielle; Raymond, Lindsey. “Generative AI at Work.” Quarterly Journal of Economics, 2025.
https://academic.oup.com/qje/article/140/2/889/7990658

5. OpenAI. “Morgan Stanley.” 26 March 2024.
https://openai.com/index/morgan-stanley/

6. Glean. Booking.com customer case study. Undated.
https://www.glean.com/customers/booking-com

7. Gartner. “Gartner Predicts 30% of Generative AI Projects Will Be Abandoned After Proof of Concept by End of 2025.” 29 July 2024.
https://www.gartner.com/en/newsroom/press-releases/2024-07-29-gartner-predicts-30-percent-of-generative-ai-projects-will-be-abandoned-after-proof-of-concept-by-end-of-2025

8. Forrester. “The Forrester Wave: Cognitive Search, Q4 2021.” Q4 2021.
https://www.forrester.com/report/the-forrester-wave-cognitive-search-q4-2021/RES176208

9. ServiceNow. “ServiceNow to acquire Moveworks.” 10 March 2025.
https://www.servicenow.com/company/media/press-room/servicenow-to-acquire-moveworks.html

10. Writer. Series C announcement. 12 November 2024.
https://writer.com/blog/series-c/

11. Writer. Knowledge Graph product page. Undated.
https://writer.com/product/knowledge-graph/

12. Palantir. Artificial Intelligence Platform. Undated.
https://www.palantir.com/platforms/aip/

13. Atlassian. “Introducing Atlassian Rovo.” 30 April 2024.
https://www.atlassian.com/blog/announcements/introducing-atlassian-rovo-ai

14. Microsoft. Microsoft Copilot Studio product page. Undated.
https://www.microsoft.com/en-us/microsoft-copilot/microsoft-copilot-studio

15. Glean. “Introducing Glean Agents.” 25 February 2025.
https://www.glean.com/blog/introducing-glean-agents

16. Lucidworks. $100 million funding announcement. 12 August 2019.
https://lucidworks.com/post/lucidworks-raises-100m/

17. Guru. Pricing page. Undated.
https://www.getguru.com/pricing

18. Slite. Pricing page. Undated.
https://slite.com/pricing

19. Slab. Pricing page. Undated.
https://slab.com/pricing/

20. Tettra. Pricing page. Undated.
https://tettra.com/pricing

21. Capacity. Tettra acquisition announcement. 1 February 2024.
https://capacity.com/blog/capacity-acquires-tettra/

22. Forbes. Rewind funding and valuation report. 4 May 2023.
https://www.forbes.com/sites/alexkonrad/2023/05/04/rewind-ai-raises-12-million-at-350-million-valuation/

23. TechCrunch. Mem funding report. 1 November 2022.
https://techcrunch.com/2022/11/01/mem-a-work-focused-app-with-ai-smarts-raises-23-5m/

24. Consortium for Service Innovation. “KCS Practices Guide v6.” 2016.
https://library.serviceinnovation.org/KCS_Practices_Guide_v6

25. ISO. ISO 30401:2018 Knowledge management systems. November 2018.
https://www.iso.org/standard/68683.html

26. APQC. Knowledge Management practice page. Undated.
https://www.apqc.org/expertise/knowledge-management

27. Grand View Research. Artificial Intelligence Consulting Market report page. Undated.
https://www.grandviewresearch.com/industry-analysis/artificial-intelligence-ai-consulting-market-report

28. Gartner. Worldwide IT spending forecast. 21 January 2025.
https://www.gartner.com/en/newsroom/press-releases/2025-01-21-gartner-forecasts-worldwide-it-spending-to-grow-9-point-8-percent-in-2025

29. Accenture. $3 billion AI investment announcement. 13 June 2023.
https://newsroom.accenture.com/news/2023/accenture-to-invest-3-billion-in-ai-to-accelerate-clients-reinvention

30. Accenture. FY2024 results. 26 September 2024.
https://newsroom.accenture.com/news/2024/accenture-reports-fourth-quarter-and-full-year-fiscal-2024-results

31. EQT. Perficient acquisition announcement. 5 May 2024.
https://eqtgroup.com/news/eqt-to-acquire-perficient-in-approximately-usd-3-billion-all-cash-transaction-2024-05-05

32. Thoughtworks. Apax acquisition announcement. 5 August 2024.
https://www.thoughtworks.com/en-us/about-us/news/2024/thoughtworks-to-be-acquired-by-apax-funds-for-usd1-75-billion

33. Capgemini. WNS acquisition announcement. 7 July 2025.
https://www.capgemini.com/news/press-releases/capgemini-to-acquire-wns-to-create-a-global-leader-in-intelligent-operations/

34. IBM. Hakkoda acquisition announcement. 7 April 2025.
https://newsroom.ibm.com/2025-04-07-ibm-to-acquire-hakkoda-inc-to-expand-data-and-ai-consulting-capabilities

35. Gartner. 2025 generative-AI spending forecast. 31 March 2025.
https://www.gartner.com/en/newsroom/press-releases/2025-03-31-gartner-forecasts-worldwide-generative-ai-spending-to-total-644-billion-in-2025

36. Menlo Ventures. “2024: The State of Generative AI in the Enterprise.” 20 November 2024.
https://menlovc.com/perspective/2024-the-state-of-generative-ai-in-the-enterprise/

37. McKinsey. “The State of AI.” 12 March 2025.
https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai

38. Microsoft. “Changes coming to Topics.” Updated 22 February 2024.
https://learn.microsoft.com/en-us/viva/topics/changes-coming-to-topics

39. Reuters. Meta Workplace shutdown report. 14 May 2024.
https://www.reuters.com/technology/meta-shut-workplace-app-focus-building-ai-metaverse-2024-05-14/

40. IBM. Watson Health asset-sale announcement. 21 January 2022.
https://newsroom.ibm.com/2022-01-21-Francisco-Partners-to-Acquire-Healthcare-Data-and-Analytics-Assets-from-IBM

41. British Columbia Civil Resolution Tribunal. Moffatt v. Air Canada, 2024 BCCRT 149. 14 February 2024.
https://www.canlii.org/en/bc/bccrt/doc/2024/2024bccrt149/2024bccrt149.html

42. Reuters. Builder.ai insolvency report. 20 May 2025.
https://www.reuters.com/technology/artificial-intelligence/microsoft-backed-builderai-files-insolvency-2025-05-20/

43. Letta. Seed-round announcement. September 2024.
https://www.letta.com/blog/letta-seed-round

44. AWS. Amazon Bedrock AgentCore announcement. 16 July 2025.
https://aws.amazon.com/blogs/aws/introducing-amazon-bedrock-agentcore-securely-deploy-and-operate-ai-agents-at-any-scale/

45. Google Cloud. Vertex AI Memory Bank documentation. Undated.
https://cloud.google.com/vertex-ai/generative-ai/docs/agent-engine/memory-bank/overview

46. OpenAI. “New tools for building agents.” 11 March 2025.
https://openai.com/index/new-tools-for-building-agents/

🧒 Простыми словами: компаниям нужен не “ещё один умный склад заметок”. Им нужен сотрудник-робот, который знает, где лежит правда, показывает источник, не путает старое с новым и безопасно делает работу. Поэтому продавать надо не “второй мозг”, а конкретный результат: быстрее обслужили клиента, не потеряли решение, не повторили работу. Сам мозг остаётся двигателем под капотом.

End date: 30 August 2026, Europe/London. Exact time not checked.
