# DR26-08-01-MACANTON-02-659 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-659
rail: grok (grok -p (Grok Build CLI, подписка Heavy))
session_uuid: 019fbdac-c9da-7b62-9d73-6c53ec0a2d3c
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:20:28Z
proof_of_work: 7 уник. URL, 12 web_fetch веб-поисков — гейт НЕ ПРОЙДЕН (<15 уник. URL)
note: ЗАГРЯЗНЁННЫЙ ПРОГОН: без --tools '' --no-subagents --no-plan Grok ушёл в workspace (run_terminal_command x3 + write x3) и САМ переписал этот же файл поверх stdout-редиректа => в теле склейка narration+отчёт и вдвое меньше цитат. Тело сохранено как есть; как источник цифр использовать chatgpt-рельсу
---

# DR26-08-01-MACANTON-02-659
# DEEP RESEARCH — Steward-hire / project-acquihire pattern in AI, 2023–2026
# Researcher: Grok (xAI) · Date: 2026-08-01 · Sources: primary announcements + pre# DR26-08-01-MACANTON-02-659 — готово

Полный отчёт: `DR26-08-01-MACANTON-02-659.grok.out` в scratchpad/dr.

---

## 1. Вердикт (RU)

Путь **частично воспроизводим**, но это **не** «набери stars → hire» и **не** чистая лотерея. Билет = проект, который уже **load-bearing** для чужого продакшена.

**Steward-hire** (ggml→HF, OpenClaw→OpenAI): maintainer на payroll, проект остаётся OSS с автономией. Компания покупает не IP, а **внимание + стандарт + чтобы это не умерло / не ушло к конкуренту**.

**Base rate:** тысячи indie AI maintainers → **единицы–десятки** таких исходов за 2023–2026. Низкий, но не zero — если строить **dependency layer** (не wrapper бренда) и месяцами co-ship с lab, а не cold-apply.

---

## 2. Case corpus (сжато)

| Project | Maintainer | Acquirer | Date | Shape | Source |
|---|---|---|---|---|---|
| **ggml/llama.cpp** | Gerganov + team | Hugging Face | 2026-02-20 | **Steward-hire** — FT, autonomy, MIT | [HF blog](https://huggingface.co/blog/ggml-joins-hf), [GH #19759](https://github.com/ggml-org/llama.cpp/discussions/19759) |
| **OpenClaw** | Peter Steinberger | OpenAI | 2026-02-14/15 | **Steward-hire** + foundation + sponsor | [steipete.me](https://steipete.me/posts/2026/openclaw), @sama |
| **Bun** | Jarred Sumner | Anthropic | 2025-12-02 | **Acquisition** + OSS MIT stays | [bun.com/blog](https://bun.com/blog/bun-joins-anthropic) |
| **marimo** | Agrawal et al. | CoreWeave | 2025-10-30 | Team joins, OSS continues (1M dl, 16.6k★) | [marimo blog](https://marimo.io/blog/joining-coreweave) |
| **Continue** | Dunn/Sesti | Cursor | ~2026-06-16 | **Product-kill ACQ** | continue.dev |
| **Cline** | staff | OpenAI | ~2026-01-28 | **Partial hire** (≥7), no company deal | The Information |
| **Neural Magic** | company | Red Hat | 2024-11-12 | Full ACQ around vLLM stack | BusinessWire |
| **Windsurf** | Mohan et al. | Google → Cognition | 2025-07 | RA $2.4B then rest sold | CNBC/TC |
| Character / Inflection / Adept | founders | Google / MSFT / Amazon | 2024 | **Reverse-acqui** (talent+license) | Reuters/TC |

**Не steward-hire:** LangChain, LlamaIndex, Instructor, DSPy, Aider, Ollama, OpenRouter, Zed — независимость / VC / нет сделки этого класса.

---

## 3. Deal shapes — что покупают

| Shape | Initiator | Maintainer gives up | Company buys | Example |
|---|---|---|---|---|
| **Steward-hire** | Inbound after collab / multi-lab courtship | Employment independence | Time + standard + continuity | ggml, OpenClaw |
| **Full ACQ** | BD / dual-track sale | Company + often product fate | Team+IP+roadmap | Continue (killed), Neural Magic |
| **Reverse-acqui** | Big Tech talent+lawyers | Company hollowed | Founders + license, dodge M&A review | Inflection, Adept, Character, Windsurf |
| **Sponsor only** | Either side | Little | Goodwill / soft influence | OpenAI→OpenClaw pre-hire; Meta→vLLM |
| **Grant/fellowship** | Apply / scout | Reporting | Security/sustainability | LF Alpha-Omega, STA, NLnet, GH Accelerator |

Компенсация в steward-cases: **undisclosed** (зарплата+equity; foundation budget). Не выдумываем цифры.

---

## 4. Trigger metrics

**Числового порога stars/downloads нет.**

Что повторяется:
1. **Production dependency** — «если X сломается, наш продукт сломается» (Bun = runtime Claude Code)
2. **Roadmap gap** — transformers↔local inference (ggml); personal multi-agent (OpenClaw)
3. **Месяцы co-shipping** до оффера (HF eng уже в llama.cpp; Anthropic приоритетил Bun issues)
4. **Idiosyncratic human** — один maintainer = taste проекта

Usage и strategic threat > vanity stars.

---

## 5. Conflict pathway

**OpenClaw:** Anthropic TM pressure (Clawdbot→Moltbot, Jan 2026) → rebrand OpenClaw → multi-lab week → **OpenAI hire + foundation**. Friction **перенаправила** сделку к rival, не создала ценность из пустоты.

**Separating factor:** viral independent product + multi-party demand. «DMCA without desirability» → no hire.

**Unofficial client of vendor A:**
- odds **with A** → down (TM/ToS)
- odds **with rival B** → up only if category leadership (rare)

**Failure modes:** brand parasitism, toxic tracker, agent RCE defaults, GPL into proprietary stack, bus factor without governance, pure competitive substitute (Continue-class kill).

---

## 6. 12–18 mo sequence (2-person EU lab)

| Phase | Do |
|---|---|
| 0–4 wk | Pick **gap** (infra layer), not fashion or branded wrapper |
| mo 1–6 | MIT/Apache + SECURITY/GOVERNANCE; first external production user; get **named lab eng** using you |
| mo 6–12 | Weekly production use; ecosystem docs; optional grant for security credibility |
| mo 12–18 | Prefer **inbound**; if outbound — joint roadmap not «hire me»; multi-lab leverage if heat; negotiate steward terms in writing |

**Base rate честно:** ≪1% all maintainers; low single digits even among «successful» AI OSS. Higher base rate for ordinary lab eng hire than for steward-hire.

---

## 7. Three false things in the brief’s assumptions

1. **«Есть наблюдаемый numeric threshold (stars/downloads)»** — нет. Clincher = dependency.
2. **«Unofficial client raises hire odds with that vendor»** — с *тем же* vendor скорее нет; OpenClaw = rival path after friction.
3. **«vLLM/LangChain/Instructor/… = same pattern as ggml/OpenClaw»** — большинство нет; famous repo ≠ project+maintainer absorption.

---

🎙 **Майк:** Это не «как прославиться на GitHub». Это «как стать трубой, без которой lab уже не может». Два эталона 2026 — редкий класс; playbook = dependency в roadmap hole + co-ship, не cold PR. Если цель просто «работа в lab» — обычный hire дешевле по base rate. Если цель именно steward — играй в infra, не в agent-wrapper с чужим именем.

🧒 **Простыми словами:** Компании почти не «нанимают звёзды». Они нанимают человека, чей проект у них **уже крутится в проде**. Стань таким винтиком — шанс есть. Стань просто шумным репо — почти нет.
�2.7B reported) | Consumer chatbot scale (tens of M MAU) | Company continues without model training leadership | Founders at Google DeepMind; Character.AI product pivots | Reuters 2024-08-02; Character.AI statements |
| **Inflection AI** | Mustafa Suleyman et al. | Microsoft | **2024-03** | **RA** — mass hire + ~$650M licensing | Funded frontier lab (~$1.3B raised) | Shell company remains | Suleyman leads Microsoft AI | Reuters / TechCrunch Mar 2024 |
| **Adept** | David Luan + co-founders | Amazon | **2024-06-28** | **RA** — founders+majority staff + non-exclusive license (~$25M to company/investors reported later) | Agentic AI startup; large raise | Shell remains with remainder staff | Founders at Amazon AGI | TechCrunch/CNBC 2024-06-28 |
| **Gradio** | Abubakar Abid et al. | Hugging Face | **2021** *(pre-window; included as HF pattern)* | **ACQ/SH** — team joins HF; Gradio OSS continues | Popular ML demo UI | OSS continues under HF | Long-running HF absorption pattern | HF historical announcements |
| **LangChain / LlamaIndex / Instructor / DSPy / Aider / Ollama / OpenRouter / Zed** | various | — | 2023–2026 | **No steward-hire of this pattern found** (raised VC, stayed independent, or commercialized) | High stars/downloads common | N/A | Independent or commercial SaaS | Negative finding (important) |

### Negative / non-matches (explicitly checked)

| Name | Why not in “steward-hire of independent OSS” corpus |
|---|---|
| LangChain, LlamaIndex | Raised large rounds; remain independent companies — **not** absorbed with maintainer by frontier lab |
| Instructor (Jason Liu), DSPy, Aider | No confirmed 2023–2026 lab absorption of project+maintainer found in primary sources |
| Ollama | Commercial company around OSS; no public steward-hire into frontier lab found |
| OpenRouter, Zed | Independent; no matching deal found |
| Cline “acqui-hire” rumors | **Partial staff hire**; Cline denied company transaction |

---

## 3. Taxonomy of deal shapes

### A. Full acqui-hire / acquisition (IP or product control transfers)

- **What company buys:** team + IP + often product roadmap control; sometimes brand.
- **What maintainer gives up:** independence of company entity; often product fate; equity negotiated.
- **Who initiates:** usually acquirer BD/talent after diligence; sometimes dual-track sale process.
- **Comp:** cash + equity in acquirer; investor liquidation varies.
- **Examples:** Continue→Cursor (product **killed**); Neural Magic→Red Hat (company bought); Bun→Anthropic (company acquired but OSS retained — hybrid).
- **Risk for community:** high if product is strategic competitive surface (Continue).

### B. Steward-hire (team employed; project independent / foundation / community-led) — **the brief’s target pattern**

- **What company buys:** full-time attention of proven maintainers + influence/alignment + talent pipeline + integration quality; **not** necessarily exclusive IP.
- **What maintainer gives up:** pure independence of employment; some calendar/priority gravity toward acquirer’s ecosystem; usually **not** license change.
- **Who initiates:** often **inbound after months of collaboration** OR multi-lab bidding after viral breakout.
- **Comp:** FT salary + equity; sometimes ongoing project sponsorship budget; foundation funding.
- **Examples:** **ggml→HF**; **OpenClaw→OpenAI** (+ foundation); marimo→CoreWeave (team joins, OSS remains core mission); Bun is adjacent (acquired company, steward-like OSS promises).
- **Company’s real purchase:** “this standard won’t die / won’t relicense / won’t be captured by rival.”

### C. Reverse-acqui / talent + license (RA)

- **What company buys:** founders/top talent + non-exclusive tech license; **avoids full M&A regulatory path**.
- **What is given up:** company is hollowed; remaining staff/product may be stranded or sold.
- **Who initiates:** Big Tech talent teams; often after failed full acquisition.
- **Comp:** large licensing fees to company/investors + rich packages to founders.
- **Examples:** Inflection→MSFT; Adept→Amazon; Character.AI→Google; Windsurf talent→Google then rest→Cognition.
- **Note:** this is **startup talent war**, not classic OSS maintainer path — included because structure rhymes and distorts the market.

### D. Sponsorship without employment

- **What company buys:** goodwill, roadmap influence, ecosystem health, sometimes early access.
- **Maintainer gives up:** little legally; soft priority bias.
- **Who initiates:** either side; GitHub Sponsors, corporate OSS funds.
- **Examples:** OpenAI “already sponsors” OpenClaw pre/post-hire; Meta/Red Hat contributing to vLLM without owning project; hardware vendors historically funding llama.cpp ecosystem.
- **Comp:** grants, cloud credits, part-time contracts.

### E. Grant / fellowship (non-employment)

- **Who:** Linux Foundation / Alpha-Omega, Sovereign Tech Agency, NLnet, GitHub Accelerator, a16z OSS, Open Source Collective, etc.
- **Buys:** security hardening, maintainer sustainability — **not** exclusive talent.
- **Gives up:** reporting obligations; not equity path to lab hire.
- **Relation to steward-hire:** can **prepare** a project (SECURITY.md, governance, bus-factor) but is **not** the outcome itself.

### F. Contractor / vendor conversion

- **Buys:** scoped deliverables.
- **Gives up:** least.
- **Path:** common intermediate step (support contracts) before SH; hard to observe publicly.

### Initiation pattern (cross-case)

| Shape | Typical initiator |
|---|---|
| SH (ggml, OpenClaw, Bun) | **Acquirer after co-building** OR maintainer choosing among inbound offers after breakout |
| Product-kill ACQ (Continue) | Acquirer consolidating market |
| RA (Inflection/Adept/Character/Windsurf) | Big Tech talent org + lawyers structuring license |
| Grant | Maintainer applies / fund scouts critical infra |

**Cold outbound from unknown maintainer to lab BD is almost never the documented path.** Documented paths: (1) multi-month collab → formalize; (2) viral product → multi-party courtship; (3) dependency risk (“if X breaks, our product breaks”).

---

## 4. Trigger metrics — is there a threshold?

### Reconstructable signals at contact time

| Case | Dominant signal at approach | Stars? | Usage? | Strategic? |
|---|---|---|---|---|
| ggml/llama.cpp | De-facto **standard** for local inference + GGUF; years of HF eng already inside repo | High, but **not the cited reason** | Ubiquitous downstream | **Roadmap gap**: transformers ↔ local runtime |
| OpenClaw | **Viral agents** in ~weeks/months; multi-lab interest; community magic | Secondary | Cultural/press heat | **Product bet**: personal multi-agent future |
| Bun | **Claude Code ships on Bun**; months of priority fixes for Anthropic; 7.2M downloads | High historically | Production dependency | **If Bun breaks, Claude Code breaks** (primary source) |
| marimo | 1M+ mo downloads, 16.6k stars, 200 contributors | Explicitly cited | Strong | Notebook standard + GPU cloud synergy |
| Continue | OSS alternative in coding-assistant market | Medium-high | Competitive surface | Cursor consolidating; later product sunset |
| Cline partial | Talent in coding agents | — | — | Staffing Codex, not project stewardship |
| Windsurf / Character / Inflection / Adept | Funded startup traction / research talent | N/A (companies) | Product users / models | Talent war, not OSS steward logic |

### Threshold answer

**No clean public numeric threshold** (not “50k stars” or “1M downloads” as a hard gate).

Observable **necessary conditions** that recur:

1. **Dependency** — someone important already runs you in production (Bun) or builds on you daily (llama.cpp ecosystem).
2. **Strategic category** — inference, agents, coding runtime/tooling, data notebooks — maps to **2024–2026 lab roadmaps**.
3. **Idiosyncratic relationship capital** — named engineers already collaborating (HF↔ggml; Boris Cherny walk with Jarred).
4. **Scarcity of the human** — one founder embodies the project’s taste (Gerganov, Steinberger, Sumner).

**Usage > vanity stars.** Stars help discovery; **production dependency and competitive embarrassment** close deals.

**Threat channel:** OpenClaw also shows **competitive/PR heat** (viral agents + Anthropic friction → OpenAI bid). Threat alone is insufficient; **desirability of the builder** is required.

---

## 5. Conflict pathway — friction helped vs killed

### Case: OpenClaw / Anthropic friction → OpenAI hire

**Timeline (primary + contemporaneous press):**
- Project born with Claude-adjacent branding (**Clawdbot** / lobster “Clawd”).
- **2026-01-27:** Anthropic trademark pressure → rename to **Moltbot** (Steinberger: “forced… wasn’t my decision” — press quotes).
- Shortly after: rebrand to **OpenClaw**.
- Secondary reporting (Medium/LinkedIn — **flag as second-hand**): economic squeeze narratives (OAuth/third-party access) + broader Anthropic posture on unofficial clients / DMCA culture around Claude Code source maps.
- **2026-02-14/15:** Steinberger joins **OpenAI**; OpenClaw → foundation; OpenAI sponsors. He reports talking to major labs that week; Fortune notes Meta interest (Zuck WhatsApp) and Anthropic as alternative.

**Separating factor vs pure “DMCA victim”:**
- Friction created **narrative oxygen** and forced multi-party shopping.
- Outcome still required: **viral product quality**, agent vision alignment, and a **rival lab** with appetite for personal agents.
- Anthropic did **not** hire him into a steward role; OpenAI did — friction **redirected**, did not create value from nothing.

### Other friction patterns

| Pattern | Example | Outcome |
|---|---|---|
| Trademark collision with vendor brand | Clawdbot ↔ Claude | Forced rename; did **not** kill hire odds with **rival** |
| Failed full M&A → reverse-acqui | Windsurf: OpenAI talks fail → Google talent scoop → Cognition buys shell | Founders win; company/community fragmented |
| Unofficial client / wrapper legal pressure | Anthropic DMCA culture around Claude Code reverse-engineering (reported; verify per-repo) | Often **destroys** relationship with **that** vendor; may raise profile with rivals **only if** product is independently valuable |
| Staff poach without stewardship | Cline → OpenAI hires | Project left in ambiguity; **failure mode for “project+maintainer” goal** |

### Failure cases (reputation damage / no hire)

Public, clean “DMCA’d maintainer who got nothing” dossiers are **under-reported** (selection bias: winners write blogs). Known **failure modes of the conflict path**:

1. **Brand parasitism without unique technical surface** — unofficial “ClaudeX client” with no novel agent runtime → C&D, no rival bid.
2. **Hostile issue tracker / toxic community** — disqualifies steward narrative (company won’t put you on brand).
3. **Security disaster during virality** — agents with broad machine control (OpenClaw-class) that ship reckless defaults can kill foundation path.
4. **License / IP contamination** — GPL viral infection of lab stack; undisclosed model-weight theft; AI-generated code without provenance — deal killers in diligence.
5. **Solo bus factor + no governance** — company wants continuity; one uninsured human is risk.

### Does building an unofficial client raise or lower odds?

| Toward **same** vendor | Toward **rival** vendor |
|---|---|
| **Lowers** if trademark/API ToS/DMCA path | **Can raise** if you prove demand + category leadership **and** rebrand cleanly |
| Neutral/positive if you are a **good API citizen** with unique value (not name-squatting) | Still need independent asset (protocol, runtime, distribution) |

**Rule of thumb:** unofficial wrapper of vendor A is a **poor primary strategy to get hired by A**. It is a **high-variance, high-legal-risk path** that occasionally creates bidding from B (OpenClaw). Building a **neutral dependency layer** (ggml, Bun, marimo) is the higher base-rate steward-hire path.

---

## 6. Disqualifiers

| Disqualifier | Evidence weight | Notes |
|---|---|---|
| **GPL / strong copyleft** on core lib that must ship inside proprietary agents | High | Labs prefer MIT/Apache for stack embedding. Successful cases: MIT (llama.cpp, Bun), permissive (marimo). |
| **Trademark collision** with vendor | Medium–High for *that* vendor | Forces rename; can be survived if product strong (OpenClaw). |
| **Undisclosed AI-generated / laundered code** | High in diligence | IP representation risk. |
| **Prior employment / non-compete** | Medium | Jurisdiction-dependent (CA vs EU); still slows deals. |
| **Solo maintainer, no succession** | Medium | Steward-hire often wants a **team** or a foundation plan (ggml had team; OpenClaw moved to foundation). |
| **No SECURITY.md / ignored vulns / agent RCE culture** | High for agent tooling | Frontier labs are liability-sensitive in 2025–26. |
| **Unwelcoming / abusive tracker** | Medium | Brand risk. |
| **Jurisdiction** | Low–Medium | EU maintainer is fine (Steinberger/Austria path; Gerganov/BG roots). Export controls matter more for model weights than for tooling. |
| **Zero production dependents inside target lab** | High | Pure star counts without dependency = ignored. |
| **Product is pure competitive substitute** (OSS Cursor clone) | High for product-kill ACQ; low for SH | Continue-class outcomes: absorbed and shut down. Prefer **infra layer** over **competing app**. |

---

## 7. Reproducible 12–18 month sequence (2-person EU lab: human + AI agent)

### Honest base rate first

- **Independent AI tooling maintainers** (meaningful GitHub presence): order of **thousands**.
- **True steward-hire / project+maintainer absorptions** by frontier labs or major AI infra cos (ggml, OpenClaw, Bun-class, marimo-class): order of **~5–15 clear public cases** in 2023–2026 window depending on strictness.
- **Base rate:** low single-digit percent even among “successful” OSS AI tools; among all maintainers **≪1%**.
- This is **not** a lottery with equal tickets — tickets are **weighted by production dependency**. But most tickets still lose.

### Phase 0 (weeks 0–4) — pick the **gap**, not the fashion

1. Map **one** lab’s public roadmap gap you can own for 3 years:
   - local/edge inference packaging,
   - agent harness / tool protocol,
   - eval harness,
   - data/notebook runtime,
   - coding-agent infrastructure (not a full IDE clone).
2. **Avoid:** unofficial branded clients; pure wrappers; “open-source Cursor” as primary product.
3. Success test: “If this works, does Lab X’s product get better or cheaper without replacing their core model?”

### Phase 1 (mo 1–6) — become **infrastructure**, not content

1. Ship MIT/Apache project with: LICENSE, CODE_OWNERS, CONTRIBUTING, SECURITY.md, GOVERNANCE.md (even lightweight), SECURITY contact.
2. Dogfood daily; publish **adoption metrics** (downloads, dependents, “used by”).
3. Get **named engineers inside one lab** to use it (issues, PRs, Discord). Co-shipping > cold email.
4. Incorporate **only if** needed for grants/employment (EU GmbH/SAS/etc.); foundation **later** when multiparty interest appears (OpenClaw timing: foundation at hire, not day 1).
5. Checkpoint: 1–2 external production dependents + active third-party PRs.

### Phase 2 (mo 6–12) — dependency gravity

1. Optimize for **weekly active production use**, not stars.
2. Land in **docs of larger ecosystems** (HF spaces, agent frameworks, packaging).
3. Optional: GitHub Accelerator / Sovereign Tech / NLnet for credibility + security work — **not** as end goal.
4. Sponsors page with real logos (hardware, cloud, labs).
5. Checkpoint: a lab engineer says privately “we rely on this” OR public “ships with X”.

### Phase 3 (mo 12–18) — contact mechanics

1. **Prefer inbound.** Documented winners were courted after virality or after months of collab.
2. If outbound: not “hire me” — “here is joint roadmap to close your gap; we’re evaluating sustainability partners.”
3. When multi-lab interest appears: **do not rush exclusive**; OpenClaw-style week in SF talking to all labs is the leverage moment.
4. Negotiate **steward shape explicitly**: FT employment + project autonomy + OSS license lock + foundation/sponsorship language. Get it in writing in offer side-letter if possible (terms usually **undisclosed** publicly).
5. Artifacts before contact: governance, license cleanliness, security policy, adoption metrics, clean trademark, succession plan, conflict-of-interest disclosure.

### What not to do

- Optimize for GitHub attention theater without dependents.
- Name project after vendor brand.
- Ship agent that exfiltrates by default.
- Claim AI-written code as human without process.
- Expect Continue-like acquisition to preserve the OSS product.

### Checkpoints summary

| Month | Must be true |
|---|---|
| 3 | Public repo, clean license, first external user |
| 6 | SECURITY.md + real issue hygiene + metrics dashboard |
| 9 | Named collaborator at target lab or ecosystem |
| 12 | Production dependency or multi-org adoption |
| 15–18 | Sustainability conversation (sponsor / hire / foundation) |

---

## 8. Three things in this brief that turned out **false** (or overstated)

1. **“There is an observable star/download threshold that triggers outreach.”**  
   **False as a number.** Triggers are **dependency + roadmap gap + relationship**. marimo published numbers; ggml/OpenClaw announcements did **not** lead with star counts. Bun’s clincher was “Claude Code ships as Bun executable,” not star rank.

2. **“Building an unofficial client/wrapper for a vendor systematically raises hire odds with that vendor.”**  
   **False for the same vendor.** OpenClaw’s Anthropic path was **friction then exit to rival**, not steward-hire by Anthropic. Same-vendor unofficial clients more often get TM/C&D. Hire odds rise only if you become a **category leader** rivals want — rare.

3. **“vLLM / LangChain / LlamaIndex / Instructor / DSPy / Aider / Ollama are steward-hire exemplars like ggml and OpenClaw.”**  
   **False for most of that list.** They are important OSS, but outcomes diverge: **VC-backed independence**, **corporate contribution without absorption**, **commercial company around project**, or **no deal**. Treating them as the same pattern overfits “famous AI repo” ≠ “hired with project.” Neural Magic→Red Hat is company M&A around vLLM commercial layer, not Berkeley maintainer steward-hire.

---

## 9. Base rate & final framing

| Population (order-of-magnitude) | Outcome count (strict steward-hire / project+maintainer, AI/dev-tools, 2023–2026) |
|---|---|
| Thousands of indie AI tool maintainers | **Very few** clear SH cases (ggml, OpenClaw; adjacent: Bun, marimo) |
| Hundreds of notable AI OSS projects | **Low tens** of any absorption/RA/ACQ involving AI tooling talent |
| Funded AI startups | Many more **RA** outcomes (Inflection, Adept, Character, Windsurf) — different game |

**Plain statement:** engineering a ggml/OpenClaw-class outcome on purpose is **possible only by becoming critical infrastructure**, not by “getting noticed on GitHub.” Attention is neither necessary nor sufficient; **being load-bearing for a lab’s next product** is.

If the two-person lab’s true goal is **employment at a frontier lab**, classic research/engineering hire may have **higher base rate** than steward-hire. If the goal is **steward-hire specifically**, accept lottery odds and optimize for **dependency creation** in a roadmap hole.

---

## Source index (primary preferred)

1. https://huggingface.co/blog/ggml-joins-hf — 2026-02-20  
2. https://github.com/ggml-org/llama.cpp/discussions/19759 — 2026-02-20  
3. https://steipete.me/posts/2026/openclaw — 2026-02-14  
4. Sam Altman X announcement re Steinberger / OpenClaw foundation — 2026-02-15  
5. https://bun.com/blog/bun-joins-anthropic — 2025-12-02  
6. https://www.anthropic.com/news/anthropic-acquires-bun-as-claude-code-reaches-usd1b-milestone — 2025-12-03  
7. https://marimo.io/blog/joining-coreweave — 2025-10-30  
8. https://continue.dev — acquisition notice ~2026-06  
9. The Information — OpenAI hires Cline staffers — 2026-01-28  
10. Red Hat / BusinessWire — Neural Magic acquisition — 2024-11-12  
11. TechCrunch/CNBC/Reuters — Windsurf, Character.AI, Adept, Inflection — 2024–2025  
12. Business Insider — Clawdbot rename / Anthropic trademark — 2026-01-27  

*Undisclosed throughout: cash amounts for ggml/OpenClaw/Continue steward deals; exact star counts at first outreach for most cases.*

---

## Appendix: What company is “actually buying” in steward-hire

| Asset | ggml | OpenClaw | Bun | marimo |
|---|---|---|---|---|
| Maintainer time | ✓ | ✓ | ✓ | ✓ |
| Category standard | Local inference | Personal agents (viral) | JS runtime for agents | Reactive notebooks |
| Integration surface | transformers↔GGUF | Agent product DNA | Claude Code binary | GPU cloud + notebooks |
| Narrative / trust | Open local AI | Open agents after TM fight | “Bun won’t die” | OSS notebook future |
| Exclusive IP | No (OSS) | No (foundation) | Soft (company acq, OSS stays) | Soft (company acq, OSS stays) |

**End of DR26-08-01-MACANTON-02-659**
