---
dr_id: DR26-09-22-HUB-04-1433
title: "Ранжированный корпус материалов для подготовки к интервью RS/MTS (Research Scientist/MTS)"
date: 2026-09-22
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-09-22-HUB-04-1433): Ранжированный корпус материалов для подготовки к интервью RS/MTS (Research Scientist/MTS)

> GLM-отчёт ранжирует учебные материалы по LLM и ML-математике под профиль Антона (COO/CTO, Python/C++, PhD, без классического ML-research трека) и даёт 6-недельный план подготовки к RS/MTS интервью с нарезкой на упражнения.

## Ключевые выводы
- Профиль кандидата (продакшн-инженер с LLM-агентами, не ML-исследователь) сильнее в системном инжиниринге и продакшн-циклах, слабее в классических ML-исследованиях и тренировке моделей с нуля — план фокусируется на LLM-специфике, а не на общей математике/ML.
- Обязательный минимум материалов: Stanford CS336 (120-150ч, критически высокий приоритет), Karpathy 'Zero to Hero' (80-100ч), Alisa Liu's Math Notes (40-60ч), Sebastian Raschka 'Build a LLM from Scratch' (60-80ч), Chip Huyen 'AI Engineering' (50-70ч, критически высокий).
- Можно пропустить: Deep Learning Book (Goodfellow, устарел, 2016, нет LLM-материала), 'Mathematics for ML' (Deisenroth, слишком базовая), Blitzstein Probability (общая, не ML-контекст), Jay Alammar (уровень новичка), общие ML-курсы.
- 6-недельный план: нед.1 архитектура трансформера и токенизация, нед.2 оптимизаторы/стабильность обучения/параллелизм, нед.3 инференс (KV-cache, спекулятивное декодирование, long context), нед.4 alignment/RLHF/DPO/GRPO, нед.5 scaling laws и MoE, нед.6 mock-интервью и behavioral.
- 12 практических упражнений по нарастающей сложности: от tokenizer(minbpe)/attention/LayerNorm (уровень 1) через GPT-2 block/RoPE/AdamW (уровень 2) и KV-cache/mixed precision/DDP (уровень 3) до speculative decoding/DPO trainer/MoE layer (уровень 4).
- Тренировка без ИИ-ассистента: активное вспоминание (flashcards, blank page), голосовые самообъяснения по технике Фейнмана, whiteboard-практика с таймером, peer mock-интервью, писать код в простом редакторе без автодополнения.
- Отчёт помечен куратором как 'НЕ КАНОН': ответ GLM не проверен по ссылкам, в учебный план и CRM не кормить; собрано только 4 из 6 заказанных LLM-ног (targets: chatgpt, gemini, grok, claudeai, glm, mistral), кворум DR не выполнен по стандарту.

## Рекомендации / решения
- Строить фундамент по формуле отчёта: Karpathy (фундамент) + CS336 (практика/backbone) + Alisa Liu (математика) + Chip Huyen (системы для инференса/MLOps).
- Пропустить устаревшую/общую математику и ML-курсы (Goodfellow, Deisenroth, Blitzstein, general ML) в пользу LLM-специфичных материалов.
- Перед тем как резать план на подкасты и практические сессии — свериться с живым скелетом interview-prep-curriculum-alisa-liu и проверить недостающие ноги DR, поскольку сам отчёт не верифицирован по первичным источникам.
- На неделе 6 использовать Pramp/interviewing.io и LeetCode Hard как финальную проверку готовности перед реальным интервью.

## Сущности
- **Люди:** Andrej Karpathy, Alisa Liu, Sebastian Raschka, Chip Huyen, Jay Alammar, Lilian Weng, Anton Dziatkovskii (Tony Dzi)
- **Компании:** Stanford, Palo Alto AI Research Lab, Hugging Face, EleutherAI
- **Продукты/инструменты:** Stanford CS336, Zero to Hero (Karpathy), nanoGPT, minbpe, Build a LLM from Scratch (Raschka), AI Engineering / ML Interviews Book (Chip Huyen), Deep Learning Book (Goodfellow), Mathematics for Machine Learning (Deisenroth), d2l.ai, Blitzstein Probability, vLLM, TGI, PagedAttention, DPO, PPO, GRPO, FSDP, ZeRO/DeepSpeed, RoPE, YaRN, Pramp, interviewing.io, LeetCode

## Открытые вопросы
- Отчёт не проверен по первичным ссылкам (явно помечен 'НЕ КАНОН') — точные URL и цитаты не подтверждены, риск устаревших или несуществующих ссылок.
- На диске собрано только 4 из 6 заказанных LLM-ног; неясно, какие именно 2 отсутствуют и расходятся ли их рекомендации с версией GLM.
- Тезис 'что уже сильнее среднего кандидата' раскрыт поверхностно — только общая фраза про продакшн-опыт, без конкретики.
- Стоимость и детали мок-интервью площадок (Pramp, interviewing.io) не приведены, хотя запрос это требовал.

## Источник
- DR-ID `DR26-09-22-HUB-04-1433` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- interview-prep-curriculum-alisa-liu
- job-search-diaries-catalog
- alpha-protocol-recall-plus-dr
- teach-anton-by-podcast-default
- notebooklm-integration
