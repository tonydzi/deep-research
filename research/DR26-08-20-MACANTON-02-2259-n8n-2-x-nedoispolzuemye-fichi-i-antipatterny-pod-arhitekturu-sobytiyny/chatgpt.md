# DR26-08-20-MACANTON-02-2259 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

20.08.2026, 23:08–23:31 PT

Главный вывод: n8n вам полезнее всего не как более мощный LLM-конструктор, а как надёжная облачная приёмная, долговечная очередь и журнал состояний. Максимальную отдачу дадут пять вещей:

1. Починить контроль публикации/активации и zombie schedules.
2. Перенести короткоживущее состояние из workflow static data в Data Tables/Postgres.
3. Сделать настоящую очередь cloud→local с lease/idempotency/DLQ.
4. Перестроить subscription-LLM sidecar; queue mode n8n эту проблему не лечит.
5. Ввести внешний аудит тишины, конфигурации и клиент-идентичных probes.

Ваш вывод «REST PUT безопаснее MCP `removeNode` для orphan-heavy workflows» сейчас верен практически, но не как общее свойство REST: REST также опасен без post-write проверки. Python Code → JavaScript — разумная временная политика; доказанной универсальной причины различия `_input` я не нашёл.

## 1. Ранжированная карта

| # | Кандидат | Какая ваша боль снимается | Отдача | Цена | Риск | Минимальная версия | Вердикт |
|---:|---|---|---|---|---|---|---|
| 1 | Deployment guard: snapshot → PUT → read-back → publish-state check → live canary | MCP-деактивация, незаметно выключенные workflow, model drift | Очень высокая | 1–2 дня | Низкий | 2.14.2 | **Внедрить сейчас** |
| 2 | Внешний trigger/silence auditor | Zombie, zero-execution webhook, WAF 403, stopped schedule | Очень высокая | 2–3 дня | Низкий | Любая 2.x | **Внедрить сейчас** |
| 3 | Data Tables для heartbeat/dedup/idempotency и DLQ | static-data flush lag, нет видимого состояния | Высокая | 1–2 дня | Средний | CRUD развит в 2.3; у вас 2.14.2 достаточно | **Внедрить сейчас** |
| 4 | Durable cloud→local queue в Data Tables или Postgres | Спящие машины, NAT, catch-up, poison jobs | Очень высокая | 3–5 дней | Средний | 2.14.2 | **Внедрить сейчас** |
| 5 | Асинхронный subscription-LLM gateway | $216/15 дней, зависание одного CLI блокирует всех | Очень высокая | 3–6 дней | Высокий: ToS/quota | n8n 2.14.2; sidecar независим | **Пилот** |
| 6 | Retention: возраст + count + внешняя агрегация | Потеря 30–90-дневной форензики | Высокая | 0.5–2 дня | Рост Postgres | 2.14.2 | **Сейчас** |
| 7 | Upgrade 2.14.2 → ≥2.27.x после staging | Schedule duplicate bugs; новые MCP controls | Высокая | 1–3 дня | Регрессии | ≥2.27 для двух schedule-защит | **Пилот немедленно** |
| 8 | Prometheus + readiness + Postgres/Redis metrics | Instance-wide outage, saturation | Средняя/высокая | 1–2 дня | Низкий | 2.x | **Сейчас** |
| 9 | Eval smoke suite | Тихая деградация agent/RAG/model aliases | Средняя | 1–2 дня | LLM cost | Light eval — проверить на staging | **Пилот** |
| 10 | Queue mode n8n | Изоляция долгих execution, горизонтальные workers | Низкая при нынешнем объёме | 2–4 дня + Redis | Выше ops burden | 2.x | **Позже** |
| 11 | Instance-level MCP | Ограниченный запуск опубликованных workflows | Низкая/средняя | 1 день | Большая control-plane поверхность | У вас есть; granular scopes улучшены в 2.32 | **Read-only пилот после upgrade** |
| 12 | Built-in Git environments | Dev→prod promotion | Средняя | 2–5 дней + лицензия | Конфликт с API edits | Business/Enterprise | **Позже** |
| 13 | External Secrets | Централизация секретов | Для вас отрицательная | 2–4 дня + лицензия | Секреты уйдут в облачный контур | Enterprise | **Не внедрять** |
| 14 | MTProto community nodes | Telegram user-account access | Возможная | 2–5 дней | Supply-chain + session takeover | Зависит от пакета | **Отдельный sandbox-пилот** |

## 2. Топ-5 подробно

### 2.1 Deployment guard и неизменность active/published состояния

До: скрипт делает PUT или MCP mutation; HTTP 200 считается успехом.

После: изменение считается принятым только когда read-back, версия, active/published состояние и canary совпали с намерением.

Протокол:

1. `GET /api/v1/workflows/{id}`.
2. Сохранить полный snapshot и отдельно canonical deployment body.
3. Зафиксировать:
   - `active`;
   - published/active version, если поле присутствует в вашей API-схеме;
   - node count;
   - hash нормализованных `nodes/connections/settings`;
   - trigger inventory;
   - model inventory;
   - credential-reference inventory.
4. Для PUT передавать только `name`, `nodes`, `connections`, `settings`.
5. `settings` пересобрать по whitelist вашей конкретной 2.14.2, не переносить неизвестные ключи.
6. Сразу повторить GET и сравнить canonical hash.
7. Отдельно проверить active/published состояние. Не предполагать, что PUT сохраняет активацию.
8. Для production-trigger workflow выполнить синтетическое событие тем же клиентом, который будет работать в production, с browser-like UA.
9. Для schedule — ждать ближайшее контрольное окно либо иметь отдельный временный canary webhook.
10. При любом расхождении вернуть snapshot и снова проверить фактическое состояние.

Нельзя гарантировать сохранение `active`, просто не посылая его в PUT: у n8n saved draft, published version и active registration могут быть разными слоями.

Отдельный guard:

```text
expected_active=true
before_active=true
PUT
GET
if after_active != expected_active:
    abort + alert + rollback
if canonical_hash != proposed_hash:
    abort
if trigger_canary not observed:
    abort
```

Для MCP:

- `removeNode` официально удаляет связи удалённого узла; связанные AI subnodes могут остаться disconnected.
- Новые MCP validation tools различают warnings/errors, но текущие docs не подтверждают, что workflow с любым disconnected node обязан быть несохраняемым.
- Ваш строгий отказ, вероятнее всего, относится к community `n8n-mcp` или его validation policy, а не к n8n core.
- Инцидент с `active=false` также нельзя уверенно приписать core `removeNode`: опубликованный MCP tools reference не обещает менять active. Это либо wrapper behaviour, либо отдельный publish/update effect.

Поэтому ваш практический вывод сохраняется: raw REST PUT под вашим guard безопаснее community MCP mutation для уже существующих, orphan-heavy production workflows. Официальный instance MCP тоже пока не заслужил write-доступ.

Rollback: PUT сохранённого canonical body, восстановление ожидаемого publish/active state, restart main process при доказанном stale trigger registration.

Метрика:

- `deployment_state_mismatch_total = 0`;
- `active_workflow_unplanned_minutes = 0`;
- `production_edits_with_canary / production_edits = 100%`.

Harvest: через 30 дней или после 20 production edits, что наступит раньше.

Источники: официальный MCP tools reference описывает семантику `removeNode` и обязательную validation стадию; workflow-level validation отдельно проверяет наличие trigger-like node, а не отсутствие всех disconnected nodes. [MCP tools reference](https://github.com/n8n-io/n8n-docs/blob/main/docs/connect/connect-to-n8n-mcp-server/mcp-server-tools-reference.md), [workflow validation source](https://github.com/n8n-io/n8n/blob/master/packages/workflow/src/workflow-validation.ts).

### 2.2 Zombie schedules: причина, версия и детектор

Наиболее доказанный корень класса: активные triggers находятся в памяти main n8n process (`ActiveWorkflowManager`). Изменение DB из другого процесса — особенно `n8n import:workflow` внутри работающего контейнера — меняет `workflow_entity.active`, но не удаляет уже зарегистрированный trigger из памяти сервера. Поэтому UI/API показывают inactive, а старый trigger продолжает исполняться. Restart очищает регистрацию.

Для Schedule Trigger есть две последующие защиты:

- deterministic cron fix — сообщается для 2.19.1;
- execution-level duplicate protection — сообщается для 2.27.0.

Ваш 2.14.2 старше обеих. Однако polling-trigger вариант всё ещё воспроизводился на 2.32.6 после CLI import. Значит upgrade уменьшит schedule-risk, но не закрывает весь класс stale polling registration. Это community reproduction, хотя с конкретным сценарием и версиями. [Issue #35251](https://github.com/n8n-io/n8n/issues/35251).

Что можно определить через public API:

- `active=false` + новые `mode:"trigger"` executions после времени деактивации = доказанный zombie;
- один scheduled workflow + два execution в одном cron-window, особенно с разными workflow version IDs = duplicate/stale registration;
- активный schedule без выполнения после `expected_at + grace` = missing registration;
- workflow active/published, но source webhook reports wrong URL/pending errors = registration drift.

Чего API не даёт: authoritative список фактически зарегистрированных in-memory cron/poll triggers. Поэтому «workflow inactive и сейчас точно не зарегистрирован» одним GET доказать нельзя.

Детерминированный внешний аудитор:

```text
inventory:
  workflow_id
  trigger_node_id
  trigger_kind
  expected_schedule/timezone
  expected_active
  max_silence
  duplicate_window
  canary_method

every 5 minutes externally:
  GET workflows
  GET executions incrementally
  compare intended active vs actual active
  flag inactive + fresh trigger execution
  flag >1 execution in one schedule window
  flag active + overdue
```

Для inactive workflow сохраняйте `inactive_since` сами: публичная модель может не давать точный момент деактивации.

Mitigation:

- не применять CLI import к живой базе из отдельного процесса;
- для API updates использовать normal server API;
- после изменения trigger topology делать deactivate → проверить тишину → activate → canary;
- на 2.14.2 после подозрения на stale registration — controlled restart main n8n process является более сильным reset, чем toggle;
- upgrade сначала в staging как минимум до ≥2.27.x, предпочтительно до последнего patched 2.x после чтения release notes.

Do NOT do this if: вы собираетесь автоматически restart-ить n8n только по одному пропущенному tick. Сначала нужны два независимых сигнала: DB/API state и execution/time-window evidence.

Метрики:

- `zombie_trigger_executions_total = 0`;
- `duplicate_schedule_windows_total = 0`;
- `overdue_active_triggers < 1%`;
- detection latency <10 минут.

Harvest: 14 дней, потому что у вас retention сейчас примерно 10 дней.

### 2.3 Data Tables вместо global static data

`$getWorkflowStaticData()` не является транзакционной меж-execution очередью. Ваш measured flush lag — достаточная причина убрать оттуда всё, где write-after-read должен быть немедленно виден.

Data Tables подходят для:

- heartbeat sources;
- idempotency keys;
- recent-alert dedup;
- job state;
- poison/dead-letter rows;
- trigger last-seen registry;
- deployment receipts.

У вас 2.14.2 уже после Data Table CRUD additions в 2.3. MCP data-table tools появились только в 2.16, но это не мешает Data Table node/API в 2.14.2. В 2.14.1 также закрыли SQL injection в Data Table node, поэтому 2.14.2 включает этот security fix. [2.x release notes](https://github.com/n8n-io/n8n-docs/blob/main/docs/changelog/release-notes.md), [security advisory](https://github.com/n8n-io/n8n/security/advisories/GHSA-98c2-4cr3-4jc3).

Схема watchdog:

```text
heartbeats
- source_key string
- every_min number
- last_seen_at date
- last_idempotency_key string
- status string
- updated_by string
```

Схема job queue:

```text
jobs
- job_key string
- type string
- state string        # pending/claimed/ready/delivered/dead
- payload_ref string  # не секрет и не vault payload
- target string
- attempts number
- available_at date
- lease_until date
- claimed_by string
- result_ref string
- last_error string
```

Критичная оговорка: необходимо проверить на вашей версии, поддерживает ли Data Table node атомарный conditional update/upsert, достаточный для lease. Если нельзя сделать «claim only where state=pending and lease expired» атомарно, Data Tables годятся для heartbeat и видимого журнала, но не для конкурентной очереди. Тогда очередь должна жить в вашем Postgres с `SELECT … FOR UPDATE SKIP LOCKED` или atomic `UPDATE … RETURNING`.

Do NOT do this if: ожидаются миллионы строк, сложные транзакции, несколько конкурентных claimers или строгая referential integrity. Используйте Postgres.

Метрики:

- static-data write/read miss rate: было >0, цель 0;
- duplicate delivery rate <0.1%;
- jobs stuck beyond lease = 0;
- DLQ age p95 <24 часа.

Harvest: 14 дней.

### 2.4 Cloud n8n как EARS + outbound-only local queue

Ваш уже работающий Telegram→wake listener доказывает transport, но нынешний прямой HTTP push не должен быть единственным носителем задания. Wake — подсказка; durable queue — истина.

Рекомендуемая цепь:

```text
Telegram/GitHub/webhook
  → validate source/signature
  → derive idempotency key
  → INSERT pending job
  → optional wake hint over tailnet
  → return 2xx quickly

local poller
  → claim job with lease
  → local vault/GPU/CLI work
  → POST result receipt outbound
  → state ready/delivered
```

Если hub спит, job остаётся pending. После пробуждения:

- забирает ограниченный batch;
- сортирует по priority/created_at;
- не запускает всё разом;
- применяет per-type concurrency;
- poison job после N попыток уходит в `dead`, а не блокирует очередь;
- `job_key` делает повторное событие безопасным.

Tailnet direct call:

- допустим как низколатентный wake hint;
- не открывает публичный порт;
- но это всё равно inbound service внутри private mesh;
- компрометация n8n VPS даёт путь к listener, поэтому ACL должна разрешать VPS→только конкретный port/path;
- listener принимает opaque job ID, не prompt, секрет или vault content.

Outbound polling:

- сильнее соответствует вашему zero-inbound правилу;
- проще firewall;
- переживает сон;
- задержка равна poll interval;
- частые пустые poll ticks снова создают ваш старый анти-паттерн. Используйте wake hint + редкий backstop poll.

Telegram Trigger:

- официальный node использует Bot API webhook, не polling и не MTProto;
- Telegram хранит недоставленные updates не более 24 часов;
- при non-2xx Telegram повторяет доставку;
- `update_id` позволяет dedup и восстановление порядка;
- `getWebhookInfo.pending_update_count` и last error должны мониториться;
- один bot допускает только один active webhook;
- в актуальном node type 1.3 `setWebhook` отправляет `drop_pending_updates=true`. Это особенно опасно при redeploy: регистрация webhook может намеренно выбросить накопившиеся сообщения. Нужно проверить, какая node version записана в ваших workflows, а не только версию n8n. [Telegram Trigger source](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/nodes/Telegram/TelegramTrigger.node.ts), [Telegram Bot API](https://core.telegram.org/bots/api).

Отсюда прямой ответ: missed window восстанавливается только пока Telegram хранит pending updates, максимум 24 часа; redeploy node v1.3 может сам запросить их удаление. Для важных событий Telegram update сначала записывается в durable inbox.

Schedule Trigger:

- не делает гарантированный catch-up всех пропущенных cron occurrences после downtime;
- проектируйте schedule как «проверь, есть ли незакрытая работа за период», а не «каждый tick уникален и обязательно случится»;
- timezone задавайте явно в workflow `settings.timezone`; DST-тестируйте на staging;
- jobs должны иметь business-period key, например `daily-summary:2026-08-20`, чтобы повторный или поздний запуск был безопасен.

Webhook zero executions:

- GitHub: проверить webhook в repo settings, active flag, event selection, recent deliveries, response codes, signature secret;
- n8n: workflow active/published, production URL, webhook node/path;
- synthetic event: GitHub ping/redelivery либо controlled PR event;
- alarm на отсутствие событий строится внешним clock-based auditor;
- для редких природных событий нужен регулярный synthetic canary, иначе «тишина» неотличима от отсутствия реальных событий.

Wait node:

- waiting executions не prune-ятся;
- resume webhook живёт в n8n DB и должен переживать обычный restart;
- это хороший human-in-loop механизм для внутренних approval flows;
- всё равно задавайте timeout branch и idempotency key: повторное нажатие/повторный webhook не должно совершать действие дважды.

Do NOT do this if: событие содержит vault data, секрет или owner voice. В queue должна идти ссылка/opaque ID, обработка остаётся local.

Метрики:

- event→pending p95 <5 секунд;
- pending→claimed после пробуждения p95 <10 минут;
- lost acknowledged events = 0;
- duplicate side effects = 0;
- oldest pending age и DLQ depth видны ежедневно.

Harvest: 21 день.

### 2.5 Subscription-LLM gateway

Queue mode n8n не чинит ваш single-threaded `BaseHTTPServer`. Он лишь позволит нескольким n8n workers одновременно открыть ещё больше запросов к уже заблокированному gateway.

Правильная форма sidecar:

- async HTTP server: FastAPI/Starlette + uvicorn или aiohttp;
- одна request handler не выполняет CLI синхронно;
- spawn subprocess через async process API;
- bounded semaphore на provider/account;
- отдельный queue/worker pool для Claude, Codex, Gemini;
- hard timeout на subprocess;
- TERM → grace → KILL;
- stdout/stderr size limits;
- structured JSON response;
- request ID/idempotency key;
- `/healthz`, `/readyz`, `/metrics`;
- startup validation сообщает `NO_SECRET` как красное состояние; секрет загружается launcher/service manager, не из workflow;
- jobs переживают рестарт — хотя бы SQLite/Postgres spool, а не память процесса.

Контракт:

```json
POST /v1/chat/completions
Authorization: Bearer ...
X-Request-Id: ...
{
  "model": "codex-subscription",
  "messages": [{"role":"user","content":"..."}],
  "stream": false,
  "response_format": {"type":"json_object"}
}
```

Но если ваши CLI не реализуют tool-call protocol, shim не должен притворяться полноценным OpenAI agent backend. Он может быть OpenAI-compatible только на subset: non-streaming chat completion. Для agent tools используйте обычный n8n workflow-tool pattern: n8n вызывает sidecar для одного reasoning turn, а tools остаются явными workflow branches. Не прокидывайте CLI filesystem powers через cloud n8n.

n8n 2.x:

- OpenAI credential содержит Base URL и optional custom header;
- `lmChatOpenAi` также принимает Base URL;
- исходник предупреждает: non-OpenAI models могут не поддерживать tool calls и JSON response format;
- модель discovery вызывает `/models`;
- embedding node может использовать ту же credential/base URL, но ваш shim обязан реально реализовать embeddings API;
- обычный OpenAI app node и отдельные operations могут иметь provider-specific assumptions. Проверять node-by-node.
[OpenAI credential source](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/credentials/OpenAiApi.credentials.ts), [OpenAI Chat Model source](https://github.com/n8n-io/n8n/blob/master/packages/%40n8n/nodes-langchain/nodes/llms/LMChatOpenAi/LmChatOpenAi.node.ts).

HTTP Request settings:

- node timeout чуть больше gateway job SLA, но не 900/1800 секунд без верхней границы;
- предпочтительно `POST job → 202 {job_id}`, затем Wait/backoff polling или callback;
- не держать n8n HTTP socket 30 минут;
- retries только для connection failure/502/503/504 и только с idempotency key;
- 401/403, validation 4xx, `insufficient_quota`, policy refusal — не retry;
- 429 разделить на short rate limit (`Retry-After`) и exhausted quota/circuit-open;
- ограничить n8n concurrency, но основной limiter должен быть в gateway.

Начальные concurrency limits нельзя честно вывести из предоставленных данных. Безопасный пилот: 1 concurrent process/account/provider, затем поднимать до 2 только после измерения vendor behaviour. Не запускать race на три подписки: он утраивает расход. Sequential fallback:

```text
preferred provider
→ retryable transport failure?
→ circuit breaker check
→ next provider
→ final metered API only for allow-listed high-value jobs
```

Circuit breaker:

- open после последовательности доказанных provider failures;
- half-open одним probe;
- quota exhaustion имеет отдельный reset time;
- prompt/result сохраняются без секретов;
- fallback помечает фактического provider/model.

ToS: подписка на interactive CLI не автоматически разрешает превращать её в shared automation API. Это необходимо проверить отдельно для каждого vendor/account. Не маскировать автоматизацию и не отдавать gateway сотрудникам как общий model service без подтверждённых условий.

Когда платить per token лучше:

- SLA важнее цены;
- высокая конкурентность;
- embeddings/batch API;
- streaming/tool calls должны быть строго OpenAI-compatible;
- unattended automation запрещена subscription terms;
- запрос имеет прямую измеримую ценность выше стоимости API;
- локальная машина спит, а ответ нужен немедленно.

Метрики:

- metered OpenAI spend −90% на перенесённых lanes;
- sidecar queue wait p95 <60 секунд;
- stuck process count = 0;
- timeout rate <2%;
- fallback rate <10%;
- task success не хуже baseline.

Harvest: 14 дней и минимум 100 запросов.

## 3. Остальные блоки

### Queue mode

Что покупает:

- main принимает triggers/API, workers выполняют jobs;
- можно ограничить и масштабировать execution concurrency;
- Redis даёт queue/stalled-job coordination;
- долгий workflow меньше мешает другим execution.

Чего не покупает:

- не уменьшает 4155 runs вашего минутного Mongo poller;
- не заменяет event trigger;
- не исправляет single-threaded sidecar;
- не делает `$getWorkflowStaticData` транзакционным;
- не гарантирует правильность schedule registration;
- добавляет Redis, workers, encryption-key consistency, graceful shutdown и ещё одну failure domain.

При ~1k executions/day throughput сам по себе не обосновывает queue mode. Сначала уберите пустые polls. Queue mode оправдан, если measurement показывает saturation или требуется blast-radius isolation долгих audio/LLM executions.

Для queue mode все main/workers должны иметь одинаковый `N8N_ENCRYPTION_KEY`; Redis variables и worker lock/stalled intervals документированы официально. [Queue-mode variables](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/queue-mode.md).

Do NOT do this if: единственная проблема — один минутный poller или зависающий downstream server.

### Error workflows и DLQ

У вас уже зрелая central error workflow. Нативного replacement вашему anti-storm gate я не нашёл.

Error Trigger получает данные о failed execution/workflow и error; точная форма зависит от того, упало ли workflow при automatic execution или ошибка была raised вручную. Не кодируйте against assumed fields: сохраните реальный payload вашего 2.14.2 и version it.

DLQ поверх error workflow:

1. Нормализовать `execution_id`, `workflow_id`, `node`, `error_class`, `message`, `first_seen`.
2. Классифицировать retryable/non-retryable.
3. Записать immutable failure record.
4. Payload хранить ссылкой на saved execution, пока retention позволяет; критичные минимальные входы копировать отдельно с redaction.
5. Retryable row → `available_at`.
6. Non-retryable → dead/manual.
7. Повторная доставка обязана иметь исходный idempotency key.

UI умеет retry with original workflow или currently saved workflow. Официальный public API reference следует проверить на вашей 2.14.2: я не установил документированный public `/api/v1` endpoint для retry execution. Не используйте внутренний `/rest/` как стабильный production contract. [Execution retry docs](https://docs.n8n.io/workflows/executions/all-executions/), [public API reference](https://github.com/n8n-io/n8n-docs/blob/main/docs/connect/n8n-api/api-reference.md).

### Retention

Ваши ~10k executions упираются именно в default count cap. В актуальной 2.x документации:

- `EXECUTIONS_DATA_PRUNE=true`;
- `EXECUTIONS_DATA_MAX_AGE`, default 336 hours;
- `EXECUTIONS_DATA_PRUNE_MAX_COUNT`, default 10,000;
- pruning происходит при достижении любого условия;
- waiting/running/new не prune;
- annotated executions не prune.
[Execution env reference](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/executions.md).

Рекомендация:

- success full payload: короткое окно;
- error full payload: 60–90 дней, если DB позволяет;
- долгосрочная статистика: отдельная агрегированная таблица без raw prompts/secrets;
- поднять count cap после измерения среднего execution bytes;
- Postgres storage/disk/WAL/backup growth alarm;
- не ставить count=0, пока нет capacity model.

### Sub-workflows

Разделяйте три контракта:

- Execute Sub-workflow Trigger: обычный reusable workflow;
- Execute Sub-workflow node: parent→child orchestration;
- Call n8n Workflow Tool / `toolWorkflow`: AI tool contract.

Входы child должны быть явно типизированы и versioned. Tool-workflow — не общий subroutine: его output должен быть коротким и пригодным для модели.

У вас уже измерено, что часть sub-workflow/tool executions не появляется отдельной строкой так, как ожидает ваш аудит. Поэтому observability ставьте внутри child:

```text
on entry: usage event
on exit: outcome + duration
on error: explicit failure receipt
```

Не судить о живости tool workflow по `/executions?workflowId=child` в одиночку.

Do NOT do this if: child нужен только одному parent и содержит 2–3 простых nodes; extra workflow ухудшит трассировку.

### Evaluations

Использовать стоит не как «benchmark модели», а как regression canary:

- 20–50 замороженных CRM/RAG inputs;
- assertions: schema valid, required fields, no forbidden model alias, retrieval contains expected doc ID, tool chosen from allow-list;
- один evaluator не должен быть тем же LLM, который произвёл ответ, для критичных checks;
- больше deterministic assertions, меньше LLM-as-judge.

Metric-based evaluation имеет licensing gates; точный набор, доступный на вашей self-hosted license, надо проверить в UI/API. Не строить rollout на функции, доступность которой не подтверждена.

### AI Agent, memory и generic model endpoint

Ваш Inner Agent pattern уже правильнее монолита: небольшой supervisor + специализированные tool workflows.

Уменьшить flaky Postgres Chat Memory:

- memory failure не должна валить deterministic CRM operation;
- session key должен быть устойчивым и namespaced;
- memory reads/writes логировать отдельно;
- ограничивать history;
- critical facts хранить в CRM/Postgres canonical tables, не в chat memory;
- 11% error — достаточно, чтобы убрать её из critical path.

Structured Output Parser полезен для machine contract, но model-generated JSON всё равно валидировать schema node/Code node и иметь repair/fail branch.

Generic HTTP endpoint как model backend работает только если shim реализует OpenAI Chat surface. Bare `/ask {prompt}` нельзя напрямую подключить как LangChain Chat Model без adapter.

### MCP server и client

В 2.14.2 instance MCP уже может быть включён instance-wide и workflow-by-workflow. Но docs существенно расширились позже:

- современный server умеет search/execute workflows и создавать/редактировать workflows/Data Tables;
- Data Table MCP tools отмечены «available from 2.16.0»;
- granular OAuth scope selection появился в 2.32;
- `validate_node_config` — с 2.25.1.
[MCP setup](https://github.com/n8n-io/n8n-docs/blob/main/docs/connect/connect-to-n8n-mcp-server.md), [MCP tools versions](https://github.com/n8n-io/n8n-docs/blob/main/docs/connect/connect-to-n8n-mcp-server/mcp-server-tools-reference.md).

Для вас:

- 2.14.2: не считать current docs точным описанием;
- pilot только read-only token/OAuth;
- enable только allow-listed workflows;
- отдельные harmless wrapper workflows вместо прямого доступа к CRM mutation;
- ротируемый token лучше long-lived all-powerful API key, но OAuth client с write scopes всё равно опасен;
- после ≥2.32 выбирать custom scopes;
- write/build tools держать выключенными до staging adversarial tests.

MCP Client Tool полезен для того, чтобы n8n agent вызвал внешний MCP server. Вам он почти не нужен: мозг и tools локальны, а открывать локальный MCP cloud instance противоречит boundary.

### External Secrets

Business/Enterprise feature; providers и детали менялись, поэтому список надо сверять с вашей лицензией и release. Главное: external secrets не гарантируют, что n8n DB вообще не содержит credential records. Обычно credential/config хранит reference, а secret value резолвится у provider.

Для вашей архитектуры это плохой fit: secret provider должен быть доступен cloud n8n, то есть секреты уже не «local only».

Do NOT do this if: правило «все secrets только на hub» остаётся буквальным. Оставьте cloud n8n только его минимальные cloud-specific credentials.

### Source control

Built-in environments доступны Business/Enterprise. Git включает workflows, Data Tables по выбору, tags и stubs для variables/credentials. Он не переносит значения credential secrets и execution history. Push отправляет saved, не published workflow; publish на destination отдельный шаг.

Официальные docs прямо не рекомендуют push и pull в один instance из-за overwrite/data-loss. [Source-control tutorial](https://docs.n8n.io/source-control-environments/create-environments/).

REST edits и Git могут сосуществовать технически, но только если один direction/source of truth:

- допустимо: API generator меняет dev instance → dev pushes Git → prod only pulls;
- плохо: API меняет prod, а Git позже pull-ит поверх;
- плохо: UI, API и Git одинаково считаются authoritative.

Для вас пока лучше оставить canonical JSON repository + API deployer единственным writer. Built-in Git имеет смысл вместе со вторым staging instance и protected production.

### Monitoring watchman

Минимум:

- внешний `/healthz`;
- внешний `/healthz/readiness`: проверяет DB connected+migrated;
- `/metrics` с `N8N_METRICS=true`;
- Postgres availability/connections/storage/WAL/slow queries;
- Redis availability/queue depth/stalled jobs, если появится queue mode;
- process RSS/heap/restarts/event-loop lag;
- execution rates by workflow/status;
- trigger last-seen;
- intended vs actual active inventory;
- synthetic WAF/API/webhook probes с production UA/client.

`/healthz` не проверяет DB; readiness проверяет. `/metrics` выключен по умолчанию. [Official monitoring docs](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/keep-n8n-running/monitor-n8n.md).

Prometheus не знает:

- что GitHub webhook не подключён;
- что конкретный trigger должен был сработать;
- что WAF режет Python UA;
- что модель alias дорогая;
- что output семантически пуст;
- что workflow inactive вопреки intent.

Внешний watchdog должен жить вне VPS/DB/reverse proxy n8n. Он раз в 5–20 минут:

1. проверяет DNS/TLS/WAF path;
2. `/healthz` и `/healthz/readiness`;
3. REST API browser-like UA;
4. synthetic webhook тем же Python client;
5. ждёт receipt вне n8n — например, запись/alert в независимом rail;
6. проверяет trigger silence и active inventory;
7. ежедневно выдаёт green receipt с возрастом данных.

Красные alarms: immediate. Green receipt: раз в сутки, с counts и последним успешным canary.

## 4. Do NOT do this

- Не переносить vault, GPU, local SQLite, browser sessions, secret store или owner voice в n8n.
- Не переносить fleet watchdog внутрь n8n: circular dependency.
- Не считать queue mode лекарством от плохого poller или blocked sidecar.
- Не держать HTTP execution открытым 15–30 минут, если можно выдать job ID.
- Не делать fan-out race по Claude/Codex/Gemini: платите квотой трижды.
- Не retry-ить `insufficient_quota`, auth failures и validation errors.
- Не принимать HTTP 200 PUT/MCP result как доказательство сохранения active state.
- Не использовать CLI import против БД живого n8n process.
- Не определять zombie по `active` field одному.
- Не считать curl smoke test доказательством Python client path.
- Не использовать static data как lock, queue или fast cross-execution store.
- Не давать instance MCP write scopes к production CRM.
- Не ставить community MTProto node в основной n8n container без code review, pinning и отдельного service account.
- Не хранить Telegram MTProto session string в workflow JSON.
- Не переносить редкий webhook обратно в частый poller ради «надёжности»; оставьте редкий external backstop.
- Не сохранять все successful execution payloads на 90 дней без замера bytes/day.
- Не считать pinned-data/manual execution dry run: реального транзакционного dry-run production workflow в общем случае нет.
- Не делать workflow из 20–40 детерминированных transform nodes, если 40-строчный тестируемый Python/JS script яснее.
- Не превращать n8n в application database. Data Tables — control state; CRM/RAG canonical data остаётся в Mongo/Postgres.
- Не дробить каждый кусок на sub-workflow: у вас уже есть observability tax.
- Не использовать bare model aliases. Allow-list explicit model IDs и deny-list всех дорогих строк, включая versioned flagship IDs.
- Не позволять n8n auto-retry узлу, пока реальный error class не классифицирован.

Симптомы orchestrator creep:

- Code node >40–60 строк и имеет собственную domain model;
- workflow хранит canonical business state;
- больше половины nodes — преобразование данных, не интеграция;
- тест требует canvas screenshots;
- rollback невозможен без ручного кликанья;
- reasoning prompt решает то, что SQL/код решает детерминированно;
- workflow нуждается в GPU/filesystem/secrets;
- одна execution длится десятки минут из-за compute, а не ожидания внешнего события.

## 5. Programmatic deployment и тестирование

Canonicalization перед diff:

- удалить read-only fields;
- сортировать object keys;
- nodes сортировать по stable node ID;
- connections нормализовать по source/output/index;
- positions хранить, но вынести в отдельный cosmetic diff;
- pinned data не включать в production artifact;
- credential references сравнивать по logical name/type mapping, не считать IDs переносимыми;
- settings пересобирать по schema allow-list;
- node IDs не регенерировать без причины.

Между instances:

- workflow IDs могут отличаться;
- credential IDs практически требуют mapping;
- sub-workflow references/caller IDs нужно перепривязать;
- Data Table IDs также instance-specific;
- webhook URLs зависят от instance/base URL;
- OAuth callbacks и credentials нельзя переносить как plaintext.

Promotion:

```text
canonical source JSON
→ lint/schema/model policy
→ create/update staging
→ read-back hash
→ fixture/manual run
→ trigger canary
→ snapshot prod
→ update prod
→ read-back
→ publish/activate explicitly
→ production canary
→ observe
```

Pinned data годится для node-level development. Manual execution не проверяет registration of production webhook/schedule. Настоящего универсального dry-run нет: downstream writes нужно заменять shadow sinks или inject-able `dry_run` branches.

## 6. Community nodes

Для Telegram Bot API, MongoDB, GitHub, Postgres и PGVector у n8n уже есть official/built-in nodes. Базовый выбор — built-in либо HTTP Request.

| Стек | Рекомендация |
|---|---|
| Telegram Bot API | Built-in Telegram/Telegram Trigger |
| Telegram MTProto/user account | Только isolated community-node pilot |
| MongoDB | Built-in; HTTP Request неприменим к native Mongo protocol |
| GitHub | Built-in trigger/action или HTTP Request для нового API surface |
| Postgres | Built-in Postgres |
| PGVector/RAG | Built-in PGVector nodes для простого RAG; SQL/Postgres для точного контроля |
| Subscription CLIs | HTTP Request к вашему gateway, не community Codex node |

MTProto candidates, которые удалось установить:

- `@telepilotco/n8n-nodes-telepilot` 0.5.2: опубликован примерно год назад, собственная страница помечает beta и признаёт несовместимость с частью installations. Для production — нет.
- `@telecopilotco/n8n-nodes-telepilot` 1.9.4: также опубликован примерно год назад.
- `n8n-nodes-telegram-mtproto`: минимум n8n 1.82, небольшой repo — 7 stars/4 forks на момент индексации; session string генерируется вручную, trigger слушает все сообщения без нормального filter surface.
- `n8n-nodes-telegram-client`: активнее по истории, но всё равно arbitrary code + privileged MTProto session.
[MTProto node repository](https://github.com/veezex/n8n-nodes-telegram-mtproto), [TelePilot npm](https://www.npmjs.com/package/%40telepilotco/n8n-nodes-telepilot), [Telegram client node](https://github.com/ofekb/n8n-nodes-telegram-client).

Install verification:

1. Pin exact version, не `latest`.
2. Inspect source, install scripts, dependencies and network calls.
3. Build image offline/reproducibly.
4. Generate SBOM and vulnerability scan.
5. Separate test Telegram account.
6. No vault/host mounts.
7. Confirm node appears and workflow imports.
8. Restart container and test trigger re-registration.
9. Test revoke/session logout.
10. Upgrade only after diff review.

Community nodes execute arbitrary code inside n8n’s trust boundary; официальный security audit отдельно перечисляет community/custom nodes как risk surface. [n8n security audit](https://docs.n8n.io/hosting/securing/security-audit/).

## 7. Версионно-источниковое приложение

### Документировано и проверено

- Execution pruning defaults: 336 hours and 10,000 count; either condition prunes. [Official execution variables](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/executions.md).
- `/healthz` only reachability; `/healthz/readiness` includes DB/migrations; `/metrics` disabled by default. [Official monitoring](https://github.com/n8n-io/n8n-docs/blob/main/docs/deploy/host-n8n/keep-n8n-running/monitor-n8n.md).
- Telegram Trigger is a webhook and configures Telegram `setWebhook`; node type 1.3 sets `drop_pending_updates`. [Source code](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/nodes/Telegram/TelegramTrigger.node.ts).
- Telegram keeps undelivered updates ≤24h, retries non-2xx, supplies sequential `update_id` and `pending_update_count`. [Telegram Bot API](https://core.telegram.org/bots/api).
- OpenAI credentials/chat node support custom Base URL, but compatibility features may differ. [Credential source](https://github.com/n8n-io/n8n/blob/master/packages/nodes-base/credentials/OpenAiApi.credentials.ts), [chat-model source](https://github.com/n8n-io/n8n/blob/master/packages/%40n8n/nodes-langchain/nodes/llms/LMChatOpenAi/LmChatOpenAi.node.ts).
- Built-in source control is Business/Enterprise and warns against bidirectional push/pull on one instance. [Official source-control tutorial](https://docs.n8n.io/source-control-environments/create-environments/).
- Data Table full table CRUD shipped in 2.3; MCP Data Table tools begin at 2.16. [Release notes](https://github.com/n8n-io/n8n-docs/blob/main/docs/changelog/release-notes.md), [MCP tools](https://github.com/n8n-io/n8n-docs/blob/main/docs/connect/connect-to-n8n-mcp-server/mcp-server-tools-reference.md).
- Data Table SQL injection fixed in 2.14.1; your 2.14.2 is after the fix. [Security advisory](https://github.com/n8n-io/n8n/security/advisories/GHSA-98c2-4cr3-4jc3).
- Granular instance MCP OAuth scopes shipped in 2.32. [Release notes](https://github.com/n8n-io/n8n-docs/blob/main/docs/changelog/release-notes.md).

### Community reports с сильной репродукцией

- Separate-process CLI import leaves stale in-memory trigger registration; restart clears it.
- Schedule protections reported in 2.19.1 and 2.27.0.
- Polling-trigger duplication still reproduced in 2.32.6.
[Issue #35251](https://github.com/n8n-io/n8n/issues/35251).

- Deactivated workflows continuing to execute have multiple reports, но не все закрыты как confirmed core bug. [Issue #21770](https://github.com/n8n-io/n8n/issues/21770), [Issue #30607](https://github.com/n8n-io/n8n/issues/30607).

### Мои выводы

- Queue mode не окупается вашим volume без доказанной saturation.
- Data Tables подходят для heartbeat/dedup; Postgres лучше для конкурентного lease queue.
- REST PUT безопаснее community MCP mutation только благодаря вашему guard, а не сам по себе.
- Wake call должен быть hint, durable row — источником истины.
- n8n queue mode не исправляет sidecar starvation.
- Instance MCP write access сейчас имеет отрицательное risk/reward для production CRM.

## 8. Что не удалось установить

1. Точную первопричину вашего Python `_input` расхождения. Не найден подтверждённый issue/PR, объясняющий, почему byte-identical code работает в одном `PythonTaskRunnerSandbox`, но не в другом.
   Проверить:
   - полные workflow JSON;
   - Code node `typeVersion`;
   - language/mode;
   - task runner mode/version;
   - execution mode;
   - input item count;
   - imported legacy parameters;
   - worker image/version consistency.
   
   До выяснения правило «production Code nodes — JavaScript; Python только isolated test» разумно.

2. Документированный public API endpoint для retry/replay failed execution в 2.14.2. UI-функция есть; внутренний `/rest` endpoint не считать стабильным API.

3. Был ли именно ваш MCP `removeNode` причиной `active=false`. Нужны before/after MCP request logs или wrapper source. Официальная семантика `removeNode` сама по себе деактивацию не обещает.

4. Атомарность Data Table upsert/conditional update в вашей 2.14.2, достаточная для конкурентного lease. Это надо нагрузочно проверить двумя параллельными claimers.

5. Exact plan/provider list External Secrets для вашей лицензии и сборки. Это не меняет verdict «не подходит при local-only secrets».

6. Точный current stable patch на 20.08.2026: документы уже отражают 2.33.x, но перед upgrade надо брать release registry непосредственно в день установки и не использовать floating `latest`.

7. Поведение Wait/resume при rolling upgrade именно 2.14.2→current. Нужен staging test с waiting execution до restart/upgrade и callback после него.

🧒 Простыми словами: n8n у вас должен быть диспетчером у телефона. Он услышал событие, записал его в журнал и позвал нужный компьютер. Думать, писать голосом Антона и хранить ключи он не должен. Самые опасные поломки сейчас — когда табличка говорит «выключено», а мотор всё ещё крутится, или наоборот. Поэтому первый фикс — внешний счётчик, который смотрит не на табличку, а на реальные поездки.
