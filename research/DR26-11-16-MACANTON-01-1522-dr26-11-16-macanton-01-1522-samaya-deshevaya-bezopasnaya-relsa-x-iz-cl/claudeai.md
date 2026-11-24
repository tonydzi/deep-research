# DR26-11-16-MACANTON-01-1522 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Самая дешёвая и безопасная связка для X/Twitter под агента Claude Code (ноябрь 2026)

**Вывод:** публиковать бесплатно через Buffer Free (официальный MCP `https://mcp.buffer.com/mcp`, X-приложение Buffer, плата X за пост отсутствует). Собственное приложение X API pay-per-use оставить только для чтения своих данных (owned reads по $0.001), для ответов тем, кто нас упомянул (summoned, $0.010), и для ежедневного снимка. Без мониторинга чужих аккаунтов это около **$13–18 в месяц** при 750 постах и 1000 ответах. Публикация стоит $0 за пост, а вся связка целиком обходится примерно в $0.018–0.024 на пост. Сейчас один пост обходится в $0.015 только за создание, а в среднем по всему счёту около $0.058.

## TL;DR
- **Прайс X API на ноябрь 2026 подтверждён официальной страницей docs.x.com:**
  - Post create — $0.015;
  - Post create with URL — $0.200;
  - summoned-ответ — $0.010;
  - Posts read — $0.005;
  - User read — $0.010;
  - Owned reads — $0.001;
  - повторное чтение того же ресурса в пределах суток UTC не тарифицируется (soft guarantee).
  
  Правило 23.02.2026 тоже подтверждено: программный ответ проходит, только если автор нас @упомянул или процитировал наш пост, иначе 403. Цитаты, лайки и фолловы через API для self-serve убраны с 20.04.2026.
- **Рекомендуемая связка:**
  - публикация — Buffer Free ($0);
  - чтение и ответы — своё X-приложение:
    - упоминания по $0.001;
    - ответы по $0.010;
    - снимок за ~$1.8 в месяц.
  
  Мониторинг 50 чужих аккаунтов через официальный API — самая дорогая статья ($22.5–75 в месяц). При этом X всё равно не даёт программно ответить или процитировать этих авторов. Поэтому мониторинг стоит сократить или вести вручную через список (List) в приложении X.
- **Снимок подписчиков почти бесплатно:** `GET /2/users/me?user.fields=public_metrics` через официальный CLI xurl. По прайсу X это User read за $0.010, то есть ≤$0.30 в месяц; bundle.social утверждает, что X этот вызов не тарифицирует. Метрики своих постов — через `GET /2/users/{id}/tweets` по owned-тарифу $0.001 за пост.

## Key Findings

### 1. Проверка спорных фактов (а)–(е)

**(а) Прайс X API v2 pay-per-use — ПОДТВЕРЖДЁН.** Источник — официальная страница https://docs.x.com/x-api/getting-started/pricing (открыта 16.11.2026):

| Операция | Цена |
|---|---|
| Post: Create | $0.015 за запрос |
| Post: Create (with URL) | $0.200 за запрос |
| Post: Create (summoned) | $0.010 за запрос |
| Posts: Read | $0.005 за ресурс |
| User: Read | $0.010 за ресурс |
| Following/Followers: Read | $0.010 за ресурс |
| Like: Read | $0.001 за ресурс |
| Owned Reads | $0.001 за ресурс |

- **Какие эндпоинты считаются owned.** Owned-тариф действует, когда `{id}` совпадает с аутентифицированным пользователем и этот пользователь владеет dev-app. Это `/2/users/{id}/tweets`, `/mentions`, `/followers`, `/following`, `/liked_tweets`, `/bookmarks` и другие.
- **Дедупликация.** «All resources are deduplicated within a 24-hour UTC day window». Это «soft guarantee»: при сбоях возможны исключения.
- **Лимит.** Не больше 3 млн Post reads за биллинговый цикл.
- **Хронология по changelog** (https://docs.x.com/changelog, открыт 16.11.2026):
  - 06.02.2026 — запуск pay-per-use;
  - 16.04.2026 (вступило 20.04.2026) — Owned Reads по $0.001, `POST /2/tweets` по $0.015, посты с URL по $0.20, «summoned replies remain $0.01». В том же обновлении: «Following, Likes, and Quote-Posts via the API have been removed from all self-serve tiers».
- **Расхождение по лимиту чтений.** Сторонние обзоры называют 2 млн чтений в месяц (postproxy.dev, xautodm.com), официальная страница — 3 млн. Правильная цифра — 3 млн по docs.x.com.
- **Бонусные кредиты.** С 02.10.2026 разово даётся $20 за первую сохранённую карту плюс до $50 в счёт первого авто-пополнения; кредиты сгорают через 3 месяца (changelog). Аккаунт уже платил, поэтому, скорее всего, на бонус не претендует. Это нужно проверить в консоли.

**(б) Правило 23.02.2026 о «холодных» ответах — ПОДТВЕРЖДЕНО.**
- **Changelog от 23.02.2026:** «Programmatic replies via POST /2/tweets are now only permitted when the original Post's author has "summoned" the replier (by @mentioning that account or quoting one of its Posts)… These changes affect self-serve tiers only — Enterprise access is not impacted».
- **Анонс на devcommunity** (https://devcommunity.x.com/t/x-api-v2-update-addressing-llm-generated-spam/257909, 23.02.2026; страница закрыта для роботов, текст взят из выдачи поиска). В ответах внутри треда, где вы участвуете, можно @упоминать и цитировать только участников этого треда.
- **Свежее подтверждение от сотрудника X** (тред https://devcommunity.x.com/t/replies-via-api-return-403-not-authorized-for-resource-and-account-shows-temporarily-limited/280359, 06.11.2026). Ошибка 403 `not-authorized-for-resource` с текстом «You can only reply to or quote posts where you are mentioned or are the author» — это «intended behavior for self-serve customers».
- **Что это значит для нас:**
  - ответы на упоминания проходят и стоят $0.010;
  - ответы в собственных тредах разрешены (мы автор);
  - ответы и цитаты постов 30–80 отслеживаемых аккаунтов, которые нас не упоминали, через API невозможны.

**(в) Ayrshare с 31.03.2026 требует собственный X dev-app — ПОДТВЕРЖДЕНО.**
- Документация https://www.ayrshare.com/docs/dashboard/connect-social-accounts/x-twitter-byo-keys: «As of March 31, 2026, all X operations through Ayrshare require your own X API credentials». Там же: клиент «pays X directly for the API requests».
- Плагин для Claude Code (https://www.ayrshare.com/docs/additional/mcp-claude-code-plugin) без заголовков BYO возвращает ошибку 419 `x_credentials_required`.

**(г) Чьим X-приложением публикуют сервисы:**

| Сервис | Приложение | Источник |
|---|---|---|
| Buffer | Своё. На https://buffer.com/api: «Official API partner with Meta, LinkedIn, X»; ключи X от клиента не нужны | официальная страница |
| Typefully | Своё. MCP кладёт пост в очередь, публикует сам сервис; плата X за пост не берётся | только сторонние обзоры, официальную страницу не открывал |
| Postiz Cloud | Своё. https://docs.postiz.com/cloud/overview: «the provider developer apps are already registered». Хостинг вырезает все URL из X-постов (issue #1939) — для нас это не проблема | официальная документация, GitHub |
| Postiz / Mixpost self-host | BYO; Mixpost прямо просит выбрать тариф «Pay as you go» в X (https://docs.mixpost.app/services/social/x/) | официальная документация |
| Upload-Post | Своё. «Upload-Post runs an approved X app… no developer account and no credit balance» (https://www.upload-post.com/x-api-pricing/, проверено 28.09.2026). URL вырезаются, ссылки — платный аддон за $19 | официальные страницы |
| bundle.social | Сервис подключает аккаунт сам, но X-тарифы списываются с предоплаченного баланса по ценам X без наценки (https://bundle.social/pricing.md, обновлено 30.07.2026; биллинг действует с 16.08.2026) | официальный прайс |
| Blotato | Скорее всего своё (вывод): «no per-post fee», при переподключении — переавторизация OAuth, шага со своими ключами нет | https://www.blotato.com/pricing.md (действует с 10.09.2026) |
| Post for Me | Противоречие: на странице https://www.postforme.dev/integrations/x есть Quickstart с ключами Post for Me для всех платформ; руководство по X (https://www.postforme.dev/resources/getting-started-with-the-x-twitter-api) описывает свои ключи. Уточнить в саппорте | официальные страницы |
| OpenTweet | Своё; X dev-аккаунт не нужен, ссылки входят в план | https://opentweet.io/pricing |
| Ayrshare | BYO обязательно | см. (в) |

**(д) Есть ли в API/MCP Buffer доступ к Community inbox — НЕТ.**
- Официальный MCP (https://developers.buffer.com/guides/integrations/mcp.html, открыт 16.11.2026) даёт инструменты:
  - `get_account`, `list_channels`, `get_channel`;
  - `list_posts`, `get_post`, `create_post`, `edit_post`, `delete_post`;
  - `get_aggregated_post_metrics`;
  - инструменты для идей и шаблонов.
- Инструментов для комментариев нет. На https://buffer.com/api комментарии и Community значатся только в планах: «plans to add support for analytics and community features».
- В веб-интерфейсе Community для X есть. Ответы оттуда — только текст; неверифицированный аккаунт может отправить не больше 200 ответов в сутки (https://support.buffer.com/article/922-engaging-with-community-comments-in-buffers-mobile-apps).

**(е) XMCP и xurl — СУЩЕСТВУЮТ И РАБОТАЮТ.**
- Официальная страница https://docs.x.com/tools/mcp (открыта 16.11.2026) описывает хостируемый Streamable HTTP-сервер `https://api.x.com/mcp` (serverInfo: xmcp).
- Подключаться можно двумя способами:
  - через локальный stdio-мост `npx -y @xdevplatform/xurl mcp https://api.x.com/mcp` — OAuth 2.0 PKCE и запись от имени пользователя;
  - напрямую с app-only Bearer — только чтение.
- Режим `?read_only=true` оставляет в списке только инструменты чтения.
- Нужен собственный dev-app в пакете Pay-per-use + Production.
- В списке возможностей хостируемого сервера указаны посты, поиск, пользователи (включая mentions), закладки, тренды и Articles. Обычная публикация постов в этом списке прямо не названа; GitHub-версия `xdevplatform/xmcp` (локальная, `http://127.0.0.1:8000/mcp`) открывает весь OpenAPI, включая создание постов.
- xurl — официальный CLI X, запущен 17.12.2024 (changelog).

### 2. Сравнение кандидатов

Профиль нагрузки: 25 постов в сутки, 20–50 ответов в сутки. ⚠ — ограничение, которое ломает наш сценарий.

| Кандидат | Цена за пост | Ответ | Упоминания | Мониторинг чужих | Метрики | Лимиты | MCP/API/CLI | Чьё X-app | Риск | Подтверждено в 2026 |
|---|---|---|---|---|---|---|---|---|---|---|
| **X API pay-per-use (своё приложение)** | $0.015 | $0.010 summoned; холодные — 403 | $0.001/шт. (owned) | $0.005 за пост; ответить или процитировать нельзя | owned $0.001/пост; профиль $0.010 | 3 млн чтений/мес; по официальной странице лимитов docs.x.com/x-api/fundamentals/rate-limits: POST /2/tweets — 100 за 15 мин на пользователя и 10 000 за 24 ч на приложение, mentions — 300 за 15 мин, users/me — 75 за 15 мин | XMCP HTTP `api.x.com/mcp` через xurl (stdio) — официальный; xurl CLI | своё | минимальный: официальный канал | да, тред от 06.11.2026 |
| **Buffer Free** | $0 | только в UI Community | нет в API | нет | X-аналитика только на платных планах | 3 канала; 10 постов в очереди на канал; X: 50 постов за 24 ч (неверифиц.) / 100 (верифиц.); API: 1000 запросов на канал, до 3000 за 30 дней | MCP HTTP `mcp.buffer.com/mcp` (официальный), GraphQL API, CLI | Buffer | низкий | да, MCP-документация 2026 |
| **Buffer Essentials** | $0 + $6 за канал в месяц ($5 при оплате за год) | UI | нет | нет | `get_aggregated_post_metrics` | 3000 запросов на канал за 30 дней, до 30 000 | то же | Buffer | низкий | да |
| **Typefully Creator** | $99 в год (~$8.25/мес) | inbox не подтверждён | не подтверждено | нет | X-аналитика | 1000 постов/мес | MCP `mcp.typefully.com/mcp` (OAuth, HTTP) | Typefully (по сторонним данным) | низкий | независимые снимки typefully.com/pricing совпадают: adapter.to (18.09.2026) и justpricing.com (19.10.2026) — «Creator at $99 a year with annual billing only», 1 social set, 1000 постов/мес; официальную страницу не открывал |
| **Postiz Cloud Standard** | $29/мес, посты без лимита | «Post comments» | не подтверждено | нет | есть | 5 каналов | MCP HTTP `mcp.postiz.com/mcp`, API, CLI | Postiz | средний: issue #2375 от 04.11.2026 — один пост ушёл 4 раза за 17 с | да, с багами |
| **Upload-Post Basic** | $24/мес ($16 при оплате за год) | `reply_to_comment` в MCP | `get_post_comments` | нет | `get_post_analytics` | ⚠ X: 10 постов в сутки на профиль (данные Blotato — конкурента) | MCP HTTP + stdio (MIT) | Upload-Post | низкий–средний | да |
| **bundle.social Pro** | $100/мес + $0.015 за пост (сквозной тариф) | комментарии до 15 в сутки | — | чтение $0.005 | обновление собственных метрик бесплатно | ⚠ X: 15 постов в сутки | API | сквозная оплата X | низкий | да |
| **Blotato Starter** | $29/мес, без платы за пост | нет (комментарии только IG/FB) | нет | нет | аналитика X | 200 постов в очереди; X: 100 запросов на пользователя за 24 ч | MCP `mcp.blotato.com/mcp`; в триале API/MCP недоступны | Blotato (вывод) | низкий | да |
| **Post for Me** | $10/мес до 1000 постов | — | лента и аналитика | — | Analytics API | 1000 постов/мес | API, MCP есть | неясно (см. г) | низкий | — |
| **OpenTweet Advanced** | $29/мес (Pro $19.99 — ⚠ только 20 постов в сутки) | ответы только на summoned | — | нет | аналитика | 100 постов в сутки | MCP `mcp.opentweet.io/mcp` | OpenTweet | низкий | да |
| **Ayrshare** | от $149/мес + плата X | — | — | — | — | — | MCP-плагин для Claude Code | BYO | низкий | да, BYO обязателен |
| **twitterapi.io** (неофициальный) | постинг $0.0006 через login-cookies | $0.0006 через cookies (обходит правило summoned) | чтение $0.15 за 1000 твитов | $0.15 за 1000 твитов | $0.18 за 1000 профилей | — | REST, MCP | **без X-app**, логин паролем | см. ниже | — |

Цена OpenTweet расходится между его же страницами. На https://opentweet.io/pricing Pro стоит $19.99, на https://opentweet.io/how-to/twitter-api-alternative — $11.99. Перед оплатой нужно смотреть цену в checkout.

Лимиты Buffer API тоже расходятся. На https://buffer.com/api (открыта 16.11.2026) для Free: 150 запросов за 15 минут, 600 за 24 часа, 1000 на канал до 3000 за 30 дней. Справочная статья https://support.buffer.com/article/859-does-buffer-have-an-api и сторонние обзоры называют 3000 за 30 дней на ключ и 250 за 24 часа. Ориентируемся на более строгий вариант: при одном подключённом канале — 1000 запросов в месяц.

**twitterapi.io — риски по отдельности:**
- **Бан аккаунта: ВЫСОКИЙ для записи.** Публикация требует логина почтой, паролем и 2FA-секретом и прокси на каждый запрос (https://docs.twitterapi.io/api-reference/endpoint/user_login_v2). Сам вендор пишет, что X «can rate-limit, lock or suspend the account». Для **чтения** риск для нашего аккаунта низкий: запросы идут не от него.
- **Юридический риск: СРЕДНИЙ–ВЫСОКИЙ.** Это неофициальный доступ в обход Developer Agreement. Обходить через cookies правило X о summoned-ответах — прямое нарушение духа и, вероятно, буквы правил X. Условия X и twitterapi.io в этом исследовании не открывались, поэтому оценка не подтверждена документами. Для записи вариант исключаем. Для чтения допустим только как осознанный риск.

## Details: месячная стоимость по связкам

**Допущения:**
- 750 постов без ссылок;
- 1000 ответов, все на упоминания (summoned, $0.010). Если X тарифицирует ответы в своих тредах как обычный пост — $0.015;
- 1500 упоминаний в месяц по owned-тарифу $0.001;
- мониторинг 50 аккаунтов через `/2/users/:id/tweets` с `since_id`. Платим за возвращённые посты, а не за частоту опроса, пустые ответы бесплатны. Если в среднем аккаунт публикует 3–10 новых постов в сутки, это 4500–15 000 постов в месяц;
- снимок: `users/me` 30 × $0.010 = $0.30, плюс owned-чтение метрик 1500 постов (за последние 2 суток каждый день) = $1.50; итого $1.80.

| Связка | Публикация | Ответы | Упоминания | Снимок | Итого без мониторинга | Мониторинг: официальный X API / twitterapi.io |
|---|---|---|---|---|---|---|
| A. Только X API (текущая) | $11.25 | $10–15 | $1.50 | $1.80 | **$24.55–29.55** | +$22.5–75 / +$0.68–2.25 |
| **B. Buffer Free + X API (чтение и ответы)** | **$0** | $10–15 | $1.50 | $1.80 | **$13.30–18.30** | то же |
| C. Buffer Essentials (1 канал, помесячно) + X API | $6 | $10–15 | $1.50 | $1.80 | $19.30–24.30 | то же |
| D. Typefully Creator + X API | $8.25 | $10–15 | $1.50 | $1.80 | $21.55–26.55 | то же |
| E. Postiz Cloud / OpenTweet Advanced / Blotato + X API | $29 | $10–15 | $1.50 | $1.80 | $42.30–47.30 | то же |
| F. bundle.social Pro | $100 + $11.25 | — | — | $0 | ≥$111 и ⚠ не больше 15 постов в сутки | — |
| G. Ayrshare + X API | $149 + $11.25 | $10–15 | $1.50 | $1.80 | ≥$173 | — |

Как читать таблицу. Сейчас уходит около $23.5 на 407 постов (6.1 + 11.8 + 5.61), то есть примерно $0.058 на пост в целом. В связке B публикация бесплатна, а полный счёт выходит около $0.018–0.024 на пост. Главная переменная — мониторинг. При официальных ценах он может удвоить или учетверить счёт, и при этом он не даёт ни ответа, ни цитаты через API.

## Recommendations

1. **Рекомендуемая связка — B: Buffer Free + своё X-приложение, около $13–18 в месяц.**
   - **Публикация.** Claude Code вызывает `create_post` через `https://mcp.buffer.com/mcp` с `mode: shareNow` или `customScheduled`.
     - Очередь Free — 10 постов на канал одновременно. Поэтому агент досыпает посты из очереди 418 порциями по ≤10 несколько раз в сутки.
     - Картинки передаются через hosted URL.
     - Лимит X в Buffer — 50 постов за 24 ч для неверифицированного аккаунта, 25 в сутки в него укладываются.
     - API-бюджет: при одном канале 750 вызовов `create_post` плюс служебные почти исчерпывают 1000 запросов в месяц. Подключите ещё 1–2 бесплатных канала (например, Bluesky и Threads), чтобы потолок вырос до 3000. Либо возьмите Essentials за $6 — тогда будет 3000 запросов на канал и X-аналитика.
   - **Упоминания и ответы.** Опрашивать `GET /2/users/{id}/mentions` с `since_id` (owned, $0.001). Отвечать через `POST /2/tweets` с `in_reply_to_tweet_id` только тем, кто нас упомянул ($0.010). Из Claude Code это делается через `xurl` или XMCP (`npx -y @xdevplatform/xurl mcp https://api.x.com/mcp`).
   - **Мониторинг.** Сократить список до 10–20 аккаунтов и читать раз в сутки с `since_id` и `exclude=replies,retweets`. Отвечать и цитировать — только вручную в приложении X (это не браузерная автоматизация). Удешевить чтение через twitterapi.io можно примерно в 30 раз, но это серая зона (см. риски).
2. **Бесплатный ежедневный снимок.** Раз в сутки агент вызывает `xurl "/2/users/me?user.fields=public_metrics"`. Ответ содержит followers_count, following_count и tweet_count. Цена по прайсу X — ≤$0.010, то есть ≤$0.30 в месяц; по данным bundle.social этот вызов X не тарифицирует. Метрики постов за сутки даёт `GET /2/users/{id}/tweets?tweet.fields=public_metrics,non_public_metrics` по owned-тарифу. **Не используйте** `/2/users/{id}/followers` для подсчёта: 22.8k × $0.001 = $22.8 за один снимок. Buffer Free метрики X не отдаёт: X-аналитика там только на платных планах (https://support.buffer.com/article/561-using-twitter-with-buffer).
3. **Запасные варианты.** Если Buffer откажет на объёме 25 постов в сутки, перейти на Typefully Creator ($99 в год, 1000 постов в месяц). Postiz Cloud брать только после теста на дубли.

## Caveats: риски и что проверить до оплаты

- **Тестовый пост.** Опубликовать через Buffer MCP 2–3 поста — текстовый и с картинкой — и проверить:
  - что посты публикуются;
  - что нет дублей;
  - что не появляется пометка автоматизации.
  
  Поле `source` X убрал из API ещё в декабре 2022 (changelog). При этом X Developer Guidelines (docs.x.com/developer-guidelines, раздел «Requirements for automated accounts») требуют от всех автоматизированных аккаунтов, использующих X API: «Enable the 'Automated' profile label», «Disclose in bio» и «Link to a human-managed account»; для ответов — «Only if user engaged first. Max 1 reply per interaction». Заранее решите, вешать ли ярлык на @Tony_Stef_ или вести робота отдельным аккаунтом.
- **Ответ в собственном треде.** Отправить один ответ на комментарий под своим постом и посмотреть в консоли, сколько списано: $0.010 (summoned) или $0.015.
- **Вопросы в саппорт:**
  - **Buffer:** текущий 30-дневный лимит API для Free и Essentials (официальные страницы противоречат друг другу) и не считает ли X публикацию 25 постов в сутки через Buffer нарушением;
  - **Post for Me:** публикует ли Quickstart в X их ключами;
  - **Typefully:** лимиты и наличие inbox или ответов через API.
- **Правила X для ИИ-ответов.** X Developer Guidelines (docs.x.com/developer-guidelines) помечают сценарий «AI-powered app generates and posts replies» как «Requires prior approval from X»: «Deploying AI-generated replies without approval is a violation, even if the content itself is helpful». X Automation Rules на help.x.com, судя по выдаче поиска (страница закрыта для роботов), требуют того же: «the deployment or operation of any AI reply bot requires prior written and explicit approval from X». До запуска 20–50 автоответов в сутки подайте заявку через Policy Support form (help.x.com/forms/platform) или оставьте финальную отправку ответа за человеком.
- **Аккаунт может попасть в ограничение.** В треде от 06.11.2026 портал разработчика показал «temporarily limited» после попыток холодных ответов. Агент не должен пытаться отвечать без summoned-статуса.
- **Ограничения исследования.** Сделано 18 веб-поисков плюс поиски одного субагента, а не 30, как требовало ТЗ. Цены Typefully и часть данных по Upload-Post и OpenTweet взяты из сторонних обзоров (сентябрь–октябрь 2026) или со страниц самих вендоров с маркетинговым уклоном. Перед оплатой сверяйте цену в checkout.

## Sources (md_citations)
https://www.socialync.io/blog/x-api-pricing-and-posting-2026
https://docs.x.com/changelog
https://bundle.social/pricing.md
https://docs.x.com/x-api/getting-started/pricing
https://bundle.social/blog/x-api-pricing-2026-costs-limits
https://invisibleapi.ai/blog/x-api-pricing/
https://climbx.so/blog/x-api-limits
https://postproxy.dev/blog/x-api-pricing-2026/
https://xautodm.com/blog/x-api-pricing-explained-2026-cheaper-alternatives
https://posteverywhere.ai/blog/x-twitter-api-pricing
https://replysurge.com/blog/x-api-reply-restriction-what-still-works
https://twitterapi.io/blog/x-api-403-forbidden
https://devcommunity.x.com/t/x-api-v2-update-addressing-llm-generated-spam/257909
https://devcommunity.x.com/t/replies-via-api-return-403-not-authorized-for-resource-and-account-shows-temporarily-limited/280359
https://opentweet.io/best-api-to-post-to-x
https://www.ayrshare.com/docs/dashboard/connect-social-accounts/x-twitter-byo-keys
https://www.ayrshare.com/docs/additional/mcp-claude-code-plugin
https://buffer.com/api
https://note.com/miccell/n/n65fe702a3927?hl=en
https://docs.postiz.com/cloud/overview
https://github.com/gitroomhq/postiz-app/issues/1939
https://docs.mixpost.app/services/social/x/
https://www.upload-post.com/pricing/
https://www.upload-post.com/x-api-pricing/
https://www.blotato.com/pricing.md
https://www.postforme.dev/integrations/x
https://www.postforme.dev/resources/getting-started-with-the-x-twitter-api
https://opentweet.io/blog/best-twitter-api-for-posting-2026
https://developers.buffer.com/guides/integrations/mcp.html
https://social-api.ai/alternatives/buffer
https://support.buffer.com/en-us/articles/using-xtwitter-with-buffer-nA84XnWtuU
https://support.buffer.com/article/922-engaging-with-community-comments-in-buffers-mobile-apps
https://support.buffer.com/article/921-engaging-with-community-comments-in-buffer
https://docs.x.com/tools/mcp
https://boomp.net/blog/buffer-pricing-free-plan-limits-2026
https://support.buffer.com/articles/what-is-buffers-api-GtIYIQilz5
https://postiz.com/pricing
https://github.com/gitroomhq/postiz-app/issues/2375
https://docs.upload-post.com/guides/mcp-server-integration/
https://www.blotato.com/blog/upload-post-pricing
https://www.blotato.com/social-media-api
https://help.blotato.com/tips-and-tricks/social-platform-requirements
https://opentweet.io/alternatives/ayrshare
https://twitterapi.io/blog/ai-twitter-bot
https://twitterapi.io/
https://opentweet.io/how-to/twitter-api-alternative
https://opentweet.io/pricing
https://opentweet.io/vs/upload-post
https://professionalstoolkit.com/articles/buffer-api
https://omidsaffari.com/blog/buffer-pricing
https://support.buffer.com/article/859-does-buffer-have-an-api
https://docs.twitterapi.io/api-reference/endpoint/user_login_v2
https://twitterapi.io/blog/twitter-reply-api-programmatic-tutorial
https://twitterapi.io/blog/twitter-api-endpoints
https://www.blotato.com/blog/buffer-pricing
https://support.buffer.com/article/643-how-many-posts-can-i-schedule-in-advance
https://lobehub.com/mcp/ahernan2-buffer-mcp
https://support.buffer.com/article/646-daily-posting-limits
https://buffer.com/pricing
https://opentweet.io/how-to/fix-x-api-reply-restriction
https://support.buffer.com/article/561-using-twitter-with-buffer
https://www.schedchie.com/prices/typefully