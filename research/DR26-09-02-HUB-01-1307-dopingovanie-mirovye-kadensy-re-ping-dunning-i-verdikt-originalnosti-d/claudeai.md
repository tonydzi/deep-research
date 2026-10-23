# DR26-09-02-HUB-01-1307 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# A. Prior art in sales engagement platforms

**Cadence shape.** Outreach.io: 8–12 touches over 17–21 days, accelerating-then-decelerating spacing (1–2 days early, 3+ days later); throttled by "Max Adds Per User Every 24h" and hard sends caps of 1,000/day–5,000/week per user ([oneshot.ai](https://www.oneshot.ai/blog/how-many-steps-should-you-have-in-your-outreach-sequences), [support.outreach.io](https://support.outreach.io/support/solutions/articles/159000426245-how-to-configure-outreach-sequence-throttles)). Salesloft's own "Big Book of Cadences": aggressive outbound = 8–12 touches/10–14 days; enterprise = 12–16 touches/21–30 days, tapering to 1–2/month nurture after a front-loaded first 10 days ([pages.salesloft.com PDF](https://pages.salesloft.com/rs/432-WAJ-793/images/Big%20Book%20of%20Sales%20Cadences.pdf)). Belkins: recommended ~6 touches over 3 weeks, 3-day spacing giving +31% reply rate ([belkins.io](https://belkins.io/blog/sales-follow-up-statistics)).

**The exact case you're describing — "agreed, then went silent" — has a named product feature.** Apollo.io ships an explicit no-show/stalled-deal re-engagement workflow: 15-Minute Buffer → Day-Of Grace → "Reset With Context" (references the specific momentum that existed) → Calendar-Link Reoffer if step 1 gets no response ([apollo.io](https://www.apollo.io/academy/templates/automatically-hit-no-shows), [stalled-deal template](https://www.apollo.io/academy/templates/follow-up-on-deals-no-progress)). This is the closest formal analog to "доп at": a person made a commitment (booked a call, said yes), then vanished — the industry treats this as a distinct sequence type from cold outreach, with escalating-then-flattening tone, exactly like yours (established).

**Give-up thresholds:** cross-vendor practitioner consensus is 5–8 touches over 2–4 weeks, then archive or 90-day recycle (speculative — no vendor publishes a hard number; Outreach just has a "Finished (No Reply)" terminal state) ([support.outreach.io](https://support.outreach.io/hc/en-us/articles/211861917-Outreach-Sequence-States-Overview)).

**Missed touch handling:** Salesloft does NOT auto-shift the whole calendar on one missed touch — it sits "overdue" until a human/system skips it. The only confirmed date-shift mechanism is bulk "Pause Cadence," which pushes every due step forward by exactly the paused days ([champions.salesloft.com](https://champions.salesloft.com/cadence-7/cadences-the-importance-of-following-the-plan-105)). Your instinct to keep a fixed calendar rather than reflow it around misses matches real vendor behavior.

# Reply-rate-by-touch data (established/emerging, table below)

| Source | N | Finding | Confidence |
|---|---|---|---|
| Backlinko (12M emails) | 12,000,000 | 8.5% reply on 1st send; +65.8% more replies with 1 follow-up; 58%/42% first-email/follow-up split | established |
| Woodpecker (20M) | 20,000,000 | 1st follow-up ~40% more effective than initial; 58%/42% split (same figure, likely cross-cited) | emerging |
| Gong.io | 304,174 emails | Peak reply rate on 1st follow-up (~31%), declining to ~14% by follow-up 8 — steady diminishing returns, no cliff | established |
| Belkins | own client data | 3-step sequence peaks at 9.2% reply, drives 94% of all responses; 2nd follow-up +9%, 3rd follow-up **–20%** | emerging |
| InsideSales/XANT (2014 Lead Response Report) | large industry sample | 12 attempts needed for 95% contact rate; reps average only 1.3 attempts; 30%+ of leads never contacted at all | established |
| "80% of sales need 5 follow-ups" | — | **Debunked.** Traces to an unsourced 1942 door-to-door survey of <40 Long Island salesmen; the associated 44/22/14/12% quit-curve has no traceable primary source at all; "National Sales Executive Association" citation is unverifiable | established-as-myth |

Source: [conciergr.com — the 80% myth](https://conciergr.com/blog/80-percent-sales-follow-up-myth), [Gong — 7 tips](https://www.gong.io/blog/7-tips-for-writing-the-perfect-follow-up-sales-email-according-to-science), [InsideSales 2014 report PDF](https://resources.insidesales.com/wp-content/uploads/2019/11/2014-Lead-Response-Report.pdf).

# B. Dunning / collections

| Product | Attempts | Spacing | Total window | Recovery rate claimed | Recovery rate observed independently |
|---|---|---|---|---|---|
| Stripe Smart Retries | up to 8 | ML-chosen, or fixed 1/3/5/7-day custom schedule | 2 weeks default | ~55% (Stripe marketing) | 25–35% (Redux Payments audit of 200+ accounts) |
| Chargebee | up to 12 (Smart) / 5 (Custom) | auto or custom day-list, e.g. "1,4,8" | max 45 days/step | not published | — |
| Churnbuster | not fixed | reason-adaptive | 25–28 days default | ~15–21% clear before first email even sent; ~50–60% for a 5%-churn cohort | — |
| Baremetrics Recover | — | — | — | 45–70% (marketing) | **12.7% median** across 119 real B2B SaaS accounts, May 2026 |
| Recurly | — | — | — | 53% monthly / 23% annual via retries alone; ~70% combined with dunning email + card updater | — |

Sources: [Stripe docs](https://docs.stripe.com/billing/revenue-recovery/smart-retries), [Redux Payments audit](https://www.reduxpayments.com/blog/stripe-smart-retries-explained), [Chargebee docs](https://www.chargebee.com/docs/payments/2.0/others/dunning_retries), [Churnbuster docs](https://churnbuster.io/docs/retries/), [Baremetrics benchmark study](https://baremetrics.com/blog/subscription-payment-recovery-benchmarks), [Recurly](https://recurly.com/blog/failed-payment-recovery-data-based-strategy/).

**Important pattern in this table (worth flagging for your own reporting, per house rule §5.4-бис "выгодный вывод → усиленная проверка"):** every vendor's *marketed* recovery rate is 2–5x higher than the one *independently measured* number we found (Baremetrics 45–70% claimed vs 12.7% median measured; Stripe 55% claimed vs 25–35% audited). If you publish recovery-rate numbers for your own "допинг" mechanism later, measure them yourself — don't trust a self-reported percentage, including your own.

**Regulation.** US Reg F (12 CFR §1006.14(b), CFPB, effective 2021) sets a hard numeric cap: **no more than 7 calls in 7 consecutive calendar days** about a debt, and no call within 7 days after an actual live conversation about it. This cap covers **phone calls only** — email and text are explicitly excluded from the numeric limit ([Cornell Law text](https://www.law.cornell.edu/cfr/text/12/1006.14), [CFPB FAQ](https://www.consumerfinance.gov/compliance/compliance-resources/other-applicable-requirements/debt-collection/debt-collection-rule-faqs/)). UK/FCA has no numeric cap, only a principles-based "reasonable frequency, stop if not producing engagement" standard ([FCA/Ofgem/Ofwat/Ofcom joint statement 2024](https://ukrn.org.uk/app/uploads/2024/03/UKRN-Joint-Debt-Collection-Statement-March-2024.pdf)).

**Verdict on your schedule (7 daily + 3 weekly, hard stop day 31):** your week-1 cadence lands *exactly* at the FDCPA phone-call ceiling (7 in 7 days) even though the cap wouldn't legally apply to you at all (you're not collecting debt, and messengers/email aren't covered by the numeric cap anyway). Weeks 2–4 (1/week) are far softer than anything regulation restricts. Against SaaS dunning, your total window (31 days) sits almost exactly on Churnbuster's 25–28-day default, and your touch count (10) is between Chargebee's Custom-mode ceiling (5) and Smart-mode ceiling (12). **Overall: average-to-firm, not aggressive** — you're inside every real-world and legal benchmark found, just front-loaded harder in week 1 than typical SaaS dunning (which usually tapers within days, not stays daily for 7).

# C. The delete-and-resend trick

**Nothing found as a named/published technique**, in EN or RU, across growth-hacking forums, BlackHatWorld, Reddit (r/telegram, r/sales, r/growthhacking), GitHub, sales-engagement blogs, or RU Telegram-marketing sites (vc.ru, sendsay, wazzup24, mango-office). What exists is (a) Telegram's own "delete and resend" framed purely as message *correction*, not a bump tactic ([telegram.org/blog/unsend-and-usage](https://telegram.org/blog/unsend-and-usage)), and (b) bulk-delete userbots (`delethon`, `wipemychat`) built for privacy cleanup, not follow-up ([github.com/BingLingGroup/delethon](https://github.com/BingLingGroup/delethon)). This is a genuinely thin literature search, so treat "nothing found" as "not indexed in public English/Russian sources we could reach" rather than absolute proof no one has ever done it privately.

**Mechanics, verified:**
- **Telegram:** delete-for-everyone in a 1-on-1 chat leaves zero trace on the recipient's side, no time limit, since v5.5 (2019) ([TechCrunch](https://techcrunch.com/2019/03/25/going-going-gone/)).
- **WhatsApp: the trick is dead there.** "Delete for Everyone" leaves a visible "This message was deleted" stub on the recipient's side — confirmed across independent sources ([gbyte.com](https://www.gbyte.com/blog/whatsapp-delete-for-everyone), [Bitdefender](https://www.bitdefender.com/en-us/blog/hotforsecurity/whatsapp-delete-for-everyone-feature-potentially-puts-user-privacy-at-risk)). Your architecture already special-cases this correctly by only doing delete+resend in Telegram — good, the research confirms that's the right call, not just caution.
- **Unread/reorder does happen** — Telegram's dialog list is ordered by `top_message` (newest message ID), so a resend genuinely moves the chat to the top and re-arms the unread badge, because the recipient hasn't opened the new message ID yet ([core.telegram.org/constructor/dialog](https://core.telegram.org/constructor/dialog)). This is just default new-message behavior, not a special exploit — which also means it can't be patched away as a "bug," it's structural.
- **Muted chats:** badge/counter still updates, notification banner is suppressed — standard behavior, no evidence resend does anything different.
- **PEER_FLOOD risk is real and specifically triggered by identical repeated text.** Telegram's spam detection compares message content across recipients; sending the same string repeatedly "will almost certainly trigger a PeerFlood restriction" per multiple outreach-tool postmortems ([crmchat.ai](https://crmchat.ai/blog/telegram-bulk-messaging-limits-risks)). Exact thresholds are undocumented by Telegram itself — Telethon/Pyrogram maintainers say limits are opaque and variable ([Telethon issue](https://github.com/LonamiWebs/Telethon/issues/398)). Your founder's empirical ~30 taps/24h cap (2022, keyword-derived) is therefore the actual state of the art here — nobody has published Telegram's real threshold, so a hand-calibrated cap from account-ban experience is about as good as it gets.

# D. Originality verdict

- **(a) The cadence idea** (re-ping after a broken commitment, front-loaded then tapering, hard stop) — **known, well-documented prior art.** Apollo's no-show/stalled-deal sequences are functionally the same idea; dunning cadences are the same idea applied to money. You did not invent "keep pinging someone who agreed and vanished." Evidence: strong (Apollo product docs + entire dunning industry).
- **(b) "Same text, no new words" principle** — **rare, genuinely under-documented, and contrarian to industry consensus.** Every sales-engagement source found (Gong's dataset, lemlist, Woodpecker) explicitly recommends *varying* the message and adding new value per touch; short reminder-only pings are "15x less likely" to convert per Gong. Nobody publishes data on literal verbatim resend as a strategy — it's not that it's proven wrong, it's that it's untested/unaddressed in the literature. This is your most defensible originality claim, but note it cuts against the data-backed consensus, not with it — your reasoning (it's not "did you see this", it's re-presenting their own exact words per Cialdini commitment-consistency) is a plausible mechanism nobody has published, but it is a bet against the house data, not a confirmed edge.
- **(c) The delete-and-resend mechanic** — **novel as a named/published technique**, best evidence: an actual empty search result across EN+RU forums, blogs, GitHub, Reddit, BlackHatWorld. This is the strongest "original ideologue" claim of the three, though weakly evidenced (absence-of-evidence, not evidence of true uniqueness) and it only works on one platform (Telegram, not WhatsApp).

**Overall: not an original ideologue on (a); a rare/untested idea on (b); genuinely undocumented on (c).** The honest story is "invented an undocumented mechanical trick and combined it with a well-known cadence discipline" — not "invented follow-up persistence."

# E. Psychology

- **Mere-exposure (Zajonc):** established core effect, but a meta-analysis shows it **peaks at 10–20 exposures and can reverse (reduce preference) beyond that** — this is a real ceiling on your cadence design, not just a green light ([Wikipedia, citing meta-analysis](https://en.wikipedia.org/wiki/Mere-exposure_effect)).
- **Zeigarnik effect:** **weaker than commonly believed.** A 2025 systematic review/meta-analysis found **no memory advantage for unfinished tasks at all**, and the "unkept promise = open loop" framing you're relying on has zero direct research support — it's folk-psychology extrapolation, not science ([Wikipedia](https://en.wikipedia.org/wiki/Zeigarnik_effect)). Don't cite Zeigarnik as evidence in a blog post; a knowledgeable reader will call it out.
- **Cialdini commitment-consistency:** established core principle, and practitioner sources do support "quote their own words back to them" as stronger than a generic nudge (self-justified commitments are processed as near-absolute by the person who made them) — this is your best-grounded psychological mechanism, though still theory + anecdote, not a controlled reply-rate study ([SalesGravy](https://salesgravy.com/professional-selling-the-secret-power-of-commitment-questions-part-1/)).
- **When persistence backfires:** Gong's 304K-email dataset shows **steady decline, not a cliff** (31%→14% reply rate by touch 8) — no sudden backlash point, just diminishing returns. Secondary-sourced (not independently verified) claims that unsubscribe/spam-flag rates roughly triple by touch 4 and triple again by touch 5 ([Instantly.ai aggregation](https://instantly.ai/blog/how-many-times-should-you-really-follow-up-with-a-prospect/)) — treat as emerging, not established.
- **Verbatim vs. varied message:** no direct A/B data exists comparing exact-repeat vs. new-value touches. The surrounding data (Gong: reminder-only pings are 15x worse than substantive follow-ups) points against verbatim resend on *content grounds* — but note your mechanic isn't relying on new content, it's relying on the delete+resend *delivery mechanic* (unread, top-of-list) doing work that new copy would otherwise have to do. That's a real distinction the data doesn't directly address either way.

# F. Prioritization under capacity

Real systems gate at **enrollment**, not mid-queue eviction: Outreach's "Max Adds Per User Every 24h" throttle caps how many new prospects enter a sequence per day; no vendor documentation was found describing a high-value lead literally bumping a lower one out of an already-full daily send window ([support.outreach.io](https://support.outreach.io/support/solutions/articles/159000426245-how-to-configure-outreach-sequence-throttles)). Score-gated tiering is the closest real pattern: hot leads → immediate real-time send, warm → automated sequence, cold → passive/low-frequency track — banding by threshold, not runtime preemption. On missed touches: Salesloft explicitly does **not** auto-reflow the whole calendar on one miss — it stays "overdue" until skipped; the only confirmed date-shift mechanism is bulk "Pause Cadence," which shifts every step forward by exactly the paused days ([champions.salesloft.com](https://champions.salesloft.com/cadence-7/cadences-the-importance-of-following-the-plan-105)).

---

## Deliverable

**1) Comparison table of cadences**

| System | Touches | Spacing | Stop rule |
|---|---|---|---|
| Your "допинг" | 10 | daily ×7, then weekly ×3 | hard stop day 31 or any reply |
| Apollo no-show sequence | 4 steps | 15min → same-day → next → after-non-response | not published |
| Salesloft aggressive | 8–12 | 1–3 days, taper after day 10 | 10–14 days |
| Belkins recommended | ~6 | 3-day spacing | 3 weeks |
| Stripe Smart Retries default | up to 8 | ML-chosen | 2 weeks |
| Chargebee Custom | up to 5 | custom day-list | 45 days/step |
| Churnbuster default | not fixed | reason-adaptive | 25–28 days |
| FDCPA Reg F (legal ceiling, phone only) | ≤7 calls | within 7 days | 7-day cooldown after live convo |

**2) Reply-rate-by-touch:** see table in section A above (Gong: 31%→14% over 8 touches; Belkins: touch 3 costs you ~20%; InsideSales: 12 attempts for 95% contact rate but reps average 1.3).

**3) Delete-and-resend prior art:** none found published; mechanics confirmed real on Telegram, confirmed dead on WhatsApp (placeholder). See section C.

**4) Originality verdict:** cadence = known; verbatim-text principle = rare/untested; delete-resend mechanic = novel (best available evidence: a genuine empty search). See section D.

**5) Three concrete recommendations for your schedule and priority engine:**
1. **Cap week-1 density or accept you're at the exact legal-analogy ceiling.** 7 daily touches matches the FDCPA 7-in-7 cap almost coincidentally — fine legally (doesn't apply to you), but the mere-exposure ceiling (10–20 exposures before it *reverses*) means don't extend the daily phase past day 7, and don't add more channels in week 1 — you're already near the saturation point the research flags.
2. **Build tier-based enrollment gating, not mid-queue eviction**, since that's the only pattern with real vendor precedent: when your ~5/day/account messenger cap is hit, decide who gets a NEW doping cycle started today by lead-value score, and let already-running cycles for lower-tier contacts finish on their existing calendar rather than getting bumped — this matches Outreach's actual throttle design and avoids you inventing an untested eviction mechanic.
3. **Instrument recovery rate yourself before quoting one anywhere.** Every dunning vendor's marketed recovery % was 2–5x its independently-measured number (Stripe 55% claimed vs 25–35% audited; Baremetrics 45–70% claimed vs 12.7% median measured). If "допинг" gets a public number attached (blog post, pitch, internal metric), measure actual reply-after-doping rate from your own logs, don't estimate it — per house rule on self-serving conclusions needing stronger proof, this is exactly the kind of favorable number that needs independent verification before it goes external.

**6) Postability verdict:** This is genuinely postable, but the honest angle is narrower than "I invented a growth trick." The cadence idea (re-ping the silent after they agreed) is well-trodden — Apollo and every dunning vendor already do it, so leading with "nobody does this" would embarrass you with anyone who's used Outreach or Stripe billing. What's actually novel and worth the blog post is the **specific mechanic**: deleting and re-sending identical text to defeat Telegram's read-state and re-arm the unread badge, deliberately paired with sending the exact same words back (a crude but real application of commitment-consistency — "these are YOUR words, not mine") instead of manufacturing new content — and the honest caveat that it's a one-platform trick (dead on WhatsApp) that risks PEER_FLOOD if you don't rate-limit it, which is presumably why the 2022 version capped at ~30 taps/24h. Frame the post as "here's an undocumented Telegram mechanic + why we bet against the industry's 'always add new value' consensus," not "I discovered persistence works" — the second framing is where you'd embarrass yourselves.
