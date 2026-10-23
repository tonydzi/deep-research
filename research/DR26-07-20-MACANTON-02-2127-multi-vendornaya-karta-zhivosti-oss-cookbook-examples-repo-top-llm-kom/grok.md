# DR26-07-20-MACANTON-02-2127 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-20-MACANTON-02-2127 — Grok (SuperGrok Heavy) отчёт

- source: https://приватный чат
- collected: 2026-07-21 (Mac16, сессия eager-swirles, забрано Claude через Chrome)
- thought: 2m19s, 416 sources
- verbatim ниже (текст страницы, таблицы линеаризованы get_page_text)

---

Verified and extended liveness map (as of 2026-07-21). User's HIGH/MED-HIGH/SKIP map is largely accurate, with targeted corrections based on GitHub API scans of closed/merged PRs (last ~90–180 days), CONTRIBUTING.md files, release notes, open issues, and community signals. Evidence prioritizes recent activity (Apr–Jul 2026). Copybara-style internal landing of closed external PRs is primarily a Google core-repo pattern (e.g., adk-python); community-facing repos more often show direct merges or explicit rejection.

## (1) Per-repo scorecard

**google/adk-python-community** — Agree HIGH. Explicit community plugins/integrations/services repo. Proven external merges: #141 AgentGovernancePlugin (imran-siddique, May 2026), #110 HITL approval gateway (garythomasgeorge, Jun 2026), #151 pluggable taxonomy/policy (ViktorVeselov), #115 S3ArtifactService (miyannishar). Releases credit new external authors. Gatekeepers: Community Module Owners / Approvers / Repo Maintainers (role-based). Wants now: MCP security/guardrails (prompt injection, SSRF, elicitation — open in sibling adk-python), tool-call governance, multi-agent patterns, memory/services integrations, HITL. Strong match for authority routing, MCP tooling/signing, adversarial verification. Norms: Google CLA (once covers multiple projects); issue-associated for non-trivial; small focused PRs; unit + E2E evidence (logs/screenshots); Conventional Commits, autoformat.

**huggingface/cookbook** — Agree HIGH. Community-driven practical notebooks. Issue #303 "search agent cookbook" still open. Gatekeepers: @merveenoyan, @stevhliu (tag in every PR). Wants: search agents, multi-agent/RAG patterns, practical open-source end-to-end examples. Norms: practical, open-source tools/models only, runnable error-free, clear neutral writing, add to _toctree.yml + index.md, lowercase filename, authorship line after first header, images to huggingface/cookbook-images dataset. Tag gatekeepers. Check open issues/PRs first.

**microsoft/semantic-kernel** — Agree MED-HIGH (issue-first, not pure cookbook). Active. External merges for bugs/docs/samples; features require prior discussion. Open issues on memory-poisoning guards / function-invocation approval confirmed. Plugins/memory connectors deliberately NOT hosted in core — host separately and link. Norms: Microsoft CLA; issue-first ("DON'T surprise us with big PRs"); tests required; Black for Python; breaking changes rejected without discussion.

**groq/groq-api-cookbook** — ⚠ DISAGREE / correct to MED. Rubric exists and historically enabled fast merges, but 2026 closed PRs are almost all unmerged; last merges clustered ~Sep–Nov 2025. Current reality slower/backlogged. Rubric (Usefulness/Originality/Clarity/Accuracy/Depth/Grammar, 1–4; <3 any axis risks reject).

**openai/openai-agents-python (+ -js)** — Agree MED-HIGH → ⬆ HIGH. Extremely active; 300+ contributors; external example/docs PRs merge regularly (Syed-Umer-Ali runnable fixes, hsusul visualization, Jul 2026). Issue #1097 (-js) exactly matches approval-with-modification. Gatekeeper: @seratch. Wants: HITL / approve-with-modified-args, multi-agent handoffs, guardrails, MCP tools, sessions/memory, sandbox/tracing examples. Norms: tests, conventional commits, changesets (-js), PR template.

**xai-org/xai-cookbook** — Agree MED-HIGH. Notebook guidelines; some 2026 merges; backlog of closed-unmerged externals real. Norms: notebooks in examples/, E2E runnable, minimize deps (%pip), update registry.yaml, unique value, PR title prefixes ([Content]/[Improvement]/[BugFix]), open issue first if unsure.

**google-gemini/cookbook** — Agree MED-HIGH. Issue-first, pedagogical, rejects heavy third-party pip deps (prefer official google-genai). Wants: agent trust-layer/middleware recipes (#1268), MCP recipes (#1271), SQL agents. Norms: Google CLA, Template.ipynb, nbfmt/nblint, Colab badge + live Colab link in PR, official SDKs, small single-purpose notebooks.

**google-gemini/genai-processors** — Agree MED-HIGH (higher for pipelines). contrib/ folder for community processors + tests/docs. Wants: composable/parallel processors, measured concurrency patterns.

**togethercomputer/together-cookbook** — Agree MED-HIGH. PRs or Discord; recent activity Jul 2026.

**mistralai/cookbook** — Agree MED (caution: backlog). Welcoming language but high closed-unmerged volume → poor ROI vs higher-velocity targets.

**⭐ microsoft/agent-framework (missed door / elevated)** — HIGH potential (new primary). Active Jul 2026, 12k+ stars, multi-agent workflows, memory providers, HITL, 197+ contributors. Explicit successor to Autogen + SK. Norms как SK: CLA, issue-first, tests. Samples repo exists.

**⭐ meta-llama/PurpleLlama (missed door)** — MED-HIGH for security/evals. Active Jul 2026 (4.3k stars). Wants: guardrails, safety classifiers, evals, prompt-attack detection, multimodal safety. Security-focused PRs favored. → Meta НЕ мёртв целиком, дверь = security/evals.

**⭐ genkit-ai/genkit (+ samples) (missed)** — MED-HIGH. Active (6k+ stars). Plugin/samples authoring guide.

**NVIDIA/GenerativeAIExamples** — MED-HIGH. Explicit invites, community/ folder, DCO required (git commit -s).

**Perplexity api-cookbook / AWS bedrock+agentcore samples / Alibaba** — MED. Perplexity активен с CONTRIBUTING; AWS structured community-friendly; Alibaba тонко (Java-heavy).

## (2) Ranked TOP-10 (merge probability)
1. openai-agents-python/-js: authority routing + approve-with-modification поверх #1097 — **75–85%**
2. adk-python-community: MCP tool-call signing / governance plugin + authority routing/adversarial verify — **70–80%**
3. microsoft/agent-framework (или SK samples через issue): persistent memory + memory-poisoning guards — **65–75%**
4. huggingface/cookbook: search-agent / consensus+adversarial verify под #303, тегать @merveenoyan @stevhliu — **65–75%**
5. google-gemini/cookbook: trust-layer/guardrails middleware (#1268) или MCP (#1271) + citation-faithfulness; issue-first — **55–70%**
6. genai-processors contrib/: pipeline-vs-barrier processors с замерами — **60–70%**
7. meta-llama/PurpleLlama: citation-faithfulness eval / reasoning-quality gates как security/eval тулы — **55–65%**
8. groq-api-cookbook: high-rubric single-purpose notebook — **50–65%** (лимитер = текущий merge rate)
9. xai-cookbook или together-cookbook: consensus/verification или tool-signing notebook — **45–60%**
10. NVIDIA GenerativeAIExamples / AWS / genkit samples: fleet ops или memory patterns — **50–60%**

## (3) Do-not-bother
autogen (maintenance mode) · llama-cookbook (стоит с ~Nov 2025) · DeepSeek (нет кукбука) · Cohere (архив; staff-only) · Qwen-Agent (external PRs incl. security closed unmerged) · mistralai/cookbook депприоритет · pure research DeepMind / core ADK (Copybara, тяжёлый процесс).

## Cross-cutting norms that kill PRs
Missing CLA (Google/MS) или DCO (NVIDIA) · пропуск issue-discussion в issue-first культурах · third-party deps (особенно Gemini) · non-runnable / нет Colab / нет тестов · широкий multi-concern PR · низкая оригинальность / промо-тон / (Groq) низкий rubric score.

## Timing/optics
Small single-purpose notebooks / focused plugins > multi-file demos везде. Mid-week. Live runnable evidence (Colab links, measured outputs), честный AI-authorship дисклоз, кредит авторам issues. Google CLA — одна подпись на все проекты.
