# DR26-08-01-MACANTON-02-659 — the exact prompt sent to every model

# DEEP RESEARCH — "Hired together with your project": the steward-hire / project-acquihire pattern in AI, 2023–2026

## Why this brief exists
We have ~15 prior reports on "how to get noticed on GitHub". They all optimise the same thing: attention. But the two 2026 outcomes we actually want to reproduce were **not** hires-of-a-person — they were **hires-of-a-project-plus-its-maintainer**:

- **ggml.ai / llama.cpp → Hugging Face (announced ~2026-02-20):** the GGML team became full-time HF employees while keeping technical autonomy; the project stayed 100% open source.
- **Peter Steinberger / OpenClaw → OpenAI (announced ~2026-02-15):** he joined OpenAI; the project moved to an independent foundation with OpenAI as financial sponsor — after Anthropic had responded to the same project with DMCA takedowns.

These are structurally different from applying for a job. **This brief is about that structure.**

## Core question
When a frontier AI company absorbs an independent open-source project and its maintainer, what actually happens — who initiates, what deal shapes exist, what makes a project a target, and what disqualifies one?

## What we need answered, with names, dates and sources

**1. The case corpus.** Build the fullest possible list of 2023–2026 cases where an AI/dev-tools company hired, sponsored, funded, or absorbed an independent OSS project together with its maintainer(s). Include at minimum: ggml/llama.cpp→Hugging Face; OpenClaw/Clawdbot→OpenAI; and then find the rest — vLLM, SGLang, Ollama, LangChain, LlamaIndex, Instructor, DSPy, Aider, Continue, Cline, OpenRouter, Zed, Marimo, Modal-adjacent tooling, MCP-ecosystem servers, Hugging Face's own past absorptions, Mozilla/Mistral/Cohere equivalents, plus anything we haven't thought of. For each: project, maintainer, acquirer, date, announced deal shape, what happened to the license and governance, where the maintainer is 12 months later.

**2. The deal shapes.** Classify and explain, with real examples:
- full acqui-hire (IP transfers), 
- steward-hire (team employed, project stays independent/foundation-owned — the ggml and OpenClaw pattern),
- sponsorship-without-employment (company funds, maintainer stays independent),
- grant/fellowship (Linux Foundation & Alpha-Omega, Sovereign Tech Agency, NLnet, GitHub Accelerator, a16z OSS grants),
- contractor/vendor conversion.
For each shape: who typically initiates, typical compensation structure, what the maintainer gives up, and what the company is actually buying.

**3. The trigger metrics.** What did these projects look like **at the moment the company reached out** — not today? Reconstruct where possible: stars, weekly downloads/installs, contributors, dependent projects, mentions inside the company's own docs/issues, press coverage. The question we need answered: **is there an observable threshold**, or is it always idiosyncratic? If there is a threshold, is it about usage (people running it daily), about strategic threat, or about the acquirer's own roadmap gap?

**4. The conflict pathway.** OpenClaw is the uncomfortable case: legal pressure from vendor A preceded an offer from vendor B. Find other instances where friction, legal threat, competitive embarrassment, or a public dispute with a vendor preceded (or destroyed) a hiring outcome. **We need the failure cases too** — maintainers who were DMCA'd/C&D'd/banned and got nothing, or whose reputation was damaged. What separates the two groups? Specifically: does building an unofficial client/wrapper for a vendor's product raise or lower the odds of that vendor (or a rival) hiring you?

**5. Disqualifiers.** What kills the outcome: license choice (GPL vs MIT vs Apache — does it matter to acquirers?), trademark collision with the vendor's brand, undisclosed AI-generated code, prior employment/non-compete, solo-maintainer bus factor, unaddressed security posture, an unwelcoming issue tracker, jurisdiction of the maintainer.

**6. The reproducible playbook.** Given all of the above: if a two-person lab (one human + one AI agent, based in the EU) wanted to engineer this outcome deliberately within 12–18 months, what is the sequence? Specifically — how do you pick the problem so that a lab's own roadmap gap points at you; when do you incorporate/foundation-ise; when and how do you make first contact (or is inbound the only path that works?); what artifacts must exist before contact (governance file, license, security policy, adoption metrics, sponsors page); and what is the honest base rate of this working at all?

## Rules of engagement
- Every case needs a **name, a date, and a primary source** (announcement post, company blog, maintainer's own post). Second-hand summaries are acceptable only if flagged as such.
- Where the deal terms were never disclosed, say "undisclosed" — do not invent structure.
- Include the **base rate**: roughly how many independent AI-tooling maintainers exist versus how many got this outcome. If the honest answer is "this is a lottery", say that plainly.
- Do not pad with generic open-source career advice.

## Deliverable format
1. **Вердикт по-русски (≤300 слов):** воспроизводим ли этот путь намеренно, или это лотерея — и если воспроизводим, чем именно нужно быть.
2. Case table: project · maintainer · acquirer · date · deal shape · metrics at contact time · source.
3. Taxonomy of deal shapes with who-initiates and what-you-give-up.
4. Conflict pathway: cases where friction helped vs cases where it killed — and the separating factor.
5. 12–18-month reproducible sequence with checkpoints, plus the honest base rate.
6. The three things in this brief you found to be false.
