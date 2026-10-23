# DR26-09-15-ZB-04-0758 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Срез рынка и короткий ответ

Дата проверки: 15 сентября 2026 года.

Короткий вывод:

- Официальный MCP уже есть более чем у 30 сервисов, однако многие новые продукты мало проверены временем.
- Формулировка «enterprise-договор с X» почти нигде публично не подтверждается. Практически важный критерий другой: сервис использует собственное X-приложение, а пользователь лишь подключает аккаунт по OAuth. Это устраняет необходимость собственного developer-app и enrollment в X pay-per-use.
- Self-hosted Postiz и Mixpost требуют собственные platform credentials. Для X это означает собственное developer-приложение и оплату X API.
- Typefully MCP умеет не только создавать черновики: `publish_at: "now"` публикует немедленно. Он также умеет планировать автопубликацию. [Документация Typefully MCP](https://support.typefully.com/en/articles/13128440-typefully-mcp-server), [API v2 migration guide](https://support.typefully.com/en/articles/13133296-typefully-api-v1-v2-migration-guide).
- Для @Tony_Stef наиболее рациональный старт: Buffer для X и основных сетей. Если действительно нужны Reddit, Dev.to, Hashnode, Telegram, Discord и Medium из одного места, Postiz Cloud Team покрывает их все.
- Нельзя считать собственное приложение X резервным путём, пока оно не enrolled в pay-per-use: платформа X прямо связывает доступ API с developer account, app и доступными credits. [X Developer Portal](https://docs.x.com/fundamentals/developer-portal), [X pay-per-use и Post Cap](https://docs.x.com/x-api/fundamentals/post-cap).

Под «полным списком» ниже понимаются публично обнаруженные к 15.09.2026 универсальные сервисы публикации, для которых сам производитель документирует MCP. Не включены неофициальные community-обёртки, Zapier/Composio как посредники, аналитические MCP без публикации и узкоспециализированные генераторы клипов. Единого официального реестра всех MCP-серверов не существует, поэтому абсолютную полноту рынка доказать нельзя.

B. Сравнительная таблица

Сокращения: «vendor» означает собственное приложение сервиса и OAuth пользователя, а не доказанный публично enterprise-контракт. «BYOK» означает собственные developer credentials пользователя.

| Сервис | MCP-эндпоинт | Авторизация | Основные площадки | Свой X-ключ | Бесплатный тариф | Платный тариф | MCP-действие | Главное ограничение |
|---|---|---|---|---|---|---|---|---|
| [Buffer](https://developers.buffer.com/guides/integrations/mcp.html) | `https://mcp.buffer.com/mcp` | Bearer API key | X, LinkedIn, Facebook, Instagram, Threads, Bluesky, Mastodon, Pinterest, TikTok, YouTube, Google Business | Нет, vendor | 3 канала; до 10 запланированных постов одновременно на канал; 3 000 API-запросов/30 дней | Essentials от $5/канал/мес. при годовой оплате | Сейчас, очередь, расписание, draft | Цена растёт за каждый канал; первый комментарий только на платных планах. [Тарифы](https://buffer.com/pricing) |
| [Typefully](https://support.typefully.com/en/articles/13128440-typefully-mcp-server) | `https://mcp.typefully.com/mcp` | OAuth; API key для headless | X, LinkedIn, Threads, Bluesky, Mastodon; в продукте также Substack Notes | Нет, vendor | Бесплатный план есть; актуальный точный лимит публично не зафиксирован | Starter был $12.50/мес. в декабре 2025, поэтому цена ⚠ возможно устарела | Немедленная публикация, расписание, drafts, threads | Не покрывает Reddit, Dev.to, Hashnode, Telegram, Discord и Medium. [Поддерживаемые сети](https://support.typefully.com/en/articles/8728077-what-and-where-can-i-publish-with-typefully) |
| [Metricool](https://metricool.com/metricool-mcp-claude/) | `https://ai.metricool.com/mcp` | OAuth либо token + user ID | X, LinkedIn, Facebook, Instagram, TikTok, YouTube, Pinterest, Threads, Bluesky, Twitch | Нет, но X add-on | Free: 1 brand, 20 posts/мес.; без X и LinkedIn | Starter от €16/мес.; X дополнительно €10/$10 за аккаунт/мес. | Сейчас и расписание | X оплачивается отдельно за каждый аккаунт. [X add-on](https://help.metricool.com/your-guide-to-the-x-twitter-add-on-wt5wy), [тарифы](https://metricool.com/pricing/) |
| [Postiz Cloud / self-hosted](https://docs.postiz.com/mcp/introduction) | `https://api.postiz.com/mcp` | OAuth или Bearer API key | X, LinkedIn, Meta, Threads, Bluesky, Mastodon, Reddit, Lemmy, Discord, Telegram, Slack, Medium, Dev.to, Hashnode, WordPress и др. | Cloud: нет; self-host: да | Self-hosted AGPL бесплатен | Cloud Standard $29/мес., 5 каналов, 400 постов; Team $39, 10 каналов, unlimited | Draft, schedule, publish now | Self-host требует настройки приложений площадок; Cloud Standard ограничен 400 постами. [Тарифы](https://postiz.com/pricing), [инструменты MCP](https://docs.postiz.com/mcp/tools) |
| [Mixpost](https://docs.mixpost.app/mcp/) | `https://host/<MIXPOST_CORE_PATH>/mcp` | Bearer access token | X, Facebook, Instagram, LinkedIn, Mastodon, Bluesky, Threads, TikTok, YouTube, Pinterest и др. | Да | Lite бесплатно, но без MCP и с урезанными сетями | Pro $299 one-time; Enterprise $1 199 | Draft, schedule, publish | Для X прямо требуется собственный paid/pay-per-use developer app. [Настройка X](https://docs.mixpost.app/services/social/x/), [тарифы](https://mixpost.app/pricing) |
| [Zernio](https://docs.zernio.com/mcp) | `https://mcp.zernio.com/mcp` | OAuth или Bearer API key | X, Instagram, TikTok, LinkedIn, Facebook, YouTube, Threads, Reddit, Pinterest, Bluesky, Snapchat, GMB; Telegram, Discord, Slack | Нет, vendor | 2 подключённых аккаунта | Далее $6 за аккаунт/мес.; X: $0.015 обычный пост, $0.20 пост со ссылкой | Сейчас, расписание, draft | Для задачи со ссылками X стоит примерно $6–18/мес. при 1–3 постах в день. [Тарификация](https://docs.zernio.com/pricing), [сети](https://zernio.com/agents) |
| [Bundle.social](https://bundle.social/social-media-mcp-server) | Локальный stdio: `npx bundlesocial-mcp` | API key | X, Meta, TikTok, LinkedIn, YouTube, Pinterest, Reddit, Threads, Bluesky, Mastodon, Discord, Slack, GMB | Нет, vendor | 20 постов/мес., до 3 аккаунтов; X до 5/день | Pro $100/мес., 10 000 постов | Публикация, расписание, аналитика | X metered: $0.015 текст, $0.20 ссылка; публичного remote URL нет. [API для агентов](https://bundle.social/social-media-api-for-ai-agents), [лимиты](https://info.bundle.social/api-reference/rate-limits) |
| [Ayrshare](https://www.ayrshare.com/) | `https://api.ayrshare.com/mcp` | Bearer API key | 14 сетей, включая X, Bluesky, Reddit, Telegram, LinkedIn, Meta, TikTok, Threads, Pinterest, YouTube | Нет, vendor | Только 28-дневный trial | Premium $149/мес. за один social profile; Launch $299 за 10 | Сейчас, расписание, история, комментарии, сообщения | Очень дорого для одного автора. [Тарифы](https://www.ayrshare.com/pricing) |
| [Planable](https://planable.io/blog/how-to-schedule-social-media-posts-with-mcp/) | `https://mcp.planable.io/mcp` | OAuth | X, Facebook, Instagram, LinkedIn, TikTok, YouTube, Pinterest, GMB, Threads, Bluesky | Нет, vendor | Первые 50 постов; X отключён | Basic $33/workspace/мес. annual, 60 posts; Pro $49, 150 | Только создаёт drafts для проверки | Не автономная рельса: MCP-пост должен пройти UI/approval. [Тарифы](https://planable.io/pricing) |
| [Agorapulse](https://support.agorapulse.com/en/articles/12960496-model-context-protocool-mcp-integration-explained) | `https://api.openmcp.agorapulse.com/mcp` | OAuth | X, Meta, LinkedIn, TikTok, YouTube, GMB, Threads, Pinterest, Bluesky | Нет, vendor | Free удалён 6 июля 2026; MCP недоступен Legacy/Free | Standard $99 monthly/$79 annual за пользователя, 10 profiles | Только drafts | MCP beta и не может автоматически публиковать. [Цены](https://support.agorapulse.com/en/articles/8762696-pricing-per-user-explained) |
| [Hootsuite Perch](https://www.hootsuite.com/integrations/mcp) | `https://mcp.hootsuite.com/perch` | OAuth/workspace | X, Facebook, Instagram, LinkedIn, TikTok, YouTube, Threads, Bluesky, Pinterest | Нет, vendor | 14-дневный trial | Standard $99/user/мес. annual, 10 accounts | Сейчас и расписание | Высокая цена; почти нет dev-native площадок. [Тарифы](https://www.hootsuite.com/plans) |
| [SocialPilot](https://help.socialpilot.co/article/882-how-to-connect-socialpilot-with-claude-using-mcp-server) | `https://mcp.socialpilot.co/mcp` | OAuth; возможен URL с API key | X, Meta, LinkedIn, TikTok, Threads, Bluesky, YouTube, Pinterest, GMB | Нет, vendor | 14-дневный trial | Essentials $30 monthly/$25.50 annual, 7 accounts | Сейчас, расписание, draft | Нет Mastodon, Reddit и публикационных dev-платформ. [MCP tools](https://help.socialpilot.co/article/885-socialpilot-mcp-server-and-supported-integrations), [цены](https://www.socialpilot.co/plans) |
| [Vista Social](https://support.vistasocial.com/hc/en-us/articles/40393513382043-Connect-Your-AI-Assistant-to-Vista-Social-MCP-integration) | Индивидуальный universal MCP URL в настройках | OAuth либо account link/key | X, Meta, LinkedIn, TikTok, YouTube, Threads, Bluesky, Reddit и др. | Нет, но X add-on | 14-дневный trial | Professional $79/мес., 15 profiles; X ещё $29/account/мес. | Draft и schedule; немедленный режим неочевиден | X дорог; документация расходится в описании MCP rate limits. [Инструменты](https://support.vistasocial.com/hc/en-us/articles/46525029430299-Vista-Social-s-Available-MCP-Tools), [цены](https://vistasocial.com/pricing) |
| [PostSyncer](https://docs.postsyncer.com/mcp/overview) | `https://postsyncer.com/mcp` | OAuth либо Bearer | X, Instagram, Facebook, LinkedIn, TikTok, YouTube, Pinterest, Threads, Telegram, Bluesky, Mastodon | Нет, vendor | 7-дневный trial | Starter $29 monthly/$24 annual, 10 accounts | Публикация, расписание, комментарии, аналитика | API Starter: 100 запросов/день и 10 на платформу. [Цены](https://postsyncer.com/comment-management) |
| [Post Bridge](https://www.post-bridge.com/mcp/docs) | `https://www.post-bridge.com/api/mcp/mcp` | OAuth2 или `pb_live` API key | X, Instagram, LinkedIn, Facebook, TikTok, YouTube, Bluesky, Threads, Pinterest, GMB | Нет, vendor | 7-дневный trial | Starter $29/мес., 5 accounts; Creator $39, 15 | Draft, schedule, publish now | Нет Mastodon, Reddit, Telegram, Discord и long-form dev-площадок. [Тарифы](https://www.post-bridge.com/pricing) |
| [Schedchie](https://www.schedchie.com/help/connect-an-ai-assistant) | `https://mcp.schedchie.com/mcp` | OAuth | X, Instagram, Facebook, TikTok, YouTube, LinkedIn, Pinterest, Bluesky, Threads, Mastodon, Telegram, GMB | Нет, X credit bundle | Trial 14 дней | €7.99/мес.; X ещё €4.99 за 30 постов/30 дней | Schedule/edit/retry; отдельный publish-now не заявлен | Для 1–3 X-постов/день потребуется 1–3 пакета X ежемесячно. [X add-on](https://www.schedchie.com/help/x-twitter-add-on), [AI plan](https://www.schedchie.com/ai) |
| [SocialRobot](https://socialrobot.io/mcp) | `https://socialrobot.io/api/mcp` | OAuth | X, Instagram, LinkedIn, TikTok, Pinterest, Threads, Facebook, Bluesky, Mastodon | Нет, vendor | 2 accounts, 15 posts/мес.; без X | Pro $29/мес., 20 accounts, X включён | Schedule/publish | X начинается только с Pro; документация лимитов неоднородна. [Тарифы](https://socialrobot.io/) |
| [Nuelink](https://docs.nuelink.com/mcp/) | `https://mcp.nuelink.com/mcp?api_key=…` | API key в URL | X, Meta, LinkedIn, TikTok, YouTube, Pinterest, GMB, Telegram, Threads, Bluesky, Mastodon | Нет, vendor | 7-дневный trial | Standard около $12/мес., 10 channels; Premium около $32, 30 | Сейчас, draft, schedule, queue | MCP помечен early alpha; ключ в URL повышает риск утечки через логи. [Цены](https://nuelink.com/pricing) |
| [OneUp](https://docs.oneupapp.io/docs/oneup-mcp/chatgpt-setup) | `https://feed.oneupapp.io/mcp/oneup` | OAuth-коннектор + OneUp key | X, Meta, LinkedIn, TikTok, YouTube, Pinterest, GMB, Snapchat, Bluesky, Threads, Discord | Нет, vendor | 7-дневный trial | Basic $25 monthly/$15 annual, 5 accounts | Публикация и расписание | X-ссылки требуют add-on $50/мес. за 250 ссылочных постов; иначе URL удаляется. [X links](https://help.oneupapp.io/en-us/article/link-posts-on-x-whao47), [цены](https://www.oneupapp.io/price) |
| [Hermoso](https://hermoso.ai/social-media-scheduler/) | `https://app.hermoso.ai/mcp` | OAuth | X, Meta, Threads, TikTok, YouTube, LinkedIn, Pinterest, Bluesky, Telegram; Reddit immediate-only | Нет, metered | 250+ earnable credits | Starter $19; Pro $49; Studio $149 | Сейчас и расписание | Цена X рассчитывается перед публикацией; нет first comment; молодая платформа |
| [PostFast](https://postfa.st/blog/social-media-scheduler-api-build-a-posting-automation-with-postfast) | `https://mcp.postfa.st/mcp`; локально `npx postfast-mcp` | OAuth или API key | X, Instagram, Facebook, TikTok, LinkedIn, YouTube, Threads, Pinterest, Bluesky, Telegram, GMB | Нет, vendor | 7-дневный trial | Starter €12 monthly/€10 annual, 4 accounts, 150 scheduled posts | Draft, schedule, publish | Лимит 150 scheduled на Starter; нет Mastodon/Reddit/Discord. [Цены](https://postfa.st/pricing) |
| [Upload-Post](https://docs.upload-post.com/guides/mcp-server-integration/) | `https://mcp.upload-post.com/mcp` | OAuth 2.1 или API key | X, TikTok, Instagram, YouTube, LinkedIn, Facebook, Threads, Reddit, Bluesky, Pinterest, GMB, Discord, Telegram | Нет, add-on | 10 uploads/мес., 2 profiles | Basic от $16/мес. annual; X links ещё $19/мес., максимум 50 | Publish, schedule, analyze | Без X add-on ссылки вырезаются; Reddit API сейчас возвращает 503. [X](https://www.upload-post.com/platforms/x), [Reddit status](https://docs.upload-post.com/api/overview/) |
| [Zoho Social](https://www.zoho.com/social/mcp.html) | Персональный сервер на `mcp.zoho.com` | Zoho OAuth, выбор разрешённых tools | X, Facebook, Instagram, LinkedIn, GMB, YouTube, Pinterest, TikTok, Mastodon, Threads, Bluesky; Telegram Business на старших планах | Нет, vendor | 1 brand, 6 channels; без scheduling | Региональные цены, смотреть текущий checkout | Publish, schedule, draft | Endpoint создаётся для аккаунта; бесплатный план не является полноценной рельсой. [Тарифы](https://www.zoho.com/social/pricing.html) |
| [Sprout Social](https://support.sproutsocial.com/hc/en-us/articles/41085761844365-ChatGPT-Integration-Overview-and-Setup) | `https://mcp.sproutsocial.com/mcp` | OAuth | В MCP сейчас TikTok published insights/performance | Не применимо | 30-дневный trial | Essentials $79/seat/мес. annual | Не публикует | Не подходит: MCP ориентирован на ChatGPT и аналитику, Claude не поддержан. [Цены](https://sproutsocial.com/pricing) |
| [Outstand](https://www.outstand.so/facts) | `https://mcp.outstand.so/mcp` | API key | X, LinkedIn, Instagram, TikTok, Facebook, Threads, Bluesky, YouTube, Pinterest, GMB, Vimeo, Reddit | Да для X, GMB, Vimeo, Reddit | Нет | $19/мес., 3 000 posts, unlimited accounts | Сейчас и расписание | BYOK X: исходную проблему не решает |
| [PurrPlan](https://purrplan.ai/en/mcp-social-media-scheduler/) | `https://app.purrplan.ai/api/mcp` | Scoped Bearer token | X, Instagram, TikTok, LinkedIn, Facebook, YouTube, Bluesky, Threads, Pinterest, Telegram | Нет, credits | 7-дневный trial | Pro €19/мес.; lifetime founders €99 | По умолчанию draft; write-token разрешает direct/schedule | Facebook Groups и personal profiles работают через browser extension, а не официальный publishing API. [Facebook method](https://purrplan.ai/en/reseau/facebook/), [X credits](https://purrplan.ai/en/reseau/x-twitter/) |
| [Publora](https://github.com/mcp/com.publora/mcp-server) | `https://mcp.publora.com` | OAuth 2.1 или Bearer key | X, LinkedIn, Instagram, Threads, TikTok, YouTube, Facebook, Bluesky, Mastodon, Telegram | Нет, vendor | 3 accounts, 15 posts/мес.; без X | Pro $5.99/account/мес., минимум 5 accounts = $29.95 | Schedule/publish | Бесплатный X отсутствует; Pinterest подключается, но публикация ещё не поддерживается. [Цены](https://publora.com/pricing), [ограничение Pinterest](https://www.publora.com/blog/post-to-linkedin-from-cursor) |
| [PostJay](https://postjay.com/mcp) | `https://postjay.com/api/mcp` | OAuth или API key | X, Instagram, TikTok, YouTube, LinkedIn, Facebook, Pinterest, Threads, Bluesky, Mastodon, Discord, Telegram, Tumblr, Reddit | Нет, vendor | Beta сейчас бесплатна | Планируется Starter $9/мес., 5 accounts; Creator $19, 15 | Сейчас и расписание | Очень молодой сервис; страница говорит «18 platforms», но перечисляет только 14. [Цены](https://postjay.com/pricing) |
| [PostMCP AI](https://www.postmcpai.com/docs/mcp) | `https://mcp.postmcpai.com/mcp?apikey=…` | API key в URL | LinkedIn, X, Facebook, Instagram, Threads, Bluesky, YouTube | Нет, credits | 5 profiles, 20 credits | Starter $20/мес., 2 000 credits; Pro $50 | Create, schedule, retry | X delivery стоит 5 credits, ссылочный пост ещё 50; ключ попадает в URL. [Цены](https://www.postmcpai.com/pricing), [FAQ](https://www.postmcpai.com/docs/faq) |
| [Pub Studio](https://pub.studio/) | Персональный URL выдаётся в аккаунте | OAuth/API credential | X, LinkedIn, Instagram, Threads | Нет, vendor | 7-дневный trial | $29/мес., 15 accounts; Pro $49 | Draft, schedule, publish | Всего четыре площадки; молодой сервис. [Цены](https://pub.studio/pricing) |
| [WritePat](https://writepat.com/help/developers/mcp) | `https://writepat.com/api/mcp` | API key query/Bearer | X, LinkedIn, Threads, Bluesky, Substack | Нет, vendor | 15 разовых AI credits; публикация требует trial/paid | Creator $20/мес., 1 000 posts; Pro $39, 1 500 | MCP создаёт и планирует drafts | Не полностью автономный zero-review путь. [Планы](https://writepat.com/help/billing-plans/creator-vs-pro) |
| [Eden](https://eden.so/blog/schedule-social-media-posts-from-claude) | `https://mcp.eden.so/mcp` | OAuth | X, LinkedIn, Threads, Instagram, TikTok, Facebook Pages, YouTube Shorts, Substack | Нет, vendor | Free account | Платные планы от $29/мес. | Немедленно и schedule | Нет Mastodon, Bluesky, Reddit, Telegram, Discord, Dev.to/Hashnode/Medium. [Цены](https://eden.so/pricing) |
| [Genna](https://www.genna.co/mcp/) | `https://mcp.genna.co/mcp` | OAuth 2.1 | Основной документированный набор: Instagram, TikTok, YouTube, Facebook, LinkedIn | X не подтверждён в основном наборе | Неясно | От R$39.90/мес. | Publish/schedule | Не следует выбирать как X-рельсу без отдельного письменного подтверждения X |
| [Schedule & Chill](https://schedulenchill.com/help/developers/mcp) | `https://schedulenchill.com/mcp-oauth` | OAuth; Bearer-вариант | Сейчас публично: LinkedIn, Bluesky, Mastodon, YouTube | X нет | 2 channels, 500 posts/мес. | Планы расширения заявлены, но не все сети live | `publish_now`, schedule, drafts | Не решает X; часть адаптеров ещё проходит review. [Текущий продукт](https://schedulenchill.com/) |
| [BulkPublish](https://www.bulkpublish.com/integrations/mcp-server/) | `https://mcp.bulkpublish.com/mcp`; local npm server | OAuth 2.1 либо API key | X, Meta, TikTok, YouTube, Threads, Bluesky, Pinterest, GMB, LinkedIn, Mastodon, Discord, Telegram, Tumblr, Snapchat | Нет, credits/vendor | 3 channels, 30 posts/мес., 10 scheduled | Pro $13.99/мес., 30 channels, 900 posts | Draft, schedule, publish, analytics | Молодой сервис; стоимость одного X-credit публично не раскрыта. [Тарифы](https://www.bulkpublish.com/) |
| [OpenTweet](https://opentweet.io/) | `https://mcp.opentweet.io/mcp` | Bearer API key | X; cross-post в Bluesky | Нет, vendor | Trial | Pro $11.99/мес. при annual billing | Публикация, schedule, threads | Один X-аккаунт и до 20 постов/день; узкое покрытие. [Тарифы](https://opentweet.io/pricing) |

C. Кто в действительности платит за X

Публичные страницы большинства сервисов не раскрывают договорную конструкцию с X. Поэтому утверждать, что у каждого есть именно enterprise-договор, нельзя.

Достоверно можно разделить сервисы по пользовательскому результату:

1. Vendor-managed, без собственного X developer app

Сюда относятся Buffer, Typefully, Hootsuite, SocialPilot, Post Bridge, PostSyncer, Nuelink, PostFast, Eden, Zoho Social, Ayrshare, Postiz Cloud и другие hosted-сервисы из таблицы. Пользователь подключает X-аккаунт по OAuth; ключи X в Claude или MCP не передаются.

Buffer дополнительно называет себя официальным API-партнёром X и других платформ. [Buffer API](https://buffer.com/api).

Это не означает «X для пользователя бесплатен»: стоимость API может быть уже включена в подписку, вынесена в add-on либо тарифицироваться за пост.

2. Vendor-managed с отдельной оплатой X

- Metricool: €10/$10 в месяц за каждый X-аккаунт. [Источник](https://help.metricool.com/your-guide-to-the-x-twitter-add-on-wt5wy).
- Vista Social: $29 в месяц за X-аккаунт. [Источник](https://vistasocial.com/pricing).
- Schedchie: €4.99 за пакет из 30 X-постов на 30 дней. [Источник](https://www.schedchie.com/help/x-twitter-add-on).
- Zernio и Bundle.social: $0.015 за обычную публикацию, $0.20 за публикацию со ссылкой. [Zernio](https://docs.zernio.com/pricing), [Bundle.social](https://bundle.social/).
- OneUp: X-ссылки требуют $50 add-on за 250 ссылочных постов. [Источник](https://help.oneupapp.io/en-us/article/link-posts-on-x-whao47).
- Upload-Post: $19 за максимум 50 X-постов со ссылкой; без add-on URL удаляется. [Источник](https://www.upload-post.com/platforms/x).
- Hermoso, PurrPlan, PostMCP AI и BulkPublish используют внутренние credits.

Для @Tony_Stef критически важно проверять именно тариф на ссылочный X-пост. Некоторые сервисы дешёвы для текста, но становятся существенно дороже при наличии URL.

3. BYOK: собственное приложение и биллинг X

- Mixpost: официальная инструкция прямо требует X Developer account, API key и secret, а также pay-as-you-go доступ. [Mixpost X setup](https://docs.mixpost.app/services/social/x/).
- Postiz self-hosted: `.env.example` содержит переменные credentials для X и других провайдеров. [Postiz environment example](https://github.com/gitroomhq/postiz-app/blob/main/.env.example).
- Outstand: собственные credentials обязательны для X, Reddit, Google Business и Vimeo. [Outstand facts](https://www.outstand.so/facts).

Следовательно, self-hosted Mixpost и Postiz не обходят текущую ошибку `403 client-not-enrolled`. Они лишь дают собственный сервер-планировщик поверх того же непройденного X developer access.

4. Нужны ли self-hosted-сервисам ключи для каждой площадки

Фраза «платные API-ключи для каждой площадки» неверна. Нужны provider credentials или авторизованное подключение, но способ и стоимость различаются:

- Mastodon: регистрируется OAuth application на конкретном instance; отдельной metered-оплаты API в документации нет. Источник ⚠ старше июня 2026 и может устареть: [Mastodon application API](https://docs.joinmastodon.org/methods/apps/).
- Bluesky: публикация выполняется после аутентификации пользователя; публичной платы за вызов API не описано. [Создание поста Bluesky](https://docs.bsky.app/docs/tutorials/creating-a-post).
- Telegram: нужен bot token от BotFather и права администратора канала; metered-платы за API не заявлено. [Telegram Bot API](https://core.telegram.org/bots/api).
- Discord: bot application либо incoming webhook; per-call тарификации в официальной документации нет. [Discord webhooks](https://docs.discord.com/developers/resources/webhook).
- Dev.to: используется Forem API key пользователя. [Forem API](https://developers.forem.com/api/v0).
- Reddit: это уже не гарантированно «бесплатная площадка». Eligible non-commercial clients могут получить free access с лимитом 100 queries/minute на OAuth client, но commercial use и отдельные категории требуют письменного одобрения или коммерческого договора. [Reddit Data API Wiki, 11 мая 2026](https://support.reddithelp.com/hc/en-us/articles/16160319875092-Reddit-Data-API-Wiki), [Responsible Builder Policy, 5 июня 2026](https://support.reddithelp.com/hc/en-us/articles/42728983564564-Responsible-Builder-Policy).
- Hashnode использует персональную/token-based авторизацию, но публично подтверждённого текущего тарифа API найти не удалось. Перед выбором self-hosted рельсы нужно проверить entitlement непосредственно в текущем Hashnode developer UI.

Meta, LinkedIn, TikTok, YouTube и Pinterest часто не берут деньги непосредственно за каждый API-вызов, но требуют developer app, verification/app review, redirect URIs и соблюдение quotas. Mixpost отдельно предупреждает о Meta и LinkedIn App Review. [Mixpost upgrade/app-review notes](https://docs.mixpost.app/enterprise/upgrading/upgrading-v7).

D. Черновик против реальной публикации

Немедленную публикацию через MCP явно документируют:

- Buffer: `shareNow`, а также queue/custom schedule. [MCP integration](https://developers.buffer.com/guides/integrations/mcp.html).
- Typefully: `publish_at: "now"`. [API v2 guide](https://support.typefully.com/en/articles/13133296-typefully-api-v1-v2-migration-guide).
- Postiz: draft, schedule либо `now`. [MCP tools](https://docs.postiz.com/mcp/tools).
- Mixpost, Zernio, Ayrshare, Hootsuite Perch, SocialPilot, Post Bridge, Nuelink, Hermoso, PostFast, Upload-Post, Eden, PostJay, BulkPublish и другие, отмеченные в таблице как publish.

Только черновик:

- Planable: посты, созданные Claude через MCP, попадают в Planable как drafts для review. [Источник](https://planable.io/blog/how-to-schedule-social-media-posts-with-mcp/).
- Agorapulse: publishing tools MCP создают drafts и не могут выполнять autopublish. [Источник](https://support.agorapulse.com/en/articles/12960496-model-context-protocool-mcp-integration-explained).

Условная автономность:

- PurrPlan по умолчанию безопасно создаёт draft, но scoped write-token может разрешить direct publishing.
- WritePat ориентирует MCP на создание и планирование drafts, хотя основной продукт умеет последующую автопубликацию.
- Schedchie документирует scheduling/edit/retry, но отдельный гарантированный `publish_now` в открытой документации не показан.
- Vista Social перечисляет создание и изменение draft/scheduled posts; отдельный immediate tool следует проверить в реальной MCP schema после подключения.

Ответ по Typefully однозначен: его MCP не draft-only. Он способен немедленно публиковать и планировать автоматическую публикацию. [Typefully AI agents](https://typefully.com/ai-agents).

E. Лимиты, треды, ссылки и dev-площадки

1. Наиболее значимые бесплатные лимиты

- Buffer: 3 канала и 10 scheduled posts одновременно на каждый канал. После публикации место освобождается, то есть это размер очереди, а не месячный потолок. [Buffer limits](https://support.buffer.com/en-us/articles/how-many-posts-can-i-schedule-in-advance-Kmy2IEecqm).
- Metricool: 20 posts/month, но X и LinkedIn исключены из Free. [Тарифы](https://metricool.com/pricing/).
- SocialRobot: 2 accounts и 15 posts/month, X исключён. [Источник](https://socialrobot.io/).
- Publora: 3 accounts, 15 posts/month, X исключён. [Источник](https://publora.com/pricing).
- Bundle.social: 20 posts/month и 3 accounts; X до 5 posts/day. [Источник](https://bundle.social/).
- Upload-Post: 10 uploads/month и 2 profiles. [Источник](https://www.upload-post.com/).
- BulkPublish: 3 channels, 30 posts/month, 10 scheduled. [Источник](https://www.bulkpublish.com/).
- Schedule & Chill: 2 channels и 500 posts/month, но X отсутствует. [Источник](https://schedulenchill.com/).
- PostJay beta пока бесплатна, но это временная коммерческая стадия, а не гарантированный постоянный free plan. [Тарифы](https://postjay.com/pricing).

2. Треды и первый комментарий

- Buffer поддерживает X-треды до 25 частей; на Free одновременно можно держать один запланированный тред. [Buffer X guide](https://support.buffer.com/en-us/articles/using-xtwitter-with-buffer-nA84XnWtuU).
- Typefully является одним из наиболее развитых вариантов для X-тредов и их кросспостинга. [X limitations](https://support.typefully.com/en/articles/8717699-twitter-x-posting-limitations).
- SocialPilot документирует треды и first comments на поддерживаемых планах. [Планы](https://www.socialpilot.co/plans).
- PostSyncer заявляет threads и first comment. [Источник](https://postsyncer.com/comment-management).
- Buffer предоставляет first comment как платную функцию. [Тарифы](https://buffer.com/pricing).

Поддержка first comment всегда зависит от конкретной площадки. Нельзя считать наличие кнопки в веб-интерфейсе доказательством, что тот же параметр уже доступен через MCP.

3. Покрытие dev-среды

| Сервис | Mastodon | Bluesky | Reddit | Dev.to | Hashnode | Telegram | Discord | Medium |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Postiz | Да | Да | Да | Да | Да | Да | Да | Да |
| Typefully | Да | Да | Нет | Нет | Нет | Нет | Нет | Нет |
| Buffer | Да | Да | Нет | Нет | Нет | Нет | Нет | Нет |
| Zernio | Нет | Да | Да | Нет | Нет | Да | Да | Нет |
| Bundle.social | Да | Да | Да | Нет | Нет | Нет | Да | Нет |
| Ayrshare | Нет | Да | Да | Нет | Нет | Да | Нет | Нет |
| PostSyncer | Да | Да | Нет | Нет | Нет | Да | Нет | Нет |
| Upload-Post | Нет | Да | Да, но API сейчас 503 | Нет | Нет | Да | Да | Нет |
| Publora | Да | Да | Нет | Нет | Нет | Да | Нет | Нет |
| PostJay | Да | Да | Да | Нет | Нет | Да | Да | Нет |
| BulkPublish | Да | Да | Нет | Нет | Нет | Да | Да | Нет |
| Zoho Social | Да | Да | Нет | Нет | Нет | Telegram Business на старших планах | Нет | Нет |

Postiz является единственным найденным официальным MCP-планировщиком, который документирует все восемь запрошенных dev-площадок в одной системе. [Полный список integrations](https://docs.postiz.com/public-api/integrations/list), [Create post API](https://docs.postiz.com/public-api/posts/create).

Но один и тот же текст нельзя слепо размножать на все восемь сетей: Reddit, Dev.to, Hashnode и Medium имеют разные форматы, правила самопромо и ожидания аудитории. Postiz решает транспорт, а не адаптацию контента.

F. Риск блокировок и отказов

1. Официальный API против браузерной автоматизации

X прямо запрещает scripting сайта, browser automation, scraping и другие неофициальные способы автоматизации; нарушение может привести к приостановке API-доступа либо аккаунта. [X Developer Guidelines](https://docs.x.com/developer-guidelines).

Использование официального API снижает риск, связанный с методом публикации, но не освобождает от правил поведения. Владелец аккаунта остаётся ответственным за spam, одинаковые посты, агрессивные ответы и автоматизированные действия. [X Automation Rules, апрель 2026](https://help.x.com/en/rules-and-policies/x-automation), [Restricted use cases](https://docs.x.com/developer-terms/restricted-use-cases).

Поэтому:

- Buffer, Typefully, Hootsuite, Postiz Cloud и другие OAuth/API-сервисы имеют меньший технический риск, чем Selenium/Playwright/расширение, нажимающее кнопки сайта.
- PurrPlan использует официальный API для Facebook Pages, но browser extension для Groups и personal profiles. Последний режим следует считать более рискованным и отдельным от официальной API-публикации. [Описание PurrPlan](https://purrplan.ai/en/reseau/facebook/).
- Наличие OAuth не даёт иммунитета от блокировки: автоматизированный spam через официальный API остаётся spam.

2. Были ли массовые блокировки в 2025–2026

В проверенных официальных политиках, status histories и vendor incident reports не найдено подтверждённых массовых блокировок X/Meta-аккаунтов только за сам факт публикации через Buffer, Hootsuite, Typefully или другой официальный API-планировщик.

Зафиксированы другие реальные сбои:

- В июле 2026 Buffer сообщал о неудачной публикации Meta video posts и необходимости повторить их после исправления. [Инцидент](https://status.buffer.com/incidents/01KXWT08G978T30H7RGQ4AMQQ5).
- В июне 2026 у Buffer были сбои Meta API. [Инцидент](https://status.buffer.com/incidents/01KTY2MNCT2YFP266VMZPMBXCS).
- В апреле 2026 сбой Bluesky привёл примерно к 2 000 failed posts у 780 Buffer accounts. [Инцидент](https://status.buffer.com/incidents/01KPARTSS96KC9XTXY04TFFFM3).

То есть основной доказанный операционный риск официальных планировщиков — failed/delayed posts из-за upstream API, OAuth expiry или исчерпания credits, а не автоматический бан аккаунта.

3. Что будет при закрытии сервиса или потере API

Нужно различать уже опубликованный и ещё ожидающий пост:

- Уже опубликованный пост находится на площадке и обычно остаётся там. Например, Buffer прямо пишет, что удаление Buffer-аккаунта не удаляет уже опубликованные social posts. [Buffer Terms, 5 августа 2026](https://buffer.com/legal).
- Drafts, очереди, расписание и media assets находятся у планировщика. Если он закрывается, теряет X-доступ, исчерпывает credits или пользователь отзывает OAuth, будущая публикация не происходит.
- OpenTweet прямо предупреждает, что после отмены подписки scheduled posts больше не публикуются. [OpenTweet pricing FAQ](https://opentweet.io/pricing).
- Отзыв platform connection до момента отправки также делает scheduled post неисполнимым; Hermoso описывает такой пост как refused/failed. [Hermoso scheduler](https://hermoso.ai/social-media-scheduler/).

Практическая страховка: оригинальный текст и planned timestamp должны оставаться в собственной базе, а агент после scheduled time должен проверять реальный URL опубликованного поста. Ответ MCP `scheduled: true` ещё не доказывает публикацию.

G. Одновременное использование нескольких сервисов

Подключить один X-аккаунт к нескольким OAuth-приложениям технически можно. X показывает каждое подключённое приложение отдельно и позволяет отзывать доступ по одному. [X connected apps](https://help.x.com/en/managing-your-account/connect-or-revoke-access-to-third-party-apps).

Основные проблемы возникают не из-за OAuth, а из-за оркестрации:

- Несколько сервисов могут отправить один и тот же пост.
- Retry одного сервиса может пересечься с ручным повтором другого.
- Лимиты площадки относятся к аккаунту и активности в целом, а не дают независимую квоту каждому планировщику. [X limits](https://help.x.com/en/rules-and-policies/x-limits).
- Дублированный либо почти одинаковый контент может нарушить X automation rules.
- Отзыв доступа одному приложению обычно не отключает остальные, но смена пароля или security action площадки может инвалидировать несколько tokens сразу.

Безопасная схема:

- Один «писатель» на каждую площадку.
- Buffer владеет X, LinkedIn, Bluesky и Mastodon.
- Postiz владеет Reddit, Dev.to, Hashnode, Telegram, Discord и Medium.
- Центральный агент хранит `content_id`, целевые площадки, время и фактический URL результата.
- Перед retry агент проверяет, не существует ли уже публикация с этим `content_id`.
- На пересечении площадок, например Bluesky, право публикации получает только один сервис.

Таким образом, конфликтов OAuth обычно нет; дубли появляются из-за двух активных очередей, а не из-за двух авторизаций.

H. Три рекомендации

1. Дешевле всего: Buffer Free

Почему:

- $0.
- X доступен без собственного developer-приложения.
- Официальный MCP умеет `shareNow`, очередь, расписание и draft.
- Трёх каналов достаточно для схемы X + Bluesky + Mastodon либо X + LinkedIn + Bluesky.
- 10 scheduled posts на канал достаточно при ежедневном пополнении очереди для ритма 1–3 поста в день.
- Buffer публично называет себя официальным API-партнёром X. [MCP](https://developers.buffer.com/guides/integrations/mcp.html), [API partnership](https://buffer.com/api), [Free plan](https://buffer.com/pricing).

Главный минус: нет Reddit, Telegram, Discord, Dev.to, Hashnode и Medium. Если потребуется больше трёх каналов, Essentials оплачивается за каждый канал.

Для автора со ссылками Buffer выглядит дешевле Zernio, Bundle.social, OneUp и Upload-Post, потому что на опубликованной странице Buffer нет отдельной платы именно за ссылочный X-пост.

2. Шире всего по площадкам: Postiz Cloud Team

Почему:

- Единственный найденный MCP-сервис, закрывающий Mastodon, Bluesky, Reddit, Dev.to, Hashnode, Telegram, Discord и Medium одновременно.
- Есть немедленная публикация, drafts и scheduling.
- Cloud снимает проблему собственных X credentials.
- Team стоит $39/мес. за 10 channels и unlimited posts; Standard $29 покрывает только 5 channels и 400 posts/month. [Тарифы](https://postiz.com/pricing), [интеграции](https://docs.postiz.com/public-api/integrations/list).

Главный минус: продукт моложе Buffer/Hootsuite, а 10 каналов почти полностью расходуются на требуемый набор. Для всех площадок плюс X, LinkedIn и дополнительные аккаунты может понадобиться Pro.

Не выбирать self-hosted Postiz ради обхода X API: он требует собственные provider credentials и вернёт задачу к той же проблеме X enrollment.

3. Надёжнее всего: Hootsuite Perch, но рациональный выбор для одного автора всё равно Buffer

Hootsuite имеет зрелую платформу, отдельный официальный publishing MCP Perch, OAuth, немедленную публикацию и 10 social accounts на Standard. [MCP](https://www.hootsuite.com/integrations/mcp), [планы](https://www.hootsuite.com/plans), [MCP Terms от 24 июня 2026](https://www.hootsuite.com/legal/mcp-server-terms).

Но $99 за пользователя в месяц при годовой оплате плохо соответствует масштабу одного автора. Кроме того, Hootsuite почти не закрывает dev-native площадки.

Поэтому:

- «Надёжнее независимо от цены» — Hootsuite Perch.
- «Наиболее надёжный и разумный для @Tony_Stef» — Buffer.
- Typefully лучше Buffer, если центр стратегии — сложные X-треды и ручная редакционная работа, но его текущая публичная цена и free-limit менее прозрачны.

I. Итоговый выбор

Рекомендованная последовательность:

1. Подключить Buffer Free к X через OAuth.
2. Выполнить тестовый немедленный пост с безопасной ссылкой.
3. Проверить, что MCP вернул не только успешный запрос, но и реальный URL поста.
4. Проверить scheduled post и thread.
5. Добавить Bluesky и Mastodon либо LinkedIn в оставшиеся два бесплатных канала.
6. Не подключать Postiz немедленно: добавить Postiz Cloud Team только после решения действительно публиковать минимум в несколько из Reddit, Dev.to, Hashnode, Telegram, Discord и Medium.
7. Если Postiz добавлен, назначить одному сервису единоличное владение каждой площадкой.

Итоговая экономика:

- Минимальный рабочий вариант: Buffer, $0.
- Прозрачная metered-альтернатива при 30–90 ссылочных X-постах: Zernio примерно $6–18/мес. только за X link delivery, плюс возможная оплата дополнительных аккаунтов.
- Полная dev-сетка: Postiz Cloud Team, $39/мес.
- Enterprise-надежность: Hootsuite, от $99/мес., но для данного масштаба это переплата.

🧒 Простыми словами: свой сломанный ключ X чинить необязательно. Buffer уже держит рабочую дверь в X: Claude отдаёт ему пост, а Buffer публикует. Если позже понадобится весь «город разработчиков», добавляется Postiz, но каждая площадка закрепляется только за одним отправителем.
