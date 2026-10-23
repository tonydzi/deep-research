# DR26-08-20-MACANTON-02-2259 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-20-MACANTON-02-2259
vendor: Z.ai GLM (chat.z.ai)
door: browser-pane-anon
rail: glm

collected: 2026-08-28T13:12:26Z
took_sec: 0
bytes: 80888
note: >-
  Анонимная веб-сессия GLM с веб-поиском и deep-think. Это НЕ вендорский режим Deep Research — уровень «умная модель + поиск».
  Забор через локальный мост dr_glm_relay.py (0 токенов, тело не проходит через контекст).
---

Thought Process
DEEP RESEARCH REPORT — n8n Under-Used Capabilities & High-Value Patterns

Knowledge boundary declaration: My training data extends to early April 2025. Your instance runs v2.14.2 with dates in 2026. I cannot verify behavior, bug fixes, or feature states at your exact version. Where I make claims about n8n internals, I flag the confidence level. Where I cannot verify something at your version, I say so explicitly rather than extrapolating. This report is structured so you can identify every claim's evidentiary basis and know what you must test yourself.

1. RANKED USE-CASE MAP
#
	
WHAT
	
OUR PAIN ADDRESSED
	
EXPECTED PAYOFF
	
IMPL. COST
	
RISK
	
MIN N8N VERSION
	
VERDICT

1	Execution retention tuning via EXECUTIONS_DATA_* env vars	Pain #7 (log retention destroys forensics)	Restores 30–90 day forensic window for near-zero effort	1–2 hours	Very low	0.x (long-standing)	Adopt now
2	Dead-letter queue pattern on Error Trigger	Pain #4 (retries on non-retryable errors), general error handling	Stop burning money on 429 retries; structured retry/fallback	4–8 hours	Low	Error Trigger node has existed since ~0.166; payload shape may have evolved	Adopt now
3	Retry-from-failure via API (POST /executions/{id}/retry)	Pain #4, general ops	Manual replay of failed executions without re-running entire pipeline	2–4 hours (build into error workflow)	Low	Existed pre-1.0; exact API shape at 2.14.2 unverified	Adopt now
4	Zombie schedule detection script (API-based)	Pain #1 (zombie schedules — single largest hidden cost)	Detect zombies deterministically; prevent recurrence	4–6 hours	Low (read-only)	Any version with REST API	Adopt now
5	Absence-of-event monitoring (trigger heartbeat)	Pain #9 (silent degradation), webhook-with-zero-executions	Detect triggers that stopped firing, workflows deactivated by edit	6–10 hours	Medium (must live outside n8n per rule #8)	Any version	Adopt now
6	Threaded/async sidecar replacement	Pain §1.6 (single-threaded sidecar blocks for 15+ min)	Eliminate head-of-line blocking; unblock subscription-CLI usage	2–3 days	Medium	N/A (external to n8n)	Adopt now
7	Queue mode evaluation	Pain #6 ($getWorkflowStaticData flush lag), execution isolation	May resolve static data consistency; enables worker scaling	1–2 days (eval) + ongoing ops cost	Medium-high (operational complexity, may NOT fix static data)	0.x (Redis-backed queue mode is long-standing)	Pilot
8	Data Tables for cross-execution state	Pain #6 (static data flush lag)	Proper DB-backed state instead of in-memory static data	3–5 days	Medium (feature was very new at my cutoff; maturity unknown at 2.14.2)	~1.40+ (estimate; feature was rolling out in early 2025)	Pilot — depends on maturity at your version
9	Source control / git-backed deployment	§1.3 (API-driven edits), §(f) (versioning practice)	Proper diff history, rollback, environment promotion	3–5 days eval + migration	High (may conflict with REST API edits — unverified coexistence)	Pro/Enterprise tier (exact tier naming may have changed)	Pilot — blocked on coexistence question
10	External secrets	§1.5 rule (secrets stay local), DB credential exposure	Remove secrets from n8n DB; resolve at execution time	2–3 days	Medium (license gate; adds external dependency)	Advanced/Enterprise tier	Later — your secrets are already local by architecture
11	n8n AI nodes with custom base URL	§1.6 (use subscription quotas not metered API)	Use built-in agent/chat nodes with your sidecar as backend	2–4 hours per node type	Medium (hardcoded provider behaviors, streaming issues)	1.x+ for custom base URL support	Pilot — test each node type individually
12	Instance-level MCP server	§1.3 (we have not piloted it)	Authoring/data access via MCP protocol	3–5 days eval	Medium-high (security surface, capability drift)	~1.50+ (was GA-ing in early 2025)	Later — docs drifted, security assessment incomplete
13	Evaluations feature	General quality monitoring	Automated sanity checks on AI outputs	Unknown	Unknown (feature was nascent at my cutoff)	Unknown (post-1.50 estimate)	Later — insufficient data
14	Community nodes (Telegram MTProto, MongoDB, GitHub)	§(g)	Richer node functionality vs HTTP Request	1–2 days each	High (arbitrary code execution, maintenance uncertainty)	Any	Never for us — see §3 analysis
2. TOP 5 IN DEPTH
TOP 1: Execution Retention Tuning via EXECUTIONS_DATA_* Environment Variables

What it is: n8n has a set of environment variables that control execution pruning behavior. These have existed for many versions and are well-documented.

Implementation sketch:

bash
# In your n8n .env or docker-compose:
EXECUTIONS_DATA_PRUNE=true                          # Enable pruning (likely already true)
EXECUTIONS_DATA_MAX_AGE=2160                        # 90 days in hours (90 * 24)
EXECUTIONS_DATA_MAX_COUNT=50000                     # Absolute ceiling

The pruning logic: n8n runs a prune job periodically. It deletes executions older than MAX_AGE and trims to MAX_COUNT if exceeded, whichever is more restrictive. Setting MAX_AGE=2160 with your ~1000 executions/day means ~90k executions max — well within MAX_COUNT=50000 would cap it at 50 days instead. Set MAX_COUNT high enough that MAX_AGE is the binding constraint.

What you MUST also set to keep the data useful:

bash
EXECUTIONS_DATA_SAVE_ON_ERROR=all                   # Keep error execution data (default is often 'all' but verify)
EXECUTIONS_DATA_SAVE_ON_SUCCESS=all                 # Keep success execution data — REQUIRED for forensics
EXECUTIONS_DATA_SAVE_MANUAL_EXECUTIONS=true         # Keep manual test runs

If SAVE_ON_SUCCESS is set to a minimal mode, successful executions are pruned even within the retention window. This is the setting most likely to silently destroy your forensic data.

Gotchas and failure modes:

DB growth: 90 days × 1000 exec/day × average execution size = you must model your DB growth. Postgres-backed, this is predictable; check pg_total_relation_size('execution_entity') after a week and extrapolate.
Prune job timing: The prune job runs on a schedule (not continuously). If your instance restarts frequently, the prune may not run as expected. Monitor actual retention via SELECT MIN("startedAt") FROM execution_entity.
MAX_AGE interacts with MAX_COUNT: If you set MAX_AGE=2160 but MAX_COUNT=10000, and you hit 10k executions in 10 days, the count cap wins and you still lose forensics. Set MAX_COUNT ≥ MAX_AGE_hours × avg_executions_per_hour.
Version uncertainty: At my cutoff, these env vars were stable. I cannot verify they haven't been renamed or deprecated by 2.14.2. Check your instance's startup logs for any deprecation warnings.

Rollback: Revert env vars and restart. Already-pruned data is gone (unless you have DB backups).

Success metric: "I can query the API for error executions from 60 days ago and get results." Harvest date: 7 days after deployment (by then you'll have 7 days of retained history beyond the old 10-day window — trivially verifiable).

Source classification: Documented and verified (pre-cutoff). Env var names are from official n8n docs on execution settings. Behavior is well-established.

TOP 2: Dead-Letter Queue Pattern on Error Trigger

What it is: Your current error workflow catches everything and applies rate-limiting/dedup. The missing piece is routing: some errors are retryable (transient network failures), some are not (429 insufficient_quota, invalid credential), and some need human attention. A DLQ pattern classifies and routes accordingly.

Implementation sketch:

Your Error Trigger workflow receives a payload. Based on my pre-cutoff knowledge, the Error Trigger node output includes:

execution.id
execution.workflow.id
execution.workflow.name
execution.error.message
execution.error.stack (or similar — verify field name at your version)
The full execution data (if includeData is configured)

Build this flow inside your existing error workflow, before the dedup/rate-limit stage:

text
Error Trigger
  → Code node: classify error
    → Switch node on classification:
      → "retryable" (5xx, timeout, ECONNREFUSED): 
           → Check retry count < 3
             → yes: POST /api/v1/executions/{id}/retry (see Top 3)
             → no: route to DLQ storage
      → "non-retryable" (429, 401, 403, validation):
           → Log + suppress (do NOT retry)
           → If 429: tag as "quota_exhausted" for dashboarding
      → "unknown":
           → Route to DLQ storage + alert

Classification logic (Code node):

javascript
const errorMsg = $input.first().json.execution.error.message || '';
const is429 = errorMsg.includes('429') || errorMsg.includes('insufficient_quota');
const isAuth = errorMsg.includes('401') || errorMsg.includes('403') || errorMsg.includes('credential');
const is5xx = /5\d\d/.test(errorMsg);
const isTimeout = errorMsg.includes('timeout') || errorMsg.includes('ETIMEDOUT') || errorMsg.includes('ECONNREFUSED');


if (is429 || isAuth) return 'non_retryable';
if (is5xx || isTimeout) return 'retryable';
return 'unknown';

DLQ storage options (ranked for your setup):

Postgres table (dlq_entries) — you already have Postgres. Write a row with execution ID, workflow ID, error classification, timestamp, processed flag. Query it for retry/backlog visibility.
A dedicated n8n workflow that receives DLQ items via webhook and stores them — but this adds n8n-as-dependency to your error path. Less ideal.
Data Tables — if they're mature enough at your version (see Top 8).

Gotchas and failure modes:

Error workflow itself can fail: If your error workflow errors, n8n falls back to logging to its internal error log. Keep the error workflow SIMPLE. The classification code node should have a try/catch that defaults to 'unknown' rather than throwing.
Retry-from-API creates a new execution: POST /executions/{id}/retry (if it exists at your version) creates a NEW execution, not a continuation. The original failed execution stays failed. Track retry lineage via a custom header or a mapping table.
429 retry is the exact failure class #4: Your classification MUST catch 429 before any generic retry logic. Test this specifically.
Error Trigger payload shape uncertainty: I am not 100% certain of the exact field names at 2.14.2. Your FIRST step is to trigger a test error, let your existing error workflow log the raw payload, and inspect the actual structure. Do NOT assume my field names are correct.

Rollback: Remove the classification/routing nodes from the error workflow, revert to your current flat dedup/rate-limit path. DLQ entries in Postgres are inert if nothing reads them.

Success metric: "Zero retry attempts on 429 errors in a 14-day window (measured by querying DLQ for classification='non_retryable' AND retried=true — this count MUST be 0)." Harvest date: 14 days after deployment.

Source classification: Pattern is well-established (DLQ is a standard pattern). Error Trigger node exists and is documented. Exact payload field names: community report / inference — verify at your version before building.

TOP 3: Retry-from-Failure via API

What it is: n8n's REST API has (or had) an endpoint to replay a failed execution from the point of failure. This lets you retry without re-running the entire workflow from the beginning.

Implementation sketch:

text
# Verify the endpoint exists at your version:
curl -X POST https://your-n8n/api/v1/executions/{failed_execution_id}/retry \
  -H "X-N8N-API-KEY: your-key" \
  -H "User-Agent: your-approved-UA"  # Your WAF rule!

Integrate into the DLQ pattern (Top 2): for retryable errors with retry_count < 3, call this endpoint from an HTTP Request node inside the error workflow.

Track retry count — the execution itself doesn't carry a retry counter. Options:

Store retry count in a Postgres table keyed by original execution ID
Use the execution's retryOf field if n8n populates it (unverified — check)
Tag the retried execution with metadata (if n8n supports execution tags at your version — unverified)

Gotchas and failure modes:

Endpoint may not exist at 2.14.2: This endpoint existed pre-1.0. I cannot guarantee it hasn't been removed or renamed. Test with a single known-failed execution FIRST.
WAF User-Agent requirement: Your own failure class — any HTTP Request node calling your own n8n API must set the User-Agent header, or the WAF will 403 it. This is trivially solvable but fatally easy to forget.
Retry executes from the FAILED NODE, not from the start: This is the whole point, but it means the retry depends on the execution's intermediate data being available. If execution data has been pruned (see Top 1), retry will fail.
Concurrency: If 50 executions fail simultaneously and your error workflow retries all of them, you get a thundering herd. Your existing rate-limit (≤5 alerts/minute) helps, but retry rate-limiting should be SEPARATE from alert rate-limiting.
The retried execution is a NEW execution: It gets a new ID. The original stays in error state. Your dashboards must account for this or you'll double-count.

Rollback: Remove the retry HTTP Request node from the error workflow. Existing retried executions are unaffected.

Success metric: "Successfully retried ≥3 executions from the DLQ in the first 14 days, each reaching success state on retry." Harvest date: 14 days.

Source classification: Endpoint documented in pre-cutoff n8n API docs. Existence at 2.14.2: unverified — test before building.

TOP 4: Zombie Schedule Detection Script

What it is: A deterministic, API-based check that compares the set of workflows with active=false against the set of workflows that have recent trigger-mode executions. Any intersection is a zombie.

Implementation sketch (Python, runs from your Linux anchor or cron):

python
import requests, datetime


N8N_API = "https://your-n8n/api/v1"
HEADERS = {
    "X-N8N-API-KEY": "your-key",
    "User-Agent": "Mozilla/5.0 (your approved UA string)"
}
ZOMBIE_THRESHOLD_HOURS = 26  # Slightly more than 24h to avoid false positives


# Step 1: Get all inactive workflows
resp = requests.get(f"{N8N_API}/workflows", headers=HEADERS, params={"active": "false"})
inactive_workflows = {w["id"]: w["name"] for w in resp.json()["data"]}


# Step 2: For each inactive workflow, check for recent trigger-mode executions
zombies = []
for wf_id, wf_name in inactive_workflows.items():
    cutoff = (datetime.datetime.utcnow() - datetime.timedelta(hours=ZOMBIE_THRESHOLD_HOURS)).isoformat()
    resp = requests.get(f"{N8N_API}/executions", headers=HEADERS, params={
        "workflowId": wf_id,
        "status": "success",  # Zombies often "succeed" because the trigger fires and the flow runs
        "limit": 1
    })
    executions = resp.json().get("data", [])
    if executions:
        last_exec = executions[0]
        # Check if the execution was trigger-initiated
        # Field name for "mode" or "trigger" may vary — INSPECT your actual API response
        if last_exec.get("mode") == "trigger":
            started = datetime.datetime.fromisoformat(last_exec["startedAt"].replace("Z", "+00:00"))
            if datetime.datetime.utcnow().replace(tzinfo=started.tzinfo) - started < datetime.timedelta(hours=ZOMBIE_THRESHOLD_HOURS):
                zombies.append({
                    "id": wf_id,
                    "name": wf_name,
                    "last_trigger_exec": last_exec["startedAt"],
                    "execution_id": last_exec["id"]
                })

CRITICAL: field name verification. The mode field (values: "trigger", "webhook", "manual", etc.) is what distinguishes a schedule-triggered execution from a manually-triggered one. I am not certain this field exists in the executions API response at 2.14.2, or that its value is "trigger". You MUST inspect a real API response from a known schedule-triggered execution before relying on this.

Alternative detection if mode field doesn't exist: check if the workflow has a Schedule Trigger node in its definition, AND has recent executions. Less precise but still useful.

Root cause analysis (honest assessment):

I cannot give you the definitive root cause in the n8n codebase at v2.14.2. Here is what I know from pre-cutoff architectural understanding:

n8n's ActiveWorkflowRunner manages trigger lifecycle. When a workflow is activated, the runner calls addWorkflow() which registers the trigger (for schedule triggers, this typically means adding a cron job to an in-process scheduler like node-cron or to a queue-based scheduler in Redis queue mode). When deactivated, it calls removeWorkflow() which should unregister the trigger.

The zombie bug likely stems from one of:

Race condition in deactivate path: The API sets active=false in the DB and returns 200, but the in-process runner's removeWorkflow() hasn't completed (or failed silently).
Queue mode split-brain: In queue mode, the main process handles the API call (DB update) but the worker process holds the trigger registration. If the "deactivate" signal doesn't reach the worker, the trigger keeps firing.
Missing cleanup in certain deactivate code paths: There may be a code path (e.g., deactivate via API vs. deactivate via UI) that updates the DB but skips the trigger unregistration.
Cron library quirk: node-cron or similar may not reliably remove scheduled jobs under certain conditions.

Whether it's fixed at 2.14.2: I cannot establish this. I have not seen a definitive PR that addresses this class of bug. Community reports of zombie schedules continued through early 2025. You should check the n8n GitHub issues for issues tagged "schedule" + "zombie" or "active false" + "still running" and look for closed issues with fix PRs.

Remediation when detected: Your own finding — force-toggle activate→deactivate or delete and recreate the schedule node. The API-based approach: PUT /workflows/{id} with active: true, wait 10 seconds, then PUT again with active: false. This forces a full removeWorkflow() → addWorkflow() → removeWorkflow() cycle.

Rollback: This is a read-only detection script. No rollback needed.

Success metric: "Script runs daily for 14 days with zero false positives (no active workflows flagged as zombies) and can detect a manually-created zombie within 26 hours." To test: temporarily create a zombie by activating a schedule workflow, then setting active=false via API without toggling — if the script detects it, it works. Harvest date: 14 days.

Source classification: Detection pattern: your inference and mine (standard differential check). Root cause: not established — requires source code inspection at your version. Fix status: not established.

TOP 5: Absence-of-Event Monitoring (Trigger Heartbeat)

What it is: A system that detects when a trigger that SHOULD be firing has STOPPED firing. This directly addresses failure classes #1 (zombie schedules, inverted), #2 (workflow silently deactivated), and #9 (silent degradation). It also covers the "webhook-with-zero-executions" class.

The fundamental problem: Absence-of-event cannot arrive as an event. You need an external system that EXPECTS to see evidence of each trigger and alarms when it doesn't.

Implementation sketch:

Layer 1: Trigger heartbeat table (Postgres)

sql
CREATE TABLE trigger_heartbeats (
    workflow_id INTEGER PRIMARY KEY,
    trigger_type TEXT NOT NULL,        -- 'schedule', 'webhook', 'telegram'
    expected_interval_min INTEGER NOT NULL,
    last_seen_at TIMESTAMPTZ NOT NULL,
    alert_threshold_min INTEGER NOT NULL,  -- typically 2-3x expected interval
    is_active BOOLEAN NOT NULL DEFAULT true
);

Layer 2: n8n workflow that UPDATEs the heartbeat on every trigger fire

Add a single node (HTTP Request or Postgres node) at the START of each monitored workflow:

text
Schedule Trigger / Telegram Trigger / Webhook
  → HTTP Request: POST https://your-anchor/heartbeat
       body: {"workflow_id": "{{$workflow.id}}", "trigger_type": "schedule"}
  → [rest of existing workflow]

The anchor endpoint writes/updates last_seen_at = now() in the trigger_heartbeats table. This adds ~1 node and ~50ms to each workflow execution. With 1000 exec/day, that's 1000 tiny writes/day — negligible.

Layer 3: External checker (runs on the Linux anchor, NOT in n8n)

A cron job (every 5–10 minutes) on the Linux anchor:

python
# Pseudocode
for row in trigger_heartbeats.where(is_active=True):
    elapsed = now() - row.last_seen_at
    if elapsed > row.alert_threshold_min minutes:
        fire_alert(f"Trigger silent: workflow {row.workflow_id} ({row.trigger_type}), "
                   f"last seen {elapsed} ago, expected every {row.expected_interval_min}min")

This checker MUST run outside n8n per your rule #8. It runs on the Linux anchor which is always-on and independent.

Layer 4: Bootstrap the table from your existing workflows

python
# One-time: for each active workflow, inspect its nodes for trigger type
for wf in get_all_workflows(active=True):
    trigger_type = detect_trigger_type(wf['nodes'])  # 'schedule', 'webhook', 'telegram', 'polling'
    if trigger_type:
        expected_interval = extract_interval(wf['nodes'])  # e.g., 1 minute for your CRM poller
        upsert into trigger_heartbeats(workflow_id, trigger_type, expected_interval,
                                        alert_threshold_min=expected_interval*3,
                                        last_seen_at=now())  # Initialize to now to avoid immediate false alarm

Special case: webhooks with zero executions. Your GitHub PR webhook has zero executions. The heartbeat system handles this naturally: if it's registered with expected_interval=NULL or a very large threshold, it won't alarm. But to DETECT "this webhook was supposed to be connected but has never fired," add a separate check: WHERE trigger_type='webhook' AND last_seen_at IS NULL AND created_at < now() - 7 days. This flags webhooks that were registered but never received a hit.

Gotchas and failure modes:

Bootstrap false alarms: When you first deploy, all last_seen_at values are "now." If a workflow's trigger hasn't fired yet, the elapsed time starts counting from bootstrap. Set last_seen_at = now() initially and accept that you're blind for the first threshold period.
The heartbeat call itself can fail: If your Postgres endpoint is down, heartbeats stop updating, and the checker alarms on EVERY trigger. This is a "who watches the watchman" problem — but the checker is on a different machine, so if both the heartbeat writer (n8n) AND the checker (anchor) fail simultaneously, you have a bigger problem. The checker should distinguish "one trigger silent" (likely the workflow) from "ALL triggers silent" (likely the heartbeat infrastructure).
Scheduled workflows with intentional gaps: Some schedules run only on weekdays, or at specific times. The expected_interval_min must account for this, or you'll get false alarms on weekends. Simple approach: set alert_threshold_min generously (3-5x the interval). Better approach: make the checker aware of the schedule pattern.
Adding a node to 71 active workflows: This is a bulk edit via the REST API. For each workflow, inject a Postgres/HTTP Request node as the first node after the trigger, connected trigger.output → heartbeat.input, heartbeat.output → [existing first node].input. This is mechanical but must handle connection rewriting carefully. Test on 2-3 workflows first.
Does this create a circular dependency? The heartbeat writer runs INSIDE n8n workflows. The checker runs OUTSIDE n8n. The checker alerts via a channel that does NOT depend on n8n (e.g., direct Telegram bot API call from the anchor, not through an n8n workflow). This satisfies rule #8 — the watcher is external.

Rollback: Remove the heartbeat nodes from workflows via REST API (bulk edit). Drop the Postgres table. Disable the cron job.

Success metric: "Within 30 days, the absence-of-event system detects at least 1 real silent-failure event that would not have been caught by our existing error workflow (which only fires ON error, not on silence)." Also: "Zero false alarms caused by the heartbeat infrastructure itself in a 14-day stability window." Harvest date: 30 days.

Source classification: Pattern: your inference and mine (standard heartbeat/absence-detection pattern, not n8n-specific). No n8n version dependency — uses standard nodes and external infrastructure.

3. "DO NOT DO THIS" SECTION
Confirmed anti-patterns (validating your list):

1. Do NOT put reasoning or the owner's writing voice into n8n. Your 2026-08-20 verdict is architecturally correct. n8n is an orchestrator, not a reasoning engine. Putting LLM chains inside n8n workflows means: (a) you can't easily swap models without editing workflows, (b) you lose the interactive feedback loop that CLI tools provide, (c) you pay the orchestration overhead on every token, (d) debugging a multi-step LLM chain inside an n8n execution view is painful compared to a terminal. Keep n8n as the trigger/CRM/routing layer. Local machines do the thinking.

2. Do NOT use the Python Code node for anything critical. Your failure class #5 is unresolved. Until someone identifies the root cause of the intermittent _input not defined error, Python Code nodes are unreliable. Your workaround (convert to JavaScript) is the correct mitigation. I cannot refute your conclusion with evidence — I can only confirm that the Python Code node was a relatively new feature at my cutoff and had known rough edges.

3. Do NOT use MCP tools for production workflow editing. Your failure class #2 directly demonstrates why. The n8n-mcp community tool's strict validation (rejecting disconnected nodes) is MORE restrictive than n8n core, which means "safe" MCP operations can force destructive edits. Your conclusion — "raw REST PUT is safer than the MCP removeNode for orphan-heavy workflows" — is consistent with the evidence you presented and I have no contrary evidence. I cannot fully confirm it because I don't have the MCP tool's source code behavior at your version, but the logic is sound.

4. Do NOT move fleet watchdogs into n8n. Rule #8 is correct. A watchdog that lives inside the thing it watches will fail silently alongside the thing it's supposed to watch. Your external dead-man's-switch design (on the Linux anchor, checking n8n from outside) is the correct architecture.

5. Do NOT store secrets in n8n's credential store if they protect local resources. Your architecture rule (secrets stay local) is correct. Even if n8n's external secrets feature works, the point is that n8n should not HAVE the secrets for local resources — it should not need them because it should not reach local resources.

Anti-patterns you may not have considered:

6. Do NOT use n8n as a message queue. Your outbound-poll job queue (local polls cloud) is the correct pattern. Do NOT be tempted to replace it with "n8n workflows that push to local machines via webhook/tailnet." Reasons: (a) n8n has no message persistence guarantee — if the n8n process restarts between "generate job" and "deliver job," the job is lost; (b) n8n has no dead-letter queue for outgoing messages; (c) n8n has no consumer acknowledgment — it can't tell if the local machine actually processed the job; (d) your poll-based queue gives you pending → claimed → ready → delivered state tracking that n8n cannot match. Keep the queue in Postgres, keep n8n as a producer, keep local as the consumer.

7. Do NOT put retry logic on 429 errors. You already learned this (failure class #4), but I'll state it as a general rule: classify BEFORE retrying. Any retry policy that doesn't inspect the error type first will make some error classes strictly worse. Build the classification gate (Top 2) before any retry logic.

8. Do NOT use sub-workflows (executeWorkflowTrigger) for high-frequency operations without understanding the overhead. Each sub-workflow execution creates a full execution record, goes through the trigger registration/resolution path, and adds latency. Your CRM agent's 20 toolWorkflow calls are fine because they're agent-driven (not every execution hits all 20). But if you were to, say, put every lead-processing step in a separate sub-workflow called 413 times/day each, you'd multiply your execution count and storage. Use sub-workflows for REUSE (same logic called from multiple parents), not for MODULARITY within a single flow.

9. Do NOT use the n8n Schedule Trigger for anything where missed ticks matter. Schedule triggers in n8n do NOT have catch-up behavior for missed ticks (as far as I know from pre-cutoff). If n8n is down from 2:00 to 2:10 and a workflow is scheduled for every minute, those 10 ticks are LOST — they don't fire 10 times when n8n comes back. If you need guaranteed tick processing, use an external scheduler (cron on the anchor) that POSTs to an n8n webhook. The webhook will queue the requests and they'll be processed when n8n is available. This is a fundamental architectural difference: schedule triggers are "fire at this time" (lossy), webhooks are "deliver this message" (can be made durable).

10. Do NOT assume active=false stops a schedule trigger. Failure class #1. Until you have the zombie detection script (Top 4) running and confirming no zombies, treat active=false as a REQUEST to stop, not a guarantee. For critical deactivations, verify via the API that no new trigger-mode executions appear in the following threshold period.

11. Do NOT use community nodes. See §(g) analysis below. For your stack (Telegram, MongoDB, GitHub, Postgres), HTTP Request nodes give you full control, no arbitrary code execution risk, no maintenance dependency on a third-party maintainer, and no version-locking to a specific n8n release. The functionality gap is small; the risk gap is large.

12. Do NOT build a "control tower" dashboard in n8n. The owner explicitly rejected this. But more fundamentally: n8n's canvas is a workflow editor, not an operational dashboard. Your HTML dashboards are the right tool. Don't let n8n's visual appeal creep into operational tooling decisions.

13. Do NOT put GPU work, knowledge vault access, or IP-sensitive outbound posting through n8n. Your architectural rule is correct and I have no counter-argument. These belong on the hub, accessed via the outbound-poll queue.

"Orchestrator creep" symptoms — when an n8n workflow should have been a 40-line script:
The workflow has more than 15 nodes and none of them are triggers or LLM calls (pure data transformation/routing)
The workflow contains a Code node with >50 lines that doesn't need n8n's integration context (no API calls, no database access that isn't also available from a script)
You're working around n8n's data model (e.g., splitting items, re-joining them, splitting again) instead of letting a script handle the data naturally
The workflow's execution time is dominated by Code node execution, not by waiting on external APIs
You've had to add "helper" workflows that exist only to serve another workflow's data needs

The test: "Could I replace this with a Python script that reads from the same DB, calls the same APIs, and runs from cron?" If yes, and the workflow doesn't need n8n's trigger/webhook/credential-management capabilities, it should probably be a script.

4. ANSWERING PART 3 BLOCKS (a)–(g) IN DETAIL
(a) Under-used n8n 2.x Capabilities

Queue mode / worker scaling (Redis-backed):

What it buys you: Execution isolation from the main process, horizontal scaling (multiple workers), and a persistent job queue that survives restarts.

What it does NOT buy you at your volume: With ~1000 executions/day and one 1-minute poller, a single n8n process handles this easily. The poller (413 runs/day = ~17/hour) is not a throughput problem. Queue mode's main value for you would be execution isolation — a stuck execution can't block the main process — and restart resilience — queued jobs survive an n8n restart.

$getWorkflowStaticData('global') in queue mode: This is the critical question for you (failure class #6). In queue mode, static data is stored in the database and synchronized through the queue. The flush lag you observe (multi-second) is likely worse in queue mode, not better, because the write goes through Redis → worker → DB → Redis → reader, adding network hops. In default (non-queue) mode, static data is in-memory and the lag comes from the write-ahead log / debounce interval. I cannot verify this at 2.14.2 — test it.

Operational cost: Requires a Redis instance (you may already have one for other purposes). More complex deployment (main + worker processes). Monitoring Redis health becomes a new requirement.

Verdict for you: Pilot only if you need execution isolation or restart resilience. Do NOT adopt it to fix the static data flush lag — it may make it worse. If you adopt it, benchmark the static data flush latency before and after.

Source classification: Architecture inference based on n8n's queue mode design. Exact static data behavior at 2.14.2: unverified.

Error workflows — beyond what you already do:

Per-workflow vs. global: You use global (settings.errorWorkflow on all 63 active workflows). This is correct for your scale. Per-workflow error workflows would mean 63 separate error-handling workflows — unmaintainable. The global pattern with classification/routing (Top 2) gives you per-workflow behavior from a single workflow.

Error Trigger payload: As noted in Top 2, I cannot give you the exact field names at 2.14.2. Log the raw payload and inspect.

Dead-letter queue: See Top 2. The key insight is that your current error workflow is a notification pipeline (dedup → rate-limit → alert). A DLQ adds a storage and replay dimension on top of it.

Manual replay: See Top 3. The API endpoint for retrying from failure is the missing piece.

Source classification: Error Trigger node documented. Payload shape at 2.14.2: unverified. Retry endpoint: documented pre-cutoff, existence at 2.14.2: unverified.

External secrets:

Providers (at my cutoff): AWS Secrets Manager, Azure Key Vault, HashiCorp Vault, and possibly others. The exact list may have expanded by 2.14.2.

License tier: This was an Enterprise feature at my cutoff. The tier naming may have changed.

Does it remove credentials from the DB? Yes — when external secrets are configured, n8n stores only a reference (provider name + secret path/identifier), not the actual value. The value is resolved at execution time by calling the external provider.

Relevance to you: LOW. Your architecture already keeps secrets local — n8n doesn't HAVE the local secrets because it doesn't access local resources. The credentials n8n DOES have (Telegram bot tokens, API keys for cloud services) are cloud credentials that are appropriate to store in n8n's credential store. External secrets would add an external dependency (Vault/AWS) for marginal security benefit on credentials that are already in a controlled VPS.

Source classification: Documented pre-cutoff. Exact provider list and tier at 2.14.2: unverified.

Source control / git-backed workflow deployment:

What is versioned: Workflow definitions (nodes, connections, settings — exactly what GET /api/v1/workflows/{id} returns minus credentials and execution data).

What is NOT versioned: Credentials (stored separately), execution data, per-instance workflow IDs, active/inactive state (by design — you don't want a git push to accidentally activate/deactivate production workflows).

How environments/branches work: The intended model is: dev branch → test branch → main branch, each mapped to a different n8n instance. You push workflow changes through git, and pull/deploy on each instance.

License gate: This was a Starter or Pro feature at my cutoff (below Enterprise).

CRITICAL QUESTION — coexistence with REST API edits:

This is the question that determines whether this feature is usable for you. There are two possible models:

Git is the source of truth: You edit workflows via git (or via the n8n UI which commits to git), and never use the REST API for edits. The REST API becomes read-only.

Dual-write: Both git and REST API can modify workflows, and they sync. This is fragile — any edit via REST API that isn't committed to git creates drift, and any git push that doesn't account for API-made changes overwrites them.

I believe n8n's source control feature is designed for model 1 (git as source of truth), which means it CANNOT coexist with your REST API-driven edit workflow. If you adopt source control, you must stop editing workflows via REST API and instead edit them in git (JSON files) and push/deploy.

I cannot verify this at 2.14.2. The docs may have evolved. But the fundamental architectural tension remains: two write paths to the same resource without a merging strategy = drift.

Verdict for you: Pilot ONLY if you're willing to migrate your entire edit workflow from REST API to git-based. Given that your Python tooling (snapshot before/after, rollback pairs) already gives you versioning, the marginal value of n8n's built-in git integration is low. The cost (rewriting your deployment tooling) is high.

Source classification: Feature documented pre-cutoff. Coexistence with REST API: inference — verify in current docs and by testing.

Execution data retention / pruning configuration:

See Top 1 for the full implementation. The key env vars:

EXECUTIONS_DATA_PRUNE (true/false)
EXECUTIONS_DATA_MAX_AGE (hours)
EXECUTIONS_DATA_MAX_COUNT (integer)
EXECUTIONS_DATA_SAVE_ON_ERROR ("all" | "none" | minimal modes)
EXECUTIONS_DATA_SAVE_ON_SUCCESS ("all" | "none" | minimal modes)
EXECUTIONS_DATA_SAVE_MANUAL_EXECUTIONS (true/false)
EXECUTIONS_DATA_SAVE_EXECUTION_PROGRESS (true/false) — for long-running workflows, saves intermediate data

What SAVE_ON_SUCCESS actually does in 2.x: If set to anything other than "all", successful executions may have their input/output data stripped after completion, even within the retention window. You MUST set this to "all" for forensics.

Source classification: Documented pre-cutoff. Env var names: verified. Exact behavior of save modes at 2.14.2: likely stable but unverified.

Sub-workflow patterns:

executeWorkflowTrigger (formerly "Execute Workflow" node):

Calls a sub-workflow that starts with an Execute Workflow Trigger node
Parent WAITS for child to complete
Data passed via the trigger node's output
Child errors propagate to parent (the parent node errors)
Child execution IS logged separately in the executions list (you can find it by looking for executions of the child workflow ID)
BUT: the parent execution's view does NOT show the child's internal node execution details — it shows the Execute Workflow node as a single step. This is the "observability quirk" you mentioned.

toolWorkflow:

Used in AI Agent / LangChain node context
Exposes a sub-workflow as a "tool" with a name and description
The AGENT decides whether/when to call it (not the parent workflow's control flow)
Data contract: the agent sends parameters, the tool returns a result string
Error handling: if the tool sub-workflow errors, the agent receives the error as a tool response and can decide how to handle it
Observability: same quirk — the tool execution shows as a single step in the agent's execution view

Error propagation difference: executeWorkflowTrigger errors are HARD errors (parent fails). toolWorkflow errors are SOFT errors from the agent's perspective (the agent gets an error message and can retry or try a different tool). This is a significant architectural difference.

Typing: Neither pattern enforces strong typing on the data passed between parent and child. The child receives items as JSON. If the parent sends unexpected shapes, the child fails at runtime.

Source classification: Documented behavior. Node names may have changed at 2.14.2 — verify.

Evaluations:

At my cutoff, n8n was introducing evaluation features for AI workflows. These appeared to be designed for testing AI outputs against expected results (similar to LangChain's evaluation chains).

What I cannot tell you: Whether these are mature enough at 2.14.2 for production sanity checks, what the exact API/config looks like, and whether they can run on a schedule (e.g., "evaluate last 100 summarizations daily"). This feature was nascent at my cutoff.

Verdict for you: Later. Not enough information to evaluate.

Source classification: Not established — feature was too new at my cutoff.

AI Agent / LangChain nodes:

Current node inventory (at my cutoff): AI Agent node (the main orchestrator), sub-nodes for Chat Model, Memory, Tools, and output parsing. The Agent node supports ReAct, plan-and-execute, and other strategies.

Memory backends — Postgres chat memory: You report ~11% error rate. This is likely the flakiest part of your agent stack. Common failure modes for Postgres-backed chat memory: connection pool exhaustion (each agent execution opens a DB connection for memory read/write), table lock contention under concurrent executions, and schema migration issues after n8n updates. I cannot diagnose the specific 11% error without seeing the actual error messages. If you haven't already: query your error workflow's data for executions where the agent workflow errored, and look for patterns in the error messages.

Tool-calling patterns: Your toolWorkflow approach (20 sub-workflows as tools) is the standard n8n pattern. The agent receives tool descriptions and decides which to call. Each call triggers a sub-workflow execution.

Structured output: The Agent node supports output schemas (JSON Schema) that constrain the agent's final output. This is useful for ensuring the agent returns a parseable response.

Generic HTTP endpoint as model backend: See (c) below.

Source classification: Documented pre-cutoff. Postgres memory error rate: your data (I cannot verify the cause).

MCP server node and MCP client node:

At my cutoff, n8n was rolling out:

Instance-level MCP server: Exposes n8n workflows and data tables as MCP tools/resources. Two-level opt-in: instance toggle + per-workflow "Available in MCP" flag. Auth via OAuth2 or rotatable access token.
MCP client node: Allows n8n workflows to call external MCP servers.

Your concern about docs drift is valid. At my cutoff, the docs were inconsistent — some pages described the MCP server as "read-only authoring," others described it as "workflow building + data tables." This suggests the feature was still being defined.

Security assessment vs API key:

API key: simple, static (or rotatable), full API access. Compromise = full control.
MCP server with OAuth2: more complex setup, scoped access (per-workflow), token rotation built in. But adds an OAuth2 dependency.
MCP server with access token: similar to API key but with per-workflow scoping.

For your use case (docs-only MCP client, management tools OFF): The instance-level MCP server is not relevant to you right now. You're using MCP as a client to read n8n docs, not exposing n8n as an MCP server. The instance-level MCP server is for the opposite direction (external tools calling into n8n).

Source classification: Feature in flux at my cutoff. Exact capabilities at 2.14.2: unverified. Security assessment: inference.

Data Tables:

At my cutoff, Data Tables were a new feature for structured data storage within n8n — essentially a lightweight database accessible from workflows.

Are they the right replacement for $getWorkflowStaticData given failure class #6?

Maybe, but with caveats:

Data Tables are DB-backed (Postgres), so writes are immediately durable — no flush lag. This directly addresses failure class #6.
BUT: Data Tables have their own API overhead (read/write through n8n's API layer), which may be slower than in-memory static data for read-heavy patterns.
Data Tables are a shared resource — if multiple executions read/write the same table concurrently, you need to handle concurrency (optimistic locking, last-write-wins, etc.).
Maturity concern: This feature was very new at my cutoff. I cannot assess its reliability, performance, or API stability at 2.14.2.

Verdict: Pilot with a non-critical use case first. Do NOT migrate your dead-man's-switch heartbeat state (which depends on $getWorkflowStaticData) to Data Tables until you've verified write consistency under concurrent webhook executions.

Source classification: Community report / inference — feature was too new at my cutoff for reliable assessment.

Anything else genuinely under-used:

Pinned data for testing: n8n allows pinning input data to nodes (stored in the workflow definition). This lets you test a workflow with consistent data without live triggers. You should be using this for pre-deployment testing of workflows edited via REST API. Pin the expected input, do a manual execution, verify the output, then unpin before activating. This is the closest thing to a "dry run" n8n offers.

Execution breakpoints / step-through debugging: If this exists at 2.14.2 (it was being discussed at my cutoff), it would be valuable for debugging complex agent workflows without adding log nodes everywhere. Unverified at your version.

(b) "n8n as Event-Driven EARS for Local Machines"

Telegram trigger reliability:

n8n's Telegram Trigger node uses the Bot API long-polling method by default (not webhooks, not MTProto). This means n8n actively polls getUpdates on a loop.

During n8n restart: Messages sent during downtime are queued by Telegram's servers for up to 24 hours (Telegram's getUpdates guarantee). When n8n comes back, the next poll retrieves them. No messages are lost unless the downtime exceeds 24 hours or the messages are older than the oldest unconfirmed update.
During workflow deploy: If you update a workflow that contains a Telegram Trigger, the trigger is re-registered. There may be a brief gap (seconds) during which incoming messages are not processed. They remain in Telegram's queue.
Duplicate delivery: Long-polling with getUpdates uses offset-based deduplication. n8n acknowledges updates by incrementing the offset. If n8n crashes before acknowledging, the same updates are re-delivered on restart. Duplicates are possible but rare in practice.
Ordering: Telegram guarantees ordering within a single chat. Across chats, no ordering guarantee. Long-polling preserves per-chat ordering because it processes updates in the order returned by getUpdates.
Missed window recovery: Fully recoverable for windows < 24 hours. Not recoverable after that.

Bot API vs webhook mode vs MTProto:

Bot API polling (default): simple, works behind NAT, no inbound ports needed. This is what you're using.
Bot API webhook mode: Telegram pushes to your endpoint. Requires inbound HTTPS. More efficient (no polling) but more complex setup. Not suitable for your architecture (n8n is cloud, but you want to minimize inbound surface).
MTProto / user-account access: Uses a real Telegram client protocol. Can access channels/groups that bots can't. But: violates Telegram TOS for automation, requires phone number auth, session management is complex. There are community nodes for this but see §(g) for why to avoid them.

Schedule trigger vs event trigger reliability:

Schedule triggers do NOT have catch-up behavior. If n8n is down, those ticks are lost. (See anti-pattern #9.)
timezone handling: Schedule triggers use the workflow's timezone setting (or the instance default). If this is misconfigured, triggers fire at the wrong wall-clock time. DST transitions can cause double-fires or skipped fires depending on the cron expression and timezone implementation. I cannot verify n8n's DST handling at 2.14.2 — test with a timezone that has upcoming DST changes.
Zombie detection: See Top 4.

Waking a machine behind NAT:

Your current pattern (outbound poll queue) is correct. The local machine polls cloud; cloud never initiates to local. This maintains zero inbound ports.

Alternative: n8n-initiated call over tailnet:
If your n8n VPS is on the same tailnet as the hub, n8n COULD initiate a call to the hub's tailnet IP. This would mean:

The hub has zero inbound ports from the INTERNET, but DOES have an inbound listener on the tailnet interface.
This is probably acceptable for your security model (tailnet is a trusted network).
BUT: it breaks your architectural rule ("local polls cloud; home machine exposes no inbound ports"). The tailnet IP is still an inbound port from the hub's perspective.
AND: if the hub is asleep (Windows sleep), the tailnet interface is down and the call fails. You'd need Wake-on-LAN over the tailnet, which adds complexity.

Verdict: Stay with outbound poll. It's simpler, it works when the machine is asleep (jobs queue until it wakes), and it matches your architectural rule.

Idempotency and at-least-once:
Your pending → claimed → ready → delivered state machine with idempotency keys is the correct pattern. Ensure:

claimed has a timeout — if the local process crashes after claiming but before completing, the claim expires and another poll can pick it up.
delivered is the terminal state — never re-process a delivered job.
The idempotency key is generated by the PRODUCER (n8n), not the consumer, to prevent duplicate job creation.

Catch-up storms:
When the hub wakes after hours of sleep, it may have hundreds of queued jobs. If it processes them all at full speed, it could overwhelm local resources (GPU, API rate limits). Mitigation: the consumer should have a concurrency limit (process N jobs at a time, not all at once) and a backoff if error rate spikes.

Poison messages:
A job that consistently fails (bad data, incompatible format) can block the queue if the consumer re-queues it on failure. Mitigation: after N failures, move to a dead-letter state instead of re-queuing.

Webhook-with-zero-executions detection:
See the special case in Top 5 (Layer 4). Register the webhook in the heartbeat table with last_seen_at = NULL. The checker alarms if last_seen_at IS NULL AND created_at < now() - threshold. This detects "wired but never hit" webhooks.

To prove a webhook is actually wired end-to-end (not just registered in n8n but connected to the external source): send a test payload from the external source (e.g., a test GitHub push event) and verify it appears in n8n's execution list. This is a manual test, not automatable in general (you need the external source to cooperate).

Wait node / resume-webhook patterns:

The Wait node allows a workflow to pause and resume when a webhook is called or a time elapses. Durability across restarts: I cannot verify this at 2.14.2. At my cutoff, there were reports that Wait node state could be lost on n8n restart, leaving workflows permanently paused. If you need durable human-in-the-loop, implement it externally (state in Postgres, workflow polls for state change) rather than relying on the Wait node's internal state.

Source classification: Telegram trigger behavior: documented (Bot API specs) + inference about n8n's implementation. Schedule trigger catch-up: inference (standard cron behavior, but n8n may differ). Wait node durability: community report (unverified).

(c) Making n8n Use Paid Subscription LLM Quotas

HTTP Request node against a local OpenAI-compatible shim:

This is your current approach (sidecar at POST /ask). The question is how to do it RIGHT.

Exposing the on-prem endpoint to cloud n8n:

Options ranked by your constraints:

Tailnet: Both n8n VPS and hub on the same tailnet. n8n calls http://hub-tailnet-ip:port/ask. Simplest, lowest latency, no additional infrastructure. Risk: if the tailnet goes down, all LLM calls fail. Your WAF doesn't apply (tailnet traffic doesn't go through it).
WireGuard tunnel (separate from tailnet): Same concept, separate tunnel. Redundancy if tailnet has issues.
Cloudflare Tunnel (cloudflared): Hub runs cloudflared tunnel, n8n calls https://your-tunnel-domain/ask. Traffic goes through Cloudflare's network. Adds latency and a Cloudflare dependency, but works without direct network connectivity.
Reverse proxy with mTLS on a VPS: Stand up a tiny reverse proxy on a VPS with a public IP, mTLS between n8n and the proxy, SSH tunnel from the proxy to the hub. Most complex, most robust.

For your volume (~1000 LLM calls/day across all workflows): Option 1 (tailnet) is almost certainly sufficient. The failure mode (tailnet down) is detectable and you already have alerting infrastructure.

What breaks with streaming/SSE:

The HTTP Request node in n8n does NOT support SSE (Server-Sent Events) streaming. If your sidecar returns a streaming response, the HTTP Request node will either:

Buffer the entire response and return it when the stream completes (if it treats the response as a regular HTTP response)
Fail with a timeout if the stream takes longer than the configured timeout
Return only the first chunk (unlikely but possible depending on implementation)

This means you CANNOT use streaming with the HTTP Request node. The entire response must be generated before n8n sees any of it. For your use case (summarization, translation, tool calls), this is probably fine — you're not doing interactive chat through n8n. But if you ever need streaming (e.g., real-time transcription display), the HTTP Request node won't work.

n8n AI nodes with custom base URL:

n8n's OpenAI-compatible nodes (Chat Model, Embeddings) support setting a custom baseURL. This lets you point them at your sidecar instead of api.openai.com.

What this actually covers (at my cutoff):

Chat Model node with baseURL set: sends POST {baseURL}/chat/completions with OpenAI-compatible request body. Your sidecar must implement this endpoint.
Embeddings node with baseURL set: sends POST {baseURL}/embeddings.
Agent node using a Chat Model sub-node with custom baseURL: inherits the custom base URL.

Where it hard-codes provider behavior:

The request body format is OpenAI-specific (messages array with role/content, model field, etc.). Your sidecar must accept this format even if the underlying provider (Claude, Gemini) uses a different format. Your sidecar already does this translation.
Streaming: The Chat Model node may send stream: true in the request body by default. Your sidecar must handle this — either ignore it (generate full response) or support streaming (but n8n won't consume the stream, see above). Check if the node sends stream: true and if so, whether your sidecar handles it correctly.
Token counting: The node may try to parse usage from the response for token counting/display. If your sidecar doesn't return OpenAI-compatible usage data, the node may show incorrect token counts or log warnings.
Model name validation: Some n8n versions validate the model field against known OpenAI models. If your sidecar uses custom model names (e.g., claude-sonnet), the node may reject them. Check if there's a "disable model validation" option or if this was fixed.

Timeouts, retries, and concurrency:

HTTP Request node settings for a backend with minute-scale p95:

Timeout: Set to the maximum you can tolerate. For Claude at 900s timeout in the sidecar, set the HTTP Request node timeout to at least 960s (16 minutes). Note: n8n may have a global execution timeout that overrides this — check EXECUTIONS_TIMEOUT or per-workflow executionTimeout settings.
Retry: Set to 0 (no retries at the HTTP Request node level). Your sidecar already retries 3× internally. Adding n8n-level retries on top = 9 total attempts for a single call. Let the sidecar handle retries.
Full Response: Enable this to get the full response object, not just the body. Useful for debugging.

How to stop one slow call from starving the rest:

This is the core problem with your current single-threaded sidecar. The fix belongs in the sidecar, not in n8n.

Right server shape for a CLI-backed backend where each call spawns a process:

text
            ┌─────────────────────────────────┐
            │  Async HTTP server (aiohttp /   │
            │  FastAPI with async handlers)   │
            │                                 │
            │  - Accepts N concurrent requests│
            │  - Per-request timeout (config) │
            │  - Returns 503 if at capacity   │
            └──────┬──────────────────────────┘
                   │
            ┌──────▼──────────────────────────┐
            │  Process pool (asyncio.subprocess│
            │  or concurrent.futures)         │
            │                                 │
            │  - Max N concurrent CLI spawns  │
            │  - Queue excess requests        │
            │  - Per-process timeout          │
            └──────┬──────────────────────────┘
                   │
            ┌──────▼──────────────────────────┐
            │  CLI processes                  │
            │  (claude, codex, gemini)        │
            └─────────────────────────────────┘

Concrete recommendation: FastAPI + asyncio.subprocess

FastAPI with async def endpoints — inherently handles concurrent requests
asyncio.create_subprocess_exec() to spawn CLI processes — non-blocking
asyncio.wait_for() for per-request timeout — if the CLI process exceeds the timeout, kill it
asyncio.Semaphore(N) to limit concurrent CLI spawns — N should be the number of concurrent CLI processes your machine can handle (probably 2-3 for GPU-bound work, more for CPU-only)
Shared secret: read at request time from env (not at import time), fail gracefully with 500 if not set rather than crashing at startup

Concurrency limit calculation:

Each Claude/Codex/Gemini CLI call uses GPU memory and potentially GPU compute
If your hub has 2 GPUs and each call uses 1 GPU, max concurrency = 2
If calls can share a GPU (depends on model size and VRAM), maybe 3-4
Set the semaphore conservatively (2) and increase if you have headroom
When the semaphore is full, return HTTP 503 (Service Unavailable) with a Retry-After header. n8n's HTTP Request node can be configured to retry on 503.

Honest risk assessment of routing subscription-CLI traffic through an automation server:

Rate limits: Claude Code CLI, Codex CLI, and Gemini CLI each have their own rate limits (requests/minute, tokens/minute, concurrent sessions). Your sidecar must enforce per-provider rate limiting. If n8n sends 10 concurrent requests to Claude, and Claude allows 3 concurrent, 7 will fail or queue.
Per-account concurrency: Most subscription CLIs allow only 1-2 concurrent sessions per account. If you have 1 Claude account, you can run 1-2 Claude calls simultaneously. This is your REAL bottleneck, not the sidecar's threading.
Terms of service: Using subscription CLI tools as an API backend may violate ToS. Claude Code's terms specifically allow programmatic use (it's designed for it), but other CLIs may not. I cannot give legal advice. This is a risk you must assess.
Fallback ladder inside n8n:
text
Try Claude → if 503/429/error → Try Gemini → if 503/429/error → Try Codex → if all fail → DLQ + alert

Implement as a sequential fallback chain (NOT fan-out race). Fan-out races burn quota on multiple providers for a single request. Sequential fallback only uses the next provider if the previous one fails.

Circuit breaker: Track per-provider failure rate. If Claude fails 5 times in 10 minutes, stop trying Claude for 30 minutes (circuit open). This prevents wasting time on a provider that's down or quota-exhausted. Implement in the sidecar or in an n8n workflow that wraps the HTTP Request call.

Where this is a bad idea and you should just pay per token:

If the engineering cost of building a reliable async sidecar + circuit breaker + fallback ladder exceeds the token cost savings over 6 months
If ToS violations are a real risk for your use case
If you need streaming (which this architecture doesn't support)
If per-account concurrency limits make the subscription approach impractical (e.g., you need 5 concurrent Claude calls but your account allows 1)

At your current burn rate ($216/15 days on a single provider), you're spending ~$432/month on metered API. Your subscriptions cost $540/month for "generous" quotas. If the subscriptions can absorb the current workload, the savings are ~$432/month minus the engineering cost of the sidecar (one-time, maybe 2-3 days of work). Payback period: under a month. It's worth doing.

Source classification: HTTP Request node behavior: documented + inference. Custom base URL support: documented pre-cutoff. Streaming limitation: inference based on node design. Sidecar architecture: inference (standard async server pattern). ToS risk: not established (requires legal analysis).

(d) Monitoring n8n Itself

Prometheus metrics endpoint:

Enable via N8N_METRICS=true env var. Exposes /metrics on the n8n port.

What it exposes (at my cutoff):

n8n_executions_total (counter, by status: success/error/running)
n8n_execution_duration_seconds (histogram)
n8n_active_workflows (gauge)
Queue-related metrics if in queue mode: n8n_queue_size, n8n_workers_active

What predicts your failure classes:

n8n_executions_total{status="error"} rising → error spike (but your error workflow already catches this)
n8n_execution_duration_seconds p99 increasing → slow backend (could indicate sidecar blocking)
n8n_active_workflows decreasing → workflows being deactivated (failure class #2)
n8n_queue_size growing unbounded (queue mode only) → workers stuck

What it does NOT cover:

Per-trigger firing rate (you can't tell if a specific schedule trigger stopped firing)
Credential validity (expired API keys don't show up in metrics)
WAF block rate (that's in your WAF's metrics, not n8n's)
Webhook delivery success/failure (no metric for "webhook received 0 hits today")

Verdict: Enable it (trivial cost), but it's a complement to your heartbeat system (Top 5), not a replacement. Prometheus metrics tell you ABOUT executions; heartbeats tell you about the ABSENCE of executions.

Source classification: Documented pre-cutoff. Exact metric names at 2.14.2: unverified — inspect your /metrics endpoint.

Health/readiness endpoints:

n8n exposes /healthz (liveness) and potentially /readyz (readiness). These check:

Liveness: is the n8n process running?
Readiness: is n8n connected to its DB (and Redis, if in queue mode)?

What they do NOT check:

Are workflow triggers actually firing?
Are credentials valid?
Is the WAF passing traffic?

DB and queue health signals:

Postgres: monitor pg_stat_activity for connection count, pg_locks for lock contention, replication lag if applicable
Redis (if queue mode): redis-cli info for memory usage, connected clients, blocked clients

Memory-leak indicators (Postgres-backed):

n8n's main memory usage growing monotonically over days/weeks without corresponding increase in execution volume
Node.js heap growing (check via process.memoryUsage() if you add a monitoring endpoint, or via external process monitoring)
At my cutoff, there were community reports of memory growth in SQLite-backed instances (due to SQLite's in-memory caching). Postgres-backed instances were less affected because the DB handles caching. But: any n8n instance that caches workflow definitions, credential data, or execution data in memory could leak. Watch the process's RSS over time.

External dead-man's-switch for n8n:

Design (runs on the Linux anchor, outside n8n):

text
Every 5 minutes, anchor cron:
  1. GET https://n8n/healthz → if not 200, alert "n8n down"
  2. GET https://n8n/api/v1/executions?limit=1 → if no response or error, alert "n8n API unresponsive"
  3. Check Prometheus metrics (if exposed) for n8n_executions_total in last 5 minutes → if zero, alert "no executions in 5 min"
  4. (Optional) POST a test webhook on n8n and verify the execution completes → end-to-end check

Step 4 is the strongest check but adds load. Steps 1-3 are lightweight and catch most failure modes.

Error-workflow best practices at scale:

Your existing anti-storm gate (dedup within 10 min, rate-limit ≤5/min, summary every 5 min) is solid. I am not aware of a native n8n feature that replaces this at my cutoff. If one was added by 2.14.2, I cannot confirm it.

Distinguishing "one workflow broken" from "instance-wide outage":

One workflow broken: error workflow fires with a single workflow ID
Instance-wide outage: error workflow fires with MANY different workflow IDs in rapid succession, OR the error workflow itself fails to fire (which means n8n is down, caught by the external dead-man's-switch)

Your existing rate-limiting handles this implicitly: a storm of errors from many workflows triggers the "storm, N suppressed" summary, which is distinguishable from a single-workflow error.

Daily green receipt:
Add a daily scheduled workflow that:

Checks the heartbeat table (Top 5) for any triggers that haven't fired in their expected window
Checks the error workflow's DLQ (Top 2) for unresolved entries
Posts a "ALL GREEN: X triggers healthy, Y errors resolved, Z DLQ items pending" message to Telegram

This gives you a daily positive signal, not just negative alarms. If you DON'T receive the daily green, that itself is an alarm.

Detecting silent failure specifically:

FAILURE MODE
	
DETECTION METHOD

Trigger stopped firing	Heartbeat system (Top 5)
Workflow deactivated by edit	Heartbeat system + n8n_active_workflows metric decreasing
Credential expired	Test execution of a workflow using that credential (periodic smoke test)
WAF 403ing one client	WAF logs (not n8n-side detectable without an external check using the same client)
Schedule became zombie	Zombie detection script (Top 4)
Webhook never connected	Heartbeat system last_seen_at IS NULL check
Sidecar blocked	HTTP Request node timeout → error → error workflow → DLQ classification as "timeout"

The WAF case is the hardest to detect from n8n's side because n8n sees the 403 and may not distinguish "WAF blocked" from "endpoint gone." Your existing lesson (test with the same client) is the mitigation. For ongoing monitoring: have your external dead-man's-switch send a test request with the SAME User-Agent that production uses (not curl), and alarm if it gets 403.

Source classification: Prometheus metrics: documented pre-cutoff, exact names unverified. Health endpoints: documented. External DMS design: inference (standard pattern). Native error dedup: not established (may have been added post-cutoff).

(e) Anti-Patterns

Covered in Section 3 above. No additional anti-patterns beyond the 13 listed.

(f) Programmatic Workflow Deployment and Versioning via the API

Safe update protocol:

python
def safe_update_workflow(workflow_id, new_nodes, new_connections, new_settings=None):
    # 1. Read current state
    current = get_workflow(workflow_id)
    
    # 2. Snapshot to disk (you already do this)
    snapshot_to_disk(current, label="before")
    
    # 3. Build the PUT payload — ONLY allowed fields
    payload = {
        "name": current["name"],  # Preserve name unless intentionally changing
        "nodes": new_nodes,
        "connections": new_connections,
    }
    if new_settings is not None:
        payload["settings"] = new_settings
    elif current.get("settings"):
        # Carry forward ONLY whitelisted settings keys
        allowed_keys = {
            "executionOrder", "errorWorkflow", "executionTimeout",
            "saveDataErrorExecution", "saveDataSuccessExecution",
            "saveExecutionProgress", "saveManualExecutions",
            "timezone", "callerPolicy", "callerIds"
        }
        payload["settings"] = {
            k: v for k, v in current["settings"].items()
            if k in allowed_keys
        }
    
    # 4. NEVER include these fields in the payload:
    #    id, active, versionId, tags, staticData, pinData,
    #    createdAt, updatedAt, any credential references at top level
    
    # 5. PUT
    response = put_workflow(workflow_id, payload)
    
    # 6. VERIFY by reading back — do NOT trust the response

How to avoid the settings whitelist 400: Step 3 above — always filter settings through the allowed keys set. NEVER pass through settings from a workflow that was last saved by the n8n UI (which may include legacy keys like binaryMode).

How to guarantee an edit never silently changes active: Step 6-7 — read back and compare. The PUT response may claim success even if active changed (failure class #2). The ONLY reliable check is a subsequent GET.

Why failure class #2 happened with MCP: The MCP removeNode operation likely:

Reads the workflow
Removes the specified node
Validates the result (rejects disconnected nodes)
Writes back via PUT

If step 3 fails, the MCP tool may have fallen back to a "save anyway" path that included active: false in the payload (perhaps as a safety measure — "deactivate before editing"). Or the MCP tool's PUT payload included active: false as part of its "safe edit" protocol. I cannot verify the MCP tool's exact behavior because I don't have its source code. But your observation (MCP edit flipped active=false) combined with your workaround (raw REST PUT doesn't do this) strongly suggests the MCP tool is the culprit, not n8n core.

Your conclusion ("raw REST PUT is safer than the MCP removeNode for orphan-heavy workflows") is consistent with the evidence. I cannot fully confirm it without inspecting the MCP tool's code, but I have no contrary evidence.

Diff/rollback strategies for workflow JSON:

What makes n8n workflow JSON diff-hostile:

Node positions: Every node has position: [x, y]. Edits that don't change logic can change positions (e.g., n8n auto-layout). Diff noise.
Pinned data: pinData contains test data pinned to nodes. Changes frequently, irrelevant to logic diffs.
Node typeVersion: May change across n8n versions even if the node's behavior doesn't change.
Credential references: Stored as {"id": "uuid", "name": "Credential Name"}. The UUID is instance-specific. Diffing across instances shows credential ID changes that aren't meaningful.
Connection format: Connections are keyed by node name and output/input index. A node rename changes the connection keys, producing a large diff for a small change.
parameters ordering: JSON object key ordering isn't guaranteed. The same parameters can serialize in different orders across reads, producing noisy diffs.

Mitigation strategies:

Strip position, pinData, and typeVersion before diffing (normalize)
Replace credential UUIDs with credential names for diff purposes
Sort JSON object keys (use json.dumps(obj, sort_keys=True))
Use a semantic diff tool that understands JSON structure (not line-level diff)

Promoting changes across instances:

Workflow IDs are instance-specific. A workflow with ID 42 on dev may have ID 87 on prod.
Credential IDs are instance-specific. You MUST remap credential references.
Recommended approach: Don't try to sync IDs. Instead: export the workflow from dev (strip ID, strip credential IDs, keep credential NAMES), import to prod (n8n assigns new ID), then use the REST API to link credentials by name.
The built-in source control feature handles this mapping. If you don't use source control, you must build the remapping yourself.

Source control vs API edits — can they coexist?

See (a) above. My assessment: probably not safely. They represent two write paths to the same resource. If you adopt source control, stop using the REST API for edits. If you keep the REST API, don't enable source control.

Testing a workflow before activating it:

Pinned data: Pin expected input data to the trigger node. Run a manual execution. The trigger node outputs the pinned data instead of waiting for a real trigger event. Verify the output.
Manual execution via API: POST /api/v1/workflows/{id}/run — this executes the workflow once without activating it. Check the response for the execution ID, then fetch the execution result.
Dry run: There is no true "dry run" in n8n — any execution that calls external APIs will make real API calls. The closest thing is: pin data that mimics the expected input, run manually, inspect the output WITHOUT letting the final step execute (e.g., disable the "post to Telegram" node, check that the preceding nodes produced the right data).

I cannot verify the exact API endpoint for manual execution at 2.14.2. At my cutoff, it was POST /api/v1/workflows/{id}/run. Check your API.

Source classification: Settings whitelist: verified (you documented it, and it matches pre-cutoff docs). MCP behavior: inferred from your evidence. Diff-hostility: verified (structural properties of n8n JSON). Source control coexistence: inference — verify in current docs. Manual execution endpoint: documented pre-cutoff, unverified at 2.14.2.

(g) Community Nodes for Our Stack

General security assessment of community nodes:

Community nodes in n8n run as part of the n8n process with the same permissions. A malicious or compromised community node can:

Read all credentials stored in n8n's database
Make arbitrary network requests from your VPS
Execute arbitrary code on your VPS
Access your n8n API key and database connection string

The install process (npm install from a community package registry) runs code at install time. There is no sandboxing, no code review gate, and no signature verification for community nodes.

For a security-conscious team that already has a "blast radius" mindset (your MCP docs-only mode decision), community nodes are a poor fit.

Telegram:

Official n8n Telegram node: supports Bot API only (polling and webhook modes). Does NOT support MTProto/user-account access.
Community Telegram nodes (e.g., n8n-nodes-telegram-userbot): provide MTProto/user-account access. Maintenance status at my cutoff: uncertain, typically maintained by individuals. Security: runs arbitrary Telegram client code in your n8n process.
Verdict for you: If you need Bot API functionality, use the official node. If you need MTProto/user-account access, use a local script on the hub that connects via MTProto and exposes an HTTP API to n8n. Do NOT install a community MTProto node.
Are you better off with HTTP Request? For Bot API: the official node is fine (it's maintained by n8n). For MTProto: yes, HTTP Request to a local MTProto proxy is safer than a community node.

MongoDB:

Community MongoDB node (n8n-nodes-mongodb or similar): provides native MongoDB operations (find, insert, update, aggregate).
Maintenance status at my cutoff: maintained but not by n8n core team. Occasional version lag behind MongoDB driver updates.
Are you better off with HTTP Request? If you have a MongoDB API layer (e.g., MongoDB Atlas Data API, or a custom REST wrapper), HTTP Request works. If you need raw MongoDB operations (aggregation pipelines, change streams), the community node is the only option within n8n. But: you could also run MongoDB operations from a local script exposed via HTTP, keeping the MongoDB driver out of n8n.
Verdict for you: Given that your CRM poller (the heaviest MongoDB user) runs 413 times/day and has ~0% error rate with whatever method you're currently using, don't change it. If you're already using the community node and it works, the migration risk isn't worth it. If you're using HTTP Request to a MongoDB API, keep that too.

GitHub:

Community GitHub node (n8n-nodes-github): provides GitHub API operations (list PRs, create issues, etc.).
Maintenance status at my cutoff: maintained, reasonable quality.
Are you better off with HTTP Request? The GitHub REST API is well-documented and straightforward. HTTP Request nodes give you full control over headers (including your WAF-friendly User-Agent), pagination, and error handling. The community node may abstract these in ways that hide useful details.
Verdict for you: Use HTTP Request. Your GitHub PR webhook workflow has zero executions — you don't need a rich node for a workflow that isn't running. When you do connect it, HTTP Request to the GitHub API is simpler and more transparent.

Postgres/pgvector RAG:

n8n has a built-in Postgres node. It handles basic queries.
For pgvector-specific operations (vector search, embedding storage), the built-in node may not support =>> or vector operators natively.
Community pgvector node: I am not aware of a mature one at my cutoff.
Are you better off with HTTP Request? For pgvector queries, you likely need to run custom SQL (SELECT * FROM docs ORDER BY embedding <=> $1 LIMIT 5). The built-in Postgres node may support raw SQL mode. If it does, use it. If not, HTTP Request to a local Postgres HTTP wrapper (e.g., PostgREST, or a custom FastAPI endpoint) gives you full control.
Verdict for you: Use the built-in Postgres node for basic operations. For pgvector, test whether the built-in node's raw SQL mode supports vector operators. If not, add a small HTTP endpoint on the hub (or the Linux anchor, since Postgres may be accessible from there) that accepts a query embedding and returns results.

Summary table:

INTEGRATION
	
COMMUNITY NODE?
	
USE INSTEAD
	
REASON

Telegram Bot API	No (official node exists)	Official node	Maintained by n8n, no security concern
Telegram MTProto	Yes (community)	Local MTProto proxy + HTTP Request	Arbitrary code execution risk, TOS risk
MongoDB	Yes (community)	Keep whatever works	Don't fix 0% error rate
GitHub	Yes (community)	HTTP Request to GitHub API	Simpler, more transparent, no security risk
Postgres/pgvector	Built-in (basic) + none (pgvector)	Built-in for basic; custom HTTP for pgvector if needed	No mature community pgvector node

Source classification: Official node capabilities: documented. Community node security model: documented (no sandboxing). Community node maintenance status: community report — verify current status before any decision. Postgres node raw SQL support: unverified at 2.14.2.

5. OPEN QUESTIONS

These are things I could not establish with confidence. You must test or research them yourself.

Zombie schedule root cause in n8n codebase at v2.14.2. I've described the likely architectural causes but cannot point to the exact code path or confirm whether it's fixed. Action: inspect ActiveWorkflowRunner source at your version, or search n8n GitHub issues for "schedule" + "active false" + "still running" and look for closed issues with fix PRs.

Python Code node _input intermittent failure root cause. I cannot identify why byte-identical code fails in one workflow and succeeds 4155× in another. Action: file a bug report with the two workflow JSONs and the exact error stack trace. Or: add logging inside the Python sandbox (if possible) to inspect what's in the global namespace when _input is missing.

$getWorkflowStaticData('global') flush latency in queue mode at 2.14.2. I inferred it may be worse in queue mode but cannot verify. Action: benchmark with a write-then-immediate-read test in both default and queue mode.

Error Trigger exact payload schema at 2.14.2. I gave probable field names but they may have changed. Action: trigger a test error, log the raw payload from your existing error workflow, inspect.

POST /api/v1/executions/{id}/retry existence and behavior at 2.14.2. Action: test with a single known-failed execution.

Source control feature coexistence with REST API edits. I inferred they conflict but cannot verify. Action: read the current source control docs carefully, specifically looking for "API" or "programmatic" mentions. Or: enable source control on a test instance, make a REST API edit, and see what happens.

Data Tables maturity, API shape, and consistency guarantees at 2.14.2. Action: create a Data Table, write to it from two concurrent webhook executions, and verify both writes land. Test read-after-write latency.

Instance-level MCP server exact capabilities at 2.14.2. The docs were inconsistent at my cutoff. Action: enable it (instance toggle + one workflow "Available in MCP"), connect with an MCP client, and list the exposed tools/resources.

Schedule trigger catch-up behavior after downtime. I inferred no catch-up but cannot verify. Action: stop n8n for 10 minutes with a 1-minute schedule workflow active, restart, and check if 10 executions appear or just 1.

Wait node durability across restarts. Community reports suggested state loss. Action: create a workflow with a Wait node (wait for webhook), restart n8n, send the resume webhook, and check if the workflow resumes.

Postgres chat memory 11% error rate — actual error messages. I cannot diagnose without the