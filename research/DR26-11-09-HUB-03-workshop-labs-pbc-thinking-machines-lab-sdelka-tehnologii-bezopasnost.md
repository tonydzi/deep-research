---
dr_id: DR26-11-09-HUB-03
title: "Workshop Labs PBC × Thinking Machines Lab: сделка, технологии, безопасность, benchmark"
date: 2026-11-09
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-09-HUB-03): Workshop Labs PBC × Thinking Machines Lab: сделка, технологии, безопасность, benchmark

> Что публично подтверждено о присоединении Workshop Labs к Thinking Machines Lab (найм основателей и покупка технологии), каковы технологии Workshop (Trellis, Silo) и их threat model, и как проверяемо сравнить LoRA и retrieval для персональных агентов.

## Ключевые выводы
- Первичный источник Workshop (13.04.2026): Rudolf Laine и Luke Drago написали «Workshop Labs is joining Thinking Machines»; мотив стратегический: совпадение миссий, ранний доступ к Tinker, разговоры с Mira Murati и John Schulman.
- The Information со ссылкой на представителя TML: Thinking Machines наняла двух сооснователей (Laine, Drago) и купила технологию стартапа; цена не раскрыта. Публичного первичного документа TML о «acquired Workshop Labs PBC» не найдено, структура сделки (акции, merger, asset purchase, acqui-hire с IP assignment), consideration, earn-outs, судьба инвесторских прав и полный IP-scope неизвестны.
- Пайплайн Workshop (реконструкция из вакансии и поста от 16.03.2026): OAuth-коннекторы (Gmail, Drive, Calendar, Notion) → data sync → app/key layer → post-training Trellis в multi-GPU TEE → зашифрованный per-user LoRA → inference на custom vLLM с multi-tenant LoRA. Схема отбора/фильтрации/consent данных публично не описана.
- Trellis-KimiK2T (12.03.2026): ~6,600 tokens/s на одном 8×H200, заявлено «~50× быстрее» лучшей open-source альтернативы. Но код только планировался к open-source после safety evals, сравнение не apples-to-apples (HF comparator не обучал experts, constant-length режим), цены Tinker/Fireworks взяты из публичного прайса. На GitHub Workshop 7 репозиториев (tinfoil-*, monkeypatch demo под MIT), репозитория Trellis или Silo нет.
- Silo (совместно с Tinfoil): hardware-isolated CPU/GPU TEE, remote attestation, нет SSH/shell, user-specific keys, fail closed либо transparency log, Sigstore. Оговорки самих авторов: side channels, firmware, физический доступ, NVLink на Hopper не шифруется, нет multi-node TEE (только один 8-GPU node), зависимость от GitHub CI, нет fully reproducible builds, формальный аудит на 16.03 не проведён. Overhead «<10%» получен экстраполяцией пяти коротких прогонов до двухчасового (плюс 50+ полных прогонов в закрытой beta).
- Интеграция в Tinker: концептуальная конвергенция подтверждена (TML 15.10.2026: экспертиза пользователя должна быть «in the training signal», LoRA, full fine-tuning beta, checkpoint lineage), но code-level интеграция Trellis/Silo/коннекторов в Tinker публично не доказана. Лицензии неоднородны: Tinker SDK и tinker-cookbook Apache-2.0, monkeypatch demo MIT, Tinfoil confidential-model-router AGPL-3.0, Kimi-K2 Modified MIT (атрибуция при >100M MAU или >$20M/мес); лицензия Trellis/Silo как продуктов не установлена.
- Аудит tinker-cookbook #978 (17.10.2026, commit df2aff6): fail-open ветки верификаторов. MCQ-extractor берёт первое, а не итоговое «answer is…» и может спутать «I» с вариантом I; tau2-bench даёт score 1.0 при нереализованных nl_assertions/db_check и пустых expected_actions; LongBench substring containment принимает отрицание правильного ответа. PR #994 (06.11, ещё open) чинит перестановку ответа в GPQA, поэтому брать этот фикс = дублирование.
- persona-portability-benchmark (tonydzi): сильная часть — controlled model swap, frozen memory, byte-identical envelope, blind три судьи, contamination probe (`probe_context.py` ловит скрытую подгрузку AGENTS.md/CLAUDE.md). Rerun 29.10.2026 на 18 моделях: Claude-panel и Gemini-control ρ≈0.82, но GPT-control поставил три OpenAI-системы в top-3, а основной panel на 14–16 места; это предупреждение против единого LLM-judge скаляра. Бенчмарк не отвечает на вопрос retrieval vs LoRA (нет проверки удаления, актуализации, provenance, ACL).
- Предложенный дизайн эксперимента (методика отчёта, а не установленный результат): 4 arm'а (B без персонализации, R retrieval, L LoRA, C LoRA+retrieval), split по эпизодам решений, режимы static t0 и dynamic t1 (плюс L-retrain), пять классов held-out тестов (стабильные решения, updated, revoked, provenance, permission/ACL), canary secrets с последующим revoke, раздельные endpoints, детерминированные graders для безопасности, McNemar/paired bootstrap, preregistration.
- Рекомендация на 09.11.2026: combined design — LoRA для устойчивого judgment/style/skill, retrieval-store для фактов, источников, ACL и всего обновляемого/отзываемого. Контрдовод: TML в финансовой задаче получила 84.66% против 78.2% у лучшей frontier-модели, что поддерживает LoRA для tacit judgment, но не тестирует deletion, revocation и permissions.

## Рекомендации / решения
- Не утверждать публично «acquisition of Workshop Labs PBC» и «Silo/Trellis интегрированы в Tinker»; использовать формулировку «найм основателей + покупка технологии, стратегическая конвергенция».
- Для архитектуры персонального агента принять combined design: LoRA для стабильных предпочтений и стиля, retrieval с source_id, valid_from/valid_to, ACL для изменяемых фактов и provenance.
- Вклад в tinker-cookbook делать не дубликатами #971/#994, а классом: параметризованный adversarial regression corpus (self-correction, negation, malformed refs, missing optional deps, empty expected actions, parser ambiguity).
- Конкретные пути вклада: MCQ adversarial extraction suite, IFEval fail-closed (missing dependency/exception = failed constraint), tau2-bench semantics (реализовать nl_assertions/db_check через judge либо исключать из denominator с явным счётом), hardening containment/finality, отдельный personalization/security eval recipe (freshness, revocation, provenance, ACL).
- Провести factorial benchmark B/R/L/C на одном base checkpoint с canary secrets и revoke events; security-метрики мерить программными graders, taste — человеком-владельцем и минимум одним cross-family judge.
- Перенести `probe_context.py` и изолированный workroot в любой retrieval-vs-LoRA тест, чтобы скрытые context sources не загрязняли сравнение.
- Пересмотреть оценку Workshop при появлении: юридического документа о структуре сделки, TML release note или кода с Silo/Trellis внутри Tinker, полного Trellis source с независимым воспроизведением 50×, независимого аудита Silo, либо benchmark'а, где LoRA-only надёжно удаляет/обновляет без переобучения.

## Сущности
- **Люди:** Rudolf Laine, Luke Drago, Mira Murati, John Schulman
- **Компании:** Workshop Labs PBC, Thinking Machines Lab, Tinfoil, Moonshot AI, The Information, Hugging Face, Fireworks, NVIDIA, Sigstore
- **Продукты/инструменты:** Tinker, tinker-cookbook, Trellis / Trellis-KimiK2T, Silo, Kimi K2 Thinking, vLLM, LoRA, TEE (Trusted Execution Environment), tinfoil-garden / tinfoil-sync / tinfoil-memex / tinfoil-embeddings / tinfoil-inference / tinfoil-training, monkeypatch-train-kimi-k2-thinking-demo, confidential-model-router, persona-portability-benchmark, probe_context.py, tau2-bench, MMLU-Pro, GPQA, LongBench, IFEval, H200, Hopper, Blackwell

## Открытые вопросы
- Юридическая структура сделки: покупка акций, merger, asset purchase или acqui-hire с IP assignment; сумма, earn-outs, обязательства, судьба инвесторских прав и employee equity.
- Полный перечень переданных активов: репозитории, патенты, домены; передан ли личный IP основателей вне корпоративных активов.
- Интегрированы ли Trellis, Silo и OAuth-коннекторы Workshop в Tinker на code/product уровне.
- Будет ли Trellis опубликован под какой-либо лицензией и воспроизводится ли независимо заявленное ускорение 50×.
- Проведён ли и каков результат формального аудита Silo, включая attestation и build-chain verification после 16.03.2026.
- Как именно отбирались, фильтровались и дедуплицировались пользовательские данные из Gmail/Drive/Calendar/Notion при обучении, было ли согласие на уровне объекта.
- Способен ли LoRA-only после revoke/update обеспечить удаление, provenance и нулевую неавторизованную утечку, сопоставимые с retrieval, без дорогого переобучения (должен показать эксперимент).
- Исход PR #994 и закроют ли остальные пути из #978 существующие PR.

## Источник
- DR-ID `DR26-11-09-HUB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- LoRA vs retrieval для персонализации
- Confidential computing / TEE и remote attestation
- Fail-open verifiers и надёжность benchmark grading
- LLM-as-judge и judge-family bias
- Acqui-hire и структура сделок в AI
- Revocation, provenance и ACL в персональной памяти
- Contamination probes в benchmark'ах
- Вклад в OSS по классу дефектов, а не по симптомам

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-10 через gh: 3 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 3 · 🤷 0.
