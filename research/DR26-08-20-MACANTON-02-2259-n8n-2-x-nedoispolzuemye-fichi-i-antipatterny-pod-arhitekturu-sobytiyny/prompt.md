# DR26-08-20-MACANTON-02-2259 — the exact prompt sent to every model

# DEEP RESEARCH REQUEST — n8n: under-used capabilities and high-value patterns for a specific small AI-lab automation fleet

You are doing deep, source-backed research for a working engineering team, not writing a blog post. We already run n8n in production. We do NOT need "what is n8n" or "top 10 automation ideas". We need to find the capabilities and patterns we are currently NOT using that would pay off for OUR concrete architecture and OUR concrete recurring failures, described in detail below.

Cite primary sources (official n8n docs, n8n release notes/changelog, n8n GitHub issues/PRs, n8n community forum threads with dates, source code) and give version numbers. Where something changed recently, say WHEN and in WHICH n8n version. Where you are unsure or where the community disagrees, say so explicitly instead of smoothing it over. A confident wrong answer costs us more than an honest "not established".

---

## PART 1 — OUR SETUP (facts, measured, not aspirational)

### 1.1 The n8n instance
- Self-hosted n8n, **version 2.14.2**, running on a **VPS we own** (not n8n Cloud, not on our main always-on desktop). Reachable over HTTPS. Public REST API at `/api/v1/` with an `X-N8N-API-KEY` header.
- **89 workflows** (last full deterministic audit: 71 active / 18 dormant, 1675 nodes, 63 sub-workflow links). Roughly 10,000 executions per 10 days.
- A WAF sits in front: **API and `/webhook/*` calls return 403 unless the client sends a browser-like `User-Agent`**. We lost weeks to this once — a `urllib` POST to our dead-man's-switch webhook silently 403'd while a `curl` smoke test returned 200, because curl sends its own UA. (Lesson we already learned: test an endpoint with the same client the production code uses.)
- Execution log retention: n8n prunes to roughly the **last ~10k executions ≈ last 10 days**, which destroys our ability to do longer-window forensics.

### 1.2 What actually runs there (the workload clusters)
1. **CRM cluster (~38 workflows)** — Telegram-based lead management. A "Main Agent" (inbound supervisor) that **polls MongoDB every 1 minute** for inbound leads: 4155 runs in a 10-day window (~413/day), ~0% error rate. An "Inner Agent" (CRM-UI brain) that exposes **20 tool sub-workflows** via `toolWorkflow` — this is our template for "LangChain agent with tools".
2. **Content/summary bots** — scheduled read → LLM-summarize → post. Busiest: a translation bot at 2901 runs (~288/day).
3. **Audio** — Whisper STT and ElevenLabs TTS as reusable sub-workflows; "Personal Audio Summary" at 1435 runs (~144/day).
4. **Standalone Telegram agent bots** — staff assistant, docs Q&A bot, a longevity/health bot.
5. **Dev/RAG** — a Postgres **pgvector** RAG backend, a PR-automation experiment, a docs-site updater.
6. **Reliability workflows we built ourselves (2026-07-04)**:
   - a **dead-man's-switch watchdog** (one workflow, two triggers, shared `$getWorkflowStaticData('global')`): producers POST `{"source":"...","every_min":N}` heartbeats to a webhook; a 5-minute schedule trigger flags any source overdue by >2× its interval and fires a single alert path;
   - a **central Error Trigger workflow** wired as `settings.errorWorkflow` on **all 63 active workflows** (idempotent re-run script);
   - an **anti-storm gate** inside the error catcher: dedup identical `workflow::node` failures within 10 minutes, global rate-limit ≤5 alerts/minute, one "storm, N suppressed" summary per 5 minutes.

### 1.3 How we write to n8n
We do NOT click in the UI for production changes. We drive the **public REST API from Python** (`GET/PUT /api/v1/workflows/{id}`, `POST /workflows/{id}/archive`, `/api/v1/executions?status=error&includeData=true`). Before every edit we snapshot all 89 workflows to disk, and each edit writes a before/after pair for rollback.

Known API gotchas we have already paid for:
- `PUT /api/v1/workflows/{id}` accepts **only** `{name, nodes, connections, settings}` — read-only fields must be stripped.
- The `settings` object is **schema-strict**: any key outside the whitelist (`executionOrder, errorWorkflow, executionTimeout, saveDataErrorExecution, saveDataSuccessExecution, saveExecutionProgress, saveManualExecutions, timezone, callerPolicy, callerIds`) returns `400 settings must NOT have additional properties`. Legacy workflows carrying e.g. `binaryMode: "separate"` 400'd until we stripped it. n8n's own UI silently strips such keys on save, which is why the drift is invisible until you use the API.
- We also use the **n8n MCP server** (community `n8n-mcp`), deliberately in **docs-only mode** (node documentation + validation tools loaded, the ~15 management tools kept OFF, API key kept out of the MCP env). Reason: blast radius. We separately know n8n now ships an **instance-level MCP server** (`<instance>/mcp-server/http`, GA, two-level opt-in: instance toggle + a per-workflow "Available in MCP" flag, OAuth2 or rotatable access token), which we have NOT piloted yet.

### 1.4 The wider fleet n8n has to cooperate with
- **Hub**: an always-on Windows desktop with 2 GPUs, ~230 scheduled tasks in Windows Task Scheduler, holds the knowledge vault (a large Markdown "second brain"), local embeddings/Whisper on GPU, browser sessions, and all secrets. This is where "IP-sensitive" outbound actions must happen.
- **Mac laptop**: ~44 launchd jobs.
- **A small always-on Linux anchor node** (VPS).
- **Two employee laptops** that sleep, close their lids, and log out.
- Inter-machine bus = a file-sync mesh plus a Telegram group used as the live coordination rail.
- **A live, working event rail already exists**: an n8n workflow `Telegram trigger → filter → HTTP Request → a local "wake-listener" HTTP endpoint on the hub`. It fires several times a day. So "n8n as the always-on ear that wakes a local machine" is proven in production for us, not theoretical.
- A second webhook workflow (`GitHub PR webhook`) is **active with zero executions** — built and never connected to any repository.

### 1.5 Our architectural rule (decided 2026-06-16, still in force)
The dividing line is **where the data and the compute live**:
- Cloud n8n reaches: the internet, Telegram/MTProto, our CRM data bus (Mongo/Postgres), hosted LLM APIs, webhooks.
- Cloud n8n does NOT reach: the knowledge vault, the GPUs, local SQLite fact databases, the secret store, local file pipelines.
- **Rule:** touches vault/GPU/local files → local only. Always-on inbound + messaging + no reasoning needed → cloud n8n. Needs both → **split loop**: cloud orchestrates and delivers, local thinks and writes, connected by an **outbound-only job queue** (local polls cloud; the home machine exposes no inbound ports). Idempotency mandatory: `pending → claimed → ready → delivered` with idempotency keys, at-least-once semantics.
- A visual control-tower for local jobs was explicitly **rejected** by the owner ("I don't need the n8n canvas, the HTML dashboards you build me are enough"). So "it looks nice on a canvas" is not a valid argument here.
- Recent verdict (2026-08-20) on a related question: **n8n is our ears and trigger and CRM — it is not our brain and not our LLM budget.** Do not propose putting reasoning or the owner's writing voice into n8n.

### 1.6 A hard economic constraint you must respect
We pay for several LLM **subscriptions** (~$540/month) that come with generous included quotas, and our standing rule is: **burn what is already paid for**. Our n8n LLM nodes currently sit on a **metered pay-per-token OpenAI account**, which directly violates that rule. We have a local HTTP sidecar (`POST /ask {provider, prompt}` with a shared-secret header) that wraps our **subscription CLIs** (Claude Code CLI, Codex CLI, a Gemini CLI) and was written specifically so n8n could call subscription-backed models over plain HTTP Request nodes instead of the paid OpenAI node.

**Its known weaknesses** (we need your research to address these, not repeat them):
- it is a single-threaded `BaseHTTPServer` with no per-request timeout, while the underlying `ask()` retries 3× with 2/4/8s backoff against **900s (Claude) and 1800s (Codex/Gemini) timeouts** — so ONE stuck call blocks the entire sidecar for 15+ minutes and every other flow queues silently behind it;
- it reads its shared secret from an env var at import time, so it dies at startup if launched without the environment (a failure class that has bitten us before);
- the Codex rail runs with `--sandbox read-only`, so it structurally cannot write files;
- it is a bare `ask(provider, prompt) -> str` text contract — it is NOT an agent with tools.

### 1.7 A related structural pain (why we care about n8n as a scheduler at all)
Some of our night jobs are launched by the **operating system scheduler** (Task Scheduler / launchd) — those always start, regardless of which account is logged in. Others are **app-level scheduled tasks inside a desktop LLM app**, which are bound to one account's session and one vendor's quota: if that quota is exhausted, they cannot run at all and cannot be pushed onto a different vendor. We also just found that a CLI tool (`gh`) **cannot read the macOS Keychain when launched under launchd**, which breaks credentialed jobs in a way that is invisible until you look.
Measured: **9,066 scheduler ticks/day** across two machines; 45 pollers more frequent than 30 minutes on one machine and 26 on another; background robots consume **~60% of our entire token spend**; our worst poller runs every 2 minutes (720 ticks/day); one voice-processing lane burned ~17.7M tokens/day to process **0.57 voice messages/day**. One lead-thread poller wastes **81.6% of its ticks** on nothing.

---

## PART 2 — OUR SCAR TISSUE (the failure classes n8n has already inflicted on us)

Any recommendation you make must survive these. If a proposed pattern is vulnerable to one of them, say so and give the mitigation.

1. **Zombie schedules.** Five workflows reported `active=false` but their schedule triggers **kept firing daily** (executions show `mode:"trigger"` at a fixed time every day) — a stale trigger registration. Setting `active=false` was NOT enough to stop them; we had to force-toggle activate→deactivate or delete the schedule node. Each zombie run fanned out expensive LLM agents over whole chat histories — this was our single largest hidden cost drain. **We want to know the actual root cause in the n8n codebase, whether it is fixed in later versions, and how to detect a zombie deterministically via the API.**
2. **A structural edit silently deactivating a workflow.** Using the MCP `removeNode` operation to clean up orphan/disconnected nodes flipped `active=false` on our CRM agent and inbound bot — production was off for a full day and nothing alerted. Related: the MCP layer's strict validation **refuses to save any workflow containing a disconnected node**, while n8n core tolerates them — so the "safe" tool forced destructive edits that the raw REST PUT would not have required. Our conclusion was: for orphan-heavy workflows, **raw REST PUT is safer than the MCP tool**. Confirm or refute this with sources.
3. **Model-alias cost trap.** Our OpenAI spend hit **$216 in 15 days**, ~92% of it output tokens on a flagship model, because `lmChatOpenAi` nodes were set to the **bare alias** `gpt-5` which the provider resolved to the expensive flagship, while explicit versioned strings billed to cheaper lines. Our first sweep only caught the bare alias and **missed nodes explicitly set to the flagship version string**, so the bleeding continued for another week. We now sweep all workflows' node JSON for every pricey model string via the REST API.
4. **Retries applied to a non-retryable error.** Most of our "errors" on two busy workflows were `429 insufficient_quota` — a billing-balance condition. **Retries make it strictly worse** (more failed calls, more spend). We now read the real execution error before adding any retry.
5. **Python Code node failing on `_input`.** A Code node in Python mode threw `name '_input' is not defined` in `PythonTaskRunnerSandbox.runUsingIncomingItems`, and every daily run failed for weeks — while byte-identical `_input.first().json` syntax ran fine 4155× in another workflow of the same node type/version/mode. We never isolated the root cause; our workaround is "convert Python Code nodes to JavaScript". **We want the actual root cause and the reliable rule for when Python Code nodes are safe.**
6. **`$getWorkflowStaticData('global')` flush lag.** Persisting state between separate webhook executions has a **multi-second flush lag** in queue/worker mode — a write-then-read inside ~3s misses the write. Fine for minute-scale heartbeats, fatal for anything tighter. We know n8n now has a **Data Tables** API; we have not evaluated it.
7. **Log retention destroys forensics** (see 1.1).
8. **A watchdog that lives inside the thing it watches is not a watchdog.** This is a hard rule for us. n8n currently has its own external watchdog. Any proposal to move our *fleet* watchdogs into n8n must explain how it avoids the circular dependency.
9. **Silent-degradation in general.** Our most expensive failures are not crashes — they are green-looking things that quietly do nothing (403 with a 200 smoke test, a webhook with zero executions, a "red" alarm that has been wrong for so long people stopped reading it).

---

## PART 3 — WHAT WE WANT FROM YOU

Answer the seven blocks below. **Rank everything by value to US given Part 1 and Part 2** — not by general popularity. For every recommendation give: (i) what it is and the n8n version it requires, (ii) a concrete implementation sketch we could build (node types, API shape, or config), (iii) the gotchas/failure modes, (iv) an explicit "do NOT do this if…" line.

### (a) Under-used n8n 2.x capabilities, mapped to our use cases
Cover at minimum, and tell us which ones we should adopt first:
- **Queue mode / worker scaling** (Redis-backed): what it actually buys a single-tenant instance with our volume (~1k executions/day, one 1-minute poller dominating); does it change the `$getWorkflowStaticData` semantics we depend on; operational cost.
- **Error workflows** — beyond what we already do: per-workflow vs global patterns, what the Error Trigger payload actually contains, how to build a real **dead-letter queue** on top of it, retry-from-failure / manual replay of a failed execution via API.
- **External secrets** (which providers, which license tier, whether it removes credentials from the DB) — relevant because our rule is that secrets stay local.
- **Source control / git-backed workflow deployment**: what exactly is versioned, what is NOT (credentials? execution data? per-instance IDs?), how environments/branches are meant to work, what the licensing gate is, and — critically for us — **whether it can coexist with our REST-API-driven edits or whether the two fight each other**.
- **Execution data retention / pruning configuration**: how to keep longer forensic history without unbounded growth; what `EXECUTIONS_DATA_*` env vars actually do in 2.x.
- **Sub-workflow patterns**: `executeWorkflowTrigger` vs `toolWorkflow`, data passing and typing, error propagation from child to parent, and the observability quirk that sub-workflow executions don't log separately the way we expect.
- **Evaluations** (the newer n8n eval features): what they measure, whether they are usable for a non-benchmark production sanity check.
- **AI Agent / LangChain nodes**: current node inventory, memory backends (we use Postgres chat memory and it has been our flakiest layer at ~11% error), tool-calling patterns, structured output, and how to make the agent call a **generic HTTP endpoint** as its model backend (see (c)).
- **MCP server node and MCP client node**: what our instance-level MCP actually exposes at 2.14.2 vs current versions (the docs drifted from "read-only authoring" to "workflow building + data tables"), how the two-level opt-in behaves, and the honest security assessment vs an API key.
- **Data Tables**: are they the right replacement for `$getWorkflowStaticData` given failure class 6?
- Anything else genuinely under-used that fits us — but only if it fits Part 1.

### (b) "n8n as event-driven EARS for local machines" — the pattern we are actually betting on
We already have `Telegram trigger → filter → HTTP → local wake-listener`. Deepen it:
- Reliability and delivery semantics of **Telegram triggers** in n8n: Bot API polling vs webhook mode vs MTProto; what happens to messages during an n8n restart or a workflow deploy; duplicate delivery; ordering; whether a missed window is recoverable.
- **Schedule trigger vs event trigger reliability**: catch-up behaviour after downtime, `timezone` handling, DST, and the deterministic way to detect the **zombie schedule** class (failure 1) via API or DB.
- Waking a machine behind NAT: our outbound-poll queue vs an n8n-initiated call over a private mesh network (we use a tailnet). Security posture of each; how to keep the home machine with **zero inbound ports**; idempotency and at-least-once handling; what to do when the local side is asleep (queue depth, catch-up storms, poison messages).
- The **webhook-with-zero-executions** class: how to prove a webhook is actually wired end-to-end, and how to alarm on "this trigger has not fired in N days" — noting that absence-of-event cannot itself arrive as an event.
- Wait node / resume-webhook patterns for human-in-the-loop approvals, and their durability across restarts.

### (c) Making n8n use OUR paid subscription LLM quotas instead of a metered API
This is a direct economic requirement (1.6). We want the practical menu:
- Driving an **HTTP Request node** (or an n8n AI node with a custom base URL) against a local OpenAI-compatible shim, including how to expose an on-prem endpoint to a cloud n8n instance safely (private mesh vs tunnel vs reverse proxy with mTLS), and what breaks with streaming/SSE.
- What n8n's OpenAI-compatible / custom-base-URL support actually covers in 2.x for the chat-model, embeddings, and agent nodes — and where it hard-codes provider behaviour.
- **Timeouts, retries, and concurrency**: correct HTTP Request node timeout/retry settings for a backend whose p95 is minutes; how to stop one slow call from starving the rest; whether queue mode with N workers is the right answer or whether the fix belongs in the sidecar (we already know ours must become threaded/async — tell us the right server shape and the right concurrency limit given a CLI-backed backend where each call spawns a process).
- Honest risk assessment of routing subscription-CLI traffic through an automation server: rate limits, per-account concurrency, terms-of-service exposure, and what a sane fallback ladder between providers looks like inside n8n (fan-out race vs sequential fallback vs circuit breaker).
- Where this is a **bad idea** and we should just pay per token.

### (d) Monitoring n8n itself — who watches the watchman
- The **Prometheus metrics endpoint**: what it exposes in 2.x, which metrics actually predict our failure classes, what it does NOT cover.
- Health/readiness endpoints, DB and queue health signals, memory-leak indicators (we have seen reports of SQLite-backed memory growth; we need to know what our Postgres-backed instance should be watched for).
- The correct **external** dead-man's-switch design for an n8n instance (must live outside n8n, per Part 2 #8).
- Error-workflow best practices at scale: alert dedup/rate-limiting (we built our own — is there now a native way?), distinguishing "one workflow broken" from "instance-wide outage", and getting a **daily green receipt** rather than only red alarms.
- How to detect **silent** failure specifically: a trigger that stopped firing, a workflow deactivated by an edit, a credential that expired, a WAF that started 403ing one client, a schedule that became a zombie.

### (e) Anti-patterns — what we should NOT move into n8n
Validate, correct, and extend our list: the knowledge vault, GPU work, secrets, the owner's writing voice (quality-gated to specific models), IP-sensitive outbound posting, fleet watchdogs (circular dependency), and heavy compute inside the orchestrator process. Add the anti-patterns we have not thought of — especially "orchestrator creep" symptoms and the point at which an n8n workflow should have been a 40-line script.

### (f) Programmatic workflow deployment and versioning via the API
We already drive `GET/PUT /api/v1/workflows/{id}` from Python. We want the mature practice around it:
- Safe update protocol: what to strip, how to avoid the `settings` whitelist 400, how to guarantee an edit never silently changes `active` (failure 2), and how to verify an edit landed by reading back the fact rather than trusting the response.
- Diff/rollback strategies for workflow JSON (node IDs, positions, credential references, pinned data) — what makes n8n workflow JSON diff-hostile and how teams handle it.
- Promoting a change from dev to prod on the same or a second instance; how workflow/credential IDs behave across instances.
- How the built-in **source control** feature and API-driven edits interact (see (a)) — can they coexist, and what is the recommended single source of truth.
- Testing a workflow before activating it: pinned data, manual executions via API, and whether there is a real "dry run".

### (g) Community nodes for our stack
Telegram (including MTProto/user-account access vs Bot API), MongoDB, GitHub, and anything relevant to a Postgres+pgvector RAG. For each: maintenance status, last release, install/verification story on a self-hosted instance, security posture (community nodes run arbitrary code), and whether we are better off with a plain HTTP Request node.

---

## PART 4 — OUTPUT FORMAT

1. **Ranked use-case map** — a table of candidate adoptions, each with: what it is · which of OUR pains (Part 1/2) it removes · expected payoff · implementation cost (hours/days) · risk · required n8n version · verdict (adopt now / pilot / later / never for us).
2. **Top 5 in depth** — for each, an implementation sketch concrete enough to build from, the gotchas, the rollback, and how we would *measure* whether it worked (we require a named success metric and a harvest date before we turn anything on).
3. **A "do NOT do this" section** — explicitly including the things that look attractive but fail against Part 2.
4. **Version-and-source appendix** — every claim tied to a dated source; separate "documented and verified" from "community report" from "your inference".
5. **Open questions** — what you could not establish, and what we would have to test ourselves to settle it.

Constraints on your answer: prefer primary sources; give dates and version numbers; flag anything where n8n's behaviour changed between 2.14 and current; do not invent numbers, node names, env vars, or API fields — if you are not sure a field exists, say so. Where our stated conclusion (e.g. "REST PUT is safer than the MCP removeNode", "convert Python Code nodes to JavaScript") is wrong or outdated, say so plainly and show the evidence.
