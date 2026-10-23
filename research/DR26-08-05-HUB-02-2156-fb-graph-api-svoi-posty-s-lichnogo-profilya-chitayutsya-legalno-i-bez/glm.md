# DR26-08-05-HUB-02-2156 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chat.z.ai (GLM-5.2, Advanced Search + Deep Think Max)
dr_id: DR26-08-05-HUB-02-2156
topic: "FB Graph API: легальный способ читать СВОИ посты с ЛИЧНОГО профиля Facebook без автоматизации браузера"
date: 2026-08-05

chat_url: https://приватный чат


proof_of_work: "23 источника с URL и датами, пометки [вторичный]/[сообщество]/[single-source]"
verbatim: true
---

> Снимок ответа verbatim, эхо промпта отрезано по якорю «Вердикт: официальный путь».

Вердикт: официальный путь есть — с оговорками. Graph API v26.0 (релиз 29.07.2026) сохраняет эндпоинт /me/posts и разрешение user_posts; для доступа к собственным данным администратору приложения НЕ требуется прохождение App Review, достаточно Development Mode + Standard Access. Главная оговорка — токен живёт ~60 дней и не имеет задокументированного серверсайда-механизма неинтерактивного продления; раз в ~60 дней потребуется короткое ручное действие (перелогин через OAuth), полностью автоматизировать этот шаг без браузера нельзя.

1. Статус user_posts в актуальной версии Graph API

Разрешение user_posts существует и документировано в Permissions Reference для Graph API v26.0 (текущая версия, выпущена 29.07.2026). Описание гласит: «The user_posts permission allows your app to access the posts that a user has made on their timeline».

Применимо к личному профилю владельца токена: да. Эндпоинт /me/posts (он же /{user-id}/posts) требует именно User Access Token + user_posts и возвращает «Posts created on the app user's Timeline and Posts in which the app user has been tagged».
Требует ли App Review: для доступа к данным пользователей, не имеющих роли в приложении — да (Advanced Access). Для самого администратора/разработчика/тестировщика приложения — нет, достаточно Standard Access (см. раздел 3).
Волна ограничений после 2018: user_posts пережил чистку 2018–2019 гг. (когда были закрыты read_stream, user_actions:*, часть полей /me/feed), но остался в категории «advanced permissions» с узким Allowed Usage (цифровые/физические альбомы таймлайна; родительский контроль для несовершеннолетних). Это означает, что App Review для Advanced Access пройдёт только под эти сценарии — но для self-use они не нужны.

Источник, дата: developers.facebook.com/docs/permissions и developers.facebook.com/docs/graph-api/reference/user/posts, обе страницы несут метку Graph API Version v26.0; проверено 06.08.2026.

Уверенность: [established] — первоисточник (Meta for Developers), текущая версия API, перекрёстно подтверждено страницей Post node.

2. Какие эндпоинты и поля реально работают сегодня

Рабочий эндпоинт: GET /v26.0/me/posts?fields=id,message,created_time,permalink_url

ЭНДПОИНТ
	
СТАТУС (V26.0)
	
ЧТО ОТДАЁТ
	
ЧТО НУЖНО

/me/posts	Живой	Посты на таймлайне app user + посты, где он тегнут	User Token + user_posts
/me/feed	Существует, но существенно урезан	Лента пользователя; после 2018 отсутствуют посты друзей и большая часть контента других	User Token + user_posts / read_insights в зависимости от поля
/me/published_posts	Не документирован для User (только Page)	—	—

Доступные поля на Post node (v26.0, подтверждено из первоисточника):

ПОЛЕ
	
ТИП
	
DEFAULT
	
НАЗНАЧЕНИЕ

id	token (Post ID)	Да	Стабильный идентификатор
message	string	Да	Полный текст поста
created_time	datetime	Да	Время публикации (UNIX timestamp)
permalink_url	uri	Нет	«The permanent static URL to the post on www.facebook.com»

Все четыре поля, требуемые в задаче, присутствуют в спецификации v26.0. permalink_url на Post node не был затронут изменением v20.0 (тогда менялось только поведение permalink_url для Comment node при наличии лишь Page Public Content Access).

Ключевое отличие от браузерного пути: здесь permalink_url отдаётся напрямую полем — не требуется декодировать base64 content_id и собирать S:_I<user_id>:<story_fbid>.

Уверенность: [established] — первоисточник (Post node reference v26.0).

Что нужно проверить экспериментом: реально ли /me/posts возвращает посты, созданные вручную через facebook.com (а не только опубликованные самим приложением). Документация Post node различает два случая: «A user access token can read any post your application created on behalf of that user» и «A user's post can be read if the owner has granted the user_posts permission» — вторая формулировка покрывает вручную созданные посты, но это единственное место, где стоит сделать контрольный запрос через Graph API Explorer до написания кода.

3. App Review для приложения с одним пользователем — самим собой

Это ключевой положительный факт всего исследования. Документация Access Levels (Graph API v26.0) прямо разделяет два уровня:

«Permissions with Standard Access... can only be requested from app users who have a role on the requesting app... If your app will only be used by people who have a role on it, the permissions and features your app requires will only need Standard Access... All Business, Consumer, and Gaming apps are automatically approved for Standard Access for all permissions and features available to their app type.»

Документация App Roles (обновлена 08.09.2025) уточняет, что означает «роль»:

«Administrators... can grant the app any permission while it is in development, and all features are active for Admins while it is in development.»

И аналогично для Developer и Tester: «can grant the app any permission while it is in development, and all features are active for [role] while it is in development».

Блог-пост о Live Mode (23.09.2019, всё ещё актуальная политика) подтверждает: «When an app is in development mode, it will have access to all permissions and features, but can only access data for the following roles on the app: Administrator, Developer, Tester and Analytics User».

Вывод для сценария «один автор = владелец аккаунта = администратор приложения»: App Review не требуется. Приложение остаётся в Development Mode, автор добавляет сам себя как Administrator, получает User Token с разрешением user_posts (Standard Access), и /me/posts работает для его собственного профиля без каких-либо подач на ревью, скринкастов и Business Verification. Сценарий «для личного использования» явно описан как назначение Standard Access.

Оговорки:

В Dev Mode есть ограничения по rate limits и количеству пользователей (только роли), но для одного пользователя это нерелевантно.
90-дневное правило неактивности: «If your app does not use a permission for 90 days, usually due to user inactivity, your app user must regrant your app that permission». При ежедневной выгрузке это правило не сработает.
Приложение должно быть типа Consumer (а не Business), чтобы не требовалась Business Verification для Advanced Access — но Standard Access и так автоматический для всех типов.

Уверенность: [established] — три независимые страницы первоисточника Meta (Access Levels, App Roles, Live Mode blog) сходятся.

4. Срок жизни токенов и продление

Каноническая цепочка (документация FB Login, Long-Lived Access Tokens):

Short-lived User Token (~1–2 часа) — выдаётся Login Dialog / Graph API Explorer.
Обмен short → long через GET /v26.0/oauth/access_token?grant_type=fb_exchange_token&client_id=...&client_secret=...&fb_exchange_token=.... На выходе — long-lived User Token со сроком ~60 дней.
Long-lived User Token — ~60 дней.

Можно ли продлить long-lived User Token неинтерактивно (серверсайда, без SDK)?

Документация Meta однозначна: «You can not use an expired token to request a long-lived token. If the token has expired, your app must send the user through the login flow again to regenerate». И далее: «Native mobile apps using Facebook's SDKs get long-lived User access tokens, good for about 60 days. These tokens are refreshed once per day, when the person using your app makes a request to Facebook's servers. If no requests are made, the token will expire after about 60 days, and the person will need to go through the login flow again to obtain a new token».

Сторона «за» автоматическое продление: ряд сообщественных тредов утверждает, что регулярное использование long-lived токена само по себе продлевает его на 60 дней (якобы сервер обновляет expiry при каждом запросе).

Сторона «против» (официальная): документация привязывает механизм «refreshed once per day» исключительно к native mobile SDK. Для server-side REST-клиента (curl/HTTP-библиотека) задокументированного эндпоинта long → long не существует. Эндпоинт fb_exchange_token принимает только short-lived токен. Это подтверждается множеством тредов разработчиков, у которых токен умирал ровно через 60 дней серверсайд-использования.

Консенсус: для чисто серверной автоматизации без браузера long-lived User Token умирает через ~60 дней, и легального неинтерактивного продления нет. Придётся раз в ≤60 дней инициировать Login Flow.

Сравнение с Threads API: у Threads ситуация лучше — там задокументирован эндпоинт GET graph.threads.net/access_token?grant_type=th_refresh_token, который продлевает long-lived токен на новые 60 дней, если он старше 24 часов, но не истёк. Для Facebook User Token аналогичного th_refresh_token-механизма в документации нет. Это асимметрия между платформами Meta.

Что ломает токен (до истечения 60 дней):

Смена пароля Facebook-аккаунта — инвалидирует сессию и токен.
«Log out of all sessions» в настройках безопасности Facebook.
Удаление приложения пользователем из Business Integrations / Settings → Apps.
Ревокация разрешения user_posts через настройки Facebook.
Ротация со стороны платформы (редко, но бывает при инцидентах безопасности).
90 дней неактивности по разрешению (отдельно от 60-дневного expiry токена).

Легальная автоматизация продления: единственный способ — перед истечением 60 дней выполнить OAuth Login Flow заново. Если FB-сессия в браузере жива, это один клик (без ввода пароля). Чисто серверсайд-способы (system user tokens) применимы только к бизнес-активам (Pages, Ad Accounts), но не к личному профилю User — system user token не представляет личный профиль и не даёт user_posts.

Уверенность: [established] для отсутствия серверсайд-продления (официальная документация + множество подтверждений); [emerging] для тезиса «регулярное использование иногда продлевает» — только low-authority сообщества, официального подтверждения нет.

5. Альтернативы, если user_posts недостаточен
5.1. Facebook Content Library / Content Library API

Не подходит. Доступ к Meta Content Library и Content Library API предоставляется только исследователям, аффилированным с квалифицированным академическим или некоммерческим научным учреждением: «To be eligible for access, applicants must be affiliated with a qualified academic institution or a qualified research institution». Это инструмент для публичных данных исследовательского характера, а не для экспорта собственных постов. Заявка идёт через CASD/Research Tools Manager, рассмотрение 2–3 недели + 2–3 недели обработки. Для одиночного автора без академической аффилиации путь закрыт; кроме того, Content Library работает с публичным контентом Pages/IG, а не с личным профилем.

Уверенность: [established], источник — первоисточник Meta (get-access, актуальная страница v26.0).

5.2. Download Your Information (DYI)

DYI — это ручной/полуручной экспорт архива данных аккаунта через настройки Facebook. Доступен через facebook.com/settings → «Download your information».

Программный запуск по расписанию: официального публичного API для инициирования DYI нет. У Meta есть внутренние эндпоинты GraphQL, которые использует веб-клиент, но они не задокументированы и обращение к ним подпадает под Automated Data Collection Terms (см. раздел 6). [single-source для отсутствия API; основано на отсутствии документации]
Формат: JSON или HTML; посты приходят в секции posts/ с полями timestamp, data[].post, uri (permalink присутствует в некоторых категориях).
Задержка: генерация архива занимает от минут до суток; не годится для ежедневной выгрузки за последние сутки без задержки.
permalink / story_fbid: в JSON-экспорте присутствуют URI постов, но структура нестабильна и меняется.

Уверенность: [emerging] — для программного инициирования DYI нет официального API; оценка основана на актуальной документации Help Center. 【single-source】

5.3. Instagram / Threads кросс-постинг

Если автор кросс-постит те же тексты в Threads, доступ через Threads API стабильнее в части продления токена.

Threads API (graph.threads.net): поддерживает long-lived token с продлением через grant_type=th_refresh_token, который обновляет токен на 60 дней, если он старше 24 часов и не истёк. Это даёт легальное серверсайд-продление, недоступное для Facebook User Token.
Поля: текст поста, timestamp, permalink, id — отдаются через /me/threads и контент-эндпоинты. Text-only посты поддерживаются с начала 2025.
Оговорка: это даёт тексты из Threads, а не из Facebook. Если кросс-постинг идёт FB→Threads автоматически и идентично, это рабочий суррогат. Если посты публикуются только в FB, Threads API бесполезен.
Лимиты: 4,800 запросов в привязке к импрешенам; для 10–15 постов/день некритично.

Уверенность: [established] для Threads API refresh-механизма (официальная документация, обновлена 27.03.2025).

5.4. RSS/Atom, e-mail уведомления, Webhooks
RSS/Atom: Meta не предоставляет RSS-фиды для личных профилей или Pages с 2015+; живых официальных RSS нет. [no source found — основано на отсутствии документации; historically deprecated]
E-mail уведомления: можно включить уведомления о собственных публикациях, но они не содержат полного текста и permalink в стабильной форме; не годятся для парсинга.
Webhooks (Graph API webhooks product): для объекта User существует подписка на поле feed, которое «Describes changes to most sections of the user's profile, such as About, Photos, Posts, Friends, and Likes». НО payload поля feed для User object содержит только {field: "feed"} — без ID поста, текста или permalink. Это уведомление «что-то изменилось», после которого нужно делать follow-up запрос /me/posts через Graph API. Для объекта Page webhooks feed отдают больше деталей (post_id, message, from). Для личного профиля webhook лишь экономит один polling-запрос, но не заменяет Graph API. Требует HTTPS endpoint с публичным адресом.

Уверенность: [established] для webhooks (первоисточник v26.0 + hookdeck-гайд от 22.07.2026 как вторичное подтверждение структуры).

6. Правовая и рискованная рамка
6.1. Automated Data Collection Terms (effective October 7, 2024)

Отдельный документ Meta — Automated Data Collection Terms — прямо запрещает автоматизированный сбор данных без явного письменного разрешения: «You will not engage in Automated Data Collection without first obtaining Meta's express written permission or in any manner that is not explicitly authorized by Meta». Причём «acceptance of these Terms alone does not constitute the required written permission».

Ключевая оговорка: эти Terms применяются к сбору данных через автоматизированные средства вне официального Graph API. Использование Graph API с валидным токеном и разрешениями — это легитимный доступ через Platform, а не «Automated Data Collection» в смысле этого документа. Графа «Use of data collected through Automated Data Collection» в Terms описывает именно scraping/сбор через неавторизованные автоматизированные средства (поисковые движки, preview-сервисы).

6.2. Meta Platform Terms (последнее обновление 3 февраля 2026)

Platform Terms регулируют именно использование Graph API. Ключевые пункты:

Лицензия на использование Platform — «limited, non-exclusive, non-sublicensable... license to use, access, and integrate with Platform» при соблюдении Terms и Developer Policies. Запрещено «reverse engineer, or otherwise exploit Platform».
Enforcement: «We may suspend or end your App's access to any Platform APIs, permissions, or features that your App has not used or accessed within a 28-day period». То есть 28 дней неактивности = риск потери доступа к разрешениям/фичам (отдельно от 90-дневного правила перегранта).
При нарушении: «suspending or permanently removing your App(s) and account, removing your access and your App's access to Platform».
6.3. Meta Terms of Service (effective January 1, 2025)

Общие ToS для пользователей. Содержат обязательство «Create only one account (your own) and use it for personal purposes» и запрет передачи доступа. Автоматизированный доступ к собственному аккаунту через Graph API ToS прямо не запрещают; запрещена автоматизация через scraping залогиненных страниц.

6.4. Граница «мой аккаунт, мои данные» vs. нарушение
Внутри Graph API с валидным токеном и user_posts: легально. Это и есть предусмотренный платформой способ доступа к собственным данным. Platform Terms это разрешают.
Браузерная автоматизация залогиненной сессии (текущий путь через Professional Dashboard → Content Library): формально подпадает под Automated Data Collection Terms, требующие письменного разрешения Meta, которого у автора нет. Даже для собственных данных. То, что аккаунт свой, не выводит из-под этих Terms — они применяются к способу сбора, а не к владельцу данных.
Прецеденты: дело Meta v. Bright Data (январь 2024, федеральный суд) установило, что logged-out scraping публичных данных не регулируется ToS Meta, поскольку ToS применяется только к держателям аккаунтов, активно использующим продукты Meta. Но это защищает именно logged-out scraping публичного; logged-in scraping (как в текущем пути автора) остаётся под действием ToS, и у Meta «stronger legal standing».
6.5. Риск блокировки аккаунта на практике
Через Graph API (Dev Mode, свой аккаунт, свой app): риск минимален. Это штатный механизм. Основной риск — случайное нарушение 28-дневного правила неактивности (решается регулярным использованием) и annual Data Use Checkup (применим только к Advanced Access, которого у автора не будет).
Через браузерную автоматизацию залогиненной сессии: риск выше. Meta применяет автоматический и ручной enforcement, включая «suspending or permanently removing your App(s) and account». На практике аккаунты, занимающиеся автоматизированным скрейпингом собственного/чужого контента через Selenium/Playwright с залогиненной сессией, попадают под checkpoint-блокировки (требование подтвердить личность, сменить пароль) и временные/постоянные заморозки. [single-source для практики — сообщества r/facebook, разработческие треды; количественной статистики Meta не публикует]

Уверенность: [established] для текста Terms (первоисточники, даты указаны); [emerging] для практического риска блокировки при браузерной автоматизации.

7. Устойчивость браузерного пути и внутренние JSON-эндпоинты

Если браузерный путь остаётся основным, оценка устойчивости:

Частота поломок разметки Professional Dashboard: Meta регулярно (раз в несколько недель–месяцев) меняет внутреннюю структуру relay/GraphQL-запросов и DOM Professional Dashboard. Конкретная частота не задокументирована; сообщества разработчиков фиксируют поломки automation-скриптов после каждого крупного релиза Meta Business Suite. [low-authority, сообщества]
Стабильный внутренний GraphQL doc_id: внутренние GraphQL-мутации и запросы (graphql.facebook.com/api/graphql/ с doc_id) не задокументированы как публичный API и переживают редизайны непредсказуемо — doc_id инвалидируются при изменениях схемы. Обращение к ним из стороннего клиента с залогиненной сессией усиливает риск под пунктом 6.1–6.4 (Automated Data Collection).
Риск: выше, чем у Graph API, и по техническим, и по ToS-причинам. Любое обращение к недокументированным эндпоинтам с doc_id — это автоматизированный сбор вне официального Platform, то есть именно то, что требуют Automated Data Collection Terms.

Уверенность: [speculative] для частоты поломок (нет количественных данных); [established] для ToS-риска обращения к внутренним GraphQL.

Сводная таблица вариантов
ПУТЬ
	
РАБОТАЕТ В 2026
	
ЧТО НУЖНО ОДИН РАЗ
	
ЧТО НУЖНО РЕГУЛЯРНО
	
СРОК ЖИЗНИ
	
РИСК ДЛЯ АККАУНТА
	
ПРИГОДНОСТЬ ДЛЯ ЕЖЕДНЕВНОЙ АВТОМАТИЗАЦИИ

Graph API /me/posts + user_posts (Dev Mode, сам-админ)	Да (v26.0)	Создать Consumer app, добавить себя как Admin, пройти OAuth Login, обменять токен на long-lived	Раз в ≤60 дней — повторный OAuth Login Flow (1 клик, если сессия жива); ежедневный polling-запрос	~60 дней, продление только интерактивно	Низкий (штатный Platform-доступ)	Высокая
Webhooks user.feed + Graph API follow-up	Да (v26.0)	То же + публичный HTTPS endpoint для callback	Как выше; webhook лишь триггер, данные тянутся через /me/posts	Как выше	Низкий	Средняя (лишний компонент, payload скудный)
Threads API (если кросс-постинг)	Да	Threads-приложение, OAuth, long-lived token	Продление токена через th_refresh_token (серверсайд, легально)	60 дней, продлевается неинтерактивно	Низкий	Высокая, но только для Threads-контента
DYI (Download Your Information)	Частично (ручной)	Ничего	Ручной запрос архива каждый раз	—	Низкий	Низкая (задержки, нет API)
Content Library API	Нет (для self-use)	Академическая аффилиация, заявка 4–6 нед	—	—	—	Не применимо
Браузерная автоматизация (текущий путь)	Да, но хрупко	Скрипт + залогиненная сессия	Поддержка скрипта при поломках разметки	Сессия (недели–месяцы)	Средне-высокий (ToS)	Средняя (хрупкость)
Пошаговая инструкция для победившего варианта (Graph API /me/posts)
Шаг 1. Создание приложения
Зайти на developers.facebook.com → My Apps → Create App.
Выбрать тип Consumer (не Business — иначе потребуется Business Verification для Advanced Access; для Standard Access это необязательно, но Consumer проще для личного use case).
Название приложения, контактный e-mail, App Purpose — выберите сценарий, closest к «для себя» (например, «None» / Utilities). Приложение создаётся в Development Mode по умолчанию.
Шаг 2. Настройка Facebook Login
В App Dashboard → Add Product → Facebook Login → Set Up.
В Facebook Login → Settings: указать Valid OAuth Redirect URIs (например, https://localhost/ для локального тестирования или ваш HTTPS-endpoint).
App Dashboard → App Settings → Basic: заполнить Privacy Policy URL (можно любую публичную страницу), App Icon, сохранить.
Шаг 3. Добавление разрешения user_posts
App Dashboard → App Review → Permissions and Features.
Найти user_posts → нажать Get Standard Access (не Advanced). Standard Access выдаётся автоматически для всех типов приложений.
Шаг 4. Добавление себя как Administrator (если не автоматически)

Создатель приложения уже является Administrator. Проверить: App Dashboard → Roles → People — ваш аккаунт должен быть с ролью Administrator.

Шаг 5. Получение User Access Token через OAuth
В браузере, будучи залогиненным в свой Facebook-аккаунт, открыть:
text
https://www.facebook.com/v26.0/dialog/oauth?client_id=ВАШ_APP_ID&redirect_uri=ВАШ_REDIRECT_URI&scope=user_posts&response_type=code
Подтвердить диалог разрешения (запрашивается user_posts).
На redirect URI получить code.
Обменять code на short-lived token:
text
GET https://graph.facebook.com/v26.0/oauth/access_token?
  client_id=ВАШ_APP_ID&redirect_uri=ВАШ_REDIRECT_URI&
  client_secret=ВАШ_APP_SECRET&code=ПОЛУЧЕННЫЙ_CODE

Альтернатива для первого теста — Graph API Explorer (developers.facebook.com/tools/explorer/): выбрать своё приложение, нажать «Generate Access Token», отметить user_posts. Это short-lived токен для быстрой проверки.

Шаг 6. Обмен на long-lived token
text
GET https://graph.facebook.com/v26.0/oauth/access_token?
  grant_type=fb_exchange_token&
  client_id=ВАШ_APP_ID&
  client_secret=ВАШ_APP_SECRET&
  fb_exchange_token=SHORT_LIVED_TOKEN

В ответе — access_token (long-lived, ~60 дней) и expires_in (~5184000 секунд).

Шаг 7. Контрольный запрос
text
GET https://graph.facebook.com/v26.0/me/posts?
  fields=id,message,created_time,permalink_url&
  limit=100&
  access_token=LONG_LIVED_TOKEN

Ожидаемый ответ:

json
{ "data": [ { "id": "...", "message": "...", "created_time": "2026-08-05T...", "permalink_url": "https://www.facebook.com/..." } ], "paging": {...} }

Для выгрузки «за последние сутки» — фильтрация по created_time на стороне клиента (параметр since поддерживается, но надёжнее фильтровать локально, учитывая таймзону).

Шаг 8. Ежедневная автоматизация
Cron-задача (раз в сутки) выполняет GET-запрос из Шага 7, парсит JSON, отбирает посты за последние 24 часа, формирует анонсы в Telegram.
Токен хранить в секрете (env variable / secret manager), не в коде.
Логировать expires_in при каждом запросе для мониторинга срока жизни.
Что делать, когда токен протухнет

Симптомы: API возвращает error: { code: 190, message: "Error validating access token: Session has expired..." } или code: 200 (permission error после ревокации).

Порядок действий:

Проверить токен через Access Token Debugger (developers.facebook.com/tools/debug/accesstoken/) — покажет expires_at и валидность.
Если токен истёк по времени (60 дней): единственный путь — повторить Шаги 5–6 (OAuth Login Flow + обмен на long-lived). Если FB-сессия в браузере жива, это один клик в диалоге без ввода пароля. Полностью серверсайд-способа нет.
Если токен инвалидирован досрочно (смена пароля / logout all sessions / удаление app): повторить Шаг 5 с нуля, убедиться что разрешение user_posts всё ещё granted в настройках Facebook → Settings → Apps.
Автоматизация напоминания: за 7 дней до expires_at (вычисляется из expires_in при получении токена) отправлять себе уведомление (Telegram-бот / e-mail) о необходимости перелогина.
Частичная автоматизация (серая зона): можно автоматизировать OAuth-редирект через headless-браузер с сохранённой сессией — но это возвращает браузерную автоматизацию и её ToS-риски (раздел 6). Легального чистого server-side способа нет.

Что НЕЛЬЗЯ делать: пытаться обменять long-lived токен через fb_exchange_token (эндпоинт принимает только short-lived) — будет ошибка.

Что нужно проверить экспериментом
[критично] Возвращает ли /me/posts в Dev Mode посты, созданные вручную через facebook.com (не приложением). Документация это поддерживает, но формулировка про «post your application created on behalf of that user» создаёт неоднозначность. Эксперимент: опубликовать тестовый пост вручную, через ~5 минут сделать запрос /me/posts — пост должен появиться.
[критично] Присутствует ли permalink_url в ответе /me/posts для постов личного профиля (а не только Page posts). Поле задокументировано на Post node, но historically были случаи, когда для User-постов оно возвращало пустоту или ошибку «field not available on User node». Эксперимент: явный запрос fields=permalink_url.
[желательно] Поведение long-lived токена при регулярном ежедневном использовании — продлевается ли автоматически (сообщества расходятся с документацией). Эксперимент: получить long-lived токен, ежедневно делать запрос, через 55 дней проверить expires_at через debugger. Если сдвинулся — продление работает; если осталось 5 дней — не работает.
[желательно] Работают ли webhooks user.feed для собственного профиля в Dev Mode и приходит ли уведомление при публикации поста. Требует публичного HTTPS-endpoint.
Финальная рекомендация

Рекомендуемый вариант: Graph API /me/posts + user_posts в Development Mode (Consumer app, сам-админ), long-lived token 60 дней с ручным перелогином раз в ≤60 дней.

Trade-off: единственная ручная операция — раз в ~60 дней кликнуть OAuth-диалог (1 секунда, если FB-сессия жива). Взамен — легальный, задокументированный, ToS-compliant доступ с прямым permalink_url без декодирования base64.
Уверенность: [established] для технической осуществимости и легальности; [emerging] для безотказности автопродления токена (см. эксперимент 3).
Fallback, если эксперимент 1 или 2 провалится (например, /me/posts не отдаёт вручную созданные посты или нет permalink_url): рассмотреть связку с Threads API при наличии кросс-постинга — там токен продлевается неинтерактивно, что решает главную боль.
Чего избегать: браузерной автоматизации залогиненной сессии как основного пути — она работает, но подпадает под Automated Data Collection Terms (effective Oct 7, 2024) и несёт средне-высокий риск блокировки аккаунта.
Источники
User Posts — Graph API v26.0, Meta for Developers. https://developers.facebook.com/docs/graph-api/reference/user/posts — проверено 06.08.2026.
Post node — Graph API v26.0, Meta for Developers. https://developers.facebook.com/docs/graph-api/reference/post — проверено 06.08.2026.
Permissions Reference, Meta for Developers (v26.0). https://developers.facebook.com/docs/permissions — проверено 06.08.2026.
App Roles, Meta for Developers, Updated Sep 8, 2025. https://developers.facebook.com/documentation/development/build-and-test/app-roles【turn5fetch0】
Access Levels, Meta for Developers (v26.0). https://developers.facebook.com/docs/graph-api/overview/access-levels【turn1fetch0】
All apps must be set to Live Mode for production use, Meta for Developers Blog, Sep 23, 2019. https://developers.facebook.com/blog/post/2019/09/23/live-mode-for-production-use【turn1fetch2】
Long-Lived Access Tokens, Meta for Developers (FB Login). https://developers.facebook.com/documentation/facebook-login/guides/access-tokens/get-long-lived — проверено 06.08.2026.
Threads — Long-Lived Access Tokens, Meta for Developers, Mar 27, 2025. https://developers.facebook.com/documentation/threads/get-started/long-lived-tokens【turn0search16】
Graph API Versions / Changelog, Meta for Developers. https://developers.facebook.com/docs/graph-api/changelog — v26.0 released July 29, 2026.
Introducing Graph API v26.0, Meta for Developers Blog, Jul 29, 2026. https://developers.facebook.com/blog/post/2026/07/29/introducing-graph-api-v26-and-marketing-api-v26【turn0search16】
Webhooks Reference: User — v26.0, Meta for Developers. https://developers.facebook.com/docs/graph-api/webhooks/reference/user — проверено 06.08.2026.
Webhooks from Meta, Meta for Developers. https://developers.facebook.com/docs/graph-api/webhooks【turn0search10】
Guide to Facebook Webhooks, Hookdeck, Jul 22, 2026. https://hookdeck.com/webhooks/platforms/guide-to-facebook-webhooks-features-and-best-practices [вторичный источник]
Meta Content Library — Get access, Meta for Developers (v26.0). https://developers.facebook.com/docs/content-library-and-api/get-access【turn5fetch2】
Meta Platform Terms, last updated Feb 3, 2026. https://developers.facebook.com/terms/dfc_platform_terms【turn5fetch1】【turn4search23】
Meta Terms of Service, effective Jan 1, 2025. https://www.facebook.com/terms【turn4search22】
Automated Data Collection Terms, effective Oct 7, 2024. https://www.facebook.com/apps/site_scraping_tos_terms.php【turn1fetch1】
v20.0 Changelog (permalink_url Comment change), Meta for Developers. https://developers.facebook.com/docs/graph-api/changelog/version20.0【turn5fetch3】
App Review Submission Guide, Meta for Developers, Jun 30, 2026. https://developers.facebook.com/documentation/resp-plat-initiatives/individual-processes/app-review/submission-guide【turn0search22】
Facebook Page Access Token That Doesn't Expire, bundle.social, Aug 2, 2026. https://bundle.social/blog/facebook-page-access-token [вторичный, недавний]
Refreshing facebook long lived access token without user interaction, Stack Overflow. https://stackoverflow.com/questions/24166392/refreshing-facebook-long-lived-access-token-without-user-interaction [сообщество]
Many of our tokens have expired, Meta Developer Community Forum. https://developers.facebook.com/community/threads/1060220508426875 [сообщество]
Meta Ad Library Scraping & Terms of Service, admake.ai. https://admakeai.com/blog/meta-ad-library-scraping-terms-of-service [вторичный, ссылается на Meta v. Bright Data, Jan 2024]
Deep Think
Max