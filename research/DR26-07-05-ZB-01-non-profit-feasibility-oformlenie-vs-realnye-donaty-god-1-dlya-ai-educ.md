---
dr_id: DR26-07-05-ZB-01
title: "Non-profit feasibility: оформление vs реальные донаты год-1 для AI-education проекта"
date: 2026-07-05
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-05-ZB-01): Non-profit feasibility: оформление vs реальные донаты год-1 для AI-education проекта

> Отчёт отвечает, стоит ли соло-фаундеру (Португалия, $0 на счёте, команда 3 человека) регистрировать non-profit для open-source AI-образовательного проекта прямо сейчас, или сначала собирать донаты неформально.

## Ключевые выводы
- Оба вендора сходятся: собственный non-profit сейчас открывать не стоит — при $0 на счёте оформление (US 501(c)(3), Portugal associação, Estonia MTÜ) съест больше времени/денег, чем принесёт в год-1.
- US 501(c)(3) удалённо: 6–10 недель до легального приёма tax-deductible донатов (bottleneck — EIN без SSN/ITIN через факс/почту), стоимость $400–$550 DIY / $3.5k–$10k с юристом, но даёт tax-deduction для US донов.
- Portugal associação: регистрация €300 за 1–3 недели, но полная модель органов требует 9 учредителей (по 3 в Direção/Conselho Fiscal/Mesa da Assembleia Geral) — тяжело для команды из 3; статус Utilidade Pública (налоговые льготы донорам) не даётся автоматически, требует ~3 года работы организации.
- Estonia MTÜ через e-Residency — оба вендора НЕ рекомендуют: не даёт tax-deductibility для US донов, добавляет cross-border tax слой для португальского резидента, никакого fundraising unlock.
- Fiscal sponsorship — лучший маршрут без своего юрлица: HCB (Hack Club Bank) даёт полноценный 501(c)(3)-статус, гранты, payroll за 7% revenue, без setup fees; Open Source Collective (OSC) — 10% fee, но это 501(c)(6), НЕ charity, донаты через него не tax-deductible для US донов (IRS не считает open-source software автоматически public benefit).
- Break-even расчёт: экономически свой US 501(c)(3) оправдан только при pipeline ≥$50k–100k/год или подписанном grant/sponsor LOI ≥$25k–50k, требующем собственного юрлица.
- Реальные бенчмарки для схожих проектов отрезвляющие: The Odin Project (1.78M учащихся) собирает лишь $8.6k–18k/год через Open Collective; freeCodeCamp основатель вложил $150k личных денег в первые 3 года до значимых донатов; ML Collective держится на волонтёрах, донаты только под конкретные события (~$50k на конференцию).
- Прогноз для проекта Антона год-1 без anchor donor: pessimistic $0.5k–5k, base $3k–15k, optimistic $15k–50k; медиана грантов для нового микро-non-profit — $0; corporate sponsorships реалистичнее грантов ($2.5k–10k base, до $75k optimistic).
- Founder salary разрешена во всех структурах, но требует independent board approval, reasonable-compensation тест и запрет self-dealing (excess benefit transaction по IRS); fiscal sponsors (HCB) имеют встроенный payroll — самый простой путь к первой зарплате.
- Главные убийцы микро-non-profit в первые 2 года: 'board theater' (друзья в board без реальной fiduciary роли), банковский KYC (особенно для ex-crypto/иностранного резидента), grant reporting overhead, restricted funds misuse, brand confusion между non-profit и параллельным paid consulting.

## Рекомендации / решения
- Не регистрировать собственное юрлицо сейчас; сразу принимать донаты через GitHub Sponsors + Ko-fi + публичную budget-страницу + sponsor deck (0–7 дней).
- Параллельно (через 2–4 недели) подать в fiscal sponsor: HCB — если нужен 501(c)(3)/гранты/payroll; OSC — если важнее open-source community/transparency, чем charity-статус.
- Возвращаться к вопросу собственного US 501(c)(3) только после pipeline $50k–100k/год или signed grant/sponsor LOI ≥$25k–50k, требующего своего юрлица.
- Portugal associação рассматривать только под конкретный PT/EU грант, университет или муниципального партнёра, требующего локальное юрлицо — не как дефолтный маршрут.
- Estonia MTÜ не использовать вовсе для этого кейса — нет fundraising upside, есть лишний cross-border tax слой.
- Держать paid AI-consulting фаундера юридически и брендово отдельно от non-profit/donation-бренда: отдельные счета, страницы, disclosure, никакого founder self-approval зарплаты/контрактов.
- Следовать 90-дневному плану: days 0–7 donation-page без юрлица, days 7–30 заявки в fiscal sponsors + sponsor outreach (30 спонсоров), days 30–60 fundraising campaign с публичными метриками, days 60–90 decision gate по факту собранных донатов/grant LOI.

## Сущности
- **Люди:** Quincy Larson (основатель freeCodeCamp)
- **Компании:** Hack Club Bank (HCB), Open Source Collective (OSC), Open Collective Foundation, Fast Forward, OpenAI, Anthropic (Claude Corps), Schmidt Futures / Schmidt Sciences, Mozilla Foundation, freeCodeCamp, The Odin Project, ML Collective, Digital Europe Programme, Amalia (Portugal open-source AI model)
- **Продукты/инструменты:** GitHub Sponsors, Ko-fi, IRS Form 1023-EZ / 1023, Form 990-N, e-Residency (Эстония)

## Открытые вопросы
- Нет надёжной публичной медианы 'first-year donations for tiny AI-education nonprofits' — доступные данные сильно смещены survivorship bias (провалившиеся микропроекты не публикуют цифры).
- Возможно ли для Portugal associação досрочно получить Estatuto de Utilidade Pública (обычно требует 3 года работы) через исключительное решение правительства — неясно.
- Неизвестна реальная вероятность acceptance в HCB/OSC/Fast Forward для конкретного проекта Антона.
- Не проверено на практике, насколько банковский KYC (ex-crypto background + Portugal residency + $0 стартовый капитал) реально затормозит открытие счёта для US-структуры.

## Источник
- DR-ID `DR26-07-05-ZB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-ZB-01-nonprofit-chatgpt.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-ZB-01-nonprofit-gemini.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-ZB-01-nonprofit-grok.md`

## Связано
- [[ai-education-project]]
- [[second-brain-northstar]]
- [[non-profit-fiscal-sponsorship]]
- [[open-source-funding]]
- [[portugal-associacao]]
