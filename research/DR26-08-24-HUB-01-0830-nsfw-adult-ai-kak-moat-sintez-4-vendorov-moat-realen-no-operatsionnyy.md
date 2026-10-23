---
dr_id: DR26-08-24-HUB-01-0830
title: "NSFW/adult AI как moat — синтез 4 вендоров (moat реален, но операционный; победил угол OF-copilot)"
date: 2026-08-24
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-24-HUB-01-0830): NSFW/adult AI как moat — синтез 4 вендоров

> Гипотеза Антона (24.08, голосом): NSFW = защитный moat для AI-стартапа, потому что крупные вендоры туда структурно не пойдут. Веер: ChatGPT (gpt-5-6-pro, 69 источников) · Gemini (Deep Research) · Grok (116 источников, через Firefox) · GLM-5.2 (Advanced Search). claude.ai — ран шёл >1ч, добор ночным `dr_collect`. Кворум 4/6 закрыт.

## 🤝 ГДЕ СОШЛИСЬ ВСЕ 4 (сильный консенсус)

1. **Премиса «большие не придут» подтверждена ДВАЖДЫ [established]:** OpenAI запаузил adult mode бессрочно (март 2026, давление сотрудников/инвесторов); **xAI выпилил Grok-компаньонов Ani/Valentine/Mika 24.07.2026** («это был эксперимент»). Anthropic/Google — ноль движения. Оба самых смелых захода больших вендоров откатились.
2. **Но moat НЕ в модели.** Open-weights (Qwen/Mistral abliterated, 200+ NSFW-моделей на HuggingFace, Featherless/OpenRouter) закрыли capability-gap бесплатно. Настоящий moat = **операционный**: high-risk платежи (CCBill/Segpay/Epoch 5-10% + резервы 90-180 дней, Visa VAMP порог 0.9%), запреты app-store и рекламы, age-verification, compliance-ноу-хау + **проприетарные outcome-labelled диалоги** (данные, которых нет в open-weights).
3. **Победивший угол у всех четырёх один: НЕ автономный бот, а human-in-the-loop copilot для OF/Fanvue-агентств** + data-moat из реальных диалогов + compliance-обёртка. Формулировки разные (Grok «hybrid AI-assist», Gemini «Compliant B2B AI CRM», GLM «Angle 4 + data + compliance», ChatGPT «Intimacy AI Trust & Revenue OS / CreatorCopilot») — суть одна.
4. **Почему AI не заменил чатеров (4 разрыва):** long-horizon память (месяцы отношений с китом), persona-consistency + «человеческие несовершенства» (keyboard smash, опечатки), whale-психология как ремесло с редким обучающим сигналом, и ToS OnlyFans (человек жмёт send) — юридическая константа. Гибрид = равновесие.
5. **Терапевт-угол — избегать [established]:** FDA активно ужесточает границу wellness/device (DHAC ноя-2025), state-лицензирование. Только «wellness/coaching» упаковка.
6. **Waifu-угол — жестокий рынок:** трекаемый мобильный спенд $162.8M за H1 2026 (Appfigures), топ-10% приложений забирают 89% выручки; Replika падает ($14M→$4.8M ARR), Character.AI ушёл в acqui-hire Google, Ex-Human снесён Apple. Прибыльно для бутстрапа, слабо для венчура.
7. **Инвест-упаковка:** «adult AI» не фандится; фандится «creator-economy infrastructure / privacy / compliance». Venice.ai — единорог ($65M Series A, $1B, ~$70M ARR) но от **крипто-капитала** (Dragonfly/Coinbase); Fanvue $22M Series A от exited-founders (Inner Circle), не brand-name VC. Сам состав инвесторов = подтверждение vice-clause-барьера.

## ⚔️ ГДЕ РАЗОШЛИСЬ (не сглаживать)

- **Uncensored-инфра слой:** Gemini ставит его №1 из четырёх исходных углов (B2B-мультипликаторы); GLM говорит «skip — Venice уже победил, капиталоёмко»; Grok №3; ChatGPT — в составе 5-го угла. Развилка: строить свою инфру vs арендовать чужую. Для соло-фаундера с AI-флотом консенсус склоняется к «арендовать».
- **Размер рынка:** консультантские прогнозы ($10-36B companion) все четверо зовут мусором; реальные цифры расходятся в 3× по voice ($4.4B Grand View vs $12.1B Fortune BI). Рабочая цифра: adult online ~$66-73B, AI-slice <1% сегодня, адресуемый пул №1 = OF chat/PPV ~$4-5B.
- **Уникальное у GLM:** xAI retirement 24.07 (свежайшее), Venice-единорог, NY-закон параллельный SB243, UK «request offense» с фев-2026. **Уникальное у ChatGPT:** Quinn (человеческое аудио, $1.8M/мес — AI-voice пока проигрывает живому голосу), Wrtn (narrative-worlds упаковка), провалы Dot/Soulmate/Woebot, детальный payment-плейбук (2 PSP, descriptor testing, written approval на gen-AI модель). **Уникальное у Gemini:** SB243 private right of action $1000/нарушение;識 «Identity Discontinuity» (Replika-траур). **Уникальное у Grok:** hybrid-экономика чатеров, Appfigures-цифры по приложениям (Zeta $33M H1).

## 💎 Что это значит для НАС (связка с волтом)

Консенсус-рекомендация 4 вендоров — это **ровно пространство Виктора (alpha-viktor-onlyfans-crm-2026-06-11) и OnlyMonster Павла (alpha-pavel-onlymonster-creator-os-2026-07-11)**, которых Антон уже знает лично. DR 18.06 говорил то же: «двигаться вверх по стеку, data-moat из диалогов = единственное, что Supercreator не скопирует». Внешний веер независимо подтвердил нашу внутреннюю альфу двухмесячной давности.

## Связано
nsfw-ai-dr-post-series-2026-08-24 (5 черновиков постов по этому синтезу, 24.08) · concept-adult-creator-economy · alpha-viktor-onlyfans-crm-knowledge-update-2026-06-18 · insight-DR-DR26-07-28-HUB-26-2339-молодые-предприниматели-вокруг-onlyfans-adult-рынк · decision-2026-08-24-nsfw-ai-moat
- insight-DR-DR26-08-25-MACANTON-15-0740-venice-ai-vs-fanvue-сравнение-финансирования-оценк — тот же кластер DR за день до — сравнивает Venice и Fanvue
- insight-DR-DR26-08-29-MACANTON-18-0743-venice-ai-vs-fanvue-проверка-фактов-про-unicorn-ст — уже ссылается на сиблинг-DR по Venice/Fanvue, нужен и этот более свежий фактчек

## Оригиналы
- `_originals/deep-research/DR26-08-24-HUB-01-0830-nsfw-ai-moat-grok.md` (116 sources)
- `_originals/deep-research/DR26-08-24-HUB-01-0830-nsfw-ai-moat-gemini.md`
- `_originals/deep-research/DR26-08-24-HUB-01-0830-nsfw-ai-moat-glm.md`
- `_originals/deep-research/DR26-08-24-HUB-01-0830-nsfw-ai-moat-chatgpt.md` (69 sources, gpt-5-6-pro)
- claudeai: ран не завершён на момент синтеза, чат https://приватный чат (добор ночной)

## 🆕 Пятый лег: claude.ai (добран 25.08 22:09, после починки dr_collect)
Кворум был закрыт без него; лег добран вручную ремонтной сессией (bb-аккаунт был невидим ночному сборщику, см. Breakage-Journal 25.08). Что он МЕНЯЕТ, а не повторяет:
- **Разошёлся в ранжировании углов.** Четвёрка ставила первым copilot для агентств. claude.ai ставит первым **«торговца оружием» (Angle 5)** — compliance + платежи + модерация + takedown-tooling ДЛЯ всей ниши, а copilot считает лишь wedge'ом для кэша и данных. Аргумент: моат ниши = чокпоинты (карточные сети, эквайринг, app-store, регулятор), и продавать надо именно их, потому что каждый новый закон делает продукт дороже, а контентной ответственности ноль. Для рейза это чище всего проходит LP vice-clause.
- **Совпало с решением Антона по упаковке**: «не adult AI, а creator-monetization infrastructure + compliance» — claude.ai пришёл к той же рамке независимо, снизу, от чокпоинтов.
- **Новая конкретика к переговорам (даты и цифры, не вайб):** TAKE IT DOWN Act — FTC начала enforcement 19.05.2026, до **$53 088 за нарушение**, 48ч на снятие NCII; California SB 243 (companion-боты) с 01.01.2026, $1 000 за нарушение + частный иск; NY-закон с 05.11.2025. Платежи: у Segpay из семи эквайеров **только один US принимает AI adult**, PayPal — полный бан; ставки 5-10% + резервы 0-20% на 90-180 дней, порог чарджбэков ~0.75%. Причина смерти в нише названа прямо: «больше AI-компаньонов умирает от платежей, чем от продукта».
- **Против нашей же гипотезы (не сглаживаю):** если моат сводится к «готовности терпеть банковскую боль» — это барьер, а не монополия, повторяемый любым упрямым; и «indefinitely paused» у OpenAI ≠ «навсегда».
