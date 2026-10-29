---
dr_id: DR26-09-16-MACANTON-02-0453
title: "Синтез DR26-09-16-MACANTON-02-0453 — вторая память: какие репо взять в Graph RAG над волтом, какие тесты ставить, и почему это «память», а не «мозг»"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Синтез ДР: вторая память — репо, тесты, модели, термин

> **Собрано 3/6:** ChatGPT/codex (43 источника, 33 КБ) · claude.ai CLI (103 источника, 48 КБ) · Grok (58 источников, 39 КБ). **Не собрано:** Gemini (CLI пустое тело; Firefox «не нашёл пункт Deep Research»), GLM (слайдер-капча, эхо промпта), Mistral (двери на Mac16 нет). Кворум 4/6 не добран, пол «3 разные LLM» выполнен; недобор докупает `dr_latecomer_sweep.py`.
> Отчёты: «внутренний архив лаборатории». Промпт: там же, `queue/DR26-09-16-MACANTON-02-0453.md`.
> Заказ: голосовая Антона 16.09 04:47 («прошу Claude провести глубокое исследование… репозитории на GitHub… тесты для второго мозга… второй мозг — это вторая память»).
> Связи: sqlite-graph-memory · second-brain-starter-kit · vault-data-architecture · always-on-memory-pilot · memory-records-close-by-fields-not-rewrite · watchdog-must-verify-the-item · MOC-AI-Agents · mission-get-noticed-hired-by-llm-company
> Таблица (Google Sheets): см. строку «Таблица» в конце файла.

## 1. Вердикт в семи строках
1. **Конвейер не менять, модули занимать.** Все 3: Mem0 / Letta / Cognee / LightRAG / MS GraphRAG решают другую задачу (память диалога, рантайм агента, LLM-извлечённый граф) и тянут сервис, граф-БД или LLM на ингесте. Волт + wikilinks + SQLite остаются истиной.
2. **Сначала золотой набор, потом любая замена модели.** Все 3, одними словами: «пока нет gold set, каждая замена модели — суеверие» (Grok). Wikilinks = бесплатная разметка релевантности, `superseded_by`/`valid_*` из §8.4-бис = бесплатный темпоральный тест.
3. **Гибрид: FTS5/BM25 + RRF рядом с dense.** Все 3. Закрывает слабость эмбеддингов на именах, ID и русской морфологии; FTS5 уже внутри SQLite.
4. **Модели: BGE-M3 как первый претендент, bge-reranker-v2-m3 как первый реранкер.** Grok принёс единственный русский замер (rus-MIRACL nDCG@10: multilingual-e5-base 61.41 → BGE-M3 70.50 → BGE-M3 + bge-reranker-v2-m3 76.44, Dialogue 2025). ChatGPT: BGE-M3 «первый эксперимент». Claude: multilingual-e5-large как малый риск, BGE-M3 как апгрейд. ⚠️ Claude писал «e5-base = English-only» — это ошибка ПРОМПТА (я написал e5-base), в репо стоит `multilingual-e5-base`; вывод «сменить модель» остаётся, диагноз «русский не представлен вовсе» — нет.
5. **sqlite-vec вместо `.npy/.pkl`** — adopt (Claude, Grok; ChatGPT «если упаковка на Mac/Win пройдёт»). Personalized PageRank из HippoRAG поверх СУЩЕСТВУЮЩИХ wikilinks вместо тупого 1-hop/40 — borrow (все 3). Битемпоральная схема Graphiti как колонки в SQLite, без Neo4j — borrow (все 3, и это уже наш `docs/bitemporal.md`).
6. **Консолидация = ночной job, «закрыть, не удалить», LLM никогда не удаляет.** Все 3. Claude добавил уникальную улику: Reddy & Challaram (arXiv 2606.01435) — одна LLM, решающая «что совпало + новее побеждает», даёт 54%/7%; разделение на LLM-извлечение + детерминированную политику даёт 82–93%/27–41%. Для волта, который правит человек: **human-edit-wins**, обратное Graphiti recency-wins.
7. **Термин: «вторая память» защищаем, «второй мозг» — это слой мышления поверх.** Все 3. Линия Bush (Memex, 1945) → Bell/Gemmell (MyLifeBits, e-memory) → Clark & Chalmers (extended mind) → клинический «протез памяти». Forte = методология для здоровой памяти; Claude: Forte в 2026 переименовал дисциплину в «Personal Context Management» (один источник, 🤔 не перепроверено). Поле сходится на «agent memory / memory layer», не на «second brain».

## 2. Q1 — репозитории: сводная таблица (голоса = сколько из 3 отчётов)
| # | Репо | Вердикт | Что взять (одна вещь) | Цена | Голоса |
|---|---|---|---|---|---|
| 1 | asg017/sqlite-vec (~8.1k★, MIT/Apache) | **adopt** | `vec0` virtual table вместо `.npy/.pkl`; вектора рядом с `ab_recall` и `turns` | 4–20 ч, C-расширение, ноль сервисов; pre-v1 → закрепить версию | 3 |
| 2 | OSU-NLP-Group/HippoRAG (2) (MIT, arXiv 2502.14802) | borrow | Personalized PageRank по графу wikilinks + passage-узлы; НЕ OpenIE — рёбра у нас уже лучше | 12–24 ч на NetworkX | 3 |
| 3 | getzep/graphiti (~30.9k★, Apache) | borrow схему | `valid_from/valid_to/observed_at`, invalidate-not-delete, эпизоды как провенанс | 1–2 дня; Neo4j НЕ ставим | 3 |
| 4 | basicmachines-co/basic-memory (~4k★, **AGPL**) | borrow идею, код не копировать | ближайший родственник: markdown+wikilinks+SQLite+FTS+vector+reranker; `doctor` (сверка файл↔БД); `build_context` | 8–16 ч переписать | 3 |
| 5 | mem0ai/mem0 (~65k★, Apache) | borrow | (а) ADD-only извлечение, старое не перезаписывать; (б) `memory-benchmarks` как шаблон скоринга; (в) semantic+BM25+entity fusion | 8 ч на harness; облако ⛔ для волта | 3 |
| 6 | langchain-ai/langmem (~1.7k★, MIT) | borrow фрейминг | hot-path tools vs background memory manager → ночной консолидатор | 8–12 ч | 3 |
| 7 | letta-ai/letta (Apache; ушёл в letta-code) | borrow политику | core (пиннутые MOC + последние N решений) vs archival (волт); sleep-time «dreaming» как ночной job | 8 ч | 3 |
| 8 | HKUDS/LightRAG (~39.7k★, MIT) | borrow | dual-level entity/theme = наш `looks_like_entity`; heading-aware чанкер | 4 ч роутинг; полный KG-extract ⛔ | 3 |
| 9 | jina-ai/late-chunking (paper 2409.04701) | borrow | эмбед всей заметки, пул токенов в чанки; нужен 8k-контекст (BGE-M3) | 4–8 ч | 1 (Grok) |
| 10 | BAAI/FlagEmbedding BGE-M3 | adopt как претендент | dense+sparse+multi-vector одним моделью, RU+EN, 8192 | 8–20 ч + переиндекс | 3 |
| 11 | agiresearch/A-mem (MIT, NeurIPS 2025) | borrow только на ингесте | Zettelkasten-write-back: новая заметка → линки/теги соседей; на 100k полный прогон ⛔ | 8–16 ч | 2 |
| 12 | gusye1234/nano-graphrag | borrow мелочь | content-hash как ключ идемпотентного индекса | ½ дня | 2 |
| 13 | neuml/txtai | borrow идею | «одна БД = dense + sparse + graph + SQL» | скип как фреймворк | 2 |
| 14 | topoteretes/cognee (~30.7k★) | skip adopt | `eval_framework`, session-cache→promote | тяжёлая платформа | 3 |
| 15 | VectifyAI/PageIndex (~35.7k★) | skip/позже | vectorless tree для длинных PDF/канона | ниша | 3 |
| — | khoj (AGPL), obsidian-copilot (AGPL), smart-connections (paywall), MS GraphRAG (maintenance), MemoRAG/memary/A-mem-старый, Verba (archived), R2R, RAGFlow, Chroma, LanceDB | skip | не тот слой или не тот масштаб | — | 3 |

Уникальные находки одного вендора (🤔 перепроверить перед тем, как строить): `bartkamphuis/memore` — детерминированная консолидация без LLM в пути решения (Claude); `mem-agent-mcp` `BaseMemoryConnector` как паттерн ингеста рядом с voice2brain (Claude, репо неактивен ~10 мес).

## 3. Q2 — тесты: что ставить первым
**Бенчмарки = таксономия вопросов, не табло.** LoCoMo, LongMemEval (и V2 05.2026), MemBench, PersonaMem, MSC — про диалог, не про волт заметок; вендорские 92–94% на LoCoMo — managed-платформа и маркетинг (Grok: независимые реплики расходятся на десятки пунктов). Ближайший публичный кузен по ДИЗАЙНУ — TREC iKAT (Personal Textual Knowledge Base). Needle-in-haystack — про длинный контекст, для 100k-волта бесполезен. Ranx / ir-measures / sklearn — ноль API, ставить первыми; RAGAS/DeepEval/Phoenix — судья заменяем на локальный; Phoenix — SQLite по умолчанию, самая близкая по форме.

**Рецепт золотого набора (0 API):** 200 → 600 вопросов из волта, стратификация по языку/типу/степени связности; классы: entity · theme · bridge (два узла через wikilink) · compare · temporal (`valid_from`/`superseded_by`) · unanswerable; gold = путь заметки + 1-hop соседи с пометкой relevant/noise; hard negatives = near-dup из `/dedup` (cos > 0.90–0.92); JSONL `{id, query, lang, class, gold_paths[], gold_graph_paths[], hard_negatives[], answer_span?, as_of?}`; скоринг по file-id: Recall@12, Recall@60, MRR, nDCG@12; срез по классу × языку.

**Пять первых тестов и порог «хорошо» (инженерные цели для ЭТОГО волта, не научная норма):**
| # | Тест | Порог |
|---|---|---|
| 0 | Свежесть индекса: `mtime > indexed_at` | 0 после ночного job; жёлтый < 1 % |
| 1 | Recall@12 по gold-файлам, vector-only vs +graph, по классам | entity ≥ 0.90 · theme ≥ 0.80 · bridge ≥ 0.70; граф обязан бить вектор на entity И bridge, иначе гейт остаётся |
| 2 | nDCG@12 с hard negatives, rerank on/off | ≥ 0.70 всего, ≥ 0.75 после rerank; дельта < 0.02 = реранкер-театр |
| 3 | Загрязнение top-12 superseded/near-dup | < 5 % |
| 4 | Темпоральные/`superseded_by` цепочки | 60–70 % (LongMemEval даёт ~30 п.п. просадки полю) |
| 5 | Faithfulness `/ask` на 50 held-out (локальная 6-грамм/косинус ≥ 0.75) | ≥ 0.90; retrieval зелёный + это красное = LLM игнорирует чанки |

## 4. Q3 — модели (RU+EN, локально)
| Модель | Роль | Лицензия | Улика | Вердикт |
|---|---|---|---|---|
| multilingual-e5-base (текущая) | эмбеддер | MIT | rus-MIRACL 61.41; контекст 512 = настоящая дыра | контроль |
| multilingual-e5-large(-instruct) | эмбеддер | MIT | rus-MIRACL 66.99; Mr.TyDi RU MRR@10 65.8 | претендент малого риска |
| **BGE-M3** | эмбеддер | MIT | rus-MIRACL **70.50**, 8192 ctx, dense+sparse+multi-vector | **первый эксперимент** |
| Qwen3-Embedding-0.6B / 4B / 8B | эмбеддер | Apache | MMTEB 64.33 / 69.45 / 70.58; RU-числа not found | 0.6B после BGE-M3; 4B/8B только на десктопных GPU |
| jina-embeddings-v3 / jina-reranker-v2 | — | **CC-BY-NC** | сильные, но NC | ⛔ в публичном MIT-репо |
| USER-bge-m3 (deepvk) | эмбеддер | ? | rus-MIRACL 67.23 < BGE-M3; проигрывает на смешанном RU+EN | не единственной моделью |
| mmarco-mMiniLMv2-L12 (текущий) | реранкер | MIT | базовый | заменить в A/B |
| **bge-reranker-v2-m3** | реранкер | Apache/MIT | BGE-M3 + он: rus-MIRACL 70.50 → **76.44** | **первая замена** |
| Qwen3-Reranker-0.6B / 4B | реранкер | Apache | MTEB-R 65.80 / 69.76, MMTEB-R 66.36 / 72.74 | A/B после bge; 4B — если 0.6B упёрся |
| ColBERT / jina-colbert-v2 | late interaction | MIT / NC | нужен PLAID-индекс | позже, если multi-vector BGE-M3 не хватит |

Латентность на 60 кандидатов **не измерена ни одним вендором** — мерить на Mac16 MPS и на 2-GPU десктопе как «тест 0» харнесса. Числа MIRACL расходятся на 10–20 пунктов между подмножествами разных статей — сравнивать только внутри одной таблицы.

## 5. Q4 — консолидация / забывание / битемпоральность
- Заметки = источник истины; производные таблицы могут закрывать окна, но не переписывать markdown без гейта.
- **Close, don't delete:** `valid_to = now` на исчезнувшем wikilink; recall предпочитает `valid_to IS NULL`.
- Провенанс на каждой строке: `source_id, observed_at, confidence, source_kind (human_edit | agent_write)`. **Human-edit-wins** по умолчанию (наша модель доверия обратна Graphiti).
- Противоречия не схлопывать LLM-кой на ингесте: обе заметки остаются, `contradicts Other`, конфликт всплывает в `/ask`. Разделять извлечение (LLM) и политику (детерминированный компаратор `(subject_key, valid_from, source_kind)`).
- Near-dup merge = `/dedup` как консолидатор: канон + `superseded_by` у проигравшего, архивная папка, исключение из открытого индекса.
- Забывание = ранжирование: `retrieval_prior = base × recency_decay × access_boost × link_boost × authority × pin` — только после relevance, никогда как триггер удаления; уникальное автобиографическое событие не затухает.
- «Сон» = ночной dry-run, который ПРЕДЛАГАЕТ кластеры/резюме и называет затронутые источники; автоматически рождаются только производные записи.

## 6. Q5 — термин
- **Вторая память** = non-parametric, provenance-preserving хранилище записей человека + ассоциативный recall (vector + graph + rerank), отвечающее «что было верно, когда, по какой заметке», включая периоды, которые биологическая память не достаёт. Не решает — помнит и находит. Это `sqlite-graph-memory` + волт.
- **Второй мозг** = слой мышления/решений, который ПОЛЬЗУЕТСЯ второй памятью: суждение, планы, аутрич, код, разбор противоречий. Это Claude Code + скиллы + `/decide`.
- **Цифровой двойник / AI-клон** = второй мозг + вторая память + голос с правом действовать от лица Антона под гейтами. Это цель №1, отдельный продукт, не синоним.
- Родословная: Memex (Bush 1945, «enlarged intimate supplement to memory») → MyLifeBits / e-memory (Bell & Gemmell) → extended mind (Clark & Chalmers 1998, notebook Отто) → transactive memory (Wegner) → клинический memory/cognitive prosthesis. Forte/PARA = «фабрика, не склад», для здоровой памяти. Exocortex = претензия на мышление. PKG = инженерное имя того, что уже есть wikilinks.
- Куда сходится поле 2025–26: research/vendors — «agent memory», «memory layer», «temporal context graph»; PKM/Obsidian — «second brain» (Khoj: «Your AI second brain»); Mem0 07.2026: «a second brain is not memory». В статьях и в README писать «second memory»; «second brain» оставить за слоем агента; с торговой маркой Forte в потребительской копии не воевать.
- Глоссарий RU: вторая память · второй мозг · протез памяти · экзокортекс · транзактивная память · расширенный разум · персональный граф знаний · битемпоральная память (когда факт был верен / когда система узнала) · цифровой двойник.

## 7. Q6 — что ломается и ночная диагностика (`rag_health`, одна таблица)
| Поломка | Диагностика (0 API) | Починка |
|---|---|---|
| Чанк режет смысл («он решил» без имени) | `anaphora_chunk_rate` на 1 % выборке: чанк начинается с местоимения/«this»/«он» без имени собственного | title + H1 в каждый чанк; late chunking |
| Протухший индекс | `count(*) WHERE mtime > indexed_at` + sha256 vs хеш индекса (ловит git checkout без mtime) | инкрементальный реиндекс по хешу; удалять вектора пропавших путей |
| Дубли в top-12 | pairwise cos ≥ 0.92 или одинаковый title внутри top-12: `dup_in_top12` | дедуп на индексе; фильтр `superseded_by` |
| Link rot | распарсить `…`, цели без файла, сироты без входящих | мёртвые рёбра не расширять |
| RU↔EN асимметрия | парные билингв-вопросы; Recall@12 RU→RU / EN→EN / RU→EN / EN→RU; флаг > 20 п.п. | BGE-M3 / Qwen3; EN-only модели ⛔ |
| Entity vs theme | `--ab` срез по `looks_like_entity`; флаг, если entity-срез отстаёт > 20 п.п. | BM25-фолбэк на entity-запросах; гейт тюнить на gold |
| Пере-расширение графа | «survival rate» = выжившие в top-12 / предложенные (≤ 40); хронически < 10 % | не расширять через hub-MOC; PPR вместо 1-hop |
| Реранкер верит заголовкам | ablation: rerank title-only vs body-only vs both; title-only ≈ full = ленивый | передавать тело, не имя файла (наши `reglament-*` слаги — риск) |
| Смешанные версии факта | два чанка одной сущности, разный `observed_at`, оба в top-12 | битемпоральный фильтр `valid_to IS NULL` |
| Дубли сущностей в графе | «Anton»/«Антон»/«tonydzi» три узла | alias-таблица, не LLM-извлечение |
| Citation drift | `cited_path not in retrieved_paths` | fail ответа; класс уже в `/tt` |
| Реранкер не доказан | nDCG on/off на gold; дельта < 0.02 | сменить или убрать |

Сторож читает ВЫХОД (`ab_recall`, gold-скоры), не процесс индексатора (watchdog-must-verify-the-item); алерт в 03, если канареечный Recall@12 упал > 5 пунктов к прошлой неделе.

## 8. План на 2 недели (слияние трёх планов, по выигрышу на час)
1. Заморозить 200-вопросный gold JSONL из волта (entity/theme/bridge/temporal/RU/EN/unanswerable), пути — из wikilinks. ~6 ч. Отпирает всё остальное.
2. Скоринг `brain_ask.py --ab` → Recall@12/60, MRR, nDCG@12 в SQLite; без LLM-судьи. ~3 ч.
3. Инкрементальный индекс: content-hash + generation id; удалять вектора пропавших файлов; счётчики stale/missing/orphan/dup ПЕРЕД каждым замером. ~4 ч.
4. FTS5/BM25 + RRF с dense top-60 до реранка. ~4 ч.
5. Эмбеддер → BGE-M3 (dense и dense+sparse), полный реиндекс, прогон gold; регресс на смешанном RU+EN → Qwen3-Embedding-0.6B / multilingual-e5-large. ~6 ч + 1 ч.
6. Реранкер → bge-reranker-v2-m3, A/B на тех же top-60; оставить только если nDCG@12 растёт; потом Qwen3-Reranker-0.6B. ~2 ч.
7. Title + H1 в каждый чанк (бедный late chunking). ~1 ч.
8. `sqlite-vec` `vec0` вместо `.npy/.pkl`, версия закреплена. ~4–8 ч.
9. Гейт сущностей затянуть на gold, потом PPR по wikilinks (NetworkX → SQLite) вместо 1-hop/40. ~8 ч.
10. Материализовать рёбра с `valid_from/valid_to/observed_at/source_kind`, human-edit-wins; ночной `rag_health` с алертом в 03. ~9 ч.

⛔ В эти две недели НЕ: Mem0/Letta/Cognee как рантайм, Neo4j, MS GraphRAG, файн-тюн эмбеддера, платный API-судья, 4B/8B модели до того, как gold скажет, что 0.6B — узкое место.

## 9. Расхождения и что не проверено
- Claude: «e5-base English-only, русский не представлен» — ложная предпосылка из моего промпта; в репо `multilingual-e5-base` (проверено `gh api` 16.09). Рекомендация сменить модель держится на замере Grok, не на этом тезисе.
- Claude: Forte переименовал BASB в «Personal Context Management» (2026) — один вторичный источник, 🤔.
- ChatGPT: HippoRAG «последняя активность/звёзды not found»; Claude: 2026-09-03, ~4.0k★; Grok: 2025-09-04. Дата активности HippoRAG **не сходится** — перед тем, как брать код, открыть страницу коммитов самим.
- Числа звёзд по 4 репо расходятся на сотни между вендорами — брать из таблицы как порядок, не как факт.
- Латентность на 60 кандидатов: не измерена никем, «not found» честно у всех троих.
- Gemini/GLM/Mistral не собраны; недобор докупает latecomer-sweep, синтез при их приходе не переписываем, дописываем §9.

## 10. Для контента (§9.4)
Тизер: «Второй мозг — не мозг. 3 из 6 LLM независимо сказали: то, что я строю, называется вторая память (Bush 1945 → Clark & Chalmers), а мозг — это слой, который ею пользуется. Первый шаг к качеству — не новая модель, а золотой набор из собственных wikilinks. Исследования и синтез — в комментариях.» Черновик уже в воронке (`voice_triage.py append`, 16.09 04:59), тизер через `dr_post_draft.py`.

Таблица: https://docs.google.com/spreadsheets/d/1o3osFhLNn4lIqjEGqkBkDAqKrAaCMxc3Bdn-8zi8iCo (вкладки Репо · Модели · Тесты · План 2 нед; доступ поимённо a@)

### Baseline линейки, Mac16 16.09 11:31 (дописано после синтеза)
multilingual-e5-base + mmarco-mMiniLMv2, 27 061 чанк, 46 мин на CPU/mps, gold 184. Recall@12 vector: title 0.933 ✅ · body 0.667 ❌ · bridge 0.177 ❌ · temporal 0.750 (n=4). nDCG@12 vector: 0.924 · 0.519 · 0.127 · 0.385. Граф: bridge nDCG +0.026 (порог полезности +0.03 не взят), body +0.005. RU nDCG 0.484 против EN 0.662: разрыв 0.18 — первая своя улика в пользу вывода Grok про модель под русский (§4). Скоры: `_imports/eval/scores-20260916-1131.jsonl`, SQLite gold_eval 12 строк. Программа: task-2026-09-16-second-memory-quality-program.
