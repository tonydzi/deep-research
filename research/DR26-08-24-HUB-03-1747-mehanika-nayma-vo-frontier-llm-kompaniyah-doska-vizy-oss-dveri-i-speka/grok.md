# DR26-08-24-HUB-03-1747 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. BLOCK 1 — DOES THE PUBLIC JOB BOARD EVEN MATTER?

FACT / UNKNOWN split first, because this is the question everyone answers with folklore.

UNKNOWN (the thing you actually asked): nobody has published a measured fill-source breakdown for DevRel / Developer Education / Community / Applied AI / Forward Deployed roles at frontier LLM labs. There is no Ashby/Greenhouse/LinkedIn study that says “X% of Anthropic DevRel reqs closed via the board.” If someone quotes a number for this slice, they are laundering a broader tech statistic. I could not verify a lab-specific measured share.

FACT (broader market, not this slice):
- LinkedIn’s own recurring hiring narrative, repeated in their Global Talent Trends / talent-flow materials across the 2010s–2020s, is that a large majority of placements involve networking rather than cold apply. The frequently-repeated “85% of jobs are filled via networking” line is a LinkedIn marketing statistic, not a DevRel-at-Anthropic measurement. Treat as folklore-with-a-corporate-PR-parent, not a lab rate.
- Greenhouse’s and Ashby’s own customer blogs (public, vendor-biased) consistently describe time-to-fill for specialized technical roles in the 30–60+ day range and emphasize inbound + referral as higher-quality than job-board apply. These are vendor reports, not lab disclosures.
- Large-lab career pages themselves advertise both “open roles” and evergreen “we’re always looking” language. OpenAI Careers (https://openai.com/careers), Anthropic Careers (https://www.anthropic.com/careers), DeepMind Careers (https://deepmind.google/careers) all mix posted reqs with general interest / rolling forms. That pattern is a FACT of how they present hiring. It is not a measurement of fill source.

FACT (public statements and first-person paths, which are evidence of mechanism, not of share):
- Logan Kilpatrick’s OpenAI DevRel hire (2023) and later Google AI Studio move (2024) were not “I applied to a Greenhouse req as a stranger.” He was already a visible developer-relations person at Streamlit, with a public audience. Coverage: OpenAI/Google announcements and his own posts, 2023–2024.
- Alex Albert’s Anthropic path (Claude-relations, 2023–2024) is the cleanest lab-side example of inbound-from-reputation: prolific public Claude usage/Twitter → company noticed → hired into a relations seat. First-person/public trail on X and Anthropic-adjacent interviews, 2023–2024.
- Riley Goodside → Scale AI (Staff Prompt Engineer, 2022) is the canonical “Twitter artifact → company inbound” hire of the GPT-3 era. Covered contemporaneously (his posts; Scale’s visible employment).
- Hugging Face has, for years, described itself as hiring out of its own community (forums, libraries, courses). This is company-cultural FACT, repeated by staff in public talks and blog posts 2022–2025. It is still not a percentage.

ESTIMATE (inference, not a measurement):
For the role families you care about at the top-10 labs, the public board is real but it is not the main fill pipe. A working prior, labeled ESTIMATE:

- Board-as-actual-search: ~15–30% of fills
- Referral / employee-known: ~25–40%
- Inbound-from-reputation (they email you because of a repo, a thread, a talk, a Discord): ~20–35%
- Headhunter / agency: ~5–15% (higher at FDE/solutions; lower at community)
- Internal transfer: material at Google/Meta/Microsoft; near-zero if you are outside
- Acqui-hire / team-hire: rare, spiky, real (especially 2023–2025 talent raids). Not a volume path.

Why this ESTIMATE and not a number: DevRel/FDE seats are few (often 1–8 per lab), high-trust, and “the person is already known to the engineering team” is the hiring manager’s actual risk control. When the req is posted, it often runs in parallel with a name already in mind. That is standard specialized-hiring practice; it is not unique to AI.

FACT-adjacent on “posted after the candidate is identified”:
- US labor-compliance and visa sponsorship create a reason to post a req even when the candidate is known (H-1B/PERM public posting rules; internal equity; finance needing a req number). So “it was on the board” does not mean “they found the person on the board.”
- Conversely, labs also post roles they fail to fill and then close/reopen under a new title. Title churn (Developer Advocate ↔ Developer Relations Engineer ↔ Partner Engineer ↔ Solutions ↔ Forward Deployed) is a FACT you will see if you watch Ashby/Greenhouse histories. Same seat, new label.

Time-from-posting-to-close:

FACT: no lab publishes this for DevRel.
ESTIMATE from general technical recruiting plus how these reqs behave:
- If a champion already exists: 2–6 weeks, board posting may be simultaneous or even after verbal.
- If they are truly searching: 6–16 weeks. DevRel reqs can sit because the bar is “this person can stand in front of our engineers without embarrassing us,” which is a chemistry search, not a funnel.
- FDE/solutions at Sierra/Harvey/Scale-style shops can close faster when a staffed recruiting team is running a batch (on-site weeks).
- “Evergreen” DevRel posts (OpenAI/Anthropic/HF have all run rolling-looking developer-facing reqs) can be open for months and are then a talent magnet, not a clock.

Board still matters for three mechanical reasons, even if it is not how you get in:
1. It is the only machine-readable list of what they will currently defend to finance (a posted req = a real budget line more often than a tweet).
2. It is the language they use internally (“we are hiring a Partner Engineer, not a Community Manager”). Your inbound pitch has to match the req’s nouns.
3. For visa/relocation, a posted req in a specific city is evidence. A tweet is not.

What to believe: treat the board as a sensor, not a door. The door is a named human inside the lab who already wants you. The board tells you which noun to use when you walk through.

---

B. BLOCK 2 — LIVE ROLE MAP FOR THIS PROFILE

Observation method (read this before any company line): this report is written 2026-08-24 with no live fetch of career pages or ATS APIs, per your output contract. Therefore every “currently open” claim is UNVERIFIED. I will not invent postings. What follows is (a) role families these companies actually use as titles, (b) whether that family has been recurrent 2023–2026, (c) the honest screen bar versus your profile, (d) location/visa posture as last-known public policy. Recurrence ≠ a job you can apply to today.

How to read the bar:
- REJECT-AT-SCREEN = you die in the first technical gate (coding interview, ML fundamentals, “implement X,” take-home at SWE bar).
- REJECT-AT-LOOP = you can get a first call and then lose to “engineers wouldn’t respect this person” or “founder risk.”
- WINNABLE = the evaluation is teaching, writing, community, partner motion, public artifact, workshop. Still competitive; not charity.

Profile fit, in one line: you are not a Research Scientist, not a Research Engineer, not a production SWE, not a classical FDE. You are a developer-activation operator with a PhD wrapper and a public multi-agent artifact. That maps to DevRel / Developer Education / Community / Partner / some Applied-AI-without-coding-loop titles. It does not map to “Forward Deployed Engineer” as Palantir-descended companies run it.

---

1) Anthropic
Role titles they actually use (recurrent language, 2023–2026): Developer Relations, Claude Relations, Partner Engineer / Partnerships, Solutions / Technical Success, Applied AI, Forward Deployed / start-up-facing technical roles, Technical Writer / Education, sometimes “Claude Evangelist”-adjacent internal language around relations.
Locations last-known: San Francisco, New York; some remote-US; limited non-US. London has been discussed in headcount rumors; UNVERIFIED as a standing DevRel site. Relocation to SF/NYC raises odds. FACT of company geography: HQ SF.
Bar vs you:
- Partner Engineer / FDE-shaped: REJECT-AT-SCREEN. They will run a technical bar designed to protect customer-facing engineering credibility. AI-co-builder-as-primary-coder is a screen-out if they test live implementation.
- Developer Relations / Claude Relations / Education / Community-facing: WINNABLE only with a named champion and proof you activate developers on *Claude*, not on “agents in general.” Inbound-from-reputation is the realistic door (Alex Albert path is the existence proof). Cold board apply: low.
- Applied AI (research-adjacent): REJECT-AT-SCREEN. Wrong publication venue and wrong stack.
Live postings 2026-08-24: UNVERIFIED.

2) OpenAI
Role titles: Developer Advocate / Developer Relations, Developer Experience, Solutions Architect, Technical Partner Manager, Applied AI Engineer, Forward Deployed / Startups-facing, Technical Writer, Community.
Locations: San Francisco primary; London presence; NYC; some remote but the gravity is SF. FACT: OpenAI has repeatedly signaled SF-centric culture for core teams.
Bar vs you:
- DevRel Engineer / DevEx / Solutions: REJECT-AT-SCREEN or REJECT-AT-LOOP. Logan Kilpatrick’s seat was won from an existing DevRel franchise (Streamlit audience), not from a non-coder-operator story. They hire people who can demo in front of staff engineers.
- Technical Partner / Startups Success / Education: more WINNABLE if you can show cohort-activation (you have this from accelerators) *and* artifacts on their API.
- FDE: REJECT-AT-SCREEN.
Live postings: UNVERIFIED. Careers: https://openai.com/careers

3) Google DeepMind / Google AI (Gemini, AI Studio, Google AI Developers)
Role titles (Google’s actual nouns, not DeepMind-research nouns): Developer Relations Engineer (DRE), Developer Programs Engineer, Developer Advocate, Technical Writer, Solutions Engineer (Cloud/AI), Partner Engineer, Applied. DeepMind-proper titles are Research Scientist / Research Engineer / Research Software Engineer — those are a different caste.
Locations: Mountain View, NYC, London (DeepMind HQ), Zurich, Paris, Munich; Google has a real London/Zurich/Paris footprint. FACT: DeepMind HQ is London (https://deepmind.google).
Bar vs you:
- DeepMind Research*: REJECT-AT-SCREEN. h=7 / ZK-blockchain PhD is not a GDM research packet.
- Google DRE / Developer Programs / AI Studio DevRel: WINNABLE in principle — this is the Logan-at-Google-shaped seat — but Google’s DRE loop is a known technical gauntlet (you teach, *and* you code through a Google-style interview). ESTIMATE: REJECT-AT-LOOP unless the hiring manager rewrites the loop around teaching samples.
- Cloud AI Solutions: coding + customer + Google Cloud cert gravity. Harsh.
Live postings: UNVERIFIED. https://careers.google.com and https://deepmind.google/careers

4) Meta AI (FAIR + Llama + PyTorch)
Role titles: Developer Advocate (PyTorch, Llama, ExecuTorch), Partner Engineer, Solutions Engineer, Technical Writer, Community. Llama ecosystem has had genuinely community-facing hiring.
Locations: Menlo Park, NYC, London, Paris, Zurich. FACT: Meta has a large Zurich office; London and Paris exist.
Bar vs you:
- PyTorch/Llama DevRel: possible if you become a Llama-stack person in public (llama.cpp-class, vLLM-class, or a widely used Llama cookbook). Today you are not that. ESTIMATE: REJECT-AT-LOOP for lack of stack-specific audience.
- FAIR research: REJECT-AT-SCREEN.
Live postings: UNVERIFIED. https://www.metacareers.com

5) xAI
Role titles last-known: overwhelmingly Software Engineer, plus research, data, infra. DevRel/Community/Education have been thin to absent in public boards historically. ESTIMATE: they under-hire DevRel on purpose (Elon-company pattern: the product is the evangelism).
Locations: Bay Area / Palo Alto; Austin; Memphis (Colossus). Not Lisbon-friendly. Not Paris-friendly.
Bar vs you: REJECT-AT-SCREEN for almost everything they post. A Community/DevRel seat, if it appears, would still be culture-fit brutal and engineering-cred brutal.
Live postings: UNVERIFIED. Careers have lived at https://x.ai/careers and Ashby.

6) Mistral
Role titles: Developer Advocate / Developer Relations, Solutions Engineer, Partner, Technical Writer, Deployment / Applied. French/English bilingual is a silent plus.
Locations: Paris is the center of gravity. FACT: Mistral is a Paris company (https://mistral.ai). London/other EU offices have been discussed as they scaled; treat extra cities as UNVERIFIED unless the req says so. Remote exists in some postings historically, but Paris presence is the tell.
Bar vs you:
- DevRel / Education / Community for *European developers*: one of your actually-winnable top-10 seats IF you are willing to be in Paris. Your EU-eligibility is a real advantage here (see Block 3).
- Solutions/Deployment: REJECT-AT-SCREEN if they run an engineer loop.
Live postings: UNVERIFIED. This is the one top-10 lab where “I will live in your HQ city” is not theatre.

7) Hugging Face
Role titles they actually use: Developer Advocate, ML Engineer, Open-Source Engineer, Community, Partnerships, Technical Writer, Expert / Education (course, leaderboard, docs). This is the lab whose hiring machine is *designed* for your mechanism (public artifact → staff).
Locations: remote-friendly as a cultural FACT (company has said so for years); legal entities in US and France; NYC / Paris gravity for some teams.
Bar vs you:
- Community / DevRel / Education / docs / partnerships: WINNABLE relative to every other top-10. Still not easy — they can hire from a global community of people who already live in `transformers` / `diffusers` / `smolagents` / Hub.
- “ML Engineer” as titled: often a real engineering bar. REJECT-AT-SCREEN if they mean it.
Your risk at HF is not “non-coder.” It is “your artifacts are not *their* artifacts.” A multi-agent consensus repo that is not used inside the HF ecosystem is a polite no.
Live postings: UNVERIFIED. https://huggingface.co/jobs and Greenhouse historically.

8) DeepSeek
Role titles: Research Scientist, Research Engineer, infrastructure. Western DevRel board: not a thing I can verify. Company center is China (Hangzhou).
Locations: China primary. International research hires happened after 2025 visibility; DevRel-for-the-West as a posted family: UNVERIFIED, likely thin.
Bar vs you: REJECT for logistics + language + research caste. Do not build a 90-day plan around DeepSeek DevRel. ESTIMATE.

9) Qwen / Alibaba Cloud
Role titles: Developer Advocate / Evangelist / Cloud AI solutions exist inside Alibaba Cloud’s international arms. Tongyi/Qwen community work exists. Titles will look like Alibaba Cloud DevRel, not “Qwen Developer Educator.”
Locations: Hangzhou/Beijing; Singapore is the realistic international node; some Dubai/EU cloud presence. US is politically messy for Alibaba.
Bar vs you: possible at Alibaba Cloud international DevRel in Singapore if they are hiring, which is UNVERIFIED. Not a prestige-equivalent of Anthropic. Visa path is Employment Pass, not O-1.
Live postings: UNVERIFIED. Alibaba/Alibaba Cloud career sites.

10) Cohere
Role titles: Developer Experience, Developer Advocate, Applied AI Engineer, Forward Deployed / Field, Technical Writer, Partnerships. Cohere for AI (C4AI) is a research-community side door, not a DevRel job, but it is a real surface.
Locations: Toronto (HQ gravity), San Francisco, London, and they have hired remote. FACT: Cohere is Canadian; London is a real office story.
Bar vs you:
- DevEx / DevRel / Education: WINNABLE relative to OpenAI/Anthropic, especially London if you use a UK visa path.
- FDE/Applied Engineer: REJECT-AT-SCREEN.
Live postings: UNVERIFIED. https://cohere.com/careers

---

SECOND TIER (higher hit-rate than the top-10 board, often the actual first offer)

Perplexity — titles: engineering-heavy; API/platform DevRel has appeared as they opened the API. Location: SF. Bar: DevRel possible; FDE/SWE no. UNVERIFIED live.

Together AI — titles: Developer Advocate, Solutions, Partner. Location: SF; some remote. Bar: WINNABLE for DevRel if you are a visible inference/serving person. You are not yet. UNVERIFIED live.

LangChain — titles: Developer Advocate, Community, Field / Solutions, Education. Location: US remote-ish / SF. Bar: this family hired from YouTube/Discord/content in 2023–2024 (Greg Kamradt is the named existence proof). WINNABLE. Also a stepping-stone, not the end goal. UNVERIFIED live.

Replicate — titles: DevRel, community, partnerships. Location: often remote-friendly historically. Bar: people who publish models/demos get in. WINNABLE if you become a Replicate-native builder. UNVERIFIED live.

Modal — titles: Developer Advocate, Solutions. Location: NYC/US. Bar: they sell “pythonic infra”; you must look like you live in that DX. Possible. UNVERIFIED live.

Fireworks — titles: DevRel / Solutions / Partner. Location: SF. Same inference-lab pattern as Together. UNVERIFIED live.

Groq — titles: Developer Advocate, Solutions Engineer. Location: SF / other US. Bar: Solutions often SWE. DevRel possible. UNVERIFIED live.

Scale AI — titles: Prompt Engineer / Staff Prompt (Riley path, 2022), Solutions, Applied, Gov/FDE-like. Location: SF. Bar: the 2022 Twitter-prompt path is mostly closed; 2026 Scale is more operational/gov/enterprise. ESTIMATE: harder than 2022. UNVERIFIED live.

Sierra — titles: Forward Deployed Engineer is the brand. Location: SF. Bar: REJECT-AT-SCREEN. Palantir-descended FDE loop. Do not waste a cycle proving you are not an FDE.

Harvey — titles: FDE / Applied / Solutions, some GTM-technical. Location: SF / others. Bar: REJECT-AT-SCREEN for FDE. Legal-tech domain you do not have. UNVERIFIED live.

Cursor / Anysphere — titles: engineering, research, sometimes developer-facing. Location: SF. Bar: they hire people who live in the coding-agent product. Your “AI co-builder shipped orchestration” story is closer to their *religion* than to Sierra’s. DevRel as a titled seat may be thin (the product evangelizes itself). ESTIMATE: better as inbound-from-artifact than as board. UNVERIFIED live.

ElevenLabs — titles: Developer Advocate, Solutions, Partner. Location: mixed US/EU (company has a real transatlantic story). Bar: audio-stack specific. WINNABLE only if you make their API your public beat. UNVERIFIED live.

Also relevant, even if you did not list them (ESTIMATE: higher P(offer) than DeepSeek/xAI):
- Vercel (DevRel machine; Lee Robinson path). Not an LLM lab; it is the professional DevRel major-league.
- NVIDIA Developer / DLI / Inception-adjacent DevRel. Heavy, real, visa-capable, AI-adjacent.
- Databricks / Snowflake (Mosaic, Cortex) Developer Advocate + Solutions.
- LlamaIndex, Weights & Biases, Pinecone, Weaviate, Pydantic/Logfire, Neon — ecosystem DevRel. Stepping-stones with the same evaluation you can actually pass.
- Cognition (Devin), Windsurf, Poolside — coding-agent houses. Same “AI-builder” religious fit as Cursor; still an engineering bar.

Team-hire / acqui-hire: real in 2023–2025 (Inflection→Microsoft; Adept talent→Amazon; Character talent stories; various tiny agent wrappers). For a 7-repo family with modest adoption, ESTIMATE: mid-tier (LangChain/Together/HF-ecosystem company) might buy *you*, not the company. Top-10 labs acqui-hire research teams and product surfaces, not DevRel operators. Do not plan on acqui-hire as the primary path.

---

C. BLOCK 3 — RELOCATION AND VISA

Assumption you gave: relocation is on the table; treat as EU-eligible unless noted. Lisbon base. Date of this block: 2026-08-24.

C1. Who actually sponsors non-engineering / DevRel

FACT: companies do not publish “we sponsor O-1 for DevRel and we refuse it for Community Manager.” Policy is case-by-case, attorney-driven, and quieter than engineering sponsorship.

ESTIMATE, from observable behavior (international staff in DevRel/community seats + public “we sponsor” language on career pages):

Will sponsor US work authorization for the right person, including non-SWE, when the hiring manager is insistent:
- Google / DeepMind (industrial-scale immigration machine; H-1B + L-1 + O-1 all exist in their population)
- Meta (same)
- OpenAI (has publicly hired internationally; O-1 is widely believed to be in their toolkit for people who cannot wait for H-1B — this is ESTIMATE of mechanism, not a published DevRel-O-1 disclosure)
- Anthropic (same shape as OpenAI; smaller immigration team, more “if we want you we will figure it out”)
- NVIDIA, Microsoft, Amazon (not in your top-10 but they actually have the machine)

Will sponsor in *their home country* more easily than in the US:
- Mistral: France. For an EU-eligible person this is not “sponsorship,” it is an employment contract. This is the single largest mechanical unlock in your list.
- Hugging Face: France/US entities; remote-first reduces visa need; when they need a US entity hire they can do it but they would rather not if remote works.
- Cohere: Canada (work permit / Global Talent Stream historically for tech) and UK (sponsor license). US hire is possible but not their easiest door.

Quietly reluctant or structurally bad at non-SWE US sponsorship (ESTIMATE, not a leaked policy):
- Most Series B–D inference/agent startups (Together, Fireworks, Groq, Modal, Replicate, Anysphere, Sierra, Harvey, ElevenLabs). They will sponsor a Staff SWE. They will hesitate on DevRel because (a) H-1B timing, (b) “specialty occupation” memos for Community/DevRel are harder than for “Software Engineer,” (c) they can hire a US-person DevRel instead.
- xAI: unknown, probably engineering-first, culturally impatient with visa calendars.
- DeepSeek: not a US-sponsorship story.
- Qwen/Alibaba: Singapore EP or China; US is the wrong continent politically.

UNKNOWN: I could not verify a written “we do not sponsor DevRel” policy for any lab above. Absence of international DevRel names at a company is a proxy, not a rule.

C2. Realistic US path for *this* packet (PhD, h=7, i10=5, ~137 citations, OSS, public preprints)

Calendar constraint that kills amateur plans: this is 24 Aug 2026. The H-1B cap lottery for FY2027 already ran in March 2026. Next regular-cap lottery is March 2027 for an October 2027 start. FACT of how the H-1B calendar works (USCIS annual registration). H-1B is not a 90-day path from today. It is a 14-month path at best, with lottery risk.

Routes, bluntly:

O-1A (extraordinary ability, employer-sponsored I-129)
- Speed: fastest US *employment* path that is not lottery-based. With premium processing, USCIS adjudication is still on the order of days-to-weeks after filing; end-to-end including attorney packet, evidence, and consular processing is typically ~2–5 months if the packet is ready and the company is fast. ESTIMATE of 2026 wall-clock; fees change, premium processing exists as a USCIS product.
- Robustness: medium. It is discretionary. A Request for Evidence (RFE) is common. DevRel is a weaker O-1 story than “staff researcher with 5k citations.”
- Evidence they actually demand (8 criteria, need 3; in practice a serious petition builds 4–5): original contributions of major significance; authorship of scholarly articles; judging the work of others; critical/essential role at a distinguished organization; high salary relative to the field; press in major outlets; memberships that require achievement; (rarely) commercial success.
- Your packet as it stands: scholarly criterion is *present but thin*. h=7 / ~137 citations is not what USCIS examiners see in successful AI-research O-1s (those packets look like hundreds-to-thousands of citations, venue prestige, letters from famous researchers). You must win on original contributions (the OSS + consensus system), critical role, judging (hackathon judge, program committee, cohort selection), and press. That means the 90-day work is evidence-manufacturing, not form-filling.
- Cost: ESTIMATE $8k–25k all-in (attorney + USCIS filing + premium). Do not treat this as a quote. Company usually pays.
- Who it fits: OpenAI, Anthropic, Google, Meta will run O-1 when they want the person and H-1B is closed. A Series B startup’s first DevRel: less certain.

EB-1A (self-petition extraordinary ability, immigrant)
- Speed: slower than O-1 to a work-start, more robust as a status if approved. I-140 can be premium-processed; then consular processing or AOS. For a Portuguese/EU-born applicant, chargeability is generally current — this is the key advantage versus India/China. ESTIMATE end-to-end 8–18 months if current and clean; not a 90-day start-work tool.
- You can self-petition without a job, then port into an offer. This is the “I might move first” nuclear option. The evidence bar is similar-to-higher than O-1A.
- Cost: ESTIMATE $10k–30k attorney+fees. You can pay this yourself.

H-1B
- Lottery, annual, next useful shot March 2027. Master’s/PhD cap is a slightly better bucket than regular, not a guarantee. Odds fluctuate by year; anyone quoting a stable percentage is selling something. Specialty-occupation memo for “Developer Advocate” is doable (degree + specialized role) but weaker than SWE. Some employers refuse to try.
- Cap-exempt: universities, nonprofits, certain research orgs. DeepMind-as-Google is not your cap-exempt cheat code. Don’t romance this.

L-1
- Irrelevant unless a company employs you abroad for 1 year in a qualifying affiliate then transfers you. Only real if you join Mistral Paris or HF France or Google EU *first*, then transfer. This is a 12–24 month path, not a first move. It is a valid two-step.

Cap-exempt / other US:
- O-1 is the serious US answer for 2026 H2–2027 H1.
- TN: no (not Canadian/Mexican).
- E-2: Portugal is a treaty country, but E-2 is for investors in *their own* US business, not for a W-2 at Anthropic.

C3. EU-eligible unlocks (this is your actual edge)

FACT, if you are an EU citizen or otherwise have EU right-to-work:
- Paris (Mistral, HF France, Google Paris, Meta Paris, possibly others): you show up and sign a CDD/CDI. No visa drama. This is a structural advantage American applicants do not have.
- Amsterdam: highly usable (Google has presence; EU tech scene; Booking/Adyen/etc. are not the goal but the city is an easy EU base). Frontier-lab density is lower than Paris/London.
- Berlin/Munich: Google Munich, EU Blue Card not needed if you already have EU rights.
- Zurich: EU citizenship does **not** give you free Swiss work rights. Switzerland is EFTA/bilaterals with quotas. Google Zurich and Meta Zurich hire internationally but the company must run a permit. DevRel is harder to justify than a scarce research engineer. ESTIMATE: Zurich is a real Meta/Google engineering city and a weak DevRel-from-Lisbon city.
- London: Brexit. EU passport does **not** give you UK right-to-work. You need a Skilled Worker visa (employer sponsor license) or, more interestingly for you, a Global Talent visa (digital technology / research). Global Talent is designed for people with field recognition and does not require a job offer at application. Endorsing bodies and the exact 2026 Home Office handbook are the source of truth (gov.uk). ESTIMATE: Global Talent is one of the two best geographic plays you have (the other is Paris as EU). Timeline often a few weeks to a few months after endorsement, not a lottery.

C4. Does city-choice raise odds? Yes. Not a little.

ESTIMATE, ranked:
1. Paris + Mistral (or HF France): largest odds lift per unit of personal inconvenience. HQ city, EU paperwork already solved, they need European developer activation, your accelerator/cohort skill is locally scarce.
2. London + DeepMind/Google AI DevRel or Cohere: real seats, Global Talent visa is the door, city is a lab magnet.
3. San Francisco + Anthropic/OpenAI/Meta/Cursor/Together: still the volume of seats. Visa is the tax. Being *willing* to relocate is necessary but not sufficient; being *already there* for 6 weeks of overlapping coffees is a different sport. ESTIMATE: a 4–8 week SF residency with a public reason to be there (talks, office hours, hackathons) raises inbound odds more than 40 cold applications.
4. Zurich: only if a Google/Meta req is real and they will run the permit. Do not move speculatively.
5. Singapore: Alibaba/Qwen, Google, some China-adjacent labs. EP salary floors apply. Only if you have decided Asia is the product.

C5. Named cases of O-1 specifically for DevRel/community, not researchers

UNKNOWN as a documented class. I will not fabricate names of DevRel people whose public immigration status I cannot verify. US visa status is usually private.

What can be said without lying:
- Google/Meta/Microsoft run O-1s for many non-research specialists (including developer relations internally). That is ESTIMATE from the existence of large immigration teams and public international DevRel staff, not a named petition.
- OpenAI/Anthropic almost certainly have used O-1 for people they wanted when H-1B was closed. Whether those people were DevRel vs research vs engineering is UNVERIFIED per name.
- Hugging Face’s remote-first posture means many international community hires never needed a US O-1 at all. That is a feature: you can be on the team from Lisbon or Paris.
- Do not count on “DevRel O-1” as a well-worn groove. Count on: (a) EU work in Paris/Amsterdam as the default, (b) UK Global Talent for London, (c) US O-1 only when a top-lab manager is already in love with you and their attorney says the packet will fly. The packet will not fly on h=7 alone.

---

D. BLOCK 4 — THE OSS DOOR: NAMED CASES AND THE FAILURE SIDE

I am restricting to named, public, reverse-engineerable sequences. Where the first-contact moment is not public, I label it UNKNOWN rather than invent a DM.

SUCCESS CASES (sequence, not vibes)

1) Alex Albert → Anthropic (Claude Relations), 2023–2024
- What he built: public Claude usage, jailbreak/capability posts, high-signal Twitter demonstrating the product better than the company sometimes did.
- Duration visible before hire: months of posting, not years of PRs.
- First contact: UNKNOWN in the sense of “who sent the first email,” but the direction of the story as told in public is company-noticed-the-person, not cold-apply. Inbound-from-reputation.
- Resulting role: Claude-facing relations (titles shifted in public usage: relations / community-facing Claude).
- Reverse-engineer: he was a *user-evangelist of that exact model*, not a generic “AI community” person. The company hired its most useful public interpreter.
- Sources: his X account and contemporaneous coverage 2023–2024; Anthropic public presence. Live title today: UNVERIFIED.

2) Logan Kilpatrick → OpenAI DevRel (2023), then Google AI Studio (2024)
- What he built: Streamlit community/DevRel franchise, public teaching, a developer audience that already existed.
- Duration: years of developer-facing work, not a single repo.
- First contact: UNKNOWN exact email; the mechanism is lateral hire of a known DevRel, not a first-time OSS contribution.
- Resulting role: Head of Developer Relations / developer-facing leadership at OpenAI; later Google.
- Reverse-engineer: labs hire DevRel from DevRel. The OSS is proof; the *audience* is the asset.
- Sources: OpenAI/Google public announcements; his posts 2023–2024.

3) Riley Goodside → Scale AI, Staff Prompt Engineer, 2022
- What he built: public GPT-3 prompting threads that became the field’s early textbook.
- Duration: intense public streak measured in months.
- First contact: inbound from reputation (the standard telling: Scale noticed Twitter).
- Resulting role: staff-level prompt/applied, not “Community Manager.”
- Reverse-engineer: a single sharp public artifact, tightly on the company’s problem, beat a résumé. This window was 2022. ESTIMATE: the exact trick is mostly closed; every lab is now drowning in prompt-Twitter.
- Sources: his posts; contemporaneous reporting 2022.

4) Philipp Schmid → Hugging Face (from prolific AWS/HF tutorial work)
- What he built: high-quality, high-volume technical tutorials on transformers/SageMaker/HF stack; public teaching.
- Duration: years of content.
- First contact: UNKNOWN exact; mechanism is the HF community-to-staff machine.
- Resulting role: technical / DevRel-shaped engineering-education seat at HF (public bio trail).
- Reverse-engineer: he taught *their library* better than they did, in public, for a long time.
- Sources: his blog/HF profiles, 2022–2025.

5) Omar Sanseviero → Hugging Face Head of / Chief Evangelist-style DevRel
- What he built: ML GDE-style public education, course material, relentless public Hub/transformers teaching.
- Duration: years.
- First contact: community visibility; he was already a public educator before the HF title.
- Resulting role: developer-relations leadership at HF.
- Reverse-engineer: same HF machine. Later career moves UNVERIFIED here.
- Sources: public bios, HF announcements, talks 2022–2024.

6) Lewis Tunstall, Leandro von Werra, Pedro Cuenca, Clémentine Fourrier → Hugging Face
- What they built (clustered because it is one machine, not one lottery ticket):
  - Tunstall: *NLP with Transformers* book + HF course education.
  - von Werra: TRL and open post-training software.
  - Cuenca: diffusers / on-device / community engineering in public issues.
  - Fourrier: Open LLM Leaderboard — a public evaluation surface the whole field used.
- First contact: each was visible in the HF ecosystem *before* or as they joined; HF’s stated practice is hiring contributors.
- Resulting roles: staff ML/community/eval/education.
- Reverse-engineer: HF does not hire “a person who does OSS.” They hire “a person whose OSS is already part of our product surface.” Leaderboard and TRL are not drive-by PRs.
- Sources: HF blog, library author lists, book, leaderboard posts, 2022–2025.

7) Greg Kamradt → LangChain (DevRel / applied-education), 2023–2024
- What he built: YouTube/tutorials on RAG and LangChain that became default student material.
- Duration: months of content timed to the 2023 RAG explosion.
- First contact: UNKNOWN exact DM, but the public story is content → company. LangChain in 2023–2024 hired out of its own student body.
- Resulting role: developer-facing / applied education at LangChain (later moves UNVERIFIED; the hire-in is the point).
- Reverse-engineer: he did not send a PR and wait. He became the unofficial instructor of their product.
- Sources: his YouTube; LangChain public team pages 2023–2024.

8) Lee Robinson → Vercel (DevRel → VP-level developer education)
- Not an LLM lab. Included because it is the cleanest professionalized version of the same sequence in developer tools: public Next.js teaching → hired to do that inside the company.
- Reverse-engineer: this is what “DevRel hire” looks like when the company is good at it. LLM labs copy this when they mature.
- Sources: Vercel public; his talks 2020–2024.

9) Swyx (Shawn Wang) → Temporal Head of DevRel (pre-LLM-lab, same mechanism)
- Public writing + community → Head of DevRel at an infra company. Later independent (Latent Space).
- Reverse-engineer: writing a public Developer Experience canon got him the seat. He did not remain at a frontier lab; the mechanism still holds at infra.
- Sources: his writing; Temporal public 2021–2023.

Hugging Face as a system, not an anecdote: if you want a *repeatable* OSS door, it is HF (and, in 2023, LangChain Discord). Anthropic/OpenAI OSS doors exist but they look like “you became our public interpreter,” not “you merged a cookbook PR.”

FAILURE CASES (the side that matters)

1) Drive-by cookbook / example PRs
- What happened: hundreds of people merged a sample into `openai-cookbook`, `anthropic-cookbook`, `mistralai` examples, HF Hub cards. Almost none were hired.
- Why: a merged PR is a polite thank-you, not a dependency. Maintainers distinguish “sent a notebook” from “own a surface we would miss if you vanished.”
- This is the failure mode that most resembles your current evidence. ESTIMATE, blunt: merged vendor-cookbook PRs are table stakes for a conversation, not a hiring packet.

2) Viral agent repos of 2023 (AutoGPT / BabyAGI class)
- Named adjacent: Toran Bruce Richards (AutoGPT / Significant Gravitas) and Yohei Nakajima (BabyAGI) became famous. They did not become OpenAI/Anthropic DevRel. They stayed founder/indie/VC.
- Why: the artifact was a demo of the *model vendor’s* API, not a piece of the vendor’s developer platform. Labs do not hire every person who wraps their API. They also became “founders,” which triggers a different committee.

3) Georgi Gerganov (llama.cpp, whisper.cpp, ggml)
- What he built: actually-important infrastructure. Adoption numbers that dwarf a 7-repo family.
- Result: as far as public record goes, he remained independent rather than becoming a lab DevRel/engineer. Offers almost certainly existed (UNKNOWN specifics; do not invent). The failure-to-convert-into-a-lab-job is either “he did not want the job” or “the labs could not buy the person without buying a threat to their stack story.”
- Reverse-engineer: even world-class OSS does not automatically become a DevRel offer. Sometimes it becomes leverage to *never be hired*.

4) Simon Willison (Datasette, LLM CLI, shotgun debugging of model behavior)
- Result: not hired by a lab. Became more influential outside.
- Why: he is more useful to himself than to a corporate DevRel org, and his public stance is independent. Labs sometimes *consult* this shape; they rarely *employ* it as a junior-or-mid DevRel.

5) “I shipped a GPT / a Hugging Face Space / a hackathon agent”
- Volume failure. 2023–2024 filled the Hub and the GPT Store with undistinguished artifacts. No inbound. Why: no users, no unique insight, no relationship with a maintainer, no follow-up.

6) Crypto-native builders pivoting with a thin LLM wrapper
- Pattern, not one name: 2022–2024 AI labs treated “ex-Solana DevRel” as a smell unless the person had already built a serious LLM artifact. Why: they have been burned by narrative-first people. Your accelerator skill is real; your tokenomics history is a tax until overwritten by LLM-stack proof.

What the success sequences share, mechanically:
- Time-on-target on *that company’s* surface (Claude, Streamlit, transformers, LangChain, prompting-as-Scale’s-problem).
- Audience or dependency: either lots of developers already learn from you, or a library they already use.
- Inbound direction: the company pinged them, or a staff engineer intro’d them, more often than they cold-applied.
- Duration: 3–18 months of visible, on-topic work. Not a weekend PR.
- The moment they noticed: a staff engineer saw the artifact in the wild (Twitter, a customer using it, an internal Slack “who made this,” a Discord answer that saved them). That moment is almost never the careers page.

What they do not share: a PhD, a citation count, or a multi-lab scattershot.

---

E. BLOCK 5 — MONITORING SPEC (MACHINE-READABLE SURFACES)

E0. Discovery rule (do this; do not hardcode slugs forever)
Companies switch ATS. The durable monitor is:
1. Fetch the careers URL.
2. Extract `ashbyhq.com`, `greenhouse.io`, `lever.co`, `workable.com`, `smartrecruiters.com`, `welcometothejungle.com`, `teamtailor.com`, `recruitee.com`, or in-house.
3. Hit the matching public jobs API with the slug you just extracted.
4. Diff against yesterday. Alert on new reqs whose title matches a role-family regex, not on all SWE.

All slugs below are LAST-KNOWN, not live-verified on 2026-08-24. Treat as seeds for the discovery rule.

E1. ATS patterns (public, no auth)

Ashby Job Board API
- Docs: https://developers.ashbyhq.com/docs/job-board-api
- JSON: `GET https://api.ashbyhq.com/posting-api/job-board/{boardName}`
- Optional: `?includeCompensation=true`
- Human board: `https://jobs.ashbyhq.com/{boardName}`
LAST-KNOWN board names (UNVERIFIED today): `anthropic`, `xai`, `mistral`, `perplexity`, `togetherai` / `together-ai`, `langchain`, `anysphere`, `elevenlabs`, `harvey` / `harveyai`, `sierra` / `sierra-ai`, `modal`, `fireworks` / `fireworks-ai`, `cursor` (if split from anysphere). OpenAI has historically been Greenhouse, not Ashby — do not assume.

Greenhouse Job Board API
- Docs: https://developers.greenhouse.io/job-board.html
- JSON list: `GET https://boards-api.greenhouse.io/v1/boards/{board_token}/jobs`
- With content: `?content=true`
- One job: `GET https://boards-api.greenhouse.io/v1/boards/{board_token}/jobs/{id}`
LAST-KNOWN tokens (UNVERIFIED today): `openai`, `huggingface`, `cohere`, `scaleai`, `groq`. Meta/Google are not Greenhouse.

Lever
- Docs: https://github.com/lever/postings-api
- JSON: `GET https://api.lever.co/v0/postings/{site}?mode=json`
Use only if discovery finds `jobs.lever.co/{site}`.

SmartRecruiters
- `GET https://api.smartrecruiters.com/v1/companies/{companyIdentifier}/postings`
Rare at frontier labs.

Workable
- Widget: `https://apply.workable.com/api/v1/widget/accounts/{account}`
Rare here.

Welcome to the Jungle (EU, Mistral-adjacent companies use this)
- Human: https://www.welcometothejungle.com
- They have internal JSON the site uses; a stable public documented API for third parties is weaker than Ashby/Greenhouse. ESTIMATE: scrape-of-XHR is how people poll WTTJ; that is ToS-grey. Prefer: if Mistral also has Ashby, poll Ashby.

Google
- Human: https://careers.google.com
- Search XHR has existed at paths like `https://careers.google.com/api/v3/search/` with query params. UNOFFICIAL, changes, ToS-grey. DeepMind: https://deepmind.google/careers (often a subset / separate CMS). Poll both.

Meta
- Human: https://www.metacareers.com
- API has been GraphQL and has changed repeatedly. No stable public “boards-api” equivalent. ESTIMATE: HTML/XHR monitor, or a third-party aggregator, is the practical sensor.

DeepSeek / Alibaba / Qwen
- In-house career pages, China-centric. No reliable Ashby/Greenhouse assumption. Poll the HTML careers URL; expect breakage.

Company career URLs to seed (human, then discover ATS):
- https://www.anthropic.com/careers
- https://openai.com/careers
- https://deepmind.google/careers
- https://careers.google.com
- https://www.metacareers.com
- https://x.ai/careers
- https://mistral.ai/careers (or /jobs — confirm by fetch later)
- https://huggingface.co/jobs
- https://cohere.com/careers
- Perplexity / Together / LangChain / Replicate / Modal / Fireworks / Groq / Scale / Sierra / Harvey / Anysphere / ElevenLabs: usually `https://jobs.ashbyhq.com/{slug}` or a `/careers` that redirects there.

E2. Aggregators worth polling

Worth it:
- Hacker News “Who is hiring?” monthly thread. Cadence: first Monday of the month (community custom, FACT of HN culture for years). Machine: Algolia `https://hn.algolia.com/api/v1/search?query=who%20is%20hiring&tags=story` then filter by current month, then fetch comments via `https://hn.algolia.com/api/v1/items/{story_id}` or Firebase `https://hacker-news.firebaseio.com/v0/...`. This is one of the best LinkedIn-substitutes for early, founder-written reqs.
- Remote OK JSON: `https://remoteok.com/api` (noisy; filter by company allowlist).
- Ashby/Greenhouse themselves, polled per company, beat any aggregator for recall.
- YC Work at a Startup: https://www.workatastartup.com — useful for YC-batch AI cos (not the top-10). No first-class public API I can verify; page JSON/XHR. Cursor/Anysphere is not a typical WaaS hunt.
- a16z talent/jobs surfaces: marketing, not a reliable API. Low priority.
- EU: Welcome to the Jungle; Berlin Startup Jobs RSS historically; EURES is too noisy.

Weak / avoid as primary:
- Wellfound (AngelList): login-walled, anti-bot, no trustworthy public API anymore.
- Otta: login-walled, UK-strong, no public API. Useful if you later get an account; not a machine surface.
- ai-jobs.net: human-curated; RSS/site scrape may exist but I will not invent an endpoint. Secondary.
- LinkedIn jobs API: you have no usable LinkedIn. Official Marketing/Jobs APIs are partner-restricted. Do not build a fantasy LinkedIn poller.

E3. Early-signal channels (role appears before the board)

X / Twitter (this is your actual LinkedIn replacement for inbound):
Poll these classes, not a giant influencer list:
- Company jobs/dev accounts: @OpenAI, @OpenAIDevs, @AnthropicAI, @MistralAI, @huggingface, @Cohere, @xai, @GoogleDeepMind, @GoogleAIStudio, @PyTorch, @metaai, plus each second-tier @company.
- Named DevRel / relations leads whose posting historically precedes or replaces a req: Logan Kilpatrick (now Google-side), Alex Albert (Anthropic-side), Omar Sanseviero (HF-side), company-specific developer-relations staff as they appear. Handles change; resolve by current company bio, not a frozen list.
- Recruiters who post reqs with Ashby links before the career page updates. Discover by watching who posts `jobs.ashbyhq.com/anthropic` links.
- Do not worship generic AI-Twitter. Watch people who can open a req.

Discord / Slack (high signal, bad API):
- Official: Anthropic Discord, OpenAI developer community / forum, Hugging Face Discord, LangChain Discord, Llama (Meta) Discord, Mistral’s community surface if live, Cohere Discord.
- Hiring signal is usually `#jobs` or a staff message, not a formal req. You will need a logged-in user bot or a human relay. ToS: respect each server’s rules; many forbid scraping.
- HF Forums (https://discuss.huggingface.co) historically beat HF jobs page for “we are looking for someone to own X.”

HN monthly: already covered.

Mailing lists / calendars:
- Google for Developers / GDE newsletters.
- Hugging Face newsletter.
- Meetup.com / Luma / Partiful calendars for SF, Paris, London LLM meetups. A “we’re hiring, come talk” at a meetup is weeks ahead of ATS. Poll Luma/Meetup for host ∈ company allowlist.
- University/lab mailing lists: less relevant to you.

E4. Non-HR side doors (these are hiring funnels)

Rolling vs cohort, with honesty about dates. Windows move; verify before applying. All “currently open” = UNVERIFIED on 2026-08-24.

- Anthropic / Claude Community Ambassadors (or successor name): this class of program is a real, designed top-of-funnel for relations hires. Typically application cohort, not rolling. If a window is closed, the move is to act like an ambassador in public anyway (the Alex Albert move).
- Google Developer Experts (ML / Cloud / AI): cohort/review cycles, ongoing. This is the longest-running DevRel farm system in the industry. Direct feeder to Google DRE conversations. https://developers.google.com/community/experts
- Cohere for AI (C4AI): research community, scholars, groups. Cohort. Converts more to research than DevRel, but London/Toronto staff see you. https://cohere.com/research/cohere-for-ai (confirm current URL).
- Hugging Face: no need for a named “ambassador” title — Hub + libraries + course contributions *are* the program, rolling, 365 days. Grants have existed; windows sporadic.
- Meta Llama: community / impact grants have existed in waves. Cohort when a call is open. DevRel hire correlation is weaker than HF.
- NVIDIA DLI / University Ambassador / Inception: cohort, real, but NVIDIA not a top-10 LLM lab. Useful as credibility.
- AWS Heroes / ML Hero, Microsoft MVP: cohort, real DevRel farms, not LLM labs.
- LangChain Ambassadors / Champions: existed in the 2023–2025 growth era; confirm if still alive. Cohort/rolling mixed.
- OpenAI: startup fund / researcher access / customer councils — not a clean ambassador program. Forum moderators and standout API builders get inbound. Rolling in practice.
- Contract / contract-to-hire: quietly common for developer education (course build, workshop tour, docs sprint, 3-month contractor). This is often the *actual* first paid relationship. No public window; it starts from a staff intro. Monitor: staff saying “we need someone to write the agents course.”
- Residencies / fellowships that are *not* your door: MATS, GDM internships, Meta Residency — these are research-engineer farms. Do not apply as a camouflaged researcher.

E5. No LinkedIn: what compensates, what you lose

Compensates reasonably:
- ATS APIs (the reqs themselves)
- X (hiring managers, inbound reputation — this is how Alex Albert and Riley Goodside were hired)
- Discord/forums of the target labs
- HN Who is hiring
- Your public GitHub + blog + talks as the searchable packet recruiters *do* find even without LinkedIn (Google search, not LinkedIn search)
- Employee intros via GitHub issue threads and Discord, which is how OSS hiring actually works

Structural loss (do not cope this away):
- Recruiter InMail. You will not be in the Boolean string `("developer advocate" OR devrel) AND (LLM OR "large language")`.
- Easy graph of “2nd degree at Anthropic.” Intros cost more work.
- Company alum mapping (who left OpenAI DevRel last month).
- Salary/level comps on the role.
- “Easy apply” volume, which you should not want anyway.
- Proof-of-employment optics for some EU recruiters who still live inside LinkedIn.

Compensation architecture: a public, boring, always-on packet (site + GitHub + X + one canonical “what I will do for your developers” page) plus ATS polling plus 2–3 staff relationships per target lab. That is the substitute. Building a fake LinkedIn presence later is optional; it is not the 90-day bottleneck.

---

F. BLOCK 6 — ANTI-PATTERNS (WHAT KILLS THIS PROFILE)

Sources: hiring-manager public writing and talks, not a secret rubric. Where I do not have a named quote, it is labeled ESTIMATE.

(a) Crypto / Web3-heavy history

FACT-adjacent: 2022–2024, after the FTX/crypto-winter overlap with the LLM boom, AI lab culture treated crypto résumés as a prior on “narrative over substance.” This was said out loud on X, in podcasts, and in hiring backchannels; it was not published as an official screen. ESTIMATE: it has softened in 2025–2026 because a lot of the competent operators moved, but it has not died.

What they actually screen out:
- Tokenomics as the lead story
- “Community growth” that meant paid shills / airdrop campaigns
- DAO governance theatre with no shipped developer product
- Job-hopping across three L1s in two years

What they do not automatically screen out:
- Hackathon ops, developer cohorts, incubation, if you can point at people who shipped
- Cryptography/ZK as a PhD topic, if you do not sell it as “so I understand LLMs”

Killer move: leading with crypto. Survival move: one line of biography, then immediately the LLM artifact. Your multi-actor systems / cohort-activation is the transferable skill; “tokenomics” is the slur they will use if you let them.

(b) Explicitly AI-assisted rather than hand-coding

This is the live wire in 2026.

ESTIMATE of the actual screen, role by role:
- FDE / Solutions / Partner Engineer / DevRel *Engineer*: they will run a live coding or take-home. “My co-builder writes it” fails in the room, in front of the engineer whose veto is the hire. This is not a philosophy debate. It is a 45-minute loop.
- Developer Education / Technical Writer / Community / classic Advocate: they will ask for a writing sample, a recorded workshop, a doc PR, a curriculum. AI-assisted production is fine if the *taste* is yours and you can go off-script when an engineer heckles you.
- The respect test, said in public by multiple DevRel leaders over years (the Swyx/Lee Robinson/Tomomi Imura-type canon, not a lab HR doc): DevRel fails when engineering decides you cannot be left alone with a customer. That decision is often made on “did they debug in front of me.”

Named tension, not a lab quote: the industry is simultaneously (1) shipping AI coding tools and (2) interviewing as if they did not. Cursor/Anysphere is the company where your co-builder story is *on-mission*. Sierra/Harvey/Palantir-FDE is the company where it is disqualifying. OpenAI/Anthropic sit in between and default to the engineer veto.

Do not lie about it. Do not lead with it in an FDE loop. Put the repos in front of them and offer “watch me ship a integration this afternoon” as the exam, not LeetCode.

(c) Founder applying to be an employee

Classic screens, none of them unique to AI:
- Flight risk (you will leave in 9 months to found again)
- Cannot take a manager
- Will rebuild their DevRel org into your startup
- Compensation expectation at founder-fantasy levels
- “Team hire” talk when they wanted one IC

Counter-pattern that works (ESTIMATE): time-box the commitment in your own head (24–36 months), talk about *their* developers not your legend, and if you want a team-hire, sell a *surface they already feel they lack* (eval harness, education track, EU developer activation), not “buy my seven repos.”

Other killers that are not in your three bullets but will kill you anyway:
- Scattershot: applying to research, FDE, DevRel, and “any AI role” at the same lab in one week. Looks like lottery.
- Cold 800-word manifesto to a general jobs@ alias.
- Claiming cookbook PRs as if they were core infra.
- Public crypto-maximalist residue that a staff engineer can find in 90 seconds of Google.
- Asking for remote-from-Lisbon as the opening move at a SF-HQ relations team. Relocation is available — say the city first.
- Slop: AI-flavored outbound. These are the people who built the slop detectors.

On “what hiring managers say they screen out”: I will not invent quotes from unnamed Anthropic recruiters. The consistent public DevRel-manager line across 2020–2025 (tooling companies + labs) is: no engineering respect, no teaching proof, no specific product fluency, founder-as-tourist. Your three risks map onto those exactly.

---

G. BLOCK 7 — THE 90-DAY GAP CLOSE

Be blunt. Ranked by leverage on P(offer at a top-10 or a real feeder).

GAP 1 (highest leverage): you are not yet a public interpreter of any one lab’s stack.
Your artifacts prove “I can operate a multi-agent system.” They do not prove “developers who want to ship on Claude/Mistral/HF become productive because of me.” Every named success in Block 4 was on-stack for the hirer.
Close in 90 days:
- Pick two, not ten: Anthropic (Claude) and Mistral, with HF as the always-on OSS rail.
- Ship one flagship, boringly useful thing per stack that other developers actually run: not another agent framework. An eval/recipe/reference architecture with a number (time-to-first-successful-agent, cost, failure modes) and a weekly office-hour.
- Threshold that matters: ESTIMATE, not a magical star count. A cookbook PR is ~0. A repo staff engineers mention in public, or 300–1,000 real downstream users, or a workshop that a lab’s own DevRel shares, is the beginning of inbound. llama.cpp-scale is not available in 90 days; “mentioned by a staff engineer” is.
- Kill metric: if nobody at the target lab has reason to type your name in Slack, you did not close the gap.

GAP 2: the FDE/DevRel-Engineer coding loop will fail you, and you keep language that invites that loop.
You will not become a hand-coder at their bar in 90 days. Pretending is how you burn the only Anthropic intro you get.
Close in 90 days:
- Target titles: Developer Relations, Developer Education, Community, Technical Writer, Partnerships, “Claude Relations”-shaped. Refuse to self-identify into Forward Deployed / Solutions Engineer / Partner Engineer unless the manager explicitly waives the coding loop.
- Change the exam: public weekly shipping log on *their* SDK, so the interview is a walkthrough of your repo and a live teach-back, not LeetCode.
- One recorded 25-minute workshop: “take a working engineer from zero to a production-ish agent on Claude / on Mistral,” with heckle-resilience. This is the portfolio piece DevRel loops actually watch.

GAP 3: evidence packet for O-1 / Global Talent / “why you” is thin where labs and attorneys look.
h=7 / 137 citations will not carry an O-1. Cookbook PRs will not carry a hiring manager. Crypto will be Googled.
Close in 90 days (this is the same work as Gap 1, re-packaged as evidence):
- Two independent letters-in-waiting: people with recognizable lab or ecosystem names who will write “original contribution / critical role” in attorney English. Earn them by being useful, not by asking on day 2.
- Judging: run or judge a public developer cohort/hackathon on the target API (you already know how to run cohorts — do it on Claude/Mistral, not on a chain).
- Press: one non-vanity technical write-up that a lab or a serious newsletter links. Not a startup launch.
- A single canonical page: who you are, the honest non-coder+co-builder positioning, the two artifacts, the city you will move to, the role family you want. No crypto hero narrative.

What is not a 90-day gap (do not waste the quarter):
- Raising h-index
- Getting an arXiv cs.MA preprint to “land” as if it were NeurIPS
- Learning to pass Google’s SWE onsite
- Spray applications to DeepSeek/xAI/Sierra FDE
- Building a LinkedIn from zero as the main workstream

Odds math, ESTIMATE, so you do not romanticize:
- Cold board apply to OpenAI/Anthropic DevRel today: very low single digits, maybe worse.
- On-stack public interpreter + one internal champion + willing to sit in Paris or SF: the path that actually produced the named hires.
- Feeder offer (HF / LangChain / Mistral DevRel / Cohere London / Together) in 1–2 quarters is a more realistic first win than Anthropic staff DevRel in 90 days. Take the feeder if it puts you in the graph.

---

H. PRIORITIZED ACTION LIST

This week (expected value: set the sensor and pick a city, stop spraying)

1. Build the monitor, nothing else: Ashby+Greenhouse poller for the seed slugs + careers-page ATS discovery + HN Algolia on the monthly thread + an allowlist of company X accounts. Alert on title regex: `(devrel|developer relations|developer advocate|developer education|developer experience|technical writer|community|partner engineer|solutions architect|forward deployed)`. Do not alert on generic SWE.
2. Choose the city you will say out loud: Paris (default, EU, Mistral/HF) or London (Global Talent, GDM/Cohere) or SF (Anthropic/OpenAI, visa tax). Write it on the canonical page. “Lisbon remote” is not a plan.
3. Freeze target titles. Delete FDE/Sierra/Harvey/DeepSeek from the 90-day board. Keep: Anthropic relations/education, Mistral DevRel, HF DevRel/community, Cohere DevEx, LangChain/Together as feeders.
4. Start the flagship Claude artifact this week, not a new framework: a reproducible “operator-grade multi-agent eval + recipe” that uses their API and produces a number. Same skeleton for Mistral the following week.

This month

5. Run a public cohort of actual developers (you know how) on Claude and/or Mistral. Capacity 20–40. Publish what broke. This is both Gap 1 and O-1 “judging/critical role” evidence.
6. Become useful in the two Discords/forums that match the targets. Not launching a personal brand. Answering the hard integration questions with repos, not vibes. Staff notice this; they do not notice cold applies.
7. Record the 25-minute teach-back. Put it on the canonical page.
8. UK Global Talent: if London is the city, start the endorsement packet this month (it is slow in calendar time, fast compared to H-1B). If Paris, talk to Mistral/HF as a person who can start on a CDD/CDI, not as a visa project.
9. Do not send 30 applications. Send 0 until one staff engineer at a target lab has a reason to know your name. Then send 3, each matching a live req noun, each naming the city.

This quarter

10. Get one of: (a) a lab staff engineer publicly or privately using/citing the artifact, (b) a contractor/course build for a lab or a lab-adjacent company, (c) an HF-native library/eval that their community actually runs. Any of these three is a door. Applications are not.
11. Convert that into a champion intro. The ask is not “will you refer me.” The ask is “I want to do this job in your city; does this title match a req you would defend.”
12. If a feeder offer appears (HF, Mistral Paris, Cohere London, LangChain, Together): take it unless it is FDE-with-coding-loop. A feeder in-graph beats another quarter of Lisbon-based mythology about Anthropic.
13. Only then run O-1, and only if the company is US and the manager is already yes. Do not pre-buy an O-1 as if it were a ticket into the lab.
14. Re-evaluate DeepSeek/Qwen/xAI at end of quarter only if a real, named, located DevRel req exists. Until then they are noise.

What not to do at all this quarter: rewrite positioning essays, collect more weakly-related preprints, apply to research scientist reqs, lead with crypto, perform “open to work” on a LinkedIn you do not have, or wait for the public board to love you.

The mechanics, compressed: the board is a sensor; the door is a staff champion; the visa that actually matches your calendar is EU-Paris or UK Global Talent, not H-1B; the OSS door works only when the artifact is on their stack and they would miss you; the 90-day job is to become the public interpreter of one model, in one city, with numbers.
