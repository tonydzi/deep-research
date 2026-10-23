# DR26-07-04-ZB-03 — raw report from scoring

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Dynamic Lead Temperature and Reactivation Scoring for a Founder-Louted Relationship Sales Motion

## Executive takeaway

For your situation, the best model is **not** a classic SaaS MQL score and **not** a black-box predictive score. The highest-leverage design is a **hybrid band-plus-rank model**:

1. **Use fit as a gate, not as the main score.** Once a contact is plausibly in-scope for AI development, team-hire, or venture-network value, stop debating whether they are an 82 or an 84 in “ICP.” Recent buyer intelligence matters more than static fit for deciding who deserves founder attention **now**. Common Room’s 2026 account-prioritization guidance is explicit on this: fit should filter; live signals, recency, and buying-group momentum should decide priority. citeturn13view0

2. **Store temperature as a small set of operational bands, not a fake-precise 0–100 status.** Salesforce’s current SMB guidance explicitly recommends simple thresholds such as hot/warm/cold for small teams, and modern scoring systems from HubSpot and Salesforce separate **fit** from **engagement** rather than forcing everything into a single opaque number. citeturn3view5turn14view0turn14view1

3. **Rank old contacts for reactivation with a recency-frequency-depth-resurgence model.** Recent best practice in signal-based GTM is to weight **recency, frequency, and depth**, then apply **signal decay**, with recent signals carrying meaningfully more weight than 30–60 day-old activity. In parallel, recent RFM extensions in the academic literature keep showing that adding business-specific extra dimensions beyond plain RFM improves response prediction. citeturn13view0turn3view1turn22academia19

4. **Use rules first, LLM second.** For reply/no-reply, message counts, who wrote last, unanswered streaks, last contact date, call count, and community activity, deterministic rules are enough. Use an LLM only for the narrow tasks rules struggle with: ambiguous sentiment, “not now, circle back in September” extraction, referral detection, or detecting buying intent hidden in free text. Research on enterprise email reply behavior found that historical interaction features and temporal features dominate reply prediction, while vendors like Outreach use ML for sentiment because that layer is less reliably captured by rules. citeturn25view0turn17view0

5. **Do not start with predictive ML trained on your current CRM labels.** Your statuses are already stale, which means your labels are dirty. Microsoft’s predictive lead scoring requires a minimum recent set of qualified and disqualified labels, and Salesforce recommends substantial engagement history plus opportunity-linked examples before AI scoring works well. Dirty labels plus low outreach throughput is the perfect setup for a confident but wrong model. citeturn3view4turn15view1

The practical recommendation is therefore: **derive temperature automatically from events, maintain a separate reactivation rank for the dormant pool, and let assistants/AI agents work from explainable factor lists rather than from one magic number.** HubSpot’s current scoring tools explicitly support score histories, event-driven updates, and factor breakdowns; that is the right operational pattern. citeturn14view2turn14view3

## What current best practice converges on

Modern lead scoring has moved away from static demographic-only ranking and toward **dynamic, event-driven prioritization**. HubSpot now separates **fit scores**, **engagement scores**, and **combined scores**. Microsoft’s current scoring model builder also separates demographic attributes from interaction signals and supports positive or negative points within defined time windows. Salesforce’s Einstein Behavior Scoring explicitly bakes in **score decay**, emphasizes **recency**, and shows positive and negative predictive factors rather than keeping the model fully opaque. citeturn14view0turn14view1turn3view3turn15view1

For relationship-based B2B sales, the useful shift is from “lead qualification” to **timing qualification**. Common Room’s current guidance is blunt: the question is not which accounts could buy one day, but which ones are worth working **this week** because behavior says the window is open now. Their 2026 playbook explicitly recommends evaluating **recency, frequency, and depth**, then decaying older signals; it also recommends priority bands such as “today / this week / this month” instead of static tiers. citeturn13view0

That matches your Telegram-driven reality almost perfectly. You have a founder-led motion with extremely limited outbound volume, so the model’s job is not to maximize top-of-funnel coverage. Its job is to answer a much narrower question: **if the founder can only send 3–5 high-context outbound messages per day, who is most likely to respond positively now?** In that setup, community resurfacing, recent bilateral history, and conversation depth matter far more than broad marketing-style intent proxies. Common Room’s signals documentation now treats community and social activity—messages, replies, reactions, comments, mentions, and role signals—as first-class GTM inputs precisely because these signals reveal timing and context that static CRM fields do not. citeturn16view0

Recent vendor practice also converges on **trend visibility** and **explainability**. HubSpot’s current score history shows six months of score changes and the events that moved the score; HubSpot deal scores now expose key factors and trajectory, not just a number. That is important here because assistants and AI agents need to know *why* a lead is rising or cooling: “replied twice last month,” “founder wrote last 19 days ago,” “posted in shared chat yesterday,” “last call was 420 days ago.” A founder queue built on explainable factors is much safer than one built on latent weights nobody trusts. citeturn14view2turn14view3

## Comparison of scoring models

| Model | Core logic | Best use case | Strengths | Main failure mode in your case | Verdict |
|---|---|---|---|---|---|
| **Static lifecycle/status model** | Manual stage such as new / contacted / qualified / dormant | Small pipelines with clean, frequent manual updates | Easy to understand; low setup cost | Your statuses are already stale; thousands of “new” leads have call history, so the label becomes fiction quickly | **Reject as primary model**. Keep statuses only as downstream outcomes, not as the source of truth. HubSpot still supports lifecycle stages, but that is not enough for dynamic prioritization. citeturn7search8turn14view2 |
| **Fit + engagement score** | Separate fit from behavioral engagement; optionally combine | General CRM prioritization | Matches current CRM best practice; supports positive and negative event scoring; explainable | Still too generic if you do not model relationship depth, bilateral history, and reactivation signals from chats | **Good baseline**, but incomplete for a founder-network motion. Officially aligned with HubSpot/Microsoft/Salesforce. citeturn14view0turn14view1turn3view3turn15view1 |
| **RFM-style reactivation model** | Rank by recency, frequency, and a business-specific third/fourth factor | Large dormant base reactivation | Excellent for sorting old contacts when outreach capacity is tiny; easy to compute nightly; robust to messy pipeline labels | Plain RFM ignores conversation quality and network resurfacing unless extended | **Strong fit** if extended to RFD or RFDE. Recent literature and signal-based GTM practice support adding extra dimensions beyond classic RFM. citeturn3view1turn22academia19turn13view0 |
| **Supervised predictive / AI model** | Train on historical won/lost or qualified/disqualified labels | Mature orgs with clean outcomes and enough recent labels | Can find nonlinear patterns and update automatically | Your labels are dirty, your outreach volume is low, and the training target is not yet trustworthy | **Do later, not now**. Microsoft and Salesforce both require meaningful recent training data before predictive scoring is reliable. citeturn3view4turn15view1 |

The current market signal is clear: for a small, high-touch team, **simple operational bands plus dynamic signal inputs beat elaborate static scores**. Salesforce’s current guidance for small businesses literally recommends starting with a small number of behavior-based signals, factoring in recency, and using simple thresholds such as hot/warm/cold. citeturn3view5

## Recommended hybrid model for your case

The model I would implement is a **Hybrid Relationship Heat model** with two outputs per person:

- **Temperature band** for operational state.
- **Reactivation priority score** for sorting the dormant pool.

This is still one coherent model. The band tells you **what state the relationship is in**. The rank tells you **which dormant people deserve the next founder message first**.

### Temperature bands and transition rules

Use **five bands** plus one exclusion state:

| Band | Meaning | Typical criteria to enter | Typical criteria to decay down |
|---|---|---|---|
| **Hot** | Live bilateral momentum right now | Positive reply in last 30 days; call held in last 45 days; 2-way Telegram exchange in last 21 days; founder has an open next step and the SLA is still live | No bilateral touch for 21–30 days; founder wrote last and no reply after follow-up window; explicit “not now” |
| **Warm** | Recent relationship energy, but no immediate next step | Reply in last 90 days; call in last 90 days; at least 2 meaningful replies in last 180 days; recent shared-chat activity plus known history | No bilateral touch for 60–90 days |
| **Lukewarm** | Known relationship with depth, but mostly historical | Had call(s) historically; replied before; nontrivial thread depth; occasional resurfacing in shared chats | No meaningful bilateral touch for 180+ days and no recent resurfacing |
| **Cold** | Known contact, mostly dormant | Contact exists; little recent bilateral engagement; maybe one old call or weak past exchange | Explicit negative outcome, invalid record, or repeated non-response moves to archive; otherwise remains cold |
| **Archived** | Do not work unless new signal appears | Explicit “not interested,” wrong person, bounced/invalid, blocked, or policy exclusion | Can return only on strong new inbound or trusted network signal |
| **Excluded** | Not in your serviceable universe | Irrelevant persona/company/context | N/A |

This is intentionally compact. Current vendor practice supports keeping the model simple, separating fit from engagement, and allowing negative scoring. Microsoft’s scoring builder explicitly supports positive and negative points within time windows; Salesforce uses built-in score decay and recent-over-old weighting; HubSpot supports fit, engagement, and combined models with negative point rules. citeturn3view3turn15view1turn14view1

**Promotion rules** should be event-driven, not manual. A lead should move up when one of four things happens: a direct reply, a direct call, a lead-initiated touch, or a resurfacing signal from a shared environment that occurs on top of prior relationship history. That last rule matters in your motion because community/channel activity often reveals that the person is in a fresh cycle of attention. Common Room’s current signal architecture and account-prioritization guidance treat cross-channel engagement and person-level activity as the right trigger substrate for this kind of prioritization. citeturn16view0turn13view0

**Decay rules** should be automatic. A modern score should not be frozen at the level it reached the day someone once filled a form or took a meeting. Salesforce explicitly notes that recent activities should score higher than older ones and that score decay is built into behavior scoring; Common Room’s 2026 account-prioritization guidance says signals from the last seven days should materially outrank 30–60 day-old events and that older signals should decay unless engagement continues. citeturn15view1turn13view0

Use an exponential decay function for every time-sensitive signal:

```text
decayed_value = raw_weight * 2 ^ (-days_since_event / half_life_days)
```

A good starting configuration for your motion is:

- **Direct positive reply:** half-life **45 days**
- **Completed call / meeting:** half-life **60 days**
- **Lead-initiated DM:** half-life **75 days**
- **Shared-chat post / reply / reaction:** half-life **21–24 days**
- **Historical relationship memory** such as prior calls and distinct interaction months: half-life **365 days**
- **Unanswered founder outbound penalty:** half-life **30 days**

Those exact numbers are implementation defaults, not universal law. The reasoning is straightforward: modern scoring systems weight recency heavily, and Common Room’s current advice that 7-day activity should outrank 30–60 day activity by roughly a few multiples implies a transient-signal half-life in the low-20-day range; bilateral relationship memory should decay much more slowly because trust survives longer than a surface signal does. citeturn15view1turn13view0

### Reactivation score and decay logic

For the old called-contact pool, use a separate **Reactivation Priority** score based on **Recency, Frequency, Depth, and Resurgence**, with penalties for unresolved silence and explicit negatives.

A practical formulation is:

```text
ReactivationPriority
  = 0.35 * Recency
  + 0.20 * Frequency
  + 0.25 * Depth
  + 0.20 * Resurgence
  - Penalties
```

Where:

```text
Recency
  = 100 * max(
      2 ^ (-days_since_last_reply / 90),
      0.7 * 2 ^ (-days_since_last_call / 120)
    )

Frequency
  = min(100, 18 * log2(1 + total_calls + meaningful_reply_threads))

Depth
  = min(100,
      10 * distinct_months_with_bilateral_interaction
    + 8  * max_roundtrip_turns_in_any_thread
    + 12 * lead_initiated_threads
    + 10 * completed_calls
    )

Resurgence
  = min(100,
      60 * 2 ^ (-days_since_shared_chat_activity / 24)
    + 40 * 2 ^ (-days_since_shared_chat_spike / 14)
    )

Penalties
  = 15 * unanswered_outbound_streak
    + 25 * explicit_negative_recent
    + 15 * explicit_not_now_until_future_date
```

This is just RFM adapted to a relationship network. The “M” in classic RFM is not useful at the lead stage for you, so replace it with **Depth**. Recent academic work keeps extending RFM with extra dimensions such as diversity or engagement because three-variable RFM is often too narrow for real response prediction; that logic applies directly here. citeturn3view1turn22academia19

The key inputs should be defined using **meaningful bilateral interaction**, not any contact at all. For your database, I would define “meaningful” as one of: a completed call, a reply from the lead, a lead-initiated message, or a shared-chat resurfacing event that occurs after an already established founder relationship. This avoids promoting contacts merely because the founder sent messages into the void. That distinction aligns with modern buyer-intelligence practice, which prioritizes real engagement from identified people and deprioritizes static list membership or superficial activity. citeturn13view0turn16view0

Because founder capacity is constrained, use **priority bands on top of the reactivation score**:

- **Now:** score ≥ 75
- **This week:** 60–74
- **This month:** 45–59
- **Park:** <45

That mirrors current signal-based GTM practice better than long deciles or percentile ranks. A founder and assistants can act on “now / this week / this month” far more reliably than on “72 vs 68.” citeturn13view0turn3view5

### Pseudocode rules

```python
from math import log2

def decay(days: float, half_life: float) -> float:
    return 2 ** (-days / half_life)

def compute_temperature(contact):
    # deterministic raw facts
    days_reply = contact.days_since_last_reply
    days_call = contact.days_since_last_call
    days_bidirectional = contact.days_since_last_meaningful_bilateral_touch
    days_shared_chat = contact.days_since_last_shared_chat_activity
    founder_wrote_last = contact.founder_wrote_last
    unanswered_days = contact.days_since_last_founder_message_if_unanswered
    explicit_negative = contact.explicit_negative_recent
    explicit_not_now = contact.explicit_not_now_until_future_date
    invalid = contact.invalid_or_blocked

    if invalid or explicit_negative:
        return "Archived"

    # live engagement layer
    live = 0
    if days_reply is not None:
        live += 40 * decay(days_reply, 45)
    if days_call is not None:
        live += 30 * decay(days_call, 60)
    if contact.lead_initiated_last_days is not None:
        live += 20 * decay(contact.lead_initiated_last_days, 75)
    if days_shared_chat is not None:
        live += 10 * decay(days_shared_chat, 24)

    # friction / cooling
    friction = 0
    if founder_wrote_last and unanswered_days is not None:
        friction += 20 * decay(unanswered_days, 30)
    if explicit_not_now:
        friction += 25

    score = live - friction

    # band cutoffs
    if score >= 45:
        return "Hot"
    if score >= 25:
        return "Warm"
    if contact.has_any_prior_call or contact.total_meaningful_reply_threads > 0:
        if days_bidirectional is not None and days_bidirectional <= 180:
            return "Lukewarm"
        return "Cold"
    return "Cold"


def compute_reactivation_priority(contact):
    if contact.invalid_or_blocked or contact.explicit_negative_recent:
        return 0

    recency = 0
    if contact.days_since_last_reply is not None:
        recency = max(recency, 100 * decay(contact.days_since_last_reply, 90))
    if contact.days_since_last_call is not None:
        recency = max(recency, 70 * decay(contact.days_since_last_call, 120))

    frequency = min(
        100,
        18 * log2(1 + contact.total_calls + contact.total_meaningful_reply_threads)
    )

    depth = min(
        100,
        10 * contact.distinct_months_with_bilateral_interaction +
        8  * contact.max_roundtrip_turns +
        12 * contact.lead_initiated_threads +
        10 * contact.completed_calls
    )

    resurgence = 0
    if contact.days_since_last_shared_chat_activity is not None:
        resurgence += 60 * decay(contact.days_since_last_shared_chat_activity, 24)
    if contact.days_since_shared_chat_spike is not None:
        resurgence += 40 * decay(contact.days_since_shared_chat_spike, 14)
    resurgence = min(100, resurgence)

    penalties = (
        15 * contact.unanswered_outbound_streak +
        25 * int(contact.explicit_not_now_until_future_date) +
        25 * int(contact.explicit_negative_recent)
    )

    score = 0.35 * recency + 0.20 * frequency + 0.25 * depth + 0.20 * resurgence - penalties
    return max(0, min(100, round(score, 1)))


def daily_founder_queue(contacts):
    scored = []
    for c in contacts:
        c.temperature = compute_temperature(c)
        c.reactivation = compute_reactivation_priority(c)

        eligible = (
            c.fit_gate_passed and
            not c.invalid_or_blocked and
            not c.explicit_negative_recent and
            c.days_since_last_founder_outbound >= 14
        )

        if not eligible:
            continue

        founder_priority = (
            100 if c.temperature == "Hot" else
            80  if c.temperature == "Warm" else
            0
        ) + c.reactivation

        scored.append((founder_priority, c))

    return [c for _, c in sorted(scored, key=lambda x: x[0], reverse=True)[:5]]
```

This is intentionally conservative. It is easy to explain, easy to backtest, and easy to recalibrate once you have a few hundred real reactivation attempts labeled with outcomes such as **positive reply**, **meeting booked**, or **opportunity created**. That matches current CRM scoring practice much better than jumping directly to predictive ML on dirty outcome labels. citeturn3view4turn15view1turn14view2

## Automatic classification from Telegram and call history

The most predictive features for reply behavior are usually **historical interaction features** and **temporal features**, not fancy semantics. In Microsoft’s enterprise email reply study, the strongest drivers for reply action were historical interaction features; temporal features were strongest for reply-time prediction. The most important feature groups included prior interaction history, message/body characteristics, and timing. citeturn25view0turn12view0

That means the following Telegram-derived features should be treated as **deterministic first-class inputs**:

| Feature | Use rules or LLM | Why it matters |
|---|---|---|
| Ever replied | Rules | Strong boundary between known dormant relationship and one-sided outreach history; historical interaction matters most for reply action. citeturn25view0 |
| Reply count total | Rules | Frequency proxy; historical counts repeatedly appear among top predictive feature groups. citeturn25view0 |
| Days since last reply | Rules | Core recency signal; current scoring systems and GTM playbooks heavily weight recency and decay. citeturn15view1turn13view0 |
| Days since last call | Rules | Strong relationship-memory signal for high-touch sales; should decay slower than chat activity. This is an inference from modern recency-weighted scoring and your motion. citeturn15view1turn13view0 |
| Who wrote last | Rules | If founder wrote last and silence is stretching, the thread is cooling; conversation research shows long reply delays are associated with conversation ending. citeturn10view1 |
| Unanswered outbound streak | Rules | Negative scoring input; modern tools support subtracting points and using negative factors. citeturn14view1turn15view1 |
| Thread depth | Rules | Back-and-forth depth is a relationship-quality proxy; recent practice explicitly recommends evaluating depth, not just activity count. citeturn13view0 |
| Lead-initiated thread count | Rules | High-value relationship signal in founder-led sales; not every touch is equally informative. This is an inference, but it aligns with the broader historical-interaction evidence base. citeturn25view0 |
| Shared-chat activity in last 14/30 days | Rules | Resurfacing signal; modern buyer-intelligence stacks treat community and social activity as actionable GTM signals. citeturn16view0 |
| Shared-chat spike after long silence | Rules | Especially useful for reactivation because it signals renewed attention without direct outbound from you. Signal-based GTM explicitly centers on fresh behavioral windows. citeturn13view0turn16view0 |

Where an LLM is genuinely useful is narrower:

- **Latest inbound intent classification:** positive interest, objection, referral, unsubscribe-like rejection, or “not now.” Outreach uses ML for exactly this type of sentiment/intent classification and reports 80–90% top-level accuracy in its supported languages. citeturn17view0
- **Deferral extraction:** detect phrases like “after summer,” “Q4,” “once funding closes,” “when we hire CTO,” and convert them into structured snooze dates.
- **Referral extraction:** detect “speak to X instead.”
- **Opportunity-topic detection:** AI development request, team augmentation, intro request, investment context, partnership, recruiting, or generic networking.
- **Ambiguous tone resolution:** short replies such as “interesting,” “maybe later,” or “let’s revisit” are where rules fail and an LLM pays for itself.

Where an LLM is **wasteful**:

- counting messages
- deciding whether a reply exists
- deciding who sent the last message
- counting calls
- detecting unanswered streaks
- measuring time since last interaction
- detecting public-chat activity counts

Those are database operations, not reasoning tasks. Using an LLM there adds cost and error for no gain. The right split is: **structure with rules, meaning with AI**. That split is consistent with both modern CRM scoring design and communication-prediction research. citeturn14view1turn25view0turn17view0

A good minimal schema for each person is:

```json
{
  "fit_gate_passed": true,
  "calls_total": 3,
  "calls_completed_last_365d": 1,
  "replied_ever": true,
  "replies_total": 7,
  "meaningful_reply_threads": 3,
  "lead_initiated_threads": 2,
  "founder_wrote_last": true,
  "days_since_last_reply": 41,
  "days_since_last_call": 83,
  "days_since_last_meaningful_bilateral_touch": 41,
  "max_roundtrip_turns": 6,
  "distinct_months_with_bilateral_interaction": 4,
  "shared_chat_activity_14d": 2,
  "shared_chat_activity_30d": 5,
  "shared_chat_spike": true,
  "latest_inbound_intent": "positive|objection|referral|not_now|neutral",
  "latest_inbound_tone": "positive|neutral|negative",
  "defer_until": "2026-09-01",
  "explicit_negative_recent": false,
  "invalid_or_blocked": false
}
```

If you implement only this schema plus nightly recomputation, you already have something much closer to a real RevOps system than most manually maintained founder CRMs. HubSpot’s current score history/performance tooling is a good reference pattern here: event-driven scores, history, factor visibility, and trend monitoring rather than one frozen field. citeturn14view2turn14view3

## Anti-patterns and the minimum viable rollout

The biggest anti-pattern is **over-engineering too early**. Relationship-based B2B scoring fails when teams create 40+ weights, five interlocking formulas, and a 0–100 score that nobody can explain. Current GTM best practice is moving the other way: use fewer inputs, separate fit from live engagement, and work in priority bands that map directly to action. Salesforce says this plainly for small teams; Common Room says static tiers are less useful than dynamic priority bands. citeturn3view5turn13view0

The second anti-pattern is **folding fit and temperature into one number**. Fit answers “could this person buy?” Temperature answers “should I contact them now?” Those are different questions. HubSpot and Microsoft both support separate property-based and event-based scoring precisely because these dimensions have different operational purposes. citeturn14view0turn14view1turn3view3

The third anti-pattern is **no decay**. A founder-led network is especially vulnerable to this because old trust can fool you into thinking a relationship is still live. Modern scoring systems explicitly weight recent activity more than old activity, and Salesforce now treats decay as built-in behavior-score logic. If you keep someone warm forever because they once took a call, the queue rots. citeturn15view1turn13view0

The fourth anti-pattern is **using LLMs as a substitute for data engineering**. If the system cannot deterministically tell whether the lead replied, how many times, and who wrote last, you do not have an AI problem; you have a parsing and event-modeling problem. Communication research says prior interactions and timing already explain a large share of reply behavior. Spend your sophistication budget on extracting structured events and reliable labels, not on asking an LLM to re-discover counts from raw threads every night. citeturn25view0

The fifth anti-pattern is **training predictive ML on bad targets**. Microsoft’s and Salesforce’s current docs both make clear that predictive scoring depends on enough historical examples with usable outcomes. Your manual statuses are already inconsistent with call history, so they are not fit to be training truth. Start with rules, gather fresh reactivation outcomes for 8–12 weeks, then consider a supervised model on clean labels such as `positive_reply`, `booked_call`, and `opportunity_created`. citeturn3view4turn15view1

The minimum viable rollout is small:

1. **Backfill events** from SQLite, call logs, Telegram archives, and shared-chat activity.
2. **Compute deterministic features** nightly.
3. **Run one lightweight LLM classifier** only on latest inbound messages and unresolved ambiguous threads.
4. **Write two fields** per person: `temperature_band` and `reactivation_priority`.
5. **Write one explanation field**: top three factors.
6. **Route** only `Hot`, `Warm`, and `Now/This week` reactivation contacts into the founder queue.
7. **Review outcomes weekly** and recalibrate half-lives quarterly.
8. **Delay predictive ML** until you have a trustworthy outcome dataset.

That is the minimal viable scoring architecture that will survive contact-volume growth, assistant handoffs, and AI-agent involvement without becoming a maintenance trap. citeturn14view2turn17view2turn3view5turn13view0

## Source and case list

| Source or case | Why it matters |
|---|---|
| **HubSpot Knowledge Base — lead scoring tool** | Current official separation of fit, engagement, and combined scores; confirms negative scoring and operational score building. citeturn14view0turn14view1 |
| **HubSpot Knowledge Base — score history and performance** | Strong reference for explainable scoring: trend view, event history, performance review. citeturn14view2 |
| **HubSpot Knowledge Base — intent signals** | Confirms modern CRM practice of tracking 30-day signal history such as funding, job changes, visitor and research intent. Useful for future signal enrichment. citeturn14view4 |
| **Microsoft Dynamics 365 — lead and opportunity scoring** | Official evidence that predictive scoring needs enough recent labeled data; supports delaying ML until labels are clean. citeturn3view4 |
| **Microsoft Customer Insights — scoring model builder** | Official evidence for event- and attribute-based scoring with positive and negative points inside time windows. citeturn3view3 |
| **Salesforce Trailhead — Einstein Behavior Scoring** | Official evidence for recency weighting, built-in decay, positive and negative factors, and the value of simple thresholds for smaller teams. citeturn15view1turn3view5 |
| **Common Room — account prioritization guide** | Best current GTM articulation of fit as filter, recency/frequency/depth, signal decay, and action bands like now / this week / this month. Highly relevant to your founder-capacity constraint. citeturn13view0 |
| **Common Room docs — signals and integrations** | Direct evidence that community and social activity are now treated as actionable person-level GTM signals. Useful for your shared-chat resurfacing logic. citeturn16view0 |
| **Microsoft Research / SIGIR — enterprise email reply behavior** | Best evidence base for which conversation features matter most: prior interactions, timing, content properties, and reply-history patterns. Supports rules-first feature engineering. citeturn25view0turn12view0 |
| **Outreach support — sentiment classification** | Practical evidence that ML is useful for top-level sentiment/intent labels, with published category taxonomy and accuracy range. citeturn17view0 |
| **Case: Notion + Common Room** | Vendor-reported but relevant example of unified signals, scoring, job-change tracking, and real-time alerts generating measurable pipeline and meeting lift. citeturn19view0 |
| **Case: Zapier + Common Room / Superhuman + Common Room / BrightTALK + Outreach** | Vendor-reported but useful directional cases showing that signal-based prioritization and sentiment-aware engagement can materially improve meetings, reply rates, and time-to-opportunity. citeturn21search0turn21search1turn20view0 |

The confidence level is **high** on the architecture and the feature hierarchy, **medium** on the exact half-life values until you calibrate them on your own Telegram outcomes, and **low** on any attempt to jump directly to predictive ML before you clean labels and log fresh reactivation results. That is the correct level of skepticism for this kind of founder CRM.