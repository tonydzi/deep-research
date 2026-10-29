---
dr_id: DR26-08-29-MACANTON-22-0759
title: "EU entity для non-dilutive funding (EIC/Horizon) — выбор юрисдикции под US-parent"
date: 2026-08-29
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-29-MACANTON-22-0759): EU entity для non-dilutive funding (EIC/Horizon) — выбор юрисдикции под US-parent

> Отчёт сравнивает Estonia, Ireland, Netherlands, Cyprus, Luxembourg, Lithuania и Malta как EU-дочку под Delaware C-Corp для соответствия EIC/Horizon Europe и минимизации трения (налоги, банкинг, PT CFC/PE риск), рекомендуя Estonia как топ-выбор.

## Ключевые выводы
- EIC/Horizon Europe требует, чтобы заявитель был юрлицом, учреждённым в стране ЕС или ассоциированной стране Horizon Europe; при non-EU родителе (Delaware) EU-дочка должна быть 'effectively established' к моменту Step 2 полной заявки.
- Все 7 кандидатов (Estonia, Ireland, Netherlands, Cyprus, Luxembourg, Lithuania, Malta) юридически проходят гейт EIC как члены ЕС.
- Ранжирование: 1-е место Estonia, 2-е Ireland, 3-е Netherlands; Cyprus интересен но добавляет трение (с 2026 defensive WHT меры на low-tax jurisdictions); Luxembourg/Malta/Lithuania рабочие, но не оптимальны.
- Estonia: 0% налог на нераспределённую/реинвестированную прибыль, 20% на дивиденды при распределении, 0% withholding tax на дивиденды нерезидентам — удобно для pre-seed с ограниченным кэшем; полностью удалённое учреждение через e-Residency, налоговое резидентство по факту регистрации (не по management&control).
- Ireland: 12.5% на активный трейдинговый доход (25% на пассивный), сильный US-treaty и репутация 'blue-chip' для US acquirer, но требует EEA-резидента-директора либо Section 137 bond (~€1,600 за 2 года) и банки часто требуют личного присутствия.
- Netherlands: 19%/25.8% CIT, Innovation Box снижает эффективную ставку на квалифицирующую IP-прибыль до 9%, но существенные требования к substance (реальные люди/decision-making в NL) — избыточно для полностью удалённой pre-seed команды.
- Ключевой риск для основателя-резидента Португалии: CFC-правила PT применяются, если эффективная ставка налога <50% от PT CIT, и PE-риск, если контракты/решения фактически исполняются из Португалии — Estonia не входит в чёрный список PT, но требует документированного substance (EE-адрес, контакт-персона, EE-подписанные контракты, board-решения в EE timezone).
- Рекомендуемая структура: Delaware C-Corp как top-level parent + Estonian OÜ как wholly-owned subsidiary для EIC-проектов и EU service revenue; IP либо остаётся в Delaware с лицензией на arm's-length условиях EE-дочке (проще на pre-seed), либо позже передаётся EE-дочке с transfer-pricing документацией.
- Стоимость и сроки для Estonia: e-Residency ~€100-120 + доставка; регистрация OÜ 3-8 недель; разовая настройка €800-1,500; ежегодные расходы €1,500-3,000 (бухгалтерия, адрес/контакт-персона).

## Рекомендации / решения
- Выбрать Estonia как основную EU-юрисдикцию для EIC/Horizon-заявки: 0% налог на реинвестируемую прибыль, полностью удалённое e-Residency оформление, отсутствие withholding tax на дивиденды, сильная digital-репутация без stigma 'tax haven'.
- Держать Delaware C-Corp как материнскую компанию (для US-инвесторов и acqui-hire), создать Estonian OÜ как дочку, через которую проходит EIC-финансируемая работа и EU service revenue.
- На старте использовать простую IP-схему: core IP остаётся в Delaware, лицензируется EE-дочке на arm's-length условиях; позже при необходимости — передать EU-специфичный IP в Estonia с transfer-pricing memo (FAR-анализ).
- Для банкинга не полагаться на классический эстонский банк (строгий AML для удалённых основателей) — использовать fintech EU IBAN (Wise Business, Revolut Business и т.п.).
- Обязательно проконсультироваться с португальским налоговым юристом по CFC/PE-рискам и документировать substance в Estonia (адрес, контакт-персона, EE-подписанные контракты, board-решения в EE-часовом поясе), чтобы избежать риска, что PT сочтёт компанию управляемой из Португалии.
- Ireland рассматривать как запасной вариант, если планируется наём команды в ЕС в ближайшее время и приемлемо больше административного трения (EEA-директор/bond, банкинг с личным присутствием).
- Netherlands рассматривать только при значительной R&D/IP-структуре, оправдывающей Innovation Box и более высокие требования к substance.
- Заказать формальный transfer-pricing memo по мере роста, покрывающий функции/активы/риски EE-дочки, критично для PT и US-налоговых органов.

## Сущности
- **Люди:** —
- **Компании:** Delaware C-Corp (Antons's parent), Estonian OÜ (proposed subsidiary), Wise, Revolut Business, LHV, Swedbank, SEB, wamo
- **Продукты/инструменты:** EIC Accelerator, Horizon Europe, e-Residency (Estonia), Innovation Box (Netherlands), IP Box (Cyprus), SOPARFI (Luxembourg)

## Открытые вопросы
- Точный ответ по применимости PT CFC 'motive test' исключения (ATAD-aligned) к конкретной структуре основателя требует подтверждения у португальского налогового юриста.
- Актуальные ставки Malta imputation/refund system 'verify latest rates' — отчёт помечает как требующие проверки, реформы продолжаются.
- Cyprus defensive WHT/deductibility меры с 2026 на платежи связанным компаниям в low-tax jurisdictions — нужно уточнить, не создаёт ли структура непрямую LTJ-экспозицию.
- Решение по IP-структуре (Option A: лицензия vs Option B: передача IP Estonia) не финализировано — зависит от будущих инвесторов и масштаба.
- Выбор конкретного банковского провайдера (эстонский банк vs fintech EMI) не сделан.

## Источник
- DR-ID `DR26-08-29-MACANTON-22-0759` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- EIC Accelerator eligibility
- Horizon Europe funding
- Estonia e-Residency
- Portugal CFC rules
- US-parent EU-subsidiary structure
- non-dilutive funding
- transfer pricing
- Delaware C-Corp
