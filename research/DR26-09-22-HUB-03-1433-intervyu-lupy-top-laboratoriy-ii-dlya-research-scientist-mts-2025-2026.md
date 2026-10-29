---
dr_id: DR26-09-22-HUB-03-1433
title: "Интервью-лупы топ-лабораторий ИИ для Research Scientist/MTS (2025-2026)"
date: 2026-09-22
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-09-22-HUB-03-1433): Интервью-лупы топ-лабораторий ИИ для Research Scientist/MTS (2025-2026)

> Как устроены интервью-лупы (этапы, форматы, сроки, критерии) на Research Scientist/MTS/Research Engineer в OpenAI, Anthropic, DeepMind, xAI, Meta и др., и в каком порядке к ним готовиться.

## Ключевые выводы
- ML-кодинг «с нуля» (multi-head attention + KV-cache, top-k/top-p/beam search сэмплирование, BPE-токенизатор, полный тренировочный цикл, стабильные численные функции) — ядро лупов OpenAI, Anthropic, xAI, DeepMind, Perplexity, Mistral; это самый частый и важный этап для RS/MTS
- OpenAI, Anthropic и (в основном) Google DeepMind НЕ используют классический LeetCode-центричный луп на onsite — вместо этого практический ML-кодинг и research discussion; LeetCode-центричны Meta, Amazon, Apple, Nvidia, Microsoft
- Job talk (презентация исследования, 45-60 мин) — обязателен/типичен в Google DeepMind, Meta FAIR, Nvidia, Microsoft Research, обычно только для кандидатов с PhD; в OpenAI/Anthropic чаще неформальный deep-dive по своим работам
- Скорость и длина лупа сильно различаются: xAI/Perplexity/Reflection — быстрые процессы (~1-5 недель), Google DeepMind и Apple/Microsoft — медленные (5-8, местами до 16 недель) из-за committee review / team match
- Без PhD и публикаций реалистично открыты роли Research Engineer / MTS (инженерный трек) и Applied Scientist; признаваемые альтернативные сигналы — коммиты в известные OSS-репо (vLLM, transformers, PyTorch, llama.cpp, TRL, axolotl), собственные продовые ML-системы; Research Scientist без PhD — редкое исключение (обычно через residency-программы)
- Изменения 2025→2026: OpenAI и Anthropic запрещают AI-ассистентов в live-кодинге и оценивают способность писать код без помощи; DeepMind/xAI/Perplexity, наоборот, разрешают или поощряют AI-assisted раунды; появились отдельные раунды по работе с AI-агентами
- Статус оплачиваемого 'work trial' (OpenAI исторически, также Meta Superintelligence Labs, Scale AI, SSI для финальных кандидатов) на 2025-2026 расходится между источниками — где-то описан как обязательный, где-то как отменённый/опциональный
- Частые причины провала: неспособность объяснить основы (Batch Normalization, vanishing gradient), неэффективный/нечитаемый код, слабая коммуникация о своей работе, незнание публикаций лаборатории, отсутствие вопросов в конце интервью, в Anthropic отдельно — отсутствие искреннего интереса к safety-миссии
- Публичных описаний лупов для Meta Superintelligence Labs, SSI, Thinking Machines Lab и Reflection AI ни один из вендоров не нашёл — предполагается точечный найм через прямые рекомендации
- Приоритет подготовки (общий для большинства целевых компаний): ML-кодинг-ядро (2-4 недели) → 2-3 ключевые собственные работы для discussion (5-мин и 45-мин версии) → ML-breadth rapid-fire → алгоритмический кодинг как страховка (если Meta/Amazon/Nvidia/Apple/Microsoft) → математика/деривации → ML system design → инфраструктура/распараллеливание (если xAI/Meta/OpenAI-infra) → поведенческое параллельно

## Рекомендации / решения
- Начинать подготовку с ML-кодинга (attention/KV-cache, сэмплирование, тренировочный цикл) — 3-4 недели, это критический этап почти для всех целевых лабораторий
- Заранее подготовить 2-3 ключевые исследовательские/инженерные работы к глубокому обсуждению — с цифрами, аблляциями и честными ограничениями, в форматах на 5 и на 45 минут
- Если в списке целей есть Meta, Amazon, Nvidia, Apple или Microsoft — добавить классический алгоритмический кодинг (30-50 задач уровня LeetCode Medium)
- Позиционироваться на роли Research Engineer/MTS/Applied Scientist (а не Research Scientist) как более доступную дверь без PhD, опираясь на OSS-вклады в известные ML-репозитории и собственные продовые системы как замену публикациям
- Не полагаться на цифры и цитаты одного отчёта без проверки: второй прогон того же вендора (GLM) честно признал отсутствие браузинга и пометил большинство конкретики как анекдотичную/неподтверждённую — стоит перепроверить ключевые факты напрямую (Glassdoor, Blind, карьерные страницы, рекрутёр) перед тем как строить план на конкретных числах длительности/этапов
- Практиковать live-кодинг без AI-ассистентов отдельно от AI-assisted раундов — политика различается по компаниям (OpenAI/Anthropic запрещают, DeepMind/xAI/Perplexity поощряют) и это нужно уточнять по конкретной команде

## Сущности
- **Люди:** Alisa Wuffles (Liu), Yongzx, Yuan Meng
- **Компании:** OpenAI, Anthropic, Google DeepMind, Meta (Superintelligence Labs / FAIR), xAI, Mistral AI, Cohere, Scale AI, Perplexity, Reflection AI, Thinking Machines Lab, SSI (Safe Superintelligence), Nvidia, Apple, Amazon, Microsoft
- **Продукты/инструменты:** vLLM, SGLang, Hugging Face transformers, PyTorch, llama.cpp, lm-eval-harness, TRL, axolotl, Stanford CS336, LeetCode 75 / Neetcode Blind 75, The Illustrated GPT-2

## Открытые вопросы
- Реальный текущий статус оплачиваемого work trial в OpenAI и других лабораториях на 2025-2026 — источники расходятся
- Структура интервью-лупов Meta Superintelligence Labs, SSI, Thinking Machines Lab и Reflection AI — публичных описаний не нашёл ни один вендор, вероятен точечный найм по рекомендациям
- Точная по-компанийная политика допуска AI-ассистентов на кодинг-раундах в 2026 году — один из двух прогонов честно не смог это подтвердить без веб-доступа
- Достоверность цитат и списка «реально открытых страниц» в первом GLM-отчёте (маркеры цитирования нетипичны для GLM и похожи на артефакт другого инструмента) — не верифицировано
- Используется ли тайтл MTS в Anthropic официально — не найдено надёжного подтверждения

## Источник
- DR-ID `DR26-09-22-HUB-03-1433` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- interview-prep
- alpha-protocol-recall-plus-dr
- teach-anton-by-podcast-default
- job-search-ai-labs
- never-call-anton-a-founder
