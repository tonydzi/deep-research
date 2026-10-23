# DR26-07-29-MACANTON-01-1302 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: Grok CLI (grok build, локальная рельса, веб-поиск включён) — аккаунт Антона
topic: DR26-07-29-MACANTON-01-1302 — контент-заводы на LLM-агентах, конвейер целиком
date_collected: 2026-07-31


note: "веб-DR на grok.com запрещён каноном /firefox §2 (AUP xAI) → рельса = локальный CLI. Цитаты НЕ проверены на момент сохранения."
---

Собираю доказательную базу по всем шести блокам: параллельный поиск по прод-конвейерам, таймаутам апрува, валидации детекторов, хранилищам, дистрибуции и AI-раскрытию.Открываю ключевые первоисточники и углубляю поиск по пробелам.# DR26-07-29-MACANTON-01-1302  
## Многоагентные контент-конвейеры: конвейер целиком  
**Дата сбора:** 2026-07-31 · язык: RU · источники EN/RU  

---

## 1. Резюме на 10 строк

1. **Прод-конвейеры с цифрами редки:** чаще есть инди-разборы (час работы, $0.80/пост) и маркетинг вендоров; мало независимых аудитов «выпуск × охват × доля ручных правок».  
2. **Шов writer→editor→publish в живых системах почти всегда держит человека на one-way door** (публикация, внешнее письмо, merge), а не «молчание = ок».  
3. **Default-approve по таймауту** в HITL-паттернах и ITSM/агентных гайдах **реже**, чем default-deny / end-run / exception-only; **данных о цене auto-publish контента почти нет**.  
4. **Ваш провал маркеров на baseline автора — не аномалия**, а норма: стилевые линтеры без hold-out и negative control ломают precision.  
5. **Детектор ≠ читатель:** люди ловят AI около шанса (~50–57%); маркеры «роботности» и «человек почувствовал» — разные оси.  
6. **Хранение:** Google Docs удобен человеку, плох роботу; надёжный контракт — **леджер состояний отдельно от текста**.  
7. **Reddit/Medium в 2026 — не «добавить рельс»:** Reddit = прогрев + 9:1 + риск shadowban; Medium = AI не в paywall / без disclosure = Network Only.  
8. **EU AI Act ст. 50 с 02.08.2026** для текста «public interest» требует disclosure **если нет** human review + editorial responsibility.  
9. **Честная AI-подпись** в полевых/экспериментальных работах чаще **режет engagement** (TikTok MSI; lab-эксперименты), механизм — не качество, а **слабее connection с автором**.  
10. **Три риска вашей схемы:** (а) 24ч silence→publish на публичные каналы; (б) анти-слоп без калибровки на автора; (в) 4–5/день + кросс-пост без native-адаптации + Reddit «с нуля».

---

## 2. Блок 1 — Кто построил многоагентный контент-конвейер

### 1.1 Живые / полуживые разборы (не лендинг)

| Система | Роли / швы | Что измерено | Уровень |
|---|---|---|---|
| **Louis Bouchard + Towards AI** (28.07.2026) | Nova (research agent) → **human guideline** → Brown (LangGraph writer) → reviewer/editor loop (свежий контекст) → **human final edit** → packaging/translation. Контракт = Markdown-файлы. | Было 2–3 дня на YouTube-релиз → **~5 часов** своей работы; агенты ~90% рутины, человек ~10% (тема, вкус, финал). Фиксированные 3 прохода review, не score-stop. | **ИЗМЕРЕНО** (self-report, один автор) [single-source] [emerging] |
| **Golden Corpus Content Pipeline** (15.02.2026) | Brief → overlap check → write+rules → images → SEO/AEO → social drafts → verification agent; human spot-check 1/5. | **161 пост**, 4 языка; API **~$0.80/пост**; human review 10–15 мин (или near zero при mature); vs $400–850 human team. | **ЗАЯВЛЕНО** / частично **МАРКЕТИНГ** (vendor blog) [single-source] [speculative] |
| **BoundaryML «AI Content Pipeline Revisited»** (17.02.2026) | Автоматизируют middle; **LinkedIn/email не auto-post** — human review на one-way door; «90% good enough». | Episode prep: 3–4 часа → **~10 мин** hands-off + human. | **ИЗМЕРЕНО** (self-report) [single-source] [emerging] |
| **X: @johniosifov** (27.07.2026) | Agent schedule → posts → PR → self-review → merge → publish; **0 manual posts 277 дней**; 153 bursts. | 201 followers; +27/7d; 1 968 PR. **Рост медленный** — constraint = reach, not agent. | **ЗАЯВЛЕНО** (founder metrics) [single-source] [emerging] · **контрарный кейс** к «human always approve» |
| **X: @FinnZacMolly / zagging.ai** (24.07.2026) | AI drafts article + PR as separate GitHub identity; **human merges** («The gate is the point»). | Нет объёма/охватов. | **ЗАЯВЛЕНО** [low-authority] |

**Цитата (Bouchard):** *«I can now research, write, and prepare a full YouTube release in about five hours of my own work. The same job used to consume two or three full days.»* — https://www.louisbouchard.ai/ai-agent-content-pipeline/ · 2026-07-28

**Цитата (Golden Corpus):** *«what takes a content team 3-5 days per post takes the agent the same day»* + таблица ~$0.80 API — https://goldencorpus.com/building/content-agent-pipeline · 2026-02-15

### 1.2 Вендоры / агентства (осторожно)

- **Jasper Content Pipelines / Agents** — «100+ agents», «7,500 product descriptions in 24 hours», «3x content production» на лендинге. **МАРКЕТИНГ**. https://www.jasper.ai/ · https://www.jasper.ai/content-pipelines  
- **StackAI / Naturaily / Mindstudio** — паттерны HITL и «content factory», без независимого аудита вашего класса. **МАРКЕТИНГ / ЗАЯВЛЕНО**.  
- **Spiral Scout** multi-agent case (Santa letters, CPQ) — production, но **не social content factory**. https://spiralscout.com/blog/missing-layer-ai-agent-production-runtime

### 1.3 Провалы и координация (важнее успехов)

| Находка | Уровень | Источник |
|---|---|---|
| **MAST**: 14 failure modes, 3 кластера (system design · inter-agent misalignment · task verification); inter-annotator **κ=0.88**; 150 traces | **ИЗМЕРЕНО** [established] | Cemri et al., arXiv:2503.13657, Mar 2025 — https://arxiv.org/abs/2503.13657 |
| Production multi-agent: fail rates **41–87%**, mostly **coordination**, not model | **ИЗМЕРЕНО** (meta/analysis claim) [emerging] | arXiv:2605.03310 abstract — https://arxiv.org/abs/2605.03310 |
| **Role boundary collapse** (orchestrator лезет в код); overhead creep на мелких задачах | **ЗАЯВЛЕНО** [single-source] | Medium, Yang Xu, 2026 — production lessons |
| «Coordination is **10× harder** than capability»; perfect 50× → format hallucination on 51st | **ЗАЯВЛЕНО** [low-authority] | Reddit/Facebook practitioner posts 2026 |
| Demo fails: partial failures, inconsistent state, no human checkpoints on irreversible steps | **ЗАЯВЛЕНО** | DEV.to, Mar 2026 — https://dev.to/wassimchegham/why-your-ai-agent-demo-falls-apart-in-production-1320 |
| Dirty input data > AI quality как #1 failure content pipeline | **ЗАЯВЛЕНО** [single-source] | Mindstudio, 2026-07-15 |

**Практика на X (30 дней):** повторяется правило *«An AI agent should never grade its own work»* (@zostaff, 26.06.2026) и *human merge* gate (@FinnZacMolly). Контрпример — @johniosifov: full auto 277 дней, но рост ~200 followers = сигнал, что **автономия ≠ дистрибуция**.

### 1.4 Деградация качества при росте velocity

- Маркетологические обзоры: quality > quantity; cannibalization reach при excess frequency (Facebook/LinkedIn practitioner consensus). **ЗАЯВЛЕНО**, не RCT «N постов/день → −X% ER».  
- **Не нашёл** независимого panel-study: *«при масштабе AI content factory engagement per post падает на Y%»* с методикой.  
- Graphite (2025): доля AI-статей в вебе выросла резко; это про объём в SERP, не про ваш social ER. https://graphite.io/five-percent/research/more-articles-are-now-created-by-ai-than-humans · 2025-10-01 [emerging]

### 1.5 Чего нет (блок 1)

- **Не нашёл** публичных P&L «агентство 50 постов/день, CAC, unsubscribe rate, human edit rate %» с аудитом.  
- **Не нашёл** open-source репо с **production** writer→editor→distributor + метриками >6 мес, кроме blog self-reports.  
- **Не нашёл** кейса «свернули конвейер из-за репутационного инцидента auto-publish» с цифрами.

---

## 3. Блок 2 — Окна апрува: «человек не бутылочное горлышко»

### 2.1 Паттерны таймаутов (контент + соседние домены)

| Паттерн | Default при молчании | Где | Уровень |
|---|---|---|---|
| **Durable wait + SLA + escalation** | Явно проектируется; «deadlines pass silently» = anti-pattern | Temporal HITL approvals, 16.07.2026 | **ЗАЯВЛЕНО** (pattern) [emerging] https://temporal.io/blog/human-in-the-loop-approvals |
| **Exception-only** (auto-approve unless flagged) | Approve | StackAI, 03.03.2026; только при mature validators | **ЗАЯВЛЕНО** [single-source] https://www.stackai.com/insights/human-in-the-loop-ai-agents-how-to-design-approval-workflows-for-safe-and-scalable-automation |
| **Sampled approvals** 5–20% low-risk | Mixed | StackAI | **ЗАЯВЛЕНО** |
| **Timeout → End run** | **Не publish** | Zapier Human-in-the-Loop (min 1 unit; minutes–weeks) | **ИЗМЕРЕНО** (product behavior) https://help.zapier.com/hc/en-us/articles/38731463206029 · ~2026 |
| **Timeout → auto-deny** | Reject | SAPL clinical HITL, PT120S, `noAutoApprove: true` | **ЗАЯВЛЕНО** (docs) https://sapl.io/guides/ai-hitl/ |
| **Default deny recommended** | Reject | agentic-patterns.com HITL | **ЗАЯВЛЕНО** [low-authority] |
| **AI auto-approve PR, human merges** | Approve review only | Ona.com story, 09.04.2026 | **ИЗМЕРЕНО** (team lead time claim) [single-source] https://ona.com/stories/auto-approving-low-risk-prs |
| **Publish content = high blast radius** | Supervised by default | StackAI matrix: «publishing content» → human | **ЗАЯВЛЕНО** |

**Ключевой конфликт с вашей схемой:** enterprise HITL для **внешних коммуникаций** почти всегда *supervised* или *exception-only с сильными валидаторами*, а не *silence = publish*. Default-approve по молчанию встречается, но **как зрелый режим после** логов/флагов/сэмплинга — не как day-1 для публичного контента.

### 2.2 Последствия auto-approve по таймауту (контент)

**Не нашёл** полевых цифр: «% отписок / complaints / rollbacks после silent auto-publish постов».  
Есть только косвенное:

- Temporal формулирует failure modes approval: unanswered, silent deadline, lost context — **без** stats качества контента.  
- OpenClaw-style: *«reviewer controls cadence rather than racing a timeout»* для irreversible — **ЗАЯВЛЕНО**.  
- SAPL: auto-deny на timeout в safety-critical — не контент.

→ **Пустой блок по цифрам последствий content auto-approve.** Не заполняю выдумкой.

### 2.3 Длительность окон

| Контекст | Окно | Источник |
|---|---|---|
| External email agent | 5–30 min | StackAI matrix |
| DB write / access | 15–120 min | StackAI |
| Zapier HITL | configurable minutes–weeks | Zapier docs |
| Clinical suspend | 120 s auto-deny | SAPL |
| Document approvals Temporal | multi-day capable durable | Temporal |
| **Ваши 24ч** | — | нет industry benchmark «24h content» [no source found for 24h as standard] |

### 2.4 «Согласен» vs «не увидел»

Механики из HITL-литературы (не social-content papers):

1. **ACK/read receipt** на пинг (не «отправили»).  
2. **Multi-channel notify + escalation** (другой approver), не auto-publish.  
3. **Evidence pack** 10–30 сек review (StackAI) — снижает «не открыл, потому что лень».  
4. **Propose ≠ commit** — state machine: `proposed → notified → acknowledged → decided → executed`.

**Не нашёл** paper с A/B «read receipt vs no» на content approval queues.

### 2.5 Ответ не в той системе (чат вместо Docs)

**Не нашёл** готового open-source «catch cross-channel reply» для Google Docs approval.  
Практики из соседних доменов (обобщение, **МОЁ МНЕНИЕ**):

- Единый **command surface**: approve/reject/edit только через бот-команды с ID поста (`+ #doc123` / `edit #doc123 …`).  
- Inbox-watcher на чат редактора: intent classifier «это про пост N?» → блок auto-timeout.  
- Таймаут **не срабатывает**, если есть *any* linked activity (comment, chat mention, doc open event).  
- Google Docs comments API + Drive activity — отдельный канал сигналов «человек трогал».

Без этого 24h silence = **ложный negative** (человек ответил в TG, пост ушёл).

### 2.6 Rubber-stamping / выгорание

| Факт | Уровень | Источник |
|---|---|---|
| Detection rates drop after **~60 min** continuous review; inspection **&lt;500 LOC/h** | **ИЗМЕРЕНО** (code review research, cited) [established in SE] | arXiv:2407.01407 cited in https://www.augmentcode.com/guides/deep-code-review-recall-vs-precision · 2026-06 |
| Rubber-stamp = approve without meaningful review; false sense of safety | **ЗАЯВЛЕНО** | minware, coderaven 2026-07-02 |
| Metrics: review load ratio, time-to-approve, comment density, override rate, escalation rate | **ЗАЯВЛЕНО** | Graphite/industry guides |
| Automation complacency: humans rubber-stamp agent suggestions | **ЗАЯВЛЕНО** | Augment Code guide |

**Перенос на контент:** 4–5 апрувов/день × длинные тексты → риск «штамп» реален; ловят **time-on-doc &lt; threshold**, zero edits, zero comments, approve latency &lt; 5s. **Не нашёл** content-specific rubber-stamp study.

### 2.7 Голос практиков (X, ~30 дней)

- @composedgee / AION (23.07.2026): *«Nothing goes public without a human clicking approve… There's no undo button on a bad tweet»* — **ЗАЯВЛЕНО**, product pitch [low-authority].  
- @crtrsai (26.07.2026): CMS **draft only**, human publish.  
- Контр: @johniosifov full auto — но другой risk appetite и slow growth.

---

## 4. Блок 3 — Валидация анти-слоп детекторов (не правила, а proof)

### 3.1 Как доказывают, что детектор «не врёт»

Стандарт (из detector literature, transferable на rule-based linter):

1. **Ground truth:** размеченный корпус human / AI / hybrid; лучше multi-annotator.  
2. **Precision / Recall / F1 / FPR / FNR** на hold-out.  
3. **Inter-rater agreement** (κ) на human labels — MAST κ=0.88 как образец rigor.  
4. **Stratified slices:** short vs long; native vs non-native; domain (tech/marketing); author-specific.  
5. **Adversarial / paraphrase stress tests** (Sadasivan et al.).  
6. **Calibration:** score vs true rate (для probabilistic detectors).

**Классика bias (старше 2024 — помечено):**  
Liang, Yuksekgonul, Zou (2023, *Patterns*): 7 detectors; TOEFL non-native **FPR 61.22%**; 97.8% flagged by ≥1; near-perfect on US 8th-grade essays.  
https://arxiv.org/abs/2304.02819 · HAI summary https://hai.stanford.edu/news/ai-detectors-biased-against-non-native-english-writers  
**ИЗМЕРЕНО** [established] · **источник 2023**.

Sadasivan et al. (2023): detectors not reliable under paraphrase/adversarial. https://arxiv.org/abs/2303.11156 · **ИЗМЕРЕНО** [established] · **старше 2024**.

### 3.2 Систематические ошибки

| Ошибка | Доказательство |
|---|---|
| Non-native / simplified English → false AI | Liang 2023 **ИЗМЕРЕНО** |
| Короткие тексты нестабильны | industry consensus; formal short-text FPR tables vary [mixed] |
| Technical / formulaic prose | higher false AI [emerging, partial] |
| **Author style overlaps «AI tells»** | ваш кейс + scholarly complaints about distinctive styles [emerging] |
| Detector agreement ≠ truth | tools disagree widely |

### 3.3 Как правильно валидировать **ваш** marker-linter (методики)

Ваш замер «у владельца X, а не Y чаще, чем у бота» — это **правильный negative control**, который большинство тулов **не делают**.

**Промышленный / research-grade каркас:**

| Шаг | Что делать | Зачем |
|---|---|---|
| **A. Author style baseline** | 200–500 **hand-written** постов автора (pre-AI или verified), per platform/lang | personal false-positive map |
| **B. Negative control** | прогнать linter; любой маркер с **freq ≥ threshold у human** → demote/remove/weight↓ | ровно ваш провал |
| **C. Positive control** | raw LLM drafts + known-slop set | recall |
| **D. Hold-out** | 20–30% baseline не трогать при threshold-tuning | против overfitting |
| **E. Matched pairs** | same topic: human vs bot | isolate style vs topic |
| **F. Inter-rater** | 2–3 человека: «звучит роботно?» κ | human perception label |
| **G. Live A/B** | PASS vs FAIL-rewrite vs short-degrade → ER, unsub, comments «AI?» | **business ground truth** |
| **H. Drift monitor** | monthly re-baseline; model/prompt changes | concept drift |

**Инструменты (частично):**  
- Custom scripts + labeled SQLite (ваш AK-47 путь).  
- Для ML-детекторов: open eval harnesses; **не** GPTZero-as-oracle.  
- Process evidence (Google Docs revision timeline) — ортогональная ось provenance, не style.  
- **Не нашёл** готового OSS «personal style baseline calibrator for anti-slop rules» под ваш стек. [no source found]

**МОЁ МНЕНИЕ:** вердикт PASS/WARN/FAIL у вас должен быть **author-relative**, не universal:  
`score = f(markers | baseline_author_RU, baseline_author_EN)`.

### 3.4 «Детектор сработал» vs «читатель заметил»

Это **разные вещи** — evidence:

| Исследование | Результат | Уровень |
|---|---|---|
| Jakesch et al. PNAS 2023 | Humans ~**50–52%** accuracy identifying AI self-presentations (≈ chance) | **ИЗМЕРЕНО** [established] · **2023** https://www.pnas.org/doi/10.1073/pnas.2208839120 |
| Fiedler 2025 (lecturers) | Humans 57% AI / 64% human; ≈ chance; pro AI texts &lt;20% correct | **ИЗМЕРЕНО** [emerging] |
| PSU lab (2024) | Humans ~**53%** | **ИЗМЕРЕНО** [single-source summary] |
| Scare et al. (grading) | **94%** AI submissions undetected by markers | **ИЗМЕРЕНО** [emerging] |
| Detectors | high FPR/FNR depending on slice | **ИЗМЕРЕНО** |

**Вывод:** ваш linter может FAIL-ить текст, который аудитория **не** сочтёт «роботом», и PASS-ить то, что «пахнет AI» по смыслу/пустым insight. Нужна **двойная метка:** style-linter + human-perception sample + live engagement.

---

## 5. Блок 4 — Хранение: Docs vs Notion vs Airtable vs git

### 5.1 Сравнение под «робот пишет → человек правит → робот забирает»

| Слой | Сильные | Слабые для конвейера | Уровень |
|---|---|---|---|
| **Google Docs** | UX редактора, comments, suggest mode, mobile | API: revisions merge/coalesce; hard multi-bot; comments ≠ version history | **ЗАЯВЛЕНО** + Drive docs |
| **Notion** | DB status fields + page body | API rate limits; block model awkward for full post round-trip | **ЗАЯВЛЕНО** |
| **Airtable** | Отличный **state machine** (status, SLA, assignee) | Плохое long-form editing | **ЗАЯВЛЕНО** |
| **Git (+ PR)** | Diff, blame, bot≠human identity, CI gates | Non-tech editor friction | **ИЗМЕРЕНО** в eng-контенте (zagging pattern) |
| **Markdown + DB** (Bouchard) | Inspectable stages, resume checkpoints | Нужен viewer для не-технаря | **ИЗМЕРЕНО** (self) |

**Паттерн-победитель в prod-агентах:**  
**леджер состояний (DB/Airtable/git meta) отдельно от body (Docs/MD).**  
Иначе коллизии «робот перезаписал правку человека».

### 5.2 Google Docs API конкретно

Из Drive API docs (2026):  
- Revision history exists; **editor revisions may be merged** — API «might not show all changes».  
https://developers.google.com/workspace/drive/api/guides/change-overview · updated 2026-04-20  
**ИЗМЕРЕНО** (platform behavior).

Практика детекта human vs bot (составная):

1. **Отдельный service account** пишет черновик; редактор — **личный Google user**.  
2. `lastModifyingUser` / revision `lastModifyingUser` ≠ bot SA → human touch.  
3. Comments API — human signals (resolve, reply).  
4. **Не полагаться** на «весь diff» как на полный audit (coalescing).  
5. Education tooling (Draftback-class): process timeline (paste bursts vs typing) — для provenance, не для style. [mixed sources]

**Грабли:**  
- Два бота одновременно → last-write-wins.  
- Suggest mode vs direct edit — разный retrieval path.  
- Comments **не** в version history (Google support threads).

### 5.3 Чего нет

- **Не нашёл** benchmark «Docs vs Notion for AI editorial loop: conflict rate, lost edits %».  
- **Не нашёл** production open recipe «idempotent Doc pull with optimistic locking» с цифрами.

**МОЁ МНЕНИЕ (AK-47):**  
`posts` table: status, approval_deadline, last_human_touch_at, body_hash, source_doc_id;  
Doc = human canvas only; publisher reads **только** из ledger after `status=approved|timeout_policy`.

---

## 6. Блок 5 — Дистрибуция: квоты, Reddit, Medium, disclosure rules

### 6.1 Практические потолки (не hard API limit, а «душит охват»)

| Площадка | Рабочий темп (practitioner/marketing data) | Подтверждение |
|---|---|---|
| **Facebook Page** | Часто **1–2/день** brands; больше → cannibalization/fatigue | Sprout Social 2026: «1–2×/day… more → algorithm fatigue» **ЗАЯВЛЕНО** [emerging] https://sproutsocial.com/insights/best-times-to-post-on-facebook/ |
| **FB Profile** | Зависит; excess mediocre → reach drop (anecdotal groups) | **ЗАЯВЛЕНО** [low-authority] |
| **X** | API/automation risk + quality; hard public «N/day throttle table» for organic | **Не нашёл** stable official organic cap table 2026 |
| **Telegram channel** | Высокий volume tolerated vs FB; hard algo-throttle table | **Не нашёл** reliable measured throttle study |
| **LinkedIn** | **2–5/week** sweet spot (Buffer 2M+ posts); company 3–5/week; 2× in 24h cannibalizes (practitioner claim) | Buffer 28.08.2025 **ИЗМЕРЕНО** https://buffer.com/resources/how-often-to-post-on-linkedin/ · Hootsuite 2026 **ЗАЯВЛЕНО** |
| **Reddit** | Не «N/day», а **9:1 participation** + subreddit gates | см. ниже |
| **Medium** | Quality/distribution gates, не count/day | policy |

**Конфликт источников LinkedIn:** Buffer говорит frequency compounds; другие — не 2×/24h. **Не усредняю:** для small lab безопаснее **≤1/day LI**, 2–5/week.

### 6.2 Ссылки, кросс-пост, автоматизация

- **External links:** платформы historically deprioritize outbound — **заявлено** в industry blogs, **не** единый Meta whitepaper 2026 с %.  
- **Cross-post identical:**  
  - Cross-platform fingerprint sharing: **нет** (Socialync 2026) **ЗАЯВЛЕНО** [single-source] https://www.socialync.io/blog/avoid-content-duplication-penalties-cross-posting-2026  
  - **Но** native format / watermarks / same caption → low engagement loop.  
  - Inside Facebook multi-group same post → «Limited originality» anecdotes. **ЗАЯВЛЕНО** [low-authority].  
- **Automation:** platform ToS + fingerprint; ваши Reddit bans согласуются с risk model, не с одной «post count» цифрой.

### 6.3 Reddit 2026: вход с нуля

| Тема | Факт | Уровень |
|---|---|---|
| **9:1 / 10% self-promo** | Admin/mod guideline: for every 1 self-promo, **9** other posts/comments non-promo; not hard law | **ЗАЯВЛЕНО** (official-ish 2014 clarification still cited) https://www.reddit.com/r/modnews/comments/2oamgp/ · **старше 2024**, but still referenced 2025–26 |
| **Account age / karma** | Per-sub AutoMod: often **7–30 days**, **50–500+ karma**; large subs higher | **ЗАЯВЛЕНО** 2026 guides [emerging] |
| **Shadowban** | Invisible posts; check logged-out / r/ShadowBan | established practice |
| **Appeal** | reddit.com/appeals; ≤1/day; responses hours→**months**; success anecdotal, no official rate | **ЗАЯВЛЕНО** r/ShadowBan wiki/threads; marketing blogs claim «relatively high» for FPs — **[low-authority], treat as unverified %** |
| **What bans** | spam ratio, multi-post same link, new account promo, ban evasion, automation signals | **ЗАЯВЛЕНО** |

**Процедура прогрева (синтез guide-ов, не magic):**  
1) 30+ дней organic comments only;  
2) karma in target subs;  
3) &lt;10% promo lifetime;  
4) never VPN/ban-evasion;  
5) value-first posts before link;  
6) logged-out checks weekly.

**Вероятность разбана:** **нет** официальной цифры. Community: weeks–months of daily appeals. **Не придумываю %.**

### 6.4 Medium 2026

Официальная AI policy (проверено):

- **AI-generated writing** (majority AI, little edit) — **нельзя paywall** в Partner Program (disclosed or not).  
- **Undisclosed AI** → **Network Only** distribution (followers only).  
- AI-assist must be **labeled**; grammar/outline/fact-check tools — no disclosure needed.  
- Disclosure example: first 2 paragraphs: *“This story was written with the assistance of an AI writing program.”*  
https://help.medium.com/hc/en-us/articles/22576852947223-Artificial-Intelligence-AI-content-policy  

**ИЗМЕРЕНО** как platform rules [established for Medium].  
Publications как bootstrapping audience — **ЗАЯВЛЕНО** в writer blogs, без единой success-rate table.

### 6.5 Раскрытие AI: платформы + EU AI Act ст. 50

**EU AI Act Article 50** — entry **2 August 2026** (Art. 113):  
https://artificialintelligenceact.eu/article/50/

| Кто | Обязанность |
|---|---|
| **Providers** of systems generating synthetic text | Mark outputs **machine-readable / detectable** as AI (technically feasible) — Art.50(2) |
| **Deployers** of AI that generates/manipulates **text published to inform the public on matters of public interest** | **Disclose** AI generation/manipulation — Art.50(4) |
| **Исключение** | AI content underwent **human review or editorial control** AND a natural/legal person holds **editorial responsibility** |
| Deepfakes (image/audio/video) | separate disclosure rules |

**К вашему кейсу [emerging legal interpretation, not legal advice]:**  
- Личный блог/канал «лабораторный build-in-public» может попадать под «public interest» **узко** — зависит от content (новости/общество vs diary).  
- Если **человек-редактор** реально правит/отвечает и **держит editorial responsibility** + timeout auto-publish **без** review — исключение **слабее** (нет human review на конкретный item).  
- Machine-readable watermarking — обязанность **провайдера модели**, не вашего publisher-бота, но deployers disclosure — ваша зона.

**Платформы:** Medium — см. выше; Meta/TikTok/YouTube — labeling policies (MSI cites 2023–24). **Не нашёл** 2026 hard ban AI text on TG/X personal accounts as such.

---

## 7. Блок 6 — Честная AI-подпись: замеры

### 7.1 Главные исследования

| Работа | Дизайн | Результат | Уровень |
|---|---|---|---|
| **Carney, Riveros, Tully — MSI 24-154** (2024, TikTok field + experiments) | 1.1M+ posts, AIGC label; preregistered experiments | **Disclosure ↓ engagement**; not via quality/deception — via **↓ connection to creator** | **ИЗМЕРЕНО** [emerging→established] https://www.msi.org/working-paper/made-with-ai-consumer-engagement-with-media-containing-ai-disclosures/ · PDF MSI_Report_24-154 |
| **Seeger et al. 2026** | 2 experiments n=325, n=371 | AI-generated/enhanced labels ↓ affective & behavioral engagement, esp. emotional content; late disclosure helps enhanced, **not** fully generated | **ИЗМЕРЕНО** [emerging] Springer |
| **Chen et al. 2025** short-form video | experiment | **Direct positive** on engagement intention **and** negative via ↓ perceived quality (mediation conflict) | **ИЗМЕРЕНО** [emerging] — **несогласованность** с MSI |
| **Lin & Zhang JCOM 2026** | experiment n=433 | Labels may have unintended credibility effects in misinfo context | **ИЗМЕРЕНО** [emerging] |
| **Wang et al. 2026 TikTok deepfakes** | 787 videos field | Inauthenticity ↓ engagement; **label moderates** (softens penalty) | **ИЗМЕРЕНО** [emerging] — **другой** контекст (deepfake) |

**Сохраняю разногласие:**  
- **Сторона A (MSI/Seeger):** label hurts engagement / connection.  
- **Сторона B (Chen):** disclosure can raise engagement intention while lowering quality perception.  
- **Сторона C (deepfake field):** label helps when content already looks fake.  

Для **текстовых AI-каналов** ближе A; B — short-form video China/context may not transfer.

### 7.2 Формулировки раскрытия

- Seeger: timing early vs late matters for **AI-enhanced**, not full AI. **ИЗМЕРЕНО**.  
- Medium recommends dry first-paragraph sentence.  
- **Не нашёл** RCT «сухая плашка vs “я синтетический, дайте фидбек”» на TG/X с ER delta. [no source found]

**МОЁ МНЕНИЕ:** для tech audience «synthetic cofounder + ask feedback» может **смягчить** connection-penalty MSI; для mass RU FB — риск выше. Нужен **ваш** A/B, не перенос TikTok.

### 7.3 Не раскрыли → разоблачили

- **Не нашёл** large-scale field study «undisclosed AI → audience discovery → unsub cascade %» с контролем.  
- Medium: undisclosed → Network Only (platform penalty, not audience). **ИЗМЕРЕНО** (policy).  
- Trust literature: deception backlash stronger than honest low quality — **theory**, thin field numbers for social posts.

### 7.4 Площадки / аудитории

- Tech vs mass: systematic comparative **не найден** beyond fragments.  
- Emotional content more sensitive to AI labels (Seeger). **ИЗМЕРЕНО**.

### 7.5 X (последние недели)

Слабый сигнал: жалобы «captions robotic / AI» (@SilverShadesx 31.07.2026) — anecdotal [low-authority]. Недостаточно для claims.

---

## 8. Три вещи в **вашей** схеме, которые выглядят ошибкой

### Ошибка 1 — **24 часа молчания → автопубликация** как принцип «человек не bottleneck»

**Почему спорю:**  
HITL-паттерны для **customer-facing / publish** (StackAI, Zapier end-on-timeout, SAPL deny, Temporal escalation) почти всегда трактуют молчание как **«не безопасно продолжать»**, а не «одобрено». Exception-only auto-approve допускается **после** зрелых валидаторов + audit sample — у вас gate 2% PASS на 221 черновике = **не mature**.

**Риск:** один токсичный/фактический косяк + timeout = репутация; rubber-stamp editor выгорит, а auto path унесёт мусор.  
**Контрсталь:** @johniosifov full auto 277d — но growth bottleneck и другой brand risk.

**Лучше (из найденного опыта):**  
timeout → **hold / escalate / degrade-to-draft-queue**, не public; **или** auto только для tier-0 (short, no links, no claims) после 30 дней shadow metrics.

### Ошибка 2 — **Универсальный anti-slop как quality gate** без author-relative calibration first

Вы уже **опровергли** detector на baseline владельца — это золотой negative control.  
Продолжать FAIL→rewrite по маркерам, которые **чаще у живого автора**, = optimize for wrong objective (detector score ≠ audience).

**Правильно:** перекалибровать веса на baseline; ground truth = human «robotic?» + live ER, не PASS rate 2%.

### Ошибка 3 — **План Reddit/Medium «рельс» + 4–5/день multi-platform same voice**

- Reddit: два shadowban уже = сигнал; re-entry = **недели/месяцы** 9:1, не «подключить API». Appeal success rate **unknown**.  
- Medium: majority-AI **не монетизируется**; без label — Network Only.  
- FB 1–2/day brands; LinkedIn 2–5/week; identical cross-post → engagement tax даже без hard ban.  
Ваши 4–5/day на RU+EN × 3 platforms = **velocity выше** typical healthy page cadence на Meta/LI.

**Четвёртый мягкий риск (бонус):** Google Doc as SoT без ledger — lost human edits / false timeout.

---

## 9. Чего я **не** нашёл (сводка пробелов)

1. Независимый ROI multi-agent social content factory (posts/day × ER × cost × human edit %).  
2. % quality/reputation damage from **content** silence-auto-approve.  
3. Official Reddit shadowban appeal success rate.  
4. Hard organic post/day throttle tables for TG/X/FB from the platforms.  
5. RCT dry vs personal AI disclosure on Telegram/Facebook text channels.  
6. Field study «audience discovered undisclosed AI → unsub %».  
7. Production open system «cross-channel approval catcher» (chat reply blocks Doc timeout).  
8. Longitudinal proof «more AI posts → worse per-post metrics» with controls.

---

## 10. Решения: опции и рекомендация

### Опция A — **Supervised ship (консервативно)**  
Human must approve; timeout → park, not publish. Shadow 14–30d.  
- **+** согласуется с HITL/production agent practice; EU Art.50 editorial exception крепче.  
- **−** editor bottleneck (но у вас 4–5/day × 24h window уже не «всегда online»).

### Опция B — **Tiered autonomy (рекомендую)**  
| Tier | Критерии | Timeout policy |
|---|---|---|
| 0 | short, no links, no numbers, PASS+author-calibrated, low risk | auto after 24h **если** ACK ping (read) OR second reminder at 12h |
| 1 | WARN, links, claims | never auto; escalate |
| 2 | FAIL after 2 rewrites / degraded short | human only or drop |

Ledger ≠ Doc. Cross-channel reply watcher. Sample 10% auto for audit.  
- **+** «человек не bottleneck» **без** слепого default-approve.  
- **−** сложнее; нужна классификация tier.

### Опция C — **Full auto like @johniosifov**  
Publish without human.  
- **+** max velocity.  
- **−** contradicts most HITL guidance; your anti-slop FPR; Medium/Reddit suicide; disclosure/trust risk.

### Опция D — **Reddit/Medium later**  
90 days warm organic Reddit; Medium only human-heavy longreads with disclosure, no Partner paywall AI.  
- **+** stops repeating ban.  
- **−** slower distribution.

---

### Рекомендация

**Опция B (tiered autonomy) + D (Reddit/Medium delayed) + author-calibrated linter rebuild.**

- **Confidence:** **[emerging]**  
- **Главная assumption:** ваш риск — **репутация/бан/доверие**, не «не успеть 5 постов»; anti-slop 2% PASS + 40% FAIL означает, что **exception-only auto-publish premature**.  
- **Первый эксперимент на 14 дней shadow→limited live:**  
  1) recalibrate markers on 100+ human posts;  
  2) tier-0 only auto;  
  3) A/B disclosure on 50% EN X posts (MSI hypothesis: −connection);  
  4) measure: edit rate, timeout rate, ER, «AI?» comments, unsub.  

**Не делать сейчас:** silent 24h approve на всё; Reddit re-entry automation; Medium Partner moneti­zation на AI-majority.

---

## 11. Первичные источники (URL + дата; verbatim-ready)

1. https://www.louisbouchard.ai/ai-agent-content-pipeline/ — 2026-07-28 — *«five hours of my own work… two or three full days»*  
2. https://goldencorpus.com/building/content-agent-pipeline — 2026-02-15 — 161 posts, ~$0.80  
3. https://arxiv.org/abs/2503.13657 — Cemri et al. MAST, Mar 2025 — 14 failure modes, κ=0.88  
4. https://arxiv.org/abs/2605.03310 — multi-agent fail 41–87% coordination  
5. https://temporal.io/blog/human-in-the-loop-approvals — 2026-07-16  
6. https://www.stackai.com/insights/human-in-the-loop-ai-agents-how-to-design-approval-workflows-for-safe-and-scalable-automation — 2026-03-03  
7. https://help.zapier.com/hc/en-us/articles/38731463206029 — HITL timeout end run  
8. https://sapl.io/guides/ai-hitl/ — timeout auto-deny  
9. https://ona.com/stories/auto-approving-low-risk-prs — 2026-04-09  
10. https://arxiv.org/abs/2304.02819 — Liang et al. 2023 FPR 61.22% non-native **(старше 2024)**  
11. https://hai.stanford.edu/news/ai-detectors-biased-against-non-native-english-writers — 2023-05-15 **(старше 2024)**  
12. https://www.pnas.org/doi/10.1073/pnas.2208839120 — Jakesch 2023 humans ~50% **(старше 2024)**  
13. https://developers.google.com/workspace/drive/api/guides/change-overview — 2026-04-20 revisions may merge  
14. https://sproutsocial.com/insights/best-times-to-post-on-facebook/ — FB 1–2/day  
15. https://buffer.com/resources/how-often-to-post-on-linkedin/ — 2025-08-28 LI 2–5/week  
16. https://www.reddit.com/r/modnews/comments/2oamgp/ — 10:1 clarification **(2014, still cited)**  
17. https://help.medium.com/hc/en-us/articles/22576852947223-Artificial-Intelligence-AI-content-policy — Medium AI rules  
18. https://artificialintelligenceact.eu/article/50/ — EU AI Act Art.50, applies 2026-08-02  
19. https://thearf-org-unified-admin.s3.amazonaws.com/MSI_Report_24-154.pdf — MSI Made with AI, 2024 — disclosure ↓ engagement via connection  
20. https://www.msi.org/working-paper/made-with-ai-consumer-engagement-with-media-containing-ai-disclosures/ — MSI landing  
21. https://link.springer.com/article/10.1007/s12525-026-00883-2 — Seeger 2026 labeling ↓ engagement  
22. https://www.augmentcode.com/guides/deep-code-review-recall-vs-precision — 2026-06 fatigue ~60 min  
23. https://boundaryml.com/podcast/2026-02-17-automating-aitw — 90% automate, human on one-way doors  
24. https://www.jasper.ai/content-pipelines — marketing reference  
25. X @johniosifov 2026-07-27 — 277 days full auto, 201 followers (contrarian)  
26. X @FinnZacMolly 2026-07-24 — human merge gate  
27. X @zostaff 2026-06-26 — agent never grades own work  
28. https://www.socialync.io/blog/avoid-content-duplication-penalties-cross-posting-2026 — 2026-01-24 cross-post claims  

---

### 🎙 Майк (кофаундер-голос)
Вы построили **правильную форму** конвейера (gate → rewrite → human window → distribute), но **default-approve 24h** и **некалиброванный linter** — это два места, где чужой prod-опыт орёт «стоп». Не ускоряйте ship; ускоряйте **доказательство**, что auto-tier не стреляет себе в ногу. Shadow уже тикает — используйте его на tier-calibration, не на «ещё агентов».

### 🧒 Простыми словами
Представь конвейер булочек: робот месит, робот печёт, человек пробует, потом продают.  
Если человек 24 часа молчит и булочки **сами** едут в магазин — иногда продадите нормальное, иногда — сырое, и покупатели больше не придут.  
Умные пекарни: молчание = **не продаём**, а откладываем.  
И «детектор сырости» надо настраивать на **ваш** вкус, а не на чужой учебник — вы уже видели, что «сырое» по учебнику у вас бывает в **живых** булочках.
