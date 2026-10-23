# DR26-10-02-ZB-02-1136 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll pull live comparisons, eval boards, and method papers, then write the full report in this reply.A. BLUF

No published 2025–2026 evaluation measures the thing you actually post: one 200–280 character, first-person, dry, concrete failure, with no hashtags, emoji, or stock AI phrasing. On the boards that do exist, the hypothesis “Grok writes the best short posts because it was trained on X” is not supported. Human creative-writing votes put Gemini and Claude ahead of every Grok variant that has enough votes to rank (best Grok on that board is 24th). The only board with a “social media and short formats” cut names GPT-6.1 Sol as the category winner. Grok 4.7 is a real contender on long English prose and is the fiction winner on one AI-judged board, with a low slop score, so it belongs in a blind draft pool. It is not the default. For a “pain of the day” generator, draft with GPT-6.1 Sol and a current Claude (Opus 5.5 or Fable 5.1), keep Grok 4.7 as the third arm, throw out anything that fails a character/hashtag/emoji check in code, and let a human pick. Live likes on an account under 2,000 followers will not crown a winner.

Claim labels below: sourced = opened in this run. Inferred = conclusion from those sources, not a number printed by them.

B. What the public boards actually measure

None of them is a tweet test.

LMArena Text / Creative Writing is blind human votes on open-ended text, including creative writing, not a 280-character constraint. Page read 30 Sep 2026: 1,363,171 votes, 408 models. https://arena.ai/leaderboard/text/creative-writing (requested as https://lmarena.ai/leaderboard/text/creative-writing).

EQ-Bench Creative Writing v3 is 32 story prompts, 3 samples each, temperature 0.7, judged by Claude (Elo judge updated 1 Mar 2026 to Claude Sonnet 4.6; rubric still Claude Sonnet 4). https://eqbench.com/about.html and the score table at https://eqbench.com/creative_writing.html. Lengths on that table are in the thousands (Grok 4.7 at 6,282; GPT-5.5 at 12,945). That is a different task from a post.

The AI Writing Benchmark (Johannes Eva, personal project, no vendor funding) ranks English writing from blind duels. 102 models, 103,355 AI verdicts, updated 3 Oct 2026. Human scores are mostly still empty. Rank is the AI-judge score. https://aiwritingbenchmark.com/ and https://aiwritingbenchmark.com/about/. Its homepage does split “social media and short formats” from fiction, email, and marketing. The filtered leaderboard URL returned the overall table, not a full category ranking, so only the category winners printed on the homepage are used below.

Hemingway-bench is expert writers doing blind pairwise review (5,000+ comparisons). The write-up is dated 4 Feb 2026 and 29 Sep 2026. Its model set is Gemini 3, Claude Opus 4.5, GPT-5.2, and an unspecified Grok. Do not treat those ranks as ranks of Grok 4.7 or Claude Opus 5.5. https://surgehq.ai/blog/hemingway-bench-ai-writing-leaderboard

Vendor blogs that “tested tweets” (VoiceMoat 30 Jul 2026, Reprompt.org, Ritner Digital 15 Apr 2026, XposterAI 25 May 2026, Fwdslash 3 Sep 2026) are not blinded public panels. They are not used for the ranking.

C. Published comparisons, question 1

C1. Human votes, creative writing, 30 Sep 2026. Sourced from https://arena.ai/leaderboard/text/creative-writing.

Stable top, thousands of votes: Claude Fable 5 high, rank 3, 1503 ±8, 8,072 votes. Claude Opus 4.6 high, rank 4, 1501 ±6, 13,974 votes. Gemini 3.1 Pro preview, rank 11, 1480 ±5, 22,144 votes. GPT-5.6 Sol xhigh, rank 19, 1469 ±8, 7,388 votes.

Preliminary, wide intervals: Gemini 4 Argon high, rank 1, 1522 ±19, 1,104 votes. Claude Opus 5.5 high, rank 2, 1515 ±21, 908 votes. A gap of about 20 points with a ±20 interval is not a settled lead.

Grok on the same board: Grok 4.20 beta1, rank 24, 1463 ±10, 4,172 votes. Grok 4.5, rank 35, 1451 ±8, 8,173 votes. Grok 4.20 reasoning, rank 46, 1446 ±7. Grok 4.6 high, rank 49, 1445 ±9. Grok 4.7 xhigh, rank 68, 1433 ±17, only 1,293 votes. Grok 4.1, rank 71, 1431 ±7.

The confidence intervals of Claude Fable 5 high (about 1495–1511) and Grok 4.20 beta1 (about 1453–1473) do not overlap. On this human creative board, current Grok is behind the Claude and Gemini cluster, not leading it. DeepSeek V4 Pro is rank 45 at 1446 ±7, in the same band as mid-pack Grok. No Mistral and no Llama appeared in the top 90 rows that rendered.

C2. EQ-Bench Creative Writing v3, Claude-judged stories. Sourced from https://eqbench.com/creative_writing.html.

Elo order at the top: GPT-6 Astra 2173.3 (slop 1.2, rubric 84.00). Claude Fable 5.1 2162.0 (slop 1.1, rubric 84.75). Claude Opus 5 2132.6 (slop 0.9, rubric 85.35). GPT-6 Sol 2124.7 (slop 1.2). Kimi K3 2082.3 (slop 1.3). GLM-5.3 2075.0 (slop 1.2). Claude Opus 5.5 2050.1 (slop 1.4). Grok 4.7 2006.7 (slop 1.2, rubric 85.80).

Grok 4.7’s rubric, 85.80, is the highest rubric in that top cluster, while its Elo is 8th. Rubric and Elo disagree. Further down: GPT-5.5 Elo 1843.9 but length 12,945 and slop 1.8. Gemini 3.8 Flash Elo 1747.9, slop 3.1. Grok 4.5 Elo 1578.8, slop 2.5. Grok 4.20 beta Elo 1573.6, rubric 72.55, a weak writing score. DeepSeek V4 Pro Elo 1553.2, slop 2.7. Gemini 3.1 Pro preview Elo 1491.3, slop 4.2. Mistral Large 3 Elo 1412.2, slop 3.9. Gemini 2.5 Pro preview slop 4.0 to 4.9.

Hemingway’s authors report that this style of Claude autograder agreed with expert writers as little as 43% of the time, and that it rewarded overwritten metaphor. https://surgehq.ai/blog/hemingway-bench-ai-writing-leaderboard. EQ-Bench’s own about page says its judge can prefer a model family, and that length bias in pairwise judging is strong enough that they truncate outputs before Elo judging. https://eqbench.com/about.html. So a high EQ-Bench Elo is not a vote for blunt short posts.

C3. AI Writing Benchmark, English, 3 Oct 2026. Sourced from https://aiwritingbenchmark.com/.

Overall: Claude Opus 5.5 adaptive high 96.9. GPT-6.1 Sol high 95.3. Kimi K3 92.4. GPT-6 Astra high 92.2. Claude Fable 5 89.7. GPT-6 Sol 89.4. Grok 4.7 high 89.3. Claude Fable 5.1 89.2. GLM 5.3 89.1. Claude Sonnet 5.5 88.8. A score of 50 means an even chance of beating the average model. Human score column on the leaderboard page was blank for these models. https://aiwritingbenchmark.com/leaderboard/?category=social_short_formats returned that same overall table, so the category filter did not render a separate ranking in this run.

Category winners printed on the homepage, recomputed on that category’s duels only:

Social media and short formats: GPT-6.1 Sol high, 95.7.
Fiction and narrative: Grok 4.7 high, 96.8.
Marketing and sales: Claude Sonnet 5.5, 96.6.
Personal communication: Claude Opus 5.5, 98.7.
Articles: Claude Fable 5, 95.8.

That is the closest public number to your format, and it is still broader than “200–280 character pain post.” It is also an AI-judge score. Inferred: it is a reason to put GPT-6.1 Sol in the draft pool, not a reason to skip the blind test.

C4. Expert writers, earlier model generation. Sourced from https://surgehq.ai/blog/hemingway-bench-ai-writing-leaderboard.

Top 3: Gemini 3 Flash, Gemini 3 Pro, Claude Opus 4.5. Persona notes from the raters: Opus 4.5 sounded the most human and the least like it was performing. GPT-5.2 was strong on practical and marketing text and weaker on creative tasks. Grok, version not named, “leaned heavily into informality,” which sometimes fit casual mail and often did not, and it overused tropes and pop-culture references. Llama 4 Maverick lost points for verbose, generic output. Qwen3 was original and also error-prone, with forced humor.

C5. Where the boards disagree. Sourced as a comparison of C1–C4.

Human creative votes (LMArena): Gemini and Claude, Grok outside the top 20.
Claude-judged long stories (EQ-Bench): GPT-6 Astra and Claude Fable 5.1, Grok 4.7 eighth, Gemini Flash well behind on Elo and much higher on slop.
AI-judged English writing: Claude Opus 5.5 overall, GPT-6.1 Sol on short social, Grok 4.7 on fiction.
Expert writers on the early-2026 set: Gemini 3 and Claude Opus 4.5, Grok described as too informal.

Inferred: “best writer” is not one model, and “best at a 240-character engineer complaint” is not what any of these boards decided. Anyone who quotes a single leaderboard as the answer is choosing a judge and a genre.

D. Does training on X make Grok better at short posts? Question 2

What xAI and X actually say. Sourced.

X’s help page says Grok was pre-trained on publicly available sources plus data reviewed by human AI Tutors. It also says X may share public posts, post metadata, Spaces, profiles, and your Grok inputs and outputs with xAI for training and fine-tuning, unless you opt out or protect the account. The stated uses include language, answer quality, humor and wit, and political balance. It does not claim this produces better short posts. The same page says Grok can choose, at answer time, to search public X posts and the web. That is retrieval, not a property of the weights. https://help.x.com/en/using-x/about-grok

The EU-style training summary for Grok 4.5, dated 8 Jul 2026, says pre-training used more than 10 trillion text tokens from a diverse public corpus, “including, for example, certain scientific text, legal and official documents, social media posts, and source code,” plus licensed data, user and contractor data, and synthetic data generated by Grok itself. Collection for that summary runs to June 2026, and the model is “continuously refined on new data after this date.” xAI says it does not train on X data from users who opted out. Social posts are one example inside a corpus of that size. The summary does not give a share, and it does not report a tweet-writing eval. https://media.x.ai/v1/website/public-summary-of-training-content-for-grok-4.5_8jul2026.docx-fc25014a.pdf

What that does and does not show. Inferred.

X data in the mix is real. A causal claim that this mix makes 200–280 character posts better than Claude or GPT is not in these documents, and the boards in section C go the other way for creative writing and, on the one short-social cut, point at GPT-6.1 Sol. Live X search can help a post about what is happening on X today. It does not help a post about a failure in your own lab, and it can drag a draft toward whatever is trending. Humor is an explicit training target on the help page, which cuts both ways for dry irony: the model has a reason to add a joke.

No independent controlled comparison that isolates “trained on X posts” as the cause of better short copy turned up in this run. Heavy-user and vendor write-ups that say “Grok sounds native to Twitter” were not blinded panels. Treat that sentence as folklore until your own blind test says otherwise.

E. Failure modes for short copy, question 3

Over-length, sourced as a long-form measurement and inferred for tweets. On EQ-Bench, GPT-5.5’s length is 12,945 and GPT-5.6 Terra’s is 10,271, against about 6,000 for Claude Opus 5 (6,003) and Grok 4.7 (6,282). https://eqbench.com/creative_writing.html. Hemingway’s raters called Llama 4 Maverick verbose. Inferred: a model that runs long when unconstrained will explain the pain, add a moral, or add a second sentence, unless something outside the model cuts it.

Character limits, sourced. Asking the model to count characters is a weak control. CharBench finds character-level tasks hard for current models: average accuracy about 50%, and some positional tasks at 43.6% and 32.3%. Counting performance tracks the true count and the string length more than tokenizer trivia. https://arxiv.org/abs/2508.02591 and https://arxiv.org/html/2508.02591v3. A second paper shows the same class of failure is systematic across Llama, Qwen, and Gemma, including cases where the count is represented internally and then not emitted. https://arxiv.org/html/2604.00778v1. Inferred: the gate has to be a program that counts characters after generation. Reject and resample. Do not ask the model whether it complied.

Slop words, sourced on long stories, not on tweets. EQ-Bench slop is the rate of phrases over-used by models. In the top cluster: Claude Opus 5 at 0.9, Claude Fable 5.1 at 1.1, Grok 4.7 and the GPT-6 pair and GLM-5.3 at 1.2, Claude Opus 5.5 at 1.4. Gemini Flash sits at 3.1–3.5. Gemini 3.1 Pro preview is 4.2. Older Gemini 2.5 Pro previews are 4.0–4.9. Mistral Large 3 is 3.9. https://eqbench.com/creative_writing.html. Inferred: if the failure mode you hate is stock literary filler, the Gemini Flash line and Mistral Large 3 are the ones the long-story board flags, not Grok 4.7.

Em dashes and the other “sounds like AI” marks, sourced, and they do not track quality. On 3,350 ranked English texts, counted 20 Sep 2026, marks per 1,000 words (em dashes, stock phrases, “not X but Y,” lists of three): Claude Sonnet 5.5 at 2.4, Claude Fable 5.1 at 2.5, GPT-6 Sol at 2.8, Grok 4.7 at 7.4, Claude Opus 5 at 9.1, Kimi K3 at 14.5, Claude Fable 5 at 14.6. Almost all of the high scores are em dashes. Grok 4.7 specifically: 4.5 em dashes, 0 stock phrases, 0.8 “not X but Y,” 2.1 lists of three, per 1,000 words. “Delve” was too rare to make the top ten stock words. Across the corpus, stock phrases were about 0.5 per 1,000 words, roughly one every 2,000 words. Correlation between this index and writing score was about zero (they also report −0.32 on the 36 models with at least 30 texts). The best writers were among the heaviest dash users. https://aiwritingbenchmark.com/insights/which-ai-sounds-least-like-ai/

Inferred for your house style: banning the em dash is a constraint you want, not a quality signal. Claude Fable 5.1, Claude Sonnet 5.5, and GPT-6 Sol already produce few dashes. Grok 4.7 produces more, so a post-filter that replaces or rejects dashes will fire more often on Grok. Kimi K3 and Claude Fable 5 will need the same filter even though they rank at the top for prose.

Hashtags and emoji. Not measured in any table opened in this run. The same article says emoji are counted separately and are not in the slop index, and that the index depends on the prompt: ask for a list and you get a list. Unknown per model. Inferred: put the ban in the prompt and enforce it in code, because no board tells you who adds them by default.

Humor overshoot. Sourced only for an unnamed Grok in the Hemingway rater notes, plus X’s help page listing humor as a training goal. Inferred risk for this format: a punchline glued onto a real incident. That is a reject reason in the rubric below, not a proven rate.

Instruction drift into a lesson. Not directly measured. Hemingway’s example of a model that turns a short-story prompt into life advice is the nearest sourced analogue, and it was the overwritten model, not a named production system you can buy today. Inferred: “one failure, no advice” has to be a scored criterion, because nothing in the public boards enforces it.

Invisible watermarks. As of the 2 Oct 2026 note on the same site, Claude and Gemini text carries an invisible mark; ChatGPT, Grok, Mistral, Meta, and local models do not, and people outside those companies cannot check a text today. https://aiwritingbenchmark.com/ (homepage summary of that note). This is not a quality difference.

F. Prompting that has evidence, question 4

Few-shot with the author’s own text, sourced. Low-resource authorship transfer on Reddit used about 16 posts, roughly 500 words, as exemplars in a rewrite prompt, after a neutral paraphrase step. https://arxiv.org/html/2212.08986v3. A 2026 style-transfer paper uses GPT 5.1 16-shot as the prompting baseline against a trained adapter method, and the adapter won on their metric (0.905 vs 0.768 on the example they print). https://arxiv.org/pdf/2603.23069. Inferred for you: 8–16 of your own posted pains, pasted as finished posts, beat an adjective list (“dry, blunt, peer tone”). Do not include posts you would no longer publish. Exemplars teach the model your worst habits too.

Best of n, then pick, sourced as a method, n inferred. Best-of-n (draw n, keep the one a judge prefers) is, inside the class those authors study, the sampling scheme that maximizes win rate against the base model for a given distance from it. https://arxiv.org/abs/2406.00832. Repeated sampling raises the chance that some sample is good, often as a log-linear coverage curve, but when there is no automatic checker, majority vote and reward models stop improving after a few hundred samples. https://arxiv.org/abs/2407.21787. Inferred: for a 240-character post, generate 8, delete the ones that fail the code gate, and only then have a human or a different model rank the survivors. Going from 8 to 100 is not where the gain is, because you cannot automatically verify “sounds like a person who was there.”

Critique then rewrite, sourced, with a limit. Self-Refine (same model writes, critiques, rewrites) beat one-shot generation by about 20 percentage points on average across seven tasks, including constrained generation, in human and automatic preference. Gains were uneven and later passes added less. https://arxiv.org/html/2303.17651v2. Inferred, from the self-preference papers in section G: use one rewrite pass, and use a different family as the critic than the one that drafted. A Claude critic of a Claude draft will tend to like Claude’s habits.

Temperature. The only setting tied to a writing board opened here is EQ-Bench’s 0.7 with min_p 0.1. https://eqbench.com/about.html. Inferred: use 0.7 as the control arm. If you try 0.9 or 1.0, do it as another arm of the blind test, not as a belief. Best-of-n needs some spread. A single sample at temperature 0 does not.

Tools. Typefully’s writing assistant, shipped 23 Feb 2026, says it learns tone from your samples and drafts, can store a standing rule such as “no em dashes,” rewrites in the editor, and does not auto-post. https://typefully.com/changelog/writing-assistant-133 and https://typefully.com/ai. That matches the few-shot evidence. It is a product claim, not a bake-off against raw GPT, Claude, Grok, or Gemini. No methods write-up for Hypefury or Taplio was opened in this run. Nothing here says those products have a better base model.

A workable recipe, inferred as the combination of the pieces above:

Paste 8–16 of your real posts. State the incident in one or two plain sentences, including the concrete thing that broke. Require 200–280 characters, first person, one failure, no lesson, no hashtag, no emoji, no em dash, no “delve,” no “game-changer,” no “not X but Y.” Ask for 8 drafts. Count characters, strip or reject banned surface forms in code. Rank what remains blind. Allow one cross-family rewrite only when the critic points at a specific fault (a moral, a joke, a second idea, a dash). Post that one.

G. A fair test on an account under 2,000 followers, question 5

Decide on blind preference. Do not decide on likes.

What X’s ranker actually weights, sourced from the defaults in https://raw.githubusercontent.com/xai-org/x-algorithm/main/vm-ranker/params.rs (file read in full this run; repo commit dated 29 Sep 2026). These multiply predicted probabilities, and a parallel write-up of home-mixer/params/param.rs quotes X’s own comment that the weights are not multipliers on raw counts, because rare actions are up-weighted so they can affect the score at all. https://ppc.land/x-gives-copy-link-shares-a-feed-weight-of-20-against-0-5-for-likes/

Defaults in the file that was read:

Copy-link share 20.0. Reply 5.0. DM share 5.0. Quote 5.0. Follow author 4.0. Generic share 2.0. Repost 1.0. Like 0.5. Click 0.3. Open link 0.2. Dwell 0.05. Continuous click-dwell 0.4. Not dwelled −0.02. Block author −31.2. Not interested −47.52. Mute author −58.8. Report −234.0. Extra reply weight when you and the viewer follow each other: +15.0. Out-of-network factor 0.75.

There is no bookmark parameter in that file. PPC Land, citing a third-party reading of both the 2023 Heavy Ranker and the 2026 file, says bookmarks have no weight in either, and that the old “bookmark = 10” figure was a misreading of a 2023 “clicked into the conversation and stayed” signal. The 2023 reply weight was 13.5, not today’s 5.0. Do not use 2023 cheat-sheets.

Inferred from those weights: a like is a weak outcome even when you can measure it. Replies, quotes, and private shares are the public outcomes that match the ranker. You will often be unable to see copy-link shares and DM shares, which are the heaviest positive terms. Optimizing the test for likes optimizes the smallest positive weight you can actually see.

Why a small account cannot settle this with engagement. Inferred arithmetic, standard two-proportion sample size at 80% power and 5% two-sided (n per arm ≈ 16 × p × (1−p) / d²). If 30% of posts get at least one reply and you want to detect a lift to 50%, you need on the order of 100 posts per model. If you want to detect a large lift, 50% to 80%, you still need on the order of 40 per model. At one post a day and four models, that is many months, and the audience, the topics, and the product will have changed. Fourteen posts per model, which is what an eight-week rotation gives you, can show a blowout. It cannot show a 20% edge. The out-of-network multiplier (0.75) makes reach for non-followers even noisier.

Design that will actually return a signal.

Freeze the rubric before you generate anything. Score only: one concrete failure a peer would recognize; first person; sounds like a person who was in the room; no lesson and no call to action; no joke bolted on; 200–280 characters after the code gate; no hashtag, emoji, em dash, or banned phrase. Preference is holistic. The checks are pass/fail.

Build 40 items from real incidents, written as two plain sentences each, by a person. Same wrapper prompt for every model. Four models is the maximum: GPT-6.1 Sol, Claude Opus 5.5 or Claude Fable 5.1 (pick one Claude and keep it), Grok 4.7, and one challenger if you insist (Gemini 4 Argon only if you accept that its arena lead is preliminary). Do not add Mistral Medium 3.5 (overall writing score 22.2 on the English board) or an old Llama on the strength of these sources.

Each model returns 8 samples at temperature 0.7. Code drops failures. If none survive, that model scores a forfeit on that item, which is data, not a reason to relax the gate. A human who did not prompt the models sees the survivors unlabeled, order shuffled, and picks a winner. Three such readers, engineers who read X, is enough. Forty items × three readers is the sample. Swap order. Never show the model name. Do not let the prompter vote.

Pre-register the decision rule: the winner is the model with the most first-place picks. If the top two are within a small margin you set now (for example 5 percentage points of first-place share), call it a tie and keep the cheaper one. Do not fish for a metric that separates them after you see the table.

LLM-as-judge, if you use one at all. Self-preference is documented, and it is not only vanity. GPT-4 gave higher scores to lower-perplexity text, whether or not it wrote that text. https://arxiv.org/abs/2410.21819. On a set of more than 5,000 items with human labels, GPT-4o and Claude 3.5 Sonnet gave higher scores to their own outputs and to their own family. https://arxiv.org/abs/2508.06709. On rubric checks that can be verified by a program, a judge was more than 50% more likely to mark its own failed output as passing. Ensembling judges reduced that and did not remove it. On a subjective medical rubric the skew reached about 10 points. https://arxiv.org/html/2604.06996v3. A later attempt to separate “narcissism” from “the judge is just bad at the item” still left residual self-preference after uncertainty was accounted for. https://arxiv.org/html/2601.22548v4. A 2026 mitigation paper reports about a 31.5% reduction from splitting the rubric into separate dimensions, not elimination, and reports that a more capable model is not a less biased judge. https://arxiv.org/abs/2604.22891.

Practical rule, inferred from those papers: the judge must not be from a family that drafted. If Claude, GPT, Grok, and Gemini all draft, do not use any of them as the only judge. Use the three humans. If you want a model in the loop, use one held-out family (Kimi K3 is the obvious candidate that actually ranks near the top on the English writing board) and throw away any item where the model judge and the humans disagree. Swap position on every pair, as EQ-Bench does, because position bias is large. https://eqbench.com/about.html. Never give the judge your target conclusion.

Live posting is a calibration, not the ranking. Post only the human winner, or, if you want a check that preference predicts replies, rotate models in a pre-written schedule that balances weekday and topic, one post a day, for at least eight weeks. Do not post four phrasings of the same incident. Log replies, quotes, reposts, follows, and bookmarks. Treat likes as a footnote. Look at the live numbers only for a gross miss, such as one model getting replies and the others getting silence. Absence of a statistically significant like-rate gap is the expected result, not evidence that the models are equal.

H. Verdict table, question 6

Confidence is for this format, not for writing in general. “Draft / critique / rewrite” is the recommended job.

GPT-6.1 Sol. Strength: only published winner found for “social media and short formats” (95.7), and second overall on that English board (95.3). Sibling GPT-6 Sol has a very low machine-prose index (2.8 per 1,000 words). Weakness: the short-format number is an AI-judge score with almost no human votes yet, and the category is wider than your template. GPT-5.5 and GPT-5.6 Terra run very long on EQ-Bench, so length drift is a family risk you must gate. Use: primary drafter. Confidence: medium-low.

Claude Opus 5.5. Strength: first on the English writing board (96.9) and second on LMArena creative, though that arena row is preliminary (908 votes, ±21). Personal-communication winner at 98.7. Weakness: EQ-Bench slop 1.4 and Elo behind Fable 5.1 and Opus 5. Arena interval still wide. Use: primary drafter, alternate to Fable 5.1. Confidence: medium-low.

Claude Fable 5.1. Strength: EQ-Bench Elo second (2162), slop 1.1, and one of the lowest em-dash rates among top models (0.6 per 1,000). LMArena creative rank 10 at 1482 ±13 with 2,506 votes, behind Fable 5 high. Weakness: expensive relative to the others on the arena price column ($10 / $50 per million). Can still write a polished paragraph if you do not gate length. Use: drafter, or the critic when GPT drafted. Confidence: medium.

Claude Opus 5. Strength: lowest slop in the EQ-Bench top cluster (0.9) and Elo third (2132.6). Weakness: heavier em-dash rate than Fable 5.1 (about 7.9 per 1,000 in the September count). Use: critic or rewrite pass, not a fourth drafter. Confidence: medium-low.

Grok 4.7. Strength: fiction winner on the English board (96.8), overall 7th (89.3), EQ-Bench slop 1.2 and the best rubric in that top cluster (85.80). X data and a humor objective are real. Weakness: LMArena creative rank 68 with a wide interval and few votes. More em dashes than GPT-6 Sol or Fable 5.1 (4.5 per 1,000). Older rater notes, on an unnamed Grok, say it overdoes informality and pop-culture. No evidence that X pretraining wins this format. Use: third drafter, the hypothesis arm. Also a good critic of Claude or GPT drafts. Do not make it the only model. Confidence: medium that it is worth testing, low that it wins.

Grok 4.5 and Grok 4.20. Strength: 4.20 beta1 is the highest Grok on the human creative board, still 24th. Weakness: 4.20 beta’s EQ-Bench rubric is 72.55. 4.5’s slop is 2.5 and its Elo is far behind 4.7. Use: do not use for this job while 4.7 is available. Confidence: medium.

Gemini 4 Argon high. Strength: rank 1 on LMArena creative. Weakness: preliminary, ±19, 1,104 votes, and there is no short-post number. The Gemini Flash line has the worst slop among frontier models on EQ-Bench (3.1 and up). Use: optional fourth drafter only inside the blind test. Not the production default. Confidence: low for this format.

Gemini 3.x Pro / Flash. Strength: Gemini 3 Pro and 3.1 Pro are high on human creative votes. Hemingway’s experts liked Gemini 3 Flash and 3 Pro on an older set. Weakness: slop 3.1–4.2 on the current story board. Use: not the voice model if stock phrasing is the defect you are avoiding. Confidence: medium on the slop gap, low on tweets.

Kimi K3. Strength: third on the English board (92.4), fifth on EQ-Bench Elo. Weakness: among the heaviest em-dash users (11.9 per 1,000). Use: held-out judge, not a drafter, unless you enjoy deleting dashes. Confidence: medium for “strong prose, wrong surface habits.”

DeepSeek V4 Pro, Qwen3.8, GLM-5.3. GLM-5.3 is close on both EQ-Bench (Elo 2075, slop 1.2) and the English board (89.1). DeepSeek V4 Pro is mid-pack (Elo 1553, slop 2.7; English board 59.1 for the Pro row that rendered). Qwen is mixed and not near the English-board top (Qwen3.8 Max at 73.0 on that board). Use: skip for the voice. GLM-5.3 is the only one of these three worth a later challenger slot. Confidence: low to medium.

Mistral Large 3 / Medium 3.5 and Llama 4 Maverick. Mistral Large 3 Elo 1412 and slop 3.9. Mistral Medium 3.5 at 22.2 on the English board. Llama 4 Maverick called verbose and generic by Hemingway raters, older set. Use: not for this format. Confidence: medium.

I. Ranked recommendation for the generator

1. Production draft pool, in this order, until your 40-item blind test says otherwise: GPT-6.1 Sol, then Claude Fable 5.1 (or Opus 5.5 if Fable’s price is the constraint), then Grok 4.7. Generate 8 from each on the days you test. On ordinary days, generate 8 from the current winner only.

2. Code gate before any human looks: 200–280 characters, no hashtag, no emoji, no em dash, banned-phrase list. Forfeit if a model cannot clear it inside 8 draws.

3. Human picks the one post. One cross-family rewrite only when the note names a concrete fault. GPT critic for a Claude draft, Claude critic for a GPT draft, Grok critic for either. Not the authoring model.

4. Do not ship Gemini Flash, Mistral, or Llama as the voice on the evidence opened here. Gemini 4 Argon can enter the blind pool as a fourth name. It should not be the default while its creative-writing lead rests on about a thousand votes.

5. Confidence in this ordering for your exact format: low-to-medium. Confidence that Grok is not already proven the winner by X training data: high. The ordering is a prior for the test in section G, not a substitute for it.

J. Limits of this run

The social-category page did not return a full per-model table, so only homepage category winners are cited. The separate EQ-Bench slop-score page did not render its table (it stopped at “loading”), so slop numbers are only those printed on the Creative Writing v3 table. Hemingway’s Grok is not versioned. No opened source is a blind human panel on 200–280 character engineer posts. Hypefury and Taplio system prompts were not found. Sample-size figures in section G are calculated here from the usual two-proportion formula, not copied from an X paper.
