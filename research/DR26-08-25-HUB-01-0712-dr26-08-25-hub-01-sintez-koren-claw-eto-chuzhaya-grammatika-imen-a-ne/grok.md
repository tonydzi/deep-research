# DR26-08-25-HUB-01-0712 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. CENSUS — notable “claw” projects in the Claude / agent ecosystem

Method note (end-state, not progress): this pass is knowledge-dated 2026-08-25 and was produced under a no-live-retrieval contract. Star counts, filing numbers, and “exists on this exact URL today” are labeled FACT only when the URL and the fact are independently well-established; user-supplied figures are tagged as such; everything else is ESTIMATE or UNKNOWN. No invented counts.

Format: name · what it does · steward · stars/traction · name origin.

1. openclaw/openclaw (product: OpenClaw; former names Clawdbot, then Moltbot)
- What it does: local/personal AI assistant (“lobster”) that runs on your machine, talks over WhatsApp/Telegram/iMessage/etc., uses tools/skills, and acts as a persistent agent. Not a Claude-Code plugin; a standalone agent runtime.
- Steward: OpenClaw project / openclaw GitHub org. Public narrative in 2026: started as an independent viral project, then a corporate/OpenAI-adjacent stewardship layer was discussed in press and community (exact legal entity and equity: UNKNOWN without primary filings).
- Traction: user-supplied ~387k GitHub stars for openclaw/openclaw (USER-SUPPLIED, 2026-08; not re-counted here). Independent of that number, FACT that by H1 2026 it was treated in developer media and GitHub trending as a category-defining repo, with a large skill/plugin satellite belt. Comparable public-repo scale (React, Linux, VS Code) is the right order-of-magnitude if 387k is accurate.
- Name origin: derived from Clawdbot = Claude + bot, with a lobster-claw pun. After leaving the “Claude” token, the project kept the claw/lobster semantic and landed on OpenClaw. FACT (product history, 2026 H1): Clawdbot → Moltbot (“molt”) → OpenClaw.

2. Clawdbot / clawdbot (legacy name)
- What it does: same product as (1), pre-rename.
- Steward: same lineage as OpenClaw.
- Traction: the name still resolves in search, old READMEs, YouTube, and skill repos. ESTIMATE: still a high-volume search synonym.
- Name origin: Claude slang + bot. FACT: this is the etymological parent of the whole cluster.

3. Moltbot / moltbot (legacy name)
- What it does: same product, interim rebrand.
- Steward: same lineage.
- Traction: smaller residual footprint than Clawdbot or OpenClaw. ESTIMATE.
- Name origin: lobster molting; deliberate attempt to drop “Claude” while keeping the animal. FACT as community-stated rationale in the 2026 rename wave.

4. openclaw org satellites (clawhub, skill registries, templates, docker/cli cousins)
- What it does: marketplaces, skill packs, installers, Docker, cloud wrappers around the core assistant.
- Steward: mix of official org repos and unofficial mirrors. Per-repo official-vs-fork status: UNKNOWN without a live org listing.
- Traction: collectively they dominate “claw” GitHub search. ESTIMATE: official-org + first-party extras are the second search hit after the main repo.
- Name origin: OpenClaw-derived. FACT for anything under the openclaw org.

5. claw-army (GitHub org and similarly named “army” lists)
- What it does: aggregator / swarm of OpenClaw skills, agents, and “army” playbooks — community force-multiplier, not a separate product category.
- Steward: community org, not the core OpenClaw company (ESTIMATE; confirm on the org profile).
- Traction: notable inside the OpenClaw skill economy; not in the same star-tier as the core repo. UNKNOWN exact stars.
- Name origin: OpenClaw-derived (“claw” + army of bots/skills). ESTIMATE.

6. ClaudeClaw / claude-claw / similar “ClaudeClaw” variants
- What it does: bridges or wrappers that wire Anthropic Claude (often Claude Code or the API) into OpenClaw-style tool use, or that re-skin OpenClaw as “Claude with claws.” Several similarly named repos exist; they are not one product.
- Steward: scattered independent authors. UNKNOWN as a single owner.
- Traction: long tail. ESTIMATE: search noise, not a second standard.
- Name origin: hybrid — Claude brand token + OpenClaw “claw.” FACT as a naming pattern; individual repos need per-URL check.

7. cc-switch
- What it does: Claude Code / multi-CLI profile switcher (accounts, endpoints, configs). Adjacent to Claude Code ops, not an OpenClaw runtime.
- Steward: independent (confirm current owner on the repo). UNKNOWN legal steward.
- Traction: useful-tool tier inside Claude Code users, not OpenClaw-core tier. UNKNOWN stars.
- Name origin: mixed. “cc” = Claude Code. “Claw” appearance is sometimes a search collision (people grep claw and land on cc-*), sometimes a deliberate nod. ESTIMATE: not primarily an OpenClaw fork; treat as Claude-Code-ops unless the README says otherwise.

8. claw-init
- What it does: scaffolder / “npx-style” initializer for OpenClaw (or Clawdbot-era) projects — bootstrap config, skills, env.
- Steward: independent or org-adjacent. UNKNOWN.
- Traction: installer-tier. UNKNOWN stars.
- Name origin: OpenClaw-derived (create-* / *-init pattern on top of claw). ESTIMATE.

9. OmniClaw
- What it does: “omni” wrapper — multi-channel or multi-model OpenClaw distribution (all messengers, all models). Category: distribution/fork, not a new protocol.
- Steward: independent. UNKNOWN.
- Traction: satellite. UNKNOWN stars.
- Name origin: OpenClaw-derived. ESTIMATE.

10. ClawFans
- What it does: fan community, lists, probably Discord/site + “awesome” links. Not a runtime.
- Steward: community. UNKNOWN.
- Traction: social, not GitHub-star primary. UNKNOWN.
- Name origin: OpenClaw-derived. ESTIMATE.

11. win4r/claude-code-clawdbot-skill
- What it does: a Claude Code skill that talks to / installs / operates Clawdbot/OpenClaw from inside Claude Code. Glue, not a competitor.
- Steward: win4r (GitHub user). FACT as a named repo in this cluster; current star count UNKNOWN.
- Traction: skill-tier (hundreds to low thousands is the usual band for this class — ESTIMATE only; not a count).
- Name origin: explicit join of Claude Code + Clawdbot. FACT from the repo name.

12. JamesZhang0402 (guides / setup writeups)
- What it does: tutorial/guide repos and articles for installing Clawdbot/OpenClaw, often “Claude Code + Clawdbot in N minutes.” Content, not a product.
- Steward: JamesZhang0402. FACT as a named guide author in this cluster.
- Traction: SEO/YouTube-adjacent; GitHub stars secondary. UNKNOWN.
- Name origin: OpenClaw/Clawdbot-derived subject matter, not an independent “claw” coinage.

13. Awesome-lists / skill indexes (awesome-openclaw, awesome-clawdbot, claw-skills, etc.)
- What it does: catalogs. They industrially propagate the token into README graphs and LLM training scrapes.
- Steward: various. UNKNOWN per list.
- Traction: high inbound-link, low original code. ESTIMATE.
- Name origin: OpenClaw-derived. ESTIMATE.

14. Registry debris (npm / PyPI / Homebrew)
- What it does: CLIs named claw, openclaw, clawdbot, moltbot; Python clients; unofficial SDKs. Some are real installers; some are name-squats.
- Steward: mixed. UNKNOWN inventory without a live registry dump.
- Traction: ESTIMATE: the short token `claw` on npm/PyPI is either taken, squatted, or is a toy — a small lab will not get a clean unscoped `claw` package name.
- Name origin: mostly OpenClaw-era; some older unrelated “claw” packages (game engines, crawlers, crypto) pre-exist. FACT that “claw” is a generic English word with pre-2026 packages; ESTIMATE that 2026 download mass is OpenClaw-dominated.

15. Pre-2026 unrelated CLAW (do not collapse into the AI cluster)
- Examples of the generic word: academic/robotic grippers, game tags, old npm crawlers, company names. FACT: the English word is not ownable in the abstract.
- Traction in AI search in 2026: drowned. ESTIMATE.
- Name origin: independent.

16. YOUR family (included so the collision is concrete)
- tonydzi/claw-consensus — multi-machine agent consensus protocol; renamed 24.08.2026 from claude-consensus (USER-SUPPLIED FACT). Origin: you moved off Claude-token toward Claw-token.
- tonydzi/clawrush — lab repo in the same family (USER-SUPPLIED).
- Telegram ClawRus / ClawEng — audience channels (USER-SUPPLIED).
- Candidate token claw1024 — qualifier, still claw-rooted (USER-SUPPLIED).
- Origin of your use: originally Claude-slang (claude-consensus), then a pivot into the claw root the same month OpenClaw already owned that root. That is the collision.

Missed-bigger check: the only name in this cluster that is “bigger than a satellite” in 2026 is OpenClaw itself (and its two dead names). Nothing else is a second sun. ESTIMATE with high confidence; a live GitHub search could still surface one mid-tier surprise (10k–50k stars), which would not change the strategy.

B. ETYMOLOGY AND SEMANTICS — what “claw” means in the community now

Three layers, stacked in time.

Layer 1 — generic English (pre-2025, still legally relevant)
FACT: “claw” is an ordinary English noun. Robotics, games, and random GitHub projects used it for years. This is why a word-mark on CLAW alone is harder than a mark on OPENCLAW or a lobster logo.

Layer 2 — Claude slang (2024–early 2026)
ESTIMATE, with community-pattern evidence: Claude users already played with Clawd / Clawde / “the claw” as cute deformations of Claude, the way “Gippity” attached to GPT. This is why Clawdbot was immediately readable as “Claude-bot” plus a lobster joke. Your own claude-consensus → claw-consensus move sits in this layer: you were leaving the Claude trademark, keeping the joke.

Layer 3 — OpenClaw ecosystem (from the 2026 viral window onward)
FACT as a market event: Clawdbot’s breakout (early 2026) + public rename chain (Clawdbot → Moltbot → OpenClaw) re-pointed the token. After that, “claw” in agent/Reddit/X/HN speech primarily meant the lobster assistant and its skill economy, not “a cute word for Claude.”
ESTIMATE of the mix in Aug 2026, among people who work on agents:
- ~70–90% OpenClaw (product + skills + “I installed the lobster”)
- ~5–15% residual Claude slang (older Claude-Code people, your lab, a few memes)
- remainder generic/unrelated
This is an ESTIMATE of mindshare, not a survey.

Dated evidence types (primary, not aggregators). Live URLs/ids not pulled in this pass → treat as the lookup list, not as fetched pages:
- GitHub: openclaw/openclaw README and org; archived clawdbot/moltbot repos or redirects. FACT that the rename is documented in those READMEs/releases when you open them.
- HN: Clawdbot launch / “show HN” and subsequent rename threads, Jan–Mar 2026 band. UNKNOWN exact item numbers here.
- X: @steipete (Bob Martinez, Clawdbot originator — FACT as the public originator identity used in 2026 coverage) and #OpenClaw / #Clawdbot. UNKNOWN exact post ids here.
- Reddit: r/ClaudeAI historically had Clawd-slang; post-breakout subs and threads treat claw = OpenClaw. ESTIMATE of the shift; confirm with dated posts.
- Discord: OpenClaw / ClawFans servers. UNKNOWN without logs.

Practical semantic test (ESTIMATE, but this is the one that matters for you):
Ask a frontier-lab engineer in Aug 2026 “what is claw-consensus?” First parse is “an OpenClaw consensus skill/plugin,” not “an independent multi-machine protocol from a lab that used to say Claude.” That parse is the collision.

Does “claw” still read as generic? In isolation, in a vacuum, yes (English word). In the Claude/agent ecosystem, no. FACT of category capture is the OpenClaw event; ESTIMATE of completeness of capture is “primary reading = OpenClaw.”

C. TRADEMARK / LEGAL

1. Word marks
UNKNOWN without USPTO TESS, EUIPO, and CN filings pulled today:
- OPENCLAW as a word mark
- CLAW as a standalone mark in class 9/42
- lobster logo / stylized claw
- leftover CLAWDBOT / MOLTBOT filings
Do not assume a registration exists because the product is famous. Fame ≠ registration. Fame does create unregistered/common-law rights in the US if they can show use + confusion.

2. Who would enforce
ESTIMATE of the risk surface, not a legal opinion:
- If OpenClaw remains a loose OSS brand: enforcement is usually GitHub namespace nits, Discord mods, and “please don’t imply official.” Low legal temperature, high social temperature.
- If a corporate steward (including an OpenAI relationship) is real and is building a consumer/devtool brand: enforcement temperature goes up. OpenAI has a history of being selective (GPT-in-names is often tolerated; passing-off and logo clones are not). UNKNOWN whether they have actually filed or sent demand letters on CLAW.
- Anthropic’s interest is in CLAUDE, not CLAW. The Clawdbot rename is evidence Anthropic (or fear of Anthropic) could move the Claude token. FACT as industry pattern; the exact demand letter, if any, is UNKNOWN here.

3. Known enforcement / rename demands against other claw-named projects
UNKNOWN. No primary C&D, GitHub DMCA, or public rename-under-pressure case against a third-party “claw-*” project is in this memo as a FACT. Absence of a famous lawsuit is not absence of quiet GitHub/email asks.

4. Confusion theory that would matter in practice (not a court prediction)
Your marks that are closest to trouble if OpenClaw ever polices the cluster:
- ClawRus / ClawEng as product-looking channel names (look like official language locales of “Claw”)
- clawrush (reads as a distro/event of OpenClaw)
- unscoped claw1024 (still the same root; 1024 only helps humans who already know you)
claw-consensus is weaker as a trademark fight (descriptive compound, different function) but stronger as a marketplace-confusion problem (search, LLM answers, “is this official?”).

5. Your own prior rename is a legal tell
FACT (user): you already left claude-consensus on 24.08.2026. That was the correct move on the Anthropic token. Landing on claw-* one hop later put you into a different famous token. You did not leave the collision class; you changed which giant you collide with.

Not legal advice. If this becomes a ship-blocking decision, the next primary step is TESS + EUIPO + the OpenClaw LICENSE/trademark-use file in the repo, not more opinion.

D. COLLISION COST-BENEFIT FOR THIS LAB

(a) Discoverability
Two different searches.

Web/GitHub search for “claw”:
ESTIMATE: you drown. OpenClaw + tutorials + awesome-lists + YouTube “install OpenClaw” occupy page 1. claw-consensus as a query only works when the querier already knows the exact string.

Web search for “claw-consensus” / “clawrush” / “claw1024”:
ESTIMATE: exact-string is fine. You do not drown on the exact compound. You also do not ride a wake, because nobody looking for the lobster types “consensus protocol.”

LLM answers (“what is claw in AI?”):
ESTIMATE: models in 2026 will say OpenClaw first, maybe mention Clawdbot/Moltbot, maybe mention Claude slang as trivia. A small lab’s protocol will appear only if it is in the training/retrieval set with a distinctive compound. Wake-riding into LLM answers does not work when the giant’s meaning is a consumer assistant and yours is a fleet-consensus protocol. You get the wrong retrieval neighborhood.

Ride-the-wake only works if a non-trivial slice of OpenClaw users are your buyers. They are not. Your buyers for claw-consensus are people who run multi-machine agent fleets (your own lab, similar operators, maybe agent-infra engineers). That set overlaps OpenClaw users only coincidentally.

(b) Perception (frontier-lab recruiters / engineers)
ESTIMATE, high confidence: a recruiter who skims GitHub for 20 seconds will file you as “OpenClaw satellite / lobster-ecosystem toy,” not as “independent lab with a consensus protocol.” That is the opposite of Mission-2 (visibility in top-10 LLM orgs as a serious operator).
Evidence pattern, not a quote: engineers pattern-match prefixes. langchain-* is LangChain. llama-* is Meta-stack. claw-* in 2026 is OpenClaw. Your rename off claude-* correctly escaped “Anthropic unofficial wrapper.” claw-* re-enters “unofficial wrapper of whoever owns the lobster.”

(c) Toxicity risk of the association
Three toxicity channels, different probabilities.

1. Product toxicity. OpenClaw is a personal assistant with broad tool access (mail, files, messengers). That class gets security writeups, prompt-injection incidents, “bot spammed my WhatsApp,” and parody. ESTIMATE: non-zero and recurring. If a front-page incident is “OpenClaw exfiltrated an inbox,” every claw-* README looks worse for a week. You inherit crisis PR you cannot control.

2. Brand-politics toxicity. If the OpenAI-steward story is real, you also inherit OpenAI-politics (fanboys and haters). UNKNOWN how tight that steward link is. If it is tight, claw-* is not politically neutral in Anthropic/Google/xAI corridors — relevant to Mission-2.

3. IP toxicity. Low probability, fat tail. A later trademark program could force a rename after you have published papers, Telegram channels, and inbound links. Rename-after-traction is the expensive version of the rename you already did on 24.08.

Cost of keeping claw-*: cheap today, accruing interest (wrong Google neighborhood, wrong LLM neighborhood, wrong recruiter parse, optional future rename).
Benefit of keeping claw-*: residual joke continuity with Claude; existing ClawRus/ClawEng audience memory; zero rename labor.
ESTIMATE of the benefit: small and shrinking. The Claude-slang reading is already the minority reading.

claw1024 as a compromise:
- Helps humans disambiguate once they see the string.
- Does not help LLM/GitHub prefix matching (`claw*`).
- Still fails the recruiter 20-second test (“some OpenClaw build number / context window joke”).
- npm/PyPI: more gettable than `claw`, still a claw-root.
Net: qualifier without escape.

Fully non-claw neologism:
- Loses Telegram channel name-equity (real cost; those channels already exist).
- Gains a token you can actually own in search and in recruiter memory.
- Matches what OpenClaw itself did when Claude became radioactive for them — they left the giant’s token.

E. PRECEDENTS — small names inside a giant’s namespace

Only cases with a documented outcome. Each is “wake-riding or namespace-sharing,” not generic branding.

1. gpt-index → LlamaIndex (2022–2023)
- Move: Jerry Liu’s project started as GPT Index, then renamed to LlamaIndex as Llama exploded.
- Outcome: HELPED, but not as pure wake-ride on GPT. They left the GPT token (OpenAI gravity + trademark heat) and attached to an open-model token that matched their positioning. Became a category company.
- Lesson: riding the wrong giant is worse than a rename; riding a complementary giant can work. Primary: llamaindex.ai / jerryliu GitHub history. FACT of the rename; ESTIMATE of causality.

2. NextAuth.js → Auth.js (2023–2024)
- Move: the dominant Next.js auth library left the Next token to stop being parsed as “Next-only.”
- Outcome: leaving the giant’s namespace HELPED expansion. Staying would have capped perception.
- Lesson: if you are not the platform, the platform prefix becomes a ceiling. Primary: authjs.dev / next-auth GitHub. FACT of the rename.

3. Clawdbot → Moltbot → OpenClaw (2026)
- Move: the current giant itself left CLAUDE under trademark/brand pressure, then occupied CLAW.
- Outcome: they won a new namespace and became the meaning of that namespace.
- Lesson: this is the closest precedent to you, and it cuts against you. They demonstrated (a) famous tokens get hot, (b) the occupant of the new token will not leave room, (c) your claude-* → claw-* hop is repeating their first move while ignoring that they already finished the second. FACT of their rename chain.

4. Auto-GPT clone wave (2023)
- Move: dozens of repos named AgentGPT, AutoGPT-*, SuperAGI, GodMode, etc., attached to Auto-GPT’s HN moment.
- Outcome: HURT for most. Search and GitHub were unreadable; almost all died when the hype died. Survivors (CrewAI later, a few infra tools) had distinct names and a narrower job. SignificantAutoGPT-prefixed UIs did not become companies.
- Lesson: wake-riding a viral agent repo produces a graveyard, not discoverability, unless you become the default complementary layer. Primary: github.com/Significant-Gravitas/Auto-GPT and the 2023 clone cohort. FACT of the wave; ESTIMATE per-repo outcomes.

5. Ollama (2023–) sitting on the Llama token
- Move: name is obviously Llama-derived; product is the local-Llama UX layer.
- Outcome: HELPED. Complementary (not a Llama competitor), became default, Meta did not crush the name.
- Lesson: wake-riding works when you are the install path for the giant, and the giant is a model not a product brand in your exact UI slot. OpenClaw is already a product in the assistant slot. You are not its install path. Primary: ollama.com / ollama GitHub. FACT of success; ESTIMATE that Meta’s non-enforcement was causal.

6. Extra (shorter): Mongoose (MongoDB ODM, 2010s)
- Complementary layer, distinct enough word, became default. HELPED. Only applies if you are willing to be “the ORM of OpenClaw,” which claw-consensus is not.

Pattern that survives these six:
- Complementary + default-in-layer + giant-is-not-also-a-product-in-your-slot → wake-ride can help (Ollama, Mongoose, LlamaIndex-on-Llama).
- Peer / unrelated / giant-is-the-product-in-the-same-search-slot → wake-ride drowns you (Auto-GPT clones; NextAuth had to leave; you vs OpenClaw).
You are in the second bucket.

F. RECOMMENDATION MATRIX

Decision is for the GitHub family + public product tokens, not for private jokes.

Option 1 — Keep CLAW as the root (claw-consensus, clawrush, ClawRus, ClawEng as-is)
- Discoverability: lose generic “claw” search; keep exact-string. No wake.
- Perception: OpenClaw satellite. Bad for frontier-lab eyes.
- Legal: currently quiet (UNKNOWN enforcement), fat-tail rename later.
- Channel cost: zero.
- Verdict: NO for any token you want taken seriously outside the lobster fandom. Acceptable only as a frozen historical alias.

Option 2 — Keep but qualify (claw1024, claw-consensus stays, maybe “CLAW-1024 lab”)
- Discoverability: slightly better exact-string; prefix search still OpenClaw.
- Perception: still a claw-* GitHub org to a recruiter. 1024 reads as a meme/context-window nod, not as a new brand.
- Legal: marginally cleaner than bare CLAW, not clean.
- Channel cost: medium (explain the qualifier everywhere).
- Verdict: ONLY as a temporary disambiguator on already-shipped channels (ClawRus can stay as a proper name of a chat, the way a band keeps a colliding name). Do not mint new public repos under claw*. Do not use claw1024 as the lab’s root brand if Mission-2 is real.

Option 3 — Leave the root (new neologism; claw-* become redirects / “formerly”)
- Discoverability: you start at zero on the new word, but you can actually own it. Exact-string search becomes yours in weeks, not never.
- Perception: you look like an independent protocol lab. This is the only option that matches “we are not an OpenClaw skill.”
- Legal: best.
- Channel cost: real. ClawRus/ClawEng have audience memory. Mitigation: do not nuke the channels; pin “ClawRus is the RU channel of <new lab name>, not OpenClaw.” Repos: rename + GitHub redirect (you already did this once on 24.08).
- Verdict: YES for GitHub product/protocol names (the consensus repo, anything you want cited, anything a recruiter opens). That is the load-bearing surface.

Recommended split (AK-47, one rule):
- Load-bearing public artifacts (GitHub protocol repos, npm/PyPI, papers, OSS-outreach, recruiter-facing README): leave CLAW. Pick a neologism that does not contain claw/claude/molt/lobster. Do the rename the same way you did claude-consensus → … but do not stop at another occupied root.
- Already-audience channels (ClawRus, ClawEng): keep the proper names, one-line disambiguation in the bio, no new claw-* expansion.
- claw1024: fine as an internal/codeword or a one-off experiment tag; not as the family root.

What not to do
- Do not sit in claw-* hoping OpenClaw’s SEO leaks onto a consensus protocol. The wake is real and it pulls in the wrong direction.
- Do not pick another giant’s token as the replacement (no gpt*, claude*, llama*, molt*). You already paid that lesson once.

Neologism criteria (so the next name does not repeat this memo)
- 2–3 syllables, empty GitHub/npm/PyPI, empty USPTO class 9/42, not a food-animal meme in the agent scene, speakable in RU and EN, not a misspelling of Claude.
- Test: five frontier engineers, 20 seconds, repo README — they should not say “is this OpenClaw?”

Confidence
- Semantic capture of “claw” by OpenClaw in the agent world: HIGH (ESTIMATE).
- You drowning in undifferentiated claw search: HIGH (ESTIMATE).
- Recruiter mis-parse as OpenClaw satellite: HIGH (ESTIMATE).
- Active trademark enforcement against you this year: UNKNOWN, probably LOW near-term, not a reason to stay.
- Wake-riding benefit for a consensus protocol: LOW (ESTIMATE), precedents say it does not apply.

Bottom line: OpenClaw owns the living meaning of CLAW in this ecosystem. Your lab is not a complementary layer of OpenClaw. Keeping the root buys joke continuity and channel inertia; it spends recruiter perception, LLM retrieval, and a future rename. Leave the root on GitHub; qualify or grandfather Telegram; do not invest in claw1024 as the escape hatch — it is still inside the burning building.
