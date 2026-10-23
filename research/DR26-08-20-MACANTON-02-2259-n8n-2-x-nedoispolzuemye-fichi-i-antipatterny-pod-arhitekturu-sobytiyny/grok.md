# DR26-08-20-MACANTON-02-2259 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

The output contract forbids files and progress chatter; the report still needs primary sources, so I am pulling official n8n docs, changelogs, and issue threads first, then writing the full report here.I have the primary-source map; next I am pulling the official docs and the GitHub threads that actually decide the recommendations.A. Ranked use-case map

Verdicts are for **your** instance: self-hosted **n8n 2.14.2** (released 2026-03-26), ~1k executions/day, REST-driven edits, n8n = ears/CRM/triggers not brain, secrets and vault stay on the hub, watchdog must live outside the thing it watches. Current upstream is **n8n 2.36.6** (2026-08-24). Do not treat current-docs fields as present on 2.14.2 until you read them back from *your* OpenAPI.1(https://docs.n8n.io/changelog/release-notes-2.x)

| # | Candidate | Pain it removes | Payoff | Cost | Risk | Needs version | Verdict |
|---|---|---|---|---|---|---|---|
| 1 | Route LLM through HTTP Request → your subscription sidecar; strip `lmChatOpenAi` metered nodes | 1.6 money fire; scar 3 alias trap; scar 4 retry-on-429 | Stops the OpenAI bill. Uses already-paid ~$540/mo quotas | 2–4 days sidecar rewrite + 1 day n8n rewire + 1 day sweep | Medium: ToS/concurrency; one hung CLI still hurts if sidecar stays single-thread | Any 2.x (HTTP Request is old) | **Adopt now** |
| 2 | Safe PUT protocol: never trust `active`; use activate/deactivate; `publishIfActive=false` if present; read-back | Scar 2 silent unpublish; scar 1 zombies; 1.3 API drift | Stops “edit took prod down for a day” | 1 day code + 1 day detector | Low if you always GET-after-PUT | `publishIfActive` is in **current** API docs; **unconfirmed on 2.14.2** | **Adopt now** (protocol); **pilot** the query param |
| 3 | Execution prune: raise count/age **and** `saveDataSuccessExecution=none` on the 1-min CRM poller; export errors out | Scar 7 forensics; 10k/10d wall is the documented default | History longer than 10 days without unbounded DB | Hours | Low. Wrong `none` on a workflow you debug often | 2.x env vars as documented | **Adopt now** |
| 4 | External dead-man’s-switch **outside** n8n + silent-failure jobs (last-fire age, active vs executions, webhook=0) | Scar 8 circular watchdog; scar 9 green-and-dead; GitHub webhook with 0 runs | You notice mute ears, not just crashes | 1–2 days | Low if it lives on hub/anchor, not in n8n | Any | **Adopt now** |
| 5 | Replace `$getWorkflowStaticData('global')` for the DMS with Data Table upsert **or** Postgres row | Scar 6 flush lag | Heartbeats that do not miss a write inside 3s | 0.5–1 day | Low for minute-scale; Data Tables cap 200 MiB | Data Tables + `/datatables` API: present by 2.14 era | **Adopt now** (Postgres if you already have it; Data Table if you want n8n-native) |
| 6 | WAF allowlist for Telegram + GitHub User-Agents on `/webhook/*`; synthetic probe with **that** UA | Scar 9; 1.1 WAF 403; GitHub PR webhook = 0 executions | Makes “wired” a fact, not a curl 200 | Hours | Low | Any | **Adopt now** |
| 7 | Convert remaining Python Code nodes to JavaScript; native Python only `_items`/`_item` | Scar 5 `_input` | Stops a class of daily red runs | Hours | Low | 2.0 removed Pyodide `_input` | **Adopt now** (keep your workaround; it is the documented rule) |
| 8 | Per-workflow save flags + error-workflow DLQ in Postgres + `POST /executions/{id}/retry` only for retryable errors | Scar 4; scar 7; 1.2 error catcher already exists | Replay without retrying quota deaths | 1–2 days | Medium if retry is naive | Retry endpoint is documented | **Pilot** |
| 9 | `N8N_METRICS=true` + scrape `/metrics` + `/healthz/readiness` from **outside** | Scar 8–9; “who watches n8n” | Process/DB signal, not workflow-green | Half day | Low. Do not expose `/metrics` to the internet | All self-hosted editions | **Adopt now** |
| 10 | Event-ify the 1-min Mongo poller (change stream / Telegram-first); keep n8n as ear not poller | 1.7 wasted ticks; 4155 runs/10d of mostly nothing | Cuts execution budget and token spend | 2–5 days | Medium (missed events) | Any | **Pilot** |
| 11 | Durable scheduler (misfire policy, grace) | Scar 1 zombies after downtime | Real catch-up semantics | Upgrade cost | Medium (new subsystem) | Docs dated 2026-07; **not** a 2.14.2 feature | **Later** (on upgrade) |
| 12 | Official instance MCP for **read/search only**; keep management tools off | You already chose blast-radius | Authoring MCP in 2.14 can create/update workflows | Hours to leave disabled | High if write tools on | Write tools from **2.13/2.14**; lots more after 2.15 | **Later / never for prod writes** |
| 13 | Queue mode + Redis workers | Not your volume problem | Almost nothing at ~1k exec/day; **worsens** staticData | Days of ops | High (new failure class; 2.x queue regressions exist) | 2.x; Postgres required | **Never for us now** |
| 14 | Built-in source control (Git) | 1.3 versioning | Fights your REST SoT; Enterprise/Business | Days + license | High conflict with Python PUT | Business/Enterprise | **Never** until you drop REST as SoT |
| 15 | External secrets | 1.4 secrets-local rule | Secrets would be reachable from the VPS | License + vault network | Violates “secrets stay on hub” | Enterprise only | **Never** under current rule |
| 16 | n8n Evaluations | Agent quality | Wrong tool for ops sanity; license-gated parallelism | Days | Low-medium | Metric evals: Cloud Pro/Enterprise; Community = 1 workflow | **Later / never for ops** |
| 17 | AI Agent nodes + Postgres Chat Memory tuning | Inner Agent 11% memory errors | Improves a layer you are trying to **shrink** | Days | High (flaky; burns tokens) | LangChain nodes already in 2.14 | **Never** as a growth path; freeze, don’t expand |
| 18 | Wait / resume-webhook HITL | Human approvals | Durable if wait >65s (offloaded to DB) | 1 day | Medium in queue mode later | Any 2.x; 2.0 changed parent/child wait return | **Pilot** only for true HITL |
| 19 | Community MTProto / extra Mongo/GitHub nodes | Telegram user-account | Arbitrary code in the n8n process | Hours | High (security + ToS) | Community packages | **Never**; HTTP Request + official nodes |
| 20 | Upgrade 2.14.2 → 2.36.x | Many fixes; MCP; durable scheduler; API fields | 5 months of upstream | 2–5 days soak | Medium (2.x has had queue/publish bugs) | — | **Pilot** on a clone, not prod this week |

---

B. Top 5 in depth

---

B1. Subscription quota rail (money)

What / version. n8n HTTP Request node (core, all 2.x) calling your hub sidecar. Do **not** point `lmChatOpenAi` at OpenAI. Current OpenAI **credentials** docs list API key + org, not a custom base URL. Current Chat Model docs list Timeout / Max Retries / Responses API tools, not a documented Base URL. Treat “OpenAI-compatible base URL on the official OpenAI credential” as **unverified on 2.14.2**. Use HTTP Request. Global AI-node timeout env is `N8N_AI_TIMEOUT_MAX` (default 3_600_000 ms) — only relevant if you keep AI nodes.2(https://docs.n8n.io/integrations/builtin/credentials/openai)

Sketch.

Sidecar (this is the real fix; n8n cannot save a single-thread `BaseHTTPServer`):

- Server: FastAPI/Starlette + uvicorn, or any async HTTP server. Not `http.server.BaseHTTPServer`.
- Concurrency: `asyncio.Semaphore(2)` or `3` to start. Each call spawns a CLI process; 2–3 is the honest limit on one machine. Queue extra requests; return **503 + Retry-After** when the semaphore is full so n8n does not pile in-process.
- Timeouts: per-request timeout = provider timeout + 30s (Claude 900s → 930s; Codex/Gemini 1800s → 1830s). Kill the subprocess on timeout. Never let `ask()` retry 3× with 2/4/8s **inside** a request that already has no deadline.
- Secret: read from a file (or fd) **per request**, not at import. Missing secret → process stays up, request 401s.
- Contract stay `POST /ask {provider, prompt, idempotency_key}` → `{text, provider, usage?}`. Still not an agent. Tools stay on the hub.
- Codex `--sandbox read-only`: do not send write jobs to that rail.

n8n side:

- HTTP Request, header auth = sidecar shared secret, custom User-Agent that the WAF allows (you already learned this).
- Node **Settings → Retry on Fail = off** for this node. 429/quota and “CLI busy” must not multiply.
- Timeout: set the node/workflow timeout **above** p95 of the CLI, below a hang (start 960s for Claude rail, 1860s for Codex). Also raise any reverse-proxy (Caddy/nginx) `proxy_read_timeout`; people keep hitting Traefik 10s/60s while n8n waits minutes.
- Sequential fallback, not fan-out race: HTTP Request Claude → IF error → HTTP Request Codex → IF error → Gemini. Fan-out doubles subscription burn for one answer.
- Circuit: a tiny Data Table/Postgres row `provider, failures, opened_at`. After 3 failures in 10 min, skip that provider for 15 min. Do this in the sidecar, not in 38 workflows.

Exposure of the hub to cloud n8n:

- Keep **zero inbound ports** on the home machine. Do not have n8n call the hub.
- Invert: n8n writes job `{id, prompt, provider, idempotency_key}` to Mongo/Postgres (you already have that bus). Hub **polls outbound** (existing architecture 2026-06-16). Sidecar stays local.
- If you insist n8n→hub: Tailscale/headscale to one listener bound to the tailnet IP, mTLS or a rotating header, still no public inbound. Cloudflare Tunnel is a public inbound in disguise; worse ToS/IP story for “IP-sensitive” work.

Do NOT do this if: the job needs the owner’s writing voice, vault writes, GPU, or browser cookies. Those stay local CLIs, not n8n.

Measure / harvest.

- Metric: `$` on the metered OpenAI account per 7 days; count of `lmChatOpenAi` nodes with any `gpt-*` / `o*` / `chatgpt*` string via REST GET all workflows = 0.
- Harvest: **2026-09-07**.
- Rollback: revert those workflows from your before/after snapshots; sidecar is additive.

Gotcha. Routing subscription CLIs through an automation server is a grey ToS area (shared accounts, automation, concurrency). Keep n8n off the prompt-composition path; it only ships a short instruction + payload id. Pay per token when you need many parallel <10s calls (CRM tool storm). That is the honest exception.

---

B2. Safe programmatic publish/activate (scars 1 and 2)

What / version. n8n 2.0 replaced Activate with **Publish**. Production runs the **published** version, not the latest save. Public API still exposes `active` as a **read-only** field on create/update. Current PUT docs: if the workflow is published, PUT **re-publishes** unless query `publishIfActive=false`. Activate/deactivate are separate operations (`POST /workflows/{id}/activate` and `/deactivate`, scoped `workflow:activate`). Your 2.14.2 may not have `publishIfActive` — probe it.3(https://docs.n8n.io/connect/n8n-api/workflow)

This is the actual mechanism behind both scars, better than “MCP is cursed”:

- **Scar 2:** `active` in a PUT body is ignored. A management tool that rewrites `nodes` on a published workflow either (a) republishes the new graph immediately, or (b) fails validation and leaves you unpublished. Community `n8n-mcp` additionally **refuses disconnected nodes** and will delete them to make the document “valid”. n8n core still accepts orphans. That mismatch is real. **REST PUT is safer than community-MCP `removeNode` for orphan-heavy graphs.** Official instance MCP `update_workflow` (shipped around 2.14.0) is the same class of risk as MCP: it mutates. It is not safer.
- **Scar 1 (zombies):** `active=false` in DB/API does **not** always unregister an in-memory cron. Community reports of inactive workflows still firing go back years; there is **no single GitHub issue + fix version** I can honestly pin as “fixed in 2.x”. 2.0 publish/unpublish made this worse to reason about (`active` vs `activeVersionId`). PUT cannot deactivate. You must call **deactivate/unpublish**, then **prove** the schedule is gone.

Sketch (Python, around your existing client).

```
1. GET /api/v1/workflows/{id}
2. Snapshot to disk (you already do).
3. Strip read-only: id, active, activeVersionId, activeVersion, createdAt, updatedAt,
   isArchived, versionId, triggerCount, meta, tags, shared, staticData? (see below).
4. settings: allowlist FROM YOUR INSTANCE, not from memory.
   Probe: PUT with one extra key; on 400 "must NOT have additional properties", strip.
   Do not send binaryMode. 2.0 removed in-memory binary mode; your 400 on
   binaryMode:"separate" is consistent with a strict schema even if later OpenAPI
   examples still show the key (docs drift — treat as unverified).
5. PUT /api/v1/workflows/{id}?publishIfActive=false
   If 400 on unknown query: PUT without it.
6. GET again. Assert:
   - node ids you did not intend to drop are present
   - settings.errorWorkflow unchanged
   - active == previous active
   - activeVersionId == previous activeVersionId  (this is the publish bit)
7. If you meant to change live behavior: POST .../activate (or deactivate)
   then GET until active/activeVersionId matches intent.
8. Never take “PUT 200” as success.
```

Leave `staticData` out of PUT unless you are deliberately migrating it. Overwriting it from a stale GET is how heartbeats reset.

Zombie detector (deterministic, API-only):

```
For each workflow in GET /workflows (paginate; limit max 250):
  published = (active is True) or (activeVersionId is not None)
  GET /executions?workflowId={id}&limit=50
  latest_trigger = max(executions where mode in {trigger, webhook} and startedAt)
If not published:
  if latest_trigger.startedAt > workflow.updatedAt + 2 minutes:
    ALERT zombie (inactive but still firing)
If published and has a Schedule Trigger:
  if now - latest_trigger > 2 * expected_interval:
    ALERT mute schedule (opposite of zombie)
If published and has a Webhook Trigger:
  if execution count == 0 and updatedAt older than 24h:
    ALERT webhook never wired  (your GitHub PR workflow)
```

`triggerCount` on the workflow object is “number of active trigger nodes” — useful extra signal, not proof by itself.

Do NOT do this if: you turn on official MCP write tools “just to edit faster”. Same scar 2.

Measure / harvest.

- Metric: 0 executions with `mode=trigger` on workflows where `active=false` over 7 days; 0 unexpected `active true→false` without a matching deactivate in your audit log.
- Harvest: **2026-09-07**.
- Rollback: your before/after JSON + `POST /activate`.

Gotcha. `N8N_WORKFLOW_AUTODEACTIVATION_ENABLED` (default false) unpublished after N crashed executions (default 3). If someone enabled this, that is another silent-off path. Read the env; do not assume default.4(https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/executions)

---

B3. Execution retention and a real forensic trail

What / version. Documented 2.x behaviour: prune when **either** age **or** count trips. Defaults: `EXECUTIONS_DATA_PRUNE=true`, `EXECUTIONS_DATA_MAX_AGE=336` (14 days), `EXECUTIONS_DATA_PRUNE_MAX_COUNT=10000`. Finished executions only; `new`/`running`/`waiting` skipped; annotated executions never pruned; hard-delete buffer default 1h. Your “~10k ≈ 10 days” is the **count default**, not a mysterious bug. The 1-min CRM poller alone is ~4130/10d, ~40% of the cap.5(https://docs.n8n.io/deploy/host-n8n/configure-n8n/scaling/manage-execution-data)

Sketch.

Env (instance):

```
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=720          # 30 days, if disk allows
EXECUTIONS_DATA_PRUNE_MAX_COUNT=0    # 0 = no count cap (docs)
# or 50000 if you want a cap
EXECUTIONS_DATA_SAVE_ON_SUCCESS=all  # instance default
EXECUTIONS_DATA_SAVE_ON_ERROR=all
EXECUTIONS_DATA_SAVE_ON_PROGRESS=false
```

Per-workflow `settings` (this is the actual lever):

- Main Agent 1-min poller: `saveDataSuccessExecution: "none"`, `saveDataErrorExecution: "all"`, `saveExecutionProgress: false`.
- Translation bot 288/day: same if you do not debug it.
- Keep full success data on low-volume, high-stakes (DMS, error catcher, GitHub webhook).

Error path (you already have a global Error Trigger): write a row to **Postgres** (not n8n execution log): `ts, workflow_id, execution_id, node, message, fingerprint, raw_json`. That is your >30 day forensic store. n8n’s log will still prune.

Replay: `POST /api/v1/executions/{id}/retry`. Official docs exist. Community (2026-06) uses body `{"loadWorkflowExecution": true}` to retry with the **saved** workflow vs current. **Confirm the body against your 2.14.2 OpenAPI.** Only retry after classifying the error (scar 4): no retry on `insufficient_quota` / 429 billing / WAF 403 / invalid payload.

Do NOT do this if: you set `PRUNE_MAX_COUNT=0` and `SAVE_ON_SUCCESS=all` on the 1-min poller. DB will grow without bound. Raise retention **and** stop saving successes on hot pollers, together.

Measure / harvest.

- Metric: `SELECT count(*) FROM execution_entity` (or API) after 14 days > 10k **and** you can still `GET` an error execution from day −12; error-export table has rows older than n8n’s prune.
- Harvest: **2026-09-07**.
- Rollback: revert env, restart.

Gotcha. Waiting executions are not pruned — a leaked Wait node is a disk leak. Binary data follows `N8N_DEFAULT_BINARY_DATA_MODE` (2.0: no in-memory default; filesystem in regular mode, database in queue mode).

---

B4. Who watches n8n (must live outside n8n)

What / version. `/healthz` always on main (liveness, **not** DB). `/healthz/readiness` = DB connected + migrated. `/metrics` off by default; `N8N_METRICS=true`; self-hosted all editions; **not** on Cloud. Queue metrics need `N8N_METRICS_INCLUDE_QUEUE_METRICS=true` (only useful if you ever run queue mode). Do not publish `/metrics` to the internet. Workers: health off until `QUEUE_HEALTH_CHECK_ACTIVE`. There is no native “this schedule did not fire” metric. Prometheus will not detect zombies, unpublished workflows, expired credentials, or WAF 403. Those are **application** checks.6(https://docs.n8n.io/deploy/host-n8n/keep-n8n-running/monitor-n8n)

Sketch. Run this on the **hub or Linux anchor**, as a Task Scheduler / systemd timer, 5 min. Not an n8n workflow.

```
1) GET https://n8n/healthz/readiness  with a browser-like UA
   fail → page (Telegram 02/03) “n8n not ready”
2) GET /api/v1/workflows  +  /api/v1/executions?status=error&limit=20
3) Silent checks from B2 (zombie, mute schedule, webhook 0)
4) Heartbeat file/row THIS SCRIPT writes; a *second* checker
   (different machine) alerts if the checker itself is late
5) Daily green receipt at 07:00 Lisbon: “n8n ready, 71 published,
   0 zombies, 0 mute schedules, last error age=…”
   Absence of this message is the alarm (you already distrust red).
```

Error-workflow at scale: you already built dedup ≤10 min and ≤5 alerts/min. **No native replacement** in 2.14.2. Keep yours. Add: if error-workflow itself has not run **and** `GET /executions?status=error` is non-empty, the external checker pages (error catcher died). Distinguish instance-wide vs one workflow: >N distinct workflow_ids in errors in 5 min → instance incident; else per-workflow.

WAF probe: POST your dead-man webhook with **urllib’s UA** and with curl’s UA. Expect both 2xx. Today you only learned the second.

Credential expiry: n8n does not expose a first-class “credential expired” metric. Practical: a canary workflow per credential class (Telegram getMe, Mongo ping, OpenAI models.list if you still have a key) on a 6h schedule, save errors only. External checker alerts if canary missing.

Do NOT do this if: the checker is another n8n workflow on the same instance. That violates your rule 8.

Measure / harvest.

- Metric: one green receipt/day in TG-03; time-to-detect a forced mute (unpublish a dummy schedule) < 10 min; time-to-detect urllib-403 < 10 min.
- Harvest: **2026-09-03** (forced-fail drill).
- Rollback: disable the timer; n8n unchanged.

---

B5. Durable state for the existing DMS (scar 6)

What / version. Official position in product docs: Data Tables are for “markers to prevent duplicate runs”, lookups, eval data. Default instance cap **200 MiB**, override `N8N_DATA_TABLES_MAX_SIZE_BYTES`. Access: Data Table node, UI tab, `/datatables` API. **No access from Code node.** Community on 2.5.2 (2026-01): `$getWorkflowStaticData` not persisting between production executions; some users saw it “work again” on 2.7.x without a changelog line — treat as **unreliable**, not as a bug you wait to be fixed. n8n staff-shaped replies (and the docs’ own framing) say it is not a persistence layer. Queue mode makes this worse (workers, races). You should **not** adopt queue mode to fix it.7(https://docs.n8n.io/build/work-with-data/data-tables)

Sketch. Prefer **Postgres** you already run for RAG. Data Tables are fine if you want zero new infra.

Table `n8n_heartbeats(source PRIMARY KEY, every_min INT, last_seen TIMESTAMPTZ, extra JSONB)`.

Webhook workflow:

- Postgres node **upsert** on `source`.
- Do not read-after-write in the same execution to “confirm”; the upsert is the confirm.

Schedule every 5 min:

- `SELECT source FROM n8n_heartbeats WHERE last_seen < now() - (every_min * 2) * interval '1 minute'`.
- One alert path (you already have storm gating).

If Data Table instead: columns `source` (string, unique), `every_min` (number), `last_seen` (string ISO). Operation **Upsert** on `source`. Same 5-min reader.

Do NOT do this if: you need sub-second locking or huge history — then it is a 40-line script + Postgres, not n8n. Do not put **fleet** watchdogs here (hub Task Scheduler, launchd, Syncthing). Only producer heartbeats that n8n is the ear for.

Measure / harvest.

- Metric: inject a heartbeat, fire a reader 1s later, `last_seen` matches; 0 missed-due alerts that were false (writer had actually posted).
- Harvest: **2026-09-03**.
- Rollback: staticData path still in git snapshots.

Gotcha. Data Tables are **inside** n8n. If n8n is down, the table is down. That is acceptable for “did producer X beep into n8n?” and unacceptable for “is n8n alive?” — B4 covers that.

---

C. Do NOT do this

1. **Queue mode at ~1k exec/day.** Official value of queue mode is scaling workers. You are not CPU/throughput bound; you are bound by a 1-min poller, a single-thread sidecar, and silent triggers. Queue mode adds Redis, extra processes, webhook extra hop, binary-storage constraints (no filesystem binary in queue mode), staticData races, and 2.x reports of stalled-job behaviour getting **worse** after `QUEUE_WORKER_MAX_STALLED_COUNT` was removed in 2.0. Do not use workers as a substitute for a threaded sidecar.8(https://community.n8n.io/t/after-upgrading-n8n-self-hosted-to-v2-x-queue-mode-reliability-much-worse/247235)

2. **n8n as brain / voice / vault / GPU.** Your 2026-08-20 verdict stands. AI Agent expansion, Evaluations-as-prod-gate, Postgres Chat Memory “tuning” as a project — all grow the flakiest 11% layer and spend the metered budget.

3. **Native Python Code nodes using `_input`.** Not a mysterious sandbox bug. **Documented 2.0 break:** Pyodide removed; native Python injects only `_items` (all-items) and `_item` (each-item); no other builtins; bracket access only. A second workflow “with the same node type/version” that still runs `_input.first().json` is almost certainly a different `parameters.language` / leftover typeVersion / actually JS. Rule: JavaScript for n8n builtins (`$getWorkflowStaticData`, `$input`, `$execution`); Python native only for pure transforms on `_items`. Your “convert to JS” workaround is correct, not a hack.9(https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.code)

4. **Retries on non-retryable errors.** Still true. Node-level Retry on Fail and Chat Model “Max Retries” will hammer `insufficient_quota`. Classify first.

5. **Bare model aliases in any remaining n8n LLM node.** Sweep **all** model strings, not just `gpt-5`.

6. **Community MCP write path, and official instance MCP write tools, on prod.** Official 2.14 can create/update workflows. That is the blast radius you disabled community MCP for. Read-only docs MCP: keep. Instance MCP: leave off, or enable with **no** workflow `Available in MCP` on CRM/inbound.

7. **External Secrets / Source Control.** Enterprise/Business. Source control versions workflows + credential **stubs** + variables; not secrets, not execution data, not per-instance IDs as you’d hope. Pull/push will fight REST PUT (two SoTs). External secrets also require the VPS to reach a vault — opposite of “secrets stay on the hub”.

8. **n8n-hosted fleet watchdog.** Circular. Your DMS watching **producers** via webhook is fine; watching n8n from n8n is not.

9. **MTProto community nodes, random Mongo/GitHub community nodes.** Community packages execute inside the n8n process. Official Telegram node is Bot API webhook, not MTProto. User-account Telegram is a ban/ToS class you do not need; you already have Telethon elsewhere.

10. **Inbound ports on the hub “just for n8n HTTP Request to the sidecar”.** Violates the 2026-06-16 split. Invert the queue.

11. **Wait node for anything <65s as if it were durable.** Docs: waits <65s stay in-process and die with the process. ≥65s / webhook / form offload to DB and survive restart. Resume URL is **per execution** and changes on partial runs. Wait node uses **server time**, not workflow timezone. HITL parent/child return values **changed in 2.0** (parent now gets child output, not child input).

12. **Trusting curl against `/webhook/*`.** Your WAF lesson. GitHub’s hook UA and Telegram’s UA are not curl. The zero-execution GitHub workflow is this class until proven otherwise.

13. **Orchestrator creep.** If a workflow is: schedule → HTTP → if → HTTP, it is a 40-line Python on the hub with launchd/Task Scheduler. n8n earns its keep for Telegram/CRM fan-in, webhooks, and “wake the hub”. When you add a 12th Code node, you wanted a script.

---

D. Version-and-source appendix

Documented (official docs / changelog / API pages, dates as fetched 2026-08-24)

| Claim | Version / date | Source |
|---|---|---|
| n8n 2.14.2 released 2026-03-26; 2.14.0 on 2026-03-24 | 2.14.x | docs.n8n.io/changelog/release-notes-2.x |
| Current GitHub tag [email hidden] on 2026-08-24 | 2.36.6 | github.com/n8n-io/n8n/releases |
| 2.0: task runners default; Pyodide Python removed; native Python only `_items`/`_item`; no `_input` | 2.0 (GA late 2025 / 2.0 line) | docs.n8n.io/changelog/v20-breaking-changes ; Code node docs |
| 2.0: Activate → Publish; save ≠ live | 2.0 | same + save-and-publish-workflows |
| 2.0: remove in-memory binary mode; `QUEUE_WORKER_MAX_STALLED_COUNT` removed (no auto-retry stalled jobs) | 2.0 | v20-breaking-changes |
| 2.0: parent waiting on child Wait/HITL now receives child **output** | 2.0 | v20-breaking-changes |
| Prune defaults 10_000 / 336h / prune on either | 2.x | manage-execution-data ; executions env vars (page “last updated 10 days ago”) |
| `N8N_WORKFLOW_AUTODEACTIVATION_*` | 2.x env | executions env vars |
| `N8N_CONCURRENCY_PRODUCTION_LIMIT` default -1 | 2.x | same |
| `N8N_AI_TIMEOUT_MAX` default 3600000 ms | 2.x | same |
| Data Tables: 200 MiB, `/datatables`, no Code-node access | current docs, updated ~13 days ago | data-tables |
| Source control: Business + Enterprise; workflows + tags + credential/variable stubs | current | use-source-control-and-environments |
| External secrets: Enterprise; 1Password Connect, AWS, Azure, GCP, Vault, Infisical (Infisical from 2.26.0); multi-vault from 2.10.0 | Enterprise | use-external-secret-stores |
| Instance MCP: two-level opt-in; search + execute + (from 2.13.0) create/edit workflows + data tables | 1.120 search, 1.121 execute, 2.13 authoring, 2.14 create/update widely noted | connect-to-n8n-mcp-server ; Reddit report on 2.14.2 |
| MCP `test_workflow` / pin-data helpers | **2.15.0+** (you do not have them) | mcp-server-tools-reference |
| PUT workflow: `active` read-only; republish unless `publishIfActive=false` | **current** API page | connect/n8n-api/workflow |
| `POST /api/v1/executions/{id}/retry` | current API | connect/n8n-api/execution |
| Error Trigger payload: execution.id/url/error/lastNodeExecuted/mode; workflow id/name; retryOf optional; trigger-node errors differ | current | errortrigger node docs |
| Wait <65s in-process; longer offloaded; timezone = server; `$execution.resumeUrl` | current | Wait node docs |
| `/healthz`, `/healthz/readiness`, `/metrics` + `N8N_METRICS` | current | monitor-n8n ; enable-prometheus-metrics |
| Queue mode: main handles timers/webhooks, workers execute; Redis; not SQLite | current | enable-queue-mode |
| Durable scheduler misfire policies | docs 2026-07-16; per-node grace from **2.36.0** | durable-scheduler |
| Evaluations metric mode: Pro/Enterprise; Community 1 workflow; parallel 1/3/5 by license | current | use-metrics-to-measure-quality |
| OpenAI Chat Model: Timeout, Max Retries; Responses API tools | current | lmchatopenai |

Community / GitHub (not gospel)

| Claim | When | Source | Confidence |
|---|---|---|---|
| Inactive workflow still executing | 2025-07-31 | community.n8n.io/t/159519 | Recurring class; no single fix version |
| `$getWorkflowStaticData` not persisting in prod on 2.5.2; “works on 2.7” | 2026-01 | community.n8n.io/t/255097 | Unreliable API; do not depend |
| Native Python missing builtins `_xxx` | 2026-01-02, n8n 2.1.4 | community.n8n.io/t/244368 | Matches official docs |
| Python runner cannot see previous-node input | 2025-12, issues 23530 / 23587 | GitHub | Same class as scar 5 |
| Telegram Trigger has no native polling; webhook-only; polling = DIY getUpdates | 2025–2026 threads | community.n8n.io/t/289235, t/121004 | Established for the official node |
| Queue mode reliability worse after 2.0 stalled-job change | 2026-01-08 | community.n8n.io/t/247235 | Plausible; don’t copy queue mode |
| Official MCP on 2.14.2: `create_workflow_from_code`, `update_workflow` | 2026-03 | Reddit r/n8n 1s6aytd | Consistent with docs 2.13–2.14 |
| Retry body `{loadWorkflowExecution:true}` | 2026-06 | community.n8n.io/t/300785 | Verify on your OpenAPI |
| HTTP Request Retry on Fail is a node Setting | current | httprequest common-issues | Documented |

Inference (ours, labeled)

- Your 10k/10d wall **is** `EXECUTIONS_DATA_PRUNE_MAX_COUNT=10000`.
- Scar 1 root in **codebase**: in-memory ActiveWorkflows/cron vs DB `active`/`activeVersionId` divergence; PUT cannot deactivate because `active` is read-only. Not proven as one patched bug.
- Scar 2: community-MCP extra validation + possible republish-on-PUT. REST PUT without deactivate is safer for orphans. **Confirmed as a policy, not as a single upstream bug ID.**
- Scar 5: `_input` is Pyodide-only. Inconsistent “same node” almost certainly isn’t the same runtime parameters.
- GitHub PR webhook 0 executions: not connected **or** WAF 403 on GitHub’s UA **or** unpublished. Absence cannot arrive as an event — you must poll (B2/B4).
- OpenAI-compatible Base URL on official OpenAI credentials: **not documented** on the credentials page we fetched. Do not design as if it exists.

Telegram trigger delivery (for (b), condensed)

- Official Telegram Trigger = Bot API **webhook** to n8n’s `/webhook/...` when published. Not MTProto. Not polling.
- Restart/deploy: n8n re-registers the webhook on activate. During the gap Telegram retries failed POSTs for a limited time (Telegram-side; not an n8n guarantee). Use `update_id` as idempotency key.
- Ordering: not guaranteed under retries. Duplicates happen if n8n returned 4xx/5xx (your WAF 403 is a silent drop — Telegram may retry or give up; n8n shows **zero** executions).
- Missed window: not recoverable from n8n; `getUpdates` offset catch-up is a **separate** workflow you would have to build.
- Your production pattern `Telegram → filter → HTTP → hub wake-listener` is the right split. Keep it. Put idempotency_key on the hub POST.

Wake-behind-NAT

- Keep outbound-poll queue (local polls cloud). Tailnet n8n→hub is optional for low-latency wake only; still bind the listener to the tailnet, not 0.0.0.0:public.
- Asleep laptop: queue depth with a max, poison after N failures, **no catch-up storm** (drop or sample, don’t replay 10k). Employee lid-close machines must not be required in the middle of a pipe.

Schedule vs event

- Default timezone `GENERIC_TIMEZONE`, default America/New_York if unset; per-workflow `settings.timezone`. DST follows the IANA zone.
- Wait node ignores workflow timezone (server time) — different from Schedule Trigger. Do not mix them blindly.
- In-memory scheduler (2.14.2): missed ticks during downtime are **skipped**, not caught up (durable-scheduler docs say the new scheduler’s skip policy “matches the in-memory scheduler”). Durable scheduler (later 2.x) can catch up; you do not have it yet.

Sub-workflows

- `executeWorkflow` + `executeWorkflowTrigger`: data = items in, items out. 2.0 wait/HITL return change. Child errors: parent fails unless you set child `onError` / parent continue. Sub-executions are separate rows **if** saved; people expect them inline — they are not. LangChain `toolWorkflow` always last-run-only output even after Execute Workflow Trigger v1.2 merge behaviour (release notes PR 30716).
- Do not add more toolWorkflows to burn tokens in n8n.

Community nodes (g)

| Need | Use | Avoid |
|---|---|---|
| Telegram bots | Official Telegram node + Trigger (Bot API) | MTProto/userbot community packages (ToS, session theft, arbitrary code) |
| Mongo CRM | Official MongoDB node you already run | `n8n-nodes-mongodb-ex` unless you have a named gap and are willing to run foreign JS in-process |
| GitHub | Official GitHub node + **connect the webhook** + allowlist GitHub UA | Community GitHub nodes |
| pgvector RAG | Official Postgres node, raw SQL; you already have a RAG backend — HTTP to **that** | New vector community nodes |

Install/verify community nodes only if you ever violate the above: `N8N_COMMUNITY_PACKAGES_ENABLED`, npm provenance required for new publishes (n8n-nodes-starter, May 2026). Verification = hash-pin the package.

HTTP Request vs dedicated node: if the official node is a thin REST wrapper, HTTP Request is fewer surprises (you already live in REST). Use dedicated nodes for Trigger/webhook registration (Telegram, GitHub) where the node owns subscription lifecycle.

---

E. Open questions (you must test; I will not invent)

1. Does **2.14.2** PUT accept `?publishIfActive=false`? Current docs have it; 2.14 may not. One PUT on a clone tells you.
2. Exact `settings` additionalProperties list **on your binary**. Current OpenAPI examples include `binaryMode`, `availableInMCP`, `timeSavedPerExecution`, `redactionPolicy` — your 400 on `binaryMode` means **your** schema ≠ today’s example. `GET` `/api/v1/docs` (or the swagger JSON your instance serves) is the only truth.
3. Body schema of `POST /executions/{id}/retry` on 2.14.2 (`loadWorkflow` vs `loadWorkflowExecution` vs empty).
4. Zombie root in **your** process model: one Node process or more? `EXECUTIONS_MODE`? Any accidental second container on the same DB? Two mains = classic double-fire. I could not name a closed GitHub issue that fixes scar 1 for all 2.x users.
5. Why one Python node accepted `_input` and the twin did not: dump both nodes’ JSON (`typeVersion`, `parameters.language`, `pythonCode` vs `jsCode`, `mode`). I predict they differ. If they do not, that is a runner bug worth a minimal repro to n8n — still not a reason to keep Python.
6. OpenAI Chat Model / credentials **Base URL** field on 2.14.2 UI. Docs do not list it. Look at the node in the editor; if absent, HTTP Request is the path (already recommended).
7. Streaming/SSE: HTTP Request in 2.14 is not a robust SSE client for 15-minute CLI streams. Assume **non-streaming**, one JSON body at the end. Do not design chat UX through n8n.
8. Data Tables API completeness on 2.14.2 vs later (`add_data_table_rows` MCP tools are 2.16+). REST `/datatables` should still work; confirm CRUD on your instance.
9. Telegram during n8n restart: capture `getWebhookInfo` pending_update_count before/after a bounce. I will not guess Telegram’s retry budget.
10. Whether `N8N_WORKFLOW_AUTODEACTIVATION_ENABLED` is on. Read env.
11. Postgres Chat Memory 11%: I did not find a single root cause that matches “11%”. Known failure modes: session id undefined, credential scoping on sub-nodes (GitHub 17603, 2025-07), type casts. Worth a **log sample of 20 errors**, not a rewrite.
12. Upgrade 2.14.2 → 2.36.x: read 2.x breaking notes beyond 2.0 (durable scheduler, MCP UI 2.33, webhook response offload 2.34). Soak on a clone with your 89-workflow snapshot replayed.

If you only do five things this week: sidecar concurrency, PUT/activate protocol + zombie detector, prune+stop-saving the 1-min poller, external n8n DMS + WAF-UA probes, move heartbeats off staticData. Everything else is either a license you do not have, a scale problem you do not have, or a brain you already decided n8n is not.
