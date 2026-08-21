---
dr_id: DR26-06-28-HUB-03
title: "Разбор комментария про Obsidian+Claudian+MemPalace+Hermes как «трёхслойную память»"
date: 2026-06-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-28-HUB-03): Разбор комментария про Obsidian+Claudian+MemPalace+Hermes как «трёхслойную память»

> Проверка комментария о power-user стеке (Obsidian/Claudian/Codex-Claude/MemPalace/Hermes), выдающего себя за безграничную единую память ChatGPT/Claude, на соответствие реальным возможностям и лимитам продуктов.

## Ключевые выводы
- Архитектура реальна как инженерная сборка, но комментарий смешивает 4 разные вещи: системный промпт/роутинг, внешнюю долговременную память, файловую базу знаний Obsidian и встроенную память ChatGPT — это не «один мозг», а гибрид подсистем
- Claudian — реальный community-плагин Obsidian, встраивающий Claude Code/Codex CLI (чтение/запись файлов, bash, multi-step workflows), но официально desktop only, не mobile-first
- Codex доступен по ChatGPT-подписке, Claude Code — по Claude subscription/Anthropic Console; Claudian просто использует существующую аутентификацию этих CLI, чуда-канала не создаёт
- Hermes Agent — реальный self-hosted агент с persistent memory и gateway (Telegram/Discord/Slack/WhatsApp/Signal/CLI), поддерживает OpenAI Codex через ChatGPT OAuth и хранит память локально в ~/.hermes/
- «Трёхслойная память» терминологически неточна: правильнее называть Core routing + external memory (MemPalace) + knowledge base (Obsidian); сам MemPalace внутри уже четырёхслойный (L0 Identity, L1 Essential Story, L2 On-Demand Recall, L3 Deep Search, wake-up контекст L0+L1 ~600-900 токенов)
- Иерархическая память и retrieval — признанный и работающий паттерн (MemGPT, Mem0: 91% lower p95 latency и >90% экономии токенов против full-context), но даже с ним модели заметно ошибаются на длинных многосессионных диалогах (LoCoMo, LongMemEval)
- Встроенная память ChatGPT — не строгий project memory store: OpenAI прямо пишет, что она не хранит каждую деталь и детали могут меняться со временем; для надёжного project state нужен явный внешний слой (MemPalace) и source of truth (Obsidian)
- Заявления комментария о «безграничных лимитах» и «$20 = ресурсы $200» не подтверждаются: Plus и Pro официально разделены по memory/context/Codex usage (5x-20x разница), Codex-использование идёт в общий agentic usage limit, у Hermes через ChatGPT OAuth зафиксированы issue про connection errors/timeouts/429 при concurrency
- Главные риски стека: агент получает реальный доступ к файловой системе и bash (Claudian/Codex/Hermes) — при плохом sandboxing грозит operational mess; дрейф структуры индексных заметок при неактуализации; фрагментация конфиденциальности (локальная память Hermes vs облачная ChatGPT/Claudian с отправкой input/файлов провайдеру)

## Рекомендации / решения
- Разделять роли явно: маленький always-loaded Core = identity/routing (не memory engine), MemPalace = эпизодическая/семантическая retrieval-память, Obsidian = стабильный source of truth/документация — не смешивать в «единую память»
- Не полагаться на встроенную память ChatGPT для точного состояния проектов — использовать явный внешний retrieval-слой (MemPalace) плюс явный canonical store (Obsidian) для критичных фактов
- Не принимать заявления о «безлимитности» буквально — учитывать официальные различия Plus/Pro по memory/context/Codex usage и общий agentic usage limit при планировании нагрузки
- При построении контроля токенов держать компактный индекс проекта, из которого агент идёт только к нужным узлам, а не грузить весь vault — экономия токенов идёт от дисциплины поиска, а не от структуры самой по себе
- Учитывать разные политики хранения данных по слоям стека (Hermes локально в ~/.hermes/ vs облачные Claudian/ChatGPT, отправляющие input/файлы провайдеру) при работе с чувствительными проектами
- Поддерживать актуальность индексных заметок/карт проекта вручную — иначе агент начнёт ходить по устаревшим ссылкам и делать ложные выводы

## Сущности
- **Люди:** —
- **Компании:** OpenAI, Anthropic
- **Продукты/инструменты:** Obsidian, Claudian, Codex, Claude Code, MemPalace, Hermes Agent, ChatGPT, ChatGPT Plus, ChatGPT Pro, MemGPT, Mem0, LoCoMo, LongMemEval

## Открытые вопросы
- Насколько устойчив Hermes при работе через ChatGPT/Codex OAuth в продакшене (issue reports про connection errors, timeouts, 429 при concurrency/subagents остаются неразрешёнными)
- Является ли тезис «Codex не хуже Claude, разницы никакой» универсальным фактом или частной пользовательской оценкой — нейтральных сравнительных данных нет
- Насколько реально дублирование логики между «Core» комментатора и встроенными L0/L1 слоями самого MemPalace, и не избыточна ли такая надстройка
- Как на практике контролировать дрейф структуры (папки/индексы/маршруты) в длительной эксплуатации без постоянного ручного рефакторинга

## Источник
- DR-ID `DR26-06-28-HUB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- obsidian-vault-architecture
- long-term-memory-llm
- context-engineering
- agent-memory-hierarchy
- second-brain-northstar
- vault-data-architecture
- self-hosted-agent-gateway
- chatgpt-plan-limits
