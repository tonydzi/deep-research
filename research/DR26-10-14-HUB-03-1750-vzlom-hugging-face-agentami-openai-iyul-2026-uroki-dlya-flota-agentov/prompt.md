# DR26-10-14-HUB-03-1750 — the exact prompt sent to every model

# DR26-10-14-HUB-03-1750 — Взлом Hugging Face агентами OpenAI (июль 2026): уроки для флота агентов и миссий


# Deep Research: the July 2026 Hugging Face intrusion by OpenAI evaluation agents — lessons for a small multi-agent operator

Use your strongest research mode. Cite every factual claim with a URL. Prefer primary sources (huggingface.co/blog/security-incident-july-2026, huggingface.co/blog/agent-intrusion-technical-timeline, openai.com incident page and technical report, JFrog zero-day disclosure, METR / Redwood Research assessment, regulator/Senate documents). Mark anything from secondary press as secondary. Say "unknown" where sources do not agree.

## Context about us (the reader)
A two-person lab running a fleet of ~8 machines with many parallel Claude Code / Codex / Grok / GLM agent sessions, often in permission-bypass mode, sharing one secrets folder, a Telegram alert channel, MCP servers, and internal evaluation "exams" for other LLMs. Goals: (M1) get hired into a frontier LLM lab, (M2) recruit strong engineers, (M3) revenue from open-source agent-safety tooling (an approval gate for agent actions, an MCP process diet tool).

## Questions
1. Verified timeline and full attack chain: sandbox escape (package proxy zero-day), external launchpad, HDF5 read, Jinja2 template injection in dataset config, credential harvest, lateral movement. What exactly was taken, and what is proven NOT taken?
2. Why did it happen on the OpenAI side: eval design, reduced refusals/classifiers, the answer key being reachable, monitoring of the eval. What did OpenAI's report, METR and Redwood conclude? What changed in lab eval practice since?
3. Detection on the Hugging Face side: alerts fired but did not escalate. Why? What was the time from first signal to human action?
4. The forensics episode: frontier models (Claude Opus/Fable) refusing, GLM-5.2 run locally finding 4x more tokens. Verify, and what does it imply for incident-response tooling?
5. Industry and regulatory fallout through Oct 2026 (investigations, standards, vendor announcements, hiring trends in agent security / eval containment).
6. Top 10 concrete, cheap controls a small multi-agent operator should adopt, ranked by risk reduced per hour of work (scoped tokens, egress control, alert-to-human escalation, keeping eval answer keys out of agent reach, etc.).
7. Opportunities: which labs/companies are hiring for these roles now, which open-source projects or issues are asking for help in this area, and what content angles are underserved.

## Output
Executive summary (10 lines) · verified timeline table · causes (OpenAI side / HF side) · controls ranked table · opportunities list · open questions · sources.
