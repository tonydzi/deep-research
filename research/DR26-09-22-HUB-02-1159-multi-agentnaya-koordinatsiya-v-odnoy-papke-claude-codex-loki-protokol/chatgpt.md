# DR26-09-22-HUB-02-1159 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. ЧТО В МИРЕ УЖЕ СДЕЛАНО

Срез данных: 22.09.2026. Звёзды и число коммитов — снимок на эту дату. Я проверял живые страницы документации, репозитории, историю коммитов и issue. Там, где GitHub не отдал точную дату последнего коммита, так и пишу: «не проверено», а не подменяю её датой индексации.

A1. Репозитории с реализацией кодом

1. MCP Agent Mail — Dicklesworthstone/mcp_agent_mail

URL: https://github.com/Dicklesworthstone/mcp_agent_mail

На 22.09.2026: 2,2 тыс. звёзд, 227 форков, около 867 коммитов. Точная дата последнего коммита в этом проходе не извлечена; репозиторий выглядит живым, но точную свежесть помечаю как не проверенную.

Что реализовано кодом:

- MCP-сервер на FastMCP с идентичностями агентов, почтовыми ящиками, тредами, поиском, SQLite и Git-аудитом.
- Advisory reservations для файлов и glob-областей с TTL.
- При конфликте резервирование не обязательно блокируется жёстко: вызывающий агент получает сведения о пересечении и владельце.
- Есть pre-commit guard, который может предупреждать или блокировать коммит при нарушении reservations.
- Есть уведомления о просроченных и освобождённых резервациях.

Механизм одной фразой: «почта между агентами плюс временные advisory-claims на файловые области».

Уровень зрелости: код есть; наличие функций видно в репозитории и документации, но я не запускал сервер в этом исследовании. Семантического claim на вопрос или задачу как обязательного первого уровня не обнаружено.

2. Axis — VirSanghavi/axis

URL: https://github.com/VirSanghavi/axis

На 22.09.2026: 5 звёзд, 1 форк, около 1005 коммитов. Точная дата последнего коммита не проверена; проект индексировался в последние дни, поэтому «вероятно жив», уверенность средняя.

Что реализовано кодом:

- Общий MCP job board для разных coding-agent CLI.
- Атомарный захват работы через SQL-конструкцию класса `SELECT ... FOR UPDATE SKIP LOCKED`.
- Advisory file locks.
- Fingerprint содержимого файла, позволяющий заметить, что файл изменился после выдачи права.
- Отказ содержит владельца, задачу, время истечения и возможные дальнейшие действия.
- Автор прямо отделяет filesystem isolation от intent coordination: worktree разводит файлы, но не мешает двум агентам взяться за одну функцию.

Механизм: «атомарный job claim плюс file claim с fingerprint».

Уровень зрелости: кодовая реализация, но маленькая аудитория и мало внешней валидации.

3. Limen — Meirtz/Limen

URL: https://github.com/Meirtz/Limen

На 22.09.2026: 8 звёзд, 1 форк, около 127 коммитов. Точная дата последнего коммита не проверена. В README проект обозначен как alpha.

Что реализовано кодом:

- MCP-сервер координации на SQLite.
- Типизированные leases: read, write и propose.
- Scope задаётся path region.
- TTL, renew и release.
- Mediated write: сервер сверяет, входит ли target в lease, и записывает hash, размер, агента и свидетельство операции.
- Опциональная подпись Ed25519.

Механизм: «типизированные leases на области путей плюс засвидетельствованная запись».

Уровень зрелости: код есть, но проект ранний; сильнее обычного `.lock` тем, что контролирует не только выдачу lease, но и саму заявленную запись.

4. OpenMOSS/claude-codex-handoff

Основной репозиторий: https://github.com/OpenMOSS/claude-codex-handoff

Протокол: https://github.com/OpenMOSS/claude-codex-handoff/blob/main/PROTOCOL.md

История коммитов: https://github.com/OpenMOSS/claude-codex-handoff/commits/main/

На 22.09.2026: 40 звёзд, 2 форка, 5 коммитов. Последний проверенный коммит — 04.07.2026.

Это самый прямой найденный аналог именно Claude Code × Codex:

- Два append-only JSONL-потока сообщений.
- Типы конвертов: task, handoff, question, done, status, error, cancel.
- Atomic claim files.
- Отдельные cursors по сессиям.
- Send locks.
- Правило «side effect до продвижения cursor», чтобы сообщение не потерялось при падении.
- Оба агента используют одну `.handoff`-папку проекта, оставаясь отдельными процессами.

Механизм: «двусторонний append-only mailbox Claude↔Codex с claim-файлами и курсорами».

Уровень зрелости: реализовано кодом и спецификацией, но маленькая история и нет полноценного task/file lease hierarchy.

5. shared-agent-memory — dan-calin/shared-agent-memory

Основной репозиторий: https://github.com/dan-calin/shared-agent-memory

История коммитов: https://github.com/dan-calin/shared-agent-memory/commits/main/

На 22.09.2026: 18 звёзд, 3 форка, 12 коммитов. Последний проверенный коммит — 03.08.2026.

Что реализовано:

- Общая память через MCP.
- Опциональный координационный журнал `activity.jsonl`.
- Claims на точный файл с TTL около двух часов.
- Claude Code `PreToolUse` hook, способный предупреждать или блокировать конфликтующую запись.
- Часть поведения дополнительно задаётся инструкциями в `CLAUDE.md`/`AGENTS.md`.

Механизм: «exact-file claim плюс hook перед записью».

Уровень зрелости: смешанный. Файловый claim и hook — код; часть дисциплины — prompt convention. Это ближайший найденный аналог пункта G, но без question-level claim.

6. GitGuardex — opencue/gitguardex

URL: https://github.com/opencue/gitguardex

На 22.09.2026: 28 звёзд, 4 форка; на странице указана версия 7.1.1. Точная дата последнего коммита не проверена.

Что реализовано:

- Отдельный worktree и branch на задачу.
- Явные file claims.
- PR-only delivery.
- Preflight, CI, review и fail-closed cleanup.
- Контролируемое слияние вместо совместной записи в один checkout.

Механизм: «worktree на задачу плюс file claims и merge gate».

Уровень зрелости: кодовая альтернатива общей папке. Он устраняет физическую гонку записей, но не сам по себе семантический дубль работы.

7. pact — zekariasasaminew/pact

URL: https://github.com/zekariasasaminew/pact

На 22.09.2026: 2 звезды, 0 форков, около 241 коммита. Точная дата последнего коммита не проверена.

Что реализовано:

- Worktrees для исполнителей.
- Общий SQLite/WAL coordinator.
- Advisory glob leases с TTL.
- Threaded messaging и typed handoff lifecycle.
- Разворачивание glob-паттернов по реальному workspace для поиска пересечений.
- PID/git lock вокруг `worktree add/remove`.
- Аудит конфликтов.

Важно: сам README предупреждает, что file-claim coordination экспериментальна и не гарантирует надёжность при настоящей конкуренции.

Механизм: «worktree-изоляция плюс экспериментальные glob-leases и handoff state machine».

Уровень зрелости: код есть, но автор сам не заявляет production-grade exclusivity.

8. codex-orchestrator — zm2231/codex-orchestrator

URL: https://github.com/zm2231/codex-orchestrator

На 22.09.2026: 1 звезда, 0 форков, 3 коммита. Точная дата последнего коммита не проверена; проект очень ранний.

Что реализовано:

- Постоянные процессы Claude и Codex.
- Worktree isolation.
- Ролевой pipeline implementer → reviewer → QA.
- Reviewer работает read-only и обязан выдать `APPROVED` либо `CHANGES_REQUESTED`.
- REST/SSE-интерфейс.

Механизм: «фиксированная ролевая цепочка поверх отдельных worktree».

Уровень зрелости: кодовый prototype; роли есть, но они назначены по стадии pipeline, а не динамически по отдельному артефакту.

9. multi-agent-orchestrator-skill — hyw007726/multi-agent-orchestrator-skill

URL: https://github.com/hyw007726/multi-agent-orchestrator-skill

На 22.09.2026: 3 звезды, 1 форк, около 168 коммитов. Точная дата последнего коммита не проверена.

Что реализовано:

- Поддержка Claude, Codex, Gemini, Kilo и OpenCode.
- Отдельный worktree на worker.
- Общая runtime/coordination область.
- Проверка результатов, restart и handoff.

Механизм: «мультивендорный launcher с worktree на исполнителя».

Уровень зрелости: код есть; это автоматизация изоляции, а не протокол совместной записи в одну папку.

10. Colony — opencue/colony

URL: https://github.com/opencue/colony

README: https://github.com/opencue/colony/blob/main/README.md

Количество звёзд и точная дата последнего коммита в этом проходе не извлечены.

Что реализовано:

- Локальная SQLite/FTS-база.
- Claims, handoffs и ready-state задач.
- Второй агент видит чужой claim и может отступить, перейти в review или выбрать другую lane.
- Сочетание task-level coordination и file claims.
- Colony координирует, но не исполняет coding agents самостоятельно.

Механизм: «семантический task claim плюс файловая видимость и handoff».

Уровень зрелости: по дизайну это один из наиболее близких аналогов question-level exclusivity; стабильный `question_id + epoch` в форме, описанной Palo Alto AI Research Lab, не найден.

Дополнительные worktree-обёртки, найденные, но не включённые в строгую сравнительную таблицу из-за отсутствия проверенных в этом проходе метаданных по свежести:

- Agetor: https://github.com/alamops/agetor
- AWO: https://github.com/ystepanoff/awo
- Magic CC Codex Worker: https://github.com/wenqingyu/magic-cc-codex-worker
- Orka: https://github.com/Dusttoo/orka
- Orchestrate: https://github.com/bestagentkits/orchestrate

Итог по репозиториям: найдено десять проектов, которые достаточно близки и имеют код; только OpenMOSS/claude-codex-handoff прямо называет пару Claude×Codex. Axis и Colony ближе остальных к семантическому «одна задача — один исполнитель». MCP Agent Mail и Limen сильнее остальных на почте, lease и аудите. Большинство проектов молоды и с небольшой независимой пользовательской базой.

A2. Worktree против общей папки

Официальная документация OpenAI описывает worktree как отдельный checkout с общей Git-базой: параллельные чаты получают независимые рабочие каталоги. Один branch нельзя одновременно checkout в два worktree; ignored-файлы приходится переносить через `.worktreeinclude`, а symlink там пропускается. Codex App также чистит старые worktree и по умолчанию сохраняет ограниченное число последних. https://learn.chatgpt.com/docs/environments/git-worktrees

Anthropic предлагает `claude --worktree` как штатный способ запустить изолированные параллельные сессии. https://code.claude.com/docs/en/common-workflows

Практическое сравнение:

Общая папка лучше, когда:

- Агентам нужен мгновенно общий runtime-state: mailbox, локальная БД, generated artifacts, один запущенный стенд.
- Один агент должен сразу видеть незакоммиченное изменение другого.
- Нужен дешёвый handoff без merge/rebase.
- Работа мелкая и ownership можно надёжно разделить claims.

Worktree лучше, когда:

- Агенты массово переписывают код.
- Изменения долго живут независимо.
- Нужен обычный Git review и откат.
- Один агент может испортить индекс, branch или незакоммиченные изменения другого.

Слабости worktree:

- Он разводит байты, но не намерения: два агента могут независимо реализовать одну функцию или внести несовместимые schema/API-решения. Это прямо мотивирует job board в Axis. https://github.com/VirSanghavi/axis
- Возникают forgotten merges и незавершённые ветки; практики предлагают end hooks и merge automation. https://www.reddit.com/r/ClaudeAI/comments/1t9tolw/running_two_claude_code_agents_on_the_same_repo/
- Порты, фоновые worker-процессы, контейнеры и БД остаются общими, если их не изолировать отдельно. https://www.reddit.com/r/ClaudeAI/comments/1vx0lkt/if_you_run_multiple_ai_agents_on_the_same_repo/
- Secondary repositories, nested repos и runtime configuration плохо вписываются в «один worktree решает всё». https://news.ycombinator.com/item?id <tg-id>
- Есть реальные баги привязки agent session к неправильному worktree. https://github.com/openai/codex/issues/31572
- Codex Apply/Handoff мог целиться не в тот worktree. https://github.com/openai/codex/issues/25685
- Управляемый worktree Codex мог изменить shared Git config и повлиять на все связанные checkout. https://github.com/openai/codex/issues/34828
- В Claude Code наблюдалось пересечение cwd между параллельными subagents. https://github.com/anthropics/claude-code/issues/76250

Вывод: worktree — хорошая физическая изоляция, но не замена semantic claim, resource lease и явному handoff.

A3. Что выяснили практики

1. Reddit, r/ClaudeAI, 08.02.2026, пользователь kargnas2

Тред: https://www.reddit.com/r/ClaudeAI/comments/1qzduim/stop_running_multiple_claude_code_agents_in_the/

Наблюдение: несколько Claude Code в одном checkout перезаписывают и откатывают изменения друг друга. Предложенное лечение — отдельные worktree. Обсуждение отдельно отмечает новый предел: после распараллеливания bottleneck переносится в человеческий review, а плохое разбиение scope создаёт новую волну конфликтов.

2. Reddit, r/ClaudeAI, 11.05.2026, автор EastMove5163

Тред: https://www.reddit.com/r/ClaudeAI/comments/1t9tolw/running_two_claude_code_agents_on_the_same_repo/

TomBiohacker описал семантическую коллизию: один агент меняет базовый тип или конфигурацию и молча делает план другого устаревшим. Schemas, migrations и cross-cutting refactors плохо параллелятся даже при разных файлах.

Appropriate-Seat-534 отметил, что чистый Git merge не означает логически согласованную систему. sambeau отдельно указал на забытые worktree-ветки и предложил автоматический end hook.

3. Reddit, r/ClaudeAI, 24.08.2026; комментарии продолжались примерно 21–28 дней до снимка

Тред: https://www.reddit.com/r/ClaudeAI/comments/1vx0lkt/if_you_run_multiple_ai_agents_on_the_same_repo/

Это один из немногих найденных живых тредов за последние 2–4 недели.

Практические выводы участников:

- quesobob: контейнер или network namespace нужен каждому агенту, иначе worktree не решает порты и процессы.
- Plastic-Risk-6309: leases нужны и симулятору/runtime-ресурсам.
- please-dont-deploy: worktree даёт filesystem isolation, но не ownership; модульный ownership приходится сериализовать очередью.
- aiku-io: отдельная временная БД и уникальный port на сессию; старый background worker способен выполнить задания новой сессии.
- two-pigeons: агенты используют общий chatroom, чтобы объявлять рабочие lanes.

4. Reddit, r/AI_Agents, 2026

https://www.reddit.com/r/AI_Agents/comments/1v53mns/running_multiple_coding_agents_in_parallel_broke/

Описан shared tree, в котором изменения оказались частично перезаписаны. Worktree устранил физическое повреждение, а objective fingerprint использовался, чтобы отличить агента, который реально продвигается, от агента, который циклически повторяет одну активность.

5. Reddit, r/ClaudeCode, 2026

https://www.reddit.com/r/ClaudeCode/comments/1sr2ni6/how_are_you_guys_managing_multiple_ai_coding/

Практический совет — делить работу заранее по module/service boundaries. Это полезно, но остаётся соглашением, если не подкреплено task claim или enforcement hook.

6. Hacker News, Show HN: ChatML, около марта 2026, автор Marcio Castilho / mcastilho

https://news.ycombinator.com/item?id <tg-id>

Desktop-интерфейс для параллельных coding agents на worktrees; автор заявлял более 750 PR. Отклик был скромным: 6 points и 12 comments на момент снимка. Это полезный отрицательный сигнал: удобная worktree-обёртка без нового координационного примитива сама по себе не гарантирует заметного распространения.

7. dev.to, Andrea Schiona, начало сентября 2026

https://dev.to/andrea_schiona/running-coding-agents-in-parallel-with-git-worktrees-4cnk

Автор называет совместный checkout источником конфликтов и предлагает worktree. Материал полезен как практическое руководство, но semantic collision отдельно не решает.

8. Lobsters и X/Twitter

В этом проходе я не нашёл ни одного достаточно релевантного Lobsters-треда с проверяемым описанием реальной коллизии. Надёжно извлечь инженерные X/Twitter-треды также не удалось. Это «0 подтверждённых результатов поиска», а не утверждение, что таких публикаций нет.

A4. Официальная позиция Anthropic и OpenAI

Anthropic

Официальные agent teams:

https://code.claude.com/docs/en/agent-teams

По состоянию на 22.09.2026:

- Функция экспериментальная и выключена по умолчанию.
- Team lead создаёт независимые Claude Code instances.
- У команды есть shared task list и direct messaging.
- Захват задач защищается file locking.
- Inbox и task state хранятся локально; сообщение считается отправленным после успешной записи в mailbox, а получатель валидирует его при чтении.
- `TeammateIdle`, `TaskCreated` и `TaskCompleted` hooks могут остановить действие через exit code 2.
- Anthropic прямо предупреждает: если два участника редактируют один файл, изменения могут быть перезаписаны; рекомендуемая профилактика — дать каждому разные файлы.
- Документация перечисляет lagging task status, отсутствие nested teams и ограничение одной команды на сессию.

Это уже встроенная координация, но только между экземплярами Claude. Обязательного per-file lease, semantic question claim или compare-and-swap записи документация не описывает.

Для отдельной физической изоляции Anthropic рекомендует `claude --worktree`. https://code.claude.com/docs/en/common-workflows

Показательные открытые/закрытые проблемы:

- CWD оставался в удалённом или протухшем worktree: автор rtimmons, 01.04.2026, закрыто как not planned. https://github.com/anthropics/claude-code/issues/42282
- Параллельные subagents получали общий cwd-slot и мутировали чужой worktree; issue описывает повторяемость после прежнего исправления и десятки инцидентов. https://github.com/anthropics/claude-code/issues/76250
- `.claude` subdirectories не наследовались в worktree; участники обсуждали symlink и settings inheritance. https://github.com/anthropics/claude-code/issues/28041
- Sandbox subagent не мог обратиться к worktree в `/tmp` даже через additional directories. https://github.com/anthropics/claude-code/issues/32034

OpenAI

Официальная документация Codex делает ставку на worktrees для параллельных chats. Общей папке с locks отдельного официального режима в проверенной документации не найдено. https://learn.chatgpt.com/docs/environments/git-worktrees

Наиболее релевантные requests и bugs:

- tianalei, 06.08.2026: просьба автоматически координировать overlapping write sets, file ownership и compare-before-write, а блокировать только настоящий semantic conflict. Issue открыт. https://github.com/openai/codex/issues/37226
- Правило «увидел unexpected changes — остановись» слишком грубое для нескольких агентов на общей ветке; предлагается продолжать при unrelated changes и останавливаться только при semantic conflict. https://github.com/openai/codex/issues/16195
- michaeltyson, 17.05.2026: spawned agent работал не в ожидаемом worktree; закрыто как duplicate. https://github.com/openai/codex/issues/23095
- Shared checkout и branch drift между subagents. https://github.com/openai/codex/issues/31572
- rs545837, 05.03.2026: entity-level merge функций и классов вместо line-level merge; issue закрыто как not planned. https://github.com/openai/codex/issues/13554
- scharalambous3, 28.02.2026: запрос `codex --worktree`; закрыто как duplicate. https://github.com/openai/codex/issues/13120
- garylesueur, 11.08.2026: UI показывал checks от неправильного worktree; issue открыт. https://github.com/openai/codex/issues/38082
- pezezzle, 01.06.2026: Apply/Handoff направлялся не в тот worktree; issue открыт. https://github.com/openai/codex/issues/25685
- jomoskus, 22.07.2026: managed worktree менял shared Git config; issue открыт. https://github.com/openai/codex/issues/34828
- Windows: `apply_patch` мог уйти за границы linked worktree. https://github.com/openai/codex/issues/37522

Итог по официальной позиции: оба вендора признают параллельность и строят вокруг task lists, subagents и worktrees. Anthropic уже имеет mailbox и task locking внутри одной vendor-системы. Ни один из проверенных официальных продуктов не предоставляет межвендорный Claude×Codex semantic claim + exact-file lease для одного checkout.

A5. Что стоит забрать

1. Semantic claim до file claim

Стоимость: средняя.

Источник идеи: Axis job board и Colony task claims. https://github.com/VirSanghavi/axis  
https://github.com/opencue/colony

Что чинит: два агента берут разные файлы, но решают один вопрос или одновременно меняют один API-контракт.

Рекомендуемый объект: `question_id`, `epoch`, `owner`, `intent_fingerprint`, `dependencies`, `expires_at`.

2. Content fingerprint и compare-before-write

Стоимость: дёшево/средне.

Источник: Axis. https://github.com/VirSanghavi/axis

Что чинит: lease был выдан честно, но файл изменился до записи. Агент обязан получить `STALE_BASE`, перечитать файл и перепланировать, а не молча затереть изменение.

3. Typed leases: read/write/propose

Стоимость: средняя.

Источник: Limen. https://github.com/Meirtz/Limen

Что чинит: reviewer может анализировать или предложить patch, не получая право переписать артефакт автора.

4. Mediated write receipt с hash и actor witness

Стоимость: средняя/дорогая.

Источник: Limen. https://github.com/Meirtz/Limen

Что чинит: «lease был» перестаёт быть недоказуемым обещанием. В журнал попадают agent, исходный hash, конечный hash, bytes и результат записи.

5. Threaded mailbox и searchable audit

Стоимость: средняя.

Источник: MCP Agent Mail. https://github.com/Dicklesworthstone/mcp_agent_mail

Что чинит: агент видит не только текущий lock, но и причину, владельца, связанную задачу и историю handoff.

6. PreToolUse/pre-commit enforcement

Стоимость: дёшево для Claude Code, средне для мультивендорной поддержки.

Источники: shared-agent-memory и MCP Agent Mail.  
https://github.com/dan-calin/shared-agent-memory  
https://github.com/Dicklesworthstone/mcp_agent_mail

Что чинит: правила перестают быть только текстом в `AGENTS.md`.

7. Runtime-resource leases

Стоимость: средняя.

Источник реальной боли: параллельные агенты конфликтуют не только за файлы, но и за ports, test DB, browser profile, simulator и background workers. https://www.reddit.com/r/ClaudeAI/comments/1vx0lkt/if_you_run_multiple_ai_agents_on_the_same_repo/

Нужные scopes: `tcp:3000`, `db:test-17`, `browser-profile:X`, `device:ios-sim-2`, `migration:head`.

8. Явный loser protocol

Стоимость: дёшево.

Что должен узнать проигравший:

- кто владелец;
- какой `question_id`;
- какой scope конфликтует;
- когда истекает lease;
- hash/epoch, на котором он проиграл;
- допустимые действия.

Что он должен сделать:

- `STAND_DOWN`: прекратить запись;
- `WAIT`: ждать release/expiry;
- `SWITCH_LANE`: взять независимую задачу;
- `REVIEW`: перейти в read-only reviewer;
- `HANDOFF_REQUEST`: попросить владельца разделить scope.

Axis уже возвращает actionable denial; task-level вариант виден в Colony.  
https://github.com/VirSanghavi/axis  
https://github.com/opencue/colony

Критический принцип: проигрыш должен быть отдельным машинно-читаемым outcome, а не warning в логе. Победитель при release публикует artifact hash, тесты и итоговый status; проигравший перед продолжением перечитывает состояние.

B. ГДЕ PALO ALTO AI RESEARCH LAB ВПЕРЕДИ

Оценка ниже — не патентный поиск и не доказательство глобальной уникальности. Это честное сравнение с найденными реализациями и официальными механизмами на 22.09.2026.

B-A. Роли implementer/reviewer назначаются по артефакту; себя никто не ревьюит

Вердикт: существует в более слабом виде; вы впереди по гранулярности, но не по самой идее разделения ролей.

Похожие вещи:

- codex-orchestrator имеет implementer → reviewer → QA и read-only reviewer. https://github.com/zm2231/codex-orchestrator
- Anthropic agent teams поддерживает специализированных teammates и review-паттерны, но не описывает обязательное динамическое назначение роли на каждый артефакт. https://code.claude.com/docs/en/agent-teams

Не найдено: общая матрица, где Claude является автором файла X и reviewer файла Y, Codex — наоборот, а self-review технически запрещён.

Наивность: если правило не обеспечено author identity в журнале и машинной проверкой `reviewer_id != author_id`, оно остаётся конвенцией. Нужен negative test на alias, restart и смену session ID.

Уверенность: средняя.

B-B. Claim на вопрос → lease на точный файл → advisory-видимость

Вердикт: основные детали существуют отдельно; связанная трёхступенчатая иерархия со стабильным `question_id` и epoch в найденных проектах не обнаружена. Здесь вы действительно впереди, но не «с нуля».

Аналоги:

- Axis: атомарный job claim плюс file claim. https://github.com/VirSanghavi/axis
- Colony: task claims и file-level coordination. https://github.com/opencue/colony
- Anthropic: shared task claim с file locking, но не output-file lease. https://code.claude.com/docs/en/agent-teams
- MCP Agent Mail: advisory file reservations, но не обязательный semantic claim. https://github.com/Dicklesworthstone/mcp_agent_mail

Сильная сторона вашей схемы: вопрос, физический output и наблюдаемость — разные сущности. Это правильно ловит случай «разные файлы, одна задача».

Риск: стабильный `question_id` сам по себе не ловит перефразированные дубли. Нужны canonical subject, affected symbols/resources и необязательный intent fingerprint. Автоматическое semantic matching следует оставлять advisory, иначе false positives начнут сериализовать независимую работу.

Уверенность: средне-высокая.

B-C. Смена пишущего только после release/close и нового конверта; reviewer не правит ветку автора

Вердикт: в существенной части это уже существует. Вы не впереди концептуально.

Аналоги:

- Worktree/PR pipelines отделяют автора от reviewer и передают результат через merge/review. https://github.com/opencue/gitguardex
- codex-orchestrator делает reviewer read-only и требует явный verdict. https://github.com/zm2231/codex-orchestrator
- OpenMOSS задаёт typed handoff/done/cancel envelopes. https://github.com/OpenMOSS/claude-codex-handoff/blob/main/PROTOCOL.md

Ваш вариант ценен как строгая state machine для общей папки. Новизна возможна не в правиле, а в формальном переходе:

`CLAIMED → WRITING → PROPOSED → UNDER_REVIEW → RELEASED/CLOSED → NEW_EPOCH`.

Наивность: запрет reviewer править ветку автора иногда замедляет тривиальные исправления. Нужен отдельный `propose_patch` без права применить patch.

Уверенность: высокая.

B-D. Один физический источник инструкций/скиллов через junction

Вердикт: практично и сравнительно редко, но это не сильная исследовательская новизна.

Близкие решения:

- OpenMOSS использует одну shared `.handoff`-область для Claude и Codex. https://github.com/OpenMOSS/claude-codex-handoff
- Пользователи Claude Code просили symlink/settings inheritance для worktrees, потому что копирование `.claude` расходится. https://github.com/anthropics/claude-code/issues/28041
- Codex предлагает `.worktreeinclude`, то есть копирование ignored state, а не единый физический источник. https://learn.chatgpt.com/docs/environments/git-worktrees

Где вы сильнее: устранён silent configuration drift между Claude и Codex.

Где схема наивна:

- Windows junction не является переносимым протоколом.
- В контейнере, WSL, macOS и remote runner понадобится другой mount/symlink adapter.
- Общий источник создаёт общий blast radius: ошибочная правка мгновенно ломает обоих.

Правильная формулировка для open source: «single authoritative skill store with platform adapters», а junction — одна референсная реализация.

Уверенность: высокая.

B-E. SQLite + `BEGIN IMMEDIATE`; честно отсутствующая межмашинная эксклюзия

Вердикт: SQLite local authority широко существует; здесь вы не впереди. Честная граница гарантий — сильная инженерная дисциплина, но не новый механизм.

Аналоги:

- MCP Agent Mail использует SQLite и локальную координацию. https://github.com/Dicklesworthstone/mcp_agent_mail
- pact использует SQLite WAL coordinator. https://github.com/zekariasasaminew/pact
- Colony использует локальную SQLite. https://github.com/opencue/colony

Плюс вашей формулировки: она не выдаёт локальную транзакцию за distributed mutex.

Недостающее: fencing token. Даже на одном узле старый процесс способен проснуться после TTL и записать результат нового epoch. Каждая write/commit операция должна предъявлять монотонный lease generation; устаревший generation отвергается.

Для межмашинной версии понадобятся либо single-writer coordinator, либо настоящий consensus/conditional store. Перенос SQLite-файла через Syncthing/Dropbox не создаёт distributed lock.

Уверенность: высокая.

B-F. Sender fail-closed проверяет verb по контракту приёмника, извлечённому статическим разбором его исходника

Вердикт: точного аналога в проверенных проектах не найдено. Это наиболее редкая и потенциально оригинальная часть вашей системы.

Ближайшие, но более слабые механизмы:

- Anthropic пишет сообщение в mailbox, после чего получатель валидирует его при чтении; это consumer-side validation, а не sender-side extraction контракта из consumer source. https://code.claude.com/docs/en/agent-teams
- MCP требует JSON Schema для tool inputs/outputs; контракт публикуется явно, а не извлекается отправителем из реализации приёмника. https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-11-25/server/tools.mdx
- Agent Messaging Protocol описывает envelope/security/delivery validation, но не sender-side AST extraction списка verbs. https://github.com/agentmessaging/protocol/blob/main/spec/07-security.md
- Agentmail проверяет подпись, hash, decrypt/replay properties сообщения, а не допустимость доменного verb по исходнику consumer. https://github.com/erichexter/agentmail
- cf-mail использует outbound allowlist и fail-closed send, но это статическая политика отправителя, а не импорт фактического receiver contract. https://github.com/Coldplay-now/cf-mail/blob/main/docs/AGENT_MAIL_PROTOCOL.md

Поэтому ваша гипотеза подтверждается в рамках этого поиска: аналога «sender gate статически читает source of truth приёмника и не выдаёт transport receipt для неизвестного verb» я не нашёл.

Но текущая реализация через AST исходника хрупка:

- Dynamic registration, decorators, generated code и plugins могут не попасть в статический список.
- Языки и версии parser расходятся.
- Sender может видеть другую revision исходника, чем реально запущенный consumer.
- Допустимый verb ещё не означает, что конкретный deployed consumer его применил.

Более сильный публичный вариант:

1. Receiver build генерирует canonical `capabilities.json`.
2. Manifest содержит protocol version, verbs, JSON Schemas, consumer build hash и monotonic contract epoch.
3. Sender проверяет manifest до помещения конверта.
4. В envelope записываются `contract_hash` и `required_consumer_epoch`.
5. Transport возвращает только `ENQUEUED`.
6. Consumer отдельно возвращает `ACCEPTED` или `REJECTED_CONTRACT`.
7. После side effect идёт отдельный `APPLIED` с evidence hash.
8. Статический разбор остаётся CI-проверкой, доказывающей, что manifest совпадает с кодом.

Это сохраняет вашу ключевую защиту от «зелёной транспортной квитанции при недоставке», но убирает runtime-зависимость sender от AST чужого языка.

Уверенность в отсутствии близкого аналога: средне-высокая, не абсолютная.

B-G. Exact-file lease по умолчанию; folder lock только opt-in с причиной, TTL и видимым scope

Вердикт: exact-file claims уже существуют; вы впереди не по примитиву, а по безопасному default и обязательному объяснению расширенного scope.

Аналоги:

- shared-agent-memory заявляет claims на точные файлы и hook enforcement. https://github.com/dan-calin/shared-agent-memory
- Axis имеет advisory per-file locks. https://github.com/VirSanghavi/axis
- MCP Agent Mail, pact и Limen позволяют широкие path/glob regions.  
https://github.com/Dicklesworthstone/mcp_agent_mail  
https://github.com/zekariasasaminew/pact  
https://github.com/Meirtz/Limen

Ваша полезная часть: folder scope требует явного решения, reason и TTL, поэтому случайный широкий claim виден как исключение.

Наивность: exact file недостаточен для generated pairs, schema+migration, header+implementation и package manifest+lockfile. Нужен explicit resource bundle, но без неявного захвата всей папки.

Уверенность: высокая.

Сводный честный рейтинг:

- A: впереди по гранулярности, не по базовой идее.
- B: сильная композиция известных примитивов; вероятно впереди по формализации.
- C: в основном уже известный паттерн.
- D: полезная эксплуатационная находка, слабая переносимость, небольшая исследовательская новизна.
- E: стандартный local-first SQLite; не новизна.
- F: самый вероятный оригинальный вклад.
- G: известный exact-file claim с более безопасной политикой по умолчанию.

C. КУДА И КОМУ ЭТО ПОЛОЖИТЬ

C1. Что именно выпускать

Рекомендуемый пакет:

1. Небольшая vendor-neutral specification.
2. Python reference implementation.
3. MCP server как основной integration surface.
4. Claude Code hook adapter.
5. Codex/AGENTS.md adapter и CLI wrapper.
6. Compatibility adapter для OpenMOSS JSONL.
7. Deterministic collision benchmark.
8. Fuzz/property tests state machine.
9. Один runnable demo: Claude Code и Codex одновременно пытаются взять один question, разные файлы и один runtime port.
10. Threat/guarantee document: что гарантируется на одном узле и чего нет между машинами.

MCP-сервер здесь реалистичнее, чем «ещё один автономный orchestrator»: MCP уже поддерживается несколькими agent hosts и даёт формальные tool schemas. https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-11-25/server/tools.mdx

C2. Где публиковать, чтобы нашли

1. GitHub

Нужны:

- ясное имя, содержащее `agent coordination`, `locks` или `leases`;
- README с 30-секундным demo;
- матрица Claude Code/Codex/Gemini/Aider;
- protocol spec;
- guarantee table;
- benchmark;
- `SECURITY.md`, `CONTRIBUTING.md`, license;
- tagged release, а не только `main`.

Репозиторий должен показывать не архитектурную схему, а воспроизводимый конфликт «до» и предотвращённый конфликт «после».

2. Official MCP Registry

Документация API и namespace/authentication:

https://github.com/modelcontextprotocol/registry/blob/main/docs/reference/api/official-registry-api.md

Для подачи потребуется валидный MCP package, namespace ownership и аутентификация через поддерживаемый GitHub/OIDC либо DNS/HTTP-механизм. Package metadata и transport должны проходить registry validation.

Позиционирование: не «multi-agent framework», а `coordination, semantic claims, leases and verified delivery for coding agents`.

3. awesome-mcp-servers

https://github.com/punkpeye/awesome-mcp-servers

Подавать после рабочего release и короткого demo. Описание должно конкретно говорить: task claims, file leases, contract-checked envelopes; общие слова «AI collaboration» затеряются.

4. Awesome Claude Code

https://github.com/subinium/awesome-claude-code

Уместен Claude hook/plugin adapter. В заявке нужны категория, короткое описание, рабочая ссылка и доказательство, что проект относится к экосистеме Claude Code.

5. Awesome AI Agents

https://github.com/aloth/awesome-ai-agents

Подавать основной проект как infrastructure/tooling, но только после публичной документации и install path.

6. ACP Registry

Объяснение реестра: https://zed.dev/blog/acp-registry  
Спецификация ACP: https://zed.dev/acp

ACP Registry уместен только если вы выпускаете самостоятельный coding-agent executable, который IDE может запускать по ACP. Для одного MCP coordination server это неправильный каталог.

7. Show HN

Правила: https://news.ycombinator.com/showhn.html

Требования по сути: заголовок начинается с `Show HN`, продукт должен быть доступен людям для непосредственного использования, а пост — показывать сделанное, не только идею.

Хороший заголовок: `Show HN: A local lease protocol that stops Claude and Codex overwriting each other`.

Нужен live terminal recording и репозиторий. ChatML показывает, что одна worktree GUI-обёртка получила лишь умеренный отклик; отличаться следует benchmark и новым failure model. https://news.ycombinator.com/item?id <tg-id>

8. Reddit

Наиболее подходящие сообщества:

- r/ClaudeAI — https://www.reddit.com/r/ClaudeAI/
- r/ClaudeCode — https://www.reddit.com/r/ClaudeCode/
- r/ChatGPTCoding — https://www.reddit.com/r/ChatGPTCoding/
- r/LocalLLaMA — https://www.reddit.com/r/LocalLLaMA/
- r/AI_Agents — https://www.reddit.com/r/AI_Agents/
- r/ExperiencedDevs — https://www.reddit.com/r/ExperiencedDevs/

Формат: не пресс-релиз, а postmortem с воспроизводимым race, SQLite trace и сравнением shared-folder/worktree. В r/ExperiencedDevs нужен инженерный разбор, а не самореклама.

9. Zenodo DOI

Инструкция интеграции GitHub: https://help.zenodo.org/docs/github/

Порядок: включить репозиторий в Zenodo, добавить `CITATION.cff` или Zenodo metadata, сделать versioned GitHub release; Zenodo архивирует release и выдаёт DOI.

Это стоит делать одновременно с `v1.0.0-spec`, чтобы citation ссылалась на фиксированный protocol revision.

10. arXiv

По теме уже появляются работы о multi-agent software-engineering environments, например AgentRoom и tap:

https://arxiv.org/abs/2608.23740  
https://arxiv.org/abs/2606.14445

Рекомендация: primary `cs.SE`, secondary `cs.MA` или `cs.AI`. Это вывод по предмету; точные актуальные требования подачи arXiv в этом проходе не проверены.

Статья должна содержать benchmark, иначе спецификация локов будет выглядеть инженерной заметкой, а не исследовательским результатом.

11. AAMAS 2027

Официальный call: https://warwick.ac.uk/fac/sci/dcs/aamas2027/calls/

На момент проверки call включает generative/agentic AI; указанные сроки — abstract 01.10.2026, paper 08.10.2026. Подавать стоит только при наличии формальной модели и экспериментов, а не один Python package.

12. ICSE 2027 AGENT workshop

https://conf.researchr.org/home/icse-2027/agent-2027

Тематически это наиболее точный venue: agentic software engineering, coordination, protocols и evaluation. Указанный дедлайн — 27.11.2026. Подходят tool/benchmark/experience/vision форматы.

13. ICSE 2027 RAISE

https://conf.researchr.org/home/icse-2027/raise-2027

Подходит для software engineering for AIware и AI for software engineering. Call допускает high-quality work, опубликованный или планируемый на arXiv, согласно странице workshop.

C3. Кому написать поимённо и где это будет уместно

1. tianalei — OpenAI Codex issue #37226

https://github.com/openai/codex/issues/37226

Это самый точный внешний адрес: автор прямо просит overlapping write sets, file ownership, compare-before-write и semantic-conflict detection. Issue открыт с 06.08.2026.

Уместное сообщение: ссылка на runnable benchmark и proposal, где semantic question claim дополняет worktree, а не требует встроить весь ваш orchestrator в Codex.

2. Vir Sanghavi — Axis

https://github.com/VirSanghavi/axis

Уместный канал: новый focused issue/discussion про совместимость Axis jobs с `question_id/epoch` и loser outcomes.

Почему: Axis уже формулирует различие между file isolation и intent coordination. Это вероятный первый interoperability partner, а не просто аудитория.

3. Jeffrey Emanuel / Dicklesworthstone — MCP Agent Mail

https://github.com/Dicklesworthstone/mcp_agent_mail

Уместный канал: Discussion или узкий issue с предложением optional semantic claim namespace и delivery receipt levels.

Неуместно: общий рекламный issue «посмотрите наш проект». Нужен конкретный adapter или failing test.

4. Meirtz — Limen

https://github.com/Meirtz/Limen

Уместный канал: issue о mapping `question claim → typed path leases → witnessed write`. Limen уже имеет наиболее сильную модель read/write/propose leases.

5. xpqiu / OpenMOSS

https://github.com/OpenMOSS/claude-codex-handoff

Уместный канал: новый compatibility issue по преобразованию OpenMOSS envelopes в ваш `question_id`, `epoch`, `contract_hash`, `ACCEPTED/APPLIED`.

Это особенно уместно, потому что OpenMOSS уже решает именно Claude×Codex, но не имеет вашей lease hierarchy.

6. zekariasasaminew — pact

https://github.com/zekariasasaminew/pact

Уместный канал: issue с concurrency test и fencing token, поскольку README прямо признаёт экспериментальную надёжность file claims.

7. garylesueur — Codex issue #38082

https://github.com/openai/codex/issues/38082

Issue открыт с 11.08.2026 и имеет обсуждение 12.08.2026. Писать стоит только если вы покажете, как envelope содержит workspace/worktree identity и не позволяет UI принять checks от другого checkout. Общий рассказ про semantic locks там будет оффтопиком.

8. EastMove5163 и участники треда r/ClaudeAI

https://www.reddit.com/r/ClaudeAI/comments/1t9tolw/running_two_claude_code_agents_on_the_same_repo/

Уместен комментарий только с открытым кодом и демонстрацией конфликта base type/schema. Именно эту коллизию file lock не ловит.

9. Участники живого августовского треда r/ClaudeAI

https://www.reddit.com/r/ClaudeAI/comments/1vx0lkt/if_you_run_multiple_ai_agents_on_the_same_repo/

Это подтверждённый живой тред: комментарии были примерно 21–28 дней назад. Сообщение уместно, если protocol уже включает port/DB/runtime leases. Обязательно раскрыть свою связь с проектом.

Не стоит поднимать старые закрытые issues без нового воспроизводимого материала:

- rtimmons / Anthropic #42282 — закрыто not planned: https://github.com/anthropics/claude-code/issues/42282
- rs545837 / Codex #13554 — закрыто not planned: https://github.com/openai/codex/issues/13554
- michaeltyson / Codex #23095 — закрыто как duplicate: https://github.com/openai/codex/issues/23095

Подтверждённых GitHub-тредов с комментариями именно за последние 2–4 недели найдено мало. Живым в требуемом окне уверенно считаю августовский Reddit-тред; перечисленные Codex issues открыты, но их последняя проверенная активность старше четырёх недель.

C4. С какими стандартами состыковаться

1. MCP

Спецификация tools: https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-11-25/server/tools.mdx

MCP решает обнаружение и вызов tools между host/client/server. Он не задаёт semantic ownership рабочей задачи.

Совместимость:

- Вынести `claim_question`, `lease_resource`, `renew`, `release`, `send_envelope`, `ack_envelope` как MCP tools.
- Публиковать JSON Schema.
- Не переопределять MCP transport и capability negotiation.

2. A2A

Спецификация: https://google-a2a.github.io/A2A/specification/  
Proto: https://github.com/a2aproject/A2A/blob/main/specification/a2a.proto

A2A уже имеет tasks, context IDs, messages, artifacts и statuses для удалённого agent-to-agent взаимодействия.

Совместимость:

- `question_id` можно связать с A2A task/context.
- Lease reference, epoch и contract hash передавать extension metadata.
- Не создавать конкурирующий сетевой RPC-протокол, если сообщение уже идёт через A2A.

Ваш протокол должен отвечать за local workspace concurrency; A2A — за удалённое общение агентов.

3. ACP

https://zed.dev/acp

ACP связывает editor/IDE и coding agent. Это не agent-to-agent lock protocol.

Нужна совместимость только на уровне adapter: IDE запускает агента через ACP, а агент вызывает coordination MCP server.

4. Agent Protocol

https://github.com/AI-Engineer-Foundation/agent-protocol

Это общий OpenAPI-интерфейс взаимодействия с агентами. Он не застолбил семантику file/task leases, но его task/step IDs можно использовать как внешний correlation ID.

5. OpenMOSS Claude-Codex Handoff

https://github.com/OpenMOSS/claude-codex-handoff/blob/main/PROTOCOL.md

Это не отраслевой стандарт, но самый важный фактический compatibility target для вашей пары вендоров.

Минимальный мост:

- OpenMOSS task → ваш question claim.
- OpenMOSS handoff → release + new envelope.
- done → close.
- cancel → revoke.
- cursor → last-consumed sequence.
- Дополнительно: contract hash, fencing epoch и accepted/applied acknowledgements.

C5. Какой формат здесь действительно имеет шанс

Наилучший формат: MCP-сервер + короткая формальная спецификация + collision benchmark.

Почему:

- MCP Agent Mail с конкретными mailbox и reservations собрал 2,2 тыс. звёзд. https://github.com/Dicklesworthstone/mcp_agent_mail
- Узкие spec/prototype проекты без широкой интеграции пока имеют заметно меньший охват: OpenMOSS — 40 звёзд, Limen — 8, Axis — 5, pact — 2, codex-orchestrator — 1 на снимке 22.09.2026.  
https://github.com/OpenMOSS/claude-codex-handoff  
https://github.com/Meirtz/Limen  
https://github.com/VirSanghavi/axis  
https://github.com/zekariasasaminew/pact  
https://github.com/zm2231/codex-orchestrator
- Worktree GUI ChatML получил умеренный Show HN-отклик — 6 points и 12 comments, несмотря на заявленные сотни PR. https://news.ycombinator.com/item?id <tg-id>

Это не чистый эксперимент: возраст и продвижение проектов различаются. Но сигнал ясен — «ещё одна схема оркестрации» слаба; инструмент, который ставится одной командой и предотвращает видимый race, значительно сильнее.

Минимальный benchmark должен включать:

- два агента пишут один файл;
- два агента пишут разные файлы одного вопроса;
- lease истёк, старый writer проснулся;
- reviewer пытается применить patch без write-role;
- folder claim блокирует unrelated file;
- одинаковый intent перефразирован;
- два агента борются за port и test DB;
- transport принял неизвестный verb;
- consumer знает verb, но side effect упал;
- crash между записью side effect и продвижением cursor;
- stale receiver contract;
- restart с новым session ID, но старым lease token.

Метрики:

- lost writes;
- duplicate work;
- false serialization;
- median wait;
- stale-writer rejection rate;
- receipt truthfulness;
- recovery after crash;
- ручные вмешательства;
- overhead на операцию.

D. ИТОГОВЫЙ ВЕРДИКТ И ГРАНИЦЫ УВЕРЕННОСТИ

Мир уже решил значительную часть физической проблемы:

- worktrees;
- file claims;
- TTL leases;
- mailboxes;
- SQLite job boards;
- hook/pre-commit enforcement;
- role pipelines.

Мир заметно хуже решил семантическую проблему:

- одинаковая задача в разных файлах;
- изменение базового контракта, обесценивающее чужой план;
- runtime resources вне Git;
- явный machine-readable loser path;
- доказуемое различие `ENQUEUED`, `ACCEPTED` и `APPLIED`.

Palo Alto AI Research Lab не следует позиционировать всё A–G как новый протокол, изобретённый с нуля. A, C, E и G имеют сильные аналоги. D — хорошая эксплуатационная техника. B — наиболее сильная композиционная новизна. F — самый редкий найденный механизм и лучший кандидат на центральный тезис статьи.

Главная корректировка перед публикацией: не делать статический AST приёмника runtime-протоколом. Делать из него CI-генератор и validator canonical capability manifest; в runtime передавать version, manifest hash и fencing epoch, а квитанции разделить на `ENQUEUED`, `ACCEPTED` и `APPLIED`.

Данных не хватило для:

- точных дат последних коммитов части GitHub-репозиториев из-за неполной выдачи GitHub;
- надёжной выборки X/Twitter;
- подтверждённого Lobsters-треда;
- доказательства глобальной уникальности пункта F;
- независимого запуска и нагрузочного теста найденных реализаций.

САМОЕ ценное для заимствования: взять у Axis content fingerprint и actionable loser response, потому что вместе они не только обнаруживают конфликт, но и заставляют проигравшего безопасно остановиться и перепланировать.
