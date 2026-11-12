# DR26-11-04-HUB-02-1908 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Анализ альтернатив Terminum для мультимашинной среды ИИ-агентов

В данном документе представлен исчерпывающий анализ архитектурных решений, способных заменить или дополнить Terminum в рамках распределенной мультимашинной инфраструктуры ИИ-агентов. По состоянию на 4 ноября 2026 года ландшафт инструментов претерпел значительные изменения благодаря внедрению протокола Agent Client Protocol (ACP), который позволил отвязать пользовательский интерфейс от прямого парсинга терминального вывода. Для технического руководителя, управляющего парком из Windows-рабочих станций, Mac и always-on Linux серверов через единую сеть Tailscale, критически важно получить доступ к консолидированному списку сессий с мобильных устройств (iPad, смартфон) без усложнения текущего стека. Анализ показывает, что для сценария удобного управления существующим флотом оптимальным выбором является Agent of Empires, нативно интегрирующийся с tmux и предоставляющий веб-доступ без дополнительных облачных зависимостей. Для сценария создания первичной среды разработки с параллельным запуском новых агентов лидером признается Superset (Paradev), предлагающий глубокую интеграцию с Git worktrees. Дальнейшие разделы подробно раскрывают технические нюансы, ограничения базовой архитектуры и результаты глубокого исследования рынка.

Анализ архитектуры Terminum и ограничений базовой линии

Для корректной оценки альтернатив необходимо установить точные функциональные границы текущего решения, основанного на Terminum (документация и исходный код доступны по адресу https://github.com/extractumio/terminum, дата обращения — 4 ноября 2026 года). В существующем рабочем процессе Terminum развернут внутри Windows WSL, тогда как сами процессы ИИ-агентов (Claude Code, OpenAI Codex) физически выполняются на удаленных хостах Linux и Mac. Terminum функционирует преимущественно как веб-эмулятор терминала и мультиплексор, транслирующий потоки псевдотерминала (PTY) в браузер.

Основное различие, которое необходимо провести при анализе отказоустойчивости, заключается в разнице между восстановлением макета терминала и фактическим возобновлением работы ИИ-агента. Когда клиентское устройство (например, iPad) отключается от сети, Terminum успешно сохраняет макет окон и последние строки вывода (stdout/stderr). При переподключении пользователь видит тот же экран. Однако эта устойчивость работает исключительно на уровне абстракции клиент-серверного соединения. Если происходит полная перезагрузка удаленного Linux-сервера, операционная система неизбежно завершает все запущенные процессы агентов. После перезагрузки Terminum способен восстановить интерфейс, но не может автоматически "оживить" убитый процесс Claude Code. Для фактического продолжения прерванной задачи требуется, чтобы сам оркестратор обладал семантическим пониманием состояния агента и умел инициировать процедуры гидратации, такие как вызов claude --resume или отправка команды session/load через протокол ACP.   

Существующие в вашей инфраструктуре скрипты уже успешно решают задачи ведения заметок (per-session notes) и отслеживания прогресса. Следовательно, дублирование этого функционала в новых инструментах имеет минимальную ценность. Основная потребность заключается в устранении громоздкой настройки WSL, получении прозрачной индикации статусов ожидания (когда агент требует подтверждения команды) и обеспечении доступа к удаленным файлам через мобильный браузер, избегая при этом создания дополнительных stdio-MCP процессов для каждой сессии. Очевидно, что сохранение Terminum остается рациональным решением только в том случае, если затраты на развертывание новых демонов на всех хостах превышают потребность в нативных мобильных интерфейсах и семантическом отслеживании состояния.

Исследование рынка и категоризация кандидатов

В ходе глубокого исследования рынка были проанализированы десятки инструментов, претендующих на роль оркестраторов ИИ-агентов. Чтобы сформировать релевантный шорт-лист, кандидаты были жестко отфильтрованы и разделены на четыре функциональные категории: удаленное управление существующими агентами (A), полноформатные оркестраторы сессий и рабочих деревьев (B), универсальные удаленные IDE (C) и исключительно десктопные приложения (D).

Категория универсальных удаленных IDE была полностью исключена из рассмотрения. Например, проект Coder / code-server (доступен по адресу https://github.com/coder/code-server, 4 ноября 2026 года) представляет собой мощный инструмент для развертывания облачных сред разработки, ориентированный на запуск полноценного VS Code в браузере. Хотя Coder имеет базовую поддержку MCP-серверов, он не специализируется на извлечении метаданных из CLI-агентов, таких как статусы ожидания подтверждения. Использование тяжеловесной IDE для простого управления пулом CLI-агентов является архитектурно избыточным. Аналогичным образом был отклонен проект Wave Terminal (https://github.com/wavetermdev/waveterm, 4 ноября 2026 года). Несмотря на наличие встроенного графического редактора, поддержки удаленных SSH-соединений и рендеринга Markdown, это десктопное кросс-платформенное приложение, не предоставляющее требуемого легковесного веб-интерфейса для доступа с iPad или телефона.   

Утилиты вроде ttyd и WeTTY также не прошли отбор, поскольку они лишь транслируют PTY-потоки в браузер, функционально повторяя базовые возможности Terminum, но не добавляя семантического слоя для распознавания состояний агентов. Проект Vibe Kanban (https://github.com/BloopAI/vibe-kanban, 4 ноября 2026 года) был исключен из шорт-листа, так как его архитектура жестко привязана к управлению пулл-реквестами через интеграцию с GitHub CLI. Это надстройка для управления проектами, а не мультиплексор низкоуровневых терминальных сессий. Наконец, инструмент cmux (https://cmux.com/, 4 ноября 2026 года) представляет собой нативный мультиплексор для macOS с программируемым сокет-API, но он сознательно позиционируется как примитив, а не как оркестратор с готовым веб-интерфейсом для мобильных устройств, что делает его неподходящим для заявленного рабочего процесса.   

В результате строгого отсева был сформирован финальный список из шести подлинно сопоставимых кандидатов: Agent of Empires, Happy (Happy Coder), CloudCLI (Claude Code UI), Superset (Paradev), HAPI и Agent Deck.

Сравнительная матрица функциональных возможностей

Для объективного сравнения отобранных инструментов была составлена детализированная матрица, опирающаяся исключительно на официальную документацию, релизы и описания архитектуры по состоянию на 4 ноября 2026 года. Таблица ниже синтезирует критически важные для вашего рабочего процесса параметры.

Характеристика	Agent of Empires (AoE)	Happy (Happy Coder)	CloudCLI (Claude Code UI)	Superset (Paradev)	HAPI	Agent Deck
Официальный ресурс (проверено 04.11.2026)	

https://github.com/agent-of-empires/agent-of-empires

	

https://github.com/slopus/happy

	

https://github.com/siteboon/claudecodeui

	

https://github.com/superset-sh/superset

	

https://github.com/tiann/hapi

	

https://github.com/asheshgoplani/agent-deck


ОС (Сервер / Клиент)	

Сервер: Linux, macOS, WSL. Клиент: Web

	

Сервер: Mac, Win, Linux. Клиент: iOS, Android, Web

	

Сервер: Node.js (любая ОС). Клиент: Web

	

Сервер: macOS, Linux AppImage. Клиент: Web/Desktop

	

Сервер: Mac, Win, Linux. Клиент: Terminal/Web

	

Сервер: macOS, Linux, WSL. Клиент: Terminal


Мобильный доступ / Интерактивный ввод	

Да (PWA веб-дашборд с адаптивным дизайном, полный контроль)

	

Да (Нативные приложения iOS/Android, встроенный чат-интерфейс)

	

Да (Адаптивный Web UI, доступен через IP:port)

	

Да (Только через Superset Pro для iOS, требует удаленного доступа)

	

Ограниченно (Связка Remote -> Local handoff)

	

Нет (Только мост через ботов Telegram/Slack)


Подключение к существующим / Новые сессии	

Подключается к существующим tmux, запускает новые

	

Управляет собственным демоном, восстанавливает сессии из SQLite

	

Автоматически сканирует «внутренний путь лаборатории» на наличие старых сессий и создает новые

	

Инициирует новые сессии, изолированные в Git worktrees

	

Поддерживает hapi resume и запуск новых сессий через ACP

	

Запускает новые сессии, перехватывает TUI


Поддержка Claude Code, Codex и др.	

Да / Да (также OpenCode, Gemini, Copilot, Hermes)

	

Да / Да (также Grok, Gemini, ACP агенты)

	

Да / Да (также Cursor CLI, GPT модели)

	

Да / Да (поддержка 100+ параллельных агентов)

	

Да / Да (также Grok, DeepSeek Harness, Pi)

	

Да / Да (также Gemini CLI, OpenCode, Pi)


Требования к авторизации (CLI vs API)	

Переиспользует существующие CLI-токены (claude auth)

	

Переиспользует существующие подписки, не требует своих ключей

	

Использует локальную конфигурацию CLI («внутренний путь лаборатории»)

	

Переиспользует локальные CLI токены, не проксирует API

	

Опирается на локальную аутентификацию (напр., Grok login)

	

Использует токены самого инструмента (l запускает login)


Множественные SSH хосты	

Управление удаленными сессиями поверх SSH и Tailscale

	

Фоновый демон поддерживает удаленные подключения (Remote Agent beta)

	

Рекомендуется Tailnet/LAN для доступа к локальному порту сервера

	

Поддержка удаленных хостов (Wake offline hosts, SSH remote environments)

	UNKNOWN (Не описана прямая мульти-SSH оркестрация)	

Да (Запуск через dot-agent-deck connect по SSH)


Персистентность (tmux) / Дисконнект	

Каждый агент в своем tmux. При обрыве связи всё продолжает работать

	

Работает без tmux, состояние сохраняется демоном в SQLite непрерывно

	

Выживает при отключении UI (процессы остаются в фоне Node.js сервера)

	

Сессии персистентны, терминалы выживают при перезапуске клиента

	

UNKNOWN (Полагается на ACP session/load)

	

Запускается как фоновый демон, выживает при отключении TUI


Семантика перезагрузки сервера	

ОС убивает процесс и tmux. Требуется ручной перезапуск или ACP восстановление

	

Демон стартует автоматически, восстанавливает метаданные из локальной БД

	

Сканирует локальную историю, но требует реинициализации процессов

	

Восстанавливает терминалы, но может потребовать переподключения агента

	

Механизм session/load пытается восстановить контекст после рестарта

	

Автоматический рестарт сессий через session restart


Уведомления / Статусы (Ожидание)	

Нативная поддержка ACP: статусы Running, Waiting, Idle; Swipe-to-approve

	

E2EE push-уведомления об ошибках и запросах разрешений

	

Встроенная очередь задач, UI подтверждения действий, интеграция с TaskMaster

	

Индикаторы выполнения, бейджи в доке при необходимости внимания

	

Маршрутизация разрешений (default/auto/bypass) через ACP

	

Переходы статусов (running -> waiting) передаются в daemon


Удаленные файлы / Загрузка изображений	

Интегрированный просмотр диффов, доступ к директориям через терминал

	

Полноценный встроенный файловый браузер

	

Интерактивное дерево файлов, загрузка в локальную среду

	

Встроенный Diff Viewer, браузер для превью

	

Локальный доступ, удаленный режим ограничен передачей контекста

	

Управление Git worktrees, загрузка через TUI


Отображение вывода / Закладки	

Рендеринг сырого вывода терминала через xterm.js в веб-интерфейсе

	

Чат-интерфейс, форматированный вывод

	

Чат-интерфейс с поддержкой Markdown и экспортом (PDF/HTML)

	

Встроенный чат, поддержка комментариев к строкам диффа

	

Точный рендеринг LaTeX и Markdown

	

TUI рендеринг ANSI-цветов, буфер на 160KB


Скрипты планов / Заметки	

Поддержка хуков on_create, on_destroy в .agent-of-empires/config.toml

	

Механизм авто-компактизации контекста, внутренние логи сессий

	

Интеграция планирования через TaskMaster AI

	

Исполнение скриптов настройки .superset/setup.local.sh

	

Поддержка режима планирования (plan mode)

	

Управление хуками Markdown-задач


Self-host vs Vendor Cloud	

Исключительно Self-host. Опционально CityHall для мульти-арендности

	

Доступен Cloud relay, но полностью поддерживается Self-host реле

	

Поддерживает как локальный запуск (Self-host), так и CloudCLI Cloud

	

Приложение бесплатно для локального запуска, облачные фичи опциональны

	

Полностью локальный инструмент

	

Полностью локальный (bind to 127.0.0.1)


Границы безопасности	

Passphrase авторизация для веб-дашборда, Tailscale защита

	

Полное E2E шифрование (X25519, AES-256-GCM), сервер не видит данных

	

В локальном режиме порт открыт, требуется защита на уровне сети/VPN

	

Данные не проксируются, API-ключи остаются на устройстве

	

Переиспользует локальную модель безопасности CLI

	

Связь по локальному сокету, Token-paired браузерный мост


Лицензия	

MIT License

	

MIT License

	

AGPL-3.0-or-later

	

Elastic License 2.0 (MIT для базовых компонентов)

	UNKNOWN (Open-source, специфика не указана)	

MIT License


Цена / Затраты на внедрение	

Бесплатно и Open Source

	

Базовый функционал бесплатно, голосовой помощник по подписке ($19.99/мес)

	

Self-host бесплатно, Cloud от €7/мес

	

Десктоп бесплатно, Pro (включая мобильный доступ) платно

	Бесплатно	Бесплатно
Релизы и зрелость	

Активное сообщество, частые релизы миграций БД (v028)

	

Стабильные релизы десктопа (v0.0.88) и CLI (v1.2.6)

	

Версия 1.37.7 (Октябрь 2026), активная разработка

	

Версия 2.0.3, кросс-платформенные сборки

	

Версия 0.7.2, относительно узкая база

	

Стабильная версия 1.0, 6.5k загрузок

  
Детальный анализ архитектур и инкрементальной ценности

Понимание инкрементальной ценности предлагаемых решений невозможно без осознания фундаментального сдвига, который привнес Agent Client Protocol (ACP). Традиционные терминальные мультиплексоры (включая Terminum) относятся к потоку ввода-вывода как к неструктурированному тексту. Это означает, что когда Claude Code запрашивает разрешение на выполнение деструктивной команды rm -rf, оркестратор не знает об этом, пока пользователь визуально не прочтет текст на экране. Интеграция ACP позволяет агентам и клиентским интерфейсам обмениваться типизированными сообщениями в формате JSON-RPC. Благодаря этому мобильный телефон может отрендерить не просто сжатый текст терминала, а четкую карточку запроса с кнопками "Утвердить" или "Отклонить", формируя структурированные переходы состояний (Running -> Waiting -> Idle).   

Agent of Empires (AoE)

Согласно документации на официальном сайте проекта (https://www.agent-of-empires.com/, по состоянию на 4 ноября 2026 года), AoE архитектурно не пытается заменить tmux, а органично оборачивает его. Каждый агент запускается в собственной сессии tmux, что гарантирует бесперебойную работу при закрытии ноутбука или обрыве SSH-соединения. Принципиальное преимущество для вашей инфраструктуры заключается в том, что PWA веб-дашборд может быть выставлен напрямую во внутреннюю сеть через существующий Tailscale funnel. Инструмент использует ACP для точного определения статусов ожидания (waiting for input). При запуске новых задач AoE автоматически управляет изолированными Git worktrees и, опционально, Docker-песочницами, что предотвращает конфликты при параллельном редактировании файлов.   

Superset (Paradev)

Superset, документация которого доступна по адресу https://github.com/superset-sh/superset (дата обращения — 4 ноября 2026 года), предлагает концепцию "агентской IDE". Его отличительной особенностью является способность масштабироваться до сотен параллельных агентов без конфликтов. Это достигается за счет принудительного использования Git worktrees для каждой новой задачи, где каждый агент работает в своей изолированной ветке. Встроенный diff-viewer позволяет инженеру рецензировать изменения кода в удобном формате перед их слиянием в основную ветку. Инструмент не проксирует API-вызовы, напрямую используя локальные ключи и аутентификацию CLI. Однако доступ с мобильных устройств (iOS) возможен только при наличии платной подписки Superset Pro и настроенного удаленного доступа к хост-машине.   

Happy (Happy Coder)

Проект Happy (https://github.com/slopus/happy, 4 ноября 2026 года) представляет собой радикально иной подход, фокусируясь на непрерывности и безопасности мобильного доступа. Архитектура исключает использование tmux. Вместо этого фоновый демон (happy-agent) сохраняет каждое состояние сессии непосредственно в базу данных SQLite, обеспечивая высочайший уровень устойчивости — при падении или перезапуске демона сессии восстанавливаются с диска. Взаимодействие между телефоном и рабочим компьютером происходит через реле-сервер (который можно поднять самостоятельно) с использованием строгой системы сквозного шифрования (E2EE), базирующейся на ключах X25519 и шифре AES-256-GCM. Это гарантирует, что исходный код никогда не передается в открытом виде. Интерфейс на телефоне реализован в виде нативного приложения с поддержкой чата, а не эмуляции терминала.   

CloudCLI (Claude Code UI)

CloudCLI (https://github.com/siteboon/claudecodeui, 4 ноября 2026 года) — это легковесный Node.js веб-интерфейс, который выделяется способностью автоматически обнаруживать и подключаться к существующим сессиям путем сканирования директории «внутренний путь лаборатории» Инструмент предоставляет адаптивный веб-интерфейс с интегрированным файловым менеджером, браузером Git-деревьев и полноценным терминалом на базе xterm.js. Поставляется под лицензией AGPL-3.0 и может быть развернут локально бесплатно, предоставляя доступ к агентам (Claude Code, Cursor CLI, Codex) через браузер по локальному IP или через Tailscale.   

Рекомендации и ранжирование

На основе предоставленных вводных данных, где ключевыми требованиями являются агрегированный список сессий, четкие состояния ожидания/утверждения, доступ через браузер с различных устройств (включая iPad) поверх существующего Tailnet, а также отсутствие желания усложнять развертывание новыми облачными зависимостями, сформированы следующие рекомендации.

Сценарий 1: Удобный дашборд для управления существующим парком машин

В данном сценарии главная цель — получить визуальный контроль над уже работающими на Windows, Mac и Linux агентами, не разрушая текущие рабочие процессы на базе SSH и tmux.

Лидер: Agent of Empires (AoE)
Причина: Архитектура AoE идеально накладывается на вашу инфраструктуру. Поскольку вы уже используете tmux и Tailscale, развертывание AoE на always-on Linux сервере позволит выставить PWA веб-дашборд напрямую в ваш Tailnet. Вы получите требуемый доступ с iPad и телефона через веб-браузер без установки нативных приложений и без маршрутизации трафика через внешние реле-серверы. Использование протокола ACP обеспечит четкое понимание состояний (Waiting/Idle), устраняя необходимость постоянно проверять терминалы на предмет зависших запросов разрешений. Отказ от Terminum в пользу AoE значительно упростит развертывание, избавив от необходимости поддерживать мосты через Windows WSL.   

Запасной вариант: CloudCLI (Claude Code UI)
Причина: Если Docker-песочницы и хуки AoE окажутся избыточными, CloudCLI предоставит минималистичный Node.js интерфейс. Его главная ценность — автоматическое обнаружение сессий в «внутренний путь лаборатории», что позволяет легко подхватывать работу, инициированную из обычного терминала. Развертывание сводится к запуску одной команды npx в сети Tailscale.   

Сценарий 2: Первичная среда для запуска новых агентских задач

В данном сценарии фокус смещается на инициализацию десятков конкурентных сессий, требующих жесткой изоляции файлов и последующего слияния кода.

Лидер: Superset (Paradev)
Причина: Superset концептуально спроектирован для решения проблемы конфликтов при параллельной работе ИИ-агентов. Принудительное создание Git worktree для каждой новой задачи гарантирует, что агенты не будут перезаписывать файлы друг друга. Встроенный интерфейс просмотра диффов (diff viewer) радикально ускоряет процесс ревью кода перед его вливанием в основную ветку. Несмотря на то, что мобильный доступ здесь реализован хуже (требуется версия Pro и настройка удаленного доступа), как среда для запуска и ревью на десктопе, Superset не имеет себе равных.   

Запасной вариант: Happy (Happy Coder)
Причина: Если инициализация новых задач часто происходит на ходу с мобильного телефона, E2E-шифрованное приложение Happy обеспечивает непревзойденный UX. Архитектура демона с сохранением состояния в SQLite гарантирует, что созданные сессии не пропадут даже при сбоях. Однако необходимость запуска реле-сервера (или доверия вендорскому облаку) является компромиссом, который необходимо учитывать.   

Когда рационально сохранить Terminum?
Отказ от миграции и сохранение Terminum имеет смысл в узком сценарии: если ваши существующие bash-скрипты, отвечающие за генерацию планов и заметок, имеют жесткую, низкоуровневую привязку к пайпам Terminum или структуре его PTY, которую экономически нецелесообразно переписывать под API tmux или демоны новых оркестраторов. Также, если в реальности ваши задачи имеют исключительно пакетный характер и агенты редко требуют интерактивных разрешений, внедрение ACP-совместимых оркестраторов не принесет ощутимого сокращения времени простоя.

Ограниченный план тестирования (Bounded Evaluation Plan)

Для валидации предложенных рекомендаций (с фокусом на Agent of Empires как на лидера для контроля флота) предлагается провести серию из пяти изолированных тестов. Тестирование не должно затрагивать производственные серверы.

Задача 1: Валидация изоляции через Git Worktrees

Действие: Используя веб-дашборд AoE на тестовом узле, инициировать две параллельные сессии Claude Code, поручив им рефакторинг одного и того же крупного модуля.

Условия приемки: AoE должен автоматически создать два независимых worktree. Агенты не должны блокировать файлы друг друга. Встроенный diff-viewer должен позволить просмотреть и слить обе ветки без потери данных.

Необходимые данные: Логи создания веток Git, время выполнения задач.

Задача 2: Тестирование разрыва клиентского соединения (Client Disconnect)

Действие: Подключиться к веб-дашборду AoE с iPad через Tailscale. Запустить в Codex длительную задачу (например, генерацию тестов для 50 файлов). Принудительно отключить Wi-Fi на iPad на 10 минут.

Условия приемки: Поскольку сессия опирается на серверный tmux, задача должна продолжать выполняться. При переподключении iPad интерфейс должен мгновенно синхронизироваться, отображая актуальный статус без необходимости вводить команды восстановления.

Задача 3: Тестирование перезагрузки хоста (Host Reboot)

Действие: Во время генерации кода инициировать команду sudo reboot на тестовом Linux-узле.

Условия приемки: После загрузки хоста операционная система неизбежно убьет процесс агента и сессию tmux. Тест считается успешным, если AoE сохранил метаданные сессии и позволяет в один клик инициировать команду гидратации (например, через ACP session/load или claude --resume) для возобновления работы с прерванного места.

Задача 4: Перехват разрешений (Status/Approval via ACP)

Действие: Отправить агенту промпт, требующий выполнения деструктивной shell-команды (например, удаление временной директории).

Условия приемки: Вместо того чтобы выводить в терминал стандартный текст "Allow this command? (y/n)", веб-дашборд на смартфоне должен перехватить JSON-RPC запрос через ACP и отрендерить нативную UI-карточку с кнопками утверждения/отклонения. Статус сессии в агрегированном списке должен смениться с "Running" на "Waiting".

Задача 5: Доступ к удаленным файлам

Действие: Через веб-дашборд на iPad открыть конфигурационный файл удаленного Linux-сервера, внести изменения и сохранить.

Условия приемки: Изменения должны быть корректно записаны на диск удаленной машины с сохранением оригинальных прав доступа (permissions) и владельца файла, без необходимости открывать отдельную SSH-сессию.

Критерии опровержения рекомендации:
Рекомендация в пользу Agent of Empires будет аннулирована, если в ходе тестирования обнаружится, что реализация протокола ACP в AoE конфликтует с текущими версиями Claude Code, приводя к зависанию статуса "Waiting", или если веб-интерфейс потребляет избыточные ресурсы оперативной памяти при рендеринге больших диффов на мобильных устройствах. В таком случае фокус должен сместиться на CloudCLI для обеспечения веб-доступа и Superset для изоляции рабочих сред. Отсутствие идеального механизма гидратации при жесткой перезагрузке хоста (Задача 3) не является поводом для полной дисквалификации, так как ни один из рассмотренных оркестраторов не способен предотвратить физическое уничтожение процесса ядром ОС, однако разница в удобстве последующего восстановления является критическим фактором выбора.

Представленный анализ опирается на верифицируемые данные из официальных репозиториев и документации проектов. Степень уверенности в архитектурных особенностях высока, однако в областях, где официальная документация не предоставляет явных подтверждений (помечено как UNKNOWN), требуется эмпирическая проверка в рамках предложенного плана тестирования.

## Источники (ссылки панели Gemini)
https://github.com/tiann/hapi/blob/main/docs/guide/agents.md
https://github.com/coder/code-server
https://github.com/coder/coder/blob/main/server.json
https://github.com/wavetermdev/waveterm
https://github.com/code/app-waveterm
https://vibekanban.com/docs/integrations/github-integration
https://medium.com/@gokulofficial18602/im-about-to-try-vibe-kanban-the-ai-agent-orchestration-platform-that-might-change-how-we-code-ed25f79ba262
https://explainx.ai/skills/aradotso/trending-skills/cmux-terminal-multiplexer
https://dev.to/arshtechpro/cmux-the-native-macos-terminal-built-for-running-ai-coding-agents-in-parallel-52il
https://github.com/agent-of-empires/agent-of-empires
https://github.com/slopus/happy
https://github.com/siteboon/claudecodeui/blob/main/README.md
https://github.com/superset-sh/superset?ref=sharedsapience.com
https://github.com/asheshgoplani/agent-deck
https://yetanotherorchestrator.app/apps/agent-of-empires/
https://github.com/slopus/happy/blob/main/README.md
https://cc.deeptoai.com/docs/en/tools/happy-mobile-claude-code-client
https://github.com/siteboon/claudecodeui/blob/main/docs/README.zh-CN.md
https://github.com/paradevai/paradev
https://www.agent-of-empires.com/
https://skillsllm.com/skill/superset
https://github.com/mvanhorn/agent-deck/blob/main/README.md
https://prompts.brightcoding.dev/blog/stop-juggling-ai-agents-use-agent-of-empires-instead
https://github.com/slopus/happy-agent/blob/main/README.md
https://happy.engineering/how-it-works/
https://yuv.ai/blog/superset
https://paradev.com/
https://github.com/slopus/happy-agent
https://www.agent-of-empires.com/guides/mcp-control/
https://github.com/newtv-ai/switchboard
https://github.com/vfarcic/dot-agent-deck
https://github.com/siteboon/claudecodeui/releases
https://www.agent-of-empires.com/docs/structured-view/
https://sourceforge.net/projects/happy-coder.mirror/
https://www.blog.brightcoding.dev/2026/02/19/happy-coder-the-secure-mobile-cli-revolution
https://jtianling.com/en/agent-of-empires-intro.html
https://github.com/siteboon/claudecodeui/blob/main/docs/README.ru.md
https://www.reddit.com/r/coding_agents/comments/1tu153i/agent_deck_opensource_mac_app_for_managing_ai/
https://news.ycombinator.com/item?id <tg-id>
https://github.com/flashclub/agent-deck
https://github.com/agent-of-empires/agent-of-empires/blob/main/docs/guides/repo-config.md
https://github.com/b1ank1108/awesome-stars/blob/master/README.md
https://github.com/agent-of-empires
https://github.com/slopus/happy-server
https://happy.engineering/docs/faq/
https://maude.pilotdev.fr/compare/maude-vs-happier/
https://ntfyx.me/docs/coding-agents-on-your-phone/
https://www.holdgrenade.com/vs/happy
https://github.com/agent-of-empires/agent-of-empires/releases
https://github.com/slopus/happy/releases
https://github.com/slopus/happy-desktop/releases
https://github.com/siteboon/claudecodeui/blob/main/CHANGELOG.md
https://github.com/superset-sh/superset/releases
https://github.com/tiann/hapi/actions/runs/20879608582
https://dev.to/tigergethigher/what-is-agent-client-protocol-acp-and-why-it-matters-for-developer-tools-5ck5
https://omidsaffari.com/blog/agent-client-protocol
https://github.com/slopus/happy-agent/blob/main/THIRD-PARTY-NOTICES.md
https://github.com/slopus/slopus.github.io
https://mobilecli.app/compare/happy-coder/
https://github.com/slopus/happy/discussions/75
https://codeongrass.com/blog/grass-vs-happy-coder/
https://happy.engineering/docs/guides/happy-coder-best-practices/
https://github.com/slopus/happy/issues/772
https://skillsllm.com/skill/hapi
https://hapi.run/docs/guide/installation
https://github.com/tiann/hapi?ref=producthunt
https://github.com/tiann/hapi/blob/main/README.md
https://github.com/tiann/hapi
https://github.com/tiann/hapi?ref=agents-report.net
https://followagents.com/en/agents/hapi
https://github.com/aslafy-z/hauth
https://www.reddit.com/r/CloudCLI/comments/1u6j4m6/welcome_to_rcloudcli_links_setup_and_what/
https://skillsllm.com/skill/agent-deck
https://github.com/NVitlam/agent-deck
https://github.com/xuan-team/agent-deck-companion
https://github.com/harry123180/agent-deck
https://github.com/buildermethods/agent-os/discussions/170
https://virtuslab.com/blog/ai/vibe-kanban
https://github.com/chrisbanes/vibe-kanban-orchestrator
https://github.com/BloopAI/vibe-kanban
https://github.com/BloopAI/vibe-kanban/blob/main/docs/configuration-customisation/agent-configurations.mdx
https://github.com/tryemerge/vibe-factory
https://github.com/yigitkonur/mcp-vibekanban/issues
https://www.waveterm.dev/
https://github.com/wavetermdev/waveterm-docs
https://github.com/isaccanedo/waveterm
https://forum.snapcraft.io/t/classic-confinement-request-for-waveterm/43562
https://github.com/sanshao85/tideterm
https://github.com/isaccanedo/waveterm/blob/main/BUILD.md
https://sourceforge.net/projects/wave-terminal.mirror/
https://www.reddit.com/r/commandline/comments/1bff001/announcing_wave_terminal_an_opensource_ainative/
https://www.reddit.com/r/selfhosted/comments/ayjf2r/github_codercomcodeserver_run_vs_code_on_a_remote/
https://github.com/coder/coder/blob/main/contributing/templates.md
https://github.com/coder/coder/releases
https://github.com/coder/coder/blob/main/docs/tutorials/template-from-scratch.md
https://github.com/coder/coder
https://github.com/coder/coder-1
https://github.com/agent-of-empires/agent-of-empires/blob/main/.github/CODEOWNERS
https://www.6erskills.com/skill/njbrake/agent-of-empires
https://github.com/hesreallyhim/awesome-claude-code/issues/483
https://cmux.com/
https://github.com/noamsiegel/git-wt-cmux
https://mcpmarket.com/server/cmux-agent
https://github.com/manaflow-ai/cmux
https://github.com/manaflow-ai/cmux/issues/8737
https://github.com/coder/xum
https://github.com/amirlehmam/wmux
https://ice-ice-bear.github.io/posts/2026-03-23-tmux-cmux/
https://github.com/superset-sh/skills
https://github.com/superset-sh/superset/blob/main/apps/docs/content/docs/agent-integration.mdx
https://github.com/siteboon/claudecodeui
https://github.com/HandyS11/cloudcli-plugin-claude-usage
https://www.natecue.com/en/resources/cloudcli-remote-web-ui-claude-code/
https://skills.snappy.ai/skills/snappy-agent-host
https://news.ycombinator.com/item?id <tg-id>
https://github.com/slopus/happy/issues/246
https://www.reddit.com/r/ClaudeCode/comments/1r6cou0/how_do_you_keep_claude_code_running_247_and/
https://vibecodinghub.org/blog/agent-of-empires-review
https://github.com/bradAGI/awesome-cli-coding-agents?ref=savageevan.com
https://awesome.ecosyste.ms/lists/dspachos%2Fawesome-ai-tools
https://www.reddit.com/r/opensourcescribes/
https://skillsllm.com/skill/awesome-agent-orchestrators
https://github.com/agent-of-empires/agent-of-empires/issues/2287
https://greenlitbooks.com/field-notes
https://github.com/agent-of-empires/agent-of-empires/blob/main/docs/guides/web-dashboard.md
https://news.ycombinator.com/item?id <tg-id>
https://github.com/siteboon/claudecodeui/blob/main/docs/README.ja.md
https://github.com/siteboon/claudecodeui/blob/main/docs/README.de.md
https://github.com/siteboon/claudecodeui/blob/main/docs/README.tr.md
https://github.com/siteboon/claudecodeui/blob/main/package.json
https://github.com/siteboon/claudecodeui/blob/main/docs/README.zh-TW.md
https://github.com/tsl0922/ttyd
https://github.com/slopus/happy-docs
https://github.com/slopus/happy/blob/main/packages/happy-cli/README.md
https://github.com/slopus/happy/issues/1153
https://github.com/slopus/happy-agent/blob/main/DEVELOPMENT.md
https://github.com/slopus/happy-agent/blob/main/master-plans/25-releases.md
https://github.com/njbrake/agent-of-empires/blob/main/AGENTS.md
https://github.com/LN-Zap/agent-of-empires-security-review/blob/main/README.md
https://github.com/agent-of-empires/agent-of-empires/blob/main/docs/installation.md
https://github.com/agent-of-empires/agent-of-empires/blob/main/docs/guides/sandbox.md
https://github.com/njbrake/agent-of-empires/blob/main/README.md
https://github.com/agent-of-empires/agent-of-empires/issues/2761
https://github.com/superset-sh/superset/blob/main/apps/docs/content/docs/automations.mdx
https://www.iamtayyab.com/blog/mobile-remote-coding-agent-apps-security
https://github.com/buzzni/saycode-desktop-releases
https://betterstack.com/community/guides/ai/agent-of-empires/
https://github.com/agent-of-empires/agent-of-empires/blob/main/docs/guides/configuration.md
https://github.com/superset-sh/superset/issues/5729
https://happy.engineering/recepies/harnesses/
https://zed.dev/blog/acp-registry
https://www.reddit.com/r/Jetbrains/comments/1pewvju/bring_your_own_ai_agent_using_agent_client/
https://blog.brokk.ai/muse-acp-wire-muse-spark-up-to-intellij-zed-and-whatever-else/
https://github.com/angristan/awesome-stars/blob/master/README.md?GUbeL0UL=EB5XS92cyuGB6S
https://github.com/devswha/ovis/blob/main/DESIGN.md
https://github.com/bradagi/awesome-cli-coding-agents