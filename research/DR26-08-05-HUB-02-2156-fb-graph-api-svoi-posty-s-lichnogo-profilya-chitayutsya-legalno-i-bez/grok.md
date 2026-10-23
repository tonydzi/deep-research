# DR26-08-05-HUB-02-2156 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (Grok Heavy)
dr_id: DR26-08-05-HUB-02-2156
topic: "FB Graph API: легальный способ читать СВОИ посты с ЛИЧНОГО профиля Facebook без автоматизации браузера"
date: 2026-08-05

chat_url: https://приватный чат

proof_of_work: "Thought for 1m 28s · 518 sources"
verbatim: true
---

> Снимок ответа verbatim, эхо промпта отрезано по якорю «Thought for 1m 28s» (методика скилла dr-fanout).

Есть официальный путь с оговорками.

Официальный, поддерживаемый Meta способ в 2026 году существует через Graph API (v26.0, введён 29 июля 2026): эндпоинт GET /me/posts или /{user-id}/posts с разрешением user_posts и User Access Token. Он отдаёт собственные посты личного профиля (Timeline + tagged), включая стабильный id, message (полный текст), created_time и permalink_url. Developers.facebook +1

Ключевые оговорки: приложение должно быть в режиме, где вы (владелец) имеете роль Admin/Developer/Tester (Standard Access автоматически; App Review не требуется для собственных данных); токен живёт ~60 дней и требует периодического обновления с участием человека (полный non-interactive forever-токен для User отсутствует); allowed usage разрешения узкий (книги/альбомы timeline, parental controls); браузерная автоматизация Professional Dashboard нарушает ToS даже для своих данных. Developers.facebook +2

Эксперимент обязателен: создать Consumer-приложение, добавить себя Admin, сгенерировать токен в Graph API Explorer с user_posts и проверить реальный объём возвращаемых постов (все текстовые? stories? задержка? rate limits при 10–15 постах/день).

## Таблица вариантов (состояние на август 2026)

| Путь | Работает в 2026? | Что требуется один раз | Что требуется регулярно | Срок жизни | Риск для аккаунта | Пригодность для ежедневной автоматизации |
|---|---|---|---|---|---|---|
| Graph API /me/posts + user_posts (own app, Standard Access / Dev Mode) | Да (docs v26.0) | Создать Consumer app, добавить себя Admin, получить User token с user_posts | Обновление long-lived token ~раз в 60 дней (login или exchange) | ~60 дней (long-lived) | Низкий (официальный API) | Высокая (после setup; polling или + webhooks) |
| Content Library API / Meta Research Tools | Нет для этого кейса | Академическая/некоммерческая аффилиация + заявка через CASD | N/A | N/A | Низкий (research) | Нулевая (только public content, cleanroom, не для private daily export) Transparency.meta |
| Download Your Information / EYI (Export Your Information) | Частично | Настройка deep-link или ручной экспорт | Пользовательское подтверждение/инициация (даже scheduled daily) | По запросу | Нулевой | Низкая (задержка часы–дни, не real-time, не pure API trigger) Developers.facebook |
| Instagram/Threads Graph API (если кросс-пост) | Частично (Threads лучше) | Professional/Threads account + app + threads_basic и т.п. | Token refresh ~60 дней | | Низкий | Средняя (только если тексты дублируются на Threads; личный FB profile напрямую не покрывает) |
| Webhooks (User.feed) | Да как дополнение | Setup callback + subscribe to user.feed | Поддержание endpoint | Пока app/token живы | Низкий | Средняя (real-time notify о изменениях, но не full historical dump; нужен follow-up Graph call) Developers.facebook |
| Browser automation (Professional Dashboard / Content Library UI) | Работает технически | Скрипт + logged-in session | Поддержание при изменениях UI | Хрупкий | Высокий (явное нарушение ToS) | Низкая (хрупкость + риск) |
| RSS / e-mail / неофициальные | Нет стабильного | N/A | N/A | N/A | Высокий | Нулевая |

## Пошаговая инструкция для победившего варианта (Graph API, own data)

1. Зайти на developers.facebook.com → создать приложение (тип Consumer / Other → Consumer; не Business, если не нужен BM). Developers.facebook
2. В App Dashboard → Roles → добавить себя как Administrator (или Developer/Tester).
3. Добавить продукт Facebook Login (если нужно).
4. В Graph API Explorer (или через Login flow): выбрать своё приложение → Get User Access Token → запросить user_posts (и при необходимости public_profile). Авторизоваться своим аккаунтом. Получить short-lived token.
5. Обменять на long-lived (~60 дней):
   `GET https://graph.facebook.com/v26.0/oauth/access_token?grant_type=fb_exchange_token&client_id={APP_ID}&client_secret={APP_SECRET}&fb_exchange_token={SHORT_TOKEN}`
   (только server-side). Developers.facebook
6. Запрос:
   `GET https://graph.facebook.com/v26.0/me/posts?fields=id,message,created_time,permalink_url&since={unix_yesterday}&limit=50&access_token={LONG_TOKEN}`
   (или /me/feed). Пагинация через paging.next.
   Ожидаемый ответ: массив Post nodes с нужными полями. Developers.facebook +1
7. (Опционально) Настроить Webhooks на User object → field feed для near-real-time уведомлений об изменениях, затем догружать полный пост. Developers.facebook
8. Хранить token безопасно (server-side). App может оставаться в Development Mode — для role-holders этого достаточно. Developers.facebook +1

## Что делать, когда токен протухнет

- Если long-lived ещё не истёк — попробовать повторный exchange (fb_exchange_token с текущим long-lived; многие отчёты 2025–2026 подтверждают, что это даёт новый ~60-дневный, если ≥24 ч и не expired). Dev
- Если expired — пользователь (вы) должен заново пройти login flow (Graph Explorer / OAuth dialog), получить новый short-lived и обменять. Полностью non-interactive продление после expiry невозможно. Инвалидация также происходит при смене пароля, logout all sessions, revoke permissions. Developers.facebook

## Правовая рамка и риски

Meta явно запрещает automated data collection вне Platform APIs, даже logged-in и для своих данных:
«You may not access or collect data from our Products using automated means (without our prior permission) … regardless of whether such automated access or collection is undertaken while logged-in to a Facebook account» (ToS 3.2.3 + Automated Data Collection policy, обновлено Feb 2025). Developers.facebook

Браузерный путь (Professional Dashboard + scroll/декодирование content_id) — нарушение. Официальный Graph API — разрешённый способ. Практика enforcement для low-volume own-data обычно мягче, чем для scraping чужих, но прецеденты банов существуют, и ToS не делает исключения. [established] для запрета; [emerging] для реальной вероятности бана при 10–15 постах/день.

## Неуверенности и что проверить экспериментом

- Полнота возврата: docs говорят «Posts created on the app user's Timeline and Posts in which the app user has been tagged». Не все типы (stories? temporary? hidden?) гарантированы. [established] по docs; completeness — [speculative] без теста.
- Rate limits / soft throttling при ежедневных 10–15 постах + polling.
- Data Use Checkup или будущие ограничения allowed_usage (очень узкий: books/albums + parental).
- Реальная возможность silent re-exchange long-lived до expiry (официально не обещано как perpetual, но работает у многих).
- Устаревшие источники (до 2025) игнорировались; все ключевые claims — из Meta docs 2025–2026 (v26.0, Access Levels Jun 2026, tokens Jun 2026, ToS Feb 2025).

**Рекомендация [established + emerging]:** Идти по Graph API пути (Dev Mode / Standard Access + self as Admin). Это единственный легальный и относительно устойчивый способ. Настроить polling раз в сутки + опциональные webhooks. Раз в ~50–55 дней вручную (или через простой script с browser login) обновлять token. Браузерный путь оставить только как временный fallback и минимизировать (высокий ToS-риск). Content Library / DYI — не для ежедневной приватной автоматизации.

Если эксперимент в Graph Explorer покажет пустые/неполные данные — сообщить, пересмотрим (возможны дополнительные миграции или скрытые ограничения после 2018). Источники: все указанные выше официальные страницы developers.facebook.com (открыты и процитированы), Transparency Center (Content Library), ToS/Automated Data Collection.

---
518 sources

⚠️ Замечание сборщика: Grok Heavy отдал источники агрегированно («Developers.facebook +1», «Transparency.meta») без инлайн-URL — известная повадка рельсы (поле 28.07: 485 sources и ноль инлайн-ссылок). Конкретные URL и даты требуют сверки по другим рельсам.
