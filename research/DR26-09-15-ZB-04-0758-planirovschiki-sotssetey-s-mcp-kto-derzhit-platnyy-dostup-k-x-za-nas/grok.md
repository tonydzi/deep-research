# DR26-09-15-ZB-04-0758 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала подниму живые источники по MCP-планировщикам, X API и тарифам — отчёт пойдёт сразу по пунктам.Дальше читаю официальные страницы вендоров: MCP-эндпоинты, кто платит за X, и что именно умеет публиковать.Дальше читаю официальные страницы вендоров: MCP-эндпоинты, кто платит за X, и что именно умеет публиковать.Дальше сверю тарифы, «кто платит за X» и кто умеет публиковать сразу, а не только класть черновик.A. Сравнительная таблица (снято 15 сентября 2026)

Рынок MCP-планировщиков за 2026 год раздвоился: (1) классические планировщики, которые открыли MCP поверх своей очереди; (2) agent-first API, у которых MCP — сам продукт. Официального MCP у самих площадок (кроме XMCP у X) нет. Ниже — только first-party серверы, которые я открыл или сверил по живым страницам вендора в этом прогоне. Сообщественные обёртки (Publer community MCP, старый `pepuscz/typefully-mcp-server`) в таблицу не входят.

| Название | MCP-эндпоинт | Авторизация | Площадки | Свой X-ключ? | Бесплатный тариф | Платный тариф | Публикует или черновик | Главное ограничение |
|---|---|---|---|---|---|---|---|---|
| Buffer | `https://mcp.buffer.com/mcp` | OAuth (Claude Connectors) или Bearer API-ключ | X, LinkedIn, Threads, Instagram, Facebook, TikTok, YouTube, Pinterest, Bluesky, Mastodon, Google Business | Нет, OAuth в приложение Buffer | 3 канала, 10 scheduled-постов на канал (можно refill), 1 тред, MCP включён, first comment нет | Essentials $5/канал/мес (год), Team $10/канал; first comment с Essentials | Агент кладёт draft/scheduled в очередь Buffer. Сам Buffer потом публикует. Мгновенный «publish now без человека» вендор прямо отрицает | Очередь, не автопилот. Free не тянет 1–3 поста/день. Нет Reddit/Dev.to/Telegram/Discord |
| Typefully | `https://mcp.typefully.com/mcp` | OAuth (рекомендуют) или Bearer API-ключ. Старый URL с ключом в query ещё жив | X, LinkedIn, Threads, Bluesky, Mastodon, Substack Notes | Нет, OAuth в Typefully | Есть Free. Живая страница цен — JS, цифры расходятся: снимки Aug 2026 дают ~1 social set и 10–15 постов/мес; Eden 19.08.2026 пишет «MCP на каждом плане, включая free» | Агрегаторы: Creator ~$12.50/мес (год). Официальный виджет в этом прогоне цифр не отдал | MCP официально: create/edit/schedule drafts, очередь, медиа. REST API v2 умеет `publish_at="now"`. Eden классифицирует MCP как drafts-and-queue | Нет IG/TikTok/Reddit/Dev.to. Free слишком узкий для ежедневки. Немедленный publish — через API, не как главная MCP-ручка |
| Postiz Cloud | `https://mcp.postiz.com/mcp` + Bearer; или `https://api.postiz.com/mcp/<API_KEY>` | API-ключ Postiz (OAuth-токен `pos_` тоже) | 30+: X, LinkedIn, IG, FB, TikTok, YT, Threads, Reddit, Telegram, Discord, Slack, Bluesky, Mastodon, Medium, Hashnode, Dev.to, WordPress и др. | На Cloud — нет (OAuth через приложение Postiz). На self-host — да, `X_API_KEY`/`X_API_SECRET` | Self-host бесплатно (AGPL). Cloud: 7 дней trial | Cloud Standard $29/мес / 5 каналов; Team $39/10; Pro $49/30; Ultimate $99/100. Посты безлимит. MCP входит | `schedulePostTool`: schedule, draft **или сразу publish**. Треды через массив `postsAndComments`. First comment = Post comments / Internal Plugs | Self-host упирается в ваш 403. Cloud $29 и 5 каналов на старте. Продукт моложе Buffer |
| Mixpost | `https://<host>/<MIXPOST_CORE_PATH>/mcp` (дефолт `/mixpost/mcp`) | Bearer access token | Pro: ~11–12 сетей. Lite: только Facebook Pages, X, Mastodon | Да, своё X-приложение + Pay-as-you-go | Lite MIT, 3 площадки | Pro $299 once. MCP за Pro | create/schedule/publish, но approval-workspace может остановить «сразу в эфир» | Self-host + BYO ключи каждой сети. Для вашей задачи X — тупик |
| Ayrshare | `https://api.ayrshare.com/mcp` | Только `Authorization: Bearer`. **Нет OAuth-метаданных** — личный Claude connector не авторизуется | Bluesky, FB Pages, GBP, IG, LinkedIn, Pinterest, Reddit, Snapchat, Telegram, Threads, X, TikTok, YouTube | **Да, с 31.03.2026.** Без заголовков `X-Twitter-OAuth1-Api-Key/Secret` X-вызов = error 419. Раньше X платил Ayrshare | 20 realtime-постов/мес на Basic (брендинг Ayrshare). Eden: у новых аккаунтов нет постоянного free, 28-дневный trial | Premium $149/мес, 1 профиль, 1000 постов. Launch $299. Business от ~$499–599 | 27 tools, публикация через тот же API chain | Именно ваш 403. Claude Desktop/claude.ai personal connector не работает. RSS→X умер |
| Metricool | `https://ai.metricool.com/mcp` (встречается и `mcp.metricool.ai/mcp`) | OAuth 2.0 / PKCE. n8n — заголовок `X-Mc-Auth` | IG, FB, X, LinkedIn, TikTok, YT, Pinterest, Twitch, Bluesky, Threads, GBP + Ads | Нет, OAuth в Metricool | MCP на любом плане, включая Free. Free: не старше 3 мес данных, **не больше 20 scheduled-постов** | Планы Metricool (MCP не тарифицируется отдельно) | Help: «see, create, and publish posts». Права = полный доступ к аккаунту | Free: 20 постов. Нет Dev.to/Hashnode/Telegram как у Postiz. Маркетолог-стек |
| Outstand | `https://mcp.outstand.so/mcp` (live с марта 2026) | OAuth connector или Bearer `ost_` | 12: X, LinkedIn, IG, FB, Threads, TikTok, YT, Bluesky, Pinterest, GBP, Vimeo, Reddit | Нет (OAuth в Outstand) | Free tier, без карты | $19/мес: 3000 постов, далее $0.007 затем $0.005. MCP бесплатно | 28 tools: `create_post` публикует, есть schedule, replies, media | Нет Mastodon/Dev.to/Hashnode/Telegram/Discord. Молодой вендор |
| OpenTweet | `https://mcp.opentweet.io/mcp` или `npx -y @opentweet/mcp-server` | Bearer `ot_` | X (+ зеркало в Bluesky в docs) | Нет. «No Twitter dev account needed» — они держат X-коннект | 7 дней trial без карты | Pro $11.99/мес ($9.99 год). Advanced $29, Agency $49 | 43 tools: draft, **publish now**, schedule, треды до 25, batch 50 | Почти только X. Не закрывает дев-площадки |
| SocialClaw | `https://getsocialclaw.com/mcp` | Bearer `sc_live_…`; аккаунты OAuth в дашборде | 11: X, LinkedIn, IG, FB Pages, TikTok, Discord, Telegram, YT, Reddit, WordPress, Pinterest | Нет | Free tier | От $15/мес flat | 17 tools: validate → `publish_draft` → inspect delivery. Официальные API, не браузер | Нет Bluesky/Threads/Mastodon/Dev.to. Self-host нет. Статья вендора |
| bundle.social | `npx -y bundlesocial-mcp` (stdio) | `BUNDLESOCIAL_API_KEY` | 15 сетей, OAuth в дашборде | Нет | 14-дневный trial | Flat, без per-account (цифру живой страницы цен в этом прогоне не открывал) | `create_post` + `schedule_post` | stdio, не remote URL. Dev.to/Hashnode не заявлены |
| Eden | `https://mcp.eden.so/mcp` | OAuth | 8: X, LinkedIn, Threads, IG, TikTok, FB Pages, YT Shorts, Substack | Нет | Free $0 без карты | От $29/мес | Draft + schedule + publish, плюс research-корпус | Нет Reddit/Dev.to/Mastodon/Telegram. Это research+publish, не дешёвый пост-рельс |
| Publora | `https://mcp.publora.com` | Bearer `sk_…` | X, LinkedIn, IG, Threads, TikTok, YT, FB, Bluesky, Mastodon, Telegram | Не требуют свой X-ключ в README | Не проверял live free | Сайт: «from $5.40/mo» | `create_post` = schedule; в примерах есть «Published to Twitter…» | Маленький вендор. 18 tools, analytics сильно LinkedIn-центричны |
| Schedulala | `https://schedulala.com/api/mcp` | OAuth | 12: X, IG, TikTok, LinkedIn, YT, FB, Threads, Bluesky, Pinterest, Mastodon, Telegram, GBP | Нет (OAuth) | Не проверял live | Не проверял live | Schedule + publish. **First comment на 8 площадках одним полем API** | Цифры тарифа в этом прогоне не открывал. Нет Dev.to/Hashnode/Reddit/Discord |
| Hootsuite Perch | заявлено `hootsuite.com/integrations/mcp` (Perch = publishing; Nest = inbox; Lumen = listening) | OAuth | Классический Hootsuite-набор | Нет, партнёр X | Нет free. Trial 10–20 постов/день | Standard ~$99/user/мес за 10 аккаунтов | Publishing MCP есть (инвентарь Outstand 30.08.2026; Eden 19.08.2026) | Enterprise-цена. Собственную MCP-документацию Hootsuite в этом прогоне не открывал |
| XMCP (X, не планировщик) | `https://api.x.com/mcp` с 30.06.2026 | OAuth 2.0 + ваш X developer app (`xurl mcp` для user-context writes) | Только X | **Да, и биллинг PPU на вас** | Нет | $0.015/пост, **$0.20 если в посте ссылка** (OpenTweet 09.09.2026). Schedule-эндпоинта на self-serve нет | Publish now. Schedule нет | Это ровно ваш 403 и штраф за ссылки |

Не путать с официальными: Zapier/Composio MCP (агрегаторы), Publer community GitHub, Sprout Social MCP (слушание/аналитика, не постинг — инвентарь Outstand 30.08.2026).

B. Три именованные рекомендации

1. «Дешевле всего, чтобы X жил без вашего PPU» — Buffer Essentials на один канал X ($5/мес, год) + MCP.

Зачем: Buffer держит X-партнёрство, вы только логинитесь OAuth. MCP бесплатен даже на Free, но Free даёт 10 scheduled-постов на канал — при 1–3 постах/день это 30–90 постов/мес, Free не хватает. Essentials даёт безлимит очереди, треды, first comment. Риск бана как у классического планировщика через официальный API — низкий относительно браузерной автоматизации.

Минус под вашу задачу: Buffer сам пишет, что ИИ не публикует без вас — посты падают как draft/scheduled, «you are always the last step». Для автономии агент должен сразу ставить **scheduled**, не draft. Мгновенный publish-now Buffer не обещает. Дев-площадок нет.

Альтернатива ещё дешевле на чистый X: OpenTweet Pro $11.99/мес — единственный, кто честно пишет «нет X developer account» + publish now + schedule. Дороже Buffer, но ближе к «агент сам жмёт Send».

2. «Шире всего по площадкам, где живут инженеры» — Postiz Cloud $29/мес (5 каналов) + MCP.

Зачем: это единственный first-party MCP в выборке, у которого в одном списке X **и** Mastodon, Bluesky, Reddit, Dev.to, Hashnode, Medium, Telegram, Discord. `schedulePostTool` умеет немедленную публикацию, треды, scheduled comments. На Cloud ключи платформ держит Postiz, не вы — self-host как раз требует `X_API_KEY` и ваш 403.

Минус: $29, 5 каналов на Standard (X + 4 дев-сети влезают). Продукт моложе Buffer/Typefully. Вендор просит human-in-the-loop, но технически publish now есть.

Если 5 каналов мало — Ultimate $99/100 каналов избыточен; разумный апгрейд Team $39/10.

3. «Надёжнее всего на X + не тащить дешёвый OSS на бой» — Typefully (OAuth MCP) как рельса X, и отдельный второй MCP на дев-площадки.

Зачем: Typefully — X-native (треды, AutoPlug = ссылка/добор после поста, AutoRT, natural posting ±4 мин). MCP официальный, OAuth, без своего X-приложения. REST умеет `publish_at="now"`. Они прямо предупреждают про X automation rules — это зрелый тон, не «постим как бот». Buffer рядом по надёжности бренда, но Typefully лучше как писательская машина для @Tony_Stef_.

Стек без конфликта: Typefully MCP на X + Postiz MCP на Dev.to/Hashnode/Reddit/Telegram. Два OAuth в два разных приложения X разрешает. Дубль поста будет только если агент отправит один текст в оба. Не делайте Ayrshare и self-host Postiz/Mixpost на X — это ваш текущий 403.

Не брать: Ayrshare (с 31.03.2026 X = BYO + кредиты X, ссылочные посты дорогие); XMCP; self-host Postiz/Mixpost для X.

C. Пункт 1 — полный список официальных MCP (сентябрь 2026)

Сверил по живым страницам вендоров и двум обзорам августа 2026 (Eden 19.08, Outstand inventory 30.08). Категория «официальный» = first-party сервер вендора, не GitHub-обёртка.

Подтверждены открытием страницы в этом прогоне: Buffer, Typefully, Postiz, Mixpost, Ayrshare, Metricool, Outstand, OpenTweet, SocialClaw, bundle.social, Eden, Publora, Schedulala.

Подтверждены вторичными обзорами Aug 2026, собственную MCP-документацию не открывал: Hootsuite (три сервера Perch/Nest/Lumen), Taplio (только LinkedIn). Upload-Post заявлен в MCP Registry как `com.upload-post/mcp` (13+ сетей вкл. Reddit/Discord/Telegram/Bluesky) — карточку каталога видел, сайт вендора целиком не разбирал.

Нет first-party publishing MCP (на дату обзоров): Later, Sendible, SocialBee, Loomly, CoSchedule. Sprout Social MCP — данные, не постинг. Publer API есть на Business, официального MCP нет (community-репо).

Методы авторизации сгруппированы так:

- OAuth remote URL, удобно Claude Connectors: Buffer, Typefully, Metricool, Outstand, Eden, Schedulala.
- Только Bearer header (Claude Desktop personal connector часто ломается): Ayrshare, Postiz, OpenTweet, SocialClaw, Publora, Mixpost.
- stdio npx: bundle.social, опционально OpenTweet.

D. Пункт 2 — кто платит за X

Это главная развилка 2026 года. X закрыл новые Basic/Pro, self-serve = Pay-Per-Use. Незачисленное приложение даёт ровно ваш `403 client-not-enrolled`. Треды X Developers с этим текстом идут с февраля по сентябрь 2026.

Класс A — пользователь только OAuth, биллинг X на стороне сервиса (то, что вам нужно):

- Buffer, Typefully, Hootsuite, Metricool: классические партнёры, аккаунт подключается как канал.
- Postiz **Cloud** (не self-host): OAuth в дашборде Postiz.
- OpenTweet: прямо «handles the X connection», «no developer account».
- Outstand, SocialClaw, bundle.social, Eden, Publora, Schedulala: OAuth в их дашборде, своего X app не просят.

Класс B — BYO, вы платите X:

- Ayrshare с **31 марта 2026**: «Previously, Ayrshare covered the cost of X's API usage. Under X's current model, each Ayrshare customer maintains their own X Developer account and pays X directly». Без ключей — error 419. Нужны кредиты в console.x.com, иначе 402 CreditsDepleted. RSS→X больше не работает.
- Postiz **self-host**: `X_API_KEY` + `X_API_SECRET`, Native App, OAuth 1.0a.
- Mixpost: то же, плюс явно «Pay as you go» в форме.
- XMCP: биллинг вашего X-плана, $0.20 за пост со ссылкой.

Self-hosted Postiz/Mixpost требуют ключи **каждой** платной сети с developer-app (X, Meta, LinkedIn, TikTok, Google). Бесплатные/дешёвые ключи обычно:

- Mastodon — своё приложение на инстансе, бесплатно.
- Bluesky — app password, бесплатно.
- Telegram — Bot API token через BotFather, бесплатно (канал — бот-админ).
- Discord — bot token + webhook, бесплатно.
- Reddit — бесплатное script/web app на reddit.com/prefs/apps, но OAuth и лимиты свои.
- Dev.to / Hashnode / Medium — персональный API key в настройках, бесплатно.

X, Meta, LinkedIn, TikTok, YouTube — не бесплатны в смысле «просто ключ»: ревью приложения и/или платный доступ. Для X в 2026 это ещё и PPU.

Цена X PPU, если пойдёте своим приложением: OpenTweet 09.09.2026 — $0.015 за пост, **$0.20 если есть ссылка**. У вас ссылки в постах. 1–3 ссылочных поста/день ≈ $6–18/мес только X, без schedule. Ayrshare в своих доках даёт более мягкие ~$0.01 text / ~$0.02 media и говорит «пилот, subject to change» — расхождение с $0.20 материально, закладывайте худшую цифру.

E. Пункт 3 — черновик vs публикация. Отдельно Typefully

| Сервис | Немедленный publish из MCP | Schedule, который сам выйдет | Только черновик |
|---|---|---|---|
| Postiz | Да, `schedulePostTool` type immediate | Да | Да |
| OpenTweet | Да | Да | Да |
| Outstand | Да, `create_post` | Да | — |
| SocialClaw | Да, `publish_draft` | Да | Да |
| bundle.social | Да, `create_post` | Да, `schedule_post` | — |
| Ayrshare | Да (через API chain) | Да | — |
| Metricool | Help: create **and publish** | Да | — |
| Mixpost | Да, если нет approval-гейта | Да | Да |
| Typefully MCP | Официальный help: create/edit/**schedule drafts**. Примеры промптов — create/schedule, не «publish now» | Да: scheduled draft Typefully сам публикует | Да, это основной путь |
| Typefully REST | Да, `publish_at="now"` (async, poll `publish_state`) | Да | Да |
| Buffer MCP | Нет. FAQ: «Will my AI assistant post without my permission? Nope — you're always the last step. Posts land as drafts or scheduled» | Да, если агент ставит scheduled, не draft | Да, это дефолт |
| Eden | Да (по обзору 19.08.2026) | Да | — |

Typefully конкретно: MCP — очередь/черновик/расписание. Это не «MCP не умеет постить»; это «MCP кладёт в Typefully, Typefully постит в назначенный слот». Немедленный эфир — REST `publish_at="now"` или UI. Eden 19.08.2026 прямо пишет: Typefully MCP = «Yes, drafts» / «drafts-and-queue server by design». Agent Skills страница Typefully говорит «schedule and publish» — это про продукт целиком, не про набор MCP-tools. Для 1–3 постов/день schedule на next-free-slot достаточно, если готовы, что пост живёт в очереди минуты/часы, а не уходит в ту же секунду.

F. Пункт 4 — лимиты и подводные камни

Месячные посты на free:

- Buffer Free: 10 scheduled **на канал**, refill (не «10 навсегда», но и не безлимит). 1 тред.
- Typefully Free: снимки 10 или 15 постов/мес (официальный виджет не прочитался). Для 1–3/день мало.
- Metricool Free: 20 scheduled.
- Ayrshare Basic: 20 realtime/мес.
- OpenTweet trial 7 дней.
- Postiz Cloud: 7 дней, не вечный free.
- Outstand: free есть, квоту живьём не мерил.

Аккаунты: Buffer считает канал = один профиль, $5/канал. Postiz Standard = 5 каналов. Typefully — social set (группа профилей одного бренда); точная шапка на live-виджете не считана.

Rate limit: Buffer API Free 100/15 мин, 250/сутки, 3000/мес. Postiz community-MCP упоминает 30 req/hour на public API — это лимит API Postiz, не X. X PPU режет уже на стороне X.

Ссылка в первый комментарий:

- Buffer: First comment scheduling с Essentials, на Free нет.
- Typefully: AutoPlug (добор/ссылка после треда), применяется как account-level и API его подхватывает. Это ближайший аналог «ссылка в первый коммент» на X.
- Postiz: Post comments + Internal/Global Plugs на всех платных Cloud.
- Schedulala: first comments на 8 из 12 «через одно API field».

Треды: Typefully (нативная нарезка, 4 newline / threadify). Buffer threaded posts: 1 на Free, безлимит на платных. Postiz: несколько элементов в `postsAndComments`. OpenTweet: до 25. X Articles в Typefully планируются, публиковать Article нужен X Premium.

Подводные камни, которые ломают агента:

- Ayrshare + Claude.ai personal connector: tools/list зелёный, каждый tool call 403 code 102.
- Typefully MCP на Claude: нужен Pro/Max.
- Buffer MCP на Free упрётся в 10 слотов.
- Посты со ссылками: если рельса BYO X — $0.20/шт.
- С февраля 2026 X запретил programmatic replies на self-serve (кроме ответа, когда вас упомянули). Enterprise исключён. OpenTweet это фиксирует.

G. Пункт 5 — риск банов 2025–2026

Что открыл:

- Правила X: планирование своего контента через официальный API — разрешённый класс. Нельзя inauthentic automation, дубли, накрутка. Typefully в API-доках ставит это первым предупреждением: нарушение X automation rules → бан аккаунта, не «бан Typefully».
- Unfollr 08.04.2026: scheduling через Buffer/Hootsuite/Typefully = allowed; 50+ твитов/день уже рискованно.
- Март 2026: волна «inauthentic behavior». Никита Бир: баг спам-фильтра ~12 часов, ~99% откатили. Полезный урок: третьестороннее приложение отвечает как вы («you are ultimately responsible for third-party applications you authorize»).
- Июнь 2026 bot purge: ~208 аккаунтов/мин на пике. Резали авто-engagement, датацентровые сессии, слишком ровный каденс. Аккаунт, который только шедулит и никогда не открывает X руками, выглядел хуже.
- SocialClaw 04.07.2026: серверы из их списка используют official APIs; ToS ломает **браузерная** автоматизация, не Buffer-класс.
- Hootsuite help (обновлялась 2026): X отваливается из-за lock/revoke/suspend — это не «Hootsuite забанил», это X. После апелляции канал переподключают, scheduled в Hootsuite не всегда теряются при revoke.

Официальный API vs браузер:

- Официальный API (всё в таблице выше, кроме явных scraper MCP вроде Bright Data/Apify): риск как у любого планировщика. Не ноль — волны 2026 ловили и легитимных.
- Headless Chrome / Playwright против x.com: высокий риск, серая зона, не берите как рельсу автопостинга.

Если сервис закроется или потеряет X API:

- Запланированные посты живут **у них**, не в X. Закрылся Buffer/Typefully — очередь не уезжает на x.com. Нужен экспорт и ручной перенос.
- Ayrshare-модель 2026 хуже: если у вас BYO ключи, посты идут вашим приложением; сервис-прослойка умерла — ключи остаются, но MCP/API-обвязка пропадает.
- Revoke приложения в x.com/settings/connected_apps мгновенно гасит рельсу. Переподключение = новый OAuth, старые scheduled могут застрять.
- Self-host: очередь ваша, но без живых токенов X ничего не уедет.

H. Пункт 6 — покрытие дев-площадок

Нужно: Mastodon, Bluesky, Reddit, Dev.to, Hashnode, Telegram-каналы, Discord, Medium.

| | Mastodon | Bluesky | Reddit | Dev.to | Hashnode | Telegram | Discord | Medium |
|---|---|---|---|---|---|---|---|---|
| Postiz | да | да | да | да | да | да | да | да |
| Typefully | да | да | нет | нет | нет | нет | нет | нет |
| Buffer | да | да | нет | нет | нет | нет | нет | нет |
| Mixpost Pro | да | смотреть тариф | — | — | — | да в части сборок | — | — |
| Ayrshare | нет в прайсе | да | да | нет | нет | да | нет | нет |
| Outstand | нет | да | да | нет | нет | нет | нет | нет |
| SocialClaw | нет | нет | да | нет | нет | да | да | нет |
| Metricool | нет | да | нет | нет | нет | нет | нет | нет |
| Publora | да | да | нет | нет | нет | да | нет | нет |
| Schedulala | да | да | нет | нет | нет | да | нет | нет |
| OpenTweet | нет | зеркало в docs | нет | нет | нет | нет | нет | нет |
| Eden | нет | нет | нет | нет | нет | нет | нет | нет |

Единственный, кто закрывает **весь** дев-набор одним MCP — Postiz. Остальные надо стыковать.

I. Пункт 7 — несколько сервисов сразу

Конфликта OAuth нет. X держит список connected apps. Buffer + Typefully + Postiz на одном @Tony_Stef_ — штатно. Каждый постит своим приложением.

Что реально ломается:

- Дубль, если агент кросспостит один текст в Typefully и Postiz на один X.
- Разные очереди, разные «next free slot», можно случайно выложить два поста за минуту — это уже паттерн, который 2026-фильтры не любят.
- Analytics разъедутся (половина в Typefully, половина в Postiz).
- Ayrshare BYO + Buffer одновременно на одном X-приложении не смешивать: Ayrshare хочет **ваши** consumer keys, Buffer использует свои. Это разные app id.

Рабочая схема без войны: один «рот» на X (Typefully или Buffer или OpenTweet), второй MCP только на площадки, которых нет у первого (Postiz на Dev.to/Hashnode/Reddit/Telegram). Агенту в промпте жёстко: X → рельса 1, остальное → рельса 2.

J. Что это значит для вашего 403

Своё developer-приложение без PPU-enrollment не починить планировщиком: self-host Postiz/Mixpost и Ayrshare снова ударятся в тот же 403 или в 419/402. Лечить X-рельсу нужно сервисом класса A.

Ссылочные посты делают прямой X API ещё хуже ($0.20). Планировщик с enterprise-договором это прячет внутри подписки.

Автономия Claude: Buffer и Typefully MCP — очередь. Postiz/OpenTweet/Outstand — могут сразу в эфир. Для 1–3/день очередь безопаснее (совпадает с советом OpenTweet: агенту давать schedule, не publish_now). Для «отправил в чат — уже в ленте» нужен Postiz Cloud или OpenTweet.

Источники и даты (всё открыто в этом прогоне)

- Buffer MCP и FAQ «always the last step»: https://buffer.com/mcp — живое, сентябрь 2026
- Buffer цены, каналы, first comment, 10 scheduled на Free: https://buffer.com/pricing — 15.09.2026
- Buffer developer MCP: https://developers.buffer.com/guides/integrations/mcp.html
- Typefully MCP: https://support.typefully.com/en/articles/13128440-typefully-mcp-server — updated ~2 weeks ago
- Typefully agents + endpoint: https://typefully.com/agent-skills и https://typefully.com/ai-agents
- Typefully API v2, `publish_at="now"`, AutoPlug, X rules: https://typefully.com/docs/api и https://support.typefully.com/en/articles/8718287-typefully-api — 05.05.2026
- Postiz MCP tools: https://docs.postiz.com/mcp/introduction — 02.09.2026
- Postiz MCP marketing + 30 платформ: https://postiz.com/mcp
- Postiz цены: https://postiz.com/pricing
- Postiz self-host X keys: https://docs.postiz.com/self-host/providers/x-twitter — 19.08.2026
- Mixpost MCP: https://docs.mixpost.app/mcp/ — 04.09.2026
- Mixpost X PPU: https://docs.mixpost.app/services/social/x/
- Ayrshare MCP connect + BYO headers: https://www.ayrshare.com/docs/additional/mcp-action-connect
- Ayrshare X BYO с 31.03.2026, «раньше платили мы»: https://www.ayrshare.com/docs/dashboard/connect-social-accounts/x-twitter-byo-keys
- Ayrshare pricing: https://www.ayrshare.com/pricing
- Metricool MCP URL и права publish: https://help.metricool.com/en/article/how-to-connect-metricools-mcp-1364s63/ ; free 20 posts: связанный help + Eden
- Outstand MCP 28 tools: https://mcpservers.org/servers/outstand-mcp и https://www.outstand.so/mcp
- OpenTweet MCP, $0.20/link, no dev account: https://opentweet.io/mcp и https://opentweet.io/blog/which-ai-agents-can-post-to-x-2026 — 09.09.2026
- SocialClaw сравнение 04.07.2026: https://getsocialclaw.com/blog/best-social-media-mcp-servers
- bundle.social MCP: https://bundle.social/social-media-mcp-server — July 2026
- Eden обзор 19.08.2026: https://eden.so/blog/best-social-media-mcp-servers/
- Outstand inventory «who actually publishes» 14.07/30.08.2026: https://www.outstand.so/blog/best-social-media-mcp-servers-2026
- Publora MCP: https://github.com/publora/mcp-server
- Schedulala MCP URL (из каталога): `https://schedulala.com/api/mcp`
- X 403 client-not-enrolled: https://devcommunity.x.com/t/client-not-enrolled-app-stuck-on-free-deprecated-25-ppu-credits-loaded/272570 — 05.08.2026
- X Basic→PPU: https://devcommunity.x.com/t/important-update-legacy-x-api-basic-plans-are-moving-to-pay-per-use-ppu/266305 — 21.05.2026
- Март 2026 inauthentic wave: https://postory.io/blog/x-inauthentic-behavior-suspension — 08.09.2026
- Июнь 2026 bot purge: https://socialnexis.com/guides/x-june-2026-bot-purge-anatomy — 15.06.2026

Возможно устарело (старше июня 2026 или вендорский маркетинг без живого тарифа): часть агрегаторов Typefully pricing; post-bridge July 2026 («у Hootsuite нет MCP» — это уже неправда по Eden/Outstand августа); SocialClaw/Outstand как саморекомендации.

Проверено: живые страницы Buffer, Typefully MCP/API, Postiz, Mixpost, Ayrshare BYO, Metricool help, OpenTweet, Outstand listing, Publora README. Не проверено: точная цифра Typefully Creator на виджете (JS), точный URL Hootsuite Perch MCP, полный прайс bundle.social/Schedulala/SocialClaw, эмпирика «Buffer MCP умеет ли share now вопреки FAQ».
