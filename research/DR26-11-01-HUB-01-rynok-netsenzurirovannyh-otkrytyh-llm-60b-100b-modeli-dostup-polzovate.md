---
dr_id: DR26-11-01-HUB-01
title: "Рынок нецензурированных открытых LLM 60B–100B+: модели, доступ, пользователи, регуляторика"
date: 2026-11-01
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-01-HUB-01): Рынок нецензурированных открытых LLM 60B–100B+: модели, доступ, пользователи, регуляторика

> Какие «нецензурированные» открытые LLM размером 60B–100B+ существуют на 30.10.2026, как к ним получить доступ без своей GPU, кто ими пользуется и как это регулируется.

## Ключевые выводы
- Технического дефицита больших permissive open-weight моделей нет: в диапазоне 70B–235B+ доступны аблитерированные Llama-3.3-70B, Qwen2.5-72B, Qwen3-235B-A22B, GLM-4.5-Air, DeepSeek-V3 и gpt-oss-120b, а исторически Dolphin и Hermes охватывают 70B–141B.
- Снятие отказов стало почти механическим: работа Arditi et al. показала, что отказ во многом опосредован одним направлением в представлениях (13 моделей до 72B), а rank-one правка весов делает эффект постоянным. Работа EMNLP 2026 уточнила, что отказ неоднороден и не сводится к одной оси. Safety-alignment в весах не является надёжной границей после публикации checkpoint.
- Потери качества от abliteration независимо не измерены: есть авторские метрики (Qwen2.5-72B Heretic: 3 отказа из 100, KL 0.0363), но KL не равен MMLU, кодингу или long-context. Общепринятого мета-бенчмарка нет.
- Serverless API почти всегда сохраняют внешний policy-слой (ToS, AUP, лицензия базы, право отключить аккаунт). Featherless (Llama-3.3-70B abliterated, $2.88/$2.88 за 1M, API-промпты не логируются) и Infermatic (от $9/мес, 70B/72B RP-модели, no-logs) явно продают permissive 70B+. У OpenRouter ToS запрещает unapproved red-teaming и jailbreaking. Together, Fireworks, DeepInfra, Groq, Novita и Hyperbolic в проверке не подтвердили shared uncensored 70B+ SKU.
- Самый предсказуемый путь к поведению без отказов на уровне модели — аренда GPU или managed endpoint со своими весами: RunPod A100 80GB $1.79/ч, H100 PCIe $2.89/ч; HF Inference Endpoints A100 ≈$2.50/ч, H200 ≈$5/ч; Chutes private TEE от ≈$1.80/ч. Квантованная 70B помещается в одну 80GB GPU, gpt-oss-120b рассчитана на один 80GB, а Qwen3-235B Q4 весит ≈142 GB и требует несколько GPU.
- Основной подтверждённый спрос: по OpenRouter State of AI 2025 около 52% токенов open-source моделей приходится на roleplay (ноябрь 2024 – ноябрь 2025), вторая категория — программирование. Дополнительные группы: писатели и RP-разработчики, red-teamers и исследователи, privacy/self-hosting аудитория. Репрезентативной демографии и оценки рынка в деньгах нет.
- Криминальное использование документировано, но преувеличено маркетингом: по данным Google, Recorded Future, Trend Micro, Microsoft FraudGPT и WormGPT-подобные продукты — в основном jailbroken или перепакованные модели, а AI даёт ускорение и масштаб, а не новые возможности. Надёжных данных о рынке текстовых uncensored LLM 60B+ для CSAM не найдено.
- Риск: open weights необратимы (нельзя отозвать checkpoint), разрыв открытых и закрытых моделей в cyber сократился до ≈4–7 месяцев (UK AISI). Но реальный дополнительный uplift от снятия отказов зависит от домена и остаётся спорным; в fraud-сценариях измеренный операционный uplift был ограниченным.
- Регуляторика: EU AI Act не запрещает open-weight или uncensored модели как класс (ст. 53 и 55, исключение для open-source не освобождает GPAI с системным риском, порог ≈10^25 FLOP, обязанности с 2.08.2025). Executive Order США от 2.06.2026 не вводит лицензирование релиза. Правила CAC Китая (13.07.2023) касаются публичных сервисов внутри Китая. Hugging Face не запрещает abliteration, а модерирует по категориям контента (NFAA-метки, дизейбл, бан). Судебного прецедента о незаконности публикации uncensored весов не найдено.

## Рекомендации / решения
- Для воспроизводимого исследования с минимумом model-level отказов брать RunPod со своим Llama-3.3-70B, Qwen-72B или gpt-oss-120b derivative и самому контролировать веса, system prompt и логи.
- Из готовых сервисов первым пробовать Featherless с Llama-3.3-70B-Instruct-abliterated; Infermatic брать для длинных интерактивных RP/qualitative сессий, но не как единственный источник для публикуемых бенчмарков.
- Как управляемый middle ground использовать HF Inference Endpoint с приватным permissive checkpoint, учитывая риск Content Policy; Chutes private/TEE — если важна конфиденциальность.
- OpenRouter держать как запасной convenience-слой, а не основную среду для adversarial/red-team исследований из-за ToS.
- Если важнее ум, чем вседозволенность, начинать с актуальных open-weight MoE (GLM-5.x, Kimi, DeepSeek, Qwen3-класс) через Together, Chutes или свой деплой, а permissive derivative подключать лишь если отказы реально мешают законному корпусу.
- Для воспроизводимости фиксировать точный хеш модели, квантизацию, версию inference-движка, system prompt и параметры сэмплинга.
- Проверять цепочку лицензий derivative (Llama Community License/AUP наследуется независимо от меток репозитория) и ToS провайдера до использования.

## Сущности
- **Люди:** Arditi et al., mlabonne, huihui-ai, joshebbs, mlgawd, ThalisAI, cebeuq
- **Компании:** Meta, Alibaba (Qwen), Z.ai / Zhipu, DeepSeek, OpenAI, Mistral AI, Cohere, Moonshot AI, Nous Research, Cognitive Computations, Hugging Face, Featherless, OpenRouter, Infermatic, Venice.ai, Together AI, Fireworks AI, DeepInfra, Groq, Novita AI, Hyperbolic, NanoGPT, RunPod, Lambda, Modal, Vast.ai, Chutes, Akash, UK AISI, NIST CAISI, Google Threat Intelligence Group, Recorded Future, Trend Micro, Microsoft, Anthropic, Mandiant
- **Продукты/инструменты:** Llama-3.3-70B-Instruct-abliterated, Qwen2.5-72B-Instruct-abliterated, Qwen2.5-72B-Instruct-heretic, Qwen3-235B-A22B abliterated, GLM-4.5-Air-abliterated, DeepSeek-V3-abliterated, gpt-oss-120b-abliterated, Hermes-4-70B, Hermes-3-Llama-3.1-70B-lorablated, Dolphin-2.9.2-Qwen2-72B, Dolphin Mixtral 8x22B, Command R+, Kimi K2, FailSpy abliterator, FraudGPT, WormGPT, GGUF, vLLM, SGLang

## Открытые вопросы
- Нет надёжной оценки выручки или MAU рынка uncensored LLM и репрезентативной демографии пользователей.
- Нет стандартизованной независимой оценки потери общих способностей после abliteration моделей 100B+.
- Неизвестна доля WormGPT/FraudGPT-подобных продуктов в реальном криминальном использовании AI.
- Не восстановлен точный каталог NanoGPT на 30.10.2026 (страницы и ToS обновились 31.10–01.11).
- Не подтверждены крупные uncensored derivative для Command R+ и каноническая 60B+ версия для Kimi K2.
- Величина реального дополнительного вреда (uplift) от снятия отказов остаётся спорной и зависит от домена.
- Не найден судебный прецедент о правомерности публикации uncensored весов 60B+.
- Неясно, насколько единым механизмом можно считать направление отказа после работы EMNLP 2026.

## Источник
- DR-ID `DR26-11-01-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- abliteration и refusal direction
- open-weight safety и необратимость релиза
- GPU rental vs serverless inference
- EU AI Act GPAI (ст. 53, 55)
- roleplay как основной спрос на open-source LLM
- jailbreak-as-a-service и WormGPT/FraudGPT
- self-hosting и приватность инференса
- цепочка лицензий derivative-моделей

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-02 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
