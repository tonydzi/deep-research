---
dr_id: DR26-09-23-ZB-02-1231
title: "Иностранные AI/compute-компании в Долине — кому нужен местный landing lead с комьюнити, и где открытые двери"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# DR26-09-23-ZB-02-1231 — Иностранные богатые AI/compute-компании в Долине: нужен ли им «landing lead» с комнатой

> Заказ: Антон, 23.09.2026 12:31, голосом. Расширение ДР №1 (китайские лабы) на ЛЮБЫЕ иностранные супербогатые AI/compute-компании (тип «Lambda, но иностранная»): Корея, Япония, Индия, Сингапур, Залив, Израиль, Европа. Потребитель: отдельный CV-кластер `sv-landing` + максимально активный apply.
> Рельсы: **Grok** (headless DeeperSearch, 60 КБ, 71 URL) · **claude.ai Fable 5.1 Max + Research** (44 КБ, 429+ источников, артефакт на русском) · **Mistral Vibe deep-research** (канвас, ~40 компаний, 47 источников) · **ChatGPT Deep Research** (18 мин, 53 цитаты, 556 поисков, 67 КБ). Gemini и GLM не доехали.
> Оригиналы: «внутренний архив лаборатории»{grok,claudeai,mistral,chatgpt}.md` + `_originals\deep-research\DR26-09-23-ZB-02-1231-*`; ноги и чаты в `legs.md` рядом.

## TL;DR — вердикт четырёх рельс совпал

**Гипотеза верна наполовину, и «половина» — не та, что заказывали.** Все четыре рельсы независимо сказали одно:

1. Иностранные компании реально открывают Долину в 2025–2026 и реально нанимают местных. Но вакансия называется не «landing lead / Head of US Ecosystem», а **Venture Ecosystem Lead · Director of DevRel · Partner Manager SI/ISV · Events Lead North America · Field & Partner Marketing · Silicon Ecosystem Partnerships Director**. Место GM/US CEO уже занято (Mistral — Janiewicz, Nebius — Lawrence, Upstage — Roh, Rebellions — Choy, G42 US — Dalloul); открыт слой ПОД этим человеком.
2. Чистый спрос — **европейский, британский, корейский**: Nebius, ElevenLabs, Mistral, Black Forest Labs, FuriosaAI, Upstage, Nscale, n8n, DeepL, Lovable, FriendliAI, Sarvam (research). Китайские лабы — самые большие продукты и самая радиоактивная зона месяца: 8.09.2026 NSA/CISA/FBI (AA26-251A) назвали DeepSeek, Moonshot, **Alibaba**, MiniMax, StepFun, Z.AI в advisory о дистилляции; Zhipu в Entity List с 16.01.2025; 21.07.2026 Treasury угрожал санкциями китайским производителям моделей; 17.09.2026 Reuters про Qwen в госсайте США. Все четыре рельсы: **не строить бренд на китайских логотипах в этот месяц**.
3. Тезис «оператор с готовой комнатой настолько дефицитен, что его возьмут без big-tech-бренда» — **не доказан ни одной рельсой**; это гипотеза, проверяемая только response rate. Grok прямо: «he is not scarce enough to be handed a country-manager title».
4. Кампания = **~10 адресных писем, не 40** (Grok), две трети усилий на компании с ЖИВЫМ реквизитом (ChatGPT).

⚠️ **Факт для Антона, не решение за него:** заявка на Alibaba Cloud GenAI BD (Sunnyvale) ушла сегодня в 12:11 по решению после ДР №1. ДР №2 добавил три улики против Alibaba в этом месяце (advisory 8.09, Treasury 21.07, Reuters 17.09). Заявка отозвана не будет; но дальнейшие касания Alibaba (Alex Chen, Elaine Wu, ещё 4 роли Sunnyvale) — на его слово.

## Топ целей: консенсус 4 рельс (сколько рельс поставили компанию в свой топ-7)

| Компания | Страна | Рельсы в топ-7 | Живая дверь на 23.09 | Человек | Что делаем |
|---|---|---|---|---|---|
| **Nebius** | NL | grok #1 · claude #1 · mistral #4 (chatgpt: вне окна, SF с 2024) | Venture Ecosystem Lead SF (careers.nebius.com, индекс 8.09) · Director of Product, Ecosystem US remote **$228–285K** · ISV Partner BD **$141–176K** (уже в волне) · Sr Dev Advocate (подано, code-gate) | Mona Li VP Global Startup Ecosystem · Waqas Makhdum VP DevRel · Dan Lawrence SVP GM Americas | Добавить Venture Ecosystem Lead + Director Product Ecosystem. Head of Developer Community закрыт 9.03 — не писать «буду вашим Head of Community» |
| **ElevenLabs** | UK | chatgpt #1 · claude #2 · mistral #3 · grok #7 | Events Lead NA (подано, spam-flag) · Partner Programs & Marketplace (подано? entry_sv_7 = Partner Marketing NA) · Startup Partnerships SF («strong Bay Area network of VCs and startups») · DevRel Engineer (SF pref) · Developer Community Growth · Creative Partnerships | Alex Holt Field CTO (8.06.2026); CEO слишком высоко для первого письма | Перегнать Events Lead с хаба (Location = San Francisco), добавить Startup Partnerships + DevRel Engineer + Community Growth. 173 hires в Калифорнии (22.06.2026) |
| **Mistral AI** | FR | mistral #1 · grok #3 · claude #3 · chatgpt #4 | AI Developer Relations Engineer SF/Palo Alto · **VP Global Developer Relations** (claude: ~сен 2026) · Partner Manager SI USA · AI Deployment Strategist USA · Strategic Partner Lead OEM & Neoclouds | Marjorie Janiewicz US GM · ⚠️ Sophia Yang: Grok видит её Head of DevRel (пост 3.03.2026), claude.ai — «ушла в Fireworks AI» → перед письмом проверить X | ⛔ Кап Ashby 3/90д исчерпан 01.09 → только письмо. Адресат письма = Janiewicz (не Yang, пока не проверено) |
| **Black Forest Labs** | DE | 4/4 (grok #4, claude #4, mistral #9, chatgpt #12) | DevRel Engineer SF (claude, ~нач. сен: «build local SF developer community, hackathons») · FDE SF $180–300K · Sr Partnerships (подано, unconfirmed) | US lead не найден ни одной рельсой | Не перегонять без письма; спросить «кто владеет SF» |
| **FuriosaAI** | KR | chatgpt #2 · claude #5 · mistral #6 (grok: 15 чел, Santa Clara) | **Silicon Ecosystem Partnerships Director — US, Santa Clara** (Ashby furiosa-ai) · BD & Sales Global · PMM US · Solutions Architect US | June Paik CEO; US lead не назван | **Подать** (самая буквальная JD под гипотезу). Amber: экспорт-контроль чипов = compliance scoping, не отказ |
| **Upstage** | KR | 4/4 (grok #5, claude #7, mistral #5, chatgpt #8) | Открытой DevRel/ecosystem-роли нет. 2025 = «первый год в США», $157M total (employee post), San Jose = здание KOTRA | **Kasey Roh, US CEO** | Письмо «работать под вами», не «быть US CEO»; тёплый путь KOTRA SV (то же здание) |
| **Sarvam AI** | IN | 4/4 (grok #9, claude #6, mistral #8, chatgpt #14) | SF-офис с июля 2026, наём research, GTM не открыт | Devendra Chaplot (advisor, Palo Alto, ex-Mistral) · Pratyush Kumar | Письмо Chaplot «кофе в Palo Alto», без GTM-питча (инженер Sarvam сказал: наём research) |
| **Nscale** | UK | grok #2 only | **Director, Developer Relations SF $230–343K** (Teal 25.08) · DevRel Engineer $170–230K | Josh Payne CEO; hiring manager не назван | Подать через ATS (Ashby 404 в прошлом заходе → искать Teal/Glassdoor/сайт). ⛔ S-1 подана 18.09 — никаких «интро инвесторам» |
| **n8n** | DE | chatgpt #3 only | Senior Partner Manager SI — West Coast (Ashby n8n e3fadce2…, 14.09) **$157.5–211.75K + $67.5–90.75K комиссия + equity** | Jan Oberhauser CEO (был на SF-митапе 25.08) | Подать. Самая чистая документированная вилка в наборе |
| **DeepL** | DE | chatgpt #5 · mistral #12 | ⚠️ Расхождение: Mistral — только Austin (фев 2024); ChatGPT — **первый SF-офис 17.06.2026** (пресс-релиз DeepL + Mixhalo) и Field & Partner Marketing SF на Ashby DeepL | Sebastian Enderlein CTO | Подать Field & Partner Marketing SF. Предупреждение ChatGPT: май 2026 сокращение ~21% штата |
| **Lovable** | SE | grok #6 · chatgpt #6 · mistral #11 | Partner Development Manager Hyperscalers & GSIs SF | Anton Osika CEO; Tomas Halgas = technical evangelism (CB Insights 18.09) | ⛔ Кап 3/90д сожжён сегодня, обе роли closed. Только письмо-«дистрибуция», не кандидат |
| **FriendliAI** | KR (US-inc) | chatgpt #7 | 19 позиций, DevRel-место занято: **Daniel Gross Sr Developer Advocate с сен 2026 (ex-AWS)** | Byung-Gon Chun CEO · Daniel Gross | Прецедент, что корейский inference-cloud платит за US-advocacy. Письмо «рядом, не вместо» |
| Core42 / G42 | UAE | mistral #2 · grok #15 · claude #9 · chatgpt: screened out | US-мощности (Sunnyvale, Stockton, NY), SF-инжиниринг с 2024; community-реквизита нет | Sherif Tawfik CBO · Ali Dalloul CEO G42 US | Расхождение рельс (Mistral #2 vs ChatGPT «не допущен»). Только инфраструктурный брифинг, ⛔ никаких frontier-lab гостей |
| Rebellions | KR | 4/4, но место занято | Marshall Choy CBO + Jennifer Glore EVP = US-стендап ноябрь 2025 | Choy | Не «landing lead», а design-win завтрак |
| Parloa · Gradium · Granola · Founders Future · Northern Gritstone | DE/FR/UK | chatgpt only | Свежие SF-лендинги без ecosystem-места (create-the-function) | Kosub · — · Ursula Wild · — · Duncan Johnson | Вторая волна, «продать функцию до реквизита» |

Единодушно **вне списка**: DeepSeek, Moonshot, Z.ai, MiniMax, StepFun, ByteDance/BytePlus (4/4); Sakana (Tokyo only), Cohere (живой реквизит = NYC), Synthesia (NYC/Austin), Wayve (Sunnyvale = research), Airwallex, Kore.ai, MBZUAI (research host).

## Где рельсы расходятся (работа второго захода)

| Вопрос | Grok | claude.ai | Mistral | ChatGPT | Что делать |
|---|---|---|---|---|---|
| Sophia Yang (Mistral DevRel) | в роли, нанимает (3.03.2026) | **ушла в Fireworks AI**; Mistral ищет VP Global DevRel | не упоминает | не упоминает | Проверить X @sophiamyang перед письмом; письмо → Janiewicz |
| DeepL в Долине | — | — | только Austin | SF-офис 17.06.2026 (первоисточник DeepL) | ChatGPT с первоисточником, берём SF |
| Core42/G42 | #15, sovereign caution | #9 | **#2** | screened out | Не тратить первые 30 дней |
| Nebius | #1, живой SF-реквизит | #1 | #4 | вне окна (SF с 2024) | Окно ChatGPT формальное; Nebius в топ-2 |
| Alibaba | в advisory 8.09, ⛔ | #8, «не Entity List, но риск» | dangerous | hold/red (Reuters 17.09) | См. блок выше |
| Вилка «Head/Director» | $220–350K base (по двум director-band) | $220–320K+ | $180–280K | $200–250K + equity | Диапазон переговоров: **$220–300K** Director, **$160–220K** IC |
| Цена 90-дневного пакета | $60–80K фикс / четверть от $250–320K | ~$90–140K program spend | $25–60K/квартал | **$45–75K** | Предлагать $60–75K за 90 дней + программные расходы отдельно |
| Chief of Staff to US GM | ⛔ не целиться | в списке титулов | «лучший клин без DevRel-req» | Tier B для create-компаний | Только там, где реквизита нет (Upstage, Parloa, Gradium) |

## Ключевые факты (≥2 рельс, с датами)

- Nebius: Dan Lawrence SVP GM Americas 9.03.2026; Head of Developer Community снят в тот же день; два этажа SoMa 274 Brannan 24.08.2026; Nebius Inflection 9.06 (Volozh, Lawrence, Mona Li); Aishwarya Srinivasan — fractional DevRel Lead с 13.03.2026 (прецедент: работу делает человек с готовой аудиторией, дробно).
- ElevenLabs: $500M Series D при $11B (4.02.2026), >$330M ARR на конец 2025; California 173 hires (22.06.2026); SF-штат 43 (careers, 23.09).
- Mistral: Palo Alto ~30 чел (янв 2025), Series D €3B при >€21B (сен 2026, Samsung); ~20 SF/PA-вакансий; Brian Hall CMO 18.06.2026.
- FuriosaAI: $125M bridge 30.07.2025 при $735M, отказ Meta $800M; Santa Clara = North American hub; pre-IPO $300M+ (15.09.2025).
- Upstage: US entity с марта 2024, Kasey Roh; «2025 = год флага в США»; total funding $157M (employee post 17.12.2025).
- Sarvam: Sarvam Labs SF 18.03.2025; SF-офис + Chaplot advisor 29–30.07.2026; $234M/$300M Series B при $1.5B (15.06.2026, Mistral) vs «rumor» (Grok) — не пересчитывать жизнь под незакрытый раунд.
- Precedent DevRel-оттока: Sophia Yang ~1.5–2 года → Fireworks (claude); Daniel Gross AWS → FriendliAI (сен 2026, ChatGPT).
- Intermediaries = мосты, не конкуренты: KOTRA SV (3003 N 1st St San Jose = адрес Upstage), JETRO, EnterpriseSG GIA SF (оператор Plug and Play; 25.05.2026 корридор с Google Cloud), Nordic Innovation House (470 Ramona St, Palo Alto), Japan Innovation Campus (214 Homer Ave, Palo Alto), French Tech SF, Alchemist (Ravi Belani / Ian Bergman).

## Позиционирование (консенсус + наши правила)

- Продаём **«я уже веду комнату, в которую вы нанимаете»**: hired COO/CTO-операционщик, программист Python/C++, PhD по обучению разработчиков, лаборатория в Palo Alto с 2023, 100+ практиков, ужины и рабочие сессии, evals на GitHub. Слово «founder» ⛔ (never-call-anton-a-founder).
- Не просим GM/US CEO. Просим титул, который у них написан: Venture Ecosystem Lead, Director DevRel, Partner Manager, Events Lead, Silicon Ecosystem Partnerships Director.
- Доказательство = дата и формат, не прилагательные: «ужин 18–24 инженера, их инженер 25 минут без слайдов, список гостей согласован письменно» (Grok). Proof-артефакт к письму = one-page план под ИХ продукт, не cover letter (ChatGPT).
- ⚠️ ChatGPT: «no sponsorship needed» не писать как юридическое заключение; фраза «O-1 status active; based in Palo Alto». В анкетах поле «нужна ли виза» остаётся No — это факт, не заключение.
- Каденс 30 дней: день 0 apply + письмо; день 2–3 артефакт; день 5–7 один тёплый мост (KOTRA/JETRO/инвестор); день 9–12 приглашение на существующий ужин; день 21 один бамп с новым фактом; день 30 стоп (совпало у Grok и ChatGPT; согласуется с action-over-inaction-no-ping-blocks и правилом «второй бамп без новой ценности нежелателен»).

## Что меняется в кампании сегодня (applied)

1. **Волна 2 подач sv-landing** (все чистые entity): FuriosaAI Silicon Ecosystem Partnerships Director (Ashby) · n8n Sr Partner Manager SI West Coast (Ashby) · DeepL Field & Partner Marketing SF (Ashby) · Nscale Director DevRel SF (ATS искать) · Nebius Venture Ecosystem Lead + Director of Product, Ecosystem (Greenhouse, хаб) · ElevenLabs Startup Partnerships + DevRel Engineer + Community Growth (Ashby, хаб). Lovable и Mistral — капы, только письма.
2. Письма (по каналу, который есть): Kasey Roh (Upstage), Devendra Chaplot (Sarvam), Mona Li (Nebius), Janiewicz (Mistral), Choy (Rebellions). LinkedIn забанен, email не угадываем → письма уходят в те двери, где есть реальный адрес/форма; остальное = TODO с пометкой «канала нет».
3. CV-кластер `sv-landing`: headline по консенсусу «Silicon Valley AI Ecosystem & Market-Entry Operator | COO/CTO track | Python & C++ | PhD developer education | Palo Alto AI practitioner community 500+».
4. Китайские лабы: 30-дневный hold (кроме уже поданной Alibaba, решает Антон).

## Открыто / не проверено

- Gemini и GLM не собраны (добор суточным sweep).
- Живость реквизитов на 23.09 на первичных ATS не перепроверена рельсами (Nscale, Mistral DevRel, Nebius VEL видны на индексах/агрегаторах).
- Именных прецедентов провала «Head of US Ecosystem» не нашла ни одна рельса — это отсутствие данных, не отсутствие провалов.
- Цифры о комьюнити Антона (500+) во всех четырёх отчётах взяты из брифа, не проверялись.
