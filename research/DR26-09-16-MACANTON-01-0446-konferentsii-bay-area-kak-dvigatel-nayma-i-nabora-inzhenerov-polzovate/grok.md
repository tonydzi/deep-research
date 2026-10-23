# DR26-09-16-MACANTON-01-0446 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll pull live sources on event conversion, hiring on the floor, OSS contributor playbooks, and Anton's existing conference notes, then write the full report.Next I’ll open the primary hiring, DevRel, OSS, and Bay Area event pages so claims rest on URLs actually read this run.A. EXECUTIVE ANSWER — Redesign the event machine, do not attend more events

Stop treating Bay Area AI events as reconnaissance. Treat them as a two-pipeline factory: (1) warm referrals into frontier-lab adjacent roles (DevRel, developer advocate, FDE, ecosystem, agent-tooling PM) and (2) engineers who try, issue, and PR your tools. The current default — 3–5 events a week, “see what’s out there,” weak follow-up — is the failure mode you already named. Volume without a close loop is entertainment.

The evidence that actually matters is not “conferences get people jobs.” It is that referrals crush cold applications, and events are only useful if they manufacture referrals and public work.

CareerPlug’s 2024 analysis of 10M+ applications from 60,000+ small businesses: employee referrals were ~2% of applicants and ~11% of hires — about 10× job-board conversion. Applicant-to-interview was 3%; interview-to-hire 27%; ~180 applicants per hire. https://www.careerplug.com/blog/recruiting-metrics-and-kpis/ https://staffinghub.com/hiring/company-pages-referrals-result-in-more-hires-recruiting-metrics-report

Navero’s 2026 funnel synthesis (citing CareerPlug/Gem/Jobvite-class benchmarks): ~0.5% of applicants get offers; referrals/direct sourcing convert 4–10× job boards; referrals ~7% of applications but 30–40% of hires in some datasets. https://www.navero.me/blog/recruiting-funnel-metrics

Anthropic, on its own careers page (opened this run): “If you’ve done interesting independent research, written a thoughtful blog post, or contributed to open source, put that at the top of your resume.” They sponsor visas for eligible roles. https://www.anthropic.com/careers

DevRel hiring managers, updated Feb 2025: public portfolio (talks, writing, OSS, community) beats a resume; much hiring is referral/network, not boards. https://developerrelations.com/guides/how-to-get-a-job-in-devrel/

Frontier-lab pattern from practitioner synthesis: build visible work → get known by people inside → get referred → then apply. Do not pitch as a job-seeker at research events; pitch as someone working the same problems. Conferences accelerate relationship-building; they do not replace it. https://letsdatascience.com/blog/how-to-land-a-job-at-openai-anthropic-or-google-deepmind

OSS conversion is not “I told someone about my repo.” PyCon US 2025 sprints: 370 new PRs to the Python org in four days, ~2× normal velocity; first-time contributor rooms with maintainers in the room. That is the gold standard: same-room, time-boxed, first-PR-ready tasks. https://pyfound.blogspot.com/2025/09/sprints-are-best-part-of-conference.html

The redesign, in one sentence: cut attendance to 2 events per week, make one of them a talk/demo/host slot, pre-select 3–5 named people, show one artifact, close every contact in 24 hours, and fire any event type that does not produce a referral, a demo-try, or a GitHub issue inside 30 days.

Realistic hit rates (OPINION, model — no public per-meetup hire dataset exists for Bay Area AI events; do not treat these as measured):

| Event type | Qualified contacts / night | Referral or interview in 30d | OSS: demo-try / issue / PR in 30d | Verdict |
|---|---|---|---|---|
| Generic mixer / “AI happy hour” | 0–1 | ~0 | ~0 | Skip |
| Talk meetup as guest | 1–3 | 0–1 in 10 nights | 0–1 try | Only if speakers are target-company ICs |
| Talk meetup as lightning speaker | 4–8 | 1 in 4–6 talks | 2–5 tries, 1 issue | Default high-leverage |
| Company-hosted office hours / demo night | 2–5, higher quality | 1 in 3–5 if you show work | 1–3 tries | Prioritize labs/tooling cos |
| Hack night / sprint / project night | 2–4 working pairs | rare for hire | 1 PR per 2–3 nights if tasks exist | Best for OSS |
| Small dinner (8–20) | 3–6 deep | highest per hour | 1–2 serious users | Best for hire |
| AIE World’s Fair class (thousands) | 8–15 if pre-targeted; 0 if wandering | 1–2 referrals if you work the expo + side events | 3–10 tries if you demo | One per year, not weekly |
| TechCrunch Disrupt / founder pitch | lots of cards | near-zero for lab DevRel | near-zero for OSS | Skip for these two missions |

You do not have a conference problem. You have a follow-up and positioning problem. Events are cheap in the Bay Area. Attention and close-loop discipline are the scarce resource.

---

1. EVENT SELECTION

Evidence-based (hiring channel quality)

Referrals and employee-sourced candidates convert an order of magnitude better than cold apps. CareerPlug 2024: referrals 2% of applicants, 11% of hires; careers pages 13% of applicants, 26% of hires; job boards 61% of apps, 42% of hires. https://www.careerplug.com/blog/recruiting-metrics-and-kpis/

Ashby-class data, as summarized in 2026 recruiting writeups: referred candidates application→interview ~40% vs ~12% inbound. https://studojo.com/reports/referrals-vs-applications-how-people-get-hired-2026

That implies the only event types that “convert to hiring conversations” are events where you can leave with a named employee who will put you in an internal referral tool. Booth recruiters at mega-conferences can start a process; they rarely create the internal voucher a frontier lab needs.

Evidence-based (which Bay Area surfaces actually contain those people)

AI Engineer World’s Fair 2026 (Moscone West, Jun 29–Jul 2, 2026; ~6,000 engineers reported by attendees): labs on the floor include OpenAI, Google DeepMind, Amazon AGI Labs; Anthropic speakers on the program (Mike Krieger, Angela Jiang, Katelyn Lesse); OpenAI DevEx (Romain Huet); explicit “debut, hire, and compete” expo language; 65+ side events; New Engineer Orientation; hallway track. This is the one large conference that matches both missions. https://ai.engineer/worldsfair/2026 https://dev.to/hanzla/inside-ai-engineer-worlds-fair-2026-what-6000-engineers-showed-up-to-build-fgm

Latent Space / AI Engineer SF meetups at AWS Builder Loft: lightning talks 5–10 min, hiring pitches explicitly requested (“If you’re hiring, please be prepared with a short 1-2 sentence hiring pitch and perhaps a URL”), speakers from Replit, Microsoft, Minimax, Gemini. This is the weekly high-signal room. https://luma.com/aieng2025 https://luma.com/aieng

Bay Area density right now: foundercal listed 209 SF startup/tech events in September 2026, 75 in one week. Dilution is the tax. https://foundercal.com/cities/sf

Company-hosted and agent-specific meetups (Pinecone, Lightning AI MCP, Bay Area AI Agents SF) put vendor DevRel and product engineers in the room. Those people are referrers into DevRel/DX/ecosystem roles, not research scientist roles. https://luma.com/5lms9659 https://luma.com/ifu54996 https://luma.com/hovkz2i1

SF Python: Presentation Night + Project Night (collaborative coding). Project Night is the OSS-conversion format. https://manifestlaw.com/blog/san-francisco-tech-networking-events/

Counter-evidence you must not ignore

There is no public dataset that says “X% of AI meetup attendees get hired.” Career-fair offer rates (~24% in one virtual-fair vendor claim) are for job fairs, not mixers, and are not frontier-lab numbers. https://jobfairx.com/employer/resources/recruiting-roi

A 2025 career-advice piece argues most “referrals” that show up in hire stats come from former colleagues, not conference badges. Treat hallway chats as seeds, not hires. https://optimcareers.com/expert-articles/networking-events-are-dead

Anthropic engineering composition analysis (1,680 engineers): Google/Meta/Amazon/Microsoft/Stripe dominate prior employers; almost no conventional junior pipeline. Your path is not “I met a recruiter at a mixer.” It is “an IC saw the work.” https://www.techtimes.com/articles/318396/20260615/anthropic-engineering-team-infrastructure-army-new-analysis-1680-engineers.htm

Hackathon-to-hire numbers from India campus placement (70% onboarding among participants in placement-process hackathons) do not transfer to a 45+ Bay Area operator. https://analyticsindiamag.com/it-services/hackathons-emerge-as-recruitment-powerhouses-for-indias-it-sector

Event-type ranking for YOUR two missions (evidence + opinion, labeled)

HIRE (DevRel / advocate / FDE / ecosystem / agent PM)

1. Small dinners and invite-only company nights — OPINION, high. Density of ICs who can refer.
2. Company-hosted technical meetups where their engineers speak — EVIDENCE-adjacent: DevRel hiring happens through those rooms. https://developerrelations.com/guides/how-to-get-a-job-in-devrel/
3. Lightning talk at Latent Space / AI Engineer SF / agent meetups — EVIDENCE: organizers want speakers; audience is the hiring pool. https://swyx.io/cfp-advice https://luma.com/aieng2025
4. AIE World’s Fair expo + side events, one week a year — EVIDENCE: labs on the floor, hire language, badge scan. https://ai.engineer/worldsfair/2026 https://www.ai.engineer/faq
5. Hackathons only if you ship with employees of a target company.
6. TechCrunch Disrupt / generic founder mixers — skip. Wrong buyer.

OSS users / contributors

1. Sprint / project night / hack night with a 10-minute first task — EVIDENCE: PyCon sprint PR spike. https://pyfound.blogspot.com/2025/09/sprints-are-best-part-of-conference.html
2. Lightning demo (not a pitch) — EVIDENCE: CNCF recruiting playbook: in-person workshops + first-PR path. https://contribute.cncf.io/projects/best-practices/community/recruiting-playbook
3. Hosting your own themed meetup (agent memory / second brain / Claude fleet) — EVIDENCE: swyx; organizers control the guest list and the ask. https://swyx.io/manifest-meetups https://dx.tips/meetups-guide
4. Large conference demo table — useful once a year.
5. Mixers with no laptop, no issue, no QR — skip.

Hit-rate honesty: expect ~1 warm referral per 4–8 high-signal nights if you speak and follow up; ~0 from generic mixers. Expect ~1 GitHub issue per 2 demo-nights if you hand people a 10-minute task and ping in 24h. Confidence: medium on ranking, low on exact percentages because nobody publishes meetup→offer tables.

---

2. PRE-EVENT PREP (30–60 minutes, non-negotiable)

Luma guest lists are often public. Luma’s own help: hosts can show/hide “who’s going”; public lists are recommended as social proof. Mine the visible names. Host-only CSV export is not available to you as a guest. https://help.luma.com/p/event-guest-list https://help.luma.com/p/managing-your-guest-list

Prep routine (do this, in order)

Minute 0–10. Score the event. Kill it unless at least one is true: (a) speaker/host works at a top-10 LLM company or a DevRel/DX team you would join; (b) format is demo/lightning/sprint, not drinks; (c) you have a speaking or table slot; (d) 3 named people you already wanted are on the guest list.

Minute 10–25. Target list of 3–5 people. Sources: Luma “Going” names; speaker list; company pages of speakers; X search `from:handle`; LinkedIn “People also viewed.” Write one line each: why them, what you will show, what you will ask.

Minute 25–40. Artifact. One thing that runs in <90 seconds on your laptop, offline-capable: a 30-second screen recording + live fallback. One QR to a single landing URL that is a GitHub issue template or a 10-minute task, not a homepage.

Minute 40–50. 15-second intro (pick the version for the room). Out loud, twice.

Minute 50–60. CRM rows pre-created: Event, Date, Target, Company, Hook, Artifact, Ask, Channel. Empty rows get filled on the walk out the door.

Do not “research the industry.” Research five humans.

Templates that work (structure, not poetry)

Target card:
Person:
Company / team:
Why they matter (hire / OSS / both):
Public artifact of THEIRS I can mention:
My artifact I will show:
The 10-minute ask:
Follow-up channel I will use tonight:

15-second skeleton:
I [do X in production]. I shipped [named public thing]. I’m here because [their problem]. If useful, I can [show / leave / follow up with] [specific artifact].

Evidence for this style of prep: Manifest Law’s SF networking guide (Dec 22, 2025) — research attendees/speakers, concise intro, 3–5 people, follow up 24–48h. https://manifestlaw.com/blog/san-francisco-tech-networking-events/

Swyx: meetup organizers always need speakers; first meetup talks are the trial run. https://swyx.io/cfp-advice

Confidence: high on the routine; the 30–60 minute cap is OPINION (AK-47). If you cannot prep in an hour, the event is too vague to attend.

---

3. THE PITCH (45+, non-coder, agent-builder)

Do not say “non-coder.” Do not say “I vibe-code.” Do not say “tiny lab” as an apology. Anthropic’s own page rewards independent research, blog posts, and OSS — not credentials. https://www.anthropic.com/careers

DevRel hiring wants proof you can explain and gather developers, not a CS pedigree. https://developerrelations.com/guides/how-to-get-a-job-in-devrel/

Age: do not hide it, do not joke first. Lead with shipped systems. The market bias exists; your counter is artifacts, not youth.

Stating the goal: split by audience.

Recruiter / hiring manager: state the goal. They are paid to route. Vague “exploring” wastes their 90 seconds.

Senior engineer: state the work, not the job hunt. Let’s Data Science: at lab events, pitch as someone working the problems, not as a job-seeker. https://letsdatascience.com/blog/how-to-land-a-job-at-openai-anthropic-or-google-deepmind

Founder: state the collaboration (users, design partners), mention the job only if they ask.

Three 15-second intros (ready to use)

A. Recruiter / hiring manager
“I’m Anton, Palo Alto. I run a production multi-machine Claude agent fleet — memory, second brain, CRM automation — and I ship it in public: OSS repos and PRs into the Anthropic cookbook and Claude Code ecosystem. I’m looking for a DevRel or forward-deployed role at a frontier lab where that exact work is the job. If you own that pipeline, I’ll send one page and a repo tonight.”

B. Senior engineer
“I build agent memory and multi-machine orchestration for Claude in production, not slides. GitHub is tonydzi. If you have a painful agent-context or fleet problem, I can show a 90-second demo and leave you a 10-minute issue to break it.”

C. Founder
“I run a small AI research lab that ships agent-memory and second-brain tooling as OSS, and I need design partners who will actually file issues. I’m also open to DevRel/FDE seats at labs. If your engineers would use this, I’ll onboard one of them this week.”

What to cut: “I’m not a real engineer.” “I’m just looking around.” “We should grab coffee sometime” with no artifact.

Confidence: high that public work is the currency at Anthropic/DevRel; medium that stating the job goal helps with recruiters and hurts with researchers — that split is practitioner consensus, not an A/B test.

---

4. FINDING JOB OPPORTUNITIES ON THE FLOOR

Who actually moves hiring

1. Team ICs and tech leads who will put their name on a referral. Highest value. Mid-level ICs (not the famous director, not the booth intern) are the conversion path described in OpenAI-referral playbooks. https://resumestailor.com/companies/openai/referral
2. DevRel / DX / ecosystem leads. They hire people who already show up in their community. https://developerrelations.com/guides/how-to-get-a-job-in-devrel/
3. Hiring managers who spoke. After their talk, one specific question + one artifact. Do not ask for a job in the Q&A line.
4. Booth recruiters. Useful at AIEWF-class expos (badge scan is real). https://www.ai.engineer/faq They can open a req; they cannot voucher you. Treat as a CRM record, not a win.
5. Internal referrers you already know from GitHub. Events are for converting a GitHub name into a face.

Meetup “invisible interviewers”: speakers, organizers, hiring-team members hanging at the back. https://www.gankinterview.cn/en/blog/the-art-of-networking-how-to-secure-a-direct-interview-pass-by-attending-meetup-

How to get a referral from a 5-minute chat (do not ask in minute 1)

Minute 0–1: their work, not yours.
Minute 1–3: one sentence on your artifact that maps to their problem.
Minute 3–4: show, don’t describe.
Minute 4–5: “If this is useful to your team, would you be open to me sending a 6-line summary you could forward internally? I know referrals cost you political capital — I’ll make it copy-paste.”

The Interview Guys (2026): referred candidates ~4× more likely to get offers in compiled analyses; asking for a referral before you have given the person something to voucher is the fail. Make them owe you a forwarded artifact, then ask. https://blog.theinterviewguys.com/referred-candidates-get-hired-4x-more-often-so-stop-asking-for-a/

Visa / O-1 without killing the chat

University of Colorado Career Services: do not ask about H-1B sponsorship at a career fair or networking event; use the time to show value. https://www.colorado.edu/career/international-students/visa-and-work-authorization/when-ask-sponsorship

Manifest Law (SF, Dec 2025): you may mention status in 1:1 with recruiters/HMs; do not lead with it in a group. https://manifestlaw.com/blog/san-francisco-tech-networking-events/

Anthropic FAQ: “Do you sponsor visas? Yes. We sponsor visas and green cards for eligible roles.” So for Anthropic you do not need to plead. https://www.anthropic.com/careers

Script, only when they ask or when a recruiter is taking your details:
“I’m visa-sensitive. I can start on a path that labs already use — Anthropic publicly sponsors eligible roles — and I’m also building an O-1 evidence file from OSS and talks. Happy to send the one-pager to whoever owns immigration so we don’t waste a loop.”

Do not dump O-1 criteria on an engineer. Speaking invitations help O-1 as supporting narrative, not as one of the eight regulatory criteria. https://legalservicesincorporated.com/immigration-law/extraordinary-ability-niw-visas/can-conference-speaking-invitations-help-your-o-1-visa-petition/

Confidence: high on “don’t lead with visa”; high that Anthropic sponsors; medium on O-1 value of meetup talks (supporting only).

---

5. FINDING ENGINEER-USERS FOR OSS

What converts is not inspiration. It is a first PR in the room, or a 10-minute task that is still open when they get home.

Evidence

PyCon sprints: 2× PR velocity; dedicated first-time contributor rooms; maintainers sitting with newcomers. https://pyfound.blogspot.com/2025/09/sprints-are-best-part-of-conference.html https://pycon.blogspot.com/2025/04/pyconus-sprints.html

CNCF recruiting playbook: always-be-recruiting with specific roles; good-first-issue labels; New Contributor Workshops as a live contributing guide with a first-PR end state; in-person at KubeCon-class events; contributor playground repos; office hours. https://contribute.cncf.io/projects/best-practices/community/recruiting-playbook

GitHub Open Source Guides: 28% of casual contributions are docs (Igor Steinmacher et al.); `/contribute` pages; you do not need code. https://opensource.guide/how-to-contribute/

Playbook for your table / laptop (do this)

1. One problem sentence: “Claude agents forget across machines. This repo is the memory layer.”
2. Live demo, 90 seconds, on your machine. No Wi-Fi dependency.
3. QR to ONE GitHub issue with a checkbox list that takes 10 minutes (docs typo, fixture, eval case, README repro). Not “star the repo.”
4. Ask: “Can you try this before you leave, or in the Uber home? I’ll reply on the issue tonight.”
5. Capture GitHub handle in CRM before they walk away.
6. 24h: comment on their issue or open one in their name if they emailed a bug. Convert chat → issue. Chat that stays in Telegram/LinkedIn dies.

What actually becomes a first PR

- They ran it.
- They had a specific failure or a specific 10-minute win.
- You answered in <24h.
- The CONTRIBUTING path did not require a 40-minute setup.

If your repo needs a fleet to install, you will get zero event PRs. Ship a playground repo (CNCF’s own advice).

Do not recruit “customers.” Recruit testers. Offer a named role: “design partner for agent-memory evals, 30 minutes a week, your name in the README.”

Confidence: high on sprint/workshop conversion vs mixer conversion; medium on 24h being the critical window (strong in sales follow-up data, plausible for OSS).

---

6. FOLLOW-UP SYSTEM (this is the product)

You already said conferences are pointless without follow-up. That is the whole system.

Evidence on timing

Pursue Networking (Oct 2025): follow up 24–48h; they claim response rates drop ~50% after 72h; same-day LinkedIn request; 10–20 meaningful connections per conference, not 100 cards. Vendor-sourced, treat as directional. https://pursuenetworking.com/blog/linkedin-follow-up-after-conference/

Prospeo 2026 playbook: Day 1 / 3 / 10 / 17; 93% of replies by Day 10 in 3-7-7 cadence data they cite; event leads 25–45% open, 3–10% reply; stop after Day 17 and switch to LinkedIn. Sales dataset, not job-search dataset, but the decay curve is the useful part. https://prospeo.io/s/conference-follow-up-email

Manifest Law: 24–48h, specific conversation points. https://manifestlaw.com/blog/san-francisco-tech-networking-events/

Cadence for YOU (hire + OSS, not SaaS pipeline)

Hour 0 (before sleep): LinkedIn or X with the memory hook. Log CRM.
Hour 24: Email or DM with the promised artifact. One ask.
Hour 72: If no reply, send the thing you promised anyway, plus one extra useful link. No “just checking in.”
Day 14: One new fact (you tried their suggestion, you opened the issue, a talk got scheduled). Then stop or move to a quarterly “I shipped X” broadcast.

Four ready messages

1) LinkedIn, same night, engineer
“[Name] — you at [event], we talked about [specific pain, e.g. agent memory compaction]. I’m Anton (tonydzi). Here’s the 90-sec demo I mentioned: [URL]. If you want to break it, this 10-min issue is the door: [issue]. I’ll answer same day.”

2) Email, 24h, hiring manager / DevRel lead
Subject: [their team] + agent-memory fleet — from [event]
“[Name] — we spoke after [talk/booth] about [their sentence]. I run a production Claude fleet and ship OSS into that ecosystem (GitHub tonydzi). One page: [URL]. I’m targeting DevRel / FDE / ecosystem roles where that work is the job. If a referral is a stretch, a 15-min teardown of the repo with someone on your DX team is enough. Two slots: [day] or [day].”

3) X / Telegram DM, OSS
“You asked if this runs locally. It does. Clone → [command] → paste the error here or on [issue]. I’ll patch tonight.”

4) Day 14, no reply
“Closing the loop from [event]. I added the eval case you mentioned: [PR/issue]. If the timing is wrong, I’ll stop pinging. If you want a design-partner slot, reply ‘in’.”

CRM fields that matter (minimum)

event_name, event_date, person_name, company, team, role_guess, github, linkedin_or_x, channel_they_use, conversation_hook (verbatim), artifact_shown, promise_made, ask_type (referral / intro / demo-try / issue / talk-slot), warmth (hot/warm/polite), followup_1, followup_2, followup_3, status (replied / issue_opened / pr / intro_made / interview / dead), dead_reason, visa_mentioned (y/n), o1_evidence_flag (speaker / letter / quote).

Mistakes that kill the lead

Generic “great to meet you.” Pitching a product. Asking for a referral in the first message. Waiting a week. Connecting with everyone. Identical copy-paste. Not delivering the promised link. Four emails after silence. Logging in notes.app instead of CRM.

Confidence: high on 24h and specificity; medium on exact Day 1/3/10/17 for job-search (that cadence is sales).

---

7. SCORECARD — WAS THE EVENT WORTH IT?

Inputs
Hours door-to-door (include commute). Cash (ticket, transit, parking). Prep minutes. Follow-up minutes next day.

Outputs (count in 30 days, not that night)
Qualified contacts: conversation ≥5 min + a hook you can quote + a next artifact.
Target-company ICs (named, lab or DevRel/DX).
Referrals submitted (they confirm they clicked the tool).
Interviews scheduled.
Demo-tries (they ran it or watched the 90s and asked a technical question).
GitHub issues from that event.
PRs merged from that event.
Talk/demo slots obtained for a future event.
O-1 evidence artifacts (photo of you speaking, invite email, recap).

Scoring (OPINION thresholds — set these as your kill rules)

Green (repeat this type): ≥3 qualified contacts AND (≥1 target IC OR ≥1 demo-try that became an issue).
Yellow (one more chance): 1–2 qualified, zero issues, follow-up still open.
Red (stop this type for 60 days): 0 qualified after honest follow-up, or you skipped follow-up. Skipping follow-up is an automatic Red — the event did not fail, you did.

Stop attending this kind of event when, across 3 instances:
- no target-company IC, and
- no GitHub issue, and
- no referral, and
- you cannot name what you would do differently.

Large conference exception: judge the week, not the session. AIEWF is worth it if you leave with 8+ qualified and 2+ lab/DX contacts. If you only sat in talks, it was a YouTube subscription with worse coffee.

Confidence: high that you need a kill rule; the numeric thresholds are OPINION.

---

8. SPEAKING / HOSTING AS LEVERAGE

Evidence it outperforms sitting in the audience

Swyx: meetup organizers are starved for speakers; meetup talks are the on-ramp to conference talks; CFP is a numbers game (2–5 abstracts per conference). https://swyx.io/cfp-advice

GitHub Blog (Rachel, event content programs): session title = email subject line; show why it must be live, not a blog post. https://github.blog/developer-skills/career-growth/9-tips-to-go-from-attendee-to-speaker-at-a-tech-conference/

Lee Robinson via dx.tips: if the goal is socialization, 5–7 minute lightning talks beat long talks. Record if you can; even 300 YouTube views is 2–5× the room and reaches people who will never attend. https://dx.tips/meetups-guide

AIE World’s Fair 2025 CFP (swyx/Latent Space): “no prior speaking exp required, no famous corp required, just good ideas, demos, and real life experience”; speakers get free tickets; rejected nontrivial submissions get discounts. Your war-story talk is in-genre. https://www.latent.space/p/aiewf-2025-cfp

O-1: speaking invitations are supporting evidence, not a listed criterion. Still collect them. https://legalservicesincorporated.com/immigration-law/extraordinary-ability-niw-visas/can-conference-speaking-invitations-help-your-o-1-visa-petition/

Hosting: swyx pulled 25 people on 5 days’ notice with a theme, Luma, nametags. Theme shapes every stranger conversation. SF no-show on free events 40–60%. https://swyx.io/manifest-meetups https://dx.tips/meetups-guide

How to get a Bay Area lightning slot this month

1. Email/DM the host of Latent Space / AI Engineer SF / Bay Area AI Agents / SF Python with a 5-minute title, one-sentence abstract, and a demo URL. Organizers want this. Example rooms: https://luma.com/aieng https://luma.com/hovkz2i1
2. Title that will work: “Claude agents forget across machines: what broke in a real multi-node fleet.” War-story genre. https://swyx.io/cfp-advice
3. Offer to be the beginner slot. Hardest to fill.
4. Host your own 12–20 person “Agent Memory Night” in Palo Alto/Mountain View. Theme beats cute names. Thursday is overcrowded; Tuesday/Wednesday is easier. https://dx.tips/meetups-guide
5. AIEWF: submit CFP when 2027 opens; meanwhile use side events and NEO.

Opinion: one 5-minute talk is worth 4 silent attendances for both hire-visibility and OSS. Hosting a 15-person night is worth 8 silent attendances because you own the list.

Confidence: high on “speak > sit”; medium on exact multiples.

---

9. COUNTER-EVIDENCE — WHEN TO STOP GOING

Be blunt.

Frontier labs do not hire from badges. They hire from prior FAANG/infra employers, internships, fellowships, papers, and public work that someone inside already respects. Anthropic engineering is an infrastructure army fed by Google/Meta/Amazon/Microsoft/Stripe. https://www.techtimes.com/articles/318396/20260615/anthropic-engineering-team-infrastructure-army-new-analysis-1680-engineers.htm

OpenAI application volume is enormous (400k/year cited in one 2026 synthesis). Cold apps are a lottery; referrals are the median path in network-driven cultures. https://letsdatascience.com/blog/how-to-land-a-job-at-openai-anthropic-or-google-deepmind https://resumestailor.com/companies/openai/referral

If your GitHub, blog, and PRs are not getting stronger every month, events are a displacement activity. Anthropic told you what to put at the top of the resume. https://www.anthropic.com/careers

Networking-event skepticism: many “hires through networking” stats mix former coworkers with strangers at mixers. Do not confuse them. https://optimcareers.com/expert-articles/networking-events-are-dead

DevRel market: fewer roles, more candidates, experience preferred. Events help only if they produce public talks and a visible community. https://developerrelations.com/guides/how-to-get-a-job-in-devrel/

When to stop events and do something else

Stop the weekly circuit if, after 30 days of the new system:
- you have not spoken once,
- you have 0 GitHub issues from event contacts,
- you have 0 internal referrals submitted,
- and your public artifact (README, demo, cookbook PR, blog post) did not ship.

Replace event hours with:
1. One serious public writeup of the fleet/memory system (Anthropic literally asked for this).
2. PRs into the projects the labs already watch (Claude Code, MCP, agent memory, cookbooks) — this is how ICs learn your name without a badge.
3. A weekly office hour on Discord/GitHub for the 10 people who already tried the tool.
4. Targeted 1:1s with people you already met, not new rooms.

Events remain useful as a forcing function for talks and O-1 evidence. They are not the hiring engine. Public work + one internal voucher is the hiring engine.

Confidence: high on this section. This is the part most conference content will not say.

---

10. 30-DAY PLAN (3–5 events available per week; attend 2)

Weekly rhythm

Sunday (45 min)
- Score last week on the scorecard.
- Kill one event type if Red.
- Pick next week’s 2 attend + 1 speak/host attempt from Luma/Latent Space/foundercal.
- Pre-write 3 target cards.

Mon–Thu
- One event only on a talk/demo night (Latent Space, AI Engineer SF, Agents SF, company-hosted).
- One event only if it is a sprint/project night OR a dinner with named ICs.
- Skip mixers, pitch nights, “founders and VCs,” anything without a speaker from a target company.
- Same-night follow-up is part of the event. If you cannot follow up tonight, do not go.

Friday
- Host or co-host a 90-minute Agent Memory / Claude Fleet office hour (12 people, Luma, nametags, 3 lightning demos including yours). Or submit a lightning talk for next week.
- Ship one public artifact: issue batch, README, 800-word war story, or a cookbook-style PR.

Saturday
- Deep work on the repo playground so a stranger can get a first issue in 10 minutes. This is the conversion engine.

Metrics to review each Sunday

Must-move (if two are zero, the week failed):
1. Follow-up sent within 24h / qualified contacts (target 100%).
2. Target-company ICs logged (target ≥2/week).
3. Demo-tries (target ≥3/week).
4. GitHub issues from event people (target ≥1/week).
5. Talk or host slots booked (target ≥1/month, ideally 1/week of attempts).

Lagging (30-day):
6. Referrals submitted (target ≥2/month).
7. Interviews scheduled (target ≥1/month — stretch).
8. PRs from others (target ≥1/month).
9. Public post or recorded talk (target ≥2/month).
10. Hours in events vs hours in public work (events should be ≤40% of the week’s “visibility” time).

Hard caps
- Max 2 attended events/week.
- Max 1 large conference/year in this category (AIEWF), worked as a campaign, not a tourist week.
- Zero events in a week where follow-up debt >5 unanswered first-touches.

---

READY-TO-USE BLOCK (copy)

Scorecard table

Event | Date | Hours | $ | Qualified | Target ICs | Referrals | Interviews 30d | Demo-tries | Issues | PRs | Talk slot | Verdict G/Y/R | Kill this type?

Weekly plan (repeat 4 weeks)

Sun: score + pick 2 events + 5 targets
Tue or Wed: talk-meetup (prefer speaking)
Thu: sprint/dinner/company-hosted OR skip
Fri: host 12-person night or CFP/email hosts
Same night as every event: 100% follow-up
Sat: playground repo + one public writeup

---

EVIDENCE vs OPINION (read this so you don’t overfit)

Evidence-based: referral 10× job boards (CareerPlug); Anthropic wants OSS/blog at top of resume and sponsors visas; DevRel hires from public work and networks; AIEWF is the lab-dense conference; Luma guest lists can be public; follow-up decay is real in sales/networking literature; sprints multiply PRs; meetup organizers need speakers; O-1 speaking is supporting not a criterion.

Opinion (mine, as cofounder): 2 events/week cap; the numeric hit-rate table; “one talk = four silent nights”; kill rules; do not say non-coder; state job goal only to recruiters/HMs; your bottleneck is follow-up not access.

Confidence overall: medium-high on strategy, low on any single percentage for “meetups → Anthropic offer,” because that number is not published.

Coverage gaps (inside the report, not a follow-up): no first-party hire stats from Anthropic/OpenAI/xAI events; no Bay Area AI-meetup conversion study; some follow-up timing stats come from sales vendors; GitHub Blog speaker-tips page returned a fetch error this run (URL still listed from search); Optim Careers page fetch failed, claim taken from search snippet plus title.

Sources opened or fetched this run (full URLs)

https://www.careerplug.com/blog/recruiting-metrics-and-kpis/
https://staffinghub.com/hiring/company-pages-referrals-result-in-more-hires-recruiting-metrics-report
https://www.navero.me/blog/recruiting-funnel-metrics
https://www.anthropic.com/careers
https://developerrelations.com/guides/how-to-get-a-job-in-devrel/
https://letsdatascience.com/blog/how-to-land-a-job-at-openai-anthropic-or-google-deepmind
https://pyfound.blogspot.com/2025/09/sprints-are-best-part-of-conference.html
https://pycon.blogspot.com/2025/04/pyconus-sprints.html
https://contribute.cncf.io/projects/best-practices/community/recruiting-playbook
https://opensource.guide/how-to-contribute/
https://pursuenetworking.com/blog/linkedin-follow-up-after-conference/
https://prospeo.io/s/conference-follow-up-email
https://swyx.io/manifest-meetups
https://swyx.io/cfp-advice
https://dx.tips/meetups-guide
https://www.latent.space/p/aiewf-2025-cfp
https://ai.engineer/worldsfair/2026
https://www.ai.engineer/faq
https://luma.com/aieng2025
https://luma.com/aieng
https://luma.com/hovkz2i1
https://help.luma.com/p/event-guest-list
https://manifestlaw.com/blog/san-francisco-tech-networking-events/
https://www.colorado.edu/career/international-students/visa-and-work-authorization/when-ask-sponsorship
https://legalservicesincorporated.com/immigration-law/extraordinary-ability-niw-visas/can-conference-speaking-invitations-help-your-o-1-visa-petition/
https://foundercal.com/cities/sf
https://blog.theinterviewguys.com/referred-candidates-get-hired-4x-more-often-so-stop-asking-for-a/
https://resumestailor.com/companies/openai/referral
https://www.techtimes.com/articles/318396/20260615/anthropic-engineering-team-infrastructure-army-new-analysis-1680-engineers.htm
https://github.blog/developer-skills/career-growth/9-tips-to-go-from-attendee-to-speaker-at-a-tech-conference/
https://studojo.com/reports/referrals-vs-applications-how-people-get-hired-2026
https://dev.to/hanzla/inside-ai-engineer-worlds-fair-2026-what-6000-engineers-showed-up-to-build-fgm

ELI5: Going to five parties a week and collecting names is like buying groceries and never cooking. Cook: two rooms, a 5-minute show-and-tell, one toy people can break tonight, a message before you sleep. Labs hire people their engineers already trust, not people with the most badges.
