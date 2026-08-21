---
dr_id: DR26-07-06-ZB-01
title: "Выбор сигнального источника intent-to-buy для AI-CRM «Радар возможностей» (крипто/Web3)"
date: 2026-07-06
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-06-ZB-01): Выбор сигнального источника intent-to-buy для AI-CRM «Радар возможностей» (крипто/Web3)

> DR сравнивает Web3.career Jobs API, SEC EDGAR, крипто-нативные раунды (DefiLlama/RootData/CryptoRank/Messari) и X API как источник сигнала «звонить прямо сейчас» для крипто-CRM с бюджетом ~$0 и Python stdlib, и рекомендует v1-архитектуру.

## Ключевые выводы
- Web3.career Jobs API — лучший компромисс для v1: официальный API, бесплатный доступ к 100,175 Web3 job listings, простая аутентификация через token query param, есть отдельный RSS/XML endpoint /v1.xml, поддержка stdlib-парсинга (xml.etree)
- DefiLlama Raises — сильнее по смыслу сигнала (7101 funding rounds, ручная верификация раундов), но не основной источник для v1: официальный raises-endpoint платный (Pro API $300/мес), а Terms прямо запрещают scraping/robots для коммерческого использования
- RootData — 19,000+ crypto projects, 8,600 funding rounds, ежедневные апдейты и хороший entity search (Project/VC/People), но fundraising-endpoint доступен только на Plus ($128/мес) или Pro ($328/мес), плюс 2 credits за запись
- CryptoRank — 10K+ funding rounds, есть search по name/ticker→id/slug (хорош для entity resolution), но free sandbox лишь 400 credits/day, funding feed требует Pro ($399/мес), детали раунда — Business ($699/мес)
- Messari Fundraising API — 14k+ rounds, 15k+ investors, институциональный продукт, но доступ только через Enterprise/sales, для v1 бюджетно не подходит
- SEC EDGAR Form D — бесплатный и официальный, но низкое покрытие для крипто/Web3-базы (US-regulatory slice), удобный structured dataset публикуется только quarterly, real-time путь требует отдельного парсинга EDGAR API
- Adzuna — не крипто-нативный, free-лимиты малы (25/min, 250/day, 1000/week, 2500/month), коммерческое использование по licensing выглядит как 14-day trial, не опора для постоянного продукта
- X API — лучшая свежесть (filtered stream P99 latency 6-7 сек), но Recent Search видит только последние 7 дней, а filtered stream на pay-per-use ограничен 1,000 rules per project — мало против ~7,400 организаций в базе
- Главный риск Web3.career — не доступ, а шумность сигнала (найм ≠ прямой intent-to-buy): нужен scoring-слой поверх фида (роль, seniority, GTM/partnerships/enterprise/compliance/API/devrel словарь, burst вакансий за 30 дней)
- Второй риск для всех источников — entity resolution: одна крипто-сущность может фигурировать как Foundation/Labs/Protocol/DAO/Network/тикер токена одновременно; платные провайдеры (RootData, CryptoRank, Messari) дают готовый entity graph, бесплатный jobs-фид — нет

## Рекомендации / решения
- Взять Web3.career Jobs API как единственный основной сигнальный источник для v1 (бесплатно, официально, crypto-native, stdlib-совместимо)
- Построить поверх фида собственный scoring/фильтр по ролям (GTM, partnerships, enterprise, compliance, integrations, API/devrel) вместо использования сырого потока вакансий как есть
- Построить собственную alias/normalization таблицу названий компаний (Foundation/Labs/Protocol/DAO варианты одной сущности), так как источник не даёт entity graph
- Держать DefiLlama Raises как резерв №2 и первый апгрейд, когда бюджет вырастет до $128–300/мес или появится официальное разрешение на доступ к raises-данным
- Не скрейпить публичный сайт DefiLlama Raises — это прямое нарушение Terms of Use для коммерческого использования
- Не использовать X API как основной v1-источник (лимиты 7 дней/1000 rules плохо масштабируются на ~7,400 организаций) — рассматривать только как enrichment-слой позже
- Не выбирать Adzuna и SEC EDGAR как основной крипто-сигнал (плохое покрытие/licensing-fit), использовать их лишь как вспомогательные поздние enrichment-источники

## Сущности
- **Люди:** —
- **Компании:** Web3.career, DefiLlama, RootData, CryptoRank, Messari, SEC EDGAR, Adzuna, CryptoJobsList, X (Twitter)
- **Продукты/инструменты:** Web3.career Jobs API, Web3.career RSS/XML endpoint (/v1.xml), DefiLlama Raises API, RootData Funding API, CryptoRank Funding API, Messari Fundraising API, SEC EDGAR Form D Data Sets, Adzuna API, X API Filtered Stream / Recent Search

## Открытые вопросы
- Точный base URL хоста Web3.career API не виден в индексируемых сниппетах docs — нужно уточнить при интеграции
- Насколько стабильна нестандартность JSON-ответа Web3.career (top-level array mixed types, jobs-массив обычно на index 2) — риск хрупкости парсера
- При каком именно росте бюджета (порог между $128 и $300/мес) переходить на RootData vs DefiLlama Pro как апгрейд
- Как именно строить alias/entity-resolution таблицу для крипто-сущностей своими силами — механизм в отчёте не детализирован

## Источник
- DR-ID `DR26-07-06-ZB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- AI-CRM Радар возможностей
- intent-to-buy signal
- crypto/Web3 data providers
- ak47-simplicity
- vault-data-architecture
- decision-radar-signal-source-v1
- insight-DR-DR26-07-28-HUB-16-2339-выбор-источника-сигнала-intent-to-buy-для-ai-crm-р — практически идентичный DR (тот же вопрос, тот же рекомендованный источник) на 3 недели раньше
