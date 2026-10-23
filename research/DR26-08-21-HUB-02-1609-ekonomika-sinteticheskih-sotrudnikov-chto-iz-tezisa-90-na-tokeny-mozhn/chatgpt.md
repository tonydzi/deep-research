# DR26-08-21-HUB-02-1609 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Deep research report: the unit economics of “synthetic employees”

Research cut-off: 21 August 2026.

Classification used throughout:

- MEASURED: observed in administrative data, controlled studies, company filings or vendor usage telemetry.
- ESTIMATED: transparent calculation from measured inputs.
- SPECULATIVE: forecast, planning assumption or untested organizational thesis.

## Executive verdict

The “replace 90% of routine knowledge-work staff” thesis is not defensible in 2026. The strongest current evidence supports substantial task-level substitution in narrow, measurable workflows—FAQs, simple customer-service contacts, HR transactions, drafting and some coding—not 90% elimination of whole jobs. Anthropic found only about 4% of occupations using Claude across at least 75% of their tasks, and its observed usage leaned 57% toward augmentation versus 43% automation; this measures usage rather than successful completion but strongly contradicts universal whole-job replacement ([Anthropic Economic Index](https://www.anthropic.com/news/the-anthropic-economic-index)).

The cost thesis is only half right:

- AI inference or a power-user subscription really can cost approximately $100–$250 per active user per month: Anthropic reports $150–$250 per Claude Code developer per month, while OpenAI reports approximately $100–$200 for Codex, both with large variance ([Anthropic](https://code.claude.com/docs/en/costs), [OpenAI](https://help.openai.com/en/articles/20001106-codex-rate-card)).
- That model bill is not the cost of an employee-equivalent unit. Once one to two hours of daily expert review, infrastructure, evaluation, security and rework are included, a plausible supervised-agent stack is approximately $1,900–$4,000 per month under the assumptions below.
- A technical Bay Area employee is more likely to cost approximately $20,700 per month in compensation and benefits before general corporate overhead, not $10,000–$15,000. A US national technical-worker proxy is approximately $14,800 per month ([BLS SF wages](https://www.bls.gov/regions/west/news-release/occupationalemploymentandwages_sanfrancisco.htm), [BLS compensation](https://www.bls.gov/news.release/archives/ecec_03202026.htm)).
- $800–$1,500 per month is possible for entry-level or low-cost outsourcing-provider labor, and is close to the Philippines median technical compensation before full load, but is not a representative fully loaded mid-level technical-worker cost in India or Eastern Europe ([Infosys compensation](https://www.levels.fyi/companies/infosys/salaries/software-engineer), [India market](https://www.levels.fyi/t/software-engineer/locations/india), [Philippines market](https://www.levels.fyi/t/software-engineer/locations/philippines)).

The most supportable org-design thesis is therefore:

> AI-native businesses can operate with fewer people, but the proven design is “humans with agents and strong evaluation systems,” not autonomous synthetic employees replacing 90% of staff.

That interpretation is consistent with controlled productivity evidence, field deployments and current agent-reliability benchmarks ([NBER customer-support study](https://www.nber.org/papers/w31161), [METR developer study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/), [TheAgentCompany](https://arxiv.org/abs/2412.14161)).

---

## 1. Defensible cost-per-FTE comparison

### Method and comparability warning

“Knowledge worker” is too broad for a single honest salary comparison. A customer-service representative, accountant and software engineer have materially different costs. Because the thesis explicitly invokes Claude Code and Codex, the table uses computer/software workers as the closest cross-country proxy.

US figures come from probability-sample BLS data. International figures use Levels.fyi’s self-reported all-level software-engineer medians; these are likely skewed toward technology companies and are not statistically representative national surveys ([BLS methodology](https://www.bls.gov/news.release/archives/ecec_03202026.htm), [Levels Poland](https://www.levels.fyi/t/software-engineer/locations/poland), [Levels Romania](https://www.levels.fyi/t/software-engineer/locations/romania), [Levels India](https://www.levels.fyi/t/software-engineer/locations/india), [Levels Philippines](https://www.levels.fyi/t/software-engineer/locations/philippines)).

The “planning TCO” column adds:

- 10% for recruiting, equipment, local HR and general employment administration—an ESTIMATED scenario, not a measured universal ratio.
- A further 15% buyer-side supervision/QC burden for offshore workers—also an ESTIMATED sensitivity assumption, because research does not supply a reliable universal percentage.

### Monthly comparison in US dollars

| Worker/stack | Compensation plus statutory benefits | Planning TCO | Evidence and confidence |
|---|---:|---:|---|
| SF Bay Area technical worker | $20,700/month | $22,700/month | MEASURED wages, ESTIMATED conversion. BLS reports $80.51/hour for SF computer/mathematical work versus $57.73 nationally; BLS management/professional compensation was $77.39/hour against $52.28 in wages, a 1.480 benefit multiplier ([BLS SF](https://www.bls.gov/regions/west/news-release/occupationalemploymentandwages_sanfrancisco.htm), [BLS ECEC](https://www.bls.gov/news.release/archives/ecec_03202026.htm)). High confidence. |
| US remote proxy | $14,800/month | $16,300/month | MEASURED national wage, ESTIMATED load. “Remote” is not separately measured; national computer/mathematical wages are used ([BLS SF/national table](https://www.bls.gov/regions/west/news-release/occupationalemploymentandwages_sanfrancisco.htm)). Medium-high confidence. |
| Eastern Europe: Romania | $3,600/month | $4,600/month | Levels.fyi median RON191,296/year; Romania’s ordinary employer work-insurance contribution is 2.25% ([Levels](https://www.levels.fyi/t/software-engineer/locations/romania), [PwC](https://taxsummaries.pwc.com/romania/individual/other-taxes)). Converted near RON4.49/USD ([FX source](https://www.cursvalutar.ro/curs-usd/)). Medium-low confidence. |
| Eastern Europe: Poland | $7,100–$7,300/month | $9,000–$9,200/month | Levels.fyi median PLN263,985/year; Polish employer contributions generally range from 19.21% to 22.41%, subject to contribution caps ([Levels](https://www.levels.fyi/t/software-engineer/locations/poland), [PwC](https://taxsummaries.pwc.com/poland/individual/other-taxes)). Converted near PLN3.70/USD ([FX source](https://www.exchange-rates.org/exchange-rate-history/usd-pln)). Medium-low confidence. |
| India | $2,700–$2,900/month | $3,500–$3,700/month | Levels.fyi median ₹2,963,901/year, converted near ₹95.72/USD ([Levels](https://www.levels.fyi/t/software-engineer/locations/india), [FX](https://tradingeconomics.com/india/currency)). Employer EPF is normally 12% of basic wages and allowances—not necessarily 12% of total compensation—plus 0.5% EDLI, so a 6–12% effective load is an estimate ([EPFO FAQ](https://www.epfindia.gov.in/site_en/FAQ.php/Resource/site_docs/PDFs/Downloads_PDFs/Feedback.php)). Medium-low confidence. |
| Philippines | $1,300–$1,350/month | $1,650–$1,700/month | Levels.fyi median ₱913,934/year, converted near ₱61.81/USD ([Levels](https://www.levels.fyi/t/software-engineer/locations/philippines), [FX](https://www.poundsterlinglive.com/history/USD-PHP-2026)). Employer SSS is 10% of monthly salary credit capped at ₱35,000, while PhilHealth’s total premium is 5% subject to a ₱100,000 income ceiling ([SSS](https://www.sss.gov.ph/pay-contribution/), [PhilHealth](https://www.philhealth.gov.ph/advisories/2025/PA2025-0002.pdf)). Medium-low confidence. |
| AI model/subscription only | $100–$250/month typical coding usage | Not an FTE | MEASURED vendor telemetry: Claude Code $150–$250 and Codex $100–$200 per developer per month ([Anthropic](https://code.claude.com/docs/en/costs), [OpenAI](https://help.openai.com/en/articles/20001106-codex-rate-card)). High confidence on billing, no confidence on FTE equivalence. |
| Supervised agent stack | $1,900–$4,000/month | Not an FTE | ESTIMATED: $150–$250 inference, one to two review hours per workday at BLS’s $77.39/hour management/professional compensation rate, plus $50–$300 for infrastructure, logging and maintenance ([BLS](https://www.bls.gov/news.release/archives/ecec_03202026.htm), [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework)). Low confidence; workload-specific. |

The offshore numbers are direct-hire planning proxies, not agency quotes. An agency can quote a cheaper worker while adding margin, pooling management or assigning junior staff. For example, Levels.fyi reports Infosys software-engineer median compensation of ₹882,862/year, roughly $770 per month at August 2026 exchange rates, demonstrating that the lower end of the thesis is possible inside low-cost service providers—but not that it represents a mid-level independent technical employee ([Levels.fyi Infosys](https://www.levels.fyi/companies/infosys/salaries/software-engineer), [USD/INR](https://tradingeconomics.com/india/currency)).

For a less technical SF business/financial worker, BLS’s $60.95 hourly wage produces approximately $15,800 per month after applying BLS’s management/business compensation-to-wage ratio; thus $10,000–$15,000 is plausible for some professional roles, but low for Bay Area software or high-end technical staff ([BLS SF](https://www.bls.gov/regions/west/news-release/occupationalemploymentandwages_sanfrancisco.htm), [BLS compensation](https://www.bls.gov/news.release/archives/ecec_03202026.htm)).

---

## 2. The real cost of intensive AI-agent work

### Subscriptions

As of August 2026:

- Claude Pro costs $20 monthly or $200 annually; Claude Max 5x costs $100 monthly and Max 20x costs $200 monthly. These are individual plans with relative usage capacities, not guaranteed unlimited compute ([Anthropic pricing](https://support.claude.com/en/articles/11049762-choose-a-claude-plan)).
- ChatGPT Pro has $100 and $200 monthly tiers, providing respectively 5x and 20x Plus usage; both include Codex, but models retain separate usage allowances and can become temporarily unavailable when an allowance is exhausted ([OpenAI](https://help.openai.com/en/articles/9793128-what-is-chatgpt-pro)).
- Across enterprise deployments, Anthropic observes approximately $13 per active developer-day and $150–$250 per developer-month; 90% remain below $30 per active day, while agent teams consume about seven times the tokens of ordinary sessions ([Anthropic Claude Code costs](https://code.claude.com/docs/en/costs)).
- OpenAI reports an average Codex cost of approximately $100–$200 per developer-month, with model choice, concurrency, automations and fast mode creating substantial variation ([OpenAI rate card](https://help.openai.com/en/articles/20001106-codex-rate-card)).

### API token burn

No credible source establishes a universal “full AI employee workday” token count. Context length, repeated repository reads, caching, tool results, output length and concurrent agents dominate consumption.

Transparent ESTIMATED illustrations:

| Daily tokens | Claude Sonnet 5 | GPT-5.4 |
|---|---:|---:|
| 1M input + 0.1M output | $3/day | $4/day |
| 5M input + 0.5M output | $15/day | $20/day |
| 10M input + 1M output | $30/day | $40/day |

These calculations use Sonnet 5’s $2/million input and $10/million output prices and GPT-5.4’s $2.50/million input and $15/million output prices ([Anthropic](https://www.anthropic.com/claude/sonnet), [OpenAI](https://developers.openai.com/api/docs/models/gpt-5.4)). Caching can reduce Sonnet input costs by up to 90%; GPT-5.4 cached input costs $0.25/million, while very long GPT-5.4 contexts trigger higher rates ([Anthropic](https://www.anthropic.com/claude/sonnet), [OpenAI](https://developers.openai.com/api/docs/models/gpt-5.4)).

The $100–$250 model bill is therefore credible. Calling it a “$200 employee” is not.

### Hidden costs

A production agent requires some combination of:

- Human acceptance testing and exception handling.
- Rework after incorrect or incomplete output.
- Process documentation, evaluation datasets and regression tests.
- Data connectors and context preparation.
- Sandboxes, CI, logging, identity and access controls.
- Security, privacy and compliance review.
- Incident response and rollback mechanisms.
- Prompt/workflow maintenance after model or vendor changes.

NIST treats AI as a socio-technical system and explicitly recommends defined responsibilities for human-AI configurations, oversight, risk measurement and governance rather than treating the model as an independent accountable worker ([NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/), [NIST GenAI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)).

---

## 3. Offshore coordination versus AI supervision

Research confirms offshore coordination costs exist, but does not yield a defensible universal percentage:

- A study of 102 outsourcing relationships found that scale and geographic distance increase coordination and control costs ([Journal of Management Information Systems](https://www.sciencedirect.com/science/article/pii/S0272696312000897)).
- A systematic literature review found inadequate project management mentioned in 64% of reviewed sources, poor monitoring in 51%, relationship management in 50% and quality control in 15%; these are frequencies of reported problems, not cost ratios ([IET Software](https://ietresearch.onlinelibrary.wiley.com/doi/full/10.1049/iet-sen.2015.0080)).
- Outsourcing research identifies requirements definition, knowledge transfer, control and coordination as material client-side costs omitted from simple wage comparisons ([MIS Quarterly](https://aisel.aisnet.org/misq/vol32/iss2/9/)).

There is even less mature evidence for a universal “AI supervision ratio.” METR’s experiment is an important warning: experienced developers believed AI had made them 24% faster, while measured completion time showed they were 19% slower, illustrating why subjective supervision estimates are unreliable ([METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)).

A 2026 expense-data study found that among highly AI-exposed firms, every $1 decline in online-labor spending was associated with only about $0.03 of additional AI spending. That demonstrates payment substitution, not equivalent output, quality or FTE capacity ([Payrolls to Prompts](https://arxiv.org/abs/2602.00139)).

Verdict: neither “offshore requires 30% management” nor “one manager can supervise 20 agents” is currently safe to publish as a general fact.

---

## 4. What capability studies actually establish

### Controlled and field evidence

- In a field study of 5,179 customer-support agents, generative AI increased issues resolved per hour by 14% on average and by 34% for novice and lower-skilled workers, with little benefit for the most experienced workers ([NBER](https://www.nber.org/papers/w31161)).
- In an experiment with 758 BCG consultants, participants working on tasks inside the model’s capability frontier were more than 25% faster, produced work rated more than 40% higher and completed over 12% more tasks; performance deteriorated on tasks outside the frontier ([Harvard Business School](https://aiinstitute.hbs.edu/navigating-the-jagged-technological-frontier/)).
- Danish administrative data found no detectable aggregate earnings or recorded-hours effect after two years of chatbot adoption and ruled out effects larger than approximately 2%, despite observable task restructuring ([University of Chicago/NBER working paper](https://bfi.uchicago.edu/working-papers/large-language-models-small-labor-market-effects/)).
- A US adoption survey estimated that generative AI assisted approximately 1–5% of total work hours and produced reported time savings equal to about 1.4% of aggregate work hours at the time of the study ([NBER](https://www.nber.org/papers/w32966)).

These support augmentation and limited task substitution, not mass whole-job removal.

### Agent reliability

- METR’s task-horizon benchmark estimates the length of human tasks an agent can complete with a given success probability, but explicitly warns that an eight-hour benchmark horizon does not equal automating an eight-hour job; real work contains organizational context, social interaction and poorly specified success criteria, and estimates above roughly 16 hours remain unreliable ([METR](https://metr.org/time-horizons/)).
- On RE-Bench, AI systems were competitive at short time budgets, but humans achieved approximately twice AI performance at a 32-hour budget ([RE-Bench](https://arxiv.org/abs/2411.15114)).
- In TheAgentCompany’s simulated software firm, the best reported model completed 30.3% of 175 realistic workplace tasks and received 39.3% partial-credit performance ([TheAgentCompany](https://arxiv.org/abs/2412.14161)).
- In CRMArena, state-of-the-art agents completed under 40% of realistic customer-service CRM tasks with ReAct and under 55% with function calling ([CRMArena](https://arxiv.org/abs/2411.02305)).

### Labor-exposure reports

These are often misquoted as replacement forecasts:

- Anthropic found Claude activity in at least 25% of tasks for 36% of occupations, but in at least 75% of tasks for only about 4%; the dataset contained no occupation showing complete automation across its task set ([Anthropic](https://www.anthropic.com/news/the-anthropic-economic-index)).
- ILO–NASK estimated that 25% of global employment and 34% of high-income-country employment has some GenAI exposure, while concluding that job transformation is more likely than complete replacement and clerical work is the most exposed ([ILO](https://www.ilo.org/resource/news/one-four-jobs-risk-being-transformed-genai-new-ilo%E2%80%93nask-global-index-shows)).
- OpenAI’s earlier exposure model estimated that approximately 80% of US workers could have at least 10% of tasks affected and 19% could have at least 50% affected; “affected” includes time savings and augmentation and is not a layoff prediction ([OpenAI](https://openai.com/index/gpts-are-gpts/)).
- McKinsey modeled approximately 27% of European and 30% of US work hours as potentially automated by 2030 in its midpoint scenario; those are modeled hours, not observed jobs eliminated ([McKinsey](https://www.mckinsey.com/mgi/our-research/a-new-future-of-work-the-race-to-deploy-ai-and-raise-skills-in-europe-and-beyond)).
- Goldman Sachs’s “too much spend, too little benefit?” report presents conflicting views on long-term productivity and deployment economics rather than a measured AI-versus-offshore FTE comparison ([Goldman Sachs](https://www.goldmansachs.com/intelligence/pages/gs-research/gen-ai-too-much-spend-too-little-benefit/report.pdf)).

---

## 5. Five strongest real cases for the thesis

These cases support narrow substitution or major augmentation. None independently proves 90% company-wide staff replacement.

### 1. Klarna customer service

Klarna’s SEC prospectus reported that its assistant handled 69% of service chats in the 12 months to June 2025, generated an internally estimated workload equivalent to more than 700 FTEs and contributed approximately $39 million in 2024 cost savings. “700 FTE-equivalent” was calculated from reduced chat and telephone volume; it was not evidence that 700 identified employees were fired ([SEC filing](https://www.sec.gov/Archives/edgar/data/2003292/000200329225000052/klarnagroupplc424b4.htm)).

Klarna’s later annual filing reported 80% of 2025 chats, 31 million conversations since launch, repeat inquiries down 25%, and two-minute average resolution versus 12 minutes for human service; these remain company-reported operational metrics ([Klarna annual filing](https://www.sec.gov/Archives/edgar/data/2003292/000200329226000007/klar-20251231.htm)).

Verdict: strong evidence for high-volume Tier-1 contact substitution; not proof of 700 employee replacements or general employee equivalence.

### 2. IBM AskHR

IBM reports that AskHR contains 94% of common employee questions, reduced support tickets by 75% and helped reduce HR operating costs by 40% over four years; it handled 11.5 million interactions in 2024 ([IBM case study](https://www.ibm.com/case-studies/ibm-askhr)).

However, AskHR combines rules-based automation, workflow redesign and newer generative AI, so the entire result cannot be attributed to current LLM agents. IBM also disclosed that an early “big bang” rollout drove HR net-promoter score to −35 before redesign ultimately raised it to +74 ([IBM](https://www.ibm.com/think/insights/embracing-future-of-hr-ai-first-enterprise)).

Verdict: strong evidence for transactional HR automation, with strong evidence that implementation capability determines the result.

### 3. Salesforce customer support

Salesforce reported 380,000 Agentforce conversations in its fiscal 2025 fourth quarter, an 84% resolution rate and 2% requiring human escalation ([Salesforce results](https://investor.salesforce.com/news/news-details/2025/Salesforce-Announces-Fourth-Quarter-and-Fiscal-Year-2025-Results/)). Its later customer-zero page reports more than two million conversations and a 68% resolution rate ([Salesforce](https://www.salesforce.com/agentforce/use-cases/customer-zero/)).

CEO Marc Benioff subsequently said the support workforce had declined from approximately 9,000 to 5,000 while AI and humans each handled roughly 1.5 million conversations, but this causal attribution was not independently audited and Salesforce’s own operations leadership separately said its need for headcount had not disappeared ([ITPro](https://www.itpro.com/technology/artificial-intelligence/marc-benioff-says-salesforce-has-already-cut-4-000-customer-support-staff-for-ai-agents-and-workforce-productivity-hasnt-dipped), [Salesforce operations account](https://www.salesforce.com/news/stories/ai-agent-customer-service-salesforce-learnings/)).

Verdict: credible containment data; uncertain causal staff-replacement count.

### 4. BBVA augmentation

BBVA reported deploying ChatGPT Enterprise to approximately 100,000 employees, saving roughly three hours weekly per employee on average and up to 80% of time in selected workflows. In one Peru workflow, a query fell from 7.5 minutes to one minute; BBVA also trained 250 leaders and established structured internal champions and governance ([OpenAI/BBVA case](https://openai.com/index/bbva/)).

This is vendor/client self-report and demonstrates broad augmentation—not workforce elimination.

### 5. Generative AI in customer support

The strongest independent field result remains the anonymized Fortune 500 support deployment involving 5,179 agents: 14% average productivity improvement and 34% for novices and lower performers ([NBER](https://www.nber.org/papers/w31161)).

The result supports a smaller or higher-throughput support organization over time, but it directly measured human performance with assistance, not humans being replaced.

---

## 6. Five most instructive walkbacks and failures

Only the first three are genuine workforce-claim reversals or material narrowings. There are not five well-documented cases of “company fired X% for AI and then rehired the same X%.”

### 1. Klarna: quality and human-access correction

After emphasizing cost reduction and AI service, Klarna’s CEO acknowledged that cost had been prioritized too heavily and that service quality suffered; Klarna resumed hiring some human support workers and retained a human option for complex cases. AP also documented cases such as identity theft where customers required human resolution ([Associated Press](https://apnews.com/article/ca87ae77d7c6797ebb2628bd1b532929)).

This was a partial correction, not abandonment of AI.

### 2. Commonwealth Bank of Australia: redundancies reversed

CBA announced 45 customer-service redundancies linked to a voice bot, then reversed the decision and apologized, stating that the positions were not redundant. The union reported increased call volume, overtime and team leaders taking calls after deployment ([ABC Australia](https://www.abc.net.au/news/2025-08-21/cba-backtracks-on-ai-job-cuts-as-chatbot-lifts-call-volumes/105679492), [Finance Sector Union](https://www.fsunion.org.au/WIN-CBA-backflips-on-customer-service-job-cuts/)).

This is the clearest recent example of cutting before operational evidence was mature.

### 3. IBM: 7,800-role forecast narrowed to hundreds

In 2023 IBM projected that approximately 7,800 back-office positions might be replaced or left unfilled over five years. By 2025 its CEO said only a couple of hundred HR positions had actually been replaced while software-development and sales hiring increased ([Axios](https://www.axios.com/newsletters/axios-ai-plus-cf521d18-654d-46a2-b138-7f6a9ce36c92)).

This was a major narrowing of an earlier forecast, although the five-year period had not ended.

### 4. Duolingo: “AI-first” mandate softened

Duolingo cut approximately 10% of contractors in late 2023 while saying AI could perform some of their work, although human experts continued validating AI output ([TechCrunch](https://techcrunch.com/2024/01/09/duolingo-cut-10-of-its-contractor-workforce-as-the-company-embraces-ai/)). Its 2025 AI-first memo said it would gradually stop using contractors where AI could handle the work and might tolerate small temporary quality trade-offs ([The Register](https://www.theregister.com/software/2025/04/29/duolingo-ditches-more-contractors-in-ai-first-refocus/646340)).

The CEO later said he did not expect AI to replace employees, and in 2026 changed course on tying individual performance directly to AI use because the tools did not fit every workflow; outcomes became the focus instead ([Fortune](https://fortune.com/2025/05/24/duolingo-ai-first-employees-ceo-luis-von-ahn/), [TechRadar](https://www.techradar.com/ai-platforms-assistants/were-trying-to-push-something-that-in-some-cases-did-not-fit-duolingos-ceo-changes-course-on-ai-at-work)).

No reliable evidence shows Duolingo rehired the contractors or quantified AI-caused quality deterioration.

### 5. McDonald’s/IBM drive-through automation pilot

McDonald’s ended a three-year automated-ordering pilot covering more than 100 restaurants after mixed results and conspicuous ordering errors; McDonald’s did not publish an accuracy rate ([Associated Press](https://apnews.com/article/bebc898363f2d550e1a0cd3c682fa234)).

This was not an LLM coding-agent deployment or a documented workforce reversal. It is nevertheless a useful warning against moving from demo performance to labor-elimination claims without production accuracy data.

---

## 7. Evidence for the “orchestrator plus agent fleet” organization

Tiny AI-native teams are real:

- Base44, reportedly an eight-person company approximately six months after launch, was acquired by Wix for an initial approximately $80 million. Wix’s SEC filing said Base44 had only a few million dollars of ARR in June 2025, although it forecast $40–$50 million by year-end ([TechCrunch](https://techcrunch.com/2025/06/18/6-month-old-solo-owned-vibe-coder-base44-sells-to-wix-for-80m-cash/), [Wix](https://www.wix.com/press-room/home/post/wix-further-expands-into-vibe-coding-with-acquisition-of-base44-a-hyper-growth-startup-that-simplif), [Wix SEC filing](https://www.sec.gov/Archives/edgar/data/1576789/000162828025038117/secondquarter2025.htm)).
- Lovable reported reaching $100 million ARR with 45 FTEs eight months after launch; this was a company-supplied revenue figure reported by TechCrunch ([TechCrunch](https://techcrunch.com/2025/07/23/eight-months-in-swedish-unicorn-lovable-crosses-the-100m-arr-milestone/)).
- Forbes, citing company and PitchBook information, reported Midjourney at approximately $300 million fiscal-2024 revenue and profitable without external funding, but a dependable employee count was not disclosed ([Forbes](https://www.forbes.com/companies/midjourney/)).

These companies sell AI products. Their revenue per employee does not establish that an ordinary accounting firm, agency, retailer or manufacturer can reproduce the same ratio, nor does public evidence isolate AI’s contribution from software economics, capital, contractors, founder labor and product-market fit.

I found no verified one-person business with either $1 billion of annual revenue or a defensible $1 billion arm’s-length valuation. The “one-person unicorn” remains SPECULATIVE discourse.

### Required orchestrator skills

Evidence from BBVA, IBM, NIST and METR supports the following human skill profile:

1. Process decomposition: distinguish deterministic, measurable subtasks from ambiguous judgment.
2. Domain expertise: recognize superficially plausible but substantively wrong output.
3. Evaluation engineering: create test cases, acceptance thresholds and regression monitoring.
4. Context and data architecture: give agents reliable access to current company information.
5. Security and compliance: control permissions, confidential information and external actions.
6. Exception design: define when an agent must stop or escalate.
7. Change management: train staff and redesign jobs rather than simply adding a chatbot.
8. Unit-economics measurement: compare accepted output, rework and review time—not tokens or conversations alone.

BBVA trained 250 leaders and built a structured champion network, while IBM’s failed initial rollout and later recovery show that orchestration capability is a material operating asset ([BBVA](https://openai.com/index/bbva/), [IBM](https://www.ibm.com/think/insights/embracing-future-of-hr-ai-first-enterprise)).

---

## 8. Strongest counter-case

### Reliability and long-horizon work

Current agents remain weak where work has:

- Ambiguous requirements.
- Long causal chains.
- Unavailable organizational context.
- Interpersonal negotiation.
- High cost of a single error.
- Legal, fiduciary or client accountability.
- Success criteria that cannot be tested automatically.

TheAgentCompany’s 30.3% full-completion result, CRMArena’s sub-40%/55% results and RE-Bench’s human advantage at 32 hours are direct evidence that current agent performance does not generalize from short coding benchmarks to employee-grade reliability ([TheAgentCompany](https://arxiv.org/abs/2412.14161), [CRMArena](https://arxiv.org/abs/2411.02305), [RE-Bench](https://arxiv.org/abs/2411.15114)).

Even successful deployments constrain scope. Octopus Energy’s 2026 autonomous-email trial covered approximately 8,000 emails per week—4% of its UK email volume—with 76% customer satisfaction versus 72% for human replies, but excluded vulnerable customers, sensitive situations and complex complaints and was explicitly positioned as support rather than replacement ([Octopus Energy](https://octopus.energy/press/more-news-press-releases/octopus-energy-s-ai-trial-wins-customer-approval/)).

### Accountability and liability

The deploying business remains accountable for agent output. A Canadian tribunal ordered Air Canada to pay C$812.02 after its chatbot supplied incorrect bereavement-fare information; the decision rejected the idea that the chatbot was a separate legal entity responsible for its own statements ([Moffatt v Air Canada](https://cdn.lawreportgroup.com/acuris/files/Cybersecurity-New/Utah%202024bccrt149.pdf)).

That chatbot was not proven to be a modern LLM agent, but the liability principle is directly relevant: substituting software for an employee does not substitute away the company’s obligations.

---

## 9. Legal, HR and advertising exposure

This is a high-level comparison, not jurisdiction-specific legal advice.

### European Union

- AI used for recruitment, employment or worker management is classified as high-risk. From 2 December 2027, covered systems face requirements involving risk management, data quality, logs, documentation, human oversight, robustness, cybersecurity and accuracy ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)).
- Workplace emotion recognition has generally been prohibited since February 2025, while chatbot and AI-generated-content transparency requirements became applicable in August 2026 ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)).
- GDPR Article 22 restricts decisions based solely on automated processing that produce legal or similarly significant effects, subject to limited exceptions and safeguards such as human intervention ([GDPR](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679)).
- EU collective-redundancy rules require timely consultation, disclosure of reasons and selection criteria, and consideration of avoidance, mitigation, redeployment and retraining; the directive applies to establishments employing at least 20 workers, with national threshold implementations ([European Commission](https://employment-social-affairs.ec.europa.eu/policies-and-activities/rights-work/labour-law/employee-involvement/collective-redundancies_en)).
- Commercial content is subject to unfair-commercial-practices and misleading-advertising rules, so “replace 90% of your employees” would need evidence applicable to the advertised service and audience—not unrelated vendor case studies ([EU UCPD](https://commission.europa.eu/law/law-topic/consumer-protection-law/unfair-commercial-practices-and-price-indication/unfair-commercial-practices-directive_en)).

### United States

- Federal WARN generally requires covered employers with at least 100 employees to provide 60 days’ notice for certain plant closings or mass layoffs affecting at least 50 workers at a single site; state “mini-WARN” rules can be broader ([US Department of Labor](https://www.dol.gov/general/topic/termination/plantclosings)).
- Federal anti-discrimination law applies to termination and layoff selection regardless of whether an AI tool recommends or executes the selection ([EEOC](https://www.eeoc.gov/newsroom/eeoc-launches-initiative-artificial-intelligence-and-algorithmic-fairness)).
- Paid or commercially connected marketing must be truthful, non-deceptive and supported by evidence before publication. The FTC has separately brought enforcement actions against deceptive AI claims and schemes ([FTC advertising guidance](https://www.ftc.gov/business-guidance/resources/advertising-faqs-guide-small-business), [FTC AI enforcement](https://www.ftc.gov/news-events/news/press-releases/2024/09/ftc-announces-crackdown-deceptive-ai-claims-schemes)).

Pure independent editorial opinion is different from advertising, but organic social content can become commercial advertising when it promotes the creator’s service, course, affiliate relationship or paid consulting offer.

### Russia and other Russian-speaking markets

- Russian Labour Code Article 180 generally requires offering suitable vacancies and giving an employee personal written notice at least two months before redundancy ([ConsultantPlus](https://www.consultant.ru/document/cons_doc_LAW_34683/6887e2fd9f9775c467fb9cc45fbc5874a04174cc/)).
- Russia’s advertising law prohibits unreliable advertising and incorrect comparisons; an unqualified “AI is always cheaper than offshore labor” claim would therefore be risky in commercial content ([ConsultantPlus](https://www.consultant.ru/document/cons_doc_LAW_58968/)).
- Kazakhstan’s 2025 AI law includes disclosure requirements for AI-powered goods and services and labeling of synthetic output, while automated personal-data decisions remain subject to data-protection rules ([Kazakhstan legislation](https://adilet.kz/ru/laws/doc-217048/st-21/)).

“Russian-speaking markets” is not a legal jurisdiction. Russia, Kazakhstan, Armenia, Georgia, Ukraine, the Baltics and Russian-speaking EU audiences require separate employment, data and advertising analyses.

---

## 10. Verdict by function

| Function | What is defensible now |
|---|---|
| Tier-0/Tier-1 customer support | MEASURED: 14% average productivity improvement in an independent field study; vendor/company deployments report 68–94% containment or resolution for qualifying contacts ([NBER](https://www.nber.org/papers/w31161), [Salesforce](https://www.salesforce.com/agentforce/use-cases/customer-zero/), [IBM](https://www.ibm.com/case-studies/ibm-askhr)). ESTIMATED planning range: 20–50% net human-capacity reduction after exceptions and review. Do not present that estimate as a universal fact. |
| Transactional HR/employee FAQ | MEASURED: IBM reports 94% containment of common questions and 40% operating-cost reduction over four years ([IBM](https://www.ibm.com/case-studies/ibm-askhr)). This does not mean 94% of HR employees can be removed. |
| Research, drafting and consulting | MEASURED: more than 25% faster and over 40% higher quality for tasks inside the capability frontier in the BCG experiment ([HBS](https://aiinstitute.hbs.edu/navigating-the-jagged-technological-frontier/)). ESTIMATED: 10–30% net capacity gain is a cautious planning scenario, not a replacement finding. |
| Coding/junior development | Mixed. MEASURED: experienced open-source developers were 19% slower in METR’s study; simulated employee-like agents completed only 30.3% of tasks in TheAgentCompany ([METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/), [TheAgentCompany](https://arxiv.org/abs/2412.14161)). No general FTE-replacement percentage is defensible. |
| Bookkeeping | I found no independently audited 2025–2026 case demonstrating employee-grade autonomous bookkeeping and a verified corresponding FTE reduction. Drafting, categorization and reconciliation assistance are plausible; final controls, tax treatment and sign-off remain human-accountability functions. |
| Content production | Major drafting acceleration is documented in controlled knowledge-work tasks, but reliable evidence for whole content-team replacement is absent ([HBS](https://aiinstitute.hbs.edu/navigating-the-jagged-technological-frontier/)). Brand judgment, factual verification, rights and approval remain review costs. |
| General operations/admin | Usage data show increasing automation of API-mediated office/admin tasks but do not measure correct completion or jobs removed ([Anthropic January 2026 index](https://www.anthropic.com/research/anthropic-economic-index-january-2026-report)). No universal replacement percentage is defensible. |
| Mixed business workforce | SPECULATIVE planning range: 0–10% near-term whole-headcount elimination and 5–20% net capacity improvement after redesign may be plausible for a digital business, but neither is an established cross-industry result. A public “90%” claim is unsupported. |

The only circumstances in which “90%” can be published responsibly are narrow statements such as “up to 90% of common FAQ interactions were contained in this specified deployment.” It cannot be silently converted into “90% of employees.”

---

## 11. Exact numeric claims safe to publish

Safe when reproduced with the stated qualifier:

1. “BLS reports $80.51/hour for computer and mathematical workers in the SF metro versus $57.73 nationally; applying BLS professional benefit ratios produces an estimated $20,700 versus $14,800 monthly compensation cost.” ([BLS SF](https://www.bls.gov/regions/west/news-release/occupationalemploymentandwages_sanfrancisco.htm), [BLS ECEC](https://www.bls.gov/news.release/archives/ecec_03202026.htm))

2. “Anthropic reports average Claude Code enterprise usage of about $13 per active developer-day and $150–$250 per developer-month, with 90% below $30 per active day.” ([Anthropic](https://code.claude.com/docs/en/costs))

3. “OpenAI reports Codex averaging approximately $100–$200 per developer-month, with large variation by model, concurrency and automation.” ([OpenAI](https://help.openai.com/en/articles/20001106-codex-rate-card))

4. “Klarna’s SEC filing estimated that its assistant handled 69% of chats, performed workload equivalent to more than 700 FTEs and contributed about $39 million in 2024 savings; this was an internal workload estimate, not 700 verified firings.” ([SEC](https://www.sec.gov/Archives/edgar/data/2003292/000200329225000052/klarnagroupplc424b4.htm))

5. “In a field study of 5,179 support agents, generative AI increased productivity by 14% overall and 34% for novice and lower-skilled workers.” ([NBER](https://www.nber.org/papers/w31161))

6. “In METR’s study of 16 experienced open-source developers completing 246 real issues, AI increased measured completion time by 19% even though developers expected a speed-up.” ([METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/))

7. “Anthropic observed Claude use across at least 75% of tasks in only about 4% of occupations; observed usage was 57% augmentation and 43% automation.” ([Anthropic](https://www.anthropic.com/news/the-anthropic-economic-index))

8. “The best reported system in TheAgentCompany fully completed 30.3% of simulated workplace tasks.” ([TheAgentCompany](https://arxiv.org/abs/2412.14161))

9. “Claude Max and ChatGPT Pro currently top out at $200 monthly for their highest individual usage tier, but the subscription price is not employee-equivalent total cost.” ([Anthropic](https://support.claude.com/en/articles/11049762-choose-a-claude-plan), [OpenAI](https://help.openai.com/en/articles/9793128-what-is-chatgpt-pro))

---

## 12. Claims not safe to publish as facts

- “Businesses can replace 90% of knowledge workers today.”
- “One $200 AI subscription equals one full-time employee.”
- “AI is already cheaper than every offshore worker.”
- “A fully loaded mid-level India or Philippines employee universally costs $800–$1,500.”
- “Klarna fired 700 people and replaced them with AI.”
- “Salesforce conclusively replaced 4,000 support workers with agents without quality loss.”
- “AI works 24/7 without hiring or management friction.”
- “One manager can supervise 10, 20 or 100 autonomous agents.”
- “AI coding agents deliver 10x productivity.”
- “Agent output has no quality, compliance or liability overhead.”
- “A proven one-person unicorn already exists.”
- “AI-native revenue-per-employee ratios are transferable to ordinary businesses.”

These can be hypotheses, scenarios or quotations attributed to their proponents, but not presented as established outcomes.

---

## 13. What could not be verified

1. A universal token count for one “full agent workday.”
2. A valid conversion between model tokens and human FTE capacity.
3. Any cross-industry evidence supporting 90% whole-staff replacement.
4. A representative fully loaded $800–$1,500 monthly mid-level technical-worker cost across India.
5. A universal offshore supervision or QC percentage.
6. A universal AI-agent supervision percentage.
7. Independently audited evidence that Salesforce’s entire reported 4,000-person support reduction was caused by AI.
8. Evidence that Klarna terminated exactly 700 employees because of its assistant.
9. Evidence that Duolingo rehired its terminated contractors or measured a specific AI-caused quality decline.
10. Five genuine cases where companies fired a quantified workforce for AI and then rehired the same workers; only Klarna’s partial correction and CBA’s reversal are clearly documented.
11. An independently audited case of full bookkeeping-job replacement by an autonomous agent.
12. A verified one-person unicorn.
13. A causal estimate of how much AI—not software economics, contractors, capital or founder labor—explains the revenue-per-employee ratios of Base44, Lovable or Midjourney.
14. A universal rule for “Russian-speaking markets”; each country has separate employment, advertising, AI and data law.
15. A reliable general employee-replacement percentage for research, content, junior development, bookkeeping or operations.

## Bottom line

The economic opportunity is real, but its unit is the workflow, not the employee.

AI compute is already dramatically cheaper than professional labor. Reliable accepted output is not: its cost includes context preparation, supervision, evaluation, exceptions and liability. The strongest defensible public thesis is that well-designed businesses can automate large shares of narrowly defined routine transactions and substantially increase the capacity of a smaller expert team. The claim that they can currently replace 90% of routine knowledge-work staff is unsupported, operationally dangerous and, when used commercially, potentially misleading.
