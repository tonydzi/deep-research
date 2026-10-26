# DR26-10-18-HUB-02-0542 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: Mistral Le Chat / Vibe Work, skill deep-research, Think (account PaloAlto Ai Research Lab)
chat: https://chat.mistral.ai/work/97735bbb-b4d9-4c0c-bb3c-da6e45de1035
dr_id: DR26-10-18-HUB-02-0542
collected: 2026-10-18 07:16 Lisbon (ответ готов 06:34)
---

Фабрика публикации AI-подкастов (NotebookLM) на все платформы

Идентификатор запроса: DR26-10-18-HUB-02-0542 · Дата исследования: 18 октября 2026 г.

Вопрос

Как построить дневную рутину «готовый эпизод NotebookLM Audio Overview → обогащение (тексты, визуал, SEO/GEO) → публикация на YouTube, Spotify и других аудиоплатформах, X» при потоке ~35 эпизодов/день (RU и EN); кто уже построил такие конвейеры, какими инструментами, с какими результатами и рисками банов.

Резюме (главные выводы по убыванию важности)

[established] Официального публичного API у потребительского NotebookLM нет. Есть только Preview API в Gemini Notebook Enterprise (notebook + audio overview), отдельный Google Podcast API закрыт для новых клиентов. Всё, что работает у «пионеров», — браузерная автоматизация (Playwright/CDP) или неофициальные недокументированные эндпоинты, которые Google может сломать в любой момент. Для 35 эп/день Enterprise API — единственный «белый» путь.

[established] Главный риск бана — политика YouTube «inauthentic content» (переименована из «repetitious content» 15 июля 2025): массовый, шаблонный, минимально-обработанный контент не монетизируется. 35 видео/день одинакового формата «статичная картинка + аудио» — ровно профиль риска. Известен кейс удаления канала NotebookLM-подкастов без предупреждения [low-authority, single-source]; известны и успешно работающие каналы — исход зависит от доли ручного труда и уникальности.

[established] 35 загрузок/день на YouTube вписывается в дефолтную квоту. С июня 2026 г. модель квот изменилась: videos.insert — отдельный бакет 100 вызовов/день (а не «10 000 units ≈ 6 загрузок», как писали раньше). Проверяйте бакеты в своём проекте; расширение — только через аудит-форму Google.

[established] Spotify не имеет публичного API загрузки. Издание на Spotify и Apple — только через RSS-хостинг (у Buzzsprout и Transistor есть полноценные REST API; у RSS.com — API-ключи и автодистрибуция). YouTube принимает RSS (ingest), но RSS-эпизоды там публикуются со статичной картинкой и без «passthrough» — для качественного визуала лучше Data API.

[emerging] Для цитирования в ChatGPT/Perplexity/Google AI Overviews решает текстовая страница эпизода, а не аудио. Академический базис: добавление цитат, статистики и ссылок поднимает видимость в генеративных движках до ~40% (KDD 2024). Практические замеры «структурированный транскрипт + FAQPage + AudioObject → рост цитирований в 2–3×» — [single-source]; полный транскрипт — обязательный элемент.

[established] YouTube — платформа №1 подкастов в США по опросам (37–44% в 2025–2026), Spotify/Apple следуют; по времени — Spotify 29%, YouTube 28%, Apple 18%. Для RU-аудитории обязательна Яндекс Музыка (приём подкастов через RSS по заявке).

[emerging] Аудиограмма/волна почти не удерживает зрителя на YouTube против настоящего видео — данные противоречивы: старый кейс WNYC (+58% против статики в Facebook) против свежих наблюдений «разницы в удержании нет». Реалистичный максимум автоматизации: Wave/аудиограмма + караоке-субтитры + главы; аватары (HeyGen) и Video Overview — для топ-эпизодов.

Методология

Поисковые углы: (1) официальный статус API NotebookLM/Gemini Notebook Enterprise; (2) неофициальные инструменты и open-source конвейеры (GitHub, Reddit r/notebooklm, r/n8n, r/podcasting); (3) политики YouTube/Spotify по AI-контенту 2024–2026; (4) аудиоплатформы: API/RSS, цены, доли рынка (США и RU); (5) визуальные подходы и retention; (6) GEO/AEO для подкастов; (7) лимиты API (YouTube Data API, X API), локальный стек (Whisper).

Типы источников: официальная документация (Google/YouTube/Spotify/X/Яндекс/schema.org), Google Cloud docs, GitHub-репозитории, arXiv/KDD-статья, отраслевые исследования (Edison, Signal Hill/Cumulus, Triton), Reddit-кейсы, обзорные блоги и маркетинговые страницы (помечены [low-authority]).

**Ограничения:**Retention-данные конкретно для NotebookLM-каналов в открытом доступе отсутствуют; ряд цифр о ценах/лимитах — со сторонних снапшотов (могут меняться); X API-тарифы менялись несколько раз за год — приведены последние доступные значения. Русскоязычный рынок подкастов измерен слабо (данные 2020–2021 гг. и агрегаты Яндекс Музыки 2025 г.).

Выполненные поисковые запросы (26): «NotebookLM API audio overview download automation»; «NotebookLM Enterprise API audio overview 2025»; «notebooklm-py github unofficial NotebookLM API»; «NotebookLM audio overview to YouTube automation n8n workflow»; «NotebookLM podcast YouTube channel automation case study»; «YouTube inauthentic content policy repetitious content monetization AI 2025 July»; «YouTube altered content AI disclosure label synthetic media podcast»; «Spotify for Creators API upload episodes automation 2025»; «podcast hosting API comparison Buzzsprout Transistor RSS.com Podbean pricing 2025»; «YouTube podcasts RSS ingest claiming podcast 2025»; «AI generated podcast channel monetization NotebookLM demonetized Reddit»; «NotebookLM Video Overview feature launch slides 2025»; «audiogram vs static image podcast YouTube retention data»; «HeyGen avatar podcast video lipsync audio diarization»; «Opus Clip pricing API 2025 clips shorts automation»; «X Twitter API pricing tiers 2025 video upload limits»; «podcast platform market share 2025 Spotify Apple YouTube consumption Edison»; «generative engine optimization GEO research study citations LLM 2024 2025»; «podcast show notes full transcript SEO AI search ChatGPT Perplexity citation»; «schema.org PodcastEpisode FAQPage podcast website SEO»; «YouTube Data API quota 1600 units upload limit per day audit»; «Castmagic Podsqueeze Swell AI pricing API podcast show notes automation 2025»; «русские подкаст платформы Яндекс Музыка Apple Podcasts доля слушателей 2025»; «X Twitter video podcast upload premium limits minutes 2025»; «HeyGen pricing plans 2025 credits API»; «Headliner pricing audiogram podcast 2025»; «Remotion programmatic video podcast audiogram ffmpeg waveform»; «NotebookLM Slides feature studio slide deck 2025»; «GitHub Actions NotebookLM RSS daily podcast automation template»; «NotebookLM daily limit audio overviews generations per day Pro user»; «Gemini Notebook Enterprise pricing per user per month»; «Transistor API upload episode podcast developers»; «Buzzsprout API upload episode podcast»; «Spotify video podcasts creators upload 2025»; «Repurpose.io pricing podcast repurposing 2025»; «Яндекс Музыка добавить подкаст автору RSS заявка»; «faster-whisper GPU transcription speed benchmark RTX»; «Auphonic API pricing production credits»; «Veo 3.1 pricing per video Runway API cost per second 2025»; «YouTube upload limit per day how many videos spam 2025»; «Apple Podcasts submit RSS connect dashboard 2025».

Открытые и прочитанные URL (10): docs.cloud.google.com (API audio overview, Enterprise); autocontentapi.com/blog/does-notebooklm-have-an-api (статус-API, обновл. 2026-08-10); bundle.social/blog/youtube-api-quota-exceeded-limits-fixes (2026-08-04); parse.gl/blog/podcast-ai-visibility-playbook (2026-05-31, review 2026-10-05); github.com/upamune/notebooklm-podcast-automator (README); github.com/assafkip/notebooklm-daily-podcast (README); reddit.com (попытка r/notebooklm — заблокирован бот-доступ, использованы сниппеты); github.com/teng-lin/notebooklm-py; github.com/remotion-dev/template-audiogram; developers.google.com (квоты YouTube API).

1. Кто уже автоматизировал NotebookLM → публикация
1.1 Таблица кейсов «кто → как → результат → ссылка»

Кто

	

Как

	

Результат / статус

	

Ссылка




upamune (GitHub)

	

Python + Playwright + Chrome DevTools Protocol: создание ноутбуков, источники (сайты/YouTube), запуск Audio Overview + автозагрузка на Spotify for Creators через авторизованный браузер

	

Рабочий прототип полного цикла «источник → AO → Spotify»; май 2025 г.; все риски ToS на пользователе [single-source]

	

GitHub: upamune/notebooklm-podcast-automator




israelbls (GitHub)

	

REST API (FastAPI + Playwright): загрузка источников (URL/YouTube/текст), генерация AO с промптами, трекинг статуса, отдача MP3/M4A

	

Готовый «самодельный API» поверх NotebookLM; поддерживает EN/HE интерфейсы [single-source]

	

GitHub: israelbls/notebooklm-podcast-automator




assafkip (GitHub)

	

GitHub Actions без сервера: сбор новостей из RSS/Reddit → Claude отбирает темы → NotebookLM делает аудио → генерация RSS, страницы эпизодов, обложек, email-дайджеста

	

Полностью автономный ежедневный AI-news подкаст; релиз июнь 2026, активен сентябрь 2026 [single-source]

	

GitHub: assafkip/notebooklm-daily-podcast




inoueUJ (GitHub)

	

Только GitHub Actions: RSS-фиды/сайтмапы → каждое утро Audio Overview «радио-шоу»

	

Доказательство, что весь конвейер можно держать в CI без сервера [single-source]

	

GitHub: inoueUJ/notebooklm-radio




jackson7705 (GitHub)

	

Блог-посты → манифест эпизодов → NotebookLM-аудио → собственный RSS → Apple/Spotify

	

«Podcast-kit-template»: подкаст из блога без LLM API-ключей [single-source]

	

GitHub: jackson7705/podcast-kit-template




teng-lin (GitHub)

	

notebooklm-py — неофициальный Python API/CLI/агентский скилл: ноутбуки, источники, аудио-обзоры, слайды, видео, отчёты через недокументированные API Google

	

Наиболее полный неофициальный SDK; явное предупреждение «API могут сломаться»

	

GitHub: teng-lin/notebooklm-py, PyPI: notebooklm-py




GrapefruitCultural74 (Reddit r/n8n)

	

Два n8n-шаблона: (1) контент → AI-голоса → MP3; (2) MP3 → RSS-хостинг → «Spotify и все каталоги подхватывают сами»

	

Популярные шаблоны в галерее n8n; полная цепочка внутри n8n [low-authority, single-source]

	

Reddit r/n8n: Upload Podcast Episodes to Spotify Automatically




AutoContent API (коммерческий)

	

Независимый REST API «NotebookLM-стиля»: из URL/текста → подкаст, транскрипт, саммари, слайды, видео; webhooks, polling; описание полного пайплайна «генерация → RSS → Spotify»

	

Альтернатива без Google-аккаунта; продукт маркетинговый, цифр клиентов нет [low-authority]

	

autocontentapi.com, Статус API NotebookLM




Google (официально)

	

Gemini Notebook Enterprise Preview API: создание ноутбуков, источников и audio overview программно; отдельный Podcast API — deprecated, новые клиенты не подключаются

	

Единственный официальный автоматический путь; нужен Workspace/Cloud-контракт

	

Google Cloud: Manage audio overview (API)




«Lichskorpion» (Reddit r/notebooklm)

	

Загрузка NotebookLM-подкастов сериями на YouTube

	

Канал удалён без предупреждения — «spam, deceptive practices, scams» (окт. 2024) [low-authority, single-source]

	

Reddit: YouTube deleted my channel




Пользователи r/notebooklm (2026)

	

Монетизируемые NotebookLM-каналы

	

Треды о демонетизации «inauthentic content» и успешных апелляциях; полей нет [low-authority]

	

Reddit: appeal demonetization

Чисел (просмотры/подписчики/эпизоды в день) с достоверными источниками не найдено — все публичные кейсы это инструменты и треды, а не раскрытая статистика каналов. [no source found] Это само по себе важный вывод: ни один заметный канал не публикует «мы делаем N NotebookLM-эпизодов в день и получаем X просмотров» с проверяемыми данными.

1.2 Как достают аудио и что с этим ограничениями

Официально: кнопка Download в Studio (файл WAV/M4A) — вручную; Enterprise Preview API — программно (Google Cloud docs; статус-API — AutoContent API, обновл. 2026-08-10). [established]

Неофициально: Playwright/CDP-автоматизация браузера (upamune, israelbls, assafkip) или недокументированные эндпоинты (notebooklm-py). Ограничения: ломаются при смене UI, риск блокировки аккаунта Google, «API may break — Google can change internal endpoints anytime» (PyPI: notebooklm-py). [established]

Лимиты генерации NotebookLM (критично для 35/день): бесплатные аккаунты — 3 Audio Overview/день; Pro — до 20/день (Fastio) — но в 2026 г. Google перешёл на 5-часовые бюджеты использования (Plus/Pro = 2×/4× бюджета) (Elephas, 2026). Источники расходятся → [emerging, данные противоречивы]. Вывод: 35 AO/день на одном потребительском аккаунте невозможно; нужны Enterprise API, пул аккаунтов (риск ToS) или снижение темпа.

1.3 Баны и раскрытия

YouTube «inauthentic content» (с 15 июля 2025, переименование «repetitious content»): массовый, шаблонный, повторяющийся контент не монетизируется; политика применяется на уровне канала (YouTube Help: monetization policies). [established] YouTube прямо уточнял, что это «минорное обновление уже существовавшего правила», а не новый запрет (TubeBuddy, 2025).

AI-раскрытие YouTube: с 18 марта 2024 г. обязательна метка для реалистичного синтетического контента (зритель может принять за реального человека/место/событие); «Made with AI» под плеером; с мая 2026 YouTube вешает метки сам по C2PA-креденшлам (YouTube Help: disclosures, YouTube Blog, 2023-11-14). TTS-голос сам по себе метки не требует (AIR Media-Tech, 2026) [single-source] — т.е. «статика + аудио NotebookLM» раскрывать не обязательно, но аватары реалистичных людей (HeyGen) — обязательно. [established]

Spotify: публичной политики обязательной AI-метки для подкастов не найдено [no source found]; API-загрузки нет (см. §2), модерация каталога непрозрачна — задокументирован случай удаления 7-летнего шоу «The Young God» в марте 2025 без объяснений (RSS.com blog, 2026) [single-source].

Сообщество подкастеров уже фиксирует «AI-фермы на NotebookLM» как проблему (Reddit r/podcasts, 2025) — репутационный риск ниши. [low-authority]

2. Аудио-платформы: куда класть
2.1 Таблица «платформа → API/RSS → автоматизация → аудитория → цена»

Платформа

	

Приём контента

	

Автоматизация

	

Аудитория

	

Цена




Spotify

	

RSS от любого хостинга; собственный хостинг Spotify for Creators; видео-подкасты только через веб-загрузку вручную

	

Публичного API загрузки нет (подтверждение поддержки Spotify: community thread); есть неофициальный внутренний API Anchor (higuchiki docs) [low-authority]

	

США: №1–2 (по времени — 29%, Share of Ear Q3 2025, Westwood One, 2025-11-17)

	

Бесплатно




YouTube

	

(а) Data API videos.insert (видео любое); (б) RSS-ingest для подкастов (статичная обложка, без «passthrough», лента должна быть без рекламы)

	

API: 100 вставок/день по умолчанию с июня 2026 (Google: Quota Calculator, Bundle.social, 2026-08-04); RSS-ingest — автозагрузка новых эпизодов (YouTube Help, Podnews)

	

№1 в США по опросам: 37% (Edison 2026), 42% (Signal Hill Fall 2025), 44% (апрель 2026) — Podcast Insights, PodRewind, CompaniesHistory

	

API бесплатно




Apple Podcasts

	

Только RSS через Podcasts Connect

	

Ручная подача фида один раз, далее авто (Apple: Submit a show); API загрузки эпизодов нет [established]

	

США: 5–18% в зависимости от метрики (PodRewind); сильна в iOS-аудитории

	

Бесплатно




Яндекс Музыка (RU)

	

RSS по заявке автора с модерацией (Яндекс: Разместить подкаст)

	

Обновления фида подхватываются автоматически (quasa, RU) [single-source]

	

Главная RU-платформа; у отдельных шоу до 54% аудитории на Яндексе (Podcasts.ru) [low-authority]

	

Бесплатно




RSS-хостинги с API

	

Transistor: полный REST API + upload аудио (PUT) (developers.transistor.fm, changelog); Buzzsprout: create/update эпизодов; 9 окт. 2026 — API расширен загрузкой эпизодов, редактированием транскриптов, управлением каталогами (buzzsprout.com/api, Quasa, 2026-10-09 [single-source]); RSS.com: API-ключи, автодистрибуция в Spotify/Apple/Amazon (help.rss.com)

	

Один API-вызов = эпизод во всех каталогах (Apple, Amazon Music, Pocket Casts, Deezer, iHeart и др.) — «autodistribution» (RSS.com)

	

Независимо от аудитории, это транспорт

	

Transistor $19/49/99 (Transistor); Buzzsprout от $12 (StackScored, 2026-04-21); RSS.com, Podbean от $17–19, Libsyn от $5; «аудио+видео на Apple/Spotify/YouTube одной загрузкой»: Buzzsprout $25, Libsyn $12, RSS.com Max $37 (SpotSaaS, 2026)




X (Twitter)

	

Видео-посты: Premium — до 4 ч на web/iOS, до 10 мин на Android (X Help: longer videos); аудио-«посты» в web-плеере; Spaces

	

API pay-per-use (с окт. 2025): пост ≈ $0.015, $0.20 если с ссылкой, чтение $0.005; лимиты менялись несколько раз (Sorsa, обновл. окт. 2026, Blotato, 2026) [low-authority, тарифы волатильны]; legacy Basic $100/мес, Pro $5000/мес (Aether)

	

Мала для подкаст-прослушивания, велика для клипов

	

Premium от ~$8/мес; API: 35 постов/день с ссылками ≈ $210/мес




Amazon Music, Pocket Casts, Deezer, iHeart

	

RSS через хостинг

	

Автодистрибуция хостинга

	

Дают хвост; отдельно не автоматизируются

	

Бесплатно (в составе хостинга)

2.2 Доли рынка и языки

США/EN: YouTube 37–44% «самая используемая платформа» (Edison Podcast Consumer 2026 — 37%; Signal Hill Fall 2025 — 42%; апрель 2026 — 44%; Triton — 40%) — разброс между опросами сохраняю, это не ошибка, а разные методики (Podcast Insights 2026, PodRewind, CompaniesHistory). По времени прослушивания: Spotify 29% > YouTube 28% > Apple 18% (Edison Share of Ear Q3 2025, Westwood One, 2025-11-17). 92% слушают, а не смотрят, но 57% американцев 12+ «и слушали, и смотрели» подкасты (Infinite Dial 2026, Podder). [established]

RU: системных свежих измерений нет. Яндекс Музыка — ключевой канал (подкасты принимаются по заявке через RSS, см. выше), Apple сильна среди iOS, YouTube — среди видео-слушателей (Podcasts.ru, РБК Тренды — данные 2021 г., устаревшие, отмечаю явно). [speculative]

3. Визуальный ряд для YouTube
3.1 Таблица «подход → стоимость/эпизод → время → удержание → риск спама → пример»

Подход

	

Стоимость/эпизод (15–30 мин)

	

Время

	

Удержание

	

Риск спама

	

Пример/инструмент




(a) Статика + волна/аудиограмма

	

~$0 (ffmpeg showwaves — FFmpeg Micro); Remotion-шаблон бесплатно (remotion.dev/templates/audiogram); Headliner $9.99–19.99/мес (CreatorStackClub, 2026 [low-authority], headliner.app)

	

Минуты на GPU/CPU

	

Противоречиво: WNYC — аудиограммы обошли статику на 58% в Facebook (старые данные, соцсеть, не YouTube) (Content Allies [single-source]); свежее наблюдение: за 1,5 года на YouTube разницы в retention нет (Reddit r/podcasting, 2024 [low-authority]); на YouTube аудиограмма проигрывает «настоящему» видео (The Podcast Consultant [single-source])

	

Средний: 35 одинаковых волн/день = шаблон, см. §1.3

	

Headliner, ffmpeg, Remotion




(b) NotebookLM Video Overview / Slide Deck

	

Входит в подписку NotebookLM

	

5–15 мин генерации

	

Неизвестно; слайды дают смену кадров — лучше статики, данных нет [no source found]

	

Низкий-средний

	

Video Overview: «narrated slides» с цитатами/диаграммами из источников — запуск 29 июля 2025 (Google Blog, Android Authority, 2025-07-29, Workspace Updates, 2025-07). Прямой синхронизации Video Overview с Audio Overview нет — это отдельные независимые генерации (в доках они описаны как разные Studio-выходы) [inference — проверено косвенно]. Slide Deck — второй по популярности выход Studio (NotebookLM в X, 2025-12)




(c) Слайды/озвучка по расшифровке с таймкодами

	

~$0 (Marp/Gamma + ffmpeg) + LLM-токены; Gamma от ~$10/мес (цена меняется)

	

10–30 мин рендера

	

Данных по удержанию нет [no source found]

	

Средний: типовые шаблоны слайдов выглядят «конвейерно»

	

Marp+ffmpeg, Remotion, Google Slides API




(d) AI-видео/b-roll по смыслу реплик

	

Дорого: Veo 3.1 Lite $0.05/сек (720p) → Standard с аудио $0.40–0.75/сек (OpenRouter, CloudZero, 2026, SeedDance, 2026); Veo 3.1 GA с 17.11.2025. 20 мин b-roll ≈ $60–900 → только шорты

	

Часы рендера на 35 эп

	

B-roll оживляет, но клип-залипания не даёт; данных по NotebookLM-подкастам нет [no source found]

	

Средний

	

Veo 3.1, Kling 3.0 ($6.99/мес, Rangy, 2026), Runway ($15+/мес), Pika




(e) Аватары двух ведущих поверх аудио (диаризация)

	

HeyGen: Creator $29/мес, API — pay-as-you-go кредиты (heygen.com/pricing, HeyGen Help); есть готовый «AI Video Podcast» (два спикера из URL/PDF) (heygen.com/apps/ai-video-podcast) и audio-to-video API с липсинком (developers.heygen.com/audio-to-video)

	

Минуты-десятки минут/эп

	

Потенциально выше статики («лицо удерживает» — логика YouTube-алгоритма, The Podcast Consultant [single-source]); данных по AI-аватарам подкастов нет [no source found]

	

Высокий: реалистичные аватары = обязательная метка YouTube «altered/synthetic» (YouTube Help); риск «uncanny valley» и «AI-slop»-восприятия

	

HeyGen, Hedra, Captions, Synthesia




(f) Караоке-субтитры, главы, авто-шорты

	

Opus Clip: Starter $15 (150 мин обработки), Pro $29 (eesel, 2026 [single-source], opus.pro); API есть (opus.pro/api); Descript, Vizard — аналоги

	

Минуты

	

Субтитры повышают досматривание шортов; точных цифр нет [no source found]

	

Низкий

	

Opus Clip (ClipAnything), Descript

3.2 Что удерживает, что выглядит как спам

[established] YouTube оптимизирует под watch-time визуального контента; аудио со статикой и аудиограммы проигрывают видео с людьми (The Podcast Consultant, 2025/2026).

[emerging] Спор о пользе аудиограмм не закрыт: старый WNYC-кейс (+58% против статики) — про Facebook, не про YouTube; свежий опыт оператора «1,5 года, разницы нет» — Reddit. Я сохраняю обе позиции: аудиограмма дешёвая и не вредит, но «серебряной пули» удержания она не даёт.

Видео-подкасты — растущий сегмент: 53% слушателей США выбирают «watchable» подкасты (Backlinko, 2025); видео на YouTube — самая быстрорастущая точка входа (Spotify for Creators).

4. Тексты: описания, Q&A и GEO
4.1 Что реально работает для цитирования LLM-поиском

[established — академический базис] Статья GEO (Aggarwal et al., KDD 2024): добавление цитат, статистики и ссылок в текст повышает видимость источника в генеративных движках до ~40%; кейс-стади, методология открытая (arXiv:2311.09735, ACm DL).

[emerging, single-source] Практика подкастов: страницы эпизодов с полным структурным транскриптом + FAQPage + AudioObject schema цитировались в 2–3 раза чаще (FORKOFF 2026, через Parse, 2026-05-31); «высококачественные транскрипты → в 4–7 раз больше AI-цитирований» (Am I Cited, через тот же обзор). Ключевой механический принцип: AI читает отрендеренный HTML и JSON-LD, аудио он не транскрибирует — транскрипт должен быть server-side HTML с метками спикеров и таймкодами, не спрятан за JS-переключателем (Parse).

[single-source] Чек-лист AEO для подкастов: полный транскрипт = «самый высокоэффективный шаг»; на странице — PodcastEpisode/PodcastSeries + FAQPage schema; ответ на вопрос — в первых строках (Springcast).

Schema.org: типы PodcastEpisode/PodcastSeries официальны (schema.org/PodcastEpisode); FAQPage — для Q&A-блоков. Доказательств прироста позиций классического SEO именно от PodcastEpisode нет [no source found] — это семантическая разметка для машин, ставьте без иллюзий.

Q&A-формат: структура «вопрос → ответ» (как FAQ/Reddit) рекомендована для AEO в отраслевых гайдах (Parse, Springcast); прямых A/B-исследований «Q&A vs обычное описание» для подкастов не найдено [no source found] — рекомендация следует из механики ретрива (извлекаемые пассажи), а не из замеров.

4.2 Авторская подпись / раскрытие AI

Примеров формата «сделано Антоном и его AI-кофаундером» с исследованиями реакции аудитории не найдено [no source found]. Ближайшее: обсуждения в r/notebooklm о монетизации AI-подкастов, где ключевой критерий — права на исходники и честность с Google ToS (Reddit, 2025) [low-authority].

Практический консенсус из политик: честное раскрытие («AI-ведущие по вашим материалам») не противоречит ни YouTube (метка нужна только для фотореалистичного), ни лучшим практикам доверия. [speculative — моя рекомендация, не источник]

5. Конвейер на 35 эпизодов в день
5.1 Лимиты — узкие места

Ограничение

	

Значение

	

Источник, дата




NotebookLM: генерация AO

	

Free 3/день; Pro ~20/день; с 2026 — 5-часовые бюджеты (Plus 2×, Pro 4×) — источники противоречивы

	

Fastio, Elephas, 2026 — [emerging]




YouTube Data API

	

С июня 2026: videos.insert 100 вызовов/день, search.list 100/день, прочее 10 000 units/день; сброс в полночь PT; расширение только через аудит-форму

	

Google Quota Calculator, Google: аудиты, Bundle.social, 2026-08-04 — [established]




YouTube: неофициальный дневной лимит загрузок

	

~10–20 (новые каналы) … ~100 (возрастные) — наблюдения, не официальный документ

	

SocialRails, 2026 [low-authority]; репорт о харде в 100 видео/день (Reddit r/PartneredYoutube, 2026) [single-source] — [speculative]




YouTube политика

	

«inauthentic content»: массовое/шаблонное → демонетизация

	

YouTube Help, обновл. 15.07.2025 — [established]




Spotify

	

API загрузки нет; RSS; видео — вручную через web

	

Spotify Community, Spotify Support — [established]




X API

	

Pay-per-use: ~$0.015/пост, ~$0.20 с ссылкой; тарифы менялись окт. 2025 – апр. 2026

	

Sorsa, окт. 2026, Outstand, 2026 — [emerging, волатильно]




Транскрипция локально

	

faster-whisper до 4–5× быстрее openai/whisper (SYSTRAN); RTX 4060 large-v3 int8 ≈ 7× реального времени (PromptQuorum, 2026 [single-source]); RTX 5060 Ti справится с 35×25 мин ≈ 875 мин аудио за ~2 ч GPU [inference]

	

—

5.2 Готовые инструменты конвейера

n8n: полные шаблоны «контент → AI-голоса → MP3 → RSS-хост → все каталоги» (Reddit r/n8n, 2025) [low-authority]; open-source аналоги: assafkip/notebooklm-daily-podcast, inoueUJ/notebooklm-radio, jackson7705/podcast-kit-template (см. §1.1).

SaaS обработки текста: Castmagic (шоу-ноты/цитаты/соцпосты из аудио, от ~$23/мес — indevai); Podsqueeze (бесплатно 50 мин/мес, от $12/мес — Rephonic, 2025); Swell AI; Descript. Все они генерируют тексты, но ни один не «клеит» полный конвейер с YouTube-загрузкой — оркестрацию собираете сами.

Кросспостинг видео: Repurpose.io от $35/мес (repurpose.io) — автоматизация «подкаст → клипы → соцсети», но ориентирован на видео-исходники.

Аудио-мастеринг: Auphonic API, бесплатно 2 ч/мес, платно от $13/мес (Auphonic, Outmano [single-source]); для 35×25 мин ≈ 875 мин/мес нужен платный тариф ($30–50) или локальный ffmpeg loudnorm ($0).

5.3 Рекомендуемая архитектура (один оператор, RTX 5060 Ti, LLM-подписки)

Принцип: локально — то, что предсказуемо и дёшево (транскрипция, рендер, мастеринг); покупаем — то, что даёт API и дистрибуцию (хостинг, шортс, опционально HeyGen).

Plain text
NotebookLM (Enterprise API или Playwright-автоматизация)
   ↓ WAV/M4A + (опц.) Video Overview
[Локально] faster-whisper large-v3 (диаризация WhisperX/pyannote)
   ↓ транскрипт с таймкодами и спикерами
[LLM-подписка] 1 вызов на эпизод → заголовок, описание, Q&A-блок (5–7 вопрос-ответ),
   главы с таймкодами, саммари, 3 цитаты, посты для X, JSON-LD (PodcastEpisode+FAQPage)
[Локально] ffmpeg/Remotion: обложка (шаблон+Flux/SDXL) + волна/аудиограмма
   + караоке-субтитры по транскрипту + главы в метаданных; топ-10% эпизодов → HeyGen-аватары
   ↓ готовый MP4 + текстовый пакет
[Buzzsprout/Transistor API] 1 POST → эпизод в RSS → Spotify, Apple, Amazon, Яндекс, Pocket Casts, Deezer, iHeart
[YouTube Data API] videos.insert ≤35/день (укладывается в бакет 100) — своё видео, свои главы, своё описание
[X] 1–3 шорта/день из лучших моментов (Opus Clip API) + ссылка; посты через API pay-per-use или руками
[Сайт-зеркало] страница эпизода: полный транскрипт (server-side HTML), FAQPage, AudioObject → GEO

MVP за неделю (ручных мест: загрузка аудио из NotebookLM + approve текстов):

День 1–2: скрипт Playwright для скачивания AO (или Enterprise API, если доступ) + faster-whisper локально.

День 2–3: LLM-промпт-пакет «описание+Q&A+главы+X-пост»; сайт-страница эпизода с шаблоном (NextDocs/любой SSG).

День 3–4: ffmpeg-скрипт «обложка+волна+субтитры» (одна команда, батчем).

День 4–5: аккаунт Transistor/Buzzsprout, API-загрузка, подача RSS в Spotify/Apple/Яндекс.

День 5–7: YouTube Data API-загрузчик (40 units на метаданные поверх вставки — следите бакетами), плейлист-подкаст, тестовые 5 эпизодов.

Целевой конвейер (1–2 месяца): n8n/self-hosted оркестратор очередей; полуавто-модерация оператора (30–60 мин/день на approve); топ-эпизоды → HeyGen-аватары и Video Overview; авто-шорты через Opus Clip; GEO-трекинг цитирований (Parse/Am I Cited или ручные промпт-тесты раз в неделю).

Ориентировочный бюджет в месяц: хостинг $12–49 · YouTube API $0 · X API ~$0–210 (зависит от постов со ссылками) · Opus Clip $15–29 · HeyGen (опц.) $29+ · Auphonic (опц.) $13–50 · LLM-подписки — уже есть · NotebookLM Pro/Enterprise — цена Enterprise непублична, сторонние оценки ~$9/лицензия/мес (FelloAI, 2026 [low-authority]) · итого ~$50–200/мес без учёта уже оплаченных LLM. [частично speculative — оценки]

6. Опровергнуто / мифы / риски

Утверждение

	

Вердикт

	

Почему




«У NotebookLM есть публичный API»

	

Опровергнуто

	

Публичного self-serve API нет; только Enterprise Preview (Google Cloud, AutoContent, 2026-08-10) [established]




«YouTube-квота = 10 000 units, значит только 6 загрузок/день»

	

Устарело

	

С июня 2026 — отдельный бакет 100 videos.insert/день (Google, Bundle.social, 2026-08-04)




«YouTube банит все NotebookLM-каналы»

	

Ложная генерализация

	

Есть кейс удаления (Reddit, 2024 [low-authority]) и есть монетизируемые каналы; July 2025-политика карает шаблонность и масс-продакшн, а не AI как таковой (YouTube Help). Дисклеймер YouTube: AI-контент с реальной ценностью монетизируем (Fliki, 2025 [single-source])




«Аудиограммы сильно повышают удержание на YouTube»

	

Спорно

	

Данные WNYC — старые и про Facebook; свежие наблюдения разницы не видят (см. §3.1). Сохраняю обе версии как несогласованные




«Video Overview NotebookLM синхронизируется с Audio Overview»

	

Опровергнуто (практически)

	

Это разные Studio-выходы с раздельной генерацией; опции «сложить звук AO на слайды VO» в доках нет (Google Blog, 2025) [inference from docs]




«Spotify можно заливать по API»

	

Опровергнуто

	

Публичного API нет, только RSS/ручная web-загрузка (Spotify Community) [established]




«Полный транскрипт точно гарантирует цитирование в ChatGPT»

	

Преувеличено

	

Транскрипт+schema — самый сильный фактор по всем источникам, но цифры роста (2–7×) — [single-source], а гарантий нет (Parse, 2026)




«35 эпизодов в день — нормальный режим для YouTube»

	

Высокий риск

	

Формально API позволяет; фактически это профиль «inauthentic content» + неофициальные дневные лимиты загрузок; безопасный режим — 5–10/день с уникализацией каждого

7. Открытые вопросы

Цена и квоты Gemini Notebook Enterprise для 35 AO/день — публичной цифры нет; нужен прямой запрос Google Cloud. Это главный блокер полностью автоматической генерации.

Retention-статистика именно NotebookLM-каналов (статика vs Video Overview vs аватары) — в открытом доступе отсутствует; придётся мерить самим на A/B-плейлистах.

Точные текущие тарифы X API — источник от октября 2026, но тарифы менялись 3 раза за год; перед интеграцией сверить с docs.x.com.

Единая RU-статистика платформ — свежих измерений нет (последние полные — 2020–2021).

Политика Spotify к AI-подкастам — обязательных AI-меток не найдено, но модерация каталога задокументированно жёсткая и непрозрачная (кейс «The Young God»).

Раскрытие формата «ведущий + AI-кофаундер»: эффект на аудиторию не исследован.

8. Рекомендации

Вариант A — «Минимальный белый» (5–10 эп/день): NotebookLM Pro вручную/полуавтоматом → локальный стек (Whisper + ffmpeg + LLM) → Transistor API → RSS-дистрибуция + YouTube Data API. Плюсы: дёшево (~$50/мес), почти нулевой риск банов. Минусы: не 35/день, аудио-визуал слабый. [established — технически]

Вариант B — «Enterprise-фабрика» (35 эп/день): контракт Gemini Notebook Enterprise → официальный API AO → тот же локальный стек → YouTube 35 вставок (в пределах бакета 100) + HeyGen-аватары на топ-20% + авто-шорты. Плюсы: масштаб, легальность, воспроизводимость. Минусы: непубличная цена Enterprise; YouTube-риск «inauthentic content» на 35 шаблонов/день — нужен оператор-редактор и вариативность форматов; X-постинг со ссылками дороже всего.

Вариант C — «Гибрид» (рекомендуемый): 10–15 «полных» эпизодов/день с уникализацией (Video Overview или аватары на лучших, свои обложки, живые главы, Q&A-страницы с транскриптом) + остальные как аудио через RSS на аудио-платформы без YouTube-перегрузки; YouTube постепенно масштабировать по данным retention. Плюсы: покрывает все платформы, удерживает риск-профиль под контролем, конвертирует GEO-эффект. Минусы: сложнее оркестрация, нужен 1 оператор на 1–2 ч/день.

Единая рекомендация: Вариант C. [emerging — уверенность средняя] Главное допущение, на котором она держится: YouTube-алгоритм и монетизация толерантны к AI-аудио при уникальном визуале/текстах и умеренном темпе, а Enterprise API окажется доступным по цене для вашей команды (если нет — откат к Варианту A с последующим ростом). Первое контрольное число: через 4 недели сравнить retention и подписки двух форматов (статика vs Video Overview) на 2×30 эпизодах — это и решит спор об аудиограммах вашими данными.

Примечания об источниках

Источник

	

Достоверность

	

Обновлено




Google Cloud: Manage audio overview (API)

	

5/5

	

—




Google: Quota Calculator YouTube Data API

	

5/5

	

—




Google: Quota and Compliance Audits

	

5/5

	

—




YouTube Help: Monetization policies (inauthentic content)

	

5/5

	

2025-07-15




YouTube Help: Deliver podcasts using an RSS feed

	

5/5

	

—




YouTube Help: How this content was made (AI-раскрытие)

	

5/5

	

—




YouTube Blog: Disclosing altered or synthetic content

	

5/5

	

2023-11-14




Google Blog: Video Overviews in NotebookLM

	

5/5

	

2025-07-29




Workspace Updates: Video Overviews rollout

	

5/5

	

2025-07




Workspace Updates: Audio Overviews 50+ языков

	

5/5

	

2025-04-29




X Help: Longer videos for Premium

	

5/5

	

—




Spotify Community: нет API загрузки (staff)

	

4/5

	

—




Spotify Support: Publishing videos

	

5/5

	

—




Apple Podcasts: Submit a show

	

5/5

	

—




Яндекс Музыка: Разместить подкаст

	

5/5

	

—




Schema.org: PodcastEpisode

	

5/5

	

—




Aggarwal et al. GEO, arXiv:2311.09735 (KDD 2024)

	

5/5

	

2024-08 (KDD)




Westwood One / Cumulus / Signal Hill: Podcast Download Fall 2025

	

4/5

	

2025-11-17




Podcast Insights: 2026 Podcast Stats

	

4/5

	

2026




PodRewind: Platform Market Share 2026

	

3/5

	

2026




CompaniesHistory: Podcasting Market Statistics 2026

	

3/5

	

2026




Backlinko: Podcast Statistics

	

4/5

	

2025




Transistor: API Reference и changelog: API upload

	

5/5

	

—




Buzzsprout: The Buzzsprout API

	

5/5

	

—




Quasa: Buzzsprout API expansion

	

3/5

	

2026-10-09




StackScored: Podcast Hosting Pricing

	

3/5

	

2026-04-21




SpotSaaS: Best Podcast Hosting 2026

	

3/5

	

2026




RSS.com: Hosting Compared + autodistribution

	

3/5

	

2026




Podnews: YouTube RSS plans (no passthrough, без рекламы)

	

4/5

	

2023–2024




GitHub: upamune/notebooklm-podcast-automator

	

3/5

	

2025-05-30




GitHub: israelbls/notebooklm-podcast-automator

	

3/5

	

—




GitHub: teng-lin/notebooklm-py, PyPI

	

3/5

	

—




GitHub: assafkip/notebooklm-daily-podcast

	

3/5

	

2026-06-25 (коммиты до 2026-09-05)




GitHub: inoueUJ/notebooklm-radio

	

3/5

	

—




GitHub: jackson7705/podcast-kit-template

	

3/5

	

—




GitHub: remotion-dev/template-audiogram

	

4/5

	

—




SYSTRAN/faster-whisper

	

5/5

	

—




GitHub: papsl/spotify-podcaster-uploader

	

2/5

	

—




Higuchiki: unofficial Spotify for Creators API

	

2/5

	

—




AutoContent API: Does NotebookLM have an API

	

2/5 (вендор)

	

2026-08-10




AutoContent API: Automate Podcast on Spotify

	

2/5 (вендор)

	

—




Parse: Podcast AI visibility playbook

	

3/5

	

2026-05-31 (review 2026-10-05)




Springcast: AEO for podcasts

	

3/5

	

—




Content Allies: Podcast audiograms (данные WNYC)

	

3/5

	

—




The Podcast Consultant: YouTube Podcast

	

3/5

	

2025–2026




Reddit r/notebooklm: удаление канала

	

2/5

	

2024-10




Reddit r/notebooklm: апелляция демонетизации

	

2/5

	

2026




Reddit r/n8n: Spotify автоматизация

	

2/5

	

2025




Reddit r/podcasting: аудиограммы и retention

	

2/5

	

2024




Reddit r/podcasts: AI content farms

	

2/5

	

2025




Fastio: NotebookLM Audio Overview Limit

	

2/5

	

2025–2026




Elephas: NotebookLM Daily Limits 2026

	

2/5

	

2026




FelloAI: NotebookLM Pricing 2026

	

2/5

	

2026




Bundle.social: YouTube API Quota (2026)

	

3/5

	

2026-08-04




Sorsa: X API Pricing 2026

	

2/5

	

2026-10




Blotato: X API Pricing 2026

	

2/5

	

2026




Outstand: X API Pricing 2026

	

2/5

	

2026




Aether: X API tiers

	

2/5

	

2026




OpenRouter: Veo 3.1 pricing

	

4/5

	

2026




CloudZero: Veo 3 pricing

	

3/5

	

2026




SeedDance: AI Video API Pricing Comparison

	

3/5

	

2026




Rangy: AI Video Pricing 2026

	

3/5

	

2026




HeyGen: Pricing, API pricing, Audio-to-Video API, AI Video Podcast

	

4/5 (вендор)

	

—




OpusClip: Pricing, API, eesel: pricing 2026

	

3/5

	

2026




Headliner: Pricing, CreatorStackClub: Headliner pricing

	

3/5

	

2026




Repurpose.io: Pricing

	

4/5 (вендор)

	

—




Auphonic: Pricing

	

4/5 (вендор)

	

—




Rephonic: Five AI Podcast Summarizers

	

3/5

	

2025




GigaGPU: Whisper vs Faster-Whisper

	

3/5

	

2026




PromptQuorum: Whisper STT comparison 2026

	

2/5

	

2026




SocialRails: YouTube Upload Limits 2026

	

2/5

	

2026




Podcasts.ru: Где слушают подкасты в России

	

3/5

	

2021–2023




РБК Тренды: Кто и где слушает подкасты на русском

	

3/5

	

2021 (устаревшее — отмечено)




Podcasts.ru: Итоги 2025 Яндекс Музыки

	

3/5

	

2025-12




XDA: Video Overviews launch

	

3/5

	

2025-08-09




Android Authority: Video Overviews

	

3/5

	

2025-07-29




TubeBuddy: YouTube Monetization Update

	

3/5

	

2025-07




AIR Media-Tech: AI disclosure 2026

	

3/5

	

2026




AIR Media-Tech: Inauthentic content

	

2/5

	

2026




Fliki: Monetization Policy 2025

	

2/5

	

2025-07

Конфликты и оговорки:

Лимиты NotebookLM: Fastio (3 free / 20 Pro в день) против Elephas (5-часовые бюджеты 2026) — обе версии приведены, выбрать можно только проверкой своего аккаунта.

Доли YouTube в США: 37% (Edison) vs 42% (Signal Hill) vs 44% (Signal Hill апрель 2026) vs 40% (Triton) — методики опросов разные; по времени (Share of Ear) Spotify (29%) чуть выше YouTube (28%).

Польза аудиограмм: WNYC (+58%, Facebook, старые) vs оператор-наблюдение «нет разницы» (YouTube, 1,5 года) — обе стороны приведены; решается вашим A/B-тестом.

Цифры «рост AI-цитирований в 2–7×» — из одного обзорного источника (Parse), опирающегося на частные аудиты FORKOFF/Am I Cited: [single-source].




## Ссылки канваса

- https://github.com/upamune/notebooklm-podcast-automator
- https://github.com/israelbls/notebooklm-podcast-automator
- https://github.com/assafkip/notebooklm-daily-podcast
- https://github.com/inoueUJ/notebooklm-radio
- https://github.com/jackson7705/podcast-kit-template
- https://github.com/teng-lin/notebooklm-py
- https://pypi.org/project/notebooklm-py/
- https://www.reddit.com/r/n8n/comments/1mrblk2/upload_podcast_episodes_to_spotify_automatically/
- https://autocontentapi.com/blog/automate-podcast-generation-spotify
- https://autocontentapi.com/blog/does-notebooklm-have-an-api
- https://docs.cloud.google.com/gemini/enterprise/notebooklm-enterprise/docs/api-audio-overview
- https://www.reddit.com/r/notebooklm/comments/1fvj3j4/youtube_deleted_my_channel_for_uploading/
- https://www.reddit.com/r/notebooklm/comments/1suh7jm/has_anyone_successfully_appealed_youtube/
- https://fast.io/resources/notebooklm-audio-overview-limit/
- https://elephas.app/blog/notebooklm-daily-limit
- https://support.google.com/youtube/answer/1311392?hl=en
- https://www.tubebuddy.com/blog/youtube-monetization-update/
- https://support.google.com/youtube/answer/15447836?hl=en
- https://blog.youtube/news-and-events/disclosing-ai-generated-content/
- https://air.io/en/youtube-glossary/what-is-youtubes-ai-content-disclosure-policy
- https://rss.com/blog/best-podcast-hosting-platforms/
- https://www.reddit.com/r/podcasts/comments/1liyjtc/we_have_entered_the_thick_of_pure_ai_content/
- https://community.spotify.com/t5/Spotify-for-Developers/Request-for-Access-to-Upload-Podcasts-to-Spotify-from-External/td-p/7044523
- https://higuchiki.github.io/spotify-for-creators-api/
- https://www.westwoodone.com/blog/2025/11/17/audio-remains-the-primary-mode-of-podcast-consumption-despite-growing-video-use-92-say-they-listen-to-podcasts-according-to-cumulus-media-and-signal-hill-insights-podcast/
- https://developers.google.com/youtube/data/determine_quota_cost
- https://bundle.social/blog/youtube-api-quota-exceeded-limits-fixes
- https://support.google.com/youtube/answer/13525207?hl=en
- https://podnews.net/update/youtube-rss-plans
- https://www.podcastinsights.com/podcast-statistics/
- https://podrewind.com/blog/podcast-platform-market-share-2026
- https://www.companieshistory.com/podcasting-market-statistics/
- https://podcasters.apple.com/support/897-submit-a-show
- https://yandex.ru/support/music/ru/podcast-authors/audio-placement
- https://quasa.io/ru/media/kak-dobavit-podkast-v-yandeks-muzyku-cherez-rss-i-obnovlyat-vypuski-avtomaticheski
- https://podcasts.ru/gde-slushayut-podkasty-v-rossii/
- https://developers.transistor.fm/
- https://transistor.fm/changelog/api-upload/
- https://www.buzzsprout.com/api
- https://quasa.io/stories/buzzsprout-s-api-can-now-publish-episodes-but-it-still-has-no-webhooks
- https://help.rss.com/en/support/solutions/articles/44002797895-connect-apple-podcasts-to-rss-com
- https://transistor.fm/best-podcast-hosting-platform/
- https://www.stackscored.com/pricing/podcast-hosting/
- https://www.spotsaas.com/blog/best-podcast-hosting-platforms
- https://help.x.com/en/using-x/premium-longer-videos
- https://api.sorsa.io/blog/twitter-api-pricing-2026
- https://www.blotato.com/blog/twitter-api-pricing
- https://aetherhq.dev/x-api
- https://www.podderapp.com/post/audiograms-for-podcasters
- https://trends.rbc.ru/trends/social/60e5639e9a7947713a6f6621
- https://www.ffmpeg-micro.com/blog/an-ffmpeg-audiogram-is-one-filter-the-rest-is-what-breaks
- https://www.remotion.dev/templates/audiogram
- https://www.creatorstackclub.com/software/headliner/pricing
- https://www.headliner.app/pricing/
- https://contentallies.com/learn/how-to-make-podcast-audiograms
- https://www.reddit.com/r/podcasting/comments/1fv4epl/is_there_point_in_audiograms/
- https://thepodcastconsultant.com/blog/youtube-podcast
- https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-video-overviews-studio-upgrades/
- https://www.androidauthority.com/notebooklm-video-overviews-launched-3582151/
- https://workspaceupdates.googleblog.com/2025/07/video-overviews-studio-panel-updates-notebooklm.html
- https://x.com/NotebookLM/status/2001335565589848142
- https://openrouter.ai/google/veo-3.1
- https://www.cloudzero.com/blog/veo-3-pricing/
- https://www.seeddance.io/blog/ai-video-api-pricing-comparison
- https://rangy.ai/blog/ai-video-pricing-explained/
- https://www.heygen.com/pricing
- https://help.heygen.com/en/articles/10060327-heygen-api-pricing-explained
- https://www.heygen.com/apps/ai-video-podcast
- https://developers.heygen.com/audio-to-video
- https://www.eesel.ai/blog/opusclip-pricing
- https://www.opus.pro/
- https://www.opus.pro/api
- https://backlinko.com/podcast-stats
- https://creators.spotify.com/features/video
- https://arxiv.org/abs/2311.09735
- https://dl.acm.org/doi/abs/10.1145/3637528.3671900
- https://parse.gl/blog/podcast-ai-visibility-playbook
- https://www.springcast.io/en/blog/aeo-for-podcasts/
- https://schema.org/PodcastEpisode
- https://www.reddit.com/r/notebooklm/comments/1jf3qao/can_notebooklm_audio_overview_podcast_be/
- https://developers.google.com/youtube/data/guides/quota_and_compliance_audits
- https://socialrails.com/blog/youtube-upload-limits-complete-guide
- https://www.reddit.com/r/PartneredYoutube/comments/1ol6agg/youtube_daily_limit_of_100_videos_upload/
- https://support.spotify.com/us/creators/article/publishing-videos/
- https://www.outstand.so/blog/x-api-pricing
- https://github.com/SYSTRAN/faster-whisper
- https://www.promptquorum.com/power-local-llm/local-whisper-stt-comparison-2026
- https://cms.indevai.com/tool/podsqueeze/
- https://rephonic.com/blog/podcast-summarizer/
- https://repurpose.io/pricing/
- https://auphonic.com/pricing
- https://outmano.com/tools/auphonic/pricing
- https://felloai.com/notebooklm-pricing/
- https://fliki.ai/blog/youtube-monetization-policy-2025
- https://workspaceupdates.googleblog.com/2025/04/language-expansion-audio-overviews-notebooklm.html
- https://github.com/remotion-dev/template-audiogram
- https://github.com/papsl/spotify-podcaster-uploader
- https://www.opus.pro/pricing
- https://gigagpu.com/whisper-vs-faster-whisper-speed/
- https://podcasts.ru/itogi-2025-goda-v-podkastah-ot-yandeks-muzyki/
- https://www.xda-developers.com/notebooklms-video-overviews-launch/
- https://newmoneymatrix.org/youtube-inauthentic-content-policy-explained/