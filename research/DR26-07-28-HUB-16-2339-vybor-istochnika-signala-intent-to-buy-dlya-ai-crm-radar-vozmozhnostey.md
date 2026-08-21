---
dr_id: DR26-07-28-HUB-16-2339
title: "Выбор источника сигнала intent-to-buy для AI-CRM «Радар возможностей» (крипто/Web3-база)"
date: 2026-07-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-16-2339): Выбор источника сигнала intent-to-buy для AI-CRM «Радар возможностей» (крипто/Web3-база)

> Deep research сравнил источники сигналов (вакансии, раунды, соцсети) для AI-CRM с ~7400 крипто/Web3-лидами и бюджетом ~$0, и выбрал одно рекомендованное решение для v1 плюс резерв.

## Ключевые выводы
- ТОП-1 для v1: Web3.career Jobs API — официально заявлен free access к 100,175 Web3 job listings через единый endpoint, авторизация простым query-параметром `token`, есть отдельный RSS/XML endpoint `/v1.xml` — лучшее сочетание крипто-покрытия, нулевой цены, официального API и совместимости с Python stdlib.
- Резерв №2: DefiLlama Raises — сильнее по смыслу сигнала (крипто-нативная база раундов, на сайте 7101 funding rounds, ручная верификация), но не годится для v1: официальный raises-endpoint только в Pro-тарифе за $300/мес, а Terms прямо запрещают scraping/robots для коммерческого использования.
- RootData: 19,000+ crypto projects, 8,600 funding rounds, ежедневные обновления, сильный entity resolution (поиск по Project/VC/People), но fundraising-endpoint доступен только с Plus ($128/мес) или Pro ($328/мес) — дорого для v1.
- CryptoRank Funding API: 10K+ раундов, 11K+ фондов/инвесторов, поиск по name/ticker → id/slug (хорошее сопоставление названий), но бесплатный sandbox только 400 credits/day, полный feed требует Pro ($399/мес) или Business ($699/мес).
- Messari Fundraising API: 14k+ rounds, 15k+ investors — институциональный продукт, доступ к fundraising-данным только через Enterprise/sales-контакт, бюджетно не подходит.
- SEC EDGAR Form D: официальный и бесплатный, но узок для крипто/Web3-базы (US-regulatory exempt-offerings), а структурированные Form D datasets публикуются только ежеквартально — плохая свежесть для удобного формата.
- Adzuna API: не крипто-специализирован, free-tier мал (25/min, 250/day, 1000/week, 2500/month), а коммерческое использование сверх личного/исследовательского фактически ограничено 14-дневным trial.
- CryptoJobsList: крипто-специализированный джоб-борд, но публичного/официального API не найдено.
- X/Twitter API: лучшая свежесть (filtered stream P99 latency ~6-7 сек), но recent search видит только последние 7 дней, а filtered stream на pay-per-use ограничен 1,000 rules per project — мало против ~7,400 организаций, поэтому не годится как основной v1-источник.

## Рекомендации / решения
- Взять Web3.career Jobs API как единственный основной сигнал для v1, добавив поверх собственный scoring-слой фильтрации шума (роли GTM/partnerships/enterprise/compliance/integrations/devrel, seniority, частота burst вакансий на компанию).
- Держать DefiLlama Raises как резерв №2 и переходить на него (или на RootData), когда бюджет вырастет хотя бы до $128–300/мес.
- Построить собственную таблицу alias/нормализации названий компаний — бесплатный Web3.career-фид не даёт готового тикера/домена/alias-графа сущностей.
- Не скрейпить публичный сайт DefiLlama для коммерческого использования — это запрещено Terms of Use; легальный путь только через платный официальный API или письменное разрешение.
- Не использовать X API как основной v1-источник из-за лимитов recent-search (7 дней) и filtered stream (1,000 rules/project); рассматривать только как enrichment-слой при наличии точных official handles.
- Adzuna и SEC EDGAR Form D откладывать как более поздние вспомогательные enrichment-источники, не как основной крипто/Web3 signal feed.

## Сущности
- **Люди:** —
- **Компании:** —
- **Продукты/инструменты:** Web3.career, DefiLlama, RootData, CryptoRank, Messari, Adzuna, SEC EDGAR, CryptoJobsList, X (Twitter) API

## Открытые вопросы
- Точный base host URL Web3.career API не виден в индексируемых сниппетах документации — нужно уточнить при интеграции.
- Насколько эффективен scoring-слой фильтрации шума на практике — не протестировано, только предложено концептуально.
- Вырастет ли бюджет до уровня, открывающего RootData/DefiLlama Pro (от $128 до $300+/мес) — не решено.
- Готового бесплатного источника entity-resolution/alias-данных для крипто-компаний не найдено — требуется строить самостоятельно.

## Источник
- DR-ID `DR26-07-28-HUB-16-2339` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-28-HUB-16-2339-deep-research-signalov-dlya-ai-crm-chatgpt.md`
- оригинал: `E:\Obsidian\Anton-Knowledge\01-Conversations\ChatGPT\conversations\2026-07-06-deep-research-signalov-dlya-ai-crm-6a4c134d.md`

## Связано
- [[AI-CRM]]
- [[Радар возможностей]]
- [[intent-to-buy сигнал]]
- [[крипто/Web3 lead sourcing]]
- [[entity resolution]]
