# DR26-08-06-ZB-01-0934 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-06-ZB-01-0934
rail: grok
vendor: xAI Grok (CLI, node direct)
door: cli-headless (node direct)
collected: 2026-08-06T12:59:28Z
bytes: 29075
urls: 25
secs: 253
exit: 0
---

I'll research dev.to as a practitioner playbook: real posts, rules, mechanics, cold-start data, GEO citability, and alternatives — with dated sources and confidence labels.Official docs and high-engagement posts next — guidelines, editor features, and what actually trends.Pulling primary pages: editor guide, code of conduct, top lists, and high-engagement post samples.# DR Playbook: Becoming a Respected, Cited Author on DEV Community (dev.to) from Zero — 2026

**Audience:** Palo Alto AI Research Lab (founder Anton Dziatkovskii; multi-agent OSS tooling; “everything we build becomes content”)  
**Goals:** GEO/LLM citability + engineer-tester recruitment + frontier-AI hiring/acqui-hire signal  
**Research date:** 2026-08-06  
**Scope note:** Platform rules and top-list composition change. AI policy below is still the official staff post (Dec 2022 / CoC reference July 2023). Treat enforcement norms as observed practice, not a legal guarantee.

---

## 1. One-paragraph verdict

**Yes — invest in dev.to as a secondary English distribution rail, not as the lab’s primary reputation engine.** DEV gives free discovery, Markdown+embeds, org accounts, and a beginner-to-mid audience that rewards first-person “I built / I broke / here’s the code” posts; top non-staff posts in the yearly ranking sit in the ~200–350 reaction band with heavy comment threads. It is **not** where senior systems engineers primarily form reputation (that remains personal blog → HN / Lobsters / GitHub). For your profile, the win is: (1) English syndication of lab build-logs with `canonical_url` pointing home, (2) `#showdev` launches of open-source tools with installable demos, (3) transparent AI-assist disclosure that matches official rules. Expect **modest early metrics** (tens to low hundreds of views per post at cold start unless external traffic or a hit title lands) and treat 90 days as a **quality+consistency experiment**, not a follower rocket. [established] on mechanics (editor guide, CoC, orgs); [emerging] on GEO value of DEV specifically; [speculative] on acqui-hire signal from DEV alone.

---

## 2. Native voice, evidenced

### 2.1 What top 2025–2026 posts sound like

From yearly top non-staff pieces and their openings:

| Post | Date / engagement (as listed on `/top/year`) | Opening pattern |
|------|-----------------------------------------------|-----------------|
| [I Replaced Redis with PostgreSQL (And It's Faster)](https://dev.to/polliog/i-replaced-redis-with-postgresql-and-its-faster-4942) — Polliog | Jan 2026 · ~337 reactions · 46 comments | Stack snapshot → pain (“Two databases…”) → bold claim → “Here’s what happened.” |
| [You're Not Building Netflix: Stop Coding Like You Are](https://dev.to/adamthedeveloper/youre-not-building-netflix-stop-coding-like-you-are-1707) — Adam | Nov 2025 · ~345 reactions · 100+ comments | Joke about junior→over-engineer arc → anecdote → war-crime code dump. |
| [Stop Installing Libraries: 10 Browser APIs…](https://dev.to/sylwia-lask/stop-installing-libraries-10-browser-apis-that-already-solve-your-problems-35bi) — Sylwia Laskowska | Feb 2026 · ~265 reactions · 90+ comments | Soft thesis → personal “what should I write” → list promise → light humour. |

**Voice features that recur** [established from corpus of these posts]:

- **Person:** first person **“I”** dominates technical posts; “we” appears for team/legacy code, not corporate brand voice.
- **Tense:** past simple for the story, present for the lesson and code (“I ripped out… Here’s what happened”).
- **Sentence length:** short hooks (5–12 words), then medium explanation; lists and tables break walls of text.
- **Humour:** dry, self-deprecating, insider (“I’m not even joking”, “a bit creepy. Very useful”) — not standup monologues.
- **Structure:** problem → concrete code → numbers/decision matrix → **when NOT to** → TL;DR + comment question.
- **Opening 2–3 lines:** (1) concrete situation, (2) friction in one breath, (3) payoff or “here’s what I did.”

**Post-mortem / “we broke it” genre on DEV:** **Yes, rewarded — but as *personal* technical narrative with code and trade-offs, not pure culture-war HN essay.** Polliog’s Redis→Postgres post is a soft post-mortem (cost, ops complexity, latency tables, decision matrix) and attracted thoughtful technical comments *plus* AI-slop accusations. Adam’s over-engineering piece is opinionated post-mortem of architecture debt. HN still owns the brutal “company post-mortem / security incident” prestige lane; DEV owns the **tutorialized failure with copy-pasteable fixes**. [emerging]

### 2.2 Voice that repels (failure modes with evidence)

| Failure mode | Evidence | Severity |
|--------------|----------|----------|
| **Undisclosed / obvious AI slop** | Comments on high-engagement posts: “Thanks Chat GPT. Dead internet theory…”, “what is this slop?”; CoC requires AI disclosure. | High — reputation kill, report risk |
| **Corporate/salesy product pitch** | `#showdev` mods: community-driven, not “overly corporate or salesy”; staff: advertising belongs in Listings. | High for brand accounts |
| **SEO farm / mass backlinks** | AI guidelines ban posts whose main purpose is SEO backlinks (exceptions: personal blog; company blog under org). | Ban/suspension risk |
| **Generic listicles without lived detail** | Community note that beginner web lists do well *but* senior readers dismiss “5 tips” energy. | Soft — low trust |
| **Wall of unbroken prose / no code** | Official formatting series stresses H2 structure, code blocks, a11y. | Soft — low completion |
| **Comment spam / bot comments** | AI guidelines: do not use AI to generate comments. | Suspension risk |

### 2.3 Same topic, five platforms (hypothetical: “We replaced Redis with Postgres for agent job queues”)

| Platform | Native shape |
|----------|----------------|
| **dev.to** | “I Replaced Redis with Postgres for Our Agent Queue (And When You Shouldn’t).” Short hook, code, table, decision matrix, ask for benchmarks in comments. |
| **Hacker News** | Link to long post-mortem; title understated; comments demolish methodology; no emoji; expect “this is wrong because…”. |
| **Medium** | Softer narrative, more subheads, “lessons for product teams,” possible paywall noise. |
| **Reddit r/programming** | Direct link + one-line context; no blog-spam vibe; flair; readiness for “benchmarks or GTFO.” |
| **Habr** | Longer formal tech article, diagrams, RU/EN rigor, fewer meme titles, more systems depth expected. |

### 2.4 Voice spec (copyable)

**Do**

- Lead with **I did X under constraint Y** in 1–2 sentences.
- Show **numbers** (latency, $ cost, failure modes) even if small-scale.
- Include **when NOT to do this** (decision matrix).
- End with a **specific question** that invites engineers to argue.
- Use **H2s**, fenced code with language tags, tables, TL;DR near end (or top for GEO).
- Disclose AI assist per CoC (`#ABotWroteThis` or one clear sentence).

**Don’t**

- Open with “In today’s rapidly evolving AI landscape…”
- Publish pure product landing pages under `#showdev`
- Hide that Mycroft drafted + human reviewed
- Flood comments with auto-thanks
- Use 11-file architecture essays without a punchline and real code

**Three openings — same paragraph (lab topic)**

**Bad (repels):**  
“In the modern multi-agent orchestration paradigm, organizations are increasingly leveraging sophisticated tooling ecosystems to maximize operational excellence and developer productivity across distributed AI workloads.”

**Acceptable (generic tech blog):**  
“Running a fleet of LLM agents creates a lot of background jobs. We used Redis for queues, then moved most of that work into PostgreSQL. This post explains why and how.”

**Native (DEV-shaped):**  
“We had Redis for agent job queues and Postgres for everything else. Two backups. Two outage modes. One 2 a.m. page when the queue and the DB disagreed. I ripped Redis out of that path — here’s the code, the numbers, and when you should keep Redis.”

---

## 3. Platform mechanics that change reach

### 3.1 Tags

- **Max 4 tags**, comma-separated in front matter. [established]
- Tags feed discovery: readers follow tags; posts surface on tag pages (`/t/javascript`, `/t/showdev`, etc.). Official topics guide: choose active communities + language/tool tags.
- **High-traffic general tags** (observed on top posts): `webdev`, `javascript`, `programming`, `ai`, `discuss`, `showdev`, `career`. [emerging — observational]
- **Dead/thin tags:** obscure product names, invented hashtags, over-niche labels with no followers. [speculative — no public dead-tag list]
- **Re-tagging:** Tag moderators can **remove** a tag that doesn’t fit and sometimes **add** one if the post has fewer than 4 tags. `#showdev` mod confirms this. [established]

**Lab tag menu (rotate, never waste a slot):**  
`showdev` · `ai` · `opensource` · `python`/`javascript` · `architecture` · `productivity` · `discuss` (for opinion/post-mortem)

### 3.2 `canonical_url`

- Front matter: `canonical_url: https://your-original-url` tells search engines the preferred source. [established]
- Cross-posting **without** canonical risks DEV’s stronger DA outranking your blog. Practitioners recommend setting canonical **to origin** and often **delaying** cross-post ~48–72h so Google indexes the original first. [emerging — practitioner advice, not Google policy doc]
- **Direction that wins for lab goals:**  
  - **Own domain first** (authority + GEO home) → DEV with `canonical_url` home.  
  - **DEV-native exclusive** only when you want max DEV feed performance and will later reverse-syndicate carefully.  
- DEV does not publicly document “de-ranking cross-posts”; de-ranking is a **search-engine duplicate-content** issue, not a proven DEV feed penalty. [established on canonical support; speculative on feed de-rank]

### 3.3 Series

- Front matter `series: Name` groups posts. Official “Best Practices: Creating a Series” exists (referenced from formatting series).
- **Compounds** when each part is self-contained and links prev/next; **fragments** when part 3 is unreadable alone. [emerging]
- Use for multi-agent deep dives: “Building Multi-Agent Tooling in Public” parts 1–N.

### 3.4 Liquid tags / embeds

Official embeds via `{% embed URL %}` include **GitHub (repo, issue, gist), CodePen, CodeSandbox, YouTube, StackBlitz, Twitter/X, Reddit**, etc. Also `{% link %}`, `{% user %}`, `{% organization %}`.

**What helps your lab:** GitHub repo cards, Gist of the failing config, short Loom/YouTube of the agent loop. Avoid embed spam.

### 3.5 Cover images

- Official: **best size 1000 × 420**. [established]
- Measurable CTR effect: **[no source found]** for DEV-specific A/B. Treat as “helps social previews and feed polish,” not proven conversion science. [speculative]
- Style 2026: readable title text on solid/gradient, high contrast, no tiny screenshots of code (use real code blocks).

### 3.6 Timing

- One DEV author analysis (Nov 2024): timing “should not be so important” but good posts can still lag in main feed. [single-source] [low-authority]
- General social data (not DEV-specific): midweek daytime. [weak transfer]
- **Practical rule:** post when **you will be online to reply within 1–4 hours** (early engagement). [emerging practice]

### 3.7 Reactions, top lists, what drives them

Reactions: ❤️ heart · 🦄 unicorn · 🤯 exploding head · 🙌 raised hands · 🔥 fire (UI confirms multi-type reactions).

**Drivers onto weekly/monthly/year tops** (mixed evidence):

1. Early engagement (comments + reactions)  
2. Tag followership  
3. **Editorial “Top 7 Featured”** and social promotion by DEV team (not pure algorithm) — staff describe human curation and “balancing the feeds”. [established that curation exists]
4. Contrarian + useful titles (“I replaced X with Y”, “Stop doing Z”)

Year tops are **heavily mixed with DEV Team challenges** — organic authors compete with prize-pool announcements.

### 3.8 Comment norms

- Reply **personally and specifically** (see Polliog / Adam threads — authors re-engage for days).  
- Disagreement: steelman, then numbers; don’t pile-on.  
- Latency: same-day ideal; within 24–48h minimum for a rising post. [emerging — observed, not codified]  
- Do **not** bot-comment.

---

## 4. Self-promotion boundaries (critical)

### 4.1 Rules vs enforcement

| Layer | Rule | Source date |
|-------|------|-------------|
| CoC | Cite sources; follow AI guidelines & disclose AI | Updated **2023-07-31** |
| AI guidelines | Allowed if disclosed, good faith, fact-checked; **not** for clout/SEO spam/scams | Staff post **2022-12-19** (still linked from CoC) |
| `#showdev` | Show projects/products; **community-driven, not overly corporate/salesy** | Tag mod post **2023-01**; tag page current |
| Spam fight | Report abuse; Gemini-based spam defense described **2026-01** | Staff posts 2022, 2026 |

**Acceptable `#showdev`:** problem you hit → what you built → architecture/code → limits → GitHub link → ask for testers.  
**Spam:** multi-post product blasts, link-only articles, “use our SaaS” without technical meat, undisclosed AI SEO farms.

### 4.2 Personal vs org

- **Org accounts are first-class and free** (landing page, branding, CTA box, analytics, followers). Official help docs. [established]
- Marketer guidance (older but still cited): **human face outperforms pure brand**; create personal account first, then org and post as person-for-org. [single-source] [low-authority for 2026 metrics]
- **Lab recommendation:** `Anton` personal + `PaloAltoAI` org; publish under Anton “for PaloAltoAI” so comments feel human.

### 4.3 AI-generated / AI-assisted (2026)

**Official policy (still the one CoC points to):** AI-assisted content is **allowed** if:

1. Good faith  
2. **Disclosed** (`#ABotWroteThis` or explicit sentence, even at end)  
3. Fact-checked by a human who understands the topic  
4. Not primarily for brand clout / SEO spam / scams  

AI-generated **comments** forbidden (except translation/AT tools). [established as written policy; **possibly stale on enforcement intensity** — base text is 2022–23]

**Community tolerance 2025–26:** harsher than the formal policy. Even technical posts get “ChatGPT” accusations; structured summary-comments get roasted as slop. **Disclosure helps legally/platform-wise; it does not fully protect against taste backlash.** Human voice + unique data + “when not to” is the real protection. [emerging]

**For Mycroft-drafted, human-reviewed lab posts — recommended disclosure (compliant + non-cringy):**

> *Draft assisted by our synthetic co-founder (LLM); architecture, numbers, and final wording reviewed by a human who ran the system. We stand behind the claims.*

Avoid making “AI wrote this” the hook unless the post is *about* AI writing experiments.

---

## 5. Cold start from zero

### 5.1 Realistic 90-day trajectory (numbers, not vibes)

**Hard truth:** there is **no public DEV median dashboard**. Numbers below are **ranges synthesized from single-author case studies + feed observation**. Mark [speculative] where noted.

| Metric | Weeks 1–4 | Weeks 5–8 | Weeks 9–12 (good execution) |
|--------|-----------|-----------|------------------------------|
| Views / post | 20–200 | 100–800 | 300–3,000 (hit: 10k+) |
| Reactions / post | 0–15 | 5–40 | 20–100+ |
| Comments / post | 0–5 | 2–15 | 5–40 |
| Followers total | 0–50 | 50–300 | 200–1,000 |
| Posts | 6–12 | +6–12 | +6–12 |

**Case study (optimistic upper bound, not median):** Muhammad Usman, joined late Nov 2024 → **10k followers by Feb 2025**, **53 posts**, **~43k total views**, 752 reactions, 118 comments — heavy beginner-web list content. Commenter Cesar Aguirre: DEV “tends to be a place for beginners”; beginner-friendly web content works. [single-source trajectory] [low-authority for generalizing]

**For a multi-agent tooling lab:** expect **lower follower counts than listicle creators**, but **higher quality inbound** (GitHub stars, tester DMs) if posts are specific. [speculative]

### 5.2 What accelerates cold start

Ranked by likely ROI for you:

1. **External referral** (X, TG, HN when earned, GitHub README link) — strongest first-day spike  
2. **Comment first for 1–2 weeks** on adjacent posts (real answers, not “great post!”) — wellness badges / visibility [emerging]  
3. **Right tags + readable title**  
4. **Series** after 2–3 related posts  
5. **DEV Challenges** (optional; good for badges, dilutes lab brand if overused)  
6. Newsletter / Top 7 — not controllable; write for humans first  

### 5.3 Consistency vs volume

- Consistency of **quality at a sustainable cadence** beats raw volume of thin posts (policy + community both punish farm volume).  
- Author commentary favors **cadence you can sustain** + topic fit over pure volume. [emerging]  
- Suggested lab cadence: **2 posts/week** (1 deep build-log + 1 short tip/showdev) rather than 4 AI-thin posts.

---

## 6. GEO / LLM-citability

### 6.1 Is DEV content actually cited?

- **Broad GEO research 2025–26** finds citation engines skew: ChatGPT → encyclopedic (e.g. Wikipedia-heavy), Perplexity → community/social (Reddit-heavy), Claude → blogs-heavy in some analyses. [emerging — third-party studies; methods vary]
- **DEV-specific share of citations: [no source found].** Do not claim “DEV is highly cited by Claude.”  
- **Comparative guess (label clearly):** GitHub READMEs + personal blogs with clear versioned facts often beat UGC blogs for tool how-tos; Medium is mixed; Habr is strong in RU LLM corpora but weaker EN; DEV is **indexable public HTML** so it *can* be retrieved, but is not a top documented citation source in the studies above. [speculative]

**Experiment that would settle it for the lab (run quarterly):**  
Query ChatGPT / Perplexity / Claude / AI Overviews with 10 fixed prompts about *your* tools and multi-agent ops; log whether `dev.to/...` URLs appear. Until then, treat GEO as **“structure content for citability + own the canonical on your domain.”**

### 6.2 Structural choices that raise quote odds

From GEO practice literature (not DEV-specific): answer-first TL;DR, question-shaped H2s, explicit versions, tables, code blocks, clear entity names.

**Lab checklist:**

- First 80 words: **direct answer**  
- H2s like “How do we kill a hung agent without data loss?”  
- Version pins: `Claude Code 2.x`, `Postgres 16`, date of benchmark  
- Tables of trade-offs  
- Unique primary data (your failure rates, fleet size, dollar costs)

---

## 7. Risks

### 7.1 Ban / flag risks (from official lists)

Suspension/ban triggers in AI guidelines include: intent to harm/scam, soliciting money as main purpose, SEO backlink farms (with stated exceptions), plagiarism. CoC: harassment, hate, doxxing, etc.

Also: spam automation (staff invest in Gemini spam defense, Jan 2026).

### 7.2 Content-farm reputation / case against DEV

**Case against (steelman):**

1. **Signal-to-noise is degraded** — showdev and latest feeds are full of low-effort “I built X” posts; senior engineers often prefer HN/Lobsters/personal blogs. [emerging]
2. **Audience skew junior** — great for testers at some levels; weak for frontier-lab peer recognition.  
3. **AI slop association** — platform is known for AI content waves since ChatGPT; even good posts get accused.  
4. **You already have distribution** (Habr, TG, X, FB, Medium, GitHub) — another channel dilutes editorial energy.  
5. **Acqui-hire signal** is more likely from **GitHub stars + issue quality + HN hits** than DEV follower counts. [speculative]  
6. **Ownership:** content lives on Forem; mitigate with canonical home blog.

**Case for (kept short):** free discovery, ENG-native audience, org tooling, low barrier, good for OSS launch notes and tester CTAs.

### 7.3 Alternatives comparison

| Platform | Reach | Senior signal | Ownership | Fit for lab |
|----------|-------|---------------|-----------|-------------|
| **dev.to** | High among mid/junior + hobbyists | Medium-low | Low (canonical helps) | EN tutorials, showdev launches |
| **Hashnode** | Mid; better custom domain | Medium | Higher with custom domain | Personal brand + SEO |
| **Personal blog + syndication** | Slow then compounding | Highest if content is elite | Full | **Primary GEO home** |
| **Lobsters** | Low volume, high quality | **Highest** for pure eng | N/A (links) | Deep post-mortems (invite needed) |
| **Hacker News** | Spikey, huge | High (brutal filter) | Link out | 1–2 flagship posts/year |
| **Medium** | Medium | Medium-low | Weak | Already there; don’t over-invest |

**Alongside, not instead:** keep **canonical blog + GitHub**; use DEV as EN mirror; earn Lobsters/HN only on flagship pieces.

---

## Deliverables (actionable)

### A. Mechanics checklist (pipeline-enforceable)

```
[ ] Title: concrete verb + object + (optional) constraint; ≤70 chars preferred
[ ] Opening 3 lines: I/we did X under Y → friction → payoff
[ ] Length: 800–2500 words for deep; 400–800 for tips (not hard rule)
[ ] H2s only under title (no second H1)
[ ] ≤4 tags; at least one high-traffic + one precise
[ ] cover_image 1000x420
[ ] series: set if part of arc
[ ] canonical_url → lab domain original (if cross-post)
[ ] GitHub embed / real code blocks with language tags
[ ] Version numbers + date of measurement
[ ] TL;DR or answer-first block
[ ] “When NOT to” section for architecture claims
[ ] AI disclosure sentence if Mycroft drafted
[ ] CTA: one ask (try repo / file issue / report benchmark) — not sales page
[ ] #showdev only if primary purpose is showing a built artifact
[ ] Author will be online 1–4h post-publish
[ ] No bot comments scheduled
```

### B. Content menu (10 titles, ranked by expected return for *your* goals)

1. **I Gave Our Agents Skip-Locked Postgres Queues and Killed Redis (Here’s When That’s Stupid)** — high return (showdev + architecture + numbers)  
2. **We Run a Fleet of Claude Agents Across 4 Machines. Here’s the Bus That Doesn’t Drop Tasks** — high (unique primary data)  
3. **Post-Mortem: The Night Our Agent Loop Burned $X Because Nobody Owned the Watchdog** — high (genre fit)  
4. **Stop Prompting Multi-Agent Systems Like They’re Chatbots: The 7 Failure Modes We Logged** — high (list + data)  
5. **Show DEV: Open-Source Tooling We Built So Non-Engineers Can Drive LLM Fleets** — high for testers  
6. **Canonical Cross-Posting for Labs: How We Syndicate Without Nuking Our Own SEO** — medium (meta, shares well)  
7. **Dogfooding Our Agent Harness for 30 Days: What Broke First** — medium-high  
8. **The Connect Rule: Why “We Shipped It” Isn’t Done Until Someone Uses It** — medium (philosophy + systems)  
9. **How We Structure Agent Logs So Humans Can Debug Without Opening Five Dashboards** — medium  
10. **What Frontier Labs Might Actually Care About in OSS Tooling (And What We Measure)** — lower on DEV, higher as flagship for HN later  

### C. 90-day plan

| Phase | Cadence | Checkpoints |
|-------|---------|-------------|
| **Days 0–14** | Account + org; comment 5×/week; 2 posts | Profile complete; org CTA → GitHub; first 2 posts live with canonical |
| **Days 15–45** | 2 posts/week; 1 series started | ≥8 posts; avg views tracked; ≥1 post >500 views or 20 reactions |
| **Days 46–90** | 2 posts/week; 1 flagship cross-link to HN *if* earned | ≥16 posts; 100–500 followers (stretch); ≥3 tester conversations attributed; GEO probe run once |
| **Kill criteria** | — | If after 90 days: 0 meaningful GitHub/tester outcomes *and* no post >300 views despite external shares → park cadence at 1/month |

### D. Decision options

| Option | Trade-off | Risk |
|--------|-----------|------|
| **A. Full DEV push (2–4/wk)** | Fast audience learning | Slop association; energy drain |
| **B. DEV as syndication only (1–2/wk, canonical home)** | Best ownership + EN reach | Slightly less “native” feed juice |
| **C. Skip DEV; double down on blog + HN/Lobsters + GitHub** | Max senior signal | Slower EN mid-market reach |
| **D. Hashnode custom domain instead of DEV** | Ownership | Less community circulation than DEV historically |

### E. Recommendation

**Option B — DEV as English syndication + selective native showdev, 2 posts/week, personal face + lab org, strict AI disclosure, canonical to lab site.**  

- **Confidence:** [emerging]  
- **Main assumption:** Your unique multi-agent fleet data will outperform generic AI listicles enough to attract engineer-testers even if follower growth stays modest.  
- **Do not bet acqui-hire narrative on DEV metrics alone** — keep GitHub + flagship posts as the prestige layer.

---

## Sources (dated)

| # | Source | Date | Notes |
|---|--------|------|-------|
| 1 | [DEV Editor Guide](https://dev.to/p/editor_guide) | Live docs (fetched 2026-08) | tags≤4, canonical, cover 1000×420, series, liquid tags |
| 2 | [DEV Code of Conduct](https://dev.to/code-of-conduct) | **2023-07-31** | AI disclosure, citations |
| 3 | [Guidelines for AI-assisted Articles on DEV](https://dev.to/devteam/guidelines-for-ai-assisted-articles-on-dev-17n6) | **2022-12-19** (edited 2023-01) | **Possibly stale** on culture; still CoC-linked |
| 4 | [Top posts this year](https://dev.to/top/year) | Fetched **2026-08-06** | Engagement snapshots |
| 5 | [I Replaced Redis with PostgreSQL…](https://dev.to/polliog/i-replaced-redis-with-postgresql-and-its-faster-4942) | **2026-01** | Voice + comment culture |
| 6 | [You're Not Building Netflix…](https://dev.to/adamthedeveloper/youre-not-building-netflix-stop-coding-like-you-are-1707) | **2025-11** | Opinion voice |
| 7 | [Stop Installing Libraries…](https://dev.to/sylwia-lask/stop-installing-libraries-10-browser-apis-that-already-solve-your-problems-35bi) | **2026-02** | List-native voice |
| 8 | [Best Practices: Formatting](https://dev.to/devteam/best-practices-for-writing-on-dev-formatting-5fnc) | **2023-04-17** | **>12 months — possibly stale** |
| 9 | [Best Practices: Topics](https://dev.to/devteam/best-practices-for-writing-on-dev-topics-1d0j) | **2023-05-17** | **>12 months** |
| 10 | [About #showdev (tag mod)](https://dev.to/cicirello/about-the-showdev-tag-from-one-of-the-showdev-moderators-3beh) | **2023-01-24** | **>12 months** |
| 11 | [Organizations help](https://dev.to/help/organizations) | Live | Org features |
| 12 | [From 0 to 10K Followers…](https://dev.to/web_dev-usman/from-0-to-10k-followers-on-devto-what-worked-what-didnt-5a81) | **2025-02-27** | Cold-start case [single-source] |
| 13 | [When is the best time to post on Dev?](https://dev.to/westernal/when-is-the-best-time-to-post-on-dev-1icb) | **2024-11-21** | [single-source] |
| 14 | [Cross-posting 72-hour rule](https://chudi.dev/blog/devto-cross-posting-automation) | **2026-01** (updated Mar 2026) | Practitioner SEO |
| 15 | [Mike Bifulco: canonical tags](https://mikebifulco.com/posts/own-your-work-with-canonical-tags) | **2024-07-12** | Cross-post SEO |
| 16 | [How does promotion work on DEV?](https://dev.to/grahamthedev/how-does-the-promotion-of-posts-work-on-dev-39c) | **2021-08** | Editorial promotion — **stale** |
| 17 | [Fighting Spam at Scale (Gemini)](https://dev.to/devteam/fighting-spam-at-scale-how-we-use-gemini-to-protect-the-dev-community-277j) | **2026-01-22** | Moderation investment |
| 18 | [Is Dev.to victim of its own success?](https://dev.to/samuelfaure/is-dev-to-victim-of-its-own-success-1ioj) | **2021-07** | Spam/corporate concerns — **stale** |
| 19 | [DEV vs Hashnode vs Medium (2026 comparison)](https://resources.plainenglish.io/in-plain-english-vs-devto-hashnode-medium-and-hackernoon-best-platform-for-reach-in-2026) | **2026-02-17** | Platform comparison |
| 20 | [daily.dev: syndication](https://business.daily.dev/resources/developer-content-syndication-distribute-technical-articles-beyond-blog/) | ~2025–26 | Tags, API, reach claims [low-authority marketing] |
| 21 | [GEO / citation pattern articles](https://discoveredlabs.com/blog/ai-citation-patterns-how-chatgpt-claude-and-perplexity-choose-sources) | **2025-12** | LLM citability [emerging] |
| 22 | [Lobsters vs HN](https://daily.dev/blog/lobsters-vs-hacker-news-invite-only-alternative/) | **2026-08** | Senior-signal comparison |
| 23 | [Best developer communities 2026](https://medium.com/@NimrodKramer/best-developer-communities-in-2026-where-engineers-actually-talk-dc8143ee2fc3) | ~2026 | DEV “surface-level” critique [single-source] |
| 24 | [HitSubscribe marketer guide to DEV](https://hitsubscribe.com/dev-to-guide-for-marketers/) | **2020-08** | Personal>brand — **stale** |

**Claims explicitly without sources:**  
- Exact median views for new accounts in 2026 — **[no source found]**  
- DEV de-ranks cross-posts in *its own* feed — **[no source found]**  
- Cover image CTR lift % on DEV — **[no source found]**  
- DEV’s share of ChatGPT/Perplexity citations — **[no source found]**

---

**Bottom line for the lab:** Use DEV as a **loud English workshop floor** for build-in-public multi-agent tooling — first-person, measured, disclosed, canonically owned — not as the cathedral of reputation. The cathedral remains **your domain + GitHub + the rare HN/Lobsters hit**.
