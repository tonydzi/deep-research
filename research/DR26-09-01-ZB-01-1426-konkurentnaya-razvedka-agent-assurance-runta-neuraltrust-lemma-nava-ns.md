---
dr_id: DR26-09-01-ZB-01-1426
title: "Конкурентная разведка agent-assurance (Runta, NeuralTrust, Lemma, Nava) + NSF Phase I elig"
date: 2026-09-01
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-09-01-ZB-01-1426): Конкурентная разведка agent-assurance (Runta, NeuralTrust, Lemma, Nava) + NSF Phase I eligibility для O-1

> Отчёт картирует рынок agent-assurance/financial-agent-verification (4 прямых конкурента + широкая категория) и проверяет, может ли фаундер на визе O-1 претендовать на NSF SBIR Phase I, чтобы решить go/no-go по запуску Flight Recorder перед 45-дневным customer discovery.

## Ключевые выводы
- Идентифицированы 4 профильных конкурента с досье: Runta ($20M seed, инвестор a16z, agent execution/security), NeuralTrust ($20M seed, горизонтальный agent security), Lemma (pre-seed $2.3M, agent observability, заявляют 1M traces/day), Nava (seed $8.3M, escrow + verification до исполнения)
- Построена более широкая карта категории: agent identity (Okta, WorkOS), agentic payments (Skyfire, Catena), payment rails (Visa, Mastercard, x402, Stripe MPP), crypto-native policy engines (Turnkey, Fireblocks, Privy)
- Задокументированы регуляторные драйверы спроса на agent-assurance: GENIUS Act, FINRA 2026, OCC/Fed/FDIC guidance, EU AI Act
- Собраны инциденты 2025-2026 с денежными действиями AI-агентов как сигнал спроса: Grok/Bankr, AiXBT, Replit
- Проведён разбор NSF SBIR Phase I eligibility специально для фаундера на визе O-1, с альтернативными non-dilutive источниками финансирования: EIC Accelerator, Aptos Payments Grant, Ethereum Foundation ESP
- Отчёт структурирован как 5 аргументов за / 5 против запуска Flight Recorder, 3 именованные стратегические опции с trade-offs, и единая рекомендация, привязанная к 45-дневному discovery-бенчмарку go/no-go

## Рекомендации / решения
- Использовать 45-дневный customer discovery как формальный go/no-go бенчмарк перед вложением ресурсов в Flight Recorder
- Проверить применимость NSF SBIR Phase I для структуры компании с co-founder-гражданином США (>50% ownership) как обходной путь для фаундера на O-1
- Рассмотреть non-dilutive альтернативы (EIC Accelerator, Aptos Payments Grant, Ethereum Foundation ESP) параллельно с NSF, если eligibility окажется проблемной

## Сущности
- **Люди:** Anton
- **Компании:** Runta, NeuralTrust, Lemma, Nava, a16z, Okta, WorkOS, Skyfire, Catena, Visa, Mastercard, Stripe, Turnkey, Fireblocks, Privy
- **Продукты/инструменты:** Flight Recorder, x402, Stripe MPP, NSF SBIR Phase I, EIC Accelerator, Aptos Payments Grant, Ethereum Foundation ESP, GENIUS Act, EU AI Act

## Открытые вопросы
- Итоговый вердикт по нише (занята / есть окно / данных мало) и confidence tier не виден в имеющемся тексте — только в полном артефакте
- Точный ответ по NSF Phase I eligibility для O-1 (проходит ли фаундер без грин-карты, точные цитаты правил с nsf.gov/seedfund) не приведён в сводке, требует чтения артефакта
- Не указано, был ли найден кто-либо, кто уже делает financial-grade audit trail + authorization proof именно для денежных действий агентов (ключевой вопрос части 2 отчёта)
- Отчёт собран из нескольких AI-вендоров по теме реестра, но в данном фрагменте присутствует только секция claude — сравнение с другими вендорами недоступно

## Источник
- DR-ID `DR26-09-01-ZB-01-1426` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- agent-assurance
- AI financial action verification
- agentic payments security
- NSF SBIR Phase I eligibility
- O-1 visa non-dilutive funding
- stablecoin infra financing
- policy proof / replay для AI-агентов
