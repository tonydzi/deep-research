# DR26-08-05-HUB-02-2156 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt.com (ChatGPT Pro + Web search)
dr_id: DR26-08-05-HUB-02-2156
topic: "FB Graph API: легальный способ читать СВОИ посты с ЛИЧНОГО профиля Facebook без автоматизации браузера"
date: 2026-08-05

chat_url: https://приватный чат


proof_of_work: "Worked for 53m 28s; 43 источника официальной документации Meta с датами проверки; 30+ открытых сайтов"
verbatim: true
---

> Снимок ответа verbatim, эхо промпта отрезано по якорю «Worked for».

Worked for 53m 28s
Есть официальный путь — с оговорками

[established] В актуальном Graph API v26.0 разрешение user_posts существует, endpoint GET /me/posts документирован для личного профиля владельца token, а поля id, message, created_time и permalink_url доступны через объект Post. Для приложения, которым пользуется только человек с ролью администратора/разработчика/тестировщика этого приложения, достаточно Standard Access в Development Mode — без App Review. Ежедневная работа может быть полностью browserless. Первичная OAuth-авторизация и, консервативно, периодическая повторная авторизация требуют человеческого входа в Facebook. (Meta Graph API Versions, 29.07.2026
; User Posts, без даты, проверено 05.08.2026, v26.0
; Access Levels, без даты, проверено 05.08.2026
; Graph API Explorer, без даты, проверено 05.08.2026
). 
Facebook Developers
+3
Facebook Developers
+3
Facebook Developers
+3

Но есть две существенные оговорки:

[emerging] Policy fit вашего exact use case не безупречен. Текущий Allowed Usage для user_posts сформулирован как создание физических/цифровых книг или альбомов таймлайна и перенос «memories» в Facebook или другие social apps. Ежедневное чтение свежих постов для подготовки Telegram-анонсов технически близко к переносу собственного контента, но не названо прямо. Я не нашёл отдельного исключения для role-only Development Mode. (Permissions Reference, без даты, проверено 05.08.2026
; Graph API v14 changelog, 25.05.2022
). 
Facebook Developers
+1

[established] «Вечный» unattended User token не документирован. Long-lived token обычно действует около 60 дней; отдельно существует примерно 90-дневное окно доступа приложения к данным. SDK умеют продлевать token при пользовательской активности, но официальный generic OAuth refresh_token grant для server-only Facebook User token не описан. Поэтому production-план должен предполагать ручной OAuth re-login не реже чем до истечения 60 дней, пока live-эксперимент не докажет rolling refresh именно для вашего режима. (Access Tokens, без даты, проверено 05.08.2026
; Long-Lived Tokens, без даты, проверено 05.08.2026
; Authentication versus Data Access, без даты, проверено 05.08.2026
). 
Facebook Developers
+2
Facebook Developers
+2

Матрица вариантов
Путь	Работает в 2026	Что сделать один раз	Что делать регулярно	Срок/latency	Риск аккаунта	Пригодность
Graph API /me/posts, self-admin app, Standard Access	Да. Победитель. user_posts жив в v26.0. (User Posts
, б/д, проверено 05.08.2026). 
Facebook Developers
	Создать app, добавить Facebook Login и user_posts, выдать token самому себе	Polling; token monitoring; возможно OAuth re-login ≤60 дней	Почти real-time; token ≈60 дней nominal	Низкий technical/account risk, но medium policy ambiguity по Allowed Usage	Отлично
Graph API с Advanced Access/App Review	Да, но для вашей схемы не нужен	Business Verification, App Review, демонстрация use case	Compliance/review maintenance	Долгий организационный путь	Низкий после approval	Нужен только для не-role пользователей
User feed Webhook + API readback	Документирован user.feed, но current delivery личных постов не подтверждена	Callback, verification, subscription	Поддерживать endpoint и token	Потенциально real-time	Низкий при официальной конфигурации	Эксперимент, не primary
Download/Export Your Information → external service	Да; UI предлагает выбрать периодичность export. [single-source] (Facebook Help, б/д, проверено 05.08.2026
). 
Facebook
	Выбрать Posts, JSON, destination и frequency	Возможно подтверждать доступ к destination	Batch; SLA не документирован	Низкий	Backup; id/permalink не гарантированы
Data Portability destination integration	Да; Facebook Posts поддерживаются	Пройти Meta onboarding как destination и реализовать destination API	Поддерживать OAuth/API destination	Batch transfer	Низкий	Слишком тяжело для одного пользователя
Meta Content Library API	Да, но это research product для публичного архива	Получить institutional research approval	Работать в secure environment	Near-real-time public archive	Низкий	Практически непригодно
Threads как source of truth	Да; API отдаёт id, text, timestamp, permalink	Публиковать/кросс-постить весь текст в Threads	Refresh Threads token	Near-real-time; token можно явно refresh	Низкий	Хороший shadow channel, но не Facebook identity
Instagram Professional mirror	Частично	Professional account и media API	Refresh token	Media-centric	Низкий	Не подходит для text-only FB posts
Professional Dashboard DOM automation	По вашему замеру — да	Постоянная browser session	Repair selectors/scroll/rendering	Near-real-time	Средний contractual/account risk	Только fallback
Replay внутреннего /api/graphql/ с doc_id	Технически возможно, официально не поддерживается	Reverse engineering session requests	Обновлять doc_id, variables, cookies, CSRF	Near-real-time	Средний–высокий	Не считать durable API
RSS/Atom/e-mail structured feed	Текущего официального пути для personal profile не найдено	—	—	—	—	Нет [no source found]
1. Что именно осталось от user_posts
1.1. Permission не закрыт

[established] Graph API v26.0 выпущен 29 июля 2026 года. Его актуальная User Posts reference по-прежнему описывает коллекцию Posts пользователя и требует User Access Token с permission user_posts. То есть распространённая формулировка «Facebook API больше не читает personal profiles» неверна применительно к самому владельцу token. Она верна для произвольного чтения чужих профилей. (Versions, 29.07.2026
; User Posts, б/д, проверено 05.08.2026
). 
Facebook Developers
+2
Facebook Developers
+2

1.2. Что произошло после Cambridge Analytica

После 2018 года Meta:

потребовала App Review для расширенных user permissions, включая user_posts, когда приложение обслуживает обычных non-role users;

перестала возвращать данные других пользователей через posts/photos/albums и связанные объекты;

отдельно закрыла publish_actions, то есть программную публикацию в личный профиль.

Это не было закрытием чтения своих posts. Закрытие publish_actions часто ошибочно переносят на user_posts. (Facebook Login Changelog, запись 02.07.2018, deadline 01.08.2018
; Facebook Login privacy update, 01.05.2018
; Publish Permission update, 31.07.2018
). 
Facebook Developers
+2
Facebook Developers
+2

1.3. Что было урезано позже

[established] В 2021–2022 годах Meta сузила Allowed Usage user_posts. Текущая формулировка разрешает:

создавать physical/digital books or albums таймлайна;

делиться memories из таймлайна в Facebook или других social apps.

Приложения, которые ранее получили approval, но перестали соответствовать Allowed Usage, могли потерять permission. Сам permission при этом сохранён. (v11.0 changelog, изменение вступало в силу 07.09.2021
; v14.0 changelog, 25.05.2022
; Permissions Reference, б/д, проверено 05.08.2026
). 
Facebook Developers
+2
Facebook Developers
+2

Вывод по вашему workflow

[emerging] Private archival processing собственных posts хорошо согласуется с духом permission. Автоматическое создание Telegram-анонса из свежего post не является очевидным злоупотреблением, но слово «fresh posts → derived Telegram announcement» в Allowed Usage отсутствует. Это policy ambiguity, а не технический blocker.

Эксперимент не разрешит юридическую неоднозначность; её мог бы окончательно снять только письменный ответ Meta или успешный App Review с честно описанным use case. Для role-only app review не требуется, поэтому получить такой формальный ответ через review практически неоткуда.

2. Endpoint и возвращаемые данные
2.1. Победивший endpoint: /me/posts

GET /v26.0/me/posts — правильный endpoint. User Posts может включать Posts на таймлайне app user и Posts, в которых пользователь tagged, поэтому для строгого требования «только мои публикации» стоит запросить from{id} и оставить записи, где from.id == me.id. (User Posts, б/д, проверено 05.08.2026
). 
Facebook Developers
+1

GET /me/feed тоже остаётся документированным, но его semantics шире: posts и links, опубликованные этим человеком или другими людьми на его profile. Для вашей задачи это худший источник. (User Feed, б/д, проверено 05.08.2026, v26.0
). 
Facebook Developers

2.2. Нужные поля
Требование	Graph API field	Статус
Stable identifier	id	Canonical Graph object ID
Полный текущий текст	message	Документированное содержимое Post
Точное время публикации	created_time	Время публикации Post
Permanent link	permalink_url	Документирован как permanent static URL
Проверка авторства	from{id}	Автор/публикующий profile

created_time и permalink_url прямо описаны в актуальной Post reference. Практический independent report от 7 мая 2026 года подтверждает рабочий запрос /me/posts?fields=id,message,created_time,permalink_url и хранение id как primary key; это полезная современная проверка, но не равно официальной гарантии. [low-authority] (Post reference, б/д, проверено 05.08.2026, v26.0
; EY-Office implementation report, 07.05.2026
). 
Facebook Developers
+1

Ограничения, которые Meta явно не гарантирует

Используйте id как opaque primary key в рамках этого app. Не разбирайте его формат и не рассчитывайте на равенство IDs между двумя разными Meta apps. Формальной гарантии cross-app immutability я не нашёл. [no source found]

message — текущий текст, а не edit history.

Для photo-only/link-only posts message может отсутствовать.

Я не нашёл в документации формальной гарантии, что очень длинный message никогда не truncates. [no source found]

Удалённый post естественным образом перестанет читаться.

Лучше сохранять одновременно id, permalink_url и raw JSON response.

2.3. Рабочий запрос
Bash
curl -G "https://graph.facebook.com/v26.0/me/posts" \
  --data-urlencode "fields=id,message,created_time,permalink_url,from{id}" \
  --data-urlencode "since=1785888000" \
  --data-urlencode "until=1785974400" \
  --data-urlencode "limit=100" \
  --data-urlencode "access_token=$FB_USER_TOKEN"

Graph API документирует time-based pagination с Unix timestamps. Следовать надо по paging.next, а не самостоятельно собирать следующий cursor. (Paginated Results, б/д, проверено 05.08.2026
). 
Facebook Developers

Для production:

Запрашивать окно 26 часов, затем локально отрезать строго последние 24 часа.

Всегда проходить paging.next, даже если ожидается только 10–15 posts.

Дедуплицировать по id.

Не публиковать запись автоматически, если from.id отсутствует или не совпадает с /me.id.

Хранить UTC, исходный created_time, fetched_at и hash raw JSON.

Делать polling одним fleet-node, а не размножать User token по всем машинам.

Рекомендуемая запись:

JSON
{
  "facebook_app_id": "123456789",
  "post_id": "opaque_graph_post_id",
  "author_id": "app_scoped_user_id",
  "message": "Полный текущий текст",
  "created_time": "2026-08-05T20:42:17+0000",
  "permalink_url": "https://www.facebook.com/...",
  "fetched_at": "2026-08-05T21:10:00Z",
  "raw_sha256": "..."
}
3. Почему одному self-admin не нужен App Review
3.1. Standard Access

[established] Meta определяет Standard Access как режим для приложений:

используемых только людьми с app roles;

либо находящихся в разработке и тестирующих endpoints.

Graph API Explorer дополнительно говорит прямо: если app находится in development, можно grant permission, и запросы будут учитывать её для данных людей, имеющих роль в app. (Access Levels, б/д, проверено 05.08.2026
; Graph API Explorer, б/д, проверено 05.08.2026
). 
Facebook Developers
+1

Следовательно, схема:

Facebook profile owner
        │
        ├── App role: Administrator
        │
        ├── App mode: Development
        │
        └── Granted permission: user_posts

официально не требует App Review.

3.2. Когда review понадобится

Advanced Access нужен, если permission должно использоваться людьми, не имеющими роли в app. Для Advanced Access требуется Business Verification и, в зависимости от permission, App Review. Consumer apps в Live mode не могут раздавать Standard Access permissions обычным non-role users. (Access Levels, б/д, проверено 05.08.2026
; Publish, б/д, проверено 05.08.2026
; Business Verification for Advanced Access, 01.02.2023
). 
Facebook Developers
+2
Facebook Developers
+2

Для вашего случая:

не включать Live mode;

не добавлять внешних пользователей;

не делать consumer product;

не подавать App Review без необходимости.

4. Пошаговая настройка победившего варианта

Названия экранов могут слегка отличаться по locale и rollout. Текущая официальная схема app creation использует use case “Authenticate and request data from users with Facebook Login”; свежий практический отчёт от 07.05.2026 подтверждает, что user_posts добавляется через Use cases → Customize. Последнее — [low-authority] [single-source]. (Create an App, б/д, проверено 05.08.2026
; Customize Facebook Login use case, б/д
; EY-Office, 07.05.2026
). 
Facebook Developers
+2
Facebook Developers
+2

Шаг 1. Создать app

Открыть Meta for Developers → My Apps → Create App.

Выбрать use case Authenticate and request data from users with Facebook Login.

Задать app name и contact email.

Оставить app в состоянии In development.

В App roles проверить, что ваш Facebook account имеет роль Administrator.

Шаг 2. Добавить permission

Открыть Use cases.

У use case Facebook Login выбрать Customize.

Найти user_posts.

Добавить его к app.

В некоторых вариантах dashboard тот же control находится в App Review → Permissions and Features.

Шаг 3. Smoke test через Graph API Explorer

Открыть Graph API Explorer
.

В dropdown Meta App выбрать созданное app.

В User or Page выбрать Get User Access Token.

Добавить permission user_posts.

Подтвердить authorization под тем же Facebook account.

Explorer официально предназначен для apps, где пользователь является administrator/developer/tester, и позволяет получать User tokens для выбранного app. (Graph API Explorer, б/д, проверено 05.08.2026
). 
Facebook Developers
+1

Выполнить последовательно:

http
GET /v26.0/me?fields=id,name
http
GET /v26.0/me/permissions

В ответе должно быть:

JSON
{
  "permission": "user_posts",
  "status": "granted"
}

Затем:

http
GET /v26.0/me/posts
  ?fields=id,message,created_time,permalink_url,from{id}
  &limit=10
Go/no-go критерий

Официальный путь подтверждён для вашего конкретного account/app, если:

возвращаются известные posts личного profile;

message совпадает с Facebook;

created_time совпадает с UI;

permalink_url открывает тот же post;

id повторяется неизменным при повторном запросе;

posts не требуют Professional Dashboard.

Шаг 4. Настроить production OAuth

Explorer годится для smoke test, но production token лучше получать собственным OAuth callback.

В Facebook Login → Settings добавить HTTPS callback в Valid OAuth Redirect URIs, например:

https://hub.example.com/oauth/facebook/callback

Authorization URL:

https://www.facebook.com/v26.0/dialog/oauth
  ?client_id=APP_ID
  &redirect_uri=URL_ENCODED_CALLBACK
  &state=CRYPTO_RANDOM_STATE
  &scope=user_posts
  &response_type=code

После callback обменять code на short-lived token server-side:

Bash
curl -G "https://graph.facebook.com/v26.0/oauth/access_token" \
  --data-urlencode "client_id=$FB_APP_ID" \
  --data-urlencode "client_secret=$FB_APP_SECRET" \
  --data-urlencode "redirect_uri=https://hub.example.com/oauth/facebook/callback" \
  --data-urlencode "code=$OAUTH_CODE"

Manual Login Flow официально использует authorization code и последующий обмен через OAuth endpoint. (Manual Login Flow, б/д, проверено 05.08.2026
). 
Facebook Developers

Шаг 5. Обменять short-lived token на long-lived
Bash
curl -G "https://graph.facebook.com/v26.0/oauth/access_token" \
  --data-urlencode "grant_type=fb_exchange_token" \
  --data-urlencode "client_id=$FB_APP_ID" \
  --data-urlencode "client_secret=$FB_APP_SECRET" \
  --data-urlencode "fb_exchange_token=$SHORT_LIVED_TOKEN"

Это официальный fb_exchange_token flow. (Long-Lived Tokens, б/д, проверено 05.08.2026
). 
Facebook Developers

Token:

не класть в Git;

не писать в Telegram;

не синхронизировать через общий Syncthing vault;

не передавать пяти fleet nodes;

хранить только у single-writer poller в OS secret store;

App Secret держать отдельно от User token.

Шаг 6. Production poller

Минимальный Python skeleton:

Python
Run
from __future__ import annotations

import os
from datetime import datetime, timedelta, timezone
from typing import Any

import requests
from dateutil.parser import isoparse

API_VERSION = "v26.0"
GRAPH_BASE = f"https://graph.facebook.com/{API_VERSION}"
TOKEN = os.environ["FB_USER_TOKEN"]


class FacebookAPIError(RuntimeError):
    pass


def graph_get(path_or_url: str, params: dict[str, Any] | None = None) -> dict[str, Any]:
    url = (
        path_or_url
        if path_or_url.startswith("https://")
        else f"{GRAPH_BASE}/{path_or_url.lstrip('/')}"
    )

    request_params = dict(params or {})
    if not path_or_url.startswith("https://"):
        request_params["access_token"] = TOKEN

    response = requests.get(url, params=request_params, timeout=30)

    try:
        payload = response.json()
    except ValueError as exc:
        raise FacebookAPIError(
            f"Facebook returned non-JSON HTTP {response.status_code}"
        ) from exc

    if not response.ok or "error" in payload:
        error = payload.get("error", {})
        raise FacebookAPIError(
            "Facebook API failure: "
            f"http={response.status_code}, "
            f"code={error.get('code')}, "
            f"subcode={error.get('error_subcode')}, "
            f"message={error.get('message')}"
        )

    return payload


def fetch_owned_posts(hours: int = 24) -> list[dict[str, Any]]:
    now = datetime.now(timezone.utc)
    exact_since = now - timedelta(hours=hours)

    # Overlap protects against scheduler delay and eventual ordering effects.
    query_since = exact_since - timedelta(hours=2)

    me = graph_get("me", {"fields": "id"})
    me_id = str(me["id"])

    path_or_url = "me/posts"
    params: dict[str, Any] | None = {
        "fields": "id,message,created_time,permalink_url,from{id}",
        "since": int(query_since.timestamp()),
        "until": int(now.timestamp()),
        "limit": 100,
    }

    output: dict[str, dict[str, Any]] = {}

    for _ in range(20):  # Pagination guard.
        page = graph_get(path_or_url, params)

        for post in page.get("data", []):
            created = isoparse(post["created_time"]).astimezone(timezone.utc)
            author_id = str((post.get("from") or {}).get("id") or "")

            if not (exact_since <= created <= now):
                continue

            # Fail closed: tagged/foreign posts must not enter Telegram pipeline.
            if author_id != me_id:
                continue

            post_id = str(post["id"])
            output[post_id] = {
                "id": post_id,
                "message": post.get("message"),
                "created_time": post["created_time"],
                "permalink_url": post.get("permalink_url"),
                "from": post.get("from"),
                "fetched_at": now.isoformat(),
            }

        next_url = (page.get("paging") or {}).get("next")
        if not next_url:
            break

        path_or_url = next_url
        params = None
    else:
        raise FacebookAPIError("Pagination guard reached")

    return sorted(output.values(), key=lambda item: item["created_time"])

Dependencies:

Bash
pip install requests python-dateutil
5. Token lifecycle: где именно придётся ходить руками
5.1. Два отдельных expiration clock
Clock	Что означает	Typical value
expires_at	Срок действия самого User Access Token	около 60 дней для long-lived
data_access_expires_at	До какого момента app разрешён доступ к user data	около 90 дней с последней user activity

Short-lived tokens обычно живут 1–2 часа, long-lived — около 60 дней, причём Meta отдельно предупреждает не строить систему на предположении, что эти durations никогда не изменятся. Data access expiration составляет 90 дней, исходя из последней активности пользователя. (Access Tokens, б/д, проверено 05.08.2026
; Authentication versus Data Access, б/д
). 
Facebook Developers
+1

Проверка:

http
GET /debug_token
  ?input_token=FB_USER_TOKEN
  &access_token=APP_ACCESS_TOKEN

Ответ содержит, среди прочего:

JSON
{
  "data": {
    "is_valid": true,
    "expires_at": 1790000000,
    "data_access_expires_at": 1792500000,
    "scopes": ["user_posts"],
    "user_id": "..."
  }
}

Поля is_valid и data_access_expires_at документированы Meta. (Debug Token, б/д, проверено 05.08.2026
). 
Facebook Developers

5.2. Можно ли продлевать полностью неинтерактивно

Документированное поведение:

long-lived token примерно 60 дней;

Meta пишет, что такие tokens refresh один раз в день, когда person using the app делает request к Facebook servers;

iOS, Android и JavaScript SDK автоматически refresh token, если человек использовал app за последние 90 дней;

expired token нельзя обменять на новый long-lived: после expiration нужен login flow. (Long-Lived Tokens, б/д, проверено 05.08.2026
). 
Facebook Developers
+2
Facebook Developers
+2

Что не документировано:

считается ли nightly server cron «person using the app»;

двигает ли /me/posts с backend expires_at;

существует ли generic grant_type=refresh_token для Facebook User token.

Для последнего официальный источник не найден: [no source found].

Операционный вывод

[established] Не закладывать indefinite refresh. Построить систему так, будто re-login потребуется до 60-го дня.

[speculative] Возможно, фактическая активность будет rolling-refresh token. Это надо измерить, а не предполагать.

5.3. Эксперимент на auto-renewal

Сразу после получения long-lived token:

Сохранить значения:

issued_at;

expires_at;

data_access_expires_at.

Запускать только backend /me/posts; не открывать приложение и не использовать JS SDK.

Повторить /debug_token через 24, 48, 72 часа и через неделю.

Сравнить timestamps.

Интерпретация:

expires_at сдвигается вперёд → cron activity, вероятно, поддерживает rolling token для этого app/account;

expires_at не меняется → планировать human OAuth;

двигается только data_access_expires_at → token и data authorization имеют разные renewal conditions;

token внезапно invalid → trigger reauthorization.

5.4. Что делать, когда token протух

Poller получает auth error и переводит rail в TOKEN_EXPIRED, а не в browser-scraping fallback.

/debug_token подтверждает is_valid=false или близкий expires_at.

Оператор открывает internal URL вида:

https://hub.example.com/reconnect/facebook

Проходит Facebook OAuth dialog.

Backend получает short-lived token.

Backend сразу обменивает его на long-lived.

Новый token атомарно записывается в secret store.

Выполняются:

/me;

/me/permissions;

/me/posts?limit=1.

Только после успешного smoke test poller возвращается в RUNNING.

Meta рекомендует тестировать login flow в том числе после смены Facebook password и выхода с других devices; такие security events могут потребовать нового login. (Testing a Login Flow, б/д, проверено 05.08.2026
). 
Facebook Developers

Практический alerting:

T-14 days → warning
T-7 days  → operator task
T-3 days  → Telegram red alert
T-0       → stop publishing derived announcements
6. Официальные альтернативы
6.1. Download/Export Your Information

[established] [single-source] Facebook Help сейчас позволяет:

экспортировать выбранные категории information;

выбирать date range;

получать HTML или machine-readable JSON;

экспортировать во внешний сервис;

выбрать, как часто выполнять export.

(Export a copy of your Facebook information, без даты, проверено 05.08.2026
). 
Facebook
+2
Facebook
+2

Однако публичная документация не отвечает на критические вопросы:

какие exact frequency доступны — daily/weekly/monthly: [no source found];

есть ли API для создания/изменения export job: [no source found];

содержит ли Posts JSON story_fbid, canonical Graph id или permanent permalink: [no source found];

SLA от публикации до появления export: [no source found];

гарантирована ли schema stability: [no source found].

Правильный тест DYI

Создать export:

Profile: нужный personal profile
Categories: Posts
Date range: Last week
Format: JSON
Media quality: Low
Destination: внешний cloud storage

В полученных файлах искать:

id
post_id
story_fbid
permalink
permalink_url
uri
url
timestamp
title
data[].post
attachments

После этого:

сопоставить десять posts с /me/posts;

измерить latency;

проверить edits и deletes;

проверить, меняется ли directory/schema следующего export.

До такого теста DYI нельзя считать заменой API. Но как независимый archival rail он полезен.

6.2. Data Portability destination

Meta имеет отдельный официальный Data Portability product: destination service проходит onboarding, после чего Meta через Data Transfer Project передаёт туда выбранные user data. Документация прямо перечисляет Facebook Posts как поддерживаемый data type. (Data Portability Overview, б/д, проверено 05.08.2026
; Onboarding Guide, б/д
; Deep Linking Data Parameters, б/д
). 
Facebook Developers
+2
Facebook Developers
+2

Но это не простой personal API:

надо подать onboarding application;

Meta должна approve destination;

destination реализует свои OAuth/API endpoints;

transfer инициируется через Export Your Information;

exact transfer latency и canonical Facebook permalink в публичной документации не гарантированы.

Для private one-user integration это организационно несоразмерно. Имеет смысл только если вы строите полноценный reusable «personal data vault destination».

6.3. Meta Content Library API

Не путать:

Professional Dashboard → Content Library

с research product:

Meta Content Library and Content Library API

Research product предоставляет публичный archive Facebook/Instagram/WhatsApp Channels и предназначен для исследователей. Доступ получают люди из qualified academic или nonprofit institutions, ведущие scientific/public-interest research; API работает через SOMAR Virtual Data Enclave или Meta Secure Research Environment. Владение конкретным profile не является отдельным основанием доступа. (Meta Content Library, обновлено 30.04.2026
; Meta newsroom, 21.11.2023, обновлено 26.09.2024
; SOMAR, без даты, проверено 05.08.2026
). 
Meta Transparency
+2
About Facebook
+2

Вердикт: не operational API для автора, вытаскивающего собственные posts.

6.4. Threads

Threads API официально отдаёт own user posts с полями text, timestamp, permalink и media/post ID. Для Threads long-lived token существует явный refresh endpoint; refreshed token снова действует 60 дней. [single-source] (Retrieve User Posts, б/д, проверено 05.08.2026
; Threads Long-Lived Tokens, б/д
). 
Facebook Developers
+1

Это делает Threads более удобным automation source, но:

Threads id не является Facebook id;

Threads permalink ведёт в Threads;

Facebook cross-post может отличаться;

не все Facebook text posts обязательно существуют в Threads.

Подходит как новая publishing architecture:

canonical text store
      ├── Facebook human publication
      ├── Threads API publication
      └── Telegram announcement

Но не как способ восстановить canonical identity уже опубликованного Facebook Post.

6.5. Instagram

Instagram Media API возвращает только media, принадлежащие Instagram Professional accounts. Для обычных personal accounts прежний Basic Display API закрыт с 4 декабря 2024 года. (IG Media, б/д, проверено 05.08.2026
; Basic Display shutdown announcement, 04.09.2024
). 
Facebook Developers
+1

Для text-heavy Facebook profile это слабая замена: Instagram требует media-centric object и даёт Instagram identity, а не Facebook identity.

6.6. Webhooks

[emerging] Текущая User Webhook reference содержит field feed, описанный как изменения большинства частей profile, включая Posts. Meta Webhooks в целом предназначены для real-time HTTP notifications и настраиваются через /app/subscriptions. (User Webhook reference, б/д, проверено 05.08.2026
; Webhooks overview, б/д
; Subscriptions edge, б/д
). 
Facebook Developers
+2
Facebook Developers
+2

Но я не нашёл актуального end-to-end примера 2025–2026, где user.feed надёжно delivers новый post личного profile в role-only app. Поэтому:

не считать webhook доказанным;

сначала сделать изолированный test;

даже при callback использовать webhook лишь как trigger;

canonical data всё равно перечитывать через /{post-id} или /me/posts.

Тест:

1. Subscribe object=user, field=feed.
2. Опубликовать уникальный test post.
3. Проверить callback.
4. Из callback извлечь object/post ID.
5. Readback: /POST_ID?fields=id,message,created_time,permalink_url.
6.7. RSS, Atom, e-mail

Текущего официального RSS/Atom feed для personal profile я не нашёл. Структурированного e-mail notification с canonical post ID и permalink тоже не найдено. [no source found]

Сервисы, обещающие Facebook-to-RSS для personal profile, почти неизбежно возвращают нас к scraping/browser automation.

7. Terms of Service и риск browser automation
7.1. «Мой аккаунт, мои данные» — не исключение из Terms

Facebook Terms, effective 1 января 2025 года, запрещают access or collect data automated means без prior permission и прямо уточняют, что это действует независимо от того, выполняется ли доступ while logged in. Terms также запрещают обход technological measures. (Facebook Terms, effective 01.01.2025
). 
Facebook
+1

Automated Data Collection Terms, effective 7 октября 2024 года, отдельно говорят, что automated collection нельзя выполнять без express written permission либо explicit authorization Meta. Простого принятия этих Terms недостаточно для получения permission. (Automated Data Collection Terms, effective 07.10.2024
; Developer summary, б/д
). 
Facebook
+1

Следовательно:

Graph API с выданным permission — явно authorised interface;

Export Your Information — явно authorised UI workflow;

DOM scraping залогиненного Professional Dashboard — автоматизированный доступ без отдельного permission;

replay private /api/graphql/ — тот же web-product access, а не публичная Graph API authorization;

тот факт, что контент ваш, уменьшает privacy harm, но не отменяет contractual restriction.

Это оценка contractual policy; она не является выводом о гражданской или уголовной законности во всех юрисдикциях.

7.2. Практика enforcement

Meta публично описывает:

отключение accounts, cease-and-desist и lawsuits против scrapers;

Voyager Labs: fake logged-in accounts, scraping posts/profile data, settlement и permanent injunction в декабре 2024 года;

BrandTotal/Unimania: browser extensions, собиравшие данные при посещении Facebook; permanent injunction и финансовое урегулирование;

behavioral detection, rate limits и другие anti-scraping меры.

(Voyager Labs, 12.01.2023, обновлено 13.12.2024
; BrandTotal/Unimania, 01.10.2020, обновлено 30.09.2022
; How We Combat Scraping, 15.04.2021
). 
About Facebook
+2
About Facebook
+2

Эти cases не являются прямыми аналогами вашей схемы: там были mass/commercial scraping, чужие данные, fake accounts или browser extensions. Публичного enforcement precedent исключительно за low-volume выгрузку 10–15 собственных posts в день я не нашёл. [no source found]

Поэтому практическая оценка:

Graph API: low account risk.

Scheduled DYI: low.

Один logged-in browser, свои posts, низкая частота: вероятно ниже mass-scraping risk, но не ноль — [speculative].

Cookie replay, direct internal GraphQL, обход checkpoints: medium/high.

Fake accounts, fingerprint spoofing, proxy rotation: high и явно не оправдано вашей задачей.

8. Насколько устойчив внутренний GraphQL Content Library
Документированное поведение

Публичного/versioned Professional Dashboard Content Library API Meta не документирует. [no source found]

Community evidence

В community reverse-engineering действительно встречаются requests к:

POST https://www.facebook.com/api/graphql/

с параметрами:

fb_api_req_friendly_name
doc_id
variables
fb_dtsg
lsd

Пример такого вызова опубликован в GitHub Gist 6 июня 2025 года. Это не официальный contract и относится к внутренним web APIs. [low-authority] [single-source] (GitHub Gist, 06.06.2025
). 
Gist

Вывод

[established] Нельзя считать doc_id stable endpoint:

он не входит в public Graph API versioning;

schema/variables могут измениться без changelog;

нужны Facebook web cookies и CSRF/session values;

session может попасть под checkpoint;

обращение остаётся automated logged-in web access с точки зрения Terms.

Надёжной статистики «Meta ломает Professional Dashboard каждые N недель» не найдено. [no source found]

Если browser fallback сохраняется:

Считать browser session credential boundary.

Извлекать structured network response, а не парсить rendered table.

Не hardcode один doc_id как постоянный API.

Идентифицировать operation прежде всего по fb_api_req_friendly_name и response contract.

На неизвестной schema переводить pipeline в FAIL_CLOSED.

Не использовать stealth, proxies или обход checkpoints.

Не пускать fallback автоматически после Graph API auth failure: сначала human diagnosis.

Это сделает fallback менее хрупким, но не превратит его в официальный browserless API.

9. Что проверить экспериментом
Неопределённость	Эксперимент	Критерий успеха
Можно ли выдать user_posts свежему self-admin app	Development Mode → Explorer → grant user_posts → /me/posts	Возвращается собственный известный post
Полнота message	Опубликовать длинный post, Unicode, URLs, переносы строк	Byte-normalized текст совпадает
Поведение edits	Прочитать post, отредактировать, прочитать снова	Тот же id, обновлённый message
Tagged posts	Создать tagged test case	Фильтр from.id удаляет чужую публикацию
Стабильность id	Запрашивать один post многократно	id не меняется
Token rolling refresh	/debug_token до и после backend-only polling	Изменение либо отсутствие изменения expires_at
user.feed webhook	Subscribe → publish unique test post	Callback приходит и позволяет readback
DYI canonical identity	7-day Posts JSON export	Найдены story_fbid/permalink либо подтверждено их отсутствие
DYI latency	Сравнить publish time и появление файла	Измерен usable worst case
Policy fit	Только письменный Meta answer/App Review	Явное approval exact use case
Итоговое решение
Вариант A — основной

Graph API v26.0 + user_posts + self-admin Development app.

Плюсы:

official/versioned interface;

canonical Post object;

прямые id, message, created_time, permalink_url;

browserless daily operation;

App Review не нужен;

минимум account risk.

Минусы:

token lifecycle;

narrow Allowed Usage создаёт policy ambiguity для Telegram derivation.

Вариант B — независимый backup

Recurring Export Your Information в JSON на cloud storage.

Плюсы:

официальный user-data export;

не зависит от Graph API permission;

полезен как archival backstop.

Минусы:

неизвестные latency и schema;

canonical ID/permalink не гарантированы;

нет документированного export-control API.

Вариант C — emergency fallback

Professional Dashboard через живой browser, предпочтительно с network response capture вместо DOM parsing.

Плюсы:

по вашему замеру уже работает;

видит то, что показывает owner UI.

Минусы:

самый хрупкий;

Terms risk;

browser/session/checkpoint dependency;

private GraphQL не versioned.

Рекомендация

[established] Внедрять Graph API self-admin path как primary rail; оставить app в Development Mode, pin v26.0, читать /me/posts, фильтровать from.id, хранить opaque id + permalink_url, поставить /debug_token watchdog и заранее предусмотреть one-click OAuth reauthorization. Параллельно включить recurring DYI JSON как archival backup. Browser automation не удалять сразу, но перевести в ручной emergency fallback и не обращаться к internal GraphQL в штатном режиме.

Уверенность:

существование и техническая работоспособность official route: [established];

отсутствие App Review для self-admin role-only app: [established];

browserless ежедневная эксплуатация: [established];

indefinite unattended token renewal: [speculative] до измерения;

полное соответствие Telegram-анонсов текущему Allowed Usage: [emerging].

Источники
Официальная документация Meta

Graph API Versions
 — v26.0 released 29.07.2026.

User Posts — Graph API
 — без даты; проверено 05.08.2026; отображается v26.0.

Post — Graph API
 — без даты; проверено 05.08.2026; отображается v26.0.

User Feed — Graph API
 — без даты; проверено 05.08.2026.

Permissions Reference
 — без даты; проверено 05.08.2026.

Graph API v11.0 changelog
 — v11 released 08.06.2021; user_posts change effective 07.09.2021.

Graph API v14.0 changelog
 — v14 released 25.05.2022.

Facebook Login Changelog
 — historical entry 02.07.2018; review deadline 01.08.2018.

Facebook Login makes updates to further protect privacy
 — 01.05.2018.

Platform Update on Publish Permission
 — 31.07.2018.

Access Levels
 — без даты; проверено 05.08.2026.

Graph API Explorer Guide
 — без даты; проверено 05.08.2026.

Publish / Release an app
 — без даты; проверено 05.08.2026.

Business Verification for Advanced Access
 — 01.02.2023.

Create an App with Meta
 — без даты; проверено 05.08.2026.

Customize Facebook Login Use Case
 — без даты; проверено 05.08.2026.

Paginated Results
 — без даты; проверено 05.08.2026.

Access Tokens
 — без даты; проверено 05.08.2026.

Long-Lived Access Tokens
 — без даты; проверено 05.08.2026.

Authentication versus Data Access
 — без даты; проверено 05.08.2026.

Debug Token
 — без даты; проверено 05.08.2026.

Manually Build a Login Flow
 — без даты; проверено 05.08.2026.

Testing a Login Flow
 — без даты; проверено 05.08.2026.

Export a copy of your Facebook information
 — без даты; проверено 05.08.2026.

Data Portability Overview
 — без даты; проверено 05.08.2026.

Data Portability Onboarding Guide
 — без даты; проверено 05.08.2026.

Data Portability Deep Linking Parameters
 — без даты; проверено 05.08.2026.

Meta Content Library and API
 — обновлено 30.04.2026.

New Tools to Support Independent Research
 — 21.11.2023; обновлено 26.09.2024.

SOMAR Meta Content Library
 — без даты; проверено 05.08.2026.

Threads: Retrieve User Posts
 — без даты; проверено 05.08.2026.

Threads: Long-Lived Tokens
 — без даты; проверено 05.08.2026.

Instagram Media
 — без даты; проверено 05.08.2026.

Instagram Basic Display API shutdown
 — 04.09.2024; shutdown 04.12.2024.

User Webhooks Reference
 — без даты; проверено 05.08.2026.

Webhooks from Meta
 — без даты; проверено 05.08.2026.

Subscriptions Edge
 — без даты; проверено 05.08.2026.

Facebook Terms of Service
 — effective 01.01.2025.

Automated Data Collection Terms
 — effective 07.10.2024.

Automated Data Collection — Developer summary
 — без даты; проверено 05.08.2026.

Leading the Fight Against Scraping-for-Hire
 — 12.01.2023; обновлено 13.12.2024.

Taking Legal Action Against Data Scraping
 — 01.10.2020; обновлено 30.09.2022.

How We Combat Scraping
 — 15.04.2021.

Community / implementation evidence

EY-Office: собственное приложение для чтения Facebook posts
 — 07.05.2026; [low-authority].

GitHub Gist: internal /api/graphql/ example
 — 06.06.2025; [low-authority] [single-source].

Sources
ChatGPT can make mistakes. Check important info.

﻿
Web search
 

Pro