# DR26-08-05-HUB-01-1858 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-05-HUB-01-1858
source: grok
source_url: https://приватный чат
mode: SuperGrok Heavy (рой суб-агентов)
topic: Архитектура «фронта без модалок»
collected: 2026-08-05 22:30


---

Thought for 1m 19s

Архитектура «фронта без модалок»: как автономный флот AI-агентов на Windows никогда не встаёт на окне разрешения

Research questions:
Какова архитектура, при которой круглосуточный неприсматриваемый флот Claude Code (десктоп + CLI + MCP) на Windows 11 никогда не приходит в состояние «дальше только человеческий клик», а если пришёл — восстанавливается работа, а не окно?

Scope / Date: Август 2026, фокус на Claude Code / Agent SDK (документация code.claude.com, GitHub issues anthropics/claude-code), сравнительный анализ платформ, Windows-специфика. Источники приоритетно 2025–2026; факты старше ~3 мес. помечены.
Overall confidence: [established] по слоям Claude Code и официальным режимам; [emerging] по поведению scheduled tasks и регрессиям v2.1.206+; [speculative] по узким edge-cases MCP identity rotation.

Executive Summary
Claude Code имеет многослойную систему разрешений (hooks → deny/ask → mode → allow → canUseTool). bypassPermissions и PreToolUse не покрывают явные ask-правила, MCP с requiresUserInteraction, org-connector ask и circuit-breaker’ы. [established]
Scheduled tasks в Desktop имеют per-task permission mode; после v2.1.206 наследование defaultMode сломалось, create_scheduled_task часто поднимает диалог выше конфига. [emerging, подтверждено несколькими issues + docs]
Серьёзные платформы решают unattended либо sandbox’ом (модалка невозможна как класс), либо deny-unapproved / NeverConfirm, либо human-gate только на plan/PR. Pure UI-clicker — самообман. [established]
Рекомендация: маршрутизировать scheduled-работу в headless CLI/SDK с dontAsk+allowlist или изолированным bypass; интерактив — только для browser/OAuth/create; внешний observer вне runtime Claude; handoff-артефакт вместо resume мёртвой сессии. [high confidence на архитектуру, medium на конкретные флаги после обновлений]
1. Карта слоёв (подвопрос 1)

Задокументированное поведение продукта (офиц. docs code.claude.com, даты 2026-07–08):

Механизм	Где работает	Что покрывает	Что НЕ покрывает / структурно выше	Угроза, которую закрывает	Источник + дата
permissions.defaultMode (default/manual, acceptEdits, plan, auto, dontAsk, bypassPermissions)	Desktop app, CLI, SDK, VS Code/JetBrains	Baseline auto-approve	Explicit ask rules, MCP requiresUserInteraction, org connector ask, circuit-breaker rm -rf / ~, protected paths (в старых версиях)	Prompt fatigue vs accidental damage	[web:0][web:5][web:35] 2026-08-05; [web:2] 2026-08-04
permissions.allow / deny / ask (rules + globs)	Все поверхности; managed settings имеют высший приоритет	Pre-approve / block конкретных tool/command	Не переопределяют deny/ask; bare tool name удаляет tool из контекста	Least-privilege	[web:2][web:36] 2026-08
PreToolUse hooks (permissionDecision: allow/deny/ask/defer)	CLI/SDK/Desktop (settings + SDK)	Программный контроль до prompt	Не обходят deny/ask rules; hook deny > allow	Dynamic policy, logging	[web:7][web:39] 2026-08; SDK hooks
CLI: --permission-mode, --dangerously-skip-permissions (=bypass), --allowedTools / --disallowedTools, -p headless	CLI / non-interactive	Запуск без UI	В non-interactive dialog’ов нет; background session с bypass требует предварительного interactive accept	CI / unattended	[web:5][web:19] 2026-08-05
SDK: permissionMode, canUseTool callback, allowedTools	Agent SDK (TS/Python)	Runtime decision	canUseTool shadow’ится bypass/bare allow (warning CLAUDE_SDK_CAN_USE_TOOL_SHADOWED с v2.1.198); hooks рекомендуются для полного контроля	Programmatic agents	[web:1][web:3][web:37] 2026-07
MCP: requiresUserInteraction meta, org connector controls, per-tool always-allow	MCP servers + Claude Code	Force prompt даже в bypass/auto	Identity rotation серверов сбрасывает per-tool allow (issue)	Third-party tool risk	[web:36] 2026-08; issues #82532 2026-07-30 [single-source]
Protected paths (.git, .claude, .vscode …)	Все	Writes never auto (кроме bypass после v2.1.126)	В bypass — allowed; в других — prompt	Accidental repo/config corruption	[web:0][web:35]
Browser site card «Allow Claude to act on <site>?»	Claude for Chrome / computer-use	Always allow только кнопкой	Конфигом settings.json не гасится	Site trust / prompt injection	support.claude.com Chrome permissions; [web:34]
Scheduled-task own mode + approvedPermissions	Desktop routines	Per-task	create_scheduled_task часто hard-coded выше mode (user + issues); inheritance defaultMode сломано v2.1.206	Unattended stall	[web:30][web:38] 2026-07-29; #77817 2026-07-15

Порядок оценки (SDK, наиболее явный): Hooks → Deny rules → Ask rules → Permission mode → Allow rules → canUseTool.
(Встречаются небольшие вариации формулировок между страницами permissions.md и agent-sdk/permissions; deny/ask всегда раньше mode.) [web:37][web:6]

Сообщения сообщества / issues: create_scheduled_task поднимает диалог даже при трёх рычагах (user 2026-07-14 + #76755 2026-07-11, #77817). Protected-dir writes и identity rotation MCP — известные боли. [emerging]

Гипотеза: Некоторые tool’ы (особенно scheduled-tasks MCP) имеют hardcoded path выше permission system для safety circuit-breaker’а. Опровергается только исходниками Anthropic.

2. Правило маршрутизации лейнов (подвопрос 2)

Правило (одна страница):

Интерактивный Desktop app — только работа, где модалка структурно возможна и нужна:
live browser sessions + site-trust cards, OAuth/refresh flows, требующие keychain/UI, AskUserQuestion, MCP с requiresUserInteraction / org-ask, сам create_scheduled_task (update существующего слота — обход), computer-use / screen.
Headless CLI / Agent SDK (claude -p / SDK query) — вся scheduled / unattended работа, которую можно выразить через allowlist + dontAsk или изолированный bypassPermissions. Модалки как класса нет. Cloud routines / web scheduled — zero permission prompts by design.

Что ломается при переносе app → headless:

MCP OAuth/refresh: --bare не читает keychain → нужны explicit API keys / --mcp-config. Решается.
Live browser sessions / Claude-in-Chrome site cards: недоступны или остаются отдельным слоем. Не решается полностью.
Screen / desktop access: отсутствует.
Credential inheritance: subscription login vs <key-var> billing. Решается ключами.
Session-message tools: недоступны в unattended (user 2026-07-27). Решается внешним каналом.
create_scheduled_task hardcode: остаётся app-side. Решается только update существующего слота + ручной pre-approve.

Вывод: Scheduled fleet обязан жить в headless/CLI (или cloud routines). Desktop — только bootstrap и interactive exceptions. [established по docs headless + desktop-scheduled-tasks 2026-07/08]

3. Сравнительная таблица платформ (подвопрос 3)
Платформа	Стратегия unattended	Что делает на unknown tool	Queue / kill / human	Источник + дата
Devin	Isolated VM + sandbox; Autonomous mode с --sandbox; background subagents deny unapproved	Auto внутри sandbox; human на plan/merge	Продолжает в sandbox; human gate на checkpoints	docs.devin.ai 2026-07 [established]
Cursor Cloud / Background agents	Dedicated VM; auto-run terminal; artifacts + PR	Auto внутри sandbox; local может зависнуть	Runs to completion в cloud; local hang на disappeared prompt	cursor.com 2026-08; forum 2025–26
OpenHands	ConfirmationPolicy: AlwaysConfirm / NeverConfirm / ConfirmRisky + LLM analyzer; Docker sandbox	Configurable; headless = always-approve	WAITING_FOR_CONFIRMATION; custom handler	docs.openhands.dev 2026-07 [established]
Aider	--yes-always + --message	Auto-yes	Продолжает; нет modal class	aider.chat 2026-04/05
GitHub Copilot coding agent	Secure Actions env; draft PR; human review before CI	Human для merge/CI; CLI --allow-all / yolo + managed disableBypassPermissionsMode	Agent finishes → PR ждёт human	docs.github.com 2026-07; changelog 2026-07
Amp (Sourcegraph)	Permission rules (allow/ask/deny/delegate); full-auto через allow '*'	Allow-all или ask	Продолжает; policy plugin	ampcode.com/permissions 2026
Factory	Autonomy Level (Off/Low/Medium/High); risk-based; denylist	Ниже level — ask; blocklist never	Streams; pause только выше level	docs.factory.ai 2026-07 [single-source]
Codex CLI	Sandbox modes (read-only / workspace-write / danger-full-access) + --ask-for-approval never	Sandbox bounds; never = no prompt	Headless never stalls	developers.openai.com/codex 2026-07 [established]

Общий паттерн серьёзных команд: либо sandbox делает модалку невозможной, либо deny-unapproved / NeverConfirm, либо human только на plan/PR-gate. Pure mid-tool human click — антипаттерн для unattended.
X (последние 30 дней): практики говорят о permissions-файлах, stacking layers (auto + alignment), phone-ping для dangerous actions, а не о clicker’ах. [post:125] @0xLagosaur 2026-07-30; [post:129] @the_niresh 2026-08-05.

4. Рекомендация под наш стек (3–5 шагов, цена/эффект)
Маршрутизация scheduled → headless CLI/SDK с dontAsk + жёсткий allowlist (или изолированный bypass в VM)
Закрывает класс проблемы (модалки как класса нет).
Уверенность: high. Опровержение: если конкретный MCP tool всё равно возвращает requiresUserInteraction или CLI падает на OAuth.
Per-task mode = bypassPermissions + pre-approve через «Run now» + Always allow; create_scheduled_task заменить на update существующего слота
Закрывает симптом hard-coded create + regression v2.1.206.
Уверенность: medium (зависит от версии; issues показывают хрупкость). Опровержение: после следующего auto-update mode снова сбросится.
Внешний observer (отдельный процесс / cron / n8n) вне Claude runtime
Метрика: transcript mtime + last event type (pending tool_use / permissionDecision) + process list; алерт в Telegram/email, не зависящий от Claude tools.
Закрывает класс «сторож внутри той же среды».
Уверенность: high. Опровержение: если transcript формат изменится и парсер сломается.
Handoff-артефакт (HANDOFF.md / checkpoint JSON: goal, done, open TODOs, cwd, git status, next action) + свежая сессия вместо resume мёртвой
Закрывает класс «мёртвая сессия с append-only transcript, окна нет».
Уверенность: high (офиц. checkpointing + community pattern). Опровержение: если resume надёжно восстанавливает mid-tool state (сейчас не так).
Config-as-code + canary probe после каждого обновления приложения
Checksum settings.json / per-task mode / allow-list; canary tool call.
Закрывает drift и silent regressions.
Уверенность: high.
5. Явный вердикт по ветке UI-автоматизации (подвопрос 5)

(а) Реализуемость и хрупкость: Технически возможно (UI Automation / Win32 / computer-use). Хрупкость высокая: переезд кнопок между версиями, локализация, focus races, риск клика не в то окно, TOCTOU-атаки на GUI-агентов (arXiv 2026). [established по security research]

(б) Безопасность: Кликер не читает смысл → превращает все будущие гейты (деньги, irreversible, исходящие, секреты) в «автоматическое да». Владелец думает, что гейт есть. Это классический self-deception. Anthropic сами предупреждают про computer-use + prompt injection. [established]

(в) Узкая конструкция: Жёсткий allow-list точного текста диалога + имени инструмента + отказ по умолчанию + audit-log + auto-disable на незнакомом — теоретически возможна, но на практике самообманна: любой drift UI или новый tool ломает allow-list, а fail-open возвращает полную дыру.

(г) Что делают серьёзные команды: Sandbox / VM / cloud agent / deny-unapproved / external human-gate на plan/PR. Не кликают модалки.

Вердикт: НЕ рекомендую. Рекомендую только при условиях X = полностью изолированная throwaway VM + жёсткий allow-list + continuous audit + auto-kill на mismatch + human review логов каждый день. Даже тогда — только как временный костыль, не архитектура. [high confidence]

6. Наблюдаемость и анти-паттерны (подвопросы 6–7)

Принцип размещения сторожа: Observer обязан жить вне permission/runtime domain наблюдаемого объекта.
Метрика: «sessions with pending tool_use / permissionDecision > N min без result»; «scheduled runs без EndConversation». Алерт — внешний канал (Telegram/email), потребитель — оператор (телефон), не другой агент.
(Измеренный провал 2026-07-27: сторож внутри той же unattended-среды, канал отключён тем же ограничением.)

Анти-паттерны и ловля:

Fail-open PreToolUse (глотает ошибку → allow) → мониторить exit codes hooks внешне.
Config drift между машинами / per-task vs global → checksum + canary.
«Разрешено» в одном месте, «спроси» в другом (особенно после update) → sample transcript на permissionMode.
Тишина = ок → active heartbeat в external status file; отсутствие heartbeat = алерт.
Sources (все реально открытые)

Офиц. docs Claude Code (2026-07–08):
https://code.claude.com/docs/en/permission-modes (2026-08-05)
https://code.claude.com/docs/en/permissions (2026-08-04/05)
https://code.claude.com/docs/en/agent-sdk/permissions (2026-07)
https://code.claude.com/docs/en/agent-sdk/hooks
https://code.claude.com/docs/en/desktop-scheduled-tasks (2026-07-29)
https://code.claude.com/docs/en/checkpointing (2026-08-04)
https://code.claude.com/docs/en/sessions (2026-08-05)

Issues (community, [single-source] где отмечено):
#77817 (2026-07-15), #76755 (2026-07-11), #82532 (2026-07-30), #40470, #46224 и др.

Платформы: docs.devin.ai (2026-07), cursor.com (2026-08), docs.openhands.dev (2026-07), aider.chat, docs.github.com / github.blog (2026-07), ampcode.com/permissions, developers.openai.com/codex (2026-07).

X (последние ~30 дней): @0xLagosaur 2026-07-30, @the_niresh 2026-08-05 и др. (sentiment, не primary evidence).

Security research: Akamai UI Automation 2024, arXiv TOCTOU 2026, Anthropic computer-use warnings.

Где данных нет — «нет данных» (Factory детали — single-source; точный hardcode create_scheduled_task — только issues + user measurement).

Decision-oriented ending:
Конкретные опции:
A) Полный переход scheduled на headless + dontAsk/allowlist (рекомендуется). Trade-off: потеря live browser/OAuth convenience.
B) Desktop + per-task bypass + external observer (временный). Risk: regressions после update.
C) UI-clicker (не рекомендуется).

Единая рекомендация: A + 3 (external observer) + 4 (handoff). Confidence high. Главное допущение: headless CLI/SDK действительно не имеет modal class и MCP без requiresUserInteraction покрывают workload. Если допущение ложно — возвращаемся к B с жёстким canary.

617 sources
MCP identity rotation
OpenHands confirmation policy




Heavy