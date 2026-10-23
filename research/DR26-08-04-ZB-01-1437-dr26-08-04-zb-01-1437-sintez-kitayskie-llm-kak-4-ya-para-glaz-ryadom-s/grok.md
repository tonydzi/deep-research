# DR26-08-04-ZB-01-1437 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
id: DR26-08-04-ZB-01-1437
rail: grok
rail_detail: grok CLI headless (подписка xAI, локальный CLI, без браузера)
captured: 2026-08-04
captured_by: AUTO-mac16-260731-dr-runner-daily

source: headless CLI (без браузера, приватного чат-URL нет)
---

Готовлю фактологичный DR по китайским LLM на 01.08.2026: сначала соберу свежие релизы, бенчмарки и прайсы с официальных и независимых источников.Дальше — официальные прайсы и остальные лаборатории: GLM, MiniMax, Qwen, Doubao, Hunyuan.# DEEP RESEARCH: китайские LLM на 1 августа 2026

**Заказчик:** AI-лаборатория, 5 машин, Claude Code как основной агент  
**Цель:** 3-я / 4-я независимая «пара глаз» рядом с Grok и Gemini  
**Срез данных:** ~01.08.2026 (сбор 04.08.2026)  
**Метод:** официальные docs + независимые лидерборды + агрегаторы; цены без официальной страницы помечены «не подтверждено»

---

## 1. Ландшафт игроков на 01.08.2026

### 1.1. Живые и активно релизившие (фев–авг 2026)

| Лаборатория | Флагман (на ~01.08) | Дата | Архитектура / размер | Веса | Лицензия | Темп 6 мес |
|---|---|---|---|---|---|---|
| **DeepSeek** | V4-Pro + V4-Flash-0731 | API V4 с ~24.04.2026; Flash snapshot 31.07.2026 | 1M ctx, max out 384K | закрытые (API) | proprietary API | 🔥 ускорение |
| **Alibaba Qwen** | Qwen3.7-Max / Qwen3.8-Max; open: 3.5/3.6 | 3.7-Max ~20–21.05.2026; 3.8-Max в Model Studio летом 2026 | 3.7-Max closed, 1M ctx; open = Apache 2.0 | Max closed; 3.5/3.6 open | Apache (open) / proprietary (Max) | 🔥 ускорение |
| **Moonshot / Kimi** | **Kimi K3** (+ K2.6, K2.7 Code) | K2.5 27.01; K2.6 ~20.04; **K3 ~июль 2026** | K3: ~2.8T, 1M ctx, multimodal | K2.x open; K3 API-first | open weights (K2.x); K3 closed API | 🔥 ускорение |
| **Z.ai / Zhipu (GLM)** | **GLM-5.2** (после GLM-5 11.02, 5-Turbo, 5.1) | GLM-5.2 **13–16.06.2026** | ~753B class, **1M ctx** usable | open (HF/ModelScope) | **MIT** (GLM-5 / 5.2) | 🔥 ускорение |
| **MiniMax** | **M3** (фронтир с 01.06); M2.7 18.03 | M2.7 18.03; M3 01.06.2026 | M2.7 ~230B MoE / ~10B active; M3 1M ctx, MSA | M2 open; **M2.7 одно время закрывали** → M3 open-weights путь | Modified-MIT / open (зависит от поколения) | 🔥 ускорение (IPO HK 01.2026) |
| **ByteDance Seed / Doubao** | Seed 2.0 Pro / Code / Lite / Mini | **14.02.2026** | agent-oriented family | closed | proprietary | ⚡ жив (Doubao #1 CN app) |
| **Tencent Hunyuan** | Hunyuan-3 / Hy3-preview (~295B MoE / 21B act.) | spring–summer 2026 (Hy3-preview ~апр 2026) | agent/tool focus | частично open | custom / open variants | ⚡ жив, не лидер EN coding |
| **Baidu Ernie** | Ernie 5.x / X1 Turbo line; Ernie 4.5 open Apache | 2025–spring 2026 | search-grounded | 4.5 open Apache | Apache (4.5) / closed flagship | 🟡 стабильно, меньше hype EN |
| **StepFun** | Step-серия (multimodal) | ongoing 2025–26 | multimodal | mixed | mixed | 🟡 жив, не first-tier coding |
| **iFlytek Spark** | Spark enterprise line | enterprise cadence | CN enterprise | closed | proprietary | 🟡 B2B CN |
| **Xiaomi MiMo** | MiMo V2.x | 2026 releases | consumer/device | mixed | mixed | 🟡 нишевый |
| **Meituan LongCat** | LongCat | 2025–26 | vertical | closed | proprietary | 🟡 vertical |
| **InclusionAI / Ant Ling** | Ling family | 2025–26 | Ant Group ecosystem | mixed | mixed | 🟡 ecosystem |
| **01.AI** | Yi legacy | **исторический контекст** | — | open older Yi | Apache-ish older | 🔴 заглох / pivot |
| **Baichuan** | Baichuan legacy | **исторический контекст** | — | older open | custom | 🔴 тишина |

Источники по флагманам: [DeepSeek pricing docs](https://api-docs.deepseek.com/quick_start/pricing/) (crawled 04.08.2026); [z.ai GLM-5.2 blog 16.06.2026](https://z.ai/blog/glm-5.2); [MiniMax M2.7 18.03.2026](https://www.minimax.io/news/minimax-m27-en); [Qwen3.7 blog 19.05.2026](https://qwen.ai/blog?id=qwen3.7); [Seed 2.0 14.02.2026](https://seed.bytedance.com/en/blog/seed-2-0-official-launch); [Kimi K3 coverage Jul 2026](https://trilogyai.substack.com/p/kimi-k3-is-live-pricing-benchmarks).

### 1.2. Кто ускорился / заглох

| Тренд | Кто | Суть |
|---|---|---|
| **Ускорились** | DeepSeek, Qwen, Moonshot, Z.ai, MiniMax | плотный релиз-каденс, coding/agent push, intl endpoints, Claude Code drop-in |
| **Стабильны, но не «третье мнение для EU-лабы»** | ByteDance Seed, Tencent Hunyuan, Baidu Ernie | сильны в CN product/search; intl friction (KYC, CN console) |
| **Заглохли / сменили модель** | **01.AI**, **Baichuan** | мало публичных frontier-релизов в 2026; не брать как second opinion |
| **Смена бизнес-модели** | MiniMax (после IPO): волна «close weights» на M2.7 → потом open-path на M3; Z.ai: **Coding Plan** как Claude Max-аналог; Qwen: Max closed, mid open |

---

## 2. Качество (на ~01.08.2026)

### 2.1. Независимые замеры (цифры + даты)

**SWE-bench Verified (важно для кода)**  
- Official mini-SWE-agent snapshot (Simon Willison / SWE-bench, **19.02.2026**): топ — Claude 4.5 Opus high; Gemini 3 Flash; **MiniMax M2.5**; в top-10 также **GLM-5, Kimi K2.5, DeepSeek V3.2**.1(https://www.alibabacloud.com/help/en/model-studio/model-pricing)  
- BenchLM public snapshot (~авг 2026): Claude Opus 4.6 **75.6%**, MiniMax M2.7 **75.4%**, GLM-5 **72.8%** (агрегатор; не единственный «истина»).2(https://www.alibabacloud.com/help/en/model-studio/models)  
- SWE-bench site (live, 04.08.2026): китайские модели стабильно в верхнем квартиле на Verified при mini-SWE-agent scaffold.3(https://paywithmoon.com/merchants/openrouter)  

**LMArena / Artificial Analysis / агрегаторы (июль–авг 2026)**  
- Western frontier (Claude Opus/Fable line, GPT-5.x, Gemini 3.x) всё ещё держит **preference Elo** верх; китайские — сильнее на **coding / agent / cost-efficiency**, чем на «общий chat vibe».  
- OpenCompass mid-July 2026 (CN-side): DeepSeek-V4-Pro, Doubao-Seed-2.0-Pro, Kimi-K2.6, Qwen3.6-Max-Preview среди сильнейших domestic.  
- Kimi K3: claims 4-е место Artificial Analysis + лид Arena Code WebDev (coverage 17.07.2026) — **частично lab-adjacent**, ждать независимых re-runs.4(https://www.reddit.com/r/SillyTavernAI/comments/1eyj3ob/how_can_i_pay_for_openrouter_without_visa/)  

**Где разрыв закрылся vs Claude/GPT/Gemini**

| Ось | Статус на 01.08.2026 | Доказательство |
|---|---|---|
| **Agentic coding / SWE-bench** | Разрыв **узкий** (1–5 pp на Verified у лучших CN vs top Claude) | M2.7/GLM/Kimi в top-10 независимого SWE-bench 02.2026+ |
| **Tool-use / multi-step agents** | Разрыв **частично закрыт** у Kimi (swarm), GLM-5.2 (long-horizon), MiniMax M3 | product positioning + coding plan adoption |
| **Math / AIME-class** | Китайские **часто паритет/выше** на self-report и части независимых; осторожно с contamination | исторический паттерн DeepSeek-R1 / Qwen math |
| **Длинный контекст (MRCR/needle)** | **1M** у DeepSeek V4, Kimi K3, GLM-5.2, Qwen Max — маркетинг; «usable» 1M лучше всего документирован у GLM-5.2 | [z.ai GLM-5.2](https://z.ai/blog/glm-5.2) |
| **Русский язык** | Нет публичного независимого RU-лидерборда уровня LMSYS; **гипотеза (med)**: Qwen/DeepSeek/Kimi приемлемы на техтексте; слабее Claude на стиле/идиомах | нет hard source → не claim |
| **Общий chat / creative EN** | Разрыв **остался** (Arena Elo) | LMArena western lead 2026 |

### 2.2. Нам важнее всего: agent coding + tool-use

Практическая иерархия **для code review / second opinion** (не общий chat):

1. **GLM-5.2 / MiniMax M2.7–M3 / Kimi K2.6–K3** — agentic coding  
2. **DeepSeek V4-Pro** — цена×качество + native Anthropic endpoint  
3. **Qwen3.7/3.8-Max** — сильный, но дороже и Max closed  
4. Doubao/Seed, Hunyuan, Ernie — вторично для EU headless code review  

### 2.3. КОНТАМИНАЦИЯ И НАКРУТКА бенчмарков

**Что задокументировано (класс проблем, не только CN):**

1. **Train/test leakage на публичных бенчах** — системная болезнь отрасли с 2023–2025; китайские open-модели чаще обвиняют, потому что веса публичны и train recipes иногда leak. Классика: inflated MMLU/GSM8K/HumanEval при n-gram overlap.  
2. **Scaffold / agent harness gaming на SWE-bench** — score = model × agent × retries × tools; lab-reported ≠ mini-SWE-agent official. Simon Willison (19.02.2026) специально подчёркивал ценность **не self-reported** runs.1(https://www.alibabacloud.com/help/en/model-studio/model-pricing)  
3. **Arena / vote brigading** — спорадические обвинения в 2025–26 против нескольких labs (не всегда CN); верить **только** high-vote, de-anonymized, multi-week Elo.  
4. **Self-report math (AIME/HMMT)** без transcript release — **низкий trust**.

**Каким замерам верить (для нашей лабы):**

| Доверять | Не доверять |
|---|---|
| SWE-bench Verified **official** scaffold | Lab PDF «SOTA on 12 benches» без harness |
| LiveCodeBench / Aider Polyglot **независимые** | «Internal agent eval» |
| Arena Elo с ≥10k votes + confidence intervals | Preview aliases без даты |
| Artificial Analysis Intelligence Index (платный, reproducible-ish) | Marketing «beats Claude» без ссылки |

**Правило:** для second opinion **наш A/B на 20 реальных диффов** > любой лидерборд.

---

## 3. Подписки и цены (практика)

### 3.1. DeepSeek — официальный прайс (верифицировано 04.08.2026)

Источник: [api-docs.deepseek.com/quick_start/pricing](https://api-docs.deepseek.com/quick_start/pricing/)

| Модель | Cache hit in | Cache miss in | Out | Ctx | Concurrency |
|---|---|---|---|---|---|
| **deepseek-v4-flash** | **$0.0028** | **$0.14** | **$0.28** | 1M | 2500 |
| **deepseek-v4-pro** | **$0.003625** | **$0.435** | **$0.87** | 1M | 500 |

- Peak/off-peak: **анонсирован 2×** в пики 09–12 и 14–18 Beijing time — **effective date TBA**.  
- Legacy `deepseek-chat` / `deepseek-reasoner` — retirement deadline **24.07.2026** (по вторичным трекерам).  
- Free tier: chat app free; API — new-user free tokens (вторичные: ~5M; **перепроверить в кабинете**).  
- **Anthropic-compatible:** `https://api.deepseek.com/anthropic` — официальный Claude Code guide.  
- Оплата: platform.deepseek.com, международные карты (практика 2025–26 работает для EU; **не подтверждено** единым ToS-параграфом в этом dig — проверить checkout).  
- **CNY:** не публикуется в USD docs; API billed USD.

### 3.2. Z.ai / GLM — официальный API + Coding Plan

**API (docs.z.ai, USD per 1M):**

| Model | In | Cache in | Out |
|---|---|---|---|
| **GLM-5.2** | **$1.4** | **$0.26** | **$4.4** |
| GLM-5 | $1.0 | $0.2 | $3.2 |
| GLM-5-Turbo | $1.2 | $0.24 | $4.0 |
| GLM-4.7-Flash / 4.5-Flash | **Free** | Free | Free |

Источник: [docs.z.ai/guides/overview/pricing](https://docs.z.ai/guides/overview/pricing) (crawled 04.08.2026).

**GLM Coding Plan** ([z.ai/subscribe](https://z.ai/subscribe)):

| Tier | List / promo | Usage (ориентир) |
|---|---|---|
| Lite | **$18 → ~$12.6/mo** promo | 10k credits/week; ~80 prompts / 5h |
| Pro | **$80 → ~$56/mo** (другие источники: $72 list) | 6× Lite |
| Max | **$168 → ~$117.6/mo** | 14× Lite |

⚠️ **Цены подписок плавают** (promo −20%/−30%, quarterly/yearly). List на z.ai/subscribe на 04.08: Lite $12.6 (from $18), Pro $56 (from $80), Max $117.6 (from $168). Вторичные (aipricing.guru): Lite $18 / Pro $72 / Max $160 — **расхождение → верить z.ai/subscribe в момент оплаты**.

**bigmodel.cn (CN):** цены в **юанях** (пример: GLM-5 input ~4–6 CNY / 1M tiered) — дешевле, но KYC CN.  
**z.ai (intl):** USD, Coding Plan, Claude Code / OpenCode / ZCode.

### 3.3. Moonshot / Kimi

| Product | Price | Note |
|---|---|---|
| **Kimi K3 API** | **$3.00 in / $15.00 out**; cache hit **$0.30** | 1M ctx; ~17.07.2026 coverage |
| K2.6 / K2.7 Code | ~**$0.95 / $4.00** (benchlm Jul 31 sync) | open-weight generation |
| K2.5 | ~**$0.60 / $3.00** | older |
| **Membership** | Adagio free; Moderato **$19**; Allegretto **$39**; Allegro **$99**; Vivace **$199** | Kimi Code credits 1×/5×/15×/30× |

Источники: [benchlm Kimi pricing](https://benchlm.ai/moonshot/api-pricing); [codeagentswarm Kimi plans 18.07.2026](https://www.codeagentswarm.com/en/guides/kimi-code-plans-and-pricing).  
**Бюджет $25:** только Moderato $19 — K3 API burn **не** влезет.

### 3.4. MiniMax

| Plan | Price | Что |
|---|---|---|
| Token Plan Plus | **$20/mo** | ~1.7B tok/mo M3-class (claim) |
| Max | **$50/mo** | ~5.1B |
| Ultra | **$120/mo** | heavy agents |
| PAYG M2.7 | ~**$0.30 / $1.20** | официальные трекеры |

Источник: [platform.minimax.io Token Plan](https://platform.minimax.io/subscribe/token-plan) (crawled 04.08.2026).

### 3.5. Alibaba Qwen / Model Studio

| Model | In / Out (часто цитируемое) | Статус |
|---|---|---|
| Qwen3.7-Max | **$2.50 / $7.50**; cache ~$0.25 (90% off) | VentureBeat 02.06.2026 + multi reviews |
| Qwen3.7-Plus | **$0.40 / $1.60** | VB 02.06.2026 |
| OpenRouter Qwen3.7-Max | ~**$1.475 / $4.425** | OR page (reseller) |
| AI Token Plan | from **$6/mo** | Model Studio marketing |

**Регионы:** Singapore free quota for new users; also Beijing / HK / **Frankfurt** / Virginia endpoints.  
Singapore vs Hangzhou: разные base URL, free tier только SG; intl card на Alibaba Cloud international.  
OpenAI-compatible + **Anthropic path** в Model Studio: `.../apps/anthropic` (docs Model Studio models page, 15.07.2026).

### 3.6. ByteDance Doubao / Seed

- API: **Volcano Engine / Volcengine** (CN), intl сложнее (ID/enterprise).  
- Seed 2.0 series с 14.02.2026.  
- Для EU: чаще **агрегаторы** (OpenRouter/302/EvoLink) — middleware risk.  
- Прямой Doubao для нашей задачи **не рекомендуется** (friction > benefit).

### 3.7. Оплата из ЕС / free tier / KYC (сводка)

| Провайдер | Visa/MC из EU | Alipay/WeChat | Агрегатор | Free tier | KYC |
|---|---|---|---|---|---|
| **DeepSeek** api.deepseek.com | да (практика) | — | OpenRouter, Fireworks, etc. | chat free; API grant | email OK |
| **z.ai** | да (Coding Plan USD) | bigmodel.cn | OpenRouter | free chat + free Flash models | email |
| **MiniMax** platform.minimax.io | да (intl site) | CN side | OpenRouter | trial | email |
| **Kimi** platform.moonshot.ai / kimi.com | да (membership) | CN | OpenRouter | Adagio free | email |
| **Alibaba Model Studio** | Alibaba Cloud billing | CN | OR | SG free quota | Alibaba account + card |
| **OpenRouter** | **major cards + USDC + Alipay** | yes | — | limited free models | email; min top-up $5 |
| **Together / Fireworks / DeepInfra / Novita** | cards | — | — | trial varies | email |

OpenRouter FAQ: major credit cards, Alipay, USDC.5(https://openrouter.ai/docs/faq)  

### 3.8. История резких цен (риск)

- **GLM API:** в 2025–26 многократно менялся (сообщество помнит «$3 → $10 → $18»-wave на mid-tier; сейчас 5.2 = $1.4/$4.4 — **стабильнее, но Coding Plan quota multipliers меняются**).  
- **DeepSeek:** peak 2× **скоро**; aliases retired 24.07.2026.  
- **MiniMax:** M2.7 closed-weights episode post-IPO (апр 2026 commentary) — product policy volatility.

---

## 4. Совместимость и интеграция

### 4.1. Anthropic-compatible (drop-in Claude Code)

| Провайдер | Anthropic endpoint | OpenAI-compatible | Tool calling | Streaming | Prompt cache | Ctx practice |
|---|---|---|---|---|---|---|
| **DeepSeek** | ✅ **официально** `https://api.deepseek.com/anthropic` | ✅ | ✅ | ✅ | ✅ (hit rates в pricing) | 1M advertised |
| **Z.ai GLM** | ✅ Claude Code / ZCode docs | ✅ | ✅ | ✅ | ✅ | 1M на 5.2 |
| **Alibaba Qwen** | ✅ Model Studio `.../apps/anthropic` | ✅ `compatible-mode/v1` | ✅ | ✅ | ✅ | 1M Max |
| **MiniMax** | частично / via gateways | ✅ | ✅ | ✅ | cache pricing | 205K–1M |
| **Kimi** | via gateways / Kimi Code CLI | ✅ | ✅ | ✅ | ✅ | 256K–1M |
| **OpenRouter** | partial Anthropic-style routing | ✅ primary | ✅ | ✅ | provider-dep | varies |

**Claude Code 5 шагов — DeepSeek (официально):**

```bash
# 1. API key: platform.deepseek.com
# 2. env:
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=<DeepSeek_API_Key>
export ANTHROPIC_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
# 3. cd repo && claude
```

Источник: [DeepSeek Claude Code integration](https://api-docs.deepseek.com/quick_start/agent_integrations/claude_code/).

### 4.2. Latency / EU endpoints

| | EU region endpoint | Latency PT/DE (ориентир) |
|---|---|---|
| Alibaba | **Frankfurt** `eu-central-1` | лучший среди CN bigtech |
| DeepSeek | global `api.deepseek.com` (CN infra) | TTFT выше из EU; приемлемо для async review |
| z.ai | intl edge | medium |
| OpenRouter | multi-provider routing | зависит от upstream |
| MiniMax / Kimi | intl platforms | medium–high |

**Для async second opinion (не live pair-programming)** latency не блокер.

### 4.3. Локально: RTX 5060 Ti 16GB / 2× GPU

| Модель | 1×16GB | 2×16GB | Коммент |
|---|---|---|---|
| Qwen3.6-27B dense Q4/Q5 | ✅ | ✅ | лучший local coding sweet spot 2026 |
| Qwen3-Coder-Next 80B-A3B MoE Q4 | ⚠️ | ✅ | |
| GLM-5.x full | ❌ | ❌ | cloud only practically |
| DeepSeek V4 | ❌ | ❌ | no public weights |
| Kimi K2.x full trillion MoE | ❌ | ❌ | needs multi-node |
| MiniMax M2-class smaller | ⚠️ | ⚠️ | depends on released size |

**Вывод:** для «третьей пары глаз» **API > local** на вашем железе; local — только mid-size Qwen как offline fallback.

---

## 5. Риски (честно)

### 5.1. Юридика / запреты

| Юрисдикция | Что | Дата | Нам |
|---|---|---|---|
| **Италия** | Garante: ban processing Italian users' data (DeepSeek app/web) | **30.01.2025** | lab в PT — не Италия; всё же GDPR exposure |
| **Германия (Berlin DPA)** | unlawful data transfer to CN; pressure to remove apps | **06.2025** | не nationwide criminal ban API |
| **Австралия** | **gov devices** ban DeepSeek | **02.2025** | не private lab |
| **US states / federal gov / Navy** | gov & sensitive sector bans | 2025 | не ваш кейс, если private code |
| **ЕС resident → pay CN provider** | **не запрещено** per se; GDPR Art.44+ transfer rules apply if personal data | — | tech code without PII = ниже риск, **не ноль** |

Источники: Global Legal Insights DE 30.06.2025; Al Jazeera 06.02.2025; AI-regulation.com Jan 2026 retrospective.6(https://platform.minimax.io/subscribe/token-plan)

### 5.2. Данные / ToS / серверы

- Типичный CN provider ToS: prompts **могут** использоваться для training **unless** enterprise/opt-out.  
- **DeepSeek / z.ai / MiniMax:** free tiers = худший privacy posture; paid API = лучше, но **серверы в КНР** (или HK) по умолчанию.  
- **Alibaba Model Studio Singapore/Frankfurt** — лучше data residency narrative, всё равно Alibaba group.  
- **OpenRouter:** data also hits upstream CN model unless you choose EU-hosted replicas (редко для frontier CN).

**Для code review:** вы **фактически публикуете** фрагменты репозитория + промпт-контекст (пути, секреты в коде, бизнес-логику) **третьей стороне в КНР**. Это **не** open-source publish, но **это не «локально»**.

**Митигация AK-47:**
1. Scrub secrets / `.env` / credentials перед отправкой  
2. Только diff + minimal context, не весь vault  
3. Запрет PII / medical / lead CRM  
4. Отдельный API key + budget cap  
5. Лог: что ушло наружу  

### 5.3. Цензура vs code review

- Отказы: Taiwan/Tiananmen/Xi/politics, иногда «cybersecurity dual-use».  
- **На тех. code review** (Python/TS, architecture, tests) — **обычно не мешает**.  
- Риск: security tooling, scraping, auth bypass discussion → soft refuse.  
- Grok/Gemini тоже имеют safety layers; **разные** отказанные темы → полезно для diversity.

### 5.4. Устойчивость

- Резкие price changes (GLM history; DeepSeek peak TBA)  
- Alias deprecations (DeepSeek Jul 2026)  
- Foreign account freezes — rare but reported community-side  
- Model swap mid-plan (Coding Plan GLM-5 → 5.2 with **2–3× quota burn**)

### 5.5. Риск «отдаём код на ревью»

| Что уходит | Риск |
|---|---|
| Diff + file paths | reverse-eng product |
| Comments / TODOs / internal URLs | OSINT |
| Hardcoded secrets | **критично** |
| Prompt with vault rules | meta-leak |
| Полный repo zip | **недопустимо** |

---

## 6. Рекомендация под вашу задачу

**Критерии:** независимое 2-е мнение (код + стратегия), headless CLI/HTTP, **≤$25/мес**, карта из ЕС, **другой** взгляд чем Grok/Gemini.

### ТОП-3

#### 🥇 #1 — DeepSeek V4 via official Anthropic endpoint  
**Зачем:** лучший $/качество + **нативный** Claude Code drop-in + дешёвый burn на ежедневный second-pass.

| | |
|---|---|
| **Модель** | `deepseek-v4-pro` (тяжёлые ревью) + `v4-flash` (субагенты) |
| **Цена** | PAYG: Pro $0.435/$0.87; Flash $0.14/$0.28 → **$10–25/мес** при 2–5 review/day |
| **Подписка** | нет Claude-Max аналога; prepaid balance |
| **Отличие от Grok/Gemini** | Hangzhou lab, MoE/efficiency culture, другой pretrain corpus, сильнее «mathy» code paths; **не** Google search stack и **не** xAI style |

**5 шагов:**
1. `platform.deepseek.com` → API key, top-up $10–20 (Visa)  
2. Env vars из §4.1  
3. Wrapper script `ds-review.sh` (отдельный env, не ломает основной Claude)  
4. Промпт: «adversarial code review; list bugs, missing tests, simpler design»  
5. Лог spend + cap $25  

**Риск:** GDPR/DeepSeek bans narrative; peak 2× prices TBA; data → CN.

---

#### 🥈 #2 — Z.ai GLM Coding Plan Lite (~$12.6–18) + GLM-5.2  
**Зачем:** **настоящий coding-plan** (как Claude Max-lite), MIT open weights option, long-horizon agent engineering.

| | |
|---|---|
| **Модель** | GLM-5.2 (quota 1–3×) |
| **Цена** | Lite **~$12.6–18/mo** (promo); или PAYG $1.4/$4.4 |
| **Отличие** | Tsinghua/Zhipu lineage; «long-horizon engineering» bias; open weights → можно сверить с self-host later; **другой** error pattern vs DeepSeek |

**5 шагов:**
1. z.ai account + Coding Plan Lite (USD card)  
2. Docs DevPack / Claude Code: model `"GLM-5.2"` или `GLM-5.2[1m]`  
3. Peak hours Beijing 14–18: 2–3× quota — планировать EU morning  
4. Fallback free Flash for triage  
5. Monthly: 10 hard reviews + 1 strategy memo  

**Риск:** quota burn на 5.2; price/plan churn; CN infra.

---

#### 🥉 #3 — MiniMax Token Plan Plus **$20** *или* OpenRouter multi-route  
**Зачем:** сильный SWE-bench pedigree (M2.5/M2.7); $20 ровно в бюджет; альтернатива — OpenRouter как **единый EU billing** на Kimi-K2.6 + MiniMax + Qwen mid.

| | |
|---|---|
| **A: MiniMax Plus $20** | M3/M2.7 family, agent quotas |
| **B: OpenRouter $20 credits** | Visa, multi-model, 5.5% fee; switch model without new KYC |
| **Отличие** | MiniMax = agentic self-evolution narrative; OpenRouter = meta-diversity (разные labs в одном ключе) |

**5 шагов (OpenRouter path — самый низкий friction):**
1. openrouter.ai → Stripe Visa $20  
2. `ANTHROPIC_BASE_URL` / OpenAI client → OpenRouter  
3. Models: `minimax/minimax-m2.7` + `moonshotai/kimi-k2.6` + `deepseek/deepseek-v4-pro` rotate  
4. Script: randomize lab for blind second opinion  
5. Cap auto-disable at $20  

**Риск:** middleman ToS; MiniMax policy volatility post-IPO.

---

### Что НЕ брать

| Не брать | Почему |
|---|---|
| **Kimi K3 API** как daily | $3/$15 — сожрёт $25 за 1–2 тяжёлых сессии |
| **Qwen3.7/3.8-Max** daily | $2.5/$7.5; Token Plan reports «burn in 2h» |
| **Doubao/Seed direct** | CN KYC, console friction |
| **01.AI / Baichuan** | заглохли |
| **Только free chat** | нет API stability, training use of prompts |
| **bigmodel.cn without CN entity** | KYC/payment wall |
| **Отдавать полный vault/CRM** | data risk disproportionate |

### Diversity matrix (даст ли *другой* взгляд)

| | Grok | Gemini | DeepSeek | GLM | MiniMax/Kimi |
|---|---|---|---|---|---|
| Lab culture | xAI contrarian | Google scale | efficiency research | academic eng | product agents |
| Refusal style | loose | safety-heavy | CN political | CN + eng | CN + product |
| Code bias | general | multimodal/search | math/structure | long-horizon | agent loops |
| **Blind disagreement rate (hyp.)** | baseline | baseline | **high vs both** | **high** | **med-high** |

Для «третьей пары глаз» важна **не** максимальная Elo, а **некоррелированные ошибки**. DeepSeek + GLM дают максимальную некоррелированность с Grok+Gemini.

---

## 7. Сводная таблица

| Провайдер | Модель | $/мес | $/1M in | $/1M out | API-совм. | Оплата EU | Open weights | Главный риск |
|---|---|---|---|---|---|---|---|---|
| **DeepSeek** | V4-Pro | PAYG ~10–25 | 0.435 (0.0036 hit) | 0.87 | **Anthropic+OpenAI** | ✅ card | ❌ | GDPR/bans; peak 2× TBA |
| DeepSeek | V4-Flash | PAYG | 0.14 (0.0028 hit) | 0.28 | same | ✅ | ❌ | same |
| **z.ai** | GLM-5.2 | Lite 12.6–18 / PAYG | 1.4 (0.26 hit) | 4.4 | Anthropic+OpenAI | ✅ | ✅ MIT | quota×3 peak; price churn |
| **MiniMax** | M2.7/M3 | Plus **20** | ~0.30 | ~1.20 | OpenAI (+gateways) | ✅ | mixed | weights policy flips |
| **Moonshot** | K2.6 | PAYG / memb 19+ | ~0.95 | ~4.00 | OpenAI | ✅ | ✅ K2.x | K3 too expensive |
| Moonshot | **K3** | — | **3.00** (0.30 hit) | **15** | OpenAI | ✅ | ❌ | burn rate |
| **Alibaba** | Qwen3.7-Max | Token plan / PAYG | ~2.50 | ~7.50 | OpenAI+Anthropic | ✅ Alibaba | ❌ Max | cost; console UX |
| Alibaba | Qwen3.7-Plus | PAYG | ~0.40 | ~1.60 | same | ✅ | ❌ proprietary Plus | mid-tier quality |
| **OpenRouter** | multi | credits 5–25 | = upstream +fee | = upstream | OpenAI | ✅ Visa | n/a | middleman |
| ByteDance | Seed 2.0 | n/a easy | n/c intl | n/c | OpenAI via CN | ⚠️ | ❌ | KYC/console |
| Tencent | Hunyuan-3 | n/c | n/c | n/c | mixed | ⚠️ | partial | weak intl DX |
| Baidu | Ernie 5.x | n/c | n/c | n/c | mixed | ⚠️ | 4.5 Apache | CN-first |
| 01.AI / Baichuan | — | — | — | — | — | — | legacy | **dead for our use** |

`n/c` = не подтверждено официальным EN прайсом в этом dig.

---

## 8. Практический план для лабы (AK-47)

1. **Сейчас (≤1 день):** OpenRouter $10 + DeepSeek $10 prepaid — smoke test на 5 реальных диффах.  
2. **Неделя 1:** DeepSeek = primary Chinese second opinion в `secondop.py`-стиле (как Grok rail).  
3. **Неделя 2:** если DeepSeek «соглашается с Grok слишком часто» → добавить GLM Lite.  
4. **Не делать:** 4 китайских + 2 западных одновременно без rubric (шум).  
5. **Постоянно:** scrub secrets; budget hard-cap; quarterly legal recheck.

---

## 9. Что протухнет быстрее всего

| Что | Срок перепроверки |
|---|---|
| **DeepSeek peak 2× pricing effective date** | **еженедельно** до анонса; потом сразу |
| GLM Coding Plan list/promo + quota multipliers | **1–2 недели** / при каждой смене plan page |
| Kimi K3 open-weights vs API-only | **при следующем Moonshot blog** |
| Qwen3.8-Max vs 3.7-Max pricing | **ежемесячно** Model Studio pricing |
| MiniMax M3 license / weights status | **ежемесячно** |
| DeepSeek / CN bans в EU | **при новостях DPA**; иначе **раз в квартал** |
| SWE-bench / Arena positions | **раз в месяц** (official runs) |
| Anthropic-compat quirks (Claude Code version bumps) | **после каждого CC update** |

**Жёсткий next re-check date: 2026-09-01** (DeepSeek peak policy + Q3 pricing wave).

---

### 🎙 Майк
Берём **DeepSeek V4 Anthropic endpoint** как дефолт «китайская пара глаз» — единственный, кто одновременно дёшев, официально drop-in под Claude Code и реально другой lab. GLM Lite — backup, если нужен coding-plan UX. K3/Max — только точечные «тяжёлые» решения, не подписка на каждый день. Код наружу = секреты scrubbed, diff-only, hard cap $25.

### 🧒 Простыми словами
Китайские модели в 2026 уже **почти как взрослые** на коде, и часто **гораздо дешевле**. Для вас лучший «третий судья» — **DeepSeek**: подключается к Claude Code почти одной командой, стоит копейки, думает не как Grok и не как Gemini. Второе место — **GLM** (как абонемент в спортзал на код). Не стоит гнаться за самой дорогой Kimi K3 — она съест весь месячный бюджет за пару тяжёлых дней. Главное правило: **не отдавать пароли и весь сейф**, только кусочки кода на проверку.
