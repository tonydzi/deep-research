# DR26-08-06-HUB-03-1725 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-06-HUB-03-1725
vendor: xAI Grok
door: cli-headless
rail: grok
cmd: «внутренний путь лаборатории»

collected: 2026-08-06T17:13:26Z
took_sec: 204
exit: 0
sources_unique_urls: 29
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

Начинаю веб-разведку: каталог оркестраторов, ToS, распределение нагрузки и источники квот.Дальше: первоисточники ToS, документация оркестраторов и источники квот.# Deep Research: рынок оркестраторов, которые жгут подписки, а не поштучный API

**Дата среза:** 2026-08-06  
**Режим:** только веб-первоисточники; локальные файлы/репо заказчика не открывались.  
**Уверенность:** high по публичным ToS/докам; medium по «кто реально банит» (зависит от enforcement-волн и user reports); low там, где вендор молчит — помечено **гипотеза**.

---

## 1. Сводная таблица оркестраторов / агентов

Главный столбец — **как аутентифицируется каждый провайдер**. «CLI-reuse» = дёргает локально установленный CLI вендора как подпроцесс (паттерн OpenClaw). «Embedded OAuth» = встраивает подписочный OAuth/токен в свой HTTP-клиент (то, что Anthropic режет).

| Инструмент | Лицензия / кто / свежесть | Auth по провайдерам (главный столбец) | CLI-reuse чужого вендора? | Quota-aware routing / fallback | Headless / cron / из другого агента | На чём ломается (живые отзывы) |
|---|---|---|---|---|---|---|
| **OpenClaw** (ex Clawdbot/Moltbot) | OSS-оркестратор; docs openclaw.ai; активен 2026 | **Anthropic:** (1) API key pay-as-you-go; (2) **Claude CLI** — reuse login на том же хосте (`claude -p` / stream-json). **OpenAI Codex, Qwen, MiniMax, Z.AI** — subscription-style OAuth по докам. **Gemini:** bundled `google-gemini-cli` backend. | **Да**, first-class: `claude-cli`, `google-gemini-cli`; Codex идёт через app-server harness, не отдельный `codex-cli` backend | **Да:** `agents.defaults.model.fallbacks`, auth cooldown profiles, CLI как safety-net | Gateway + `openclaw agent`; long timeouts; node hosts; пригоден к headless | Баны/ломания OAuth-harness (янв–апр 2026); version gate `msg_lifecycle_v1` на Claude Code ≥2.1.206; Docker path/login pitfalls; после OAuth-бана уходит в API-cost |
| **OpenCode** (anomalyco) | OSS TUI/agent; docs opencode.ai | **Anthropic:** API key; **Claude Pro/Max plugins выпилены с 1.3.0** — «Anthropic explicitly prohibits this». **OpenAI:** ChatGPT Plus/Pro OAuth **или** API key. **GitHub Copilot / GitLab Duo** device/OAuth. **xAI:** SuperGrok device OAuth **или** API key. 75+ providers via Models.dev | **Нет** нативного CLI-reuse как primary (можно через baseURL → CLIProxyAPI **гипотеза**) | Частично: model picker, provider order (OpenRouter и т.п.); не «остаток бака Max» | TUI-first; desktop; headless/CI возможен, но не главный UX | Legal pressure Anthropic (PR «anthropic legal requests»); bans после OAuth; community anti-Anthropic backlash; glitches multi-model |
| **Claude Code Router** (musistudio) | OSS local proxy; npm `@musistudio/claude-code-router` | **Только API-ключи / OpenAI-compatible endpoints** (OpenRouter, DeepSeek, Gemini API, Ollama…). Pro/Max как отдельный provider — open issue, не продукт | **Нет** (проксирует HTTP Anthropic-shape, не spawn `claude`) | Task-type routing (default/background/think/longContext) — **по типу запроса, не по % квоты** | Да: local daemon :3456; ideal for cron-ish pipelines | «Super glitchy»; latency через proxy; не решает подписочный Max |
| **Hermes Agent** (Nous Research) | MIT; github.com/NousResearch/hermes-agent; Feb 2026+ | **Nous Portal** OAuth sub. **OpenAI Codex** ChatGPT device OAuth (+ import `~/.codex/auth.json`). **Anthropic:** Max OAuth **только extra/overage credits** (base Max **не** жжёт); Pro OAuth **нет**; API key; setup-token; auto-detect Claude Code creds. **xAI SuperGrok** browser OAuth. **Gemini:** API key only (consumer Pro/Ultra path **не** documented). **Copilot** OAuth / `copilot-acp` spawn. Qwen/MiniMax OAuth и десятки API keys | **Да** для coding: skills shell out `claude -p`; `copilot-acp` spawns CLI; community pattern = subprocess, не embedded token | Fallback chain + auxiliary models (vision/summarize → cheaper). Quota-% routing **не** first-class | Designed for VPS/Telegram/cron; headless strong | Hard usage limits без visibility (Codex+sub); Anthropic OAuth semantics «extra only» surprise; 403 на xAI OAuth tier |
| **goose** (Block → AAIF/Linux Foundation) | OSS (Apache-ish); goose-docs.ai | **API keys** 15+ providers. **ACP providers (subscription path):** Claude ACP (`claude-agent-acp` + logged-in Claude CLI), Codex ACP (ChatGPT Plus/Pro), Amp ACP, Pi ACP. Deprecated raw CLI providers → ACP | **Да via ACP adapters** (spawn/wrap vendor agents), не raw token extract | Provider switch; не built-in multi-bucket quota balancer | `goose run`, CLI + desktop + API; headless ok | No session fork/resume on ACP yet; binary PATH issues; setup friction |
| **Aider** | Apache-2.0; Aider-AI | **Только API keys / local** (Anthropic, OpenAI, DeepSeek, Ollama…). Нет consumer Claude/ChatGPT OAuth в core | **Нет** | Architect/Editor dual-model (strong+cheap) — static split, not live quota | CLI; scriptable; cron-friendly | Не «agent swarm»; multi-turn дороже у Cline/Roo; leaderboard lag |
| **goose-class vs Aider:** Aider = pair-programmer, не multi-agent orchestrator | | | | | | |
| **Continue** | Apache-2.0; continuedev (Cursor ecosystem) | **BYOK API keys** + Hub; local Ollama/LM Studio | **Нет** | Model profiles; не subscription-quota router | IDE + CLI; CI partial | Token bills surprise; Hub team features paywall |
| **Cline / Roo Code** | Apache-2.0; Cline + fork Roo | **BYOK** + Cline usage-billing OAuth (их gateway, не Claude Max). Roo → roomote cloud | **Нет** native Claude CLI reuse | Multi-mode (architect/code/debug); not multi-sub bucket | IDE primary; CLI exists (Cline 3.x) | Token burn high vs Aider; fork fragmentation |
| **Kilo Code** | MIT; Kilo-Org | **BYOK** + Kilo Gateway 0% markup. **xAI:** SuperGrok/X Premium **OAuth** documented | **Нет** (OAuth in-app, not spawn) | Auto Model Frontier/Balanced/Free | IDE + CLI | Gateway dependency; still mostly API-metered |
| **OpenHands** (All-Hands-AI) | MIT | **API keys** / Bedrock / OpenHands Cloud keys; no Claude Max OAuth path as primary | **Нет** | Multi-agent runtime; not sub-quota | Docker headless excellent; GitHub Issues | Infra cost + API cost; setup heavy |
| **Zed agent panel** | Zed Industries; proprietary + OSS editor | **ChatGPT subscription sign-in** for agent (May 2026 blog); also API; Codex CLI in terminal | **Частично:** runs Codex CLI in terminal threads; not full multi-vendor CLI matrix | Agent model selection; not fleet quota | GUI-first; terminal threads | Editor lock-in; not fleet orchestrator |
| **Crush** (Charm) | Charmbracelet; Go TUI | Multi-provider; **попал под январский OAuth crackdown** рядом с OpenCode (user/tech reports) | **Нет** (свой harness) | Provider switch | TUI; headless limited | OAuth ban wave; Charm TUI complexity |
| **Amp** (Sourcegraph) | Proprietary product + amp-acp | **Amp subscription** via Amp CLI; goose ACP wraps it | Amp CLI itself; not Claude CLI | Deep mode product routing | CLI + IDE | Credit burn; proxy bypass bugs (CLIProxyAPI issues) |
| **Warp agent mode** | Warp; client AGPL-3.0 (2026) | Warp account + **spawns Claude Code / Codex / Antigravity as sub-agents**; auto-route open models | **Да** — manages vendor CLIs as sub-agents | Auto-routed open models + vendor agents | Terminal-native; less pure cron | AGPL; not multi-machine lab orchestrator |
| **CLIProxyAPI** (router-for-me) + forks CliRelay | OSS Go proxy | **OAuth/device** for Claude / Codex / Gemini CLI / Grok Build / Antigravity; multi-account round-robin; exposes OpenAI/Claude/Gemini-compatible HTTP | **Да** — wraps CLI subscriptions as API for **any** client (OpenCode, Cline, custom) | Multi-account LB; 429 Retry-After issues open | Perfect headless daemon | **ToS risk high** if spoofs/embeds OAuth as third-party API; maintenance; nested stream bugs |
| **LiteLLM / Bifrost / OpenRouter** | Gateway class | API keys / cloud IAM; OpenRouter = single key multi-model | **Нет** (unless backend is your proxy) | Strong failover, spend limits, smart routing | Yes | Not subscription-native; OpenRouter markup |
| **Pi** (earendil / Armin lineage) | Minimal harness | Via own auth / ACP | Via pi-acp in goose | Minimal | CLI | Too thin for multi-sub fleet |
| **Claude Code (official)** — baseline, не оркестратор | Anthropic proprietary CLI | Native OAuth (Pro/Max/Team) or API | N/A (is the CLI) | Single vendor | `claude -p` headless | 5h + weekly limits; 4×/week hits = your case |

### Кто реально «класс OpenClaw» (несколько агентов/моделей + подписочные входы)

**Полноценные (subscription-aware multi-rail):**

1. **OpenClaw** — эталон CLI-reuse + fallbacks.  
2. **Hermes Agent** — always-on coordinator + Codex OAuth + SuperGrok OAuth + Claude subprocess/skills + (осторожный) Anthropic OAuth extra-credits.  
3. **goose + ACP** — subscription path через vendor agents, не token theft.  
4. **CLIProxyAPI** — инфраструктурный слой «подписка → OpenAI-compatible API» (юридически самый серый).  
5. **Warp agent mode** — terminal that spawns vendor CLIs.  
6. **OpenCode** — multi-provider, но **Anthropic subscription path вырезан**; OpenAI/xAI/Copilot sub остаются.

**Не того класса (почти только API-ключ / один вендор):** Aider, Continue, Cline/Roo (кроме своего billing), OpenHands, Claude Code Router, classic Amp without ACP.

Источники таблицы (выборка):  
https://docs.openclaw.ai/providers/anthropic · https://docs.openclaw.ai/gateway/cli-backends · https://opencode.ai/docs/providers/ · https://hermes-agent.nousresearch.com/docs/integrations/providers · https://goose-docs.ai/docs/guides/acp-providers · https://github.com/router-for-me/CLIProxyAPI · https://zed.dev/blog/chatgpt-subscription-in-zed · https://kilo.ai/docs/ai-providers/xai · https://github.com/NousResearch/hermes-agent · https://aider.chat/

---

## 2. Карта риска: CLI-subprocess vs embedded OAuth

### 2.1 Что запретил Anthropic (первоисточник)

**Claude Code Docs → Legal and compliance → Authentication and credential use** (актуально на дату среза):

> «**OAuth authentication** is intended exclusively for purchasers of Claude Free, Pro, Max, Team, and Enterprise subscription plans and is designed to support ordinary use of **Claude Code and other native Anthropic applications**.»  
> «**Developers** building products or services that interact with Claude’s capabilities, including those using the Agent SDK, should use **API key authentication**… Anthropic **does not permit third-party developers to offer Claude.ai login or to route requests through Free, Pro, or Max plan credentials on behalf of their users**.»  
> «Anthropic reserves the right to take measures to enforce these restrictions and **may do so without prior notice**.»

Источник: https://code.claude.com/docs/en/legal-and-compliance

**Consumer Terms (EEA consumer version, anthropic.com/legal/consumer-terms)** — отдельный жёсткий пункт про автоматизацию:

> «Except when you are accessing our Services via an Anthropic API Key or where we otherwise explicitly permit it, **to access the Services through automated or non-human means, whether through a bot, script, or otherwise**.»

Также: нельзя делиться credentials; нельзя crawl/scrape «other than as permitted».

Источник: https://www.anthropic.com/legal/consumer-terms

**Agent SDK / `claude -p` billing (Support, update 15–16 June 2026):**  
Anthropic **paused** отдельный Agent SDK credit plan. **Сейчас:** Claude Agent SDK, `claude -p`, and third-party app usage **still draw from subscription usage limits**. Ранее анонсированный monthly credit **недоступен**.

Источник: https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan

**OpenClaw’s own framing:** запускает installed Claude Code CLI in non-interactive print mode; цитирует staff «allowed again»; рекомендует API key для shared production automation.

Источник: https://docs.openclaw.ai/providers/anthropic

### 2.2 Чем CLI-reuse ОТЛИЧАЕТСЯ от embedded OAuth (юридическая механика)

| | **Embedded OAuth / token theft** | **Spawn local Claude CLI (`claude -p`)** |
|---|---|---|
| Что делает клиент | Извлекает OAuth token, ходит на Anthropic API **своим** HTTP-стеком, часто со spoofed client identity | OS process `claude` с уже сделанным `claude auth login`; auth остаётся внутри официального бинарника |
| Что говорит docs Anthropic | Прямо запрещено third-party route of Free/Pro/Max credentials | Не названо «разрешённым third-party product»; `claude -p` = programmatic/Agent SDK path, **сейчас жрёт subscription limits** |
| OpenCode | Плагины Pro/Max **выпилены 1.3.0** с явной фразой «Anthropic explicitly prohibits this» | Не primary path |
| OpenClaw | OAuth-harness path historically blocked (апр 2026 wave) | Documented «Claude CLI» provider |
| Consumer Terms automation clause | Явно bot/script risk | **Гипотеза среднего риска:** subprocess всё равно «automated means», если нет explicit permit; спор: вы используете *native* Claude Code, а не third-party login UI |

**Ключевое отличие не «магия ToS = green», а *где* живёт credential и *кого* Anthropic считает developer product.**

- **Third-party product offering Claude.ai login / routing Max credentials** = red (docs).  
- **You running official Claude Code non-interactively** = grey→amber: billed as subscription Agent SDK usage today; enforcement historically focused on **spoofing harness + OAuth in foreign clients**, not on `claude -p` itself.  
- **Your orchestrator that only shells out** still can look like automation / non-ordinary use if volume = 24/7 agent farm (Anthropic already cited 24/7 background abuse when introducing weekly limits — community reports of Aug 2025 weekly caps).

### 2.3 OpenAI сторона

**Terms of Use** (ROW, openai.com/policies/row-terms-of-use) содержат запрет:

> «**Automatically or programmatically extract data or Output**»

Источник: https://openai.com/policies/row-terms-of-use/

При этом OpenAI **официально** поддерживает ChatGPT subscription → Codex CLI и **third-party agent surfaces**:

- Zed: sign in with ChatGPT for agent — https://zed.dev/blog/chatgpt-subscription-in-zed  
- OpenCode: ChatGPT Plus/Pro OAuth first-class — https://opencode.ai/docs/providers/  
- Hermes: Codex device OAuth + import `~/.codex` — Hermes providers docs  
- goose: codex-acp — https://goose-docs.ai/docs/guides/acp-providers  

OpenAI staff на GitHub discussion re forking Codex CLI: Terms + Apache license «quite permissive»; OSS projects like OpenCode do similar things (не formal legal opinion, но signal).

Источник: https://github.com/openai/codex/discussions/8338

**Вывод по OpenAI:** spawn/`codex` login path и OAuth device flow в third-party **сейчас industry-normal**; risk ниже Anthropic. Риски: hard usage limits without visibility в agent loops; accidental API-key billing if `<key-var>` leaks into env.

### 2.4 Известные баны и как вендор отличает

**Задокументированные волны Anthropic:**

| Дата | Событие | Источник |
|---|---|---|
| ~5–9 Jan 2026 | OAuth в third-party (OpenCode #6930 ban report; Crush mentioned) | https://github.com/anomalyco/opencode/issues/6930 · https://ai.sulat.com/claude-code-cripples-third-party-coding-agents-from-using-oauth-6548e9b49df3 |
| Feb 2026 | Docs formalize Authentication and credential use | Claude Code legal page |
| Mar 2026 | OpenCode removes Anthropic OAuth plugin (legal requests PR) | https://ridakaddir.com/blog/post/did-anthropic-kill-opencode-claude-subscription-ban |
| ~4 Apr 2026 | Broader enforcement OpenClaw/OpenCode/NanoClaw OAuth harness | Community: https://kersai.com/anthropic-killed-third-party-claude-access-heres-every-workaround-that-still-works/ · TechCrunch re OpenClaw creator ban (secondary reports) |

**Признаки детекции (из public reverse-engineering + HN, не официальный whitelist Anthropic):**

1. **Client identity / headers** не равны Claude Code.  
2. **OAuth token used outside native app** API surface.  
3. **Volume patterns:** 24/7 multi-session, multi-IP, multi-device same account.  
4. **Error string** (user reports): *«This credential is only authorized for use with Claude Code…»*  
5. **Гипотеза:** `claude -p` from official binary harder to distinguish from human terminal use than custom HTTP client — но telemetry (session density, tool loop rate, concurrent sessions) всё равно видна.

**Честный вердикт риска для вашей установки:**

| Паттерн | Risk of ban | Risk of ToS conflict | Практический комментарий |
|---|---|---|---|
| OpenCode/OpenClaw **embedded** Max OAuth | **High** (уже массово резали) | **High** | Не делать |
| OpenClaw/Hermes **`claude -p` / CLI backend** | **Medium** (сейчас «живёт», staff signals mixed; policy can flip) | **Medium–High** under automation clause + «ordinary use» | Работает; не «safe harbor» |
| Official interactive Claude Code | **Low** | **Low** | Baseline |
| Hermes Anthropic OAuth «extra credits only» | **Medium** (routes «as Claude Code» — docs Hermes; ToS third-party still awkward) | **High grey** | Сюрприз биллинга + policy risk |
| CLIProxyAPI exposing Claude OAuth as OpenAI API to many tools | **High** | **High** | Самый близкий к banned harness class |
| Codex OAuth / ChatGPT Pro in third-party | **Low–Medium** | **Lower** than Anthropic today | OpenAI product direction |
| SuperGrok OAuth in Hermes/OpenCode/Kilo | **Low–Medium** | Undocumented edge cases (403 tier) | Separate from API spend |

**Если риск бана реален — да, реален**, особенно на Anthropic, и enforcement уже **доказан кейсами 2026**, не теорией. CLI-reuse — **лучшая из серых зон**, не зелёная.

---

## 3. Как крупные пользователи распределяют нагрузку (и что применимо к вам)

### 3.1 Устоявшаяся таксономия классов работы

Из engineering blogs + routing literature 2025–2026:

| Класс | Примеры | Куда обычно | Метрика «не сломалось» |
|---|---|---|---|
| **Easy / grunt** | rename, format, imports, boilerplate, simple edit | Flash / Haiku / mini / local 7–14B | Exact match / lint pass |
| **Medium** | multi-file feature, moderate debug | Sonnet / GPT mid / Gemini Pro | Tests green + review time |
| **Hard / architecture** | system design, deep debug, novel algo | Opus / GPT high / strongest | Human accept rate; rework loops |
| **Mechanical non-LLM** | shell, grep, file IO, MCP | **Не модель** | Wall time, success rate |
| **Retrieval / read** | codebase search, doc skim | cheap + tools, not frontier | Answer found without re-ask |
| **Long-context synthesize** | large PR summary | long-context mid tier | Factual checklist |
| **Safety / irreversible** | prod migrate, secrets, money | strongest + human gate | Zero silent wrong |

Источники таксономии/роутинга:  
https://www.morphllm.com/llm-router · https://www.digitalapplied.com/blog/llm-model-routing-2026-cost-quality-optimization-engineering-guide · https://aws.amazon.com/blogs/machine-learning/multi-llm-routing-strategies-for-generative-ai-applications-on-aws/ · arXiv task-profile routing https://arxiv.org/html/2604.09377v1 · Aider Architect/Editor pattern https://aider.chat/

**Критично для ваших цифр:** 54% shell + 16% code edit + 12% read + 11% MCP = **большая доля — не «thinking»**. Это классический anti-pattern «frontier model as OS».

### 3.2 Как измеряют «хуже стало»

Практики из production routing:

1. **Shadow A/B:** same task → cheap model; compare tests / human accept (не только BLEU).  
2. **Rework rate:** % tasks needing second pass on stronger model.  
3. **Loop length:** tool rounds until done (дешёвая модель часто **дороже** из-за 3× rounds — classic Cline vs Aider complaint).  
4. **Escape hatch rate:** fallback to Opus frequency.  
5. **User friction:** «I had to rewrite the prompt».

### 3.3 Что переносится без потери / что откатывают

| Обычно **переезжает** | Обычно **откатывают назад** |
|---|---|
| Shell orchestration wrappers (не LLM) | Multi-hour autonomous coding on weak model |
| Search/read summarization | Subtle architecture decisions |
| Boilerplate generation | Security-sensitive patches |
| Format/lint/typefix | «Why is production weird» debugging |
| Draft FA / triage classification | Voice/identity-sensitive writing (your Fable lane) |
| Parallel review by second vendor | Single-shot complex refactors |

### 3.4 Применимо к вашей установке (4 subs × 6 machines × 1 owner)

| Ваш бак | Логичная роль | Почему |
|---|---|---|
| **Anthropic Max $200** | Hard coding + judgement + author voice; **не** shell farm | Уже 4×/week limit; 36.8M tokens mostly mechanical |
| **ChatGPT Pro $200 / Codex** | Medium coding, PR implement, second opinion, long agent runs when Claude hot | Sub path officially friendly; **сейчас 4% — мёртвые деньги** |
| **SuperGrok $300** | Bulk reasoning/code where Grok Build/CLI fits; cheapish agent loops | Самый дорогой бак **не измеряется** — слепая зона |
| **Gemini Pro $20** | Grunt: read/summarize, flash classification, high RPD CLI quota | Дешёвый; Gemini CLI `/stats model` даёт session limits snapshot |

**Архитектурный паттерн «под вас» (не «переезд с Claude Code»):**  
Claude Code остаётся **дверь/оркестратор UI**, но **работа** уходит:

1. Mechanical → scripts / local tools (0 tokens).  
2. Grunt LLM → Gemini CLI subprocess or Codex.  
3. Medium implement → Codex / Grok.  
4. Hard + voice → Claude Max only.  
5. Fallback chain: Claude exhausted → Codex → Grok → Gemini (OpenClaw/Hermes/goose style).

Это ровно то, что community Hermes-setups описывают: Hermes brain = Codex sub; coding specialist = `claude -p` Max; Telegram front — https://www.reddit.com/r/hermesagent/comments/1t9chdk/the_ai_agent_setup_that_finally_clicked_for_me/

---

## 4. Измеримость остатка квоты

| Вендор | Есть machine-readable остаток **subscription**? | Что именно | Как снять |
|---|---|---|---|
| **OpenAI / Codex (ChatGPT sub)** | **Да** | `rate_limits.primary.used_percent` (и related) в CLI transcript — вы уже это делаете | Codex CLI transcripts; community also notes website/CLI show usage |
| **Anthropic API key (Console)** | **Да** (rate limits, not Max plan %) | Headers: `anthropic-ratelimit-requests-remaining`, `anthropic-ratelimit-tokens-remaining`, reset times | https://platform.claude.com/docs/en/api/rate-limits |
| **Anthropic Max / Claude Code subscription** | **Частично / хрупко** | UI: Settings → Usage (5h + weekly bars) — https://support.claude.com/en/articles/9797557-usage-limit-best-practices. Community: endpoint `https://claude.ai/api/oauth/usage` returns `five_hour.utilization`, `seven_day.utilization` (gist monperrus; **неофициальный**, может сломаться). OpenClaw claims subscription credentials show quota windows in Control UI | **Надёжно:** парсить `/status` UI глазами или screenshot OCR **гипотеза**. **Хрупко:** oauth usage API с session cookie/token. **Нет** публичного стабильного SDK «% Max remaining» как у Codex |
| **xAI SuperGrok consumer** | **По сути нет** для machine orchestration | API: console rate limits by spend tier (RPS/TPM) — https://docs.x.ai/developers/rate-limits. SuperGrok consumer: Reddit «you can't see remaining quota»; some June 2026 marketing claims dashboard counters — inconsistent | API headers `X-RateLimit-*` only for **API key** path. Subscription: UI only / opaque 429 |
| **Google Gemini Pro (consumer) / Gemini CLI** | **Частично** | Gemini CLI: `/stats model` — session token usage + limits for current quota — https://geminicli.com/docs/resources/quota-and-pricing/. Pro plan RPD (e.g. 1500) documented. AI Studio rate-limit page for API keys | CLI `/stats`; API key → AI Studio limits. **Нет** clean `used_percent` JSON как Codex для Pro sub across all surfaces |
| **xAI API (not SuperGrok chat)** | **Да** | Tier + headers | Console + response headers |

**Прямые ответы «нет»:**

- **SuperGrok subscription % remaining** — **нет** воспроизводимого first-class machine source (искал: xAI rate-limits docs = API teams; Reddit SuperGrok = no remaining quota; no public SuperGrok CLI usage JSON equivalent to Codex).  
- **Anthropic Max %** — **нет** documented public API on par with Codex `used_percent`; only UI + unofficial oauth usage endpoint + ratelimit headers **for API keys** (другой продукт).

**Искал и не нашёл:** официальный Anthropic «subscription utilization API for Max» в platform docs; официальный xAI SuperGrok quota API; Gemini Pro «used_percent» field in CLI JSONL на уровне Codex.

---

## 5. Три вещи, которые вы (по цифрам) делаете неправильно

### 5.1 Платите ~$740/мес, а жжёте один бак до упора

**Факт из вашего замера:** Claude limit 4×/week; Codex **4%**; Grok/Gemini **не измерялись**.  
**Неправильно:** покупать четыре подписки как «склад мощности», а маршрутизировать 100% работы в одну дверь.  
**Правильно по рынку:** static tiering **до** fancy router — 50–70% savings from workload-to-model matching (routing literature). Ваш Codex Pro $200 ≈ сжигается впустую.

### 5.2 Frontier-модель делает shell/MCP/read (82% «механики» в духе вашего 54+16+12+11)

**Неправильно:** Claude Code как единственный runtime для OS-агентности.  
**Правильно:** оркестратор (можно оставить Claude Code) должен **делегировать** grunt на Codex/Grok/Gemini CLI subprocess **или** на zero-LLM scripts. OpenClaw/Hermes/goose ACP существуют именно для этого.  
Aider-style Architect/Editor: дорогая модель думает коротко, дешёвая пишет diff — у вас наоборот: дорогая модель крутит shell.

### 5.3 Нет machine-readable quota → нет fallback → только «упёрлись 4 раза»

**Неправильно:** discover limit by 429/lockout.  
**Правильно:**  
- Codex: уже есть `used_percent` → **включить** в router policy (switch off Claude when weekly > N%).  
- Anthropic: минимум scrape Settings/Usage или unofficial oauth usage **с пониманием fragility** + conservative time-based budget (e.g. stop Max agentic after K turns / hour).  
- Grok/Gemini: instrument **before** scale-out (Gemini `/stats`; Grok — accept opacity, use hard daily caps).  

Без (5.3) любой multi-vendor orchestrator — театр: fallback-цепочка без датчика бака = всегда late.

---

## Практическая карта «что ставить руками» (без смены primary door)

Не «переезжайте с Claude Code». **Добавьте слой отдачи работы:**

| Приоритет | Слой | Зачем |
|---|---|---|
| 1 | **Измерить** Grok + Gemini utilization (даже грубо: daily counters log) | Иначе $300+$20 = чёрный ящик |
| 2 | **Codex CLI as worker** from Claude Code (subprocess) for medium implement | Сжечь 4% → 40–60% |
| 3 | **OpenClaw CLI backends** or **Hermes** only if need always-on multi-channel; иначе thin shell router в skills | Не тащить second OS без нужды |
| 4 | **Не** CLIProxyAPI/OpenCode Anthropic OAuth | Ban surface |
| 5 | **goose ACP** if want standards-based Claude/Codex subscription without inventing harness | Cleaner than token copy |

---

## Ограничения этого отчёта

- ToS и product policy **меняются без notice** (Anthropic June 15 pause = пример).  
- «Allowed again» staff quotes ≠ contract.  
- Release dates/stars по OSS-инструментам — secondary sources; перед продом сверять GitHub releases.  
- Не legal advice: риск-карта для engineering decision, не opinion of counsel.

---

### Ключевые URL (индекс)

- OpenClaw Anthropic: https://docs.openclaw.ai/providers/anthropic  
- OpenClaw CLI backends: https://docs.openclaw.ai/gateway/cli-backends  
- Anthropic Claude Code legal: https://code.claude.com/docs/en/legal-and-compliance  
- Anthropic Consumer Terms: https://www.anthropic.com/legal/consumer-terms  
- Anthropic Agent SDK plan: https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan  
- Anthropic usage limits UI: https://support.claude.com/en/articles/9797557-usage-limit-best-practices  
- Anthropic API rate limit headers: https://platform.claude.com/docs/en/api/rate-limits  
- OpenCode providers: https://opencode.ai/docs/providers/  
- Hermes providers: https://hermes-agent.nousresearch.com/docs/integrations/providers  
- goose ACP: https://goose-docs.ai/docs/guides/acp-providers  
- CLIProxyAPI: https://github.com/router-for-me/CLIProxyAPI  
- OpenAI Terms: https://openai.com/policies/row-terms-of-use/  
- Zed ChatGPT sub: https://zed.dev/blog/chatgpt-subscription-in-zed  
- Gemini CLI quota: https://geminicli.com/docs/resources/quota-and-pricing/  
- xAI API rate limits: https://docs.x.ai/developers/rate-limits  
- Kilo xAI SuperGrok OAuth: https://kilo.ai/docs/ai-providers/xai  

**Конец отчёта.**
