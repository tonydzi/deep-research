# DR26-08-04-ZB-14-2214 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

## 1. Прямой ответ в 5 строк

1. **[established]** Литература подтверждает, что неконтролируемый fan-out создаёт шум, но **не подтверждает**, что именно хабы являются главным объяснением провала graph expansion на entity-запросах. Прямой абляции «тип запроса × степень соседей» почти никто не публиковал.
2. **[established]** Лучшие системы не заменяют routing одним ACT-R-подобным штрафом: Microsoft GraphRAG маршрутизирует entity/local и thematic/global запросы по-разному; HippoRAG 2 исправляет factual retrieval не одним fan penalty, а совокупностью passage nodes, PPR и query-to-passage links.
3. **[established]** ACT-R fan effect надёжен как модель интерференции в малых лабораторных сетях, но его формулы не являются доказанным оптимальным ranking function для IR/RAG. Base-level decay особенно опасно смешивать с истинностью и релевантностью.
4. **[emerging]** Для вашего стека лучший следующий шаг — оставить нынешний query-type gate и добавить дешёвую защиту: exact entity matching, лимит соседей и degree-normalized score. Не заменять gate до локальной абляции.
5. **[established]** Ваш результат 1/5 против 5/1 выразителен, но статистически слаб: Fisher exact для таблицы `[[1,5],[5,1]]` даёт примерно `p=0.080`; это сигнал для эксперимента, не основание считать механизм установленным.

---

## 2. Разбор спора

### 2.1 Что именно уже следует из вашего A/B

Наблюдение показывает взаимодействие между типом запроса и полезностью расширения:

| Тип | Graph помог | Graph навредил | Доля помощи |
|---|---:|---:|---:|
| Entity | 1 | 5 | 16,7% |
| Thematic | 5 | 1 | 83,3% |

Это не просто «graph expansion иногда плох»: знаки эффекта поменялись местами. Но на шести нетривиальных наблюдениях в каждой группе:

- двусторонний sign/binomial test для `1 против 5` даёт около `p=0.219`;
- Fisher exact для взаимодействия двух групп — около `p=0.080`;
- доверительные интервалы очень широки;
- selection bias возможен, если запросы составлялись после просмотра волта.

**Вывод [established]:** ваши данные оправдывают gate как осторожную рабочую политику, но не доказывают, что тип запроса является причинным фактором.

### 2.2 Гипотеза «виноваты хабы»

У неё есть разумное механистическое основание:

- число соседей при one-hop expansion может расти почти без ограничения;
- generic MOC/index/person/company notes имеют высокую степень, но низкую условную релевантность;
- если все соседи получают одинаковый шанс, один seed-хаб расходует весь candidate budget;
- reranker вынужден выбирать релевантное из более трудного пула и иногда теряет исходные vector hits из-за лимита.

Современные работы прямо признают проблему роста ego-graph. Например, SeedER пишет: **“ego-graph expansion grows rapidly”** и поэтому использует learned selective expansion, а не выдаёт всех соседей ([SeedER, 2026](https://arxiv.org/abs/2605.23753)). SAGE также не оставляет one-hop кандидатов без фильтра: **“filtering the neighbors using dense+sparse retrieval”**; на OTT-QA и STaRK авторы сообщают прирост retrieval recall на 5,7 и 8,5 пункта ([SAGE, 2026](https://arxiv.org/abs/2602.16964)).

В более раннем semantic-network IR применялась нормализация по степени именно для уменьшения влияния highly connected nodes: **“the effect of very highly-connected nodes”** ([Hughes & Ramage, 2010](https://aclanthology.org/W10-3506.pdf)).

Но из этого не следует, что хабы объясняют ваш эффект полностью:

1. Fan penalty уменьшает вес каждого соседа, но не отличает полезное ребро `authored` от слабого `mentioned in`.
2. Если entity note имеет всего три соседа, но запрос просит конкретный атрибут самой сущности, все три соседа всё равно могут быть off-topic.
3. Высокая степень иногда полезна тематическому запросу: хаб может быть именно хорошей обзорной картой темы.
4. Проблема может быть не в числе кандидатов, а в потере precision при любом semantic drift.

**Вывод [established]:** хабы — признанный источник candidate explosion и шума.

**Вывод [emerging]:** нет опубликованного результата, показывающего, что degree normalization устраняет специфическую деградацию entity-запросов настолько, что query routing становится ненужным.

### 2.3 Гипотеза «виновата природа entity-запроса»

Здесь доказательств больше, хотя они преимущественно косвенные.

Microsoft GraphRAG прямо разделяет режимы:

> “Local search uses entities as the entry point…”

и

> “Global search uses the pre-computed summaries of each entity community…”

Документация дополнительно говорит, что local search лучше для targeted entity questions, а global — для broad thematic questions ([официальное обсуждение Microsoft GraphRAG](https://github.com/microsoft/graphrag/discussions/721)). Исходная статья была мотивирована именно слабостью обычного RAG на **global questions**, например “What are the main themes…”, а не на точечных entity lookup ([Edge et al., 2024](https://arxiv.org/abs/2404.16130)).

HippoRAG 2 начинается с признания противоположности factual и associative задач:

> “performance on more basic factual memory tasks drops considerably below standard RAG”

([HippoRAG 2, 2025](https://arxiv.org/abs/2502.14802)). Это особенно существенно: ухудшение factual memory наблюдалось не только у наивного one-hop, а у более сложных graph/memory-подходов. Исправление потребовало passage integration и query-to-passage links, а не просто уменьшения влияния хабов.

С точки зрения decision theory:

- точечному запросу обычно важны Precision@1, MRR и отсутствие подмены сущности;
- тематическому — coverage, diversity и recall;
- graph expansion преимущественно покупает recall ценой precision.

**Вывод [established]:** разные классы запросов имеют разные функции потерь. Один и тот же retriever необязательно должен быть оптимален для обоих.

### 2.4 Где эти объяснения действительно разделяли

Строгих экспериментов вида:

```text
entity/thematic × low/high degree × raw/normalized expansion
```

в изученных первичных источниках я не нашёл.

Ближайшие аналоги:

- **Microsoft GraphRAG:** сравнивает local/global и flat baseline на broad sense-making questions, но не делает degree ablation ([статья](https://arxiv.org/abs/2404.16130)).
- **HippoRAG 2:** отдельно измеряет factual, sense-making и associative memory, но passage integration, PPR и online LLM changes включены пакетом; чистый вклад degree normalization не изолирован ([статья](https://arxiv.org/abs/2502.14802)).
- **SAGE:** фильтрует расширенных соседей и показывает recall gain, но не публикует factorial entity-vs-theme/hub ablation ([статья](https://arxiv.org/abs/2602.16964)).
- **SeedER:** противопоставляет selective learned policy разрастающемуся ego-graph, но задача — compositional KG retrieval, а не entity lookup в персональном волте ([статья](https://arxiv.org/abs/2605.23753)).
- **ACT-R fan experiments:** меняют число ассоциаций и измеряют latency/accuracy человеческой памяти, но это не document retrieval и не различение типов пользовательского запроса.

**Итог [established]:** утверждение другого ресёрча «если убрать хабы, gate не нужен» — проверяемая, но пока **спекулятивная экстраполяция**, а не вывод из опубликованной литературы.

---

## 2.5 ACT-R: что переносится

Стандартная base-level activation:

\[
B_i=\ln\left(\sum_{j=1}^{n} t_j^{-d}\right)
\]

где \(t_j\) — время с момента обращения/предъявления, \(d\) — decay parameter.

Полная активация обычно записывается как:

\[
A_i=B_i+\sum_j W_jS_{ji}+\epsilon
\]

а fan effect — через:

\[
S_{ji}=S-\ln(fan_j)
\]

Формулы и интерпретация приведены в ACT-R-моделях: **“strength of association … decreases as a logarithmic function of the fan”** ([описание модели и формул](https://www.frankritter.com/papers/kim08.pdf)); современный разбор также описывает \(1/fan_j\) как частный случай вероятностной ассоциации ([Thomson et al., 2017](https://onlinelibrary.wiley.com/doi/full/10.1111/tops.12244)).

Что переносится разумно:

- **[established]** ограниченный activation budget;
- **[established]** уменьшение веса неоднозначного/высокофанового cue;
- **[emerging]** логарифм степени как простой robust prior;
- **[established]** recency/frequency как полезные признаки пользовательской памяти.

Что не переносится автоматически:

- wiki-link — не обязательно ассоциация человеческой памяти;
- degree заметки — не число конкурирующих фактов для данного cue;
- retrieval count может означать популярность, а не релевантность;
- старый факт может быть нормативно важнее нового;
- частое ошибочное извлечение само себя подкрепляет;
- в ACT-R activation предсказывает latency/probability human recall, а не calibrated relevance документа.

**Вывод [established]:** ACT-R даёт хорошую форму регуляризатора, но не валидирует конкретный ranking function для RAG.

### Измеренные переносы в IR/RAG

Старая IR-литература по spreading activation показывает, что constrained activation может работать, но не установила универсальный выигрыш. Crestani характеризует область как набор разнообразных моделей, которые необходимо **“critically analyzed”**, а не как единый стандартный алгоритм ([Crestani, 1997](https://doi.org/10.1023/A:1006569829653)).

Более современный наиболее близкий перенос — HippoRAG:

- PPR распространяет query relevance по графу с нормализованными переходами;
- это функционально ближе к bounded spreading activation, чем буквальный ACT-R;
- HippoRAG 2 сообщает около **7% улучшения associative-memory tasks** относительно сильной embedding model ([HippoRAG 2](https://arxiv.org/abs/2502.14802));
- но авторы одновременно признают, что предыдущие graph approaches проседали на factual memory.

Zep использует temporal KG и на DMR сообщает **94,8% против 93,4% у MemGPT**, а на LongMemEval — улучшения до **18,5%** и снижение latency на 90% ([Zep paper](https://arxiv.org/abs/2501.13956)). Однако это не ACT-R decay: timestamps описывают validity и invalidation фактов, а не естественное забывание по частоте обращений.

**Вывод [established]:** измеренный выигрыш есть у PPR/temporal graph packages, но чистого контролируемого результата «ACT-R fan + base-level decay побеждает RAG» нет.

---

## Где классика/консенсус не реплицировались или были ограничены

1. **ACT-R не объясняет весь fan effect.** Работа *Interference and ACT-R* сообщает: **“this cannot be accounted for using ACT-R”** для обнаруженного паттерна интерференции ([West et al.](https://citeseerx.ist.psu.edu/document?doi=0aebaa4fcd5c1fffff700d7c39474877591dfbe1&repid=rep1&type=pdf)).  
   **[established]** Стандартный fan mechanism — не полное объяснение интерференции.

2. **Экологическая экстраполяция степени не проверена.** Анализ ACT-R отмечает, что хорошее соответствие известно при fan size **1–4**, а для реалистически больших fan sizes экспериментальных данных мало ([Fisher et al., 2022](https://mathpsych.org/presentation/725)).  
   **[established]** Перенос с fan=1–4 на markdown-хаб с 200 ссылками — экстраполяция на два порядка величины.

3. **Spreading activation имеет predictive, но не обязательно explanatory power.** Нейросетевая критика прямо говорит: **“predictive but not explanatory power”** ([Goetz & Walters, 2000](https://www.sciencedirect.com/science/article/abs/pii/S0364021399000245)).  
   **[emerging]** Хороший fit психологических реакций не делает формулу оптимальным IR ranker.

4. **Graph retrieval не гарантирует хороший ответ.** На KET-RAG gold answer присутствовал в контексте у 77–91% вопросов, но итоговая accuracy составляла 35–78%; 73–84% ошибок авторы классифицировали как reasoning failures ([Reasoning Bottleneck in Graph-RAG, 2026](https://arxiv.org/abs/2603.14045)).  
   **[emerging]** Даже устранение hub noise может не устранить downstream harm.

5. **Temporal graph не гарантирует правильный latest fact.** На MemoryAgentBench сообщается только 7% для Zep/Graphiti в одном conflict-resolution setup против 54% HippoRAG-v2; deterministic resolver достиг 78,0–94,8% single-hop в зависимости от модели ([Reddy & Challaram, 2026](https://arxiv.org/abs/2606.01435)). Авторы предупреждают, что меняли pipeline пакетом.  
   **[emerging]** Timestamp-aware storage и retrieval-decay — не замена явному типу задачи и aggregation rule.

---

## 3. Как устроены системы 2024–2026

| Система | Расширение / routing | Degree penalty | Время | Что измеряли |
|---|---|---|---|---|
| **Microsoft GraphRAG** | **Условно:** Local Search для entity-centric; Global Search для corpus themes; DRIFT комбинирует local/global. Не простой always-on one-hop. | Явного ACT-R fan penalty в опубликованном алгоритме нет. Community detection косвенно контролирует структуру. | Нет base-level decay; основной corpus статичен. | Global sense-making на корпусах около 1M tokens; LLM-judge: comprehensiveness/diversity. Не hub ablation. [Статья](https://arxiv.org/abs/2404.16130), [официальное описание режимов](https://github.com/microsoft/graphrag/discussions/721). |
| **LightRAG** | Dual-level: low-level entities и high-level themes/relations; mode выбирается запросом/конфигурацией, но published pipeline не является learned entity gate. | Явного `-ln(degree)` нет. Graph neighbourhood ограничивается retrieval budget и relevance. | Нет usage-based decay. | Multi-domain QA, сравнение quality/cost с baseline GraphRAG/RAG; нет чистой hub-degree абляции. [Paper](https://openreview.net/pdf?id=AibDk9Z48i). |
| **HippoRAG** | Query seeds → Personalized PageRank → passages; graph используется штатно. | Да, PPR нормализует переходную вероятность по исходящим рёбрам; это не буквальный ACT-R fan. | Нет ACT-R base-level decay. | Multi-hop QA: MuSiQue, 2WikiMultiHopQA, HotpotQA; retrieval и answer metrics. [Paper](https://arxiv.org/abs/2405.14831). |
| **HippoRAG 2** | PPR остаётся, но добавлены passage nodes, dense passage links и online LLM recognition. Graph traversal не просто выключается для factual queries. | PPR degree normalization; отдельного доказанного `fan penalty` нет. | Нет usage-recency decay. | NaturalQuestions/PopQA, NarrativeQA, MuSiQue, 2Wiki, HotpotQA, LV-Eval; сообщает +7% на associative tasks и улучшение factual memory. Не изолирует degree. [Paper](https://arxiv.org/abs/2502.14802). |
| **RAPTOR** | Это tree retrieval, не wiki/KG expansion. Ищет по chunks и рекурсивным summaries; all-layer или tree traversal. | Неприменимо: нет degree-normalized wiki graph. | Нет decay. | QASPER F1 55,7 против 53,9; QuALITY accuracy 82,6 против 62,3 предыдущего SOTA; all-layer retrieval лучше отдельных уровней. [Paper](https://arxiv.org/abs/2401.18059). |
| **Zep / Graphiti** | Hybrid semantic + BM25 + graph traversal; поддерживает targeted search от центрального node и broad exploration. Это конфигурируемые search recipes, не ACT-R gate. | Можно rerank by graph distance; опубликованного ACT-R degree penalty нет. | Есть bi-temporal validity/invalidation, но не usage-based forgetting. | DMR 94,8% против 93,4% MemGPT; LongMemEval, позднее заявлены LoCoMo 94,7%. [Paper](https://arxiv.org/abs/2501.13956), [docs](https://help.getzep.com/graphiti/getting-started/overview), [search docs](https://help.getzep.com/graphiti/working-with-data/searching). |
| **LlamaIndex Property Graph** | По умолчанию объединяет `LLMSynonymRetriever` и `VectorContextRetriever`; `path_depth` настраивается; можно заменить custom retriever. Не автоматически query-conditional. | В default API не задокументирован fan penalty. | Нет default decay. | Framework, а не опубликованная система с собственным строгим benchmark. [Документация](https://docs.llamaindex.ai/en/stable/module_guides/indexing/lpg_index_guide/). |
| **Neo4j GraphRAG** | Разные retrievers: Vector, Hybrid, VectorCypher, Text2Cypher, ToolsRetriever. Расширение происходит только если выбран traversal/Cypher retriever. | Зависит от пользовательского Cypher; default degree penalty нет. | Зависит от properties/query; base-level decay нет. | Библиотека не публикует общий benchmark, доказывающий преимущество traversal; это конструктор. [Документация](https://neo4j.com/docs/neo4j-graphrag-python/current/user_guide_rag.html). |
| **Letta / MemGPT** | Agent выбирает memory tools; archival memory — vector DB/search, а graph expansion не является базовым механизмом. | Нет. | Memory может редактироваться агентом, но ACT-R decay нет. | MemGPT дал DMR baseline 93,4% в сравнении Zep; отдельного graph expansion benchmark нет. [Letta docs](https://docs.letta.com/v1-sdk/memory/context-hierarchy), [MemGPT repo](https://github.com/deductive-ai/MemGPT). |

**Сводный вывод [established]:** routing и специализированные retrievers — обычная промышленная практика. Буквальный ACT-R `fan + decay` не является консенсусной промышленной архитектурой.

---

## 4. Альтернативы и цена для вашего стека

Шкала: низкая — несколько SQL/Python условий; средняя — новый индекс или небольшой размеченный набор; высокая — обучение/LLM-routing/новая KG-инфраструктура.

| Вариант | Возможный выигрыш | Главный риск | Цена для markdown + SQLite |
|---|---|---|---|
| **Degree-normalized one-hop**: `graph_score = seed_score × edge_weight / log2(2+degree(seed))` | Сдерживает MOC/дневники/people-index hubs, сохраняет граф для обоих типов. | Штрафует полезные тематические хабы; не исправляет слабые типы рёбер. | **Низкая:** degree хранить/считать в SQLite; 20–40 строк логики. |
| **Top-N соседей после semantic scoring** | Ограничивает candidate explosion; ближе к SAGE. | Хороший сосед может не пройти локальный similarity filter. | **Низкая:** rerank соседей отдельно, взять 2–5 на seed. |
| **Typed-edge allowlist** | Часто точнее degree: для entity разрешить `authored/by/works-at`, запретить generic `mentioned`. | Wiki links сейчас, вероятно, не типизированы; ручная разметка. | **Средняя:** типы в YAML/SQLite; не-технарю нужен простой UI/справочник из 5–8 типов. |
| **Personalized PageRank** | Несколько seeds усиливают согласованные пути; degree normalization встроена. | На one-hop малом графе может не дать ничего поверх простой формулы; hubs всё равно могут накапливать rank. | **Средняя:** NetworkX/SQLite adjacency; настройка restart \(\alpha\); желательно offline cache. |
| **Entity exact/alias match → vector chunks той же заметки** | Максимальный precision для человека/компании/артефакта; объяснимо. | Ambiguous aliases; entity может быть распределена по нескольким заметкам. | **Низкая:** таблица `entity_alias(alias,note_id)` + title/frontmatter lookup. Лучшая первая инвестиция. |
| **BM25 + dense + reranker, без graph** | Сильный baseline; exact terms и смысл дополняют друг друга. | Не собирает распределённые multi-hop/theme evidence. | **Низкая/средняя:** SQLite FTS5 + RRF; поддержка почти не нужна. |
| **Query-type rule gate** | Уже согласуется с вашим A/B; прозрачен и легко чинится. | Ошибка классификации mixed queries. | **Очень низкая:** rules + ручной override `graph:on/off`. |
| **LLM/classifier routing** | Может различать entity, theme, temporal, multi-hop, list/aggregation. | Нестабильность, latency, prompt drift; десятков labels мало для обучения. | **Средняя:** zero-shot LLM или logistic regression на embeddings; нужен audit log. |
| **Learned expansion gate per seed/edge** | Теоретически превосходит глобальный gate и fan penalty. SeedER показывает потенциал selective policy. | Нужны сотни/тысячи labelled decisions; трудно ремонтировать не-технарю. | **Высокая:** сейчас не рекомендую. |
| **Reranker-only over raw vector top-50** | Изолирует пользу сильного rerank без graph noise. | Если evidence не попал в top-50, reranker его не создаст. | **Низкая:** обязательный экспериментальный baseline. |
| **Recency/usage decay** | Хорош для «что последнее», active projects, daily notes. | Popularity feedback loop; забывание вечных фактов; не решает topicality. | **Средняя:** timestamps и retrieval counters; применять только к temporal query types. |

### Рекомендуемая простая политика

**[emerging]**

```text
1. Exact title/alias hit?
   Да → entity-note chunks + hybrid lexical/vector; graph off by default.
2. Query просит relations/list/history/multi-hop?
   Да → graph on, но top-3 neighbors/seed и degree normalization.
3. Broad theme/synthesis?
   Да → graph on.
4. Иначе → vector + FTS5 + reranker.
5. Всегда сохранять ручной override.
```

Это не «сложный cognitive architecture», а четыре понятных режима, которые можно диагностировать по журналу.

---

## 5. Проверяемый эксперимент за один вечер

### 5.1 Основной дизайн

Нужно разделить три фактора:

- `Q`: query type — entity / thematic;
- `H`: hub exposure — есть ли среди seed notes или их соседей высокостепенной узел;
- `M`: retrieval method.

Методы:

- **V:** vector top-K → reranker;
- **G:** vector → raw one-hop → reranker;
- **F:** vector → one-hop с fan penalty → reranker;
- **P:** vector → one-hop, но hubs полностью исключены;
- **Gate:** V для entity, лучший из F/G для theme.

`P` важен: один fan penalty может быть слишком слабым. Полное hub removal — сильная интервенция, проверяющая причинную гипотезу.

### 5.2 Запросы

Минимум для одного вечера:

- **40 запросов:** 20 entity + 20 thematic;
- в каждой категории 10 с high hub exposure и 10 с low exposure;
- предпочтительно **60:** 30 + 30, если каждый judgment — только pairwise winner.

Ваши нынешние 12 нетривиальных сравнений можно включить только если методы будут прогнаны заново вслепую.

High hub определить **до просмотра результатов**, например:

```text
hub = degree >= max(20, percentile_95(degree))
hub_exposure = хотя бы один seed или raw-expanded neighbour является hub
```

Дополнительно записывать:

- degree каждого vector seed;
- число соседей до/после фильтра;
- долю candidate budget, пришедшую от top-1 high-degree seed;
- позиции исходных vector results после rerank;
- типы/направление wiki-links, если доступны.

### 5.3 Одинаковые ограничения

Для всех вариантов:

- один embedding model и один reranker;
- одинаковый финальный budget, например 10 chunks;
- одинаковый исходный vector top-K;
- одинаковый chunking;
- одинаковый prompt/display;
- cache результатов;
- анонимные случайно переставленные варианты `A/B/C/D`.

Если G передаёт reranker 200 кандидатов, а V — 20, различается не только граф. Поэтому:

- либо одинаковый candidate cap, например 50;
- либо отдельно измерять `pre-rerank recall` и финальный quality.

### 5.4 Человеческая оценка

На каждый запрос оценить финальный top-5 или top-10:

1. **Utility winner:** какой набор полезнее, forced choice + tie.
2. **Precision@5:** сколько результатов прямо помогают ответить.
3. **Off-topic@5:** сколько результатов относятся к соседнему, но другому предмету.
4. **Coverage 0–2:** отсутствует / частично / достаточно.
5. **Entity substitution:** да/нет — подменена ли конкретная сущность.
6. **Best evidence rank / MRR:** позиция первого достаточного результата.
7. **Judgment confidence:** low/medium/high.

Главная метрика — pairwise utility, потому что она соответствует вашему исходному A/B. Precision@5 и off-topic нужны для объяснения механизма.

### 5.5 Анализ

Для каждой пары методов использовать paired sign test или exact McNemar; ties показывать отдельно. Не полагаться на средний LLM score.

Ключевые сравнения:

| Сравнение | Что проверяет |
|---|---|
| G против V | исходный эффект расширения |
| F против G | достаточно ли fan penalty |
| P против G | причинны ли hubs вообще |
| F против V отдельно по query type | остаётся ли type interaction после штрафа |
| F high-hub против F low-hub | остаточный hub effect |
| Gate против F | нужен ли routing после нормализации |

Можно оценить простую ordinal/logistic regression:

```text
graph_win ~ query_type + hub_exposure + query_type:hub_exposure
```

При 40 запросах коэффициенты будут нестабильны; используйте её как описание, а не как окончательную inferential модель.

### 5.6 Что опровергнет гипотезы

#### Гипотеза H-hub: «деградацию entity объясняют хабы»

Считать её практически опровергнутой, если:

- после удаления hubs (`P`) entity harm сохраняется: `P` проигрывает V хотя бы на 60% нетривиальных entity pairs;
- либо F/P почти не улучшает entity Precision@5 и off-topic;
- либо вред G одинаков при high- и low-hub exposure;
- при этом thematic graph benefit остаётся.

**[emerging]** При 20 entity queries порог `≥12 из 20 проигрышей` — практический, но не строгий статистический. Для двустороннего sign test понадобилось бы примерно `≥15 из 20` побед одного метода без ties для `p<0.05`.

#### Гипотеза H-type: «тип запроса определяет полезность графа»

Считать её опровергнутой, если после fan normalization:

- F не хуже V на entity и остаётся лучше на thematic;
- interaction по query type исчезает;
- результат вместо этого устойчиво разделяется по hub exposure;
- Gate не выигрывает у F и не уменьшает catastrophic harms.

#### Что поддержит обе гипотезы одновременно

Наиболее вероятный результат:

- F заметно уменьшает entity harm, но не устраняет его;
- query type остаётся значимым operational predictor;
- hubs объясняют часть шума, а mismatch retrieval objective — остаток.

Тогда правильна комбинация gate + bounded expansion, а не выбор одного механизма.

### 5.7 Сколько данных нужно на самом деле

- **40 запросов:** хорошее инженерное решение для вашего личного волта.
- **60–80:** разумно для устойчивой локальной рекомендации.
- **100–150 стратифицированных запросов:** уже можно оценивать interaction с приемлемой стабильностью.
- Тысячи не нужны, если цель — ваш конкретный workload, а не публикация.

**Вывод [established]:** 12 наблюдений недостаточно для установления причины, но 40–60 слепых paired comparisons достаточно для решения, какую конфигурацию оставить в личной системе.

---

## 6. Контраргументы против моей рекомендации

### Когда ваш нынешний gate уже является правильным ответом

1. **[established] Разные ошибки имеют разную цену.** Если один неверный entity result хуже, чем пропущенная тематическая связь, precision-oriented routing рационален даже при небольшом среднем преимуществе graph.

2. **[established] Волт мал.** Для 14 000 chunks хороший hybrid lexical+dense retriever с reranker уже может давать высокую entity recall; сложный graph policy решает проблему, которой почти нет.

3. **[emerging] Query types легко различимы.** `Кто/что такое X`, имя в кавычках, точный title/alias — дешёвые и объяснимые признаки. Ради них не нужен learned router.

4. **[established] Maintainability является частью качества.** Не-технарь может понять «для темы включить соседей» и вручную переопределить режим. Ошибку PPR/activation score диагностировать намного труднее.

5. **[established] Usage decay создаёт новые failure modes.** Часто запрашиваемое может стать ещё чаще запрашиваемым; редко используемая, но важная policy note исчезнет из retrieval.

6. **[emerging] Reranker уже выполняет soft filtering.** Если после разумного candidate cap он не спасает entity precision, добавление ACT-R prior может лишь немного переставить тот же шум.

7. **[established] Промышленные аналоги поддерживают routing.** Microsoft GraphRAG не пытается одним global activation rule одинаково отвечать на targeted и thematic вопросы.

### Аргумент против gate

Gate ошибается на смешанных запросах:

> «Что мы обсуждали с Иваном о найме?»

Это entity anchor, но thematic/relational intent. Жёсткое правило `есть имя → graph off` пропустит нужные соседние notes.

Поэтому gate должен классифицировать **intent**, а не наличие named entity:

- `attribute/fact about X` → exact/entity mode;
- `relations/history/thoughts involving X` → graph mode.

### Когда я бы убрал gate

**[emerging]** Только если ваша локальная абляция покажет одновременно:

- F/P не уступает V на entity;
- F заметно лучше V на thematic;
- catastrophic entity harms исчезли;
- performance определяется hub exposure, а не intent;
- правило остаётся устойчивым при разных K и candidate budgets.

До такого результата удалять gate — преждевременно.

---

## Главное допущение рекомендации

**[speculative]** Я предполагаю, что ваши wiki-links семантически неоднородны: часть означает сильную предметную связь, а часть — навигацию, упоминание, индексирование или организационную принадлежность.

Если на самом деле почти все ссылки строго типизированы и означают answer-bearing relation, а все пять entity harms действительно проходят через несколько огромных MOC-хабов, вывод меняется: degree cap или PPR может устранить проблему, и query-type gate станет избыточным.

Это допущение проверяется очень дёшево: вручную классифицировать все expanded candidates из пяти вредных entity cases как:

```text
hub-mediated / non-hub
answer-bearing / merely related / unrelated
```

Если ≥80% вредных результатов пришли через high-degree hubs и исчезают при их удалении без потери полезных entity neighbors, позиция другого ресёрча получает сильную локальную поддержку.

---

## 7. Первичные источники

1. Edge et al., *From Local to Global: A Graph RAG Approach to Query-Focused Summarization* — “global questions directed at an entire text corpus” — https://arxiv.org/abs/2404.16130  
2. Microsoft GraphRAG, официальное объяснение Local/Global Search — “Local search uses entities as the entry point” — https://github.com/microsoft/graphrag/discussions/721  
3. Microsoft Research, Project GraphRAG / DRIFT — “Combining global and local search methods” — https://www.microsoft.com/en-us/research/project/graphrag/  
4. Gutiérrez et al., *HippoRAG* — “Personalized PageRank algorithm” — https://arxiv.org/abs/2405.14831  
5. Gutiérrez et al., *HippoRAG 2* — “performance on more basic factual memory tasks drops considerably” — https://arxiv.org/abs/2502.14802  
6. LightRAG, *Simple and Fast Retrieval-Augmented Generation* — “Dual-level Retrieval Paradigm” — https://openreview.net/pdf?id=AibDk9Z48i  
7. Sarthi et al., *RAPTOR* — “improve the best performance on the QuALITY benchmark by 20%” — https://arxiv.org/abs/2401.18059  
8. Rasmussen et al., *Zep: A Temporal Knowledge Graph Architecture for Agent Memory* — “94.8% vs 93.4%” — https://arxiv.org/abs/2501.13956  
9. Graphiti repository — “temporal context graph engine” — https://github.com/getzep/graphiti  
10. Graphiti search documentation — “broad exploration and targeted, entity-specific information retrieval” — https://help.getzep.com/graphiti/working-with-data/searching  
11. LlamaIndex Property Graph documentation — default `LLMSynonymRetriever` and `VectorContextRetriever` — https://docs.llamaindex.ai/en/stable/module_guides/indexing/lpg_index_guide/  
12. Neo4j GraphRAG documentation — “combining vector-based similarity searches with graph traversal” — https://neo4j.com/docs/neo4j-graphrag-python/current/user_guide_rag.html  
13. Letta memory documentation — “Archival memory is a general-purpose vector DB” — https://docs.letta.com/v1-sdk/memory/context-hierarchy  
14. MemGPT repository — archival-memory retrieval implementation — https://github.com/deductive-ai/MemGPT  
15. Thomson et al., *An Account of Interference in Associative Memory: Learning the Fan Effect* — association strength attenuated by fan — https://onlinelibrary.wiley.com/doi/full/10.1111/tops.12244  
16. Schneider & Anderson, *Modeling Fan Effects on the Time Course of Associative Recognition* — “standard equations for declarative memory retrieval in ACT-R” — https://pmc.ncbi.nlm.nih.gov/articles/PMC3259266/  
17. West et al., *Interference and ACT-R: New Evidence from the Fan Effect* — “cannot be accounted for using ACT-R” — https://citeseerx.ist.psu.edu/document?doi=0aebaa4fcd5c1fffff700d7c39474877591dfbe1&repid=rep1&type=pdf  
18. Fisher et al., *On the Limits of Spreading Activation in ACT-R* — evidence mainly at fan size 1–4 — https://mathpsych.org/presentation/725  
19. Crestani, *Application of Spreading Activation Techniques in Information Retrieval* — critical survey — https://doi.org/10.1023/A:1006569829653  
20. Hughes & Ramage, *Measuring Conceptual Similarity by Spreading Activation over Wikipedia’s Hyperlink Structure* — degree treatment for highly connected nodes — https://aclanthology.org/W10-3506.pdf  
21. SAGE, *Structure Aware Graph Expansion for Retrieval of Heterogeneous Data* — “filtering the neighbors using dense+sparse retrieval” — https://arxiv.org/abs/2602.16964  
22. SeedER, *Seed-and-Expand Retrieval from Knowledge Graphs* — “ego-graph expansion grows rapidly” — https://arxiv.org/abs/2605.23753  
23. Xiao et al., *GraphRAG-Bench* — 1 018 вопросов, 16 дисциплин, 20 учебников — https://arxiv.org/abs/2506.02404  
24. *The Reasoning Bottleneck in Graph-RAG* — gold context 77–91%, accuracy 35–78% — https://arxiv.org/abs/2603.14045  
25. Reddy & Challaram, *Don’t Ask the LLM to Track Freshness* — conflict-resolution comparison — https://arxiv.org/abs/2606.01435  

**Финальная рекомендация [emerging]:** пока сохранить intent gate; добавить exact alias lookup, одинаковый candidate budget и `top-N + degree normalization` внутри разрешённого expansion. Удалять gate только после factorial A/B, где hub intervention действительно устраняет entity harm.
