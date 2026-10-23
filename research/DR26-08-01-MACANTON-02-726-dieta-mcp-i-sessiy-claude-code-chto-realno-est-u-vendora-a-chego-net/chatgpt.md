# DR26-08-01-MACANTON-02-726 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-726
rail: chatgpt (codex exec (gpt-5.6-sol, подписка ChatGPT a2))
session_uuid: 019fbdbb-71c9-7d82-b847-9e1c0a44db67
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:34:30Z
proof_of_work: 11 уник. URL, 15 веб-поисков — гейт НЕ ПРОЙДЕН (<15 уник. URL)
note: чистый прогон; 11 уник. URL — ниже гейта 15, но 15 веб-поисков = proof-of-work есть. Корень: тема (внутренности Claude Code/MCP) бедна публичными цитируемыми источниками, а не рельса села
---

Ниже — состояние публичных источников на 1 августа 2026 года. Главный вывод: ToolSearch экономит контекст, но не является механизмом ленивого запуска MCP. Для снижения RAM нужны отключение соединения с сервером, вынос тяжёлого состояния в HTTP-демон либо остановка самой idle-сессии.

Отдельная оговорка: `24 × RSS` может завышать реальное физическое потребление, поскольку RSS каждого процесса включает разделяемые отображения бинаря и библиотек. Для диеты ориентируйтесь также на `phys_footprint`, private/dirty pages и дельту system memory pressure, а не только на сумму RSS.

## 1. Диета одной сессии

### Что документировано

В Claude Code 2.1.219 есть несколько поддерживаемых поверхностей:

- `/mcp` позволяет отключить сервер без удаления конфигурации. Состояние записывается per-project в «внутренний путь лаборатории»:
  - `disabledMcpServers` — opt-out для обычных, plugin-, connector- и default-on builtin-серверов;
  - `enabledMcpServers` — opt-in для default-off builtin-серверов.
- `computer-use` прямо назван default-off. Если его нет в `enabledMcpServers`, Claude Code не должен к нему подключаться.
- Зарезервированы встроенные имена `workspace`, `claude-in-chrome`, `computer-use`, `Claude Preview` и `Claude Browser`. [Документация MCP Claude Code](https://code.claude.com/docs/en/mcp)
- `--chrome` включает Chrome-интеграцию только для данной сессии.
- `--tools "Bash,Edit,Read"` ограничивает встроенные инструменты, но документация специально предупреждает: флаг не управляет MCP.
- `--disallowedTools "mcp__*"` удаляет MCP-tools из доступного модели набора, однако надёжный способ вообще не загружать конфиг — `--strict-mcp-config` без `--mcp-config`.
- `--bare` пропускает MCP, plugins, hooks, skills, auto-memory и `CLAUDE.md`; предназначен прежде всего для быстрых scripted-вызовов.
- `--safe-mode` отключает все пользовательские кастомизации и MCP, но оставляет штатные built-in tools. Это хороший диагностический baseline. [CLI reference](https://code.claude.com/docs/en/cli-usage)
- `disableClaudeAiConnectors` работает в любом settings scope и запрещает автоматическое подключение claude.ai-коннекторов.
- `disableMobileSimulatorTools` существует, но только как managed setting. Он блокирует доступ Claude к iOS Simulator tools; документация не обещает выгрузку соответствующего кода из процесса.
- Есть также `disableArtifact`, `disableBundledSkills`, `disableAgentView`, `disableAllHooks`, `disableBrowserExternalNavigation`. Последний управляет навигацией, а не обязательно существованием Browser MCP runtime. [Settings reference](https://code.claude.com/docs/en/settings)

Что не найдено в публичной документации:

- лимит JS/Bun heap или RSS одной сессии;
- флаг вида `disableBuiltinMcpPresets`;
- отдельные глобальные switches для `workspace`, Preview/Browser или «visualization» как универсального пресета;
- обещание, что deny tool или `disableMobileSimulatorTools` физически выгружает реализацию из адресного пространства;
- vendor breakdown базовых ~340 МБ.

### Чем, вероятно, складывается RSS

Из наблюдаемой архитектуры и changelog:

- JS/Bun runtime, TUI/rendering, event loop и сетевой клиент;
- transcript/conversation state и структуры текущего контекста;
- tool metadata, server instructions, MCP connection state;
- hooks/plugins/skills/LSP и другие обнаруженные расширения;
- native-модули. Например, Anthropic отдельно сообщал, что native image processor теперь загружается при первом использовании — то есть некоторые компоненты действительно поддерживают runtime lazy loading;
- allocator arenas, JIT/compiled code, caches и memory-mapped binary pages.

Сообщество подтверждает рост памяти в длинных сессиях и при subagent/parallel-agent нагрузке, вплоть до heap/OOM, но issue reports не дают стабильной формулы «база + X МБ на пресет». Есть также практическое подтверждение, что дублированные stdio-MCP заметно ухудшают memory pressure и стабильность. [Пример issue с несколькими сессиями и MCP](https://github.com/anthropics/claude-code/issues/26224)

**Уверенность:**

- управление `/mcp`, `--bare`, `--safe-mode`, `--strict-mcp-config`: **established**;
- `computer-use` default-off: **established**;
- отключённый builtin не устанавливает MCP-соединение: **established**;
- это существенно уменьшит базовые 340 МБ: **speculative**, пока не измерено;
- точный состав 340 МБ: **speculative**.

Практический A/B для вашей машины:

1. Новый чистый процесс `--safe-mode`.
2. Новый `--bare`.
3. Обычный процесс, все default-on builtins отключены через `/mcp`.
4. Обычный процесс с текущим профилем.
5. После одинакового короткого prompt измерить `phys_footprint`, private dirty и system-wide memory delta.

Если `safe-mode ≈ normal`, основная масса находится в ядре сессии, а не MCP-пресетах. Если `bare` заметно легче, добавлять обратно подсистемы по одной.

## 2. Ленивая загрузка tools

### Что документировано

ToolSearch откладывает именно schemas/context exposure:

- по умолчанию MCP tools deferred;
- в контекст входят только найденные/использованные tools;
- `ENABLE_TOOL_SEARCH=auto[:N]` управляет порогом;
- `alwaysLoad: true` возвращает upfront schema loading для сервера или отдельного tool. [Tool-search section](https://code.claude.com/docs/en/mcp)

Но та же документация говорит:

- MCP-серверы подключаются в фоне при старте;
- после успешного соединения выполняются discovery-запросы `tools/list`, `prompts/list`, `resources/list`;
- ToolSearch при необходимости ждёт сервер, который ещё подключается;
- HTTP/SSE соединения автоматически переподключаются;
- `alwaysLoad` делает старт блокирующим, тогда как остальные серверы продолжают соединяться асинхронно.

Следовательно:

> Deferred tool ≠ deferred MCP server/runtime.

У stdio Claude Code всё равно должен запустить процесс, провести `initialize` и получить каталог, чтобы ToolSearch знал, что искать. Для builtin in-process реализаций документация не раскрывает, загружается ли конкретный модуль только при подключении; доказано лишь, что native image processor имеет отдельную ленивую загрузку.

Поддерживаемого `startOnFirstToolCall` в конфигурации Claude Code не найдено.

### Практика сообщества

Прокси и gateway часто реализуют двухступенчатую модель:

1. Gateway постоянно держит компактный каталог.
2. Конкретный backend/container запускается при первом вызове.

Это уже не ToolSearch Claude Code, а lifecycle policy gateway.

**Уверенность:**

- schemas deferred: **established**;
- configured MCP connects/initializes до первого реального tool call: **established**;
- builtin preset code полностью загружается при соединении: **speculative**;
- vanilla Claude Code умеет cold-start сервера по первому вызову: **не подтверждено / фактически нет публичной ручки**.

## 3. Общий демон и MCP gateway

### Когда shared HTTP — правильная норма

Shared Streamable HTTP особенно уместен для серверов, которые:

- держат дорогой внешний connection pool, SDK client или большой read-only cache;
- представляют один внешний аккаунт/устройство: Telegram, WhatsApp, browser farm, database pool;
- обслуживают много локальных агентов;
- требуют централизованных audit logs, rate limits и credential management;
- могут надёжно разделять user/session state.

Stdio остаётся разумным для:

- лёгких локальных stateless tools;
- одноразовых CLI wrappers;
- filesystem/shell tools, где process boundary даёт полезную изоляцию;
- серверов, использующих session-local cwd, environment или ephemeral credentials;
- недоверенного кода, который лучше убивать вместе с клиентом.

В актуальном MCP стандартным HTTP-транспортом является **Streamable HTTP**. Старый HTTP+SSE оставлен для совместимости и считается deprecated. Ваш Telegram SSE-демон архитектурно правильнее 48 stdio-процессов, но при следующем плановом обновлении лучше предоставить Streamable HTTP endpoint, сохранив SSE только для старых клиентов. [MCP transport specification](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports)

### Готовые решения

Наиболее релевантные вашему локальному сценарию:

- **Docker MCP Gateway** — общий gateway, container isolation, credential injection, resource controls и запуск backend-контейнера при первом tool call. Это наиболее прямое готовое воплощение «один порт + lazy process start». Часть AI Governance сейчас invite-only, но gateway также распространяется отдельно. [Docker MCP Gateway](https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/)
- **Microsoft MCP Gateway** — Kubernetes-oriented, session-aware routing, distributed session store, auth через Entra ID, проксирование локальных stdio и remote HTTP servers. Это скорее серверная/кластерная система, избыточная для одного Mac. Некоторые agent/session части прямо помечены preview. [Microsoft MCP Gateway](https://github.com/microsoft/mcp-gateway)
- **IBM ContextForge** — федерация MCP/REST/gRPC, registry, auth, observability, несколько транспортов и composable catalog. Версия 1.0.x делает его серьёзным кандидатом, но продукт и сам MCP gateway landscape всё ещё молоды. [ContextForge overview](https://ibm.github.io/mcp-context-forge/latest/overview/)
- MCPProxy и аналогичные локальные агрегаторы практически удобны, но слабее стандартизованы в lifecycle/auth/isolation и сильнее зависят от конкретного проекта.

Оценка зрелости:

- Streamable HTTP как транспорт: **established**.
- Shared service для дорогого backend state: **established distributed-systems pattern**.
- Docker/Microsoft/IBM gateways как реализации: **emerging**, хотя отдельные компоненты production-grade.
- Единый общепринятый «nginx для MCP», победивший рынок: **нет**.
- Полностью переносимый стандарт lazy-start backend: **нет**; это функция gateway, не протокола MCP.

## 4. Разумный предел сессий

### Что говорит вендор

Публичного лимита «N Claude Code sessions на X GB RAM» нет.

Anthropic документирует другое:

- каждый teammate/worker является отдельной Claude Code session;
- параллелизм умножает token usage и расход rate limits;
- для управляемого параллелизма предлагаются subagents, agent view, agent teams, worktrees, `/batch` и Claude Code on the web. [Parallel agents](https://code.claude.com/docs/en/agents)
- В Agent View supervisor останавливает завершившийся и не подключённый процесс примерно после часа idle; attach восстанавливает его. Это фактически официальный механизм флота, который отделяет сохранённую session от постоянно живого процесса. [Agent View](https://code.claude.com/docs/en/agent-view)
- Agent teams экспериментальны и сами состоят из отдельных экземпляров; это не RAM-оптимизация.

### Практическая оценка для вашей машины

При baseline около 340 МБ:

- 15 живых процессов — уже примерно 5.1 ГБ gross RSS;
- 24 — 8.2 ГБ;
- 36 — 12.2 ГБ,

до MCP, Desktop, IDE, browsers, filesystem cache и рабочего набора macOS. На 32 ГБ постоянно держать 24–36 резидентных интерактивных процессов неразумно независимо от формального отсутствия лимита. Своп 13.1 ГБ и load 61 — уже фактическое превышение устойчивой ёмкости.

Практический потолок следует задавать не числом сессий, а SLO:

- swap-in/swap-out почти нулевые в steady state;
- memory pressure зелёный или устойчиво низкий жёлтый;
- runnable queue не растёт;
- interactive latency не деградирует;
- inactive sessions представлены transcript/session state, а не процессами.

Для вашей конфигурации я бы начал с soft cap **8–12 одновременно резидентных сессий**, остальные — supervisor-stopped или cloud/background. Это инженерная оценка, не рекомендация Anthropic.

**Уверенность:**

- официального числового потолка нет: **established**;
- Agent View как официальный supervisor/offload path: **established**;
- 8–12 как потолок именно вашей машины: **emerging/empirical**;
- универсальный потолок для всех 32-ГБ Mac: **speculative**.

## 5. Чем ломается общий MCP-демон

Основные классы проблем:

1. **Смешение identity и authorization.**  
   Один long-lived SDK client может неявно обслуживать всех callers от имени одного пользователя. `Mcp-Session-Id` нельзя считать authentication token; авторизацию требуется проверять на каждом запросе. MCP отдельно запрещает blind token passthrough и требует audience validation. [MCP security practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)

2. **Смешение session state.**  
   Текущий диалог, cwd, selected account/chat, transaction, pagination cursor, subscriptions и pending requests нельзя хранить в process-global singleton. Нужны immutable shared caches плюс state, индексированный как минимум по authenticated principal и MCP session.

3. **Credential collision.**  
   Refresh одного OAuth token может затереть другой; logout одной сессии — разлогинить всех. SQLite credential/session stores требуют single-writer discipline либо отдельного authoritative owner.

4. **Конкурентные записи и идемпотентность.**  
   Два агента могут одновременно отправить сообщение, сделать commit, обновить запись или повторить call после timeout. Нужны locks на узкие resources, idempotency keys, deduplication и audit trail.

5. **Backpressure и noisy neighbor.**  
   Одна тяжёлая выборка или stuck subscription забивает общий event loop, pool или Telegram flood limit. Нужны per-client quotas, bounded queues, deadlines и cancellation propagation.

6. **Notifications и `list_changed`.**  
   Нельзя отправлять notification не той логической сессии. При нескольких replicas нужны session affinity или корректный distributed session store.

7. **Общий blast radius.**  
   Leak/crash/expired credential теперь ломает все Claude sessions. Нужны supervisor restart, health/readiness checks, circuit breakers и деградация отдельных backends.

8. **Локальная HTTP-поверхность.**  
   `127.0.0.1` лучше, чем `0.0.0.0`, но остаются DNS rebinding, CSRF-подобные вызовы из браузера и случайный доступ других локальных пользователей. Проверяйте `Origin`, используйте local auth token или Unix socket перед HTTP-прокси, не логируйте secrets.

Ваш Telegram-кейс — хороший пример правильной декомпозиции:

- ровно один owner Telethon session/sqlite;
- MCP handlers не создают дополнительные Telegram client sessions;
- shared read cache допустим;
- mutable per-Claude-session state не должен жить в глобальных переменных;
- отправка/изменение сообщений должна иметь сериализацию и audit/idempotency semantics.

**Уверенность:** security/session требования — **established**; конкретные гонки SDK и sqlite — **established practice**; необходимая гранулярность locking для вашего сервера — **зависит от реализации**.

## Топ-3 хода для этой машины

| Ход | Ожидаемая цена | Риск |
|---|---:|---|
| Перевести парк на Agent View/supervisor и останавливать завершённые unattended sessions через ~1 час; оставить 8–12 резидентных | Низкая–средняя: изменение привычки запуска и attach | Низкий. Возможна задержка cold resume; фоновые sessions не переживают sleep |
| Провести controlled RSS/`phys_footprint` A/B: `--safe-mode`, `--bare`, normal, builtin MCP off; затем закрепить per-project `/mcp`, `disableClaudeAiConnectors`, `--strict-mcp-config` для lean-профиля | Низкая: несколько часов измерений | Низкий. Главный риск — ожидать большой выигрыш там, где builtin уже default-off или код остаётся mapped |
| Переносить только тяжёлые shared-state MCP на Streamable HTTP; для stdio-only кандидатов пилотировать Docker MCP Gateway с lazy container start и limits | Средняя: контейнеризация, auth, health checks, миграция SSE → Streamable HTTP | Средний: Docker baseline RAM, latency cold start, общий blast radius и сложность identity/session isolation |

Приоритет я бы выставил так: **сначала сократить число процессов-сессий**, затем доказать реальную цену builtin-пресетов измерением, и только потом вводить gateway. Даже полное устранение всех внешних MCP-процессов не решит 8–12 ГБ базовых session binaries; supervisor решает именно доминирующую часть.
