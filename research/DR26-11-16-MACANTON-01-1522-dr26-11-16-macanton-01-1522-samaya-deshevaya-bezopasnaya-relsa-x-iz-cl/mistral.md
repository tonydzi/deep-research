# DR26-11-16-MACANTON-01-1522 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Приступаю к глубокому исследованию. План: (1) актуальные цены и правила X API v2 на ноябрь 2026, (2) ограничения автоматизации (ответы, «automated» label), (3) кандидаты-сервисы с MCP/API и их модели BYO vs собственное приложение, (4) неофициальные API и риски, (5) бесплатные способы ежедневного снимка метрик. Начинаю с первой волны поисков.

Ran 5 searches

Первая волна подтверждает pay-per-use модель. Открываю первичный источник (docs.x.com pricing) и продолжаю поиск по правилам автоматизации ответов.

Ran a web search · Ran 3 searches

Правило 23.02.2026 подтверждено (ответ только если упомянули/процитировали, тариф «Post: Create (summoned)» $0.010). Теперь исследую Buffer и Typefully.

Ran 6 searches

Buffer выглядит сильно: официальный MCP бесплатно, официальный партнёр X. Открываю первичные страницы Buffer и ищу Typefully/Postiz.

Ran a web search · Ran 5 searches

Теперь Ayrshare, Upload-Post, bundle.social, Blotato и официальный XMCP/xurl.

Ran 8 searches

Открываю первичные страницы кандидатов: Upload-Post, bundle.social, Blotato, Ayrshare (BYO), XMCP-гайд.

Ran a web search

Открываю продолжения: тарифы bundle.social X pass-through, лимиты Blotato, XMCP-гайд, twitterapi.io и правила лимитов X API.

Ran a web search · Ran 2 searches

Теперь Typefully, Postiz (чьё приложение), лимиты Buffer и X API rate limits, бесплатные способы подсчёта подписчиков.

Ran a web search · Ran 6 searches

Открываю оставшиеся первичные страницы (Typefully MCP, Postiz docs/MCP, Blotato ai-info) и проверяю спорные вопросы.

Ran a web search · Ran 5 searches

Продолжаю: цены Typefully/Postiz, полный список инструментов Buffer MCP, проверка живого бесплатного эндпоинта счётчика подписчиков, лимиты Blotato для X.

Ran a web search · Ran 2 searches

Проверяю тонкости правила об ответах (что именно «works»), влияние на Buffer Community и первичную цену Typefully Creator.

Ran a web search · Ran 4 searches

Читаю продолжение ReplySurge (точную таблицу ограничений) и проверяю правило про @упоминания/цитаты и бесплатные эндпоинты для метрик своего аккаунта.

Ran a web search · Ran 3 searches

Финальная проверка: биллинг загрузки изображений в X API, аналитика в Buffer MCP, и обзор «дешёвых» альтернативных способов публикации.

Ran a web search · Ran 2 searches

Последняя волна верификации: точные цены Ayrshare, новый кандидат Postproxy и официальный CLI xurl.

Ran a web search

Последние две проверки цен (Typefully Creator, Postproxy) — и собираю отчёт.

Ran a web search
DR26-11-16-MACANTON-01-1522 — Самая дешёвая и безопасная рельса X/Twitter для агента Claude Code

Дата исследования: 16 ноября 2026. Выполнено ~50 поисковых запросов, открыто и прочитано 27 первичных и вторичных источников; все ссылки ниже реально открыты и прочитаны. Все цены в USD.

1. Вопрос

Найти для аккаунта @Tony_Stef_ (22.8k подписчиков, ~25 постов/сутки из Claude Code, без ссылок) самую дешёвую из безопасных «рельс» публикации и чтения X, покрывающую 4 функции: публикация ~750 постов/мес (часть с изображениями), чтение упоминаний + ответы подписчикам, мониторинг 30-80 чужих аккаунтов каждые ~4 часа, ежедневный снимок метрик. Без браузерной автоматизации, без ссылок в постах.

2. Резюме (ранжированные выводы)
Правило 23.02.2026 подтверждено и шире, чем вы знали. Программные ответы через POST /2/tweets на self-serve-тарифах разрешены только если автор оригинального поста «summoned» вас (упомянул или процитировал); такие ответы тарифицируются отдельной позицией $0.010 («Post: Create (summoned)»). Холодные ответы дают 403 ( docs.x.com/pricing,  ReplySurge). Ответы на свои собственные посты разрешены (подтверждение сотрудника X в треде 257955, цит. по  ReplySurge). Enterprise и «Public Utility» приложения освобождены от ограничения, поэтому крупные сервисы (Buffer и др.) могут отвечать там, где ваш собственный app не может.
Два некомпенсированных ужесточения: с 28.02.2026 запрещены @упоминания «чужих» в обычных (не ответных) постах, с 16.03.2026 запрещено программное цитирование чужих постов (quote_tweet_id), цитировать свои можно ( ReplySurge). Это напрямую ограничивает вашу функцию 3: мониторить чужие посты можно, но программно ответить или процитировать чужой пост нельзя, пока автор не упомянет вас.
Дешевле всего публиковать через Buffer. Buffer: официальный партнёр X по API, официальный удалённый MCP-сервер https://mcp.buffer.com/mcp (HTTP + OAuth), API на всех тарифах включая Free ( buffer.com/api,  developers.buffer.com). Essentials стоит $6/канал/мес ($5 при годовой оплате) с неограниченной очередью и 3 000 API-запросов на канал в месяц ( buffer.com/pricing.md). Публикация 750 постов обойдётся в $0.008 за пост против текущих $0.015, изображения без доплаты, риск публикации переходит на официальное приложение Buffer.
Чтение своих данных осталось копеечным. Owned Reads ($0.001 за ресурс: свои посты, упоминания GET /2/users/{id}/mentions, фолловеры и др.) не подорожали; чтение упоминаний и ежедневный снимок метрик ваших постов вместе стоят около $2/мес ( docs.x.com/pricing).
Мониторинг 50 чужих аккаунтов — единственная дорогая статья на официальном API. Каждое чтение чужого поста стоит $0.005 с дедупликацией в пределах суток UTC, то есть ~$15-38/мес в зависимости от активности аккаунтов ( docs.x.com/pricing). Неофициальный twitterapi.io делает то же за ~$0.5-1.1/мес ($0.15 за 1 000 твитов), но это серая зона ( twitterapi.io/pricing).
Рекомендуемая связка: Buffer Essentials $6 + X API pay-per-use на owned-чтение и summoned-ответы (+ опционально twitterapi.io только на мониторинг). Итог ≈ $33-56/мес всё на официальных API, или ≈ $19/мес с серым мониторингом. Это дешевле и безопаснее текущей чистой рельсы X API (~$39-61/мес).
Плоские тарифы конкурентов не проходят по дневным лимитам X: Upload-Post Basic/Professional режут X на 10/20 постов в сутки ( docs.upload-post.com), bundle.social Pro на 15 ( bundle.social/pricing.md), Postproxy на 20 ( postproxy.dev/pricing). Вам нужно 25/сутки.
3. Методология

Поиск: цены и лимиты вендоров (официальные pricing-страницы и docs), правила X (docs.x.com, changelog, форум разработчиков через вторичные разборы, т.к. devcommunity.x.com блокирует автоматическое чтение), обзоры и калькуляторы конкурентов (Blotato, OpenTweet, Upload-Post, TwittrAPI, Sorsa) для перепроверки. Внутренних источников по вашей задаче не было, все данные публичные. Ограничения: (1) форум X Developers не открывается роботом, поэтому цитаты тредов взяты через открытый разбор ReplySurge; (2) x.com (посты @XDevelopers) цитируются по тому же разбору; (3) на страницах вендоров не всегда указана дата, отмечено «без даты». Противоречащие цифры приведены обеими сторонами, без усреднения.

4. Проверенная фактура X API (16.11.2026)
4.1 Тарифы pay-per-use ( docs.x.com/x-api/getting-started/pricing, страница без даты, читана 16.11.2026)
Операция
Цена
Post: Create
$0.015/запрос
Post: Create (с URL)
$0.200/запрос
Post: Create (summoned, ответ на упомянувшего/процитировавшего)
$0.010/запрос
Post: Read
$0.005/ресурс
User: Read
$0.010/ресурс
Owned Reads (свои посты /2/users/{id}/tweets, упоминания /2/users/{id}/mentions, свои фолловеры/подписки и др.)
$0.001/ресурс
Interaction: Delete
$0.010
Webhook post.create (X Activity API)
$0.005/событие
Дедупликация чтений
в пределах суток UTC

Ваши известные цены подтверждены без изменений: $0.015 / $0.20 / $0.005 / $0.010 / $0.001. Новое для вас: отдельный тариф summoned-ответа $0.010 (дешевле обычного поста). Потолок pay-per-use: 3 млн чтений постов в месяц, свыше только Enterprise ( docs.x.com/pricing). Новый аккаунт pay-per-use получает бонусы: $20 за привязку карты + до $50 матчем первого автопополнения ( docs.x.com/changelog).

4.2 Лимиты ( api.sorsa.io, извлечение лимитов X от 02.11.2026, страница обновлена в октябре 2026)
POST /2/tweets: 100 запросов/15 мин на пользователя, 10 000/24 ч на приложение: вашим 25 постам/сутки лимиты не помеха.
Неверифицированный аккаунт: максимум 50 постов и 200 ответов в сутки. Если @Tony_Stef_ не верифицирован, запас по постам вдвое, по ответам впритык к вашей оценке 20-50/сутки.
Чтение таймлайна чужого аккаунта: 10 000/15 мин на приложение, 900/15 мин на пользователя; упоминания: 450/15 мин на приложение.
4.3 Правила автоматизации (критично для вашего дизайна)
23.02.2026: программные ответы через POST /2/tweets на self-serve-тарифах (Free/Basic/Pro/Pay-Per-Use) разрешены только при «summoned»-контексте; Enterprise и Public Utility приложения не затронуты ( ReplySurge, с цитатами объявления X от 23-24.02.2026).
Ответы на собственные посты разрешены (X Staff, тред 257955, 24.02.2026): авто-ответы-«продолжения» под своими постами работают ( ReplySurge).
28.02.2026: @упоминания пользователей в обычных постах заблокированы; в ответах можно упоминать только уже участвующих в треде ( ReplySurge).
16.03.2026: программное цитирование чужих постов (quote_tweet_id) заблокировано, своих постов разрешено ( ReplySurge).
Апрель 2026: лайки, подписки и цитирования как API-записи удалены со всех self-serve-планов, по данным извлечения от 02.11.2026 они выполняются только на Enterprise ( api.sorsa.io).
Интент X: «Мы нашли 42 000 аккаунтов, автоматизирующих ответы чат-ботами, и удалили их. Программная AI-вовлечённость без человека в контуре противоречит нашей миссии» (Nikita Bier, then Head of Product, 24.07.2026, цит. по  ReplySurge). Проектируйте ответы с человеком в контуре (approver-режим Claude Code).
Двойной биллинг изображений (не подтверждено документально): разработчики на форуме X сообщают, что с 31.08.2026 каждый загруженный медиа-объект тарифицируется как PostCreate сверх самого поста (пост с картинкой = $0.030). На странице тарифов это не зафиксировано, открыть треды форум не дал. Закладывайте worst case для прямого API; через Buffer/Typefully/Postiz доплаты нет.
4.4 Официальные инструменты для агентов
XMCP: официальный MCP-сервер X, запущен 30.06.2026, hosted-эндпоинт https://api.x.com/mcp, 140+ операций, работает на ключах вашего dev-приложения, биллинг тот же pay-per-use; есть read-only режим ?read_only=true ( opentweet.io XMCP guide от 04.07.2026,  docs.x.com/changelog).
xurl: официальный curl-подобный CLI X API на ключах вашего приложения, с SKILL.md для агентов (npx skills add https://github.com/xdevplatform/xurl) ( docs.x.com/tools/xurl). Идеален для Claude Code: чтение и запись без отдельного MCP.
5. Кандидаты

Формат: цена за пост · ответ · чтение упоминаний · мониторинг чужих аккаунтов · метрики · лимиты · MCP/API/CLI · чьё X-приложение · риск · подтверждение 2026.

X API v2 pay-per-use напрямую (ваша текущая рельса)
Параметр
Значение
Цена за пост / за ответ
$0.015 / summoned-ответ $0.010; холодные ответы 403 ( docs.x.com/pricing)
Чтение упоминаний
Owned Read $0.001 за упоминание ( docs.x.com/pricing)
Мониторинг чужих аккаунтов
$0.005 за чужой пост, дедупликация в пределах суток UTC
Метрики
свои посты (показы через organic-метрики до 30 дней) и фолловеры: owned $0.001/пост, user read $0.010 ( docs.x.com/pricing)
MCP/API/CLI
XMCP https://api.x.com/mcp, xurl CLI, REST ( opentweet.io,  docs.x.com/tools/xurl)
Чьё приложение
ваше (весь риск и биллинг на вашем dev-app)
Риск
минимальный технический; ограничения ответов/упоминаний/цитат (п.4.3)
Подтверждение 2026
вы сами работаете на нём; тарифы подтверждены docs.x.com 16.11.2026
Buffer (Free и Essentials)
Параметр
Значение
Тарифы
Free $0 (3 канала, очередь максимум 10 постов на канал, 1 API-ключ, 1 000 запросов/канал/мес); Essentials $6/канал/мес ($5 годовых), безлимитная очередь, 3 ключа, 3 000 запросов/канал/мес; 15-минутный лимит 150/450 запросов ( buffer.com/pricing.md,  buffer.com/api)
Цена за пост
$0 per-post; при 750 постах/мес: $0 на Free (не проходит по очереди), $0.008 на Essentials
Ответы подписчикам
Buffer Community inbox: комментарии X в near real-time, ответы из Buffer на всех тарифах; неверифицированный X-аккаунт ограничен 200 ответами/сутки; на платном тарифе бэкфилл комментариев за 30 дней, на Free только новые посты ( support.buffer.com). В API/MCP инструментов ответов на комментарии X нет: ответами управляет человек в UI Buffer (это совпадает с политикой X о «человеке в контуре»)
Чтение упоминаний
через Community (UI), не через API/MCP; упоминания вне комментариев к своим постам Community не покрывает
Мониторинг чужих аккаунтов
нет
Метрики
Insights на Free: рост подписчиков, engagement, показы, 30 дней истории ( buffer.com/pricing.md); в API/MCP есть includeMetrics у постов ( developers.buffer.com). Конфликт: справка Buffer утверждала, что X не отдаёт показы через API (не открыто, помечено в п.10)
MCP
официальный, удалённый, https://mcp.buffer.com/mcp, Streamable HTTP, OAuth, create_post для X (текст, треды, изображения/видео), list_posts, get_post, аналитика ( developers.buffer.com)
Чьё приложение
Buffer: официальный API-партнёр X, публикация через приложение Buffer, ваш dev-app не участвует ( buffer.com/api)
Риск
низкий: официальный партнёр, без браузерной автоматизации; ярлык «automated» не требуется для обычного планирования
Подтверждение 2026
метрики API Buffer: 30 млн созданных через API/MCP постов; документы читаны 16.11.2026

Оценка: Free не берёт вашу очередь из 418 постов (10 слотов, автодоводка возможна, но хрупко: 750 create-запросов + обслуживание впритык к 1 000/мес на канал). Essentials решает всё за $6.

Typefully
Параметр
Значение
Тарифы
Free $0, 10 постов/мес; Creator $99/год (~$8.25/мес, только годовая оплата), 1 000 постов/мес; Business $18-20/set/мес, 1 500 постов/мес ( typefully.com/pricing, цены по  justpricing от 23.10.2026)
Цена за пост
Creator: $8.25/750 = $0.011
Ответы
фича «Reply and quote tweets» есть в UI (человек); API/MCP ответов на чужие комментарии нет
Чтение упоминаний
нет
Мониторинг
нет
Метрики
X-аналитика включена с Creator (детальные метрики, CSV) ( typefully.com/pricing)
MCP
официальный, удалённый, https://mcp.typefully.com/mcp, Streamable HTTP + OAuth, есть раздел «Claude Code (CLI)»; создание/планирование драфтов, треды, медиа, очередь; API/MCP на всех тарифах ( support.typefully.com, статья от 28.08.2026)
Чьё приложение
Typefully (сервис публикует сам, ключи X не нужны)
Риск
низкий; лимит 1 000 постов/мес на Creator впритык (750 ок)
Подтверждение 2026
MCP-статья и цены октябрь 2026
Postiz Cloud / self-hosted
Параметр
Значение
Тарифы
Standard $29/мес, 5 каналов, безлимит постов/мес; Team $39/10; Pro $49/30; Ultimate $99/100; API, MCP, CLI и аналитика включены во все тарифы ( postiz.com/pricing)
Цена за пост
$29/750 = $0.039
Ответы/чтение
комментариев-инбокса для X нет; упоминания не читает
Метрики
аналитика включена; для X есть отключаемый фетч аналитики ( docs.postiz.com/providers/x-twitter)
MCP
официальный https://mcp.postiz.com/mcp-oauth-dynamic (OAuth, без ключа), Claude/ChatGPT/Cursor/Codex ( postiz.com/mcp)
Чьё приложение
Cloud: подключение X через OAuth без ключей разработчика, то есть приложение Postiz (косвенное подтверждение; явной строки в доках нет, вопрос в саппорт). Self-hosted: только BYO, свой X dev-app обязателен ( docs.postiz.com/providers/x-twitter)
Риск
низкий (официальный API); self-host добавляет поддержку собственного приложения за $0 подписки, но биллинг X остаётся вашим
Подтверждение 2026
цены читаны 16.11.2026
Ayrshare
Параметр
Значение
Тарифы
Premium $149/мес за 1 профиль (до 14 сетей); Launch $299/10 профилей; Business $599/30 ( ayrshare.com/pricing)
Чьё приложение
BYO: только ваш X dev-app и ваши кредиты, запросы несут заголовки X-Twitter-OAuth1-Api-Key/Secret от вашего приложения ( ayrshare.com/x-twitter-api). Платите Ayrshare за оболочку и повторно X за каждую операцию
MCP
есть собственный hosted MCP ( ayrshare.com/ai-agent, читан по сниппету выдачи; страница открыта 16.11.2026)
Риск
низкий, но экономически проигрывает всем: двойная плата
Вердикт
не конкурент для одного аккаунта
Upload-Post

Basic $24/мес (безлимит загрузок, 5 профилей), но X cap: Free/Basic 10 постов/24 ч, Professional 20, Advanced/Business 30; ссылки из X-постов вырезаются по умолчанию (аддон X Links $19/мес за 50 ссылок) ( docs.upload-post.com, обновлено октябрь 2026). Под 25 постов/сутки проходит только Advanced за $147/мес. Официальный MCP и Claude Code plugin есть. Вердикт: не подходит по цене из-за дневного cap.

bundle.social

Pro $100/мес (10 000 постов), но X cap 15 постов/24 ч и 15 комментариев/24 ч, плюс сквозная тарификация X 1:1 ($0.015 пост, $0.20 с ссылкой, $0.005 чтение) с предоплаченным балансом ( bundle.social/pricing.md от 30.07.2026). Полезная деталь от них: «обновление метрик собственного аккаунта X (фолловеры и др.) не тарифицируется X» (проверить, см. п.10). Вердикт: не проходит по cap.

Blotato

Starter $29/мес (20 аккаунтов, без per-post платы, очередь 200 постов), Creator $97 (очередь 1 000): ваша очередь 418 требует Creator либо автодоводку. Официальные API платформ, собственные approved-приложения, MCP https://mcp.blotato.com/mcp (35 инструментов, включая Claude Code), аналитика 8 платформ включая X, но комментарии только Instagram/Facebook, X-ответов нет; триал не включает API/MCP ( blotato.com/pricing.md от 10.09.2026,  blotato.com/ai-info от 17.09.2026). Вердикт: дороже Buffer, без чтения.

OpenTweet

Advanced $29/мес: 100 постов/день, 300 ссылок/мес, 10 API-ключей, X-аналитика; Pro $19.99 (20 постов/день) ( opentweet.io/pricing). Публикация через официальный API без вашего dev-app ( twittrapi.com от 20.10.2026). Чтение упоминаний и чужих таймлайнов в тарифах не заявлено. Вердикт: живая альтернатива публикации за $29, но Buffer вдвое дешевле.

Postproxy

Agentic $17/мес (5 профилей, безлимит постов и engagements), официальный API/MCP/комментарии/DM, ЕС-хостинг; X cap 20 постов/24 ч, выше только через BYOK/pay-as-you-go ( postproxy.dev/pricing). Вердикт: не проходит по 25/сутки.

Неофициальные API (серая зона, оцениваю риск отдельно)
twitterapi.io (чтение): $0.15 за 1 000 твитов, профили от $0.01/1K, минимум $0.00015/вызов ( twitterapi.io/pricing). Мониторинг 50 аккаунтов каждые 4 ч = ~7 500 прочитанных постов/мес ≈ $1.13/мес (против $15-38 на официальном API). Риск для аккаунта: низкий (скрейп их инфраструктурой, ваш аккаунт не участвует, cookie не нужны). Риск юридический/ToS: есть (обход API X, данные могут ломаться, оплаты и стабильность не гарантированы). Годится только на мониторинг, не на публикацию.
TwittrAPI и session-cookie-публикация: $0.0001/действие через ваш cookie и прокси ( twittrapi.com). Прямое нарушение правил X («use non-API-based forms of automation» прямо запрещено автоматизацией-политикой X, ср. п.4.3): риск бана аккаунта высокий. Не рекомендую в принципе, аккаунт 22.8k не стоит $10 экономии.
6. Месячная цена для вашего профиля

Профиль: 750 постов без ссылок + 1 000 ответов + чтение упоминаний (~1 200/мес) + мониторинг 50 аккаунтов каждые 4 ч (~3 000-7 500 уникальных чужих постов/мес) + 30 ежедневных снимков (25 своих постов + 1 чтение профиля). Сумма цен X API взята из  docs.x.com/pricing.

Связка
Публикация
Ответы (1 000, summoned)
Упоминания
Мониторинг
Снимок
Итого/мес
X API напрямую (текущая)
$11.25
$10.00
$1.20
$15-37.50
$1.05
$38.5-61.0
Buffer Free + X API (чтение)
$0
$10.00
$1.20
$15-37.50
$1.05
$27.3-49.8 (очередь max 10, впритык по API-квоте, хрупко)
Buffer Essentials + X API (чтение)
$6.00
$10.00
$1.20
$15-37.50
$1.05
$33.3-55.8
Typefully Creator + X API (чтение)
$8.25
$10.00
$1.20
$15-37.50
$0.30-1.05
$35.0-57.5
Postiz Standard + X API
$29.00
$10.00
$1.20
$15-37.50
$1.05
$56.3-78.8
Blotato Starter + X API
$29.00
$10.00
$1.20
$15-37.50
$1.05
$56.3-78.8
Upload-Post Advanced + X API
$147.00
$10.00
$1.20
$15-37.50
$1.05
$174-196.8
Ayrshare Premium + X API (BYO)
$149+11.25
$10
$1.20
$15-37.50
$1.05
$187-209.8
Buffer Essentials + X API + twitterapi.io (мониторинг)
$6.00
$10.00
$1.20
$0.50-1.15
$1.05
$18.8-19.4

Примечания к расчёту: (1) «1 000 ответов» через собственный X API возможны только в summoned-контексте; если ваши 1 000 ответов это в основном ответы на комментарии под своими постами, часть из них X API заблокирует (см. п.9, тест №1); (2) мониторинг на X API не зависит от частоты опроса (дедупликация за сутки UTC), только от числа уникальных новых постов; (3) для неверифицированного аккаунта потолок 200 ответов/сутки; (4) медиа на прямом X API закладывайте worst case ×2 (п. 4.3), через Buffer доплаты нет.

7. Рекомендация

Связка: Buffer Essentials ($6/мес, 1 канал) через официальный MCP https://mcp.buffer.com/mcp для публикации + ваш X API pay-per-use (XMCP или xurl) для owned-чтения и summoned-ответов.

Почему:

Цена за пост $0.008 против текущих $0.015 (−47% на публикации), изображения и треды без доплаты, очередь безлимитная (ваши 418 постов встанут целиком). MCP удалённый, HTTP + OAuth, работает прямо из Claude Code ( developers.buffer.com,  buffer.com/pricing.md).
Безопасность: Buffer публикует своим официальным partner-приложением X ( buffer.com/api); ваш dev-app перестаёт несть риск публикаций; никакого браузера; никакого «серого» API.
Чтение и снимок дешевы на owned-тарифах: упоминания и свои посты $0.001 за ресурс, свой профиль $0.010/день ( docs.x.com/pricing), суммарно ~$2.3/мес.
Ответы: на упомянувших/процитировавших вас отвечайте через X API по $0.010 (это ровно то, что X разрешает); на комментарии под своими постами отвечает человек через Buffer Community inbox (200 ответов/сутки для неверифицированного аккаунта) ( support.buffer.com). Это же и есть правильный с точки зрения X дизайн: публикация автоматическая, ответы с человеком в контуре.
Мониторинг 50 аккаунтов: на официальном API это главная статья ($15-38/мес). Если принимаете серую зону для чтения, twitterapi.io снизит её до ~$1/мес ( twitterapi.io/pricing); публикация при этом остаётся белой (Buffer). Мой совет: начать с официального чтения месяц, посмотреть реальные уникальные объёмы, потом решать.

Итог: $33-56/мес полностью белая связка (≈$19/мес с серым мониторингом), против $38.5-61 на текущей чистой рельсе. Экономия скромная в деньгах, но существенная по риску: публикации уходят с вашего dev-app на официального партнёра, очередь и MCP бесплатны, summoned-ответы дешевле постов.

Запасной вариант, если Buffer не понравится: Typefully Creator ($99/год) даёт те же 750 постов ($0.011/пост), официальный MCP с разделом Claude Code и X-аналитику в UI ( typefully.com/pricing,  support.typefully.com).

8. Бесплатный ежедневный снимок числа подписчиков

Реально работающие в 2026 варианты без браузерной автоматизации:

Buffer Insights уже в вашей связке: на всех тарифах включая Free, «рост подписчиков, engagement, показы, производительность постов, 30 дней истории» ( buffer.com/pricing.md). В UI Buffer, не через API: агенту недоступно, вам на глаза ежедневно.
X API почти бесплатно: GET /2/users/by/username/{handle}?user.fields=public_metrics стоит $0.010 за чтение профиля ( docs.x.com/pricing), то есть $0.30/мес за 30 снимков, автоматизируется из Claude Code через xurl. Публичные метрики (followers_count, following_count, tweet_count) отдаются любым read-API ( twitterapi.io blog по эндпоинтам X, открыт 16.11.2026; официальный путь /2/users/by/username/{username} с user.fields=public_metrics там же).
Проверьте бесплатно ли: bundle.social заявляет, что «обновление собственных метрик аккаунта X (фолловеры, подписки, счётчик постов) не тарифицируется: X не метрит этот вызов» ( bundle.social/pricing.md от 30.07.2026). Это противоречит таблице User Read $0.010 из  docs.x.com/pricing. Один тестовый день покажет: снимайте баланс до/после вызова в Developer Console. Если подтвердится, снимок подписчиков $0/мес.
Показы (impressions) своих постов: только через X API (organic/non-public метрики доступны 30 дней от создания поста) на owned-тарифе $0.001/пост ( docs.x.com/x-api/fundamentals/metrics, открыт 16.11.2026) или в аналитике Typefully Creator. Buffer в тарифах обещает показы в Insights, но справка Buffer исторически говорила, что X не отдаёт показы через API (см. конфликт в п.10): уточните у Buffer до оплаты.

Старый бесплатный cdn.syndication.twimg.com-виджет счётчика проверить не удалось (X блокирует автоматическое чтение), на него не рассчитывайте.

9. Риски и что проверить до оплаты

Тесты до денег (1 день, ~$1 кредитов X):

Тест-ответ через ваш X API на комментарий под своим постом. Формулировка X «you have not been mentioned or otherwise engaged by the author» допускает, что комментатор, написавший под вашим постом, считается «engaged» вами: если тест пройдёт, функция 2 автоматизируется целиком; если 403, ответы на комментарии остаются в Buffer Community (человек).
Тест summoned-ответа ($0.010) и тест поста с изображением: сверьте биллинг в Developer Console ($0.015 или $0.030 за пост с картинкой, п. 4.3).
Тест public_metrics своего профиля: списывается ли $0.010 или ноль (п. 8.3).
Тест X API reading 5 мониторинговых аккаунтов в течение суток: измерьте реальные уникальные посты/день, чтобы уточнить $15-38.

Вопросы в Buffer support (до апгрейда до Essentials):

Работают ли ответы на комментарии X в Community после ограничения 23.02.2026, и каков статус X-приложения Buffer (Enterprise/Public Utility)?
Что именно Buffer Insights показывает для X: фолловеры, показы (impressions) или только engagement? Доступно ли это через API/MCP?
Не противоречит ли автодоводка очереди (~25 create_post/сутки через MCP) fair use политике Essentials?

Вопрос в Postiz (если рассматриваете его вместо Buffer): публикует ли Postiz Cloud на X через собственное приложение Postiz (без BYO)? Self-hosted точно BYO ( docs.postiz.com/providers/x-twitter).

Риски по порядку величины:

Аккаунт: планировщики через официальные API (Buffer, Typefully, Postiz) исторически безопасны; cookie-публикация (TwittrAPI) недопустима; автолайки/автофолловы больше невозможны на self-serve ( api.sorsa.io).
Функция 3 (мониторинг с последующими ответами/цитатами): программные ответы и цитаты чужих постов заблокированы с 23.02-16.03.2026 ( ReplySurge). Мониторинг остаётся полезным для контента и для выявления тех, кто вас упомянул/процитировал (тогда отвечать можно). Пересмотрите ожидания от «последующих ответов/цитат».
Ярлык «automated»: X требует помечать автоматические аккаунты ( replysurge, цитата политики X). Обычный контент-планировщик не требует метки, но авто-вовлечённость без человека в контуре это ровно то, за что X удалил 42 000 аккаунтов: держите ответы и упоминания за human-approver-ом.
Ценовая волатильность X: за 2026 год X поднял цену поста $0.01→$0.015, ввел $0.20 за ссылку, снял лайки/фолловы с self-serve, ввел и поднимал потолок чтений (2M→3M) ( opentweet.io XMCP guide от 04.07.2026,  docs.x.com/pricing,  api.sorsa.io). Плоские тарифы (Buffer) фиксируют вашу главную статью расходов.
10. Замечания к источникам
Источник
Достоверность
Обновлено
 X API pricing, docs.x.com
5/5
без даты, читано 16.11.2026
 X API changelog, docs.x.com
5/5
читано 16.11.2026
 xurl, docs.x.com
5/5
читано 16.11.2026
 ReplySurge: разбор ограничения ответов
4/5 (вторичный, с цитатами первичных)
14.09.2026
 Sorsa: rate limits X API
4/5 (извлечение таблиц X от 02.11.2026; сам Sorsa коммерческий конкурент)
октябрь 2026
 Buffer pricing
5/5
читано 16.11.2026
 Buffer API и  Buffer MCP guide
5/5
читано 16.11.2026
 Buffer Community: комментарии X
5/5
без даты, читано 16.11.2026
 Typefully pricing
5/5 (числа цен дорендерены JS, сняты через justpricing)
читано 16.11.2026
 Typefully MCP Server
5/5
28.08.2026
 Justpricing: цены Typefully
3/5 (агрегатор)
23.10.2026
 Postiz pricing,  Postiz MCP,  Postiz docs X
5/5
читано 16.11.2026
 Upload-Post: pricing and limits
5/5
октябрь 2026
 bundle.social pricing.md
5/5
30.07.2026
 Blotato pricing.md,  Blotato ai-info
4/5 (самоописание вендора)
10.09.2026 / 17.09.2026
 Ayrshare pricing,  Ayrshare X API (BYO)
5/5
читано 16.11.2026
 OpenTweet pricing,  OpenTweet XMCP guide
3/5 (вендор; гайд датирован)
16.11.2026 / 04.07.2026
 Postproxy pricing
5/5
читано 16.11.2026
 twitterapi.io pricing
3/5 (неофициальный сервис, самопубликация цен)
читано 16.11.2026
 TwittrAPI: сравнение пяти способов
3/5 (вендор серого сегмента, но таблица сверена с первоисточниками)
проверено 20.10.2026

Конфликты (показываю обе стороны, без усреднения):

Потолок чтений pay-per-use: 3 млн/мес по  docs.x.com/pricing и  api.sorsa.io против 2 млн по  opentweet.io (04.07.2026): вероятно, потолок поднят с 2 до 3 млн между июлем и октябрем 2026, ориентируйтесь на 3 млн.
Снимок метрик своего профиля: $0.010 за User Read по  docs.x.com/pricing против «X не тарифицирует этот вызов» по  bundle.social/pricing.md: только живой тест (п. 8.3).
Показы X в Buffer: тарифная страница обещает impressions в Insights ( buffer.com/pricing.md); справочный центр Buffer (не открыт, недоступен автоматическому чтению) исторически утверждает, что X не отдаёт показы через API: вопрос №2 в Buffer support.
Двойной биллинг медиа в X API: жалобы на форуме X Developers (треды не открылись, форум блокирует роботов) против отсутствия такой строки в  docs.x.com/pricing: считайте неразрешённым до теста №2.
Цена Typefully Creator: страница  typefully.com/pricing не отдала число в статическом HTML; $99/год сняты с  justpricing (19-23.10.2026).
11. Открытые вопросы
Пройдёт ли через ваш X API ответ на комментарий под вашим постом (формулировка «otherwise engaged» допускает оба исхода) — определяет, автоматизируется ли функция 2 полностью.
Является ли X-приложение Buffer Enterprise/Public Utility (отсюда работоспособность ответов Buffer Community) — официальный вопрос в поддержку.
Публикует ли Postiz Cloud через собственное приложение X — подтвердить перед выбором между Buffer/Postiz.
Тарифицируется ли вызов собственных публичных метрик (п. 8.3) — от этого зависит «бесплатность» снимка.
Реальная суточная активность ваших 50 мониторинговых аккаунтов — уточнит главную статью расходов ($15 vs $38/мес).
Точный биллинг постов с медиа на прямом X API ($0.015 vs $0.030) — не задокументирован X на странице тарифов.
12. Следующие шаги
Сегодня: 4 тестовых вызова X API из п.9 (~$1), снять показания баланса Developer Console до/после.
Подключить Buffer Free, подключить X-канал, добавить MCP https://mcp.buffer.com/mcp в Claude Code, отправить 5 тестовых постов через create_post (текст + изображение), проверить Community inbox на X-комментарии.
Отправить 3 вопроса в Buffer support (п.9).
По ответам: апгрейд до Essentials $6 (или годовая $5/мес), выгрузка очереди 418 постов через MCP, перевод чтения на xurl/XMCP с read-only-режимом для мониторинга (https://api.x.com/mcp?read_only=true).
Через месяц эксплуатации: пересчитать мониторинг по факту и решить, оставаться на официальном чтении ($15-38) или переносить только его на twitterapi.io (~$1).
Sources
2/2
3:39pm
Knowledge automatically saves key insights from your chats
Set up now




Think
Vibe can make mistakes. Check answers. Learn more
Самая дешёвая X-рельса для агента

## Sources (links)
https://docs.x.com/x-api/getting-started/pricing
https://www.outstand.so/blog/x-api-pricing
https://opentweet.io/x-api-pricing
https://www.socialync.io/blog/x-api-pricing-and-posting-2026
https://www.blotato.com/blog/twitter-api-pricing
https://data365.co/guides/twitter-api-limitations-and-pricing
https://www.activepieces.com/blog/how-to-get-an-x-twitter-api-key-limits-and-errors
https://invisibleapi.ai/blog/x-api-pricing/
https://www.twittrapi.com/blog/x-api-pricing-2026
https://posteverywhere.ai/blog/x-twitter-api-pricing
https://www.socialcrawl.dev/blog/x-twitter-api-2026
https://api.sorsa.io/blog/twitter-api-pricing-2026
https://www.tryadeli.com/blog/x-api-pricing
https://elfsight.com/blog/how-to-get-x-twitter-api-key-in-2026/
https://www.postzen.dev/blog/twitter-api-pricing
https://xautodm.com/blog/x-api-pricing-explained-2026-cheaper-alternatives
https://climbx.so/blog/x-api-limits
https://api.sorsa.io/blog/twitter-api-rate-limits-2026
https://apiserpent.com/blog/x-api-comparison-2026
https://www.xpoz.ai/blog/guides/understanding-twitter-api-pricing-tiers-and-alternatives/
https://devcommunity.x.com/t/pay-per-use-please-document-media-upload-billing-follow-up-to-274646/279445
https://kalen.si/blog/twitter-api
https://www.upload-post.com/x-api-pricing/
https://apnews.com/article/prices-personal-data-walmart-doordash-delta-uber-19c634a5205557ebe0af99ec5c3a729a
https://twitterapi.io/blog/x-developer-portal
https://www.getxapi.com/twitter-api-pricing
https://apis.io/providers/x/
https://replysurge.com/blog/x-api-reply-restriction-what-still-works
https://devcommunity.x.com/t/x-api-v2-update-addressing-llm-generated-spam/257909
https://x.com/XDevelopers/status/2026084506822730185
https://opentweet.io/answers/x-api-403-forbidden-posting
https://devcommunity.x.com/t/getting-a-403-when-replying-to-tweets-via-api-but-live-interaction-works-fine/260431
https://devcommunity.x.com/t/error-403-when-posting-replies-or-liking-tweets/256706
https://piunikaweb.com/2026/02/24/x-api-blocks-automated-spam-replies/
https://fireply.ai/blog/x-api-reply-restriction-2026
https://devcommunity.x.com/t/replies-via-api-return-403-not-authorized-for-resource-and-account-shows-temporarily-limited/280359
https://devcommunity.x.com/t/api-v2-403-forbidden-on-community-posting-community-id-payload-despite-pay-per-use-tier-and-prior-functionality-feb-17-2026/257434
https://socialnexis.com/guides/x-api-basic-enterprise-automation-rules
https://opentweet.io/how-to/fix-x-api-reply-restriction
https://www.codewords.ai/blog/automatic-tweet-reply
https://help.x.com/en/rules-and-policies/x-automation
https://docs.x.com/changelog
https://docs.x.com/developer-guidelines
https://verifiedher.com/platforms/x
https://www.unfollr.com/blog/twitter-automation-rules
https://opentweet.io/blog/twitter-automation-rules-2026
https://help.x.com/en/rules-and-policies/authenticity
https://tweetfull.com/blog/x-bots-how-automated-accounts-shape-twitter-x-in-2026/
https://note.com/munou_ac/n/n263ece4f9db4?hl=en
https://www.post-bridge.com/blog/social-media-posting-bots
https://note.com/munou_ac/n/n0fb681d97ded?hl=en
https://www.blotato.com/blog/buffer-pricing
https://postotter.app/buffer-pricing
https://www.socialync.io/blog/buffer-pricing-2026
https://postiv.ai/blog/buffer-pricing
https://pubblica.net/blog/buffer-pricing
https://www.usagepricing.com/blueprint/buffer
https://checkthat.ai/brands/buffer/pricing
https://support.buffer.com/article/595-features-available-on-each-buffer-plan
https://support.buffer.com/en-us/articles/buffer-pricing-and-features-6pJrOPuzIt
https://buffer.com/pricing
https://developers.buffer.com/guides/integrations/mcp.html
https://thatmarketingbuddy.com/mcp/buffer
https://buffer.com/mcp
https://buffer.com/resources/best-mcp-servers/
https://www.contextgate.ai/connectors/buffer/
https://www.pulsemcp.com/servers/ahernan2-buffer
https://zapier.com/mcp/buffer
https://registry.modelcontextprotocol.io/
https://github.com/modelcontextprotocol/servers
https://github.com/modelcontextprotocol
https://developers.buffer.com/guides/api-limits.html
https://buffer.com/api
https://buffer.com/pricing.md
https://schedulenchill.com/buffer-vs-schedule-and-chill
https://www.postzen.dev/blog/buffer-api
https://zernio.com/blog/buffer-api
https://use-apify.com/blog/buffer-free-plan-guide
https://www.lilachbullock.com/schedule-twitter-posts-free/
https://postory.io/blog/free-alternative-to-buffer
https://x.com/buffer/status/2097464920375808001
https://www.autotweet.io/questions/is-buffer-free
https://twitterapi.io/blog/buffer-alternatives
https://support.buffer.com/article/561-using-twitter-with-buffer
https://xautopilot.app/blog/buffer-pricing-2026-worth-it
https://withsynapse.app/blog/free-social-media-scheduler
https://support.buffer.com/article/921-engaging-with-community-comments-in-buffer
https://support.buffer.com/article/977-how-comments-work-across-each-social-network-in-buffer-community
https://support.buffer.com/en-us/articles/how-comments-work-across-each-social-network-in-buffer-community-2IEzkHcO0Q
https://support.buffer.com/article/922-engaging-with-community-comments-in-buffers-mobile-apps
https://support.buffer.com/en-us/articles/using-community-on-the-buffer-mobile-app-mCNla9C1We
https://support.buffer.com/article/922-using-community-on-the-buffer-mobile-app
https://growthopsweekly.com/articles/buffer-comment-replies-by-platform-2026-route-the-dms-it-skips
https://buffer.com/resources/introducing-community/
https://buffer.com/community
https://support.buffer.com/en-us/articles/using-community-in-buffer-jvTv4uQacA
https://buffer.com/made-for/developers
https://buffer.com/resources/twitter-api-changes/
https://support.buffer.com/en-us/articles/using-xtwitter-with-buffer-nA84XnWtuU
https://support.buffer.com/article/859-does-buffer-have-an-api
https://support.buffer.com/en-us/articles/using-buffers-api-GtIYIQilz5
https://buffer.com/developers/api
https://www.producthunt.com/products/buffer-api
https://opentweet.io/blog/typefully-pricing-2026
https://justpricing.com/typefully-pricing
https://voicemoat.com/blog/typefully-pricing-2026-breakdown
https://www.schedchie.com/prices/typefully
https://www.postel.app/typefully-pricing
https://www.toolworthy.ai/tool/typefully
https://xautopilot.app/blog/typefully-pricing-plans-costs-value-2026
https://xpatla.com/blog/typefully-pricing-2026
https://typefully.com/pricing
https://aiproductivity.ai/pricing/typefully/
https://support.typefully.com/en/articles/13128440-typefully-mcp-server
https://github.com/pepuscz/typefully-mcp-server
https://github.com/muhammedsamal/typefully-mcp
https://github.com/pascalporedda/typefully-mcp-server
https://mcp.so/server/typefully-mcp-server/pepuscz
https://composio.dev/toolkits/typefully
https://glama.ai/mcp/servers/pepuscz/typefully-mcp-server
https://typefully.com/changelog/all-new-api-zapier-integration-mcp-and-126
https://aerostack.dev/mcp/aerostack/mcp-typefully
https://lobehub.com/mcp/pascal-typefully-mcp-server
https://postiz.com/pricing
https://www.blotato.com/blog/postiz-pricing
https://hermoso.ai/pricing/postiz/
https://www.stork.ai/en/postiz
https://filipkonecny.com/blog/postiz-pricing/
https://ecom-tools.de/en/postiz-review/
https://omidsaffari.com/blog/postiz-review
https://docs.postiz.com/cloud/plans
https://postplanify.com/postiz-pricing
https://bestsocialmediascheduler.com/reviews/postiz
https://github.com/solomonneas/postiz-mcp
https://github.com/antoniolg/postiz-mcp
https://mcpservers.org/servers/postiz-mcp
https://knuckles-team.github.io/postiz-agent/
https://postiz.com/mcp
https://github.com/antoniolg/postiz-mcp/blob/main/README.md
https://www.reddit.com/r/selfhosted/comments/1jzsfav/postiz_v1392_opensource_social_media_scheduling/
https://github.com/oculairmedia/postizz-MCP
https://github.com/oculairmedia/postizz-MCP/blob/master/CLAUDE.md
https://github.com/cristdulcey/postiz-mcp
https://docs.postiz.com/self-host/providers/x-twitter
https://x.com/grok/status/2044019782354100551
https://xcloud.host/what-is-postiz-key-features-and-best-use-cases/
https://ossalt.com/guides/self-host-postiz-2026
https://dev.to/aryaniyaps/how-i-self-hosted-postiz-with-tailscale-and-the-env-vars-the-docs-dont-tell-you-about-4i8g
https://noobclaw.com/blog/postiz-self-hosted-tiktok-api-approval/
https://github.com/sujayxaradhya/postiz-selfhosted
https://github.com/sujayxaradhya/postiz-selfhosted/blob/main/README.md
https://crunchtools.com/self-hosting-postiz-rhel10-one-container-six-platforms/
https://opsily.com/blog/postiz-ai
https://mcp.buffer.com/mcp
https://www.ayrshare.com/pricing/
https://www.ayrshare.com/x-twitter-api/
https://social-api.ai/blog/ayrshare-pricing-social-media-api-2026
https://www.blotato.com/blog/ayrshare-pricing
https://www.plugkit.co/blog/ayrshare-alternative-pricing/
https://withsynapse.app/blog/ayrshare-alternative
https://heytasky.com/tools/ayrshare/
https://opentweet.io/alternatives/ayrshare
https://omnisocials.com/ayrshare-alternative
https://www.capterra.com/p/213297/Ayrshare/
https://www.ayrshare.com/
https://www.ayrshare.com/ai-agent/
https://github.com/vanman2024/ayrshare-mcp
https://ayrshare.online/
https://www.ayrshare.com/social-media-publishing-api/
https://github.com/ayrshare/ayrshare-social-media-api-claude-plugin
https://www.ayrshare.com/integrations/
https://www.ayrshare.com/use-cases/ai-startups/
https://www.reddit.com/r/mcp/comments/1sh269d/ayrshare_unofficial_mcp_server_enables_ai_agents/
https://aerostack.dev/mcp/aerostack/mcp-ayrshare
https://www.upload-post.com/pricing/
https://docs.upload-post.com/resources/pricing-and-limits
https://apis.io/plans/upload-post/upload-post-plans-pricing/
https://www.linkstartai.com/en/agents/upload-post
https://postgate.studio/blog/social-media-api-pricing-comparison
https://socialmediascheduler.cc/blog/social-media-posting-api-comparison
https://enjoy.dev/blog/best-social-media-api/
https://www.blotato.com/blog/upload-post-pricing
https://dev.to/perufitlife/9-social-media-posting-apis-compared-by-price-october-2026-read-from-each-pricing-page-509b
https://www.upload-post.com/mcp/
https://github.com/Upload-Post/upload-post-mcp
https://github.com/Upload-Post/upload-post-mcp/blob/main/README.md
https://docs.upload-post.com/guides/mcp-server-integration/
https://github.com/Upload-Post/upload-post-plugin
https://glama.ai/mcp/servers/MrSagarShah/upload-post-mcp
https://www.publishport.app/blog/can-claude-auto-publish-to-social-media
https://postqued.com/mcp
https://www.blotato.com/blog/best-social-media-mcp-servers
https://mcpservers.org/servers/upload-post/upload-post-mcp
https://bundle.social/pricing
https://bundle.social/pricing.md
https://thatmarketingbuddy.com/pricing/bundle-social
https://thatmarketingbuddy.com/software/bundle-social
https://schedulenchill.com/bundle-social-vs-schedule-and-chill
https://www.saasworthy.com/product/bundle-social
https://startupfortune.com/how-bundles-social-media-api-scales-without-per-account-fees/
https://www.capterra.com/p/10035746/bundle-social/
https://www.g2.com/products/bundle-social/reviews
https://www.blotato.com/pricing
https://www.blotato.com/pricing.md
https://www.plugkit.co/blog/blotato-pricing-api-alternatives/
https://www.upload-post.com/blotato-pricing/
https://aifunnelinsider.com/blotato-review/
https://www-blotato-com.translate.goog/blog/twitter-api-pricing?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc
https://ryandoser.com/blotato-review/
https://www.capterra.com/p/10053855/Blotato/
https://opentweet.io/blog/xmcp-x-official-mcp-server-guide
https://postory.io/blog/x-twitter-mcp
https://mcp.directory/blog/x-twitter-mcp-server
https://note.com/doerstokyo_kb/n/na5ec128601bf?hl=en
https://revive.social/social-media-mcp/
https://insightfulpipe.com/blog/best-x-twitter-mcp-servers
https://agentpedia.codes/blog/connect-x-hosted-mcp-server
https://devcommunity.x.com/t/announcing-the-hosted-x-mcp/269558
https://note.com/npaka/n/nae69651a3c79?hl=en
https://docs.x.com/tools/mcp
https://twitterapi.io/pricing
https://twitterapi.io/blog/x-api-cost-breakdown-2026
https://twitterapi.io/blog/twitterapi-vs-official-x-api
https://twitterapi.io/blog/twitter-api-pricing-2025
https://www.socialcrawl.dev/blog/best-twitter-x-data-apis-2026
https://dev.to/juehui_xiao_e03da76cad4b8/i-replaced-dead-x-search-with-twitterapiio-for-my-daily-trends-board-the-real-cost-and-the-429-12ee
https://twitterapi.io/blog/x-api-news
https://twitterapi.io/
https://www.twitterapis.com/twitter-api-pricing
https://twitterapi.io/blog/twitter-api-cost
https://info.bundle.social/api-reference/platforms/twitter
https://www.upload-post.com/bundle-social-alternative/
https://bundle.social/blog/x-api-pricing-2026-costs-limits
https://engagedigital.com/what-a-social-api-costs-to-run/
https://www.blotato.com/blog/blotato-vs-bundle-social
https://api.sorsa.io/blog/twitter-api-alternative
https://twitterapi.io/blog/social-media-api
https://www.blotato.com/research/social-media-agent-pricing
https://www.blotato.com/blog/social-media-api
https://www.blotato.com/tools/social-media-posting-limits
https://www.blotato.com/blog/how-to-use-blotato
https://www.blotato.com/social-media-api
https://www.blotato.com/ai-info
https://help.blotato.com/settings/social-accounts
https://www.blotato.com/blog/social-media-api-rate-limits
https://www.twittrapi.com/blog/cheapest-way-to-post-to-x-programmatically
https://www.blotato.com/blog/blotato-vs-postfast
https://cdn.syndication.twimg.com/widgets/followbutton
https://adapter.to/blog/how-much-does-typefully-cost
https://bestsocialmediascheduler.com/reviews/typefully
https://ilo.so/directory/typefully
https://support.typefully.com/en/articles/8717699-twitter-x-posting-limitations
https://www.usecarly.com/blog/claude-typefully-integration/
https://typefully.com/ai-agents
https://zapier.com/mcp/typefully
https://mcp.pipedream.com/app/typefully
https://docs.postiz.com/providers/x-twitter
https://postiz.com/channels/x
https://postiz.com/blog/direct-postiz-integration-oauth-api
https://postiz.com/codex/x
https://postiz.com/claude-code/x
https://github.com/gitroomhq/postiz-app
https://apnews.com/article/korea-google-apple-app-tech-fine-aedd6061a785441380d4daaf7afbc43d
https://apnews.com/article/ai-layoffs-meta-microsoft-barclays-300bd6c2585b4a6996c2ac553d870055
https://www.itsolutionstuff.com/post/how-to-retrieve-number-of-twitter-followers-count-without-authentication-or-api-example.html
https://api.sorsa.io/playground/recent-followers
https://xapis.dev/tools/twitter-follower-count-checker
https://unifapi.com/tools/x-followers-viewer
https://pulse.walls.sh/blog/twitter-follower-count
https://xapis.dev/tools
https://twexapi.io/twitter-viewer
https://twitterapi.io/blog/twitter-follower-count-api-tutorial
https://www.postforme.dev/
https://www.postforme.dev/integrations/x
https://www.postforme.dev/pricing
https://www.postforme.dev/social-media/posting
https://buffer.com/resources/social-media-api-multi-platform-posting/
https://www.blotato.com/blog/best-social-media-api-tools
https://www.blotato.com/blog/twitter-api-alternatives
https://github.com/xdevplatform/xurl
https://github.com/xdevplatform/xurl/blob/main/README.md
https://docs.x.com/tools/xurl
https://github.com/xdevplatform/xurl/blob/main/SKILL.md
https://github.com/pasogott/xurl
https://github.com/BofAI/xurl
https://pkg.go.dev/github.com/xdevplatform/xurl
https://github.com/mangeshraut712/xurl
https://pkg.go.dev/github.com/kzu/xurl
https://github.com/azaj01/xurl
https://mcp.typefully.com/mcp
https://cdn.syndication.twimg.com/widgets/followbutton/info.json?lang=en&screen_names=rem
https://kaspars.net/blog/twitter-follower-count-without-api
https://stackoverflow.com/questions/66285850/get-twitter-followers/66286006
https://stackoverflow.com/questions/66285850/get-twitter-followers
https://www.pcbway.com/blog/PCB_Design_Tutorial/IoT_Twitter_Follower_Tracker_and_Status_Notifier.html
https://remysharp.com/2019/04/02/twitter-without-tracking
https://git.hacksrus.org/samw/twitter-prometheus
https://blog.devgenius.io/easiest-way-to-track-any-users-twitter-follower-count-b30661c0c16a?gi=31993fc55c15
https://cdn.syndication.twimg.com/widgets/followbutton/info.json?screen_names=%25s
https://buffer.com/resources/buffer-features/
https://www.plugkit.co/blog/social-media-mcp-servers-compared/
https://www.blotato.com/blog/metricool-vs-buffer
https://apnews.com/article/syria-israel-arrests-raid-bufferzone-quneitra-325d1f9cc98096e54b5404f8f97873b0
https://typefully.com/changelog
https://typefully.com/changelog/reply-and-quote-tweets-56
https://x.com/typefully/status/1992993358416138661
https://typefully.featurebase.app/
https://x.com/typefully
https://support.typefully.com/en/articles/8717356-share-your-drafts-get-comments
https://support.typefully.com/en/articles/8861989-why-does-my-published-twitter-x-post-appear-in-my-replies-tab
https://typefully.com/
https://typefully.com/typefully/notifications-DhXw7PL
https://drive.google.com/file/d/1NEbK4EMZ0YcyVZb1vGDJ6F2a86uX0_c6/view?usp=drivesdk
https://opentweet.io/how-to/fix-x-api-like-follow-endpoint-removed
https://x.com/XDevelopers/status/2044919377544261979
https://api.sorsa.io/blog/migrate-from-twitter-api
https://socialnexis.com/guides/x-rate-limits-plan-tiers
https://devcommunity.x.com/t/x-api-pricing-update-owned-reads-now-0-001-other-changes-effective-april-20-2026/263025
https://taupe.site/en/entry/x-api-posting-policy-review/
https://x.com/XDevelopers/status/2044919375153483812
https://docs.postiz.com/general/analytics
https://postiz.com/blog/social-media-engagement-metrics&gsessionid=ttzFc5h-zSancNTCIpWYj4d6zzBWfrK8ySlRFRysUP5yKNwk1ms5qg
https://postiz.com/blog/content-performance-analysis
https://clawhub.ai/nevo-david/postiz
https://aiskill.market/skills/postiz
https://postiz.info/
https://postiz.com/
https://github.com/gitroomhq/postiz-agent
https://postiz.com/blog/social-media-analytics-reporting
https://cdn.syndication.twimg.com/widgets/followbutton/info.json?lang=en&screen_names=nasa
https://github.com/gitroomhq/postiz-app/discussions/542
https://github.com/sype/postiz-app/pull/11
https://theailazyleader.com/blog/install-postiz-open-source-social-scheduler
https://help.blotato.com/tips-and-tricks/social-platform-requirements
https://help.blotato.com/api/media
https://help.blotato.com/support/errors
https://buffer.com/changelog/community-permissions-team-management-updates
https://www.getcreator.io/learn/typefully-pricing-in-2026-what-you-actually-pay
https://www.blotato.com/blog/typefully-alternatives
https://hookpost.hookstep.in/alternatives/typefully
https://devcommunity.x.com/t/restricting-programmatic-replies-does-this-affect-replying-to-itself/257955
https://roboin.io/article/en/2026/02/24/x-limits-api-based-automated-replies-to-combat-engagement-farming/
https://www.tekedia.com/x-restricts-programmatic-bot-replies-to-boost-genuine-users-interaction/
https://devcommunity.x.com/t/update-to-reply-behavior-in-x-api-v2-restricting-programmatic-replies/257909
https://docs.x.com/x-api/posts/manage-tweets/introduction
https://reachmore.co/blogs/auto-reply-twitter-2026
https://phemex.com/news/article/x-developers-restrict-api-to-combat-automated-spam-replies-62263
https://tweetreplier.com/blog/which-apps-have-access-to-your-x-account
https://devcommunity.x.com/t/unable-to-post-tweet-with-mention/258356
https://devcommunity.x.com/t/mentions-blocked-in-regular-posts-not-replies-contradicts-announcement/258399
https://devcommunity.x.com/t/quote-tweet-id-no-longer-works-for-cross-account-quote-posts-undocumented-change/259927
https://devcommunity.x.com/t/unable-to-post-tweet-with-mention/258356/8
https://devcommunity.x.com/t/unable-to-post-tweet-with-mention/258356?page=2
https://devcommunity.x.com/t/programmatic-replies-restriction/258028
https://docs.x.com/x-api/introduction
https://twitterapi.io/blog/twitter-counter-api-guide
https://twitterapi.io/blog/twitter-profile-analytics-api-guide
https://twitterapi.io/blog/twitter-api-endpoints
https://api.sorsa.io/blog/twitter-analytics-api
https://postproxy.dev/blog/x-api-pricing-2026/
https://docs.x.com/x-api/fundamentals/metrics
https://devcommunity.x.com/t/do-non-public-metrics-and-organic-metrics-return-on-get-2-tweets-for-my-own-posts-via-a-read-only-user-context-connector/276441
https://www.getxapi.com/blogs/x-twitter-api-object-reference
https://twitterapi.io/blog/twitter-stats-api-guide
https://social-api.ai/blog/social-media-analytics-api
https://www.postzen.dev/blog/social-media-analytics-api
https://typefully.com/blog/x-analytics-ai-agents
https://www.postmcpai.com/social-media-analytics-api
https://api.x.com/mcp
https://mcp.postiz.com/mcp-oauth-dynamic
https://devcommunity.x.com/t/undocumented-postcreate-usage-for-media-uploads-please-clarify-pay-per-use-billing/274646
https://devcommunity.x.com/t/being-charged-much-more-than-expected/268033
https://bundle.social/blog/x-api-post-tweet
https://www.upload-post.com/platforms/x/
https://opentweet.io/
https://bestsocialmediascheduler.com/reviews/opentweet
https://www.sofarbot.com/stories/opentweet-branko-3501-mrr
https://ottermind.ai/skills/opentweet-x-poster
https://voicemoat.com/blog/what-is-opentweet
https://www.capterra.com/p/10038186/OpenTweet/
https://apnews.com/article/trump-arch-military-complex-14eaf1a088ef04e24b5a15a971d12eee
https://apnews.com/article/student-loan-caps-91ddf9904c1d1ffffacddcc6ffcce141
https://postproxy.dev/pricing
https://opentweet.io/pricing