# DR26-10-18-HUB-02-0542 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: claude.ai Research (Opus 5.5 Max, org bb claude_max)
chat: https://приватный чат
dr_id: DR26-10-18-HUB-02-0542
collected: 2026-10-18 07:05 Lisbon
---

# Фабрика публикации подкастов NotebookLM (Gemini Notebook): кто уже это построил, куда выкладывать, как не попасть под бан. Состояние на 18 октября 2026

Технически 35 эпизодов в день реально выложить уже сейчас. YouTube Data API с 1 июня 2026 года выделяет на загрузки отдельную корзину на 100 вызовов `videos.insert` в сутки на проект. RSS-хостинги с безлимитными загрузками закрывают Spotify, Apple, Amazon, Яндекс Музыку и YouTube Music. Узких мест два. Первое — генерация в NotebookLM: официального API для обычных аккаунтов нет, а лимиты зависят от тарифа. Второе, главное, — политика YouTube против «массового/повторяющегося» контента. Поэтому рекомендую не лить все 35 эпизодов в день на один YouTube-канал. Нужна многоярусная схема: всё уходит в RSS и Telegram, а на YouTube попадают только отобранные и визуально проработанные эпизоды, раздельно по RU и EN.

## TL;DR

- **Техника уже не ограничивает.** `videos.insert` теперь стоит 1 единицу в отдельной корзине «Video Uploads» с лимитом 100 вызовов в день. Это по официальной документации Google на октябрь 2026; цифра 1600 units устарела ещё 4 декабря 2025 года. RSS-хостинги (RSS.com, Transistor, Captivate) без лимита на загрузки покрывают аудиоплатформы. Telegram Bot API принимает до 50 МБ, а через локальный Bot API-сервер — до 2000 МБ. Вынимать аудио из NotebookLM придётся неофициальным `notebooklm-py` или браузерной автоматизацией. Официальный Podcast API в Google Cloud объявлен устаревшим 20 мая 2026 года и новых клиентов не принимает. [established]
- **Главный риск — политика, а не квоты.** С 15 июля 2025 года YouTube называет массовый шаблонный контент «inauthentic content» и лишает такие каналы монетизации. 35 одинаково оформленных видео в день с одними и теми же двумя AI-голосами — ровно тот профиль, который ловит канальная проверка. Spotify с 19 мая 2026 года удаляет шоу, выдающие себя за чужих ведущих, но обязательной AI-метки для подкастов я не нашёл. Статья 50 EU AI Act действует с 2 августа 2026 года. Обязанность публикующего (deployer) раскрывать ИИ относится к дипфейкам, а безымянные голоса NotebookLM, скорее всего, под это определение не подпадают. Добровольная маркировка всё равно рекомендуется. [emerging]
- **Рекомендация.** MVP за неделю: `notebooklm-py` → faster-whisper на RTX 5060 Ti → LLM пишет описание, главы, Q&A и транскрипт → ffmpeg собирает обложку, волну и караоке-субтитры → RSS.com/Transistor (все эпизоды) + Telegram (все) + YouTube (3–5 лучших эпизодов в день на язык, отдельные RU и EN каналы) + сайт-зеркало с транскриптами и schema.org. Целевой вариант — синхронизированные слайды или NotebookLM Video Overview для флагманских эпизодов. Ориентир расходов: около $50–100 на бэклог и около $250–500 в месяц на устойчивые 35 эпизодов в день, без аватаров и AI-видео. [emerging; ценовые оценки — speculative]

## Key Findings

1. **Цифра 1600 units опровергнута.** [established] Официальная справка Google по `videos.insert` говорит дословно: «Quota impact: 100 calls per day. A call to this method has a quota cost of 1 unit in the Video Uploads quota bucket». Предыстория подтверждается первоисточником — Revision History YouTube Data API: запись от 4 декабря 2025 года фиксирует «a change in the quota cost of a video upload from approximately 1600 units to approximately 100 units», а запись от 1 июня 2026 года — «transitioning to a granular quota system… starting with videos.insert and search.list» (отдельные корзины по 100 вызовов для `videos.insert` и `search.list` плюс 10 000 единиц на остальные методы). Многие гайды 2026 года до сих пор пишут по-старому: OutlierKit — «uploading a video costs about 1,600», Growati — «videos.insert costs 1600 units, which caps a default project at six uploads per day», ChannelCrawler — «videos.insert | 1,600 units». Это устаревшие данные.

2. **35 загрузок в день помещаются в один проект без расширения квоты.** [established] Но есть три оговорки.
   - Проекты, созданные после 28 июля 2020 года и не прошедшие compliance-аудит, исторически могли загружать только приватные видео. В октябре 2026 года источники расходятся: по Blotato (2026), Revision History это ограничение подтверждает, а справка по загрузке на 8 октября 2026 года — нет. [single-source по конфликту] Проверить тестовой загрузкой.
   - YouTube API Services Developer Policies (developers.google.com) требуют: «you must create exactly one (1) API Project for that API Client». Дробить квоту на несколько GCP-проектов запрещено; единственное исключение там же — отдельный «Upload Project» с префиксом mfk110 для загрузки своих видео на свои каналы.
   - Лимит загрузок на канал в сутки YouTube не публикует; справка об ошибках загрузки говорит только, что он зависит от страны и истории канала (по пересказу SocialRails, 2026). Цифры «10–15 / 50–100 / 2000 в день» на сторонних блогах ничем не подтверждены. [low-authority]

3. **Официального API NotebookLM для обычного аккаунта нет.** [established]
   - Google Cloud документирует API Gemini Notebook Enterprise: блокноты, источники, `notebooks.audioOverviews.create`. Статус Preview, нужны лицензии Gemini Enterprise. По словам участника форума Google AI Developers, подписка Enterprise содержит минимум 15 лицензий. [single-source]
   - Отдельный Podcast API (текст на вход, MP3 на выход) в release notes Gemini Enterprise от 20 мая 2026 года помечен Deprecated: «Google isn't allowlisting new customers».
   - Практики пользуются `teng-lin/notebooklm-py` — неофициальным Python API и CLI на реверс-инжиниринге (PyPI v0.8.4) с командами `generate audio` и `download audio/video/cinematic-video/slide-deck`. README предупреждает: «Unofficial Library — Use at Your Own Risk».

4. **NotebookLM переименован, лимиты поменялись.** [emerging]
   - 16 июля 2026 года продукт стал Gemini Notebook.
   - До 2 сентября 2026 года действовали фиксированные лимиты Audio Overview: 3 в день на Standard, 6 на Plus, 20 на Pro, 100 на Ultra 20 TB и 200 на Ultra 30 TB (сводки fast.io, notebooklm-guide.com со ссылкой на справку Google).
   - Со 2 сентября 2026 года действует вычислительный бюджет с окнами по 5 часов и недельным потолком: Plus 2×, Pro 4×, Ultra 5× или 20× от бюджета Pro (elephas.app, kortex-notebooklm.com).
   - Источники противоречат друг другу: часть сайтов до сих пор приводит фиксированные «3/20/100 в день». [conflict, low-authority] Для 35 эпизодов в день Pro, по всей видимости, не хватит; практичный вариант — Ultra. Замерить на своём аккаунте за неделю.

5. **Video Overview можно скачать в MP4, но с аудио Audio Overview он не синхронизируется.** [established]
   - Справка Gemini Notebook описывает форматы Explainer, Brief/Short и Cinematic; Cinematic — только английский и только 18+.
   - Video Overview — отдельный артефакт со своим закадровым голосом, а не видеоряд к готовому Audio Overview.
   - Cinematic запущен 4 марта 2026 года на базе Veo 3 и Nano Banana Pro; Short (60-секундная вертикаль) — 30 июня 2026 года (хронология Jellypod).
   - По данным сторонних сервисов удаления водяных знаков, в конце ролика есть заставка «Made with Google». [low-authority] Удалять её не рекомендую: это противоречит духу статьи 50 AI Act и политики раскрытия.

6. **YouTube — платформа №1 для подкастов, но по-разному в зависимости от метрики.** [established] Цифры — в разделе 2.3. Обратная сторона: статичная картинка на YouTube удерживает зрителя плохо.

7. **Ретеншн-данных по визуальным форматам мало.** [emerging, данные тонкие]
   - Единственный открытый замер статичной картинки старый. Дэниел Дж. Льюис (The Audacity to Podcast, 2017, страница обновлена в 2024): «Only a 3.4% average view duration. 90% of the initially 23,000 viewers were gone within 90 seconds». [single-source, устарело]
   - Свежий бенчмарк без разбивки по форматам: отчёт Podcast Marketing Academy × Lower Street 2025 (311 подкастеров) — медианное время просмотра подкаста на YouTube 7,5 минуты при 24 минутах в аудио, средний CTR 2,25%.
   - Опрос Sounds Profitable (через Signal Hill): только 7% предпочитают статичную картинку, 70% — ведущих в студии.
   - Сравнительных замеров удержания для слайдов, AI-аватаров и AI-b-roll в открытых источниках нет. [no source found]

## Details

### 1. Кто уже автоматизировал NotebookLM → публикацию

**Таблица кейсов «кто сделал → как → результат → источник»**

| Кто | Как | Результат / цифры | Источник, дата | Статус |
|---|---|---|---|---|
| teng-lin, `notebooklm-py` | Неофициальный Python API и CLI; генерация и скачивание аудио, видео, слайдов, инфографики; интеграция с Claude Code/Codex | v0.8.4 на PyPI; продвигался аккаунтом GitHub Projects в X (2026) | GitHub teng-lin/notebooklm-py; PyPI | Проверено (README) [established как инструмент] |
| ishandutta2007, `notebooklm-api` | Обёртка: CLI + MCP-сервер, экспорт MP3/M4A | Цифр нет | GitHub, 2026 | [single-source] |
| upamune, `notebooklm-podcast-automator` | Playwright + Chrome DevTools Protocol: блокнот, URL-источники, Audio Overview, **загрузка в Spotify** через браузер | Цифр нет | GitHub (дата не указана) | [single-source]; хрупко |
| useapi.net, шаблон n8n | Форма → блокнот → Audio Overview → скачивание .m4a через платный сторонний API на вашем Google-аккаунте | «3 links in, a 3.1 MB .m4a out» | PR #225 в awesome-n8n-templates, 30 сен 2026 | [single-source]; посредник получает доступ к аккаунту |
| Абишек Лакандри, блог | n8n + `notebooklm-py`: подкасты EN и непальский, Video Overview, метаданные от локальной LLM, автозагрузка на YouTube | Цифр просмотров нет | abisheklakandri.com (2026) | [low-authority, single-source] |
| n8n workflow #12020 | Google Drive → транскрипция OpenAI → show notes → обложка → Buzzsprout; YouTube «static background + audio» как расширение | Цифр нет | n8n.io | Шаблон, не кейс канала |
| AutoContent API | Сторонний REST-сервис «в стиле NotebookLM», не Google | Нет | autocontentapi.com, 9 авг 2026 | [low-authority] |
| Medium, «How I Cloned a YouTube Channel Using NotebookLM» | NotebookLM для разбора чужого канала и сценариев | Канал-образец: 8 видео, 2,1 тыс. подписчиков, «$600/month» | vencoding.medium.com | [low-authority] |
| Дэнн Смигрод (WGAN Forum) | NotebookLM → хостинг → «nearly 20 podcast apps» + YouTube «менее чем за 30 минут» | Ручной процесс | SlideShare, конец 2024 | Устарело (2024) |

**Чего не нашлось.** Публичного канала, целиком сделанного на NotebookLM, с проверяемой статистикой роста (подписчики, просмотры, эпизоды в день) я не нашёл. [no source found] Документально подтверждённых банов аккаунтов Google за `notebooklm-py` тоже нет. [no source found] Риск при этом реален: неофициальные эндпоинты могут сломаться или привести к блокировке в любой момент. [speculative]

**Как вынимают аудио, по надёжности:** (1) вручную через Download в Studio — формат по одним гайдам WAV, по `notebooklm-py` M4A/MP3 [conflict]; (2) `notebooklm download audio`; (3) Playwright; (4) Enterprise API — только для организаций с лицензиями, Preview; (5) Podcast API — закрыт для новых клиентов с 20 мая 2026 года.

**Условия коммерческого использования вывода NotebookLM и наличие SynthID в Audio Overview** по первоисточнику проверить не удалось. [no source found] Справка Google предупреждает только: «Audio Overviews, including voices, are AI-generated and may contain inaccuracies or audio glitches». До запуска на 1000+ эпизодов в месяц прочитайте Generative AI Additional Terms для своего тарифа.

### 2. Аудиоплатформы: куда класть

**2.1. Spotify for Creators.**
- Публичного API для загрузки эпизодов я не нашёл. [no source found]
- Два официальных пути: хостить на Spotify for Creators и загружать аудио или видео вручную (или через Riverside), либо подать внешнюю RSS-ленту, из которой Spotify подтягивает эпизоды сам (справка Spotify, 2026).
- Spotify for Creators называет себя «the only hosting platform that supports both formats»: видео-подкаст в Spotify требует хостинга именно там.
- Вывод: Spotify подключается через RSS внешнего хостинга; видео-версия выпадает из автоматики, разве что через браузерный скрипт вроде upamune (хрупко, вероятно нарушение ToS). [speculative]

**2.2. RSS-хостинги.** 35 эпизодов × ~20 минут ≈ 11,7 часа аудио в день, около 350 часов в месяц. Хостинги с почасовыми лимитами (Buzzsprout: Audio $15 в месяц при годовой оплате, 6 часов загрузки в месяц на плане за $19, по Podcast Host Lab, 2026) исключены.

**2.3. Где слушают в 2026 году** [established для США; для RU — low-authority]
- Edison Research at SSRS, «The Podcast Consumer 2026» (вышел 4 июня 2026, данные Edison Podcast Metrics за I квартал 2026): YouTube — самая используемая платформа у 37% еженедельных потребителей (31% → 36% → 37% в 2024/2025/I кв. 2026), Spotify 28% → 22% → 21%, Apple 14% → 13% → 12%; по словам Меган Лазовик, «YouTube is the service used most for podcasts in 2026 among weekly consumers».
- Edison Infinite Dial 2026, доля времени в IV квартале 2025: YouTube 32%, Spotify 25%, Apple около 20%.
- Signal Hill / Cumulus, апрель 2026 (n=600): YouTube 44%, Spotify 14%, Apple 5%; Westwood One доверяет выборкам Edison и Triton больше.
- Edison Podcast Metrics, III квартал 2026: Spotify 17%, Apple 9%, YouTube первый.
- Великобритания: YouTube 29%, Spotify 28%, BBC Sounds 15% (Edison UK).
- Россия: аудитория подкастов 20+ млн, ведущие площадки — Яндекс Музыка (68%) и VK (54%); студийный блог video-podcast.ru без методики. [low-authority]

**Таблица платформ «платформа → API/RSS → автоматизация → аудитория → цена»**

| Платформа | API / RSS | Автоматизация | Аудитория / роль | Цена |
|---|---|---|---|---|
| YouTube (видео) | Data API v3, `videos.insert` | Полная; 100 загрузок в день на проект | №1 в США (37–44% «use most»); 47% начинают новый подкаст на YouTube (Edison через Riverside) | Бесплатно |
| YouTube Podcasts / Music через RSS | Studio: Create → New podcast → Submit RSS | Автозагрузка эпизодов как видео со статичной обложкой; не во всех странах; нужны «advanced features»; заменить аудио опубликованного видео нельзя | Присутствие в YouTube Music | Бесплатно |
| Spotify | RSS или ручная загрузка в Spotify for Creators | Через RSS — полная; видео — вручную | №2 по времени слушания | Бесплатно |
| Apple Podcasts | RSS | Полная | Лидер чистого аудио: Apple 39%, Spotify 28% для audio-only (Buzzsprout через Riverside, июль 2026) [single-source] | Бесплатно |
| Amazon Music, Pocket Casts, Deezer, iHeart, Podcast Index | RSS (автодистрибуция хостингов) | Полная | Нишевая | Бесплатно |
| RSS.com | API на тарифе Max; безлимит эпизодов | Высокая; «аудио → видео для YouTube» на платных тарифах | — | Free; Plus $15/мес (год); Max $37/мес с API и видео (SpotSaaS, 2026) |
| Transistor | API, безлимит шоу и загрузок (1000 МБ на файл) | Высокая | — | $19 / $49 / $99 в месяц |
| Captivate | Безлимит загрузок, лимит по скачиваниям | Высокая | — | От $17–19 в месяц (до 30 000 скачиваний) |
| Buzzsprout | Лимит часов загрузки | Не подходит для 35 в день | — | Audio $15–18; Audio+Video $25–30 |
| Libsyn | RSS, видео на всех планах | Средняя | — | От $5; с видео от $12 |
| Яндекс Музыка | Только RSS через форму; модерация «в течение нескольких рабочих дней»; MP3/M4A/AAC/FLAC/MP4, от 40 кбит/с (рекомендовано 192); соответствие законодательству РФ | После одобрения подтягивает RSS сам | Крупнейшая RU-площадка [low-authority по доле] | Бесплатно |
| VK Подкасты / VK Видео | Через сообщество VK; RSS-импорт по гайдам | Частичная [single-source] | Молодая RU-аудитория [low-authority] | Бесплатно |
| Звук (Студио) | Свой хостинг с дистрибуцией в Яндекс Музыку | Средняя | RU | [no source found] |
| Telegram | Bot API: `sendAudio`/`sendVideo` до 50 МБ; через URL до 20 МБ; локальный Bot API-сервер до 2000 МБ (core.telegram.org) | Полная | Ваша крупная аудитория — самый дешёвый гарантированный охват | Бесплатно |
| X | Pay-per-use API с 6 февраля 2026 | Полная | Анонсы, не прослушивание | $0,015 за пост; $0,20 со ссылкой (с 20 апреля 2026) |

**Telegram.** 30-минутный MP3 на 128 кбит/с ≈ 29 МБ — влезает в 50 МБ публичного Bot API. Видео 1080p на 15–30 минут не влезет без сильного сжатия; нужен локальный `telegram-bot-api --local` (Docker), он поднимает потолок до 2 ГБ. Рекомендация: аудио (`sendAudio` с обложкой и главами в подписи) плюс вертикальный клип, полное видео не нужно.

**X.** Видео в посте: до 20 минут и 8 ГБ на обычном аккаунте, до 125 минут и 16 ГБ на Premium (outstand.so, 2026; перепроверить на docs.x.com). [single-source] Пост со ссылкой в 13 раз дороже: 35 постов в день со ссылкой ≈ $210 в месяц, без ссылки с нативным клипом ≈ $16 в месяц плюс медиа (около $0,005 за запрос). Вывод: нативные 60–140-секундные клипы без URL, ссылка — в профиле или закрепе.

**RU и EN: одно шоу или два?** Рекомендую два шоу: две RSS-ленты и два YouTube-канала. [emerging] Тег `<language>` в RSS один на ленту, рекомендации платформ строятся по языку, смешанный канал портит сигналы YouTube, а у Яндекса RU-лента модерируется отдельно. Прямых рекомендаций платформ о двуязычных шоу я не нашёл [no source found]; вывод — по устройству RSS и логике рекомендательных систем. [speculative]

### 3. Визуальный ряд для YouTube

**Таблица визуальных подходов «подход → стоимость/эпизод → время → удержание → риск спама → пример»**

| Подход | Стоимость на эпизод (15–30 мин) | Время | Удержание (данные) | Риск «inauthentic» | Пример / примечание |
|---|---|---|---|---|---|
| (a) Обложка + волна (ffmpeg `showwaves`, Remotion, Headliner) | ≈ $0 локально; Headliner [no source found] | Минуты с NVENC [speculative] | Худшее из измеренного: 3,4% AVD, 90% ушли за 90 секунд (Льюис, 2017) [single-source, устарело]; статику предпочитают 7% (Sounds Profitable) | Высокий при 35 в день: один шаблон, «little variation» | NPR, Slate, NYT в 2023: статика 75–1000 просмотров, у NPR с видеорядом ~3000 (Bloomberg через Tubefilter) |
| (b) NotebookLM Video Overview / Slide Deck | Входит в подписку | Минуты; квота общая с аудио | [no source found] | Средний: шаблонный стиль, «Made with Google» | С Audio Overview не синхронизируется; публиковать как отдельное короткое видео; Slide Deck (PDF/PPTX) можно наложить на аудио по таймкодам (c) |
| (c) Слайды по транскрипту с таймкодами (Marp/Remotion + ffmpeg, Google Slides API, Gamma) | ~$0,05–0,30 на LLM [speculative] | 5–15 мин рендера [speculative] | [no source found] | Ниже, если слайды содержательные | Лучший баланс для образовательного контента [speculative] |
| (d) AI-картинки и видео по смыслу (Flux + Ken Burns; Veo, Kling, Runway, Higgsfield) | Картинки локально ≈ $0; облачное видео дорого [no source found] | Flux на 16 ГБ — секунды на кадр [speculative] | [no source found] | Средний–высокий: «AI slop» | Картинка раз в 20–40 с разумна; облачное видео при 1050 эпизодах в месяц не окупается [speculative] |
| (e) Аватары ведущих (HeyGen, Hedra, Synthesia) + диаризация | На порядки выше бюджета [no source found] | Облачный рендер | [no source found] | Высокий + возможная обязательная AI-метка | Не рекомендую для массового потока |
| (f) Караоке-субтитры, главы, автошорты (Opus Clip, Vizard, Descript или локально) | ≈ $0 локально | Минуты | Цифр эффекта не нашёл [no source found] | Низкий | Обязательный слой для (a)–(e) |

**Интерпретация.** Данные слабые, но сходятся: статика на YouTube — это «склад», а не продукт. Отчёт Lower Street 2025: шоу, выкладывающие полные аудиоэпизоды почти без монтажа, «often see lower engagement». По наблюдению Адриана Нга (Backbeat Recording Studio, 21 июля 2026), на канале NPR эпизоды, сделанные с прицелом на видео, набирают около 20 000 просмотров на старте против около 2000 у статичных; это не официальная цифра NPR. [single-source] Отсюда схема: дешёвая статика + субтитры + главы для архива, содержательный визуал (c) для отобранных эпизодов. Аватары и AI-видео для 35 эпизодов в день — дорогой способ получить ярлык «AI slop». [speculative] Вопрос закроет собственный A/B-тест: 10 эпизодов (a) против 10 эпизодов (c) на одном канале, сравнить AVD и удержание на 30-й секунде через YouTube Analytics API.

### 4. Тексты: описание, Q&A и GEO

**Что известно.** Самая цитируемая работа — «GEO: Generative Engine Optimization» (Aggarwal, Murahari, Rajpurohit, Kalyan, Narasimhan, Deshpande; Принстон, arXiv:2311.09735, v3 от 28 июня 2024; KDD 2024): добавление цитат, статистики и ссылок на источники повышало видимость в ответах генеративных движков; аннотация формулирует это так: «GEO can boost visibility by up to 40% in generative engine responses». Прямых замеров того, что Q&A-формат или полные транскрипты подкастов повышают цитирование в ChatGPT, Perplexity или AI Overviews, я не нашёл. [no source found]

**Практическая логика.** [speculative, низкорисковая] Генеративный поиск цитирует текст, а не аудио. Каждому эпизоду нужна индексируемая HTML-страница: (1) ответ в первом абзаце; (2) 5–8 пар «вопрос → ответ» с фактами и цифрами; (3) главы с таймкодами, те же — в описании YouTube; (4) полный транскрипт с метками спикеров; (5) schema.org `PodcastEpisode` + `PodcastSeries`, `FAQPage` — только для реальных Q&A; (6) ссылки на первоисточники эпизода — ваше главное отличие от «AI slop».

**Подпись и раскрытие ИИ.** Формулировка «Ведущие — AI-голоса Google NotebookLM; исследование, материалы и редактура — Антон и его AI-кофаундер» честна, отвечает духу «human editorial control» из Кодекса практики ЕС и даёт YouTube аргумент «оригинальный вклад». Данных о реакции аудитории на такие подписи я не нашёл. [no source found] Фон тревожный: Bloomberg (30 апреля 2026) по данным Podcast Index насчитал 39% AI-шоу среди новых лент — 4 243 из 10 871 за девять дней (в пересказе Futurism), а более поздний снимок New Feeds Report от Podcast Index, приведённый Gizmodo, дал 35,4% (485 AI-лент в сутки), так что прозрачность становится конкурентным преимуществом. [emerging]

### 5. Политики, раскрытие и юрисдикция ЕС

**YouTube, монетизация.** С 15 июля 2025 года «repetitious content» переименован в «inauthentic content»: массовый или повторяющийся контент, «template-based with little to no variation across videos». YouTube подчёркивает, что «channels that use AI in their content remain eligible for monetization» (Рене Ричи через Social Media Today, 13 июля 2025). Отраслевые сводки 2026 года утверждают, что проверка стала канальной, а не повидео. [low-authority, single-source]

**YouTube, раскрытие** (справка «Disclosing use of GenAI content», в Studio — настройка «AI use»):
- Раскрывать обязательно реалистичный AI-контент: реальный человек говорит то, чего не говорил; реалистичная сцена, которой не было; AI-музыка как главный фокус.
- Клонирование собственного голоса и помощь ИИ со сценарием или обложкой раскрытия не требуют.
- Generic AI-голоса, не имитирующие реальных людей, прямо не упомянуты; по буквальному чтению раскрытие, вероятно, не обязательно. [emerging, интерпретация] Но YouTube может поставить метку сам — по C2PA или своему детектору. AI-джингл в начале эпизода раскрытия требует.
- Раскрытие, по словам YouTube, «won't limit a video's audience or impact its eligibility to earn money». Рекомендация: ставить «AI use» на все эпизоды — ноль затрат, защита от санкций за систематическое нераскрытие.

**Spotify.** 19 мая 2026 года Spotify объявил, что будет удалять подкасты, выдающие себя за чужого ведущего, «whether that's using AI voice cloning or any other method», и ввёл бейдж «Verified by Spotify». Обязательной AI-маркировки подкастов не нашлось. [established по impersonation; no source found по обязательному раскрытию] Пакет мер против AI-спама от 25 сентября 2025 года (фильтры, AI-кредиты по DDEX) относится к музыке.

**EU AI Act, статья 50.** [established по срокам; применимость — emerging]
- Применяется с 2 августа 2026 года. Финальный Code of Practice on Transparency of AI-generated Content опубликован 10 июня 2026 года; Еврокомиссия и AI Board признали его адекватным добровольным инструментом.
- Провайдер (Google) обязан машиночитаемо маркировать вывод (ст. 50(2)); для систем, выпущенных до 2 августа 2026 года, — до 2 декабря 2026 года.
- Публикующий (вы — deployer) обязан раскрывать дипфейки (ст. 50(4)): контент, который «resembles existing persons, objects, places, entities or events and would falsely appear authentic» (ст. 3(60)). Безымянные AI-ведущие NotebookLM, не изображающие реальных людей, по буквальному чтению дипфейком не являются.
- Норма 50(4) об AI-тексте по вопросам публичного интереса снимается при человеческой редакторской проверке — держите её задокументированной.
- Кодекс предлагает три EU-иконки для раскрытия; для аудио Orrick (август 2026) ссылается на §110 руководства Еврокомиссии: раскрытие должно быть «perceivable… including through visible or audible labels».
- Вывод: юридической обязанности, скорее всего, нет, но фраза в начале эпизода и иконка в описании стоят ноль и закрывают риск широкого прочтения. [emerging]

### 6. Конвейер на 35 эпизодов в день

**SaaS** (Castmagic, Podsqueeze, Swell AI, Repurpose.io, Descript, Auphonic): актуальные цены и лимиты API на 2026 год я не проверял. [no source found] При 1050 эпизодах в месяц поминутная тарификация SaaS-транскрайбера дороже локального faster-whisper, поэтому транскрипцию, диаризацию и рендер логично держать локально, а покупать LLM-подписки и хостинг. [speculative]

**RTX 5060 Ti (8 или 16 ГБ).** Опубликованных бенчмарков этой карты на этих задачах я не нашёл; все цифры — оценки. [speculative]

| Задача | 8 ГБ | 16 ГБ | Оценка на 20-минутный эпизод |
|---|---|---|---|
| faster-whisper large-v3 | Да (int8) | Да (fp16) | ~1–3 мин |
| Диаризация pyannote | Да | Да | ~1–3 мин |
| Локальная LLM (квантованная) | 7–8B | до ~14B | ~1–2 мин; надёжнее облачные подписки |
| Flux / SDXL | SDXL — да; Flux — тесно | Flux — комфортно | ~5–30 с на кадр |
| Локальные видеомодели (Wan, LTX) | Практически нет | Короткие клипы, медленно | Непрактично для 35 в день |
| ffmpeg NVENC (статика + волна + субтитры) | Да | Да | ~1–5 мин |
| Remotion (слайды) | Зависит от CPU | — | ~5–15 мин |

Итог: 35 эпизодов × ~10 минут ≈ 6 часов в сутки — одна карта справляется, если исключить AI-видео. [speculative]

**Месячный бюджет** (хостинг — по сводкам 2026 года, остальное [speculative]):

| Статья | Бэклог ~35 эпизодов (разово) | 35 в день (~1050 в месяц) |
|---|---|---|
| NotebookLM / Gemini (Pro $19,99 или Ultra $99,99) | $20 | $100–200 (Ultra; 1–2 аккаунта — проверить ToS) |
| Хостинг RSS (2 шоу) | $0–37 | $37–99 |
| LLM-подписки и API для текстов | $20–40 | $40–150 |
| X API (без ссылок) | ~$1 | ~$16–25 |
| Telegram, YouTube API | $0 | $0 (VPS для локального Bot API ~$5–10) |
| Электричество GPU (ЕС) | <$5 | ~$15–30 |
| **Итого** | **≈ $50–100** | **≈ $250–500** |

Аватары или облачное AI-видео добавят сотни и тысячи долларов в месяц. [speculative]

## Recommendations

### MVP за неделю

1. **День 1.** Два шоу на RSS.com (Max, ради API и видео) или Transistor: RU и EN. Подать RSS в Spotify, Apple, Amazon и Яндекс Музыку. Создать отдельные RU- и EN-каналы на YouTube. GCP-проект с YouTube Data API, тестовая загрузка (проверить, не уходит ли видео в private), подать форму Audit and Quota Extension.
2. **Дни 2–3.** Скрипт `notebooklm-py` для пакетного скачивания готовых Audio Overview. Генерацию оставить ручной или полуручной: промпт — ваш «оригинальный вклад».
3. **Дни 3–4.** Локальный конвейер: faster-whisper + pyannote → JSON с репликами; LLM → заголовок, BLUF-описание, 5–8 Q&A, главы, теги, текст раскрытия; ffmpeg NVENC → 16:9 (обложка + волна + караоке-субтитры ASS) и 9:16 клип на 60–90 секунд.
4. **День 5.** Публикация: хостинг (все эпизоды); Telegram `sendAudio` + клип через локальный Bot API (все); YouTube `videos.insert` + главы + «AI use» (3–5 лучших в день на язык); X — нативный клип без ссылки.
5. **Дни 6–7.** Статический сайт-зеркало (Astro/Hugo) со страницей на эпизод: транскрипт, Q&A, `PodcastEpisode` JSON-LD. В n8n (self-hosted) — расписание и ретраи.

### Целевой конвейер (1–2 месяца)

- Слайды по транскрипту (Marp/Remotion) с таймкодами из Whisper для YouTube-эпизодов; для флагманов — Slide Deck из NotebookLM, наложенный на аудио.
- NotebookLM Video Overview Short как самостоятельный Shorts-контент (с честным «Made with Google»).
- A/B-тест (a) против (c) по AVD.
- Перевод отобранных эпизодов RU↔EN повторной генерацией в NotebookLM на другом языке, а не дубляжом.

### Варианты для решения

- **Вариант A — «Всё везде, 35 в день на YouTube».** Плюсы: максимальный охват, простая автоматика. Минусы: высокий риск inauthentic/YPP-санкций, низкое удержание статики, размывание канала. Не рекомендую.
- **Вариант B — «Ярусы» (рекомендуемый).** RSS + Telegram + сайт для всех эпизодов, YouTube только для отобранных с содержательным визуалом, X — клипы. Плюсы: полный охват аудиоплатформ, YouTube выглядит как редакционный продукт. Минусы: ежедневный отбор (~15 минут).
- **Вариант C — «Только Enterprise API».** Плюсы: нет риска ToS. Минусы: Preview-статус, лицензии Enterprise (по словам участника форума, от 15 [single-source]), непропорционально для одного оператора.

**Итоговая рекомендация: вариант B.** Уверенность [emerging]. Ключевое допущение: YouTube оценивает канал целиком, поэтому меньший, разнообразный и прозрачно подписанный поток безопаснее и лучше для удержания, чем максимальный объём. Если тест (a) против (c) не покажет разницы в AVD, а YouTube не применит санкций к аудио-архивам, долю YouTube-публикаций можно постепенно увеличивать.

## Caveats

**Мифы и опровержения:**
- «Загрузка стоит 1600 units, значит максимум 6 в день» — устарело с 4 декабря 2025 года; сейчас 1 единица в корзине на 100 загрузок в день. [established]
- «Можно открыть 10 GCP-проектов и умножить квоту» — нарушение Developer Policies, риск отзыва доступа. [established по нескольким вторичным источникам]
- «У NotebookLM есть публичный API» — нет: только Enterprise (Preview), Podcast API закрыт для новых клиентов. [established]
- «Video Overview можно наложить на аудио Audio Overview» — нет, это разные артефакты с разной озвучкой. [established]
- «YouTube запретил AI-каналы» — нет; запрещена монетизация массового шаблонного контента. [established]
- «EU AI Act обязывает маркировать любой AI-подкаст» — по буквальному тексту обязанность deployer касается дипфейков и текста публичного интереса без редактуры; практика регуляторов ещё не сложилась. [emerging]

**Ограничения исследования:**
- Цифры по квотам YouTube и лимитам NotebookLM в основном из вторичных сводок 2026 года; из первичных подтверждена справка `videos.insert`.
- Ретеншн-данные по форматам устарели (2017) или агрегированы.
- Не проверены: цены аватаров, AI-видео и SaaS, бенчмарки RTX 5060 Ti, коммерческие условия NotebookLM, SynthID в аудио. [no source found]
- Доли российских площадок — из маркетинговых блогов. [low-authority]
- Вопросы закроют: (1) 2-недельный A/B-тест форматов на YouTube; (2) недельный замер бюджета Audio Overview на Pro и Ultra; (3) тестовая загрузка через API на private-ограничение.

## Протокол исследования

Поисковые запросы: «YouTube Data API videos.insert quota cost units 2026»; «notebooklm-py unofficial API download audio overview»; «NotebookLM Enterprise Podcast API Google Cloud»; «YouTube inauthentic content policy mass-produced repetitive monetization July 2025»; «NotebookLM renamed Gemini Notebook audio overview limits Pro Ultra»; «Spotify for Creators API upload episodes automation RSS 2026»; «YouTube podcasts RSS feed ingestion audio uploads static image 2026»; «EU AI Act Article 50 deployers deep fake audio disclosure August 2026 code of practice»; «n8n workflow NotebookLM podcast auto publish YouTube»; «YouTube channel entirely NotebookLM podcasts subscribers views reddit»; «NotebookLM Video Overview download mp4 styles cinematic languages 2026»; «YouTube daily upload limit per channel verified account 2026»; «YouTube API quota extension audit form compliance multiple projects»; «podcast hosting API upload episodes Transistor Buzzsprout Captivate RSS.com pricing comparison 2026»; «X API pricing pay-per-use 2026 post video upload cost»; «Edison podcast consumer 2026 YouTube Spotify Apple most used platform share»; «Telegram Bot API file upload limit 50 MB local Bot API server 2000 MB sendAudio»; «Яндекс Музыка подкасты добавить RSS VK подкасты Звук 2026 аудитория». Запрос о раскрытии AI на YouTube не выполнился из-за лимита поиска; эту тему и удержание закрывал отдельный исследовательский агент.

Открытые страницы: официальная справка Google «Videos: insert | YouTube Data API» (Revision History открыть не удалось — отклонено инструментом). Агент открыл справку YouTube «Disclosing use of GenAI content», пост Spotify Newsroom от 19 мая 2026 года, The Audacity to Podcast (эпизод 332) и материал Backbeat Recording Studio. Остальное — из поисковых выдержек перечисленных в тексте источников.

---
## Сопроводительный текст Claude

Запускаю исследование по всем пяти блокам. Счёт везде будет в двух режимах: разовая выкладка текущих ~35 эпизодов и устойчивые 35 эпизодов в день. В бриф я добавил три пункта: русскоязычные площадки (Яндекс Музыка, VK, Звук, Telegram), требования EU AI Act к маркировке синтетического контента и сверку актуальной квоты YouTube API по официальной документации. Отчёт будет на русском, с источниками, датами и метками уверенности.

Я подготовил отчёт о фабрике публикации подкастов из NotebookLM (Gemini Notebook). Технически 35 эпизодов в день выложить можно уже сейчас. Главный риск не в квотах, а в политике YouTube против массового однотипного контента. Поэтому я рекомендую вариант B, многоярусную схему:
- **Все эпизоды** уходят на RSS-хостинг, откуда их забирают Spotify, Apple, Amazon и Яндекс Музыка. Они же публикуются в Telegram и на сайте-зеркале с транскриптами.
- **На YouTube** — только 3–5 отобранных эпизодов в день на язык, на отдельных каналах для RU и EN.
- **В X** — короткие нативные клипы без ссылки.

Уверенность в рекомендации средняя: она держится на допущении, что YouTube оценивает канал целиком.

**Что я выяснил:**
- **Квота YouTube API.** Распространённая цифра «1600 единиц за загрузку» устарела с 4 декабря 2025 года. Сейчас `videos.insert` стоит 1 единицу из отдельного лимита на 100 загрузок в сутки на проект. Это подтверждено официальной справкой Google. Дробить квоту на несколько проектов запрещено правилами для разработчиков. Неясно, загружают ли непроверенные проекты по-прежнему только приватные видео, — это нужно проверить тестовой загрузкой.
- **NotebookLM.** Публичного API для обычного аккаунта нет. Enterprise API пока в Preview и требует лицензий, а отдельный Podcast API с 20 мая 2026 года не принимает новых клиентов. На практике аудио выгружают неофициальной библиотекой `notebooklm-py`, на свой риск.
- **Лимиты генерации.** Со 2 сентября 2026 года фиксированные дневные лимиты заменены вычислительным бюджетом, и источники противоречат друг другу. Pro на 35 эпизодов в день, вероятно, не хватит, скорее понадобится Ultra. Это стоит замерить на своём аккаунте за неделю.
- **Видео не синхронизируется с аудио.** Video Overview — отдельный ролик со своей озвучкой, наложить его на готовый Audio Overview нельзя.
- **Обложка с волной на YouTube удерживает плохо.** Единственный открытый замер — 2017 года: 90% зрителей ушли за 90 секунд. Сравнительных данных по слайдам, аватарам и AI-видео в открытых источниках я не нашёл. Отчёт предлагает собственный A/B-тест: обложка с волной против слайдов по транскрипту.
- **Маркировка ИИ.** По буквальному тексту статьи 50 EU AI Act безымянные голоса NotebookLM, скорее всего, не считаются дипфейком, так что обязательной маркировки, вероятно, нет. Spotify удаляет только шоу, выдающие себя за чужих ведущих. Добровольная пометка «AI use» на YouTube ничего не стоит и не снижает охват, поэтому её стоит ставить.
- **Прецедентов нет.** Публичного канала, полностью сделанного на NotebookLM, с проверяемой статистикой роста я не нашёл.

**Расходы** — грубая оценка: около $50–100 на текущий бэклог и около $250–500 в месяц при 35 эпизодах в день, без аватаров и AI-видео. По моей оценке, одна RTX 5060 Ti потянет транскрипцию и рендер. Опубликованных бенчмарков этой карты я не нашёл.

Ещё в отчёте есть таблицы готовых решений, платформ и форматов видео, план запуска за неделю и список устаревших утверждений, которые до сих пор встречаются в гайдах.

## Источники (md_citations, 86 уникальных из 115)

- https://www.outstand.so/blog/youtube-api-pricing-quota
- https://discuss.ai.google.dev/t/how-to-access-notebooklm-via-api/5084?page=2
- https://www.searchenginejournal.com/youtube-clarifies-monetization-update-targeting-spam-not-reaction-channels/550755/
- https://developers.google.com/youtube/v3/docs/videos/insert
- https://www.postzen.dev/blog/youtube-api-quota-exceeded
- https://www.blotato.com/blog/youtube-api-pricing
- https://socialrails.com/blog/youtube-upload-limits-complete-guide
- https://apexgear.blog/youtube-upload-limits-guide
- https://autocontentapi.com/blog/does-notebooklm-have-an-api
- https://notebookclipper.com/blog/gemini-notebook-api
- https://docs.cloud.google.com/gemini/enterprise/notebooklm-enterprise/docs/podcast-api
- https://docs.cloud.google.com/gemini/enterprise/docs/release-notes
- https://github.com/teng-lin/notebooklm-py
- https://pypi.org/project/notebooklm-py/
- https://geminiomniprompts.org/blog/notebooklm-limits/
- https://www.kortex-notebooklm.com/blog/notebooklm-renamed-gemini-notebook
- https://fast.io/resources/notebooklm-audio-overview-limit/
- https://notebooklm-guide.com/notebooklm-audio-overview-limits/
- https://elephas.app/blog/notebooklm-daily-limit
- https://superlore.ai/blog/notebooklm-limits-workarounds
- https://support.google.com/notebooklm/answer/16454555?hl=en
- https://www.jellypod.com/blog/notebooklm-video-overview-vs-explainer-video
- https://notebooklmremover.org/blog/notebooklm-video-overview-watermark/
- https://theaudacitytopodcast.com/why-you-should-not-publish-audio-podcasts-on-youtube-tap332/
- https://www.podcastnewsdaily.com/news/youtube-rises-instagram-falls-inside-2025-podcast-marketing-trends/article_f423031a-a4c6-4d69-b6dd-8533e2338e12.html
- https://signalhillinsights.com/youtube-and-video-will-grow-podcasting-in-2024/
- https://x.com/GithubProjects/status/2059341323384320131
- https://github.com/ishandutta2007/notebooklm-api
- https://github.com/upamune/notebooklm-podcast-automator
- https://github.com/enescingoz/awesome-n8n-templates/pull/225
- https://www.abisheklakandri.com/blog/n8n-notebooklm-automated-ai-learning-pipeline
- https://n8n.io/workflows/12020-automated-podcast-production-and-publishing-with-openai-airtable-and-buzzsprout/
- https://vencoding.medium.com/how-i-cloned-a-youtube-channel-using-googles-notebooklm-db0d43ee1879
- https://www.slideshare.net/slideshow/notebooklm-podcasting-workflow-publish-to-youtube-and-nearly-20-podcast-apps-in-less-than-30-minutes/274123390
- https://exploreaitogether.com/export-download-notebooklm-guide/
- https://support.google.com/notebooklm/answer/16212820?hl=en
- https://www.lilachbullock.com/how-to-upload-a-podcast-to-spotify/
- https://support.spotify.com/us/creators/article/adding-a-new-show-to-your-spotify-for-creators-account/
- https://creators.spotify.com/switch
- https://podcasthostlab.com/podcast-hosting-price-tracker/
- https://www.podcastinsights.com/podcast-statistics/
- https://marketintelligencetools.com/reports/podcast-industry-statistics/
- https://www.companieshistory.com/podcasting-market-statistics/
- https://talkers.com/2026/10/08/edison-youtube-dominates-as-1-podcast-platform/
- https://www.urbaninsite.com/news/youtube-leads-podcast-platforms/
- https://video-podcast.ru/blog/kak-prodvigat-podkasty-v-rossii
- https://riverside.com/blog/podcast-statistics
- https://support.google.com/youtube/answer/13525207?hl=en
- https://www.spotsaas.com/blog/best-podcast-hosting-platforms
- https://riverside.com/blog/best-podcast-hosting
- https://www.thepodcasthost.com/websites-hosting/best-podcast-hosting/
- https://dupple.com/learn/best-podcast-hosting
- https://yandex.ru/support/music/ru/podcast-authors/audio-placement
- https://quasa.io/ru/media/kak-dobavit-podkast-v-yandeks-muzyku-cherez-rss-i-obnovlyat-vypuski-avtomaticheski
- https://smink.ru/blog/sozdanie-sayta/platformy-dlya-podkastov-v-rossii-polnyy-gid-po-slushaniyu-i-publikacii/
- https://faq-st.zvuk.com/article/41531
- https://core.telegram.org/bots/api
- https://dev.to/serhii_a9c08345ac360cf5c8/how-to-upload-files-and-reuse-file-ids-in-telegram-bot-api-with-php-353a
- https://postproxy.dev/blog/x-api-pricing-2026/
- https://invisibleapi.ai/blog/x-api-pricing/
- https://twitterapi.io/blog/x-api-cost-breakdown-2026
- https://api.sorsa.io/blog/twitter-api-pricing-2026
- https://bigmike.help/en/devops/local-telegram-bot-api-advantages-limitations-of-the-standard-api-and-set-eb4a3b/
- https://hub.docker.com/r/dhanushreddy29/telegram-bot-api
- https://www.outstand.so/blog/x-api-pricing
- https://frameos.studio/blog/best-social-media-apis
- https://www.biggerbetterbackbeat.com/youtube-shows-vs-podcasts/
- https://grokipedia.com/page/2025_YouTube_demonetization_for_inauthentic_content
- https://www.socialmediatoday.com/news/youtube-clarifies-monetization-update-inauthentic-repeated-content/752892/
- https://air.io/en/monetization/youtube-monetization-policy-changes-2026-a-complete-dated-timeline
- https://support.google.com/youtube/answer/14328491?hl=en&co=GENIE.Platform%3DAndroid
- https://air.io/en/youtube-hacks/youtube-ai-disclosure-in-2026-label-it-yourself-or-youtube-will
- https://www.layer3labs.io/guides/youtube-ai-content-policy
- https://www.musicbusinessworldwide.com/spotify-extends-verified-by-spotify-badges-to-podcasts-further-cracking-down-on-ai-impersonators/
- https://www.pymnts.com/news/artificial-intelligence/2025/spotify-rolls-out-new-filters-disclosure-rules-ai-content/
- https://musically.com/2025/09/25/spotify-reveals-its-latest-measures-to-handle-ai-music/
- https://digital-strategy.ec.europa.eu/en/policies/code-practice-ai-generated-content
- https://euaicompass.com/eu-ai-act-article-50-transparency-guide.html
- https://europeancompliancesuite.com/eu-ai-act-article-50-labelling/
- https://artificialintelligenceact.eu/transparency-rules-article-50/
- https://www.nicfab.eu/en/posts/ai-transparency-disclosure/
- https://agentwares-agentguard.vercel.app/ai-disclosure/eu-ai-act-article-50-4-deepfakes-and-ai-text
- https://www.orrick.com/en/Insights/2026/08/EU-AI-Act-Transparency-Obligations-for-AI-Generated-Content-Article-50
- https://www.getphyllo.com/post/is-the-youtube-api-free-in-2026-quota-limits-costs-when-to-pay
- https://outlierkit.com/resources/youtube-api-pricing/
- https://outlierkit.com/resources/youtube-api-quota/
