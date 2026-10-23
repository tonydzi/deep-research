# DR26-09-04-HUB-02-2326 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# A. Фактология bb

Срез источников на 05.09.2026.

Обозначения:

- ✅ подтверждено первичным источником;
- 🤔 вывод из опубликованной архитектуры, но не прямое утверждение авторов;
- ❌ публичного подтверждения не найдено.

## A0. Сначала разделим два разных bb

| Проект | Что это | Код и доступ | Связь между проектами |
|---|---|---|---|
| **bb #1, Browserbase** | Внутренний Slack-агент Browserbase для саппорта, расследований, тикетов и PR | Закрытый внутренний инструмент. Опубликовано подробное архитектурное описание, но не harness-код | ❌ В публичных материалах связи с get-bb не найдено |
| **bb #2, get-bb/bb** | Local-first агентный IDE/оркестратор поверх Claude Code, Codex, Cursor и ACP-провайдеров | Открытый TypeScript-монорепозиторий, лицензия MIT, около 3,3 тыс. GitHub-звёзд на дату исследования | ❌ Browserbase в просмотренных материалах проекта не фигурирует |

Источники: [Browserbase: How we built our internal agent](https://www.browserbase.com/blog/internal-agents), [get-bb/bb](https://github.com/get-bb/bb), [лицензия get-bb/bb](https://github.com/get-bb/bb/blob/main/LICENSE), [история проекта Sawyer Hood](https://www.sawyerhood.com/blog/an-agentic-ide-that-builds-itself).

## A1. bb #1: что это, доступ, цена и лицензия

✅ Browserbase описывает `@bb` как внутреннего агента в Slack. Его вызывают вручную в треде или запускают событиями, например закрытым саппорт-тикетом или webhook. Агент расследует проблему, ищет контекст, пишет ответ, обновляет внутренние системы и может подготовить PR. Авторы публикации от 16 апреля 2026 года: Kyle Jeong и Derek Meegan. [Источник Browserbase](https://www.browserbase.com/blog/internal-agents).

✅ Это не `getbb.app`, не публичный SaaS и не открытый репозиторий. В просмотренной публичной организации Browserbase есть Stagehand и другие проекты, но исходников описанного `@bb` harness нет. [GitHub Browserbase](https://github.com/Browserbase/).

❌ Публичный доступ, тариф и лицензия для bb #1 не заявлены. Следовательно, его нельзя «установить себе»: можно только перенести архитектурные паттерны.

✅ Страница подкаста с Derek Meegan повторяет основные заявления Browserbase: полное покрытие feature requests, сокращение времени первого ответа и JIT-доступ через proxy. [Insecure Agents, episode 40](https://www.insecureagents.com/episodes/derek-meegan), [Spotify](https://open.spotify.com/episode/7Ab7GaiPRXcYblR7XWh35T).

❌ Публичного текстового транскрипта я на открытых страницах эпизода и Spotify не нашёл. Поэтому технические детали ниже опираются прежде всего на инженерный пост Browserbase, а не на пересказ подкаста.

## A2. Реальные метрики bb #1

✅ Browserbase заявляет:

- 100% coverage feature-request pipeline при нулевых человеческих усилиях;
- сканирование каждого закрытого саппорт-тикета и meeting transcript;
- «99%» первых ответов теперь попадают в окно менее 24 часов;
- расследование, занимавшее 30–60 минут ручного поиска по логам, сворачивается до одного сообщения в Slack;
- многие инженеры перешли от ручного написания основной части PR к ревью работы агента.

Все эти цифры находятся в собственной публикации Browserbase. [Источник](https://www.browserbase.com/blog/internal-agents).

❌ Не опубликованы:

- период замера;
- число feature requests и тикетов;
- исходное распределение времени ответа;
- доля PR, дошедших до merge;
- количество откатов и дефектов;
- контрольная группа;
- стоимость одной задачи;
- независимый аудит.

Вердикт: **100% coverage подтверждено только как внутреннее утверждение компании. Это метрика охвата входящего потока, а не доказанная точность решения задач.**

## A3. На чём построен bb #1

✅ Базовый runtime: **OpenCode**. Внутри эфемерной изолированной Linux-VM агент получает файловую систему, процессы и сеть. Предварительно прогретый snapshot обновляется каждые 30 минут; в нём уже лежат репозитории, собранный монорепозиторий, OpenCode и внутренние инструменты. VM выключается после 30 минут простоя, а состояние сохраняется только через snapshot или внешнее хранилище. [Источник](https://www.browserbase.com/blog/internal-agents).

✅ Агенту выставлены шесть базовых инструментов: `read`, `write`, `edit`, `exec`, `safebash`, `skill`. Внешние сервисы не выдаются ему в виде десятков постоянно загруженных tool schemas: нужные skill-файлы и TypeScript-обёртки подгружаются по требованию. [Источник](https://www.browserbase.com/blog/internal-agents).

❌ Конкретная LLM в публикации не названа.

✅ Stagehand присутствует как специализированный skill для написания Browserbase-автоматизаций. Это не доказательство, что Stagehand является ядром агента. [Источник bb](https://www.browserbase.com/blog/internal-agents), [репозиторий Stagehand](https://github.com/browserbase/stagehand).

✅ UI агента сравнивается с Director, но Director является отдельным продуктом Browserbase: визуальным агентом, который выполняет браузерные задачи и может экспортировать Stagehand-код. [Director](https://www.browserbase.com/director), [анонс Director](https://www.browserbase.com/blog/introducing-director).

Таким образом:

- core loop: OpenCode;
- Browserbase Sessions: инфраструктура браузерных запусков;
- Stagehand: один из прикладных skills;
- Director: похожий интерфейс и соседний продукт;
- собственный harness bb: закрыт.

## A4. Как устроен integration proxy и JIT credentials

✅ Опубликованный механизм конкретнее маркетинговой формулировки:

1. Sandbox получает `BB_PROXY_URL`, короткоживущий `BB_SESSION_TOKEN` и bypass-токен.
2. Агент отправляет proxy-запрос примерно в форме `service + method + args`.
3. Proxy проверяет session token в KV.
4. Из KV извлекается permission scope данного запуска.
5. Proxy сопоставляет `service.method` с разрешёнными glob-паттернами.
6. Только после разрешения серверная обёртка вызывает реальный сервис с настоящими credentials, хранящимися вне sandbox.
7. Для небольшого числа обычных сетевых API firewall/proxy перехватывает исходящий запрос и подставляет ключ на egress; внутри среды виден лишь placeholder.

[Источник Browserbase](https://www.browserbase.com/blog/internal-agents).

❌ Не описаны OAuth token exchange, Vault dynamic secrets, облачный STS или workload identity.

🤔 Наиболее точное название механизма: **короткоживущий session capability token плюс серверный integration proxy**. Настоящий API-ключ за proxy может оставаться долгоживущим; публикация доказывает отсутствие сырого ключа в sandbox, но не доказывает динамическую ротацию всех upstream-credentials.

## A5. Что значит «permissions computed fresh»

✅ Browserbase показывает `PermissionConfig`, объединяющий:

- glob-разрешения вида `service.method`;
- включение или выключение базовых OpenCode tools;
- тип события и режим запуска;
- дополнительное ограничение underlying service identity, например read-only Snowflake user.

Интерактивный Slack-режим получает более широкие возможности, а webhook-задача стартует с узким scope, рассчитанным для конкретного события. Proxy затем проверяет scope при каждом обращении к integration method. [Источник](https://www.browserbase.com/blog/internal-agents).

❌ OPA, Cedar или другой общий policy engine не назван.

Поэтому корректная техническая формулировка:

- scope вычисляется заново **при создании invocation**;
- каждый proxy-вызов снова проверяется против этого scope;
- нет доказательства, что сложная ABAC-политика полностью пересчитывается с нуля на каждом tool call.

## A6. bb #2: что это и что даёт поверх голого Claude Code

✅ `get-bb/bb` позиционируется как agentic IDE с desktop/web/CLI/API-интерфейсами. Он хранит треды, позволяет смотреть выполнение вживую, вмешиваться, передавать задачу между провайдерами, управлять worktree и подключать плагины. Провайдеры включают Claude Code, Codex, Cursor и ACP-совместимых агентов. [Репозиторий](https://github.com/get-bb/bb), [system overview](https://github.com/get-bb/bb/blob/main/docs/system-overview.md).

✅ Это MIT-проект. Пользователь приносит уже оплаченные подписки и авторизованные provider CLI. [Лицензия](https://github.com/get-bb/bb/blob/main/LICENSE), [сайт](https://getbb.app/).

✅ Архитектура:

- центральный bb server и SQLite `bb.db`;
- HTTP/WebSocket API;
- host daemon на каждой исполнительной машине;
- проекты, треды, append-only events, worktrees;
- плагины с доступом к процессу bb server.

[System overview](https://github.com/get-bb/bb/blob/main/docs/system-overview.md), [Plugin SDK](https://github.com/get-bb/bb/blob/main/packages/plugin-sdk/README.md).

✅ Поверх голого Claude Code он добавляет:

- единый UI для разных агентов;
- долговечные треды и provider handoff;
- удалённое управление;
- собственные tasks/workflows/automations;
- cron и одноразовые запуски;
- плагины;
- параллельные worktree;
- scheduled messages и контроль concurrency.

[Tasks plugin](https://github.com/get-bb/bb/blob/main/plugins/tasks/PLUGIN_OVERVIEW.md), [workflows](https://github.com/get-bb/bb/blob/main/plugins/workflows/PLUGIN_OVERVIEW.md), [automations](https://github.com/get-bb/bb/blob/main/plugins/automations/PLUGIN_OVERVIEW.md), [changelog](https://getbb.app/changelog).

### Цена bb #2

✅ Код MIT и отдельной лицензии покупать не нужно. Расходы на модели идут через ваши Claude/Codex/Cursor-подписки или API. [Репозиторий](https://github.com/get-bb/bb), [лицензия](https://github.com/get-bb/bb/blob/main/LICENSE).

❌ Отдельной публичной цены за bb Connect или полностью управляемую облачную эксплуатацию в просмотренных материалах я не нашёл.

### Windows

✅ Нативные PowerShell/CMD не поддерживаются. Windows работает только через WSL2. Авторы рекомендуют держать проект в Linux-файловой системе WSL, а не в `/mnt/c`, потому что доступ к Windows-файлам медленнее и хуже для file watching. [Platform support](https://github.com/get-bb/bb/blob/main/docs/platform-support.md), [Microsoft WSL filesystem guidance](https://learn.microsoft.com/en-us/windows/wsl/filesystems).

🤔 На вашем Windows-хабе это создаёт второй пользовательский мир:

- отдельный Linux home;
- отдельный «внутренний путь лаборатории»;
- отдельная авторизация provider CLI;
- Linux-пути вместо `C:\...` и `E:\...`;
- PowerShell-хуки не становятся Linux-хуками автоматически.

### Конфликт с вашими hooks и skills

✅ Claude provider bb запускает настоящий Claude Code CLI и читает его `CLAUDE.md` и skills. Поэтому сам по себе bb не подменяет Claude runtime. [Claude Code provider](https://github.com/get-bb/bb/blob/main/plugins/provider-claude-code/PLUGIN_OVERVIEW.md).

🤔 Нативные Claude hooks должны продолжить работать, **если они установлены внутри того же WSL-home и вызывают доступные Linux-команды**. Windows-копия `%USERPROFILE%\.claude` и PowerShell-only скрипты автоматически не переедут.

✅ Плагины bb исполняются с полным доверием внутри server process и могут читать данные других плагинов, включая их secrets. Это не security boundary. [Plugin SDK](https://github.com/get-bb/bb/blob/main/packages/plugin-sdk/README.md).

✅ Плагин secrets защищает секрет от попадания в разговор, но сохраняет полученное значение в dotenv-файл. Он **не решает plaintext at rest**. [Secrets skill](https://github.com/get-bb/bb/blob/main/plugins/secrets/skills/локальный секрет-стор).

✅ Memory plugin создаёт собственную долговечную FTS5-память и прямо предупреждает, что параллельная provider-native memory может дать дубли и противоречия. Для вашего существующего RAG/TurnState этот плагин лучше отключить. [Memory plugin](https://github.com/get-bb/bb/blob/main/plugins/memory/README.md).

### Syncthing

✅ SQLite не следует использовать как живую базу, одновременно синхронизируемую или открываемую через сетевую файловую систему. SQLite рекомендует client/server-подход, если к данным обращаются с нескольких машин. [SQLite: appropriate uses](https://www.sqlite.org/whentouse.html), [network filesystem caveats](https://www.sqlite.org/useovernet.html).

✅ Syncthing при одновременных несовместимых изменениях создаёт `sync-conflict`-файлы, но это не превращает SQLite в распределённую базу. [Syncthing syncing/conflicts](https://github.com/syncthing/docs/blob/main/users/syncing.rst).

Следствие для вас:

- `bb.db`, plugin databases, host identity и runtime logs **не синкать Syncthing-ом**;
- поднять один центральный bb server на Linux-анкерном узле или отдельном локальном Linux-диске;
- остальные машины подключать host daemon;
- через Syncthing передавать исходники и декларативные конфиги, но не живое состояние bb;
- базу бэкапить SQLite backup API или остановленной копией.

✅ API bb по умолчанию рассчитан на `127.0.0.1`. Документация предупреждает, что публичный API не аутентифицирован и позволяет выполнение команд и чтение файлов; выставлять его на `0.0.0.0` в недоверенную сеть нельзя. [Configuration](https://github.com/get-bb/bb/blob/main/docs/configuration.md).

## A7. Вердикт по фактам

- **bb #1:** архитектурно интересен, но установить его нельзя.
- **bb #2:** реальный MIT-инструмент, но это IDE/оркестратор, а не реализация security-архитектуры Browserbase.
- **bb #2 не лечит автоматически** plaintext-секреты, недоказанные deploy, тихо-зелёные сторожа и межмашинный backlog.
- Для вашего флота он имеет смысл только как ограниченный UI/dispatcher-canary на Linux, а не как новая центральная система истины.

---

# B. Насколько архитектура bb #1 оригинальна

## B1. Сравнение с существующими системами

| Система | Изоляция | Credentials | Permissions | Что совпадает с bb #1 |
|---|---|---|---|---|
| Claude Code | macOS Seatbelt; Linux/WSL2 bubblewrap; hooks и rules | Credential masking proxy умеет показывать sandbox sentinel и подставлять секрет только для разрешённых HTTPS hosts | `allow/deny/ask`, `dontAsk`, `PreToolUse` | Почти все строительные блоки уже доступны локально |
| OpenAI Codex | Нативные sandbox и approval policies на Windows, macOS и Linux/WSL | Секреты отдельно от рабочего контекста; env policy | Sandbox mode плюс approval policy | Для вашего Windows-хаба практичнее WSL-only Claude sandbox |
| Cursor background agents | Отдельная VM на агента, Firecracker | Secrets выдаются в удалённую среду; есть redaction/network controls | Ограничения среды и сети | Очень похожая модель эфемерного worker |
| Devin | Отдельная managed development machine/session | Environment blueprint и secret provisioning | Session-level environment policy | Тот же класс managed remote coding sandbox |
| E2B | Изолированные Linux-VM и templates | Секреты передаются runtime-механизмом пользователя | Определяется вызывающим harness | Готовая замена самостоятельной VM-инфраструктуре |
| Modal Sandboxes | Эфемерные sandboxes с lifecycle и timeout | Secret objects | App-defined | Аналог background invocation |
| Daytona | Быстрые container/VM sandboxes, snapshots | Egress secret proxy с placeholder и host allowlist | Policy вокруг sandbox и proxy | Самый близкий публичный аналог credential broker bb |
| gVisor | Userspace application kernel для OCI | Не решает credentials | Изоляция syscall boundary | Возможная нижняя изоляция |
| Firecracker | KVM microVM на Linux | Не решает credentials | VM boundary | Возможная нижняя изоляция |

Источники: [Claude Code sandboxing](https://code.claude.com/docs/en/sandboxing), [Claude permissions](https://code.claude.com/docs/en/permissions), [Claude hooks](https://code.claude.com/docs/en/hooks), [Codex sandboxing](https://learn.chatgpt.com/docs/sandboxing), [Cursor cloud agent security](https://prod.cursor.com/docs/cloud-agent/security), [Devin deployment](https://docs.devin.ai/enterprise/deployment/overview), [E2B docs](https://docs.e2b.dev/), [Modal Sandboxes](https://modal.com/docs/guide/sandboxes), [Daytona Sandboxes](https://www.daytona.io/docs/en/sandboxes/), [Daytona Secrets](https://www.daytona.io/docs/en/secrets/), [gVisor](https://gvisor.dev/docs/), [Firecracker](https://firecracker-microvm.github.io/).

Важная поправка к исходной постановке: ✅ **у Codex уже есть нативный Windows sandbox**. «На Windows sandbox невозможен» справедливо для нативного Claude Code, но не для Codex. [Codex sandboxing](https://learn.chatgpt.com/docs/sandboxing), [Claude sandboxing](https://code.claude.com/docs/en/sandboxing).

## B2. MCP и permissioning

✅ MCP authorization standard описывает OAuth-механизм аутентификации удалённого MCP server. Он не задаёт универсальную per-method ABAC-модель для каждого бизнес-вызова. Авторизация конкретного tool остаётся ответственностью MCP server и host. [MCP authorization](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization).

✅ Claude `PreToolUse` запускается перед tool call, видит tool name и input и может `allow`, `deny` или `ask`. Он работает и с MCP tools. Но файловые ссылки через `@...` могут обходить `PreToolUse` для обычного `Read`, поэтому hook нельзя считать единственной security boundary. [Claude hooks](https://code.claude.com/docs/en/hooks).

Вывод: Browserbase не изобрёл tool permissions, но правильно перенёс окончательную проверку из промпта в proxy, которому агент не может приказать «игнорировать правила».

## B3. Code Mode: происхождение и измеренные выигрыши

❌ Называть Browserbase автором идеи неправильно. Исполнение кода как action language существовало до их публикации.

✅ В феврале 2024 года CodeAct предложил агентам генерировать исполняемый Python вместо JSON/text actions. Авторы сообщили улучшение success rate до 20% на своих API/action benchmarks. Это исследовательский benchmark, не доказательство такого же выигрыша на production-интеграциях. [CodeAct paper](https://arxiv.org/abs/2402.01030).

✅ Cloudflare в сентябре 2025 года публично назвала похожий подход Code Mode. В последующей реализации для Cloudflare API два инструмента `search` и `execute` заменяли огромный набор schemas; авторы оценили schema footprint примерно в 1,17 млн токенов против около 1 тыс., то есть сокращение на 99,9%. Это экономия описаний API, а не success-rate задач. [Code Mode](https://blog.cloudflare.com/code-mode/), [Code Mode MCP](https://blog.cloudflare.com/code-mode-mcp/).

✅ Anthropic programmatic tool calling показал:

- в среднем около 11% улучшения на BrowseComp и DeepSearchQA;
- 24% меньше input tokens;
- на внутреннем 75-tool benchmark около 38% меньше billed input tokens при примерно неизменной точности;
- для простых цепочек из одного-двух вызовов дополнительный code layer мог увеличить стоимость без улучшения качества.

[Anthropic programmatic tool calling](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling).

✅ Browserbase оценивает экономию контекста на своём примере примерно в 88%, когда сорок постоянно загруженных tool schemas заменяются typed code interface. Это rough estimate, а не воспроизводимый benchmark. [Browserbase Code Mode](https://www.browserbase.com/blog/code-mode-is-all-you-need).

❌ Публичных чисел Browserbase по снижению latency, error rate, secret leakage или проценту успешно завершённых code-mode задач нет.

## B4. Что в bb #1 действительно новое

Не новое:

- Linux sandbox;
- snapshot/prewarm;
- lazy-loaded skills;
- code-as-action;
- egress allowlist;
- секреты за proxy;
- per-session capability token;
- read-only service identities;
- webhook-driven workers;
- approval-free low-risk execution.

Практически ценное сочетание:

- один общий OpenCode loop;
- очень маленький стабильный tool surface;
- typed service packages вместо россыпи MCP schemas;
- event-specific scope;
- proxy-enforced `service.method`;
- реальные credentials никогда не попадают в agent runtime;
- фоновые задачи завершаются и выключают sandbox.

🤔 Оригинальность bb #1 не в новом примитиве, а в **операционно простой сборке известных примитивов в один внутренний конвейер**.

---

# C. Применимость к вашему флоту

## C1. Секреты: целевая архитектура

Сейчас секрет, синкаемый plaintext на шесть машин, одновременно:

- копируется на каждый узел;
- читается любым процессом того же пользователя;
- попадает под риск prompt injection;
- тяжело отзывается;
- не имеет per-event scope.

Целевая модель:

```text
event runner
   |
   | короткоживущий run capability
   v
локальный broker на конкретном узле
   |
   +-- service.method policy
   +-- host/domain allowlist
   +-- audit run_id
   |
   +--> выполняет API-вызов сам
   или
   +--> подставляет секрет только в разрешённый HTTPS egress
```

Для браузерного логина пароль иногда всё же должен попасть в browser automation channel. Его следует передавать не в LLM-контекст, а непосредственно в autofill/browser adapter для конкретного домена и конкретного `run_id`.

### Сравнение вариантов

| Вариант | Цена | Офлайн | Syncthing | Сложность | Ремонт непрофессионалом | Вердикт |
|---|---:|---|---|---|---|---|
| DPAPI / macOS Keychain / Linux Secret Service | Встроено в ОС | Да | Сам секрет не переносим между машинами | Низкая | Хорошая, если дать одну команду восстановления | Нужен на каждом узле для master/bootstrap key |
| SOPS + age | Бесплатно, MPL-2.0 | Да | Шифротекст синкать можно | Низкая-средняя | Хорошая | Лучший первый шаг |
| Vault dev-mode | Бесплатно | Нет надёжного persistence | Не применимо | Низкая при запуске, плохая в эксплуатации | Плохая | ❌ Не использовать |
| Production Vault | Бесплатная Community-редакция, но дорог в сопровождении | Возможен локально | Нельзя синкать живое состояние | Высокая | Плохая | Оверинжиниринг для шести личных узлов |
| Infisical Cloud | Free до 5 identities; Pro $20 за identity/месяц при годовой оплате; Advanced $40 | Ограниченно, нужен cache/agent | Секреты не синкаются | Средняя | Средняя | Хорошо, но 7 identities уже выходят за free |
| Doppler | Developer $0 до 3 users и 50 service tokens; Team $21/user/месяц | Да через encrypted fallback cache | Не нужно | Средняя | Хорошая | Практичный облачный вариант |
| 1Password Service Account / Connect | Требует подходящего 1Password-плана; точная automation-цена публично не подтверждена | Connect может держать локальный cache | Не нужно | Средняя | Очень хорошая UI-ремонтопригодность | Лучший выбор, если уже куплен подходящий план |
| Локальный собственный broker | Разработка и сопровождение | Да | Синкать только policy/config, не state | Средняя | Хорошая при одной команде диагностики | Нужен даже поверх облачного vault |

Источники: [Windows DPAPI](https://learn.microsoft.com/en-us/windows/win32/api/dpapi/nf-dpapi-cryptprotectdata), [macOS Keychain](https://support.apple.com/guide/keychain-access/what-is-keychain-access-kyca1083/mac), [Linux Secret Service](https://specifications.freedesktop.org/secret-service-spec/latest/), [SOPS](https://github.com/getsops/sops), [Vault dev mode](https://developer.hashicorp.com/vault/docs/concepts/dev-server), [Infisical pricing](https://infisical.com/pricing), [Infisical machine identities](https://infisical.com/docs/documentation/platform/identities/machine-identities), [Doppler pricing](https://www.doppler.com/pricing), [Doppler service tokens](https://docs.doppler.com/docs/service-tokens), [Doppler offline fallback](https://docs.doppler.com/docs/automatic-fallbacks), [1Password Service Accounts](https://developer.1password.com/docs/service-accounts/get-started/), [1Password Connect](https://developer.1password.com/docs/connect/get-started/).

### Конкретная рекомендация

**Первый слой:**

1. Перевести синкаемый plaintext-файл в `SOPS + age`.
2. На каждом узле хранить age private key через DPAPI, Keychain или Secret Service.
3. Сохранить тот же пользовательский интерфейс: `secret get X` или `run-with-secrets`.
4. Старые plaintext-файлы не удалять до отрицательных тестов и подтверждения на всех шести узлах; затем удалить отдельным контролируемым этапом.

**Второй слой:**

1. Локальный broker принимает `run_id`, `event_type`, `service`, `method`, `target`.
2. Выдаёт короткоживущую capability или выполняет запрос сам.
3. Запрещает возврат raw secret по умолчанию.
4. Пишет audit: узел, run, service.method, target, outcome, но не значение секрета.
5. Для GitHub, Telegram, почты и публикации ограничивает не только сервис, но и конкретный repo/chat/recipient.

**Третий слой:**

Claude Code уже умеет credential masking: sandbox видит sentinel, а встроенный TLS proxy подставляет настоящий env-secret только для разрешённых `injectHosts`. Это почти готовый локальный аналог Browserbase egress injection и должно быть использовано на WSL/Linux-узлах. [Claude sandbox credential masking](https://code.claude.com/docs/en/sandboxing).

### Важное ограничение

❌ Ни SOPS, ни Infisical, ни Doppler сами по себе не дают Browserbase-модель. Если агент может вызвать `secret get` без ограничений, вы лишь зашифровали диск, но не ввели least privilege.

Также без IdP или аппаратной attestation остаётся **secret zero**:

- age private key;
- Infisical client secret;
- Doppler service token;
- 1Password service-account token.

Его нужно привязать к конкретной машине через OS store и выдать ему минимальный scope.

## C2. Sandbox на Windows 11

### Реальные варианты

| Механизм | Изоляция | Запуск | Ограничения | Пригодность |
|---|---|---|---|---|
| Codex native Windows sandbox | Нативный Windows | Низкая операционная цена | Только для Codex; нужно проверять доступ к вашим путям | ✅ Использовать для Codex-задач |
| Claude Code в WSL2 | bubblewrap; опциональный seccomp | Подходит для регулярных worker | Отдельный Linux-home, пути, auth; Windows interoperability может стать escape | ✅ Для рискованных Claude-задач |
| Windows Sandbox | Полная одноразовая Hyper-V VM | Несколько секунд; закрытие стирает состояние | Нет удобного always-on режима, ограниченная автоматизация, один экземпляр | Только ручные untrusted-тесты |
| Docker Desktop + WSL2 | Container boundary | Стабилен после прогрева daemon | Не полноценная VM boundary; volume permissions критичны | Для воспроизводимых build/test |
| Firejail | Linux namespaces/seccomp | Лёгкий | Только Linux | Для Linux-анкерного узла |
| Firecracker/gVisor | Более сильная Linux isolation | Требует Linux/KVM или специального runtime | Сложная эксплуатация на личном Windows-хабе | ❌ Не строить самим |

Источники: [Codex sandboxing](https://learn.chatgpt.com/docs/sandboxing), [Claude sandboxing](https://code.claude.com/docs/en/sandboxing), [Windows Sandbox](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/), [Docker Desktop WSL best practices](https://docs.docker.com/desktop/features/wsl/best-practices/), [Firejail](https://github.com/netblue30/firejail), [Firecracker](https://firecracker-microvm.github.io/), [gVisor](https://gvisor.dev/docs/).

✅ Claude предупреждает, что WSL2-процесс может запускать Windows executables через interoperability channel. Опциональный seccomp-фильтр нужен, если WSL sandbox должен быть настоящей границей. Также стоит включить `failIfUnavailable: true` и запретить unsandboxed fallback. [Claude sandboxing](https://code.claude.com/docs/en/sandboxing).

### Цена в задержке

✅ Windows Sandbox заявляет запуск «за несколько секунд». Firecracker публикует целевой boot менее 125 мс и memory overhead менее 5 MiB на microVM, но это Linux/KVM-показатель и его нельзя переносить на ваш Windows pipeline. Daytona заявляет менее 90 мс для своих container sandboxes, тоже в собственной управляемой среде. [Windows Sandbox](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/), [Firecracker](https://firecracker-microvm.github.io/), [Daytona](https://www.daytona.io/docs/en/sandboxes/).

❌ Сопоставимого публичного cold-start числа для Browserbase bb нет.

Для вас важнее измерить:

- p50/p95 старта worker;
- p95 окончания;
- долю sandbox bootstrap failures;
- cross-filesystem I/O;
- процент задач, ушедших в unsandboxed fallback.

### Стоит ли изолировать личную машину

Да, но выборочно. Угроза здесь не другой tenant, а:

- prompt injection из issue, сайта или репозитория;
- ошибочный `rm`, recursive write или upload;
- компрометированная dependency;
- чтение всех секретов процессом одного пользователя.

Sandbox бесполезен, если вы одновременно:

- монтируете весь «внутренний путь лаборатории» на запись;
- выдаёте generic shell;
- передаёте все secrets;
- разрешаете запуск `powershell.exe` из WSL.

Вердикт: **не переносить 100+ рутин в WSL целиком. Выделить WSL/Linux worker для untrusted code и browser jobs, а нативные безопасные Windows-задачи оставить в Task Scheduler.**

## C3. Минимальный least-privilege поверх Claude Code и MCP

OPA/Cedar вам сейчас не нужен. Достаточен детерминированный policy function.

### Event envelope

```text
run_id
event_type
source
actor
target
risk_tier
expires_at
dedupe_key
```

### Пример scopes

| Event type | Разрешить | Запретить |
|---|---|---|
| `support.read` | Read/Grep, read-only CRM, logs | Write, shell, outbound message |
| `issue.fix` | Один repo/worktree, edit, tests, branch, draft PR | Merge, deploy, другие repos, raw secrets |
| `health.check` | Один именованный диагностический script | Generic shell, filesystem write, MCP |
| `deploy.apply` | Конкретный artifact hash и конкретный node | Любой другой package, внешняя коммуникация |
| `outbound.dm` | Один recipient, один channel, лимит тела | Чтение vault, новые recipients, массовая отправка |
| `tier2.*` | Создать blocked-record и завершиться | Ожидание интерактивного approval |

### Исполнение

1. Фоновый Claude запускать в `dontAsk`.
2. Дать точные allow-rules.
3. Выставить hard-deny для путей, доменов, recipients и destructive commands.
4. Включить строгий sandbox.
5. `PreToolUse` проверяет tool input против envelope.
6. MCP server или integration proxy повторно проверяет `service.method` и target.
7. Любой неизвестный случай завершает run кодом `POLICY_DENIED`, а не открывает окно approval.

Claude документирует `dontAsk` как режим, автоматически отклоняющий всё, что не было предварительно разрешено. Это именно то, что нужно безлюдной сессии. [Claude permissions](https://code.claude.com/docs/en/permissions).

### Не ломать 100+ рутин

Вводить в три этапа:

1. **Shadow:** hook только пишет «разрешил бы/запретил бы».
2. **Canary:** enforcement для 5–10 read-only рутин.
3. **Waves:** по одному классу событий, а не по одному скрипту.

Первая policy-версия должна допускать текущий нормальный трафик и красить:

- неизвестный event type;
- generic shell там, где должен быть named script;
- секрет вне broker;
- выход за target;
- prompt approval в unattended mode.

## C4. Event-driven trajectories против фоновой чат-сессии

Event-driven запуск сам по себе ничего не чинит. Он помогает, если вокруг него есть конечный автомат:

```text
RECEIVED
  -> AUTHORIZED
  -> RUNNING
  -> SUCCEEDED | FAILED | BLOCKED | TIMED_OUT
```

Обязательные свойства:

- idempotency/dedupe key;
- timeout;
- конечный статус;
- машинный receipt;
- сохранённый output hash;
- retry policy;
- отдельное состояние `BLOCKED`, а не висящий процесс.

Для Tier-2 действие должно:

1. записать запрос approval;
2. завершить worker;
3. после approval создать новый run с тем же business ID.

Это прямо лечит ваши 93 scheduler-вызова, застрявших на approval: **не потому, что запуск event-driven, а потому, что unattended runtime физически не умеет ждать интерактивное окно.**

✅ bb #2 уже движется в эту сторону: automations имеют timeout, process-group kill, ограничение одного выполнения на automation, retry через 30 и 60 секунд, pause после третьего подряд failure и settlement осиротевших runs после рестарта. [Changelog 0.39](https://getbb.app/changelog), [automations plugin](https://github.com/get-bb/bb/blob/main/plugins/automations/PLUGIN_OVERVIEW.md).

Но bb #2 не знает ваш deploy receipt и межмашинный ACK. Их всё равно придётся реализовать в вашем event ledger.

## C5. Автоматический PR из issue

### Открытые и готовые реализации

| Вариант | Что умеет | Опубликованный success-rate |
|---|---|---|
| mini-SWE-agent | Берёт issue, работает в sandbox, правит repo | Более 74% SWE-bench Verified в заявленной конфигурации |
| SWE-agent | Issue-to-patch framework; исходный проект теперь рекомендует mini-версию | Результаты зависят от модели и SWE-bench split |
| OpenHands | Open-source agent platform для coding tasks | Есть benchmark-результаты, но не эквивалент production merge-rate |
| Claude Code GitHub Action | `@claude` в issue/PR, анализ, исправление и PR | ❌ Публичного production success-rate нет |
| Codex cloud | Фоновая задача из web/GitHub/GitLab/Linear/Slack, diff и PR | ❌ Публичного production merge-rate нет |

Источники: [mini-SWE-agent](https://github.com/SWE-agent/mini-swe-agent), [SWE-agent](https://github.com/SWE-agent/SWE-agent), [SWE-bench](https://www.swebench.com/), [OpenHands](https://github.com/OpenHands/OpenHands), [Claude Code GitHub Actions](https://code.claude.com/docs/en/github-actions), [Codex cloud](https://learn.chatgpt.com/docs/cloud).

Важно: SWE-bench Verified измеряет решение исторических GitHub issues в специально собранных репозиториях. Это не доля PR, которые maintainer реально принял в вашем production repo. [SWE-bench](https://www.swebench.com/).

### Что можно поставить за вечер

Самый короткий путь:

1. Claude Code GitHub Action или Codex cloud.
2. Один некритичный repo.
3. Только явный `@claude`/label, не все новые issues.
4. Отдельная branch или draft PR.
5. Никакого merge/deploy.
6. Ограничение времени, concurrency и paths.
7. Обязательные тесты.
8. Человек ревьюит diff.

Это даст проверяемый результат быстрее, чем установка bb #2 и написание собственной GitHub-интеграции.

## C6. Что не нужно копировать

### Из bb #1

Не копировать:

- собственный Firecracker/gVisor-кластер;
- prewarm snapshot каждые 30 минут для всех задач;
- отдельный SaaS-grade proxy для каждого read-only локального инструмента;
- OPA/Cedar до появления реально сложной policy;
- универсального Slack-агента с широкими правами;
- Stagehand там, где сервис имеет нормальный API;
- полную многотенантную compliance-изоляцию.

Это оправдано для SaaS, работающего с клиентскими данными и десятками инженеров. Для личного флота из шести машин дешевле native sandbox плюс один Linux worker.

Копировать:

- event-specific scope;
- secrets вне runtime;
- egress injection;
- proxy-side permission check;
- narrow underlying identity;
- fail-closed unattended runs;
- output receipt;
- idempotency и timeout.

### Из bb #2

Не включать на первом этапе:

- bb Memory;
- bb Tasks как новую истину;
- bb Automations поверх уже работающих scheduled tasks;
- bb Workflows;
- secrets plugin как «решение безопасности»;
- Syncthing для `bb.db`;
- публичный bind bb API.

Иначе вы получите вторые память, планировщик, реестр задач и базу, но не закроете текущие долги.

## C7. Стоит ли устанавливать get-bb/bb

**Вердикт: не ставить fleet-wide. Поставить только bounded canary на Linux-анкер или отдельный WSL2 instance.**

Что он потенциально даст:

- единый экран Claude/Codex/Cursor;
- steering живой задачи;
- сохранённые треды;
- provider handoff;
- remote view;
- worktree orchestration;
- удобное наблюдение за несколькими агентами.

Что он не даёт:

- доказательство deploy на каждой машине;
- автоматическое разрешение 187 межмашинных задач;
- устранение 142 непринятых посылок;
- cryptographic secret broker;
- защиту от тихо-зелёного output;
- совместимость Windows hooks без миграции;
- безопасный Syncthing SQLite.

Риск зрелости ненулевой. Например, в 0.33 был зафиксирован Claude Code bridge regression, ломавший каждый turn у затронутых пользователей; позже его исправили. Отдельный тяжёлый пользователь сообщил о `bb.db` размером 1,7 GB и одном синхронном запросе, блокировавшем клиентов на 11,2 секунды. Это единичные issue, не доказательство общего дефекта, но для центрального флотового слоя достаточно, чтобы требовать canary. [Issue 853](https://github.com/get-bb/bb/issues/853), [issue 1131](https://github.com/get-bb/bb/issues/1131).

---

# D. Приземление

## D1. Минимальный сценарий: один вечер

### Что ставим

1. `SOPS + age` для копии секретов.
2. Age key в DPAPI/Keychain/Secret Service.
3. Один compatibility wrapper с прежним интерфейсом получения секрета.
4. Пять representative scheduled routines переводим в noninteractive `dontAsk`.
5. Добавляем timeout, terminal status и output receipt.
6. На одном WSL/Linux-run включаем Claude strict sandbox и credential masking для одного API.
7. Опционально ставим bb #2 только на Linux-анкер/WSL в отдельный data dir:
   - bind только loopback;
   - telemetry off;
   - один тестовый repo;
   - только provider plugin;
   - без memory/tasks/automations/workflows.

### Какую боль закрывает

- Начинает убирать plaintext из Syncthing.
- Устраняет approval-hang для первых пяти рутин.
- Доказывает, может ли bb #2 жить рядом с вашим Claude runtime.
- Вводит первый настоящий output receipt.

### Метрика успеха

- 20 тестовых unattended runs;
- 0 approval windows;
- 20 terminal statuses;
- 20 output receipts;
- 0 чтений секрета вне разрешённого service;
- 5 положительных и 5 отрицательных sandbox tests;
- bb #2: 5 последовательных задач без потери треда, hook или worktree.

### Что может сломаться

- скрипты ожидают plaintext path;
- PowerShell hook не запускается в WSL;
- sandbox блокирует нужный бинарник;
- credential masking не поддерживает конкретный browser-login flow;
- bb provider bridge несовместим с текущей версией CLI.

### Откат

- старый стор оставить read-only и не удалять;
- wrapper переключить обратно на старый backend;
- выключить один canary hook;
- остановить bb server;
- штатные scheduled tasks не трогать.

**Вердикт:** делать. Это самый высокий эффект на единицу риска.

## D2. Средний сценарий: одна неделя

### Что строим

1. Локальный secret broker на каждом типе ОС.
2. Единый `service.method` API.
3. Короткоживущие event capabilities.
4. Audit без secret values.
5. Event state machine с idempotency, timeout и retry.
6. Десять low-risk рутин переводим на новую рельсу.
7. Один центральный bb #2 server на Linux, два enrolled hosts.
8. `bb.db` остаётся только на локальном диске server.
9. Существующие scheduled tasks для перенесённых рутин отключаются, но не удаляются.
10. Один автоматический issue-to-draft-PR flow на некритичном repo.

### Какую боль закрывает

- Класс approval-hangs.
- Секреты для мигрированных сервисов.
- «Запустился» против «действительно выдал результат».
- Двойное выполнение.
- Часть недоказанных deploy.
- Ручной PR из простого issue.

### Метрики

- approval blocks в canary: с 93-класса до 0;
- output registry coverage: не ниже 95% для мигрированных рутин;
- 100% runs имеют terminal status;
- 100% deploy canary имеют machine receipt;
- 0 raw-secret reads агентом для proxy-compatible API;
- 10 intentionally broken runs дают 10 красных результатов;
- ни одной двойной внешней отправки;
- p95 завершения не хуже текущего более чем на согласованный порог.

### Риски

- broker становится общей точкой отказа;
- токен истёк при офлайн-режиме;
- старый и новый scheduler одновременно отправят сообщение;
- WSL hook увидит другой path;
- центральный bb server упадёт и лишит UI всех узлов.

### Откат

- старые tasks сохранены выключенными;
- rollback включает их по одному;
- broker adapter временно читает зашифрованный SOPS backend;
- dual-read допустим, dual-write запрещён;
- внешний side effect защищён idempotency key;
- bb host daemon отключается без изменения исходников.

**Вердикт:** делать после вечернего canary, но bb #2 держать вспомогательным UI, не ledger истины.

## D3. Полный сценарий: один месяц

### Что строим

1. Production secret backend:
   - 1Password, если уже есть подходящий automation plan;
   - Doppler, если приоритет простота и service tokens;
   - Infisical, если оправдана цена identities;
   - proper Vault только при готовности обслуживать сервер.
2. Local broker/cache на каждом узле.
3. Policy manifest для каждого класса из 100+ рутин.
4. Event runner с durable queue.
5. Signed/opaque run capabilities.
6. Output proof и artifact hash для каждого deploy.
7. Linux sandbox worker для untrusted code и browser jobs.
8. Канарейка: Windows, затем узел другого типа ОС, затем остальные.
9. Automated issue-to-PR для выбранных repos.
10. bb #2 остаётся dispatcher/UI только если доказал стабильность за неделю.
11. Его state не синкается, а резервируется корректным SQLite backup.

### Цели на ваших исходных данных

| Метрика | Сейчас | Цель месяца |
|---|---:|---:|
| Невыполненные межмашинные задачи | 187 | менее 10, ни одной старше 24 ч |
| Непринятые deploy-посылки | 142 | 0 |
| «Applied» без машинного доказательства | 284 | 0 |
| Approval blocks из рутинных сессий | 93 | 0 |
| Output registry coverage | 17% | не ниже 95%, затем 100% для KEEP |
| Машины с plaintext synced secrets | 6 | 0 |
| Пойманные искусственные поломки | 5 из 16 | минимум 14 из 16 на первом проходе, затем 16 из 16 |

### Риски

- broker или центральная очередь останавливают весь fleet;
- offline fallback оставляет отозванный secret дольше TTL;
- неверная policy массово запрещает рабочие routines;
- dual-run порождает двойной DM, PR или публикацию;
- Linux worker не видит нужное Windows-only состояние;
- bb server становится ещё одним центром обслуживания.

### Страховка и откат

- canary на одном узле;
- затем узел другого типа ОС;
- policy shadow до enforcement;
- старые tasks выключать, не удалять;
- старый интерфейс секрета сохраняется через adapter;
- очередь имеет локальный spool;
- внешние действия требуют idempotency key;
- fail-closed для денег, удаления и публикации;
- fail-open допустим только для чисто read-only локальной диагностики;
- rollback проверяется отдельным упражнением до fleet rollout.

**Вердикт:** архитектура оправдана только после того, как недельный слой реально уменьшит ваши четыре исходных счётчика. Не строить месячный вариант авансом.**

## D4. Писать ли Derek Meegan и Browserbase

**Да. Это рациональнее, чем пытаться восстановить закрытую реализацию по одному посту.**

Спросить конкретно:

1. Какой реальный TTL и механизм ротации `BB_SESSION_TOKEN`?
2. Как отзывается уже запущенный session scope?
3. Upstream credentials динамические или долгоживущие за proxy?
4. Scope рассчитывается один раз на invocation или может меняться во время run?
5. Какая VM/sandbox-технология используется?
6. Какие escape и prompt-injection инциденты они наблюдали?
7. Каковы denominator и период для 100% coverage?
8. Какой процент PR merged, reverted и вручную переписан?
9. Планируется ли публикация integration-proxy или permission harness?
10. Как они обрабатывают high-consequence writes и человеческие approvals?

Что предложить взамен:

- анонимизированный корпус ваших отказов;
- данные шести узлов и сотен автономных routines;
- кейс 93 из 101 approval blocks;
- кейс 284 ложных «applied»;
- 16 mutation failures, из которых regression grid поймал только 5;
- результаты сравнения native Windows Codex sandbox против Claude WSL2;
- OSS-вклад в Stagehand или публичные security docs.

Stagehand открыт под MIT и принимает issues/PR, поэтому технический вклад можно сделать предметным. [Stagehand repository](https://github.com/browserbase/stagehand).

Отдельно стоит написать maintainers **get-bb**, не смешивая разговоры. Предложение им:

- документация Windows home против WSL home;
- предупреждение не синкать `bb.db` через Syncthing;
- event output receipt plugin;
- unattended fail-closed permission mode;
- тест совместимости с существующими Claude hooks;
- graceful backup/restore для большой SQLite-базы.

---

# Финальная таблица: наша боль → что из bb лечит → дешёвая замена → вердикт

| Ваша боль | Что из bb лечит | Чем заменить дешевле | Вердикт |
|---|---|---|---|
| 187 межмашинных задач, 71 старше недели | bb #2 даёт threads/tasks/remote UI, но не подтверждённую доставку | Durable queue + ACK + дедуп + deadline + один ledger | **bb не брать как решение** |
| 142 непринятые deploy-посылки | Event-driven execution bb #1 | Deploy state machine + artifact hash + machine receipt | **Брать паттерн, не продукт** |
| 284 «applied» без доказательства | В bb #1 sandbox заканчивается конечным job result | Проверка артефакта на целевой машине, не exit code отправителя | **Брать обязательно** |
| 93 approval blocks из unattended runs | Узкий scope на входе, отсутствие интерактивного approval | Claude `dontAsk` + fail-closed `BLOCKED` + новый run после approval | **Брать немедленно** |
| Три сессии остановили spawn на 6:42 | Ephemeral worker + timeout | Process-group kill, terminal state, slot release в `finally` | **Брать немедленно** |
| 17% output-registry coverage | Event/job lifecycle bb #1 | Обязательный receipt и freshness check | **Брать обязательно** |
| Plaintext secrets на шести машинах | Integration proxy и egress injection | SOPS+age сейчас; local broker и Claude credential mask следующим слоем | **Брать архитектуру** |
| Агент должен логиниться сам | Broker не отнимает автономность | Domain-scoped autofill/API proxy без выдачи raw secret LLM | **Брать выборочно** |
| Тесты поймали 5 из 16 поломок | bb напрямую это не лечит | Mutation corpus как обязательная acceptance grid | **bb не нужен** |
| 100+ существующих scheduled tasks | bb #2 Automations может заменить scheduler | Оставить Task Scheduler; ввести общий event envelope/receipt | **Не мигрировать массово** |
| Разные Claude/Codex/Cursor рельсы | bb #2 даёт общий UI и handoff | Текущая шина плюс унифицированный run ledger | **Canary, не fleet-wide** |
| Windows-хаб | bb #2 работает лишь через WSL2 | Codex native sandbox; Claude risk jobs в WSL/Linux | **Не делать bb центральным на Windows** |
| Hooks/skills уже существуют | Claude provider bb сохраняет native runtime | Установить те же hooks в WSL-home; не включать дублирующие bb plugins | **Совместимо только после canary** |
| Syncthing между машинами | bb #2 central server поддерживает remote hosts | Один server, локальный SQLite, sync только source/config | **Живую bb.db не синкать** |
| Автоматический PR из issue | Оба bb демонстрируют такой workflow | Claude GitHub Action или Codex cloud на одном repo | **Не нужен bb ради PR** |
| Personal fleet, не SaaS | Firecracker/prewarm/full proxy были бы избыточны | Native sandbox + один Linux worker + selective broker | **Не копировать SaaS-слой** |

## Итоговый инженерный вердикт

1. **bb #1 установить нельзя, но три его паттерна вам нужны срочно:** unattended fail-closed execution, event-scoped capabilities и secret broker/egress injection.
2. **bb #2 не является открытой версией bb #1.**
3. **Fleet-wide установку get-bb/bb сейчас не делать.**
4. **Один canary get-bb на Linux-анкерном узле оправдан** для проверки UI, steering, provider handoff и remote supervision.
5. **Не отдавать bb #2 память, scheduler, task ledger, secrets и Syncthing-state.**
6. Первый практический шаг: **SOPS+OS key store, `dontAsk`, timeout, terminal state и output receipt для пяти рутин.**
7. Через неделю решение о bb #2 принимать по вашим счётчикам, а не по удобству интерфейса.

🧒 Простыми словами: тут оказались две машины с одинаковым названием. Первая у Browserbase закрыта, но у неё полезная схема замков и ключей. Вторая открыта, но это скорее удобный пульт для агентов. Нам нужен не новый пульт на всех компьютерах, а сначала нормальные замки для секретов, задания без висящих окон и чек, что посылка действительно дошла. Открытый bb можно попробовать на одном Linux-компьютере и только потом решать, оставлять ли его.

Дата окончания среза: 05.09.2026, Europe/London.
