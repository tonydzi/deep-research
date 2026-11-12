# DR26-11-04-HUB-02-1908 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

DR26-11-04-HUB-02-1908 — Альтернативы Terminum для многомашинного воркфлоу с AI-агентами

Дата исследования: 4–5 ноября 2026. Все утверждения проверены по первоисточникам, доступ к которым выполнен в эту дату; для быстроменяющихся тем указана дата конкретного источника.

Краткий ответ (Executive summary)

Terminum (v0.30.4, 4 ноября 2026) — всё ещё самый точный функциональный аналог самого себя для сценария «браузерная панель над существующим парком tmux-сессий на нескольких SSH-хостах»: ни один из проверенных конкурентов не сочетает одновременно (а) агрегированный список сессий с нескольких SSH-хостов, (б) полноценный браузерный терминал с реальным вводом с телефона/iPad, (в) attach к уже существующим сессиям, (г) файловую панель на удалённых хостах. Но проекту три недели (первый коммит 16 октября 2026), у него ~10 звёзд и фактически один автор — это решение с высоким bus-factor.

Существенная часть «посевного» списка изменилась или умерла: Vibe Kanban официально закрывается (последний релиз 24 апреля 2026); cmux переименован в Xum и использует собственный цикл агента (биллинг через API, не через подписки); Superset переименован в Paradev (macOS-first, мобильное приложение за Pro-подписку); Omnara сменил концепцию с «пульта Claude Code» на managed-платформу агентов с API-ключами — все они выпадают из прямого сравнения.

Самые сильные новые конкуренты, которых не было в списке: Paseo (getpaseo/paseo, Apache-2.0, 23,1 тыс. звёзд, self-hosted демон + нативные мобильные приложения с полной паритетностью), T3 Code (pingdotgg/t3code, MIT, 46,8 тыс. звёзд, «agent harness control surface» с веб/десктоп/iOS/Android), Herdr (herdrdev/herdr, Apache-2.0, 48,1 тыс. звёзд, мультиплексер с мультихостовостью и честной семантикой восстановления) и HAPI (tiann/hapi, AGPL-3.0, локальный хаб + E2E-шифрованный релей).

Официальные вендорские механизмы уже закрывают половину потребности: Claude Code Remote Control (входит в подписки Pro/Max/Ultra; API-ключи не поддерживаются) и Codex Remote Connections (ChatGPT-десктоп как хост, управление из мобильного приложения) дают ожидание-апрувов, push-уведомления и продолжение существующей сессии — без какой-либо новой инфраструктуры, но без единой панели по всем агентам.

Ни один инструмент не «возобновляет задачу» после ребута в строгом смысле. Все восстанавливают раскладку/историю; сами процессы агентов умирают. Herdr формально умеет «resume supported agent sessions» после рестарта (за счёт собственных механизмов резюме агентов) — это ближе всего к возобновлению, но и там «оригинальные процессы не выживают».

Рекомендация по сценариям: (1) панель над существующим парком — оставить Terminum (лид), запасной — официальный Remote Control/Remote Connections + tmux по SSH; (2) основная среда для новых параллельных задач — Paseo (лид), запасной — T3 Code, далее HAPI. Разные архитектуры решают разные потребности; единого победителя сознательно не назначено.

Главная неопределённость: все выводы о юзабилити (телефон, уведомления, скорость) — из документации и репозиториев, не из эксплуатации; перед любым решением нужен ограниченный параллельный пилот (план в разделе 9).

1. Вопрос

Найти программное обеспечение, функционально аналогичное Terminum (github.com/extractumio/terminum (https://github.com/extractumio/terminum)), и объяснить, какие варианты лучше всего подходят под анонимизированный воркфлоу: один технический владелец, много параллельных сессий Claude Code и OpenAI Codex на уже оплаченных подписках; рабочая станция Windows, Mac'и и always-on Linux-сервер; SSH и tmux уже есть, есть приватный tailnet; нужны браузерный интерфейс с десктопа/iPad/телефона, агрегированный список сессий по хостам, ясные состояния ожидания/апрува, доступ к удалённым файлам и живучесть работы при отключении клиента. Существующие скрипты уже дают планы/прогресс и заметки по сессиям; лишние per-session stdio-MCP-процессы нежелательны; миграция не авторизована. Это исследование — без установки, покупки и обращения к мейнтейнерам.

2. Методология и покрытие

Поиски: ~55 веб-поисков + ~25 открытий первоисточников (репозитории, релизы, SECURITY.md, официальные доки Anthropic/OpenAI, сайты проектов). Поиск начинался с широкого картирования, затем сужался до первичных источников; сторонние обзорные страницы использовались только как сигналы для поиска и явно помечены.

Приоритет первоисточников: факты о функциях, лицензиях, ценах и поддержке берутся из репозиториев, официальных доков и релиз-нотов. Сторонние обзоры (continuumcode.ai, paradev.com/compare, skillsllm и т.п.) не считаются достаточным доказательством и помечены как вторичные.

Ограничения: (а) звёздность и активность фиксируются как наблюдаемые данные GitHub на дату доступа — это не измерение качества; (б) юзабилити на реальном телефоне/iPad не проверялось (это продуктовое исследование без установки); (в) часть полей матрицы сознательно помечена UNKNOWN вместо уверенного «нет»; (г) цены могут меняться; все цены указаны по состоянию на дату доступа.

Категоризация рынка (по требованию): (A) удалённое управление существующими CLI-агентами; (B) оркестратор агентов/сессий/ворктри, запускающий собственную работу; (C) общий удалённый IDE/терминал; (D) только-десктопные приложения.

3. Базовая линия: Terminum — подтверждённое, заявленное, непроверенное
3.1 Подтверждено первоисточниками (репозиторий, релизы, SECURITY.md, сайт)

Что это: self-hosted, tmux-нативная браузерная IDE для десятков кодинг-агентов; terminumd — один Rust-сервер на 127.0.0.1:48080, управляющий приватным tmux-сервером (tmux -L terminum) и tmux-сессиями удалённых хостов по SSH; страница отдаёт сетку tmux (capture-pane), только изменившиеся строки (README репозитория (https://github.com/extractumio/terminum), доступ 4–5 ноября 2026).

Функции: список сессий (tmux-сессии/окна/панели + состояния агентов), режимы терминала Wrap/Grid/Fit/Screen, файловая панель с просмотром/редактированием (локально и по SSH через helper), вставка скриншотов и загрузка файлов до 100 МБ (на хост сессии), Quote & Ask, закладки, напоминания, история отправленного, поиск, проекты, 20 цветовых схем (README (https://github.com/extractumio/terminum); terminum.app (https://terminum.app/)).

Агенты: хуки для Claude Code (поставка как плагин, ставится через claude plugin) и Codex (записи в ~/.codex/hooks.json) дают точные состояния: working / waiting for approval or answer / done / subagents (README (https://github.com/extractumio/terminum); SECURITY.md (https://github.com/extractumio/terminum/blob/main/SECURITY.md)). OpenCode также распознаётся (файл NOTICE, релизы v0.27.x).

Живучесть: сессии живут в tmux и переживают закрытие браузера; terminumd работает как launchd-агент (macOS 14+) или systemd user-сервис (Ubuntu/Debian, loginctl enable-linger); после ребута восстанавливаются сессии/окна/сплиты/папки и последние 300 строк (README (https://github.com/extractumio/terminum); terminum.app FAQ (https://terminum.app/)).

Удалённые хосты: terminum hosts add <имя> '<ssh-строка>' --files; terminumd держит tmux-сессию хоста по SSH, переподключается после обрыва; файловый helper копируется на хост только после явного разрешения; можно attach'ить и чужие (уже существующие) tmux-сессии пользователя на хостах (README (https://github.com/extractumio/terminum); SECURITY.md (https://github.com/extractumio/terminum/blob/main/SECURITY.md)).

Безопасность: слушает только loopback; единый пароль для всех браузеров; доступ с других устройств — через tailscale serve (HTTPS tailnet'а); Funnel запрещён; релизы подписаны; подробная модель угроз в SECURITY.md (CSP без eval, quoting для tmux send-keys, запись файлов только под $HOME:/tmp, allowlist SSH-опций без -o/-F/ProxyCommand и т.д.) (SECURITY.md (https://github.com/extractumio/terminum/blob/main/SECURITY.md)).

Лицензия/цена: AGPL-3.0 или коммерческая лицензия; «бесплатно для персонального использования и для всех, кто принимает AGPL» (README (https://github.com/extractumio/terminum); terminum.app FAQ (https://terminum.app/)).

Активность: v0.30.4 от 4 ноября 2026; 51 тег, ~360 коммитов, релизы практически ежедневно; первый коммит «Initial import of terminum» — 16 октября 2026; ~10 звёзд, 3 форка, 3 контрибьютора (страница репозитория (https://github.com/extractumio/terminum); релизы (https://github.com/extractumio/terminum/releases)).

Windows: нативной поддержки нет; официальный путь Linux — Ubuntu/Debian (systemd), что и позволяет запуск в WSL2 — конфигурация пользователя соответствует документированному пути (README (https://github.com/extractumio/terminum)).

3.2 Заявления автора, не подтверждённые независимо

Релиз-ноты содержат развёрнутые разделы «Verified» (сотни тестов, live-проверки против реального terminumd/tmux в Chromium/WebKit) — это самодекларируемые результаты тестирования самого автора; независимого аудита или значимого сообщества (которое могло бы их подтвердить) у проекта пока нет (~10 звёзд на дату доступа).

Заявления об удобстве телефонной раскладки («touch-first», hot-key row) — не проверены на реальном iPhone (автор сам помечает в v0.26.2: «Not verified: … a physical iPhone»).

Устойчивость на медленных хостах: v0.30.3/0.30.4 сами признают краевые случаи (список хоста читается «до ~11 с», моментальные рассинхроны строк) — честно, но подчёркивает молодость кода.

3.3 Наши непроверенные допущения (сформулированы явно)

Что установка в WSL2 на рабочей станции Windows работает стабильно длительное время (документированный Ubuntu-путь, но сама связка WSL2+systemd+linger автором не документирована).

Что производительность по tailnet на телефоне приемлема.

Что хуки Claude Code/Codex не конфликтуют с уже установленными hook'ами пользователя.

Что единый пароль — приемлемая модель для одного человека (модель безопасности описана, но не аудирована).

3.4 Критическое различие: восстановление раскладки vs возобновление задачи

Terminum восстанавливает раскладку и контекст чтения, но не возобновляет выполнение задачи: после ребута/убийства tmux-сервера заново создаются сессии/окна/сплиты/папки и показываются последние 300 строк, однако запущенные процессы (агенты) гибнут вместе с машиной и не перезапускаются; продолжение разговора возможно только средствами самих агентов (например, claude --resume/--continue), что Terminum не заявляет как функцию (README (https://github.com/extractumio/terminum): «Survives reboots: sessions, windows and splits come back with their last 300 lines»; terminum.app FAQ (https://terminum.app/)). Для сравнения: Herdr заявляет «restores the saved layout and can resume supported agent sessions; the original processes do not survive» — то есть формально идёт дальше, используя собственные механизмы резюме агентов (herdrdev/herdr README (https://github.com/herdrdev/herdr), доступ 5 ноября 2026). Отключение клиента (без ребута) безопасно для обоих: сессии живут в tmux/сервере.

3.5 Замечание про MCP

Функции «Plan and ETA / Agent notes» в Terminum реализованы через MCP-сервер Terminum («terminum's MCP server lets the agent keep the session's plan, progress and notes» — terminum.app (https://terminum.app/); в релизе v0.27.1 исправлялся «terminum mcp», опрашивающий свою панель). Это входит в лёгкое напряжение с предпочтением «без лишних per-session stdio-MCP-процессов»: от хуков и MCP можно отказаться (состояния будут читаться с экрана), но тогда Plan/ETA/заметки недоступны. Поскольку существующие скрипты пользователя уже дают планы/прогресс/заметки, потеря некритична.

4. Карта рынка (ноябрь 2026)

Проверенные имена и их судьба:

Инструмент

	

Категория

	

Статус (дата доступа)




Terminum (https://github.com/extractumio/terminum)

	

A (панель над tmux-парком)

	

Активен, v0.30.4 от 04.11.2026; очень молодой




Happy / happy-cli (https://github.com/slopus/happy-cli)

	

A (мобильный клиент CLI-агентов, релей)

	

Активен; MIT; merged в основной репозиторий




HAPI (tiann/hapi) (https://github.com/tiann/hapi)

	

A (локальный хаб + релей; «local-first Happy»)

	

Активен, v0.34.2 от 05.11.2026; AGPL-3.0




CloudCLI (siteboon/claudecodeui) (https://github.com/siteboon/claudecodeui)

	

A/B (self-hosted веб-UI агентов)

	

Активен, коммиты 04.11.2026; AGPL-3.0-or-later




Agent of Empires (njbrake) (https://github.com/njbrake/agent-of-empires)

	

A/B (TUI+веб+CLI на tmux)

	

Активен, v1.21.0 от 21.10.2026; MIT; поддержка Mozilla.ai




T3 Code (pingdotgg/t3code) (https://github.com/pingdotgg/t3code)

	

B (контрол-плоскость, свои потоки в ворктри)

	

Активен, v0.1.7 от 04.11.2026; MIT; «очень рано»




Paseo (getpaseo/paseo) (https://github.com/getpaseo/paseo)

	

B (self-hosted демон + мобильные клиенты)

	

Активен, v0.15.1 от 05.11.2026; Apache-2.0




Herdr (herdrdev/herdr) (https://github.com/herdrdev/herdr)

	

A/C (терминальный мультиплексер агентов)

	

Активен, v0.9.5 от 14.10.2026; Apache-2.0; 48,1 тыс. звёзд




Xum, ранее cmux (coder/cmux) (https://github.com/coder/cmux)

	

B (свой цикл агента)

	

Переименован в Xum; AGPL-3.0; 2,1 тыс. звёзд




Paradev, ранее Superset (superset-sh/superset) (https://github.com/superset-sh/superset/blob/main/README.md)

	

B (десктоп-оркестратор ворктри)

	

Переименован; MIT (реликенз 04.11.2026); macOS-first




Claude Code Remote Control (https://code.claude.com/docs/en/remote-control)

	

A (официальный, вендорский)

	

Действует; входит в подписки Pro/Max/Ultra/Team/Enterprise




Codex Remote Connections (https://developers.openai.com/codex/remote-connections)

	

A (официальный, вендорский)

	

Действует; через ChatGPT-десктоп/мобильное приложение




Vibe Kanban (BloopAI) (https://github.com/BloopAI/vibe-kanban)

	

B (канбан-оркестратор)

	

Sunsetting; последний релиз v0.1.44 от 24.04.2026




Omnara (omnara-ai/omnara) (https://github.com/omnara-ai/omnara)

	

B (managed-платформа)

	

Пивот: теперь API-платформа агентов с API-ключами




Wave Terminal (https://github.com/wavetermdev/waveterm)

	

D (десктоп-терминал)

	

Активен; без браузерного UI




code-server (coder) (https://github.com/coder/code-server)

	

C (VS Code в браузере)

	

Активен; не агенто-ориентирован




ttyd (https://github.com/tsl0922/ttyd) / WeTTY (https://github.com/butofc/wetty)

	

C (расшаривание терминала)

	

Активны; без списка сессий/статусов




sshx (https://github.com/ekzhang/sshx)

	

C (коллаборативный терминал)

	

Активен; идёт через публичный mesh-релей sshx.io




Agent Deck (yolkmonday) (https://github.com/yolkmonday/agent-deck)

	

D (локальный read-only дашборд)

	

v0.2.1 от 23.09.2026; 0 звёзд; один человек




Crystal → Nimbalyst (stravu) (https://github.com/stravu/crystal)

	

B/D (десктоп, ворктри)

	

Переименован в Nimbalyst; десктоп




Claudito (https://github.com/comfortablynumb/claudito)

	

B (веб-менеджер только Claude Code)

	

Ранняя разработка, «bugs are expected»




Conductor (conductor.build) (https://happy.engineering/docs/comparisons/alternatives/)

	

B (Mac-приложение ворктри)

	

macOS-only; закрытый код; телефон в Pro $50/мес (данные со страницы сравнений Happy, проверено 04.11.2026)

5. Шортлист: матрицы по серьёзным кандидатам

Обозначения: UNKNOWN — не доказано источником (не уверенное «нет»). Все данные — на дату доступа 4–5 ноября 2026.

5.1 Terminum (базовая линия) — github.com/extractumio/terminum (https://github.com/extractumio/terminum)

Параметр

	

Значение




Сервер / клиент

	

Сервер: macOS 14+, Ubuntu/Debian (systemd; WSL2). Клиент: любой браузер




Мобильные

	

Браузер, touch-first раскладка, hot-key row, вставка и загрузка файлов. Нативного приложения нет




Существующие сессии

	

Да: attach чужих tmux-сессий (локально и на хостах); новые окна тоже




Агенты

	

Claude Code, Codex, OpenCode (метки); любые TUI через хуки/парсинг экрана




Подписки vs API

	

Подписочные CLI-логины; API не требуется




Несколько SSH-хостов

	

Да, terminum hosts add, автопереподключение




Персистентность

	

tmux; переживает клиент; после ребута — раскладка/папки/300 строк, задача не возобновляется




Статусы/апрувы/push

	

Точные состояния через хуки; напоминания (in-page); push-уведомлений ОС нет




Файлы/изображения

	

Файловая панель по SSH; скриншот одним ⌘V; загрузка до 100 МБ




Quote&Ask/закладки/рендер

	

Да (Quote #NNNN, закладки, Markdown/Mermaid/таблицы)




Планы/заметки

	

Да, через MCP-сервер Terminum (можно отключить)




Self-host vs релей

	

Чистый self-host, loopback + Tailscale; вендорского релея нет




Безопасность

	

Пароль на все браузеры; CSP без eval; allowlist SSH; подписанные релизы; SECURITY.md




Лицензия / цена

	

AGPL-3.0 или коммерческая; бесплатно для личного использования




Зрелость

	

~3 недели, ~10 звёзд, 1 основной автор, ежедневные релизы; высокий bus-factor

5.2 Happy (Happy Coder) — github.com/slopus/happy-cli (https://github.com/slopus/happy-cli), happy.engineering (https://happy.engineering/docs/how-it-works/)

Параметр

	

Значение




Сервер / клиент

	

CLI (happy) на вашей машине (Node ≥20; macOS/Windows/Linux); релей-сервер вендора (open source, ~12 тыс. строк TS, самохостинг «за 2 минуты»)




Мобильные

	

Нативные iOS/Android; реальный двусторонний ввод, push (happy notify)




Существующие сессии

	

Нет в общем случае: happy оборачивает и запускает сессию (Claude/Codex/Gemini). UNKNOWN: подключение к уже запущенной чужой tmux-сессии




Агенты

	

Claude (default), happy codex, happy gemini; Grok, ACP-агенты (beta)




Подписки vs API

	

Подписочные CLI (claude, Codex CLI, gemini-cli должны быть установлены и залогинены)




Несколько SSH-хостов

	

UNKNOWN (модель — CLI на одной машине; агрегации по нескольким хостам не заявлено)




Персистентность

	

Сессия живёт, пока работает happy/агент на машине; happy daemon; ребут-семантика отдельно не документирована — UNKNOWN




Статусы/апрувы/push

	

Push-уведомления; голос; наблюдение и управление с телефона




Файлы/изображения

	

File mentions, slash-команды; полноценной файловой панели по SSH нет




Quote&Ask/рендер

	

Chat-подобный интерфейс; рендер вывода ограничен




Планы/заметки

	

Нет (аналог — свои скрипты)




Self-host vs релей

	

Релей вендора с E2E-шифрованием (сервер видит только шифроблобы); свой сервер через HAPPY_SERVER_URL




Безопасность

	

E2E-шифрование, QR-pairing, zero-round-trip auth; доверие к серверу снижено конструкцией




Лицензия / цена

	

MIT; бесплатно, платный голос (how-it-works (https://happy.engineering/docs/how-it-works/), alternatives (https://happy.engineering/docs/comparisons/alternatives/))




Зрелость

	

Ранний CLI-репозиторий 551 звезда (CLI merged в основной репозиторий); активная разработка

5.3 HAPI — github.com/tiann/hapi (https://github.com/tiann/hapi), hapi.run (https://hapi.run/)

Параметр

	

Значение




Сервер / клиент

	

Хаб (npx @twsxtd/hapi hub) на вашей машине (Bun/TS); клиенты: Web/PWA (бесплатно), нативные iOS/Android (платно), Telegram Mini App




Мобильные

	

Web/PWA с push; нативные приложения с чатом, апрувами, файлами, диктовкой




Существующие сессии

	

Частично: «Shared Codex Sessions — использовать Codex из терминала и телефона одновременно» (Codex ≥0.154.0); в остальном оборачивает запуск (hapi claude, hapi codex)




Агенты

	

Claude Code, Codex, Cursor Agent, Grok Build, OpenCode, Kimi, Copilot, Antigravity, Pi, DeepSeek Harness




Подписки vs API

	

Подписочные CLI-агенты; API не требуется




Несколько SSH-хостов

	

UNKNOWN (хаб на одной машине)




Персистентность

	

Агент работает, пока работает хаб/машина; «AFK Without Stopping» — апрувы с телефона; ребут-семантика UNKNOWN




Статусы/апрувы/push

	

Однотапные апрувы с телефона; PWA-push и нативные push




Файлы/изображения

	

Workspace Browser (opt-in, скоуп через --workspace-root); изображения/аудио в чате; удалённые файлы других хостов — UNKNOWN




Quote&Ask/рендер

	

Chat-интерфейс; рендер вывода UNKNOWN




Планы/заметки

	

Нет заявлено




Self-host vs релей

	

Релей с E2E (WireGuard + TLS) по умолчанию; self-host варианты: Cloudflare Tunnel, Tailscale (docs)




Безопасность

	

E2E-шифрование релея; SECURITY.md в репо




Лицензия / цена

	

AGPL-3.0-only (миграция с января 2026); Web/PWA бесплатно, нативные приложения платно; цена UNKNOWN




Зрелость

	

5,4 тыс. звёзд, 645 форков, 129 контрибьюторов, v0.34.2 (05.11.2026), 1772 коммитов — зрелый активный проект

5.4 CloudCLI (бывшая Claude Code UI) — github.com/siteboon/claudecodeui (https://github.com/siteboon/claudecodeui), cloudcli.ai/open-source (https://cloudcli.ai/open-source)

Параметр

	

Значение




Сервер / клиент

	

Node v22+ на вашей машине (npx @cloudcli-ai/cloudcli, порт 3001); клиент — браузер (десктоп/планшет/мобильный); опциональный десктоп-компаньон (macOS/Windows)




Мобильные

	

Отзывчивый веб-дизайн; реальный ввод (чат + встроенный терминал)




Существующие сессии

	

Частично: «all your existing sessions are discovered automatically» из «внутренний путь лаборатории» (Claude Code, Cursor CLI, Codex, Gemini CLI) — обнаружение и resume существующих проектов на машине




Агенты

	

Claude Code, Codex, Cursor CLI, OpenCode, Gemini CLI




Подписки vs API

	

Подписки («All options use your own AI subscriptions»)




Несколько SSH-хостов

	

Нет/UNKNOWN: инстанс привязан к одной машине; отдельный инстанс на хост, единого списка нет




Персистентность

	

Сервер на хосте; сессии живут процессами агентов на хосте; ребут-семантика UNKNOWN




Статусы/апрувы/push

	

Интерактивный чат + инструменты; статусы ожидания и push-уведомления UNKNOWN




Файлы/изображения

	

Проводник файлов с редактированием, Git-панель, browser-use; изображения UNKNOWN




Quote&Ask/рендер

	

Чат-транскрипты; рендер Markdown в чате




Планы/заметки

	

Плагины (Task Queue, Scheduler, PRISM, ccx); интеграция TaskMaster (опц.)




Self-host vs релей

	

Чистый self-host (AGPL, без paywall в UI) или CloudCLI Cloud от €7/мес




Безопасность

	

Инструменты Claude Code по умолчанию выключены; авторизация self-host: UNKNOWN




Лицензия / цена

	

AGPL-3.0-or-later; self-host бесплатно; облако от €7/мес (cloudcli.ai (https://cloudcli.ai/open-source))




Зрелость

	

~13 тыс. звёзд (по странице cloudcli.ai), 897 коммитов, коммиты 04.11.2026, очень активен; npm-дистрибуция

5.5 Agent of Empires (AoE) — github.com/njbrake/agent-of-empires (https://github.com/njbrake/agent-of-empires), agent-of-empires.com (https://www.agent-of-empires.com/)

Параметр

	

Значение




Сервер / клиент

	

Rust-бинарь на Linux/macOS (Windows — только WSL2); клиенты: TUI, веб-дашборд (aoe serve), CLI, HTTP API




Мобильные

	

Браузер: «Desktop: full dashboard… Phone: structured view» — мобильный доступ заявлен




Существующие сессии

	

Сессии — tmux-сессии, управляемые AoE (aoe add --cmd claude); можно напрямую tmux attach; attach чужих существующих сессий UNKNOWN




Агенты

	

Claude Code, Codex CLI, OpenCode, Kimi Code, Mistral Vibe, Cursor CLI, Copilot CLI, Pi.dev, Factory Droid и др.




Подписки vs API

	

Подписочные CLI




Несколько SSH-хостов

	

Не в ядре; CityHall (experimental, Labs) — self-hostable контроль-плоскость, координирующая AoE на многих машинах (аккаунты, роли, SSO)




Персистентность

	

tmux; «close aoe — nothing happens»; ребут-семантика: сессии tmux сохраняются, возобновление задач UNKNOWN




Статусы/апрувы/push

	

Статус-детекция (running/waiting/idle), уведомления (тип/канал — UNKNOWN), статус-бар tmux




Файлы/изображения

	

Diff-просмотр и правки файлов в TUI; файловая панель по SSH — UNKNOWN




Quote&Ask/рендер

	

Диффы; рендер чата UNKNOWN




Планы/заметки

	

Нет (профи, repo-hooks, кастомные команды)




Self-host vs релей

	

Чистый self-host; MIT; без релея




Безопасность

	

Веб-дашборд: модель доступа UNKNOWN (проверить перед экспонированием)




Лицензия / цена

	

MIT; бесплатно




Зрелость

	

3,6 тыс. звёзд, 396 форков, 147 контрибьюторов, 126 релизов, v1.21.0 (21.10.2026); поддерживается Mozilla.ai; активный

5.6 T3 Code — github.com/pingdotgg/t3code (https://github.com/pingdotgg/t3code)

Параметр

	

Значение




Сервер / клиент

	

Сервер t3 (Node; t3 service install — фоновый сервис); клиенты: веб, Electron-десктоп, нативные iOS/Android




Мобильные

	

«Best-in-class mobile app (iOS, Android)» — реальный ввод




Существующие сессии

	

Нет: контролирует агентов, «если они настроены на вашей машине» — запускает свои потоки (thread'ы); attach существующих терминальных сессий UNKNOWN




Агенты

	

Claude Code, Codex, Cursor, Grok Build, OpenCode, Antigravity, fx




Подписки vs API

	

Подписки: «Works with your subscriptions… Install and authenticate at least one provider»




Несколько SSH-хостов

	

UNKNOWN (модель — сервер на одной машине; doc «Remote access from a phone or another machine» — про доступ клиента, не про агрегацию хостов)




Персистентность

	

Потоки живут на сервере; ребут-семантика UNKNOWN




Статусы/апрувы/push

	

Управление агентами из приложения; push через нативные приложения (детали UNKNOWN)




Файлы/изображения

	

Ворктри на поток, inline-диффы, one-click PR; файловая панель по SSH UNKNOWN




Quote&Ask/рендер

	

Диффы, PR; рендер вывода UNKNOWN




Планы/заметки

	

Нет заявлено




Self-host vs релей

	

Self-host (npx t3 serve headless); удалённый доступ к своей машине; вендорский релей — UNKNOWN (T3 Connect упоминается в сторонних обзорах как опция паринга)




Безопасность

	

SECURITY.md в репо; детали pairing-токенов UNKNOWN




Лицензия / цена

	

MIT; бесплатно; платного тира не заявлено




Зрелость

	

46,8 тыс. звёзд, 9,5 тыс. форков, 941 контрибьютор, 782 релиза, v0.1.7 (04.11.2026); сами авторы: «We are very very early… Expect bugs», вклады почти не принимаются

5.7 Paseo — github.com/getpaseo/paseo (https://github.com/getpaseo/paseo), paseo.sh (https://paseo.sh/)

Параметр

	

Значение




Сервер / клиент

	

Демон (Node) на вашей машине/сервере; клиенты: десктоп (Electron), нативные мобильные iOS/Android с полной паритетностью, веб, CLI, TypeScript SDK




Мобильные

	

«The mobile app is the full app, native on iOS and Android» — реальный ввод, полный функционал




Существующие сессии

	

Нет: запускает своих агентов (paseo run --provider claude/opus-4.6 …); attach чужих tmux-сессий UNKNOWN




Агенты

	

Claude Code, Codex, Copilot, OpenCode, Pi, Antigravity, Muse Code




Подписки vs API

	

Подписки: «agent CLI installed and configured with your credentials»




Несколько SSH-хостов

	

Да (с оговоркой): демон на каждой машине; CLI умеет paseo run --host workstation.local:6767 --cwd …; клиенты подключаются к хостам (Pair Device); степень агрегации хостов в одном UI — UNKNOWN




Персистентность

	

Агенты на демоне; ребут-семантика UNKNOWN




Статусы/апрувы/push

	

Полный цикл в мобильном приложении (создание, follow-up, ревью, мердж); push через нативные приложения (детали UNKNOWN)




Файлы/изображения

	

Редактор файлов, диффы, PR, встроенный браузер, терминалы; удалённые файлы других хостов UNKNOWN




Quote&Ask/рендер

	

Диффы/PR; рендер вывода UNKNOWN




Планы/заметки

	

Скиллы handoff/advisor/committee; schedules; заметок как таковых нет




Self-host vs релей

	

Self-host: демон + Docker (ghcr.io/getpaseo/paseo, пароль через PASEO_PASSWORD); pairing через opt-in E2E-релей (официальный релей — open source, Elixir) или напрямую TCP/Tailscale/VPN




Безопасность

	

«No telemetry, tracking, or forced log-ins»; E2E-релей; без релея — прямое TCP/Tailscale




Лицензия / цена

	

Apache-2.0; бесплатно; финансирование через спонсоров




Зрелость

	

23,1 тыс. звёзд, 2,8 тыс. форков, 353 контрибьютора, 218 релизов, v0.15.1 (05.11.2026); «built by one person» (Mohamed Boudra) + сообщество — тоже bus-factor, но большой

5.8 Herdr — github.com/herdrdev/herdr (https://github.com/herdrdev/herdr), herdr.dev/docs (https://herdr.dev/docs)

Параметр

	

Значение




Сервер / клиент

	

Один Rust-бинарь (без Electron) на macOS/Linux/Windows (PowerShell-инсталлер); herdr-core — фоновый сервер; плюс нативное десктоп-приложение herdr-app




Мобильные

	

Браузерного UI нет — это терминальный мультиплексер; с телефона — через SSH-клиент (Termius/Blink); для пользователя это ключевое несоответствие требованию «браузер с iPad/телефона»




Существующие сессии

	

«Не оборачивает и не заменяет агентов — владеет их терминалами»; direct attach для агентов документирован; attach произвольных чужих tmux-сессий UNKNOWN




Агенты

	

Claude Code, Codex, Cursor, OpenCode, Grok и остальные CLI




Подписки vs API

	

Подписочные CLI




Несколько SSH-хостов

	

Да: «Local and saved SSH machines in one window, combined agent list, independent reconnects»




Персистентность

	

Переживает закрытие клиента/SSH-обрыв; после рестарта — восстановление раскладки + «can resume supported agent sessions» (процессы не выживают); pane history replay




Статусы/апрувы/push

	

Каждая панель помечена working/blocked/idle; «когда агенту нужен ответ — herdr говорит»; уведомления конфигурируются




Файлы/изображения

	

Файловые операции через агентов/терминал; файловой панели нет




Quote&Ask/рендер

	

Нет




Планы/заметки

	

Нет (зато socket API и плагины)




Self-host vs релей

	

Чистый локальный; «free for individuals. Forever»; команды — платные




Безопасность

	

Локально; SSH-машины; модель доступа UNKNOWN в деталях




Лицензия / цена

	

Apache-2.0; бесплатно для индивидуалов




Зрелость

	

48,1 тыс. звёзд, 3,9 тыс. форков, 99 контрибьюторов, 98 релизов, v0.9.5 (14.10.2026); экосистема (herdr-core, app, plugins, nix)

5.9 Официальные вендорские каналы (обязательный пункт сравнения)

Параметр

	

Claude Code Remote Control (https://code.claude.com/docs/en/remote-control)

	

Codex Remote Connections (https://developers.openai.com/codex/remote-connections)




Как работает

	

claude remote-control (сервер) или /remote-control из существующей сессии; QR/URL; claude.ai/code или мобильное приложение Claude

	

ChatGPT-десктоп на macOS/Windows как хост; Codex в мобильном приложении ChatGPT; также подключение к проектам на SSH-хосте




Требования

	

Подписки Pro/Max/Ultra/Team/Enterprise; API-ключи не поддерживаются; обязателен api.anthropic.com (без прокси/Bedrock/шлюзов)

	

Codex-доступ в аккаунте ChatGPT; подписочная модель




Существующие сессии

	

Да — /remote-control из уже идущей сессии

	

Да — «continue existing chats», «steer active work»




Агенты

	

Только Claude Code

	

Только Codex




Функции

	

Сообщения с телефона/браузера, изображения и файлы, апрувы инструментов, автопереподключение (сон ноутбука)

	

Follow-up, апрувы команд, ревью выводов/диффов/скриншотов, уведомления, переключение хостов




Мультихостовость

	

Нет (одна машина на сессию; это не дашборд)

	

Частично: несколько подключённых хостов в приложении




Персистентность

	

Локальный процесс Claude должен работать; «cloud sessions» — отдельный режим на инфраструктуре Anthropic

	

Хост должен быть онлайн; Local computer access with Work Cloud — облачный контейнер-фолбэк (без доступа к файлам хоста)




Цена

	

Входит в подписку

	

Входит в подписку

5.10 Краткие матрицы оставшихся серьёзных, но частично подходящих

Xum (быв. cmux, Coder) — github.com/coder/cmux (https://github.com/coder/cmux): AGPL-3.0; изолированные воркспейсы (local/worktree/SSH); «Xum has a custom agent loop» — собственный harness, модели через провайдеров (мульти-модель, Ollama, OpenRouter) → биллинг через API, а не через подписки Claude/Codex — материальная трата при уже оплаченных подписках (возможность подключить подписочные логины — UNKNOWN); VS Code-расширение; «server mode has a responsive UI for mobile users»; 2,1 тыс. звёзд. Категория B.

Paradev (быв. Superset) — github.com/superset-sh/superset (https://github.com/superset-sh/superset/blob/main/README.md): MIT (реликенз 04.11.2026); macOS (основная), Linux experimental AppImage, Windows недоступен; воркспейс = ворктри; полный цикл ревью/PR/браузер-превью; агенты: Claude Code, Codex, OpenCode, Cursor Agent, Gemini CLI, Copilot, Amp, Antigravity, Droid, fx и др. — «bring your own subscriptions»; remote hosts, iPhone/iPad-приложение и автоматизации — в Pro-плане (iOS 26+); цена Pro в открытом виде не публикуется («tailored to your organization» для организаций); привлёк $11,5M seed (вторичный источник dealroom.co (https://dealroom.co/news/166945-paradev-raises-11-5m-seed-to-run-coding-agents-by-the-hundreds/)). Категория B, macOS-first.

6. Явно отклонённые и почти-подходящие

Vibe Kanban (https://github.com/BloopAI/vibe-kanban) — отклонён: официальный sunsetting (баннер в README), последний релиз v0.1.44 от 24.04.2026; сторонние источники датируют анонс 10 апреля 2026 (paradev.com/compare (https://paradev.com/compare/superset-vs-vibe-kanban) — вторичный). Вкладываться в закрывающийся продукт нерационально.

Omnara (https://github.com/omnara-ai/omnara) — отклонён для этого воркфлоу: проект пивотировался в «open-source managed agent platform» (альтернатива Claude Managed Agents / Agents API) — свои воркеры, Postgres, API-ключи провайдеров; управление существующими подписочными CLI-агентами больше не является его моделью.

Conductor (https://happy.engineering/docs/comparisons/alternatives/) — отклонён: macOS-only, закрытый код, телефонное приложение — в Pro за $50/мес (данные со страницы сравнений Happy, 04.11.2026; собственная документация закрыта).

Wave Terminal (https://github.com/wavetermdev/waveterm) — отклонён как полная замена: отличный десктоп-терминал (macOS/Linux/Windows) с durable-SSH и редактированием удалённых файлов, но нет браузерного/мобильного доступа, а AI-ассистент — BYOK API-ключи, не подписочные CLI-агенты. Полезен как дополнение на десктопе.

ttyd (https://github.com/tsl0922/ttyd) / WeTTY (https://github.com/butofc/wetty) — отклонены: расшаривание одного терминала в браузер (basic auth, SSL, ZMODEM); нет списка сессий, статусов агентов, attach нескольких хостов. Годятся как строительный блок, не как решение.

sshx (https://github.com/ekzhang/sshx) — отклонён для приватного использования: совместный терминал через глобальный публичный mesh-релей sshx.io (E2E-шифрование есть, но модель — публичная ссылка-шеринг); self-host релея не заявлен как поддерживаемый продукт.

code-server (https://github.com/coder/code-server) — отклонён как полная замена: общий «VS Code в браузере» без агентных состояний, апрув-центра и tmux-семантики; полированный IDE ≠ автоматическая замена (категория C).

Agent Deck (yolkmonday) (https://github.com/yolkmonday/agent-deck) — отклонён: локальный read-only дашборд одного компьютера (Tauri; читает «внутренний путь лаборатории», ~/.codex/sessions и т.п.), без удалённых хостов и без мобильного доступа; на Windows часть функций отключена; 0 звёзд, v0.2.1 (23.09.2026). Возможен только как дополнение к десктопу.

Crystal/Nimbalyst (https://github.com/stravu/crystal) — отклонён: десктоп-приложение для параллельных ворктри на одной машине.

Claudito (https://github.com/comfortablynumb/claudito) — отклонён: только Claude Code (без Codex), ранняя разработка («bugs are expected»).

Claude Squad — не попал в шортлист: актуальный канонический репозиторий не удалось открыть в ходе проверки (страница недоступна), а описания в сторонних источниках противоречивы (TUI-менеджер против «multiplayer session layer») — существование и функции не верифицированы, честно помечаем как неподтверждённые.

agent-of-empires (форк jtianling) — активный форк оригинала (726 коммитов, AoE jt 0.1.0 от 02.11.2026), но на странице форка 0 звёзд и нет контрибьюторов; за основным продуктом оставляем njbrake/agent-of-empires (https://github.com/njbrake/agent-of-empires).

7. Инкрементальная ценность относительно текущей настройки

Что уже есть у пользователя: SSH + tmux (все хосты), приватный tailnet, собственные скрипты планов/прогресса/заметок, Terminum на WSL с агентами на Linux/Mac-хостах. Отсюда честная оценка приращения каждого кандидата:

Herdr: + лучшая в категории семантика восстановления (layout + resume поддерживаемых сессий), + мультихостовость в одном окне, + статус-детекция из коробки; − нет браузерного UI (iPad/телефон только через SSH-клиент). Приращение для «браузера с iPad» — отрицательное.

Paseo / T3 Code / HAPI / Happy: + настоящие мобильные клиенты с push и однотапными апрувами (то, чего у Terminum нет и что закрывает самый больной разрыв), + оркестрация ворктри для новых задач; − все они запускают свои сессии, а не подсоединяются к существующему tmux-парку; − агрегация нескольких SSH-хостов в одном UI не доказана (Paseo — частично через демоны на хостах); − планы/заметки у Paseo/HAPI отсутствуют — при наличии своих скриптов это скорее плюс: меньше дублирования.

CloudCLI: + зрелая веб-обвязка чат/файлы/git/терминал; − по сути один инстанс = один хост: для парка машин инкремент — запуск N копий без единого списка; существующие сессии обнаруживаются на машине инстанса.

AoE: + веб-дашборд + TUI + HTTP API, MIT, поддерживается Mozilla.ai; − мультихостовость только через экспериментальный CityHall; веб-модель доступа не описана.

Официальные Remote Control / Remote Connections: + ноль новой инфраструктуры, push-апрувы из мобильных приложений, продолжение существующей сессии, включено в уже оплаченные подписки; − нет единой панели по всем агентам, по одному механизму на агента, требуется онлайн исходной машины; Remote Control не работает за прокси/шлюзами (только api.anthropic.com).

Xum: − собственный цикл агента = потенциальный новый API-биллинг при уже оплаченных подписках (материальная трата, не автоматическая дисквалификация, но здесь не оправданная).

Paradev: + сильный ревью-цикл и автоматизации; − macOS-primary (Windows-станция пользователя вне поддержки), мобильное — за Pro.

Вывод: для «панели над существующим парком» ни один кандидат не даёт приращения, кроме push-уведомлений (HAPI/T3/Paseo/Happy/официальные приложения); для «новой основной среды» приращение Paseo/T3/HAPI существенно — но это другая архитектура (оркестратор, запускающий свою работу), а не замена по прямой.

8. Ранжирование и рекомендации
8.1 Сценарий 1: удобная панель, управляющая существующим парком (без миграции)

Terminum (оставить) — единственный в шортлисте, дающий одновременно браузер + агрегацию SSH-хостов + attach существующих сессий + удалённые файлы. Миграция не авторизована; замена «на равное» здесь ничего не добавляет, а риски молодости Terminum демпфируются тем, что это не единственная точка входа (SSH/tmux остаются).

Официальные Claude Code Remote Control + Codex Remote Connections — запасной вариант и одновременно быстрое дополнение: закрывают push-уведомления и однотапные апрувы на телефоне для уже существующих сессий, без новой инфраструктуры и без изменений на машинах (code.claude.com/docs/en/remote-control (https://code.claude.com/docs/en/remote-control); developers.openai.com/codex/remote-connections (https://developers.openai.com/codex/remote-connections)). Ограничения Remote Control: только api.anthropic.com-эндпоинт (не работает за прокси/Bedrock), только Claude Code.

Herdr — если браузерное требование ослабить до «терминал с iPad/телефона через SSH-клиент»: лучшая мультихостовость и статус-модель; но это отход от явного требования браузера.

HAPI — точечно, для общих Codex-сессий (терминал + телефон одновременно) — единственный проверенный «attach-подобный» механизм у обёрток.

CloudCLI / AoE — если позже понадобится веб-интерфейс на самом Linux-сервере (одна машина) — CloudCLI; для командной/мульти-машинной координации — AoE с CityHall (экспериментально).

8.2 Сценарий 2: основная среда, в которой запускается новая работа агентов

Paseo (лид) — self-hosted демон на always-on Linux-сервере, полный функционал в нативных мобильных приложениях, мультипровайдерность (Claude Code + Codex и др.) на подписочных логинах, ворктри на задачу, Apache-2.0, работа с несколькими хостами через демоны и --host, без телеметрии; pairing либо через E2E-релей, либо напрямую по tailnet — ложится поверх существующего Tailscale (getpaseo/paseo (https://github.com/getpaseo/paseo)).

T3 Code (запасной) — самый быстрорастущий проект категории, MIT, веб+десктоп+iOS/Android, подписки; риски: «very very early», почти не принимает вклады, ночная каденция (pingdotgg/t3code (https://github.com/pingdotgg/t3code)).

HAPI (второй запасной) — зрелее по инженерии (129 контрибьюторов), E2E-релей, PWA бесплатно; минус — AGPL и платные нативные клиенты.

CloudCLI — если приоритет файлы/git/терминал в браузере на одной машине.

Paradev / Xum — не рекомендуются первыми: Paradev — macOS-primary и мобильное за Pro; Xum — собственный цикл агента с API-биллингом вместо уже оплаченных подписок.

8.3 Когда оставить Terminum — рационально

Пока потребность №1 — «видеть и вести весь существующий парк в браузере»: прямого аналога нет; каждая замена либо теряет браузер (Herdr), либо теряет attach/мультихост (Paseo/T3/HAPI/CloudCLI), либо теряет оба (дашборды).

Единственные подтверждённые потери Terminum: отсутствие OS-push и отсутствие возобновления задачи после ребута (только раскладка + 300 строк). Обе закрываются не сменой панели, а дополнением: официальные Remote Control/Remote Connections (push, апрувы) и claude --resume/Codex resume (продолжение разговора после рестарта) — что и так лежит в существующих скриптах/SSH-процессах.

Риск одной компании/автора у Terminum реален (~10 звёзд, 3 недели); поэтому рациональный паттерн — Terminum как панель + Paseo как пилот для новых задач, без миграции существующего парка.

9. Ограниченный план оценки (5 задач, без установки на production)

Общие условия: всё тестируется на одноразовой VM (клон Linux-сервера) и на рабочей станции, без перезагрузки боевых хостов; клиентское отключение и хостовый ребут тестируются отдельно; никакие изменения не переносятся в продакшн без отдельного решения.

#

	

Задача

	

Что меряем/проверяем

	

Критерий приёмки

	

Критерий провала




1

	

Апрув с телефона в ожидании: агент доводится до запроса разрешения; апрув с iPad-браузера (Terminum) и с нативного приложения (Paseo/T3/HAPI + официальные Remote Control/Remote Connections)

	

Время и число действий от «waiting» до принятия; видимость статуса ожидания из других сессий

	

Апрув проходит ≤2 действий; статус ожидания виден из другой сессии

	

Статус не виден или апрув невозможен с мобильного экрана




2

	

Агрегированный список по 3 хостам (Linux-VM, Mac, второй Linux)

	

Корректность списка сессий; поведение при обрыве SSH-линка; задержка обновления списка

	

Список консистентен; переподключение после обрыва автоматически

	

Потеря сессий из списка или ручное вмешательство после обрыва




3

	

Удалённые файлы и изображения: просмотр/правка файла на хосте из браузера/iPad; вставка скриншота в агента

	

Работоспособность файловой панели по SSH; доставка изображения в Claude Code/Codex на удалённом хосте

	

Правка сохраняется на хосте; изображение доходит до агента

	

Файлы недоступны либо изображение теряется на пути к агенту




4

	

Отключение клиента ≠ ребут хоста (только на одноразовой VM): (а) закрыть браузер/оборвать tailnet на 10 минут; (б) отдельно — рестарт демона; (в) отдельно — полный ребут VM; после каждого — проверить, что вернулось

	

Что восстановлено: раскладка/папки/последние строки vs сам процесс агента vs разговор (через claude --resume/Codex resume)

	

(а),(б) — работа продолжается без потерь; (в) — заранее известно, что именно исчезает и как продолжить разговор

	

Несоответствие заявленной семантике или потеря истории




5

	

Параллельный пилот новых задач: 1 неделя, новые задачи запускаются в пилотном инструменте (Paseo на Linux-VM; при проблемах — T3 Code, затем HAPI), существующий парк не трогается

	

Надёжность pairing с телефона; уведомления; совокупное время на обслуживание; конфликты с существующими hook'ами/MCP

	

Пилот не требует изменений на боевых хостах; уведомления приходят; время на обслуживание приемлемо для владельца

	

Необходимость менять боевые машины, либо уведомления ненадёжны

Данные, которые нужно собрать: для каждой задачи — субъективная оценка владельца (удобство, скорость), журнал инцидентов (обрывы, потерянные статусы), перечень ручных действий, версия каждого инструмента на дату теста.

Что опрокинет рекомендацию:

Если Paseo на пилоте покажет ненадёжный pairing/уведомления или невозможность работы демона на headless Linux-сервере с подписочными логинами — лид в сценарии 2 переходит к T3 Code (проверить стабильность t3 service install на том же сервере), затем к HAPI.

Если официальные Remote Control/Remote Connections покроют все мобильные сценарии владельца — потребность в мобильном слое Paseo/T3 падает, и лидом сценария 2 может стать CloudCLI (проще, зрелее веб-слой) при смягчении требования мультихоста.

Если Terminum в параллельном режиме покажет системные дефекты (потеря сессий, зависания terminumd, недопустимые задержки на медленных хостах) — панелью по умолчанию становится Herdr через SSH-клиенты (приняв отход от браузера) или AoE+CityHall, а браузерная потребность закрывается связкой официальных приложений + CloudCLI per-host.

10. Открытые вопросы и неопределённости

Мультихостовость Paseo/T3/HAPI в одном UI: заявлены клиенты к хостам, но единого агрегированного списка «все демоны в одном окне» мы не подтвердили — UNKNOWN до пилота.

Push-уведомления CloudCLI/AoE (тип и канал): не документированы явно — UNKNOWN.

Attach существующих tmux-сессий: подтверждён только у Terminum (и частично HAPI для Codex); для остальных — UNKNOWN.

Ребут-семантика Paseo/T3/CloudCLI: не документирована — UNKNOWN (закроет тест 4).

Цены: HAPI-нативные приложения, Paradev Pro, Happy-voice — точные цифры не опубликованы на открытых страницах, которые мы открыли; Conductor Pro $50/мес — со страницы сравнений Happy (вторичный источник).

Claude Squad: канонический репозиторий не верифицирован.

Сторонние заявления о популярности (например, «550k пользователей» T3 Code) не использованы как факты.

Все звёздности/активность — снимок GitHub на 4–5 ноября 2026, не прогноз.

Примечания об источниках

Источник

	

Достоверность

	

Обновлено




Terminum — репозиторий и README (GitHub) (https://github.com/extractumio/terminum)

	

4/5

	

04.11.2026




Terminum — релизы (v0.30.4) (https://github.com/extractumio/terminum/releases)

	

4/5

	

04.11.2026




Terminum — SECURITY.md (https://github.com/extractumio/terminum/blob/main/SECURITY.md)

	

4/5

	

01.11.2026 (коммит)




terminum.app — официальный сайт и FAQ (https://terminum.app/)

	

3/5

	

доступ 05.11.2026




Claude Code Remote Control — официальные доки Anthropic (https://code.claude.com/docs/en/remote-control)

	

5/5

	

доступ 05.11.2026




Codex Remote Connections — официальные доки OpenAI (https://developers.openai.com/codex/remote-connections)

	

5/5

	

доступ 05.11.2026




HAPI — репозиторий (tiann/hapi) (https://github.com/tiann/hapi)

	

4/5

	

05.11.2026




hapi.run — официальный сайт (https://hapi.run/)

	

3/5

	

доступ 05.11.2026




CloudCLI — репозиторий (siteboon/claudecodeui) (https://github.com/siteboon/claudecodeui)

	

4/5

	

04.11.2026




CloudCLI — cloudcli.ai/open-source (https://cloudcli.ai/open-source)

	

3/5

	

доступ 05.11.2026




Happy CLI — репозиторий (slopus/happy-cli) (https://github.com/slopus/happy-cli)

	

4/5

	

доступ 05.11.2026




Happy — как это работает (официальные доки) (https://happy.engineering/docs/how-it-works/)

	

3/5

	

доступ 04.11.2026




Happy — сравнение альтернатив (официальные доки) (https://happy.engineering/docs/comparisons/alternatives/)

	

3/5

	

04.11.2026 (по заявлению страницы)




Agent of Empires — репозиторий (njbrake) (https://github.com/njbrake/agent-of-empires)

	

4/5

	

21.10.2026 (релиз)




Agent of Empires — сайт (CityHall) (https://www.agent-of-empires.com/)

	

3/5

	

доступ 05.11.2026




Herdr — репозиторий (herdrdev/herdr) (https://github.com/herdrdev/herdr)

	

4/5

	

14.10.2026 (релиз v0.9.5)




Herdr — документация herdr.dev (https://herdr.dev/docs)

	

3/5

	

доступ 05.11.2026




T3 Code — репозиторий (pingdotgg/t3code) (https://github.com/pingdotgg/t3code)

	

4/5

	

04.11.2026




Paseo — репозиторий (getpaseo/paseo) (https://github.com/getpaseo/paseo)

	

4/5

	

05.11.2026




Paseo — официальный сайт paseo.sh (https://paseo.sh/)

	

3/5

	

доступ 05.11.2026




Vibe Kanban — репозиторий (sunsetting) (https://github.com/BloopAI/vibe-kanban)

	

4/5

	

24.04.2026 (последний релиз)




Omnara — репозиторий (пивот в платформу) (https://github.com/omnara-ai/omnara)

	

4/5

	

доступ 05.11.2026




Xum (быв. cmux) — репозиторий Coder (https://github.com/coder/cmux)

	

4/5

	

доступ 05.11.2026




Paradev (быв. Superset) — README репозитория (https://github.com/superset-sh/superset/blob/main/README.md)

	

4/5

	

04.11.2026 (MIT-реликенз)




Paradev — страница pricing (https://paradev.com/pricing)

	

3/5

	

доступ 05.11.2026




Wave Terminal — репозиторий (https://github.com/wavetermdev/waveterm)

	

4/5

	

доступ 05.11.2026




ttyd — репозиторий (https://github.com/tsl0922/ttyd)

	

4/5

	

доступ 05.11.2026




WeTTY — репозиторий (https://github.com/butofc/wetty)

	

3/5

	

доступ 05.11.2026 (по результатам поиска)




sshx — репозиторий (https://github.com/ekzhang/sshx)

	

4/5

	

доступ 05.11.2026




code-server — репозиторий (https://github.com/coder/code-server)

	

4/5

	

доступ 05.11.2026




Agent Deck — репозиторий (yolkmonday) (https://github.com/yolkmonday/agent-deck)

	

3/5

	

23.09.2026 (v0.2.1)




Crystal/Nimbalyst — репозиторий (stravu) (https://github.com/stravu/crystal)

	

3/5

	

доступ 05.11.2026 (по результатам поиска)




Claudito — репозиторий (https://github.com/comfortablynumb/claudito)

	

3/5

	

доступ 05.11.2026




Dealroom — новость о $11,5M seed Paradev (https://dealroom.co/news/166945-paradev-raises-11-5m-seed-to-run-coding-agents-by-the-hundreds/)

	

2/5

	

доступ 05.11.2026 (вторичный источник)




Paradev vs Paseo — сравнение (https://paradev.com/compare/superset-vs-paseo)

	

2/5

	

доступ 05.11.2026 (вторичный, вендорский)

Конфликты и оговорки. (1) Звёздность CloudCLI расходится между источниками (13 тыс. на cloudcli.ai против 14,4 тыс. на star-history) — взята vendor-страница. (2) Функции Terminum описаны автором и подтверждены только его собственными release-нотами — независимой валидации нет. (3) Оценки Conductor ($50/мес Pro) и Happy (платный голос) взяты со страницы сравнений Happy — вендорский, но аккуратно датированный вторичный источник. (4) Сторонние заявления о пользователях/популярности сознательно исключены из выводов. (5) Никакие проценты и точечные оценки не изобретались; все количественные поля — наблюдаемые метаданные GitHub или UNKNOWN.