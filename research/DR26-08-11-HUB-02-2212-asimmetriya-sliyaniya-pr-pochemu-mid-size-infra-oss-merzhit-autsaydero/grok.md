# DR26-08-11-HUB-02-2212 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-11-HUB-02-2212
rail: grok (CLI)
mode: cli-headless
door: /usr/local/bin/grok -p
collected: 2026-08-11T14:29:01-07:00

unique_urls: 7
chars: 24336
note: 7 уник. URL против 27 у chatgpt-рельсы — второе мнение, не доказательная база
---

Researching OSS merge-rate evidence, vendor-repo maintainer economics, and conversion paths for unknown contributors — testing your hypothesis against public data, not just the 68-PR sample.# Why mid-size infra merges outsiders while flagship LLM cookbooks do not

**Verdict up front:** Your working hypothesis is **mostly right, and the July cookbook strategy was aimed at the wrong class**. The driver is not “big vendor vs small OSS” per se — it is **whether the repo *is the product* a maintainer ships**, versus **whether the repo is a brand/education surface with no engineering payoff for merging strangers**. One refinement matters: even *inside* vendor orgs, **product SDK repos behave more like mid-size infra** than cookbooks do. Your 4/5/39 split is not a sampling fluke; it matches the structure of these surfaces.

---

## 1. Is the asymmetry real and general?

### What the literature actually measures

There is **no published 2025–2026 paper that slices merge rates by “vendor cookbook vs mid-size SDK vs awesome list.”** That comparison, as a formal category, is essentially **your measurement**. What *does* exist:

| Claim | Numbers | Source quality | Tier |
|---|---|---|---|
| Social/network factors dominate acceptance | Direct collaboration with integrators is the strongest predictor; ecosystem PR volume → up to **+25.2%** acceptance odds; models reach **F1 ≈ 0.92** (newcomers harder) | Meijer, Riveni, Rastogi — *Empir. Softw. Eng.* 2025; ~1.8M PRs, NPM ecosystem; arXiv [2410.14695](https://arxiv.org/abs/2410.14695), published Oct 2025 | **[established]** within SE research |
| Intra-project history slightly beats pure ecosystem resume | Intra-project contributions ~**2%** larger effect than ecosystem-wide in their models | Same paper | **[established]** |
| Newcomers are systematically disadvantaged | Multiple studies; PRs by newcomers more often rejected | Zhang et al. 2023 survey of 94 factors; Soares/Lee/Kovalenko lines cited in Meijer | **[established]** |
| PR age correlates with rejection | Older PRs more likely rejected (control variable across studies) | Zhang et al. 2023; Zampetti 2019 etc. | **[established]** |
| Global merge volume exploded; maintainer attention did not | ~**25M** PRs/month (Jan 2023) → **~90M**/month (2026); GitHub shipped **PR caps for non-write users** (Jun 2026) | [GitHub Blog, 2026-06-18](https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/) | **[established]** for volume; **[emerging]** for “slop” as cause |
| Disclosed AI trailers merge ≥ baseline (aggregate GH) | Window Jul 1–25 2026: Co-Authored-By Claude merge rate **95.7%** vs all PRs **88.2%** | GitHub Community measurement [single-source, self-report](https://github.com/orgs/community/discussions/185387) | **[speculative]** selection bias huge |

**What does *not* exist (say plainly):** a clean table “cookbook external merge rate = X%, SDK = Y%” from a peer-reviewed corpus. **[no source found]** for that exact cross-class table outside your 68-PR sample + the live GH snapshots below.

### Live evidence on the exact repos you are stuck in (Aug 2026)

Pulled via GitHub Search API (snapshot ~2026-08-11). Not a full population study — **[emerging]**, but concrete:

| Repo class | Repo | Merged (≈12 mo or all-time where noted) | Open | Who merges |
|---|---|---|---|---|
| Vendor cookbook | `openai/openai-cookbook` | **241** since 2025-08 | **167** | Dominated by `*-openai` / `*-oai` logins |
| Vendor cookbook | `anthropics/claude-cookbooks` | **115** since 2025-08 | **235** | Dominated by `*-ant` / Anthropic staff; one high-volume internal-looking author |
| Vendor cookbook | `mistralai/cookbook` | **44** since 2025-08 | **47** | Almost entirely one staff author (`jenperson`) |
| Vendor cookbook | `xai-org/xai-cookbook` | **6** all-time | **19** | xAI staff only in sample |
| Community cookbook | `huggingface/cookbook` | **14** since 2025-08 | mixed | Staff + true outsiders + partner product notebooks |
| Mid-size product | `pydantic/logfire` | **578** since 2025-08 | — | Core + many externals (incl. your account) |
| Mid-size product | `modelcontextprotocol/go-sdk` | **441** since 2025-08 | — | Diverse externals (incl. you) |

**Critical nuance that can look like it refutes you:** vendor cookbooks *do* merge a lot. OpenAI has **hundreds** of merges. The asymmetry is not “nothing merges” — it is **who is allowed into the merge set**. Staff/partner merges keep the repo alive; cold external notebooks rot in the open queue for months (oldest OpenAI open PR in the sample from **2024-07**; Claude cookbooks open PRs from **2024-03** with 1 bot-ish comment).

### Better explanation if class is wrong?

Steelman of the alternative:

> *It’s not class — it’s patch size, tests, CLA, issue linkage, author history.*

These matter (**[established]** — Gousios/Tsay/Zhang lines). They do **not** explain your split cleanly:

- You **did** merge into product-class repos with the same author account, same lab, same AI disclosure style.
- Vendor cookbook CONTRIBUTING for OpenAI does not ask for better tests — it **pre-emptively disclaims review**:

> “Contributions are reviewed on a **best-effort basis** — we can't provide guarantees around when or if content contributions will be reviewed or merged.”  
> Source: [`openai/openai-cookbook` CONTRIBUTING.md](https://github.com/openai/openai-cookbook/blob/main/CONTRIBUTING.md) (live, 7 lines; no date stamp — treat as current policy text).

That is not a quality filter. That is an **explicit non-SLA**.

**Hypothesis score:** class-as-product vs class-as-brand → **[emerging → near established for this niche]** given (a) your outcome data, (b) live author distributions, (c) OpenAI’s own CONTRIBUTING. Not [established] in the academic literature because nobody published that slice.

---

## 2. Economics of the vendor cookbook maintainer

### Who owns it?

| Surface | Likely owner | Measured on | Merge incentive for stranger notebook |
|---|---|---|---|
| First-party cookbook | **DevRel / DX / developer content**, sometimes rotating engineers near product launches | Launch coverage, traffic to docs, partner co-marketing, internal example for new model APIs | **Low.** Review cost high (brand, correctness, API drift); upside accrues to *vendor narrative*, not to the maintainer’s eng roadmap |
| Product SDK / library | **Product eng + OSS maintainers** | Bugs fixed, releases shipped, CI green, customers unblocked | **High.** External fix reduces their backlog |
| Awesome list | **Single curator** (often one person) | Taste, list quality, anti-spam | **Near zero** unless the entry is obviously elite |

First-hand policy signal (not a blog guess): OpenAI’s CONTRIBUTING is a **best-effort / no-guarantee** contract for content. Anthropic’s CONTRIBUTING is longer and quality-heavy (ruff, notebook validation, Claude review) — that raises the **bar** for outsiders without creating a **queue owner with SLA**. HF’s cookbook explicitly invites community recipes and names reviewers (`@merveenoyan`, `@stevhliu`) in the README contribution path — different economic design.

### Why ignore is rational

1. **Brand cost is asymmetric.** A merged notebook becomes “official OpenAI/Anthropic guidance.” A silent open PR costs almost nothing.
2. **Content is not backlog.** Product bugs create customer pressure; community notebooks do not.
3. **2025–2026 AI volume shock.** GitHub’s own framing: Eternal September + AI → PR caps for outsiders. Cookbook maintainers face the same flood with *less* engineering payoff.
4. **Partner pipeline already fills the “external” slot.** See §3.

### Awesome lists specifically

`sindresorhus/awesome` PR template: “**Fully AI-generated pull requests are not accepted**” + must be 100% ready (no draft).  
`sindresorhus/awesome`: **26** merged since 2025-01 vs **84** open — chronic backlog.  
`WangRongsheng/awesome-LLM-resources`: **20** merged since 2025-01, **19** open — slow, curator-gated.  
Economics: **taste + reputation of the list**, not engineering utility. Cold adds from unknown labs are noise.

---

## 3. What actually got merged in *your* stuck vendor repos

Reconstruction from merged PR authors (last ~12 months samples):

### Pattern A — Staff ship the cookbook (dominant)

- OpenAI: `vb-openai`, `kkahadze-oai`, `erikakettleson-openai`, `neilh-oai`, `cguo-oai`, …
- Anthropic: `cj-ant`, `mattmccarley-ant`, `markn-ant`, `kevintang513` (company: Anthropic), `bradabrams` (company: Anthropic), `nikblanchet` (company: Anthropic)
- xAI: `tagakov-xai`, `egec1`, `damien-xai`, `zhaohan-dong`
- Mistral: almost entirely `jenperson`

**Interpretation:** the cookbook is an **internal content pipeline with a public git UI**, not a community project.

### Pattern B — Corporate partner / co-marketing (true “external” that works)

| Case | Repo | Author | Signal |
|---|---|---|---|
| Fraud Review Agent + MongoDB Atlas | `anthropics/claude-cookbooks` #759 | `mohammaddaoudfarooqi` | **company: MongoDB**, Solutions Architect |
| RapidFire AI multi-config SFT | `huggingface/cookbook` #341 | `kamran-rapidfireAI` | **company: RapidFireAI** — product demo notebook |
| AML Agents + Amazon Bedrock | `openai/openai-cookbook` #2896 | `deepakjain108` | Partner path under `examples/partners/AWS/`; thin public profile |
| Cerebras-related | `openai/openai-cookbook` | `zhenweig-cerebras` | Login encodes employer |

**Interpretation:** the door that works is **BD/partner/DevRel relationship**, not cold OSS.

### Pattern C — Rare genuine community (mostly HF; tiny elsewhere)

- HF: `behroozazarkhalili` (DSPy GEPA tutorial, Oct 2025), `ahnjj` (Korean smolagents), `Spartan-71` (dependency fix), `gideonphilip` (widget metadata fix). HF **documents** “community effort” + tag named maintainers.
- OpenAI polish that looks external: `minskimm` data-flow diagram — body notes “Suggested by review feedback” and AI generation trailer; likely **already in a review thread with staff**, not a cold first touch.
- Anthropic `Briiick` (Alexander Bricken) “Fable 5 classifier…” — name recognition / prior relationship **[unverified without private context]**; not a pure cold unknown.

### How successful outsiders get in (reconstructed mechanism)

```
invite / partner / prior human contact
        OR
fix that staff already wants + tiny surface
        OR  
(HF-style) community cookbook with named reviewers
        ↓
merge
```

Cold “here is a cool notebook we built with your API” is **structurally the lowest priority packet** on a DevRel desk in 2026.

---

## 4. Conversion: do 4 mid-size merges buy you flagship trust?

### What the research says

Meijer et al. 2025 (NPM, not LLM cookbooks — **domain transfer is imperfect**):

- Ecosystem experience **helps**, especially **newcomers**.
- **Direct collaboration with the integrator** beats pure resume.
- Intra-project history still slightly stronger than pure cross-repo resume.
- Prediction hard for pure newcomers.

### What that means for *you*

| Transfer path | Plausible? | Mechanism | Evidence |
|---|---|---|---|
| logfire merge → faster openai-cookbook triage | **Weak** | Different org, different CODEOWNERS, different reputation graph | No shared maintainers; Meijer effect is *within ecosystem graphs* |
| MCP go-sdk / fastmcp merges → other MCP / agent-infra repos | **Medium–strong** | Same dependency ecosystem; maintainers see related work | Meijer + your own go-sdk multi-merge pattern |
| Mid-size merges → hiring / DevRel attention at flagship | **Weak–medium, slow** | Portfolio signal on GH profile; not a triage accelerator | No study found that vendor DevRel ranks applicants by third-party OSS merges **[no source found for DevRel hiring rubric]** |
| Mid-size merges → flagship *SDK* (not cookbook) | **Medium** if product-adjacent | Shared problem domain + issue-linked fixes | Closer to product economics |

**Honest answer:** the four merges are **real capital inside the product-OSS graph** and **almost worthless as a queue-jump token** for Anthropic/OpenAI/xAI *cookbooks*. Those are separate reputation systems. Treating “merged contributor on logfire” as “now they will review our claude-cookbooks PR” is a category error.

**Deliberate transfer mechanisms that *do* work (ordered):**

1. **Same human** (you meet the cookbook owner at a conference / Discord / partner intro) — social edge Meijer calls direct collaboration.  
2. **Same dependency graph** (you ship a library they import; your fix unblocks *their* users).  
3. **Public consumption** (their customers cite your tool; then *they* invite a cookbook).  
4. **Portfolio for humans** (hiring, advisors) — offline the PR queue.

---

## 5. The 17 open threads — ball on you

### Evidence on latency

- Long wait for first response demotivates and correlates with abandonment (Li et al. 2021/2022 line; Khatoonabadi abandoned-PR work). Older source (TSE 2021: “Are You Still Working on This?”) — **still directionally valid**, not 2026-specific.  
- Sentry’s 2026 external-PR playbook (product OSS, not cookbook): **triage within 2 business days**; “**closing fast is kind**”; require linked issue for non-trivial work.  
- 18-day late *contributor* reply after a human/bot asked for changes: **still salvageable** if the thread has an owner. Silence after review request is what kills PRs in abandonment studies. **[emerging]** practice consensus; not a RCTs.

### Force-push vs incremental commits

| Situation | Prefer | Why |
|---|---|---|
| Address a clear bot checklist / one round of nits | Force-push (or squash) to clean tip | Reviewers re-read final state |
| Multi-round human design discussion | Incremental commits with messages mapping to review comments | Preserves review narrative |
| CI red after base moved | Rebase + force-with-lease | Stale PR tax |

### Stale: revive in place vs close+reopen

- **Revive in place** when a human already engaged — context is the asset.  
- **Close+reopen fresh** when: (a) completely different design, (b) PR is a content dump against a rewritten cookbook structure, (c) bot war / broken checks that need a clean SHA, (d) you want to re-enter a PR-cap queue cleanly (GitHub PR limits for non-write users, Jun 2026).  
- **Do not** “bump” with empty comments on silent cookbooks — that is spam in the 2026 regime.

### Tactical playbook for your 17

1. **Respond within 24–48h of any human/bot ask** — even partial: “got it, shipping X by DATE.”  
2. **Map each review item → commit or checkbox** in the reply.  
3. **If 14+ days since *their* last word and you delivered everything** — one short ping with diff summary; then calendar a 21-day kill.  
4. **huggingface/cookbook #366 (18d):** HF is the one cookbook class that still looks community-real — **prioritize** over Anthropic six-pack.  
5. **Bot-only CI requests:** fix, don’t argue; if bot is wrong, one human-directed comment with evidence.  
6. **Never open more PRs into a silent vendor cookbook** while 6 already sit there (PR caps + attention math).

---

## 6. Where a small unknown lab should aim (2025–2026)

### Ranked target classes

| Rank | Class | Examples (LLM/agent 2025–26) | Why | Disqualify if… |
|---|---|---|---|---|
| **1** | **Product is the repo** — SDKs, runtimes, protocols | `modelcontextprotocol/*`, MCP server frameworks (fastmcp-class), agent eval harnesses (`inspect_ai`-class), observability (`pydantic/logfire`) | Maintainer merges = ships product; your 4 merges already prove this | No issue activity 90d; CLA nightmare; hostile AI policy with no disclosure path |
| **2** | **Independent mid-size agent frameworks with real issue queues** | Projects in the size band of Agno / frameworks that *reply* (your open threads with ball-on-you are better than silent cookbooks) | Engineering benefit; visible stars | Maintainer is one person drowning + no CODEOWNERS |
| **3** | **Community cookbooks with named reviewers** | `huggingface/cookbook` | Explicit community design | Treat like product only if you follow their author/toctree rules |
| **4** | **Vendor product SDKs (not demos)** | Agent SDKs, official language clients — *only* issue-linked bugfixes | Closer to product economics than cookbooks | Speculative features without issue |
| **5** | **Vendor cookbooks / demos** | claude-cookbooks, openai-cookbook, xai-cookbook, mistral cookbook | Visibility *if* you get in — but entry is partner/staff | **Default: do not cold-PR** |
| **6** | **Awesome lists** | awesome-LLM-*, sindresorhus/awesome | SEO vanity | Curator gate + AI bans; low eng signal |

### Disqualifiers (hard)

- CONTRIBUTING says “best-effort / no guarantee” and no named reviewer  
- Open PR pile older than 6 months with 0–1 comments (your Claude cookbook queue shape)  
- Curated list with “AI-generated PRs not accepted” *and* no human relationship  
- Demo/gallery repo whose commits are 95%+ staff  
- You already have ≥2 silent PRs there

### What *not* to abandon

- Threads where a **human** already spoke (your 17) — that is the only warm capital in flagship space.  
- MCP / eval / observability graph — double down.

---

## 7. AI-authorship variable

### Documented backlash (2025–2026) — real

| Policy type | Examples | Source |
|---|---|---|
| Disclose AI assistance | Ghostty (evolved to stricter); MicroPython disclosure reports; many CONTRIB clauses | Ghostty policy discussions; RedMonk landscape Feb 2026; policy catalogs |
| Ban / restrict AI-generated PRs | Ghostty (drive-by AI only on pre-approved issues); Godot 2026 contribution policy tightening; tldraw auto-close external PRs (reported) | [Godot article 2026-06-30](https://godotengine.org/article/contribution-policy-2026/); Ghostty PR #10412 coverage |
| Platform controls | GitHub PR caps for users without write access (Jun 2026); discussion of collaborator-only PR modes | [GitHub Blog 2026-06-18](https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/) |
| Maintainer narrative | “Good-faith and bad-faith AI impose same triage cost” | Industry commentary 2026 |

### Counter-evidence

- Aggregate search: PRs with `Co-Authored-By: Claude` merged **at least as often** as baseline in one Jul 2026 window — **but** opt-in disclosure selects careful authors. [single-source]  
- Sentry playbook: review AI-looking PRs with **extra checks**, not auto-reject — product OSS stance.  
- Your own merges in product repos with disclosed AI assistance (if that was your practice) are **existence proofs** that disclosure ≠ death in product class.

### Does it differ by repo class?

| Class | AI disclosure effect (2026) | Confidence |
|---|---|---|
| Mid-size product infra | Neutral → slight positive if tests/CI prove competence | **[emerging]** |
| Vendor cookbook | Near-irrelevant vs **brand + queue** — disclosure won’t get you reviewed; slop pattern will get you closed faster | **[emerging]** |
| Awesome lists | Often **negative** (explicit bans) | **[established]** for lists that state it |
| Hostile-policy projects (Ghostty-class) | Disclosure mandatory; drive-by banned | **[established]** for those projects |

**Bottom line:** disclosed AI is not the reason your cookbooks are silent. **Class + flood + non-SLA** are. Disclosure remains correct: it avoids “caught slop” reputation and satisfies growing policies.

---

## Disagreements worth preserving

| Position | Steelman | Counter |
|---|---|---|
| “Just write better PRs” | Description quality associates with outcomes (2026 PR-description study, 80k PRs) | Doesn’t fix OpenAI’s written non-guarantee or 2024-open queue |
| “Ecosystem resume transfers everywhere” | Meijer +25% | Transfer is **within graphs**; logfire ↛ anthropic-cookbook |
| “Vendors hate community” | HF cookbook proves some vendors design for community | First-party model vendors use cookbooks as **internal content rails** |
| “AI is why” | Volume shock is real | Your product merges with same AI style refute pure AI-blame |

---

## Decision-oriented ending

### Options (concrete)

| Option | What you do | Upside | Risk |
|---|---|---|---|
| **A. Kill cold cookbook lane** | Close or freeze silent cookbook/awesome PRs; stop new ones | Stop burning cycles; clean strategy | Lose rare lottery win |
| **B. Finish the 17 warm threads only** | 48h response SLA; kill after full delivery + one ping | Convert only real options | Time cost |
| **C. Double down product-OSS** | MCP, eval, observability, agent runtime bugs with linked issues | Matches measured merge engine | Less “vendor logo” vanity |
| **D. Partner door, not PR door** | Pitch MongoDB-style co-branded notebooks via DevRel/partners | Only proven external merge path into vendor cookbooks | Needs relationships; slower |
| **E. HF as sole cookbook exception** | Treat HF as community product; follow their author/toctree rules | Real external merges observed | Smaller prestige than OpenAI logo |

### Single recommendation

**Do A + B + C. Drop D unless a human intro appears. Keep E only if bandwidth remains.**

- **Confidence:** **[emerging]** (high for your sample + live GH author distributions; medium for generalizing to all vendors).  
- **Main assumption:** maintainer incentives for first-party cookbooks stay “brand content, no review SLA” through 2026 — which OpenAI has written down and which author histograms confirm for Anthropic/xAI/Mistral.  
- **If that assumption fails:** only a published external-contribution program with named owners and SLA would rehabilitate cold cookbook PRs — watch for that explicitly; until then, treat “Contributions welcome” on vendor cookbooks as **marketing copy**, not an ops contract.

**July strategy judgment (no flattery):** aiming 40+ PRs at vendor cookbooks and awesome lists was a **structurally wrong class**. The 4 merges in product-class mid-size infra are the signal. The 39 silences are the system working as designed — not a failure of patch craftsmanship.

---

### Source list (all used)

1. Meijer, Riveni, Rastogi — *Ecosystem-wide influences on pull request decisions* — Empir. Softw. Eng. 2025 / [arXiv:2410.14695](https://arxiv.org/abs/2410.14695) (Mar 2025 preprint; journal Oct 2025)  
2. [openai/openai-cookbook CONTRIBUTING.md](https://github.com/openai/openai-cookbook/blob/main/CONTRIBUTING.md) — live policy text  
3. GitHub Blog — [How pull request limits are cutting down the noise](https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/) — 2026-06-18  
4. GitHub Changelog — PR limits for users without write access — 2026-06-17 (via secondary reports)  
5. Sentry Develop — [Handling an External Contributor Pull Request](https://develop.sentry.dev/sdk/getting-started/playbooks/development/handling-external-contributor-pr/) — v1.0.0, 2026-02-21  
6. [sindresorhus/awesome PR template](https://github.com/sindresorhus/awesome/blob/main/pull_request_template.md) — AI PR ban  
7. Hugging Face cookbook README contribution path — tag `@merveenoyan` / `@stevhliu`  
8. Godot — [Changes to our Contribution Policies](https://godotengine.org/article/contribution-policy-2026/) — 2026-06-30  
9. GitHub Community discussion measurements on Co-Authored-By Claude merge rates — 2026-07 window [single-source]  
10. Live GitHub Search API snapshots for named repos — 2026-08-11 (this report)  
11. Zhang et al. / Gousios et al. classical PR acceptance factor literature (via Meijer citations)  
12. Abandoned PR literature: Khatoonabadi et al.; Li et al. “Are You Still Working on This?” TSE 2021  
13. RedMonk / Ghostty AI policy coverage — Feb 2026 era [secondary journalism]  
14. Your lab outcome data — mid-Jul → 11 Aug 2026, 68 PRs (primary for this memo; not re-derived)

---

🎙 **Майк:** July cookbooks were a wrong door with a nice logo on it. The four mid-size merges are the real company. Stop paying brand tax; cash the product-OSS lane and finish only the 17 where a human already said your name.

🧒 **Простыми словами:**  
Представь: ты чинишь чужой молоток — хозяин рад, потому что молотком **работают**. А «книга рецептов» у большой компании — это витрина магазина. Чужие рецепты туда почти не ставят: страшно за бренд, а пользы хозяину мало. Вы стучались в витрины. Сработало там, где чинили настоящий инструмент. Дальше — инструменты и только те разговоры, где вам уже ответили.
