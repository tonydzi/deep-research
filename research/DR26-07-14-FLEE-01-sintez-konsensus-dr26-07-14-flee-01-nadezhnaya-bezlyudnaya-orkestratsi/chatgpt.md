# DR26-07-14-FLEE-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt-deep-research
dr_id: DR26-07-14-FLEE-01
topic: Fully unattended Deep Research fan-out orchestration
collected: 2026-07-16


stats: completed in 8m, 20 citations, 774 searches
---

# Fully Unattended Deep Research Fan-Out on Consumer LLM Web Apps

## Executive Summary

The current best practice is **not** to build an “autonomous browser agent” that improvises its way through ChatGPT, Gemini, and Grok on every run. The reliable pattern is a **durable orchestration state machine** wrapped around **deterministic browser steps**, with explicit **pre-burn probes**, **idempotent resume**, **vendor-specific extraction ladders**, and a **human re-auth / drift-repair lane** when a run has not yet consumed quota. That conclusion is strongly supported by the maturity of durable-execution systems such as Temporal and Inngest, by Playwright’s own guidance to prefer role-based locators and assertions over brittle selectors, and by the growing body of browser-agent tooling that separates deterministic actions from model-assisted discovery. citeturn9search12turn9search10turn19view0turn19view3turn18view2

For **ChatGPT**, unattended orchestration is technically viable but still fragile at the report-rendering layer. OpenAI’s current product now officially supports Deep Research plans, report review, and downloads in Markdown/Word/PDF, which is a major improvement over earlier DOM-only scraping. But public GitHub evidence from active browser wrappers shows that Deep Research activation and extraction still break when ChatGPT changes the composer flow or renders reports in an out-of-process iframe; the most robust technical collector today is often a structured data path rather than “scrape what is visibly rendered,” but that path is still undocumented and therefore [single-source]. citeturn40view0turn40view2turn21view0turn25view0turn38view1turn38view2

For **Gemini**, the situation is materially better. Google’s own help flow confirms the two-stage Deep Research lifecycle—submit prompt, review/edit plan, then hit **Start research**—which makes canarying easier. Gemini also has stronger official export/share surfaces than the others, including export to Google Docs, public chat sharing, and persistent activity retention controls. More importantly, Google now offers an official **Gemini Deep Research Agent** with **background execution** and polling via the Interactions API, plus consumer **Scheduled Actions** and the newer **Gemini Spark** automation layer. Those do **not** grant the same “free with my consumer subscription” entitlement as the consumer app, but they are the clearest path for retiring the browser hack. citeturn11view1turn21view1turn30view0turn30view1turn22view4turn41view0turn41view1

For **Grok**, the technical path exists, but the policy risk is qualitatively different. xAI’s current Acceptable Use Policy explicitly prohibits “accessing the Services through automated or non-human means, whether through a bot, script, or otherwise.” At the same time, xAI is expanding automation-friendly surfaces elsewhere—its API, async tooling, and Grok Build headless scripting—but those are not equivalent to unattended automation of the logged-in consumer Grok web app. That makes a no-human Grok consumer pipeline a bright-red compliance risk even if it works technically. citeturn12view0turn21view3turn41view2turn23search10

My bottom-line recommendation is therefore:

**Build a two-vendor unattended production path first: ChatGPT + Gemini, using a durable ledgered state machine and explicit pre-burn probes; keep Grok out of unattended consumer automation unless you explicitly accept suspension risk.** This recommendation is **[established]** for architecture, and **[emerging]** for the exact vendor-specific collectors. Its main assumption is that the objective is **consumer-subscription Deep Research**, not API-based equivalents. citeturn40view0turn11view1turn12view0turn22view4turn22view0

## Key Findings

| Key finding | Confidence | Why it matters | Evidence |
|---|---|---|---|
| A **durable workflow / ledger** is the right orchestration primitive for “submit → wait 10–40 min → resume → collect,” not a single long-lived browser process. | [established] | Long-running work survives crashes, deploys, and retries only if state is externalized and resumable. | Temporal explicitly frames workflows as durable, long-running, crash-resumable executions; Inngest makes the same case with persisted steps and sleeps. citeturn9search12turn9search18turn9search10turn9search13 |
| The safest UI strategy is **deterministic role/ARIA locators first**, **partial aria-snapshot canaries second**, and **LLM/semantic discovery only as a fallback**. | [established] | This is how you survive copy tweaks and DOM churn without silently clicking the wrong control. | Playwright recommends user-facing locators such as `getByRole()` and provides aria snapshots with partial matching; Stagehand’s `observe()` is explicitly for discovering/validating actions before acting. citeturn19view0turn19view1turn19view3turn18view2 |
| **Live self-healing on quota-bearing runs is a bad default.** Use semantic discovery in **dry-run / drift-repair mode**, not during the actual paid Deep Research submission. | [established] | The quota cost of one malformed submission is higher than the value of a “clever” autonomous save. | This is an inference from Playwright’s deterministic testing practice plus Stagehand’s “discover/validate before acting” design. The tooling supports discovery; it does not prove that autonomous live healing should be trusted on irreversible steps. citeturn19view3turn18view2turn17search6 |
| **Gemini currently has the best official retirement path** away from consumer-web automation. | [established] | Google now exposes an official Deep Research Agent with background execution and polling, while consumer Gemini also offers Scheduled Actions and Spark. | Google documents the Gemini Deep Research Agent, background polling, Scheduled Actions, and Spark. citeturn22view4turn22view2turn41view0turn41view1 |
| **ChatGPT consumer Deep Research is more automatable than before, but still brittle at extraction time.** | [emerging] | OpenAI added official report downloads, but active browser wrappers still report breakage from composer drift and OOPIF extraction quirks. | Official help/release notes document Deep Research plans and Markdown/Word/PDF export; Oracle issues document real July 2026 breakages in activation and out-of-process iframe extraction [single-source]. citeturn40view0turn40view2turn21view0turn38view1turn38view2 |
| **Grok consumer unattended automation is policy-hostile.** | [established] | You can make it work technically, but the policy text is explicit enough that “works” does not equal “acceptable.” | xAI AUP bans access through automated or non-human means, while xAI separately promotes Grok Build headless scripting and API automation. citeturn12view0turn21view3turn41view2turn23search10 |
| Projects that hard-code AI consumer-web UIs often **break fast, stagnate, or get archived**, while broader browser-automation layers stay maintained. | [established] | This is the strongest practical argument for keeping vendor logic thin and orchestration generic. | `cbusillo/chatgpt-automation-mcp` is archived and explicitly marked “OUT OF DATE”; `browserbase/gemini-browser` was archived in May 2026; active repos like `10x-chat` and `conduit-bridge` stay broader and thinner. citeturn16view2turn35view0turn16view0turn36view0 |
| Exact unattended **session longevity is not published** for these consumer products; forced re-auth must be treated as inevitable and first-class. | [emerging] | Any architecture that assumes “login lasts forever” will eventually miss runs. | Google’s sign-in help emphasizes cookies/trusted devices; Workspace admins can set session length; OpenAI has active session controls, but neither vendor publishes a simple fixed TTL for personal unattended browser sessions. Exact TTL: [no source found]. citeturn15search21turn15search4turn15search9 |

## Contradictions & Debates

A real tension now exists between **“consumer-web Deep Research is the only subscription path”** and **“official APIs are finally becoming agent-grade.”** OpenAI has background execution and the Responses API with web search, file search, computer use, and MCPs; Google has a proper Deep Research Agent with background polling; xAI has API tools plus Grok Build headless scripting. But none of those, today, cleanly replicate the exact value proposition of “run the premium consumer Deep Research feature I already pay for via my web subscription quota.” The official automation surfaces are real, and improving fast, but they are still not one-for-one substitutes for the consumer web experience. citeturn22view0turn22view1turn22view4turn21view3turn41view2

There is also a design debate between **LangGraph-style checkpointing** and **Temporal/Inngest-style durable execution**. LangGraph documents persistence and durable execution for long-running, stateful agents, and for a single always-on hub with low concurrency that may be enough. The steelman case for LangGraph is operational simplicity: fewer moving parts, good state checkpoints, and explicit interrupt points. The counterargument—raised by a competitor, so [single-source][low-authority]—is that raw checkpointing is not the same as production-grade durable execution when workers crash, versions roll, and side effects must be retried safely. For this specific use case, where a bad replay could resubmit a quota-burning Deep Research job, the stronger side is the durable-workflow camp. citeturn10search8turn10search3turn10search7turn9search12turn9search10turn10search0

Another disagreement is over **what should count as the “stable extraction surface.”** The officialist position says: always prefer product-supported outputs like PDF, Word, Markdown, Docs export, or share links, because they are intended for users and less likely to vanish without warning. The opposing, more engineering-driven view says: a structured backend payload is often much more stable than rendered UI, especially when UIs move into cross-origin or out-of-process frames. ChatGPT is the strongest example of this split. Official exports are now materially better than they were in early 2025, but active field reports show rendering-layer collection can still fail in ways that structured fetches avoid. My view is mixed: **prefer official surfaces when they are scriptable and integrity-checkable; otherwise prefer structured internal data over DOM scraping, but tag it as [single-source] and keep a parser-fix lane.** citeturn40view2turn21view0turn25view0turn38view2

The most important contradiction is policy, not code: **xAI is simultaneously automation-friendly and automation-hostile**, depending on which surface you use. xAI’s docs strongly support headless scripting and agent integration for **Grok Build** and the **API**, yet the consumer AUP forbids automated/non-human access to the service. The less convincing argument is “they probably only mean abusive bots.” The steelman version of that argument is that xAI clearly distinguishes supported automation products from consumer chat, so maybe pragmatic enforcement will target only abusive scale. The stronger argument is simpler: the policy text is explicit, current, and enforceable, so a production unattended consumer Grok pipeline should be treated as knowingly non-compliant. citeturn12view0turn21view3turn41view2turn27news37

## Best Practices

The orchestration layer should be built as a **finite-state machine with a job ledger**, not as an agent that “just keeps the browser open.” A minimal resilient state graph is: `created → vendor_preflight → prompt_staged → mode_verified → submit_committed → started → waiting → ready_to_collect → collected → normalized → delivered`, plus `needs_reauth`, `drift_suspected`, `quota_burn_suspected`, and `aborted`. Every transition should write durable state, timestamps, hashes, screenshots/aria snapshots, and the vendor thread identifier if known. This is directly aligned with durable-execution practice: long-running work should be restartable from event history, and retries must not re-run a side effect unless the system can prove the prior attempt did not commit. citeturn9search12turn9search5turn9search10turn9search14

The **canary-before-burn** pattern should have three gates before the irreversible submit step. First, a **surface gate**: role-based and aria-snapshot checks confirm the expected vendor composer exists and the Deep Research affordance is present. Second, a **payload gate**: the exact prompt as staged in the composer is re-read from the DOM or accessibility tree, normalized, size-checked, and hash-compared to the source prompt. Third, a **mode gate**: the vendor-specific Deep Research lifecycle signal must appear before a submit is considered safe. Playwright provides the primitives for this—role-based locators, auto-waiting, and aria snapshots with partial matching—and Stagehand can be used to validate or discover actions when deterministic locators fail. citeturn19view0turn19view1turn19view2turn18view2

For **ChatGPT**, the best mode proof is not “the button looked selected,” but “the Deep Research plan lifecycle is visible.” OpenAI’s current help text says Deep Research creates a proposed research plan before research begins, lets the user review/modify it, then eventually opens a fullscreen report view with citations and multiple download formats. In practice, that means the submitter should refuse to mark `submit_committed` until it can prove that the Deep Research pathway is active and the run has generated the expected lifecycle markers, not just that the composer dropdown contained the right string. citeturn40view0turn40view1turn40view2

For **Gemini**, the canary is easier because Google documents the flow unambiguously: submit prompt, plan appears, optional edit, then **Start research** must be clicked. That means an unattended orchestrator should explicitly distinguish `plan_ready` from `research_started`; the absence of the **Start research** action should halt the run rather than “wait and hope.” After start, the daemon can safely detach and come back because Google documents that the report is completed asynchronously and surfaced in the chat thread when ready. citeturn11view1

For **selector resilience**, the hierarchy should be: deterministic Playwright locator → normalized locator → aria-snapshot partial matcher → Stagehand `observe()` in dry-run → human-approved patch. Playwright’s new `locator.normalize()` is relevant here because it can convert implementation-detail selectors toward more resilient references. The key operational rule is that **semantic fallback should be discovery-only by default**. Let it propose where the Deep Research toggle or export button moved, but do not let it commit a real research submission without either a canary template match or human approval. citeturn19view4turn19view0turn19view3turn18view2

For **report extraction**, always prefer the **least-render-dependent surface** that is both available and integrity-checkable. For ChatGPT, official report downloads and shared links are now real product surfaces; for Gemini, export to Docs and public share links are strong official outputs; for Grok, history and share links exist, but official export is weak on the consumer side. DOM scraping should be the lowest-preference collector, except where no structured or export path exists. citeturn40view2turn30view2turn21view1turn30view0turn21view2

The **do-not-do list** is short but important. Do **not** rely on CSS/XPath selectors keyed to volatile class names. Do **not** let a model “guess” its way through a quota-bearing submit. Do **not** treat a visible plan as proof that research actually started. Do **not** trust a browser download unless the orchestrator verifies a file landed, has non-trivial size, and matches the expected MIME/content signature. And do **not** run unattended consumer Grok automation on an account you care about keeping. citeturn19view3turn11view1turn12view0

## Failure Modes

The dominant failure mode is **UI drift before commit**: the mode toggle moves, the label changes, the dropdown shape changes, or the product inserts a wrapper flow. This is not hypothetical. Active July 2026 Oracle issues show ChatGPT Deep Research activation failing because the expected composer option was not found, and separate failures where the system captured only wrapper text from the “Deep Research App” rather than the final report. That is exactly the pattern that burns quota if your orchestrator cannot distinguish “not actually started” from “started but still running.” citeturn38view1turn38view2

The second failure mode is **successful submission but bad collection**. ChatGPT is the clearest case: a run can succeed, the report can be visibly complete, and the collector can still save only the wrapper/tool-call shell. Community exporters that fetch `/backend-api/conversation/<id>` and parse the message tree exist precisely because the render layer is not always the most trustworthy source of truth. That method is technically strong but still [single-source] and unofficial, so it must sit behind contract tests and a quick repair path. citeturn25view0turn38view2

The third failure mode is **session breakage**. None of these consumer products publish a simple “session lasts X days” guarantee for unattended browser profiles. Google explicitly ties sign-in persistence to cookies and trusted-device state, and Workspace admins can force shorter session lengths. OpenAI has added active-session controls that make session review and revocation more visible. The operational conclusion is straightforward: persistent profiles are necessary, but they are not a substitute for a `needs_reauth` state and an out-of-band alert that asks a human to reopen the vendor profile only when authentication actually expires. Exact TTL remains [no source found]. citeturn15search21turn15search4turn15search9

The fourth failure mode is **policy or rate-limit enforcement**. OpenAI’s Terms of Use prohibit automatically or programmatically extracting data or output and prohibit interference or bypassing rate limits; Google’s Terms bar abusive/disruptive use and automated access that violates machine-readable instructions; xAI’s AUP is stricter, explicitly banning automated/non-human access. These documents do not provide a neat red/green line for a low-cadence personal automation on OpenAI or Google consumer surfaces, so those remain gray-zone risks. Grok is different: the text is explicit enough that the risk is not gray. Reuters’ July 2026 reporting that xAI had already suspended over 52,000 accounts in 2026—while related to broader abuse, not specifically browser automation—shows the company is willing to enforce at scale. citeturn14view0turn14view1turn12view0turn27news37

The fifth failure mode is **project abandonment**. Repositories built around specific AI web UIs often go stale as soon as the UI changes. `chatgpt-automation-mcp` is archived and marked “OUT OF DATE.” `browserbase/gemini-browser` was archived in May 2026. Even active broader bridges such as `10x-chat` and `conduit-bridge` are intentionally generic rather than tightly coupling themselves to Deep Research flows. That pattern matters: your code should assume that Deep Research-specific selectors will churn faster than generic “login, compose, read thread, export reply” primitives. citeturn16view2turn35view0turn16view0turn36view0

## Strategic Recommendations

The architecture I would choose is a **daemonized, vendor-isolated, ledger-driven orchestrator** with one dedicated Firefox profile per vendor, but with the browser layer treated as a **worker**, not the source of truth. The source of truth is the orchestration ledger: prompt hash, vendor, target mode, captured thread IDs/URLs, phase transitions, mode-proof artifacts, and extraction artifacts. Temporal and Inngest both support this operating model well; if you do not want another service, a local SQLite/Postgres state machine can also implement it cleanly as long as every side effect is guarded by idempotency rules. citeturn9search12turn9search10turn9search4

A good vendor fallback ladder is different per product.

For **ChatGPT**, the ladder should be: **UI automation for submission → official report download if integrity-checkable → structured conversation fetch [single-source] → shared link snapshot → human escalation**. The rationale is that OpenAI now officially supports report downloads in Markdown/Word/PDF, but active field evidence shows OOPIF-rendered Deep Research reports can still defeat naive DOM tools; the private conversation endpoint remains the best technical escape hatch when official exports cannot be exercised reliably. citeturn40view2turn21view0turn25view0turn38view2

For **Gemini**, the ladder should be: **UI automation for prompt + plan approval + Start research → export to Docs → public share link → activity/history recovery → human escalation**. Google’s official surfaces are better here than on ChatGPT or Grok: the plan/start lifecycle is documented, Docs export is first-party, and public links preserve the full conversation snapshot, including Canvas artifacts and generated media. citeturn11view1turn21view1turn30view0turn30view1

For **Grok**, the strategic answer is: **do not build unattended consumer orchestration unless you deliberately accept policy risk**. If you still insist, the technical ladder would be **UI automation → public share link → DOM text extraction → human escalation**, but I would not advise putting that in a scheduled unattended fan-out on an account you care about. If you want a supported Grok automation surface, use the **API** or **Grok Build headless/ACP** instead; just be clear that this does **not** solve the consumer-subscription Deep Research requirement. citeturn12view0turn21view2turn21view3turn41view2

The named implementation options are:

| Option | What it is | Strengths | Weaknesses | Best fit |
|---|---|---|---|---|
| **Ledger FSM Local** | systemd/cron + Python/TS daemon + SQLite/Postgres + Playwright workers | Easiest to repair locally; lowest moving parts; full control over probes | You own retries, observability, and replay correctness | Single hub, modest concurrency |
| **Inngest Durable Steps** | Event/cron-driven durable functions with resumable steps | Strong durability with lighter mental model than Temporal; good for async waits | Extra platform dependency; less universal mindshare than Temporal | Small team wanting durability without heavy workflow ops |
| **Temporal Workflow Engine** | Full durable-execution workflow service | Strongest replay, retries, visibility, versioning | Highest operational overhead and complexity | Multi-tenant or higher-scale orchestrator |
| **LangGraph Runtime** | Checkpointed stateful agent graph | Good developer ergonomics; built-in interrupts/persistence | Replay-side-effect risk unless carefully designed; weaker fit for irreversible quota-burning UI submits | Only if you already run LangGraph heavily |

This ranking is evidence-based: **Temporal** is the strongest durability story, **Inngest** is the most practical durable-step system for many teams, and a **local ledger FSM** is the most repairable for a single-owner hub. For your stated constraint that a non-technical owner must be able to fix it, my single recommendation is:

**Recommendation:** choose **Ledger FSM Local** now, with an upgrade path to **Inngest** later if concurrency or organizational complexity grows. Keep the browser layer deterministic, keep semantic recovery in dry-run mode, and exclude unattended Grok consumer runs.  
**Confidence:** **[established]** for the architecture, **[emerging]** for some vendor-specific collectors.  
**Main assumption:** one always-on hub, low-to-moderate concurrent jobs, and a requirement to preserve consumer-subscription economics rather than switch entirely to APIs. citeturn9search12turn9search10turn9search4turn19view3turn18view2turn12view0

## Action Plan

Start by implementing the orchestration ledger and the **three-gate canary** before you touch more vendor UI logic. Concretely: create a `jobs` table, a `vendor_runs` table, an `artifacts` table, and a `transitions` append-only log; persist prompt hash, vendor, source prompt, normalized prompt, expected mode, commit token, thread identifier, collector result hash, and a small set of proof artifacts such as aria snapshots and screenshots. This gives you idempotent resume and makes a malformed run diagnosable instead of mysterious. citeturn9search12turn9search14turn19view1

Next, build the **submission worker** in quota-safe order:  
first load the vendor profile;  
then run the surface gate;  
then stage the prompt and re-read it for exact-match proof;  
then verify the Deep Research mode-specific lifecycle signal;  
only then allow the submit transition to commit.  
For ChatGPT, the canary success condition is plan lifecycle visibility. For Gemini, it is plan visibility plus discovery of **Start research** and then a proof that research entered a running state. Runs that fail anywhere before `submit_committed` should hard-abort with zero retries until a human or drift-repair pass clears them. citeturn40view0turn40view1turn11view1turn19view2

Then build **collector workers** that are vendor-specific and versioned separately from the submitter. ChatGPT’s collector should prefer official export if it can prove file integrity, then fall back to a structured conversation fetch parser [single-source], and only then try render-layer scraping. Gemini’s collector should prefer Docs export or a public share link snapshot. Grok should remain manual or excluded from unattended scheduling unless you affirmatively accept the account risk. citeturn40view2turn21view1turn30view0turn25view0turn12view0

After that, add **drift detection and repair governance**. Store a baseline aria snapshot for each vendor’s pre-submit composer state and compare on every run. If the snapshot diff exceeds a small threshold, route the run to `drift_suspected` instead of letting a semantic fallback “wing it.” Use Stagehand `observe()` or similar semantic tooling only in that repair lane to generate candidate new actions for review. This keeps discovery flexible without spending a premium Deep Research task on a wrong click. citeturn19view1turn18view2

Finally, put in place a human lane for only two events: **`needs_reauth`** and **`drift_suspected`**. Everything else should be fully unattended. This keeps human involvement rare, bounded, and economically justified. If a vendor adds a true official scheduling or background Deep Research surface for consumers, that is the exact moment to retire that vendor’s browser path. Gemini is closest to that future already; OpenAI is moving in that direction through Work, Tasks, and the Responses/agent stack; xAI is moving there for API/Build, not for consumer Grok web. citeturn41view0turn41view1turn22view0turn22view1turn33search0turn12view0

## Sources

The list below includes the sources cited in the report, with working URLs and the freshest visible publication/update date. For live documentation pages without a visible publication date, I mark them as **undated live docs, accessed 2026-07-16** rather than inventing a date.

- OpenAI Help Center, **Deep research in ChatGPT**, updated **2026-07-14**.  
  https://help.openai.com/en/articles/10500283-deep-research-in-chatgpt

- OpenAI, **Introducing deep research**, published **2025-02-02** with updates through **2025-07-17** visible on-page.  
  https://openai.com/index/introducing-deep-research/

- OpenAI Help Center, **ChatGPT — Release Notes**, visible entries including **2025-05-08**, **2025-05-12**, and **2026-07-15**.  
  https://help.openai.com/en/articles/6825453-chatgpt-release-notes

- OpenAI Help Center, **Scheduled Tasks in ChatGPT**, updated **2026-07-14**.  
  https://help.openai.com/en/articles/10291617-tasks-in-chatgpt

- OpenAI API docs, **Background mode**, undated live docs, accessed **2026-07-16**.  
  https://developers.openai.com/api/docs/guides/background

- OpenAI API docs, **Migrate to the Responses API**, undated live docs, accessed **2026-07-16**.  
  https://developers.openai.com/api/docs/guides/migrate-to-responses

- OpenAI Help Center, **ChatGPT Shared Links FAQ**, updated **2026-07-13**.  
  https://help.openai.com/en/articles/7925741-chatgpt-shared-links-faq

- OpenAI Help Center, **Exporting your ChatGPT history and data**, updated **2026-07-13**.  
  https://help.openai.com/en/articles/7260999-exporting-your-chatgpt-history-and-data

- OpenAI Help Center, **Data Export: Are my shared links included when I export my data from ChatGPT?**, updated **2026-07-13**.  
  https://help.openai.com/en/articles/7943616-data-export-are-my-shared-links-included-when-i-export-my-data-from-chatgpt

- OpenAI, **Terms of Use**, published/effective **2026-01-01**.  
  https://openai.com/policies/row-terms-of-use/

- Google Support, **Use Deep Research in Gemini Apps**, undated help page, accessed **2026-07-16**.  
  https://support.google.com/gemini/answer/15719111

- Google Gemini product page, **Gemini Deep Research**, undated product page, accessed **2026-07-16**.  
  https://gemini.google/overview/deep-research/

- Google Support, **Export responses from Gemini Apps**, undated help page, accessed **2026-07-16**.  
  https://support.google.com/gemini/answer/14184041

- Google Support, **Share your chats from Gemini Apps**, undated help page, accessed **2026-07-16**.  
  https://support.google.com/gemini/answer/13743730

- Google Support, **Gemini Apps Privacy Hub**, updated **2026-06-29**.  
  https://support.google.com/gemini/answer/13594961

- Google AI for Developers, **Gemini Deep Research Agent**, undated live docs, accessed **2026-07-16**.  
  https://ai.google.dev/gemini-api/docs/deep-research

- Google Cloud, **Gemini Deep Research Agent**, undated live docs, accessed **2026-07-16**.  
  https://docs.cloud.google.com/gemini-enterprise-agent-platform/agents/google/deep-research

- Google Support, **Schedule actions in Gemini Apps**, undated help page, accessed **2026-07-16**.  
  https://support.google.com/gemini/answer/16316416

- Google Support, **Use Gemini Spark to manage your tasks & workflows in Gemini Apps**, undated help page, accessed **2026-07-16**.  
  https://support.google.com/gemini/answer/17094507

- Google, **Terms of Service**, undated live terms page, accessed **2026-07-16**.  
  https://policies.google.com/terms

- xAI, **Acceptable Use Policy**, effective **2026-06-26**.  
  https://x.ai/legal/acceptable-use-policy

- xAI, **Consumer Terms of Service**, effective **2026-06-26**.  
  https://x.ai/legal/terms-of-service

- xAI, **Consumer FAQs**, published **2025-05-12**.  
  https://x.ai/legal/faq

- xAI Docs, **Grok Build overview**, published **2026-07-06**.  
  https://docs.x.ai/build/overview

- xAI Docs, **Headless & Scripting**, published **2026-06-10**.  
  https://docs.x.ai/build/cli/headless-scripting

- xAI News, **Introducing Grok Build**, published **2026-05-25**.  
  https://x.ai/news/grok-build-cli

- Reuters, **Musk’s xAI sues Grok user over sexualized ‘deepfakes’**, published **2026-07-15**.  
  https://www.reuters.com/legal/litigation/musks-xai-sues-grok-user-over-sexualized-deepfakes-2026-07-15/

- Playwright docs, **Locators**, undated live docs, accessed **2026-07-16**.  
  https://playwright.dev/docs/locators

- Playwright docs, **Best Practices**, undated live docs, accessed **2026-07-16**.  
  https://playwright.dev/docs/best-practices

- Playwright docs, **Auto-waiting**, undated live docs, accessed **2026-07-16**.  
  https://playwright.dev/docs/actionability

- Playwright docs, **Aria snapshots**, undated live docs, accessed **2026-07-16**.  
  https://playwright.dev/docs/aria-snapshots

- Playwright Python docs, **BrowserType.launch_persistent_context**, undated live docs, accessed **2026-07-16**.  
  https://playwright.dev/python/docs/api/class-browsertype

- Playwright docs, **Playwright MCP**, undated live docs, accessed **2026-07-16**.  
  https://playwright.dev/docs/getting-started-mcp

- Stagehand docs, **Overview / Observe / Extract**, undated live docs, accessed **2026-07-16**.  
  https://stagehand.dev/  
  https://docs.stagehand.dev/v3/basics/observe  
  https://docs.stagehand.dev/v3/basics/extract

- AgentQL docs, **A query language for the web**, undated live docs, accessed **2026-07-16**.  
  https://docs.agentql.com/concepts/query-language

- Temporal docs, **Workflows**, undated live docs, accessed **2026-07-16**.  
  https://docs.temporal.io/workflows

- Temporal blog, **The definitive guide to Durable Execution**, published **2025-05-06**.  
  https://temporal.io/blog/what-is-durable-execution

- Inngest docs, **Durable Execution / Durable Workflows**, undated live docs, accessed **2026-07-16**.  
  https://www.inngest.com/platform/durable-execution  
  https://www.inngest.com/docs/patterns/durable

- Inngest blog, **How to Build a Durable AI Agent with Inngest**, published **2026-03-17**.  
  https://www.inngest.com/blog/ai-agents-inngest-durable-steps

- LangGraph docs, **Overview / Persistence / Durable execution**, undated live docs, accessed **2026-07-16**.  
  https://docs.langchain.com/oss/python/langgraph/overview  
  https://docs.langchain.com/oss/python/langgraph/persistence  
  https://langchain-5e9cc07a.mintlify.app/oss/javascript/langgraph/durable-execution

- Diagrid blog, **Why Checkpoints Aren’t Durable Execution**, published **2026-03-17** approximately via crawl context [single-source][low-authority].  
  https://www.diagrid.io/blog/checkpoints-are-not-durable-execution-why-langgraph-crewai-google-adk-and-others-fall-short-for-production-agent-workflows

- GitHub, **MikeChongCan/10x-chat README**, accessed **2026-07-16**.  
  https://github.com/MikeChongCan/10x-chat

- GitHub, **elvatis/conduit-bridge README**, accessed **2026-07-16**.  
  https://github.com/elvatis/conduit-bridge

- GitHub, **cbusillo/chatgpt-automation-mcp**, archived **2026-04-27**.  
  https://github.com/cbusillo/chatgpt-automation-mcp

- GitHub, **browserbase/gemini-browser**, archived **2026-05-20**.  
  https://github.com/browserbase/gemini-browser

- GitHub, **svandragt/chatgpt-export README**, accessed **2026-07-16** [single-source].  
  https://github.com/svandragt/chatgpt-export

- GitHub, **steipete/oracle browser mode docs**, accessed **2026-07-16**.  
  https://github.com/steipete/oracle/blob/main/docs/browser-mode.md

- GitHub issue, **Oracle #281 — Deep Research activation cannot find composer option**, opened **2026-07-01** [single-source].  
  https://github.com/steipete/oracle/issues/281

- GitHub issue, **Oracle #297 — Placeholder-capture misses Deep Research App wrapper**, opened **2026-07-07** [single-source].  
  https://github.com/steipete/oracle/issues/297

- GitHub, **andylizf/deep-research-skill README**, accessed **2026-07-16** [single-source].  
  https://github.com/andylizf/deep-research-skill

- Google Support, **Stay signed in or out of your Google Account**, undated help page, accessed **2026-07-16**.  
  https://support.google.com/accounts/answer/54490

- Google Workspace knowledge base, **Set session length for Google services**, undated help page, accessed **2026-07-16**.  
  https://knowledge.workspace.google.com/admin/security/set-session-length-for-google-services

- OpenAI Help Center, **ChatGPT release notes: Active account session controls**, visible release entry **2026-07-15**.  
  https://help.openai.com/en/articles/6825453-chatgpt-release-notes