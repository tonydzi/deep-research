# DR26-08-16-MACANTON-01-0731 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

[16.08.2026 00:18 PT]

Ниже именно delta-исследование. Темы из DEDUP не пересобирал. Срез источников: 16 августа 2026. У динамических страниц OpenAI/Anthropic дата публикации не указана; я помечаю их как «live docs, accessed 2026-08-16», а не придумываю дату.

HTML-дашборд не создан: текущая сессия имеет read-only-доступ к файловой системе.

# A. Executive Summary

1. **Ваша архитектура в целом правильная.** Для кода параллельные писатели должны оставаться в отдельных git worktrees. Для негитового Obsidian-волта нужен один писатель на файл/зону; Syncthing не является системой транзакций или блокировок.

2. **Codex теперь официально определяет порядок инструкций:** глобальный файл загружается первым; проектные файлы идут от project root к CWD; более глубокий файл стоит позже и при конфликте должен выигрывать. В каждом каталоге `AGENTS.override.md` полностью заменяет `AGENTS.md` этого же каталога, но не удаляет глобальный слой и не отменяет инструкции из других каталогов. [OpenAI AGENTS.md live docs, дата публикации не указана, accessed 2026-08-16](https://developers.openai.com/codex/guides/agents-md)

3. **Главная найденная опасность для вашего 54–96 KiB глобального канона:** документация называет `project_doc_max_bytes` совокупным пределом проектной цепочки, но формулировки config reference всё ещё говорят о максимуме для `AGENTS.md`. Точный byte-boundary и поведение частичного последнего файла публично описаны недостаточно. Есть задокументированная история тихой обрезки. Поэтому нельзя считать большой глобальный mirror и project chain надёжными без машинного sentinel-теста после каждого обновления. [OpenAI live docs](https://developers.openai.com/codex/guides/agents-md), [GitHub issue #7138, 2025-12, single-source](https://github.com/openai/codex/issues/7138)

4. **`approval_policy = "on-failure"` больше не является скрытым рекомендуемым режимом:** текущий reference прямо объявляет его deprecated. Для non-interactive `codex exec` OpenAI рекомендует `never`; официальный MCP-пример сочетает `never` с `workspace-write`. [OpenAI config reference, live docs](https://developers.openai.com/codex/config-reference), [OpenAI MCP Server guide, live docs](https://learn.chatgpt.com/docs/mcp-server)

5. **Codex parity резко выросла:** skills, plugins, полноценные Claude-compatible hooks, `/import`, local memories, subagents/multi-agent и MCP client/server теперь существуют в официальной документации. Но для вашей двойной harness-системы hooks и memories нельзя включать веером: совпадающие hooks запускаются все, а несколько hooks одного события стартуют параллельно. Это прямой путь к двойной записи журналов, двойному outbound и гонкам. [OpenAI Hooks live docs](https://learn.chatgpt.com/docs/hooks)

6. **Не стоит массово генерировать AGENTS.md и docstrings.** Крупное исследование 2026 года не нашло прироста task success от repo-context-файлов и измерило рост inference cost более чем на 20%; небольшое практическое исследование июля 2026 получило противоположный результат на двух задачах. Правильный вывод не «AGENTS.md бесполезны», а «держать их короткими, проверяемыми и локальными». [SRI/ETH, 2026](https://www.sri.inf.ethz.ch/publications/gloaguen2026agentsmd), [Main Branch, 2026-07-03, low-authority](https://mainbranch.dev/articles/benchmarking-agents-md/)

7. **Лучший общий контур continuity:** SQLite остаётся системой истины для задач, а handoff-файлы — переносимым снимком контекста. Нативный `/import` полезен для разовой миграции и восстановления недавних разговоров, но не заменяет task board: CLI импортирует не более 50 чатов за последние 30 дней и не доступен во время выполняющейся задачи. [OpenAI import docs, live docs](https://learn.chatgpt.com/docs/import)

Моя рекомендация: **Option A — controlled dual harness**: общий SQLite board + один lease-владелец файла + worktree-per-code-writer + сериализация vault writes + короткие nested AGENTS.md + hooks только через единый idempotent dispatcher. Уверенность: **[established]**. Главная предпосылка: ваши lease и task-board операции действительно атомарны, а не являются advisory-файлами поверх Syncthing.

# B. Key Findings

## 1. Codex CLI instruction-layer mechanics

### 1.1 Precedence и merge order

**[established]**

Порядок текущей официальной реализации:

1. В `$CODEX_HOME` Codex выбирает первый непустой файл:

   - `AGENTS.override.md`;
   - иначе `AGENTS.md`.

2. Затем определяется project root. По умолчанию это ближайший предок с `.git`.

3. От project root до текущего рабочего каталога включительно Codex в каждом каталоге выбирает максимум один файл:

   - `AGENTS.override.md`;
   - иначе `AGENTS.md`;
   - иначе первый подходящий `project_doc_fallback_filenames`.

4. Выбранные проектные файлы конкатенируются от корня вниз. Более глубокие инструкции расположены позже и должны иметь приоритет при прямом конфликте.

Глобальный слой идёт раньше проектного, следовательно **проектное правило выигрывает у глобального при явном конфликте**, но глобальный текст остаётся в prompt. [OpenAI AGENTS.md live docs, publication date not shown](https://developers.openai.com/codex/guides/agents-md)

Это prompt precedence, а не механическая policy enforcement. Модель всё равно может неверно разрешить неявный или неоднозначный конфликт.

### 1.2 `AGENTS.override.md`

**[established]**

`AGENTS.override.md` — **полная замена базового файла только в том же каталоге**:

- root override заменяет root `AGENTS.md`;
- nested override заменяет nested `AGENTS.md`;
- global override заменяет глобальный `$CODEX_HOME/AGENTS.md`;
- project override не стирает global guidance;
- override не стирает инструкции из родительских или дочерних каталогов.

Это подтверждается официальным алгоритмом «at most one file per directory» и исходным кодом loader. [OpenAI docs](https://developers.openai.com/codex/guides/agents-md), [OpenAI Codex source, accessed 2026-08-16](https://github.com/openai/codex/blob/main/codex-rs/codex-home/src/instructions/mod.rs)

Запрос на дополнительный `AGENTS.local.md` всё ещё описывал нынешний override как replacement, а не additive overlay. [GitHub issue #28739, 2026-06-17, single-source](https://github.com/openai/codex/issues/28739)

### 1.3 Project root markers

**[established]**

- Default: `[".git"]`.
- Если marker не найден, проверяется только CWD.
- Пустой список отключает подъём к родителям.
- В submodule собственный `.git` останавливает поиск, поэтому AGENTS.md superproject может быть тихо пропущен. [Codex source](https://github.com/openai/codex/blob/main/codex-rs/core/src/agents_md.rs), [issue #30789, 2026-07, single-source](https://github.com/openai/codex/issues/30789)

Для негитового волта стандартный default означает: root `AGENTS.md` не будет найден при старте из глубокой папки, если не настроить marker или не запускать Codex из корня волта.

Рекомендация для волта:

```toml
project_root_markers = [".vault-root"]
```

Создать один стабильный пустой marker в корне волта. Не использовать `.obsidian`: вложенные/копированные конфигурации могут неожиданно менять root semantics.

### 1.4 Fallback filenames и `CLAUDE.md`

**[established]**

Default fallback names официально не перечислены, то есть безопасно считать default пустым. `CLAUDE.md` использовать можно, но только если явно настроить:

```toml
project_doc_fallback_filenames = ["CLAUDE.md"]
```

Fallback проверяется только когда в данном каталоге нет `AGENTS.override.md` и `AGENTS.md`. [OpenAI config reference](https://developers.openai.com/codex/config-reference)

Для вашей системы я не рекомендую глобально включать `CLAUDE.md` fallback: это может незаметно загрузить Claude-specific инструкции и продублировать mirror. Лучше явный тонкий `AGENTS.md`, сгенерированный из канона.

### 1.5 `project_doc_max_bytes` и truncation

**[emerging]**

Официальный guide говорит:

- default 32 KiB;
- Codex перестаёт добавлять файлы, когда совокупный размер достигает предела;
- пустые файлы пропускаются. [OpenAI AGENTS.md docs](https://developers.openai.com/codex/guides/agents-md)

Следствие из root-to-CWD traversal: **в бюджет первыми входят верхние project-файлы; более глубокий поздний файл рискует не попасть полностью или вообще**. Но документация не фиксирует достаточно точно:

- режется ли последний файл по byte boundary или пропускается целиком;
- может ли UTF-8 быть обрезан посреди символа;
- включён ли глобальный `$CODEX_HOME/AGENTS.md` в тот же счётчик;
- выдаётся ли warning во всех поверхностях.

Для этих деталей: **[no source found]**.

Старая полевой issue документирует тихую обрезку без TUI-warning. [Issue #7138, 2025-12, single-source](https://github.com/openai/codex/issues/7138)

`project_doc_max_bytes` указан в официальной схеме как top-level key. Схема `[projects."<path>"]` документирует `trust_level`, но не per-project `project_doc_max_bytes`; поэтому помещать лимит внутрь project table не следует. [OpenAI config reference](https://developers.openai.com/codex/config-reference)

Рекомендованный config:

```toml
project_doc_max_bytes = 131072
project_root_markers = [".git", ".vault-root"]
```

Однако увеличение cap — страховка, не лицензия раздувать always-loaded prompt.

### 1.6 Approval и sandbox

**[established]**

Текущие режимы:

- `untrusted`;
- `on-request`;
- `never`;
- granular approval object.

`on-failure` deprecated; live docs рекомендуют `on-request` для interactive и `never` для non-interactive. [OpenAI config reference](https://developers.openai.com/codex/config-reference)

Sandbox modes:

- `read-only`;
- `workspace-write`;
- `danger-full-access`.

Для доверенного personal monorepo:

```toml
sandbox_mode = "workspace-write"
approval_policy = "never"
```

Это лучший non-interactive default: задача либо выполняется в заранее разрешённых корнях, либо детерминированно падает. Официальный MCP-пример использует эту комбинацию. [OpenAI MCP guide](https://learn.chatgpt.com/docs/mcp-server)

`danger-full-access + never` оставлять только отдельному profile для уже механически ограниченных maintenance jobs. Это не должно быть общим default.

---

## 2. Codex-native parity features

### 2.1 Skills

**[established]**

Codex skill — каталог с обязательным `SKILL.md`, YAML frontmatter `name` и `description`, плюс optional `scripts/`, `references/`, `assets/`. Это структурно совместимо с современным Claude Code skill format, но optional vendor metadata различается. [OpenAI Build skills live docs](https://learn.chatgpt.com/docs/build-skills)

Codex ищет repo skills в `.agents/skills` по пути CWD → repo root, user skills в `~/.agents/skills`, поддерживает symlinks. [OpenAI Build skills](https://learn.chatgpt.com/docs/build-skills)

Вывод:

- одно общее содержимое `SKILL.md` возможно;
- vendor-specific wrappers/metadata держать рядом;
- общий физический каталог лучше публиковать в harness locations symlink’ами;
- scripts должны определять harness по входному contract, а не по случайным environment variables.

### 2.2 Plugins

**[emerging]**

Plugins — installable bundle для skills, MCP, hooks и assets. Документация уже считает их общей поверхностью ChatGPT/Codex, но это более молодая и изменчивая система, чем standalone skills. [OpenAI Build skills](https://learn.chatgpt.com/docs/build-skills)

Для вашего личного волта сейчас plugins оправданы только для действительно переносимого пакета. Локальные canonical workflows оставлять skills + shared scripts.

### 2.3 Hooks

**[established] по API, [emerging] по эксплуатационной зрелости**

Документированы события:

- `SessionStart`, `SessionEnd`;
- `UserPromptSubmit`;
- `PreToolUse`, `PermissionRequest`, `PostToolUse`;
- `PreCompact`, `PostCompact`;
- `SubagentStart`, `SubagentStop`;
- `Stop`.

Формат — JSON hook files/config, события получают JSON через stdin. Codex сохраняет совместимость с Claude plugin environment variables `CLAUDE_PLUGIN_ROOT` и `CLAUDE_PLUGIN_DATA`. [OpenAI Hooks](https://learn.chatgpt.com/docs/hooks)

Критическая семантика:

- все matching hooks из разных файлов запускаются;
- несколько command hooks одного события стартуют параллельно;
- один не успевает остановить другой;
- некоторые tool paths могут обходить общий hook path;
- hooks официально не считаются полным security boundary. [OpenAI Hooks](https://learn.chatgpt.com/docs/hooks)

Поэтому Claude Code hook и Codex hook, вызывающие один и тот же script, дадут два запуска, если оба harness наблюдают одно событие.

Решение: общий dispatcher обязан принимать:

```text
event_id
harness
session_id
turn_id
cwd
target
```

и выполнять атомарный `INSERT ... ON CONFLICT DO NOTHING` до side effect.

### 2.4 `/import` migrator

**[established]**

CLI `/import` умеет переносить из Claude Code:

- instructions → `AGENTS.md`;
- `settings.json` → `config.toml`;
- skills;
- plugins;
- project folders;
- Claude project memories;
- до 50 chats за последние 30 дней;
- MCP config;
- hooks;
- slash commands → skills;
- subagents → Codex agents.

Импорт не удаляет исходную конфигурацию. Команда недоступна во время running task, remote session и при подключении к local app-server daemon. [OpenAI Import live docs](https://learn.chatgpt.com/docs/import)

OpenAI отдельно требует перепроверять после импорта permissions, auth/headers/transports MCP, hook differences и шаблоны с shell interpolation/path placeholders. Это означает, что миграция семантическая, но **не обещает byte-for-byte fidelity**. [OpenAI Import](https://learn.chatgpt.com/docs/import)

Для вас: запускать только в disposable `CODEX_HOME`, затем diff. Не импортировать автоматически поверх production mirror.

### 2.5 Memories

**[emerging]**

Local Codex memories:

- отделены от ChatGPT web memory;
- генерируются из eligible prior chats;
- короткие и активные sessions пропускаются;
- обновляются фоном после idle;
- генерация может быть пропущена при низком остатке rate limit;
- хранятся под `CODEX_HOME`;
- OpenAI прямо говорит не использовать memories как единственный дом обязательных правил. [OpenAI Memories live docs](https://learn.chatgpt.com/docs/customization/memories)

В публичной текущей странице не найдено нормативного описания именно термина «two-stage pipeline»: **[no source found]**. Наблюдаемое разделение — selection/extraction из разговоров и последующее использование local memory — не достаточно для утверждения о стабильном двухстадийном API.

Для вашей системы memories стоит держать выключенными для production continuity: у вас уже есть проверяемая общая память/RAG, а vendor-local summary создаёт второй, невидимый Claude источник истины.

### 2.6 Goals

**[speculative]**

Публичный use-case «Follow a goal» существует, а текущая среда Codex показывает goal API и per-goal token budget. Но официальной публичной страницы, надёжно фиксирующей стабильность, persistence и совместимость целей между CLI 0.144–0.147, найти не удалось: **[no source found]**.

Использовать goals можно как локальный execution budget, но не как task board и не как cross-vendor state.

### 2.7 Multi-agent/subagents

**[emerging]**

Subagents теперь официальная Codex-поверхность; config reference содержит лимиты concurrent threads. [OpenAI Subagents live docs](https://learn.chatgpt.com/docs/agent-configuration/subagents), [config reference](https://developers.openai.com/codex/config-reference)

Для вашей системы включать только для read-only research/review или заранее разделённых worktrees. Не позволять Codex-native subagents создавать второй параллельный scheduler поверх общей доски.

### 2.8 MCP server и client

**[established]**

`codex mcp-server` предоставляет:

- `codex` для старта thread;
- `codex-reply` для продолжения по `threadId`.

Параметры включают cwd, model, sandbox, approval policy, developer instructions и config overrides. [OpenAI MCP Server guide](https://learn.chatgpt.com/docs/mcp-server)

`codex mcp add` регистрирует stdio/HTTP MCP server; config хранится глобально или project-scoped. [OpenAI MCP client docs](https://learn.chatgpt.com/docs/extend/mcp)

Для Claude→Codex это чище, чем shelling out в `codex exec`, если нужен длинный живой thread. Но task identity всё равно должна приходить извне из SQLite.

### Enable now

- Skills: да.
- MCP client: да.
- `codex mcp-server`: да, после canary.
- `/import`: только disposable dry run.
- Hooks: только через idempotent dispatcher.
- Subagents: read-only/isolated tasks.
- Memories: нет в production.
- Goals: экспериментально, не источник истины.
- Plugins: точечно, не мигрировать весь канон.

---

## 3. Concurrency delta since July 2026

### Код

**[established]**

Anthropic теперь документирует native `claude --worktree`: отдельный каталог и branch исключают пересечение файлов между сессиями. [Claude Code worktrees live docs, accessed 2026-08-16](https://code.claude.com/docs/en/worktrees)

OpenAI документирует Codex worktree workflows как отдельную environment surface. [OpenAI Worktrees live docs](https://learn.chatgpt.com/docs/environments/git-worktrees)

Новая индустриальная практика июля–августа 2026 формулирует правило как «one writer per worktree, one explicit handoff point». Это низкоавторитетный, но согласующийся с механикой источник. [ClawTab, 2026-07-18, updated 2026-08-01, low-authority](https://clawtab.cc/articles/claude-code-codex-cli-tmux)

### Негитовый волт

**[established] по механизму, [emerging] по свежим incidents**

Syncthing обнаруживает concurrent versions, но не даёт агентам transactional compare-and-swap на содержимое markdown-файла. Свежие пользовательские отчёты продолжают показывать `sync-conflict` и «undeleted»/вернувшиеся файлы. [Syncthing/Obsidian report, 2026-06-12, low-authority](https://www.reddit.com/r/Syncthing/comments/1u3g0e6/sync_leading_to_undeleted_and_modified_files/)

Практические рекомендации пользователей в июле 2026: исключить volatile `.obsidian/workspace.json` и включить Staggered File Versioning. [Reddit, 2026-07-17, low-authority](https://www.reddit.com/r/ObsidianMD/comments/1uyyvqq/syncthing_for_notes_sync/)

Для vault:

- разные файлы — параллельно под directory/file leases;
- один markdown-файл — сериализовать;
- массовый rename/delete — serialize whole subtree;
- `.obsidian`, SQLite DB, JSONL ledgers — отдельные правила;
- перед записью проверять `mtime + size + hash` против состояния lease acquisition;
- после atomic replace перепроверять hash;
- conflict copies считать RED event, не cleanup noise.

### Новые native coordination features

- Claude Code: native worktrees и worktree hooks.
- Codex: subagents, steering, thread continuation через MCP, imported external sessions.
- Нативной cross-vendor блокировки общих файлов не найдено: **[no source found]**.
- Нативной Codex/Claude общей task queue не найдено: **[no source found]**.

Следовательно ваша SQLite board остаётся необходимой.

---

## 4. Bootstrap undocumented legacy codebase

### Что не делать

**[established]**

Не генерировать сотни per-directory AGENTS.md одной кампанией. SRI/ETH обнаружили отсутствие улучшения success rate и более 20% дополнительной inference cost от context files; авторы рекомендуют минимальные human-written requirements. [SRI/ETH, 2026](https://www.sri.inf.ethz.ch/publications/gloaguen2026agentsmd)

Небольшой июльский benchmark получил противоположный эффект: меньше стоимость и tighter diffs на двух задачах после нескольких повторов. Он поддерживает пользу точного локального контекста, но слишком мал для общей экстраполяции. [Main Branch, 2026-07-03, low-authority](https://mainbranch.dev/articles/benchmarking-agents-md/)

### State-of-practice plan

**[emerging]**

1. **Механическая repo map без LLM**

   - package/module tree;
   - import graph;
   - CLI/daemon/scheduler entry points;
   - database writers/readers;
   - subprocess/network/filesystem side effects;
   - test mapping;
   - churn и runtime usage.

2. **Risk map**

   Приоритет тестов:

   - KEEP-critical engines;
   - код, которого агент коснулся;
   - high fan-in/high fan-out modules;
   - persistence, sync, auth, scheduler;
   - modules с высоким churn и нулевыми тестами.

3. **Документация по demand**

   Создавать nested `AGENTS.md` только когда каталог:

   - имеет особый запуск/тест;
   - содержит опасные invariants;
   - уже вызвал ≥3 ошибки класса;
   - является самостоятельным subsystem boundary.

4. **Docstrings**

   Не поднимать покрытие с 69% до 100% вслепую. Backfill только для public/complex APIs, side effects и agent-touched code. Синтаксически корректный, но неверный docstring хуже отсутствующего.

5. **Agent-touched test gate**

   Для любого изменённого участка:

   - минимум characterization test текущего поведения;
   - regression test для исправляемого дефекта;
   - negative/error-path case;
   - test manifest с именем автора/ревьюера и touched symbols.

Публичные SWE Atlas результаты подчёркивают, что сложность не в количестве тестов, а в выборе правильных targets и call sites. [Scale AI SWE Atlas, 2026-05-07](https://scale.com/blog/swe-atlas-complete)

### Context-efficient format

Для корня:

```text
REPO_MAP.md          2–4 KiB: domains, entry points, commands
SYSTEMS/<name>.md    1–2 KiB: boundary, inputs, outputs, risks
AGENTS.md            только invariants и exact commands
symbols.sqlite       полная механическая карта
```

`llms.txt` полезен как индекс ссылок, но не как копия всей документации. Доказательств, что llms.txt-style repo map сам по себе повышает coding success на больших Python monorepos, не найдено: **[no source found]**.

### Measured outcomes to collect internally

- time-to-first-correct-file;
- files opened per task;
- tokens до первого patch;
- regression escape rate;
- percentage agent-touched functions with tests;
- stale-doc rate;
- false guidance incidents;
- context bytes per successful task.

---

## 5. Cross-vendor session/task continuity

**[established] для primitives, [emerging] для dual-CLI practice**

Нативные разговоры vendor-specific. Codex `/import` может перенести недавние Claude chats, а `codex mcp-server` продолжает собственный thread по `threadId`; ни один механизм не создаёт общую двустороннюю session identity. [OpenAI Import](https://learn.chatgpt.com/docs/import), [OpenAI MCP Server](https://learn.chatgpt.com/docs/mcp-server)

Лучший общий дизайн:

```text
task_id
objective
acceptance_criteria
status
owner_harness
owner_session
lease_expires_at
workspace/worktree
files_or_zones
base_hashes
last_checkpoint
artifact_paths
tests_run
review_vendor
review_status
```

Handoff document — не board, а immutable checkpoint:

```yaml
task_id:
from:
to:
created_at:
expires_at:
workspace:
base_revision:
lease_state:
completed:
remaining:
decisions:
evidence:
tests:
risks:
next_command:
codex_thread_id:
claude_session_id:
```

### Duplicate-work prevention

До начала каждого turn:

1. claim task атомарной SQL-транзакцией;
2. проверить active lease;
3. проверить свежий checkpoint;
4. сравнить base revision/hash;
5. только затем читать большой handoff/context;
6. checkpoint обновлять после каждого material milestone;
7. завершение — `artifact.proposed`, не `done`.

`codex exec resume` или reuse rollout-файла должны считаться оптимизацией доставки контекста, но не источником task state. Точный публичный contract ручного reuse rollout JSONL между версиями 0.144–0.147 найти не удалось: **[no source found]**. Hook docs прямо предупреждают, что transcript format не является стабильным interface. [OpenAI Hooks](https://learn.chatgpt.com/docs/hooks)

# C. Contradictions & Debates

1. **AGENTS.md помогают или мешают?**

   - SRI/ETH: нет прироста success, >20% cost.
   - Маленький практический тест: выигрыш по времени, стоимости и diff size.
   - Синтез: длинные общие инструкции вредят; короткие repository-specific commands/invariants могут окупаться.

2. **Override как удобный local customization**

   - Формально override работает.
   - Практически он заменяет base, поэтому local addition требует копировать весь файл и создаёт drift.
   - Запрос на additive local overlay остаётся обоснованным. [Issue #28739, 2026-06-17](https://github.com/openai/codex/issues/28739)

3. **Hooks как enforcement**

   - Они умеют блокировать/переписывать некоторые tool calls.
   - OpenAI предупреждает, что отдельные paths могут обходить hook pipeline.
   - Поэтому hooks — UX/policy guardrail; финальные security gates должны жить в вызываемых executables и файловых permissions.

4. **Memory как continuity**

   - Удобна для человеческих предпочтений.
   - Фоновая, quota-dependent и vendor-local природа делает её плохим ledger.
   - Для вашей системы RAG + SQLite сильнее по воспроизводимости.

# D. Best Practices

- Один писатель на worktree или vault file.
- Project instructions короче global canon, а не его копия.
- Sentinel в начале, середине и конце каждой instruction layer.
- Startup self-test проверяет, какие sentinels реально видит модель.
- `workspace-write + never` для non-interactive.
- Hooks идут через единый deduplicating dispatcher.
- Side-effect hooks используют durable event IDs.
- Vault write: lease → base hash → backup/versioning → atomic replace → post-hash.
- Context генерируется по task_id, а не «весь репозиторий в prompt».
- Tests follow touched paths.
- `/import` сначала в disposable `CODEX_HOME`.
- Vendor session IDs сохраняются как ссылки, не как primary keys.
- Transcript/rollout JSONL не парсить как стабильный API.

# E. Failure Modes

| Failure | Почему тихий | Митигатор |
|---|---|---|
| Nested AGENTS не загрузился | project root найден не там | `.vault-root` marker + sentinel |
| Хвост инструкций обрезан | cap без обязательного warning | byte counter + end sentinel |
| Override удалил root policy | replacement принят за merge | lint effective chain |
| Claude и Codex записали один note | Syncthing видит версии постфактум | SQL lease + base hash |
| Hook сработал дважды | matching hooks запускаются все | idempotency key |
| Два hooks сделали side effect параллельно | same-event hooks concurrent | dispatcher/transaction |
| `codex exec` ждёт approval в automation | interactive policy | `never` |
| `danger-full-access` расширил blast radius | trusted repo принят за trusted command | separate profile |
| `/import` размножил инструкции | source setup сохраняется | disposable import + diff |
| Memory вернула устаревшее правило | background summary не ledger | rules only in AGENTS/shared DB |
| Worktree task затронул shared vault | worktree изолирует только git tree | отдельный vault lease |
| Generated AGENTS раздули prompt | автоматическая документация стала always-loaded | короткие boundary files |
| Rollout parser сломался после upgrade | transcript schema unstable | explicit handoff schema |
| Codex и Claude взяли одну задачу | chat history не task lock | atomic SQL claim |

# F. Strategic Recommendations for OUR stack

## Instruction design

```text
~/.codex/AGENTS.md
  Personal cross-repo operating floor, ≤20–30 KiB effective target
  Receive-only generated mirror

vault/.vault-root
vault/AGENTS.md
  Только 6 invariants + vault write protocol

code/AGENTS.md
  Repo commands, architecture entry points, test policy

code/<true subsystem>/AGENTS.md
  Только subsystem-specific exceptions

shared/agent-protocol.md
  Полный contract, загружается по task bootstrap, не дублируется
```

Ваш нынешний глобальный mirror может оставаться больше, но механически проверяйте фактическую загрузку. Не полагайтесь на установленный cap без regression probe.

## Concurrency policy

- **Vault:** serialize per file; subtree lease для rename/move.
- **Python code:** worktree per writer.
- **Cross-tree resources:** DB schema, ports, caches, virtualenv, generated indexes — отдельные resource leases.
- **Review:** другой vendor, read-only checkout или clean review worktree.
- **Syncthing:** staggered versioning; ignore volatile workspace files; conflict file = incident.

## Parity rollout

Enable:

- shared skills;
- MCP client;
- Codex MCP server;
- imported subagents only after diff;
- minimal session/checkpoint hooks.

Hold:

- local memories;
- auto-import sync;
- side-effecting duplicated hooks;
- unrestricted Codex-native multi-agent writing.

## Bootstrap plan

Не «документировать 650k LOC». Сначала покрыть 20% модулей, которые создают 80% runtime/change risk:

1. entry-point inventory;
2. import/call/storage graph;
3. touched-path tests;
4. subsystem cards;
5. nested AGENTS только у реальных boundaries;
6. test debt сокращать по usage/churn/risk.

# G. Action Plan

## Phase 0 — 1 день, ноль LLM

- Добавить `.vault-root`.
- Зафиксировать top-level `project_doc_max_bytes`.
- Написать effective-instruction probe с тремя sentinels.
- Проверить 0.144, 0.146 и 0.147 отдельно.
- Посчитать байты каждой effective chain.
- Проверить `workspace-write + never` на create/edit/delete-denied cases.

Критерий: одна таблица version × loaded layers × last sentinel × sandbox result.

## Phase 1 — 2–3 дня

- Ввести общий hook dispatcher.
- Ключ дедупликации: harness/session/turn/event/tool-call.
- Перевести только logging/checkpoint hooks.
- Нарочно запустить Claude и Codex на одном test event.
- Доказать один side effect.

## Phase 2 — 3–5 дней

- Repo-map generator без LLM.
- Список entry points, imports, writers, tests, churn.
- Risk ranking top 50 modules.
- Создать subsystem cards только для top 10.
- Не менять docstrings массово.

## Phase 3 — 1 неделя

- Characterization tests для top critical paths.
- Test-on-touch gate.
- Отдельная очередь «tests missing for touched path».
- Метрики: escapes, tokens, time-to-first-file.

## Phase 4 — canary continuity

- Claude → Codex handoff одного реального task.
- Codex → Claude reverse handoff.
- Kill/restart между checkpoint.
- Duplicate claimant test.
- Expired lease takeover test.
- Stale base-hash rejection.

## Phase 5 — optional

- `codex mcp-server` как постоянный исполнитель Claude coordinator.
- Goals только как token/time budget.
- Multi-agent только для read-only mapping или isolated worktrees.
- Memories оставить выключенными до сравнительного эксперимента.

# H. Sources

1. [OpenAI: Custom instructions with AGENTS.md](https://developers.openai.com/codex/guides/agents-md) — live docs, publication date not shown, accessed 2026-08-16.
2. [OpenAI: Configuration Reference](https://developers.openai.com/codex/config-reference) — live docs, publication date not shown, accessed 2026-08-16.
3. [OpenAI Codex source: agents_md.rs](https://github.com/openai/codex/blob/main/codex-rs/core/src/agents_md.rs) — accessed 2026-08-16.
4. [OpenAI Codex source: home instructions loader](https://github.com/openai/codex/blob/main/codex-rs/codex-home/src/instructions/mod.rs) — accessed 2026-08-16.
5. [OpenAI: Build skills](https://learn.chatgpt.com/docs/build-skills) — live docs, publication date not shown.
6. [OpenAI: Hooks](https://learn.chatgpt.com/docs/hooks) — live docs, publication date not shown.
7. [OpenAI: Import from another agent](https://learn.chatgpt.com/docs/import) — live docs, publication date not shown.
8. [OpenAI: Memories](https://learn.chatgpt.com/docs/customization/memories) — live docs, publication date not shown.
9. [OpenAI: Subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents) — live docs, publication date not shown.
10. [OpenAI: Codex as MCP server](https://learn.chatgpt.com/docs/mcp-server) — live docs, publication date not shown.
11. [OpenAI: Codex as MCP client](https://learn.chatgpt.com/docs/extend/mcp) — live docs, publication date not shown.
12. [OpenAI: Worktrees](https://learn.chatgpt.com/docs/environments/git-worktrees) — live docs, publication date not shown.
13. [OpenAI Codex releases](https://github.com/openai/codex/releases) — version history, accessed 2026-08-16.
14. [Issue #28739: additive AGENTS overlays](https://github.com/openai/codex/issues/28739) — 2026-06-17. [single-source]
15. [Issue #30789: submodule root stops discovery](https://github.com/openai/codex/issues/30789) — 2026-07. [single-source]
16. [Issue #7138: silent AGENTS truncation](https://github.com/openai/codex/issues/7138) — 2025-12. [single-source; older evidence]
17. [Anthropic: Run parallel sessions with worktrees](https://code.claude.com/docs/en/worktrees) — live docs, publication date not shown.
18. [SRI/ETH: Evaluating AGENTS.md](https://www.sri.inf.ethz.ch/publications/gloaguen2026agentsmd) — 2026.
19. [Main Branch: AGENTS.md benchmark](https://mainbranch.dev/articles/benchmarking-agents-md/) — 2026-07-03. [low-authority]
20. [Scale AI: SWE Atlas Complete](https://scale.com/blog/swe-atlas-complete) — 2026-05-07. [single-source]
21. [ClawTab: Claude Code + Codex CLI + worktrees](https://clawtab.cc/articles/claude-code-codex-cli-tmux) — 2026-07-18, updated 2026-08-01. [low-authority]
22. [Syncthing conflict report](https://www.reddit.com/r/Syncthing/comments/1u3g0e6/sync_leading_to_undeleted_and_modified_files/) — 2026-06-12. [low-authority]
23. [Obsidian/Syncthing versioning practice](https://www.reddit.com/r/ObsidianMD/comments/1uyyvqq/syncthing_for_notes_sync/) — 2026-07-17. [low-authority]

## Decision options

**Option A: Controlled dual harness — рекомендую.**  
Общий SQLite board, worktree-per-code-writer, file leases для волта, короткие AGENTS, единый idempotent hook dispatcher. Средняя стоимость внедрения, минимальный риск потери данных. **[established]**

**Option B: Native-first convergence.**  
Перенести coordination в Codex subagents/import/memories/MCP и оставить Claude внешним reviewer. Меньше собственного glue-кода, но vendor lock-in и пока слабее cross-vendor auditability. **[emerging]**

**Option C: Full serialization.**  
Всегда только один активный writer во всём codebase и vault. Самый безопасный вариант, но теряется большая часть экономического смысла двух подписок.

**Рекомендация: Option A. [established]** Главная предпосылка: SQLite task board и leases размещены на локальном транзакционном диске, а Syncthing переносит только результаты/реплики, но не исполняет роль lock manager.

🧒 Простыми словами: Codex и Claude можно пускать в одну мастерскую, но нельзя давать им одновременно пилить одну доску. Для кода каждому нужен свой верстак-worktree. Для заметок нужен жетон: у кого жетон, тот сейчас пишет файл. Syncthing — это курьер, а не диспетчер.

[16.08.2026 00:18 PT]
