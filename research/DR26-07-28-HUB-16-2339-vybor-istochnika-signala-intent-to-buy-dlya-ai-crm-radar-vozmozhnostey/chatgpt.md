# DR26-07-28-HUB-16-2339 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Deep research сигналов для AI-CRM"
type: deep-research-original
source: chatgpt

dr_id: DR26-07-28-HUB-16-2339
conversation_id: 6a4c134d-3630-83eb-8c64-822899d8ca01
url: "https://приватный чат"
research_started_at: 2026-07-06T20:44:13.808572Z
research_stopped_at: 2026-07-06T20:55:57.520663Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-07-28T23:39:05
vault_chat_note: "2026-07-06-deep-research-signalov-dlya-ai-crm-6a4c134d.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
DEEP RESEARCH — выбор источника сигнала "intent-to-buy" для AI-CRM «Радар возможностей»



КОНТЕКСТ:

- Продукт: CRM превращается из журнала звонков в движок дилфлоу. Под каждый

  активный лид нужен сигнал "почему звоним именно сейчас" (вакансия / свежий

  раунд / смена топа / анонс), затем тёплый интро-путь и авто-черновик захода.

- База лидов: ~7400 активных, ПРЕИМУЩЕСТВЕННО крипто/Web3 (фаундеры токен-проектов,

  DeFi, инфра, крипто-инвесторы/VC). Не традиционный US-SaaS.

- Стек прототипа: Python (stdlib), вывод в локальный HTML-дашборд. Бюджет ~$0,

  предпочтение free-tier / дешёвых API. Сопоставление по названию компании (org).



ЗАДАЧА: сравни источники сигнала и дай ОДИН рекомендованный для v1 + резерв.

Кандидаты (дополни своими):

  A) Adzuna API (вакансии, free 250 req/day)

  B) SEC EDGAR Form D (раунды США)

  C) Крипто-раунды: CryptoRank, RootData, DefiLlama Raises, Messari (сравни по API/цене/покрытию)

  D) Крипто-джоб-борды: Web3.career, CryptoJobsList (есть ли API/feed)

  E) X/Twitter анонсы раундов (через API/листы — реалистично ли дёшево)



КРИТЕРИИ СРАВНЕНИЯ (таблица):

  1. Покрытие именно КРИПТО/Web3-компаний (главное).

  2. Свежесть сигнала (как быстро после события появляется).

  3. Стоимость / наличие free-tier / лимиты.

  4. Качество сопоставления по названию компании (есть ли тикер/домен/aliases).

  5. Легальность скрейпа / есть официальный API.

  6. Уровень шума (ложные срабатывания).



ФОРМАТ ОТВЕТА:

  - Сравнительная таблица по 6 критериям.

  - ТОП-1 рекомендация для v1 (крипто-база, бюджет ~$0) + почему.

  - Резервный источник №2.

  - Конкретный endpoint/пример запроса для рекомендованного.

  - Главные риски/подводные камни внедрения.@Deep research
```

## Отчёт
# Выбор сигнального источника intent-to-buy для AI-CRM Радар возможностей

## Короткий вывод

Если держаться ваших реальных ограничений — **крипто/Web3-база, бюджет около нуля, Python stdlib, локальный HTML, желателен официальный и юридически чистый источник** — то для **v1** я бы выбрал **Web3.career Jobs API** как основной источник сигнала. Это не самый “сильный” коммерческий триггер по смыслу, но это лучший компромисс между **покрытием именно Web3, нулевой стоимостью, наличием официального API, свежестью и внедряемостью**. У Web3.career есть официальный API-лендинг с заявленным **free access to 100,175 Web3 job listings via a single API endpoint**, поддержкой фильтров и отдельным **RSS/XML endpoint**, а аутентификация идёт простым `token` query parameter, что хорошо ложится на stdlib-пайплайн. citeturn4view0turn16search0turn17search1turn29search0

Если смотреть не на “лучший за $0”, а на **лучший по качеству самого сигнала “почему звоним сейчас”**, то выиграл бы **DefiLlama Raises**: это крипто-нативный датасет раундов, на сайте сейчас видно **7101 funding rounds**, а методология у них ручная — новые раунды команда добавляет и верифицирует вручную, плюс собирает данные из анонсов, новостей, страниц фондов и пользовательских сабмитов. Но у этого источника есть два жёстких минуса для вашего v1: **официальный API для raises — pro-only за $300/мес**, а **скрейп публичного сайта для коммерческого использования прямо запрещён условиями**. Поэтому для v1 это не основной источник, а **резерв №2**. citeturn9view0turn24view0turn8search0turn36search1turn37view0turn37view2

## Сравнительная таблица по критериям

| Источник | Покрытие именно crypto/Web3 | Свежесть сигнала | Стоимость / лимиты | Качество матчинга по названию org | API / легальность | Уровень шума |
|---|---|---|---|---|---|---|
| **Web3.career Jobs API** | **Высокое**: API заявляет доступ к **100,175 Web3 listings** и поиск по сотням blockchain/crypto компаний. citeturn4view0turn29search0 | **Высокая**: сигнал появляется в момент публикации вакансии работодателем; для job-signal это один из самых быстрых и дешёвых триггеров. Источник регулярно обновляется, страницы вакансий датированы июлем 2026. citeturn4view0turn2search13turn29search4 | **Отлично для v1**: официальный лэндинг прямо говорит о **free access**; отдельный RSS/XML endpoint тоже есть. citeturn4view0turn17search1 | **Среднее**: source выглядит в первую очередь name-based; в индексируемых сниппетах нет явного обещания ticker/domain/aliases, поэтому придётся строить нормализацию названий самому. citeturn4view0turn16search2 | **Сильная сторона**: официальный API, токен в query param, отдельный docs site. Это самый “чистый” бесплатный вариант из крипто-специализированных. citeturn16search0turn29search0 | **Средний–высокий**: hiring — это хороший, но не идеальный proxy для “buy now”; без фильтрации по ролям будет много ложных срабатываний. Это уже аналитический риск, не проблема источника. |
| **DefiLlama Raises** | **Высокое**: крипто-нативный coverage, на публичной странице видно **7101 rounds**; есть investor/project pages, категории и данные по lead investor/valuation/chains. citeturn9view0turn32search2turn33view0 | **Средняя–высокая**: команда пишет, что новые raises добавляются вручную по мере обнаружения в X, новостях, портфелях фондов и пользовательских сабмитах; но дата в базе — это **date of announcement**, а не обязательно дата закрытия сделки, и лаг иногда может доходить до года. citeturn24view0 | **Плохо для v1**: официальный raises endpoint есть, но он **pro-only**, а Pro API стоит **$300/мес**. citeturn8search0turn4view4 | **Среднее**: данные курированы и нормализованы по крипто-проектам, но в публично индексируемой документации не видно отдельного официального alias/search endpoint именно для raises. citeturn9view0turn24view0 | **Слабое место**: официальный API есть, но платный; при этом Terms запрещают программный доступ кроме official public API, а также scraping/robots для commercial use без согласия. citeturn36search1turn37view0turn37view2 | **Низкий**: “свежий раунд” — один из самых сильных и наименее шумных GTM-сигналов для аутрича. |
| **RootData Funding API** | **Очень высокое**: RootData заявляет **19,000+ crypto projects**, **18,000 people**, **8,600 funding rounds**, плюс ежедневные manual updates. citeturn5view2 | **Высокая**: сами заявляют “real-time updates” и 1,000+ datapoints updated daily. citeturn5view2 | **Плохо для v1**: pricing — **Basic free**, но fundraising endpoint доступен только для **Plus/Pro**, а Plus стоит **$128/мес**, Pro — **$328/мес**; к тому же round data consumes **2 credits per piece**. citeturn21search1turn22search0 | **Сильное**: в официальной документации есть **Search for Project/VC/People**, ID list и project endpoints — это намного лучше чисто названий из job boards. citeturn38search0 | **Официальный API**, юридически чище скрейпа, но для v1 слишком дорогой относительно вашего бюджета. citeturn21search0turn21search1turn22search0 | **Низкий**: funding-events — низкошумный сигнал. |
| **CryptoRank Funding API** | **Очень высокое**: официальный маркетинг обещает **10K+ funding rounds** и **11K+ funds & investors**. citeturn10view1 | **Высокая**: есть отдельный latest funding rounds feed и detail endpoint со source links, investors и category. citeturn20search1turn20search2 | **Плохо для v1**: sandbox бесплатный, но всего **400 credits/day**; funding feed доступен с **Pro**, detail по одному раунду — с **Business**; цены **$399/мес** и **$699/мес**. citeturn10view0turn20search1turn20search2 | **Сильное**: есть server-side currency search по **name or ticker** → `id/slug`, что очень помогает с entity resolution. citeturn20search4 | **Официальный API** отличный, но ценник для вашего v1 неадекватный. citeturn20search3turn10view0 | **Низкий**: round feed сам по себе хороший low-noise trigger. |
| **Messari Fundraising API** | **Очень высокое**: Messari пишет про **14k+ rounds**, **15k+ investors**, **1k+ funds**, а Fundraising API покрывает rounds, investors, orgs, projects, funds и M&A. citeturn39search14turn39search1 | **Высокая**: продукт позиционируется как institutional-grade fundraising/events monitoring. citeturn23search1turn39search8 | **Плохо для v1**: fundraising data относится к Enterprise/add-on доступу; pricing page и API tiers ведут в sales/contact flow. citeturn5view1turn23search3 | **Сильное**: есть Organizations и Projects endpoints, а Organizations response содержит structured links. citeturn39search1turn39search4 | **Официальный API**, но для v1 бюджетно не подходит. citeturn23search3turn39search0 | **Низкий**: funding как сигнал силён, но источник избыточен по бюджету. |
| **SEC EDGAR Form D** | **Низкое для вашей базы**: SEC Form D — это structured data по exempt offerings под Rule 504/506 и Section 4(a)(5), то есть inherently US-regulatory slice, а не crypto-native dataset. Для mostly Web3/crypto базы это будет узким покрытием. citeturn14view0 | **Двояко**: EDGAR APIs в целом обновляются почти real-time, но **Form D Data Sets** как удобный structured dataset публикуются **quarterly**. То есть либо получаете хороший SLA и высокий парсинг-оверхед, либо удобный формат и плохую свежесть. citeturn4view5turn14view0 | **Бесплатно**, официально. citeturn4view5turn14view0 | **Среднее**: у SEC есть filer history / CIK world, но для crypto/Web3 сущностей матчингу всё равно часто не хватит доменов/алиасов и многие юниты будут вне SEC-потока. Это вывод из scope источника. citeturn4view5turn14view0 | **Сильная сторона** — полностью официальный источник. citeturn4view5turn14view0 | **Низкий–средний**: сам факт filing’а силён, но часть записей будет плохо конвертироваться в “звонить прямо сейчас” для Web3 vendor sales. |
| **Adzuna API** | **Низкое**: это general jobs API, а не crypto-native источник. citeturn4view7 | **Высокая** для вакансий. citeturn13search0 | **Плохо для масштаба и коммерции**: бесплатные лимиты **25/min, 250/day, 1000/week, 2500/month**; moreover, any other commercial/academic/government use beyond publishing/personal research описан как **14-day trial**. citeturn5view0 | **Слабое**: в примере ответа есть только `company.display_name`, без явных aliases/domain. citeturn13search0 | **Официальный API есть**, но licensing fit для вашего внутреннего CRM выглядит плохим. citeturn5view0 | **Средний–высокий**: generic hiring signal + слабое crypto coverage. |
| **CryptoJobsList** | **Среднее**: сайт явно crypto/Web3-специализированный и остаётся активным job board. citeturn5view3turn11search16 | **Высокая** по самим вакансиям. citeturn11search16turn2search4 | **Неясно для API-использования**: я не нашёл у них публичной official API page; официальный сайт/terms акцентируют jobs, newsletter и Telegram/Discord bots. citeturn11search0turn5view6 | **Среднее**: компания обычно указана, но без явной docs-истории про alias/entity graph. citeturn5view3turn11search16 | **Слабее Web3.career**: как источник вакансий сайт релевантен, но как системный v1 data-source хуже из-за отсутствия найденного public API. citeturn11search0turn5view6 | **Средний–высокий**: те же проблемы, что у job signal, плюс более слабая интеграционная база. |
| **X API / lists / search** | **Потенциально очень высокое**: именно там часто впервые появляются round/team/product анонсы. citeturn24view0turn7view2 | **Лучшая свежесть**: filtered stream даёт near-real-time delivery, P99 latency около **6–7 секунд**. citeturn7view2 | **Плохо для v1**: поиск recent — только **last 7 days**; full archive — только для pay-per-use / Enterprise; filtered stream на pay-per-use имеет **1,000 rules per project**, что мало против ваших ~7,400 orgs, а reads тарифицируются по ресурсу. citeturn7view0turn7view2turn4view6 | **Может быть сильным**, если у вас уже есть точные official handles/aliases; без этого матчинг и recall будут страдать. Это inference из механики rules/search. citeturn7view0turn7view2 | **Официальный API есть**, но для дешёвого системного мониторинга на 7.4k компаний он слишком неудобен, а policy/rate/cost envelope хрупкий. citeturn4view6turn5view5turn7view2 | **Очень высокий**: без жёсткого whitelisting official accounts вы утонете в слухах, репостах и нерелевантных постах. Это аналитический вывод из природы public conversation API. |

## Рекомендация для v1

**ТОП-1 для v1: Web3.career Jobs API.** Не потому, что “вакансия” сильнее “раунда”, а потому что на ваших ограничениях это **лучший первый кирпич**, который реально можно поставить в прод без грязных допущений. У него одновременно есть: крипто/Web3-фокус, официальный API, нулевая цена, простая аутентификация, пригодность для stdlib и отсутствие явного legal-red-flag уровня “скрейп запрещён”. У конкурентов с более сильным сигналом почти всегда ломается хотя бы один из этих пунктов: либо цена, либо licence, либо операционная сложность. citeturn4view0turn16search0turn17search1turn29search0turn8search0turn37view2

Практически это означает следующее. Для **“Радара возможностей” v1** я бы не пытался сразу ловить “всё”: не aнонсы, не X, не EDGAR, не новостные фиды. Я бы сначала забрал **один дешёвый и устойчивый поток** и превратил его в работающий sales trigger: *“эта Web3-компания сейчас нанимает → значит, у неё есть бюджет, команда в движении, расширение функций или go-to-market сдвиг, и у звонка есть why-now.”* Это особенно разумно, если дальше вы всё равно собираетесь генерировать warm-intro paths и авто-черновики захода. citeturn4view0turn29search0

Ключевой caveat: **job signal шумнее funding signal**. Поэтому его нельзя пускать в CRM как raw feed. Для v1 его нужно жёстко фильтровать на стороне продукта: оставлять только роли, которые статистически лучше коррелируют с вашим целевым аутричем — go-to-market, partnerships, enterprise, institutional, compliance, integrations, solutions engineering, API/devrel и похожие функции. Это уже не требование источника, а требование к вашей логике ранжирования.

## Резервный источник

**Резерв №2: DefiLlama Raises.** Это мой выбор в качестве **лучшего по смыслу** сигнала, который пока проигрывает только из-за вашего бюджетно-юридического контура. У DefiLlama крипто-нативная raises-база, ручная верификация, видимые поля по round type, investor set, category, description, valuation и chain context. Для sales motion это обычно сильнее вакансий: свежий раунд лучше объясняет, почему у компании **сейчас** появились деньги, план расширения, новые задачи, внешние подрядчики и appetite to buy. citeturn9view0turn24view0

Но для вашего v1 это **резерв, а не main** по двум причинам. Первая: официальный raises endpoint сидит в pro-tier, где DefiLlama сама пишет, что **Pro API = $300/mo**. Вторая: их Terms of Use отдельно запрещают **programmatically access any API other than official public API**, а также **copy, scrape, harvest** данные для commercial use и использовать **robots/spiders** для доступа к Site. То есть путь “ну мы тихо спарсим public /raises” для CRM-продукта — плохая идея и юридически, и операционно. citeturn8search0turn4view4turn36search1turn37view0turn37view2

Если позже бюджет вырастет хотя бы до **$128–300/мес**, landscape меняется. Тогда уже есть смысл рассматривать **RootData** как сильного кандидата из-за entity resolution и search graph, а **DefiLlama Raises** — как очень качественный crypto-native funding trigger. На нулевом бюджете этого преимущества вы не монетизируете. citeturn21search1turn22search0turn5view2turn8search0

## Конкретный endpoint и пример запроса

Для **рекомендованного источника Web3.career** официально задокументированы три вещи, которых уже достаточно для v1-скелета:

- есть **single API endpoint**;
- для всех запросов нужен **`token` query parameter**;
- есть официальный **RSS/XML endpoint `/v1.xml`** с теми же параметрами, что и основной endpoint. citeturn4view0turn16search0turn17search1

Поскольку индексируемый сниппет docs не показывает базовый host API, безопаснее использовать следующий **документированный skeleton**, а точный base URL взять из docs в момент интеграции:

```http
GET <WEB3_CAREER_API_BASE>/v1.xml?token=YOUR_TOKEN
```

Это хороший вариант именно под ваш стек, потому что `xml.etree.ElementTree` есть в stdlib, а дальше вы уже делаете свои фильтры, дедупликацию и матчинг по `org` в локальном Python-пайплайне. Сам API-лендинг также говорит, что можно фильтровать listings по technology / role type / location и управлять размером ответа. citeturn4view0turn17search1

Для **резервного источника DefiLlama Raises** документированный официальный endpoint уже виден явно в API docs:

```http
GET https://pro-api.llama.fi/{YOUR_API_KEY}/api/raises
```

Но это уже **paid path**, а не бесплатный v1-path. citeturn8search0turn4view4

Отдельный технический нюанс по Web3.career: в их docs есть нестандартность ответа — **top-level JSON array mixed types**, а реальный jobs array лежит обычно на **index 2**. Если брать JSON-ветку, это надо учитывать сразу, иначе вы словите лишнюю хрупкость в парсере. citeturn16search2

## Главные риски и подводные камни внедрения

Главный риск у **Web3.career** — не доступ, а **смысловая шумность**. Если вы просто подключите весь hiring feed, то получите кучу слабых или нерелевантных поводов: стажёры, однотипные engineering-hiring bursts, старые reposts, роли без отношения к вашему offer. Это надо лечить не сменой источника, а scoring-слоем поверх источника: возраст вакансии, seniority, функция, наличие enterprise/API/compliance/institutional словаря, число параллельных вакансий на компанию, burst за 30 дней, и только потом показ в CRM. Источник даёт сырьё; качество сделки делает ваша ранжирующая логика. citeturn4view0turn29search0

Второй риск — **entity resolution по одному названию компании**. Это особенно болезненно в Web3, где одна и та же сущность может фигурировать как `Foundation`, `Labs`, `Protocol`, `DAO`, `Network`, `xyz`, бренд токена и юрлицо одновременно. Платные crypto-data providers выигрывают именно здесь: у RootData есть search для Project/VC/People и ID list, у CryptoRank — currency search по name/ticker → `id/slug`, у Messari — structured organizations/projects/funds. Бесплатный jobs feed даст вам coverage, но не даст готовой entity graph. Значит, alias table и нормализация названий — обязательны. citeturn38search0turn20search4turn39search1turn39search4

Третий риск — соблазн “усилить” v1 дешёвым X-мониторингом. На бумаге X очень красив: Recent Search доступен всем разработчикам, Filtered Stream даёт near-real-time posts с P99 latency 6–7 секунд, full archive есть у pay-per-use/Enterprise. На практике для вашей базы это плохой первый источник: **recent search видит только 7 дней**, а **filtered stream в pay-per-use даёт 1,000 rules per project**, что плохо масштабируется на ~7,400 активных лидов. Плюс read-cost model делает постоянный мониторинг непредсказуемым по затратам. X как enrichment layer — да; X как v1 source of truth — нет. citeturn7view0turn7view2turn4view6

Четвёртый риск — неверная ставка на **DefiLlama public scrape**. Даже если public pages визуально выглядят пригодными, их Terms прямо запрещают scraping/robots для commercial use и программный доступ вне official public API. Для внутреннего CRM-движка это уже не “серый хак”, а лишний юридический хвост. Если вы хотите использовать DefiLlama как production source, нормальный путь только один: официальный API и соответствующий бюджет, либо письменное разрешение. citeturn36search1turn37view0turn37view2

Пятый риск — недооценить ограничения альтернатив. **Adzuna** выглядит дешёво, но free limits малы, а terms для “other commercial use” выглядят как 14-day trial, то есть это не опора для вашего постоянного внутреннего продуктового контура. **SEC Form D** юридически чист и бесплатен, но структурный dataset quarterly, а real-time путь требует отдельного EDGAR-парсинга. Оба источника годятся как вспомогательные later-stage enrichments, но не как лучший первый крипто/Web3 signal feed. citeturn5view0turn4view5turn14view0

Итог в одном предложении: **для v1 берите Web3.career Jobs API как единственный основной сигнал**, а **DefiLlama Raises держите как первый апгрейд, когда появится бюджет или официальное разрешение на доступ к raises-data**. Это самый прагматичный, а не самый красивый выбор.
