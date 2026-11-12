# DR26-11-04-HUB-02-1908 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Альтернативы Terminum для существующего мультимашинного рабочего пространства ИИ-агентов

**Короткий ответ: прямой замены Terminum, которая бы лучше закрывала ваш основной сценарий, мы не нашли. Нужна единая браузерная панель поверх уже работающих tmux-сессий Claude Code и Codex на нескольких SSH-хостах, без стороннего relay. Поэтому как панель управления существующим парком Terminum рационально оставить. Для запуска новой работы сильнейший кандидат — Paseo: мультихостовый демон, нативные мобильные клиенты, подписочная авторизация CLI, Apache-2.0. Запасной вариант — Agent of Empires.** Это разные архитектуры для разных задач, поэтому единого победителя здесь нет.

Дата обращения ко всем источникам: 4 ноября 2026 г., если не указано иное.

## TL;DR

- **Панель для существующего парка:** оставить Terminum (https://github.com/extractumio/terminum). Только у него по первичным источникам подтверждено сочетание четырёх свойств: подключение к *уже запущенным* tmux-сессиям, в том числе на удалённых SSH-хостах; единый список сессий; удалённые файлы; отсутствие relay. Запасной вариант — веб-режим Agent Deck. Он tmux-нативный, MIT, но фактически привязан к одному хосту.
- **Основная среда для новой работы:** Paseo v0.15.0 от 3 ноября 2026 г. (https://paseo.sh/changelog) как главный кандидат на пилот. Запасной вариант — Agent of Empires (tmux + git worktrees + веб-дашборд, https://www.agent-of-empires.com/guides/web-dashboard/). HAPI и Happy — допустимые альтернативы с мобильным фокусом. Они требуют запускать агентов через свою обёртку и, у Happy, по умолчанию пользуются сторонним relay.
- **Главная оговорка:** «переживает перезагрузку» в Terminum означает восстановление раскладки tmux и последних 300 строк, а *не* возобновление задачи агента. Реальную перезагрузку автор сам помечает как непроверенную. Ни одно решение не следует выбирать без ограниченного пилота по плану в конце отчёта.

## Охват источников и неопределённость

- Выполнено 18 веб-поисков из запрошенных 30 (лимит инструмента исчерпан). Сделано 6 полных загрузок страниц и одно делегированное исследование по Paseo, Wave, ttyd и code-server. Поэтому охват рынка широкий по категориям, но неполный по нишевым проектам. Ряд найденных имён (Codeman, vibepanel, run-kit, MulmoTerminal, t3code) отмечен как «обнаружен, не проверен».
- Решающие факты взяты из README, релизов, документации и страниц магазинов приложений. Сторонние обзоры (vibecodinghub, hysenlabs, codepick и т. п.) использовались только для навигации. Там, где они — единственный источник, это оговорено.
- Звёзды упомянуты только там, где их видно на загруженной странице GitHub. Измерений производительности нет.
- **UNKNOWN** означает «не доказано», а не «нет».

## Шаг 1. Проверка Terminum

### (a) Заявления автора

README (https://github.com/extractumio/terminum) и сайт (https://terminum.app/) заявляют:

- «self-hosted, tmux-native browser IDE for running dozens of coding agents at once»;
- «No app, no account, no vendor relay»;
- Claude Code и Codex на Mac, Linux-сервере или любом ssh-хосте в одном списке сессий;
- файлы на любом хосте;
- вставка скриншотов и перетаскивание файлов на хост сессии;
- «Zero server setup: only tmux needed»;
- план/ETA и заметки через MCP;
- межсессионные оповещения;
- статусы working/waiting/done/subagents;
- Quote & Ask, закладки, отрендеренный Markdown/Mermaid;
- «Survives reboots: sessions, windows and splits come back with their last 300 lines».

Сайт формулирует это шире: «Sessions that survive closing the browser, restarting the server and rebooting». Это формулировка автора, а не верифицированный результат.

### (b) Функциональность, подтверждённая исходниками и релизами

- **Платформы сервера:** macOS 14+ с tmux 3.2+, Ubuntu/Debian как systemd user service (README, https://github.com/extractumio/terminum). Нативной поддержки Windows не заявлено. Ваш запуск UI в WSL — это сценарий вне документации.
- **Сеть и аутентификация:** terminumd слушает 127.0.0.1:48080. Доступ идёт через `ssh -L` или `tailscale serve`, вход по паролю. В v0.27.0 (2 ноября 2026 г.) секрет больше не отправляется в каждом запросе: используется HMAC с идентификатором загрузки демона. Автор открыто признаёт остаточный риск: программа, занявшая порт, пока terminumd не работает, может прочитать вход из хранилища браузера (https://github.com/extractumio/terminum/releases/tag/v0.27.0).
- **Подключение к существующим сессиям:** с v0.2.0 работает `terminum tmux attach <session> [--host <host>]`. Terminum «never starts that tmux and never makes or closes a session there». Каждая подключённая сессия хоста — отдельное ssh-соединение (https://github.com/extractumio/terminum/releases/tag/v0.2.0).
- **Удалённые хосты:** `terminum hosts add devbox '[email hidden]' --files` копирует на хост helper и держит его в версии Terminum (README).
- **Статусы:** `terminum hooks install` ставит плагин Claude Code и записи в `~/.codex/hooks.json`. Без хуков статус определяется чтением экрана. В v0.8.0 автор отметил: «Not checked: Codex loading terminum's hooks.json» (https://github.com/extractumio/terminum/releases/tag/v0.8.0).
- **Планы и заметки идут через `terminum mcp`.** Релиз v0.27.1 чинит ситуацию, когда «Agents on a host lost terminum's tools» (https://github.com/extractumio/terminum/releases/tag/v0.27.1). Значит, функции планов опираются на MCP-сервер, который агент запускает сам. Для вас это прямой конфликт с требованием «без лишних stdio-MCP на сессию», а сама функция дублирует ваши скрипты. Вывод: эту часть Terminum стоит не включать, а ценность хуков статуса оценивать отдельно.
- **Телефон:** v0.11.0 добавил меню ввода на телефоне и прикрепление нескольких файлов (https://github.com/extractumio/terminum/releases/tag/v0.11.0). Однако релизы v0.26.3–v0.27.3 повторяют «Not verified: a physical iPhone» (https://github.com/extractumio/terminum/releases/tag/v0.27.3).
- **Зрелость:** 3 звезды, 0 форков, 279 коммитов на момент загрузки. Релизы почти ежедневные: v0.1.0 от 16 октября 2026 г., затем v0.27.x к 2 ноября. Лицензия AGPL-3.0 или коммерческая. Проект — продолжение iterm-enhancer. Это очень молодой проект одного автора с сильной тестовой дисциплиной (в v0.27.1 перечислено 698 Rust-тестов), но без внешней базы пользователей.

### Перезагрузка: раскладка и последние строки ≠ возобновление задачи

- В v0.23.0 появилось хранение последних 300 строк каждой панели (настройка lastLines, 0–2000). После перезагрузки или убитого tmux-сервера в меню строки есть «Show Last 300 Lines» (https://github.com/extractumio/terminum/releases/tag/v0.23.0).
- В v0.25.0 после kill-server или перезагрузки возвращаются имена окон, метки Unread и последние строки. Заголовок разговора агента сохраняется «while a shell runs there, until an agent runs again» (https://github.com/extractumio/terminum/releases/tag/v0.25.0). Иначе говоря, в восстановленной панели запускается **shell, а не агент**. Чтобы продолжить задачу, нужно вручную выполнить `claude --resume` или `codex resume`. Для панелей хостов последние строки доступны только через меню. Там же сказано: «Not verified: a real reboot, a host's restore».
- Известное ограничение из v0.9.0: tmux-сервер, корректно остановленный с одной панелью, не восстанавливается (https://github.com/extractumio/terminum/releases/tag/v0.9.0).
- **Отключение клиента** — другое дело. Сессии живут в tmux, поэтому закрытие вкладки или потеря сети телефоном работу не прерывает. Это свойство tmux, которое у вас уже есть.

### (c) Наши непроверенные допущения

- Устойчивость terminumd в WSL на Windows.
- Удобство ввода с iPhone/iPad.
- Корректная работа хуков Codex на ваших версиях.
- Поведение при реальной перезагрузке удалённого хоста.
- Нагрузка от множества ssh-соединений (по одному на каждую подключённую чужую сессию).

## Шаг 2. Рынок: поиск и сужение

### Категории

- **(A) Удалённое управление существующими CLI-агентами:** Terminum; официальный Claude Code Remote Control; Codex Remote в ChatGPT mobile; HAPI и Happy (частично — они управляют сессиями, запущенными через их обёртку).
- **(B) Оркестраторы, запускающие собственную работу:** Paseo, Agent of Empires, Agent Deck, CloudCLI (гибрид A/B), Vibe Kanban.
- **(C) Общие удалённые IDE/терминалы:** code-server, ttyd/WeTTY.
- **(D) Только десктоп:** Superset, cmux, Wave Terminal.

### Отклонённые или понижённые кандидаты и причины

- **Vibe Kanban** (https://github.com/BloopAI/vibe-kanban): в README объявлено «Vibe Kanban is sunsetting», дальше проект поддерживает сообщество. Для основной среды риск сопровождения неприемлем.
- **Superset** (https://github.com/superset-sh/superset/blob/main/README.md): десктопное приложение-оркестратор worktree, по сторонним источникам ориентировано на macOS. Браузерного доступа к флоту с iPad и телефона нет. Категория D/B, не замена.
- **cmux** (https://github.com/manaflow-ai/cmux): нативный терминал для macOS на базе Ghostty с кольцами уведомлений по OSC 9/99/777. Умеет возобновлять сессии агентов по сохранённому ID. iOS-клиент — бета в TestFlight, ранний доступ в Founders Edition (https://github.com/manaflow-ai/cmux/blob/main/README.md). Сервер только macOS, браузерного клиента нет — категория D.
- **Wave Terminal** (https://github.com/wavetermdev/waveterm): по данным субагента, последняя версия v0.14.5 от 16 апреля. Есть «Durable SSH Sessions», но это десктоп для macOS/Linux/Windows без браузерного или мобильного клиента (https://docs.waveterm.dev/). Категория D.
- **ttyd** (https://github.com/tsl0922/ttyd): живой, но последний подтверждённый GitHub-релиз — 1.7.7 от 30 марта 2024 г. (https://github.com/tsl0922/ttyd/releases/tag/1.7.7). Docker-образы 1.7.8 существуют, но дата неоднозначна. Это голый терминал в браузере без списка сессий и статусов: строительный блок, а не конкурент. WeTTY первичными источниками не проверен.
- **code-server** (https://github.com/coder/code-server/releases): очень активен, по данным субагента релиз вышел 4 ноября 2026 г. на базе VS Code 1.145. Это полноценная IDE в браузере, но на один хост, без агрегированного списка агентов и статусов ожидания. Категория C. Полированная IDE ≠ функциональная замена.
- **Claude Code UI / CloudCLI** (https://github.com/siteboon/claudecodeui): оставлен в коротком списке как гибрид, но с оговорками (см. матрицу).
- **Обнаружены, не проверены:** Codeman (https://github.com/Ark0N/Codeman — self-hosted «mission control» на tmux, по README спаунит агентов и стримит терминал в браузер), а также vibepanel, run-kit, MulmoTerminal из каталога https://github.com/bradagi/awesome-cli-coding-agents. Codeman выглядит как прямой конкурент сценария B. Включать его в ранжирование без проверки первичной документации было бы некорректно, но для второй волны поиска он приоритетен.

### Короткий список (8)

Terminum, Paseo, HAPI, Happy, Agent of Empires, Agent Deck, CloudCLI, а также официальные Remote Control от Anthropic и OpenAI. Последние два объединены в одну позицию: это нулевая по стоимости база сравнения.

## Шаг 3. Матрица по серьёзным кандидатам

### Terminum (категория A)

- **Продукт:** extractumio/terminum, https://github.com/extractumio/terminum
- **ОС сервера:** macOS 14+, Ubuntu/Debian. Удалённые хосты — Linux/macOS только с tmux.
- **Клиент:** браузер (Safari/Chrome; заявлен iPhone Safari). Нативных приложений нет.
- **Мобильный ввод:** есть, с кнопками и меню (v0.11.0). На физическом iPhone автором не проверено.
- **Подключение к существующим сессиям:** да, `terminum tmux attach`, включая сессии хоста.
- **Claude Code / Codex:** оба, через хуки. Другие агенты отображаются как обычные терминалы.
- **Авторизация:** родные CLI с вашими подписками. Terminum не ходит к API моделей.
- **Несколько SSH-хостов:** да.
- **Персистентность:** tmux. После перезагрузки — раскладка и 300 строк, без возобновления агента.
- **Уведомления:** межсессионные оповещения в UI. Push на телефон — UNKNOWN.
- **Файлы и изображения:** файловая панель на хостах, вставка скриншотов, загрузка до 100 МБ (PR #6, https://github.com/extractumio/terminum/pull/6).
- **Quote & Ask / закладки / рендер:** да.
- **Планы и заметки:** да, через MCP (для вас это минус).
- **Self-host:** полностью, relay нет.
- **Безопасность:** loopback, пароль, HMAC, подписанные релизы.
- **Лицензия:** AGPL-3.0 или коммерческая.
- **Цена:** $0.
- **Зрелость:** около 3 недель, 3 звезды, почти ежедневные релизы.

### Paseo (категория B, с импортом сессий)

- **Продукт:** getpaseo/paseo, https://github.com/getpaseo/paseo/releases, https://paseo.sh/docs
- **ОС демона:** macOS, Linux, Windows. Клиенты: desktop, web, iOS, Android, CLI (https://paseo.sh/alternatives/conductor).
- **Мобильный ввод:** нативные приложения, в том числе в Google Play (https://play.google.com/store/apps/details?id=sh.paseo&hl=en).
- **Подключение к существующим сессиям:** к живому процессу — нет данных. Есть импорт истории: changelog 0.1.71 — «Import existing Claude, Codex, and OpenCode sessions… with the full timeline» (https://paseo.sh/changelog). Это возобновление разговора из сохранённой истории, а не подключение к вашим tmux-панелям.
- **Агенты:** Claude Code, Codex, Copilot, OpenCode, Pi и другие.
- **Авторизация:** «usage in Paseo counts against your normal Claude plan limits. It does not require a separate pool of Agent SDK credits» (https://paseo.sh/docs/claude-code).
- **Несколько хостов:** да. Можно запускать демон где угодно и подключаться к нему с любого клиента; хосты добавляются по Remote SSH (https://paseo.sh/docs/connectivity).
- **Персистентность:** агенты живут в демоне. «Running agents keep going, and clients reconnect automatically» при перезапуске демона (https://paseo.sh/docs/troubleshooting). v0.15.0 сохраняет терминальные программы при перезапусках и обновлениях. Опция «Resume agents after restart» по умолчанию выключена. Ограничения: первое обновление с 0.14.0 и обновления desktop на Windows всё ещё закрывают терминалы. Простаивающие агенты останавливаются через 1 час и возобновляются при следующем промпте (https://paseo.sh/docs/configuration).
- **Статусы, approvals, push:** approvals в клиентах есть. Push-уведомления демон отправляет: в PR #8501 (https://github.com/getpaseo/paseo/pull/8501) сказано «Every daemon push notification requests high delivery priority» — это события завершения агента, запроса разрешения и внимания терминала. Есть открытые баги: #2622 (на iOS push «finished» может теряться в 180-секундном окне присутствия) и #8069.
- **Файлы:** редактор, терминалы, diff и браузер в одном окне. Загрузка изображений с телефона — UNKNOWN.
- **Quote & Ask / закладки:** UNKNOWN.
- **Планы:** не основная функция.
- **Self-host и relay:** «Relay is off on new installations… Paseo asks before enabling it» (https://paseo.sh/docs/security). Relay с E2E-шифрованием, есть собственная реализация на Elixir (https://github.com/getpaseo/paseo-relay). Для вас достаточно tailnet.
- **Лицензия:** Apache-2.0 с v0.7.0 (31 августа 2026 г.), раньше AGPL-3.0. LICENSE: https://github.com/getpaseo/paseo/blob/main/LICENSE
- **Цена:** $0.
- **Зрелость:** активная разработка, v0.15.0 от 3 ноября 2026 г.

### HAPI (категория A/B)

- **Продукт:** tiann/hapi, https://github.com/tiann/hapi (5.1k звёзд на момент загрузки)
- **Архитектура:** CLI-обёртка, хаб (HTTP + Socket.IO + SQLite) и PWA (https://tiann-hapi.mintlify.app/).
- **Клиенты:** нативные iOS/Android — по README, «SwiftUI/UIKit and Kotlin Compose clients with chat, approvals, session creation, files, dictation, and push notifications»; также Web/PWA и Telegram Mini App.
- **Подключение к существующим сессиям:** нет, агенты запускаются через `hapi claude` или `hapi codex`. Поддерживается resume.
- **Агенты:** Claude Code, Codex (0.154+ через app-server), Cursor, Grok, OpenCode, Kimi, Copilot, Antigravity, Pi, DeepSeek (https://github.com/tiann/hapi/blob/main/docs/guide/agents.md).
- **Авторизация:** локальный режим оборачивает нативный TUI. Удалённый режим «drive the Claude Agent SDK». Учитывается ли это в подписке или требует отдельного биллинга — **UNKNOWN**, критично проверить.
- **Дополнительные процессы:** многие интеграции идут через ACP по stdio, то есть возможны дополнительные процессы на сессию.
- **Несколько хостов:** UNKNOWN.
- **Relay:** `hub --relay`, E2E (WireGuard+TLS). Хаб самохостится.
- **Лицензия:** AGPL-3.0 по боковой панели репозитория (https://github.com/tiann/hapi); последний релиз — v0.34.1 от 4 ноября 2026 г.
- **Зрелость:** частые релизы (https://github.com/tiann/hapi/releases).

### Happy / Happy Engineering (категория A/B)

- **Продукт:** slopus/happy, https://happy.engineering/
- **Изменение статуса:** проект превратился в десктопную «harness», а исходный CLI переведён в режим поддержки: «in maintenance mode… new features ship in the Happy desktop app» (https://happy.engineering/docs/faq/).
- **Клиенты:** iOS/Android, веб-приложение. Магазин: бесплатно с In-App Purchases (https://apps.apple.com/us/app/happy-codex-claude-code-app/id6748571505).
- **Авторизация:** подписка или API-ключ.
- **Relay:** по умолчанию свой, с E2E-шифрованием. Самохостинг через Docker (https://github.com/slopus/happy/tree/main/packages/happy-server).
- **Лицензия:** MIT.
- **Подключение к существующим сессиям:** нет, требуется запуск через `happy`.
- **Несколько машин:** да, по описанию в FAQ.
- **Для вас:** поворот продукта к десктопу и обязательный relay-аккаунт — материальные минусы.

### Agent of Empires (категория B)

- **Продукт:** agent-of-empires/agent-of-empires, https://github.com/njbrake/agent-of-empires/blob/main/README.md, https://www.agent-of-empires.com/
- **ОС:** Linux, macOS. Основа — tmux и git worktrees, опционально Docker-песочница.
- **Агенты:** Claude Code, Codex и много других.
- **Веб-дашборд:** помечен как «Experimental»/beta. Есть PWA и «structured view» через ACP со swipe-to-approve.
- **Удалённый доступ:** `aoe serve --remote` через Tailscale Funnel или Cloudflare. Токен 256 бит плюс пароль, в режиме --remote токен меняется каждые 4 часа. `--host 0.0.0.0` даёт HTTP для доступа по LAN/VPN (https://www.agent-of-empires.com/guides/web-dashboard/). Для tailnet без Funnel — привязка к tailnet IP (https://www.agent-of-empires.com/guides/tailscale/).
- **Push:** упоминается в руководстве по телефону (https://www.agent-of-empires.com/guides/remote-phone-access/).
- **Персистентность:** resume разговоров Claude после перезагрузки (README). Для Codex — UNKNOWN.
- **Несколько хостов:** сайт https://www.agent-of-empires.com/ описывает CityHall — «a self-hostable control-plane server that coordinates Agent of Empires across many machines from one shared backend». Мультихостовый путь, таким образом, существует, но агрегация агентов через него нами не проверена.
- **Лицензия:** MIT по первичному файлу LICENSE (https://github.com/njbrake/agent-of-empires/blob/main/LICENSE: «MIT License, Copyright (c) 2026 Nathan Brake»); один файл, src/tui/hyperlink.rs, — Apache-2.0 (код из herdr).
- **Подключение к существующим сессиям:** нет. Наоборот, к сессиям AoE можно подключиться через `tmux attach`.

### Agent Deck (категория B)

- **Продукт:** asheshgoplani/agent-deck, https://github.com/asheshgoplani/agent-deck (MIT, 1.2k звёзд, 4 440 коммитов)
- **ОС:** macOS, Linux, Windows через WSL.
- **Веб-режим:** `agent-deck web` на 127.0.0.1:8420, есть `--read-only` и `--token-file`.
- **Агенты:** Claude — «Full (status, MCP, fork, resume)». Codex — определение статуса и fork (https://github.com/asheshgoplani/agent-deck/blob/main/README.md).
- **Авторизация:** запускает немодифицированный `claude` с вашим логином.
- **MCP:** пулинг MCP-сокетов, то есть меньше stdio-процессов на сессию. Для вас это плюс.
- **Несколько хостов:** в v1.12.0 есть «show configured remotes safely in Fleet» (https://github.com/asheshgoplani/agent-deck/releases). Полноценная мультихостовая агрегация — UNKNOWN.
- **Телеметрия:** в репозитории есть файл TELEMETRY.md — нужно проверить, что именно собирается.

### CloudCLI / Claude Code UI (категория A/B)

- **Продукт:** siteboon/claudecodeui, https://github.com/siteboon/claudecodeui
- **Установка:** `npx @cloudcli-ai/cloudcli`, порт 3001.
- **Сессии:** автоматически находит сессии в «внутренний путь лаборатории» (по истории, а не живые tmux-панели).
- **Агенты:** Claude Code, Cursor CLI, Codex, Gemini.
- **Возможности:** файловый проводник, Git, терминал xterm.js. MCP настраивается через UI.
- **Цена:** собственные подписки. CloudCLI Cloud — по актуальному README siteboon/claudecodeui «Starts at €7/month» (в евро; в старом npm-README было $7) (https://github.com/siteboon/claudecodeui/blob/main/README.md).
- **Несколько хостов:** одна инсталляция на хост.
- **Лицензия:** AGPL-3.0-or-later — README siteboon/claudecodeui: «GNU Affero General Public License v3.0 or later (AGPL-3.0-or-later)».

### Официальные Remote Control (категория A, база сравнения)

- **Claude Code Remote Control:**
  - claude.ai/code и приложение Claude;
  - одна удалённая сессия на интерактивный процесс; серверный режим — до 32 одновременных сессий по умолчанию (в документации Anthropic, цитируемой claudcod.com: «--capacity | Maximum concurrent sessions; default 32»); без сети серверный режим примерно через 10 минут завершается;
  - локальный процесс должен работать (рекомендуется tmux);
  - нужен Pro/Max/Team/Enterprise и вход через claude.ai, API-ключи не поддерживаются;
  - трафик идёт через API Anthropic;
  - сведения из вторичного источника с прямыми цитатами документации: https://claudcod.com/blog/claude-code-remote-control/.
- **Codex в ChatGPT mobile:**
  - объявлен 14 мая 2026 г. (https://9to5mac.com/2026/05/14/openai-brings-codex-control-to-chatgpt-for-iphone-and-android/);
  - настройка начинается в десктопном приложении Codex/ChatGPT, не из CLI; поддержка Windows обещана позже;
  - есть подключение к SSH-хостам.
- **Сводно:** $0 сверх подписки, но это две отдельные вендорские панели без общего списка.

## Шаг 4. Анализ

### Инкрементальная ценность относительно того, что у вас уже есть

У вас уже есть SSH, tmux, tailnet, скрипты планов и заметок и Terminum. Значит, долговечность при отключении клиента вы уже получаете от tmux, а сетевой доступ — от tailnet. Новое решение должно добавить хотя бы одно из трёх:

1. Лучший мобильный ввод и approvals.
2. Агрегированный список по хостам с надёжными статусами.
3. Реальное возобновление задачи агента после перезапуска.

По этим критериям:

- Terminum добавляет (2) и удобства (файлы, Quote & Ask), но не (3).
- Paseo добавляет (1) и частично (3) через опцию resume и долговечность демона. Цена — отказ от ваших tmux-панелей как источника истины.
- AoE и Agent Deck добавляют worktree-изоляцию и статусы в рамках одного хоста.
- Официальные Remote Control добавляют (1) бесплатно, но по одной вендорской панели на каждого агента.

### Рейтинг 1: панель управления существующим парком

1. **Terminum — лидер.** Это единственный проверенный инструмент, который подключается к чужим tmux-сессиям на нескольких хостах без relay. MCP-функции планов включать не стоит.
2. **Agent Deck (веб) — запасной.** tmux-нативный, MIT, пулинг MCP. Минусы: сессии должны быть его собственными, а мультихостовость не доказана.
3. Официальные Remote Control как мобильное дополнение.
4. Agent of Empires (веб).
5. ttyd за tailnet как аварийный минимум.

Почему лидером остаётся Terminum, несмотря на молодость: альтернативы требуют перезапуска агентов через свою обёртку (HAPI, Happy, Paseo) или собственных сессий (AoE, Agent Deck). Для управления *существующим* парком это миграция, а миграция не санкционирована. Главные риски — один автор и WSL вне документации. Они снижаются тем, что проект — тонкий слой над tmux: при отказе сессии продолжают жить.

### Рейтинг 2: основная среда для запуска новой работы

1. **Paseo — лидер.** Мультихостовый демон, включая Windows. Нативные мобильные клиенты, подписочная авторизация, relay выключен по умолчанию, Apache-2.0, активные релизы. Компромиссы: свой процессный слой вместо tmux, молодая логика сохранения терминалов (0.15.0), push-уведомления есть, но с открытыми багами доставки на iOS (#2622, #8069), вложения с телефона — UNKNOWN.
2. **Agent of Empires — запасной.** tmux и worktrees совместимы с вашей привычной моделью, всегда можно сделать `tmux attach`. Компромиссы: веб-дашборд экспериментальный, только Linux/macOS, мультихостовость возможна через CityHall, но нами не проверена, структурированный вид через ACP может порождать дополнительные процессы.
3. HAPI — сильный мобильный UX, но вопрос биллинга удалённого режима через Claude Agent SDK открыт.
4. Happy — relay по умолчанию, продукт смещается к десктопу.
5. CloudCLI — однохостовый.
6. Agent Deck — сильнее как TUI, чем как браузерная среда.

### Где рационально оставить Terminum

Terminum стоит оставить при любом исходе пилота Paseo как «окно» в tmux-сессии, которые запускают ваши скрипты. Paseo, если пройдёт пилот, становится местом запуска новой параллельной работы с телефона. Две панели — это реальная цена, но они решают разные задачи. Новая облачная или API-зависимость возникает только в трёх случаях: включение relay Happy или Paseo, CloudCLI Cloud (от €7 в месяц по актуальному README) или удалённый режим HAPI, если окажется, что он тарифицируется отдельно.

## Шаг 5. Ограниченный план оценки (5 задач)

Общие правила:

- Только непроизводственная копия репозитория.
- Тестовый хост или VM. **Производственные хосты не перезагружать.**
- Отключение клиента и перезапуск хоста тестировать раздельно.
- Длительность — до двух недель.
- Данные для фиксации: журналы времени, скриншоты, версии CLI, количество процессов (`ps`), сетевые точки выхода (`ss`/`lsof`), расход лимитов подписки по панелям вендоров.

**Задача 1. Утренний обход парка (панель).** С iPad открыть список всех сессий на 3 хостах и найти все, что ждут ответа.

- Принятие: все ожидающие сессии видны в одном списке, статусы совпадают с реальностью по Claude и по Codex.
- Провал: пропущенное ожидание или ложное «working».
- Сравнить: Terminum с хуками и без них против Agent Deck web.

**Задача 2. Approval с телефона.** Агент запрашивает разрешение на команду, ответ даётся с iPhone по сотовой сети через tailnet.

- Принятие: ввод доходит с первого раза, контекст запроса читаем.
- Провал: нужна внешняя клавиатура или ответ уходит не в ту панель.
- Сравнить: Terminum, Paseo, официальные Remote Control.

**Задача 3. Отключение клиента.** Посреди длинной задачи закрыть браузер или приложение и выключить Wi-Fi на 30 минут.

- Принятие: задача продолжилась, после переподключения видна полная история.
- Провал: процесс остановился или история потеряна.
- Ожидание: tmux-решения пройдут тривиально; для Paseo это проверка демона.

**Задача 4. Перезапуск сервиса и хоста (только тестовая VM).** Отдельно: (a) перезапуск terminumd или демона Paseo; (b) kill-server tmux; (c) перезагрузка тестовой VM.

- Принятие для (c): понятно, какие задачи нужно возобновлять. Время до продолжения работы агента с сохранённым контекстом зафиксировано.
- Провал: незаметная потеря задачи или повторный запуск без контекста.
- Для Paseo проверить опцию «Resume agents after restart».

**Задача 5. Новая параллельная работа с файлами и изображением.** Запустить 3 агента (2 Claude, 1 Codex) на двух хостах в отдельных worktree, приложить скриншот с телефона, просмотреть diff.

- Принятие: запуск, вложение и обзор diff без SSH-клиента; работа идёт по подписке без API-ключа; число MCP- и ACP-процессов на сессию не растёт.
- Провал: требуется API-ключ или каждая сессия порождает дополнительные stdio-процессы.
- Сравнить: Paseo против Agent of Empires.

### Что опровергло бы рекомендации

- Terminum нестабилен в WSL или неудобен на физическом iPhone, а Agent Deck или AoE закрывают задачу 1 на одном Linux-сервере, где и так живёт большинство агентов. Тогда переносим UI на Linux-сервер или понижаем Terminum.
- Paseo не проходит задачи 2–5: Windows-демон, потеря терминалов, отсутствие push. Тогда лидером сценария 2 становится Agent of Empires.
- Удалённый режим HAPI подтверждённо работает по подписке, а хаб покрывает несколько машин. Тогда HAPI конкурирует с Paseo.
- Проверка Codeman по первичным источникам показывает мультихостовость и подключение к существующим tmux-сессиям. Тогда он попадает в оба рейтинга.
- Официальные Remote Control от Anthropic и OpenAI получают общий список сессий по нескольким хостам. Тогда сторонний мобильный слой теряет большую часть ценности.

## Оговорки

- Поисковый бюджет ограничил проверку надёжности push-уведомлений Paseo на практике, агрегации агентов через CityHall у AoE и мультихостовости Agent Deck (лицензии HAPI — AGPL-3.0 и CloudCLI — AGPL-3.0-or-later подтверждены на этапе обогащения). Непроверенное помечено UNKNOWN.
- Многие проекты меняются еженедельно. Факты актуальны на 4 ноября 2026 г.
- Отдельные данные о Superset, Vibe Kanban (лицензия, закрытие bloop) и об официальном Remote Control взяты из вторичных источников и для решения не используются.