# DR26-10-26-HUB-03-1338 — the exact prompt sent to every model

# DR26-10-26-HUB-03-1338 — Как LLM думает на разных слоях, особенно глубоких: layerwise computation и causal interpretability

---
title: "Как LLM вычисляет ответ по слоям: recall и заказ Deep Research"
type: reference
stage: proposed
date_added: 2026-10-26
related: ["teach-2026-10-18-how-llm-thinks", "teach-2026-10-18-how-llm-thinks-corrections-v2", "2026-10-11-glubokoe-issledovanie-llm-6acaed4e"]
sources: ["https://приватный чат", "https://notebooklm.google.com/notebook/237d34bf-3644-4fda-9655-7df16cca750b"]

corr_id: voice001:<chat-id>:t1793020565
---


# Recall: что уже изучали

Это пакет заказа исследования, не новый научный отчёт. Научные утверждения прежних материалов перечислены как предмет повторной проверки внешними исследователями.

| Дата | Прочитанный источник | Что переносим в новый заказ |
|---|---|---|
| 11.10.2026 | `01-Conversations/ChatGPT/conversations/2026-10-11-glubokoe-issledovanie-llm-6acaed4e.md` | Исходные вопросы Антона: что происходит на внутренних слоях, как наблюдать вычисления, связь данных и поведения, alignment и веса. Есть исходный разговор и развёрнутый ответ ChatGPT. |
| 18.10.2026 | `05-Resources/NotebookLM/_packets/teach-2026-10-18-how-llm-thinks.md` | Прежний обзор приборов: logit/tuned lens, probes, steering, SAE, circuit tracing, NLA, J-lens; практические входы TransformerLens, Gemma Scope, Neuronpedia. Исторические данные и токсичность сохраняются как контекст, не замещают тему слоёв. |
| 18.10.2026 | `05-Resources/NotebookLM/_packets/teach-2026-10-18-how-llm-thinks-corrections-v2.md` | Обязательные оговорки: веса/активации/CoT различаются; декодируемость не доказывает использование; SAE и графы неполны; вмешательства требуют контролей. В v1 были чрезмерные выводы о данных и токсичности. |
| 18–19.10.2026 | `05-Resources/NotebookLM/_teach-curriculum.md:490–508` | Зафиксированы RU/EN v2 и один существующий блокнот. Техническая приёмка аудио не равна полной научной проверке или независимому прослушиванию. Повторный заказ 19.10 обнаружил дубликат и переиспользовал материал. |
| 19.10.2026 | `teach-2026-10-19-llm-serving-memory.md`, реестр DR26-10-19-HUB-05-2154 | Соседняя тема: многопользовательский сервис, KV-cache, история и фильтры. В строке реестра указана одна нога Grok; это не кворум исследования внутренних слоёв. |

Проверены память узла, RAG, FTS сессий, пакеты NotebookLM, исходник ChatGPT и реестр. RAG ответил, но релевантные точные источники найдены локальным поиском. Поверхности RAG частично скрыты SECRET-GATE; у свежих jsonl неполное покрытие. FTS показал точные сессии 06466d68-93d3-44b2-989d-0ba3c25eeea8, 1892d36d-8758-49ac-b958-582fbfd538e5 и b01f570a-2635-4055-bfb3-838b78461d6e. Полнота всей истории всех устройств не доказана.

Важный пробел прежнего пакета: обзор приборов уже есть, но нужна доказательная карта вычислений по глубине, отдельно ранние, средние, поздние слои и шаги генерации. Нужен новый многовендорный заказ по этому пробелу с проверкой прежних выводов.

# Исследовательское задание для внешних LLM

Conduct a deep research investigation. Research cutoff: actual execution date; state that date. Do not ask clarifying questions. Produce the research in English, with a short Russian executive summary and glossary. Search primary papers, official research pages, model cards, repositories and documented replications. This is a scientific investigation, not a market comparison.

TOPIC: How transformer LLMs compute across layers, especially deep/internal and late layers; what can be observed, causally established and reproduced.

OBJECTIVE: Continue an existing October 11 investigation and October 18 teaching package. Explain the computation clearly for an engineer, distinguish demonstrated mechanisms from metaphors, and specify a small reproducible open-model experiment. Do not treat supplied prior claims as verified evidence.

PRIOR WORK TO RECHECK: An earlier overview covered logit/tuned lens, probes, steering, sparse autoencoders, activation patching/ablation, attribution graphs/circuit tracing, Natural Language Autoencoders and J-space/J-lens. It separated weights, transient activations and generated chain-of-thought. A correction required causal interventions, random-direction and unrelated-position controls, and held-out paraphrases. It warned that intermediate decoding is not a literal inner monologue, feature activation is not intention, reconstruction is incomplete, and ordinary closed chat APIs do not provide arbitrary internal tensors. Preserve these distinctions, update or overturn them with sources.

Previous ancillary questions concerned toxic training data, alignment and historical corpora. Retain them only where they explain layerwise mechanisms. Do not let corpus history or multi-user serving displace the main topic. The prior package's claim that all post-2000 text exceeds all earlier text was explicitly marked not established; do not repeat it as fact. Toxic-data findings were conditional experiments, not a universal safety prescription.

FOUR REQUIRED RESEARCH DIRECTIONS. Each vendor investigates ALL FOUR independently so agreement can be compared:

1. LAYERWISE COMPUTATION. Trace one token through embeddings, residual stream, attention, MLP and unembedding/logits. Compare empirical roles of early, middle and late blocks for lexical, syntactic, semantic, factual, relational and answer-preparation information. Distinguish absolute layer index from normalized depth. Explain what changes across token positions and repeated generation steps. Avoid a universal staircase unless demonstrated across architectures, models and tasks. Distinguish architectural depth, hidden representations, reasoning-token time, recurrence and test-time compute. Ask whether late layers refine, suppress, route, calibrate or compose rather than simply becoming more abstract.

2. OBSERVABILITY AND CAUSALITY. Compare logit/tuned lens, probes, attention maps, SAE/transcoders, circuit tracing, NLA and J-lens. For each: required access, measured object, layer/token granularity, assumptions, trained decoder, reconstruction/coverage errors, artifacts, controls, reproducible code and known failures. Separate decodability, correlation, necessity, sufficiency and causal mediation. Compare patching, ablation and steering; address off-manifold interventions, redundant mechanisms and probe leakage. Explain when causal claims survive held-out tasks and model variants.

3. DEEP REPRESENTATIONS AND BEHAVIOR. Investigate multi-hop inference, planning, numerical reasoning, latent representations, multilingual concepts, uncertainty, truthfulness, refusal and alignment. What evidence places these mechanisms in particular layers or distributed circuits? Can base/instruct/posttraining comparisons locate changes? How does CoT relate to internal computation, and when is it unfaithful? Recheck claims about hidden intentions, situational awareness, introspection, evaluation awareness and toxic persona features with explicit alternative explanations. Neither assume consciousness nor dismiss it by rhetoric; state what the experiments can and cannot establish.

4. REPRODUCIBILITY AND TRANSFER. Compare findings across at least two open model families and base/instruct variants where evidence exists; label unavailable comparisons. Describe access limits for closed models without inferring internals from prose. Propose an experiment on a small supported open model: two prompt conditions, per-layer measurements, logit difference and behavioral metrics, intervention positions, matched-norm random controls, held-out paraphrases, seed handling and uncertainty. Report prerequisites, supported versions and estimated RAM/VRAM separately from measured use. Give a CPU-friendly fallback and a browser demonstration if feasible. No installation, training, paid API or execution is requested in this order.

FOUR REQUIRED STAGED OUTPUTS (stages are distinct from vendor rails):

Release 1: prior-work audit and evidence map. List what the prior package established, overclaimed, left open or needs updating. Create a source-to-question matrix and explicit search-coverage limits.

Release 2: layer-by-layer explanatory atlas, with a worked token example, distinctions between layer depth and reasoning steps, diagrams and a model/task comparison table. Label proposed examples as illustrative.

Release 3: adversarial review of deep-layer claims and measurement tools. Preserve contradictory findings, methodological confounds, null results and strongest counterarguments. Give an evidence-strength table.

Release 4: reproducibility protocol and decision memo. Provide one smallest useful experiment, acceptance/falsification criteria, constraints, open questions and next research steps. This is a proposal, never claim that our experiment was run.

OUTPUT FORMAT: A. Executive summary; B. Four staged outputs; C. Key findings with evidence strength; D. Contradictions and debates; E. Best practices and failure modes; F. Reproducibility protocol; G. Action plan; H. Sources. Include a glossary and a compact matrix: claim | model/version | depth/layer | token/task | method | intervention/control | observed effect | uncertainty | source/date | replication status.

Candidate primary leads to verify, not endorsements: transformer-circuits.pub; anthropic.com research on circuit tracing, natural-language-autoencoders and global-workspace; OpenAI persona-feature research; Google DeepMind Gemma Scope; TransformerLens and circuit-tracer official repositories; original tuned-lens and activation-patching papers. Verify existence, exact publication date, method and claimed result, including any 2026 material. Prefer the original experiment over retellings.

MINIMUM ACCEPTANCE: four substantive directions, four staged outputs, linked primary evidence for every major claim, explicit limitations, and a proposed falsifiable experiment. A plan, an echoed prompt or a list of citations without findings is not a research report.

# Исполнение и приёмка

Заказать один общий веер на шесть браузерных рельс: ChatGPT Deep Research, Claude.ai Research, Gemini Deep Research, Grok, GLM, Mistral. Минимум четыре независимых вендора с полными отчётами; два аккаунта одного вендора считаются одной рельсой. Codex собирает recall и заказ, но не является исследовательской ногой.

К общему телу добавляется универсальный §1 и отдельный платформенный блок из `08-Templates/dr-platform-playbook.md`. UI-режимы выбираются по живому интерфейсу и текущей поправке /dr-fanout от 26.10. Модель из текста промпта не является доказательством настройки режима.

Состояния различаются: prepared → queued → submitted → researching → report retrieved → gate passed → cross-vendor synthesis. Реестр running не доказывает фактического старта браузера. Для каждой ноги нужны URL беседы, наблюдаемый режим, дата, readback отправленного текста и наблюдаемый статус. Полный отчёт проходит `dr_leg_gate.py --strict --prompt <manifest> <report>` до зачёта.

Независимый ревьюер этого пакета: Claude, session d71b832e-8c45-4976-b2aa-f164de4f6c34. Адресный PROPOSE отправлен как рабочий аккаунт:1992658; в ограниченном окне ответа по точному corr_id не было. До содержательного ответа статус NO_QUORUM / SELF_DECISION, не консенсус. Лизы и независимое ревью сохраняются.

рельса: Codex subscription для recall/подготовки; исследования только в браузерных подписках внешних вендоров. Новое аудио не заказывается этим пакетом.
