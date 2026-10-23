# DR26-08-01-MACANTON-02-726 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-726
rail: grok (grok -p (Grok Build CLI, подписка Heavy))
session_uuid: 019fbdb9-grok-726
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:34:30Z
proof_of_work: 13 уник. URL, web_fetch веб-поисков — гейт НЕ ПРОЙДЕН (<15 уник. URL)
note: изолированная cwd СРАБОТАЛА: Grok снова написал файл (REPORT.md), но в изоляторе, чужого не тронул. 13 уник. URL — ниже гейта 15
---

# DR26-08-01-MACANTON-02-726

# Deep Research: диета MCP-коннекторов и сессий Claude Code на мульти-сессионной машине

**Дата:** 2026-08-01  
**Машина-контекст:** macOS, Claude Code 2.1.219, 32 ГБ RAM, 15–36 сессий, пик swap 94%  
**Статус доказанного до DR:** telegram→SSE-демон ✅ · whatsapp thin-client ✅ · computer-use = builtin (0 внешних процессов) ✅ · idle>5ч kill ✅ · RSS бинаря ~340 МБ × N = главный потребитель

---

## Executive summary

1. **~340 МБ/сессия** — это в основном сам Node/Electron-подобный рантайм Claude Code (V8 heap + системный промпт + tool registry + история), а не отдельный «MCP-процесс пресета». Вендор **не публикует** ручку «уменьшить RSS бинаря на 50%».
2. **Вендорские ручки, которые реально есть:**
   - `disabledMcpServers` / `enabledMcpServers` в «внутренний путь лаборатории» (per-project) — в т.ч. built-in (`computer-use` default-off, opt-in).
   - `/mcp disable`, `--disallowedTools "mcp__*"`, `--strict-mcp-config`, `--bare` / `CLAUDE_CODE_SIMPLE=1`, `CLAUDE_CODE_SAFE_MODE=1`.
   - ToolSearch (`ENABLE_TOOL_SEARCH`) — **только схемы в контексте**, не процессный runtime.
3. **Shared-daemon (SSE / Streamable HTTP)** — **канон MCP-спеки** для multi-client. Stdio = 1 процесс на 1 сессию by design. Зрелые прокси: `sparfenyuk/mcp-proxy` (transport bridge), `metatool-ai/metamcp` (aggregator), community: harshal-mcp-proxy / punt-labs (daemon+lazy) — maturity разная.
4. **Официального потолка сессий нет.** Практический soft-cap на 32 ГБ ≈ **8–12 «толстых» интерактивных** или **15–20** с жёсткой диетой + idle-reap; 24×340 МБ = 8.2 ГБ — уже в зоне thrash.
5. **Топ-3 хода для MacAnton:** (A) audit+disable unused MCP + confirm computer-use off · (B) session budget+idle уже сделан, ужесточить · (C) не строить mega-proxy — добить stdio→HTTP только у fat-серверов.

---

## Q1. Диета одной сессии: ~340 МБ и вендорские ручки

### Что определяет RSS бинаря (~340 МБ)

| Компонент | Вклад (порядок) | Документировано? |
|---|---|---|
| Сам бинарь Claude Code (V8/Node runtime, JS heap, native bindings) | основной пол | **нет** vendor breakdown RSS |
| Системный промпт + tool registry (builtin tools) | значимый, но скорее tokens, не 100+ МБ | partial (tool search docs) |
| История сессии / terminal output cache | растёт со временем | **да** — changelog: «Reduced long-session memory growth from terminal output cache» |
| Auto-memory / CLAUDE.md hierarchy | context + disk, не главный RSS | settings: `autoMemoryEnabled` |
| Stdio MCP child processes | **отдельно** от RSS claude (как telegram 350–380 МБ) | **да** — stdio = spawn |
| Builtin MCP presets (`computer-use`, `claude-in-chrome`, `Claude Browser/Preview`, `workspace`) | **внутри** бинаря; отдельного `ps` нет (ваш замер) | **да** — reserved names |

**Вывод по вашему замеру:** 24 × ~340 МБ = 8.2 ГБ — это **N копий рантайма**, не «MCP-ферма». После перевода telegram на SSE главный рычаг — **число живых сессий**, не диета одного tool-схемы.

### Вендорские ручки (документировано)

#### A. Built-in MCP presets — **документировано, не фольклор**

Источник: [Connect Claude Code to tools via MCP](https://code.claude.com/docs/en/mcp)

- Reserved names: `workspace`, `claude-in-chrome`, `computer-use`, `Claude Preview`, `Claude Browser`.
- Per-project toggle в «внутренний путь лаборатории»:
  - **`disabledMcpServers`** — opt-out для default-on (user MCP, plugins, claude.ai connectors, built-ins that default on).
  - **`enabledMcpServers`** — opt-in для default-**off**, **явно `computer-use`**.
- UI: `/mcp` → disable без удаления конфига.
- **Важно:** `enabledMcpServers` / `disabledMcpServers` ≠ `enabledMcpjsonServers` / `disabledMcpjsonServers` (последние — approval project `.mcp.json`).

**Практика:** `computer-use` по умолчанию **выключен** (opt-in). Если не добавляли в `enabledMcpServers` — он не должен поднимать tool surface. `claude-in-chrome` / browser presets — смотреть `/mcp` и `disabledMcpServers`.

**Уверенность:** **established** (vendor docs, v2.1.x).

#### B. User/project MCP servers

| Механизм | Scope | Эффект на RAM |
|---|---|---|
| `claude mcp remove` / `/mcp` disable | user/project | stdio child **не стартует** → экономия RSS child |
| HTTP/SSE/WS type вместо stdio | config | **0 child process** на сессию (thin client HTTP) |
| `alwaysLoad: true` | per-server | **хуже** для старта: блокирует, грузит схемы; не экономит RAM |
| `--strict-mcp-config --mcp-config file` | CLI | только перечисленные серверы |
| `--disallowedTools "mcp__*"` | CLI | убирает MCP tools из context; **не гарантирует** unspawn stdio (зависит от connect path) |
| `--bare` / `CLAUDE_CODE_SIMPLE=1` | CLI/env | no auto-discovery MCP/hooks/skills/CLAUDE.md; минимальный toolset |
| `--safe-mode` / `CLAUDE_CODE_SAFE_MODE=1` | CLI/env | troubleshooting: off plugins/MCP/hooks/memory |
| Managed: `allowedMcpServers` / `deniedMcpServers` | org policy | lockdown |

Источники: [MCP docs](https://code.claude.com/docs/en/mcp), [CLI reference](https://code.claude.com/docs/en/cli-reference), [env-vars](https://code.claude.com/docs/en/env-vars), [settings](https://code.claude.com/docs/en/settings).

#### C. Чего **нет** в документации (фольклор / gap)

| Claim | Статус |
|---|---|
| «Env `DISABLE_COMPUTER_USE=1` / `CLAUDE_CODE_NO_BROWSER`» | **не найдено** в env-vars reference → folklore unless private build |
| «Флаг уменьшения heap/RSS бинаря» | **не документирован** |
| «Per-session отключение iOS-simulator preset» | нет отдельного documented preset name; iOS sim, если есть, — через plugin/MCP, не reserved list выше |
| «ToolSearch выгружает builtin runtime из памяти» | **опровергается** логикой docs: deferred = **schemas in context**, не process |

**Уверенность по gap:** **established**, что docs не дают RAM-diet для самого бинаря.

### Практический «до → после» на данных MacAnton

| | До | После (достижимо вендором) |
|---|---|---|
| telegram stdio × N | ~350–380 МБ × сессия | 0 python (уже сделано) |
| whatsapp | ~8 МБ thin | без изменений |
| computer-use | 0 процессов (builtin) | confirm не в `enabledMcpServers` |
| бинарь × 24 | 8.2 ГБ | **только** N↓ или idle-kill (уже) |
| unused user MCP stdio | spawn на каждую сессию | `/mcp disable` + HTTP daemon |

---

## Q2. Ленивая загрузка тулов: схемы vs runtime

### Документировано

1. **Tool Search / deferred tools** ([tool-search](https://code.claude.com/docs/en/agent-sdk/tool-search), [MCP § Scale with tool search](https://code.claude.com/docs/en/mcp#scale-with-mcp-tool-search)):
   - Default: tool **definitions withheld** from context; names + server instructions at start; full schema on ToolSearch hit.
   - `ENABLE_TOOL_SEARCH`: unset/true/auto/auto:N/false.
   - `alwaysLoad: true` — **anti-lazy** for schemas; **blocks** startup connect.
   - `anthropic/alwaysLoad` in tool `_meta` — per-tool.

2. **MCP connection lifecycle** ([env-vars `MCP_CONNECTION_NONBLOCKING`](https://code.claude.com/docs/en/env-vars)):
   - С v2.1.142: **non-blocking by default** — servers connect **in background** after prompt available.
   - Это **не** «не поднимать процесс до первого tool call».
   - Stdio: Claude Code **spawns** server as part of connect (background, но spawn всё равно происходит).
   - HTTP/SSE: client opens connection to **already-running** (or remote) server — no per-session fat process.

3. **Feature request** [Lazy context loading #44536](https://github.com/anthropics/claude-code/issues/44536): extend ToolSearch pattern to more context components — signal that **runtime process lazy-start is NOT done** for stdio.

4. **Community / third-party** (harshal-mcp-proxy, IONOS selective loading): process-level lazy start exists **outside** Claude Code — catalog snapshot + spawn-on-first-invoke. Это **не** vendor feature Claude Code.

### Ответ на вопрос «откладывается ли runtime пресета?»

| Слой | Откладывается? | Доказательство |
|---|---|---|
| Tool **schemas** in model context | **Да** (ToolSearch default) | vendor docs |
| MCP **stdio process** start | **Нет** (connect всё равно; non-blocking ≠ no-spawn) | MCP_CONNECTION_NONBLOCKING + lifecycle docs |
| **Builtin** preset runtime inside binary | **Нет evidence** of unload; tools may be deferred in context only | no docs; your `ps` = 0 external processes anyway |
| HTTP shared daemon upstream | **Может** (если daemon lazy) — вне Claude Code | community proxies |

**Уверенность:** schemas deferred = **established**; process-lazy в Claude Code = **established NO** (docs describe connect-at-session, not invoke-at-first-call); third-party process-lazy = **emerging**.

---

## Q3. Паттерн «общий демон» как норма + supervisors

### Что говорит промышленный стандарт (MCP spec)

[Streamable HTTP transport](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/streamable-http):

> «the server operates as an **independent process that can handle multiple client connections**.»

- **HTTP (Streamable HTTP)** = канон remote/multi-client (POST + optional SSE per-request).
- **SSE (старый HTTP+SSE)** = **deprecated** since 2025-03-26; Claude Code still supports, docs: «use HTTP where available».
- **stdio** = 1:1 process-to-client; ideal for local single-host tools needing FS/OS access — **не** для multi-session fat clients.

Claude Code docs: «HTTP servers are the **recommended** option for connecting to remote MCP servers.»

**Классы, где shared-process = норма:**

| Класс | Почему shared | Пример |
|---|---|---|
| Chat/IM bridges (Telegram, Discord) | auth session, sqlite session file, rate limits, one network identity | ваш telegram SSE |
| DB / SaaS API MCP | connection pool, OAuth token, rate limit | GitHub remote HTTP, Sentry |
| Heavy local runtimes (embeddings, browsers, models) | RAM duplication catastrophic | playwright, neo4j, embedding servers |
| Filesystem-bound, project-scoped, no shared state | stdio OK / even preferred isolation | simple script MCP |

**Классы, где stdio-per-session ещё оправдан:**

- Stateless tiny tools (~8 МБ, как ваш whatsapp thin wrapper если already thin).
- Need per-session cwd / `CLAUDE_PROJECT_DIR` isolation without multi-tenant server design.
- Security: untrusted project `.mcp.json` shouldn't share process with secrets of other projects.

### Готовые multiplexers / proxies (зрелость)

| Проект | Роль | Зрелость | Заметка |
|---|---|---|---|
| **MCP Streamable HTTP** (spec) | multi-client server transport | **established** | строить/держать сервер на HTTP, не invent protocol |
| **sparfenyuk/mcp-proxy** | stdio↔SSE/StreamableHTTP bridge | **emerging→established** community | transport only, not aggregator |
| **metatool-ai/metamcp** | aggregator + middleware + namespaces | **emerging** (active OSS) | Docker gateway; groups servers |
| **VeriTeknik/pluggedin-mcp-proxy** | hub one-connection-all-MCPs | **emerging** | product+OSS |
| **HarshalRathore/harshal-mcp-proxy** | shared daemon + schema deferral + lazy spawn + idle | **emerging** (small repo) | claims ~2.7 GB RAM save; **не** industrial standard |
| **punt-labs/mcp-proxy** | Go thin stdio → shared daemon WS | **emerging** | process isolation boundary |
| **mcp-funnel** (chris-schra) | filter + discovery toolsets | **emerging** | context, not RAM multi-session |
| **Higress mcp.higress.ai** | enterprise AI gateway | **emerging/enterprise** | Streamable HTTP market |

**Принцип AK-47 / mature standard:**  
**Не** invent «свой MCP-мультиплексор». Дефолт:

1. Fat MCP → **один** HTTP/SSE daemon (launchd) + клиенты `type: http`.
2. Нужен bridge stdio-only client → `mcp-proxy` transport.
3. Нужен aggregator многих серверов в один endpoint → MetaMCP / plugged.in **только если** боль = «слишком много server entries», не «один fat process».

**Уверенность:** multi-client HTTP = **established** (spec); specific proxies = **emerging**; custom protocol = **anti-pattern**.

---

## Q4. Пределы: сколько сессий разумно

### Документировано вендором

- **Нет** официального «max concurrent Claude Code sessions per machine».
- Есть:
  - memory-pressure reap background shells (macOS/Linux, idle 30 min) — `CLAUDE_CODE_DISABLE_BG_SHELL_PRESSURE_REAP`.
  - long-session memory growth fixes (terminal output cache).
  - headless/`-p`/`--bare` for scripted agents (orchestration path).
  - subagents / background tasks (in-session), not multi-process fleet API.
  - Desktop app + CLI can coexist; no published concurrency SLA.

### Практика сообщества + ваш замер

| Режим | Оценка на 32 ГБ | Логика |
|---|---|---|
| «Толстые» interactive (полный CLAUDE.md, hooks, MCP) | **soft cap ~8–12** | 12×340 МБ ≈ 4 ГБ + OS + Desktop + browsers + swap headroom |
| С диетой MCP + idle reap | **~15–20** | ваш 15–36: верх уже thrash (load 61, swap 94%) |
| 24+ simultaneous | **доказанный thrash** | 8.2 ГБ только claude binaries |
| Headless `-p --bare` burst | выше, но **короткоживущие** | не держать 36 idle interactive |

### Оркестрация вместо толпы interactive (vendor-aligned)

1. **Subagents** внутри одной сессии (background by default v2.1.198+) — делят parent process context cost differently (not N full binaries).
2. **`claude -p` / Agent SDK** + queue (Task Scheduler / launchd) — workers spawn, finish, die.
3. **`--bare` / strict MCP** for routine workers.
4. **Один Desktop** + мало long-lived CLI, остальное ephemeral.

**Уверенность:** no official number = **established absence**; soft-cap 8–12 thick / 15–20 dieted on 32 ГБ = **emerging** (physics + your metrics + community thrash reports). 36 = **speculative** only as «possible with aggressive kill», not healthy.

---

## Q5. Анти-паттерны shared daemon

### Известные грабли класса (ваш telegram + peers)

| Анти-паттерн | Симптом | Класс |
|---|---|---|
| **N Telethon/clients × 1 sqlite session** | `database is locked`, session corruption, `AuthKeyDuplicatedError` | state-on-disk not multi-process safe |
| **N network sessions × 1 account** | Telegram видит толпу devices; flood/ban risk | identity/rate-limit |
| **Shared OAuth token without refresh lock** | thundering herd refresh → revoke | auth races |
| **Shared writeable workspace / git** | concurrent edits, clobber, lock fights | isolation |
| **Stdio server assumed multi-tenant after HTTP wrap** | global mutable vars, cwd, temp files collide | process-state |
| **One browser profile / CDP many agents** | flaky clicks, cookie races, ban IP | external side-effect |
| **Daemon dies → all sessions blind** | single point of failure | availability |
| **No Origin bind / 0.0.0.0** | DNS rebinding, local tool RCE from web | MCP HTTP security (spec MUST validate Origin, bind localhost) |
| **Mixing project trust boundaries** | untrusted repo MCP hits privileged daemon | security |
| **SIGINT / session restart kills shared? (wrong layer)** | if still stdio-attached wrongly | [MCP session manager posts](https://dev.to/daichikudo/fixing-claude-codes-sigint-problem-how-i-built-mcp-session-manager-3nke) |
| **Lazy catalog stale** | tools/list wrong after upstream update | ops |
| **Per-session secrets in shared process env** | cross-session leak | multi-tenant design |

### Telegram-specific (документировано у chigwell telegram-mcp + Telethon)

- Session pool / multiple session strings for multi-client same account.
- Telegram forbids one auth key from two IPs simultaneously.
- SQLite session file is **single-writer** class problem.

### Правила безопасного shared daemon

1. **Один writer** к session/auth store; clients stateless HTTP.
2. **Очередь/mutex** на side-effect tools (send message, click, write file).
3. **localhost + Origin check + auth** (MCP spec security).
4. **Health + restart** (launchd KeepAlive) + client reconnect (Claude Code has backoff for HTTP/SSE).
5. **Не** шарить daemon между untrusted projects без ACL.
6. **Idempotency** tool calls where possible.

**Уверенность:** sqlite/session races = **established** (Telethon issues, SO); MCP HTTP security rules = **established** (spec); multi-tenant MCP design maturity = **emerging**.

---

## Топ-3 хода для MacAnton (цена / риск)

### 1) Audit built-in + user MCP: disable everything not needed (рекомендуется первым)

**Что сделать**
- `claude mcp list` + в живой сессии `/mcp`.
- Убедиться: `computer-use` **не** в `enabledMcpServers`.
- Все default-on unused → `disabledMcpServers` или `/mcp disable`.
- Оставшиеся fat stdio → уже HTTP (telegram done) или disable на worker-only projects.
- Для рутинных worker-сессий: `--strict-mcp-config` минимальный json **или** `--disallowedTools "mcp__*"` + bare где допустимо.

**Цена:** 30–90 мин аудита; 0$ infra.  
**Выигрыш:** убирает **child** RSS и connect storms; на бинарь 340 МБ почти не влияет.  
**Риск:** low — toggle обратимый; можно сломать workflow если disable нужный server (лечится `/mcp` enable).  
**Уверенность эффекта на 8.2 ГБ бинарей:** **low–med** (косвенно); на «шум» connect/CPU: **high**.

### 2) Жёсткий session budget + idle (главный рычаг RAM)

**Что сделать**
- Soft cap: **≤12** long-lived interactive на Mac16; остальное — `-p`/ephemeral или другая машина (хаб).
- Idle reap: уже >5ч <10% CPU — ужесточить до **2–3ч** idle или «нет human input».
- Не держать 24 «на всякий»: resume из transcript дешевле, чем 8 ГБ swap.
- Desktop app: 1 instance; не плодить дубли CLI+Desktop на одну задачу.

**Цена:** process/ops discipline; возможная боль «сессию убили».  
**Выигрыш:** **линейный** — каждая убитая сессия ≈ 340 МБ + children. 24→12 ≈ **−4 ГБ**.  
**Риск:** med — потеря mid-flight context (mitigation: transcript resume, handoff notes).  
**Уверенность:** **established** (арифметика + ваш thrash замер).

### 3) Не строить mega-proxy; добить stdio→HTTP только fat servers + launchd

**Что сделать**
- Инвентарь: `ps` все MCP children по command line; rank by RSS.
- Только top fat (если ещё есть кроме cured telegram) → Streamable HTTP/SSE daemon + `type: http` в конфиге.
- **Не** внедрять harshal-mcp-proxy/MetaMCP «на всех» без боли: у вас ToolSearch уже режет **context**; боль = **process RSS**, лечится per-server daemon (как telegram/whatsapp pattern).
- Supervisor: launchd (macOS native) > custom multiplexer protocol.

**Цена:** 0.5–2 дня на каждый fat server.  
**Выигрыш:** N×RSS_server → 1×RSS_server (как telegram: 48×370 МБ → 0).  
**Риск:** med — auth/state races (см. Q5); нужен healthcheck; regression на tool names.  
**Уверенность подхода:** **established** (MCP HTTP multi-client); specific product choice: prefer **minimal** custom daemon over young mega-proxy = **emerging** preference aligned AK-47.

---

## Сводка уверенностей

| Вопрос | Ключевой вердикт | Уверенность |
|---|---|---|
| Q1 RSS 340 МБ | runtime бинаря; нет vendor RAM-slider | established |
| Q1 disable presets | `disabledMcpServers` / `enabledMcpServers` / `/mcp` — real | established |
| Q2 ToolSearch | schemas only, not process | established |
| Q2 process lazy-start in CC | нет | established |
| Q3 shared HTTP daemon | MCP spec norm for multi-client | established |
| Q3 which proxy | transport bridge mature-ish; aggregators emerging | emerging |
| Q4 session cap | no official; 8–12 thick / 15–20 dieted on 32GB | emerging |
| Q5 shared daemon footguns | real, esp. session files & identity | established |

---

## Источники (primary)

1. https://code.claude.com/docs/en/mcp — transports, built-ins, disable lists, tool search, alwaysLoad  
2. https://code.claude.com/docs/en/agent-sdk/tool-search — deferred definitions  
3. https://code.claude.com/docs/en/env-vars — ENABLE_TOOL_SEARCH, MCP_CONNECTION_NONBLOCKING, SAFE_MODE, SIMPLE/bare  
4. https://code.claude.com/docs/en/cli-reference — --mcp-config, --strict-mcp-config, --disallowedTools, --safe-mode  
5. https://code.claude.com/docs/en/settings — allowed/denied MCP managed keys  
6. https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/streamable-http — multi-client independent process  
7. https://github.com/anthropics/claude-code/issues/44536 — lazy context still feature request beyond tools  
8. https://github.com/sparfenyuk/mcp-proxy · https://github.com/metatool-ai/metamcp · https://github.com/Harshalrathore/harshal-mcp-proxy  
9. Telethon multi-session: https://github.com/LonamiWebs/Telethon/issues/146 · telegram-mcp session pool notes  
10. Arxiv agent systems notes on Claude Code deferred tools / context pipeline (secondary)

---

## Что НЕ делать (явный no-go)

1. Писать свой «MCP bus protocol» поверх JSON-RPC — spec уже есть.  
2. Ждать, что ToolSearch снизит RSS с 340→100 МБ — **не тот слой**.  
3. 36 interactive sessions на 32 ГБ «потому что можно» — swap 94% = доказанный fail.  
4. N stdio Telethon/sqlite — уже пройдено; не повторять на других stateful MCP.

---

*Конец отчёта DR26-08-01-MACANTON-02-726*
