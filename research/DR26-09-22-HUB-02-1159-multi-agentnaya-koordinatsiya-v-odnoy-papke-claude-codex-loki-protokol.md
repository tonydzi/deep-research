---
dr_id: DR26-09-22-HUB-02-1159
title: "Мульти-агентная координация в одной папке: Claude×Codex локи/протоколы в мире vs наш прото"
date: 2026-09-22
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-09-22-HUB-02-1159): Мульти-агентная координация в одной папке: Claude×Codex локи/протоколы в мире vs наш протокол

> Три LLM-вендора (ChatGPT, Claude.ai, Grok; GLM упал на капче) сверили, кто в мире уже связал Claude Code и Codex локами/протоколом в общей рабочей папке, честно оценили 7 пунктов нашего координационного протокола (implementer/reviewer по артефакту, claim→lease→visibility, junction-скиллы, SQLite-authority, fail-closed send-гейт, file-lease-по-умолчанию) против найденного, и наметили, куда и кому выложить это как открытый артефакт.

## Ключевые выводы
- Найдено ~15-20 GitHub-репо с реальным кодом координации (не промпт-соглашением): mcp_agent_mail лидирует (2153⭐, пуш 21-22.09.2026, автоинсталлер под все CLI-агентов сразу); почти все остальные — 0-11⭐, созданы июль-сентябрь 2026. Только OpenMOSS/claude-codex-handoff и ещё 2-3 репо явно называют пару именно Claude×Codex, а не N копий одного вендора.
- Академическое исследование по 33 596 PR от AI-агентов в 2807 репо (arXiv 2607.04697) показало: конфликт между PR разных вендоров — 41.7% против 19.8% у одного вендора, но кросс-вендорная параллельная активность — всего 0.5% случаев (122 из 2807 репо). Межвендорная коллизия редкая, но злее.
- AgentRoom (arXiv 2608.23740, 24.08.2026) — ближайший академический аналог: CRDT-слияние конкурентных правок + MCP file-claim с формальным доказательством взаимного исключения; протестирован на реальных Claude Code 2.1.119 и Codex CLI 0.120.0, снижает collision rate с 0.20 почти до нуля.
- Ни у Anthropic, ни у OpenAI нет первоклассной кросс-вендорной координации. Anthropic Agent Teams (экспериментальная, выключена по умолчанию) координирует только N копий Claude через файловый лок на claim задачи + mailbox в «внутренний путь лаборатории»; сама документация предупреждает «два тиммейта, правящие один файл — перезапись». Прямой запрос lock-файла закрыт как «not planned» (issue #19364).
- Живой тред anthropics/claude-code#76727 (wshallwshall, открыт 11.07.2026, комментарии до 20.09.2026): разбор 13 782 Edit/Write за 30 дней на 15-20 параллельных сессиях — 44% ушли в главное дерево (реальный баг), но 29% были корректными абсолютными записями в чужой worktree. Вывод: лок должен смотреть на target path записи, а не на cwd сессии.
- Пункт F нашего протокола (отправитель статически парсит исходник приёмника на допустимые глаголы ДО транспортной квитанции) — аналога не найдено ни в одном из ~20 просмотренных проектов; лучший кандидат на реально новый вклад, но текущая AST-реализация хрупкая (динамическая регистрация, decorators, расхождение версий кода).
- Пункт B (claim на question_id/epoch → lease на точный файл → advisory-видимость) тоже не имеет точного аналога; ближайшие — Axis (атомарный job claim + file claim) и Colony (task claims), но нигде stable question-объект не вынесен отдельно от файла как первый уровень захвата.
- Worktree — вендорский дефолт (`claude --worktree`, автo-worktree у Codex-приложения), но не ловит семантический конфликт: кейс Carlini (Anthropic engineering blog, 05.02.2026) — 16 контейнеров с локами на задачу всё равно затирали фиксы друг друга на неразрезанной задаче сборки GCC; живой Show HN-тред (naw103/foremerge, 492⭐, 22.09.2026) — тот же класс бага с PaymentService.
- Формат, реально получающий отклик: MCP-сервер + одноключевой установщик (mcp_agent_mail, 2153⭐) или arXiv-статья с бенчмарком на именованных версиях CLI (AgentRoom); голые спеки/прототипы без install path остаются на 0-2⭐ независимо от возраста (agent-coord, AGNT-LOCK, kigster/agent-lock и др.).

## Рекомендации / решения
- Паковать протокол как MCP-сервер + Python reference implementation + collision benchmark (сценарии: два пишут один файл, lease истёк и старый writer проснулся, folder-claim блокирует чужой файл, неизвестный verb на transport и т.д.) — это доказанно рабочий формат в нише.
- Ослабить хрупкость пункта F: не гонять runtime AST-парсинг чужого исходника, а генерировать canonical capabilities.json на билде приёмника, проверять CI-валидатором, а receipt разделить на ENQUEUED/ACCEPTED/APPLIED.
- Написать wshallwshall в claude-code#76727 — самый точный и живой адрес: сослаться на его находку (лок по target path, не cwd) и предложить runnable демо, а не презентацию всего протокола.
- Написать naw103 (foremerge, живой HN-тред 22.09.2026) про совместимость question_id/epoch с их symbol-level intent (replace/extend) — прямой interoperability-партнёр.
- Подавать в ICSE 2027 AGENT workshop (дедлайн 27.11.2026) или arXiv cs.SE/cs.MA с реальным бенчмарком на Claude Code × Codex, а не голой спекой — по образцу AgentRoom.
- Не заявлять приоритет по пунктам A, C, E, G (существуют в мире в похожем/слабом виде) — вести публикационный нарратив вокруг B и F как действительно редких находок.
- Явно писать в README границы гарантий (нет межмашинной эксклюзии, папка не лочится по умолчанию, ревьюер не может стать писателем) — это тот же приём честности, что сделал foremerge и agent-sync заметными.

## Сущности
- **Люди:** wshallwshall, hipvlady, tianalei, naw103, Nicholas Carlini, Vir Sanghavi, Meirtz, dan-calin, kigster, daveyod1, garylesueur, Jeffrey Emanuel (Dicklesworthstone), kcarriedo, drexthealpha
- **Компании:** Anthropic, OpenAI, Google (A2A), Zed
- **Продукты/инструменты:** Claude Code, Codex / codex-cli, MCP (Model Context Protocol), A2A, ACP (Zed Agent Client Protocol), mcp_agent_mail, Axis, Limen, OpenMOSS/claude-codex-handoff, AgentRoom, CoAgent, MPAC, Beads, Gas Town, foremerge, worktrunk, claude-squad, Claude Code Agent Teams, agent-locks, agent-coord, pact, shared-agent-memory, GitGuardex, Colony, awesome-mcp-servers, modelcontextprotocol/registry, gh-issue-lease, agent-sync

## Открытые вопросы
- Точные даты последних коммитов части репозиториев не подтверждены — GitHub API отдавал не все метаданные в этом прогоне.
- Уникальность пункта F (fail-closed sender gate по исходнику приёмника) проверена только на публичном открытом коде — не доказательство отсутствия в закрытых решениях.
- openai/codex Discussion #22749 найден только по заголовку, содержимое не прочитано ни одним вендором.
- Систематический обход X/Twitter и Lobsters не дал результатов — пробел поиска, а не доказательство пустоты.
- Живость части тредов (комментарии за последние 2-4 недели) не подтверждена окончательно — нужна ручная перепроверка через `gh issue view --comments` перед контактом.
- Секция GLM не дала содержательного ответа (упёрлась в капчу верификации) — не учтена в синтезе, покрытие фактически 3 вендора из 4.

## Источник
- DR-ID `DR26-09-22-HUB-02-1159` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»

## Связано
- multi-agent-role-discipline
- llm-pact
- one-system-propagate
- machine-bus-telegram-rail
- git-worktree-isolation
- PACT-PARITY-20260922
