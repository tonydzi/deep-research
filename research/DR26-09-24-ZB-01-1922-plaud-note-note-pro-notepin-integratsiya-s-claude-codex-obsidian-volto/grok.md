# DR26-09-24-ZB-01-1922 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Соберу живые источники по CLI, MCP, API и реальным связкам Plaud с волтом и CRM. Начинаю с официальных страниц и репозиториев.Официальные страницы нашлись. Открываю документацию CLI, MCP и API целиком, плюс обвязки и пользовательские пайплайны.# DR26-09-24-ZB-01-1922 — Plaud: CLI, MCP, API и связки с Claude / Codex / Obsidian / CRM

Период источников: в основном май–сентябрь 2026. Цитаты ниже сняты со страниц, открытых в этом заходе. Поиск по Reddit отдал сниппеты, но живая страница Reddit не открылась — цифры оттуда не используются.

## Краткий вывод (10 пунктов)

1. Для личного аккаунта Plaud официально даёт не «публичный API с ключом», а два read-only клиента: CLI `@plaud-ai/cli` и MCP `@plaud-ai/mcp`. Поддержка прямо пишет: публичного API для своих записей нет, ключ не нужен. https://support.plaud.ai/hc/en-us/articles/60726890231449-How-can-I-get-API-access-to-my-Plaud-data
2. MCP — семь инструментов (`login`, `logout`, `get_current_user`, `list_files`, `get_file`, `get_note`, `get_transcript`) плюс шесть скиллов-инструкций. Клиенты из коробки: Claude Desktop, Claude Code, Codex Desktop, Cursor, Windsurf, VS Code, Zed, Claude Web, ChatGPT Web. https://docs.plaud.ai/plaud-mcp-cli/mcp
3. CLI умеет логин, список, поиск по имени (не по тексту стенограммы), транскрипт, markdown-саммари и временную ссылку на аудио. Токены CLI: `~/.plaud/tokens.json`. Токены MCP — отдельный файл `~/.plaud/tokens-mcp.json`. База по умолчанию: `https://platform.plaud.ai/developer/api`. https://docs.plaud.ai/plaud-mcp-cli/cli
4. Ссылка на аудио живёт 24 часа. Формат файла в официальных CLI/MCP не назван. Экспорт из приложения: аудио MP3 и WAV, саммари в том числе Markdown. https://support.plaud.ai/hc/en-us/articles/57751026815257-Plaud-CLI и https://support.plaud.ai/hc/en-us/articles/50835453223705-Export-recordings-transcripts-and-summaries
5. Старый «Plaud OAuth API» — закрытая бета, отдаёт только 20 последних записей и новых не пускает. Поддержка говорит, что MCP и CLI этим лимитом не связаны. Поиск CLI при этом смотрит только 500 последних имён. https://support.plaud.ai/hc/en-us/articles/59291709269401-Why-doesn-t-the-Plaud-API-return-all-of-my-recordings
6. Вебхука «появилась новая запись» у личного CLI/MCP нет. Единственный официальный push личного аккаунта — Zapier, и он стреляет не на загрузку аудио, а на «transcripts generated» и «summaries generated». Plaud в Zapier только триггер, не действие. https://support.plaud.ai/hc/en-us/articles/12200669941647-Zapier-integration
7. Plaud Embedded (`platform-us.plaud.ai`, партнёрский `client_id` / secret / API key) — другой продукт: чужое приложение на железе Plaud и своя транскрипция. Это не способ выгрузить свой волт. Транскрипция там описана как poll, не как push. https://docs.plaud.ai/documentation/embedded_sdk/transcription_api
8. MCP и CLI бесплатны для активного аккаунта, но текст появляется только после транскрипции, а она ест минуты плана: Starter 300 мин/мес, Pro 1 200 мин/мес, Unlimited в одной статье «без месячного потолка», в другой «до 24 часов в сутки». Короче ~200 слов автосаммари не стартует. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP и https://support.plaud.ai/hc/en-us/articles/62437655066649-Automatic-transcription-and-summaries
9. Для живого чата лучший слой — официальный MCP. Для робота в Syncthing-волт и CRM — один поллер на официальном CLI, на одной бодрствующей машине. Шесть копий токена и неофициальный web API (Obsidian-плагин, plaud-tools, applaud) — серая зона, авторы сами пишут про риск бана.
10. Нативный HubSpot/CRM-коннектор не найден. Официальный сценарий CRM — промпт в Claude/ChatGPT, где рядом с Plaud подключён HubSpot. Это ручной ход ассистента, не ночной робот. https://eu.plaud.ai/blogs/news/plaud-mcp-for-chatgpt-and-claude

## 1. Официальные поверхности

Две разные двери. Их нельзя смешивать.

**Личный аккаунт (то, что нужно флоту).** Поддержка, обновлено 7 сентября 2026: «Plaud does not offer a public API today» и «no API key, no waiting list». Вместо ключа — MCP и CLI. Если нужен продукт поверх железа Plaud — это отдельно Plaud Embedded, доступ просят на dev.plaud.ai. https://support.plaud.ai/hc/en-us/articles/60726890231449-How-can-I-get-API-access-to-my-Plaud-data

GA MCP и CLI — 12 мая 2026, пакеты `@plaud-ai/mcp` и `@plaud-ai/cli`. https://docs.plaud.ai/plaud-mcp-cli/changelog.md

Страница npm `@plaud-ai/cli` в этом заходе не открылась (Cloudflare «Just a moment»). Версию с реестра npm поэтому не цитирую.

### CLI `@plaud-ai/cli`

Нужны Node.js ≥ 20 и аккаунт Plaud. Установка: `npm install -g @plaud-ai/cli`.

Команды, как в доке:

- `plaud login` / `logout` / `me`
- `plaud files` (страница 1–1000, размер 10–100, по умолчанию 20)
- `plaud recent` (7 дней) и `plaud recent --days N`
- `plaud today`
- `plaud search` — «Client-side keyword search (case-insensitive) against recording names. Scans up to 500 most recent recordings.» Фильтры `--from`, `--to`, `--max` (по умолчанию 50)
- `plaud file <id>`, `plaud audio <id>` («24-hour download URL»), `plaud transcript <id>`, `plaud summary <id>` (Markdown), флаги `-o`
- `plaud version`, `plaud update`

Коды выхода: 0 успех, 1 аргументы, 2 auth (`plaud login`), 3 сеть, 4 таймаут. Ошибки идут в stderr.

Конфиг: токены «stored at `~/.plaud/tokens.json`». Опционально `~/.plaud/cli.yaml` с `api_base: "https://platform.plaud.ai/developer/api"` и `timeout: 30000`. Переменные поверх файла: `PLAUD_API_BASE`, `PLAUD_CLIENT_ID` / `PLAUD_CLI_CLIENT_ID`, `PLAUD_CLIENT_SECRET`, `PLAUD_AUTH_URL`, `PLAUD_TOKEN_URL`, `PLAUD_REFRESH_URL`. Срок жизни access/refresh в этой доке не назван. При «Token refresh errors» совет: удалить `tokens.json` и залогиниться снова. https://docs.plaud.ai/plaud-mcp-cli/cli

Поддержка добавляет: CLI «works on a Mac mini or home server», команды «include them in scripts or scheduled jobs», Windows / Mac / Linux, и CLI «currently free for all active Plaud accounts». Чего CLI не умеет: писать аудио, править записи и стенограммы, быть чат-ассистентом. https://support.plaud.ai/hc/en-us/articles/57751026815257-Plaud-CLI

### MCP `@plaud-ai/mcp`

Установка одной командой: `npx -y @plaud-ai/mcp@latest install`. Флаг `--no-login` — «Skip the browser sign-in step (useful on remote or headless machines)». Ручной stdio-конфиг: `npx -y @plaud-ai/mcp@latest`. Для HTTP-клиентов URL из поддержки: `https://mcp.plaud.ai/mcp`, аутентификация OAuth. https://docs.plaud.ai/plaud-mcp-cli/mcp и https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP

Инструменты:

| Инструмент | Что отдаёт |
| --- | --- |
| `login` / `logout` | браузерный OAuth / отзыв |
| `get_current_user` | аккаунт |
| `list_files` | список; `query` по имени, `date_from` / `date_to` как `YYYY-MM-DD`, `page` / `page_size` |
| `get_file` | метаданные плюс `presigned_url` («valid 24 hours»), `source_list` (сегменты с таймкодами и спикерами), `note_list` (Markdown) |
| `get_note` | саммари, action items, ключевые темы |
| `get_transcript` | стенограмма с таймкодами и ярлыками спикеров |

Важная дыра пагинации: у `list_files` «Pagination (ignored when filters are set)». Фильтр по дате или слову и постраничный обход вместе не работают. https://docs.plaud.ai/plaud-mcp-cli/mcp

Поля списка: `id`, `name`, `created_at`, `start_at`, `duration` (миллисекунды), `serial_number`.

Скиллы — не отдельные API, а инструкции клиенту: `plaud-browse`, `plaud-find`, `plaud-read`, `plaud-digest`, `plaud-followup`, `plaud-export` («save to Notion», «post to Slack»). Notion и Slack тут делает сам ассистент, не Plaud.

Codex Desktop в таблице «Auto-configured». Снятие: `plaud-mcp unsetup codex`. Claude Code: `npx -y @plaud-ai/mcp clean-plugin`, затем `/plugin install plaud`. Токены MCP: при ошибке refresh удалить `~/.plaud/tokens-mcp.json`. Удалённый логин: «forward port 8199 first: `ssh -L 8199:localhost:8199 user@host`». https://docs.plaud.ai/plaud-mcp-cli/mcp

Поддержка: MCP «read-only», «can't generate new transcripts or summaries», «returns all AI summaries that have already been generated», включая кастомные шаблоны. «Plaud MCP is currently free for all active Plaud accounts.» Нужен Plaud Cloud Sync. Новая запись видна, «as soon as the recording has been uploaded … and its transcript and summary have been generated». Аудио ассистент тоже может забрать: «temporary download link (valid for 24 hours)». Коннектор Claude «tied to your account, not the device» и после одного клика работает в web, desktop, mobile и Claude Code. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP

HTTP-режим: «your recording data passes through Plaud's MCP server (hosted in the US)». Plaud пишет, что после запроса данные не хранит. https://docs.plaud.ai/plaud-mcp-cli/mcp

### Лимит 20 записей — это не CLI

Статья поддержки: «The Plaud OAuth API intentionally returns only your 20 most recent recordings.» Это «limited-access beta», «not open to new sign-ups», «no confirmed timeline» на снятие лимита. Следующий абзац: полную историю брать через MCP или CLI, «Neither is subject to the 20-recording limit». https://support.plaud.ai/hc/en-us/articles/59291709269401-Why-doesn-t-the-Plaud-API-return-all-of-my-recordings

Итого три «API», и все называются похоже:

- OAuth API беты — 20 последних, закрыт для новых.
- CLI/MCP — OAuth пользователя на `platform.plaud.ai/developer/api`, чтение, пагинация до 100×1000 в CLI, но поиск только 500 имён.
- Embedded partner API — чужой продукт, свои ключи.

### Plaud Embedded / platform.plaud.ai

Индекс доки делит платформу на Embedded (SDK + Transcription API) и «Plaud MCP & CLI». https://docs.plaud.ai/llms.txt

Живая страница Transcription API описывает poll, не вебхук: POST `https://platform-us.plaud.ai/developer/api/open/partner/ai/transcriptions/` с заголовками `X-Client-Api-Key` и `X-Client-Id`, тело `file_url` и `params` (`language`, модель `plaud-fast-whisper`, VAD, `diarization.enabled`). Ответ: `transcription_id`, статус `PENDING`. Дальше GET того же пути, пока не `SUCCESS`, и тогда в `data` есть `text`, `language`, `duration`, `segments[]` со `speaker`. Записи длиннее 5 часов надо резать. «Plaud's cloud services are hosted in the U.S. by default.» Мультирегион (Japan, Europe, Singapore) — «please reach out to our sales team». https://docs.plaud.ai/documentation/embedded_sdk/transcription_api

Краткий старт Embedded: портал `https://portal.plaud.ai/`, Client ID и Secret для user token и привязки устройств, отдельный API Key «NOT your client_secret». https://docs.plaud.ai/documentation/embedded_sdk/quickstart

Поисковый индекс старой страницы `https://docs.plaud.ai/documentation/get_started/quickstart` содержал вебхук `audio_transcribe.completed` и лимиты 10 запросов/мин на partner token и 60 qpm на транскрипцию. Открытая сегодня по этому URL страница — другой Embedded quickstart, без вебхука и без этих цифр. Считать вебхук партнёрского API живым по этому заходу нельзя. В индексе llms.txt у транскрипции указан poll: «Keep polling while the status is PENDING…». https://docs.plaud.ai/llms.txt

### Планы: CLI бесплатен, текст — нет

MCP и CLI не требуют Pro. Минуты транскрипции — да.

Открытая статья минут (21 сентября 2026): Starter «300 free minutes per month» после привязки устройства; Pro «1,200 minutes per month»; Unlimited «Unlimited transcription with no monthly cap.» Минуты не переносятся. Запись и Private Cloud Sync работают и при нуле минут, транскрипция встаёт. Удаление файла минуты не возвращает. https://support.plaud.ai/hc/en-us/articles/11177116525455-Transcription-minutes

Другая открытая статья (12 августа 2026) говорит иначе про Unlimited: «up to 24 hours of transcription per day». Pro там же: «1,200 minutes (20 hours) each month, plus … Ask Plaud and custom summary templates.» Extra Minutes: пакеты 600 / 3 000 / 6 000, живут 24 месяца, тратятся после плана. https://support.plaud.ai/hc/en-us/articles/11476636337295-What-s-the-difference-between-Extra-Minutes-and-the-Starter-Pro-and-Unlimited-plans

Обе формулировки официальные. Практический смысл: месячного потолка у Unlimited нет, суточный потолок 24 часа в другой статье есть. Какая побеждает при споре — в открытых страницах не сказано.

Цены с витрины, которую отдала страница политики (США): Pro $17.99/мес или $99.99/год, 1 200 мин; Unlimited $29.99/мес или $239.99/год. https://www.plaud.ai/policies/privacy-policy Европейская витрина на открытой статье про MCP показывает другие цифры в евро (Pro €19.99/мес, €110.99/год; Unlimited €34.99/мес, €224.99/год). Цену надо брать со своего регионального магазина. https://eu.plaud.ai/blogs/news/plaud-mcp-for-chatgpt-and-claude

Автопоток: «Auto-summarize new recordings» включён по умолчанию и стартует, когда запись уже в облаке. Порог: «more than 200 words (about 5 minutes of speech or longer)». Короче — файл синкается, текст ждёт ручного Generate. Спикеры: «Auto speaker labeling: on by default.» Минуты автопотока списываются так же, как ручные. Правка AutoFlow — только в мобильном приложении, не в Web. https://support.plaud.ai/hc/en-us/articles/62437655066649-Automatic-transcription-and-summaries

### Регионы данных личного аккаунта

Статья безопасности: EU/EEA — AWS Frankfurt, Северная и Южная Америка — AWS US-West Oregon, Asia Pacific — AWS Singapore, Japan — AWS Japan. Записи, стенограммы и саммари лежат, «until the user manually deletes them or cancels their account». При выключенном Cloud Sync «recordings stay on-device; cloud used only on-demand for AI processing». https://support.plaud.ai/hc/en-us/articles/57744166222489-Plaud-s-security-and-compliance

Политика от 16 июня 2026: «We operate servers in the United States, Germany, Japan, and Singapore.» Для ЕС — SCC, представитель Prighter, портал `https://app.prighter.com/portal/plaud`. Аудио не идёт в обучение моделей «unless we have obtained your explicit and informed consent». https://www.plaud.ai/policies/privacy-policy

Отдельно: локальный stdio-MCP ходит в API Plaud с машины. HTTP-коннектор Claude/ChatGPT гоняет содержимое через MCP-сервер в США, даже если аккаунт европейский. https://docs.plaud.ai/plaud-mcp-cli/mcp

### Форматы

Экспорт из приложения, статья от 22 сентября 2026: аудио MP3 и WAV (на Web «WAV … Plaud Web supports MP3 only»); стенограмма TXT, SRT, DOCX, PDF, с тумблерами таймкодов и спикеров; саммари TXT, Markdown, DOCX, PDF; mind map PNG или Markdown. https://support.plaud.ai/hc/en-us/articles/50835453223705-Export-recordings-transcripts-and-summaries

Что именно лежит за 24-часовой `presigned_url` CLI/MCP (ogg, opus, mp3) — в официальных CLI/MCP страницах не сказано.

### Вебхук «новая запись»

У CLI и MCP — не найден. Поддержка описывает появление записи как «спросите ассистента ещё раз», то есть pull. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP

Официальный push личного аккаунта — Zapier: «two trigger events: transcripts generated and summaries generated.» «Plaud is only available as a trigger.» Командный воркспейс: коннектор видит только файлы, которыми владеете вы, не файлы коллег. https://support.plaud.ai/hc/en-us/articles/12200669941647-Zapier-integration

Лимиты запросов именно CLI/MCP — не найдены ни в CLI-доке, ни в MCP-доке, ни в статьях поддержки, которые открыты выше.

## 2. Чужие обвязки

Официальный путь read-only. Всё, что пишет в аккаунт, ходит в закрытый web API (`api.plaud.ai`), не в developer API.

| Имя | URL | Тип | Свежесть, как на открытой странице | Что даёт | Что ломается |
| --- | --- | --- | --- | --- | --- |
| `@plaud-ai/cli` | https://docs.plaud.ai/plaud-mcp-cli/cli | официальный CLI | GA 12 мая 2026, дока живая | логин, список, поиск по имени, транскрипт, markdown-саммари, 24ч аудио-URL | нет bulk-sync, нет правки, поиск только 500 имён |
| `@plaud-ai/mcp` | https://docs.plaud.ai/plaud-mcp-cli/mcp | официальный MCP | та же GA, поддержка обновлена 22 сен 2026 | 7 read-only tools, скиллы, Claude/Codex/ChatGPT | нет вебхука, пагинация отключается фильтрами, HTTP идёт через США |
| Zapier | https://support.plaud.ai/hc/en-us/articles/12200669941647-Zapier-integration | официальный интегратор | статья 28 авг 2026 | push на готовый транскрипт и саммари в 7000+ приложений | только триггер; не аудио-arrived; не прямой Obsidian |
| `plaudit-cli` | https://lib.rs/crates/plaudit-cli | Rust-порт официального CLI | релиз 0.1.4 от 5 июня 2026 | тот же OAuth+PKCE, порт 8199, общий `~/.plaud/tokens.json`, команда `sync` в Markdown `YYYY-MM-DD_slug.md`, уже лежащие файлы пропускает | молодой порт, не Plaud |
| `danielgwilson/plaud` (npm `plaud`) | https://github.com/danielgwilson/plaud | неофициальный экспортёр | 33 коммита на открытой странице; npm-версия со страницы не снята | JSON, ZIP, bulk, локальный store, дедуп, `--audio-format opus`, скилл агента | bearer и приватные web-эндпоинты; «may break»; «may reject non-browser request fingerprints» |
| `sergivalverde/plaud-toolkit` | https://github.com/sergivalverde/plaud-toolkit | TS core+CLI+MCP+Obsidian | 9 коммитов, в истории поиска последний видимый слой — июнь 2026, альфа | login email/пароль, регион us/eu, sync в папку, локальный mlx-whisper на Apple Silicon | сам README: «no official API»; токены «~300 days» — это их замер web API, не официальный OAuth |
| `ckelsoe/obsidian-plaud-importer` | https://community.obsidian.md/plugins/plaud-importer | плагин Obsidian | версия 0.42.3, «Last updated 4 hours ago» на 24 сен 2026, 1k загрузок, 82 релиза | один md на запись, frontmatter, автосинк 15 мин–сутки, вложения, опциональное аудио | «undocumented, reverse-engineered» `GET /file/simple/web`, `POST /ai/transsumm/{id}`, `GET /file/detail/{id}`; десктоп; сессия не read-only ключ |
| `massive-value/plaud-tools` | https://github.com/massive-value/plaud-tools | Python CLI+MCP+трей | 218 коммитов; топик GitHub обновлён 24 сен 2026 | запись: переименование, папки, правка стенограммы, загрузка аудио, merge | «Terms of Service may restrict; your account could be rate-limited or suspended»; сессия «about 30 days» |
| `rsteckler/applaud` | https://github.com/rsteckler/applaud | локальный сервер + n8n | топик обновлён 12 июля 2026, инсталлер v0.5.11 | poll раз в 10 минут, диск, вебхуки `audio_ready` и `transcript_ready`, шаблоны n8n | куки браузера `pld_ut` (~1 день) и `pld_urt` (~30 дней); на Windows куки с диска не читаются |
| Plaud Embedded | https://docs.plaud.ai/documentation/embedded_sdk/transcription_api | партнёрский API | дока живая | транскрипция чужого аудио, diarization-флаг, модель `plaud-fast-whisper` | не ваш личный архив; US по умолчанию; доступ через портал |

Ещё имена с открытого списка топика https://github.com/topics/plaud (33 репозитория, страницу открыл, README каждого не читал): `jgatlit/plaude-api` (конвейер в Obsidian, обновлён 15 сен 2026), `teribuilder/plaud-to-notion` (поллинг на Маке «без Make/Zapier», 8 авг 2026), `leegonzales/plaud-cli` (тонкий клиент поверх официального MCP, Rust, 27 июня 2026), `Namraks-Labs/plaud-mcp` (локальный MCP в markdown, 28 мая 2026), `RemiPelloux/OpenPlod` (локальный Bluetooth-волт Note Pro, Mistral/Voxtral, свой MCP, 23 сен 2026), `dans-huang/PinPoint` (синк и handoff в Codex или Claude на Apple Silicon, 8 сен 2026), `yetiswang/plaudio` (локальные ярлыки спикеров, mlx-whisper и pyannote, 18 июня 2026). Это указатели, не разобранные пайплайны.

Официального узла n8n у Plaud на открытых страницах нет. Make как официальная интеграция не найден. Zapier — единственная официальная шина.

## 3. Как люди реально стыкуют

### Obsidian

Самый живой путь на 24 сентября 2026 — плагин Plaud Importer. Каждая запись становится одной заметкой: YAML (`plaud-id`, дата, начало и конец, длительность, спикеры, теги, URL), саммари Plaud, транскрипт с главами, папка `<note>-assets`. Шаблон подпапки в стиле Moment, например `{{YYYY-MM}}`. Имя по умолчанию `YYYY-MM-DD <title>`. Автосинк выключен по умолчанию, интервал от 15 минут до раза в сутки. Повторный импорт затирает заметку, если в Plaud что-то изменилось. Аудио выключено по умолчанию: «roughly 15 MB per hour». Запись короче обработки: плагин пишет заглушку, пока в Plaud нет текста. Регион EU плагин определяет сам. Токен лежит в SecretStorage Obsidian, не в `data.json`, и не едет через Obsidian Sync — каждый волт логинится отдельно. https://community.obsidian.md/plugins/plaud-importer

Автор плагина прямо пишет, чего ждать: «Plaud does not offer an official API yet»; эндпоинты неофициальные; «The plugin may break without warning». Единственная запись обратно в Plaud — заголовок, и только если включить write-back. Пароль в плагин не вводится: логин на сайте Plaud, плагин забирает сессию. Сессия email «renews itself … for about 30 days»; Google/Apple короче и чаще просит войти снова.

`plaud-toolkit` кладёт заметки в `Plaud/Notes`, аудио в `Plaud/Audio`, синк раз в 60 минут, локальный mlx-whisper только на Apple Silicon, если серверной стенограммы нет. Логин — email, пароль и регион `us`/`eu` в `~/.plaud/config.json`. Это не официальный `tokens.json`. https://github.com/sergivalverde/plaud-toolkit

`plaudit sync ~/plaud-export` пишет один markdown на запись и пропускает уже существующие файлы. Это ближайший готовый инкремент к официальному OAuth, но пакет чужой. https://lib.rs/crates/plaudit-cli

### Claude / ChatGPT / Codex

Официальный путь чата: коннектор. Claude — Customize / Settings → Connectors → Plaud Web MCP → Authorize. ChatGPT на странице поддержки: Plugins → Plaud; на европейской статье от 22 сентября 2026 уже «Apps» → Plaud → Connect. Codex, Claude Code, Cursor — `npx -y @plaud-ai/mcp@latest install`, потом полный перезапуск, не просто закрыть окно. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP и https://eu.plaud.ai/blogs/news/plaud-mcp-for-chatgpt-and-claude и https://docs.plaud.ai/plaud-mcp-cli/mcp

CRM в этой европейской статье — не коннектор Plaud→HubSpot, а промпт: «Update my Salesforce with the key outcomes…» и фраза «Connect HubSpot or Linear alongside Plaud». Ассистент читает транскрипт Plaud и сам пишет в CRM, если CRM у него уже подключён. Это живая сессия, не cron.

`plaud-tools` ставит MCP в Claude Desktop, Claude Code и Codex и умеет то, чего официальный слой не умеет (переименовать, поправить стенограмму, загрузить файл). Автор сравнивает: официальные инструменты «expose three endpoints — list recordings, get one recording, get the current user» и «cannot change anything». https://github.com/massive-value/plaud-tools

### CRM / Notion

Notion без кода: Plaud Web → Explore → Integrations → Zapier. Триггер — готовый транскрипт или саммари. Бесплатного плана Zapier достаточно, чтобы начать. https://support.plaud.ai/hc/en-us/articles/12200669941647-Zapier-integration

Прямого «Plaud → ваша leads.db» нет. Скилл `plaud-export` в официальном MCP — это просьба ассистенту «save to Notion» / «post to Slack», не серверный коннектор. https://docs.plaud.ai/plaud-mcp-cli/mcp

## 4. Как ловят новые записи без человека

Цепочка, которую складывают открытые доки, без обещанной цифры задержки:

1. Запись должна попасть в облако. Private Cloud Sync: «recordings upload automatically after capture»; есть тумблер «Sync on Wi-Fi only». Облако безлимитное для активированного аккаунта. https://support.plaud.ai/hc/en-us/articles/50835508850841-Private-Cloud-Sync
2. NotePin умеет отдельный путь: Wi-Fi только 2.4 ГГц, кнопка «Sync to cloud while charging», устройство заряжается и не пишет. Без включённого Private Cloud Sync не поедет. Статья говорит про NotePin после прошивки, не про Note / Note Pro. https://support.plaud.ai/hc/en-us/articles/11681029415439-How-to-enable-Sync-to-cloud-while-charging-transfer
3. Политика описывает Desktop: после конца записи файл уходит в Private Cloud; автозапись может смотреть, запущено ли приложение встречи, и это происходит на машине, без разбора содержимого встречи. Папки-сторожа «смотри Downloads» в открытых доках нет. https://www.plaud.ai/policies/privacy-policy
4. Текст. Автопоток ждёт облако и порог ~200 слов. Пока текста нет, MCP «не видит» полезную запись, даже если аудио уже лежит. https://support.plaud.ai/hc/en-us/articles/62437655066649-Automatic-transcription-and-summaries и https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP
5. Дальше только pull, если не Zapier. Поддержка CLI прямо разрешает scheduled jobs. Готовой команды `plaud sync` у официального CLI нет — её надо написать поверх `plaud recent` / `plaud files`. https://support.plaud.ai/hc/en-us/articles/57751026815257-Plaud-CLI

Чужие поллеры называют свои интервалы: плагин Obsidian от 15 минут, applaud «every 10 minutes (configurable)». Это их выбор, не SLA Plaud. Задержка «конец записи → запись видна в API» в минутах или секундах на открытых официальных страницах не найдена.

Zapier ближе всех к push, но событие — «транскрипт или саммари готовы», не «файл лёг на устройство».

## 5. Несколько машин

Что сказано точно:

- CLI живёт на Windows, Mac и Linux. https://support.plaud.ai/hc/en-us/articles/57751026815257-Plaud-CLI
- Токены CLI и MCP — разные файлы в одном каталоге `~/.plaud/`. Удаление `rm -rf ~/.plaud` сносит оба. https://docs.plaud.ai/plaud-mcp-cli/cli и https://docs.plaud.ai/plaud-mcp-cli/mcp
- Headless: `--no-login`, затем логин через проброс порта 8199. Тот же порт использует `plaudit`. https://docs.plaud.ai/plaud-mcp-cli/mcp и https://lib.rs/crates/plaudit-cli
- Веб-коннектор Claude привязан к аккаунту, не к устройству. Локальный stdio-MCP — к конфигу конкретной машины. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP
- `plaudit` пишет, что его `tokens.json` «is the same config directory used by Plaud's official CLI, so credentials interoperate». Это совместимость двух программ на одной машине, не лицензия на шесть копий. https://lib.rs/crates/plaudit-cli

Чего в доке нет: можно ли один refresh-token скопировать на шесть машин, ротируется ли refresh при использовании, какой TTL у официального access token, гасится ли сессия при втором `plaud login`. При ошибке refresh официальный совет один: удалить файл и войти снова. Значит, общий файл на Syncthing опасен: одна машина «починит» токен и сломает остальные, а сам файл уедет во все узлы.

Неофициальные сроки (не переносить на официальный OAuth): plaud-tools «about 30 days»; applaud `pld_ut` около суток и `pld_urt` около 30 дней; plaud-toolkit заявляет «~300 days». https://github.com/massive-value/plaud-tools и https://github.com/rsteckler/applaud и https://github.com/sergivalverde/plaud-toolkit

VPS без браузера официальный MCP поднимает (`--no-login` + ssh -L 8199 один раз). Дальше stdio-процесс должен жить там, где сидит агент. Агент на ноутбуке не увидит MCP, запущенный только на VPS, если не пробросить сам MCP. HTTP `https://mcp.plaud.ai/mcp` как раз для клиентов без локального процесса, но трафик записей идёт через США.

## 6. Грабли и риски

**Качество стенограммы против своего Whisper large-v3.** Сравнения на открытых официальных страницах нет. Официально есть ярлыки спикеров (авто по умолчанию) и модель партнёрского API с именем `plaud-fast-whisper`. https://support.plaud.ai/hc/en-us/articles/62437655066649-Automatic-transcription-and-summaries и https://docs.plaud.ai/documentation/embedded_sdk/transcription_api

Независимые цифры с Reddit в этом заходе не подтверждены: страница треда не открылась. Косвенный сигнал, что людям не хватает облачного текста: plaud-toolkit и репозиторий `yetiswang/plaudio` гоняют локальный mlx-whisper, второй ещё и pyannote. Это выбор авторов, не замер Plaud. https://github.com/sergivalverde/plaud-toolkit и https://github.com/topics/plaud

**Короткие записи.** Меньше ~200 слов автотекст не стартует. Робот, который ждёт саммари, будет вечно ждать пятиминутный созвон. https://support.plaud.ai/hc/en-us/articles/62437655066649-Automatic-transcription-and-summaries

**Согласие на запись.** У Plaud нет таблицы «one-party / two-party». Есть одна фраза: «Before capturing a conversation, it's good practice to give others a quick heads-up. Local regulations differ». https://support.plaud.ai/hc/en-us/articles/57744166222489-Plaud-s-security-and-compliance Та же мысль в подвале витрины. https://www.plaud.ai/policies/privacy-policy Юридический разбор Калифорнии (всестороннее согласие) в этом заходе на первоисточник закона не открывался. Для флота это значит: Plaud не блокирует запись без согласия второй стороны. Ответственность на том, кто нажал кнопку.

**GDPR.** Для аккаунта EU/EEA хранение — Франкфурт, плюс SCC и представитель в ЕС. Это хранение аккаунта, не путь HTTP-MCP: тот сервер в США, и через него проходит текст записи в момент запроса. https://support.plaud.ai/hc/en-us/articles/57744166222489-Plaud-s-security-and-compliance и https://docs.plaud.ai/plaud-mcp-cli/mcp Партнёрская транскрипция Embedded по умолчанию тоже в США, Европа — через отдел продаж, не галочка в личном кабинете. https://docs.plaud.ai/documentation/embedded_sdk/transcription_api

**ToS и бан.** Дословный пользовательский договор с `https://web.plaud.ai/user-agreement` в этом заходе не скачался (страница вернула пустую оболочку). Поисковый индекс этой страницы содержит жёсткий запрет на автоматических агентов и на вытаскивание bearer-токенов. Цитатой это не подтверждаю. Что подтверждено открытыми README: plaud-tools пишет, что web API «may restrict» и аккаунт «could be rate-limited or suspended»; плагин Obsidian говорит, что соблюдение ToS — ваша забота. Официальные CLI и MCP поддержка сама предлагает для скриптов и ассистентов. Серая зона — не они, а обратный web API. https://github.com/massive-value/plaud-tools и https://community.obsidian.md/plugins/plaud-importer

**Сбои.** Отдельной статус-страницы в открытых материалах нет. Поддержка MCP советует при 500 проверить сеть и переподключить коннектор. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP

**Прочие мины:**

- Поиск CLI не ищет по тексту стенограммы, только по имени, и только среди 500 последних. https://docs.plaud.ai/plaud-mcp-cli/cli
- Фильтрованный `list_files` не листается. https://docs.plaud.ai/plaud-mcp-cli/mcp
- 24-часовая ссылка протухает. Робот должен скачать аудио сразу, а не сохранить URL. https://support.plaud.ai/hc/en-us/articles/57751026815257-Plaud-CLI
- Два официальных файла токенов в одном каталоге. Снос каталога гасит и чат, и робота.
- Плагин Obsidian при ресинке затирает местные правки заметки. https://community.obsidian.md/plugins/plaud-importer

## 7. Что выбрать

| Задача | Слой | Почему |
| --- | --- | --- |
| Живой чат на ноутбуке (Claude Desktop / Claude Code / Codex) | официальный stdio MCP | Поддержан, read-only, Codex в списке автоустановки, токен локальный. HTTP-коннектор claude.ai удобнее между устройствами, но гоняет текст через США |
| Спросить «что решили во вторник» из браузера | Plaud Web MCP / плагин ChatGPT | Один OAuth на аккаунт, без Node. Не робот |
| Ночной робот → общий волт Syncthing → карточка CRM | официальный CLI на одной машине | Поддержка прямо разрешает scheduled jobs и машину без монитора. Идемпотентность по `id` дописываете вы: у CLI нет `sync` |
| Push, когда саммари готово, в Notion/почту/вебхук Zapier | официальный Zapier | Единственный официальный push. В ваш волт напрямую не кладёт |
| Свой продукт на чужих устройствах Plaud | Embedded, не личный CLI | Другие ключи, другой хост, poll транскрипции |
| Правка заголовков, папок, локальный Whisper, вебхук на свой n8n | чужой web API | Умеет больше и первый ломается. Для аккаунта с лидами я бы не ставил |

Не ставить MCP на все шесть машин «чтобы везде был контекст». Чату хватает MCP на тех машинах, где Антон разговаривает. Роботу хватает одного поллера.

## Архитектура на 6 машин

Один писатель. Остальные только читают волт.

**Кто поллит.** Одна машина, которая не спит и имеет браузер хотя бы для первого логина. Ноутбук плох: крышка, сон, другой часовой пояс. VPS годится после одного `ssh -L 8199`. Хаб Windows тоже годится: CLI официально на Windows. Второго поллера нет.

**Где токен.** Только `~/.plaud/tokens.json` на этой машине, права `0600`. Каталог не внутри волта и не в Syncthing. В git не класть. На остальных пяти машинах этого файла нет. Если на ноутбуке нужен ещё и чат, у него свой `~/.plaud/tokens-mcp.json` от отдельного `plaud login` / MCP install. Не копировать файл с VPS на ноутбук «чтобы не логиниться».

**Первый вход.** На поллере: Node ≥ 20, `npm install -g @plaud-ai/cli`, `plaud login`. Если браузера нет — проброс 8199, URL из терминала открыть на машине с браузером. Проверка: `plaud me`, затем `plaud today`.

**Цикл.** Раз в 10–15 минут, не чаще: смысл не в секундах, а в том, что транскрипция и так ждёт облако и порог слов.

1. `plaud recent --days 3` (или постраничный `plaud files`).
2. Для каждого `id`, которого нет в журнале, `plaud file <id>`.
3. Если `transcript` и `summary` ещё false — не писать финальную заметку. Записать id в очередь «аудио есть, текста нет» и выйти. Иначе короткие созвоны зависнут, а длинные придут вторым проходом.
4. Когда текст готов: `plaud summary` и `plaud transcript` в файлы, `plaud audio` скачать сразу.
5. Собрать одну заметку и положить в волт атомарно (временный файл → rename). Syncthing подхватит.

**Идемпотентность.** Ключ — `id` Plaud, не заголовок. Журнал `id → sha256 тела` рядом с поллером, не в синкаемой папке заметок. Повторный прогон тот же id не создаёт вторую заметку. Если саммари в Plaud пересобрали, меняется хеш — тогда одна перезапись, не новый файл. Заголовок в имени файла не использовать как ключ: переименование в приложении иначе даст дубль.

**Имена.** `Calls/2026/09/2026-09-24_<slug>__<id>.md` плюс `Calls/2026/09/2026-09-24_<slug>__<id>.mp3` (или тот контейнер, который реально отдал URL — расширение проверить по заголовку ответа, официальный формат URL не документирован). В YAML: `plaud_id`, `start_at`, `duration_ms`, `serial_number`, `source: plaud`, `transcript: yes|no`. Slug — усечённое имя, без слэшей.

**CRM.** Поллер не логинится в CRM и не шлёт письма. Он кладёт заметку в папку, которую уже ест ваш конвейер «recap → карточка → черновик». Черновик остаётся черновиком. Zapier в HubSpot имеет смысл только если нужен push без своей машины; тогда волт и CRM разъедутся, и появится второй писатель. Для одного источника правды Zapier не включать.

**Час.** На ноутбуках, где идёт живой Claude или Codex: официальный MCP, вопрос «достань вчерашний созвон». Они читают Plaud напрямую и не пишут в волт. Иначе два писателя перетрут друг друга.

**Чего не делать.** Не ставить obsidian-plaud-importer в синкаемый волт на нескольких машинах: у каждой свой токен, обе будут писать одни и те же заметки. Не включать plaud-tools на аккаунте, с которого кормится CRM. Не класть аудио всех встреч в волт без нужды: плагин оценивает аудио примерно в 15 МБ на час, Syncthing на шести узлах это умножит.

## Что не задокументировано

Искал в открытых страницах CLI, MCP, changelog, поддержки (CLI, MCP, API access, лимит 20, Zapier, минуты, два тарифа Unlimited, Cloud Sync, зарядка NotePin, автосаммари, безопасность, экспорт), политики, Embedded transcription quickstart, llms.txt, евростатьи про Claude.

- Срок жизни официального access и refresh токена CLI/MCP — не найден. Есть только «при ошибке refresh удали файл».
- Можно ли один `tokens.json` на нескольких машинах одновременно — не найден.
- Лимит запросов в минуту у CLI/MCP — не найден.
- Вебхук «новая запись загружена» для личного аккаунта — не найден. Есть Zapier на готовый текст.
- Живой вебхук Embedded `audio_transcribe.completed` — на открытой сегодня странице quickstart его нет; в поиске он был у старого индекса. Сейчас подтверждён poll.
- Формат байтов за `plaud audio` / `presigned_url` — не найден (у чужого applaud на диске `audio.ogg`, это не дока Plaud).
- Задержка от стоп-кнопки до видимости в API, в минутах — не найдена.
- Папка-сторож десктоп-приложения — не найдена.
- Нативный HubSpot, Salesforce, ваша CRM — не найдены. Есть промпт «подключи HubSpot рядом».
- Официальный n8n и Make — не найдены.
- Таблица one-party / two-party и отдельный запрет записи в Калифорнии внутри доков Plaud — не найдена, только общий совет предупредить.
- Замер Plaud против Whisper large-v3 — не найден.
- Статус-страница аварий — не найдена.
- Победитель спора «Unlimited без месячного потолка» против «до 24 часов в сутки» — не найден. Обе статьи открыты.
- Дословный текст запрета автоматических агентов с web.plaud.ai/user-agreement — страница в этом заходе не отдала тело.

## Все URL, которые реально открыты

1. https://docs.plaud.ai/llms.txt
2. https://docs.plaud.ai/plaud-mcp-cli/mcp
3. https://docs.plaud.ai/plaud-mcp-cli/cli
4. https://docs.plaud.ai/plaud-mcp-cli/changelog.md
5. https://docs.plaud.ai/documentation/embedded_sdk/transcription_api
6. https://docs.plaud.ai/documentation/embedded_sdk/quickstart
7. https://docs.plaud.ai/documentation/get_started/quickstart
8. https://support.plaud.ai/hc/en-us/articles/57751078986265-Plaud-MCP
9. https://support.plaud.ai/hc/en-us/articles/57751026815257-Plaud-CLI
10. https://support.plaud.ai/hc/en-us/articles/60726890231449-How-can-I-get-API-access-to-my-Plaud-data
11. https://support.plaud.ai/hc/en-us/articles/59291709269401-Why-doesn-t-the-Plaud-API-return-all-of-my-recordings
12. https://support.plaud.ai/hc/en-us/articles/12200669941647-Zapier-integration
13. https://support.plaud.ai/hc/en-us/articles/11177116525455-Transcription-minutes
14. https://support.plaud.ai/hc/en-us/articles/11476636337295-What-s-the-difference-between-Extra-Minutes-and-the-Starter-Pro-and-Unlimited-plans
15. https://support.plaud.ai/hc/en-us/articles/62437655066649-Automatic-transcription-and-summaries
16. https://support.plaud.ai/hc/en-us/articles/50835508850841-Private-Cloud-Sync
17. https://support.plaud.ai/hc/en-us/articles/11681029415439-How-to-enable-Sync-to-cloud-while-charging-transfer
18. https://support.plaud.ai/hc/en-us/articles/57744166222489-Plaud-s-security-and-compliance
19. https://support.plaud.ai/hc/en-us/articles/50835453223705-Export-recordings-transcripts-and-summaries
20. https://www.plaud.ai/policies/privacy-policy
21. https://eu.plaud.ai/blogs/news/plaud-mcp-for-chatgpt-and-claude
22. https://github.com/sergivalverde/plaud-toolkit
23. https://github.com/danielgwilson/plaud
24. https://github.com/massive-value/plaud-tools
25. https://github.com/rsteckler/applaud
26. https://github.com/topics/plaud
27. https://community.obsidian.md/plugins/plaud-importer
28. https://lib.rs/crates/plaudit-cli

Открывались и почти ничего не отдали, поэтому из них нет цитат: https://www.npmjs.com/package/@plaud-ai/cli (Cloudflare), https://web.plaud.ai/user-agreement (пустая оболочка), страница Reddit треда Note vs Note Pro.

Покрытие: официальные CLI/MCP/Embedded, поддержка, политика, Zapier, четыре чужих репозитория целиком и список топика. Не открыты построчно README `plaude-api`, `plaud-to-notion`, OpenPlod, PinPoint — они только в списке топика. Договор web.plaud.ai дословно не снят.

🎙 Майк: чат кормим официальным MCP на ноутбуке, волт пишет один CLI-поллер. Шесть копий токена и обратный web API на аккаунте с лидами — лишняя дыра.

Простыми словами: Plaud сам в твой дневник не стучится. Он как почтовый ящик: пока запись не доехала в их облако и они не напечатали текст, забирать нечего. Для разговора с Клодом ставим их официальную розетку. Для ночного робота, который раскладывает заметки по общему шкафу на шесть компьютеров, достаточно одного будильника на одном компьютере. Будильник смотрит ящик и кладёт новую бумажку в шкаф. Остальные компьютеры бумажку уже видят через синхронизацию. Ключ от ящика в шкаф не кладём, иначе он разъедется по всем машинам.
