# DR26-08-11-HUB-02-2212 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-11-HUB-02-2212
rail: chatgpt (codex CLI exec)
model: codex default
mode: cli-headless, без вендорского Deep Research
door: codex exec --skip-git-repo-check
collected: 2026-08-11T14:27:06-07:00

unique_urls: 27
chars: 30400
note: prompt подавался БЕЗ YAML-frontmatter — с '---' в начале и grok, и codex падают 'unexpected argument'
---

11.08.2026, 10:18 PT

# Research verdict

**[emerging] Your repo-class hypothesis is directionally right, but too coarse.**

The stronger model is:

> An unsolicited PR merges when it removes an already-recognized maintenance cost from a repository whose owners are operationally accountable for shipping it. It stalls when it asks maintainers to make a new product/editorial decision, especially in a high-volume repository where the contributor has no local trust.

“Library versus cookbook” is a useful proxy for this incentive difference, but it is not the causal variable by itself.

The practical hierarchy appears to be:

1. Maintainer requested or pre-approved the work.
2. The patch fixes a reproduced problem already inside the roadmap.
3. The author has local history in that repository.
4. The patch is small, tested and inexpensive to review.
5. The repository has an active triage process and a named owner.
6. Only then: repository class.

This does mean the July cookbook strategy was structurally weak. The problem was not necessarily patch quality. Unsolicited notebooks combine the worst properties for reviewers: large surface, nondeterministic outputs, dependencies, ongoing freshness obligations, security/brand review and an implicit request to endorse the author’s choice of topic.

There is, however, **no published 2025–2026 dataset I could find that reports outsider merge rates separately for SDKs, cookbooks, infrastructure projects and awesome lists**. Any exact four-class percentages would therefore be invented. Your 68-PR dataset is useful early evidence, not yet an ecosystem baseline.

---

## 1. Is the asymmetry real and general?

### What the evidence establishes

**[established] Repository governance and reviewer capacity materially affect acceptance and latency.** A July 2026 study found that submission-time patch features explain only a modest portion of review effort and time-to-merge; reviewer availability, workflow and team-specific practices remain major unexplained variables ([Yang et al., 20 July 2026](https://arxiv.org/abs/2607.12057)).

**[established, older evidence] Local contributor history matters.** Research across 20 popular OSS projects found that contributors with higher project-specific acceptance rates and a history of timely responses receive faster maintainer responses ([Khatoonabadi et al., 13 November 2023](https://arxiv.org/abs/2311.07786)). Another study found continued contribution to the same project correlated with higher later acceptance ([Legay, Decan & Mens, 15 December 2018](https://arxiv.org/abs/1812.06269)). These are older than the requested window but remain the best direct evidence I found.

**[emerging] One-time contributors have become disproportionately expensive in the AI-agent era.** A 2026 study of 294 repositories reports that after the recent AI contribution surge, PR volume rose 6.8%, while merge ratios fell 1.06% overall and 18.18% for one-time contributors ([“AI Slop is DDoSing Open Source,” July 2026](https://www.alphaxiv.org/overview/2607.04003)). The underlying paper is recent and its causal design needs replication, so this is not yet established.

**[established] GitHub itself now treats prior local acceptance as a trust signal.** Its proposed PR-volume controls explicitly contemplate allowing contributors to bypass limits after a previously merged PR **in that repository**, along with account age and organization membership ([GitHub, 18 June 2026](https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/)). GitHub also added a repository setting that can prohibit PRs from non-collaborators ([GitHub, 13 February 2026](https://github.blog/changelog/2026-02-13-new-repository-settings-for-configuring-pull-request-access/)).

### What is not established

| Comparison | Published 2025–2026 rate | Honest conclusion |
|---|---:|---|
| Vendor SDK/library | No class-wide number found | Likely heterogeneous: mature SDKs may be tightly controlled; ecosystem SDKs may welcome fixes |
| Vendor cookbook/demo | No class-wide number found | Your results and current queues suggest high unsolicited-content friction |
| Independent mid-size infrastructure | No class-wide number found | Your four fast merges are consistent with direct maintainer utility |
| Awesome lists | No class-wide number found | Governance ranges from active curation to abandonment; class label alone is insufficient |
| Time to first human response by these classes | No comparative number found | Must be measured directly from GitHub event histories |

### Refined causal model

Your original theory says that cookbook PRs provide “no engineering benefit.” That overstates the case. Fixing a broken or obsolete first-party example has real engineering and support value. The crucial distinction is:

- A bug fix reduces an obligation the maintainer already owns.
- A new notebook creates an obligation: editorial judgment, testing, future model/API migrations, security checks and implied endorsement.

Thus, **patch type within a repo can matter as much as repo class**. A five-line correction to an existing cookbook has a different approval economy from a new 500-cell notebook.

### Verdict on the hypothesis

**[emerging] Confirmed in direction, refuted as a sufficient explanation.**

A better formula is:

`merge likelihood ≈ prior alignment × local trust × maintainer utility ÷ review + ownership cost`

Repo class changes the numerator and denominator, but does not determine them.

---

## 2. The economics of vendor cookbooks and awesome lists

### Vendor cookbooks

The repositories describe themselves as publication surfaces, not merely code stores. OpenAI’s repository feeds `cookbook.openai.com` and contains author and content registries; Anthropic’s cookbook is also surfaced as a product-like catalog on its developer platform ([OpenAI repository, accessed 11 August 2026](https://github.com/openai/openai-cookbook); [Anthropic cookbook, accessed 11 August 2026](https://platform.claude.com/cookbook/)).

That changes the economics:

- A merge can become vendor-branded instructional content.
- The reviewer must validate current API behavior, pedagogical quality, security, dependencies and longevity.
- A new recipe competes with an internal editorial roadmap.
- Notebook execution is harder to verify than a deterministic unit-tested patch.
- An accepted third-party integration can be misread as endorsement.

OpenAI’s registry/template system includes publication metadata and a self-review checklist, which is evidence of editorial control rather than a simple code-merge pipeline ([repository documentation, accessed 11 August 2026](https://github.com/openai/openai-cookbook); the clearest structural summary I found is [DeepWiki, accessed 11 August 2026](https://deepwiki.com/openai/openai-cookbook/2-repository-structure-and-contribution-guidelines) **[low-authority]**).

Anthropic explicitly tells prospective contributors to propose new examples and guides through issues before contributing, to avoid duplicated work ([Anthropic README, accessed 11 August 2026](https://github.com/anthropics/claude-cookbooks)). That is a strong signal that an unsolicited finished notebook starts behind a proposal-backed one.

**Who internally owns these repositories and what are their KPIs?** I found no reliable public source that names the current owner or gives internal performance metrics for all six repositories. Claims such as “DevRel is measured on API activation and therefore ignores PRs” remain **[speculative] [no source found]**.

The defensible inference is narrower:

> **[emerging]** Because these repositories publish vendor-branded education and have explicit content registries or proposal workflows, their maintainers behave partly as editors/product owners, regardless of whether their job title is DevRel, developer education or engineering.

### Curated awesome lists

An awesome-list maintainer receives almost none of the direct engineering benefit of a code fix. Each addition instead creates:

- A factual verification task.
- A relevance and quality judgment.
- Link-rot and removal obligations.
- Pressure from other projects demanding equal inclusion.
- A reputational risk if the linked resource is promotional, insecure or abandoned.

There is no 2025–2026 comparative study of awesome-list merge rates that I could locate. The incentive analysis is therefore **[speculative]** but consistent with the structure of the artifact. Lists with explicit objective criteria and automation are better targets than personality-driven “best of” lists.

---

## 3. What gets merged in the six vendor repositories?

A fully defensible reconstruction requires event-level extraction of every merged PR, author affiliation, preceding issue and maintainer invitation. GitHub’s public web index did not expose a complete queryable history in this research environment, so I will not fabricate a cohort table.

What the visible evidence does support:

### Anthropic Claude Cookbooks

The current registry includes recipes attributed both to Anthropic and to named non-Anthropic-looking accounts, including `PedramNavid` for programmatic tool calling, tool search and context-compaction material ([registry, accessed 11 August 2026](https://github.com/anthropics/claude-cookbooks/blob/main/registry.yaml)). Whether those authors were employees, contractors, invited partners or cold contributors cannot be established from the registry alone: **[no source found]**.

The repository’s own instructions ask authors to discuss ideas before submitting. Therefore the successful route is likely proposal → scope agreement → implementation, but the relationship histories of individual merged authors remain unverified.

### OpenAI Cookbook

The repository has more than 1,500 closed PRs and, at the time indexed, over 100 open PRs ([PR queue, accessed 11 August 2026](https://github.com/openai/openai-cookbook/pulls)). That queue size alone makes silence unsurprising.

A visible outside-authored new notebook, PR #2659, had 21 commits and bot review but remained open after submission ([PR #2659, 6 May 2026](https://github.com/openai/openai-cookbook/pull/2659)). This is one case, not a rate, but it illustrates that detailed execution proof and bot approval do not substitute for editorial sponsorship. **[single-source]**

### Google ADK community repository

Google describes `adk-python-community` as the home for community-contributed integrations, tools and deployment scripts, explicitly separating these from the core ADK product ([Google ADK README, accessed 11 August 2026](https://github.com/google/adk-python)). That is closer to a governed extension marketplace than a normal cookbook.

The implication is important: a patch must fit the defined extension taxonomy and maintenance contract. Being “community” does not mean arbitrary examples are cheap to accept.

### Hugging Face Cookbook

Hugging Face historically operates with a more community-oriented identity, but I did not obtain a complete last-12-month merged-outsider cohort. Treat any claim that it has a higher cold-contributor merge rate as **[speculative] [no source found]** until measured.

### xAI and Mistral cookbooks

I found no sufficiently complete primary-source cohort for either repository. **[no source found]**

### What would settle this question

Run a reproducible GitHub GraphQL audit for 11 August 2025–11 August 2026:

1. All merged, closed-unmerged and open PRs.
2. Exclude bots and organization members.
3. Label patch type: fix existing content, new content, dependency update, tooling.
4. Record preceding linked issue and whether a maintainer commented before first commit.
5. Record author’s prior merges in the target repo.
6. Manually verify employer/relationship only from public evidence.

Without that audit, named “successful outsider” stories risk becoming selection-biased anecdotes.

---

## 4. Does mid-size merged-contributor status transfer to flagship vendors?

**[established] Local trust transfers strongly within the same repository.** The empirical studies and GitHub’s own planned trust controls support this.

**[speculative] Cross-repository trust transfers weakly unless a human or dependency edge carries it.** I found no evidence that an OpenAI or Anthropic reviewer’s queue automatically prioritizes an author because that person merged into Logfire, Basic Memory or an MCP SDK.

The reputation graphs are not completely separate, but transfer requires one of these mechanisms:

1. **Shared maintainer:** a person from the target vendor reviewed or uses the upstream project.
2. **Dependency edge:** the vendor repo depends on the component you fixed.
3. **Issue relevance:** the merged upstream change directly resolves a target-repo problem.
4. **Human introduction:** the upstream maintainer explicitly vouches for you.
5. **Repeated visibility:** conference, working group, Discord or standards participation makes the GitHub identity recognizable.
6. **Public technical artifact:** the fix becomes cited, deployed or discussed beyond the original repo.

A 2024 cross-project study found that experienced contributors in the **target project** produced most cross-project PR recommendations; nearly half of such recommendations were accepted by issue participants ([APSEC, December 2024](https://conf.researchr.org/details/apsec-2024/apsec-2024-technical-track/4/An-Empirical-Study-of-Cross-Project-Pull-Request-Recommendation-in-GitHub)). This is adjacent, not direct evidence of hiring or reviewer trust, so the transfer conclusion remains **[emerging]**.

### How to trigger transfer deliberately

Do not say only “we have four merged PRs.” Make the graph explicit:

> “This fixes the same MCP transport edge case accepted upstream in `modelcontextprotocol/go-sdk#1148`; here is the upstream review, regression test and why this repository encounters the same condition.”

That converts generic prestige into review-relevant evidence.

For hiring, the four merges are a credible signal of execution and reviewer cooperation, but there is no published evidence that they independently improve vendor hiring odds. **[no source found]**

---

## 5. The 17 open threads: what to do now

**[established, older evidence] Timely contributor responses correlate with faster future responses and higher local acceptance history.** That argues for replying now, even after 18 days, rather than abandoning the threads ([Khatoonabadi et al., 13 November 2023](https://arxiv.org/abs/2311.07786)).

There is no credible threshold showing that an 18-day delay makes a merge unrecoverable. Established projects commonly tolerate 17–30 days before closing author-inactive PRs: ESLint documents 17 days and Jekyll 30 days ([ESLint maintainer guide, accessed 11 August 2026](https://eslint.org/docs/latest/maintain/review-pull-requests); [Jekyll maintainer guide, accessed 11 August 2026](https://jekyllrb.com/docs/maintaining/reviewing-a-pull-request/)). These are project policies, not causal studies.

### Response protocol

For every thread:

1. Respond to the human before the bot-only threads.
2. Start with: “Thanks, and apologies for the delayed response.”
3. Answer every requested change in a checklist.
4. Push the requested fixes in one focused update.
5. Post exact verification: commands, outputs and environment.
6. Mark unresolved judgment calls explicitly.
7. Re-request review once, using GitHub’s review mechanism when available.
8. Do not post repeated “any update?” comments without new information.

### Force-push versus incremental commits

- During active review, prefer incremental commits because reviewers can see exactly what addressed each comment.
- After approval, squash/rebase if the repository requests it.
- Force-push only when maintainers request a clean history or the branch contains mistakes/secrets.
- Never rewrite reviewed history silently.

This is workflow advice; I found no current experiment showing a general merge-rate advantage for either style. **[no source found]**

### Bot-requested changes

Treat required checks as real contract terms:

- Fix deterministic lint, formatting, CLA and test failures.
- Do not churn content merely to satisfy low-confidence generative review.
- Reply with evidence when the bot is wrong.
- After all checks pass, explicitly state that the branch is ready for human review.

### Revive or reopen?

Revive the existing PR unless:

- The maintainer explicitly asked for resubmission.
- The base architecture changed so much that the original discussion is misleading.
- The branch is unrecoverably polluted.
- A stale bot closed it and the project’s documented route is a fresh PR.

GitHub’s stale action removes the stale state after new interaction, while default timings are 60 days to stale and another seven to close ([GitHub Actions Stale, accessed 11 August 2026](https://github.com/actions/stale)). A fresh PR made only to regain queue position discards context and can look like notification gaming.

### Priority among the 17

1. `microsoft/agent-framework#7581`
2. `google/adk-go#1299`
3. `inspect_ai#4697` and `#4714`
4. `agno#9247` and `#9498`
5. `huggingface/cookbook#366`

The code/infrastructure threads outrank the cookbook because they offer stronger local-trust compounding. Within each group, do the smallest requested update first.

---

## 6. Where a small unknown lab should aim

### Ranked target classes

| Rank | Target | Why it can work | Risk/disqualifier |
|---:|---|---|---|
| 1 | Existing bugs in MCP SDKs and protocol tooling | Standards adjacency, testable behavior, cross-vendor relevance | Avoid unsolicited API redesigns |
| 2 | Observability and evaluation instrumentation | Vendors depend on traces/evals; fixes have direct operational value | Integration matrix can make patches large |
| 3 | Provider adapters in agent frameworks | Bounded change with an obvious owner/user | Avoid adding obscure providers without users/tests |
| 4 | Serialization, compatibility and CI fixes | Small, deterministic, low editorial cost | Low visibility if purely cosmetic |
| 5 | Security hardening with reproducible tests | High maintainer utility and strong signal | Coordinate privately for exploitable vulnerabilities |
| 6 | Documentation fixes attached to shipped behavior | Cheap to review if correcting an existing page | New tutorials remain editorially expensive |
| 7 | New cookbook content | Potential reach | Only after a maintainer approves the topic |
| 8 | Awesome lists | Easy to author | Weak hiring signal, opaque curation, little technical trust |

### Named targets

1. **`modelcontextprotocol/*` SDK repositories**  
   You already have local trust in `go-sdk`. Compound it with issue-linked compatibility or conformance fixes.

2. **`pydantic/logfire`**  
   You have a merge. Continue with instrumentation defects and documentation that corrects observed behavior.

3. **`pydantic/pydantic-ai`**  
   High visibility and active outside contributors, but only after issue alignment. Its current guide explicitly says non-trivial work should be agreed with a maintainer and assigned before a PR ([Pydantic AI contributing guide, accessed 11 August 2026](https://pydantic.dev/docs/ai/project/contributing/)). This is a good target only if you follow that route.

4. **`Arize-ai/openinference`**  
   Cross-framework OpenTelemetry instrumentation provides bounded integrations and dependency-graph visibility ([OpenInference repository, accessed 11 August 2026](https://github.com/Arize-ai/openinference)). Merge-rate claim not independently measured: **[speculative]**.

5. **`PrefectHQ/fastmcp` or its current canonical successor**  
   You already possess local proof. Target real interoperability issues, not another generic sample.

6. **`UKGovernmentBEIS/inspect_ai`**  
   Evaluation infrastructure has technical visibility and your two active threads already provide a trust on-ramp.

7. **`google/adk-go`**  
   Prefer the shipped SDK over gallery content; complete #1299 before opening anything else.

8. **`BerriAI/litellm`**  
   Large user footprint and an explicit contribution workflow ([LiteLLM CONTRIBUTING, accessed 11 August 2026](https://github.com/BerriAI/litellm/blob/litellm_internal_staging/CONTRIBUTING.md)). However, queue saturation, security complexity and a very high change rate make it a riskier target.

### Disqualifiers

Do not start work when any two apply:

- No human maintainer activity in 30 days.
- More than 100 untriaged PRs.
- CONTRIBUTING says invitation or assignment required and you lack it.
- The change is a new tutorial rather than a correction.
- No linked issue or maintainer agreement.
- You cannot provide a deterministic regression test.
- Equivalent PR already exists.
- Success depends on subjective editorial endorsement.
- The only value is a profile badge or backlink.
- The project has an explicit policy incompatible with your AI workflow.

---

## 7. AI-assisted authorship

### What is known

**[emerging] Disclosure by itself is not proven to reduce merge probability.** I found no robust study that isolates disclosed AI assistance while controlling for patch quality, author history and repository.

**[established] Maintainer concern is concentrated on responsibility and review burden, not merely tool use.**

- ESLint permits AI-generated code only when the contributor has personally reviewed and tested it, and tells contributors not to feed maintainer comments into AI to generate replies ([ESLint AI policy, accessed 11 August 2026](https://eslint.org/docs/latest/contribute/ai-policy)).
- OSRF requires disclosure and human responsibility for generated contributions ([Open Robotics, 8 May 2025](https://www.openrobotics.org/blog/2025/5/8/osrf-adopts-policy-on-use-of-generative-ai-in-contributions); [full policy, 9 May 2025](https://osralliance.org/wp-content/uploads/2025/05/OSRF-Policy-on-the-Use-of-Generative-Tools-Generative-AI-in-Contributions.pdf)).
- OpenInfra treats sign-off as human responsibility for the entire contribution, including AI-assisted material ([OpenInfra policy, 2025](https://openinfra.org/legal/ai-policy/)).
- Kubernetes’ 2026 policy emphasizes human understanding, testing and responsibility ([Kubernetes, 26 June 2026](https://kubernetes.io/blog/2026/06/26/open-source-maintainership-in-the-age-of-ai/)).
- A May 2026 policy study found fragmented approaches: some projects permit disclosed and supervised use, while others close low-effort AI submissions or block offenders ([“AI Policy, Disclosure, and Human in the Loop,” 15 May 2026](https://paperity.org/p/374043744/ai-policy-disclosure-and-human-in-the-loop-how-are-contribution-guidelines-adapting-to)).

**[emerging] The backlash is partly a volume and trust-collapse effect.** The 294-repository study reports the steepest merge-ratio decline among one-time contributors, while GitHub’s 2026 controls use prior local merges as a trust signal. This makes openly AI-assisted, high-volume, cross-repository submission especially vulnerable to being classified as contribution farming, even when individual patches are reasonable.

### Counter-evidence

The policies above do not universally ban AI. ESLint, OSRF, OpenInfra and Kubernetes all provide routes for responsible use. Disclosure can therefore be neutral or positive when paired with:

- A human-authored problem statement.
- Reproduction before implementation.
- Tests that would fail without the fix.
- A concise explanation of why the change is correct.
- Personal, direct responses to reviewers.
- Low submission volume.
- Evidence that the author understands every changed line.

### Operational recommendation

Keep disclosure, but change its shape:

> “AI-assisted; I reviewed every changed line and remain responsible. Reproduction: … Regression test: … Commands run: …”

Do not make AI participation the headline of the PR. The headline should be the maintained problem being removed.

---

# Concrete options

### Option A: Compound the four local reputation graphs

For the next 30 days, work only in MCP Go SDK, FastMCP, Logfire and Basic Memory, plus the existing 17 response-required threads.

- Benefit: highest expected merge probability and strongest evidence of sustained contribution.
- Risk: less immediate flagship-vendor branding.
- Expected result: “repeat trusted contributor,” which is stronger than four unrelated one-off merges.

### Option B: Issue-first flagship conversion

Choose one vendor SDK, preferably `google/adk-go`, and one evaluation project, preferably `inspect_ai`. Reproduce an existing problem, obtain scope agreement, then implement.

- Benefit: deliberate entry into higher-value local trust graphs.
- Risk: slower and dependent on maintainer responsiveness.
- Avoid cookbooks until invited.

### Option C: Dependency-bridge strategy

Find defects where MCP, Logfire/OpenTelemetry or evaluation tooling affects a flagship vendor example or SDK. Get the underlying fix merged first, then bring a small downstream update that cites it.

- Benefit: turns existing merges into relevant social proof.
- Risk: suitable bridge issues are rarer and require deeper system knowledge.

### Option D: Continue broad cookbook outreach

- Benefit: occasional high-visibility hit.
- Risk: high silence rate, weak accumulated trust, editorial rather than engineering review.
- I do not recommend it except for maintainer-requested topics.

# Single recommendation

**[emerging] Choose A + C: first clear all 17 ball-on-your-side threads, then spend 70% of new effort compounding the four repositories where you have merges and 30% building explicit dependency bridges into `google/adk-go`, `inspect_ai`, `pydantic-ai` and OpenInference. Stop unsolicited new cookbook and awesome-list PRs.**

The main assumption is that your objective is durable technical reputation and vendor attention, not merely total merged-PR count. If raw merge count is the objective, smaller infrastructure repositories would dominate even more strongly.

The decisive experiment is a 30-day prospective test:

- Ten issue-approved infrastructure/SDK fixes.
- Ten unsolicited cookbook/list submissions matched by patch size and author.
- Record first-human-response time, review rounds and merge outcome.

That would distinguish repo class from patch size and author behavior much more cleanly than the current heterogeneous 68-PR sample.

---

## Source register

1. Yang et al., “Predicting Acceptance and Review Effort in Human and Agent Pull Requests,” 20 July 2026: https://arxiv.org/abs/2607.12057  
2. Khatoonabadi et al., “Predicting the First Response Latency…,” 13 November 2023: https://arxiv.org/abs/2311.07786  
3. Legay, Decan & Mens, “On the impact of pull request decisions…,” 15 December 2018: https://arxiv.org/abs/1812.06269  
4. “AI Slop is DDoSing Open Source,” July 2026: https://www.alphaxiv.org/overview/2607.04003  
5. GitHub, “How pull request limits are cutting down the noise,” 18 June 2026: https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/  
6. GitHub, “New repository settings for configuring pull request access,” 13 February 2026: https://github.blog/changelog/2026-02-13-new-repository-settings-for-configuring-pull-request-access/  
7. OpenAI Cookbook repository, accessed 11 August 2026: https://github.com/openai/openai-cookbook  
8. OpenAI Cookbook PR queue, accessed 11 August 2026: https://github.com/openai/openai-cookbook/pulls  
9. OpenAI Cookbook PR #2659, 6 May 2026: https://github.com/openai/openai-cookbook/pull/2659  
10. OpenAI Cookbook structural summary, accessed 11 August 2026: https://deepwiki.com/openai/openai-cookbook/2-repository-structure-and-contribution-guidelines  
11. Anthropic Claude Cookbooks, accessed 11 August 2026: https://github.com/anthropics/claude-cookbooks  
12. Anthropic cookbook publication site, accessed 11 August 2026: https://platform.claude.com/cookbook/  
13. Anthropic cookbook registry, accessed 11 August 2026: https://github.com/anthropics/claude-cookbooks/blob/main/registry.yaml  
14. Google ADK repository, accessed 11 August 2026: https://github.com/google/adk-python  
15. APSEC cross-project PR study, December 2024: https://conf.researchr.org/details/apsec-2024/apsec-2024-technical-track/4/An-Empirical-Study-of-Cross-Project-Pull-Request-Recommendation-in-GitHub  
16. ESLint PR-review guide, accessed 11 August 2026: https://eslint.org/docs/latest/maintain/review-pull-requests  
17. Jekyll PR-review guide, accessed 11 August 2026: https://jekyllrb.com/docs/maintaining/reviewing-a-pull-request/  
18. GitHub Actions Stale, accessed 11 August 2026: https://github.com/actions/stale  
19. Pydantic AI contributing guide, accessed 11 August 2026: https://pydantic.dev/docs/ai/project/contributing/  
20. OpenInference repository, accessed 11 August 2026: https://github.com/Arize-ai/openinference  
21. LiteLLM CONTRIBUTING, accessed 11 August 2026: https://github.com/BerriAI/litellm/blob/litellm_internal_staging/CONTRIBUTING.md  
22. ESLint AI policy, accessed 11 August 2026: https://eslint.org/docs/latest/contribute/ai-policy  
23. Open Robotics AI policy announcement, 8 May 2025: https://www.openrobotics.org/blog/2025/5/8/osrf-adopts-policy-on-use-of-generative-ai-in-contributions  
24. OSRF full AI policy, 9 May 2025: https://osralliance.org/wp-content/uploads/2025/05/OSRF-Policy-on-the-Use-of-Generative-Tools-Generative-AI-in-Contributions.pdf  
25. OpenInfra AI policy, 2025: https://openinfra.org/legal/ai-policy/  
26. Kubernetes AI maintainership policy, 26 June 2026: https://kubernetes.io/blog/2026/06/26/open-source-maintainership-in-the-age-of-ai/  
27. “AI Policy, Disclosure, and Human in the Loop,” 15 May 2026: https://paperity.org/p/374043744/ai-policy-disclosure-and-human-in-the-loop-how-are-contribution-guidelines-adapting-to  

🧒 Простыми словами: вы стучались не только не в те двери, но часто приносили хозяину новую комнату, которую ему потом обслуживать. Библиотечный баг снимает с хозяина боль. Новый cookbook добавляет ему работу и ответственность. Поэтому сейчас надо быстро ответить во всех 17 живых разговорах, закрепиться там, где вас уже знают, и входить к крупным вендорам через общую техническую проблему, а не через ещё один холодный ноутбук.

11.08.2026, 10:18 PT
