# DR26-09-15-ZB-04-0758 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

[⏰ вторник, утро, 08:53, 15 сентября 2026]

# Планировщики соцсетей с MCP как рельса автопостинга — сентябрь 2026

Сразу главный вывод под твою ситуацию (X-app без pay-per-use, 403 client-not-enrolled): тебе нужен **хостящийся сервис, который держит доступ к X на СВОЕЙ стороне и пускает тебя по OAuth**, а не self-hosted (Postiz/Mixpost требуют собственное X-dev-приложение — ровно та стена, в которую ты уже упёрся). Такие сервисы с официальным MCP есть: **Typefully, Buffer, Outstand**.

---

## A. Сравнительная таблица найденных сервисов

| Сервис | MCP-эндпоинт | Авторизация | Площадки | Нужен свой X-ключ? | Бесплатный тариф | Платный тариф | Публикует или только черновик | Главное ограничение |
|---|---|---|---|---|---|---|---|---|
| **Typefully** | `https://mcp.typefully.com/mcp` | OAuth (в браузере); API-key опц. для headless | X, LinkedIn, Threads, Bluesky, Mastodon | **Нет** — Typefully держит доступ | $0: 1 соц-набор, **15 постов/мес**, MCP/API включены | Pro $8/соц-набор/мес (год), до 10 наборов; Business $18 | **И то, и другое**: publish-now ИЛИ schedule существующего черновика | Узкий набор площадок; 15 постов/мес на free = ~1 короткий день при 1-3/день |
| **Buffer** | `https://mcp.buffer.com/mcp` | Bearer API-key (из настроек Buffer) | X, LinkedIn, Instagram, FB, Threads, Bluesky, Mastodon, Pinterest, TikTok, YouTube, Google Business | **Нет** — Buffer держит доступ | MCP бесплатен на всех планах вкл. free; free-план = **3 канала**, очередь | Платит **за канал**: ~$25-30/мес за 5 каналов | **Публикует**: режимы `shareNow`, `shareNext`, `addToQueue`, `customScheduled` | Биллинг по каналам; MCP-ключ = доступ ко всему аккаунту (нет узкого скоупа) |
| **Outstand** | `https://mcp.outstand.so/mcp` | OAuth в один клик ИЛИ Bearer | X, LinkedIn, IG, FB, Threads, TikTok, YouTube, Bluesky, Pinterest, Google Business, Vimeo, Reddit (12) | **Нет** — Outstand держит доступ | Free-тариф есть (без карты) | $19/мес = 3000 постов; сверх — $0.007/пост | **Публикует** сразу ИЛИ планирует; кросс-постинг одним запросом | Нет dev-площадок (Dev.to/Hashnode/Medium/Telegram/Discord); моложе всех (live с марта 2026) |
| **Postiz (cloud)** | `https://api.postiz.com/mcp/<API_KEY>` | Приватный API-key | 30+: X, LinkedIn, Reddit, Discord, Telegram, Mastodon, Bluesky, **Medium, Dev.to, Hashnode**, WordPress, IG, TikTok, YouTube, Pinterest… | Cloud: **нет** (держат они). Self-host: **ДА, свой X-app** | Cloud «от $0», self-host бесплатен | Cloud от $29/мес за 5 каналов | Публикует И планирует | Шире всех по dev-площадкам; но self-host = свой X-ключ + Postgres/Redis/Temporal |
| **DemandBird** | (свой MCP-эндпоинт) | plain-English через Claude/ChatGPT | X (фокус) | Нет (хостится) | — (проверить) | — | Draft/schedule/manage X-постов | Узко X-центричный; данных по тарифам мало |
| **SchedPilot** | (свой MCP) | — | мультиплатформа | Нет (хостится) | — | — | Планирование | Мало публичных данных, молодой |
| **Metricool** | офиц. MCP | план-based | мультиплатформа + аналитика | Нет | Работает на free-тарифе | — | Планирование + аналитика | Заточен под аналитику/маркетинг, не под dev-аудиторию |
| **PostEverywhere** | хостится, 38 инструментов | — | 11 платформ | Нет | Нет (от $9/мес, 7 дн триал) | от $9/мес | Посты/кампании/аналитика | Нет free-тарифа |
| **Postiz/Mixpost (self-hosted)** | `/api/mcp/{KEY}` (Postiz) | API-key | 30+ (Postiz) / много (Mixpost) | **ДА — свой ключ для КАЖДОЙ площадки** | Софт бесплатен | хостинг $5-15/мес | Публикует И планирует | **Именно твоя стена**: X требует свой dev-app (Native App, OAuth 1.0a) |
| **Hootsuite / Publer / Sprout / SocialPilot / Later** | ❌ нет MCP (только REST API) | — | — | — | — | — | — | MCP-слоя нет на сент. 2026 |

Данные собраны 15.09.2026 из живых источников (ниже). Обзорные блоги помечают, что «рынок двигался быстро в 2026» — цифры тарифов сверяй перед оплатой.

---

## B. Разбор по твоим пунктам

**1. Кто держит официальный MCP (сент. 2026):** Typefully, Buffer, Outstand, Postiz (cloud+self-host), Metricool, PostEverywhere, плюс более узкие DemandBird/SchedPilot. **НЕ имеют MCP:** Hootsuite, Publer, Sprout Social, SocialPilot, Later, SocialBee, Loomly, Planoly (только REST API либо ничего).

**2. Развилка «кто платит X» — ЭТО ГЛАВНОЕ для тебя:**
- **Держат доступ на своей стороне (ты только OAuth-логинишься, свой dev-app НЕ нужен):** Typefully, Buffer, Outstand, Postiz-**cloud**, Metricool. Это обходит твой 403 — они approved-партнёры X, биллинг X на них.
- **Bring-your-own-key (нужен свой dev-app + биллинг):** Postiz-**self-hosted**, Mixpost. Документация Postiz прямо требует: «создай свой X-app, выбери Native App для OAuth 1.0a». Это ровно твоя текущая стена — **не бери self-host для X**.
- Для self-hosted **бесплатны ключи** там, где площадка сама не берёт денег за API: **Mastodon, Bluesky, Telegram (бот), Discord (webhook/бот), Reddit, Dev.to, Hashnode, Nostr, Mastodon**. Платный/огороженный — именно X, а также Meta (IG/FB) требует бизнес-верификацию. То есть self-host идеален для dev-площадок, но спотыкается ровно на X.

**3. Черновик vs публикация:**
- **Typefully MCP ПУБЛИКУЕТ** (не только черновик): есть действие «immediately publish an existing draft to all configured platforms» и «schedule at a specific time». Твой прямой вопрос — да, публикует, а не только планирует.
- Buffer — режимы `shareNow`/`shareNext`/`addToQueue`/`customScheduled` (публикация и очередь).
- Outstand, Postiz — публикуют сразу или планируют.

**4. Лимиты/подводные камни:**
- Typefully free = **15 постов/мес** (при 1-3/день выработаешь за ~неделю → упрёшься; Pro $8 снимает).
- Buffer free = **3 канала**, платишь за канал.
- Outstand $19 = 3000 постов/мес — щедро по объёму.
- «Ссылка в первый комментарий» — это фича IG/LinkedIn; в MCP-инструментах напрямую не гарантирована, под X (твой кейс со ссылками в постах) решается **тредом**: Typefully и Buffer оба тред-нативны (Typefully вообще заточен под треды X).
- Rate limits: Buffer — скользящие окна 15мин/24ч/30дн по плану.

**5. Риск блокировки:**
- Ключевой принцип X-2026: **«автоматизируй создание и планирование контента — НИКОГДА не автоматизируй engagement (авто-фоллоу/лайк/реплай)»**. Планирование СВОИХ постов через approved-инструмент — легально и безопасно.
- Была **мартовская 2026 волна банов** за «inauthentic behavior», но целила в engagement-автоматизацию, не в планировщики.
- **Явное предупреждение под твой пункт 7:** одновременное использование НЕСКОЛЬКИХ инструментов может выглядеть как «coordinated automation» — комбинированные API-вызовы от разных тулов повышают риск. Вывод: если делишь X и dev-площадки между сервисами — держи **X на ОДНОМ сервисе**, а второй пусть работает по dev-площадкам, где нет X-вызовов.
- Официальный API (Typefully/Buffer/Outstand) безопаснее браузерной автоматизации: ты остаёшься ответственным за то, что авторизовал, но approved-партнёр = whitelисте.
- Если сервис закроется/потеряет доступ к API: уже опубликованные посты остаются на площадке (они нативные), но очередь запланированных и черновики теряются. Держи исходники постов у себя (у тебя и так волт) — это страховка.

**6. Покрытие dev-среды (Mastodon, Bluesky, Reddit, Dev.to, Hashnode, Telegram, Discord, Medium):**
- **Только Postiz** покрывает ВСЮ эту связку, включая Dev.to, Hashnode, Medium, Telegram, Discord, Reddit — то есть именно площадки, где живут инженеры. Это его сила.
- Outstand добавляет Reddit, но не Dev.to/Hashnode/Medium/Telegram/Discord.
- Typefully/Buffer — только Mastodon+Bluesky из dev-списка.

**7. Несколько сервисов одновременно:** технически возможно — каждый MCP подключается как отдельный сервер в Claude. Конфликтов OAuth нет (разные аккаунты/токены). Но: (а) риск «coordinated automation» на X — держи X на одном; (б) дублирование инструментов (у двух серверов есть «post to X») — Claude может путать, разведи по площадкам явно в промпте/скилле.

---

## C. Три именованные рекомендации

### 🥇 Дешевле всего и надёжнее по X → **Typefully (Pro $8/мес)**
Держит X на своей стороне (обходит твой 403), MCP реально **публикует**, тред-нативен (твои ссылки укладываются в тред), OAuth в один клик. Free-план (15 постов/мес) не потянет 1-3/день, но $8/соц-набор — самый дешёвый вход в «настоящую публикацию». Минус — узкие площадки (X, LinkedIn, Threads, Bluesky, Mastodon), для dev-заметности маловато.

### 🌍 Шире всего по площадкам (dev-среда) → **Postiz (cloud, от $0/$29)**
Единственный, кто закрывает **Dev.to, Hashnode, Medium, Telegram, Discord, Reddit, Mastodon, Bluesky** — ровно карту, где живут инженеры, а не маркетологи. Бери **cloud-версию**, не self-host: cloud держит X-доступ у себя, self-host заставит регистрировать свой X-app (твоя стена). Cloud «от $0» на старте, $29 за 5 каналов при масштабе.

### 🛡 Надёжнее всего по объёму/цене-за-пост → **Outstand ($19 = 3000 постов)**
Самый предсказуемый по деньгам при 1-3/день (3000 постов/мес — вагон запаса), OAuth в один клик, держат X у себя, 12 площадок вкл. Reddit, publish-now. Минус — моложе всех (live март 2026) и без «глубоких» dev-площадок.

**Мой совет как кофаундера (🎙 Майк):** идёшь верно, что ищешь именно «кто держит X у себя» — это единственный обход 403 без своего биллинга. Оптимальная связка под ДВЕ твои цели (заметность + автономный агент): **Postiz-cloud как основная рельса** (закрывает и X, и весь dev-пояс одним MCP — не надо два сервиса, снимается риск «coordinated automation»). Если бюджет = ноль и dev-площадки не срочны — **Typefully Pro $8** как минимальный честный старт по X. Один вопрос к тебе: тебе важнее ширина dev-охвата **сейчас** (→ Postiz) или минимальная цена **сейчас** с расширением потом (→ Typefully)?

---

## Источники (открыты 15.09.2026; данные быстро устаревают — тарифы сверяй)
- Typefully MCP (офиц.): https://support.typefully.com/en/articles/13128440-typefully-mcp-server
- Typefully MCP (Zapier): https://zapier.com/mcp/typefully
- Typefully pricing/free-план (Efficient App, 2026): https://efficient.app/apps/typefully
- Buffer MCP (офиц. developers): https://developers.buffer.com/guides/integrations/mcp.html
- Buffer MCP (Socialync, 2026): https://www.socialync.io/mcp-server/buffer
- Buffer MCP платформы (Glama): https://glama.ai/mcp/servers/ahernan2/buffer-mcp
- Outstand MCP: https://www.outstand.so/mcp
- Postiz MCP (офиц.): https://postiz.com/mcp
- Postiz X-provider (self-host свой ключ): https://docs.postiz.com/providers/x-twitter
- Postiz cloud vs self-host (Railway, авг’26): https://railway.com/deploy/postiz
- Обзор MCP-серверов (PostEverywhere, 2026): https://posteverywhere.ai/blog/social-media-mcp-servers
- Обзор (Socialync, «честный список»): https://www.socialync.io/blog/best-social-media-mcp-servers-2026
- Риск банов/правила X-автоматизации 2026: https://opentweet.io/blog/twitter-automation-rules-2026 и https://socialnexis.com/guides/twitter-automation-safe-2026
- DemandBird X-MCP: https://demandbird.com/integrations/twitter-mcp/

🧒 Простыми словами: у тебя своя дверь в X заперта (нужен платный ключ, а его нет). Есть сервисы-«швейцары», у которых дверь в X УЖЕ открыта — ты просто показываешь им пропуск (логин), и они постят за тебя. Самый дешёвый швейцар — Typefully ($8). Швейцар, который умеет постить ещё и на все «программистские» площадки — Postiz. Швейцар с самым большим запасом постов за фиксированную цену — Outstand ($19). Self-hosted штуки (Postiz-сам-ставишь, Mixpost) не бери для X — они снова попросят тот самый ключ, которого у тебя нет.
