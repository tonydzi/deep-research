# DR26-08-04-ZB-14-2214 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Исследую спор по graph-expansion vs ACT-R fan-penalty: сначала соберу первичные источники по системам GraphRAG и переносу ACT-R в IR/RAG.# Graph-expansion vs ACT-R fan-penalty: разбор для personal second brain

## (1) Прямой ответ (5 строк)

1. **Ваш гейт по типу запроса (entity off / theme on) — правильный дефолт для wiki-волта**, а не «временный костыль». [emerging]  
2. **Гипотеза «виноваты только хабы» частично верна (шум hub/fan-out реален), но не объясняет весь entity-провал**: entity-запросы требуют *точности*, а 1-hop по `wiki` подмешивает *соседние темы*, не «факты про сущность». [emerging]  
3. **Промышленные GraphRAG-2024–26 решают это маршрутизацией local/global или low/high, а не ACT-R base-level.** LightRAG даже **поднимает** high-degree узлы, а не штрафует — обратное ACT-R fan-penalty. [established]  
4. **Ваш N ≈ 12 (6+6) слишком мал, чтобы «закрыть» архитектуру**; нужен вечерний 2×2-эксперимент (~40–60 суждений). [established]  
5. **Рекомендация: оставить гейт; fan-penalty/PPR — дешёвая страховка на theme-режиме; ACT-R decay — overkill.** [emerging]

---

## (2) Разбор спора

### 2.1 Что утверждает «возражающий»

Плохой результат на entity = **паразитные хабы** (заметки с сотнями ссылок). Лекарство: **bounded spreading activation** (fan-penalty + decay/base-level), после чего гейт не нужен.

### 2.2 Что говорит литература (две разные механики)

| Механика | Суть | Где доказана |
|---|---|---|
| **Fan effect (ACT-R)** | Чем больше фактов связано с cue, тем **слабее** активация на каждое ребро → **медленнее/хуже retrieval** | Психология: Anderson 1974; ACT-R formalization |
| **Query-type mismatch** | Для entity нужна **precision**; 1-hop соседи — часто *другой* предмет | GraphRAG local vs global; LightRAG low vs high; Adaptive-RAG; HippoRAG 2 simple QA |

**Fan effect (классика):**

> «If there is one association, the activation only has to spread to a single link whereas multiple associations would divide the activation to many links.»  
> — Wikipedia / Fan effect, https://en.wikipedia.org/wiki/Fan_effect

> «The fan effect refers to the fact that cues that are associated with more facts result in slower recall. In ACT-R, the amount of activation spread from a cue…»  
> — West et al., ICCM 2010, http://act-r.psy.cmu.edu/wordpress/wp-content/uploads/2012/12/1075West%20Pyke%20RutledgeT%20Lang_ICCM_2010_Interference%20in%20ActR%20fan%20effect.pdf

**Unconstrained SA в IR ломается (важно против «просто включить ACT-R»):**

> «Pure spreading activation… converges to a fixed state and retrieval results become nearly independent of the input query.»  
> — Berthold et al., 2009, цит. в SA-RAG, https://arxiv.org/html/2512.15922v3

> SA в IR «needs to be constrained by limiting the number of steps… and by using a decay factor smaller than one.»  
> — SA-RAG survey of Crestani et al. line, https://arxiv.org/html/2512.15922v3

### 2.3 Разделяли ли hub-noise и entity-nature?

**Да, частично — но не «ACT-R vs гейт», а «task type × structure method».**

**HippoRAG 2** явно разделил *factual* (entity-heavy), *sense-making* (theme), *associativity* (multi-hop):

> «All previous structure-augmented methods underperform against the strongest embedding-based RAG methods… on all three benchmark types.»  
> «Each method type experiences the largest performance decay in tasks outside its own experimental setup.»  
> — Gutiérrez et al., https://arxiv.org/html/2502.14802

Цифры (F1, Llama-3.3-70B reader, Table 2 в paper): structure-методы **падают** на simple/factual относительно NV-Embed; HippoRAG 2 чинит **associativity** (+~7 pp avg associative) **без** деградации factual — через **PPR + recognition filter + passage nodes**, не через гейт entity/theme, но и **не через ACT-R BLL**.

**Microsoft GraphRAG** — не «всегда expand», а **два режима**:

> Local Search «is well-suited for answering questions that require an understanding of **specific entities**…»  
> — https://microsoft.github.io/graphrag/query/local_search/

> Global Search — для «What are the top 5 themes…» где vector RAG «perform[s] terribly».  
> — https://microsoft.github.io/graphrag/query/global_search/

Это **гейт по intent**, институционализированный.

**LightRAG** — dual-level: specific (entity) vs abstract (theme):

> «Specific Queries… Who wrote Pride and Prejudice?» vs «Abstract Queries… How does AI influence modern education?»  
> Low-level = entities; High-level = themes.  
> — https://lightrag.github.io/

Ablation: low-only «focuses excessively on entities and their immediate neighbors… struggles… for complex queries»; high-only «may struggle with… precise, detailed answers.»  
— там же.

### 2.4 Вердикт по «виноваты хабы?»

| Утверждение | Вердикт | Уверенность |
|---|---|---|
| Хабы добавляют шум в 1-hop | **Да**, особенно на power-law wiki-графах | [established] в IR/network science; [emerging] как *главная* причина вашего entity-провала |
| Fan-penalty/PPR **снижает** hub noise | **Да** (PPR damping, SA constraints) | [established] |
| После fan-penalty **гейт не нужен** | **Слабо поддержано** | [speculative] — индустрия всё равно держит local/global; LightRAG **бонусит** degree |
| Entity-провал = **сама природа** entity (precision) + **семантика wiki-рёбер** (ассоциация ≠ «о сущности») | **Сильнее как полная гипотеза** | [emerging] |
| Ваш 1/5 vs 5/1 «доказывает» архитектуру | **Нет** — слишком мало наблюдений | [established] статистика |

**Ключевой нюанс вашего стека:** 1-hop по `wiki` в Obsidian — это **не KG relation «facts about X»**, а **авторская ассоциация** (часто hub: MOC, daily, people-index). Для entity «кто такой X / что решили с X» соседи MOC/index = шум **даже при низкой degree**, если ребро «не то типа». Fan-penalty этого **не лечит**.

---

## (3) Таблица систем 2024–2026

| Система | Expand always / conditional | Degree penalty? | Temporal decay? | Что мерили | Бенчмарки |
|---|---|---|---|---|---|
| **Microsoft GraphRAG** | **Conditional**: Local (entity neighborhood) vs Global (community summaries); DRIFT hybrid | **Нет penalty**. В leaf community summary: prioritize edges by **sum of node degrees** (prominence **up**, not down) | Нет activation decay | Comprehensiveness/diversity win rates ~72–83% vs vector RAG on global Q; local = entity Q | Podcast ~1M tok, News ~1.7M; 125 LLM-generated global Q |
| **LightRAG** | **Dual always-on hybrid**: low-level keywords + high-level keywords; mix + vector | **Opposite of fan-penalty**: sort by **node/edge degree (rank)** descending | Нет | Win rates vs GraphRAG/naive (LLM-as-judge: comprehensiveness, diversity, empowerment); ablation high/low | Legal / multi-domain (paper); Neo4j impl confirms degree ranking |
| **HippoRAG / HippoRAG 2** | **Always** structure path via **Personalized PageRank** (seeds from query); fallback dense if no triples | **Implicit damping** via PPR (not ACT-R fan formula). Reset weight factor 0.05 for passages | Нет | F1/Recall@5; HippoRAG 2 **+7** avg associative over SOTA embed; **no deterioration** on factual/sense-making | NQ, PopQA, MuSiQue, 2Wiki, HotpotQA, LV-Eval, NarrativeQA |
| **RAPTOR** | Tree of **summaries** (not wiki 1-hop); collapsed-tree retrieves multi-level | Нет | Нет | +vs BM25/DPR; QuALITY +GPT-4 82.6% (prior SOTA 62.3%); NarrativeQA SOTA METEOR with UnifiedQA | NarrativeQA, QASPER, QuALITY |
| **Zep / Graphiti** | Hybrid: vector + BM25 + graph; optional **distance-from-central-node** rerank | Не ACT-R fan; centrality distance | **Yes (bi-temporal)**: edge invalidation / lifecycle, not BLL decay | LoCoMo **94.7% @ 155ms**; LongMemEval **90.2% @ 162ms** | LoCoMo, LongMemEval |
| **LlamaIndex PropertyGraph** | After vector/synonym **node select → traverse adjacent paths** (1-hop triples default; depth configurable) | Нет | Нет | Product/examples; not a single public SOTA paper | — |
| **Neo4j GraphRAG** | Local entity traverse + community; integrates GraphRAG/LightRAG patterns | Uses degree as **importance** in LightRAG path | Optional temporal props | Whitepapers; production caveats on LightRAG | — |
| **Letta / MemGPT** | **Agent tool retrieval** over core/recall/archival tiers; **not** graph spreading | N/A | Summarization / eviction of context (OS analogy), not ACT-R BLL | Agent memory benchmarks (contested LoCoMo claims) | LoCoMo variants |
| **SA-RAG (2025–26)** | Spreading activation with **thresholds, weight rescale, hop-limited subgraph** | Weight by query–edge similarity; prune low activation | Decay via hop/threshold, not full ACT-R BLL | Up to **+25–39% absolute correctness** vs naive RAG when SA + CoT; small open models | MuSiQue, 2Wiki **n=100 each** |
| **Adaptive-RAG (NAACL 2024)** | **Classifier routes**: no / single / multi-step retrieval by **complexity** | N/A | N/A | Better accuracy–efficiency than always-multi; oracle upper bound large | SQuAD, NQ, Trivia, MuSiQue, Hotpot, 2Wiki |

### Цитаты-якоря по системам

**GraphRAG local:**

> «These entities serve as access points into the knowledge graph, enabling the extraction of further relevant details such as connected entities, relationships… and … text chunks.»  
> https://microsoft.github.io/graphrag/query/local_search/

**GraphRAG degree-as-prominence (не fan-penalty):**

> «for each community edge in decreasing order of combined source and target node degree (i.e., overall prominence)…»  
> https://arxiv.org/html/2404.16130v2

**LightRAG ranks by degree (анти-ACT-R):**

> «Rather than just finding relevant information, LightRAG prioritizes based on how well-connected and central…»  
> «entities are ranked by node degree»  
> https://neo4j.com/blog/developer/under-the-covers-with-lightrag-retrieval/

**HippoRAG 2 PPR + recognition memory:**

> «HippoRAG 2 builds upon the Personalized PageRank algorithm… with deeper passage integration and more effective online use of an LLM… achieving a 7% improvement in associative memory tasks… while also exhibiting superior factual knowledge…»  
> https://arxiv.org/html/2502.14802

**Graphiti temporal (не BLL):**

> «Temporal Awareness: Tracks changes in facts… Temporal edge invalidation»  
> «Hybrid Search: Combines vector similarity, BM25 full-text, and graph traversal… Results can be reranked by distance from a central node»  
> https://help.getzep.com/graphiti/getting-started/overview

**Adaptive-RAG = ваш гейт, академически:**

> «dynamically select the most suitable strategy… based on the query complexity… operationalized with a classifier»  
> https://arxiv.org/html/2403.14403 / https://arxiv.org/abs/2403.14403

---

## (4) Альтернативы и цена для вашего стека

Стек: markdown + SQLite + e5 + reranker + 1-hop wiki. Чинит не-технарь.

| Альтернатива | Суть | Ожидаемый эффект vs ваш гейт | Цена внедрения (AK-47) | Риск |
|---|---|---|---|---|
| **Гейт entity/theme (текущий)** | Heuristic: имена/CRM-ids → no expand; «что я думаю» → expand | Baseline; aligned with GraphRAG/LightRAG dual | **Уже есть** | Ошибки классификатора |
| **Fan-penalty на expand** | `score_neighbor /= log(1+degree)` или `1/√deg` | Может **чуть** снизить theme-noise; **не заменит** entity-off | 0.5–1 вечер: degree в SQLite при индексе | Under-retrieve полезных hub-MOC на theme |
| **PPR вместо 1-hop** | Seeds = vector hits; reset; damp=0.85 | Лучше multi-hop associativity (HippoRAG) | 1–2 дня; scipy/networkx; тюнинг reset | Сложнее debug; не-технарю тяжело |
| **PageRank edge norm** | Precompute edge weight `1/(deg_u·deg_v)` | Как fan, offline | 0.5 дня | То же |
| **Тип ребра** | Expand only `related_to` / not `moc` / not `daily` | **Сильный** precision gain на entity | 1 день: tagging links + schema | Нужна дисциплина ссылок |
| **Обучаемый гейт** | Adaptive-RAG-style tiny classifier | Лучше heuristic | 2–3 дня + silver labels | Overkill; drift |
| **Query routing LLM** | 1-call: entity\|theme\|multi | Flexible | ~50–200ms + cost/query | Latency; non-det |
| **Reranker-only, no graph** | Vector + e5 + rerank | Entity часто **лучше**; theme хуже multi-hop | 0 (выключить expand) | Theme regress (ваш 5/1) |
| **Entity linking + exact** | CRM/person slug match → open note, no hop | Best entity precision | 1 день: aliases table | Miss soft mentions |
| **ACT-R BLL (time decay + practice)** | \(B_i = \ln\sum t_j^{-d}\) | Для **access frequency**, не для 1-hop wiki noise | 2+ дня: event log every open | **Нет** измеренного win в personal RAG 2024–26; maintenance |

**Практичный порядок (если трогать):**  
1) гейт оставить → 2) entity-linking exact → 3) на theme: soft fan-penalty **или** cap neighbors (`top-M by degree-normalized`) → 4) **не** full ACT-R.

---

## (5) Проверяемый план эксперимента (1 вечер)

### Цель
Отличить **H1: виноваты хабы** от **H2: виноват тип запроса (entity vs theme)**.

### Дизайн: 2×2 + ablation

| | Expand OFF | Expand ON (1-hop raw) | Expand ON + fan-penalty |
|---|---|---|---|
| **Entity queries** | A | B | C |
| **Theme queries** | D | E | F |

**Опционально (тот же вечер, 30 мин):**  
**G** = Expand ON, но **исключить top-5% degree hubs** из neighbors (hard hub-drop).

### Запросы
- **Entity:** 20 штук (люди, компании, конкретные decision/артефакты из CRM/волта).  
- **Theme:** 20 штук («что я думаю про X», «как мы решаем Y», «какие уроки по Z»).  
- **Минимум:** 12+12 (ваш текущий масштаб ×2); **лучше 20+20**.

### Процедура (судья = человек, как раньше)
1. Для каждого query прогнать условия **вслепую** (label A/B/C shuffled).  
2. Судья: top-8 кандидатов **до** генерации ответа (retrieval quality).  
3. Оценка **на карточку**:  
   - **Useful** ∈ {−1, 0, +1} vs pure vector baseline  
   - **Precision@5** (relevant/5)  
   - **Noise flag**: «есть ли явный hub-мусор?» (да/нет)  
   - **Answerability** 0–2 (хватит ли для ответа)

### Метрики (primary)
1. **ΔUseful** = mean Useful (expand − no-expand) **по strate entity / theme**  
2. **Hub-noise rate** = доля запросов с noise flag  
3. **P@5**  
4. Время судьи (sanity)

### Сколько нужно для вывода

| N entity (paired) | Что можно сказать |
|---|---|
| 6 (ваш сейчас) | Гипотеза, не решение |
| **≥20** | Направление + rough effect |
| ≥40 | Стабильнее для decision memo |

Грубо: sign test / Wilcoxon на paired Useful.  
При N=20 entity, если raw expand **вредит ≥14/20**, а fan-penalty **не** доводит до ≤6/20 вред — H1 **слабая**.

### Правила опровержения

| Гипотеза | Опровергается, если |
|---|---|
| **H1 «только хабы»** | На entity: fan-penalty **и** hub-drop **не** возвращают Useful ≥ baseline (среднее Δ ≥ 0), **или** hub-noise flag высокий, но удаление hubs не чинит P@5 |
| **H2 «тип запроса»** | На entity: expand+fan **стабильно лучше** no-expand (ΔUseful > 0, p≈0.05 или ≥70% wins); **и** theme: expand OFF **не** хуже expand ON |
| **H1+H2 (оба)** | Entity: fan помогает частично; theme: expand помогает; гейт + soft fan = лучший комбо |

### Протокол «один вечер» (≈3–4 ч)
1. 30 мин: 40 queries в `eval_queries.json` (20/20).  
2. 45 мин: скрипт 3 режима (off / hop / hop+fan), dump top-8 JSONL.  
3. 90 мин: человек labels (40×3 ≈ 120 карточек; 40–50 сек → tight).  
4. 30 мин: pivot table + вердикт.  
5. 15 мин: записать в vault decision.

**Fan formula (фиксировать до старта):**  
`neighbor_score = vec_seed_score * 1/sqrt(1+degree(neighbor))`, cap 5 neighbors/seed, total pool ≤ 2K before rerank.

---

## (6) Когда ваш гейт — ПРАВИЛЬНЫЙ ответ (контраргументы **против** ACT-R-усложнения)

1. **Цена precision на entity выше цены recall.** Промах «сосед-MOC в топе» = неправильный совет/аутрич. GraphRAG/LightRAG **специально** делят local/high-precision vs global/theme.  
2. **Wiki-рёбра ≠ ACT-R facts.** ACT-R fan — про «сколько фактов привязано к cue». У вас ребро часто = «лежит рядом в голове», не «атрибут сущности».  
3. **LightRAG empirically promotes hubs** — если бы fan-penalty был универсальным лекарством, industry default был бы inverse-degree; он **не** таков.  
4. **HippoRAG 2** чинит factual+associative **без** query-type gate, но ценой **OpenIE KG + PPR + LLM triple filter** — **не** ваш AK-47 markdown stack. Перенос «просто fan» ≠ перенос HippoRAG.  
5. **Unconstrained SA «pointless»** (Berthold 2009) — ACT-R-like expand **требует** constraints ≈ вашему гейту/cap/threshold.  
6. **Сложность vs ремонтопригодность:** BLL + practice log + decay params — 3 новых failure modes; гейт — 1 if.  
7. **Ваш замер уже разделяет тип запроса** (1/5 vs 5/1). Даже если N мал, *направление* совпадает с Adaptive-RAG / GraphRAG dual.  
8. **Опровержение «гейта навсегда»:** если вечерний тест покажет entity+fan ≥ entity-off **и** theme+fan ≥ theme-raw — тогда **смягчить** гейт до soft-gate. Пока нет — не усложнять.

### Когда всё-таки добавить fan (не вместо гейта)
- Theme-режим: neighbors часто «размазаны» хабами.  
- Метрика hub-noise > 30% на theme.  
- 10 строк кода, A/B в shadow.

---

## (7) Где классика/консенсус НЕ реплицировались

| Ожидание | Реальность |
|---|---|
| «Graph always helps multi-hop» | HippoRAG 2: structure-RAG **хуже** SOTA dense на simple/factual и часто на sense-making, если не HippoRAG 2–класса |
| «Higher degree = noise → always penalize» | GraphRAG community build & LightRAG retrieval **rank by degree up** |
| «Spreading activation = free multi-hop» | Pure SA → query-independent fixed point (Berthold 2009); нужны hop/decay/threshold |
| «Microsoft GraphRAG beats all» | Wins on **global sensemaking**; local entity path — **отдельный** algorithm; cost high (LightRAG critique) |
| «ACT-R BLL → measurable RAG win» | В 2024–26 industrial RAG: **PPR, dual-level, adaptive routing, temporal invalidation** — **не** published ACT-R BLL wins on personal vaults |
| «LLM-as-judge GraphRAG metrics = truth» | GraphRAG/LightRAG heavy on LLM judge; claim extraction (GraphRAG Exp.2) only partially aligns |
| «More context always better» | Entity: neighbors = distraction (your A/B; LightRAG low-only ablation warning) |

---

## Главное допущение рекомендации

> **Допущение:** в *вашем* волте 1-hop `wiki` на entity-запросах чаще добавляет **off-topic ассоциации** (включая, но не только, хабы), а не missing attributes той же сущности; поэтому **precision-first routing** важнее **activation-physics**.

Если окажется ложным (соседи entity-запросов — почти всегда «ещё факты про X», а не «смежные проекты/MOC»), тогда **H1 (hubs)** выигрывает, гейт можно сузить до «expand always + fan + cap», и ACT-R-подобная нормализация станет основной, а не страховкой.

**Второе допущение:** судья-человек на десятках запросов **коррелирует** с «полезностью в работе». Если нет — любой A/B (включая ваш 1/5) шумный.

---

## Честно про ваш замер N

| | Entity | Theme |
|---|---|---|
| Help / Harm | 1 / 5 | 5 / 1 |
| n | 6 | 6 |

- Point estimates: entity expand harm rate **83%**; theme help **83%** — **сильный сигнал направления**.  
- **95% CI** при n=6 огромны (Wilson): harm ~36–99%.  
- **Нельзя** на этом выбирать ACT-R vs gate как «доказано».  
- **Можно** (и правильно) держать **рабочий дефолт = gate**, пока вечерний n≥20/20 не скажет иначе.

Правило: **policy на weak evidence OK; complexity (ACT-R) — нет.**

---

## Список источников (открыты / процитированы)

1. Microsoft GraphRAG Local Search — https://microsoft.github.io/graphrag/query/local_search/  
2. Microsoft GraphRAG Global Search — https://microsoft.github.io/graphrag/query/global_search/  
3. Edge et al. GraphRAG paper — https://arxiv.org/html/2404.16130v2  
4. GraphRAG dynamic community selection — https://www.microsoft.com/en-us/research/blog/graphrag-improving-global-search-via-dynamic-community-selection/  
5. LightRAG project page — https://lightrag.github.io/  
6. LightRAG paper — https://arxiv.org/abs/2410.05779  
7. Neo4j Under the covers with LightRAG: Retrieval — https://neo4j.com/blog/developer/under-the-covers-with-lightrag-retrieval/  
8. HippoRAG 2 — https://arxiv.org/html/2502.14802  
9. HippoRAG (OpenReview) — https://openreview.net/forum?id=hkujvAPVsg  
10. RAPTOR — https://arxiv.org/html/2401.18059  
11. Adaptive-RAG — https://arxiv.org/abs/2403.14403 , https://arxiv.org/html/2403.14403  
12. SA-RAG (spreading activation) — https://arxiv.org/html/2512.15922v3  
13. Graphiti overview (Zep) — https://help.getzep.com/graphiti/getting-started/overview  
14. LlamaIndex Property Graph basic — https://developers.llamaindex.ai/python/examples/property_graph/property_graph_basic/  
15. Fan effect — https://en.wikipedia.org/wiki/Fan_effect  
16. West et al. ACT-R fan ICCM — http://act-r.psy.cmu.edu/wordpress/wp-content/uploads/2012/12/1075West%20Pyke%20RutledgeT%20Lang_ICCM_2010_Interference%20in%20ActR%20fan%20effect.pdf  
17. Schneider & Anderson fan time course — https://pmc.ncbi.nlm.nih.gov/articles/PMC3259266/  
18. Letta agent memory / MemGPT tiers — https://www.letta.com/blog/agent-memory/  
19. Meilisearch Adaptive RAG overview — https://www.meilisearch.com/blog/adaptive-rag  

---

## Итоговая рекомендация (кофаундер)

**Не менять архитектуру на ACT-R.** Оставить **гейт entity/theme**.  
Вечером — **2×2 + fan arm** (§5).  
Если H1 выиграет — добавить **5 строк fan на theme-path**, не снимая гейт.  
Если H2 выиграет — гейт = production, fan optional.

---

🎙 **Майк:** Идём верно: ваш гейт совпадает с тем, как делят local/global индустрией. «Возражение ACT-R» — умное, но лечит **другой** слой (шум степени), не **intent mismatch**. Не покупаем сложность до вечернего 2×2. Вопрос: гоняем эксперимент §5 на ваших 40 запросах в ближайший слот, или сначала вытащим degree-гистограмму хабов из волта (0 токенов, 20 мин) — чтобы сразу видеть, есть ли «паразитные хабы» как класс?

---

🧒 **Простыми словами:**  
Представь библиотеку. Если спросить «где книга про Ивана?» — не надо тащить все полки «рядом с Иваном» (там ещё кухня, налоги, чаты). Если спросить «о чём я вообще думаю про здоровье?» — соседние полки помогают.  
Умный человек сказал: «проблема в том, что есть супер-полки с тысячей связей». Иногда да. Но чаще проблема в том, **какой вопрос** задали.  
Поэтому рубильник «для кого / про что» — правильный. Штраф за «слишком популярную полку» — запасной болтик, не новый двигатель.  
Ваших 12 проб мало, как 6 бросков монеты. Вечером сделаем 40 — и будет ясно.
