# DR26-08-06-ZB-01-0934 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-06-ZB-01-0934
rail: codex
vendor: OpenAI (Codex CLI, node direct)
door: cli-headless (node direct)
collected: 2026-08-06T13:21:36Z
bytes: 53014
urls: 52
secs: 1327
exit: 0
---

# 1. Verdict

[emerging] DEV is worth a 90-day experiment, but as a community-and-syndication layer—not the lab’s canonical archive, not a volume SEO farm, and not the main hiring signal. Its 2026 feed combines follows, quality, freshness, engagement, human moderation, and newly added semantic embeddings, which gives a zero-follower author some discovery surface but no guaranteed distribution ([DEV Team, May 22, 2026](https://dev.to/devteam/how-were-using-gemini-embeddings-to-build-a-smarter-community-driven-feed-on-dev-1b9f)). The winning posture is two evidence-heavy posts per week, published under Anton’s personal byline and attributed to a Palo Alto AI Research Lab organization: real repositories, reproducible tests, exact failures, limitations, and human replies. Expect most posts to be modest; optimize for 2–5 engineer-testers and a reusable, citable English corpus—not viral reactions. For GEO, DEV is genuinely cited by some AI engines, but it is weaker than Medium in the best reproducible 2026 dataset and produced no OpenAI citations in that particular sample; therefore, an owned lab blog plus full DEV syndication with `canonical_url` is the highest-value architecture.

## Evidence limits

DEV exposes reactions and comments publicly, but article views and follower conversion are generally author-side analytics. There is no representative public dataset establishing typical 90-day views, causal cover-image uplift, an exact feed formula, an optimum posting hour, or personal-versus-organization performance. Where those questions lack evidence, I give explicit planning hypotheses rather than pretend they are platform facts.

# 2. Voice spec

## What successful DEV writing sounds like

[emerging] I analysed five posts returned in DEV’s high-engagement annual surface and the official API snapshot on August 6, 2026. This is a purposive sample, not a controlled study.

| Named post | Engagement snapshot | Approximate voice |
|---|---:|---|
| [“Stop Installing Libraries: 10 Browser APIs…”](https://dev.to/sylwia-lask/stop-installing-libraries-10-browser-apis-that-already-solve-your-problems-35bi), Feb. 4, 2026 | 265 reactions, 92 comments | ~9.5 words/sentence; short, practical, friendly; “you” substantially more than “we” |
| [“You’re Not Building Netflix: Stop Coding Like You Are”](https://dev.to/adamthedeveloper/youre-not-building-netflix-stop-coding-like-you-are-1707), Nov. 23, 2025 | 345 reactions, 104 comments | ~14 words/sentence; provocative second-person opening, joke, then personal engineering evidence |
| [“I Replaced Redis with PostgreSQL (And It’s Faster)”](https://dev.to/polliog/i-replaced-redis-with-postgresql-and-its-faster-4942), Jan. 9, 2026 | 337 reactions, 46 comments | ~18 words/sentence; concrete decision, benchmark, costs, three-month results, explicit “not for everyone” boundary |
| [“I built an app in every frontend framework”](https://dev.to/lissy93/i-built-an-app-in-every-frontend-framework-4a9g), Jan. 5, 2026 | 249 reactions, 57 comments | ~22 words/sentence; first-person experiment, source and benchmarks, opinionated pros/cons |
| [“The Real Skill in Programming Is Debugging…”](https://dev.to/sylwia-lask/the-real-skill-in-programming-is-debugging-everything-else-is-copy-paste-i39), Mar. 5, 2026 | 246 reactions, 133 comments | ~10 words/sentence; personal thesis, career story, pop-culture humour, direct invitation to disagree |

Across those five bodies, my approximate text calculation produced 4,419 words in 294 sentences—about 15 words per sentence. “I” and “you” occurred roughly eleven times as often as “we.” [emerging] The platform-native default is therefore:

- First-person singular for observations: “I ran,” “I expected,” “I was wrong.”
- “We” only for things the lab actually did together.
- Second person for the transferable lesson, not for scolding.
- Past tense for the experiment; present tense for what the result means.
- Contractions and short paragraphs.
- One light joke or self-own, then back to engineering.
- A receipt—benchmark, stack trace, screenshot, commit, or code—within the first screen.

The strongest openings compress three moves into two or three lines:

1. A concrete event or result.
2. A contradiction or cost.
3. A promise of evidence.

“I replaced Redis” works because the title and opening contain a disputed decision plus a falsifiable result. “You’re not building Netflix” works because the provocation is followed by experience rather than motivational filler.

## Copyable style guide

[emerging]

Do:

- Lead with the failure, number, or surprising decision.
- Name the actual tool, version, workload, and evaluation date.
- Use “I” for judgment and “we” for documented team actions.
- Alternate short verdicts with slightly longer explanation.
- Show the code or log that changed your mind.
- Include “where this does not work.”
- End with one genuine technical question.
- Make every title legible without knowing the lab or product.
- Sound like an engineer showing another engineer the workbench.

Don’t:

- Open with “We are thrilled to announce…”
- Call ordinary automation “revolutionary.”
- Use a generic history-of-AI introduction.
- Publish an external-link stub; DEV’s terms require substantial on-platform content ([DEV Terms, accessed Aug. 6, 2026](https://dev.to/terms)).
- Invent benchmark precision or hide the test environment.
- Convert every paragraph into bullets.
- Use emojis as structural punctuation.
- attach four fashionable tags to unrelated content.
- Ask for stars, sign-ups, follows, and demos in the same post.
- make a claim that an AI drafted but no human can defend in the comments.

## Three openings for the same article

**Bad**

> We are thrilled to announce our revolutionary multi-agent quality assurance framework, which leverages cutting-edge AI to transform software development and improve productivity.

**Acceptable**

> We built a validator after our coding agents repeatedly reported that broken changes had passed. It runs the actual test suite, checks the diff, and records the evidence before an agent can say “done.”

**Native DEV**

> Our agent said the fix passed. CI disagreed 43 seconds later.  
>   
> We gave the same bug to 12 agents: seven declared victory, three edited the wrong file, and two actually fixed it. Here’s the harness, the failure log, and the guardrail we added.

## What repels the community

[established] DEV trusted members are instructed to down-rank posts that are factually wrong, poorly written, dishonest, or stealth marketing, and to flag spam and plagiarism. Negative moderator signals are private, so outsiders cannot honestly produce a public “most downvoted” list ([Trusted Member Guide, accessed Aug. 6, 2026](https://dev.to/trusted-member)). Exact downvote counts are therefore `[no source found]`.

The observable failure modes are:

- **Mass-produced, zero-history posting.** DEV’s January 2026 moderation description says it evaluates quality, authenticity, promotion, automated-generation signals, and user context such as account age, article count, badges, and comments. A brand-new account with no history receives less benefit of the doubt ([DEV Team, Jan. 22, 2026](https://dev.to/devteam/fighting-spam-at-scale-how-we-use-gemini-to-protect-the-dev-community-277j)). [established][single-source]
- **AI-polished prose without disclosure.** The current official guideline permits AI-assisted writing only with disclosure, fact-checking, and human subject understanding; AI-generated comments are prohibited. The page was last updated April 8, 2024, so it is formally current but evidentially stale ([DEV AI guidelines, Apr. 8, 2024 — ⚠ stale](https://dev.to/guidelines-for-ai-assisted-articles-on-dev/)). [established][single-source]
- **Generic AI voice.** A July 2026 DEV article about burnout explicitly described low-effort, barely reviewed generated content as exhausting and disposable; it attracted more than 150 comments, showing that this is not a marginal irritation ([Klaudia Grzondziel, July 9, 2026](https://dev.to/klaudiagrz/should-i-quit-it-or-just-live-through-the-burnout-1gng)). [emerging][single-source]
- **Flooding.** One author’s uncontrolled five-posts-in-24-hours experiment earned only 11 total views. That does not prove a per-author reach quota, but it demonstrates that volume alone can fail dramatically ([May 22, 2026](https://dev.to/kyb8801/24h-after-my-5-posts-per-day-experiment-on-devto-11-views-total-the-platform-was-right-3abk)). [speculative][low-authority][single-source]
- **Press releases wearing tutorial tags.** DEV’s organization guidance tells companies to publish full posts, remain authentic and straightforward, and avoid overly promotional content ([DEV organization guidance, accessed Aug. 6, 2026](https://dev.to/help/organizations/maximizing-your-dev-organization)). [established][single-source]
- **AI-looking banners plus AI-looking copy.** In a June 2026 moderation discussion, a trusted moderator disclosed that he had been using DEV’s Sloan workflow to request AI disclosure and said AI-generated banners increased his suspicion. Other commenters defended technically accurate AI-polished writing, so community opinion is genuinely divided ([“Has Sloan Flagged Your Article Lately?”, June 15, 2026](https://dev.to/dannwaneri/has-sloan-flagged-your-article-lately-1gmh)). [emerging][single-source]

### Does disclosure help or hurt?

[emerging] It helps policy compliance and gives the author a defensible trust position. It may lose readers who reject all AI-authored prose, but no DEV-specific A/B test was found: `[no source found]`.

Use a precise end note, not a defensive banner:

> Disclosure: an AI collaborator helped cluster the run logs and draft the first outline. Anton selected the claims, reran the commands on July 31, 2026, checked every linked source, and rewrote the conclusions.

That is stronger than either “100% human” theatre or an ambiguous “AI-assisted.”

## Are post-mortems rewarded?

[emerging] The genre is not automatically rewarded on DEV.

Three recent literal post-mortem/failure articles had nearly no engagement when checked:

- [“The 5 Things We Broke Building Our First Major ML Pipeline…”](https://dev.to/best_techcompany_200e9f2/the-5-things-we-broke-building-our-first-major-ml-pipeline-at-besttech-and-how-we-fixed-them-c96), Jan. 5, 2026: zero reactions/comments.
- [“We Broke Production Due to an API Change…”](https://dev.to/specshield_a17bcb1ca84675/we-broke-production-due-to-an-api-change-heres-how-i-fixed-it-42en), Apr. 16, 2026: zero reactions/comments.
- [“The Same Root Cause Keeps Coming Back…”](https://dev.to/_06a3df6b50aec966668fb/the-same-root-cause-keeps-coming-back-because-nobody-tracks-it-i-built-a-zero-dep-cli-that-does-3i34), July 3, 2026: one comment, zero reactions.

By contrast, the Redis-to-Postgres transformation combined a failure/cost story with a broad, useful decision and reached 337 reactions. On Hacker News, consequential incident reports can be enormous: Cloudflare’s November 2025 outage post-mortem reached 1,465 points and 916 comments ([HN thread, Nov. 18, 2025](https://news.ycombinator.com/item?id <tg-id>)). [emerging][single-source]

The conclusion: on DEV, “we broke production” is not itself a hook. Reframe it as:

> I broke X under Y conditions. Here is the failing test, the root cause, the guardrail, and when you should use it.

That supplies personal stakes plus reusable education—the DEV combination.

## Same topic, five native voices

Hypothetical topic: twelve coding agents attempted the same bug.

| Platform | Native rendering |
|---|---|
| **DEV** | “I Gave 12 Coding Agents the Same Bug. Seven Lied About Passing Tests.” First-person experiment, compact humour, code/logs, approachable explanation, open question. |
| **Hacker News** | “Failure modes in 12 coding agents on the same repository.” Restrained original title, full methodology, no emoji, no engagement CTA. HN asks for original titles and discourages promotional behaviour ([HN Guidelines, accessed Aug. 6, 2026](https://news.ycombinator.com/newsguidelines.html)). |
| **Medium** | “What Twelve Coding Agents Taught Me About Trust.” More narrative arc, polished thesis, fewer implementation details. Medium’s current distribution guidance prioritizes a compelling reason for this writer to tell the story and rejects sensational framing ([Medium, updated June 29, 2026](https://help.medium.com/hc/en-us/articles/360006362473-Medium-s-Distribution-Guidelines-How-curators-review-stories-for-Boost-General-and-Network-Distribution)). |
| **Reddit r/programming** | Use the article’s descriptive title verbatim, disclose authorship, no product CTA, and expect methodology scrutiny. The subreddit bans LLM-generated submissions and in May 2026 was actively debating whether to exclude even human-written LLM topics ([moderator discussion, May 5, 2026](https://www.reddit.com/r/programming/comments/1t4odyl/looking_for_feedback_on_ai_content_in/)). |
| **Habr** | A longer Russian technical case: architecture, environment, failure taxonomy, tables, screenshots, and reproducibility. Habr requires substantive narrative and a clear topical title and limits profile-as-company-ad behaviour ([Habr rules, accessed Aug. 6, 2026](https://habr.com/en/docs/help/rules/)). |

This comparison is an editorial inference from rules and observed posts, not a controlled language study. [emerging]

# 3. Mechanics checklist

## Enforceable pre-publish checklist

| Check | Pipeline rule | Evidence/confidence |
|---|---|---|
| Ownership | Confirm the lab owns the text, images and code excerpts. | DEV can remove copyright/plagiarism violations ([Terms](https://dev.to/terms)). [established] |
| Canonical | Choose exactly one source of truth. If the owned blog is primary, set `canonical_url` to that exact article URL. | DEV supports `canonical_url` and RSS imports ([DEV writing help](https://dev.to/help/writing-editing-scheduling)). [established] |
| Full text | Publish a complete article, not “read the rest here.” | Substantial on-platform content is required by DEV’s terms. [established] |
| Title | Include one concrete object and one result/tension. No “revolutionary,” “ultimate,” or unsupported percentage. | High-performer sample; house rule. [emerging] |
| Opening | In the first 80 words: event/result, why it mattered, and the evidence readers will see. | High-performer sample. [emerging] |
| Length | Default 800–1,800 words; exceed it only when the technical receipt requires it. | House rule; `[no source found]` for a universal optimum. |
| Tags | One broad audience tag, one problem tag, one implementation tag, one optional format tag. Maximum four. | DEV editor limit is four ([Editor Guide](https://dev.to/p/editor_guide)). [established] |
| Cover | Exactly 1000×420; readable on mobile; descriptive alt text; no generic humanoid robot art. | Official dimension is 1000×420 ([Editor Guide](https://dev.to/p/editor_guide)); style is a house rule. |
| Headings | Start body hierarchy at H2. Use descriptive or genuinely question-shaped headings. | DEV editor guidance. [established] |
| Reproducibility | State OS/runtime/model/library versions, workload, test date, sample size and exclusions. | Editorial/GEO house rule. |
| Code | Runnable or explicitly pseudocode; link to the exact commit/tag. | House rule. |
| Evidence | Include at least one failure log, benchmark table, diff, trace or screenshot. | High-performer pattern. [emerging] |
| Limits | Add “Where this fails” or “What this does not prove.” | House rule. |
| Embeds | Prefer GitHub repo/issue/Gist or runnable StackBlitz/CodePen. Include a plain link fallback. | Supported officially; engagement benefit is `[speculative]`. |
| Series | Each entry must work alone. Do not create a series until at least a second article is certain. | DEV hides series chrome until entry two ([writing help](https://dev.to/help/writing-editing-scheduling)). [established] |
| AI disclosure | Name what AI did, what the human verified, and the verification date. | Official AI guideline; current page last updated 2024. [established] |
| Promotion | One restrained CTA after the conclusion; no CTA cluster. | DEV terms and organization guidance. [established] |
| Comments | Human replies only; target first substantive response within 24 hours. | AI comments are prohibited; 24 hours is an internal SLA, not a platform rule. |
| Final QA | Open the published page and verify cover crop, embeds, code, canonical HTML, links, tags and disclosure. | House rule. |

## Tags: what the available data says

DEV permits four tags, and moderators can add or remove tags they moderate; an author cannot simply restore a moderator-removed tag ([Tag Moderation, accessed Aug. 6, 2026](https://dev.to/tag-moderation)). [established][single-source]

No public tag-level view counts were found. The following is an engagement-ceiling proxy: median reactions among the first 100 posts returned by the official API for each tag with `top=365`, captured August 6, 2026. Articles overlap across tags; this is not total traffic.

| Tag | Median reactions in top-returned sample |
|---|---:|
| `webdev` | 107 |
| `ai` | 95 |
| `programming` | 82 |
| `opensource` | 42 |
| `agents` | 35 |
| `showdev` | 31 |
| `llm` | 21 |
| `mcp` | 21 |
| `python` | 16 |
| `devtools` | 2 |

[emerging][single-source] Source: [Forem API documentation](https://developers.forem.com/api/v1) and the public articles endpoint, snapshot Aug. 6, 2026.

Practical combinations:

- Agent benchmark: `#ai #agents #opensource #testing`
- MCP implementation: `#ai #mcp #opensource #python`
- Genuine project release: `#showdev #opensource #agents #devtools`
- Production failure: `#debugging #ai #testing #programming`

Do not automatically attach `#showdev` to every lab article.

## `#showdev`: exact promotion boundary

[established] The official terms prohibit posts designed primarily for promotion, backlink accumulation, or off-site conversion. Full technical content and an incidental relevant link are allowed ([DEV Terms](https://dev.to/terms)).

The best available `#showdev` interpretation comes from a tag moderator: `#tutorial` teaches “how,” while `#showdev` shows “what” someone built; a post may use both when it genuinely does both. The moderator emphasizes community-driven, non-corporate presentation ([Vincent Cicirello, Jan. 24, 2023 — ⚠ stale](https://dev.to/cicirello/about-the-showdev-tag-from-one-of-the-showdev-moderators-3beh)). [single-source]

Acceptable:

- The complete technical story is on DEV.
- The repository is open and directly relevant.
- The post shows architecture, unexpected failures and limitations.
- The product link appears after readers already received value.
- The author is present in comments.

Spam-shaped:

- Release notes every few days.
- “We launched X” followed by feature bullets.
- A demo and wait-list link with no implementation detail.
- Near-identical posts under several accounts.
- Four broad tags unrelated to the implementation.
- An AI-written comparison engineered to rank for competitor names.

## Canonical URLs and cross-post direction

[established] Setting `canonical_url` leaves the full DEV article accessible while placing the preferred/original URL in its canonical metadata. DEV explicitly encourages RSS imports and cross-posting ([DEV help](https://dev.to/help/writing-editing-scheduling)).

DEV claims this prevents reposting penalties and boosts the original. Google’s wording is more cautious: canonical annotations are strong signals, not commands, and Google may select another canonical ([Google, updated July 10, 2026](https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls)). Google has also said that when publishers require a syndicated copy to stay out of search, `noindex` is more reliable than canonical—advice from June 2023 and therefore stale, but still an important contradiction ([Google Search Office Hours, June 2023 — ⚠ stale](https://developers.google.com/search/help/office-hours/2023/june)). Bing similarly recommends consolidating duplicate signals for search and AI visibility ([Bing, Dec. 19, 2025](https://blogs.bing.com/webmaster/December-2025/Does-Duplicate-Content-Hurt-SEO-and-AI-Search-Visibility)).

No DEV statement says cross-posts are internally de-ranked. In my top-100 annual API snapshot, five posts had external canonical URLs, including highly engaged entries such as [“Anyone Can Commit Code as You on GitHub”](https://dev.to/yuyatakeyama/anyone-can-commit-code-as-you-on-github-4eb1). That disproves a categorical exclusion but cannot rule out a smaller ranking penalty. [emerging][single-source]

Recommended direction:

1. Publish on `наш сайт` or another durable owned English domain.
2. Publish the full adapted article on DEV with canonical pointing to the owned article.
3. Import that same original into Medium; Medium’s import tool sets the original canonical and backdates the story ([Medium help, accessed Aug. 6, 2026](https://help.medium.com/hc/en-us/articles/214550207-Importing-a-post-to-Medium)).
4. Keep the GitHub README focused on installation, evidence and usage; link to the article rather than duplicating it.
5. If no durable owned site exists, make DEV the canonical and import from DEV into Medium.

## Series

[established] A DEV series receives a series title, page and table of contents, but these appear only after entry two ([DEV writing help](https://dev.to/help/writing-editing-scheduling)).

[emerging][single-source] A successful 2026 example also illustrates decay:

- [“An LLM API Call, in 4 GIFs”](https://dev.to/jasmin/an-llm-api-call-in-4-gifs-33b1), May 26: 111 reactions/65 comments.
- [Part two](https://dev.to/jasmin/streaming-an-llm-response-in-4-gifs-16dh): 35/14.
- [Part three](https://dev.to/jasmin/the-messages-array-in-4-gifs-1k1j): 16/18.

Series navigation did not make later instalments compound automatically. Use at most three parts initially, give each an independent promise, and publish part two only if part one demonstrates demand.

## Embeds

DEV supports GitHub repositories/issues/Gists, CodePen, CodeSandbox, StackBlitz, YouTube, Loom, Reddit, Stack Overflow and many other embeds ([Editor Guide](https://dev.to/p/editor_guide)). [established][single-source]

No credible causal comparison of embed types was found: `[no source found]`.

Best practical order:

1. GitHub issue or repository card for the artifact.
2. Runnable StackBlitz/CodePen for browser code.
3. A short video only when motion or timing is essential.
4. Plain fallback links because embeds can break or be blocked.
5. Never make a video the only place containing the result.

## Covers

The recommended size is 1000×420. In the August 6 API snapshot, 100/100 top annual posts had covers, while 148/363 fresh posts—40.8%—had them. That is a strong association with selection, not proof of click-through causality. [emerging][single-source]

A measurable causal CTR uplift is `[no source found]`.

Use:

- A real chart, trace, terminal fragment or architecture object.
- Four to eight words maximum.
- Strong contrast and a mobile crop.
- A consistent lab mark, small enough not to resemble an ad.

Avoid generic AI faces, glowing brains, robots typing, or fake dashboards.

## Timing and cadence

DEV scheduling uses the author’s local timezone ([DEV writing help](https://dev.to/help/writing-editing-scheduling)). [established][single-source]

In the top-100 annual snapshot, 87 posts were published Monday–Thursday: Wednesday 26, Thursday 25, Monday 20, Tuesday 16. Friday had six, Sunday seven and Saturday zero. There is no base-rate control, so this does not prove those days cause reach. [speculative][single-source]

Start with Tuesday and Thursday at 14:00–17:00 UTC, rotate one slot monthly, and compare first-24-hour views and meaningful comments. The time recommendation is a test hypothesis: `[no source found]`.

DEV awards 4-, 8- and 16-week writing-streak badges, but no current source says regular cadence itself receives an algorithmic multiplier ([DEV engagement help](https://dev.to/help/reacting-commenting-engaging)). [established] Consistency helps readers and production discipline; it is not a proven feed hack.

## Reactions, Top lists and newsletter

Current article pages expose five positive reactions—Love, Unicorn, Wow/Exploding Head, Well Done/Raised Hands and Hot Take/Fire—plus a separate Save action ([DEV engagement help](https://dev.to/help/reacting-commenting-engaging)). [established][single-source]

The exact weight of each reaction, save or comment is not published: `[no source found]`.

[emerging] The May 2026 feed description says ranking now combines semantic similarity with follows, quality, time decay and social signals; human curation remains important ([DEV Team, May 22, 2026](https://dev.to/devteam/how-were-using-gemini-embeddings-to-build-a-smarter-community-driven-feed-on-dev-1b9f)). A trusted-user upvote can improve ranking and a trusted-user downvote can deprioritize it ([Trusted Member Guide](https://dev.to/trusted-member)).

The weekly Top 7 is not simply “seven most reacted posts.” DEV says it combines editorial feedback, traffic and engagement; the current cycle runs Saturday through Friday, and authors must not self-apply `#top7` ([Top 7 page, accessed Aug. 6, 2026](https://dev.to/t/top7)). [established][single-source]

## Comment norms

No official author-response deadline exists: `[no source found]`.

The named high performers commonly replied, accepted corrections, asked follow-up questions and expanded on limitations. A practical SLA is:

- Acknowledge reproducible errors immediately.
- Give substantive replies within 24 hours.
- Thank the correction, rerun the test, edit the post and note the edit.
- Disagree with evidence, not status.
- Do not let an AI persona generate comments; DEV’s policy prohibits AI-generated comments.
- Do not hide technically inconvenient disagreement unless it violates conduct rules.

## Founder profile or organization?

DEV provides free organization accounts for companies, open-source projects and groups, with branding, analytics, followers and team publishing. Setup begins with a real personal account, which then creates or joins the organization ([organization setup, accessed Aug. 6, 2026](https://dev.to/organizations/setup-guide)). [established][single-source]

There is no reliable personal-versus-organization reach comparison: `[no source found]`.

Recommended structure:

- Author: Anton Dziatkovskii.
- Organization: Palo Alto AI Research Lab.
- Bio: plain-language description plus GitHub.
- Article voice: “I” for Anton’s observation; “we” for documented lab actions.
- No faceless corporate byline when a person can defend the work.

# 4. Content menu

Ranked by expected return for DEV’s mix of practical engineering, first-person experimentation and discussion.

| Rank | Working title | Why it fits |
|---:|---|---|
| 1 | **I Gave 12 Coding Agents the Same Bug. Seven Said the Tests Passed.** | Benchmark, conflict, falsifiable evidence and multi-agent relevance. |
| 2 | **Our Agent Said “Done.” The Repository Was Still Broken.** | Accessible failure story leading to a reusable verification gate. |
| 3 | **We Let Two LLMs Review Each Other’s Code for 30 Days. What Actually Improved?** | Strong experiment; requires real baseline and failure taxonomy. |
| 4 | **Your Coding Agent Can Write Code. Can It Prove the Fix Works?** | Broad reader problem; natural tutorial plus open-source harness. |
| 5 | **I Cut a 40,000-Token Agent Handoff to 900 Tokens—Without Losing the Fix** | Concrete before/after; disclose corpus and evaluation criteria. |
| 6 | **One Bug, Four Agents, Four Root Causes. Here’s the Trace.** | Native first-person debugging story with logs and disagreement. |
| 7 | **We Replaced a 24/7 AI Agent With 200 Lines of Deterministic Code** | Counter-hype result likely to attract sceptical senior engineers. |
| 8 | **The Hidden Failure Mode in Multi-Agent Systems: Everyone Says “Done”** | Strong thesis, but must contain actual incident data to avoid generic AI prose. |
| 9 | **I Turned Every Agent Run Into a Reproducible Incident Report** | Useful operational pattern and potential open-source release. |
| 10 | **Show DEV: An Open-Source Flight Recorder for Coding Agents** | Best explicit `#showdev` item; show source, demo, storage format and limitations. |

Recommended first four publication order: 2, 7, 1, 5. It establishes practical scepticism before the larger agent benchmark, distancing the lab from generic “agents will change everything” content.

# 5. Ninety-day plan

## What a cold start actually looks like

The evidence is thin and highly variable:

- One new author reported fewer than 25 views, zero reactions, zero comments and zero followers three days after a first post ([May 11, 2026](https://dev.to/tokidigital/my-first-devto-post-got-25-views-3-things-im-changing-3jg2)). [speculative][low-authority][single-source]
- Another author reported 983 views, 25 reactions and zero comments across 16 articles—61 views per article—by October 2025 ([Oct. 22, 2025](https://dev.to/gnomeman4201/built-a-free-analytics-tool-for-devto-because-the-dashboard-wasnt-enough-18a3)). [speculative][low-authority][single-source]
- A May 2026 account reported 184 weekly readers across 13 articles but only two DEV-internal views and no new followers; 152 visits came from external referrals ([May 22, 2026](https://dev.to/foxck016077/day-14-184-reader-321-avg-read-time-0-new-followers-0-sales-1457)). [speculative][low-authority][single-source]
- A 2026 analysis of 1,600 **featured** 2025 articles found averages of 34 reactions and 12 comments, but featured selection makes this unsuitable as a typical baseline ([Jan. 11, 2026](https://dev.to/andrey_andrade_7a534cfc95/i-scraped-1600-devto-articles-from-2025-and-built-an-infographic-3paf)). [speculative][low-authority][single-source]

Accordingly, no defensible public “typical 90 days” figure exists: `[no source found]`.

For planning—not as a claimed platform average—use these hypotheses for 24 good posts:

| Outcome after 90 days | Cumulative views | Reactions | Followers | Engineer outcomes |
|---|---:|---:|---:|---|
| Floor / not working | 500–1,500 | 10–40 | 0–25 | No substantive tester |
| Base case | 1,500–6,000 | 40–180 | 25–125 | 1–3 useful testers/issues |
| Strong | 6,000–20,000 | 180–500 | 125–400 | 3–10 testers, citations or contributors |
| Breakout | Above 20,000 | Above 500 | Above 400 | One or more posts escape DEV into newsletters/HN/social |

[speculative] These are decision bands built from the anecdotes and API engagement distribution, not measured population percentiles.

## Days 0–30: establish trust

Publish eight posts, Tuesday and Thursday.

- Week 1: complete founder profile and organization; follow relevant tags and 15–25 credible authors.
- Leave three to five substantive comments before the first article. This is community participation, not engagement bait.
- Posts 1–2: false “done” report and deterministic-code replacement.
- Posts 3–4: token handoff and exact debugging trace.
- Posts 5–8: two tutorials, one benchmark preview, one genuine `#showdev`.
- Reply personally within 24 hours.
- Share selectively from Anton’s existing channels, but do not manufacture reactions.

Day-30 checkpoint:

- Eight posts live.
- At least two substantive non-author comments across the corpus.
- At least one GitHub issue, reproduction attempt or concrete technical objection.
- Record per-post: 24-hour/7-day views, reactions, saves, comments, follower delta, GitHub referrals and issue creation.
- No target is allowed to be “number of posts” alone.

## Days 31–60: find the repeatable format

Publish another eight posts.

- Rank the first eight by 7-day views, meaningful comments, saves, GitHub actions and follower conversion.
- Continue only the two highest-performing content archetypes.
- Launch a series only if its first proposed entry is already in the top quartile of the account’s posts.
- Perform one direct comparison experiment: same evidence density, different title/opening.
- Pitch nothing to the newsletter; Top 7 is editorial.
- Update old posts when comments reveal a real error.

Day-60 checkpoint:

- Sixteen posts.
- Median seven-day views at least 25% above the first four-post baseline.
- Three or more substantive outside commenters.
- At least one repeat commenter.
- At least one repository action attributable to DEV.
- If no external technical action exists, reduce cadence rather than increase it.

## Days 61–90: publish the pillar result

Publish seven ordinary posts plus one flagship benchmark.

- Run and publish the twelve-agent experiment with a frozen harness and raw results.
- Release the harness or dataset on GitHub.
- Write a standalone methodology post only if readers request it.
- Adapt the flagship for Hacker News with a restrained title.
- Cross-link the best three DEV posts with internal cards.
- Publish a 90-day transparent retrospective with numbers—including failures.

Day-90 go/no-go:

Continue at two posts per week if at least two of these are true:

- 50+ followers.
- Three posts with 10+ reactions.
- Five substantive engineer comments.
- Two external reproduction attempts, issues or pull requests.
- One verified AI citation for a target technical query.
- Median seven-day views improved at least 50% from posts 1–4 to posts 21–24.

These are lab success thresholds, not DEV norms.

## What accelerates cold start

[emerging]

1. **Real participation before publishing.** DEV explicitly advises organizations to comment, follow and interact genuinely; moderation also examines account history ([organization guidance](https://dev.to/help/organizations/maximizing-your-dev-organization), [spam system](https://dev.to/devteam/fighting-spam-at-scale-how-we-use-gemini-to-protect-the-dev-community-277j)).
2. **Relevant broad+niche tags.** Tags route the post into followed-topic feeds; semantic matching now adds another discovery path.
3. **A strong artifact outside DEV.** GitHub gives readers something to test and gives the article primary evidence.
4. **External referral traffic.** It guarantees visits, but whether referral traffic itself changes feed ranking is `[no source found]`.
5. **Author replies.** They turn a post into a technical discussion; no causal ranking coefficient is public.
6. **Newsletter selection.** Potentially large, but editorial and not controllable.
7. **Series only after demand.** The real series example showed substantial part-to-part decay.
8. **Quality consistency, not volume.** No current evidence shows that posting four times weekly receives a cadence bonus.

# 6. GEO and LLM citability

## Is DEV actually cited?

[emerging][single-source] Yes—by some engines, in at least one reproducible 2026 dataset.

OpenAttribution’s May 2026 study ran 330 prompts through production web-search/grounding APIs for OpenAI, Anthropic, Gemini and Perplexity and released the citation rows. My hostname count from its public CSV produced:

| Domain | Citations | Unique URLs | Provider split |
|---|---:|---:|---|
| `medium.com` | 129 | 80 | Gemini 56, Anthropic 73 |
| `dev.to` | 69 | 23 | Gemini 16, Perplexity 26, Anthropic 27 |
| `*.github.com` | 61 | 44 | OpenAI 33, Anthropic 8, Gemini 10, Perplexity 10 |
| `habr.com` | 1 | 1 | Anthropic 1 |
| `hashnode.com` | 1 | 1 | Anthropic 1 |

Sources: [OpenAttribution report, May 2026](https://openattribution.org/research/citation-compliance-may-2026) and its [public citation CSV](https://raw.githubusercontent.com/openattribution-org/policycheck/main/research/data/enriched_citations_20260515_115259.csv).

Limits:

- These were API products, not necessarily the consumer ChatGPT/Claude interfaces.
- The prompt set was category-specific.
- A zero does not prove an engine can never cite the domain.
- GitHub URLs include repositories, issues and other pages; README-only performance cannot be isolated: `[no source found]`.
- Personal blogs are split across thousands of hostnames, so no fair aggregate comparison was possible.
- Gemini grounding is not identical to Google AI Overviews.
- No DEV-specific Google AI Overview dataset was found: `[no source found]`.

The honest answer by engine is therefore:

- **Claude:** direct positive evidence.
- **Perplexity:** direct positive evidence.
- **Gemini grounding:** direct positive evidence.
- **ChatGPT/OpenAI web search:** zero DEV citations in this sample; broader proof not found.
- **Google AI Overviews:** no platform-specific proof found.

Medium had nearly twice DEV’s citations in this sample. GitHub reached all four providers and is the more important proof surface. Habr and Hashnode were almost absent, although the query mix could disadvantage them.

## What structure raises citation odds?

[established] Google says there is no special AI schema or secret AI-only optimization; normal crawlability, indexability and people-first content remain foundational ([Google AI search guidance, updated July 2026](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)).

[emerging] Bing’s AI Performance guidance recommends descriptive headings, concise self-contained sections, tables and FAQ-style answers where appropriate because those structures make content easier to interpret and ground ([Bing AI Performance, accessed Aug. 6, 2026](https://www.bing.com/webmasters/help/ai-performance-9f8e7d6c)).

[emerging] A May 2026 controlled study ran 252,000 paired trials across six LLMs and found topical relevance and position in the provided context were the strongest drivers of first citation. Recent timestamps and explicit facts helped; formatting-only changes had relatively little effect ([“What Gets Cited,” May 25, 2026](https://arxiv.org/abs/2605.25517)). [single-source]

[established but stale] The foundational KDD GEO study found that adding credible citations, statistics and quotations could raise its visibility metric by up to roughly 40%, but the documents were already present in a fixed retrieval context; it did not prove improved organic discovery ([Aggarwal et al., 2024 — ⚠ stale](https://arxiv.org/abs/2311.09735)).

A July 2026 critical survey of 45 GEO studies makes the key correction: existing evidence supports improving how already-retrieved content is used or cited, but does not establish stable, longitudinal, cross-platform improvements in organic discoverability or downstream traffic ([Martinez, July 15, 2026](https://arxiv.org/abs/2607.14035)). [emerging][single-source]

Therefore the article-level GEO rules should be:

- Put the direct answer and primary result in the first 100 words.
- Use descriptive H2s; question-shaped headings only where readers genuinely ask that question.
- State exact software/model versions and test dates.
- Include compact benchmark and decision tables.
- Cite primary documentation, papers and raw datasets.
- Name the author and verification responsibility.
- Separate observation, inference and recommendation.
- Include code blocks that reproduce the result.
- Link the exact repository tag/commit and raw output.
- Update the visible “tested on” date when rerunning.
- Keep one canonical URL.
- Do not repeat keywords unnaturally.
- Do not add FAQ/schema merely for AI; Google says special AI markup is unnecessary.

No study establishes a specific citation lift for TL;DRs, question H2s, code blocks, or version numbers individually: `[no source found]`. They remain strong usability and grounding choices, not magic ranking levers.

## How to measure GEO instead of believing it

Maintain a fixed 20-query panel, for example:

- “How do coding agents prove a repository-level fix?”
- “multi-agent false completion detection”
- “coding agent verification harness”
- “deterministic guardrails for Claude Code”
- “LLM code review disagreement benchmark”

Monthly:

1. Run each query twice across available ChatGPT, Claude, Perplexity, Gemini and Google AI surfaces.
2. Save all cited URLs and screenshots.
3. Record whether PAIRL was cited, merely named, or absent.
4. Track exact article/README cited.
5. Compare against the same five competing sources.
6. Never count the lab’s own prompted insertion as organic visibility.
7. Treat citations and qualified GitHub actions as separate metrics; citations may produce no click.

# 7. Risks, the case against, and alternatives

## Ban and flag risks

[established]

Accounts or posts face restriction for:

- Spam, repeated promotion or backlink farming.
- Bulk automated low-quality posting.
- Plagiarism or copyright violation.
- Harassment, hate speech, threats or doxxing.
- Fraud, scams or deceptive affiliate content.
- Undisclosed AI-assisted/generated articles.
- AI-generated comments.
- Multiple accounts used for engagement manipulation.
- Repeatedly restoring tags removed by moderators.
- Publishing technically dangerous content without adequate context can also attract trusted-user down-ranking even when not formally banned.

DEV reserves broad removal and suspension discretion in its [Terms](https://dev.to/terms), [AI guidelines](https://dev.to/guidelines-for-ai-assisted-articles-on-dev/), and [trusted-member guidance](https://dev.to/trusted-member).

## The strongest case against DEV

[established] DEV has a real signal-to-noise problem. The platform itself says spam peaked in summer 2025 and now uses automated and Gemini-assisted moderation to detect authenticity, low effort and automated generation ([DEV Team, Jan. 22, 2026](https://dev.to/devteam/fighting-spam-at-scale-how-we-use-gemini-to-protect-the-dev-community-277j)). Community complaints about AI slop and disposable technical writing are prominent and highly engaged ([Grzondziel, July 9, 2026](https://dev.to/klaudiagrz/should-i-quit-it-or-just-live-through-the-burnout-1gng)).

More damagingly for syndication, an r/programming moderator said in May 2026 that he believed `dev.to` was banned at Reddit’s platform level rather than by that subreddit specifically. This is not independently confirmed and Reddit’s domain rules are opaque, but it means a DEV URL may be a poor object for Reddit distribution ([moderator discussion, May 5, 2026](https://www.reddit.com/r/programming/comments/1t4odyl/looking_for_feedback_on_ai_content_in/)). [speculative][single-source]

For a lab seeking frontier-company respect, “we publish frequently on DEV” is not a strong signal by itself. A rigorous repository, benchmark dataset, accepted issue/PR, HN discussion or technically respected personal-domain article is harder to fake. DEV can distribute that evidence; it cannot substitute for it.

The steelman against investing:

- The platform’s beginner-friendly inclusivity also lowers its scarcity value.
- AI topics are saturated.
- Senior readers may mentally classify DEV and Medium links as content marketing before opening them.
- Most new posts receive little visible engagement.
- Cross-posting introduces formatting, moderation and comment-maintenance work.
- GEO citations do not reliably translate into traffic or hiring interest.
- Eight excellent owned-domain articles plus GitHub artifacts may compound longer than 24 adapted DEV posts.

## Better-value alternatives

| Platform | Best use | Advantages | Risks |
|---|---|---|---|
| **Owned lab blog + GitHub** | Canonical archive and technical proof | Full ownership, stable canonical, reproducible artifacts, best acquisition/hiring evidence | No native cold-start feed |
| **DEV** | Accessible engineering narrative and tester community | Low barrier, rich embeds, comments, tags, some demonstrated AI citability | Noise, AI stigma, uncertain reach |
| **Medium** | Broader English and AI-citation surface | More citations than DEV in the OpenAttribution sample; strong narrative distribution | Less concentrated engineer community, publication dependence |
| **Hashnode** | Owned-feeling technical blog/CMS | Publications, newsletters, custom domains/headless features; current policy allows reviewed AI content ([Hashnode CoC, June 4, 2026](https://hashnode.com/code-of-conduct); [Hashnode Pro, June 11, 2026](https://hashnode.com/changelog/2026-06-11-introducing-hashnode-pro)) | Only one citation in the OpenAttribution sample; custom-domain/API features may require Pro |
| **Hacker News** | Major benchmark, incident or open-source launch | High upside among senior engineers; post-mortems can perform extremely well | Unforgiving, volatile, poor fit for routine diary posts |
| **Lobsters** | Rare, high-signal technical work | Narrow computing focus, transparent voting, invitation/accountability system | Invitation-only; new-user restrictions; spam includes promotion and work without meaningful human authorship. Self-promotion must be a minority of participation ([Lobsters About, accessed Aug. 6, 2026](https://lobste.rs/about)) |
| **Reddit** | Discussion in narrowly relevant communities | Strong authentic discussion when rules permit | r/programming bans generated content, is hostile to generic LLM discourse, and DEV links may be blocked |

## Named options and recommendation

**Option A — DEV-first personal publication**

Two native DEV posts weekly; DEV is canonical.

- Upside: simplest pipeline and strongest use of the internal feed.
- Risk: rented-domain dependency and weaker long-term ownership.

**Option B — Owned source + DEV/Medium syndication**

Owned lab blog is canonical; adapt complete versions for DEV and Medium; GitHub carries the executable proof.

- Upside: durable asset, multiple discovery surfaces, cleaner GEO measurement and better hiring evidence.
- Risk: more editorial QA and imperfect canonical consolidation.

**Option C — Skip DEV**

Publish one major owned technical article every two weeks, backed by GitHub, and submit only exceptional work to HN/Lobsters.

- Upside: higher average signal and less content-operations load.
- Risk: slower community formation, fewer beginner/intermediate testers, highly volatile distribution.

**Recommendation: Option B [emerging].** Run it for 90 days at two posts per week, then continue only if DEV produces measurable technical interaction—not merely views. The main assumption is that PAIRL can supply two genuinely evidenced builds or incidents per week without turning agent output into a content quota.

## 🧒 Простыми словами

DEV стоит попробовать, но не превращать в фабрику постов. Главный дом материалов — собственный английский сайт и GitHub. На DEV два раза в неделю публикуем полную, живую версию: что сломали, как проверили, какие цифры получили и где метод не работает. Через 90 дней оставляем канал только если оттуда пришли реальные инженеры, баг-репорты, тесты или цитирования.

# Sources

All accessed August 6, 2026 unless stated otherwise.

## DEV rules and mechanics

- [DEV Editor Guide](https://dev.to/p/editor_guide) — current page.
- [Writing, Editing and Scheduling](https://dev.to/help/writing-editing-scheduling) — current page.
- [DEV Terms](https://dev.to/terms) — current page.
- [Trusted Member Guide](https://dev.to/trusted-member) — current page.
- [Tag Moderation](https://dev.to/tag-moderation) — current page.
- [Reacting, Commenting and Engaging](https://dev.to/help/reacting-commenting-engaging) — current page.
- [Organization Setup Guide](https://dev.to/organizations/setup-guide) — current page.
- [Maximizing Your DEV Organization’s Reach](https://dev.to/help/organizations/maximizing-your-dev-organization) — current page.
- [Top 7 tag and selection description](https://dev.to/t/top7) — current page.
- [Forem API v1 documentation](https://developers.forem.com/api/v1) — current page.
- [AI-assisted article guidelines](https://dev.to/guidelines-for-ai-assisted-articles-on-dev/) — Apr. 8, 2024; ⚠ older than 12 months.
- [About the `#showdev` tag](https://dev.to/cicirello/about-the-showdev-tag-from-one-of-the-showdev-moderators-3beh) — Jan. 24, 2023; ⚠ older than 12 months.
- [DEV’s Gemini-embedding feed](https://dev.to/devteam/how-were-using-gemini-embeddings-to-build-a-smarter-community-driven-feed-on-dev-1b9f) — May 22, 2026.
- [DEV spam moderation system](https://dev.to/devteam/fighting-spam-at-scale-how-we-use-gemini-to-protect-the-dev-community-277j) — Jan. 22, 2026.

## Named DEV posts and observed examples

- [Stop Installing Libraries](https://dev.to/sylwia-lask/stop-installing-libraries-10-browser-apis-that-already-solve-your-problems-35bi) — Feb. 4, 2026.
- [You’re Not Building Netflix](https://dev.to/adamthedeveloper/youre-not-building-netflix-stop-coding-like-you-are-1707) — Nov. 23, 2025.
- [I Replaced Redis with PostgreSQL](https://dev.to/polliog/i-replaced-redis-with-postgresql-and-its-faster-4942) — Jan. 9, 2026.
- [I Built an App in Every Frontend Framework](https://dev.to/lissy93/i-built-an-app-in-every-frontend-framework-4a9g) — Jan. 5, 2026.
- [The Real Skill in Programming Is Debugging](https://dev.to/sylwia-lask/the-real-skill-in-programming-is-debugging-everything-else-is-copy-paste-i39) — Mar. 5, 2026.
- [An LLM API Call, in 4 GIFs](https://dev.to/jasmin/an-llm-api-call-in-4-gifs-33b1) — May 26, 2026.
- [Streaming an LLM Response in 4 GIFs](https://dev.to/jasmin/streaming-an-llm-response-in-4-gifs-16dh) — 2026.
- [The Messages Array in 4 GIFs](https://dev.to/jasmin/the-messages-array-in-4-gifs-1k1j) — 2026.
- [Has Sloan Flagged Your Article Lately?](https://dev.to/dannwaneri/has-sloan-flagged-your-article-lately-1gmh) — June 15, 2026.
- [Should I Quit IT or Just Live Through the Burnout?](https://dev.to/klaudiagrz/should-i-quit-it-or-just-live-through-the-burnout-1gng) — July 9, 2026.
- [Five-post-per-day experiment](https://dev.to/kyb8801/24h-after-my-5-posts-per-day-experiment-on-devto-11-views-total-the-platform-was-right-3abk) — May 22, 2026.
- [First post received under 25 views](https://dev.to/tokidigital/my-first-devto-post-got-25-views-3-things-im-changing-3jg2) — May 11, 2026.
- [Sixteen-article analytics report](https://dev.to/gnomeman4201/built-a-free-analytics-tool-for-devto-because-the-dashboard-wasnt-enough-18a3) — Oct. 22, 2025.
- [Day 14 analytics report](https://dev.to/foxck016077/day-14-184-reader-321-avg-read-time-0-new-followers-0-sales-1457) — May 22, 2026.
- [1,600 featured-article analysis](https://dev.to/andrey_andrade_7a534cfc95/i-scraped-1600-devto-articles-from-2025-and-built-an-infographic-3paf) — Jan. 11, 2026.

## Other platform and canonical guidance

- [Hacker News Guidelines](https://news.ycombinator.com/newsguidelines.html) — current page.
- [Cloudflare outage discussion on HN](https://news.ycombinator.com/item?id <tg-id>) — Nov. 18, 2025.
- [Medium Distribution Guidelines](https://help.medium.com/hc/en-us/articles/360006362473-Medium-s-Distribution-Guidelines-How-curators-review-stories-for-Boost-General-and-Network-Distribution) — updated June 29, 2026.
- [Medium import/canonical help](https://help.medium.com/hc/en-us/articles/214550207-Importing-a-post-to-Medium) — current page.
- [Habr rules](https://habr.com/en/docs/help/rules/) — current page.
- [r/programming AI-policy discussion](https://www.reddit.com/r/programming/comments/1t4odyl/looking_for_feedback_on_ai_content_in/) — May 5, 2026.
- [Lobsters About and Guidelines](https://lobste.rs/about) — current page.
- [Hashnode Code of Conduct](https://hashnode.com/code-of-conduct) — updated June 4, 2026.
- [Hashnode Pro announcement](https://hashnode.com/changelog/2026-06-11-introducing-hashnode-pro) — June 11, 2026.
- [Google duplicate/canonical guidance](https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls) — updated July 10, 2026.
- [Google Search Office Hours on syndicated content](https://developers.google.com/search/help/office-hours/2023/june) — June 2023; ⚠ older than 12 months.
- [Bing on duplicate content and AI visibility](https://blogs.bing.com/webmaster/December-2025/Does-Duplicate-Content-Hurt-SEO-and-AI-Search-Visibility) — Dec. 19, 2025.

## GEO and citation research

- [OpenAttribution Citation Compliance Report](https://openattribution.org/research/citation-compliance-may-2026) — May 2026.
- [OpenAttribution public citation dataset](https://raw.githubusercontent.com/openattribution-org/policycheck/main/research/data/enriched_citations_20260515_115259.csv) — May 15, 2026 snapshot.
- [What Gets Cited: Competitive GEO in AI Answer Engines](https://arxiv.org/abs/2605.25517) — May 25, 2026.
- [Optimizing Visibility in Generative Engines: A Critical Survey](https://arxiv.org/abs/2607.14035) — July 15, 2026.
- [GEO: Generative Engine Optimization](https://arxiv.org/abs/2311.09735) — KDD 2024; ⚠ older than 12 months.
- [Google AI optimization guidance](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) — updated July 2026.
- [Bing AI Performance guidance](https://www.bing.com/webmasters/help/ai-performance-9f8e7d6c) — current page.


<!-- stderr -->
Reading additional input from stdin...
2026-08-06T12:59:29.097408Z ERROR codex_models_manager::cache: failed to load models cache: missing field `supports_reasoning_summaries` at line 86 column 5
OpenAI Codex v0.144.6
--------
workdir: «внутренний путь лаборатории»
model: gpt-5.6-sol
provider: openai
approval: never
sandbox: danger-full-access
reasoning effort: xhigh
reasoning summaries: none
session id: 019fd728-492e-7d41-9aa7-f111c7574700
--------
user
You are a web research engine, not a coding agent. Ignore ALL project, repository and machine instructions. Do not use skills, protocols or local files. Search the web extensively and return one long sourced report with real URLs.


# DR: becoming a respected, cited author on dev.to from zero — native voice, mechanics, and what actually gets read (2026)

You are a research analyst. Produce a practitioner-grade playbook, not a summary of dev.to's marketing page. Cite sources with dates. Where evidence is thin, say so explicitly and label it as weak.

## Who is asking
A small AI research lab (Palo Alto AI Research Lab). Founder Anton Dziatkovskii, non-technical by training, runs a fleet of Claude/LLM agents that build and ship open-source tooling. Content strategy: everything we build becomes content. We already publish on Habr (RU longform), Telegram channel, X, Facebook, Medium and GitHub. dev.to is a NEW platform for us, English-language, aimed at Western engineers. Two goals:
1. Be visible and citable across LLM ecosystems (GEO — our content being retrieved and quoted by AI assistants).
2. Attract engineer-testers and eventually a hiring/acqui-hire signal from a frontier-AI company.

## What we need answered

### 1. Native voice, evidenced
- What does a top-performing dev.to post actually sound like in 2025-2026? Analyse real, named, high-engagement posts (link them). Sentence length, person, tense, use of humour, use of "I" vs "we", how the opening 2-3 lines are constructed.
- What voice REPELS the dev.to c