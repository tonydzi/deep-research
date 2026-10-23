# DR26-09-02-HUB-01-1307 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll pull live sources on sales cadences, dunning, and the delete-and-resend mechanic, then write the full report from what those pages actually say.Next I’ll open the primary studies and look for the delete-and-resend mechanic, flood limits, and cadence docs.I’ll open the primary papers and platform docs next so the numbers in the report come from pages actually read, not summaries.A. CADENCE COMPARISON (platforms, dunning, us)

What the world actually runs, versus our 7-daily + 3-weekly + hard-stop-31. This is about people who already agreed, then went silent — not first-touch cold.

| Discipline / product | Typical touches | Spacing | Stop rule | Notes |
|---|---|---|---|---|
| Outreach FUP (post-reply / post-meeting, their own playbook) | ~5–7 (example: 3 auto emails + 4 manual; high-value = 6 steps) | First auto nudge ~day 3; phone after a couple more days; final email ~1 week | Any reply stops; after ~5 no-replies the rep gets a “what next?” task | Built for “they agreed to a next step then ghosted.” Mix of email / phone / LinkedIn, not identical text. |
| Apollo example (tier-1 sequence) | ~6–8 | Days 1, 4, 7, 10, 23, 25 | Reply / ruleset stop; OOO pauses | Email + LinkedIn + call + gift; they explicitly say “a single automated reply to your original email” as a resurface, then wait weeks. |
| Salesloft (common public template) | ~10–12 mixed | Day 1 (call+email), 3, then video, day 11, 16… | Skip-step exists; throttle queues overflow to next day | Cadence is day-based. Missed auto-email is queued, not dropped. |
| Instantly / lemlist / Woodpecker (cold email, the public default) | 4–7 emails over 14–21 days | 2–3 days first gap, then 4–7, then 7–14 | Reply / unsubscribe / bounce; Instantly: delay is calendar days, send slips to next allowed day | Almost nobody publishes daily identical sends. |
| Klenty | User-built; throttle = max new starts/day | Per-step wait in days | Stop on reply (optional stop on auto-reply); email-priority = follow-ups before new first-steps | Same engine idea as Outreach: cap enrollment, prioritize in-flight. |
| Telegram sales blogs (CRMChat etc.) | 3–4 DMs | Day 1 / 3–4 / 7–9 / 14 | Stop at 4; 60-day re-engage bucket | They warn: daily or 5+ DMs → reports. |
| Stripe Smart Retries (charge, not copy) | Default rec: 8 tries in 2 weeks (config 1–8 tries, 1–8 weeks) | ML-timed, not daily-fixed | Hard decline → don’t retry until new card; then cancel / unpaid / past_due | Silent retries. Customer email is a separate layer. |
| Chargebee dunning | Custom max 5 retries (example 1, 4, 8 days); Smart Retry up to 12 by error type | Expanding, not daily | End of dunning period → configured final action | Emails are decoupled from retries. |
| Baremetrics Recover | Up to 7 emails | Days 0, 3, 7, 13, 15, 20, 30 | Day 30; plus pre-expiry 30/7-day card reminders | Copy changes and urgency rises. |
| Churnbuster | Campaign window default ~25–28 days (ecommerce often ~1 month) | Adaptive by decline reason; retries spaced, not 1 email per retry | Recovered / cancel / passive churn at window end | First email strongest; they warn against over-emailing. |
| Recurly retries | Until 7 declines, or 20 attempts, or 60 days | Soft vs hard decline logic; gateway errors every 2–3 days | Whichever cap hits first, or dunning ends | ML “intelligent retries.” |
| FDCPA / CFPB (US consumer debt PHONE) | Presumed illegal: >7 calls in 7 days, or any call within 7 days after a live conversation | 8am–9pm | Consumer can demand stop | Caps apply to calls, not email/SMS/DM — but this is the public “harassment” line. |
| **Ours: doping/tapping** | **10 (7 daily + days 14/21/28)** | **Daily for a week, then weekly** | **Reply = stop; channel ban = stop that channel; hard stop day 31** | **Same original text. Messenger: delete-and-resend. Email: resend.** |

Verdict on aggressiveness: **aggressive, not average**. Confidence: established.

Sales engagement after a verbal yes is 3–7 mixed touches over 1–3 weeks, with new words and channel changes. SaaS dunning — the closest formal cousin — is 4–7 emails over ~30 days at days 0/3/7/15/20/30. Nobody in those two industries publishes “the same sentence, every day, for seven days.” Week 1 of ours sits on the same numeric line as the US debt-collector phone presumption (7 contacts in 7 days). That law does not bind B2B Telegram DMs, but it is the only published numeric harassment cap in this family of problems.1(https://outreach.io/resources/blog/follow-up-sequences)

Sources opened: Outreach FUP; Apollo sequences; Instantly help + 2026 benchmark; Stripe Smart Retries; Baremetrics Recover + 1M-email dunning table; Chargebee dunning docs; Churnbuster data docs; Recurly retry docs; CFPB FDCPA FAQ.1(https://outreach.io/resources/blog/follow-up-sequences)

---

B. REPLY RATE BY TOUCH (with sources and the fake 80%)

Cold-email numbers are not commitment-collection numbers. They are still the only large public datasets. Dunning recovery is the better analogue for “they already owed us a yes.”

**Cold / sales-engagement**

| Source | What they measured | Number |
|---|---|---|
| Woodpecker (opened) | No follow-up vs ≥1 follow-up | 9% → 13% average; experienced senders 16% → 27% |
| Woodpecker (20M+, cited on their DE post) | Sequence length 1–3 vs 4–7 emails | 9% vs 27% campaign reply |
| Woodpecker (opened) | First follow-up vs opener | ~40% higher reply than the initial email |
| Woodpecker (opened) | Minimum gap | Give at least 2–3 days; too soon = pushy |
| Backlinko + Pitchbox, 12M emails, 2019 (opened) | Baseline reply | 8.5% |
| Same | One extra follow-up | +65.8% replies; 3+ messages best overall; multi-contact +93%; both together +160% |
| Belkins 2026 study, 7.53M emails in 2025 (opened) | Per-step reply | Step 1 highest at 0.59%; steps 2–6 = 58.6% of all replies; step 1 = 41.4% |
| Same | Meetings | Step 3 books more meetings than steps 1+2; steps 3–5 = 53.5% of email meetings |
| Same | Practical stop | 3–5 email steps; after 5, switch channel |
| Same | Other channels | LinkedIn ~7%+ reply; messenger-to-connections 12.22%; cold call connect 18.6%, 33.6% of appointments |
| Instantly 2026 benchmark (opened) | Platform average | 3.43% reply; top quartile 5.5%; elite ≥10% |
| Same | Where replies live | 58% on step 1, 42% on later steps; sweet spot 4–7 touches |
| Whali citing Instantly-shaped table (secondary) | Sequence length vs reply | 1 email 4.1% → 2: 6.64% → 3: 6.94% peak → 4: 5.75% → 5+: 3.01% |
| Gong Engage 2024 (opened help) | Flow emails | Median reply 1.8%; top quartile 3.9%; manual 2.1% vs automated 1.1% |
| Gong, 304k follow-up emails (opened) | Guilt language | “Never heard back” ↑ replies, ↓ meetings 14%; “Hope all is well” ↑ replies and +24% meetings |
| Outreach A/B, 2,000+ per arm (opened) | Bumper with “just checking in” vs without | +86% reply (statistically significant) — this is a short in-thread bump, not a verbatim clone of email 1 |
| Secondary bump study (26k prospects, cited; not a primary PDF I opened) | Opener 2.5% + bump1 +5.2 + bump2 +3.7 | Cumulative 11.4%; next-day follow-up −11% replies; wait 3 days +31% |
| RAIN Group (widely cited; Fluid CRM checked 2026) | Touches to first meeting | Average 8; top performers ~5 |
| InsideSales / XANT 2017, 8,000 companies (opened) | Observed (not optimal) cadence | Median first→second gap ~16h 45m; gap 7→8 ~6d 21h; median first→last <5 days; software median 61 days |
| Older XANT inbound audit | Observed persistence | ~4.47 follow-ups vs a recommended ~12 |

**Dunning recovery (commitment analogue)**

| Source | Schedule / finding | Number |
|---|---|---|
| Baremetrics Recover, >1M dunning emails (opened) | Recovery rate by day delinquent | Day 0: 13.25%; day 3: 11.46%; day 7: 11.51%; day 15: 4.22%; day 20: 3.83%; day 30: 4.20% |
| Same | Open / click decay | Opens 41% → ~27% by day 30; clicks 11% → ~4% |
| Stripe | Smart Retries vs fixed schedule | Stripe’s own claim: +9% revenue vs fixed (third-party recaps; Stripe docs confirm ML timing, default 8 tries / 2 weeks) |
| Recurly | Intelligent retries | Historical ebook: ~70% of failed renewals recoverable; stop at 7 declines / 20 attempts / 60 days |
| Industry recaps (Recova / DunningBee; vendor-interested) | Native Stripe-only vs dedicated recovery | ~30–60% vs ~60–80% with emails + retries; treat as emerging, not gospel |

**The “80% of sales need 5 follow-ups” claim**

Folklore. Fluid CRM chased it on 11 Aug 2026: usually pinned on “The Marketing Donut” or a “National Sales Executive Association.” No study, sample, or year. The named association does not appear to exist. Same infographic family carries “44% quit after one,” “48% never follow up,” “92% quit by the fourth no.” Those also dead-end. Confidence: established that the claim is untraceable. Real substitutes: RAIN ~8 touches to a meeting; Woodpecker / Instantly / Belkins on email steps.2(https://fluidcrm.io/blog/sales-statistics/)

What this means for us: extra touches still harvest a large share of eventual replies (42–59% after email 1). Returns fall after ~4–7 emails. Daily spacing is the opposite of the 2–3 day minimum Woodpecker and the bump studies publish. Guilt-copy (“you didn’t answer”) gets replies that are not meetings. A short bump in-thread can work; a week of identical clones is untested in these datasets. Confidence: established for email; speculative for messenger delete-resend.

---

C. THE DELETE-AND-RESEND TRICK — PRIOR ART, NAME, RISKS

**Did anyone publicly teach this as a sales / CRM move?**

Searches run this session, English and Russian, across growth blogs, sales-engagement posts, Telegram-marketing (CRMChat and similar), GitHub:

- No named playbook “delete your last DM, resend the identical text, restore unread, jump the chat to the top.”
- No standard name. Closest public cousins are email “bump” / “bumper” (reply-to-self in the same thread) and SaaS “dunning.” Crypto-CRM “tapping” looks internal.
- GitHub hits are the opposite job: delete duplicate channel posts, or retry-send bugs that accidentally double-send.
- RU hits are “how to recover a deleted Telegram message,” not “how to weaponize delete.”

Confidence: emerging that it is not documented public prior art. Speculative that nobody in closed crypto ops ever did it — absence of a blog is not absence of a practice.

**Telegram DM behavior (the trick can work here)**

Official Telegram, 24 Mar 2019: delete for both sides in a private chat, messages “disappear for both you and the other person – without leaving a trace.” Official @telegram, 2023: “Deleted messages leave no trace.” TechCrunch tested it in 2019: unlike WhatsApp, **no placeholder is baked into history**. No deletion notification is sent. Push notification of the *old* send can linger on the lock screen after the in-app message is gone. Older app versions may not honor remote delete.3(https://telegram.org/blog/unsend-privacy-emoji)

So on modern Telegram 1:1: delete-for-them + resend identical text ≈ the old bubble vanishes, a new unread appears, the dialog jumps up the chat list. That is the whole mechanic. It is a privacy feature being used as a bump.

**WhatsApp (the trick dies here)**

Delete for Everyone replaces the bubble with **“This message was deleted.”** Recipient sees the tombstone. They may still have the original in notification history. Lifewire / Sendwo / WA help-style writeups all agree the placeholder is visible on both sides. There is a time window (historically short; later writeups say up to ~2 days depending on version — do not treat the hour-count as stable). Group admin deletes are even more labeled.

Using delete-and-resend on WhatsApp does not silently resurface. It advertises “I deleted something, then said the same thing again.” That is worse than a normal follow-up. Confidence: established.

**Unread / notification / mute**

Muted Telegram chats still receive the message. No sound, vibration, or banner. Unread badge for muted chats is a user setting (iOS: Settings → Notifications → Include Muted Chats). A new message still moves an unmuted archived chat back to the main list (Telegram’s own archive rule). So: mute does not kill the list-bump; it kills the interrupt. If they muted you because you already tapped them, daily resends train them to keep you muted. Confidence: established for mute-silences-notify; emerging for “bump still works in muted DMs” (standard list sort, not A/B’d for this tactic).

**Anti-spam / PEER_FLOOD / identical text**

PEER_FLOOD is Telegram’s “you messaged too many people who don’t know you,” usually after burst sends, new accounts, or reports. Client libraries (WTelegramClient FAQ, MadelineProto) treat FLOOD_WAIT_X as a rate-limit sleep and PEER_FLOOD as a spam restriction; PHONE_NUMBER_BANNED / USER_DEACTIVATED_BAN are worse. Grey-hat sender blogs (not Telegram-official) quote corridors like 20–40 new DMs/day on a young account, 50+/hour as burst, 3–5 FLOOD_WAITs in an hour → PEER_FLOOD. Treat those numbers as speculative folklore.

More relevant to *identical repeats to one peer*: generic flood detectors (GetStream “identical” flood, tg-spam `--duplicates.threshold`) flag the same normalized text from one user in a short window. That is group-moderation software, not Telegram’s published DM policy — but it is how operators think about clone-spam. Recipients can still Report → spam. A 7-day identical loop is a report magnet even if it is not PEER_FLOOD.

CRMChat’s public Telegram advice (the opposite of ours): 20–40 new outreach DMs/day on a cold account; 2–3 follow-ups in 7–10 days; stop at 4; “messages within 24 hours of each other” get flagged. Confidence: established that PEER_FLOOD exists and identical clones are a spam shape; speculative on exact same-peer thresholds.

**Other traces**

- Screenshots / notification shade keep the first copy.
- “Delete for me” only (revoke=false) leaves their copy — then resend = duplicate, not a bump.
- Secret chats are a different delete model; do not assume the same.
- If they had already read it, you are not restoring unread in a moral sense — you are manufacturing a new unread of a message they already dismissed.

---

D. ORIGINALITY VERDICT

| Component | Verdict | Best evidence |
|---|---|---|
| (a) The cadence idea: persist after a yes, stop on reply, give-up date | **Known** | Outreach named this “Follow-up Sequences” for prospects who agreed then went dark. Dunning is the same state machine for money. Instantly/lemlist/Woodpecker/Apollo/Salesloft all ship day-based sequences with reply-stop. 7+3+stop-31 is a specific curve, not a new class. |
| (b) Same text, no new words | **Rare** | Email bumpers exist (short “just checking in” in the same thread, Outreach +86% reply). Dunning **changes** copy and turns up loss-aversion. Sales doctrine is “new value each touch.” Verbatim replay of the original agreed message, 10 times, is not how those tools are taught. Closest spirit: Apollo’s “single automated reply to your original email” once, not daily. |
| (c) Delete-and-resend to re-unread and jump the chat list | **Novel as public documented tactic** | Telegram’s no-trace delete is official (2019). WhatsApp’s placeholder is official. Nobody in the sales-engagement or Telegram-marketing corpus I opened teaches combining them into a commitment collector. Founder can fairly claim public-ideologue status for (c), not for (a). |

Confidence: (a) established; (b) emerging; (c) emerging (public record), speculative (private crypto ops).

He is not the inventor of follow-up. He is plausibly the inventor of using Telegram’s privacy-delete as a dunning bump.

---

E. PSYCHOLOGY AND ETIQUETTE

**Why persistence can work (lab, not our A/B)**

- Commitment-consistency (Cialdini; Freedman & Fraser 1966): a public or written yes creates pressure to act consistently. Foot-in-the-door: small-sign yes → 76% vs 17% on the ugly billboard. Deutsch & Gerard: public commitments are stickier than private ones. A call-agreed next step is a real commitment. Reminding them of *their* words is on-theory. Nagging them for ignoring you is a different move. Confidence: established as social-psych; emerging as applied to DM tapping.
- Zeigarnik: unfinished tasks stay more available in memory. An open commitment is an unfinished task. A resurfaced original message can reopen it. A new “hey remember me” starts a different task (managing you). Confidence: established effect, speculative mapping.
- Mere-exposure: repeated stimulus increases liking *up to a point*, then boredom/irritation. Daily identical clones skip the “different instance” that mere-exposure studies usually use. Confidence: speculative that mere-exposure helps here; more likely habituation + mute.
- Reciprocity: they owe a reply because they took the call / you did your part. That is collections psychology, which is why dunning works when the customer still wants the product.

**When it flips negative**

- Belkins: by email 4, spam complaints 0.5% → 1.6%, unsubscribes 0.1% → 2% (cited in 16.5M-email writeups; the 2026 Belkins page I opened emphasizes channel-switch after step 5 rather than reprinting that exact complaint table).
- Woodpecker: 5th–7th follow-up is a fraction of total replies; extra mail burns sender reputation.
- Instantly-shaped curve: reply falls after 3 emails in sequence (6.94% → 5.75% → 3.01%).
- Gong: “I never heard back” buys replies that are not meetings.
- Telegram operators: 2–3 follow-ups tolerated; daily = report.
- FDCPA exists because daily contact *feels* like harassment even when the debt is real.

**Same text vs new value — which wins?**

No study I opened A/B’d “verbatim original, 10 times” against “new value each touch.” What exists:

- In-thread **short bump** (“just checking in”) beat a bump without that phrase by 86% at Outreach — still new words, still one extra sentence, still email.
- Bumper sequences that add a one-liner in-thread lifted 2.5% → 11.4% over two bumps (secondary).
- Dunning: copy **must** change; same template wears out (Baremetrics: “change your email copy periodically”; recovery still happens at day 30 but at ~4%, not 13%).
- Belkins: step 4 uptick when the angle is actually different; they tell you not to write step 3 as a nudge of step 1.
- Gong: empty social phrases move replies, not meetings.

For **post-agreement** (they already said yes), the theoretical winner is: **their original commitment, resurfaced once or twice**, then either a one-line binary (“still doing Thursday, or kill it?”) or a loss-aversion dunning line. Repeating the full original daily fights Zeigarnik (the task feels complete-and-annoying, not incomplete-and-pulling). Confidence: emerging.

---

F. PRIORITIZATION WHEN YOU ONLY HAVE ~5 TOUCHES/DAY

This is a solved product problem. Do not invent a new queue theory.

**How the platforms order work**

- Salesloft Rhythm / Conductor AI: Buyer Engagement Score (meeting/opp in 7 days) + Deal Priority. Cadences have High/Medium/Low. Throttling: daily email cap per cadence or user; **overflow is queued to the next slot, never dropped**. Skip-step exists when the work happened outside the tool.
- Outreach: **Max active prospects per user** + **Max adds per user every 24 hours**. New enrollments wait in pending. Sequence throttle is specifically for staggering starts.
- Apollo: higher **prospect score** moves through the sequence first; when several automated emails are due, **email priority settings** pick which sequence fires; OOO pauses without counting as a reply.
- Klenty: **Cadence throttle** = max new starts/day; **Email priority** = send follow-ups before new first-steps (protect in-flight).
- lemlist: sending speed only throttles **new** step-1 leads; follow-ups share the global daily cap.

**Best practices that map onto a 5/day messenger cap**

1. **Enrollment throttle.** Cap new doping starts per account per day (Outreach “max adds / 24h”). If 5 is the hard anti-ban budget, maybe 1–2 new enrollments, 3–4 in-flight. Dumping 20 people into day-1 on Monday is how you skip days 14/21/28 for everyone.
2. **Tier preemption.** Score = f(deal value or promised $ or strategic worth, recency of the original yes, days left until day 31, channel health, whether they muted/blocked). High-value due-today bumps a low-value day-1 out of today’s 5. This is Salesloft priority + Apollo score, not a new idea.
3. **Missed touches: shift, don’t skip.** If Tuesday’s tap didn’t send because the account was PEER_FLOOD-cooling, do not jump to the Wednesday copy of the same text and “use up” the cadence. Queue it (Salesloft throttle behavior). Skipping daily identical taps is almost a no-op anyway — the content doesn’t change — so a missed day should slide the remaining calendar, except the hard stop at 31 stays a wall.
4. **Follow-ups beat new logos.** Klenty’s email-priority. A day-7 silent-after-yes is hotter than a new cold.
5. **Circuit breakers.** Reply anywhere → stop all channels. Ban/PEER_FLOOD/report on one channel → freeze that channel, do not “make it up” on another the same day. OOO / “I’ll be back 12 Sep” → pause like Apollo.

Confidence: established as product design; our 5/day number is a local constraint.

---

G. THREE CONCRETE RECOMMENDATIONS

**1. Change the curve to dunning, not collections-harassment.**

Replace 7 consecutive daily identical taps with something like:

- Day 0: original (already sent)
- Day 1: delete-and-resend original (Telegram only) **or** a 1-line bumper on email
- Day 3: same
- Day 7: same, or switch to a binary close (“still on, or close the loop?”)
- Days 14 / 21 / 28: weekly
- Hard stop 31, as now

That is Baremetrics 0/3/7/15/20/30 with a messenger bump mechanic. You keep the “same agreed text” principle for the first three resurfaces, then you earn the right to one new sentence. Daily for a week is the part that is both unlike prior art and nearest the FDCPA 7-in-7 line. Confidence: high this is the correct default; we do not have an A/B on doping itself.

**2. Telegram-only for delete-and-resend; never WhatsApp; never after a mute/report.**

WhatsApp placeholder kills stealth and looks like cover-up. If they muted the chat, you are spending a scarce daily slot on a badge they chose not to see — deprioritize. If a send returns PEER_FLOOD / FLOOD_WAIT / they blocked, freeze that rail. Keep a local copy of the original text so you are not depending on the live bubble remaining.

**3. Priority engine: in-flight > new, $ > calendar, shift not skip.**

Daily budget N=5 per messenger account:

- Sort due taps by: (1) any reply-risk / they opened something today, (2) $ or worth tier, (3) days since last successful delivery, (4) days to hard-stop.
- New enrollments: max 1–2/day/account.
- If the account has only 2 slots left, spend them on day-7 and day-14 of high-tier, not on five new day-1s.
- Missed day → delay remaining taps 1 day, do not delete a weekly.

That is Outreach throttle + Klenty follow-up-priority + Salesloft overflow-queue, applied to doping.

---

H. IS THIS POSTABLE AS “BURN IN HELL, IGNORER — MY AI WILL KEEP TAPPING YOU”?

The **delete-and-resend on Telegram** is a real, under-documented mechanic. A technical post that says: they already agreed; we resend their own next-step, not a new pitch; Telegram’s official no-trace delete lets the chat jump unread; WhatsApp’s tombstone makes the same move look like a trick; we stop on any reply and we kill the rail on a ban — that can be published as a novel growth note without looking illiterate. The **cadence** (persist 30 days, stop on reply) is not novel; claiming it is would be the embarrassing part. The **title** is the other embarrassment: it frames a collections harassment fantasy, which is exactly how a journalist or a regulator will read seven daily identical DMs. Dunning vendors brag about recovery rates and changing copy. They do not brag about making the debtor’s chat badge bleed. Post the mechanic, own the WhatsApp failure, show the stop rules, and drop the hell-language — or you look like you discovered dunning and a privacy feature and mistook both for a personality.

---

CONFIDENCE MAP (short)

- Sales/dunning schedules and the 80%-claim autopsy: established
- Reply-by-touch tables: established (email); not transferable 1:1 to Telegram DMs
- Telegram no-trace delete vs WhatsApp placeholder: established
- “Nobody teaches delete-and-resend as tapping”: emerging (public web this run)
- Daily identical clones outperform spaced bumps on messenger: speculative (no study)
- PEER_FLOOD exact same-peer thresholds: speculative
- Psychology mapping to doping: established principles, speculative dose-response
- Priority-queue design: established (copy the SEP products)

DR26-09-02-HUB-01-1307.
