# DR26-11-12-HUB-01-0234 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

DR26-11-12-HUB-01-0234: коннекторы под дыры флота

Дата замера: 13 октября - 12 ноября 2026 (10 000 сессий, хаб H76340). Дата отчёта: 12 ноября 2026. Все цены и статусы проверены по источникам, датированным 2025-2026 годом; там, где проверка невозможна, стоит «unknown».

Вопрос

Для пяти «дыр» флота (Luma, Facebook+Messenger, X, YC Work at a Startup, WhatsApp) определить: (A) покрывает ли официальное API или официальный MCP-сервер реальную задачу агента, (B) точную цену, (C) риск нарушения ToS при неофициальном маршруте, (D) лучшие сторонние MCP/SDK, (E) рекомендацию: официальный путь / сторонний / оставить браузер / отказаться.

Резюме

X: главный сюрприз в вашу пользу. С 30 июня 2026 у X есть официальный hosted MCP-сервер (api.x.com/mcp), а API переведён на pay-per-use: $0.015 за пост ($0.20 с URL), $0.005 за чтение чужого поста, $0.001 за свои упоминания. Ваш сценарий (5 постов, 50 ответов, 500 чтений в день) стоит примерно $40-130 в месяц. Ваша 34-часовая остановка на HTTP 402 это просто исчерпанные кредиты, лечится auto-recharge. Критический нюанс: с февраля 2026 X блокирует «холодные» программные ответы (403), ответ возможен только если автор оригинала упомянул вас или процитировал вас.

Luma: официальный MCP есть и бесплатен (mcp.luma.com, любой аккаунт Luma, Luma Plus не нужен). Он закрывает discovery мероприятий и управление своими событиями, но не умеет регистрировать вас как участника чужого события. Итог: discovery через официальный MCP, финальный клик RSVP остаётся в браузере.

Facebook + Messenger: официального пути для личного профиля нет. Публикацию в личные ленты Meta закрыла ещё в 2018, личные аккаунты невидимы для Messenger API, а ToS с 1 января 2025 прямо запрещают автоматизированный сбор «в том числе в залогиненном состоянии». Meta банит автоматизацию в течение минут. Вариант-обходка: перевести активности на Page, тогда Graph API легален.

WhatsApp: критический риск, мост надо убирать. Официальный Cloud API работает только с бизнес-номерами, личного пути нет, а новые условия (в силе с 15 января 2026) прямо запрещают универсальные AI-ассистенты. С января 2026 Meta автоматизированно банит Baileys/QR-мосты; в марте 2026 фиксировались волны банов 40-60% номеров за 48 часов. Риск потерять личный номер навсегда.

YC WaaS: официального API нет, ToS запрещает скрейпинг, объём мал (37 навигаций). Оставить браузер для откликов, discovery отдать стороннему MCP workatastartup-mcp.

Методология

По каждой из 5 платформ выполнено: (1) поиск официальной документации API/MCP с открытием первичных страниц (docs.x.com, help.luma.com, developers.facebook.com), (2) проверка тарифов 2026 года, (3) поиск кейсов enforcement 2025-2026, (4) обзор сторонних MCP-серверов на GitHub со звёздами и датами последних коммитов. Ограничения: цены X API менялись трижды за год (февраль, апрель, август-сентябрь 2026), любые цифры стоит перепроверить в Developer Console; данные о волнах банов WhatsApp опираются на отчёты сообществ и вендоров, а не на статистику Meta; для ряда репозиториев число звёзд не удалось подтвердить. Внутренние данные замера (навигации/сессии) взяты из постановки задачи и не проверялись.

1. Luma (lu.ma): 204 навигации / 66 сессий

A. Официальный вариант. Есть и API, и официальный MCP-сервер, но они «организаторские». REST API (docs.luma.com) создаёт события, рассылает приглашения, управляет гость-листами, регистрации и memberships (Luma Help: Luma API). Ключ выдаётся на календарь, лимит 200 запросов/мин, требуется активная подписка Luma Plus на этом календаре (Luma Help: Luma API); её цену официально справка не называет, сторонние обзоры дают $59/мес при годовой оплате (api-evangelist/luma, ZoomInfo review, август 2026). Официальный MCP живёт на https://mcp.luma.com, подключается бесплатно с любым аккаунтом Luma, без Luma Plus (Luma Help: Luma MCP Server). Он умеет: искать публичные события по ключу, дате и географии, показывать «ваши события» (посещаете/хостите) и полностью управлять событиями, которые вы хостите (Luma Help: Luma MCP Server). Чего нет ни в API, ни в MCP: зарегистрироваться участником на чужом событии. Инструментов RSVP attendee в списке нет. Итог: discovery покрыт официально, сама регистрация участника не покрыта.

B. Цена. Официальный MCP: бесплатно (Luma Help: Luma MCP Server). REST API: включён в Luma Plus, по сторонним данным $59/мес (api-evangelist/luma); для вашей задачи (вы не организатор) он не нужен.

C. Риск ToS. Unknown. Публичных кейсов enforcement против браузерной автоматизации Luma в 2025-2026 не найдено. Объём мал, риск оценивается как низкий, но документально не подтверждён.

D. Лучшие сторонние. Все вторичны относительно официального: adelaidasofia/luma-mcp (14 инструментов, draft+confirm, аудит-лог; звёзды: unknown), montaguegabe/luma-events-mcp (управление событиями, звёзды: unknown), MegaGimen/lumaMCP для discovery (Glama). Дата последнего коммита каждого: unknown.

E. Рекомендация. Официальный MCP для discovery + браузер только для финального RSVP. Решающий факт: официальный MCP бесплатен и закрывает поиск, но инструмента «зарегистрироваться участником» не существует.

2. Facebook + Messenger (личный профиль): 138 / 59

A. Официальный вариант: нет. Для личного профиля официального API не существует. Публикация в личные ленты удалена в 2018 (Outstand: Facebook API Guide 2026), «поддерживаемого обходного пути нет» (Bundle.social, 2026). Личные профили официально закрыты для сторонних данных (SocialCrawl, 2026). Messenger API работает только со страницами (Page) и бизнес/creator-аккаунтами Instagram: «личные аккаунты невидимы для API» (dev.to, guide to Meta's Messenger API, Replient, 2026). Требуемый тип аккаунта для официального маршрута: страница Facebook (Page) с Page access token, либо Instagram professional account; тогда комментарии и DM читаются/пишутся легально через Graph API (Outstand). Для личного профиля: official option отсутствует, unknown, когда и если появится.

B. Цена. Не применима: официального маршрута для личного профиля нет. Граф API для Page формально бесплатен, но требует прохождения App Review (SocialCrawl).

C. Риск ToS: высокий. Обновлённые Meta Terms (в силе с 1 января 2025) запрещают автоматизированный доступ и сбор «независимо от того, залогинены вы или нет» (FacebookScraperAPI, 2026). Enforcement 2025-2026: операторы с «прогретыми» аккаунтами 3-5 лет, резидентными прокси и антидетект-браузерами сообщали о банах через минуты после автоматических действий (AdMakeAI, 2025); массовые волны банов с непрозрачной процедурой апелляции задокументированы Наблюдательным советом Meta (TechCrunch, 4 июня 2026); Meta годами судится со скрейперами (TechCrunch, 25 августа 2026). Цена ошибки: личный профиль человека, то есть самый незаменимый аккаунт флота.

D. Лучшие сторонние. Все неофициальные, все на cookie/bridge-архитектуре: YoTro/facebook-mcp (посты, поиск, Messenger-сообщения; последний коммит 24 октября 2026; звёзды: unknown), fukata/messenger-mcp (личный Messenger через fbchat-v2, включая E2EE-чаты; звёзды: unknown), matpb/meta-mcp-rs (личный Messenger + WhatsApp через неофициальные сайдкары, с аудит-логом; звёзды: unknown), stuV2/messenger-mcp (мост mautrix-meta; звёзды: unknown). Все прямо нарушают ToS Meta.

E. Рекомендация. Оставить браузер с жёстким rate-limiting и человеческим темпом, либо перевести активность на страницу (Page) и уйти в легальный Graph API. Решающий факт: официального API для личного профиля нет и не предвидится, а штраф за поимку это сам профиль.

3. X (x.com): 99/40 браузер + официальный рейл на 49 постов и 402

A. Официальный вариант: есть, и это лучший исход дела. С 30 июня 2026 X предоставляет официальный hosted MCP на https://api.x.com/mcp: 200+ инструментов, авто-генерация из OpenAPI-спеки X API, авторизация под вашим же аккаунтом (TechCrunch, 30 июня 2026, анонс X Developers, гайд OpenTweet). Он покрывает: постинг, поиск (включая полный архив), чтение таймлайнов и упоминаний, тренды, закладки, статьи (MCP.Directory, 2026). Новых возможностей MCP не добавляет, это обёртка X API (TechCrunch). С 6 февраля 2026 API работает только по модели pay-per-use, free-тир закрыт, Basic/Pro недоступны новым разработчикам (Postproxy, февраль 2026); подписка Pro официально депрекирована 14 августа 2026 с миграцией после 1 сентября 2026 (Blotato). Критическое ограничение для вашей задачи: с февраля 2026 X блокирует программные ответы на self-serve-тарифах: reply проходит только если автор оригинала упомянул вас или процитировал вас (Infatoshi/x-mcp, README); 1 марта 2026 подтверждено, что и фолбэк через цитирование ловит тот же 403 (JohannesHoppe/x-autonomous-mcp, README). Постинг, чтение упоминаний и поиск покрываются полностью; холодные ответы нет.

B. Цена (по официальному прайсу docs.x.com, страница обновлена 16 октября 2026). Pay-per-use, кредиты покупаются заранее: Post: Create $0.015 за запрос, Post: Create с URL $0.20; чтение поста $0.005 за ресурс; профиль $0.010; Owned Reads (свои посты, упоминания, закладки, лайки, подписки) $0.001 за ресурс; Counts Recent $0.005/запрос, Counts All $0.010; Trends $0.010. Дедупликация повторных чтений внутри суток UTC. Потолок 3 млн чтений постов в месяц, выше только Enterprise. Бонусы: $20 кредитов за первую привязку карты + матч первого auto-recharge до $50, до 20% кэшбэка кредитами xAI. Расчёт для 5 постов + 50 ответов + 500 чтений в день (30 дней):

Посты: 150 шт., $2.25 без ссылок, до $30 если каждый с URL;

Ответы: 1500 шт. × $0.015 = $22.50 (те, что X пропустит; см. блокировку холодных ответов выше);

Чтения: 15 000 в месяц, $15 если все свои (Owned Reads $0.001), $75 если все чужие/поиск ($0.005);

Итого реалистично: $40-130 в месяц, доминируют чтения и посты с URL.
Ваш HTTP 402 «credits depleted» это штатное поведение pay-per-use: при нулевом балансе запросы блокируются до пополнения (docs.x.com); авто-recharge (например, пополнение $25 при пороге $5) предотвращает повторение 34-часовой остановки.

C. Риск ToS браузерного маршрута: высокий. ToS X запрещают «crawling or scraping в любой форме без письменного согласия» (Weirapp, октябрь 2026); гайдлайны разработчика прямо предупреждают, что автоматизация вне API (браузерные скрипты, скрейпинг) ведёт к permanent suspension (First100, сентябрь 2026, OpenTweet, 2026). Кейсы 2025-2026: 24 августа 2026 X отправил cease-and-desist проекту Nitter за «незаконное использование и обход API X», инстансы легли, репозиторий архивирован (TechCrunch, 25 августа 2026, Dealroom); иск X к Bright Data был отклонён в мае 2024 и закрыт мировым в июне 2025 (Wikipedia: Bright Data). Автоматизированный follow/unfollow, массовые действия и обход блокировок баниются (OpenTweet).

D. Лучшие сторонние. EnesCinr/twitter-mcp: 407 звёзд на 27 сентября 2026, самый популярный открытый сервер, последний коммит 3 ноября 2026 (First100); Infatoshi/x-mcp (API-based, звёзды: unknown) ценен документацией ограничений февраля 2026. С появлением XMCP все они теряют смысл.

E. Рекомендация. Официальный XMCP + pay-per-use с auto-recharge, ~$40-130/мес. Решающий факт: с июня 2026 существует официальный hosted MCP, а браузерный постинг на X грозит permanent suspension, так что браузерные 102 попытки нужно разпарковать в API, а ответы строить только на упоминания и цитирования (холодные ответы API блокирует).

4. YC Work at a Startup: 37 / 12

A. Официальный вариант: нет. Публичного API или MCP у workatastartup.com не найдено; отклик идёт через авторизацию account.ycombinator.com (Apify-скрейпер показывает apply_url). Условия YC прямо запрещают «data mining, robots, scraping or similar data gathering and extraction methods» (разбор в issue проекта joule, триаж 29 октября 2026); robots.txt при этом ничего не запрещает, но ToS имеет приоритет (там же). Итог: официального маршрута для откликов и переписки с фаундерами нет, unknown, появится ли.

B. Цена. Не применима: API нет. Все существующие инструменты бесплатные краулинговые.

C. Риск ToS: средний. Скрейпинг запрещён условиями (joule issue), но публичных кейсов enforcement 2025-2026 против отдельных пользователей или лёгких скрейперов не найдено. Объём флота мал, риск де-факто низкий, юридически ненулевой.

D. Лучшие сторонние. MIt9/workatastartup-mcp: поиск вакансий и компаний, фильтры по зарплате, equity, визам; релиз 22 августа 2026, последний коммит 24 сентября 2026, звёзды: unknown; публикуется на PyPI (workatastartup-mcp). Сторонний скрейпер jwc20/waasuapi умеет логиниться и тянуть компании/вакансии. MCP со стороны фаундеров (пайплайн кандидатов, 17 звёзд, для компаний, не кандидатов): PulseMCP: yc-software/waas. Ни один не умеет подавать отклик кандидатом.

E. Рекомендация. Оставить браузер для откликов (объём 37 навигаций не оправдывает риск), discovery отдать MIt9/workatastartup-mcp. Решающий факт: официального API нет, отклик требует авторизованной сессии YC, а ToS запрещает скрейпинг.

5. WhatsApp (личный, неофициальный мост): ~1100 вызовов, ~18% ошибок

A. Официальный вариант для личных чатов: нет. WhatsApp Business Platform (Cloud API) работает только с бизнес-номерами; «пути к API через личный аккаунт не существует» (Zoko, 2026, Meta for Developers). Более того, обновлённые условия WhatsApp Business Platform (новые аккаунты с 15 октября 2025, все остальные с 15 января 2026) прямо запрещают universal/general-purpose AI-ассистентов и чат-ботов (Whale.co.il, 2026, Zylos Research, 26 января 2026). Требования официального маршрута: отдельный номер, бизнес-верификация Meta, шаблоны, opt-in; он по конструкции не обслуживает вашу переписку с людьми (TheAutomate, 2026).

B. Цена. Официальный Cloud API: с 1 июля 2025 оплата помодельно, за доставленное шаблонное сообщение (категории marketing/utility/authentication, цены по стране получателя); с 1 октября 2026 платными становятся и ответы внутри 24-часового окна, после бесплатных 1000 сообщений в месяц на номер (Pasquale Pillitteri, август-октябрь 2026, Whale.co.il). Для личного номера: не применима. Нынешний мост: бесплатен, но его цена это риск номера.

C. Риск ToS: критический. Сам WhatsApp предупреждает, что привязка аккаунта к неофициальному приложению «нарушает наши условия» и аккаунт «может быть временно или навсегда забанен» (цитируется в README matpb/meta-mcp-rs). Meta детектирует именно библиотеки-мосты по именам: Baileys, whatsapp-web.js, WAHA, Evolution API, Green API (Whale.co.il). Enforcement 2025-2026: с января 2026 crackdown усилился (Whale.co.il, PipesBot, 2026); в марте 2026 сообщества фиксировали баны 40-60% Baileys/QR-аккаунтов за 48 часов (CuboSuite, 2026, аноекдотально); «любая API не через официальный Cloud API приведёт к бану» (r/whatsapp). Ваш текущий мост это ровно этот класс, 18% ошибок согласуются с деградацией cat-and-mouse.

D. Лучшие сторонние. lharries/whatsapp-mcp: самый популярный (около 6.4-6.7 тыс. звёзд), но последний коммит в main 13 июля 2025, то есть проект стоит больше года (GitHub, PulseMCP); jaingxyz/whatsapp-mcp (Baileys, resident-режим, AGPL; звёзды: unknown); Evolution API (9433 звезды на 27 августа 2026, режимы Baileys или официальный Cloud API) (Pasquale Pillitteri). Все Baileys-варианты несут один и тот же риск бана.

E. Рекомендация. Отключить мост от личного номера (drop): риск необратимой потери номера несопоставим с ценой. Автоматизируемое общение вынести на отдельный номер через официальный Cloud API (или на другой канал: email/Telegram), личный WhatsApp оставить человеку. Решающий факт: Meta с января 2026 системно банит именно такие мосты, а официальный API для личных чатов не существует.

Итоговая таблица

Пункт

	

Официальный вариант

	

Цена

	

Риск ToS браузера/моста

	

Лучший неофициальный

	

Рекомендация




Luma

	

Официальный MCP mcp.luma.com (бесплатно, discovery + свои события); API organizer-side требует Luma Plus (Help MCP, Help API)

	

MCP: $0; Luma Plus ~$59/мес (сторонние данные, api-evangelist)

	

Unknown, кейсов нет

	

adelaidasofia/luma-mcp (unknown звёзд)

	

Официальный MCP + браузер только для RSVP-клика: инструмента регистрации участника нет




Facebook + Messenger

	

Нет для личного профиля; только Page/IG business через Graph API (Outstand, dev.to)

	

N/A (официального пути нет)

	

Высокий: ToS 2025 запрещает авто-даже-залогиненным, баны за минуты (FacebookScraperAPI, AdMakeAI)

	

YoTro/facebook-mcp, коммит 24.10.2026, звёзды unknown

	

Держать браузер с rate-limit или мигрировать на Page; официального API нет с 2018




X

	

Официальный API pay-per-use + hosted XMCP с 30.06.2026 (TechCrunch)

	

$0.015/пост ($0.20 с URL), $0.005/чтение, $0.001 owned; ваш профиль нагрузки $40-130/мес (docs.x.com)

	

Высокий: браузерная автоматизация = permanent suspension, Nitter C&D 08.2026 (First100, TechCrunch)

	

EnesCinr/twitter-mcp, 407 звёзд, коммит 03.11.2026

	

Официальный XMCP + auto-recharge; холодные ответы API блокирует (403), браузер не возвращать




YC WaaS

	

Нет (joule issue, 29.10.2026)

	

N/A

	

Средний: ToS запрещает скрейпинг, кейсов против пользователей нет (joule)

	

MIt9/workatastartup-mcp, коммит 24.09.2026, звёзды unknown

	

Браузер для откликов, MCP для discovery: API нет, отклик только через сессию YC




WhatsApp

	

Нет для личного номера; Cloud API только бизнес, AI-ассистенты запрещены с 15.01.2026 (Zoko, Whale)

	

N/A для личного; официально: помодельно с 01.07.2025, окно-ответы платны с 01.10.2026 (Pillitteri)

	

Критический: баны Baileys/QR волнами с 01.2026, 40-60% за 48 ч в 03.2026 (CuboSuite)

	

lharries/whatsapp-mcp, ~6.4k звёзд, но коммит 13.07.2025 (стоит)

	

Drop: снять мост с личного номера, автоматизацию вынести на отдельный номер/канал

Заметки об источниках

Источник

	

Достоверность

	

Обновлено




docs.x.com: X API pricing (открыт полностью)

	

5/5

	

16.10.2026 (dateModified страницы)




Luma Help: Luma MCP Server (открыт)

	

5/5

	

unknown




Luma Help: Luma API (открыт)

	

5/5

	

unknown




docs.luma.com: Getting Started

	

5/5

	

unknown




TechCrunch: X hosted MCP

	

5/5

	

30.06.2026




X Developers: Announcing the hosted X MCP

	

5/5

	

30.06.2026




TechCrunch: Nitter cease-and-desist

	

5/5

	

25.08.2026




TechCrunch: Oversight Board о банах Meta

	

5/5

	

04.06.2026




Meta for Developers: WhatsApp Business Platform

	

5/5

	

unknown




Postproxy: X API pricing 2026

	

4/5

	

02-05.2026




Blotato: X API pricing

	

3/5

	

09.2026




OpenTweet: XMCP guide и automation rules

	

3/5

	

09-10.2026




Infatoshi/x-mcp README (ограничение ответов с 02.2026)

	

4/5

	

2026




JohannesHoppe/x-autonomous-mcp README (403 на цитирования, 01.03.2026)

	

4/5

	

01.03.2026




First100: Twitter MCP Server (407 звёзд EnesCinr на 27.09.2026)

	

3/5

	

27.09.2026




Weirapp: Is Scraping Twitter Legal

	

3/5

	

10.2026




Outstand: Facebook API Guide 2026

	

3/5

	

2026




Bundle.social: Facebook Graph API Guide

	

3/5

	

2026




SocialCrawl: Facebook data API 2026

	

3/5

	

2026




dev.to: Guide to Meta's Messenger API

	

3/5

	

unknown




FacebookScraperAPI: Is scraping Facebook legal (ToS с 01.01.2025)

	

3/5

	

2026




AdMakeAI: автоматизация и баны

	

3/5

	

2025-2026




joule issue #113: Work at a Startup, deferred (YC ToS запрещает скрейпинг)

	

4/5

	

29.10.2026




MIt9/workatastartup-mcp (коммит 24.09.2026) и PyPI

	

4/5

	

24.09.2026




PulseMCP: yc-software/waas (17 звёзд, founder-side)

	

3/5

	

unknown




Zoko: WhatsApp API without business verification

	

3/5

	

2026




Whale.co.il: WhatsApp AI policy 2026 (запрет AI-ассистентов с 15.01.2026)

	

3/5

	

2026




Zylos Research: WhatsApp API and Automation

	

3/5

	

26.01.2026




Pasquale Pillitteri: Evolution API vs Cloud API (9433 звезды Evolution, тарифы)

	

3/5

	

27.08.2026




CuboSuite: Baileys блокируется и волны банов 2026

	

2/5

	

2026




PipesBot: Baileys ban risk

	

3/5

	

2026




lharries/whatsapp-mcp (~6.4k звёзд, коммит 13.07.2025) и PulseMCP

	

4/5

	

13.07.2025




api-evangelist/luma и ZoomInfo: Luma API Review (цена Luma Plus $59/мес)

	

3/5

	

08.2026




YoTro/facebook-mcp (коммит 24.10.2026) и fukata/messenger-mcp

	

4/5

	

24.10.2026

Конфликты и оговорки: цифры X API у сторонних обзоров расходятся в мелочах (например, $0.010 за пост у Zernio против $0.015 на docs.x.com); за истину взят официальный прайс. Волны банов WhatsApp и проценты выживаемости Baileys основаны на отчётах вендоров и сообществ, Meta их не подтверждаёт. Число звёзд ряда MCP-репозиториев (YoTro, MIt9, adelaidasofia, matpb) не удалось верифицировать: unknown.

Открытые вопросы

Появится ли у Luma инструмент «зарегистрироваться участником» в официальном MCP; сейчас это единственный незакрытый шаг.

Ослабит ли X блокировку холодных ответов (февраль 2026) для платных pay-per-use аккаунтов; формулировки X об этом не найдены.

Точная цена Luma Plus на ноябрь 2026: официально не опубликована, $59/мес это сторонние данные.

Звёзды/активность: YoTro/facebook-mcp, MIt9/workatastartup-mcp, matpb/meta-mcp-rs (звёзды unknown).

Реальная частота банов WhatsApp-мостов в Европе (данные CuboSuite о 40-60% касаются в основном Бразилии).

Рекомендации и следующие шаги

X (сделать на этой неделе): подключить официальный XMCP (api.x.com/mcp) вместо браузерного рейла, включить auto-recharge (пополнение $25, порог $5), заложить бюджет $50-150/мес. Перестроить логику ответов: только на упоминания и цитирования; холодные ответы либо через человека, либо не делать.

Luma: подключить mcp.luma.com (бесплатно) для discovery; в браузере оставить только финальный RSVP.

WhatsApp (срочно): снять неофициальный мост с личного номера. Автоматизируемую переписку вынести на отдельный номер с официальным Cloud API или на другой канал. Завести резервную экспорт-процедуру чатов, пока номер жив.

Facebook: снизить браузерный темп до человеческого, мониторить флаги «automation»; стратегически рассмотреть миграцию активностей на страницу (Page) ради легального Graph API.

YC WaaS: оставить браузер как есть, добавить MIt9/workatastartup-mcp для поиска вакансий, чтобы сессии сокращались до отклика.

Общее: через месяц перемерить долю ошибок по каждому каналу; критерий успеха: X без 402, WhatsApp без риска номера, Luma с discovery через MCP.




## Sources (links)
https://docs.luma.com/reference/getting-started-with-your-api
https://help.luma.com/p/luma-api
https://github.com/api-evangelist/luma
https://pipeline.zoominfo.com/sales/luma-api-review
https://help.luma.com/p/mcp
https://github.com/adelaidasofia/luma-mcp
https://github.com/montaguegabe/luma-events-mcp
https://glama.ai/mcp/servers/MegaGimen/lumaMCP
https://www.outstand.so/blog/facebook-api
https://bundle.social/blog/facebook-graph-api
https://www.socialcrawl.dev/blog/facebook-data-api-2026
https://dev.to/peter11241342/how-instagram-dm-automation-actually-works-a-developers-guide-to-metas-messenger-api-2i30
https://replient.ai/en/guide/dm-automation-instagram-facebook
https://facebookscraperapi.com/blog/is-scraping-facebook-legal
https://admakeai.com/blog/automate-facebook-without-getting-banned
https://techcrunch.com/2026/06/04/metas-oversight-board-says-account-bans-lack-due-process-transparency/
https://techcrunch.com/2026/08/25/x-sends-cease-and-desist-to-open-source-project-nitter-over-alleged-scraping/
https://github.com/YoTro/facebook-mcp
https://github.com/fukata/messenger-mcp
https://github.com/matpb/meta-mcp-rs
https://techcrunch.com/2026/06/30/x-now-offers-an-mcp-server-to-make-its-platform-easier-for-ai-tools-to-use/
https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558
https://opentweet.io/blog/xmcp-x-official-mcp-server-guide
https://mcp.directory/blog/x-twitter-mcp-server
https://postproxy.dev/blog/x-api-pricing-2026/
https://www.blotato.com/blog/twitter-api-pricing
https://github.com/Infatoshi/x-mcp
https://github.com/JohannesHoppe/x-autonomous-mcp
https://docs.x.com/x-api/getting-started/pricing
https://weirapp.io/blog/is-scraping-twitter-legal
https://www.firsthundred.app/blog/twitter-mcp-server
https://opentweet.io/blog/twitter-automation-rules-2026
https://dealroom.co/news/147098-x-shuts-down-seven-year-old-nitter-with-cease-and-desist-letters/
https://en.wikipedia.org/wiki/Bright_Data
https://github.com/EnesCinr/twitter-mcp
https://apify.com/santamaria-automations/workatastartup-scraper
https://github.com/Rhovian/joule/issues/113
https://github.com/MIt9/workatastartup-mcp
https://pypi.org/project/workatastartup-mcp/
https://github.com/jwc20/waasuapi
https://www.pulsemcp.com/servers/yc-software-waas
https://www.zoko.io/post/use-whatsapp-api-without-business-verification
https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform
https://whale.co.il/en/blog/whatsapp-ai-policy-2026
https://zylos.ai/research/2026-01-26-whatsapp-api-automation/
https://theautomate.ai/blog/whatsapp-web-api/
https://pasqualepillitteri.it/en/news/12969/integrate-whatsapp-evolution-api-vs-cloud-api
https://www.pipes.bot/blog/baileys-vs-whatsapp-cloud-api-ban
https://blog.cubosuite.com.br/baileys-whatsapp-como-funciona-por-que-esta-sendo-bloqueado-e-o-que-usar/
https://www.reddit.com/r/whatsapp/comments/1rijf6b/whatsapp_message_automation_via_non_business/
https://github.com/lharries/whatsapp-mcp
https://www.pulsemcp.com/servers/lharries-whatsapp-bridge
https://github.com/jaingxyz/whatsapp-mcp
https://blog.cubosuite.com.br/meta-banindo-whatsapp-nao-oficial-em-2026-o-que-mudou-e-o-que-fazer/