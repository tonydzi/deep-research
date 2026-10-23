# DR26-08-04-ZB-09-1803 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Провожу глубокое исследование механики попадания в ответы AI-движков: сначала подниму известное по теме, затем сверю с свежими источниками по стадиям краул→цитата.# Протокол попадания в ответы AI-движков  
## Малая независимая лаборатория · evidence review + playbook

**Дата сборки:** 2026-08-04  
**Вердикт по гипотезе (сразу):**

| | |
|---|---|
| **Гипотеза** | «Планка входа — не известность org, а атрибутируемый артефакт на поверхности с авторитетом индекса (GitHub, awesome-листы); сайту вторично» |
| **Вердикт** | **Частично подтверждена** для *developer / open-source / “what tools exist”* запросов; **не универсальна** для *“does org X exist / who is company Y”* |
| **Уверенность** | **medium–high** на OSS-вертикали (ваши замеры + earned-media bias); **medium** на entity-verification; **low** на точные сроки до первой цитаты |

Ваши данные **не опровергают** гипотезу на профильном вопросе (Perplexity: 10/10 GitHub/awesome/Microsoft/AI Alliance; сайт 0). Они **опровергают** наивную версию «достаточно robots + llms.txt + crawler hits»: **13 845 AI-краулов при 0 в индексе Bing/DDG = краул ≠ индекс ≠ retrieval ≠ цитата**.

---

## 1. Механика по стадиям

### 1.1 Общий конвейер (все движки)

| Стадия | Что происходит | Что решает переход | Типичный fail малой lab |
|---|---|---|---|
| **A. Crawl** | Бот забирает HTML | robots allow, HTTP 200, ссылки/sitemap, crawl budget | **Не ваш fail** — у вас PerplexityBot/ClaudeBot/GPTBot уже ходят |
| **B. Index** | URL попадает в поисковый/проприетарный индекс | качество + **внешние ссылки** + trust domain + freshness | **Ваш fail №1** — 0 в Bing/DDG |
| **C. Retrieval** | По запросу вытаскивают кандидатов (10–30) | релевантность query↔passage + authority домена | Малый сайт не в top-N retrieval set |
| **D. Rerank / select** | Из кандидатов оставляют 3–10 для ответа | trust, freshness, answer-fit, diversity, anti-spam | Есть в индексе, но не цитируют |
| **E. Generation + cite** | LLM пишет ответ и вешает attribution | passage usable as evidence; entity disambiguation | «Организации нет» / чужой бренд с тем же именем |

**Ключевой established-факт:** visibility в generative engines — это не rank в SERP, а *impression внутри синтезированного ответа* (длина/позиция/влияние цитаты).

---

### 1.2 По движкам: общий слой vs свой

| Движок | Краул | Индекс | Retrieval backend | Что официально / замерено | Статус |
|---|---|---|---|---|---|
| **Perplexity** | **Свой** PerplexityBot | **Свой** (+ Bing как long-tail fallback в industry writeups) | hybrid lexical+embedding → rerank → Sonar | Собственный crawler+index; ответ с inline citations | **Emerging** (архитектура из industry/engineer writeups; официальный whitepaper end-to-end не найден) |
| **ChatGPT Search** | **Не свой** (на retrieval) | В основном **Bing** + partner content | query rewrite → search provider → select cite | OpenAI (31 Oct 2024): *«third-party search providers, as well as content provided directly by our partners»* · Seer Interactive (Feb 2025): **87%+** SearchGPT citations match Bing top organic · Blyskal (Jul 2025): overlap с Bing/Google **сдвигается во времени** (не фиксирован навсегда) | **Established** (Bing-зависимость исторически); **emerging** (доля Bing vs других — дрейфует) |
| **Gemini / AI Overviews** | **Googlebot** | **Google index** + Knowledge Graph | Grounding with Google Search | Google docs: grounding → inline citations from Search results | **Established** |
| **Claude + web search** | **Не свой** | **Brave Search** (не Google/Bing) | tool `web_search` → results → cite | Anthropic docs: search tool + citations · TechCrunch 21 Mar 2025: Brave в subprocessor list + `BraveSearchParams` + совпадение citations с Brave | **Established** (Brave backend, 2025); детали rerank — **emerging** |

#### Переходы «что именно двигает»

| Переход | Perplexity | ChatGPT Search | Gemini | Claude web |
|---|---|---|---|---|
| Crawl→Index | свой краул + (часто) сигналы trust; industry: custom index ~миллиарды URL | **не ваш сайт-краул OpenAI**, а **индекс Bing** | Google quality + links | Brave index quality + links |
| Index→Retrieval | lexical+semantic match к query | Bing ranking top-N | Google ranking + KG entities | Brave ranking top-N |
| Retrieval→Cite | authority, freshness, structure, diversity; часто **GitHub/Reddit/Wikipedia** в source mix | select ~15% retrieved (industry claim — **emerging**); earned bias | grounding + authority; entity «exists?» | conservative; tight to Brave top |

**Практический вывод для вас:**  
- **Сайт + llms.txt** влияет на A (crawl), почти не на B–E.  
- **Bing index = 0** режет ChatGPT Search и часть Perplexity long-tail.  
- **Google index = 0 / слабый** режет Gemini.  
- **Brave index = 0** режет Claude web.  
- **GitHub** уже в «авторитетном индексе» → короче путь B→E на OSS-запросах. Это **согласуется** с вашим замером Perplexity 10/10.

---

## 2. Что повышает шанс цитирования (ранг по силе)

**Легенда статуса:**  
- **E** = established (peer-reviewed / vendor docs / крупные измеряемые corpus studies)  
- **M** = emerging (industry A/B, SEO-panels, arXiv без KDD-level peer review)  
- **S** = speculative / folklore  
- **N** = negative evidence (доказано, что **не** помогает или нет эффекта)

| # | Фактор | Сила (1–10) | Статус | Источник · дата · что взято | Комментарий для lab |
|---|---|---|---:|---|---|
| 1 | **Earned media / 3rd-party authority** (обзоры, roundups, awesome, trade press) | **9** | E/M | Chen et al. arXiv:2509.08919, Sep 2025: AI Search **overwhelming bias to Earned** vs Brand-owned; strategy = dominate earned media + big-brand bias for niche | Awesome-листы = earned **на домене github.com** — объясняет ваш 10/10 |
| 2 | **Быть в top retrieval set авторитетного индекса** (Bing/Google/Brave/GitHub) | **9** | E | Bing help: new site без ссылок часто не находится; ChatGPT↔Bing overlap studies 2025 | Ваш 0 в Bing = structural block для ChatGPT |
| 3 | **Wikipedia / encyclopedic entity** | **8** | E/M | Multi-engine citation studies 2025–2026: Wikipedia top-1/top-3 (Profound Jun 2025: ChatGPT Wiki **7.8%**; Similarweb/5WPR Q1 2026: Wiki+Reddit >25% ChatGPT US) | Для org «верифицируема» — сильный, но **порог входа высок** |
| 4 | **Reddit / UGC discussion of the artifact** | **8** | M | Perplexity Reddit-heavy; multi-engine Reddit #1 in several 2026 panels | Работает как *mention surface*, не как «ваш сайт» |
| 5 | **GitHub repo / org as citable artifact** | **8** | M | GitHub в top-20 cited domains (5WPR Q1 2026 ~1.6%; industry indices); **ваш замер** Perplexity 10/10 GH/awesome | Для OSS-governance вопросов — **главный канал** |
| 6 | **Контент-форма: stats + quotes + external citations** | **7** | E | Aggarwal et al. GEO, KDD 2024 / arXiv 2311.09735: visibility **up to ~40%**; cite/quote/stats strongest among tested | Работает **если** страница уже в retrieval set |
| 7 | **Machine-scannable structure** (прямые ответы, tables, clear claims) | **7** | M | CMU AutoGEO arXiv:2510.11438, Oct 2025: cooperative/semantically explicit content preferred; industry summaries cite 35–60% lift | README с TL;DR + comparison table |
| 8 | **Упоминания на HN / arXiv / high-trust forums** | **6** | M | arXiv 2506.12643 (Jun 2025): HN posts → spikes stars/forks/contributors on GH AI projects | Доказан **GitHub activity**, не AI-citation напрямую |
| 9 | **Свежесть (update dates, recent commits)** | **6** | M | Industry Perplexity guides: refresh top sources 24–72h (не peer-reviewed) | Полезно для freshness bias; не silver bullet |
| 10 | **Stars / popularity signals** | **5** | S/M | Корреляция popularity↔visibility вероятна; **прямого causal paper «stars → AI cite» не найдено** | Stars = proxy attention, не ranking factor с формулой |
| 11 | **Awesome-list inclusion** | **7** (внутри OSS) | M | Механически = earned link на high-trust domain; согласуется с вашим Perplexity run | **Сильнее**, чем «N stars без списков» |
| 12 | **Schema.org Organization** | **4** | S | Полезно для Google entity understanding (SEO-канон); **нет** публичного RCT «schema → LLM cite org exists» | Делать дёшево; не ждать чуда |
| 13 | **llms.txt** | **1–2** | **N** | Ahrefs Jun 2026: 97% llms.txt never read · SE Ranking Nov 2025: no correlation with citations · Otterly Feb 2026: 0.1% bot traffic to llms.txt · Contentful Jun 2026: no validated lift | **Совпадает с вашими 13k crawls + 0 citations** |
| 14 | **CITATION.cff** | **2** | S | Стандарт academic citation metadata на GH; **нет** evidence для LLM answer engines | Ок для учёных; GEO-эффекта нет в literature |
| 15 | **Только собственный сайт + robots allow** | **2** без index/links | E (neg) | Bing: *no links → hard to discover*; ваши 0 pages indexed | Необходимое, не достаточное |

### Что **доказано исследованием** vs **фольклор**

| Доказано (E/M) | Фольклор / недоказано (S) | Опровергнуто/пусто (N) |
|---|---|---|
| Earned > brand-owned в AI answers | «llms.txt = ticket to citations» | llms.txt → citation lift |
| Form (stats/quotes/cites) ↑ visibility if retrieved | «CITATION.cff = AI SEO» | — |
| Wiki/Reddit/YouTube disproportionate share | «N stars guarantee cite» | — |
| Backend indices: Bing / Google / Brave / Perplexity-own | «Schema Organization alone verifies lab» | — |
| Big brand bias / major labels default | «Name coinage works in 2 weeks» | — |

---

## 3. Как движки решают, что организация **существует**

Gemini-ответ *«no verifiable independent lab operating under that name or domain»* — это **entity verification / anti-hallucination gate**, не SEO-rank.

| Сигнал | Что даёт | Время (реально) | Что нужно | Сила для «exists» | Статус |
|---|---|---|---|---|---|
| **GitHub org + ≥1 публичный repo с README, license, activity** | Атрибутируемый артефакт + handle | **1 день** создать; **дни–недели** до краула | org, repo, commit history | **Высокая** для OSS-запросов | M + ваш замер |
| **Awesome / curated list mention** | 3rd-party corroboration | **1–8 недель** (PR + merge) | merged PR в живой list | **Высокая** | M |
| **News / trade press / blog roundup** | Earned corroboration | **2–12 недель** | pitch / HN / journalist | **Высокая** | E/M |
| **Wikidata item** | Structured entity ID | **1–7 дней** создать; принятие — **недели** | notability/sources; community | **Средняя–высокая** для KG-like systems | M (Google KG смежен; прямых Gemini-docs нет) |
| **Wikipedia article** | Strongest public notability | **месяцы–годы**; часто **reject** | independent reliable sources (GNG) | **Очень высокая** | E (notability policy) |
| **Crunchbase / similar directories** | Commercial entity stub | **1–14 дней** | company profile, sometimes verification | **Средняя** (directories цитируются реже Wiki) | S/M |
| **arXiv affiliation** | Academic existence signal | **1–8 недель** paper | preprint + real affiliation string | **Высокая** в research queries | M |
| **Schema Organization on site** | Helps parsers | **1 день** | JSON-LD | **Низкая alone** | S |
| **Domain alone (new, 0 backlinks)** | Weak / null | **недели–месяцы** index | DNS + content | **Низкая** — ваш кейс | E |
| **Cross-platform same handle** (GH + X + site + ORCID) | Disambiguation graph | **1–4 недели** align | consistent NAP/name | **Средняя** | S/M |

**Что не удалось найти:**  
публичный paper/vendor doc с формулой *«если Wikidata + Crunchbase + N news → Gemini says exists»*. Entity existence — **чёрный ящик**; работают **корроборации из ≥2 независимых trusted surfaces**.

**Интерпретация Gemini на ваш домен (hypothesis, medium):**  
модель искала **независимые подтверждения имени lab** → нашла коллизию с **Palo Alto Networks** + **нет earned/Wiki/GH-org footprint** → fail-closed «not verifiable». Это **не** «сайт плохой», а «сущность не доказана».

---

## 4. Роль имени (коллизия с крупным брендом)

| Вопрос | Ответ | Доказательная база |
|---|---|---|
| Подавляет ли коллизия с Palo Alto Networks? | **Да, ожидаемо сильно** на brand/navigational и «lab in Palo Alto» запросах | Classic SEO: brand SERP owned by bigger entity (Marie Haynes-type brand ranking literature — general). Для AI: Chen et al. 2025 **big brand bias** + default to major labels under unbranded prompts |
| Есть ли **замер именно Palo Alto AI Research Lab vs Palo Alto Networks**? | **Нет публичного** (не выдумано) | — |
| Что выигрывает **уникальный coined token**? | Disambiguation: меньше collision, проще exact-match retrieval | **S/M** — логика IR + entity linking; **нет** controlled study «coin → first AI cite in N weeks» |
| Срок | Не «магия 14 дней». Token помогает **после** появления артефакта+упоминаний | Ожидание: **4–12 недель** при активном earned; **0** если только rename сайта |

**Практический AK-47:**  
не «переименовать всё завтра», а **ввести unique product/token** (repo name, toolkit name) как primary entity, а lab name — secondary. Движки цитируют **артефакт** («X toolkit») чаще, чем **юрлицо** с коллизией.

---

## 5. Личный бренд vs бренд организации

| Измерение | Человек (solo / founder) | Молодое юрлицо / lab |
|---|---|---|
| Верифицируемость | GitHub user, papers, consistent handle, talks | Нужны multi-source mentions; Crunchbase weak alone |
| Скорость «появиться в ответах» | **Часто быстрее** на tool/repo queries | Медленнее без artifacts |
| Риск | Уход key person | Name collision, empty brand |
| Evidence | **Ваш замер Gemini:** 3-й результат = **solo engineer + 1 hook repo** · Chen 2025: earned/artifact > brand-owned · GEO 2024: lower-ranked sources can gain via content ops | Нет RCT «person always > company» |

**Вердикт:** для 1–3 человек **надёжнее верифицируется человек + артефакт**, чем «лаборатория» как org.  
**Стратегия:** публичный face (founder GitHub) **и** org-репо; в ответах движков выигрывает **named artifact**, org догоняет через earned.

**Кейсы «solo раньше компании»:**  
- **Ваш** — Gemini, hook-repo engineer (первичный evidence).  
- Систематических peer-reviewed case series **не найдено** (не выдумываю Truth Terminal / random GH celebs как «доказанные GEO-кейсы»).

---

## 6. Вердикт по гипотезе — честно

| Часть гипотезы | Вердикт | Почему |
|---|---|---|
| Известность org **не** главная планка на OSS-запросах | **Подтверждена** | Perplexity 10/10 GH/lists; Gemini solo engineer |
| Артефакт на high-authority surface важнее сайта | **Подтверждена** | Earned bias + ваш crawl-without-index |
| «Поэтому сайту не надо уделять внимания» | **Опровергнута как абсолют** | ChatGPT/Gemini/Claude всё равно опираются на web indices; entity queries требуют multi-surface |
| llms.txt/robots = путь в ответы | **Опровергнута** | N-evidence + ваши 13k crawls |
| GitHub/awesome = оптимальный ROI для 1–3 чел. | **Подтверждена (medium–high)** | ROI-логика + measurements |

---

## 7. Пошаговый протокол (1–3 человека, 0 backlinks)

### Неделя 1 — «стать находимым артефактом»  
**Цель:** entity = **repo**, не «сайт lab».

| День | Действие | Метрика проверки | Ожидание |
|---|---|---|---|
| 1 | 1 flagship repo: clear name (**unique token**), one-sentence problem, install in 5 min, license, CONTRIBUTING | `gh repo view` public; clone cold works | 0 citations OK |
| 1 | README GEO-form: stats / quotes / compare table / «who this is for» (GEO 2024) | Self-rubric: 3 stats + 1 comparison | |
| 1–2 | GitHub **org** + pin repo; same handle on X/site | Org URL stable | |
| 2 | CITATION.cff + DOI later optional (**low priority**) | File present | no cite lift expected |
| 2–3 | Submit **Bing Webmaster** + IndexNow/URL submit; Google Search Console; check Brave-indexability later | Bing: *discovered* > 0 | index may still lag |
| 3 | Homepage: **redirect narrative to repo**; Organization JSON-LD; disambiguation line *«not affiliated with Palo Alto Networks»* | Rich results test / view-source | |
| 3–5 | 3–5 **awesome-*** PRs** (agent governance, LLM agents, AI safety tools) — only lists that match scope | PR opened IDs | |
| 5–7 | 1 technical post: **problem → repo → results** (HN / relevant Reddit / personal blog that is already indexed) | Post URL; upvotes optional | |

**Метрика недели 1:**  
- [ ] Repo в топ-3 Google/Bing по **exact unique token**  
- [ ] ≥1 external URL mentions repo (awesome PR count as pending)  
- [ ] Bing: хотя бы *crawled*, не обязательно indexed  

**Срок до первой цитаты:** обычно **ещё нет** (0–2%).

---

### Недели 2–4 — «заслужить earned»  
**Цель:** 3+ независимых mentions артефакта.

| Шаг | Действие | Метрика | Статус эффекта |
|---|---|---|---|
| 1 | Дожать merges в awesome-lists | ≥1 merged | M high for OSS |
| 2 | Weekly commits (signal of life) | ≥1 meaningful commit/week | M freshness |
| 3 | Short demo GIF + `docs/` with FAQ schema | docs page | M structure |
| 4 | Seed discussion: «we open-sourced X for agent governance» in 1–2 communities (not spam) | thread URL | M Reddit/HN path |
| 5 | Indexing push: 5–10 quality backlinks (lists, blog, guest) — **не** PBN | Bing indexed pages ≥1 | E discovery |
| 6 | Weekly **citation probe** (fixed prompts, 3 engines, log) | spreadsheet | measurement |

**Фиксированные probe-запросы (не менять формулировки месяц):**
1. `independent open source AI agent governance labs`  
2. `open source tools for AI agent governance`  
3. exact: `"<your unique token>"`  
4. `Palo Alto AI Research Lab` (collision test)  
5. founder name + toolkit  

**Метрика недель 2–4:**  
- [ ] Bing/Google: **≥1 page indexed** (site OR significant GH README via web)  
- [ ] ≥2 third-party mentions  
- [ ] Probe #3 (exact token) → repo appears in ≥1 engine  

**Ожидаемый срок первой цитаты:** **2–6 недель** после merge в заметный list **или** HN-front / decent discussion — **range speculative (S)**, но согласован с crawl→index lags.

---

### Недели 5–12 — «закрепить сущность + earned density»  
**Цель:** multi-engine cite на category prompts, не только exact token.

| Неделя | Фокус | Метрика |
|---|---|---|
| 5–6 | Второй артефакт **или** evaluation benchmark (цифры!) — GEO loves stats | New citeable page |
| 6–8 | Trade/devrel: 1 talk abstract, 1 comparison blog that **others can cite** | External domain |
| 7–9 | Wikidata item **if** you already have 2+ independent sources; **не** force Wikipedia early | Q-id created |
| 8–10 | Disambiguation content: «Palo Alto AI Research Lab (independent) vs Palo Alto Networks» on **earned** surfaces if possible | Collision probe improves |
| 10–12 | Personal brand pass: founder interviews / consistent bio linking org+repo | Gemini «who is» probe |

**Метрики «готово / не готово» (12 недель):**

| Уровень | Критерий | Движки |
|---|---|---|
| L0 crawl | AI bots hit repo/site | logs |
| L1 index | Bing+Google show ≥1 URL for token | `site:` / webmaster |
| L2 retrieve | Token query → your repo in sources | Perplexity/ChatGPT/Claude |
| L3 cite category | Category prompt cites you among tools | ≥1/5 engines, ≥2/5 runs (SparkToro: high variance — measure multi-run) |
| L4 entity | «Does lab exist?» → yes + correct disambiguation | Gemini/ChatGPT |

**Реалистичный first-cite target:**  
- **L2:** weeks 3–8  
- **L3:** weeks 6–16  
- **L4:** often **later** than L3 (entity harder than artifact)

---

### Приоритет ROI (если времени 5 ч/нед)

| Приоритет | Часы | Зачем |
|---|---:|---|
| 1. Flagship repo + README | 40% | artifact |
| 2. Awesome / curated lists | 20% | earned on GH |
| 3. Bing/Google index + 5 real links | 15% | ChatGPT/Gemini path |
| 4. Community thread + weekly probe log | 15% | Reddit/HN surfaces |
| 5. Site polish / llms.txt / schema | 5% | hygiene only |
| 6. Wikipedia/Crunchbase early | 5% | only after sources exist |

**Не делать:**  
крутить llms.txt как growth lever · ждать, что 13k crawls «созреют» · строить «сайт компании» без repo · cold PR spam в 20 awesome сразу · претендовать на Wiki без independent sources.

---

## 8. Карта «ваш текущий статус» (по вашим данным)

| Стадия | Факт | Следствие |
|---|---|---|
| Crawl | 13 845 AI hits | A ✅ |
| Index (Bing/DDG) | 0 | B ❌ → ChatGPT/часть Perplexity blocked |
| External links | 0 | B/C ❌ |
| Forks | 0 | weak social proof |
| Category cite | GH/lists win; site 0 | D/E path = **artifacts** |
| Entity | Gemini: not verifiable | L4 ❌; collision likely |

**Вывод в одну строку:** вы уже **видимы краулерам**, но **не являетесь evidence-объектом** для retrieval/citation. Лечите **B+C через GitHub+earned**, не A через llms.txt.

---

## 9. Чего **не удалось найти** (явный список)

1. Peer-reviewed causal study: **awesome-list inclusion → AI citation rate**.  
2. Peer-reviewed: **GitHub stars → generative citation**.  
3. Evidence: **CITATION.cff → LLM answers**.  
4. Positive RCT: **llms.txt → citations** (наоборот, negative studies).  
5. Public formula: Wikidata/Crunchbase thresholds for Gemini «exists».  
6. Measured case study: **name collision with cybersecurity giant** (Palo Alto Networks class).  
7. Controlled timeline: **coined brand → first multi-engine cite in N days**.  
8. Official Perplexity architecture whitepaper with ranking weights.  
9. OpenAI current (2026) exact % Bing vs other providers (publicly **drifting**).  
10. Series of verified «solo engineer beat funded company in AI answers» beyond anecdotal + your Gemini observation.

---

## 10. Confidence summary

| Утверждение | Уровень |
|---|---|
| Crawl ≠ index ≠ cite | **established** |
| Earned media >> brand site in AI answers | **established/emerging** |
| Form (stats/quotes) helps if retrieved | **established** (GEO 2024) |
| ChatGPT historically Bing-tied | **established** (2024–25); share **emerging** |
| Claude web → Brave | **established** (Mar 2025 evidence) |
| Gemini → Google grounding | **established** |
| llms.txt no citation lift | **emerging–established** (multiple 2025–26 studies) |
| GitHub/awesome optimal for small OSS lab | **emerging** + **your data** |
| Unique token beats collision | **speculative** (strong IR logic, weak RCTs) |
| First cite in 2–6 weeks after list merge | **speculative range** |

---

### 🎙 Майк
Гипотеза в OSS-слое **держится**: вы уже доказали, что движки едят **артефакты на чужом авторитете**, а не «лабораторию на домене». Сайт сейчас — витрина без входного билета в индекс; билет — **repo + merge в списки + 2–3 внешних упоминания**, потом уже entity. Не лечите llms.txt — лечите **B и earned**.

### 🧒 Простыми словами
Представь: роботы уже **заглядывают в окно** вашего сайта (краул), но **не записывают адрес в телефонную книгу** (индекс) и **не называют вас в разговоре** (цитата).  
На вопрос «какие инструменты?» они называют **инструменты с GitHub и из готовых списков**, как рецепты из известной поваренной книги — а не вывеску новой кафешки без отзывов.  
Делайте **одну понятную штуку на GitHub**, попадите в **хорошие списки**, соберите **несколько чужих упоминаний** — и тогда вас начнут называть. Свой сайт — витрина, не вход.

---

**Источники (ядро):**  
GEO Aggarwal et al. KDD 2024 / arXiv:2311.09735 · Chen et al. arXiv:2509.08919 (Sep 2025) · OpenAI ChatGPT search (31 Oct 2024) · Seer Interactive SearchGPT–Bing (Feb 2025) · TechCrunch Claude+Brave (21 Mar 2025) · Anthropic web search docs · Bing Webmaster «why not indexed» · Ahrefs/SE Ranking/Otterly/Contentful on llms.txt (2025–2026) · citation-source panels Profound/5WPR/industry 2025–2026 · CMU AutoGEO arXiv:2510.11438 · **ваши замеры** (crawler logs, Perplexity 10/10, Gemini solo engineer, 0 backlinks).
