# DR26-11-12-HUB-01-0234 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-11-12-HUB-01-0234 — Коннекторы под дыры флота: Luma, Facebook/Messenger, X, YC WaaS, WhatsApp

## Итоговая матрица

Проверка выполнена по состоянию на **12 ноября 2026 года**. Под «официальным» ниже я считаю только API/MCP, опубликованные самим владельцем платформы; API для организаторов, Pages и Business-аккаунтов не считаются заменой личному пользовательскому доступу.

| item | official option | price | ToS risk | best non-official | recommendation |
|---|---|---|---|---|---|
| **Luma** | **Частично.** Официальный remote MCP `https://mcp.luma.com` доступен любому Luma-аккаунту через OAuth и умеет искать публичные события, показывать события, которые пользователь посещает/хостит, и полноценно управлять собственными событиями. Но в опубликованном списке MCP-tools **нет действия “зарегистрировать текущего пользователя как attendee”**. Public API требует Luma Plus и является calendar/organizer-scoped: управление событиями, гостями и регистрациями организатора, а не attendee self-RSVP. Следовательно, discovery — official; attendee registration — **нет подтверждённого official endpoint, unknown/unsupported**. citeturn578185search0turn578185search1 | MCP — **$0 extra**, Plus не нужен. Public API — Luma Plus **$69/мес помесячно или $59/мес при annual billing** на calendar; API для вашей задачи регистрации attendee всё равно не решает дыру. citeturn578185search0turn887762search3 | **Средний.** Terms требуют использовать только “publicly supported interfaces”; scripted browser прямо не назван запрещённым, поэтому утверждать, что любой Playwright-клик сам по себе нарушает ToS, нельзя. Однако private endpoints/reverse-engineered calls уже явно хуже по позиции. Публичного 2025–2026 кейса санкций именно за attendee browser automation я не нашёл: **unknown**. citeturn578185search4 | [`adelaidasofia/luma-mcp`](https://github.com/adelaidasofia/luma-mcp), **0★**, 83 commits; exact last-commit date **unknown** из доступной GitHub-выдачи. Он использует официальный host-side API и потому **не решает attendee registration**. citeturn125296search0 | **Official MCP для discovery + keep browser только для RSVP.** Решающий факт: собственный MCP Luma уже закрывает поиск, но published tool surface не содержит attendee self-registration. citeturn578185search0 |
| **Facebook + Messenger, personal profile** | **Нет подтверждённого official API для требуемой работы.** Messenger Platform — **Page-to-user**, требует Facebook Page, Page access token и `pages_messaging`; Conversations API также Page-scoped, а Advanced Access нужен для разговоров с людьми без роли в app/Page. Это не личные Messenger DMs. Для комментариев Meta Graph API поддерживает Page workflows, но официального 2026 endpoint, который позволял бы агенту читать/отвечать на комментарии **от имени personal profile**, я не смог верифицировать: **unknown/unsupported**, не путать с Page API. citeturn632238search0turn632238search2 | Для требуемого personal-profile rail — **N/A**, потому что официального продукта нет. Отдельную per-call цену Meta для Page Messenger/Graph API в доступных официальных 2026 материалах я не смог верифицировать: **unknown**; в любом случае Page API не заменяет personal account. citeturn632238search0turn632238search2 | **Высокий для чтения/скрейпинга; medium-high для UI automation в целом.** Meta прямо считает automation для извлечения Facebook data без разрешения нарушением условий и в 2025 описывала анти-скрейпинг-системы, специально выявляющие автоматизированное поведение. Конкретного публичного 2025–2026 кейса, где личный аккаунт был доказанно забанен именно за agent-driven browser replies/DMs, я не нашёл: **unknown**. citeturn632238search1turn274410search3 | [`m008v/fbchat-v2`](https://github.com/m008v/fbchat-v2): unofficial personal-account/cookie library, E2EE Messenger, внутренние GraphQL/MQTT механизмы; latest release **v2.3.2, 15 Sep 2026**, 63 forks, GitHub stars **unknown** из доступной выдачи, repository updated **15 Sep 2026**. Сам проект предупреждает, что это не официальный SDK. citeturn281169search3turn281169search4turn125296search27 | **Keep browser, но минимизировать и не масштабировать; third-party bridge — только если принимаете ban/session risk.** Решающий факт: официальная Meta automation surface начинается с **Page**, а вам нужен **personal profile**. citeturn632238search0turn632238search2 |
| **X** | **Да, практически полностью.** X API v2 официально покрывает posting/replies, чтение Posts, mentions/search; Search Recent доступен всем developers, Full Archive — pay-per-use/Enterprise. Нужны approved developer account + Project/App. Для user-context OAuth 2.0 PKCE ключевые scopes: `tweet.read`, `tweet.write`, `users.read`, обычно `offline.access` для refresh token; X отдельно публикует `dm.read`/`dm.write` для DMs. citeturn621919view0turn621919view2turn737471search5 | **Pay-per-use credits, старых $200/$5,000 tiers для новой модели больше нет.** Post read **$0.005/resource**; User read **$0.010/resource**; Post create **$0.015/request**; summoned reply/quote, где пользователь был mentioned, **$0.010/request**; content create with URL **$0.200/request**. Search не имеет отдельного опубликованного “per query” тарифа: стоимость возникает по returned Posts — напр. 100 search results = **$0.50** только по Post reads, плюс возможные User resources. Для 150 posts + 1,500 replies + 15,000 Post reads: **$92.25/мес**, если все replies qualify как summoned (`2.25 + 15 + 75`), либо **$99.75/мес**, если replies тарифицируются как обычный Post Create (`2.25 + 22.50 + 75`). URL-content, User reads и прочие resources сверху. citeturn366411search0turn366411search1turn621919view2 | **Очень высокий для браузерной автоматизации.** X прямо запрещает “non-API-based forms of automation, such as scripting the X website” и предупреждает о permanent suspension; ToS от **9 Oct 2026** запрещают automated access/search вне опубликованных interfaces. Конкретный 2026 enforcement: 9 Feb X suspended managed X app credentials Composio “due to platform policy enforcement”; это **API-app enforcement, не browser-automation case**, но показывает реальный platform kill-switch. citeturn737471search0turn737471search1turn737471search3 | [`IvanBBaev/x-mcp`](https://github.com/IvanBBaev/x-mcp), **1★**, 41 typed tools, OAuth2 PKCE, локальный credit budget, только официальный X API; exact last-commit date **unknown** из доступной GitHub-выдачи. citeturn125296search1turn125296search16 | **Official API; browser rail drop.** Решающий факт: API теперь стоит порядка **$92–100/мес** на вашем workload, а scripting x.com прямо запрещён и может закончиться permanent suspension. citeturn366411search0turn737471search1 |
| **YC Work at a Startup** | **Да — новый официальный Bookface MCP закрывает applications.** YC superseded standalone `waas-mcp` hosted server’ом `https://api.ycombinator.com/v1/mcp`; OAuth через YC account, MCP access должен быть enabled, tools зависят от Bookface role/access. YC прямо пишет, что для **eligible candidate accounts** MCP умеет искать вакансии, управлять candidate profile и **apply to YC startups**. В опубликованном описании candidate-side founder messaging отдельно не заявлен; `hiring.get_candidate_messages` / `hiring.send_candidate_message` относятся к hiring-side workflow. Поэтому **candidate application = yes; post-application founder messaging = unknown**. citeturn492367search0 | Цена Bookface MCP для candidate accounts в официальных материалах не опубликована: **unknown**. Отдельного per-application fee я не смог верифицировать. citeturn492367search0 | **Высокий для browser/private-API automation**, особенно после появления официального MCP. YC Terms запрещают data mining, robots, scraping и получение материалов “through any means not intentionally made available”; YC может suspend/terminate аккаунт. Публичный 2025–2026 named enforcement case именно за WaaS browser automation я не нашёл: **unknown**. citeturn166743view0 | Надёжного third-party кандидата с одновременно подтверждаемыми stars + last commit + хорошей compliance-позицией я не нашёл: **unknown**. Старый [`yc-software/waas-mcp`](https://github.com/yc-software/waas-mcp) — **официальный, не third-party**, 20★, updated **25 Sep 2026**, но уже помечен как superseded и больше не поддерживается. citeturn125296search23turn492367search0 | **Official Bookface MCP.** Решающий факт: YC теперь сам даёт eligible candidate accounts инструмент **apply to YC startups**; продолжать Playwright для applications после этого бессмысленно и юридически хуже. citeturn492367search0turn166743view0 |
| **WhatsApp personal** | **Нет.** Официальный WhatsApp Cloud API — это **WhatsApp Business Platform**: WABA/business assets, registered business phone number/system-user auth; permissions `whatsapp_business_management` и `whatsapp_business_messaging`. Это не API для чтения/отправки сообщений из личного WhatsApp Messenger аккаунта. Официального personal-account MCP/API, эквивалентного WhatsApp Web linked-device session для агента, я не нашёл: **unknown/unsupported**. citeturn939517search0turn939517search2turn939517search4 | Personal — **N/A**. Business Platform pricing не является заменой вашему use case; точную 2026 country/category rate для абстрактного “personal messaging” корректно привести нельзя. **Unknown/not comparable.** citeturn939517search0 | **Очень высокий.** WhatsApp Terms запрещают impermissible automated use, auto-messaging и неавторизованное создание API-подобных сервисов; Messaging Guidelines от **23 Sep 2026** прямо относят unofficial clients и harmful automation к adversarial behavior. Help Center отдельно говорит, что linking via unofficial app/site может привести к temporary/permanent ban. В H1 2025 WhatsApp забанил >6.8 млн scam-center accounts — реальный enforcement масштаба платформы, но **не доказательство банов именно whatsmeow/MCP bridges**; bridge-specific 2025–26 case: **unknown**. citeturn939517search5turn939517search6turn939517search7turn939517search10 | [`verygoodplugins/whatsapp-mcp`](https://github.com/verygoodplugins/whatsapp-mcp), **293★**, 190 forks; maintained fork of original lharries project; latest verified release **0.7.0, 23 Sep 2026**, exact last-commit date **unknown**. Он непосредственно даёт personal read/send через linked-device bridge, то есть fit высокий, compliance низкий. citeturn281169search0turn281169search1turn281169search2 | **Keep browser/official WhatsApp Web for low-volume human-supervised actions; drop unofficial bridge for unattended fleet use.** Решающий факт: Help Center прямо связывает unofficial linked clients с temporary/permanent bans, а у вашего bridge уже ~18% ошибок. citeturn939517search7turn939517search9 |

## Что реально покрывают официальные интерфейсы

### Luma: MCP появился, но RSVP-дырку не закрыл

Luma в 2026 заметно изменила картину: у неё есть **официальный remote MCP** с OAuth, доступный без Luma Plus. Он достаточно широкий: поиск публичных событий по ключевым словам, географии и датам; получение списка событий, которые пользователь посещает или организует; управление event/calendar assets; работа с гостями, билетами, approval, приглашениями и blasts для событий, которыми пользователь управляет. citeturn578185search0

Критическая граница проходит по роли пользователя. Все write-операции вокруг registrations в документации описаны с позиции **host/manager**: approve, decline, waitlist, invite, modify tickets. Операции “найти чужое событие и зарегистрировать текущий аккаунт как attendee” в опубликованном MCP surface нет. Поэтому для ваших 204 navigations / 66 sessions правильная архитектура — не «браузер на всё», а **MCP discovery → браузер только на registration form**. citeturn578185search0

Public API этого не исправляет. Luma описывает его как API для управления **your events and calendars**; ключ создаётся на конкретном calendar, а Luma Plus требуется именно для этого calendar. citeturn578185search1

Есть небольшое противоречие внутри собственной документации Luma: общая страница Integrations всё ещё называет MCP “read-only”, тогда как специализированная MCP page перечисляет множество write operations. Для capability analysis более конкретная MCP page является сильнейшим текущим источником, но конфликт стоит иметь в виду при автоматической генерации policy. citeturn578185search0turn578185search2

### Facebook/Messenger: personal identity — принципиальная граница

Официальная Messenger Platform в 2026 по-прежнему построена вокруг **Facebook Pages**. Meta требует Page access token, полученный от человека с соответствующей Page task, и `pages_messaging`; Conversations API добавляет `pages_manage_metadata` и `pages_read_engagement`, а Advanced Access нужен для взаимодействия с людьми, которые не имеют роли в app/Page. citeturn632238search0turn632238search2

Это нельзя экстраполировать на личный Messenger inbox. Page-scoped ID и Page access token не становятся персональным OAuth-доступом к личным DMs пользователя. Аналогично, Page moderation/comment APIs не дают оснований утверждать, что можно через Graph API читать и отвечать на comments **от имени personal profile**. В доступной официальной 2026 документации я не нашёл такого endpoint; поэтому статус именно этой capability — **unknown/unsupported**, а не «Page API почти подходит». citeturn632238search0turn632238search2

`fbchat-v2` закрывает техническую дыру именно потому, что делает то, чего официальный API не предлагает: authenticates as a real Facebook user via cookies и использует внутренние web/MQTT/GraphQL механизмы. Это отличный индикатор product fit и одновременно плохая compliance-характеристика. Релиз 2.3.2 вышел 15 сентября 2026; предыдущие релизы добавляли E2EE и timeline operations, включая private GraphQL mutations. citeturn281169search3turn125296search27

### YC WaaS: к ноябрю 2026 browser applications уже технический долг

Это наиболее существенное изменение среди пяти платформ. YC официально пишет, что standalone `yc-software/waas-mcp` **superseded** новым hosted Bookface MCP; подключение идёт через Streamable HTTP + OAuth на YC account. Для **eligible candidate accounts** прямо заявлены job search, candidate-profile management и application to YC startups. citeturn492367search0

Это означает, что из вашего старого browser demand как минимум стадия **find → inspect → apply** теперь имеет официальный маршрут. Отдельно важно не смешивать две стороны рынка: `hiring.send_candidate_message` и `hiring.get_candidate_messages` перечислены в recruiter/founder workflow. Я не нашёл явной формулировки, что candidate account может через MCP инициировать/вести founder messaging после application. Для этого capability корректный статус на 12 ноября — **unknown**. citeturn492367search0

YC Terms при этом очень недвусмысленны относительно старого обходного пути: запрещены data mining, robots, scraping и иные способы получения материалов, которые сайт не сделал намеренно доступными. После появления собственного candidate MCP аргумент «иного машинного интерфейса нет» становится ещё слабее. citeturn166743view0

### WhatsApp: Business API не превращается в personal API

Cloud API — официальный и зрелый, но это другой продукт. Для него нужны WhatsApp Business assets/WABA, registration business phone number и access token с `whatsapp_business_messaging`; management operations используют `whatsapp_business_management`. citeturn939517search0turn939517search2turn939517search4

Он не позволяет взять обычный личный WhatsApp account и использовать его как programmable personal inbox. Поэтому миграция вашего personal bridge на Cloud API — не «сменить транспорт»: это сменить **identity/account model** на бизнес-номер и business messaging semantics. citeturn939517search0turn939517search2

## X: цена, credit depletion и реальный месячный budget

X к ноябрю 2026 использует consumption-based credit model: в Developer Console заранее приобретаются credits, после чего списание идёт по фактическим ресурсам или write requests. X прямо сравнивает это с прежней моделью фиксированных **$200 / $5,000 monthly tiers** и заявляет отсутствие fixed monthly fee и monthly object caps в новой схеме. citeturn366411search0turn366411search1

Текущая официальная таблица тарификации:

| billed resource | ставка 12 Nov 2026 | как считать |
|---|---:|---|
| Post read | **$0.005 / resource fetched** | 500 posts returned = $2.50 |
| User read | **$0.010 / resource fetched** | expansions/user lookups могут добавлять стоимость |
| DM Event read | **$0.010 / resource** | не относится к вашему заданному read workload, если DMs не читаются |
| Post create | **$0.015 / request** | обычный новый post; обычные replies могут попадать сюда |
| Content create | **$0.010 / request** | X определяет это как summoned reply/quote, т.е. reply на post, где вас mentioned |
| Content create with URL | **$0.200 / request** | отдельная дорогая write-категория для URL content |

Все ставки — официальная текущая pricing page. citeturn366411search0

Для вашего сценария **150 posts + 1,500 replies + 15,000 reads / 30 дней** арифметика без User reads:

`150 × $0.015 = $2.25` на собственные posts.  
`15,000 × $0.005 = $75.00` на Post reads.  
Если все 1,500 replies являются summoned replies: `1,500 × $0.010 = $15.00`. Итого **$92.25/month**. citeturn366411search0

Если 50 replies/day — это обычные replies на произвольные posts, а не ответы на mentions, безопаснее бюджетировать их как обычный create: `1,500 × $0.015 = $22.50`, итого **$99.75/month**. Это не расхождение в тарифе, а неопределённость классификации вашего конкретного reply mix. citeturn366411search0

**Search не тарифицируется как отдельный “search request” в опубликованной таблице.** Recent Search возвращает до 100 Posts/request; биллинг pricing page определяет Post Read именно per resource fetched. Поэтому запрос, вернувший 100 Posts, даёт примерно **$0.50 Post-read cost**, а не некую отдельную `$X/search` ставку; expansions могут добавить User-resource charges. Recent Search доступен всем developers, Full Archive — pay-per-use/Enterprise. citeturn621919view2turn366411search0

HTTP `402 "credits depleted"` из вашего лога согласуется с моделью prepaid credits: Developer Console позволяет покупать credits и смотреть consumption в реальном времени. Но отдельную официальную 2026 error-code page, которая формально определяет именно строку `"credits depleted"` и grace behavior, я не смог верифицировать; эта часть — **unknown beyond the returned error message**. citeturn366411search1

Для production rail я бы ставил четыре предохранителя: собственный soft budget на session/day/month; предупреждение при 50/75/90% кредитного budget; прекращение автоматических retry на 402; и разделение write budget и read/search budget, чтобы агрессивный search не мог обнулить posting. Такое решение особенно естественно с MCP вроде `IvanBBaev/x-mcp`, который уже содержит локальный session credit budget, хотя сам сервер сторонний. citeturn125296search1

OAuth для unattended agent хорошо ложится на confidential **Automated App or bot**; X перечисляет этот app type как confidential client. Для posting нужны `tweet.write`; чтения — `tweet.read`; user objects — `users.read`; `offline.access` выдаёт refresh token и снимает необходимость интерактивной реавторизации каждые два часа. citeturn621919view0

## ToS и фактический enforcement

Риск здесь полезно разделять на три уровня: **явно запрещено в тексте**, **технически детектируется**, и **есть публичный 2025–2026 enforcement case для похожего паттерна**. Это не одно и то же.

**X** — самый однозначный случай. Automation Rules, обновлённые в апреле 2026, буквально запрещают scripting x.com и предупреждают о permanent suspension; Terms с 9 октября запрещают automated access/search вне published interfaces. Поэтому ваш browser posting/read rail на X не просто ненадёжен — он находится непосредственно в запрещённой категории. citeturn737471search0turn737471search1 Конкретный enforcement в 2026 тоже есть: X 9 февраля suspended managed X credentials Composio из-за platform-policy enforcement, после чего Composio пришлось переводить пользователей на их собственные developer credentials. Это не browser case, поэтому нельзя использовать его как доказательство, что X обязательно забанит ваш браузерный агент; зато это реальное подтверждение, что X оперативно отключает automation integrations. citeturn737471search3

**WhatsApp** почти столь же однозначен. Terms запрещают unauthorized automated use и auto-messaging, а Messaging Guidelines от 23 сентября 2026 прямо называют unofficial clients adversarial behavior. Help Center ещё жёстче: linking account через unofficial app/site может закончиться temporary или permanent ban. citeturn939517search5turn939517search6turn939517search7 WhatsApp действительно массово применяет account enforcement: в первой половине 2025 было заблокировано более 6.8 млн scam-center accounts. Но это были scam accounts, а не пользователи `whatsmeow`; публичный named case 2025–2026, где бан однозначно связан именно с MCP/whatsmeow linked-device bridge, я не нашёл — **unknown**. citeturn939517search10

**YC** формально запрещает robots/scraping/data mining и доступ к material через средства, которые не были намеренно предоставлены сайтом. Это делает browser/API reverse engineering рискованнее обычного UI clicking. YC прямо оставляет за собой suspension/termination. Но публичного 2025–2026 enforcement report по WaaS candidate automation я не нашёл — **unknown**. citeturn166743view0

**Facebook/Messenger**: Meta давно и явно считает unauthorized automated data collection нарушением, а в феврале 2025 описала системы Anti-Scraping team, которые специально ищут scraping vectors и patterns, отличающие автоматизацию от normal use. citeturn632238search1turn274410search3 Это хорошо подтверждает detection capability, но не даёт чистого causal case для вашей конкретной комбинации “read own comments + reply + DM via personal account”. Такой публичный 2025–2026 case я не нашёл — **unknown**.

**Luma**: Terms говорят, что доступ должен идти только через publicly supported interfaces. Это достаточное основание не использовать private endpoints, но слишком слабое, чтобы уверенно утверждать, что Playwright поверх обычной registration page сам по себе запрещён. Поэтому риск здесь ниже, чем у X/WhatsApp/YC. Публичного 2025–2026 enforcement case именно против browser attendee registration я не нашёл — **unknown**. citeturn578185search4

## Сторонние MCP/SDK: пригодность важнее числа stars

GitHub stars здесь плохо коррелируют с production suitability: самые правильные по compliance проекты могут быть новыми и иметь 0–1★, тогда как personal-account bridges набирают сотни stars именно потому, что обходят отсутствие официальной capability. Поэтому я бы взвешивал **fit → auth model → maintenance → policy risk → stars**, а не наоборот.

Для **Luma** `adelaidasofia/luma-mcp` технически аккуратный: официальный Luma API, draft/confirm writes, audit log, 0★ и 83 commits. Но его capability — organizer workflows, а значит ваш attendee gap остаётся. citeturn125296search0

Для **Facebook personal** `m008v/fbchat-v2` — лучший найденный по actual fit и свежести: latest release 15 Sep 2026, personal cookies, E2EE Messenger и активная работа с внутренними Facebook mechanisms. Именно использование реального user session делает его одновременно полезным и рискованным; оно не превращается в sanctioned Meta API от того, что библиотека maintained. citeturn281169search3turn125296search27

Для **X** `IvanBBaev/x-mcp` архитектурно соответствует fleet use: 41 typed tools, официальный X API v2, OAuth2 PKCE, cost-aware budget и отсутствие scraping/private API. Его всего 1★, поэтому ecosystem maturity небольшая, но policy fit лучше практически любого browser-based X MCP. citeturn125296search1

Для **YC** искать третий слой теперь мало смысла. Официальный `yc-software/waas-mcp` имел 20★ и был updated 25 Sep 2026, но YC сам объявил его superseded hosted Bookface MCP. Это сильный сигнал не строить новый dependency на community browser wrapper. citeturn125296search23turn492367search0

Для **WhatsApp** `verygoodplugins/whatsapp-mcp` — наиболее убедительный personal bridge из найденных: 293★, 190 forks, активный maintained fork, v0.7.0 от 23 Sep 2026. По engineering health он заметно сильнее многих одноразовых MCP wrappers. Но он остаётся неофициальным linked-device client, а значит высокая maintenance quality не устраняет WhatsApp policy risk. citeturn281169search0turn281169search1turn939517search7

Там, где GitHub не отдавал exact last-commit timestamp в доступной публичной индексированной выдаче, я сознательно написал **unknown**, а не подменял дату последнего release/update датой коммита.

## Операционная рекомендация для флота

На ваших объёмах оптимальный разрез не совпадает с «поставить MCP на всё».

**Luma — `official MCP + browser RSVP`.** MCP должен съесть discovery/listing workload; браузер оставить узким, stateful действием только на регистрационную форму. Это уменьшает количество browser navigations, сохраняя capability, которой официальный MCP пока не публикует. citeturn578185search0

**Facebook/Messenger personal — `keep browser, rate-limit, human-equivalent cadence`; не переводить основной аккаунт на unofficial library как unattended rail.** Технически `fbchat-v2` лучше браузера по latency/reliability, но auth model хуже с точки зрения platform risk: cookies + internal protocols. Если аккаунт важный, browser с малой частотой операций и human review — менее агрессивный компромисс, хотя официально sanctioned personal automation rail всё равно отсутствует. citeturn632238search0turn125296search27

**X — `official API only`.** При вашем объёме экономия на browser automation не имеет смысла: примерно $92–100/месяц базового API usage — дешёвая страховка против explicit ToS violation и parked sessions. Разнести read/write credits, поставить hard stop на 402 и запретить fallback на website scripting. citeturn366411search0turn737471search1

**YC WaaS — `official Bookface MCP`.** Applications уже officially supported. Browser fallback допустим только для capability, которую hosted MCP действительно не exposes — прежде всего candidate-side founder messaging, пока она не будет подтверждена в tool schema. citeturn492367search0

**WhatsApp personal — `drop unofficial bridge for unattended production`; оставить официальный клиент/WhatsApp Web с человеком в цикле либо мигрировать коммуникационный use case на Business Platform, если бизнес-identity приемлема.** При ~1,100 calls и ~18% errors вы уже платите существенный reliability tax, а policy risk не теоретический: WhatsApp отдельно предупреждает о bans за unofficial linked clients. citeturn939517search7turn939517search9

Итого по пяти дырам: **X и YC уже следует полностью вывести из browser automation; Luma — сократить browser до RSVP; Facebook personal остаётся настоящей API-дырой; WhatsApp personal — API-дырой с особенно плохим risk/reward для unofficial bridge.** Это оставляет браузер как exception layer, а не универсальный transport, что при исходной ~10% browser failure rate должно убрать значительную часть неустойчивости именно там, где платформы к ноябрю 2026 уже дали официальные rails.

## Sources (safe_urls)
https://about.fb.com/news/2021/04/how-we-combat-scraping/amp/
https://about.fb.com/news/2021/04/how-we-combat-scraping/amp/?utm_source=chatgpt.com
https://about.fb.com/news/2025/08/new-whatsapp-tools-tips-beat-messaging-scams/
https://about.fb.com/news/2025/08/new-whatsapp-tools-tips-beat-messaging-scams/?utm_source=chatgpt.com
https://composio.openstatus.dev/events
https://composio.openstatus.dev/events?utm_source=chatgpt.com
https://developer.x.com/
https://developer.x.com/?lang=en
https://developer.x.com/?lang=en&utm_source=chatgpt.com
https://developer.x.com/?utm_source=chatgpt.com
https://docs.x.com/fundamentals/authentication/oauth-2-0/authorization-code
https://docs.x.com/x-api/posts/search/introduction
https://engineering.fb.com/2025/02/18/security/protecting-user-data-through-source-code-analysis/?trk=article-ssr-frontend-pulse_little-text-block
https://engineering.fb.com/2025/02/18/security/protecting-user-data-through-source-code-analysis/?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com
https://faq.whatsapp.com/1217634902127718/?locale=pt_PT
https://faq.whatsapp.com/1217634902127718/?locale=pt_PT&utm_source=chatgpt.com
https://faq.whatsapp.com/378279804439436/?cms_platform=android&locale=el_GR
https://faq.whatsapp.com/378279804439436/?cms_platform=android&locale=el_GR&utm_source=chatgpt.com
https://github.com/IvanBBaev/x-mcp
https://github.com/IvanBBaev/x-mcp/blob/main/docs/01-api-landscape.md
https://github.com/IvanBBaev/x-mcp/blob/main/docs/01-api-landscape.md?utm_source=chatgpt.com
https://github.com/IvanBBaev/x-mcp?utm_source=chatgpt.com
https://github.com/adelaidasofia/luma-mcp
https://github.com/adelaidasofia/luma-mcp?utm_source=chatgpt.com
https://github.com/m008v/fbchat-v2
https://github.com/m008v/fbchat-v2/releases
https://github.com/m008v/fbchat-v2/releases?utm_source=chatgpt.com
https://github.com/m008v/fbchat-v2?utm_source=chatgpt.com
https://github.com/orgs/yc-software/repositories
https://github.com/orgs/yc-software/repositories?utm_source=chatgpt.com
https://github.com/topics/facebook-messenger-chatbot?o=desc&s=forks
https://github.com/topics/facebook-messenger-chatbot?o=desc&s=forks&utm_source=chatgpt.com
https://github.com/verygoodplugins/whatsapp-mcp
https://github.com/verygoodplugins/whatsapp-mcp/blob/main/CHANGELOG.md
https://github.com/verygoodplugins/whatsapp-mcp/blob/main/CHANGELOG.md?utm_source=chatgpt.com
https://github.com/verygoodplugins/whatsapp-mcp/releases
https://github.com/verygoodplugins/whatsapp-mcp/releases?utm_source=chatgpt.com
https://github.com/verygoodplugins/whatsapp-mcp?utm_source=chatgpt.com
https://github.com/yc-software/waas-mcp/blob/main/README.md
https://github.com/yc-software/waas-mcp/blob/main/README.md?utm_source=chatgpt.com
https://help.luma.com/p/integrations
https://help.luma.com/p/integrations?utm_source=chatgpt.com
https://help.luma.com/p/luma-api
https://help.luma.com/p/luma-api?utm_source=chatgpt.com
https://help.luma.com/p/luma-vs-splash
https://help.luma.com/p/luma-vs-splash?utm_source=chatgpt.com
https://help.luma.com/p/mcp
https://help.luma.com/p/mcp?utm_source=chatgpt.com
https://help.x.com/en/rules-and-policies/x-api
https://help.x.com/en/rules-and-policies/x-api?utm_source=chatgpt.com
https://help.x.com/en/rules-and-policies/x-automation
https://help.x.com/en/rules-and-policies/x-automation?utm_source=chatgpt.com
https://luma.com/terms
https://luma.com/terms?utm_source=chatgpt.com
https://www.postman.com/meta/messenger-platform-api/documentation/iyp204x/messenger-platform-api?entity=folder-22794852-9fcfb7b7-1d80-422d-b5c8-eb2b1003d4fc
https://www.postman.com/meta/messenger-platform-api/documentation/iyp204x/messenger-platform-api?entity=folder-22794852-9fcfb7b7-1d80-422d-b5c8-eb2b1003d4fc&utm_source=chatgpt.com
https://www.postman.com/meta/messenger-platform-api/folder/22794852-255610cd-47f5-4f4d-b3fa-71aec360be9a
https://www.postman.com/meta/messenger-platform-api/folder/22794852-255610cd-47f5-4f4d-b3fa-71aec360be9a?utm_source=chatgpt.com
https://www.postman.com/meta/whatsapp-business-platform/documentation/wlk6lh4/whatsapp-cloud-api?entity=request-13382743-64af82ce-9319-4d7a-a2d6-c8c01040a6de
https://www.postman.com/meta/whatsapp-business-platform/documentation/wlk6lh4/whatsapp-cloud-api?entity=request-13382743-64af82ce-9319-4d7a-a2d6-c8c01040a6de&utm_source=chatgpt.com
https://www.postman.com/meta/whatsapp-business-platform/folder/zuoeksl/registration
https://www.postman.com/meta/whatsapp-business-platform/folder/zuoeksl/registration?utm_source=chatgpt.com
https://www.postman.com/meta/whatsapp-business-platform/overview
https://www.postman.com/meta/whatsapp-business-platform/overview?utm_source=chatgpt.com
https://www.whatsapp.com/legal/messaging-guidelines?lang=pt&trk=public_post_comment-text
https://www.whatsapp.com/legal/messaging-guidelines?lang=pt&trk=public_post_comment-text&utm_source=chatgpt.com
https://www.whatsapp.com/legal/terms-of-service?channel=null
https://www.whatsapp.com/legal/terms-of-service?channel=null&utm_source=chatgpt.com
https://www.ycombinator.com/legal
https://x.com/en/tos
https://x.com/en/tos?utm_source=chatgpt.com