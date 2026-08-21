---
dr_id: DR26-07-14-HUB-15
title: "Codex CLI as a live Telegram consensus participant alongside Claude Code"
date: 2026-07-14
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-15): Codex CLI as a live Telegram consensus participant alongside Claude Code

> Whether and how OpenAI Codex CLI can function as a live, stateful peer to Claude Code in a shared Telegram consensus chat, covering statefulness, subscription quotas, ToS risk, turn-taking design, and injection/spoofing defenses.

## Ключевые выводы
- Codex CLI is stateful enough for supervised automation but not yet clean enough to be the sole source of truth: `codex exec resume --last/<SESSION_ID>` and SDK durable threads work, but programmatic session-ID capture, promptless resume, and headless forking remain open gaps (GitHub feature requests, not shipped).
- The codex app-server (JSON-RPC 2.0 over stdio/Unix sockets) is architecturally superior to stateless `codex exec` loops or transcript replay for live dialogue: it exposes thread/start, thread/resume, thread/fork, turn/start, and streams item/agentMessage/delta + turn/completed events, avoiding the token blow-up of re-injecting full history each turn.
- Consumer ChatGPT Plus/Pro subscriptions impose rolling 5-hour local-message windows that vary sharply by model: Plus gives ~15-90 msgs/5h on Sol (deep reasoning), ~20-110 on Terra, ~50-280 on Luna; Pro 5x and Pro 20x scale these ~5x/~20x; local messages and cloud tasks share the same window, plus additional weekly caps may apply.
- Heavy/high-velocity usage triggers two documented failure patterns beyond simple rate-limiting: a client/server '429 desynchronization' bug where the local CLI reports ample quota (e.g. 96% left) while the server rejects requests with HTTP 429, and community-reported silent downgrades to lighter models ('performance backstab') plus 'high-risk cyber activity' false-positive moderation flags on benign local dev tasks (restarting processes, querying SQLite) that revoke access to frontier Codex models [single-source/speculative].
- OpenAI's own product surface explicitly supports non-interactive automation (codex exec for CI/scripts, SDK, app-server, and Business/Enterprise 'access tokens' for trusted scripts/schedulers) while the consumer Terms of Use prohibit 'automatically or programmatically extracting data or Output' and bypassing rate limits — no official page blesses unattended bot-to-bot orchestration on a personal Plus/Pro subscription, making consumer automation a policy gray zone versus clearly-sanctioned workspace automation.
- Free-form multi-agent chat (AutoGen-style) is a proven anti-pattern: recent 2025-2026 literature (Debate or Vote; The Cost of Consensus; anonymization/identity-bias studies; problem-drift studies) finds most gains attributed to multi-agent debate actually come from majority voting or isolated self-correction, and open debate suffers conformity, sycophantic 'mutual flattery' loops, identity bias, and conversational drift away from the original question.
- Structured, typed artifact exchange outperforms prose dialogue: patterns like PACT (Action/State Passing) and DCI (Debate-Deliberate-Decide) that pass only grounded facts/diffs/verdicts (not private chain-of-thought) show better token efficiency and resistance to premature/false consensus than free-form or even role-based pipeline (CrewAI-style) approaches.
- Industry practice converges on 'single-writer plus independent reviewer/critic' rather than open peer meshes: Anthropic's multi-agent research system benefits mainly on breadth-first research tasks (reports 90.2% improvement over single-agent baseline) but notes coding is less parallelizable; Cognition moved from 'Don't Build Multi-Agents' (2025) to a 2026 position endorsing multi-agent intelligence only when writes stay single-threaded, to avoid parallel/implicit decision conflicts.
- Telegram's Bot API provides attribution metadata (from, sender_chat, via_bot, author_signature) but no cryptographic proof of authorship, making identity spoofing (e.g. faking a '[Codex]:' prefix) trivial; the recommended fix is an Ed25519-signed message envelope (agent_id, vendor, host_id, task_id, phase, seq, ts, body_sha256, signature) verified against a local public-key allowlist before any message can influence execution, since shared-secret HMAC is unsafe in a heterogeneous fleet (a compromised agent could extract the secret and forge messages as another agent).
- Prompt injection is treated as structurally unmitigable rather than a prompting bug (OWASP LLM01:2025, UK NCSC 'prompt injection is not SQL injection', OpenAI's own guidance to constrain impact not detect attacks); a specific documented multi-agent risk is temporally decoupled memory poisoning (the MINJA attack) where malicious instructions planted via a compromised document/web fetch persist in long-term memory across sessions and get executed later, and intermediate 'trusted' agents can inadvertently launder/reformat injected payloads as they relay them downstream to a more privileged actor agent.

## Рекомендации / решения
- Adopt a 'Hybrid Signed Artifact Chat' architecture: Telegram stays a human-visible summary/objection layer only; all execution-relevant content must pass as signed, typed artifacts (proposal/objection/verdict JSON) on the existing synced file bus/consensus engine, never as trusted free prose.
- For Codex continuity, persist a small local ledger per task (codex_thread_id, artifact_state_hash, last_human_visible_summary, last usage snapshot) and prefer `codex exec resume <SESSION_ID>` or app-server thread/resume as a performance optimization, rebuilding from the state file (not the Telegram transcript) when resume fails or is stale.
- Migrate the Codex control plane from bare CLI child-process calls to the codex app-server or SDK for deterministic thread lifecycle, event streaming, and rate-limit reads; if unattended/24-7 operation matters, move Codex auth from a personal ChatGPT subscription to API-key auth or Business/Enterprise access tokens rather than relying on consumer ToS ambiguity.
- Enforce a rigid turn-taking envelope (e.g. PROPOSE_PLAN → COUNTERARGUMENT → REVISED_PLAN → VERIFY → COMMIT_READY, or PACT/DCI-style typed epistemic moves: propose/challenge/bridge/synthesize) with a non-LLM stop predicate (hard round cap, e.g. max_rounds=3, or timeout) and automatic escalation to human approval on unresolved disagreement, rather than allowing open-ended debate.
- Add quota-aware pacing: read rate-limit/token-usage state (`/status`, app-server thread/tokenUsage/updated, account/rateLimits/read) before launching turns, budget reviewer roles to lighter model tiers, and trap HTTP 429 / JSON-RPC -32001 errors with fallback routing (e.g. a metered API-compatible gateway) rather than looping retries against a desynchronized client-side quota counter.
- Require every actionable inter-agent message to carry an Ed25519-signed envelope validated against a pinned local keyring; drop unsigned or invalid-signature messages before they ever enter an agent's LLM context, and keep the Telegram bus treated as untrusted data by default (isolate 'chat content' from 'approved task spec').

## Сущности
- **Люди:** —
- **Компании:** OpenAI, Anthropic, Cognition, AWS, Google, OWASP, UK NCSC
- **Продукты/инструменты:** Codex CLI, codex exec, Codex app-server, Codex SDK, MCP (Model Context Protocol), Claude Code, Telegram Bot API, AutoGen, CrewAI, Symphony (OpenAI), AWS CLI Agent Orchestrator, Agent Orchestrator (AgentWrapper), Ed25519, Minisign, Sigstore, HMAC-SHA256, GPT-5.6 Sol/Terra/Luna, GPT-5.3-Codex, GPT-5.2, Ofox.ai (API gateway), OpenClaw, MINJA (memory poisoning attack)

## Открытые вопросы
- No official OpenAI page explicitly authorizes or prohibits unattended bot-to-bot orchestration on a consumer ChatGPT Plus/Pro subscription — the policy remains a documented gray zone [no source found].
- Exact current Codex model context-window ceilings for subscription-auth CLI use were not found in official material [no source found].
- Reports of account bans/suspensions and 'high-risk cyber activity' false-positive flags for heavy/automated Codex usage remain anecdotal/single-source, not a verified enforcement rule.
- Whether the '429 desynchronization' bug (client shows quota remaining but server rejects with 429) has an official acknowledgement/fix from OpenAI, or is purely community-documented, is unresolved.
- The gemini vendor section was truncated in the source report before its Strategic Recommendations/Action Plan/Sources sections could be captured, so its full source list and final synthesis were not available for this distillation.

## Источник
- DR-ID `DR26-07-14-HUB-15` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-14-HUB-15-codex-consensus-chat-chatgpt.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-14-HUB-15-codex-consensus-chat-gemini.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-14-HUB-15-codex-consensus-chat-grok.md`

## Связано
- [[codex-app-server]]
- [[multi-agent-debate-vs-voting]]
- [[prompt-injection-defense]]
- [[ed25519-message-signing]]
- [[consensus-engine-artifact-exchange]]
- [[openai-subscription-quotas]]
- [[machine-bus-telegram-rail]]
- [[single-writer-review-pattern]]
