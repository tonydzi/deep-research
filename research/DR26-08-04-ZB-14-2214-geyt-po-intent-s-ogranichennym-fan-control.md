---
dr_id: DR26-08-04-ZB-14-2214
title: "гейт по intent с ограниченным fan-control"
date: 2026-08-10
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# гейт по intent с ограниченным fan-control

## Вердикт для потребителя

выбрать гибрид: оставить query-type gate как основной рубильник, не добавлять ACT-R time-decay, а degree-normalized fan-control тестировать только внутри разрешённого graph-режима. для точного факта или атрибута сущности граф по умолчанию выключен; для темы, связей, истории и multi-hop включён с лимитом соседей и штрафом степени. ответ переворачивается в always-on graph только если слепой локальный тест покажет, что fan-penalty не уступает vector-only на entity-запросах, сохраняет выигрыш на тематических запросах и убирает тяжёлые ошибки. если fan-control не улучшает raw graph, оставить текущий гейт без усложнения.

## Ключевые выводы

- **4/4:** хабы создают fan-out и шум, но ни одна рельса не дала прямого независимого доказательства, что только хабы объясняют локальный провал `1 помог / 5 навредил` на entity-запросах.
- **4/4:** тип запроса меняет функцию потерь. entity lookup требует precision, тематический поиск покупает coverage и recall. поэтому routing по intent остаётся рациональным даже при нормализации степени.
- **4/4:** буквальная связка ACT-R `fan-penalty + base-level decay` не является подтверждённым стандартом RAG. опубликованные системы используют routing, PPR, лимиты, фильтры, типы рёбер и bi-temporal validity.
- **4/4:** time-decay не лечит semantic drift. частота обращения может закреплять старую ошибку, а старая policy-note может оставаться важной.
- **3/4:** мягкая нормализация степени или PPR может уменьшить hub-noise, но её вклад не отделён от других механизмов в промышленных системах.
- **3/4:** Microsoft GraphRAG разделяет local entity и global thematic режимы; это поддерживает intent-routing, но не является прямым тестом вашего wiki-графа.
- **3/4:** HippoRAG 2 устраняет часть factual regressions пакетом из PPR, passage nodes и query-to-passage links, а не одним ACT-R fan-penalty.
- **2/4:** текущие 12 нетривиальных сравнений дают сильный инженерный сигнал направления, но не устанавливают причину. оценки статистической значимости есть только в одной сильной рельсе и не считаются независимым подтверждением.
- **2/4:** chatgpt и grok независимо пришли к одному production-решению, но частично опирались на одни и те же GraphRAG, HippoRAG 2 и ACT-R источники. это согласие по общей доказательной базе, а не четыре независимые репликации.

## Где рельсы разошлись

| пункт | chatgpt | gemini | glm | grok | кому верить и почему |
|---|---|---|---|---|---|
| финальная политика | gate + exact match + bounded degree-normalized expansion | gate оставить, экспериментировать | gate оставить, предлагает PageRank-normalization | gate оставить, fan только как страховку | chatgpt и grok: вывод привязан к первичным работам и прямо отвечает на решение |
| Microsoft GraphRAG | conditional local/global | ошибочно описывает expansion как always-on | conditional local/global | conditional local/global | chatgpt и grok: совпадают с официальной архитектурой local/global |
| HippoRAG | PPR даёт degree normalization | утверждает, что penalty нет и проблему решает иерархия | PPR трактует как hub-control | PPR, не ACT-R | chatgpt и grok: точнее отделяют PPR от буквального ACT-R |
| LightRAG и degree | явного `-ln(degree)` нет | ограничение `k` называет fan-penalty | заявляет веса рёбер как penalty без чистой абляции | отмечает ранжирование высокой степени вверх | grok для факта о degree-ranking, chatgpt для осторожного вывода; gemini и glm смешивают cap, edge weight и fan-penalty |
| Zep / Graphiti | bi-temporal validity, не usage-decay | заявляет decay и degree control без надёжного основания | смешивает bi-temporal graph и decay | temporal invalidation, не ACT-R BLL | chatgpt и grok: различают срок истинности факта и забывание по обращениям |
| цена exact entity linking | низкая: title/alias lookup | очень высокая: предполагает NER и отдельную базу | средняя | низкая при alias table | chatgpt и grok: для этого волта достаточно title/frontmatter/alias, NER не обязателен |
| размер вечернего теста | 🤔 40 минимум, 60 предпочтительно | 🤔 45 | 🤔 30-50, затем приводит расчёт около 64 | 🤔 40 | это проектные гипотезы, не измерение волта; взять 40 как выполнимый пилот, не как статистическую гарантию |
| качество источников | 25 в основном первичных ссылок, конкретные ограничения | много общих и вторичных ссылок, есть неверные paper ids | много поисковых сниппетов, Medium/LinkedIn и неподтверждённых формулировок | 19 преимущественно первичных и официальных ссылок | rails_valid: chatgpt, grok; rails_weak: gemini, glm |

## Что конкретно менять в коде (если менять)

```yaml
router:
  entity_attribute_or_exact_fact:
    graph_enabled: false
    retrieval: exact_alias_then_lexical_dense_rerank
  theme_relation_history_multihop:
    graph_enabled: true
    expansion: bounded_degree_normalized_1hop
  ambiguous:
    graph_enabled: false
  manual_override: [graph_on, graph_off]

graph_candidate_score:
  formula: "seed_score * edge_weight / sqrt(1 + out_degree(seed))"
  actr_base_level_decay: disabled
  retrieval_count_boost: disabled
  time_decay: disabled

candidate_budget:
  vector_seed_k: "unchanged_from_current_baseline"
  neighbors_per_seed: "🤔 5, зафиксировать до пилота"
  pre_rerank_total: "🤔 не больше 2 * vector_seed_k"
  final_k: "equal_across_all_test_arms"

shadow_ablation:
  query_count: "🤔 40 total: 20 entity + 20 thematic"
  arms: [vector_only, raw_1hop, fan_normalized_1hop, hub_drop_1hop, current_gate]
  blind_order: true
  primary_metric: pairwise_utility_vs_vector_only
  diagnostics: [precision_at_5, off_topic_at_5, entity_substitution, first_relevant_rank, hub_noise]
  adopt_fan_control_if:
    entity: "fan_normalized_1hop >= vector_only"
    thematic: "fan_normalized_1hop >= raw_1hop"
    catastrophic_entity_harm: "none observed in pilot"
  remove_gate_if:
    all_conditions_required:
      - "fan_normalized_1hop >= vector_only on entity"
      - "fan_normalized_1hop >= raw_1hop on thematic"
      - "result separates by hub exposure, not query intent"
      - "stable under at least two candidate budgets"
```

`🤔` означает стартовую гипотезу из отчётов, а не установленный порог. значения нельзя переносить в production без локального shadow-теста.

## Открытые вопросы / чего в отчётах нет

- никто не измерил degree distribution, долю hub-mediated кандидатов и типы wiki-ссылок именно в этом волте.
- никто не разобрал пять вредных entity-кейсов на `hub-mediated / non-hub` и `answer-bearing / related / unrelated`.
- нет чистой опубликованной абляции `query intent × hub exposure × fan penalty` на персональном markdown-волте.
- нет измеренного выигрыша ACT-R base-level learning или usage-decay для `brain_ask`.
- неизвестно, вытесняет ли graph expansion исходные vector hits из фиксированного candidate budget или проблема возникает уже после reranker.
- неизвестна точность текущего query-type gate на смешанных запросах вида «что мы обсуждали с X о Y».
- не измерены latency, стоимость и ремонтопригодность вариантов на текущем SQLite-пайплайне.
- числа для объёма пилота и cap соседей являются 🤔 инженерными стартовыми значениями, а не результатами power analysis на локальной дисперсии.

## Провенанс

- «внутренний архив лаборатории»
- «внутренний архив лаборатории»
- «внутренний архив лаборатории»
- «внутренний архив лаборатории»
