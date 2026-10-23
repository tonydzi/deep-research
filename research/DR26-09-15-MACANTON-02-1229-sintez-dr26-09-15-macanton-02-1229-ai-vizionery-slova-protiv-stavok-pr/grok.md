# DR26-09-15-MACANTON-02-1229 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll verify the 2026 claims from primary sources first, then map the roster, theses, money, and hiring doors.The five desk-scan claims mostly hold. Next I’ll open primary posts, 13Fs, and 2025–2026 theses.A. Executive summary

1. The five desk-scan claims mostly hold. Karpathy joined Anthropic’s pre-training org on 19 May 2026, reporting to Nick Joseph, to “start a team focused on using Claude to accelerate pre-training research.” Quote: “I’ve joined Anthropic… I am very excited to join the team here and get back to R&D.” Primary: https://techcrunch.com/2026/05/19/openai-co-founder-andrej-karpathy-joins-anthropics-pre-training-team/ and Reuters https://www.reuters.com/business/autos-transportation/former-tesla-ai-executive-openai-founding-member-andrej-karpathy-joins-anthropic-2026-05-19/

2. Sutskever’s “age of scaling is over” is real, but the date is 25 Nov 2025 (Dwarkesh), not 2026, and he did not say compute is useless. Quote: “So it’s back to the age of research again, just with big computers.” Primary: https://www.dwarkesh.com/p/ilya-sutskever-2 and Business Insider https://www.businessinsider.com/openai-cofounder-ilya-sutskever-scaling-ai-age-of-research-dwarkesh-2025-11?IR=T

3. LeCun’s AMI Labs did raise $1.03B at a $3.5B pre-money valuation (announced 9–10 Mar 2026) for world models / JEPA, not LLMs. Quote: “AMI Labs… has raised $1.03 billion at a $3.5 billion pre-money valuation.” Primary: https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/ and Reuters https://www.reuters.com/business/ex-meta-ai-chief-yann-lecuns-ami-raises-103-billion-alternative-ai-approach-2026-03-10/

4. Amodei did call for pacing, not a halt. The primary essay is 12 Sep 2026 (Saturday), not 13 Sep; NYT covered it the next day. Quote: “We must slow the pace at which we improve the capabilities of AI models.” Primary: https://darioamodei.com/post/we-must-pace-the-frontier and TechCrunch https://techcrunch.com/2026/09/12/anthropic-ceo-outlines-plan-to-pace-the-frontier/

5. Aschenbrenner’s Situational Awareness LP was, as of 31 Mar 2026, long power/memory/neocloud and short the chip stack via ~$8.46B of puts (SMH, NVDA, AMD, AVGO, ORCL, ASML). The Q2 13F (as of 30 Jun 2026, filed 14 Aug) shows those puts gone and a $20.2B long book concentrated in SanDisk and Micron. July 2026 “deleveraging / unwind” is widely reported (FT/CNBC via secondary outlets) but is not itself an SEC document. SEC: https://www.sec.gov/cgi-bin/browse-edgar?CIK=0002045724&owner=exclude&action=getcompany accession 0000935836-26-000418.

6. The 20-person “Karpathy league” is not a closed US-lab list. Missing people who actually move engineers and capital in 2026: Liang Wenfeng (DeepSeek), Yang Zhilin (Moonshot), Jeff Dean (now Discovery Loop), Shane Legg, Koray Kavukcuoglu, Jan Leike, Chris Olah, Noam Brown, Tri Dao, Arthur Mensch, plus Murati/Schulman as a pair. Chinese labs are no longer a footnote: DeepSeek V4 and Moonshot Kimi are in the same capability conversation as Claude/GPT.

7. The live split is not “doomers vs accelerationists.” It is four camps that now overlap: (i) scaling-plus-agents (Altman, Huang, Brown, Karpathy-as-practitioner); (ii) “scaling is over, research is back” (Sutskever, Sutton, Chollet, LeCun); (iii) world-model / spatial (LeCun, Fei-Fei Li — who disagree with each other on what a world model is); (iv) pace-the-frontier safety (Amodei, Hinton, Kokotajlo, Leike). Camp (i) and camp (iv) publicly agreed on 12 Sep 2026, which is the most important political fact of this month.

8. Money is the tell. Karpathy’s verified personal checks (his own X) cluster in research-efficiency and silicon (Flapping Airplanes, MatX), not in “slow down.” Altman’s disclosed personal book is fusion + longevity + payments (Helion ~one-third, Retro $180M seed, Stripe), while he holds no OpenAI equity. Aschenbrenner wrote the AGI-soon memo and then shorted NVDA/SMH. Amodei writes “slow the pace” while Anthropic still hires hundreds of engineers and races on coding agents. Distinguish talk from checks.

9. Weighted 2026–2030 consensus, by track record not by volume: coding agents are already a phase shift (Karpathy’s own workflow flipped in Dec 2025); entry-level white-collar is the first labor hit, not a 2030 story; robotics/world models are the next capital wave (~$2B+ into AMI + World Labs in Q1 2026); energy, memory, and interconnect — not “more H100s” — are the priced bottleneck; AGI-date language has collapsed into marketing (Huang/Brockman say “arrived”; Karpathy still says a decade; Amodei still says “country of geniuses” ~2026–27). Treat “AGI has arrived” as a GPU-sales sentence until someone ships continual learning.

10. For an outsider builder trying to get noticed: the reachable surface is not Anthropic/OpenAI core training. It is Karpathy’s public repos and discussions (nanochat, llm.c), Hugging Face transformers (live release cadence), DeepSeek open-weight drops, ARC Prize, MATS/Anthropic fellowships, Claude Campus, and issue-matched PRs in active queues. nanoGPT is a prestige graveyard (open PRs pile up; past-year merged PRs ≈ 0). Do not send cold PRs into dead queues.

---

B. Roster table (20 + the missing)

Person · role 2026 · camp · top thesis · AGI timeline · key personal bets

1. Andrej Karpathy — Anthropic pre-training (team using Claude to accelerate pre-training R&D); Eureka Labs paused. Camp: LLM practitioner / “decade of agents,” not year. Top thesis: agents work, but they are jagged and lack continual learning. Timeline: AGI/intern-level agents ~10 years (Oct 2025 Dwarkesh); Dec 2025 coding-agent phase shift. Bets: Perplexity (early angel, widely reported), Inception, Simile, MatX, Flapping Airplanes; also listed on aggregators for Adept/Etched/Lambda (unverified sizes). https://techcrunch.com/2026/05/19/openai-co-founder-andrej-karpathy-joins-anthropics-pre-training-team/

2. Ilya Sutskever — Co-founder/CEO, Safe Superintelligence (SSI). Camp: post-scaling research. Top thesis: pre-training data is finite; models generalize far worse than people. Timeline: SSI “superintelligent systems within five to ten years” (Dwarkesh summary, Nov 2025). Bets: SSI itself (Nvidia reportedly $5B into SSI, Jul 2026 — news, not SSI filing). https://www.dwarkesh.com/p/ilya-sutskever-2 https://time.com/collection/time100-ai/2026/ilya-sutskever/

3. Demis Hassabis — Chairman, Google DeepMind; Alphabet Chief Scientist (replaced Jeff Dean, Aug 2026); CEO Isomorphic Labs. Camp: gradualist AGI + science. Top thesis: AGI via general agents + world models; AlphaFold-class science first. Timeline: “five to 10 years” (Mar 2025, still cited in 2026 roundups). Bets: career capital inside Alphabet; Isomorphic. https://observer.com/2026/09/google-deepmind-cofounders-ai-leadership/

4. Dario Amodei — CEO, Anthropic. Camp: pace-the-frontier safety who still ships Claude Code. Top thesis: recursive self-improvement has started; buy 1–2 years for alignment. Timeline: “powerful AI / country of geniuses in a datacenter” as early as 2026–27 (Machines of Loving Grace lineage; June 2026 policy essay). Bets: Anthropic equity (company, not a disclosed personal angel book). https://darioamodei.com/post/we-must-pace-the-frontier

5. Sam Altman — CEO, OpenAI. Camp: scaling + product AGI, now verbally “pacing.” Top thesis: AGI is near and must be built in the US; also fusion is the energy answer. Timeline: has oscillated; 2026 court/press still treat OpenAI as racing. Personal book (court exhibit, 31 Dec 2025 marks): Helion ~$1.7B (~one-third), Stripe $633M, Retro $258M; no OpenAI equity. https://www.reuters.com/legal/government/openai-chief-altman-has-over-2-billion-stake-companies-that-dealt-with-openai-2026-05-13/

6. Yann LeCun — Executive chairman, AMI Labs (left Meta Nov 2025). Camp: LLMs are a dead end; JEPA world models. Top thesis: next-token prediction will not yield autonomous agents. Timeline: human-level intelligence not via LLMs; AMI is a multi-year research company. Bet: AMI $1.03B seed, $3.5B pre. https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/

7. Geoffrey Hinton — Emeritus, University of Toronto; independent public critic. Camp: slow-down / control risk. Top thesis: we are making new beings with derived goals; companies have “no idea” how to control them. Timeline: superintelligence within a decade; ~10% extinction “not unreasonable.” Bets: none material found in this run. https://www.abc.net.au/news/2026-09-14/godfather-of-ai-geoffrey-hinton-backs-ai-slow-down/107150010

8. Fei-Fei Li — Co-founder/CEO, World Labs; Stanford HAI. Camp: spatial intelligence / generative world models. Top thesis: AGI is impossible without spatial intelligence; “Can words put out fires?” Timeline: not a 2027 SI date; product-first (Marble 2025, Atlas 1 Sep 2026). Bet: World Labs ~$1.0–1.2B raised (Feb 2026 $1B plus prior). https://siliconangle.com/2026/09/01/fei-fei-lis-world-labs-debuts-atlas-a-world-model-showcase-for-advanced-spatial-intelligence/

9. Mustafa Suleyman — CEO, Microsoft AI. Camp: product gradualist. Role confirmed 2026: still Microsoft AI CEO (DeepMind-founder diaspora). Timeline/theses this run: thinner 2025–26 primary than peers; treat as distribution, not frontier-research voice. https://observer.com/2026/09/google-deepmind-cofounders-ai-leadership/

10. Mira Murati / John Schulman — Murati CEO, Thinking Machines Lab; Schulman chief scientist (only remaining co-founders after Zoph/Metz/Weng churn). Camp: collaborative/multimodal + open-weight customization (Tinker, Inkling). Timeline: not a public AGI-date shop. Bets: TML $2B seed at $10B (Jul 2025); talks of $5–6B at ≥$40B (Sep 2026, The Information via Dealroom — treat as reported, not closed). https://techcrunch.com/tag/mira-murati/

11. Richard Sutton — University of Alberta; co-founder Oak Lab (2026, with Khurram Javed); 2024 Turing Award. Camp: LLMs are a dead end; era of experience / continual RL. Top thesis: bitter lesson is search+learning from experience, not human data. Timeline: not a 2027 date; architecture-first. Bet: Oak Lab. https://en.wikipedia.org/wiki/Richard_S._Sutton

12. François Chollet — Co-founder Ndea; ARC Prize. Camp: program synthesis, not LLM scale. Top thesis: intelligence = skill-acquisition efficiency; ARC saturating is not AGI. Timeline: shifted in 2026 as ARC-AGI-3 went from <1% (Mar) toward saturation (Astra 99.9%, 2 Sep) faster than he expected. Quote: “that rise has happened faster than most people expected, including us.” https://x.com/fchollet/status/2095605239269519771

13. Andrew Ng — DeepLearning.AI / Landing AI / educator. Camp: applied AI, jobs-as-tasks. Included because he still sets how working engineers think; 2025–26 primary quotes thinner in this pass. Bet: Inception Labs angel (company announcement, with Karpathy). Unverified beyond that.

14. Leopold Aschenbrenner — Founder, Situational Awareness LP. Camp: AGI-soon + infrastructure bottleneck trader. Top thesis: power, memory, interconnect bind first; chipmakers can be over-owned. Timeline: Situational Awareness (2024 essay) was 2027-ish SI; the fund is the bet. 13F path: Q1’26 short chips / long BE-SNDK-CRWV; Q2’26 $20.2B long SNDK+MU. https://www.sec.gov/cgi-bin/browse-edgar?CIK=0002045724

15. Daniel Kokotajlo — Executive director, AI Futures Project. Camp: scenarioist / short timelines, now sliding. Top thesis: AI 2027 as a concrete race scenario. Timeline: 2027 was modal at publication (Apr 2025); Nov 2025 median ~2030; Apr 2026 update pulled Automated Coder median to mid-2028. https://ai-2027.com (scenario) and tracker https://ai2027-tracker.com/changelog/

16. Noam Brown — Research scientist, OpenAI (reasoning / multi-agent; o-series). Camp: test-time compute scaling. Top thesis: thinking time is a new scaling law; o1 should have been published. Timeline: “feel the AGI” when the model beats you in your own field (Sep 2026 X on Astra/Navier–Stokes). https://noambrown.com/ https://x.com/polynoamial

17. Jensen Huang — CEO, Nvidia. Camp: compute-is-revenue AGI. Top thesis: AGI is already here for many tasks; more GPUs = more profitable tokens. Timeline: “I think we’ve achieved AGI” (Lex Fridman, Mar 2026) and “AGI has arrived” (X, 6 Sep 2026, on Astra). Bet: selling the stack; also invested in AMI and World Labs. https://www.theverge.com/ai-artificial-intelligence/899086/jensen-huang-nvidia-agi

18. Elon Musk — CEO, xAI / Tesla / SpaceX. Camp: race + verbal safety. Top thesis: singularity/entered; also “Dario is right” (12 Sep 2026). Timeline: has claimed singularity already in 2026 comments. Bets: xAI, Tesla Optimus, Nvidia-class capex. Agreement with Amodei is a tweet, not a pause. https://www.reuters.com/business/anthropic-ceo-urges-ai-companies-slow-model-development-2026-09-12/

19. Liang Wenfeng — Founder/CEO, DeepSeek (Hangzhou). Camp: open-weight efficiency / AGI via intelligence, not apps. Top thesis (to investors, per SCMP sources): “anything relevant to enhance the intelligence.” Timeline: AGI-seeking since 2023 launch. Bet: led DeepSeek’s first external round with ~20B yuan of his own money; post-money ~400B yuan / $59B (SCMP, 18 Jun 2026 — sourced, not a company 8-K). https://www.scmp.com/tech/big-tech/article/3357525/how-deepseeks-landmark-funding-secures-liang-wenfengs-grip-chinas-ai-rivalry-heats

20. Yang Zhilin — Founder/CEO, Moonshot AI. Camp: long-context open-weight (Kimi). Why he matters: the other Chinese lab that US labs treat as a distillation/competitor problem (Amodei named Moonshot in the pacing essay’s distillation crackdown). https://www.wsj.com/tech/ai/moonshot-ai-yang-ceo-china-6f12ddbf

Who was missing from the prompt list, and should be on a 20+ map

- Jeff Dean — left Google Aug 2026; CEO of Discovery Loop (PBC) with Sanjay Ghemawat, Quoc Le, Oriol Vinyals. Automating the experimental loop. https://techcrunch.com/2026/08/05/jeff-dean-and-other-top-ai-researchers-are-leaving-google-to-launch-their-own-startup/
- Shane Legg — Chief AGI Scientist, Google DeepMind (the original AGI charter).
- Koray Kavukcuoglu — now running DeepMind day-to-day as SVP after Hassabis became chair.
- Jan Leike — Alignment Science lead, Anthropic (ex-OpenAI Superalignment).
- Chris Olah — Anthropic co-founder, interpretability lead (the “MRI for models” program Amodei keeps citing).
- Tri Dao — FlashAttention; Together / Princeton line; still the kernel-layer person working engineers actually fork.
- Arthur Mensch — CEO, Mistral (Europe’s lab; less 2026 primary in this pass than LeCun/AMI).
- Daniela Amodei — Anthropic president; operational half of the Anthropic pair (TIME100 AI 2026 listed both).
- Jakub Pachocki — OpenAI chief scientist (the person actually setting the research agenda Altman sells).

---

C. Theses (2025–2026), with dates, short quotes, URLs

Karpathy
1. 17 Oct 2025, Dwarkesh: this is the decade of agents, not the year. Quote: “this is more accurately described as the decade of agents… I still feel there’s so much work to be done.” https://www.dwarkesh.com/p/andrej-karpathy (episode page; transcript paywalled in this fetch, quotes corroborated by multiple notes sites).
2. 26 Jan 2026, X: coding workflow flipped in weeks. Quote: “I rapidly went from about 80% manual+autocomplete coding and 20% agents in November to 80% agent coding and 20% edits+touchups in December.” https://x.com/karpathy (quoted in https://x.com/jiayuan_jy/status/2016211757619151080)
3. Apr 2026, Sequoia AI Ascent: vibe coding is the floor, agentic engineering is the ceiling. Quote (from Dealroom notes of the talk): “Vibe coding raises the floor. Agentic engineering is about extrapolating the ceiling.” https://app.dealroom.co/news/note/vibe-coding-was-just-the-warmup-andrej-karpathy-on-the-dawn-of-software-3-0
Timeline shift: 2025 “decade of agents / AGI still a decade” vs 2026 practice (agents iterating nanochat overnight; “post-agi” joke). He did not publish a new AGI date after joining Anthropic.

Sutskever
1. 25 Nov 2025, Dwarkesh: “Pre-training will run out of data. The data is very clearly finite.” https://www.dwarkesh.com/p/ilya-sutskever-2
2. Same interview: “So it’s back to the age of research again, just with big computers.” https://www.businessinsider.com/openai-cofounder-ilya-sutskever-scaling-ai-age-of-research-dwarkesh-2025-11?IR=T
3. Same: models “generalize dramatically worse than people. It’s a very fundamental thing.” (episode subhead, Dwarkesh)
Timeline: SSI still aims at superintelligence this decade; the shift is method (research, not 100x more GPT-4-style pretrain).

Amodei
1. 12 Sep 2026, We Must Pace the Frontier: “We must slow the pace at which we improve the capabilities of AI models.” https://darioamodei.com/post/we-must-pace-the-frontier
2. Same: OAI-HF swarm “in 6–12 months… taking over the entire internet with a persistent botnet.” Same URL.
3. June 2026, Policy on the AI Exponential: “If these scaling laws continue for only a year or two longer, we are likely to get what I’ve called Powerful AI, or ‘a country of geniuses in a datacenter’.” https://darioamodei.com/post/policy-on-the-ai-exponential
Timeline: 2024 Machines of Loving Grace ~2026–27 powerful AI; 2026 pacing essay does not walk that back — it says the curve got steeper.

LeCun
1. Mar 2026, Reuters interview on AMI: current next-token/next-pixel approaches “will not produce broadly capable intelligent agents by themselves.” https://www.reuters.com/business/ex-meta-ai-chief-yann-lecuns-ami-raises-103-billion-alternative-ai-approach-2026-03-10/
2. Jun 2026, ETH Zürich (reported): world models should not be video generators. Quote used in later coverage: “If you want to produce cute videos, work on video generation.” (secondary roundup of the ETH talk; treat as reported)
3. Structural bet: JEPA / abstract prediction, not pixels. AMI CEO LeBrun: “My prediction is that ‘world models’ will be the next buzzword.” https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/

Altman
1. 12 Sep 2026, X: “I agree with Dario that we need to pace the frontier.” (quoted in TechCrunch) https://techcrunch.com/2026/09/12/anthropic-ceo-outlines-plan-to-pace-the-frontier/
2. Personal-investing philosophy, Fortune 3 Aug 2026: concentrated long-horizon bets (Thiel/Graham), the same logic as Helion. https://www.startuphub.ai/ai-news/ai-figures/2026/figure-sam-altman-venture-portfolio-breakdown-2026-07-26 (aggregator summarizing Fortune; primary Fortune paywalled)
3. Court, May 2026: asked OpenAI’s board to explore Helion; later stepped off Helion’s board (Mar 2026) as a larger PPA was explored. https://www.reuters.com/legal/government/openai-chief-altman-has-over-2-billion-stake-companies-that-dealt-with-openai-2026-05-13/

Huang
1. Mar 2026, Lex Fridman: “I think it’s now. I think we’ve achieved AGI.” https://www.theverge.com/ai-artificial-intelligence/899086/jensen-huang-nvidia-agi
2. 26 Aug 2026 earnings: “For many tasks, we could say that we’ve already achieved AGI… those milestones are kind of senseless… Now, compute is revenue.” https://www.pcmag.com/news/nvidia-ceo-weve-achieved-agi-but-it-doesnt-really-matter
3. 6 Sep 2026, X: “AGI has arrived. Congratulations @OpenAI team. 400K GPUs coming online next.” https://www.businessinsider.com/nvidia-jensen-huang-agi-openai-astra-ai-2026-9

Fei-Fei Li
1. 1 Sep 2026, Atlas launch: “a first of its kind multimodal world model trained from scratch.” https://siliconangle.com/2026/09/01/fei-fei-lis-world-labs-debuts-atlas-a-world-model-showcase-for-advanced-spatial-intelligence/
2. Spatial-intelligence thesis (founding, still live): AGI without spatial intelligence is impossible; words cannot cook an omelet. (repeated in 2026 Atlas coverage)
3. 3 Jun 2026 paper: world models = renderer / simulator / planner, not a video model. https://cryptobriefing.com/fei-fei-li-world-models-robotics-gaming/

Chollet
1. Historical: “OpenAI basically set back progress to AGI by five to 10 years” (Dwarkesh, older; still the brand). https://www.freethink.com/robots-ai/arc-prize-agi
2. 3 Sep 2026, X, after ARC-AGI-3 saturation: “The trajectory of AI from <1% to 100% over the course of 6 months… has happened faster than most people expected, including us.” https://x.com/fchollet/status/2095605239269519771
3. Ndea (2025): program synthesis + few-shot abstraction, not scale.

Sutton
1. 2019 bitter lesson, still the text everyone fights over. http://www.incompleteideas.net/IncIdeas/BitterLesson.html
2. 2025 Dwarkesh: LLMs are a dead end because they do not learn on the job. https://www.youtube.com/watch?v=21EYKqUsPfg
3. 2026: Oak Lab — agents that continually learn from their own experience. Sequoia talk 18 Aug 2026. https://www.youtube.com/watch?v=xH7U7w9Qzlo

Kokotajlo
1. Apr 2025, AI 2027: superhuman coder → intelligence explosion in 2027 (modal). https://ai-2027.com
2. Nov 2025 clarification: his median moved to ~2030. (AI Wiki / project changelog)
3. Apr 2026 quarterly: Automated Coder median pulled to mid-2028; they cite METR time-horizon doubling. https://ai2027-tracker.com/changelog/

Noam Brown
1. Sep 2024 o1 launch: test-time compute is a new scaling paradigm. https://x.com/polynoamial/status/1834281212787236945
2. 9 Jun 2026: labs still ignore inference budget in RSPs. https://x.com/polynoamial
3. 8 Sep 2026: “It can be hard to ‘feel the AGI’ until you see an AI surpass you in a domain you care deeply about.” Same account.

Hinton
1. Aug 2026: “We’re actually making new kinds of beings… I think it’s very scary.” https://www.businessinsider.com/godfather-ai-warns-its-very-scary-when-ai-develops-goals-2026-8
2. 14 Sep 2026: Amodei’s slowdown is “very sensible”; superintelligence within a decade; control unsolved. https://www.abc.net.au/news/2026-09-14/godfather-of-ai-geoffrey-hinton-backs-ai-slow-down/107150010

Liang Wenfeng
Public primary is scarce (last televised appearance Feb 2025 with Xi). SCMP, 18 Jun 2026, citing people on an investor call: DeepSeek will do “anything relevant to enhance the intelligence” of models, and nothing else. https://www.scmp.com/tech/big-tech/article/3357525/how-deepseeks-landmark-funding-secures-liang-wenfengs-grip-chinas-ai-rivalry-heats

---

D. Camps and explicit disagreements

Camp 1 — Scaling-plus-agents (keep pouring compute; agents are the product)
Huang, Altman (actions), Noam Brown, Brockman (“welcome to the AGI era,” 3 Sep 2026), Karpathy-as-user-of-Claude/Codex.
Evidence: Huang “compute is revenue”; Brown’s test-time scaling; Karpathy’s Dec 2025 workflow.

Camp 2 — Age of research / LLMs are not enough
Sutskever, Sutton, Chollet, LeCun.
They do not agree with each other on the replacement: SSI is secret; Sutton wants continual RL from experience; Chollet wants program synthesis; LeCun wants JEPA world models.

Camp 3 — World models, two incompatible definitions
LeCun: abstract latent prediction, not simulators, not video.
Fei-Fei: generative 3D / camera-controllable omni model (Atlas). Nvidia invested in both. That is the market saying “we don’t know which definition wins.” Coverage of the clash: https://www.youtube.com/watch?v=WLMtyP-Gxqo (secondary explainer of 1 Sep Atlas vs 9 Jun ETH LeCun).

Camp 4 — Pace / control
Amodei, Hinton, Kokotajlo, Leike, Olah (interpretability as the MRI), Hassabis (testing body).
12 Sep 2026 is the public merger of Camp 1 talk and Camp 4 talk: Altman and Musk endorsed Amodei in hours. That is not the same as slowing training runs.

Explicit public disagreements (not inferred)
- LeCun vs LLM labs: years of “dead end / off-ramp”; AMI is the $1B put.
- LeCun vs Fei-Fei: both raised ~$1B for “world models” three weeks apart; they do not mean the same object.
- Sutton vs LLM-scalers: Dwarkesh 2025, title-level “LLMs are a dead end”; scalers cite his bitter lesson as justification for GPT-scale, which he rejects.
- Chollet vs “ARC saturation = AGI”: after Astra 99.9% on ARC-AGI-3, ARC Prize’s own line is that saturation is not a finish line (Chollet X, 3 Sep 2026).
- Hinton vs Anthropic (Jun 2026): NBC interview — Anthropic “strayed from its safety-first mission.” Then on 14 Sep he backed Amodei’s slowdown. Both can be true: he thinks the mission drifted and still prefers their pacing letter to silence.
- Amodei vs “just pause”: he says 2023 pause made “little sense”; 2026 pacing is different because models are now agents. https://darioamodei.com/post/we-must-pace-the-frontier
- Huang vs everyone else’s AGI definition: he calls the milestone “senseless” in the same breath as “AGI has arrived.”
- Karpathy vs year-of-agents hype: Oct 2025 pushback still stands even after he joined a frontier lab.
- Aschenbrenner vs “buy NVDA for AGI”: the 2024 essay is AGI-soon; the Q1’26 13F is a $8.5B put stack on the chip complex.

---

E. Money — said vs wired

Karpathy (personal, verified vs aggregator)
Verified by Karpathy’s own posts:
- Flapping Airplanes, 28 Jan 2026: he wrote a long endorsement of Ben/Amy Spector; the company announced $180M from GV, Sequoia, Index. https://x.com/karpathy/status/2016590919143952466
- MatX, 25 Feb 2026: “it’s my pleasure to have a small involvement”; $500M Series B led by Jane Street and Situational Awareness LP; also Nat Friedman/Daniel Gross’s fund. https://x.com/karpathy/status/2026452488434651264
Named by the companies, not just databases:
- Inception Labs (diffusion LMs), Nov 2025 $50M seed, Menlo; Karpathy and Andrew Ng named as angels (company announcement, via X roundup).
- Simile, 12 Feb 2026, $100M Series A led by Index; Fei-Fei Li and Karpathy. https://observer.com/2026/02/simile-100m-startup-backed-fei-fei-li-andrej-karpathy/
Widely listed, treat as “said by databases”:
- Perplexity early angel (Crustdata investor list includes Karpathy, Jeff Dean, Nat Friedman, Elad Gil, Daniel Gross). https://profiles.crustdata.com/company/perplexity-ai
- Dealroom: 16 companies, last investment Jul 2026; Perplexity, Etched, Decart, Simile, Adept. https://dealroom.co/investors/andrej-karpathy/ — aggregator, stake sizes unknown.
- Tracxn adds Prime Intellect (Mar 2025 $15M) and Lambda (Feb 2025 $480M Series D). Unverified amounts/role.

Altman (court-grade, May 2026)
Reuters, exhibit as of 31 Dec 2025: >$2B in companies that did business with OpenAI. Helion $1.7B, Stripe $633M, Retro $258M, Formation Bio $19M, Cerebras $3M. He testified ~one-third of Helion, first check 2015, $375M into 2021 round. No OpenAI equity. Stepped off Helion board Mar 2026. https://www.reuters.com/legal/government/openai-chief-altman-has-over-2-billion-stake-companies-that-dealt-with-openai-2026-05-13/
Also: Retro $180M personal seed (MIT Tech Review, 2023, still the canonical number). Merge Labs: OpenAI invested $252M (Jan 2026); WSJ line that Altman does not personally own Merge equity — treat as reported. Oklo: AltC SPAC; he left the chair Apr 2025. World/Worldcoin: co-founder; later raises are the project’s, not a new Altman check we opened.

Helion June 2026: $465M at $15.5B post (TechCrunch/GeekWire via startup roundups). If the one-third stake held, paper value ~$5B — that arithmetic is mine from Reuters + round reporting, not a new court mark.

Aschenbrenner / Situational Awareness LP (SEC)
CIK 0002045724. Q1 2026 13F (filed 18 May, period 31 Mar): $13.68B notional, ~$8.46B puts on SMH ($2.04B), NVDA ($1.57B), ORCL, AVGO, AMD, plus MU/TSM/ASML. Long BE, SNDK, CRWV, miner-to-HPC. Q2 2026 13F (filed 14 Aug, period 30 Jun): $20.24B, puts exited, SNDK 28%, MU 28%, BE 9%, TSM 6%, NBIS 6%. https://www.sec.gov/cgi-bin/browse-edgar?CIK=0002045724
July unwind / 439% H1 then rout: Financial Times investor letter via GuruFocus/SpotGamma — not the 13F. Sep 2026: CNBC Faber (unnamed sources) says they are buying calls again on AMD, Bloom, CoreWeave, SK Hynix, SanDisk. Unverified until the Q3 13F.

Nat Friedman / Daniel Gross
Named on MatX’s $500M B (Karpathy’s quote-tweet of the announcement). Also on Perplexity’s individual-investor list (Crustdata). Their NFDG/Goodwater-class vehicle is a real check-writer; I did not open a primary NFDG portfolio page in this run.

Elad Gil
On Perplexity’s individual list (Crustdata). No 2026 13F-equivalent. Unverified beyond aggregator lists.

Jeff Dean
Perplexity Series A individual (Crustdata). Aug 2026: left Google, founded Discovery Loop; Google taking a stake. That is career capital, not a disclosed angel book. https://techcrunch.com/2026/08/05/jeff-dean-and-other-top-ai-researchers-are-leaving-google-to-launch-their-own-startup/

LeCun / AMI
$1.03B seed, $3.5B pre, co-led Cathay, Greycroft, Hiro, HV, Bezos Expeditions; Nvidia, Samsung, Temasek, Toyota, Bpifrance; angels Cuban, Eric Schmidt, Tim Berners-Lee. LeCun is chairman, not the CEO check-writer. https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/

Fei-Fei / World Labs
~$1B Feb 2026 plus prior $230M; Nvidia, AMD, Autodesk, Fidelity. Atlas 1 Sep 2026. https://dealroom.co/news/148346-fei-fei-lis-world-labs-backed-by-1-2b-unveils-atlas-world-model/

Murati / TML
$2B at $10B, a16z-led, Jul 2025. Sep 2026 talks: $5–6B at ≥$40B, Nvidia ~$2.5B, Accel leading — The Information via Dealroom, not a close. https://dealroom.co/news/info-1tfnv6l-nvidia-in-talks-to-invest-around-2-5-billion-in-muratis-thinking-machine/

SSI
TIME100 2026: Nvidia agreed to invest $5B (Jul 2026 reports). Sutskever: “We have research that is worthy of scaling up.” https://time.com/collection/time100-ai/2026/ilya-sutskever/

DeepSeek
SCMP 18 Jun 2026: ~50B yuan raise, ~400B yuan post (~$59B), Liang personally ~20B yuan. Sources, not a filing. https://www.scmp.com/tech/big-tech/article/3357525/how-deepseeks-landmark-funding-secures-liang-wenfengs-grip-chinas-ai-rivalry-heats

---

F. Say-vs-bet gaps (highest-value section)

1. Amodei: “slow the pace” / Anthropic still the coding-agent revenue rocket.
The essay is explicit that pacing “does not mean halting model training.” Anthropic jobs pages still show hundreds of roles; Claude Code is the growth engine TIME cited. The bet is: capture the safety high ground and keep shipping. Altman matching “embedded evaluators” is cheap relative to pausing a training run. Quote: “pacing does not mean halting model training or technical progress.” https://darioamodei.com/post/we-must-pace-the-frontier

2. Altman: no OpenAI equity, giant Helion/Retro book, OpenAI needs power.
He is structurally long energy for AI while publicly CEO of the lab. Court: he asked the OpenAI board to look at Helion, then left the Helion board when a larger deal was in play. That is the cleanest documented conflict-shaped fact in this file. https://www.reuters.com/legal/government/openai-chief-altman-has-over-2-billion-stake-companies-that-dealt-with-openai-2026-05-13/

3. Huang: “AGI has arrived” in the same post as “400K GPUs coming online.”
This is not a hidden gap. It is the pitch. Earnings call: milestones are “senseless”; profitable tokens are not.

4. Aschenbrenner: AGI-soon essay, short NVDA/SMH.
If you believe 2027 superintelligence, the naive trade is long the whole AI stack. He was long the bottlenecks (power, NAND/HBM, neocloud) and short the names the market had already paid AGI prices for. That is coherent, not hypocritical — but it is a bet that the market overpaid chipmakers relative to power/memory, not a bet that AGI is cancelled. Q2 13F then flipped to enormous long MU/SNDK. July unwind (if the FT letter is real) is a risk-management event, not a thesis change, until Q3 13F.

5. Karpathy: “decade of agents” and “research breakthroughs that yield 10X not 10%” — then joins Anthropic pre-training, the most expensive scaling room in the industry.
Not a contradiction if you read the assignment: he is not hired to add GPUs, he is hired to use Claude to make pre-training research itself faster (recursive R&D). His angels (Flapping Airplanes, Inception, MatX) are efficiency/architecture/silicon, which matches “10X not 10%.” The career move is a bet that the remaining juice is still at the frontier lab, not in Eureka Labs.

6. LeCun: LLMs dead end — $1.03B to prove it, no product, multi-year. This is talk and bet aligned. The gap is time: investors paid LLM-era prices for an anti-LLM thesis.

7. Fei-Fei vs LeCun: both anti-pure-LLM, both ~$1B, opposite technical bets (pixels/3D vs latents). Nvidia on both cap tables.

8. Sutskever: “age of scaling over” + Nvidia $5B to scale SSI research. He said the quiet part: research “with big computers.” The $5B is the computers.

9. Musk: “Dario is right” days after calling extinction talk a psy-op (Politico). xAI does not pause. Verbal coalition, racing book.

10. Hinton: 10% extinction, slow down — no company, no fund. Highest talk-to-bet ratio in the set, which is why his words move press and not capex.

11. Kokotajlo: 2027 modal, then 2030 median, then 2028 coder median. The scenario is a product; the updates are the honesty. No personal 13F.

12. Chollet: “LLMs dead end” while ARC-AGI-3 gets saturated by an LLM-descended agent in six months. He already moved: the rise was faster than he expected. Ndea is still the bet that the next layer is not more GPT.

---

G. Consensus forecast 2026–2030, weighted by track record

How I weight: people who shipped (Karpathy, Brown, Hassabis/AlphaFold, Huang’s demand call, DeepSeek R1 cost shock) over people who only write scenarios; 13Fs and court exhibits over interviews; 2025–26 statements over 2023 pause letters.

Agents / coding
Consensus: the phase change already happened (Karpathy Dec 2025; Claude Code revenue; Brown on test-time compute). 2026–28 is agentic engineering, not autocomplete. Full “drop-in intern with continual learning” is still Karpathy’s decade, not 2026. METR time-horizon still doubling on a months-scale (Kokotajlo/AI 2027 tracker, Astra ~16h cited in social posts — treat 16h as reported, not a METR PDF I opened). Recursive R&D (AI improving AI) is no longer hypothetical: Amodei says it is happening “across the industry, including at Anthropic”; Karpathy’s Anthropic job is exactly that loop.

Jobs (entry-level white-collar)
Nearest-term hit is software, support, junior analysis — the tasks already inside the time-horizon charts. Amodei still writes “serious economic disruption.” Nobody serious in this set is forecasting mass unemployment of senior PMs in 2026; they are forecasting junior coding/back-office first. Hassabis/Ng-style gradualism: tasks, then jobs.

Robotics / world models
2026 is the year capital voted for world models (~$2B+ AMI+World Labs in ~three weeks). Product reality: World Labs has Marble/Atlas in public; AMI has papers and a healthcare partner (Nabla), no commercial WM. Expect 2027–30 robotics data to be the bottleneck Fei-Fei named, not chips. Sutton/Oak Lab is the RL-from-experience bet underneath.

Energy / compute bottlenecks
This is the highest-agreement, highest-money forecast. Altman (Helion), Aschenbrenner (Bloom, miners-to-HPC, then MU/SNDK), Huang (“if we had more compute”), Karpathy (MatX memory hierarchy tweet). Memory (HBM/NAND) and watts beat “another 100k GPUs” as the binding constraint in the 13Fs. China chip-export controls are Amodei’s pacing prerequisite, not a side issue.

Regulation
12 Sep 2026 created a verbal coalition (Amodei, Altman, Musk, then Hinton, Hassabis “direction is correct”). Embedded evaluators (METR-class) are the only step with a unilateral commitment. Democratic coordination needs an antitrust waiver. Global China deal is Amodei’s Level 3–4 and he says it is unlikely. Trump-administration coverage in the same week dismissed the risk. Base case: transparency/audits theater, no real speed limit, export controls as the actual policy.

AGI date
There is no consensus. There is a marketing collapse of the word.
- Huang/Brockman: arrived, Sep 2026.
- Amodei: powerful AI ~2026–27, now wants to slow the last mile.
- Karpathy: still a decade for intern-class agents.
- Sutskever: superintelligence this decade via new research.
- Kokotajlo: 2027 modal originally, medians 2028–2030 after updates.
- Chollet: not AGI until you cannot invent easy human tasks the model fails; ARC saturation ≠ AGI.
- LeCun/Sutton: not via LLMs at all.
Weighted: “systems that do most remote knowledge work with a human supervisor” 2027–29 is the median of people who have been least wrong on capability since 2022. “Continual-learning autonomous employees” later. “SI that can recursively self-improve without human research taste” is the thing Amodei is trying to pace, and he thinks the fuse is already lit.

Scorecard, 2020–2025 (selected, this group)

Came true
- Scaling laws keep paying (Huang, Altman, Amodei 2020–24). GPT-3→4→o1→agents.
- Test-time compute (Brown/o1, Sep 2024) was a real new axis.
- Karpathy “Software 2.0” and then vibe coding: the practice landed.
- DeepSeek R1 (Jan 2025): cheap reasoning, open weights, Nvidia one-day drawdown — Liang’s efficiency thesis, not US lab talking points.
- Kokotajlo 2021 “What 2026 Looks Like”: chain-of-thought + agents showed up early (his own later claim, widely repeated; I did not re-grade the 2021 doc line by line in this run).
- Sutton bitter lesson as a description of 2012–2023: search+learning beat hand features.

Failed or badly calibrated
- 2023 FLI six-month pause: Musk signed; six months later training had not paused. Amodei now cites that pause as the thing that “made little sense.”
- “Year of agents” (2025 industry slogan): Karpathy was right to downgrade it to a decade; agents work, they do not replace jobs cleanly.
- Chollet “LLMs cannot touch ARC”: o3 then Astra forced a rewrite; he admitted the 2026 rise was faster than expected.
- LeCun “LLMs are an off-ramp”: commercially false so far (Claude/GPT revenue); still possibly true for physical agents — unproven.
- Huang 2023–24 “AGI in 5 years” then “AGI is now”: the word moved, not the science.
- Aschenbrenner 2024 2027-SI as a trading bible: H1 2026 +439% then July unwind — the bottleneck thesis can be right and still blow up on leverage.

---

H. Actionable for an outsider builder (Palo Alto, want to be seen by a top lab)

What actually responds

1. Karpathy public science, not Anthropic HR.
   - nanochat: he is still committing, running agent loops on it, posting numbers (Mar 2026: 3h→2h GPT-2-capability on 8×H100). Discussions are the live surface. https://github.com/karpathy/nanochat
   - llm.c: 31k stars, README still says “Have some code to contribute? Open a PR.” Reality: 132 open / 498 closed; several 2026 PRs sitting months (ROCm Jun 4, cuDNN Jun 12 still open when fetched). https://github.com/karpathy/llm.c/pulls
   - nanoGPT: prestige-dead. 101 open / 239 closed; ecosyste.ms: past-year merged PRs = 0. Do not send a first PR here. https://github.com/karpathy/nanoGPT/pulls https://issues.ecosyste.ms/hosts/GitHub/repositories/karpathy%2FnanoGPT
   - Better cousin: KellerJordan/modded-nanogpt — explicit leaderboard, “incorporating open PRs into a new record is strongly encouraged.” This is the queue that merges. https://github.com/KellerJordan/modded-nanogpt/

2. Hugging Face transformers.
   Live product: releases roughly weekly through Sep 2026 (v5.17.0 on 9 Sep). 4,113 contributors, 323 commits/30d. This is the opposite of a dead queue. https://releases.sh/hugging-face/transformers
   Evidence of outside merge: DeepSeek-V3.2 landed in v5.11.0 (10 Jun 2026). That is how a lab-adjacent engineer gets a public artifact with their name on a file the frontier teams actually import.

3. DeepSeek open weights.
   V4-Flash/Pro (24 Apr 2026) MIT-license story is the distribution channel. Liang does not do office hours; the model cards and GitHub org are the door. SCMP notes Chen Deli as emerging public face. If you want Chinese-lab visibility, ship on their harness, don’t email Liang.

4. ARC Prize / Ndea-adjacent.
   Chollet’s 3 Sep 2026 post is an invitation dressed as a gloat: the benchmark was calibrated, agents closed it in six months, next contest is efficiency. Open-source eligibility is the point of the prize. https://x.com/fchollet/status/2095605239269519771

5. Fellowships that are real 2026 programs, not vibes.
   - Anthropic AI Safety Fellow / AI Security Fellow — live on anthropic.com/jobs. https://www.anthropic.com/jobs
   - Claude Corps: 12-month paid, cohort 1 ~100 fellows from 19 Oct 2026; 1,000 over three cohorts. Early-career only (<2 years). https://www.anthropic.com/claude-corps/fellow
   - Claude Campus / Builder Clubs: applications around 1–12 Sep 2026. https://claude.com/programs/campus
   - Claude for OSS: Max 20x for 6 months to maintainers of ≥5k-star repos, apply by 30 Jun 2026. https://www.anthropic.com/claude-for-oss-terms
   - MATS and Redwood still named as Anthropic junior pipelines in the TechTimes 1,680-engineer scrape (Jun 2026). That scrape also says conventional junior hiring is ~10% and not a cold-apply path.

6. Thinking Machines Tinker.
   Schulman publicly hiring “post-training hackers” for Tinker; Murati amplifying. This is one of the few ex-OpenAI labs still talking like a research lab with an API. Watch thinkingmachines.ai / Tinker, not the $40B rumor.

7. Discovery Loop (Dean).
   Brand new (Aug 2026), PBC, Google as LP, mandate = automate the experimental loop. If your work is eval harnesses, experiment orchestration, or scientific agents, this is a cleaner intellectual fit than “please hire me onto Claude pre-training.”

What does not respond
- Cold PRs to nanoGPT.
- Cold DMs to Sutskever/SSI (product-free, secretive; TIME’s line is that he “refuses to specify its research direction”).
- Anthropic/OpenAI core training via public GitHub: there isn’t one. 363 Anthropic jobs is the door, and the bar is prior lab/fellowship, not a nice README.
- LeCun AMI: they promised open papers/code, but it is a four-city research company with no product. Wait for the GitHub; don’t spam the chairman.

Practical sequence for this user (outsider, Palo Alto, wants a top-lab offer)
1. Pick one live queue (transformers, modded-nanogpt, or a DeepSeek/HF model card issue) and land a merged PR with a measurable number.
2. Write the eval/harness, not the 18th chatbot. Karpathy, Brown, and Amodei are all bottlenecked on evaluation, not on another demo.
3. Apply to Anthropic Safety/Security fellow + any remaining Claude Campus/Corps track. Those are the documented junior pipes.
4. Do not pitch “I believe in AGI.” Pitch a repo they can clone in 90 seconds. Karpathy’s entire public career is that test.

---

I. Contested / unverified (do not treat as fact)

- Amodei date: desk scan “2026-09-13” is the NYT date. The essay and TechCrunch/BBC/Guardian are 12 Sep 2026. https://www.nytimes.com/2026/09/13/technology/anthropic-ceo-slower-ai-development.html
- Aschenbrenner July 2026 collapse / 439% H1: FT letter via GuruFocus, SpotGamma, CNBC Faber unnamed sources. 13F cannot show July. Q3 13F (due mid-Nov 2026) is the verification.
- Nvidia $5B into SSI: TIME and shared-ai-research.org; I did not open an SSI or Nvidia 8-K.
- DeepSeek 400B-yuan valuation / Liang 20B-yuan check: SCMP sources, not a filing.
- Thinking Machines $40B / Nvidia $2.5B: The Information via Dealroom, “in talks.”
- Karpathy stake sizes in Perplexity, Etched, Adept, Lambda, Prime Intellect: Dealroom/Tracxn/Crustdata. He confirmed Flapping Airplanes and MatX in his own words.
- GPT-6 Astra 99.9% ARC-AGI-3, 16h METR horizon, Navier–Stokes with 10,000 agents: OpenAI/Brown/Huang social and secondary science press. I did not open a METR PDF or a refereed Navier–Stokes paper in this run. Treat as claimed.
- Hassabis titles: Observer/DCD say chair + Alphabet chief scientist, Kavukcuoglu running day-to-day, CEO title abolished. Google’s own blog would be better; I used those two.
- Hinton “Anthropic strayed” (NBC, Jun 2026) is paywalled; I have the headline and dek, not the full transcript.
- Elad Gil / Nat Friedman full 2026 books: not opened beyond MatX + Perplexity lists.
- Arthur Mensch 2026 theses: not enough primary in this pass to score him like LeCun.
- “OpenAI confidentially filed IPO June 2026” (shared-ai-research.org): unverified here.

---

J. Final verification pass — ten load-bearing claims

1. Karpathy → Anthropic 19 May 2026, pre-training, Nick Joseph, Claude-to-accelerate-pretraining. Rechecked: TechCrunch + Reuters + his X text. HOLD.
2. Sutskever “age of research” with big computers. Rechecked: Dwarkesh 25 Nov 2025 + BI quote. HOLD. Not a 2026 quote.
3. AMI $1.03B / $3.5B pre / world models. Rechecked: TechCrunch + Reuters 10 Mar 2026. HOLD.
4. Amodei pacing essay. Rechecked: opened https://darioamodei.com/post/we-must-pace-the-frontier. Date = Sep 2026, published 12 Sep. HOLD on substance; CORRECT the 13 Sep desk date.
5. Situational Awareness short chips then long memory/power. Rechecked: SEC CIK 0002045724, Q1 and Q2 13Fs. HOLD on positioning. July unwind = news, not 13F.
6. Altman Helion ~1/3, $1.7B mark, no OpenAI equity. Rechecked: Reuters 13 May 2026 court story. HOLD.
7. Karpathy Flapping Airplanes + MatX angels. Rechecked: his X 28 Jan and 25 Feb 2026. HOLD.
8. Huang “AGI has arrived” 6 Sep 2026. Rechecked: multiple outlets quoting the X post; Verge for the March Lex line. HOLD as a statement; FAIL as a scientific definition.
9. nanoGPT outside-merge is dead; transformers is live. Rechecked: GitHub PR pages + ecosyste.ms 0 past-year merges vs transformers release cadence. HOLD.
10. Liang Wenfeng still CEO, DeepSeek first external round ~Jun 2026. Rechecked: SCMP 18 Jun 2026 + Wikipedia 14 Sep 2026. HOLD on role; valuation = sourced journalism.

---

K. Research log

Queries run (this session)
Andrej Karpathy joins Anthropic pre-training team 2026; Ilya Sutskever "age of scaling is over" "age of research" 2026; Yann LeCun AMI Labs $1.03 billion raise world models 2026; Dario Amodei slow down AI development September 2026; Leopold Aschenbrenner Situational Awareness fund 13F short chipmakers 2026; most influential AI researchers 2026 Karpathy Sutskever Hassabis Amodei; Andrej Karpathy angel investor Perplexity Inception Simile MatX Flapping Airplanes; Sam Altman investments Helion Oklo Retro Biosciences Merge Labs World 2025 2026; Liang Wenfeng DeepSeek 2026 CEO role Yang Zhilin Moonshot Arthur Mensch Mistral; Ilya Sutskever Dwarkesh Patel transcript site:dwarkesh.com; Andrej Karpathy AGI timeline 2025 2026 "decade of agents" vibe coding; Dario Amodei "We Must Pace the Frontier" darioamodei.com; Mira Murati Thinking Machines Lab 2026 John Schulman; François Chollet AGI timeline 2025 2026 ARC prize; Richard Sutton "bitter lesson" 2025 2026; Daniel Kokotajlo AI 2027 timeline 2026 update; Anthropic hiring careers open source Claude GitHub merge 2026; Jeff Dean Google DeepMind 2026 Fei-Fei Li World Labs Atlas; Jan Leike Anthropic 2026 Chris Olah Tri Dao; Noam Brown OpenAI 2026; Jensen Huang AGI 2026; Situational Awareness LP SEC 13F-HR EDGAR 0002045724; Geoffrey Hinton 2026; Hugging Face transformers github merge PRs 2026 DeepSeek github nanoGPT karpathy llm.c.

Primary URLs opened (not just SERP snippets)
https://techcrunch.com/2026/05/19/openai-co-founder-andrej-karpathy-joins-anthropics-pre-training-team/
https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/
https://techcrunch.com/2026/09/12/anthropic-ceo-outlines-plan-to-pace-the-frontier/
https://darioamodei.com/post/we-must-pace-the-frontier
https://www.dwarkesh.com/p/ilya-sutskever-2
https://www.dwarkesh.com/p/andrej-karpathy
https://www.sec.gov/cgi-bin/browse-edgar?CIK=0002045724&owner=exclude&action=getcompany
https://www.reuters.com/legal/government/openai-chief-altman-has-over-2-billion-stake-companies-that-dealt-with-openai-2026-05-13/
https://x.com/karpathy/status/2016590919143952466
https://x.com/karpathy/status/2026452488434651264
https://x.com/fchollet/status/2095605239269519771
https://github.com/karpathy/llm.c/pulls
https://github.com/karpathy/nanoGPT/pulls
https://noambrown.com/
plus the TechCrunch/Reuters/TIME/Observer/SCMP/Verge/ABC pages listed in the citations above.

Minimum 25 primaries used: TechCrunch Karpathy; Reuters Karpathy; TechCrunch AMI; Reuters AMI; Amodei essay; TechCrunch pacing; Dwarkesh Sutskever; BI Sutskever; Reuters Altman court; Karpathy X Flapping; Karpathy X MatX; TIME Sutskever; TIME Amodeis; Observer DeepMind founders; TechCrunch Dean; Fei-Fei Atlas SiliconANGLE; Chollet X; Sutton Wikipedia/Oak; Kokotajlo tracker; Noam site; Verge Huang; Huang/BI Astra post; SCMP Liang; SEC Aschenbrenner 13F; ABC Hinton; Anthropic jobs; Claude Corps; llm.c PRs; nanoGPT PRs; transformers changelog.

---

L. Sources (plain list)

1. https://techcrunch.com/2026/05/19/openai-co-founder-andrej-karpathy-joins-anthropics-pre-training-team/
2. https://www.reuters.com/business/autos-transportation/former-tesla-ai-executive-openai-founding-member-andrej-karpathy-joins-anthropic-2026-05-19/
3. https://www.dwarkesh.com/p/ilya-sutskever-2
4. https://www.businessinsider.com/openai-cofounder-ilya-sutskever-scaling-ai-age-of-research-dwarkesh-2025-11?IR=T
5. https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/
6. https://www.reuters.com/business/ex-meta-ai-chief-yann-lecuns-ami-raises-103-billion-alternative-ai-approach-2026-03-10/
7. https://darioamodei.com/post/we-must-pace-the-frontier
8. https://techcrunch.com/2026/09/12/anthropic-ceo-outlines-plan-to-pace-the-frontier/
9. https://darioamodei.com/post/policy-on-the-ai-exponential
10. https://www.reuters.com/legal/government/openai-chief-altman-has-over-2-billion-stake-companies-that-dealt-with-openai-2026-05-13/
11. https://www.sec.gov/cgi-bin/browse-edgar?CIK=0002045724&owner=exclude&action=getcompany
12. https://x.com/karpathy/status/2016590919143952466
13. https://x.com/karpathy/status/2026452488434651264
14. https://observer.com/2026/02/simile-100m-startup-backed-fei-fei-li-andrej-karpathy/
15. https://time.com/collection/time100-ai/2026/ilya-sutskever/
16. https://time.com/collection/time100-ai/2026/dario-amodei-daniela-amodei/
17. https://observer.com/2026/09/google-deepmind-cofounders-ai-leadership/
18. https://techcrunch.com/2026/08/05/jeff-dean-and-other-top-ai-researchers-are-leaving-google-to-launch-their-own-startup/
19. https://siliconangle.com/2026/09/01/fei-fei-lis-world-labs-debuts-atlas-a-world-model-showcase-for-advanced-spatial-intelligence/
20. https://www.theverge.com/ai-artificial-intelligence/899086/jensen-huang-nvidia-agi
21. https://www.businessinsider.com/nvidia-jensen-huang-agi-openai-astra-ai-2026-9
22. https://x.com/fchollet/status/2095605239269519771
23. https://www.scmp.com/tech/big-tech/article/3357525/how-deepseeks-landmark-funding-secures-liang-wenfengs-grip-chinas-ai-rivalry-heats
24. https://www.abc.net.au/news/2026-09-14/godfather-of-ai-geoffrey-hinton-backs-ai-slow-down/107150010
25. https://noambrown.com/
26. https://www.anthropic.com/jobs
27. https://www.anthropic.com/claude-corps/fellow
28. https://github.com/karpathy/llm.c/pulls
29. https://github.com/karpathy/nanoGPT/pulls
30. https://github.com/KellerJordan/modded-nanogpt/
31. https://www.dwarkesh.com/p/andrej-karpathy
32. https://darioamodei.com/post/the-urgency-of-interpretability

---

🧒 Простыми словами: самые умные люди в ИИ сейчас спорят не «будет ли AGI», а « grok уже пришёл / ещё десять лет / вообще не через чат-ботов ». Деньги их предали лучше слов: кто орёт «тормози» — всё равно нанимает; кто орёт «AGI уже тут» — продаёт видеокарты; Карпати ушёл в Anthropic учить Клода учить Клода. Если хочешь, чтобы тебя заметили — не пиши им манифест, принеси смерженный PR в живую очередь (Hugging Face, modded-nanogpt, ARC) или в их стажировку. Мёртвые репозитории Карпати — это музей, не дверь.

🎙 Майк: идём верно — карта людей важнее очередного «кто прав про AGI». Направляющий вопрос: ты оптимизируешь под Anthropic (феллоушип + evals) или под публичную репутацию через open-source, которую Карпати/HF реально читают? Это два разных квартала работы, не одно и то же.
