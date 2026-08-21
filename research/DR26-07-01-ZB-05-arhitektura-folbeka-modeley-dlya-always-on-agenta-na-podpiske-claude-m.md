---
dr_id: DR26-07-01-ZB-05
title: "Архитектура фолбэка моделей для always-on агента на подписке (Claude Max/Weekly) без случа"
date: 2026-07-01
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-01-ZB-05): Архитектура фолбэка моделей для always-on агента на подписке (Claude Max/Weekly) без случайных трат на платный API

> ⚠️ **UPDATE 2026-07-14:** эта заметка была сгенерена по ПРОМПТУ (тела отчёта тогда не было — ChatGPT не отдаёт DR через экспорт). Полный отчёт СОБРАН 14.07 (Export→Markdown), лежит в `_originals\deep-research\` (путь в «Источник»). Актуальный синтез: [[decision-agent-stack-dr-bundle-synthesis-2026-07-14]].


> Запрос глубокого ресёрча (ChatGPT Deep Research) на тему: как построить лестницу фолбэка моделей и wrapper вокруг claude -p, чтобы агент деградировал грациозно при упоре в лимит подписки, не переходя на платный API — отчёт содержит только постановку задачи/контекст, самого синтеза-ответа в тексте нет.

## Ключевые выводы
- claude -p / Claude Code не умеет сам переключаться на другую модель/провайдера при упоре в лимит — нужен внешний wrapper-ladder
- Инцидент-триггер: inbox-робот на --model sonnet упёрся в weekly limit и тихо упал в лог, из-за чего пир увидел 'hub offline' — транспорт (пинги/scheduler) 0-LLM и переживает любой облачный сбой, но точка отказа — шаг 'thinking'
- Уже существует отложенный внутренний DR по локальным моделям (Ollama+Qwen), заблокированный до появления GPU на 16GB
- Сообщество предлагает держать дешёвую вторую LLM как фолбэк: Minimax (~$10/мес) или Kimi 2.6 через OpenRouter, чтобы агент самовосстанавливался при падении основной модели
- Практики запрещают сырые API-ключи во избежание неожиданных платных трат, оставляя только подписку на coding-plan + дешёвый фолбэк
- Соло-пользователь реально тратит ≤~30% дневного weekly-лимита Claude Max 20x — рой агентов нужен, чтобы полноценно использовать подписку
- Хаб Антона имеет 2 GPU (может хостить локальные модели), ноутбук — тонкий клиент

## Рекомендации / решения
_нет_

## Сущности
- **Люди:** Anton
- **Компании:** Anthropic, OpenAI, OpenRouter, Minimax, Moonshot AI (Kimi), DeepSeek, Alibaba (Qwen), Ollama, LiteLLM, LangChain, vLLM
- **Продукты/инструменты:** Claude Code, claude -p, Claude Max/Weekly, Ollama, Qwen, Minimax, Kimi 2.6, OpenRouter, LiteLLM proxy, LangChain fallbacks, vLLM

## Открытые вопросы
- Не приведён итоговый ответ ресёрча: конкретная лестница фолбэка (rung-by-rung с cost/quality/trigger) отсутствует в тексте отчёта
- Не решено, оправдан ли ~$10/мес OpenRouter-фолбэк против local-only или subscription-only, и при каких условиях
- Дизайн wrapper'а (детект лимита/ошибки → переключение провайдера → fail loud вместо тихого лога в файл) не описан в предоставленном тексте
- Какая именно локальная модель подойдёт под 2-GPU хаб и какой месячный cost envelope — не отвечено
- Какие guard-механизмы предотвращают случайный платный API-спенд — не раскрыто

## Источник
- DR-ID `DR26-07-01-ZB-05` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-01-ZB-05-llm-fallback-architecture-research-6a4ad-chatgpt.md`

## Связано
- [[model-fallback-ladder]]
- [[claude-code-wrapper]]
- [[spend-guard]]
- [[local-llm-hub-gpu]]
- [[agent-stack]]
- [[silent-failure-to-log]]
- [[subscription-limits-management]]
- [[insight-DR-DR26-06-26-HUB-04-отказоустойчивость-llm-инфраструктуры-failover-меж]] — прямой сиквел той же темы: архитектура фолбэка моделей для always-on агента продолжает failover-исследование
- [[insight-DR-DR26-06-26-HUB-06-выбор-локальной-llm-3-6-gb-vram-на-gtx-1660-super-]] — прямое продолжение: выбор конкретной локальной модели на GPU хаба питает архитектуру фолбэка always-on агента
- [[insight-DR-DR26-07-28-HUB-06-2338-сетап-друга-claude-code-через-vm-genspark-anthropi]] — обе про архитектуру фолбэка/резерва при упоре в лимит подписки
- [[insight-DR-DR26-07-28-HUB-20-2339-fallback-architecture-for-a-subscription-only-alwa]] — прямой предшественник по теме, только без синтеза
