---
dr_id: DR26-08-01-MACANTON-02-726
title: "Диета MCP и сессий Claude Code: что реально есть у вендора, а чего нет"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Диета MCP: главный вывод — ToolSearch экономит контекст, а не память

> **Вердикт: `Deferred tool ≠ deferred MCP server`.** ToolSearch откладывает только схемы/контекст. Сервер всё равно подключается в фоне при старте, проходит `initialize` и отвечает на `tools/list` — иначе ToolSearch не знал бы, что искать. Чтобы упала RAM, нужно **отключить соединение**, вынести тяжёлое состояние в HTTP-демон либо остановить саму idle-сессию.

⚠️ **Статус в реестре — `dead`, и это честно:** обе рельсы дали 11 и 13 уникальных URL при пороге 15. Но тела настоящие (16 и 21 KB) и proof-of-work есть (15 веб-поисков). 🤔 Гипотеза о корне: тема — внутренности Claude Code/MCP — бедна публичными цитируемыми источниками; заказ не тощий (промпт 4.9 KB), рельсы не сели. Перезапуск тех же рельс счётчик, скорее всего, не поднимет. **Содержимое использовать можно, а решение про калибровку гейта — за Антоном.**

## 1. Ручки, которые ДОКУМЕНТИРОВАНЫ (established)

- `/mcp` отключает сервер без удаления конфига; состояние пишется per-project в «внутренний путь лаборатории» → `disabledMcpServers` (opt-out) и `enabledMcpServers` (opt-in для default-off).
- **`computer-use` — default-off.** Если его нет в `enabledMcpServers`, Claude Code к нему не подключается. ⭐ Прямо бьёт по нашей задаче: 5.2 ГБ резидентного computer-use.
- Зарезервированные builtin-имена: `workspace`, `claude-in-chrome`, `computer-use`, `Claude Preview`, `Claude Browser`.
- `--strict-mcp-config` без `--mcp-config` — надёжный способ **вообще не грузить конфиг**.
- `--bare` пропускает MCP, plugins, hooks, skills, auto-memory и `CLAUDE.md`.
- `--safe-mode` отключает кастомизации и MCP, оставляя built-in tools — **хороший диагностический baseline**.
- `disableClaudeAiConnectors` — запрещает автоподключение claude.ai-коннекторов, работает в любом scope.
- ⚠️ `--tools "Bash,Edit,Read"` **не управляет MCP** — документация предупреждает об этом прямо. `--disallowedTools "mcp__*"` убирает tools из набора модели, но не соединение.

## 2. Чего у вендора НЕТ (искали — не нашли)

- лимита JS/Bun heap или RSS на сессию;
- флага вида `disableBuiltinMcpPresets`;
- отдельных глобальных switch'ей для `workspace`, Preview/Browser или «visualization» как пресета;
- `startOnFirstToolCall` в конфиге Claude Code;
- обещания, что deny-tool или `disableMobileSimulatorTools` **физически выгружает** реализацию из адресного пространства;
- разбивки базовых ~340 МБ по вендорам.

Единственное подтверждение runtime lazy loading — native image processor, который Anthropic отдельно заявил как загружаемый при первом использовании.

## 3. ⚠️ Поправка к нашей же методике замера

`24 × RSS` **завышает** реальное физическое потребление: RSS каждого процесса включает разделяемые отображения бинаря и библиотек. Мерить надо `phys_footprint`, private/dirty pages и дельту system memory pressure. Это касается proc-patrol-robot — наши цифры «10.9 ГБ резидентных коннекторов» посчитаны суммой RSS и, вероятно, завышены.

## 4. Что это меняет — готовый A/B на нашей машине

Отчёт даёт прямой протокол вместо гадания (одинаковый короткий промпт, потом `phys_footprint` + private dirty + system-wide delta):

1. чистый процесс `--safe-mode`
2. чистый `--bare`
3. обычный, все default-on builtins выключены через `/mcp`
4. обычный с текущим профилем

**Читается так:** если `safe-mode ≈ normal` — масса в ядре сессии, а не в MCP-пресетах, и диета коннекторов ничего не даст. Если `bare` заметно легче — возвращать подсистемы по одной.

## 5. Демон и gateway

- **Streamable HTTP — текущий стандартный транспорт MCP; старый HTTP+SSE deprecated** (оставлен для совместимости). Наш Telegram-SSE-демон архитектурно прав против 48 stdio-процессов, но при следующем плановом обновлении стоит дать Streamable HTTP endpoint, сохранив SSE для старых клиентов.
- Shared HTTP уместен там, где сервер держит дорогой connection pool / один внешний аккаунт (Telegram, WhatsApp, browser farm, БД) и обслуживает много агентов.
- stdio остаётся разумным для лёгких stateless-инструментов и недоверенного кода, который лучше убивать вместе с клиентом.
- **Docker MCP Gateway** — самое прямое готовое воплощение «один порт + запуск backend-контейнера по первому tool call». Microsoft MCP Gateway — кластерный, избыточен для одного Mac. IBM ContextForge — серьёзный кандидат, но ландшафт молод.
- ⭐ Ключевое: «ленивый старт по первому вызову» — это **lifecycle policy gateway'я, а не фича Claude Code**. Хотим lazy — берём gateway.

## 6. Потребитель

Живая сессия ON AIR `connectors Mac` («MCP lazy-load: убрать 10.9 ГБ резидентных коннекторов»). Ей отсюда нужны три вещи: (1) `computer-use` default-off — проверить `enabledMcpServers`; (2) `disableClaudeAiConnectors`; (3) прогнать A/B из §4 **до** любой стройки, потому что гипотеза «пресеты едят RAM» пока `speculative`, а не измерена.

## Провенанс

- `_originals/deep-research/DR26-08-01-MACANTON-02-726-chatgpt.md` — codex exec, 11 уник. URL, 15 веб-поисков.
- `_originals/deep-research/DR26-08-01-MACANTON-02-726-grok.md` — grok -p, 13 уник. URL. ⭐ Прогон в **изолированной cwd** — Grok снова написал файл (`REPORT.md`), но в изоляторе, чужого не тронул. Фикс сработал.
- Рельса gemini снята через `retarget --drop`.

Связано: proc-patrol-robot · dr-headless-rails-codex-grok
